# Resume: The Wonderful World of Operating Systems

**Tanggal:** 19 September 2026

---

## 1. Konsep Dasar Operating System

### 1.1 Definisi

**Operating system (OS)** adalah interface antara hardware komputer dan pengguna (user). OS bertanggung jawab membuat komputer berjalan seefisien mungkin sekaligus membuatnya mudah digunakan.

**Hardware** adalah komponen fisik dari sebuah komputer. Ini berbeda dengan OS — hardware adalah bagian fisiknya, sedangkan OS adalah software yang mengelola bagian fisik tersebut.

### 1.2 Fungsi Utama OS

OS menjadi jembatan komunikasi antara manusia dan komputer. Komputer secara internal berkomunikasi menggunakan bahasa **binary**, yaitu bahasa yang hanya terdiri dari angka 0 dan 1. OS menyediakan interface yang menjembatani kesenjangan komunikasi ini, sehingga manusia bisa berinteraksi dengan komputer dalam cara yang jauh lebih kompleks tanpa perlu memahami binary secara langsung.

Selain itu, OS modern memungkinkan komputer untuk:

- Menjalankan banyak aplikasi (multiple applications) secara bersamaan.
- Mengakses perangkat eksternal (external devices) seperti printer, keyboard, dan mouse.

### 1.3 Sejarah Singkat

Pada tahun 1950-an, komputer belum memiliki OS seperti sekarang. Komputer di era ini tidak bisa menjalankan banyak program secara simultan — pengguna harus menunggu satu program selesai, mereset komputer, lalu memuat (load) program berikutnya secara manual.

> *Bayangkan jika kamu harus menyalakan dan mematikan komputer setiap kali ingin membuka aplikasi baru — untuk mengirim satu email saja butuh waktu sangat lama.*

Keterbatasan inilah yang mendorong evolusi OS menjadi seperti yang digunakan saat ini, di mana efisiensi dan multitasking menjadi standar.

---

## 2. Klasifikasi Operating System

### 2.1 Windows dan macOS

Windows diperkenalkan tahun 1985, macOS tahun 1984. Keduanya digunakan di komputer personal maupun enterprise.

- **Windows**: closed-source — source code tidak dibagikan bebas ke publik.
- **macOS**: partially open source — sebagian komponen open-source (misalnya kernel-nya), sebagian lagi closed-source.

### 2.2 Linux

Versi pertama Linux dirilis tahun 1991, disusul rilis-rilis besar lainnya di awal 1990-an. Linux bersifat **completely open-source** — siapa pun bisa mengakses Linux beserta source code-nya, sehingga developer di komunitas Linux dapat berkolaborasi secara bebas.

Linux sangat penting bagi industri security. Ada distribusi Linux yang secara khusus dirancang untuk keperluan security (contoh distribusi yang disebut: **Ubuntu**).

### 2.3 ChromeOS

Diluncurkan tahun 2011. Bersifat **partially open source**, diturunkan dari **Chromium OS** yang sepenuhnya open source. ChromeOS sering digunakan di bidang pendidikan (education field).

### 2.4 Android dan iOS

Keduanya merupakan **mobile operating system**, digunakan pada perangkat mobile seperti telepon, tablet, dan jam tangan.

- **Android**: diperkenalkan 2008, bersifat open source.
- **iOS**: diperkenalkan 2007, bersifat partially open source.

### 2.5 Ringkasan Perbandingan

| OS | Tahun Rilis | Sifat Source |
|---|---|---|
| macOS | 1984 | Partially open source |
| Windows | 1985 | Closed source |
| Linux | 1991 | Completely open source |
| ChromeOS | 2011 | Partially open source |
| iOS | 2007 | Partially open source |
| Android | 2008 | Open source |

---

## 3. Operating System dan Keamanan (Security)

### 3.1 Kenapa OS Security Penting

Keamanan OS krusial untuk keamanan komputer secara keseluruhan. Ini mencakup pengamanan file, akses data, dan autentikasi pengguna (user authentication), guna melindungi dari ancaman seperti **virus**, **worm**, dan **malware**.

### 3.2 Tugas Security Analyst Terkait OS

Pemahaman mendalam tentang OS dibutuhkan untuk menjalankan tugas-tugas berikut:

- Mengonfigurasi dan menjaga keamanan sistem dengan mengelola akses (access management).
- Mengelola dan mengonfigurasi **firewall**.
- Menetapkan security policy.
- Mengaktifkan proteksi virus (virus protection).
- Melakukan auditing, accounting, dan logging untuk mendeteksi perilaku tidak biasa (unusual behavior).

### 3.3 Kerentanan (Vulnerabilities)

Masalah security tidak bisa dihindari pada semua OS. Bagian penting dari melindungi OS adalah menjaga sistem dan seluruh komponennya tetap up to date.

**Legacy operating system** adalah OS yang sudah usang (outdated) namun masih digunakan. Organisasi terus memakainya karena software yang mereka andalkan tidak kompatibel dengan OS yang lebih baru — kondisi ini lebih umum terjadi di industri yang bergantung pada **embedded software** (software yang ditanam di dalam komponen equipment). Legacy OS rentan terhadap masalah security karena sudah tidak lagi disupport atau diperbarui, sehingga berpotensi rentan terhadap ancaman baru.

Bahkan OS yang selalu diperbarui pun tetap bisa menjadi rentan terhadap serangan. Karena sulit menjaga semua sistem tetap up to date setiap saat, security analyst wajib memahami risiko yang ditimbulkan oleh legacy OS.

---

