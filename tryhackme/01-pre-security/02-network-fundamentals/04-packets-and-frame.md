# Resume Materi — Networking: Packets, Frames & Protocols
**TryHackMe | Pre-Security Path**
**Tanggal: 01 September 2026**

---

## 1. Packet dan Frame

### 1.1 Definisi dan Perbedaan

**Packet** adalah potongan data yang beroperasi di **Layer 3 (Network Layer)** OSI model. Ia membawa IP header dan payload — sehingga setiap kali membicarakan IP address, konteksnya adalah packet.

**Frame** beroperasi di **Layer 2 (Data Link Layer)** OSI model. Frame membungkus packet dan menambahkan informasi seperti MAC address. Ketika encapsulating information dilepas, yang tersisa adalah frame itu sendiri.

Keduanya bukan hal yang sama: packet adalah isi, frame adalah pembungkusnya.

### 1.2 Encapsulation dan Decapsulation

**Encapsulation** adalah proses membungkus packet menjadi frame saat data bergerak turun melewati layer OSI.

**Decapsulation** adalah kebalikannya — membuka lapisan frame untuk mengekstrak packet di sisi penerima.

### 1.3 Mengapa Packet Efisien

Data tidak dikirim sekaligus dalam satu blok besar, melainkan dipecah menjadi potongan-potongan kecil (packet). Ini mengurangi risiko **bottleneck** di jaringan. Contoh nyata: ketika browser memuat gambar, gambar tersebut dikirim dalam beberapa packet terpisah dan direkonstruksi ulang di sisi penerima.

### 1.4 IP Packet Headers

| Header | Fungsi |
|---|---|
| **Time to Live (TTL)** | Timer kedaluwarsa packet — mencegah packet menyumbat jaringan jika tidak pernah sampai tujuan |
| **Checksum** | Nilai kalkulasi matematika untuk verifikasi integritas data — jika nilainya berbeda dari yang diharapkan, data dianggap corrupt |
| **Source Address** | IP address pengirim, agar data tahu ke mana harus kembali |
| **Destination Address** | IP address penerima, agar data tahu ke mana harus pergi |

---

## 2. TCP — Transmission Control Protocol

### 2.1 Karakteristik Utama

TCP adalah protokol yang bersifat **connection-based**: koneksi harus dibangun terlebih dahulu antara client dan server sebelum data apapun dikirim. Karena itu, TCP **menjamin** bahwa data yang dikirim akan diterima di ujung yang lain.

TCP/IP model terdiri dari **4 layer** (versi ringkas dari OSI model):

1. Application
2. Transport
3. Internet
4. Network Interface

Informasi ditambahkan ke setiap layer saat packet melewatinya (encapsulation), dan proses sebaliknya disebut decapsulation.

### 2.2 Kelebihan dan Kekurangan TCP

| Kelebihan | Kekurangan |
|---|---|
| Menjamin integritas data | Membutuhkan koneksi yang andal — jika satu chunk gagal, seluruh chunk harus dikirim ulang |
| Mampu sinkronisasi dua perangkat agar tidak saling flood data dalam urutan salah | Koneksi lambat menyebabkan bottleneck karena koneksi direservasi terus-menerus |
| Melakukan banyak proses untuk menjamin reliability | Jauh lebih lambat dari UDP karena komputasi yang lebih berat |

### 2.3 TCP Headers

| Header | Fungsi |
|---|---|
| **Source Port** | Port pengirim, dipilih secara acak dari range 0–65535 yang belum dipakai |
| **Destination Port** | Port tujuan, tidak dipilih acak — mengikuti standar protokol (misal: port 80 untuk HTTP) |
| **Source IP** | IP address perangkat pengirim |
| **Destination IP** | IP address perangkat tujuan |
| **Sequence Number** | Nomor acak yang diberikan ke potongan data pertama saat koneksi dimulai |
| **Acknowledgement Number** | Sequence number + 1, digunakan untuk potongan data berikutnya |
| **Checksum** | Kalkulasi matematika untuk memastikan integritas TCP — hasil berbeda berarti data corrupt |
| **Data** | Tempat bytes file yang sedang dikirimkan disimpan |
| **Flag** | Menentukan cara packet ditangani selama handshake (SYN, ACK, FIN, RST, dll) |

