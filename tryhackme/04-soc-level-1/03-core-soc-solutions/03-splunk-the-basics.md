# Resume Room TryHackMe: Splunk Basics

Tanggal: 22 Oktober 2026

Room: Splunk Basics (Introduction, Connect with the Lab, Splunk Components, Navigating Splunk, Adding Data, Conclusion)

## 1. Gambaran Umum

### 1.1 Splunk dan SIEM

**Splunk** adalah salah satu solusi **SIEM** (Security Information and Event Management) terkemuka. Splunk mengumpulkan, menganalisis, dan mengorelasikan log jaringan dan mesin secara real-time.

Nilai utamanya ada pada dua hal: visibilitas yang lebih baik terhadap aktivitas jaringan, dan percepatan proses deteksi.

### 1.2 Tujuan Pembelajaran

Room ini mencakup empat hal:

- Memahami komponen-komponen Splunk
- Menjelajahi opsi yang tersedia di antarmuka Splunk
- Memahami proses log ingestion
- Mempraktikkan ingestion log ke Splunk dan menganalisisnya

### 1.3 Prasyarat

Pemula di bidang SIEM disarankan menyelesaikan room **Introduction to SIEM** terlebih dahulu.

### 1.4 Lingkungan Lab

Lingkungan terdiri dari dua komponen:

- **Attacker machine (AttackBox)**: dipakai jika tidak menggunakan VPN TryHackMe.
- **Lab machine**: target yang menjalankan instance Splunk.

Lab machine membutuhkan waktu sekitar 3 sampai 5 menit untuk menyala. Setelah menyala, Splunk diakses melalui `http://MACHINE_IP`, baik langsung dari AttackBox maupun via TryHackMe VPN.

URL dipakai persis seperti yang ditampilkan. **Jangan menambahkan `:8000`** karena lab ini menyajikan Splunk pada port HTTP default.

## 2. Komponen Splunk

Splunk memiliki tiga komponen utama yang bekerja bersama untuk mencari dan menganalisis data: **Forwarder**, **Indexer**, dan **Search Head**.

### 2.1 Splunk Forwarder

Agen ringan yang diinstal pada endpoint yang dimonitor. Tugas utamanya mengumpulkan data dan mengirimkannya ke instance Splunk. Dampak ke performa endpoint minimal karena memakai sedikit resource.

Sumber data utama yang dikumpulkan:

- Web server yang menghasilkan web traffic
- Mesin Windows yang menghasilkan Windows Event Logs, PowerShell, dan data Sysmon
- Host Linux yang menghasilkan log host-centric
- Database yang menghasilkan DB connection request, response, dan error

Forwarder mengirim data dari sumber log ke Indexer.

### 2.2 Splunk Indexer

Pemroses utama data yang diterima dari forwarder. Indexer melakukan tiga hal: parsing dan normalisasi data menjadi pasangan **field-value**, kategorisasi, dan penyimpanan hasilnya sebagai **events**. Dengan begitu data siap dicari dan dianalisis.

### 2.3 Splunk Search Head

Tempat pengguna mencari log yang sudah diindeks, berada di dalam aplikasi **Search & Reporting**. Pencarian memakai **SPL** (Search Processing Language), bahasa query untuk data terindeks.

Ketika pengguna mencari, permintaan dikirim ke indexer, lalu event yang relevan dikembalikan sebagai pasangan field-value.

Search Head juga mengubah hasil menjadi tabel dan visualisasi. Jenis visualisasi yang tersedia di tab Visualization mencakup line chart, area chart, column chart, bar chart, pie chart, scatter chart, bubble chart, single value, radial gauge, filler gauge, marker gauge, dan peta. Contohnya Column Chart yang berfungsi membandingkan nilai atau field, dengan search fragment:

```
| stats count by comparison_category
```

### 2.4 Alur Data

