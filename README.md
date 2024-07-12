
## UAS_PRAKTIKUM_PCD
## SARIFA ARINI LATUCONSINA (202231041)
## TEORI MENGENAI DETEKSI DAUN
Deteksi daun dalam pengolahan citra digital melibatkan beberapa langkah utama yang melibatkan teknik-teknik pemrosesan gambar. Berikut adalah teori singkat mengenai deteksi daun:

### 1. **Akuisisi Gambar**
Langkah pertama adalah memperoleh gambar dari daun yang akan dideteksi, biasanya menggunakan kamera digital atau sensor gambar.

### 2. **Preprocessing (Pra-pemrosesan)**
Pra-pemrosesan gambar dilakukan untuk meningkatkan kualitas gambar dan mempersiapkannya untuk analisis lebih lanjut. Teknik yang umum digunakan meliputi:
- **Konversi Warna**: Mengubah gambar dari ruang warna RGB (atau BGR dalam OpenCV) ke grayscale untuk menyederhanakan pemrosesan.
- **Peningkatan Kontras**: Menggunakan metode seperti histogram equalization untuk meningkatkan kontras gambar.

### 3. **Segmentasi Gambar**
Segmentasi adalah proses memisahkan objek (daun) dari latar belakang. Beberapa metode yang digunakan adalah:
- **Thresholding**: Membagi gambar berdasarkan tingkat kecerahan piksel. Metode ini sederhana namun efektif untuk gambar dengan latar belakang yang kontras.
- **Deteksi Tepi**: Menggunakan operator seperti Sobel, Canny, atau Laplacian untuk mendeteksi tepi objek dalam gambar.

### 4. **Ekstraksi Fitur**
Setelah segmentasi, fitur-fitur penting dari daun diekstraksi. Fitur ini bisa berupa:
- **Kontur**: Menggunakan algoritma seperti `cv2.findContours` untuk menemukan kontur daun.
- **Ciri-ciri Bentuk**: Ekstraksi ciri bentuk seperti panjang, lebar, area, dan perimeter daun.

### 5. **Klasifikasi**
Jika tujuan deteksi daun adalah identifikasi jenis daun, maka langkah klasifikasi dilakukan. Metode yang umum digunakan meliputi:
- **Klasifikasi Berbasis Ciri**: Menggunakan ciri-ciri yang diekstraksi untuk mengklasifikasikan jenis daun menggunakan algoritma seperti K-Nearest Neighbors (KNN), Support Vector Machines (SVM), atau Decision Trees.
- **Jaringan Saraf Tiruan**: Menggunakan deep learning, khususnya Convolutional Neural Networks (CNN), untuk klasifikasi yang lebih akurat dengan data gambar besar.

### 6. **Post-processing (Pasca-pemrosesan)**
Langkah ini melibatkan penyempurnaan hasil segmentasi atau klasifikasi. Misalnya, menghaluskan tepi objek atau menggabungkan hasil segmentasi yang terfragmentasi.

### 7. **Visualisasi Hasil**
Hasil akhir dari deteksi dan segmentasi daun divisualisasikan untuk analisis lebih lanjut atau untuk ditampilkan kepada pengguna. Teknik visualisasi meliputi penandaan daun yang terdeteksi pada gambar asli atau menampilkan gambar biner hasil segmentasi.

### Aplikasi
- **Agrikultur**: Untuk memantau kesehatan tanaman, mendeteksi penyakit, dan mengoptimalkan perawatan tanaman.
- **Penelitian Botani**: Untuk klasifikasi dan identifikasi spesies tanaman.
- **Industri Pertanian**: Untuk otomatisasi proses panen dan pemeliharaan tanaman.

Secara keseluruhan, deteksi daun dalam pengolahan citra digital merupakan kombinasi dari berbagai teknik pemrosesan gambar dan pembelajaran mesin untuk mencapai hasil yang akurat dan efisien.

- ## Referensi Jurnal : http://jti.aisyahuniversity.ac.id/index.php/AJIEE

## Tahapan Cara Penyelesaian Project Deteksi Daun

Berikut adalah penjelasan dari kode yang diberikan untuk deteksi daun:

### Import Library
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
```
Kode ini mengimpor tiga library penting:
- `cv2` untuk pengolahan gambar.
- `numpy` (`np`) untuk operasi numerik.
- `matplotlib.pyplot` (`plt`) untuk visualisasi gambar.

### Konversi dari BGR ke RGB
```python
image_path = 'daunmangga.jpeg'
original_image = cv2.imread(image_path)
original_image = cv2.cvtColor(original_image, cv2.COLOR_BGR2RGB)
```
- `cv2.imread(image_path)` membaca gambar dari file.
- `cv2.cvtColor(original_image, cv2.COLOR_BGR2RGB)` mengonversi gambar dari format BGR (default OpenCV) ke format RGB.

### Konversi ke Grayscale
```python
gray_image = cv2.cvtColor(original_image, cv2.COLOR_RGB2GRAY)
```
- `cv2.cvtColor(original_image, cv2.COLOR_RGB2GRAY)` mengonversi gambar RGB ke grayscale untuk mempermudah proses pemrosesan gambar selanjutnya.

### Terapkan Thresholding untuk Membuat Gambar Biner
```python
_, binary_image = cv2.threshold(gray_image, 127, 255, cv2.THRESH_BINARY_INV)
```
- `cv2.threshold(gray_image, 127, 255, cv2.THRESH_BINARY_INV)` menerapkan thresholding untuk mengubah gambar grayscale menjadi gambar biner. Nilai piksel yang lebih terang dari 127 akan diubah menjadi 0 (hitam), dan yang lebih gelap menjadi 255 (putih) karena `cv2.THRESH_BINARY_INV`.

### Temukan Kontur
```python
contours, _ = cv2.findContours(binary_image, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
```
- `cv2.findContours(binary_image, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)` menemukan kontur dalam gambar biner. `cv2.RETR_EXTERNAL` hanya mengambil kontur luar, dan `cv2.CHAIN_APPROX_SIMPLE` menyederhanakan kontur.

### Buat Mask Kosong untuk Mangga dan Daun
```python
mask_mango = np.zeros_like(gray_image)
mask_leave = np.zeros_like(gray_image)
```
- `np.zeros_like(gray_image)` membuat mask kosong (semua nilai 0) dengan ukuran yang sama dengan gambar grayscale untuk mangga dan daun.

### Asumsikan Kontur Terbesar adalah Mangga dan Lainnya adalah Daun
```python
largest_contour = max(contours, key=cv2.contourArea)
cv2.drawContours(mask_mango, [largest_contour], -1, 255, thickness=cv2.FILLED)

for contour in contours:
    if contour is not largest_contour:
        cv2.drawContours(mask_leave, [contour], -1, 255, thickness=cv2.FILLED)
```
- `max(contours, key=cv2.contourArea)` menemukan kontur terbesar, yang diasumsikan sebagai mangga.
- `cv2.drawContours(mask_mango, [largest_contour], -1, 255, thickness=cv2.FILLED)` menggambar kontur terbesar pada mask mangga.
- Loop `for` menggambar kontur lainnya pada mask daun.

### Segmentasi Mangga dan Daun Menggunakan Mask
```python
segmented_mango = cv2.bitwise_and(original_image, original_image, mask=mask_mango)
segmented_leave = cv2.bitwise_and(original_image, original_image, mask=mask_leave)
```
- `cv2.bitwise_and(original_image, original_image, mask=mask_mango)` menerapkan mask untuk memisahkan mangga dari gambar asli.
- `cv2.bitwise_and(original_image, original_image, mask=mask_leave)` menerapkan mask untuk memisahkan daun dari gambar asli.

### Tampilkan Hasil
```python
fig, axes = plt.subplots(2, 2, figsize=(10, 10))

axes[0, 0].imshow(original_image)
axes[0, 0].set_title('Gambar Asli')

axes[0, 1].imshow(mask_mango, cmap='gray')
axes[0, 1].set_title('Mask Mangga')

axes[1, 0].imshow(segmented_mango)
axes[1, 0].set_title('Segmentasi Mangga')

axes[1, 1].imshow(segmented_leave)
axes[1, 1].set_title('Segmentasi Daun')

plt.tight_layout()
plt.show()
```
- `plt.subplots(2, 2, figsize=(10, 10))` membuat grid 2x2 untuk menampilkan gambar.
- `imshow` menampilkan gambar pada setiap subplot dengan judul masing-masing.
- `plt.tight_layout()` menyesuaikan tata letak subplot agar tidak saling tumpang tindih.
- `plt.show()` menampilkan semua gambar pada grid.

Kode ini pada intinya memisahkan mangga dan daun pada gambar menggunakan teknik pengolahan gambar dan menampilkan hasil segmentasi.


### Kesimpulan

Deteksi daun dalam pengolahan citra digital adalah proses yang melibatkan beberapa langkah utama untuk memisahkan dan mengidentifikasi daun dari gambar. Proses ini dimulai dengan akuisisi gambar dan diikuti oleh berbagai teknik pra-pemrosesan seperti konversi warna dan peningkatan kontras. Segmentasi gambar adalah langkah kunci yang memisahkan objek daun dari latar belakang menggunakan metode seperti thresholding dan deteksi tepi.

Setelah segmentasi, fitur-fitur penting dari daun diekstraksi untuk tujuan analisis lebih lanjut. Ekstraksi fitur ini mencakup identifikasi kontur dan pengukuran ciri bentuk daun. Jika diperlukan, klasifikasi dilakukan menggunakan algoritma pembelajaran mesin untuk mengidentifikasi jenis daun.

Teknik post-processing digunakan untuk menyempurnakan hasil segmentasi atau klasifikasi sebelum hasil akhirnya divisualisasikan. Aplikasi dari deteksi daun meliputi sektor agrikultur, penelitian botani, dan industri pertanian, yang semuanya diuntungkan dari kemampuan untuk memantau kesehatan tanaman, mendeteksi penyakit, dan mengoptimalkan perawatan tanaman.

Secara keseluruhan, deteksi daun menggunakan pengolahan citra digital menawarkan solusi efektif untuk berbagai aplikasi, memungkinkan analisis yang lebih akurat dan otomatisasi proses yang sebelumnya memerlukan intervensi manusia.

