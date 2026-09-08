# Resume — Networking Fundamentals (Course 3, Module 1)

Tanggal: 17 Agustus 2026

---

## 1. Konsep Dasar Jaringan

Sebuah **network** adalah sekumpulan perangkat yang saling terhubung satu sama lain. Semua konsep lain di room ini (topologi, perangkat, protokol) pada dasarnya menjelaskan bagaimana kumpulan perangkat tersebut saling berkomunikasi.

### 1.1 Bandwidth dan Speed

**Bandwidth** adalah kapasitas maksimum transmisi data pada sebuah jaringan, diukur dalam bit per detik. Ini menggambarkan seberapa besar "jalur" yang tersedia untuk data mengalir.

**Speed** adalah kecepatan aktual sebuah perangkat dalam mengirim dan menerima data, juga diukur dalam bit per detik. Ini menggambarkan seberapa cepat data benar-benar bergerak melalui jalur tersebut.

Bandwidth besar tidak otomatis berarti speed tinggi — bandwidth adalah kapasitas, speed adalah kecepatan aktual pemakaiannya.

---

## 2. Klasifikasi Jaringan Berdasarkan Cakupan

### 2.1 Local Area Network (LAN)

Jaringan yang mencakup area kecil, seperti satu gedung kantor, satu sekolah, atau satu rumah. Perangkat-perangkatnya berada dalam jarak fisik yang dekat.

### 2.2 Wide Area Network (WAN)

Jaringan yang mencakup area geografis luas, seperti satu kota, satu negara bagian, atau satu negara. WAN pada dasarnya menghubungkan banyak LAN yang tersebar di lokasi berbeda menjadi satu jaringan besar.

Pembedaan ini menentukan jenis infrastruktur yang relevan — LAN cukup diatur dengan switch dan router lokal, sedangkan WAN membutuhkan infrastruktur routing yang jauh lebih kompleks.

---

## 3. Perangkat Jaringan Standar

| Perangkat | Fungsi |
|---|---|
| **Hub** | Menyiarkan (broadcast) data ke semua perangkat di jaringan tanpa mengenali tujuan spesifik |
| **Switch** | Mengarahkan data secara terarah hanya ke perangkat tujuan tertentu dalam jaringan lokal |
| **Router** | Menghubungkan beberapa jaringan berbeda menjadi satu, mengatur routing antar jaringan |
| **Modem** | Menghubungkan router ke internet, membawa akses internet masuk ke LAN |

Perbedaan kunci hub vs switch: hub tidak "pintar" (broadcast ke semua), switch tahu alamat tujuan sehingga data hanya dikirim ke perangkat yang relevan. Perbedaan kunci switch vs router: switch bekerja di dalam satu jaringan lokal, router menghubungkan jaringan yang berbeda.

---

## 4. Cloud Computing dan Cloud Network

### 4.1 Definisi

**Cloud computing** adalah praktik menggunakan server, aplikasi, dan layanan jaringan jarak jauh yang di-hosting di internet, sebagai pengganti perangkat fisik lokal.

**Cloud network** adalah kumpulan server atau komputer yang menyimpan sumber daya dan data di pusat data (data center) jarak jauh, dapat diakses melalui internet.

### 4.2 Alasan Penggunaan Cloud Service Provider (CSP)

- Menyediakan analitik bisnis untuk memantau traffic web dan penjualan.
- Menawarkan daya pemrosesan yang hanya dibayar sesuai pemakaian (pay-as-you-use), bukan biaya tetap infrastruktur.
- Server jarak jauh memungkinkan aplikasi web diakses dari lokasi mana pun.

Catatan penting: penggunaan CSP tidak menghilangkan kebutuhan langkah keamanan cloud tambahan — memindahkan infrastruktur ke cloud tetap membutuhkan pengamanan tersendiri.

### 4.3 Software Defined Networks (SDN)

Pendekatan jaringan yang dikelola secara terpusat melalui software, bukan dikonfigurasi manual per perangkat.

Cloud computing dan SDN sama-sama memberi dua manfaat utama: **penurunan biaya** (mengurangi kebutuhan investasi infrastruktur fisik) dan **peningkatan skalabilitas** (kapasitas jaringan lebih mudah disesuaikan naik-turun sesuai kebutuhan).

---

## 5. Model TCP/IP

