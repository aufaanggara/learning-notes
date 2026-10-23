# Resume Materi — JavaScript: Simple Demo
31 Agustus 2026

---

## 1. Gambaran Umum Room

Room ini memperkenalkan tiga pilar utama bahasa pemrograman imperatif melalui studi kasus pembuatan game "Guess the Number":

- **Variabel** — menyimpan dan mengubah nilai selama program berjalan
- **Conditional statements** — membuat keputusan berdasarkan kondisi
- **Loop** — mengulang instruksi selama kondisi tertentu terpenuhi

Program dibangun secara bertahap dari v1 hingga v4, masing-masing menambahkan fitur baru di atas versi sebelumnya.

---

## 2. Cara Menjalankan JavaScript

JavaScript bisa dijalankan dua cara:

- **Browser** — buka Web Developer Tools (F12 di Firefox), jalankan di tab Console
- **Node.js** — jalankan file `.js` langsung dari terminal

Room ini menggunakan Node.js secara eksklusif. Perintah untuk menjalankan:

```bash
node namafile.js
```

Contoh: `node guess_v3.js` — file tersedia di `/home/ubuntu/JavaScript-Demo`.

---

## 3. Variabel dan Konstanta

### 3.1 let — Variabel

Dideklarasikan dengan `let`. Nilainya **bisa berubah** sepanjang program berjalan.

```javascript
let tries = 0;
let guess = 0;
```

`tries` diinisialisasi ke `0` karena pengguna belum melakukan percobaan apapun saat program dimulai. `guess` diinisialisasi ke `0` karena `0` tidak mungkin menjadi nilai `secret` (secret selalu antara 1–20).

### 3.2 const — Konstanta

Dideklarasikan dengan `const`. Nilainya **tidak bisa berubah** setelah dideklarasikan.

```javascript
const secret = 12;
```

Dipakai untuk nilai yang tidak boleh berubah selama program berjalan, seperti angka rahasia yang sudah dipilih.

### 3.3 Perbandingan let vs const

| | `let` | `const` |
|---|---|---|
| Nilai | Bisa berubah | Tidak bisa berubah |
| Contoh | `tries`, `guess` | `secret`, `MIN_NUMBER` |

---

## 4. Tipe Data yang Dipakai

- **Number** — angka bulat maupun desimal. Contoh: `0`, `7`, `3.14`
- **String** — teks yang diapit tanda kutip. Contoh: `"Take a guess: "`
- **Boolean** — hasil perbandingan, hanya `true` atau `false`
- **NaN** (Not a Number) — nilai yang muncul saat operasi matematika gagal, misalnya `parseInt("abc", 10)` menghasilkan `NaN`

---

## 5. Method dan Fungsi Penting

### 5.1 Output ke Layar

```javascript
console.log("I'm thinking of a number between 1 and 20");
console.log("You got it in", tries, "tries!");
```

Menampilkan teks ke terminal. Bisa menerima banyak argumen sekaligus, dipisah koma.

### 5.2 Menghasilkan Angka Acak dalam Rentang

```javascript
const secret = Math.floor(Math.random() * (20)) + 1;
```

Breakdown:

- `Math.random()` — menghasilkan angka desimal acak dari `0` (inklusif) hingga `1` (eksklusif). Contoh: `0.372`
- `* 20` — merentangkan ke `0` hingga hampir `20`. Contoh: `7.44`
- `Math.floor()` — membulatkan ke bawah, menghilangkan desimal. Contoh: `7.44` menjadi `7`
- `+ 1` — menggeser rentang dari `0–19` menjadi `1–20`

Versi fleksibel (dipakai di v4) dengan konstanta `MIN_NUMBER` dan `MAX_NUMBER`:

```javascript
const secret = Math.floor(Math.random() * (MAX_NUMBER - MIN_NUMBER + 1)) + MIN_NUMBER;
```

### 5.3 Mengubah Teks Menjadi Angka

```javascript
guess = parseInt(text, 10);
```

Mengubah string input pengguna menjadi bilangan bulat basis 10. Jika input bukan angka (misalnya `"abc"`), hasilnya adalah `NaN`.

