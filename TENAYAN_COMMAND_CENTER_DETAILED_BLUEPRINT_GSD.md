# TENAYAN COMMAND CENTER — DETAILED BLUEPRINT FOR GSD/CODEX

**Target:** membangun web application PLTU Tenayan yang tampilannya sangat mendekati mockup command center yang sudah dibuat: dark, premium, high-tech, tegas, professional, dan terasa seperti control room / war room operasi pembangkit.

**Nama produk kerja:** `Tenayan Command Center`  
**Nama modul utama:** `Executive Morning Dashboard` / `Daily Ops War Room`  
**Lokasi unit:** PLTU Tenayan, Pekanbaru, Riau  
**Tujuan utama:** seluruh bahan meeting pagi PLTU Tenayan terdigitalisasi, terpusat, dapat diisi oleh user berbeda per bidang, tervalidasi, ditampilkan dalam dashboard real-time/daily, menjadi action tracker, dan dapat digenerate menjadi laporan meeting pagi.

---

## 0. Prinsip Besar

Aplikasi ini **bukan sekadar tempat upload PDF/slide**. Aplikasi ini harus menjadi **sistem operasi harian PLTU**.

Alur utama:

```text
User login per bidang
→ input/update data harian
→ validasi dan submit
→ supervisor/manager review
→ dashboard utama otomatis update
→ issue/action plan terbentuk
→ report meeting pagi otomatis digenerate
→ histori tersimpan
→ phase 2 AI membaca data historis untuk insight dan rekomendasi
```

Target UI:

```text
Sangat mirip mockup:
dark navy + charcoal
cyan/teal neon accent
green/amber/red status
glassmorphism card
compact enterprise dashboard
fixed sidebar
top command header
dense KPI cards
line chart, donut, gauge, table, photo grid
professional, bukan playful
```

---

## 1. Pendekatan Pembangunan dengan GSD

Gunakan GSD sebagai sistem kerja spec-driven. Jangan langsung membangun fullstack sekaligus. Pecah menjadi milestone dan phase kecil agar Codex tidak kehilangan konteks dan UI tidak melenceng.

### 1.1 Instalasi GSD untuk Codex

Jalankan di root project:

```bash
npx get-shit-done-cc@latest
```

Pilih runtime Codex dan local install untuk project ini. Jika ingin non-interactive, gunakan opsi installer GSD sesuai versi yang terpasang. Setelah install, pastikan command GSD tersedia di Codex.

### 1.2 Workflow GSD yang dipakai

Gunakan loop:

```text
new project
→ discuss phase
→ plan phase
→ execute phase
→ verify work
→ ship phase
→ complete milestone
```

Untuk Codex/GSD, jalankan command sesuai bentuk yang didukung runtime Anda. Jika GSD menampilkan command dengan prefix berbeda, ikuti command yang muncul dari `$gsd-help` atau `/gsd-help`.

### 1.3 Strategi Milestone

**Jangan membangun backend dulu.**  
Prioritas utama adalah menghasilkan frontend statis yang **semirip mungkin dengan mockup**.

#### Milestone 1 — UI Fidelity Lock
Tujuan: membuat frontend statis dengan dummy data yang secara visual sangat mirip mockup.

Phase:
1. Project setup + design tokens.
2. App shell: sidebar, topbar, status strip, layout 16:9.
3. Reusable component library.
4. Executive Morning Dashboard static.
5. Semua module page static.
6. Visual QA dan screenshot comparison.

#### Milestone 2 — Auth, Role, Input Workflow
Tujuan: user berbeda dapat login, mengisi laporan, submit, review, approve.

Phase:
1. Auth dan RBAC.
2. Form input per modul.
3. Submission session harian.
4. Approval workflow.
5. Audit log.

#### Milestone 3 — Backend Data & Storage
Tujuan: data disimpan, dashboard membaca dari database, upload gambar aktif.

Phase:
1. Database schema.
2. API routes / service layer.
3. File upload storage.
4. Data validation.
5. Seeding dari dummy data.

#### Milestone 4 — Report Generator
Tujuan: dashboard dapat diexport menjadi PDF/PPT/Excel dan archive report.

Phase:
1. Report template HTML.
2. Export PDF.
3. Export PPT.
4. Archive dan approval.
5. Distribution list.

#### Milestone 5 — AI Phase 2
Tujuan: daily brief, anomaly detection, ask data, action recommendation.

Phase:
1. AI briefing generator.
2. RAG over daily reports.
3. Anomaly detection.
4. Root cause suggestion.
5. Chat assistant.

---

## 2. Recommended Tech Stack

### 2.1 Frontend

| Area | Pilihan |
|---|---|
| Framework | Next.js 15+ / React |
| Language | TypeScript |
| Styling | Tailwind CSS |
| UI primitives | shadcn/ui |
| Icons | lucide-react |
| Chart | Recharts |
| Table | TanStack Table |
| Animation | Framer Motion |
| State | Zustand / TanStack Query |
| Forms | React Hook Form + Zod |
| Date | date-fns |
| Export PPT | pptxgenjs |
| Export PDF | Playwright screenshot PDF / react-pdf / server-side print |
| File upload UI | UploadThing / custom multipart |

### 2.2 Backend

Pilih salah satu:

**Option A — Supabase cepat dan realistis**
- PostgreSQL
- Supabase Auth
- Supabase Storage
- Row Level Security
- Edge Functions bila perlu

**Option B — Full custom**
- Next.js API routes / NestJS / Express
- PostgreSQL
- Prisma ORM
- S3-compatible storage
- JWT auth

Rekomendasi MVP: **Next.js + Supabase + PostgreSQL + Storage**.

---

## 3. Struktur Folder Target

