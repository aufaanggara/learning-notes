# RESUME MATERI — SYSTEMS AS ATTACK VECTORS
**Room:** Systems as Attack Vectors (TryHackMe) | **Tanggal:** 8 Agustus 2026

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 1. KONSEP INTI: SISTEM SEBAGAI ATTACK VECTOR

Sebuah **sistem** adalah tempat penyimpanan dan pemrosesan data digital — bisa berupa server fisik, mesin lab, atau platform cloud seperti Microsoft 365. Contoh: bank menyimpan data kartu di sebuah sistem, email tersimpan di mail server.

Nilai sebuah sistem bagi penyerang ditentukan oleh **skala dampak** jika sistem itu dibobol, bukan sekadar jenis sistemnya. Membobol satu mailbox pengguna via phishing hanya memberi akses ke satu akun, sedangkan membobol mail server memberi akses ke ribuan mailbox sekaligus.

Tabel nilai serangan berdasarkan jenis sistem yang dibobol:

| Sistem yang Dibobol | Nilai bagi Penyerang |
|---|---|
| Laptop pribadi siswa | Curi profil game, jadikan bagian botnet |
| Laptop admin IT senior bank | Akses ke sistem perbankan internal |
| Mail server perusahaan hukum | Bocorkan seluruh mailbox, pemerasan |
| Server inti jaringan industri | Enkripsi seluruh jaringan (ransomware) |
| Panel manajemen website pemerintah | Defacement / aktivisme |

Prinsip kunci: penyerang tidak membedakan **human hacking** dan **system hacking** sebagai dua hal terpisah — keduanya adalah jalur masuk yang setara nilainya, sehingga upaya perlindungan (mitigasi dan deteksi) harus diberikan secara seimbang pada manusia maupun sistem.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 2. TIGA JALUR UTAMA SERANGAN PADA SISTEM

Dalam serangan serius, tujuan pertama penyerang selalu sama: mendapatkan **akses** ke sistem target. Apa yang terjadi setelahnya (pencurian data, ransomware, penghancuran data) tergantung motivasi. Ada tiga jalur utama bagaimana sistem diserang:

### 2.1 Human-Led Attacks (Serangan yang Dipicu Manusia)

Pengguna sistem sering menjadi titik awal serangan melalui kebiasaan berisiko:
- Memasukkan USB berbahaya yang ditemukan sembarangan
- Mengunduh malware dari sumber bajakan
- Menggunakan ulang password lemah di banyak akun

Data pendukung: **81% dari kebocoran data (breaches)** melibatkan password yang dicuri atau bocor.

**RubberDucky** adalah contoh perangkat USB berbahaya yang menjalankan malware secara otomatis begitu dicolokkan ke komputer, tanpa memerlukan interaksi tambahan dari korban.

### 2.2 Vulnerabilities (Kerentanan)

Setiap software berpotensi memiliki celah keamanan (flaws). Beberapa celah butuh waktu sangat lama untuk ditemukan — contohnya **Shellshock**, kerentanan Linux yang sudah ada sejak 1992 namun baru ditemukan tahun 2014.

Data pendukung (2024):
- Lebih dari **40.000** kerentanan software dipublikasikan
- Lebih dari **300** di antaranya dieksploitasi secara aktif dalam serangan besar
- Lebih dari **100.000** host Windows outdated (mis. Windows XP, Server 2008) masih ditemukan di seluruh dunia
- **325** kerentanan kritis baru tercatat di CISA KEV Catalog sejak 2024, dengan Microsoft menyumbang jumlah terbanyak (60)

Contoh CVE penting yang dibahas sebagai timeline kerentanan Windows:
- **CVE-2017-0144 (EternalBlue)** — kerentanan SMB kritis
- **CVE-2019-0708**
- **CVE-2020-1472**
- **CVE-2021-34527 (PrintNightmare)** — celah kritis pada Print Spooler
- **CVE-2022-30190 (Follina)** — celah kritis pada MS Office
- **CVE-2023-24880** — celah bypass keamanan zero-day
- **CVE-2025-53770 (ToolShell)** — kerentanan RCE kritis pada SharePoint on-premise, memungkinkan penyerang tidak terautentikasi mengeksekusi kode dari jarak jauh

### 2.3 Supply Chain Attacks (Serangan Rantai Pasok)

Setiap aplikasi bergantung pada ribuan **library**. Jika penyerang berhasil membobol satu aplikasi/library dan mendorong update berbahaya ke seluruh penggunanya, semua pengguna itu ikut terkompromi. Contoh nyata: **SolarWinds** dan **3CX**, keduanya berdampak pada ribuan perusahaan. TryHackMe sendiri pernah menjadi korban serangan supply chain melalui **Lottie Player**, library animasi yang mereka gunakan.

Ciri khas serangan supply chain: software atau aplikasi yang selama ini tepercaya, tiba-tiba mulai berperilaku mencurigakan (menjalankan perintah berbahaya) segera setelah menerima update.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 3. KERENTANAN vs MISCONFIGURATION

Dua sumber celah keamanan sistem memiliki sifat yang berbeda secara mendasar:

**Vulnerability (Kerentanan)** — bug pada software itu sendiri. Setelah dipublikasikan, diberi nomor **CVE (Common Vulnerabilities and Exposures)**. Sejak dipublikasikan, terjadi perlombaan: penyerang mengembangkan exploit, sementara defender bergegas menerapkan patch.

**Misconfiguration (Kesalahan Konfigurasi)** — bukan bug, melainkan kesalahan dalam cara sistem disetel, umumnya oleh tim IT, sering terjadi karena ingin menyederhanakan proses (contoh: memakai password sederhana seperti "1111").

