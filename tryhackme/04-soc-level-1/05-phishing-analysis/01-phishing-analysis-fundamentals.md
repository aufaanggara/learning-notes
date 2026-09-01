# Resume: Phishing Analysis (Fundamentals) — TryHackMe
**Tanggal:** 1 September 2026

---

## 1. Anatomi Alamat Email

### 1.1 Sejarah Singkat

Email seperti yang dikenal sekarang mulai populer pada 1970-an di ARPANET oleh Ray Tomlinson, yang memperkenalkan simbol `@` untuk memisahkan user dari sistem tujuan.

### 1.2 Struktur Alamat Email

Sebuah alamat email terdiri dari tiga elemen:

- **Username** — mailbox pengguna yang mengidentifikasi penerima secara spesifik di email server.
- **Simbol `@`** — memisahkan username dari domain dan memberi tahu sistem ke mana email harus dirutekan.
- **Domain name** — menentukan mail server yang bertanggung jawab menerima pesan.

Contoh: pada `david@tryhackme.com`, `david` adalah username dan `tryhackme.com` adalah domain.

Analogi: domain seperti nama jalan/gedung, username seperti nomor mailbox spesifik di lokasi tersebut.

---

## 2. Protokol Pengiriman Email

### 2.1 Tiga Protokol Utama

- **SMTP (Simple Mail Transfer Protocol)** — mengirim email.
- **POP3 (Post Office Protocol)** — mengunduh email ke satu perangkat.
- **IMAP (Internet Message Access Protocol)** — menyinkronkan email di berbagai perangkat.

### 2.2 Perbandingan POP3 vs IMAP

**POP3:**
- Email diunduh dan disimpan hanya di satu perangkat.
- Pesan terkirim disimpan di perangkat pengirim.
- Email hanya bisa diakses dari perangkat tempat email diunduh.
- Email umumnya terhapus dari server setelah diunduh.

**IMAP:**
- Email disimpan di server, bisa diunduh ke banyak perangkat.
- Pesan terkirim disimpan di server.
- Sinkron di berbagai perangkat.
- Email tetap ada di server kecuali dihapus secara eksplisit.

### 2.3 Alur Perjalanan Email (Sender ke Receiver)

1. User mengirim email — email client pengirim mengirim pesan ke mail server via **SMTP**.
2. Mail server melakukan query **DNS** untuk mencari mail server domain penerima.
3. DNS merespons dengan alamat mail server penerima.
4. Email dikirim melalui internet ke server penerima.
5. Penerima memeriksa mailbox — email client penerima terhubung ke mail server mereka.
6. Email diambil — diunduh (**POP3**) atau disinkronkan (**IMAP**) ke perangkat penerima.

---

## 3. Struktur Email: Header dan Body

Email terdiri dari dua bagian utama:

- **Email header** — metadata tentang pesan (pengirim, server yang terlibat, dsb).
- **Email body** — konten pesan sebenarnya, berupa plain text atau HTML.

### 3.1 Komponen Utama Header

- **From** — alamat email pengirim.
- **To** — alamat email penerima.
- **Reply-To** — alamat tujuan balasan dikirim (opsional, tidak wajib ada).
- **Subject** — baris subjek email.
- **Date** — waktu dan tanggal email dikirim.

### 3.2 Melihat Message Source (Raw Header)

Message source menampilkan seluruh pesan mentah, termasuk semua field header dan body, mengungkap detail teknis yang tidak terlihat di tampilan inbox standar (misalnya originating IP address).

Cara akses di Thunderbird: menu **View → Message Source**, atau shortcut `ctrl + u`.

### 3.3 Field Header Teknis Penting

