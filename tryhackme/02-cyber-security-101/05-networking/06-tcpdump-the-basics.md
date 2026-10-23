# Resume Materi: Tcpdump — The Basics
**Platform:** TryHackMe | **Tanggal:** 1 September 2026

---

## 1. Pengantar Tcpdump

**Tcpdump** adalah command-line tool untuk menangkap dan menganalisis lalu lintas jaringan secara langsung (live capture) maupun dari file rekaman.

Ditulis dalam bahasa C dan C++, dirilis untuk sistem Unix pada akhir 1980-an atau awal 1990-an. Sangat stabil dan cepat karena berbasis library **libpcap** — fondasi dari banyak tools jaringan modern, termasuk Wireshark. Versi Windows-nya disebut **winpcap**.

Tujuan utama belajar tcpdump adalah memahami protokol jaringan secara nyata, bukan hanya teori — karena semua kompleksitas teknis seperti ARP query, three-way handshake, dan sebagainya tersembunyi di balik antarmuka yang ramah.


---

## 2. Basic Packet Capture

### 2.1 Menentukan Network Interface

Sebelum menangkap, pilih interface mana yang ingin didengarkan.

```
tcpdump -i INTERFACE
tcpdump -i any          # semua interface sekaligus
tcpdump -i eth0         # interface spesifik
```

Untuk melihat daftar interface yang tersedia:

```
ip address show
ip a s
```

### 2.2 Menyimpan Paket ke File

```
tcpdump -w FILE.pcap
```

Ekstensi yang umum dipakai adalah `.pcap`. Saat opsi `-w` aktif, paket tidak akan tampil di layar — langsung disimpan ke file. File ini bisa dibuka nanti dengan Wireshark atau dibaca ulang dengan tcpdump.

### 2.3 Membaca Paket dari File

```
tcpdump -r FILE.pcap
```

Berguna untuk analisis ulang tanpa harus capture live. Tidak memerlukan hak akses root karena hanya membaca file.

### 2.4 Membatasi Jumlah Paket

```
tcpdump -c COUNT
```

Tanpa `-c`, capture berjalan terus sampai dihentikan manual dengan CTRL-C.

### 2.5 Tidak Me-resolve Nama

```
tcpdump -n     # tidak resolve IP address ke domain name
tcpdump -nn    # tidak resolve IP address DAN port number
```

Tanpa flag ini, tcpdump otomatis menerjemahkan IP ke nama domain (misal `93.184.215.14` jadi `example.com`) dan port ke nama layanan (misal `80` jadi `http`). Pakai `-nn` untuk melihat angka mentahnya.

### 2.6 Tingkat Verbositas Output

```
tcpdump -v     # sedikit lebih detail (TTL, total length, dll)
tcpdump -vv    # lebih verbose lagi
tcpdump -vvv   # paling verbose; lihat manual untuk detail lengkap
```

### 2.7 Ringkasan Switch Basic

| Switch | Fungsi |
|---|---|
| `-i INTERFACE` | Pilih network interface |
| `-w FILE` | Simpan paket ke file |
| `-r FILE` | Baca paket dari file |
| `-c COUNT` | Batasi jumlah paket yang ditangkap |
| `-n` | Jangan resolve IP address |
| `-nn` | Jangan resolve IP dan port |
| `-v / -vv / -vvv` | Tingkatkan detail output |


---

## 3. Filtering Expressions

Tanpa filter, tcpdump menampilkan semua paket — tidak praktis dalam kondisi nyata karena jumlahnya bisa jutaan.

### 3.1 Filter by Host

```
tcpdump host IP
tcpdump host HOSTNAME

tcpdump src host IP           # hanya dari sumber ini
tcpdump dst host IP           # hanya ke tujuan ini
```

Menangkap semua paket yang melibatkan host tertentu, baik sebagai pengirim maupun penerima. Capture membutuhkan hak root atau `sudo`.

### 3.2 Filter by Port

```
tcpdump port PORT_NUMBER
tcpdump src port PORT_NUMBER
tcpdump dst port PORT_NUMBER
```

Contoh umum: `port 53` untuk DNS, `port 80` untuk HTTP, `port 443` untuk HTTPS, `port 22` untuk SSH.

Catatan: DNS menggunakan UDP dan TCP port 53 secara default.

### 3.3 Filter by Protocol

```
tcpdump PROTOCOL
```

Protokol yang bisa digunakan langsung sebagai filter: `ip`, `ip6`, `udp`, `tcp`, `icmp`, `arp`.

Catatan: `dns` bukan nama protokol yang dikenali — gunakan `port 53` sebagai gantinya.

### 3.4 Logical Operators