---

## 3. Three-Way Handshake

### 3.1 Pengertian

**Three-Way Handshake** adalah proses yang digunakan TCP untuk membangun koneksi antara dua perangkat sebelum data dikirim. Proses ini menggunakan beberapa pesan khusus berbentuk flag.

### 3.2 Pesan-Pesan dalam Handshake

| Step | Message | Pengirim | Keterangan |
|---|---|---|---|
| 1 | **SYN** | Client → Server | Memulai koneksi dan mensinkronisasi dua perangkat |
| 2 | **SYN/ACK** | Server → Client | Server mengakui upaya sinkronisasi dari client |
| 3 | **ACK** | Client → Server | Client mengakui balasan server — koneksi terbentuk |
| 4 | **DATA** | Client → Server | Data aktual mulai dikirim |
| 5 | **FIN** | Siapapun | Menutup koneksi secara bersih setelah selesai |
| # | **RST** | Siapapun | Mengakhiri koneksi secara paksa — dipakai saat ada masalah serius (error, low resource, service tidak berfungsi) |

Diagram alur normal:

```
Alice ──── SYN ────────────→ Bob
Alice ←─── SYN/ACK ─────────  Bob
Alice ──── ACK ────────────→ Bob
[koneksi terbentuk — data mulai mengalir]
```

### 3.3 Sequence Number dan ISN

Setiap data yang dikirim diberi **nomor urut acak (Initial Sequence Number / ISN)**. Setiap potongan data berikutnya menggunakan ISN + 1. Kedua perangkat harus menyepakati urutan ini agar data dapat direkonstruksi dalam urutan yang benar.

Proses kesepakatan ISN dalam tiga langkah:

1. **SYN** — Client: *"ISN saya = 0, ayo sinkronisasi"*
2. **SYN/ACK** — Server: *"ISN saya = 5000, saya acknowledge ISN kamu (0)"*
3. **ACK** — Client: *"Saya acknowledge ISN kamu (5000), data saya dimulai dari ISN+1 = 1"*

Contoh urutan nomor:

| Device | ISN | Final Number |
|---|---|---|
| Client (Sender) | 0 | 0 + 1 = 1 |
| Client (Sender) | 1 | 1 + 1 = 2 |
| Client (Sender) | 2 | 2 + 1 = 3 |

### 3.4 Menutup Koneksi TCP

TCP akan menutup koneksi setelah memastikan semua data telah diterima. Karena TCP mereservasi resource sistem, **menutup koneksi sesegera mungkin adalah best practice**.

Proses penutupan dimulai dengan pengiriman packet **FIN**, dan perangkat lain harus mengakuinya:

```
Alice ──── FIN ────────────→ Bob    (Alice minta tutup)
Alice ←─── ACK ─────────────  Bob    (Bob akui)
Alice ←─── FIN ─────────────  Bob    (Bob juga minta tutup)
Alice ──── ACK ────────────→ Bob    (Alice akui — selesai)
```

---

## 4. Stealth Scan (Half-Open Scan)

### 4.1 Konteks

Stealth scan adalah teknik scanning port menggunakan **Nmap** yang tidak menyelesaikan Three-Way Handshake secara penuh, sehingga koneksi tidak benar-benar terbentuk dan kemungkinan besar tidak tercatat di log server.

### 4.2 Perbandingan TCP Scan vs Stealth Scan

| Aspek | `-sT` (TCP Connect Scan) | `-sS` (Stealth / Half-Open Scan) |
|---|---|---|
| Handshake | Penuh (SYN → SYN/ACK → ACK) | Setengah (SYN → SYN/ACK → RST) |
| Koneksi terbentuk | Ya | Tidak |
| Tercatat di log | Ya | Kemungkinan tidak |
| Kecepatan | Lebih lambat | Lebih cepat |
| Kemungkinan terdeteksi | Tinggi | Lebih rendah |

