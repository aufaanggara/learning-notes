# Resume: Play It Safe: Manage Security Risks (Google Cybersecurity Certificate, Course 2)

Tanggal: 11 Oktober 2026

Course ini membahas cara profesional keamanan siber memakai domain keamanan, manajemen risiko, kerangka kerja (framework), kontrol, audit, alat seperti SIEM, dan playbook untuk melindungi operasional bisnis serta aset dan data organisasi.

Catatan: course ini bersifat konsep dan tidak mengajarkan command atau switch tertentu, sehingga resume ini tidak memuat daftar command.

## 1. Delapan Domain Keamanan CISSP

CISSP (Certified Information Systems Security Professional) membagi keamanan menjadi delapan domain. Tiap domain mewakili area tanggung jawab yang saling berkaitan dalam menjaga keamanan organisasi.

### 1.1 Daftar Domain

1. **Security and Risk Management** — menetapkan tujuan keamanan, mitigasi risiko, kepatuhan (compliance), kelangsungan bisnis, dan aspek hukum.
2. **Asset Security** — mengamankan aset digital dan fisik, mencakup penyimpanan, pemeliharaan, retensi, dan penghancuran data.
3. **Security Architecture and Engineering** — mengoptimalkan keamanan data lewat perangkat dan proses yang efektif, serta prinsip seperti shared responsibility.
4. **Communication and Network Security** — mengelola dan mengamankan jaringan fisik dan komunikasi nirkabel.
5. **Identity and Access Management (IAM)** — menjaga identitas dan akses data tetap valid lewat autentikasi dan otorisasi.
6. **Security Assessment and Testing** — melakukan pengujian keamanan, pengumpulan dan analisis data, serta audit.
7. **Security Operations** — menyelidiki insiden dan menerapkan langkah pencegahan; domain ini menaungi kerja SIEM dan incident response.
8. **Software Development Security** — menerapkan praktik pengembangan perangkat lunak yang aman.

### 1.2 Fungsi Domain

Domain membantu organisasi memetakan di mana risiko berada dan siapa yang bertanggung jawab. Satu insiden sering menyentuh beberapa domain sekaligus, misalnya kebocoran data yang melibatkan akses (IAM), jaringan, dan operasi keamanan.

## 2. Ancaman, Risiko, dan Kerentanan

### 2.1 Definisi Inti

**Risk** adalah segala sesuatu yang dapat berdampak pada kerahasiaan, integritas, atau ketersediaan sebuah aset.

**Threat** adalah keadaan atau peristiwa yang dapat berdampak negatif pada aset organisasi.

**Vulnerability** adalah kelemahan yang dapat dimanfaatkan oleh threat.

Hubungannya: threat memanfaatkan vulnerability sehingga menimbulkan risk terhadap aset.

> Analogi: rumah dengan jendela yang kuncinya rusak. Jendela rusak adalah vulnerability, pencuri adalah threat, dan kemungkinan barang hilang adalah risk.

### 2.2 Faktor dan Tingkat Risiko

Risiko dinilai dari **likelihood** (kemungkinan terjadi) dan **impact** (dampak jika terjadi). Risiko dengan kemungkinan dan dampak tinggi diprioritaskan lebih dulu.

Sumber ancaman bisa internal (misalnya **insider threat**) maupun eksternal. Ancaman juga bisa berupa serangan terarah dan berkelanjutan seperti **advanced persistent threat (APT)**.

### 2.3 Strategi Menangani Risiko

- **Mitigation** — mengurangi kemungkinan atau dampak risiko dengan kontrol.
- **Avoidance** — menghindari aktivitas yang menimbulkan risiko.
- **Transference** — memindahkan risiko ke pihak lain, misalnya asuransi.
- **Acceptance** — menerima risiko karena biaya penanganannya lebih besar dari dampaknya.

## 3. Aset dan Data

### 3.1 Manajemen dan Klasifikasi Aset

**Asset management** adalah proses melacak dan mengklasifikasikan aset agar diamankan sesuai nilainya. Klasifikasi umum:

- **Restricted** — sangat sensitif, kebocoran berdampak berat.
- **Confidential** — sensitif, akses terbatas pada pihak yang berwenang.
- **Internal-only** — untuk lingkup internal organisasi.
- **Public** — boleh dibagikan secara terbuka.

### 3.2 Peran dalam Pengelolaan Data

