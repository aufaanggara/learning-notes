# Resume Materi: Keamanan Jaringan (Firewall, VPN, Proxy Server, Security Zone, Subnetting) — 11 Oktober 2026

Resume ini merangkum seluruh materi room: firewall, VPN beserta protokolnya, security zone, proxy server, subnetting dan CIDR, network protocol, serta keamanan Wi-Fi. Room ini bersifat teori murni, sehingga tidak ada command atau switch yang dibahas.

&nbsp;

## 1. Firewall

### 1.1 Definisi dan Fungsi

**Firewall** adalah perangkat keamanan jaringan yang memantau traffic masuk dan keluar jaringan, lalu mengizinkan atau memblokirnya berdasarkan aturan keamanan (security rules) yang ditetapkan kebijakan organisasi.

Salah satu mekanismenya adalah **port filtering**, yaitu memblokir atau mengizinkan port tertentu. Contoh: hanya port 443 (HTTPS) dan port 25 (email) yang diizinkan, sisanya diblokir.

Firewall tradisional memutuskan berdasarkan nomor port dan alamat IP dari paket data. Firewall juga disebut sebagai network virtual appliance (NVA) atau perangkat hardware yang memeriksa traffic sebelum masuk ke private network.

### 1.2 Bentuk Firewall

- **Hardware firewall**: perangkat fisik yang memeriksa setiap paket sebelum masuk jaringan; bentuk pertahanan paling dasar.
- **Software firewall**: program di komputer atau server dengan fungsi sama. Lebih murah dan tidak butuh ruang, tetapi menambah beban pemrosesan perangkat. Di komputer ia menganalisis traffic komputer itu; di server ia melindungi semua perangkat yang terhubung ke server.
- **Cloud-based firewall (FaaS)**: software firewall yang di-hosting penyedia cloud sebagai layanan. Aturan diatur lewat antarmuka penyedia, traffic disaring sebelum mencapai jaringan on-site, dan aset di cloud ikut terlindungi.

### 1.3 Stateless vs Stateful

- **Stateless**: bekerja berdasarkan aturan yang sudah ditentukan administrator dan tidak melacak informasi paket. Aturan harus dikonfigurasi untuk dua arah. Tidak mendeteksi tren mencurigakan, sehingga dianggap kurang aman.
- **Stateful**: melacak informasi yang melewatinya dan menyaring ancaman secara proaktif. Menggunakan **state table** untuk melacak koneksi sehingga cukup satu aturan satu arah, karena traffic balik dicocokkan dengan sesi yang ada.

### 1.4 Next Generation Firewall (NGFW)

**NGFW** adalah firewall paling canggih, melampaui stateful firewall. Kemampuannya:

- **Stateful inspection** untuk traffic masuk dan keluar.
- **Deep packet inspection**: sejenis packet sniffing yang memeriksa isi paket dan bertindak bila ada ancaman.
- **Intrusion prevention**: mendeteksi ancaman dan memberi tahu administrator firewall.
- **Application aware**: inspeksi sampai application layer TCP/IP, sehingga aturan bisa berbasis aplikasi, bukan hanya IP dan port.
- Fitur tambahan umum: **Malware Sandboxing**, **Network Anti-Virus**, **URL and DNS Filtering**.
- Sebagian terhubung ke **cloud-based threat intelligence** agar cepat diperbarui terhadap ancaman baru.

&nbsp;

## 2. Virtual Private Network (VPN)

### 2.1 Latar Belakang dan Fungsi

Request internet melewati ISP menuju server tujuan, dan membawa informasi pribadi. Jika traffic disadap, aktivitas internet bisa dikaitkan dengan lokasi fisik dan data pribadi (misalnya rekening bank dan kartu kredit).

**VPN** adalah layanan keamanan jaringan yang mengubah IP publik, menyembunyikan lokasi virtual, dan mengenkripsi data in transit untuk menjaga confidentiality. VPN menyediakan server yang berperan sebagai gateway antara komputer dan internet, membentuk jalur mirip tunnel virtual. VPN juga memungkinkan koneksi tepercaya di atas jaringan yang tidak tepercaya.

### 2.2 Encapsulation dan Encrypted Tunnel

