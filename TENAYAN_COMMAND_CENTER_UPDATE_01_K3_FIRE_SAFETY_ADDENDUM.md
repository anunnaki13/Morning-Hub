
---

# UPDATE 01 — PENAMBAHAN MODUL K3 & FIRE SAFETY / EMERGENCY READINESS

**Tanggal update:** 17 Mei 2026  
**Sumber kebutuhan baru:** dokumen `10. Laporan K3 (12 DESEMBER 2025).pptx`  
**Tujuan update:** menambahkan modul K3, Fire System, Emergency Equipment, Patrol Finding, Monitoring IZAT, dan P2K3 Patrol ke dalam Tenayan Command Center tanpa merusak target visual utama: dark premium high-tech command center seperti mockup.

## U0. Ringkasan Perubahan Blueprint

Tambahkan satu modul baru di aplikasi:

```text
K3 & Fire Safety
```

Nama teknis modul:

```text
hse-fire-safety
```

Nama alternatif untuk UI/brand internal:

```text
K3 & Emergency Readiness
HSE & Fire Safety Command
Safety & Emergency Readiness
```

Rekomendasi final untuk sidebar:

```text
K3 & Fire Safety
```

Modul ini mencakup:

1. Fire System Readiness.
2. Area Ketidaksiapan Fire System.
3. Emergency Equipment Readiness.
4. K3 Culture Dashboard.
5. Patrol Finding Management.
6. Total Open Finding & Aging > 2 Minggu.
7. Monitoring Upload IZAT.
8. P2K3 Patrol Evidence.
9. K3 Action Log yang tersambung ke Action Plan Center.
10. Report K3 untuk bahan meeting pagi.

---

## U1. Update Struktur Menu Sidebar

Ganti menu dari blueprint lama:

```text
1. Dashboard
2. Unit Readiness
3. Operation Performance
4. Efficiency
5. Energy Primary
6. Chemical & Water Quality
7. Action Plan
8. Reports
```

Menjadi:

```text
1. Dashboard
2. Unit Readiness
3. Operation Performance
4. Efficiency
5. Energy Primary
6. Chemical & Water Quality
7. K3 & Fire Safety
8. Action Plan
9. Reports
```

Tambahkan icon rekomendasi dari `lucide-react`:

```ts
K3_FIRE_SAFETY: ShieldAlert | Flame | Siren | HardHat
```

Rekomendasi terbaik:

```ts
ShieldAlert
```

Karena modul ini bukan hanya fire system, tetapi juga K3, patrol, finding, dan emergency readiness.

---

## U2. Update Struktur Folder

Tambahkan route, input form, mock data, dan komponen module-specific.

```text
app/
├─ k3-fire-safety/page.tsx
├─ input/
│  ├─ k3-fire-safety/page.tsx
│  ├─ fire-equipment/page.tsx
│  ├─ emergency-equipment/page.tsx
│  ├─ k3-patrol/page.tsx
│  └─ izat-upload/page.tsx

components/
├─ hse/
│  ├─ fire-equipment-readiness-matrix.tsx
│  ├─ fire-defect-register.tsx
│  ├─ emergency-equipment-grid.tsx
│  ├─ emergency-equipment-card.tsx
│  ├─ k3-culture-score-card.tsx
│  ├─ k3-culture-trend.tsx
│  ├─ patrol-finding-aging-chart.tsx
│  ├─ patrol-finding-table.tsx
│  ├─ p2k3-patrol-gallery.tsx
│  ├─ izat-upload-compliance.tsx
│  ├─ hse-action-log.tsx
│  └─ safety-risk-summary.tsx

lib/
├─ mock-data/
│  ├─ hse-fire-safety.ts
├─ hse-status-rules.ts
├─ hse-thresholds.ts
```

Update `SidebarNav` menu config:

```ts
const navItems = [
  { key: 'dashboard', label: 'Dashboard', href: '/dashboard', icon: LayoutDashboard },
  { key: 'unit-readiness', label: 'Unit Readiness', href: '/unit-readiness', icon: ShieldCheck },
  { key: 'operation-performance', label: 'Operation Performance', href: '/operation-performance', icon: ChartNoAxesCombined },
  { key: 'efficiency', label: 'Efficiency', href: '/efficiency', icon: Gauge },
  { key: 'energy-primary', label: 'Energy Primary', href: '/energy-primary', icon: Zap },
  { key: 'chemical-water-quality', label: 'Chemical & Water Quality', href: '/chemical-water-quality', icon: Droplets },
  { key: 'k3-fire-safety', label: 'K3 & Fire Safety', href: '/k3-fire-safety', icon: ShieldAlert },
  { key: 'action-plan', label: 'Action Plan', href: '/action-plan', icon: ClipboardList },
  { key: 'reports', label: 'Reports', href: '/reports', icon: FileText },
];
```

---

## U3. Update User Role dan Permission

Tambahkan role berikut:

