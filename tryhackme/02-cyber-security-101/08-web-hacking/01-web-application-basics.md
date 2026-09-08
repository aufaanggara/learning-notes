# Resume Materi — Web Application Basics
**TryHackMe Room | Diselesaikan: 25 Mei 2026**

---

## 1. Gambaran Umum Web Application

Sebuah **web application** adalah program yang berjalan di server dan diakses pengguna melalui web browser, tanpa perlu instalasi di sisi client. Komunikasinya bersifat dua arah: browser mengirim **request**, server memproses lalu mengirim **response**.

Seluruh komponen web application terbagi menjadi dua lapisan besar:

- **Front End** — bagian yang terlihat dan diinteraksikan langsung oleh pengguna di browser
- **Back End** — bagian yang tidak terlihat, bekerja di balik layar untuk mendukung fungsionalitas

---

## 2. Front End

Front End dibangun dari tiga teknologi utama yang bekerja bersama:

**HTML (Hypertext Markup Language)**
Fondasi struktural halaman web. Berisi instruksi tentang *apa* yang ditampilkan — judul, paragraf, gambar, tombol. HTML bersifat statis dan tidak memiliki logika.

**CSS (Cascading Style Sheets)**
Mengatur tampilan visual dari elemen HTML — warna, ukuran font, tata letak, jarak, dan estetika keseluruhan. CSS tidak mengubah struktur, hanya mengubah penampilan.

**JavaScript (JS)**
Memberikan interaktivitas dan logika dinamis pada halaman. Memungkinkan konten berubah tanpa reload, form divalidasi di sisi client, animasi berjalan, dan keputusan dibuat berdasarkan input pengguna.

---

## 3. Back End

Back End adalah semua komponen yang berjalan di sisi server dan tidak langsung terlihat oleh pengguna.

**Database**
Tempat data disimpan, dimodifikasi, dan diambil kembali. Menyimpan informasi seperti preferensi pengguna, data akun, konten dinamis, dan sebagainya.

**Infrastructure**
Kumpulan komponen yang menopang jalannya web application: web server, application server, storage, perangkat jaringan, dan software pendukung lainnya.

**WAF (Web Application Firewall)**
Komponen opsional yang bertugas menyaring request masuk dan memblokir traffic berbahaya sebelum sampai ke web server. Memberikan lapisan proteksi tambahan terhadap serangan umum.

---

## 4. URL — Uniform Resource Locator

**URL** adalah alamat lengkap yang mengarahkan browser ke sebuah resource di internet. Setiap bagian URL memiliki fungsi spesifik.

**Contoh URL lengkap:**

```
http://user:password@tryhackme.com:80/view-room?id=1#task3
```

### 4.1 Komponen URL

| Komponen | Contoh | Fungsi |
|---|---|---|
| **Scheme** | `http://` atau `https://` | Protokol komunikasi yang digunakan |
| **User** | `user:password@` | Kredensial login (jarang dipakai, tidak aman) |
| **Host/Domain** | `tryhackme.com` | Nama website yang dituju |
| **Port** | `:80` | Pintu layanan spesifik di server |
| **Path** | `/view-room` | File atau halaman spesifik yang diminta |
| **Query String** | `?id=1` | Parameter tambahan, diawali tanda `?` |
| **Fragment** | `#task3` | Bagian spesifik dalam halaman, diawali `#` |

### 4.2 Catatan Keamanan URL

- **HTTPS** mengenkripsi koneksi, lebih aman dari HTTP biasa. Browser dan praktisi keamanan selalu merekomendasikan HTTPS.
- **Typosquatting** adalah praktik mendaftarkan domain yang mirip dengan aslinya namun ada kesalahan ketik kecil, digunakan untuk serangan phishing.
- **Query String** dapat dimodifikasi oleh pengguna, sehingga rentan terhadap **injection attacks** jika tidak divalidasi dengan benar.
- **Fragment** juga dapat dimodifikasi dan harus disanitasi untuk mencegah injection.
- Port standar: **80** untuk HTTP, **443** untuk HTTPS. Range port: 1 hingga 65.535.

