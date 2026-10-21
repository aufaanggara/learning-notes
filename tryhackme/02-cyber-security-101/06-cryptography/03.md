# Hashing Basics — Resume Materi
**TryHackMe Room: Hashing Basics**
Tanggal: 21 Oktober 2026

---

## 1. Hash Function & Hash Value

**Hash value** (juga disebut **digest**) adalah string berukuran tetap yang dihasilkan dari proses hashing. **Hash function** mengambil input dengan ukuran sembarang dan menghasilkan output dengan panjang tetap.

Sifat-sifat kunci hash function:

- Input sama selalu menghasilkan output yang sama
- Satu bit berubah pada input → output berubah total (drastis)
- Satu arah — tidak bisa dibalik untuk mendapatkan input asli
- Cepat untuk dihitung, sangat lambat (atau tidak mungkin) untuk dibalik
- Dirancang untuk menghindari collision dan mencegah rekayasa collision yang disengaja

Dua kegunaan utama hashing di keamanan siber:

1. Penyimpanan kata sandi untuk autentikasi
2. Integrity checking untuk verifikasi file

---

## 2. Algoritma Hash & Ukuran Output

| Algoritma | Bit | Byte | Karakter Hex | Status |
|-----------|-----|------|--------------|--------|
| MD5 | 128 | 16 | 32 | Tidak aman |
| SHA-1 | 160 | 20 | 40 | Tidak aman |
| SHA-256 | 256 | 32 | 64 | Aman |
| SHA-512 | 512 | 64 | 128 | Aman |
| yescrypt | 256 | 32 | — | Default & direkomendasikan |

Cara menghitung ukuran byte dari output hex: **jumlah karakter hex ÷ 2**

Contoh: MD5 menghasilkan 32 karakter hex → 32 ÷ 2 = **16 byte**

Rumus total kemungkinan nilai hash: **2^(jumlah_bit)**

Contoh: hash 8-bit → 2^8 = 256 kemungkinan nilai

Output mentah hash berupa raw bytes, kemudian dikodekan ke **hexadecimal** atau **base64**.

---

## 3. Hash Collision & Pigeonhole Effect

**Hash collision** terjadi ketika dua input berbeda menghasilkan hash yang sama.

**Pigeonhole effect** menjelaskan mengapa collision tidak bisa dihindari secara matematis: jumlah input yang mungkin tidak terbatas, sedangkan jumlah output terbatas. Seperti 21 merpati yang harus masuk ke 16 lubang — pasti ada lubang yang diisi lebih dari satu.

Hash function yang baik memastikan probabilitas collision sangat kecil (negligible), meskipun tidak bisa nol secara absolut.

MD5 dan SHA-1 sudah berhasil di-engineer collision-nya dan kini dianggap tidak aman. Namun, belum ada serangan yang menghasilkan collision pada keduanya secara bersamaan — sehingga membandingkan MD5 dan SHA-1 dari file yang sama masih bisa mendeteksi manipulasi.

---

## 4. Perintah Hashing di Linux

```
md5sum [file]
```
Menghitung hash MD5 dari file.

```
sha1sum [file]
```
Menghitung hash SHA-1 dari file.

```
sha256sum [file]
```
Menghitung hash SHA-256 dari file.

```
sha512sum [file]
```
Menghitung hash SHA-512 dari file.

```
md5sum *.txt
```
Menghitung hash MD5 dari semua file bertipe `.txt` sekaligus. Tanda `*` adalah wildcard yang berarti "semua file dengan ekstensi tersebut."

```
wc -l [file]
```
Menghitung jumlah baris dalam file. Flag `-l` berarti lines. Berguna untuk mengetahui jumlah entri dalam wordlist.

```
hexdump -C [file]
```
Menampilkan isi file dalam format hexadecimal dan ASCII. Berguna untuk melihat representasi biner dari karakter.

---

## 5. Penyimpanan Kata Sandi yang Aman

### 5.1 Tiga Praktik Tidak Aman

