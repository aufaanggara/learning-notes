# Resume: Navigate Threats, Risks, and Vulnerabilities

**Tanggal:** 3 November 2026

Cakupan: video threat/risk/vulnerability, video ransomware dan dampak organisasi, video profil Security Engineer, video NIST Risk Management Framework, bacaan *Manage common threats, risks, and vulnerabilities*, dan practice quiz.

---

## 1. Aset dan Peran Analis

### 1.1 Aset

**Asset** adalah item yang dianggap memiliki nilai bagi organisasi. Aset bisa digital maupun fisik, dan tujuan utama organisasi adalah melindunginya.

Aset digital: informasi pribadi karyawan, klien, atau vendor, misalnya Social Security Number (SSN) atau nomor identifikasi nasional, tanggal lahir, nomor rekening bank, dan alamat surat. Kekayaan intelektual seperti paten atau data berhak cipta juga termasuk aset.

Aset fisik: payment kiosk, server, komputer desktop, dan ruang kantor.

### 1.2 Peran Analis Tingkat Pemula

Analis menangani aset digital dan fisik organisasi di lingkungan yang penuh threat, risk, dan vulnerability. Tugas utamanya membantu membela aset dari ancaman dalam maupun luar, mengedukasi orang agar sadar keamanan, mendorong pelaporan aktivitas mencurigakan, serta memantau dan mendokumentasikan akses ke aset kritis.

---

## 2. Konsep Inti: Threat, Risk, Vulnerability

### 2.1 Threat

**Threat** adalah setiap keadaan atau peristiwa yang dapat berdampak negatif terhadap aset.

- **Social engineering:** teknik manipulasi yang mengeksploitasi kesalahan manusia untuk memperoleh informasi pribadi, akses, atau barang berharga.
- **Phishing:** bentuk social engineering berupa tautan berbahaya di email yang tampak berasal dari perusahaan atau orang yang sah, untuk mencuri data sensitif seperti nama pengguna, kata sandi, atau informasi perbankan.
- **Insider threat:** staf atau vendor menyalahgunakan akses resminya untuk memperoleh data yang dapat merugikan organisasi.
- **Advanced persistent threat (APT):** threat actor mempertahankan akses tidak sah ke sebuah sistem dalam waktu lama.

### 2.2 Risk

**Risk** adalah segala sesuatu yang dapat memengaruhi confidentiality, integrity, atau availability aset. Rumus dasarnya: risk sama dengan kemungkinan terjadinya sebuah threat.

> Risk ibarat terlambat tiba di tempat kerja, sedangkan threat adalah lalu lintas, kecelakaan, atau ban kempis.

Contoh risk: tidak adanya protokol backup sehingga data tidak dapat dipulihkan saat kecelakaan atau insiden keamanan.

Faktor yang memengaruhi kemungkinan risk:

- **External risk:** apa pun di luar organisasi yang berpotensi merugikan aset, misalnya threat actor yang mencoba mengakses informasi pribadi.
- **Internal risk:** karyawan atau mantan karyawan, vendor, atau mitra tepercaya yang menimbulkan risiko keamanan.
- **Legacy systems:** sistem lama yang tidak terdata atau tidak diperbarui tetapi masih memengaruhi aset, misalnya mesin penjual otomatis lama yang menerima kartu kredit atau workstation yang masih terhubung ke sistem akuntansi lama.
- **Multiparty risk:** mengalihdayakan pekerjaan ke vendor pihak ketiga memberi mereka akses ke kekayaan intelektual seperti trade secrets, desain perangkat lunak, dan penemuan.
- **Software compliance/licensing:** perangkat lunak yang tidak diperbarui atau tidak patuh, atau patch yang tidak dipasang tepat waktu.

### 2.3 Vulnerability

**Vulnerability** adalah kelemahan yang dapat dieksploitasi oleh threat. Contohnya firewall, perangkat lunak, atau aplikasi yang usang; kata sandi lemah; dan data rahasia yang tidak dilindungi.