Diagram stealth scan:

```
Attacker ──── SYN ──────────→ Target
Attacker ←─── SYN/ACK ───────  Target
Attacker ──── RST ──────────→ Target   ← batalkan, tidak jadi konek
```

Setelah menerima SYN/ACK (yang mengonfirmasi port terbuka), attacker langsung mengirim RST untuk memutus koneksi sebelum selesai.

---

## 5. UDP — User Datagram Protocol

### 5.1 Karakteristik Utama

UDP adalah protokol yang bersifat **stateless**: tidak memerlukan koneksi konstan antara dua perangkat. Three-Way Handshake tidak terjadi, tidak ada sinkronisasi, dan tidak ada acknowledgement.

UDP cocok digunakan saat aplikasi dapat **mentoleransi kehilangan data**, seperti video streaming atau voice chat, atau saat koneksi tidak stabil bukan masalah besar.

### 5.2 Kelebihan dan Kekurangan UDP

| Kelebihan | Kekurangan |
|---|---|
| Jauh lebih cepat dari TCP | Tidak peduli apakah data diterima atau tidak |
| Kontrol kecepatan kirim diserahkan ke aplikasi (user software) | Koneksi tidak stabil menghasilkan pengalaman yang sangat buruk bagi user |
| Tidak mereservasi koneksi secara terus-menerus | Tidak ada safeguard seperti data integrity check |

Tidak ada proses apapun dalam membangun koneksi — tidak ada perhatian terhadap apakah data diterima, dan tidak ada perlindungan seperti yang ditawarkan TCP.

### 5.3 UDP Headers

UDP packets lebih sederhana dari TCP dan memiliki lebih sedikit header. Beberapa header dibagi bersama dengan TCP:

| Header | Fungsi |
|---|---|
| **Time to Live (TTL)** | Timer kedaluwarsa packet — mencegah packet menyumbat jaringan |
| **Source Address** | IP pengirim, agar data tahu cara kembali |
| **Destination Address** | IP tujuan, agar data tahu ke mana pergi |
| **Source Port** | Port pengirim, dipilih secara acak dari 0–65535 |
| **Destination Port** | Port tujuan, tidak dipilih acak (misal: port 80 untuk web server) |
| **Data** | Bytes file yang sedang dikirimkan |

### 5.4 Alur Koneksi UDP

Karena stateless, tidak ada acknowledgement selama koneksi. Diagram:

```
Bob  ──── REQUEST ──────────→ Alice
Bob  ←─── RESPONSE ──────────  Alice
Bob  ←─── RESPONSE ──────────  Alice   ← terus kirim tanpa
Bob  ←─── RESPONSE ──────────  Alice     tunggu konfirmasi
```

---

## 6. Perbandingan TCP vs UDP

| Aspek | TCP | UDP |
|---|---|---|
| Jenis | Connection-based | Stateless |
| Three-Way Handshake | Ada | Tidak ada |
| Jaminan data diterima | Ya | Tidak |
| Acknowledgement | Ada | Tidak ada |
| Integritas data | Ada (checksum) | Tidak ada |
| Kecepatan | Lambat | Cepat |
| Reservasi resource | Ya | Tidak |
| Cocok untuk | File transfer, web browsing | Video streaming, voice chat, gaming |

---

## 7. Port

### 7.1 Pengertian

**Port** adalah titik numerik di mana data dipertukarkan antara perangkat jaringan. Setiap data yang dikirim atau diterima melewati port tertentu. Nilainya berkisar antara **0 hingga 65535**.

Port berfungsi seperti aturan di pelabuhan: hanya trafik yang kompatibel yang boleh masuk di port tertentu. Analogi: kapal pesiar tidak bisa berlabuh di dermaga yang dirancang untuk kapal nelayan.

