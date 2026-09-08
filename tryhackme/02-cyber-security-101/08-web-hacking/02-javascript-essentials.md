# Resume Materi — JavaScript Essentials
**TryHackMe Room: JavaScript Essentials**
**Tanggal: 01 September 2026**

---

## Daftar Isi

1. [Apa Itu JavaScript](#1-apa-itu-javascript)
2. [Variabel & Tipe Data](#2-variabel--tipe-data)
3. [Fungsi (Functions)](#3-fungsi-functions)
4. [Perulangan (Loops)](#4-perulangan-loops)
5. [Control Flow](#5-control-flow)
6. [Dialogue Functions & Cara Penyalahgunaannya](#6-dialogue-functions--cara-penyalahgunaannya)
7. [Integrasi JS dalam HTML](#7-integrasi-js-dalam-html)
8. [Minifikasi & Obfuskasi](#8-minifikasi--obfuskasi)
9. [Bypassing Login Forms](#9-bypassing-login-forms)
10. [Best Practices Keamanan JS](#10-best-practices-keamanan-js)
11. [Glossary](#11-glossary)
12. [Tools & Platform Rujukan](#12-tools--platform-rujukan)
13. [Catatan Ringkas untuk Ditulis Tangan](#13-catatan-ringkas-untuk-ditulis-tangan)

---

## 1. Apa Itu JavaScript

**JavaScript (JS)** adalah bahasa scripting populer yang digunakan untuk menambahkan interaktivitas ke halaman web yang sudah dibangun dengan HTML dan CSS.

JS bersifat **interpreted**, artinya kode dieksekusi langsung oleh browser tanpa perlu proses kompilasi terlebih dahulu. Ini berbeda dengan bahasa seperti C++ atau Java yang harus dikompilasi dulu sebelum bisa dijalankan.

JS terutama berjalan di **sisi client (browser)**, sehingga siapapun yang tahu cara membuka developer tools bisa melihat, memeriksa, dan berinteraksi dengan kode JS di halaman web manapun. Inilah yang menjadikan pemahaman JS penting dari sudut pandang keamanan — bukan hanya untuk pengembangan, tapi juga untuk memahami bagaimana penyerang memanfaatkan fungsionalitas yang sah untuk tujuan jahat.

### 1.1 Peran JS dalam Web

- **HTML** membangun struktur halaman (elemen, konten)
- **CSS** mengatur tampilan (warna, layout, font)
- **JavaScript** menambahkan logika dan interaktivitas (validasi form, animasi, respons klik, pembaruan konten dinamis)

---

## 2. Variabel & Tipe Data

### 2.1 Variabel

Variabel adalah wadah untuk menyimpan nilai data. Setiap variabel diberi nama agar nilainya bisa dipanggil kembali kapanpun dibutuhkan.

Ada tiga kata kunci untuk mendeklarasikan variabel di JS:

| Kata Kunci | Scope | Bisa Diubah |
|---|---|---|
| `var` | Function-scoped | Ya |
| `let` | Block-scoped | Ya |
| `const` | Block-scoped | Tidak |

**Function-scoped** berarti variabel bisa diakses dari manapun di dalam fungsi yang sama, bahkan di luar blok `{ }` tempat ia dideklarasikan.

**Block-scoped** berarti variabel hanya bisa diakses dari dalam blok `{ }` tempat ia dideklarasikan. Ini memberikan kontrol visibilitas yang lebih ketat dan merupakan praktik yang lebih aman dibanding `var`.

Rekomendasi: gunakan `let` untuk nilai yang berubah, `const` untuk nilai yang tetap. Hindari `var` di kode modern.

### 2.2 Tipe Data

Tipe data mendefinisikan jenis nilai yang bisa disimpan sebuah variabel.

- **string** — teks, ditulis dalam tanda kutip. Contoh: `"Hello World"`
- **number** — angka (bulat maupun desimal). Contoh: `25`, `3.14`
- **boolean** — hanya dua nilai: `true` atau `false`
- **null** — nilai kosong yang disengaja (sengaja dikosongkan)
- **undefined** — variabel dideklarasikan tapi belum diisi nilai
- **object** — struktur data kompleks yang bisa menampung banyak nilai; termasuk **array** (daftar terurut) dan objek key-value

---

## 3. Fungsi (Functions)

**Fungsi** adalah blok kode yang dirancang untuk menjalankan tugas tertentu dan dapat dipanggil berulang kali tanpa perlu menulis ulang kodenya.

### 3.1 Sintaks Dasar

```javascript
function namafungsi(parameter) {
    // isi kode yang akan dijalankan
}

// Cara memanggil fungsi
namafungsi(argumen);
```

### 3.2 Contoh Penggunaan

```javascript
function PrintResult(rollNum) {
    alert("Username with roll number " + rollNum + " has passed the exam");
}

const rollNumbers = [101, 102, 103];

for (let i = 0; i < 100; i++) {
    PrintResult(rollNumbers[i]);
}
```

Catatan pada contoh di atas: array `rollNumbers` hanya punya 3 elemen (indeks 0, 1, 2). Setelah `i = 2`, nilai `rollNumbers[i]` menjadi `undefined` karena indeks tersebut tidak ada.

### 3.3 Mengapa Fungsi Penting

Tanpa fungsi, kode yang sama harus ditulis berulang kali untuk setiap kasus. Fungsi memungkinkan kode dipakai kembali (**reusable**) dan membuat program lebih terorganisir.

---

## 4. Perulangan (Loops)

**Loop** memungkinkan sebuah blok kode dijalankan berulang kali selama kondisi tertentu bernilai `true`.

### 4.1 Tiga Jenis Loop

**`for`** — paling umum digunakan, cocok ketika jumlah iterasi sudah diketahui.

```javascript
for (let i = 0; i < 100; i++) {
    PrintResult(rollNumbers[i]);
}
```

Cara membaca: mulai dari `i = 0`; jalankan selama `i < 100`; setiap putaran `i` bertambah 1 (`i++`).

**`while`** — jalankan selama kondisi bernilai true; kondisi dicek sebelum eksekusi.

```javascript
while (kondisi) {
    // isi kode
}
```

**`do...while`** — mirip `while`, tapi kode dijalankan minimal satu kali terlebih dahulu, baru kondisi dicek.

```javascript
do {
    // isi kode
} while (kondisi);
```

---

## 5. Control Flow

**Control flow** mengacu pada urutan di mana pernyataan dan blok kode dieksekusi, yang ditentukan oleh kondisi tertentu.

### 5.1 If-Else

Pernyataan kondisional yang paling umum. Mengeksekusi blok kode berbeda tergantung apakah kondisi bernilai `true` atau `false`.

```javascript
if (age >= 18) {
    document.getElementById("message").innerHTML = "You are an adult.";
} else {
    document.getElementById("message").innerHTML = "You are a minor.";
}
```

### 5.2 Switch

Digunakan ketika ada banyak kondisi yang perlu diperiksa berdasarkan satu nilai. Lebih rapi dari banyak `if-else` yang bertumpuk.

```javascript
switch (nilai) {
    case "A":
        // kode jika nilai = "A"
        break;
    case "B":
        // kode jika nilai = "B"
        break;
    default:
        // kode jika tidak ada case yang cocok
}
```

Kata kunci `break` wajib ditulis di akhir setiap `case` untuk menghentikan eksekusi — tanpanya, JS akan terus mengeksekusi case berikutnya meski kondisinya tidak cocok (**fall-through**).

### 5.3 Relevansi Keamanan: Bypassing Control Flow

Karena JS berjalan di sisi client, logika kontrol seperti pengecekan login yang ditulis di JS bisa dibaca langsung dari source code halaman. Penyerang tidak perlu menebak — cukup buka developer tools, baca kondisinya, dan masukkan nilai yang sesuai.

Inilah mengapa **autentikasi tidak boleh hanya mengandalkan JS sisi client**. Validasi dan pengecekan kritis harus selalu dilakukan di server.

---

## 6. Dialogue Functions & Cara Penyalahgunaannya

JS menyediakan tiga fungsi built-in untuk membuat kotak dialog interaktif dengan pengguna. Jika tidak diimplementasikan dengan aman, ketiganya bisa dieksploitasi untuk serangan seperti **Cross-Site Scripting (XSS)**.

### 6.1 Alert

Menampilkan kotak pesan dengan satu tombol OK. Digunakan untuk menyampaikan informasi atau peringatan.

```javascript
alert("Hello THM");
```

**Cara dieksploitasi:** Penyerang bisa membuat file HTML yang menjalankan `alert()` dalam loop ratusan kali, memaksa korban menutup kotak satu per satu — menyebabkan gangguan berat (denial of experience).

```javascript
for (let i = 0; i < 500; i++) {
    alert("Hacked");
}
```

### 6.2 Prompt

Menampilkan kotak input yang meminta pengguna memasukkan data. Mengembalikan nilai yang diketik (jika OK) atau `null` (jika Cancel).

```javascript
name = prompt("What is your name?");
alert("Hello " + name);
```

**Cara dieksploitasi:** Bisa digunakan untuk mengumpulkan data dari pengguna yang tidak curiga jika disuntikkan ke halaman web yang terkompromi.

### 6.3 Confirm

Menampilkan kotak pesan dengan dua tombol: OK dan Cancel. Mengembalikan `true` (OK) atau `false` (Cancel).

```javascript
confirm("Do you want to proceed?");
```

**Cara dieksploitasi:** Hasil `true`/`false` dari confirm bisa dimanipulasi untuk membypass logika kondisional dalam program.

---

## 7. Integrasi JS dalam HTML

Ada dua cara memasukkan JS ke dalam halaman HTML.

### 7.1 Internal JavaScript

Kode JS ditulis langsung di dalam file HTML, ditempatkan di antara tag `<script>` dan `</script>`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Internal JS</title>
</head>
<body>
    <h1>Addition of Two Numbers</h1>
    <p id="result"></p>

    <script>
        let x = 5;
        let y = 10;
        let result = x + y;
        document.getElementById("result").innerHTML = "The result is: " + result;
    </script>
</body>
</html>
```

Tag `<script>` bisa diletakkan di:
- `<head>` — untuk script yang perlu dimuat sebelum konten halaman dirender
- `<body>` — untuk script yang berinteraksi dengan elemen HTML saat dimuat

### 7.2 External JavaScript

Kode JS disimpan di file terpisah berekstensi `.js`, lalu dihubungkan ke HTML menggunakan atribut `src`.

```html
<script src="script.js"></script>
```

File `.js` bisa disimpan di server yang sama maupun di server eksternal (CDN). Metode ini lebih disukai untuk proyek besar karena memisahkan logika dari struktur, membuat kode lebih terorganisir dan mudah dipelihara.

### 7.3 Cara Mengidentifikasi Jenis JS (Perspektif Pentester)

Buka halaman di browser → klik kanan → **View Page Source** → cari tag `<script>`:

- Tag `<script>` **tanpa** atribut `src` → **Internal JS** (kode ada di dalam file HTML itu sendiri)
- Tag `<script>` **dengan** atribut `src` → **External JS** (kode dimuat dari file atau server lain)

Identifikasi ini penting saat pen-testing karena menentukan di mana kode JS yang relevan berada dan bagaimana cara mengaksesnya.

### 7.4 Manipulasi DOM dengan JS

JS berinteraksi dengan elemen HTML melalui **DOM (Document Object Model)**. Dua cara umum:

```javascript
// Memilih elemen berdasarkan ID dan mengubah isinya
document.getElementById("result").innerHTML = "The result is: " + result;
```

- `document.getElementById("id")` — memilih elemen HTML berdasarkan atribut `id`
- `.innerHTML` — properti untuk membaca atau mengubah konten HTML di dalam elemen yang dipilih

---

## 8. Minifikasi & Obfuskasi

### 8.1 Minifikasi

**Minifikasi** adalah proses mengompres file JS dengan menghapus semua karakter yang tidak diperlukan untuk eksekusi:

- Spasi dan baris baru
- Komentar
- Nama variabel yang panjang diperpendek

Tujuan utama: **mengurangi ukuran file** sehingga halaman web memuat lebih cepat, terutama di lingkungan produksi. Kode hasil minifikasi tetap berfungsi identik, hanya tidak bisa dibaca manusia.

### 8.2 Obfuskasi

**Obfuskasi** adalah proses mengubah kode JS agar lebih sulit dipahami oleh manusia, tanpa mengubah fungsionalitasnya. Teknik yang digunakan:

- Mengganti nama variabel dan fungsi menjadi nama acak yang tidak bermakna
- Menambahkan kode palsu (dummy code) yang tidak berpengaruh pada eksekusi
- Mengubah string menjadi representasi karakter heksadesimal atau bentuk encoded lain

Hasil obfuskasi tetap dapat dieksekusi oleh browser dengan sempurna. Ukuran file biasanya justru bertambah (contoh: dari 58 bytes menjadi 1.67 KB).

### 8.3 Perbandingan

| Aspek | Minifikasi | Obfuskasi |
|---|---|---|
| Tujuan | Kurangi ukuran file | Sembunyikan logika kode |
| Ukuran file | Berkurang | Bisa bertambah |
| Keterbacaan | Sangat rendah | Sangat rendah |
| Fungsionalitas | Sama persis | Sama persis |

### 8.4 Deobfuskasi

**Deobfuskasi** adalah kebalikan dari obfuskasi — mengembalikan kode yang sudah diobfuskasi ke bentuk yang bisa dibaca manusia. Ini dilakukan menggunakan tool online.

Penting untuk dipahami: deobfuskasi tidak akan mengembalikan nama variabel asli yang bermakna (seperti `userPassword`). Yang dikembalikan hanyalah struktur logikanya — nama variabel akan tetap acak tapi lebih terbaca dibanding bentuk obfuskasinya.

Implikasi keamanan: obfuskasi bukan solusi keamanan mutlak. Seorang analis yang terampil tetap bisa memahami logika kode meski sudah diobfuskasi, hanya membutuhkan waktu dan usaha lebih. Oleh karena itu, **jangan simpan data sensitif di JS** meski sudah diobfuskasi.

---

## 9. Bypassing Login Forms

Autentikasi yang seluruh logikanya ditulis di JavaScript sisi client memiliki kerentanan fundamental: kode pemeriksaannya bisa dibaca langsung dari browser.

### 9.1 Skenario

Seorang developer membuat form login yang mengecek username dan password menggunakan `if-else` di JS:

```javascript
if (username === "admin" && password === "secret123") {
    // izinkan masuk
}
```

Karena JS berjalan di browser, siapapun yang membuka **View Page Source** atau tab **Sources** di developer tools bisa langsung melihat kondisi ini — termasuk nilai username dan password yang valid.

### 9.2 Cara Bypass

1. Buka halaman login di browser
2. Klik kanan → Inspect → tab **Sources**
3. Cari file JS yang menangani autentikasi
4. Baca kondisi `if-else` untuk mengetahui kredensial yang valid
5. Masukkan kredensial tersebut → berhasil login

### 9.3 Pelajaran

Validasi dan autentikasi yang bersifat kritis **harus selalu diproses di server**, bukan di JS sisi client. JS boleh dipakai untuk validasi format (misalnya pastikan field tidak kosong), tapi pengecekan kredensial yang sesungguhnya harus terjadi di backend.

---

## 10. Best Practices Keamanan JS

### 10.1 Jangan Hanya Andalkan Client-Side Validation

Validasi form di JS sisi client bisa dinonaktifkan atau dimanipulasi oleh pengguna dengan mudah — cukup buka developer tools dan modifikasi kondisinya. Selalu lakukan validasi juga di sisi server sebagai lapisan keamanan yang tidak bisa dilewati.

### 10.2 Jangan Sembarangan Include Library Eksternal

JS memungkinkan penyertaan script dari sumber mana pun via atribut `src`. Pelaku jahat sengaja membuat library palsu dengan nama yang menyerupai library populer dan mengunggahnya ke internet. Jika library berbahaya disertakan, seluruh aplikasi web terekspos.

Langkah aman:
- Selalu verifikasi sumber library sebelum digunakan
- Gunakan CDN resmi yang sudah terpercaya
- Jika memungkinkan, host library sendiri di server yang dikontrol

### 10.3 Jangan Hardcode Data Sensitif

Jangan pernah menyimpan **API key**, **access token**, **password**, atau kredensial apapun langsung di dalam kode JS. Source code JS bisa dilihat siapapun melalui browser.

Contoh praktik buruk yang harus dihindari:

```javascript
// Bad Practice — JANGAN dilakukan
const privateAPIKey = 'pk_TryHackMe-1337';
```

Data sensitif harus disimpan di sisi server dan diakses melalui request yang terautentikasi.

### 10.4 Selalu Minify & Obfuscate di Production

Saat kode JS akan dipublikasikan di lingkungan produksi, selalu minifikasi dan obfuskasi terlebih dahulu. Ini tidak memberikan keamanan absolut, tapi mempersulit dan memperlambat upaya reverse engineering oleh penyerang — setiap hambatan yang ditambahkan berarti waktu ekstra untuk mendeteksi dan merespons ancaman.

---

## 11. Glossary

**Interpreted** — sifat bahasa pemrograman di mana kode dieksekusi langsung baris per baris oleh interpreter (browser), tanpa perlu dikompilasi dulu menjadi binary.

**Function-scoped** — variabel yang dideklarasikan dengan `var` bisa diakses dari manapun di dalam fungsi yang sama, termasuk di luar blok `{ }` tempat ia dideklarasikan.

**Block-scoped** — variabel hanya bisa diakses dari dalam blok `{ }` tempat ia dideklarasikan. Berlaku untuk `let` dan `const`.

**DOM (Document Object Model)** — representasi terstruktur dari dokumen HTML yang dibuat oleh browser. JS menggunakan DOM untuk mengakses dan memanipulasi elemen halaman web secara dinamis.

**innerHTML** — properti JS untuk membaca atau mengubah konten HTML di dalam sebuah elemen.

**getElementById** — metode JS untuk memilih elemen HTML berdasarkan nilai atribut `id`-nya.

**Client-side** — kode yang dieksekusi di browser pengguna. Bisa dilihat dan dimanipulasi oleh siapapun.

**Server-side** — kode yang dieksekusi di server. Tidak terlihat oleh pengguna browser biasa.

**XSS (Cross-Site Scripting)** — serangan di mana penyerang menyuntikkan kode JS berbahaya ke halaman web yang kemudian dieksekusi di browser korban.

**Hardcode** — menulis nilai secara langsung di dalam kode (bukan mengambilnya dari konfigurasi atau server). Berbahaya untuk data sensitif karena bisa dibaca dari source code.

**Minifikasi** — proses mengompres kode JS dengan menghapus spasi, komentar, dan karakter tidak penting untuk mengurangi ukuran file dan mempercepat loading.

**Obfuskasi** — proses mengubah kode agar sulit dibaca dan dipahami manusia, tanpa mengubah fungsionalitasnya saat dieksekusi browser.

**Deobfuskasi** — proses kebalikan dari obfuskasi — mengembalikan kode yang diobfuskasi ke bentuk yang lebih bisa dibaca manusia menggunakan tool khusus.

**Attack surface** — semua titik atau celah dalam suatu sistem yang berpotensi bisa diserang atau dieksploitasi.

**Supply chain attack** — serangan yang menargetkan library atau dependency pihak ketiga yang digunakan oleh sebuah aplikasi, bukan menyerang aplikasinya langsung.

**Fall-through** — perilaku dalam `switch` di mana eksekusi berlanjut ke `case` berikutnya meski kondisinya tidak cocok, terjadi ketika `break` tidak ditulis.

**CDN (Content Delivery Network)** — jaringan server yang mendistribusikan file (termasuk library JS) dari lokasi yang dekat dengan pengguna untuk mempercepat loading.

**DOM Manipulation** — penggunaan JS untuk mengakses, mengubah, menambah, atau menghapus elemen dan konten di halaman HTML secara dinamis setelah halaman dimuat.

---

## 12. Tools & Platform Rujukan

**Google Chrome DevTools**
- Fungsi: Inspeksi dan eksekusi kode JS langsung di browser, lihat source code, debug script
- Cara buka: `Ctrl + Shift + I` atau klik kanan → Inspect
- Tab yang relevan: Console (eksekusi JS), Sources (lihat file JS), Elements (lihat HTML)

**JavaScript Obfuscator (obfuscator.io)**
- Fungsi: Tool online untuk meminifikasi dan mengobfuskasi kode JS
- URL: https://obfuscator.io

**Deobfuscator (obfuscator.io)**
- Fungsi: Tool online untuk melakukan deobfuskasi — mengembalikan kode JS yang diobfuskasi ke bentuk yang lebih readable
- URL: https://obf-io.deobfuscate.io (atau via fitur deobfuscate di obfuscator.io)

**Pluma Text Editor**
- Fungsi: Text editor bawaan di VM Linux (Ubuntu) yang digunakan untuk menulis dan mengedit file HTML/JS dalam latihan room ini
- Cara buka: klik kanan file → Open With → Pluma

---

## 13. Catatan Ringkas untuk Ditulis Tangan

### JavaScript Dasar

- JS — bahasa scripting untuk interaktivitas web (validasi, animasi, logika)
- Interpreted — langsung dieksekusi browser, tanpa kompilasi
- Client-side — berjalan di browser, bisa dilihat & dimanipulasi user
- HTML = struktur | CSS = tampilan | JS = logika & interaktivitas

### Variabel

- var — function-scoped, bisa diubah
- let — block-scoped, bisa diubah (disarankan)
- const — block-scoped, tidak bisa diubah
- Block-scoped — hanya hidup dalam blok { } tempat ia dibuat
- Function-scoped — hidup di seluruh fungsi meski di luar { }

### Tipe Data

- string — teks ("Hello")
- number — angka (25, 3.14)
- boolean — true / false
- null — kosong disengaja
- undefined — belum diisi
- object — data kompleks (array, key-value)

### Fungsi

- function nama(param) { } — deklarasi fungsi
- nama(argumen); — cara memanggil fungsi
- Tujuan: reusable, jalankan tugas yang sama berkali-kali

### Loop

- for (let i = 0; i < N; i++) — loop dengan counter, jalankan N kali
- while (kondisi) — selama kondisi true, terus jalan
- do { } while (kondisi) — jalan minimal 1x, baru cek kondisi
- i++ — singkatan dari i = i + 1

### Control Flow

- if (kondisi) { } else { } — pilih blok berdasarkan kondisi
- switch (nilai) { case X: break; default: } — banyak kondisi dari 1 nilai
- break — wajib di tiap case switch, hentikan eksekusi agar tidak fall-through
- Fall-through — eksekusi lanjut ke case berikut kalau break tidak ada

### Dialogue Functions

- alert("pesan") — tampilkan pesan + tombol OK
- prompt("pertanyaan") — minta input user, return nilai atau null
- confirm("pertanyaan") — OK/Cancel, return true/false
- XSS — penyalahgunaan JS melalui injeksi kode di halaman web

### Integrasi JS di HTML

- Internal JS — kode di dalam tag script di file HTML
- External JS — kode di file .js terpisah, dihubungkan via src="file.js"
- Cek jenis JS: View Page Source → cari tag script
- Ada src → External | Tanpa src → Internal

### DOM Manipulation

- document.getElementById("id") — pilih elemen HTML berdasarkan id
- .innerHTML — baca atau ubah konten elemen
- JS eksekusi saat browser load file HTML

### Minifikasi & Obfuskasi

- Minifikasi — hapus spasi/komentar, kecilkan file, percepat loading
- Obfuskasi — samarkan logika kode, ganti nama variabel jadi acak
- Deobfuskasi — kembalikan kode obfuskasi ke bentuk readable (pakai tool online)
- Obfuskasi bukan keamanan mutlak — bisa ditembus, hanya memperlambat
- Kode obfuskasi tetap berfungsi sempurna di browser

### Bypassing Login

- Logic JS client-side bisa dibaca via DevTools → Sources
- Cukup baca kondisi if-else untuk tahu kredensial yang valid
- Solusi: validasi kritis HARUS di server, bukan JS client-side

### Best Practices

- Jangan hanya client-side validation — user bisa disable JS
- Jangan include library sembarang — bisa berisi malware (supply chain attack)
- Jangan hardcode secrets — API key, password, token jangan taruh di JS
- Selalu minify & obfuscate di production — persulit reverse engineering
- Hardcode — tulis nilai langsung di kode, berbahaya untuk data sensitif

### Tools

- Ctrl+Shift+I — buka Chrome DevTools
- Tab Console — eksekusi JS langsung di browser
- Tab Sources — lihat semua file JS yang dimuat halaman
- Tab Elements — lihat struktur HTML
- Ctrl+V di Console — paste kode, Enter untuk jalankan
- obfuscator.io — tool obfuskasi JS online
- Pluma — text editor di VM Linux untuk edit HTML/JS
