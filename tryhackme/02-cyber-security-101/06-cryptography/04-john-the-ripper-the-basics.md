# John the Ripper — Resume Materi
**Room:** John the Ripper — The Basics | **Tanggal:** 21 Oktober 2026

---

## 1. Konsep Dasar

### 1.1 Apa itu Hash

**Hash** adalah representasi fixed-length dari data dengan panjang berapa pun, dihasilkan oleh algoritma hashing. Sifat utamanya adalah **satu arah** — mudah menghasilkan hash dari input, tapi secara komputasi mustahil menemukan input asli dari hash yang ada.

Contoh nyata dengan MD5:
- Input `polo` (4 karakter) → output `b53759f3ce692de7aff1b5779d3964da` (32 karakter)
- Input `polomints` (9 karakter) → output `584b6e4f4586e136bc280f27f9c64f3b` (32 karakter)

Panjang input tidak mempengaruhi panjang output — selalu fixed sesuai algoritma.

### 1.2 Mengapa Hash Aman (dan Mengapa Bisa Di-crack)

Keamanan hash berakar dari konsep **P vs NP** dalam ilmu komputer:
- **P (Polynomial Time):** masalah yang solusinya bisa ditemukan dalam waktu wajar. Membuat hash termasuk kategori ini.
- **NP (Non-deterministic Polynomial Time):** solusinya sulit ditemukan tapi mudah diverifikasi. Membalik hash (un-hashing) termasuk kategori ini — secara teori tidak layak dikerjakan dengan komputer standar.

Meskipun begitu, hash tetap bisa di-crack dengan **dictionary attack**: hash ribuan/jutaan kata dari wordlist, lalu bandingkan hasilnya dengan hash target. Jika cocok, password ditemukan. Inilah yang dilakukan John the Ripper.

### 1.3 Apa itu John the Ripper

**John the Ripper** adalah tool pemecah hash yang cepat, serbaguna, dan mendukung ratusan tipe hash. Tersedia dalam dua varian:

- **Core:** distribusi standar, fitur terbatas
- **Jumbo John:** varian extended paling populer, include semua tools tambahan seperti `zip2john`, `rar2john`, `ssh2john`, dan ratusan custom rules bawaan

Kali Linux secara default sudah menyertakan Jumbo John. Verifikasi dengan menjalankan:

```bash
john --version
```

---

## 2. Wordlist

Untuk melakukan dictionary attack, John membutuhkan **wordlist** — daftar kata yang akan di-hash satu per satu dan dibandingkan dengan target.

**RockYou** (`rockyou.txt`) adalah wordlist paling populer, berisi sekitar 14 juta password nyata dari kebocoran data situs rockyou.com tahun 2009. Di Kali Linux dan AttackBox, lokasinya ada di:

```
/usr/share/wordlists/rockyou.txt
```

Koleksi wordlist tambahan yang lebih lengkap tersedia di repositori **SecLists**, khususnya di subdirektori `/Passwords/Leaked-Databases/`. Untuk mengekstrak rockyou dari format `.tar.gz`:

```bash
tar xvzf rockyou.txt.tar.gz
```

---

## 3. Identifikasi Tipe Hash

Sebelum crack, hash perlu diidentifikasi tipe-nya agar format yang tepat bisa ditentukan.

### 3.1 Tools Identifikasi

**hashid** — tool modern, langsung di CLI, output bersih:
```bash
hashid [hash]
```

**hash-identifier** — tool lama dengan tampilan ASCII art, mode interaktif:
```bash
hash-identifier
hash-identifier [hash]
```

Keduanya menghasilkan daftar kemungkinan tipe hash berurutan dari yang paling mungkin. Perhatikan tipe di urutan teratas sebagai kandidat utama.

### 3.2 Identifier Format dalam Hash Linux

Hash yang disimpan di `/etc/shadow` Linux menyertakan identifier format secara eksplisit — karakter setelah tanda `$` pertama:

| Identifier | Format |
|---|---|
| `$1$` | md5crypt |
| `$5$` | sha256crypt |
| `$6$` | sha512crypt (paling umum di Linux modern) |
| `$y$` | yescrypt (Kali Linux modern, sangat lambat di-crack) |

### 3.3 Mencari Format yang Tepat di John

Setelah tipe hash diketahui, cari nama format yang dikenali John:

```bash
john --list=formats | grep -iF "nama_hash"
```

Flag grep:
- `-i` = ignore case (tidak peduli huruf besar/kecil)
- `-F` = fixed string (cari teks harfiah, bukan regex pattern)

**Pola penamaan format standar (hash polos):** prefiks `Raw-` + nama tipe

| Tipe Hash | Format John |
|---|---|
| MD5 | `Raw-MD5` |
| SHA1 | `Raw-SHA1` |
| SHA256 | `Raw-SHA256` |
| SHA512 | `Raw-SHA512` |
| MD4 | `Raw-MD4` |
| NTLM | `NT` (pengecualian — tanpa Raw-) |
| Whirlpool | `Whirlpool` (pengecualian — tanpa Raw-) |
| sha512crypt | `sha512crypt` |
| yescrypt | `crypt` |

---

## 4. Sintaks dan Mode Cracking

### 4.1 Sintaks Dasar

```bash
john [options] [file path]
```

- `john` — memanggil program
- `[options]` — flag dan modifier
- `[file path]` — file berisi hash yang akan di-crack

### 4.2 Mode Automatic (tanpa format)

John mendeteksi tipe hash secara otomatis. Tidak selalu akurat, tapi berguna jika tipe hash tidak diketahui:

```bash
john --wordlist=[path wordlist] [file hash]
```

Contoh:
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

### 4.3 Mode Format-Specific

Format ditentukan secara eksplisit untuk hasil lebih akurat dan cepat:

```bash
john --format=[format] --wordlist=[path wordlist] [file hash]
```

Contoh:
```bash
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

### 4.4 Melihat Hasil yang Sudah Di-crack

John menyimpan semua hasil crack di `~/.john/john.pot`. Hash yang pernah di-crack tidak akan diproses ulang. Untuk melihat hasil:

```bash
john --show [file hash]
```

---

## 5. Alur Crack Hash Standar

1. Lihat isi file hash: `cat [file]`
2. Identifikasi tipe hash: `hashid [hash]`
3. Cari format John: `john --list=formats | grep -iF "[tipe]"`
4. Crack: `john --format=[format] --wordlist=rockyou.txt [file]`
5. Lihat hasil: `john --show [file]`

---

## 6. Windows Authentication Hash (NTLM)

### 6.1 Konsep

**NTHash** (disebut juga NTLM) adalah format hash yang digunakan Windows modern untuk menyimpan password pengguna dan layanan. Nama NTLM merujuk pada versi sebelumnya (LM), sehingga sering ditulis NT/LM.

Hash ini disimpan di **SAM (Security Account Manager)** database. Cara mendapatkan hash NTLM:
- Dump SAM database menggunakan tool seperti **Mimikatz**
- Dari **NTDS.dit** (database Active Directory)

Alternatif serangan selain crack: **pass the hash** — menggunakan hash langsung tanpa perlu mengetahui password aslinya.

### 6.2 Format dan Perintah

Format John untuk hash NTLM adalah `NT` (bukan NTLM atau Raw-NT):

```bash
john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt [file]
```

---

## 7. Cracking /etc/shadow (Linux)

### 7.1 Konsep

`/etc/shadow` adalah file Linux yang menyimpan hash password semua user. Hanya dapat diakses oleh root. File ini tidak berdiri sendiri — John membutuhkan informasi dari `/etc/passwd` (informasi akun user) untuk memahami konteks data.

### 7.2 Unshadowing

**Unshadow** adalah tool bawaan John suite untuk menggabungkan `/etc/passwd` dan `/etc/shadow` menjadi satu file yang siap diproses John:

```bash
unshadow [path passwd] [path shadow] > [output file]
```

- `[path passwd]` — salinan file `/etc/passwd` dari mesin target
- `[path shadow]` — salinan file `/etc/shadow` dari mesin target
- `>` — redirect output ke file baru

Contoh:
```bash
unshadow local_passwd local_shadow > unshadowed.txt
```

Cara menyalin file dari sistem:
```bash
cp /etc/passwd ~/local_passwd
sudo cp /etc/shadow ~/local_shadow
```

### 7.3 Cracking

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt
```

Format bisa dikenali dari identifier `$6$` di dalam file unshadowed → berarti `sha512crypt`.

Ketika berhasil, output John menampilkan password beserta nama user pemiliknya (karena data dari `/etc/passwd` ikut disertakan).

