# Resume Materi: Course 3, Module 3 - Network Intrusion Tactics dan Serangan DoS/DDoS

Tanggal: 21 Oktober 2026

Materi mencakup tujuan mengamankan jaringan, taktik intrusi jaringan (packet sniffing dan IP spoofing), serta serangan Denial of Service. Bagian berikutnya dari course membahas security hardening.


## 1. Gambaran Umum

Tujuan modul ini adalah memahami cara mengamankan jaringan secara efektif. Caranya dengan mengenali jenis-jenis serangan jaringan dan cara bertahan terhadapnya, karena pemahaman ini menjadi dasar pekerjaan seorang security analyst.

Tiga kelompok serangan yang dibahas:

- **Packet sniffing**: menangkap dan memeriksa data packets di jaringan.
- **IP spoofing**: memalsukan source IP untuk menyamar sebagai sistem yang sah.
- **DoS dan DDoS**: membanjiri jaringan atau server dengan traffic hingga tidak dapat melayani pengguna sah.


## 2. Packet Sniffing

### 2.1 Definisi

**Packet sniffing** adalah praktik menangkap dan memeriksa data packets yang melintasi jaringan. Dalam konteks serangan (malicious packet sniffing), penyerang mencegat packet untuk memperoleh akses tidak sah ke informasi di dalamnya.

### 2.2 Passive Packet Sniffing

Aktor jahat terhubung ke **network hub** dan melihat semua traffic di jaringan. Penyerang hanya mengamati, tidak mengubah isi packet. Dampaknya: informasi yang masuk dan keluar dari perangkat target dapat dilihat.

### 2.3 Active Packet Sniffing

**Active packet sniffing** adalah serangan ketika data packets dimanipulasi saat dalam perjalanan (in transit). Contoh dampaknya: packet dapat dialihkan ke port yang tidak dimaksudkan.

### 2.4 Pencegahan

Salah satu langkah yang disebut di materi: perusahaan sebaiknya menghindari penggunaan **unprotected Wi-Fi** untuk membantu melindungi diri dari packet sniffing.


## 3. IP Spoofing

### 3.1 Definisi

**IP spoofing** adalah serangan ketika penyerang mengubah **source IP** dari sebuah data packet untuk menyamar sebagai sistem yang sah (authorized) dan mendapatkan akses ke jaringan.

### 3.2 Serangan IP Spoofing yang Umum

Tiga serangan yang termasuk IP spoofing menurut materi:

- **On-path attack**: aktor jahat menempatkan dirinya di tengah koneksi yang sah dan mencegat atau mengubah data in transit. Contoh skenario: penyerang masuk ke jaringan, berada di antara web browser dan web server, lalu men-sniff packet untuk mengetahui IP address dan MAC address perangkat.
- **Replay attack**: aktor jahat mencegat sebuah data packet in transit, lalu menunda atau mengulanginya di waktu lain.
- **Smurf attack**: penyerang men-sniff IP address pengguna yang sah, lalu membanjirinya dengan paket ICMP.

**KRACK attacks** bukan termasuk serangan IP spoofing yang umum menurut materi.


## 4. Denial of Service (DoS) dan Distributed Denial of Service (DDoS)

### 4.1 DoS

**DoS attack** menargetkan jaringan atau server dan membanjirinya dengan network traffic. Hasilnya, target kewalahan, bisa crash, dan operasional bisnis normal tidak dapat dilanjutkan. Serangan dianggap berhasil jika ada bagian jaringan yang overloaded.

### 4.2 DDoS

**DDoS attack** adalah DoS yang menggunakan banyak perangkat atau server di lokasi berbeda untuk membanjiri jaringan target dengan traffic yang tidak diinginkan. Menggunakan banyak server di lokasi berbeda adalah ciri DDoS, bukan DoS.

### 4.3 Botnet

**Botnet** adalah kumpulan komputer yang terinfeksi malware dan berada di bawah kendali satu threat actor yang disebut **bot-herder**.

### 4.4 ICMP

**Internet Control Message Protocol (ICMP)** adalah protokol internet yang dipakai perangkat untuk saling memberi tahu tentang kesalahan transmisi data di jaringan. Protokol ini menjadi dasar tiga serangan di bawah (ICMP flood, ping of death, smurf).

### 4.5 Contoh Serangan DoS

- **ICMP flood**: penyerang berulang kali mengirim paket ICMP request ke server jaringan.
- **SYN flood**: mensimulasikan koneksi TCP/IP dan membanjiri server dengan paket SYN. Indikatornya: server berhenti merespons setelah menerima jumlah paket SYN masuk yang sangat tinggi.
- **Ping of death**: hacker mem-ping sistem dengan paket ICMP berukuran terlalu besar, yaitu lebih dari 64KB. Ukuran maksimum paket ICMP IPv4 yang benar adalah 64KB.


