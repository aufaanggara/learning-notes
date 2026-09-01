# Resume Materi: What is Networking?
**TryHackMe — Pre-Security Path**
**Tanggal penyelesaian: 31 Agustus 2026**

---

## 1. Konsep Dasar Jaringan

**Jaringan (network)** adalah kumpulan entitas yang saling terhubung dan dapat berkomunikasi satu sama lain. Dalam konteks komputasi, jaringan terbentuk ketika dua atau lebih perangkat teknologi saling terhubung — mulai dari jaringan sekecil dua komputer di satu ruangan, hingga jaringan global yang mencakup miliaran perangkat.

Perangkat yang bisa menjadi bagian dari jaringan tidak terbatas pada komputer dan laptop. Smartphone, kamera keamanan (CCTV), lampu lalu lintas, sensor cuaca, dan perangkat pertanian cerdas semuanya bisa menjadi node dalam sebuah jaringan.

Jaringan sudah menyatu dalam infrastruktur kehidupan modern — distribusi listrik, pengumpulan data cuaca, hingga pengaturan lalu lintas jalan semuanya bergantung pada jaringan. Karena itu, **memahami jaringan adalah fondasi utama dalam cybersecurity** — tidak ada serangan maupun pertahanan siber yang terjadi di luar konteks jaringan.


## 2. Internet dan Tipe Jaringan

### 2.1 Definisi Internet

**Internet** adalah satu jaringan raksasa yang terbentuk dari banyak jaringan-jaringan kecil yang saling terhubung. Internet bukan satu entitas tunggal, melainkan hasil penggabungan jutaan jaringan privat di seluruh dunia menjadi satu ekosistem komunikasi global.

Analogi yang digunakan dalam materi: Alice, Bob, dan Jim masing-masing berada di jaringannya sendiri. Zayn dan Toby berbicara dalam bahasa berbeda dari Bob dan Jim — hanya Alice yang bisa berbicara dalam kedua bahasa, sehingga Alice menjadi jembatan antar kelompok. Inilah cara kerja Internet: jaringan-jaringan yang berbeda terhubung melalui titik-titik penghubung yang "mengerti" kedua sisi.

### 2.2 Sejarah Singkat

- **Akhir 1960-an — ARPANET**: Jaringan komputer pertama yang terdokumentasi, dikembangkan atas pendanaan Departemen Pertahanan Amerika Serikat. Tujuan awalnya adalah komunikasi militer dan riset, bukan untuk publik umum.
- **1989 — World Wide Web (WWW)**: Diciptakan oleh **Tim Berners-Lee**. WWW mengubah Internet dari infrastruktur komunikasi teknis menjadi platform penyimpanan dan berbagi informasi yang bisa diakses publik — inilah Internet yang kita kenal hari ini.

Penting: **Internet dan WWW adalah dua hal berbeda.** Internet adalah infrastruktur jaringannya; WWW adalah layanan (aplikasi) yang berjalan di atas infrastruktur tersebut.

### 2.3 Dua Tipe Jaringan

**Private Network** adalah jaringan lokal yang bersifat terbatas — contohnya jaringan di dalam rumah, kantor, atau kampus. Perangkat di dalamnya berkomunikasi satu sama lain dalam lingkup terisolasi dan tidak langsung terekspos ke Internet.

**Public Network** adalah jaringan yang menghubungkan semua private network menjadi satu kesatuan global. Inilah Internet. Ketika sebuah perangkat mengakses Internet, ia meninggalkan lingkup private network-nya dan masuk ke public network.


## 3. Identifikasi Perangkat dalam Jaringan

Agar komunikasi jaringan bisa berjalan dengan benar, setiap perangkat harus memiliki identitas yang bisa dikenali. Tanpa sistem identifikasi yang jelas, data tidak tahu harus dikirim ke mana dan dari siapa asalnya.

Perangkat jaringan memiliki dua cara identifikasi, dianalogikan seperti manusia:

| Manusia | Perangkat |
|---------|-----------|
| Nama (bisa berubah) | IP Address |
| Sidik jari (permanen) | MAC Address |


## 4. IP Address

### 4.1 Struktur dan Format