Header dan footer paket data memuat MAC address dan IP address tujuan. Jika paket dienkripsi penuh, router tidak bisa membaca alamat tersebut dan paket tidak sampai tujuan.

**Encapsulation** menyelesaikan masalah ini: VPN mengenkripsi paket, lalu membungkusnya dalam paket lain yang tetap terbaca router. Routing berjalan, data pribadi tetap tidak terbaca selama transit.

VPN juga memakai **encrypted tunnel** antara perangkat dan server VPN. Tunnel hanya bisa dibuka dengan cryptographic key yang sesuai.

### 2.3 Jenis VPN

- **Remote access VPN**: dipakai individu untuk menghubungkan perangkat pribadi ke VPN server lewat internet, serta mengenkripsi data yang dikirim dan diterima perangkat tersebut.
- **Site-to-site VPN**: dipakai enterprise untuk memperluas jaringan ke lokasi lain, berguna bagi organisasi dengan banyak kantor. Lebih rumit dikonfigurasi dan dikelola dibanding remote access VPN.

Enterprise memakai VPN untuk melindungi komunikasi perangkat pengguna ke resource perusahaan (server atau virtual machine yang menjalankan aplikasi bisnis). VPN yang tepercaya juga meminimalkan aksesnya sendiri terhadap aktivitas pengguna melalui enkripsi kuat.

### 2.4 VPN Protocol: WireGuard vs IPSec

**VPN protocol** adalah seperangkat aturan yang menentukan bagaimana data bergerak antar **endpoint** (perangkat apa pun di jaringan: komputer, mobile device, server) dan bagaimana tunnel terbentuk. Penyedia VPN berbeda menyediakan protokol berbeda.

| Aspek | WireGuard | IPSec |
|---|---|---|
| Usia | Relatif baru | Lebih tua, salah satu yang paling awal |
| Kompleksitas | Sederhana | Lebih kompleks |
| Performa | Cepat, kode lebih sedikit | Standar |
| Dukungan koneksi | Site-to-site dan client-server | Umum untuk site-to-site |

**WireGuard** berkecepatan tinggi dengan enkripsi tingkat lanjut, open source (mudah di-deploy dan di-debug), cocok untuk streaming dan unduhan besar.

**IPSec** mengenkripsi dan mengautentikasi paket data untuk koneksi aman. Banyak sistem operasi mendukungnya. Sebagian klien memilihnya karena riwayat panjang, pengujian keamanan ekstensif, dan adopsi luas.

Pemilihan bergantung pada kecepatan koneksi, kompatibilitas infrastruktur, dan kebutuhan bisnis atau individu.

### 2.5 SD-WAN

**Software-defined wide area network (SD-WAN)** adalah layanan WAN virtual yang menghubungkan pengguna ke aplikasi secara aman lintas lokasi dan jarak geografis jauh. Organisasi makin sering menggabungkan VPN dan SD-WAN sebagai layanan berbasis cloud.

&nbsp;

## 3. Network Segmentation dan Security Zone

### 3.1 Konsep

**Network segmentation** membagi jaringan menjadi segmen dengan hak akses dan aturan keamanan masing-masing. **Security zone** adalah segmen jaringan hasil teknik ini yang melindungi jaringan internal dari internet. Fungsinya: menjadi barrier bagi jaringan internal, menjaga privasi antar kelompok, dan mencegah masalah menyebar ke seluruh jaringan.

Contoh: Wi-Fi tamu hotel dipisahkan dari jaringan terenkripsi staf. Di universitas, subnet fakultas dipisahkan dari subnet mahasiswa, sehingga kontaminasi di satu subnet bisa diisolasi.

### 3.2 Klasifikasi Zona

- **Uncontrolled zone**: jaringan di luar kendali organisasi, misalnya internet.
- **Controlled zone**: subnet yang melindungi jaringan internal dari uncontrolled zone. Berisi tiga sub-zona:

**Demilitarized zone (DMZ)** adalah lapisan terluar berisi layanan public-facing: web server, proxy server yang menghosting website publik, DNS server, serta email dan file server untuk komunikasi eksternal. DMZ berperan sebagai perimeter jaringan internal.

**Internal network** berisi server privat dan data yang perlu dilindungi.

