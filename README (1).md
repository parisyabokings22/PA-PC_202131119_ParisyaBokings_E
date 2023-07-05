# Uas Pengolahan citra
PARISYA BOKINGS _ 202131119

TEORI
-
- Konversi Gambar ke Ruang Warna Keabuan (Grayscale) menggunakan fungsi `cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)`. Konversi ini dilakukan untuk mengurangi kompleksitas pemrosesan dan fokus pada informasi intensitas keabuan.
- Reduksi Noise menggunakan Gaussian Blur dengan Fungsi `cv2.GaussianBlur(image, (5, 5), 0)` untuk mereduksi noise pada gambar keabuan. Metode ini menerapkan filter Gaussian pada gambar untuk menghaluskan dan mengurangi noise sebelum deteksi tepi.
- Deteksi Tepi menggunakan Canny Edge Detection dengan Fungsi `cv2.Canny(blurred, 50, 150)` untuk mendeteksi tepi pada gambar yang telah direduksi noise. Metode Canny Edge Detection bekerja dengan menemukan perbedaan intensitas piksel yang signifikan dalam gambar.
- Threshold dan Deteksi Garis menggunakan Hough Transform dengan Fungsi `cv2.HoughLinesP(edges, 1, np.pi/180, threshold=100, minLineLength=100, maxLineGap=50)` untuk mendeteksi garis pada gambar dengan menggunakan Hough Transform. Parameter-parameter yang digunakan seperti threshold, minLineLength, dan maxLineGap dapat diatur untuk mengatur sensitivitas dan ketepatan deteksi.
- Menggambar Garis Deteksi pada Gambar Asli Setelah mendapatkan garis-garis deteksi, menggunakan fungsi `cv2.line(image, (x1, y1), (x2, y2), (0, 0, 255), 3)` untuk menggambar garis-garis tersebut pada gambar asli. Ini memungkinkan kita untuk melihat visualisasi dari marka jalan yang telah terdeteksi.

TAHAPAN
-
- Awal yang kita lakukan yaitu kita mengimpor library yang diperlukan, `cv2` (OpenCV) dan `numpy` (untuk operasi numerik).
- Setelah itu kita mendefinisikan fungsi `detect_lane_markings` yang akan digunakan untuk mendeteksi marka jalan pada gambar. Fungsi ini akan menerima input berupa gambar dan mengembalikan gambar dengan garis marka jalan yang terdeteksi.
- Di dalam fungsi `detect_lane_markings`, pertama-tama kita mengubah gambar ke ruang warna keabuan menggunakan `cv2.cvtColor` dengan parameter `cv2.COLOR_BGR2GRAY`.
- Selanjutnya, kita mengurangi noise pada gambar menggunakan metode Gaussian blur. Ini dilakukan dengan memanggil `cv2.GaussianBlur` dan memberikan gambar keabuan sebagai input.
- Setelah mengurangi noise, kita melakukan deteksi tepi menggunakan metode Canny edge detection dengan memanggil `cv2.Canny`. Parameter 50 dan 150 adalah threshold yang digunakan untuk menentukan kekuatan tepi.
- Setelah sudah mendapatkan tepi, kita menggunakan metode HoughLinesP untuk mendeteksi garis pada gambar, `cv2.HoughLinesP` akan mengembalikan array yang berisi koordinat titik awal dan akhir dari garis yang terdeteksi.
- lalu kita menggambar garis deteksi pada gambar asli menggunakan `cv2.line`. Setiap garis dalam array hasil deteksi akan digambar dengan warna merah (0, 0, 255) dan ketebalan garis sebesar 3 piksel.
- Yang terakhir yaitu fungsi `detect_lane_markings` akan mengembalikan gambar dengan garis marka jalan yang terdeteksi.
- Bagian utama kode yaitu kita membaca gambar dengan menggunakan `cv2.imread` dan menyimpannya dalam variabel `image`.
- Kemudian, kita memanggil fungsi `detect_lane_markings` dengan gambar sebagai argumen, dan hasilnya disimpan dalam variabel `result`.
- Selanjutnya kita menampilkan hasil deteksi marka jalan dengan menggunakan `cv2.imshow`, dan menunggu tombol keyboard ditekan dengan `cv2.waitKey`. Setelah tombol keyboard ditekan, kita menutup jendela tampilan menggunakan `cv2.destroyAllWindows`.

