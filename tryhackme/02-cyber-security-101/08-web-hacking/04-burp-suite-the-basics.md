# Burp Suite Basics: Resume Materi

Room: Burp Suite Basics (TryHackMe)
Tanggal: 1 November 2026

---

## 1. Konsep Dasar Burp Suite

### 1.1 Definisi dan Fungsi

**Burp Suite** adalah framework berbasis Java untuk web application penetration testing. Statusnya standar industri untuk security assessment aplikasi web dan mobile, termasuk yang memakai **API**.

Kemampuan intinya: menangkap dan memanipulasi seluruh traffic **HTTP/HTTPS** antara browser dan web server. Request bisa dilihat, diubah, lalu diteruskan, dan response juga bisa dimodifikasi sebelum diterima browser. Kemampuan ini menjadi tulang punggung seluruh framework dan membuatnya sangat berguna untuk manual web testing.

Komponen paling fundamental adalah **Burp Proxy**. Semua modul lain bekerja dengan data yang melewati Proxy.

### 1.2 Tiga Edisi

Room ini hanya memakai Community Edition.

| Edisi | Lisensi | Ciri utama |
|---|---|---|
| Community | Gratis, non-komersial | Fitur terbatas, tidak bisa save project |
| Professional | Berbayar | Versi tanpa batasan, untuk tester profesional |
| Enterprise | Berbayar | Continuous scanning di server |

**Professional** menambahkan:

- Automated vulnerability scanner
- Fuzzer/brute-forcer tanpa rate limit
- Save project dan pembuatan report
- Built-in API untuk integrasi dengan tool lain
- Akses tak terbatas untuk menambah extension
- Akses ke **Burp Collaborator** (request catcher unik, self-hosted atau di server milik PortSwigger)

**Enterprise** berjalan di server dan memindai target secara otomatis dan berkala, mirip cara Nessus memindai infrastruktur. Berbeda dengan dua edisi lain yang dipakai untuk serangan manual dari mesin lokal.

### 1.3 Fitur Community Edition

**Proxy** adalah fitur paling terkenal. Fungsinya intercept dan modifikasi request serta response saat berinteraksi dengan aplikasi web.

**Repeater** menangkap, memodifikasi, dan mengirim ulang request yang sama berkali-kali. Berguna untuk menyusun payload lewat trial and error (misalnya **SQLi**) atau menguji endpoint.

**Intruder** menyemprotkan request ke endpoint dalam jumlah banyak. Dipakai untuk **brute-force** dan **fuzzing**. Di Community kecepatannya dibatasi (**rate limited**).

**Decoder** melakukan transformasi data: decode informasi yang tertangkap atau encode payload sebelum dikirim ke target.

**Comparer** membandingkan dua data di level kata atau byte. Data besar bisa langsung dikirim ke Comparer dengan satu keyboard shortcut.

**Sequencer** menilai keacakan token seperti session cookie atau data acak lainnya. Jika algoritma pembangkitnya tidak aman secara randomness, terbuka peluang serangan berdampak besar.

### 1.4 Extension dan BApp Store

Codebase Java Burp memungkinkan pengembangan extension. Bahasa yang didukung: Java, Python (lewat interpreter **Jython**), dan Ruby (lewat interpreter **JRuby**).

Modul **Extender** dipakai untuk memuat extension dengan cepat. **BApp Store** adalah marketplace untuk mengunduh modul pihak ketiga. Sebagian extension butuh lisensi Professional, tapi banyak yang tetap tersedia di Community. Contoh: **Logger++** yang memperluas fitur logging bawaan.

---

## 2. Instalasi dan Dashboard

### 2.1 Instalasi

AttackBox sudah memiliki Burp Suite. Kali Linux juga sudah menyertakannya, dan jika hilang bisa dipasang dari repositori apt Kali.

Untuk Linux, macOS, dan Windows, unduh installer **Burp Suite Community Edition** dari halaman downloads PortSwigger dengan memilih OS di dropdown.

