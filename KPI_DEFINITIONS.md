# KPI_DEFINITIONS — Dashboard SIAP Digital (Prototype)

> **DATA SIMULASI / PROTOTYPE.** Definisi di bawah adalah **asumsi prototype**. Surat B-513/DJTPD.4/AI.01.01/08/2026 (24 Agustus 2026), Lampiran Bagian 3, hanya memuat teks sembilan kebutuhan data analitik dan menyebutnya sebagai **usulan awal**. Surat tidak menetapkan rumus, penyebut, populasi, periode, atau sumber bukti. Semua definisi di sini perlu disahkan oleh pemilik indikator.

Status per 2026-10-19. Nilai dihitung dari CSV detail (periode `2026-10`, 8 instansi). Nama instansi adalah Instansi Pusat riil (INS01 Kementerian PANRB, INS02 Kementerian Komunikasi dan Digital, INS03 Kementerian Dalam Negeri, INS04 Kementerian Keuangan, INS05 Kementerian Kesehatan, INS06 Badan Kepegawaian Negara, INS07 Badan Pusat Statistik, INS08 Arsip Nasional Republik Indonesia) yang dipakai **hanya sebagai label**; seluruh nilai tetap simulasi.

## Prinsip perhitungan yang dipakai di ARIS

1. **Satu baris = satu unit analisis.** Tidak ada duplikasi ID di dataset (sudah dicek: `aplikasi_id`, `layanan_id`, pasangan `instansi_id`×`aplikasi_id`, `repo_id`, `audit_id`).
2. **Persentase = AVERAGE flag 100/0.** Setiap feed detail punya kolom flag `pct_*` bernilai 100 bila baris memenuhi kriteria dan 0 bila tidak. Rata-rata flag atas himpunan penyebut sama dengan `100 × pembilang / penyebut`. Ini berlaku untuk total maupun per instansi (rata-rata dihitung dari baris, **bukan** rata-rata dari persentase instansi).
3. **Jumlah = SUM kolom `jumlah`** (konstanta 1 per baris). Ini dipakai untuk KPI-01.
4. **Penyebut dibatasi lewat filter widget** (operator *Filter rows* di Assign data) bila definisinya tidak mencakup seluruh baris (KPI-05, 06, 08, 09).
5. **Nilai tidak berasal dari angka statis.** `06_kpi_ringkasan.csv` dan `07_kpi_per_instansi.csv` hanya dipakai sebagai pembanding validasi, bukan sebagai sumber widget.

## Ringkasan

| KPI | Indikator (teks surat, disingkat) | Satuan | Pembilang | Penyebut | Hasil |
|---|---|---|---|---|---|
| KPI-01 | Jumlah aplikasi yang menerapkan standar Aplikasi Pemdi | Jumlah | 29 | 40 (konteks) | **29 aplikasi** (72,5%) |
| KPI-02 | % layanan digital diselenggarakan melalui aplikasi | % | 46 | 64 | **71,9%** |
| KPI-03 | % aplikasi bagi pakai nasional diselenggarakan & dimanfaatkan instansi | % | 42 | 64 | **65,6%** |
| KPI-04 | % aplikasi terhubung dengan Portal | % | 30 | 40 | **75,0%** |
| KPI-05 | % repositori kode sumber terbuka yang digunakan | % | 13 | 42 | **31,0%** |
| KPI-06 | % repositori kode sumber tertutup yang digunakan | % | 25 | 42 | **59,5%** |
| KPI-07 | % aplikasi memanfaatkan Sistem Penghubung | % | 27 | 40 | **67,5%** |
| KPI-08 | % kelaikan aplikasi | % | 28 | 35 | **80,0%** |
| KPI-09 | % hasil Audit Teknologi terhadap aplikasi | % | 25 | 32 | **78,1%** |

---

## KPI-01 — Jumlah aplikasi yang telah menerapkan standar Aplikasi Pemerintah Digital

| Aspek | Definisi |
|---|---|
| Teks surat | Jumlah Aplikasi Pemerintah Digital yang telah menerapkan standar Aplikasi Pemerintah Digital |
| Unit analisis | Aplikasi (`01_aplikasi.csv`, 40 baris, 1 baris per `aplikasi_id`) |
| Pembilang | Aplikasi dengan `menerapkan_standar = "Memenuhi"` → **29** |
| Penyebut | Tidak diperlukan oleh indikator resmi (satuan Jumlah). Total 40 hanya ditampilkan sebagai konteks komposisi. |
| Rumus | `COUNT(aplikasi | menerapkan_standar = Memenuhi)`, diimplementasikan sebagai `SUM(jumlah)` dipartisi `menerapkan_standar` |
| Data feed / kolom | `DEMO_DF_SIAP_Aplikasi` → `menerapkan_standar`, `jumlah` |
| Filter / agregasi | Tanpa filter; agregasi **SUM** (bukan AVG) |
| Nilai kosong / tak diketahui | Tidak ada nilai kosong. Nilai selain `Memenuhi` (yaitu `Belum Memenuhi`) masuk kategori "Belum Memenuhi". |
| Hasil | Memenuhi **29** (72,5%), Belum Memenuhi **11** (27,5%) |
| Widget | Tab 1 · Pie chart "KPI-01 · Jumlah Aplikasi Menerapkan Standar". Label menampilkan nilai absolut dan persentase. |
| Asumsi / isu | Kriteria "menerapkan standar" (Standar Penyelenggaraan, Internasional, Kapabilitas, Regulasi lain, hal. 3 surat) dan mekanisme pembuktiannya belum ditetapkan. Indikator tetap disajikan sebagai **jumlah**; persentase hanya pelengkap komposisi. |