| Role | Deskripsi | Akses Utama |
|---|---|---|
| `manager_k3` | reviewer/approver data K3 | review, approve, escalation modul K3 |
| `admin_k3` | admin utama K3 | input/edit fire system, K3 culture, patrol, IZAT, P2K3 |
| `officer_k3` | petugas/officer K3 lapangan | input temuan patrol, upload evidence, update follow-up |
| `fire_system_pic` | PIC fire system/emergency equipment | update kondisi hydrant, APAR, pump, deluge, gas system |
| `p2k3_user` | user patrol P2K3 | input hasil patrol P2K3 dan upload foto |

Update contoh dummy users:

```ts
export const users = [
  { id: 'u001', name: 'A. Pratama', role: 'manager_operasi', department: 'Operasi' },
  { id: 'u002', name: 'M. Hidayat', role: 'operator_operasi', department: 'Operasi' },
  { id: 'u003', name: 'R. Saputra', role: 'admin_kinerja', department: 'Kinerja' },
  { id: 'u004', name: 'D. Kurniawan', role: 'admin_efisiensi', department: 'Efisiensi' },
  { id: 'u005', name: 'S. Widodo', role: 'admin_energi_primer', department: 'Energi Primer' },
  { id: 'u006', name: 'T. Prabowo', role: 'admin_kimia_lab', department: 'Kimia' },
  { id: 'u007', name: 'S. Hidayat', role: 'gm', department: 'Manajemen' },
  { id: 'u008', name: 'R. Gunawan', role: 'manager_k3', department: 'K3' },
  { id: 'u009', name: 'I. Maulana', role: 'admin_k3', department: 'K3' },
  { id: 'u010', name: 'Y. Setiawan', role: 'officer_k3', department: 'K3' },
  { id: 'u011', name: 'F. Rahman', role: 'fire_system_pic', department: 'K3 / Fire System' },
  { id: 'u012', name: 'T. Nugroho', role: 'p2k3_user', department: 'P2K3' }
];
```

Update module ownership:

| Modul | Input Owner | Reviewer | Viewer |
|---|---|---|---|
| K3 & Fire Safety | `admin_k3`, `officer_k3`, `fire_system_pic`, `p2k3_user` | `manager_k3` / `manager_operasi` | semua viewer/manajemen |

Permission detail:

| Fitur | admin_k3 | officer_k3 | fire_system_pic | p2k3_user | manager_k3 | GM |
|---|---:|---:|---:|---:|---:|---:|
| View dashboard K3 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Input fire equipment | ✅ | ❌ | ✅ | ❌ | ✅ | ✅ |
| Input emergency equipment | ✅ | ❌ | ✅ | ❌ | ✅ | ✅ |
| Input patrol finding | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ |
| Upload evidence | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Submit laporan K3 | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Review/approve | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| Escalate issue | ✅ | ❌ | ✅ | ❌ | ✅ | ✅ |
| Close finding | ✅ | ❌ | ❌ | ❌ | ✅ | ✅ |

---

## U4. Modul Baru — K3 & Fire Safety

### U4.1 Tujuan Modul

Menjadi pusat monitoring keselamatan kerja, fire protection, emergency readiness, temuan patrol, upload evidence, dan tindak lanjut K3 untuk meeting pagi PLTU Tenayan.

Modul ini harus menjawab pertanyaan meeting pagi:

```text
Apakah fire system siap?
Equipment proteksi kebakaran mana yang not ready?
Adakah hydrant/APAR/pump/deluge/gas system critical?
Berapa nilai budaya K3 minggu ini?
Berapa temuan patrol open?
Berapa temuan open > 2 minggu?
Apakah upload IZAT compliant?
Apa temuan P2K3 terbaru?
Action K3 mana yang overdue?
```

### U4.2 Visual Direction

Tetap gunakan style yang sama dengan mockup:

```text
dark navy / charcoal
cyan/teal command center accent
green/amber/red status
compact enterprise dashboard
photo evidence grid
readiness matrix
fire safety risk panel
```

Jangan membuat modul K3 tampil seperti form safety biasa. Modul ini harus terasa seperti **Safety Command Center**.

### U4.3 Page Title

```text
K3 & Fire Safety
```

Subtitle:

```text
Fire system readiness, emergency equipment, patrol findings, and safety culture
```

### U4.4 Top KPI Cards

Top KPI harus berisi 8 card:

| KPI Card | Contoh Nilai | Keterangan |
|---|---:|---|
| K3 Culture Score | 99.1% | skor budaya K3 minggu berjalan |
| Fire System Readiness | 99.6% | ready vs total fire equipment |
| Total Fire Equipment | 758 | total APAR/hydrant/system yang dimonitor |
| Not Ready Equipment | 3 | item fire system tidak ready |
| Critical Emergency System | 1 | contoh: Inergen System not ready |
| Open Patrol Findings | 24 | total temuan patrol open |
| Open > 2 Weeks | 7 | temuan aging lebih dari 2 minggu |
| IZAT Upload Compliance | 96% | compliance upload IZAT |

Catatan:
- Angka di atas dummy awal untuk prototype.
- Saat backend aktif, angka dihitung dari database.

