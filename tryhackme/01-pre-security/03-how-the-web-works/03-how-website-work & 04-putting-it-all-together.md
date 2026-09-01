# Resume Materi: How Websites Work
**Tanggal:** 31 Januari 2026

---

## 1. Konsep Dasar Cara Kerja Website

Ketika browser mengunjungi sebuah website, terjadi pertukaran data antara dua pihak: **browser** (client) dan **web server**.

### 1.1 Request dan Response

Browser mengirim **request** ke web server untuk meminta halaman tertentu. Server memproses permintaan itu dan mengembalikan **response** berupa data (HTML, CSS, JavaScript, gambar, dll) yang kemudian dirender oleh browser menjadi tampilan halaman.

Web server sendiri hanyalah komputer khusus di tempat lain yang bertugas menangani request tersebut.

### 1.2 Dua Komponen Utama Website

- **Front End (client-side)** — bagian yang dirender dan ditampilkan oleh browser; ini yang dilihat dan diinteraksikan langsung oleh pengguna.
- **Back End (server-side)** — server yang memproses request dan mengembalikan response.

---

## 2. HTML (HyperText Markup Language)

HTML adalah bahasa dasar penyusun struktur website. **Elemen (tag)** adalah building block dari halaman HTML dan memberi tahu browser bagaimana konten harus ditampilkan.

### 2.1 Struktur Dasar Dokumen HTML

Struktur berikut sama untuk setiap website:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Page Title</title>
    </head>
    <body>
        <h1>Example Heading</h1>
        <p>Example paragraph..</p>
    </body>
