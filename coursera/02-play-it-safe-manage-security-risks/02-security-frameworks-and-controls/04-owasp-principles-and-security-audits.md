# Resume Room: OWASP Security Principles dan Security Audits (11 Oktober 2026)

Resume ini mencakup seluruh materi room: prinsip keamanan OWASP, security audit (jenis, elemen, checklist), perjalanan karier di cybersecurity, kuis latihan, dan portfolio activity Botium Toys. Room ini bersifat teori/konsep, sehingga tidak ada command atau switch yang dibahas.


## 1. Prinsip Keamanan OWASP

**OWASP** (Open Web Application Security Project, kini bernama Open Worldwide Application Security Project) menyediakan prinsip keamanan yang dipakai analis cybersecurity bersama framework NIST dan CIA triad untuk meminimalkan ancaman dan risiko. Dalam pekerjaan sehari-hari, prinsip ini tertanam pada tugas seperti menganalisis log, memantau dashboard **SIEM**, atau memakai **vulnerability scanner**.

### 1.1 Enam Prinsip Dasar

**Minimize attack surface area**. Attack surface adalah seluruh potensi kerentanan yang dapat dieksploitasi threat actor, termasuk **attack vector** (jalur penyerang menembus pertahanan) seperti phishing email dan password lemah. Cara meminimalkannya: menonaktifkan fitur software, membatasi akses ke aset tertentu, dan menetapkan persyaratan password yang lebih kompleks.

**Principle of least privilege**. Pengguna hanya mendapat akses minimum yang diperlukan untuk tugas sehari-hari. Tujuannya mengurangi dampak jika terjadi breach. Contoh: analis pemula boleh mengakses data log tetapi tidak boleh mengubah user permission, sehingga kredensial yang dikompromikan tetap hanya memberi akses terbatas.

**Defense in depth**. Organisasi memakai beberapa security control yang menangani risiko dengan cara berbeda, sehingga penyerang harus menembus banyak lapisan. Contoh kontrol: **MFA** (langkah tambahan selain username dan password), firewall, intrusion detection system, dan pengaturan permission.

**Separation of duties**. Tindakan kritis tidak boleh bergantung pada satu orang; tidak ada individu yang memegang privilege sebanyak itu sehingga bisa menyalahgunakan sistem atau melakukan fraud. Tiap orang tetap mengikuti least privilege. Contoh: orang yang menandatangani cek gaji tidak boleh sama dengan orang yang menyiapkannya.

**Keep security simple**. Hindari solusi keamanan yang rumit tanpa alasan, karena menjadi sulit dikelola dan menghambat kerja kolaboratif.

**Fix security issues correctly**. Saat insiden terjadi: identifikasi root cause dengan cepat, kendalikan dampaknya, identifikasi dan perbaiki kerentanan, lalu lakukan pengujian untuk memastikan remediasi berhasil. Contoh: password wifi organisasi yang lemah diperbaiki dengan password policy yang lebih ketat.

### 1.2 Empat Prinsip Tambahan

**Establish secure defaults**. Kondisi paling aman dari sebuah aplikasi adalah kondisi default bagi pengguna; membuat aplikasi menjadi tidak aman seharusnya butuh usaha ekstra.

**Fail securely**. Saat sebuah kontrol gagal atau berhenti, kontrol itu beralih ke opsi paling aman. Contoh: firewall yang gagal menutup semua koneksi dan memblokir koneksi baru, bukan menerima semuanya.

**Don't trust services**. Partner pihak ketiga sering punya kebijakan keamanan berbeda, jadi organisasi tidak boleh menganggap sistem mereka aman. Contoh: maskapai harus memverifikasi saldo reward points dari vendor sebelum menampilkannya ke pelanggan.

**Avoid security by obscurity**. Keamanan sistem penting tidak boleh bergantung pada menyembunyikan detail. Menurut contoh OWASP Mobile Top 10 (2016), keamanan aplikasi tidak boleh bergantung pada kerahasiaan source code, melainkan pada password policy yang wajar, defense in depth, batasan transaksi bisnis, arsitektur jaringan yang solid, serta kontrol fraud dan audit.

### 1.3 Penerapan oleh Analis Pemula

