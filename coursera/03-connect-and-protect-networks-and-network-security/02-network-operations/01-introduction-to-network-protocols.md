# RESUME MATERI: UNDERSTANDING NETWORK OPERATIONS (Network Protocols & Wireless Security)

Tanggal: 11 Oktober 2026

Cakupan: pengantar network operations, network protocols (communication, management, security), protokol tambahan (NAT, DHCP, ARP, Telnet, SSH, email), IEEE 802.11 dan evolusi wireless security, profil karier network security engineer, serta latihan Recognize Different Network Protocols.

&nbsp;

## 1. Pengantar Network Operations

### 1.1 Mengapa Topik Ini Penting

Jaringan memindahkan data antar perangkat, dan proses perpindahan inilah yang bisa dieksploitasi oleh **malicious actor**. Tools dan protokol jaringan adalah lapisan yang menjaga perpindahan data tetap terlindungi. Bagi security analyst, pemahaman ini dipakai setiap hari.

### 1.2 Topik yang Dicakup

Section ini membahas protokol jaringan umum, VPN, firewall, security zones, dan proxy server. Room ini sendiri berfokus pada protokol jaringan dan wireless security.

### 1.3 Studi Kasus: Salah Protokol

Sebuah serangan berhasil dideteksi hanya karena protokol yang dipakai salah. Volume trafik normal dan sumbernya adalah trusted IP, tetapi protokolnya tidak sesuai konteks. Anomali kecil itu cukup untuk menghentikan serangan sebelum menimbulkan kerusakan nyata.

Pelajarannya: deteksi tidak selalu bergantung pada indikator mencolok. Analis yang hafal protokol apa yang seharusnya dipakai di suatu skenario akan lebih peka terhadap anomali.

&nbsp;

## 2. Konsep Dasar Network Protocol

### 2.1 Definisi

**Network protocol** adalah seperangkat aturan yang dipakai dua perangkat atau lebih untuk menentukan **urutan pengiriman (order of delivery)** dan **struktur data**.

Protokol berfungsi sebagai instruksi yang menyertai data di dalam data packet, memberi tahu perangkat penerima apa yang harus dilakukan dengan data tersebut. Ibaratnya bahasa umum yang membuat perangkat di seluruh dunia saling mengerti.

### 2.2 Relevansi Keamanan

Sebagian protokol memiliki kerentanan. Contohnya **DNS** dapat disalahgunakan untuk mengalihkan trafik dari website sah ke website berbahaya berisi malware. Analis perlu paham cara kerja sekaligus risiko tiap protokol.

### 2.3 Tiga Kategori Protokol

| Kategori | Fungsi |
|---|---|
| **Communication protocols** | Mengatur pertukaran data, cara transmisi, timing, dan pemulihan data yang hilang |
| **Management protocols** | Monitoring dan pengelolaan jaringan, error reporting, optimasi performa |
| **Security protocols** | Enkripsi untuk melindungi data in transit |

Ada puluhan protokol, tetapi untuk entry-level analyst yang wajib dikuasai adalah yang tercantum di resume ini.

### 2.4 Alur Mengakses Sebuah Website

Satu kunjungan website melibatkan empat protokol berurutan, dicontohkan dengan situs Yummy Recipes For Me.

1. **TCP** membentuk koneksi dan memverifikasi kedua perangkat (handshake) sebelum komunikasi lanjut.
2. **ARP** menentukan MAC address perangkat berikutnya (misalnya router) di jalur data packet.
3. **HTTPS** mengirim request halaman web dan menerima response secara aman.
4. **DNS** menerjemahkan domain menjadi IP address yang dipakai sebagai destination address packet.

&nbsp;

## 3. Communication Protocols

### 3.1 Transmission Control Protocol (TCP)

Protokol internet yang memungkinkan dua perangkat membentuk koneksi dan melakukan streaming data. Berjalan di **transport layer** TCP/IP model. Koneksi dibentuk lewat **three-way handshake**:

1. Perangkat mengirim paket **SYN** (synchronize) ke server.
2. Server membalas dengan paket **SYN/ACK** sebagai tanda menerima permintaan.
3. Perangkat mengirim paket **ACK** terakhir, lalu koneksi TCP terbentuk.