---

## 5. HTTP Messages

**HTTP messages** adalah paket data yang dipertukarkan antara client dan server. Ada dua jenis:

- **HTTP Request** — dikirim oleh pengguna/browser untuk memicu aksi di server
- **HTTP Response** — dikirim oleh server sebagai balasan atas request

### 5.1 Struktur Umum HTTP Message

Setiap HTTP message (baik request maupun response) memiliki empat bagian:

**1. Start Line**
Baris pertama yang mengidentifikasi jenis pesan. Untuk request berisi method, path, dan versi HTTP. Untuk response berisi versi HTTP, status code, dan reason phrase.

**2. Headers**
Pasangan key-value yang memberikan informasi tambahan dan instruksi kepada client maupun server. Mencakup hal-hal seperti tipe konten, ukuran data, informasi browser, dan keamanan.

**3. Empty Line**
Baris kosong wajib yang memisahkan headers dari body. Tanpa baris ini, parser akan salah menginterpretasi pesan dan menyebabkan error.

**4. Body**
Tempat data aktual berada. Pada request, body berisi data yang dikirim user (form data, JSON, dll). Pada response, body berisi konten yang diminta (HTML, JSON, gambar, dll).

---

## 6. HTTP Request — Request Line dan Methods

### 6.1 Format Request Line

```
METHOD /path HTTP/version
```

Contoh:

```
GET /user/login.html HTTP/1.1
```

### 6.2 HTTP Methods

**Method utama (CRUD operations):**

**GET**
Mengambil data dari server tanpa membuat perubahan apapun. Data tidak boleh dimasukkan ke URL GET karena muncul sebagai plaintext — hindari menyertakan token atau password.

**POST**
Mengirim data baru ke server, biasanya untuk membuat atau memperbarui resource. Selalu validasi dan sanitasi input untuk mencegah SQL injection dan XSS.

**PUT**
Mengganti seluruh resource di server dengan data baru (replace total). Verifikasi otorisasi pengguna sebelum menerima request ini.

**DELETE**
Menghapus resource dari server. Hanya pengguna yang berwenang yang boleh melakukan operasi ini.

**Method tambahan:**

**PATCH** — Memperbarui sebagian resource, bukan mengganti keseluruhan. Selalu validasi untuk menghindari inkonsistensi data.

**HEAD** — Identik dengan GET namun hanya mengambil headers, tanpa body. Berguna untuk cek metadata tanpa mendownload konten penuh.

**OPTIONS** — Menanyakan ke server method apa saja yang didukung untuk sebuah resource. Membantu client memahami kapabilitas server.

**TRACE** — Menampilkan method yang diizinkan, digunakan untuk debugging. Sebaiknya **dinonaktifkan** di production karena membuka informasi yang berguna bagi penyerang.

**CONNECT** — Membuat koneksi tunnel yang aman, digunakan khususnya untuk HTTPS. Tidak umum tapi kritis untuk komunikasi terenkripsi.

### 6.3 Versi HTTP

| Versi | Tahun | Fitur Utama |
|---|---|---|
| HTTP/0.9 | 1991 | Hanya mendukung GET |
| HTTP/1.0 | 1996 | Tambah headers, dukungan berbagai tipe konten |
| **HTTP/1.1** | 1997 | Persistent connections, chunked transfer encoding — **paling umum dipakai** |
| HTTP/2 | 2015 | Multiplexing, header compression, prioritization |
| HTTP/3 | 2022 | Berbasis protokol QUIC, lebih cepat dan aman |

Meskipun HTTP/2 dan HTTP/3 lebih modern, HTTP/1.1 masih dominan karena kompatibilitasnya yang luas.

---

## 7. HTTP Request — Headers dan Body

### 7.1 Common Request Headers

**Host**
Menentukan nama web server tujuan dari request.
Contoh: `Host: tryhackme.com`

