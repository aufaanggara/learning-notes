# Resume Materi — Nmap (TryHackMe)
Tanggal: 13 Mei 2026

---

## 1. Apa Itu Nmap

**Nmap** (Network Mapper) adalah network scanner open-source yang pertama kali dirilis tahun 1997. Nmap dirancang untuk menjawab dua pertanyaan utama dalam reconnaissance jaringan:

- Host mana yang sedang aktif di jaringan ini?
- Layanan apa yang berjalan di host-host tersebut?

Pendekatan manual — menggunakan `ping`, `arp-scan`, atau `telnet` satu per satu — terlalu lambat dan terbatas untuk jaringan berskala besar. Nmap mengotomatiskan proses ini dengan cara yang fleksibel dan dapat disesuaikan untuk berbagai skenario.

### 1.1 Hak Akses dan Dampaknya

Nmap berperilaku berbeda tergantung hak akses yang digunakan:

- **Dengan sudo/root** — akses penuh ke semua fitur; default scan adalah **SYN scan** (`-sS`)
- **Tanpa sudo (local user)** — fitur terbatas; default scan adalah **connect scan** (`-sT`)

Perbedaan ini terjadi karena beberapa teknik scan (seperti SYN scan) membutuhkan kemampuan membuat **raw packet**, yang hanya bisa dilakukan dengan hak akses root.

---

## 2. Cara Menentukan Target

Nmap mendukung beberapa format penulisan target:

| Format | Contoh | Keterangan |
|---|---|---|
| IP tunggal | `192.168.0.1` | Scan satu host |
| IP range | `192.168.0.1-10` | Scan dari .1 sampai .10 |
| Subnet | `192.168.0.1/24` | Setara dengan .0 sampai .255 |
| Hostname | `example.thm` | Scan lewat nama domain |

**List scan** (`-sL`) adalah opsi khusus yang hanya menampilkan daftar target yang akan dipindai tanpa benar-benar mengirim paket apapun. Berguna untuk verifikasi target sebelum scan sesungguhnya dijalankan.

```
nmap -sL 192.168.0.1/24
```

---

## 3. Host Discovery — Siapa yang Online?

### 3.1 Ping Scan (`-sn`)

**Ping scan** mencari host yang aktif tanpa melakukan port scanning sama sekali. Cocok dipakai ketika tujuannya hanya memetakan perangkat yang hidup di jaringan tanpa menimbulkan banyak noise.

```
nmap -sn 192.168.0.1/24
```

### 3.2 Jaringan Lokal vs Remote

Cara Nmap melakukan host discovery berbeda tergantung posisi target:

**Jaringan lokal** (Ethernet/WiFi — terhubung langsung):
- Nmap mengirim **ARP request** ke setiap host
- Jika dibalas → host ditandai "Host is up"
- Bonus: MAC address terlihat → bisa identifikasi vendor kartu jaringan

**Jaringan remote** (ada satu atau lebih router di antaranya):
- ARP tidak bisa melewati router, jadi Nmap menggunakan kombinasi probe:
  - 2x ICMP echo (ping)
  - 2x ICMP timestamp request
  - 2x TCP SYN ke port 443
  - 2x TCP ACK ke port 80
- Jika tidak ada satupun yang dibalas → host dianggap down
- MAC address tidak terlihat karena beda segmen jaringan

### 3.3 Force Scan (`-Pn`)

Jika host tidak merespons selama fase discovery (misalnya firewall memblokir ICMP), Nmap akan menganggapnya down dan melewatinya. Opsi **`-Pn`** memaksa Nmap untuk tetap melakukan port scan ke semua host meski tidak ada respons di fase discovery.

---

## 4. Port Scanning — Siapa yang Listening?

TCP dan UDP masing-masing memiliki **65.535 port**. Port scanning adalah proses menentukan port mana yang memiliki layanan yang aktif mendengarkan koneksi.

**Network service** = proses yang listen di port TCP atau UDP tertentu.
Contoh: web server (port 80/443), DNS (port 53), SSH (port 22).

### 4.1 TCP Connect Scan (`-sT`)

Menyelesaikan **three-way handshake** secara penuh: SYN → SYN-ACK → ACK, lalu Nmap langsung memutus koneksi dengan RST-ACK.

```
nmap -sT 192.168.0.1
```

- Port terbuka: SYN → SYN-ACK → ACK → RST-ACK (4 paket)
- Port tertutup: SYN → RST-ACK (2 paket, langsung ditolak target)
- **Kelebihan:** Bisa dijalankan tanpa sudo
- **Kekurangan:** Meninggalkan log di target karena koneksi benar-benar terbentuk

