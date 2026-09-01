# Resume: Phishing Analysis - Case Studies

**Tanggal:** 1 September 2026
**Room:** Phishing Analysis - Case Studies (Room kedua dari modul Phishing Analysis, SOC Level 1 Path)

---

## 1. Pendahuluan dan Tujuan Room

Room ini merupakan praktik lanjutan setelah room **Phishing Analysis Fundamentals**. Fokusnya adalah menganalisis sampel email phishing nyata untuk memahami taktik yang digunakan penyerang meniru komunikasi legitimate.

### 1.1 Objectives (Tujuan Pembelajaran)

- Mengidentifikasi taktik **social engineering** umum dalam phishing.
- Menganalisis red flag yang terkandung dalam email phishing.
- Mendeteksi manipulasi link dan **tracking pixel**.
- Membedah teknik **credential harvesting** dan manipulasi attachment.

---

## 2. Teknik-Teknik Phishing yang Dibahas

Room ini membahas delapan sampel email phishing nyata (Task 2-7), masing-masing menonjolkan kombinasi teknik berbeda. Berikut klasifikasi teknik yang muncul berulang di seluruh sampel.

### 2.1 Spoofed Email Address (Alamat Email Dipalsukan)

Penyerang mengatur **display name** (nama tampilan) menjadi nama brand terpercaya (PayPal, Distribution Center, Netflix billing, Apple Support, DHL Express), padahal alamat email asli (bagian `user@domain`) sama sekali tidak terkait dengan brand tersebut.

Poin kunci: display name adalah teks bebas yang bisa diisi apa saja oleh pengirim, sedangkan domain di dalam tanda `< >` menunjukkan sumber pengiriman yang sebenarnya. Verifikasi keaslian pengirim harus selalu mengacu ke domain, bukan display name.

### 2.2 Sense of Urgency / Artificial Urgency

Subjek dan isi email dirancang memicu reaksi cepat tanpa berpikir panjang: transaksi mendadak, akun ditangguhkan (suspended), batas waktu kedaluwarsa yang sempit, atau pembelian tak sah. Tujuannya membuat korban bertindak (klik link/buka attachment) sebelum sempat memverifikasi.

### 2.3 Brand Impersonation

Penggunaan template HTML dan logo resmi (PayPal, DHL, Adobe, Microsoft/Outlook, Netflix, Apple) untuk membangun kesan keaslian secara visual. Sering dikombinasikan dengan **layering** beberapa brand terpercaya sekaligus (contoh: OneDrive lalu Adobe lalu Microsoft) supaya korban semakin percaya di setiap tahap.

### 2.4 URL Shortening dan Link Manipulation

- **URL shortening**: layanan pemendek URL (mis. `is.gd`) dipakai untuk menyembunyikan tujuan akhir link agar tidak bisa diverifikasi sekilas.
- **Link manipulation**: teks tampilan link (display text) tidak mencerminkan hyperlink destination sebenarnya di source code HTML — nomor tracking atau tombol terlihat sah, tapi `href` mengarah ke domain berbeda.

Tool bantu untuk investigasi shortened URL tanpa mengunjungi tujuannya secara langsung: **WhereGoes**.

### 2.5 Pixel Tracking

Penyisipan gambar sangat kecil/tak terlihat (sering `width:0px;height:0px;display:none;`) di body email. Ketika gambar ini otomatis dimuat (email dibuka), server penyerang menerima sinyal bahwa email telah dibaca. Ini salah satu alasan email client (Yahoo, dll) otomatis memblokir gambar pada email mencurigakan.

### 2.6 Link Redirection Chain (Rantai Redirect Bertingkat)

Satu klik awal bisa memicu beberapa kali redirect melalui domain berbeda sebelum sampai ke halaman akhir. Setiap tahap redirect menambah lapisan penyamaran sehingga filter email dasar sulit mendeteksi tujuan akhir yang berbahaya.

### 2.7 Credential Harvesting

Halaman login palsu (meniru Outlook/Office365/mail provider lain) dibuat murni sebagai **front** untuk mengirim kredensial langsung ke server penyerang — bukan melakukan autentikasi asli. Ciri khasnya: pesan error generik muncul terlepas dari kredensial apa pun yang dimasukkan (valid maupun tidak).

Catatan penting: typo dan tata bahasa buruk semakin tidak bisa diandalkan sebagai indikator, karena AI kini memudahkan penyerang membuat konten yang rapi dan bebas kesalahan.

### 2.8 Attachment sebagai Vektor Serangan

Beberapa sampel tidak menaruh link berbahaya langsung di body email (agar lolos filter dasar), melainkan menyembunyikannya di dalam attachment:

- **PDF** berisi embedded link menuju domain non-official.
- **.dot** (Microsoft Word Template) — format tidak wajar untuk sebuah tanda terima/receipt, berisi gambar besar yang jika diklik mengarah ke redirect link panjang dan kompleks.
- **.xlsx** (Excel) — berisi satu clickable link yang memicu unduhan dan eksekusi file `.exe` berbahaya saat diklik.

### 2.9 Recipient BCC (Blind Carbon Copy)