- **X-Originating-Ip** — field custom (prefix `X-`) yang menunjukkan alamat IP asli pengirim.
- **Received: from ...** — baris yang mencatat jalur pengiriman email antar server, juga bisa menunjukkan IP asal.
- **DKIM-Signature** — tanda tangan digital domain pengirim untuk verifikasi keaslian.
- **Return-Path** — alamat tujuan bounce/error message.
- **Received-SPF** — hasil pengecekan SPF (apakah IP pengirim diizinkan mengirim atas nama domain tersebut). Nilainya bisa `pass`, `fail`, dsb.
- **Authentication-Results** — hasil gabungan dari validasi autentikasi (DKIM, SPF, DMARC) yang dilakukan oleh mail server penerima. Formatnya mencantumkan nama mail server yang melakukan pengecekan tersebut.
- **Sender** — pengirim aktual di level teknis (bisa berbeda dari From jika email dikirim atas nama pihak lain).

### 3.4 Melihat HTML Source dari Body Email

Body HTML bisa diperiksa source code-nya untuk melihat bagaimana elemen (link, gambar) sebenarnya disusun — sering mengungkap ketidaksesuaian antara domain yang diklaim dengan domain sumber gambar/link sebenarnya (red flag phishing).

---

## 4. Attachment dalam Email

### 4.1 Header Terkait Attachment

- **Content-Type** — menunjukkan tipe file, misalnya `application/pdf`.
- **Content-Disposition** — menandakan file adalah attachment, mencantumkan nama filenya.
- **Content-Transfer-Encoding** — menunjukkan metode encoding file, biasanya `base64`.

### 4.2 Merekonstruksi Attachment dari Base64

Data attachment biasanya di-encode dalam base64 di dalam body email. Untuk mendapatkan file aslinya kembali, data base64 tersebut perlu di-decode menggunakan tool seperti CyberChef (operasi **"From Base64"**) atau PDF converter.

Poin penting: yang di-copy untuk didecode HANYA blok data base64-nya saja — bukan header Content-Type/Content-Disposition/Content-Transfer-Encoding, dan tidak termasuk boundary MIME (baris pemisah seperti `-----------------------xxxxxxxx`).

Hasil decode data biner (seperti PDF) akan terlihat seperti karakter acak/rusak jika ditampilkan sebagai teks di layar — ini normal. Solusinya adalah menyimpan hasil decode sebagai file dengan ekstensi yang sesuai (misalnya `.pdf`), lalu membukanya dengan software yang sesuai (PDF reader), bukan membacanya langsung sebagai teks.

Indikator bahwa hasil decode adalah PDF valid: struktur dengan penanda `%PDF-1.x` di awal dan `%%EOF` di akhir file.

---

## 5. Klasifikasi Email Berbahaya

- **Spam** — email massal tidak diminta ke banyak penerima. Bentuk yang lebih berbahaya disebut **malspam**.
- **Phishing** — email yang menyamar sebagai entitas terpercaya untuk menipu penerima membocorkan informasi sensitif.
- **Spear Phishing** — phishing yang ditargetkan pada individu/organisasi spesifik, memakai informasi personal.
- **Whaling** — spear phishing yang menargetkan eksekutif tingkat tinggi (CEO, CFO) untuk data sensitif atau akses finansial.
- **Smishing** — phishing melalui SMS/pesan teks, menargetkan pengguna mobile.
- **Vishing** — phishing melalui panggilan suara (voice call), menggunakan social engineering lewat telepon.
- **Business Email Compromise (BEC)** — jenis serangan di mana pihak lawan mendapat akses ke akun email internal yang SAH, lalu menggunakannya untuk menipu orang lain melakukan tindakan tidak sah/curang. Berbeda dari phishing biasa karena identitas yang dipakai benar-benar valid (bukan hasil menyamar dari luar).

---

## 6. Ciri-Ciri Umum Email Phishing

