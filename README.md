# Outlier Detection in Ocean Wave Data using Modified Z-Score

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

> **Tugas Besar Penambangan Data (IF25-32025)** > Implementasi metode *Robust Statistics* untuk meningkatkan kualitas data gelombang laut (*Significant Wave Height*) dan validasi kinerja menggunakan Machine Learning.

---

## Tim Penyusun (Kelompok 1)

| No | Nama Anggota | NIM | Peran |
|:--:|:---|:--:|:---|
| 1 | **Martua Kevin A.M.H Lubis** | 122140119 |
| 2 | **Elkana Juanro Manullang** | 122140168 | 
| 3 | **Rachel Olivia Manullang** | 122140181 | 
| 4 | **Reyhan Fadel** | 122140... |

---

## Latar Belakang & Masalah

Data deret waktu (*time-series*) pada sensor kelautan seringkali mengandung *noise* atau anomali ekstrem yang disebabkan oleh kesalahan sensor atau kondisi cuaca ekstrem. Metode deteksi outlier konvensional (seperti Standard Z-Score) sering gagal karena menggunakan **Rata-rata (Mean)** yang sensitif terhadap outlier itu sendiri.

Proyek ini mengimplementasikan metode **Modified Z-Score** berdasarkan jurnal rujukan utama untuk mendeteksi anomali pada data Tinggi Gelombang (*Significant Wave Height* - `Hsig`).

**Jurnal Rujukan:**
> *Outlier Detection Performance of a Modified Z‑Score Method in Time‑Series RSS Observation...*

---

## Metodologi: Why Modified Z-Score?

Kami menggunakan pendekatan statistik yang *robust* (tahan banting) terhadap nilai ekstrem.

### Rumus Matematis
Sebuah titik data $x_i$ dianggap sebagai outlier jika nilai $M_i$ melebihi ambang batas (*threshold*) **3.5**.

$$M_i = \frac{0.6745(x_i - \tilde{x})}{\text{MAD}}$$

Dimana:
* $\tilde{x}$ : **Median** data (Nilai tengah).
* $\text{MAD}$ : **Median Absolute Deviation** (Median dari |$x_i - \tilde{x}$|).
* $0.6745$ : Konstanta normalisasi untuk konsistensi dengan standar deviasi.

### Mengapa Lebih Baik?
| Standard Z-Score (Konvensional) | Modified Z-Score (Usulan) |
| :--- | :--- |
| Menggunakan **Mean** & **Std Dev**. | Menggunakan **Median** & **MAD**. |
| Mean mudah "terseret" oleh nilai ekstrem. | Median stabil dan tidak bergeser oleh outlier. |
| Kurang efektif pada data distribusi miring (*skewed*). | Sangat efektif pada data *skewed* dan *heavy-tailed*. |

---

| Dataset | Karakteristik | Outlier Dibuang | Akurasi (Raw) | Akurasi (Modified Z-Score) | Peningkatan |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **Gelombang (1)** | Stabil / Homogen | 0 | 99.54% | 99.54% | **0%** (Data Bersih) |
| **Gelombang (2)** | Timpang / Skewed | ~3.700 | 93.10% | **98.50%** | **+5.4%** 🚀 |

### Critical Thinking Insight
* **Pada Dataset 1:** Tidak ditemukan outlier karena distribusi data sangat seragam. Nilai MAD cukup besar sehingga batas toleransi (Threshold) menjadi lebar.
* **Pada Dataset 2:** Ditemukan banyak outlier. Hal ini karena data didominasi nilai rendah (laut tenang), menyebabkan Median dan MAD sangat kecil. Akibatnya, algoritma menjadi sangat sensitif; lonjakan gelombang sedikit saja langsung dianggap anomali karena melewati batas toleransi yang sempit.

---

## Struktur Repository

```bash
├── data/
│   ├── Gelombang (1).xlsx    # Dataset Stabil
│   └── Gelombang (2).xlsx    # Dataset Skewed
├── images/
│   ├── time_series_result.png
│   └── accuracy_comparison.png
├── src/
│   └── main_analysis.ipynb   # Kode Utama (Jupyter Notebook)
└── README.md                 # Dokumentasi Proyek
