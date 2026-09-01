# Resume Materi — Foundations of Computer Networks for Security Analysts

**Room:** Google Cybersecurity Certificate — Modul Pengantar Jaringan
**Tanggal penyelesaian:** 16 Agustus 2026

---

## 1. Mengapa Analis Keamanan Perlu Memahami Jaringan

Sebelum mengamankan sebuah sistem, seorang **security analyst** harus lebih dulu memahami desain dan cara kerja jaringan. Pemahaman ini menjadi dasar untuk mengenali **vulnerability** pada struktur jaringan dan bagaimana **malicious actor** mengeksploitasinya.

Mengamankan jaringan organisasi adalah bagian inti tanggung jawab analis keamanan, mencakup perlindungan terhadap **threat**, **risk**, dan **vulnerability**.

Modul ini mencakup tiga area besar: struktur jaringan, perangkat/tools standar networking, dan cloud network — dengan **TCP/IP model** sebagai kerangka dasar untuk memahami bagaimana data diorganisasi dan bergerak dalam jaringan.

---

## 2. Definisi dan Struktur Dasar Jaringan

### 2.1 Apa Itu Network

**Network** adalah sekelompok perangkat yang saling terhubung — laptop, cell phone, smart device, workstation, printer, server. Perangkat berkomunikasi melalui koneksi **wired** (kabel) atau **wireless**.

### 2.2 Identifikasi Perangkat

Perangkat saling menemukan satu sama lain menggunakan dua jenis alamat unik:

- **IP address** — mengidentifikasi perangkat pada level jaringan, dipakai untuk routing data antar-network.
- **MAC address** — alamat fisik unik yang melekat pada network interface perangkat, dipakai pada level jaringan lokal (data link layer).

Setiap **data packet** yang dikirim membawa informasi **source** (sumber) dan **destination** (tujuan) data.

### 2.3 Client-Server Model

Model yang menjelaskan bagaimana **server** menyediakan informasi dan layanan ke perangkat lain (**client**). Client mengirim **request**, server menjalankan request tersebut dan mengembalikan hasil.

Contoh server umum:

- **DNS server** — melakukan domain name lookup.
- **File server** — menyimpan dan mengambil file dari database.
- **Mail server** — mengatur email perusahaan.

---

## 3. Klasifikasi Jaringan Berdasarkan Cakupan

| Aspek | LAN | WAN |
|---|---|---|
| Kepanjangan | Local Area Network | Wide Area Network |
| Cakupan | Kecil — rumah, kantor, sekolah | Luas — kota, negara bagian, negara |

**LAN** terbentuk ketika perangkat pribadi terhubung ke WiFi rumah; LAN kemudian tersambung ke internet. **WAN** memungkinkan komunikasi lintas lokasi geografis jauh — internet sendiri adalah contoh **WAN** terbesar.

Catatan penting: untuk menghubungkan **seluruh kota**, tipe jaringan yang tepat adalah **WAN**, bukan LAN, karena LAN secara definisi hanya mencakup area kecil.

---

## 4. Perangkat Jaringan Fisik

### 4.1 Hub

Perangkat yang **broadcast** semua informasi ke setiap perangkat yang terhubung, tanpa memilah tujuan — mirip menara radio yang menyiarkan sinyal ke semua penerima pada frekuensi yang sama.

Karena sifatnya ini, hub **rentan terhadap eavesdropping** dan jarang dipakai pada jaringan modern. Lebih umum ditemukan pada setup terbatas seperti home office.

### 4.2 Switch

Perangkat lebih cerdas dari hub karena hanya meneruskan data ke perangkat tujuan yang dimaksud (**intended destination**), bukan menyiarkan ke semua perangkat.

Switch menyimpan **MAC address table** yang memetakan MAC address perangkat ke nomor port pada switch, dipakai untuk meneruskan packet secara tepat sasaran. Switch beroperasi pada **data link layer** dalam TCP/IP model.

Keuntungan switch: mengontrol flow of traffic, meningkatkan network performance, meneruskan data hanya ke tujuan yang benar — sehingga switch lebih **secure** dibanding hub.

### 4.3 Router

Perangkat yang menghubungkan **banyak jaringan berbeda** dan mengarahkan traffic berdasarkan **IP address** jaringan tujuan. Router beroperasi pada **network layer** dalam TCP/IP model.

Router membaca informasi pada **IP header**, lalu meneruskan packet ke router berikutnya di sepanjang jalur menuju tujuan — proses berulang hingga packet sampai. Router juga bisa memiliki fitur **firewall** bawaan yang mengizinkan atau memblokir incoming traffic, mencegah malicious traffic masuk ke private network.

Alur pengiriman data lintas jaringan:

```
Computer → Router (baca destination address) → Router jaringan tujuan → Perangkat tujuan
```

