# Bab 11: IPv4 Addressing and Subnetting

> Kumpulan ringkasan lengkap Sub-Bab 11.1 – 11.8 mengenai struktur alamat IPv4, jenis transmisi, jenis-jenis alamat IPv4, segmentasi jaringan, subnetting, hingga VLSM (Variable Length Subnet Mask).

## Daftar Isi

- [Sub-Bab 11.1: IPv4 Address Structure](#ringkasan-sub-bab-111-ipv4-address-structure)
- [Sub-Bab 11.2: IPv4 Unicast, Broadcast, dan Multicast](#ringkasan-sub-bab-112-ipv4-unicast-broadcast-dan-multicast)
- [Sub-Bab 11.3: Types of IPv4 Addresses](#ringkasan-sub-bab-113-types-of-ipv4-addresses)
- [Sub-Bab 11.4: Network Segmentation](#ringkasan-sub-bab-114-network-segmentation)
- [Sub-Bab 11.5: Subnet an IPv4 Network](#ringkasan-sub-bab-115-subnet-an-ipv4-network)
- [Sub-Bab 11.6: Subnet a Slash 16 and a Slash 8 Prefix](#ringkasan-sub-bab-116-subnet-a-slash-16-and-a-slash-8-prefix)
- [Sub-Bab 11.7: Subnet to Meet Requirements](#ringkasan-sub-bab-117-subnet-to-meet-requirements)
- [Sub-Bab 11.8: VLSM (Variable Length Subnet Mask)](#ringkasan-sub-bab-118-vlsm-variable-length-subnet-mask)

---

# Ringkasan Sub-Bab 11.1: IPv4 Address Structure

---

## A. Bagian Jaringan dan Host (Network and Host Portions)

- **Alamat IPv4 adalah alamat hierarkis 32-bit** yang secara struktural terbagi menjadi dua bagian utama, yaitu **network portion (bagian jaringan)** dan **host portion (bagian host)**. Kedua bagian ini tidak bisa dipisahkan secara sembarangan — pembagiannya ditentukan oleh subnet mask yang digunakan.

- **Network portion** berfungsi untuk mengidentifikasi jaringan tempat sebuah perangkat berada, sedangkan **host portion** berfungsi untuk mengidentifikasi perangkat spesifik di dalam jaringan tersebut. Analogi sederhananya: network portion adalah nama jalan dan kota, sedangkan host portion adalah nomor rumahnya.

- Untuk menentukan mana bagian jaringan dan mana bagian host, **seluruh aliran 32-bit harus dilihat secara berurutan**, dan perbandingannya dilakukan dengan menggunakan subnet mask.

**Contoh visual:**

| | Network Portion | Host Portion |
|---|---|---|
| IPv4 Address | 192.168.10 | .10 |
| Biner | 11000000.10101000.00001010 | 00001010 |

---

## B. Subnet Mask

- **Subnet mask adalah deretan 32-bit** yang digunakan untuk membedakan bagian jaringan dari bagian host pada sebuah alamat IPv4. Proses perbandingannya dilakukan **bit per bit dari kiri ke kanan**.

- Proses teknis yang digunakan untuk mengidentifikasi bagian jaringan dan bagian host dari sebuah alamat IPv4 disebut **ANDing** — yaitu operasi logika Boolean yang membandingkan bit alamat IP dengan bit subnet mask secara bersamaan.

- Bagian subnet mask yang bernilai **1 (atau 255 dalam desimal)** menandai bagian jaringan, sedangkan bagian yang bernilai **0** menandai bagian host. Analogi yang tepat: subnet mask seperti stempel highlighter — bagian yang di-highlight adalah jaringan, bagian yang kosong adalah host.

**Contoh visual:**

| | Network Portion | Host Portion |
|---|---|---|
| IPv4 Address | 192.168.10 | .10 |
| Subnet Mask | 255.255.255 | .0 |
| Biner Mask | 11111111.11111111.11111111 | 00000000 |

---

## C. Prefix Length (Panjang Prefix)

- **Prefix length adalah metode alternatif yang lebih praktis** untuk merepresentasikan subnet mask, sehingga tidak perlu selalu menuliskan format panjang seperti "255.255.255.0".

- Prefix length dihitung dengan **menghitung jumlah bit yang bernilai 1** dalam subnet mask, kemudian ditulis menggunakan **slash notation (notasi garis miring)**, misalnya `/24` berarti ada 24 bit bernilai 1 dalam subnet mask tersebut.

- Prefix length itu seperti kode pos yang dipersingkat — daripada menulis alamat panjang, cukup tulis "/24" untuk menyatakan 24 bit pertama adalah identitas jaringan.

**Tabel lengkap Subnet Mask dan Prefix Length:**

| Subnet Mask | 32-bit Address | Prefix Length |
|---|---|---|
| 255.0.0.0 | 11111111.00000000.00000000.00000000 | /8 |
| 255.255.0.0 | 11111111.11111111.00000000.00000000 | /16 |
| 255.255.255.0 | 11111111.11111111.11111111.00000000 | /24 |
| 255.255.255.128 | 11111111.11111111.11111111.10000000 | /25 |
| 255.255.255.192 | 11111111.11111111.11111111.11000000 | /26 |
| 255.255.255.224 | 11111111.11111111.11111111.11100000 | /27 |
| 255.255.255.240 | 11111111.11111111.11111111.11110000 | /28 |
| 255.255.255.248 | 11111111.11111111.11111111.11111000 | /29 |
| 255.255.255.252 | 11111111.11111111.11111111.11111100 | /30 |

---

## D. Determining the Network: Logical AND

- **Operasi Boolean Logical AND** adalah operasi matematika biner yang digunakan untuk menentukan alamat jaringan dari sebuah host. Prinsip dasarnya: **hanya kombinasi 1 AND 1 yang menghasilkan 1**, semua kombinasi lainnya menghasilkan 0.

**Tabel kebenaran Logical AND:**

| Operasi | Hasil |
|---|---|
| 1 AND 1 | = 1 |
| 0 AND 1 | = 0 |
| 1 AND 0 | = 0 |
| 0 AND 0 | = 0 |

- Dalam konteks jaringan, **alamat IPv4 host di-AND-kan secara logis, bit per bit, dengan subnet mask** untuk menghasilkan alamat jaringan. Proses ini seperti filter saringan — hanya bit yang bernilai 1 di kedua sisi (IP dan mask) yang lolos; sisanya langsung menjadi 0.

**Contoh perhitungan AND:**

| | 192 | 168 | 10 | 10 |
|---|---|---|---|---|
| IPv4 host address | 11000000 | 10101000 | 00001010 | 00001010 |
| AND | | | | |
| Subnet Mask (255.255.255.0) | 11111111 | 11111111 | 11111111 | 00000000 |
| **= IPv4 network address** | **11000000** | **10101000** | **00001010** | **00000000** |

**Hasil: 192.168.10.0** — inilah alamat jaringannya.

---

## E. Network, Host, dan Broadcast Address

- Dalam setiap jaringan IPv4, terdapat **tiga jenis alamat IP** yang memiliki fungsi berbeda-beda dan tidak boleh dicampuradukkan penggunaannya.

**1. Network Address (Alamat Jaringan)**
Alamat yang mewakili seluruh jaringan secara keseluruhan. Pada alamat ini, **semua bit host bernilai 0**. Alamat ini **tidak dapat digunakan** untuk perangkat manapun karena hanya berfungsi sebagai identitas jaringan. Contoh: `192.168.10.0`

**2. First Usable Host Address (Alamat Host Pertama)**
Alamat host pertama yang dapat digunakan untuk perangkat. Bagian host bernilai **semua 0 kecuali bit terakhir yang bernilai 1**. Contoh: `192.168.10.1`

**3. Last Usable Host Address (Alamat Host Terakhir)**
Alamat host terakhir yang dapat digunakan. Bagian host bernilai **semua 1 kecuali bit terakhir yang bernilai 0**. Contoh: `192.168.10.254`

**4. Broadcast Address (Alamat Broadcast)**
Alamat khusus yang digunakan untuk mengirim data ke **semua perangkat dalam satu jaringan sekaligus**. Pada alamat ini, **semua bit host bernilai 1**. Alamat ini juga **tidak dapat digunakan** untuk perangkat. Contoh: `192.168.10.255`

**Tabel ringkasan untuk jaringan 192.168.10.0/24:**

| Jenis Alamat | Alamat | Host Bits |
|---|---|---|
| Network Address | 192.168.10.0 | Semua 0 (00000000) |
| First Usable Host | 192.168.10.1 | 00000001 |
| Last Usable Host | 192.168.10.254 | 11111110 |
| Broadcast Address | 192.168.10.255 | Semua 1 (11111111) |

> **Analogi penting:** Bayangkan sebuah gedung apartemen. `192.168.10.0` adalah nama gedungnya sendiri (network address) — bukan nomor unit. `192.168.10.255` adalah pengeras suara di lobby yang suaranya terdengar ke semua unit sekaligus (broadcast). Unit yang bisa dihuni hanya dari nomor `.1` sampai `.254`.

---

Siap lanjut ke **Bab 11.2**? Instruksikan kapan kamu siap!

---

# Ringkasan Sub-Bab 11.2: IPv4 Unicast, Broadcast, dan Multicast

---

## Pengantar

Setiap paket IPv4 yang dikirimkan dalam jaringan memiliki **jenis transmisi** yang menentukan kepada siapa paket tersebut ditujukan. Terdapat **tiga jenis transmisi** dalam komunikasi IPv4, yaitu **Unicast**, **Broadcast**, dan **Multicast**. Masing-masing memiliki karakteristik, cara kerja, dan skenario penggunaan yang berbeda-beda.

---

## A. Unicast

- **Unicast adalah jenis transmisi di mana sebuah paket dikirimkan ke satu alamat IP tujuan secara spesifik.** Artinya, hanya satu perangkat penerima yang dituju, dan perangkat lain dalam jaringan yang sama tidak akan menerima paket tersebut meskipun berada di jaringan yang sama.

- Unicast merupakan jenis komunikasi yang **paling umum digunakan** dalam jaringan sehari-hari, misalnya ketika sebuah komputer mengakses sebuah server web, atau ketika satu perangkat mengirim data ke perangkat lain secara langsung.

- **Contoh konkret:** PC dengan alamat `172.16.4.1/24` mengirimkan paket unicast langsung ke printer di alamat `172.16.4.253/24`. Paket tersebut hanya diterima oleh printer tersebut — perangkat lain di jaringan yang sama tidak menerima paket itu sama sekali.

- **Alur transmisi:** Paket dikirim dari satu sumber → langsung ke satu tujuan yang sudah ditentukan → perangkat lain tidak terganggu.

> **Analogi:** Unicast seperti mengirim pesan WhatsApp pribadi ke satu orang tertentu. Hanya orang itu yang menerima pesannya, bukan semua orang di daftar kontakmu.

---

## B. Broadcast

- **Broadcast adalah jenis transmisi di mana sebuah paket dikirimkan ke semua perangkat dalam satu jaringan secara bersamaan.** Setiap perangkat yang berada dalam broadcast domain yang sama akan menerima dan memproses paket tersebut, tanpa terkecuali.

- Alamat broadcast yang digunakan adalah **255.255.255.255** yang dikenal sebagai **Limited Broadcast** — artinya broadcast ini hanya berlaku dalam satu jaringan lokal dan tidak diteruskan ke jaringan lain.

- **Contoh konkret:** PC dengan alamat `172.16.4.1/24` mengirimkan paket broadcast ke alamat tujuan `255.255.255.255`. Semua perangkat yang berada dalam jaringan `172.16.4.0/24` akan menerima paket tersebut secara serentak.

- **Peran router dalam broadcast:** Router **tidak akan meneruskan (forward) paket broadcast** ke jaringan lain. Ini berarti broadcast hanya berlaku dan terbatas di dalam satu broadcast domain saja. Router bertindak sebagai batas (boundary) yang menghentikan penyebaran broadcast.

- Broadcast sering digunakan oleh protokol-protokol jaringan tertentu, misalnya:
    - **ARP (Address Resolution Protocol)** menggunakan broadcast untuk menemukan alamat MAC dari sebuah alamat IP.
    - **DHCP Discover** — host mengirimkan broadcast untuk menemukan server DHCP di jaringan.

> **Analogi:** Broadcast seperti guru yang mengumumkan sesuatu lewat mikrofon di dalam kelas. Semua orang di kelas itu mendengar pengumuman tersebut, tetapi pengumuman tidak keluar dari pintu kelas ke kelas lain — router adalah dinding pembatasnya.

---

## C. Multicast

- **Multicast adalah jenis transmisi di mana sebuah paket dikirimkan ke sebuah grup alamat multicast tertentu**, bukan ke semua perangkat (seperti broadcast) dan bukan ke satu perangkat saja (seperti unicast). Hanya perangkat yang **secara aktif terdaftar sebagai anggota grup multicast** tersebut yang akan menerima paket.

- **Rentang alamat multicast IPv4** adalah blok `224.0.0.0` hingga `239.255.255.255`. Alamat-alamat dalam rentang ini digunakan khusus sebagai penanda grup multicast dan tidak bisa digunakan sebagai alamat host biasa.

- **Contoh konkret:** PC dengan alamat `172.16.4.1` mengirimkan paket multicast ke alamat grup `224.10.10.5`. Dalam jaringan tersebut, hanya perangkat `172.16.4.2`, `172.16.4.3`, dan `172.16.4.4` yang terdaftar sebagai anggota grup `224.10.10.5`, sehingga hanya ketiga perangkat itulah yang menerima paket. Perangkat lain yang tidak terdaftar dalam grup tersebut tidak menerima paket sama sekali, meskipun berada di jaringan yang sama.

- Multicast sangat berguna untuk **efisiensi jaringan** karena memungkinkan pengiriman data ke banyak penerima sekaligus tanpa harus mengirimkan paket secara individual (seperti unicast) maupun membanjiri seluruh jaringan (seperti broadcast).

- Multicast umumnya digunakan dalam aplikasi seperti **video streaming**, **konferensi video**, dan **routing protocol** tertentu yang membutuhkan komunikasi satu-ke-banyak secara selektif.

> **Analogi:** Multicast seperti grup WhatsApp. Hanya anggota grup yang menerima pesan yang dikirimkan ke grup tersebut. Orang di luar grup tidak akan menerima apapun, meskipun mereka ada di jaringan (daftar kontak) yang sama.

---

## Tabel Perbandingan Ketiga Jenis Transmisi

| Aspek | Unicast | Broadcast | Multicast |
|---|---|---|---|
| **Penerima** | Satu perangkat spesifik | Semua perangkat dalam jaringan | Anggota grup multicast saja |
| **Alamat tujuan** | Alamat IP spesifik host | 255.255.255.255 | 224.0.0.0 – 239.255.255.255 |
| **Diteruskan router?** | Ya | Tidak | Tergantung konfigurasi |
| **Efisiensi** | Efisien untuk 1 penerima | Boros jika penerima sedikit | Efisien untuk banyak penerima selektif |
| **Contoh penggunaan** | Akses web, transfer file | ARP, DHCP Discover | Video streaming, routing protocol |

---

Siap lanjut ke **Bab 11.3**? Instruksikan kapan kamu siap!

---

# Ringkasan Sub-Bab 11.3: Types of IPv4 Addresses

---

## Pengantar

Tidak semua alamat IPv4 berfungsi dengan cara yang sama. Terdapat beberapa **kategori dan jenis alamat IPv4** yang masing-masing memiliki fungsi, cakupan, dan aturan penggunaan yang berbeda. Sub-bab ini membahas perbedaan antara alamat **publik dan privat**, mekanisme **NAT**, alamat-alamat **khusus (special use)**, sistem **classful addressing** lama, serta bagaimana **IANA dan RIR** mengelola distribusi alamat IP di seluruh dunia.

---

## A. Public and Private IPv4 Addresses (Alamat Publik dan Privat)

- **Alamat IPv4 publik** adalah alamat yang dapat **diarahkan secara global (globally routable)** di seluruh internet, artinya paket dengan alamat publik dapat dikirimkan dan diterima melewati router-router ISP di seluruh dunia. Alamat publik bersifat **unik secara global** — tidak ada dua perangkat di internet yang boleh menggunakan alamat publik yang sama secara bersamaan.

- **Alamat IPv4 privat** adalah blok-blok alamat yang didefinisikan dalam **RFC 1918** dan diperuntukkan khusus untuk penggunaan internal dalam jaringan lokal (LAN) organisasi, perusahaan, atau rumah tangga. Alamat privat **tidak bersifat unik secara global** — artinya alamat yang sama boleh digunakan secara berulang di jaringan-jaringan privat yang berbeda di seluruh dunia tanpa konflik, karena alamat ini **tidak dapat diarahkan secara global (not globally routable)** di internet publik.

**Tiga blok alamat privat berdasarkan RFC 1918:**

| Network Address dan Prefix | RFC 1918 Private Address Range |
|---|---|
| 10.0.0.0/8 | 10.0.0.0 – 10.255.255.255 |
| 172.16.0.0/12 | 172.16.0.0 – 172.31.255.255 |
| 192.168.0.0/16 | 192.168.0.0 – 192.168.255.255 |

> **Analogi:** Alamat privat seperti nomor ekstensi telepon internal kantor. Kamu bisa menghubungi rekan kerja dengan ekstensi 101, 102, atau 103 — tetapi nomor ekstensi itu tidak bisa dihubungi langsung dari luar kantor. Orang luar harus menghubungi nomor utama kantor (alamat publik) terlebih dahulu.

---

## B. Routing to the Internet — Network Address Translation (NAT)

- Karena alamat privat tidak dapat diarahkan di internet publik, diperlukan sebuah mekanisme untuk memungkinkan perangkat beralamat privat tetap bisa berkomunikasi dengan internet. Mekanisme tersebut disebut **Network Address Translation (NAT)**.

- **NAT bekerja dengan cara menerjemahkan (translating) alamat IPv4 privat menjadi alamat IPv4 publik** sebelum paket dikirimkan ke internet, dan sebaliknya ketika paket balasan diterima dari internet. Dengan NAT, banyak perangkat beralamat privat dalam satu jaringan dapat berbagi satu atau beberapa alamat publik untuk berkomunikasi ke luar.

- **NAT biasanya diaktifkan pada router tepi (edge router)** — yaitu router yang berada di batas antara jaringan internal (intranet) dan internet. Router inilah yang melakukan proses penggantian alamat sumber dari privat ke publik setiap kali ada paket yang hendak keluar ke internet.

- **Contoh skenario NAT:** Tiga jaringan internal yang masing-masing menggunakan alamat privat — Network 1: `10.0.0.0/8`, Network 2: `172.16.0.0/16`, Network 3: `192.168.0.0/24` — semuanya dapat mengakses internet melalui ISP dengan cara router melakukan NAT, mengganti alamat sumber privat dengan alamat publik yang sah sebelum paket diteruskan ke internet.

> **Analogi:** NAT seperti resepsionis kantor. Surat dari dalam kantor (beralamat privat) tidak bisa langsung dikirim ke luar kota. Resepsionis mengganti alamat pengirim dengan alamat kantor resmi (alamat publik) sebelum surat tersebut dikirimkan ke pos.

---

## C. Special Use IPv4 Addresses (Alamat IPv4 Khusus)

Selain alamat publik dan privat, terdapat beberapa jenis alamat IPv4 yang diperuntukkan bagi **tujuan-tujuan teknis khusus** dan tidak dapat digunakan sebagai alamat host biasa.

### 1. Loopback Addresses (Alamat Loopback)

- **Rentang alamat loopback:** `127.0.0.0/8` (mencakup `127.0.0.1` hingga `127.255.255.254`), namun dalam praktiknya yang paling umum digunakan hanyalah **`127.0.0.1`**.

- **Fungsi utama:** Alamat loopback digunakan oleh sebuah host untuk **menguji apakah stack protokol TCP/IP pada perangkat tersebut sudah berfungsi dengan benar**, tanpa perlu mengirim paket ke jaringan eksternal. Ketika sebuah perangkat melakukan `ping 127.0.0.1`, paket tersebut tidak keluar ke jaringan sama sekali — paket hanya diloopback kembali ke perangkat itu sendiri.

- **Contoh penggunaan di terminal:**
```
ping 127.0.0.1
```
Output yang diharapkan:
```
Pinging 127.0.0.1 with 32 bytes of data:
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128
```
Jika ada balasan, berarti TCP/IP pada perangkat tersebut bekerja normal.

> **Analogi:** Alamat loopback seperti berbicara ke cermin. Kamu tidak mengirim suara ke orang lain — kamu hanya memastikan suaramu sendiri berfungsi dengan baik. Kalau ada balasan, berarti sistem TCP/IP-mu bekerja normal.

### 2. Link-Local Addresses (Alamat Link-Local)

- **Rentang alamat link-local:** `169.254.0.0/16` (mencakup `169.254.0.1` hingga `169.254.255.254`).

- Alamat ini umumnya dikenal dengan istilah **APIPA (Automatic Private IP Addressing)** atau **self-assigned addresses (alamat yang ditetapkan sendiri)**.

- **Fungsi utama:** Alamat link-local digunakan oleh **klien Windows** untuk melakukan konfigurasi alamat IP secara otomatis **ketika tidak ada server DHCP yang tersedia** di jaringan. Ketika komputer Windows gagal mendapatkan alamat IP dari server DHCP, sistem operasi secara otomatis akan menetapkan sendiri sebuah alamat dari rentang `169.254.x.x` agar perangkat tetap bisa berkomunikasi di jaringan lokal meskipun tanpa konfigurasi manual.

- Alamat link-local hanya berlaku dalam satu segmen jaringan lokal dan **tidak dapat diarahkan melewati router**.

> **Analogi:** Alamat link-local seperti nama panggilan darurat yang kamu buat sendiri ketika tidak ada yang memberikanmu nama resmi. Komputer Windows yang tidak bisa menemukan server DHCP akan "membuat nama sendiri" dengan alamat 169.254.x.x agar tetap bisa berkomunikasi di jaringan lokal.

---

## D. Legacy Classful Addressing (Pengalamatan Classful Lama)

- Sistem **classful addressing** pertama kali didefinisikan dalam **RFC 790 (1981)** yang mengalokasikan seluruh ruang alamat IPv4 ke dalam kelas-kelas tetap berdasarkan rentang nilai oktet pertamanya.

**Pembagian kelas-kelas alamat IPv4:**

| Kelas | Rentang Alamat | Total Jaringan | Total Host per Jaringan |
|---|---|---|---|
| **Class A** | 0.0.0.0/8 – 127.0.0.0/8 | 128 | 16.777.214 |
| **Class B** | 128.0.0.0/16 – 191.255.0.0/16 | 16.384 | 65.534 |
| **Class C** | 192.0.0.0/24 – 223.255.255.0/24 | 2.097.152 | 254 |
| **Class D** | 224.0.0.0 – 239.0.0.0 | — | Digunakan untuk multicast |
| **Class E** | 240.0.0.0 – 255.0.0.0 | — | Dicadangkan/eksperimental |

- **Distribusi ruang alamat secara proporsional:** Class A menempati **50%** dari total ruang alamat IPv4, Class B **25%**, Class C **12,5%**, serta Class D dan E masing-masing **12,5%**.

- **Kelemahan utama classful addressing:** Sistem ini sangat **tidak efisien dan boros alamat IPv4**. Misalnya, sebuah organisasi yang membutuhkan 300 host terpaksa menggunakan alamat Class B (yang menyediakan 65.534 host) karena Class C hanya menyediakan 254 host — sehingga puluhan ribu alamat terbuang sia-sia tanpa digunakan.

- Karena pemborosan yang masif ini, sistem classful addressing kemudian **digantikan dengan classless addressing (CIDR — Classless Inter-Domain Routing)** yang mengabaikan aturan kelas-kelas lama dan memungkinkan pengalokasian alamat yang jauh lebih fleksibel dan efisien sesuai kebutuhan aktual.

> **Analogi:** Classful addressing seperti toko yang hanya menjual kaos dalam 3 ukuran: S (kecil sekali), M (sedang), dan L (besar sekali). Kalau kamu butuh ukuran di antaranya, kamu tetap harus beli yang besar dan ukuran yang tersisa terbuang. Classless addressing hadir seperti toko yang menjual kaos dengan ukuran custom — pas sesuai kebutuhan, tidak ada yang terbuang.

---

## E. Assignment of IP Addresses — IANA dan RIR

- **IANA (Internet Assigned Numbers Authority)** adalah otoritas tertinggi yang bertanggung jawab untuk **mengelola dan mengalokasikan seluruh blok alamat IPv4 dan IPv6** di tingkat global. IANA tidak langsung mendistribusikan alamat ke organisasi atau individu, melainkan mendelegasikannya ke lima lembaga regional.

- IANA mendistribusikan blok-blok alamat besar kepada **lima Regional Internet Registries (RIR)** yang masing-masing bertanggung jawab atas wilayah geografis tertentu di dunia.

**Lima RIR beserta wilayah cakupannya:**

| RIR | Wilayah Cakupan |
|---|---|
| **ARIN** | Amerika Utara |
| **LACNIC** | Amerika Latin dan Karibia |
| **RIPE NCC** | Eropa, Timur Tengah, dan Asia Tengah |
| **AfriNIC** | Afrika |
| **APNIC** | Asia Pasifik dan Australia |

- **Alur distribusi alamat IP:** IANA → RIR → ISP besar → ISP kecil → Organisasi/pengguna akhir. RIR bertanggung jawab mengalokasikan blok alamat kepada ISP di wilayahnya, kemudian ISP menyediakan blok alamat yang lebih kecil kepada ISP yang lebih kecil lagi atau langsung kepada organisasi dan pelanggan.

> **Analogi:** Sistem ini seperti pembagian wilayah distribusi dari pusat ke distributor regional. IANA adalah pabrik pusatnya, RIR adalah distributor regional di setiap benua, ISP besar adalah toko grosir, dan ISP kecil serta organisasi adalah toko eceran yang langsung melayani pelanggan akhir.

---

## Ringkasan Jenis-Jenis Alamat IPv4

| Jenis Alamat | Rentang | Fungsi Utama | Bisa Dirutekan ke Internet? |
|---|---|---|---|
| **Publik** | Berbagai rentang (non-privat) | Komunikasi global di internet | Ya |
| **Privat (RFC 1918)** | 10.x, 172.16-31.x, 192.168.x | Jaringan internal organisasi | Tidak (butuh NAT) |
| **Loopback** | 127.0.0.1 | Pengujian TCP/IP lokal | Tidak |
| **Link-Local (APIPA)** | 169.254.x.x | Auto-konfigurasi saat DHCP gagal | Tidak |
| **Multicast (Class D)** | 224.0.0.0 – 239.x.x.x | Komunikasi grup multicast | Terbatas |
| **Reserved (Class E)** | 240.0.0.0 – 255.x.x.x | Eksperimental/dicadangkan | Tidak |

---

Siap lanjut ke **Bab 11.4**? Instruksikan kapan kamu siap!

---

# Ringkasan Sub-Bab 11.4: Network Segmentation

---

## Pengantar

Ketika sebuah jaringan tumbuh semakin besar, muncul berbagai masalah yang berkaitan dengan efisiensi dan performa komunikasi. Sub-bab ini membahas konsep **broadcast domain**, masalah yang ditimbulkan oleh jaringan yang terlalu besar, serta bagaimana **subnetting** digunakan sebagai solusi untuk **melakukan segmentasi jaringan** menjadi bagian-bagian yang lebih kecil dan lebih terkelola dengan baik.

---

## A. Broadcast Domains and Segmentation (Domain Broadcast dan Segmentasi)

- **Broadcast domain** adalah sekumpulan perangkat jaringan yang semuanya akan menerima paket broadcast yang dikirimkan oleh salah satu anggotanya. Seluruh perangkat yang berada dalam satu broadcast domain terhubung satu sama lain melalui switch, dan setiap broadcast yang dikirimkan akan diteruskan ke semua perangkat dalam domain tersebut.

- **Banyak protokol jaringan bergantung pada mekanisme broadcast dan multicast** untuk menjalankan fungsinya. Dua contoh paling umum adalah:
    - **ARP (Address Resolution Protocol)** — menggunakan broadcast untuk menemukan alamat MAC (hardware address) dari sebuah perangkat yang diketahui alamat IP-nya. Ketika sebuah host ingin berkomunikasi dengan host lain, ia akan mengirimkan broadcast ARP ke seluruh jaringan untuk menanyakan "siapa yang memiliki alamat IP ini?"
    - **DHCP Discover** — ketika sebuah host baru bergabung ke jaringan dan membutuhkan alamat IP, ia mengirimkan paket broadcast DHCP Discover untuk menemukan server DHCP yang tersedia di jaringan.

- **Peran switch dalam broadcast:** Switch meneruskan paket broadcast **keluar dari semua interface (port) yang dimilikinya**, kecuali interface tempat broadcast tersebut pertama kali diterima. Ini berarti setiap broadcast yang masuk ke switch akan disebarkan ke semua perangkat yang terhubung, tanpa seleksi.

- **Satu-satunya perangkat yang dapat menghentikan penyebaran broadcast adalah router.** Router secara default **tidak meneruskan (forward) paket broadcast** ke jaringan lain. Setiap interface pada router terhubung ke sebuah broadcast domain tersendiri, dan broadcast hanya akan beredar di dalam broadcast domain spesifik tempat ia berasal — tidak akan melewati router ke jaringan lain.

> **Analogi:** Broadcast domain seperti sebuah ruangan yang penuh orang. Kalau seseorang berteriak (broadcast), semua orang di ruangan itu mendengar. Router adalah tembok tebal antar ruangan — teriakan tidak bisa menembus tembok, sehingga orang di ruangan lain tidak terganggu sama sekali.

---

## B. Problems with Large Broadcast Domains (Masalah pada Broadcast Domain yang Besar)

- **Masalah utama broadcast domain yang besar** adalah bahwa semakin banyak perangkat yang berada dalam satu broadcast domain, semakin banyak pula **traffic broadcast yang dihasilkan** secara kolektif. Setiap perangkat yang mengirimkan broadcast akan membebani seluruh perangkat lain di domain tersebut, karena setiap perangkat harus memproses setiap paket broadcast yang diterima — bahkan jika broadcast tersebut tidak relevan bagi perangkat tersebut.

- **Dampak negatif broadcast berlebihan** terhadap jaringan meliputi:
    - Penurunan performa jaringan secara keseluruhan karena bandwidth terpakai oleh traffic broadcast yang tidak produktif.
    - Setiap perangkat harus mengalokasikan sumber daya CPU untuk memproses paket broadcast yang diterima, meskipun paket tersebut tidak ditujukan kepadanya.
    - Semakin besar jaringan, semakin parah dampak yang ditimbulkan — kondisi ini dikenal sebagai **broadcast storm** dalam kasus ekstrem.

- **Contoh konkret masalah:** Sebuah jaringan LAN besar bernama **LAN 1: 172.16.0.0/16** dengan **400 pengguna** semuanya berada dalam satu broadcast domain yang sama. Setiap broadcast yang dikirimkan oleh salah satu dari 400 perangkat tersebut akan diterima dan diproses oleh 399 perangkat lainnya — menyebabkan beban yang sangat berat pada jaringan.

- **Solusi yang diterapkan adalah subnetting** — yaitu proses memperkecil ukuran jaringan dengan cara membaginya menjadi beberapa broadcast domain yang lebih kecil. Dengan subnetting, jaringan `172.16.0.0/16` yang tadinya menampung 400 pengguna dalam satu domain dapat dibagi menjadi dua subnet terpisah: **172.16.0.0/24** (200 pengguna) dan **172.16.1.0/24** (200 pengguna). Broadcast dari subnet pertama tidak akan mengganggu perangkat di subnet kedua, dan sebaliknya.

> **Analogi:** Satu kantor besar dengan 400 karyawan yang semua teriak-terikan (broadcast) tentu sangat kacau dan mengganggu. Solusinya adalah membagi menjadi dua ruangan dengan 200 orang masing-masing, lalu pasang tembok tebal berupa router di tengah. Keributan di satu ruangan tidak akan mengganggu ruangan lain.

---

## C. Reasons for Segmenting Networks (Alasan-Alasan Melakukan Segmentasi Jaringan)

Subnetting dan segmentasi jaringan memberikan **tiga manfaat utama** yang saling melengkapi:

**1. Mengurangi traffic jaringan dan meningkatkan performa**
Dengan membagi jaringan besar menjadi subnet-subnet yang lebih kecil, jumlah perangkat dalam setiap broadcast domain berkurang secara signifikan. Hal ini langsung mengurangi volume traffic broadcast yang harus ditangani oleh setiap perangkat, sehingga bandwidth yang tersedia dapat lebih banyak digunakan untuk traffic data yang produktif dan performa jaringan secara keseluruhan meningkat.

**2. Mengimplementasikan kebijakan keamanan antar subnet**
Subnetting memungkinkan administrator jaringan untuk menerapkan **kebijakan keamanan (security policy) yang berbeda-beda** pada setiap subnet. Misalnya, subnet yang berisi server keuangan dapat diberi pembatasan akses yang ketat sehingga tidak sembarang perangkat dari subnet lain dapat mengaksesnya. Router atau firewall yang berada di antara subnet dapat dikonfigurasi untuk memfilter traffic berdasarkan kebijakan keamanan yang telah ditetapkan.

**3. Mengurangi dampak broadcast yang tidak normal**
Ketika terjadi masalah jaringan seperti broadcast storm atau kesalahan konfigurasi yang menghasilkan broadcast berlebihan, dampaknya hanya terbatas pada subnet tempat masalah tersebut terjadi. Perangkat di subnet lain tidak akan terdampak karena router memblok penyebaran broadcast ke luar subnet.

---

## D. Dasar-Dasar Pembagian Subnet dalam Praktik

Dalam praktiknya, subnet dapat dibentuk berdasarkan berbagai pertimbangan organisasional. Terdapat **tiga pendekatan utama** yang umum digunakan:

### 1. Berdasarkan Lokasi (Location)
Jaringan disegmentasi berdasarkan **lokasi fisik** perangkat. Setiap lantai gedung, setiap ruangan, atau setiap cabang kantor mendapatkan subnet tersendiri. Pendekatan ini memudahkan manajemen jaringan secara fisik karena setiap subnet berkorespondensi langsung dengan lokasi geografis yang spesifik.

**Contoh:** Sebuah gedung bertingkat menggunakan subnet berbeda untuk setiap lantainya:
- LAN 1: `10.0.1.0/24` — Lantai 1 (First floor)
- LAN 2: `10.0.2.0/24` — Lantai 2 (Second floor)
- LAN 3: `10.0.3.0/24` — Lantai 3 (Third floor)
- LAN 4: `10.0.4.0/24` — Lantai 4 (Fourth floor)
- LAN 5: `10.0.5.0/24` — Lantai 5 (Fifth floor)

Semua subnet tersebut terhubung ke satu router yang tersambung ke internet.

### 2. Berdasarkan Grup atau Fungsi (Group or Function)
Jaringan disegmentasi berdasarkan **departemen atau fungsi** dari pengguna maupun perangkat. Pendekatan ini sangat berguna untuk menerapkan kebijakan keamanan yang berbeda-beda per departemen, serta memudahkan pengelolaan akses antar divisi dalam sebuah organisasi.

**Contoh:** Sebuah institusi membagi jaringannya berdasarkan departemen:
- LAN 1: `10.0.1.0/24` — Administration
- LAN 2: `10.0.2.0/24` — Students
- LAN 3: `10.0.3.0/24` — Human Resources
- LAN 4: `10.0.4.0/24` — Accounting

### 3. Berdasarkan Jenis Perangkat (Device Type)
Jaringan disegmentasi berdasarkan **jenis atau kategori perangkat** yang ada. Pendekatan ini memungkinkan pengaturan prioritas traffic, kebijakan keamanan, dan manajemen bandwidth yang disesuaikan dengan karakteristik masing-masing jenis perangkat.

**Contoh:** Jaringan dibagi berdasarkan kategori perangkat:
- LAN 1: `10.0.1.0/24` — All Hosts (semua komputer pengguna)
- LAN 2: `10.0.2.0/24` — All Printers (semua printer jaringan)
- LAN 3: `10.0.3.0/24` — All Servers (semua server)

> **Analogi:** Segmentasi jaringan seperti tata letak di sebuah mall besar. Lantai 1 khusus fashion, lantai 2 khusus makanan, lantai 3 khusus elektronik. Kalau ada keributan di lantai makanan, pengunjung lantai elektronik tidak terganggu. Setiap lantai punya "aturan" sendiri, dan satpam (router) mengatur perpindahan antar lantai.

---

## Ringkasan Konsep Kunci Sub-Bab 11.4

| Konsep | Penjelasan Singkat |
|---|---|
| **Broadcast Domain** | Kumpulan perangkat yang menerima broadcast yang sama; dibatasi oleh router |
| **Switch** | Meneruskan broadcast ke semua port kecuali port asal |
| **Router** | Satu-satunya perangkat yang menghentikan penyebaran broadcast |
| **Masalah domain besar** | Traffic broadcast berlebihan menurunkan performa jaringan |
| **Subnetting** | Solusi membagi jaringan besar menjadi broadcast domain yang lebih kecil |
| **Manfaat segmentasi** | Performa meningkat, keamanan lebih baik, dampak broadcast terisolasi |
| **Dasar pembagian subnet** | Berdasarkan lokasi, fungsi/grup, atau jenis perangkat |

---

Siap lanjut ke **Bab 11.5**? Instruksikan kapan kamu siap!

---

# Ringkasan Sub-Bab 11.5: Subnet an IPv4 Network

---

## Pengantar

Sub-bab ini membahas secara teknis bagaimana proses **subnetting** dilakukan pada sebuah jaringan IPv4. Subnetting adalah proses meminjam bit dari bagian host untuk dijadikan bit jaringan, sehingga satu jaringan besar dapat dipecah menjadi beberapa subnet yang lebih kecil. Terdapat dua pendekatan utama: subnetting **pada batas oktet (octet boundary)** yang lebih sederhana, dan subnetting **di dalam satu oktet (within an octet boundary)** yang lebih fleksibel.

---

## A. Subnet on an Octet Boundary (Subnetting pada Batas Oktet)

- **Subnetting paling mudah dan paling sederhana** dilakukan pada batas oktet, yaitu menggunakan prefix length **/8**, **/16**, dan **/24**. Pada titik-titik ini, batas antara bagian jaringan dan bagian host jatuh tepat di antara dua oktet, sehingga tidak perlu melakukan perhitungan biner yang rumit.

- **Prinsip dasar yang harus dipahami:** Semakin panjang prefix length yang digunakan (semakin banyak bit yang dialokasikan untuk jaringan), maka semakin sedikit bit yang tersisa untuk host, sehingga semakin sedikit pula jumlah host yang dapat ditampung dalam setiap subnet.

**Tabel dasar prefix length pada batas oktet:**

| Prefix Length | Subnet Mask | Subnet Mask dalam Biner | Jumlah Host |
|---|---|---|---|
| **/8** | 255.0.0.0 | 11111111.00000000.00000000.00000000 | 16.777.214 |
| **/16** | 255.255.0.0 | 11111111.11111111.00000000.00000000 | 65.534 |
| **/24** | 255.255.255.0 | 11111111.11111111.11111111.00000000 | 254 |

> **Catatan penting:** Rumus menghitung jumlah host yang dapat digunakan dalam sebuah subnet adalah **2ⁿ - 2**, di mana **n** adalah jumlah bit host yang tersedia. Dikurangi 2 karena satu alamat digunakan sebagai network address dan satu lagi sebagai broadcast address.

> **Analogi:** Semakin banyak digit kode pos yang kamu tetapkan untuk nama jalan (network portion), semakin sedikit nomor rumah (host portion) yang bisa kamu buat di jalan tersebut.

---

## B. Subnet on an Octet Boundary — Contoh Praktis dengan 10.0.0.0/8

Berikut adalah contoh nyata bagaimana jaringan `10.0.0.0/8` dapat di-subnet menggunakan dua pendekatan berbeda pada batas oktet:

### Pendekatan 1 — Subnet dengan /16 (256 Possible Subnets, 65.534 hosts per subnet)

Dengan menggunakan prefix `/16` pada jaringan `10.0.0.0/8`, diperoleh **256 subnet** yang masing-masing dapat menampung **65.534 host**.

| Subnet Address | Host Range | Broadcast |
|---|---|---|
| 10.0.0.0/16 | 10.0.0.1 – 10.0.255.254 | 10.0.255.255 |
| 10.1.0.0/16 | 10.1.0.1 – 10.1.255.254 | 10.1.255.255 |
| 10.2.0.0/16 | 10.2.0.1 – 10.2.255.254 | 10.2.255.255 |
| 10.3.0.0/16 | 10.3.0.1 – 10.3.255.254 | 10.3.255.255 |
| … | … | … |
| 10.255.0.0/16 | 10.255.0.1 – 10.255.255.254 | 10.255.255.255 |

### Pendekatan 2 — Subnet dengan /24 (65.536 Possible Subnets, 254 hosts per subnet)

Dengan menggunakan prefix `/24` pada jaringan `10.0.0.0/8`, diperoleh **65.536 subnet** yang masing-masing dapat menampung **254 host**.

| Subnet Address | Host Range | Broadcast |
|---|---|---|
| 10.0.0.0/24 | 10.0.0.1 – 10.0.0.254 | 10.0.0.255 |
| 10.0.1.0/24 | 10.0.1.1 – 10.0.1.254 | 10.0.1.255 |
| 10.0.2.0/24 | 10.0.2.1 – 10.0.2.254 | 10.0.2.255 |
| … | … | … |
| 10.0.255.0/24 | 10.0.255.1 – 10.0.255.254 | 10.0.255.255 |
| 10.1.0.0/24 | 10.1.0.1 – 10.1.0.254 | 10.1.0.255 |
| … | … | … |
| 10.255.255.0/24 | 10.255.255.1 – 10.255.255.254 | 10.255.255.255 |

---

## C. Subnet within an Octet Boundary (Subnetting di Dalam Satu Oktet)

- Ketika kebutuhan jaringan tidak pas dengan batas oktet, subnetting dapat dilakukan **di dalam oktet keempat** dari sebuah jaringan `/24`. Pendekatan ini memberikan fleksibilitas lebih besar dalam membagi jaringan sesuai kebutuhan yang lebih spesifik.

- **Prinsip peminjaman bit:** Setiap bit yang "dipinjam" dari bagian host untuk dijadikan bagian jaringan akan **melipatgandakan jumlah subnet** yang tersedia, tetapi sekaligus **mengurangi jumlah host** yang dapat ditampung per subnet. Hubungan ini bersifat trade-off — tidak bisa mendapatkan banyak subnet sekaligus banyak host dalam satu skema subnetting yang sama.

- **Rumus kunci:**
    - Jumlah subnet = **2ⁿ** (di mana n = jumlah bit yang dipinjam dari bagian host)
    - Jumlah host per subnet = **2ʰ - 2** (di mana h = jumlah bit host yang tersisa)

**Tabel lengkap subnetting di dalam oktet keempat untuk jaringan /24:**

| Prefix Length | Subnet Mask | Subnet Mask dalam Biner | Jumlah Subnet | Jumlah Host/Subnet |
|---|---|---|---|---|
| **/25** | 255.255.255.128 | 11111111.11111111.11111111.1**0000000** | 2 | 126 |
| **/26** | 255.255.255.192 | 11111111.11111111.11111111.11**000000** | 4 | 62 |
| **/27** | 255.255.255.224 | 11111111.11111111.11111111.111**00000** | 8 | 30 |
| **/28** | 255.255.255.240 | 11111111.11111111.11111111.1111**0000** | 16 | 14 |
| **/29** | 255.255.255.248 | 11111111.11111111.11111111.11111**000** | 32 | 6 |
| **/30** | 255.255.255.252 | 11111111.11111111.11111111.111111**00** | 64 | 2 |

**Penjelasan per prefix:**

- **/25** — Meminjam **1 bit** dari host, menghasilkan **2 subnet** dengan masing-masing **126 host** yang dapat digunakan. Cocok untuk membagi satu jaringan /24 menjadi dua bagian besar yang hampir sama.

- **/26** — Meminjam **2 bit** dari host, menghasilkan **4 subnet** dengan masing-masing **62 host** yang dapat digunakan. Cocok untuk jaringan yang membutuhkan empat segmen dengan jumlah host sedang.

- **/27** — Meminjam **3 bit** dari host, menghasilkan **8 subnet** dengan masing-masing **30 host** yang dapat digunakan. Sangat umum digunakan untuk segmen LAN berukuran kecil hingga sedang.

- **/28** — Meminjam **4 bit** dari host, menghasilkan **16 subnet** dengan masing-masing **14 host** yang dapat digunakan. Cocok untuk jaringan dengan segmen-segmen kecil.

- **/29** — Meminjam **5 bit** dari host, menghasilkan **32 subnet** dengan masing-masing **6 host** yang dapat digunakan. Cocok untuk segmen yang hanya membutuhkan sedikit perangkat.

- **/30** — Meminjam **6 bit** dari host, menghasilkan **64 subnet** dengan masing-masing hanya **2 host** yang dapat digunakan. Prefix ini paling sering digunakan untuk **koneksi point-to-point antar router** (WAN link) karena koneksi semacam ini hanya membutuhkan tepat dua alamat host.

> **Analogi:** Subnetting di dalam satu oktet seperti memotong-motong sebuah pizza /24. Satu pizza bisa dipotong jadi 2 bagian besar (/25), 4 bagian (/26), 8 bagian (/27), 16 bagian (/28), 32 bagian (/29), atau 64 bagian kecil (/30). Semakin banyak potongannya, semakin kecil tiap potongannya — tidak bisa mendapat banyak potongan besar sekaligus.

---

## D. Cara Membaca dan Menghitung Subnet Secara Praktis

Untuk keperluan ujian, berikut adalah langkah-langkah praktis dalam menghitung subnet:

**Langkah 1 — Tentukan jumlah bit yang dipinjam**
Kurangkan prefix length subnet dari prefix length jaringan asal.
Contoh: Jaringan /24 di-subnet menjadi /27 → bit yang dipinjam = 27 - 24 = **3 bit**

**Langkah 2 — Hitung jumlah subnet**
Gunakan rumus **2ⁿ** di mana n = jumlah bit yang dipinjam.
Contoh: 2³ = **8 subnet**

**Langkah 3 — Hitung jumlah host per subnet**
Hitung sisa bit host = 32 - prefix length baru.
Contoh: 32 - 27 = 5 bit host → 2⁵ - 2 = **30 host per subnet**

**Langkah 4 — Hitung block size (interval antar subnet)**
Block size = **256 - nilai oktet terakhir subnet mask**.
Contoh: Subnet mask /27 = 255.255.255.224 → block size = 256 - 224 = **32**

**Langkah 5 — Tentukan subnet address, host range, dan broadcast**
Dengan block size 32 pada jaringan 192.168.1.0/27:

| Subnet | Network Address | Host Range | Broadcast |
|---|---|---|---|
| Subnet 1 | 192.168.1.0/27 | 192.168.1.1 – 192.168.1.30 | 192.168.1.31 |
| Subnet 2 | 192.168.1.32/27 | 192.168.1.33 – 192.168.1.62 | 192.168.1.63 |
| Subnet 3 | 192.168.1.64/27 | 192.168.1.65 – 192.168.1.94 | 192.168.1.95 |
| Subnet 4 | 192.168.1.96/27 | 192.168.1.97 – 192.168.1.126 | 192.168.1.127 |
| … | … | … | … |

---

## Ringkasan Konsep Kunci Sub-Bab 11.5

| Konsep | Penjelasan Singkat |
|---|---|
| **Octet boundary** | Subnetting paling mudah dilakukan pada /8, /16, /24 |
| **Bit yang dipinjam** | Setiap bit yang dipinjam dari host melipatgandakan jumlah subnet |
| **Trade-off subnetting** | Lebih banyak subnet = lebih sedikit host per subnet |
| **Rumus subnet** | Jumlah subnet = 2ⁿ; Jumlah host = 2ʰ - 2 |
| **Block size** | 256 dikurangi nilai oktet terakhir subnet mask |
| **/30** | Prefix paling umum untuk WAN point-to-point (hanya 2 host) |
| **Prefix /25 s/d /30** | Enam pilihan subnetting di dalam oktet keempat jaringan /24 |

---

Siap lanjut ke **Bab 11.6**? Instruksikan kapan kamu siap!

---

# Ringkasan Sub-Bab 11.6: Subnet a Slash 16 and a Slash 8 Prefix

---

## Pengantar

Sub-bab ini memperluas konsep subnetting ke jaringan yang lebih besar, yaitu jaringan dengan prefix **/16** dan **/8**. Jika pada sub-bab sebelumnya subnetting dilakukan di dalam oktet keempat pada jaringan /24, maka pada sub-bab ini proses subnetting **melampaui beberapa oktet** sekaligus karena tersedianya lebih banyak bit host yang dapat dipinjam. Sub-bab ini juga menyajikan dua skenario praktis: membuat **100 subnet dari jaringan /16** dan membuat **1000 subnet dari jaringan /8**.

---

## A. Create Subnets with a Slash 16 Prefix

- Jaringan dengan prefix **/16** memiliki **16 bit untuk bagian jaringan** dan **16 bit untuk bagian host**, sehingga tersedia lebih banyak fleksibilitas dalam melakukan subnetting dibandingkan jaringan /24.

- Karena tersedia 16 bit host, bit-bit tersebut dapat dipinjam mulai dari oktet ketiga hingga oktet keempat untuk membentuk subnet. Namun **2 bit terakhir tidak dapat dipinjam** karena setiap subnet membutuhkan minimal 2 bit host untuk network address dan broadcast address.

- **Semakin besar prefix length yang dipilih** (semakin banyak bit yang dipinjam), semakin banyak subnet yang terbentuk, tetapi semakin sedikit host yang dapat ditampung per subnet.

**Tabel lengkap semua skenario subnetting pada jaringan /16:**

| Prefix Length | Subnet Mask | Jumlah Subnet | Jumlah Host/Subnet |
|---|---|---|---|
| **/17** | 255.255.128.0 | 2 | 32.766 |
| **/18** | 255.255.192.0 | 4 | 16.382 |
| **/19** | 255.255.224.0 | 8 | 8.190 |
| **/20** | 255.255.240.0 | 16 | 4.094 |
| **/21** | 255.255.248.0 | 32 | 2.046 |
| **/22** | 255.255.252.0 | 64 | 1.022 |
| **/23** | 255.255.254.0 | 128 | 510 |
| **/24** | 255.255.255.0 | 256 | 254 |
| **/25** | 255.255.255.128 | 512 | 126 |
| **/26** | 255.255.255.192 | 1.024 | 62 |
| **/27** | 255.255.255.224 | 2.048 | 30 |
| **/28** | 255.255.255.240 | 4.096 | 14 |
| **/29** | 255.255.255.248 | 8.192 | 6 |
| **/30** | 255.255.255.252 | 16.384 | 2 |

**Penjelasan pola biner per prefix pada jaringan /16:**

- **/17** — Meminjam **1 bit** dari oktet ketiga: `nnnnnnnn.nnnnnnnn.n`**`hhhhhhh.hhhhhhhh`** → 2 subnet, 32.766 host
- **/18** — Meminjam **2 bit** dari oktet ketiga: `nnnnnnnn.nnnnnnnn.nn`**`hhhhhh.hhhhhhhh`** → 4 subnet, 16.382 host
- **/19** — Meminjam **3 bit** dari oktet ketiga: `nnnnnnnn.nnnnnnnn.nnn`**`hhhhh.hhhhhhhh`** → 8 subnet, 8.190 host
- **/20** — Meminjam **4 bit** dari oktet ketiga: `nnnnnnnn.nnnnnnnn.nnnn`**`hhhh.hhhhhhhh`** → 16 subnet, 4.094 host
- **/24** — Meminjam **8 bit penuh** dari oktet ketiga: `nnnnnnnn.nnnnnnnn.nnnnnnnn.`**`hhhhhhhh`** → 256 subnet, 254 host
- **/30** — Meminjam **8 bit oktet ketiga + 6 bit oktet keempat**: → 16.384 subnet, hanya 2 host

> **Analogi:** Bayangkan kamu punya sebidang tanah besar /16. Kamu bisa memotongnya jadi 2 kavling besar, atau 4, atau 8, dan seterusnya sampai 16.384 kavling sangat kecil. Semakin banyak kavling yang kamu buat, semakin sempit tiap kavlingnya.

---

## B. Create 100 Subnets with a Slash 16 Prefix — Skenario Praktis

- **Skenario:** Sebuah perusahaan besar membutuhkan **setidaknya 100 subnet** untuk jaringan internalnya, dan telah memilih alamat privat **172.16.0.0/16** sebagai alamat jaringan internalnya.

- Pada jaringan `172.16.0.0/16`, struktur bitnya adalah sebagai berikut:
    - Bagian jaringan (16 bit): `nnnnnnnn.nnnnnnnn`
    - Bagian host (16 bit): `hhhhhhhh.hhhhhhhh`

- Karena tersedia **16 bit host**, terdapat hingga **14 bit yang dapat dipinjam** untuk keperluan subnetting (2 bit terakhir tidak dapat dipinjam karena setiap subnet membutuhkan minimal 2 host address).

**Rincian peminjaman bit dan jumlah subnet yang dihasilkan:**

| Bit yang Dipinjam | Rumus | Jumlah Subnet |
|---|---|---|
| 1 bit | 2¹ | 2 subnet |
| 2 bit | 2² | 4 subnet |
| 3 bit | 2³ | 8 subnet |
| 4 bit | 2⁴ | 16 subnet |
| 5 bit | 2⁵ | 32 subnet |
| 6 bit | 2⁶ | 64 subnet |
| **7 bit** | **2⁷** | **128 subnet ✓** |
| 8 bit | 2⁸ | 256 subnet |
| 9 bit | 2⁹ | 512 subnet |
| 10 bit | 2¹⁰ | 1.024 subnet |
| 11 bit | 2¹¹ | 2.048 subnet |
| 12 bit | 2¹² | 4.096 subnet |
| 13 bit | 2¹³ | 8.192 subnet |
| 14 bit | 2¹⁴ | 16.384 subnet |

- **Kesimpulan perhitungan:** Untuk memenuhi kebutuhan minimal 100 subnet, diperlukan peminjaman **7 bit** karena 2⁷ = **128 subnet** — ini adalah jumlah subnet terkecil yang masih memenuhi atau melampaui kebutuhan 100 subnet. Meminjam hanya 6 bit menghasilkan 64 subnet yang masih kurang dari 100.

- **Prefix yang digunakan:** Jaringan asal /16 + 7 bit dipinjam = prefix **/23**
    - Subnet mask yang digunakan: **255.255.254.0**
    - Jumlah host per subnet: 2⁹ - 2 = **510 host per subnet**

- **Struktur bit hasil subnetting:**
    - `nnnnnnnn.nnnnnnnn.nnnnnnn`**`h.hhhhhhhh`**
    - 23 bit jaringan, 9 bit host

> **Analogi:** Meminjam bit untuk subnet seperti memecah uang besar. Kamu punya uang Rp 100.000 (jaringan /16). Setiap kali kamu "meminjam" satu tingkat pecahan, jumlah lembarnya berlipat dua tapi nilai tiap lembarnya jadi setengah. Mau 100 lembar? Kamu perlu pecah sampai 7 kali, menghasilkan 128 lembar.

---

## C. Create 1000 Subnets with a Slash 8 Prefix — Skenario Praktis

- **Skenario:** Sebuah ISP kecil membutuhkan **setidaknya 1000 subnet** untuk para kliennya, menggunakan alamat jaringan **10.0.0.0/8**.

- Pada jaringan `10.0.0.0/8`, struktur bitnya adalah sebagai berikut:
    - Bagian jaringan (8 bit): `nnnnnnnn`
    - Bagian host (24 bit): `hhhhhhhh.hhhhhhhh.hhhhhhhh`

- Karena tersedia **24 bit host**, terdapat hingga **22 bit yang dapat dipinjam** untuk keperluan subnetting (2 bit terakhir tidak dapat dipinjam).

**Rincian peminjaman bit dan jumlah subnet yang dihasilkan:**

| Bit yang Dipinjam | Rumus | Jumlah Subnet |
|---|---|---|
| 1 bit | 2¹ | 2 subnet |
| 2 bit | 2² | 4 subnet |
| 3 bit | 2³ | 8 subnet |
| 4 bit | 2⁴ | 16 subnet |
| 5 bit | 2⁵ | 32 subnet |
| 6 bit | 2⁶ | 64 subnet |
| 7 bit | 2⁷ | 128 subnet |
| 8 bit | 2⁸ | 256 subnet |
| 9 bit | 2⁹ | 512 subnet |
| **10 bit** | **2¹⁰** | **1.024 subnet ✓** |
| … | … | … |
| 22 bit | 2²² | 4.194.304 subnet |

- **Kesimpulan perhitungan:** Untuk memenuhi kebutuhan minimal 1000 subnet, diperlukan peminjaman **10 bit** karena 2¹⁰ = **1.024 subnet** — ini adalah jumlah subnet terkecil yang masih memenuhi atau melampaui kebutuhan 1000 subnet. Meminjam hanya 9 bit menghasilkan 512 subnet yang masih kurang dari 1000.

- **Prefix yang digunakan:** Jaringan asal /8 + 10 bit dipinjam = prefix **/18**
    - Subnet mask yang digunakan: **255.255.192.0**
    - Jumlah host per subnet: 2¹⁴ - 2 = **16.382 host per subnet**

- **Struktur bit hasil subnetting:**
    - `nnnnnnnn.nnnnnnnn.nn`**`hhhhhh.hhhhhhhh`**
    - 18 bit jaringan, 14 bit host

> **Analogi:** Jaringan /8 seperti tanah yang jauh lebih luas lagi. Kamu punya 24 bit host yang bisa dipinjam — bagaikan lahan ribuan hektare. ISP yang butuh 1000 kavling cukup meminjam 10 bit, menghasilkan 1024 kavling sekaligus, dengan tiap kavling masih sangat luas (16.382 host).

---

## D. Perbandingan Ketiga Skenario Subnetting

| Aspek | Jaringan /24 | Jaringan /16 (100 subnet) | Jaringan /8 (1000 subnet) |
|---|---|---|---|
| **Jaringan asal** | 192.168.x.0/24 | 172.16.0.0/16 | 10.0.0.0/8 |
| **Bit host tersedia** | 8 bit | 16 bit | 24 bit |
| **Bit dapat dipinjam** | Maks. 6 bit | Maks. 14 bit | Maks. 22 bit |
| **Target subnet** | Bervariasi | Min. 100 subnet | Min. 1000 subnet |
| **Bit yang dipinjam** | Sesuai kebutuhan | 7 bit | 10 bit |
| **Prefix hasil** | /25 s/d /30 | /23 | /18 |
| **Subnet terbentuk** | 2 s/d 64 | 128 subnet | 1.024 subnet |
| **Host per subnet** | 126 s/d 2 | 510 host | 16.382 host |

---

## E. Cara Praktis Menentukan Prefix untuk Target Subnet Tertentu

Untuk soal ujian yang meminta menentukan prefix berdasarkan kebutuhan subnet, gunakan langkah berikut:

**Langkah 1 — Tentukan jumlah subnet yang dibutuhkan**
Identifikasi berapa subnet minimal yang diperlukan dari soal.

**Langkah 2 — Cari pangkat 2 yang memenuhi**
Cari nilai n terkecil sehingga 2ⁿ ≥ jumlah subnet yang dibutuhkan.

**Langkah 3 — Hitung prefix baru**
Tambahkan n (bit yang dipinjam) ke prefix jaringan asal.
Contoh: Jaringan /16, butuh 100 subnet → n = 7 → prefix baru = 16 + 7 = **/23**

**Langkah 4 — Hitung host per subnet**
Sisa bit host = 32 - prefix baru → jumlah host = 2^(sisa bit) - 2
Contoh: Prefix /23 → sisa bit = 32 - 23 = 9 → host = 2⁹ - 2 = **510 host**

**Langkah 5 — Verifikasi**
Pastikan jumlah subnet yang terbentuk ≥ kebutuhan dan jumlah host per subnet mencukupi.

---

## Ringkasan Konsep Kunci Sub-Bab 11.6

| Konsep | Penjelasan Singkat |
|---|---|
| **Jaringan /16** | Tersedia 16 bit host; dapat dipinjam maksimal 14 bit untuk subnetting |
| **Jaringan /8** | Tersedia 24 bit host; dapat dipinjam maksimal 22 bit untuk subnetting |
| **Aturan 2 bit terakhir** | 2 bit host terakhir tidak boleh dipinjam (butuh untuk network & broadcast) |
| **Rumus subnet** | 2ⁿ di mana n = jumlah bit yang dipinjam |
| **Rumus host** | 2ʰ - 2 di mana h = sisa bit host setelah peminjaman |
| **100 subnet dari /16** | Pinjam 7 bit → prefix /23 → 128 subnet, 510 host/subnet |
| **1000 subnet dari /8** | Pinjam 10 bit → prefix /18 → 1024 subnet, 16.382 host/subnet |
| **Prinsip umum** | Semakin besar prefix asal, semakin banyak bit yang bisa dipinjam |

---

Siap lanjut ke **Bab 11.7**? Instruksikan kapan kamu siap!

---

# Ringkasan Sub-Bab 11.7: Subnet to Meet Requirements

---

## Pengantar

Sub-bab ini membahas bagaimana subnetting diterapkan dalam konteks **kebutuhan nyata sebuah organisasi atau perusahaan**. Tidak cukup hanya memahami cara menghitung subnet secara matematis — seorang administrator jaringan juga harus mampu **merancang skema subnetting yang efisien** dengan mempertimbangkan keseimbangan antara jumlah subnet yang dibutuhkan, jumlah host per subnet, serta pembagian antara ruang alamat privat dan publik. Sub-bab ini juga menyajikan sebuah **contoh kasus nyata** subnetting yang efisien pada jaringan publik.

---

## A. Subnet Private versus Public IPv4 Address Space

- Dalam sebuah jaringan enterprise (perusahaan besar), terdapat dua area jaringan yang memiliki kebutuhan pengalamatan yang **sangat berbeda** dan harus direncanakan secara terpisah:

### 1. Intranet (Jaringan Internal)
- **Intranet** adalah jaringan internal perusahaan yang hanya dapat diakses oleh karyawan dan perangkat internal organisasi tersebut.
- Intranet menggunakan **alamat IPv4 privat** (sesuai RFC 1918) untuk semua perangkat internalnya, yaitu dari rentang `10.0.0.0/8`, `172.16.0.0/12`, atau `192.168.0.0/16`.
- Sebuah perusahaan besar dapat menggunakan **`10.0.0.0/8`** sebagai ruang alamat intranet dan melakukan subnetting pada batas `/16` atau `/24` sesuai kebutuhan struktur organisasinya.
- Karena menggunakan alamat privat, perangkat-perangkat di intranet **tidak dapat diakses langsung dari internet** dan harus melalui mekanisme NAT jika perlu berkomunikasi ke luar.

### 2. DMZ (Demilitarized Zone)
- **DMZ** adalah area jaringan khusus yang berisi server-server milik perusahaan yang **harus dapat diakses dari internet publik**, seperti web server, mail server, DNS server, dan sebagainya.
- Perangkat-perangkat di DMZ **harus dikonfigurasi dengan alamat IPv4 publik** agar dapat ditemukan dan diakses oleh pengguna dari luar jaringan perusahaan.
- DMZ biasanya ditempatkan di antara jaringan internal (intranet) dan internet, dilindungi oleh firewall dari kedua sisi untuk memastikan keamanan.

> **Analogi:** Jaringan enterprise seperti sebuah kantor besar. Intranet adalah ruangan-ruangan kerja di dalam gedung yang menggunakan sistem nomor ruangan internal (alamat privat) — orang luar tidak perlu tahu nomor ruanganmu. DMZ adalah lobi dan resepsionis yang menghadap ke jalan umum — mereka harus punya alamat resmi yang bisa ditemukan oleh siapapun dari luar (alamat publik).

---

## B. Minimize Unused Host IPv4 Addresses and Maximize Subnets

- Dalam merencanakan skema subnetting, terdapat **dua pertimbangan utama yang saling bertolak belakang (trade-off)** yang harus diseimbangkan oleh administrator jaringan:

    **1. Jumlah alamat host yang dibutuhkan untuk setiap subnet** — semakin banyak host yang harus ditampung per subnet, semakin besar subnet yang dibutuhkan, yang berarti semakin sedikit subnet yang bisa dibuat.

    **2. Jumlah subnet individual yang dibutuhkan** — semakin banyak subnet yang diperlukan, semakin kecil ukuran setiap subnet, yang berarti semakin sedikit host yang bisa ditampung per subnet.

- **Tujuan utama perencanaan subnet yang baik** adalah meminimalkan alamat host yang tidak terpakai (wasted addresses) dalam setiap subnet sekaligus memaksimalkan jumlah subnet yang dapat dibentuk — sehingga penggunaan ruang alamat menjadi seefisien mungkin.

**Tabel trade-off subnetting untuk jaringan /24:**

| Prefix Length | Subnet Mask | Jumlah Subnet | Jumlah Host/Subnet |
|---|---|---|---|
| **/25** | 255.255.255.128 | 2 | 126 |
| **/26** | 255.255.255.192 | 4 | 62 |
| **/27** | 255.255.255.224 | 8 | 30 |
| **/28** | 255.255.255.240 | 16 | 14 |
| **/29** | 255.255.255.248 | 32 | 6 |
| **/30** | 255.255.255.252 | 64 | 2 |

- **Prinsip penting dalam memilih prefix:** Selalu pilih prefix yang menghasilkan jumlah subnet dan jumlah host per subnet yang **paling mendekati kebutuhan aktual** — tidak terlalu besar (membuang alamat) dan tidak terlalu kecil (tidak cukup menampung host).

> **Analogi:** Merencanakan subnet seperti membagi kamar hotel. Kamu bisa bikin 2 suite mewah besar, atau 64 kamar standar kecil-kecil. Pilih sesuai kebutuhan: butuh banyak ruang per kamar atau butuh banyak kamar? Jangan pesan suite besar kalau hanya butuh tempat tidur untuk satu malam.

---

## C. Example: Efficient IPv4 Subnetting — Studi Kasus Nyata

Berikut adalah contoh kasus nyata perancangan subnetting yang efisien dalam sebuah perusahaan:

### Kondisi Awal dan Kebutuhan

- **Alokasi alamat dari ISP:** Kantor pusat perusahaan mendapatkan alokasi alamat jaringan **publik `172.16.0.0/22`** dari ISP-nya.

- **Kapasitas total alamat yang tersedia:**
    - Jaringan /22 memiliki **10 bit host** (32 - 22 = 10 bit)
    - Total host yang tersedia: 2¹⁰ - 2 = **1.022 alamat host**

- **Struktur biner alamat `172.16.0.0/22`:**
    - Network portion: `10101100.00010000.000000` (22 bit)
    - Host portion: `00.00000000` (10 bit)

- **Kebutuhan organisasi:**
    - Terdapat **5 lokasi** (1 kantor pusat + 4 cabang)
    - Terdapat **5 koneksi internet** (satu per lokasi)
    - Total subnet yang dibutuhkan: **10 subnet** (5 untuk LAN di tiap lokasi + 5 untuk koneksi antar router/WAN link)
    - Subnet terbesar membutuhkan **40 alamat host** (di kantor pusat)

### Proses Pemilihan Prefix

- Dari kebutuhan 10 subnet dengan subnet terbesar 40 host, administrator harus memilih prefix yang:
    - Menghasilkan **minimal 10 subnet**
    - Setiap subnet menampung **minimal 40 host**

- **Prefix /26 dipilih** karena:
    - Bit yang dipinjam dari /22: 26 - 22 = **4 bit** → 2⁴ = **16 subnet** (memenuhi kebutuhan 10 subnet)
    - Sisa bit host: 32 - 26 = **6 bit** → 2⁶ - 2 = **62 host per subnet** (memenuhi kebutuhan 40 host)
    - Subnet mask yang digunakan: **255.255.255.192**

### Hasil Alokasi Subnet

Dari 16 subnet /26 yang tersedia, dialokasikan **10 subnet** sebagai berikut:

**Subnet untuk LAN (Local Area Network) di setiap lokasi:**

| Lokasi | Network Address | Jumlah Host Aktual | Keterangan |
|---|---|---|---|
| Corporate Headquarters | 172.16.0.0/26 | 40 host | Kantor pusat |
| Branch 1 | 172.16.0.192/26 | 25 host | Cabang 1 |
| Branch 2 | 172.16.1.0/26 | 30 host | Cabang 2 |
| Branch 3 | 172.16.1.192/26 | 10 host | Cabang 3 |
| Branch 4 | 172.16.2.64/26 | 15 host | Cabang 4 |

**Subnet untuk koneksi WAN antar router:**

| Koneksi | Network Address | Jumlah Host |
|---|---|---|
| Link Router 1 | 172.16.0.64/26 | 2 host |
| Link Router 2 | 172.16.0.128/26 | 2 host |
| Link Router 3 | 172.16.1.64/26 | 2 host |
| Link Router 4 | 172.16.1.128/26 | 2 host |
| Link Router 5 | 172.16.2.0/26 | 2 host |

Semua lokasi terhubung melalui **ISP di tengah** yang menghubungkan seluruh jaringan perusahaan.

### Analisis Efisiensi

- **Total subnet yang terbentuk:** 16 subnet (hanya 10 yang digunakan, 6 tersisa untuk kebutuhan masa depan)
- **Alamat per subnet:** 62 host yang dapat digunakan
- **Total alamat tersedia:** 1.022 (dari alokasi /22 oleh ISP)
- **Alokasi sudah efisien** karena prefix /26 adalah pilihan terkecil yang masih memenuhi kedua syarat (≥10 subnet dan ≥40 host per subnet)

> **Analogi:** Seperti seorang manajer properti yang mendapat 1.022 unit apartemen dari developer (ISP). Ia harus membagi ke 5 gedung cabang dengan kebutuhan berbeda — gedung terbesar butuh 40 unit. Dengan memilih /26 (62 unit per blok), ia bisa membuat 10 blok sekaligus dan masih ada 6 blok sisa untuk kebutuhan masa depan.

---

## D. Langkah-Langkah Sistematis Merancang Subnet untuk Memenuhi Kebutuhan

Untuk keperluan ujian, berikut adalah langkah sistematis yang dapat digunakan ketika menghadapi soal perancangan subnetting:

**Langkah 1 — Identifikasi kebutuhan**
Tentukan berapa jumlah subnet yang dibutuhkan dan berapa jumlah host maksimum per subnet dari soal.

**Langkah 2 — Tentukan kebutuhan host terlebih dahulu**
Cari nilai h terkecil sehingga 2ʰ - 2 ≥ jumlah host terbesar yang dibutuhkan.
Contoh: Butuh 40 host → h = 6 karena 2⁶ - 2 = 62 ≥ 40 ✓

**Langkah 3 — Hitung prefix baru**
Prefix baru = 32 - h
Contoh: h = 6 → prefix = 32 - 6 = **/26**

**Langkah 4 — Verifikasi jumlah subnet**
Hitung jumlah subnet yang terbentuk: 2^(prefix baru - prefix asal)
Contoh: 2^(26-22) = 2⁴ = **16 subnet** — apakah ≥ kebutuhan 10 subnet? Ya ✓

**Langkah 5 — Alokasikan subnet**
Mulai dari network address pertama, hitung setiap subnet berdasarkan block size.
Block size = 2ʰ = 2⁶ = **64**
- Subnet 1: 172.16.0.0/26 (host: .1 – .62, broadcast: .63)
- Subnet 2: 172.16.0.64/26 (host: .65 – .126, broadcast: .127)
- Subnet 3: 172.16.0.128/26 (host: .129 – .190, broadcast: .191)
- Subnet 4: 172.16.0.192/26 (host: .193 – .254, broadcast: .255)
- Subnet 5: 172.16.1.0/26 → dan seterusnya

---

## Ringkasan Konsep Kunci Sub-Bab 11.7

| Konsep | Penjelasan Singkat |
|---|---|
| **Intranet** | Jaringan internal perusahaan; menggunakan alamat privat RFC 1918 |
| **DMZ** | Area server yang menghadap internet; wajib menggunakan alamat publik |
| **Trade-off subnetting** | Lebih banyak subnet = lebih sedikit host per subnet, dan sebaliknya |
| **Efisiensi subnetting** | Pilih prefix terkecil yang masih memenuhi kebutuhan subnet dan host |
| **Studi kasus 172.16.0.0/22** | 10 subnet dibutuhkan; /26 dipilih → 16 subnet, 62 host/subnet |
| **Block size** | 2^(jumlah bit host) = interval antara satu network address ke berikutnya |
| **Langkah perancangan** | Identifikasi kebutuhan → hitung h → tentukan prefix → verifikasi → alokasikan |

---

Siap lanjut ke **Bab 11.8**? Instruksikan kapan kamu siap!

---

# Ringkasan Sub-Bab 11.8: VLSM (Variable Length Subnet Mask)

---

## Pengantar

Sub-bab ini memperkenalkan konsep **VLSM (Variable Length Subnet Mask)** sebagai solusi atas keterbatasan subnetting tradisional. Jika pada subnetting tradisional semua subnet memiliki ukuran yang sama (menggunakan satu subnet mask yang seragam), maka VLSM memungkinkan penggunaan **subnet mask yang berbeda-beda** untuk setiap subnet dalam satu jaringan yang sama — sehingga setiap subnet dapat dirancang sesuai dengan kebutuhan host yang sesungguhnya tanpa membuang alamat yang tidak diperlukan.

---

## A. Masalah Utama: IPv4 Address Conservation (Penghematan Alamat IPv4)

### Skenario Awal dengan Subnetting Tradisional

- **Topologi yang diberikan:** Terdapat empat gedung yang terhubung secara berurutan melalui empat router (R1, R2, R3, R4), dengan detail kebutuhan host sebagai berikut:
    - Building A: **25 hosts** (terhubung ke R1)
    - Building B: **20 hosts** (terhubung ke R2)
    - Building C: **15 hosts** (terhubung ke R3)
    - Building D: **28 hosts** (terhubung ke R4) ← subnet terbesar

- **Total kebutuhan subnet:** 7 subnet — terdiri dari **4 subnet LAN** (satu per gedung) dan **3 subnet WAN** (untuk link point-to-point antar router: R1–R2, R2–R3, dan R3–R4).

- **Pendekatan subnetting tradisional:** Karena subnet terbesar membutuhkan 28 host, maka dipilih prefix **/27** yang menyediakan 2⁵ - 2 = **30 host per subnet**. Dengan prefix /27, tersedia **8 subnet** yang cukup untuk mendukung 7 subnet yang dibutuhkan.

**Hasil alokasi subnet tradisional dengan /27 pada jaringan 192.168.20.0:**

| Subnet | Network Address | Jumlah Host Aktual | Keterangan |
|---|---|---|---|
| LAN Building A | 192.168.20.0/27 | 25 hosts | Router R1 |
| LAN Building B | 192.168.20.32/27 | 20 hosts | Router R2 |
| LAN Building C | 192.168.20.64/27 | 15 hosts | Router R3 |
| LAN Building D | 192.168.20.96/27 | 28 hosts | Router R4 |
| WAN Link R1–R2 | 192.168.20.128/27 | 2 hosts | Point-to-point |
| WAN Link R2–R3 | 192.168.20.160/27 | 2 hosts | Point-to-point |
| WAN Link R3–R4 | 192.168.20.192/27 | 2 hosts | Point-to-point |

### Masalah Pemborosan Alamat

- **Setiap subnet /27 menyediakan 30 host** yang dapat digunakan. Namun ketiga subnet WAN (link R1–R2, R2–R3, dan R3–R4) hanya membutuhkan **2 host** saja — karena koneksi point-to-point antar dua router hanya memerlukan satu alamat untuk tiap ujung koneksi.

- **Perhitungan pemborosan:**
    - Setiap subnet WAN menyia-nyiakan: 30 - 2 = **28 alamat**
    - Total 3 subnet WAN: 28 × 3 = **84 alamat yang tidak terpakai**

- **Kesimpulan:** Menerapkan skema subnetting tradisional (satu subnet mask seragam) pada skenario ini sangat **tidak efisien dan boros** — sebanyak 84 alamat terbuang sia-sia hanya karena subnet mask tidak dapat disesuaikan dengan kebutuhan aktual setiap subnet.

> **Analogi:** Ini seperti menyewa 7 ruangan konferensi dengan ukuran yang sama (30 kursi tiap ruangan) padahal 3 ruangan di antaranya hanya dipakai oleh 2 orang saja. Kursi-kursi yang kosong itu terbuang sia-sia dan tidak bisa digunakan oleh pihak lain.

---

## B. Solusi: VLSM (Variable Length Subnet Mask)

- **VLSM dikembangkan khusus untuk mengatasi pemborosan alamat** yang terjadi pada subnetting tradisional. VLSM memungkinkan administrator jaringan untuk **melakukan subnet dari sebuah subnet** (subnetting a subnet) — artinya setelah membentuk subnet-subnet besar untuk kebutuhan LAN, salah satu subnet yang tersisa dapat dipecah lagi menjadi subnet-subnet yang lebih kecil untuk kebutuhan WAN atau segmen lain yang hanya membutuhkan sedikit host.

- **Prinsip utama VLSM:** Setiap subnet dapat memiliki **prefix length yang berbeda-beda** sesuai dengan jumlah host yang dibutuhkan — tidak harus seragam seperti pada subnetting tradisional. Inilah yang dimaksud dengan "variable length" — panjang subnet mask yang bervariasi.

- **Aturan paling penting dalam VLSM:** Ketika merancang skema VLSM, **selalu mulai dengan memenuhi kebutuhan host dari subnet terbesar terlebih dahulu**, kemudian lanjutkan subnetting secara bertahap untuk subnet-subnet yang lebih kecil hingga kebutuhan host dari subnet terkecil terpenuhi.

> **Analogi:** Subnetting tradisional seperti memberi setiap karyawan kotak makan siang berukuran sama. Karyawan yang lapar senang, tapi karyawan yang makan sedikit membuang banyak makanan. VLSM hadir sebagai solusi kotak makan yang bisa disesuaikan ukurannya per orang — tidak ada makanan yang terbuang.

---

## C. Perbandingan Visual: Subnetting Tradisional vs VLSM

- **Subnetting tradisional** menghasilkan subnet-subnet yang **semua berukuran sama** — seperti pie yang dipotong dengan irisan yang identik. Meskipun beberapa irisan hanya diisi sedikit, ukurannya tetap sama besar.

- **VLSM** menghasilkan subnet-subnet dengan **ukuran yang bervariasi** — sebagian besar irisan berukuran besar (untuk LAN dengan banyak host), namun satu subnet terakhir yang tersisa dipecah lebih lanjut menjadi beberapa subnet kecil (untuk WAN yang hanya butuh 2 host).

**Ilustrasi perbedaan konseptual:**

```
Subnetting Tradisional (/27):        VLSM:
┌──────────────────────┐             ┌──────────────────────┐
│ Subnet 1 (30 hosts)  │             │ Subnet 1 /27 (30 h)  │
├──────────────────────┤             ├──────────────────────┤
│ Subnet 2 (30 hosts)  │             │ Subnet 2 /27 (30 h)  │
├──────────────────────┤             ├──────────────────────┤
│ Subnet 3 (30 hosts)  │             │ Subnet 3 /27 (30 h)  │
├──────────────────────┤             ├──────────────────────┤
│ Subnet 4 (30 hosts)  │             │ Subnet 4 /27 (30 h)  │
├──────────────────────┤             ├──────┬───┬───┬───┤
│ Subnet 5 (30 hosts)  │             │ /30  │/30│/30│... │
├──────────────────────┤             │ (2h) │   │   │   │
│ Subnet 6 (30 hosts)  │             └──────┴───┴───┴───┘
├──────────────────────┤             ← Subnet terakhir dipecah
│ Subnet 7 (30 hosts)  │               menjadi 8 subnet /30
└──────────────────────┘
  84 alamat terbuang!                  Tidak ada yang terbuang!
```

---

## D. Implementasi VLSM — Langkah demi Langkah

### Langkah 1 — Identifikasi Semua Kebutuhan Subnet dan Urutkan dari Terbesar ke Terkecil

Dari skenario topologi yang sama, kebutuhan subnet diurutkan sebagai berikut:

| Subnet | Kebutuhan Host | Urutan Prioritas |
|---|---|---|
| Building D | 28 hosts | 1 (terbesar) |
| Building A | 25 hosts | 2 |
| Building B | 20 hosts | 3 |
| Building C | 15 hosts | 4 |
| WAN Link R1–R2 | 2 hosts | 5 |
| WAN Link R2–R3 | 2 hosts | 6 |
| WAN Link R3–R4 | 2 hosts | 7 (terkecil) |

### Langkah 2 — Alokasikan Subnet untuk LAN (Kebutuhan Terbesar)

- Untuk subnet terbesar (28 host), prefix **/27** digunakan karena 2⁵ - 2 = 30 ≥ 28 ✓
- Block size /27 = **32** (setiap subnet melompat 32 alamat)
- Alokasikan empat subnet /27 berturut-turut untuk keempat gedung:

| Subnet LAN | Network Address | Host Range | Broadcast | Host Aktual |
|---|---|---|---|---|
| Building A | 192.168.20.0**/27** | .1 – .30 | .31 | 25 hosts |
| Building B | 192.168.20.32**/27** | .33 – .62 | .63 | 20 hosts |
| Building C | 192.168.20.64**/27** | .65 – .94 | .95 | 15 hosts |
| Building D | 192.168.20.96**/27** | .97 – .126 | .127 | 28 hosts |

- Setelah empat subnet /27 dialokasikan, subnet berikutnya yang tersedia dimulai dari **192.168.20.128/27** — subnet inilah yang akan dipecah lebih lanjut menggunakan VLSM untuk kebutuhan WAN.

### Langkah 3 — Pecah Subnet Tersisa untuk WAN (Kebutuhan Terkecil)

- Subnet `192.168.20.128/27` yang tersisa memiliki **30 host** yang dapat digunakan — jauh lebih besar dari kebutuhan WAN yang hanya 2 host.
- Dengan VLSM, subnet `192.168.20.128/27` ini dipecah menjadi **8 subnet /30** untuk memenuhi kebutuhan link WAN point-to-point.
- Prefix **/30** digunakan untuk WAN karena 2² - 2 = **2 host** — tepat sesuai kebutuhan koneksi point-to-point antar dua router.
- Block size /30 = **4** (setiap subnet WAN melompat 4 alamat)

| Subnet WAN | Network Address | Host Range | Broadcast | Host Aktual |
|---|---|---|---|---|
| Link R1–R2 | 192.168.20.224**/30** | .225 – .226 | .227 | 2 hosts |
| Link R2–R3 | 192.168.20.228**/30** | .229 – .230 | .231 | 2 hosts |
| Link R3–R4 | 192.168.20.232**/30** | .233 – .234 | .235 | 2 hosts |

> **Catatan:** Subnet WAN dimulai dari .224 bukan .128, karena dalam implementasi nyata subnet /27 yang dipecah adalah subnet yang tersedia setelah keempat subnet LAN dialokasikan — yaitu mulai dari 192.168.20.224/27 yang kemudian dipecah menjadi /30.

---

## E. VLSM Topology Address Assignment — Detail Lengkap

Berikut adalah hasil akhir pengalamatan topologi setelah VLSM diterapkan secara lengkap, termasuk alamat yang ditetapkan ke setiap interface router:

### Jaringan LAN (menggunakan /27):

| Lokasi | Network | Interface Router | Alamat Router | Jumlah Host |
|---|---|---|---|---|
| Building A | 192.168.20.0/27 | R1 G0/0/0 | 192.168.20.1/27 | 25 hosts |
| Building B | 192.168.20.32/27 | R2 G0/0/0 | 192.168.20.33/27 | 20 hosts |
| Building C | 192.168.20.64/27 | R3 G0/0/0 | 192.168.20.65/27 | 15 hosts |
| Building D | 192.168.20.96/27 | R4 G0/0/0 | 192.168.20.97/27 | 28 hosts |

### Jaringan WAN antar-router (menggunakan /30):

| Koneksi | Network | Alamat R (sisi A) | Interface | Alamat R (sisi B) | Interface |
|---|---|---|---|---|---|
| R1 – R2 | 192.168.20.224/30 | R1: .225 | G0/0/1 | R2: .226 | G0/0/1 |
| R2 – R3 | 192.168.20.228/30 | R2: .229 | G0/1/0 | R3: .230 | G0/0/1 |
| R3 – R4 | 192.168.20.232/30 | R3: .233 | G0/1/0 | R4: .234 | G0/0/1 |

> **Analogi:** VLSM seperti memotong kue dengan cerdas. Potongan besar untuk yang lapar (subnet LAN), potongan kecil untuk yang hanya mau sedikit (subnet WAN). Tidak ada kue yang terbuang, semua orang puas sesuai porsinya masing-masing.

---

## F. Perbandingan Hasil: Subnetting Tradisional vs VLSM

| Aspek | Subnetting Tradisional (/27) | VLSM (/27 + /30) |
|---|---|---|
| **Subnet mask** | Seragam — semua /27 | Bervariasi — /27 untuk LAN, /30 untuk WAN |
| **Subnet LAN** | 4 subnet /27 | 4 subnet /27 |
| **Subnet WAN** | 3 subnet /27 (30 host/subnet) | 3 subnet /30 (2 host/subnet) |
| **Alamat terbuang di WAN** | 28 × 3 = **84 alamat** | 0 alamat |
| **Efisiensi penggunaan alamat** | Rendah | Tinggi |
| **Kompleksitas perancangan** | Sederhana | Lebih kompleks |
| **Fleksibilitas** | Terbatas | Sangat fleksibel |

---

## G. Aturan dan Prinsip Penting dalam VLSM

Berikut adalah prinsip-prinsip kunci VLSM yang perlu dihapal untuk keperluan ujian:

**1. Selalu mulai dari subnet terbesar**
Alokasikan subnet dengan kebutuhan host terbesar terlebih dahulu, baru kemudian lanjutkan ke subnet yang lebih kecil. Urutan ini penting untuk memastikan tidak ada konflik atau tumpang tindih antar subnet.

**2. Subnet tidak boleh tumpang tindih**
Setiap alamat dalam suatu subnet tidak boleh digunakan oleh subnet lain. Pastikan setiap network address dan host range tidak saling bertabrakan.

**3. Gunakan prefix yang paling efisien**
Untuk setiap kelompok subnet, pilih prefix terkecil yang masih memenuhi kebutuhan host — jangan menggunakan subnet yang jauh lebih besar dari kebutuhan aktual.

**4. /30 adalah standar untuk WAN point-to-point**
Koneksi WAN antar dua router selalu menggunakan **/30** karena hanya membutuhkan tepat 2 alamat host — satu untuk tiap ujung koneksi.

**5. Sisa subnet dapat digunakan untuk pengembangan**
Subnet-subnet yang belum dialokasikan dapat disimpan untuk kebutuhan jaringan di masa depan tanpa harus merombak keseluruhan skema pengalamatan.

---

## Ringkasan Konsep Kunci Sub-Bab 11.8

| Konsep | Penjelasan Singkat |
|---|---|
| **VLSM** | Teknik subnetting yang memungkinkan subnet mask berbeda-beda dalam satu jaringan |
| **Masalah subnetting tradisional** | Semua subnet berukuran sama sehingga banyak alamat terbuang di subnet kecil |
| **Pemborosan WAN tradisional** | 3 subnet WAN /27 membuang 84 alamat yang tidak terpakai |
| **Prinsip VLSM** | Mulai dari subnet terbesar, lanjutkan ke yang terkecil secara bertahap |
| **Subnet dari subnet** | VLSM memecah subnet yang tersisa menjadi subnet-subnet lebih kecil |
| **Prefix /27 untuk LAN** | Menyediakan 30 host; digunakan untuk gedung dengan 15–28 host |
| **Prefix /30 untuk WAN** | Menyediakan 2 host; standar untuk koneksi point-to-point antar router |
| **Keunggulan VLSM** | Tidak ada alamat yang terbuang; penggunaan ruang alamat lebih efisien |

---

Siap lanjut ke **Bab 11.9**? Instruksikan kapan kamu siap!