# Resume Materi: Security Frameworks and Controls (Coursera)

Tanggal: 11 Oktober 2026

Room ini bersifat teori murni, sehingga tidak ada command, tool, atau switch yang dibahas.


## 1. Peran Security Analyst

### 1.1 Cakupan Peran

**Security analyst** tidak hanya melindungi organisasi, tetapi juga melindungi orang-orang yang datanya dikelola organisasi: customer, vendor, dan employee.

Kebocoran data (**data breach**) pada pihak-pihak tersebut dapat merusak stabilitas finansial dan reputasi mereka.

### 1.2 Pekerjaan Harian

Pekerjaan sehari-hari berpusat pada penerapan **security frameworks**, **security controls**, dan **design principles** ke dalam **security audit**.

Ketiganya saling melengkapi. Framework memberi pedoman umum, controls menjadi langkah konkret, dan design principles menjadi dasar perancangan sistem keamanan.

Menjaga kerahasiaan informasi pelanggan (**confidentiality**) adalah bagian penting dari pekerjaan ini, dan **NIST Cybersecurity Framework** berperan besar di dalamnya.


## 2. Security Frameworks

### 2.1 Definisi

**Security frameworks** adalah panduan (guidelines) untuk menyusun rencana yang memitigasi risiko dan ancaman terhadap data dan privasi, misalnya **social engineering** dan **ransomware**.

Organisasi memakainya sebagai titik awal untuk membuat kebijakan dan proses keamanan sendiri, karena kebutuhan perlindungan organisasi dan individu sering saling tumpang tindih.

### 2.2 Dukungan terhadap Compliance

Frameworks membantu organisasi mematuhi hukum dan regulasi (**compliance**).

Contohnya, industri kesehatan memakai frameworks untuk mematuhi **HIPAA** (Health Insurance Portability and Accountability Act) di Amerika Serikat, yang mewajibkan tenaga medis menjaga keamanan informasi pasien.

### 2.3 Cakupan: Virtual dan Fisik

Keamanan mencakup ranah virtual dan fisik. Banyak organisasi memiliki rencana menjaga keselamatan lingkungan kerja, misalnya akses gedung yang memerlukan key card atau badge.

Sebagian frameworks memberi panduan untuk **prevent**, **detect**, dan **respond** terhadap security breach, terutama serangan social engineering seperti **phishing** yang menargetkan karyawan.

### 2.4 Faktor Manusia

Manusia (people) adalah ancaman terbesar bagi keamanan perusahaan, bukan organisasi kompetitor.

Karena itu frameworks dipakai untuk menyusun rencana yang meningkatkan awareness dan edukasi karyawan agar mereka mampu melindungi organisasi, rekan kerja, dan diri sendiri.

Dua hal yang esensial untuk meminimalkan kemungkinan breach:

- Pelatihan mengenali **red flags** (potensi ancaman)
- Rencana pelaporan dan penanganan isu keamanan secara cepat

### 2.5 Contoh Framework Spesifik

**Cyber Threat Framework (CTF)** dikembangkan pemerintah AS. Menurut Office of the Director of National Intelligence, tujuannya menyediakan bahasa yang seragam untuk mendeskripsikan dan mengomunikasikan aktivitas ancaman siber.

Dengan bahasa seragam, profesional keamanan siber dapat menganalisis dan berbagi informasi lebih efisien, sehingga respons terhadap lanskap ancaman yang terus berkembang (taktik dan teknik threat actors) semakin baik.

**ISO/IEC 27001** adalah framework yang diakui secara internasional, bagian dari keluarga standar ISO 27000. Framework ini membantu organisasi dari semua sektor dan ukuran mengelola keamanan aset seperti informasi finansial, kekayaan intelektual, data karyawan, dan informasi pihak ketiga.

ISO/IEC 27001 menguraikan requirement untuk **information security management system**, best practices, dan controls untuk mengelola risiko. Framework ini tidak mewajibkan controls tertentu, tetapi menyediakan kumpulan controls yang dapat dipakai untuk meningkatkan **security posture**.

Framework lain yang disebut sebagai pembahasan sebelumnya adalah **NIST Risk Management Framework (RMF)** dan **NIST Cybersecurity Framework (CSF)**.


## 3. Security Controls

### 3.1 Definisi

**Security controls** adalah safeguard yang dirancang untuk mengurangi risiko keamanan yang **spesifik**. Controls digunakan bersama frameworks untuk menurunkan kemungkinan dan dampak dari sebuah threat, risk, atau vulnerability.

Tanpa controls yang tepat, organisasi berisiko mengalami kerugian finansial dan kerusakan reputasi akibat trespassing, pembuatan akun karyawan palsu, atau pemberian benefit gratis secara tidak sah.