**Restricted zone** berada di dalam internal network dan melindungi informasi sangat rahasia yang hanya bisa diakses karyawan dengan privilege tertentu.

### 3.3 Penempatan Firewall

DMZ idealnya berada di antara dua firewall: satu menyaring traffic dari luar ke DMZ, satu menyaring traffic yang masuk ke internal network. Restricted zone dilindungi firewall tambahan. Dengan begitu, serangan yang menembus DMZ tidak menyebar ke internal network, dan serangan yang menembus internal network tidak mencapai restricted zone.

Security analyst bertugas mengatur access control policy pada firewall tersebut, misalnya membatasi IP dan port. Contoh: hanya HTTPS yang boleh mengakses web server di DMZ.

&nbsp;

## 4. Proxy Server

### 4.1 Definisi dan Mekanisme

**Proxy server** adalah dedicated server di antara internet dan jaringan internal yang memenuhi permintaan klien dengan meneruskannya ke server lain. Saat ada permintaan dari internet, proxy menentukan apakah koneksi aman.

Proxy memakai IP publik yang berbeda dari private network dan memanfaatkan **network address translation (NAT)**, sehingga IP internal tersembunyi. Klien yang menerima respons HTTPS akan melihat IP yang terdistorsi atau tidak ada IP, bukan IP asli web server.

### 4.2 Fungsi Keamanan

- Memblokir akses ke website tidak aman atau terlarang di jaringan organisasi.
- Menyimpan data yang sering diminta di temporary memory (cache), sehingga mengurangi kontak dengan internal server.
- Dapat dikonfigurasi dengan aturan seperti firewall, misalnya filter untuk situs yang teridentifikasi mengandung malware.

### 4.3 Jenis Proxy

- **Forward proxy**: mengatur dan membatasi akses internet pengguna internal. Menerima traffic keluar dari karyawan, menyembunyikan IP pengguna, menyetujui request, lalu meneruskannya ke tujuan.
- **Reverse proxy**: kebalikan forward proxy. Menerima traffic dari pihak eksternal, menyetujuinya, dan meneruskannya ke internal server. Melindungi web server berisi data rahasia agar IP-nya tidak terekspos.
- **Email proxy**: memfilter spam dengan memverifikasi apakah alamat pengirim dipalsukan, sehingga menurunkan risiko **phishing** yang menyamar sebagai orang yang dikenal organisasi.

Studi kasus: sebuah ISP broadband besar di AS memakai email proxy dengan beberapa lapisan anti-spam sebelum pesan dikirim. Sekitar 95% pesan ditandai spam, dan filter bisa di-scale tanpa memengaruhi platform email.

&nbsp;

## 5. Subnetting dan CIDR

### 5.1 Subnetting

**Subnetting** membagi satu jaringan besar menjadi kelompok kecil terorganisir bernama **subnet**. Subnet didefinisikan oleh kombinasi **IP address** dan **network mask** tiap perangkat, sehingga membentuk "jaringan di dalam jaringan".

Manfaatnya:

- Jaringan lebih efisien dan bandwidth terpakai optimal.
- Komunikasi antar perangkat dalam subnet yang sama dijaga switch tetap di subnet itu, sehingga lebih cepat.
- Dapat dipakai membentuk security zone.
- Organisasi bisa membuat jaringan sendiri tanpa meminta IP tambahan dari ISP.

Subnetwork terisolasi dibentuk lewat tiga pendekatan: physical isolation, routing configuration, dan firewall.

### 5.2 CIDR

**Classless Inter-Domain Routing (CIDR)** adalah metode memberikan subnet mask pada IP address untuk membentuk subnet. CIDR menggantikan **classful addressing** (Class A sampai E, dipakai sejak 1980-an) yang kehabisan kapasitas pada 1990-an akibat melonjaknya jumlah perangkat.

Alamat CIDR berformat IPv4 ditambah garis miring dan angka yang disebut **IP network prefix**.

```
198.51.100.0/24
```

Notasi di atas mencakup 198.51.100.0 sampai 198.51.100.255. CIDR mengurangi entri routing table dan menyediakan lebih banyak IP dalam jaringan. Untuk tahap ini cukup pahami konsepnya; matematika detail CIDR dipelajari di tahap lanjut.

&nbsp;

## 6. Network Protocol