- **Spoofed From Address** — alamat pengirim dipalsukan agar terlihat seperti entitas terpercaya (contoh domain typo: `microsof.com` bukan `microsoft.com`).
- **Urgent Subject or Message** — menciptakan rasa urgensi ("akun akan terkunci dalam 24 jam").
- **Brand Impersonation** — meniru organisasi sah (logo, warna, gaya visual mirip brand asli).
- **Grammar & Spelling Issues** — kesalahan tata bahasa/ejaan (meski makin jarang karena bantuan AI).
- **Generic Content** — kurang personalisasi ("Dear Customer" bukan nama asli penerima).
- **Hidden or Shortened Links** — hyperlink menyamarkan tujuan asli (contoh: `bit.ly/secure-login`).
- **Malicious Attachments** — lampiran disamarkan sebagai file sah (contoh: `invoice.pdf.exe`).

Penting: identifikasi brand yang dipalsukan (spoofed) dilakukan dengan membandingkan nama pengirim yang tampil vs domain email asli di field From — ketidaksesuaian antara keduanya adalah indikator kuat impersonation.

---

## 7. Safe Analysis: Defanging

Defanging adalah teknik membuat URL, domain, atau alamat email menjadi tidak bisa diklik untuk mencegah klik tidak sengaja saat menganalisis konten mencurigakan. Caranya dengan mengganti karakter khusus (`@`, `.`) dengan karakter alternatif dalam kurung siku.

Contoh:
- Original: `http://www.suspiciousdomain.com`
- Defanged: `hxxp[://]www[.]suspiciousdomain[.]com`

Untuk IP address, pola serupa berlaku, contoh: `103[.]234[.]236[.]83` (dari `103.234.236.83`).

---

## 8. Tools yang Digunakan

- **Thunderbird** — email client untuk membuka file `.eml`, melihat tampilan rendered maupun raw message source (`View → Message Source` atau `ctrl+u`).
- **CyberChef** — tool berbasis web untuk berbagai operasi encoding/decoding dan analisis data. Operasi yang dipakai di room ini:
  - **From Base64** — mendecode string base64 menjadi data aslinya (misalnya isi file PDF).
  - **Defang IP Addresses** — mengubah alamat IP menjadi bentuk defanged secara otomatis agar aman ditampilkan/dicatat.

---

## Glossary

- **SMTP** — protokol untuk mengirim email.
- **POP3** — protokol untuk mengunduh email ke satu perangkat (email hilang dari server setelah diunduh).
- **IMAP** — protokol untuk menyinkronkan email antar banyak perangkat (email tetap di server).
- **DNS** — sistem yang digunakan mail server pengirim untuk menemukan alamat mail server domain penerima.
- **Header** — metadata email (pengirim, penerima, jalur pengiriman, dsb).
- **Body** — isi konten pesan email (plain text atau HTML).
- **Message Source** — tampilan raw/mentah dari seluruh isi email termasuk header teknis lengkap.
- **X-Originating-IP** — header custom yang mencatat IP asli pengirim.
- **DKIM** — mekanisme tanda tangan digital untuk verifikasi keaslian domain pengirim.
- **SPF** — mekanisme yang memverifikasi apakah IP pengirim diizinkan mengirim atas nama domain tersebut.
- **DMARC** — kebijakan yang menggabungkan hasil SPF dan DKIM untuk menentukan tindakan terhadap email yang gagal autentikasi.
- **Authentication-Results** — header yang mencatat hasil gabungan SPF, DKIM, DMARC beserta mail server yang melakukan pengecekan.
- **Content-Type** — header yang menunjukkan jenis file attachment.
- **Content-Disposition** — header yang menandakan sebuah bagian email adalah attachment beserta nama filenya.
- **Content-Transfer-Encoding** — header yang menunjukkan metode encoding data attachment (umumnya base64).
- **Base64** — metode encoding data biner menjadi teks ASCII, sering dipakai untuk menyisipkan attachment dalam body email.
- **Spoofing** — memalsukan identitas pengirim (nama/domain) agar terlihat seperti entitas terpercaya.
- **Spam** — email massal tidak diminta.
- **Malspam** — spam yang membawa muatan berbahaya.
- **Phishing** — penipuan email untuk mencuri informasi sensitif dengan menyamar sebagai entitas terpercaya.
- **Spear Phishing** — phishing yang ditargetkan ke individu/organisasi spesifik.
- **Whaling** — spear phishing yang menargetkan eksekutif tingkat tinggi.
- **Smishing** — phishing via SMS.
- **Vishing** — phishing via panggilan suara.
- **BEC (Business Email Compromise)** — serangan menggunakan akun email internal sah yang telah diambil alih.
- **Defanging** — teknik menonaktifkan link/IP/domain agar tidak bisa diklik secara tidak sengaja saat dianalisis.
- **Social Engineering** — teknik manipulasi psikologis untuk menipu korban melakukan suatu tindakan atau membocorkan informasi.

