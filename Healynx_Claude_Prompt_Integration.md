# Claude — Wire Healynx Integration Layer

You are wiring together the Healynx patient mobile and doctor desktop apps. Build the API client, auth system, state stores, TanStack Query hooks, WebSocket layer, modals, and enforce AI content rules across both apps.

**Tech stack (fixed):**
- Next.js 14 + React 18 + TypeScript
- TanStack Query 5.x, Zustand 4.x
- React Hook Form + Zod
- `fetch` only. No axios.

---

## 1. API Client — `lib/api-client.ts`

This is the single HTTP client used by every hook and store. It handles JWT attachment, 401 auto-refresh, and error normalisation. No component or hook calls `fetch` directly.

```ts
const BASE_URL = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:8000/api/v1';
let accessToken: string | null = null;

export function setAccessToken(token: string | null) { accessToken = token; }
export function getAccessToken() { return accessToken; }

async function refreshAccessToken(): Promise<string> {
  const res = await fetch(`${BASE_URL}/auth/refresh`, { method: 'POST', credentials: 'include' });
  if (!res.ok) throw new Error('Refresh failed');
  const data = await res.json();
  accessToken = data.access_token;
  return data.access_token;
}

export class ApiError extends Error {
  constructor(public status: number, message: string, public details?: unknown) {
    super(message); this.name = 'ApiError';
  }
}

export async function apiClient<T>(path: string, options: RequestInit = {}): Promise<T> {
  const headers: Record<string, string> = {
    'Content-Type': 'application/json',
    ...(options.headers as Record<string, string>),
  };
  if (accessToken) headers['Authorization'] = `Bearer ${accessToken}`;

  let res = await fetch(`${BASE_URL}${path}`, { ...options, headers, credentials: 'include' });

  if (res.status === 401 && accessToken) {
    try {
      const newToken = await refreshAccessToken();
      headers['Authorization'] = `Bearer ${newToken}`;
      res = await fetch(`${BASE_URL}${path}`, { ...options, headers, credentials: 'include' });
    } catch {
      if (typeof window !== 'undefined') window.location.href = '/login';
      throw new ApiError(401, 'Session expired');
    }
  }
  if (!res.ok) {
    const error = await res.json().catch(() => ({ message: res.statusText }));
    throw new ApiError(res.status, error.message || 'Request failed', error);
  }
  return res.json();
}
```

---

## 2. Auth Store — `stores/auth-store.ts`

```ts
import { create } from 'zustand';
import { apiClient, setAccessToken } from '@/lib/api-client';

interface User {
  id: string; email: string; role: 'patient' | 'doctor' | 'clinic_admin';
  tenant_id: string; full_name: string; mfa_enabled: boolean;
}

interface AuthState {
  user: User | null;
  isAuthenticated: boolean;
  isLoading: boolean; // true during initial session check

  login: (email: string, password: string) => Promise<{ mfaRequired?: boolean; mfaToken?: string }>;
  verifyMfa: (mfaToken: string, totpCode: string) => Promise<void>;
  logout: () => Promise<void>;
  refreshSession: () => Promise<void>;
}

export const useAuthStore = create<AuthState>((set, get) => ({
  user: null, isAuthenticated: false, isLoading: true,

  refreshSession: async () => {
    try {
      const res = await fetch(`${process.env.NEXT_PUBLIC_API_URL}/auth/refresh`, { method: 'POST', credentials: 'include' });
      if (!res.ok) throw new Error('No session');
      const data = await res.json();
      setAccessToken(data.access_token);
      set({ user: data.user, isAuthenticated: true, isLoading: false });
    } catch {
      set({ user: null, isAuthenticated: false, isLoading: false });
    }
  },

  login: async (email, password) => {
    const data = await apiClient<{ access_token: string; user: User; mfa_required?: boolean; mfa_token?: string }>('/auth/login', {
      method: 'POST', body: JSON.stringify({ email, password }),
    });
    if (data.mfa_required) return { mfaRequired: true, mfaToken: data.mfa_token };
    setAccessToken(data.access_token);
    set({ user: data.user, isAuthenticated: true });
    return {};
  },

  verifyMfa: async (mfaToken, totpCode) => {
    const data = await apiClient<{ access_token: string; user: User }>('/auth/mfa/verify', {
      method: 'POST', body: JSON.stringify({ mfa_token: mfaToken, totp_code: totpCode }),
    });
    setAccessToken(data.access_token);
    set({ user: data.user, isAuthenticated: true });
  },

  logout: async () => {
    await apiClient('/auth/logout', { method: 'POST' });
    setAccessToken(null);
    set({ user: null, isAuthenticated: false });
    if (typeof window !== 'undefined') window.location.href = '/login';
  },
}));
```