| Operator | Fungsi | Contoh |
|---|---|---|
| `and` | Kedua kondisi harus terpenuhi | `tcpdump host 1.1.1.1 and tcp` |
| `or` | Salah satu kondisi cukup | `tcpdump udp or icmp` |
| `not` | Kondisi harus tidak terpenuhi | `tcpdump not tcp` |

### 3.5 Membaca File & Menghitung Paket

```
tcpdump -r traffic.pcap -c 5 -n
tcpdump -r traffic.pcap src host 192.168.124.1 -n | wc -l
```

Pipe ke `wc -l` untuk menghitung jumlah baris/paket hasil filter.

### 3.6 Ringkasan Switch Filtering

| Switch | Fungsi |
|---|---|
| `host IP/HOSTNAME` | Filter berdasarkan IP atau hostname |
| `src host IP` | Filter hanya dari sumber tertentu |
| `dst host IP` | Filter hanya ke tujuan tertentu |
| `port NUMBER` | Filter berdasarkan nomor port |
| `src port NUMBER` | Filter berdasarkan port sumber |
| `dst port NUMBER` | Filter berdasarkan port tujuan |
| `PROTOCOL` | Filter berdasarkan protokol |


---

## 4. Advanced Filtering

### 4.1 Filter by Packet Length

```
tcpdump greater LENGTH    # panjang >= LENGTH byte
tcpdump less LENGTH       # panjang <= LENGTH byte
```

Tidak menggunakan simbol `>=` atau `<=` — harus pakai keyword `greater` dan `less`.

### 4.2 Binary Operations

Dipakai untuk filtering tingkat lanjut berbasis bit pada header paket.

**& (AND)** — mengembalikan 1 hanya jika kedua bit bernilai 1:

| Input 1 | Input 2 | Hasil |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

**| (OR)** — mengembalikan 1 jika salah satu bit bernilai 1:

| Input 1 | Input 2 | Hasil |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

**! (NOT)** — membalik nilai bit:

| Input | Hasil |
|---|---|
| 0 | 1 |
| 1 | 0 |

### 4.3 Header Bytes — Sintaks proto[expr:size]

Tcpdump memungkinkan akses langsung ke byte tertentu dalam header protokol.

Sintaks: `proto[expr:size]`

- `proto` — nama protokol: `arp`, `ether`, `icmp`, `ip`, `ip6`, `tcp`, `udp`
- `expr` — posisi byte (offset), dihitung dari 0
- `size` — jumlah byte yang dibaca: 1, 2, atau 4. **Opsional, default = 1**

Contoh dari manual pcap-filter:

```
ether[0] & 1 != 0
```
Mengambil byte pertama Ethernet header, AND dengan 1 (0000 0001 binary). Hasilnya tidak sama dengan 0 berarti paket dikirim ke **multicast address**.

```
ip[0] & 0xf != 5
```
Mengambil byte pertama IP header, AND dengan 0xf (0000 1111 binary). Hasilnya tidak sama dengan 5 (0000 0101) berarti paket IP memiliki **options**.

### 4.4 Filter TCP Flags

Referensi ke field TCP flags menggunakan `tcp[tcpflags]`.

Flag yang tersedia:

| Flag | Nama Lengkap |
|---|---|
| `tcp-syn` | SYN — Synchronize (memulai koneksi) |
| `tcp-ack` | ACK — Acknowledge (konfirmasi penerimaan) |
| `tcp-fin` | FIN — Finish (mengakhiri koneksi) |
| `tcp-rst` | RST — Reset (menghentikan koneksi paksa) |
| `tcp-push` | Push (kirim data segera) |

Cara penggunaan:

```
tcpdump "tcp[tcpflags] == tcp-syn"
```
Hanya paket dengan flag SYN saja yang aktif (semua flag lain harus mati).

```
tcpdump "tcp[tcpflags] & tcp-syn != 0"
```
Paket yang minimal memiliki flag SYN aktif (flag lain boleh aktif juga).

```
tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"
```
Paket yang minimal memiliki SYN atau ACK aktif.

Perbedaan `==` vs `!= 0`:
- `==` → hanya flag itu yang boleh aktif, sisanya harus 0
- `!= 0` → minimal flag itu aktif, flag lain tidak dipermasalahkan


---

## 5. Displaying Packets

### 5.1 Opsi Tampilan Output

```
tcpdump -q
```
Quick output — menampilkan informasi singkat: timestamp, IP sumber, IP tujuan, protokol, dan ukuran data saja.

```
tcpdump -e
```
Menampilkan **link-level header**, yaitu MAC address pengirim dan penerima. Berguna untuk menganalisis protokol layer 2 seperti ARP dan DHCP, serta melacak sumber paket mencurigakan.

