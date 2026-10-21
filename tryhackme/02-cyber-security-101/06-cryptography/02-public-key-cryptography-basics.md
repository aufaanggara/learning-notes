# Public Key Cryptography Basics — Resume Materi
**TryHackMe Room** | **21 Oktober 2026**

---

## 1. Empat Pilar Keamanan Komunikasi

Keempat konsep ini adalah fondasi dari seluruh room. Setiap teknologi yang dibahas (RSA, DH, SSH, GPG) pada dasarnya bekerja untuk memenuhi satu atau lebih pilar ini.

**Authentication** — memastikan identitas pihak yang diajak bicara. Kamu tahu *siapa* lawan bicaramu.

**Authenticity** — memverifikasi bahwa pesan benar-benar berasal dari pengirim yang diklaim, bukan pihak lain.

**Integrity** — memastikan data tidak diubah atau dirusak selama perjalanan dari pengirim ke penerima.

**Confidentiality** — mencegah pihak tidak berwenang membaca isi komunikasi.

Kriptografi simetris (private key) terutama melindungi **confidentiality**. Kriptografi asimetris (public key) berperan besar dalam **authentication, authenticity, dan integrity**.

---

## 2. Kriptografi Asimetris — Konsep Inti

### 2.1 Pasangan Kunci

Setiap entitas dalam kriptografi asimetris memiliki dua kunci yang berpasangan secara matematis:

- **Public key** — boleh dibagikan ke siapapun. Digunakan untuk enkripsi pesan yang ditujukan ke pemilik kunci, dan untuk verifikasi tanda tangan digital.
- **Private key** — harus dijaga rahasia oleh pemiliknya. Digunakan untuk dekripsi pesan yang diterima, dan untuk membuat tanda tangan digital.

### 2.2 Dua Pola Penggunaan

**Untuk enkripsi (menjaga kerahasiaan):**
Pengirim mengenkripsi dengan **public key penerima** → hanya penerima yang bisa dekripsi dengan **private key**-nya sendiri.

**Untuk tanda tangan digital (membuktikan keaslian):**
Pengirim menandatangani dengan **private key**-nya sendiri → siapapun bisa verifikasi dengan **public key** pengirim yang tersedia publik.

### 2.3 Kombinasi Asimetris + Simetris

Kriptografi asimetris relatif lambat dibanding simetris, sehingga dalam praktiknya keduanya dikombinasikan:

1. Asimetris dipakai **sekali** di awal untuk menegosiasikan dan menyepakati kunci simetris.
2. Selanjutnya, komunikasi berlangsung menggunakan **enkripsi simetris** yang jauh lebih cepat.

Pola ini dipakai di TLS/HTTPS, SSH, dan hampir semua protokol komunikasi aman modern.

---

## 3. RSA

### 3.1 Dasar Matematika

RSA didasarkan pada sulitnya **memfaktorkan bilangan besar**. Mengalikan dua bilangan prima besar itu mudah, tetapi menemukan kembali dua faktor primanya dari hasil perkalian sangat sulit secara komputasi.

Contoh prinsip:
- 113 × 127 = 14351 → mudah dihitung
- Diberikan 14351, temukan dua faktor primanya → jauh lebih sulit

Dalam aplikasi nyata, p dan q masing-masing berupa bilangan prima **minimal 300 digit**. Komputer tidak mampu memfaktorkan bilangan dengan lebih dari 600 digit dalam waktu yang wajar.

### 3.2 Variabel RSA

| Variabel | Keterangan |
|---|---|
| p, q | Dua bilangan prima besar (rahasia) |
| n | Hasil perkalian p × q (publik) |
| e | Komponen kunci publik |
| d | Komponen kunci privat |
| m | Plaintext (pesan asli) |
| c | Ciphertext (pesan terenkripsi) |

- **Kunci publik** = (n, e)
- **Kunci privat** = (n, d)