---

## 8. Single Crack Mode

### 8.1 Konsep

**Single Crack Mode** adalah mode di mana John tidak menggunakan wordlist eksternal, melainkan membangun wordlist sendiri dari informasi username. Teknik ini disebut **word mangling** — John memutasi username menjadi variasi-variasi yang mungkin digunakan sebagai password.

Contoh untuk username `Markus`:
- `Markus1`, `Markus2`, `Markus3`
- `MArkus`, `MARkus`, `MARKus`
- `Markus!`, `Markus$`, `Markus*`

John menggunakan sekumpulan aturan yang disebut **mangling rules** untuk mendefinisikan cara mutasi ini.

### 8.2 GECOS

John juga memanfaatkan **GECOS field** — field kelima dalam record `/etc/passwd` yang berisi informasi umum user (nama lengkap, nomor kantor, telepon). Data ini dipakai sebagai bahan tambahan untuk menghasilkan wordlist yang lebih relevan.

### 8.3 Format File untuk Single Crack Mode

File hash **harus menyertakan username** di depan hash, dipisahkan tanda titik dua:

```
username:hash
```

Contoh:
```
mike:1efee03cdcb96d90ad48ccc7b8666033
```

### 8.4 Perintah

```bash
john --single --format=[format] [file hash]
```

- `--single` — aktifkan mode single crack
- `--format` — tetap wajib ditentukan

Contoh:
```bash
john --single --format=raw-sha256 hashes.txt
```

---

## 9. Custom Rules

### 9.1 Konsep

**Custom Rules** memungkinkan pengguna mendefinisikan pola mangling sendiri, sangat berguna ketika struktur password target sudah diketahui. Contoh umum: banyak organisasi mewajibkan password berisi huruf kapital, angka, dan simbol — sehingga pola seperti `Polopassword1!` sangat umum digunakan.

### 9.2 Lokasi File Konfigurasi

Rules didefinisikan di file `john.conf`:
- AttackBox: `/opt/john/john.conf`
- Kali / install dari package manager: `/etc/john/john.conf`

Jumbo John sudah menyertakan ratusan custom rules bawaan — lihat sekitar baris 678 untuk referensi.

### 9.3 Struktur Rule

```
[List.Rules:NamaRule]
cAz"[0-9][!£$%@]"
```

**Modifier utama:**

| Modifier | Fungsi |
|---|---|
| `c` | Kapitalisasi huruf pertama kata |
| `Az` | Append — tambahkan karakter di akhir kata |
| `A0` | Prepend — tambahkan karakter di awal kata |

**Character set** (ditulis di dalam `" "` dan `[ ]`):

| Notasi | Karakter yang dicakup |
|---|---|
| `[0-9]` | Angka 0 sampai 9 |
| `[0]` | Hanya angka 0 |
| `[A-Z]` | Semua huruf kapital |
| `[a-z]` | Semua huruf kecil |
| `[A-z]` | Huruf besar dan kecil |
| `[!£$%@]` | Simbol !, £, $, %, @ |

**Contoh lengkap** untuk menghasilkan pola `Polopassword1!`:

```
[List.Rules:PoloPassword]
cAz"[0-9][!£$%@]"
```

Dibaca: kapitalisasi huruf pertama (`c`), tambahkan di akhir (`Az`) satu angka 0-9 diikuti satu simbol dari set `[!£$%@]`.

### 9.4 Menggunakan Custom Rule

```bash
john --wordlist=[path wordlist] --rule=NamaRule [file hash]
```

