# Resume: Network Traffic Analysis
**Tanggal:** 8 September 2026

---

## 1. Definisi dan Tujuan Network Traffic Analysis (NTA)

### 1.1 Apa itu NTA

**Network Traffic Analysis (NTA)** adalah proses menangkap (capture), memeriksa (inspect), dan menganalisis data saat mengalir di jaringan, dengan tujuan mendapat visibilitas penuh atas apa yang dikomunikasikan baik di dalam maupun keluar jaringan.

NTA **bukan sinonim dari Wireshark**. NTA lebih luas: gabungan dari korelasi berbagai log, deep packet inspection, dan statistik network flow, dengan tujuan-tujuan spesifik.

Skill ini penting bukan hanya untuk SOC L1 analyst, tapi juga untuk peran blue team dan red team lainnya — untuk menavigasi informasi jaringan dan membedakan mana yang normal (baseline) dan mana yang menyimpang.

### 1.2 Kenapa NTA Dibutuhkan

Firewall dan perangkat lain umumnya hanya mencatat **metadata** dari sebuah koneksi (query, response, IP, port), bukan **isi/konten**-nya. Threat actor memanfaatkan celah ini untuk menyembunyikan aktivitas berbahaya di dalam traffic yang terlihat normal di level log.

Contoh nyata: log DNS di firewall hanya menunjukkan query mencurigakan menuju domain tertentu, tapi baru ketahuan mengandung perintah **Command and Control (C2)** setelah isi paket DNS-nya (TXT record) diperiksa langsung — teknik ini disebut **DNS tunneling**.

Secara umum, NTA digunakan untuk:

- Memonitor performa jaringan.
- Mendeteksi keanehan (lonjakan performa mendadak, jaringan melambat).
- Memeriksa isi komunikasi mencurigakan, baik internal maupun eksternal (eksfiltrasi via DNS, download file berbahaya via HTTP, lateral movement).

Dari sudut pandang SOC, NTA membantu:

- Mendeteksi aktivitas mencurigakan/berbahaya.
- Merekonstruksi serangan saat incident response.
- Memverifikasi dan memvalidasi alert.

---

## 2. Apa yang Bisa Diamati (Model TCP/IP)

Traffic jaringan diamati berdasarkan arsitektur **TCP/IP stack**, yang punya 4 layer. Setiap layer menambahkan header-nya sendiri ke data dari layer di atasnya sebelum diteruskan. Log biasanya hanya mencatat sebagian kecil dari header-header ini, tidak pernah detail paket secara penuh — inilah alasan NTA dibutuhkan.

### 2.1 Application Layer

Terdiri dari dua struktur: application header dan application data (payload). Contoh HTTP: web proxy/firewall biasanya mencatat header request/response (misalnya nama file yang diminta, status code), tapi **tidak mencatat isi payload/body**-nya. Ini yang membuat file berbahaya bisa lolos dari log meski request-nya tercatat.

### 2.2 Transport Layer

Data disegmentasi dan dienkapsulasi dengan header transport (umumnya TCP atau UDP). Log firewall biasanya mencatat port sumber/tujuan dan flags, tapi tidak semua field.

Field **sequence number** krusial untuk mendeteksi **session hijacking** — jika sequence number tiba-tiba melompat jauh dari yang seharusnya, itu indikasi ada paket asing yang mencoba menyisip ke sesi TCP yang sedang berjalan.

### 2.3 Internet Layer

Menambahkan header IP. Jika segmen lebih besar dari **MTU (Maximum Transmission Unit)**, data dipecah menjadi fragmen. Field yang umum dicatat: source/destination IP dan TTL.

Untuk mendeteksi **fragmentation attack**, perlu diperiksa field fragment offset dan total length. Salah satu variasinya adalah **overlapping byte ranges**: attacker membuat fragmen dengan offset yang tumpang tindih sehingga paket bisa disusun ulang (reassembly) dengan lebih dari satu cara — teknik ini bisa dipakai untuk melewati IDS.

### 2.4 Link Layer

Menambahkan header dengan informasi pengalamatan MAC address. Untuk serangan seperti **ARP poisoning/spoofing**, log biasa tidak cukup — dibutuhkan full packet capture beserta konteksnya, misalnya untuk mendeteksi satu MAC address yang membalas banyak ARP request dari IP berbeda (gratuitous ARP yang mencurigakan).

