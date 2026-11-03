# Resume: Pengantar Kursus dan Delapan Security Domain CISSP (3 November 2026)

Sumber: Coursera, Google Cybersecurity Certificate. Cakupan: video pengantar kursus, dua video delapan security domain, video perjalanan karier Ashley, bacaan "Security domains cybersecurity analysts need to know", matching exercise, dan practice assignment "Test your knowledge: More about the CISSP security domains".

## 1. Gambaran Umum Materi

Dunia keamanan, atau **cybersecurity**, sangat luas. Bagian pengantar ini membangun pengetahuan inti yang dipakai profesional keamanan untuk memitigasi dan mengelola risiko serta ancaman organisasi setiap hari.

Topik yang akan dibahas dalam rangkaian video berikutnya:

- Fokus dari **eight security domains** CISSP.
- **Threats**, **risks**, dan **vulnerabilities** secara lebih rinci.
- **Three layers of the web** beserta contoh berbagai jenis serangan.
- Pengelolaan risiko dengan **NIST Risk Management Framework (NIST RMF)** dari National Institute of Standards and Technology.

Delapan domain sebelumnya sudah diperkenalkan di kursus pertama, dan pembahasan ini memperdalam fokus masing-masing. Video setelah domain membahas threats, risks, vulnerabilities (termasuk **ransomware**) dan three layers of the web.

## 2. Konsep Dasar

### 2.1 Eight Security Domains CISSP

CISSP mengidentifikasi delapan kategori keamanan. Tim keamanan menggunakannya untuk tiga hal:

- Mengorganisasi tugas harian.
- Mengidentifikasi celah keamanan yang dapat menimbulkan konsekuensi negatif bagi organisasi.
- Menetapkan **security posture**.

### 2.2 Security Posture

**Security posture** adalah kemampuan organisasi mengelola pertahanan terhadap aset dan data kritisnya serta bereaksi terhadap perubahan. Setiap organisasi wajib mengembangkannya.

### 2.3 Information Security (InfoSec)

**InfoSec** adalah serangkaian proses yang ditetapkan untuk mengamankan informasi, dan terkait dengan domain security and risk management. Organisasi dapat memakai **playbooks** dan pelatihan sebagai bagian dari program security and risk management, berdasarkan kebutuhan dan risiko yang dirasakan.

Proses desain InfoSec yang disebut:

- Incident response
- Vulnerability management
- Application security
- Cloud security
- Infrastructure security

## 3. Delapan Security Domain

### 3.1 Domain 1: Security and Risk Management

Berfokus pada pembentukan security posture organisasi. Area fokusnya:

- **Security goals and objectives**: mendefinisikan tujuan keamanan untuk mengurangi risiko terhadap aset dan data kritis seperti **PII** (personally identifiable information).
- **Risk mitigation**: prosedur dan aturan yang tepat untuk mengurangi dampak risiko, misalnya breach, dengan cepat.
- **Compliance**: metode utama untuk mengembangkan kebijakan keamanan internal, persyaratan regulasi, dan standar independen.
- **Business continuity**: kemampuan menjaga produktivitas sehari-hari melalui rencana pemulihan bencana (disaster recovery plans).
- **Legal regulations**: hukum berbeda di tiap negara, tetapi tujuannya serupa. Profesional keamanan mengikuti aturan dan ekspektasi perilaku etis untuk meminimalkan negligence, abuse, atau fraud.
- **Professional and organizational ethics**.

Contoh: tim keamanan mungkin perlu mengubah cara PII diperlakukan agar sesuai dengan **GDPR** (General Data Protection Regulation) milik Uni Eropa.

### 3.2 Domain 2: Asset Security

Berfokus pada pengamanan aset digital dan fisik, serta **storage, maintenance, retention, dan destruction** data fisik maupun virtual. Aset seperti PII atau **SPII** harus terlindungi, baik saat tersimpan di komputer, ditransfer lewat jaringan seperti internet, maupun dikumpulkan secara fisik.

Poin penting:

- Mengetahui data apa yang dimiliki dan siapa yang mengaksesnya diperlukan untuk security posture yang kuat.
- Kehilangan atau pencurian aset meningkatkan risiko, sehingga pelacakan aset dan data sangat penting.
- **Security impact analysis**, penetapan **recovery plan**, dan pengelolaan paparan data bergantung pada tingkat risiko tiap aset.
- Analis dapat membuat **backup** agar lingkungan bisa dipulihkan bila security incident membahayakan data.
- Contoh disposal: analis mengawasi penghancuran **hard drive** agar data privat tidak bisa diakses threat actors.

### 3.3 Domain 3: Security Architecture and Engineering

Berfokus pada optimalisasi keamanan data dengan memastikan tools, sistem, dan proses yang efektif tersedia. Security architects dan engineers membuat proses ini.

Konsep inti: **shared responsibility**, yaitu semua individu berperan aktif menurunkan risiko dan menjaga keamanan fisik maupun virtual. Kebijakan yang mendorong pengguna mengenali dan melaporkan masalah keamanan membuat banyak isu tertangani cepat.

Prinsip desain terkait (dibahas lebih lanjut di bagian program berikutnya):

- Threat modeling
- Least privilege
- Defense in depth
- Fail securely
- Separation of duties
- Keep it simple
- Zero trust
- Trust but verify

Contoh pengelolaan data: tool **SIEM** (security information and event management) memantau tanda aktivitas login atau pengguna yang tidak biasa, yang dapat menandakan threat actor mencoba mengakses data privat.

### 3.4 Domain 4: Communication and Network Security

Berfokus pada pengelolaan dan pengamanan jaringan fisik serta komunikasi nirkabel, mencakup komunikasi on-site, remote, dan cloud.

- Organisasi dengan lingkungan remote, hybrid, dan on-site harus menjaga data tetap aman. Mengelola koneksi eksternal pekerja remote adalah tantangan.
- Kontrol keamanan jaringan seperti **restricted network access** melindungi pengguna saat bepergian atau bekerja di luar kantor utama.
- Contoh risiko: koneksi **bluetooth** tidak aman dan **hotspot wifi publik**. Tim keamanan dapat menghapus akses ke kanal tersebut di tingkat organisasi agar karyawan tidak terdorong berperilaku tidak aman.

### 3.5 Domain 5: Identity and Access Management (IAM)

Berfokus pada akses dan otorisasi untuk menjaga data tetap aman. IAM memastikan identitas pengguna dipercaya dan diautentikasi, serta akses ke aset fisik dan logis diotorisasi. Ini mencegah pengguna tidak sah sambil memungkinkan pengguna berwenang menjalankan tugasnya.

Tujuan utama: mengurangi risiko keseluruhan terhadap sistem dan data. Bagi entry-level analyst, ini berarti membatasi akses pengguna hanya pada yang dibutuhkan.

Contoh masalah: bila semua orang memakai login administrator yang sama, tidak ada cara melacak siapa mengakses data apa, dan saat breach, aktivitas sah tidak dapat dipisahkan dari aktivitas threat actor.

Empat komponen IAM:

- **Identification**: pengguna menyatakan siapa dirinya melalui user name, kartu akses, atau data biometrik seperti sidik jari.
- **Authentication**: verifikasi untuk membuktikan identitas, misalnya password atau PIN.
- **Authorization**: tingkat akses setelah identitas terkonfirmasi, bergantung pada peran dalam organisasi.
- **Accountability**: pemantauan dan pencatatan tindakan pengguna, seperti percobaan login, untuk membuktikan sistem dan data digunakan semestinya.

**Principle of least privilege**: hanya memberi akses dan otorisasi minimal yang diperlukan untuk menyelesaikan suatu tugas. Contoh: customer service representative hanya dapat melihat data privat pelanggan, seperti nomor telepon, saat menangani masalah pelanggan itu, lalu akses dicabut setelah masalah selesai.

### 3.6 Domain 6: Security Assessment and Testing

Berfokus pada identifikasi dan mitigasi risks, threats, dan vulnerabilities melalui tiga kegiatan:

- **Security control testing**: memeriksa tujuan dan sasaran organisasi, lalu mengevaluasi apakah kontrol yang dipakai benar-benar mencapainya. Membantu menemukan cara baru dan lebih baik memitigasi ancaman.
- **Pengumpulan dan analisis data keamanan** secara berkala untuk mencegah ancaman dan risiko.
- **Security audits** untuk memantau dan mengurangi kemungkinan data breach. Contoh tugas: mengaudit user permissions untuk memvalidasi tingkat akses pengguna.