Contoh:
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --rule=PoloPassword hash.txt
```

---

## 10. Tool Konversi (xyzToJohn)

Pola umum untuk format non-hash: **konversi dulu ke format hash yang John mengerti, baru di-crack**.

### 10.1 Zip2John — Crack File ZIP Berpassword

```bash
zip2john [options] [zip file] > [output file]
```

- `[options]` — opsi checksum spesifik (jarang diperlukan)
- `[zip file]` — path ke file ZIP
- `>` — redirect output
- `[output file]` — file penyimpan hasil konversi

Contoh:
```bash
zip2john secure.zip > zip_hash.txt
john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt
```

Membuka ZIP setelah password ditemukan:
```bash
unzip [file.zip]
```

### 10.2 Rar2John — Crack File RAR Berpassword

RAR adalah format kompresi yang dibuat oleh WinRAR, mirip ZIP secara fungsi.

```bash
rar2john [rar file] > [output file]
```

Kadang perlu dipanggil dengan path lengkap:
```bash
/opt/john/rar2john rarfile.rar > rar_hash.txt
john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt
```

Membuka RAR setelah password ditemukan:
```bash
unrar x [file.rar]   # ekstrak dengan struktur folder asli
unrar e [file.rar]   # ekstrak semua ke folder saat ini tanpa struktur
```

### 10.3 SSH2John — Crack Passphrase SSH Private Key

**id_rsa** adalah file private key SSH yang digunakan untuk autentikasi berbasis kunci (pengganti password). File ini bisa dilindungi passphrase — inilah yang di-crack.

```bash
ssh2john [id_rsa file] > [output file]
```

Jika `ssh2john` tidak tersedia sebagai command langsung, gunakan versi Python:
```bash
python3 /opt/john/ssh2john.py id_rsa > id_rsa_hash.txt   # AttackBox
python /usr/share/john/ssh2john.py id_rsa > id_rsa_hash.txt  # Kali
```

Crack:
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
```

Login SSH menggunakan key setelah passphrase diketahui:
```bash
ssh -i id_rsa user@[IP_TARGET]
```

### 10.4 Ringkasan Semua Tool Konversi

| Tool | Input | Kegunaan |
|---|---|---|
| `unshadow` | passwd + shadow | Gabungkan untuk crack Linux hash |
| `zip2john` | file .zip | Ekstrak hash dari ZIP berpassword |
| `rar2john` | file .rar | Ekstrak hash dari RAR berpassword |
| `ssh2john` | file id_rsa | Ekstrak hash dari SSH private key |

Pola selalu sama: `[tool] [input] > [output.txt]` → `john --wordlist=rockyou.txt [output.txt]`

---

## 11. Perbandingan Mode John

| Mode | Wordlist Eksternal | Cara Kerja |
|---|---|---|
| Wordlist mode (`--wordlist`) | Ya, wajib disediakan | Hash setiap kata dari wordlist, bandingkan dengan target |
| Single crack mode (`--single`) | Tidak butuh | Bangun wordlist dari username via word mangling |
| Automatic mode | Ya, wajib disediakan | Sama dengan wordlist mode tapi format otomatis terdeteksi |
| Custom rules (`--rule`) | Ya, wajib disediakan | Hash kata dari wordlist yang dimodifikasi sesuai rule |

---

## 12. Glossary

**Hash** — representasi fixed-length dari data, dihasilkan fungsi hashing satu arah

**Dictionary Attack** — serangan dengan cara meng-hash semua kata dari wordlist dan membandingkan hasilnya dengan hash target

**Wordlist** — daftar kata/password yang digunakan sebagai bahan dictionary attack

**Word Mangling** — teknik mutasi kata (biasanya username) menjadi variasi-variasi yang mungkin digunakan sebagai password

**GECOS** — field kelima dalam record `/etc/passwd`, berisi informasi umum user (nama lengkap, nomor kantor, telepon)

**SAM Database** — Security Account Manager, tempat Windows menyimpan hash password user

**Pass the Hash** — teknik serangan yang menggunakan hash langsung untuk autentikasi, tanpa perlu mengetahui password aslinya

**Jumbo John** — versi extended John the Ripper yang menyertakan tools tambahan dan ratusan format hash lebih banyak dari versi core

**john.pot** — file cache John di `~/.john/john.pot`, menyimpan semua hash yang pernah berhasil di-crack

**P vs NP** — konsep matematika yang mendasari keamanan hash; P = masalah yang mudah diselesaikan, NP = masalah yang solusinya mudah diverifikasi tapi sulit ditemukan

**id_rsa** — file private key SSH yang digunakan untuk autentikasi berbasis kunci ke server remote

**Mangling Rules** — aturan yang mendefinisikan cara John memutasi kata dalam single crack mode maupun custom rules

**Fixed-length Output** — sifat hash di mana panjang output selalu sama terlepas dari panjang input

---

## 13. Tools & Platform Rujukan

**John the Ripper (Jumbo)**
- Tool utama pemecah hash yang dibahas di seluruh room ini
- Download dan dokumentasi: https://www.openwall.com/john/

