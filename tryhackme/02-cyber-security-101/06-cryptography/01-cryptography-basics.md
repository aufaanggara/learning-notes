# Cryptography Basics — Resume Materi
**Room:** Cryptography Basics (TryHackMe)
**Tanggal:** 22 Oktober 2026

---

## 1. Fondasi Kriptografi

### 1.1 Definisi dan Tujuan

**Kriptografi** adalah praktik dan studi teknik untuk komunikasi yang aman serta perlindungan data, khususnya di hadapan pihak yang berpotensi menjadi ancaman (**adversaries**).

Tujuan utamanya: memastikan **secure communication in the presence of adversaries** — tidak ada pihak ketiga yang bisa membaca atau mengubah data yang dipertukarkan.

Kriptografi melindungi tiga aspek utama:

- **Confidentiality** — data hanya bisa dibaca oleh pihak yang berhak
- **Integrity** — data tidak bisa diubah tanpa terdeteksi
- **Authenticity** — memastikan identitas pihak yang berkomunikasi benar-benar sah

### 1.2 Peran Kriptografi di Dunia Digital

Kriptografi bekerja di balik layar hampir di semua aktivitas digital. Beberapa skenario nyata:

- Login ke website → kredensial dienkripsi sebelum dikirim ke server
- Koneksi SSH → membangun tunnel terenkripsi agar tidak bisa disadap
- Online banking → browser memverifikasi sertifikat server untuk memastikan koneksi ke bank asli, bukan server penyerang
- Download file → hash function memverifikasi file yang diunduh identik dengan file asli di server

---

## 2. Istilah Inti Kriptografi

Semua istilah ini wajib dipahami sebelum membaca materi kriptografi lebih lanjut.

**Plaintext** — pesan atau data asli yang bisa dibaca sebelum dienkripsi. Bisa berupa dokumen, gambar, file multimedia, atau data biner apapun.

**Ciphertext** — versi acak dan tidak terbaca dari plaintext setelah enkripsi. Idealnya, tidak ada informasi apapun yang bisa diekstrak dari ciphertext selain perkiraan ukurannya.

**Cipher** — algoritma atau metode untuk mengubah plaintext menjadi ciphertext dan sebaliknya. Cipher biasanya dikembangkan oleh matematikawan dan bersifat pengetahuan publik — yang dirahasiakan bukan cipher-nya, melainkan key-nya.

**Key** — string bit yang digunakan cipher untuk mengenkripsi atau mendekripsi data. Harus dijaga kerahasiaannya, kecuali dalam kasus public key pada asymmetric encryption.

**Encryption** — proses mengubah plaintext menjadi ciphertext menggunakan cipher dan key. Pilihan cipher boleh diungkapkan; key tidak.

**Decryption** — proses kebalikan enkripsi: mengubah ciphertext kembali menjadi plaintext menggunakan cipher dan key yang sama. Tanpa key yang benar, mendapatkan plaintext kembali seharusnya mustahil secara praktis.

### 2.1 Alur Enkripsi dan Dekripsi

```
Enkripsi: Sender → Plaintext → [Encrypt + Key] → Ciphertext
Dekripsi:        → Ciphertext → [Decrypt + Key] → Plaintext → Recipient
```

---

## 3. Regulasi Kriptografi

Kriptografi bukan hanya pilihan teknis — di banyak industri, penggunaannya diwajibkan oleh regulasi.

**PCI DSS** (Payment Card Industry Data Security Standard) — wajib bagi perusahaan yang menangani kartu kredit. Data harus dienkripsi baik saat disimpan (**at rest**) maupun saat dikirim (**in motion**).

Untuk rekam medis, standar berbeda-beda per negara:

| Regulasi | Wilayah | Cakupan |
|----------|---------|---------|
| HIPAA | USA | Health Insurance Portability & Accountability Act |
| HITECH | USA | Health Information Technology for Economic & Clinical Health |
| GDPR | EU | General Data Protection Regulation |
| DPA | UK | Data Protection Act |

Kriptografi pada regulasi ini bersifat keharusan yang tersembunyi dari pengguna langsung — ada di balik sistem, bukan sesuatu yang pengguna atur sendiri.

---

## 4. Caesar Cipher

### 4.1 Cara Kerja

Caesar Cipher berasal dari abad pertama SM dan merupakan salah satu cipher historis paling sederhana. Prinsipnya: **geser setiap huruf sejumlah N posisi ke kanan** dalam alfabet. Saat mencapai Z, perhitungan dimulai lagi dari A (wrapping).