Contoh kasus nyata misconfiguration:
- Password "123456" membocorkan data chat 64 juta lamaran kerja McDonald's
- Cloud AWS yang salah dikonfigurasi menyebabkan kebocoran data 106 juta nasabah bank
- Smart fridge yang salah dikonfigurasi digunakan diam-diam dalam serangan botnet

Perbedaan kunci dalam penanganan: **vulnerability butuh patch (update software)**, sedangkan **misconfiguration cukup dibenahi lewat setup yang lebih baik**, tanpa perlu update software.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 4. STRATEGI RESPONS DAN MITIGASI

### 4.1 Merespons Vulnerability (Saat Patch Belum Tersedia)

Jawaban utama untuk sebuah CVE selalu berupa **patch** dari vendor. Selama menunggu patch (termasuk untuk kasus zero-day), langkah sementara yang bisa diambil:
- Membatasi akses sistem hanya untuk IP tepercaya
- Menerapkan mitigasi sementara yang disediakan vendor
- Memblokir pola serangan yang sudah dikenal di **IPS (Intrusion Prevention System)** atau **WAF (Web Application Firewall)**

### 4.2 Merespons Misconfiguration (Proaktif)

Karena analis SOC sering baru menyadari misconfiguration setelah dieksploitasi, di perusahaan kecil analis juga bisa berperan proaktif melalui:
- **Penetration Testing** — menyewa ethical hacker untuk mensimulasikan serangan dan melaporkan celah yang ditemukan
- **Vulnerability Scans** — menjalankan tool secara berkala untuk mendeteksi password default atau software usang
- **Configuration Audits** — meninjau manual kesesuaian sistem terhadap best practice seperti **CIS benchmarks**

### 4.3 Mitigasi Umum untuk Melindungi Sistem

| Mitigasi | Fungsi |
|---|---|
| **Patch Management** | Proses melacak dan menambal sistem rentan secara sistematis, signifikan menurunkan peluang serangan berhasil |
| **Training for IT** | Tim IT yang paham risiko misconfiguration lebih kecil kemungkinan meninggalkan sistem tanpa perlindungan |
| **Network Protection** | Membatasi akses sistem hanya untuk orang/IP tepercaya membuat sistem jauh lebih sulit dibobol |
| **Antivirus Protection** | Menghentikan atau mendeteksi berbagai jenis serangan pada sistem, sama seperti fungsinya pada manusia |

Alur pertahanan berlapis: serangan yang berhasil dihalangi oleh **patched software** dan **antivirus solution** akan menyisakan sedikit ancaman yang kemudian ditangani langsung oleh **tim SOC**.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 5. PRINSIP DASAR: MENGAPA HAL INI PENTING BAGI SOC ANALYST

Analis SOC umumnya tidak mengelola sistem secara langsung, namun pemahaman terhadap pola serangan dan pertahanan sistem tetap krusial karena dua alasan:
1. Memperluas perspektif keamanan siber analis secara menyeluruh, tidak hanya berfokus pada sisi human-attack vector
2. Memungkinkan analis membagikan pengetahuan tersebut kepada departemen IT sebagai bentuk kolaborasi lintas tim

Praktik yang disarankan agar tetap relevan di bidang ini: selalu mengikuti perkembangan ancaman terbaru dan membagikannya kepada rekan kerja.

Referensi tambahan yang direkomendasikan untuk pembelajaran lanjutan:
- The DFIR Report — dokumentasi bagaimana intrusi nyata terjadi
- CISA Known Exploited Vulnerabilities Catalog — katalog kerentanan yang sudah diketahui dieksploitasi
- BleepingComputer — berita serangan supply chain terbaru
- CheckPoint — peta ancaman siber interaktif secara live

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 6. GLOSARIUM ISTILAH KUNCI

**System** — tempat penyimpanan/pemrosesan data digital: server fisik, mesin lab, atau platform cloud.

**Threat Actor** — pelaku yang melancarkan serangan siber.

**Zero-day** — kerentanan yang ditemukan penyerang sebelum diketahui pihak defender/vendor, sehingga belum ada patch yang tersedia.

**CVE (Common Vulnerabilities and Exposures)** — nomor identifikasi resmi yang diberikan pada kerentanan setelah dipublikasikan.

**Patch** — update resmi dari vendor software untuk menutup sebuah kerentanan (CVE).

**Vulnerability** — bug atau celah keamanan pada software itu sendiri.

**Misconfiguration** — kesalahan dalam cara sistem disetel/dikonfigurasi, bukan bug pada software.

**Supply Chain Attack** — serangan yang membobol satu aplikasi/library kemudian menyebar ke seluruh pengguna melalui update yang telah disusupi.

**RubberDucky** — perangkat USB yang menjalankan malware otomatis begitu dicolokkan.

**IPS (Intrusion Prevention System)** — sistem yang memblokir pola serangan jaringan yang dikenal secara real-time.

**WAF (Web Application Firewall)** — firewall yang memfilter dan memblokir serangan pada level aplikasi web.

**Penetration Testing** — simulasi serangan oleh ethical hacker untuk menemukan celah keamanan sebelum dieksploitasi pihak jahat.

**Vulnerability Scan** — pemindaian otomatis dan berkala untuk mendeteksi celah keamanan seperti password default atau software usang.

**Configuration Audit** — peninjauan manual kesesuaian konfigurasi sistem terhadap standar keamanan (contoh: CIS benchmarks).

**CIS Benchmarks** — kumpulan standar/best practice konfigurasi keamanan sistem yang diakui secara luas.

**Patch Management** — proses sistematis melacak dan menerapkan patch pada sistem yang rentan.

**ToolShell (CVE-2025-53770)** — kerentanan RCE kritis pada SharePoint on-premise yang memungkinkan akses dan eksekusi kode tanpa autentikasi.