**Plaintext** — Kata sandi disimpan apa adanya tanpa proteksi. Kasus nyata: RockYou menyimpan 14 juta+ kata sandi pengguna dalam plaintext. Saat terjadi breach, semua kata sandi langsung terbaca.

**Enkripsi usang** — Adobe menggunakan format enkripsi yang sudah deprecated. Password hints juga disimpan dalam plaintext, sehingga plaintext password bisa dikembalikan dengan relatif mudah.

**Hash tanpa salt menggunakan algoritma tidak aman** — LinkedIn menggunakan SHA-1 tanpa salt. Tanpa salt, dua user dengan password sama memiliki hash yang sama, sehingga rentan terhadap rainbow table attack.

### 5.2 Cara Penyimpanan yang Benar

Algoritma yang direkomendasikan untuk hashing kata sandi: **Argon2**, **Scrypt**, **Bcrypt**, **PBKDF2**

Alur penyimpanan yang benar:

1. Pilih algoritma aman (misal Bcrypt)
2. Generate salt unik secara acak (misal `Y4UV*^(=go_!`)
3. Gabungkan password dengan salt → `AL4RMc10kY4UV*^(=go_!`
4. Hash gabungan tersebut
5. Simpan nilai hash dan salt yang digunakan

Salt tidak perlu dirahasiakan. Fungsinya bukan untuk kerahasiaan, tapi untuk memastikan setiap user memiliki hash yang unik meskipun passwordnya sama — sehingga rainbow table menjadi tidak efektif.

### 5.3 Mengapa Enkripsi Tidak Cocok untuk Password

Enkripsi membutuhkan kunci untuk menyimpan dan membuka data. Jika kunci tersebut bocor, semua password bisa didekripsi sekaligus. Sistem autentikasi tidak butuh tahu password asli — cukup tahu apakah hash yang dikirim cocok dengan hash yang tersimpan.

---

## 6. Rainbow Table vs Dictionary Attack

### 6.1 Rainbow Table

Tabel lookup yang sudah berisi pasangan hash dan plaintext, dihitung dan disimpan sebelumnya. Saat digunakan, tinggal cocokkan hash target dengan isi tabel.

Kelebihan: sangat cepat

Kelemahan: tidak efektif jika password menggunakan salt, membutuhkan storage yang sangat besar

Website seperti CrackStation dan Hashes.com menggunakan massive pre-computed lookup tables untuk melakukan cracking hash tanpa salt secara instan.

### 6.2 Dictionary Attack (Hashcat)

Hash dihitung on the spot saat serangan berlangsung. Setiap kata dari wordlist diambil satu per satu, di-hash menggunakan algoritma yang ditentukan, lalu hasilnya dibandingkan dengan hash target.

Kelebihan: bisa menangani salt (Hashcat membaca salt langsung dari format hash), tidak butuh storage besar

Kelemahan: lebih lambat dibanding rainbow table

Jika password ada di wordlist → ditemukan. Jika tidak ada → status Exhausted.

---

## 7. Format Hash Linux (/etc/shadow)

### 7.1 Lokasi Penyimpanan

Hash kata sandi Linux disimpan di `/etc/shadow`, yang hanya bisa dibaca oleh root.

Sebelumnya disimpan di `/etc/passwd` yang bisa dibaca semua pengguna — praktik ini tidak aman.

Untuk melihat informasi lebih lengkap tentang field shadow: `man 5 shadow`
Untuk informasi tentang format enkripsi/hash: `man 5 crypt`

### 7.2 Format Field Kedua

Field kata sandi menggunakan format: `$prefix$options$salt$hash`

Prefix menentukan algoritma hashing yang digunakan, sehingga sistem tahu cara memverifikasi password.

### 7.3 Tabel Prefix (Urutan Kekuatan Menurun)

| Prefix | Algoritma |
|--------|-----------|
| `$y$` | yescrypt — default & direkomendasikan untuk sistem baru |
| `$gy$` | gost-yescrypt — menggunakan GOST R 34.11-2012 + yescrypt |
| `$7$` | scrypt — password-based key derivation function |
| `$2b$`, `$2y$`, `$2a$`, `$2x$` | bcrypt — berbasis Blowfish cipher |
| `$6$` | sha512crypt — SHA-2 512-bit, umum di sistem Linux lama |
| `$md5` | SunMD5 — dikembangkan untuk Solaris |
| `$1$` | md5crypt — dikembangkan untuk FreeBSD |

