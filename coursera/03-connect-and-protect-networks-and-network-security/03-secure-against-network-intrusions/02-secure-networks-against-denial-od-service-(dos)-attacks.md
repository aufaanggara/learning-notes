# Resume: Secure Networks Against Denial of Service (DoS) Attacks

Tanggal: 1 November 2026

Room ini mencakup video pengantar DoS/DDoS, reading tentang tcpdump, reading studi kasus serangan DDoS 2016, activity analisis komunikasi network layer (DNS dan ICMP), exemplar activity, dan quiz.

## 1. Konsep Inti DoS dan DDoS

### 1.1 Denial of Service (DoS)

**Denial of Service (DoS)** adalah serangan yang menargetkan jaringan atau server dengan membanjirinya dengan traffic jaringan. Tujuannya mengganggu operasi bisnis normal dengan membebani jaringan organisasi secara berlebihan.

Sasaran teknisnya adalah mengirim informasi sebanyak mungkin ke sebuah perangkat jaringan sampai perangkat itu crash atau tidak mampu merespons pengguna yang sah.

Dampaknya:

- Organisasi tidak dapat menjalankan operasi bisnis normal, sehingga merugikan waktu dan uang.
- Jaringan yang crash membuat organisasi rentan terhadap ancaman dan serangan keamanan lain.
- Reputasi organisasi dapat turun apabila berita serangan yang berhasil sampai ke konsumen, karena konsumen mulai mempertanyakan keamanannya.

### 1.2 Distributed Denial of Service (DDoS)

**DDoS** adalah jenis DoS yang memakai banyak perangkat atau server dari lokasi berbeda untuk membanjiri jaringan target. Banyaknya perangkat membuat total traffic lebih mungkin melampaui kapasitas server target.

DDoS jenis **volumetric** membebani jaringan dengan paket data yang tidak diinginkan dalam jumlah sangat besar sampai server tidak mampu melayani pengguna normal.

### 1.3 Prinsip: bagian mana pun yang kewalahan berarti serangan berhasil

DoS berarti denial of service, sehingga tidak penting bagian jaringan mana yang dibebani. Jika salah satu bagian berhasil dibuat kewalahan, penyerang menang.

Contoh: seorang penyerang menyusun satu paket dengan sangat cermat sehingga router menghabiskan waktu ekstra untuk memprosesnya. Volume traffic keseluruhan tidak membebani router; yang membebani adalah isi spesifik paket itu sendiri.

## 2. Serangan DoS Level Jaringan

Serangan network-level menargetkan **bandwidth jaringan** untuk memperlambat traffic. Ada tiga yang dibahas: SYN flood, ICMP flood, dan ping of death.

### 2.1 SYN Flood Attack

**SYN flood** adalah serangan DoS yang menyamar sebagai koneksi TCP dan membanjiri server dengan paket SYN, meniru langkah pertama TCP handshake.

Alur TCP handshake normal:

1. Perangkat mengirim permintaan **SYN** (synchronize) ke server.
2. Server membalas dengan paket **SYN/ACK** untuk mengakui permintaan, lalu membiarkan sebuah port terbuka untuk langkah terakhir.
3. Perangkat mengirim paket **ACK** terakhir, dan koneksi TCP terbentuk.

Penyerang memanfaatkan protokol ini dengan membanjiri server dengan permintaan SYN pada langkah pertama. Jika jumlah permintaan SYN melebihi jumlah port yang tersedia di server, server kewalahan dan tidak dapat berfungsi.

### 2.2 ICMP dan ICMP Flood

**ICMP (Internet Control Message Protocol)** adalah protokol internet yang dipakai perangkat untuk saling memberi tahu tentang error transmisi data di jaringan.

> *ICMP dapat dibayangkan seperti permintaan status update ke sebuah perangkat. Perangkat mengembalikan pesan error jika ada masalah jaringan, mirip permintaan ICMP yang "mengecek" perangkat untuk memastikan semuanya baik-baik saja.*

