# Resume Materi — Firewall (TryHackMe)
Tanggal: 2026-07-06

---

## 1. Konsep Dasar Firewall

### 1.1 Definisi

**Firewall** adalah sistem yang dirancang untuk memeriksa lalu lintas jaringan (network traffic) yang masuk dan keluar dari sebuah perangkat atau jaringan. Firewall bekerja berdasarkan aturan (rules) yang telah ditentukan untuk mengizinkan atau menolak lalu lintas tersebut.

Setiap lalu lintas yang masuk atau keluar dari perangkat akan melewati firewall terlebih dahulu sebelum diteruskan ke tujuannya. Firewall modern umumnya tidak hanya melakukan penyaringan berbasis aturan, tetapi juga menawarkan fungsi-fungsi tambahan untuk perlindungan yang lebih menyeluruh.

### 1.2 Cara Kerja

Firewall diberi instruksi berupa **rules** — daftar aturan yang mendefinisikan lalu lintas mana yang boleh lewat dan mana yang harus diblokir. Semua traffic yang tidak sesuai dengan aturan yang ada akan diperlakukan sesuai kebijakan default yang ditetapkan.


---

## 2. Jenis-Jenis Firewall

### 2.1 Stateless Firewall

Beroperasi pada **layer 3 dan layer 4** OSI model. Menyaring paket data semata-mata berdasarkan aturan yang telah ditentukan, tanpa menyimpan informasi mengenai status koneksi sebelumnya.

Setiap paket diperlakukan sebagai entitas baru yang independen. Akibatnya, firewall ini tidak dapat menerapkan kebijakan kompleks berdasarkan riwayat koneksi — jika sebelumnya sebuah sumber ditolak, paket berikutnya dari sumber yang sama tetap akan diperiksa ulang dari awal.

Keunggulan: memproses paket dengan cepat sehingga cocok untuk jaringan berkecepatan tinggi.

### 2.2 Stateful Firewall

Beroperasi pada **layer 3 dan layer 4** OSI model. Berbeda dari stateless, firewall ini menyimpan informasi status koneksi dalam sebuah **state table**.

Jika sebuah koneksi dari sumber tertentu pernah diizinkan, semua paket berikutnya dari koneksi yang sama akan otomatis diizinkan tanpa perlu diperiksa ulang. Begitu pula sebaliknya — koneksi yang pernah ditolak akan langsung ditolak pada paket-paket berikutnya.

### 2.3 Proxy Firewall

Beroperasi pada **layer 7** OSI model (Application Layer). Bertindak sebagai perantara (intermediary) antara jaringan privat dan Internet, sering disebut juga **application-level gateway**.

Tidak hanya menyaring header paket, tetapi juga memeriksa isi (konten) dari paket tersebut. Permintaan dari pengguna diteruskan melalui proxy dengan alamat IP proxy sebagai pengirim, sehingga identitas IP internal tetap tersembunyi. Mendukung **content filtering policy** untuk mengizinkan atau menolak traffic berdasarkan kontennya.

### 2.4 Next-Generation Firewall (NGFW)

Beroperasi dari **layer 3 hingga layer 7** OSI model. Merupakan jenis firewall paling canggih dengan fitur-fitur berikut:

- **Deep Packet Inspection (DPI):** memeriksa isi paket secara mendalam
- **Intrusion Prevention System (IPS):** memblokir aktivitas berbahaya secara real-time
- **Heuristic analysis:** menganalisis pola serangan dan memblokir anomali sebelum sampai ke jaringan
- **SSL/TLS decryption:** mendekripsi paket terenkripsi untuk diperiksa isinya
- **Threat intelligence feeds:** mengkorelasikan data dengan feed intelijen ancaman untuk pengambilan keputusan yang lebih akurat

### 2.5 Tabel Perbandingan

