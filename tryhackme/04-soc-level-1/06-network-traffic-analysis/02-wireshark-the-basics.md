# Wireshark: The Basics — Resume Materi
**TryHackMe Room** | 23 Oktober 2026

---

## 1. Pengenalan Wireshark

**Wireshark** adalah tool analisis paket jaringan yang bersifat open-source dan cross-platform. Fungsinya adalah menangkap (sniffing) traffic jaringan secara langsung dan memeriksa file rekaman traffic (PCAP).

Hal yang penting untuk dipahami sejak awal: Wireshark **bukan IDS (Intrusion Detection System)**. Wireshark hanya membaca dan menampilkan paket — tidak memodifikasi, tidak memblokir, dan tidak mendeteksi ancaman secara otomatis. Kualitas analisis sepenuhnya bergantung pada kemampuan analis yang memakainya.

**Use cases utama:**

- Mendeteksi dan troubleshoot masalah jaringan (bottleneck, congestion, load failure)
- Mendeteksi anomali keamanan (rogue host, port tidak wajar, traffic mencurigakan)
- Investigasi detail protokol (response code, payload, header)

---

## 2. Antarmuka (GUI) Wireshark

Wireshark membuka satu halaman all-in-one dengan lima bagian utama:

| Bagian | Fungsi |
|---|---|
| **Toolbar** | Menu dan shortcut untuk sniffing, filter, sort, export, merge |
| **Display Filter Bar** | Kolom untuk mengetik query filter |
| **Recent Files** | Daftar file PCAP yang baru dibuka (double-click untuk membuka) |
| **Capture Filter & Interfaces** | Pilih interface sniffing (eth0, lo, ens33, dll) dan capture filter |
| **Status Bar** | Status tool, profil aktif, jumlah paket |

### 2.1 Tiga Panel Saat PCAP Dibuka

Setelah file PCAP dimuat, muncul tiga panel tambahan:

**Packet List Pane** — Ringkasan semua paket: nomor, waktu, source, destination, protokol, info singkat. Klik satu paket untuk memilihnya, detail akan muncul di panel bawah.

**Packet Details Pane** — Breakdown protokol layer per layer dari paket yang dipilih. Setiap baris bisa di-expand untuk melihat field lebih dalam.

**Packet Bytes Pane** — Representasi hex dan ASCII dari paket yang dipilih. Ketika satu field di Packet Details diklik, bagian yang bersesuaian di sini akan ikut tersorot.

---

## 3. Traffic Sniffing

Tiga tombol di toolbar untuk mengontrol proses sniffing:

- **Tombol Biru (shark)** — Mulai merekam (start)
- **Tombol Merah** — Berhenti merekam (stop)
- **Tombol Hijau** — Mulai ulang proses sniffing (restart)

Status bar akan menampilkan interface yang sedang dipakai dan jumlah paket yang sudah tertangkap selama proses berlangsung.

---

## 4. Manajemen File PCAP

### 4.1 Membuka File

File PCAP bisa dimuat dengan tiga cara: lewat menu **File → Open**, drag-and-drop ke jendela Wireshark, atau double-click pada nama file di bagian Recent Files.

### 4.2 Merge File

Wireshark bisa menggabungkan dua file PCAP menjadi satu lewat **File → Merge**. Setelah memilih file kedua, Wireshark menampilkan total paket gabungan. Klik "Open" untuk menggabungkan. File hasil merge harus disimpan terlebih dahulu sebelum bisa digunakan.

### 4.3 Melihat Detail File

Akses lewat **Statistics → Capture File Properties** atau klik ikon PCAP di pojok kiri bawah. Informasi yang tersedia:

- Hash file (MD5, SHA256) — sidik jari unik file
- Waktu mulai dan selesai capture
- Interface yang digunakan
- Komentar file (capture file comments)
- Total jumlah paket

---

## 5. Packet Dissection (Bedah Paket)

Packet dissection adalah proses membedah isi paket dengan mendekode setiap layer protokol. Wireshark mengikuti model **OSI** untuk menampilkan ini.