### 4.4 Modem

Menghubungkan **router** ke **internet** melalui **Internet Service Provider (ISP)**, membawa akses internet ke dalam LAN. ISP menyediakan konektivitas via telephone line, coaxial cable, atau fiber optic cable.

Modem menerima sinyal digital dari internet, mengonversinya ke format yang kompatibel dengan koneksi fisik dari ISP, lalu meneruskannya ke router. Enterprise network skala besar sering pakai teknologi broadband lain, bukan modem konvensional.

Alur pengiriman data lintas lokasi geografis:

```
Computer → Router → Modem → Internet → Modem penerima → Router penerima → Perangkat tujuan
```

### 4.5 Wireless Access Point (WAP)

Mengirim dan menerima sinyal digital melalui **radio waves**, menciptakan jaringan wireless. Perangkat dengan wireless adapter terhubung ke access point menggunakan **Wi-Fi** — sekumpulan standar komunikasi wireless antar network device.

### 4.6 Firewall

**Network security device** yang memonitor traffic masuk dan keluar jaringan — berfungsi sebagai **first line of defense**.

Firewall membatasi traffic tertentu sesuai security rules yang dikonfigurasi organisasi, biasanya diposisikan di antara internal network yang terkontrol dengan resource eksternal yang tidak dipercaya (seperti internet). Firewall hanyalah satu lapisan pertahanan, bukan solusi keamanan tunggal.

### 4.7 Virtualization Tools

Physical device (hub, switch, router, modem) memiliki fungsi yang kini banyak digantikan oleh **virtualization tools** — software yang menjalankan operasi network setara fungsi perangkat fisik tersebut, umumnya ditawarkan oleh cloud service provider. Memberi keuntungan cost savings dan scalability.

---

## 5. Network Diagram sebagai Alat Analisis

**Network diagram** adalah peta visual yang menunjukkan perangkat pada jaringan beserta cara mereka saling terhubung, menggunakan simbol grafis dan garis (biasanya putus-putus) untuk merepresentasikan koneksi.

Diagram ini memungkinkan **network administrator** dan **security personnel** membayangkan arsitektur jaringan organisasi, serta membantu security analyst mengembangkan dan menyempurnakan strategi pengamanan.

Susunan tipikal jaringan lokal (dari luar ke dalam):

```
Internet → Firewall → Router (terhubung ke Server) → Firewall kedua → Switch → Wireless Access Point / Devices
```

Switch bersifat opsional, dipakai untuk menambah port/koneksi Ethernet. Beberapa desain menempatkan dua router paralel untuk keperluan **load balancing**, meningkatkan performa jaringan.

---

## 6. Cloud Computing dan Cloud Networking

### 6.1 Definisi Dasar

**Cloud computing** adalah praktik menggunakan remote server, aplikasi, dan layanan jaringan yang di-hosting di internet — berlawanan dengan **on-premise network**, di mana seluruh perangkat disimpan di lokasi fisik milik perusahaan.

**Cloud network** adalah kumpulan server/komputer yang menyimpan resource dan data di **remote data center**, dapat diakses via internet. Karena server tidak disimpan di lokasi fisik perusahaan, server ini disebut berada "in the cloud".

Cloud network memungkinkan layanan online diakses dari lokasi geografis mana pun, berbeda dari traditional network yang meng-hosting server secara lokal.

### 6.2 Cloud Service Provider (CSP)

Perusahaan yang menawarkan layanan cloud computing, memiliki **data center** besar berisi jutaan server di berbagai lokasi global. Perusahaan pelanggan membayar untuk storage dan compute power yang dibutuhkan, diakses melalui **API** atau **web console** milik CSP.

### 6.3 Tiga Kategori Layanan CSP

- **Software as a Service (SaaS)** — software suite yang dioperasikan CSP, digunakan perusahaan secara remote tanpa perlu meng-hosting sendiri. Mencakup seluruh lapisan hingga cloud-hosted application.
- **Infrastructure as a Service (IaaS)** — virtual computer component (virtual container, storage) yang dikonfigurasi remote via API/web console CSP, bisa menjalankan existing application tanpa modifikasi signifikan. Mencakup lapisan physical data center dan servers/networking/storage.
- **Platform as a Service (PaaS)** — tools bagi application developer untuk membangun custom application sesuai kebutuhan bisnis spesifik, diakses di cloud. Mencakup dari data center hingga database management & development tools.

Hierarkinya bertingkat: **IaaS** lapisan paling dasar, **PaaS** mencakup lebih banyak lapisan di atasnya, **SaaS** mencakup keseluruhan tumpukan hingga aplikasi siap pakai.

### 6.4 Hybrid dan Multi-Cloud

