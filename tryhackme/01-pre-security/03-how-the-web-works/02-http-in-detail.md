# Resume Materi: HTTP in Detail (TryHackMe)
**Tanggal:** 01 September 2026

---

## 1. Dasar Protokol HTTP

### 1.1 Apa itu HTTP

**HTTP** (HyperText Transfer Protocol) adalah protokol yang dipakai browser setiap kali mengakses website. Dikembangkan oleh Tim Berners-Lee bersama timnya antara 1989–1991.

HTTP adalah seperangkat aturan untuk berkomunikasi dengan web server dalam mentransmisikan data webpage — HTML, gambar, video, dan resource lain.

### 1.2 Apa itu HTTPS

**HTTPS** (HyperText Transfer Protocol Secure) adalah versi aman dari HTTP. Data yang dikirim dienkripsi, sehingga punya dua manfaat utama:

- Mencegah pihak lain melihat data yang dikirim/diterima.
- Memberi jaminan bahwa client benar-benar berkomunikasi dengan server yang sah, bukan server palsu yang menyamar.

HTTP mengirim data dalam bentuk teks polos yang bisa dibaca siapa saja yang mencegat komunikasinya — inilah alasan website modern, terutama yang menangani data sensitif (password, pembayaran), wajib memakai HTTPS.

---

## 2. Struktur URL (Uniform Resource Locator)

URL adalah instruksi tentang cara mengakses suatu resource di internet. Tidak semua fitur URL selalu dipakai dalam satu request, tapi struktur lengkapnya terdiri dari:

- **Scheme** — protokol yang dipakai untuk mengakses resource, misalnya `HTTP`, `HTTPS`, atau `FTP` (File Transfer Protocol).
- **User** — username dan password untuk login, disisipkan langsung di URL jika layanan memerlukan autentikasi.
- **Host/Domain** — nama domain atau alamat IP dari server yang ingin diakses.
- **Port** — port tujuan koneksi. Default-nya 80 untuk HTTP dan 443 untuk HTTPS, tapi bisa di-host di port mana saja antara 1–65535.
- **Path** — nama file atau lokasi resource yang ingin diakses di server.
- **Query String** — informasi tambahan yang dikirim ke path yang diminta. Contoh: `/blog?id=1` memberitahu path `/blog` untuk mengembalikan artikel dengan id 1.
- **Fragment** — referensi ke lokasi tertentu di dalam halaman yang diminta. Umum dipakai pada halaman dengan konten panjang agar bagian tertentu bisa langsung ditautkan.

Contoh URL lengkap dengan semua komponen:

```
http://user:password@tryhackme.com:80/view-room?id=1#task3
```

---

## 3. Request dan Response

### 3.1 Membuat Request

Request paling sederhana hanya butuh satu baris: `GET / HTTP/1.1`. Baris ini terdiri dari tiga bagian:

- **Request Method** — aksi yang diminta (GET).
- **Halaman yang diminta** — path resource (`/`).
- **Versi Protokol HTTP** — versi HTTP yang dipakai (`HTTP/1.1`).

Untuk kebutuhan yang lebih kompleks, data tambahan dikirim lewat **headers** (dibahas di bagian 5).

### 3.2 Contoh Request Lengkap

```
GET / HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: https://tryhackme.com/
```

Penjelasan tiap baris:

- **Baris 1** — mengirim method GET, meminta homepage (`/`), memberitahu server memakai protokol HTTP versi 1.1.
- **Baris 2** — memberitahu server bahwa website yang diminta adalah `tryhackme.com`.
- **Baris 3** — memberitahu server bahwa client menggunakan browser Firefox versi 87.
- **Baris 4** — memberitahu server bahwa halaman yang merujuk ke request ini adalah `https://tryhackme.com/`.
- **Baris terakhir (kosong)** — request HTTP selalu diakhiri baris kosong sebagai penanda request sudah selesai.

### 3.3 Contoh Response Lengkap

```
HTTP/1.1 200 OK
Server: nginx/1.15.8
Date: Fri, 09 Apr 2021 13:34:03 GMT
Content-Type: text/html
Content-Length: 98

<html>
<head>
    <title>TryHackMe</title>
</head>
<body>
    Welcome To TryHackMe.com
</body>
</html>
```

Penjelasan tiap baris:

- **Baris 1** — versi protokol HTTP yang dipakai server, diikuti status code (`200 OK` = request selesai dengan sukses).
- **Baris 2** — software web server beserta nomor versinya.
- **Baris 3** — tanggal, waktu, dan timezone server saat itu.
- **Baris 4** — Content-Type memberitahu client jenis data yang dikirim (HTML, images, videos, pdf, XML, dll).
- **Baris 5** — Content-Length memberitahu panjang response, sebagai konfirmasi tidak ada data yang hilang.
- **Baris kosong** — menandai akhir bagian header response.
- **Baris-baris sisanya** — informasi yang diminta, dalam contoh ini isi homepage.

---

## 4. HTTP Methods

Method adalah cara client menunjukkan aksi yang dimaksud saat membuat request. Ada banyak method, tapi yang paling sering dipakai adalah GET dan POST.

- **GET** — mengambil informasi dari web server.
- **POST** — mengirim data ke web server, berpotensi membuat record baru.
- **PUT** — mengirim data ke web server untuk memperbarui informasi yang sudah ada.
- **DELETE** — menghapus informasi/record dari web server.

Pemetaan ke operasi data (CRUD):

| Method | Operasi |
|---|---|
| GET | Read (baca) |
| POST | Create (buat baru) |
| PUT | Update (ubah) |
| DELETE | Delete (hapus) |

---

## 5. HTTP Status Codes

Baris pertama response HTTP selalu berisi status code, yang memberitahu client hasil dari request mereka dan potensi cara menanganinya. Status code terbagi ke dalam 5 rentang:

- **100–199 — Informational** — memberitahu bahwa bagian pertama request sudah diterima dan client harus melanjutkan mengirim sisa request. Sudah tidak umum dipakai.
- **200–299 — Success** — memberitahu bahwa request berhasil diproses.
- **300–399 — Redirection** — mengarahkan request client ke resource lain, baik halaman lain di website yang sama atau website yang sepenuhnya berbeda.
- **400–499 — Client Errors** — memberitahu bahwa ada kesalahan pada request yang dikirim client.
- **500–599 — Server Errors** — dikhususkan untuk error di sisi server, biasanya menandakan masalah yang cukup serius dalam menangani request.

### 5.1 Status Code yang Umum Ditemui

- **200 OK** — request selesai dengan sukses.
- **201 Created** — sebuah resource berhasil dibuat (misalnya user baru atau post blog baru).
- **301 Moved Permanently** — mengarahkan browser client ke halaman baru, atau memberitahu search engine bahwa halaman sudah pindah secara permanen.
- **302 Found** — mirip redirect permanen di atas, tapi sifatnya sementara dan bisa berubah lagi.
- **400 Bad Request** — ada yang salah atau hilang pada request; kadang terjadi ketika resource yang diminta mengharapkan parameter tertentu yang tidak dikirim client.
- **401 Not Authorised** — client belum diizinkan melihat resource sampai berhasil autentikasi ke aplikasi, umumnya lewat username dan password.
- **403 Forbidden** — client tidak punya izin melihat resource ini, baik sedang login maupun tidak.
- **405 Method Not Allowed** — resource tidak menerima method yang dikirim, contohnya mengirim GET request ke `/create-account` padahal resource tersebut mengharapkan POST.
- **404 Page Not Found** — halaman/resource yang diminta tidak ada.
- **500 Internal Service Error** — server mengalami error dalam menangani request dan tidak tahu cara menanganinya dengan benar.
- **503 Service Unavailable** — server tidak bisa menangani request karena sedang overload atau dalam maintenance.

Aplikasi bahkan bisa mendefinisikan status code sendiri, jadi variasi yang ditemukan di lapangan bisa lebih banyak dari daftar umum di atas.

---

## 6. HTTP Headers

Headers adalah bit data tambahan yang dikirim ke web server saat membuat request. Meski tidak ada header yang benar-benar wajib untuk membuat request HTTP, tanpa header yang tepat website sulit ditampilkan dengan benar.

### 6.1 Common Request Headers

Dikirim dari client (browser) ke server:

- **Host** — memberitahu server domain mana yang diminta, karena satu server bisa meng-host banyak website. Tanpa header ini, client hanya akan menerima website default server.
- **User-Agent** — software browser beserta versinya. Membantu server memformat website sesuai kemampuan browser tersebut, karena sebagian elemen HTML/JavaScript/CSS hanya tersedia di browser tertentu.
- **Content-Length** — saat mengirim data (misalnya lewat form), header ini memberitahu server berapa banyak data yang harus diterima, sehingga server bisa memastikan tidak ada data yang hilang.
- **Accept-Encoding** — memberitahu server metode kompresi apa yang didukung browser, agar data bisa dikirim lebih kecil.
- **Cookie** — data yang dikirim ke server untuk membantu server mengingat informasi tentang client (lihat bagian 7).