### 4.2 TCP SYN Scan / Stealth Scan (`-sS`)

Hanya mengirim paket **SYN** tanpa menyelesaikan handshake. Jika target membalas SYN-ACK (port terbuka), Nmap langsung kirim RST untuk memutus koneksi paksa.

```
nmap -sS 192.168.0.1
```

- Port terbuka: SYN → SYN-ACK → RST (3 paket, handshake tidak selesai)
- Port tertutup: SYN → RST-ACK (sama seperti connect scan)
- **Kelebihan:** Lebih sedikit log karena koneksi tidak pernah terbentuk secara resmi
- **Kekurangan:** Membutuhkan sudo/root karena perlu membuat raw packet
- **Default** jika dijalankan dengan sudo

### 4.3 UDP Scan (`-sU`)

Banyak layanan penting menggunakan UDP: DNS, DHCP, NTP, SNMP, VoIP. UDP tidak memerlukan koneksi (connectionless), sehingga mekanismenya berbeda dari TCP.

```
nmap -sU 192.168.0.1
```

- Port tertutup: target membalas ICMP "Port unreachable"
- Port terbuka: biasanya tidak ada respons (UDP tidak wajib konfirmasi)

### 4.4 Membatasi Port yang Di-scan

Secara default Nmap scan **1.000 port paling umum**. Opsi tambahan untuk mengatur ini:

```
nmap -F 192.168.0.1          # Fast mode: 100 port paling umum
nmap -p10-1024 192.168.0.1   # Scan port 10 sampai 1024
nmap -p-25 192.168.0.1       # Scan port 1 sampai 25
nmap -p- 192.168.0.1         # Scan SEMUA port (setara -p1-65535)
nmap -p1-1023 192.168.0.1    # Scan well-known ports saja
```

**Well-known ports** adalah port 1–1023, digunakan oleh layanan standar yang umum.

---

## 5. Version & OS Detection

### 5.1 OS Detection (`-O`)

Nmap menganalisis berbagai indikator dari respons target untuk menebak sistem operasi yang digunakan. Hasilnya berupa perkiraan — tidak 100% akurat, tetapi biasanya sangat mendekati.

```
nmap -sS -O 192.168.0.1
```

Output contoh: `Running: Linux 4.X|5.X` dengan detail `Linux 4.15 - 5.8`

### 5.2 Service & Version Detection (`-sV`)

Menambahkan kolom **VERSION** pada output — menampilkan nama dan versi spesifik dari layanan yang berjalan di setiap port terbuka.

```
nmap -sS -sV 192.168.0.1
```

Output contoh: `22/tcp open ssh OpenSSH 8.9p1 Ubuntu 3ubuntu0.10`

Informasi versi ini sangat berguna karena versi spesifik suatu layanan bisa dicocokkan dengan database vulnerability yang diketahui.

### 5.3 All-in-One (`-A`)

Menggabungkan OS detection, version detection, traceroute, dan fitur tambahan lainnya dalam satu flag.

```
nmap -A 192.168.0.1
```

---

## 6. Timing — Seberapa Cepat Scan Berjalan?

Scan yang terlalu cepat dapat memicu **IDS/IPS** (Intrusion Detection/Prevention System). Nmap menyediakan enam **timing template** untuk mengontrol kecepatan:

| Template | Nama | Estimasi (100 port) |
|---|---|---|
| `-T0` | paranoid | ~9.8 jam |
| `-T1` | sneaky | ~27.53 menit |
| `-T2` | polite | ~40.56 detik |
| `-T3` | normal (default) | ~0.15 detik |
| `-T4` | aggressive | ~0.13 detik |
| `-T5` | insane | secepat mungkin |

Template bisa dipanggil dengan angka atau nama: `-T0` sama dengan `-T paranoid`.

Catatan: estimasi waktu di atas bergantung pada kondisi jaringan dan sistem target — hasilnya bisa berbeda di environment berbeda.

### 6.1 Kontrol Parallelism

**Parallelism** mengatur berapa banyak probe yang dikirim secara bersamaan:

```
nmap --min-parallelism 10 --max-parallelism 50 192.168.0.1
```

- Jaringan buruk (banyak packet loss) → Nmap otomatis turunkan ke 1
- Jaringan stabil → bisa mencapai ratusan probe paralel
- Default: Nmap atur otomatis berdasarkan kondisi jaringan

### 6.2 Kontrol Rate