---

## 3. Sumber dan Alur Traffic Jaringan

### 3.1 Klasifikasi Sumber Traffic

**Intermediary sources** — perangkat yang dilalui traffic (bukan penghasil utama). Contoh: firewall, switch, web proxy, IDS/IPS, router, access point, wireless LAN controller, infrastruktur ISP. Traffic yang dihasilkan perangkat ini berasal dari layanan routing (EIGRP, OSPF, BGP), management (SNMP, PING), logging (SYSLOG), dan protokol pendukung (ARP, STP, DHCP). Volume traffic yang dihasilkan jauh lebih kecil dibanding endpoint.

**Endpoint sources** — perangkat tempat traffic **berasal dan berakhir**, dan menyumbang **porsi terbesar (bulk)** dari bandwidth jaringan. Contoh: server, host, perangkat IoT, printer, mesin lab, ponsel, tablet.

### 3.2 Klasifikasi Flow Traffic

**North-South Traffic**: traffic yang keluar/masuk LAN melewati firewall (menuju WAN). Lebih sering dimonitor ketat karena melewati firewall dengan satu atau lain cara. Contoh protokol client-server: HTTPS, DNS, SSH, VPN, SMTP, RDP. Setiap protokol punya dua arah: ingress (masuk) dan egress (keluar). Konfigurasi firewall rule dan logging yang tepat adalah kunci visibilitas di sini.

**East-West Traffic**: traffic yang tetap berada di dalam LAN korporat, sehingga lebih jarang dimonitor. Justru di sinilah attacker sering melakukan **lateral movement** setelah kompromi awal. Kategori layanan di dalamnya:

- **Directory, Authentication & Identity Services** — Kerberos/LDAP (autentikasi ke Active Directory), RADIUS/TACACS+ (kontrol akses jaringan), Certificate Authority.
- **File shares & print services** — SMB/CIFS (network drive), IPP/LPD (printing jaringan).
- **Router, switching, dan infrastructure services** — traffic DHCP, broadcast ARP, internal DNS, pesan routing protocol.
- **Application Communication** — koneksi database (SQL over TCP), microservices API (REST/gRPC).
- **Backup & Replication** — file replication antar data center, database replication (MySQL binlog, PostgreSQL streaming).
- **Monitoring & Management** — SNMP (metrik kesehatan perangkat), Syslog (logging terpusat), NetFlow/IPFIX (telemetri flow), log endpoint ke central logging server.

### 3.3 Contoh Flow: HTTPS dengan TLS Inspection

Host melakukan request ke website → request dikirim ke **NGFW** yang di dalamnya termasuk **web proxy** → web proxy bertindak sebagai web server bagi client, sekaligus membangun sesi TCP baru dengan web server sesungguhnya → proxy memeriksa isi response dari web server, lalu meneruskannya ke host jika aman.

Hasilnya ada **dua sesi TLS terpisah**: satu antara client-proxy, satu lagi antara proxy-webserver. Dari sudut pandang client, seolah ia terhubung langsung ke web server.

### 3.4 Contoh Flow: External DNS

Host mengirim DNS query ke **internal DNS server** (port 53) → DNS server cek cache-nya dulu → jika tidak ada, query diteruskan melalui router, melewati firewall, ke DNS server eksternal yang dikonfigurasi → jawaban kembali melalui jalur yang sama ke internal DNS server → diteruskan ke host.

### 3.5 Contoh Flow: SMB dengan Kerberos

Ketika host membuka network share, sesi SMB dibangun melalui tahapan berikut:

1. User login di host → autentikasi dilakukan via protokol **Kerberos** dengan **Key Distribution Center (KDC)** di Domain Controller → host menerima **Ticket Granting Ticket (TGT)**.
2. Host menggunakan TGT untuk meminta **service ticket** khusus akses SMB.
3. Service ticket dipakai untuk membangun sesi SMB ke file server, lalu host bisa mengakses share.

> Analogi: seperti masuk ke gedung perkantoran dengan banyak ruangan terkunci. Alih-alih menunjukkan KTP ke setiap pintu, cukup menunjukkan KTP sekali ke resepsionis (KDC) di lobi, lalu dapat kartu akses sementara (TGT). Setiap mau masuk ruangan tertentu, kartu itu ditukar dengan kartu akses spesifik ruangan (service ticket) tanpa perlu menunjukkan KTP lagi.