Analis dapat memakai hasil evaluasi dan laporan security assessment untuk memperbaiki kontrol yang ada atau menerapkan kontrol baru, misalnya mewajibkan **multi-factor authentication**. Organisasi juga dapat memakai **penetration testers (pen testers)** untuk menemukan vulnerabilities yang dapat dieksploitasi threat actor.

### 3.7 Domain 7: Security Operations

Berfokus pada investigasi potensi data breach dan penerapan langkah pencegahan setelah security incident.

Alur investigasi:

- Investigasi dimulai setelah security incident teridentifikasi dan membutuhkan urgensi tinggi.
- Bila ada serangan aktif, prioritasnya memitigasi serangan dan mencegah eskalasi agar informasi privat terlindungi.
- Setelah ancaman dinetralkan, bukti digital dan fisik dikumpulkan untuk **digital forensic investigation**, yang mengidentifikasi kapan, bagaimana, dan mengapa breach terjadi.
- Hasilnya menentukan area perbaikan dan langkah pencegahan serangan berikutnya.

Strategi, proses, dan tools yang tercakup:

- Training and awareness
- Reporting and documentation
- Intrusion detection and prevention
- SIEM tools
- Log management
- Incident management
- Playbooks
- Post-breach forensics
- Reflecting on lessons learned

Tim di domain ini dilatih menangani serangan aktif, misalnya data dalam jumlah besar diakses dari jaringan internal di luar jam kerja normal.

### 3.8 Domain 8: Software Development Security

Berfokus pada **secure coding practices**, yaitu panduan yang direkomendasikan untuk membuat aplikasi dan layanan yang aman, sehingga layanan andal dan melindungi organisasi serta penggunanya.

- **Software development lifecycle** adalah proses yang dipakai tim untuk membangun produk dan fitur dengan cepat. Keamanan harus masuk ke setiap elemennya, dari desain, pengembangan, pengujian, hingga rilis. Keamanan tidak boleh menjadi afterthought.
- Contoh integrasi per fase: **secure design review** pada fase desain, **secure code reviews** pada fase pengembangan dan pengujian, **penetration testing** pada fase deployment dan implementasi.
- **Application security tests** membantu memastikan vulnerabilities teridentifikasi dan dimitigasi. Diperlukan sistem untuk menguji konvensi pemrograman, software executables, dan langkah keamanan yang tertanam.
- **Quality assurance** dan pen tester memastikan perangkat lunak memenuhi standar keamanan dan kinerja.
- Contoh: analis di perusahaan farmasi memastikan enkripsi dikonfigurasi benar untuk perangkat medis baru yang menyimpan data pasien.

### 3.9 Ringkasan Pencocokan Domain dan Area Fokus

Hasil matching exercise "Identify: CISSP's eight security domains":

- Mengoptimalkan keamanan data dengan tools, sistem, dan proses efektif: **Security architecture and engineering**.
- Security goals and objectives, risk mitigation, compliance, business continuity, dan hukum: **Security and risk management**.
- Mengamankan aset serta storage, maintenance, retention, destruction data: **Asset security**.
- Mengelola dan mengamankan jaringan fisik dan komunikasi nirkabel: **Communication and network security**.
- Security control testing, audits, pengumpulan dan analisis data: **Security assessment and testing**.
- Access, authorization, dan kebijakan untuk mengamankan data dan mengelola aset: **Identity and access management**.
- Investigasi dan langkah pencegahan: **Security operations**.
- Secure coding practices untuk aplikasi dan layanan aman: **Software development security**.

## 4. Perjalanan Karier dan Keterampilan Non-Teknis

Video ini menampilkan Ashley, CE Enablement Lead untuk SecOps sales di Google (menyiapkan pelatihan bagi customer engineers yang mendukung produk Google).

### 4.1 Jalur Karier yang Tidak Linear