**Openwall Wiki**
- Dokumentasi resmi John the Ripper — tips penggunaan, update, dan referensi modifier
- URL: https://openwall.info/wiki/john

**SecLists**
- Repositori koleksi wordlist, termasuk rockyou.txt dan berbagai password list lainnya
- URL: https://github.com/danielmiessler/SecLists

**hash-identifier**
- Tool Python untuk mengidentifikasi tipe hash
- URL: https://gitlab.com/kalilinux/packages/hash-identifier

**Hashcat Example Hashes**
- Referensi tabel identifier format hash (misalnya `$6$` = sha512crypt)
- URL: https://hashcat.net/wiki/doku.php?id=example_hashes

---

## 14. Catatan Ringkas untuk Ditulis Tangan

### Hash & Konsep Dasar

- Hash — fungsi satu arah, fixed-length output
- Sifat — mudah buat, mustahil balik (P vs NP)
- MD5 output — selalu 32 karakter hex
- Dictionary attack — hash wordlist → bandingkan → cocok = crack

### Identifier Hash Linux (/etc/shadow)

- $1$ — md5crypt
- $5$ — sha256crypt
- $6$ — sha512crypt (paling umum)
- $y$ — yescrypt (lambat, Kali modern)

### Format John (hash polos)

- MD5 → Raw-MD5
- SHA1 → Raw-SHA1
- SHA256 → Raw-SHA256
- SHA512 → Raw-SHA512
- NTLM → NT (bukan Raw-NT!)
- Whirlpool → Whirlpool (tanpa Raw-)
- sha512crypt → sha512crypt
- yescrypt → crypt

### Command Utama John

- john --wordlist= --format= [file] — crack dengan format spesifik
- john --wordlist= [file] — crack otomatis (tanpa format)
- john --show [file] — lihat hasil yang sudah di-crack
- john --list=formats | grep -iF "x" — cari format yang tersedia
- john --single --format= [file] — single crack mode
- john --wordlist= --rule=NamaRule [file] — pakai custom rule

### Alur Crack Standar

1. cat [file] → lihat isi hash
2. hashid [hash] → identifikasi tipe
3. john --list=formats | grep -iF "[tipe]" → cari format
4. john --format= --wordlist=rockyou.txt [file] → crack
5. john --show [file] → lihat hasil

### Windows NTLM

- Disimpan di SAM database
- Dapat via Mimikatz atau NTDS.dit
- Format John: NT
- Alternatif serangan: pass the hash

### Linux /etc/shadow

- Hanya root yang bisa akses
- Butuh unshadow dulu sebelum crack
- unshadow [passwd] [shadow] > output.txt → gabungkan dua file
- Baca format dari identifier $x$ di output
- john --format=sha512crypt --wordlist= unshadowed.txt

### Single Crack Mode

- Tidak butuh wordlist eksternal
- John bangun wordlist dari username (word mangling)
- File hash HARUS format: username:hash
- --single → aktifkan mode ini
- GECOS — field ke-5 /etc/passwd, juga dipakai sebagai bahan tebakan

### Custom Rules (john.conf)

- Lokasi: /etc/john/john.conf (Kali) atau /opt/john/john.conf (AttackBox)
- [List.Rules:NamaRule] → deklarasi nama rule
- c — kapitalisasi huruf pertama
- Az — append di akhir kata
- A0 — prepend di depan kata
- Karakter set dalam " " dan [ ] → [0-9] [A-Z] [a-z] [!£$%@]
- --rule=NamaRule → panggil rule saat crack

### Tool Konversi (xyzToJohn)

- zip2john [file.zip] > hash.txt → ZIP
- rar2john [file.rar] > hash.txt → RAR
- ssh2john [id_rsa] > hash.txt → SSH key
- unshadow [passwd] [shadow] > hash.txt → Linux shadow
- Setelah konversi: john --wordlist=rockyou.txt hash.txt

### Buka Hasil (setelah password ditemukan)

- unzip [file.zip] → masukkan password
- unrar x [file.rar] → dengan struktur folder
- unrar e [file.rar] → tanpa struktur folder
- ssh -i id_rsa user@[IP] → login SSH dengan key

### Wordlist Penting

- RockYou — /usr/share/wordlists/rockyou.txt, 14 juta password nyata dari breach 2009
- SecLists — koleksi wordlist lebih lengkap (GitHub danielmiessler/SecLists)