Contoh dengan key = 3:

```
Plaintext  : T R Y H A C K M E
Ciphertext : W U B K D F N P H
```

Dekripsi dilakukan dengan menggeser ke arah sebaliknya (ke kiri) sebesar nilai key yang sama.

### 4.2 Kelemahan

Alfabet bahasa Inggris terdiri dari 26 huruf. Menggeser sebanyak 26 menghasilkan huruf yang sama persis, sehingga hanya ada **25 kemungkinan key yang valid**. Karena cipher-nya sudah diketahui publik dan jumlah key-nya sangat sedikit, ciphertext bisa dipecahkan dengan mencoba semua 25 kemungkinan (**brute force**) — tanpa perlu pengetahuan khusus.

Berdasarkan standar modern, Caesar Cipher dianggap **tidak aman**.

### 4.3 Cipher Historis Lainnya

- **Vigenère cipher** — abad ke-16, menggunakan kata kunci bukan angka tunggal
- **Enigma machine** — digunakan pada Perang Dunia II, mekanisme rotor yang kompleks
- **One-time pad** — digunakan era Perang Dingin, satu-satunya cipher yang terbukti sempurna secara matematis jika digunakan dengan benar

---

## 5. Jenis-Jenis Enkripsi

### 5.1 Symmetric Encryption

**Symmetric encryption** (juga disebut **private key cryptography**) menggunakan **satu kunci yang sama** untuk enkripsi maupun dekripsi. Kunci ini harus dibagi antara pengirim dan penerima.

```
Alice → [Encrypt + Shared Secret Key] → Ciphertext → [Decrypt + Shared Secret Key] → Bob
```

Masalah utama: **distribusi key**. Jika key dikirim melalui jalur yang sama dengan ciphertext, siapapun yang mencegat komunikasi bisa mendapatkan keduanya. Satu-satunya solusi terjamin adalah bertukar key secara langsung (tatap muka).

**Contoh algoritma symmetric encryption:**

**DES** (Data Encryption Standard)
- Diadopsi sebagai standar tahun 1977
- Menggunakan key 56-bit
- Dipecahkan dalam waktu kurang dari 24 jam pada tahun 1999
- Mendorong transisi ke 3DES

**3DES** (Triple DES)
- DES yang diterapkan tiga kali berturut-turut
- Ukuran key: 168-bit, namun keamanan efektifnya hanya 112-bit
- Solusi sementara (ad-hoc) ketika DES tidak lagi aman
- Deprecated sejak 2019, digantikan AES
- Masih mungkin ditemukan di sistem lama (legacy systems)

**AES** (Advanced Encryption Standard)
- Diadopsi sebagai standar tahun 2001
- Ukuran key: 128, 192, atau 256-bit
- Standar symmetric encryption yang digunakan saat ini

### 5.2 Asymmetric Encryption

**Asymmetric encryption** (juga disebut **public key cryptography**) menggunakan **dua kunci yang berbeda**: satu untuk enkripsi dan satu untuk dekripsi.

```
Alice → [Encrypt + Bob's Public Key] → Ciphertext → [Decrypt + Bob's Private Key] → Bob
```

Konsep kunci:
- Data dienkripsi menggunakan **public key** milik penerima
- Data hanya bisa didekripsi menggunakan **private key** milik penerima
- Public key boleh dibagikan ke siapapun
- Private key **wajib** dirahasiakan

Asymmetric encryption **lebih lambat** dari symmetric dan menggunakan key yang lebih besar.

**Contoh algoritma asymmetric encryption:**

**RSA**
- Menggunakan key 2048-bit, 3072-bit, atau 4096-bit
- Ukuran key minimum yang direkomendasikan: 2048-bit

**Diffie-Hellman**
- Ukuran key minimum yang direkomendasikan: 2048-bit
- Untuk keamanan yang lebih tinggi: 3072-bit atau 4096-bit

**ECC** (Elliptic Curve Cryptography)
- Mencapai keamanan setara dengan key yang jauh lebih pendek
- Key 256-bit pada ECC setara dengan key 3072-bit pada RSA

### 5.3 Dasar Matematis Asymmetric Encryption

Asymmetric encryption bergantung pada masalah matematika yang **mudah dihitung dalam satu arah tapi sangat sulit dibalik**. "Sangat sulit" di sini berarti secara praktis tidak mungkin diselesaikan — bahkan dengan teknologi komputasi saat ini bisa membutuhkan jutaan tahun.

### 5.4 Perbandingan Symmetric vs Asymmetric