Manusia juga dapat menjadi vulnerability. Tindakan klien, vendor eksternal, atau karyawan sangat memengaruhi jaringan internal, sehingga keamanan harus menjadi upaya bersama.

### 2.4 Hubungan Ketiganya

Risk hanya terbentuk jika threat dan vulnerability sama-sama ada. Threat tanpa vulnerability tidak menemukan celah, dan vulnerability tanpa threat tidak dieksploitasi.

---

## 3. Klasifikasi Risiko Aset

Organisasi menilai risiko pada tingkat rendah, sedang, dan tinggi, bergantung pada kemungkinan threat dan nilai aset.

| Tingkat | Definisi | Contoh |
|---|---|---|
| **Low-risk** | Tidak merugikan reputasi atau operasional dan tidak menimbulkan kerugian finansial jika terkompromi | Konten situs web, data riset yang dipublikasikan |
| **Medium-risk** | Tidak tersedia untuk publik dan dapat merugikan keuangan, reputasi, atau operasional | Rilis awal laporan pendapatan kuartalan yang dapat memengaruhi nilai saham |
| **High-risk** | Dilindungi regulasi atau hukum; jika terkompromi berdampak parah pada keuangan, operasional, atau reputasi | Aset bocor berisi SPII, PII, atau kekayaan intelektual |

---

## 4. Strategi dan Framework Manajemen Risiko

### 4.1 Strategi Mengelola Risk

- **Acceptance:** menerima risiko agar tidak mengganggu business continuity.
- **Avoidance:** membuat rencana untuk menghindari risiko sepenuhnya.
- **Transference:** mengalihkan risiko ke pihak ketiga untuk dikelola.
- **Mitigation:** mengurangi dampak risiko yang sudah diketahui.

### 4.2 Framework

Organisasi menerapkan proses manajemen risiko berdasarkan framework yang diterima luas untuk melindungi aset digital dan fisik. Contoh yang umum di industri keamanan siber: **NIST Risk Management Framework (NIST RMF)** dan **Health Information Trust Alliance (HITRUST)**.

---

## 5. NIST Risk Management Framework (RMF)

NIST menyediakan banyak framework untuk mengelola risk, threat, dan vulnerability. Analis tingkat pemula mungkin tidak terlibat di semua langkah RMF, tetapi memahaminya menjadi nilai tambah saat mencari kerja. RMF memiliki tujuh langkah:

| Langkah | Fungsi | Peran/contoh bagi analis |
|---|---|---|
| **1. Prepare** | Aktivitas mengelola risiko keamanan dan privasi sebelum breach | Memantau risiko dan mengidentifikasi kontrol untuk menguranginya |
| **2. Categorize** | Mengembangkan proses dan tugas manajemen risiko berdasarkan dampak risiko pada confidentiality, integrity, availability | Mengikuti proses organisasi untuk melindungi aset kritis, misalnya data pelanggan |
| **3. Select** | Memilih, menyesuaikan, dan mendokumentasikan kontrol pelindung | Menjaga playbook tetap mutakhir, membantu mengelola dokumentasi |
| **4. Implement** | Menerapkan rencana keamanan dan privasi | Mengubah persyaratan kata sandi jika karyawan sering reset password |
| **5. Assess** | Menentukan apakah kontrol diterapkan dengan benar dan memenuhi kebutuhan | Mengidentifikasi kelemahan dan menilai apakah tools, prosedur, kontrol, protokol perlu diubah |
| **6. Authorize** | Bertanggung jawab atas risiko keamanan dan privasi | Membuat laporan, plans of action, dan milestone sesuai tujuan keamanan |
| **7. Monitor** | Mengetahui bagaimana sistem beroperasi | Memastikan sistem mendukung tujuan keamanan; perubahan dilakukan bila tidak sesuai |

---

## 6. Vulnerability dan Risiko Umum Terkini

### 6.1 OWASP Top 10