**Rate** mengatur jumlah paket yang dikirim per detik — berlaku untuk keseluruhan scan, bukan per host:

```
nmap --min-rate 100 --max-rate 500 192.168.0.1
```

### 6.3 Host Timeout

Mengatur batas waktu maksimum yang bersedia ditunggu untuk satu host. Cocok dipakai ketika ada host dengan koneksi sangat lambat yang bisa menghambat seluruh proses scan:

```
nmap --host-timeout 30s 192.168.0.1
```

---

## 7. Output & Verbosity

### 7.1 Verbose Mode

Secara default, Nmap hanya menampilkan hasil akhir setelah scan selesai. **Verbose mode** menampilkan informasi real-time selama scan berjalan — termasuk tahapan yang sedang dijalankan (ARP scan, DNS resolution, SYN scan, dll).

```
nmap -sS 192.168.0.1 -v    # Verbose level 1
nmap -sS 192.168.0.1 -vv   # Verbose level 2
nmap -sS 192.168.0.1 -v4   # Verbose level 4
```

Level verbosity juga bisa dinaikkan dengan menekan tombol `v` saat scan sedang berjalan.

### 7.2 Debugging Mode

Jika verbose tidak cukup, **debugging mode** memberikan output yang jauh lebih detail. Level maksimum adalah `-d9` — siapkan untuk ribuan baris output.

```
nmap -d 192.168.0.1    # Debugging level 1
nmap -d9 192.168.0.1   # Debugging level maksimum
```

### 7.3 Menyimpan Hasil Scan

Nmap mendukung beberapa format output untuk menyimpan hasil scan:

```
nmap -sS 192.168.0.1 -oN hasil.nmap    # Normal output (human-readable)
nmap -sS 192.168.0.1 -oX hasil.xml     # XML output (machine-readable)
nmap -sS 192.168.0.1 -oG hasil.gnmap   # Grepable output (untuk grep/awk)
nmap -sS 192.168.0.1 -oA hasil         # Semua format sekaligus
```

Opsi `-oA` otomatis membuat tiga file sekaligus: `hasil.nmap`, `hasil.xml`, dan `hasil.gnmap`.

---

## 8. Cheat Sheet Semua Switch

### Umum & Target

| Switch | Fungsi |
|---|---|
| `-sL` | List scan — tampilkan target tanpa scan |

### Host Discovery

| Switch | Fungsi |
|---|---|
| `-sn` | Ping scan — host discovery only, tanpa port scan |
| `-Pn` | Paksa scan host yang tampak down (skip host discovery) |

### Port Scanning

| Switch | Fungsi |
|---|---|
| `-sT` | TCP connect scan — full three-way handshake |
| `-sS` | TCP SYN/stealth scan — hanya langkah pertama handshake |
| `-sU` | UDP scan |
| `-F` | Fast mode — 100 port paling umum |
| `-p[range]` | Tentukan range port; `-p-` untuk semua port |

### Service & OS Detection

| Switch | Fungsi |
|---|---|
| `-O` | OS detection |
| `-sV` | Service version detection |
| `-A` | OS + version + traceroute + lainnya |

### Timing

| Switch | Fungsi |
|---|---|
| `-T<0-5>` | Template timing: paranoid/sneaky/polite/normal/aggressive/insane |
| `--min-parallelism <n>` | Minimal n probe aktif bersamaan |
| `--max-parallelism <n>` | Maksimal n probe aktif bersamaan |
| `--min-rate <n>` | Minimal n paket per detik |
| `--max-rate <n>` | Maksimal n paket per detik |
| `--host-timeout <t>` | Batas waktu tunggu per host |

### Output

| Switch | Fungsi |
|---|---|
| `-v / -vv / -v4` | Verbosity level |
| `-d / -d9` | Debugging level |
| `-oN <file>` | Simpan output normal |
| `-oX <file>` | Simpan output XML |
| `-oG <file>` | Simpan output grepable |
| `-oA <nama>` | Simpan semua format sekaligus |

---

## 9. Glossary

**ARP (Address Resolution Protocol)** — protokol untuk mengetahui MAC address dari IP address di jaringan lokal yang sama; tidak bisa melewati router.

**ICMP (Internet Control Message Protocol)** — protokol untuk pesan diagnostik dan error di jaringan, termasuk yang digunakan oleh perintah ping.

**Host Discovery** — fase awal scan untuk menentukan host mana yang aktif sebelum melakukan port scan.

