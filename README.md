# COVID-19 Time Series Data Analysis and Visualization

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![License](https://img.shields.io/badge/Academic-Project-green.svg)]()

Repositori ini berisi proyek analisis dan visualisasi data runtun waktu (*time series data analysis and visualization*) terhadap dinamika pandemi global COVID-19 pada gelombang awal (22 Januari 2020 – 27 Juli 2020). Proyek ini disusun untuk memenuhi tugas **Ujian Tengah Semester (UTS) Mata Kuliah Visualisasi Data**.

---

## Daftar Isi
- [Daftar Isi](#daftar-isi)
- [Ringkasan Proyek](#ringkasan-proyek)
- [Sumber Dataset](#sumber-dataset)
- [Struktur Direktori](#struktur-direktori)
- [Panduan Setup Cepat & Menjalankan Proyek](#panduan-setup-cepat--menjalankan-proyek)
  - [Metode 1: Menggunakan Visual Studio Code (Paling Direkomendasikan)](#metode-1-menggunakan-visual-studio-code-paling-direkomendasikan)
  - [Metode 2: Menggunakan Jupyter Notebook Classic / JupyterLab](#metode-2-menggunakan-jupyter-notebook-classic--jupyterlab)
- [Rincian Visualisasi yang Diimplementasikan](#rincian-visualisasi-yang-diimplementasikan)
  - [1. Line Plot (Kasus Kumulatif Global)](#1-line-plot-kasus-kumulatif-global)
  - [2. Area Plot (Volume Kasus Harian & Stacked WHO Region)](#2-area-plot-volume-kasus-harian--stacked-who-region)
  - [3. Multiple Time Series dengan Direct Labeling](#3-multiple-time-series-dengan-direct-labeling)
  - [4. Calendar Heatmap (Pola Musiman Intra-Mingguan)](#4-calendar-heatmap-pola-musiman-intra-mingguan)
  - [5. Trend Analysis (Moving Average 7-Day & 30-Day SMA)](#5-trend-analysis-moving-average-7-day--30-day-sma)
- [Ringkasan Temuan Utama Berbasis Data](#ringkasan-temuan-utama-berbasis-data)
- [Keterbatasan Analisis](#keterbatasan-analisis)
- [Troubleshooting & FAQ](#troubleshooting--faq)
- [Catatan Pengumpulan Tugas](#catatan-pengumpulan-tugas)

---

## Ringkasan Proyek

Proyek ini mengeksplorasi dataset epidemiologi global COVID-19 dengan fokus pada integritas data, dekomposisi derau (*noise reduction*), serta penerapan teknik visualisasi runtun waktu yang efektif. 

**Objektif Utama:**
1. Membedakan secara tegas antara metrik **stok kumulatif** (*cumulative confirmed cases*) dan **arus harian** (*daily new incidence*).
2. Menangani anomali data seperti nilai harian negatif hasil revisi retrospektif melalui metode *clipping* ke 0 yang terjustifikasi.
3. Mengimplementasikan minimal empat tipe visualisasi *time series* dengan standar estetika akademis tinggi (*data-ink ratio*, pelabelan langsung, tipografi terstandarisasi).
4. Menganalisis tren menggunakan *Simple Moving Average* (SMA) dengan jendela 7 hari dan 30 hari serta mengevaluasi *trade-off* antara *smoothing* dan *lag*.

> **Catatan metodologis:** Analisis ini bersifat deskriptif-eksploratif berbasis data observasional. Pola pertumbuhan berlipat ganda yang semakin cepat (*accelerating doubling time*) terlihat dari data, namun penyebab naik/turunnya kasus di suatu negara tidak dapat ditentukan hanya dari dataset ini.

---

## Sumber Dataset

Dataset yang digunakan bersumber dari repositori publik Kaggle:
- **Nama Dataset:** [Corona Virus Report](https://www.kaggle.com/datasets/imdevskp/corona-virus-report/data)
- **Kurator:** Devakumar K.P. (`imdevskp`)
- **Sumber Asli:** Johns Hopkins University Center for Systems Science and Engineering (JHU CSSE)
- **Rentang Waktu Observasi:** 22 Januari 2020 – 27 Juli 2020 (187 hari / 188 baris observasi)

**File CSV yang Digunakan:**
1. `day_wise.csv` (14 KB) : Agregasi total tingkat global harian (188 baris × 12 kolom).
2. `full_grouped.csv` (1.814 KB) : Data harian tingkat negara disertai klasifikasi 6 Wilayah WHO (35.156 baris × 10 kolom).

---

## Struktur Direktori

```text
MIDTERM/
├── data/
│   ├── day_wise.csv                     # Dataset primer tingkat global
│   └── full_grouped.csv                 # Dataset primer tingkat negara & WHO region
├── figures/                             # Folder keluaran seluruh grafik resolusi tinggi (150 DPI)
│   ├── eda_distribution.png            # Distribusi statistik & boxplot bulanan
│   ├── eda_top10_countries.png         # Bar chart 10 negara kasus kumulatif teratas
│   ├── eda_who_regions.png             # Proporsi regional (bar chart & pie chart)
│   ├── viz1_line_plot.png              # Visualisasi 1: Line plot kumulatif + milestone
│   ├── viz2a_area_plot.png             # Visualisasi 2a: Single area plot kasus harian
│   ├── viz2b_stacked_area.png          # Visualisasi 2b: Stacked area plot per WHO region
│   ├── viz3_multi_ts.png               # Visualisasi 3: Multiple time series direct labeling
│   ├── viz4_calendar_heatmap.png       # Visualisasi 4: Calendar heatmap siklus mingguan
│   └── viz5_moving_average.png         # Visualisasi 5: Trend analysis 7MA vs 30MA
├── covid19_time_series_visualization.ipynb  # Jupyter Notebook UTAMA tugas UTS
├── requirements.txt                     # Daftar pustaka Python yang dibutuhkan
└── README.md                            # Dokumentasi lengkap proyek
```

---

## Panduan Setup & Menjalankan Proyek

### Prasyarat

- **Python 3.9+** — unduh dari [python.org](https://www.python.org/downloads/)
- **Jupyter** — bisa via [VS Code](https://code.visualstudio.com/) (paling mudah) atau Jupyter Classic

> **Notebook ini memiliki sel auto-install di bagian atas.**  
> Ketika dijalankan pada komputer baru, sel pertama kode akan secara otomatis memeriksa dan menginstal semua pustaka yang belum ada. Anda **tidak perlu** menjalankan `pip install` secara manual.

---

### Langkah Minimal (3 Langkah)

```
1. Pastikan Python 3.9+ terinstal
2. Letakkan folder data/ berisi file CSV di samping notebook
3. Buka notebook dan klik Run All
```

---

### Metode 1: VS Code (Paling Direkomendasikan)

1. **Buka folder proyek di VS Code:**  
   Pilih menu **File > Open Folder...** → arahkan ke folder `MIDTERM`.

2. **Buka file notebook:**  
   Klik `covid19_time_series_visualization.ipynb` di panel kiri.

3. **Pilih Python interpreter:**  
   Klik **Select Kernel** (pojok kanan atas) → **Python Environments...** → pilih Python 3.9+ yang terinstal di komputer Anda.  
   *(Tidak perlu membuat virtual environment—interpreter bawaan Python sudah cukup).*

4. **Jalankan notebook:**  
   Klik tombol **▶▶ Run All** di bilah atas notebook.  
   Sel `AUTO-SETUP` di awal akan otomatis menginstal pustaka yang belum ada.

---

### Metode 2: Jupyter Notebook Classic / JupyterLab

```bash
# 1. Buka terminal dan masuk ke folder proyek
cd "MIDTERM"

# 2. Instal Jupyter jika belum ada (hanya sekali)
pip install notebook

# 3. Jalankan server Jupyter
jupyter notebook
```

Browser akan terbuka secara otomatis. Klik `covid19_time_series_visualization.ipynb`, lalu pilih menu **Kernel > Restart & Run All**.  
Sel `AUTO-SETUP` akan menginstal sisa dependensi secara otomatis.

---

### Catatan Khusus: Komputer Tanpa Koneksi Internet

Jika komputer yang digunakan tidak memiliki akses internet, instal dependensi terlebih dahulu secara manual sebelum membuka Jupyter:

```bash
pip install pandas numpy matplotlib seaborn ipykernel notebook
```

---

## Rincian Visualisasi yang Diimplementasikan

Notebook ini memenuhi seluruh persyaratan penugasan dengan 4 tipe visualisasi wajib dan 1 analisis tren:

### 1. Line Plot (Kasus Kumulatif Global)
- **Tujuan:** Menampilkan arah dan laju kesinambungan pertumbuhan total kasus konfirmasi global.
- **Fitur Desain:**
  - Garis tunggal kontras tinggi dengan sumbu Y diformat dalam satuan Juta (*Millions* / M).
  - Garis referensi putus-putus (*milestone*) pada ambang 1M, 5M, dan 10M kasus.
  - Anotasi titik akhir (*endpoint callout*) yang menampilkan tanggal dan nilai akumulasi akhir ($16,48$ Juta).

### 2. Area Plot (Volume Kasus Harian & Stacked WHO Region)
- **7.2a (Single Area Plot):** Menampilkan arsiran ruang di bawah kurva kasus baru harian mentah (*raw counts*) yang dipadukan dengan kurva *7-Day Moving Average* serta anotasi hari puncak absolut ($282.756$ kasus).
- **7.2b (Stacked Area Plot):** Memvisualisasikan breakdown kontribusi harian menurut 6 *WHO Region*. Teknik *stacking* ini sah dan aditif karena setiap negara secara eksklusif hanya terdaftar di satu wilayah WHO.

### 3. Multiple Time Series dengan Direct Labeling
- **Tujuan:** Membandingkan lintasan epidemiologi 6 negara representatif: **US, Brazil, India, Russia, Germany, dan South Africa**.
- **Fitur Khusus (*No Conventional Legend*):**
  - Tidak menggunakan kotak legenda terpisah (*legend box*) demi menghilangkan beban bolak-balik pandangan (*split-attention effect*).
  - Nama negara diletakkan langsung di ujung kanan masing-masing kurva (*direct labeling*).
  - **Collision-Avoidance Algorithm:** Mencegah tumpang-tindih teks antara Brazil ($46.247$) dan India ($46.391$) yang berimpit pada hari terakhir pengamatan, dilengkapi garis penunjuk (*dotted leader lines*).

### 4. Calendar Heatmap (Pola Musiman Intra-Mingguan)
- **Tujuan:** Mereorganisasi deret waktu linear ke dalam matriks dua dimensi berstruktur **ISO Calendar Week (baris) × Day of Week (kolom)**.
- **Normalisasi:** Nilai dinormalisasi per minggu (nilai = kasus harian / rata-rata minggu itu) sehingga pola intra-mingguan terlihat terlepas dari tren naik.
- **Skema Warna:** Palet diverging `RdBu_r` berpusat di 1,0 (biru = di bawah rata-rata minggu, merah = di atas rata-rata minggu).
- **Analisis Pola:** Menguantifikasi fenomena *weekend reporting deficit* menggunakan rasio ternormalisasi pada agregat data global.

### 5. Trend Analysis (Moving Average 7-Day & 30-Day SMA)
- **Metodologi:** Menerapkan *Simple Moving Average* trailing/kausal secara eksklusif pada metrik penambahan kasus harian (`New cases`).
- **Evaluasi Parameter:**
  - **7-Day SMA:** Optimal untuk *real-time surveillance*, mengeliminasi siklus mingguan dengan *lag* rendah ($\approx 3-4$ hari).
  - **30-Day SMA:** Optimal untuk evaluasi makro gelombang, menghasilkan kurva sangat mulus namun memiliki *lag* tinggi ($\approx 15$ hari).
  - Evaluasi efek parameter `min_periods=1` pada baris-baris awal pengamatan.

---

## Ringkasan Temuan Utama Berbasis Data

Seluruh hasil berikut dihitung secara langsung (*programmatically*) dari dataset:

| Metrik Epidemiologis | Nilai Temuan Empiris | Interpretasi Analitis |
|:---|:---:|:---|
| **Total Kasus Kumulatif Global** | **16.480.485 kasus** | Akumulasi tercatat hingga 27 Juli 2020 di 187 negara. |
| **Total Kematian Global** | **654.036 jiwa** | Menghasilkan *naive* Case Fatality Rate (CFR) global sebesar **3,97%** (tidak memperhitungkan kapasitas testing). |
| **Waktu Tempuh Milestone** | **0 ke 1 Juta: 71 hari**<br>**5 Juta ke 10 Juta: 38 hari** | Laju penggandaan yang semakin cepat menunjukkan pertumbuhan berlipat ganda yang semakin cepat selama periode observasi. |
| **Puncak Kasus Baru Harian** | **282.756 kasus** | Terjadi pada **23 Juli 2020** (7-Day MA pada puncak: 242.581 kasus). |
| **Top 3 Negara Teratas** | **US (26,03%)**, Brazil, India | Ketiga negara mencakup **49,83%** (hampir separuh) dari total beban kasus dunia. |
| **Wilayah WHO Terbesar** | **Americas (53,59%)** | Berdasarkan data viz2b, Americas menggantikan posisi Eropa sebagai kontributor terbesar sejak Mei 2020. |
| **Penurunan Kasus Jerman** | **Penurunan 90,73%** dari puncak | 7-Day MA turun dari puncak 5.837 (April) menjadi hanya 541 kasus (Juli); penyebab penurunan tidak dapat ditentukan hanya dari data ini. |
| **Defisit Akhir Pekan Global** | **2,07% (raw)** | Rata-rata akhir pekan (86.467) vs hari kerja (88.296). Teredam oleh agregasi multinasional. |

---

## Keterbatasan Analisis

> [!WARNING]
> Interpretasi hasil harus mempertimbangkan batasan-batasan data berikut:

1. **Rentang Waktu Terbatas:** Data berakhir 27 Juli 2020; gelombang Delta, Omicron, dan dampak program vaksinasi global tidak tercakup dalam analisis ini.
2. **Under-reporting Sistematis:** Kapasitas *testing* berbeda antar negara secara signifikan. Negara dengan kapasitas testing rendah cenderung melaporkan kasus lebih sedikit, sehingga perbandingan antar negara bersifat indikatif, bukan absolut.
3. **Revisi Retrospektif:** Nilai harian negatif akibat revisi data retrospektif (seperti lonjakan 13 Februari 2020 di Hubei) di-*clip* ke 0 dalam preprocessing; hal ini mengubah distribusi harian secara lokal.
4. **Tanpa Normalisasi Per Kapita:** Perbandingan antar negara tidak mempertimbangkan ukuran populasi, sehingga negara berpenduduk besar cenderung dominan dalam ranking kasus absolut.
5. **Naive CFR:** Case Fatality Rate 3,97% dihitung sebagai Deaths/Confirmed × 100; nilai ini bukan IFR (*Infection Fatality Rate*) yang sebenarnya karena tidak memperhitungkan kasus tidak terdeteksi.

---

*Dibuat untuk keperluan akademik Mata Kuliah Visualisasi Data (2026).*

