# Resume Materi: SIEM Tools dan Tool Keamanan Siber (Open-Source & Proprietary)

Tanggal: 11 Oktober 2026

Room: Tool SIEM, Dashboard Splunk dan Chronicle, serta Tool Open-Source dan Proprietary

## 1. Gambaran Umum

Room ini membahas **SIEM (Security Information and Event Management)**, yaitu tool yang membantu analis keamanan memantau sistem dan mendeteksi ancaman keamanan. Pembahasan mencakup tiga hal utama: jenis-jenis SIEM, tool SIEM yang umum dipakai beserta dashboard-nya, dan perbedaan tool open-source dengan proprietary. Room ditutup dengan pembahasan mitos seputar karier di keamanan siber.

Room ini bersifat teori murni dan tidak memuat command atau switch yang harus dijalankan. Command-line Linux dan Suricata hanya disebut sekilas dan akan dibahas mendalam di bagian lain program.

## 2. Jenis-Jenis SIEM

Organisasi memilih jenis SIEM sesuai kebutuhan keamanannya. Terdapat tiga jenis utama.

### 2.1 Self-hosted SIEM

**Self-hosted SIEM** diinstal, dioperasikan, dan dipelihara oleh organisasi sendiri menggunakan infrastruktur fisiknya, misalnya kapasitas server. Aplikasi dikelola oleh departemen IT organisasi, bukan vendor pihak ketiga.

Jenis ini ideal ketika organisasi wajib mempertahankan kendali fisik atas data rahasia (confidential data).

### 2.2 Cloud-hosted SIEM

**Cloud-hosted SIEM** dipelihara dan dikelola oleh penyedia SIEM serta diakses melalui internet.

Jenis ini ideal bagi organisasi yang tidak ingin berinvestasi membangun dan memelihara infrastruktur sendiri.

### 2.3 Hybrid SIEM

**Hybrid SIEM** menggabungkan tool self-hosted dan cloud-hosted. Organisasi memilihnya untuk memperoleh keunggulan cloud sambil tetap menjaga kendali fisik atas data rahasia.

### 2.4 Cloud-native SIEM

Tool **cloud-native** juga sepenuhnya dikelola vendor seperti cloud-hosted. Bedanya, tool ini dirancang khusus untuk memanfaatkan kemampuan cloud computing, yaitu **availability**, **flexibility**, dan **scalability**. Contohnya adalah Chronicle.

### 2.5 Perbandingan Singkat

| Jenis | Dikelola oleh | Cocok untuk |
|---|---|---|
| Self-hosted | IT organisasi | Kendali fisik atas data rahasia |
| Cloud-hosted | Penyedia SIEM | Organisasi tanpa infrastruktur sendiri |
| Hybrid | Organisasi dan penyedia | Manfaat cloud sekaligus kendali fisik |

## 3. Tool SIEM yang Umum Digunakan

### 3.1 Splunk

**Splunk** adalah platform analisis data, dan menyediakan dua pilihan tool SIEM. Keduanya memungkinkan peninjauan data organisasi pada dashboard, dengan cara mengumpulkan, mencari, memantau, dan menganalisis log data dari berbagai sumber agar organisasi memperoleh visibilitas penuh atas operasi hariannya.

- **Splunk Enterprise**: tool self-hosted untuk menyimpan (retain), menganalisis, dan mencari log data organisasi sehingga menghasilkan informasi keamanan dan alert secara real-time.
- **Splunk Cloud**: tool cloud-hosted untuk mengumpulkan, mencari, dan memantau log data. Berguna bagi organisasi dengan lingkungan hybrid atau cloud-only, yaitu sebagian atau seluruh layanannya berada di cloud.

### 3.2 Chronicle

**Chronicle** (kini disebut **Google SecOps**) adalah tool SIEM cloud-native milik Google yang menyimpan, menganalisis, dan mencari log data untuk mengidentifikasi potensi ancaman, risiko, dan kerentanan. Fiturnya mencakup log monitoring, analisis data, dan data collection.

Chronicle memungkinkan pengumpulan dan analisis log data berdasarkan empat objek:

