# Resume Materi: Operating Systems Introduction
**TryHackMe — Pre Security Path**
**Tanggal: 23 Oktober 2026**

---

## 1. Apa Itu Operating System

**Operating system (OS)** adalah software inti yang mengkoordinasikan semua yang terjadi di dalam komputer. OS duduk di antara user, aplikasi, dan hardware fisik, bertindak sebagai manajer tak terlihat yang menjaga seluruh mesin berjalan sebagai satu sistem terpadu.

Lapisan sistem dari atas ke bawah:

- User
- Applications
- Operating System
- Hardware

Tanpa OS, setiap aplikasi harus mengontrol langsung CPU, memori, file, dan perangkat secara sendiri-sendiri — ini akan menyebabkan konflik besar dan sistem tidak bisa berjalan stabil. OS hadir sebagai pengorganisir pusat yang menyelesaikan masalah itu.

---

## 2. System Privilege Layers

Di dalam komputer modern, tidak semua bagian sistem punya hak akses yang sama. Pemisahan ini disengaja untuk mencegah konflik dan masalah keamanan.

### 2.1 Kernel Space

**Kernel space** adalah area inti OS yang terkunci dan sangat istimewa. Di sinilah **kernel** berjalan — bagian OS yang secara langsung mengelola hardware dan sumber daya sistem.

- Memiliki akses penuh dan tak terbatas ke CPU, RAM, storage, dan semua komponen hardware
- Tidak bisa disentuh langsung oleh aplikasi biasa
- Satu-satunya layer yang boleh berkomunikasi langsung dengan hardware

### 2.2 User Space

**User space** adalah tempat semua aplikasi standar berjalan. Aplikasi di sini sengaja dibatasi dari akses hardware langsung demi keamanan dan stabilitas sistem.

- Kapanpun sebuah aplikasi butuh melakukan sesuatu ke hardware (buka file, putar suara, konek Wi-Fi), aplikasi itu harus mengirim **system call**
- Kernel yang menerima system call tersebut, mengevaluasinya, dan mengeksekusinya atas nama aplikasi
- Satu aplikasi yang crash di user space tidak bisa meruntuhkan seluruh sistem

### 2.3 System Call

**System call** adalah mekanisme resmi bagi aplikasi di user space untuk meminta kernel melakukan sesuatu yang membutuhkan akses hardware. Aplikasi tidak bisa langsung menyentuh hardware — semua harus lewat sistem call ini.

---

## 3. Lima Tugas Utama OS

Setiap OS bertanggung jawab atas tugas-tugas inti berikut yang memungkinkan komputer berjalan dengan aman, efisien, dan dapat diprediksi.

### 3.1 Process Management

OS membuat, menjadwalkan, memprioritaskan, dan menghentikan program yang berjalan. OS memutuskan berapa banyak waktu CPU yang diperoleh setiap proses, sehingga multitasking terasa mulus tanpa komputer freeze.

Contoh: membuka browser, pemutar musik, dan media sosial secara bersamaan.

### 3.2 Memory Management

OS mengalokasikan RAM ke setiap proses, melindungi memori satu aplikasi dari proses lain, dan mengambil kembali memori saat aplikasi ditutup.

Ketika RAM habis, OS menggunakan **virtual memory** — ruang sementara di storage yang dipakai sebagai perpanjangan RAM — agar sistem tetap stabil.

### 3.3 File System Management

OS mengorganisasi file ke dalam direktori dan menangani penamaan, path, permissions, serta **metadata** (nama, ukuran, tipe, timestamp).

Contoh: membuat folder baru, menyimpan foto, atau mengatur file menjadi read-only.

### 3.4 User Management

OS menangani banyak akun pengguna, autentikasi, dan permissions untuk menentukan siapa yang bisa mengakses apa.

Contoh: login dengan password memastikan file satu user tidak bisa dibuka oleh akun user lain.

### 3.5 Device Management

OS memuat **driver** dan menyediakan **hardware abstraction layer (HAL)** — lapisan antarmuka universal yang memungkinkan aplikasi cukup bilang "print ini" atau "putar suara ini" tanpa perlu tahu detail teknis hardware-nya.

Contoh: mencolokkan mouse atau printer baru dan langsung bisa digunakan.

---

## 4. Keamanan Bawaan OS

Setiap OS sudah menjadi fondasi keamanan bahkan sebelum antivirus atau firewall apapun dipasang. Empat aspek keamanan yang OS tangani secara default:

