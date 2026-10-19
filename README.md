# panrb-dashboard — Data CSV Prototype SIAP Digital

**DATA SIMULASI / PROTOTYPE — BUKAN DATA RIIL.** Seluruh nama aplikasi, nilai, dan status di repo ini adalah data simulasi untuk prototype Dashboard Arsitektur Aplikasi Pemerintah Digital pada ARIS MashZone. Nama 8 instansi (INS01–INS08) diambil dari daftar Instansi Pusat (`instansi-pusat.xls`) **hanya sebagai label**; angka yang dikaitkan dengannya bukan kondisi riil instansi tersebut.

File CSV di root repo dibaca oleh Data Feed ARIS (`DEMO_DF_SIAP_*`) melalui URL `https://raw.githubusercontent.com/bintangkurniawanpp/panrb-dashboard/main/<file>.csv`.

| File | Isi |
|---|---|
| 01_aplikasi.csv | 40 aplikasi (KPI-01, 04, 07, 08) |
| 02_layanan.csv | 64 layanan digital (KPI-02) |
| 03_pemanfaatan_bagi_pakai.csv | 64 pasangan instansi × aplikasi bagi pakai nasional (KPI-03) |
| 04_repositori.csv | 48 repositori (KPI-05, 06) |
| 05_audit_aplikasi.csv | 40 entri audit (KPI-09) |
| 06_kpi_ringkasan.csv | 9 KPI agregat |
| 07_kpi_per_instansi.csv | 72 baris KPI per instansi |
| 08_pemetaan_widget.csv | Rancangan widget |
| 09_definisi_indikator.csv | Definisi & asumsi indikator |
