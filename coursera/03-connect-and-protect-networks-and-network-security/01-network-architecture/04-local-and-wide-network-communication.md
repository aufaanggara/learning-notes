# Resume: IP Addresses, MAC Addresses & Network Layer Communication

Tanggal: 17 Agustus 2026

## 1. Konsep Dasar IP Address

**IP address** (Internet Protocol address) adalah rangkaian karakter unik yang mengidentifikasi lokasi sebuah perangkat di internet, setara dengan alamat pos untuk sebuah rumah.

Ada dua versi IP address: **IPv4** dan **IPv6**. IPv4 dikembangkan lebih dulu, namun pertumbuhan internet yang pesat membuat seluruh kombinasi alamat IPv4 lama-kelamaan habis (**IPv4 address exhaustion**). IPv6 dikembangkan untuk mengatasi masalah ini dengan menyediakan ruang alamat jauh lebih besar.

## 2. Public vs Private IP Address

**Public IP address** diberikan oleh internet service provider (ISP) dan terhubung dengan lokasi geografis pengguna. Seluruh perangkat dalam satu local area network memakai public IP address yang sama saat berkomunikasi ke internet, karena adanya network address translation atau forwarding proxy.

**Private IP address** hanya terlihat oleh perangkat lain dalam jaringan lokal yang sama, tidak terlihat dari internet. Tiap perangkat dalam satu jaringan rumah/kantor punya private IP address unik untuk komunikasi internal.

## 3. Format IPv4 vs IPv6

**IPv4** — empat angka desimal (0-255) dipisah titik, total 4 bytes, kapasitas hingga 4,3 miliar alamat.
Contoh: `172.16.254.1`

**IPv6** — delapan angka hexadecimal (maks 4 digit) dipisah titik dua, total 16 bytes, kapasitas hingga 340 undecillion alamat.
Contoh: `2001:0db8:85a3:0000:0000:8a2e:0370:7336`

Kelompok angka nol berurutan pada IPv6 dapat disingkat dengan double colon `::`. Contoh `2002:0db8:0000:0000:0000:ff21:0023:1234` menjadi `2002:0db8::ff21:0023:1234`.

## 4. Struktur Packet IPv4

Sebuah IPv4 packet terdiri dari **header** (20-60 bytes) dan **data** (isi pesan yang ditransfer). Total ukuran packet 20-65.535 bytes.

20 bytes pertama header bersifat tetap, berisi informasi inti seperti source/destination IP address, header length, total length. Sisanya (0-40 bytes) adalah options field yang opsional.

### 4.1 Tiga Belas Field Header IPv4

- **Version (VER)** — 4 bit, menunjukkan protokol yang dipakai packet (IPv4).
- **IP Header Length (HLEN/IHL)** — menunjukkan batas antara header dan data.
- **Type of Service (ToS)** — memberi info prioritas pengiriman ke router untuk menjaga quality of service.
- **Total Length** — total panjang packet (header + data), maksimum 65.535 bytes.
- **Identification** — identifier unik untuk fragment-fragment dari packet asli yang di-fragment, agar bisa disusun kembali di tujuan.
- **Flags** — info tambahan apakah packet sudah di-fragment dan apakah masih ada fragment lain dalam perjalanan.
- **Fragmentation Offset** — posisi sebuah fragment dalam packet aslinya.
- **Time to Live (TTL)** — counter yang berkurang satu tiap melewati router; saat mencapai nol, packet dibuang dan router mengirim ICMP Time Exceeded ke pengirim. Mencegah packet diteruskan tanpa batas.
- **Protocol** — memberi tahu protokol apa yang dipakai untuk bagian data packet.
- **Header Checksum** — checksum untuk mendeteksi kerusakan header; packet rusak dibuang.
- **Source IP Address** — alamat IPv4 pengirim.
- **Destination IP Address** — alamat IPv4 tujuan.
- **Options** — opsi keamanan tambahan, berlaku jika HLEN lebih besar dari lima.

## 5. Perbedaan Header IPv4 dan IPv6

Header IPv6 jauh lebih sederhana dibanding IPv4. Field **IHL**, **Identification**, dan **Flags** pada IPv4 tidak ada di IPv6.

IPv6 memperkenalkan field baru **Flow Label**, yang menandai packet yang butuh penanganan khusus oleh router IPv6 lain.

Beberapa nama field berubah: Type of Service (IPv4) menjadi **Traffic Class** (IPv6), Total Length menjadi **Payload Length**. Field seperti Version, Source Address, dan Destination Address tetap dipertahankan meski ada perubahan ukuran/posisi.

Dari sisi keamanan, IPv6 menawarkan routing lebih efisien dan menghilangkan **private address collision** — konflik saat dua perangkat pada jaringan yang sama tidak sengaja memakai IP address identik, yang rawan terjadi di IPv4.

## 6. Network Layer (Layer 3 OSI)

Network layer mengatur pengalamatan dan pengiriman data packet dari host device ke destination device, termasuk mengarahkan packet melintasi router demi router hingga mencapai jaringan tujuan berdasarkan IP address.

Data packet disebut **IP packet** untuk koneksi TCP, dan **datagram** untuk koneksi UDP. Router memakai informasi pada IP header (source address, ukuran packet, protokol, bukan hanya destination) untuk merutekan packet antar jaringan. Destination IP address pada header disimpan dalam **routing table** untuk keperluan routing selanjutnya.