- **Authentication** — memverifikasi identitas pengguna melalui password login dan biometrik
- **Permissions** — mengontrol dengan tepat apa yang boleh dibaca, ditulis, atau dieksekusi oleh setiap user dan aplikasi
- **Isolation** — menjaga setiap proses dalam kotak perlindungannya sendiri melalui pemisahan kernel/user space
- **System Protection** — melindungi file sistem kritis dan pengaturan dari perubahan yang tidak sah

---

## 5. Cara Berinteraksi dengan OS

Interaksi dengan OS dapat dilakukan melalui dua antarmuka utama.

### 5.1 Graphical User Interface (GUI)

**GUI** menyediakan representasi visual dari sistem — ikon folder, jendela aplikasi, menu pengaturan. Ini adalah antarmuka yang paling umum digunakan sehari-hari.

- Tidak membutuhkan hafalan perintah
- Navigasi dilakukan melalui klik dan sentuhan
- Untuk membuka folder: beberapa klik navigasi

### 5.2 Command-line Interface (CLI)

**CLI** adalah antarmuka berbasis teks di mana pengguna mengetik perintah langsung untuk mengambil atau memanipulasi informasi.

- Memberikan jauh lebih banyak presisi, kontrol, dan kecepatan dibanding GUI
- Sangat efisien untuk tugas-tugas lanjutan dan administrasi sistem
- Membutuhkan keakraban dengan perintah dan sintaks yang digunakan
- Untuk membuka folder: satu perintah langsung menampilkan isinya

Perbandingan singkat:

| Aspek | GUI | CLI |
|---|---|---|
| Kemudahan | Tinggi, intuitif | Rendah, butuh latihan |
| Kecepatan (mahir) | Sedang | Tinggi |
| Presisi | Terbatas | Sangat tinggi |
| Cocok untuk | Pengguna umum | Admin, power user |

---

## 6. Tipe-Tipe Operating System

Tidak semua OS dirancang sama — perangkat dan lingkungan yang berbeda membutuhkan desain yang berbeda.

### 6.1 Lima Kategori Utama

| Tipe | Kasus Penggunaan | Karakteristik Utama |
|---|---|---|
| Desktop | PC harian, gaming, konten kreasi | GUI kaya, banyak app sekaligus, user-focused |
| Server | Web hosting, database, cloud, back-end | Headless (no GUI), uptime maksimal, multi-user |
| Mobile | Smartphone dan tablet | Touch UI, hemat daya, always connected, app sandboxing |
| Embedded | Peralatan, mobil, IoT, smart TV, router | Jejak sangat kecil, hardware terbatas |
| Virtual/Cloud | VM, container, cloud instance | Ringan, skalabel, deployment cepat |

### 6.2 OS di Dunia Nyata

**Desktop**

- **Windows** — paling luas dipakai di PC pribadi
  Versi: Windows 10 (end-of-life), Windows 11
- **macOS** — OS desktop Apple, dikenal karena GUI yang halus dan integrasi antar perangkat Apple
  Versi: Sonoma (14), Sequoia (15), Tahoe (26)
- **Linux** — bukan satu OS tunggal, melainkan keluarga OS open-source yang disebut distribusi
  Distribusi: Ubuntu, Debian, Fedora

**Server**

- **Windows** — digunakan di jaringan besar, data center, dan lingkungan korporat
  Versi: Server 2016, 2019, 2022, 2025
- **Linux** — mayoritas web server di dunia menggunakan Linux karena keandalan dan sifat open-source-nya
  Distribusi: Ubuntu Server, Debian, CentOS, Red Hat
- **Unix** — dipakai oleh perusahaan besar, sektor keuangan, telekomunikasi, dan pemerintahan
  Contoh: IBM AIX, Oracle Solaris

**Mobile**

- **Android** — OS mobile paling banyak digunakan, berjalan di ponsel, tablet, dan smart device
  Versi: Android 14–16, Manufacturer versions
- **iOS** — OS mobile Apple untuk iPhone, iPad, dan perangkat Apple lainnya
  Versi: iOS 17, 18, 26

**Embedded dan IoT**

- **Embedded Linux** — OS khusus yang tertanam ke dalam perangkat dengan fungsi tertentu
  Contoh: OpenWrt, Ubuntu Core, Yocto Project
