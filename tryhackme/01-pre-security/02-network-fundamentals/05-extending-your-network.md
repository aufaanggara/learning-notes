# Networking Fundamentals — Resume Materi
**TryHackMe | Extending Your Network**
Tanggal: 25 Mar 2026

---

## 1. Port Forwarding

### 1.1 Konsep Dasar

**Port forwarding** adalah mekanisme yang memungkinkan aplikasi atau layanan yang berjalan di jaringan lokal dapat diakses dari Internet.

Tanpa port forwarding, sebuah server hanya dapat dijangkau oleh perangkat yang berada dalam jaringan yang sama (kondisi ini disebut **intranet**). Port forwarding "membuka pintu" agar traffic dari luar bisa diteruskan ke server internal yang sesuai.

Port forwarding dikonfigurasi di **router**, bukan di server itu sendiri. Router-lah yang bertanggung jawab meneruskan traffic dari IP publik ke IP privat di dalam jaringan lokal.

### 1.2 Alur Kerja

```
[Server Internal]        [Router]           [Internet]        [Pengguna Luar]
192.168.1.10:80   →   82.62.51.70:80   →   The Internet   →   Bisa akses
```

Pengguna luar mengakses menggunakan **IP publik router** (bukan IP privat server), dan router meneruskannya ke server yang sesuai.

### 1.3 Port Forwarding vs Firewall

Keduanya berbeda peran dan sering disalahartikan sebagai hal yang sama.

| Aspek | Port Forwarding | Firewall |
|---|---|---|
| Fungsi | Membuka port tertentu agar bisa diakses | Menentukan apakah traffic boleh melewati port |
| Sifat | Membuka "pintu" | Menjaga/mengatur "pintu" |

Firewall tetap bisa memblokir traffic meskipun port sudah dibuka oleh port forwarding. Keduanya bekerja di layer yang berbeda dan tidak saling menggantikan.

---

## 2. Firewall

### 2.1 Definisi dan Fungsi

**Firewall** adalah perangkat dalam jaringan yang bertugas menentukan traffic mana yang diizinkan masuk dan keluar. Firewall berfungsi seperti **border security** — pos perbatasan yang memeriksa setiap paket sebelum diizinkan lewat.

Administrator dapat mengkonfigurasi firewall untuk **permit** (mengizinkan) atau **deny** (menolak) traffic berdasarkan faktor-faktor berikut:

- **Source** — dari jaringan mana traffic berasal
- **Destination** — ke jaringan mana traffic menuju
- **Port** — port berapa yang digunakan (misalnya port 80 saja)
- **Protocol** — protokol apa yang dipakai (TCP, UDP, atau keduanya)

Firewall melakukan **packet inspection** untuk mengevaluasi setiap paket berdasarkan faktor-faktor di atas.

### 2.2 Bentuk Firewall

Firewall hadir dalam berbagai wujud, menyesuaikan skala jaringan:

- **Hardware dedicated** — perangkat fisik khusus, umum dipakai di jaringan besar/perusahaan, mampu menangani volume data yang sangat besar
- **Software** — aplikasi yang berjalan di sistem operasi, contohnya **Snort**
- **Residential router** — router rumahan yang sudah memiliki firewall bawaan

Firewall dapat dikategorikan ke dalam 2 hingga 5 kategori, namun dua kategori utama yang paling penting adalah stateful dan stateless.

### 2.3 Stateful Firewall

Firewall jenis ini mengevaluasi **keseluruhan koneksi**, bukan sekadar paket individual. Keputusannya bersifat **dinamis** — berubah sesuai perkembangan koneksi.

Karakteristik utama:

- Mengonsumsi **resource lebih banyak** karena harus melacak status setiap koneksi
- Mampu menangkap anomali yang terjadi di tengah koneksi — misalnya mengizinkan bagian awal TCP handshake, lalu memblokir ketika handshake gagal di tahap berikutnya
- Jika koneksi dari suatu host dinilai buruk, **seluruh perangkat host tersebut diblokir**, bukan hanya paket yang bermasalah
- Lebih **cerdas dan kontekstual** dibanding stateless

