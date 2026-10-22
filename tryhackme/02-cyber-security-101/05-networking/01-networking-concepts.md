# Resume Materi: Networking Concepts
**TryHackMe — Networking Concepts**
**Tanggal:** 26 April 2026

---

## 1. OSI Model

### 1.1 Gambaran Umum

**OSI (Open Systems Interconnection)** adalah model konseptual yang dikembangkan oleh **ISO (International Organization for Standardization)** untuk mendefinisikan kerangka kerja komunikasi dalam jaringan komputer. Model ini bersifat teoretis, namun penting dipahami karena menjadi bahasa universal dalam dunia networking — istilah seperti "layer 3 switch" atau "layer 7 firewall" tidak akan dipahami tanpa mengetahui model ini.

OSI terdiri dari **7 layer**, dinomori dari bawah (fisik) ke atas (aplikasi).

Mnemonik untuk mengingat urutan layer 1 ke 7:
**"Please Do Not Throw Spinach Pizza Away"**

### 1.2 Tujuh Layer OSI

| Layer | Nama | Fungsi Utama | Protokol / Contoh |
|---|---|---|---|
| 7 | Application | Layanan langsung ke aplikasi end-user | HTTP, FTP, DNS, POP3, SMTP, IMAP |
| 6 | Presentation | Encoding, enkripsi, kompresi data | Unicode, MIME, JPEG, PNG, MPEG |
| 5 | Session | Membangun, menjaga, menyinkronkan sesi | NFS, RPC |
| 4 | Transport | Komunikasi end-to-end antar aplikasi | TCP, UDP |
| 3 | Network | Logical addressing dan routing antar jaringan | IP, ICMP, IPSec |
| 2 | Data Link | Transfer data antar node dalam segmen yang sama | Ethernet (802.3), WiFi (802.11) |
| 1 | Physical | Media transmisi fisik (kabel, sinyal) | Sinyal elektrik, optik, nirkabel |

### 1.3 Nama Unit Data per Layer

Setiap layer punya sebutan berbeda untuk unit data yang diproses. Ini wajib dihapal karena sering muncul di soal maupun praktik nyata.

- Layer 7 → **Application Data**
- Layer 4 via TCP → **Segment**
- Layer 4 via UDP → **Datagram**
- Layer 3 → **Packet**
- Layer 2 → **Frame**

### 1.4 Detail Layer yang Perlu Dipahami Lebih Dalam

**Physical Layer (Layer 1)**
Menangani koneksi fisik antar perangkat: kabel, sinyal, dan definisi binary 0 dan 1. Transmisi bisa berupa sinyal elektrik, optik, atau nirkabel. Contoh medium: kabel Ethernet, kabel serat optik, WiFi band 2.4 GHz / 5 GHz / 6 GHz.

**Data Link Layer (Layer 2)**
Mengatur komunikasi antar perangkat dalam satu **network segment** (sekelompok perangkat yang terhubung via medium bersama). Setiap perangkat di layer ini memiliki **MAC address** — alamat fisik sepanjang 6 byte (48 bit), ditulis dalam format heksadesimal dengan pemisah titik dua, contoh: `a4:c3:f0:85:ac:2d`.

- 3 byte kiri = identitas **vendor** (pembuat perangkat)
- 3 byte kanan = **alamat unik** perangkat

Dalam setiap frame Ethernet/WiFi selalu ada dua MAC address: **destination** dan **source**.

**Network Layer (Layer 3)**
Menangani pengiriman data **antar jaringan yang berbeda** (bukan dalam satu segmen seperti layer 2). Bertanggung jawab untuk logical addressing (IP address) dan routing — menentukan jalur terbaik bagi paket untuk sampai ke tujuan.

**Transport Layer (Layer 4)**
Memungkinkan komunikasi end-to-end antara aplikasi di host yang berbeda. Mendukung flow control, segmentation, dan error correction. Mengidentifikasi proses menggunakan **port numbers** (1–65535, port 0 dicadangkan).

**Session Layer (Layer 5)**
Membangun, mempertahankan, dan menyinkronkan sesi komunikasi. Menyediakan mekanisme pemulihan jika terjadi kegagalan transmisi di tengah jalan.

**Presentation Layer (Layer 6)**
Memastikan data disampaikan dalam format yang dapat dipahami application layer. Menangani encoding (misal ASCII/Unicode), kompresi, dan enkripsi. Contoh penggunaan nyata: format gambar JPEG/PNG saat dikirim via email menggunakan MIME agar bisa dilampirkan sebagai file.