**Usage:** On app mount, call `useAuthStore().refreshSession()`. After login, redirect based on `user.role`: patient→`/(patient)/dashboard`, doctor→`/(doctor)/dashboard`, admin→`/(admin)/dashboard`.

---

## 3. Survey Draft Store — `stores/survey-draft-store.ts`

Auto-saves survey progress to localStorage.

```ts
import { create } from 'zustand';

interface Supplement { name: string; dosage: number; unit: string; frequency: string; duration_months: number; health_purpose?: string }
interface MedicationEntry { name: string; dosage: string; frequency: string }
interface LifestyleEntry { diet?: string; exercise_frequency?: string; sleep_hours?: string; alcohol?: string; smoking?: string; caffeine?: string; water_intake?: string; stress_level?: string }

interface SurveyDraft {
  supplements: Supplement[];
  health_variables: { conditions: string[]; medications: MedicationEntry[]; allergies: string[] };
  lifestyle: LifestyleEntry;
  current_step: number; // 1-4
  last_saved_at: string | null;
}

interface SurveyDraftState {
  draft: SurveyDraft | null; isDirty: boolean;
  setDraft: (partial: Partial<SurveyDraft>) => void;
  saveDraft: () => void;
  loadDraft: () => SurveyDraft | null;
  clearDraft: () => void;
}

export const useSurveyDraftStore = create<SurveyDraftState>((set, get) => ({
  draft: null, isDirty: false,
  setDraft: (partial) => set((s) => ({ draft: { ...s.draft, ...partial } as SurveyDraft, isDirty: true })),
  saveDraft: () => {
    const { draft } = get();
    if (!draft) return;
    draft.last_saved_at = new Date().toISOString();
    localStorage.setItem('healynx-survey-draft', JSON.stringify(draft));
    set({ isDirty: false });
  },
  loadDraft: () => {
    const raw = localStorage.getItem('healynx-survey-draft');
    if (!raw) return null;
    return JSON.parse(raw) as SurveyDraft;
  },
  clearDraft: () => { localStorage.removeItem('healynx-survey-draft'); set({ draft: null, isDirty: false }); },
}));
```

**Auto-save:** In survey step components, call `saveDraft()` on every field `onBlur`. Debounce 500ms. On survey mount, call `loadDraft()` — if exists, prompt "Continue where you left off? [Resume] [Start Fresh]".

---

## 4. Emergency Store — `stores/emergency-store.ts`

```ts
import { create } from 'zustand';
import { apiClient, setAccessToken } from '@/lib/api-client';

interface EmergencyState {
  isActive: boolean; sessionId: string | null; patientId: string | null;
  patientName: string | null; reason: string | null; remainingSeconds: number;
  activate: (patientId: string, patientName: string, reason: string, justification?: string) => Promise<void>;
  deactivate: () => Promise<void>;
  tick: () => void;
  _interval: ReturnType<typeof setInterval> | null;
}

export const useEmergencyStore = create<EmergencyState>((set, get) => ({
  isActive: false, sessionId: null, patientId: null, patientName: null, reason: null, remainingSeconds: 0, _interval: null,

  activate: async (patientId, patientName, reason, justification?) => {
    const data = await apiClient<{ emergency_session_id: string; access_token: string; expires_at: string; patient_id: string }>('/emergency-access', {
      method: 'POST', body: JSON.stringify({ patient_id: patientId, reason, justification }),
    });
    setAccessToken(data.access_token); // swap to emergency JWT
    const interval = setInterval(() => get().tick(), 1000);
    set({ isActive: true, sessionId: data.emergency_session_id, patientId, patientName, reason, remainingSeconds: 1800, _interval: interval });
  },

  deactivate: async () => {
    const { sessionId, _interval } = get();
    if (sessionId) await apiClient(`/emergency-access/${sessionId}`, { method: 'DELETE' });
    if (_interval) clearInterval(_interval);
    setAccessToken(null);
    set({ isActive: false, sessionId: null, patientId: null, patientName: null, reason: null, remainingSeconds: 0, _interval: null });
    // re-authenticate with original JWT
    await useAuthStore.getState().refreshSession();
  },

  tick: () => {
    const { remainingSeconds } = get();
    if (remainingSeconds <= 1) { get().deactivate(); return; }
    set({ remainingSeconds: remainingSeconds - 1 });
  },
}));
```

