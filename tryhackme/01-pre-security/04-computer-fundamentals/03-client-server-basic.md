# Resume Materi: Networking Fundamentals
**Platform:** TryHackMe | **Tanggal:** 29 Agustus 2026

---

## 1. Sejarah dan Latar Belakang Jaringan

Pada awalnya, komputer bekerja secara **standalone** — menyimpan file sendiri, menjalankan program sendiri, dan tidak berkomunikasi dengan komputer lain.

Seiring waktu, berbagai organisasi di seluruh dunia mulai menghubungkan sistem-sistem tersebut dengan dua tujuan utama:

- **Information exchange** — bertukar data antar komputer tanpa batasan jarak
- **Resource sharing** — berbagi sumber daya komputasi antar sistem

Dari sinilah lahir cikal bakal internet modern, melalui jaringan-jaringan awal berikut:

| Jaringan | Asal |
|---|---|
| **ARPANET** | Departemen Pertahanan Amerika Serikat |
| **CYCLADES** | Lembaga riset Prancis |
| **NPL** | National Physical Laboratory, Inggris |
| **NSFNET** | National Science Foundation, Amerika |

Sejalan dengan perkembangan ini, komputer yang terhubung mulai **berspesialisasi** — satu komputer bisa menyediakan layanan tertentu dan komputer lain bisa menggunakannya. Inilah dasar dari model **Client-Server**.

---

## 2. Konsep Inti Jaringan

### 2.1 Client dan Server

**Client** adalah pihak yang **meminta** layanan atau data. Contoh: browser di komputermu.

**Server** adalah pihak yang **melayani** dan mengirimkan data. Contoh: server tempat website di-hosting.

Aturan yang tidak boleh dilupakan: **client selalu yang pertama memulai request**, bukan server.

### 2.2 Request dan Response

**Request** adalah permintaan yang dikirim client ke server.

**Response** adalah jawaban yang dikirim server kembali ke client.

Kondisi penting: jika request tidak diformat dengan benar, atau resource yang diminta tidak tersedia, server akan mengirim **error response** — bukan data yang diminta. Contoh: "No pepperoni pizza available" dalam analogi, atau status `404 Not Found` dalam HTTP.

### 2.3 Protocol

**Protocol** adalah seperangkat aturan dan standar komunikasi yang disepakati agar dua sistem bisa saling bertukar data.

Protocol mendefinisikan:

- Perintah/method apa yang dipahami client dan server (contoh: `GET`)
- Bagaimana request disusun dan diurutkan
- Sintaks atau bahasa yang digunakan
- Respons yang diberikan untuk request yang valid
- Respons yang diberikan untuk request yang salah atau tidak dapat diproses

### 2.4 Port

**Port** adalah angka numerik yang mengidentifikasi **layanan spesifik** yang berjalan di sebuah sistem. Ketika client ingin mengakses layanan tertentu di server, client harus terhubung ke **port yang benar**.

Satu server dapat menjalankan **banyak layanan secara bersamaan**, masing-masing diidentifikasi oleh nomor port yang berbeda. Analogi mudahnya: seperti satu gedung restoran dengan tiga pintu berbeda — satu untuk takeaway, satu untuk makan di tempat, satu untuk delivery.

### 2.5 DNS

**DNS (Domain Name Service)** adalah sistem yang menerjemahkan **nama domain** yang mudah dibaca manusia menjadi **alamat IP** yang digunakan komputer untuk menemukan server.

Tanpa DNS, kamu harus menghafal alamat IP (contoh: `142.250.190.78`) setiap kali ingin mengakses sebuah website alih-alih cukup mengetik `google.com`.

**IP address** adalah alamat unik yang dimiliki setiap sistem di jaringan — seperti alamat rumah (nama jalan, nomor, kota, negara), tapi untuk sistem komputer.

---

## 3. HTTP dan HTTPS

**HTTP (Hypertext Transfer Protocol)** dan **HTTPS (HTTP Secure)** adalah protokol **client-server stateless** yang digunakan untuk komunikasi di World Wide Web.

**Stateless** berarti setiap request diproses secara **independen** — server tidak menyimpan memori tentang request sebelumnya dari client yang sama.

Untuk mengatasi keterbatasan ini, website modern mengimplementasikan **mekanisme statefulness** di level aplikasi, seperti:

- **Session identifier** yang dibuat server saat login
- Disimpan dalam **cookie atau token** di sisi client
- Dikirim bersama setiap request berikutnya agar server mengenali sesi yang sedang aktif

Tanpa mekanisme ini, pengguna harus melakukan autentikasi ulang di setiap request baru.

---

## 4. HTTP Methods

Spesifikasi resmi HTTP (dokumen yang disebut **RFC — Request for Comments**) mendefinisikan **9 core methods**. Dalam terminologi HTTP, perintah disebut **method**, bukan command.

| Method | Fungsi |
|---|---|
| **GET** | Mengambil/retrieve resource dari server |
| **POST** | Mengirim data baru ke server |
| **PUT** | Menggantikan/update resource di server secara penuh |
| **DELETE** | Menghapus resource di server |
| **PATCH** | Memperbarui sebagian dari resource di server |
| **HEAD** | Sama seperti GET, tetapi hanya mengambil header (tanpa body) |
| **OPTIONS** | Menanyakan method apa saja yang didukung server |
| **CONNECT** | Membangun tunnel ke server |
| **TRACE** | Loop-back test untuk keperluan debugging |

Method yang paling umum digunakan dalam browsing sehari-hari adalah **GET** dan **POST**.

---

## 5. Method GET — Detail