### 3.3 Proses Enkripsi dan Dekripsi

Enkripsi: `c = m^e mod n`

Dekripsi: `m = c^d mod n`

Keamanan RSA bergantung pada fakta bahwa menghitung `e` dari `d` (atau sebaliknya) membutuhkan faktorisasi `n`, yang secara komputasi tidak layak untuk n yang cukup besar.

### 3.4 RSA dalam CTF

Soal CTF terkait RSA biasanya memberikan sebagian variabel dan meminta kamu memecahkan enkripsinya. Tools yang umum dipakai:

```
RsaCtfTool   # tool otomatis untuk menyerang berbagai kelemahan RSA
rsatool      # tool untuk kalkulasi dan manipulasi variabel RSA
```

---

## 4. Diffie-Hellman Key Exchange

### 4.1 Tujuan

DH memungkinkan dua pihak **menyepakati sebuah kunci bersama** melalui saluran komunikasi yang tidak aman, tanpa pernah mengirimkan kunci itu sendiri. Pengintip yang melihat seluruh pertukaran tidak bisa merekonstruksi kunci yang disepakati.

### 4.2 Variabel

| Variabel | Keterangan |
|---|---|
| p | Bilangan prima besar (publik) |
| g | Generator, 0 < g < p (publik) |
| a | Private key Alice (rahasia) |
| b | Private key Bob (rahasia) |
| A | Kunci publik Alice = g^a mod p |
| B | Kunci publik Bob = g^b mod p |

### 4.3 Langkah-Langkah DH

1. Alice dan Bob sepakat pada nilai **p** dan **g** (keduanya publik, boleh disadap).
2. Masing-masing memilih private key secara independen: Alice pilih **a**, Bob pilih **b** — keduanya rahasia.
3. Masing-masing hitung kunci publik: Alice hitung **A = g^a mod p**, Bob hitung **B = g^b mod p**.
4. Alice dan Bob saling tukar kunci publik (**key exchange** — boleh disadap).
5. Masing-masing hitung shared secret menggunakan kunci publik yang diterima dan private key sendiri:
   - Alice: `B^a mod p = (g^b)^a mod p = g^ab mod p`
   - Bob: `A^b mod p = (g^a)^b mod p = g^ab mod p`
   - Keduanya menghasilkan nilai yang sama → **shared secret = g^ab mod p**

### 4.4 Kenapa Hasilnya Sama?

Karena hukum matematika: `(x^b)^a = (x^a)^b = x^ab`. Urutan pemangkatan tidak mengubah hasil akhir.

### 4.5 Keamanan DH

Pengintip hanya bisa melihat p, g, A, dan B. Untuk mendapat kunci, mereka harus mencari nilai **a** dari persamaan `A = g^a mod p` — ini disebut **Discrete Logarithm Problem**, yang sangat sulit secara komputasi untuk nilai p yang besar.

### 4.6 DH vs RSA — Peran Masing-Masing

| | DH | RSA |
|---|---|---|
| Fungsi utama | Key agreement | Digital signatures, authentication |
| Kelemahan | Tidak verifikasi identitas | Lebih lambat dari DH |

Keduanya sering digabungkan dalam satu protokol (contoh: TLS) — DH untuk sepakat kunci, RSA untuk verifikasi identitas.

---

## 5. SSH

### 5.1 Dua Metode Autentikasi

**Password authentication** — pengguna memasukkan username dan password. Mudah dipakai, tapi tidak ideal untuk keamanan tinggi karena password bisa disadap, ditebak, atau di-brute force.

**Key authentication** — menggunakan pasangan public/private key. Tidak ada password yang dikirim melalui jaringan. Cara kerjanya: server menyimpan public key pengguna, saat koneksi server memverifikasi bahwa klien memiliki private key yang cocok.

### 5.2 Autentikasi Server (Fingerprint)

