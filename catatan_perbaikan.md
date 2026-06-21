# Catatan Perbaikan Web Portofolio - Izza Mafaza

Dokumen ini berisi temuan hasil analisis efisiensi, performa, serta rekomendasi langkah-langkah perbaikan untuk website portofolio pribadi Izza Mafaza.

---

## 1. Perbaikan Kritis JavaScript (Logika & Bug)

### A. Redundansi Event Listener pada Tab Detail Proyek
*   **Lokasi:** [main.js](file:///f:/Project/Web_Porto/izzamafaza.github.io/assets/js/main.js) (Baris 206–256)
*   **Masalah:** Terdapat dua event listener `DOMContentLoaded` terpisah yang mengikat handler klik ke elemen `.services-list a` yang sama. Satu untuk menampilkan `.service-content` dan yang lainnya untuk menampilkan `.service-content` sekaligus `.service-title`. Hal ini menyebabkan browser menjalankan pemrosesan DOM ganda secara berlebihan setiap kali tab diklik.
*   **Solusi:** Hapus blok event listener pertama (baris 206–228) dan cukup pertahankan blok kedua yang sudah mencakup update konten gambar sekaligus judul deskripsi secara dinamis.

### B. Bug Global State pada Indeks Carousel Gambar
*   **Lokasi:** [main.js](file:///f:/Project/Web_Porto/izzamafaza.github.io/assets/js/main.js) (Baris 258)
*   **Masalah:** Variabel `let currentSlideIndex = 0;` dideklarasikan secara global di tingkat file. Jika terdapat lebih dari satu carousel di halaman web, interaksi pada salah satu carousel akan memengaruhi indeks slide carousel lainnya, menyebabkan navigasi gambar menjadi tidak sinkron.
*   **Solusi:** Enkapsulasi variabel indeks slide ke dalam cakupan masing-masing kontainer carousel menggunakan penanganan berbasis *object-oriented* JavaScript atau ganti logika custom tersebut dengan memanfaatkan instance Swiper.js yang sudah terpasang.

### C. Pemanggilan Fungsi Carousel Tanpa Argumen di HTML
*   **Lokasi:** [project-no3.html](file:///f:/Project/Web_Porto/izzamafaza.github.io/project-no3.html) (Baris 131–136)
*   **Masalah:** Fungsi `prevSlide` dan `nextSlide` di JavaScript didefinisikan menerima parameter `container` (kontainer carousel). Namun, pemanggilan di elemen HTML ditulis tanpa argumen: `onclick="prevSlide()"`. Hal ini memicu error runtime JavaScript `Cannot read properties of undefined` saat tombol navigasi diklik.
*   **Solusi:** Ubah sistem pemicu klik. Gunakan event listener langsung melalui JavaScript di `main.js` daripada menggunakan atribut inline HTML `onclick`.

---

## 2. Optimalisasi Performa (Page Load Speed)

### A. Pemuatan Script Pihak Ketiga (Vendors) Secara Selektif
*   **Lokasi:** Seluruh halaman HTML (`index.html`, `about.html`, dll.)
*   **Masalah:** Semua pustaka vendor (seperti `typed.umd.js`, `glightbox.min.js`, `isotope.pkgd.min.js`, dan `validate.js`) dimuat di setiap halaman, terlepas dari apakah halaman tersebut benar-benar menggunakannya atau tidak.
*   **Solusi:** Lakukan pemilahan script. Misalnya, muat `typed.umd.js` hanya pada halaman yang menggunakan teks dinamis, dan hapus pustaka-pustaka tersebut dari halaman `index.html` jika hanya menampilkan elemen hero statis.

### B. Konversi dan Kompresi Gambar ke WebP
*   **Lokasi:** Folder `assets/img/` dan `assets/img/portfolio/`
*   **Masalah:** File gambar saat ini menggunakan format `.png` dan `.jpg` dengan resolusi tinggi, yang memperlambat waktu pemuatan awal (*First Contentful Paint*).
*   **Solusi:** Kompres dan konversi seluruh gambar aset proyek ke format **.webp**. WebP menawarkan kualitas visual yang serupa dengan ukuran file 30% hingga 70% lebih kecil dibanding PNG/JPG.

---

## 3. Struktur Kode & Pemeliharaan (Maintainability)

### A. Duplikasi Header, Navbar, dan Footer
*   **Lokasi:** Seluruh file HTML utama
*   **Masalah:** Elemen tata letak utama (Header, Navigasi, dan Footer) diduplikasi di setiap halaman statis. Jika terjadi perubahan struktur menu navigasi atau kontak footer, pengembang harus mengedit semua file HTML satu per satu.
*   **Solusi:** Untuk jangka panjang, pertimbangkan migrasi ke sistem static site generator (seperti Jekyll, Hugo) atau framework modern (seperti Vite + React/Next.js) agar elemen layout utama dapat dikelola sebagai komponen tunggal (Prinsip DRY - *Don't Repeat Yourself*).

### B. Penghapusan File Duplikat Mati
*   **Lokasi:** [project-no4.html](file:///f:/Project/Web_Porto/izzamafaza.github.io/project-no4.html)
*   **Masalah:** File ini merupakan duplikat persis dari `project-no1.html` dan menu penghubungnya telah dikomentari di `projects.html`.
*   **Solusi:** Hapus file [project-no4.html](file:///f:/Project/Web_Porto/izzamafaza.github.io/project-no4.html) agar repositori bersih dari file sampah (*dead code*).
