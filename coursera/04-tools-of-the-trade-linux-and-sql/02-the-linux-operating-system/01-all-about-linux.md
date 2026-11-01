# Resume Room: Introduction to Linux dalam Cybersecurity

**Tanggal penyelesaian:** 22 September 2026

---

## 1. Linux dalam Konteks Keamanan Siber

### 1.1 Kedudukan Linux di Bidang Security

Linux adalah sistem operasi yang paling banyak digunakan di dunia keamanan siber. Pemahaman terhadap Linux menjadi dasar penting bagi siapa pun yang bekerja sebagai security analyst, karena banyak tugas keamanan sehari-hari dijalankan di atas sistem ini.

### 1.2 Penggunaan Linux oleh Security Analyst

Ada tiga area utama penggunaan Linux dalam pekerjaan security analyst:

- **Log review** — memeriksa berbagai jenis log (misalnya error log) untuk mengidentifikasi apa yang sedang terjadi di dalam sistem saat menyelidiki suatu masalah.
- **Identity and access management** — memverifikasi akses dan otorisasi pengguna dalam sistem, karena pengelolaan akses adalah kunci menjaga keamanan sistem.
- **Distribusi khusus tugas** — digital forensic tools digunakan untuk menyelidiki suatu event alert, sementara distribusi untuk pen testing dipakai dalam offensive security guna mencari vulnerabilities di dalam sistem. Setiap distribusi diciptakan untuk memenuhi kebutuhan penggunanya masing-masing.

---

## 2. Sejarah dan Filosofi Linux

### 2.1 Asal Usul

Linux lahir dari gabungan dua proyek independen pada awal 1990-an:

- **Linus Torvalds** mengembangkan **Linux kernel** untuk meningkatkan sistem operasi UNIX yang sudah ada dan menjadikannya open source.
- **Richard Stallman** secara bersamaan mengerjakan **GNU**, sistem operasi berbasis UNIX lainnya, dengan tujuan yang sama: menciptakan software gratis dan terbuka bagi siapa saja. GNU saat itu belum memiliki kernel sendiri.

Penggabungan kernel Torvalds dengan sistem GNU milik Stallman menghasilkan apa yang sekarang dikenal sebagai Linux.

### 2.2 Sifat Open Source

Linux bersifat **open source**, artinya kode sumbernya dapat diakses, dibagikan, dan dimodifikasi secara bebas oleh siapa saja. Linux dan banyak program pendukungnya dilisensikan di bawah **GNU Public License**, yang mengizinkan penggunaan, distribusi, dan modifikasi software secara bebas.

Filosofi open source ini melahirkan komunitas developer besar yang berkolaborasi memajukan bidang komputasi. Hasilnya, saat ini terdapat lebih dari **600 distribusi (distro) Linux** yang berbeda-beda.

---

## 3. Arsitektur Linux

Sebuah request untuk menyelesaikan task mengalir melalui enam komponen arsitektur Linux secara berurutan: **user → applications → shell → Filesystem Hierarchy Standard (FHS) → kernel → hardware**.

### 3.1 User

**User** adalah orang yang berinteraksi dengan komputer, memulai dan mengelola tugas komputer. Linux adalah **multi-user system**, artinya banyak pengguna dapat menggunakan resource yang sama secara bersamaan.

### 3.2 Applications

**Application** adalah program yang menjalankan tugas tertentu (contoh: word processor, kalkulator, Nano sebagai text editor). Aplikasi bisa sudah pre-installed atau harus diinstal terlebih dahulu.

Di Linux, instalasi aplikasi umumnya dilakukan lewat **package manager**, yaitu tool yang membantu menginstal, mengelola, dan menghapus package atau aplikasi. **Package** sendiri adalah satuan software yang bisa digabungkan dengan package lain untuk membentuk sebuah aplikasi.

### 3.3 Shell

**Shell** adalah command-line interpreter — komponen yang memproses command dan menampilkan hasilnya. Semua input ke shell berbasis teks. Shell setara dengan konsep **CLI (Command Line Interface)**.

> Shell dapat dibayangkan sebagai penerjemah antara pengguna dan komputer — ia menerjemahkan perintah yang dimasukkan pengguna sehingga komputer bisa menjalankan tugas yang diinginkan.

### 3.4 Filesystem Hierarchy Standard (FHS)

**FHS** adalah komponen Linux OS yang mengorganisasi data dan menentukan lokasi penyimpanan data dalam sistem operasi.