**IP Address** (Internet Protocol Address) adalah label numerik yang digunakan untuk mengidentifikasi sebuah perangkat dalam jaringan. Format standarnya adalah **IPv4**: empat kelompok angka (oktet) yang dipisahkan titik, masing-masing bernilai antara 0 hingga 255.

Contoh: `192.168.1.1` terdiri dari empat oktet — `192`, `168`, `1`, dan `1`.

IP Address bersifat dinamis — bisa berubah dari waktu ke waktu dan bisa berpindah ke perangkat lain. Namun dalam satu jaringan yang sama, **tidak boleh ada dua perangkat aktif dengan IP Address yang identik secara bersamaan** — kondisi ini akan menyebabkan konflik IP.

### 4.2 IP Private vs IP Public

**IP Private** digunakan untuk komunikasi antar perangkat di dalam jaringan lokal. Alamat ini tidak bisa diakses langsung dari Internet dan hanya berlaku di dalam lingkup jaringan lokalnya.

Range IP private yang umum:
- `192.168.x.x`
- `10.x.x.x`
- `172.16.x.x` hingga `172.31.x.x`

**IP Public** digunakan untuk mengidentifikasi perangkat atau jaringan di Internet. Alamat ini diberikan oleh **ISP (Internet Service Provider)** — penyedia layanan Internet seperti Telkom, Indihome, dan sejenisnya.

Fakta penting: dua perangkat berbeda dalam satu jaringan lokal bisa memiliki IP private yang berbeda, tetapi **berbagi satu IP public yang sama**, karena yang "tampil" ke Internet adalah jaringan secara keseluruhan, bukan perangkat individual.

Contoh:

| Device | IP Private | IP Public |
|--------|-----------|-----------|
| DESKTOP-KJE57FD | 192.168.1.77 | 86.157.52.21 |
| CMNatic-PC | 192.168.1.74 | 86.157.52.21 |

### 4.3 IPv4 vs IPv6

**IPv4** menggunakan sistem 32-bit, menghasilkan sekitar **4,29 miliar alamat** (2^32). Dengan pertumbuhan perangkat yang terhubung ke Internet — Cisco memperkirakan 50 miliar perangkat pada akhir 2021 — stok alamat IPv4 hampir habis.

**IPv6** dikembangkan sebagai solusi, menggunakan sistem 128-bit yang menghasilkan lebih dari **340 triliun alamat** (2^128). Selain kapasitas yang jauh lebih besar, IPv6 juga dirancang dengan metodologi baru yang lebih efisien.

| Versi | Bit | Kapasitas | Contoh Format |
|-------|-----|-----------|---------------|
| IPv4 | 32-bit | ~4,29 miliar | `86.157.52.21` |
| IPv6 | 128-bit | 340 triliun+ | `2a00:22c4:a531:c500:425f:cce6:c36b:f64d` |


## 5. MAC Address

### 5.1 Definisi dan Struktur

**MAC Address** (Media Access Control Address) adalah identitas fisik permanen yang tertanam pada **network interface** — microchip yang terpasang di motherboard setiap perangkat jaringan. MAC Address ditetapkan oleh pabrik saat perangkat diproduksi dan tidak berubah selama masa hidup perangkat tersebut.

Format: 12 karakter heksadesimal yang dipisahkan titik dua setiap dua karakter.

Contoh: `a4:c3:f0:85:ac:2d`

Struktur MAC Address terbagi menjadi dua bagian:
- **6 karakter pertama** (`a4:c3:f0`) — kode vendor, merepresentasikan perusahaan pembuat network interface (Intel, Realtek, Broadcom, dll). Setiap vendor memiliki kode unik yang terdaftar secara global.
- **6 karakter terakhir** (`85:ac:2d`) — nomor unik yang membedakan perangkat ini dari semua perangkat lain yang dibuat oleh vendor yang sama.

### 5.2 Perbedaan MAC Address dan IP Address

| Aspek | IP Address | MAC Address |
|-------|-----------|-------------|
| Sifat | Dinamis, bisa berubah | Permanen, ditetapkan di pabrik |
| Ditetapkan oleh | ISP / administrator jaringan | Produsen hardware |
| Level | Software (jaringan) | Hardware (fisik) |
| Analogi | Nama manusia | Sidik jari manusia |