---

## 5. Zod Schemas — `lib/validators.ts`

```ts
import { z } from 'zod';

export const supplementSchema = z.object({
  name: z.string().min(1, 'Supplement name is required'),
  dosage: z.number().positive('Dosage must be positive'),
  unit: z.string().min(1, 'Unit is required'),
  frequency: z.enum(['daily', 'twice_daily', 'weekly', 'monthly', 'as_needed']),
  duration_months: z.number().positive().max(600),
  health_purpose: z.string().optional(),
});

export const medicationSchema = z.object({
  name: z.string().min(1), dosage: z.string().optional(), frequency: z.string().optional(),
});

export const step1Schema = z.object({
  supplements: z.array(supplementSchema).min(1, 'Add at least one supplement'),
});

export const step2Schema = z.object({
  conditions: z.array(z.string()).optional(),
  medications: z.array(medicationSchema).optional(),
  allergies: z.array(z.string()).optional(),
});

export const step3Schema = z.object({
  diet: z.string().optional(),
  exercise_frequency: z.string().optional(),
  sleep_hours: z.string().optional(),
  alcohol: z.string().optional(),
  smoking: z.string().optional(),
  caffeine: z.string().optional(),
  water_intake: z.string().optional(),
  stress_level: z.string().optional(),
});

export const step4Schema = z.object({
  confirmed: z.literal(true, { errorMap: () => ({ message: 'Please confirm the information is accurate' }) }),
});

export const surveySubmissionSchema = step1Schema.merge(step2Schema).extend({
  lifestyle: step3Schema.optional(),
});
```

---

## 6. TanStack Query Hooks

### Patient Hooks — `hooks/use-dashboard.ts`, `hooks/use-survey.ts`, `hooks/use-records.ts`

```ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { apiClient } from '@/lib/api-client';
import type { PatientDashboard, Assessment, AssessmentSubmitResponse, TaskStatus, MedicalRecord, ShareResponse } from '@/types';

// Dashboard
export function usePatientDashboard() {
  return useQuery({
    queryKey: ['dashboard', 'patient'],
    queryFn: () => apiClient<PatientDashboard>('/patients/me/dashboard'),
    staleTime: 30_000, retry: 2,
  });
}

// Submit survey
export function useSubmitSurvey() {
  return useMutation({
    mutationFn: (data: any) => apiClient<AssessmentSubmitResponse>('/assessments', { method: 'POST', body: JSON.stringify(data) }),
  });
}

// Poll task
export function useTaskPolling(taskId: string | null, options: { enabled: boolean }) {
  return useQuery({
    queryKey: ['task', taskId],
    queryFn: () => apiClient<TaskStatus>(`/tasks/${taskId}`),
    enabled: options.enabled && !!taskId,
    refetchInterval: (query) => {
      const data = query.state.data;
      if (!data) return 500;
      if (data.status === 'completed' || data.status === 'failed') return false;
      return 500;
    },
  });
}

// Fetch completed assessment
export function useAssessment(assessmentId: string | null) {
  return useQuery({
    queryKey: ['assessment', assessmentId],
    queryFn: () => apiClient<Assessment>(`/assessments/${assessmentId}`),
    enabled: !!assessmentId,
    staleTime: 300_000,
  });
}

// Records
export function useRecords(params?: { type?: string; search?: string; page?: number }) {
  const searchParams = new URLSearchParams();
  if (params?.type) searchParams.set('type', params.type);
  if (params?.search) searchParams.set('search', params.search);
  if (params?.page) searchParams.set('page', String(params.page));
  return useQuery({
    queryKey: ['records', params],
    queryFn: () => apiClient<{ items: MedicalRecord[]; total: number }>(`/patients/me/records?${searchParams}`),
    staleTime: 30_000,
  });
}

// Share record
export function useShareRecord() {
  return useMutation({
    mutationFn: ({ recordId, data }: { recordId: string; data: any }) =>
      apiClient<ShareResponse>(`/records/${recordId}/share`, { method: 'POST', body: JSON.stringify(data) }),
  });
}
```

### Doctor Hooks — `hooks/use-doctor-dashboard.ts`, `hooks/use-patient-detail.ts`