### 2.4 Stateless Firewall

Firewall jenis ini menggunakan **aturan statis (static rules)** untuk mengevaluasi **paket individual** satu per satu, tanpa mempertimbangkan konteks koneksi secara keseluruhan.

Karakteristik utama:

- Konsumsi **resource jauh lebih rendah**
- Sebuah perangkat yang mengirim paket buruk **tidak otomatis diblokir seluruhnya** — hanya paket yang tidak sesuai aturan yang ditolak
- Kelemahannya: hanya efektif jika traffic persis cocok dengan aturan yang sudah didefinisikan. Jika tidak ada aturan yang cocok, firewall ini tidak berdaya
- **Unggul dalam menghadapi DDoS** (Distributed Denial-of-Service) atau traffic massal dari banyak host sekaligus, karena dapat memproses volume besar dengan efisien

### 2.5 Perbandingan Stateful vs Stateless

| Aspek | Stateful | Stateless |
|---|---|---|
| Basis evaluasi | Keseluruhan koneksi | Paket individual |
| Penggunaan resource | Tinggi | Rendah |
| Kecerdasan keputusan | Dinamis | Statis |
| Respons terhadap host buruk | Blokir seluruh perangkat | Hanya paket yang cocok aturan |
| Keunggulan | Lebih akurat & kontekstual | Efektif untuk traffic massal/DDoS |

---

## 3. VPN (Virtual Private Network)

### 3.1 Definisi

**VPN** (Virtual Private Network) adalah teknologi yang memungkinkan perangkat-perangkat di jaringan yang berbeda untuk berkomunikasi secara aman melalui Internet, dengan cara membuat **tunnel** — jalur privat terenkripsi yang melintas di atas jaringan publik.

Perangkat-perangkat yang terhubung melalui tunnel ini membentuk **jaringan privat tersendiri**, meskipun mereka secara fisik berada di lokasi yang berbeda.

### 3.2 Cara Kerja (Contoh 3 Jaringan)

Bayangkan dua kantor terpisah yang masing-masing punya jaringan sendiri:

- **Network #1** — Office #1
- **Network #2** — Office #2
- **Network #3** — Jaringan privat VPN (terbentuk dari perangkat terpilih di N#1 dan N#2)

Perangkat di Network #3 tetap merupakan bagian dari jaringan asalnya (N#1 atau N#2), tetapi secara bersamaan juga membentuk jaringan privat yang **hanya bisa diakses oleh sesama anggota VPN**.

### 3.3 Manfaat VPN

**Koneksi lintas lokasi geografis**

Bisnis dengan banyak kantor di kota atau negara berbeda dapat mengakses server dan infrastruktur bersama seolah berada dalam satu jaringan lokal yang sama.

**Privasi data**

VPN mengenkripsi seluruh data yang dikirimkan sehingga hanya bisa dipahami oleh perangkat pengirim dan penerima. Data yang melintas tidak rentan terhadap **sniffing** (penyadapan). Sangat berguna di jaringan WiFi publik yang tidak menyediakan enkripsi.

**Anonimitas**

Tanpa VPN, traffic dapat dilihat dan dilacak oleh **ISP** (penyedia layanan internet) serta perantara jaringan lainnya. VPN menyembunyikan traffic dari pihak-pihak tersebut.

Catatan penting: tingkat anonimitas yang diberikan VPN bergantung pada sejauh mana **provider VPN itu sendiri** menghormati privasi pengguna. VPN yang menyimpan log seluruh aktivitas penggunanya pada dasarnya tidak memberikan anonimitas yang berarti.

### 3.4 Teknologi VPN

**PPP (Point-to-Point Protocol)**

Teknologi dasar yang digunakan untuk autentikasi dan enkripsi data dalam koneksi VPN. Bekerja dengan mekanisme **private key dan public certificate** (konsep serupa dengan SSH) — keduanya harus cocok agar koneksi berhasil.