**Application Layer (Layer 7)**
Menyediakan layanan jaringan langsung ke aplikasi pengguna. Web browser menggunakan HTTP untuk meminta halaman web. Protokol yang umum ditemui: HTTP, FTP, DNS, POP3, SMTP, IMAP, SSH, Telnet.

---

## 2. TCP/IP Model

### 2.1 Latar Belakang

**TCP/IP (Transmission Control Protocol/Internet Protocol)** adalah model yang benar-benar diimplementasikan di jaringan nyata — berbeda dengan OSI yang bersifat konseptual. Dikembangkan pada tahun 1970-an oleh **Department of Defense (DoD)** Amerika Serikat dengan tujuan utama: jaringan tetap berfungsi meskipun sebagian infrastrukturnya rusak atau diserang. Ketahanan ini dimungkinkan oleh desain protokol routing-nya yang adaptif terhadap perubahan topologi.

### 2.2 Perbandingan TCP/IP vs OSI

TCP/IP versi standar (RFC 1122) memiliki 4 layer. Layer 5, 6, 7 pada OSI digabung menjadi satu Application Layer.

| Layer OSI | TCP/IP (RFC 1122) | Protokol |
|---|---|---|
| 7, 6, 5 | Application Layer | HTTP, HTTPS, FTP, POP3, SMTP, IMAP, Telnet, SSH |
| 4 | Transport Layer | TCP, UDP |
| 3 | Internet Layer | IP, ICMP, IPSec |
| 2 | Link Layer | Ethernet 802.3, WiFi 802.11 |
| 1 | (tidak ada di RFC 1122) | — |

Buku teks modern (seperti *Computer Networking: A Top-Down Approach* — Kurose & Ross) menampilkan TCP/IP sebagai **5 layer** dengan menambahkan Physical Layer: Application, Transport, Network, Link, Physical.

---

## 3. IP Address dan Subnet

### 3.1 Struktur IPv4

Setiap perangkat dalam jaringan membutuhkan identitas unik agar bisa ditemukan dan dikomunikasikan. Dalam TCP/IP, identitas ini adalah **IP address**.

IPv4 terdiri dari **4 oktet × 8 bit = 32 bit total**. Setiap oktet bernilai antara 0–255.

Contoh: `192.168.1.1` → terdiri dari Oktet#1.Oktet#2.Oktet#3.Oktet#4

**Alamat yang dicadangkan dalam setiap subnet:**
- Angka **0** di oktet terakhir → **Network address** (contoh: 192.168.1.0)
- Angka **255** di oktet terakhir → **Broadcast address** (contoh: 192.168.1.255)
- Broadcast address berarti mengirim ke **semua host** dalam jaringan tersebut

Total alamat IPv4 yang tersedia: sekitar **4 miliar (2³²)** — sudah hampir habis, sehingga muncul IPv6 (128 bit).

### 3.2 Subnet Mask dan Notasi CIDR

**Subnet mask** mendefinisikan berapa bit dari IP address yang "dikunci" (tidak berubah) di seluruh jaringan tersebut.

- `255.255.255.0` = **/24** → 24 bit pertama dikunci → tiga oktet pertama sama di seluruh subnet
- Dengan /24, range alamat yang tersedia: **x.x.x.1** sampai **x.x.x.254**

Contoh: IP `192.168.66.89/24`
- Network address: `192.168.66.0`
- Broadcast address: `192.168.66.255`
- Range host: `192.168.66.1` – `192.168.66.254`

### 3.3 Melihat Konfigurasi Jaringan

```bash
# Windows
ipconfig

# Linux/UNIX
ifconfig
ip address show    # atau disingkat: ip a s
```

Output `ifconfig` menampilkan: IP address (`inet`), subnet mask (`netmask`), broadcast address (`broadcast`), dan MAC address (`ether`).

Output `ip a s` menampilkan IP dalam notasi CIDR (contoh: `192.168.66.89/24`).

### 3.4 Private vs Public IP Address

Ditetapkan oleh **RFC 1918** — standar global yang sudah baku dan tidak berubah.

**Range Private IP (wajib dihapal):**

| Range | Notasi CIDR |
|---|---|
| 10.0.0.0 – 10.255.255.255 | 10/8 |
| 172.16.0.0 – 172.31.255.255 | 172.16/12 |
| 192.168.0.0 – 192.168.255.255 | 192.168/16 |

Cara cepat identifikasi:
- Diawali `10.x.x.x` → pasti private
- Diawali `172.16–31.x.x` → pasti private
- Diawali `192.168.x.x` → pasti private
- Selain itu → public