**ICMP flood** adalah serangan DoS di mana penyerang berulang kali mengirim paket ICMP ke server jaringan. Server dipaksa membalas dengan paket ICMP, sehingga seluruh bandwidth masuk dan keluar habis dan server crash.

### 2.3 Ping of Death

**Ping of death** adalah serangan DoS di mana penyerang melakukan ping ke sistem dengan paket ICMP yang lebih besar dari 64 kilobyte, yaitu ukuran maksimum paket ICMP yang terbentuk dengan benar. Server jaringan yang rentan akan kelebihan beban dan crash.

> *Seperti menjatuhkan batu besar ke gundukan sarang semut. Tiap semut hanya sanggup membawa beban tertentu saat mengangkut makanan, tetapi satu batu besar menindas banyak semut sekaligus, dan koloni tidak berfungsi sampai membangun ulang operasinya di tempat lain.*

### 2.4 Perbandingan Singkat

Dua pola serangan yang dibedakan di materi:

- **SYN flood dan ICMP flood** memanfaatkan protokol komunikasi dengan mengirim permintaan dalam jumlah sangat besar.
- **Ping of death** membebani server dengan satu permintaan berukuran besar.

## 3. Network Protocol Analyzer dan tcpdump

### 3.1 Network Protocol Analyzer

**Network protocol analyzer** (disebut juga **packet sniffer** atau packet analyzer) adalah tool untuk menangkap dan menganalisis traffic data dalam jaringan. Umumnya dipakai sebagai alat investigasi untuk memantau jaringan dan mengidentifikasi aktivitas mencurigakan.

Contoh analyzer yang umum:

- SolarWinds NetFlow Traffic Analyzer
- ManageEngine OpManager
- Azure Network Watcher
- Wireshark
- tcpdump

Materi memfokuskan tcpdump, tetapi konsep yang dipelajari berlaku untuk analyzer lain.

### 3.2 Karakteristik tcpdump

**tcpdump** adalah network protocol analyzer berbasis command-line.

- Ringan (lightweight): memakai sedikit memori dan CPU.
- Menggunakan library open-source **libpcap**.
- Berbasis teks: semua command dijalankan di terminal.
- Preinstalled di banyak distribusi Linux, dan dapat dipasang di sistem Unix-based lain seperti macOS.
- Mengonversi informasi penting traffic menjadi format yang mudah dibaca manusia dan mencetak informasi tiap paket langsung ke terminal.
- Menampilkan source IP, destination IP, dan nomor port yang dipakai.

Output dicetak sebagai sniffed packets di command line, dan opsional dapat disimpan ke log file. Materi tidak membahas switch/flag tcpdump secara spesifik; yang dibahas adalah cara membaca output.

### 3.3 Membaca Output tcpdump

Contoh baris output dari materi:

```
20:00:29.538395 IP 198.168.10.1.41 > 198.111.123.1.61012: Flags [P.], seq 120:176, ack 1, win 501, options [nop,nop,TS val 4106659748 ecr 2979487360], length 144
```

Bagian-bagian yang dijelaskan:

- **Timestamp** (`20:00:29.538395`): jam, menit, detik, dan pecahan detik.
- **Source IP** (`198.168.10.1`): asal paket.
- **Source port** (`41`): port tempat paket berasal.
- **Destination IP** (`198.111.123.1`): tujuan pengiriman paket.
- **Destination port** (`61012`): port tujuan paket.

Catatan: secara default tcpdump mencoba me-resolve alamat host menjadi hostname, dan mengganti nomor port dengan nama service yang umum memakai port tersebut (contohnya port 53 tampil sebagai `domain`).

### 3.4 Kegunaan Umum dan Penyalahgunaan

Network protocol analyzer dipakai untuk menangkap dan melihat komunikasi jaringan, mengumpulkan statistik, dan troubleshooting performa jaringan. Kegunaan lain:

- Menetapkan baseline pola traffic dan metrik utilisasi jaringan.
- Mendeteksi dan mengidentifikasi traffic berbahaya.
- Membuat alert custom saat ada masalah jaringan atau ancaman keamanan.
- Menemukan instant messaging (IM), traffic, atau wireless access point yang tidak sah.