Keterbatasan: PPP bersifat **non-routable**, artinya tidak bisa keluar dari jaringan dengan sendirinya. Membutuhkan PPTP untuk difungsikan sebagai VPN.

**PPTP (Point-to-Point Tunneling Protocol)**

Teknologi yang memungkinkan data dari PPP untuk berjalan keluar dari jaringan dan melintas di Internet. PPTP-lah yang membuat PPP menjadi VPN yang fungsional.

- Kelebihan: sangat mudah dikonfigurasi, didukung oleh hampir semua perangkat
- Kelemahan: enkripsinya **lemah** dibandingkan alternatif modern

**IPSec (Internet Protocol Security)**

Protokol yang mengenkripsi data menggunakan kerangka **Internet Protocol (IP)** yang sudah ada sebagai dasarnya.

- Kelebihan: enkripsi **sangat kuat**, didukung banyak perangkat
- Kelemahan: konfigurasi lebih **kompleks dan sulit** dibanding PPP/PPTP

| Teknologi | Kemudahan Setup | Kekuatan Enkripsi | Routable |
|---|---|---|---|
| PPP | — | Ada | Tidak |
| PPTP | Sangat mudah | Lemah | Ya |
| IPSec | Sulit | Kuat | Ya |

---

## 4. LAN Networking Devices

### 4.1 Router

**Router** adalah perangkat yang bertugas menghubungkan jaringan-jaringan berbeda dan meneruskan data di antara mereka. Proses pengiriman data antar jaringan ini disebut **routing**.

Router beroperasi di **Layer 3 (Network Layer)** dari model OSI. Router sering memiliki antarmuka konfigurasi (berbasis web atau konsol) yang memungkinkan administrator mengatur aturan seperti port forwarding dan firewalling.

Router adalah **perangkat dedicated** — fungsinya tidak tumpang tindih dengan switch.

**Faktor pemilihan jalur routing:**

Ketika ada lebih dari satu jalur menuju tujuan, router (atau protokol routing) akan memilih berdasarkan:

- Jalur mana yang **paling pendek** (hop count)
- Jalur mana yang **paling andal** (reliabilitas link)
- Jalur mana yang memiliki **medium lebih cepat** (misalnya fiber lebih cepat dari copper)

### 4.2 Switch

**Switch** adalah perangkat jaringan khusus yang menghubungkan banyak perangkat dalam satu jaringan lokal menggunakan kabel Ethernet. Switch mampu mengelola **3 hingga 63 perangkat** dalam satu jaringan.

Switch dapat beroperasi di Layer 2 atau Layer 3 dari model OSI, tetapi sifatnya **eksklusif** — sebuah switch Layer 2 tidak dapat sekaligus beroperasi di Layer 3.

**Layer 2 Switch**

Mengirimkan **frames** ke perangkat yang tepat berdasarkan **MAC address**. Paket IP yang dikirim dikapsulasi di dalam frames sebelum diteruskan.

Fungsi Layer 2 switch murni terbatas pada pengiriman frames ke perangkat yang benar — tidak lebih dari itu.

**Layer 3 Switch**

Lebih canggih dari Layer 2 karena dapat menjalankan **sebagian fungsi router**. Kemampuannya meliputi:

- Mengirimkan frames ke perangkat (seperti Layer 2)
- Merutekan paket ke jaringan lain menggunakan protokol IP

Layer 3 switch dapat memiliki **lebih dari satu IP address** untuk mengelola beberapa segmen jaringan sekaligus (contoh: 192.168.1.1 dan 192.168.2.1).

### 4.3 Perbandingan Router vs Switch

| Perangkat | Layer OSI | Basis pengiriman | Fungsi utama |
|---|---|---|---|
| Router | Layer 3 | IP Address | Hubungkan jaringan berbeda |
| Switch Layer 2 | Layer 2 | MAC Address | Hubungkan perangkat dalam satu jaringan |
| Switch Layer 3 | Layer 2 & 3 | MAC + IP Address | Hubungkan perangkat + sebagian routing |