- Windows: jalankan file executable
- Linux: jalankan script dari terminal, dengan atau tanpa sudo
- Tanpa sudo di Linux: terpasang di home directory pada `~/BurpSuiteCommunity/BurpSuiteCommunity` dan tidak masuk ke PATH
- Default setting installer umumnya aman diterima, tetap baca dengan teliti

### 2.2 Peluncuran Pertama

Setelah menerima terms and conditions, Burp meminta tipe project. Di Community pilihannya terbatas, cukup klik **Next**. Jendela berikutnya memilih konfigurasi, biarkan default lalu klik **Start Burp**.

Pada peluncuran pertama bisa muncul layar training. Setelah itu yang tampil adalah **Dashboard**.

### 2.3 Empat Kuadran Dashboard

Dashboard dibagi empat kuadran, diberi nomor berlawanan arah jarum jam mulai dari kiri atas.

**Tasks** (kiri atas) mendefinisikan background task. Task default di Community adalah **Live Passive Crawl** yang mencatat otomatis halaman yang dikunjungi. Professional punya tambahan seperti on-demand scan.

**Event log** (kiri bawah) mencatat aksi Burp, seperti proxy dimulai, dan detail koneksi yang lewat Burp.

**Issue activity** (kanan atas) khusus Professional. Menampilkan kerentanan hasil automated scanner, diurutkan menurut severity dan bisa difilter menurut certainty.

**Advisory** (kanan bawah) berisi detail kerentanan, referensi, dan saran remediasi, dan bisa diekspor ke report. Di Community biasanya kosong.

Ikon tanda tanya di berbagai tab membuka jendela bantuan khusus untuk bagian tersebut.

---

## 3. Navigasi dan Settings

### 3.1 Navigasi

Navigasi default lewat menu bar atas.

- **Module Selection**: baris pertama berisi modul (Dashboard, Target, Proxy, Intruder, dst)
- **Sub-tabs**: baris kedua tepat di bawahnya, berisi pengaturan spesifik modul yang dipilih
- **Detaching tabs**: menu Window lalu pilih Detach untuk tab tertentu, sehingga tab terbuka di jendela terpisah. Pasang kembali dengan cara yang sama

Keyboard shortcut default:

| Shortcut | Tab |
|---|---|
| Ctrl + Shift + D | Dashboard |
| Ctrl + Shift + T | Target |
| Ctrl + Shift + P | Proxy |
| Ctrl + Shift + I | Intruder |
| Ctrl + Shift + R | Repeater |

### 3.2 Dua Jenis Settings

**Global settings** (disebut juga User settings) memengaruhi seluruh instalasi Burp dan berlaku setiap aplikasi dijalankan sebagai konfigurasi dasar.

**Project settings** hanya berlaku untuk project saat ini. Community tidak mendukung save project, jadi pengaturan project hilang saat Burp ditutup.

Akses lewat tombol **Settings** di navigation bar atas, yang membuka jendela terpisah. Menu kiri jendela ini berisi:

- **Search**: cari setting dengan keyword
- **Type filter**: tampilkan hanya setting User atau Project
- **Categories**: Tools (Proxy, Intruder, Repeater, Sequencer, Burp's browser), Project (Scope, Collaborator, Tasks, Automatic backup, Logging), Sessions, Network, User interface, Suite, Extensions, dan Configuration library

Banyak tool punya tombol pintasan ke kategori setting terkait, misalnya tombol **Proxy settings** di modul Proxy.

---

## 4. Burp Proxy

### 4.1 Cara Kerja

Alur traffic: Browser, lalu Burp Proxy, lalu Internet. Proxy menangkap request dan response antara user dan target server. Traffic yang tertangkap bisa dimanipulasi, dikirim ke tool lain, atau diizinkan lanjut ke tujuan.

**Intercepting requests**: request ditahan sebelum sampai ke server dan muncul di tab Proxy. Aksi yang tersedia: **Forward** (teruskan), **Drop** (buang), edit, atau kirim ke modul lain. Tombol **Intercept is on** dipakai untuk mematikan intercept supaya request lewat tanpa terhenti.

**Capture and logging**: request tetap dicatat secara default walau intercept dimatikan, berguna untuk analisis belakangan.

**WebSocket support**: komunikasi WebSocket juga ditangkap dan dicatat.

**Logs and history**: sub-tab **HTTP history** dan **WebSockets history** menyimpan request yang tertangkap, bisa dianalisis ulang dan dikirim ke modul lain.

### 4.2 Fitur Penting di Proxy Settings

**Response interception**: secara default response server tidak di-intercept kecuali diminta per request. Checkbox "Intercept responses based on the following rules" bersama rules yang ditentukan memungkinkan interception yang lebih fleksibel. Contoh rule: Content type header Matches text, Request Was intercepted, Status code Does not match 304, URL Is in target scope. Opsi "Automatically update Content-Length header when the response is edited" menjaga header tetap sesuai setelah response diedit.

**Match and Replace**: memakai regular expression untuk memodifikasi request masuk dan keluar secara dinamis, misalnya mengganti user agent atau memanipulasi cookie.

Proxy listener default berada di `127.0.0.1:8080`.

---

## 5. Menghubungkan Browser ke Burp

### 5.1 FoxyProxy di Firefox

Browser harus diarahkan agar traffic melewati Burp. Langkahnya:

1. Install ekstensi **FoxyProxy Basic** di Firefox (sudah terpasang di AttackBox)
2. Klik ikon FoxyProxy di kanan atas, pilih **Options**, lalu klik **Add**
3. Isi konfigurasi:

```
Title : Burp (bebas)
IP    : 127.0.0.1
Port  : 8080
```

4. Klik **Save**
5. Klik ikon FoxyProxy dan pilih konfigurasi Burp untuk mengaktifkan
6. Pastikan Burp Suite berjalan, kalau tidak browser tidak bisa membuat request
7. Aktifkan **Intercept is on** di tab Proxy
8. Buka sebuah website: browser akan hang dan request muncul di Burp

Hal yang perlu diingat:

- Browser hang saat intercept menyala adalah perilaku normal
- Intercept yang lupa dimatikan akan menghalangi browser membuat request apa pun
- Klik kanan pada request di Burp membuka menu aksi (forward, drop, kirim ke tool lain)
- Di AttackBox, tutup tab lain sebelum intercept supaya tidak banyak WebSocket request yang masuk

### 5.2 Burp Browser

Burp menyertakan browser **Chromium** bawaan yang sudah dikonfigurasi memakai proxy tanpa setup tambahan. Cara membuka: tab Proxy, klik tombol **Open browser**.

Masalah di Linux sebagai root (termasuk AttackBox): browser gagal start karena tidak bisa membuat **sandbox**. Dua solusi:

- **Smart option**: buat user baru dengan hak akses rendah lalu jalankan Burp lewat akun itu
- **Easy option**: Settings, Tools, Burp's browser, lalu centang **Allow Burp's browser to run without a sandbox**. Opsi ini nonaktif secara default karena alasan keamanan, karena kompromi browser bisa memberi penyerang akses ke seluruh mesin

---

## 6. Target: Site Map, Issue Definitions, Scope

### 6.1 Tiga Sub-tab Target

**Site map** memetakan aplikasi target dalam struktur pohon. Setiap halaman yang dikunjungi saat proxy aktif muncul otomatis. Di Professional bisa dipakai untuk automated crawling. Di Community tetap berguna untuk pengumpulan data pada tahap enumeration awal, dan sangat berguna memetakan API karena semua endpoint yang diakses aplikasi tertangkap. Endpoint tersembunyi yang tidak ada link-nya tetap muncul di sini jika ada request ke sana.

**Issue definitions** adalah daftar lengkap kerentanan web yang dicari scanner, lengkap dengan deskripsi dan referensi. Meski Community tidak punya scanner penuh, daftar ini berguna untuk menulis report atau mendeskripsikan temuan dari manual testing.

**Scope settings** mengatur target scope dengan include atau exclude domain/IP, supaya fokus pada aplikasi yang diuji dan tidak menangkap traffic yang tidak relevan.

### 6.2 Scoping dan Targeting

Tanpa scope, Burp menangkap dan mencatat semua traffic sehingga cepat menjadi berantakan.

Cara menetapkan scope: tab Target, klik kanan target di daftar kiri, pilih **Add To Scope**. Burp menawarkan untuk berhenti mencatat traffic di luar scope, pilih **yes** pada umumnya. Cek hasilnya di sub-tab Scope.

Perhatian: berhenti mencatat out-of-scope tidak otomatis berhenti meng-intercept. Agar proxy benar-benar mengabaikan traffic di luar scope, buka Proxy settings dan aktifkan rule **And URL Is in target scope** di bagian Request interception rules (dan Response interception rules). Hasilnya tampilan traffic jauh lebih bersih.

---

## 7. Proxying HTTPS

### 7.1 Masalah Sertifikat

Saat membuka situs **TLS** lewat Burp, browser menolak karena tidak mempercayai sertifikat yang disajikan Burp (**PortSwigger CA**). Muncul error seperti "Software is Preventing Firefox From Safely Connecting to This Site".

AttackBox sudah dikonfigurasi, jadi langkah ini hanya diperlukan di mesin sendiri.

### 7.2 Menambahkan CA Certificate ke Firefox

1. Dengan Burp Proxy aktif, buka `http://burp/cert` untuk mengunduh file `cacert.der`
2. Ketik `about:preferences` di URL bar Firefox, lalu cari "certificates"
3. Klik **View Certificates**
4. Klik **Import**, pilih file `cacert.der`
5. Centang **Trust this CA to identify websites**, lalu OK

Setelah itu situs TLS bisa diakses tanpa certificate error dan tetap bisa di-intercept.

Catatan: situs dengan **HSTS** yang ketat (misalnya yang masuk HSTS Preload List) bisa tetap ditolak walau CA sudah terpasang. Untuk situs seperti itu, nonaktifkan FoxyProxy saat mengaksesnya.

---

## 8. Contoh Serangan: Reflected XSS

### 8.1 Konsep

**XSS** (Cross-Site Scripting) adalah penyisipan script sisi klien, biasanya JavaScript, ke halaman web sehingga dieksekusi oleh browser. Jenis yang dipakai di room ini adalah **Reflected XSS**, yang hanya memengaruhi orang yang membuat web request tersebut.

### 8.2 Skenario Form Support

Form support memiliki field Contact Email dengan **client-side filter** yang menolak karakter khusus. Mengetik payload langsung di browser akan diblokir. Filter di sisi klien mudah dilewati karena Burp memungkinkan mengubah request setelah melewati browser.

Langkahnya:

1. Pastikan Burp Proxy aktif dan intercept menyala
2. Isi form dengan data sah, misalnya email `pentester@example.thm` dan query `Test Attack`
3. Submit, request tertangkap di Burp
4. Di raw request, ganti nilai field email dengan payload
5. Select payload lalu tekan **Ctrl + U** untuk URL encode (agar aman dikirim)
6. Klik **Forward**
7. Alert box muncul di browser sebagai tanda XSS berhasil

Payload yang dipakai:

```
<script>alert("Succ3ssful XSS")</script>
```

Inti pelajarannya: validasi yang hanya ada di client-side tidak bisa diandalkan karena penyerang bisa mengirim request langsung lewat proxy.

---

## 9. Glossary

**Proxy**: perantara antara browser dan server yang menangkap traffic.

**Intercept**: menahan request atau response sebelum sampai tujuan.

**Forward**: meneruskan request yang ditahan ke tujuannya.

**Drop**: membuang request yang ditahan.

**Scope**: batasan target yang dipantau dan diuji.

**Endpoint**: path spesifik di sebuah web atau API, misalnya `/about`.

**Site map**: peta struktur target yang terbentuk dari traffic yang dikunjungi.

**Crawling**: menjelajahi link sebuah situs secara otomatis.

**Fuzzing**: mengirim banyak input acak atau tak terduga untuk mencari kerentanan.

**Brute-force**: mencoba banyak kombinasi (misalnya password) sampai ada yang cocok.

**Rate limited**: kecepatan request dibatasi, berlaku untuk Intruder di Community.

**Payload**: data atau kode yang dikirim dalam pengujian atau serangan.

**SQLi**: SQL Injection, penyisipan query ke input yang diteruskan ke database.

**XSS**: Cross-Site Scripting, penyisipan script ke halaman web.

**Reflected XSS**: XSS yang hanya berdampak pada pembuat request.

**Client-side**: validasi atau logika yang berjalan di browser.

**Server-side**: validasi atau logika yang berjalan di server.

**URL encoding**: mengubah karakter khusus ke format aman, misalnya `<` menjadi `%3C`.

**TLS/HTTPS**: protokol komunikasi terenkripsi.

**CA (Certificate Authority)**: pihak yang menerbitkan sertifikat yang dipercaya browser.

**HSTS**: HTTP Strict Transport Security, memaksa browser memakai koneksi aman.

**WebSocket**: koneksi dua arah yang terus terbuka antara browser dan server.

**Sandbox**: lingkungan terisolasi untuk menjalankan program agar dampak masalah terbatas.

**Burp Collaborator**: request catcher unik milik Professional.

**Remediation**: langkah perbaikan setelah kerentanan ditemukan.

---

## 10. Tools & Platform Rujukan

- **Burp Suite Community Edition**: tool utama room ini, diunduh dari halaman downloads PortSwigger
- **FoxyProxy Basic**: ekstensi Firefox untuk mengalihkan traffic ke proxy
- **Burp Browser (Chromium)**: browser bawaan Burp yang sudah terhubung ke proxy
- **BApp Store**: marketplace extension Burp
- **Logger++**: extension untuk memperluas logging
- **Jython / JRuby**: interpreter untuk menulis extension Python / Ruby
- **Nessus**: scanner infrastruktur otomatis, dipakai sebagai pembanding Burp Enterprise
- **Burp Repeater**: tool untuk request manual berulang, materi room berikutnya dalam modul
- **CA Certificate Burp**: diunduh dari `http://burp/cert`
- **Dokumentasi Burp (ikon tanda tanya)**: bantuan kontekstual di dalam aplikasi

---

## 11. Catatan Ringkas untuk Ditulis Tangan

### Burp Suite Umum
Burp Suite = framework Java untuk web pentest
Fungsi inti = intercept/modify traffic HTTP/HTTPS browser dan server
Burp Proxy = komponen inti
Edisi = Community (gratis), Professional (berbayar), Enterprise (berbayar)

### Edisi
Community = gratis, non-komersial, tidak bisa save project
Professional = scanner otomatis, fuzzer tanpa rate limit, save project, API, extension tak terbatas, Collaborator
Enterprise = scan terus-menerus di server, mirip Nessus

### Fitur Community
Proxy = intercept dan ubah request/response
Repeater = kirim ulang request berkali-kali (SQLi, uji endpoint)
Intruder = spray request, brute-force, fuzzing (rate limited)
Decoder = encode/decode data
Comparer = bandingkan 2 data (kata/byte)
Sequencer = uji keacakan token/session cookie

### Extension
Extender = modul load extension
BApp Store = marketplace extension
Bahasa = Java, Python (Jython), Ruby (JRuby)
Contoh = Logger++

### Instalasi
Kali = sudah terpasang, atau dari apt
Windows = jalankan executable
Linux = jalankan script, dengan/tanpa sudo
Tanpa sudo = terpasang di ~/BurpSuiteCommunity/BurpSuiteCommunity, tidak masuk PATH
Peluncuran = project type: Next, config default: Start Burp

### Dashboard (berlawanan arah jarum jam dari kiri atas)
Tasks = background task, default Live Passive Crawl
Event log = catatan aksi Burp
Issue activity = hasil scanner, khusus Professional
Advisory = detail kerentanan dan remediasi, kosong di Community
Ikon ? = bantuan kontekstual

### Navigasi
Module Selection = baris atas
Sub-tabs = baris kedua
Window > Detach = pisahkan tab ke jendela sendiri
Ctrl+Shift+D = Dashboard
Ctrl+Shift+T = Target
Ctrl+Shift+P = Proxy
Ctrl+Shift+I = Intruder
Ctrl+Shift+R = Repeater

### Settings
Global/User settings = berlaku ke seluruh instalasi
Project settings = hanya sesi itu, hilang di Community saat ditutup
Menu Settings = Search, Type filter (User/Project), Categories
Tombol Proxy settings = pintasan ke setting Proxy

### Proxy
Alur = Browser > Burp (127.0.0.1:8080) > Internet
Forward = teruskan request
Drop = buang request
Intercept is on = tombol toggle intercept
Capture and logging = tetap dicatat walau intercept off
HTTP history = riwayat request
WebSockets history = riwayat WebSocket
Response interception = default off, atur lewat rules
Match and Replace = regex ubah request/response otomatis (user agent, cookie)

### Setup Browser
FoxyProxy Basic = ekstensi Firefox
Title = Burp, IP = 127.0.0.1, Port = 8080
Aktifkan = pilih konfigurasi Burp di ikon FoxyProxy
Burp harus berjalan = kalau tidak browser tidak bisa request
Browser hang saat intercept on = normal
Klik kanan request = forward/drop/send to tool
Open browser = Chromium bawaan Burp
Error sandbox (root) = buat user low-privilege, atau centang Allow Burp's browser to run without a sandbox (Settings > Tools > Burp's browser)

