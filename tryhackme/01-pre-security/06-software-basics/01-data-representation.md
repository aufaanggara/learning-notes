# Resume Materi: Representing Colors & Numbers
**TryHackMe — Pre-Security Path**
**Tanggal:** 31 Agustus 2026

---

## 1. Sistem Bilangan

Komputer hanya mengenal dua kondisi fisik — ada atau tidaknya arus listrik, polaritas magnet, ada atau tidaknya cahaya — yang dikodekan sebagai **0** dan **1**. Dari sinilah seluruh sistem representasi data komputer bertumpu.

Ada empat sistem bilangan yang perlu dipahami:

| Sistem | Basis | Digit yang Digunakan | Pengelompokan Bit |
|---|---|---|---|
| Desimal | 10 | 0–9 | — |
| Biner | 2 | 0–1 | Per bit |
| Heksadesimal | 16 | 0–9 dan A–F | 4 bit per digit |
| Oktal | 8 | 0–7 | 3 bit per digit |

Sistem desimal adalah sistem yang digunakan manusia sehari-hari. Tiga sistem lainnya adalah cara komputer dan analis merepresentasikan data secara efisien.


## 2. Biner dan Bit

### 2.1 Definisi

**Bit** adalah singkatan dari *binary digit* — unit data terkecil, bernilai 0 atau 1.

**Byte** adalah kumpulan 8 bit. Istilah lainnya adalah **octet**. Satu byte mampu merepresentasikan 2⁸ = **256 nilai berbeda** (0 sampai 255).

### 2.2 Cara Konversi Biner ke Desimal

Setiap posisi bit memiliki nilai berdasarkan pangkat 2, dihitung dari kanan ke kiri mulai dari 2⁰:

```
Posisi :  7    6    5    4    3    2    1    0
Nilai  : 128   64   32   16    8    4    2    1
```

Aturannya sederhana: jika bit bernilai **1**, nilai posisinya ikut dijumlahkan. Jika **0**, diabaikan.

Contoh — `10111100`:

```
128 + 0 + 32 + 16 + 8 + 4 + 0 + 0 = 188
```

Nilai posisi bisa dihafal dengan cara: mulai dari 1 di paling kanan, lalu gandakan ke kiri — `1, 2, 4, 8, 16, 32, 64, 128`.

### 2.3 Tabel Biner 4-bit (0–15)

Tabel ini penting karena langsung berkaitan dengan heksadesimal:

| Desimal | Biner | Desimal | Biner |
|---|---|---|---|
| 0 | 0000 | 8 | 1000 |
| 1 | 0001 | 9 | 1001 |
| 2 | 0010 | 10 | 1010 |
| 3 | 0011 | 11 | 1011 |
| 4 | 0100 | 12 | 1100 |
| 5 | 0101 | 13 | 1101 |
| 6 | 0110 | 14 | 1110 |
| 7 | 0111 | 15 | 1111 |

### 2.4 Pola Biner untuk Hafalan Cepat

Daripada menghafal satu per satu, kenali polanya per kolom dari kanan:

- Bit paling kanan: selang-seling `0, 1, 0, 1, ...`
- Bit kedua dari kanan: berubah setiap dua angka `0, 0, 1, 1, 0, 0, 1, 1, ...`
- Bit ketiga dari kanan: berubah setiap empat angka `0, 0, 0, 0, 1, 1, 1, 1, ...`
- Bit paling kiri: berubah setiap delapan angka


## 3. Heksadesimal

### 3.1 Mengapa Heksadesimal?

Biner 24-bit seperti `10100011 11101010 00101010` tidak praktis dibaca atau diketik. Heksadesimal memampatkan setiap 4 bit menjadi satu karakter tunggal, sehingga string panjang tersebut bisa ditulis sebagai `A3EA2A` — jauh lebih singkat dan mudah dibaca.

### 3.2 Tabel Heksadesimal

Digit hex 0–9 sama persis dengan desimal. Untuk nilai 10–15, digunakan huruf:

| Desimal | Hex | Desimal | Hex |
|---|---|---|---|
| 0–9 | 0–9 | 10 | A |
| 11 | B | 12 | C |
| 13 | D | 14 | E |
| 15 | F | | |

Hafalan cepat: setelah angka 9, lanjutkan dengan `A B C D E F` yang mewakili nilai `10 11 12 13 14 15`.

### 3.3 Konversi Heksadesimal ke Desimal