Penyerang juga dapat menyalahgunakan analyzer untuk menangkap paket berisi informasi sensitif seperti username dan password akun.

## 4. Studi Kasus: Serangan DDoS terhadap Penyedia DNS (2016)

### 4.1 Latar Belakang

Server DNS menerjemahkan nama domain website menjadi IP address numerik dari server yang menyimpan website tersebut. Pada hari serangan, banyak perusahaan besar memakai satu **DNS service provider** yang meng-hosting sistem DNS mereka. Pada 21 Oktober 2016, penyedia itu menjadi korban DDoS.

### 4.2 Persiapan Serangan: Botnet

Sebelum serangan, sekelompok mahasiswa membuat **botnet** dengan niat menyerang server dan jaringan gaming. Botnet adalah kumpulan komputer terinfeksi malware yang dikendalikan satu threat actor yang disebut **bot-herder**. Setiap komputer dalam botnet dapat dikendalikan jarak jauh untuk mengirim paket data ke sistem target. Pada serangan botnet, semua bot diperintahkan mengirim paket secara bersamaan sehingga terjadi DDoS.

Para mahasiswa memposting kode botnet secara online agar dapat diakses ribuan pengguna dan pihak berwenang tidak dapat melacaknya kembali ke mereka. Akibatnya, aktor jahat lain, termasuk pelaku serangan ke penyedia DNS, ikut mempelajari dan mengendalikan botnet tersebut.

### 4.3 Hari Serangan

- Pukul 07:00, botnet mengirim puluhan juta permintaan DNS ke penyedia layanan.
- Sistem kewalahan dan layanan DNS shut down, sehingga semua website pengguna penyedia itu tidak dapat dijangkau (pengguna tidak diarahkan ke website yang diketik).
- Outage terjadi di seluruh Amerika Utara dan Eropa.
- Sistem pulih setelah dua jam downtime.
- Gelombang serangan botnet susulan dapat dimitigasi karena perusahaan DNS sudah siap.

### 4.4 Pelajaran

Operasi penting yang terdistribusi di berbagai host dan dapat di-scale secara dinamis tetap dapat berjalan meski infrastruktur host baseline offline. Security analyst perlu menyadari keseriusan DDoS karena ada tindakan konkret untuk melindungi organisasi; strategi mitigasi dibahas pada materi selanjutnya.

## 5. Activity: Analisis Komunikasi Network Layer (DNS dan ICMP)

### 5.1 Tujuan

Menganalisis traffic DNS dan ICMP in transit dari data network protocol analyzer untuk mengidentifikasi protokol yang terdampak dalam sebuah insiden. Pada internet layer model TCP/IP, IP memformat data menjadi **IP datagram**; informasi dalam datagram membantu analyst melihat paket yang mencurigakan.

### 5.2 Skenario

Pelanggan klien tidak dapat membuka `www.yummyrecipesforme.com` dan menerima error "destination port unreachable". Saat memuat halaman, browser mengirim query ke server DNS melalui **UDP** untuk mendapatkan IP address domain, lalu memakai IP itu sebagai tujuan HTTPS request. Analyzer (tcpdump) menunjukkan setiap paket UDP ke server DNS dibalas paket **ICMP** berisi pesan "udp port 53 unreachable".

### 5.3 Pembacaan Log

Baris kunci:

```
13:24:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A? yummyrecipesforme.com. (24)
13:24:36.098564 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2 udp port 53 unreachable length 254
```

Interpretasi:

- Dua baris pertama: request keluar dari komputer ke server DNS (paket UDP) untuk meminta IP address domain.
- Dua baris berikutnya: respons berupa pesan error ICMP bahwa paket UDP tidak dapat dikirim ke port 53 server DNS.
- Source berada di kiri simbol `>`, destination di kanan. Pada request: `192.51.100.15` (komputer) ke `203.0.113.2.domain` (server DNS). Pada error ICMP arahnya dibalik: dari `203.0.113.2` ke `192.51.100.15`.
- `35084` adalah query identification number; tanda `+` menandakan ada flag terkait pesan UDP; `A?` adalah flag permintaan **A record** (memetakan domain name ke IP address).
- Port 53 adalah port layanan DNS. "unreachable" berarti query tidak sampai karena tidak ada service yang listening pada port DNS penerima.
- Pola yang sama (UDP lalu ICMP unreachable) berulang dua kali lagi pada log, dengan error identik.

