# Searching Skills — Resume Materi
**TryHackMe | 21 Oktober 2026**

---

## 1. Evaluasi Informasi

Kemampuan menilai kualitas informasi adalah fondasi dari riset yang baik. Internet memungkinkan siapa saja mempublikasikan tulisan — blog, artikel, wiki — tanpa harus punya keahlian di bidang yang dibahas. Tugas pembaca adalah menyaring mana yang kredibel.

Ada empat filter utama yang dipakai saat mengevaluasi informasi:

**Source** — Identifikasi siapa penulis atau organisasinya. Apakah mereka punya reputasi dan otoritas di bidang tersebut? Menulis sebuah blog post tidak otomatis menjadikan seseorang ahli.

**Evidence and reasoning** — Periksa apakah klaim yang dibuat didukung oleh bukti konkret dan penalaran yang logis. Hindari sumber yang hanya membuat pernyataan tanpa dasar yang bisa diverifikasi.

**Objectivity and bias** — Evaluasi apakah informasi disajikan secara objektif dari berbagai perspektif. Waspadai penulis yang mendorong agenda tersembunyi, baik untuk mempromosikan produk maupun menyerang kompetitor.

**Corroboration and consistency** — Validasi informasi dengan mengecek beberapa sumber independen. Klaim yang solid biasanya dikonfirmasi oleh banyak sumber terpercaya yang berbeda.

---

## 2. Search Engine & Operator Pencarian

### 2.1 Search Engine Umum

Tiga search engine utama yang mendukung advanced search:

- Google
- Bing
- DuckDuckGo

### 2.2 Google Search Operators

Operator ini mempersempit hasil pencarian agar lebih presisi dan relevan.

| Operator | Fungsi | Contoh |
|---|---|---|
| `"exact phrase"` | Cari frasa persis kata per kata | `"passive reconnaissance"` |
| `site:` | Batasi hasil ke domain tertentu | `site:tryhackme.com success stories` |
| `-` | Hilangkan kata/frasa dari hasil | `pyramids -tourism` |
| `filetype:` | Cari tipe file tertentu (pdf, doc, xls, ppt) | `filetype:ppt cyber security` |

Operator ini bisa dikombinasikan. Misalnya, `filetype:pdf cyber warfare report` akan mengembalikan hanya file PDF yang mengandung ketiga kata tersebut.

---

## 3. Specialized Search Engines

Search engine umum seperti Google didesain untuk halaman web. Untuk kebutuhan teknis yang lebih spesifik, ada search engine khusus yang jauh lebih efektif.

### 3.1 Shodan

Shodan adalah search engine untuk **perangkat yang terhubung ke internet**. Bukan mencari halaman web, tapi mencari perangkat — server, router, webcam, industrial control system, dan IoT devices.

Shodan mengembalikan informasi seperti versi server, lokasi geografis, dan header respons. Contoh query `apache 2.4.1` akan menampilkan daftar semua server yang masih menjalankan Apache versi tersebut beserta distribusinya per negara.

Referensi tambahan: Search Query Examples dan Shodan Trends (butuh subscription) tersedia di situsnya.

### 3.2 Censys

Censys terlihat mirip Shodan, tapi fokusnya berbeda. Shodan berfokus pada perangkat dan sistem (server, router, webcam), sedangkan Censys berfokus pada **internet-connected hosts, websites, sertifikat SSL, dan aset internet lainnya**.

Kasus penggunaan utama Censys:
- Enumerasi domain yang sedang aktif digunakan
- Audit port terbuka dan layanan yang berjalan
- Menemukan rogue asset dalam sebuah jaringan

### 3.3 VirusTotal

VirusTotal adalah layanan online untuk **memindai file atau URL menggunakan puluhan antivirus engine sekaligus**. Satu file bisa dicek terhadap 60+ engine dalam satu operasi.

Input yang diterima:
- Upload file langsung
- URL yang ingin dicek
- Hash file (untuk mengecek hasil scan file yang pernah diunggah sebelumnya)