-JURNAL-
-
OpenCV

OpenCV (Open Source Computer Vision Library) adalah sebuah pustaka perangkat lunak yang ditujukan untuk pengolahan citra dinamis secara real-time, yang dibuat oleh Intel, dan sekarang didukung oleh Willow Garage dan Itseez. Program ini bebas dan berada dalam naungan sumber terbuka dari lisensi BSD. OpenCV yang digunakan di dalam sistem yang akan dibangun yaitu OpenCVSharp yaitu yang berisikan tentang method image processing, yang didalamnya mempunyai banyak method. Namun yang akan dipakai dan dipanggil dalam sistem antara lain: grayscale, threshold, filter smooth, operasi morphologi dan hough transform.

Preprocessing
Preprocessing diantaranya adalah konversi
Frame RGB ke grayscale dengan pemanggilan
fungsi melalui library opencvsharp adalah sebagai
berikut: Cv.CvtColor(src,gray,ColorConversion.BgrTo Gray);

Proses selanjutnya adalah cropping frame dengan pemanggilan fungsi melalui library opencvsharp sebagai berikut: src.SetROI(new CvRect(0, (src.Height/2)+100, src.Width-1, src.Height-1));

Proses selanjutnya adalah median filter dengan
pemanggilan fungsi melalui library opencvsharp
sebagai berikut: Cv.Smooth(gray, gray, SmoothType.Median, 7);

Proses selanjutnya adalah threshold dengan pemanggilan fungsi melalui library opencvsharp
sebagai berikut: gray.Threshold(gray, 85, 255, ThresholdType.Binary);

Hough Transform

Pada proses hough transform adalah proses pembentukan marka yang ditemukan pada citra biner yang memiliki struktur persamaan garis.
Dalam pemanggilan fungsi inisialisasi pembentukan garis pada library opencvsharp adalah sebagai berikut: 
line = gray.HoughLines2(memo, HoughLinesMethod.Probabilistic, 5,Math.PI / 180, 200, 75, 150);
Dalam fungsi (HoughLines2) merupakan
fungsi yang mengandung persamaan garis dengan rumus ρ = x cos θ + y sin θ. Dimana (HoughLines2) implementasinya akan memberikan output keluaran dengan deteksi garis berupa.
Probabilistik Berikut adalah proses inisialisasi titik dan batasan deteksi sudut, dan pembuatan koordinat titik marka: for (int i = 0; i < line.Total; i++); double angle = Math.Atan2(dy, dx) *
180 / Math.PI; Setelah proses inisialisasi titik probabilistik selanjutnya adalah penggambaran garis dengan menyambungkan titik-titik yang telah diketahui. 

Estimasi Posisi

Pada proses ini sistem akan mengestimasi posisi kendaraan dengan marka kiri dan samping perframe.
Bar estimasi posisi kendaraan dengan marka ini
melibatkan titik tengah kendaraan dan batas
kendaraan. Titik tengah kendaraan diinisalisasikan
sistem itu menggunakan kode program (src.Width
/ 2) dan menerapkan rumus persamaan 1 sampai 5
yang telah dijelaskan pada landasan teori.
Berikut adalah pemindai bar estimasi posisi
kendaraan jika mobil lebih dekat pada marka sebelah kanan, dilihat pada gambar 2.
Gambar 3 Bar Estimasi Posisi

ini gambaran sedikit dari beberapa jurna yang saya baca di internet.