### U4.5 Panel Layout Utama

Gunakan grid layout 16:9 seperti modul lain.

#### Row 1

1. **Fire System Readiness Matrix**  
   Tabel equipment fire system dengan total, ready, not ready, readiness percentage.

2. **Critical Fire Defect Register**  
   Daftar area ketidaksiapan fire system, SR number, severity, PIC, due date, status.

3. **Safety Risk Summary**  
   Ringkasan risk: critical, high, medium, low, dan rekomendasi fokus meeting.

#### Row 2

1. **Emergency Equipment Readiness**  
   Card grid untuk Industrial Pool, Ambulance, Fire Truck, Diesel Fire Pump, Electric Fire Pump, Jockey Pump, Foam Blander Tank, PRV Deluge System, Inergen System, CO2 System.

2. **K3 Culture Trend**  
   Grafik nilai budaya K3 mingguan, termasuk M-49/M-50.

3. **Patrol Finding Aging**  
   Chart total open, open > 2 minggu, close this week, overdue.

#### Row 3

1. **Patrol Finding Table**  
   Daftar temuan K3/patrol.

2. **P2K3 Patrol Evidence Gallery**  
   Foto patrol P2K3 dengan tanggal, area, caption, uploader.

3. **Monitoring Upload IZAT**  
   Matrix/gauge compliance upload evidence IZAT per minggu/unit/area.

#### Bottom

1. **K3 Action Log**  
   Action K3 yang tersambung ke Action Plan Center.

2. **Fire System Map / Area Status**  
   Optional visual area grid: Garasi Alat Berat, ESP Unit 2, Coalyard B, Fire Pump House, CCR, Coal Yard, Turbine Building.

---

## U5. Data Awal dari Laporan K3 untuk Mock Data

Gunakan data dari dokumen K3 sebagai dummy seed awal.

### U5.1 Fire System Readiness Matrix

```ts
export const fireEquipmentSummary = [
  { equipmentType: 'APAR CO2 (3.2 KG)', total: 46, ready: 46, notReady: 0 },
  { equipmentType: 'APAR Powder (4.5 KG)', total: 463, ready: 463, notReady: 0 },
  { equipmentType: 'APAR Powder (5 KG)', total: 8, ready: 8, notReady: 0 },
  { equipmentType: 'APAB Foam (60 KG)', total: 5, ready: 5, notReady: 0 },
  { equipmentType: 'Hydrant Box Pillar', total: 31, ready: 30, notReady: 1 },
  { equipmentType: 'Hydrant Box Indoor & Outdoor', total: 170, ready: 169, notReady: 1 },
  { equipmentType: 'Hydrant Foam', total: 4, ready: 4, notReady: 0 },
  { equipmentType: 'Pilar Hydrant', total: 31, ready: 30, notReady: 1 }
];
```

Total:

```ts
const totalFireEquipment = 758;
const totalReadyFireEquipment = 755;
const totalNotReadyFireEquipment = 3;
const fireSystemReadinessPct = 99.60;
```

### U5.2 Critical Fire Defect Register

```ts
export const fireDefects = [
  {
    id: 'FIRE-2025-001',
    equipmentName: 'Hydrant Box Indoor & Outdoor',
    quantity: 1,
    area: 'Garasi Alat Berat (H-158)',
    defectDescription: 'Valve rusak',
    srNumber: 'H-158',
    severity: 'HIGH',
    status: 'OPEN',
    pic: 'Fire System PIC',
    dueDate: '2025-12-20'
  },
  {
    id: 'FIRE-2025-002',
    equipmentName: 'Hydrant Pilar Outdoor',
    quantity: 1,
    area: 'ESP Unit 2',
    defectDescription: 'Gate valve rusak / bocor, stok pillar kosong',
    srNumber: '641512',
    severity: 'HIGH',
    status: 'IN_PROGRESS',
    pic: 'Fire System PIC',
    dueDate: '2025-12-22'
  },
  {
    id: 'FIRE-2025-003',
    equipmentName: 'Box Hydrant Pilar',
    quantity: 1,
    area: 'Coalyard sisi B',
    defectDescription: 'Box pillar rusak',
    srNumber: '641511',
    severity: 'MEDIUM',
    status: 'OPEN',
    pic: 'Fire System PIC',
    dueDate: '2025-12-25'
  }
];
```

### U5.3 Emergency Equipment Readiness

