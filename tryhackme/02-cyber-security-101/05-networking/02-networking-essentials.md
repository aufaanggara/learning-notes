# Resume Materi — Networking Essentials
**TryHackMe | 22 Oktober 2026**

---

## 1. DHCP — Dynamic Host Configuration Protocol

### 1.1 Fungsi & Kebutuhan

**DHCP** adalah protokol yang mengonfigurasi perangkat secara otomatis saat terhubung ke jaringan baru, sehingga pengguna tidak perlu mengatur manual.

Konfigurasi minimal yang dibutuhkan setiap perangkat untuk mengakses jaringan:
- IP address beserta subnet mask
- Router (gateway)
- DNS server

Tanpa DHCP, setiap perangkat harus dikonfigurasi manual — rentan konflik IP jika dua perangkat mendapat IP yang sama, dan sangat tidak efisien untuk jaringan besar.

### 1.2 Detail Teknis

- Protokol berbasis **UDP**, application-level
- Server mendengarkan di **port 67**
- Client mengirim dari **port 68**
- Smartphone dan laptop menggunakan DHCP secara default

### 1.3 Proses DORA

DHCP bekerja melalui empat langkah berurutan yang disingkat **DORA**:

| Langkah | Pesan | Arah | Keterangan |
|---|---|---|---|
| Discover | DHCPDISCOVER | Client → broadcast | Client cari DHCP server |
| Offer | DHCPOFFER | Server → client | Server tawarkan IP tersedia |
| Request | DHCPREQUEST | Client → broadcast | Client terima tawaran |
| Acknowledge | DHCPACK | Server → client | Server konfirmasi IP assigned |

**Poin kritis:** Pada langkah Discover dan Request, client belum punya IP sehingga:
- Source IP = `0.0.0.0`
- Destination IP = `255.255.255.255` (broadcast)
- Destination MAC = `ff:ff:ff:ff:ff:ff` (broadcast layer 2)

---

## 2. ARP — Address Resolution Protocol

### 2.1 Fungsi & Alasan Keberadaan

**ARP** menerjemahkan IP address (layer 3) menjadi MAC address (layer 2).

Komunikasi di jaringan Ethernet dan WiFi pada level frame membutuhkan MAC address, bukan IP address. Ketika sebuah host tahu IP tujuan tetapi belum tahu MAC address-nya, ARP digunakan untuk mencari tahu.

Perangkat tidak perlu mengetahui MAC address satu sama lain sepanjang waktu — hanya saat sedang aktif berkomunikasi.

### 2.2 MAC Address

- Panjang **48 bit**, direpresentasikan dalam notasi heksadesimal
- Contoh: `44:DF:65:D8:FE:6C`
- Tertanam pada kartu jaringan fisik perangkat

### 2.3 Cara Kerja ARP

**ARP Request:**
- Dikirim dari MAC address pengirim ke broadcast MAC `ff:ff:ff:ff:ff:ff`
- Isi: "Who has IP x.x.x.x? Tell x.x.x.x"
- Semua perangkat di LAN menerima, hanya yang memiliki IP tersebut yang membalas

**ARP Reply:**
- Dikirim unicast kembali ke penanya
- Isi: "IP x.x.x.x is at MAC xx:xx:xx:xx:xx:xx"
- Setelah ini, kedua host bisa bertukar frame layer 2

### 2.4 Karakteristik Teknis

ARP **tidak dibungkus dalam paket UDP maupun IP** — dikemas langsung di dalam **Ethernet frame** dengan Type field `0x0806`.

Posisi ARP dalam model layer sering diperdebatkan:
- Dianggap **layer 2** karena berurusan dengan MAC address
- Ada yang argumenkan **layer 3** karena mendukung operasi IP
- Yang pasti: ARP adalah **jembatan dari layer 3 ke layer 2**

### 2.5 Membaca Output ARP

```bash
# tshark — membaca file capture ARP
tshark -r arp.pcapng -Nm
```

Output tshark menggunakan format: `Who has [IP]? Tell [IP]` dan `[IP] is at [MAC]`

```bash
# tcpdump — alternatif dengan terminologi berbeda
tcpdump -r arp.pcapng -n -v
```

Output tcpdump menggunakan istilah **Request** dan **Reply** secara eksplisit.