Perlu diperhatikan: hasil scan tidak selalu 100% akurat. Sebuah file bisa saja di-flag sebagai virus oleh beberapa engine padahal sebenarnya bukan — ini disebut false positive. Bagian komentar komunitas di VirusTotal membantu memberikan konteks tambahan.

### 3.4 Have I Been Pwned (HIBP)

HIBP melakukan satu hal: **memberi tahu apakah sebuah alamat email pernah muncul dalam data breach**.

Relevansinya: banyak orang menggunakan password yang sama di berbagai platform. Jika satu platform bocor, akun di platform lain dengan password yang sama juga berisiko. Password biasanya disimpan dalam format terenkripsi, tapi yang tidak kompleks bisa dipulihkan menggunakan berbagai teknik serangan.

---

## 4. Vulnerabilities & Exploit Resources

### 4.1 CVE (Common Vulnerabilities and Exposures)

CVE adalah **sistem penomoran standar untuk kerentanan keamanan** pada perangkat lunak dan hardware. Setiap kerentanan diberi ID unik dengan format:

```
CVE-TAHUN-NOMOR
Contoh: CVE-2024-29988
```

ID ini memastikan semua pihak — peneliti, vendor, tim IT — merujuk pada kerentanan yang persis sama tanpa ambiguitas.

CVE dikelola oleh **MITRE Corporation**. Untuk mencari CVE yang sudah ada, tersedia dua referensi utama: CVE Program website dan National Vulnerability Database (NVD).

### 4.2 Exploit Database

Exploit Database adalah **koleksi kode eksploit dari berbagai penulis**. Beberapa kode sudah diuji dan ditandai sebagai verified.

Aturan penting: jangan mengeksploitasi sistem tanpa izin resmi, biasanya berupa perjanjian yang mengikat secara hukum. Exploit Database hanya digunakan setelah ada izin tersebut.

### 4.3 GitHub

GitHub sebagai platform pengembangan software juga menjadi sumber **proof-of-concept (PoC) dan exploit code** yang terkait dengan CVE tertentu. Mencari nomor CVE di GitHub sering menghasilkan repository berisi tools pengujian, checker, dan implementasi eksploit yang dibuat komunitas.

---

## 5. Dokumentasi Teknis

Membaca dokumentasi resmi adalah skill yang membedakan pemula dari profesional. Dokumentasi resmi selalu paling up-to-date dan paling lengkap.

### 5.1 Linux Manual Pages (Man Pages)

Setiap perintah di Linux dan sistem berbasis Unix memiliki man page bawaan. Man page juga tersedia untuk system calls, library functions, dan file konfigurasi.

```bash
man <perintah>    # Buka manual page untuk perintah tertentu
man ip            # Contoh: manual page untuk perintah ip
man cat           # Manual page untuk perintah cat
```

Navigasi dalam man page:
- Tekan `h` untuk bantuan
- Tekan `q` untuk keluar

Jika lebih suka membaca di browser, cukup ketik `man ip` di Google — halaman manualnya biasanya muncul di hasil teratas.

### 5.2 Microsoft Windows Documentation

Microsoft menyediakan halaman **Technical Documentation** resmi di Microsoft Learn untuk semua produk dan perintahnya. Contohnya, mencari `ipconfig` di sana akan menampilkan referensi lengkap tentang perintah tersebut.

### 5.3 Product Documentation

Setiap produk populer diharapkan memiliki dokumentasi resmi yang terorganisir. Contoh yang relevan untuk keamanan siber: Snort Official Documentation, Apache HTTP Server Documentation, PHP Documentation, dan Node.js Documentation.

---

## 6. Perintah Jaringan

### 6.1 Perbandingan netstat dan ss

| Aspek | Linux | Windows |
|---|---|---|
| Perintah lama | `netstat` (sudah digantikan) | `netstat` (masih aktif) |
| Perintah baru | `ss` (socket statistics) | tidak ada pengganti |

### 6.2 Flag Penting

```
ss -p              # Tampilkan proses (nama program & PID) yang terkait tiap socket — Linux
netstat -b         # Tampilkan nama executable tiap koneksi aktif & listening port — Windows
```