Korban tidak dikirimi email secara langsung di kolom To, melainkan lewat kolom **Bcc**. Kolom To biasanya diisi alamat lain (acak/palsu) untuk mengaburkan target sebenarnya dan menyamarkan email sebagai broadcast massal, bukan serangan bertarget.

### 2.10 Poor Grammar and Typos

Kesalahan ejaan pada nama brand (mis. "Netfllx", "Netlix") atau tata bahasa yang janggal (nonsensical wording) pada instruksi di halaman phishing. Indikator klasik, tapi keandalannya menurun seiring penyalahgunaan AI untuk membuat konten lebih meyakinkan.

### 2.11 Conflicting Geographical/Language Markers

Ketidaksesuaian antara domain pengirim, alamat tujuan pada dokumen, dan bahasa isi dokumen (contoh: domain Jerman, invoice dialamatkan ke India, isi dokumen berbahasa Mandarin). Kombinasi yang saling bertentangan seperti ini adalah red flag klasik yang menunjukkan dokumen tidak organik/asli.

---

## 3. Dampak Jika Malware/Executable Berhasil Dijalankan

Ketika attachment (mis. Excel) berhasil memicu eksekusi payload (`.exe`), potensi dampak yang bisa dilakukan penyerang:

- **Establish Persistence**: membuat backdoor atau scheduled task agar akses tetap ada setelah reboot sistem.
- **Exfiltrate Data**: mencuri file sensitif, kredensial, atau password yang tersimpan di browser.
- **Deploy Ransomware**: mengenkripsi sistem korban dan meminta tebusan (ransom) untuk pemulihan data.

---

## 4. Studi Kasus per Task

### 4.1 Task 2 — Cancel Your Order (Tiruan PayPal)

Email tiruan tanda terima PayPal untuk pembelian gift card. Red flag utama: display name `service@paypal.com` tidak cocok dengan domain asli pengirim (`sultanbogor.com`); tombol "Cancel the order" mengarah ke shortened URL (`is.gd`) yang setelah di-expand ternyata mengarah ke halaman tidak relevan (Google consent page), menandakan link sudah tidak aktif/redirect acak.

### 4.2 Task 3 — Track Your Package (Tiruan Distribution Center)

Notifikasi pengiriman palsu dengan nomor tracking fiktif untuk memicu urgensi. Investigasi raw HTML source mengungkap hyperlink destination yang berbeda total dari display text, plus tracking pixel (`Tracking.png`) tersembunyi yang mengirim sinyal ke server penyerang saat email dibuka.

### 4.3 Task 4 — Download Document Here (Rantai Redirect Multi-Tahap)

Email fax palsu dari "Citrix Attachments" dengan tenggat kedaluwarsa hari yang sama (artificial urgency). Alur serangan: klik tombol download → redirect ke halaman tiruan OneDrive → redirect kedua ke halaman tiruan Adobe (dengan instruksi janggal/nonsensical) → halaman login palsu (tiruan Outlook) yang selalu menampilkan error "Invalid Credentials" apa pun input yang dimasukkan, karena halaman hanya mengirim kredensial ke server penyerang.

### 4.4 Task 5 — Your Account Is on Hold (Tiruan Netflix)

Email dengan typo pada nama brand ("Netfllx", "Netlix") menginformasikan akun ditangguhkan. Link berbahaya disembunyikan di dalam attachment PDF (bukan langsung di body), dengan link berjudul "Update Payment Account" menuju domain non-Netflix. Terdapat red flag nomor telepon format tidak wajar, namun juga trik menyisipkan domain help center Netflix yang benar-benar asli untuk membangun kepercayaan palsu.

### 4.5 Task 6 — Your Recent Purchase (Tiruan Apple Support)

Body email kosong sepenuhnya; satu-satunya konten adalah attachment `.dot` (format tidak wajar untuk receipt). Korban di-BCC, sementara kolom To diisi alamat lain. Gambar besar dalam dokumen berisi redirect link yang sangat panjang dan kompleks menuju domain phishing, meski memuat kata familiar ("apps", "ios") agar tampak sah.

### 4.6 Task 7 — Scheduled Shipment (Tiruan DHL Express)

Notifikasi pengiriman dengan display name "DHL Express" namun domain pengirim (`glamcarcompany.de`) sama sekali tidak terkait DHL. Attachment `.xlsx` berisi kejanggalan geografis/bahasa (domain Jerman, alamat India, isi dokumen Mandarin) dan satu clickable link yang memicu unduhan serta eksekusi payload `regasms.exe`. Di lingkungan pengujian, eksekusi ini gagal dengan system error, namun tetap menunjukkan niat penyerang menjalankan kode di mesin korban.

---

## 5. Glossary (Istilah Kunci)