**GET** digunakan untuk **mengambil resource dari web server**. Ketika kamu mengetik URL di browser, browser secara otomatis membangun dan mengirim GET request di balik layar — kamu tidak perlu menulis request-nya secara manual.

### 5.1 Alur HTTP Communication

```
CLIENT (browser)
    |
    |--- 1. HTTP REQUEST (GET /index.html) --->
    |                                        TRYHACKME SERVER
    |<-- 2. HTTP RESPONSE (200 OK + konten) ---
    |
  Halaman tampil
```

Semua komunikasi ini berjalan melalui internet.

### 5.2 Field Penting dalam GET Request

Setiap GET request memiliki field-field berikut yang bisa dilihat di browser Developer Tools:

- **Scheme** — protokol yang digunakan: `HTTP` atau `HTTPS`
- **Host** — nama host atau server yang dituju (contoh: `httpdemo.local:8080`)
- **Filename** — file spesifik yang diminta dari host; nilai `/` diterjemahkan menjadi `index.html`
- **Address** — alamat IP tempat server di-hosting (contoh: `127.0.0.1` untuk lokal)
- **Status** — menunjukkan apakah request berhasil atau tidak (contoh: `200 OK`)

---

## 6. HTTP Status Codes

Status code adalah angka tiga digit yang dikirim server dalam response untuk menunjukkan hasil dari sebuah request.

| Kode | Arti | Keterangan |
|---|---|---|
| **200** | OK | Request berhasil diproses |
| **404** | Not Found | Resource yang diminta tidak ditemukan |

---

## 7. Struktur HTTP Response

Setiap response dari server terdiri dari **dua bagian**:

**Response Header** berisi metadata tentang respons — bukan konten utamanya, melainkan informasi pendukung seperti:

- `Content-Type` — tipe konten yang dikirim (contoh: `text/html`)
- `Content-Length` — ukuran konten dalam bytes
- `Date` — waktu respons dikirim
- `Last-Modified` — waktu terakhir resource dimodifikasi
- `Server` — informasi software server

**Response Body** berisi konten aktual yang diminta — misalnya kode HTML lengkap dari halaman website yang diakses.

---

## 8. Inspeksi Request dengan Browser Developer Tools

Browser modern menyediakan **Developer Tools** untuk memeriksa, mendebug, dan menganalisis request dan response secara langsung.

**Cara membuka:** tekan `F12` atau klik kanan di halaman → pilih **Inspect**

**Tab yang digunakan untuk inspeksi network:**

Buka tab **Network**, lalu reload halaman. Setiap request yang dibuat browser akan muncul sebagai entri. Klik salah satu entri untuk melihat detailnya di panel kanan.

Sub-tab dalam panel detail:

- **Headers** — berisi informasi request dan response header
- **Cookies** — cookie yang dikirim atau diterima
- **Request** — isi lengkap dari request yang dikirim client
- **Response** — isi response body (contoh: kode HTML)
- **Timings** — durasi waktu tiap tahap request

Saat membuka `http://httpdemo.local:8080`, browser tidak hanya membuat satu request — ia membuat beberapa GET request sekaligus untuk semua resource yang dibutuhkan halaman:

- `/` → dokumen HTML utama (`index.html`) → status `200`
- `style.css` → stylesheet → status `200`
- `script.js` → JavaScript → status `200`
- `favicon.ico` → ikon browser → status `404` (tidak ditemukan)

Ini menggambarkan bahwa satu kali kunjungan ke sebuah halaman web bisa memicu **banyak GET request** secara bersamaan.

---

## 9. Glossary — Istilah Wajib Dihapal

| Istilah | Definisi |
|---|---|
| **Client** | Pihak yang memulai dan mengirim request ke server |
| **Server** | Pihak yang menerima request dan mengirim response |
| **Request** | Permintaan yang dikirim client ke server |
| **Response** | Jawaban yang dikirim server ke client |
| **Protocol** | Aturan standar komunikasi antar sistem |
| **Port** | Nomor yang mengidentifikasi layanan spesifik di server |
| **DNS** | Sistem yang menerjemahkan nama domain menjadi IP address |
| **IP Address** | Alamat unik setiap sistem di jaringan |
| **HTTP** | Hypertext Transfer Protocol — protokol komunikasi web |
| **HTTPS** | HTTP Secure — versi HTTP yang dienkripsi |
| **Stateless** | Setiap request diproses independen tanpa memori sesi sebelumnya |
| **Session** | Sesi aktif yang dilacak server menggunakan cookie atau token |
| **HTTP Method** | Perintah dalam protokol HTTP yang menentukan jenis aksi (GET, POST, dll) |
| **Status Code** | Kode angka dalam response yang menunjukkan hasil request |
| **Response Header** | Metadata yang menyertai response, bukan konten utama |
| **Response Body** | Konten aktual yang dikirim server (HTML, JSON, dll) |
| **RFC** | Request for Comments — dokumen spesifikasi resmi standar internet |
| **Standalone** | Komputer yang bekerja mandiri tanpa koneksi ke komputer lain |
| **Resource Sharing** | Berbagi sumber daya komputasi antar sistem yang terhubung |
| **Developer Tools** | Tool bawaan browser untuk inspeksi dan debugging web request |

---

## 10. Tools & Platform Rujukan

| Nama | Fungsi |
|---|---|
| **Firefox Developer Tools** | Inspeksi HTTP request dan response langsung dari browser; akses via F12 → tab Network |
| **TryHackMe** | Platform belajar cybersecurity berbasis room dan virtual machine | https://tryhackme.com |
