# Resume Materi: OS Hardening dan Brute Force Attacks

Coursera Cybersecurity | Security Hardening | 1 November 2026

Cakupan resume: video OS hardening, bacaan Brute force attacks and OS hardening, activity Apply OS hardening techniques beserta exemplar, dan kuis Test your knowledge: OS hardening.

## 1. Konsep Inti

### 1.1 Sistem Operasi dan Alasan Pengamanannya

**Operating system (OS)** adalah antarmuka antara perangkat keras komputer dan pengguna. OS adalah program pertama yang dimuat saat komputer dinyalakan, dan bertindak sebagai perantara antara aplikasi perangkat lunak dan perangkat keras.

Satu OS yang tidak aman dapat menyebabkan seluruh jaringan terkompromi, sehingga OS pada setiap sistem harus diamankan. Semua jenis OS memiliki praktik hardening yang serupa.

### 1.2 Security Hardening

**Security hardening** adalah proses memperkuat sebuah sistem untuk mengurangi kerentanan (*vulnerability*) dan **attack surface**-nya. **OS hardening** adalah rangkaian prosedur untuk menjaga dan meningkatkan keamanan OS.

Langkah seperti **access privileges** dan **password policies** menjalani pemeriksaan keamanan secara rutin sebagai bagian dari OS hardening.

### 1.3 Dua Kelompok Tugas Hardening

Tugas hardening dibedakan berdasarkan frekuensi pelaksanaannya.

- **Dilakukan berkala:** patch update, backup, menjaga daftar perangkat dan pengguna berwenang tetap terbaru, serta pembuangan hardware dan software.
- **Dilakukan satu kali sebagai langkah pengamanan awal:** contohnya mengonfigurasi pengaturan perangkat agar sesuai dengan standar enkripsi yang aman.

## 2. Tugas OS Hardening

### 2.1 Patch Update

**Patch update** adalah pembaruan software dan OS yang menutup celah keamanan di dalam suatu program atau produk. Patch yang disediakan vendor OS harus membawa OS ke versi terbarunya.

Begitu vendor mempublikasikan patch beserta perbaikan celahnya, pelaku jahat (*malicious actors*) langsung tahu letak celah pada sistem yang masih memakai OS lama. Karena itu patch harus dipasang secepat mungkin setelah dirilis.

Contoh dari video: tim pembicara melakukan **emergency patch** atas kerentanan pada sebuah programming library yang dipakai hampir di mana-mana, sehingga sebagian besar server dan aplikasi harus segera ditambal.

### 2.2 Baseline Configuration

**Baseline configuration** (disebut juga **baseline image**) adalah kumpulan spesifikasi terdokumentasi dalam sebuah sistem yang menjadi dasar untuk build, rilis, dan update berikutnya. OS yang baru diperbarui ditambahkan ke baseline ini.

Contoh isi baseline adalah aturan **firewall** berupa daftar port jaringan yang diizinkan dan tidak diizinkan.

Fungsinya dalam investigasi: bila tim keamanan mencurigai aktivitas tidak biasa pada OS, konfigurasi saat ini dibandingkan dengan baseline untuk memastikan tidak ada yang berubah.

### 2.3 Hardware and Software Disposal

Perangkat keras lama harus di-**wipe** dan dibuang dengan benar. Aplikasi yang tidak lagi dipakai sebaiknya dihapus, karena beberapa bahasa pemrograman populer memiliki kerentanan yang sudah diketahui.

Menghapus software yang tidak terpakai berarti tidak ada kerentanan tambahan dari program-program yang digunakan software tersebut.

### 2.4 Strong Password Policy dan MFA

**Strong password policy** mewajibkan password mengikuti aturan tertentu, misalnya minimal delapan karakter dengan huruf kapital, angka, dan simbol. Kebijakan juga lazim menetapkan bahwa pengguna kehilangan akses ke jaringan setelah salah memasukkan password beberapa kali berturut-turut.

**Multi-factor authentication (MFA)** mewajibkan pengguna memverifikasi identitas dengan dua cara atau lebih. Tiga jenis faktor:

- Sesuatu yang diketahui, seperti password.
- Sesuatu yang dimiliki, seperti kartu ID.
- Sesuatu yang unik pada diri pengguna, seperti sidik jari.

### 2.5 Physical Security Hardening

**Physical security** juga bagian dari security hardening. Contohnya mengamankan ruang fisik dengan kamera keamanan dan petugas keamanan.

Menghapus atau menonaktifkan aplikasi tidak terpakai dan mengurangi izin akses antarperangkat termasuk hardening pada sisi sistem, bukan contoh physical security hardening.

## 3. Brute Force Attacks

### 3.1 Definisi dan Jenis

