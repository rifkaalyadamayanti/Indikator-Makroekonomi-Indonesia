# Indikator-Makroekonomi-Indonesia

# 🇮🇩 Tutorial Tableau: Dashboard Makroekonomi Indonesia
### Panduan Lengkap dari Data, Layouting, hingga Publish

> **Tools:** Tableau Desktop / Tableau Public · **Data:** BPS, Kemendag, Bank Indonesia · **Level:** Pemula hingga Menengah

---

## Daftar Isi

1. [Dataset yang Dibutuhkan & Sumbernya](#1-dataset-yang-dibutuhkan--sumbernya)
2. [Persiapan: Install & Setup Tableau](#2-persiapan-install--setup-tableau)
3. [Import & Koneksi Data](#3-import--koneksi-data)
4. [Membersihkan Data (Data Interpreter)](#4-membersihkan-data-data-interpreter)
5. [Membuat Kalkulasi & Calculated Fields](#5-membuat-kalkulasi--calculated-fields)
6. [Layouting Dashboard — Wireframe & Struktur](#6-layouting-dashboard--wireframe--struktur)
7. [Membangun Sheet Visual Satu per Satu](#7-membangun-sheet-visual-satu-per-satu)
8. [Merakit Dashboard di Dashboard Canvas](#8-merakit-dashboard-di-dashboard-canvas)
9. [Design System: Warna, Font & Tema](#9-design-system-warna-font--tema)
10. [Interaktivitas: Filter, Action & Parameter](#10-interaktivitas-filter-action--parameter)
11. [Publish ke Tableau Public / Tableau Server](#11-publish-ke-tableau-public--tableau-server)
12. [Tips & Best Practices](#12-tips--best-practices)

---

## 1. Dataset yang Dibutuhkan & Sumbernya

Kita akan menggunakan **6 dataset utama** yang semuanya tersedia gratis dari sumber resmi pemerintah Indonesia. Unduh semua file dan simpan dalam satu folder, misalnya `C:\Tableau\Indonesia_Makro\data\`.

---

### 1.1 Pertumbuhan PDB Triwulanan

| Atribut | Detail |
|---|---|
| **Nama file** | `pdb_triwulanan.xlsx` |
| **Sumber** | BPS — Badan Pusat Statistik |
| **URL** | https://www.bps.go.id/id/statistics-table/1/MTExNiMx/produk-domestik-bruto--pdb--triwulanan-atas-dasar-harga-berlaku-menurut-lapangan-usaha.html |
| **Alternatif** | https://databoks.katadata.co.id/datapublish/tag/pdb |
| **Format** | XLSX / CSV |
| **Kolom yang dibutuhkan** | Tahun, Kuartal, Growth_YoY (%), Growth_QoQ (%), PDB_ADHB_Triliun, Lapangan_Usaha, Pulau |
| **Rentang data** | 2015 Q1 — 2025 Q4 |

**Data sampel yang perlu disiapkan:**

```
Tahun | Kuartal | Growth_YoY | Growth_QoQ | PDB_ADHB_T | Lapangan_Usaha
2020  | Q1      | 2.97       | -2.41      | 3922.6     | Pertanian
2020  | Q2      | -5.32      | -4.19      | 3687.7     | Industri Pengolahan
...
2025  | Q4      | 5.39       | 0.86       | 6012.5     | Konstruksi
```

> 💡 **Cara unduh di BPS:** Masuk ke bps.go.id → Tabel Statistik → Ekonomi → PDB → klik tabel yang diinginkan → klik ikon **"Unduh Excel"** di pojok kanan atas tabel.

---

### 1.2 Inflasi Bulanan per Komponen

| Atribut | Detail |
|---|---|
| **Nama file** | `inflasi_bulanan.xlsx` |
| **Sumber** | BPS — Inflasi / IHK |
| **URL** | https://www.bps.go.id/id/statistics-table/2/NDc3IzI=/inflasi-bulanan--tahun-dasar-2022-.html |
| **Alternatif** | https://databoks.katadata.co.id/datapublish/tag/inflasi |
| **Format** | XLSX |
| **Kolom yang dibutuhkan** | Tahun, Bulan, Tanggal, Kelompok_Komoditas, Inflasi_YoY (%), Inflasi_MoM (%), Kontribusi_pp |
| **Rentang data** | Jan 2020 — Apr 2026 |

**Kelompok komoditas yang ada di data BPS:**
- Makanan, Minuman & Tembakau
- Pakaian & Alas Kaki
- Perumahan, Air, Listrik & Bahan Bakar
- Perlengkapan, Peralatan & Pemeliharaan Rutin RT
- Kesehatan
- Transportasi
- Informasi, Komunikasi & Jasa Keuangan
- Rekreasi, Olahraga & Budaya
- Pendidikan
- Penyediaan Makanan & Minuman / Restoran
- Perawatan Pribadi & Jasa Lainnya
- **Umum** (total semua kelompok)

---

### 1.3 Ketenagakerjaan per Provinsi

| Atribut | Detail |
|---|---|
| **Nama file** | `ketenagakerjaan.xlsx` |
| **Sumber** | BPS — Ketenagakerjaan |
| **URL** | https://www.bps.go.id/id/statistics-table/2/NTYyIzI=/tingkat-pengangguran-terbuka--tpt--menurut-provinsi.html |
| **Format** | XLSX |
| **Kolom yang dibutuhkan** | Tahun, Periode (Feb/Agt), Provinsi, Kode_Provinsi, TPT_Persen, Pekerja_Formal_Persen, Pekerja_Informal_Persen, Total_Angkatan_Kerja |
| **Rentang data** | 2015 — 2025 (Februari & Agustus) |

> ⚠️ **Penting:** BPS merilis data TPT dua kali setahun — bulan **Februari** dan **Agustus**. Buat kolom `Semester` untuk membedakannya.

---

### 1.4 Ekspor-Impor & Neraca Perdagangan

| Atribut | Detail |
|---|---|
| **Nama file** | `ekspor_impor.xlsx` |
| **Sumber** | Kemendag — Satu Data Perdagangan |
| **URL** | https://satudata.kemendag.go.id/data-informasi/data-perdagangan/ekspor |
| **Sumber alternatif** | BPS: https://www.bps.go.id/id/statistics-table/2/MTk4NSMy/nilai-ekspor-menurut-komoditas.html |
| **Format** | XLSX |
| **Kolom yang dibutuhkan** | Tahun, Bulan, Tanggal, Ekspor_Total_USD, Ekspor_Migas_USD, Ekspor_NonMigas_USD, Impor_Total_USD, Impor_Migas_USD, Impor_NonMigas_USD, Neraca_USD, Komoditas, Negara_Tujuan |
| **Rentang data** | Jan 2018 — Mar 2026 |

---

### 1.5 Indeks Pembangunan Manusia (IPM) per Provinsi

| Atribut | Detail |
|---|---|
| **Nama file** | `ipm_provinsi.xlsx` |
| **Sumber** | BPS — IPM |
| **URL** | https://www.bps.go.id/id/statistics-table/2/MjEyMiMy/indeks-pembangunan-manusia-menurut-provinsi.html |
| **Format** | XLSX |
| **Kolom yang dibutuhkan** | Tahun, Provinsi, Kode_Provinsi, IPM_Score, AHH (Angka Harapan Hidup), HLS (Harapan Lama Sekolah), RLS (Rata-rata Lama Sekolah), Pengeluaran_Kapita_Ribu |
| **Rentang data** | 2015 — 2025 |

---

### 1.6 Kemiskinan & Gini Ratio per Provinsi

| Atribut | Detail |
|---|---|
| **Nama file** | `kemiskinan.xlsx` |
| **Sumber** | BPS — Kemiskinan |
| **URL** | https://www.bps.go.id/id/statistics-table/2/MTkwMiMy/persentase-penduduk-miskin-menurut-provinsi.html |
| **Format** | XLSX |
| **Kolom yang dibutuhkan** | Tahun, Semester, Provinsi, Kode_Provinsi, Miskin_Persen, Jumlah_Miskin_Ribu, Garis_Kemiskinan, Gini_Ratio |
| **Rentang data** | 2015 — 2025 |

---

### Ringkasan Semua Dataset

| # | File | Sumber | Format | Baris Data |
|---|---|---|---|---|
| 1 | pdb_triwulanan.xlsx | BPS | XLSX | ~600 baris |
| 2 | inflasi_bulanan.xlsx | BPS | XLSX | ~3.000 baris |
| 3 | ketenagakerjaan.xlsx | BPS | XLSX | ~800 baris |
| 4 | ekspor_impor.xlsx | Kemendag/BPS | XLSX | ~5.000 baris |
| 5 | ipm_provinsi.xlsx | BPS | XLSX | ~400 baris |
| 6 | kemiskinan.xlsx | BPS | XLSX | ~600 baris |

---

## 2. Persiapan: Install & Setup Tableau

### 2.1 Pilih Versi Tableau

| Versi | Harga | Cocok Untuk |
|---|---|---|
| **Tableau Public** | Gratis | Portofolio, dashboard publik, pembelajaran |
| **Tableau Desktop** | Berbayar (ada trial 14 hari) | Profesional, data sensitif |
| **Tableau Cloud** | Berbayar | Tim, kolaborasi |

> 💡 Untuk tutorial ini, **Tableau Public** sudah cukup. Download di: https://public.tableau.com/app/discover

### 2.2 Instalasi

1. Buka halaman download Tableau Public
2. Masukkan email → klik **"Download the App"**
3. Jalankan installer (Windows/Mac)
4. Buat akun Tableau Public gratis saat diminta

### 2.3 Kenali Interface Tableau Desktop

```
┌─────────────────────────────────────────────────────────────┐
│  FILE   DATA   WORKSHEET   DASHBOARD   STORY   ANALYSIS     │  ← Menu Bar
├──────────────────┬──────────────────────────────────────────┤
│                  │  Pages   Filters   Marks                  │
│  Data Pane       ├──────────────────────────────────────────┤
│  ─────────       │  Columns                                  │
│  📁 Dimensions   │  Rows                                     │
│    Tahun         ├──────────────────────────────────────────┤
│    Provinsi      │                                           │
│    Kelompok      │        V I E W / C A N V A S             │
│                  │                                           │
│  📊 Measures     │        (Drag fields here)                 │
│    Growth_YoY    │                                           │
│    Inflasi_%     │                                           │
│    Ekspor_USD    │                                           │
└──────────────────┴──────────────────────────────────────────┘
│ Sheet 1 | Sheet 2 | + New Sheet | Dashboard 1 | + Dashboard │  ← Sheet Tabs
└─────────────────────────────────────────────────────────────┘
```

**Panel-panel penting:**
- **Data Pane (kiri):** Semua field dari dataset. Biru = Dimensions (kategori/teks), Hijau = Measures (angka)
- **Columns & Rows Shelf:** Drag field ke sini untuk menentukan sumbu chart
- **Marks Card:** Kontrol Color, Size, Label, Tooltip, Shape dari visual
- **Filter Shelf:** Drag field ke sini untuk memfilter data
- **Pages Shelf:** Buat animasi berdasarkan field tertentu (misal: animasi per tahun)

---

## 3. Import & Koneksi Data

### 3.1 Connect ke File Excel

1. Buka Tableau → di halaman **Start**, klik **"Microsoft Excel"** di panel **Connect**
2. Navigasi ke folder data → pilih `pdb_triwulanan.xlsx` → **Open**
3. Tableau membuka **Data Source** tab

```
┌──────────────────────────────────────────────────────────┐
│  DATA SOURCE TAB                                          │
├────────────────┬─────────────────────────────────────────┤
│  Connections   │  Tables / Sheets                        │
│  ─────────     │                                         │
│  📗 pdb_tri-   │  📄 Sheet1 — PDB_Data                   │
│     wulanan    │  📄 Sheet2 — Metadata                   │
│                │                                         │
│                │  ↓ Drag sheet ke canvas di bawah        │
├────────────────┴─────────────────────────────────────────┤
│                     DATA PREVIEW                         │
│  Tahun │ Kuartal │ Growth_YoY │ PDB_ADHB_T │ Lap_Usaha  │
│  2020  │ Q1      │ 2.97       │ 3922.6     │ Pertanian  │
│  2020  │ Q2      │ -5.32      │ 3687.7     │ Industri   │
└──────────────────────────────────────────────────────────┘
```

### 3.2 Add Lebih dari Satu File (Multiple Connections)

Tableau bisa terhubung ke beberapa file sekaligus:

1. Di Data Source tab, klik **"Add"** di panel Connections (kiri atas)
2. Pilih `inflasi_bulanan.xlsx` → Open
3. Ulangi untuk semua 6 file

Setiap file akan muncul sebagai **Data Source terpisah** di panel kiri Tableau. Untuk menggunakannya di worksheet, klik ikon database dan pilih sumber mana yang aktif.

### 3.3 Join atau Union Data (Jika Perlu)

Jika Anda ingin menggabungkan sheet dalam satu file yang strukturnya sama (misal: data per tahun di sheet terpisah):

**Union (gabung baris):**
- Drag sheet pertama ke canvas
- Drag sheet kedua ke bawah canvas — Tableau otomatis membuat **Union**
- Cocok untuk: data tahunan yang tersimpan di sheet terpisah

**Join (gabung kolom berdasarkan key):**
- Drag tabel pertama ke canvas
- Drag tabel kedua ke samping tabel pertama
- Pilih jenis join: Inner / Left / Right / Full Outer
- Tentukan field join key (misalnya: Provinsi, Tahun)

> ⚠️ **Untuk dashboard ini, kita TIDAK akan join semua tabel jadi satu** — setiap sheet Tableau akan connect ke data source yang berbeda. Ini lebih bersih dan performa lebih baik.

---

## 4. Membersihkan Data (Data Interpreter)

### 4.1 Aktifkan Data Interpreter

Data BPS sering punya format tidak bersih (multi-row header, baris kosong, catatan kaki). Tableau punya fitur otomatis untuk ini:

1. Di Data Source tab, setelah drag sheet ke canvas
2. Di panel kiri, centang **"Use Data Interpreter"**
3. Tableau otomatis mendeteksi dan menghapus header ganda, baris kosong, dan catatan kaki

> Klik **"Review the results"** untuk melihat apa yang diubah Tableau sebelum menerima hasilnya.

### 4.2 Perbaiki Tipe Data

Setelah import, cek ikon di atas setiap kolom di Data Preview:

| Ikon | Tipe | Ubah Jika |
|---|---|---|
| `Abc` | String/Text | Kolom angka terbaca sebagai teks |
| `#` | Number | Benar untuk semua angka |
| `📅` | Date | Pastikan kolom tanggal terdeteksi |
| `🌍` | Geographic | Otomatis untuk nama provinsi/negara |

**Cara mengubah tipe data:**
- Klik ikon di atas nama kolom → pilih tipe yang benar
- Untuk kolom `Tahun`: ubah ke **Number (Whole)**
- Untuk kolom `Tanggal`: ubah ke **Date**
- Untuk kolom `Provinsi`: ubah ke **Geographic Role → State/Province**

### 4.3 Rename Kolom

Klik kanan nama kolom → **Rename** → beri nama yang deskriptif:

```
Growth_YoY     → PDB Growth YoY (%)
PDB_ADHB_T     → PDB ADHB (Triliun Rp)
Miskin_Persen  → Tingkat Kemiskinan (%)
TPT_Persen     → Tingkat Pengangguran (%)
```

### 4.4 Buat Kolom Tanggal dari Tahun + Bulan/Kuartal

Jika data BPS tidak punya kolom tanggal lengkap, buat via **Calculated Field** di Data Source:

```
// Calculated Field: Tanggal_Full (untuk data bulanan)
MAKEDATE([Tahun], [Bulan], 1)

// Calculated Field: Tanggal_Kuartal (untuk data PDB triwulanan)
MAKEDATE([Tahun],
  IF [Kuartal] = "Q1" THEN 1
  ELSEIF [Kuartal] = "Q2" THEN 4
  ELSEIF [Kuartal] = "Q3" THEN 7
  ELSE 10
  END,
1)
```

Klik kanan di area kosong Data Pane → **Create Calculated Field** → nama: `Tanggal_Full` → masukkan formula di atas.

---

## 5. Membuat Kalkulasi & Calculated Fields

Tableau menggunakan **Calculated Fields** sebagai pengganti DAX/formula. Buat semua di sini sebelum mulai visualisasi.

### 5.1 Calculated Fields untuk PDB

**Klik kanan di Data Pane → Create Calculated Field**

```tableau
// ── FIELD: PDB Growth Status ──────────────────────────
// Menentukan apakah pertumbuhan positif/negatif/kritis
IF [PDB Growth YoY (%)] >= 5 THEN "Kuat (≥5%)"
ELSEIF [PDB Growth YoY (%)] >= 3 THEN "Moderat (3-5%)"
ELSEIF [PDB Growth YoY (%)] >= 0 THEN "Lemah (0-3%)"
ELSE "Kontraksi (<0%)"
END
```

```tableau
// ── FIELD: YoY Change Label ────────────────────────────
// Label untuk kartu KPI (dengan tanda panah)
IF [PDB Growth YoY (%)] > 0
THEN "▲ " + STR(ROUND([PDB Growth YoY (%)], 2)) + "%"
ELSE "▼ " + STR(ABS(ROUND([PDB Growth YoY (%)], 2))) + "%"
END
```

### 5.2 Calculated Fields untuk Neraca Perdagangan

```tableau
// ── FIELD: Neraca Perdagangan (USD Miliar) ─────────────
[Ekspor_Total_USD] - [Impor_Total_USD]

// ── FIELD: Status Neraca ───────────────────────────────
IF [Neraca Perdagangan (USD Miliar)] > 0
THEN "Surplus"
ELSE "Defisit"
END

// ── FIELD: Ekspor Growth YoY (%) ──────────────────────
// Ini membutuhkan LOD (Level of Detail) Expression
(SUM([Ekspor_Total_USD]) - LOOKUP(SUM([Ekspor_Total_USD]), -12))
/ ABS(LOOKUP(SUM([Ekspor_Total_USD]), -12)) * 100
```

### 5.3 LOD Expression untuk Perbandingan Nasional vs Provinsi

Level of Detail (LOD) adalah fitur kuat Tableau untuk menghitung agregasi di level berbeda:

```tableau
// ── FIELD: IPM Rata-rata Nasional ─────────────────────
// Fixed LOD: hitung rata-rata tanpa terpengaruh filter Provinsi
{ FIXED [Tahun] : AVG([IPM Score]) }

// ── FIELD: Deviasi IPM dari Nasional ──────────────────
[IPM Score] - [IPM Rata-rata Nasional]

// ── FIELD: Kemiskinan Rata-rata Nasional ──────────────
{ FIXED [Tahun], [Semester] : AVG([Tingkat Kemiskinan (%)]) }
```

### 5.4 Parameter untuk Filter Interaktif

**Klik kanan di Data Pane → Create Parameter**

```
Parameter: Pilih Tahun
Data type: Integer
Current value: 2025
Allowable values: Range
  Minimum: 2015
  Maximum: 2026
  Step size: 1
```

```
Parameter: Pilih Indikator Utama
Data type: String
Allowable values: List
  - "PDB Growth YoY (%)"
  - "Tingkat Inflasi (%)"
  - "Tingkat Pengangguran (%)"
  - "IPM Score"
  - "Tingkat Kemiskinan (%)"
```

---

## 6. Layouting Dashboard — Wireframe & Struktur

### 6.1 Konsep Dashboard: 5 Halaman (Stories)

Kita akan membuat **1 Tableau Story** yang berisi **5 Dashboard** sebagai halaman/chapter:

```
┌─────────────────────────────────────────────────────────────────┐
│  Story: Dashboard Makroekonomi Indonesia                         │
│  ┌──────────┬─────────────┬───────────┬────────────┬──────────┐ │
│  │ Overview │  Ekonomi    │  Harga &  │ Perdagangan│ Sosial & │ │
│  │ Eksekutif│  & PDB      │  Inflasi  │            │ Naker    │ │
│  └──────────┴─────────────┴───────────┴────────────┴──────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### 6.2 Wireframe Dashboard 1: Overview Eksekutif

```
┌─────────────────────────────────────────────────────────────────┐
│  [🇮🇩 Merah] DASHBOARD MAKROEKONOMI INDONESIA          2025 ▾  │
│  ─────────────────────────────────────────────────────────────  │
│                                                                  │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌──────┐ │
│  │ PDB     │  │ Inflasi │  │ TPT     │  │ Ekspor  │  │ IPM  │ │
│  │ 5.11%   │  │ 2.42%   │  │ 4.85%   │  │+$3.32B  │  │75.90 │ │
│  │ ▲Growth │  │ YoY     │  │ Nasional│  │ Neraca  │  │2025  │ │
│  └─────────┘  └─────────┘  └─────────┘  └─────────┘  └──────┘ │
│                                                                  │
│  ┌──────────────────────────────┐  ┌───────────────────────────┐│
│  │  Line Chart:                 │  │  Bar Chart:               ││
│  │  Tren PDB Growth 2015–2025   │  │  Kontribusi Sektor ke PDB ││
│  │  (Kuartalan, YoY %)          │  │  (Waterfall / Bar Horiz.) ││
│  │                              │  │                           ││
│  └──────────────────────────────┘  └───────────────────────────┘│
│                                                                  │
│  ┌────────────────────┐  ┌──────────────┐  ┌───────────────────┐│
│  │  Map: IPM          │  │  Donut:      │  │  Bullet Chart:    ││
│  │  per Provinsi      │  │  PDB per     │  │  Realisasi vs     ││
│  │  (Filled Map)      │  │  Pulau       │  │  Target Indikator ││
│  └────────────────────┘  └──────────────┘  └───────────────────┘│
└─────────────────────────────────────────────────────────────────┘
```

### 6.3 Wireframe Dashboard 2: Pertumbuhan Ekonomi & PDB

```
┌─────────────────────────────────────────────────────────────────┐
│  [Filter: Tahun] [Filter: Lapangan Usaha] [Filter: Pulau]       │
│  ─────────────────────────────────────────────────────────────  │
│                                                                  │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  LINE + AREA Chart: PDB Growth YoY & QoQ (dual axis)      │  │
│  │  Tampilkan pandemi 2020 sebagai shading merah              │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                  │
│  ┌─────────────────────────────┐  ┌────────────────────────────┐│
│  │  Gantt/Waterfall:           │  │  Treemap:                  ││
│  │  Kontribusi Lapangan Usaha  │  │  Ukuran PDB per Sektor     ││
│  │  ke Pertumbuhan (poin %)    │  │  (Size = PDB ADHB)         ││
│  └─────────────────────────────┘  └────────────────────────────┘│
│                                                                  │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  Tabel: PDB per Lapangan Usaha — Perbandingan Antar Tahun  │  │
│  │  (dengan color encoding merah/hijau untuk growth)          │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

### 6.4 Wireframe Dashboard 3: Harga & Inflasi

```
┌─────────────────────────────────────────────────────────────────┐
│  [Filter: Tahun] [Filter: Kelompok Komoditas] [Filter: Provinsi]│
│                                                                  │
│  ┌─────────┐  ┌─────────┐  ┌──────────────────────────────────┐│
│  │ Inflasi │  │ Inflasi │  │  Area Chart:                     ││
│  │ YoY     │  │ MoM     │  │  Dekomposisi Inflasi per         ││
│  │ 2.42%   │  │ 0.33%   │  │  Kelompok (Stacked Area, 2020–26)││
│  └─────────┘  └─────────┘  └──────────────────────────────────┘│
│                                                                  │
│  ┌─────────────────────────────────┐  ┌────────────────────────┐│
│  │  Heatmap:                       │  │  Bar Chart:            ││
│  │  Inflasi per Bulan × Tahun      │  │  Inflasi per Kelompok  ││
│  │  (Warna = tinggi/rendah)        │  │  YoY — Ranking         ││
│  └─────────────────────────────────┘  └────────────────────────┘│
│                                                                  │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  Filled Map: Inflasi per Provinsi (38 provinsi, bulan ini) │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

### 6.5 Wireframe Dashboard 4: Perdagangan Internasional

```
┌─────────────────────────────────────────────────────────────────┐
│  [Filter: Tahun] [Filter: Komoditas] [Filter: Jenis: Migas/NonM]│
│                                                                  │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                       │
│  │ Ekspor   │  │ Impor    │  │ Neraca   │                       │
│  │ $22.53B  │  │ $19.21B  │  │ +$3.32B  │                       │
│  └──────────┘  └──────────┘  └──────────┘                       │
│                                                                  │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  Bar Chart: Ekspor vs Impor per Bulan                      │  │
│  │  + Line: Neraca Perdagangan (secondary axis)               │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                  │
│  ┌─────────────────────────┐  ┌───────────────────────────────┐ │
│  │  Treemap:               │  │  Symbol Map:                  │ │
│  │  Top 15 Komoditas       │  │  Negara Tujuan Ekspor         │ │
│  │  Ekspor Non-Migas       │  │  (Bubble = nilai ekspor)      │ │
│  └─────────────────────────┘  └───────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### 6.6 Wireframe Dashboard 5: Sosial & Ketenagakerjaan

```
┌─────────────────────────────────────────────────────────────────┐
│  [Filter: Tahun] [Filter: Provinsi] [Filter: Periode]           │
│                                                                  │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐        │
│  │ TPT      │  │ IPM      │  │ Kemisk.  │  │ Gini     │        │
│  │ 4.85%    │  │ 75.90    │  │ 7.50%    │  │ 0.38     │        │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘        │
│                                                                  │
│  ┌──────────────────────────┐  ┌──────────────────────────────┐ │
│  │  Filled Map:             │  │  Scatter Plot:               │ │
│  │  TPT per Provinsi        │  │  IPM (X) vs Kemiskinan (Y)  │ │
│  │  (Warna gradient)        │  │  Ukuran = Penduduk Miskin    │ │
│  │                          │  │  Warna = Pulau               │ │
│  └──────────────────────────┘  └──────────────────────────────┘ │
│                                                                  │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  Small Multiples: Tren IPM 5 Provinsi Terpilih            │  │
│  │  (Line chart kecil berdampingan, 2015–2025)               │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 7. Membangun Sheet Visual Satu per Satu

Sebelum merakit dashboard, setiap visual dibuat sebagai **Sheet** terpisah.

### 7.1 Sheet 01 — KPI Card: PDB Growth

**Tujuan:** Menampilkan satu angka besar (nilai terkini)

1. Klik tab **"Sheet 1"** → rename: `KPI_PDB_Growth`
2. Di Data Pane, connect ke `pdb_triwulanan`
3. Drag `PDB Growth YoY (%)` ke **Rows shelf**
4. Ubah aggregasi: klik `SUM(PDB Growth...)` → pilih **Average** atau **Last Value**

**Konfigurasi Marks Card:**
- Ubah mark type dari Automatic ke **Text**
- Drag `PDB Growth YoY (%)` ke **Label** di Marks card
- Format angka: klik kanan → Format → Number → Percentage, 2 decimal

**Hilangkan gridlines & axes:**
- Format → Lines → set semua ke None
- Klik kanan axis → Hide Header

> 💡 **Cara KPI Card di Tableau:** Tableau tidak punya visual "Card" bawaan seperti Power BI. Kita simulasikan dengan Text mark type dan menyembunyikan semua elemen chart lainnya.

### 7.2 Sheet 02 — Line Chart: Tren PDB Growth

1. Rename sheet: `Chart_PDB_Tren`
2. Drag `Tanggal_Kuartal` → **Columns shelf**
3. Drag `PDB Growth YoY (%)` → **Rows shelf**
4. Di Columns: klik `Tanggal_Kuartal` → pilih granularitas **Quarter** (bukan Year)

**Tambahkan Secondary Y-Axis untuk PDB nominal:**
5. Drag `PDB ADHB (Triliun Rp)` → lepas di **area kanan chart** (muncul opsi "Drop field here" di axis kanan)
6. Klik kanan axis kanan → **Synchronize Axis** (jika skala berbeda, jangan sync)
7. Ubah mark type PDB nominal ke **Area** (klik dropdown di Marks card)

**Tambahkan Reference Band (periode pandemi):**
8. Klik kanan area chart → **Add Reference Line**
9. Pilih **Band** → Scope: Table
10. Value: Tanggal Q2 2020 to Q3 2020 → Label: "Pandemi" → Color: merah transparan

**Konfigurasi visual:**
- Line `Growth YoY`: warna merah `#C0392B`, ketebalan 2px
- Area `PDB nominal`: warna biru `#2980B9`, transparansi 70%
- Tooltip: tambahkan semua field relevan

### 7.3 Sheet 03 — Bar Chart: Kontribusi Sektor

1. Rename: `Chart_Kontribusi_Sektor`
2. Drag `Lapangan_Usaha` → **Rows**
3. Drag `Kontribusi_pp` → **Columns**
4. Ubah ke **Horizontal Bar Chart**
5. Sort: klik ikon sort di axis → descending

**Tambahkan color encoding:**
6. Drag `Kontribusi_pp` ke **Color** di Marks card
7. Edit color: klik Color → Edit Colors → pilih palette **Red-Blue Diverging**
8. Set center point di 0 (nilai negatif merah, positif biru/hijau)

**Tambahkan reference line di 0:**
9. Klik kanan axis → Add Reference Line → value: 0 → line style: dashed, hitam

### 7.4 Sheet 04 — Filled Map: IPM per Provinsi

1. Rename: `Map_IPM_Provinsi`
2. Connect ke data source `ipm_provinsi`
3. Double-click field `Provinsi` → Tableau otomatis membuat peta!
4. Drag `IPM Score` ke **Color** di Marks card
5. Ubah mark type ke **Filled Map** (klik dropdown Marks)

**Konfigurasi Color:**
6. Klik Color → Edit Colors
7. Pilih palette: **Sequential** → **Green-Gold** atau custom
8. Set: rendah = merah, tinggi = hijau (sesuai konvensi IPM)

**Tambahkan Label Provinsi:**
9. Drag `Provinsi` ke **Label** di Marks card
10. Format label: font kecil 7-8pt, warna putih/hitam kontras

**Tambahkan Tooltip lengkap:**
11. Klik **Tooltip** di Marks card → edit:
```
Provinsi: <Nama_Provinsi>
IPM: <AGG(IPM Score)>
Harapan Hidup: <AGG(AHH)> tahun
Lama Sekolah: <AGG(RLS)> tahun
Tahun: <Tahun>
```

### 7.5 Sheet 05 — Heatmap: Inflasi per Bulan × Tahun

1. Rename: `Heatmap_Inflasi`
2. Connect ke `inflasi_bulanan`
3. Drag `Tahun` → **Columns** (ubah ke discrete)
4. Drag `Bulan` (nama bulan) → **Rows**
5. Drag `Inflasi_YoY (%)` → **Color** → ubah ke Average
6. Ubah mark type ke **Square**

**Konfigurasi warna:**
7. Klik Color → Edit Colors → pilih **Temperature Diverging**
8. Set: hijau (inflasi rendah/deflasi) → putih (netral 0) → merah (inflasi tinggi)
9. Tambahkan border: Color → Border → abu-abu

**Tambahkan label nilai:**
10. Drag `Inflasi_YoY (%)` ke **Label** → format: 1 decimal

> 💡 **Tips:** Heatmap adalah salah satu visual paling informatif untuk data inflasi karena langsung terlihat pola musiman (bulan tertentu selalu tinggi/rendah) dan anomali (pandemi 2020).

### 7.6 Sheet 06 — Scatter Plot: IPM vs Kemiskinan

1. Rename: `Scatter_IPM_Kemiskinan`
2. Drag `IPM Score` → **Columns**
3. Drag `Tingkat Kemiskinan (%)` → **Rows**
4. Drag `Provinsi` → **Detail** di Marks card (agar tiap titik = satu provinsi)
5. Drag `Jumlah_Miskin_Ribu` → **Size** (provinsi besar = titik besar)
6. Drag `Pulau` → **Color**

**Tambahkan quadrant lines (garis referensi):**
7. Klik kanan axis X → Add Reference Line → value: `AVG([IPM Score])` → label: "Rata-rata IPM"
8. Klik kanan axis Y → Add Reference Line → value: `AVG([Tingkat Kemiskinan (%)])` → label: "Rata-rata Kemiskinan"

**Tambahkan Label Provinsi:**
9. Klik **Label** di Marks → Show mark labels: **Never** (agar tidak overload)
10. Aktifkan **"Allow labels to overlap"** hanya untuk provinsi yang dipilih via filter

**Trend line:**
11. Klik kanan di canvas → **Trend Lines** → Show Trend Lines → pilih **Linear**

### 7.7 Sheet 07 — Bar Chart: Neraca Perdagangan Bulanan

1. Rename: `Chart_Neraca_Dagang`
2. Connect ke `ekspor_impor`
3. Drag `Tanggal` → **Columns** (granularitas Month)
4. Drag `Neraca Perdagangan (USD Miliar)` (calculated field) → **Rows**
5. Mark type: **Bar**

**Warna kondisional (merah vs hijau):**
6. Drag `Status Neraca` (calculated field) → **Color** di Marks
7. Edit Colors: "Surplus" = hijau `#27AE60`, "Defisit" = merah `#C0392B`

**Tambahkan Ekspor & Impor sebagai line overlay:**
8. Buat **Dual Axis**: Drag `Ekspor_Total_USD` ke canvas (di area drop Rows kedua)
9. Klik kanan axis kanan → Synchronize Axis
10. Di Marks card Ekspor: ubah ke **Line**, warna biru
11. Di Marks card Impor: ubah ke **Line**, warna oranye

### 7.8 Sheet 08 — Treemap: Komoditas Ekspor

1. Rename: `Treemap_Komoditas_Ekspor`
2. Drag `Komoditas` → **Color** dan **Label**
3. Drag `Ekspor_Total_USD` → **Size**
4. Ubah mark type ke **Square**
5. Klik Size → Edit sizes → ubah ke largest

**Format label di dalam kotak:**
6. Klik Label → edit: `<Komoditas>\n$<AGG(Ekspor_Total_USD)>`
7. Ukuran font: 10pt, warna: putih

### 7.9 Sheet 09 — Small Multiples: Tren IPM per Provinsi

1. Rename: `SmallMultiples_IPM`
2. Drag `Tahun` → **Columns**
3. Drag `IPM Score` → **Rows**
4. Drag `Provinsi` → **Color** (pilih 5-10 provinsi via filter)
5. Untuk small multiples sejati: drag `Provinsi` ke **Rows** juga (akan membuat 38 baris chart kecil)

**Tips small multiples:**
6. Filter hanya 5-10 provinsi terpilih untuk readability
7. Kecilkan row height: Format → Row Banding
8. Sembunyikan axis label agar lebih compact: klik kanan → Hide

---

## 8. Merakit Dashboard di Dashboard Canvas

### 8.1 Buat Dashboard Baru

Klik tombol **"New Dashboard"** (ikon di tab bar bawah, gambar kotak kecil-kecil).

**Atur ukuran canvas:**
- Di panel kiri bawah Dashboard, klik **Size**
- Pilih **Fixed size** → **Generic Desktop (1366 × 768)** atau custom **1920 × 1080**

### 8.2 Anatomi Dashboard Canvas

```
┌─────────────────────────────────────────────────────────────┐
│  LAYOUT PANEL (kiri)    │    DASHBOARD CANVAS (tengah)      │
│  ─────────────────       │    ────────────────────────────── │
│  📊 Sheets:              │    ← Drag sheets ke sini          │
│    KPI_PDB_Growth        │                                   │
│    Chart_PDB_Tren        │    ┌──────────────────────────┐  │
│    Chart_Kontribusi      │    │  KPI Cards (Horizontal)  │  │
│    Map_IPM               │    └──────────────────────────┘  │
│    Heatmap_Inflasi       │    ┌────────────┐ ┌────────────┐  │
│    ...                   │    │ Chart PDB  │ │ Bar Sektor │  │
│                           │    └────────────┘ └────────────┘  │
│  📦 Objects:             │    ┌────────────────────────────┐  │
│    Text                  │    │      Filled Map            │  │
│    Image                 │    └────────────────────────────┘  │
│    Blank                 │                                   │
│    Navigation            │                                   │
│    Extension             │                                   │
└─────────────────────────────────────────────────────────────┘
```

### 8.3 Drag Sheet ke Canvas

1. Dari panel **Sheets** (kiri bawah), drag `KPI_PDB_Growth` ke canvas
2. Tableau menampilkan **blue guide lines** saat Anda drag — lepas untuk posisikan
3. Ulangi untuk semua sheet yang dibutuhkan

**Kontrol tata letak:**
- **Tiled vs Floating:** Secara default, semua object adalah **Tiled** (otomatis tidak overlap). Pilih **Floating** untuk posisi bebas (klik kanan object → Floating)
- **Container Horizontal/Vertical:** Di panel Objects, drag "Horizontal" atau "Vertical" container ke canvas untuk mengatur grup layout. Ini seperti flexbox di CSS.

### 8.4 Gunakan Layout Container untuk Struktur Rapi

**Cara membuat layout bertingkat:**

```
Dashboard Layout:
└── Vertical Container (seluruh halaman)
    ├── Horizontal Container (header/topbar)
    │   ├── Text Object (judul dashboard)
    │   ├── Blank (spacer)
    │   └── Filter Slicer (tahun)
    ├── Horizontal Container (KPI row)
    │   ├── KPI_PDB
    │   ├── KPI_Inflasi
    │   ├── KPI_TPT
    │   ├── KPI_Ekspor
    │   └── KPI_IPM
    ├── Horizontal Container (main charts)
    │   ├── Chart_PDB_Tren (2/3 lebar)
    │   └── Bar_Sektor (1/3 lebar)
    └── Horizontal Container (bottom row)
        ├── Map_IPM (1/2)
        ├── Donut_Pulau (1/4)
        └── Bullet_Chart (1/4)
```

**Cara mengatur proporsi container:**
- Klik container → pojok kanan atas muncul ikon **▼** → klik → pilih **Select Layout Container**
- Klik dan drag border antar object untuk ubah ukuran relatif

### 8.5 Tambahkan Header/Topbar

1. Dari Objects, drag **"Text"** ke bagian atas canvas
2. Double-click → ketik teks header:
```
DASHBOARD MAKROEKONOMI INDONESIA
Sumber: BPS · Kemendag · Bank Indonesia | Diperbarui: [tanggal]
```
3. Format: font besar bold untuk judul, kecil abu untuk subtitle
4. Background: warna gelap (dark mode) atau putih bersih (light mode)

### 8.6 Tambahkan Filter Global

1. Klik satu sheet di canvas (misal: KPI_PDB)
2. Klik ikon dropdown kecil di pojok kanan atas sheet → **Filters** → tambahkan `dim_Waktu.Tahun`
3. Klik kanan filter yang muncul di canvas → **Apply to Worksheets** → **All Using This Data Source**
4. Format filter: klik kanan → **Edit Filter** → tampilkan sebagai Slider atau Dropdown

### 8.7 Tambahkan Navigasi Antar Dashboard

1. Dari Objects, drag **"Navigation"** ke area header
2. Klik kanan → Edit Button:
   - Navigate to: pilih dashboard tujuan
   - Style: Text atau Image
   - Text: "→ Pertumbuhan Ekonomi"

Buat tombol kembali di setiap dashboard agar pengguna bisa navigasi dua arah.

---

## 9. Design System: Warna, Font & Tema

### 9.1 Palet Warna Resmi

Gunakan palet ini konsisten di semua 5 dashboard:

| Peran | Warna | Hex | Penggunaan |
|---|---|---|---|
| **Primer** | Merah Indonesia | `#C0392B` | Highlight, border, aksen utama |
| **Positif** | Hijau | `#27AE60` | Nilai naik, surplus, baik |
| **Negatif** | Merah Muda | `#E74C3C` | Nilai turun, defisit, buruk |
| **Peringatan** | Oranye | `#F39C12` | Nilai moderat, perlu perhatian |
| **Informasi** | Biru | `#2980B9` | Data sekunder, referensi |
| **Aksen** | Emas | `#D4AC0D` | Header, judul, highlight premium |
| **BG Gelap** | Hitam Hangat | `#1A1612` | Background dashboard |
| **Panel** | Arang | `#242424` | Background chart |
| **Teks Utama** | Putih Krem | `#E8E0D0` | Teks di atas background gelap |
| **Teks Sekunder** | Abu | `#888888` | Label, keterangan |
| **Border** | Abu Gelap | `#3A3A3A` | Garis pemisah panel |

### 9.2 Cara Apply Tema di Tableau

Tableau tidak memiliki tema JSON seperti Power BI, tapi kita bisa customize lewat:

**Formatting global (berlaku untuk semua sheet):**
1. Klik menu **Format** → **Workbook**
2. Set:
   - Default Font: **Segoe UI** atau **Source Sans Pro** size 11
   - Color: set semua teks ke `#E8E0D0`
   - Background: `#1A1612`

**Formatting per sheet:**
1. Menu **Format** → **Shading**:
   - Worksheet: `#242424`
   - Default: `#1A1612`
2. Menu **Format** → **Borders** → set semua ke None atau warna `#3A3A3A`
3. Menu **Format** → **Lines** → semua grid line: None

### 9.3 Preferensi Font

| Elemen | Font | Ukuran | Weight |
|---|---|---|---|
| Judul Dashboard | Segoe UI Semibold | 22–28pt | Bold |
| Judul Sheet | Segoe UI | 14pt | Semibold |
| Label Axis | Segoe UI | 9pt | Regular |
| Angka KPI | Segoe UI | 32–48pt | Bold |
| Tooltip | Segoe UI | 10pt | Regular |
| Tabel | Segoe UI | 11pt | Regular |

### 9.4 Buat Custom Color Palette di Tableau

1. Tutup Tableau
2. Buka file: `Documents/My Tableau Repository/Preferences.tps`
3. Edit dengan teks editor, tambahkan di dalam tag `<preferences>`:

```xml
<color-palette name="Indonesia Makro Dark" type="ordered-diverging">
  <color>#C0392B</color>
  <color>#E74C3C</color>
  <color>#F39C12</color>
  <color>#F1C40F</color>
  <color>#FFFFFF</color>
  <color>#A8D8A8</color>
  <color>#27AE60</color>
  <color>#1E8449</color>
</color-palette>

<color-palette name="Indonesia Categorical" type="regular">
  <color>#C0392B</color>
  <color>#2980B9</color>
  <color>#27AE60</color>
  <color>#F39C12</color>
  <color>#8E44AD</color>
  <color>#1ABC9C</color>
  <color>#D4AC0D</color>
  <color>#E74C3C</color>
</color-palette>
```

4. Simpan → buka kembali Tableau → palet baru tersedia di Color picker

---

## 10. Interaktivitas: Filter, Action & Parameter

### 10.1 Filter Biasa

**Cara menambahkan filter di sheet:**
1. Drag field ke **Filter Shelf**
2. Klik kanan filter → **Show Filter** (muncul di kanan canvas)
3. Klik kanan filter yang tampil → pilih tipe tampilan:
   - **Single Value (dropdown):** untuk pilih satu nilai
   - **Multiple Values (list):** untuk pilih banyak nilai
   - **Slider:** untuk angka/tanggal

**Agar filter berlaku di seluruh dashboard:**
1. Klik kanan filter → **Apply to Worksheets**
2. Pilih: **All Using This Data Source** atau **Selected Worksheets**

### 10.2 Dashboard Actions — Fitur Paling Powerful

Actions memungkinkan interaksi antar visual. Saat klik satu visual, visual lain ikut terfilter.

**Cara membuat Action:**
1. Di Dashboard, klik menu **Dashboard** → **Actions**
2. Klik **"Add Action"** → pilih tipe:
   - **Filter Action:** klik provinsi di map → semua chart lain filter ke provinsi itu
   - **Highlight Action:** hover provinsi → highlight sama di semua chart
   - **URL Action:** klik titik data → buka URL (misalnya ke website BPS)
   - **Go to Sheet Action:** klik → navigate ke halaman/dashboard lain

**Contoh setup Filter Action (Map → semua chart):**
```
Name: Klik Provinsi → Filter Semua
Source: Map_IPM_Provinsi
Run Action On: Select
Target Sheets: Semua Sheet di Dashboard
Target Filters: Provinsi → Provinsi
Clearing the selection: Show all values
```

**Contoh Highlight Action:**
```
Name: Hover Provinsi → Highlight
Source: Map_IPM_Provinsi
Run Action On: Hover
Target Fields: Provinsi
```

### 10.3 Parameter Control

Parameter adalah nilai yang bisa diubah pengguna untuk mengontrol kalkulasi.

**Cara menampilkan Parameter Control:**
1. Klik kanan Parameter di Data Pane → **Show Parameter Control**
2. Muncul slider/dropdown di kanan canvas

**Contoh penggunaan Parameter untuk Dynamic Chart:**

Buat parameter `Pilih Indikator` (string: PDB, Inflasi, TPT, IPM, Kemiskinan).

Buat Calculated Field yang menggunakannya:
```tableau
// Field: Nilai Indikator Terpilih
CASE [Pilih Indikator]
  WHEN "PDB Growth YoY (%)" THEN [PDB Growth YoY (%)]
  WHEN "Tingkat Inflasi (%)" THEN [Inflasi_YoY (%)]
  WHEN "Tingkat Pengangguran (%)" THEN [Tingkat Pengangguran (%)]
  WHEN "IPM Score" THEN [IPM Score]
  WHEN "Tingkat Kemiskinan (%)" THEN [Tingkat Kemiskinan (%)]
END
```

Gunakan `[Nilai Indikator Terpilih]` sebagai Measure di chart. Sekarang satu chart bisa menampilkan 5 indikator berbeda berdasarkan pilihan user.

### 10.4 Tooltip Sheet (Viz in Tooltip)

Fitur ini memungkinkan chart mini muncul saat hover — sangat mengesankan secara visual.

1. Buat sheet kecil khusus tooltip, misalnya: `Tooltip_PDB_MiniChart`
   - Isi: line chart kecil PDB 5 tahun terakhir untuk provinsi yang di-hover
2. Di sheet utama (misalnya map), klik **Tooltip** di Marks card
3. Klik **"Insert"** → **Sheets** → pilih `Tooltip_PDB_MiniChart`
4. Atur ukuran: `maxwidth=300, maxheight=200`

Contoh tooltip dengan embedded chart:
```
<Nama_Provinsi>
IPM: <AGG(IPM Score)> | Kemiskinan: <AGG(Tingkat Kemiskinan (%))>%

Tren IPM 5 Tahun:
<Sheet name="Tooltip_PDB_MiniChart" maxwidth="300" maxheight="180" filter="Provinsi">
```

---

## 11. Publish ke Tableau Public / Tableau Server

### 11.1 Publish ke Tableau Public (Gratis)

1. Pastikan sudah login: menu **Server** → **Tableau Public** → **Sign In**
2. Menu **Server** → **Tableau Public** → **Save to Tableau Public**
3. Beri nama workbook: `"Dashboard Makroekonomi Indonesia 2025"`
4. Klik **Save**
5. Browser otomatis terbuka menampilkan dashboard Anda di: `public.tableau.com/app/profile/[username]`

> ⚠️ **Penting:** Tableau Public menyimpan data **secara publik**. Jangan upload data sensitif atau rahasia.

### 11.2 Mendapatkan Embed Code

Setelah publish:
1. Di halaman dashboard di Tableau Public
2. Klik ikon **Share** (bawah kanan) → **Copy Embed Code**
3. Hasilnya adalah kode `<script>` yang bisa ditempel ke website:

```html
<div class='tableauPlaceholder' style='width: 1200px; height: 720px;'>
  <object class='tableauViz' width='1200' height='720' style='display:none;'>
    <param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' />
    <param name='embed_code_version' value='3' />
    <param name='name' value='DashboardMakroekonomiIndonesia/Overview' />
    <param name='tabs' value='yes' />
    <param name='toolbar' value='yes' />
  </object>
</div>
<script type='text/javascript'>
  var divElement = document.getElementById('viz...');
  var vizElement = divElement.getElementsByTagName('object')[0];
  vizElement.style.width='100%';
  var scriptElement = document.createElement('script');
  scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';
  vizElement.parentNode.insertBefore(scriptElement, vizElement);
</script>
```

### 11.3 Update Data Secara Berkala

Karena Tableau Public tidak mendukung **scheduled refresh** untuk file lokal, ada beberapa pendekatan:

**Opsi A: Update manual** (paling sederhana)
1. Update file XLSX di komputer lokal
2. Di Tableau Desktop: klik kanan Data Source → **Refresh**
3. Republish ke Tableau Public

**Opsi B: Google Sheets sebagai data source** (semi-otomatis)
1. Upload data ke Google Sheets
2. Di Tableau: Connect → Google Sheets (butuh Tableau Desktop)
3. Data otomatis update setiap kali Sheets diupdate

**Opsi C: Tableau Prep + scheduled script**
1. Buat Tableau Prep Flow untuk transform data otomatis
2. Jalankan via script Python/R yang dijadwalkan (cron/Task Scheduler)
3. Output ke file yang sudah terkoneksi ke Tableau

---

## 12. Tips & Best Practices

### 12.1 Performa

| Tips | Detail |
|---|---|
| **Filter di data source** | Tambahkan filter context (klik kanan filter → Add to Context) untuk mempercepat kalkulasi |
| **Hindari EXCLUDE LOD di published datasource** | Ganti dengan FIXED LOD yang lebih efisien |
| **Batasi baris data** | Gunakan filter tanggal untuk hanya load 5-10 tahun terakhir saat development |
| **Extract vs Live** | Gunakan **Extract** (.hyper) bukan Live connection untuk data XLSX lokal — jauh lebih cepat |

**Cara buat Extract:**
Di Data Source tab → klik **"Extract"** di pojok kanan atas (bukan Live) → Save.

### 12.2 UX & Desain

| Tips | Detail |
|---|---|
| **Satu pertanyaan per halaman** | Setiap dashboard harus menjawab satu pertanyaan bisnis. Jangan terlalu banyak visual |
| **Hierarki visual** | Visual paling penting = paling besar. KPI di atas, detail di bawah |
| **Gunakan warna dengan hemat** | Maksimal 5-6 warna berbeda per dashboard. Terlalu banyak warna = membingungkan |
| **Label langsung di chart** | Hindari legend jika bisa. Label langsung di garis/bar lebih mudah dibaca |
| **White space adalah teman** | Jangan paksakan terlalu banyak visual. Breathing room = dashboard lebih mudah dibaca |
| **Mobile view** | Di Dashboard panel: klik **Device Preview** → set tampilan untuk Phone, Tablet |

### 12.3 Kalkulasi yang Sering Diperlukan

```tableau
// Perubahan Year-over-Year (YoY)
(SUM([Nilai]) - LOOKUP(SUM([Nilai]), -12)) / ABS(LOOKUP(SUM([Nilai]), -12))

// Running Total (kumulatif)
RUNNING_SUM(SUM([Nilai]))

// Moving Average 3 bulan
WINDOW_AVG(SUM([Nilai]), -2, 0)

// Rank provinsi berdasarkan IPM
RANK(AVG([IPM Score]))

// Conditional color (untuk parameter warna)
IF [Nilai] > 0 THEN "#27AE60"
ELSEIF [Nilai] < 0 THEN "#C0392B"
ELSE "#F39C12"
END

// Persentase dari total
SUM([Nilai]) / TOTAL(SUM([Nilai]))

// Label dengan satuan (Triliun Rupiah)
STR(ROUND(SUM([PDB_ADHB_T]) / 1000, 1)) + " T"
```

### 12.4 Shortcut Tableau yang Wajib Diingat

| Shortcut | Fungsi |
|---|---|
| `Ctrl + Z` | Undo |
| `Ctrl + Shift + Z` | Redo |
| `F5` | Refresh data |
| `Ctrl + E` | Extract data |
| `Ctrl + D` | Edit data source |
| `Alt + Drag` | Move object (floating mode) |
| `Ctrl + Click` | Select multiple fields |
| `Hold Shift + Drag measure ke axis` | Buat dual axis |

### 12.5 Checklist Sebelum Publish

```
[ ] Semua sheet sudah diberi nama deskriptif
[ ] Tooltip sudah dikonfigurasi di semua visual
[ ] Filter sudah diset ke "All using this data source"
[ ] Actions antar visual sudah diuji
[ ] Mobile view sudah dicek
[ ] Tidak ada data sensitif/pribadi
[ ] Judul dashboard sudah jelas dan informatif
[ ] Sumber data sudah dicantumkan (text object di footer)
[ ] Warna konsisten menggunakan palet yang sudah ditentukan
[ ] Semua angka sudah diformat (ribuan separator, satuan, desimal)
[ ] Dashboard sudah diuji di browser (bukan hanya di Desktop)
```

---

## Referensi & Sumber Lanjutan

| Sumber | URL | Konten |
|---|---|---|
| BPS Open Data | bps.go.id | Dataset resmi semua indikator |
| Databoks | databoks.katadata.co.id | Visualisasi & unduh data BPS |
| Kemendag Satu Data | satudata.kemendag.go.id | Ekspor-impor per komoditas |
| Bank Indonesia SEKI | bi.go.id/id/statistik/seki | Data keuangan & moneter |
| Tableau Help | help.tableau.com | Dokumentasi resmi Tableau |
| Tableau Public Gallery | public.tableau.com/gallery | Inspirasi dashboard dunia |
| Tableau LOD Explained | tableau.com/about/blog/LOD | Panduan Level of Detail |

---

> **Catatan:** Data yang digunakan dalam tutorial ini bersumber dari laporan resmi pemerintah Republik Indonesia. Selalu cek pembaruan terbaru di situs BPS dan Kemendag sebelum publish dashboard. Angka yang ditampilkan mencerminkan kondisi per Mei 2026.

---

*Tutorial ini dibuat sebagai panduan lengkap membangun Dashboard Makroekonomi Indonesia di Tableau. Seluruh data bersifat publik dan bebas digunakan untuk keperluan analisis, riset, dan edukasi.*