```ts
export const emergencyEquipmentReadiness = [
  {
    equipmentName: 'Industrial Pool',
    capacity: '2 x 4000 mm',
    condition: 'READY',
    remarks: '2784/3900 dan 2740/3900'
  },
  {
    equipmentName: 'Ambulance',
    capacity: '-',
    condition: 'READY',
    lastServiceKm: '19590 KM after service 30 Agustus 2024',
    currentKm: '20972 KM'
  },
  {
    equipmentName: 'Fire Truck',
    capacity: 'Air 4000 L, Foam 400 L',
    condition: 'READY',
    remarks: ''
  },
  {
    equipmentName: 'Diesel Fire Pump',
    capacity: '600 m3/h',
    condition: 'READY',
    remarks: 'Manual'
  },
  {
    equipmentName: 'Electric Fire Pump',
    capacity: '600 m3/h',
    condition: 'READY',
    remarks: 'Manual'
  },
  {
    equipmentName: 'Jockey Pump',
    capacity: '18 m3/h',
    condition: 'READY',
    remarks: 'Setting 0.5 – 1.0 Mpa'
  },
  {
    equipmentName: 'Foam Blander Tank',
    capacity: '2 x 2000 L',
    condition: 'READY',
    remarks: 'Manual Start'
  },
  {
    equipmentName: 'PRV Deluge System',
    capacity: '11 Protection',
    condition: 'READY',
    remarks: 'Manual Release'
  },
  {
    equipmentName: 'Inergen System',
    capacity: '84 Tabung',
    condition: 'NOT_READY',
    remarks: 'Proses penormalan, estimasi pekerjaan 90 hari'
  },
  {
    equipmentName: 'CO2 System',
    capacity: '3805/5443 Kg',
    condition: 'READY',
    remarks: 'Manual'
  }
];
```

### U5.4 K3 Culture

```ts
export const k3CultureSummary = {
  week: 49,
  year: 2025,
  scorePct: 99.1,
  criteria: 'Dashboard baru',
  resetInfo: 'Rekap Dashboard di-reset pada M27 tahun 2025',
  unitCategory: 'Unit Batu Bara'
};
```

---

## U6. Input Forms Modul K3

### U6.1 Fire Equipment Input

```ts
type FireEquipmentStatusInput = {
  reportDate: string;
  equipmentType:
    | 'APAR_CO2_3_2_KG'
    | 'APAR_POWDER_4_5_KG'
    | 'APAR_POWDER_5_KG'
    | 'APAB_FOAM_60_KG'
    | 'HYDRANT_BOX_PILLAR'
    | 'HYDRANT_BOX_INDOOR_OUTDOOR'
    | 'HYDRANT_FOAM'
    | 'PILAR_HYDRANT'
    | 'OTHER';
  total: number;
  ready: number;
  notReady: number;
  remarks?: string;
  submittedBy: string;
};
```

Validation:

```text
total = ready + notReady
notReady > 0 wajib punya defect register minimal 1 item
```

### U6.2 Fire Defect Register Input

```ts
type FireDefectInput = {
  reportDate: string;
  equipmentName: string;
  equipmentType: string;
  quantity: number;
  area: string;
  defectDescription: string;
  srNumber?: string;
  sparepartStatus?: 'AVAILABLE' | 'NOT_AVAILABLE' | 'ORDERED' | 'UNKNOWN';
  severity: 'LOW' | 'MEDIUM' | 'HIGH' | 'CRITICAL';
  operationalRisk: string;
  picUserId: string;
  targetCompletionDate: string;
  status: 'OPEN' | 'IN_PROGRESS' | 'MONITORING' | 'CLOSED';
  evidenceBeforeUrl?: string;
  evidenceAfterUrl?: string;
  remarks?: string;
};
```

Rules:

```text
severity HIGH/CRITICAL wajib masuk Action Plan Center.
Jika ada SR number, tampilkan di table dan report.
Jika status CLOSED, wajib evidence after atau manager override.
```

### U6.3 Emergency Equipment Input

```ts
type EmergencyEquipmentInput = {
  reportDate: string;
  equipmentName:
    | 'INDUSTRIAL_POOL'
    | 'AMBULANCE'
    | 'FIRE_TRUCK'
    | 'DIESEL_FIRE_PUMP'
    | 'ELECTRIC_FIRE_PUMP'
    | 'JOCKEY_PUMP'
    | 'FOAM_BLANDER_TANK'
    | 'PRV_DELUGE_SYSTEM'
    | 'INERGEN_SYSTEM'
    | 'CO2_SYSTEM'
    | 'OTHER';
  capacity: string;
  actualConditionValue?: string;
  condition: 'READY' | 'NOT_READY' | 'MAINTENANCE' | 'WATCH';
  operationMode?: 'AUTO' | 'MANUAL' | 'MANUAL_RELEASE' | 'MANUAL_START' | 'UNKNOWN';
  lastServiceDate?: string;
  lastServiceKm?: number;
  currentKm?: number;
  remarks?: string;
  photoUrl?: string;
};
```

Rules:

```text
Emergency equipment NOT_READY langsung menjadi critical issue.
Inergen System NOT_READY harus tampil di Executive Dashboard dan Safety Risk Summary.
Ambulance overdue service harus warning.
Fire pump not ready harus CRITICAL.
```

### U6.4 K3 Culture Score Input

```ts
type K3CultureScoreInput = {
  reportDate: string;
  weekNumber: number;
  year: number;
  unitCategory: string;
  scorePct: number;
  criteria: string;
  resetInfo?: string;
  notes?: string;
};
```

Threshold awal:

```ts
score >= 98: GOOD
95 <= score < 98: WATCH
score < 95: DANGER
```

