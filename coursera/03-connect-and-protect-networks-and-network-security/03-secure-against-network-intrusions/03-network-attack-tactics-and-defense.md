# Resume: Network Intrusion and Interception Attacks (21 Oktober 2026)

Cakupan: video Packet Sniffing, video IP Spoofing, reading Overview of Interception Tactics, flashcard Identify: Network Attacks, aktivitas Analyze Network Attacks beserta exemplar, dan quiz Network Interception Attack Tactics.

Catatan: room ini tidak memuat command atau switch/flag terminal. Satu-satunya tool yang disebut adalah Wireshark, dan penggunaannya dibahas secara konseptual.


## 1. Konsep Dasar

### 1.1 Struktur Data Packet

Setiap data packet terdiri dari dua bagian.

- **Header** berisi alamat IP pengirim dan penerima.
- **Body** berisi muatan data yang bisa bernilai tinggi: nama, tanggal lahir, pesan pribadi, informasi finansial, dan nomor kartu kredit.

Serangan di room ini menyasar packet yang sedang bergerak melintasi jaringan, sehingga disebut **interception attacks**.

### 1.2 Cara Kerja NIC

**Network Interface Card (NIC)** adalah hardware yang menghubungkan perangkat ke jaringan.

- Pada operasi standar, NIC membaca transmisi data. Jika packet memuat **MAC address** perangkat itu, NIC menerimanya dan meneruskannya ke perangkat untuk diproses sesuai protokol.
- Packet yang tidak ditujukan ke MAC address tersebut diabaikan.
- NIC bisa diatur ke **promiscuous mode**, yaitu menerima seluruh traffic di jaringan, termasuk packet yang bukan ditujukan untuk perangkat itu. Mode inilah yang memungkinkan sniffing.


## 2. Packet Sniffing

### 2.1 Definisi

**Packet sniffing** adalah praktik menangkap dan memeriksa data packet yang melintas di jaringan, menggunakan software tool maupun perangkat hardware.

### 2.2 Penggunaan Sah dan Penyalahgunaan

Penggunaan sah oleh security analyst: menganalisis dan menangkap packet saat menyelidiki insiden yang sedang berlangsung, serta debugging masalah jaringan.

Penyalahgunaan oleh malicious actor: melihat data yang tidak ditujukan kepada mereka. Analogi: membuka surat milik orang lain.

Alur serangan:

- Penyerang menyisipkan diri di tengah koneksi yang sah antara dua perangkat.
- Penyerang memata-matai setiap packet yang melewati perangkatnya.
- Tujuannya mencari informasi berharga di dalam packet.
- Di private network, software seperti **Wireshark** dapat dipakai untuk menangkap data dan menyimpannya untuk dipakai belakangan, termasuk untuk mengambil IP dan MAC address perangkat yang authorized.

### 2.3 Passive vs Active Packet Sniffing

**Passive packet sniffing**: packet hanya dibaca saat in transit, tanpa diubah. Karena semua traffic pada jaringan berbasis hub terlihat oleh setiap host, penyerang dapat melihat semua informasi keluar-masuk perangkat target.

Analogi: petugas pos yang berhak mengantar surat tetapi tidak berhak membaca isinya, lalu membacanya diam-diam.

**Active packet sniffing**: packet dimanipulasi saat in transit. Bentuknya antara lain menyisipkan internet protocol untuk mengalihkan packet ke port yang tidak semestinya, atau mengubah isi packet (misalnya mengganti nomor rekening penerima di body packet).

Analogi: tetangga yang menawarkan diri mengantar surat, lalu membaca atau mengubah isinya sebelum memasukkannya ke kotak surat. Walau surat tetap sampai ke rumah yang benar, tindakannya tetap jahat.

### 2.4 Pencegahan Malicious Packet Sniffing

- **VPN**: mengenkripsi data selama melintasi jaringan. Penyerang masih bisa mengganggu traffic, tetapi tidak dapat mendekode isinya. VPN adalah layanan keamanan jaringan yang mengubah public IP address dan menyembunyikan lokasi virtual agar data tetap privat di jaringan publik.
- **HTTPS**: pastikan website memakai HTTPS di awal alamat domain. HTTPS memakai SSL/TLS untuk mengenkripsi data dan mencegah eavesdropping.
- **Hindari unprotected WiFi** (kedai kopi, restoran, bandara). Jaringan ini tidak memakai enkripsi, sehingga siapa pun di jaringan dapat mengakses seluruh data keluar-masuk perangkat. Hindari WiFi publik gratis kecuali VPN sudah terpasang.


## 3. IP Spoofing