---

## 5. VLAN (Virtual Local Area Network)

### 5.1 Definisi

**VLAN** adalah teknologi yang memungkinkan perangkat-perangkat dalam satu jaringan fisik untuk **dipisahkan secara virtual** menjadi segmen-segmen terpisah. Meskipun perangkat terhubung ke switch yang sama secara fisik, mereka diperlakukan seolah berada di jaringan yang berbeda.

### 5.2 Cara Kerja dan Manfaat

Dengan VLAN, administrator dapat membagi satu switch fisik menjadi beberapa segmen berdasarkan departemen, fungsi, atau tingkat kepercayaan. Setiap VLAN biasanya memiliki **subnet IP tersendiri**.

Pemisahan ini memberikan **keamanan jaringan** karena aturan yang sudah dikonfigurasi menentukan bagaimana perangkat antar VLAN boleh berkomunikasi. Tanpa izin eksplisit, perangkat di VLAN yang berbeda tidak bisa saling terhubung.

Contoh implementasi nyata:

```
Router
  |
Layer 3 Switch
  |                    |
VLAN 1 (192.168.1.1)   VLAN 2 (192.168.2.1)
Sales Department       Accounting Department
```

Dalam skenario ini, Sales dan Accounting **keduanya bisa mengakses Internet**, tetapi **tidak bisa berkomunikasi satu sama lain** — meskipun secara fisik terhubung ke switch yang sama.

### 5.3 VLAN vs Subnetting

Keduanya sama-sama memisahkan jaringan, tetapi bekerja di lapisan yang berbeda.

| Aspek | Subnetting | VLAN |
|---|---|---|
| Layer | Layer 3 (Network) | Layer 2 (Data Link) |
| Diatur oleh | Router | Switch |
| Basis pemisahan | Rentang IP address | Konfigurasi port switch |
| Tujuan utama | Efisiensi pengalamatan IP | Keamanan dan isolasi traffic |

Dalam jaringan nyata, **keduanya digunakan bersama** — VLAN memisahkan di level switch, subnetting memisahkan di level IP, dan komunikasi antar VLAN/subnet dikelola oleh router atau Layer 3 switch.

---

## 6. Glossary — Istilah Penting

**Port Forwarding** — mekanisme di router untuk meneruskan traffic dari IP publik ke server internal di jaringan lokal

**Intranet** — jaringan internal yang hanya dapat diakses dari dalam jaringan lokal itu sendiri

**Packet Inspection** — proses firewall memeriksa isi, asal, dan tujuan setiap paket

**Stateful** — jenis firewall yang mengevaluasi keseluruhan koneksi secara dinamis

**Stateless** — jenis firewall yang mengevaluasi paket individual menggunakan aturan statis

**VPN** — teknologi yang menciptakan tunnel privat terenkripsi antar perangkat di jaringan berbeda

**Tunnel** — jalur privat terenkripsi yang dibuat oleh VPN melalui jaringan publik

**Sniffing** — tindakan menyadap dan membaca data yang melintas di jaringan

**PPP** — protokol dasar VPN untuk autentikasi dan enkripsi; non-routable

**PPTP** — protokol tunneling yang membawa data PPP keluar dari jaringan; enkripsi lemah

**IPSec** — protokol keamanan berbasis IP dengan enkripsi kuat; konfigurasi kompleks

**Routing** — proses menentukan dan mengikuti jalur optimal untuk pengiriman data antar jaringan

**Router** — perangkat Layer 3 yang menghubungkan jaringan berbeda dan melakukan routing

**Switch** — perangkat yang menghubungkan banyak perangkat dalam satu jaringan lokal

**Layer 2 Switch** — switch yang mengirimkan frames berdasarkan MAC address

**Layer 3 Switch** — switch yang bisa mengirim frames sekaligus melakukan routing IP