**Switch yang digunakan:**

| Switch | Fungsi |
|---|---|
| `-r [file]` | Baca file capture (.pcapng/.pcap) |
| `-N` | Jangan resolve nama host (tshark) |
| `-m` | Tampilkan MAC address (tshark) |
| `-n` | Jangan resolve nama host (tcpdump) |
| `-v` | Verbose — tampilkan detail lebih lengkap |

---

## 3. ICMP — Internet Control Message Protocol

### 3.1 Fungsi

**ICMP** digunakan untuk **diagnostik jaringan dan pelaporan error** — bukan untuk transfer data biasa. Dua command populer yang mengandalkan ICMP adalah `ping` dan `traceroute`.

### 3.2 Ping

`ping` mengirimkan **ICMP Echo Request** dan menunggu **ICMP Echo Reply** dari target, mengukur apakah host bisa dijangkau dan berapa lama waktu bolak-baliknya (RTT).

- **ICMP Type 8** = Echo Request (dikirim pengirim)
- **ICMP Type 0** = Echo Reply (dikirim target sebagai respons)

```bash
ping [target] -c 4
```

**Switch ping:**

| Switch | Fungsi |
|---|---|
| `-c [n]` | Kirim sejumlah n paket lalu berhenti (Linux) |

**Output ping:**

```
64 bytes from 192.168.11.1: icmp_seq=1 ttl=63 time=11.2 ms
```

- `bytes` — ukuran data dalam paket (bukan total frame)
- `icmp_seq` — nomor urut paket
- `ttl` — Time-to-Live sisa
- `time` — RTT dalam milidetik

Statistik akhir mencakup: jumlah paket terkirim & diterima, persentase packet loss, serta nilai **min/avg/max/mdev** dari RTT.

**Kenapa ping bisa gagal:**
- Target sedang offline atau dimatikan
- Firewall di sepanjang jalur memblokir paket ICMP

### 3.3 Traceroute

`traceroute` melacak seluruh rute yang dilalui paket dari source ke destination dengan mengungkap setiap router di sepanjang jalur.

**Nama perintah:**
- Linux/UNIX: `traceroute`
- Windows: `tracert`

**Mekanisme TTL:**

**TTL (Time-to-Live)** adalah field di IP header yang menunjukkan jumlah maksimum router yang boleh dilalui paket sebelum dibuang. Setiap router mengurangi TTL sebesar 1. Ketika TTL mencapai 0, router membuang paket dan mengirim **ICMP Type 11 (Time Exceeded)** kembali ke pengirim.

Traceroute memanfaatkan mekanisme ini — dengan mengirim paket ber-TTL rendah secara bertahap (1, 2, 3, dst.), setiap router di jalur dipaksa mengirimkan ICMP Time Exceeded, sehingga posisinya terungkap.

```bash
traceroute example.com
```

**Membaca output traceroute:**
- Setiap baris = satu hop (satu router)
- Tiga nilai waktu per baris = tiga probe RTT ke router tersebut
- `* * *` = router tidak merespons (memblokir atau tidak mengirim ICMP)
- Router ISP mungkin menampilkan IP privat atau publik
- Dari IP publik router, lokasi geografisnya bisa ditelusuri

**Catatan:** Rute yang ditampilkan bisa berubah setiap kali perintah dijalankan ulang.

### 3.4 Tabel ICMP Type Penting

| Type | Nama | Konteks |
|---|---|---|
| 0 | Echo Reply | Respons ping |
| 8 | Echo Request | Permintaan ping |
| 11 | Time Exceeded | TTL habis, dipakai traceroute |

---

## 4. Routing

### 4.1 Konsep Dasar

**Routing** adalah proses menentukan jalur terbaik untuk mengirimkan paket antar jaringan yang berbeda. Router menggunakan **routing table** dan **routing algorithm** untuk memutuskan lewat link mana paket diteruskan.

Karena ada lebih dari satu jalur yang bisa menghubungkan dua titik di Internet, router membutuhkan protokol routing untuk berkoordinasi dan memilih jalur optimal.

### 4.2 Routing Protocols