## KPI-02 — Persentase layanan digital yang diselenggarakan melalui aplikasi

| Aspek | Definisi |
|---|---|
| Unit analisis | Layanan digital (`02_layanan.csv`, 64 baris) |
| Pembilang | `melalui_aplikasi = "Ya"` → 46 |
| Penyebut | Seluruh layanan digital tercatat → 64 |
| Rumus | `100 × 46 / 64 = 71,875 → 71,9%` |
| Data feed / kolom | `DEMO_DF_SIAP_Layanan` → `pct_melalui_aplikasi` (100 bila `melalui_aplikasi = Ya`) |
| Filter / agregasi | Tanpa filter; **AVG** `pct_melalui_aplikasi` |
| Nilai kosong | `aplikasi_id` kosong pada 18 layanan `melalui_aplikasi = Tidak`; konsisten (tidak ada layanan "Ya" tanpa `aplikasi_id`). |
| Widget | Tab 1 · KPI tile; Tab 2 · Bar chart per instansi |
| Asumsi / isu | Lingkup "Layanan Digital Pemerintah" (instansional + tematik) dan arti "diselenggarakan melalui" perlu disepakati. |

## KPI-03 — Persentase aplikasi bagi pakai nasional yang diselenggarakan dan dimanfaatkan instansi

| Aspek | Definisi |
|---|---|
| Unit analisis | **Pasangan instansi × aplikasi bagi pakai nasional yang ditargetkan** (`03_pemanfaatan_bagi_pakai.csv`, 64 baris = 8 instansi × 8 aplikasi) |
| Pembilang | Pasangan dengan `dimanfaatkan = "Ya"` → 42 |
| Penyebut | Pasangan dengan `aplikasi_bagi_pakai_tersedia = Ya` dan `instansi_ditargetkan = Ya` → 64 (di data simulasi semua baris memenuhi) |
| Rumus | `100 × 42 / 64 = 65,6%` |
| Data feed / kolom | `DEMO_DF_SIAP_BagiPakai` → `pct_dimanfaatkan` |
| Filter / agregasi | Tanpa filter (semua baris sudah tersedia & ditargetkan); **AVG** |
| Widget | Tab 1 · KPI tile; Tab 2 · Bar chart per instansi |
| **Interpretasi yang dipilih** | Teks surat menggabungkan dua konsep: **"diselenggarakan"** (oleh instansi penyelenggara, mis. ANRI untuk Srikandi) dan **"dimanfaatkan"** (oleh instansi pengguna). Prototype hanya mengukur **pemanfaatan** oleh instansi pengguna, sesuai Bagian 2.C butir 3–4 surat ("instansi Pengguna harus memanfaatkan Aplikasi tersebut"). |
| Interpretasi alternatif (tidak dipakai) | (a) % aplikasi bagi pakai nasional (8 dari 40 aplikasi) yang sudah diselenggarakan; (b) % aplikasi bagi pakai nasional yang dimanfaatkan oleh ≥ 1 / seluruh instansi target. Keduanya menghasilkan angka berbeda. |
| **Perlu disahkan** | Unit analisis (aplikasi vs pasangan instansi-aplikasi) dan apakah "diselenggarakan" menjadi indikator terpisah. |

## KPI-04 — Persentase aplikasi yang terhubung dengan Portal

| Aspek | Definisi |
|---|---|
| Unit analisis | Aplikasi (40) |
| Pembilang / penyebut | `terhubung_portal = "Ya"` → 30 / 40 |
| Rumus | `100 × 30 / 40 = 75,0%` |
| Data feed / kolom | `DEMO_DF_SIAP_Aplikasi` → `pct_portal`; AVG; tanpa filter |
| Widget | Tab 1 · KPI tile; Tab 2 · Bar chart per instansi (**belum tersimpan**, lihat VALIDATION_REPORT) |
| Asumsi / isu | Definisi "Portal" (nasional/instansi) dan bukti status koneksi perlu divalidasi. Rekomendasi Pie Chart Ya/Tidak belum dibuat. |

## KPI-05 — Persentase repositori kode sumber terbuka yang digunakan

