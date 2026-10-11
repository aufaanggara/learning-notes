# Resume Room: Playbooks dan Incident Response

Tanggal: 11 Oktober 2026

Cakupan: video pengantar playbook, bacaan "More about playbooks", flashcard "Phases of an incident response playbook", video pengalaman software engineer Google, dan Practice Assignment "Test your knowledge: Incident response".

Catatan: room ini murni teori, tidak ada command atau switch yang dibahas.



## 1. Konsep Inti

### 1.1 Playbook

**Playbook** adalah manual yang memuat detail tentang setiap tindakan operasional. Isinya berupa daftar langkah yang sudah ditentukan dan selalu diperbarui untuk dilakukan saat merespons sebuah insiden. Playbook juga menjelaskan tools apa yang sebaiknya dipakai untuk merespons insiden keamanan.

Playbook penting karena penanganan ancaman menuntut urgensi, efisiensi, dan akurasi agar ancaman cepat teridentifikasi dan dimitigasi sehingga risikonya berkurang. Playbook memastikan semua orang mengikuti daftar tindakan yang konsisten dengan cara yang sudah ditetapkan, siapa pun yang menangani kasusnya.

### 1.2 Strategi dan Rencana dalam Playbook

Playbook disertai sebuah **strategi** yang menguraikan ekspektasi terhadap anggota tim yang ditugaskan pada suatu task. Sebagian playbook juga mencantumkan individu yang bertanggung jawab. Ekspektasi tersebut dilengkapi sebuah **rencana (plan)** yang menentukan bagaimana task spesifik dalam playbook harus diselesaikan.

### 1.3 Playbook sebagai Living Document

Playbook diperlakukan sebagai **living document**: sering diperbarui oleh anggota tim keamanan agar sesuai dengan perubahan industri dan ancaman baru. Pengelolaannya umumnya kolaboratif karena anggota tim memiliki tingkat keahlian yang berbeda.

Pembaruan biasanya dilakukan ketika:

- Teridentifikasi kegagalan, misalnya kelalaian dalam kebijakan dan prosedur, atau dalam playbook itu sendiri.
- Terjadi perubahan standar industri, seperti perubahan hukum atau kepatuhan regulasi.
- Lanskap keamanan siber berubah akibat taktik dan teknik threat actor yang terus berkembang.

### 1.4 Incident Response

**Incident response** adalah upaya cepat sebuah organisasi untuk mengidentifikasi serangan, membatasi (contain) kerusakan, dan memperbaiki dampak dari security breach.

**Incident response playbook** adalah panduan enam fase untuk membantu memitigasi dan mengelola insiden keamanan dari awal hingga akhir. Playbook ini dirujuk sepanjang seluruh insiden, bukan hanya sebelum, saat awal, atau sesudah insiden.



## 2. Jenis-Jenis Playbook

### 2.1 Klasifikasi Umum

Playbook yang dibahas di video terbagi menjadi empat jenis:

- **Incident response**
- **Security alerts**
- **Teams-specific**
- **Product-specific**

Playbook incident response adalah yang paling umum dipakai di bidang keamanan siber.

### 2.2 Playbook Berdasarkan Insiden atau Kerentanan Tertentu

Playbook kadang dibuat untuk insiden dan kerentanan spesifik, misalnya **ransomware**, **vishing**, dan **business email compromise (BEC)**. Playbook **incident response** dan **vulnerability response** sangat umum, tetapi bukan satu-satunya jenis yang dikembangkan organisasi.

### 2.3 Perbedaan Antar Organisasi dan Negara

Setiap organisasi punya tools, metodologi, protokol, dan prosedur playbook yang berbeda. Individu yang terlibat di tiap langkah respons juga berbeda tergantung negara.

Contohnya, persyaratan notifikasi insiden dari hukum dan regulasi pemerintah serta standar kepatuhan memengaruhi isi playbook. Persyaratan ini dapat berubah menurut tempat insiden bermula dan jenis data yang terdampak.

### 2.4 Playbook Incident dan Vulnerability Response