| Aspek | Symmetric | Asymmetric |
|-------|-----------|------------|
| Jumlah key | 1 (shared) | 2 (public + private) |
| Kecepatan | Cepat | Lebih lambat |
| Ukuran key | Lebih kecil | Lebih besar |
| Masalah utama | Distribusi key | Tidak ada (public key bebas dibagi) |
| Nama lain | Private key cryptography | Public key cryptography |
| Contoh | AES, 3DES, DES | RSA, ECC, Diffie-Hellman |

---

## 6. Operasi Matematika Dasar Kriptografi

### 6.1 XOR Operation

**XOR** (Exclusive OR) adalah operasi logika biner yang membandingkan dua bit dan menghasilkan:
- **1** jika kedua bit **berbeda**
- **0** jika kedua bit **sama**

Simbol: **⊕** atau **^**

Truth table XOR:

| A | B | A ⊕ B |
|---|---|--------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

**Sifat-sifat XOR yang penting:**

- `A ⊕ A = 0` — nilai XOR dengan dirinya sendiri selalu menghasilkan 0
- `A ⊕ 0 = A` — nilai XOR dengan 0 tidak berubah
- `A ⊕ B = B ⊕ A` — bersifat komutatif
- `(A ⊕ B) ⊕ C = A ⊕ (B ⊕ C)` — bersifat asosiatif

**Contoh XOR pada bilangan biner:**

```
  1010
⊕ 1100
------
  0110

Bit per bit: 1⊕1=0, 0⊕1=1, 1⊕0=1, 0⊕0=0
```

**XOR sebagai dasar enkripsi simetris:**

```
P = plaintext
K = secret key
C = ciphertext

Enkripsi: C = P ⊕ K
Dekripsi: C ⊕ K = (P ⊕ K) ⊕ K = P ⊕ (K ⊕ K) = P ⊕ 0 = P
```

Bukti matematis dekripsi bekerja karena sifat asosiatif XOR dan karena `K ⊕ K = 0`. Dalam praktiknya, implementasi XOR murni membutuhkan secret key yang panjangnya sama dengan plaintext — ini tidak praktis untuk penggunaan umum.

### 6.2 Modulo Operation

**Modulo** menghasilkan **sisa pembagian**, bukan hasil baginya. Ditulis sebagai `%` atau `mod`.

```
X % Y = sisa ketika X dibagi Y
```

Contoh:

```
25 % 5 = 0   →  25 = 5 × 5 + 0
23 % 6 = 5   →  23 = 3 × 6 + 5
23 % 7 = 2   →  23 = 3 × 7 + 2
```

**Sifat-sifat modulo yang penting:**

- Hasil selalu **non-negatif** dan selalu lebih kecil dari pembagi
- Untuk integer `a` dan bilangan positif `n`: hasil `a % n` selalu berada di rentang **0 sampai n−1**
- **Tidak bisa dibalik (irreversible)** — jika diketahui `x % 5 = 4`, tidak ada cara menentukan nilai `x` secara pasti karena tak terhingga nilai x yang memenuhi

Sifat tidak bisa dibalik inilah yang membuat modulo sangat berguna dalam kriptografi — mudah dihitung ke satu arah, sangat sulit dibalik.

**Menghitung modulo untuk angka besar:**

Kalkulator biasa sering tidak mampu menangani angka kriptografi yang sangat besar. Alternatif:
- **Python** — tipe `int` bawaan bisa menangani bilangan bulat sembarang besar, otomatis beralih ke tipe yang lebih besar sesuai kebutuhan
- **WolframAlpha** — cukup ketik `[angka] mod [pembagi]` di kotak pencarian

---

## 7. Glossary

**Adversary** — pihak yang berusaha mengakses, membaca, atau memanipulasi komunikasi tanpa izin.

**at rest** — kondisi data yang sedang disimpan (tidak bergerak/tidak dikirim).

**in motion** — kondisi data yang sedang dikirimkan melalui jaringan.

**Brute force** — metode memecahkan enkripsi dengan mencoba semua kemungkinan key secara sistematis.

**Deprecated** — status suatu teknologi/standar yang sudah tidak direkomendasikan untuk digunakan dan akan/sudah dihentikan.

**Legacy system** — sistem lama yang masih beroperasi meskipun teknologinya sudah usang atau digantikan.

**Infeasible** — secara teknis mungkin dilakukan, tetapi membutuhkan waktu atau sumber daya yang secara praktis mustahil (misalnya jutaan tahun komputasi).