### 3.2 User Datagram Protocol (UDP)

Protokol **connectionless**: tidak membentuk koneksi sebelum transmisi. Akibatnya kurang reliable dibanding TCP, tetapi lebih cepat, sehingga cocok untuk transmisi yang harus sampai cepat, misalnya DNS request ke local DNS server. Berjalan di **transport layer**.

### 3.3 Hypertext Transfer Protocol (HTTP)

Protokol **application layer** untuk komunikasi client dan website server, memakai **port 80**. Dianggap **insecure** karena tanpa enkripsi. Sebagian besar situs sudah beralih ke HTTPS, tetapi masih banyak yang memakai HTTP.

### 3.4 Domain Name System (DNS)

Protokol yang menerjemahkan nama domain menjadi IP address. Client mengirim query ke DNS server khusus, yang mencari IP address dari domain tersebut. Biasanya memakai **UDP port 53**, tetapi berpindah ke **TCP** bila respons berukuran besar. Berjalan di **application layer**.

&nbsp;

## 4. Management Protocols

### 4.1 Simple Network Management Protocol (SNMP)

Dipakai untuk monitoring dan mengelola perangkat jaringan. Mampu mereset password perangkat, mengubah baseline configuration, dan meminta laporan pemakaian bandwidth. Berjalan di **application layer**.

### 4.2 Internet Control Message Protocol (ICMP)

Dipakai perangkat untuk saling melaporkan error transmisi data. Perangkat penerima mengirim laporan ke pengirim tentang transmisi yang terjadi. Umum untuk troubleshooting konektivitas dan latency di Linux lewat perintah `ping`. Berjalan di **internet layer**.

```
ping
```

Fungsi: mengirim ICMP request ke sebuah target untuk mengecek apakah perangkat terjangkau dan berapa latency-nya. Tidak ada switch/flag yang disebutkan di materi.

### 4.3 Dynamic Host Configuration Protocol (DHCP)

Protokol **application layer** yang bekerja dengan router untuk mengonfigurasi perangkat secara otomatis: memberi **unique IP address**, alamat DNS server yang sesuai, dan **default gateway**.

| Komponen | Port |
|---|---|
| DHCP server | UDP 67 |
| DHCP client | UDP 68 |

### 4.4 Address Resolution Protocol (ARP)

Protokol terutama di **network access layer** (layer 2) yang menerjemahkan IP address di dalam data packet menjadi **MAC address** perangkat hardware.

IP address bisa berubah seiring waktu, sedangkan MAC address permanen karena melekat pada network interface card. MAC address dipakai untuk komunikasi dalam jaringan yang sama, tetapi kadang belum diketahui, dan di situlah ARP dibutuhkan.

Tiap perangkat menjalankan ARP dan menyimpan pasangan IP dan MAC yang cocok di **ARP cache**. ARP **tidak punya port number** karena layer 2, sedangkan port berkaitan dengan layer 7 (application layer).

&nbsp;

## 5. Security Protocols

### 5.1 Hypertext Transfer Protocol Secure (HTTPS)

Versi aman HTTP yang mengenkripsi seluruh transmisi dengan **SSL/TLS** (Secure Sockets Layer / Transport Layer Security), sehingga malicious actor tidak bisa membaca isinya. Memakai **port 443**. Menggunakan digital certificates untuk autentikasi dan, menurut feedback latihan, dapat beroperasi melalui TCP port 443 dan 80. Berjalan di **application layer**.

### 5.2 Secure File Transfer Protocol (SFTP)

Protokol aman untuk memindahkan file antar perangkat, berjalan di atas **SSH**, umumnya **TCP port 22**. SSH memakai **AES (Advanced Encryption Standard)** dan enkripsi lain agar penerima yang tidak dituju tidak bisa mencegat transmisi. Berjalan di **application layer**. Sering dipakai pada cloud storage: setiap upload atau download file ke cloud storage menggunakan SFTP.

### 5.3 Batasan Enkripsi

Protokol enkripsi **tidak menyembunyikan source dan destination IP address**. Jika trafik berhasil di-intercept, malicious actor tetap bisa mengetahui informasi dasar tentang trafik tersebut.