Sebuah paket HTTP umumnya memiliki 7 layer yang bisa dilihat di Packet Details:

### Layer 1 — Frame (Physical Layer)
Informasi fisik paket: ukuran frame, waktu tiba (arrival time), nomor frame, interface yang menangkap, daftar protokol dalam frame, dan coloring rule yang berlaku.

### Layer 2 — Ethernet II (Data Link Layer)
MAC address source dan destination. Menampilkan tipe Ethernet (IPv4/IPv6).

### Layer 3 — Internet Protocol (Network Layer)
IP address source dan destination, versi IP, TTL, flags, checksum, dan protocol yang dibawa (TCP/UDP).

### Layer 4 — TCP/UDP (Transport Layer)
Port source dan destination, sequence number, acknowledgment number, flags (SYN/ACK/PSH/FIN), window size, checksum.

### Protocol Errors (Lanjutan Layer 4)
Menampilkan segmen-segmen TCP yang perlu di-reassemble. Contoh: data besar yang dipecah menjadi dua paket akan digabungkan kembali di sini menjadi satu data utuh.

### Layer 5 — Application Protocol (Application Layer)
Detail spesifik protokol aplikasi yang digunakan: HTTP header, FTP command, SMB request, dll. Untuk HTTP, menampilkan status code, server, content-type, tanggal, E-Tag, dan URI request.

### Application Data
Isi konten aplikasi yang dikirim: HTML mentah, XML, teks biasa, atau data lainnya.

---

## 6. TTL (Time To Live)

**TTL** adalah nilai numerik dalam header IP yang menentukan berapa banyak "lompatan" (hop) sebuah paket boleh dilalui sebelum dibuang oleh router. Setiap kali paket melewati satu router, nilai TTL dikurangi 1. Ketika TTL mencapai 0, paket dibuang dan tidak diteruskan.

Fungsi utama TTL adalah mencegah paket nyasar berputar selamanya di jaringan.

**Nilai TTL default per sistem operasi:**

| OS | TTL Awal |
|---|---|
| Windows | 128 |
| Linux/Unix | 64 |
| Cisco/Network Device | 255 |

Dari nilai TTL yang tersisa, analis bisa memperkirakan OS pengirim dan jumlah hop yang sudah dilewati. Contoh: TTL tersisa 47, jika OS-nya Linux (64), maka 64 − 47 = **17 hop** sudah dilewati.

Catatan: nilai TTL bisa dimodifikasi secara manual, sehingga hasil analisis ini hanya sebagai petunjuk awal, bukan bukti pasti.

---

## 7. HTTP Cache dan E-Tag

**E-Tag (Entity Tag)** adalah nilai unik yang diberikan server untuk mengidentifikasi versi spesifik dari sebuah konten. Browser menyimpan nilai ini dan mengirimkannya kembali di request berikutnya untuk mengecek apakah konten sudah berubah.

**Last-Modified** adalah tanggal terakhir konten diubah di server. Browser menggunakannya bersama E-Tag untuk validasi cache.

Cara membedakan fresh download vs cache dari Wireshark:

- **HTTP 200 OK** + Content-Length > 0 → **fresh download** (server kirim konten baru)
- **HTTP 304 Not Modified** → **cache** (server konfirmasi konten belum berubah, pakai yang lama)

---

## 8. Navigasi Paket

### 8.1 Go to Packet
Loncat langsung ke nomor paket tertentu. Akses lewat toolbar, menu **Go → Go to Packet**, atau shortcut **Ctrl+G**.

### 8.2 Find Packet
Cari paket berdasarkan konten lewat **Edit → Find Packet** atau **Ctrl+F**.

Terdapat dua pengaturan penting saat menggunakan Find:

**Tipe input** (pilih salah satu):
- Display Filter — pakai syntax filter Wireshark
- Hex — cari nilai hexadecimal
- String — cari teks (paling umum digunakan)
- Regex — cari dengan pola ekspresi reguler

