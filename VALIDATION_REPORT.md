# VALIDATION_REPORT — Dashboard SIAP Digital (Prototype)

Status per 2026-10-19, **sebelum** pekerjaan dihentikan sementara atas permintaan user. ARIS tidak diubah setelah laporan ini ditulis.

- **Expected** = perhitungan independen dari CSV detail (skrip Python, membaca 01–05) dan dicocokkan ke 06/07.
- **Actual** = nilai yang ditampilkan widget ARIS (dibaca dari layar editor, lihat `screenshots/`).

### Pembaruan nama instansi (2026-10-19, sesudah pengujian ARIS)

Atas permintaan user, label "Instansi Simulasi A–H" di CSV 01–05 dan 07 diganti dengan 8 nama Instansi Pusat dari `instansi-pusat.xls`. `instansi_id` dan seluruh nilai **tidak berubah**. Huruf A–H pada tabel di bawah setara dengan:

| Huruf | ID | Instansi |
|---|---|---|
| A | INS01 | Kementerian Pendayagunaan Aparatur Negara dan Reformasi Birokrasi |
| B | INS02 | Kementerian Komunikasi dan Digital |
| C | INS03 | Kementerian Dalam Negeri |
| D | INS04 | Kementerian Keuangan |
| E | INS05 | Kementerian Kesehatan |
| F | INS06 | Badan Kepegawaian Negara |
| G | INS07 | Badan Pusat Statistik |
| H | INS08 | Arsip Nasional Republik Indonesia |

Validasi ulang setelah penggantian: 9/9 KPI di 06 dan 72/72 baris di 07 tetap PASS; tidak ada duplikasi; setiap `instansi_id` memetakan tepat ke satu nama di semua file; ke-8 nama ada persis di `instansi-pusat.xls`.

**Catatan:** nilai "Actual ARIS" dan `screenshots/` diambil **sebelum** penggantian, sehingga label bar chart masih "Instansi Simulasi A–H". Nama baru baru akan tampil di ARIS setelah CSV di-push ke GitHub dan cache feed (refresh 12 jam) diperbarui.

## 1. Validasi dataset (Phase A)

| Pemeriksaan | Hasil | Status |
|---|---|---|
| Jumlah baris 01–07 | 40, 64, 64, 48, 40, 9, 72 (sesuai README) | PASS |
| Encoding / delimiter | UTF-8, koma, header baris 1 | PASS |
| Duplikasi ID (`aplikasi_id`, `layanan_id`, instansi×aplikasi, `repo_id`, `audit_id`, audit per aplikasi) | tidak ada | PASS |
| Referensi `aplikasi_id` di 02/04/05 ke 01 | semua ada; tidak ada lintas instansi | PASS |
| Layanan `melalui_aplikasi=Ya` tanpa `aplikasi_id` / `Tidak` dengan `aplikasi_id` | 0 / 0 | PASS |
| Aplikasi di 03 = aplikasi `bagi_pakai_nasional=Ya` di 01 | 8 = 8 (APP001, 006, 011, 016, 021, 026, 031, 036) | PASS |
| Audit `Selesai` tanpa hasil / bukan `Selesai` tapi ada hasil | 0 / 0 | PASS |
| 06 vs perhitungan ulang (9 KPI: nilai, pembilang, penyebut) | 9/9 cocok | PASS |
| 07 vs perhitungan ulang (72 baris) | 72/72 cocok | PASS |
| Roll-up 07 → 06 dengan `ΣPembilang/ΣPenyebut` | cocok untuk 9 KPI | PASS |
| Rata-rata naif nilai 07 vs 06 | berbeda untuk KPI-05 (30,8 vs 31,0), KPI-06 (59,6 vs 59,5), KPI-09 (78,3 vs 78,1) | Dicatat: **jangan** rata-ratakan persentase antar instansi |

Tidak ada CSV yang perlu diperbaiki; file asli tidak diubah.

## 2. KPI agregat — Tab 1 (Ringkasan Eksekutif)