### 7.2 Standardisasi Port

Karena jumlah port sangat banyak (0–65535), aplikasi dan protokol diasosiasikan dengan **standar port tertentu** agar komunikasi antar perangkat konsisten. Contoh: semua browser mengirim data web melalui port 80 — sehingga Chrome dan Firefox bisa menginterpretasikan data dengan cara yang sama.

Port dalam range **0 hingga 1024** disebut **common ports** (port umum/standar).

### 7.3 Common Ports yang Wajib Dihapal

| Port | Protokol | Kepanjangan | Fungsi |
|---|---|---|---|
| 21 | **FTP** | File Transfer Protocol | Transfer file dari/ke server dalam model client-server |
| 22 | **SSH** | Secure Shell | Login aman ke sistem via antarmuka teks (text-based) |
| 80 | **HTTP** | HyperText Transfer Protocol | Menggerakkan World Wide Web — download teks, gambar, video |
| 443 | **HTTPS** | HTTP Secure | Sama seperti HTTP tapi terenkripsi |
| 445 | **SMB** | Server Message Block | Transfer file dan berbagi perangkat (misal: printer) di jaringan |
| 3389 | **RDP** | Remote Desktop Protocol | Login ke sistem via antarmuka desktop visual (berbeda dari SSH yang berbasis teks) |

### 7.4 Catatan Penting soal Standar Port

Standar port **bisa diubah** — misalnya menjalankan web server di port 8080 alih-alih 80. Namun aplikasi akan tetap menganggap standar yang diikuti, sehingga jika menggunakan port non-standar, **port harus ditulis eksplisit menggunakan tanda titik dua (:)**.

Contoh: `192.168.1.1:8080`

---

## 8. Glossary — Istilah Penting

**Packet** — Potongan data di Layer 3 (Network Layer) OSI, berisi IP header dan payload.

**Frame** — Potongan data di Layer 2 (Data Link Layer) OSI, membungkus packet dan menambahkan MAC address.

**Encapsulation** — Proses membungkus packet menjadi frame saat data turun melewati layer OSI.

**Decapsulation** — Proses membuka frame untuk mengekstrak packet di sisi penerima.

**TTL (Time to Live)** — Header yang menentukan batas waktu/hop sebuah packet sebelum dibuang, mencegah packet beredar selamanya di jaringan.

**Checksum** — Nilai kalkulasi matematika untuk memverifikasi integritas data — jika berbeda dari yang diharapkan, data dianggap corrupt.

**Three-Way Handshake** — Proses TCP untuk membangun koneksi antara dua perangkat melalui pertukaran pesan SYN, SYN/ACK, dan ACK.

**ISN (Initial Sequence Number)** — Nomor urut acak yang diberikan ke potongan data pertama saat koneksi TCP dibuka.

**Stateless** — Sifat protokol yang tidak menyimpan atau melacak status koneksi (UDP).

**Stateful** — Sifat protokol yang melacak dan mengingat status koneksi secara aktif (TCP).

**Bottleneck** — Kondisi kemacetan data di jaringan akibat kapasitas yang tidak seimbang.

**ACK (Acknowledgement)** — Konfirmasi bahwa sebuah packet atau rangkaian packet telah berhasil diterima.

**SYN** — Flag TCP untuk memulai koneksi dan mensinkronisasi dua perangkat.

**FIN** — Flag TCP untuk menutup koneksi secara bersih setelah komunikasi selesai.

**RST (Reset)** — Flag TCP untuk mengakhiri koneksi secara paksa — digunakan saat terjadi masalah serius atau dalam teknik stealth scan.

**Half-Open Scan** — Teknik scanning port (Nmap `-sS`) yang tidak menyelesaikan handshake, sehingga koneksi tidak terbentuk dan kemungkinan tidak tercatat di log.

**Common Port** — Port dalam range 0–1024 yang telah dialokasikan untuk protokol standar tertentu.