Saat pertama kali SSH ke server baru, klien menampilkan fingerprint public key server dan meminta konfirmasi. Ini penting untuk menghindari **man-in-the-middle attack** — penyerang yang menyamar sebagai server yang dituju.

Setelah dijawab "yes", fingerprint tersimpan di `~/.ssh/known_hosts`. Koneksi berikutnya berjalan otomatis kecuali server membalas dengan public key yang berbeda.

### 5.3 Generate Key Pair

```bash
ssh-keygen -t ed25519            # generate key pair dengan algoritma Ed25519
ssh-keygen -t rsa                # generate key pair RSA (kunci lebih panjang)
ssh-keygen -t ecdsa              # generate key pair ECDSA
```

Switch `ssh-keygen` yang perlu diketahui:

| Switch | Fungsi |
|---|---|
| `-t <type>` | Menentukan algoritma: dsa, ecdsa, ecdsa-sk, ed25519, ed25519-sk, rsa |
| `-b <bits>` | Menentukan panjang kunci (untuk RSA) |
| `-C <comment>` | Menambahkan komentar/label pada kunci |
| `-f <file>` | Menentukan nama dan lokasi file output |
| `-p` | Mengubah passphrase pada kunci yang sudah ada |

### 5.4 Algoritma yang Tersedia

| Algoritma | Keterangan |
|---|---|
| **dsa** | Digital Signature Algorithm — untuk tanda tangan digital |
| **ecdsa** | Elliptic Curve DSA — varian DSA dengan kunci lebih kecil untuk keamanan setara |
| **ecdsa-sk** | ECDSA + hardware security key untuk perlindungan private key lebih kuat |
| **ed25519** | EdDSA dengan Curve25519 — **direkomendasikan**, modern dan efisien |
| **ed25519-sk** | Ed25519 + hardware security key |
| **rsa** | RSA — kompatibel luas, kunci lebih panjang |

### 5.5 Perintah SSH Penting

```bash
ssh user@host                          # koneksi SSH dengan password
ssh -i privateKeyFile user@host        # koneksi SSH menggunakan key tertentu
ssh-copy-id user@host                  # salin public key ke server target
```

Switch `ssh` yang perlu diketahui:

| Switch | Fungsi |
|---|---|
| `-i <file>` | Menentukan file private key yang dipakai |
| `-p <port>` | Menentukan port (default 22) |
| `-v` | Mode verbose untuk debug koneksi |
| `-L` | Local port forwarding |

### 5.6 File dan Direktori Penting

| Path | Fungsi |
|---|---|
| `~/.ssh/id_ed25519` | Private key (jangan dibagikan) |
| `~/.ssh/id_ed25519.pub` | Public key (boleh dibagikan) |
| `~/.ssh/authorized_keys` | Daftar public key yang diizinkan akses ke server |
| `~/.ssh/known_hosts` | Fingerprint server yang sudah pernah dihubungi |

### 5.7 Aturan Permission

Private key harus memiliki permission **600** atau lebih ketat. SSH client akan mengabaikan (dengan peringatan) file private key yang permission-nya terlalu longgar.

```bash
chmod 600 ~/.ssh/id_ed25519
```

### 5.8 Identifikasi Algoritma dari Nama File

Konvensi penamaan SSH key selalu menyertakan nama algoritma:

- `id_rsa` → RSA
- `id_ed25519` → Ed25519
- `id_ecdsa` → ECDSA
- `id_dsa` → DSA

Konfirmasi lebih lanjut bisa dilakukan dengan melihat isi file:
- `-----BEGIN RSA PRIVATE KEY-----` → RSA
- `-----BEGIN OPENSSH PRIVATE KEY-----` → Ed25519 atau algoritma modern lainnya

### 5.9 SSH Key dalam CTF dan Pentesting

Meninggalkan SSH key di file `authorized_keys` pada mesin target bisa menjadi **backdoor** yang berguna — memberikan akses kembali tanpa password dan menghindari instabilitas reverse shell (seperti Ctrl-C yang tidak berfungsi atau tidak adanya tab completion).