- Aset tertentu
- Nama domain
- Pengguna
- Alamat IP

## 4. Dashboard Splunk

- **Security posture dashboard**: dirancang untuk **security operations center (SOC)**. Menampilkan peristiwa dan tren keamanan menonjol dalam 24 jam terakhir untuk menilai apakah infrastruktur dan kebijakan keamanan berjalan sesuai rancangan. Contoh penggunaan: menyelidiki aktivitas jaringan mencurigakan dari satu alamat IP secara real time.
- **Executive summary dashboard**: memantau kesehatan organisasi secara keseluruhan dari waktu ke waktu untuk meningkatkan langkah keamanan yang menurunkan risiko. Contoh penggunaan: ringkasan insiden dan tren untuk stakeholder.
- **Incident review dashboard**: membantu mengidentifikasi pola mencurigakan saat insiden dan menyoroti item berisiko tinggi yang perlu segera ditinjau. Menyediakan linimasa visual peristiwa yang mengarah ke insiden.
- **Risk analysis dashboard**: mengidentifikasi risiko tiap **risk object** (pengguna, komputer, atau alamat IP) dan menunjukkan perubahan perilaku, seperti login di luar jam kerja atau lalu lintas jaringan tinggi dari satu komputer. Dipakai untuk menganalisis dampak kerentanan pada aset kritis dan memprioritaskan mitigasi.

## 5. Dashboard Chronicle

- **Enterprise insights dashboard**: menyoroti alert terbaru dan mengidentifikasi nama domain mencurigakan dalam log, yang disebut **indicators of compromise (IOC)**. Setiap hasil diberi **confidence score** (kemungkinan adanya ancaman) dan tingkat **severity** (signifikansi ancaman bagi organisasi). Contoh penggunaan: memantau percobaan login atau akses data ke aset kritis dari lokasi atau perangkat tidak biasa.
- **Data ingestion and health dashboard**: menampilkan jumlah event log, sumber log, dan tingkat keberhasilan data yang diproses ke Chronicle. Dipakai untuk memastikan sumber log terkonfigurasi benar dan log diterima tanpa error.
- **IOC matches dashboard**: menunjukkan ancaman, risiko, dan kerentanan teratas. Mengamati IOC domain, IP, dan perangkat dari waktu ke waktu untuk melihat tren dan menentukan ancaman berprioritas tertinggi. Contoh penggunaan: mencari aktivitas tambahan terkait alert, seperti login mencurigakan dari lokasi geografis tidak biasa.
- **Main dashboard**: ringkasan tingkat tinggi tentang data ingestion, alerting, dan aktivitas event dari waktu ke waktu. Dipakai untuk melihat linimasa peristiwa, seperti lonjakan login gagal, lintas sumber log, perangkat, IP, dan lokasi fisik.
- **Rule detections dashboard**: statistik insiden dengan kejadian, severity, dan deteksi tertinggi. Menampilkan semua alert yang dipicu satu **detection rule**, misalnya aturan untuk pengguna yang membuka lampiran email berbahaya. Statistiknya membantu mengelola insiden berulang dan menyusun taktik mitigasi.
- **User sign in overview dashboard**: informasi perilaku akses pengguna di seluruh organisasi. Dipakai untuk mendeteksi aktivitas tidak biasa, seperti sign-in dari beberapa lokasi secara bersamaan, demi melindungi akun pengguna dan aplikasi.

### 5.1 Tujuan Dashboard SIEM

Dashboard membantu profesional keamanan mengorganisasi dan memfokuskan upaya mereka. Dengan begitu, analis dapat mengurangi risiko melalui identifikasi, analisis, dan remediasi item berprioritas tertinggi secara tepat waktu.

## 6. Tool Open-Source dan Proprietary

### 6.1 Open-source

Tool **open-source** sering gratis dan user friendly. Tujuannya menyediakan software yang dibangun publik secara kolaboratif sehingga bisa lebih aman, serta memungkinkan kustomisasi yang menghasilkan beragam layanan baru dari paket yang sama.

Source code dan materi pelatihannya tersedia bagi pengguna, sehingga dapat dimodifikasi dan ditingkatkan, selama lisensi yang ditetapkan dihormati.