### 7.4 Sembilan Field dalam /etc/shadow

Setiap baris dipisahkan oleh titik dua (`:`):

1. Username
2. Hash kata sandi (`$prefix$options$salt$hash`)
3. Tanggal terakhir password diubah (jumlah hari sejak 1 Jan 1970)
4. Minimum hari sebelum password boleh diganti lagi (0 = kapan saja)
5. Maksimum hari sebelum password harus diganti (99999 ≈ tidak pernah expired)
6. Jumlah hari peringatan sebelum password expired
7. Jumlah hari setelah expired sebelum akun dikunci (kosong = tidak dikunci otomatis)
8. Tanggal akun kadaluarsa total (kosong = tidak ada tanggal expired)
9. Reserved untuk penggunaan masa depan (selalu kosong)

---

## 8. Windows Password

Kata sandi Windows di-hash menggunakan **NTLM**, sebuah varian dari MD4.

Hash NTLM secara visual identik dengan hash MD4 dan MD5 — 32 karakter hex tanpa prefix. Karena itu, sangat penting menggunakan konteks untuk menentukan jenis hash.

Hash disimpan di **SAM (Security Accounts Manager)**. Windows mencegah pengguna biasa mengakses SAM secara langsung, tapi tools seperti **mimikatz** bisa digunakan untuk mem-bypass proteksi ini.

Hash di SAM terbagi menjadi dua jenis: **NT hashes** dan **LM hashes**.

---

## 9. Mengenali Jenis Hash

### 9.1 Langkah Identifikasi

1. **Cek prefix** — Jika ada `$y$`, `$6$`, `$2a$`, dst., algoritma langsung diketahui
2. **Hitung panjang karakter hex** — 32 = MD5/NTLM, 40 = SHA-1, 64 = SHA-256, 128 = SHA-512
3. **Gunakan tools otomatis** — `hashid` atau `hash-identifier`, tapi tidak selalu akurat
4. **Perhatikan konteks** — Hash dari web app database → kemungkinan MD5; dari `/etc/shadow` → lihat prefix; dari Windows → kemungkinan NTLM
5. **Konfirmasi di Hashcat Example Hashes** — halaman referensi resmi untuk semua format hash

MD5 dan NTLM tidak bisa dibedakan hanya dari bentuk visualnya — keduanya 32 karakter hex tanpa prefix. Satu-satunya pembeda adalah konteks: NTLM eksklusif dari sistem Windows.

---

## 10. Password Cracking dengan Hashcat

### 10.1 Sintaks Dasar

```
hashcat -m <hash_type> -a <attack_mode> hashfile wordlist
```

### 10.2 Switch Utama

`-m <angka>` — Menentukan tipe hash dalam format numerik. Referensi: Hashcat Example Hashes page.

`-a <angka>` — Menentukan mode serangan (lihat 10.3).

`-O` — Optimized kernel: lebih cepat, tapi membatasi panjang password maksimal (biasanya turun ke 32 karakter).

`-w 3` — Workload tinggi: memaksimalkan performa, tapi layar bisa lag.

`-S` — Slow candidates: dampak besar pada kecepatan, berguna untuk wordlist kecil dengan ruleset besar.

### 10.3 Attack Mode (-a)

`-a 0` — **Straight/Dictionary**: mencoba setiap kata dari wordlist satu per satu. Mode paling umum digunakan.

`-a 1` — **Combination**: menggabungkan kata dari dua wordlist berbeda.

`-a 3` — **Brute Force/Mask**: mencoba semua kombinasi karakter sesuai pola (mask) yang ditentukan.

`-a 6` — **Hybrid Wordlist + Mask**: kata dari wordlist ditambah karakter acak di belakangnya.

`-a 7` — **Hybrid Mask + Wordlist**: karakter acak di depan, kata dari wordlist di belakang.