### Target
Site map = peta pohon target, auto dari browsing, tangkap endpoint API
Issue definitions = daftar kerentanan web + deskripsi + referensi
Scope settings = include/exclude domain atau IP
Add To Scope = klik kanan target di tab Target
Prompt stop logging out-of-scope = pilih yes
And URL Is in target scope = rule di Proxy settings agar traffic luar scope tidak di-intercept

### HTTPS
Masalah = browser tidak percaya PortSwigger CA
http://burp/cert = unduh cacert.der
about:preferences = buka settings Firefox, cari certificates
View Certificates > Import = pasang cacert.der
Trust this CA to identify websites = wajib dicentang
HSTS ketat = tetap bisa ditolak, matikan FoxyProxy untuk situs itu

### XSS
XSS = injeksi script sisi klien ke halaman web
Reflected XSS = hanya memengaruhi pembuat request
Client-side filter = mudah dibypass lewat Burp
Alur = intercept on, kirim data sah, ganti field email dengan payload, Forward
Payload = <script>alert("Succ3ssful XSS")</script>
Ctrl+U (di Burp) = URL encode teks terpilih
Hasil = alert box muncul = XSS berhasil

### Istilah Kunci
Proxy = perantara browser dan server
Intercept = tahan request
Scope = batas target
Endpoint = path spesifik di web/API
Fuzzing = kirim banyak input acak
Rate limited = kecepatan dibatasi
Sandbox = lingkungan terisolasi
CA = otoritas penerbit sertifikat
HSTS = paksa koneksi HTTPS
Client-side = di browser
Server-side = di server