### 6.2 Proprietary

Tool **proprietary** dikembangkan dan dimiliki seseorang atau perusahaan. Pengguna biasanya membayar biaya penggunaan dan pelatihan.

Hanya pemilik yang dapat mengakses dan memodifikasi source code, sehingga pengguna harus menunggu pembaruan dan kadang membayar biayanya. Pengguna hanya dapat memodifikasi sejumlah fitur terbatas. Contohnya Splunk dan Google SecOps (Chronicle).

### 6.3 Miskonsepsi Umum

Anggapan bahwa tool open-source kurang efektif dan kurang aman dibanding proprietary adalah keliru. Banyak materi open-source telah menjadi standar industri.

Threat actor memang pernah mencoba memanipulasi tool open-source, tetapi paparan luas dan akses langsung ke source code membuat pengguna berniat baik dapat memperbaiki masalah segera setelah ditemukan. Hal ini justru mempersulit pihak berniat jahat.

### 6.4 Contoh Tool Open-source

- **Linux**: sistem operasi open-source yang banyak dipakai dan dapat disesuaikan melalui antarmuka command-line. **Sistem operasi** adalah antarmuka antara hardware komputer dan pengguna, dipakai untuk berkomunikasi dengan hardware dan mengelola aplikasi software. Terdapat banyak versi Linux untuk tugas tertentu.
- **Suricata**: software analisis jaringan dan deteksi ancaman open-source. Memeriksa lalu lintas jaringan untuk mengidentifikasi perilaku mencurigakan dan menghasilkan log data jaringan, dengan menemukan aktivitas lintas pengguna, komputer, atau alamat IP. Dikembangkan oleh **Open Information Security Foundation (OISF)**, yang menjaga agar proyek ini tetap gratis dan publik. Dipakai luas di sektor publik dan swasta, serta terintegrasi dengan banyak tool SIEM dan tool keamanan lain.

## 7. Mitos Karier di Keamanan Siber

Berdasarkan pengalaman Talya, engineer privacy, safety, and security di Google (sisi **blue team**):

- **Harus bisa coding, hacking, atau jago matematika**: tidak benar. Banyak peran, termasuk blue team (sisi defense), lebih mengandalkan keterampilan lain. Red team berada di sisi ofensif.
- **Wajib bergelar keamanan siber**: tidak benar. Latar belakang bisnis pun valid, dan gelar lanjutan tidak diwajibkan.
- **Bekerja terisolasi**: tidak benar, tergantung jalur yang dipilih, tetapi pada umumnya kerja sama sangat penting.
- **Keterampilan berharga**: membangun relasi, belajar cepat saat bekerja, riset, dan mengajukan pertanyaan yang tepat.
- **Saran**: ciptakan jalur karier sendiri, karena setiap orang berbeda, dan cari orang yang bisa mendukung perjalanan, termasuk saat menempuh sertifikat.

## 8. Glossary

- **SIEM**: tool yang membantu analis memantau sistem dan mendeteksi ancaman keamanan.
- **Self-hosted**: SIEM yang dijalankan dan dipelihara organisasi pada infrastruktur fisiknya sendiri.
- **Cloud-hosted**: SIEM yang dikelola penyedia dan diakses lewat internet.
- **Hybrid**: gabungan self-hosted dan cloud-hosted.
- **Cloud-native**: tool yang dirancang khusus untuk memanfaatkan availability, flexibility, dan scalability cloud.
- **Log data**: catatan aktivitas sistem yang dikumpulkan dan dianalisis SIEM.
- **SOC (security operations center)**: tim atau pusat operasi keamanan; pengguna utama security posture dashboard.
- **Risk object**: entitas yang dinilai risikonya, yaitu pengguna, komputer, atau alamat IP.
- **IOC (indicators of compromise)**: indikator adanya potensi kompromi, misalnya domain mencurigakan di log.
- **Confidence score**: label kemungkinan suatu temuan benar-benar ancaman.
- **Severity**: tingkat signifikansi ancaman bagi organisasi.
- **Detection rule**: aturan yang memicu alert ketika kondisi tertentu terpenuhi.
- **Open-source**: software dengan source code terbuka, dibangun kolaboratif, sering gratis.
- **Proprietary**: software milik pihak tertentu, source code tertutup, biasanya berbayar.
- **Sistem operasi**: antarmuka antara hardware komputer dan pengguna.
- **OISF**: Open Information Security Foundation, pengembang Suricata.
- **Blue team**: sisi defense dalam keamanan; **red team**: sisi ofensif.

