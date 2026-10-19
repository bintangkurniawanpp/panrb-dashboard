# IMPLEMENTATION_LOG — Dashboard SIAP Digital (Prototype) di ARIS

Tanggal kerja: 2026-10-19. Akun ARIS: `spbedev` (sesi browser user). Semua objek yang dibuat berawalan `DEMO`.

## Lokasi hasil

| Objek | Lokasi |
|---|---|
| Dashboard | ARIS → menu aplikasi → **Dashboards and data feeds** → Dashboards → **"DEMO — SIAP Digital \| Dashboard Arsitektur Aplikasi Pemerintah Digital"** |
| Data feed | ARIS → Dashboards and data feeds → **Data feeds** → `DEMO_DF_SIAP_Aplikasi`, `_Layanan`, `_BagiPakai`, `_Repositori`, `_Audit`, `_KPI_Ringkasan`, `_KPI_Instansi` |
| CSV sumber | https://github.com/bintangkurniawanpp/panrb-dashboard (branch `main`, root) |
| Screenshot | `screenshots/01_tab1_atas.jpg`, `02_tab1_bawah.jpg`, `03_tab2_atas.jpg`, `04_tab2_bawah.jpg` (mode edit) |

## Kronologi

| # | Fase | Aksi | Hasil |
|---|---|---|---|
| 1 | A | Membaca PDF B-513 (hal. 1–9), README, 9 CSV; menjalankan skrip validasi Python | Semua KPI di 06/07 cocok dengan perhitungan ulang; tidak ada isu data |
| 2 | B | Membuka ARIS di tab baru browser user; menelusuri Dashboards and data feeds | Ditemukan 6 dashboard dan 7 feed milik pihak lain; **tidak disentuh** |
| 3 | B | Mencoba **ARIS Document Storage** (menu Documents dan panel "ARIS documents" di Data Feed Editor) | **Gagal**: tab kosong dengan label i18n rusak `???en-US-arissrv.absDataProviderComponent.componentInfo.DBI???`, tanpa request jaringan; panel di Data Feed Editor loading tanpa akhir disertai JS exception |
| 4 | B | Mencoba **URL alias** bawaan (`URL SPBE Arsitektur` = `https://arsitektur.spbe.go.id:443`) | **Gagal**: `JBPMZCE0389 … Read timed out`; server tidak bisa memanggil URL publiknya sendiri |
| 5 | B | Atas instruksi user, memakai **folder Google Drive** bersama | `drive.usercontent.google.com/download?...` → `JBPMZCE0350 … 500`. `drive.google.com/uc?export=download&id=…` → **berhasil** |
| 6 | C | Membuat 6 feed (06, 01–05) dengan URL Drive lewat *Save* / *Save as* | Tersimpan |
| 7 | D | Membuat dashboard dan menyimpannya dengan nama yang diminta; menambah Pie KPI-01 | Tersimpan; ditemukan bahwa Measure wajib kolom Number |
| 8 | C | Menambah kolom numerik ke feed Audit | Setelah ±12 unduhan, Drive membalas **403 Forbidden** untuk semua file (`drive.google.com/uc` maupun `docs.google.com/uc`) dari IP server ARIS → **BLOCKER**, eskalasi ke user |
| 9 | B | User membuat repo GitHub dan meminta upload; lalu meminta folder kerja dijadikan repo | 9 CSV dipush (commit `d01c9e7`, `36b5bfa`). PDF & ZIP dikecualikan via `.gitignore`. Raw URL diverifikasi identik dengan file lokal |
| 10 | C | Mengarahkan 6 feed ke raw GitHub; menambah Insert column + Conditional replace di 5 feed detail; membuat `DEMO_DF_SIAP_KPI_Instansi` | Semua feed Calculate OK & tersimpan |
| 11 | D | Melengkapi Pie KPI-01 (SUM, sort desc, legend, nilai absolut + %) | 29 (72,5%) / 11 (27,5%) |
| 12 | D | Membuat tile KPI-02 sebagai template (presisi 1 desimal, sufiks %, warna navy #0B2E59, ikon & threshold dihapus), lalu Copy/Paste untuk KPI-03…09 | 8 tile, nilai sesuai |
| 13 | D | Menambah Filter rows di Assign data untuk KPI-05/06 (`digunakan=Ya`), KPI-08 (block `Belum Dinilai`), KPI-09 (`Selesai`) | Penyebut sesuai definisi |
| 14 | D | Reload dashboard setelah simpan | Widget & nilai bertahan (uji persistensi parsial) |
| 15 | D | Mengganti nama tab: "1 · Ringkasan Eksekutif", "2 · Penyelenggaraan & Integrasi", "3 · Repositori, Kelaikan & Audit"; "Show tabs as" diubah dari *Hidden in view mode* ke **Tabs** | Tersimpan |
| 16 | D | Tab 2: bar chart per instansi KPI-02, KPI-03, KPI-07 (palet MONOBLUE, label nilai %) | Tersimpan, nilai cocok 07 |
| 17 | D | Tab 2: bar chart KPI-04 per instansi (salinan KPI-07, measure `pct_portal`, judul) | Nilai benar; **dihentikan user sebelum Save** |
| 18 | — | User meminta berhenti, lalu meminta status + screenshot, lalu dokumen tanpa mengubah ARIS | Screenshot diambil di mode edit; ARIS tidak diubah lagi |
| 19 | A | Atas permintaan user, label "Instansi Simulasi A–H" di CSV 01–05 & 07 diganti dengan 8 Instansi Pusat dari `instansi-pusat.xls` (PANRB, Komdigi, Kemendagri, Kemenkeu, Kemenkes, BKN, BPS, ANRI). `instansi_id` & nilai tetap | 328 baris diperbarui; validasi ulang PASS. **Belum di-commit/push** (instruksi user), jadi ARIS masih menampilkan nama lama |

## Keputusan konfigurasi

| Keputusan | Alasan |
|---|---|
| Satu feed per CSV detail, ditambah 2 feed ringkasan sebagai pembanding | Keterlacakan indikator → kolom sumber tetap jelas; feed reusable lintas widget |
| Persentase dari AVG flag 100/0, bukan dari kolom `nilai` 06/07 | Angka dihitung dari dataset, bukan pra-hitung statis; per instansi benar secara matematis (bukan rata-rata persentase) |
| Filter penyebut di level widget, bukan di feed | Satu feed Aplikasi melayani KPI-01/04/07/08 dengan penyebut berbeda |
| Threshold bawaan KPI dihapus | Surat tidak menetapkan target; warna merah/kuning/hijau akan menyesatkan |
| KPI-01 tetap satuan Jumlah | Sesuai teks indikator; persentase hanya komposisi di pie |
| KPI-03 = pemanfaatan pasangan instansi×aplikasi; KPI-09 = Memenuhi / audit selesai | Interpretasi paling konsisten dengan surat; alternatif didokumentasikan di KPI_DEFINITIONS.md |
| Nama instansi riil dipasangkan dengan nilai simulasi tanpa mengubah ID/nilai | Permintaan user; dashboard & validasi tetap berlaku. Risiko: angka simulasi bisa disangka kondisi riil instansi tersebut, sehingga penanda "DATA SIMULASI / PROTOTYPE" di dashboard menjadi **wajib** |
| Pie memakai palet BLUERED | ARIS hanya menyediakan palet bawaan; MONOBLUE membuat dua kategori pertama nyaris sama warnanya |

## Kendala dan solusi

| Kendala | Dampak | Solusi / status |
|---|---|---|
| Document Storage tidak berfungsi di instalasi ini | Tidak bisa unggah CSV ke ARIS | Pakai URL HTTPS eksternal (disetujui user). Perlu dicek admin ARIS |
| Server ARIS tidak bisa memanggil URL publiknya sendiri | URL alias bawaan tidak bisa dipakai untuk file di server ARIS | — |
| Google Drive memblokir unduhan otomatis (403) | Feed berbasis Drive tidak andal | Pindah ke raw GitHub |
| Widget hanya menerima Measure Number | CSV teks tidak bisa langsung di-agregasi | Insert column + Conditional replace |
| Editor kadang macet saat Calculate / seleksi widget hilang setelah *Done* | Memperlambat kerja | Reload tab (draft lokal tersimpan), pilih ulang widget lewat ikon ⚙ |
| Layout Tab 1 tersusun ulang dengan celah | Tampilan tidak rapi | **Belum diperbaiki** |

## Belum selesai (BLOCKED/PENDING oleh keputusan user untuk berhenti)

1. Simpan bar chart KPI-04 per instansi (Tab 2).
2. Pie komposisi koneksi Portal (Ya/Tidak) di Tab 2 (rekomendasi visualisasi KPI-04).
3. Tab 3: bar per instansi KPI-05, 06, 08, 09 dan pie komposisi jenis repositori aktif (Terbuka/Tertutup/Belum Diklasifikasi).
4. Merapikan layout Tab 1 (ukuran tile, urutan KPI-01…09, celah).
5. Banner "DATA SIMULASI / PROTOTYPE" (widget Title / Rich text label).
6. Tampilkan cakupan penilaian KPI-08 (87,5%) dan cakupan audit KPI-09 (80,0%).
7. Uji buka ulang dashboard di **mode view** dan navigasi tab.
8. Screenshot final dalam mode view.

## Rekomendasi menuju integrasi dengan ARIS Repository aktual

- Minta admin ARIS memperbaiki komponen **Document Storage** atau menyediakan URL alias ke sumber data internal, supaya data riil tidak perlu dihosting di layanan publik.
- Sahkan definisi KPI-03 dan KPI-09 (unit analisis, penyebut) dan target tiap KPI sebelum threshold warna diaktifkan.
- Ganti sumber CSV dengan **ARIS Table / query repository** (operator *ARIS Table* tersedia di Data Feed Editor) yang membaca atribut objek Aplikasi di "Database Arsitektur SPBE", dengan pemetaan atribut ke kolom `menerapkan_standar`, `terhubung_portal`, dll.
