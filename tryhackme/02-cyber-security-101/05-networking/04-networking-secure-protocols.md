# Networking Secure Protocols — Resume Materi
**TryHackMe Room** | Diselesaikan: 03 Mei 2026

---

## 1. Latar Belakang — Mengapa Protokol Lama Tidak Aman

Protokol klasik seperti HTTP, SMTP, POP3, IMAP, dan TELNET mengirimkan semua data dalam bentuk **cleartext** — teks mentah yang bisa dibaca siapapun yang menangkap paket di jaringan.

Penyerang memanfaatkan ini dengan mengatur network card ke **promiscuous mode**, yaitu mode yang menangkap semua paket di jaringan termasuk yang bukan ditujukan ke perangkat mereka, lalu mengekstrak kredensial dari hasil tangkapan tersebut.

Tiga hal yang tidak dilindungi protokol lama:

- **Confidentiality** — isi data bisa dibaca siapapun di jalur pengiriman
- **Integrity** — isi data bisa diubah di tengah jalan tanpa diketahui
- **Authenticity** — tidak ada cara memverifikasi bahwa server yang diajak bicara adalah server yang benar, bukan server palsu

---

## 2. Tiga Pendekatan Keamanan Jaringan

| Pendekatan | Dipakai Untuk |
|---|---|
| **TLS** | Enkripsi protokol yang sudah ada (HTTP, SMTP, dll) |
| **SSH** | Pengganti TELNET untuk akses remote yang aman |
| **VPN** | Jaringan privat virtual di atas infrastruktur internet publik |

---

## 3. SSL dan TLS

### 3.1 Sejarah

- **SSL 2.0** — Netscape Communications, 1995, versi publik pertama
- **SSL 3.0** — 1996, ditemukan celah besar (serangan POODLE)
- **TLS 1.0** — IETF, 1999, pengganti resmi SSL 3.0
- **TLS 1.3** — 2018, versi terbaru dengan perombakan besar

SSL sudah tidak digunakan lagi karena terbukti tidak aman. TLS adalah penerusnya. Istilah "SSL Certificate" di dunia nyata sebenarnya merujuk ke TLS — nama lama sudah terlanjur melekat.

### 3.2 Posisi di Model OSI

TLS beroperasi di **transport layer**. Fungsinya melindungi confidentiality dan integrity komunikasi antara klien dan server tanpa mengubah protokol di lapisan bawah (TCP/IP) maupun di lapisan atas (HTTP, SMTP, dll).

### 3.3 Sertifikat TLS

Setiap server yang perlu membuktikan identitasnya harus punya sertifikat TLS yang sudah ditandatangani oleh pihak ketiga.

Alurnya:
1. Admin server buat **CSR (Certificate Signing Request)**
2. CSR dikirim ke **CA (Certificate Authority)**
3. CA verifikasi CSR dan terbitkan **sertifikat digital**
4. Sertifikat dipasang di server dan dipakai untuk membuktikan identitas ke klien

Agar klien bisa memverifikasi sertifikat, sertifikat dari CA yang menandatanganinya harus sudah terpasang di perangkat klien. Browser sudah punya daftar CA tepercaya bawaan (terlihat di Certificate Manager).

**Self-signed certificate** — sertifikat yang ditandatangani sendiri, tanpa pihak ketiga. Tidak bisa membuktikan keaslian server karena tidak ada CA yang menjamin.

**Let's Encrypt** — CA yang memungkinkan mendapat sertifikat TLS secara gratis.

---

## 4. Cipher dan Cipher Suite

**Cipher** adalah algoritma matematika untuk mengenkripsi dan mendekripsi data. TLS tidak memakai satu cipher saja, melainkan kombinasi yang disebut **cipher suite**.

Contoh cipher suite: `TLS_AES_256_GCM_SHA384`

| Bagian | Keterangan |
|---|---|
| TLS | Protokol yang digunakan |
| AES_256_GCM | Cipher untuk enkripsi data (AES 256-bit, mode GCM) |
| SHA384 | Algoritma hashing untuk verifikasi integritas |

Cipher suite lain yang umum: `TLS_CHACHA20_POLY1305_SHA256`, `TLS_AES_128_GCM_SHA256`.