- **Phishing** — upaya penipuan melalui komunikasi elektronik yang menyamar sebagai entitas terpercaya untuk mencuri data atau memicu tindakan berbahaya dari korban.
- **Social Engineering** — manipulasi psikologis untuk membuat korban melakukan tindakan atau mengungkap informasi tanpa sadar sedang dieksploitasi.
- **Spoofed Email Address** — alamat pengirim yang dipalsukan tampilannya (display name) agar terlihat berasal dari sumber terpercaya.
- **Display Name vs Domain** — display name adalah teks bebas yang ditampilkan; domain di dalam `< >` adalah sumber pengiriman aktual yang harus diverifikasi.
- **URL Shortening** — layanan pemendek URL yang bisa disalahgunakan untuk menyembunyikan tujuan link berbahaya.
- **Pixel Tracking** — gambar sangat kecil/tak terlihat yang disisipkan untuk melacak kapan email dibuka.
- **Link Manipulation** — ketidaksesuaian antara display text link dan hyperlink destination yang sebenarnya di source code.
- **Credential Harvesting** — teknik mengumpulkan username/password korban lewat halaman login palsu yang tidak melakukan autentikasi asli.
- **Link Redirection Chain** — rangkaian beberapa kali redirect URL untuk mengaburkan tujuan akhir dari filter keamanan dasar.
- **Brand Impersonation** — penggunaan logo dan template HTML brand resmi untuk menciptakan kesan keaslian.
- **Artificial Urgency / Sense of Urgency** — teknik menciptakan tekanan waktu palsu agar korban bertindak tanpa berpikir panjang.
- **BCC (Blind Carbon Copy)** — metode pengiriman email di mana penerima tidak terlihat oleh penerima lain (berbeda dari CC yang terlihat oleh semua penerima).
- **Attachment sebagai Vektor Serangan** — penggunaan file lampiran (PDF, .dot, .xlsx) untuk menyembunyikan link atau payload berbahaya, menghindari deteksi filter email berbasis body/link.
- **Payload/Executable** — file (mis. `.exe`) yang jika dijalankan mengeksekusi kode berbahaya di sistem korban.
- **Persistence** — kemampuan malware mempertahankan akses ke sistem korban meski setelah reboot (mis. lewat backdoor atau scheduled task).
- **Exfiltration** — proses pencurian data sensitif dari sistem korban ke pihak penyerang.
- **Ransomware** — jenis malware yang mengenkripsi sistem/data korban dan meminta tebusan untuk pemulihan.
- **Conflicting Geographical/Language Markers** — inkonsistensi lokasi/bahasa dalam dokumen phishing yang menjadi red flag legitimasi.

---

## 6. Tools & Platform Rujukan

- **WhereGoes** — tool online untuk mengekspansi (expand) shortened URL dan melihat tujuan akhirnya tanpa harus benar-benar mengunjungi halaman tersebut, sehingga aman digunakan saat investigasi link mencurigakan.

---

## 7. Catatan Ringkas untuk Ditulis Tangan

**Konsep Dasar**
- Phishing — penipuan elektronik, menyamar sebagai pihak terpercaya
- Social engineering — manipulasi psikologis korban

**Ciri Pengirim Palsu**
- Display name — teks bebas, bisa diisi apa saja
- Domain (dalam `< >`) — sumber pengiriman asli, wajib dicek
- Spoofed address — display name tidak cocok dengan domain

**Teknik Link**
- URL shortening — sembunyikan tujuan link asli (cek pakai WhereGoes)
- Link manipulation — display text ≠ href asli di source HTML
- Redirection chain — beberapa kali redirect sebelum sampai tujuan akhir

**Tracking**
- Pixel tracking — gambar kecil/invisible, lapor ke attacker saat email dibuka
- Ciri: `width:0px height:0px display:none`

**Credential Harvesting**
- Halaman login palsu — tidak autentikasi asli, cuma kirim data ke attacker
- Ciri: error generik muncul terus apa pun kredensial yang dimasukkan
- Typo/grammar buruk — makin gak reliable karena AI bikin konten makin rapi

**Attachment sebagai Vektor**
- PDF — embedded link ke domain non-official
- .dot — format aneh utk receipt, gambar besar → redirect link panjang
- .xlsx — link → download & eksekusi .exe

**Dampak Executable Berhasil Jalan**
- Persistence — backdoor/scheduled task, akses tetap ada setelah reboot
- Exfiltration — curi file/kredensial/password browser
- Ransomware — enkripsi sistem, minta tebusan

**Red Flag Lain**
- Artificial urgency — tekanan waktu palsu
- Brand impersonation — logo/HTML template brand asli dipakai palsu
- BCC — korban di-BCC, kolom To diisi alamat lain/acak
- Conflicting geo/language markers — domain, alamat, bahasa dokumen gak nyambung satu sama lain

**Tools**
- WhereGoes — expand shortened URL tanpa kunjungi langsung

**Studi Kasus Singkat**
- Task 2: PayPal palsu — is.gd redirect ke Google (link mati/acak)
- Task 3: Distribution Center palsu — tracking pixel + hyperlink mismatch
- Task 4: Citrix fax palsu — OneDrive → Adobe → fake Outlook login
- Task 5: Netflix palsu — link bahaya disembunyikan di PDF attachment
- Task 6: Apple Support palsu — body kosong, BCC, redirect link di gambar .dot
- Task 7: DHL palsu — domain Jerman + alamat India + isi Mandarin, xlsx → regasms.exe