Kedua playbook ini umum dipakai profesional keamanan siber tingkat pemula (entry-level). Keduanya dikembangkan berdasarkan tujuan dalam **business continuity plan** organisasi, yaitu jalur yang telah ditetapkan agar bisnis dapat pulih dan tetap beroperasi normal meski terjadi gangguan seperti security breach.

Persamaan keduanya: sama-sama berisi daftar langkah yang telah ditentukan dan selalu diperbarui. Mengikuti langkah tersebut diperlukan untuk:

- Mematuhi standar dan protokol hukum maupun organisasi.
- Meminimalkan kesalahan.
- Memastikan tindakan penting dilakukan dalam jangka waktu tertentu.

### 2.5 Risiko dan Forensik

Saat insiden, ancaman, atau kerentanan terjadi atau teridentifikasi, tingkat risiko bagi organisasi bergantung pada potensi kerusakan terhadap asetnya. Rumus dasar yang disebut di materi: risiko sama dengan kemungkinan terjadinya sebuah ancaman. Karena itu rasa urgensi sangat penting.

Mengikuti playbook juga penting ketika ada tugas **forensik**, sebab kesalahan penanganan data dapat dengan mudah mengompromikan data forensik sehingga tidak bisa dipakai.



## 3. Enam Fase Incident Response Playbook

### 3.1 Ringkasan Fase

| Fase | Tujuan inti | Aktivitas utama |
|---|---|---|
| 1. Preparation | Memitigasi kemungkinan, risiko, dan dampak insiden | Dokumentasi, staffing plans, edukasi pengguna |
| 2. Detection and analysis | Mendeteksi dan menganalisis events | Proses terdefinisi dan teknologi yang sesuai |
| 3. Containment | Mencegah kerusakan lanjutan | Tindakan membatasi insiden |
| 4. Eradication and recovery | Kembali ke operasi normal | Hapus artifacts, pulihkan lingkungan |
| 5. Post-incident activity | Lebih siap menghadapi insiden mendatang | Dokumentasi, lapor pimpinan, lessons learned |
| 6. Coordination | Kepatuhan dan respons terkoordinasi | Pelaporan dan berbagi informasi |

### 3.2 Fase 1: Preparation

Organisasi bersiap mengurangi kemungkinan, risiko, dan dampak insiden dengan mendokumentasikan prosedur, menyusun **staffing plans**, dan mengedukasi pengguna. Fase ini menjadi fondasi keberhasilan incident response. Contohnya: membuat rencana dan prosedur incident response yang menguraikan peran dan tanggung jawab tiap anggota tim keamanan.

### 3.3 Fase 2: Detection and Analysis

Tujuannya mendeteksi dan menganalisis events dengan proses terdefinisi dan teknologi yang tepat. Fase ini membantu analis menentukan apakah breach telah terjadi dan seberapa besar potensi **magnitude**-nya.

### 3.4 Fase 3: Containment

Tujuannya mencegah kerusakan lebih lanjut dan mengurangi dampak langsung insiden. Containment adalah prioritas tinggi karena mencegah risiko berkelanjutan terhadap aset dan data kritis.

### 3.5 Fase 4: Eradication and Recovery

Fase ini menghapus total **artifacts** insiden agar organisasi kembali beroperasi normal. Artifacts dihilangkan dengan menghapus **malicious code** dan memitigasi **vulnerabilities**. Setelah **due diligence** dilakukan, lingkungan terdampak dipulihkan ke kondisi aman, yang disebut juga **IT restoration**.

### 3.6 Fase 5: Post-Incident Activity

Fase ini mencakup mendokumentasikan insiden, menginformasikan pimpinan organisasi, dan menerapkan **lessons learned**. Bergantung pada tingkat keparahan, organisasi dapat melakukan **full-scale incident analysis** untuk menemukan **root cause** dan menerapkan pembaruan atau perbaikan guna meningkatkan postur keamanan.

### 3.7 Fase 6: Coordination

Fase ini mencakup pelaporan insiden dan berbagi informasi sepanjang proses incident response sesuai standar organisasi. Pentingnya: memastikan organisasi memenuhi **compliance requirements** dan memungkinkan respons serta penyelesaian yang terkoordinasi.

### 3.8 Versi Langkah pada Bacaan