**Open Web Application Security Project (OWASP)** menerbitkan dokumen kesadaran standar tentang 10 risiko keamanan paling kritis bagi aplikasi web, diperbarui secara berkala. Tiga risiko baru pada pembaruan 2017 ke 2021: **insecure design**, **software and data integrity failures**, dan **server-side request forgery**. Pembaruan ini menunjukkan keamanan terus berkembang sehingga analis harus mengikuti taktik dan teknik threat actor terkini.

Daftar OWASP 2017:

1. A01 Injection
2. A02 Broken Authentication
3. A03 Sensitive Data Exposure
4. A04 XML External Entities (XXE)
5. A05 Broken Access Control
6. A06 Security Misconfiguration
7. A07 Cross-Site Scripting (XSS)
8. A08 Insecure Deserialization
9. A09 Using Components with Known Vulnerabilities
10. A10 Insufficient Logging and Monitoring

### 6.2 Contoh Vulnerability

- **ProxyLogon:** vulnerability pre-authenticated pada server Microsoft Exchange; threat actor dapat menyelesaikan proses autentikasi untuk menjalankan kode berbahaya dari jarak jauh.
- **ZeroLogon:** vulnerability pada protokol autentikasi Netlogon milik Microsoft. Protokol autentikasi adalah cara memverifikasi identitas; Netlogon memastikan identitas pengguna sebelum mengizinkan akses.
- **Log4Shell:** memungkinkan penyerang menjalankan kode Java di komputer orang lain atau membocorkan informasi sensitif, dengan mengambil alih perangkat terhubung internet dari jarak jauh.
- **PetitPotam:** memengaruhi Windows New Technology LAN Manager (NTLM); teknik pencurian yang memungkinkan penyerang berbasis LAN memulai permintaan autentikasi.
- **Security logging and monitoring failures:** logging dan monitoring tidak memadai sehingga penyerang mengeksploitasi vulnerability tanpa diketahui organisasi.
- **Server-side request forgery:** penyerang memanipulasi aplikasi sisi server agar mengakses dan memperbarui sumber daya backend, juga dapat dipakai mencuri data.

### 6.3 Vulnerability Management

**Vulnerability management** adalah pemantauan sistem untuk mengidentifikasi dan memitigasi vulnerability. Patch yang tersedia tetapi tidak diterapkan tetap membuka peluang intrusi, sehingga pemantauan konstan penting. Semakin cepat vulnerability diidentifikasi dan ditambal, semakin kecil paparan organisasi.

---

## 7. Ransomware dan Tiga Lapisan Web

### 7.1 Ransomware

**Ransomware** adalah serangan di mana threat actor mengenkripsi data organisasi lalu menuntut pembayaran untuk memulihkan akses. Setelah disebarkan, ransomware dapat membekukan sistem jaringan, membuat perangkat tidak dapat digunakan, dan mengunci data rahasia. Threat actor memberikan **decryption key** (ibarat kata sandi untuk membuka kembali data) setelah tebusan dibayar. Negosiasi tebusan dan kebocoran data sering terjadi melalui dark web.

### 7.2 Tiga Lapisan Web

- **Surface web:** konten yang dapat diakses lewat web browser; lapisan yang paling banyak dipakai.
- **Deep web:** umumnya memerlukan otorisasi, misalnya intranet organisasi.
- **Dark web:** hanya dapat diakses dengan perangkat lunak khusus; dipilih kriminal karena kerahasiaannya.

---

## 8. Dampak Utama terhadap Organisasi

### 8.1 Dampak Finansial

Aset yang dikompromikan, misalnya oleh malware, menyebabkan produksi dan layanan terganggu, biaya perbaikan, dan denda akibat ketidakpatuhan hukum dan regulasi.

### 8.2 Identity Theft

Organisasi harus memutuskan apakah dan berapa lama menyimpan data pribadi pelanggan, karyawan, dan vendor. Menyimpan data sensitif selalu berisiko. **PII** dapat dijual atau dibocorkan melalui dark web karena threat actor merasa aman dari konsekuensi hukum.

