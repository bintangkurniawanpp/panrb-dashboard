# Prototype ARIS Dashboard - SIAP Digital (SIMULASI)

Acuan: Surat Komdigi No. B-513/DJTPD.4/AI.01.01/08/2026, Lampiran, Bagian 3 (hal. 6), memuat 9 kebutuhan data analitik dashboard SIAP Digital. Surat menyebutnya sebagai usulan awal, bukan keputusan penetapan indikator yang final.

**SELURUH NAMA INSTANSI, APLIKASI, NILAI, STATUS, DAN PERSENTASE DALAM BERKAS INI ADALAH DATA SIMULASI.** Data ini tidak menyatakan kondisi instansi atau aplikasi pemerintah yang sebenarnya.

## Langsung uji ARIS
1. Uji file `06_kpi_ringkasan.csv` pada Data Feed editor untuk KPI umum, atau `07_kpi_per_instansi.csv` untuk bar chart per instansi. Angka telah dipra-hitung untuk penyederhanaan prototype.
2. Pada CSV operator: URL = alamat HTTP(S) yang benar-benar menyajikan isi file CSV tanpa autentikasi yang menghalangi ARIS server. `Separator = Comma (,)`, `Optional header row = 1`, `Data from row = 2`. Isi `URL alias` hanya jika server ARIS Anda sudah memiliki URL alias yang dikonfigurasi.
3. Klik Calculate / Quick preview, periksa bahwa kolom `nilai`, `pembilang`, `penyebut` bertipe Number. Gunakan Change data type bila perlu.
4. Klik Save as data feed, simpan `DF_SIAP_KPI_Ringkasan` dan `DF_SIAP_KPI_Instansi`.
5. Dashboard editor > Add widget > Bar chart > Data source > Data feeds > pilih `DF_SIAP_KPI_Instansi`. Filter `kode_indikator = KPI-07`, Dimension = `instansi`, Measure = `nilai`. Jangan gabungkan kode indikator yang satuannya berlainan dalam satu sumbu.

**Catatan**: File lokal atau tautan pratinjau Google Drive tidak otomatis menjadi URL CSV yang dapat dibaca operator. Sajikan melalui HTTPS URL langsung yang dapat diakses server ARIS, atau sumber ARIS Documents/Web documents sesuai konfigurasi deployment.

## Peta file
- `01_aplikasi.csv`: 40 aplikasi simulasi; standar, portal, sistem penghubung, kelaikan, dan aplikasi nasional bagi pakai (KPI 01,04,07,08).
- `02_layanan.csv`: 64 layanan; sudah dilayani melalui aplikasi atau belum (KPI 02).
- `03_pemanfaatan_bagi_pakai.csv`: 64 pasangan instansi x aplikasi bersama nasional sebagai target pemanfaatan (KPI 03).
- `04_repositori.csv`: 48 repositori; jenis kode dan status dipakai untuk layanan (KPI 05,06).
- `05_audit_aplikasi.csv`: 40 entri audit; status dan hasil (KPI 09).
- `06_kpi_ringkasan.csv`: 9 baris indikator pada total 8 instansi simulasi; cepat untuk widget Summary/KPI.
- `07_kpi_per_instansi.csv`: 72 baris indikator per instansi; cepat untuk widget Bar chart dan filter.
- `08_pemetaan_widget.csv`: rancangan widget, data feed, filter, Dimension dan Measure per Tab.
- `09_definisi_indikator.csv`: sembilan teks indikator dari surat beserta rumus dan catatan validasi sebagai asumsi prototype.

## Rumus asumsi sementara (bukan definisi formal dari surat)
- KPI-01: Jumlah aplikasi dengan `menerapkan_standar=Memenuhi` (satuan Jumlah).
- KPI-02: 100 x layanan `melalui_aplikasi=Ya` / semua layanan yang tercatat.
- KPI-03: 100 x target pasangan instansi-aplikasi dengan `dimanfaatkan=Ya` / seluruh pasangan instansi-aplikasi nasional yang ditargetkan. Surat juga menyebut `diselenggarakan`; dimensi ketersediaan/penyelenggaraan perlu ditampilkan terpisah jika datanya tersedia.
- KPI-04: 100 x aplikasi `terhubung_portal=Ya` / semua aplikasi.
- KPI-05: 100 x repositori aktif digunakan dengan `jenis_kode_sumber=Terbuka` / semua repositori aktif digunakan untuk layanan.
- KPI-06: 100 x repositori aktif digunakan dengan `jenis_kode_sumber=Tertutup` / semua repositori aktif digunakan untuk layanan. Sisa bisa `Belum Diklasifikasi`, sehingga KPI 05 + KPI 06 tak harus 100%.
- KPI-07: 100 x aplikasi `memanfaatkan_sistem_penghubung=Ya` / semua aplikasi.
- KPI-08: 100 x aplikasi `Layak` / semua aplikasi yang telah dinilai; juga tampilkan cakupan penilaian terhadap total aplikasi.
- KPI-09: 100 x hasil audit `Memenuhi` / audit selesai dengan hasil memenuhi/tidak memenuhi; juga tampilkan cakupan audit terhadap total aplikasi. Ini INTERPRETASI PROTOTYPE: surat belum menentukan definisi persentase hasil audit.

## Prinsip hitung
- Jangan rata-ratakan persentase antar instansi. Untuk roll-up hitung `100 * SUM(pembilang) / SUM(penyebut)` per indikator.
- Data feed sudah memuat nilai `nilai` yang dihitung; jika kolom angka terbaca Text, lakukan `Change data type`.
- Hindari menghitung `Count` pada baris untuk KPI-01 di tabel summary; gunakan `nilai` sebagai measure.
- Semua data memakai `periode=2026-10` sebagai penanda simulasi snapshot, bukan realisasi terverifikasi Oktober 2026.
- Survei/pengukuran di dunia nyata memerlukan penetapan populasi, periode, sumber bukti, aturan penilaian, validasi dan pemilik indikator.

## Hubungan domain (bagian 4 surat)
Aplikasi berhubungan langsung dengan Layanan, Keamanan, Data, dan Infrastruktur; relasi aplikasi dengan domain Kinerja, SDM, dan Proses Bisnis tidak digambarkan secara langsung. Untuk prototype tahap kedua, gunakan relasi eksplisit bila melakukan integrasi data lintas-domain.
