# Encoding Karakter — Resume Materi

TryHackMe Room: Encoding | 31 Agustus 2026

---

## 1. Konsep Dasar

Komputer hanya menyimpan angka (bits). Untuk menyimpan teks, diperlukan kesepakatan yang menghubungkan angka dengan karakter — kesepakatan inilah yang disebut encoding.

**Representasi** adalah konsep bahwa semua data hidup sebagai bits dan angka di dalam memori komputer.

**Encoding** adalah pemetaan spesifik yang telah disepakati bersama (agreed-upon mapping) antara angka dan maknanya. Contoh: angka 65 → karakter "A".

**Gibberish** terjadi ketika file disimpan dengan satu encoding lalu dibuka menggunakan encoding yang berbeda — karakter tampil sebagai simbol acak.

---

## 2. ASCII

### 2.1 Definisi & Spesifikasi

**ASCII (American Standard Code for Information Interchange)** adalah standar encoding karakter yang dibuat pada 1963. Kata "American" penting — ASCII hanya dirancang untuk bahasa Inggris.

Spesifikasi teknis:
- Menggunakan 7 bit
- 128 karakter total (kode 0–127)
- Mencakup huruf Inggris, digit, tanda baca, dan control characters

### 2.2 Kode ASCII yang Wajib Dihapal

