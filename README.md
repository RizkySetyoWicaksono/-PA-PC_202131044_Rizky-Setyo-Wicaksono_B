
# Project B Pengelolahan Citra Buah Rizky Setyo Wicaksono / 202131044



## Teori yang mendukung mengenai projek terkait 

### Segmentasi Citra:

- Segmentasi citra adalah proses membagi citra menjadi beberapa bagian yang saling terpisah berdasarkan properti tertentu seperti warna, tekstur, atau intensitas.
- Tujuan segmentasi citra adalah untuk mengidentifikasi dan memisahkan objek atau area tertentu dari gambar.
- Metode segmentasi yang umum digunakan antara lain thresholding, pemrosesan berbasis wilayah (region-based), pemrosesan berbasis tepi (edge-based), dan pemrosesan berbasis kontur (contour-based).

### Pemilihan Warna:

- Pemilihan warna adalah proses memisahkan objek berdasarkan karakteristik warna mereka.
- Dalam projek deteksi buah, pemilihan warna digunakan untuk memisahkan buah dari latar belakang berdasarkan rentang warna tertentu yang dianggap mewakili buah tersebut.
- Biasanya, sistem warna seperti RGB (Red-Green-Blue) atau HSV (Hue-Saturation-Value) digunakan untuk mewakili dan memproses warna citra.

### Deteksi Buah:

- Deteksi buah adalah proses mengidentifikasi keberadaan dan lokasi buah dalam citra.
- Dalam projek ini, deteksi buah dilakukan dengan mengidentifikasi area yang memiliki warna yang sesuai dengan kriteria warna buah yang ingin dideteksi.
- Setelah deteksi dilakukan, latar belakang dapat dihapus dengan menggunakan operasi bitwise AND antara citra asli dan mask hasil deteksi untuk mendapatkan buah tanpa latar belakang.

### Pengolahan Morfologi:
- Operasi morfologi adalah kumpulan operasi matematika yang digunakan untuk memanipulasi struktur objek dalam citra berdasarkan bentuk, ukuran, atau posisi relatif mereka.
- Dalam projek ini, operasi morfologi digunakan untuk membersihkan mask deteksi buah agar lebih rapi dan mempertajam kontur objek buah yang terdeteksi.

### Contour Detection:

- Deteksi kontur adalah proses mengidentifikasi kontur atau tepi dari objek dalam citra.
- Dalam projek ini, deteksi kontur digunakan untuk menemukan kontur objek buah yang telah dihapus latar belakangnya.
- Setelah kontur ditemukan, buah dapat diidentifikasi dan dapat ditampilkan atau diproses lebih lanjut.
- Dengan menggabungkan teknik-teknik di atas, projek pengolahan citra deteksi buah dengan menghapus latar belakang dapat direalisasikan. Metode dan teknik yang tepat dapat bervariasi tergantung pada kondisi dan karakteristik gambar serta kebutuhan aplikasi yang spesifik.

## Source Code

```pyhton
pip install opencv-python

import cv2
import numpy as np
import matplotlib.pyplot as plt

img = cv2.imread('buah.jpg')

hsv_img = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)

lower_orange = np.array([0, 50, 50])
upper_orange = np.array([20, 255, 255])

lower_red = np.array([160, 50, 50])
upper_red = np.array([179, 255, 255])

lower_green = np.array([35, 50, 50])
upper_green = np.array([90, 255, 255])

mask_orange = cv2.inRange(hsv_img, lower_orange, upper_orange)
mask_red = cv2.inRange(hsv_img, lower_red, upper_red)
mask_green = cv2.inRange(hsv_img, lower_green, upper_green)

result_orange = cv2.bitwise_and(img, img, mask=mask_orange)
result_red = cv2.bitwise_and(img, img, mask=mask_red)
result_green = cv2.bitwise_and(img, img, mask=mask_green)

kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (5, 5))
cleaned_mask_orange = cv2.morphologyEx(mask_orange, cv2.MORPH_OPEN, kernel)
cleaned_mask_red = cv2.morphologyEx(mask_red, cv2.MORPH_OPEN, kernel)
cleaned_mask_green = cv2.morphologyEx(mask_green, cv2.MORPH_OPEN, kernel)

contours_orange, _ = cv2.findContours(cleaned_mask_orange, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
contours_red, _ = cv2.findContours(cleaned_mask_red, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
contours_green, _ = cv2.findContours(cleaned_mask_green, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

fig, axs = plt.subplots(2, 2, figsize=(10, 10))  # Mengatur ukuran gambar

axs[0, 0].imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
axs[0, 0].set_title('Gambar Asli')

axs[0, 1].imshow(cv2.cvtColor(result_orange, cv2.COLOR_BGR2RGB))
axs[0, 1].set_title('Jeruk')

axs[1, 0].imshow(cv2.cvtColor(result_red, cv2.COLOR_BGR2RGB))
axs[1, 0].set_title('Apel')

axs[1, 1].imshow(cv2.cvtColor(result_green, cv2.COLOR_BGR2RGB))
axs[1, 1].set_title('Mangga')

plt.tight_layout()
plt.savefig('hasil_gambar.png')  # Menyimpan hasil gambar
plt.show()

```


## Penjelasan Source Code 

Berikut saya akan menjelasakan program ini

```bash
  pip install opencv-python
```

