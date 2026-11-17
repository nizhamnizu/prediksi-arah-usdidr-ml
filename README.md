# 📈 Prediksi Arah Pergerakan Harian USD/IDR dengan Machine Learning

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=flat-square&logo=python&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-F7931E?style=flat-square&logo=scikit-learn&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-1572B6?style=flat-square)
![LightGBM](https://img.shields.io/badge/LightGBM-9cf?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-success?style=flat-square)

Proyek *Machine Learning* untuk klasifikasi biner pergerakan harian nilai tukar **USD/IDR**. Model dirancang untuk memprediksi apakah kurs penutupan USD/IDR besok akan **menguat/lebih tinggi (`1`)** dibandingkan hari ini, atau **melemah/tetap (`0`)** berdasarkan data historis pasar.

> ⚠️ **Disclaimer:** Proyek ini dibuat untuk tujuan pembelajaran dan eksplorasi data. Hasil prediksi tidak dirancang dan tidak boleh dijadikan dasar keputusan *trading* maupun investasi finansial.

---

## 📊 Ringkasan Hasil & Performa Model

Evaluasi dilakukan menggunakan skema *5-Fold Stratified Cross-Validation* pada data latih sebelum diuji satu kali pada data uji (*hold-out test set*):

| Metrik / Model | Nilai / Keterangan |
| :--- | :--- |
| 🏆 **Model Terpilih** | **Logistic Regression** (`C = 10`, dengan `StandardScaler`) |
| 🎯 **Accuracy (CV 5-Fold)** | `62.5%` (`0.625`)[cite: 1] |
| 🧪 **Accuracy (Data Uji)** | `62.1%` (`0.621`)[cite: 1] |
| ⚖️ **Precision / Recall (Data Uji)** | `61.0%` (`0.610`) / `53.1%` (`0.531`)[cite: 1] |
| 📉 **Baseline (Dummy Classifier)** | `53.2%` (`0.532` — menebak kelas mayoritas)[cite: 1] |
| 🤖 **Model Standar (KNN + Scaling)** | `58.7%` (`0.587` pada CV)[cite: 1] |

### 💡 Temuan Utama (*Key Insights*)
* **Pengaruh Indeks Dolar AS (DXY):** Sinyal prediktif terkuat berasal dari *return* harian DXY (`DXY_Return_1`) dengan korelasi Pearson sebesar **`0.26`** terhadap target[cite: 1, 4]. Hal ini sejalan dengan perbedaan zona waktu perdagangan, di mana penutupan pasar AS baru terserap penuh ke pasar Asia pada hari berikutnya.
* **Stabilitas Model Linear:** Meskipun diuji menggunakan model berbasis *tree/boosting* (Random Forest, XGBoost, LightGBM), **Logistic Regression** memberikan generalisasi terbaik tanpa *overfitting* berlebih pada data yang memiliki tingkat *noise* tinggi[cite: 1, 4].

---

## 🛠️ Alur Pengerjaan (*Workflow*)

1. **Eksplorasi & Pembersihan Data (EDA & Cleaning):** Konversi tipe data tanggal/numerik, penanganan *missing values* menggunakan *forward fill* pada hari libur bursa, pengecekan duplikat, dan analisis *outlier* pada *return* harian[cite: 4].
2. **Feature Engineering:** Pembuatan fitur berbasis informasi masa lalu hingga hari-$t$ (mencegah *data leakage*), meliputi *multi-period return* (1, 3, 5, 10 hari), rasio *Moving Average*, volatilitas *rolling*, bentuk *candlestick* harian, *return* eksternal (IHSG/JCI & DXY), serta *one-hot encoding* hari perdagangan[cite: 4].
3. **Pembagian Data:** Menggunakan `train_test_split` (`test_size=0.2`, `random_state=42`, `stratify=y`)[cite: 1, 4].
4. **Pemodelan & Tuning:** Perbandingan *Dummy Classifier*, *KNN (standar)*, *Logistic Regression*, *Random Forest*, *XGBoost*, dan *LightGBM* menggunakan `GridSearchCV` (5-fold CV) di dalam `Pipeline`[cite: 1, 4].
5. **Evaluasi Akhir & Interpretasi:** Pengujian satu kali pada *test set* menggunakan *Confusion Matrix*, *Accuracy*, *Precision*, dan *Recall*[cite: 1, 4].

---

## 📁 Dataset

Dataset terdiri dari 3 file historis harian (periode **Oktober 2015 – Oktober 2026**) yang diunduh dari **Investing.com**[cite: 1]. Sesuai konfigurasi `.gitignore`, file `.csv` tidak disertakan dalam repositori ini[cite: 1, 3].

Untuk menjalankan ulang proyek ini, unduh data historis tersebut dan letakkan di folder utama proyek dengan penamaan berikut[cite: 1]:
* `USD_IDR Historical Data (5).csv`[cite: 1]
* `Jakarta Stock Exchange Composite Index Historical Data (2).csv`[cite: 1]
* `US Dollar Index Historical Data (3).csv`[cite: 1]

---

## 🚀 Cara Menjalankan Proyek

1. **Clone repositori ini:**
   ```bash
   git clone [https://github.com/miyamgo/nama-repositori-kamu.git](https://github.com/miyamgo/nama-repositori-kamu.git)
   cd nama-repositori-kamu
