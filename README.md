# Proyek Akhir Pembelajaran Mesin (INF62325) - Prediksi Harga Tiket Wisata

Program Studi S1 Informatika, FITK, UIN Siber Syekh Nurjati Cirebon
Kelas Informatika-5C | Kelompok 2 | Dosen pengampu: Ardi Susanto, S.Kom., M.Cs.

## Anggota

| Anggota | NIM | Tanggung jawab |
|---|---|---|
| Rizki Aidil Fazri | 2488010021 | Pertemuan 12 (penyiapan data dan Pipeline) |
| Ibrahim Hilal | 2488010030 | Pertemuan 13 (pemodelan dan evaluasi) |
| Luthfi Baihaqi | 2488010036 | Pertemuan 14-15 (aplikasi web) |

## Paket dan metode

- **Dataset:** Indonesia Tourism Destination (tim GetLoc, Bangkit Academy 2021)
- **Sumber:** https://www.kaggle.com/datasets/aprabowo/indonesia-tourism-destination
- **Berkas yang dipakai:** `tourism_with_id.csv` (437 baris, 13 kolom)
- **Metode:** Regresi Linear
- **Target:** `Price` (harga tiket masuk, rupiah)
- **Fitur:** `Category`, `City`, `Rating`, `Time_Minutes`

Pertanyaan analitik: *berapa perkiraan harga tiket masuk suatu destinasi wisata di Indonesia jika diketahui kategori, kota, rating, dan durasi kunjungannya?*

Dataset mentah tidak diunggah ke repositori ini. Notebook Pertemuan 12 mengunduhnya langsung dari Kaggle.

## Isi repositori

```
00_proposal.pdf          proposal proyek akhir
notebook/
  01_eksplorasi.ipynb    eksplorasi data awal
  P12_Penyiapan_Data.ipynb       penyiapan data dan Pipeline
  P13_Pemodelan_dan_Evaluasi.ipynb   pemodelan dan evaluasi
model/
  p13_model.joblib       model terlatih
  p13_metrik.json        metrik evaluasi
app.py                   aplikasi web (menyusul)
requirements.txt
README.md
```

## Hasil sementara (Pertemuan 13)

Model yang disimpan adalah Regresi Linear dengan target `log1p`. Seluruh angka di bawah dihitung pada data uji (88 baris).

| Ukuran | Nilai |
|---|---|
| MAE | sekitar Rp 15.830 |
| RMSE | sekitar Rp 35.193 |
| R2 | -0,1849 |
| R2 baseline (menebak rata-rata) | -0,0945 |
| R2 validasi silang 5 lipatan | 0,0525 (simpangan baku 0,0670) |

**Temuan utama:** model belum mengalahkan baseline pada R2 data uji, sehingga fitur yang tersedia belum cukup informatif untuk memprediksi harga tiket. Grafik residual menunjukkan asumsi hubungan linear, ragam tetap, dan kenormalan tidak terpenuhi. Dengan target log1p, MAE turun dari sekitar Rp 25.635 (model dasar) menjadi sekitar Rp 15.830. Koefisien menunjukkan asosiasi, bukan hubungan sebab-akibat.

**Catatan penting untuk aplikasi:** model menghasilkan logaritma harga, sehingga hasil prediksi harus diubah kembali dengan `np.expm1` sebelum ditampilkan sebagai rupiah.

## Cara menjalankan

1. Buka notebook di Google Colab dan jalankan sel pertama (`pip install scikit-learn==1.9.1`), lalu pilih **Runtime -> Restart session** dan jalankan ulang dari atas.
2. Jalankan `P12_Penyiapan_Data.ipynb` sampai selesai. Hasilnya tersimpan di folder Drive `ProyekML_RegresiLinear`.
3. Jalankan `P13_Pemodelan_dan_Evaluasi.ipynb`. Notebook ini membaca hasil Pertemuan 12 dan menyimpan model.
4. Aplikasi web (menyusul): `pip install -r requirements.txt`, lalu `streamlit run app.py`.

Tautan aplikasi: (menyusul)

## Keterbatasan

- Data hanya 437 baris, dengan kategori kecil (Tempat Ibadah 17 baris dan Pusat Perbelanjaan 15 baris).
- `Time_Minutes` kosong pada 53,09% baris dan tidak acak antar kategori. Nilainya diimputasi median di dalam Pipeline.
- Dataset dirancang untuk sistem rekomendasi, bukan untuk memprediksi harga. Tidak ada fitur seperti fasilitas, harga hari kerja dan akhir pekan, atau harga turis lokal dan asing.

## Kredit

Dataset: tim GetLoc (Bangkit Academy 2021), tersedia di Kaggle melalui tautan di atas.
