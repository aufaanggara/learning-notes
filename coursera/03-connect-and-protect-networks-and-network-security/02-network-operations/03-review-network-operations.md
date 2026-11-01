# Resume Materi: Network Operations (Course 3, Module 2) - 1 November 2026

Resume ini mencakup protokol jaringan, VPN, firewall, security zone, segmentasi jaringan, dan proxy server. Materi ini tidak memuat command atau tools eksternal, sehingga fokusnya pada konsep dan istilah.


## 1. Gambaran Umum

Network operations adalah topik luas yang melibatkan berbagai tools, protokol, dan teknik agar jaringan berjalan lancar dan aman. Materi modul ini terbagi tiga: protokol jaringan, VPN untuk menjaga privasi di jaringan publik, dan infrastruktur pengaman jaringan (firewall, security zone, proxy server).

Pemahaman ini dipakai di peran apa pun sebagai security analyst.


## 2. Protokol Jaringan

### 2.1 Definisi

**Network protocols** adalah sekumpulan aturan yang digunakan dua perangkat atau lebih dalam jaringan untuk menentukan urutan pengiriman data dan struktur data.

### 2.2 Protokol Inti

**Transmission Control Protocol (TCP)** adalah protokol komunikasi internet yang memungkinkan dua perangkat membentuk koneksi dan melakukan streaming data.

**Address Resolution Protocol (ARP)** adalah protokol yang dipakai untuk menentukan alamat MAC dari router atau perangkat berikutnya di sepanjang jalur data.

**Domain Name System (DNS)** adalah protokol yang menerjemahkan nama domain internet menjadi alamat IP.

**Hypertext Transfer Protocol (HTTP)** adalah protokol lapisan aplikasi yang menyediakan metode komunikasi antara client dan server website.

**Hypertext Transfer Protocol Secure (HTTPS)** adalah protokol yang menyediakan metode komunikasi aman antara client dan server.

### 2.3 Protokol Pendukung

| Protokol | Fungsi |
|---|---|
| **SFTP** (Secure File Transfer Protocol) | Transfer file antar perangkat melalui jaringan secara aman |
| **SSH** (Secure Shell) | Membuat shell dengan sistem remote secara aman |
| **SNMP** (Simple Network Management Protocol) | Memantau dan mengelola perangkat di jaringan |

### 2.4 Standar dan Keamanan Nirkabel

**IEEE 802.11 (Wi-Fi)** adalah sekumpulan standar yang mendefinisikan komunikasi untuk wireless LAN. IEEE adalah pihak yang memelihara standar Wi-Fi.

**Wi-Fi Protected Access (WPA)** adalah protokol keamanan nirkabel agar perangkat dapat terhubung ke internet. WPA berbeda dari IEEE 802.11: yang pertama adalah protokol keamanan, yang kedua adalah standar komunikasi.


## 3. Virtual Private Network (VPN)

### 3.1 Definisi dan Tujuan

**VPN** adalah layanan keamanan jaringan yang mengubah alamat IP publik dan menyamarkan lokasi virtual pengguna, sehingga data tetap privat saat memakai jaringan publik seperti internet.

### 3.2 Cara VPN Menjaga Kerahasiaan

VPN menjaga **confidentiality** dengan **mengenkripsi data saat dalam perjalanan (data in transit)**. Fungsi memantau lalu lintas atau menerjemahkan domain bukan tugas VPN.

### 3.3 Encapsulation

**Encapsulation** adalah proses yang dilakukan layanan VPN untuk melindungi data dengan membungkus data sensitif di dalam paket data lain. Mekanisme inilah yang dipakai VPN untuk mentransfer data terenkripsi antara perangkat dan server VPN.

> *Seperti memasukkan surat rahasia ke dalam amplop, lalu amplop itu dimasukkan ke dalam kotak paket biasa. Orang luar hanya melihat kotak paketnya.*


## 4. Firewall

### 4.1 Definisi

**Firewall** adalah perangkat keamanan jaringan yang memantau lalu lintas yang masuk ke atau keluar dari jaringan.