---

## 4. Cara Mengamati Traffic Jaringan

Tiga cara utama mendapatkan sumber informasi jaringan: **Logs**, **Full Packet Capture**, dan **Network Statistics**.

### 4.1 Logs

Pintu masuk pertama untuk melihat aktivitas jaringan. Tidak ada standar universal untuk logging — setiap vendor mengimplementasikan caranya sendiri (contoh: Microsoft dengan Windows Event Logs). Kebanyakan vendor hanya mencatat field tertentu (source/destination IP), bukan paket penuh.

Beberapa protokol menawarkan standar pengiriman log ke collector, misalnya **Syslog** dan **SNMP**. Ketika log tidak cukup informatif, perlu dikorelasikan dengan full packet capture dan network statistics.

### 4.2 Full Packet Capture

Cara menangkap paket secara penuh, ada dua opsi:

**Network Tap** — perangkat fisik yang dipasang inline di jaringan, membuat salinan seluruh traffic yang lewat tanpa mempengaruhi performa. Beroperasi hanya di **link layer**, tidak butuh MAC/IP address karena menyalin sinyal elektrik/cahaya langsung. Nyaris tidak menambah delay ke jaringan.

**Port Mirroring** — pendekatan berbasis software untuk menyalin paket dari satu port perangkat intermediary ke port lain (menuju IDS, packet capture box, dll). Nama implementasinya berbeda tiap vendor; Cisco menyebutnya **SPAN (Switched Port Analyzer)**.

Contoh konfigurasi SPAN di Cisco:

```
Switch(config)# monitor session 1 source interface fastEthernet0/1
Switch(config)# monitor session 1 destination interface fastEthernet0/2
```

- `monitor session 1 source interface fastEthernet0/1` — menetapkan `fastEthernet0/1` sebagai port sumber yang traffic-nya akan diduplikasi.
- `monitor session 1 destination interface fastEthernet0/2` — menetapkan `fastEthernet0/2` sebagai port tujuan tempat salinan traffic dikirim (biasanya terhubung ke monitoring device).

Port mirroring tidak selalu perlu perangkat fisik — bisa dikonfigurasi di perangkat virtual (VMware vSwitch) atau layanan cloud (AWS VPC Traffic Mirroring).

**Best practices full packet capture:**

- **Placement** — posisi TAP/mirror harus ditempatkan sesuai jenis traffic yang ingin ditangkap; posisi yang salah tidak akan menangkap traffic yang relevan.
- **Duration** — capture penuh butuh storage proporsional dengan bandwidth. Traffic 1 Gbps selama sehari penuh butuh rata-rata 10.8 TB storage.
- **Mirror vs TAP** — TAP fisik nyaris tidak mengurangi performa; port mirroring bisa mempengaruhi performa ketika volume traffic besar melewati port yang di-mirror.

**Tools analisis full packet capture:** Wireshark, TCPdump, IPS/IDS seperti Snort, Suricata, dan Zeek.

### 4.3 Network Statistics

Mengumpulkan metadata tentang traffic yang mengalir, tanpa menyimpan isi paket individual — berguna untuk menemukan anomali seperti traffic C2, eksfiltrasi data, dan lateral movement.

**NetFlow** — protokol dari Cisco, mengumpulkan metadata flow (source/destination IP, port, protokol, jumlah paket, byte, timestamp). Contoh field yang tercatat: IPv4 source/destination address, transport port, protocol, jumlah paket dan bytes, timestamp awal-akhir flow.

**IPFIX (Internet Protocol Flow Information Export)** — penerus NetFlow yang dirilis sebagai standar netral vendor oleh IETF (berkolaborasi dengan Cisco), menawarkan fitur serupa NetFlow dengan fleksibilitas lebih dalam konfigurasi field yang ditangkap.

Implementasi NetFlow/IPFIX tidak butuh infrastruktur atau server khusus — kebanyakan vendor sudah mengimplementasikannya secara standar; hanya perlu diaktifkan dan dikonfigurasi tujuan pengiriman metadata-nya. Banyak NGFW, IPS, dan IDS sudah punya kemampuan mengumpulkan dan menganalisis data flow ini.

---

## 5. Studi Kasus dari Room

### 5.1 DNS Tunneling dan Beaconing

