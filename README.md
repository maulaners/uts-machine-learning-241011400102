# Tugas UTS: Machine Learning

Repositori ini berisi pengerjaan Ujian Tengah Semester (UTS) mata kuliah **Machine Learning**, mencakup implementasi algoritma **Linear Regression** dan **Logistic Regression** dari dasar (*from scratch*) menggunakan **NumPy**, visualisasi data dengan **Matplotlib**, manipulasi data dengan **Pandas**, serta analisis mendalam dan perbandingan dengan **scikit-learn**.

---

## 👤 Identitas Mahasiswa

- **Nama Mahasiswa:** Maulana Dwi Prasetya
- **NIM:** 241011400102
- **Kelas:** Reg B
- **Mata Kuliah:** Machine Learning
- **Random State:** 2 (diambil dari 2 digit terakhir NIM: `02` → `2`)

---

## 📌 Deskripsi Singkat Tugas

Tugas ini bertujuan untuk membangun pemahaman matematis, implementasi komputasi, dan analisis praktis pada dua algoritma fundamental machine learning:
1. **Bagian A – Linear Regression:** Memprediksi nilai penjualan bulanan toko online berdasarkan biaya iklan menggunakan dataset `Ads_Data.csv`.
2. **Bagian B – Logistic Regression:** Mengklasifikasikan email sebagai Spam atau Bukan Spam berdasarkan jumlah tautan (*JumlahLink*) dan jumlah kata promosi (*KataPromo*) menggunakan dataset `Email_Data.csv`.
3. **Bagian C – Pemahaman Konsep:** Menjawab pertanyaan teoretis konseptual mengenai cara kerja, fungsi aktivasi/loss, evaluasi, dan dinamika pengacakan data.
4. **Bonus – Perbandingan dengan scikit-learn:** Memvalidasi akurasi implementasi manual terhadap pustaka industri `scikit-learn` dan menganalisis pengaruh parameter regularisasi (`penalty`/`C`).

Seluruh model utama dan metrik evaluasi diimplementasikan secara manual menggunakan NumPy tanpa memanfaatkan model atau metrik dari scikit-learn.

---

## 🛠️ Algoritma yang Digunakan

1. **Linear Regression (Normal Equation & Gradient Descent):**
   - **Normal Equation:** Solusi analitis kuadrat terkecil $\beta = (X^T X)^{-1} X^T y$.
   - **Gradient Descent:** Optimasi iteratif pembaruan bobot $\beta \leftarrow \beta - \alpha \nabla L$ dengan eksplorasi learning rate $\alpha \in \{0.001, 0.02, 0.1\}$.
2. **Logistic Regression (Gradient Descent):**
   - Fungsi aktivasi Sigmoid: $\sigma(z) = \frac{1}{1 + e^{-z}}$.
   - Fungsi biaya Log Loss (Binary Cross-Entropy): $L = -\frac{1}{n} \sum [y \ln(p) + (1-y) \ln(1-p)]$.
   - Decision Boundary linear: $x_2 = -\frac{b + m_1 x_1}{m_2}$.

---

## 📊 Ringkasan Hasil

### 1. Linear Regression (Ads_Data.csv)
- **Ukuran Data:** 40 data (Split 80:20 → 32 data training, 8 data testing).
- **Persamaan Garis Regresi:** $\hat{y} = 29.8087 + 8.8424 \cdot X$
  *(Artinya: setiap kenaikan biaya iklan 1 juta Rp, penjualan diprediksi naik sebesar 8.84 juta Rp)*.
- **Hasil Evaluasi:**
  - **$R^2$ (Koefisien Determinasi):**
    - Training: **0.9342 (93.42%)**
    - Testing: **0.9601 (96.01%)**
  - **RMSE (Root Mean Squared Error):**
    - Training: **6.1551 juta Rp**
    - Testing: **5.6149 juta Rp**
  - **MAE (Mean Absolute Error):**
    - Training: **4.3984 juta Rp**
    - Testing: **5.3907 juta Rp**