</html>
```

- `<!DOCTYPE html>` — mendeklarasikan halaman sebagai dokumen HTML5, membantu standarisasi antar browser.
- `<html>` — root element; semua elemen lain berada di dalamnya.
- `<head>` — berisi informasi tentang halaman, seperti judul (title).
- `<body>` — mendefinisikan isi dokumen; hanya konten di dalam body yang tampil di browser.
- `<h1>` — mendefinisikan heading besar.
- `<p>` — mendefinisikan paragraf.

Elemen lain yang umum digunakan untuk keperluan berbeda: `<button>` (tombol), `<img>` (gambar), serta berbagai tag untuk list dan form.

### 2.2 Attributes (Atribut)

Tag dapat memiliki atribut untuk mengatur perilaku atau tampilannya. Satu elemen bisa punya lebih dari satu atribut sekaligus.

- **class** — dipakai untuk styling elemen; boleh dipakai berulang di banyak elemen. Contoh: `<p class="bold-text">`
- **src** — menentukan lokasi sebuah resource, misalnya gambar. Contoh: `<img src="img/cat.jpg">`
- **id** — identifier unik milik satu elemen (tidak boleh dipakai ulang); digunakan untuk styling dan untuk diakses oleh JavaScript. Contoh: `<p id="example">`

Source code HTML sebuah website bisa dilihat dengan klik kanan lalu pilih "View Page Source" (Chrome) atau "Show Page Source" (Safari).

---

## 3. CSS dan JavaScript sebagai Pelengkap HTML

Selain HTML, dua teknologi lain yang membentuk website:

- **CSS** — mengatur styling agar website terlihat menarik.
- **JavaScript** — mengimplementasikan interaktivitas dan fitur kompleks pada halaman.

### 3.1 Peran JavaScript

JavaScript mengontrol fungsionalitas halaman web. Tanpa JavaScript, halaman tidak akan punya elemen interaktif dan selalu bersifat statis. JavaScript dapat mengubah halaman secara real-time — misalnya mengubah style tombol saat diklik, atau menjalankan animasi.

### 3.2 Cara Menyisipkan JavaScript

JavaScript bisa ditulis langsung di dalam tag `<script>`, atau dimuat dari file eksternal lewat atribut `src`:

```html
<script src="/location/of/javascript_file.js"></script>
```

### 3.3 Contoh Penggunaan

Mengubah konten sebuah elemen berdasarkan id-nya:

```javascript
document.getElementById("demo").innerHTML = "Hack the Planet";
```

Menjalankan JavaScript saat sebuah **event** terjadi pada elemen, misalnya `onclick`:

```html
<button onclick='document.getElementById("demo").innerHTML = "Button Clicked";'>Click Me!</button>
```

Event seperti `onclick` atau `onhover` bisa didefinisikan langsung pada atribut elemen, atau didefinisikan terpisah di dalam tag `<script>`.

---

## 4. Sensitive Data Exposure

**Sensitive Data Exposure** terjadi ketika website tidak melindungi (atau lupa menghapus) informasi sensitif yang tampil dalam bentuk clear-text kepada end-user — paling sering ditemukan di source code frontend.

### 4.1 Bagaimana Ini Terjadi

Karena seluruh elemen HTML sebuah halaman bisa dilihat siapa pun lewat "view page source", developer yang lupa membersihkan kode sebelum deploy bisa meninggalkan jejak berupa login credentials, hidden links ke bagian privat aplikasi, atau data sensitif lain di dalam HTML maupun JavaScript.

Contoh nyata: kredensial sementara yang tertinggal dalam HTML comment.

```html
<!-- TODO: remove test credentials admin:password123 -->
```

### 4.2 Dampak dan Mitigasi

Informasi sensitif yang bocor bisa dimanfaatkan penyerang untuk memperluas akses — misalnya login ke bagian lain aplikasi atau mengakses komponen backend.

Karena itu, salah satu langkah pertama saat menilai keamanan sebuah web application adalah mereview page source code untuk mencari kredensial atau link tersembunyi yang seharusnya tidak terekspos.

---

## 5. HTML Injection

**HTML Injection** adalah kerentanan yang muncul ketika input pengguna yang tidak difilter (unfiltered user input) ditampilkan kembali di halaman. Jika website gagal mensanitasi input, penyerang bisa menyuntikkan kode HTML atau JavaScript ke halaman yang rentan.

### 5.1 Mengapa Input Sanitisation Penting

Input yang dimasukkan pengguna sering digunakan kembali di frontend maupun backend. Ketika pengguna punya kontrol atas bagaimana inputnya ditampilkan, ia bisa mengirim kode HTML/JavaScript yang akan dieksekusi apa adanya oleh browser — memberi kontrol atas tampilan dan fungsionalitas halaman kepada penyerang.

Kerentanan sejenis pada sisi database disebut **database injection**: penyerang memanipulasi query lookup database untuk login sebagai user lain, dengan mengontrol input yang langsung dipakai dalam query tersebut.

### 5.2 Alur Terjadinya HTML Injection

1. User mengetik informasi ke sebuah input field.
2. Input tersebut dipakai langsung oleh fungsi JavaScript untuk ditampilkan di halaman.
3. Karena tidak ada sanitasi, apa pun yang ditulis user (termasuk HTML/JavaScript) ikut dieksekusi dan tampil di halaman.

Contoh kode yang rentan:

```javascript
function sayHi() {
    const name = document.getElementById('name').value
    document.getElementById("welcome-msg").innerHTML = "Welcome " + name
}
```

Jika user mengisi field dengan tag HTML (misalnya `<h1>`), tag tersebut akan dieksekusi sebagai HTML murni di halaman, bukan ditampilkan sebagai teks biasa.

### 5.3 Prinsip Keamanan

Aturan dasar: **jangan pernah percaya user input**. Developer wajib mensanitasi semua input sebelum digunakan, misalnya dengan menghapus tag HTML dari input tersebut.

### 5.4 Teknik Sanitasi Input (JavaScript)

Menggunakan `textContent` alih-alih `innerHTML` — `textContent` otomatis melakukan escape terhadap tag HTML sehingga tidak dieksekusi sebagai markup:

```javascript
document.getElementById("welcome-msg").textContent = "Welcome " + name;
```

Melakukan escape HTML secara manual sebelum ditampilkan:

```javascript
function escapeHTML(text) {
    const div = document.createElement('div');
    div.textContent = text;
    return div.innerHTML;
}
```

Memfilter input agar hanya karakter aman yang diterima:

```javascript
function sanitizeInput(input) {
    return input.replace(/[^a-zA-Z0-9 ]/g, '');
}
```

Perbedaan hasil: input `<h1>Fred Bloggs</h1>` yang tidak disanitasi akan tampil sebagai heading besar; setelah disanitasi, tag tersebut hanya tampil sebagai teks literal atau dihilangkan sama sekali.

---

## 6. Alur Lengkap Request Website

Ketika sebuah website diminta di browser, ada beberapa hal yang terjadi di belakang layar secara berurutan:

1. **Request website di browser** — pengguna mengetik URL atau mengklik link.
2. **Mencari IP address server via DNS** — komputer perlu tahu IP address server tujuan; DNS (Domain Name System) menerjemahkan nama domain menjadi IP address.
3. **Terhubung ke web server** — komputer berkomunikasi dengan web server menggunakan protokol HTTP; webserver mengembalikan HTML, JavaScript, CSS, gambar, dan resource lain.
4. **Menampilkan website** — browser memformat dan menampilkan seluruh data yang diterima sebagai halaman web.

Selain empat langkah utama di atas, ada komponen tambahan (load balancer, CDN, dll) yang membantu web berjalan lebih efisien — dibahas pada bagian berikutnya.

---

## 7. Komponen Tambahan Infrastruktur Web

### 7.1 Load Balancer

Ketika traffic sebuah website sangat besar, atau aplikasi membutuhkan high availability, satu web server saja tidak cukup. Load balancer menyediakan dua fungsi utama: menangani beban traffic tinggi, dan menyediakan failover jika salah satu server tidak responsif.

Saat request masuk, load balancer menerimanya lebih dulu, lalu meneruskannya ke salah satu dari beberapa server di belakangnya berdasarkan algoritma tertentu:

- **Round-robin** — mengirim request ke tiap server secara bergiliran.
- **Weighted** — mengecek beban tiap server dan mengirim request ke server yang paling tidak sibuk.

Load balancer juga melakukan **health check** secara berkala ke tiap server. Jika sebuah server tidak merespons dengan benar, load balancer akan berhenti mengirim traffic ke server itu sampai server tersebut merespons normal kembali.

### 7.2 CDN (Content Delivery Network)

CDN membantu mengurangi traffic ke website yang sibuk dengan meng-host file statis (JavaScript, CSS, gambar, video) di ribuan server yang tersebar di seluruh dunia.

Saat user meminta salah satu file tersebut, CDN menentukan server mana yang secara fisik paling dekat dengan user, lalu mengirim request ke server itu — bukan ke server asal yang mungkin berada jauh di belahan dunia lain.

### 7.3 Database

Website umumnya membutuhkan cara menyimpan informasi penggunanya. Webserver berkomunikasi dengan database untuk menyimpan dan mengambil data.

Database bisa berupa file teks sederhana hingga cluster kompleks dari banyak server yang menyediakan kecepatan dan resilience. Beberapa database yang umum digunakan: **MySQL**, **MSSQL**, **MongoDB**, **Postgres** — masing-masing punya karakteristik dan kegunaan spesifik.

### 7.4 WAF (Web Application Firewall)

WAF ditempatkan di antara request pengguna dan web server. Tujuan utamanya melindungi webserver dari serangan hacking maupun denial of service.

WAF menganalisis setiap web request untuk mendeteksi teknik serangan umum, memeriksa apakah request berasal dari browser asli atau bot, dan menerapkan **rate limiting** — membatasi jumlah request yang diizinkan dari satu IP dalam periode tertentu. Jika sebuah request dinilai berpotensi sebagai serangan, request tersebut akan dijatuhkan (dropped) dan tidak pernah diteruskan ke webserver.

---

## 8. Cara Kerja Web Server

### 8.1 Definisi

Web server adalah software yang mendengarkan koneksi masuk dan menggunakan protokol HTTP untuk mengirimkan konten web ke client. Software web server yang umum dipakai: **Apache**, **Nginx**, **IIS**, dan **NodeJS**.

### 8.2 Root Directory

Web server mengirimkan file dari lokasi yang disebut root directory, yang ditentukan dalam konfigurasi software tersebut.

- Linux (Apache & Nginx) menggunakan lokasi default `/var/www/html`.
- Windows (IIS) menggunakan lokasi default `C:\inetpub\wwwroot`.

Contoh: request ke `http://www.example.com/picture.jpg` akan membuat server mengirim file `/var/www/html/picture.jpg` dari hard drive lokalnya.

