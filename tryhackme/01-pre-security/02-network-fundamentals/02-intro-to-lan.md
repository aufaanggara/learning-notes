# Networking Fundamentals — Resume Materi
**Room:** Pre-Security > What is Networking / LAN / Subnetting / ARP / DHCP
**Tanggal:** 01 September 2026

---

## 1. Topologi Jaringan (LAN Topology)

**Topologi** adalah desain atau tampilan fisik maupun logis dari sebuah jaringan — bagaimana perangkat saling terhubung dan bagaimana data mengalir di antara mereka.

Topologi terbagi dua jenis:

- **Topologi Fisik** — susunan nyata kabel dan perangkat secara fisik
- **Topologi Logis** — bagaimana data mengalir dalam jaringan tersebut

---

### 1.1 Star Topology

Setiap perangkat terhubung secara individual ke sebuah **switch atau hub pusat** menggunakan kabel masing-masing. Semua data yang dikirim antar perangkat harus melewati switch terlebih dahulu.

**Keunggulan:**

- Paling andal dan paling mudah dikembangkan (*scalable*)
- Jika satu kabel putus, hanya satu perangkat yang terdampak — jaringan lain tetap jalan
- Mudah menambah perangkat baru tanpa mengganggu yang sudah ada

**Kelemahan:**

- Biaya tinggi karena butuh banyak kabel dan switch
- Switch adalah **single point of failure** — jika switch mati, seluruh jaringan mati
- Semakin besar jaringan, semakin intensif pemeliharaan yang dibutuhkan

Star topology adalah topologi yang **paling umum digunakan** saat ini.

---

### 1.2 Bus Topology

Semua perangkat terhubung ke satu **kabel backbone** (kabel tulang punggung). Data bergerak ke dua arah (kiri dan kanan) di sepanjang backbone hingga mencapai perangkat tujuan. Kedua ujung kabel harus dipasang **terminator** untuk mencegah sinyal memantul balik.

**Keunggulan:**

- Murah dan mudah dipasang
- Tidak butuh perangkat jaringan khusus seperti switch

**Kelemahan:**

- Sangat rentan **bottleneck** — jika banyak perangkat mengirim data bersamaan, jaringan melambat
- Troubleshooting sulit karena semua data melewati jalur yang sama
- Backbone putus = seluruh jaringan mati (single point of failure di kabel, bukan perangkat)
- Tidak bisa menangani volume data yang besar

---

### 1.3 Ring Topology

Setiap perangkat terhubung ke dua perangkat di sebelahnya membentuk lingkaran penuh. Data bergerak satu arah (searah) melewati tiap perangkat sampai mencapai tujuan — mirip sistem estafet.

Perangkat hanya akan meneruskan data dari perangkat lain **jika ia tidak sedang mengirim data miliknya sendiri**. Jika ada data yang perlu dikirim, data miliknya sendiri didahulukan.

**Keunggulan:**

- Hemat kabel, tidak butuh perangkat pusat
- Relatif mudah di-troubleshoot karena data bergerak satu arah
- Lebih tahan bottleneck dibanding bus topology

**Kelemahan:**

- Satu kabel putus atau satu perangkat mati = seluruh jaringan lumpuh
- Tidak efisien — data bisa harus melewati banyak perangkat sebelum sampai tujuan

---

### 1.4 Perbandingan Topologi

| Aspek | Star | Bus | Ring |
|---|---|---|---|
| Biaya | Tinggi | Rendah | Sedang |
| Skalabilitas | Tinggi | Rendah | Sedang |
| Troubleshooting | Sedang | Sulit | Mudah |
| Keandalan | Tinggi | Rendah | Rendah |
| Cara rusak | Switch mati | Overload/backbone putus | Satu titik putus |

---

## 2. Perangkat Jaringan

### 2.1 Switch

Switch adalah perangkat yang menghubungkan banyak perangkat dalam **satu jaringan lokal (LAN)**. Perangkat seperti komputer, printer, atau kamera dicolokkan ke **port** switch menggunakan kabel ethernet (RJ-45).