**TCP/IP model** adalah framework untuk memvisualisasikan bagaimana data diorganisasi dan ditransmisikan melalui jaringan. Model ini jadi acuan penting bagi teknisi dan analis keamanan untuk mengidentifikasi dan mengomunikasikan di lapisan mana suatu masalah jaringan terjadi.

Model ini terdiri dari empat layer:

1. **Layer 1 — Network access**: menangani koneksi fisik antar perangkat.
2. **Layer 2 — Internet**: menangani pengalamatan dan routing antar jaringan.
3. **Layer 3 — Transport**: mengatur pengiriman data yang andal antar perangkat.
4. **Layer 4 — Application**: menentukan bagaimana data packet berinteraksi dengan perangkat penerima, termasuk untuk transfer file dan layanan email. Layer ini tempat protokol yang langsung dipakai aplikasi (FTP, HTTP, SMTP) beroperasi.

---

## 6. Model OSI

**Open Systems Interconnection (OSI) model** adalah konsep standar yang menjelaskan tujuh lapisan yang digunakan komputer untuk berkomunikasi dan mengirim data melalui jaringan. Lebih terperinci dibanding TCP/IP (empat layer), sering dipakai berdampingan sebagai kerangka konseptual komunikasi jaringan.

---

## 7. Data Packet

**Data packet** adalah unit dasar informasi yang berpindah dari satu perangkat ke perangkat lain di dalam jaringan. Terdiri dari dua bagian konseptual:

- **Header**: berisi informasi kontrol seperti alamat MAC, protokol yang digunakan, dan informasi lain untuk mengarahkan packet.
- **Body**: berisi pesan atau data aktual (payload) yang akan ditransmisikan ke perangkat penerima.

**Packet sniffing** adalah praktik menangkap dan memeriksa data packet yang melintas di sebuah jaringan. Relevan dari sisi keamanan karena bisa dipakai untuk troubleshooting sah maupun aktivitas mata-mata (eavesdropping) yang tidak sah.

---

## 8. Sistem Pengalamatan (Addressing)

### 8.1 MAC Address

Pengenal alfanumerik unik yang ditetapkan secara permanen pada setiap perangkat fisik dalam jaringan. Bersifat tetap pada level hardware.

### 8.2 IP Address

Rangkaian karakter unik yang mengidentifikasi lokasi sebuah perangkat di internet. Berbeda dari MAC address, IP address bisa berubah tergantung jaringan yang digunakan.

**Internet Protocol (IP)** sebagai konsep: sekumpulan standar untuk melakukan routing dan pengalamatan data packet saat berpindah antar perangkat dalam jaringan.

### 8.3 IPv4 vs IPv6

| Versi | Format | Contoh Pola |
|---|---|---|
| **IPv4** | Empat kelompok angka desimal dipisah titik | `169.254.255.249` |
| **IPv6** | Delapan kelompok karakter heksadesimal dipisah titik dua | `fd45:3efd:3201:ff22:0000:0000:12ff:0000` |

Cara cepat membedakan: pemisah titik (`.`) menandakan IPv4, pemisah titik dua (`:`) menandakan IPv6. IPv6 dikembangkan karena keterbatasan jumlah alamat unik yang bisa dihasilkan IPv4.

---

## 9. Port dan Protokol Transport

**Port** adalah lokasi berbasis software yang mengatur pengiriman dan penerimaan data antar perangkat dalam jaringan. Jika IP address mengarahkan data ke perangkat yang benar, port mengarahkan data ke aplikasi atau layanan yang benar di dalam perangkat itu.

Port yang dibahas secara spesifik: **port 20**, digunakan untuk transfer file berukuran besar (FTP data transfer).

### 9.1 TCP

**Transmission Control Protocol**: protokol connection-oriented, memungkinkan dua perangkat membentuk koneksi terlebih dahulu sebelum melakukan streaming data. Lebih andal karena ada proses verifikasi koneksi.

### 9.2 UDP

**User Datagram Protocol**: protokol connectionless, tidak membentuk koneksi antar perangkat sebelum transmisi data. Lebih cepat namun kurang andal dibanding TCP karena tanpa verifikasi koneksi terlebih dahulu.

---

## 10. Glosarium