Perintah dalam Python untuk menginstal library OpenCV. OpenCV (Open Source Computer Vision) adalah library populer yang menyediakan berbagai fungsi dan algoritma untuk pemrosesan citra dan penglihatan komputer. Dengan menjalankan perintah tersebut, library OpenCV akan diinstal di lingkungan Python, memungkinkan pengguna untuk menggunakan fungsi-fungsi OpenCV untuk melakukan tugas pengolahan citra dan deteksi objek.

```bash
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

- Import library yang diperlukan: cv2 (OpenCV), numpy, dan matplotlib.
- Membuat objek gambar menggunakan cv2.imread().
- Mengubah mode warna gambar menjadi HSV menggunakan cv2.cvtColor().
- Menentukan batas warna untuk masing-masing buah dalam format HSV.
- Membuat mask untuk setiap buah menggunakan cv2.inRange() dengan batas warna yang telah ditentukan.
- Menghapus latar belakang menggunakan bitwise AND dengan cv2.bitwise_and().
- Melakukan operasi morfologi pada mask menggunakan cv2.morphologyEx() untuk membersihkannya.
- Menemukan kontur pada mask yang telah dibersihkan menggunakan cv2.findContours().
- Menampilkan gambar-gambar hasil dengan matplotlib, termasuk gambar asli dan gambar hasil deteksi untuk setiap buah.


```bash
  img = cv2.imread('buah.jpg')
```

Kodingan di atas digunakan untuk memuat gambar dengan tiga buah dan latar belakang

```bash
  hsv_img = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
```

Berfungsi untuk konversi gambar ke mode warna HSV


```bash
lower_orange = np.array([0, 50, 50])
upper_orange = np.array([20, 255, 255])

lower_red = np.array([160, 50, 50])
upper_red = np.array([179, 255, 255])

lower_green = np.array([35, 50, 50])
upper_green = np.array([90, 255, 255])
```

Definisikan batas warna untuk setiap buah menggunakan range warna dalam mode HSV, saya mendefinisikan setiap warna yang akan dideteksi program


```bash
mask_orange = cv2.inRange(hsv_img, lower_orange, upper_orange)
mask_red = cv2.inRange(hsv_img, lower_red, upper_red)
mask_green = cv2.inRange(hsv_img, lower_green, upper_green)
```

Membuat mask untuk setiap buah berdasarkan batas warna

```bash
result_orange = cv2.bitwise_and(img, img, mask=mask_orange)
result_red = cv2.bitwise_and(img, img, mask=mask_red)
result_green = cv2.bitwise_and(img, img, mask=mask_green)
```

Menghapus latar belakang menggunakan bitwise AND untuk setiap buah

```bash
kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (5, 5))
cleaned_mask_orange = cv2.morphologyEx(mask_orange, cv2.MORPH_OPEN, kernel)
cleaned_mask_red = cv2.morphologyEx(mask_red, cv2.MORPH_OPEN, kernel)
cleaned_mask_green = cv2.morphologyEx(mask_green, cv2.MORPH_OPEN, kernel)
```


Kodingan di atas menggunakan operasi morfologi dengan kernel elips (5x5) untuk membersihkan mask atau memperbaiki hasil segmentasi dari masing-masing buah berdasarkan warna. Hasil operasi morfologi disimpan dalam variabel cleaned_mask_orange, cleaned_mask_red, dan cleaned_mask_green.

```bash
contours_orange, _ = cv2.findContours(cleaned_mask_orange, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
contours_red, _ = cv2.findContours(cleaned_mask_red, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
contours_green, _ = cv2.findContours(cleaned_mask_green, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
```


Kodingan tersebut menggunakan fungsi cv2.findContours untuk menemukan kontur pada setiap mask yang telah dibersihkan. Kontur eksternal dari objek dalam gambar akan diambil menggunakan metode approksimasi simpel. Hasil kontur akan disimpan dalam variabel contours_orange, contours_red, dan contours_green untuk masing-masing buah berdasarkan warnanya.

```bash
fig, axs = plt.subplots(2, 2, figsize=(10, 10)) 
```

Memisahkan dan menampilkan setiap buah yang telah dihapus latar belakangnya

```bash
axs[0, 0].imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
axs[0, 0].set_title('Gambar Asli')
```

Memberikan tampilan asli, menampilkan title, dan mematikan axis pada data.

```bash
axs[0, 0].imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
axs[0, 0].set_title('Gambar Asli')
```

Memberikan tampilan asli, menampilkan title, dan mematikan axis pada data.

```bash
axs[0, 1].imshow(cv2.cvtColor(result_orange, cv2.COLOR_BGR2RGB))
axs[0, 1].set_title('Jeruk')
```

Memberikan Buah dengan warna orange, menampilkan title, dan mematikan axis pada data.


```bash
axs[1, 0].imshow(cv2.cvtColor(result_red, cv2.COLOR_BGR2RGB))
axs[1, 0].set_title('Apel')
```

Memberikan Buah dengan warna Apel, menampilkan title, dan mematikan axis pada data.

```bash
axs[1, 1].imshow(cv2.cvtColor(result_green, cv2.COLOR_BGR2RGB))
axs[1, 1].set_title('Mangga')
```

Memberikan Buah dengan warna mangga, menampilkan title, dan mematikan axis pada data.

```bash
plt.tight_layout()
plt.savefig('hasil_gambar.png')  # Menyimpan hasil gambar
plt.show()
```

Memberikan Buah dengan warna mangga, menampilkan title, dan mematikan axis pada data.
