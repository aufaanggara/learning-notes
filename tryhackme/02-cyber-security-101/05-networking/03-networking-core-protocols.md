# Resume Materi: Networking Core Protocols
**TryHackMe Room** | 22 Oktober 2026

---

## 1. DNS (Domain Name System)

### 1.1 Konsep Dasar

**DNS** adalah sistem yang menerjemahkan nama domain yang mudah diingat manusia menjadi alamat IP yang dimengerti mesin. Tanpa DNS, setiap akses ke website membutuhkan hafalan alamat IP secara manual.

- Beroperasi di **Application Layer (Layer 7)** model ISO OSI
- Menggunakan **UDP port 53** secara default; **TCP port 53** sebagai fallback ketika data terlalu besar untuk satu paket UDP
- Siapapun yang mendaftarkan domain mendapat otoritas untuk mengatur semua DNS record domain tersebut

### 1.2 Jenis-Jenis DNS Record

**A Record** — memetakan hostname ke satu atau lebih alamat **IPv4**. Contoh: `example.com → 172.17.2.172`

**AAAA Record** — sama seperti A Record tapi untuk **IPv6** (disebut quad-A; bukan singkatan baterai, bukan Authentication Authorization Accounting)

**CNAME Record** — memetakan satu nama domain ke nama domain lain (alias). Contoh: `www.example.com → example.com`

**MX Record** — menentukan mail server yang bertanggung jawab menangani email untuk domain tersebut

### 1.3 Cara Kerja Query DNS

- Buka browser `example.com` → browser query DNS server untuk **A Record**
- Kirim email ke `user@example.com` → mail server query DNS server untuk **MX Record**

Satu perintah `nslookup` menghasilkan 4 paket:
- Paket 1 & 3 → query keluar (A Record, lalu AAAA Record)
- Paket 2 & 4 → response masuk (jawaban IPv4, lalu jawaban IPv6)

### 1.4 Command & Tool

```
nslookup <domain>
```

Output penting dari `nslookup`:
- **Server** → DNS resolver yang digunakan sistem
- **Non-authoritative answer** → jawaban dari cache resolver, bukan langsung dari server pemilik domain (valid, tapi bukan sumber pertama)
- **Address (IPv4)** → hasil A Record
- **Address (IPv6)** → hasil AAAA Record

```
tshark -r dns-query.pcapng -Nn
```

- `-r <file>` → baca dari file capture (.pcapng)
- `-N` → nonaktifkan resolusi nama jaringan
- `-n` → nonaktifkan resolusi nama DNS (tampilkan IP mentah)

---

## 2. WHOIS

### 2.1 Konsep Dasar

**WHOIS** adalah protokol dan database yang menyimpan informasi tentang siapa yang mendaftarkan sebuah domain atau alamat IP. Dibaca *"who is"* — bukan akronim.

Informasi yang tersimpan di WHOIS record bersifat **publik** secara default dan mencakup: nama registrant, nomor telepon, email, alamat, tanggal pembuatan, tanggal expired, dan nama registrar.

### 2.2 Privacy Protection

Pemilik domain dapat menggunakan **layanan privasi** yang menggantikan data asli mereka dengan data perusahaan proxy. Contoh: ketika domain menggunakan GoDaddy privacy protection, yang tampil adalah **Domains By Proxy, LLC** — bukan nama pemilik aslinya.

### 2.3 Isi WHOIS Record

- **Registrar** — perusahaan tempat domain didaftarkan (contoh: GoDaddy, CSC Corporate Domains)
- **Registrar WHOIS Server** — server WHOIS spesifik milik registrar tersebut
- **Creation Date** — kapan domain pertama kali didaftarkan
- **Updated Date** — kapan record terakhir diperbarui
- **Expiration Date** — kapan registrasi domain berakhir
- **Domain Status** — status domain (clientTransferProhibited, serverDeleteProhibited, dst.)
- **Registrant / Admin / Tech** — tiga kontak yang masing-masing punya nama, organisasi, alamat, telepon, fax, dan email

### 2.4 Command

```
whois <domain>
```

---

## 3. HTTP & HTTPS

### 3.1 Konsep Dasar

**HTTP (Hypertext Transfer Protocol)** mendefinisikan cara browser berkomunikasi dengan web server. Bergantung pada TCP.

**HTTPS** = HTTP + enkripsi SSL/TLS. Huruf S berarti *Secure*.

| Protokol | Port Default | Port Alternatif |
|----------|-------------|-----------------|
| HTTP | 80 | 8080 |
| HTTPS | 443 | 8443 |

### 3.2 HTTP Methods