Analis pemula memakai prinsip ini untuk mendorong praktik pengembangan yang aman, sehingga risiko berkurang bagi perusahaan maupun pengguna.


## 2. Security Audit

### 2.1 Definisi dan Tujuan

**Security audit** adalah tinjauan independen terhadap security controls, policies, dan procedures organisasi terhadap seperangkat ekspektasi. Audit juga dipakai menilai security control yang telah ditetapkan (**security control** = safeguard yang dirancang mengurangi risiko keamanan tertentu).

Audit memastikan pemeriksaan keamanan benar-benar dilakukan (misalnya monitoring harian dashboard SIEM) untuk mengidentifikasi threat, risk, dan vulnerability, serta memastikan ada proses remediasi bila ditemukan masalah.

**Goal** audit: memastikan praktik IT organisasi memenuhi standar industri dan standar organisasi. **Objective**: mengidentifikasi dan menangani area yang perlu remediasi dan pertumbuhan, dengan menunjukkan kegagalan saat ini serta menyusun rencana perbaikannya. Audit wajib dilakukan untuk menjaga data dan menghindari penalti serta denda dari lembaga pemerintah; frekuensinya bergantung pada hukum lokal dan federal compliance regulations.

### 2.2 Jenis Audit

- **External audit**: dilakukan oleh organisasi dari luar.
- **Internal audit**: dilakukan tim internal (compliance officer, security manager, anggota tim keamanan lain). Jenis inilah yang biasanya melibatkan analis pemula. Manfaatnya: meningkatkan security posture, menghindari denda akibat ketidakpatuhan, mengidentifikasi risiko, menilai kontrol, dan memperbaiki compliance issue.

### 2.3 Kriteria Audit

- **Kriteria internal**: kebijakan, prosedur, dan best practice yang sudah ditetapkan.
- **Kriteria eksternal**: regulatory compliance, hukum, dan federal regulations.

### 2.4 Faktor yang Memengaruhi Jenis Audit

- Jenis industri
- Ukuran organisasi
- Keterikatan dengan regulasi pemerintah yang berlaku
- Lokasi geografis bisnis
- Keputusan bisnis untuk mematuhi regulatory compliance tertentu

### 2.5 Peran Framework dan Control

Framework seperti **NIST CSF** dan seri **ISO 27000** membantu organisasi bersiap menghadapi regulatory compliance audit, sehingga menghemat waktu pada audit eksternal maupun internal. Dipakai bersama control, framework mendukung keselarasan dengan persyaratan dan standar compliance.

Tiga kategori control yang ditinjau saat audit: **administrative/managerial**, **technical**, dan **physical**.


## 3. Elemen Internal Security Audit

Lima elemen umum: (1) menetapkan scope dan goals, (2) risk assessment, (3) controls assessment, (4) menilai compliance, (5) mengomunikasikan hasil ke stakeholder. Dua elemen pertama adalah tahap **perencanaan audit**.

### 3.1 Scope dan Goals

**Scope** adalah kriteria spesifik audit: mengidentifikasi people, assets, policies, procedures, dan technologies yang dapat memengaruhi security posture. **Goals** adalah gambaran tujuan keamanan yang ingin dicapai organisasi untuk memperbaiki security posture.

Biasanya ditetapkan anggota senior dan stakeholder; analis pemula meninjau dan memahaminya agar dapat mengerjakan elemen lain. Contoh scope: menilai user permission, mengidentifikasi control/policy/procedure yang ada, mendata teknologi yang digunakan. Contoh goals: menerapkan fungsi inti framework seperti NIST CSF, menetapkan kebijakan dan prosedur untuk compliance, memperkuat kontrol sistem.

### 3.2 Risk Assessment

**Risk assessment** berfokus mengidentifikasi threats, risks, dan vulnerabilities untuk menentukan langkah keamanan yang perlu diterapkan dan dipantau. Umumnya dibuat manajer atau stakeholder; analis pemula dapat diminta menganalisis detailnya untuk menentukan kontrol dan regulasi compliance yang dibutuhkan.