### 8.3 Virtual Hosts

Satu web server bisa meng-host banyak website dengan domain berbeda menggunakan **virtual hosts**, yaitu file konfigurasi berbasis teks.

Cara kerjanya: web server memeriksa hostname yang diminta dari HTTP headers, lalu mencocokkannya dengan konfigurasi virtual host yang ada. Jika ditemukan kecocokan, website yang sesuai akan disajikan; jika tidak, website default yang akan disajikan.

Setiap virtual host bisa memiliki root directory yang dipetakan ke lokasi hard drive berbeda, misalnya `one.com` dipetakan ke `/var/www/website_one` dan `two.com` ke `/var/www/website_two`. Tidak ada batasan jumlah website yang bisa di-host dalam satu web server.

---

## 9. Static vs Dynamic Content

### 9.1 Static Content

Konten yang tidak pernah berubah, seperti gambar, JavaScript, dan CSS — bisa juga mencakup HTML yang isinya tetap. File-file ini disajikan langsung dari webserver tanpa ada pemrosesan atau perubahan.

### 9.2 Dynamic Content

Konten yang bisa berubah tergantung pada request yang masuk. Contoh: homepage blog yang menampilkan entry terbaru dan otomatis ter-update saat ada postingan baru, atau halaman search yang hasilnya berbeda-beda tergantung kata kunci yang dicari.