**Search field** (tentukan di mana dicari):
- Packet List — mencari di kolom ringkasan
- Packet Details — mencari di isi detail protokol
- Packet Bytes — mencari di data hex/ASCII

Penting: jika informasi yang dicari ada di Packet Details, pencarian harus dilakukan di Packet Details. Mencari di tempat yang salah tidak akan menemukan hasilnya meskipun data tersebut ada.

### 8.3 Mark Packets
Tandai paket penting untuk referensi cepat. Akses lewat **Edit** atau right-click → Mark/Unmark. Paket yang di-mark ditampilkan dengan warna **hitam** terlepas dari warna aslinya. Marking bersifat **tidak permanen** — hilang setelah file capture ditutup.

### 8.4 Packet Comments
Tambahkan catatan teks ke paket tertentu lewat right-click → **Packet Comments → Add New Comment**. Berbeda dengan marking, komentar **tersimpan permanen** di dalam file capture sampai dihapus secara manual. Berguna untuk meninggalkan catatan bagi analis lain.

### 8.5 Export Packets
Export subset paket tertentu lewat **File → Export Specified Packets**. Opsi range yang tersedia:

- All packets — semua paket
- Selected packets only — hanya paket yang sedang dipilih
- Marked packets only — hanya paket yang sudah di-mark
- First to last marked — dari paket marked pertama sampai terakhir
- Range — rentang nomor paket tertentu
- Remove ignored packets — hilangkan paket yang diabaikan
- Include depended upon packets — sertakan paket yang bergantung pada paket yang di-export

### 8.6 Export Objects (Files)
Ekstrak file yang pernah ditransfer lewat jaringan menggunakan **File → Export Objects**. Tersedia untuk protokol: **HTTP, DICOM, IMF, SMB, TFTP**. Berguna untuk mengambil kembali dokumen, gambar, atau file lain yang pernah dikirim dalam traffic yang direkam.

---

## 9. Pewarnaan Paket (Packet Colouring)

Wireshark mewarnai paket otomatis berdasarkan protokol dan kondisi tertentu untuk mempermudah identifikasi visual.

**Dua jenis aturan warna:**

- **Temporary** — berlaku hanya untuk sesi program saat ini, hilang setelah Wireshark ditutup
- **Permanent** — tersimpan di file profil, berlaku di sesi berikutnya

**Cara mengakses:**

- Buat aturan permanen: **View → Coloring Rules**
- Aktif/nonaktifkan semua warna: **View → Colorize Packet List**
- Warna sementara per percakapan: **View → Colorize Conversation**
- Reset warna sementara: **View → Colorize Conversation → Reset Colourisation**

---

## 10. Format Waktu (Time Display Format)

Secara default, Wireshark menampilkan waktu dalam **"Seconds Since Beginning of Capture"** — yaitu selisih detik dari paket pertama, bukan waktu nyata.

Untuk analisis yang perlu dikorelasikan dengan log sistem lain, disarankan beralih ke **UTC Time Display Format** yang menampilkan waktu sesungguhnya.

Akses lewat **View → Time Display Format**.

---

## 11. Expert Info

Wireshark secara otomatis mendeteksi kondisi protokol yang tidak normal dan mengkategorikannya. Akses lewat **Analyse → Expert Information** atau klik ikon lingkaran di pojok kiri bawah status bar.

**Tingkat severity:**

| Severity | Warna | Keterangan |
|---|---|---|
| Chat | Biru | Informasi alur kerja normal |
| Note | Cyan | Event penting, error code aplikasi |
| Warn | Kuning | Peringatan, error code tidak wajar |
| Error | Merah | Masalah serius, paket malformed |

**Group informasi yang sering muncul:**

| Group | Info |
|---|---|
| Checksum | Checksum errors |
| Comment | Packet comment terdeteksi |
| Deprecated | Penggunaan protokol yang sudah usang |
| Malformed | Paket dengan struktur yang rusak |

Dialogue box Expert Info menampilkan: nomor paket, ringkasan, group protokol, dan total kemunculan.

---

## 12. Packet Filtering