Contoh temuan: kontrol, proses, dan prosedur tidak memadai; manajemen aset fisik dan digital (termasuk peralatan karyawan) buruk; peralatan penyimpan data tidak diamankan; akses ke informasi privat di jaringan internal perlu kontrol lebih kuat. Dalam audit, risk assessment juga mengevaluasi risiko terkait budget, kontrol, proses internal, dan standar eksternal.

### 3.3 Controls Assessment

Sebelum mengerjakan tiga elemen terakhir, tinjau scope, goals, dan risk assessment lalu tanyakan: apa yang ingin dicapai audit, aset mana yang paling berisiko, apakah kontrol saat ini cukup, dan kontrol serta regulasi apa yang perlu diterapkan jika belum.

**Controls assessment** adalah peninjauan mendalam terhadap aset organisasi, lalu evaluasi risiko terhadap aset tersebut, untuk memastikan kontrol dan proses internal efektif. Analis pemula dapat ditugaskan mengklasifikasikan kontrol:

- **Administrative controls**: komponen manusia; kebijakan dan prosedur pengelolaan data, mis. password policy.
- **Technical controls**: solusi hardware/software pelindung aset, mis. **IDS** dan encryption.
- **Physical controls**: langkah mencegah akses fisik ke aset, mis. kamera pengawas dan kunci.

### 3.4 Menilai Compliance

**Compliance regulations** adalah hukum yang wajib dipatuhi organisasi agar data privat tetap aman. Contoh: organisasi yang berbisnis di Uni Eropa dan menerima pembayaran kartu kredit harus mematuhi **GDPR** dan **PCI DSS**.

### 3.5 Mengomunikasikan Hasil

Setelah audit selesai, hasil dan rekomendasi disampaikan ke stakeholder. Isinya:

- Ringkasan scope dan goals audit
- Daftar risiko beserta urgensi penanganannya
- Regulasi compliance yang perlu dipatuhi
- Rekomendasi untuk meningkatkan security posture

Daftar pertanyaan untuk dijawab stakeholder bukan bagian dari komunikasi hasil. Contoh nyata: internal password audit menemukan banyak password lemah, lalu tim compliance memberlakukan password policy yang lebih ketat.


## 4. Audit Checklist

Audit checklist dibuat sebelum audit dilakukan, dengan lima area fokus.

**Identify the scope of the audit**. Daftar aset yang dinilai (mis. firewall terkonfigurasi benar, PII aman, aset fisik terkunci); catat bagaimana audit membantu mencapai goals; tentukan frekuensi audit; evaluasi policies, protocols, dan procedures agar berjalan sesuai rencana dan diterapkan karyawan.

**Complete a risk assessment**. Evaluasi risiko terkait budget, kontrol, proses internal, dan standar eksternal (regulasi).

**Conduct the audit**. Nilai keamanan aset yang tercantum dalam scope.

**Create a mitigation plan**. Strategi untuk menurunkan tingkat risiko serta potensi biaya, penalti, atau masalah lain yang merugikan security posture.

**Communicate results to stakeholders**. Laporan detail temuan, perbaikan yang disarankan untuk menurunkan risiko, serta regulasi dan standar compliance yang harus dipenuhi.


## 5. Studi Kasus: Botium Toys

### 5.1 Skenario

**Botium Toys** adalah bisnis mainan kecil di AS dengan satu lokasi fisik (kantor, toko, gudang). Kehadiran online-nya tumbuh ke pelanggan domestik dan luar negeri, sehingga departemen IT ditekan untuk mendukung pasar global. Manajer IT memutuskan perlu internal IT audit untuk menjaga compliance dan operasional tanpa rencana yang jelas, mengamankan infrastruktur, memitigasi risiko terhadap aset kritis, serta mematuhi regulasi pemrosesan dan penerimaan pembayaran online serta bisnis di Uni Eropa.

Manajer memulai dengan menerapkan NIST CSF, menetapkan scope dan goals, mendata aset yang dikelola IT, dan menyelesaikan risk assessment. Goal audit: memberi gambaran risiko dan/atau denda yang mungkin dihadapi akibat security posture saat ini.

### 5.2 Tugas Analis