Switch beroperasi di **OSI Layer 2 (Data Link Layer)**.

**Cara kerja:**

Switch menyimpan **MAC Address Table** — tabel yang mencatat perangkat mana terhubung ke port mana. Ketika menerima data, switch langsung mengirimnya ke port tujuan yang tepat, bukan menyebarkan ke semua port.

**Ukuran port umum:** 4, 8, 16, 24, 32, 64 port

**Switch vs Hub:**

| Aspek | Switch | Hub |
|---|---|---|
| Cara kirim | Unicast (langsung ke tujuan) | Broadcast (ke semua port) |
| Kecerdasan | Punya MAC Address Table | Tidak punya memori |
| Efisiensi | Tinggi | Rendah |
| Keamanan | Lebih aman | Kurang aman |

Switch dan router bisa saling dihubungkan untuk meningkatkan **redundansi** — jika satu jalur mati, jalur lain tersedia, sehingga tidak ada downtime.

---

### 2.2 Router

Router menghubungkan **jaringan yang berbeda** satu sama lain dan meneruskan data di antara jaringan tersebut menggunakan proses yang disebut **routing**.

Router beroperasi di **OSI Layer 3 (Network Layer)**.

**Routing** adalah proses menentukan jalur terbaik agar data bisa sampai ke jaringan tujuan. Router menggunakan **IP address** dan **Routing Table** untuk membuat keputusan ini.

Perbedaan mendasar dengan switch: switch menghubungkan perangkat *dalam* satu jaringan, sementara router menghubungkan *antar* jaringan yang berbeda.

**Alur data umum:** Internet → Router → Switch → Perangkat

---

## 3. LAN (Local Area Network)

LAN adalah jaringan yang menghubungkan perangkat-perangkat dalam **area geografis terbatas** seperti satu rumah, satu gedung kantor, atau satu kampus.

**Fungsi utama LAN:**

- Berbagi file antar perangkat tanpa media fisik seperti flashdisk
- Berbagi satu printer untuk banyak komputer sekaligus
- Berbagi satu koneksi internet ke semua perangkat
- Menjaga data internal tetap aman di dalam jaringan lokal tanpa harus keluar ke internet
- Mendukung komunikasi antar perangkat dengan latensi rendah

---

## 4. Subnetting

**Subnetting** adalah proses memecah satu jaringan besar menjadi jaringan-jaringan yang lebih kecil (*subnet*) di dalam jaringan itu sendiri.

**Tujuan:** Agar administrator jaringan bisa mengkategorikan dan mengontrol bagian-bagian jaringan secara terpisah — misalnya memisahkan departemen Accounting, Finance, dan Human Resources dalam satu perusahaan meskipun semuanya terhubung ke internet yang sama.

---

### 4.1 Struktur IP Address dan Subnet Mask

IP address terdiri dari **empat oktet** (contoh: `192.168.1.1`), masing-masing oktet bernilai 0–255.

**Subnet mask** memiliki format yang sama — empat oktet, 32 bit total — dan berfungsi menentukan berapa banyak host (perangkat) yang bisa masuk ke dalam satu jaringan.

Contoh umum: `255.255.255.0` artinya jaringan bisa menampung hingga **254 perangkat**.

---

### 4.2 Tiga Cara Subnet Menggunakan IP Address

**Network Address**

Mengidentifikasi jaringan itu sendiri, bukan perangkat. Angka oktet terakhir selalu `.0`.

Contoh: Perangkat dengan IP `192.168.1.100` berada di jaringan `192.168.1.0`.

**Host Address**

Mengidentifikasi satu perangkat spesifik di dalam subnet. Rentang yang valid adalah `.1` hingga `.254` (total 254 host dalam subnet /24).

Contoh: `192.168.1.100`

**Default Gateway**