Pendekatan sama seperti biner, tapi basis yang digunakan adalah 16.

Contoh — `9BDF`:

```
9×16³ + 11×16² + 13×16¹ + 15×16⁰
= 9×4096 + 11×256 + 13×16 + 15×1
= 36864 + 2816 + 208 + 15
= 39.903
```

Huruf B = 11, D = 13, F = 15 (gunakan nilai desimalnya saat menghitung).

### 3.4 Hubungan Bit dan Digit Hex

- 4 bit = 1 digit heksadesimal
- 8 bit (1 byte) = 2 digit heksadesimal
- 24 bit (3 byte) = 6 digit heksadesimal


## 4. Oktal (Opsional)

Sistem oktal menggunakan basis 8 dengan digit 0–7. Setiap 3 bit dikelompokkan menjadi satu digit oktal. Sistem ini jarang ditemui di komputer modern dibandingkan heksadesimal.

Konversi oktal ke desimal menggunakan pangkat 8:

Contoh — `357`:

```
3×8² + 5×8¹ + 7×8⁰
= 3×64 + 5×8 + 7×1
= 192 + 40 + 7
= 239
```

| Desimal | Oktal | Biner |
|---|---|---|
| 0 | 0 | 000 |
| 1 | 1 | 001 |
| 2 | 2 | 010 |
| 3 | 3 | 011 |
| 4 | 4 | 100 |
| 5 | 5 | 101 |
| 6 | 6 | 110 |
| 7 | 7 | 111 |


## 5. Representasi Warna

### 5.1 Model RGB

Semua warna di komputer dibangun dari tiga komponen cahaya: **Red**, **Green**, dan **Blue**. Masing-masing dikontrol secara independen — bayangkan seperti tiga kenop yang bisa diputar.

### 5.2 Delapan Warna (3 bit)

Jika setiap lampu hanya bisa ON atau OFF, total kombinasi adalah 2×2×2 = **8 warna**.

| Biner | Kondisi | Warna |
|---|---|---|
| 000 | Semua mati | Hitam |
| 001 | Hanya biru | Biru |
| 010 | Hanya hijau | Hijau |
| 100 | Hanya merah | Merah |
| 011 | Hijau + biru | Cyan |
| 101 | Merah + biru | Magenta |
| 110 | Merah + hijau | Kuning |
| 111 | Semua nyala | Putih |

### 5.3 Lebih dari 16 Juta Warna (24 bit)

Jika setiap channel memiliki 256 tingkat intensitas (bukan sekadar ON/OFF):

```
256 × 256 × 256 = 16.777.216 warna
```

Ini dicapai dengan mengalokasikan **8 bit per channel**:

- Total: 3 channel × 8 bit = **24 bit = 3 byte**
- Setiap channel: nilai 0–255 (desimal), atau 00–FF (heksadesimal)
- Satu warna ditulis sebagai 6 digit hex, contoh: `A3EA2A`

### 5.4 Membaca Kode Warna Hex

Kode hex 6 digit dibagi tiga pasang, masing-masing mewakili satu channel:

Contoh — `BC002D`:

| Pasangan Hex | Channel | Nilai Desimal | Nilai Biner |
|---|---|---|---|
| BC | Red | 188 | 10111100 |
| 00 | Green | 0 | 00000000 |
| 2D | Blue | 45 | 00101101 |

Warna ini menghasilkan merah tua karena intensitas merah sangat tinggi (188), hijau tidak ada (0), dan biru sangat kecil (45).


## 6. Logika Matematika di Balik Sistem Bilangan

Semua sistem bilangan bekerja dengan prinsip yang sama: setiap digit dikalikan dengan nilai posisinya (basis dipangkatkan posisi dari kanan), lalu dijumlahkan.

| Sistem | Basis | Nilai posisi (dari kanan) |
|---|---|---|
| Desimal | 10 | 1, 10, 100, 1000, ... |
| Biner | 2 | 1, 2, 4, 8, 16, 32, 64, 128, ... |
| Heksadesimal | 16 | 1, 16, 256, 4096, ... |
| Oktal | 8 | 1, 8, 64, 512, ... |

Contoh desimal — `213`:

```
2×10² + 1×10¹ + 3×10⁰
= 200 + 10 + 3
= 213
```

Prinsip yang sama berlaku untuk biner, hex, dan oktal — hanya basesnya yang berbeda.