| Protokol | Kepanjangan | Karakteristik Utama |
|---|---|---|
| **OSPF** | Open Shortest Path First | Router saling berbagi peta topologi lengkap, hitung jalur paling efisien |
| **EIGRP** | Enhanced Interior Gateway Routing Protocol | Milik Cisco (proprietary), pertimbangkan bandwidth & delay |
| **BGP** | Border Gateway Protocol | Protokol utama Internet, hubungkan antar ISP/jaringan berbeda |
| **RIP** | Routing Information Protocol | Sederhana, pilih rute berdasarkan jumlah hop terkecil, cocok jaringan kecil |

---

## 5. NAT — Network Address Translation

### 5.1 Latar Belakang

IPv4 hanya mendukung maksimum sekitar 4 miliar alamat — tidak cukup untuk semua perangkat di dunia yang terus bertambah. **NAT** hadir sebagai solusi dengan memungkinkan banyak perangkat berbagi satu IP publik.

### 5.2 Cara Kerja

Router yang menjalankan NAT memelihara **tabel translasi** yang memetakan kombinasi IP privat + port ke kombinasi IP publik + port baru.

- Paket keluar: IP privat diganti dengan IP publik
- Paket masuk: IP publik diterjemahkan kembali ke IP privat yang sesuai
- Proses ini terjadi secara seamless tanpa perangkat menyadarinya

**Contoh tabel NAT:**

| Internal (IP:Port) | External (IP:Port) |
|---|---|
| 192.168.0.129:15401 | 212.3.4.5:19273 |
| 192.168.0.137:27912 | 212.3.4.5:32759 |
| 192.168.0.112:38192 | 212.3.4.5:41251 |

### 5.3 Batas Kapasitas NAT

Port TCP memiliki rentang **0 sampai 65535**, sehingga dengan satu IP publik, NAT dapat menangani maksimum sekitar **65.000 koneksi simultan**.

Secara teknis, jumlah IP yang dihemat selalu dinyatakan sebagai pangkat dua — dengan NAT, cukup 2 IP publik untuk menggantikan 32, menghemat 30 IP publik.

### 5.4 NAT vs Routing

- **Routing** = cara alami meneruskan paket ke tujuan berdasarkan IP
- **NAT** = harus aktif melacak dan menerjemahkan setiap koneksi yang berjalan

---

## 6. Broadcast Address

Konsep broadcast digunakan oleh DHCP dan ARP untuk menjangkau semua perangkat di jaringan sekaligus.

- **IP broadcast:** `255.255.255.255` — kirim ke semua host di jaringan
- **MAC broadcast:** `ff:ff:ff:ff:ff:ff` — kirim ke semua perangkat di LAN
- Keduanya bernilai maksimum (semua bit = 1)

Dipakai oleh: DHCP Discover, DHCP Request, ARP Request

---

## 7. Ringkasan Pemilihan Protokol

| Skenario | Protokol |
|---|---|
| Dapat IP otomatis saat konek ke jaringan baru | DHCP |
| Cari MAC address dari IP yang diketahui | ARP |
| Cek apakah host bisa dijangkau & ukur RTT | ICMP (ping) |
| Lacak rute paket ke destination | ICMP (traceroute) |
| Banyak perangkat berbagi satu IP publik | NAT |
| Tentukan jalur terbaik antar jaringan | Routing |

---

## 8. Glossary

**RTT (Round-Trip Time)** — Waktu yang dibutuhkan paket untuk pergi ke target dan kembali lagi ke pengirim.

**TTL (Time-to-Live)** — Field di IP header yang membatasi jumlah router yang boleh dilalui paket; berkurang 1 di setiap hop.

**Hop** — Satu lompatan paket melewati satu router dalam perjalanannya ke tujuan.

**MAC Address** — Alamat fisik perangkat jaringan, 48-bit dalam format heksadesimal, digunakan di layer 2.

**IP Privat** — Alamat IP untuk jaringan internal/lokal, tidak dapat di-routing langsung di Internet.

**IP Publik** — Alamat IP yang dapat dikenali dan diakses dari Internet.

**Broadcast** — Pengiriman paket ke semua perangkat di jaringan secara bersamaan.

**Unicast** — Pengiriman paket ke satu perangkat spesifik.

**Packet Loss** — Persentase paket yang gagal sampai ke tujuan dari total paket yang dikirim.