- **Eksperimen Learning Rate pada Gradient Descent (5000 iterasi):**
  - $\alpha = 0.001$: Terlalu lambat (loss akhir: 59.7947, belum konvergen sempurna).
  - $\alpha = 0.02$: **Optimal / Pas** (loss akhir: 37.8849, hasil identik dengan Normal Equation).
  - $\alpha = 0.1$: **Divergen / Overflow** (melompat melewati minimum global dan menghasilkan nilai NaN).

---

### 2. Logistic Regression (Email_Data.csv)
- **Ukuran Data:** 60 data (35 Bukan Spam, 25 Spam; Split 75:25 → 45 training, 15 testing).
- **Persamaan Model:** $P(\text{Spam}) = \sigma(-9.9723 + 0.9029 \cdot \text{JumlahLink} + 0.7995 \cdot \text{KataPromo})$
- **Hasil Evaluasi Data Testing (Threshold 0.5):**
  - **Accuracy:** **0.9333 (93.33%)**
  - **Precision:** **1.0000 (100.00%)**
  - **Recall:** **0.8571 (85.71%)**
  - **F1-Score:** **0.9231 (92.31%)**
  - **Confusion Matrix:**
    $$\begin{bmatrix} TN & FP \\ FN & TP \end{bmatrix} = \begin{bmatrix} 8 & 0 \\ 1 & 6 \end{bmatrix}$$
- **Threshold yang Dipilih:** **0.5**
  - **Alasan Pemilihan:** Pada sistem filter email spam, kesalahan **False Positive (FP)** jauh lebih merugikan pengguna karena dapat membuang email penting (seperti surat tugas, verifikasi OTP, tagihan, atau pengumuman kerja). Threshold 0.5 menghasilkan **FP = 0 (Precision 100%)**, sekaligus mempertahankan **Recall tinggi (85.71%)**, serta mencapai **F1-Score (0.9231) dan Akurasi (93.33%) tertinggi**.

---

### 3. Bonus: Perbandingan dengan scikit-learn
- **Linear Regression:** Hasil implementasi manual Normal Equation dan `sklearn.linear_model.LinearRegression` adalah **100% identik** ($b = 29.8087, m = 8.8424, R^2 = 0.9601, \text{RMSE} = 5.6149$).
- **Logistic Regression:** Performa klasifikasi identik (Akurasi 93.33%, Precision 100%, Recall 85.71%). Perbedaan kecil pada nilai koefisien disebabkan oleh regularisasi L2 default ($C = 1.0$) pada scikit-learn yang melakukan penyusutan bobot (*shrinkage*). Saat regularisasi dinonaktifkan (`penalty=None`), koefisien scikit-learn mendekati hasil manual Gradient Descent.

---

## 🎯 Kesimpulan Singkat

1. **Linear Regression:** Model terbukti memiliki performa yang sangat tinggi ($R^2 = 96.01\%$ pada data uji) dengan error rata-rata yang relatif kecil. Hubungan antara biaya iklan dan penjualan bersifat linear positif yang kuat. Pemilihan learning rate pada Gradient Descent sangat krusial agar model tidak lambat konvergen atau divergen/overflow.
2. **Logistic Regression:** Model klasifikasi spam berbasis jumlah link dan kata promosi menghasilkan performa sangat baik (Akurasi 93.33%) dengan Precision sempurna (100%) pada threshold 0.5, sehingga aman digunakan tanpa risiko membuang email penting pengguna ke folder spam.

---

## 📂 Struktur Repositori

```
uts-machine-learning-241011400102/
├── README.md
├── tugas_UTS.ipynb
├── Ads_Data.csv
└── Email_Data.csv
```

---

## 🚀 Cara Menjalankan Notebook

Pastikan Anda telah menginstal pustaka yang diperlukan:
```bash
pip install numpy pandas matplotlib scikit-learn
```

Buka notebook di Jupyter Notebook, Jupyter Lab, atau Visual Studio Code:
```bash
jupyter notebook tugas_UTS.ipynb
```
Pilih menu **Kernel** → **Restart & Run All** untuk menjalankan seluruh sel dari awal hingga akhir. Seluruh sel dirancang menggunakan path relatif dan bebas dari error.