## 7. Glossary

**Bit** — Binary digit; unit data terkecil, bernilai 0 atau 1.

**Byte** — Kumpulan 8 bit; mampu merepresentasikan 256 nilai berbeda (0–255). Disebut juga octet.

**Octet** — Istilah lain untuk byte (8 bit).

**Basis** — Jumlah digit berbeda yang digunakan dalam suatu sistem bilangan (desimal=10, biner=2, hex=16, oktal=8).

**RGB** — Red Green Blue; model warna yang digunakan komputer untuk merepresentasikan semua warna sebagai kombinasi tiga channel cahaya.

**Channel** — Satu komponen warna dalam model RGB (R, G, atau B), masing-masing bernilai 0–255.

**Hex color** — Representasi warna dalam 6 digit heksadesimal, terdiri dari tiga pasang yang mewakili R, G, dan B.

**True Color** — Sistem warna 24-bit yang mendukung lebih dari 16 juta kombinasi warna.

**Intensity** — Tingkat kekuatan/kecerahan suatu channel warna, berkisar dari 0 (mati) hingga 255 (maksimum).

**Transistor** — Komponen elektronik di dalam komputer yang hanya memiliki dua kondisi (melewatkan atau memblokir arus listrik), dasar dari representasi biner.


## 8. Tools & Platform Rujukan

Tidak ada tools atau platform eksternal yang secara eksplisit disebutkan di materi room ini, kecuali satu static site bawaan TryHackMe yang disediakan langsung di dalam room (diakses via tombol "View Site") untuk mengonversi kode hex ke biner, desimal, dan pratinjau warna. Platform tersebut tidak memiliki URL publik yang disebutkan secara eksplisit di materi.


---

## 9. Catatan Ringkas untuk Ditulis Tangan

### Sistem Bilangan

- Desimal — basis 10, digit 0–9, sistem manusia sehari-hari
- Biner — basis 2, digit 0–1, bahasa dasar komputer
- Heksadesimal — basis 16, digit 0–9 dan A–F, 4 bit per digit
- Oktal — basis 8, digit 0–7, 3 bit per digit (jarang dipakai)

### Bit & Byte

- Bit — binary digit, nilai 0 atau 1
- Byte / Octet — 8 bit, mewakili 256 nilai (0–255)
- 1 bit → 2 kondisi | 8 bit → 256 kondisi

### Konversi Biner → Desimal

- Nilai posisi (kanan ke kiri): 1, 2, 4, 8, 16, 32, 64, 128
- Bit 1 → nilai posisi dijumlah | Bit 0 → diabaikan
- Contoh: 10111100 = 128+32+16+8+4 = 188

### Konversi Hex → Desimal

- A=10, B=11, C=12, D=13, E=14, F=15
- Rumus: tiap digit × 16^posisi (dari kanan mulai 0)
- Contoh: 9BDF = 9×4096 + 11×256 + 13×16 + 15×1 = 39.903

### Konversi Oktal → Desimal

- Rumus: tiap digit × 8^posisi (dari kanan mulai 0)
- Contoh: 357 = 3×64 + 5×8 + 7×1 = 239

### Hubungan Bit dan Digit

- 4 bit = 1 digit hex
- 8 bit = 2 digit hex
- 3 bit = 1 digit oktal

### Representasi Warna RGB

- Model: Red + Green + Blue, masing-masing dikontrol independen
- 3 bit (ON/OFF) → 2³ = 8 warna
- 8 bit per channel → 256 level intensitas per warna
- 3 channel × 8 bit = 24 bit = 3 byte → 16.777.216 warna

### 8 Warna Dasar (hafal)

- 000 = Hitam | 111 = Putih
- 100 = Merah | 010 = Hijau | 001 = Biru
- 110 = Kuning | 101 = Magenta | 011 = Cyan

### Kode Warna Hex

- Format: 6 digit hex = 3 pasang (RR GG BB)
- Tiap pasang = 1 byte = nilai 00–FF = desimal 0–255
- Contoh: BC002D → R=188, G=0, B=45 → merah tua

### Pola Biner (hafalan cepat)

- Kolom paling kanan: 0,1,0,1,0,1...
- Kolom kedua kanan: 0,0,1,1,0,0,1,1...
- Kolom ketiga kanan: 0,0,0,0,1,1,1,1...
- Kolom paling kiri: 0×8 lalu 1×8...
