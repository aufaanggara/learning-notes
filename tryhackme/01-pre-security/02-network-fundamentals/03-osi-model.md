# Resume Materi — OSI Model (TryHackMe)
**Tanggal:** 01 September 2026

---

## 1. Konsep Dasar OSI Model

**OSI** (Open Systems Interconnection) Model adalah kerangka kerja standar yang mengatur bagaimana perangkat jaringan mengirim, menerima, dan menginterpretasikan data.

Manfaat utamanya adalah memungkinkan perangkat dengan fungsi dan desain yang berbeda-beda untuk tetap bisa berkomunikasi satu sama lain dalam jaringan yang sama, selama mengikuti standar OSI.

Konsep penting: setiap layer memiliki tanggung jawab yang berbeda, dan saat data melewati setiap layer, terjadi proses yang disebut **encapsulation** — yaitu penambahan informasi ke data di masing-masing layer.

Model OSI terdiri dari **7 layer**, disusun dari Layer 7 (paling atas, dekat pengguna) hingga Layer 1 (paling bawah, dekat hardware fisik).

---

## 2. Tujuh Layer OSI

**Mnemonic untuk menghafal urutan (Layer 7 ke Layer 1):**
> All People Seem To Need Data Processing

| No. | Nama Layer   | Kata Mnemonic |
|-----|-------------|---------------|
| 7   | Application | All           |
| 6   | Presentation| People        |
| 5   | Session     | Seem          |
| 4   | Transport   | To            |
| 3   | Network     | Need          |
| 2   | Data Link   | Data          |
| 1   | Physical    | Processing    |

---

## 3. Penjelasan Tiap Layer

### 3.1 Layer 1 — Physical

Layer paling dasar. Bertanggung jawab atas **komponen fisik hardware** yang digunakan dalam jaringan.

- Perangkat menggunakan **sinyal listrik** untuk mentransfer data
- Data direpresentasikan dalam format **biner** — hanya mengenal angka 1 dan 0
- Layer ini tidak peduli dengan isi atau makna data — tugasnya hanya memindahkan sinyal mentah dari satu titik ke titik lain
- Contoh perangkat: kabel Ethernet (RJ-45), kabel fiber optik, hub, sinyal Wi-Fi

### 3.2 Layer 2 — Data Link

Bertanggung jawab atas **pengalamatan fisik** dari sebuah transmisi.

- Menerima paket dari Network Layer (yang sudah mengandung IP address tujuan), lalu menambahkan **MAC address** dari perangkat penerima ke paket tersebut
- Setiap komputer yang terhubung ke jaringan memiliki **NIC (Network Interface Card)** yang dilengkapi MAC address unik

**Tentang MAC Address:**

- Ditetapkan oleh **produsen hardware** dan dibakar langsung ke dalam kartu (*burnt into the card*) — sifatnya permanen
- Secara teknis tidak bisa diubah, namun **bisa di-spoof** (dipalsukan)
- Saat data dikirim melalui jaringan, MAC address inilah yang digunakan untuk menentukan ke mana tepatnya data harus dikirim

Layer ini juga bertugas menyajikan data dalam **format yang sesuai untuk transmisi**.

Contoh perangkat: Switch, NIC.

### 3.3 Layer 3 — Network

Tempat terjadinya **routing dan re-assembly data**.

- **Routing**: menentukan jalur paling optimal untuk mengirimkan paket-paket data dari pengirim ke penerima
- **Re-assembly**: menyusun kembali paket-paket kecil menjadi data yang utuh di sisi penerima

**Faktor penentu jalur optimal:**

- Jalur terpendek — paling sedikit perangkat yang dilewati
- Jalur paling andal — jalur yang paling jarang kehilangan paket
- Koneksi fisik tercepat — fiber optik (cepat) vs kabel tembaga (lambat)

**Protokol routing yang digunakan:**

- **OSPF** (Open Shortest Path First)
- **RIP** (Routing Information Protocol)