### 3.1 Definisi

**IP spoofing** adalah serangan jaringan ketika attacker mengubah **source IP** sebuah data packet untuk menyamar sebagai sistem yang authorized dan mendapat akses ke jaringan. Dengan cara ini attacker dapat berkomunikasi dengan target dan melewati firewall rules yang seharusnya memblokir traffic luar.

Hubungan dengan sniffing: setelah sniffing berhasil, attacker memiliki IP dan MAC address perangkat authorized yang bisa ditiru.

### 3.2 Pencegahan

- **Enkripsi** harus selalu diterapkan agar data tidak terbaca malicious actor.
- **Konfigurasi firewall**: spoofing mengubah alamat pengirim agar cocok dengan alamat target network. Jika firewall menerima packet dari internet dengan sender IP yang sama dengan private network, packet itu ditolak karena semua perangkat ber-IP tersebut seharusnya sudah berada di local network. Buat rule untuk menolak seluruh incoming traffic yang memiliki IP address yang sama dengan local network.
- Firewall juga dikonfigurasi menolak IP packet yang tidak authorized dan traffic mencurigakan.


## 4. Jenis Serangan Berbasis Spoofing dan Interception

### 4.1 On-Path Attack

Malicious actor menempatkan diri di tengah koneksi yang sah dan mencegat atau mengubah data in transit. Contoh posisi: di antara web browser dan web server. Nama lain: **meddler-in-the-middle attack**.

- Attacker melakukan sniffing untuk mempelajari IP dan MAC address kedua perangkat, lalu menyamar sebagai salah satunya.
- Transmisi antara dua perangkat yang saling percaya bisa memuat username dan password.
- **DNS spoofing**: jika transmisi memuat DNS look-up, attacker dapat memalsukan respons DNS dan mengalihkan nama domain ke IP address lain yang mungkin berisi kode berbahaya.
- Pertahanan terpenting: enkripsi data in transit, misalnya dengan **TLS**.

### 4.2 Replay Attack

Malicious actor mencegat data packet in transit, lalu **menunda** atau **mengulanginya** pada waktu lain.

- Packet yang tertunda dapat menimbulkan masalah koneksi antar target.
- Transmisi milik authorized user dapat diputar ulang di kemudian waktu untuk menyamar sebagai user tersebut.

### 4.3 Smurf Attack

Kombinasi IP spoofing dengan teknik denial of service.

- Attacker melakukan sniffing terhadap IP address authorized user, lalu membanjirinya dengan packet.
- Setelah spoofed packet mencapai **broadcast address**, packet dikirim ke seluruh perangkat dan server di jaringan.
- Contoh: spoofed packet berisi **ICMP** ping. ICMP dipakai untuk troubleshooting jaringan, tetapi ICMP echo response yang terlalu banyak membuat server kewalahan hingga down dan operasional organisasi lumpuh.
- Pertahanan: advanced firewall yang memantau traffic tidak wajar. Sebagian besar **next generation firewall (NGFW)** punya fitur deteksi anomali jaringan sehingga broadcast berukuran besar terdeteksi sebelum menjatuhkan jaringan.

Catatan konsistensi materi: video menyebut smurf attack sebagai kombinasi DDoS dan IP spoofing (juga jawaban quiz), sedangkan reading menyebutnya IP spoofing yang dikombinasikan dengan teknik DoS.


## 5. Denial of Service

### 5.1 Definisi dan Varian

- **Denial of service attack (DoS)**: serangan yang menargetkan network atau server dan membanjirinya dengan network traffic. Secara umum, DoS adalah kelas serangan yang mencegah sistem yang dikompromikan menjalankan aktivitas sah atau merespons traffic sah.
- **Distributed denial of service attack (DDoS)**: jenis DoS yang memakai banyak perangkat atau server di lokasi berbeda untuk membanjiri target dengan traffic yang tidak diinginkan.
- **SYN flood attack**: jenis DoS yang mensimulasikan koneksi TCP/IP dan membanjiri server dengan **SYN packets**.

### 5.2 Perbedaan DoS dengan IP Spoofing

Pada DoS, attacker tidak menerima respons dari target host, dan semua hal pada packet (termasuk IP di header) tampak authorized. Pada IP spoofing, attacker memakai IP packet berisi IP address palsu dan terus mengirimkannya sampai network server crash.

### 5.3 Defense-in-Depth

Tidak ada satu strategi sempurna untuk menghentikan setiap jenis serangan. Pertahanan dilapis dengan beberapa strategi. Contoh: enkripsi standar industri memperkuat keamanan dan membantu bertahan dari DoS di lebih dari satu level.