### 5.3 MAC Spoofing

**MAC Spoofing** adalah teknik memalsukan MAC Address sebuah perangkat agar terlihat seperti perangkat lain di jaringan. Meskipun MAC Address dirancang sebagai identitas permanen, nilai yang ditransmisikan ke jaringan bisa dimanipulasi melalui software.

Bahaya utama: sistem keamanan yang hanya mengandalkan MAC Address sebagai filter — misalnya firewall yang hanya mengizinkan komunikasi dari MAC Address tertentu — bisa dibobol dengan cara menyalin MAC Address yang diizinkan tersebut.

Skenario nyata dari materi:

Dalam simulasi WiFi hotel berbayar, router hanya meneruskan paket dari perangkat yang MAC Address-nya sudah terdaftar (sudah bayar). Alice sudah membayar sehingga paketnya diteruskan. Bob belum membayar sehingga paketnya dibuang. Ketika Bob melakukan MAC Spoofing dengan mengubah MAC Address perangkatnya menjadi identik dengan MAC Address Alice, router tidak bisa membedakan keduanya dan meneruskan paket Bob seolah-olah itu dari Alice.

Tempat yang umum menggunakan MAC filtering: WiFi hotel, kafe, bandara, dan layanan public WiFi berbayar lainnya.


## 6. Ping dan ICMP

### 6.1 Definisi

**Ping** adalah network tool diagnostik paling dasar yang tersedia di hampir semua sistem operasi (Linux, Windows, macOS) tanpa perlu instalasi tambahan. Fungsi utamanya adalah memverifikasi apakah koneksi antara dua perangkat ada dan berfungsi, serta mengukur kecepatan koneksi tersebut.

Ping bekerja menggunakan **ICMP (Internet Control Message Protocol)** — protokol yang dirancang khusus untuk keperluan diagnostik dan kontrol jaringan, bukan untuk transfer data seperti TCP atau UDP.

### 6.2 Cara Kerja

Mekanisme ping terdiri dari dua langkah:

1. Perangkat pengirim mengirimkan **ICMP Echo Request** ke perangkat tujuan.
2. Jika target aktif dan bisa dijangkau, ia membalas dengan **ICMP Echo Reply**.

Waktu yang dibutuhkan dari pengiriman request hingga diterimanya reply disebut **latency** atau **Round Trip Time (RTT)**, diukur dalam milidetik (ms).

### 6.3 Sintaks dan Penggunaan

```
ping [IP Address atau domain]
```

Contoh penggunaan:

```bash
ping 192.168.1.254      # ping ke perangkat di jaringan lokal
ping 8.8.8.8            # ping ke DNS publik Google
ping google.com         # ping menggunakan nama domain
```

### 6.4 Membaca Output Ping

Contoh output:

```
PING 192.168.1.254 56(84) bytes of data.
64 bytes from 192.168.1.254: icmp_seq=1 ttl=63 time=2.18 ms
64 bytes from 192.168.1.254: icmp_seq=2 ttl=63 time=2.53 ms
64 bytes from 192.168.1.254: icmp_seq=6 ttl=63 time=4.45 ms

6 packets transmitted, 6 received, 0% packet loss, time 5008ms
rtt min/avg/max/mdev = 2.152/4.160/10.313/2.864 ms
```

Penjelasan setiap elemen:

| Elemen | Contoh | Artinya |
|--------|--------|---------|
| `icmp_seq` | `icmp_seq=1` | Nomor urut paket — gap di sini menandakan packet loss |
| `ttl` | `ttl=63` | Time To Live — batas lompatan (hop) sebelum paket dibuang |
| `time=X ms` | `time=2.18 ms` | Latensi satu paket (bolak-balik) |
| `X% packet loss` | `0% packet loss` | Persentase paket hilang — 0% berarti koneksi sehat |
| `time Xms` | `time 5008ms` | Total durasi sesi ping berjalan (bukan latensi) |
| `rtt min/avg/max` | `2.152/4.160/10.313` | Statistik latensi: minimum, rata-rata, maksimum |

### 6.5 Klarifikasi: `time=X ms` vs `time Xms`

Dua nilai "time" dalam output ping sering disalahartikan:

- **`time=2.18 ms`** (per baris) adalah latensi satu paket — waktu yang dibutuhkan satu paket untuk pergi ke target dan kembali. Nilai kecil berarti koneksi cepat.
- **`time 5008ms`** (di baris statistik) adalah total durasi sesi ping berjalan dari awal hingga selesai. Nilai ini bukan latensi — 5008ms ≈ 5 detik karena ping mengirim 6 paket dengan jeda ~1 detik antar paket (6 × ~1 detik = ~5–6 detik total).

### 6.6 Kegunaan dalam Konteks Cybersecurity

Ping adalah langkah pertama dalam **network reconnaissance** — proses pengumpulan informasi tentang target sebelum scanning lebih lanjut. Dengan ping, seorang pentester memverifikasi bahwa target aktif dan bisa dijangkau sebelum menggunakan tools yang lebih kompleks seperti Nmap.

Kegunaan praktis lainnya:
- Mendeteksi apakah host online atau offline (request timeout = tidak terjangkau)
- Mengukur latensi koneksi untuk menilai kualitas jaringan
- Mendeteksi packet loss sebagai indikator ketidakstabilan koneksi
- Estimasi jarak ke target berdasarkan nilai TTL


---

## Glossary — Istilah Kunci

**Network (Jaringan)** — Kumpulan perangkat yang saling terhubung dan dapat berkomunikasi.

**Internet** — Jaringan global yang terbentuk dari banyak private network yang saling terhubung melalui public network.

**ARPANET** — Jaringan komputer pertama, dikembangkan oleh Departemen Pertahanan AS pada akhir 1960-an; cikal bakal Internet.

**WWW (World Wide Web)** — Layanan berbagi informasi yang berjalan di atas Internet, diciptakan Tim Berners-Lee tahun 1989.

**Private Network** — Jaringan lokal terbatas (rumah, kantor); tidak langsung terekspos ke Internet.

**Public Network** — Jaringan yang menghubungkan semua private network; yaitu Internet itu sendiri.

**IP Address** — Label numerik untuk identifikasi perangkat di jaringan; bersifat dinamis dan bisa berubah.

**IP Private** — IP yang digunakan dalam jaringan lokal; tidak bisa diakses dari Internet secara langsung.

**IP Public** — IP yang digunakan di Internet; diberikan oleh ISP.

**ISP (Internet Service Provider)** — Penyedia layanan Internet yang mengalokasikan IP Public ke pelanggan.

**IPv4** — Versi IP dengan sistem 32-bit; kapasitas ~4,29 miliar alamat.

**IPv6** — Versi IP dengan sistem 128-bit; kapasitas 340 triliun+ alamat; diciptakan untuk menggantikan IPv4.

**Oktet** — Satu kelompok angka dalam format IPv4 (0–255); ada empat oktet dalam satu IP Address.

**MAC Address** — Identitas fisik permanen pada network interface; ditetapkan di pabrik; tidak berubah.

**Network Interface** — Microchip di motherboard yang memungkinkan perangkat terhubung ke jaringan.

**Heksadesimal** — Sistem bilangan berbasis 16 yang digunakan dalam format MAC Address.

**Vendor Code** — Enam karakter pertama MAC Address yang merepresentasikan perusahaan pembuat network interface.

**MAC Spoofing** — Teknik memalsukan MAC Address perangkat agar teridentifikasi sebagai perangkat lain di jaringan.

**MAC Filtering** — Mekanisme kontrol akses jaringan berbasis MAC Address; rentan terhadap MAC Spoofing.

**Ping** — Tool diagnostik jaringan yang memverifikasi koneksi dan mengukur latensi menggunakan paket ICMP.

**ICMP (Internet Control Message Protocol)** — Protokol untuk diagnostik dan kontrol jaringan; digunakan oleh ping.

**ICMP Echo Request** — Paket yang dikirim oleh pengirim saat melakukan ping.

**ICMP Echo Reply** — Paket balasan dari target sebagai respons terhadap Echo Request.

**Latency / RTT (Round Trip Time)** — Waktu tempuh paket dari pengirim ke target dan kembali; diukur dalam milidetik.

**Packet Loss** — Persentase paket yang tidak kembali ke pengirim; indikator ketidakstabilan koneksi.