### 10.4 Hash Mode (-m) yang Sering Dipakai

| Mode | Algoritma |
|------|-----------|
| 0 | MD5 |
| 1000 | NTLM |
| 1400 | SHA2-256 |
| 1710 | HMAC-SHA512 (key = $pass) |
| 1800 | sha512crypt $6$ (SHA-512 Unix) |
| 2410 | Cisco-ASA MD5 |
| 3200 | bcrypt $2*$ |
| 9300 | Cisco-IOS $9$ (scrypt) |

### 10.5 Contoh Perintah

```
hashcat -m 3200 -a 0 hash1.txt /usr/share/wordlists/rockyou.txt
```
Crack hash bcrypt di `hash1.txt` menggunakan wordlist rockyou.txt.

```
hashcat -m 1400 -a 0 hash2.txt /usr/share/wordlists/rockyou.txt
```
Crack hash SHA2-256.

```
hashcat -m 0 -a 0 hash4.txt /usr/share/wordlists/rockyou.txt
```
Crack hash MD5.

### 10.6 Kontrol Saat Proses Berjalan

Saat Hashcat sedang running, tekan:

`s` — tampilkan status dan progress saat ini

`p` — pause sementara

`r` — resume setelah pause

`b` — bypass (skip ke hash berikutnya)

`q` — quit (berhenti total)

### 10.7 Catatan Performa GPU vs CPU

Hashcat memanfaatkan **GPU** untuk performa maksimal. Karena VM umumnya tidak memiliki akses langsung ke GPU host, jalankan Hashcat di **OS host** untuk hasil terbaik.

Alternatif di VM: Hashcat bisa bekerja menggunakan **OpenCL** (berjalan di atas CPU), tapi kecepatannya jauh lebih rendah dibanding GPU langsung.

**John the Ripper** menggunakan CPU secara default dan bisa langsung berjalan di VM tanpa konfigurasi tambahan — meski tetap lebih cepat jika dijalankan di OS host.

Algoritma seperti **Bcrypt** dirancang khusus agar hashing di GPU tidak memberikan keuntungan kecepatan dibanding CPU — ini membuatnya sangat tahan terhadap brute force.

### 10.8 Perbedaan Kecepatan Algoritma

Bcrypt hanya mampu ~356 H/s di VM (sengaja lambat), sedangkan SHA-256 bisa mencapai ~1.200.000 H/s. Ini adalah bukti langsung mengapa algoritma yang dirancang khusus untuk password (Bcrypt, Argon2, dll) jauh lebih aman dibanding algoritma hash generik (SHA-256, MD5).

---

## 11. Integrity Checking

Hashing dapat digunakan untuk memverifikasi bahwa sebuah file tidak dimodifikasi. Prinsipnya: input sama selalu menghasilkan output yang sama, dan satu bit perubahan mengubah hash secara drastis.

Cara kerja:

1. Developer merilis file beserta nilai hash resminya (biasanya SHA-256)
2. Pengguna mendownload file, lalu menjalankan `sha256sum` pada file tersebut
3. Jika hash hasil perhitungan cocok dengan hash resmi → file identik dan tidak dimodifikasi
4. Jika berbeda → file corrupt atau telah dimanipulasi

Contoh nyata: file `Fedora-Workstation-40-1.14-x86_64-CHECKSUM` berisi hash SHA-256 resmi dari file ISO Fedora yang bisa diverifikasi setelah download.

Hashing juga bisa digunakan untuk mendeteksi **file duplikat** — dua file dengan hash yang sama memiliki isi yang identik.

---

## 12. HMAC

**HMAC (Keyed-Hash Message Authentication Code)** adalah jenis message authentication code yang menggabungkan fungsi hash kriptografis dengan sebuah kunci rahasia.

HMAC membuktikan dua hal sekaligus:

- **Keaslian (authenticity)** — pesan benar-benar berasal dari pihak yang memiliki kunci rahasia
- **Integritas (integrity)** — pesan tidak dimodifikasi atau dirusak dalam perjalanan

### 12.1 Cara Kerja HMAC