- **Bandwidth** — kapasitas maksimum transmisi data pada sebuah jaringan (bit per detik).
- **Speed** — kecepatan aktual pengiriman/penerimaan data oleh perangkat (bit per detik).
- **Network** — sekumpulan perangkat yang saling terhubung.
- **LAN** — jaringan area kecil (gedung, sekolah, rumah).
- **WAN** — jaringan area geografis luas (kota, negara bagian, negara).
- **Hub** — perangkat yang menyiarkan data ke semua perangkat di jaringan.
- **Switch** — perangkat yang mengarahkan data hanya ke perangkat tujuan tertentu.
- **Router** — perangkat yang menghubungkan beberapa jaringan menjadi satu.
- **Modem** — perangkat penghubung router ke internet.
- **Cloud computing** — penggunaan server dan layanan jarak jauh berbasis internet.
- **Cloud network** — kumpulan server/komputer di data center jarak jauh yang diakses via internet.
- **CSP (Cloud Service Provider)** — penyedia layanan cloud.
- **SDN (Software Defined Networks)** — jaringan yang dikelola terpusat melalui software.
- **TCP/IP model** — framework empat layer untuk memvisualisasikan organisasi dan transmisi data jaringan.
- **OSI model** — konsep standar tujuh layer komunikasi jaringan.
- **Data packet** — unit dasar informasi yang berpindah antar perangkat.
- **Packet sniffing** — praktik menangkap dan memeriksa data packet di jaringan.
- **MAC address** — pengenal unik permanen tiap perangkat fisik.
- **IP address** — pengenal unik lokasi perangkat di internet.
- **IPv4** — format alamat IP dengan angka desimal dipisah titik.
- **IPv6** — format alamat IP dengan karakter heksadesimal dipisah titik dua.
- **Port** — lokasi software yang mengatur pengiriman/penerimaan data antar perangkat.
- **TCP** — protokol connection-oriented, membentuk koneksi sebelum transmisi.
- **UDP** — protokol connectionless, tanpa pembentukan koneksi sebelum transmisi.

---

## 11. Tools & Platform Rujukan

Tidak ada tools atau platform eksternal (seperti Talos, VirusTotal, Shodan, AbuseIPDB, dsb) yang disebutkan secara eksplisit di materi room ini. Room ini murni membahas konsep dasar jaringan (struktur, perangkat, model TCP/IP), belum masuk ke tahap penggunaan tools analisis keamanan.

---

## 12. Catatan Ringkas untuk Ditulis Tangan

**Dasar**
- Network — kumpulan perangkat terhubung
- Bandwidth — kapasitas maks transmisi (bit/detik)
- Speed — kecepatan aktual kirim/terima data (bit/detik)

**Klasifikasi Jaringan**
- LAN — area kecil (kantor, sekolah, rumah)
- WAN — area luas (kota, negara)

**Perangkat**
- Hub — broadcast ke semua perangkat
- Switch — kirim data terarah ke perangkat tujuan
- Router — hubungkan beberapa jaringan
- Modem — hubungkan router ke internet

**Cloud**
- Cloud computing — pakai server/layanan jarak jauh via internet
- Cloud network — server di data center jarak jauh, diakses via internet
- CSP — analitik bisnis, bayar sesuai pakai, akses dari mana saja; tetap butuh keamanan tambahan
- SDN — jaringan dikelola terpusat via software
- Manfaat cloud + SDN — biaya turun, skalabilitas naik

**TCP/IP Model (4 layer)**
- L1 Network access — koneksi fisik
- L2 Internet — pengalamatan & routing
- L3 Transport — pengiriman andal
- L4 Application — interaksi ke penerima (file transfer, email)

**OSI Model**
- 7 layer, standar komunikasi jaringan, lebih detail dari TCP/IP

**Data Packet**
- Header — info kontrol (MAC, protokol)
- Body — pesan/data aktual
- Packet sniffing — tangkap & periksa packet di jaringan

**Addressing**
- MAC address — ID unik permanen tiap perangkat fisik
- IP address — ID unik lokasi di internet, bisa berubah
- IPv4 — desimal, titik pemisah, contoh 169.254.255.249
- IPv6 — heksadesimal, titik dua pemisah, contoh fd45:3efd:...

**Port & Transport**
- Port — arahkan data ke aplikasi/layanan yang benar
- Port 20 — transfer file besar (FTP)
- TCP — connection-oriented, bentuk koneksi dulu, andal
- UDP — connectionless, tanpa koneksi dulu, cepat tapi kurang andal