```text
tenayan-command-center/
├─ app/
│  ├─ layout.tsx
│  ├─ page.tsx                         # redirect ke /dashboard
│  ├─ dashboard/page.tsx
│  ├─ unit-readiness/page.tsx
│  ├─ operation-performance/page.tsx
│  ├─ efficiency/page.tsx
│  ├─ energy-primary/page.tsx
│  ├─ chemical-water-quality/page.tsx
│  ├─ action-plan/page.tsx
│  ├─ reports/page.tsx
│  ├─ input/
│  │  ├─ readiness/page.tsx
│  │  ├─ performance/page.tsx
│  │  ├─ efficiency/page.tsx
│  │  ├─ energy-primary/page.tsx
│  │  ├─ chemical-water/page.tsx
│  │  └─ action-plan/page.tsx
│  ├─ admin/
│  │  ├─ users/page.tsx
│  │  ├─ master-data/page.tsx
│  │  └─ thresholds/page.tsx
│  └─ api/
├─ components/
│  ├─ shell/
│  │  ├─ app-shell.tsx
│  │  ├─ sidebar-nav.tsx
│  │  ├─ top-header.tsx
│  │  └─ status-strip.tsx
│  ├─ dashboard/
│  │  ├─ kpi-card.tsx
│  │  ├─ dashboard-card.tsx
│  │  ├─ status-badge.tsx
│  │  ├─ gauge-card.tsx
│  │  ├─ mini-metric.tsx
│  │  ├─ data-table.tsx
│  │  ├─ plant-overview.tsx
│  │  ├─ photo-grid.tsx
│  │  └─ action-progress.tsx
│  ├─ charts/
│  │  ├─ line-trend-chart.tsx
│  │  ├─ donut-chart.tsx
│  │  ├─ gauge-chart.tsx
│  │  ├─ waterfall-chart.tsx
│  │  └─ sparkline.tsx
│  ├─ forms/
│  └─ ui/
├─ lib/
│  ├─ design-tokens.ts
│  ├─ mock-data/
│  ├─ rbac.ts
│  ├─ status-rules.ts
│  ├─ thresholds.ts
│  ├─ formatters.ts
│  └─ supabase/
├─ public/
│  ├─ assets/
│  │  ├─ mockups/
│  │  ├─ plant/
│  │  ├─ textures/
│  │  └─ report-covers/
├─ docs/
│  ├─ mockups/
│  ├─ UI_BLUEPRINT.md
│  ├─ BACKEND_BLUEPRINT.md
│  ├─ DATABASE_SCHEMA.md
│  ├─ GSD_PHASES.md
│  └─ ACCEPTANCE_CRITERIA.md
├─ .planning/
└─ package.json
```

---

## 4. UI Design System agar Mirip Mockup

### 4.1 Visual Direction

Gunakan style:

```text
Industrial command center
Dark premium
Enterprise dashboard
High information density
Subtle neon glow
Crisp thin borders
No childish gradient
No colorful SaaS pastel
No generic admin template
```

### 4.2 Color Tokens

Implement di `globals.css` dan `tailwind.config.ts`.

```css
:root {
  --bg-root: #020617;
  --bg-shell: #03111f;
  --bg-panel: rgba(8, 22, 39, 0.78);
  --bg-panel-solid: #071827;
  --bg-panel-hover: rgba(12, 32, 56, 0.92);

  --border-soft: rgba(95, 166, 200, 0.20);
  --border-strong: rgba(34, 211, 238, 0.45);

  --text-primary: #e5f3ff;
  --text-secondary: #9fb7c8;
  --text-muted: #64798b;

  --accent-cyan: #22d3ee;
  --accent-teal: #14b8a6;
  --accent-blue: #38bdf8;

  --status-good: #22c55e;
  --status-warning: #f59e0b;
  --status-danger: #ef4444;
  --status-info: #38bdf8;
  --status-neutral: #94a3b8;

  --glow-cyan: 0 0 28px rgba(34, 211, 238, 0.18);
  --glow-green: 0 0 22px rgba(34, 197, 94, 0.16);
  --shadow-card: 0 18px 40px rgba(0, 0, 0, 0.35);

  --radius-card: 16px;
  --radius-panel: 18px;
}
```

### 4.3 Background

Root background harus seperti command center:

```css
body {
  background:
    radial-gradient(circle at 20% 0%, rgba(34, 211, 238, 0.10), transparent 32%),
    radial-gradient(circle at 80% 20%, rgba(20, 184, 166, 0.08), transparent 30%),
    linear-gradient(135deg, #020617 0%, #03111f 45%, #020617 100%);
  color: var(--text-primary);
}
```

Tambahkan subtle grid:

```css
.command-grid {
  background-image:
    linear-gradient(rgba(34, 211, 238, 0.045) 1px, transparent 1px),
    linear-gradient(90deg, rgba(34, 211, 238, 0.045) 1px, transparent 1px);
  background-size: 36px 36px;
}
```

### 4.4 Typography

| Elemen | Style |
|---|---|
| Font utama | Inter / Geist Sans |
| Page title | 28–34px, font-semibold |
| Section title | 13–15px, uppercase, cyan |
| KPI label | 11–12px, uppercase, muted |
| KPI value | 26–34px, semibold |
| Table | 11–13px compact |
| Badge | 10–11px uppercase |

### 4.5 Layout Shell

Desktop-first. Target utama 16:9, 1920×1080.

```text
sidebar width: 240px / 256px
topbar height: 72px
status strip height: 44px
main padding: 20–24px
card gap: 12–16px
```

Minimum recommended viewport: `1440px`. Untuk layar kecil, dashboard boleh horizontal scroll agar command center density tetap terjaga.

### 4.6 Component Rules

Semua panel harus memakai `DashboardCard`.

```tsx
<DashboardCard title="TOP ISSUES TODAY" accent="cyan">
  ...
</DashboardCard>
```

Style card:
- background translucent dark
- border `1px solid var(--border-soft)`
- border-top subtle cyan on active/important
- radius 16px
- shadow card
- no pure white background
- no randomly colored card

### 4.7 Status Style

| Status | Warna | Label |
|---|---|---|
| good | green | GOOD, NORMAL, AMAN, ONLINE, READY |
| warning | amber | WATCH, WASPADA, MODERATE, IN PROGRESS |
| danger | red | HIGH, OVERDUE, DANGER, NOT READY |
| info | cyan/blue | OPEN, MONITORING |
| neutral | slate | UPCOMING, DRAFT |

### 4.8 Chart Style

Semua chart harus dark-compatible:
- grid line: `rgba(148, 163, 184, 0.14)`
- axis text: `#8ba6b8`
- tooltip background: `#071827`
- tooltip border: cyan translucent
- line stroke: cyan, green, amber
- chart height konsisten per card

Do not use default Recharts colors.

---

## 5. Reusable Component Library

### 5.1 `AppShell`

Responsibilities:
- render sidebar
- render top header
- render status strip
- main scroll area
- consistent background and grid

Props:
```ts
type AppShellProps = {
  activeModule: ModuleKey;
  title: string;
  subtitle?: string;
  children: React.ReactNode;
};
```

### 5.2 `SidebarNav`

Menu:
1. Dashboard
2. Unit Readiness
3. Operation Performance
4. Efficiency
5. Energy Primary
6. Chemical & Water Quality
7. Action Plan
8. Reports
9. Admin, only for super admin

Sidebar bottom:
- PLTU Tenayan
- Pekanbaru, Riau
- weather card
- last data update

### 5.3 `TopHeader`

Content:
- page title
- date
- time
- shift badge
- search input
- notification bell
- user profile dropdown

### 5.4 `StatusStrip`

Common status:
- System Status
- Weather
- Coal Supply
- CEMS Status
- Water Source, khusus Chemical module
- Data Completeness, khusus Reports module

### 5.5 `KpiCard`

Props:
```ts
type KpiCardProps = {
  label: string;
  value: string;
  unit?: string;
  icon: LucideIcon;
  status?: 'good' | 'warning' | 'danger' | 'info' | 'neutral';
  target?: string;
  delta?: string;
  footer?: string;
};
```

