# Resume Materi — Systems as Attack Vectors

Room: TryHackMe — Systems as Attack Vectors
Tanggal: 12 Agustus 2026

## 1. Konsep Inti: Sistem sebagai Attack Vector

Sebuah **sistem** adalah tempat penyimpanan dan pemrosesan data digital, bisa berupa server fisik, mesin lab, atau platform cloud seperti Microsoft 365.

Nilai sebuah sistem bagi penyerang ditentukan oleh skala dampak jika sistem itu dibobol, bukan sekadar jenisnya. Membobol satu mailbox pengguna lewat phishing hanya memberi akses ke satu akun. Membobol mail server memberi akses ke ribuan mailbox sekaligus.

### 1.1 Contoh Nilai Serangan Berdasarkan Sistem yang Dibobol

- Laptop pribadi siswa sekolah — curi profil game, jadikan bagian botnet.
- Laptop admin IT senior bank — akses ke sistem perbankan internal.
- Mail server perusahaan hukum — bocorkan seluruh mailbox, pemerasan.
- Server inti jaringan industri — enkripsi seluruh jaringan (ransomware).
- Panel manajemen website pemerintah — defacement atau aktivisme.

### 1.2 Prinsip Kunci

Penyerang tidak membedakan **human hacking** dan **system hacking** sebagai dua hal terpisah. Keduanya adalah jalur masuk yang setara nilainya, sehingga mitigasi dan deteksi harus diberikan secara seimbang pada manusia maupun sistem.

## 2. Tiga Jalur Utama Serangan pada Sistem

Tujuan pertama penyerang dalam serangan serius selalu sama: mendapatkan **akses** ke sistem target. Apa yang terjadi setelahnya (pencurian data, ransomware, penghancuran data) tergantung motivasi.

### 2.1 Human-Led Attacks

Pengguna sistem sering menjadi titik awal serangan melalui:

- Memasukkan USB berbahaya yang ditemukan sembarangan.
- Mengunduh malware dari sumber bajakan.
- Menggunakan ulang password lemah di banyak akun.

Data pendukung: **81%** dari kebocoran data (breaches) melibatkan password yang dicuri atau bocor.

**RubberDucky** adalah perangkat USB yang menjalankan malware otomatis begitu dicolokkan ke komputer, tanpa memerlukan interaksi tambahan dari korban.

### 2.2 Vulnerabilities (Kerentanan)

Setiap software berpotensi memiliki celah keamanan (flaws). Beberapa celah butuh waktu sangat lama untuk ditemukan. Contohnya **Shellshock**, kerentanan Linux yang sudah ada sejak 1992 namun baru ditemukan tahun 2014.

**Zero-day** adalah kerentanan yang ditemukan penyerang sebelum diketahui pihak defender, sehingga belum ada patch yang tersedia.

Data pendukung (2024):

- Lebih dari **40.000** kerentanan software dipublikasikan.
- Lebih dari **300** di antaranya dieksploitasi secara aktif dalam serangan besar.
- Lebih dari **100.000** host Windows outdated (Windows XP, Server 2008) masih ditemukan di seluruh dunia.
- **325** kerentanan kritis baru tercatat di CISA KEV Catalog sejak 2024, dengan Microsoft menyumbang jumlah terbanyak (60).

Begitu sebuah kerentanan dipublikasikan, ia diberi nomor **CVE (Common Vulnerabilities and Exposures)**. Sejak saat itu terjadi perlombaan: penyerang mengembangkan exploit, defender bergegas menerapkan patch.

Contoh CVE penting dalam timeline kerentanan Windows:

- **CVE-2017-0144 (EternalBlue)** — kerentanan SMB kritis.
- **CVE-2019-0708**
- **CVE-2020-1472**
- **CVE-2021-34527 (PrintNightmare)** — celah kritis pada Print Spooler.
- **CVE-2022-30190 (Follina)** — celah kritis pada MS Office.
- **CVE-2023-24880** — celah bypass keamanan zero-day.
- **CVE-2025-53770 (ToolShell)** — kerentanan RCE kritis pada SharePoint on-premise, memungkinkan penyerang tanpa autentikasi mengeksekusi kode dari jarak jauh dan mengakses seluruh file system.

