# Resume Materi: Python — Guess the Number
**Platform:** TryHackMe | **Tanggal:** 31 Agustus 2026

---

## 1. Tentang Python

Python adalah bahasa pemrograman **high-level** dan **general-purpose**.

- **High-level** berarti Python menyembunyikan detail teknis komputer seperti manajemen memori dan register CPU — programmer tidak perlu pusing dengan hal itu.
- **General-purpose** berarti Python bisa dipakai untuk berbagai keperluan: web applications, automation scripts, data science, dan machine learning.

---

## 2. Tiga Konsep Fundamental

Room ini berfokus pada tiga pilar utama pemrograman imperatif yang diimplementasikan dalam satu proyek game:

1. **Variables** — menyimpan data/nilai selama program berjalan
2. **Conditional statements** — membuat keputusan berdasarkan kondisi tertentu
3. **Iterations / loops** — mengulang eksekusi kode selama kondisi terpenuhi

---

## 3. Variables

Variabel adalah tempat menyimpan nilai. Nilai bisa berubah selama program berjalan.

Variabel yang dipakai di game ini:

| Variabel | Nilai Awal | Fungsi |
|----------|-----------|--------|
| `secret` | hasil `randint` | angka rahasia yang dipilih komputer |
| `guess`  | `0` | tebakan pengguna (0 karena di luar range 1–20) |
| `tries`  | `0` | penghitung jumlah percobaan |
| `text`   | hasil `input()` | menyimpan input mentah pengguna (bertipe string) |

Kenapa `guess` diinisialisasi ke `0`? Karena angka secret ada di range 1–20, nilai `0` mustahil sama dengan secret — ini mencegah loop langsung berhenti sebelum dimulai.

---

## 4. Fungsi dan Method Penting

### 4.1 Library `random`

```python
import random
```

Wajib di-import terlebih dahulu sebelum bisa menggunakan fungsi acak.

```python
random.randint(1, 20)
```

Menghasilkan bilangan bulat acak antara `a` dan `b` secara inklusif. Artinya `randint(1, 20)` bisa menghasilkan 1, 2, 3, ..., hingga 20.

### 4.2 Input dan Output

```python
print("teks")
```

Menampilkan teks atau nilai variabel ke layar. Bisa menerima beberapa argumen sekaligus, dipisah koma — hasilnya digabung dengan spasi.

```python
input("Take a guess: ")
```

Menampilkan prompt, lalu menunggu pengguna mengetik dan menekan Enter. Nilai yang dikembalikan **selalu bertipe string**, tidak peduli pengguna mengetik angka.

### 4.3 Konversi Tipe Data

```python
int(text)
```

Mengubah string menjadi integer. Diperlukan karena `input()` selalu mengembalikan string — kalau tidak dikonversi, perbandingan `guess < secret` tidak akan bekerja dengan benar.

---

## 5. Conditional Statements (if / elif / else)

Digunakan untuk membuat keputusan. Python mengeksekusi blok pertama yang kondisinya bernilai `True`, lalu melompati sisanya.

### 5.1 Struktur

```python
if KONDISI_1:
    # jalankan jika kondisi 1 true
elif KONDISI_2:
    # jalankan jika kondisi 1 false, kondisi 2 true
elif KONDISI_3:
    # jalankan jika kondisi 1 & 2 false, kondisi 3 true
else:
    # jalankan jika semua kondisi di atas false
```

- `if` selalu ada di posisi pertama
- `elif` (else if) boleh ada sebanyak yang dibutuhkan
- `else` tidak punya kondisi — otomatis dijalankan kalau semua di atasnya `False`

### 5.2 Operator Perbandingan

| Operator | Arti |
|----------|------|
| `<` | kurang dari |
| `>` | lebih dari |
| `==` | sama dengan |
| `!=` | tidak sama dengan |
| `or` | salah satu kondisi harus True |

### 5.3 Implementasi di Game

```python
if guess < 1 or guess > 20:
    print("That number is out of range. Try again.")
elif guess < secret:
    print("Too low, try again.")
elif guess > secret:
    print("Too high, try again.")
else:
    print("You got it in", tries, "tries!")
```

Kenapa `else` bisa langsung cetak "You got it"? Karena kalau `guess` sudah tidak kurang dari `secret` dan tidak lebih dari `secret`, satu-satunya kemungkinan yang tersisa adalah `guess == secret`.

---

## 6. Iterations / Loop (while)

Loop memungkinkan kode dijalankan berulang kali tanpa harus menulis ulang kode tersebut.

### 6.1 Struktur

```python
while KONDISI:
    # kode yang diulang (harus diindent)
```

Cara kerja:
1. Python cek kondisi
2. Kalau `True` → jalankan semua kode di dalam (indented), lalu balik ke langkah 1
3. Kalau `False` → keluar dari loop, lanjutkan kode setelah blok `while`

### 6.2 Implementasi di Game

```python
while guess != secret:
    text = input("Take a guess: ")
    guess = int(text)
    tries = tries + 1
    # ... if/elif/else di sini
```

Loop berjalan terus selama `guess != secret`. Begitu pengguna menebak dengan benar, kondisi menjadi `False` dan loop berhenti.

### 6.3 Analogi

> Seperti mencari tempat parkir di parkiran besar — kamu mengecek satu baris demi satu baris **sampai** menemukan tempat kosong. Setiap kali cek satu baris = satu iterasi.

---

## 7. Indentation (Penjorokan)