**Directory** adalah file yang mengorganisasi tempat file-file lain disimpan (biasa disebut "folder"), dan bisa berisi file atau directory lain. FHS mendefinisikan bagaimana directory, isi directory, dan storage lainnya diorganisasi agar sistem tahu di mana mencari data tertentu.

> FHS dapat dibayangkan seperti sebuah filing cabinet atau lemari arsip — cara data disusun agar mudah ditemukan kembali saat diakses sistem.

### 3.5 Kernel

**Kernel** adalah komponen Linux OS yang mengelola **process** dan **memory**. Kernel berkomunikasi dengan applications untuk mengarahkan command, dan berkomunikasi dengan hardware untuk mengeksekusi command yang dikirim shell. Kernel bersifat unik pada Linux OS dan krusial dalam alokasi resource sistem.

Kernel menggunakan **drivers** untuk memungkinkan aplikasi menjalankan tugas, serta mengontrol seluruh fungsi utama hardware agar tugas dapat diselesaikan lebih efisien dan sistem bekerja lebih cepat.

### 3.6 Hardware

**Hardware** adalah komponen fisik komputer, berbeda dari software applications yang dapat diunduh ke sistem. Hardware terbagi menjadi dua kategori:

**Peripheral devices** — komponen hardware yang terpasang dan dikendalikan oleh sistem komputer, bukan komponen inti, dan bisa ditambah/dilepas bebas. Contoh: monitor, printer, keyboard, mouse.

**Internal hardware** — komponen yang dibutuhkan untuk menjalankan komputer, termasuk motherboard (papan sirkuit utama) dan seluruh komponen yang terpasang padanya:

- **CPU (Central Processing Unit)** — prosesor utama komputer, mengeksekusi instruksi dari program sehingga program dapat berjalan.
- **RAM (Random Access Memory)** — memori jangka pendek (short-term), tempat data disimpan sementara saat menjalankan tugas; data hilang setelah komputer dimatikan. CPU mengambil data dari RAM untuk menjalankan program.
- **Hard drive** — memori jangka panjang (long-term), tempat program dan file disimpan agar bisa diakses kembali meski komputer sudah dimatikan dan dinyalakan ulang. Satu komputer bisa memiliki lebih dari satu hard drive.

### 3.7 Alur Kerja Saat User Membuat File

Contoh penerapan arsitektur di atas ketika user membuat file baru:

1. **User** memutuskan untuk membuat file baru.
2. **Application** dibuka user sebagai jembatan komunikasi dengan shell.
3. **Shell** menerima command dari user yang menunjukkan keinginan membuat file baru.
4. **FHS** — shell membuat file baru tersebut di dalam struktur FHS.
5. **Kernel** menerima konten dan lokasi file baru yang diteruskan dari FHS.
6. **Hardware** — kernel memberi tahu hardware bagaimana caranya dan di mana file baru harus disimpan secara fisik.

---

## 4. Pentingnya Belajar Berkelanjutan di Cybersecurity

### 4.1 Sifat Dinamis Bidang Ini

Cybersecurity adalah bidang yang terus berubah karena perkembangan teknologi, bisnis, dan kehidupan digital yang sangat cepat. Layanan online dan teknologi di baliknya bisa berubah drastis hanya dalam kurun waktu 12 bulan.

### 4.2 Konteks Historis

Pada masa awal internet (pertengahan 1990-an), praktisi harus membangun sistem keamanan seperti web browser, web server, dan enkripsi dari nol — bahkan sebelum Google berdiri, karena belum ada standar atau tools siap pakai.

### 4.3 Pendekatan Belajar yang Tepat

Semua praktisi, termasuk profesional senior, memulai dari nol tanpa langsung menguasai semuanya (misalnya belum tahu Linux atau pemrograman). Pembelajaran bersifat bertahap (step-by-step), dan wajar bila harus terus mencari referensi karena tidak semua hal bisa diingat sekaligus.

> Kebanyakan orang cukup belajar sampai pada tingkat yang bernilai untuk tahap awal proses, kemudian mereka terus belajar sambil berjalan.

Rasa cemas saat menghadapi hal baru adalah wajar, dan rasa percaya diri tumbuh seiring bertambahnya pengalaman. Cara efektif untuk memulai: menulis atau membaca kode sederhana, lalu memodifikasinya sedikit demi sedikit untuk membangun fondasi pengetahuan secara bertahap.

---

## 5. Glossary

