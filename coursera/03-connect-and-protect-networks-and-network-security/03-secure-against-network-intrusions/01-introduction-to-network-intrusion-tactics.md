# Resume Materi: Secure Networks from Attacks (Network Intrusion, Backdoor, dan Incident Response)

Google Cybersecurity Course, 1 November 2026

---

## 1. Gambaran Umum Materi

Bagian ini membahas mengapa jaringan harus diamankan, jenis serangan intrusi yang perlu dikenali analis keamanan, dampak serangan terhadap organisasi, serta perspektif praktisi tentang penanganan insiden (incident response).

Materi terdiri dari lima komponen:

- Video pengantar tentang pengamanan jaringan dari serangan.
- Video "mengapa jaringan perlu diamankan" beserta studi kasus Home Depot.
- Bacaan "How intrusions compromise your system" (interception, backdoor, dampak organisasi, key takeaways).
- Video wawancara seorang Chaos Specialist di Google tentang karier dan incident response.

Tidak ada command, tool, atau switch yang dibahas di materi ini karena seluruhnya bersifat konsep.

## 2. Mengapa Jaringan Perlu Diamankan

### 2.1 Ancaman yang Selalu Ada

Jaringan terus-menerus berisiko diserang oleh peretas jahat (**malicious hackers**). Setiap jaringan memiliki kerentanan bawaan (**inherent vulnerabilities**) sehingga bisa menjadi target kapan saja.

Metode penyusupan yang disebut di materi adalah **malware**, **spoofing**, dan **packet sniffing**. Operasional jaringan juga dapat diganggu oleh serangan seperti **packet flooding**.

Jika satu saja serangan berhasil, dampaknya bisa sangat merugikan organisasi. Karena itu analis keamanan harus selalu waspada terhadap kerentanan jaringan dan segera bertindak untuk memitigasinya.

### 2.2 Motivasi Penyerang

Penyerang memiliki motivasi yang beragam, sehingga jaringan apa pun bisa menjadi target. Motivasi yang disebutkan:

- Finansial.
- Personal.
- Politik.
- **Disgruntled employee**: karyawan yang tidak puas dan ingin merugikan organisasi.
- Aktivis yang tidak setuju dengan nilai-nilai perusahaan dan ingin mengganggu operasionalnya.

### 2.3 Studi Kasus: Home Depot (2014)

Sekelompok peretas menyusup dan menginfeksi server jaringan ritel perbaikan rumah Amerika, Home Depot, dengan malware. Saat administrator jaringan berhasil menghentikan serangan, data kartu kredit dan debit milik lebih dari 56 juta pelanggan sudah dicuri.

Kasus ini menunjukkan bahwa satu intrusi jaringan dapat berdampak luas terhadap organisasi besar sekaligus jutaan pelanggannya.

## 3. Jenis Serangan Jaringan

### 3.1 Network Interception Attacks

Serangan ini bekerja dengan mencegat lalu lintas jaringan, lalu mencuri informasi berharga atau mengganggu transmisi data.

**Packet sniffing** adalah penggunaan alat hardware atau software untuk menangkap dan memeriksa data yang sedang dalam perjalanan (**in transit**). Pelaku dapat melihat informasi yang bukan haknya.

Selain membaca, pelaku juga bisa mengubah lalu lintas yang dicegat. Caranya dengan menyisipkan modifikasi kode berbahaya, atau mengubah pesan dan mengganggu operasi jaringan. Contohnya, penyerang mencegat transfer bank dan mengganti rekening penerima menjadi rekening yang ia kendalikan.

Dua bentuk interception lain, yaitu **on-path attacks** dan **replay attacks**, hanya disebut namanya dan akan dibahas lebih lanjut di bagian course berikutnya.

### 3.2 Backdoor Attacks

**Backdoor** adalah kelemahan yang sengaja ditinggalkan oleh programmer atau administrator sistem dan jaringan yang memungkinkan seseorang melewati mekanisme kontrol akses normal.

Tujuan awalnya sah, yaitu membantu programmer melakukan troubleshooting atau tugas administratif. Namun backdoor juga dapat dipasang penyerang setelah berhasil mengompromikan organisasi, agar mereka memiliki **persistent access**.

> *Sebuah organisasi punya banyak pengamanan seperti kamera, pemindai biometrik, dan kode akses agar karyawan tidak keluar-masuk tanpa terdeteksi. Namun seorang karyawan bisa mencari celah dengan menemukan pintu belakang gedung yang tidak terlalu diawasi, lalu keluar diam-diam di siang hari tanpa ketahuan.*

Analogi di atas menggambarkan backdoor dalam sistem: semua kontrol akses utama bisa dilewati lewat satu jalur yang kurang diawasi.

Setelah masuk melalui backdoor, penyerang dapat:

- Memasang malware.
- Melakukan serangan **denial of service (DoS)**.
- Mencuri informasi pribadi.
- Mengubah pengaturan keamanan lain sehingga sistem rentan terhadap serangan berikutnya.

### 3.3 Denial of Service (DoS)

