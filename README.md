# Prediksi Arah Pergerakan Harian USD/IDR dengan Machine Learning

Mini project klasifikasi biner: apakah kurs USD/IDR besok **lebih tinggi** dari hari ini (1) atau turun/tetap (0).

## Ringkasan hasil
| Item | Nilai |
|---|---|
| Model terpilih | Logistic Regression (C = 10, dengan StandardScaler) |
| Accuracy CV 5 lipatan | 0,625 |
| Accuracy data uji (sekali pakai) | 0,621 |
| Precision / Recall data uji | 0,610 / 0,531 |
| Baseline (Dummy, kelas terbanyak) | 0,532 |
| Model standar (KNN + scaling), CV | 0,587 |

Sinyal terkuat berasal dari DXY (korelasi 0,26 dengan target). Hasil ini bukan dasar keputusan trading.

## Alur
EDA, data cleaning (disertai alasan), split (`test_size=0.2`, `random_state=42`, `stratify=y`), KNN sebagai model standar, model lain (Logistic Regression, Random Forest, XGBoost, LightGBM), cross-validation 5 lipatan dan tuning pada data latih, data uji dipakai satu kali, lalu interpretasi.

## Data
Tiga file CSV harian (Okt 2015 sampai Okt 2026): USD/IDR, JCI, dan US Dollar Index, berformat "Historical Data" dari Investing.com. File data tidak disertakan di repositori; unduh sendiri lalu letakkan di folder yang sama dengan notebook, dengan nama:
- `USD_IDR Historical Data (5).csv`
- `Jakarta Stock Exchange Composite Index Historical Data (2).csv`
- `US Dollar Index Historical Data (3).csv`

## Menjalankan
```bash
pip install -r requirements.txt
jupyter notebook EDA_Prediksi_Arah_USD_IDR_Sesuai_Ketentuan.ipynb
```

## Keterbatasan
Split acak pada data deret waktu dapat membuat skor uji lebih optimistis; data uji hanya sekitar 560 hari; berita dan kebijakan tidak ada di data.