Pengalamatan di layer ini menggunakan **IP Address** (contoh: 192.168.1.100). Perangkat yang bekerja di layer ini disebut **Layer 3 devices**, contohnya adalah **router**.

### 3.4 Layer 4 — Transport

Bertanggung jawab atas **transmisi data antar perangkat** menggunakan salah satu dari dua protokol: **TCP** atau **UDP**. Pilihan protokol ditentukan berdasarkan kebutuhan aplikasi.

**TCP (Transmission Control Protocol)**

Dirancang dengan mengutamakan keandalan dan jaminan pengiriman.

- Menjaga **koneksi konstan** antara dua perangkat selama proses pengiriman berlangsung
- Memiliki **error checking** — memastikan data yang dikirim sampai dan disusun kembali dalam urutan yang benar
- Cocok untuk: transfer file, browsing web, pengiriman email — layanan yang menuntut data **akurat dan lengkap**

| Kelebihan TCP | Kekurangan TCP |
|---|---|
| Menjamin akurasi data | Butuh koneksi andal; satu paket hilang = seluruh data tidak bisa digunakan |
| Sinkronisasi dua perangkat agar tidak dibanjiri data | Koneksi lambat menyebabkan bottleneck karena koneksi terus direservasi |
| Banyak proses untuk memastikan keandalan | Lebih lambat dibanding UDP |

**UDP (User Datagram Protocol)**

Tidak secanggih TCP — tidak memiliki error checking maupun jaminan pengiriman.

- Data dikirim ke tujuan **entah sampai atau tidak** — tidak ada sinkronisasi antar perangkat
- Prinsipnya: *just hope for the best, and fingers crossed*
- Cocok untuk: video streaming, protokol ARP dan DHCP, skenario di mana kecepatan lebih penting dari akurasi

| Kelebihan UDP | Kekurangan UDP |
|---|---|
| Jauh lebih cepat dari TCP | Tidak peduli apakah data diterima atau tidak |
| Application layer yang mengontrol kecepatan pengiriman paket | Koneksi tidak stabil menghasilkan pengalaman buruk bagi pengguna |
| Tidak mereservasi koneksi konstan seperti TCP | Cukup fleksibel bagi developer, tapi tanpa kepastian |

**Perbandingan ringkas TCP vs UDP:**

| Aspek | TCP | UDP |
|---|---|---|
| Kecepatan | Lambat | Cepat |
| Keandalan | Sangat andal | Tidak ada jaminan |
| Error checking | Ada | Tidak ada |
| Koneksi | Konstan & direservasi | Tidak ada koneksi permanen |
| Use case | File, web, email | Streaming, ARP, DHCP |

### 3.5 Layer 5 — Session

Bertanggung jawab atas **pembuatan, pemeliharaan, dan penutupan sesi komunikasi** antar perangkat.

- Sesi terbentuk saat koneksi berhasil dibuat — selama koneksi aktif, sesi pun aktif
- Layer ini juga menutup koneksi secara otomatis jika tidak digunakan dalam beberapa waktu atau jika koneksi terputus
- Mendukung **checkpoint**: jika data hilang di tengah proses, hanya data terbaru (setelah checkpoint terakhir) yang perlu dikirim ulang — menghemat bandwidth
- Sesi bersifat **unik dan terisolasi** — data hanya bisa mengalir di dalam satu sesi, tidak bisa berpindah ke sesi lain

### 3.6 Layer 6 — Presentation

Layer tempat **standarisasi format data** berlangsung.

- Karena developer bisa membuat software dengan cara yang berbeda-beda, data tetap harus bisa dipahami dengan cara yang sama di sisi penerima — itulah fungsi layer ini
- Bertindak sebagai **translator** antara Application Layer (7) dan layer-layer di bawahnya
- Contoh: dua pengguna menggunakan email client yang berbeda, tetapi isi email tetap tampil sama di kedua sisi

**Dua arah kerja layer ini:**