1. Secret key dipadding ke ukuran blok hash function
2. Padded key di-XOR dengan konstanta **ipad** (inner pad)
3. Pesan di-hash menggunakan hasil (key XOR ipad)
4. Hasil step 3 di-hash kembali menggunakan (key XOR **opad** — outer pad)
5. Output akhir adalah nilai HMAC (string berukuran tetap)

Formula teknis: `HMAC(K,M) = H((K⊕opad) || H((K⊕ipad) || M))`

Di mana K = kunci, M = pesan, H = hash function.

---

## 13. Hashing vs Encoding vs Encryption

| Aspek | Hashing | Encoding | Encryption |
|-------|---------|----------|------------|
| Reversible? | Tidak | Ya | Ya (dengan kunci) |
| Butuh kunci? | Tidak | Tidak | Ya |
| Tujuan utama | Integritas & verifikasi | Kompatibilitas format | Kerahasiaan data |
| Contoh | SHA-256, bcrypt | Base64, UTF-8 | AES, RSA |

**Hashing** — satu arah, tidak bisa dibalik, menghasilkan digest unik.

**Encoding** — mengubah format data agar kompatibel dengan sistem tertentu. Bukan untuk keamanan — siapapun dengan tools yang tepat bisa decode. Metode umum: ASCII, UTF-8, UTF-16, UTF-32, ISO-8859-1, Windows-1252, Base32, Base64.

**Encryption** — melindungi kerahasiaan data menggunakan cipher dan kunci. Bisa dibalik asalkan kunci diketahui.

### 13.1 Perintah Base64 di Linux

```
base64
```
Mode encode: ketik teks, tekan Enter, lalu Ctrl+D untuk mendapatkan hasil encoding Base64.

```
base64 -d
```
Mode decode: masukkan string Base64, tekan Ctrl+D untuk mendapatkan plaintext.

```
base64 -d [path/ke/file]
```
Decode langsung dari file. Lebih praktis dibanding copy-paste manual.

Flag `-d` — singkatan dari decode.

---

## 14. Glosarium

**Hash value / digest** — Output tetap berukuran fixed dari fungsi hash yang merepresentasikan data input secara unik.

**Hash function** — Fungsi yang mengambil input sembarang ukuran dan menghasilkan output ukuran tetap.

**Salt** — Nilai acak unik yang ditambahkan ke password sebelum di-hash, untuk mencegah dua password sama menghasilkan hash yang sama.

**Hash collision** — Dua input berbeda yang menghasilkan nilai hash identik.

**Pigeonhole effect** — Fenomena matematis di mana input tak terbatas dipetakan ke output terbatas, sehingga collision tidak bisa sepenuhnya dihindari.

**Plaintext** — Data atau password asli sebelum diproses (di-hash, dienkripsi, atau diencode).

**Rainbow table** — Database pre-computed berisi pasangan hash dan plaintext untuk mempercepat proses cracking.

**Dictionary attack** — Serangan dengan mencoba setiap kata dari wordlist satu per satu, di-hash on the spot, lalu dibandingkan dengan hash target.

**Brute force** — Mencoba semua kemungkinan kombinasi karakter secara sistematis.

**Wordlist** — File berisi daftar kata sandi yang digunakan dalam dictionary attack.

**Integrity** — Jaminan bahwa data tidak dimodifikasi atau dirusak.

**HMAC** — Keyed-Hash Message Authentication Code; kombinasi hash function dan kunci rahasia untuk verifikasi keaslian dan integritas.

**Stdin mode** — Kondisi di mana Hashcat menunggu input dari keyboard (bukan dari file). Terjadi jika wordlist tidak disertakan dalam perintah.

**Exhausted** — Status Hashcat yang berarti seluruh wordlist sudah dicoba tapi tidak ada yang cocok.

**Cracked** — Status Hashcat yang berarti hash berhasil dipecahkan dan plaintext ditemukan.

**Encoding** — Proses mengubah format data agar kompatibel dengan sistem tertentu; bersifat reversible dan bukan mekanisme keamanan.