Urutan kerjanya: Forwarder mengumpulkan data dari sumber log, Indexer memproses dan menyimpannya, lalu Search Head dipakai pengguna untuk mencari dan memvisualisasikan.

## 3. Antarmuka Splunk

### 3.1 Home Screen

Tampilan default saat mengakses Splunk terdiri dari empat area: **Splunk Bar** (panel atas), **Apps Panel** (kiri), **Explore Splunk** (tengah), dan **Home Dashboard** (bawah).

### 3.2 Splunk Bar

Panel paling atas dengan lima opsi:

- **Messages**: notifikasi dan pesan tingkat sistem
- **Settings**: konfigurasi pengaturan instance Splunk
- **Activity**: meninjau progres search jobs dan proses
- **Help**: tutorial dan dokumentasi
- **Find**: pencarian di seluruh App

Splunk Bar juga memungkinkan perpindahan antar app tanpa melalui Apps Panel.

### 3.3 Apps Panel

Menampilkan app yang terinstal pada instance Splunk. App default pada setiap instalasi adalah **Search & Reporting**. Opsi **Find More Apps** tersedia untuk menambah app.

### 3.4 Explore Splunk

Panel berisi tautan cepat dengan tiga menu:

- **Add Data**: menambah atau mem-forward data ke Splunk, dilanjutkan extract fields
- **Splunk Apps**: app dan add-on yang memperluas kemampuan Splunk
- **Splunk Docs**: dokumentasi lengkap Splunk dan produk Splunk lainnya

### 3.5 Home Dashboard

Secara default tidak ada dashboard yang ditampilkan. Dashboard dipilih dari dropdown atau dari halaman dashboards listing. Dashboard buatan sendiri dapat ditambahkan ke Home Dashboard dan dilihat terpisah melalui tab **Yours**.

## 4. Data Ingestion

### 4.1 Konsep

Saat data ditambahkan ke Splunk, data diproses dan diubah menjadi rangkaian **event** individual. Sumber data dapat berupa event log, website log, firewall log, dan lainnya, yang dikelompokkan ke dalam kategori.

### 4.2 Kategori Sumber Data

| Kategori | Keterangan |
|---|---|
| Files and directories | Data langsung dari file dan direktori |
| Network events | Data dari port jaringan dan event SNMP perangkat remote |
| IT Operations | Data dari Nagios, NetApp, Cisco |
| Cloud services | Data dari AWS dan Kinesis |
| Database services | Data dari Oracle, MySQL, Microsoft SQL Server |
| Security services | Data dari McAfee, Microsoft Active Directory, Symantec Endpoint Protection |
| Virtualization services | Data dari VMware dan XenApp |
| Application servers | Data dari JMX dan JMS, WebLogic, WebSphere |
| Windows sources | Windows Event Log, Registry, WMI, Active Directory, Performance Monitoring |
| Other sources | FIFO queue dan scripted input untuk data dari API dan interface remote |

### 4.3 Metode Add Data

Halaman Add Data menyediakan tiga metode:

- **Upload**: mengunggah file dari komputer lokal (log file lokal, file terstruktur seperti CSV)
- **Monitor**: memantau file dan port pada instance Splunk itu sendiri (Files, HTTP, WMI, TCP/UDP, Scripts, modular inputs)
- **Forward**: menerima data dari Splunk Forwarder (Files, TCP/UDP, Scripts)

Halaman ini juga menyediakan panduan onboarding per kategori: Cloud computing, Networking, Operating System, dan Security.

### 4.4 Lima Tahap Upload

Wizard upload berjalan dalam lima tahap berurutan:

1. **Select Source**: memilih file log dan sumber datanya
2. **Select Source Type**: menentukan jenis log, misalnya JSON atau syslog
3. **Input Settings**: memilih index tujuan dan nilai HOST yang dikaitkan dengan log
4. **Review**: meninjau seluruh konfigurasi
5. **Done**: upload selesai dan data siap dianalisis

### 4.5 Pengaturan Import Log VPN