1. Tinjau scope, goals, dan risk assessment report, dengan fokus pada aset yang dikelola IT dan poin "Additional comments" di bagian risk assessment.
2. Pertimbangkan informasi tersebut menggunakan dokumen Control Categories.
3. Isi Controls and compliance checklist dengan "yes" atau "no" pada setiap kontrol dan compliance best practice (bagian recommendations opsional).

Hasil disimpan sebagai salinan untuk portofolio. Penilaian mandiri berisi 5 poin: meninjau report, mempertimbangkan risiko berdasarkan kontrol dan best practice yang ada atau belum ada, meninjau control categories, mengisi yes/no untuk tiap kontrol, dan untuk tiap compliance best practice. Batas lulus 80% (4 dari 5 poin).

### 5.3 Exemplar

Exemplar hanyalah salah satu cara menyelesaikan activity. Yang terpenting: hasil kerja menunjukkan kemampuan memilih kontrol yang perlu atau tidak perlu diterapkan Botium Toys, serta compliance best practice yang harus dipatuhi untuk memitigasi risiko dan menghindari denda.


## 6. Perjalanan Karier di Cybersecurity

Pembicara (Wajih, security engineer di Google, departemen digital forensics) menegaskan bahwa latar belakang cybersecurity tidak wajib. Ia berasal dari pekerjaan di water park (mesin snow cone) dan bioskop (penjual popcorn), serta awalnya mahasiswa jurusan biologi sebelum tertarik pada startup cybersecurity.

**Tetap up to date**. Gunakan forum online seperti **Medium** dengan filter tag (mis. cybersecurity, cloud security). Untuk networking, hadiri conference.

**Saran masuk bidang ini**. Jangan overwhelmed oleh banyaknya spesialisasi; prioritaskan topik yang paling relevan. Abaikan anggapan bahwa bidang ini terlalu sulit; kuasai fundamental dan tetap persisten. Ada kekurangan profesional di bidang ini.


## 7. Poin Kuis Latihan

- Menonaktifkan fitur software untuk mengurangi kerentanan: **minimize the attack surface**.
- Tinjauan kontrol, kebijakan, dan prosedur terhadap ekspektasi: **security audit**.
- Memeriksa aset lalu mengevaluasi risiko untuk memastikan kontrol efektif: **controls assessment**.
- Isi komunikasi hasil audit: ringkasan scope dan goals, daftar risiko dan compliance requirements, serta rekomendasi peningkatan security posture.


## 8. Glossary

- **Attack surface**: seluruh potensi kerentanan yang dapat dieksploitasi threat actor.
- **Attack vector**: jalur yang dipakai penyerang menembus pertahanan.
- **Audit checklist**: daftar area fokus yang disusun sebelum audit.
- **Compliance regulations**: hukum yang wajib dipatuhi untuk menjaga keamanan data privat.
- **Controls assessment**: peninjauan aset dan evaluasi risiko untuk memastikan kontrol efektif.
- **Defense in depth**: beberapa lapisan security control yang menangani risiko secara berbeda.
- **External audit**: audit oleh organisasi luar.
- **GDPR**: regulasi perlindungan data untuk bisnis di Uni Eropa.
- **IDS**: intrusion detection system, contoh technical control.
- **Internal audit**: audit oleh tim internal organisasi.
- **ISO 27000**: seri standar internasional keamanan informasi.
- **MFA**: autentikasi dengan langkah tambahan selain username dan password.
- **Mitigation plan**: strategi menurunkan risiko dan potensi biaya atau penalti.
- **NIST CSF**: National Institute of Standards and Technology Cybersecurity Framework.
- **PCI DSS**: Payment Card Industry Data Security Standard, untuk pembayaran kartu kredit.
- **Remediasi**: proses memperbaiki masalah keamanan yang ditemukan.
- **Risk assessment**: identifikasi threats, risks, dan vulnerabilities.
- **Root cause**: penyebab dasar sebuah insiden.
- **Scope**: kriteria spesifik audit (people, assets, policies, procedures, technologies).
- **Security audit**: tinjauan kontrol, kebijakan, dan prosedur terhadap ekspektasi.
- **Security control**: safeguard untuk mengurangi risiko keamanan tertentu.
- **Security posture**: kondisi keamanan organisasi secara keseluruhan.
- **SIEM**: security information and event management, dashboard pemantauan keamanan.