**TTL (Time To Live)** — Nilai yang membatasi berapa kali paket bisa melewati router sebelum dibuang; berguna untuk estimasi jarak ke target.

**Network Reconnaissance** — Tahap pengumpulan informasi tentang target jaringan sebelum scanning atau pengujian lebih lanjut.

**DHCP (Dynamic Host Configuration Protocol)** — Protokol yang mengalokasikan IP Address secara otomatis ke perangkat dalam jaringan.


---

## Tools & Platform Rujukan

**Ping** — Tool diagnostik bawaan semua OS untuk mengecek koneksi dan latensi ke host tertentu. Tidak perlu instalasi; langsung tersedia di terminal Linux, Windows Command Prompt, dan macOS Terminal.

**Nmap** — Network scanner untuk host discovery, port scanning, dan service detection. Relevan sebagai langkah lanjutan setelah ping dalam proses reconnaissance.
URL: https://nmap.org

**Wireshark** — Packet analyzer berbasis GUI untuk meng-capture dan menganalisis paket jaringan secara real-time, termasuk paket ICMP dari aktivitas ping.
URL: https://www.wireshark.org

**macchanger** — Tool Linux untuk memanipulasi (spoof) MAC Address pada network interface; digunakan untuk simulasi MAC Spoofing di lab.

**ifconfig / ip** — Command bawaan Linux untuk melihat dan mengkonfigurasi informasi jaringan (IP Address, MAC Address, network interface).

**curl ifconfig.me** — Cara cepat untuk mengecek IP Public dari terminal Linux.

**TryHackMe** — Platform latihan cybersecurity berbasis browser dengan room terstruktur dari beginner hingga advanced; tempat room ini berasal.
URL: https://tryhackme.com

**HackTheBox** — Platform CTF dan lab cybersecurity untuk latihan penetration testing tingkat lebih lanjut.
URL: https://www.hackthebox.com


---

## Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar

- Network — perangkat saling terhubung dan berkomunikasi
- Internet — jaringan dari banyak jaringan (private → public)
- Private Network — jaringan lokal terbatas (rumah/kantor)
- Public Network — Internet; menghubungkan semua private network
- ARPANET — cikal bakal Internet, 1960-an, proyek militer AS
- WWW — diciptakan Tim Berners-Lee, 1989; Internet jadi platform publik
- WWW ≠ Internet — WWW adalah layanan, Internet adalah infrastruktur

### IP Address

- IP Address — identitas perangkat di jaringan, dinamis (bisa berubah)
- Format IPv4 — 4 oktet, masing-masing 0–255, contoh: 192.168.1.1
- IP Private — untuk jaringan lokal, contoh: 192.168.x.x
- IP Public — untuk Internet, diberikan ISP
- Dua device beda bisa punya IP private beda tapi IP public SAMA
- IPv4 — 32-bit, ~4,29 miliar alamat, hampir habis
- IPv6 — 128-bit, 340 triliun+ alamat, solusi kekurangan IPv4
- ISP — pihak yang memberikan IP Public ke pelanggan

### MAC Address

- MAC Address — identitas fisik permanen, ditetapkan di pabrik
- Format — 12 karakter hex dipisah titik dua, contoh: a4:c3:f0:85:ac:2d
- 6 karakter pertama — kode vendor (pembuat network interface)
- 6 karakter terakhir — nomor unik perangkat
- IP Address seperti nama (bisa berubah); MAC Address seperti sidik jari (permanen)
- MAC Spoofing — memalsukan MAC Address agar terlihat sebagai device lain
- MAC Filtering — kontrol akses berbasis MAC; rentan di-spoof

### Ping & ICMP

- Ping — tool cek koneksi dan latensi, built-in semua OS
- ICMP — protokol yang dipakai ping (bukan TCP/UDP)
- Cara kerja — kirim Echo Request → terima Echo Reply → hitung RTT
- ping [IP/domain] — sintaks dasar
- time=X ms (per baris) — latensi SATU paket, bukan total sesi
- time Xms (di statistik) — total DURASI sesi ping berjalan
- 0% packet loss — koneksi sehat
- TTL — batas lompatan paket; bisa estimasi jarak ke target
- Ping = langkah pertama network reconnaissance sebelum Nmap
