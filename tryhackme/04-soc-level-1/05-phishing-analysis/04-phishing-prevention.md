# Resume Materi: Phishing Prevention

**Tanggal:** 29 Agustus 2026
**Room:** Phishing Prevention (TryHackMe)

---

## 1. Pendahuluan

Room ini membahas dua aspek utama pertahanan terhadap phishing: mekanisme autentikasi email pada level protokol (SPF, DKIM, DMARC, S/MIME), analisis traffic SMTP menggunakan Wireshark, serta kontrol teknis dan edukasi pengguna yang diterapkan organisasi untuk mencegah dan mendeteksi phishing.

Phishing sendiri, menurut MITRE ATT&CK Framework, salah satu variannya (Phishing for Information) didefinisikan sebagai upaya menipu target agar membocorkan informasi.

---

## 2. Autentikasi Email

### 2.1 Sender Policy Framework (SPF)

SPF digunakan untuk mengautentikasi pengirim sebuah email. Dengan SPF record, penyedia layanan internet dapat memverifikasi bahwa sebuah mail server berwenang mengirim email atas nama domain tertentu. SPF record berbentuk **DNS TXT record** yang berisi daftar IP address atau domain yang diizinkan mengirim email atas nama domain tersebut.

**Alur kerja SPF:**

Ketika email dikirim, mail server penerima melakukan lookup ke DNS server milik domain pengirim untuk mengambil SPF record yang telah dipublikasikan. Hasil pengecekan (pass/fail) menentukan apakah email diteruskan ke inbox atau ditolak sebagai spam.

**Format SPF record:**

```
v=spf1 ip4:127.0.0.1 include:_spf.google.com -all
```

- `v=spf1` — menandakan awal dari SPF record
- `ip4:127.0.0.1` — menentukan IP address yang boleh mengirim mail (IPv4)
- `include:_spf.google.com` — menentukan domain lain yang diizinkan mengirim mail atas nama domain ini
- `-all` — email dari sumber yang tidak terdaftar akan ditolak (hard fail)

**Hasil verifikasi SPF dan tindakan yang diambil:**

| Hasil Verifikasi | Tindakan |
|---|---|
| Pass, Neutral, None | Accept — email diterima dan diproses |
| SoftFail, PermError | Flag — ditandai mencurigakan, tetap diterima |
| Fail, TempError | Reject — email langsung ditolak |

**Tools terkait:** SPF Surveyor (dmarcian) untuk visualisasi DNS record; Google Admin Toolbox Messageheader untuk menganalisis hasil SPF dari full header sebuah email.

### 2.2 DomainKeys Identified Mail (DKIM)

DKIM adalah standar terbuka untuk autentikasi email yang juga dipakai untuk keperluan alignment DMARC. DKIM record juga tersimpan di DNS, tapi lebih kompleks dibanding SPF. Keunggulan utamanya: **DKIM dapat bertahan meski email di-forward**, menjadikannya fondasi keamanan email yang lebih kuat dibanding SPF saja.

**Alur kerja DKIM:**

Domain pemilik mempublikasikan public DKIM key ke DNS server. Saat email dikirim, mail server pengirim menggunakan **private key** untuk menandatangani (sign) email secara digital. Server penerima mengambil **public key** dari DKIM record domain tersebut untuk mencocokkan tanda tangan. Jika cocok, email dianggap otentik; jika tidak, email dapat di-flag atau ditolak.

**Format DKIM record:**

```
v=DKIM1; k=rsa; p=<public_key>
```

- `v=DKIM1` — versi DKIM yang digunakan (opsional)
- `k=rsa` — tipe algoritma enkripsi kunci, RSA adalah standar
- `p=` — public key yang dicocokkan dengan private key untuk verifikasi tanda tangan

**Hasil verifikasi DKIM yang umum ditemui:** `permerror` menandakan kegagalan permanen dalam verifikasi DKIM, penyebabnya bisa berupa tanda tangan tidak valid, DNS record hilang/salah, modifikasi oleh forwarding server, atau kesalahan konfigurasi DKIM. Contoh yang ditemukan pada studi kasus: `dkim=permerror (no key for signature)`, artinya public key untuk memverifikasi tanda tangan tidak ditemukan.