- Saat mengirim: **enkripsi + kompresi** data
- Saat menerima: **dekripsi + dekompresi** data kembali ke bentuk semula

Fitur keamanan seperti **enkripsi HTTPS** saat mengakses situs aman terjadi di layer ini.

### 3.7 Layer 7 — Application

Layer yang paling dekat dengan pengguna (**end user**).

- Tempat protokol dan aturan ditetapkan untuk menentukan bagaimana pengguna berinteraksi dengan data yang dikirim atau diterima
- Menyediakan **GUI (Graphical User Interface)** yang ramah pengguna melalui aplikasi sehari-hari
- Contoh aplikasi: browser web, email client, FileZilla (FTP client)
- Protokol penting di layer ini: **DNS (Domain Name System)** — menerjemahkan nama domain (seperti `google.com`) menjadi IP address yang bisa digunakan jaringan

---

## 4. Konsep Re-assembly (Pemecahan dan Penyusunan Ulang Data)

Saat data dikirim melalui jaringan, data tidak dikirim dalam satu kesatuan besar. Data **dipecah menjadi paket-paket kecil** terlebih dahulu.

Alasan pemecahan:

- Jaringan memiliki batas kapasitas pengiriman data
- Paket kecil lebih fleksibel — bisa dikirim melalui jalur yang berbeda-beda
- Jika ada paket yang hilang, hanya paket itu yang perlu dikirim ulang, bukan seluruh data dari awal

Setelah semua paket tiba di tujuan, **Network Layer (Layer 3) menyusun ulang (re-assembly)** paket-paket tersebut menjadi data yang utuh seperti semula.

---

## 5. Glossary — Istilah Penting

**Encapsulation** — proses penambahan informasi ke data saat melewati setiap layer OSI

**Re-assembly** — penyusunan ulang paket-paket kecil menjadi data utuh di sisi penerima

**Packet** — potongan kecil data yang dikirim melalui jaringan

**MAC Address** — alamat fisik unik yang dibakar ke NIC oleh produsen hardware; digunakan Layer 2 untuk pengalamatan fisik

**IP Address** — alamat logis yang digunakan di Layer 3 untuk routing antar jaringan

**NIC (Network Interface Card)** — kartu jaringan yang terpasang di setiap komputer; dilengkapi MAC address unik

**Routing** — proses menentukan jalur terbaik untuk mengirimkan paket dari pengirim ke penerima

**Spoofing** — memalsukan identitas; dalam konteks Layer 2, MAC spoofing berarti memalsukan MAC address perangkat

**Bottleneck** — kondisi di mana satu titik memperlambat seluruh sistem; terjadi pada TCP saat koneksi lambat

**Checkpoint** — penanda kemajuan di Session Layer; jika data hilang, hanya data setelah checkpoint terakhir yang perlu dikirim ulang

**GUI (Graphical User Interface)** — antarmuka visual untuk pengguna; disediakan oleh aplikasi di Layer 7

**DNS (Domain Name System)** — sistem yang menerjemahkan nama domain menjadi IP address

**OSPF (Open Shortest Path First)** — protokol routing di Layer 3 yang menentukan jalur terpendek

**RIP (Routing Information Protocol)** — protokol routing di Layer 3

**TCP (Transmission Control Protocol)** — protokol Layer 4 yang mengutamakan keandalan dan jaminan pengiriman

**UDP (User Datagram Protocol)** — protokol Layer 4 yang mengutamakan kecepatan tanpa jaminan pengiriman

**Layer 3 Device** — perangkat yang bekerja di Layer 3, contohnya router

**Burnt into the card** — istilah yang menggambarkan MAC address yang tertanam permanen ke dalam NIC oleh produsen

---

## 6. Tools & Platform Rujukan

Tidak ada tools atau platform eksternal yang secara eksplisit direferensikan di room ini. Room ini bersifat konseptual (teori OSI Model) tanpa latihan berbasis tools.

---

## 7. Catatan Ringkas untuk Ditulis Tangan