### 5.6 `StatusBadge`

Props:
```ts
type StatusBadgeProps = {
  status: 'GOOD' | 'NORMAL' | 'AMAN' | 'ONLINE' | 'READY' |
          'WATCH' | 'WASPADA' | 'MODERATE' | 'IN PROGRESS' |
          'HIGH' | 'OVERDUE' | 'DANGER' | 'NOT READY' |
          'OPEN' | 'MONITORING' | 'DONE' | 'COMPLETED' | 'DRAFT';
};
```

### 5.7 `DataTable`

Compact table:
- sticky header optional
- dense row height 36–44px
- status badge support
- progress bar support
- action menu support

### 5.8 `PhotoGrid`

Untuk coalyard/drainase/upload gambar:
- 3-column or 2-column responsive
- thumbnail image
- timestamp
- location
- status
- uploaded by
- caption
- modal preview

### 5.9 `GaugeCard`

Gauge:
- semicircle or donut style
- value center
- good/warning/danger color arc
- target/threshold optional

---

## 6. Users, Role, Access, dan Workflow

### 6.1 User Roles

| Role | Akses |
|---|---|
| `super_admin` | semua modul, user management, threshold, master data |
| `gm` | view semua, approve final report, escalation |
| `manager_operasi` | view semua, approve laporan operasi/readiness/performance |
| `manager_enjiniring` | view semua, approve efficiency/technical recommendation |
| `manager_ep` | approve energy primary |
| `manager_kimia` | approve chemical & water quality |
| `operator_operasi` | input readiness, status unit, operating data |
| `admin_kinerja` | input KPI performance, target, prognosa |
| `admin_efisiensi` | input efficiency parameters |
| `admin_energi_primer` | input fuel stock, supply, logistics, coalyard photos |
| `admin_kimia_lab` | input chemical usage, pH, turbidity, water intake, action quality |
| `maintenance_user` | update outage, equipment, action follow-up |
| `viewer` | readonly dashboard dan reports |
| `auditor` | readonly + export + audit log |

### 6.2 Module Ownership

| Modul | Input Owner | Reviewer | Viewer |
|---|---|---|---|
| Executive Dashboard | auto dari semua modul | Manager Operasi / GM | semua role viewer |
| Unit Readiness | operator_operasi, maintenance_user | manager_operasi | semua |
| Operation Performance | admin_kinerja | manager_operasi | semua |
| Efficiency | admin_efisiensi | manager_enjiniring / manager_operasi | semua |
| Energy Primary | admin_energi_primer | manager_ep | semua |
| Chemical & Water Quality | admin_kimia_lab | manager_kimia | semua |
| Action Plan | semua PIC terkait | manager modul / GM | semua |
| Reports | system + admin report | manager_operasi / GM | semua sesuai permission |

### 6.3 Submission Status

Setiap data harian/modul memiliki status:

```text
DRAFT
SUBMITTED
RETURNED
APPROVED
LOCKED
ARCHIVED
```

Rules:
- User input membuat draft.
- Setelah submit, reviewer dapat approve atau return.
- Setelah report meeting pagi terkunci, data menjadi locked.
- Koreksi setelah locked harus melalui revision log.

### 6.4 Daily Reporting Session

Setiap hari ada satu session:

```ts
type DailyReportSession = {
  id: string;
  reportDate: string;
  shift: 'PAGI' | 'SORE' | 'MALAM';
  status: 'OPEN' | 'IN_REVIEW' | 'APPROVED' | 'LOCKED';
  meetingTime: string;
  createdBy: string;
  lockedAt?: string;
};
```

Dashboard utama membaca data dari session aktif.

---

## 7. Master Data

### 7.1 Plant & Unit

```ts
Plant {
  id
  name: "PLTU Tenayan"
  location: "Pekanbaru, Riau"
  timezone: "Asia/Jakarta"
}

Unit {
  id
  name: "Unit #1" | "Unit #2"
  installedCapacityMw
  commercialOperationDate
  status
}
```

### 7.2 Parameter Threshold

Harus configurable dari admin.

```ts
Threshold {
  id
  moduleKey
  parameterKey
  label
  unit
  direction: 'higher_is_better' | 'lower_is_better' | 'range_is_good'
  goodMin?
  goodMax?
  warningMin?
  warningMax?
  dangerMin?
  dangerMax?
}
```

Contoh:
- EAF: higher is better
- EFOR: lower is better
- NPHR: lower is better
- SFC: lower is better
- pH: range is good
- turbidity: lower is better
- coal stock days: range/above minimum
- water intake level: range is good

---

## 8. Module 1 — Executive Morning Dashboard

### 8.1 Tujuan

Menjadi layar utama meeting pagi. Menjawab:

```text
Apakah unit aman?
Apa isu utama hari ini?
Apa KPI merah/amber?
Stock batubara aman berapa hari?
Kualitas air/cooling tower aman?
Action apa yang overdue?
Apa fokus meeting pagi hari ini?
```

### 8.2 User

| Role | Aksi |
|---|---|
| GM | view, approve final brief, escalation |
| Manager Operasi | view, adjust morning brief, approve dashboard |
| Semua admin modul | memastikan datanya submit sebelum meeting |
| Viewer | readonly |

### 8.3 UI Panel

Top KPI cards:
1. Unit #1 Status
2. Unit #2 Status
3. Net Load Total
4. Coal Stock Days
5. EAF
6. EFOR
7. NPHR
8. Open Actions

Secondary:
- Plant Health & Readiness
- Top Issues Today
- Morning Brief
- Unit Load Trend
- Efficiency Snapshot
- Coal Stock Projection
- Water Intake Status
- Outage Progress
- Open Action Items
- Plant Overview

### 8.4 Data Dependencies

| Panel | Source |
|---|---|
| Unit status | readiness_status |
| Net load | unit_load_readings |
| Coal stock days | fuel_inventory |
| EAF/EFOR/NPHR | performance_kpi |
| Open actions | action_items |
| Morning brief | generated_summary/manual manager text |
| Plant overview | combined latest module status |

### 8.5 Business Rules

- Jika salah satu status unit danger, top dashboard harus warning.
- Top Issues Today diambil dari:
  - high priority action item
  - parameter abnormal
  - overdue issue
  - KPI tidak tercapai
- Morning Brief dapat auto-generated dari data, tapi manager bisa edit sebelum approve.
- Dashboard tidak boleh menampilkan data `DRAFT`; hanya `SUBMITTED` atau `APPROVED`. Untuk MVP bisa tampilkan draft dengan label `unverified`.

---

## 9. Module 2 — Unit Readiness

### 9.1 Tujuan

Monitoring kesiapan unit, derating, outage, operating hours, target firing, dan risiko teknis.

### 9.2 User

