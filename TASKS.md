# TASKS — Prototype Dashboard SIAP Digital (ARIS MashZone)

Status per 2026-10-19. Seluruh data adalah **DATA SIMULASI / PROTOTYPE**.

## Phase A — Source Inspection & Data Validation — DONE
- [x] Folder `D:\NUVISION\kemenpan\dashboard` dapat dibaca (PDF, README, 9 CSV, ZIP)
- [x] PDF B-513/DJTPD.4/AI.01.01/08/2026 dibaca; Bagian 3 (9 kebutuhan data analitik) dicocokkan ke KPI-01..09
- [x] Header, jumlah baris, delimiter (koma), encoding (UTF-8), nilai kosong, duplikasi diperiksa — tidak ada duplikasi ID
- [x] Relasi antar-dataset diperiksa (aplikasi_id, instansi_id) — tidak ada referensi yatim / lintas instansi
- [x] 9 KPI dihitung ulang dari CSV detail (`scratchpad/recompute.py`) — 06 dan 07 cocok 100% (0 FAIL)
- [x] Tidak ada perbaikan CSV yang diperlukan; file asli tidak diubah

## Phase B — ARIS UI Inspection & Data Source Integration — DONE
- [x] ARIS Document Storage: tab "Documents" kosong (label i18n rusak `absDataProviderComponent.componentInfo.DBI`), panel "ARIS documents" di Data Feed Editor loading tanpa akhir + JS exception → **tidak dapat dipakai**
- [x] URL alias tersedia: `URL SPBE Arsitektur` / `URL SPBE` → server ARIS sendiri; uji Calculate: `JBPMZCE0389 ... Read timed out`
- [x] Sumber yang disetujui user: folder Google Drive bersama
  - `drive.usercontent.google.com/download?...` → `JBPMZCE0350 ... Status 500` (GAGAL)
  - `drive.google.com/uc?export=download&id=<ID>` → **BERHASIL** (kolom & baris terbaca)
- [ ] `07_kpi_per_instansi.csv` **tidak ada** di folder Drive → nilai per instansi dihitung dari feed detail
- [!] **BLOCKER (13:40 WIB)**: setelah ±12 unduhan berhasil, Google Drive membalas `403 Forbidden` untuk semua file
  dari IP server ARIS (`drive.google.com/uc` dan `docs.google.com/uc`). Drive membatasi unduhan otomatis →
  tidak andal sebagai sumber data feed. Menunggu keputusan user soal hosting CSV alternatif.

## Phase C — Data Feed Implementation — IN PROGRESS
- [x] DEMO_DF_SIAP_KPI_Ringkasan (06)
- [x] DEMO_DF_SIAP_Aplikasi (01)
- [x] DEMO_DF_SIAP_Layanan (02)
- [x] DEMO_DF_SIAP_BagiPakai (03)
- [x] DEMO_DF_SIAP_Repositori (04)
- [x] DEMO_DF_SIAP_Audit (05)
- [ ] Kolom numerik untuk widget (widget hanya menerima Measure numerik): `jumlah`=1 dan flag 100/0
      (AVERAGE flag = persentase) via Insert column + Conditional replace — BLOCKED oleh 403 Drive
      (feed Audit: Insert column sudah ditambahkan di editor, BELUM disimpan)

## Phase D/E — Dashboard Implementation & Visual Design — PARTIAL
- [x] Dashboard "DEMO — SIAP Digital | Dashboard Arsitektur Aplikasi Pemerintah Digital" dibuat & disimpan
- [ ] Pie KPI-01 (feed Aplikasi, partisi `menerapkan_standar`) — menunggu kolom numerik `jumlah`
## Phase F — Validation — PENDING
## Deliverables — PENDING