| Jenis Firewall | Layer OSI | Karakteristik Utama |
|---|---|---|
| Stateless | 3-4 | Penyaringan dasar, tidak lacak koneksi, cepat |
| Stateful | 3-4 | Lacak koneksi, aturan kompleks, monitor jaringan |
| Proxy | 7 | Periksa isi paket, content filtering, kontrol aplikasi, dekripsi SSL/TLS |
| NGFW | 3-7 | Perlindungan ancaman lanjut, IPS, heuristik, dekripsi SSL/TLS |


---

## 3. Komponen Rules Firewall

Setiap aturan (rule) pada firewall terdiri dari komponen-komponen berikut:

**Source address** — alamat IP asal lalu lintas (mesin pengirim)

**Destination address** — alamat IP tujuan lalu lintas (mesin penerima)

**Port** — nomor port yang digunakan untuk lalu lintas tersebut

**Protocol** — jenis protokol komunikasi yang digunakan (TCP, UDP, dll.)

**Action** — tindakan yang diambil ketika lalu lintas cocok dengan rule (Allow, Deny, atau Forward)

**Direction** — menentukan apakah rule berlaku untuk lalu lintas masuk (Inbound) atau keluar (Outbound)

### 3.1 Jenis Aksi (Action)

**Allow** — lalu lintas yang sesuai dengan rule akan diizinkan untuk lewat.

Contoh rule Allow:

| Action | Source | Destination | Protocol | Port | Direction |
|---|---|---|---|---|---|
| Allow | 192.168.1.0/24 | Any | TCP | 80 | Outbound |

**Deny** — lalu lintas yang sesuai dengan rule akan diblokir. Penting untuk menolak traffic dari IP berbahaya dan mempersempit permukaan serangan (threat surface).

Contoh rule Deny:

| Action | Source | Destination | Protocol | Port | Direction |
|---|---|---|---|---|---|
| Deny | Any | 192.168.1.0/24 | TCP | 22 | Inbound |

**Forward** — lalu lintas dialihkan ke segmen jaringan lain. Berlaku untuk firewall yang berfungsi sebagai gateway antar segmen jaringan.

Contoh rule Forward:

| Action | Source | Destination | Protocol | Port | Direction |
|---|---|---|---|---|---|
| Forward | Any | 192.168.1.8 | TCP | 80 | Inbound |

### 3.2 Arah Rules (Directionality)

**Inbound Rules** — diterapkan hanya pada lalu lintas yang masuk ke jaringan. Contoh: mengizinkan HTTP (port 80) masuk ke web server.

**Outbound Rules** — diterapkan hanya pada lalu lintas yang keluar dari jaringan. Contoh: memblokir SMTP (port 25) dari semua perangkat kecuali mail server.

**Forward Rules** — meneruskan lalu lintas tertentu ke tujuan di dalam jaringan. Contoh: meneruskan HTTP (port 80) yang masuk ke web server internal.


---

## 4. Windows Defender Firewall

### 4.1 Gambaran Umum

**Windows Defender Firewall** adalah firewall bawaan Microsoft di sistem operasi Windows. Menyediakan fungsi dasar untuk membuat, mengizinkan, atau menolak program tertentu, serta membuat custom rules.

Cara membuka: Windows Search → ketik `Windows Defender Firewall`.

### 4.2 Network Profiles

Windows menentukan profil jaringan secara otomatis menggunakan **Network Location Awareness (NLA)**. Terdapat dua profil:

**Private networks** — konfigurasi firewall saat terhubung ke jaringan rumah atau jaringan terpercaya. Biasanya lebih permisif.

**Guest or public networks** — konfigurasi firewall saat terhubung ke jaringan publik (kafe, restoran, dll.). Biasanya lebih ketat, misalnya memblokir semua koneksi masuk dan hanya mengizinkan koneksi keluar tertentu.

Pengaturan masing-masing profil bersifat independen — perubahan pada profil publik tidak mempengaruhi profil privat.

### 4.3 Opsi Utama Dashboard