```
tcpdump -A
```
Menampilkan isi paket dalam format **ASCII**. Berguna hanya untuk traffic plain-text (HTTP lama, FTP, Telnet). Tidak berguna untuk traffic terenkripsi karena output akan berupa karakter acak.

```
tcpdump -xx
```
Menampilkan isi paket dalam format **hexadecimal**. Bekerja untuk semua jenis traffic terlepas dari enkripsi, karena menampilkan byte mentah.

```
tcpdump -X
```
Menampilkan isi paket dalam **hex dan ASCII secara berdampingan**. Gabungan terbaik dari `-xx` dan `-A`.

### 5.2 Cara Membaca Format Output Tcpdump

Format dasar satu baris output tcpdump:

```
TIMESTAMP IP SOURCE.PORT > DESTINATION.PORT: FLAGS, detail...
```

- IP address dan port dipisahkan oleh titik: `192.168.1.1.80` berarti IP `192.168.1.1` port `80`
- **Ephemeral port** (port sementara) adalah port acak bernilai besar yang dibuat otomatis oleh sisi client setiap membuka koneksi baru — nilainya berubah-ubah dan tidak tetap
- Port kecil dan tetap (80, 443, 53, 22) biasanya milik server/layanan; port besar dan acak biasanya milik client

### 5.3 Ringkasan Switch Display

| Switch | Fungsi |
|---|---|
| `-q` | Output ringkas: timestamp, IP, port, ukuran |
| `-e` | Tampilkan MAC address (link-level header) |
| `-A` | Tampilkan isi paket sebagai ASCII |
| `-xx` | Tampilkan isi paket sebagai hexadecimal |
| `-X` | Tampilkan hex dan ASCII berdampingan |


---

## 6. Contoh Command Kombinasi

```
tcpdump -i eth0 -c 50 -v
```
Tangkap 50 paket dari interface eth0, tampilkan dengan detail.

```
tcpdump -i wlo1 -w data.pcap
```
Tangkap dari interface WiFi, simpan ke file, berjalan sampai CTRL-C.

```
tcpdump -i any -nn
```
Tangkap dari semua interface, tampilkan IP dan port mentah tanpa resolusi.

```
tcpdump -i any tcp port 22
```
Tangkap traffic SSH dari semua interface.

```
tcpdump -i wlo1 udp port 123
```
Tangkap traffic NTP dari interface WiFi.

```
tcpdump -i eth0 host example.com and tcp port 443 -w https.pcap
```
Filter HTTPS traffic ke/dari example.com, simpan ke file.

```
tcpdump -r traffic.pcap arp -n
```
Baca file, tampilkan hanya paket ARP tanpa resolusi nama.

```
tcpdump -r traffic.pcap port 53 -n | wc -l
```
Hitung jumlah paket DNS dalam file.

```
tcpdump -r traffic.pcap "tcp[tcpflags] & tcp-rst != 0" -n | wc -l
```
Hitung paket TCP yang memiliki flag RST aktif.

```
tcpdump -r traffic.pcap greater 15000 -n
```
Tampilkan paket dengan ukuran >= 15000 byte.

```
tcpdump -r traffic.pcap -nnqe arp
```
Baca ARP dari file, tampilkan MAC address, ringkas, tanpa resolusi.


---

## 7. Glossary

**libpcap** — Library C/C++ yang menjadi fondasi tcpdump dan tools jaringan lainnya. Versi Windows-nya disebut winpcap.

**pcap** — Format file standar untuk menyimpan hasil packet capture. Ekstensi file: `.pcap`.

**Network interface** — Titik masuk/keluar data di jaringan, bisa berupa kartu Ethernet (eth0), WiFi (wlo1), atau loopback (lo).

**Loopback** — Interface virtual untuk komunikasi internal dalam satu mesin; alamatnya 127.0.0.1.

**ARP (Address Resolution Protocol)** — Protokol untuk mencari MAC address dari sebuah IP address di jaringan lokal.

**Three-way handshake** — Proses pembentukan koneksi TCP: SYN → SYN-ACK → ACK.

**Ephemeral port** — Port sementara bernilai besar yang dibuat otomatis oleh sisi client untuk setiap sesi koneksi baru.

**Multicast address** — Alamat khusus yang merepresentasikan sekelompok perangkat yang menerima data yang sama secara bersamaan.

**Octet** — Satu unit data sebesar 8 bit, sama dengan 1 byte.

**Hexadecimal** — Sistem bilangan basis 16 (0-9 dan A-F). Setiap oktet ditampilkan sebagai dua digit hex. Setiap digit hex merepresentasikan 4 bit.