**DoS attack** menargetkan jaringan atau server dan membanjirinya dengan lalu lintas jaringan, sehingga layanan terganggu atau tidak bisa diakses.

## 4. Dampak Serangan terhadap Organisasi

### 4.1 Finansial

Sistem yang dilumpuhkan (misalnya lewat DoS) menghentikan tugas yang menghasilkan pendapatan. Tergantung ukuran organisasi, gangguan operasional bisa menelan biaya jutaan dolar.

Biaya lain mencakup pembangunan ulang infrastruktur software dan pembayaran dalam jumlah besar terkait potensi **ransomware**. Jika data pribadi klien atau pelanggan bocor, perusahaan juga bisa menghadapi litigasi berat dan biaya penyelesaian (**settlement costs**) bila pelanggan menuntut secara hukum.

### 4.2 Reputasi

Jika publik mengetahui sebuah perusahaan pernah diserang, masyarakat dapat mulai meragukan praktik keamanannya. Mereka bisa berhenti mempercayakan data pribadi kepada perusahaan itu dan beralih ke kompetitor, sehingga retensi pelanggan menurun.

### 4.3 Keselamatan Publik

Serangan pada jaringan pemerintah berpotensi memengaruhi keselamatan dan kesejahteraan warga negara. Lembaga pertahanan di seluruh dunia banyak berinvestasi melawan taktik **cyber warfare**.

Jika pelaku berhasil mengakses jaringan listrik (**power grid**), sistem air publik, atau sistem komunikasi pertahanan militer, publik dapat menghadapi bahaya fisik.

### 4.4 Dampak Tambahan dari Video Pengantar

Mitigasi serangan membutuhkan biaya dan waktu organisasi, di luar kerugian akibat kebocoran informasi rahasia dan kerusakan reputasi.

## 5. Digital Forensics and Incident Response (DFIR)

### 5.1 Peran Chaos Specialist

Narasumber video, Matt, adalah **chaos specialist** di Google (karyawan Google boleh memilih sendiri gelar jabatannya). Pekerjaannya merencanakan penanganan segala hal yang mungkin salah, dan ketika hal itu terjadi, menyusun tim untuk memperbaikinya secepat mungkin.

Bidangnya disebut **DFIR (Digital Forensics and Incident Response)**. Daya tariknya sama dengan respons medis: hadir membantu orang pada hari terburuk mereka, saat mereka tidak tahu harus meminta bantuan ke mana lagi.

### 5.2 Latar Belakang Karier

Matt tidak berniat masuk dunia teknologi. Ia bermula sebagai lifeguard, lalu mengambil lisensi **EMT** dan sekolah pemadam kebakaran. Saat mengalami burnout, seorang teman lama dari era online gaming berbasis teks mengajaknya ke San Francisco untuk merintis startup, dan dari situ ia masuk ke teknologi.

### 5.3 Ragam Serangan yang Dihadapi Google

Google menghadapi hampir semua jenis serangan yang dialami perusahaan lain:

- **Ransomware**.
- Pencurian rahasia industri (**industrial secrets**).
- Pencarian informasi intelijen oleh negara lain.

Salah satu contoh menonjol adalah kampanye jangka panjang untuk memperoleh informasi tentang kerentanan software dari perusahaan teknologi. Pelaku membangun persona media sosial palsu seolah-olah **security researcher** yang sah, menjalin hubungan dengan peneliti keamanan lain, lalu menyusupkan malware pada saat yang tepat.

### 5.4 Kondisi Psikologis Saat Insiden

Diserang oleh **adversary** yang sudah membuat kemajuan sangat menekan. Reaksi awal biasanya rasa panik, disertai pertanyaan seperti berapa lama harus bekerja, apa saja yang sudah dilakukan penyerang, dan apa yang harus dilakukan.

Mantra yang dipegang Matt untuk menjaga fokus:

> *"As an incident responder, I am here to help."* (Sebagai incident responder, saya di sini untuk membantu.)

### 5.5 Framework 3Cs

Tiga hal terpenting untuk hasil insiden yang baik:

1. **Command**: harus ada satu orang yang secara afirmatif memimpin insiden.
2. **Control**: harus ada pihak yang mengendalikan semua orang yang terlibat agar selaras dan fokus pada misi.
3. **Communications**: yang terbesar dan terpenting dari ketiganya. Jika seseorang punya kontribusi untuk insiden, jangan langsung dikerjakan sendiri, tetapi komunikasikan dulu ke tim, misalnya usulan tindakan atau area tempat data tambahan mungkin ditemukan.

### 5.6 Saran untuk Calon Praktisi

Jika seseorang menginginkan karier di cybersecurity, kemungkinan besar ia layak berada di sana. Industri membutuhkan orang yang bersemangat, penasaran, suka bertanya, ingin membangun hal yang lebih baik, dan peduli membuat teknologi lebih aman bagi penggunanya.

## 6. Key Takeaways

Aktor jahat terus mencari cara mengeksploitasi sistem. Mereka mempelajari kerentanan baru saat muncul dan mencoba mengeksploitasi setiap celah.