### 6.2 Common Response Headers

Dikirim dari server kembali ke client setelah request:

- **Set-Cookie** — informasi yang harus disimpan client dan dikirim balik ke server pada setiap request selanjutnya (lihat bagian 7).
- **Cache-Control** — berapa lama konten response boleh disimpan di cache browser sebelum diminta ulang ke server.
- **Content-Type** — jenis data yang dikembalikan (HTML, CSS, JavaScript, Images, PDF, Video, dll), sehingga browser tahu cara memproses data tersebut.
- **Content-Encoding** — metode kompresi yang dipakai untuk memperkecil ukuran data saat dikirim lewat internet.

---

## 7. Cookies

Cookie adalah potongan data kecil yang disimpan di komputer client. Cookie tersimpan ketika client menerima header **Set-Cookie** dari server, lalu pada setiap request selanjutnya client mengirim balik data cookie tersebut ke server.

Karena HTTP bersifat **stateless** (tidak menyimpan riwayat request sebelumnya), cookie dipakai untuk mengingatkan server siapa client tersebut, pengaturan personal untuk website, atau apakah client pernah mengunjungi website sebelumnya.

### 7.1 Alur Kerja Cookie

1. Client meminta halaman: `GET / HTTP/1.1`.
2. Server merespons dengan halaman berisi form yang meminta nama user.
3. Client mengirim balik form tersebut lewat `POST / HTTP/1.1` dengan body `name=adam`.
4. Server merespons dengan header `Set-Cookie: name=adam`, memerintahkan client menyimpan data tersebut.
5. Pada request berikutnya, client mengirim kembali `Cookie: name=adam` ke server.
6. Server membaca cookie tersebut dan langsung menampilkan pesan "Welcome back adam", tanpa menampilkan form lagi.

### 7.2 Kegunaan dan Cara Melihat Cookie

Cookie paling umum dipakai untuk **autentikasi website**. Nilai cookie biasanya bukan string teks yang bisa langsung dibaca sebagai password, melainkan sebuah **token** — kode rahasia unik yang tidak mudah ditebak.

Cookie yang dikirim browser bisa dilihat lewat developer tools browser, pada tab **Network**. Tab ini menampilkan daftar semua resource yang diminta browser; setiap request bisa diklik untuk melihat detail request dan response-nya, termasuk cookie yang dikirim pada tab **Cookies**.

---

## 8. Path Parameter vs Query Parameter

Ada dua cara umum mengirim parameter identifikasi resource ke server lewat URL.

### 8.1 Path Parameter

ID menjadi bagian dari path/segmen URL itu sendiri, contoh: `/user/1`, `/blog/5`. Format ini dikenal sebagai URL yang RESTful/clean, umum dipakai untuk mengidentifikasi satu resource spesifik secara langsung dan mengikuti struktur hierarki yang jelas (misalnya `/user` lalu `/1` sebagai user tertentu).

### 8.2 Query Parameter

Parameter dikirim setelah tanda `?` dalam bentuk `key=value`, contoh: `/user?id=1`, `/blog?id=5&author=john`. Cara ini lebih cocok dipakai untuk kebutuhan filtering, searching, atau parameter yang sifatnya opsional — karena bisa menambah banyak parameter sekaligus tanpa mengubah struktur path.

Kedua cara sama-sama valid; pemilihannya tergantung tujuan — path parameter untuk mengidentifikasi resource utama, query parameter untuk data tambahan/opsional di luar identitas resource itu sendiri.

---

## Istilah Penting