### 4.1 Proses Negosiasi Cipher

Klien dan server harus sepakat dulu cipher suite mana yang dipakai sebelum komunikasi dimulai:

1. **Client Hello** — klien kirim daftar cipher suite yang didukungnya
2. **Server Hello** — server pilih satu cipher suite dari daftar klien dan memberitahu pilihannya
3. Setelah sepakat, seluruh komunikasi berikutnya memakai cipher suite tersebut

---

## 5. HTTPS — HTTP over TLS

**HTTPS** (Hypertext Transfer Protocol Secure) adalah HTTP yang berjalan di atas TLS. Port default: **443** (HTTP cleartext: port 80).

### 5.1 Urutan Koneksi HTTPS

1. **TCP Three-Way Handshake** — SYN, SYN-ACK, ACK (3 paket)
2. **TLS Negotiation** — Client Hello, Server Hello, Change Cipher Spec, Finished (5 paket, total 8 paket sebelum data)
3. **Sesi TLS terbentuk** — enkripsi aktif
4. **Komunikasi HTTP** — data dikirim dalam bentuk terenkripsi
5. **Pemutusan koneksi** — sesi TLS ditutup, lalu koneksi TCP ditutup

### 5.2 Traffic HTTPS di Wireshark

Tanpa kunci dekripsi, traffic HTTPS terlihat sebagai "Application Data" yang berisi gibberish — tidak bisa dibaca. Untuk mendekripsinya dibutuhkan **private key** atau **ssl-key.log** (session key log).

### 5.3 Cara Membuat ssl-key.log

```bash
# Chromium/Chrome
chromium --ssl-key-log-file=~/ssl-key.log

# Firefox (Linux/Mac)
SSLKEYLOGFILE=~/ssl-key.log firefox
```

- `--ssl-key-log-file=PATH` — flag Chromium untuk menyimpan session key TLS ke file di path yang ditentukan

### 5.4 Cara Mendekripsi Traffic TLS di Wireshark

1. Klik kanan paket **TLSv1.3** di Wireshark
2. Pilih **Protocol Preferences**
3. Pilih **Transport Layer Security**
4. Klik **Open Transport Layer Security preferences...**
5. Di kolom **(Pre)-Master-Secret log filename**, klik Browse → arahkan ke ssl-key.log
6. Klik **OK** → semua traffic TLS terdekripsi

Penting: harus klik kanan paket TLSv1.3, bukan TCP biasa — menu Protocol Preferences bersifat kontekstual sesuai protokol paket yang diklik.

---

## 6. Tabel Port Cleartext vs Secure

| Protokol | Port Cleartext | Protokol Secure | Port Secure |
|---|---|---|---|
| HTTP | 80 | HTTPS | 443 |
| SMTP | 25 | SMTPS | 465 / 587 |
| POP3 | 110 | POP3S | 995 |
| IMAP | 143 | IMAPS | 993 |
| FTP | 21 | FTPS | 990 |
| TELNET | 23 | SSH | 22 |

### 6.1 Catatan SMTPS — Dua Port

**Port 465** — port lama, tidak lagi resmi tapi masih banyak dipakai karena kebiasaan.

**Port 587** — port resmi saat ini, menggunakan metode **STARTTLS** yaitu koneksi dimulai sebagai plaintext lalu di-upgrade ke TLS di tengah jalan. Berbeda dengan port 443/993/995/990 yang langsung TLS dari awal.

---

## 7. SSH (Secure Shell)

SSH dibuat sebagai pengganti TELNET yang mengirim semua data dalam cleartext.

- **SSH-1** — Tatu Ylönen, 1995 (tahun yang sama dengan SSL 2.0)
- **SSH-2** — 1996, versi yang lebih aman
- **OpenSSH** — 1999, implementasi open-source, yang paling banyak dipakai sampai sekarang

Port SSH: **22** (TELNET: port 23)

### 7.1 Keunggulan SSH