### 3.2 Kategori Controls

Controls dapat berupa **physical**, **technical**, dan **administrative**, dan biasanya dipakai untuk **prevent**, **detect**, atau **correct** masalah keamanan.

| Kategori | Fokus | Contoh |
|---|---|---|
| **Physical** | Perlindungan fisik | Gates, fences, locks, security guards, CCTV, motion detectors, access cards/badges |
| **Technical** | Teknologi/sistem | Firewalls, MFA, antivirus software |
| **Administrative** | Kebijakan dan prosedur | Separation of duties, authorization, asset classification |

### 3.3 Encryption

**Encryption** adalah proses mengubah data dari format yang dapat dibaca menjadi format terenkode, umumnya dari **plaintext** menjadi **ciphertext**.

Ciphertext adalah pesan mentah terenkode yang tidak dapat dibaca manusia maupun komputer sampai di-**decrypt** kembali ke plaintext. Encryption dipakai untuk menjaga confidentiality data sensitif, misalnya informasi rekening pelanggan atau nomor social security.

### 3.4 Authentication

**Authentication** adalah proses memverifikasi siapa atau apa seseorang atau sesuatu sebelum memberi akses ke resource dalam sistem. Bentuk dasarnya adalah login dengan username dan password.

**Multi-factor authentication (MFA)** mewajibkan password ditambah satu faktor lain, seperti security code atau **biometrics** (sidik jari, suara, pemindaian wajah).

**Biometrics** adalah karakteristik fisik unik untuk verifikasi identitas, misalnya sidik jari, eye scan, atau palm scan.

**Vishing** adalah eksploitasi komunikasi suara elektronik untuk memperoleh informasi sensitif atau menyamar sebagai sumber yang dikenal, misalnya meniru suara seseorang untuk mencuri identitas lalu melakukan kejahatan. Vishing adalah contoh social engineering yang dapat mengeksploitasi biometrics.

### 3.5 Authorization

**Authorization** adalah pemberian akses ke resource tertentu dalam sistem, yaitu memverifikasi bahwa seseorang memiliki izin untuk mengakses suatu resource.

Contohnya, entry-level security analyst di pemerintah federal dapat mengakses data melalui deep web atau data internal yang hanya tersedia bagi pegawai federal.

Perbedaan inti: authentication menjawab "siapa kamu", authorization menjawab "apa yang boleh kamu akses".


## 4. Hubungan Frameworks dan Controls

Frameworks memberi pedoman umum untuk membangun rencana keamanan, sedangkan controls adalah langkah konkret untuk menurunkan risiko spesifik.

Contoh: rumah sakit memakai framework HIPAA sebagai pedoman compliance, lalu menerapkan control MFA agar pasien memvalidasi identitas sebelum mengakses rekam medis.

Frameworks dan controls bersama-sama membentuk **security posture** organisasi dan mendukung pencapaian tujuan keamanan serta kepatuhan hukum.

Keduanya umumnya bersifat **voluntary**, namun organisasi sangat dianjurkan menerapkannya untuk menjaga keamanan aset kritikal.


## 5. CIA Triad

Security controls hanyalah satu elemen dari model keamanan inti yang disebut **CIA triad**: confidentiality, integrity, dan availability.

CIA triad membantu organisasi mempertimbangkan risiko saat menyusun sistem dan security policies. Rincian tiap komponennya dibahas pada materi lanjutan.


## 6. Glossary