### U6.5 Patrol Finding Input

```ts
type PatrolFindingInput = {
  reportDate: string;
  findingDate: string;
  patrolType: 'K3_PATROL' | 'P2K3' | 'MANAGEMENT_WALKDOWN' | 'IZAT' | 'OTHER';
  area: string;
  findingCategory:
    | 'UNSAFE_ACT'
    | 'UNSAFE_CONDITION'
    | 'HOUSEKEEPING'
    | 'FIRE_SYSTEM'
    | 'PPE'
    | 'WORK_PERMIT'
    | 'ENVIRONMENT'
    | 'TRAFFIC'
    | 'OTHER';
  description: string;
  riskLevel: 'LOW' | 'MEDIUM' | 'HIGH' | 'CRITICAL';
  picUserId: string;
  dueDate: string;
  status: 'OPEN' | 'IN_PROGRESS' | 'MONITORING' | 'CLOSED';
  agingDays: number;
  evidenceBeforeUrl?: string;
  evidenceAfterUrl?: string;
  correctiveAction?: string;
  closeApprovalBy?: string;
};
```

Rules:

```text
agingDays > 14 dan status belum CLOSED → Open > 2 Weeks.
riskLevel HIGH/CRITICAL → masuk Top Issues dan Action Plan.
Patrol finding tidak bisa CLOSED tanpa correctiveAction dan evidenceAfter, kecuali override manager_k3.
```

### U6.6 IZAT Upload Monitoring Input

```ts
type IzatUploadMonitoringInput = {
  reportDate: string;
  weekNumber: number;
  area: string;
  requiredUploadCount: number;
  actualUploadCount: number;
  compliancePct: number;
  missingUploads: number;
  status: 'COMPLETE' | 'PARTIAL' | 'MISSING';
  remarks?: string;
};
```

### U6.7 P2K3 Patrol Input

```ts
type P2K3PatrolInput = {
  reportDate: string;
  patrolDate: string;
  area: string;
  participants: string[];
  summary: string;
  totalFindings: number;
  evidenceUrls: string[];
  status: 'DRAFT' | 'SUBMITTED' | 'APPROVED';
};
```

---

## U7. Database Schema Update

Tambahkan tabel berikut ke bagian database blueprint.

```sql
hse_fire_equipment_types
hse_fire_equipment_status
hse_fire_defects
hse_emergency_equipment_status
hse_k3_culture_scores
hse_patrol_findings
hse_patrol_evidence
hse_izat_upload_monitoring
hse_p2k3_patrols
hse_p2k3_patrol_evidence
hse_action_logs
```

### U7.1 `hse_fire_equipment_status`

```sql
create table hse_fire_equipment_status (
  id uuid primary key default gen_random_uuid(),
  report_date date not null,
  equipment_type text not null,
  total integer not null default 0,
  ready integer not null default 0,
  not_ready integer not null default 0,
  readiness_pct numeric(6,2),
  remarks text,
  submission_id uuid,
  created_by uuid,
  updated_by uuid,
  created_at timestamptz default now(),
  updated_at timestamptz default now(),
  constraint fire_equipment_total_check check (total = ready + not_ready)
);
```

### U7.2 `hse_fire_defects`

```sql
create table hse_fire_defects (
  id uuid primary key default gen_random_uuid(),
  report_date date not null,
  equipment_name text not null,
  equipment_type text,
  quantity integer default 1,
  area text not null,
  defect_description text not null,
  sr_number text,
  sparepart_status text,
  severity text not null check (severity in ('LOW','MEDIUM','HIGH','CRITICAL')),
  operational_risk text,
  pic_user_id uuid,
  target_completion_date date,
  status text not null check (status in ('OPEN','IN_PROGRESS','MONITORING','CLOSED')),
  action_item_id uuid,
  evidence_before_attachment_id uuid,
  evidence_after_attachment_id uuid,
  remarks text,
  created_by uuid,
  updated_by uuid,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);
```

### U7.3 `hse_emergency_equipment_status`

```sql
create table hse_emergency_equipment_status (
  id uuid primary key default gen_random_uuid(),
  report_date date not null,
  equipment_name text not null,
  capacity text,
  actual_condition_value text,
  condition text not null check (condition in ('READY','NOT_READY','MAINTENANCE','WATCH')),
  operation_mode text,
  last_service_date date,
  last_service_km integer,
  current_km integer,
  remarks text,
  photo_attachment_id uuid,
  action_item_id uuid,
  created_by uuid,
  updated_by uuid,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);
```

### U7.4 `hse_k3_culture_scores`

```sql
create table hse_k3_culture_scores (
  id uuid primary key default gen_random_uuid(),
  report_date date not null,
  week_number integer not null,
  year integer not null,
  unit_category text,
  score_pct numeric(6,2) not null,
  criteria text,
  reset_info text,
  notes text,
  created_by uuid,
  updated_by uuid,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);
```

### U7.5 `hse_patrol_findings`