| Aspek | Definisi |
|---|---|
| Unit analisis | Repositori (`04_repositori.csv`, 48 baris) |
| Penyebut | Repositori dengan `digunakan_untuk_layanan_digital = "Ya"` → **42** (6 repositori tidak digunakan dikeluarkan) |
| Pembilang | Di antara penyebut, `jenis_kode_sumber = "Terbuka"` → 13 |
| Rumus | `100 × 13 / 42 = 30,95 → 31,0%` |
| Data feed / kolom | `DEMO_DF_SIAP_Repositori` → `pct_terbuka`; filter widget **Let values pass `digunakan_untuk_layanan_digital = Ya`**; AVG |
| Nilai tak diketahui | 4 repositori aktif `Belum Diklasifikasi` **tetap di penyebut** dan tidak dihitung sebagai Terbuka maupun Tertutup. Karena itu KPI-05 + KPI-06 = 90,5%, bukan 100%. |
| Widget | Tab 1 · KPI tile |
| Asumsi / isu | Kebijakan untuk repositori campuran/belum diklasifikasi perlu ditetapkan. |

## KPI-06 — Persentase repositori kode sumber tertutup yang digunakan

Sama dengan KPI-05, kecuali pembilang `jenis_kode_sumber = "Tertutup"` → **25 / 42 = 59,5%** (kolom `pct_tertutup`). Widget: Tab 1 · KPI tile.

## KPI-07 — Persentase aplikasi yang memanfaatkan Sistem Penghubung

| Aspek | Definisi |
|---|---|
| Pembilang / penyebut | `memanfaatkan_sistem_penghubung = "Ya"` → 27 / 40 |
| Rumus | `100 × 27 / 40 = 67,5%` |
| Data feed / kolom | `DEMO_DF_SIAP_Aplikasi` → `pct_penghubung`; AVG; tanpa filter |
| Widget | Tab 1 · KPI tile; Tab 2 · Bar chart per instansi |
| Asumsi / isu | Apakah penyebut seharusnya hanya aplikasi yang **wajib** terintegrasi; perlu disepakati. |

## KPI-08 — Persentase kelaikan aplikasi

| Aspek | Definisi |
|---|---|
| Unit analisis | Aplikasi |
| Penyebut | Aplikasi yang **sudah dinilai** (`status_kelaikan` ∈ {Layak, Tidak Layak}) → **35** |
| Pembilang | `status_kelaikan = "Layak"` → 28 |
| Rumus | `100 × 28 / 35 = 80,0%` |
| Data feed / kolom | `DEMO_DF_SIAP_Aplikasi` → `pct_layak`; filter widget **Block values `status_kelaikan = Belum Dinilai`**; AVG |
| Nilai tak diketahui | 5 aplikasi `Belum Dinilai` dikeluarkan dari penyebut. Tanpa filter nilainya 70,0% (28/40); angka ini sengaja tidak dipakai. |
| Cakupan penilaian | 35/40 = **87,5%** (belum ditampilkan di dashboard; lihat isu) |
| Widget | Tab 1 · KPI tile (judul memuat "dari yang telah dinilai") |
| Asumsi / isu | Surat (hal. 4) mendefinisikan kelaikan berdasarkan uji fungsi, keamanan, beban, dan integrasi, tetapi tidak menetapkan penyebut. Cakupan penilaian wajib ditampilkan berdampingan. |

## KPI-09 — Persentase hasil Audit Teknologi terhadap aplikasi

| Aspek | Definisi |
|---|---|
| Unit analisis | Entri audit (`05_audit_aplikasi.csv`, 40 baris, 1 audit per aplikasi) |
| Penyebut | Audit dengan `status_audit = "Selesai"` → **32** (semua punya `hasil_audit` Memenuhi/Belum Memenuhi) |
| Pembilang | `hasil_audit = "Memenuhi"` → 25 |
| Rumus | `100 × 25 / 32 = 78,125 → 78,1%` |
| Data feed / kolom | `DEMO_DF_SIAP_Audit` → `pct_audit_memenuhi`; filter widget **Let values pass `status_audit = Selesai`**; AVG |
| Nilai kosong | 8 audit (`Dalam Proses` 4, `Belum Dilaksanakan` 4) tanpa `hasil_audit` dikeluarkan dari penyebut. |
| Cakupan audit | 32/40 = **80,0%** (belum ditampilkan di dashboard) |
| Widget | Tab 1 · KPI tile ("audit selesai") |
| **Interpretasi yang dipilih** | "Persentase hasil audit" ditafsirkan sebagai **tingkat kesesuaian**: proporsi audit selesai yang hasilnya Memenuhi, selaras dengan definisi audit di hal. 4 surat ("menetapkan tingkat kesesuaian ... dengan kriteria dan/atau standar"). |
| Interpretasi alternatif (tidak dipakai) | (a) **Cakupan audit** = aplikasi yang sudah diaudit / seluruh aplikasi (80,0%); (b) Memenuhi / seluruh aplikasi (62,5%). |
| **Perlu disahkan** | Makna "hasil audit" (kesesuaian vs cakupan) dan apakah hasil bertingkat (bukan biner) akan dipakai. |