### 8.3 Kerusakan Reputasi

Basis pelanggan yang solid mendukung misi, visi, dan tujuan finansial. Vulnerability yang dieksploitasi dapat membuat pelanggan pindah ke kompetitor atau memicu pemberitaan buruk yang merusak reputasi permanen. Kehilangan data pelanggan juga dapat berujung pada sanksi hukum dan denda.

---

## 9. Mitigasi oleh Analis

Organisasi harus terus meningkatkan identifikasi dan mitigasi vulnerability untuk meminimalkan threat dan risk.

- Mengedukasi orang mengenali email phishing.
- Memakai kartu akses untuk membatasi akses ruang fisik bagi pengunjung luar.
- Mendorong karyawan melaporkan aktivitas mencurigakan.
- Memantau dan mendokumentasikan akses karyawan ke aset kritis.
- Menerapkan protokol dan langkah keamanan agar tim siap menghadapi kejadian seperti ransomware.

---

## 10. Perspektif Praktisi: Security Engineer di Google

Pekerjaan harian mencakup menganalisis risiko keamanan dan memberi solusi.

- **Exception request:** menilai apakah seseorang benar-benar butuh akses khusus ke perangkat atau dokumen berdasarkan peran atau proyeknya.
- **Misconfiguration dan permintaan akses berlebih:** contoh nyata adalah vendor yang mengubah **OAuth scope**, yaitu meminta izin lebih banyak ke layanan Google daripada sebelumnya. Kasus baru ini ditangani bersama tim mitra.
- **Sistem usang:** mesin yang belum di-patch dan lemahnya device management policies terdengar seperti isu IT, tetapi juga isu keamanan siber.
- **Kolaborasi:** bekerja dengan banyak tim adalah bagian besar pekerjaan.

---

## 11. Glossary

- **Asset:** item yang dianggap bernilai bagi organisasi, digital atau fisik.
- **Threat:** keadaan atau peristiwa yang dapat berdampak negatif pada aset.
- **Risk:** hal yang dapat memengaruhi confidentiality, integrity, atau availability aset; kemungkinan terjadinya threat.
- **Vulnerability:** kelemahan yang dapat dieksploitasi oleh threat.
- **Social engineering:** manipulasi yang mengeksploitasi kesalahan manusia.
- **Phishing:** tautan atau pesan palsu untuk mencuri data sensitif.
- **Insider threat:** penyalahgunaan akses resmi oleh staf atau vendor.
- **APT (advanced persistent threat):** akses tidak sah jangka panjang ke sistem.
- **PII / SPII:** informasi identitas pribadi / yang sensitif.
- **Legacy systems:** sistem lama yang tidak diperbarui.
- **Multiparty risk:** risiko akibat akses vendor pihak ketiga.
- **Acceptance, Avoidance, Transference, Mitigation:** empat strategi mengelola risk.
- **NIST RMF:** framework tujuh langkah manajemen risiko.
- **HITRUST:** Health Information Trust Alliance, framework manajemen risiko.
- **OWASP Top 10:** daftar 10 risiko paling kritis aplikasi web.
- **Vulnerability management:** pemantauan sistem untuk mengidentifikasi dan memitigasi vulnerability.
- **Ransomware:** malware yang mengenkripsi data dan menuntut tebusan.
- **Decryption key:** kunci untuk membuka data terenkripsi.
- **Surface web, deep web, dark web:** tiga lapisan web.
- **Exception request:** permintaan akses khusus yang perlu dianalisis.
- **OAuth scope:** cakupan izin yang diminta pihak ketiga ke layanan.
- **Misconfiguration:** konfigurasi keliru atau akses melebihi kebutuhan.

---

## 12. Tools & Platform Rujukan

Tidak ada URL eksplisit yang terlihat di materi, sehingga hanya nama yang dicantumkan.