## 7. MAC Address dan Peran Switch

**MAC address** adalah identifier hexadecimal unik yang diberikan pada tiap perangkat fisik dalam jaringan — berbeda dari IP address yang menunjukkan lokasi logis, MAC address mengidentifikasi perangkat secara fisik.

Saat **switch** menerima data packet, switch membaca MAC address tujuan dan memetakannya ke port tertentu. Pemetaan ini disimpan dalam **MAC address table**, dipakai switch sebagai acuan untuk mengarahkan packet ke perangkat yang benar.

## 8. Relevansi Keamanan

Menganalisis field dalam IP data packet dapat mengungkap informasi keamanan penting: dari mana packet berasal (source IP), ke mana dituju (destination IP), dan protokol apa yang dipakai.

Pemahaman struktur data IP packet memungkinkan pengambilan keputusan tepat terkait implikasi keamanan packet yang diinspeksi, misalnya mendeteksi anomali source address, protocol tidak wajar, atau indikasi fragmentasi mencurigakan.

## 9. Glossary

- **IP address** — alamat unik yang mengidentifikasi lokasi perangkat di internet.
- **IPv4** — versi IP address 32-bit, empat angka desimal dipisah titik.
- **IPv6** — versi IP address 128-bit, delapan angka hexadecimal dipisah titik dua, mengatasi IPv4 address exhaustion.
- **Public IP address** — alamat dari ISP terhubung ke lokasi geografis, dipakai bersama seluruh perangkat dalam satu jaringan saat menghadap internet.
- **Private IP address** — alamat unik antar perangkat dalam jaringan lokal, tidak terlihat dari internet.
- **MAC address** — identifier hexadecimal unik untuk tiap perangkat fisik dalam jaringan.
- **MAC address table** — pemetaan MAC address ke port switch, dipakai untuk mengarahkan packet.
- **Router** — perangkat yang merutekan packet antar jaringan berdasarkan IP address.
- **Switch** — perangkat yang mengarahkan packet ke perangkat tujuan dalam satu jaringan berdasarkan MAC address.
- **Routing table** — penyimpanan destination IP address untuk keperluan routing packet berikutnya.
- **IP packet / datagram** — sebutan data packet pada koneksi TCP (IP packet) dan UDP (datagram).
- **TTL (Time to Live)** — counter pada header IPv4 yang membatasi jumlah hop packet sebelum dibuang.
- **Fragmentation** — proses memecah packet besar menjadi packet lebih kecil agar sesuai batas jaringan.
- **Flow Label** — field baru IPv6 untuk menandai packet yang butuh penanganan khusus.
- **Private address collision** — konflik akibat dua perangkat pada jaringan sama memakai IP address identik (rawan di IPv4, dihilangkan di IPv6).

## 10. Tools & Platform Rujukan

Tidak ada tools atau platform eksternal (seperti Talos, VirusTotal, Shodan, AbuseIPDB, dsb.) yang disebutkan pada materi room ini. Seluruh materi murni membahas konsep IP address, MAC address, dan struktur packet secara teoritis, tanpa referensi tools analisis eksternal.

## 11. Catatan Ringkas untuk Ditulis Tangan

### IP Address Dasar

- IP address — alamat unik lokasi perangkat di internet
- IPv4 — 32-bit, 4 angka desimal dipisah titik, contoh 172.16.254.1
- IPv6 — 128-bit, 8 angka hex dipisah titik dua, contoh 2001:0db8:85a3::8a2e:0370:7336
- IPv4 address exhaustion — alasan IPv6 dibuat
- `::` — singkatan kelompok nol berurutan di IPv6

### Public vs Private

- Public IP — dari ISP, terhubung lokasi geografis, dipakai bersama 1 jaringan ke internet
- Private IP — unik per perangkat, hanya terlihat dalam jaringan lokal

### Struktur Packet IPv4

- Header — 20-60 bytes
- Data — isi pesan
- Total packet — 20-65.535 bytes
- VER — versi protokol
- HLEN/IHL — panjang header
- ToS — prioritas pengiriman
- Total Length — panjang total packet
- Identification — ID fragment untuk reassembly
- Flags — status fragmentasi
- Fragmentation Offset — posisi fragment
- TTL — counter hop, habis = packet dibuang + ICMP Time Exceeded
- Protocol — protokol data
- Header Checksum — deteksi kerusakan header
- Source/Destination IP — alamat pengirim/tujuan
- Options — opsi keamanan tambahan (jika HLEN > 5)

### IPv4 vs IPv6 Header

- IPv6 tidak punya IHL, Identification, Flags
- Flow Label — field baru IPv6, penanganan khusus
- ToS (IPv4) → Traffic Class (IPv6)
- Total Length (IPv4) → Payload Length (IPv6)
- IPv6 hilangkan private address collision

### Network Layer

- Layer 3 OSI — addressing & delivery packet
- IP packet — sebutan di TCP
- Datagram — sebutan di UDP
- Routing table — simpan destination IP untuk routing berikutnya

### MAC Address & Switch

- MAC address — ID hex unik per perangkat fisik
- MAC address table — pemetaan MAC ke port, dipakai switch arahkan packet

### Keamanan

- Analisis IP packet ungkap: source, destination, protocol
- Guna: deteksi anomali, packet mencurigakan