- **Security framework**: panduan untuk menyusun rencana mitigasi risiko dan ancaman terhadap data dan privasi.
- **Security control**: safeguard yang mengurangi risiko keamanan spesifik.
- **Security posture**: kondisi keamanan organisasi yang dibentuk oleh frameworks dan controls.
- **Compliance**: kepatuhan terhadap hukum dan regulasi.
- **Voluntary**: bersifat sukarela, tidak diwajibkan, tetapi sangat dianjurkan.
- **Social engineering**: manipulasi manusia untuk memperoleh akses atau informasi sensitif.
- **Phishing**: social engineering yang menargetkan karyawan melalui pesan palsu.
- **Vishing**: social engineering lewat komunikasi suara elektronik.
- **Ransomware**: ancaman terhadap data yang dibahas sebagai risiko yang dimitigasi frameworks.
- **Red flags**: tanda potensi ancaman yang perlu dikenali karyawan.
- **Physical control**: safeguard fisik (gates, guards, CCTV, badges).
- **Technical control**: safeguard berbasis teknologi (firewall, MFA, antivirus).
- **Administrative control**: safeguard berbasis kebijakan (separation of duties, authorization, asset classification).
- **Separation of duties**: pemisahan tugas agar tidak ada satu orang memegang kendali penuh.
- **Asset classification**: pengelompokan aset berdasarkan tingkat kepentingan/sensitivitas.
- **Encryption**: pengubahan data dari plaintext ke ciphertext.
- **Decryption**: pengembalian ciphertext ke plaintext.
- **Plaintext**: data dalam format yang dapat dibaca.
- **Ciphertext**: data terenkode yang tidak dapat dibaca sebelum didekripsi.
- **Authentication**: verifikasi identitas.
- **MFA**: authentication dengan lebih dari satu faktor.
- **Biometrics**: karakteristik fisik unik untuk verifikasi identitas.
- **Authorization**: pemberian izin akses ke resource tertentu.
- **CIA triad**: model keamanan inti (confidentiality, integrity, availability).
- **CTF (Cyber Threat Framework)**: framework AS untuk bahasa seragam tentang aktivitas ancaman siber.
- **ISO/IEC 27001**: framework internasional untuk information security management system.
- **HIPAA**: regulasi AS tentang keamanan informasi pasien.
- **Security audit**: proses tempat frameworks, controls, dan design principles diterapkan.


## 7. Tools & Platform Rujukan

Tidak ada URL eksplisit yang terlihat jelas di materi, sehingga hanya nama yang dicantumkan.

- **NIST Risk Management Framework (RMF)**: framework NIST untuk manajemen risiko keamanan.
- **NIST Cybersecurity Framework (CSF)**: framework NIST untuk membangun security posture organisasi.
- **Cyber Threat Framework (CTF)**: bahasa seragam untuk komunikasi aktivitas ancaman siber (Office of the Director of National Intelligence, AS).
- **ISO/IEC 27001**: standar internasional information security management system.
- **U.S. Department of Health and Human Services, Physical Access Control presentation**: materi tambahan tentang controls untuk melindungi aset terkait kesehatan (hyperlink di materi asli, URL tidak terlihat jelas).


## 8. Catatan Ringkas untuk Ditulis Tangan

### 8.1 Peran Analyst

- Analyst — lindungi organisasi dan orang (customer, vendor, employee)
- Breach — rusak finansial dan reputasi
- Kerja harian — terapkan frameworks, controls, design principles di security audit
- Frameworks = pedoman, controls = langkah konkret, design principles = dasar rancangan
- NIST CSF — jaga confidentiality data pelanggan

### 8.2 Frameworks

- Security framework — panduan susun rencana mitigasi risiko data dan privasi
- Titik awal — dasar kebijakan dan proses organisasi
- Contoh ancaman — social engineering, ransomware
- Compliance — HIPAA (AS, jaga info pasien)
- Cakupan — virtual dan fisik (key card, badge)
- Fungsi — prevent, detect, respond breach
- Phishing — menarget karyawan
- People — ancaman terbesar (bukan kompetitor)
- Solusi — awareness, pelatihan red flags, pelaporan cepat
- CTF — bahasa seragam ancaman siber (ODNI, pemerintah AS)
- ISO/IEC 27001 — standar internasional, ISMS, tidak wajib controls tertentu
- NIST RMF dan CSF — framework rujukan sebelumnya

### 8.3 Controls

- Security control — safeguard kurangi risiko spesifik
- Tanpa controls — rugi finansial dan reputasi (trespassing, akun palsu, benefit gratis)
- Tiga kategori — physical, technical, administrative
- Fungsi — prevent, detect, correct
- Physical — gates, fences, locks, guards, CCTV, motion detectors, badges
- Technical — firewalls, MFA, antivirus
- Administrative — separation of duties, authorization, asset classification

### 8.4 Encryption, Authentication, Authorization

- Encryption — plaintext jadi ciphertext, jaga confidentiality
- Ciphertext — tidak terbaca sebelum decrypt
- Decryption — ciphertext kembali ke plaintext
- Authentication — verifikasi identitas (username + password)
- MFA — password + faktor lain (security code, biometrics)
- Biometrics — ciri fisik unik (sidik jari, eye scan, palm scan, suara, wajah)
- Vishing — exploit komunikasi suara, tiru suara, curi identitas
- Authorization — izin akses ke resource tertentu
- Authentication = siapa kamu, authorization = boleh akses apa

### 8.5 Hubungan dan CIA Triad

- Framework + control — rumah sakit: HIPAA (framework), MFA (control)
- Keduanya — bentuk security posture, dukung tujuan dan compliance
- Sifat — voluntary tapi sangat dianjurkan
- CIA triad — confidentiality, integrity, availability (model inti, dibahas lanjut)