---

## 6. Digital Signatures dan Certificates

### 6.1 Tanda Tangan Digital

**Tanda tangan digital** memverifikasi autentisitas dan integritas dokumen atau pesan digital. Berbeda dengan tanda tangan elektronik (sekadar gambar TTD yang bisa disalin siapapun), tanda tangan digital menggunakan kriptografi sehingga tidak bisa dipalsukan.

Di banyak negara modern, tanda tangan digital memiliki nilai hukum yang sama dengan tanda tangan fisik.

### 6.2 Cara Kerja Tanda Tangan Digital

Proses penandatanganan:
1. Pengirim menghitung **hash** dari dokumen.
2. Hash dienkripsi dengan **private key** pengirim → menghasilkan tanda tangan digital.
3. Dokumen asli + tanda tangan digital dikirim bersama ke penerima.

Proses verifikasi:
1. Penerima mendekripsi tanda tangan menggunakan **public key** pengirim → mendapat hash asli dari pengirim.
2. Penerima menghitung sendiri hash dari dokumen yang diterima.
3. Kedua hash dibandingkan:
   - Cocok → dokumen asli dan benar-benar dari pengirim yang diklaim.
   - Tidak cocok → dokumen telah diubah atau bukan dari pengirim yang diklaim.

### 6.3 Kenapa Pakai Hash, Bukan Enkripsi Dokumen Langsung?

Dokumen bisa berukuran besar. Mengenkripsi seluruh dokumen dengan RSA itu lambat dan tidak efisien. Hash adalah **ringkasan tetap-ukuran** dari dokumen yang jauh lebih kecil, sehingga lebih efisien untuk dienkripsi sebagai tanda tangan.

### 6.4 Perbedaan Electronic Signature vs Digital Signature

| | Electronic Signature | Digital Signature |
|---|---|---|
| Bentuk | Gambar tanda tangan | Hash terenkripsi dengan private key |
| Bisa dipalsukan? | Ya, siapapun bisa copy-paste | Tidak, butuh private key |
| Membuktikan integritas? | Tidak | Ya |

### 6.5 Sertifikat Digital

Sertifikat adalah aplikasi penting dari kriptografi kunci publik. Fungsinya membuktikan identitas — misalnya membuktikan bahwa server yang kamu akses adalah benar-benar tryhackme.com, bukan server palsu.

**Chain of Trust:**

```
Root CA
  └── Certificate Authority (CA)
        └── Organisasi/Website
              └── Browser mempercayai
```

- **Root CA** (Certificate Authority) — dipercaya secara otomatis oleh OS dan browser sejak instalasi.
- **CA** — dipercaya oleh Root CA, menerbitkan sertifikat untuk organisasi.
- **Sertifikat website** — diterbitkan oleh CA, diverifikasi oleh browser saat koneksi HTTPS.

### 6.6 TLS Certificate

Untuk mengaktifkan HTTPS pada website, dibutuhkan **TLS certificate**. Tersedia dari:
- **CA berbayar** — berbagai penyedia dengan biaya tahunan.
- **Let's Encrypt** — gratis, untuk domain yang kamu miliki sendiri.

---

## 7. PGP dan GPG

### 7.1 Definisi

**PGP** (Pretty Good Privacy) — software yang mengimplementasikan enkripsi file, penandatanganan digital, dan fungsi kriptografi lainnya.

**GPG** (GnuPG) — implementasi open-source dari standar OpenPGP. Inilah yang dipakai dalam praktik.

GPG umum digunakan untuk:
- Melindungi kerahasiaan pesan email (enkripsi).
- Menandatangani pesan email untuk konfirmasi integritas.
- Mengenkripsi/mendekripsi file.

### 7.2 Generate Key Pair GPG