**SAM (Security Accounts Manager)** — Tempat penyimpanan hash kata sandi pada sistem Windows.

**NTLM** — NT LAN Manager; algoritma hashing yang digunakan Windows untuk menyimpan password, merupakan varian MD4.

**Hash-Mode** — Angka yang digunakan Hashcat untuk mengidentifikasi tipe hash tertentu dalam perintah `-m`.

---

## 15. Tools & Platform Rujukan

**CrackStation** — Platform cracking hash online menggunakan massive rainbow table/lookup table. Efektif untuk hash tanpa salt.
URL: https://crackstation.net

**Hashes.com** — Platform cracking hash online dengan database yang berbeda dari CrackStation. Berguna sebagai alternatif jika CrackStation tidak menemukan hasil.
URL: https://hashes.com

**Hashcat** — Tool cracking hash berbasis GPU untuk serangan offline. Mendukung ratusan format hash.
URL: https://hashcat.net

**Hashcat Example Hashes** — Halaman referensi resmi Hashcat berisi daftar semua hash mode beserta contoh hash-nya. Digunakan untuk mengidentifikasi tipe hash dan menemukan mode number yang tepat.
URL: https://hashcat.net/wiki/doku.php?id=example_hashes

**John the Ripper** — Tool cracking hash berbasis CPU. Lebih mudah digunakan, mendukung auto-detect jenis hash, dan bisa langsung berjalan di VM.
URL: https://www.openwall.com/john/

**hashID** — Tools otomatis untuk mengenali jenis hash. Tersedia sebagai CLI di Linux.
Cara pakai: `hashid [hash_value]`

**hash-identifier** — Alternatif hashID untuk identifikasi jenis hash secara otomatis di Linux.

**MD5 Collision Demo** — Halaman demonstrasi collision MD5.
URL: https://www.mscs.dal.ca/~selinger/md5collision/

**Shattered** — Halaman detail tentang serangan collision pada SHA-1.
URL: https://shattered.io

**rockyou.txt** — Wordlist populer berisi 14 juta+ password dari breach RockYou. Tersedia di Kali Linux di `/usr/share/wordlists/rockyou.txt`.

---

## 16. Catatan Ringkas untuk Ditulis Tangan

### Hash Dasar

- Hash value / digest — output ukuran tetap dari fungsi hash
- Hash function — input sembarang → output tetap
- Sifat — satu arah, tidak reversible, 1 bit beda → output beda total
- Kegunaan — password storage + integrity checking

### Ukuran Output

- MD5 — 128 bit = 16 byte = 32 hex (TIDAK AMAN)
- SHA-1 — 160 bit = 20 byte = 40 hex (TIDAK AMAN)
- SHA-256 — 256 bit = 32 byte = 64 hex (AMAN)
- SHA-512 — 512 bit = 64 byte = 128 hex (AMAN)
- Rumus byte dari hex — jumlah karakter hex ÷ 2
- Rumus kemungkinan — 2^(jumlah bit)

### Collision & Pigeonhole

- Collision — 2 input beda → hash sama
- Pigeonhole — input tak terbatas, output terbatas → collision pasti ada
- MD5 & SHA-1 — sudah di-engineer collision-nya → tidak dipercaya lagi

### Perintah Linux

- md5sum [file] — hitung hash MD5
- sha1sum [file] — hitung hash SHA-1
- sha256sum [file] — hitung hash SHA-256
- sha512sum [file] — hitung hash SHA-512
- md5sum *.txt — hash semua file .txt (* = wildcard)
- wc -l [file] — hitung jumlah baris
- hexdump -C [file] — tampilkan isi file dalam hex

### Password Storage

- Buruk: plaintext, enkripsi usang, hash tanpa salt
- Bagus: algoritma khusus password + salt unik per user
- Algoritma aman — Argon2, Scrypt, Bcrypt, PBKDF2
- Salt — nilai acak unik, tidak perlu dirahasiakan
- Alur — pilih algo → buat salt → gabung pass+salt → hash → simpan hash+salt

### Format /etc/shadow