Host `WIN-016` (IP `192.168.1.16`) mengirim banyak query DNS ke TLD yang sama dengan subdomain acak berbeda-beda — pola khas **DNS tunneling**. Log firewall hanya mencatat query/response, tidak isinya. Setelah isi paket DNS diperiksa, ditemukan **TXT record** berisi perintah **C2** yang dikirim dari domain seperti `cmd1.evilc2.com`.

Insight: firewall/log standar tidak akan pernah menunjukkan payload tersembunyi di dalam record DNS — hanya full packet inspection yang bisa mengungkapnya.

### 5.2 Malicious PowerShell Download (via Web Proxy)

Skenario: user klik link phishing, memicu HTTP request untuk download file PowerShell berbahaya. Titik capture paling efisien adalah **web proxy**, karena semua device di jaringan mengirim HTTP(S) request-nya ke proxy sebelum diteruskan ke internet — jadi satu titik ini sudah cukup menangkap seluruh traffic web yang keluar-masuk jaringan.

Indikator berbahaya ditemukan di response HTTP: `Content-Disposition: attachment; filename="install.ps1"` dengan `Content-Type: application/octet-stream`.

### 5.3 DNS Infiltration (C2 via TXT Record)

Skenario: workstation terkompromi, perintah C2 diselundupkan lewat DNS TXT record. Titik capture paling efisien adalah **DNS server internal**, karena device ini menangani semua query DNS eksternal atas nama host — seluruh traffic DNS eksternal pasti melewatinya.

Query menuju domain seperti `c2.tryhackrne.thn` dengan response berupa TXT record berisi data payload C2 yang tersembunyi.

---

## 6. Glossary

- **NTA (Network Traffic Analysis)** — proses menangkap, memeriksa, dan menganalisis data yang mengalir di jaringan untuk mendapat visibilitas penuh.
- **DNS Tunneling** — teknik menyembunyikan/menyelundupkan data (termasuk perintah C2) di dalam query dan response DNS, biasanya via record TXT.
- **C2 (Command and Control)** — infrastruktur yang digunakan attacker untuk mengendalikan sistem yang sudah dikompromikan.
- **MTU (Maximum Transmission Unit)** — ukuran maksimum sebuah paket yang bisa dikirim dalam satu segmen di jaringan.
- **Fragmentation Attack** — serangan yang memanfaatkan pemecahan paket menjadi fragmen kecil untuk mengelabui IDS atau mengacaukan proses reassembly.
- **Overlapping Byte Ranges** — teknik fragmentation attack di mana offset antar fragmen tumpang tindih sehingga paket bisa disusun ulang dengan lebih dari satu cara.
- **Session Hijacking** — pengambilalihan sesi TCP yang sedang berjalan, terdeteksi lewat lompatan tidak wajar pada sequence number.
- **ARP Poisoning/Spoofing** — serangan yang memanipulasi tabel ARP korban agar traffic diarahkan melalui attacker.
- **North-South Traffic** — traffic yang keluar/masuk LAN melewati firewall menuju WAN.
- **East-West Traffic** — traffic yang tetap berada di dalam LAN, sering menjadi jalur lateral movement.
- **Lateral Movement** — teknik attacker berpindah dari satu sistem ke sistem lain di dalam jaringan yang sudah dikompromikan.
- **Kerberos** — protokol autentikasi jaringan berbasis tiket, menghindari pengiriman password berulang ke setiap layanan.
- **KDC (Key Distribution Center)** — komponen di Domain Controller yang mengeluarkan Ticket Granting Ticket dan service ticket dalam proses Kerberos.
- **TGT (Ticket Granting Ticket)** — tiket awal yang didapat setelah autentikasi Kerberos berhasil, dipakai untuk meminta service ticket.
- **Network Tap** — perangkat fisik inline yang menyalin seluruh traffic jaringan tanpa mempengaruhi performa, beroperasi di link layer.
- **Port Mirroring / SPAN** — metode berbasis software untuk menyalin traffic dari satu port ke port monitoring lain.
- **NetFlow** — protokol Cisco untuk mengumpulkan metadata flow traffic jaringan.
- **IPFIX** — standar netral vendor pengganti NetFlow, dikembangkan IETF bersama Cisco dan vendor lain.
- **TLS (Transport Layer Security)** — protokol enkripsi komunikasi antara dua pihak (client-server) agar data tidak bisa dibaca/diubah pihak lain di tengah jalan.
- **CLF (Common Log Format)** — format standar log akses web server (digunakan Apache).