Wireshark memiliki dua jenis filter:

**Capture Filter** — Diterapkan sebelum/saat merekam. Hanya menangkap paket yang sesuai filter. Paket yang tidak sesuai tidak disimpan sama sekali.

**Display Filter** — Diterapkan setelah data sudah direkam/dimuat. Menyembunyikan paket yang tidak sesuai, tapi tidak menghapusnya dari file.

**Golden Rule:** "If you can click on it, you can filter and copy it" — hampir semua nilai yang terlihat di Wireshark bisa langsung dijadikan filter dengan right-click.

### 12.1 Apply as Filter
Cara paling dasar. Klik kanan pada field atau nilai → **Apply as Filter**. Filter langsung diterapkan seketika dan packet list diperbarui. Jumlah paket yang ditampilkan berubah dan terlihat di status bar.

### 12.2 Prepare as Filter
Mirip Apply as Filter, tapi filter **belum** langsung diterapkan. Query ditambahkan ke Display Filter Bar dan menunggu eksekusi manual (tekan Enter). Berguna untuk menggabungkan beberapa kondisi dengan **AND/OR** sebelum dijalankan.

### 12.3 Conversation Filter
Menampilkan semua paket yang terlibat dalam satu percakapan tertentu (berdasarkan kombinasi IP address dan port). Paket lain disembunyikan. Akses lewat right-click → **Conversation Filter** atau **Analyse → Conversation Filter**.

### 12.4 Colorise Conversation
Mirip Conversation Filter, tapi paket terkait hanya **diwarnai** — tidak disembunyikan. Paket lain tetap terlihat. Berguna untuk melacak satu alur percakapan tanpa kehilangan konteks keseluruhan traffic.

### 12.5 Apply as Column
Menambahkan field tertentu sebagai kolom baru di Packet List Pane. Akses lewat right-click → **Apply as Column** atau **Analyse → Apply as Column**. Berguna untuk melihat nilai field tertentu di semua paket sekaligus tanpa harus membuka detail satu per satu.

### 12.6 Follow Stream
Merekonstruksi percakapan lengkap level aplikasi dalam satu tampilan bersih. Akses lewat right-click → **Follow** atau **Analyse → Follow TCP/UDP/HTTP Stream**.

Di jendela Follow Stream:
- Teks **merah** = data dari client
- Teks **biru** = data dari server

Setelah Follow Stream digunakan, Wireshark otomatis menerapkan filter untuk menampilkan hanya stream tersebut. Untuk melihat semua paket kembali, klik tombol **X** di Display Filter Bar.

**Pilihan stream yang tersedia:**

- **TCP Stream** — Menampilkan seluruh data mentah TCP termasuk header, lebih lengkap tapi lebih berantakan
- **HTTP Stream** — Khusus HTTP, sudah diparse dan lebih bersih untuk dibaca
- **UDP Stream** — Untuk traffic UDP (DNS, game, dll)
- **TLS Stream** — Untuk koneksi terenkripsi (umumnya tidak terbaca tanpa key dekripsi)

---

## 13. Display Filter Queries

### 13.1 Filter by Protocol Name

Ketik nama protokol langsung di Display Filter Bar lalu tekan Enter:

```
http
dns
tcp
udp
arp
dhcp
ftp
smtp
pop
imap
```

### 13.2 Filter by Port Number

```
tcp.port == 80
udp.port == 53
tcp.port == 443
```

### 13.3 Filter by IP Address

```
ip.addr == 192.168.1.2       (semua paket yang melibatkan IP ini)
ip.src == 192.168.1.2        (hanya paket DARI IP ini)
ip.dst == 192.168.1.2        (hanya paket KE IP ini)
```

---

## 14. Hashing File di Terminal

Untuk mendapatkan hash value dari sebuah file:

```bash
md5sum namafile
sha256sum namafile
sha1sum namafile
```

Output berupa string hash diikuti nama file. MD5 menghasilkan 32 karakter, SHA256 menghasilkan 64 karakter. Nilai hash ini bersifat unik per file — perubahan sekecil apapun pada isi file akan menghasilkan hash yang sama sekali berbeda.