### 5.4 Incident Report

Aktivitas dikerjakan dengan template **cybersecurity incident report**, terbagi dua bagian.

Bagian 1, ringkasan masalah dari log tcpdump:

- Ringkasan analisis dan protokol yang dipakai pada traffic.
- Detail yang ditunjukkan log.
- Interpretasi masalah.

Bagian 2, analisis dan satu solusi:

- Kapan masalah pertama dilaporkan.
- Skenario, kejadian, dan gejala saat pertama dilaporkan.
- Status terkini masalah.
- Informasi yang ditemukan selama investigasi.
- Langkah troubleshooting selanjutnya.
- Dugaan root cause.

Insiden ini sedang ditangani security engineer setelah dilaporkan ke supervisor; analyst di tahap ini hanya menganalisis situasi.

### 5.5 Exemplar

Exemplar disediakan untuk membandingkan hasil kerja: apa yang sudah baik dan apa yang perlu ditingkatkan. Exemplar hanyalah satu pendekatan; dalam praktik, analyst dan tim membuat best guess lalu menyelidiki lebih lanjut. Dasarnya: kemampuan menganalisis paket IP membantu menebak apa yang terjadi atau mencegah serangan masuk, dengan network protocol dan traffic log sebagai titik awal investigasi.

## 6. Ringkasan Hasil Quiz

Konsep yang diuji dan jawabannya:

- Serangan dari banyak perangkat/server di lokasi berbeda: **DDoS**.
- Serangan yang meniru koneksi TCP dan membanjiri server dengan paket langkah pertama handshake: **SYN flood**.
- Serangan dengan paket ICMP lebih besar dari 64KB: **Ping of Death**.
- Serangan yang memanfaatkan protokol komunikasi dengan permintaan berlebihan (pilih semua): **ICMP flood** dan **SYN flood**. "TCP connection attack" dan "Tailgating attack" bukan jawaban yang benar.

## 7. Glossary

- **A record**: record DNS yang memetakan domain name ke IP address.
- **Bot-herder**: threat actor yang mengendalikan botnet.
- **Botnet**: kumpulan komputer terinfeksi malware di bawah kendali satu threat actor.
- **DDoS**: DoS yang memakai banyak perangkat dari lokasi berbeda.
- **DNS service provider**: penyedia yang meng-hosting sistem DNS bagi banyak organisasi.
- **DoS**: serangan yang membanjiri jaringan atau server sampai tidak dapat melayani pengguna sah.
- **ICMP**: protokol untuk melaporkan error transmisi data antar perangkat.
- **ICMP flood**: pengiriman paket ICMP berulang untuk menghabiskan bandwidth dan membuat server crash.
- **IP datagram**: format paket data pada internet layer TCP/IP.
- **libpcap**: library open-source yang dipakai tcpdump.
- **Network protocol analyzer**: tool untuk menangkap dan menganalisis traffic jaringan; disebut juga packet sniffer.
- **Ping of death**: serangan dengan paket ICMP lebih besar dari 64KB.
- **SYN flood**: serangan yang membanjiri server dengan paket SYN pada langkah pertama TCP handshake.
- **SYN/ACK**: balasan server atas SYN dalam TCP handshake.
- **tcpdump**: network protocol analyzer berbasis command-line.
- **UDP**: protokol yang dipakai browser untuk mengirim query DNS.
- **Volumetric DDoS**: DDoS yang membebani jaringan dengan volume paket yang sangat besar.

## 8. Tools & Platform Rujukan

Tidak ada URL eksplisit pada materi; hanya nama tool yang disebutkan.