Perubahan pada dynamic content diproses di **backend** menggunakan bahasa pemrograman/scripting. Disebut backend karena semua prosesnya terjadi di balik layar — pengguna tidak bisa melihat proses ini lewat HTML source, karena HTML yang tampil adalah hasil akhir dari pemrosesan backend. Segala sesuatu yang tampak di browser disebut **frontend**.

### 9.3 Bahasa Backend dan Scripting

Tidak banyak batasan terhadap apa yang bisa dicapai bahasa backend — inilah yang membuat website menjadi interaktif bagi user. Contoh bahasa backend: **PHP**, **Python**, **Ruby**, **NodeJS**, **Perl**, dan lainnya. Bahasa-bahasa ini mampu berinteraksi dengan database, memanggil layanan eksternal, dan memproses data dari pengguna.

Contoh sederhana request PHP: `http://example.com/index.php?name=adam`

File `index.php` dibangun seperti berikut:

```php
<html><body>Hello <?php echo $_GET["name"]; ?></body></html>
```

Output yang diterima client:

```html
<html><body>Hello adam</body></html>
```

Client tidak pernah melihat kode PHP-nya karena kode tersebut diproses sepenuhnya di backend — client hanya menerima hasil akhirnya. Sifat inilah yang membuka banyak celah keamanan pada aplikasi web yang tidak dibangun dengan aman.

---

## 10. Istilah Penting

**HTTP (HyperText Transfer Protocol)** — kumpulan perintah standar yang dipakai browser untuk berkomunikasi dengan web server.

**DNS (Domain Name System)** — sistem yang menerjemahkan nama domain menjadi IP address server.

**Request** — permintaan yang dikirim browser ke web server.

**Response** — data yang dikembalikan server ke browser sebagai jawaban atas request.

**Frontend (client-side)** — bagian website yang dilihat dan dirender langsung oleh browser pengguna.

**Backend (server-side)** — bagian website yang memproses logika dan data di server, tidak terlihat oleh user.

**Element/Tag** — building block penyusun struktur HTML.

**Attribute** — properti tambahan pada sebuah tag HTML (misalnya class, id, src).

**Root Directory** — folder utama tempat web server menyimpan dan menyajikan file website.

**Virtual Host** — konfigurasi yang memungkinkan satu web server meng-host banyak website berbeda domain.

**Static Content** — konten yang tidak berubah dan disajikan langsung tanpa pemrosesan.

**Dynamic Content** — konten yang diproses backend dan bisa berubah tergantung request.

**Sensitive Data Exposure** — kerentanan berupa terekposnya informasi sensitif di source code yang bisa diakses siapa pun.

**HTML Injection** — kerentanan akibat input pengguna tidak disanitasi sehingga bisa disisipi kode HTML/JavaScript.

**Input Sanitisation** — proses membersihkan/memvalidasi input pengguna sebelum digunakan untuk mencegah injeksi kode berbahaya.

**innerHTML** — properti JavaScript yang menuliskan konten sebagai HTML mentah (rentan disalahgunakan jika berisi input user tanpa sanitasi).

