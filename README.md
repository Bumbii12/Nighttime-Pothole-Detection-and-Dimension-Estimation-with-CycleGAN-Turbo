# Deteksi dan Estimasi Dimensi Pothole pada Skenario Malam Hari

Repositori ini adalah halaman ringkasan yang menghubungkan seluruh repositori skripsi **"Deteksi dan Estimasi Dimensi Pothole pada Skenario Malam Hari: Augmentasi YOLOv12 Menggunakan Data Sintetis CycleGAN-Turbo"** (Informatika, Universitas Bengkulu). Penjelasan lengkap, kode, dan cara menjalankan ada di README masing-masing repositori.

Penelitian ini memiliki dua tujuan. Tujuan pertama adalah menganalisis pengaruh penambahan data sintetis malam hari hasil CycleGAN-Turbo terhadap kinerja YOLOv12 dalam mendeteksi lubang jalan. Tujuan kedua adalah mengestimasi panjang dan lebar lubang dalam sentimeter menggunakan Inverse Perspective Mapping (IPM) pada dataset primer malam hari dari Jl. Bencoolen, Provinsi Bengkulu.


## Repositori

| No | Repositori | Isi |
|---|---|---|
| 1 | [Bengkulu-Nighttime-Pothole-Dataset](https://github.com/Bumbii12/Bengkulu-Nighttime-Pothole-Dataset) | Dataset primer (BNPD) yang dipakai sebagai data uji: 70 citra malam dari Jl. Bencoolen sepanjang 2 km dengan 100 anotasi dari 18 lubang. Dilengkapi ukuran acuan setiap lubang (`validation_gt.csv`) dan titik acuan IPM (`src_pts.csv`). |
| 2 | [Pembuatan-Ground-Truth-Estimasi-Dimensi-Lubang](https://github.com/Bumbii12/Pembuatan-Ground-Truth-Estimasi-Dimensi-Lubang) | Skrip untuk membuat ground truth dimensi lubang dari citra tampak atas dengan objek referensi 21,0 × 14,85 cm, serta untuk menentukan empat titik acuan IPM pada setiap citra. |
| 3 | [Pothole-Night-Synthetic-Augmentation](https://github.com/Bumbii12/Pothole-Night-Synthetic-Augmentation) | Notebook pelatihan dan evaluasi 15 konfigurasi (YOLOv12n/s/m, dengan YOLOv8s dan YOLO11s sebagai pembanding) pada tiga skenario data latih: baseline, sintetis 50%, dan sintetis 100%. |
| 4 | [BNPD-Pothole-Dimension-IPM](https://github.com/Bumbii12/BNPD-Pothole-Dimension-IPM) | Evaluasi deteksi model pada BNPD dan estimasi panjang serta lebar lubang dengan IPM, termasuk perbandingan hasil dari bounding box anotasi dan bounding box prediksi. |

Urutan baca yang disarankan: mulai dari repositori 3 untuk bagian deteksi, lalu repositori 1 dan 2 untuk data dan ground truth, dan terakhir repositori 4 untuk estimasi dimensi.

## Dataset latih di Kaggle

Dataset akhir ketiga skenario pelatihan dipublikasikan di Kaggle. Data validasi (200 citra) dan data uji (100 citra) sama pada ketiga skenario.

| Skenario | Citra latih akhir | Dataset |
|---|---|---|
| Baseline | 2.100 | [the-basic](https://www.kaggle.com/datasets/pororozagi/the-basic) |
| Sintetis 50% | 3.150 | [half-50](https://www.kaggle.com/datasets/pororozagi/half-50) |
| Sintetis 100% | 4.200 | [aug-cycle](https://www.kaggle.com/datasets/pororozagi/aug-cycle) |

Prapemrosesan dan augmentasi dijalankan melalui Roboflow (resize stretch 640 × 640 piksel, histogram equalization, dan pengali 3×). Dokumentasi Roboflow tidak menyebutkan pengaturan seed, sehingga citra augmentasi yang identik tidak dapat dibangkitkan ulang. Karena itu dataset akhir ini dirilis agar eksperimen dapat diulang dengan data yang sama persis.

## Hasil utama

**Deteksi.** Pada data uji sekunder malam hari, penambahan data sintetis 50% memberi hasil terbaik pada ketiga varian YOLOv12, dengan kenaikan mAP@0.5 sebesar 9,1% hingga 13,9% dibanding baseline. Penambahan 100% hanya menaikkan mAP@0.5 sebesar 1,7% hingga 7,7%. Model terpilih adalah YOLOv12s + sintetis 50% (mAP@0.5 0,6616; mAP@0.5:0.95 0,3317).

**Deteksi pada BNPD.** Tanpa ambang confidence tetap, model baseline memperoleh mAP@0.5 tertinggi (0,8280). Pada confidence 0,5, model sintetis 50% memberi Recall (0,700) dan F1 (0,745) tertinggi, serta mendeteksi 17 dari 18 lubang.

**Estimasi dimensi.** Dari 70 anotasi yang terdeteksi, IPM menghasilkan MAE 7,97 cm untuk lebar dan 18,57 cm untuk panjang, dengan kecenderungan melebihkan ukuran (bias +5,57 cm dan +11,97 cm). Kecenderungan ini lebih berkaitan dengan bounding box hasil deteksi daripada dengan transformasi IPM.

## Penulis

Ulfa Stevi Juliana (G1A022042), Program Studi Informatika, Universitas Bengkulu.

Pembimbing: Ir. Arie Vatresia, S.T., M.TI., Ph.D., IPP dan Prof. Ir. Lindung Zalbuin Mase, Ph.D, IPM., ASEAN. Eng

Penguji: Ir. Kurnia Anggriani, S.T., M.T., Ph.D. dan Ir. Tiara Eka Putri, S.T., M.Kom.