**Private IP** tidak bisa dijangkau dari Internet secara langsung. Agar bisa mengakses Internet, router harus memiliki public IP dan mendukung **NAT (Network Address Translation)**.

**Alamat IP spesial lainnya:**
- `127.0.0.1` → **Loopback / Localhost** — merujuk pada komputer itu sendiri
- `0.0.0.0` → semua alamat / tidak ada alamat spesifik
- `255.255.255.255` → broadcast ke semua perangkat di jaringan

### 3.5 Routing

**Router** meneruskan paket data ke jaringan yang tepat. Bekerja di **layer 3**, memeriksa IP destination address lalu memilih jalur (link) terbaik berikutnya agar paket semakin mendekati tujuan. Sebuah paket biasanya melewati **beberapa router** sebelum sampai ke tujuan akhirnya.

---

## 4. UDP dan TCP

### 4.1 Port Numbers

IP address mengidentifikasi **host**, sedangkan **port number** mengidentifikasi **proses** spesifik di host tersebut. Port menggunakan dua oktet sehingga nilainya berkisar **1–65535** (2¹⁶ − 1). Port 0 dicadangkan.

### 4.2 UDP (User Datagram Protocol)

UDP adalah protokol **connectionless** di layer 4 — tidak perlu membangun koneksi sebelum mengirim data, dan tidak menyediakan mekanisme konfirmasi apakah paket sudah diterima.

Karakteristik UDP:
- Tidak ada jaminan pengiriman
- Tidak ada urutan paket
- Kecepatan lebih tinggi dibanding TCP
- Cocok untuk: streaming video/audio, gaming online, DNS, VoIP

Unit data UDP disebut **Datagram**.

### 4.3 TCP (Transmission Control Protocol)

TCP adalah protokol **connection-oriented** di layer 4 — wajib membangun koneksi terlebih dahulu sebelum data dapat dikirim. Setiap oktet data memiliki **sequence number** sehingga penerima bisa mendeteksi paket yang hilang atau terduplikasi.

Karakteristik TCP:
- Ada konfirmasi penerimaan (ACK)
- Data terkirim dalam urutan yang benar
- Ada mekanisme error correction dan retransmission
- Cocok untuk: web browsing, email, transfer file

Unit data TCP disebut **Segment**.

### 4.4 Three-Way Handshake

Proses pembangunan koneksi TCP menggunakan dua flag: **SYN (Synchronise)** dan **ACK (Acknowledgment)**.

1. **SYN** — Client mengirim paket SYN ke server, berisi sequence number acak milik client
2. **SYN-ACK** — Server merespons dengan SYN-ACK, menambahkan sequence number acak milik server
3. **ACK** — Client mengirim ACK untuk mengkonfirmasi penerimaan SYN-ACK → koneksi terbentuk, transfer data bisa dimulai

### 4.5 Perbandingan UDP vs TCP

| Aspek | UDP | TCP |
|---|---|---|
| Koneksi | Connectionless | Connection-oriented |
| Konfirmasi | Tidak ada | Ada (ACK) |
| Urutan data | Tidak dijamin | Dijamin |
| Kecepatan | Lebih cepat | Lebih lambat |
| Unit data | Datagram | Segment |
| Contoh penggunaan | Streaming, gaming, DNS | Web, email, file transfer |

---

## 5. Encapsulation

### 5.1 Konsep

**Encapsulation** adalah proses di mana setiap layer menambahkan **header** (dan kadang **trailer**) ke unit data yang diterima dari layer di atasnya, sebelum meneruskannya ke layer di bawahnya. Proses ini memungkinkan setiap layer fokus pada fungsinya sendiri tanpa perlu memahami isi data dari layer lain.

### 5.2 Proses Encapsulation (Saat Mengirim)

```
Application Data
     ↓  [Transport Layer tambah TCP/UDP Header]
TCP Segment / UDP Datagram
     ↓  [Network Layer tambah IP Header]
IP Packet
     ↓  [Link Layer tambah Ethernet/WiFi Header + Trailer]
Frame
     ↓  dikirim melalui medium fisik
```

Secara visual berlapis:

```
[ Ethernet/WiFi Header | IP Header | TCP/UDP Header | Application Data | Ethernet/WiFi Trailer ]
```

### 5.3 De-encapsulation (Saat Menerima)

Proses kebalikannya terjadi di sisi penerima — setiap layer melepas header miliknya dan meneruskan data ke layer di atasnya, hingga application data berhasil diekstrak.

### 5.4 Kehidupan Sebuah Paket (Studi Kasus)