**Port Scanning** — proses identifikasi port TCP/UDP mana yang terbuka dan memiliki layanan yang aktif.

**Three-Way Handshake** — proses pembukaan koneksi TCP: SYN (klien) → SYN-ACK (server) → ACK (klien). Koneksi baru resmi terbentuk setelah ketiga langkah ini selesai.

**Raw Packet** — paket jaringan yang dibuat secara manual pada level rendah, memungkinkan kontrol penuh atas setiap field header; hanya bisa dibuat dengan hak akses root.

**Well-Known Ports** — port nomor 1–1023 yang dialokasikan untuk layanan standar dan umum.

**Latency** — waktu tunda antara pengiriman paket dan penerimaan respons dari host target.

**IDS/IPS** — Intrusion Detection System / Intrusion Prevention System; sistem yang memantau dan/atau memblokir aktivitas jaringan yang mencurigakan.

**Grepable Output** — format teks terstruktur yang didesain agar mudah difilter dan diekstrak menggunakan tools seperti `grep` dan `awk`.

**Parallelism** — jumlah probe yang dikirim secara bersamaan dalam satu waktu.

**Rate** — kecepatan pengiriman paket, diukur dalam jumlah paket per detik.

**Network Service** — proses atau program yang berjalan di sebuah host dan mendengarkan koneksi masuk di port tertentu.

**Stealth Scan** — sebutan untuk SYN scan karena tidak menyelesaikan handshake sehingga menghasilkan lebih sedikit log di sistem target.

---

## 10. Tools & Platform Rujukan

**Nmap** — tool utama yang dibahas dalam room ini; network scanner open-source untuk host discovery, port scanning, version detection, dan OS detection.
URL: https://nmap.org

**Wireshark** — network packet analyzer; digunakan dalam room ini untuk memvisualisasikan traffic yang dihasilkan Nmap dan menjelaskan mekanisme setiap jenis scan.
URL: https://www.wireshark.org

---

## 11. Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar

- Nmap — network scanner open-source, rilis 1997
- Fungsi utama: host discovery + port/service scanning
- Sudo/root — default -sS (SYN scan), fitur penuh
- Tanpa sudo — default -sT (connect scan), fitur terbatas
- Raw packet — butuh root, dipakai SYN scan & fitur lanjutan

### Target

- IP tunggal — 192.168.0.1
- Range — 192.168.0.1-10
- Subnet — 192.168.0.1/24
- Hostname — example.thm
- -sL — list target tanpa scan

### Host Discovery

- -sn — ping scan, cari host aktif tanpa port scan
- Lokal (LAN) — pakai ARP request, dapat MAC address
- Remote (beda router) — pakai ICMP + TCP SYN/ACK combo
- -Pn — paksa scan meski host tidak balas discovery

### Port Scanning

- TCP punya 65.535 port, UDP juga 65.535 port
- -sT — connect scan, full handshake, bisa tanpa sudo, ada log
- -sS — SYN/stealth scan, handshake tidak selesai, butuh sudo, minim log
- -sU — UDP scan, port tertutup → ICMP unreachable
- -F — fast mode, 100 port umum
- -p[range] — tentukan range; -p- = semua port
- Well-known ports — port 1-1023

### Version & OS Detection

- -O — OS detection, hasil perkiraan tidak 100% akurat
- -sV — tambah kolom VERSION di output, tahu versi spesifik layanan
- -A — gabungan OS detection + version + traceroute + lainnya

### Timing

- -T0 paranoid — 9.8 jam (paling lambat, paling senyap)
- -T1 sneaky — 27 menit
- -T2 polite — 40 detik
- -T3 normal — 0.15 detik (default)
- -T4 aggressive — 0.13 detik
- -T5 insane — secepat mungkin
- --min/max-parallelism — jumlah probe bersamaan
- --min/max-rate — kecepatan paket per detik
- --host-timeout — batas waktu per host

### Output & Verbosity

- -v / -vv / -v4 — verbosity level (info real-time)
- -d / -d9 — debugging level (sangat detail)
- -oN — simpan normal (.nmap)
- -oX — simpan XML (.xml)
- -oG — simpan grepable (.gnmap)
- -oA — simpan semua format sekaligus

### Istilah Kunci

- Three-way handshake — SYN → SYN-ACK → ACK
- ARP — cari MAC address di LAN, tidak bisa lewat router
- ICMP — protokol ping & pesan error jaringan
- IDS/IPS — sistem deteksi/blokir intrusi
- Latency — waktu tunda respons host
- Network service — program yang listen di port tertentu