**GET** — mengambil data dari server (HTML, gambar, file)

**POST** — mengirim data baru ke server (form submission, upload file)

**PUT** — membuat resource baru atau mengupdate dan menimpa resource yang sudah ada

**DELETE** — menghapus file atau resource tertentu di server

### 3.3 HTTP Headers

**Request headers** (dikirim browser ke server):

- `Host` — domain atau IP tujuan
- `User-Agent` — identitas browser dan sistem operasi
- `Accept` — jenis konten yang bisa diterima browser
- `Accept-Encoding` — format kompresi yang didukung (gzip, deflate)
- `Accept-Language` — preferensi bahasa browser
- `Connection: keep-alive` — minta koneksi tetap terbuka untuk dipakai ulang

**Response headers** (dikirim server ke browser):

- `200 OK` — request berhasil diproses
- `Server` — software dan OS server (contoh: nginx/1.18.0 Ubuntu) — **tidak tampil ke pengguna**
- `Content-Type` — jenis konten yang dikirim (text/html, dll.)
- `Last-Modified` — kapan halaman terakhir diubah di server — **tidak tampil ke pengguna**
- `ETag` — sidik jari unik versi halaman untuk keperluan caching
- `Content-Encoding` — format kompresi data yang dikirim (gzip)

### 3.4 Akses Manual via Telnet

```
telnet <IP> 80
```

Setelah terkoneksi, ketik:

```
GET /namafile.html HTTP/1.1
Host: anything
```

Tekan Enter dua kali untuk mengirim request. Slash dan nama file tidak boleh ada spasi (`/namafile.html` bukan `/ namafile.html`).

---

## 4. FTP (File Transfer Protocol)

### 4.1 Konsep Dasar

**FTP** dirancang khusus untuk transfer file antar komputer — lebih efisien dari HTTP untuk keperluan ini karena tidak membawa overhead rendering halaman web.

- Port kontrol: **TCP 21**
- Transfer data dilakukan via **koneksi terpisah** dari koneksi kontrol
- Anonymous login tersedia di beberapa server publik (tanpa password)

### 4.2 Command FTP (Sisi Pengguna)

| Command | Fungsi |
|---------|--------|
| `ls` | Lihat isi direktori |
| `get <file>` | Download file dari server |
| `put <file>` | Upload file ke server |
| `type ascii` | Ganti mode ke ASCII (opsional, untuk file teks) |
| `quit` | Keluar dari sesi FTP |

### 4.3 Command Protokol Asli (Di Balik Layar)

FTP client menerjemahkan command pengguna ke protokol asli secara otomatis:

- `ls` → dikirim sebagai **LIST**
- `get` → dikirim sebagai **RETR** (retrieve)
- `put` → dikirim sebagai **STOR** (store)
- `USER` → mengirim username
- `PASS` → mengirim password
- `QUIT` → menutup sesi

### 4.4 Catatan Mode Transfer

**Binary mode** (default) — transfer byte per byte, cocok untuk semua jenis file termasuk teks. Tidak perlu ganti mode untuk download `.txt`.

**ASCII mode** (`type ascii`) — melakukan konversi format baris antar sistem (Windows vs Linux). Opsional dan kadang memunculkan warning jika ada perbedaan format newline.

### 4.5 Koneksi

```
ftp <IP>
```

---

## 5. SMTP (Simple Mail Transfer Protocol)

### 5.1 Konsep Dasar

**SMTP** mendefinisikan cara mail client berbicara dengan mail server, dan cara satu mail server berbicara dengan mail server lain. SMTP khusus untuk **mengirim** email.

- Port default: **TCP 25**
- Versi terenkripsi: port **465** (SMTPS) atau **587** (SMTP dengan STARTTLS)

### 5.2 Command SMTP

**HELO / EHLO** — memulai sesi SMTP, memperkenalkan diri ke server

**MAIL FROM: \<email\>** — menentukan alamat email pengirim

**RCPT TO: \<email\>** — menentukan alamat email penerima (harus pakai format `<user@domain>`)

**DATA** — memberitahu server bahwa konten pesan akan mulai dikirim

**.** — titik pada baris tersendiri, menandai akhir pesan

**QUIT** — menutup sesi SMTP

### 5.3 Alur Pengiriman Email via Telnet

```
telnet <IP> 25
```

Setelah terkoneksi:

```
HELO <namaclient>
MAIL FROM: <pengirim@domain>
RCPT TO: <penerima@domain>
DATA
From: pengirim@domain
To: penerima@domain
Subject: Judul email

Isi pesan di sini.
.
QUIT
```

### 5.4 Error Umum SMTP