**Frame** — unit data di Layer 2 yang mengenkapsulasi paket IP di dalamnya

**MAC Address** — identitas unik perangkat di Level Layer 2; dipakai switch untuk pengiriman frames

**VLAN** — teknologi pemisahan virtual perangkat dalam satu switch fisik menjadi beberapa segmen

**Subnetting** — pembagian jaringan berdasarkan rentang IP address yang dikelola router

**DDoS** — Distributed Denial-of-Service; serangan banjir traffic dari banyak sumber sekaligus

**ISP** — Internet Service Provider; penyedia layanan internet

**Non-routable** — tidak bisa berpindah jaringan secara mandiri; membutuhkan protokol lain

---

## 7. Tools & Platform Rujukan

**Snort**
Intrusion Detection/Prevention System (IDS/IPS) open-source yang dapat difungsikan sebagai software firewall. Disebutkan sebagai contoh implementasi firewall berbasis software.
URL: https://www.snort.org

---

## 8. Catatan Ringkas untuk Ditulis Tangan

### Port Forwarding

- Port forwarding — buka akses server internal ke internet lewat IP publik
- Dikonfigurasi di — ROUTER (bukan di server)
- Port forwarding ≠ firewall — PF buka pintu, firewall jaga pintu
- Alur: 192.168.1.10:80 → router (IP publik):80 → internet → user luar

### Firewall

- Firewall — tentukan traffic mana yang boleh masuk/keluar jaringan
- 4 faktor evaluasi — source, destination, port, protocol
- Cara kerja — packet inspection
- Bentuk — hardware dedicated, software (Snort), residential router

- Stateful — evaluasi seluruh koneksi, dinamis, resource tinggi
- Stateful — host buruk = seluruh perangkat diblokir
- Stateless — evaluasi paket individual, aturan statis, resource rendah
- Stateless — paket buruk ≠ blokir seluruh perangkat
- Stateless — unggul lawan DDoS / traffic massal

### VPN

- VPN — buat tunnel privat terenkripsi antar perangkat di jaringan berbeda
- Tunnel — jalur privat yang melintas di atas internet
- Manfaat 1 — koneksi lintas lokasi geografis
- Manfaat 2 — privasi (enkripsi, anti sniffing)
- Manfaat 3 — anonimitas (sembunyikan traffic dari ISP)
- Catatan anonimitas — level anonimitas = seberapa jujur provider VPN-nya

- PPP — autentikasi + enkripsi, pakai private key & public cert, NON-ROUTABLE
- PPTP — bawa PPP keluar jaringan, mudah setup, enkripsi LEMAH
- IPSec — enkripsi pakai framework IP, setup SULIT, enkripsi KUAT

### Router

- Router — hubungkan jaringan BERBEDA, teruskan data antar jaringan
- Layer — Layer 3 OSI (Network Layer)
- Routing — proses cari jalur optimal antar jaringan
- Faktor jalur — paling pendek, paling andal, medium paling cepat
- Router ≠ switch — fungsinya dedicated, tidak tumpang tindih

### Switch

- Switch — hubungkan banyak perangkat dalam satu jaringan lokal (3–63 perangkat)
- Layer 2 Switch — kirim frames pakai MAC address, tidak bisa routing IP
- Layer 3 Switch — kirim frames + routing IP, punya banyak IP address
- Layer 2 ≠ Layer 3 — satu switch tidak bisa keduanya sekaligus

### VLAN

- VLAN — Virtual Local Area Network, pisahkan perangkat secara virtual dalam satu switch fisik
- Tujuan — keamanan & isolasi traffic antar segmen/departemen
- Contoh: VLAN 1 Sales (192.168.1.1) & VLAN 2 Accounting (192.168.2.1) — satu switch, tidak bisa saling komunikasi
- VLAN = layer 2, diatur switch | Subnetting = layer 3, diatur router
- Di jaringan nyata — VLAN + subnetting dipakai BERSAMAAN
