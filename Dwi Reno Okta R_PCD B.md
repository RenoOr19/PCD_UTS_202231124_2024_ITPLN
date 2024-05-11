
Pertama membuat library cv2 

"import cv2
import matplotlib.pyplot as plt
import numpy as np"

kemudian fungsi imread dari pustaka OpenCV untuk membaca gambar dengan nama file "Reno_124.jpg" dan menyimpannya dalam variabel img.

"img = cv2.imread("Reno_124.jpg")"

"alpha = 1.5
beta = 50
contrast_image = cv2.convertScaleAbs(image, alpha=alpha, beta=beta)"

program menggunakan fungsi cv2.convertScaleAbs() untuk menyesuaikan kontras dan kecerahan gambar. Ini adalah salah satu cara untuk mengubah kontras dan kecerahan gambar dalam OpenCV.

image: Ini adalah gambar asli yang telah dimuat sebelumnya.
alpha umtuk Parameter ini mengontrol kontras gambar. Nilai yang lebih besar dari 1.0 meningkatkan kontras, sedangkan nilai antara 0.0 hingga 1.0 mengurangi kontras. Dalam kasus ini, nilai alpha adalah 1.5, yang berarti kontras akan ditingkatkan sebesar 50%.
beta untuk Parameter ini mengontrol kecerahan gambar. Nilai yang lebih besar dari 0 meningkatkan kecerahan, sedangkan nilai negatif mengurangi kecerahan. Dalam kasus ini, nilai beta adalah 50, yang berarti kecerahan akan ditambahkan sebesar 50 unit.

"image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)"

mengkonversi bgr ke rgb

"R, G, B = cv2.split(image_rgb)"

Pada bagian ini, program menggunakan fungsi cv2.split() untuk memisahkan gambar RGB menjadi tiga saluran warna: merah (R), hijau (G), dan biru (B)

"images = [contrast_image, R, G, B]
titles = ['Citra Kontras', 'Citra Merah', 'Citra Hijau', 'Citra Biru']
for i in range(4):
    plt.subplot(2, 4, i+1), plt.imshow(images[i], 'gray')
    plt.title(titles[i])
    plt.xticks([]), plt.yticks([])"


images = [contrast_image, R, G, B]: Variabel images berisi daftar dari empat gambar yang akan ditampilkan. Ini termasuk gambar dengan kontras yang disesuaikan (contrast_image) serta tiga saluran warna terpisah dari gambar RGB, yaitu saluran merah (R), hijau (G), dan biru (B).

titles = ['Citra Kontras', 'Citra Merah', 'Citra Hijau', 'Citra Biru']: Variabel titles berisi daftar judul untuk setiap gambar yang akan ditampilkan. Ini akan digunakan untuk memberikan judul pada setiap subplot.

for i in range(4):: Ini adalah loop for yang akan berjalan empat kali, sesuai dengan panjang list images dan titles.

plt.subplot(2, 4, i+1), plt.imshow(images[i], 'gray'): Ini adalah perintah untuk membuat subplot dalam grid 2x4. Parameter pertama adalah jumlah baris dan kolom (2 baris, 4 kolom), dan parameter kedua adalah nomor subplot yang sedang ditampilkan (dimulai dari 1). Fungsi plt.imshow() digunakan untuk menampilkan gambar pada subplot yang sesuai. Mode warna 'gray' digunakan untuk memastikan bahwa gambar ditampilkan dalam skala abu-abu.

plt.title(titles[i]): Ini adalah perintah untuk memberikan judul pada subplot yang sedang ditampilkan. Judul diambil dari list titles sesuai dengan indeks i.

plt.xticks([]), plt.yticks([]): Ini adalah perintah untuk menghilangkan tanda sumbu x dan y pada gambar yang ditampilkan.

Seluruh loop ini berfungsi untuk membuat dan menampilkan empat subplot. Setiap subplot berisi salah satu gambar yang telah dimuat sebelumnya

color = ('b','g','r')
for i, col in enumerate(color):
    plt.subplot(2, 4, i+5)
    histr = cv2.calcHist([image],[i],None,[256],[0,256])
    plt.plot(histr,color = col)
    plt.xlim([0,256])
    plt.title('Histogram ' + col.upper())

plt.show()

color = ('b','g','r'): Ini adalah tupel yang berisi kode warna untuk setiap saluran warna: biru (b), hijau (g), dan merah (r).

for i, col in enumerate(color):: Ini adalah loop for yang akan berjalan tiga kali, sesuai dengan panjang tupel color. Variabel i akan berisi indeks iterasi, sedangkan col akan berisi kode warna yang sesuai dengan iterasi saat ini.

plt.subplot(2, 4, i+5): Ini adalah perintah untuk membuat subplot baru dalam grid 2x4. Karena subplot gambar sebelumnya sudah mengisi empat kolom pertama (indeks 1-4), subplot untuk histogram dimulai dari indeks ke-5.

histr = cv2.calcHist([image],[i],None,[256],[0,256]): Fungsi cv2.calcHist() digunakan untuk menghitung histogram untuk saluran warna yang ditentukan (dalam kasus ini, saluran yang sesuai dengan kode warna col). Ini akan menghasilkan histogram dari gambar untuk saluran warna tertentu.