### 4.2 Klasifikasi Berdasarkan Cara Kerja

**Stateful** adalah kelas firewall yang melacak informasi yang melewatinya dan secara proaktif menyaring ancaman.

**Stateless** adalah kelas firewall yang bekerja berdasarkan aturan yang sudah ditentukan dan tidak melacak informasi dari paket data.

### 4.3 Klasifikasi Berdasarkan Lokasi dan Kemampuan

**Cloud-based firewalls** adalah firewall berbasis software yang dihosting oleh penyedia layanan cloud.

**Next-generation firewall (NGFW)** adalah firewall yang mampu melakukan deep packet inspection dan intrusion detection.

### 4.4 Port Filtering

**Port filtering** adalah fungsi firewall yang memblokir atau mengizinkan nomor port tertentu untuk membatasi komunikasi yang tidak diinginkan.

### 4.5 Pembeda yang Sering Diuji

Soal yang menanyakan firewall yang menganalisis lalu lintas untuk karakteristik dan perilaku mencurigakan lalu menghentikannya sebelum masuk jaringan menjawab **Stateful**, sesuai definisi "proaktif menyaring ancaman". Sebaliknya, soal yang menyebut **deep packet inspection dan intrusion detection** menjawab **NGFW**. Perhatikan kata kunci di soal untuk membedakan keduanya.


## 5. Segmentasi Jaringan dan Security Zone

### 5.1 Network Segmentation dan Subnetting

**Network segmentation** adalah teknik keamanan yang membagi jaringan menjadi segmen-segmen.

**Subnetting** adalah pembagian jaringan menjadi kelompok logis yang disebut subnet.

### 5.2 Security Zone

**Security zone** adalah segmen jaringan perusahaan yang melindungi jaringan internal dari internet.

| Zona | Definisi |
|---|---|
| **Uncontrolled zone** | Bagian jaringan di luar organisasi |
| **Controlled zone** | Subnet yang melindungi jaringan internal dari uncontrolled zone |
| **Demilitarized zone (DMZ)** | Zona berisi layanan publik: web server, proxy server, DNS server |

Perlu dibedakan: segmentasi jaringan adalah teknik pembagian, sedangkan fungsi proxy bukan membagi jaringan menjadi segmen.


## 6. Proxy Server

### 6.1 Definisi

**Proxy server** adalah server yang memenuhi permintaan clientnya dengan meneruskannya ke server lain.

### 6.2 Fungsi Utama Proxy Server

- Menentukan apakah permintaan untuk terhubung ke sebuah website diizinkan.
- Menggunakan alamat IP publik yang berbeda dari perangkat lain di jaringan privat.
- Menyimpan sementara data yang sering diminta oleh server eksternal.

### 6.3 Forward Proxy Server

**Forward proxy server** mengatur dan membatasi akses seseorang ke internet. Ia menerima lalu lintas keluar dari karyawan, menyetujuinya, lalu meneruskannya ke tujuan di internet.

### 6.4 Reverse Proxy Server

**Reverse proxy server** mengatur dan membatasi akses internet ke server internal. Fungsinya cocok untuk melindungi web server internal yang berisi data rahasia.

> *Forward proxy seperti resepsionis yang mengatur karyawan yang hendak keluar. Reverse proxy seperti satpam di pintu depan yang menyaring tamu dari luar.*

### 6.5 Jebakan Soal

Pernyataan bahwa "baik forward maupun reverse proxy menambah lapisan perlindungan dari internet" tidak tepat bila soal menanyakan khusus cara kerja forward proxy. Melindungi web server internal adalah fungsi reverse proxy.


## 7. Glossary

