# DATAFEED_CONFIGURATION — Dashboard SIAP Digital (Prototype)

Status per 2026-10-19. Semua feed dibuat oleh user `spbedev` di ARIS (`https://arsitektur.spbe.go.id`) → **Dashboards and data feeds → Data feeds**. Tidak ada feed lama milik pihak lain yang diubah atau dihapus.

## Sumber data

| Item | Nilai |
|---|---|
| Hosting CSV | Repo GitHub publik `bintangkurniawanpp/panrb-dashboard` (branch `main`), disetujui user |
| Pola URL | `https://raw.githubusercontent.com/bintangkurniawanpp/panrb-dashboard/main/<file>.csv` |
| Isi repo | 9 CSV, `README.md`, `README_PANDUAN.md`, `TASKS.md`. PDF surat dan ZIP dikecualikan lewat `.gitignore`. |
| Kesesuaian dengan file lokal | Kesembilan file identik dengan file lokal (`diff --strip-trailing-cr`); bedanya hanya line ending CRLF → LF. |

Sumber yang dicoba lebih dulu dan gagal ada di IMPLEMENTATION_LOG.md (ARIS Document Storage, URL alias, Google Drive).

## Parameter CSV operator (sama untuk semua feed)

| Parameter | Nilai |
|---|---|
| Operator | CSV (Data sources) |
| URL alias | tidak dipakai (URL ditulis langsung) |
| Separator | Comma (,) |
| Optional header row | 1 |
| Data from row | 2 |
| Refresh rate | 12h (default) |
| Autentikasi | tidak ada (raw GitHub publik) |
| Deteksi tipe | otomatis. `periode` dan `tanggal_audit` → Date; kolom angka 06/07 (`nilai`, `pembilang`, `penyebut`) → Number; sisanya Text |

## Teknik kolom numerik

Widget ARIS hanya menerima kolom **Number** sebagai Measure, sedangkan feed detail hanya berisi teks. Karena itu setiap feed detail diberi:

1. **Insert column**: `jumlah` (Number, nilai 1) dan satu atau lebih `pct_*` (Number, nilai awal 0).
2. **Conditional replace** per kolom `pct_*`: ganti dengan **100** bila kondisi teks terpenuhi (operator *equals*).

Dengan cara ini, `SUM(jumlah)` menghasilkan jumlah baris dan `AVG(pct_*)` menghasilkan persentase.

## Daftar feed

### 1. DEMO_DF_SIAP_Aplikasi
| Item | Nilai |
|---|---|
| CSV sumber | `01_aplikasi.csv` (40 baris) |
| Operator | CSV → Insert column (`jumlah`=1, `pct_standar`=0, `pct_portal`=0, `pct_penghubung`=0, `pct_layak`=0) → Conditional replace ×4 |
| Conditional replace | `pct_standar`=100 jika `menerapkan_standar = Memenuhi`; `pct_portal`=100 jika `terhubung_portal = Ya`; `pct_penghubung`=100 jika `memanfaatkan_sistem_penghubung = Ya`; `pct_layak`=100 jika `status_kelaikan = Layak` |
| Output schema | `periode`(Date), `aplikasi_id`, `nama_aplikasi`, `instansi_id`, `instansi`, `jenis_aplikasi`, `menerapkan_standar`, `terhubung_portal`, `memanfaatkan_sistem_penghubung`, `status_kelaikan`, `bagi_pakai_nasional` (Text); `jumlah`, `pct_standar`, `pct_portal`, `pct_penghubung`, `pct_layak` (Number) |
| Hasil Calculate | OK. Baris APP001–APP004 dicek manual (mis. APP003 `pct_penghubung`=0, APP004 `pct_portal`=0) |
| Dipakai oleh | KPI-01 (pie), KPI-04, KPI-07, KPI-08 (tile), bar KPI-04 dan KPI-07 (Tab 2) |
| Status | **Tersimpan, terverifikasi** |

### 2. DEMO_DF_SIAP_Layanan
| Item | Nilai |
|---|---|
| CSV sumber | `02_layanan.csv` (64 baris) |
| Operator | CSV → Insert column (`jumlah`=1, `pct_melalui_aplikasi`=0) → Conditional replace (`pct_melalui_aplikasi`=100 jika `melalui_aplikasi = Ya`) |
| Output schema | kolom CSV + `jumlah`, `pct_melalui_aplikasi` (Number) |
| Hasil Calculate | OK (LD001–LD004 = 100) |
| Dipakai oleh | KPI-02 (tile, bar Tab 2) |
| Status | **Tersimpan, terverifikasi** |

