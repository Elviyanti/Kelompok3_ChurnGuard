# 🛡️ ChurnGuard

**ChurnGuard** adalah aplikasi web *retention intelligence* yang membantu tim Customer Success memprediksi pelanggan yang berisiko *churn* (berhenti berlangganan) dan memahami sentimen ulasan pelanggan secara otomatis. Dibangun dengan **Streamlit**, ChurnGuard menggabungkan model **machine learning (XGBoost)** untuk prediksi churn dan model **NLP** untuk analisis sentimen ulasan aplikasi BYU, lengkap dengan dashboard interaktif dan laporan PDF siap-pakai.

> Proyek ini dikembangkan oleh **Kelompok 3** sebagai bagian dari tugas/portofolio data science.

🔗 **Live Demo:** [churnguard-kelompok3.streamlit.app](https://churnguard-kelompok3.streamlit.app/)
🔑 Login: `admin` / `churnguard2024`

---

## ✨ Fitur Utama

ChurnGuard terdiri dari dua modul besar yang bisa diakses dari satu dashboard setelah login:

### 📉 Churn Analysis
- **Overview** — KPI ringkas (total customer, distribusi risiko High/Medium/Low), estimasi revenue yang terancam hilang, sinyal-sinyal risiko terdeteksi (inaktivitas, keterlambatan pembayaran, rendahnya adopsi fitur, NPS detractor, dll), churn rate per plan, dan tren risiko berdasarkan tenure.
- **Prediksi Customer** — form input untuk memprediksi probabilitas churn seorang customer secara *real-time* menggunakan model XGBoost yang sudah dilatih.
- **Semua Data** — tabel eksplorasi seluruh data customer beserta skor probabilitas churn-nya.
- **Detail Customer** — profil mendalam per customer (usage, engagement, tiket support, riwayat pembayaran, dll).
- **Model & Feature Importance** — metrik performa model (Accuracy, F1-Score, Precision, Recall) dan visualisasi fitur paling berpengaruh terhadap prediksi.
- **Laporan PDF** — generate laporan eksekutif (ringkasan, performa model, distribusi risiko per plan, top 20 customer paling berisiko, dan rekomendasi tindakan) yang bisa langsung diunduh.

### 💬 Sentiment Intelligence
- **Executive Dashboard** — KPI ulasan (total, positif, netral, negatif), tren sentimen bulanan, dan distribusi ulasan negatif per hari.
- **Deep Analysis** — analisis statistik (distribusi rating, hubungan panjang ulasan vs rating), word cloud per sentimen, dan kata kunci yang paling sering muncul.
- **Data Explorer** — eksplorasi data ulasan mentah dengan filter periode, sentimen, dan jumlah kata.
- **AI Predictor** — input teks ulasan bebas dan langsung dapatkan prediksi sentimen (positif/netral/negatif) beserta tingkat keyakinan model dan Penjelasan dengan XAI (SHAP).

Pipeline NLP mencakup pembersihan teks, normalisasi *slang* Bahasa Indonesia, koreksi pengulangan huruf, penghapusan stopword, dan penanganan negasi (mis. "tidak bagus" tidak disamakan dengan "bagus").

---

## 🧱 Tech Stack

| Kategori | Teknologi |
|---|---|
| Web App / Dashboard | [Streamlit](https://streamlit.io/), `streamlit-option-menu` |
| Machine Learning | XGBoost, Random Forest, scikit-learn |
| NLP | NLTK, custom slang & negation handler, Logistic Regression, TF-IDF |
| Visualisasi | Plotly, Matplotlib, WordCloud |
| Laporan | ReportLab (PDF generator) |
| Lainnya | Pandas, NumPy, Joblib, SHAP |

---

## 📁 Struktur Project

```
Kelompok3_ChurnGuard/
├── main.py                          # Entry point: halaman login + routing antar modul
├── ChurnGuard.py                    # Modul dashboard prediksi & analisis churn
├── Sentiment_Analysis.py            # Modul dashboard analisis sentimen ulasan
├── models/                          # Model & artefak ML yang sudah dilatih (.joblib/.pkl)
├── churnguard_cleaned.csv           # Dataset customer yang sudah dibersihkan
├── churnguard_predictions.csv       # Dataset customer + hasil prediksi churn
├── data_untuk_labeling_labeled.csv  # Data ulasan yang sudah diberi label sentimen
├── ulasan_aplikasi.csv              # Dataset ulasan aplikasi (mentah/utama)
├── ulasan_aplikasi_35.csv           # Subset/varian dataset ulasan aplikasi
├── requirements.txt                 # Daftar dependency Python
└── .streamlit/                      # Konfigurasi Streamlit
```

> **Catatan:** folder `models/` memuat artefak yang di-load oleh aplikasi, di antaranya `churnguard_model.joblib`, `forward_features.joblib`, `encoder_plan.joblib`, `encoder_nps.joblib`, `model_metrics.joblib` (untuk modul Churn Analysis), serta `sentiment_model.pkl`, `tfidf_vectorizer.pkl`, dan `preprocessor.pkl` (untuk modul Sentiment Intelligence). Pastikan file-file ini tersedia di lokasi yang sesuai sebelum menjalankan aplikasi.

---

## 🚀 Menjalankan Secara Lokal

### 1. Clone repository
```bash
git clone https://github.com/Elviyanti/Kelompok3_ChurnGuard.git
cd Kelompok3_ChurnGuard
```

### 2. Buat virtual environment (opsional tapi direkomendasikan)
```bash
python -m venv venv
source venv/bin/activate    # Windows: venv\Scripts\activate
```

### 3. Install dependency
```bash
pip install -r requirements.txt
```

### 4. Jalankan aplikasi
```bash
streamlit run main.py
```

Aplikasi akan terbuka di `http://localhost:8501`. Login menggunakan kredensial default berikut (dapat diubah di `main.py`):

| Username | Password |
|---|---|
| `admin` | `churnguard2024` |

---

## Demo Online
Tidak perlu instalasi — coba langsung di:
👉 **[https://churnguard-kelompok3.streamlit.app/](https://churnguard-kelompok3.streamlit.app/)**

```
Username : admin
Password : churnguard2024
```

---

## 🧠 Ringkasan Metodologi
- **Model Churn:** XGBoost dengan feature engineering (interaksi usage × tenure, rasio penurunan penggunaan, skor engagement & risk komposit, dll.) serta proses seleksi fitur (*forward selection*) untuk memilih fitur paling prediktif.
- **Model Sentimen:** Model klasifikasi teks berbasis TF-IDF dengan pipeline pra-pemrosesan khusus Bahasa Indonesia (cleaning, normalisasi slang, stopword removal, dan penanganan negasi).
- **Evaluasi:** metrik Accuracy, Precision, Recall, dan F1-Score ditampilkan langsung di dashboard (halaman *Model & Feature Importance*) beserta indikator *train-test gap* untuk memantau overfitting.

> Untuk detail metodologi, dataset, eksplorasi data, dan hasil evaluasi yang lebih lengkap, lihat **Technical Report** proyek ini.

---

## Tim — Kelompok 3
Dikembangkan sebagai proyek kelompok untuk studi kasus *customer churn* dan *sentiment analysis*.
- Elviyanti
- Rizky Amelia Putri
- Azkia Intan Sahila
- Rifa Mardhatillah
- Naila Syakirotul Rizkiyah

---