**User-Agent**
Menyertakan informasi tentang browser yang mengirim request.
Contoh: `User-Agent: Mozilla/5.0`

**Referer**
Menunjukkan URL dari mana request ini berasal (halaman sebelumnya).
Contoh: `Referer: https://www.google.com/`

**Cookie**
Data yang sebelumnya diminta server untuk disimpan browser, dikirim kembali bersama setiap request.
Contoh: `Cookie: user_type=student; room=introtowebapplication`

**Content-Type**
Mendeskripsikan format/jenis data yang ada di dalam body request.
Contoh: `Content-Type: application/json`

### 7.2 Format Request Body

Format body request bervariasi tergantung jenis data yang dikirim:

**URL Encoded** (`application/x-www-form-urlencoded`)
Data disusun sebagai pasangan key=value, dipisah simbol `&`. Karakter khusus di-percent-encode.

```
name=Aleksandra&age=27&country=US
```

Cocok untuk form sederhana seperti halaman login.

**Form Data** (`multipart/form-data`)
Beberapa blok data dikirim sekaligus, masing-masing dipisah oleh boundary string yang unik. Mendukung pengiriman binary data seperti file dan gambar.

```
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="username"

aleksandra
----WebKitFormBoundary7MA4YWxkTrZu0gW--
```

**JSON** (`application/json`)
Data diformat sebagai pasangan name:value di dalam curly braces. Pasangan dipisah koma.

```json
{
    "name": "Aleksandra",
    "age": 27,
    "country": "US"
}
```

Paling umum digunakan untuk API modern.

**XML** (`application/xml`)
Data disusun dalam tags berlabel yang memiliki pembuka dan penutup, bisa bersarang satu sama lain.

```xml
<user>
    <name>Aleksandra</name>
    <age>27</age>
    <country>US</country>
</user>
```

Lebih umum di sistem lama dan integrasi enterprise.

---

## 8. HTTP Response — Status Line dan Status Codes

### 8.1 Format Status Line

```
HTTP/version STATUS_CODE Reason_Phrase
```

Contoh: `HTTP/1.1 200 OK`

Status Line memuat tiga informasi: versi HTTP yang digunakan, status code tiga digit, dan reason phrase yang menjelaskan status code dalam teks yang bisa dibaca manusia.

### 8.2 Kategori Status Code

| Rentang | Kategori | Artinya |
|---|---|---|
| 100–199 | Informational | Server terima sebagian request, tunggu sisanya |
| 200–299 | Successful | Request berhasil diproses sepenuhnya |
| 300–399 | Redirection | Resource pindah ke lokasi lain |
| 400–499 | Client Error | Ada masalah dari sisi pengguna/request |
| 500–599 | Server Error | Server gagal memproses request |

### 8.3 Status Code yang Wajib Dihapal

**100 Continue** — Server menerima bagian pertama request dan siap untuk menerima sisanya.

**200 OK** — Request berhasil total. Server mengirimkan resource yang diminta.

**301 Moved Permanently** — Resource dipindah permanen ke URL baru. Gunakan URL baru itu mulai sekarang.

**404 Not Found** — Server tidak menemukan resource di URL yang diberikan. Periksa kembali alamatnya.

**500 Internal Server Error** — Ada yang salah di sisi server sehingga request tidak bisa diproses.

---

## 9. HTTP Response — Headers dan Body

### 9.1 Required Response Headers

**Date**
Menunjukkan waktu tepat ketika response dibuat oleh server.
Contoh: `Date: Fri, 23 Aug 2024 10:43:21 GMT`

**Content-Type**
Memberitahu browser jenis konten yang diterima beserta character set-nya untuk ditampilkan dengan benar.
Contoh: `Content-Type: text/html; charset=utf-8`

**Server**
Mengidentifikasi software server yang menangani request. Berguna untuk debugging, tapi sering disembunyikan karena bisa dimanfaatkan penyerang untuk memetakan infrastruktur target.
Contoh: `Server: nginx`