```sql
create table hse_patrol_findings (
  id uuid primary key default gen_random_uuid(),
  report_date date not null,
  finding_date date not null,
  patrol_type text not null,
  area text not null,
  finding_category text not null,
  description text not null,
  risk_level text not null check (risk_level in ('LOW','MEDIUM','HIGH','CRITICAL')),
  pic_user_id uuid,
  due_date date,
  status text not null check (status in ('OPEN','IN_PROGRESS','MONITORING','CLOSED')),
  aging_days integer,
  corrective_action text,
  action_item_id uuid,
  close_approval_by uuid,
  closed_at timestamptz,
  created_by uuid,
  updated_by uuid,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);
```

### U7.6 `hse_patrol_evidence`

```sql
create table hse_patrol_evidence (
  id uuid primary key default gen_random_uuid(),
  patrol_finding_id uuid not null,
  attachment_id uuid not null,
  evidence_type text not null check (evidence_type in ('BEFORE','AFTER','SUPPORTING')),
  caption text,
  uploaded_by uuid,
  uploaded_at timestamptz default now()
);
```

### U7.7 `hse_izat_upload_monitoring`

```sql
create table hse_izat_upload_monitoring (
  id uuid primary key default gen_random_uuid(),
  report_date date not null,
  week_number integer not null,
  area text not null,
  required_upload_count integer default 0,
  actual_upload_count integer default 0,
  compliance_pct numeric(6,2),
  missing_uploads integer default 0,
  status text not null check (status in ('COMPLETE','PARTIAL','MISSING')),
  remarks text,
  created_by uuid,
  updated_by uuid,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);
```

### U7.8 `hse_p2k3_patrols`

```sql
create table hse_p2k3_patrols (
  id uuid primary key default gen_random_uuid(),
  report_date date not null,
  patrol_date date not null,
  area text not null,
  participants jsonb default '[]'::jsonb,
  summary text,
  total_findings integer default 0,
  status text not null check (status in ('DRAFT','SUBMITTED','APPROVED')),
  submitted_by uuid,
  approved_by uuid,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);
```

### U7.9 Update `action_items.sourceModule`

Tambahkan enum/source module:

```ts
sourceModule:
  DASHBOARD |
  READINESS |
  PERFORMANCE |
  EFFICIENCY |
  ENERGY_PRIMARY |
  CHEMICAL_WATER |
  K3_FIRE_SAFETY |
  REPORTS |
  MANUAL
```

---

## U8. API Endpoint Update

Tambahkan endpoint:

```text
GET/POST /api/k3-fire-safety
GET/POST /api/k3-fire-safety/fire-equipment
GET/POST /api/k3-fire-safety/fire-defects
GET/POST /api/k3-fire-safety/emergency-equipment
GET/POST /api/k3-fire-safety/k3-culture
GET/POST /api/k3-fire-safety/patrol-findings
GET/POST /api/k3-fire-safety/izat-upload
GET/POST /api/k3-fire-safety/p2k3-patrols
POST     /api/k3-fire-safety/submit
POST     /api/k3-fire-safety/approve
POST     /api/k3-fire-safety/return
POST     /api/k3-fire-safety/generate-actions
```

Endpoint dashboard aggregation:

```text
GET /api/k3-fire-safety/summary?date=YYYY-MM-DD&shift=PAGI
GET /api/k3-fire-safety/fire-readiness?date=YYYY-MM-DD
GET /api/k3-fire-safety/open-findings?date=YYYY-MM-DD
```

---

## U9. Update Executive Dashboard

Executive Morning Dashboard harus mengambil data ringkas dari modul K3.

### U9.1 Tambahan Status Strip

Jika ruang status strip memungkinkan, tambahkan:

```text
Safety Status: NORMAL / WATCH / CRITICAL
```

Jika tidak cukup, tampilkan di Plant Health & Readiness panel.

### U9.2 Tambahan Top Issues Logic

Top Issues Today sekarang harus memasukkan isu K3 jika memenuhi salah satu kondisi:

```text
fire equipment not ready > 0
emergency equipment critical not ready
fire pump not ready
Inergen System not ready
patrol finding high/critical open
patrol finding open > 14 days
IZAT compliance below threshold
K3 culture score below threshold
```

### U9.3 Tambahan Plant Health

Tambahkan mini metric di Plant Health & Readiness:

```text
Safety: 99%
Fire System: 99.6%
K3 Culture: 99.1%
Open K3 Findings: 24
```

### U9.4 Morning Brief Update

Morning Brief Generator harus memasukkan kalimat safety bila ada isu:

```text
Kondisi K3 secara umum baik dengan nilai budaya K3 99,1%, namun terdapat 3 item fire system not ready dan 1 emergency system critical yaitu Inergen System yang masih dalam proses penormalan.
```

Jika semua aman:

```text
Kondisi K3 dan emergency readiness dalam status aman. Tidak terdapat critical fire system defect baru.
```

---

## U10. Update Action Plan Center

### U10.1 Summary by Module

Tambahkan kategori/module:

```text
K3 & Fire Safety
```

### U10.2 Action Auto-Generation Rules