| Decimal | Hex | Simbol | Binary   | Keterangan                        |
|---------|-----|--------|----------|-----------------------------------|
| 7       | 07  | BEL    | 00000111 | Bell — dulu membunyikan "ting" di terminal |
| 10      | 0A  | \n     | 00001010 | Line Feed (new line / Enter)      |
| 13      | 0D  | \r     | 00001101 | Carriage Return                   |
| 48      | 30  | 0      | 00110000 | Digit nol                         |
| 57      | 39  | 9      | 00111001 | Digit sembilan                    |
| 65      | 41  | A      | 01000001 | Uppercase A                       |
| 90      | 5A  | Z      | 01011010 | Uppercase Z                       |
| 91      | 5B  | [      | 01011011 | Opening bracket                   |
| 97      | 61  | a      | 01100001 | Lowercase a                       |
| 122     | 7A  | z      | 01111010 | Lowercase z                       |
| 127     | 7F  | DEL    | 01111111 | Delete                            |

### 2.3 Pola Penting ASCII

Huruf ASCII tersusun secara berurutan — ini sangat berguna:

- Tahu "A" = 65, maka "B" = 66, "C" = 67, dan seterusnya
- Uppercase A–Z → hex 41 sampai 5A
- Lowercase a–z → hex 61 sampai 7A
- Selisih uppercase dan lowercase = **32 decimal / 20 hex**. Contoh: "A" = 65, "a" = 97, selisih 32

### 2.4 Contoh: "TryHackMe" dalam ASCII

Teks "TryHackMe" disimpan ke `file.txt` menggunakan ASCII:

**Binary:**
```
01010100 01110010 01111001 01001000 01100001 01000011 01101011 01001101 01100101 00001010
```

**Hexadecimal (lebih praktis dibaca):**
```
54 72 79 48 61 63 6b 4d 65 0a
```

**Decimal (jarang dipakai):**
```
124 162 171 110 141 143 153 115 145 012
```

Byte terakhir `0a` adalah `\n` (new line) — karakter yang ditambahkan saat menekan Enter. Representasi hexadecimal lebih disukai daripada decimal karena lebih kompak dan langsung mencerminkan pengelompokan 4-bit.

---

## 3. Extended ASCII & Keterbatasannya

### 3.1 Solusi Awal

Dengan menambahkan bit ke-8, Extended ASCII mendapat 128 karakter tambahan untuk bahasa non-Inggris. Muncullah berbagai standar regional:

**ISO-8859-1 (Latin-1)** — Eropa Barat: German (ß, ü), French (é, ç), Spanish (ñ, ¿), Italian, Portuguese, Nordic (ð/Ð).

**ISO-8859-2 (Latin-2)** — Eropa Tengah/Timur: Polish (ł, ń), Czech (č, ř), Hungarian (ő, ű), Croatian (đ), Romanian (ș, ț), Slovak.

**Windows-1252** — Standar Microsoft untuk Eropa Barat, berbeda dari ISO-8859-1 di beberapa kode.

### 3.2 Masalah yang Timbul

Banyaknya standar regional menyebabkan kekacauan. Contoh nyata: file disimpan dengan ISO-8859-1 lalu dibuka dengan ISO-8859-2 — karakter "ø" tampil sebagai "ř". Ini adalah akar dari masalah gibberish yang sering ditemui.

### 3.3 Mengapa 128 Karakter Tambahan Tidak Cukup

| Bahasa  | Kebutuhan Karakter                          | Standar          |
|---------|---------------------------------------------|------------------|
| Inggris | 52 karakter (A–Z + a–z)                     | ASCII cukup      |
| Arab    | 250+ (ligatur & diakritik)                  | Extended tidak cukup |
| Jepang  | 2.136 Kanji sehari-hari; 6.879 total        | JIS X 0208       |
| Cina    | ~8.000 dikenal; 87.887 total                | GB 18030-2022    |
| Emoji   | Ribuan sequence                             | Belum masuk hitungan |

---

## 4. Unicode

### 4.1 Definisi & Prinsip

**Unicode** adalah standar encoding karakter universal yang menetapkan code point unik untuk setiap karakter dari semua sistem penulisan modern maupun historis di seluruh dunia.

Unicode menyelesaikan tiga masalah sekaligus:
- Tidak perlu memilih standar encoding yang berbeda per bahasa
- Berbagai bahasa bisa digunakan dalam satu file atau pesan
- Tidak perlu khawatir dengan encoding yang dipakai penulis asli — semua pakai Unicode yang sama

**Versi terbaru:** Unicode 17.0 — mendefinisikan hampir 157.000 karakter, dengan hampir 4.000 di antaranya adalah emoji sequences.

### 4.2 Format Code Point

Setiap karakter diberi code point dengan format `U+[KODE HEX]`. Beberapa yang wajib dihapal:

- `U+0041` = Latin "A"
- `U+03A9` = Greek "Ω"
- `U+3042` = Japanese Hiragana "あ"
- `U+9F8D` = Chinese "龍" (dragon) — muncul di distro Kali Linux
- `U+1F60A` = Emoji 😊 (smiley face). Binary: `0000 0000 0000 0001 1111 0110 0000 1010`
- `U+30C4` = Japanese "ツ" (tsu) — sering dipakai sebagai smiley di luar Jepang
- `U+062A` = Arabic "ت" (taa) — bentuknya mirip senyum
- `U+265E` = ♞ Black knight catur. Binary: `0010 0110 0101 1110`
- `U+1F525` = Emoji 🔥 — butuh 4 bytes di UTF-8

---

## 5. UTF-8, UTF-16, dan UTF-32

Ketiga format UTF merepresentasikan Unicode yang sama. Perbedaannya hanya pada cara penyimpanan bytes-nya di disk/memori.

### 5.1 UTF-8

Format paling umum di web modern. Menggunakan 1 hingga 4 bytes secara dinamis berdasarkan kompleksitas karakter.

- **`U+0000` – `U+007F`** → 1 byte (identik dengan ASCII — ini yang membuatnya backward compatible)
- **Non-ASCII seperti Ω (`U+03A9`)** → 2 bytes
- **Emoji/skrip kompleks seperti 🔥 (`U+1F525`)** → 4 bytes

Keunggulan utama: efisien (tidak buang bytes) dan sepenuhnya kompatibel mundur dengan ASCII lama.

### 5.2 UTF-16

Menggunakan 2 atau 4 bytes per karakter. Umum digunakan di sistem operasi (Windows, Java).

- **Karakter umum (Latin, Cyrillic, Hanzi)** → 2 bytes. Contoh: "A" = `U+0041`
- **Karakter langka (emoji, ancient scripts)** → 4 bytes menggunakan surrogate pair. Contoh: 🔥 = `U+D83D U+DD25`

### 5.3 UTF-32

Paling sederhana tapi paling boros. Setiap karakter selalu menggunakan tepat 4 bytes tanpa pengecualian.

- "A" → `U+00000041`
- 🔥 → `U+0001F525`

### 5.4 Perbandingan Singkat

| Format  | Bytes/karakter | Efisiensi  | Penggunaan Umum          |
|---------|----------------|------------|--------------------------|
| UTF-8   | 1–4 (dinamis)  | Terbaik    | Web, Internet, Linux     |
| UTF-16  | 2 atau 4       | Seimbang   | Windows, Java, macOS     |
| UTF-32  | Selalu 4       | Terburuk   | Pemrosesan internal      |

---

## 6. Relevansi di Dunia Nyata & CTF

Materi ini bukan sekadar teori — dalam praktik cybersecurity dan CTF, encoding karakter sering muncul:

- **Decode hex string ke teks:** menemukan `54 72 79 48 61 63 6b 4d 65` → decode ke "TryHackMe"
- **Analisis file mentah:** memahami isi file di level byte/hex saat forensik atau reverse engineering
- **Encoding trik di injeksi:** SQL injection atau XSS kadang memanfaatkan perbedaan interpretasi encoding
- **Buffer overflow:** manipulasi karakter di level byte membutuhkan pemahaman ASCII dan hex

Yang perlu dikuasai bukan hafal semua kode — tapi paham konsepnya dan tahu di mana mencari referensi (ASCII table, Unicode chart) saat dibutuhkan.

---

## 7. Glossary

**Encoding** — Kesepakatan mapping antara angka dan karakter/makna tertentu.

**Representasi** — Konsep bahwa semua data disimpan sebagai bits dan angka di memori.

**Code Point** — Angka unik yang ditetapkan Unicode untuk satu karakter. Format: `U+XXXX`.

**Character Set** — Kumpulan karakter beserta nomor uniknya.

**Gibberish** — Tampilan karakter acak akibat encoding yang dipakai tidak cocok.

**Control Characters** — Karakter tak terlihat di ASCII (kode 0–31). Contoh: BEL, LF, CR, DEL.

**Backward Compatibility** — UTF-8 kompatibel penuh dengan ASCII — karakter ASCII valid di UTF-8.

**Ligature** — Gabungan dua huruf menjadi satu simbol, banyak ditemui di bahasa Arab.

**Diacritic** — Tanda tambahan pada huruf. Contoh: é, ñ, ü, ß.

**Surrogate Pair** — Dua unit 16-bit yang dipasangkan di UTF-16 untuk mewakili karakter di luar BMP.

**BMP (Basic Multilingual Plane)** — Range karakter Unicode `U+0000` hingga `U+FFFF`.

**ISO-8859-1 (Latin-1)** — Standar Extended ASCII untuk bahasa Eropa Barat.

**ISO-8859-2 (Latin-2)** — Standar Extended ASCII untuk bahasa Eropa Tengah/Timur.

**JIS X 0208** — Standar Jepang yang mendefinisikan 6.879 karakter.

**GB 18030-2022** — Standar Cina yang mendefinisikan lebih dari 87.887 karakter Hanzi.

---

## 8. Tools & Platform Rujukan

**ASCII Table Online** — Tabel referensi lengkap ASCII dengan decimal, hex, binary, dan deskripsi karakter.
URL: https://www.asciitable.com

**Unicode Character Table** — Lookup dan eksplorasi semua karakter Unicode beserta code point-nya.
URL: https://unicode-table.com

**Static Site TryHackMe** — Site interaktif yang disediakan dalam room untuk eksperimen encoding karakter secara langsung. Diakses via tombol "View Site" di room.

---

## 9. Catatan Ringkas untuk Ditulis Tangan

*Bagian ini untuk disalin ke buku catatan fisik. Format padat: kata kunci — keterangan singkat.*

### Konsep Dasar

- Encoding — kesepakatan angka ↔ karakter
- Representasi — data = bits/angka di memori
- Gibberish — akibat encoding beda saat simpan vs buka
- Character Set — kumpulan karakter + nomor uniknya

### ASCII

- Kepanjangan — American Standard Code for Information Interchange
- Tahun — 1963 | Bit — 7 | Range — 0–127 (128 karakter)
- Hanya bahasa Inggris ("A" = American)
- 7 = BEL | 10 = LF (\n) | 13 = CR
- 48 = "0" | 57 = "9"
- 65 = "A" | 90 = "Z" | hex 41–5A
- 97 = "a" | 122 = "z" | hex 61–7A
- 91 = "[" = hex 5B | 127 = DEL
- Selisih A vs a = 32 decimal / 20 hex
- "TryHackMe" hex → 54 72 79 48 61 63 6b 4d 65 0a

### Extended ASCII & Masalah

- ISO-8859-1 (Latin-1) — Eropa Barat (ß, é, ñ, ü)
- ISO-8859-2 (Latin-2) — Eropa Tengah/Timur (ł, č, ő)
- Windows-1252 — standar Microsoft regional
- Masalah — beda standar = gibberish
- Jepang: 6.879 karakter (JIS X 0208)
- Cina: 87.887 Hanzi (GB 18030-2022)

### Unicode

- Standar universal, semua bahasa, 1 sistem
- Versi 17.0 — ~157.000 karakter, ~4.000 emoji
- Format code point: U+XXXX
- U+0041 = A | U+03A9 = Ω | U+3042 = あ
- U+9F8D = 龍 (dragon/Kali) | U+1F60A = 😊
- U+30C4 = ツ | U+062A = ت | U+265E = ♞
- U+1F525 = 🔥 (4 bytes di UTF-8)

### UTF-8 / UTF-16 / UTF-32

- Semua merepresentasikan Unicode yang SAMA
- UTF-8 — 1-4 bytes dinamis | paling umum di web | backward compat ASCII
- UTF-16 — 2 atau 4 bytes | surrogate pair untuk emoji | Windows/Java
- UTF-32 — selalu 4 bytes | simpel tapi boros | pemrosesan internal
- Surrogate pair — dua 16-bit unit untuk karakter di luar BMP
- BMP — U+0000 s.d. U+FFFF
- Backward compat — U+0000–U+007F di UTF-8 identik dengan ASCII