**Tools terkait:** DKIM Record Checker dan Validator (dmarcian).

### 2.3 Domain-Based Message Authentication, Reporting, and Conformance (DMARC)

DMARC menggunakan konsep **alignment** untuk mengaitkan hasil dari SPF dan DKIM dengan konten email (khususnya domain pada header "From"). Jika alignment gagal, DMARC menginstruksikan server penerima cara menangani email tersebut berdasarkan policy yang ditetapkan di record.

**Format DMARC record:**

```
v=DMARC1; p=quarantine; rua=mailto:postmaster@website.com
```

- `v=DMARC1` — versi DMARC (wajib)
- `p=` — kebijakan DMARC (policy tag)
- `rua=mailto:...` — tag opsional, alamat tujuan pengiriman laporan agregat (aggregate report)

**Tiga jenis DMARC policy (`p=`):**

- `none` — tidak ada tindakan khusus, email tetap diproses normal (hanya monitoring)
- `quarantine` — email yang gagal alignment dipindahkan ke folder spam
- `reject` — memberikan proteksi paling ketat; email yang gagal DMARC check **langsung ditolak**

**Tools terkait:** Domain Checker (dmarcian), memeriksa DMARC, SPF, dan DKIM sekaligus untuk satu domain; DMARC Inspector untuk memahami dan memperbaiki error spesifik.

### 2.4 Secure/Multipurpose Internet Mail Extensions (S/MIME)

S/MIME adalah protokol standar untuk mengirim pesan yang ditandatangani secara digital dan dienkripsi, berbasis **kriptografi kunci publik** (private key tidak pernah dibagikan, public key didistribusikan terbuka).

**Dua komponen utama S/MIME:**

**Digital Signature** — pengirim menandatangani pesan dengan private key, penerima memverifikasi menggunakan public key pengirim. Fitur keamanan yang diberikan:
- Authentication: mengonfirmasi identitas pengirim melalui sertifikat digital
- Non-repudiation: pengirim tidak dapat menyangkal telah mengirim pesan
- Data Integrity: mendeteksi perubahan pada pesan setelah ditandatangani

**Encryption** — pengirim mengenkripsi pesan menggunakan public key milik penerima, sehingga hanya penerima yang dapat mendekripsi dengan private key miliknya. Fitur keamanan yang diberikan:
- Confidentiality: konten tetap privat dan hanya bisa dibaca oleh penerima yang dituju

**Alur kerja singkat (contoh Bob mengirim ke Mary):** Bob membuat sertifikat digital, menandatangani pesan dengan private key-nya, membagikan public key-nya ke Mary, lalu mengenkripsi pesan menggunakan public key milik Mary. Mary memverifikasi tanda tangan dengan public key Bob, dan mendekripsi pesan dengan private key miliknya sendiri. Setelah pertukaran ini, kedua pihak saling memiliki sertifikat untuk korespondensi berikutnya.

---

## 3. Analisis Traffic SMTP dengan Wireshark

### 3.1 Filter Dasar Wireshark untuk SMTP

- `smtp` — menampilkan seluruh packet berprotokol SMTP (command, response, data email, dsb)
- `smtp.response.code` — hanya menampilkan packet yang memiliki field response code (yaitu packet respons dari server)
- `smtp.response.code == <kode>` — mempersempit ke response code spesifik, misal `220`, `552`
- `<protokol> contains "<teks>"` — mencari string tertentu di dalam isi packet suatu protokol, misal `smtp contains "spamhaus"`
- `frame.number == <n>` — menampilkan packet berdasarkan nomor urut frame di keseluruhan capture (field umum Wireshark, berlaku untuk semua protokol, bukan spesifik SMTP)
- `imf` — menampilkan packet berformat **Internet Message Format** (header lengkap email: From, To, Subject, X-Mailer, dsb beserta body-nya)
- `imf contains "<teks>"` — mempersempit hasil filter `imf` untuk mencari teks tertentu di dalam header/body email, misalnya nama file attachment