Skenario: membuka TryHackMe dari browser.

1. User mengetik query pencarian → browser menyiapkan **HTTP request** (Application Layer)
2. **TCP** membangun koneksi via three-way handshake dengan server TryHackMe, lalu membungkus HTTP request menjadi **TCP segment**
3. **IP layer** menambahkan source IP (komputermu) dan destination IP (server TryHackMe) → menjadi **IP packet**
4. **Link layer** menambahkan header dan trailer → menjadi **Frame**, dikirim ke router
5. **Router** melepas frame header, membaca IP destination, lalu meneruskan ke link berikutnya — proses ini berulang di setiap router hingga paket sampai ke jaringan tujuan
6. Di sisi penerima, proses de-encapsulation berjalan terbalik hingga server mendapat HTTP request asli

---

## 6. Telnet

### 6.1 Konsep

**TELNET (Teletype Network)** adalah protokol jaringan untuk koneksi terminal jarak jauh. `telnet` sebagai client memungkinkan pengguna terhubung ke sistem remote dan mengirim perintah teks. Karena tidak terenkripsi, Telnet dianggap tidak aman untuk penggunaan produksi — namun sangat berguna untuk menguji koneksi ke port TCP tertentu secara manual.

### 6.2 Penggunaan Dasar

```bash
telnet [IP_ADDRESS] [PORT]
```

### 6.3 Server yang Diuji dan Port-nya

| Port | Server | Perilaku |
|---|---|---|
| 7 | Echo server | Memantulkan kembali semua input yang dikirim |
| 13 | Daytime server | Membalas dengan tanggal & waktu saat ini, lalu menutup koneksi |
| 80 | HTTP/Web server | Melayani halaman web via permintaan HTTP |

**Catatan penting:** Echo server dan daytime server dianggap sebagai **risiko keamanan** dan seharusnya tidak dijalankan di lingkungan produksi.

### 6.4 Cara Request Halaman Web via Telnet (Port 80)

```
telnet [IP_ADDRESS] 80
```

Setelah terhubung, ketik:

```
GET / HTTP/1.1
Host: [hostname]

```

Perhatian: `GET` harus **huruf kapital semua** — HTTP bersifat case-sensitive. Setelah `Host:`, tekan **Enter dua kali** (baris kosong menandai akhir request). Jika berhasil, server akan merespons dengan `HTTP/1.1 200 OK`.

### 6.5 Cara Keluar dari Sesi Telnet

```
CTRL + ]        # masuk ke mode kontrol telnet (escape)
quit            # ketik quit di prompt telnet> untuk menutup koneksi
```

Penting: mengetik `quit` saat masih dalam sesi aktif tidak akan keluar — teks tersebut akan dikirim ke server (dan di-echo kembali jika di echo server). Harus tekan `CTRL + ]` terlebih dahulu.

---

## 7. Glossary — Istilah Penting

**OSI** — Open Systems Interconnection; model konseptual 7 layer untuk komunikasi jaringan

**TCP/IP** — Transmission Control Protocol/Internet Protocol; model jaringan yang diimplementasikan di dunia nyata

**MAC Address** — Media Access Control address; alamat fisik perangkat jaringan, panjang 6 byte, unik per perangkat

**Network Segment** — Sekelompok perangkat yang terhubung via medium bersama dalam satu jaringan lokal

**Octet** — 8 bit = 1 byte; nilai 0–255; IP address terdiri dari 4 oktet

**Subnet** — Pembagian jaringan besar menjadi jaringan-jaringan yang lebih kecil

**Subnet Mask** — Nilai yang menentukan berapa bit dari IP address yang merupakan bagian jaringan (network portion)

**CIDR** — Classless Inter-Domain Routing; notasi ringkas untuk subnet mask, contoh `/24`

**RFC 1918** — Dokumen standar yang menetapkan tiga range IP address untuk penggunaan private

**NAT** — Network Address Translation; teknologi yang memungkinkan perangkat dengan private IP mengakses Internet melalui satu public IP di router

**SYN** — Synchronise; flag TCP yang digunakan untuk memulai koneksi

**ACK** — Acknowledgment; flag TCP yang digunakan untuk mengkonfirmasi penerimaan data atau paket

**Three-Way Handshake** — Proses tiga langkah (SYN → SYN-ACK → ACK) untuk membangun koneksi TCP

**Loopback** — Alamat `127.0.0.1` yang merujuk pada komputer itu sendiri; digunakan untuk testing lokal