&nbsp;

## 6. Protokol Tambahan

### 6.1 Network Address Translation (NAT)

Perangkat di LAN memiliki **private IP address** untuk komunikasi internal. Agar bisa ke internet publik, semua perangkat diwakili oleh satu **public IP address**. Untuk pesan keluar, router mengganti private source IP dengan public IP-nya, dan melakukan kebalikannya untuk respons masuk.

NAT umumnya butuh router atau firewall yang dikonfigurasi khusus. Menurut materi, NAT termasuk bagian layer 2 (internet layer) dan layer 3 (transport layer) TCP/IP model.

| Aspek | Private IP | Public IP |
|---|---|---|
| Ditetapkan oleh | Router | ISP dan IANA |
| Keunikan | Hanya di private network | Global di internet |
| Biaya | Gratis | Berbayar (sewa) |

Rentang **private IP**:

- 10.0.0.0 sampai 10.255.255.255
- 172.16.0.0 sampai 172.31.255.255
- 192.168.0.0 sampai 192.168.255.255

Rentang **public IP** yang dapat ditetapkan:

- 1.0.0.0 sampai 9.255.255.255
- 11.0.0.0 sampai 126.255.255.255
- 128.0.0.0 sampai 172.15.255.255
- 172.32.0.0 sampai 192.167.255.255
- 192.169.0.0 sampai 233.255.255.255

### 6.2 Telnet

Protokol **application layer** untuk terhubung ke sistem remote lewat command line prompts, bisa untuk perangkat lokal maupun remote. Mengirim semua informasi dalam **clear text**, sehingga tidak seaman SSH. Memakai **TCP port 23**.

### 6.3 Secure Shell (SSH)

Protokol **application layer** untuk koneksi aman ke sistem remote, dengan autentikasi aman dan komunikasi terenkripsi. Memakai **TCP port 22** dan menjadi pengganti protokol kurang aman seperti Telnet.

### 6.4 Post Office Protocol (POP)

Protokol application layer (disebut layer 4 di materi) untuk mengelola dan mengambil email dari mail server. Versi paling umum adalah **POP3**. Perangkat pengguna meminta email ke remote mail server lalu mengunduhnya ke perangkat lokal.

Email harus selesai diunduh sebelum bisa dibaca. Setelah diunduh, email mungkin dihapus atau tidak dari server, sehingga **tidak menjamin sinkronisasi** di banyak perangkat. Plaintext memakai TCP/UDP port 110, terenkripsi SSL/TLS memakai TCP/UDP port 995.

### 6.5 Internet Message Access Protocol (IMAP)

Dipakai untuk email masuk. Mengunduh header dan isi pesan, tetapi isinya **tetap tersimpan di mail server**, sehingga email bisa disinkronkan di banyak perangkat. Pengguna bisa membaca sebagian email sebelum unduhan selesai. Unencrypted memakai TCP port 143, terenkripsi TLS memakai TCP port 993.

### 6.6 Simple Mail Transfer Protocol (SMTP)

Dipakai untuk mengirim dan merutekan email dari pengirim ke alamat penerima. Bekerja dengan software **Message Transfer Agent (MTA)** yang mencari DNS server untuk menerjemahkan alamat email menjadi IP address. Unencrypted memakai TCP/UDP port 25, terenkripsi TLS (SMTPS) memakai TCP/UDP port 587.

Port 25 sering dipakai untuk spam volume tinggi. SMTP membantu memfilter spam dengan membatasi jumlah email yang boleh dikirim satu sumber dalam satu waktu.

### 6.7 Protokol, Port, dan Firewall

Port number dipakai perangkat jaringan untuk menentukan apa yang dilakukan pada data packet setelah sampai tujuan. Firewall dapat memfilter trafik berdasarkan port. Contoh: firewall hanya mengizinkan akses ke TCP port 995 (POP3) dari IP address milik organisasi.

Materi menekankan bahwa protokol dan port ini perlu dihafal karena sering muncul sebagai pertanyaan wawancara.

### 6.8 Tabel Ringkas Port

