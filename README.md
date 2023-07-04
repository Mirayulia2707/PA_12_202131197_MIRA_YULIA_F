
## Deteksi Marka Jalan

Deteksi marka jalan adalah salah satu aspek penting dalam sistem pengenalan dan pengawasan lalu lintas yang bertujuan untuk mengidentifikasi dan mengikuti marka jalan pada jalan raya. Teknologi deteksi marka jalan telah berkembang pesat dalam beberapa tahun terakhir dengan penggunaan sistem penglihatan komputer dan pengolahan citra.

Berikut adalah beberapa materi yang umumnya dibahas dalam konteks deteksi marka jalan:

- Pengolahan Citra: Pengolahan citra adalah tahap awal dalam deteksi marka jalan. Ini melibatkan penerapan filter dan teknik pra-pemrosesan lainnya untuk meningkatkan kualitas citra jalan, mengurangi noise, meningkatkan kontras, dan memperjelas marka jalan.

- Segmentasi Citra: Segmentasi citra adalah proses memisahkan marka jalan dari latar belakang dan objek lain dalam citra. Ini dapat dilakukan dengan menggunakan berbagai metode seperti thresholding, pemrosesan morfologi, atau pendekatan berbasis pemodelan seperti metode pengurangan latar belakang.

- Deteksi Fitur: Deteksi fitur digunakan untuk mengidentifikasi marka jalan dalam citra yang telah di-segmentasi. Ini melibatkan penerapan algoritma khusus untuk mendeteksi garis atau pola tertentu yang mewakili marka jalan. Contoh teknik yang digunakan dalam deteksi fitur termasuk Transformasi Hough, Transformasi Wavelet, atau metode berbasis pemodelan seperti filter Gabor.

- Klasifikasi: Setelah marka jalan terdeteksi, langkah selanjutnya adalah mengklasifikasikan jenis marka jalan yang ada. Ini melibatkan penerapan algoritma pembelajaran mesin atau pendekatan berbasis aturan untuk mengenali marka jalan yang berbeda seperti marka jalan putus-putus, marka jalan kontinu, marka jalan zebra cross, dan sebagainya.

- Tracking: Setelah marka jalan terdeteksi dan diklasifikasikan, langkah selanjutnya adalah melacaknya sepanjang waktu dalam urutan citra. Ini memungkinkan sistem untuk mengikuti perubahan posisi atau orientasi marka jalan seiring pergerakan kendaraan atau perubahan kondisi jalan.

- Validasi dan Evaluasi: Bagian penting dalam deteksi marka jalan adalah validasi dan evaluasi hasil deteksi. Ini melibatkan perbandingan hasil deteksi dengan ground truth (data referensi yang dianggap benar) untuk mengukur tingkat akurasi dan keandalan sistem.

Selain itu, penting juga untuk memahami persyaratan lingkungan dan teknis yang mempengaruhi deteksi marka jalan, seperti kondisi cahaya, cuaca, jenis jalan, kecepatan kendaraan, dan resolusi citra yang digunakan.

Harap dicatat bahwa teknik dan metode yang digunakan dalam deteksi marka jalan terus berkembang, dan ada banyak pendekatan yang berbeda yang dapat digunakan tergantung pada kasus penggunaan dan kebutuhan spesifik.







## Penjelasan Program
- In [1] Import Library:

mengimpor beberapa pustaka yang umum digunakan dalam pengolahan citra dan visualisasi data,Menggunakan:

import cv2
import matplotlib.pyplot as plt
import numpy as np

- In [2]Membaca Program:
img = cv2.imread('MarkaJalan.jpg') digunakan untuk membaca citra dengan nama file "MarkaJalan.jpg" menggunakan pustaka OpenCV (cv2).


-In [3] Menampilkan Citra Dalam Jendela:

cv2.imshow('Image', img)
cv2.waitKey()

cv2.waitKey(1)
cv2.destroyAllWindows()
cv2.waitKey(1)

Baris kode diatas  menggunakan beberapa fungsi dari pustaka OpenCV (cv2) untuk menampilkan citra dalam jendela, menunggu respons pengguna, dan kemudian menutup jendela setelah respons diterima.

-In [4] pemrosesan citra:
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

edges = cv2.Canny(img, 100,150)

gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY): Fungsi cv2.cvtColor()
fungsi ini digunakan untuk mengonversi citra yang awalnya dalam format BGR menjadi citra keabuan (grayscale).

edges = cv2.Canny(img, 100, 150): Fungsi cv2.Canny() digunakan untuk mendeteksi tepi dalam citra menggunakan algoritma Canny.

- In [5]membuat tampilan subplot :
Baris kode diatas  menggunakan pustaka matplotlib.pyplot untuk membuat tampilan subplot dengan dua gambar, yaitu gambar asli (grayscale) dan gambar tepi. 

- In [6] Implementasi Deteksi Marka Jalan:

def region_of_interest(img, vertices):
    mask = np.zeros_like(img)
    #channel_count = img.shape[2]
    match_mask_color = 255
    cv2.fillPoly(mask, vertices, match_mask_color)
    masked_image = cv2.bitwise_and(img, mask)
    return masked_image

def drow_the_lines(img, lines):
    img = np.copy(img)
    blank_image = np.zeros((img.shape[0], img.shape[1], 3), dtype=np.uint8)

    for line in lines:
        for x1, y1, x2, y2 in line:
            cv2.line(blank_image, (x1,y1), (x2,y2), (0, 255, 0), thickness=20)

    img = cv2.addWeighted(img, 0.8, blank_image, 1, 0.0)
    return img

image = cv2.imread('MarkaJalan.jpg')
image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
print(image.shape)
height = image.shape[0]
width = image.shape[1]
region_of_interest_vertices = [
    (0, height),
    (width/2, height/2),
    (width, height)
]
gray_image = cv2.cvtColor(image, cv2.COLOR_RGB2GRAY)
canny_image = cv2.Canny(gray_image, 200, 546)
cropped_image = region_of_interest(canny_image,
                np.array([region_of_interest_vertices], np.int32),)
lines = cv2.HoughLinesP(cropped_image,
                        rho=5,
                        theta=np.pi/180,
                        threshold=160,
                        lines=np.array([]),
                        minLineLength=40,
                        maxLineGap=50)
image_with_lines = drow_the_lines(image, lines)
plt.imshow(image_with_lines)
plt.show()

Baris-baris kode diatas adalah implementasi beberapa fungsi untuk mengolah citra jalan dengan deteksi marka jalan menggunakan transformasi Hough. 

>-region_of_interest(img, vertices):Fungsi ini menghasilkan citra dengan hanya mempertahankan bagian dari citra yang berada di dalam polygon yang ditentukan oleh vertices.

-mask = np.zeros_like(img) : Membuat citra mask yang berukuran sama dengan img dan berisi nilai nol (hitam)-match_mask_color = 255 : Menentukan nilai warna yang akan digunakan untuk mengisi mask (putih).

-cv2.fillPoly(mask, vertices, match_mask_color) : Mengisi polygon yang ditentukan oleh vertices dengan warna putih pada citra mask.

-masked_image = cv2.bitwise_and(img, mask) : Menggunakan operasi bitwise AND antara img dan mask untuk mendapatkan citra dengan ROI yang diinginkan.

-Mengembalikan citra hasil pemotongan ROI (masked_image).

- draw_the_lines(img, lines): Fungsi ini digunakan untuk menggambar garis pada citra berdasarkan array garis yang dihasilkan dari transformasi Hough. Fungsi ini mengembalikan citra dengan garis yang digambar.

-img = np.copy(img) : Membuat salinan dari citra input untuk menghindari modifikasi citra asli.

-blank_image = np.zeros((img.shape[0], img.shape[1], 3), dtype=np.uint8) : Membuat citra kosong dengan ukuran dan tipe data yang sama dengan img.

-for line in lines: : Melakukan iterasi pada setiap garis dalam array lines.

-cv2.line(blank_image, (x1, y1), (x2, y2), (0, 255, 0), thickness=20) : Menggambar garis pada blank_image berdasarkan koordinat titik awal (x1, y1) dan titik akhir (x2, y2), dengan warna (0, 255, 0) yang menandakan warna hijau dan ketebalan garis 20 piksel.

-img = cv2.addWeighted(img, 0.8, blank_image, 1, 0.0) : Menambahkan citra blank_image yang berisi garis ke citra img dengan bobot tertentu, menghasilkan citra dengan garis yang digambar.

-Mengembalikan citra dengan garis yang digambar (img).

- Baris-baris kode selanjutnya digunakan untuk memproses citra "MarkaJalan.jpg" dengan alur berikut:

-Membaca citra menggunakan cv2.imread().

-Mengubah format citra dari BGR ke RGB menggunakan cv2.cvtColor().

-Menampilkan dimensi citra menggunakan image.shape.

-Mendapatkan tinggi (height) dan lebar (width) citra.

-Mendefinisikan region_of_interest_vertices yang merupakan array koordinat yang mendefinisikan region of interest.

-Mengonversi citra keabuan menggunakan cv2.cvtColor() dengan COLOR_RGB2GRAY.

-Menerapkan deteksi tepi Canny pada citra keabuan menggunakan cv2.Canny().

-Memangkas citra menggunakan fungsi region_of_interest().

-Menerapkan transformasi Hough pada citra yang telah dipangkas menggunakan cv2.HoughLinesP().

-Menggambar garis pada citra asli menggunakan fungsi draw_the_lines().

-Menampilkan citra hasil deteksi marka jalan menggunakan plt.imshow() dan plt.show().





## Jurnal Terkait

http://digilib.unila.ac.id/56674/

http://lppm.primakara.ac.id/jurnal/index.php/smart-techno/article/view/73