- **Secure Authentication** — mendukung password, public key, dan two-factor authentication
- **Confidentiality** — enkripsi end-to-end, memberi notifikasi jika server key berubah (proteksi MITM)
- **Integrity** — kriptografi melindungi integritas traffic, bukan hanya kerahasiaannya
- **Tunneling** — bisa membuat "terowongan" aman untuk merutekan protokol lain, menghasilkan koneksi mirip VPN
- **X11 Forwarding** — memungkinkan menjalankan dan menampilkan aplikasi GUI dari sistem remote ke layar lokal

### 7.2 Perintah SSH

```bash
ssh username@hostname        # login dengan username berbeda dari yang aktif
ssh hostname                 # login dengan username yang sama dengan yang aktif
ssh 192.168.1.1 -X           # login + aktifkan X11 Forwarding
```

- `username@hostname` — tentukan user dan host tujuan
- `-X` — aktifkan X11 Forwarding untuk menampilkan GUI aplikasi remote; sistem lokal perlu memiliki X11 server

### 7.3 X11 Forwarding

Fitur SSH yang memungkinkan aplikasi grafis di mesin remote ditampilkan di layar mesin lokal melalui koneksi SSH. Bukan bagian inti SSH, melainkan fitur tambahan yang memanfaatkan terowongan SSH yang sudah terbentuk.

---

## 8. Koneksi Manual ke Protokol TLS

**Telnet** tidak bisa dipakai untuk konek ke protokol berbasis TLS karena tidak memiliki kemampuan TLS negotiation. Gunakan **OpenSSL s_client** sebagai gantinya.

```bash
openssl s_client -connect ip:443          # HTTPS
openssl s_client -connect ip:993          # IMAPS
openssl s_client -connect ip:995          # POP3S
openssl s_client -connect ip:465          # SMTPS
openssl s_client -connect ip:990          # FTPS
openssl s_client -connect ip:587 -starttls smtp   # SMTPS via STARTTLS
openssl s_client -connect ip:143 -starttls imap   # IMAP via STARTTLS
openssl s_client -connect ip:110 -starttls pop3   # POP3 via STARTTLS
```

- `-connect ip:port` — tentukan host dan port tujuan
- `-starttls protokol` — mulai koneksi sebagai plaintext lalu upgrade ke TLS; nilai protokol bisa `smtp`, `imap`, atau `pop3`

Setelah TLS session terbentuk, bisa langsung mengetik perintah protokol secara manual (GET untuk HTTP, USER/PASS untuk POP3, LOGIN untuk IMAP, dll).

---

## 9. SFTP dan FTPS

### 9.1 SFTP (SSH File Transfer Protocol)

- Bagian dari **SSH protocol suite**, bukan protokol terpisah
- Pakai port yang sama dengan SSH: **port 22**
- Setup: cukup aktifkan opsi di konfigurasi OpenSSH server
- Perintah Unix-like, bisa berbeda dari perintah FTP biasa

```bash
sftp username@hostname    # konek ke SFTP server
get filename              # download file dari server ke lokal
put filename              # upload file dari lokal ke server
```

### 9.2 FTPS (File Transfer Protocol Secure)

- FTP yang diamankan dengan **TLS**, konsep sama seperti HTTPS
- Port default: **990**
- Membutuhkan **sertifikat TLS** yang valid untuk bisa berjalan
- Bisa sulit melewati firewall ketat karena memakai koneksi terpisah untuk kontrol dan transfer data

### 9.3 Perbandingan SFTP vs FTPS

| | SFTP | FTPS |
|---|---|---|
| Dasar | SSH | FTP + TLS |
| Port | 22 | 990 |
| Sertifikat TLS | Tidak butuh | Wajib |
| Setup | Mudah (opsi OpenSSH) | Perlu konfigurasi sertifikat |

---

## 10. VPN (Virtual Private Network)

**V (Virtual)** — tidak membutuhkan kabel fisik antar lokasi, dibangun di atas infrastruktur internet yang sudah ada.

**P (Private)** — traffic terlindungi dari pengungkapan dan pengubahan sepanjang jalur pengiriman.

**N (Network)** — membentuk jaringan sendiri yang menghubungkan beberapa titik seolah berada di jaringan fisik yang sama.

Persyaratan minimal: koneksi internet + VPN server + VPN client.

### 10.1 Cara Kerja