| Protokol | Port |
|---|---|
| DHCP | UDP 67 (server), UDP 68 (client) |
| ARP | tidak ada |
| Telnet | TCP 23 |
| SSH | TCP 22 |
| POP3 | TCP/UDP 110 (unencrypted), 995 (SSL/TLS) |
| IMAP | TCP 143 (unencrypted), 993 (SSL/TLS) |
| SMTP | TCP/UDP 25 (unencrypted) |
| SMTPS | TCP/UDP 587 (TLS) |

Port lain dari bagian sebelumnya: HTTP 80, HTTPS 443, DNS 53 (UDP, TCP bila respons besar), SFTP 22 (TCP).

&nbsp;

## 7. Wireless Security: IEEE 802.11 (Wi-Fi)

### 7.1 Latar Belakang

Awalnya seluruh komunikasi internet melalui kabel fisik. Pada pertengahan 1980-an, otoritas Amerika Serikat menetapkan spektrum frekuensi radio yang bisa dipakai tanpa lisensi, sehingga internet berpeluang berkembang. Pada akhir 1990-an hingga awal 2000-an, teknologi pengiriman data lewat radio dikembangkan. Kini diakses lewat laptop, smartphone, tablet, desktop, dan smart devices (thermostat, door lock, security camera).

### 7.2 Definisi

**Wi-Fi** adalah sekumpulan standar komunikasi untuk wireless LAN. Istilah ini adalah marketing term yang ditetapkan oleh **Wireless Ethernet Compatibility Alliance (WECA)**, yang kini bernama **Wi-Fi Alliance**.

Standarnya berbasis keluarga **802.11** dari **IEEE (Institute of Electrical and Electronics Engineers)**, organisasi yang memelihara standar Wi-Fi. Karena itu Wi-Fi sering disebut **IEEE 802.11**. 802.11 adalah suite protokol komunikasi nirkabel. Protokol Wi-Fi terus beradaptasi agar semakin aman dan andal, dengan tujuan setara keamanan koneksi kabel.

### 7.3 Wired Equivalent Privacy (WEP)

Dikembangkan **1999**, standar keamanan wireless **tertua**, dirancang memberi privasi setara jaringan kabel. Sudah hampir tidak dipakai, tetapi masih mungkin ditemui, misalnya router yang memakai WEP sebagai default dan tidak pernah diubah admin, atau perangkat terlalu tua untuk protokol baru. Enkripsinya dapat dibobol, sehingga dianggap **high-risk**.

### 7.4 Wi-Fi Protected Access (WPA)

Dikembangkan **2003** untuk memperbaiki dan menggantikan WEP. Dirancang sebagai **langkah transisi** agar tetap **backwards compatible** dengan hardware lama.

- Memperbaiki kelemahan protokol dan cara enkripsi WEP lewat **Temporal Key Integrity Protocol (TKIP)**, dengan secret key lebih besar sehingga lebih sulit ditebak lewat trial and error.
- Menambahkan **message integrity check** berupa message authentication tag di tiap transmisi, yang mendeteksi dan menolak transmisi yang diubah atau dikirim ulang.

Kerentanannya: **KRACK attack (key reinstallation attack)** dapat mendekripsi transmisi WPA. Penyerang menyisipkan diri di authentication handshake dan memasukkan encryption key baru menggantikan dynamic key dari WPA. Jika key diset semua nol, transmisi seolah tidak terenkripsi. Karena itu WPA digantikan WPA2.

### 7.5 WPA2

Dirilis **2004**. Memperbaiki WPA dengan **Advanced Encryption Standard (AES)** dan menyempurnakan pemakaian TKIP lewat **Counter Mode Cipher Block Chain Message Authentication Code Protocol (CCMP)**, yang menyediakan enkapsulasi serta memastikan message authentication dan integrity.

Dianggap standar keamanan semua transmisi Wi-Fi saat ini, tetapi tetap rentan terhadap KRACK, yang mendorong lahirnya WPA3 pada **2018**.

**WPA2 Personal** cocok untuk jaringan rumah. Mudah diterapkan dan setup lebih singkat, tetapi global passphrase harus dipasang di setiap komputer dan access point, sehingga tidak manageable untuk organisasi.