- **Linux** — sistem operasi open source berbasis UNIX, hasil gabungan kernel Torvalds dan sistem GNU milik Stallman.
- **Kernel** — komponen Linux OS yang mengelola process dan memory, serta menjadi penghubung antara software dan hardware.
- **Shell** — command-line interpreter, antarmuka utama pengguna untuk berkomunikasi dengan sistem melalui CLI.
- **Filesystem Hierarchy Standard (FHS)** — komponen yang mengorganisasi data dan menentukan struktur penyimpanan dalam Linux OS.
- **Directory** — file yang mengorganisasi tempat file/direktori lain disimpan; sering disebut "folder".
- **Distribusi (distro)** — varian dari Linux; terdapat lebih dari 600 distro yang dikembangkan komunitas.
- **Package manager** — tool untuk menginstal, mengelola, dan menghapus package atau aplikasi di Linux.
- **Package** — satuan software yang dapat digabungkan dengan package lain membentuk sebuah aplikasi.
- **GNU Public License** — lisensi yang mengatur penggunaan, distribusi, dan modifikasi bebas atas Linux dan program pendukungnya.
- **Multi-user system** — karakteristik Linux yang memungkinkan banyak pengguna memakai resource sistem secara bersamaan.
- **Peripheral devices** — hardware tambahan yang terpasang dan dikendalikan sistem, bukan komponen inti (contoh: monitor, printer, mouse, keyboard).
- **Internal hardware** — komponen inti yang dibutuhkan komputer untuk berjalan, termasuk motherboard, CPU, RAM, dan hard drive.
- **CPU (Central Processing Unit)** — prosesor utama yang mengeksekusi instruksi program.
- **RAM (Random Access Memory)** — memori jangka pendek yang menyimpan data sementara saat sistem beroperasi.
- **Hard drive** — memori jangka panjang tempat program dan file disimpan secara permanen.
- **Driver** — komponen yang digunakan kernel untuk memungkinkan aplikasi menjalankan tugas pada hardware.

---

## 6. Tools & Platform Rujukan

Tidak ada tools atau platform eksternal (seperti VirusTotal, Shodan, AbuseIPDB, dsb.) yang disebutkan secara eksplisit dalam room ini. Room ini berfokus pada konsep dasar Linux dan arsitekturnya, bukan pada penggunaan tools investigasi eksternal.

---

## 7. Catatan Ringkas untuk Ditulis Tangan

**Linux & Security**
- Linux — OS paling umum dipakai di security
- Kegunaan: review log, IAM (verifikasi akses), distro khusus (forensic, pen testing)

**Sejarah**
- Torvalds — bikin Linux kernel (dari UNIX, open source)
- Stallman — bikin GNU (OS berbasis UNIX, belum ada kernel)
- Kernel Torvalds + GNU Stallman = Linux
- Open source — source code bebas diakses/dibagikan/dimodif
- GNU Public License — lisensi bebas pakai/share/modif
- 600+ distro Linux karena kontribusi komunitas

**Arsitektur Linux (alur: user → app → shell → FHS → kernel → hardware)**
- User — orang yang mulai/kelola task; Linux = multi-user system
- Application — program tugas spesifik; diinstal via package manager
- Package manager — install/kelola/hapus package
- Package — satuan software pembentuk aplikasi
- Shell — command-line interpreter, "penerjemah" user–komputer, = CLI
- FHS — organisasi data, seperti "filing cabinet"
- Directory — file pengorganisir file lain ("folder")
- Kernel — kelola process & memory, komunikasi app–hardware, pakai driver
- Hardware — komponen fisik
  - Peripheral — attached/controlled, bukan inti (monitor, printer, keyboard, mouse)
  - Internal — wajib ada (motherboard, CPU, RAM, hard drive)
  - CPU — eksekusi instruksi program
  - RAM — memori jangka pendek, hilang saat mati
  - Hard drive — memori jangka panjang, permanen

**Alur bikin file**
- User putuskan bikin file → App dibuka → Shell terima command → FHS bikin file → Kernel terima konten+lokasi → Hardware simpan fisik

**Belajar berkelanjutan**
- Cybersecurity berubah cepat, teknologi bisa beda total dalam 12 bulan
- Era 90-an: bangun security dari nol (browser, server, enkripsi) sebelum Google ada
- Semua orang mulai dari nol, belajar bertahap, wajar cari referensi
- Cukup belajar cukup buat mulai, lalu belajar sambil jalan