| Istilah | Definisi |
|---|---|
| **ARP** | Menentukan alamat MAC router atau perangkat berikutnya di jalur |
| **Cloud-based firewalls** | Software firewall yang dihosting penyedia cloud |
| **Controlled zone** | Subnet pelindung jaringan internal dari uncontrolled zone |
| **DNS** | Menerjemahkan nama domain menjadi alamat IP |
| **Encapsulation** | Membungkus data sensitif dalam paket lain oleh VPN |
| **Firewall** | Memantau lalu lintas ke atau dari jaringan |
| **Forward proxy server** | Mengatur dan membatasi akses pengguna ke internet |
| **HTTP** | Protokol lapisan aplikasi client dan server website |
| **HTTPS** | Versi aman komunikasi client dan server |
| **IEEE 802.11 (Wi-Fi)** | Standar komunikasi wireless LAN |
| **Network protocols** | Aturan urutan pengiriman dan struktur data |
| **Network segmentation** | Teknik membagi jaringan menjadi segmen |
| **NGFW** | Firewall dengan deep packet inspection dan intrusion detection |
| **Port filtering** | Blokir atau izinkan nomor port tertentu |
| **Proxy server** | Meneruskan permintaan client ke server lain |
| **Reverse proxy server** | Mengatur akses internet ke server internal |
| **SFTP** | Transfer file aman antar perangkat |
| **SSH** | Membuat shell dengan sistem remote |
| **Security zone** | Segmen jaringan perusahaan pelindung jaringan internal dari internet |
| **SNMP** | Memantau dan mengelola perangkat jaringan |
| **Stateful** | Firewall yang melacak informasi dan menyaring ancaman proaktif |
| **Stateless** | Firewall berbasis aturan tetap tanpa pelacakan |
| **Subnetting** | Pembagian jaringan menjadi subnet |
| **TCP** | Membentuk koneksi dan streaming data antar perangkat |
| **Uncontrolled zone** | Bagian jaringan di luar organisasi |
| **VPN** | Mengubah IP publik dan menyamarkan lokasi virtual |
| **WPA** | Protokol keamanan nirkabel untuk koneksi ke internet |
| **DMZ** | Zona layanan publik (web, proxy, DNS server) |


## 8. Tools & Platform Rujukan

Materi modul ini tidak menyebutkan tools atau platform eksternal untuk dirujuk, sehingga tidak ada entri pada section ini.


## 9. Catatan Ringkas untuk Ditulis Tangan

### Protokol

- Network protocols — aturan urutan dan struktur data antar perangkat
- TCP — koneksi dan streaming data antar dua perangkat
- ARP — cari MAC perangkat atau router berikutnya
- DNS — domain menjadi IP
- HTTP — protokol lapisan aplikasi client dan server web
- HTTPS — HTTP versi aman
- SFTP — transfer file aman
- SSH — shell ke sistem remote
- SNMP — pantau dan kelola perangkat jaringan
- IEEE 802.11 — standar wireless LAN (Wi-Fi)
- WPA — keamanan nirkabel

### VPN

- VPN — ubah IP publik, samarkan lokasi, data privat di jaringan publik
- Menjaga confidentiality — enkripsi data in transit
- Encapsulation — bungkus data sensitif dalam paket lain, dipakai untuk transfer data terenkripsi ke server VPN

### Firewall

- Firewall — pantau lalu lintas masuk dan keluar
- Stateful — lacak informasi, saring ancaman proaktif
- Stateless — aturan tetap, tanpa pelacakan paket
- Cloud-based — software firewall dihosting penyedia cloud
- NGFW — deep packet inspection dan intrusion detection
- Port filtering — blokir atau izinkan nomor port

### Segmentasi dan Zona

- Network segmentation — bagi jaringan jadi segmen
- Subnetting — bagi jaringan jadi subnet logis
- Security zone — segmen pelindung jaringan internal dari internet
- Uncontrolled zone — di luar organisasi
- Controlled zone — subnet pelindung internal dari uncontrolled zone
- DMZ — layanan publik (web, proxy, DNS server)

### Proxy

- Proxy server — teruskan permintaan client ke server lain
- Fungsi proxy — izinkan atau tolak koneksi, IP publik berbeda, cache data
- Forward proxy — atur akses keluar pengguna ke internet, setujui lalu teruskan
- Reverse proxy — atur akses internet ke server internal, lindungi web server rahasia
- Beda: forward melindungi pengguna keluar, reverse melindungi server masuk

### Command

- Tidak ada command atau switch pada modul ini