```ts
export function useDoctorDashboard(searchParams?: Record<string, string>) {
  const sp = new URLSearchParams(searchParams ?? {});
  return useQuery({
    queryKey: ['dashboard', 'doctor', searchParams],
    queryFn: () => apiClient('/doctors/me/dashboard?' + sp),
    staleTime: 30_000,
  });
}

export function usePatientDetail(patientId: string) {
  return useQuery({
    queryKey: ['patient', patientId],
    queryFn: () => apiClient(`/doctors/patients/${patientId}`),
    staleTime: 30_000,
  });
}
```

---

## 7. WebSocket Hook — `hooks/use-websocket.ts`

Real-time updates with automatic fallback to polling.

```ts
import { useEffect, useRef, useCallback } from 'react';

type EventType = 'assessment_completed' | 'new_alert' | 'emergency_activated' | 'emergency_ended';

export function useWebSocket(token: string | null) {
  const wsRef = useRef<WebSocket | null>(null);
  const listenersRef = useRef<Map<EventType, Set<(data: any) => void>>>(new Map());
  const reconnectRef = useRef({ attempts: 0, timer: null as ReturnType<typeof setTimeout> | null });
  const pollRef = useRef<ReturnType<typeof setInterval> | null>(null);

  const connect = useCallback(() => {
    if (!token) return;
    const WS_URL = process.env.NEXT_PUBLIC_WS_URL || 'ws://localhost:8000/ws';
    try {
      const ws = new WebSocket(`${WS_URL}?token=${token}`);
      ws.onmessage = (e) => {
        const msg = JSON.parse(e.data);
        listenersRef.current.get(msg.type as EventType)?.forEach(fn => fn(msg.payload));
      };
      ws.onclose = () => {
        if (reconnectRef.current.attempts < 10) {
          const delay = Math.min(1000 * Math.pow(2, reconnectRef.current.attempts), 30000);
          reconnectRef.current.timer = setTimeout(() => { reconnectRef.current.attempts++; connect(); }, delay);
        } else {
          startPolling();
        }
      };
      ws.onerror = () => { ws.close(); startPolling(); };
      wsRef.current = ws;
      reconnectRef.current.attempts = 0;
    } catch {
      startPolling();
    }
  }, [token]);

  const startPolling = () => {
    if (pollRef.current) return;
    pollRef.current = setInterval(async () => {
      try {
        const updates: any[] = await (await fetch(`${process.env.NEXT_PUBLIC_API_URL}/updates/poll`)).json();
        updates.forEach(msg => listenersRef.current.get(msg.type as EventType)?.forEach(fn => fn(msg.payload)));
      } catch {}
    }, 10000);
  };

  const subscribe = (type: EventType, fn: (data: any) => void) => {
    if (!listenersRef.current.has(type)) listenersRef.current.set(type, new Set());
    listenersRef.current.get(type)!.add(fn);
    return () => { listenersRef.current.get(type)?.delete(fn); };
  };

  useEffect(() => { connect(); return () => { wsRef.current?.close(); if (pollRef.current) clearInterval(pollRef.current); }; }, [connect]);

  return { subscribe };
}
```

---

## 8. Modals — Build These

### Emergency Access Modal

Trigger: Doctor taps [Emergency Access]. Non-dismissible modal with: patient name, reason dropdown (Unconscious, Life-Threatening, Severe Allergy, Psychiatric, Other) + free-text field. [Cancel] + [Confirm Emergency] (danger button, disabled until reason entered).

```tsx
'use client';
import { Modal } from '@/components/ui/modal';
import { Button } from '@/components/ui/button';
import { useEmergencyStore } from '@/stores/emergency-store';
import { useState } from 'react';

export function EmergencyAccessModal({ patientId, patientName, isOpen, onClose }: { patientId: string; patientName: string; isOpen: boolean; onClose: () => void }) {
  const [reason, setReason] = useState('');
  const [loading, setLoading] = useState(false);
  const emergency = useEmergencyStore();

  const quickReasons = ['Unconscious Patient', 'Life-Threatening Emergency', 'Severe Allergic Reaction', 'Acute Psychiatric Crisis'];

  const handleConfirm = async () => {
    setLoading(true);
    await emergency.activate(patientId, patientName, reason);
    setLoading(false);
    onClose();
  };

  return (
    <Modal open={isOpen} onClose={onClose} dismissible={false} className="border-2 border-nv-danger-600">
      <div className="bg-nv-danger-050 p-4 rounded-t-lg">
        <h3 className="flex items-center gap-2 text-nv-danger-600">🚨 EMERGENCY ACCESS</h3>
      </div>
      <div className="p-6 flex flex-col gap-4">
        <p className="text-sm text-nv-neutral-600">You are requesting unrestricted access to {patientName}'s full medical records. All actions will be logged and audited.</p>
        <div>
          <label className="text-sm font-medium">Reason for emergency access*</label>
          <textarea className="w-full h-24 mt-1 border border-nv-neutral-300 rounded-md p-3 text-sm" value={reason} onChange={e => setReason(e.target.value)} placeholder="Describe the emergency..." />
        </div>
        <div className="flex flex-wrap gap-2">
          {quickReasons.map(r => <button key={r} type="button" onClick={() => setReason(r)} className="px-3 py-1.5 text-xs border border-nv-neutral-300 rounded-full hover:bg-nv-neutral-100">{r}</button>)}
        </div>
        <div className="flex gap-3 justify-end mt-2">
          <Button variant="ghost" onClick={onClose}>Cancel</Button>
          <Button variant="danger" onClick={handleConfirm} loading={loading} disabled={!reason.trim()}>Confirm Emergency</Button>
        </div>
      </div>
    </Modal>
  );
}
```