VPN client mengenkripsi semua traffic dan mengirimnya melalui **VPN tunnel** ke VPN server. Server tujuan hanya melihat IP VPN server, bukan IP asli pengguna. ISP lokal hanya melihat traffic terenkripsi, tidak bisa membaca isinya.

### 10.2 Dua Skenario Penggunaan

**Site-to-Site VPN** — menghubungkan seluruh jaringan satu kantor cabang ke jaringan kantor pusat. VPN client ada di gateway/router cabang, bukan di perangkat individual.

**Remote Access VPN** — satu perangkat individual konek ke VPN server. Umum dipakai karyawan yang kerja dari rumah atau pengguna yang ingin bypass pembatasan geografis.

### 10.3 Kegunaan

1. Hubungkan kantor cabang ke kantor pusat secara aman
2. Bypass pembatasan geografis — IP terlihat dari lokasi VPN server, bukan lokasi asli
3. Sembunyikan traffic dari ISP — ISP hanya lihat traffic terenkripsi

### 10.4 Peringatan Penting

- Tidak semua VPN merutekan semua traffic melalui tunnel — cek dengan **DNS leak test**
- Beberapa VPN server membocorkan IP asli meski seharusnya menyembunyikannya
- Beberapa negara menganggap penggunaan VPN **ilegal** — periksa hukum lokal sebelum menggunakan, terutama saat bepergian

```bash
sudo openvpn file.ovpn    # konek ke VPN server menggunakan file konfigurasi
```

---

## 11. URL Encoding

URL encoding adalah standar penggantian karakter spesial di URL dengan format `%` diikuti dua karakter hexadecimal. Sering ditemui saat membaca isi HTTP request di Wireshark.

| Kode | Karakter |
|---|---|
| %20 | spasi |
| %40 | @ |
| %7B | { |
| %7D | } |
| %3A | : |
| %2F | / |

Untuk decode gunakan **URL decoder**, bukan Base64 decoder.

---

## 12. Glossary

**Cleartext** — data yang dikirim tanpa enkripsi, bisa dibaca siapapun yang menangkap paketnya.

**Promiscuous Mode** — mode network card yang menangkap semua paket di jaringan, termasuk yang tidak ditujukan ke perangkat tersebut.

**TLS Handshake** — proses negosiasi antara klien dan server untuk menyepakati cipher suite, bertukar sertifikat, dan membangun sesi enkripsi sebelum data dikirim.

**Cipher** — algoritma matematika untuk mengenkripsi dan mendekripsi data.

**Cipher Suite** — kombinasi beberapa algoritma (enkripsi + hashing) yang dipakai TLS dalam satu sesi.

**CSR (Certificate Signing Request)** — dokumen yang dibuat admin server berisi informasi identitas server, dikirim ke CA untuk mendapat sertifikat digital.

**CA (Certificate Authority)** — pihak ketiga yang memverifikasi identitas server dan menerbitkan sertifikat digital yang dipercaya browser.

**Self-signed Certificate** — sertifikat yang ditandatangani sendiri tanpa CA; tidak bisa membuktikan keaslian server ke pihak luar.

**STARTTLS** — metode upgrade koneksi dari plaintext ke TLS di tengah sesi; koneksi mulai tidak terenkripsi lalu di-upgrade. Berbeda dari TLS langsung dari awal.

**VPN Tunnel** — jalur terenkripsi virtual yang dibuat VPN di atas koneksi internet biasa.

**X11 Forwarding** — fitur SSH yang memungkinkan tampilan grafis aplikasi dari mesin remote diteruskan dan ditampilkan di mesin lokal.

**ssl-key.log** — file berisi session key TLS yang ditulis browser; digunakan Wireshark untuk mendekripsi traffic HTTPS.

**DNS Leak** — kondisi di mana query DNS bocor keluar dari tunnel VPN sehingga mengungkap aktivitas browsing atau IP asli pengguna.

**Promiscuous Mode** — mode network interface yang menangkap semua paket di segmen jaringan, bukan hanya yang ditujukan ke MAC address sendiri.

---

## 13. Tools & Platform Rujukan