| Role | Aksi |
|---|---|
| operator_operasi | input load, status unit, operating hours |
| maintenance_user | update outage, derating cause, critical path |
| manager_operasi | review dan approve |
| gm | view dan escalation |

### 9.3 UI Panel

Top KPI:
- Unit #1 Available MW
- Unit #2 Available MW
- Total Available Capacity
- Derating Loss
- Operating Hours #1
- Operating Hours #2
- Active Risks
- Outage Progress

Panels:
- Unit Availability & Derating
- Critical Risks & Action Plan
- Operating Hours Since Last Outage
- Target Firing / PO Countdown
- Planned vs Actual Outage Progress
- Reliability Status
- Unit #1 Detail Card
- Unit #2 Detail Card
- Outage Milestone Timeline

### 9.4 Input Fields

```ts
UnitReadinessInput {
  reportDate
  shift
  unitId
  installedCapacityMw
  baseLoadMw
  currentLoadMw
  availableCapacityMw
  deratingMw
  deratingCause
  status: ONLINE | DERATING | OUTAGE | STANDBY
  operatingHoursSinceLastOutage
  lastPlannedOutageDate
  nextScheduledOutageDate
  targetFiringDate
  technicalNotes
}
```

### 9.5 Outage Fields

```ts
OutageEvent {
  unitId
  outageType: PO | FO | MO | BOILER_INSPECTION | SERIOUS_INSPECTION
  startDate
  targetEndDate
  actualEndDate?
  plannedDurationDays
  actualDurationDays?
  progressPlannedPct
  progressActualPct
  criticalPath
  status: PLANNED | IN_PROGRESS | COMPLETED | DELAYED
}
```

### 9.6 Rules

- Derating Loss = Installed Capacity - Available/Current Capability, berdasarkan rule final.
- Jika progress actual < planned dan tanggal sudah lewat, status `DELAYED`.
- Jika current load jauh di bawah base load, trigger issue.
- Jika outage critical path tidak update > 24 jam, trigger reminder.

---

## 10. Module 3 — Operation Performance

### 10.1 Tujuan

Memantau KPI operasi seperti EAF, EFOR, NPHR, SFC, NCF, OP Man, Eff Man, prognosa, dan sisa indikator kinerja.

### 10.2 User

| Role | Aksi |
|---|---|
| admin_kinerja | input realisasi, target, prognosa |
| manager_operasi | review dan approve |
| gm | view summary |

### 10.3 UI Panel

Top KPI:
- EAF
- EFOR
- NPHR
- SFC
- NCF
- OP. MAN
- EFF. MAN
- Prognosa

Panels:
- KPI Trend Actual vs Target
- KPI Achievement Matrix
- Sisa Indikator Kinerja
- Status Operasi Unit
- Performance Alerts
- Performance Breakdown by Unit
- Benchmark vs Target Bulanan
- Catatan Operasi

### 10.4 Input Fields

```ts
PerformanceKpiInput {
  reportDate
  indicatorKey: EAF | EFOR | NPHR | SFC | NCF | OP_MAN | EFF_MAN
  unitId?
  realisasi
  target
  pencapaianPct
  prognosaSm2?
  period: DAILY | MTD | YTD | SEMESTER
  notes
}
```

### 10.5 Sisa Indikator

```ts
PerformanceAllowance {
  reportDate
  indicator: PO | FO | EMDH | EFDH
  usedHours
  remainingHours
  targetHours
  status
}
```

### 10.6 Rules

- `higher_is_better`: pencapaian = realisasi / target.
- `lower_is_better`: pencapaian harus dihitung khusus agar angka kecil dianggap baik.
- Jangan hardcode warna KPI. Gunakan threshold engine.
- Jika KPI merah/amber, wajib ada alasan atau action item.

---

## 11. Module 4 — Efficiency Monitoring

### 11.1 Tujuan

Membandingkan parameter aktual vs baseline dan mengidentifikasi loss efisiensi.

### 11.2 User

| Role | Aksi |
|---|---|
| admin_efisiensi | input/import parameter aktual dan baseline |
| operator_operasi | menambahkan catatan manuver operasi |
| manager_enjiniring | review rekomendasi teknis |
| manager_operasi | view dan approve action |

### 11.3 UI Panel

Top KPI:
- Gross Load
- Nett Load
- Coal Flow
- NPHR
- SFC
- Auxiliary Power
- Boiler Efficiency
- Vacuum Condenser

Panels:
- Parameter Comparison Actual vs Baseline
- Efficiency Loss Analysis
- Make Up Water Unit #1/#2
- Output Factor / NPHR / SFC Trend
- Heat Rate Trend
- Operator Insight
- Unit #1 Abnormal Parameters
- Unit #2 Abnormal Parameters

### 11.4 Input Fields

```ts
EfficiencyParameterInput {
  reportDate
  unitId
  parameterKey:
    GROSS_LOAD |
    NETT_LOAD |
    COAL_FLOW |
    MAIN_STEAM_PRESSURE |
    MAIN_STEAM_TEMP |
    MAIN_STEAM_FLOW |
    FINAL_FEED_WATER_TEMP |
    VACUUM_CONDENSER |
    AH_OUTLET_FLUE_GAS_TEMP |
    ECO_OUT_O2_GAS |
    SH_SPRAY_FLOW |
    AUXILIARY_POWER |
    MAKE_UP_WATER |
    OUTPUT_FACTOR |
    NPHR |
    SFC |
    BOILER_EFFICIENCY
  actualValue
  baselineValue
  unit
  timestamp
  notes
}
```

### 11.5 Efficiency Loss

```ts
EfficiencyLossItem {
  reportDate
  unitId
  category:
    EXCESS_O2 |
    HIGH_FLUE_GAS_TEMP |
    LOW_VACUUM_CONDENSER |
    BOILER_HEAT_LOSS |
    SH_SPRAY_OVERUSE |
    AUX_POWER_CONSUMPTION |
    BLOWDOWN_LOSS |
    OTHER
  contributionPct
  likelyCause
  recommendedAction
}
```

### 11.6 Operator Insight Rules

Phase 1:
- rule-based recommendation.
- Example:
  - AH Outlet Flue Gas Temp high → "Optimalkan sootblow."
  - O2 high → "Kalibrasi coal feeder / setting excess air."
  - Vacuum condenser tidak optimal → "Cek cooling tower, leak, air ingress."
  - SH spray tinggi → "Review kualitas batubara dan pembakaran."

Phase 2:
- AI recommendation with historical patterns.

---

## 12. Module 5 — Energy Primary

### 12.1 Tujuan

Monitoring energi primer: stock batubara, pasokan, co-firing, maturity level, trucking, vessel, tongkang, BBM, coalyard, drainase, dan kesiapan Dong Feng/alat.

### 12.2 User

| Role | Aksi |
|---|---|
| admin_energi_primer | input stock, pasokan, trucking, photos |
| manager_ep | review dan approve |
| maintenance_user | update alat Dong Feng/peralatan |
| manager_operasi/gm | view risiko pasokan |