- **Real-Time OS (RTOS)** — dirancang untuk aplikasi yang membutuhkan waktu respons terjamin, seperti sistem kontrol pesawat
  Contoh: FreeRTOS, VxWorks, QNX

**Virtual dan Cloud**

- **Cloud/VM** — menjalankan data center besar yang meng-host website, aplikasi, dan layanan streaming
  Contoh: Ubuntu LTS, Amazon Linux, Rocky Linux
- **Container-optimized** — alternatif lebih ringan dari VM, hanya mengemas aplikasi beserta dependensinya
  Contoh: Alpine Linux, Bottlerocket AWS, Flatcar Linux

### 6.3 Mengapa Begitu Banyak OS?

Karena setiap lingkungan punya kebutuhan berbeda:

- Laptop butuh antarmuka ramah pengguna dan dukungan multitasking
- Server butuh stabilitas, keamanan, dan kemampuan berjalan non-stop tanpa gangguan
- Perangkat mobile butuh efisiensi daya dan integrasi hardware yang ketat
- Sistem embedded butuh OS yang sangat ringan untuk fungsi spesifik

Tidak ada satu OS yang sempurna untuk semua situasi — dari sinilah ekosistem OS yang beragam berkembang.

---

## 7. Temuan Praktis dari Lab

Dalam latihan praktis room ini, sistem yang diinvestigasi adalah mesin Ubuntu yang diterima dari seorang teman. Informasi yang ditemukan dari System Monitor:

- OS: Ubuntu MATE 24.04.1 LTS (Noble Numbat) 64-bit
- Versi MATE: 1.26.2
- Kernel: Linux 6.8.0-1016-aws x86_64
- Processor: AMD EPYC 7571 × 2
- Memory: 1.9 GiB
- Available disk space: 9.1 GB
- File system type (/dev/root): ext4

Di dalam direktori /home terdapat tiga user: alex, guest, ubuntu.
Di dalam folder Documents milik user alex, terdapat file note.txt yang berisi flag: THM{new_pc_for_free!}

---

## 8. Glossary — Istilah Penting

**Operating System (OS)** — software inti yang mengelola hardware, aplikasi, dan semua sumber daya sistem.

**Kernel** — bagian inti OS yang berjalan di kernel space dan langsung mengelola hardware serta sumber daya sistem.

**Kernel space** — area OS yang sangat istimewa dengan akses penuh ke hardware; hanya kernel yang beroperasi di sini.

**User space** — area tempat aplikasi biasa berjalan dengan akses hardware yang dibatasi demi keamanan.

**System call** — permintaan resmi dari aplikasi di user space kepada kernel untuk melakukan sesuatu yang membutuhkan akses hardware.

**Virtual memory** — ruang di storage yang dipakai sebagai perpanjangan RAM ketika RAM fisik habis, agar sistem tetap stabil.

**Driver** — software yang menghubungkan OS dengan perangkat keras tertentu agar bisa dikenali dan dioperasikan.

**Hardware Abstraction Layer (HAL)** — lapisan antarmuka universal yang disediakan OS sehingga aplikasi tidak perlu tahu detail teknis hardware yang digunakan.

**GUI (Graphical User Interface)** — antarmuka visual OS berupa ikon, jendela, dan menu yang dioperasikan dengan klik atau sentuhan.

**CLI (Command-line Interface)** — antarmuka berbasis teks di mana pengguna mengetik perintah untuk mengontrol sistem.

**Distribution (distro)** — varian atau turunan dari OS Linux yang dikemas dengan komponen dan konfigurasi tertentu, contohnya Ubuntu, Debian, Fedora.

**Headless** — kondisi server OS yang berjalan tanpa GUI sama sekali, dioperasikan sepenuhnya via CLI atau remote access.

**App sandboxing** — teknik isolasi aplikasi agar satu aplikasi tidak bisa memengaruhi atau mengakses data aplikasi lain atau sistem di luar batasannya.

**Metadata** — informasi deskriptif tentang sebuah file, meliputi nama, ukuran, tipe, dan timestamp (waktu dibuat/dimodifikasi).

**Uptime** — durasi waktu sistem berjalan terus-menerus tanpa mati atau restart.

**Virtual machine (VM)** — simulasi komputer lengkap yang berjalan di atas komputer fisik, dikelola oleh OS host.

**Container** — unit software ringan yang mengemas aplikasi beserta semua dependensinya agar bisa berjalan konsisten di berbagai lingkungan.