Alamat perangkat (biasanya router) yang bertanggung jawab meneruskan data ke jaringan lain. Digunakan ketika data perlu keluar dari subnet lokal. Biasanya menggunakan alamat `.1` atau `.254`.

Contoh: `192.168.1.254`

---

### 4.3 Manfaat Subnetting

- **Efficiency** — lalu lintas data hanya beredar di subnet yang relevan, tidak membanjiri seluruh jaringan
- **Security** — perangkat di satu subnet tidak bisa sembarangan mengakses perangkat di subnet lain
- **Full Control** — administrator punya kendali penuh atas siapa bisa mengakses apa

**Contoh nyata:** Kafe memiliki dua subnet — satu untuk kasir dan karyawan (data sensitif), satu untuk WiFi pengunjung (hotspot publik). Keduanya tetap bisa mengakses internet, tapi sepenuhnya terpisah satu sama lain.

---

## 5. ARP (Address Resolution Protocol)

ARP adalah protokol yang **mengaitkan IP address (identitas logis) dengan MAC address (identitas fisik)** suatu perangkat di jaringan lokal. Ini diperlukan karena komunikasi di level LAN sebenarnya menggunakan MAC address, sementara manusia dan sistem bekerja dengan IP address.

---

### 5.1 Dua Identitas Perangkat

**MAC Address — Identitas Fisik**

Tertanam langsung ke hardware kartu jaringan (NIC) saat diproduksi. Bersifat permanen dan unik secara global — tidak ada dua perangkat di dunia yang memiliki MAC address sama. Tidak berubah meskipun perangkat pindah jaringan.

Format contoh: `01:00:AB:78:99:33`

**IP Address — Identitas Logis**

Ditetapkan melalui software atau konfigurasi. Bisa berubah tergantung jaringan yang dimasuki — laptop yang sama bisa mendapat IP berbeda di jaringan yang berbeda.

Format contoh: `192.168.1.10`

---

### 5.2 Cara Kerja ARP

Setiap perangkat menyimpan **ARP Cache** — tabel sementara yang mencatat pasangan IP address dan MAC address perangkat lain yang sudah diketahui. Cache ini bersifat sementara dan akan kedaluwarsa setelah beberapa waktu.

Untuk mengetahui MAC address dari suatu IP, ARP menggunakan dua jenis pesan:

**ARP Request**

Perangkat mengirim broadcast ke seluruh jaringan:

- Source MAC: MAC perangkat penanya
- Destination MAC: `FF:FF:FF:FF:FF:FF` (broadcast universal — semua perangkat wajib membaca)
- Pesan: *"Siapa yang memiliki IP 192.168.1.10?"*

Semua perangkat menerima pesan ini, tapi hanya yang memiliki IP tersebut yang akan merespons.

**ARP Reply**

Perangkat pemilik IP menjawab langsung ke penanya:

- Source MAC: MAC perangkat pemilik IP
- Destination MAC: MAC perangkat penanya (bukan broadcast lagi)
- Pesan: *"Saya yang punya IP itu, MAC saya adalah 18:AC:33:12:88:29"*

Setelah menerima reply, perangkat penanya menyimpan pasangan IP-MAC ini ke **ARP Cache** untuk digunakan pada komunikasi berikutnya tanpa perlu bertanya ulang.

---

### 5.3 Ancaman Keamanan: ARP Spoofing

**ARP Spoofing** (juga disebut ARP Poisoning) adalah serangan di mana penyerang mengirimkan ARP reply palsu — mengklaim bahwa MAC address mereka terhubung ke IP address milik perangkat lain.

Akibatnya, lalu lintas data yang seharusnya ke perangkat korban malah dialihkan ke penyerang. Teknik ini adalah dasar dari serangan **Man-in-the-Middle (MitM)**.

---

## 6. DHCP (Dynamic Host Configuration Protocol)

DHCP adalah protokol yang memberikan **IP address secara otomatis** ke perangkat yang terhubung ke jaringan, tanpa perlu konfigurasi manual.