File `VPN_logs` berformat **newline-delimited JSON**, sehingga wizard upload memperlakukan setiap baris sebagai satu event. Ketentuan pengaturannya:

- Source type JSON dibiarkan sesuai deteksi otomatis Splunk
- Pada Input Settings, index `VPN_Logs` dibuat atau dipilih
- Setelah upload, time picker di Search & Reporting diatur ke **All time**

Jika nama field tidak muncul di hasil pencarian, tambahkan `| spath` setelah base search agar Splunk mem-parsing field JSON dari tiap event.

File log tersedia di AttackBox pada direktori `/root/Rooms/SplunkBasic/`.

## 5. Pencarian Dasar dengan SPL

### 5.1 Elemen Query

Setiap elemen yang muncul di room ini:

- `index=VPN_Logs`: membatasi pencarian pada index tertentu
- `|` (pipe): meneruskan hasil perintah sebelumnya ke perintah berikutnya
- `spath`: mengekstrak field dari data JSON di setiap event
- `search`: menyaring hasil berdasarkan kondisi field
- `=` dan `!=`: operator sama dengan dan tidak sama dengan
- `stats count`: menghitung jumlah event
- `stats values(UserName) as UserName count`: menampilkan nilai unik field `UserName`, di-alias sebagai `UserName`, bersama jumlah event
- `as`: memberi nama alias pada hasil fungsi

### 5.2 Contoh Query dari Room

Menghitung seluruh event dalam index:

```
index=VPN_Logs
| stats count
```

Menghitung event milik satu user:

```
index=VPN_Logs
| spath
| search UserName="Maleena"
| stats count
```

Mencari username yang terkait dengan satu IP:

```
index=VPN_Logs
| spath
| search Source_ip="107.14.182.38"
| stats values(UserName) as UserName count
```

Menghitung event dari semua negara selain satu negara:

```
index=VPN_Logs
| spath
| search Source_Country!="France"
| stats count
```

Menghitung event terkait satu IP:

```
index=VPN_Logs
| spath
| search Source_ip="107.3.206.58"
| stats count
```

### 5.3 Field pada Log VPN

Event log VPN memuat field berikut: `Company`, `EventTime`, `Source_Country`, `Source_ip`, `UserName`, `action` (contoh nilai `built` dan `teardown`), `port`, `protocol`, dan `source_state`.

## 6. Glossary

- **SIEM**: platform yang mengumpulkan, menganalisis, dan mengorelasikan log untuk deteksi ancaman
- **Forwarder**: agen ringan di endpoint yang mengumpulkan dan mengirim data ke Splunk
- **Indexer**: komponen yang memparsing, menormalisasi, dan menyimpan data sebagai event
- **Search Head**: antarmuka pencarian dan visualisasi data terindeks
- **SPL**: Search Processing Language, bahasa query Splunk
- **Event**: satu unit data hasil pemrosesan di Splunk
- **Field-value pair**: pasangan nama field dan nilainya hasil normalisasi
- **Index**: tempat penyimpanan event di Splunk
- **Source type**: jenis format data, misalnya JSON atau syslog
- **Host**: nilai yang menandai mesin asal event
- **Search & Reporting**: app default Splunk untuk pencarian dan pelaporan
- **Splunk Bar**: panel navigasi paling atas
- **Apps Panel**: panel daftar app terinstal
- **Home Dashboard**: dashboard pada halaman utama, kosong secara default
- **Newline-delimited JSON**: format JSON dengan satu objek per baris
- **spath**: perintah SPL untuk mengekstrak field JSON

## 7. Tools & Platform Rujukan

Tidak ada URL eksplisit yang tertera pada materi, sehingga hanya nama yang dicantumkan.