- **Data owner** — pihak yang menentukan siapa boleh mengakses dan bagaimana data dilindungi.
- **Data custodian** — pihak yang menjalankan penyimpanan dan pengamanan teknis data.
- **Data protection officer (DPO)** — memastikan kepatuhan terhadap regulasi privasi.

### 3.3 Data Sensitif

**PII (personally identifiable information)** adalah informasi yang dapat mengidentifikasi seseorang. **SPII (sensitive PII)** adalah PII yang bila bocor berisiko lebih besar, misalnya nomor identitas atau data keuangan.

## 4. Prinsip Dasar dan CIA Triad

### 4.1 CIA Triad

**CIA triad** adalah fondasi keamanan informasi:

- **Confidentiality** — hanya pihak berwenang yang dapat mengakses data.
- **Integrity** — data akurat dan tidak diubah tanpa izin.
- **Availability** — data dan sistem dapat diakses saat dibutuhkan oleh pihak berwenang.

> Analogi: brankas bank. Kunci dan penjaga menjaga kerahasiaan, segel pada isi brankas menjaga integritas, dan jam operasional yang jelas menjaga ketersediaan.

### 4.2 Prinsip Akses

- **Principle of least privilege** — pengguna hanya diberi akses minimum yang dibutuhkan tugasnya.
- **Separation of duties** — tugas penting dibagi ke beberapa orang agar tidak ada satu orang yang menguasai seluruh proses.

## 5. Kerangka Kerja dan Kontrol Keamanan

### 5.1 Pengertian

**Framework** adalah pedoman untuk membangun rencana mengurangi risiko dan ancaman terhadap data dan privasi. **Controls** adalah langkah atau mekanisme spesifik untuk mengurangi risiko. Keduanya menjadi titik awal penyusunan kebijakan dan proses manajemen keamanan.

### 5.2 Kategori Kontrol

- **Administrative** — kebijakan dan prosedur, misalnya pelatihan keamanan dan manajemen akses.
- **Technical** — solusi teknis, misalnya firewall, enkripsi, dan autentikasi multifaktor.
- **Physical** — pengamanan fisik, misalnya kunci, kamera, dan akses gedung.

### 5.3 Tipe Kontrol

- **Preventive** — mencegah insiden terjadi.
- **Detective** — mendeteksi insiden yang sedang atau telah terjadi.
- **Corrective** — memulihkan dan memperbaiki setelah insiden.
- **Deterrent** — menghalangi pelaku agar tidak mencoba menyerang.

### 5.4 NIST Cybersecurity Framework (CSF)

NIST CSF dikelompokkan dalam lima fungsi inti:

1. **Identify** — mengelola risiko terhadap sistem, aset, data, dan kapabilitas.
2. **Protect** — menerapkan perlindungan untuk menjaga layanan penting.
3. **Detect** — mengidentifikasi terjadinya insiden keamanan.
4. **Respond** — menangani insiden yang telah terdeteksi.
5. **Recover** — memulihkan operasi dan aset yang terdampak.

### 5.5 NIST Risk Management Framework (RMF)

NIST RMF adalah proses tujuh langkah untuk mengelola risiko keamanan dan privasi:

1. **Prepare** — menyiapkan organisasi mengelola risiko.
2. **Categorize** — mengategorikan sistem dan informasi berdasarkan dampak.
3. **Select** — memilih kontrol yang sesuai.
4. **Implement** — menerapkan kontrol.
5. **Assess** — menilai apakah kontrol berjalan sebagaimana mestinya.
6. **Authorize** — memberi otorisasi operasi sistem berdasarkan risiko yang diterima.
7. **Monitor** — memantau sistem dan kontrol secara berkelanjutan.

### 5.6 Prinsip Desain Keamanan OWASP

OWASP menyusun prinsip yang menjadi panduan desain sistem yang aman:

- **Minimize attack surface area** — kurangi titik yang bisa diserang.
- **Principle of least privilege** — akses minimum yang diperlukan.
- **Defense in depth** — lapisan pertahanan berlapis.
- **Separation of duties** — pembagian tugas.
- **Keep security simple** — hindari desain yang terlalu rumit.
- **Fix security issues correctly** — perbaiki akar masalah, bukan gejalanya.
- **Establish secure defaults** — konfigurasi awal harus aman.
- **Fail securely** — saat gagal, sistem tetap dalam kondisi aman.
- **Don't trust services** — jangan percaya otomatis pada layanan pihak ketiga.
- **Avoid security by obscurity** — keamanan tidak boleh bergantung pada kerahasiaan desain.

## 6. Audit Keamanan