**mdev (Mean Deviation)** — Ukuran variasi/inkonsistensi RTT dalam hasil ping; semakin kecil semakin stabil.

**Port** — Nomor identifikasi layanan atau koneksi, rentang 0–65535.

**Routing Table** — Tabel yang disimpan router berisi informasi jalur ke berbagai jaringan.

**DORA** — Akronim untuk empat langkah DHCP: Discover, Offer, Request, Acknowledge.

**Tabel Translasi NAT** — Catatan yang dikelola router NAT berisi pemetaan IP:port internal ke IP:port eksternal.

**ICMP Time Exceeded** — Pesan ICMP Type 11 yang dikirim router ketika TTL sebuah paket mencapai nol.

---

## 9. Tools & Platform Rujukan

**tshark**
Versi command-line dari Wireshark untuk membaca dan menganalisis file packet capture (.pcapng/.pcap). Dipakai di materi untuk menampilkan paket DHCP dan ARP.

**tcpdump**
Tool command-line untuk capture dan analisis paket jaringan. Alternatif tshark dengan terminologi output yang berbeda (Request/Reply untuk ARP).

**Wireshark**
GUI tool untuk analisis paket jaringan secara visual. Ditampilkan di materi untuk melihat detail struktur paket ICMP, ARP, dan Ethernet frame.

---

## 10. Catatan Ringkas untuk Ditulis Tangan

### DHCP
- Fungsi — konfigurasi jaringan otomatis (IP, gateway, DNS)
- UDP port 67 (server), port 68 (client)
- DORA: Discover → Offer → Request → Acknowledge
- Sebelum dapat IP: src = 0.0.0.0, dst = 255.255.255.255
- MAC broadcast = ff:ff:ff:ff:ff:ff

### ARP
- Fungsi — terjemahkan IP → MAC address
- Request: broadcast ke ff:ff:ff:ff:ff:ff
- Reply: unicast balik ke penanya
- Tidak pakai UDP/IP — langsung dalam Ethernet frame
- Type field = 0x0806
- Posisi: jembatan layer 3 → layer 2

### ICMP
- Fungsi — diagnostik & error reporting jaringan
- Type 8 = Echo Request (ping keluar)
- Type 0 = Echo Reply (ping balik)
- Type 11 = Time Exceeded (TTL habis, untuk traceroute)
- ping -c [n] — stop setelah n paket (Linux)
- Output ping: bytes, ttl, time, min/avg/max/mdev, packet loss%
- traceroute (Linux) / tracert (Windows)
- TTL — berkurang 1 tiap router; saat = 0, router kirim Type 11
- * * * — router tidak kirim balasan ICMP

### Routing
- OSPF — share peta topologi, hitung jalur efisien
- EIGRP — Cisco proprietary, pertimbangkan bandwidth & delay
- BGP — protokol utama Internet, hubungkan antar ISP
- RIP — sederhana, pilih rute hop terkecil, jaringan kecil

### NAT
- Fungsi — banyak IP privat berbagi 1 IP publik
- Router kelola tabel: IP:port internal ↔ IP:port eksternal
- Maks ~65.000 koneksi simultan per IP publik (range port 0–65535)
- NAT vs routing: NAT aktif track & terjemahkan, routing natural forward

### Broadcast
- 255.255.255.255 — IP broadcast (semua host)
- ff:ff:ff:ff:ff:ff — MAC broadcast (semua perangkat LAN)
- Dipakai: DHCP Discover, DHCP Request, ARP Request

### Pemilihan Protokol Cepat
- IP otomatis → DHCP
- Cari MAC dari IP → ARP
- Cek host hidup/RTT → ICMP ping
- Lacak rute paket → ICMP traceroute
- Banyak device, 1 IP publik → NAT
- Jalur terbaik antar jaringan → Routing

### Command & Switch Ringkas
- tshark -r [file] — baca capture
- tshark -N — jangan resolve hostname
- tshark -m — tampilkan MAC
- tcpdump -r [file] — baca capture
- tcpdump -n — jangan resolve hostname
- tcpdump -v — verbose/detail lebih
- ping -c [n] — kirim n paket (Linux)
- traceroute [target] — lacak rute (Linux)
- tracert [target] — lacak rute (Windows)