- **Splunk Documentation (Splunk Docs)**: dokumentasi resmi Splunk, termasuk panduan Navigating Splunk dan daftar kategori sumber data
- **Splunk Apps**: katalog app dan add-on untuk memperluas kemampuan Splunk
- **Introduction to SIEM (room TryHackMe)**: prasyarat dasar konsep SIEM
- **TryHackMe AttackBox dan VPN**: akses ke lab machine
- **Splunk: Exploring SPL (room TryHackMe)**: lanjutan pendalaman SPL
- **Incident Handling with Splunk (room TryHackMe)**: penggunaan Splunk dalam penanganan insiden
- **Investigating With Splunk (room TryHackMe)**: investigasi menggunakan Splunk
- **Benign - Challenge (room TryHackMe)**: challenge investigasi dengan Splunk
- **PoshEclipse - Challenge (room TryHackMe)**: challenge investigasi dengan Splunk

## 8. Catatan Ringkas untuk Ditulis Tangan

### Dasar

- Splunk — SIEM, kumpul/analisis/korelasi log real-time
- SIEM — Security Information and Event Management
- Manfaat — visibilitas jaringan, deteksi lebih cepat
- Prasyarat — room Introduction to SIEM

### Lab

- AttackBox — mesin penyerang (jika tanpa VPN)
- Lab machine — target, menjalankan Splunk
- Start — tunggu 3-5 menit
- URL — http://MACHINE_IP
- Jangan tambah :8000 — Splunk di port HTTP default

### Komponen

- Forwarder — agen ringan di endpoint, kumpul dan kirim data
- Sumber forwarder — web server, Windows (Event Log, PowerShell, Sysmon), Linux, database
- Indexer — parsing, normalisasi ke field-value, kategorisasi, simpan sebagai events
- Search Head — cari log di Search & Reporting pakai SPL
- Alur — Forwarder, Indexer, Search Head
- Visualisasi — tabel, pie, bar, column, line, area, scatter, gauge, peta
- Column chart — bandingkan nilai, `| stats count by comparison_category`

### Antarmuka

- Splunk Bar — panel atas
- Messages — notifikasi sistem
- Settings — konfigurasi instance
- Activity — progres search job
- Help — tutorial dan dokumentasi
- Find — cari di App
- Apps Panel — daftar app terinstal
- App default — Search & Reporting
- Find More Apps — tambah app
- Explore Splunk — Add Data, Splunk Apps, Splunk Docs
- Home Dashboard — kosong default, pilih via dropdown atau listing
- Tab Yours — dashboard buatan sendiri

### Data Ingestion

- Ingest — data diubah jadi events
- Kategori — files/directories, network events, IT ops, cloud, database, security, virtualization, app servers, Windows, other
- Upload — file dari komputer lokal
- Monitor — file dan port di instance Splunk
- Forward — data dari Splunk Forwarder
- Lima tahap — Select Source, Select Source Type, Input Settings, Review, Done
- Select Source — pilih file
- Source Type — jenis log (JSON, syslog)
- Input Settings — index dan host
- Review — cek konfigurasi
- Done — data siap dianalisis
- VPN_logs — newline-delimited JSON, satu baris satu event
- Index dibuat — VPN_Logs
- Time picker — All time
- Path file di AttackBox — /root/Rooms/SplunkBasic/

### SPL

- index=NAMA — batasi ke index
- | — pipe, lempar hasil ke perintah berikutnya
- spath — ekstrak field dari JSON
- search — saring berdasarkan field
- = dan != — sama dengan dan tidak sama dengan
- stats count — hitung jumlah event
- stats values(F) as F count — nilai unik field plus jumlah event
- as — alias nama hasil
- Field tidak muncul — tambah | spath

### Hasil Latihan Log VPN

- Total event — 2862
- Event user Maleena — 60
- Username untuk IP 107.14.182.38 — Smith
- Event non-France — 2814
- Event IP 107.3.206.58 — 14

### Lanjutan

- Splunk: Exploring SPL
- Incident Handling with Splunk
- Investigating With Splunk
- Benign - Challenge
- PoshEclipse - Challenge
