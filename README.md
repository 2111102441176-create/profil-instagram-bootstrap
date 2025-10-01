# profil-instagram-bootstrap

Proyek ini adalah sebuah clone antarmuka pengguna (UI) dari halaman profil Instagram yang dibuat secara statis menggunakan HTML dan Bootstrap 5. Halaman ini didesain agar responsif dan dapat menyesuaikan tampilannya dengan baik di berbagai ukuran layar, mulai dari perangkat mobile hingga desktop.

# Struktur File
Struktur file untuk proyek ini sangat sederhana. Semua aset gambar disimpan dalam satu direktori, dan file HTML utama berada di root.

.
├── asset/
│   └── img/
│       ├── 1.png
│       ├── 2.png
│       ├── ... (dan gambar lainnya)
│       ├── ikn.png
│       ├── SP.png
│       └── visit.png
└── index.html

# Cara Menjalankan (Build/Run)
Proyek ini tidak memerlukan proses build atau kompilasi. Karena ini adalah halaman HTML statis, Anda hanya perlu:
1. Pastikan Anda memiliki koneksi internet (untuk memuat file Bootstrap dan Font dari CDN).
2. Buka file index.html langsung di peramban web modern apa pun (seperti Google Chrome, Firefox, atau Safari).

# Dependensi
Proyek ini bergantung pada beberapa pustaka eksternal yang dimuat melalui CDN (Content Delivery Network), sehingga tidak perlu mengunduh file apa pun secara manual.
- Bootstrap 5.3.3 CSS: Digunakan sebagai kerangka kerja utama untuk layout, komponen, dan sistem grid responsif.
- Bootstrap 5.3.3 JS Bundle: Diperlukan untuk fungsionalitas interaktif komponen Bootstrap (meskipun tidak ada yang aktif digunakan di halaman ini).
- Bootstrap Icons 1.11.3: Untuk semua ikon yang digunakan di halaman (misalnya, ikon rumah, pencarian, dll.).
- Google Fonts ('Grand Hotel'): Digunakan khusus untuk meniru gaya tulisan logo "Instagram".

# Pertanyaan Teknis
Berikut adalah jawaban untuk beberapa pertanyaan mengenai pendekatan desain dan pengembangan yang digunakan dalam proyek ini.

1. Mengapa memilih konfigurasi kolom tertentu untuk tiap breakpoint?
Konfigurasi kolom dipilih berdasarkan pendekatan mobile-first untuk meniru perilaku layout Instagram asli.

Grid Postingan (col-4): Instagram menampilkan grid foto dalam 3 kolom yang konsisten di semua ukuran layar. Menggunakan kelas col-4 (12 / 4 = 3) tanpa breakpoint prefix (seperti sm, md, dll.) memastikan layout 3 kolom ini diterapkan secara universal, dari layar terkecil hingga terbesar. Ini adalah cara paling efisien untuk mencapai tampilan tersebut.

Header Profil (col-sm-4 dan col-sm-8):

Di Mobile (XS): Tanpa kelas col-sm-*, elemen foto profil dan info profil akan otomatis menumpuk secara vertikal (perilaku default div dalam row). Ini optimal untuk layar sempit agar konten tidak terkesan sesak.

Di Tablet/Desktop (sm ke atas): Mulai dari breakpoint sm (576px), layout berubah menjadi dua kolom (col-sm-4 untuk foto dan col-sm-8 untuk info). Ini memanfaatkan ruang horizontal yang lebih luas, menempatkan foto di samping informasi bio, statistik, dan tombol, persis seperti di situs web Instagram.

2. Bagaimana kamu memastikan tombol "Diikuti" / "Kirim pesan" tetap mudah dijangkau di mobile?
Dalam desain ini, pendekatan yang diambil adalah penempatan prioritas.

Meskipun tidak "menempel" (sticky) di bagian atas atau bawah layar, tombol-tombol aksi utama seperti "Diikuti" dan "Kirim pesan" ditempatkan di bagian atas area konten, tepat di bawah informasi visual utama (foto profil).

Pendekatannya: Pada layar mobile, pengguna pertama kali melihat foto profil, diikuti langsung oleh blok yang berisi nama pengguna dan tombol-tombol aksi ini. Blok ini muncul sebelum bio dan sorotan (highlights). Dengan menempatkannya di "paruh atas" halaman (above the fold), pengguna tidak perlu melakukan scroll jauh untuk menemukan dan berinteraksi dengan aksi paling umum, sehingga tetap mudah dijangkau.

3. Jika postingan bertambah jadi 50, apa potensi masalah dan bagaimana solusi grid mengatasinya?
Menambah postingan menjadi 50 akan menimbulkan dua masalah utama: kinerja pemuatan dan pengalaman pengguna (UX).

Potensi Masalah:
Waktu Muat Lambat: Memuat 50 gambar sekaligus akan secara drastis memperlambat waktu muat halaman, terutama pada koneksi internet yang lambat. Ini akan meningkatkan bounce rate.

Penggunaan Memori & CPU: Peramban harus mengunduh, mendekode, dan merender banyak gambar, yang memakan banyak sumber daya perangkat.

DOM yang Besar: Terlalu banyak elemen <img> di HTML akan membuat struktur DOM menjadi berat dan lambat diproses.

Solusi yang Didukung oleh Struktur Grid:
Struktur grid Bootstrap yang ada (<div class="row">...</div>) sangat fleksibel dan tidak akan "rusak" jika lebih banyak kolom ditambahkan. Ia akan secara otomatis membungkus item ke baris baru. Namun, untuk mengatasi masalah kinerja, solusi yang lebih canggih diperlukan:

Lazy Loading (Pemuatan Malas): Ini adalah solusi utama. Alih-alih memuat semua 50 gambar sekaligus, kita hanya memuat gambar yang terlihat di layar (viewport) atau yang akan segera terlihat. Gambar di luar layar baru akan dimuat saat pengguna menggulir ke bawah. Ini dapat diimplementasikan dengan mudah menggunakan atribut HTML loading="lazy" pada tag <img>.

<img src="path/to/image.png" alt="..." class="feed-img" loading="lazy">

Infinite Scroll (Gulir Tak Terbatas): Daripada menaruh 50 elemen <img> di HTML awal, kita hanya memuat 12 postingan pertama. Saat pengguna menggulir mendekati bagian bawah halaman, sebuah skrip JavaScript akan secara dinamis mengambil data (misalnya, 12 postingan berikutnya) dari server dan menambahkannya ke dalam grid. Ini menciptakan ilusi konten yang tak terbatas sambil menjaga halaman tetap ringan dan cepat.

Struktur grid yang ada sangat cocok untuk kedua solusi ini karena item baru dapat dengan mudah ditambahkan ke dalam div row tanpa merusak layout.
