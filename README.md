============================================================
       HASIL VERIFIKASI IJAZAH     
============================================================
Input        : 01_HighQuality_Enhanced.jpg
===    Output    ===
Nomor Ijazah : WM
N571012022000056 T
Tanda Tangan : PRESENT
Nilai CER    : 40.00%
============================================================

============================================================
       HASIL VERIFIKASI IJAZAH     
============================================================
Input        : 02_LowContrast.jpg
===    Output    ===
Nomor Ijazah : KV
N1571012022000056 T
Tanda Tangan : PRESENT
Nilai CER    : 46.67%
============================================================

============================================================
       HASIL VERIFIKASI IJAZAH     
============================================================
Input        : 03_Blurred.jpg
===    Output    ===
Nomor Ijazah : OMO
J
Tanda Tangan : PRESENT
Nilai CER    : 100.00%
============================================================

============================================================
       HASIL VERIFIKASI IJAZAH     
============================================================
Input        : 04_HighNoise.jpg
===    Output    ===
Nomor Ijazah : V
N571012022000056 T
Tanda Tangan : PRESENT
Nilai CER    : 33.33%
============================================================

============================================================
       HASIL VERIFIKASI IJAZAH     
============================================================
Input        : 05_LowResolution_Upsampled.jpg
===    Output    ===
Nomor Ijazah : N571012022000056 TA
Tanda Tangan : PRESENT
Nilai CER    : 26.67%
============================================================

============================================================
       HASIL VERIFIKASI IJAZAH     
============================================================
Input        : 06_Faded_Underexposed.jpg
===    Output    ===
Nomor Ijazah : V
N571012022000056 T
Tanda Tangan : PRESENT
Nilai CER    : 33.33%
============================================================

============================================================
       HASIL VERIFIKASI IJAZAH     
============================================================
Input        : 07_ColorShift_WarmTint.jpg
===    Output    ===
Nomor Ijazah : N571012022000056 T
Tanda Tangan : PRESENT
Nilai CER    : 20.00%
============================================================

============================================================
       HASIL VERIFIKASI IJAZAH     
============================================================
Input        : 08_JPEGCompression_Artifacts.jpg
===    Output    ===
Nomor Ijazah : VM
N571012022000056 T
Tanda Tangan : PRESENT
Nilai CER    : 40.00%
============================================================


============================================================
       HASIL VERIFIKASI IJAZAH     
============================================================
Input        : 09_CombinedDegradation.jpg
===    Output    ===
Nomor Ijazah : N571012022000056 F
Tanda Tangan : PRESENT
Nilai CER    : 20.00%
============================================================


METODE YANG DIGUNAKAN

1. Tahap Preprocessing & Enhancing
- Grayscale Conversion
Fungsi: Mengubah citra RGB (3 saluran warna: Red, Green, Blue) menjadi citra keabu-abuan (1 saluran warna).
Tujuan: Menghilangkan informasi warna yang tidak diperlukan dan mempercepat beban komputasi pemrosesan citra pada tahap berikutnya.
- CLAHE Enhancement 
Fungsi: Meratakan distribusi intensitas keabuan secara adaptif pada wilayah lokal (tile-by-tile).
Tujuan: Memperjelas teks atau goresan tinta yang samar akibat bayangan atau variasi pencahayaan saat dokumen dipindai/difoto, tanpa memperbesar bintik noise pada latar belakang.

2. Region of Interest (ROI) Extraction
- Spatial CroppingFungsi: Memotong wilayah spesifik citra berdasarkan koordinat relatif terhadap tinggi (h) dan lebar (w) citra.
Tujuan: Mengisolasi area Nomor Ijazah dan Tanda Tangan dari elemen ijazah lainnya (seperti stempel, teks judul, atau pasfoto). Hal ini mencegah interferensi teks/objek luar saat proses deteksi dan OCR berjalan.