### 9.2 Common Response Headers

**Set-Cookie**
Mengirim cookie dari server ke browser untuk disimpan dan dikembalikan pada request berikutnya. Harus selalu disertai flag **HttpOnly** (tidak bisa diakses JavaScript) dan **Secure** (hanya dikirim via HTTPS).
Contoh: `Set-Cookie: sessionId=38af1337es7a8`

**Cache-Control**
Memberitahu browser berapa lama response boleh disimpan sebelum harus meminta ulang ke server. Gunakan `no-cache` untuk data sensitif.
Contoh: `Cache-Control: max-age=600`

**Location**
Digunakan dalam response 3xx untuk memberitahu browser ke mana harus redirect. Harus divalidasi ketat untuk mencegah **open redirect vulnerability**, di mana penyerang bisa mengarahkan pengguna ke situs berbahaya.
Contoh: `Location: /index.html`

### 9.3 Response Body

**HTTP response body** adalah tempat data aktual yang dikirim server ke client berada — HTML, JSON, gambar, atau format lainnya. Untuk mencegah **injection attacks** seperti XSS (Cross-Site Scripting), selalu lakukan sanitasi dan escaping terhadap data sebelum dimasukkan ke response, terutama konten yang berasal dari input pengguna.

---

## 10. Security Headers

Security headers adalah HTTP headers khusus yang ditambahkan ke response untuk memitigasi berbagai serangan web. Bisa dianalisis menggunakan situs **https://securityheaders.io/**.

### 10.1 Content-Security-Policy (CSP)

CSP mendefinisikan sumber-sumber konten yang diizinkan dimuat oleh browser. Memitigasi serangan XSS dan injeksi konten dari domain asing.

Contoh:

```
Content-Security-Policy: default-src 'self'; script-src 'self' https://cdn.tryhackme.com; style-src 'self'
```

**Properti CSP yang umum:**

**default-src** — Kebijakan default yang berlaku untuk semua tipe konten yang tidak punya direktif spesifik. Nilai `'self'` berarti hanya domain website itu sendiri.

**script-src** — Menentukan dari mana file JavaScript boleh dimuat.

**style-src** — Menentukan dari mana file CSS boleh dimuat.

Kata kunci `'self'` adalah nilai khusus yang merujuk pada domain yang sama tempat website di-host.

### 10.2 Strict-Transport-Security (HSTS)

HSTS memaksa browser untuk selalu terhubung ke website menggunakan HTTPS, tanpa pengecualian.

Contoh:

```
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

**Direktif HSTS:**

**max-age** — Durasi berlakunya kebijakan ini dalam detik.

**includeSubDomains** — Pengaturan opsional yang memperluas kebijakan ini ke semua subdomain website.

**preload** — Pengaturan opsional yang memungkinkan website masuk ke preload list browser, sehingga HSTS diterapkan bahkan sebelum kunjungan pertama ke website tersebut.

### 10.3 X-Content-Type-Options

Header ini mencegah browser melakukan **MIME sniffing** — yaitu menebak-nebak tipe konten berdasarkan isi file, bukan berdasarkan label Content-Type dari server.

Contoh:

```
X-Content-Type-Options: nosniff
```

**Direktif:**

**nosniff** — Instruksikan browser untuk tidak melakukan sniffing sama sekali; harus mematuhi Content-Type yang dinyatakan server.

### 10.4 Referrer-Policy

Mengontrol seberapa banyak informasi referrer (asal pengguna datang dari mana) yang dibagikan ke server tujuan saat terjadi redirect atau klik hyperlink.

Contoh konfigurasi yang tersedia:

```
Referrer-Policy: no-referrer
Referrer-Policy: same-origin
Referrer-Policy: strict-origin
Referrer-Policy: strict-origin-when-cross-origin
```

**Direktif Referrer-Policy:**

**no-referrer** — Tidak mengirim informasi referrer apapun ke server tujuan.

**same-origin** — Mengirim informasi referrer hanya jika tujuannya berada di domain yang sama. Tidak bocor ke domain eksternal.

**strict-origin** — Mengirim referrer hanya jika protokol tetap sama (HTTPS ke HTTPS). Tidak mengirim saat berpindah dari HTTPS ke HTTP.

**strict-origin-when-cross-origin** — Mengirim full URL path untuk same-origin requests, namun hanya mengirim origin (tanpa path) untuk cross-origin requests.

---

## 11. Glossary — Istilah Penting

**XSS (Cross-Site Scripting)** — Serangan injeksi script berbahaya ke dalam halaman web yang kemudian dieksekusi oleh browser pengguna lain.

**SQL Injection** — Serangan yang menyisipkan perintah SQL berbahaya ke dalam input yang diteruskan ke database, memungkinkan penyerang membaca, mengubah, atau menghapus data.

**MIME Type** — Label standar yang mengidentifikasi jenis dan format sebuah file, misalnya `text/html`, `image/jpeg`, `application/json`.

**MIME Sniffing** — Perilaku browser yang menebak MIME type berdasarkan isi konten, bukan berdasarkan header. Berbahaya karena bisa dieksploitasi.

**Typosquatting** — Praktik mendaftarkan domain yang hampir identik dengan domain populer namun ada kesalahan ejaan kecil, digunakan untuk menipu pengguna yang salah ketik.

**Open Redirect** — Kerentanan di mana penyerang dapat memanipulasi header Location untuk mengarahkan pengguna ke situs berbahaya.

**Clickjacking** — Serangan yang menipu pengguna untuk mengklik elemen tersembunyi dengan cara melapisi halaman palsu di atas halaman asli.

**HttpOnly Flag** — Flag pada cookie yang mencegah cookie diakses oleh JavaScript, sehingga tidak bisa dicuri lewat XSS.

**Secure Flag** — Flag pada cookie yang memastikan cookie hanya dikirim melalui koneksi HTTPS.

**Persistent Connection** — Koneksi HTTP yang tidak langsung ditutup setelah satu request, diperkenalkan di HTTP/1.1 untuk efisiensi komunikasi.

**Chunked Transfer Encoding** — Metode pengiriman data dalam potongan-potongan kecil (chunks) tanpa perlu menyatakan ukuran total di awal, diperkenalkan di HTTP/1.1.

**QUIC** — Protokol transport baru yang menggantikan TCP di HTTP/3, dirancang untuk koneksi yang lebih cepat dan lebih aman.

**no-cache** — Direktif Cache-Control yang menginstruksikan browser untuk tidak menyimpan response dalam cache.

**Preload List** — Daftar domain yang browser sudah tahu harus menggunakan HTTPS sebelum koneksi pertama terjadi.

**WAF** — Web Application Firewall, komponen opsional yang menyaring traffic berbahaya sebelum mencapai server.

**Boundary String** — String unik dalam format multipart/form-data yang digunakan sebagai pemisah antar blok data.

---

## 12. Tools & Platform Rujukan

**securityheaders.io**
Menganalisis security headers dari sebuah website — memberikan nilai dan laporan detail tentang header keamanan yang ada atau tidak ada.
URL: https://securityheaders.io/

---

## 13. Catatan Ringkas untuk Ditulis Tangan

### Web App — Komponen

- Front End — HTML + CSS + JS (yang terlihat user)
- Back End — Database + Infrastructure + WAF (di belakang layar)
- HTML — struktur halaman
- CSS — tampilan/gaya
- JS — logika & interaktivitas
- Database — simpan/ambil/ubah data
- Infrastructure — server, storage, networking
- WAF — filter request berbahaya (opsional)

### URL — Anatomi

- Scheme — http:// atau https://
- User — kredensial di URL (jarang, tidak aman)
- Host/Domain — nama website
- Port — :80 (HTTP), :443 (HTTPS), range 1-65535
- Path — /halaman/spesifik
- Query String — ?key=value (awali ?)
- Fragment — #bagian-halaman (awali #)
- Typosquatting — domain palsu mirip asli → phishing
- Query String & Fragment → bisa dimanipulasi → validasi wajib

### HTTP Messages — Struktur

- Request — dari user ke server
- Response — dari server ke user
- Start Line — baris pertama, identitas pesan
- Headers — key-value pairs, info tambahan
- Empty Line — pemisah wajib header & body
- Body — data aktual

### HTTP Methods

- GET — ambil data, jangan taruh credential di URL
- POST — kirim data baru, validasi input
- PUT — ganti resource total, cek otorisasi
- DELETE — hapus resource, hanya user berwenang
- PATCH — update sebagian resource
- HEAD — seperti GET tapi header saja, tanpa body
- OPTIONS — tanya method apa yang tersedia di server
- TRACE — debugging, sebaiknya dinonaktifkan
- CONNECT — buat tunnel aman (untuk HTTPS)

### HTTP Versi

- HTTP/0.9 (1991) — hanya GET
- HTTP/1.0 (1996) — tambah headers
- HTTP/1.1 (1997) — persistent connection, chunked encoding (PALING UMUM)
- HTTP/2 (2015) — multiplexing, header compression
- HTTP/3 (2022) — pakai QUIC, lebih cepat

### Request Headers

- Host — nama server tujuan
- User-Agent — info browser pengirim
- Referer — URL asal sebelum request
- Cookie — data yang sebelumnya disimpan server
- Content-Type — format data di body

### Format Request Body

- URL Encoded — key=value&key=value (form sederhana)
- Form Data (multipart) — blok dipisah boundary (upload file)
- JSON — {"key": "value"} (API modern)
- XML — tag pembuka penutup bisa nested (sistem lama)

### Status Codes

- 1xx — Informational (keep going)
- 2xx — Successful (berhasil)
- 3xx — Redirection (pindah lokasi)
- 4xx — Client Error (kesalahan user)
- 5xx — Server Error (kesalahan server)
- 100 — Continue
- 200 — OK
- 301 — Moved Permanently
- 404 — Not Found
- 500 — Internal Server Error

### Response Headers

- Date — waktu response dibuat
- Content-Type — jenis konten + charset
- Server — software server (sering disembunyikan)
- Set-Cookie — kirim cookie ke browser (pakai HttpOnly + Secure)
- Cache-Control — durasi cache (no-cache untuk data sensitif)
- Location — tujuan redirect (validasi ketat, cegah open redirect)

### Security Headers

- CSP — batasi sumber konten yang boleh dimuat
  - default-src — default semua tipe
  - script-src — sumber JS
  - style-src — sumber CSS
  - 'self' — hanya domain sendiri
- HSTS — paksa selalu HTTPS
  - max-age — durasi berlaku (detik)
  - includeSubDomains — berlaku ke subdomain
  - preload — masuk preload list browser
- X-Content-Type-Options — cegah MIME sniffing
  - nosniff — browser dilarang tebak MIME type
- Referrer-Policy — kontrol info referrer yang dibagikan
  - no-referrer — tidak kirim info apapun
  - same-origin — hanya kirim ke domain sama
  - strict-origin — hanya kirim jika protokol sama
  - strict-origin-when-cross-origin — full URL sama-origin, origin saja cross-origin

### Istilah Kunci

- XSS — injeksi script berbahaya ke halaman
- SQL Injection — injeksi perintah SQL ke input
- MIME Sniffing — browser tebak tipe file sendiri
- Typosquatting — domain palsu dengan typo
- Open Redirect — manipulasi Location header ke situs jahat
- Clickjacking — tipu klik elemen tersembunyi
- HttpOnly — cookie tidak bisa diakses JS
- Secure Flag — cookie hanya lewat HTTPS
- Persistent Connection — koneksi tidak putus per request
- Chunked Encoding — kirim data dalam potongan
- QUIC — protokol baru di HTTP/3
- WAF — firewall khusus web application