### 2.3 Supply Chain Attacks

Setiap aplikasi bergantung pada ribuan **library**. Jika penyerang membobol satu aplikasi atau library dan mendorong update berbahaya ke seluruh penggunanya, semua pengguna itu ikut terkompromi.

Contoh nyata: **SolarWinds** dan **3CX**, keduanya berdampak pada ribuan perusahaan. TryHackMe sendiri pernah menjadi korban lewat **Lottie Player**, library animasi yang mereka gunakan.

Ciri khas serangan supply chain: software yang selama ini tepercaya, tiba-tiba berperilaku mencurigakan segera setelah menerima update.

## 3. Kerentanan vs Misconfiguration

Dua sumber celah keamanan sistem memiliki sifat yang berbeda secara mendasar.

**Vulnerability (Kerentanan)** — bug pada software itu sendiri. Setelah dipublikasikan diberi nomor CVE. Solusi permanennya selalu **patch** dari vendor.

**Misconfiguration (Kesalahan Konfigurasi)** — bukan bug, melainkan kesalahan dalam cara sistem disetel, umumnya oleh tim IT. Sering terjadi karena ingin menyederhanakan proses, misalnya memakai password sederhana seperti "1111".

### 3.1 Contoh Kasus Nyata Misconfiguration

- Password "123456" membocorkan data chat 64 juta lamaran kerja McDonald's.
- Cloud AWS yang salah dikonfigurasi menyebabkan kebocoran data 106 juta nasabah bank.
- Smart fridge yang salah dikonfigurasi digunakan diam-diam dalam serangan botnet.

### 3.2 Perbedaan Kunci dalam Penanganan

Vulnerability membutuhkan patch (update software). Misconfiguration cukup dibenahi lewat setup yang lebih baik, tanpa perlu update software sama sekali.

## 4. Strategi Respons dan Mitigasi

### 4.1 Merespons Vulnerability Sebelum Patch Tersedia

Selama menunggu patch (termasuk kasus zero-day), langkah sementara yang bisa diambil:

- Membatasi akses sistem hanya untuk IP tepercaya.
- Menerapkan mitigasi sementara yang disediakan vendor.
- Memblokir pola serangan yang sudah dikenal di **IPS (Intrusion Prevention System)** atau **WAF (Web Application Firewall)**.

### 4.2 Merespons Misconfiguration Secara Proaktif

Analis SOC sering baru menyadari misconfiguration setelah dieksploitasi. Di perusahaan kecil, analis juga bisa berperan proaktif melalui:

- **Penetration Testing** — menyewa ethical hacker untuk mensimulasikan serangan dan melaporkan celah yang ditemukan.
- **Vulnerability Scans** — menjalankan tool secara berkala untuk mendeteksi password default atau software usang.
- **Configuration Audits** — meninjau manual kesesuaian sistem terhadap best practice seperti **CIS benchmarks**.

### 4.3 Mitigasi Umum untuk Melindungi Sistem

**Patch Management** — proses melacak dan menambal sistem rentan secara sistematis, signifikan menurunkan peluang serangan berhasil.

**Training for IT** — tim IT yang paham risiko misconfiguration lebih kecil kemungkinan meninggalkan sistem tanpa perlindungan.

**Network Protection** — membatasi akses sistem hanya untuk orang atau IP tepercaya membuat sistem jauh lebih sulit dibobol.

**Antivirus Protection** — menghentikan atau mendeteksi berbagai jenis serangan pada sistem, sama seperti fungsinya pada manusia.

Alur pertahanan berlapis: serangan yang berhasil dihalangi oleh patched software dan antivirus solution akan menyisakan sedikit ancaman yang kemudian ditangani langsung oleh tim SOC.

## 5. Mengapa Ini Penting bagi SOC Analyst

Analis SOC umumnya tidak mengelola sistem secara langsung. Namun pemahaman terhadap pola serangan dan pertahanan sistem tetap krusial karena dua alasan:

1. Memperluas perspektif keamanan siber analis secara menyeluruh, tidak hanya berfokus pada sisi human-attack vector.
2. Memungkinkan analis membagikan pengetahuan tersebut kepada departemen IT sebagai bentuk kolaborasi lintas tim.