- **500 unrecognized command** — typo pada command (contoh: `RCPT TP` bukan `RCPT TO`)
- **550 relay not permitted** — server menolak meneruskan email ke domain eksternal (gmail.com, dll.) karena bukan domain yang dikelolanya
- **501 recipient address must contain a domain** — format salah, tidak ada tanda `@`
- **550 Unroutable address** — format benar tapi user tidak terdaftar di server tersebut
- **250 Accepted** — penerima valid dan diterima

### 5.5 Informasi yang Terungkap di Header SMTP

Ketika server menyambut koneksi (`220`), biasanya terungkap versi software mail server yang digunakan (contoh: Exim 4.95, Ubuntu). Ini informasi yang tidak tampil ke pengguna email biasa tapi terlihat jelas di koneksi raw.

---

## 6. POP3 (Post Office Protocol version 3)

### 6.1 Konsep Dasar

**POP3** dirancang untuk mengambil email dari server ke perangkat lokal. Sifatnya download-and-delete — setelah diambil, email dihapus dari server. Cocok hanya untuk satu perangkat.

- Port default: **TCP 110** (plaintext)
- Port SSL/TLS: **TCP 995** (POP3S)

### 6.2 Command POP3

**USER \<username\>** — mengidentifikasi user yang akan login

**PASS \<password\>** — memberikan password

**STAT** — meminta jumlah pesan dan total ukurannya (format respons: `+OK jumlah total_bytes`)

**LIST** — menampilkan semua pesan dengan nomor dan ukurannya

**RETR \<nomor\>** — mengambil pesan tertentu berdasarkan nomornya

**DELE \<nomor\>** — menandai pesan untuk dihapus

**QUIT** — mengakhiri sesi dan menerapkan semua perubahan (termasuk penghapusan)

### 6.3 Koneksi Plaintext

```
telnet <IP> 110
```

### 6.4 Koneksi SSL/TLS

Dibutuhkan ketika server menolak plaintext dengan error:
`-ERR [AUTH] Plaintext authentication disallowed on non-secure (SSL/TLS) connections`

```
openssl s_client -connect <IP>:995 -quiet
```

**Penjelasan flag openssl:**

- `s_client` — subcommand untuk mode SSL/TLS client (seperti telnet tapi terenkripsi)
- `-connect <IP:PORT>` — menentukan server dan port tujuan (wajib ada)
- `-quiet` — menekan output SSL verbose dan mengubah cara openssl menangani aliran data; **penting** karena tanpanya `RETR` bisa gagal akibat EOF handling yang salah atau trigger renegotiation error
- `-legacy_renegotiation` — mengizinkan renegotiasi SSL versi lama yang diminta server (pakai jika muncul error `RENEGOTIATING`)

Setelah terkoneksi, command POP3 sama persis dengan koneksi plaintext.

### 6.5 Risiko Keamanan

Koneksi POP3 plaintext (port 110) tidak mengenkripsi data sama sekali. Siapapun yang menangkap paket di jaringan (via Wireshark, tcpdump, dll.) bisa membaca username, password, dan isi email secara langsung. Ini adalah celah **MITM (Man-in-the-Middle) attack**.

---

## 7. IMAP (Internet Message Access Protocol)

### 7.1 Konsep Dasar

**IMAP** dirancang untuk sinkronisasi email di banyak perangkat sekaligus. Email tetap tersimpan di server dan semua perubahan (dibaca, dihapus, dipindahkan) tersinkron ke semua device. Cocok untuk penggunaan multi-perangkat.

- Port default: **TCP 143** (plaintext)
- Port SSL/TLS: **TCP 993** (IMAPS)
- Menggunakan lebih banyak storage server dibanding POP3 karena email tidak dihapus setelah diambil

### 7.2 Tag pada IMAP

Setiap command IMAP diawali **tag** berupa huruf (A, B, C, D, ...). Tag ini mencocokkan setiap command dengan responnya — penting karena IMAP bisa menangani banyak command secara bersamaan.

### 7.3 Command IMAP

**LOGIN \<username\> \<password\>** — autentikasi user (diawali tag, contoh: `A LOGIN strategos pass123`)

**SELECT \<mailbox\>** — memilih folder mailbox yang akan dikerjakan (contoh: `B SELECT inbox`)

**FETCH \<nomor\> \<data_item\>** — mengambil pesan tertentu (contoh: `C FETCH 3 body[]` untuk ambil pesan nomor 3 beserta header dan body)

**MOVE \<sequence_set\> \<mailbox\>** — memindahkan pesan ke folder lain

