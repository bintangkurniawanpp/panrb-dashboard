# TASKS — Prototype Dashboard SIAP Digital (ARIS MashZone)

Status per 2026-10-19 (pekerjaan di ARIS **dihentikan sementara atas permintaan user**). Seluruh data adalah **DATA SIMULASI / PROTOTYPE**.

| Fase | Status |
|---|---|
| A — Source Inspection & Data Validation | DONE |
| B — ARIS UI Inspection & Data Source Integration | DONE (via raw GitHub) |
| C — Data Feed Implementation | DONE |
| D — Dashboard Implementation | PARTIAL (Tab 1 & 2 berisi; Tab 3 kosong) |
| E — Visual Design | PARTIAL (layout Tab 1 & banner simulasi belum) |
| F — Functional Testing & Validation | PARTIAL (Tab 1 & 2 PASS; view mode & Tab 3 belum diuji) |
| Deliverables dokumen | DONE untuk kondisi saat ini |

## Phase A — DONE
- [x] Folder, PDF B-513, README, 9 CSV dibaca
- [x] Kualitas data diperiksa (baris, encoding, delimiter, kosong, duplikasi, relasi) — tidak ada isu
- [x] 9 KPI dihitung ulang — 06 (9/9) dan 07 (72/72) cocok
- [x] Tidak ada perbaikan CSV yang diperlukan
- [x] Label instansi diganti ke 8 Instansi Pusat riil (permintaan user); nilai & ID tetap; validasi ulang PASS
- [ ] Push CSV baru ke GitHub agar ARIS menampilkan nama baru (menunggu persetujuan user)

## Phase B — DONE
- [x] ARIS Document Storage — tidak berfungsi di instalasi ini (tab kosong, i18n rusak, JS exception)
- [x] URL alias `URL SPBE Arsitektur` — `Read timed out`
- [x] Google Drive — berhasil sementara, lalu `403 Forbidden` setelah ±12 unduhan
- [x] **Raw GitHub** `bintangkurniawanpp/panrb-dashboard` (disetujui user) — berhasil; 9 CSV identik dengan lokal

## Phase C — DONE
- [x] DEMO_DF_SIAP_Aplikasi — `jumlah`, `pct_standar`, `pct_portal`, `pct_penghubung`, `pct_layak`
- [x] DEMO_DF_SIAP_Layanan — `jumlah`, `pct_melalui_aplikasi`
- [x] DEMO_DF_SIAP_BagiPakai — `jumlah`, `pct_dimanfaatkan`
- [x] DEMO_DF_SIAP_Repositori — `jumlah`, `pct_terbuka`, `pct_tertutup`
- [x] DEMO_DF_SIAP_Audit — `jumlah`, `pct_audit_memenuhi`
- [x] DEMO_DF_SIAP_KPI_Ringkasan, DEMO_DF_SIAP_KPI_Instansi (pembanding)

## Phase D/E — PARTIAL
- [x] Dashboard "DEMO — SIAP Digital | Dashboard Arsitektur Aplikasi Pemerintah Digital" tersimpan
- [x] 3 tab dinamai; "Show tabs as" = Tabs
- [x] Tab 1: Pie KPI-01 + 8 tile KPI-02…09 (filter penyebut untuk KPI-05/06/08/09)
- [x] Tab 2: bar per instansi KPI-02, KPI-03, KPI-07
- [ ] Tab 2: bar per instansi KPI-04 — dikonfigurasi, **belum disimpan**
- [ ] Tab 2: pie koneksi Portal Ya/Tidak
- [ ] Tab 3: bar per instansi KPI-05, 06, 08, 09 + pie jenis repositori aktif
- [ ] Rapikan layout Tab 1 (celah, tinggi tile, urutan)
- [ ] Banner "DATA SIMULASI / PROTOTYPE"
- [ ] Tampilkan cakupan penilaian KPI-08 (87,5%) dan cakupan audit KPI-09 (80,0%)

## Phase F — PARTIAL
- [x] 9 KPI agregat PASS; 32 nilai per instansi Tab 2 PASS
- [x] Persistensi: simpan + reload (parsial) PASS
- [ ] Buka ulang di mode view, uji navigasi tab, screenshot final

## Deliverables
- [x] TASKS.md, KPI_DEFINITIONS.md, DATAFEED_CONFIGURATION.md, VALIDATION_REPORT.md, IMPLEMENTATION_LOG.md
- [x] screenshots/ (4 file, mode edit)