### 5.4 Istilah Pembanding dari Quiz

- **Ping of death**: DoS yang terjadi ketika hacker mengirim ICMP packet berukuran melebihi ukuran maksimum ke sebuah sistem.
- **ICMP flood**: DoS dengan mengirim ICMP packet berulang-ulang ke network server.

Keduanya sering dipakai sebagai pengecoh pada soal smurf attack. Jangan tertukar dengan smurf attack.


## 6. Studi Kasus: Analisis SYN Flood pada Web Server Travel Agency

### 6.1 Skenario

Security analyst sebuah travel agency menerima alert otomatis dari monitoring system tentang masalah web server. Saat mengakses website, muncul pesan **connection timeout** di browser. Karyawan rutin memakai halaman sales website untuk mencari paket liburan.

### 6.2 Temuan

Hasil capture dengan packet sniffer menunjukkan banyak **TCP SYN requests** dari sebuah IP address yang tidak dikenal. Server kewalahan dan kehilangan kemampuan merespons. Kesimpulan: **SYN flood attack**, salah satu bentuk DoS.

### 6.3 Respons Awal dan Keterbatasannya

- Server diambil offline sementara agar pulih ke kondisi normal.
- Firewall dikonfigurasi memblokir IP pengirim SYN requests abnormal.
- Blokir IP tidak bertahan lama karena attacker dapat melakukan spoofing IP address lain untuk menghindarinya.
- Manager perlu segera diberi tahu mengenai jenis serangan dan dampaknya pada web server dan karyawan.

### 6.4 Alur Analisis (Incident Report)

Laporan memakai template **Cybersecurity incident report** dengan referensi Wireshark TCP/HTTP log.

- Section 1: identifikasi jenis serangan dari pola pada log. Pertanyaan pemandu: jenis serangan apa yang menghasilkan gejala tersebut, apa beda DoS dan DDoS, dan mengapa website lambat serta menampilkan connection timeout.
- Section 2: jelaskan serangan (gejala dan karakteristik), dampak pada jaringan dan fungsi website, konsekuensi bagi organisasi, dan opsional saran pencegahan.
- Poin wajib dalam jawaban: nama serangan dan deskripsi dampak negatifnya terhadap performa jaringan.

### 6.5 Penilaian Exemplar

- Exemplar menyimpulkan pesan connection timeout adalah hasil DoS attack, spesifiknya SYN flood.
- Langkah penalaran: analisis data log, cocokkan dengan pemahaman jenis serangan, jelaskan bagaimana kesimpulan dicapai, dokumentasikan dampaknya pada jaringan dan gambaran umum bagaimana attacker mengeksploitasi kerentanan, lalu jelaskan bagaimana serangan menyebabkan connection timeout.
- Jawaban boleh berbeda dari exemplar. Yang penting network protocol yang terlibat teridentifikasi dan laporan dibuat. Dalam praktik, analis membuat tebakan terbaik lalu menyelidiki lebih lanjut.
- Kemampuan mengenali pola pada log mempercepat deteksi dan respons insiden.


## 7. Ringkasan Perbandingan Serangan

| Serangan | Ciri utama | Pertahanan utama |
|---|---|---|
| Passive sniffing | Packet dibaca, tidak diubah | VPN, HTTPS, hindari WiFi publik |
| Active sniffing | Packet dimanipulasi in transit | VPN, HTTPS, enkripsi |
| IP spoofing | Source IP dipalsukan | Enkripsi, rule firewall |
| On-path | Penyerang di tengah koneksi | Enkripsi (TLS) |
| Replay | Packet ditunda atau diulang | Enkripsi, monitoring |
| Smurf | Spoofed packet via broadcast address | NGFW |
| SYN flood | Banjir SYN packets | Monitoring traffic, firewall |

Catatan: kolom pertahanan untuk replay dan SYN flood adalah rangkuman dari prinsip umum materi (enkripsi, defense-in-depth, monitoring), bukan pernyataan eksplisit materi.


## 8. Glossary