---

## 7. Tools & Platform Rujukan

- **Wireshark** — tool analisis full packet capture, untuk memeriksa detail paket secara mendalam.
- **TCPdump** — tool command-line untuk capture dan analisis traffic jaringan.
- **Snort** — IDS/IPS untuk mendeteksi aktivitas mencurigakan berdasarkan signature/rule.
- **Suricata** — IDS/IPS open-source dengan kemampuan deep packet inspection.
- **Zeek** — tool network security monitoring, fokus pada analisis traffic tingkat lanjut.
- **AbuseIPDB** — platform untuk memverifikasi apakah sebuah IP address ditandai sebagai berbahaya/abusive.
- **VirusTotal** — platform untuk mengecek reputasi domain, IP, atau file (apakah terindikasi malware/berbahaya).

---

## 8. Catatan Ringkas untuk Ditulis Tangan

**Definisi & Tujuan**
- NTA — capture + inspect + analyze traffic jaringan, bukan cuma Wireshark
- Kenapa NTA — log cuma catat metadata, bukan isi/payload
- DNS tunneling — sembunyikan data/C2 di query-response DNS (TXT record)
- Fungsi NTA — deteksi ancaman, rekonstruksi serangan, validasi alert

**Layer TCP/IP (atas ke bawah)**
- Application — header + payload; log cuma catat header, bukan isi payload
- Transport — TCP/UDP; sequence number → deteksi session hijacking
- Internet — IP header, TTL, MTU; fragmentation attack, overlapping byte ranges → evade IDS
- Link — MAC address; ARP poisoning/spoofing perlu full packet, bukan cuma log

**Sumber Traffic**
- Intermediary — firewall, switch, proxy, IDS/IPS, router; traffic kecil, cuma dilewati
- Endpoint — server, host, IoT, mobile; ambil bulk/porsi terbesar bandwidth

**Flow Traffic**
- North-South — LAN ke WAN, lewat firewall; HTTPS, DNS, SSH, VPN, SMTP, RDP
- East-West — dalam LAN, jalur lateral movement; kategori: Directory/Auth (Kerberos/LDAP, RADIUS), File share (SMB/CIFS), Infra (DHCP, ARP, internal DNS), App comm (SQL, REST/gRPC), Backup/replication, Monitoring (SNMP, Syslog, NetFlow)

**Flow Contoh**
- HTTPS via proxy — 2 sesi TLS terpisah: client-proxy dan proxy-webserver
- DNS eksternal — host → internal DNS server → cache atau forward ke luar via firewall
- SMB+Kerberos — login → auth ke KDC → dapat TGT → minta service ticket → sesi SMB terbentuk

**Cara Amati Traffic**
- Logs — tidak standar tiap vendor; Syslog & SNMP standar pengiriman log
- Full Packet Capture — Network Tap (fisik, inline, link layer, minim delay) vs Port Mirroring/SPAN (software, bisa pengaruhi performa)
- Command SPAN Cisco: `monitor session 1 source interface fastEthernet0/1` — set port sumber; `monitor session 1 destination interface fastEthernet0/2` — set port tujuan salinan traffic
- Best practice capture — placement tepat, storage besar (1Gbps sehari ≈ 10.8TB), TAP > mirror soal performa
- Tools capture — Wireshark, TCPdump, Snort/Suricata/Zeek
- Network Statistics — NetFlow (Cisco, metadata flow) vs IPFIX (standar terbuka, lebih fleksibel)

**Studi Kasus**
- DNS Tunneling/Beaconing — WIN-016, query subdomain acak ke TLD sama, TXT record simpan perintah C2
- Malicious PS Download — tap paling efisien di Web Proxy, flag di header Content-Disposition/install.ps1
- DNS Infiltration — tap paling efisien di DNS Server, flag di TXT record response c2.tryhackrne.thn

**Istilah Wajib Hapal**
- MTU — batas ukuran paket
- Fragmentation attack — pecah paket kecil, evade IDS
- Session hijacking — sequence number melompat jauh
- KDC — server pengeluar tiket Kerberos, di Domain Controller
- TGT — tiket awal Kerberos
- TLS — enkripsi transport layer
- CLF — format log Apache