`ss` adalah singkatan dari **socket statistics** dan menjadi standar di distribusi Linux modern sebagai pengganti `netstat`.

---

## 7. Social Media untuk OSINT

Media sosial bukan hanya platform komunikasi — bagi profesional keamanan siber, ini adalah sumber informasi yang kaya untuk riset.

### 7.1 Platform dan Kegunaannya

**LinkedIn** — Sumber informasi latar belakang **profesional dan teknis** seseorang. Berisi riwayat pekerjaan, skill, sertifikasi, tools yang dipakai, dan proyek yang pernah dikerjakan. Platform utama untuk mengetahui background teknis karyawan sebuah perusahaan.

**Facebook** — Sumber informasi **personal dan kehidupan pribadi**. Berisi nama sekolah, kampung halaman, anggota keluarga, dan momen masa kecil. Informasi ini berbahaya karena sering menjadi jawaban dari secret questions akun (misalnya: "Sekolah mana yang kamu masuki saat kecil?") yang bisa dipakai untuk reset password.

**Twitter/X** — Sumber update real-time, diskusi tren teknis, dan komunitas keamanan siber yang aktif berbagi informasi.

### 7.2 Tips Eksplorasi Platform Baru

Saat perlu menjelajahi platform yang belum dikenal, gunakan **email sementara** daripada email asli. Alasannya:
- Menghindari kontak asli terhubung denganmu saat kamu hanya menjelajah sementara
- Akun dan email sementara bisa dihapus setelah selesai

### 7.3 Implikasi untuk Keamanan

Saat melindungi sebuah perusahaan, pastikan karyawan tidak oversharing di media sosial. Informasi yang tampak tidak berbahaya (nama sekolah, tanggal lahir, nama hewan peliharaan) bisa dimanfaatkan adversary untuk menjawab secret questions dan mengambil alih akun.

### 7.4 News Outlets

Selain media sosial, ratusan website berita menyediakan informasi keamanan siber terkini. Coba beberapa outlet berbeda dan tetapkan yang paling relevan dengan kebutuhanmu.

---

## 8. Glossary

**Snake Oil** — Metode atau produk kriptografi yang palsu atau merupakan penipuan.

**CVE** — Common Vulnerabilities and Exposures; sistem penomoran standar untuk kerentanan keamanan perangkat lunak dan hardware.

**PoC (Proof of Concept)** — Kode atau demonstrasi yang membuktikan bahwa sebuah eksploit benar-benar bisa dijalankan.

**Hash** — Sidik jari unik sebuah file yang dihasilkan dari fungsi kriptografi; dipakai di VirusTotal untuk mengidentifikasi file tanpa perlu menguploadnya ulang.

**Data Breach** — Insiden di mana data sensitif terekspos secara tidak sengaja ke pihak yang tidak berwenang.

**Rogue Asset** — Perangkat atau sistem dalam sebuah jaringan yang tidak diotorisasi atau tidak diketahui keberadaannya oleh tim keamanan.

**IoT (Internet of Things)** — Perangkat fisik sehari-hari yang terhubung ke internet dan bisa dikomunikasikan secara online.

**OSINT (Open Source Intelligence)** — Pengumpulan informasi dari sumber-sumber yang tersedia secara publik.

**Oversharing** — Perilaku berbagi informasi berlebihan di media sosial yang berpotensi dieksploitasi oleh pihak tidak bertanggung jawab.

**False Positive** — Kondisi di mana sebuah file atau aktivitas yang sebenarnya aman di-flag sebagai berbahaya oleh sistem deteksi.

**NVD (National Vulnerability Database)** — Database kerentanan resmi yang dikelola pemerintah Amerika Serikat; referensi CVE yang komprehensif.

**HIBP (Have I Been Pwned)** — Layanan online untuk mengecek apakah alamat email pernah muncul dalam data breach.

**Socket Statistics (ss)** — Perintah Linux modern pengganti netstat untuk menampilkan informasi koneksi jaringan dan socket yang aktif.

---

## 9. Tools & Platform Rujukan

**Shodan** — Search engine untuk perangkat yang terhubung internet (server, router, IoT, webcam).
URL: https://www.shodan.io