Praktik yang disarankan: selalu mengikuti perkembangan ancaman terbaru dan membagikannya kepada rekan kerja.

## 6. Glosarium Istilah Kunci

**System** — tempat penyimpanan/pemrosesan data digital: server fisik, mesin lab, atau platform cloud.

**Threat Actor** — pelaku yang melancarkan serangan siber.

**Zero-day** — kerentanan yang ditemukan penyerang sebelum diketahui pihak defender/vendor, belum ada patch tersedia.

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

**Configuration Audit** — peninjauan manual kesesuaian konfigurasi sistem terhadap standar keamanan.

**CIS Benchmarks** — kumpulan standar/best practice konfigurasi keamanan sistem yang diakui secara luas.

**Patch Management** — proses sistematis melacak dan menerapkan patch pada sistem yang rentan.

**ToolShell** — julukan untuk kerentanan RCE kritis CVE-2025-53770 pada SharePoint on-premise.

## 7. Tools & Platform Rujukan

**haveibeenpwned.com** — mengecek apakah sebuah password atau akun pernah muncul dalam kebocoran data yang tercatat.

**CISA Known Exploited Vulnerabilities (KEV) Catalog** — katalog resmi kerentanan yang sudah diketahui dieksploitasi secara aktif di dunia nyata.

**CIS Benchmarks** — kumpulan standar konfigurasi keamanan sistem yang diakui secara luas, dipakai sebagai acuan saat configuration audit.

**The DFIR Report** — sumber dokumentasi bagaimana intrusi nyata terjadi di lapangan.

**BleepingComputer** — sumber berita seputar serangan supply chain terbaru.

**CheckPoint Interactive Live Cyber Threat Map** — peta ancaman siber interaktif secara live.

## 8. Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar

- System — tempat simpan/proses data: server fisik, lab, cloud
- Nilai sistem = skala dampak jika dibobol, bukan jenis sistemnya
- Human hacking & system hacking = setara, tidak dibedakan penyerang

### Tiga Jalur Serangan

- Human-led — USB nyasar, malware bajakan, reuse password (81% breach = password bocor)
- RubberDucky — USB otomatis jalankan malware saat dicolok
- Vulnerability — bug di software, contoh Shellshock (1992 ada, 2014 ketemu)
- Zero-day — kerentanan ketemu penyerang duluan, belum ada patch
- CVE — nomor resmi kerentanan setelah dipublikasi
- Supply chain — 1 app/library dibobol, update jahat nyebar ke semua user
- Contoh: SolarWinds, 3CX, Lottie Player (THM)

### CVE Penting

- CVE-2017-0144 — EternalBlue, SMB
- CVE-2019-0708
- CVE-2020-1472
- CVE-2021-34527 — PrintNightmare, Print Spooler
- CVE-2022-30190 — Follina, MS Office
- CVE-2023-24880 — zero-day bypass
- CVE-2025-53770 — ToolShell, RCE SharePoint

### Vulnerability vs Misconfiguration

- Vulnerability = bug software → butuh patch
- Misconfiguration = salah setup (bukan bug) → benahi konfigurasi, tanpa update
- Contoh misconfig: McDonald's (123456), AWS (106jt data bocor), smart fridge (botnet)

### Respons Vulnerability (tunggu patch)

- Batasi akses ke IP tepercaya
- Mitigasi sementara dari vendor
- Blokir pola serangan via IPS/WAF

### Respons Misconfiguration (proaktif)

- Penetration Testing — sewa ethical hacker simulasi serangan
- Vulnerability Scan — scan otomatis berkala
- Configuration Audit — review manual vs CIS benchmarks

### Mitigasi Umum

- Patch Management — lacak & tambal sistem rentan
- Training for IT — edukasi tim IT soal misconfig
- Network Protection — batasi akses ke IP/orang tepercaya
- Antivirus Protection — cegah/deteksi serangan

### Tools Rujukan

- haveibeenpwned.com — cek password/akun bocor
- CISA KEV Catalog — daftar CVE yang aktif dieksploitasi
- CIS Benchmarks — standar konfigurasi aman
- The DFIR Report — dokumentasi intrusi nyata
- BleepingComputer — berita supply chain attack
- CheckPoint Threat Map — peta ancaman live