```bash
gpg --full-gen-key
```

Proses interaktif yang menanyakan:
1. **Jenis kunci** — algoritma yang digunakan (ECC sign+encrypt adalah default dan direkomendasikan).
2. **Kurva eliptik** — untuk ECC: Curve25519 (default), NIST P-384, Brainpool P-256.
3. **Masa berlaku kunci** — `0` = tidak pernah expire, atau tentukan dalam hari/minggu/bulan/tahun.
4. **Identitas** — nama, alamat email, dan komentar untuk mengidentifikasi kunci.

Hasil generate menghasilkan:
- **pub** — public key (algoritma untuk signing, contoh: ed25519).
- **sub** — subkey untuk enkripsi (contoh: cv25519).

### 7.3 Perintah GPG Penting

```bash
gpg --full-gen-key                             # generate key pair baru (interactive)
gpg --import namafile.key                      # import key dari file
gpg --decrypt namafile.gpg                     # decrypt file terenkripsi
gpg --export --armor email@domain > file.asc   # export public key ke file
gpg --list-keys                                # lihat semua key di keyring
```

Switch GPG yang perlu diketahui:

| Switch | Fungsi |
|---|---|
| `--full-gen-key` | Generate key dengan semua opsi (interactive) |
| `--import` | Import key dari file ke keyring |
| `--export` | Export public key dari keyring |
| `--armor` | Output dalam format ASCII (teks), bukan binary |
| `--decrypt` | Dekripsi file .gpg |
| `--encrypt` | Enkripsi file |
| `--recipient` | Menentukan penerima enkripsi (pakai public key mereka) |
| `--sign` | Menandatangani file dengan private key |
| `--verify` | Verifikasi tanda tangan pada file |
| `--list-keys` | Tampilkan semua key di keyring |

### 7.4 Alur Kerja GPG

**Untuk menerima pesan terenkripsi:**
1. Bagikan public key ke pengirim.
2. Pengirim enkripsi menggunakan public key kamu.
3. Kamu dekripsi dengan: `gpg --decrypt namafile.gpg`

**Untuk membuka file CTF dengan key yang disediakan:**
1. Import private key yang disediakan: `gpg --import provided.key`
2. Dekripsi file: `gpg --decrypt message.gpg`

### 7.5 GPG dalam CTF

Jika private key GPG dilindungi passphrase, passphrase bisa diserang menggunakan:

```bash
gpg2john namafile.key > hash.txt    # konversi key ke format yang bisa diserang John
john hash.txt                       # crack passphrase dengan John the Ripper
```

---

## 8. Konsep Serangan Kriptografi

**Cryptanalysis** — ilmu yang mempelajari cara memecahkan atau melewati sistem kriptografi tanpa mengetahui kuncinya.

**Brute-Force Attack** — mencoba setiap kemungkinan kunci atau kombinasi password satu per satu hingga menemukan yang benar. Efektif tapi membutuhkan waktu sangat lama untuk kunci yang panjang.

**Dictionary Attack** — mencoba kata-kata dari kamus atau kombinasinya sebagai password. Lebih efisien dari brute-force karena memanfaatkan kecenderungan manusia menggunakan kata-kata yang bermakna sebagai password.

**Man-in-the-Middle (MITM) Attack** — penyerang menyisipkan diri di antara dua pihak yang berkomunikasi, berpura-pura menjadi masing-masing pihak. Inilah mengapa verifikasi fingerprint server saat SSH pertama kali penting.

**Discrete Logarithm Problem** — masalah matematika yang menjadi dasar keamanan Diffie-Hellman. Diberikan nilai `A = g^a mod p`, mencari nilai `a` secara komputasi tidak layak untuk p yang cukup besar.

---

## 9. Glossary