### 6.1 Tiga Kategori

- **Communication protocol**: membangun koneksi antar-server. Contoh: **TCP**, **UDP**, **SMTP** (kerangka komunikasi email).
- **Management protocol**: troubleshooting masalah jaringan. Contoh: **ICMP**.
- **Security protocol**: enkripsi data in transit. Contoh: **IPSec**, **SSL/TLS**.

Protokol adalah aturan bersama semua perangkat jaringan tentang cara mentransfer data.

### 6.2 Protokol Umum Lain

- **HTTP**: protokol application layer untuk komunikasi browser dan web server.
- **DNS**: protokol application layer yang memetakan host name ke IP address.
- **ARP**: protokol network layer yang memetakan IP address ke MAC address di local area network.

&nbsp;

## 7. Keamanan Wi-Fi

Protokol keamanan wireless: **WEP**, **WPA**, **WPA2**, **WPA3**. WPA3 mengenkripsi traffic dengan cipher **AES** dari perangkat ke wireless access point.

WPA2 dan WPA3 punya dua mode: **personal** (cocok untuk jaringan rumah) dan **enterprise** (umumnya untuk jaringan dan aplikasi bisnis).

&nbsp;

## 8. Glosarium

- **Firewall**: memantau dan memfilter traffic masuk/keluar berdasarkan aturan.
- **Port filtering**: memblokir/mengizinkan berdasarkan nomor port.
- **Stateless**: firewall berbasis aturan tetap, tanpa melacak paket.
- **Stateful**: firewall yang melacak koneksi lewat state table.
- **NGFW**: firewall dengan deep packet inspection, intrusion prevention, dan application awareness.
- **FaaS**: firewall sebagai layanan dari penyedia cloud.
- **VPN**: layanan yang mengenkripsi data dan menyamarkan IP serta lokasi virtual.
- **Encapsulation**: membungkus paket terenkripsi dalam paket lain agar tetap bisa dirutekan.
- **Encrypted tunnel**: jalur terenkripsi antara perangkat dan server VPN.
- **Endpoint**: perangkat apa pun yang terhubung ke jaringan.
- **Remote access VPN / Site-to-site VPN**: VPN untuk perangkat individu / antar lokasi enterprise.
- **WireGuard / IPSec**: dua VPN protocol (modern dan sederhana / lama dan luas dipakai).
- **SD-WAN**: layanan WAN virtual untuk menghubungkan pengguna ke aplikasi secara aman.
- **Network segmentation**: membagi jaringan menjadi segmen dengan aturan masing-masing.
- **Security zone**: segmen hasil segmentasi sebagai barrier keamanan.
- **Uncontrolled / Controlled zone**: zona di luar / di dalam kendali organisasi.
- **DMZ**: perimeter berisi layanan public-facing.
- **Internal network / Restricted zone**: jaringan privat / zona informasi sangat rahasia.
- **Proxy server**: perantara yang meneruskan request dan menyembunyikan IP internal.
- **NAT**: penerjemahan alamat IP yang dipakai proxy.
- **Forward / Reverse / Email proxy**: proxy untuk traffic keluar / masuk / verifikasi pengirim email.
- **Phishing**: serangan yang menyamar sebagai pihak tepercaya.
- **Subnetting / Subnet**: proses pembagian jaringan / hasil pembagiannya.
- **Network mask**: pasangan IP address yang mendefinisikan subnet.
- **CIDR / IP network prefix**: metode subnet mask berbasis notasi garis miring / angka setelahnya.
- **Classful addressing**: sistem lama pengelompokan IP Class A sampai E.
- **TCP, UDP, SMTP, ICMP, IPSec, SSL/TLS, HTTP, DNS, ARP**: lihat bagian 6.
- **WEP, WPA, WPA2, WPA3, AES**: lihat bagian 7.

&nbsp;

## 9. Tools & Platform Rujukan

- **IPAddressGuide**: online conversion tool untuk berlatih mengonversi CIDR ke IPv4 dan sebaliknya. URL tidak disebutkan eksplisit di materi.

Materi juga merujuk ke video tentang security zones sebagai penyegar, tetapi itu bukan tools eksternal.

&nbsp;

## 10. Catatan Ringkas untuk Ditulis Tangan