Dari halaman utama Windows Defender Firewall, tersedia opsi berikut:

- **Allow an app or feature through Windows Defender Firewall** — mencentang atau menghilangkan centang aplikasi tertentu per profil
- **Turn Windows Defender Firewall on or off** — menyalakan/mematikan per profil (mematikan sepenuhnya tidak direkomendasikan Microsoft)
- **Restore defaults** — mengembalikan semua pengaturan ke kondisi bawaan
- **Advanced settings** — membuka jendela untuk membuat custom rules secara detail

### 4.4 Membuat Custom Rules (Advanced Settings)

Buka **Advanced settings** dari dashboard untuk mengakses **Windows Defender Firewall with Advanced Security**. Di sini tersedia menu Inbound Rules, Outbound Rules, Connection Security Rules, dan Monitoring.

Langkah membuat Outbound Rule (contoh: blokir HTTP dan HTTPS):

1. Klik **Outbound Rules** → klik **New Rule** di panel kanan
2. **Rule Type:** pilih `Custom` → Next
3. **Program:** pilih `All programs` → Next
4. **Protocol and Ports:** Protocol = `TCP`, Remote port = `Specific Ports`, isi `80,443` (tanpa spasi) → Next
5. **Scope:** biarkan default (Any IP) → Next
6. **Action:** pilih `Block the connection` → Next
7. **Profile:** centang semua profil → Next
8. **Name:** beri nama rule → Finish

Rule yang sudah dibuat akan muncul di daftar Outbound Rules dan langsung aktif.

### 4.5 Catatan Penting

Rule custom dapat dibuat langsung di dalam VM yang disediakan TryHackMe. Dalam konteks lab room ini, beberapa rule sudah dipasang sebelumnya di VM sebagai bagian dari setup — termasuk rule "Block HTTP & HTTPS traffic" yang membuat `http://10.10.10.10` tidak bisa diakses dari dalam VM sejak awal.


---

## 5. Linux Firewall — Netfilter & ufw

### 5.1 Netfilter

**Netfilter** adalah framework inti di dalam Linux yang menyediakan fungsi-fungsi firewall: packet filtering, NAT (Network Address Translation), dan connection tracking. Framework ini menjadi fondasi bagi berbagai utilitas firewall Linux.

Tiga utilitas utama berbasis Netfilter:

**iptables** — utilitas paling banyak digunakan di distribusi Linux. Menyediakan kontrol lengkap atas lalu lintas jaringan melalui Netfilter.

**nftables** — penerus iptables dengan kemampuan packet filtering dan NAT yang lebih baik. Juga berbasis Netfilter.

**firewalld** — beroperasi di atas Netfilter dengan pendekatan berbeda: menggunakan predefined rule sets dan konfigurasi zona jaringan yang sudah siap pakai.

### 5.2 ufw (Uncomplicated Firewall)

**ufw** adalah antarmuka yang menyederhanakan pembuatan rules iptables. Cocok untuk pemula karena menggunakan sintaks yang lebih mudah dibaca. Di balik layar, ufw tetap mengonfigurasi rules di dalam iptables.

### 5.3 Perintah-Perintah ufw

**Cek status firewall:**
```
sudo ufw status
```
Menampilkan apakah firewall aktif (active) atau tidak aktif (inactive), beserta daftar rules yang ada jika aktif.

**Aktifkan firewall:**
```
sudo ufw enable
```
Mengaktifkan ufw dan mengaturnya agar otomatis berjalan saat startup sistem.

**Matikan firewall:**
```
sudo ufw disable
```
Menonaktifkan ufw.

**Atur kebijakan default outgoing:**
```
sudo ufw default allow outgoing
```
- `default` — mendefinisikan kebijakan default (berlaku untuk semua traffic kecuali ada rule spesifik yang mengoverride)
- `allow` — izinkan; ganti dengan `deny` untuk menolak
- `outgoing` — berlaku untuk traffic keluar; ganti dengan `incoming` untuk traffic masuk