**textContent** — properti JavaScript yang menuliskan konten sebagai teks biasa, otomatis melakukan escape terhadap tag HTML.

**Event** — aksi pada elemen HTML (seperti onclick, onhover) yang memicu eksekusi JavaScript.

**Load Balancer** — komponen yang mendistribusikan traffic ke beberapa server untuk menangani beban tinggi dan menyediakan failover.

**Health Check** — pengecekan berkala oleh load balancer terhadap kondisi tiap server.

**CDN (Content Delivery Network)** — jaringan server tersebar yang meng-host file statis agar diakses dari lokasi terdekat user.

**WAF (Web Application Firewall)** — komponen yang menganalisis dan menyaring request untuk melindungi web server dari serangan.

**Rate Limiting** — pembatasan jumlah request yang diizinkan dari satu IP dalam periode waktu tertentu.

---

## Tools & Platform Rujukan

**Chrome/Safari View Page Source** — fitur browser bawaan untuk melihat source code HTML sebuah halaman, dipakai untuk mengecek adanya sensitive data exposure. Diakses lewat klik kanan pada halaman, lalu pilih "View Page Source" (Chrome) atau "Show Page Source" (Safari).

---

## Catatan Ringkas untuk Ditulis Tangan

**Alur Dasar Website**
- Browser → request → internet → server → response → browser render
- Frontend — client-side, yang dilihat user
- Backend — server-side, proses request

**HTML**
- HTML — bahasa struktur website, elemen/tag = building block
- `<!DOCTYPE html>` — deklarasi HTML5
- `<html>` — root element
- `<head>` — info halaman (title, dll)
- `<body>` — konten yang tampil
- `<h1>` — heading besar
- `<p>` — paragraf
- Attribute `class` — styling, bisa dipakai berulang
- Attribute `id` — unik per elemen, dipakai JS/CSS
- Attribute `src` — lokasi resource (gambar dll)
- View source — klik kanan > View/Show Page Source

**CSS & JavaScript**
- CSS — styling/tampilan
- JavaScript — interaktivitas & fungsionalitas
- `<script src="...">` — load JS eksternal
- `document.getElementById("id").innerHTML = "..."` — ubah konten via id
- Event `onclick`, `onhover` — trigger JS saat aksi terjadi

**Sensitive Data Exposure**
- Definisi — info sensitif clear-text bocor di source code
- Sering ada di HTML comment / JS
- Mitigasi — selalu review source code sebelum deploy, hapus test credentials

**HTML Injection**
- Definisi — input user tidak disanitasi lalu dieksekusi sebagai HTML/JS
- Prinsip — never trust user input
- Sejenis: database injection (manipulasi query lewat input)

**Sanitasi Input (JS)**
- `textContent` — aman, auto-escape HTML (dipakai ganti innerHTML)
- Escape manual — buat div, set textContent, ambil innerHTML hasilnya
- Filter regex — `input.replace(/[^a-zA-Z0-9 ]/g, '')` — hanya izinkan huruf/angka/spasi

**Alur Request Website**
1. Request di browser
2. DNS — cari IP server dari domain
3. Connect via HTTP ke server
4. Browser render response (HTML/CSS/JS/gambar)

**Komponen Tambahan**
- Load Balancer — distribusi traffic, failover; algoritma round-robin & weighted; health check berkala
- CDN — host file statis di banyak server dunia, kirim dari server terdekat user
- Database — simpan/ambil data; contoh: MySQL, MSSQL, MongoDB, Postgres
- WAF — filter request sebelum sampai server; deteksi bot & attack pattern; rate limiting

**Web Server**
- Software — dengar koneksi, kirim konten via HTTP
- Contoh — Apache, Nginx, IIS, NodeJS
- Root directory Linux — /var/www/html
- Root directory Windows — C:\inetpub\wwwroot
- Virtual host — 1 server, banyak domain; cek hostname dari HTTP header; tidak ada limit jumlah website

**Static vs Dynamic**
- Static — tidak berubah, disajikan langsung (gambar, CSS, JS)
- Dynamic — berubah per request, diproses backend (blog, search)
- Backend languages — PHP, Python, Ruby, NodeJS, Perl
- Contoh PHP — `<?php echo $_GET["name"]; ?>` output HTML ke client, kode PHP tidak terlihat user