**WPA2 Enterprise** cocok untuk bisnis. Setup lebih rumit, tetapi memberi kontrol individual dan terpusat atas akses Wi-Fi. Admin dapat memberi atau mencabut akses pengguna kapan saja. Pengguna tidak pernah memegang encryption key, sehingga penyerang tidak bisa mengambil network key dari komputer individual.

### 7.6 WPA3

Protokol Wi-Fi aman yang penggunaannya terus naik seiring bertambahnya perangkat kompatibel. Perbedaan utama dari WPA2:

1. Menutup kerentanan authentication handshake terhadap **KRACK**.
2. Memakai **Simultaneous Authentication of Equals (SAE)**, yaitu password-authenticated, cipher-key-sharing agreement, yang mencegah penyerang mengunduh data dari koneksi wireless untuk didekripsi di sistem mereka.
3. Enkripsi lebih kuat: **128-bit**, dan WPA3-Enterprise menawarkan opsi **192-bit**.

### 7.7 Urutan Evolusi

```
WEP (1999) -> WPA (2003) -> WPA2 (2004) -> WPA3 (2018)
```

### 7.8 Pesan Utama untuk Analis

Sejarah protokol membantu memahami hal yang perlu dipertimbangkan saat melindungi jaringan wireless. Analis harus tahu kerentanan tiap protokol dan memastikan perangkat di jaringan memakai teknologi keamanan paling mutakhir.

&nbsp;

## 8. Perspektif Karier: Network Security Engineer (Antara, Google)

### 8.1 Pekerjaan Harian

Antara bekerja di Enterprise Infrastructure Protection Team Google yang melindungi infrastruktur tempat produk Google berjalan. Latar belakangnya electronics and communication, lalu pivot ke komputer, mengambil master di security, dan menjadi security engineer.

Hari kerja entry-level engineer berpusat pada pemecahan masalah, misalnya mencari tahu mengapa sebuah endpoint kebanjiran trafik atau melambat. Langkahnya: mendatangi endpoint, **capture traffic** untuk melihat trafik masuk dan keluar, mereproduksi masalah lewat **lab recreate**, dan berkonsultasi dengan ahli domain lain. Pekerjaannya diibaratkan seperti memecahkan puzzle terus-menerus.

### 8.2 Best Practice

**Jangan reinvent the wheel.** Gunakan protokol dan algoritma yang sudah teruji dan dinyatakan aman, lalu arahkan energi ke tantangan yang belum terpecahkan. Cybersecurity adalah bidang menarik karena teknologi tumbuh eksponensial dan terus memunculkan tantangan baru.

&nbsp;

## 9. Latihan: Recognize Different Network Protocols

Nilai 100% (lulus minimal 75%). Poin yang diuji:

1. Network protocols adalah aturan untuk menggambarkan **order of delivery** dan struktur data.
2. Protokol komunikasi aman antara client dan web server adalah **HTTPS**.
3. Protokol keamanan untuk menjaga informasi dari malicious actor adalah **SSL/TLS**, yang mengamankan HTTP menjadi HTTPS.
4. IEEE 802.11 (Wi-Fi) adalah standar komunikasi wireless LAN: **True**.

&nbsp;

## 10. Glosarium Istilah Kunci

| Istilah | Penjelasan |
|---|---|
| **Network protocol** | Aturan urutan pengiriman dan struktur data |
| **Handshake** | Proses verifikasi dan pembentukan koneksi, contoh TCP three-way handshake |
| **SYN, SYN/ACK, ACK** | Tiga paket dalam three-way handshake TCP |
| **Connectionless** | Tanpa pembentukan koneksi, ciri UDP |
| **ARP cache** | Penyimpanan pasangan IP dan MAC address |
| **MAC address** | Alamat permanen unik pada network interface card |
| **NAT** | Penggantian private IP menjadi public IP untuk trafik keluar |
| **SSL/TLS** | Enkripsi yang mengamankan HTTPS |
| **AES** | Standar enkripsi pada SSH/SFTP dan WPA2 |
| **MTA** | Software SMTP yang mencari IP tujuan via DNS |
| **IANA** | Pemberi port number protokol dan penetap public IP bersama ISP |
| **IEEE** | Organisasi pemelihara standar Wi-Fi (802.11) |
| **WECA / Wi-Fi Alliance** | Pencipta istilah Wi-Fi, kini bernama Wi-Fi Alliance |
| **TKIP** | Protokol enkripsi WPA yang memperbaiki WEP |
| **CCMP** | Protokol enkapsulasi dan autentikasi pesan WPA2 |
| **KRACK attack** | Serangan key reinstallation pada handshake WPA/WPA2 |
| **SAE** | Autentikasi password WPA3, mencegah dekripsi offline |