| KPI | Sumber (feed · measure · agregasi · filter) | Expected | Actual ARIS | Status |
|---|---|---|---|---|
| KPI-01 | Aplikasi · `jumlah` · SUM · partisi `menerapkan_standar` | Memenuhi 29 (72,5%), Belum 11 (27,5%) | Memenuhi 29 (72.5%), Belum Memenuhi 11 (27.5%) | **PASS** |
| KPI-02 | Layanan · `pct_melalui_aplikasi` · AVG | 71,9% (46/64) | 71.9 % | **PASS** |
| KPI-03 | BagiPakai · `pct_dimanfaatkan` · AVG | 65,6% (42/64) | 65.6 % | **PASS** |
| KPI-04 | Aplikasi · `pct_portal` · AVG | 75,0% (30/40) | 75.0 % | **PASS** |
| KPI-05 | Repositori · `pct_terbuka` · AVG · `digunakan=Ya` | 31,0% (13/42) | 31.0 % | **PASS** |
| KPI-06 | Repositori · `pct_tertutup` · AVG · `digunakan=Ya` | 59,5% (25/42) | 59.5 % (sebelum format: 59.524) | **PASS** |
| KPI-07 | Aplikasi · `pct_penghubung` · AVG | 67,5% (27/40) | 67.5 % | **PASS** |
| KPI-08 | Aplikasi · `pct_layak` · AVG · block `Belum Dinilai` | 80,0% (28/35) | 80.0 % (tanpa filter 70.0 = 28/40, sudah dikoreksi) | **PASS** |
| KPI-09 | Audit · `pct_audit_memenuhi` · AVG · `status_audit=Selesai` | 78,1% (25/32) | 78.1 % | **PASS** |

**Selisih yang muncul selama implementasi dan perbaikannya**

| Temuan | Penyebab | Perbaikan |
|---|---|---|
| Pie KPI-01 awalnya 50/50 | Agregasi default widget = AVG atas kolom konstanta 1 | Diubah ke **SUM** |
| KPI-02 tampil hijau dengan ikon tren | Threshold bawaan widget KPI (<10 merah, <20 kuning, ≥20 hijau) | Threshold dihapus karena target belum ditetapkan dokumen sumber |
| KPI-08 = 70,0% | Penyebut masih seluruh aplikasi | Filter Block `Belum Dinilai` → 80,0% |
| KPI-02 = 71.875 | Presisi Auto (3 desimal) | Presisi 1 desimal + sufiks `%` |

## 3. KPI per instansi — Tab 2 (Penyelenggaraan & Integrasi)

Expected dari `07_kpi_per_instansi.csv` (pembilang/penyebut di kurung); actual dari bar chart ARIS. Agregasi = AVG flag per `instansi` (dihitung dari baris, bukan dari persentase).

| Instansi | KPI-02 exp | KPI-02 ARIS | KPI-03 exp | KPI-03 ARIS | KPI-07 exp | KPI-07 ARIS | KPI-04 exp | KPI-04 ARIS* |
|---|---|---|---|---|---|---|---|---|
| A | 87,5 (7/8) | 87.5 | 62,5 (5/8) | 62.5 | 80,0 (4/5) | 80.0 | 80,0 (4/5) | 80.0 |
| B | 62,5 (5/8) | 62.5 | 62,5 (5/8) | 62.5 | 60,0 (3/5) | 60.0 | 80,0 (4/5) | 80.0 |
| C | 75,0 (6/8) | 75.0 | 75,0 (6/8) | 75.0 | 60,0 (3/5) | 60.0 | 80,0 (4/5) | 80.0 |
| D | 62,5 (5/8) | 62.5 | 50,0 (4/8) | 50.0 | 80,0 (4/5) | 80.0 | 60,0 (3/5) | 60.0 |
| E | 62,5 (5/8) | 62.5 | 75,0 (6/8) | 75.0 | 60,0 (3/5) | 60.0 | 80,0 (4/5) | 80.0 |
| F | 87,5 (7/8) | 87.5 | 62,5 (5/8) | 62.5 | 60,0 (3/5) | 60.0 | 80,0 (4/5) | 80.0 |
| G | 62,5 (5/8) | 62.5 | 62,5 (5/8) | 62.5 | 80,0 (4/5) | 80.0 | 80,0 (4/5) | 80.0 |
| H | 75,0 (6/8) | 75.0 | 75,0 (6/8) | 75.0 | 60,0 (3/5) | 60.0 | 60,0 (3/5) | 60.0 |
| **Status** | | **PASS** | | **PASS** | | **PASS** | | **PASS (nilai), belum tersimpan** |