- Lokasi — /etc/shadow (hanya root)
- Format field 2 — $prefix$options$salt$hash
- Prefix $y$ — yescrypt (default baru)
- Prefix $6$ — sha512crypt
- Prefix $2a$/$2b$ — bcrypt
- Prefix $1$ — md5crypt
- 9 field dipisah titik dua: username:hash:lastchange:min:max:warn:inactive:expire:reserved
- man 5 shadow — detail semua field
- man 5 crypt — detail format hash

### Windows Password

- Algoritma — NTLM (varian MD4)
- Lokasi — SAM (Security Accounts Manager)
- Jenis — NT hash & LM hash
- Visual — identik dengan MD4/MD5 → bedakan dari konteks!
- Tools dump — mimikatz

### Mengenali Hash

- Ada prefix ($y$, $6$, $2a$...) → langsung ketahuan
- 32 char → MD5 atau NTLM (bedakan dari konteks: Linux=MD5, Windows=NTLM)
- 40 char → SHA-1
- 64 char → SHA-256
- 128 char → SHA-512
- Tools — hashid [hash] / hash-identifier
- Referensi — hashcat.net/wiki/doku.php?id=example_hashes

### Hashcat Sintaks & Switch

- Sintaks — hashcat -m <type> -a <mode> hashfile wordlist
- -m — tipe hash (angka dari Hashcat Example Hashes)
- -a — mode serangan
- -O — optimized kernel (lebih cepat, maks 32 char)
- -w 3 — workload tinggi
- -S — slow candidates

### Hashcat Attack Mode (-a)

- 0 — Straight/Dictionary (paling umum)
- 1 — Combination (2 wordlist digabung)
- 3 — Brute Force/Mask
- 6 — Hybrid Wordlist + Mask
- 7 — Hybrid Mask + Wordlist

### Hashcat Hash Mode (-m)

- 0 — MD5
- 1000 — NTLM
- 1400 — SHA2-256
- 1800 — sha512crypt $6$
- 3200 — bcrypt $2*$
- 1710 — HMAC-SHA512 (key=$pass)

### Hashcat Status Saat Running

- s — tampilkan status
- p — pause
- r — resume
- b — bypass
- q — quit
- Exhausted — semua wordlist dicoba, tidak ketemu
- Cracked — berhasil!

### Performa GPU vs CPU

- Hashcat — GPU, jalankan di OS host untuk maksimal
- John the Ripper — CPU, bisa langsung di VM
- Bcrypt — sengaja lambat di GPU = tahan brute force
- VM — tidak akses GPU langsung, bisa pakai OpenCL tapi lambat
- OS host = sistem operasi yang langsung jalan di hardware fisik

### Rainbow Table vs Dictionary Attack

- Rainbow table — pre-computed, tinggal cocokkan, tidak mempan jika ada salt
- Dictionary attack (Hashcat) — hitung hash on the spot, bisa handle salt
- rockyou.txt — 14 juta+ password, di /usr/share/wordlists/
- CrackStation & Hashes.com — pakai rainbow table, untuk hash tanpa salt

### Integrity Checking

- Hashing untuk verifikasi file tidak dimodifikasi
- sha256sum [file] → bandingkan dengan hash resmi developer
- Hash sama = file asli, hash beda = file diubah/corrupt
- Hash juga bisa deteksi file duplikat (hash sama = isi identik)

### HMAC

- Kepanjangan — Keyed-Hash Message Authentication Code
- Fungsi — verifikasi keaslian (authenticity) + integritas
- Komponen — hash function + secret key
- Alur — key XOR ipad → hash dengan pesan → key XOR opad → hash lagi → HMAC
- Formula — HMAC(K,M) = H((K⊕opad)||H((K⊕ipad)||M))

### Hashing vs Encoding vs Encryption

- Hashing — tidak reversible, no key, tujuan: integritas
- Encoding — reversible, no key, tujuan: kompatibilitas format
- Encryption — reversible, butuh key, tujuan: kerahasiaan
- Base64 encode — ketik base64 → teks → Ctrl+D
- Base64 decode — base64 -d [file] atau base64 -d lalu ketik manual