---

## Tools & Platform Rujukan

- **Thunderbird** — email client untuk membuka dan menganalisis file `.eml`, baik tampilan rendered maupun raw source.
- **CyberChef** — https://gchq.github.io/CyberChef/ — platform web untuk berbagai operasi decode/encode data (base64, defanging IP, dsb), digunakan untuk merekonstruksi attachment dan mendefang IOC (Indicator of Compromise).

---

## Catatan Ringkas untuk Ditulis Tangan

**Struktur Email**
- Username @ Domain — mailbox @ tujuan routing
- Header — metadata (From, To, Reply-To, Subject, Date)
- Body — isi pesan (plain text / HTML)

**Protokol**
- SMTP — kirim email
- POP3 — unduh 1 device, hilang dari server
- IMAP — sync banyak device, tetap di server
- Alur: Sender → SMTP → DNS query → DNS respons → kirim ke server penerima → cek mailbox → POP3/IMAP ambil pesan

**Header Teknis Penting**
- X-Originating-Ip — IP asli pengirim
- Received: from — jalur pengiriman, bisa juga cari IP
- DKIM-Signature — tanda tangan digital domain
- SPF (Received-SPF) — cek IP diizinkan kirim atas nama domain
- DMARC — kombinasi kebijakan SPF+DKIM
- Authentication-Results — hasil gabungan cek autentikasi + nama mail server pengecek
- Return-Path — alamat bounce
- Sender vs From — bisa beda kalau kirim atas nama pihak lain

**Attachment**
- Content-Type — tipe file (application/pdf dst)
- Content-Disposition — attachment + nama file
- Content-Transfer-Encoding — biasanya base64
- Decode base64 → From Base64 (CyberChef) → save as file sesuai ekstensi (jangan baca sbg teks)
- Cek valid PDF: %PDF-1.x di awal, %%EOF di akhir

**Jenis Email Berbahaya**
- Spam — massal tak diminta
- Malspam — spam + payload jahat
- Phishing — nyamar entitas trusted
- Spear Phishing — target spesifik individu/org
- Whaling — target eksekutif (CEO/CFO)
- Smishing — via SMS
- Vishing — via telepon
- BEC — akun internal SAH diambil alih, tipu orang dalam

**Ciri Phishing**
- Spoofed From Address (domain typo)
- Urgent subject/message
- Brand impersonation
- Grammar/spelling issues
- Generic content ("Dear Customer")
- Hidden/shortened link (bit.ly dst)
- Malicious attachment (invoice.pdf.exe)
- Cek: nama pengirim tampil vs domain asli di From — beda = red flag

**Defanging**
- http[://] — ganti :// jadi [://]
- domain[.]com — ganti . jadi [.]
- Tujuan: cegah klik tidak sengaja saat analisis
- Tool: CyberChef "Defang IP Addresses"

**Tools**
- Thunderbird — buka .eml, View > Message Source (ctrl+u) untuk raw header
- CyberChef — From Base64 (decode attachment), Defang IP Addresses (defang IOC)