plt.plot(histr,color = col): Ini adalah perintah untuk memplot histogram yang telah dihitung menggunakan matplotlib. Warna garis histogram sesuai dengan kode warna yang ditentukan oleh variabel col.

plt.xlim([0,256]): Ini menetapkan batas sumbu x pada plot histogram dari 0 hingga 256, karena nilai intensitas piksel berada dalam kisaran tersebut.

plt.title('Histogram ' + col.upper()): Ini memberikan judul pada plot histogram yang sedang ditampilkan. Judulnya terdiri dari kata "Histogram" diikuti dengan kode warna yang diubah menjadi huruf besar.

Setelah loop selesai dieksekusi, semua plot histogram akan ditampilkan menggunakan perintah plt.show().

thresh_images = []
titles = ['None', 'Blue', 'Red-Blue', 'Red-Green-Blue']

kemudian menyiapkan variabel thresh_images yang akan digunakan untuk menyimpan gambar hasil pengolahan dengan thresholding, serta variabel titles yang akan digunakan untuk menyimpan judul-judul yang akan diberikan pada setiap gambar hasil thresholding. Mari kita jelaskan keduanya:

thresh_images = []: Ini adalah sebuah list kosong yang akan digunakan untuk menyimpan gambar hasil pengolahan dengan thresholding. Nantinya, gambar-gambar ini akan ditambahkan ke dalam list ini setelah dilakukan proses thresholding.

titles = ['None', 'Blue', 'Red-Blue', 'Red-Green-Blue']: Ini adalah sebuah list yang berisi judul-judul yang akan diberikan pada setiap gambar hasil thresholding. Dalam kasus ini, terdapat empat judul, yaitu "None", "Blue", "Red-Blue", dan "Red-Green-Blue". Judul "None" mengindikasikan bahwa tidak ada thresholding yang dilakukan, sedangkan judul-judul lainnya menunjukkan jenis thresholding yang dilakukan atau kombinasi saluran warna yang digunakan untuk melakukan thresholding.

for i, channel in enumerate([img, b, r-b, r-g-b]):
    # menghitung histogram
    hist = cv2.calcHist([channel], [0], None, [256], [0,256])

    # Mencari nilai ambang batas
    thresholds = []
    for i in range(1, 255):
        if hist[i-1] < hist[i] and hist[i+1] < hist[i]:
            thresholds.append(i)

    # Mengurutkan nilai ambang batas
    thresholds.sort()

    # Melakukan thresholding dengan latar belakang hitam
    _, thresh_img = cv2.threshold(channel, thresholds[0], 255, cv2.THRESH_BINARY_INV)
    thresh_images.append(thresh_img)



 `for i, channel in enumerate([img, b, r-b, r-g-b]):`: Ini adalah loop `for` yang akan berjalan empat kali, sesuai dengan jumlah elemen dalam list yang disediakan. `enumerate()` digunakan untuk mengakses indeks dan elemen dari list secara bersamaan. Dalam setiap iterasi, variabel `channel` akan menyimpan satu gambar dari list yang diberikan.

 `hist = cv2.calcHist([channel], [0], None, [256], [0,256])`: Baris ini menghitung histogram dari saluran warna tertentu pada gambar yang disimpan dalam `channel`. Histogram ini kemudian akan digunakan untuk menentukan ambang batas untuk proses thresholding.

 `thresholds = []`: Variabel ini digunakan untuk menyimpan nilai ambang batas yang akan digunakan untuk thresholding.

 `for i in range(1, 255):`: Loop ini akan memeriksa setiap nilai intensitas piksel dalam histogram, kecuali nilai ekstrem (0 dan 255).

 `if hist[i-1] < hist[i] and hist[i+1] < hist[i]:`: Pernyataan ini memeriksa apakah nilai intensitas piksel saat ini (hist[i]) lebih besar dari tetangga sebelumnya dan sesudahnya. Jika iya, maka nilai intensitas piksel saat ini dianggap sebagai ambang batas. 

 `thresholds.append(i)`: Jika kondisi di atas terpenuhi, nilai intensitas piksel saat ini akan ditambahkan ke dalam list `thresholds`.

 `thresholds.sort()`: Setelah semua ambang batas ditentukan, list `thresholds` akan diurutkan dari nilai terkecil ke terbesar.

 `_, thresh_img = cv2.threshold(channel, thresholds[0], 255, cv2.THRESH_BINARY_INV)`: Baris ini melakukan proses thresholding pada `channel` menggunakan nilai ambang batas yang pertama dalam list `thresholds`. `cv2.threshold()` digunakan untuk melakukan thresholding. Parameter ketiga (255) menentukan nilai maksimum yang akan diberikan pada piksel yang melebihi ambang batas, dan `cv2.THRESH_BINARY_INV` menentukan bahwa gambar hasil thresholding akan memiliki latar belakang hitam (0) dan objek putih (255).

 'hresh_images.append(thresh_img)`: Gambar hasil thresholding (`thresh_img`) kemudian ditambahkan ke dalam list `thresh_images`.

kemudian menampilkan gambar

plt.figure(figsize=(10, 10))
for i in range(4):
    plt.subplot(1, 4, i+1), plt.imshow(thresh_images[i], 'gray')
    plt.title(titles[i])
    plt.xticks([]), plt.yticks([])
plt.show()