- Bergabung dengan Army tepat setelah SMA, kemudian bekerja di ritel (toko perkakas, penjualan video game, perusahaan freight) sebelum mengenal teknologi sebagai pilihan.
- Militer melatih ulang di bidang IT, membuatnya memenuhi syarat sebagai PC technician.
- Kuliah di community college program associate degree cybersecurity dan mengerjakan sertifikasi.
- Menghadiri **DEFCON**, konferensi hacking besar, yang memberi kejelasan jalur karier.
- Menjadi security analyst pertama pada 2017 melalui **Veterans Training Program** gratis bagi veteran, bekerja hampir lima tahun di perusahaan itu sebelum ke Google.

### 4.2 Soft Skills dan Profil T-Shaped

- Kerja tim wajib dikuasai pemula. Pengalaman customer service, seperti menghadapi pelanggan sulit dan meredakan situasi, sangat berguna di IT.
- Profesional modern bersifat **T-shaped**: soft skills, people skills, dan technical skills.
- Kemampuan **analisis** tidak harus teknis. Membedah rhetorical devices dalam sebuah buku termasuk keterampilan analisis.
- Tidak perlu menjadi software engineer. Pekerjaan ini banyak berkaitan dengan orang dan proses, sehingga pengetahuan coding tidak selalu dibutuhkan.
- Ada banyak cara masuk ke bidang ini, dan berpikir di luar kebiasaan membantu mendapatkan pijakan awal.

## 5. Hasil Latihan

Practice assignment "Test your knowledge: More about the CISSP security domains" (nilai 100%, batas lulus 75%). Jawaban yang benar pada soal yang ditampilkan:

- Domain yang berfokus pada akses dan otorisasi untuk menjaga data aman dan memastikan pengguna mengikuti kebijakan: **identity and access management**.
- Fokus security and risk management: mendefinisikan security goals and objectives, risk mitigation, compliance, business continuity, dan regulasi.
- Domain untuk security control testing, analisis data, dan security audits: **security assessment and testing**.
- Domain untuk investigasi dan langkah pencegahan: **security operations**.

## 6. Glosarium Istilah Wajib Hafal

- **Security posture**: kemampuan organisasi mengelola pertahanan aset dan data kritis serta bereaksi terhadap perubahan.
- **Eight security domains**: delapan kategori keamanan CISSP.
- **InfoSec**: proses yang ditetapkan untuk mengamankan informasi.
- **PII**: personally identifiable information.
- **SPII**: sensitive personally identifiable information.
- **Risk mitigation**: prosedur dan aturan untuk mengurangi dampak risiko dengan cepat.
- **Compliance**: dasar pengembangan kebijakan internal, persyaratan regulasi, dan standar independen.
- **Business continuity**: menjaga produktivitas melalui rencana pemulihan bencana.
- **GDPR**: General Data Protection Regulation milik Uni Eropa.
- **Security impact analysis**: analisis dampak untuk menentukan perlakuan aset sesuai tingkat risiko.
- **Shared responsibility**: semua individu berperan aktif menurunkan risiko.
- **SIEM**: security information and event management.
- **Principle of least privilege**: akses minimal yang diperlukan untuk menyelesaikan tugas.
- **Identification, authentication, authorization, accountability**: empat komponen IAM.
- **Multi-factor authentication**: contoh kontrol baru yang diterapkan setelah assessment.
- **Security control testing**: evaluasi apakah kontrol mencapai tujuan organisasi.
- **Security audit**: pemantauan untuk mengurangi kemungkinan data breach.
- **Penetration tester (pen tester)**: pihak yang mencari vulnerabilities yang dapat dieksploitasi threat actor.
- **Digital forensic investigation**: investigasi untuk mengetahui kapan, bagaimana, dan mengapa breach terjadi.
- **Secure coding practices**: panduan membuat aplikasi dan layanan yang aman.
- **Software development lifecycle**: proses tim membangun produk dan fitur perangkat lunak.
- **T-shaped**: kombinasi soft skills, people skills, dan technical skills.
- **NIST RMF**: Risk Management Framework dari NIST (dibahas di materi berikutnya).

## 7. Tools & Platform Rujukan

Materi ini tidak menyebut tools atau platform eksternal dengan URL eksplisit. Yang disebut hanya nama atau kategorinya:

- **SIEM tools**: memantau tanda aktivitas login atau pengguna yang tidak biasa, serta mendukung log management dan incident management. Tidak ada URL.
- **DEFCON**: konferensi hacking besar yang menjadi titik balik karier Ashley. Tidak ada URL.
- **Veterans Training Program**: program pelatihan gratis bagi veteran yang membawa Ashley ke pekerjaan analis pertamanya. Tidak ada URL.

## 8. Catatan Ringkas untuk Ditulis Tangan

Tidak ada command atau switch dalam materi ini.

### 8.1 Pengantar

- Cybersecurity — bidang luas, butuh pengetahuan, skill, dan tools
- Topik ke depan — eight domains, threats/risks/vulnerabilities, three layers of the web, NIST RMF
- Ransomware — dibahas di video setelah domain
- Eight domains — kategori CISSP untuk organisasi tugas, cari celah, tetapkan security posture
- Security posture — kemampuan kelola pertahanan aset/data kritis dan bereaksi terhadap perubahan
- InfoSec — proses mengamankan informasi

### 8.2 Domain 1 sampai 4

- D1 Security and risk management — goals/objectives, risk mitigation, compliance, business continuity, legal regulations, ethics
- Risk mitigation — prosedur/aturan kurangi dampak risiko cepat
- Compliance — kebijakan internal, regulasi, standar independen
- Business continuity — pertahankan produktivitas, disaster recovery plans
- Etika — minimalkan negligence, abuse, fraud
- Proses InfoSec — incident response, vulnerability management, application security, cloud security, infrastructure security
- GDPR — contoh aturan Uni Eropa yang mengubah perlakuan PII
- D2 Asset security — aset digital/fisik; storage, maintenance, retention, destruction
- PII/SPII — wajib aman saat disimpan, ditransfer, dikumpulkan
- Asset — security impact analysis, recovery plan, backup; contoh hancurkan hard drive
- D3 Security architecture and engineering — tools, sistem, proses efektif; dibuat architects/engineers
- Shared responsibility — semua individu turunkan risiko
- Prinsip desain — threat modeling, least privilege, defense in depth, fail securely, separation of duties, keep it simple, zero trust, trust but verify
- SIEM — pantau login/aktivitas tidak biasa
- D4 Communication and network security — jaringan fisik dan nirkabel; on-site, remote, cloud
- D4 — restricted network access; hindari bluetooth tidak aman dan wifi publik

### 8.3 Domain 5 sampai 8

- D5 IAM — akses dan otorisasi; identitas dipercaya dan diautentikasi
- Identification — user name, kartu akses, biometrik
- Authentication — password, PIN
- Authorization — level akses sesuai peran
- Accountability — catat tindakan, misal percobaan login
- Least privilege — akses minimal untuk tugas, cabut setelah selesai
- Login admin bersama — tak bisa lacak, tak bisa bedakan penyerang
- D6 Security assessment and testing — control testing, analisis data, audits
- D6 — pen testers cari vulnerabilities; audit user permissions; contoh kontrol baru MFA
- D7 Security operations — investigasi dan pencegahan setelah incident
- D7 alur — urgensi, netralkan serangan, kumpulkan bukti, digital forensic (kapan/bagaimana/mengapa)
- D7 cakupan — training, reporting, IDS/IPS, SIEM, log management, incident management, playbooks, post-breach forensics, lessons learned
- D8 Software development security — secure coding practices
- D8 — keamanan di setiap fase SDLC, bukan afterthought
- D8 per fase — secure design review (desain), secure code reviews (pengembangan/pengujian), penetration testing (deployment)
- D8 — application security tests, QA, pen tester; contoh enkripsi perangkat medis

### 8.4 Karier dan Soft Skills

- Ashley — CE Enablement Lead SecOps sales, Google
- Jalur — Army, ritel, pelatihan IT militer, community college, sertifikasi, DEFCON, analis 2017, Veterans Training Program
- Soft skills — kerja tim, meredakan situasi, komunikasi (dari customer service)
- T-shaped — soft skills, people skills, technical skills
- Analisis — tidak harus teknis
- Coding — tidak wajib; kerja dengan orang dan proses
- Pesan — banyak jalan masuk, berpikir di luar kebiasaan

### 8.5 Latihan

- Matching exercise — delapan domain dicocokkan dengan area fokus, selesai
- Practice assignment — nilai 100%, lulus minimal 75%