Serangan **backdoor** dan **network interception** dipakai untuk mengakses informasi sensitif dan menimbulkan kerusakan serius, dengan dampak finansial, reputasi, dan potensi bahaya bagi publik. Analis keamanan perlu terus memperbarui pengetahuannya agar kemungkinan dan dampak serangan semacam ini berkurang. Mengamankan jaringan tidak pernah sepenting sekarang.

## 7. Glossary

- **Malicious hackers**: peretas yang menyerang jaringan dengan niat jahat.
- **Inherent vulnerabilities**: kerentanan bawaan yang dimiliki setiap jaringan.
- **Malware**: perangkat lunak berbahaya yang dipakai menyusup ke sistem (contoh: kasus Home Depot).
- **Spoofing**: salah satu metode penyusupan jaringan, disebut namanya saja di materi.
- **Packet sniffing**: menangkap dan memeriksa data in transit dengan hardware atau software.
- **Packet flooding**: serangan yang mengganggu operasional jaringan dengan membanjirinya.
- **Network interception attack**: serangan yang mencegat lalu lintas jaringan untuk mencuri atau mengubah data.
- **On-path attack**: bentuk interception, dibahas di bagian course berikutnya.
- **Replay attack**: bentuk interception, dibahas di bagian course berikutnya.
- **Backdoor**: celah yang melewati kontrol akses normal, ditinggalkan programmer atau dipasang penyerang.
- **Persistent access**: akses berkelanjutan yang dijaga penyerang setelah mengompromikan organisasi.
- **DoS (denial of service)**: serangan yang membanjiri jaringan atau server dengan traffic.
- **Ransomware**: serangan yang berujung permintaan pembayaran besar oleh pelaku.
- **Disgruntled employee**: karyawan tidak puas yang bisa menjadi pelaku ancaman.
- **Cyber warfare**: taktik perang siber yang dilawan lembaga pertahanan.
- **Settlement costs**: biaya penyelesaian hukum akibat tuntutan pelanggan.
- **Adversary**: pihak lawan yang melancarkan serangan.
- **DFIR**: Digital Forensics and Incident Response.
- **3Cs**: Command, Control, Communications.

## 8. Tools & Platform Rujukan

Materi room ini tidak menyebutkan tool, link, atau platform eksternal untuk dipakai analis, sehingga tidak ada rujukan yang bisa dicatat tanpa mengarang. Satu-satunya entitas bernama adalah Google dan Home Depot, yang muncul sebagai konteks narasumber dan studi kasus, bukan sebagai tool.

## 9. Catatan Ringkas untuk Ditulis Tangan

### Ancaman dan Motivasi

- Setiap jaringan — punya inherent vulnerabilities, bisa jadi target
- Motivasi — finansial, personal, politik
- Disgruntled employee — karyawan tidak puas, ingin rugikan organisasi
- Aktivis — tidak setuju nilai perusahaan
- Tugas analis — selalu waspada, mitigasi cepat

### Metode Serangan

- Malware — software berbahaya untuk menyusup
- Spoofing — metode penyusupan
- Packet sniffing — tangkap dan periksa data in transit
- Packet flooding — ganggu operasional jaringan
- DoS — banjiri jaringan atau server dengan traffic

### Network Interception

- Tujuan — curi informasi atau ganggu transmisi
- Bisa ubah data — sisipkan kode jahat, ubah pesan
- Contoh — transfer bank diganti ke rekening penyerang
- On-path attack — dibahas nanti
- Replay attack — dibahas nanti

### Backdoor

- Definisi — celah yang bypass kontrol akses normal
- Asal — ditinggalkan programmer atau admin untuk troubleshooting
- Atau dipasang penyerang setelah kompromi
- Tujuan penyerang — persistent access
- Analogi — pintu belakang gedung yang kurang diawasi
- Dampak — malware, DoS, curi data, ubah pengaturan keamanan

### Dampak Organisasi

- Finansial — operasi lumpuh, biaya perbaikan, ransomware, litigasi, settlement
- Reputasi — publik ragu, pelanggan pindah ke kompetitor
- Public safety — jaringan pemerintah, power grid, air, militer
- Mitigasi — butuh uang dan waktu

### Home Depot 2014

- Pelaku — kelompok peretas
- Metode — malware di server
- Korban — 56 juta+ pelanggan
- Data — kartu kredit dan debit
- Serangan baru berhenti setelah data tercuri

### DFIR dan Incident Response

- DFIR — Digital Forensics and Incident Response
- Chaos specialist — rencanakan kegagalan, bentuk tim perbaikan
- Serangan di Google — ransomware, industrial secrets, intelijen negara
- Kasus persona palsu — security researcher palsu di medsos, lalu malware
- Mantra — "I am here to help"
- Command — satu pemimpin tegas
- Control — semua selaras dan fokus
- Communications — paling penting, komunikasikan dulu sebelum bertindak

### Takeaways

- Penyerang eksploitasi setiap celah baru
- Analis harus terus belajar
- Interception dan backdoor — dampak finansial, reputasi, keselamatan publik