**Censys** — Search engine untuk host, sertifikat SSL, dan aset internet; fokus pada enumerasi dan audit.
URL: https://censys.io

**VirusTotal** — Layanan scan file/URL menggunakan 60+ antivirus engine sekaligus.
URL: https://www.virustotal.com

**Have I Been Pwned** — Cek apakah email pernah bocor dalam data breach.
URL: https://haveibeenpwned.com

**CVE Program** — Database resmi CVE yang dikelola MITRE.
URL: https://www.cve.org

**National Vulnerability Database (NVD)** — Database kerentanan resmi pemerintah AS.
URL: https://nvd.nist.gov

**Exploit Database** — Koleksi kode eksploit dari berbagai penulis; beberapa sudah diverifikasi.
URL: https://www.exploit-db.com

**GitHub** — Platform development yang juga berisi PoC dan exploit code terkait CVE.
URL: https://github.com

**Google** — Search engine umum dengan dukungan advanced operator pencarian.
URL: https://www.google.com

**Bing** — Search engine umum dengan dukungan advanced operator.
URL: https://www.bing.com

**DuckDuckGo** — Search engine umum dengan fokus privasi; mendukung operator pencarian.
URL: https://duckduckgo.com

**Microsoft Learn (Technical Documentation)** — Dokumentasi resmi semua produk dan perintah Microsoft Windows.
URL: https://learn.microsoft.com

---

## 10. Catatan Ringkas untuk Ditulis Tangan

### Evaluasi Informasi

- Source — cek otoritas penulis/organisasinya
- Evidence & reasoning — ada bukti & logika solid?
- Objectivity & bias — objektif atau ada agenda tersembunyi?
- Corroboration — dikonfirmasi beberapa sumber independen?

### Google Search Operators

- `"kata"` — cari frasa persis
- `site:domain.com` — batasi ke domain tertentu
- `-kata` — hilangkan kata dari hasil
- `filetype:pdf` — cari tipe file spesifik
- Contoh gabungan: `filetype:pdf cyber warfare report`

### Specialized Search Engines

- Shodan — cari perangkat terhubung internet (server, IoT, router, webcam)
- Censys — cari host, website, sertifikat SSL, aset internet
- VirusTotal — scan file/URL dengan 60+ antivirus engine; bisa input file/URL/hash
- HIBP — cek email apakah pernah bocor di data breach

### CVE & Exploit

- CVE — kamus standar kerentanan; format: CVE-TAHUN-NOMOR
- Dikelola MITRE, referensi lain: NVD
- Exploit DB — koleksi kode eksploit; butuh izin resmi sebelum pakai
- GitHub — sumber PoC dan exploit code komunitas

### Dokumentasi Teknis

- `man <perintah>` — buka manual page di Linux
- `q` — keluar dari man page
- Windows: cari di Microsoft Learn
- Selalu utamakan dokumentasi resmi

### Perintah Jaringan

- `ss` — pengganti netstat di Linux (socket statistics)
- `ss -p` — tampilkan proses terkait tiap socket (Linux)
- `netstat -b` — tampilkan executable tiap koneksi aktif (Windows)
- netstat di Windows masih aktif, tidak digantikan

### Social Media & OSINT

- LinkedIn — latar belakang profesional & teknis karyawan
- Facebook — info personal: sekolah, keluarga, momen masa kecil (bahaya: jawaban secret question)
- Twitter — tren keamanan siber real-time
- Gunakan email sementara saat eksplorasi platform baru
- Oversharing = risiko keamanan nyata

### Istilah Kunci

- Snake Oil — produk/metode kriptografi palsu atau penipuan
- PoC — kode bukti eksploit bisa dijalankan
- Hash — sidik jari unik file
- Data Breach — kebocoran data ke pihak tidak berwenang
- Rogue Asset — perangkat tidak diotorisasi dalam jaringan
- False Positive — file aman di-flag sebagai berbahaya
- OSINT — pengumpulan info dari sumber publik
- Oversharing — berbagi info berlebihan di medsos; risiko keamanan