IP address bisa ditetapkan dengan dua cara:
- **Manual (statis)** — administrator mengetik IP langsung ke setiap perangkat
- **Otomatis (dinamis)** — menggunakan DHCP server (cara yang paling umum dipakai)

---

### 6.1 Proses DHCP — DORA

Proses pemberian IP address oleh DHCP terdiri dari empat langkah yang disingkat **DORA**:

**D — Discover**

Perangkat baru mengirim broadcast ke seluruh jaringan mencari DHCP server. Pada tahap ini perangkat belum memiliki IP, sehingga menggunakan source IP `0.0.0.0` dan destination IP `255.255.255.255`.

Pesan: *"Apakah ada DHCP server? Saya butuh IP address."*

**O — Offer**

DHCP server merespons dengan menawarkan sebuah IP address yang tersedia dari **DHCP Pool** (daftar IP yang dikelola DHCP server).

Pesan: *"Ada! Kamu bisa menggunakan IP 192.168.1.10."*

**R — Request**

Perangkat mengkonfirmasi bahwa ia menerima tawaran tersebut. Konfirmasi ini masih dikirim secara broadcast agar DHCP server lain (jika ada lebih dari satu) tahu bahwa tawaran mereka tidak dipilih.

Pesan: *"Oke, saya mau menggunakan IP 192.168.1.10."*

**A — ACK (Acknowledgement)**

DHCP server mengirim konfirmasi akhir bahwa proses selesai. IP address resmi diberikan beserta informasi **lease time** — durasi IP tersebut dipinjamkan.

Pesan: *"Confirmed. Kamu bisa menggunakan IP itu selama 24 jam ke depan."*

---

### 6.2 Konsep Penting DHCP

**Lease Time** adalah durasi peminjaman IP address dari DHCP server. IP tidak diberikan secara permanen — setelah lease time habis, perangkat harus menjalankan proses DORA ulang. Inilah alasan IP address bersifat "dinamis" dan bisa berubah antar sesi koneksi.

**DHCP Pool** adalah kumpulan IP address yang tersedia untuk dibagikan oleh DHCP server. DHCP memilih IP yang belum dipakai dari pool ini setiap kali ada perangkat baru yang meminta.

Di jaringan rumah, router biasanya sekaligus berperan sebagai DHCP server — itulah mengapa HP atau laptop langsung mendapat IP begitu terhubung ke WiFi.

---

## 7. Glossary — Istilah Penting

**Topology** — desain atau tampilan jaringan, baik fisik maupun logis

**Backbone** — kabel utama tunggal tempat semua perangkat terhubung di bus topology

**Terminator** — penutup di ujung kabel backbone untuk mencegah sinyal memantul

**Broadcast** — pengiriman data ke semua perangkat dalam satu jaringan sekaligus

**Unicast** — pengiriman data langsung ke satu perangkat tujuan spesifik

**MAC Address** — identitas fisik perangkat yang tertanam permanen di hardware (NIC)

**IP Address** — identitas logis perangkat yang bersifat dinamis dan bisa berubah

**NIC** — Network Interface Card, kartu jaringan fisik di dalam perangkat

**Oktet** — satu segmen dari IP address; IP terdiri dari empat oktet (contoh: 192.168.1.1)

**Subnet** — bagian/potongan yang lebih kecil dari sebuah jaringan yang lebih besar

**Subnet Mask** — angka empat oktet yang menentukan ukuran dan batas sebuah subnet

**Network Address** — IP yang mengidentifikasi jaringan itu sendiri, bukan perangkat (oktet akhir .0)

**Host Address** — IP yang mengidentifikasi satu perangkat spesifik di dalam subnet

**Default Gateway** — IP perangkat (biasanya router) yang meneruskan data ke jaringan lain

**Routing** — proses menentukan jalur terbaik untuk mengirim data antar jaringan

**Routing Table** — tabel yang digunakan router untuk menyimpan informasi jalur ke berbagai jaringan

