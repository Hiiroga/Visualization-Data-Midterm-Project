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
│   └── viz8_moving_average.png         # Visualisasi 5: Trend analysis 7MA vs 30MA
├── covid19_time_series_visualization.ipynb  # Jupyter Notebook UTAMA tugas UTS
├── requirements.txt                     # Daftar pustaka Python yang dibutuhkan
└── README.md                            # Dokumentasi lengkap proyek
```

---

## Panduan Setup Cepat & Menjalankan Proyek

Pastikan di komputer Anda telah terpasang **Python 3.9** atau versi lebih baru (diunduh dari [python.org](https://www.python.org/downloads/)).

### Metode 1: Menggunakan Visual Studio Code (Paling Direkomendasikan)

Metode ini adalah cara paling mudah dan interaktif:

1. **Buka Folder Proyek di VS Code:**
   - Jalankan VS Code.
   - Pilih menu **File > Open Folder...**, kemudian arahkan ke folder proyek ini (`c:\Data D\Semester 7\VISDAT\MIDTERM`).

2. **Buka Terminal di VS Code:**
   - Tekan shortcut `` Ctrl + ` `` (Backtick) atau pilih menu **Terminal > New Terminal**.

3. **Buat dan Aktifkan Virtual Environment:**
   *Pada Windows (PowerShell):*
   ```powershell
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```
   *(Jika muncul error Execution Policy pada PowerShell, jalankan `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass` lalu ulangi perintah aktivasi).*

   *Pada macOS/Linux:*
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

4. **Instal Pustaka yang Dibutuhkan:**
   ```bash
   pip install --upgrade pip
   pip install -r requirements.txt
   ```

5. **Jalankan Notebook:**
   - Di panel kiri VS Code, klik file **`covid19_time_series_visualization.ipynb`**.
   - Pada pojok kanan atas editor notebook, klik **Select Kernel** > pilih **Python Environments...** > pilih interpreter `.venv` yang baru dibuat.
   - Klik tombol **Run All** (atau jalankan sel satu per satu dari atas ke bawah).
   - Seluruh tabel output, statistik deskriptif, dan visualisasi akan langsung ter-render dengan sempurna.

---

### Metode 2: Menggunakan Jupyter Notebook Classic / JupyterLab

Jika Anda lebih terbiasa menggunakan antarmuka peramban (*web browser*):

1. **Buka Command Prompt (CMD) atau Terminal:**
   Masuk ke direktori proyek:
   ```bash
   cd "c:\Data D\Semester 7\VISDAT\MIDTERM"
   ```

2. **Buat Virtual Environment & Instal Pustaka:**
   ```bash
   python -m venv venv
   .\venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. **Jalankan Server Jupyter:**
   ```bash
   jupyter notebook
   ```
   Browser Anda akan terbuka secara otomatis menampilkan direktori proyek.

4. **Buka dan Eksekusi:**
   - Klik pada file `covid19_time_series_visualization.ipynb`.
   - Pilih menu **Cell > Run All** (atau **Kernel > Restart & Run All**).

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
- **Skema Warna:** Palet sekuensial `YlOrRd` (Kuning ke Merah Tua) yang memetakan intensitas penularan dari rendah ke sangat tinggi.
- **Analisis Pola:** Menguantifikasi fenomena *weekend reporting deficit* pada agregat data global.

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
| **Total Kasus Kumulatif Global** | **$16.480.485$ jiwa** | Akumulasi tercatat hingga 27 Juli 2020 di 187 negara. |
| **Total Kematian Global** | **$654.036$ jiwa** | Menghasilkan Case Fatality Rate (CFR) global sebesar **$3,97\%$**. |
| **Waktu Tempuh Milestone** | **0 ke 1M: 71 hari**<br>**1M ke 10M: 87 hari** | Karakteristik eksponensial terkonfirmasi dari percepatan waktu penggandaan. |
| **Puncak Kasus Baru Harian** | **$282.756$ kasus** | Terjadi pada **23 Juli 2020** (7-Day MA pada puncak: $242.581$ kasus). |
| **Top 3 Negara Teratas** | **US ($26,03\%$)**, Brazil, India | Ketiga negara mencakup **$49,83\%$** (hampir separuh) dari total beban kasus dunia. |
| **Wilayah WHO Terbesar** | **Americas ($53,59\%$)** | Menggantikan posisi Eropa sebagai episentrum global sejak Mei 2020. |
| **Efektivitas Mitigasi Jerman** | **Penurunan $90,73\%$** | 7-Day MA turun dari puncak $5.837$ (April) menjadi hanya $541$ kasus (Juli). |
| **Defisit Akhir Pekan Global** | **$2,07\%$** | Rata-rata akhir pekan ($86.467$) vs hari kerja ($88.296$). Teredam oleh agregasi multinasional. |

---

## Troubleshooting & FAQ

**Q1: Muncul error `FileNotFoundError: Terdapat berkas yang belum tersedia di folder data` saat menjalankan sel 4.**  
> **Solusi:** Pastikan folder `./data/` berada persis di samping file `.ipynb` dan memuat file `day_wise.csv` serta `full_grouped.csv`. Jika menjalankan via VS Code, pastikan *Current Working Directory* terminal atau *Workspace Folder* mengarah ke folder `MIDTERM`.

**Q2: Mengapa label di grafik multiple time series terpotong di tepi kanan?**  
> **Solusi:** Di dalam kode sel 7.3, batas sumbu X telah ditambahkan padding otomatis sebesar `+ pd.Timedelta(days=38)` dan parameter `clip_on=False`. Pastikan seluruh sel dieksekusi secara berurutan.

**Q3: Di mana gambar grafik disimpan?**  
> **Solusi:** Setiap kali sel visualisasi dijalankan, fungsi utilitas `save_fig()` akan secara otomatis mengekspor grafik ke folder `./figures/` dengan format PNG resolusi 150 DPI.

---

## Catatan Pengumpulan Tugas

Sebelum mengumpulkan file notebook (`.ipynb`):
1. **Personalisasi Identitas Mahasiswa:**
   - Buka bagian **Header Notebook** dan **Bagian 11 (AI Usage Declaration)**.
   - Ganti teks placeholder `[Nama Lengkap]` dan `[NIM]` dengan identitas resmi Anda.
2. **Restart & Run All:**
   - Jalankan perintah *Restart Kernel & Run All Cells* untuk memastikan seluruh sel kode dieksekusi secara bersih dan seluruh visualisasi ter-render dari awal hingga akhir tanpa eror.

---
*Dibuat untuk keperluan akademik Mata Kuliah Visualisasi Data (2024).*