Pada bacaan, langkah umum playbook incident dan vulnerability response ditulis sebagai: preparation, detection, analysis, containment, eradication, dan recovery from an incident. Langkah tambahan berupa aktivitas pasca-insiden dan koordinasi upaya sepanjang investigasi serta respons. Ini sama dengan enam fase di atas, hanya dengan detection dan analysis dipisah serta eradication dan recovery dipisah.



## 4. Hubungan SIEM dan Playbook

Tools **SIEM** mengumpulkan dan menganalisis data, lalu menggunakannya untuk mendeteksi ancaman dan menghasilkan alert yang memberi tahu tim keamanan tentang potensi insiden. Setelah analis menerima alert SIEM, ia memakai playbook yang sesuai untuk memandu proses respons.

Jadi keduanya bekerja bersama menyediakan cara yang terstruktur dan efisien untuk merespons insiden. SIEM bertugas mendeteksi dan memberi alert, playbook bertugas memandu respons. Keduanya tidak untuk memprediksi ancaman masa depan dan tidak menghilangkan kebutuhan intervensi manusia.



## 5. Playbook dalam Praktik (Pengalaman Software Engineer Google)

### 5.1 Fungsi Playbook bagi Profesional Baru

Di Google, proses dan playbook adalah dokumentasi berisi pedoman atau algoritma untuk menentukan tindakan terbaik saat insiden keamanan atau privasi dan serangan aktif, demi mengelola data dan keamanan pengguna dengan baik.

Bagi pendatang baru, playbook memungkinkan menyelesaikan masalah dengan pengalaman puluhan tahun dari para senior, karena dapat bersandar pada playbook dan saran orang lain. Playbook dipakai untuk open attacks, privacy incidents, data leaks, denial of service attacks, service alerts, dan lainnya.

### 5.2 Vulnerability yang Dilaporkan Eksternal

Seorang peneliti keamanan di luar organisasi dapat menemukan celah yang berpotensi membocorkan data pengguna. Laporan vulnerability datang bersama **remediation guidance**, yaitu langkah-langkah yang disarankan untuk memperbaikinya.

### 5.3 Saran Karier

- Bicara dengan sebanyak mungkin orang di industri untuk memahami pekerjaan, keterampilan yang dibutuhkan, lowongan, dan budaya kerja berbagai perusahaan.
- Pekerjaan rekayasa perangkat lunak sekitar 50% berupa komunikasi dan review desain, bukan sekadar mengetik sendirian.
- Keamanan adalah bidang yang selalu relevan dan semakin penting.

### 5.4 Takeaway Bacaan

Proses dan prosedur dalam playbook harus terus disempurnakan. Pada setiap insiden terdokumentasi, tim perlu mempertimbangkan pelajaran yang didapat dan perbaikan agar penanganan berikutnya lebih efektif. Playbook menciptakan struktur dan memastikan kepatuhan terhadap hukum.



## 6. Glossary

- **Playbook**: manual berisi langkah terdefinisi dan terkini untuk merespons insiden.
- **Incident response**: upaya cepat mengidentifikasi serangan, membatasi kerusakan, dan memperbaiki dampak breach.
- **Incident response playbook**: panduan enam fase untuk mengelola insiden dari awal hingga akhir.
- **Living document**: dokumen yang sering diperbarui mengikuti perubahan industri dan ancaman.
- **Business continuity plan**: jalur yang memungkinkan bisnis pulih dan tetap beroperasi saat terjadi gangguan.
- **Preparation**: fase persiapan lewat dokumentasi, staffing plans, dan edukasi pengguna.
- **Detection and analysis**: fase mendeteksi dan menganalisis events untuk menilai breach dan magnitude-nya.
- **Containment**: fase mencegah kerusakan lanjutan dan mengurangi dampak langsung.
- **Eradication and recovery**: fase menghapus artifacts insiden dan memulihkan lingkungan.
- **Artifacts**: sisa-sisa insiden, seperti malicious code, yang harus dihapus total.
- **IT restoration**: nama lain proses pemulihan lingkungan terdampak ke kondisi aman.
- **Post-incident activity**: fase dokumentasi, pelaporan ke pimpinan, dan lessons learned.
- **Lessons learned**: pelajaran dari insiden untuk meningkatkan kesiapan di masa depan.
- **Root cause**: akar penyebab insiden, ditemukan lewat full-scale incident analysis.
- **Coordination**: fase pelaporan dan berbagi informasi sesuai standar organisasi.
- **Compliance requirements**: persyaratan kepatuhan hukum dan regulasi yang harus dipenuhi.
- **SIEM**: tools pengumpul dan penganalisis data yang mendeteksi ancaman dan menghasilkan alert.
- **Ransomware, vishing, BEC**: contoh serangan yang dapat memiliki playbook khusus.
- **Remediation guidance**: langkah perbaikan yang menyertai laporan vulnerability.
- **Due diligence**: kehati-hatian wajib sebelum pemulihan dimulai.