### OSI Model — Dasar

- OSI — Open Systems Interconnection, standar komunikasi jaringan
- 7 layer, dari Layer 7 (Application) ke Layer 1 (Physical)
- Mnemonic — "All People Seem To Need Data Processing"
- Encapsulation — tiap layer tambah info ke data saat data melewatinya

### Urutan 7 Layer

- L7 Application — paling dekat pengguna, GUI, DNS, HTTP
- L6 Presentation — translator, enkripsi/dekripsi, kompresi/dekompresi
- L5 Session — buat/jaga/tutup sesi, checkpoint, sesi bersifat unik
- L4 Transport — TCP vs UDP, segmentasi data
- L3 Network — routing, IP address, OSPF, RIP, re-assembly
- L2 Data Link — MAC address, NIC, pengalamatan fisik
- L1 Physical — sinyal listrik, biner (1 & 0), kabel, hub

### Layer 1 — Physical

- Sinyal listrik, format biner
- Contoh: kabel Ethernet, fiber, Wi-Fi, hub
- Tidak peduli isi data, hanya pindahkan sinyal

### Layer 2 — Data Link

- Pengalamatan fisik menggunakan MAC address
- NIC — Network Interface Card, tiap perangkat punya MAC unik
- MAC — dibakar permanen oleh produsen, tidak bisa diubah, bisa di-spoof
- Perangkat: Switch

### Layer 3 — Network

- Routing + re-assembly paket
- Faktor rute: terpendek, terandal, koneksi tercepat
- Protokol: OSPF (Open Shortest Path First), RIP (Routing Information Protocol)
- Pengalamatan: IP Address (contoh: 192.168.1.100)
- Perangkat: Router = Layer 3 device

### Layer 4 — Transport

- TCP — andal, koneksi konstan, error checking, lambat
- UDP — cepat, tanpa jaminan, tanpa koneksi, tanpa error check
- TCP cocok: file, web, email
- UDP cocok: streaming, ARP, DHCP

**TCP kelebihan:** akurasi terjamin, sinkronisasi, banyak proses keandalan

**TCP kekurangan:** butuh koneksi andal, bisa bottleneck, lebih lambat

**UDP kelebihan:** cepat, app layer kontrol kecepatan, tidak reservasi koneksi

**UDP kekurangan:** tidak peduli data sampai/tidak, tidak stabil = UX buruk

### Layer 5 — Session

- Buat, jaga, tutup sesi komunikasi
- Sesi = terbentuk saat koneksi berhasil
- Checkpoint — kalau data hilang, hanya kirim ulang data terbaru
- Sesi unik — data tidak bisa berpindah antar sesi

### Layer 6 — Presentation

- Standarisasi format data
- Translator antara L7 dan layer bawah
- Kirim: enkripsi + kompresi
- Terima: dekripsi + dekompresi
- HTTPS — enkripsi terjadi di layer ini

### Layer 7 — Application

- Paling dekat pengguna
- Protokol & aturan interaksi pengguna dengan data
- GUI — antarmuka visual (browser, email client, FileZilla)
- DNS — terjemahkan domain ke IP address

### Re-assembly

- Data dipecah jadi paket kecil saat dikirim
- L3 susun ulang paket jadi data utuh di penerima
- Kalau paket hilang — hanya paket itu yang dikirim ulang

### Istilah Kunci

- Packet — potongan kecil data
- MAC Address — alamat fisik NIC, permanen, bisa di-spoof
- IP Address — alamat logis, dipakai Layer 3
- Routing — tentukan jalur terbaik antar jaringan
- Spoofing — memalsukan identitas (contoh: MAC spoofing)
- Bottleneck — satu titik memperlambat seluruh sistem
- Checkpoint — penanda di Session Layer, hemat bandwidth
- DNS — domain → IP address
- NIC — Network Interface Card
- OSPF — protokol routing cari jalur terpendek
- RIP — protokol routing Layer 3