### 3. DEMO_DF_SIAP_BagiPakai
| Item | Nilai |
|---|---|
| CSV sumber | `03_pemanfaatan_bagi_pakai.csv` (64 baris) |
| Operator | CSV → Insert column (`jumlah`=1, `pct_dimanfaatkan`=0) → Conditional replace (`pct_dimanfaatkan`=100 jika `dimanfaatkan = Ya`) |
| Hasil Calculate | OK (APP011 di INS01 = 0, sesuai CSV) |
| Dipakai oleh | KPI-03 (tile, bar Tab 2) |
| Status | **Tersimpan, terverifikasi** |

### 4. DEMO_DF_SIAP_Repositori
| Item | Nilai |
|---|---|
| CSV sumber | `04_repositori.csv` (48 baris) |
| Operator | CSV → Insert column (`jumlah`=1, `pct_terbuka`=0, `pct_tertutup`=0) → Conditional replace ×2 (`pct_terbuka`=100 jika `jenis_kode_sumber = Terbuka`; `pct_tertutup`=100 jika `= Tertutup`) |
| Catatan | `Belum Diklasifikasi` tetap 0 di kedua kolom |
| Hasil Calculate | OK (REP001 Tertutup → 0/100; REP003 Terbuka → 100/0) |
| Dipakai oleh | KPI-05, KPI-06 (tile; filter `digunakan_untuk_layanan_digital = Ya` di level widget) |
| Status | **Tersimpan, terverifikasi** |

### 5. DEMO_DF_SIAP_Audit
| Item | Nilai |
|---|---|
| CSV sumber | `05_audit_aplikasi.csv` (40 baris) |
| Operator | CSV → Insert column (`jumlah`=1, `pct_audit_memenuhi`=0) → Conditional replace (`pct_audit_memenuhi`=100 jika `hasil_audit = Memenuhi`) |
| Hasil Calculate | OK (AUD005 Belum Memenuhi = 0; AUD006 Belum Dilaksanakan = 0) |
| Dipakai oleh | KPI-09 (tile; filter `status_audit = Selesai` di level widget) |
| Status | **Tersimpan, terverifikasi** |

### 6. DEMO_DF_SIAP_KPI_Ringkasan
| Item | Nilai |
|---|---|
| CSV sumber | `06_kpi_ringkasan.csv` (9 baris) |
| Operator | CSV saja (tanpa transformasi) |
| Output schema | `periode`(Date), `kode_indikator`, `indikator`, `satuan`, `status_data`, `definisi_pembilang`, `definisi_penyebut` (Text); `nilai`, `pembilang`, `penyebut` (Number) |
| Dipakai oleh | belum ada widget; disimpan sebagai **referensi pembanding** angka pra-hitung |
| Status | Tersimpan, terverifikasi |

### 7. DEMO_DF_SIAP_KPI_Instansi
| Item | Nilai |
|---|---|
| CSV sumber | `07_kpi_per_instansi.csv` (72 baris) |
| Operator | CSV saja (dibuat lewat *Save as* dari feed Ringkasan dengan URL diganti) |
| Dipakai oleh | belum ada widget; referensi pembanding per instansi |
| Status | Tersimpan, terverifikasi |

## Transformasi di level widget (Assign data)

Filter penyebut dipasang di widget, bukan di feed, supaya feed tetap reusable.

| Widget | Feed | Operator tambahan | Parameter |
|---|---|---|---|
| KPI-08 tile | Aplikasi | Filter rows | **Block values** `status_kelaikan` equals `Belum Dinilai` |
| KPI-09 tile | Audit | Filter rows | **Let values pass** `status_audit` equals `Selesai` |
| KPI-05 tile | Repositori | Filter rows | **Let values pass** `digunakan_untuk_layanan_digital` equals `Ya` |
| KPI-06 tile | Repositori | Filter rows | **Let values pass** `digunakan_untuk_layanan_digital` equals `Ya` |

## Catatan operasional

- Saat calculate di editor, ARIS kadang macet (spinner tanpa akhir), terutama sesudah beberapa kali gagal. Muat ulang tab editor (draft lokal tetap ada) atau pilih ulang operator sebelum *Calculate* biasanya menyelesaikannya.
- Setelah sumber/kolom feed berubah, widget yang memakainya menyimpan skema lama. Data source harus dibuka ulang (Assign data → Calculate → ✓) supaya kolom baru muncul.
- Karena URL raw GitHub bersifat publik, siapa pun yang punya URL bisa membaca CSV. Ini dapat diterima karena datanya simulasi. Untuk data riil, ganti dengan sumber internal (lihat IMPLEMENTATION_LOG, rekomendasi).