**Blokir traffic masuk pada port tertentu:**
```
sudo ufw deny 22/tcp
```
- `deny` — tolak/blokir traffic
- `22/tcp` — port 22 dengan protokol TCP (format: `port/protokol`)

**Tampilkan semua rules dalam urutan bernomor:**
```
sudo ufw status numbered
```
Menampilkan daftar rules aktif beserta nomor urut, kolom To (tujuan/port), Action (tindakan), dan From (asal).

**Hapus rule berdasarkan nomor:**
```
sudo ufw delete 2
```
- `delete` — hapus rule
- `2` — nomor rule yang ingin dihapus (nomor diperoleh dari `ufw status numbered`)

Sistem akan meminta konfirmasi sebelum menghapus — ketik `y` lalu Enter untuk melanjutkan.


---

## 6. Glossary

**Firewall** — sistem yang memeriksa dan menyaring lalu lintas jaringan masuk/keluar berdasarkan aturan yang ditetapkan.

**Stateless Firewall** — firewall yang menyaring paket tanpa mengingat status koneksi sebelumnya.

**Stateful Firewall** — firewall yang menyimpan riwayat koneksi dalam state table untuk digunakan dalam pengambilan keputusan berikutnya.

**Proxy Firewall / Application-Level Gateway** — firewall yang bertindak sebagai perantara dan memeriksa isi paket hingga layer aplikasi.

**NGFW (Next-Generation Firewall)** — firewall generasi terbaru dengan kemampuan DPI, IPS, heuristik, dan dekripsi SSL/TLS.

**Netfilter** — framework kernel Linux yang menjadi fondasi berbagai utilitas firewall (iptables, nftables, firewalld).

**ufw (Uncomplicated Firewall)** — antarmuka sederhana untuk mengelola iptables di Linux.

**iptables** — utilitas firewall Linux paling banyak digunakan, berbasis Netfilter.

**Rules** — aturan yang mendefinisikan bagaimana firewall memperlakukan lalu lintas tertentu.

**Inbound Rules** — aturan yang berlaku untuk lalu lintas yang masuk ke sistem/jaringan.

**Outbound Rules** — aturan yang berlaku untuk lalu lintas yang keluar dari sistem/jaringan.

**Forward Rules** — aturan untuk meneruskan lalu lintas ke segmen jaringan lain di dalam infrastruktur.

**Action** — tindakan yang diambil firewall terhadap paket yang cocok dengan rule (Allow, Deny, Forward).

**Deep Packet Inspection (DPI)** — teknik pemeriksaan isi paket secara mendalam hingga layer aplikasi.

**Intrusion Prevention System (IPS)** — sistem yang mendeteksi dan memblokir aktivitas berbahaya secara real-time.

**State Table** — tabel internal pada stateful firewall yang menyimpan informasi status koneksi aktif.

**Threat Surface** — keseluruhan titik potensial yang dapat dieksploitasi oleh penyerang pada suatu sistem atau jaringan.

**Network Location Awareness (NLA)** — mekanisme Windows untuk mendeteksi jenis jaringan yang sedang digunakan dan menerapkan profil firewall yang sesuai.

**NAT (Network Address Translation)** — teknik untuk menerjemahkan alamat IP privat ke alamat IP publik dan sebaliknya.

**Content Filtering** — kemampuan untuk mengizinkan atau menolak traffic berdasarkan isi/konten dari paket data.

**SSL/TLS Decryption** — kemampuan firewall untuk mendekripsi traffic terenkripsi guna memeriksa isinya sebelum diteruskan.

**Heuristic Analysis** — metode analisis ancaman berdasarkan pola dan perilaku yang mencurigakan, bukan hanya signature yang sudah diketahui.

**Threat Intelligence Feeds** — sumber data eksternal yang berisi informasi ancaman terkini, digunakan NGFW untuk membuat keputusan yang lebih akurat.