**COPY \<sequence_set\> \<data_item\>** — menyalin pesan ke folder lain

**LOGOUT** — mengakhiri sesi IMAP (contoh: `D LOGOUT`)

### 7.4 Informasi dari Respons SELECT

Ketika memilih mailbox, server mengembalikan metadata penting:

- `FLAGS` — jenis flag yang tersedia: `\Answered \Flagged \Deleted \Seen \Draft`
- `EXISTS` — jumlah total pesan di mailbox
- `RECENT` — jumlah pesan baru sejak sesi terakhir
- `UNSEEN` — nomor pesan pertama yang belum dibaca
- `UIDVALIDITY` — angka validasi untuk memastikan UID pesan masih berlaku
- `UIDNEXT` — UID yang akan diberikan untuk pesan berikutnya
- `READ-WRITE` — mailbox bisa dibaca dan dimodifikasi

### 7.5 Koneksi

```
telnet <IP> 143
```

---

## 8. Perbandingan POP3 vs IMAP

| Aspek | POP3 | IMAP |
|-------|------|------|
| Sifat email | Download & hapus dari server | Tetap di server |
| Multi-perangkat | Tidak cocok | Cocok |
| Storage server | Minimal | Lebih besar |
| Sinkronisasi | Tidak ada | Penuh (baca, hapus, pindah) |
| Port plaintext | 110 | 143 |
| Port SSL/TLS | 995 | 993 |

---

## 9. Tabel Port Semua Protokol

| Protokol | Transport | Port Default | Port SSL/TLS |
|----------|-----------|-------------|--------------|
| TELNET | TCP | 23 | — |
| DNS | UDP / TCP | 53 | — |
| HTTP | TCP | 80 | — |
| HTTPS | TCP | 443 | — |
| FTP | TCP | 21 | — |
| SMTP | TCP | 25 | 465 / 587 |
| POP3 | TCP | 110 | 995 |
| IMAP | TCP | 143 | 993 |

---

## 10. Glossary

**A Record** — DNS record yang memetakan hostname ke alamat IPv4

**AAAA Record** — DNS record yang memetakan hostname ke alamat IPv6 (quad-A)

**ASCII mode** — mode transfer FTP yang melakukan konversi format baris antar sistem; opsional untuk file teks

**Binary mode** — mode transfer FTP default; transfer file byte per byte tanpa konversi; cocok untuk semua jenis file

**CNAME Record** — DNS record yang memetakan satu nama domain ke nama domain lain (alias)

**EOF (End of File)** — sinyal yang menandai akhir dari data yang dikirim

**ETag** — sidik jari unik versi sebuah halaman web, digunakan untuk caching

**MX Record** — DNS record yang menentukan mail server untuk sebuah domain

**MITM Attack (Man-in-the-Middle)** — serangan di mana penyerang menyadap dan bisa membaca/memodifikasi komunikasi antara dua pihak

**Non-authoritative answer** — respons DNS yang berasal dari cache resolver, bukan langsung dari server pemilik domain; tetap valid

**Open Relay** — SMTP server yang mau meneruskan email ke domain manapun; berbahaya karena bisa disalahgunakan untuk spam

**Passive Mode (FTP)** — mode di mana client yang membuka koneksi data ke server, bukan sebaliknya; digunakan untuk menghindari masalah firewall

**Plaintext** — data yang dikirim tanpa enkripsi; bisa dibaca oleh siapapun yang menangkap paketnya

**Registrant** — pemilik atau pendaftar domain

**Registrar** — perusahaan tempat domain didaftarkan (contoh: GoDaddy, Namecheap)

**Relay** — proses SMTP server meneruskan email ke domain yang bukan miliknya; diblokir secara default oleh server modern

**Renegotiation (SSL)** — proses "refresh" parameter enkripsi SSL di tengah koneksi yang sedang berjalan

**SSL/TLS** — protokol enkripsi yang melindungi data selama proses transfer

**Tag (IMAP)** — huruf penanda di depan setiap command IMAP untuk mencocokkan command dengan responnya

**WHOIS** — protokol dan database yang menyimpan informasi registrasi domain; dibaca "who is", bukan akronim

---

## 11. Tools & Platform Rujukan

**nslookup** — tool bawaan sistem operasi untuk query DNS record; tersedia di Windows, Linux, macOS

**whois** — tool command line untuk query WHOIS database; tersedia di Linux dan macOS

**telnet** — tool untuk koneksi raw ke server via protokol tertentu; berguna untuk "berbicara" langsung dengan server HTTP, SMTP, FTP, POP3, IMAP secara manual