### Share Configuration Modal

Trigger: Patient taps [Share] on any record. Recipient search (connected doctors or email input). Permission toggles (View on/locked, Download off, Forward off). Expiry segmented control (24h, 7d, 30d, Custom). 6-digit PIN (optional). Note textarea. [Generate Secure Link] primary button.

Confirmation state replaces modal content: ✓ icon, recipient/expiry/permissions summary, copyable link with [📋], Share via WhatsApp/Email buttons, [Done].

### Confirmation Dialog

Reusable: title, description, [Cancel] (ghost) + [Confirm] (danger for destructive actions). Used for: delete record, revoke share, end emergency access, logout.

### Add Supplement Bottom Sheet

Trigger: [+ Add Another Supplement] on survey step 1. Form: supplement name (searchable), dosage + unit side-by-side, frequency dropdown, duration dropdown, health purpose (optional). [Add Supplement] primary button.

---

## 9. AI Content Rules — Enforce Everywhere

Every AI-generated text block in the app MUST have ALL of these:

1. **Sparkle icon** — `<Sparkles className="w-4 h-4 text-nv-secondary-600" />` from Lucide
2. **"AI-GENERATED" overline** — `<span className="text-[11px] font-semibold uppercase tracking-wider text-nv-secondary-600">AI-GENERATED</span>`
3. **Confidence bar** — 4px height, `neutral-200` track, fill: `success-600` if ≥0.8, `accent-600` if 0.6-0.79, `danger-600` if <0.6. Below 60%: show warning "Low confidence — verify before clinical use"
4. **Model version** — `<span className="text-[12px] text-nv-neutral-500 opacity-60 font-mono">Model: {version}</span>`
5. **[Flag Inaccuracy] link** — tertiary button, 14px

Create a `<AISummaryCard>` component that enforces ALL of these. Use it for: patient dashboard AI insight, doctor patient AI clinical summary, chatbot AI responses, record AI summaries. Never render AI text without this component.

---

## 10. Route Protection Middleware

Create `middleware.ts` at root:

```ts
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

const publicPaths = ['/login', '/register', '/share', '/api'];

export function middleware(request: NextRequest) {
  const { pathname } = request.nextUrl;
  if (publicPaths.some(p => pathname.startsWith(p))) return NextResponse.next();

  const token = request.cookies.get('nv_refresh')?.value;
  if (!token) return NextResponse.redirect(new URL('/login', request.url));

  return NextResponse.next();
}

export const config = {
  matcher: ['/((?!_next/static|_next/image|favicon.ico).*)'],
};
```

---

## 11. Build Order

1. `lib/api-client.ts` + `lib/validators.ts` (Zod schemas)
2. `stores/auth-store.ts` → `stores/survey-draft-store.ts` → `stores/emergency-store.ts`
3. Type definitions file: `types/index.ts` (collect all interfaces used by hooks)
4. `hooks/` — all TanStack Query hooks and `use-websocket.ts`
5. `components/ui/modal.tsx` (if not yet built) + `EmergencyAccessModal`
6. `middleware.ts`
7. Wire hooks into patient screens (replace mock data direct usage with hook calls)
8. Wire hooks into doctor screens
9. Apply AI content rules audit: find every AI text render → ensure it uses AISummaryCard
10. Test: login flow, survey submit→polling→results, emergency access activate→countdown→deactivate, share configuration