**Connection-based** — Sifat protokol yang mengharuskan koneksi dibangun terlebih dahulu sebelum data dikirim (TCP).

**Stateless** — Sifat protokol yang langsung mengirim data tanpa membangun koneksi terlebih dahulu (UDP).

---

## 9. Tools & Platform Rujukan

**Nmap** — Tool scanning jaringan untuk menemukan port terbuka, service yang berjalan, dan OS detection. Digunakan dalam konteks stealth scan (`-sS`) dan TCP connect scan (`-sT`). Tidak ada URL — diinstall langsung di sistem.

**Service Name and Transport Protocol Port Number Registry** — Registri resmi IANA yang memuat daftar lengkap semua protokol beserta nomor port standarnya.
URL: https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml

---

## 10. Catatan Ringkas untuk Ditulis Tangan

### Packet & Frame

- Packet — data Layer 3 (IP header + payload)
- Frame — data Layer 2 (packet + MAC address)
- Encapsulation — packet dibungkus jadi frame
- Decapsulation — frame dibuka, isi packet keluar
- Konteks IP address → bicara packet; encapsulating dilepas → bicara frame

### IP Packet Headers

- TTL — timer kedaluwarsa packet
- Checksum — cek integritas; beda nilai = corrupt
- Source Address — IP pengirim
- Destination Address — IP tujuan

### TCP

- Connection-based — koneksi dibangun DULU sebelum data dikirim
- Jamin data diterima ✅ | Lambat | Reservasi resource
- 4 layer: Application, Transport, Internet, Network Interface

### TCP Headers

- Source Port — acak (0–65535)
- Destination Port — tidak acak (ikut standar)
- Source IP / Destination IP
- Sequence Number — nomor acak, data pertama
- Acknowledgement Number — seq. number + 1
- Checksum — integritas TCP
- Data — bytes file
- Flag — SYN/ACK/FIN/RST (kontrol handshake)

### Three-Way Handshake

- SYN → client ke server (mulai koneksi)
- SYN/ACK → server ke client (akui)
- ACK → client ke server (konfirmasi, koneksi terbentuk)
- DATA → kirim data
- FIN → tutup bersih
- RST → tutup paksa (error/low resource)

### ISN (Initial Sequence Number)

- ISN = nomor acak untuk data pertama
- Berikutnya = ISN + 1
- SYN: "ISN saya = 0" | SYN/ACK: "ISN saya = 5000, ACK 0" | ACK: "ACK 5000, saya mulai dari 1"

### Menutup Koneksi TCP

- FIN → ACK → FIN → ACK (4 langkah)
- Tutup sesegera mungkin karena TCP reservasi resource

### Stealth Scan (Nmap)

- -sT — TCP Connect Scan (handshake penuh, tercatat di log)
- -sS — Stealth/Half-Open Scan (SYN → SYN/ACK → RST, tidak jadi konek)
- -sS lebih cepat, kemungkinan tidak masuk log server

### UDP

- Stateless — tidak butuh koneksi, tidak ada handshake
- Tidak peduli data diterima/tidak
- Lebih cepat dari TCP | Tidak reservasi resource
- Cocok: video streaming, voice chat

### UDP Headers

- TTL, Source Address, Destination Address
- Source Port (acak), Destination Port (tidak acak)
- Data

### TCP vs UDP

- TCP — connection-based, handshake, jamin terima, lambat
- UDP — stateless, no handshake, tidak jamin terima, cepat

### Port

- Range: 0–65535
- Common ports: 0–1024
- Non-standar → wajib tulis dengan colon (:) | contoh: 192.168.1.1:8080

### Common Ports (WAJIB HAPAL)

- 21 — FTP — transfer file client-server
- 22 — SSH — login aman via teks
- 80 — HTTP — web biasa (WWW)
- 443 — HTTPS — web terenkripsi
- 445 — SMB — share file + share device (printer)
- 3389 — RDP — login via tampilan desktop visual