## 9. Tools & Platform Rujukan

URL tidak disebutkan secara eksplisit di materi, sehingga hanya nama yang dicantumkan.

- **Splunk Enterprise**: SIEM self-hosted untuk retain, analisis, dan pencarian log data.
- **Splunk Cloud**: SIEM cloud-hosted untuk mengumpulkan, mencari, dan memantau log data.
- **Chronicle (Google SecOps)**: SIEM cloud-native milik Google.
- **Suricata**: software open-source analisis jaringan dan deteksi ancaman.
- **Linux**: sistem operasi open-source dengan antarmuka command-line.

## 10. Catatan Ringkas untuk Ditulis Tangan

### Jenis SIEM

- Self-hosted — diinstal dan dipelihara sendiri, infrastruktur fisik sendiri, kendali fisik atas data rahasia
- Cloud-hosted — dikelola penyedia, akses internet, tanpa infrastruktur sendiri
- Hybrid — self-hosted + cloud-hosted, manfaat cloud + kendali fisik
- Cloud-native — dirancang untuk cloud, availability, flexibility, scalability

### Tool SIEM

- Splunk — platform analisis data
- Splunk Enterprise — self-hosted, retain, analisis, cari log, alert real-time
- Splunk Cloud — cloud-hosted, kumpul, cari, pantau log, cocok hybrid atau cloud-only
- Chronicle (Google SecOps) — cloud-native, log monitoring, analisis, data collection
- Chronicle analisis berdasarkan — aset, domain, pengguna, IP

### Dashboard Splunk

- Security posture — untuk SOC, 24 jam terakhir, cek kebijakan berjalan sesuai rancangan
- Executive summary — kesehatan organisasi dari waktu ke waktu, ringkasan untuk stakeholder
- Incident review — pola mencurigakan, item berisiko tinggi, linimasa menuju insiden
- Risk analysis — risiko per risk object, login luar jam kerja, prioritas mitigasi

### Dashboard Chronicle

- Enterprise insights — alert terbaru, domain IOC, confidence score, severity
- Data ingestion and health — jumlah log, sumber log, tingkat sukses, cek error
- IOC matches — ancaman teratas, tren IOC, prioritas tertinggi
- Main — ringkasan ingestion, alerting, event, linimasa lonjakan login gagal
- Rule detections — alert per detection rule, insiden berulang, mitigasi
- User sign in overview — daftar sign-in, sign-in multi-lokasi bersamaan

### Open-source vs Proprietary

- Open-source — gratis, kolaboratif, source code terbuka, bisa dikustomisasi, patuhi lisensi
- Proprietary — dimiliki pihak tertentu, berbayar, source code tertutup, update tunggu pemilik, modifikasi terbatas
- Miskonsepsi — open-source dianggap kurang aman, padahal banyak jadi standar industri dan bug cepat diperbaiki
- Linux — OS open-source, antarmuka command-line, banyak versi
- Sistem operasi — antarmuka hardware dan pengguna
- Suricata — open-source, analisis jaringan, deteksi ancaman, buat log jaringan
- OISF — pengembang Suricata, jaga tetap gratis dan publik

### Mitos Karier

- Tidak wajib coding, hacking, atau math wiz — contoh blue team
- Tidak wajib gelar keamanan siber — latar bisnis valid
- Kerja terisolasi — mitos
- Skill kunci — relasi, belajar cepat, riset, bertanya tepat
- Saran — buat jalur sendiri, cari pendukung

### Tujuan SIEM Dashboard

- Organisir dan fokuskan upaya keamanan
- Kurangi risiko — identifikasi, analisis, remediasi prioritas tertinggi tepat waktu
- Command atau switch — tidak ada di room ini