- **tcpdump**: network protocol analyzer berbasis command-line.
- **Wireshark**: network protocol analyzer untuk menangkap dan menganalisis traffic.
- **SolarWinds NetFlow Traffic Analyzer**: network protocol analyzer.
- **ManageEngine OpManager**: network protocol analyzer.
- **Azure Network Watcher**: network protocol analyzer.
- **Cybersecurity incident report template**: template (Google Docs/DOCX) untuk menulis laporan insiden.
- **Example of a Cybersecurity Incident Report**: contoh laporan sebagai referensi.
- **Cybersecurity incident report exemplar dan exemplar explained**: jawaban contoh lengkap beserta penjelasannya.

## 9. Catatan Ringkas untuk Ditulis Tangan

### 9.1 DoS dan DDoS

- DoS — banjiri jaringan/server dengan traffic sampai crash atau tidak melayani user sah
- Dampak DoS — rugi waktu dan uang, rentan serangan lain, reputasi turun
- DDoS — DoS dari banyak perangkat di banyak lokasi
- Volumetric DDoS — paket tak diinginkan dalam jumlah sangat besar
- Prinsip — bagian mana pun yang kewalahan berarti serangan berhasil
- Contoh router — paket dibuat cermat sehingga waktu proses router bertambah, bukan karena volume

### 9.2 Serangan Network-Level

- Target — bandwidth jaringan
- TCP handshake — SYN, lalu SYN/ACK, lalu ACK
- SYN flood — banjir SYN di langkah 1, port habis, server kewalahan
- ICMP — protokol pelapor error transmisi data
- ICMP flood — ICMP berulang, server wajib membalas, bandwidth habis, crash
- Ping of death — satu paket ICMP lebih dari 64KB, maksimum ukuran valid
- SYN flood dan ICMP flood — banyak permintaan; ping of death — satu permintaan besar
- Analogi — batu besar jatuh ke sarang semut

### 9.3 Packet Analyzer dan tcpdump

- Network protocol analyzer — packet sniffer, tangkap dan analisis traffic
- Contoh — SolarWinds NetFlow, ManageEngine OpManager, Azure Network Watcher, Wireshark, tcpdump
- tcpdump — CLI, ringan, libpcap, preinstalled di Linux, ada di macOS
- Output — timestamp, source IP dan port, destination IP dan port
- Simbol > — kiri source, kanan destination
- Default — resolve hostname dan nama service
- Kegunaan — baseline, deteksi traffic berbahaya, alert custom, temukan IM/traffic/wireless AP tidak sah
- Penyalahgunaan — penyerang sniff username dan password

### 9.4 Studi Kasus DDoS 2016

- Target — DNS service provider, 21 Oktober 2016
- DNS — nama domain ke IP address
- Botnet — komputer terinfeksi malware, dikendalikan bot-herder
- Asal botnet — kode diposting mahasiswa, dipakai aktor lain
- Serangan — 07:00, puluhan juta request DNS, DNS shut down
- Dampak — website tak terjangkau, outage Amerika Utara dan Eropa
- Pemulihan — 2 jam, serangan susulan dimitigasi
- Pelajaran — operasi terdistribusi dan scalable tetap jalan saat host baseline offline

### 9.5 Activity DNS dan ICMP

- Gejala — error "destination port unreachable" di yummyrecipesforme.com
- Alur — browser kirim query DNS via UDP, lalu HTTPS ke web server
- Log — UDP ke server DNS dibalas ICMP "udp port 53 unreachable"
- Port 53 — port layanan DNS
- A? — flag request A record
- 35084+ — query ID, plus menandakan flag
- Unreachable — tidak ada service listening di port DNS
- Pola — error identik berulang 3 kali
- Report bagian 1 — ringkasan masalah, protokol, detail, interpretasi
- Report bagian 2 — kapan dilaporkan, gejala, status, temuan, langkah lanjut, root cause
- Exemplar — satu pendekatan; analyst membuat best guess lalu investigasi lanjut

### 9.6 Quiz

- Banyak perangkat/lokasi — DDoS
- Meniru langkah 1 handshake — SYN flood
- ICMP lebih dari 64KB — Ping of Death
- Pilih semua (protokol komunikasi dibanjiri request) — ICMP flood dan SYN flood