## 9. Tools & Platform Rujukan

Tidak ada URL eksplisit pada materi; hanya nama yang disebut.

- **Medium**: forum online untuk riset tren keamanan dengan filter tag.
- **Vulnerability scanner**: alat pemindai kerentanan, dipakai dalam tugas harian analis.
- **SIEM dashboard**: pemantauan informasi dan event keamanan secara harian.
- **OWASP Mobile Top 10 (2016)**: sumber contoh prinsip avoid security by obscurity.
- **Template Control categories**: dokumen kategori kontrol (Google Docs atau DOCX).
- **Template Controls and compliance checklist**: dokumen yang diisi saat audit Botium Toys.
- **Assessment and Auditing Resources**: sumber bacaan tambahan tentang audit.
- **IT Disaster Recovery Plan**: sumber bacaan tambahan.
- **Conference cybersecurity**: sarana networking dengan profesional.


## 10. Catatan Ringkas untuk Ditulis Tangan

### OWASP: 6 Prinsip Dasar
- Minimize attack surface — kurangi kerentanan (matikan fitur, batasi akses, password kuat)
- Attack vector — jalur serangan, mis. phishing, password lemah
- Least privilege — akses minimum sesuai tugas, batasi dampak breach
- Defense in depth — banyak lapisan kontrol (MFA, firewall, IDS, permission)
- Separation of duties — tak ada satu orang pegang privilege besar (contoh: cek gaji)
- Keep security simple — hindari kerumitan, sulit dikelola
- Fix security issues correctly — root cause, kendalikan, perbaiki, uji ulang

### OWASP: 4 Prinsip Tambahan
- Establish secure defaults — default = paling aman
- Fail securely — gagal = beralih ke opsi aman (firewall tutup semua koneksi)
- Don't trust services — jangan percaya partner pihak ketiga, verifikasi (contoh: reward points maskapai)
- Avoid security by obscurity — jangan andalkan kerahasiaan source code

### Security Audit
- Security audit — tinjauan kontrol, kebijakan, prosedur vs ekspektasi
- Goal — praktik IT penuhi standar industri dan organisasi
- Objective — identifikasi dan tangani area remediasi
- External audit — oleh pihak luar
- Internal audit — oleh tim internal (compliance officer, security manager)
- Kriteria internal — kebijakan, prosedur, best practice
- Kriteria eksternal — regulasi, hukum, federal regulations
- Faktor audit — industri, ukuran, regulasi pemerintah, lokasi, keputusan compliance
- NIST CSF dan ISO 27000 — framework pendukung persiapan audit

### Elemen Internal Audit
- Scope — people, assets, policies, procedures, technologies
- Goals — tujuan keamanan yang ingin dicapai
- Risk assessment — identifikasi threats, risks, vulnerabilities
- Controls assessment — tinjau aset, evaluasi risiko, pastikan kontrol efektif
- Administrative control — kebijakan dan prosedur (password policy)
- Technical control — hardware/software (IDS, encryption)
- Physical control — cegah akses fisik (CCTV, kunci)
- Compliance — GDPR (Uni Eropa), PCI DSS (kartu kredit)
- Komunikasi hasil — scope dan goals, risiko dan urgensi, regulasi, rekomendasi

### Audit Checklist (5 Area)
- Identify scope — aset, goals, frekuensi, evaluasi kebijakan
- Risk assessment — budget, kontrol, proses internal, standar eksternal
- Conduct audit — nilai aset dalam scope
- Mitigation plan — turunkan risiko, biaya, penalti
- Communicate results — laporan temuan, perbaikan, regulasi

### Botium Toys
- Bisnis mainan AS, satu lokasi fisik, online global
- Manajer IT — terapkan NIST CSF, scope, goals, risk assessment
- Tugas — review report, control categories, isi checklist yes/no
- Lulus — minimal 80% (4/5)

### Karier Cybersecurity
- Tidak wajib latar belakang khusus
- Medium — filter tag untuk tren
- Conference — networking
- Jangan overwhelmed spesialisasi, prioritaskan
- Kuasai fundamental, persisten
