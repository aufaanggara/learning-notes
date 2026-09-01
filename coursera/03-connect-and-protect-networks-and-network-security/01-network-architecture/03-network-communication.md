# Resume: Network Communication & TCP/IP Model
17 Agustus 2026

---

## 1. Komunikasi Jaringan dan Data Packet

### 1.1 Konsep Dasar Komunikasi Jaringan

Komunikasi jaringan terjadi ketika data ditransfer dari satu titik ke titik lain. Kemampuan ini penting bagi organisasi untuk berbagi resource dan data agar bisa berfungsi secara efektif, namun di sisi lain juga membuka peluang serangan karena memberi malicious actor kesempatan memanfaatkan perangkat rentan dan jaringan yang tidak terlindungi.

Data yang berpindah antar perangkat dikirim dalam bentuk **data packet** — unit dasar informasi yang berisi tujuan, asal, dan isi pesan.

### 1.2 Struktur Data Packet

Data packet dianalogikan seperti surat fisik: amplop membawa alamat tujuan dan alamat pengirim, sedangkan isi surat adalah pesannya. Struktur packet terdiri dari tiga bagian:

- **Header** — berisi IP address dan MAC address tujuan, serta protocol number yang memberi tahu perangkat penerima apa yang harus dilakukan terhadap data.
- **Body** — berisi pesan/konten yang ditransmisikan.
- **Footer** — penanda akhir packet, berfungsi seperti tanda tangan pada surat, menandakan bahwa packet telah selesai.

### 1.3 Performa Jaringan dan Indikator Serangan

**Bandwidth** adalah jumlah data yang diterima perangkat per detik, dihitung dengan membagi jumlah data dengan waktu dalam detik. **Speed** adalah laju penerimaan atau pengunduhan data packet — konsep berbeda dari bandwidth namun saling berkaitan.

Irregularitas pada bandwidth dan speed bisa mengindikasikan adanya serangan pada jaringan. Untuk menyelidikinya, security personnel menggunakan **packet sniffing**, yaitu praktik menangkap dan menginspeksi data packet yang melintas di jaringan.

---

## 2. TCP/IP Model

### 2.1 Definisi

**TCP/IP model** adalah framework standar untuk memvisualisasikan bagaimana data diorganisasi dan ditransmisikan melintasi jaringan. Model ini membantu network engineer dan security analyst mengonseptualisasikan proses jaringan serta mengomunikasikan lokasi disruption atau security threat. TCP/IP model terdiri dari empat layer dan berbasis pada TCP/IP protocol suite.

### 2.2 TCP (Transmission Control Protocol)

TCP adalah protokol komunikasi internet yang memungkinkan dua perangkat membentuk koneksi dan melakukan streaming data. Fungsinya:

- Mengorganisasi data agar bisa dikirim melintasi jaringan.
- Membangun koneksi antar perangkat dan memastikan packet sampai ke tujuan yang tepat.
- Melakukan retransmit terhadap data yang hilang atau corrupt.
- Menyimpan nomor port tujuan pada TCP header.

### 2.3 IP (Internet Protocol)

IP mendefinisikan standar routing dan addressing data packet saat berpindah antar perangkat. IP mencakup **IP address**, yaitu alamat unik setiap private network. IP mengirim packet ke tujuan yang benar dan mengandalkan TCP/UDP untuk mengirimkannya ke service yang sesuai; packet di-routing dari jaringan pengirim ke jaringan penerima.

### 2.4 Port

**Port** adalah lokasi berbasis software dalam sistem operasi perangkat yang mengatur pengiriman dan penerimaan data. Port membagi network traffic ke dalam segmen berdasarkan service yang dijalankan, sehingga perangkat tahu cara memprioritaskan dan memproses data.

Analogi: seperti mengirim surat ke teman yang tinggal di apartemen — petugas pos tidak hanya tahu gedungnya, tapi juga tahu persis nomor apartemen tujuan di dalam gedung tersebut.

Contoh port umum:

- **Port 25** — email
- **Port 443** — komunikasi internet yang aman (secure internet communication)
- **Port 20** — transfer file berukuran besar

---

## 3. Empat Layer TCP/IP Model

### 3.1 Network Access Layer

Menangani pembuatan data packet dan transmisinya. Mencakup hardware fisik: hub, modem, cable, wiring, dan switch. **Address Resolution Protocol (ARP)** berada di layer ini, berfungsi memetakan IP address ke MAC address untuk komunikasi jaringan lokal.

Protokol yang beroperasi: **Ethernet**, **Wireless LAN**.

### 3.2 Internet Layer

Melekatkan IP address ke data packet untuk menunjukkan lokasi pengirim dan penerima, serta menentukan bagaimana jaringan saling terhubung — apakah packet tetap di LAN atau dikirim ke remote network seperti internet.

Protokol di layer ini:

- **Internet Protocol (IP)** — v4 dan v6, mengirim packet ke tujuan yang benar.
- **Internet Control Message Protocol (ICMP)** — membagikan informasi error dan status update packet; berguna mendeteksi packet yang drop, hilang saat transit, masalah konektivitas, dan packet yang di-redirect ke router lain.

### 3.3 Transport Layer

Mengontrol flow of traffic, mengizinkan/menolak komunikasi antar perangkat, serta melakukan error control agar data mengalir lancar.

Dua protokol utama:

- **TCP** — reliable, connection-oriented (lihat 2.2).
- **User Datagram Protocol (UDP)** — connectionless, tidak membangun koneksi sebelum transmisi, tidak melacak data seketat TCP, dipakai untuk aplikasi performance-sensitive dan real-time seperti video streaming.

### 3.4 Application Layer

Menentukan bagaimana data packet berinteraksi dengan perangkat penerima; mengatur fungsi seperti file transfer dan email service. Setara dengan gabungan application, presentation, dan session layer pada OSI model.

Protokol umum:

- **HTTP** — Hypertext Transfer Protocol
- **SMTP** — Simple Mail Transfer Protocol
- **SSH** — Secure Shell
- **FTP** — File Transfer Protocol
- **DNS** — Domain Name System

---

## 4. OSI Model

### 4.1 Definisi

**OSI (Open Systems Interconnection) model** adalah konsep terstandardisasi yang mendeskripsikan tujuh layer yang digunakan komputer untuk berkomunikasi dan mengirim data melalui jaringan. TCP/IP model merupakan bentuk yang dipadatkan (condensed form) dari OSI model.

Network dan security professional menggunakan OSI model untuk saling berkomunikasi mengenai potensi sumber masalah atau ancaman keamanan.

### 4.2 Tujuh Layer OSI (dari atas ke bawah)

**Layer 7 — Application Layer**
Mencakup proses yang melibatkan interaksi langsung pengguna sehari-hari. Semua network protocol yang digunakan software application untuk menghubungkan pengguna ke internet berada di sini. Contoh: web browser menggunakan HTTP/HTTPS, aplikasi email menggunakan SMTP, DNS menerjemahkan domain name menjadi IP address.

**Layer 6 — Presentation Layer**
Menangani data translation dan encryption. Menambahkan/mengganti data dengan format yang bisa dipahami aplikasi (layer 7) di sisi pengirim maupun penerima, karena format di sisi pengguna bisa berbeda dari sistem penerima sehingga butuh standardized format. Fungsi formatting meliputi encryption, compression, dan konfirmasi character code set. Contoh: **SSL**, mengenkripsi data antara web server dan browser pada website HTTPS.

**Layer 5 — Session Layer**
Mengatur session — kondisi ketika koneksi terbentuk antara dua perangkat sehingga bisa saling berkomunikasi. Protokol di layer ini menjaga session tetap terbuka selama transfer data dan mengakhirinya setelah transmisi selesai. Bertanggung jawab atas authentication, reconnection, dan checkpoint; jika session terganggu, checkpoint memastikan transmisi dilanjutkan dari titik terakhir. Merespons request dari presentation layer (layer 6) dan mengirim request ke transport layer (layer 4).

**Layer 4 — Transport Layer**
Mengirimkan data antar perangkat, menangani speed, flow, dan **segmentation** — proses membagi data transmission besar menjadi segmen kecil agar bisa diproses sistem penerima. Segmen harus disusun ulang (reassembled) di tujuan agar bisa diproses pada session layer. Kecepatan transmisi harus sesuai dengan connection speed sistem tujuan. Protokol: **TCP**, **UDP**.

**Layer 3 — Network Layer**
Mengawasi penerimaan frame dari data link layer dan mengirimkannya ke tujuan berdasarkan address dalam frame data packet. Data packet berisi IP address yang memberi tahu router ke mana harus dikirim; packet di-routing dari jaringan pengirim ke jaringan penerima.

**Layer 2 — Data Link Layer**
Mengorganisasi pengiriman dan penerimaan data packet dalam satu jaringan (single network). Tempat bagi switch pada jaringan lokal dan network interface card pada perangkat lokal. Protokol: **Network Control Protocol (NCP)**, **High-Level Data Link Control (HDLC)**, **Synchronous Data Link Control (SDLC)**.

**Layer 1 — Physical Layer**
Berkorespondensi dengan hardware fisik: hub, modem, cable, wiring. Untuk melintasi kabel ethernet/coaxial, data packet diterjemahkan menjadi stream 0 dan 1, dikirim melalui physical wiring, diterima, lalu diteruskan ke layer yang lebih tinggi.

### 4.3 Perbandingan TCP/IP Model vs OSI Model

| TCP/IP Model (4 layer) | OSI Model (7 layer) |
|---|---|
| Application Layer | Application, Presentation, Session |
| Transport Layer | Transport |
| Internet Layer | Network |
| Network Access Layer | Data Link, Physical |