## 4. Perjalanan Karier di Cybersecurity (Studi Kasus: Kim, Google)

Kim adalah seorang **Technical Program Manager** di tim security, mergers, and acquisitions Google — bertugas membantu perusahaan hasil akuisisi berintegrasi ke lingkungan Google.

Latar belakang kariernya tidak linear: dimulai dari pekerja restoran, tutor bahasa Inggris untuk mahasiswa internasional, beberapa internship, baru kemudian masuk ke dunia teknologi dan akhirnya cybersecurity. Ia memulai dengan gelar bisnis, bukan gelar teknis.

Poin utama yang ia tekankan:

- Cybersecurity terbuka untuk siapa saja dari latar belakang apa pun — ketertarikan melindungi informasi dan orang lain sudah cukup sebagai modal awal.
- Skill dari pengalaman non-teknis tetap relevan dan bisa diterapkan di security.
- Skill yang paling ia andalkan adalah **membangun koneksi dengan orang lain** — pekerjaan tidak bisa selesai tanpa kolaborasi yang tepat dengan rekan kerja.
- Nasihat untuk pemula: pertahankan **open mind**. Self-doubt yang muncul di awal karier bisa tergantikan oleh dukungan dan rasa hormat dari rekan kerja seiring bertambahnya pengalaman.

---

## 5. Glossary (Istilah Kunci)

- **Operating System (OS)** — interface antara hardware komputer dan pengguna, mengelola resource hardware dan software.
- **Hardware** — komponen fisik dari komputer.
- **Binary** — bahasa komunikasi internal komputer, terdiri dari 0 dan 1.
- **Closed-source** — source code tidak dibagikan bebas ke publik (contoh: Windows).
- **Open-source** — source code dapat diakses bebas oleh siapa pun (contoh: Linux).
- **Partially open source** — sebagian komponen open-source, sebagian closed-source (contoh: macOS, ChromeOS, iOS).
- **Distribution (distro)** — varian dari Linux yang dikembangkan komunitas/vendor tertentu, sebagian dirancang khusus untuk security (contoh: Ubuntu).
- **Mobile Operating System** — OS yang dirancang untuk perangkat mobile seperti smartphone, tablet, dan smartwatch (contoh: Android, iOS).
- **Legacy Operating System** — OS yang sudah usang namun masih digunakan, biasanya karena kendala kompatibilitas software.
- **Embedded Software** — software yang ditanam di dalam komponen fisik suatu equipment/perangkat.
- **Firewall** — sistem yang mengatur dan menyaring lalu lintas jaringan untuk keperluan security.
- **Malware, Virus, Worm** — jenis-jenis ancaman/software berbahaya yang menjadi target proteksi OS security.
- **Vulnerability** — celah keamanan pada sistem/OS yang berpotensi dieksploitasi.
- **Auditing, Accounting, Logging** — aktivitas pencatatan dan pemeriksaan aktivitas sistem untuk mendeteksi perilaku mencurigakan.

---

## 6. Tools & Platform Rujukan

- **Microsoft Security Response Center (MSRC)** — daftar kerentanan (known vulnerabilities) yang memengaruhi produk dan layanan Microsoft.
- **Apple Security Updates** — daftar pembaruan security dan informasi untuk OS Apple (macOS, iOS) serta produk lainnya.
- **Common Vulnerabilities and Exposures (CVE) Report for Ubuntu** — daftar kerentanan yang diketahui memengaruhi Ubuntu (distribusi Linux).
- **Google Cloud Security Bulletin** — daftar kerentanan yang diketahui memengaruhi produk dan layanan Google Cloud.

*(Catatan: keempat resource ini disebutkan sebagai hyperlink pada materi asli, namun URL spesifik tidak tercantum secara eksplisit dalam screenshot yang tersedia.)*

---

## 7. Catatan Ringkas untuk Ditulis Tangan

**Konsep Dasar**
- OS — interface hardware & user
- Hardware — komponen fisik komputer
- Binary — bahasa komputer, 0 dan 1
- OS jembatani manusia ↔ komputer
- OS modern: multitasking + akses device eksternal (printer, keyboard, mouse)
- 1950-an: komputer belum ada OS, 1 program per waktu, reset manual

**Jenis OS & Tahun**
- macOS — 1984 — partially open source
- Windows — 1985 — closed source
- Linux — 1991 — completely open source
- ChromeOS — 2011 — partially open source (turunan Chromium OS)
- iOS — 2007 — partially open source
- Android — 2008 — open source
- Linux = penting utk security, ada distro khusus security (Ubuntu)
- ChromeOS = banyak dipakai bidang pendidikan

**Security & OS**
- OS security lindungi: file, akses data, autentikasi
- Ancaman: virus, worm, malware
- Tugas security analyst: manage access, firewall, security policy, virus protection, auditing/accounting/logging
- Legacy OS — usang tapi masih dipakai, ga disupport/update lagi, rawan ancaman baru
- Sebab pakai legacy OS — software lama gak kompatibel OS baru, embedded software
- OS up to date pun tetap bisa vulnerable

**Career Insight (Kim - Google)**
- Role: Technical Program Manager, security M&A team
- Background: restoran → tutor bahasa Inggris → internship → tech → cybersecurity
- Skill utama: connecting with people
- Advice: keep open mind, self-doubt bisa hilang seiring waktu & dukungan peer

**Tools Rujukan**
- MSRC — vuln Microsoft
- Apple Security Updates — vuln macOS/iOS
- CVE Report Ubuntu — vuln Ubuntu/Linux
- Google Cloud Security Bulletin — vuln Google Cloud