**ASCII** — American Standard Code for Information Interchange. Standar pemetaan karakter teks ke nilai numerik.

**TCP flags** — Bit kontrol dalam header TCP yang menentukan jenis/status paket: SYN, ACK, FIN, RST, Push.

**Binary operation** — Operasi matematis yang bekerja pada level bit: AND (&), OR (|), NOT (!).

**proto[expr:size]** — Sintaks pcap-filter untuk mengakses byte tertentu dalam header protokol.

**Verbose output** — Mode tampilan yang menampilkan informasi lebih lengkap dari paket, seperti TTL, total length, dan options.

**DNS (Domain Name System)** — Sistem yang menerjemahkan nama domain ke alamat IP. Menggunakan port 53 (UDP dan TCP).

**NTP (Network Time Protocol)** — Protokol sinkronisasi waktu. Menggunakan UDP port 123.

**ICMP (Internet Control Message Protocol)** — Protokol untuk pesan kontrol jaringan, dipakai oleh ping dan traceroute.


---

## 8. Tools & Platform Rujukan

**Wireshark**
Tools GUI untuk analisis packet capture. Dapat membuka file `.pcap` yang dihasilkan tcpdump dan menampilkan isi paket secara visual dan terstruktur. Lebih ramah untuk analisis mendalam dibanding membaca hex/ASCII mentah di tcpdump.

**Tshark**
Versi command-line dari Wireshark. Disebutkan sebagai alternatif tcpdump untuk analisis lalu lintas jaringan.

**man pcap-filter**
Bukan platform eksternal, tapi referensi penting — halaman manual built-in yang mendokumentasikan seluruh sintaks filtering pcap-filter secara lengkap. Diakses dengan menjalankan `man pcap-filter` di terminal.


---

## 9. Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar

- tcpdump — tool CLI capture & analisis paket jaringan
- libpcap — library dasar tcpdump; winpcap = versi Windows
- .pcap — format file hasil capture
- Butuh root/sudo untuk capture live; tidak perlu untuk baca file

### Switch Basic

- `-i INTERFACE` — pilih interface (any = semua)
- `-w FILE` — simpan ke file .pcap
- `-r FILE` — baca dari file .pcap
- `-c COUNT` — batasi jumlah paket
- `-n` — jangan resolve IP
- `-nn` — jangan resolve IP dan port
- `-v / -vv / -vvv` — tambah detail output

### Filter

- `host IP` — filter by host (src+dst)
- `src host IP` — hanya dari sumber ini
- `dst host IP` — hanya ke tujuan ini
- `port NUMBER` — filter by port
- `src port / dst port NUMBER` — port spesifik arah
- `PROTOCOL` — filter by protokol (arp, icmp, tcp, udp, ip, ip6)
- dns bukan protokol tcpdump — pakai `port 53`
- `greater LENGTH` — paket >= LENGTH byte
- `less LENGTH` — paket <= LENGTH byte
- `and / or / not` — logical operator

### TCP Flags

- `tcp[tcpflags] == tcp-syn` — hanya SYN saja aktif
- `tcp[tcpflags] & tcp-syn != 0` — minimal SYN aktif
- `tcp[tcpflags] & (tcp-syn|tcp-ack) != 0` — minimal SYN atau ACK aktif
- Flag: tcp-syn, tcp-ack, tcp-fin, tcp-rst, tcp-push

### Header Bytes

- Sintaks: `proto[expr:size]`
- expr = offset byte (mulai 0); size = jumlah byte (default 1)
- `ether[0] & 1 != 0` — filter multicast address
- `ip[0] & 0xf != 5` — filter IP dengan options

### Binary Operations

- AND (&) — hasil 1 hanya jika keduanya 1
- OR (|) — hasil 1 jika salah satu 1
- NOT (!) — membalik nilai bit

### Display Options

- `-q` — output ringkas (timestamp, IP, port, size)
- `-e` — tampilkan MAC address
- `-A` — tampilkan isi paket sebagai ASCII
- `-xx` — tampilkan isi paket sebagai hex
- `-X` — tampilkan hex dan ASCII berdampingan

### Format Output

- Format: `TIMESTAMP IP SOURCE.PORT > DESTINATION.PORT: FLAGS`
- Titik memisahkan IP dan port: `192.168.1.1.80` = IP .1.1 port 80
- Port besar acak (>1024) = ephemeral port milik client
- Port kecil tetap (22, 53, 80, 443) = port layanan server

### Perintah Berguna

- `| wc -l` — hitung jumlah baris/paket
- `ip a s` — lihat daftar network interface
- `man pcap-filter` — referensi lengkap sintaks filter