---

## 6. Input dari Pengguna (readline)

### 6.1 Mengapa Perlu Library Tambahan

Node.js secara default tidak menunggu input pengguna karena dirancang sebagai runtime untuk aplikasi web, bukan program command-line. Perlu mengimpor library `readline` untuk memaksa Node.js menunggu.

### 6.2 Tiga Baris Wajib

```javascript
import * as readline from "node:readline/promises";
import { stdin as input, stdout as output } from "node:process";

const rl = readline.createInterface({ input, output });
```

Penjelasan tiap baris:

- Baris 1 — mengimpor modul `readline` dengan `/promises` agar program bisa berhenti menunggu tanpa membekukan seluruh sistem
- Baris 2 — mengimpor `stdin` (keyboard) sebagai `input` dan `stdout` (layar) sebagai `output`
- Baris 3 — membuat antarmuka `rl` yang menggabungkan input dan output sebagai saluran komunikasi

### 6.3 Menerima Input

```javascript
const text = await rl.question("Take a guess: ");
```

- `rl.question()` — menampilkan prompt dan menunggu jawaban pengguna, mengembalikan nilai berupa **string**
- `await` — menghentikan eksekusi sementara hingga pengguna merespons

### 6.4 Menutup Antarmuka

```javascript
rl.close();
```

Harus selalu dipanggil di akhir program untuk menutup antarmuka readline dengan bersih.

---

## 7. try / finally

```javascript
try {
    // kode utama
} finally {
    rl.close();
}
```

- Blok `try` — menjalankan kode dalam lingkungan aman; jika terjadi error, program tidak langsung crash
- Blok `finally` — **selalu dijalankan** di akhir, baik program berhasil maupun gagal; dipakai untuk memastikan `rl.close()` selalu terpanggil

---

## 8. Conditional Statements (if / else if / else)

### 8.1 Struktur

```javascript
if (kondisi pertama) {
    // jalankan jika kondisi pertama true
} else if (kondisi kedua) {
    // jalankan jika kondisi pertama false, kondisi kedua true
} else {
    // jalankan jika semua kondisi di atas false
}
```

Sifatnya **mutually exclusive** — begitu satu kondisi terpenuhi (`true`), blok lainnya tidak dicek lagi.

### 8.2 Implementasi dalam Game

```javascript
if (guess < 1 || guess > 20) {
    console.log("That number is out of range. Try again.");
} else if (guess < secret) {
    console.log("Too low, try again.");
} else if (guess > secret) {
    console.log("Too high, try again.");
} else {
    console.log("You got it in", tries, "tries!");
}
```

Urutan pengecekan:

1. Di luar rentang 1–20? → tolak
2. Kurang dari secret? → terlalu rendah
3. Lebih dari secret? → terlalu tinggi
4. Tidak lebih, tidak kurang → berarti sama → benar

### 8.3 Operator Perbandingan

| Operator | Arti |
|---|---|
| `<` | kurang dari |
| `>` | lebih dari |
| `===` | sama dengan (strict) |
| `!==` | tidak sama dengan |
| `\|\|` | atau (OR) |
| `&&` | dan (AND) |

---

## 9. Loop (while)

### 9.1 Struktur

```javascript
while (kondisi) {
    // diulang terus selama kondisi = true
}
```

Loop berhenti begitu kondisi menjadi `false`.

### 9.2 Implementasi dalam Game

```javascript
while (guess !== secret) {
    const text = await rl.question("Take a guess: ");
    guess = parseInt(text, 10);
    tries = tries + 1;

    if (guess < 1 || guess > 20) {
        console.log("That number is out of range. Try again.");
    } else if (guess < secret) {
        console.log("Too low, try again.");
    } else if (guess > secret) {
        console.log("Too high, try again.");
    } else {
        console.log("You got it in", tries, "tries!");
    }
}
```

Selama `guess !== secret`, program terus meminta tebakan baru. Begitu pengguna menebak benar, kondisi menjadi `false` dan loop berhenti.