### Firewall

- Firewall — pantau/filter traffic masuk-keluar berdasarkan rules
- Port filtering — blokir/izinkan nomor port (443 HTTPS, 25 email)
- Hardware FW — perangkat fisik, cek tiap paket
- Software FW — program di PC/server, murah, beban prosesing
- Cloud FW / FaaS — di-host cloud, filter sebelum jaringan on-site
- Stateless — aturan tetap, tanpa tracking, 2 arah, kurang aman
- Stateful — tracking via state table, 1 aturan 1 arah, proaktif
- NGFW — stateful + deep packet inspection + intrusion prevention
- NGFW — application aware (app layer), aturan per aplikasi
- NGFW extra — Malware Sandboxing, Network Anti-Virus, URL and DNS Filtering
- NGFW — terhubung cloud threat intelligence

### VPN

- VPN — ubah IP publik, sembunyikan lokasi virtual, enkripsi data
- Encapsulation — paket terenkripsi dibungkus paket lain agar router tetap bisa baca
- Encrypted tunnel — perangkat ke server VPN, dibuka dengan cryptographic key
- Remote access VPN — individu, perangkat ke VPN server
- Site-to-site VPN — enterprise, hubungkan kantor/jaringan, lebih kompleks
- Endpoint — perangkat di jaringan (PC, mobile, server)
- VPN protocol — aturan pergerakan data antar endpoint
- WireGuard — baru, cepat, sederhana, open source, site-to-site dan client-server
- IPSec — lama, kompleks, adopsi luas, umum site-to-site
- SD-WAN — WAN virtual, hubungkan pengguna ke aplikasi lintas lokasi

### Security Zone

- Network segmentation — bagi jaringan jadi segmen, akses/aturan sendiri
- Security zone — barrier, jaga privasi, cegah penyebaran
- Uncontrolled zone — di luar kendali (internet)
- Controlled zone — subnet pelindung jaringan internal
- DMZ — layanan public-facing (web, proxy, DNS, email, file server)
- Internal network — server dan data privat
- Restricted zone — info sangat rahasia, privilege khusus
- Firewall — DMZ diapit 2 firewall, restricted zone firewall tambahan
- Tugas analyst — atur access control (IP, port), contoh hanya HTTPS ke DMZ
- Contoh segmentasi — Wi-Fi tamu vs staf hotel; subnet fakultas vs mahasiswa

### Proxy Server

- Proxy server — perantara, IP publik beda dari jaringan privat, sembunyikan IP internal
- NAT — penerjemahan alamat yang dipakai proxy
- Fungsi — blokir situs tidak aman, cache di temporary memory, aturan ala firewall
- Forward proxy — traffic keluar, sembunyikan IP pengguna, approve request
- Reverse proxy — traffic masuk ke internal server, lindungi IP server
- Email proxy — cek pengirim palsu, kurangi phishing
- Studi kasus — email proxy ISP, sekitar 95% pesan ditandai spam

### Subnetting dan CIDR

- Subnetting — bagi jaringan besar jadi subnet
- Subnet — didefinisikan IP address + network mask
- Manfaat — efisiensi, kecepatan dalam subnet, security zone, tanpa IP tambahan dari ISP
- Isolasi subnet — physical isolation, routing configuration, firewall
- CIDR — subnet mask berformat /prefix, gantikan classful (Class A-E)
- IP network prefix — angka setelah /
- Contoh — 198.51.100.0/24 = 198.51.100.0 sampai 198.51.100.255
- CIDR — kurangi entri routing table, lebih banyak IPv4

### Protocol dan Wi-Fi

- Communication protocol — koneksi antar-server (TCP, UDP, SMTP)
- Management protocol — troubleshooting (ICMP)
- Security protocol — enkripsi in transit (IPSec, SSL/TLS)
- HTTP — application layer, browser dan web server
- DNS — application layer, host name ke IP
- ARP — network layer, IP ke MAC di LAN
- Wireless security — WEP, WPA, WPA2, WPA3
- WPA3 — cipher AES, perangkat ke access point
- WPA2/WPA3 — mode personal (rumah), enterprise (bisnis)

### Tools

- IPAddressGuide — konversi CIDR dan IPv4 (tanpa URL di materi)