- **NIST RMF:** framework manajemen risiko tujuh langkah.
- **HITRUST (Health Information Trust Alliance):** framework manajemen risiko, terutama informasi kesehatan.
- **NIST cybersecurity risks list:** daftar risiko keamanan siber.
- **OWASP Top Ten:** daftar 10 risiko keamanan aplikasi web paling kritis.
- **NIST National Vulnerability Database:** referensi vulnerability yang dipublikasikan.
- **CISA Known Exploited Vulnerabilities Catalog:** katalog vulnerability yang diketahui telah dieksploitasi.

---

## 13. Catatan Ringkas untuk Ditulis Tangan

### Aset
- Asset — item bernilai bagi organisasi
- Digital — SSN, tanggal lahir, rekening bank, alamat surat, kekayaan intelektual
- Fisik — payment kiosk, server, desktop, ruang kantor

### Threat / Risk / Vulnerability
- Threat — kejadian yang berdampak negatif pada aset
- Social engineering — manipulasi kesalahan manusia
- Phishing — tautan email palsu curi data
- Insider threat — staf/vendor salahgunakan akses
- APT — akses tidak sah jangka panjang
- Risk — dampak ke CIA aset; risk = kemungkinan threat
- Analogi — risk = telat kerja; threat = macet, kecelakaan, ban kempis
- Faktor risk — external, internal, legacy systems, multiparty, software compliance/licensing
- Vulnerability — kelemahan yang dieksploitasi threat
- Contoh vuln — firewall/software usang, password lemah, data tak terlindungi, manusia
- Risk ada jika threat DAN vulnerability ada

### Klasifikasi Aset
- Low — tidak merugikan; web publik, riset terbit
- Medium — non-publik; rilis awal laporan pendapatan
- High — dilindungi hukum; SPII, PII, kekayaan intelektual

### Strategi Risiko
- Acceptance — terima risiko
- Avoidance — hindari sepenuhnya
- Transference — alihkan ke pihak ketiga
- Mitigation — kurangi dampak
- Framework — NIST RMF, HITRUST

### NIST RMF (7 langkah)
- Prepare — kelola risiko sebelum breach
- Categorize — proses/tugas risiko berdasar CIA
- Select — pilih, sesuaikan, dokumentasi kontrol (playbook)
- Implement — terapkan rencana keamanan/privasi
- Assess — kontrol benar & efektif?
- Authorize — akuntabilitas; laporan, plan of action, milestone
- Monitor — pantau sistem vs tujuan keamanan

### OWASP dan Vulnerability
- OWASP Top 10 — 10 risiko kritis aplikasi web
- Baru 2017 ke 2021 — insecure design, software and data integrity failures, SSRF
- ProxyLogon — Exchange, pre-authenticated, kode jahat jarak jauh
- ZeroLogon — Netlogon, autentikasi
- Log4Shell — jalankan kode Java, ambil alih perangkat
- PetitPotam — NTLM, penyerang LAN mulai permintaan autentikasi
- Logging/monitoring failures — serangan tak terdeteksi
- SSRF — manipulasi aplikasi server akses backend
- Vulnerability management — pantau, patch cepat

### Ransomware dan Web
- Ransomware — enkripsi data, minta tebusan
- Decryption key — kunci buka data
- Surface web — akses browser
- Deep web — perlu otorisasi (intranet)
- Dark web — software khusus, kriminal

### Dampak Organisasi
- Finansial — produksi terhenti, biaya perbaikan, denda
- Identity theft — PII dijual/bocor via dark web
- Reputasi — pelanggan pindah, pemberitaan buruk, sanksi hukum

### Mitigasi
- Edukasi phishing
- Kartu akses fisik
- Lapor aktivitas mencurigakan
- Pantau dan dokumentasi akses aset kritis

### Praktisi (Google Security Engineer)
- Exception request — nilai kebutuhan akses khusus
- OAuth scope — izin vendor ke layanan Google
- Misconfiguration — akses berlebih
- Sistem usang — isu keamanan, bukan hanya IT
- Kolaborasi lintas tim