### 12.3 UI Panel

Top KPI:
- Flowrate Coal
- Coal Stock Days
- Co-firing kWh Green
- Maturity Level
- Stock Efektif
- Stock Apung
- Stock OTW
- Fuel Supply Status

Panels:
- Coal Inventory Projection
- Supply Fulfillment Matrix by Supplier
- Tongkang Status
- Vessel Status
- Trucking Batubara
- Trucking Biomassa
- Stock BBM
- Penerimaan
- Pemakaian
- Coalyard & Drainage Monitoring
- Dong Feng Readiness
- Coal Composition by Vendor

### 12.4 Input Fields

```ts
FuelInventoryInput {
  reportDate
  flowrateTonPerHour
  coalStockDays
  stockEfektifTon
  stockApungTon
  stockOtwTon
  stockMinimumTargetTon
  stockTargetDays
  cofiringKwhGreen
  cofiringTargetKwh
  maturityLevel
  maturityTarget
  notes
}
```

```ts
FuelSupplyInput {
  reportDate
  supplierId
  plannedMt
  confirmedMt
  realMt
  carryOverMt
  fulfillmentPct
  status
}
```

```ts
FuelLogisticsInput {
  reportDate
  type: TONGKANG | VESSEL | TRUCKING_BATUBARA | TRUCKING_BIOMASSA
  loadingCount
  sailingCount
  queueCount
  standbyCount
  unloadingYesterdayCount
  planTodayCount
  notes
}
```

```ts
FuelPhotoEvidence {
  reportDate
  category: COALYARD_A | COALYARD_B | COALYARD_C | DRAINASE_A | DRAINASE_B | COALY
  imageUrl
  thumbnailUrl
  timestampTaken
  locationText
  status: NORMAL | WATCH | DANGER
  caption
  uploadedBy
}
```

```ts
EquipmentReadiness {
  reportDate
  equipmentName
  equipmentType: DONG_FENG | STACKER | RECLAIMER | CONVEYOR | CRUSHER | OTHER
  status: READY | NOT_READY | MAINTENANCE
  location
  note
  photoUrl?
}
```

### 12.5 Rules

- Coal Stock Days < target minimum → warning/danger.
- Supply fulfillment < threshold → WATCH.
- Dong Feng not ready → action item otomatis jika critical.
- Foto coalyard/drainase wajib minimal 1x per report session.
- Upload gambar harus punya timestamp dan uploader.

---

## 13. Module 6 — Chemical & Water Quality

### 13.1 Tujuan

Monitoring chemical usage, biaya, level intake Sungai Siak, pH, turbidity, cooling tower quality, serta action abnormal.

### 13.2 User

| Role | Aksi |
|---|---|
| admin_kimia_lab | input chemical, water quality, abnormal action |
| manager_kimia | review dan approve |
| operator_operasi | view efek ke operasi |
| manager_operasi/gm | view risk summary |

### 13.3 UI Panel

Top KPI:
- Monthly Chemical Cost
- Annual Chemical Cost
- Water Intake Level
- pH Status
- Turbidity Status
- Cooling Tower #1
- Cooling Tower #2
- Open Quality Actions

Panels:
- Chemical Consumption by Area
- Chemical Cost Composition
- Level Intake Sungai Siak
- pH CT #1 & #2 vs Inlet Make-up Water
- Turbidity CT #1 & #2 vs Inlet Make-up Water
- Abnormal Quality Action Log
- Water Quality Risk

### 13.4 Chemical Usage Input

```ts
ChemicalUsageInput {
  reportDate
  chemicalName:
    TSP |
    AMMONIA |
    HYDRAZINE |
    NAOH |
    PAC_POWDER |
    PAC_LIQUID |
    NAOCL |
    HCL |
    BACTERICIDE |
    CORROSION_INHIBITOR |
    SCALE_INHIBITOR |
    NON_OXI_BIOCIDE |
    BIODISPERSANT |
    OTHER
  area:
    BOILER |
    WTP |
    WATER_PRETREATMENT |
    COOLING_TOWER
  dailyUsage
  monthlyCumulative
  yearlyCumulative
  unit: LITER | KG | TON
  unitPriceIdr
  dailyCostIdr
  monthlyCostIdr
  yearlyCostIdr
}
```

### 13.5 Water Intake Input

```ts
WaterIntakeReading {
  reportDate
  timestamp
  source: SUNGAI_SIAK
  levelMeter
  permitMinimalLevel
  emergencyPumpStatus
  notes
}
```

### 13.6 Water Quality Input

```ts
WaterQualityReading {
  reportDate
  timestamp
  location:
    SUNGAI_SIAK |
    PRETREATMENT |
    CT_1 |
    CT_2 |
    OUTLET_GF_1 |
    OUTLET_GF_2 |
    OUTLET_GF_3 |
    OUTLET_GF_4 |
    OUTLET_GF_5 |
    OUTLET_GF_6 |
    INLET_MAKEUP_WATER |
    BOILER_FEEDWATER
  parameter: PH | TURBIDITY | CONDUCTIVITY | SILICA | TDS | COC | TEMP | OTHER
  value
  unit
  thresholdStatus
  notes
}
```

### 13.7 Abnormal Quality Action

```ts
AbnormalQualityAction {
  reportDate
  eventDateTime
  durationMinutes
  parameterAbnormal
  location
  actionTaken
  evaluatedCause
  followUp
  pic
  status: OPEN | IN_PROGRESS | MONITORING | COMPLETED
}
```

### 13.8 Rules

- pH outside normal range → abnormal event.
- turbidity above threshold → abnormal event.
- repeated abnormal > N hours/days → escalation.
- cost chemical monthly > budget threshold → warning.
- high chemical consumption must have note/cause.

---

## 14. Module 7 — Action Plan Center

### 14.1 Tujuan

Menjadi pusat tindak lanjut semua temuan meeting pagi.

Ini adalah jantung aplikasi. Setiap issue harus punya:
- sumber modul,
- prioritas,
- dampak,
- PIC,
- deadline,
- progress,
- evidence,
- blocker,
- update terakhir.

### 14.2 User

| Role | Aksi |
|---|---|
| semua admin modul | membuat action dari modul masing-masing |
| PIC action | update progress, evidence, blocker |
| manager modul | review/approve close |
| gm | escalation |
| viewer | readonly |

### 14.3 UI Panel

Top KPI:
- Open Actions
- Overdue Actions
- High Priority Issues
- Completed This Week
- Average Closure Time
- PIC Active
- Escalated Issues
- Compliance Rate

Panels:
- Filters
- Action Plan Tracker
- Action Kanban
- Summary by Module
- Action Aging Timeline
- Actions by Priority
- Executive Escalations
- Issue Drill-Down

### 14.4 Action Item Schema