**Real-Time OS (RTOS)** — OS yang dirancang untuk memberikan respons dalam waktu yang terjamin dan dapat diprediksi, digunakan di sistem kritis seperti avionik dan kontrol industri.

**ext4** — tipe file system Linux yang umum digunakan (Fourth Extended Filesystem), dikenal karena keandalan dan performa yang baik.

---

## 9. Tools & Platform Rujukan

Room ini tidak secara eksplisit menyebutkan tools atau platform eksternal. Namun, tools yang relevan untuk konteks room ini antara lain:

- **System Monitor (GNOME/MATE)** — aplikasi bawaan Linux untuk melihat informasi sistem, proses, resource usage, dan file system yang ter-mount
- **About This Computer** — shortcut di desktop Ubuntu MATE yang membuka System Monitor langsung ke tab informasi sistem

---

## 10. Catatan Ringkas untuk Ditulis Tangan

### OS — Dasar

- OS — software inti koordinasi user, app, hardware
- Posisi — User → Apps → OS → Hardware
- Tanpa OS — tiap app kelola hardware sendiri → konflik

### System Privilege Layers

- Kernel space — inti OS, akses penuh ke hardware, hanya kernel
- User space — tempat app biasa, akses hardware dibatasi
- System call — permintaan resmi app ke kernel untuk akses hardware
- Kernel — bagian OS yang langsung kelola hardware

### 5 Tugas Utama OS

- Process Management — jadwal/prioritas/hentikan proses, atur CPU time
- Memory Management — alokasi RAM, isolasi memori, pakai virtual memory kalau RAM habis
- File System Management — atur file, direktori, naming, path, permissions, metadata
- User Management — kelola akun, autentikasi, permissions
- Device Management — load driver, sediakan HAL (hardware abstraction layer)

### Keamanan Bawaan OS

- Authentication — verifikasi identitas (password, biometrik)
- Permissions — kontrol baca/tulis/eksekusi
- Isolation — tiap proses di kotak terpisah (kernel/user space)
- System Protection — lindungi file sistem kritis

### Interaksi dengan OS

- GUI — visual, klik/sentuh, intuitif, tidak butuh hafal perintah
- CLI — teks, ketik perintah, presisi tinggi, cepat untuk tugas lanjutan

### Tipe OS

- Desktop — PC harian, GUI kaya, multitasking
- Server — no GUI (headless), uptime max, multi-user
- Mobile — touch UI, hemat daya, sandboxing
- Embedded — sangat ringan, fungsi spesifik (IoT, router, mobil)
- Virtual/Cloud — ringan, skalabel, deployment cepat

### OS Dunia Nyata (Hafal)

**Desktop:** Windows (10/11), macOS (Sonoma/Sequoia/Tahoe), Linux (Ubuntu/Debian/Fedora)
**Server:** Windows Server (2016/2019/2022/2025), Linux (Ubuntu Server/CentOS/Red Hat), Unix (IBM AIX/Oracle Solaris)
**Mobile:** Android (14-16), iOS (17/18/26)
**Embedded:** Embedded Linux (OpenWrt/Yocto), RTOS (FreeRTOS/VxWorks/QNX)
**Cloud/VM:** Ubuntu LTS, Amazon Linux, Rocky Linux
**Container:** Alpine Linux, Bottlerocket AWS, Flatcar Linux

### Istilah Kunci

- Driver — software penghubung OS ↔ hardware
- HAL — lapisan universal agar app tidak perlu tahu detail hardware
- Virtual memory — RAM cadangan di storage
- Distribution — varian/turunan Linux (Ubuntu, Debian, Fedora)
- Headless — server tanpa GUI
- Sandboxing — isolasi app dari sistem dan app lain
- Metadata — info tentang file (nama, ukuran, tipe, waktu)
- ext4 — tipe file system Linux yang umum
- RTOS — OS untuk respons terjamin (kontrol pesawat, industri)
- Container — paket app + dependensi, lebih ringan dari VM

### Temuan Lab

- OS: Ubuntu MATE 24.04.1 LTS, Kernel Linux 6.8.0-1016-aws
- MATE: 1.26.2, RAM: 1.9 GiB, Disk: 9.1 GB available
- File system /dev/root → tipe: ext4
- Users di /home: alex, guest, ubuntu (3 direktori)
- Flag di /home/alex/Documents/note.txt → THM{new_pc_for_free!}