Semakin spesifik field yang disebutkan dalam filter, semakin sempit hasil yang ditampilkan karena Wireshark hanya menampilkan packet yang benar-benar memiliki field tersebut terisi.

### 3.2 SMTP Response Code

Response code SMTP terdiri dari tiga digit yang menandakan status pemrosesan sebuah perintah/pesan oleh server. Contoh yang dijumpai pada studi kasus:

- **220** — Service ready (respons standar saat koneksi/service siap)
- **421** — 4.4.2 Connection read error
- **552** — 5.7.0, pesan diblokir karena kontennya berpotensi menimbulkan masalah keamanan (potential security issue)
- **553** — Requested action not taken: mailbox name not allowed (dalam studi kasus, penyebabnya email diblokir oleh layanan reputasi seperti spamhaus.org)

Detail response code lengkap dapat dilihat pada field **"Response code:"** di panel detail Wireshark, yang menampilkan deskripsi standar beserta angka kodenya dalam satu baris, misalnya: "Requested action not taken: mailbox name not allowed (553)".

### 3.3 Inspeksi Isi Email dan Attachment

**Internet Message Format (IMF)** memuat header lengkap sebuah email (From, To, Subject, Date, Content-Type, X-Mailer, dsb) beserta body/isi pesan. Header ini bisa dianalisis langsung dari packet SMTP yang membawa data email (data fragment yang telah di-reassemble oleh Wireshark).

**Header penting yang dapat ditemukan:**

- `X-Mailer` — mencantumkan nama dan versi software/email client yang digunakan untuk mengirim email tersebut (contoh: Microsoft Outlook Express 6.00.2600.0000). Header serupa yang kadang muncul: `User-Agent`.
- `Content-Type` — menentukan jenis konten, misalnya `multipart/mixed` untuk email dengan lampiran, atau `application/octet-stream` untuk data biner/attachment.
- `Content-Transfer-Encoding` — menentukan metode encoding data lampiran, contoh yang ditemukan: `base64`.
- `Content-Disposition` — menentukan bagaimana konten ditampilkan, biasanya `attachment` diikuti `filename="..."` yang menyebutkan nama file lampiran.

**Cara menganalisis attachment mencurigakan di Wireshark:**

Setelah menemukan packet yang relevan (misalnya lewat filter `imf contains "<nama_file>"`), expand bagian **"Internet Message Format"** atau **"MIME Multipart Media Encapsulation"** di panel detail untuk melihat struktur setiap bagian email (multipart), termasuk Content-Type, Content-Transfer-Encoding, dan Content-Disposition dari masing-masing lampiran. Alternatif lain: klik kanan packet lalu pilih **Follow > TCP Stream**, atau gunakan tab **"Reassembled SMTP"** yang muncul di bagian bawah panel, untuk melihat keseluruhan isi email yang sudah digabung dalam bentuk teks mentah.

Ekstensi file lampiran seperti `.scr` (Windows Screen Saver executable) merupakan indikator umum lampiran berbahaya, karena format ini sering disalahgunakan untuk menyamarkan file executable/malware.

---

## 4. Kontrol Teknis dan Edukasi Anti-Phishing

### 4.1 Technical Defenses

Kontrol teknis diterapkan untuk mendeteksi dan memblokir pesan phishing sebelum sampai ke pengguna:

- **Email Filtering** — filter berdasarkan reputasi IP dan domain, memungkinkan pemblokiran atau karantina pesan mencurigakan
- **Secure Email Gateways (SEGs)** — memindai pesan untuk mendeteksi impersonasi, spoofing, dan teknik phishing lain yang mungkin terlewat filter lain
- **Link Rewriting** — mengganti URL mencurigakan/tidak dikenal dengan URL yang sudah di-redirect dan diamankan, memberi waktu sistem untuk memindai dan memverifikasi link
- **Sandboxing** — mengisolasi dan menguji file/link mencurigakan dalam lingkungan virtual terisolasi untuk mengamati perilakunya secara aman, tanpa risiko infeksi pada sistem produksi