Yang diulang dalam setiap iterasi:
1. Minta tebakan baru dari pengguna
2. Ubah input teks menjadi angka, simpan ke `guess`
3. Tambah `tries` sebanyak satu
4. Cek apakah tebakan valid dan bandingkan dengan `secret`
5. Tampilkan feedback

---

## 10. Validasi Input dengan Regex

### 10.1 Masalah Tanpa Validasi (v3)

Jika pengguna mengetik teks seperti `"abc"`, maka `parseInt("abc", 10)` menghasilkan `NaN`. Nilai `NaN` tidak sama dengan `secret`, sehingga loop terus berjalan — namun semua perbandingan (`<`, `>`) dengan `NaN` menghasilkan `false`, sehingga program masuk ke blok `else` dan mencetak "You got it!" meskipun tebakan salah. Ini adalah **bug**.

### 10.2 Solusi di v4

```javascript
if (!/^\d+$/.test(text)) {
    console.log("Please type a whole number (like 7).");
} else {
    // proses tebakan
}
```

### 10.3 Memahami Regex `/^\d+$/`

| Bagian | Arti |
|---|---|
| `/` `/` | Pembungkus regex di JavaScript |
| `^` | Mulai dari awal teks |
| `\d` | Satu karakter digit (0–9) |
| `+` | Satu atau lebih |
| `$` | Sampai akhir teks |

Secara keseluruhan: "dari awal sampai akhir, semua karakter harus digit angka."

### 10.4 Method `.test()`

`.test()` adalah method bawaan JavaScript untuk mencocokkan teks dengan pola regex. Mengembalikan `true` jika cocok, `false` jika tidak.

### 10.5 Mengapa Pakai `!` (Tanda Seru)

- `/^\d+$/.test(text)` → `true` jika input adalah angka
- `!/^\d+$/.test(text)` → `true` jika input **bukan** angka

Tanda `!` membalik hasil. Pesan error harus muncul saat input bukan angka, bukan saat input benar — karena itu `!` wajib ada.

---

## 11. Evolusi Versi Program

### v1 — Draft Pertama

Hanya mendeklarasikan variabel, menerima satu input, dan menghitung `tries`. Belum ada feedback, belum ada loop.

```
Fitur: variabel + konstanta + 1x input + hitung tries
Masalah: pengguna tidak tahu tebakannya salah atau benar
```

### v2 — Tambah Conditional

Menambahkan blok `if/else if/else` untuk memberi feedback. Pengguna tahu apakah tebakan terlalu tinggi, terlalu rendah, atau benar — tapi hanya mendapat **satu kesempatan**.

```
Fitur: v1 + feedback via conditional
Masalah: hanya 1 kesempatan tebak
```

### v3 — Tambah Loop

Membungkus logika input dan conditional dalam `while (guess !== secret)`. Pengguna kini bisa menebak berkali-kali.

```
Fitur: v2 + while loop
Masalah: tidak ada validasi input, bug jika user ketik huruf
```

### v4 — Validasi & Refactor

Menambahkan validasi regex dan konstanta `MIN_NUMBER`/`MAX_NUMBER` agar program lebih robust dan mudah dirawat.

```
Fitur: v3 + validasi regex + konstanta MIN/MAX
Status: versi paling lengkap dan stabil
```

---

## 12. Perbandingan v3 vs v4

| Aspek | v3 | v4 |
|---|---|---|
| Input non-angka | Bug (cetak "You got it!") | Ditangani dengan pesan error |
| Rentang angka | Hardcoded `1` dan `20` | Konstanta `MIN_NUMBER`, `MAX_NUMBER` |
| Kemudahan modifikasi | Perlu ubah di banyak tempat | Cukup ubah satu konstanta |
| Ketangguhan | Rentan input aneh | Robust |

---

## 13. Glossary

**`let`** — keyword untuk mendeklarasikan variabel yang nilainya bisa berubah.

**`const`** — keyword untuk mendeklarasikan konstanta yang nilainya tidak bisa berubah.