Username dan password adalah kontrol keamanan yang paling umum, tetapi kelemahan utamanya adalah kredensial dapat dicuri dan ditebak. **Brute force attack** adalah proses coba-coba (*trial-and-error*) untuk menemukan informasi privat, dengan tujuan mendapatkan akses dan mengompromikan jaringan.

Dua jenis yang dibahas:

- **Simple brute force attack:** penyerang memasukkan kombinasi username dan password apa pun yang terpikirkan sampai ada yang berhasil.
- **Dictionary attack:** penyerang memakai daftar password umum dan kredensial curian dari kebocoran data sebelumnya. Dinamai demikian karena awalnya yang dipakai adalah daftar kata dari kamus, sebelum aturan password kompleks menjadi praktik umum.

Serangan manual membosankan dan memakan waktu, sehingga penyerang memakai berbagai tools untuk mengotomatiskannya.

### 3.2 Menilai Kerentanan

Sebelum insiden terjadi, perusahaan dapat menjalankan serangkaian pengujian pada jaringan atau aplikasi web. Analis memakai **virtual machine** dan **sandbox** untuk menguji file mencurigakan, memeriksa kerentanan, atau mensimulasikan insiden.

**Virtual machine (VM)** adalah versi software dari komputer fisik.

- Kode dijalankan di lingkungan terisolasi sehingga tidak memengaruhi komputer atau sistem lain.
- Setelah pengujian malware, VM dapat dihapus dan diganti dengan **pristine image**, atau dikembalikan ke kondisi sebelumnya.
- Mudah berpindah antar VM, sehingga tugas keamanan lebih efisien.
- Risiko: masih ada kemungkinan kecil program berbahaya lolos dari virtualisasi dan mengakses mesin **host**.

**Sandbox** adalah lingkungan pengujian yang memungkinkan software dijalankan terpisah dari jaringan.

- Dipakai untuk menguji patch, menangani bug, mendeteksi kerentanan, mengevaluasi software atau file berkode berbahaya, dan mensimulasikan skenario serangan.
- Bisa berupa komputer fisik mandiri tanpa koneksi jaringan, tetapi VM berbasis software atau cloud biasanya lebih hemat waktu dan biaya.
- Risiko: sebagian pembuat malware memprogram malware agar mendeteksi lingkungan VM atau sandbox, lalu berperilaku seperti software tidak berbahaya selama pengujian.

### 3.3 Langkah Pencegahan

- **Salting and hashing.** Hashing mengubah informasi menjadi nilai unik untuk menentukan integritasnya; ini fungsi satu arah (*one-way function*), sehingga teks asli mustahil didekripsi. Salting menambahkan karakter acak ke password yang sudah di-hash, sehingga nilai hash lebih panjang dan kompleks.
- **MFA dan 2FA.** MFA memverifikasi identitas lewat dua cara atau lebih, misalnya username dan password, sidik jari, pengenalan wajah, atau **one-time password (OTP)** yang dikirim ke nomor telepon atau email. **2FA** sama dengan MFA tetapi hanya memakai dua bentuk verifikasi.
- **CAPTCHA dan reCAPTCHA.** CAPTCHA (*Completely Automated Public Turing test to tell Computers and Humans Apart*) meminta pengguna menyelesaikan tes sederhana untuk membuktikan dirinya manusia, sehingga software tidak dapat melakukan brute force terhadap password. **reCAPTCHA** adalah layanan CAPTCHA gratis dari Google untuk melindungi situs dari bot dan software berbahaya.
- **Password policies.** Menstandarkan praktik password: tingkat kompleksitas, frekuensi pembaruan, boleh tidaknya password dipakai ulang, dan batas jumlah percobaan login sebelum akun ditangguhkan.

Pilihan remediasi lain yang disebut pada activity: mewajibkan password kuat, memantau upaya login, mewajibkan perubahan password lebih sering, melarang penggunaan ulang password sebelumnya, dan membatasi jumlah upaya login. Semakin banyak langkah diterapkan, semakin kecil peluang pelaku jahat mengakses informasi sensitif.

## 4. Studi Kasus: yummyrecipesforme.com

### 4.1 Skenario

Seorang mantan karyawan menyerang yummyrecipesforme.com, situs penjual resep dan buku masak, untuk mengarahkan pengguna ke situs palsu berisi malware.

Urutan serangan:

- Pelaku melakukan brute force pada akun administratif dengan mencoba beberapa password default yang diketahui sampai berhasil.
- Dengan kredensial itu ia masuk ke panel admin dan mengubah source code, menanamkan fungsi **JavaScript** yang meminta pengunjung mengunduh dan menjalankan sebuah file.
- Setelah malware tertanam, ia mengubah password akun admin sehingga pemilik situs tidak bisa login.
- Pelanggan yang menjalankan file dialihkan ke versi palsu situs, dan komputer mereka berjalan lebih lambat.