```ts
ActionItem {
  id
  reportDate
  title
  description
  sourceModule:
    DASHBOARD |
    READINESS |
    PERFORMANCE |
    EFFICIENCY |
    ENERGY_PRIMARY |
    CHEMICAL_WATER |
    REPORTS |
    MANUAL
  sourceRecordId?
  category
  priority: HIGH | MEDIUM | LOW | INFO
  impactLevel: HIGH | MEDIUM | LOW
  impactMw?
  impactCostIdr?
  riskLevel?
  rootCause
  currentAction
  recommendedAction
  picUserId
  dueDate
  progressPct
  status: OPEN | IN_PROGRESS | MONITORING | DONE | CANCELLED
  blockers
  lastUpdate
  nextStep
  createdBy
  approvedClosedBy?
}
```

### 14.5 Evidence Schema

```ts
ActionEvidence {
  id
  actionItemId
  fileUrl
  fileType: IMAGE | PDF | EXCEL | DOC | OTHER
  caption
  uploadedBy
  uploadedAt
}
```

### 14.6 Rules

- Action overdue if `dueDate < today` and status not DONE.
- High priority overdue masuk Executive Escalations.
- Action cannot be DONE without final note/evidence or manager override.
- Progress update wajib mencatat `lastUpdate`.
- Close action but abnormal parameter still active → system warns.

---

## 15. Module 8 — Reports & Morning Brief

### 15.1 Tujuan

Pusat auto-generated report meeting pagi, export, approval, archive, dan AI brief.

### 15.2 User

| Role | Aksi |
|---|---|
| admin report / manager_operasi | generate, review, edit brief |
| GM | approve final |
| viewer | download sesuai akses |
| system | auto-generate scheduled report |

### 15.3 UI Panel

Top KPI:
- Reports Generated Today
- Scheduled Reports
- Data Completeness
- Pending Approval
- Export Success Rate
- Meeting Readiness
- AI Brief Status
- Archive Size

Panels:
- Morning Brief Generator
- Report Preview
- Report Schedule Calendar
- Data Source Completion
- Approval Workflow
- Export & Distribution
- Report Archive
- Report Cover Preview
- Insight Assistant

### 15.4 Report Types

```ts
ReportType =
  EXECUTIVE_DAILY_REPORT |
  UNIT_READINESS_REPORT |
  OPERATION_PERFORMANCE_REPORT |
  EFFICIENCY_REPORT |
  ENERGY_PRIMARY_REPORT |
  CHEMICAL_WATER_QUALITY_REPORT |
  ACTION_PLAN_REPORT
```

### 15.5 Report Export Schema

```ts
ReportExport {
  id
  reportDate
  reportType
  title
  version
  status: DRAFT | READY | IN_REVIEW | FINAL | DISTRIBUTED
  generatedBy: SYSTEM | USER
  approvedBy?
  pdfUrl?
  pptUrl?
  excelUrl?
  dataCompletenessPct
  createdAt
}
```

### 15.6 Data Completeness

Data completeness dihitung dari module submission:

```text
Readiness submitted?
Performance submitted?
Efficiency submitted?
Energy Primary submitted?
Chemical & Water submitted?
Action update submitted?
Weather available?
Photos required uploaded?
```

Jika completeness < 90%, report status tidak boleh FINAL tanpa override manager.

---

## 16. Weather, CEMS, Shift, dan Data Pendukung

### 16.1 Weather

Weather card harus tersedia karena mockup memilikinya.

Data fields:
```ts
WeatherReading {
  reportDate
  timestamp
  location: "Pekanbaru, Riau"
  temperatureC
  humidityPct
  windSpeedMs
  conditionText
  source: API | MANUAL
}
```

MVP:
- Manual input oleh admin operasi, atau
- Weather API dengan cache.

UI:
- Sidebar bottom weather.
- Status strip top.
- Reports data completeness.

### 16.2 CEMS Status

```ts
CemsStatus {
  reportDate
  unitId
  status: ONLINE | OFFLINE | MAINTENANCE | ERROR
  lastReadingAt
  note
}
```

### 16.3 Shift

```ts
Shift = PAGI | SORE | MALAM
```

Meeting pagi default `PAGI`.

---

## 17. Backend Database Draft

Gunakan PostgreSQL.

### 17.1 Core Tables

```sql
users
roles
user_roles
plants
units
daily_report_sessions
module_submissions
approval_logs
audit_logs
attachments
weather_readings
system_status_snapshots
thresholds
```

### 17.2 Module Tables

```sql
unit_readiness_reports
outage_events
outage_milestones
unit_risk_items

performance_kpi_reports
performance_allowances
performance_alerts

efficiency_parameter_readings
efficiency_loss_items
operator_insights

fuel_inventory_reports
fuel_supply_items
fuel_logistics_reports
fuel_stock_bbm
fuel_photo_evidence
equipment_readiness

chemical_usage_reports
chemical_cost_items
water_intake_readings
water_quality_readings
abnormal_quality_actions

action_items
action_updates
action_evidence
executive_escalations

report_exports
report_schedules
distribution_lists
distribution_recipients
```

### 17.3 Audit Log

Setiap create/update/delete harus masuk audit:

```ts
AuditLog {
  actorUserId
  action: CREATE | UPDATE | DELETE | SUBMIT | APPROVE | RETURN | LOCK | EXPORT
  entityType
  entityId
  beforeJson?
  afterJson?
  createdAt
}
```

---

## 18. API Endpoint Draft

### 18.1 Auth

```text
POST /api/auth/login
POST /api/auth/logout
GET  /api/auth/me
```

Jika Supabase Auth, gunakan client SDK.

### 18.2 Dashboard

```text
GET /api/dashboard/morning?date=YYYY-MM-DD&shift=PAGI
GET /api/dashboard/status-strip?date=YYYY-MM-DD
```

### 18.3 Module Submission

```text
GET  /api/submissions?date=YYYY-MM-DD
POST /api/submissions/:module/draft
POST /api/submissions/:module/submit
POST /api/submissions/:module/approve
POST /api/submissions/:module/return
```

### 18.4 Per Module

```text
GET/POST /api/readiness
GET/POST /api/performance
GET/POST /api/efficiency
GET/POST /api/energy-primary
GET/POST /api/chemical-water
GET/POST /api/action-plan
GET/POST /api/reports
```

### 18.5 Upload

```text
POST /api/uploads
GET  /api/uploads/:id
DELETE /api/uploads/:id
```

### 18.6 Export

```text
POST /api/reports/generate
POST /api/reports/:id/export-pdf
POST /api/reports/:id/export-ppt
POST /api/reports/:id/distribute
```

---

## 19. Data Mock untuk Frontend Static

Buat dummy data di:

```text
lib/mock-data/
├─ dashboard.ts
├─ readiness.ts
├─ performance.ts
├─ efficiency.ts
├─ energy-primary.ts
├─ chemical-water.ts
├─ action-plan.ts
├─ reports.ts
├─ users.ts
└─ weather.ts
```