3. Jalur 1: Ekstraksi Nomor Ijazah (OCR Pipeline)
Gaussian Smoothing / Blur (Enhancement Khusus OCR)
Fungsi: Mereduksi noise berfrekuensi tinggi (high-frequency noise) dan memperhalus tepi karakter.
Otsu's Binarization (THRESH_BINARY + THRESH_OTSU)
Fungsi: Mengubah citra grayscale menjadi citra biner (hitam-putih) secara otomatis.K
Optical Character Recognition (OCR) — Tesseract Engine
Fungsi: Mengonversi piksel teks pada gambar biner menjadi string karakter digital.
Konsep & Konfigurasi:
      - Engine Mode (--oem 3): Menggunakan neural network berbasis LSTM (Long Short-Term Memory) untuk pengenalan   pola sekuensial.
      - Page Segmentation Mode (--psm 6): Mengasumsikan area ROI adalah satu blok teks tunggal bersambung.
      - Character Whitelist: Membatasi kamus pembacaan hanya pada karakter alphanumeric (A-Z, a-z, 0-9) dan tanda titik/titik dua/spasi untuk menekan tingkat kesalahan pembacaan karakter (Character Error Rate).
Regex Pattern Matching
Fungsi: Memvalidasi dan mengekstraksi susunan karakter hasil OCR.

4. Jalur 2: Deteksi Tanda Tangan (Signature Detection Pipeline)
Inverted Otsu Thresholding (THRESH_BINARY_INV + THRESH_OTSU)
Fungsi: Membuat citra biner terbalik di mana goresan tinta tanda tangan bernilai 255 (putih/objek) dan latar belakang kertas bernilai 0 (hitam).
Tujuan: Kebanyakan algoritma morfologi dan analisis komponen terhubung di OpenCV menganggap piksel putih (255) sebagai objek foreground yang diproses.
Morphological Closing (MORPH_CLOSE)
Fungsi: Menghubungkan garis tanda tangan yang putus-putus dan menutup celah kecil di antara goresan pena.
Connected Component Analysis (CCA)
Fungsi: Memetakan dan mengelompokkan piksel-piksel putih yang saling bersentuhan (connected) menjadi satu kesatuan objek tunggal (blob).
Signature Decision Logic (Area Thresholding)
Fungsi: Menentukan apakah tanda tangan dianggap ada (PRESENT) atau tidak ada (NOT PRESENT).

Metode Enhancement mana yang paling efektif berdasarkan nilai CER

SANGAT EFEKTIF: 20.00%
EFEKTIF: 26.67%
CUKUP EFEKTIF: 33.33%
KURANG EFEKTIF: 40.00% - 46.67%
TIDAK EFEKTIF: 100.00%

Berdasarkan pengujian nilai Character Error Rate (CER), metode Contrast Limited Adaptive Histogram Equalization (CLAHE) terbukti paling efektif ketika diterapkan pada citra dengan kondisi perubahan warna (07_ColorShift_WarmTint) dan kombinasi degradasi (09_CombinedDegradation) dengan menghasilkan nilai CER terendah sebesar 20.00%. CLAHE bekerja secara optimal dengan membagi citra ke dalam ubin-ubin lokal kecil untuk meratakan distribusi kontras tanpa mengekspos derau latar belakang secara berlebihan. Mekanisme penyesuaian kontras lokal ini membuat 15 digit angka utama (571012022000056) tetap berhasil terekstraksi secara utuh meskipun dokumen mengalami gangguan pencahayaan maupun distorsi warna.

Penyebab nilai CER pada hasil terbaik tersebut belum mencapai 0.00% murni disebabkan oleh timbulnya kesalahan penyisipan (insertion error). Hasil pembacaan OCR masih menyedot karakter huruf tambahan di sekitar nomor ijazah (seperti huruf 'N', 'T', atau 'F'), yang secara matematis meningkatkan variabel penyisipan pada rumus kalkulasi CER sehingga mendongkrak persentase kesalahan meskipun seluruh deret angka utamanya sudah terdeteksi dengan benar.

Sebaliknya, metode peningkatan kualitas ini tidak efektif pada kondisi citra yang mengalami buram berat (03_Blurred) yang menghasilkan nilai CER sebesar 100.00%. Hal ini terjadi karena CLAHE hanya mereset distribusi histogram intensitas keabuan dan tidak memiliki kapabilitas untuk memulihkan batas garis tepi karakter yang hilang akibat kekaburan optik, sehingga algoritma binarisasi Otsu gagal memisahkan struktur teks dari latar belakang kertas ijazah.