Deteksi: beberapa jam kemudian banyak pelanggan menghubungi helpdesk. Pemilik gagal login ke panel admin lalu menghubungi penyedia hosting, dan tim analis ditugaskan menyelidiki.

### 4.2 Investigasi dengan tcpdump

Analis membuat lingkungan **sandbox**, menjalankan network protocol analyzer **tcpdump**, lalu membuka URL situs. Setelah halaman dimuat muncul permintaan mengunduh file executable untuk memperbarui browser. Setelah file dijalankan, browser dialihkan ke greatrecipesforme.com yang berisi malware.

Alur yang terekam di log:

| Langkah | Aktivitas | Protocol |
|---|---|---|
| 1 | Browser meminta IP untuk yummyrecipesforme.com ke DNS server | DNS |
| 2 | DNS membalas dengan IP yang benar | DNS |
| 3 | Browser meminta halaman web memakai IP tersebut | HTTP |
| 4 | Browser memulai unduhan malware | Tidak dirinci |
| 5 | Browser meminta IP untuk greatrecipesforme.com | DNS |
| 6 | DNS server membalas dengan IP greatrecipesforme.com | DNS |
| 7 | Browser mengirim permintaan ke IP greatrecipesforme.com | HTTP |

Hasil analisis source code oleh senior analyst: kode JavaScript ditambahkan untuk meminta pengunjung mengunduh file executable, dan file itu berisi script yang mengalihkan browser dari yummyrecipesforme.com ke greatrecipesforme.com.

### 4.3 Akar Masalah

Web server terdampak brute force attack karena password admin masih berupa password default, dan tidak ada kontrol yang mencegah brute force.

### 4.4 Penyusunan Laporan Insiden

Laporan memakai **security incident report template** dengan tiga bagian:

- Bagian pertama: network protocol yang teridentifikasi dari log tcpdump, berdasarkan empat layer **TCP/IP model**.
- Bagian kedua: dokumentasi insiden.
- Bagian ketiga: satu langkah keamanan yang direkomendasikan beserta alasan efektivitasnya.

Aturan dokumentasi insiden:

- Hindari bahasa emosional yang kuat seperti "bagus", "buruk", atau "mengerikan".
- Cantumkan fakta sebanyak mungkin: lokasi kejadian, cara terjadinya, apakah ada saksi, dan cara ditemukannya.
- Sebutkan sumber informasi dan bukti.

Dokumentasi yang akurat menjadi acuan analis lain, bahan edukasi karyawan tentang langkah keamanan perusahaan, dan membantu bisnis memenuhi berbagai audit keamanan.

### 4.5 Pelajaran Utama

Analis tidak selalu langsung tahu penyebab utama masalah jaringan atau serangan. Kemampuan menganalisis protocol yang terlibat memungkinkan analis membuat asumsi yang terinformasi tentang apa yang terjadi, sehingga tim dapat mulai menyelesaikan masalah. Dokumentasi yang baik menghemat waktu organisasi dan dapat membantu menangani serangan sejak dini.

## 5. Glosarium

**Access privileges.** Hak akses pengguna terhadap sistem atau data; diperiksa rutin dalam OS hardening.

**Attack surface.** Seluruh titik yang dapat dimanfaatkan penyerang; dikurangi melalui security hardening.

**Baseline configuration (baseline image).** Spesifikasi sistem terdokumentasi yang menjadi dasar build, rilis, dan update berikutnya.

**Brute force attack.** Proses coba-coba untuk menemukan informasi privat seperti password.

**CAPTCHA.** Tes sederhana untuk membedakan manusia dari komputer.

**Dictionary attack.** Brute force memakai daftar password umum dan kredensial curian dari kebocoran sebelumnya.

**DNS.** Protocol yang menerjemahkan nama domain menjadi alamat IP.

**Hashing.** Fungsi satu arah yang mengubah informasi menjadi nilai unik.

**HTTP.** Protocol untuk meminta halaman web dari alamat IP server.

**MFA / 2FA.** Verifikasi identitas dengan dua cara atau lebih (2FA: tepat dua).

**Network protocol.** Aturan dan standar komunikasi yang dipakai perangkat jaringan untuk mengirim data.

**One-time password (OTP).** Kode sekali pakai yang dikirim ke telepon atau email.

**Operating system (OS).** Antarmuka antara hardware dan pengguna; perantara aplikasi dan hardware.

**Patch update.** Pembaruan software atau OS yang menutup celah keamanan.

**Pristine image.** Image bersih yang menggantikan VM setelah pengujian malware.

**reCAPTCHA.** Layanan CAPTCHA gratis dari Google.

**Salting.** Penambahan karakter acak ke password yang sudah di-hash.

**Sandbox.** Lingkungan pengujian terpisah dari jaringan untuk menjalankan software.