Data harus realistis, bukan placeholder `Lorem ipsum`.

Contoh user:
```ts
export const users = [
  { id: 'u001', name: 'A. Pratama', role: 'manager_operasi', department: 'Operasi' },
  { id: 'u002', name: 'M. Hidayat', role: 'operator_operasi', department: 'Operasi' },
  { id: 'u003', name: 'R. Saputra', role: 'admin_kinerja', department: 'Kinerja' },
  { id: 'u004', name: 'D. Kurniawan', role: 'admin_efisiensi', department: 'Efisiensi' },
  { id: 'u005', name: 'S. Widodo', role: 'admin_energi_primer', department: 'Energi Primer' },
  { id: 'u006', name: 'T. Prabowo', role: 'admin_kimia_lab', department: 'Kimia' },
  { id: 'u007', name: 'S. Hidayat', role: 'gm', department: 'Manajemen' }
];
```

---

## 20. Upload Gambar

### 20.1 Modul yang Membutuhkan Upload

| Modul | Jenis gambar |
|---|---|
| Energy Primary | Coalyard A/B/C, Drainase A/B, Coal Yard, kondisi alat |
| Unit Readiness | foto outage, critical path, equipment abnormal |
| Efficiency | foto panel/alat, coal feeder, condenser/cooling tower evidence |
| Chemical & Water | foto sampling, lab result, kondisi intake/cooling tower |
| Action Plan | evidence follow-up |
| Reports | cover image optional |

### 20.2 Metadata Wajib

Setiap upload harus menyimpan:
- fileUrl
- thumbnailUrl
- uploadedBy
- uploadedAt
- module
- reportDate
- category
- locationText
- caption
- status
- originalFilename
- fileSize
- mimeType

### 20.3 UI Behavior

- Drag and drop.
- Preview thumbnail.
- Modal image viewer.
- Upload progress.
- Delete only before submission unless manager override.
- Show timestamp and uploader.
- Compress image and generate thumbnail.

---

## 21. Frontend Visual Acceptance Criteria

Agar hasil Codex tidak melenceng, masukkan acceptance criteria ini ke GSD.

### 21.1 Global UI

- [ ] Layout dark command center, bukan admin dashboard biasa.
- [ ] Sidebar fixed dengan brand `TENAYAN COMMAND CENTER`.
- [ ] Topbar berisi date, time, shift, search, notification, user.
- [ ] Semua module menggunakan shell yang sama.
- [ ] Semua card memakai background gelap translucent dan border cyan soft.
- [ ] Tidak ada card putih.
- [ ] Tidak ada warna pastel random.
- [ ] Status color konsisten: green/amber/red/cyan.
- [ ] Dashboard terlihat dense tetapi rapi di 1920×1080.
- [ ] Semua chart dark compatible.
- [ ] Typography compact dan professional.

### 21.2 Per Module

- [ ] Dashboard utama punya 8 KPI top cards.
- [ ] Readiness punya availability/derating chart dan outage timeline.
- [ ] Performance punya KPI trend dan achievement matrix.
- [ ] Efficiency punya actual vs baseline table dan loss analysis.
- [ ] Energy Primary punya stock projection, supplier matrix, photo grid, Dong Feng readiness.
- [ ] Chemical Water punya chemical cost, intake trend, pH/turbidity trend, abnormal log.
- [ ] Action Plan punya table, kanban, aging timeline, issue drill-down.
- [ ] Reports punya morning brief generator, report preview, export distribution.

### 21.3 Screenshot QA

Developer/Codex harus menghasilkan screenshot setiap phase dan dibandingkan dengan mockup referensi di `docs/mockups/`.

Buat script:
```bash
npm run screenshot:dashboard
npm run screenshot:all
```

Target:
- Visual similarity minimal 80–90% secara layout.
- Jika chart detail berbeda tidak masalah, tapi:
  - posisi panel,
  - color system,
  - card style,
  - sidebar/topbar,
  - density,
  - typography,
  harus mendekati mockup.

---

## 22. GSD Project Prompt yang Bisa Diberikan ke Codex

Gunakan ini saat `/gsd-new-project` atau command project init GSD bertanya “what are you building?”:

```text
Saya membangun Tenayan Command Center, sebuah web application command center untuk digitalisasi meeting pagi PLTU Tenayan. Target utama phase pertama adalah frontend static prototype yang sangat mirip dengan mockup dark premium high-tech command center yang tersedia di docs/mockups. Jangan fokus backend dulu. Bangun Next.js + TypeScript + Tailwind + shadcn/ui + Recharts + lucide-react. Aplikasi memiliki fixed sidebar, top header, status strip, KPI cards, dark glassmorphism panels, chart cards, data tables, gauge/donut/waterfall charts, photo grid, action kanban, dan report preview.

Modul yang harus ada:
1. Executive Morning Dashboard
2. Unit Readiness
3. Operation Performance
4. Efficiency Monitoring
5. Energy Primary
6. Chemical & Water Quality
7. Action Plan Center
8. Reports & Morning Brief
9. Admin/Master Data later

Gunakan dummy data realistis dulu. UI harus desktop-first 16:9, dark navy, cyan/teal accent, green/amber/red status. Jangan gunakan template admin generik, jangan ada card putih, jangan gunakan warna random. Semua halaman harus memakai design token dan reusable component library yang sama.
```

---

## 23. GSD Phase Plan yang Direkomendasikan

### Phase 1 — Project Setup & Design Tokens

Goal:
- Setup Next.js, TypeScript, Tailwind, shadcn/ui, Recharts, lucide, Framer Motion.
- Buat design tokens.
- Buat dark command center background.

Deliverables:
- `tailwind.config.ts`
- `app/globals.css`
- `lib/design-tokens.ts`
- base route working

Acceptance:
- App terbuka dengan dark background dan grid subtle.
- Tokens digunakan, bukan inline random colors.

### Phase 2 — App Shell

Goal:
- Build `AppShell`, `SidebarNav`, `TopHeader`, `StatusStrip`.

Acceptance:
- Sidebar/topbar sama di semua page.
- Active nav state.
- Weather and user card visible.
- Mobile minimal acceptable, desktop perfect.

### Phase 3 — Component Library

Goal:
- Build dashboard components:
  - KpiCard
  - DashboardCard
  - StatusBadge
  - DataTable
  - GaugeCard
  - PhotoGrid
  - chart wrappers

Acceptance:
- Components reusable and story/demo page available.
- Visual matches mockup.

### Phase 4 — Executive Morning Dashboard

Goal:
- Build `/dashboard` using dummy data.

Acceptance:
- Layout matches main mockup.
- 8 KPI top cards.
- Top Issues, Morning Brief, charts, action items, plant overview visible.

### Phase 5 — Unit Readiness