Persamaan kedua model:

- Sama-sama mendefinisikan standar untuk networking dan membagi proses komunikasi jaringan ke dalam layer-layer berbeda.
- Sama-sama menggambarkan proses dan protokol jaringan untuk transmisi data antara dua sistem atau lebih.
- Sama-sama mencakup application layer dan transport layer.

Perbedaan utama: TCP/IP model memiliki 4 layer (versi sederhana), sedangkan OSI model memiliki 7 layer (lebih detail). Beberapa organisasi lebih mengandalkan TCP/IP model, sebagian lain lebih memilih OSI model — security analyst perlu familiar dengan keduanya.

---

## 5. Glossary

**Data packet** — unit dasar informasi yang dikirim antar perangkat dalam jaringan, berisi header, body, footer.

**Header** — bagian packet berisi IP address, MAC address tujuan, dan protocol number.

**Bandwidth** — jumlah data yang diterima perangkat per detik.

**Speed** — laju penerimaan/pengunduhan data packet.

**Packet sniffing** — praktik menangkap dan menginspeksi data packet di jaringan.

**TCP (Transmission Control Protocol)** — protokol connection-oriented yang membentuk koneksi dan menjamin pengiriman data reliable.

**IP (Internet Protocol)** — protokol untuk routing dan addressing data packet, mencakup IP address.

**UDP (User Datagram Protocol)** — protokol connectionless, dipakai untuk aplikasi real-time/performance-sensitive.

**Port** — lokasi software yang mengatur pengiriman/penerimaan data berdasarkan service.

**ARP (Address Resolution Protocol)** — memetakan IP address ke MAC address untuk komunikasi jaringan lokal.

**ICMP (Internet Control Message Protocol)** — melaporkan error dan status update packet.

**Segmentation** — proses memecah data transmission besar menjadi segmen kecil untuk transport.

**Session** — kondisi koneksi terbuka antara dua perangkat yang memungkinkan komunikasi.

**SSL** — protokol encryption pada presentation layer untuk website HTTPS.

**TCP/IP model** — framework 4 layer (network access, internet, transport, application) untuk memvisualisasikan organisasi dan transmisi data.

**OSI model** — framework 7 layer (physical, data link, network, transport, session, presentation, application) yang terstandardisasi untuk komunikasi jaringan.

---

## 6. Tools & Platform Rujukan

Tidak ada tools atau platform eksternal (seperti Talos, VirusTotal, Shodan, AbuseIPDB, dsb.) yang disebutkan dalam materi room ini. Room ini murni membahas konsep teori jaringan (data packet, TCP/IP model, OSI model) tanpa merujuk ke tools analisis eksternal.

---

## 7. Catatan Ringkas untuk Ditulis Tangan

**Data Packet**
- Data packet — unit info dasar antar perangkat
- Header — IP address, MAC address tujuan, protocol number
- Body — isi pesan
- Footer — penanda akhir packet
- Bandwidth — data diterima/detik
- Speed — laju terima/download data
- Packet sniffing — tangkap & inspeksi packet

**TCP/IP Dasar**
- TCP — bentuk koneksi, stream data, reliable, retransmit data hilang/corrupt
- IP — routing & addressing packet, punya IP address
- Port — lokasi software atur kirim/terima data
- Port 25 — email
- Port 443 — secure internet
- Port 20 — file besar

**4 Layer TCP/IP**
- Network Access Layer — hardware (hub, modem, cable, switch), ARP (IP→MAC lokal), protokol: Ethernet, Wireless LAN
- Internet Layer — attach IP address, protokol: IP (v4/v6), ICMP (error & status report)
- Transport Layer — flow control, error control, protokol: TCP, UDP
- Application Layer — interaksi ke receiving device, protokol: HTTP, SMTP, SSH, FTP, DNS

**7 Layer OSI (atas ke bawah)**
- L7 Application — interaksi user, HTTP/HTTPS, SMTP, DNS
- L6 Presentation — translation, encryption, compression; contoh SSL
- L5 Session — buka/tutup koneksi, authentication, reconnection, checkpoint
- L4 Transport — segmentation, reassembly, speed match, TCP/UDP
- L3 Network — terima frame dari L2, routing pakai IP address
- L2 Data Link — kirim/terima packet 1 jaringan, switch & NIC lokal, protokol: NCP, HDLC, SDLC
- L1 Physical — hardware fisik, data jadi stream 0/1

**TCP/IP vs OSI**
- TCP/IP = 4 layer (versi ringkas)
- OSI = 7 layer (versi detail)
- Sama: standar networking, layer-based, ada application & transport layer
- App layer TCP/IP = App+Presentation+Session OSI
- Internet layer TCP/IP = Network layer OSI
- Network access layer TCP/IP = Data Link+Physical OSI