Action otomatis dibuat jika:

```text
Fire defect severity HIGH/CRITICAL
Emergency equipment condition NOT_READY
Patrol finding risk HIGH/CRITICAL
Patrol finding aging > 14 days
IZAT upload compliance < 90%
K3 culture score < 95%
```

Action item contoh:

```ts
{
  title: 'Normalisasi Inergen System',
  sourceModule: 'K3_FIRE_SAFETY',
  priority: 'HIGH',
  impactLevel: 'HIGH',
  rootCause: 'System not ready, proses penormalan estimasi 90 hari',
  currentAction: 'Monitoring progress penormalan dan availability sparepart/vendor',
  recommendedAction: 'Tetapkan weekly recovery milestone dan eskalasi jika tidak ada progress',
  picUserId: 'fire_system_pic',
  dueDate: '2026-03-12',
  status: 'IN_PROGRESS'
}
```

### U10.3 Issue Drill-down Support

Untuk sourceModule `K3_FIRE_SAFETY`, drill-down harus bisa menampilkan:

```text
Defect detail
Equipment condition
Area/location
SR number
Before/after evidence
Aging days
PIC
Corrective action
Close approval
```

---

## U11. Update Reports & Morning Brief

### U11.1 Report Types

Tambahkan report type:

```ts
ReportType =
  EXECUTIVE_DAILY_REPORT |
  UNIT_READINESS_REPORT |
  OPERATION_PERFORMANCE_REPORT |
  EFFICIENCY_REPORT |
  ENERGY_PRIMARY_REPORT |
  CHEMICAL_WATER_QUALITY_REPORT |
  K3_FIRE_SAFETY_REPORT |
  ACTION_PLAN_REPORT
```

### U11.2 Report Preview Card

Tambahkan card di `/reports`:

```text
K3 & Fire Safety Report
Fire system, patrol finding, emergency readiness, safety culture
```

### U11.3 Data Completeness Update

Data completeness sekarang menghitung:

```text
Readiness submitted?
Performance submitted?
Efficiency submitted?
Energy Primary submitted?
Chemical & Water submitted?
K3 & Fire Safety submitted?
Action update submitted?
Weather available?
Photos required uploaded?
```

Jika K3 belum submit, report status maksimal `IN_REVIEW`, kecuali manager override.

### U11.4 Executive Daily Report Tambahan Section

Executive Daily Report wajib punya section:

```text
Safety & Emergency Readiness Summary
```

Isi minimal:

```text
K3 Culture Score
Fire System Readiness
Not Ready Equipment
Critical Emergency Equipment
Open Patrol Findings
Open > 2 Weeks
Top K3 Actions
```

---

## U12. Update Upload Gambar

Tambahkan jenis upload untuk modul K3:

| Modul | Jenis gambar |
|---|---|
| K3 & Fire Safety | kondisi APAR/hydrant, fire pump, deluge, Inergen/CO2, patrol finding, before-after evidence, P2K3 patrol, screenshot IZAT |

Metadata tambahan untuk K3 evidence:

```ts
type HseEvidenceMetadata = {
  evidenceType: 'FIRE_DEFECT' | 'PATROL_BEFORE' | 'PATROL_AFTER' | 'P2K3_PATROL' | 'IZAT_UPLOAD' | 'EMERGENCY_EQUIPMENT';
  area: string;
  riskLevel?: 'LOW' | 'MEDIUM' | 'HIGH' | 'CRITICAL';
  equipmentName?: string;
  srNumber?: string;
  findingId?: string;
};
```

Rules:

```text
Patrol finding HIGH/CRITICAL wajib evidence before.
Close patrol finding wajib evidence after.
Fire defect HIGH/CRITICAL wajib evidence before.
Close fire defect wajib evidence after.
P2K3 patrol wajib minimal 1 foto evidence.
```

---

## U13. Update Visual Acceptance Criteria

Tambahkan ke bagian acceptance criteria:

### K3 & Fire Safety Visual

- [ ] Sidebar memiliki menu `K3 & Fire Safety` dengan icon safety/fire.
- [ ] Page `/k3-fire-safety` memakai AppShell yang sama dengan modul lain.
- [ ] Top KPI card berisi K3 Culture Score, Fire System Readiness, Total Fire Equipment, Not Ready Equipment, Critical Emergency System, Open Patrol Findings, Open > 2 Weeks, IZAT Upload Compliance.
- [ ] Ada Fire System Readiness Matrix.
- [ ] Ada Critical Fire Defect Register dengan SR Number.
- [ ] Ada Emergency Equipment Readiness Grid.
- [ ] Inergen System `NOT READY` tampil sebagai critical/warning card.
- [ ] Ada K3 Culture Trend.
- [ ] Ada Patrol Finding Aging chart.
- [ ] Ada P2K3 Patrol Evidence Gallery.
- [ ] Ada Monitoring Upload IZAT.
- [ ] Ada K3 Action Log yang tersambung secara visual ke Action Plan Center.
- [ ] Semua status menggunakan warna konsisten: READY hijau, WATCH amber, NOT READY merah, OPEN cyan/blue, IN PROGRESS amber.
- [ ] Tidak ada card putih / style keluar dari command center.