- **HTTP** — protokol untuk komunikasi antara client dan web server dalam mentransmisikan data webpage.
- **HTTPS** — versi HTTP yang datanya dienkripsi dan memberi jaminan keaslian server.
- **URL** — instruksi lengkap tentang cara mengakses suatu resource di internet.
- **Scheme** — bagian URL yang menentukan protokol akses (HTTP/HTTPS/FTP).
- **Host** — domain atau IP server tujuan dalam URL.
- **Path** — lokasi file/resource yang diminta di server.
- **Query String** — parameter tambahan setelah `?` pada URL.
- **Fragment** — referensi ke lokasi tertentu dalam halaman, ditandai `#`.
- **Request** — permintaan yang dikirim client ke server.
- **Response** — jawaban yang dikirim server ke client atas suatu request.
- **Header** — data tambahan pada request atau response, berisi informasi pendukung komunikasi.
- **Status Code** — kode 3 digit pada awal response yang menandakan hasil dari request.
- **Method** — aksi yang diminta client dalam sebuah request (GET, POST, PUT, DELETE, dll).
- **Stateless** — sifat HTTP yang tidak menyimpan riwayat/konteks dari request sebelumnya.
- **Cookie** — data kecil yang disimpan di sisi client untuk membantu server mengingat informasi tentang client tersebut.
- **Token** — kode rahasia unik sebagai representasi identitas/otorisasi, biasanya menjadi isi dari cookie autentikasi.
- **Path Parameter** — parameter identifikasi resource yang menyatu dengan path URL.
- **Query Parameter** — parameter tambahan/opsional yang dikirim lewat query string URL.

---

## Tools & Platform Rujukan

- **http.cat** — resource visual untuk mempelajari HTTP status code, memetakan tiap kode ke gambar kucing yang merepresentasikan maknanya.

---

## Catatan Ringkas untuk Ditulis Tangan

**HTTP/HTTPS**
- HTTP — protokol transmisi data web, dibuat Tim Berners-Lee 1989-1991
- HTTPS — HTTP + enkripsi, jamin data aman & server asli

**Struktur URL**
- Scheme — protokol (http/https/ftp)
- User — username:password untuk login
- Host — domain/IP server
- Port — default 80 (HTTP), 443 (HTTPS), range 1-65535
- Path — lokasi resource
- Query String — parameter tambahan setelah ?
- Fragment — lokasi spesifik di halaman, pakai #

**Request**
- Format dasar: METHOD /path HTTP/version
- Host — website yang diminta
- User-Agent — info browser & versi
- Referer — halaman asal request
- Baris kosong di akhir — penanda request selesai

**Response**
- Baris 1 — versi HTTP + status code
- Server — software & versi web server
- Content-Type — jenis data yang dikirim
- Content-Length — ukuran data response

**HTTP Methods**
- GET — ambil data (Read)
- POST — kirim data, buat record baru (Create)
- PUT — update data yang ada (Update)
- DELETE — hapus data (Delete)

**Status Code — Kategori**
- 100-199 — Informational
- 200-299 — Success
- 300-399 — Redirection
- 400-499 — Client Error
- 500-599 — Server Error

**Status Code — Umum**
- 200 OK — sukses
- 201 Created — resource baru dibuat
- 301 Moved Permanently — pindah permanen
- 302 Found — pindah sementara
- 400 Bad Request — request salah/tidak lengkap
- 401 Not Authorised — belum autentikasi
- 403 Forbidden — tidak punya izin
- 404 Page Not Found — resource tidak ada
- 405 Method Not Allowed — method salah untuk resource ini
- 500 Internal Service Error — server gagal proses
- 503 Service Unavailable — server overload/maintenance

**Headers — Request (client ke server)**
- Host — website tujuan
- User-Agent — browser & versi
- Content-Length — ukuran data dikirim
- Accept-Encoding — kompresi yang didukung browser
- Cookie — data tersimpan dikirim balik ke server

**Headers — Response (server ke client)**
- Set-Cookie — perintah simpan data di client
- Cache-Control — durasi simpan cache
- Content-Type — jenis data response
- Content-Encoding — metode kompresi data

**Cookies**
- Stateless — HTTP tidak ingat request sebelumnya
- Set-Cookie (response) — server suruh simpan data
- Cookie (request) — client kirim balik data tersimpan
- Nilai cookie umumnya token, bukan password polos
- Lihat cookie — DevTools > Network > pilih request > tab Cookies

**Path vs Query Parameter**
- Path parameter — /user/1, identifikasi resource langsung
- Query parameter — /user?id=1, untuk filter/search/opsional

**Istilah kunci**
- Request — permintaan client ke server
- Response — jawaban server ke client
- Method — aksi yang diminta (GET/POST/PUT/DELETE)
- Status Code — kode hasil request
- Stateless — tidak ingat konteks sebelumnya
- Token — kode rahasia identitas/otorisasi