**Wrapping** — perilaku alfabet pada Caesar Cipher di mana setelah Z, perhitungan kembali ke A.

**Alice dan Bob** — nama karakter fiktif yang menjadi konvensi standar dalam contoh-contoh kriptografi untuk merepresentasikan dua pihak yang berkomunikasi.

**Private key cryptography** — nama lain untuk symmetric encryption, menekankan bahwa key harus tetap privat.

**Public key cryptography** — nama lain untuk asymmetric encryption, menekankan penggunaan public key dalam proses enkripsi.

---

## 8. Tools & Platform Rujukan

**Cryptii** — platform interaktif berbasis web untuk bereksperimen dengan berbagai cipher termasuk Caesar Cipher, Vigenère, dan Enigma. Memungkinkan pengaturan rotasi/shift secara langsung dan melihat transformasi teks secara real-time.
URL: https://cryptii.com

**WolframAlpha** — mesin komputasi berbasis web untuk menyelesaikan perhitungan matematika kompleks, termasuk modulo dengan angka sangat besar. Cara pakai: ketik `[angka] mod [pembagi]` di kotak pencarian.
URL: https://www.wolframalpha.com

**Python** — bahasa pemrograman dengan tipe `int` bawaan yang mampu menangani bilangan bulat berukuran sembarang. Direkomendasikan untuk perhitungan kriptografi yang melibatkan angka sangat besar ketika kalkulator biasa tidak mampu.

---

## 9. Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar

- Kriptografi — praktik komunikasi aman dari ancaman pihak ketiga
- Tujuan — Confidentiality, Integrity, Authenticity
- Plaintext — data asli, bisa dibaca
- Ciphertext — data terenkripsi, tidak bisa dibaca
- Cipher — algoritma enkripsi/dekripsi (publik)
- Key — kunci rahasia untuk enkripsi/dekripsi
- Encryption — plaintext + key → ciphertext
- Decryption — ciphertext + key → plaintext

### Regulasi

- PCI DSS — kartu kredit, data harus enkripsi at rest & in motion
- HIPAA / HITECH — rekam medis di USA
- GDPR — data pribadi di EU
- DPA — data di UK
- at rest — data disimpan
- in motion — data dikirim

### Caesar Cipher

- Cara kerja — geser huruf N posisi ke kanan, Z wrap ke A
- Key — angka pergeseran (shift)
- Dekripsi — geser ke kiri sebesar key
- Kelemahan — hanya 25 kemungkinan key → mudah brute force
- Status — tidak aman di standar modern

### Cipher Historis Lain

- Vigenère — abad 16, keyword bukan angka
- Enigma — WW2, mesin rotor
- One-time pad — Perang Dingin, terbukti sempurna matematika

### Symmetric Encryption (Private Key)

- Satu kunci untuk enkripsi & dekripsi
- Masalah utama — distribusi key secara aman
- DES — 1977, 56-bit, dipecahkan 1999
- 3DES — DES × 3, 168-bit (efektif 112), deprecated 2019
- AES — 2001, 128/192/256-bit, standar saat ini

### Asymmetric Encryption (Public Key)

- Dua kunci — public key (enkripsi) & private key (dekripsi)
- Public key — bebas dibagikan
- Private key — wajib dirahasiakan
- RSA — 2048/3072/4096-bit, min rekomendasi 2048
- Diffie-Hellman — min rekomendasi 2048, enhanced 3072/4096
- ECC — lebih efisien, 256-bit ECC ≈ 3072-bit RSA
- Dasar — masalah matematika mudah satu arah, sangat sulit dibalik

### Perbandingan Symmetric vs Asymmetric

- Symmetric — 1 key, cepat, masalah: distribusi key
- Asymmetric — 2 key, lambat, tidak ada masalah distribusi

### XOR Operation

- Simbol — ⊕ atau ^
- Aturan — beda bit = 1, sama bit = 0
- A ⊕ A = 0
- A ⊕ 0 = A
- Komutatif — A ⊕ B = B ⊕ A
- Asosiatif — (A⊕B)⊕C = A⊕(B⊕C)
- Enkripsi — C = P ⊕ K
- Dekripsi — P = C ⊕ K (operasi yang sama)

### Modulo Operation

- Simbol — % atau mod
- Definisi — sisa pembagian X oleh Y
- 25 % 5 = 0
- 23 % 6 = 5
- 23 % 7 = 2
- Hasil selalu 0 sampai (n-1)
- Tidak reversible — x%5=4 bisa infinite nilai x
- Tools — Python atau WolframAlpha untuk angka besar