\* Bar chart KPI-04 sudah dikonfigurasi dan nilainya benar, tetapi pekerjaan dihentikan sebelum *Save* terakhir. Widget ini masih berupa perubahan lokal di editor dan belum terkonfirmasi tersimpan di server.

## 4. Tab 3 (Repositori, Kelaikan & Audit) — BELUM DIUJI

Tab sudah dibuat dan diberi nama, tetapi **belum berisi widget**. Expected yang harus dicapai nanti:

| Instansi | KPI-05 | KPI-06 | KPI-08 | KPI-09 |
|---|---|---|---|---|
| A | 33,3 (2/6) | 66,7 (4/6) | 80,0 (4/5) | 80,0 (4/5) |
| B | 40,0 (2/5) | 40,0 (2/5) | 75,0 (3/4) | 66,7 (2/3) |
| C | 40,0 (2/5) | 60,0 (3/5) | 80,0 (4/5) | 75,0 (3/4) |
| D | 20,0 (1/5) | 60,0 (3/5) | 75,0 (3/4) | 75,0 (3/4) |
| E | 40,0 (2/5) | 60,0 (3/5) | 75,0 (3/4) | 75,0 (3/4) |
| F | 20,0 (1/5) | 60,0 (3/5) | 80,0 (4/5) | 100,0 (3/3) |
| G | 20,0 (1/5) | 80,0 (4/5) | 75,0 (3/4) | 80,0 (4/5) |
| H | 33,3 (2/6) | 50,0 (3/6) | 100,0 (4/4) | 75,0 (3/4) |

## 5. Validasi visual

| Pemeriksaan | Hasil | Status |
|---|---|---|
| Semua widget Tab 1 & 2 memuat data, tidak ada "Not configured yet" | Ya (12 widget tersimpan: 9 di Tab 1, 3 di Tab 2; ditambah bar KPI-04 yang belum tersimpan) | PASS |
| Error data feed | Tidak ada pada widget terpasang | PASS |
| Pie KPI-01 menampilkan nilai absolut + % | Ya, legend aktif, tanpa efek 3D | PASS |
| Bar chart memakai dimensi `instansi` | Ya, 8 bar per chart, label nilai `%` | PASS |
| Warna navy/biru | Bar & angka KPI navy (MONOBLUE / #0B2E59). Pie memakai palet BLUERED karena ARIS tidak menyediakan palet kustom per kategori | PASS dengan catatan |
| Layout Tab 1 | **Berantakan**: setelah bar tab membungkus/berubah lebar, tile tersusun ulang dengan celah kosong; tile terlalu tinggi | **FAIL** (perlu dirapikan) |
| Penanda "DATA SIMULASI / PROTOTYPE" | Belum ditambahkan (widget Title/Rich text tersedia) | **FAIL** (belum dikerjakan) |
| Cakupan penilaian KPI-08 (87,5%) dan cakupan audit KPI-09 (80,0%) | Belum ditampilkan | Belum dikerjakan |
| Judul berbahasa Indonesia | Ya, untuk semua widget dan tab | PASS |

## 6. Validasi persistensi

| Uji | Hasil | Status |
|---|---|---|
| Simpan dashboard → muat ulang halaman (setelah KPI-07 tile) | Pie KPI-01 dan KPI-02/03/04/07 tampil kembali dengan nilai sama | PASS |
| Feed tersimpan dan muncul di daftar Data feeds | 7 feed `DEMO_DF_SIAP_*` | PASS |
| Buka ulang dashboard final dari daftar Dashboards (mode view) | Belum dilakukan | **Belum diuji** |
| Navigasi tab di mode view | "Show tabs as" sudah diubah ke **Tabs**, tetapi belum dicoba di mode view | **Belum diuji** |

## 7. Ringkasan

- **9/9 KPI agregat PASS** terhadap perhitungan independen.
- **32/32 nilai per instansi di Tab 2 PASS** (4 chart × 8 instansi), satu chart belum tersimpan.
- Belum selesai: Tab 3, layout Tab 1, banner simulasi, cakupan KPI-08/09, uji buka ulang di mode view.