- **Packet sniffing**: menangkap dan memeriksa data packet di jaringan.
- **Passive packet sniffing**: membaca packet in transit tanpa mengubahnya.
- **Active packet sniffing**: memanipulasi packet in transit.
- **Interception attack**: serangan yang mencegat data packet saat melintasi jaringan.
- **NIC**: hardware penghubung perangkat ke jaringan.
- **Promiscuous mode**: NIC menerima semua traffic, bukan hanya yang ditujukan kepadanya.
- **IP spoofing**: mengubah source IP packet untuk menyamar sebagai sistem authorized.
- **On-path attack (meddler-in-the-middle)**: attacker di tengah koneksi sah, mencegat atau mengubah data.
- **Replay attack**: packet dicegat lalu ditunda atau diulang.
- **Smurf attack**: sniff IP authorized user lalu banjiri dengan packet, spoofed packet menjangkau broadcast address.
- **DoS**: membanjiri network atau server dengan traffic.
- **DDoS**: DoS dari banyak perangkat di lokasi berbeda.
- **SYN flood**: DoS yang mensimulasikan koneksi TCP/IP dan membanjiri server dengan SYN packets.
- **ICMP**: protokol untuk troubleshooting jaringan, bisa disalahgunakan untuk flooding.
- **Broadcast address**: alamat yang meneruskan packet ke semua perangkat di jaringan.
- **NGFW**: firewall generasi lanjut dengan deteksi anomali jaringan.
- **VPN**: layanan yang mengenkripsi data dan menyembunyikan lokasi virtual.
- **HTTPS, SSL/TLS**: protokol enkripsi untuk mencegah eavesdropping.
- **Defense-in-depth**: pertahanan berlapis dengan banyak strategi.
- **Connection timeout**: pesan error browser ketika server tidak merespons.


## 9. Tools & Platform Rujukan

- **Wireshark**: software packet sniffer untuk menangkap dan menganalisis data packet. URL tidak disebutkan di materi.
- **Wireshark TCP/HTTP log** dan **How to read a Wireshark TCP/HTTP log**: materi pendukung aktivitas untuk membaca pola traffic. URL tidak tersedia di screenshot.
- **Cybersecurity incident report template dan exemplar**: template laporan insiden dan contoh jawabannya. URL tidak tersedia di screenshot.
- **Identify: Network Attacks**: flashcard peninjauan jenis serangan jaringan. URL tidak tersedia di screenshot.


## 10. Catatan Ringkas untuk Ditulis Tangan

### Dasar

- Packet — header (IP pengirim, penerima) + body (data berharga)
- Interception attack — serangan yang mencegat packet in transit
- NIC — hardware penghubung ke jaringan; terima packet bila MAC cocok
- Promiscuous mode — NIC terima semua traffic

### Packet Sniffing

- Packet sniffing — tangkap dan periksa packet di jaringan
- Penggunaan sah — investigasi insiden, debugging jaringan
- Penyalahgunaan — baca data bukan miliknya (seperti buka surat orang)
- Wireshark — tool capture di private network
- Passive — baca saja (petugas pos baca surat)
- Active — ubah packet, alihkan ke port salah (tetangga ubah surat)
- Hub — semua host bisa lihat semua traffic

### Pencegahan Sniffing

- VPN — enkripsi data in transit
- HTTPS — SSL/TLS, cegah eavesdropping
- Hindari unprotected WiFi — tanpa enkripsi (kafe, bandara)

### IP Spoofing

- IP spoofing — ubah source IP, menyamar sebagai sistem sah
- Tujuan — lewati firewall rules
- Pencegahan — enkripsi + rule firewall tolak traffic internet ber-IP sama dengan local network

### Jenis Serangan

- On-path (meddler-in-the-middle) — di tengah koneksi, curi/ubah data
- DNS spoofing — respons DNS dipalsukan, domain dialihkan ke IP jahat
- Pertahanan on-path — enkripsi, TLS
- Replay — packet ditunda atau diulang, menyamar sebagai user sah
- Smurf — sniff IP user sah + banjir packet via broadcast address (ICMP ping)
- Pertahanan smurf — NGFW, deteksi anomali
- Ping of death — ICMP packet melebihi ukuran maksimum
- ICMP flood — ICMP berulang ke server

### DoS

- DoS — banjir traffic ke network/server
- DDoS — banyak perangkat, banyak lokasi
- SYN flood — simulasi koneksi TCP/IP, banjir SYN packets
- DoS vs IP spoofing — DoS: tanpa respons, packet tampak sah; spoofing: IP palsu
- Defense-in-depth — pertahanan berlapis, enkripsi standar industri

### Studi Kasus SYN Flood

- Gejala — connection timeout, alert monitoring
- Bukti — banyak TCP SYN dari satu IP asing
- Kesimpulan — SYN flood (DoS)
- Respons — server offline, blokir IP di firewall
- Kelemahan blokir — attacker bisa spoof IP lain
- Laporan — section 1 jenis serangan, section 2 penjelasan dan dampak
- Lapor ke manager — jenis serangan + dampak ke server dan karyawan

### Tools

- Wireshark — packet sniffer
- Template incident report — format laporan insiden