### 6.1 Konsep

**Security audit** adalah peninjauan catatan, aktivitas, dan kontrol keamanan organisasi untuk menilai kepatuhan terhadap standar dan efektivitas pertahanan. Audit dapat dilakukan **internal** (oleh tim organisasi) atau **eksternal** (oleh pihak independen).

### 6.2 Tahapan Audit

1. **Establish scope and goals** — menetapkan cakupan dan tujuan audit.
2. **Conduct a risk assessment** — menilai risiko yang relevan.
3. **Complete a controls assessment** — meninjau kontrol yang ada.
4. **Assess compliance** — memeriksa kepatuhan terhadap regulasi dan standar.
5. **Communicate results** — menyampaikan temuan dan rekomendasi kepada pemangku kepentingan.

### 6.3 Regulasi dan Standar Kepatuhan

Audit sering dikaitkan dengan regulasi seperti **GDPR**, **PCI DSS**, dan **HIPAA**, yang mengatur perlindungan data pribadi, data kartu pembayaran, dan data kesehatan.

## 7. Alat Keamanan: Log dan SIEM

### 7.1 Log

**Log** adalah catatan kejadian yang terjadi pada sistem, jaringan, dan aplikasi. Analis memakai log untuk mendeteksi aktivitas mencurigakan dan menyelidiki insiden. Sumber log umum mencakup log jaringan, sistem, aplikasi, keamanan, dan autentikasi.

### 7.2 SIEM

**SIEM (security information and event management)** mengumpulkan dan menganalisis data log dari banyak sumber, menormalkannya, lalu menampilkan peringatan (alert) dan dasbor agar analis dapat bereaksi cepat. Fungsinya mencakup deteksi ancaman, pemantauan, dan dukungan investigasi.

> Analogi: ruang kontrol CCTV gedung. Banyak kamera (sumber log) mengalir ke satu ruang pemantauan yang menyorot kejadian janggal.

### 7.3 Jenis Penerapan SIEM

SIEM dapat berupa **on-premise**, **cloud-based**, atau **hybrid**, tergantung kebutuhan organisasi.

### 7.4 Contoh Tool SIEM

- **Splunk** (Splunk Enterprise dan Splunk Cloud) — platform analisis log dan data.
- **Chronicle** — SIEM berbasis cloud dari Google.

### 7.5 Network Protocol Analyzer

Alat yang menangkap dan memeriksa lalu lintas jaringan: **Wireshark** (antarmuka grafis) dan **tcpdump** (command line).

## 8. Playbook dan Incident Response

### 8.1 Playbook

**Playbook** adalah panduan berisi langkah dan aturan untuk menangani situasi tertentu secara konsisten, seperti respons insiden. Playbook bersifat dinamis dan diperbarui seiring pelajaran dari insiden. Playbook dapat dijalankan manual maupun otomatis lewat **SOAR (security orchestration, automation, and response)**.

> Analogi: prosedur evakuasi kebakaran. Tiap orang tahu langkahnya sehingga bereaksi cepat dan tidak panik.

### 8.2 Incident Response

**Incident response** adalah rangkaian langkah yang diambil organisasi untuk mengidentifikasi, menahan, dan memulihkan diri dari insiden keamanan. Tim yang menanganinya antara lain **CSIRT** (computer security incident response team) dan **SOC** (security operations center).

### 8.3 Enam Fase Incident Response

1. **Preparation** — menyiapkan kebijakan, tim, tools, dan playbook.
2. **Detection and Analysis** — mendeteksi dan menganalisis apakah suatu kejadian merupakan insiden.
3. **Containment** — membatasi penyebaran dan dampak insiden.
4. **Eradication** — menghilangkan akar penyebab, misalnya malware atau akun yang disusupi.
5. **Recovery** — mengembalikan sistem ke operasi normal.
6. **Post-incident activity** — meninjau insiden dan mencatat pelajaran untuk perbaikan.

### 8.4 Dokumentasi dan Chain of Custody

Seluruh tindakan selama insiden harus didokumentasikan. **Chain of custody** mencatat siapa saja yang menangani bukti dan kapan, agar bukti tetap sah dan tidak terkontaminasi.

## 9. Glosarium