---

## Glossary

**PCAP** — Packet Capture; format file standar untuk menyimpan rekaman traffic jaringan.

**Sniffing** — Proses menangkap dan merekam traffic jaringan yang melintas di sebuah interface.

**Packet Dissection** — Proses membedah isi paket dengan mendekode setiap layer protokol yang ada di dalamnya.

**Reassemble** — Proses menyatukan kembali potongan-potongan paket TCP yang dipecah saat pengiriman menjadi satu data utuh.

**Hop** — Satu lompatan paket melewati satu router dalam perjalanannya menuju tujuan.

**TTL (Time To Live)** — Nilai numerik dalam header IP yang menentukan batas maksimum hop sebelum paket dibuang.

**Payload** — Bagian isi/konten dari sebuah paket, di luar header protokol.

**Stream** — Rangkaian paket yang membentuk satu percakapan atau koneksi yang utuh antara dua endpoint.

**Interface** — Titik koneksi antara komputer dan jaringan (contoh: eth0, lo, ens33).

**Capture File** — File PCAP yang berisi rekaman traffic jaringan.

**Malformed Packet** — Paket yang struktur atau formatnya rusak, tidak sesuai standar protokol.

**IDS (Intrusion Detection System)** — Sistem yang mendeteksi ancaman secara otomatis; Wireshark bukan IDS.

**E-Tag (Entity Tag)** — Nilai unik yang diberikan server untuk mengidentifikasi versi spesifik dari sebuah konten, digunakan untuk validasi cache HTTP.

**ASCII Art** — Gambar atau teks yang dibuat dari karakter teks biasa, memanfaatkan susunan spasial karakter.

**Monospace Font** — Font di mana setiap karakter memiliki lebar yang sama; diperlukan agar ASCII art tampil dengan benar.

**Capture File Comments** — Catatan teks yang disematkan ke dalam file PCAP secara keseluruhan (bukan per paket), bisa dilihat di Capture File Properties.

---

## Tools & Platform Rujukan

**Wireshark** — Tool utama yang dibahas di room ini untuk analisis paket jaringan.
URL: https://www.wireshark.org

**Wireshark Official Protocol Reference** — Referensi lengkap semua protokol yang didukung Wireshark untuk dissection dan filter syntax.
URL: https://www.wireshark.org/docs/dfref/

**Wireshark Official Documentation (Expert Info)** — Dokumentasi lengkap tentang entry Expert Information.
URL: https://www.wireshark.org/docs/

**DailyMed** — Disebutkan di konteks umum verifikasi informasi; platform database label obat resmi dari US National Library of Medicine.
URL: https://dailymed.nlm.nih.gov

---

## Catatan Ringkas untuk Ditulis Tangan

### Wireshark Dasar

- Wireshark — packet analyser, open-source, cross-platform
- BUKAN IDS — hanya baca paket, tidak modifikasi
- PCAP — format file rekaman traffic jaringan
- Sniffing — menangkap traffic jaringan

### 5 Bagian GUI

- Toolbar — menu & shortcut utama
- Display Filter Bar — kolom query filter
- Recent Files — file PCAP baru dibuka
- Capture Filter & Interfaces — pilih interface (eth0, lo, dll)
- Status Bar — status, profil, jumlah paket

### 3 Panel Saat PCAP Dibuka

- Packet List Pane — ringkasan semua paket
- Packet Details Pane — breakdown layer protokol
- Packet Bytes Pane — hex + ASCII, sorot ikut klik di Details

### Tombol Sniffing

- Biru — start
- Merah — stop
- Hijau — restart

### Manajemen File

- Load: File → Open / drag-drop / double-click Recent Files
- Merge: File → Merge (harus save hasilnya)
- Detail file: Statistics → Capture File Properties
- Isi Properties: hash MD5/SHA256, waktu capture, interface, komentar, total paket

### 7 Layer Packet Dissection