**ARP Cache** — tabel sementara di setiap perangkat yang menyimpan pasangan IP-MAC yang sudah diketahui

**DHCP Pool** — kumpulan IP address tersedia yang dikelola oleh DHCP server

**Lease Time** — durasi peminjaman IP address dari DHCP server; setelah habis, proses DORA diulang

**Redundansi** — ketersediaan jalur cadangan agar jaringan tidak mati jika satu jalur rusak

**Single Point of Failure** — satu titik yang jika rusak menyebabkan seluruh jaringan berhenti

**Bottleneck** — kondisi jaringan melambat karena kapasitas jalur data tidak mampu menangani volume trafik

**MitM (Man-in-the-Middle)** — serangan di mana penyerang menyadap komunikasi antara dua pihak

**ARP Spoofing** — serangan yang mengirim ARP reply palsu untuk mengalihkan lalu lintas data ke penyerang

**FF:FF:FF:FF:FF:FF** — MAC address broadcast universal; semua perangkat di jaringan yang sama wajib membacanya

**DORA** — singkatan tahapan DHCP: Discover, Offer, Request, ACK

---

## 8. Catatan Ringkas untuk Ditulis Tangan

### Topologi

- Topology — desain jaringan (fisik & logis)
- Star — semua ke switch pusat; scalable; switch mati = semua mati
- Bus — semua ke backbone; murah; backbone putus/overload = mati
- Ring — lingkaran; 1 arah; 1 titik rusak = semua mati
- Single Point of Failure — satu titik rusak, seluruh jaringan mati

### Switch & Router

- Switch — hubungkan perangkat dalam 1 LAN; Layer 2; punya MAC Address Table
- Port — slot fisik tempat colok kabel ethernet di switch
- MAC Address Table — tabel di switch: perangkat mana di port mana
- Unicast — kirim data langsung ke tujuan (switch)
- Broadcast — kirim ke semua port (hub)
- Router — hubungkan antar jaringan berbeda; Layer 3
- Routing — proses tentukan jalur data antar jaringan
- Routing Table — tabel jalur yang dimiliki router
- Alur: Internet → Router → Switch → Perangkat

### LAN & Subnetting

- LAN — jaringan area lokal (rumah/kantor/sekolah)
- Subnetting — pecah jaringan besar jadi subnet-subnet kecil
- Subnet Mask — angka 4 oktet penentu ukuran jaringan; contoh: 255.255.255.0
- Network Address — identitas jaringan; oktet akhir .0; contoh: 192.168.1.0
- Host Address — identitas perangkat; contoh: 192.168.1.100; range .1–.254
- Default Gateway — pintu keluar ke jaringan lain; biasanya .1 atau .254
- Manfaat subnet: Efficiency, Security, Full Control

### ARP

- ARP — protokol yang cocokkan IP address dengan MAC address
- MAC Address — identitas FISIK; permanen; di-burn ke hardware (NIC)
- IP Address — identitas LOGIS; bisa berubah; diberikan via software
- ARP Request — broadcast ke FF:FF:FF:FF:FF:FF: "Siapa yang punya IP X?"
- ARP Reply — jawaban langsung ke penanya: "Saya, MAC saya adalah Y"
- ARP Cache — tabel sementara simpan pasangan IP-MAC; bersifat sementara
- FF:FF:FF:FF:FF:FF — MAC broadcast; semua perangkat wajib baca
- ARP Spoofing — kirim reply palsu; alihkan trafik ke penyerang; dasar MitM

### DHCP

- DHCP — beri IP address otomatis ke perangkat baru
- DHCP Pool — daftar IP tersedia di server
- Lease Time — durasi peminjaman IP; habis = DORA ulang
- DORA:
  - D Discover — broadcast cari DHCP server; src IP 0.0.0.0
  - O Offer — server tawarkan IP dari pool
  - R Request — perangkat konfirmasi terima tawaran; masih broadcast
  - A ACK — server konfirmasi final + beri lease time