- **Hybrid cloud environment** — organisasi menggunakan layanan CSP sebagai tambahan dari on-premise computer, network, dan storage yang sudah dimiliki.
- **Multi-cloud environment** — organisasi menggunakan lebih dari satu CSP sekaligus.

Sebagian besar organisasi memilih hybrid cloud untuk menekan biaya sekaligus mempertahankan kontrol atas resource jaringan mereka.

### 6.5 Software-Defined Network (SDN)

SDN terdiri dari **virtual network device dan service** — CSP menyediakan virtual switch, router, firewall, dan lainnya sebagai pengganti perangkat fisik.

Sebagian besar hardware modern kini mendukung **network virtualization**: switch dan router fisik menjalankan packet routing menggunakan software. Pada cloud networking, SDN tools ini di-hosting di server milik data center CSP.

### 6.6 Tiga Keuntungan Utama Cloud Computing dan SDN

- **Reliability** — ditentukan oleh seberapa available layanan cloud, seberapa secure koneksinya, dan seberapa konsisten layanan berjalan tanpa interupsi.
- **Cost** — CSP menawarkan virtual device dan service dengan biaya jauh lebih rendah dibanding biaya perusahaan menginstal, patch, upgrade, dan mengelola infrastruktur sendiri, karena skala data center CSP yang sangat besar.
- **Scalability** — CSP menerapkan **elastic utility model**, perusahaan hanya membayar sesuai kebutuhan aktual (pay-as-needed), sehingga risiko over-investasi hardware saat kebutuhan bisnis menurun dapat dihindari. Perubahan konfigurasi juga jauh lebih cepat via API/web console dibanding pengadaan hardware fisik — contohnya konfigurasi cepat **Web Application Firewall (WAF)**, **Intrusion Detection/Protection System (IDS/IPS)**, atau **L3/L4 firewall** saat menghadapi ancaman.

### 6.7 Pergeseran Fokus Keamanan di Era Cloud

Migrasi ke cloud menciptakan tumpang tindih antara **identity-based security** dan **network-based security** tradisional. Verifikasi keamanan tidak lagi cukup hanya memeriksa dari mana traffic berasal, tetapi juga identitas yang menyertai traffic tersebut.

---

## 7. Perspektif Profesional di Lapangan

Beberapa insight dari praktisi keamanan yang relevan dengan konsep di atas:

- **CISO** bertanggung jawab menjaga keamanan jaringan dan data pelanggan organisasi, sekaligus mendukung kebutuhan penegak hukum. Membangun relasi dan reputasi lewat komunitas/organisasi profesi terbukti sama pentingnya dengan skill teknis dalam perjalanan karier di bidang ini.

- **Software engineer di bidang network security** sering membangun internal tools yang mendukung kerja security team dan network team, tidak selalu langsung menangani insiden secara langsung. Cybersecurity ditekankan sebagai **team sport** — banyak solusi mungkin untuk satu masalah, sehingga kolaborasi dan brainstorming sangat membantu.

- **Offensive security engineer** mensimulasikan adversary untuk menemukan celah sebelum dieksploitasi pihak jahat. Memerlukan penguasaan:
  - **Command line** — interaksi dengan low-level system (memory, kernel) hingga high-level application.
  - **Log parsing** — menemukan root cause masalah dari log untuk keperluan debugging.
  - **Network traffic analysis** — memeriksa traffic lintas layer untuk mendeteksi kebocoran password, infrastruktur yang tidak aman, atau firewall yang salah konfigurasi.

- Kemampuan **komunikasi lintas tim** (product team, engineer, business stakeholder) sama pentingnya dengan kemampuan teknis, karena isu keamanan yang ditemukan harus benar-benar dikomunikasikan dengan pendekatan bisnis yang tepat agar diperbaiki.

---

## 8. Glossary

