```
=== Web Application Basics - Resume Materi ===
[25 May 2026]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 APA ITU WEB APPLICATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Definisi : Program yang berjalan di server & diakses via web browser
Cara     : Browser kirim request → Server proses → Server kirim response
Bedanya  : Tidak perlu install, cukup buka browser & ketik alamat
Analogi  : Planet = web app, Astronot = user/browser,
           Permukaan = Front End, Isi dalam = Back End

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 FRONT END (Yang Terlihat User)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
HTML  → Struktur halaman (Hypertext Markup Language)
        Analogi : DNA organisme = instruksi penyusun dasar
CSS   → Tampilan & gaya (Cascading Style Sheets)
        Analogi : Bagian DNA yang atur warna, bentuk, tekstur
JS    → Logika & interaktivitas (JavaScript)
        Analogi : Otak organisme = buat keputusan & respons

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 BACK END (Yang Tidak Terlihat)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Database      → Simpan, ubah, ambil data
                Analogi : Perpustakaan/lemari arsip
Infrastructure→ Web server, app server, storage, networking
                Analogi : Jalan, mobil, bahan bakar
WAF           → Web Application Firewall (opsional)
                Filter request berbahaya sebelum sampai server
                Analogi : Atmosfer pelindung sinar UV

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 ANATOMI URL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Contoh : http://user:password@tryhackme.com:80/view-room?id=1#task3

Scheme       → http:// atau https://
               HTTPS = enkripsi koneksi, lebih aman
User         → Kredensial login (JARANG & TIDAK AMAN dipakai)
Host/Domain  → tryhackme.com = nama website tujuan
               ⚠ Typosquatting = domain palsu mirip asli (phishing)
Port         → :80 (HTTP) / :443 (HTTPS) → range 1-65535
Path         → /view-room = halaman/file spesifik di server
Query String → ?id=1 = parameter tambahan (awali tanda ?)
               ⚠ Bisa dimanipulasi → rentan injection attack
Fragment     → #task3 = bagian spesifik dalam halaman (awali #)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 HTTP MESSAGES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Definisi : Paket data yang dipertukarkan antara client & server
2 Jenis  :
  HTTP Request  → Dikirim USER ke server (trigger aksi)
  HTTP Response → Dikirim SERVER ke user (balas request)

Struktur setiap message :
  1. Start Line  → Perkenalan pesan (jenis, tujuan, versi)
  2. Headers     → Info tambahan key-value pairs
  3. Empty Line  → Pemisah header & body (WAJIB ADA)
  4. Body        → Data aktual yang dikirim/diterima

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 HTTP REQUEST LINE & METHODS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Format Request Line : METHOD /path HTTP/version

METHOD UTAMA :
  GET     → Ambil data dari server (tanpa ubah apapun)
            ⚠ Jangan taruh token/password di GET (plaintext)
  POST    → Kirim data baru ke server
            ⚠ Validasi input → cegah SQL injection & XSS
  PUT     → Ganti/update data di server (replace total)
            ⚠ Cek otorisasi user sebelum terima request
  DELETE  → Hapus data dari server
            ⚠ Hanya user berwenang yang boleh delete

METHOD LAINNYA :
  PATCH   → Update SEBAGIAN resource (bukan ganti total)
  HEAD    → Seperti GET tapi hanya ambil headers (tanpa body)
  OPTIONS → Tanya ke server: "method apa saja yang tersedia?"
  TRACE   → Debugging, lihat method yang diizinkan
            ⚠ Sebaiknya DINONAKTIFKAN (security risk)
  CONNECT → Buat koneksi aman/tunnel (untuk HTTPS)

HTTP VERSION :
  HTTP/0.9 (1991) → Hanya GET
  HTTP/1.0 (1996) → Tambah headers & caching
  HTTP/1.1 (1997) → Persistent connection, chunked encoding ← PALING UMUM
  HTTP/2   (2015) → Multiplexing, header compression
  HTTP/3   (2022) → Pakai QUIC, lebih cepat & aman

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 REQUEST HEADERS (Common)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Host         → Nama web server tujuan
               Contoh : Host: tryhackme.com
User-Agent   → Info browser yang mengirim request
               Contoh : User-Agent: Mozilla/5.0
Referer      → URL asal sebelum request ini
               Contoh : Referer: https://www.google.com/
Cookie       → Data yang sebelumnya disimpan server di browser
               Contoh : Cookie: user_type=student; room=intro
Content-Type → Format/jenis data yang dikirim
               Contoh : Content-Type: application/json

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 REQUEST BODY - FORMAT DATA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
URL Encoded  → key=value&key=value
(application/x-www-form-urlencoded)
               Cocok : form sederhana, login

Form Data    → Blok-blok dipisah boundary string
(multipart/form-data)
               Cocok : upload file/gambar (binary data)

JSON         → {"name": "value", "age": 27}
(application/json)
               Cocok : API modern, data terstruktur

XML          → <tag>value</tag> bisa nested
(application/xml)
               Cocok : sistem lama, data kompleks

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 HTTP RESPONSE - STATUS CODES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Status Line : HTTP/version STATUS_CODE Reason_Phrase

5 KATEGORI :
  1xx Informational → Server terima sebagian, tunggu sisanya
  2xx Successful    → Berhasil, data dikirim balik
  3xx Redirection   → Resource pindah lokasi baru
  4xx Client Error  → Kesalahan dari SISI USER
  5xx Server Error  → Kesalahan dari SISI SERVER

KODE WAJIB HAPAL :
  100 Continue          → Server siap terima sisa request
  200 OK                → Sukses total
  301 Moved Permanently → Pindah permanen, pakai URL baru
  404 Not Found         → Resource tidak ditemukan
  500 Internal Server Error → Server error/crash

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 RESPONSE HEADERS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
REQUIRED :
  Date         → Waktu response dibuat server
  Content-Type → Jenis konten yang dikirim (+ charset)
  Server       → Software server (nginx/apache)
                 ⚠ Sering disembunyikan karena bisa bocor ke attacker

COMMON :
  Set-Cookie   → Kirim cookie ke browser untuk disimpan
                 ⚠ Wajib pakai flag HttpOnly & Secure
  Cache-Control→ Berapa lama browser boleh cache response
                 ⚠ Data sensitif → gunakan no-cache
  Location     → URL tujuan redirect (3xx)
                 ⚠ Validasi ketat → cegah open redirect

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 SECURITY HEADERS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CSP (Content-Security-Policy)
  → Tentukan sumber konten yang diizinkan dimuat browser
  → Cegah XSS & injeksi konten dari domain asing
  Properti :
    default-src → kebijakan default (biasanya 'self')
    script-src  → dari mana JS boleh dimuat
    style-src   → dari mana CSS boleh dimuat
  Contoh :
    Content-Security-Policy: default-src 'self';
    script-src 'self' https://cdn.tryhackme.com

HSTS (Strict-Transport-Security)
  → Paksa browser SELALU pakai HTTPS
  Direktif :
    max-age=63072000    → berlaku selama ini (detik)
    includeSubDomains   → berlaku juga untuk subdomain
    preload             → masuk daftar HSTS browser (bahkan sebelum visit pertama)
  Contoh :
    Strict-Transport-Security: max-age=63072000;
    includeSubDomains; preload

X-Content-Type-Options
  → Cegah browser tebak-tebak MIME type sendiri
  Direktif :
    nosniff → browser harus ikut Content-Type header, dilarang sniff
  Contoh :
    X-Content-Type-Options: nosniff

Referrer-Policy
  → Kontrol info yang dibagikan saat user klik link/redirect
  Direktif :
    no-referrer                   → tidak kirim info referrer sama sekali
    same-origin                   → kirim hanya ke sesama domain sendiri
    strict-origin                 → kirim hanya jika protokol sama (HTTPS→HTTPS)
    strict-origin-when-cross-origin → full URL untuk same-origin,
                                      origin saja untuk cross-origin
  Cek security headers di : https://securityheaders.io/

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 ISTILAH PENTING WAJIB HAPAL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
XSS              = Cross-Site Scripting → injeksi script berbahaya ke halaman
SQL Injection    = injeksi perintah SQL berbahaya ke input
MIME Type        = label jenis file (text/html, image/jpeg, dll)
MIME Sniffing    = browser menebak MIME type sendiri (berbahaya)
Typosquatting    = domain palsu mirip asli untuk phishing
Open Redirect    = manipulasi Location header → redirect ke situs jahat
Clickjacking     = menipu user klik sesuatu yang tersembunyi
HttpOnly Flag    = cookie tidak bisa diakses JavaScript
Secure Flag      = cookie hanya dikirim lewat HTTPS
no-cache         = instruksi jangan simpan cache (data sensitif)
Persistent Conn. = koneksi tidak langsung putus setelah 1 request (HTTP/1.1)
Chunked Encoding = kirim data dalam potongan-potongan (HTTP/1.1)
QUIC             = protokol baru lebih cepat yang dipakai HTTP/3
```