Goal:
- Build `/unit-readiness`.

Acceptance:
- Availability/derating chart, critical risk table, operating hours, PO countdown, outage timeline.

### Phase 6 — Operation Performance

Goal:
- Build `/operation-performance`.

Acceptance:
- KPI cards, KPI trend, matrix, sisa indikator, performance alerts.

### Phase 7 — Efficiency Monitoring

Goal:
- Build `/efficiency`.

Acceptance:
- Actual vs baseline table, efficiency loss waterfall, make up water, operator insight.

### Phase 8 — Energy Primary

Goal:
- Build `/energy-primary`.

Acceptance:
- Stock projection, supplier matrix, logistics status, coalyard photo grid, Dong Feng readiness.

### Phase 9 — Chemical & Water Quality

Goal:
- Build `/chemical-water-quality`.

Acceptance:
- Chemical cost, consumption, intake level, pH/turbidity trend, abnormal action log.

### Phase 10 — Action Plan Center

Goal:
- Build `/action-plan`.

Acceptance:
- KPI action cards, tracker table, kanban, aging timeline, drill-down card.

### Phase 11 — Reports & Morning Brief

Goal:
- Build `/reports`.

Acceptance:
- Morning brief generator, report preview, schedule, data completion, export buttons, archive, AI assistant panel.

### Phase 12 — Forms & Auth Mock

Goal:
- Build mock login and input forms per module without real DB.

Acceptance:
- Role-based navigation.
- Different users can see different input buttons.
- Draft/submit/review flow simulated.

### Phase 13 — Backend Integration

Goal:
- Integrate real auth, DB, storage, submissions.

Acceptance:
- Dashboard reads DB.
- Submit/approve persists.
- Audit log active.
- File upload active.

---

## 24. Backend Acceptance Criteria

- [ ] User login works.
- [ ] Role-based permissions enforced server-side, not only UI.
- [ ] Each module has daily submission.
- [ ] Draft → submitted → approved workflow works.
- [ ] Dashboard aggregates approved/submitted data.
- [ ] Upload image works with metadata.
- [ ] Action item update and evidence works.
- [ ] Audit log records critical changes.
- [ ] Report export creates archive.
- [ ] Threshold engine calculates status consistently.

---

## 25. AI Phase 2 Blueprint

### 25.1 AI Features

| Feature | Description |
|---|---|
| Daily Briefing Generator | Membuat narasi meeting pagi dari data terbaru |
| Ask Tenayan Data | Chat dengan data historis |
| Anomaly Detection | Deteksi parameter abnormal |
| Root Cause Suggestion | Rekomendasi penyebab dari histori |
| Action Recommendation | Saran tindak lanjut berdasarkan kasus lama |
| Report Narration | Membuat narasi otomatis untuk report |
| Follow-up Reminder | Reminder PIC untuk overdue action |
| Predictive Coal Stock | Prediksi stock kritis |
| Efficiency Advisor | Analisa loss efisiensi dan rekomendasi |

### 25.2 AI Data Boundary

AI tidak boleh langsung mengarang. AI hanya boleh menjawab dari:
- data dashboard
- daily reports
- action log
- approved report archive
- uploaded documents yang sudah diindex
- threshold rules

### 25.3 AI Output Format

Setiap AI insight harus punya:
```text
Insight
Evidence/Data Source
Confidence
Recommended Action
PIC suggestion
Risk if ignored
```

### 25.4 OpenRouter Integration

Environment:
```env
OPENROUTER_API_KEY=
AI_MODEL_BRIEFING=
AI_MODEL_REASONING=
```

Phase 2 endpoint:
```text
POST /api/ai/daily-brief
POST /api/ai/ask
POST /api/ai/anomaly-summary
POST /api/ai/action-recommendation
```

---

## 26. Important Implementation Rules for Codex

### 26.1 UI First

Do:
- build static UI first
- use dummy data
- screenshot compare
- lock design tokens
- reuse components

Do not:
- start backend before UI shell and pages are visually accepted
- use generic admin dashboard layout
- use random colors
- create one-off page-specific card styles
- hardcode all data directly inside components

### 26.2 Data Structure

Do:
- store dummy data in separate files
- match future backend schema
- use TypeScript types
- prepare data adapters

Do not:
- mix mock data in JSX everywhere
- build UI that cannot be connected to DB later

### 26.3 Module Isolation

Each module should have:
```text
page component
mock data
types
input form later
status calculation
module-specific components if needed
```

### 26.4 Visual Consistency

All pages must:
- use same sidebar
- same topbar
- same KPI card structure
- same panel title style
- same status badge component
- same chart color palette
- same table density

---

## 27. First Codex Task

Berikan instruksi pertama ini ke Codex/GSD setelah project dibuat:

```text
Start Phase 1 only. Do not build all pages yet. Setup Next.js TypeScript Tailwind shadcn/ui Recharts lucide-react and Framer Motion. Create the dark command center design tokens and base AppShell skeleton. The UI must target a premium industrial command center like the provided mockups. No backend, no database, no auth yet. Use dummy static values only. After implementation, produce a screenshot of the shell at 1920x1080 and verify that the background, sidebar space, top header area, card style, and color tokens match the mockup direction.
```

---

## 28. Definition of Done for MVP

MVP selesai jika:

1. Semua modul utama tampil dengan style konsisten.
2. User berbeda dapat login.
3. Setiap bidang dapat input data laporan harian.
4. Data bisa disubmit dan direview.
5. Dashboard utama menggabungkan data semua modul.
6. Action plan dapat dibuat/update/close.
7. Upload gambar aktif untuk modul yang perlu evidence.
8. Report meeting pagi dapat digenerate minimal PDF.
9. Data historis bisa dilihat per tanggal.
10. UI tetap mirip mockup setelah backend terhubung.

---

## 29. Catatan Koreksi dari Mockup

Mockup sebelumnya ada beberapa dummy data/label yang boleh diperbaiki:
- Lokasi harus `Pekanbaru, Riau`, bukan Pontianak/Kalimantan Barat.
- Nama user hanya dummy; nanti diganti user internal.
- Nilai KPI dan angka mockup hanya contoh.
- Logo resmi PLN/NP hanya digunakan jika asset internal disediakan dan pengguna punya hak penggunaan.
- Plant illustration boleh berupa asset custom yang digenerate/diunggah, bukan logo resmi.

---

## 30. Ringkasan Prioritas

Urutan kerja yang paling aman:

```text
1. UI fidelity static prototype
2. Design system locked
3. Reusable components
4. All module pages with dummy data
5. Role/login mock
6. Backend schema
7. Real auth + DB
8. Form input per module
9. Upload image
10. Approval workflow
11. Report export
12. AI phase 2
```

**Kunci agar mirip mockup:**  
Codex harus dipaksa membangun dari design tokens dan component library, bukan membuat halaman satu per satu dengan style bebas.