| Istilah | Definisi |
|---|---|
| **Cryptography** | Ilmu mengamankan komunikasi dan data menggunakan kode dan cipher |
| **Cryptanalysis** | Ilmu memecahkan atau melewati sistem keamanan kriptografi tanpa kunci |
| **Plaintext** | Data asli sebelum dienkripsi, bisa dibaca langsung |
| **Ciphertext** | Data setelah dienkripsi, tidak bisa dibaca tanpa kunci |
| **Symmetric encryption** | Enkripsi dengan satu kunci yang sama untuk enkripsi dan dekripsi |
| **Asymmetric encryption** | Enkripsi dengan dua kunci berbeda: public key dan private key |
| **Public key** | Kunci yang boleh dibagikan ke siapapun |
| **Private key** | Kunci yang harus dijaga rahasia oleh pemiliknya |
| **Key exchange** | Proses menyepakati kunci bersama melalui saluran tidak aman |
| **Hash** | Ringkasan tetap-ukuran dari data, dihasilkan oleh fungsi hash satu arah |
| **Digital signature** | Hash dokumen yang dienkripsi dengan private key pengirim |
| **Fingerprint** | Hash dari public key, dipakai untuk verifikasi identitas |
| **Passphrase** | Password untuk melindungi private key; tidak pernah dikirim ke server |
| **Certificate** | Dokumen digital yang membuktikan identitas dan memuat public key |
| **CA** | Certificate Authority — lembaga yang menerbitkan dan memverifikasi sertifikat |
| **Root CA** | CA tingkat tertinggi yang dipercaya browser/OS secara default |
| **Chain of Trust** | Rantai kepercayaan dari Root CA ke website melalui CA perantara |
| **TLS** | Transport Layer Security — protokol yang mengamankan komunikasi web (HTTPS) |
| **Forward Secrecy** | Properti di mana bocornya satu kunci sesi tidak mempengaruhi sesi lainnya |
| **MITM Attack** | Man-in-the-Middle — penyerang menyamar sebagai salah satu pihak komunikasi |
| **Brute-Force Attack** | Mencoba semua kemungkinan kunci/password satu per satu |
| **Dictionary Attack** | Mencoba kata-kata kamus atau kombinasinya sebagai password |
| **Discrete Logarithm Problem** | Masalah matematika sulit yang menjadi dasar keamanan DH |
| **Keyring** | Penyimpanan lokal untuk key-key GPG yang sudah diimport |
| **authorized_keys** | File di server yang menyimpan public key yang diizinkan untuk akses SSH |
| **known_hosts** | File yang menyimpan fingerprint server SSH yang sudah pernah dihubungi |

---

## 10. Tools & Platform Rujukan

**RsaCtfTool**
Fungsi: Tool otomatis untuk menyerang berbagai kelemahan implementasi RSA dalam CTF.
URL: https://github.com/RsaCtfTool/RsaCtfTool

**rsatool**
Fungsi: Kalkulasi dan manipulasi variabel RSA (p, q, n, e, d).
URL: https://github.com/ius/rsatool

**gpg2john**
Fungsi: Mengkonversi GPG private key yang dilindungi passphrase ke format yang bisa diserang John the Ripper.
Termasuk dalam: paket John the Ripper

**John the Ripper**
Fungsi: Password cracker — bisa dipakai untuk crack passphrase SSH key dan GPG key.
URL: https://www.openwall.com/john/

**Let's Encrypt**
Fungsi: Menyediakan TLS certificate gratis untuk domain yang kamu miliki.
URL: https://letsencrypt.org

**Mozilla Firefox — Trusted CAs**
Fungsi: Daftar Certificate Authority yang dipercaya oleh Firefox.
URL: https://wiki.mozilla.org/CA/Included_Certificates

**TryHackMe — Breaking RSA Room**
Fungsi: Room latihan khusus untuk memecahkan enkripsi RSA dalam konteks CTF.

---

## 11. Catatan Ringkas untuk Ditulis Tangan