Python menggunakan indentation (spasi di awal baris) untuk menandai bahwa kode tersebut berada **di dalam** blok `if`, `elif`, `else`, atau `while`. Ini bukan sekadar gaya penulisan — Python akan error kalau indentation salah.

Standar: **4 spasi** per level.

---

## 8. Evolusi Program (v1 → v2 → v3)

| Versi | File | Yang Ditambahkan | Keterbatasan |
|-------|------|-----------------|-------------|
| v1 | `guess_v1.py` | Variabel + input + randint | Tidak membandingkan guess vs secret |
| v2 | `guess_v2.py` | if/elif/else untuk feedback | Hanya 1 kesempatan menebak |
| v3 | `guess_v3.py` | while loop | — (sudah lengkap) |

---

## 9. Kode Final Lengkap

```python
import random

# --------------------------
# Guess the Number (Beginner Demo)
# --------------------------
# The computer picks a secret number.
# The player keeps guessing until they find it.

secret = random.randint(1, 20)  # a <= secret <= b
tries = 0
guess = 0  # start with a value that cannot be the secret (since secret is 1..20)

print("I'm thinking of a number between 1 and 20")

# Repeat until the user guesses the secret number.
while guess != secret:
    text = input("Take a guess: ")   # input() returns text (a string)
    guess = int(text)                # convert the text to a number

    tries = tries + 1               # add 1 try

    # Give a hint using if / elif / else.
    if guess < 1 or guess > 20:
        print("That number is out of range. Try again.")
    elif guess < secret:
        print("Too low, try again.")
    elif guess > secret:
        print("Too high, try again.")
    else:
        print("You got it in", tries, "tries!")
```

---

## 10. Glossary

**String** — tipe data teks. Semua hasil `input()` bertipe string, bahkan kalau pengguna mengetik angka.

**Integer** — tipe data bilangan bulat. Diperlukan untuk operasi aritmatika dan perbandingan numerik.

**Library** — kumpulan fungsi siap pakai yang bisa di-import ke program. Contoh: `random`.

**Indentation** — penjorokan kode (4 spasi) sebagai tanda bahwa baris tersebut berada di dalam blok tertentu. Wajib di Python.

**Iteration** — satu kali perulangan loop. Setiap kali `while` mengeksekusi isi bloknya = satu iterasi.

**Body loop** — semua kode yang ada di dalam blok `while` (yang diindent).

**Pseudo-code** — penulisan logika program menggunakan bahasa manusia, sebelum dikonversi ke kode nyata. Berguna untuk merencanakan alur sebelum coding.

**High-level language** — bahasa pemrograman yang menyembunyikan detail teknis mesin, sehingga lebih mudah dipahami manusia.

**General-purpose language** — bahasa pemrograman yang bisa digunakan untuk berbagai jenis aplikasi, tidak terbatas satu domain saja.

**Type conversion** — mengubah nilai dari satu tipe data ke tipe lain. Contoh: `int("5")` mengubah string `"5"` menjadi integer `5`.

---

## 11. Tools & Platform Rujukan

Room ini tidak menyebutkan tools atau platform eksternal secara eksplisit. Seluruh praktik dilakukan menggunakan:

- **Visual Studio Code** — editor kode yang dibuka otomatis di VM TryHackMe
- **Python** (interpreter) — dijalankan langsung via terminal dengan perintah `python namafile.py`
- **TryHackMe VM** — environment Linux (Ubuntu) tempat semua file `.py` disimpan di `/home/ubuntu/Python-Demo/`

---

## 12. Catatan Ringkas untuk Ditulis Tangan

### Python — Dasar

- Python — high-level, general-purpose
- High-level — sembunyikan detail teknis mesin
- General-purpose — web, automation, data science, ML

### Tiga Konsep Utama

- Variable — tempat simpan data
- Conditional — keputusan (if/elif/else)
- Loop/Iteration — ulang kode (while)

### Variabel Game

- `secret` — angka rahasia, isi dari `randint(1,20)`
- `guess` — tebakan user, awal = 0
- `tries` — hitung percobaan, awal = 0
- `text` — input mentah user (string)

### Fungsi Kunci

- `import random` — muat library angka acak
- `random.randint(a, b)` — angka acak inklusif a sampai b
- `print(...)` — tampilkan ke layar
- `input("...")` — minta input user, selalu return **string**
- `int(x)` — konversi string → integer

### Conditional

- `if KONDISI:` — cek pertama
- `elif KONDISI:` — cek berikutnya jika sebelumnya false
- `else:` — dijalankan jika semua false
- Operator: `<` `>` `==` `!=` `or`

### While Loop

- `while KONDISI:` — ulangi selama kondisi True
- Berhenti saat kondisi False
- `while guess != secret:` — terus tanya sampai tebakan benar

### Indentation

- 4 spasi per level — wajib di Python, bukan opsional

### Evolusi Versi

- v1 `guess_v1.py` — variabel + input, belum ada perbandingan
- v2 `guess_v2.py` — tambah if/elif/else, hanya 1 tebakan
- v3 `guess_v3.py` — tambah while loop, tebakan tak terbatas

### Istilah Singkat

- String — tipe data teks
- Integer — tipe data bilangan bulat
- Type conversion — ubah tipe data, contoh `int("5")`
- Iteration — satu kali putaran loop
- Body loop — kode di dalam while (yang diindent)
- Pseudo-code — rencana logika pakai bahasa manusia