### 4.2 User-Facing Tools & Training

Meski pertahanan teknis sudah kuat, sebagian pesan phishing tetap akan lolos ke pengguna, sehingga edukasi dan indikator visual tetap diperlukan:

- **Trust & Warning Indicators** — banner visual pada platform email, misalnya "External Sender" atau "Suspicious Link", untuk menandai tingkat kepercayaan sebuah pesan
- **Phishing Reporting** — opsi pelaporan cepat di dalam email untuk melaporkan pesan mencurigakan
- **User Awareness Training** — pelatihan karyawan mengenali upaya phishing, taktik social engineering, dan praktik email yang aman
- **Phishing Simulation Exercises** — kampanye phishing terkontrol untuk menguji dan memperkuat efektivitas pelatihan karyawan

**Contoh skenario simulasi phishing:** pengguna mengklik link dalam simulasi dan mendapat notifikasi edukatif beserta tiga aturan keamanan dasar — (1) selalu berhenti, lihat, dan pikir sebelum klik; (2) kenali red flag serangan phishing; (3) verifikasi pesan mencurigakan melalui medium komunikasi lain sebelum bertindak. Email juga dapat menampilkan banner peringatan otomatis, misalnya memperingatkan bahwa domain pengirim baru aktif dalam waktu singkat, atau bahwa display name pengirim menyerupai nama pengguna internal organisasi padahal berasal dari domain eksternal.

---

## 5. Glossary

- **SPF (Sender Policy Framework)** — mekanisme autentikasi email berbasis DNS TXT record yang mendaftar IP/domain yang berwenang mengirim atas nama sebuah domain
- **DKIM (DomainKeys Identified Mail)** — mekanisme autentikasi email berbasis tanda tangan digital menggunakan pasangan private/public key, tahan terhadap forwarding
- **DMARC (Domain-Based Message Authentication, Reporting, and Conformance)** — kebijakan yang mengaitkan hasil SPF dan DKIM (alignment) untuk menentukan tindakan terhadap email yang gagal autentikasi
- **S/MIME** — protokol pengiriman email yang ditandatangani secara digital dan/atau dienkripsi berbasis kriptografi kunci publik
- **Alignment** — kecocokan antara domain pengirim pada header "From" dengan domain yang diverifikasi oleh SPF/DKIM
- **SoftFail / PermError / TempError / Fail** — berbagai kategori hasil verifikasi SPF yang menentukan apakah email diterima, ditandai, atau ditolak
- **Response Code** — kode tiga digit dalam protokol SMTP yang menandakan status pemrosesan sebuah perintah atau pesan oleh server
- **IMF (Internet Message Format)** — format standar header dan body sebuah pesan email
- **X-Mailer** — header email opsional yang mencantumkan nama/versi software pengirim email
- **Content-Transfer-Encoding** — header yang menentukan metode encoding data pada bagian email (misal base64)
- **Sandboxing** — teknik mengisolasi file/link mencurigakan dalam lingkungan virtual terpisah untuk analisis perilaku secara aman
- **Secure Email Gateway (SEG)** — sistem yang memindai email masuk untuk mendeteksi impersonasi, spoofing, dan phishing

---

## 6. Tools & Platform Rujukan

- **SPF Surveyor** (dmarcian) — memvisualisasikan dan memvalidasi sintaks SPF record sebuah domain
- **DKIM Record Checker & Validator** (dmarcian) — memeriksa validitas DKIM record sebuah domain
- **Domain Checker** (dmarcian) — memeriksa DMARC, SPF, dan DKIM sekaligus untuk satu domain dalam satu tampilan
- **DMARC Inspector** (dmarcian) — membantu memahami dan memperbaiki error spesifik pada DMARC record
- **Google Admin Toolbox — Messageheader** — menganalisis detail pengiriman email (termasuk hasil SPF/DKIM/DMARC) dari full header email
- **Wireshark** — tool analisis traffic jaringan, digunakan pada room ini untuk memeriksa traffic SMTP dan isi email dari file pcap