- **Asset management** — pelacakan dan klasifikasi aset.
- **Availability** — data dan sistem tersedia bagi pihak berwenang.
- **CIA triad** — confidentiality, integrity, availability.
- **Chain of custody** — catatan penanganan bukti.
- **Confidentiality** — data hanya diakses pihak berwenang.
- **Controls** — mekanisme untuk mengurangi risiko.
- **CSIRT** — tim respons insiden keamanan komputer.
- **Defense in depth** — pertahanan berlapis.
- **Framework** — pedoman membangun rencana keamanan.
- **Incident response** — proses menangani insiden keamanan.
- **Integrity** — data akurat dan tidak diubah tanpa izin.
- **Least privilege** — akses minimum sesuai tugas.
- **NIST CSF** — lima fungsi: Identify, Protect, Detect, Respond, Recover.
- **NIST RMF** — tujuh langkah manajemen risiko.
- **PII / SPII** — data pribadi / data pribadi sensitif.
- **Playbook** — panduan langkah penanganan situasi.
- **Risk** — hal yang dapat berdampak pada CIA aset.
- **SIEM** — pengumpul dan penganalisis log terpusat.
- **SOAR** — orkestrasi dan otomasi respons keamanan.
- **Threat** — peristiwa yang dapat merugikan aset.
- **Vulnerability** — kelemahan yang dapat dieksploitasi.

## 10. Tools & Platform Rujukan

- **Splunk** — platform SIEM dan analisis log.
- **Chronicle** — SIEM cloud dari Google.
- **Wireshark** — protocol analyzer grafis untuk lalu lintas jaringan.
- **tcpdump** — protocol analyzer berbasis command line.
- **OWASP** — sumber prinsip dan panduan keamanan aplikasi.
- **NIST (CSF dan RMF)** — sumber kerangka kerja manajemen risiko.
- **Google Cybersecurity Community** — komunitas peserta sertifikat untuk diskusi dan jejaring.

## 11. Catatan Ringkas untuk Ditulis Tangan

### 11.1 Domain CISSP (8)

- Security and Risk Management — tujuan, risiko, compliance
- Asset Security — amankan aset, data lifecycle
- Security Architecture and Engineering — desain sistem aman
- Communication and Network Security — amankan jaringan
- IAM — autentikasi dan otorisasi
- Security Assessment and Testing — uji dan audit
- Security Operations — investigasi, SIEM, IR
- Software Development Security — kode aman

### 11.2 Risiko

- Risk — dampak ke CIA aset
- Threat — peristiwa merugikan
- Vulnerability — kelemahan dieksploitasi
- Faktor risiko — likelihood dan impact
- Insider threat — ancaman internal
- APT — serangan terarah berkelanjutan
- Strategi — mitigation, avoidance, transference, acceptance

### 11.3 Aset dan Data

- Klasifikasi — restricted, confidential, internal-only, public
- Data owner — tentukan akses
- Data custodian — jaga teknis penyimpanan
- DPO — pastikan kepatuhan privasi
- PII — data pengenal pribadi
- SPII — PII sensitif

### 11.4 Prinsip

- CIA — confidentiality, integrity, availability
- Least privilege — akses minimum
- Separation of duties — tugas dibagi

### 11.5 Framework dan Kontrol

- Framework — pedoman; controls — langkah spesifik
- Kategori — administrative, technical, physical
- Tipe — preventive, detective, corrective, deterrent
- NIST CSF — Identify, Protect, Detect, Respond, Recover
- NIST RMF — Prepare, Categorize, Select, Implement, Assess, Authorize, Monitor
- OWASP — minimize attack surface, least privilege, defense in depth, separation of duties, keep it simple, fix correctly, secure defaults, fail securely, don't trust services, no security by obscurity

### 11.6 Audit

- Internal vs eksternal
- Tahap — scope dan goals, risk assessment, controls assessment, compliance, communicate results
- Regulasi — GDPR, PCI DSS, HIPAA

### 11.7 Tools

- Log — catatan kejadian sistem
- SIEM — kumpul dan analisis log, alert, dasbor
- Penerapan SIEM — on-premise, cloud, hybrid
- Splunk — SIEM analisis log
- Chronicle — SIEM cloud Google
- Wireshark — analisis paket (GUI)
- tcpdump — analisis paket (CLI)
- SOAR — otomasi respons

### 11.8 Playbook dan IR

- Playbook — panduan langkah, terus diperbarui
- IR — identifikasi, tahan, pulihkan insiden
- CSIRT dan SOC — tim penanganan insiden
- 6 fase IR — Preparation, Detection and Analysis, Containment, Eradication, Recovery, Post-incident activity
- Chain of custody — catat penanganan bukti
- Dokumentasi — catat semua tindakan