### 4 Pilar Keamanan
- Authentication — verifikasi IDENTITAS
- Authenticity — verifikasi ASAL pesan
- Integrity — data tidak DIUBAH
- Confidentiality — tidak bisa DIBACA orang lain
- Simetris → confidentiality | Asimetris → 3 sisanya

### Kunci Asimetris
- Public key → enkripsi & verifikasi (boleh dibagikan)
- Private key → dekripsi & tanda tangan (RAHASIA)
- Enkripsi pesan → pakai public key PENERIMA
- Tanda tangan → pakai private key PENGIRIM
- Asimetris lambat → pakai sekali untuk tukar kunci simetris

### RSA
- Dasar: faktorisasi bilangan besar susah
- p, q = prima besar (rahasia)
- n = p × q (publik)
- e = komponen publik, d = komponen privat
- Public key = (n,e) | Private key = (n,d)
- Enkripsi: c = m^e mod n
- Dekripsi: m = c^d mod n
- Aman karena n sulit difaktorkan jika 600+ digit
- CTF tools: RsaCtfTool, rsatool

### Diffie-Hellman
- Tujuan: sepakat kunci bersama tanpa kirim kuncinya
- p, g = publik | a, b = private key masing-masing
- A = g^a mod p (pub Alice) | B = g^b mod p (pub Bob)
- Shared secret: B^a = A^b = g^ab mod p (hasil sama!)
- Kenapa sama: (x^b)^a = (x^a)^b = x^ab
- Keamanan: Discrete Logarithm Problem
- DH = key agreement | RSA = digital signature & auth

### SSH
- 2 metode: password (tidak ideal) vs key auth (lebih aman)
- ssh-keygen -t ed25519 → generate key pair
- ssh-keygen -t rsa → generate RSA (kunci lebih panjang)
- ssh -i keyfile user@host → SSH pakai key tertentu
- ssh-copy-id user@host → copy public key ke server
- chmod 600 ~/.ssh/id_* → permission wajib untuk private key
- ~/.ssh/authorized_keys → public key yang boleh akses server
- ~/.ssh/known_hosts → fingerprint server yang dikenal
- Nama file: id_rsa=RSA, id_ed25519=Ed25519
- Isi file: BEGIN RSA PRIVATE KEY=RSA, BEGIN OPENSSH=modern
- Algoritma: dsa, ecdsa, ecdsa-sk, ed25519 (rekomendasi), ed25519-sk, rsa
- CTF: taruh key di authorized_keys → backdoor berguna

### Digital Signature
- Tanda tangan = hash dokumen + enkripsi dengan private key
- Verifikasi: dekripsi pakai public key pengirim → cocokkan hash
- Yang dikirim: file asli + hash terenkripsi (2 hal)
- Pakai hash bukan dokumen langsung → lebih efisien
- Electronic signature (gambar TTD) ≠ Digital signature
- Legal value: sama dengan tanda tangan fisik di banyak negara

### Sertifikat & Chain of Trust
- Certificate → buktikan identitas server (pakai di HTTPS)
- Root CA → dipercaya browser/OS otomatis
- Chain: Root CA → CA → organisasi → browser percaya
- TLS cert: beli dari CA berbayar atau gratis via Let's Encrypt

### GPG
- PGP = Pretty Good Privacy | GPG = open-source OpenPGP
- gpg --full-gen-key → generate key pair (interactive)
- gpg --import file.key → import key
- gpg --decrypt file.gpg → decrypt file
- gpg --export --armor email > file.asc → export public key
- gpg --list-keys → lihat semua key di keyring
- Kalau key berpassphrase di CTF: gpg2john → john
- Backup key penting! Simpan di lokasi aman

### Serangan Kriptografi
- Brute-force — coba semua kemungkinan
- Dictionary attack — coba kata-kata kamus
- MITM — penyerang menyamar sebagai salah satu pihak
- Cryptanalysis — ilmu memecahkan sistem kriptografi