---

## 7. Tools & Platform Rujukan

**Windows Defender Firewall** — firewall bawaan Windows; diakses melalui Windows Search atau Control Panel > System and Security > Windows Defender Firewall.

**Windows Defender Firewall with Advanced Security** — antarmuka lanjutan Windows Defender Firewall untuk membuat dan mengelola custom rules secara detail; diakses melalui opsi "Advanced settings" di dashboard utama.

**ufw** — utilitas firewall Linux berbasis iptables; tersedia di distribusi berbasis Debian/Ubuntu secara default.

**iptables** — utilitas firewall Linux tingkat lanjut; tersedia di hampir semua distribusi Linux.

**nftables** — penerus iptables dengan sintaks yang lebih modern.

**firewalld** — utilitas firewall Linux dengan pendekatan zona jaringan; umum digunakan di distribusi berbasis Red Hat/CentOS/Fedora.


---

## 8. Catatan Ringkas untuk Ditulis Tangan

### Firewall — Dasar

- Firewall — periksa & saring traffic jaringan masuk/keluar berdasarkan rules
- Rules — aturan berisi: source, destination, port, protocol, action, direction
- Action — Allow / Deny / Forward
- Direction — Inbound (masuk) / Outbound (keluar) / Forward (teruskan ke segmen lain)

### Jenis Firewall

- Stateless — layer 3-4, no state table, cepat, tidak ingat koneksi lama
- Stateful — layer 3-4, punya state table, ingat riwayat koneksi
- Proxy — layer 7, periksa isi paket, sembunyikan IP internal, content filtering
- NGFW — layer 3-7, DPI + IPS + heuristik + SSL/TLS decrypt + threat intel

### Komponen Rule

- Source address — IP pengirim
- Destination address — IP penerima
- Port — nomor port
- Protocol — TCP/UDP/dll
- Action — Allow / Deny / Forward
- Direction — Inbound / Outbound

### Windows Defender Firewall

- Buka via: Windows Search → "Windows Defender Firewall"
- 2 profil: Private networks & Guest/public networks (bisa beda setting)
- Advanced settings → buat/kelola custom rules
- Inbound Rules — untuk traffic masuk
- Outbound Rules — untuk traffic keluar
- Wizard custom rule: Rule Type → Program → Protocol & Ports → Scope → Action → Profile → Name

### Linux Firewall

- Netfilter — framework kernel Linux, fondasi iptables/nftables/firewalld
- ufw — antarmuka mudah untuk iptables, cocok pemula
- iptables — utilitas paling umum, berbasis Netfilter
- nftables — penerus iptables
- firewalld — berbasis zona jaringan, pakai predefined rules

### Perintah ufw

- sudo ufw status — cek status aktif/tidak + daftar rules
- sudo ufw enable — aktifkan firewall
- sudo ufw disable — matikan firewall
- sudo ufw default allow outgoing — izinkan semua traffic keluar (default)
- sudo ufw default deny incoming — tolak semua traffic masuk (default)
- sudo ufw deny 22/tcp — blokir SSH masuk (port 22, protokol TCP)
- sudo ufw status numbered — tampilkan rules dengan nomor urut
- sudo ufw delete 2 — hapus rule nomor 2

### Istilah Kunci

- DPI — Deep Packet Inspection, periksa isi paket hingga layer aplikasi
- IPS — Intrusion Prevention System, blokir ancaman real-time
- State table — tabel koneksi aktif pada stateful firewall
- NAT — terjemah IP privat ↔ publik
- NLA — Network Location Awareness, deteksi tipe jaringan di Windows
- Threat surface — total titik potensial serangan pada sistem
- Heuristic analysis — deteksi ancaman dari pola perilaku, bukan signature
- Threat intel feeds — data ancaman eksternal untuk bantu keputusan NGFW
- Content filtering — saring traffic berdasarkan isi/konten paket