- **TCP/IP Model** — kerangka kerja fundamental yang mengatur bagaimana data diorganisasi dan berpindah dalam jaringan.
- **IP Address** — identifier unik perangkat pada level jaringan, dipakai untuk routing antar-network.
- **MAC Address** — identifier unik perangkat pada level fisik/data link, melekat pada network interface.
- **LAN (Local Area Network)** — jaringan cakupan kecil seperti rumah, kantor, sekolah.
- **WAN (Wide Area Network)** — jaringan cakupan geografis luas seperti kota, negara bagian, negara.
- **Hub** — broadcast data ke semua perangkat tanpa seleksi tujuan.
- **Switch** — meneruskan data hanya ke tujuan yang tepat menggunakan MAC address table; beroperasi di data link layer.
- **Router** — menghubungkan dan mengarahkan traffic antar-jaringan berdasarkan IP address; beroperasi di network layer.
- **Modem** — menjembatani router dengan internet melalui ISP.
- **Wireless Access Point (WAP)** — membentuk jaringan wireless menggunakan Wi-Fi.
- **Firewall** — memonitor dan memfilter traffic masuk/keluar jaringan sebagai lapisan pertahanan pertama.
- **Client-Server Model** — pola komunikasi di mana client meminta layanan/informasi dan server memenuhinya.
- **Network Diagram** — representasi visual arsitektur jaringan untuk kebutuhan analisis keamanan.
- **Cloud Computing** — penggunaan server, aplikasi, dan layanan jaringan yang di-hosting di internet.
- **CSP (Cloud Service Provider)** — perusahaan penyedia layanan cloud computing berbasis data center skala besar.
- **SaaS (Software as a Service)** — software siap pakai yang dioperasikan CSP.
- **IaaS (Infrastructure as a Service)** — infrastruktur virtual (compute, storage) yang dikonfigurasi remote.
- **PaaS (Platform as a Service)** — platform pengembangan untuk membangun custom application di cloud.
- **Hybrid Cloud** — kombinasi on-premise dan satu CSP.
- **Multi-Cloud** — penggunaan lebih dari satu CSP sekaligus.
- **SDN (Software-Defined Network)** — jaringan berbasis perangkat virtual (virtual switch, router, firewall) yang dikendalikan lewat software.
- **Elastic Utility Model** — model konsumsi layanan cloud yang menyesuaikan kapasitas secara fleksibel sesuai kebutuhan aktual.
- **WAF (Web Application Firewall)** — perangkat keamanan cloud untuk memfilter traffic aplikasi web.
- **IDS/IPS (Intrusion Detection/Protection System)** — sistem untuk mendeteksi dan/atau mencegah intrusi pada jaringan.

---

## 9. Tools & Platform Rujukan

- **Google Cloud (GC)** — platform CSP yang direkomendasikan sebagai referensi lanjutan untuk mempelajari cloud computing dan layanan yang ditawarkan. Tidak disertai URL eksplisit pada materi.

---

## 10. Catatan Ringkas untuk Ditulis Tangan

**Dasar Networking**
- Network — kumpulan device saling terhubung
- IP address — id level jaringan (routing)
- MAC address — id level fisik/data link
- Client-server model — client request, server penuhi
- DNS server — lookup domain name
- File server — simpan/ambil file
- Mail server — atur email perusahaan

**LAN vs WAN**
- LAN — area kecil (rumah/kantor)
- WAN — area luas (kota/negara), internet = WAN terbesar

**Perangkat Fisik**
- Hub — broadcast ke semua, rawan eavesdrop, layer: -
- Switch — kirim ke tujuan tepat, pakai MAC address table, data link layer
- Router — hubungkan antar-network, baca IP header, network layer
- Modem — jembatan router ke internet via ISP
- Wireless Access Point — bikin jaringan wireless pakai Wi-Fi
- Firewall — filter traffic masuk/keluar, first line of defense
- Virtualization tools — software pengganti fungsi device fisik

**Alur Data**
- Lintas network: Computer → Router → Router tujuan → Device tujuan
- Lintas lokasi: Computer → Router → Modem → Internet → Modem tujuan → Router tujuan → Device tujuan

**Network Diagram**
- Peta visual device + koneksi
- Susunan umum: Internet-Firewall-Router(+Server)-Firewall-Switch-WAP/Devices
- 2 router paralel = load balancing

**Cloud Computing**
- Cloud computing — server/app/service di-hosting di internet
- On-premise — device disimpan fisik di perusahaan
- Cloud network — resource di remote data center, akses via internet
- CSP — provider cloud, punya data center besar, akses via API/web console

**Model Layanan CSP**
- SaaS — software siap pakai
- IaaS — infrastruktur virtual (compute, storage)
- PaaS — platform bikin custom app
- Urutan cakupan: IaaS < PaaS < SaaS

**Hybrid/Multi Cloud**
- Hybrid — on-premise + 1 CSP
- Multi-cloud — lebih dari 1 CSP

**SDN**
- SDN — virtual switch/router/firewall via software
- Network virtualization — hardware fisik jalankan software utk routing

**Keuntungan Cloud**
- Reliability — available, secure, konsisten
- Cost — lebih murah dari infra sendiri
- Scalability — elastic utility model, pay-as-needed
- WAF, IDS/IPS, L3/L4 firewall — bisa dikonfig cepat via cloud

**Keamanan Era Cloud**
- Identity-based security overlap network-based security
- Verifikasi bukan cuma sumber traffic, tapi juga identitas

**Skill Offensive Security**
- Command line — interaksi low-level (memory/kernel) - high-level (app)
- Log parsing — cari root cause dari log
- Network traffic analysis — cek traffic lintas layer, deteksi leak password/firewall salah config

**Soft Skill Penting**
- Komunikasi lintas tim (business approach)
- Cybersecurity = team sport
- Continuous learning & curiosity wajib