## 7. Tools & Platform Rujukan

Tidak ada tools atau platform eksternal yang direkomendasikan untuk dipakai langsung. Bacaan hanya menyebut sumber rujukan berikut bagi yang akan bekerja di luar Amerika Serikat. URL tidak ditampilkan di materi, sehingga hanya nama yang dicatat.

- **United Kingdom, National Cyber Security Centre (NCSC) - Incident Management**: panduan manajemen insiden.
- **Australian Government - Cyber Incident Response Plan**: rencana respons insiden siber.
- **Japan Computer Emergency Response Team Coordination Center (JPCERT/CC) - Vulnerability Handling and related guidelines**: panduan penanganan kerentanan.
- **Government of Canada - Ransomware Playbook**: playbook khusus ransomware.
- **Scottish Government - Playbook Templates**: template playbook.



## 8. Catatan Ringkas untuk Ditulis Tangan

### 8.1 Playbook

- Playbook — manual langkah respons insiden, terdefinisi dan terkini
- Fungsi — konsisten siapa pun penangannya; urgensi, efisiensi, akurasi
- Strategi — ekspektasi anggota tim (kadang nama penanggung jawab)
- Plan — cara task dalam playbook dikerjakan
- Living document — sering diperbarui, dikelola kolaboratif
- Pemicu update — kegagalan; standar/hukum berubah; taktik threat actor berkembang

### 8.2 Jenis Playbook

- 4 jenis (video) — incident response, security alerts, teams-specific, product-specific
- Khusus serangan — ransomware, vishing, BEC
- Incident dan vulnerability response — umum, dipakai entry-level
- Dasar pembuatan — business continuity plan
- Beda negara — hukum notifikasi insiden memengaruhi isi playbook

### 8.3 Incident Response

- Incident response — identifikasi serangan, contain kerusakan, perbaiki dampak
- IR playbook — panduan 6 fase, dirujuk sepanjang seluruh insiden
- Risiko — bergantung potensi kerusakan aset; urgensi penting
- Forensik — salah tangani data = data forensik rusak

### 8.4 Enam Fase

- 1 Preparation — dokumentasi, staffing plans, edukasi user
- 2 Detection and analysis — deteksi dan analisis events, nilai breach dan magnitude
- 3 Containment — cegah kerusakan lanjutan, kurangi dampak langsung
- 4 Eradication and recovery — hapus artifacts (malicious code, vulnerabilities), IT restoration
- 5 Post-incident activity — dokumentasi, lapor pimpinan, lessons learned, root cause
- 6 Coordination — lapor dan berbagi info sesuai standar, compliance

### 8.5 SIEM dan Praktik

- SIEM — kumpul dan analisis data, deteksi ancaman, kirim alert
- SIEM + playbook — alert masuk, playbook memandu respons
- Playbook bukan prediksi masa depan, bukan pengganti manusia
- Remediation guidance — langkah perbaikan di laporan vulnerability
- Pengalaman engineer — playbook = pengalaman senior; pekerjaan 50% komunikasi

### 8.6 Rujukan

- NCSC UK — Incident Management
- Australian Government — Cyber Incident Response Plan
- JPCERT/CC — Vulnerability Handling
- Government of Canada — Ransomware Playbook
- Scottish Government — Playbook Templates

### 8.7 Command dan Switch

- Tidak ada command atau switch di room ini