---

## U14. Update GSD Phase Plan

Tambahkan phase baru setelah Chemical & Water Quality dan sebelum Action Plan Center.

### Phase 10 — K3 & Fire Safety

Goal:
- Build `/k3-fire-safety` static page menggunakan dummy data dari laporan K3.

Deliverables:
- `app/k3-fire-safety/page.tsx`
- `lib/mock-data/hse-fire-safety.ts`
- `components/hse/*`

Acceptance:
- K3 module visually matches command center style.
- Fire readiness matrix visible.
- Fire defect register visible.
- Emergency equipment grid visible.
- Inergen System not ready clearly visible.
- K3 culture score 99.1% visible.
- Patrol finding aging and open > 2 weeks panel visible.
- P2K3 evidence gallery visible.
- IZAT upload compliance panel visible.
- K3 actions appear and can be routed to Action Plan mock.

Update phase numbering setelahnya:

```text
Phase 10 — K3 & Fire Safety
Phase 11 — Action Plan Center
Phase 12 — Reports & Morning Brief
Phase 13 — Forms & Auth Mock
Phase 14 — Backend Integration
```

---

## U15. Update Codex/GSD Project Prompt

Ganti daftar modul di project prompt menjadi:

```text
Modul yang harus ada:
1. Executive Morning Dashboard
2. Unit Readiness
3. Operation Performance
4. Efficiency Monitoring
5. Energy Primary
6. Chemical & Water Quality
7. K3 & Fire Safety
8. Action Plan Center
9. Reports & Morning Brief
10. Admin/Master Data later
```

Tambahkan instruksi:

```text
Tambahkan modul K3 & Fire Safety berdasarkan laporan K3 meeting pagi. Modul ini harus memonitor fire system readiness, area ketidaksiapan hydrant/fire equipment, emergency equipment readiness, nilai budaya K3, patrol findings, temuan open > 2 minggu, monitoring upload IZAT, dan evidence patrol P2K3. Modul ini harus memiliki upload gambar untuk evidence before/after, status equipment, dan patrol documentation. Semua issue K3 critical/high harus otomatis masuk Action Plan Center.
```

---

## U16. First Codex Task untuk Update Ini

Jika project frontend sudah berjalan, berikan task ini ke Codex/GSD:

```text
Implement an update to the existing Tenayan Command Center blueprint by adding a new module: K3 & Fire Safety. Do not change the visual design system. Add the sidebar item after Chemical & Water Quality and before Action Plan. Create /k3-fire-safety static page using the same AppShell, TopHeader, StatusStrip, KpiCard, DashboardCard, StatusBadge, DataTable, PhotoGrid, GaugeCard, and chart components. Use realistic dummy data based on the K3 report: fire equipment readiness, 3 not-ready fire system items, emergency equipment readiness including Inergen System NOT_READY, K3 Culture Score 99.1%, patrol finding aging, IZAT upload compliance, and P2K3 evidence gallery. Also update Action Plan mock data to include sourceModule K3_FIRE_SAFETY and add K3 & Fire Safety report card in Reports page. Keep the UI dark premium command center and screenshot-compare with the existing mockups.
```

---

## U17. Definition of Done untuk Update K3

Update dianggap selesai jika:

1. Menu `K3 & Fire Safety` muncul di sidebar.
2. Route `/k3-fire-safety` tersedia.
3. Page menggunakan style command center yang sama.
4. Fire System Readiness Matrix tampil.
5. Critical Fire Defect Register tampil dengan 3 defect awal.
6. Emergency Equipment Readiness tampil dengan Inergen System `NOT READY`.
7. K3 Culture Score 99.1% tampil.
8. Patrol Finding Aging dan Open > 2 Weeks tampil.
9. Monitoring Upload IZAT tampil.
10. P2K3 Patrol Evidence Gallery tampil.
11. K3 action item muncul di Action Plan Center.
12. K3 & Fire Safety Report muncul di Reports module.
13. Data completeness report memasukkan K3.
14. Upload evidence K3 didukung di struktur UI/forms.
15. Backend schema sudah menambahkan tabel HSE/K3.
16. Role `manager_k3`, `admin_k3`, `officer_k3`, `fire_system_pic`, `p2k3_user` tersedia.

---

## U18. Catatan Strategis

Modul ini penting karena Tenayan Command Center tidak boleh hanya memonitor performa operasi dan energi primer. Meeting pagi PLTU juga membutuhkan visibilitas terhadap safety, fire protection, emergency readiness, temuan patrol, dan tindak lanjut K3.

Dengan update ini, aplikasi menjadi lebih lengkap:

```text
Operasi + Kinerja + Efisiensi + Energi Primer + Kimia/Water + K3 + Action Plan + Reports
```

Ini memperkuat posisi aplikasi sebagai:

```text
Daily Operation Command Center PLTU Tenayan
```

bukan sekadar dashboard data.
