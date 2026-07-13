# 🤖 Model Prediksi Stok: `stock_prediction_model.pkl`

File `stock_prediction_model.pkl` adalah *otak* dari sistem prediksi stok di aplikasi ini. File ini merupakan model Machine Learning yang sudah dilatih dan siap digunakan untuk melakukan prediksi.

---

## 1. Deskripsi Model

- **Jenis Model**: Decision Tree Regressor
- **Metode**: CARD (Classification and Regression Decision Tree)
- **Tujuan**: Memprediksi **kebutuhan stok obat untuk satu bulan ke depan**.

Model ini tidak hanya memberikan angka, tetapi juga membantu dalam pengambilan keputusan strategis untuk manajemen inventori, seperti kapan harus memesan ulang dan berapa banyak jumlahnya.

---

## 2. Apa yang Diprediksi?

Output utama dari model ini adalah **`predicted_stock`**, yaitu **estimasi jumlah unit obat yang idealnya harus tersedia** untuk memenuhi permintaan selama **30 hari ke depan**.

Prediksi ini dihitung berdasarkan pola historis penjualan selama **3 bulan terakhir**.

---

## 3. Fitur yang Digunakan (Input Model)

Model ini menggunakan **8 fitur (input)** yang diekstrak dari database Laravel untuk membuat prediksi. Fitur-fitur ini harus konsisten dengan yang digunakan saat pelatihan.

| No | Nama Fitur            | Keterangan                                                    |
|----|-----------------------|---------------------------------------------------------------|
| 1  | `total_masuk`         | Total stok obat yang pernah masuk secara historis.            |
| 2  | `total_keluar`        | Total stok obat yang pernah terjual/keluar secara historis.   |
| 3  | `stok_saat_ini`       | Jumlah sisa stok yang tersedia di gudang saat ini.            |
| 4  | `frekuensi_transaksi` | Seberapa sering obat ini ditransaksikan dalam 3 bulan terakhir. |
| 5  | `avg_transaksi`       | Rata-rata jumlah unit obat yang terjual per transaksi.        |
| 6  | `demand_volatility`   | Seberapa fluktuatif (naik-turun) permintaan harian obat ini.  |
| 7  | `avg_daily_sales`     | Rata-rata penjualan harian berdasarkan 3 bulan terakhir.      |
| 8  | `safety_stock`        | Jumlah stok minimum yang harus selalu tersedia sebagai buffer.  |

**Penting**: `frekuensi_transaksi` juga digunakan untuk menentukan tingkat kepercayaan (`high`, `medium`, `low`). Semakin sering transaksi, semakin tinggi kepercayaannya.

---

## 4. Performa Model

Performa model dievaluasi menggunakan beberapa metrik standar untuk masalah regresi. Anda dapat melihat performa aktual di **Streamlit Dashboard**.

- **R² Score (Test Set)**: Biasanya berada di atas **0.80**, yang menandakan model mampu menjelaskan lebih dari 80% variasi data. Semakin mendekati 1.0, semakin baik.
- **MAE (Mean Absolute Error)**: Menunjukkan rata-rata kesalahan prediksi dalam satuan unit. Contoh: MAE sebesar `5.0` berarti prediksi model rata-rata meleset sekitar 5 unit (bisa lebih tinggi atau lebih rendah dari nilai aktual).

Secara umum, model ini dianggap **stabil dan tidak overfitting** jika selisih antara R² Score pada data *training* dan *test* sangat kecil (di bawah 0.05).

---

## 5. Cara Penggunaan

File `stock_prediction_model.pkl` ini tidak dijalankan secara langsung. File ini akan dimuat oleh **Flask API** (didefinisikan di `backend/api/app.py`).

Ketika Laravel mengirimkan permintaan prediksi ke API tersebut, API akan menggunakan model ini untuk menghitung hasilnya dan mengirimkannya kembali ke Laravel.

---
*Dokumen ini dibuat untuk memberikan pemahaman tentang aset utama dalam sistem prediksi.*