**Security hardening.** Proses memperkuat sistem untuk mengurangi kerentanan dan attack surface.

**tcpdump.** Network protocol analyzer untuk menangkap dan membaca lalu lintas jaringan.

**Virtual machine (VM).** Versi software dari komputer fisik untuk menjalankan kode secara terisolasi.

## 6. Tools & Platform Rujukan

- **tcpdump** — network protocol analyzer untuk merekam lalu lintas jaringan saat investigasi.
- **reCAPTCHA** (Google) — layanan CAPTCHA gratis untuk melindungi situs dari bot dan software berbahaya.
- **Virtual machine** — lingkungan terisolasi untuk menguji file mencurigakan dan menjalankan malware.
- **Sandbox** — lingkungan pengujian terpisah dari jaringan, bisa fisik, software, atau cloud.
- **Security incident report template** — template laporan insiden tiga bagian yang dipakai pada activity.

Materi tidak menyebutkan URL eksplisit untuk item di atas.

## 7. Catatan Ringkas untuk Ditulis Tangan

### Dasar

- OS — antarmuka hardware dan pengguna; program pertama dimuat; perantara aplikasi dan hardware
- Satu OS tidak aman — seluruh jaringan bisa terkompromi
- Security hardening — perkuat sistem, kurangi kerentanan dan attack surface
- OS hardening — prosedur menjaga dan meningkatkan keamanan OS

### Frekuensi Tugas

- Berkala — update, backup, daftar perangkat dan pengguna berwenang, disposal
- Sekali — konfigurasi perangkat sesuai standar enkripsi aman

### Tugas Hardening

- Patch update — tutup celah; pasang secepat mungkin karena celah jadi publik saat patch rilis
- Emergency patch — contoh: kerentanan programming library yang dipakai luas
- Baseline configuration (baseline image) — spesifikasi terdokumentasi; dasar build dan update; pembanding saat ada aktivitas aneh
- Contoh isi baseline — aturan firewall, daftar port diizinkan dan dilarang
- Hardware disposal — wipe lalu buang dengan benar
- Software disposal — hapus aplikasi tak terpakai; kurangi kerentanan bawaan
- Strong password policy — min. 8 karakter, kapital, angka, simbol; akses dikunci setelah salah beberapa kali
- MFA — verifikasi dua cara atau lebih; tahu, punya, unik pada diri
- Physical security — kamera keamanan, petugas keamanan

### Brute Force

- Brute force — coba-coba untuk menebak informasi privat
- Simple brute force — tebak kombinasi username dan password apa pun
- Dictionary attack — daftar password umum dan kredensial dari breach
- Serangan manual lambat — penyerang memakai tools

### Menilai Kerentanan

- VM — komputer versi software; isolasi kode; bisa dihapus atau diganti pristine image
- Risiko VM — kecil kemungkinan malware lolos ke host
- Sandbox — uji software terpisah dari jaringan; patch, bug, kerentanan, simulasi serangan
- Risiko sandbox — malware bisa mendeteksi VM atau sandbox lalu berpura-pura jinak
- Sandbox fisik vs VM atau cloud — VM atau cloud lebih hemat waktu dan biaya

### Pencegahan

- Hashing — fungsi satu arah; teks asli tak bisa didekripsi
- Salting — tambah karakter acak ke hash
- MFA — dua cara atau lebih (password, sidik jari, wajah, OTP)
- 2FA — MFA dengan tepat dua verifikasi
- CAPTCHA — tes manusia atau komputer
- reCAPTCHA — CAPTCHA gratis Google
- Password policies — kompleksitas, frekuensi ganti, pakai ulang, batas percobaan login
- Opsi remediasi activity — password kuat, 2FA, pantau login, ganti lebih sering, larang password lama, batasi percobaan

### Studi Kasus

- Target — yummyrecipesforme.com
- Pelaku — mantan karyawan; brute force dengan password default admin
- Dampak — JavaScript berbahaya ditanam; pengunjung diminta unduh file; dialihkan ke greatrecipesforme.com
- Setelah serangan — password admin diubah pelaku; pemilik terkunci
- Alat investigasi — sandbox, tcpdump
- Urutan log — DNS, DNS, HTTP, unduh malware, DNS, DNS, HTTP
- Protocol teridentifikasi — DNS dan HTTP
- Akar masalah — password default, tanpa kontrol anti brute force
- Laporan — bagian 1 protocol, bagian 2 dokumentasi insiden, bagian 3 satu rekomendasi
- Dokumentasi — tanpa bahasa emosional, banyak fakta, sebut sumber bukti
- Manfaat dokumentasi — acuan analis, edukasi karyawan, kepatuhan audit
- Pelajaran — analisis protocol bantu buat asumsi terinformasi