## 5. Perbandingan Singkat

| Serangan | Cara kerja inti | Kategori |
|---|---|---|
| Passive packet sniffing | Mengamati traffic tanpa mengubah | Packet sniffing |
| Active packet sniffing | Memanipulasi packet in transit | Packet sniffing |
| On-path attack | Berada di tengah koneksi, mencegat atau mengubah data | IP spoofing |
| Replay attack | Menunda atau mengulang packet yang dicegat | IP spoofing |
| Smurf attack | Men-sniff IP sah, membanjiri dengan ICMP | IP spoofing |
| ICMP flood | Mengirim ICMP request berulang | DoS |
| SYN flood | Membanjiri server dengan SYN packets | DoS |
| Ping of death | Mengirim ICMP lebih dari 64KB | DoS |


## 6. Pembeda Penting (dari Module Challenge)

- Passive hanya melihat; yang mengubah isi packet adalah active.
- DoS dan DDoS sama-sama bertujuan membuat target kewalahan; DDoS memakai banyak sumber.
- Server berhenti merespons akibat lonjakan packet SYN menandakan SYN flood, bukan ICMP flood.
- Paket ICMP berukuran terlalu besar menandakan ping of death.
- Pencegat yang menunda packet menandakan replay attack.
- Penyerang di antara browser dan web server yang men-sniff IP dan MAC address menandakan on-path attack.
- On-path, replay, dan smurf dikelompokkan sebagai IP spoofing.


## 7. Glossary

- **Active packet sniffing**: data packets dimanipulasi saat in transit.
- **Botnet**: kumpulan komputer terinfeksi malware di bawah kendali satu threat actor (bot-herder).
- **Denial of service (DoS) attack**: membanjiri jaringan atau server dengan network traffic.
- **Distributed denial of service (DDoS) attack**: DoS memakai banyak perangkat atau server di lokasi berbeda.
- **Internet Control Message Protocol (ICMP)**: protokol untuk melaporkan kesalahan transmisi data antar perangkat.
- **ICMP flood**: DoS dengan pengiriman ICMP request berulang ke server jaringan.
- **IP spoofing**: mengubah source IP packet untuk menyamar sebagai sistem sah.
- **On-path attack**: aktor jahat di tengah koneksi sah, mencegat atau mengubah data in transit.
- **Packet sniffing**: menangkap dan memeriksa data packets di jaringan.
- **Passive packet sniffing**: terhubung ke network hub dan melihat semua traffic.
- **Ping of death**: DoS dengan paket ICMP lebih dari 64KB.
- **Replay attack**: mencegat packet in transit lalu menunda atau mengulanginya di waktu lain.
- **Smurf attack**: men-sniff IP pengguna sah lalu membanjirinya dengan paket ICMP.
- **SYN flood attack**: DoS yang mensimulasikan koneksi TCP/IP dan membanjiri server dengan paket SYN.


## 8. Tools & Platform Rujukan

Materi modul ini tidak menyebutkan tools atau platform eksternal tertentu, sehingga tidak ada yang dicantumkan.


## 9. Catatan Ringkas untuk Ditulis Tangan

### Tujuan
- Amankan jaringan - kenali serangan + cara bertahan
- Berikutnya: security hardening

### Packet sniffing
- Packet sniffing - tangkap + periksa data packets
- Passive - konek ke network hub, lihat semua traffic, tidak mengubah
- Active - packet dimanipulasi in transit, bisa dialihkan ke port tak dimaksud
- Pencegahan - hindari unprotected Wi-Fi

### IP spoofing
- IP spoofing - ubah source IP, menyamar jadi sistem sah
- On-path - di tengah koneksi, cegat/ubah data (browser - web server, sniff IP + MAC)
- Replay - cegat packet, tunda/ulang nanti
- Smurf - sniff IP sah, banjiri paket ICMP
- KRACK - bukan IP spoofing umum

### DoS dan DDoS
- DoS - banjiri jaringan/server dengan traffic, target crash
- DDoS - DoS dari banyak perangkat/server beda lokasi
- Botnet - komputer terinfeksi malware, dikontrol bot-herder
- ICMP - protokol lapor error transmisi data
- ICMP flood - ICMP request berulang ke server
- SYN flood - simulasi koneksi TCP/IP, banjir SYN, server berhenti merespons
- Ping of death - paket ICMP lebih dari 64KB (normal maks 64KB IPv4)

### Command dan tools
- Tidak ada command, switch, atau tools eksternal di modul ini