**openssl** — toolkit kriptografi open source; subcommand `s_client` digunakan sebagai pengganti telnet untuk koneksi yang membutuhkan SSL/TLS

**Wireshark** — tool analisis paket jaringan berbasis GUI; digunakan untuk memeriksa traffic secara detail termasuk HTTP stream, DNS query, SMTP session, dll. — https://www.wireshark.org

**tshark** — versi command line dari Wireshark; untuk analisis paket dari terminal

---

## 12. Catatan Ringkas untuk Ditulis Tangan

### DNS

- DNS — terjemahkan domain → IP, Layer 7 OSI
- Port — UDP 53 (default), TCP 53 (fallback)
- A Record — hostname → IPv4
- AAAA Record — hostname → IPv6 (quad-A)
- CNAME — domain → domain lain (alias)
- MX Record — tentukan mail server domain
- Browser buka domain → query A Record
- Kirim email → query MX Record
- nslookup = 4 paket: query A, resp A, query AAAA, resp AAAA
- Non-authoritative = dari cache, bukan server asli, tetap valid

### WHOIS

- WHOIS — database info pemilik domain, baca "who is"
- whois <domain> — lihat info registrasi
- Isi: nama, telp, email, alamat, tgl buat, tgl expired, registrar
- Privacy protection → nama proxy tampil, bukan pemilik asli
- Registrar = tempat daftar domain; Registrant = pemiliknya

### HTTP & HTTPS

- HTTP = port 80 TCP; HTTPS = port 443 TCP
- GET — ambil data; POST — kirim data baru
- PUT — buat/timpa resource; DELETE — hapus resource
- Header Server & Last-Modified tidak tampil ke pengguna, terlihat di raw traffic
- telnet <IP> 80 → GET /file.html HTTP/1.1 → Host: anything → Enter 2x

### FTP

- FTP = port 21 TCP, transfer file efisien
- ls → LIST; get → RETR; put → STOR
- type ascii — opsional, tidak wajib untuk .txt
- Binary mode (default) cocok semua file
- Data transfer via koneksi TERPISAH dari koneksi kontrol
- ftp <IP> → login anonymous jika server izinkan

### SMTP

- SMTP = port 25 TCP, khusus KIRIM email
- HELO/EHLO — mulai sesi
- MAIL FROM: <x@domain> — alamat pengirim
- RCPT TO: <x@domain> — alamat penerima (pakai <>)
- DATA — mulai tulis pesan
- . (titik baris sendiri) — akhir pesan
- QUIT — tutup sesi
- 550 relay not permitted = domain eksternal, tolak
- 250 Accepted = berhasil

### POP3

- POP3 = port 110 TCP (plaintext), 995 (SSL)
- Download & hapus dari server, 1 perangkat
- USER → PASS → STAT → LIST → RETR <n> → QUIT
- STAT → jumlah pesan + total bytes
- DELE <n> → tandai hapus (berlaku saat QUIT)
- telnet <IP> 110 — koneksi plaintext
- openssl s_client -connect <IP>:995 -quiet — koneksi SSL
- -quiet — sembunyikan SSL verbose + fix EOF handling (penting!)
- -legacy_renegotiation — izinkan renegotiation lama jika error
- Plaintext = password bisa dibaca di Wireshark → MITM risk

### IMAP

- IMAP = port 143 TCP (plaintext), 993 (SSL)
- Email tetap di server, sinkron semua perangkat
- Setiap command pakai TAG huruf (A B C D...)
- A LOGIN user pass — autentikasi
- B SELECT inbox — pilih folder
- C FETCH 3 body[] — ambil pesan nomor 3 (header + body)
- D LOGOUT — keluar
- EXISTS = jumlah pesan; UNSEEN = nomor pesan belum dibaca
- Tag = cocokkan command dengan responnya

### Port Wajib Hapal

- TELNET — TCP 23
- DNS — UDP/TCP 53
- HTTP — TCP 80
- HTTPS — TCP 443
- FTP — TCP 21
- SMTP — TCP 25
- POP3 — TCP 110 (SSL: 995)
- IMAP — TCP 143 (SSL: 993)

### Istilah Kunci

- Plaintext — tanpa enkripsi, bisa disadap
- MITM — sadap koneksi di tengah jalan
- SSL/TLS — lapisan enkripsi
- Open Relay — SMTP forward ke domain manapun, berbahaya
- Passive Mode FTP — client buka koneksi data ke server
- Renegotiation SSL — refresh parameter enkripsi di tengah sesi
- Non-authoritative — dari cache DNS, valid tapi bukan sumber pertama
- Registrar — perusahaan tempat domain didaftar
- Registrant — pemilik domain