**`NaN`** — Not a Number; nilai yang dihasilkan saat operasi matematika gagal, misalnya mengonversi teks non-angka.

**`parseInt(teks, basis)`** — fungsi untuk mengubah string menjadi bilangan bulat.

**`Math.random()`** — menghasilkan angka desimal acak antara 0 (inklusif) dan 1 (eksklusif).

**`Math.floor()`** — membulatkan angka ke bawah ke bilangan bulat terdekat.

**`console.log()`** — menampilkan output ke terminal.

**`await`** — menghentikan eksekusi sementara hingga operasi async selesai.

**readline** — modul Node.js untuk membaca input dari command line.

**`rl.question()`** — menampilkan prompt dan menunggu jawaban pengguna, mengembalikan string.

**`rl.close()`** — menutup antarmuka readline setelah selesai dipakai.

**`.test()`** — method regex untuk mengecek apakah sebuah teks cocok dengan pola tertentu.

**regex (regular expression)** — pola teks yang digunakan untuk mencocokkan atau memvalidasi string.

**`try/finally`** — struktur untuk menjalankan kode secara aman dan memastikan cleanup selalu terjadi.

**loop** — struktur pengulangan yang terus menjalankan blok kode selama kondisi tertentu terpenuhi.

**conditional statement** — struktur percabangan yang menjalankan kode berbeda berdasarkan kondisi.

**mutually exclusive** — sifat kondisi `if/else` di mana hanya satu blok yang dijalankan per evaluasi.

**`stdin`** — standard input, biasanya keyboard.

**`stdout`** — standard output, biasanya layar/terminal.

---

## 14. Tools & Platform Rujukan

**Node.js** — runtime environment untuk menjalankan JavaScript di luar browser, dipakai untuk menguji seluruh program di room ini. URL: https://nodejs.org

**Firefox Web Developer Tools** — console browser untuk menjalankan JavaScript secara langsung tanpa Node.js. Dibuka dengan F12 di Firefox.

---

## 15. Catatan Ringkas untuk Ditulis Tangan

### Variabel & Konstanta

- `let` — variabel, nilai bisa berubah
- `const` — konstanta, nilai tidak bisa berubah
- `NaN` — hasil parseInt gagal (input bukan angka)

### Method Penting

- `Math.random()` — desimal acak 0 sampai <1
- `Math.floor()` — bulatkan ke bawah
- `parseInt(teks, 10)` — ubah string jadi angka
- `console.log()` — tampilkan ke terminal
- `rl.question()` — minta input dari user (return string)
- `rl.close()` — tutup readline
- `.test()` — cek cocok regex, return true/false

### Formula Angka Acak 1–20

`Math.floor(Math.random() * 20) + 1`

- `* 20` → rentang 0–19
- `+ 1` → geser ke 1–20

### Conditional

- `if / else if / else` — mutually exclusive, cek berhenti di kondisi pertama yang true
- `||` = OR, `&&` = AND, `!==` = tidak sama, `===` = sama

### Loop

- `while (kondisi) { ... }` — ulangi selama kondisi true
- Berhenti saat kondisi jadi false

### readline (wajib hapal 3 baris)

```
import * as readline from "node:readline/promises";
import { stdin as input, stdout as output } from "node:process";
const rl = readline.createInterface({ input, output });
```

- `await` — tunggu user input
- Selalu akhiri dengan `rl.close()` di blok `finally`

### try / finally

- `try` — jalankan kode aman
- `finally` — selalu jalan di akhir (untuk cleanup)

### Regex Validasi Angka

- `/^\d+$/` — pola: semua karakter harus digit dari awal sampai akhir
- `^` = awal, `\d` = digit, `+` = satu atau lebih, `$` = akhir
- `!` di depan `.test()` → balik hasil (true jika BUKAN angka)

### Evolusi Versi

- v1 — variabel + 1x input, belum ada feedback
- v2 — tambah conditional (feedback 1x tebak)
- v3 — tambah while loop (bisa tebak berkali-kali)
- v4 — tambah validasi regex + konstanta MIN/MAX

### Cara Jalankan

`node namafile.js` di terminal
