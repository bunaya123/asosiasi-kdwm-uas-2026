# 📚 Sistem Asosiasi KDWM - UAS 2026

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![Google Sites](https://img.shields.io/badge/Google-Sites-4285F4.svg)](https://sites.google.com/)
[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## 📖 Deskripsi

Repository ini merupakan project **Ujian Akhir Semester (UAS) Tahun 2026** untuk mata kuliah **Konsep Data Warehouse Dan Mining (KDWM)**.

Project ini berisi implementasi **Algoritma Apriori** untuk analisis asosiasi pada dataset `customer_shopping_data.csv`. Analisis dilakukan dari proses pengumpulan data, pengolahan data, hingga penyajian hasil dalam bentuk website portofolio.

---

## 🎯 Tujuan Project

1. Mengimplementasikan materi **Association Rule Mining** yang telah dipelajari
2. Menganalisis data customer shopping menggunakan algoritma **Apriori**
3. Menemukan **frequent itemset** dan **association rules** dari data transaksi
4. Menampilkan hasil analisis dalam bentuk **website portofolio** menggunakan Google Sites
5. Memenuhi tugas Ujian Akhir Semester (UAS)

---

## 👨‍🏫 Identitas Mata Kuliah

| Keterangan | Detail |
|------------|--------|
| **Mata Kuliah** | Konsep Data Warehouse dan Mining (KDWM) |
| **Dosen Pengampu** | Agus Rifaldi, S.Kom |
| **Semester** | Genap 2025/2026 |
| **Tahun Akademik** | 2025/2026 |

---

## 📊 Dataset

**Sumber:** `customer_shopping_data.csv`

| Statistik | Nilai |
|-----------|-------|
| Jumlah Transaksi | 9,899 |
| Jumlah Kolom | 9 |
| Missing Value | 0 |
| Duplikat | 0 |

Atribut yang digunakan dalam analisis:
- `Gender` → Jenis kelamin pelanggan
- `Age` → Usia (dikelompokkan menjadi 5 rentang)
- `Category` → Kategori barang yang dibeli
- `Price` → Harga (dikelompokkan menjadi 3 rentang)
- `Quantity` → Jumlah barang (dikelompokkan menjadi 3 kelompok)
- `Payment Method` → Metode pembayaran
- `Shopping Mall` → Mal tempat belanja

---

## 🔬 Metodologi

### 1. Pra-Pemrosesan Data

Atribut numerik dikelompokkan menjadi kategori:

```python
# Usia -> 5 kelompok
age_bins = [17, 25, 35, 45, 55, 70]
age_labels = ['18-25', '26-35', '36-45', '46-55', '56-69']
df['age_group'] = pd.cut(df['age'], bins=age_bins, labels=age_labels)

# Harga -> 3 kelompok (Murah, Sedang, Mahal)
df['price_range'] = pd.qcut(df['price'], q=3, labels=['Murah', 'Sedang', 'Mahal'])

# Quantity -> 3 kelompok
qty_bins = [0, 1, 3, 5]
qty_labels = ['Sedikit (1)', 'Sedang (2-3)', 'Banyak (4-5)']
df['quantity_group'] = pd.cut(df['quantity'], bins=qty_bins, labels=qty_labels)
```

### 2. Algoritma Apriori

Algoritma diimplementasikan dari nol dengan langkah:

| Level | Proses |
|-------|--------|
| **Level 1** | Hitung support semua item tunggal → ambil yang ≥ `min_support` |
| **Level k** | Bentuk kandidat itemset berukuran-k, hitung support → ambil yang ≥ `min_support` |
| **Pruning** | Kandidat dengan subset tidak-frequent langsung dibuang |
| **Stop** | Tidak ada lagi itemset baru yang lolos `min_support` |

### 3. Metrik Evaluasi

| Metrik | Rumus | Keterangan |
|--------|-------|------------|
| **Support** | `support(X) = count(X) / n_trans` | Seberapa sering itemset muncul |
| **Confidence** | `confidence(A→B) = support(A∪B) / support(A)` | Probabilitas B muncul jika A muncul |
| **Lift** | `lift(A→B) = confidence(A→B) / support(B)` | Kekuatan hubungan (lift > 1 = positif) |

---

## 📈 Hasil Analisis

### 🔥 Frequent Itemset (Support ≥ 5%)

| Itemset | Support | Jumlah Transaksi |
|---------|---------|------------------|
| Gender=Female | 59.8% | 5,920 |
| Bayar=Cash | 44.7% | 4,424 |
| Kategori=Clothing | 34.7% | 3,435 |
| Kategori=Clothing, Gender=Female | 21.9% | 2,164 |
| Kategori=Food & Beverage | 16.2% | 1,603 |
| Kategori=Cosmetics, Gender=Female | 9.0% | 887 |

### 🏆 Association Rules (Confidence ≥ 60%)

| Rule | Support | Confidence | Lift |
|------|---------|------------|------|
| Kategori=Technology → Harga=Mahal | 8.6% | 82.2% | 3.73 |
| Kategori=Clothing & Qty=Banyak → Harga=Mahal | 3.9% | 71.2% | 3.23 |
| Kategori=Shoes → Harga=Mahal | 8.2% | 68.7% | 3.11 |
| Kategori=Technology & Bayar=Cash → Harga=Mahal | 3.8% | 75.6% | 3.43 |
| Kategori=Books → Harga=Murah | 0.4% | 66.1% | 2.85 |

### 💡 Insight Tambahan

Setelah menghilangkan aturan yang melibatkan atribut **Harga**, aturan yang melibatkan **Gender, Usia, Metode Pembayaran, dan Mall** memiliki nilai **lift sangat mendekati 1** (≈1.0–1.02), yang mengindikasikan:

> **Tidak ada asosiasi kuat antara karakteristik demografis pelanggan dengan pola belanja mereka.**

---

## 📝 Kesimpulan

| No | Kesimpulan |
|----|------------|
| 1 | **Aturan dengan lift tertinggi** menghubungkan **Kategori** dengan **Harga** (misal: `Technology → Mahal` dengan lift 3.73), yang merupakan temuan logis karena harga memang terkait erat dengan kategori barang. |
| 2 | **Tidak ada asosiasi demografis** yang kuat. Setelah menghilangkan atribut Harga, aturan yang melibatkan Gender, Usia, dan Metode Pembayaran memiliki lift mendekati 1. |
| 3 | **Profil customer dominan:** Perempuan (59.8%), menggunakan Cash (44.7%), dan membeli pakaian (34.7%). |

### 💡 Saran Pengembangan

- Turunkan `MIN_SUPPORT` dan `MIN_CONFIDENCE` untuk menemukan aturan yang lebih spesifik
- Tambahkan atribut waktu (bulan/musim) untuk analisis musiman
- Integrasikan dengan data demografis lain untuk insight yang lebih dalam

---

## 🛠️ Tools & Software yang Digunakan

| Kategori | Tools/Software |
|----------|----------------|
| **Analisis Data** | Jupyter Notebook, Python (Pandas, NumPy, Matplotlib, Seaborn) |
| **Website Portofolio** | Google Sites, Browser |
| **Version Control** | GitHub, Git |
| **Editing Kode** | Notepad (untuk mengedit kode HTML yang disisipkan ke Google Sites) |
| **Rekaman Presentasi** | OBS Studio (untuk merekam presentasi yang akan diupload ke YouTube) |

---

## 💻 Hardware yang Digunakan

| Hardware | Spesifikasi |
|----------|-------------|
| **Laptop** | [Sesuaikan dengan spesifikasi laptop Anda] |
| **Keyboard** | Mechanical Keyboard |
| **Mouse** | USB Connection Mouse |

---

## 📂 Struktur Repository

```
asosiasi-kdwm-uas-2026/
│
├── asosiasi_customer_shopping.ipynb   # 📓 Notebook utama analisis
├── customer_shopping_data.csv         # 📊 Dataset
├── README.md                          # 📄 Dokumentasi
└── [folder lain sesuai kebutuhan]
```

---

## 🚀 Cara Menjalankan Project

### Prasyarat

Pastikan Python 3.8+ terinstal, lalu install dependencies:

```bash
pip install pandas numpy matplotlib seaborn
```

### Menjalankan Notebook

```bash
# Clone repository
git clone https://github.com/bunaya123/asosiasi-kdwm-uas-2026.git
cd asosiasi-kdwm-uas-2026

# Jalankan Jupyter Notebook
jupyter notebook asosiasi_customer_shopping.ipynb
```

### ⚙️ Parameter yang Dapat Diubah

```python
# Parameter utama Apriori
MIN_SUPPORT = 0.05        # Minimal support (5%)
MIN_CONFIDENCE = 0.6      # Minimal confidence (60%)

# Ubah nilai untuk eksplorasi lebih lanjut
```

---

## 🔗 Link Project

| Keterangan | Link |
|------------|------|
| 🎥 **Video Presentasi YouTube** | [Segera Hadir] |
| 🌐 **Google Sites** | [https://sites.google.com/view/data-science-portfolio/beranda?authuser=2](https://sites.google.com/view/data-science-portfolio/beranda?authuser=2) |
| 🐙 **GitHub Profile** | [https://github.com/bunaya123](https://github.com/bunaya123) |
| 📷 **Instagram** | [https://www.instagram.com/nayaaryd_/](https://www.instagram.com/nayaaryd_/) |

---

## 👨‍💻 Author

**Bunaya Ardik Saputra**

- Mahasiswa Manajemen Sistem Informasi
- GitHub: [https://github.com/bunaya123](https://github.com/bunaya123)
- Instagram: [@nayaaryd_](https://www.instagram.com/nayaaryd_/)

---

## 📄 License

Project ini dibuat untuk keperluan akademik (UAS 2026).

© 2026 Bunaya Ardik Saputra

---

**Dibuat dengan ❤️ menggunakan Python & Jupyter Notebook**