**Wireshark** — tool analisis paket jaringan; dipakai untuk capture dan membaca traffic, termasuk mendekripsi HTTPS dengan ssl-key.log. URL: https://www.wireshark.org

**OpenSSL** — library dan tool kriptografi; dipakai untuk konek manual ke protokol berbasis TLS via `openssl s_client`. Sudah tersedia di sebagian besar distribusi Linux.

**OpenVPN** — implementasi open-source VPN; dipakai untuk konek ke VPN server menggunakan file konfigurasi `.ovpn`.

**Let's Encrypt** — CA gratis untuk mendapatkan sertifikat TLS tanpa biaya tahunan. URL: https://letsencrypt.org

**urldecoder.org** — tool online untuk decode URL encoding (%7B, %40, dll) ke karakter aslinya.

---

## 14. Catatan Ringkas untuk Ditulis Tangan

### Masalah Protokol Lama
- Cleartext — data terkirim tanpa enkripsi, siapapun bisa baca
- Promiscuous mode — tangkap semua paket di jaringan
- 3 hal tidak terlindungi: Confidentiality, Integrity, Authenticity

### 3 Pendekatan Keamanan Jaringan
- TLS — enkripsi protokol yang sudah ada
- SSH — ganti TELNET
- VPN — jaringan privat di atas internet

### SSL/TLS
- SSL 2.0 — Netscape, 1995
- TLS 1.0 — IETF, 1999
- TLS 1.3 — 2018, terbaru
- Posisi: Transport Layer OSI
- CSR → CA → Sertifikat Digital
- Self-signed — tidak bisa buktikan keaslian
- Let's Encrypt — CA gratis

### Cipher Suite
- Cipher — algoritma enkripsi/dekripsi
- Cipher suite — kombinasi cipher + hashing
- Contoh: TLS_AES_256_GCM_SHA384
- Negosiasi: Client Hello (daftar) → Server Hello (pilih satu)

### Port Wajib Hapal
- HTTP 80 → HTTPS 443
- SMTP 25 → SMTPS 465/587
- POP3 110 → POP3S 995
- IMAP 143 → IMAPS 993
- FTP 21 → FTPS 990
- TELNET 23 → SSH 22
- Port 587 pakai STARTTLS (mulai plaintext, upgrade ke TLS)

### HTTPS Urutan Koneksi
- TCP Handshake (3 paket) → TLS Negotiation (5 paket) → HTTP data
- Total 8 paket sebelum data aplikasi dikirim
- Di Wireshark tanpa kunci → Application Data (gibberish)

### Dekripsi HTTPS di Wireshark
- Buat ssl-key.log dulu saat browsing
- chromium --ssl-key-log-file=~/ssl-key.log
- SSLKEYLOGFILE=~/ssl-key.log firefox
- Klik kanan TLSv1.3 → Protocol Preferences → TLS → Browse ssl-key.log → OK

### SSH Command
- ssh username@hostname — login SSH
- ssh hostname — login dengan user yang sama
- ssh ip -X — aktifkan X11 Forwarding (GUI remote)
- -X — enable X11 Forwarding

### OpenSSL s_client
- openssl s_client -connect ip:port — konek manual ke protokol TLS
- -starttls smtp/imap/pop3 — upgrade dari plaintext ke TLS
- Telnet TIDAK bisa ke protokol TLS

### SFTP vs FTPS
- SFTP — bagian SSH, port 22, tidak butuh sertifikat
- FTPS — FTP + TLS, port 990, butuh sertifikat TLS
- sftp user@host — konek SFTP
- get filename — download
- put filename — upload

### VPN
- V=Virtual (tidak perlu kabel), P=Private (data terlindungi)
- Site-to-site: hubungkan jaringan kantor
- Remote access: satu perangkat individual
- IP asli tersembunyi → server lihat IP VPN server
- sudo openvpn file.ovpn — konek ke VPN
- Cek DNS leak test jika ragu VPN bekerja benar
- Beberapa negara: VPN ilegal, cek hukum lokal

### URL Encoding
- %20 = spasi, %40 = @, %7B = {, %7D = }, %3A = :, %2F = /
- Decode pakai URL decoder (bukan Base64)