&nbsp;

## 11. Tools & Platform Rujukan

Materi room ini tidak menyebut tools atau platform eksternal berikut URL-nya. Yang muncul hanya satu command dan beberapa organisasi rujukan:

- **ping** (command Linux): troubleshooting konektivitas dan latency lewat ICMP.
- **IANA**: penetap port number protokol dan public IP address.
- **IEEE**: pemelihara standar 802.11.
- **Wi-Fi Alliance**: organisasi pemegang istilah Wi-Fi (sebelumnya WECA).

&nbsp;

## 12. Catatan Ringkas untuk Ditulis Tangan

### Dasar Protokol

- Network protocol — aturan urutan pengiriman + struktur data
- 3 kategori — communication, management, security
- Contoh kerentanan — DNS dipakai alihkan ke situs malware
- Kasus serangan — IP trusted, volume normal, tapi protokol salah

### Communication

- TCP — koneksi + streaming data, transport layer
- Three-way handshake — SYN, SYN/ACK, ACK
- UDP — connectionless, cepat, kurang reliable, transport layer
- HTTP — application layer, port 80, insecure
- DNS — domain jadi IP, UDP 53, pindah TCP bila respons besar

### Management

- SNMP — monitor dan kelola perangkat, reset password, ubah baseline, cek bandwidth
- ICMP — laporan error transmisi, internet layer
- ping — command troubleshooting konektivitas dan latency (Linux)
- DHCP — beri IP, DNS server, default gateway; server UDP 67, client UDP 68
- ARP — IP jadi MAC, layer 2, ada ARP cache, tanpa port

### Security

- HTTPS — HTTP + SSL/TLS, port 443
- SFTP — transfer file via SSH, TCP 22, AES, dipakai cloud storage
- Catatan — enkripsi tidak menyembunyikan source/destination IP

### Tambahan

- NAT — private IP diganti public IP di router/firewall
- Private IP — dari router, gratis, unik lokal; 10.x, 172.16-31.x, 192.168.x
- Public IP — dari ISP dan IANA, unik global, berbayar
- Telnet — remote, clear text, TCP 23
- SSH — remote aman, TCP 22, pengganti Telnet
- POP3 — unduh email ke lokal, tidak jamin sinkron; 110 plain, 995 SSL/TLS
- IMAP — email tetap di server, sinkron multi device; 143 plain, 993 TLS
- SMTP — kirim dan rute email via MTA; 25 plain, 587 TLS (SMTPS)
- Firewall — filter trafik berdasarkan port

### Wi-Fi

- Wi-Fi — standar wireless LAN, istilah marketing dari WECA (kini Wi-Fi Alliance)
- IEEE 802.11 — suite protokol wireless, dipelihara IEEE
- WEP (1999) — tertua, bisa dibobol, high-risk
- WPA (2003) — transisi, TKIP, message integrity check, rentan KRACK
- KRACK — sisip key baru saat handshake, key semua nol berarti tanpa enkripsi
- WPA2 (2004) — AES + CCMP, standar saat ini, masih rentan KRACK
- WPA2 Personal — satu passphrase global, untuk rumah
- WPA2 Enterprise — kontrol terpusat, user tidak pegang key, untuk bisnis
- WPA3 (2018) — tahan KRACK, SAE, 128-bit, Enterprise opsional 192-bit

### Karier dan Praktik

- Harian engineer — debug endpoint, capture traffic, lab recreate, konsultasi ahli
- Best practice — jangan reinvent the wheel, fokus ke unsolved challenges
- Tugas analis — pastikan koneksi wireless organisasi aman dan perangkat memakai teknologi terbaru