**Broadcast** — Pengiriman data ke semua host dalam satu jaringan; alamat broadcast selalu x.x.x.255

**Encapsulation** — Proses setiap layer menambahkan header ke data yang diterima sebelum meneruskan ke layer bawahnya

**De-encapsulation** — Proses kebalikan encapsulation; setiap layer melepas header miliknya saat menerima data

**Frame** — Unit data di layer 2 (Data Link); hasil enkapsulasi oleh Ethernet atau WiFi

**Packet** — Unit data di layer 3 (Network); IP packet

**Segment** — Unit data TCP di layer 4

**Datagram** — Unit data UDP di layer 4

**Port Number** — Angka 1–65535 yang mengidentifikasi proses spesifik di sebuah host; digunakan oleh TCP dan UDP

**Routing** — Proses menentukan jalur terbaik untuk meneruskan paket dari sumber ke tujuan antar jaringan yang berbeda

**TELNET** — Teletype Network; protokol untuk koneksi terminal jarak jauh via TCP; tidak terenkripsi

---

## 8. Tools & Platform Rujukan

**Wireshark**
Aplikasi untuk menangkap dan menganalisis paket jaringan secara real-time. Digunakan dalam materi untuk menampilkan isi frame Ethernet termasuk MAC address source dan destination. URL: https://www.wireshark.org

**telnet** (command line tool)
Tersedia di sebagian besar sistem operasi. Digunakan untuk terhubung ke port TCP secara manual dan mengirim perintah teks raw. Di Linux: biasanya perlu diinstall via package manager (`apt install telnet`).

---

## 9. Catatan Ringkas untuk Ditulis Tangan

### OSI Model — 7 Layer (bawah ke atas)

- Layer 1 Physical — kabel, sinyal elektrik/optik/wireless
- Layer 2 Data Link — komunikasi dalam segmen, MAC address, Frame
- Layer 3 Network — routing antar jaringan, IP address, Packet
- Layer 4 Transport — end-to-end, TCP/UDP, port, Segment/Datagram
- Layer 5 Session — bangun & jaga sesi, NFS, RPC
- Layer 6 Presentation — encoding, enkripsi, kompresi, MIME, JPEG
- Layer 7 Application — layanan ke user, HTTP, FTP, DNS, SMTP
- Mnemonik 1→7: Please Do Not Throw Spinach Pizza Away

### TCP/IP Model

- 4 layer: Application (OSI 5,6,7) / Transport / Internet (=Network) / Link (=Data Link)
- Versi 5 layer: tambah Physical
- Dibuat DoD 1970-an — jaringan tetap fungsi meski sebagian rusak

### IP Address

- 4 oktet × 8 bit = 32 bit, tiap oktet 0–255
- Oktet terakhir 0 = network address, 255 = broadcast address
- /24 = subnet mask 255.255.255.0 = 24 bit dikunci
- Total IPv4 ≈ 4 miliar (2³²)

### Private IP — RFC 1918 (HAPAL!)

- 10.0.0.0 – 10.255.255.255 (10/8)
- 172.16.0.0 – 172.31.255.255 (172.16/12)
- 192.168.0.0 – 192.168.255.255 (192.168/16)
- 127.0.0.1 = loopback/localhost

### Command Cek IP

- Windows: ipconfig
- Linux: ifconfig | ip a s

### UDP vs TCP

- UDP — connectionless, tanpa konfirmasi, cepat, unit: Datagram
- TCP — connection-oriented, ada ACK, reliable, unit: Segment
- Port range: 1–65535, port 0 dicadangkan

### Three-Way Handshake TCP

- SYN → client ke server (sequence number client)
- SYN-ACK → server ke client (sequence number server)
- ACK → client ke server → koneksi terbentuk

### Encapsulation (atas ke bawah saat kirim)

- Application Data
- + TCP/UDP Header = Segment / Datagram
- + IP Header = Packet
- + Ethernet/WiFi Header + Trailer = Frame
- De-encapsulation = kebalikannya saat terima

### MAC Address

- 6 byte, format hex: a4:c3:f0:85:ac:2d
- 3 byte kiri = vendor, 3 byte kanan = alamat unik
- Di setiap frame ada 2 MAC: destination + source

### Telnet

- telnet [IP] [PORT] — sambung ke TCP port
- Port 7 = echo server (mantul balik input)
- Port 13 = daytime server (balas waktu, lalu tutup)
- Port 80 = HTTP server
- Request HTTP: GET / HTTP/1.1 (kapital semua!) lalu Host: [nama] lalu Enter 2x
- Keluar: CTRL + ] lalu ketik quit