- Layer 1 (Frame) — ukuran, waktu, interface, protokol dalam frame
- Layer 2 (Ethernet/MAC) — MAC address src & dst
- Layer 3 (IP) — IP address src & dst, TTL, flags
- Layer 4 (TCP/UDP) — port src & dst, seq/ack, flags, window size
- Protocol Errors — TCP segments yang di-reassemble
- Layer 5 (App Protocol) — HTTP header, FTP, SMB
- App Data — konten: HTML, XML, teks mentah

### TTL

- Tiap lewat 1 router → TTL -1; TTL = 0 → paket dibuang
- Windows default 128; Linux 64; Cisco 255
- Contoh: TTL sisa 47, Linux (64) → 64-47 = 17 hop sudah dilalui
- Bisa dimodifikasi manual → hanya petunjuk awal

### HTTP Cache

- E-Tag — sidik jari versi konten
- Last-Modified — tanggal konten terakhir diubah
- 200 OK → fresh download
- 304 Not Modified → cache (pakai yang lama)

### Navigasi Paket

- Go to Packet — Ctrl+G → loncat ke nomor paket
- Find Packet — Ctrl+F → cari by: Display Filter / Hex / String / Regex
- Search field: Packet List / Packet Details / Packet Bytes (cari di tempat yang tepat!)
- Mark — warna hitam, TIDAK permanen (hilang saat file ditutup)
- Comment — PERMANEN di dalam file sampai dihapus
- Export Packets: File → Export Specified Packets (all/selected/marked/range)
- Export Objects: File → Export Objects → HTTP/SMB/FTP/DICOM/TFTP

### Pewarnaan

- Permanent: View → Coloring Rules
- Temporary: View → Colorize Conversation
- Toggle semua warna: View → Colorize Packet List
- Reset: View → Colorize Conversation → Reset Colourisation

### Format Waktu

- Default: detik sejak capture dimulai
- Rekomendasi: UTC Time Display Format
- Ganti: View → Time Display Format

### Expert Info

- Akses: Analyse → Expert Information / klik ikon pojok kiri bawah
- Chat (Biru) — alur normal
- Note (Cyan) — event penting
- Warn (Kuning) — peringatan
- Error (Merah) — masalah serius / malformed
- Group: Checksum / Comment / Deprecated / Malformed

### Dua Jenis Filter

- Capture Filter — filter SAAT merekam (tidak tangkap yang tidak sesuai)
- Display Filter — filter SAAT melihat (sembunyikan, tidak hapus)
- Golden Rule: "If you can click on it, you can filter and copy it"

### Cara Filtering (Right-click)

- Apply as Filter — langsung terapkan, packet list berubah seketika
- Prepare as Filter — tambah ke bar, belum jalan sampai Enter (bisa AND/OR dulu)
- Conversation Filter — tampilkan semua paket 1 percakapan (IP+port), lain disembunyikan
- Colorise Conversation — sorot percakapan dengan warna, lain tetap kelihatan
- Apply as Column — tambah field sebagai kolom baru di Packet List
- Follow Stream — rekonstruksi percakapan lengkap level aplikasi
  - Merah = dari client, Biru = dari server
  - HTTP Stream = lebih bersih; TCP Stream = lebih raw
  - Hapus filter stream: klik X di Display Filter Bar

### Display Filter Queries

- Protocol name: http / dns / tcp / arp / dhcp / ftp / smtp
- By port: tcp.port == 80 / udp.port == 53
- By IP: ip.addr == x.x.x.x / ip.src == x.x.x.x / ip.dst == x.x.x.x

### Hashing di Terminal

- md5sum namafile → hash MD5 (32 karakter)
- sha256sum namafile → hash SHA256 (64 karakter)

### Istilah Singkat

- Hop — satu lompatan paket lewat satu router
- Payload — isi/konten paket di luar header
- Stream — rangkaian paket satu koneksi utuh
- Reassemble — gabungkan kembali potongan TCP
- Malformed — paket struktur rusak
- Interface — titik koneksi komputer ke jaringan (eth0, lo, dll)
