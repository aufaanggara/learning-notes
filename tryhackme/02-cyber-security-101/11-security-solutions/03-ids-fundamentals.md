# Resume Materi — Introduction to IDS
**Platform:** TryHackMe  
**Tanggal Penyelesaian:** 2025  

---

## 1. Konsep Dasar IDS

**Intrusion Detection System (IDS)** adalah solusi keamanan yang ditempatkan di dalam jaringan untuk mendeteksi aktivitas berbahaya yang berhasil melewati firewall.

Perbedaan mendasar antara firewall dan IDS:

- **Firewall** bekerja di batas (boundary) jaringan, memeriksa dan memblokir koneksi sebelum masuk.
- **IDS** bekerja di dalam jaringan, memantau traffic yang sudah masuk dan menghasilkan alert jika ada aktivitas mencurigakan.

IDS **tidak memblokir** traffic — ia hanya mendeteksi dan memberi notifikasi kepada administrator keamanan. Tindakan lanjutan sepenuhnya diserahkan kepada manusia.

---

## 2. Klasifikasi IDS

### 2.1 Berdasarkan Mode Deployment

**Host Intrusion Detection System (HIDS)**

- Diinstal secara individual di setiap host.
- Hanya memantau aktivitas host tersebut secara spesifik.
- Memberikan visibilitas detail terhadap aktivitas satu mesin.
- Kelemahannya: sulit dikelola di jaringan besar karena membutuhkan instalasi dan manajemen di setiap host (resource-intensive).

**Network Intrusion Detection System (NIDS)**

- Ditempatkan di level jaringan, memantau traffic dari semua host sekaligus.
- Memberikan pandangan terpusat (centralized view) atas seluruh aktivitas jaringan.
- Tidak terikat pada host tertentu — mendeteksi anomali di seluruh segmen jaringan.

### 2.2 Berdasarkan Mode Deteksi

**Signature-Based IDS**

- Bekerja dengan mencocokkan traffic terhadap database signature (pola serangan yang sudah diketahui).
- Sangat efisien untuk mendeteksi ancaman yang sudah dikenal — semakin kaya database-nya, semakin akurat deteksinya.
- Kelemahannya: tidak bisa mendeteksi **zero-day attack** karena belum ada signature yang terdaftar untuk serangan baru.
- Snort (yang dibahas di room ini) termasuk dalam kategori ini.

**Anomaly-Based IDS**

- Bekerja dengan mempelajari perilaku normal jaringan/sistem (disebut **baseline**), lalu memberi alert jika ada penyimpangan.
- Mampu mendeteksi zero-day attack karena tidak bergantung pada signature.
- Kelemahannya: rentan menghasilkan **false positive** — aktivitas normal yang kebetulan menyimpang dari baseline bisa salah ditandai sebagai berbahaya.
- False positive dapat dikurangi dengan **fine-tuning** (mendefinisikan baseline secara manual).

**Hybrid IDS**

- Menggabungkan signature-based dan anomaly-based untuk memanfaatkan kelebihan keduanya.
- Untuk ancaman yang sudah dikenal → menggunakan teknik signature-based.
- Untuk ancaman baru → menggunakan teknik anomaly-based.

| Jenis | Deteksi Ancaman Dikenal | Deteksi Zero-Day | False Positive |
|---|---|---|---|
| Signature-Based | Cepat & akurat | Tidak bisa | Rendah |
| Anomaly-Based | Bisa | Bisa | Tinggi |
| Hybrid | Cepat & akurat | Bisa | Sedang |

---

## 3. Snort — IDS Open Source

**Snort** adalah salah satu IDS open-source paling populer, dikembangkan sejak 1998. Snort menggunakan pendekatan signature-based dan anomaly-based untuk mengidentifikasi ancaman.

### 3.1 Direktori dan File Penting

Lokasi default konfigurasi Snort: `/etc/snort/`

Catatan: Di Snort 3, lokasi ini tidak selalu tetap — bergantung pada bagaimana Snort diinstal. Build dari source code biasanya menggunakan `/usr/local/etc/snort`.

File-file penting di direktori Snort:

- `snort.lua` — file konfigurasi utama; mendefinisikan rule yang aktif, range jaringan (`$HOME_NET`), dan berbagai pengaturan lainnya.
- `rules/` — folder yang berisi semua file rule, termasuk rule bawaan dan custom rule.
- `rules/local.rules` — file khusus untuk custom rule yang dibuat sendiri.
- `community-sid-msg.map` — pemetaan SID ke pesan rule komunitas.
- `Intro_to_IDS.pcap` — file PCAP contoh yang disediakan oleh room ini untuk latihan analisis forensik.

### 3.2 Mode Operasi Snort

**Packet Sniffer Mode**

- Membaca dan menampilkan paket jaringan tanpa melakukan analisis atau deteksi.
- Berguna untuk monitoring jaringan dan troubleshooting performa.
- Tidak berkaitan langsung dengan fungsi IDS.
- Use case: tim jaringan yang perlu melihat alur traffic mentah untuk mendiagnosis masalah koneksi.

**Packet Logging Mode**

- Melakukan deteksi secara real-time dan mencatat traffic ke dalam file **PCAP** (standard packet capture format).
- File log dapat digunakan oleh investigator forensik untuk melakukan root cause analysis pada serangan yang sudah terjadi.
- Use case: investigasi forensik pasca-serangan yang membutuhkan rekaman lengkap traffic jaringan.

**NIDS Mode**

- Mode utama Snort sebagai IDS.
- Memantau traffic secara real-time dan mencocokkannya dengan rule files.
- Menghasilkan alert setiap kali ada traffic yang cocok dengan pola serangan yang tersimpan.
- Use case: pemantauan proaktif jaringan untuk mendeteksi potensi ancaman secara real-time.

Mode yang paling relevan untuk fungsi IDS adalah **NIDS Mode**.

---

## 4. Format Rule Snort

Setiap rule Snort memiliki struktur yang spesifik. Berikut contoh rule lengkap beserta anatomisnya:

```
alert icmp any any -> $HOME_NET any (msg:"Ping Detected"; sid:10001; rev:1;)
```

### 4.1 Komponen Rule

**Action** — tindakan yang diambil saat rule terpicu. Nilai umum yang digunakan: `alert`.

**Protocol** — protokol yang dipantau. Nilai yang valid di Snort: `tcp`, `udp`, `icmp`, `ip`. Nama protokol aplikasi seperti `ssh` atau `http` tidak valid sebagai protokol di sini.

**Source IP** — alamat IP asal traffic. Gunakan `any` jika tidak ingin membatasi sumber.

**Source Port** — port asal traffic. Gunakan `any` jika tidak ingin membatasi port sumber.

`->` — operator arah traffic (dari sumber ke tujuan). Harus ditulis sebagai tanda minus diikuti tanda lebih besar (`->`), bukan karakter panah Unicode (`→`).

**Destination IP** — alamat IP tujuan. Dapat menggunakan variabel `$HOME_NET` yang nilainya didefinisikan di `snort.lua` sebagai range jaringan internal.

**Destination Port** — port tujuan traffic. Dapat diisi angka port spesifik (contoh: `22` untuk SSH, `80` untuk HTTP) atau `any`.

**Rule Metadata** — ditulis di dalam tanda kurung di akhir rule. Terdiri dari:

- `msg:"<pesan>"` — pesan yang ditampilkan ketika alert terpicu. Harus mendeskripsikan jenis aktivitas yang terdeteksi.
- `sid:<angka>` — Signature ID, identifier unik untuk setiap rule. Tidak boleh ada dua rule dengan sid yang sama.
- `rev:<angka>` — nomor revisi rule. Bertambah setiap kali rule dimodifikasi, berguna untuk tracking perubahan.

### 4.2 Contoh Rule yang Digunakan di Room Ini

Rule untuk mendeteksi ICMP ke loopback:
```
alert icmp any any -> 127.0.0.1 any (msg:"Loopback Ping Detected"; sid:10003; rev:1;)
```

Rule untuk mendeteksi koneksi SSH ke jaringan internal:
```
alert tcp any any -> $HOME_NET 22 (msg:"SSH Detected"; sid:10001; rev:1;)
```

---

## 5. Command Snort dan Penjelasan Flag

### 5.1 Melihat Isi Direktori Snort

```bash
ls /etc/snort
```

### 5.2 Membuat atau Mengedit Custom Rule

```bash
sudo nano /etc/snort/rules/local.rules
```

Setelah selesai mengedit: `Ctrl+X` → `Y` → `Enter` untuk menyimpan.

### 5.3 Menjalankan Snort untuk Deteksi Real-Time

```bash
sudo snort -q -l /var/log/snort -i lo -A alert_fast -c /etc/snort/snort.lua -R /etc/snort/rules/local.rules
```

| Flag | Fungsi |
|---|---|
| `-q` | Quiet mode — menyembunyikan banner dan info yang tidak relevan |
| `-l /var/log/snort` | Direktori penyimpanan file log output |
| `-i lo` | Network interface yang dipantau (ganti `lo` dengan nama interface yang sesuai) |
| `-A alert_fast` | Format output alert yang ringkas dan cepat |
| `-c /etc/snort/snort.lua` | Path ke file konfigurasi utama Snort |
| `-R /etc/snort/rules/local.rules` | Muat tambahan rule dari file ini (berguna jika local.rules belum didaftarkan di snort.lua) |

### 5.4 Menjalankan Snort terhadap File PCAP

```bash
sudo snort -q -l /var/log/snort -r Intro_to_IDS.pcap -A alert_fast -c /etc/snort/snort.lua -R /etc/snort/rules/local.rules
```

| Flag | Fungsi |
|---|---|
| `-r <file.pcap>` | Membaca traffic dari file PCAP alih-alih interface live |

Semua flag lainnya sama seperti mode real-time di atas.

### 5.5 Merekam Traffic ke File PCAP (dengan tcpdump)

```bash
sudo tcpdump -i lo -w /tmp/latihan.pcap -c 20
```

| Flag | Fungsi |
|---|---|
| `-i lo` | Interface yang direkam |
| `-w <file>` | Simpan hasil rekaman ke file PCAP |
| `-c 20` | Hentikan otomatis setelah menangkap sejumlah paket yang ditentukan |

### 5.6 Tips Praktis

- Pastikan direktori `/var/log/snort` sudah ada sebelum menjalankan Snort: `sudo mkdir -p /var/log/snort`
- Jika menggunakan Snort 3 dan local.rules tidak otomatis terbaca, gunakan flag `-R` untuk memuatnya secara eksplisit.
- Saat menganalisis file PCAP, pindah ke direktori yang berisi file PCAP terlebih dahulu: `cd /etc/snort`

---

## 6. Skenario Praktis — Analisis Forensik PCAP

Room ini menyajikan skenario: seorang investigator forensik pihak ketiga menerima file PCAP dari sebuah perusahaan yang jaringannya diserang. Tugas investigator adalah menjalankan Snort terhadap PCAP tersebut dan mengidentifikasi aktivitas berbahaya.

Langkah-langkah yang dilakukan:

1. Tambahkan rule yang relevan ke `local.rules` (dalam kasus ini, rule untuk mendeteksi koneksi SSH ke port 22).
2. Pindah ke direktori yang berisi file PCAP.
3. Jalankan Snort dengan flag `-r` untuk membaca file PCAP.
4. Analisis output alert untuk mengidentifikasi IP penyerang, jenis serangan, dan SID rule yang terpicu.

Dari hasil analisis file `Intro_to_IDS.pcap` di room ini ditemukan:

- IP penyerang yang mencoba koneksi SSH: `10.11.90.211`
- Target koneksi SSH: `10.10.161.151:22`
- Alert lain yang terdeteksi selain SSH: `Ping Detected` (traffic ICMP)
- SID rule SSH yang terpicu: `1000002`

Format baris alert Snort:
```
[timestamp] [**] [gid:sid:rev] "pesan" [**] [Priority: X] {PROTOKOL} src_ip:src_port -> dst_ip:dst_port
```

---

## 7. Glossary

**IDS (Intrusion Detection System)** — sistem keamanan yang mendeteksi aktivitas mencurigakan di dalam jaringan dan memberi notifikasi kepada administrator; tidak memblokir traffic secara langsung.

**Firewall** — sistem keamanan di batas jaringan yang memeriksa dan memblokir koneksi berdasarkan aturan yang ditetapkan.

**HIDS** — Host-based IDS; dipasang per host, memantau aktivitas host individual.

**NIDS** — Network-based IDS; memantau traffic seluruh jaringan secara terpusat.

**Signature** — pola unik dari sebuah serangan yang tersimpan dalam database IDS untuk keperluan deteksi.

**Zero-day attack** — serangan yang memanfaatkan kerentanan yang belum diketahui sebelumnya; belum memiliki signature di database IDS.

**Baseline** — perilaku normal jaringan/sistem yang dipelajari oleh anomaly-based IDS sebagai referensi deteksi.

**False positive** — kondisi ketika IDS salah menandai aktivitas yang sebenarnya aman sebagai berbahaya.

**Fine-tuning** — proses penyesuaian manual baseline pada anomaly-based IDS untuk mengurangi false positive.

**Promiscuous mode** — mode network interface yang memungkinkan perangkat menangkap semua paket yang melintas di jaringan, bukan hanya yang ditujukan untuknya.

**PCAP** — format standar file rekaman traffic jaringan (packet capture); dapat dianalisis ulang oleh Snort menggunakan flag `-r`.

**SID (Signature ID)** — identifier unik numerik yang membedakan satu rule Snort dari rule lainnya.

**$HOME_NET** — variabel di Snort yang merepresentasikan range IP jaringan internal; nilainya didefinisikan di `snort.lua`.

**alert_fast** — format output alert Snort yang ringkas, menampilkan informasi penting per baris secara cepat.

**Root cause analysis** — proses investigasi untuk menemukan penyebab akar dari sebuah insiden keamanan.

**Rule revision (rev)** — nomor versi sebuah rule Snort yang bertambah setiap kali rule dimodifikasi.

---

## 8. Tools & Platform Rujukan

**Snort**
- Fungsi: IDS open-source berbasis signature untuk mendeteksi intrusi pada traffic jaringan secara real-time maupun dari file PCAP.
- URL: https://www.snort.org

**tcpdump**
- Fungsi: tool command-line untuk merekam traffic jaringan ke file PCAP; tersedia secara default di sebagian besar distribusi Linux.

---

## 9. Catatan Ringkas untuk Ditulis Tangan

### IDS — Dasar

- IDS — deteksi intrusi di dalam jaringan, tidak memblokir, hanya alert
- Firewall — penjaga batas jaringan (sebelum masuk)
- IDS vs Firewall — firewall di pintu, IDS di dalam gedung

### Jenis IDS — Deployment

- HIDS — per host, detail tapi sulit dikelola di jaringan besar
- NIDS — level jaringan, terpusat, pantau semua host sekaligus

### Jenis IDS — Deteksi

- Signature-based — cocokkan dengan database pola; cepat, tidak bisa deteksi zero-day
- Anomaly-based — belajar baseline normal; bisa zero-day, rawan false positive
- Hybrid — gabungan keduanya; dikenal pakai signature, baru pakai anomaly
- False positive — aktivitas normal salah ditandai berbahaya
- Fine-tuning — setel ulang baseline untuk kurangi false positive

### Snort — Umum

- Snort — IDS open-source, sejak 1998, signature + anomaly
- Direktori utama — `/etc/snort/`
- Config utama — `snort.lua`
- Custom rule — `rules/local.rules`
- $HOME_NET — variabel range jaringan internal, didefinisikan di snort.lua

### Snort — Mode Operasi

- Packet Sniffer — tampilkan paket, tanpa analisis, untuk troubleshooting
- Packet Logging — catat traffic ke PCAP, untuk forensik pasca-serangan
- NIDS — pantau real-time + alert; mode utama IDS

### Format Rule Snort

```
action protocol src_ip src_port -> dst_ip dst_port (msg:".."; sid:X; rev:X;)
```

- action — `alert`
- protocol — `tcp` / `udp` / `icmp` / `ip` (bukan nama aplikasi)
- `->` — arah traffic, HARUS minus+lebihbesar, bukan karakter panah unicode
- $HOME_NET — IP tujuan = jaringan internal
- msg — pesan alert yang muncul
- sid — ID unik tiap rule, tidak boleh duplikat
- rev — nomor versi rule, nambah tiap rule diubah

### Contoh Rule

- ICMP ke loopback: `alert icmp any any -> 127.0.0.1 any (msg:"Loopback Ping Detected"; sid:10003; rev:1;)`
- SSH ke internal: `alert tcp any any -> $HOME_NET 22 (msg:"SSH Detected"; sid:10001; rev:1;)`

### Command Penting

- `ls /etc/snort` — lihat isi direktori Snort
- `sudo nano /etc/snort/rules/local.rules` — edit custom rule
- `sudo mkdir -p /var/log/snort` — buat direktori log jika belum ada

Snort real-time:
```
sudo snort -q -l /var/log/snort -i lo -A alert_fast -c /etc/snort/snort.lua -R /etc/snort/rules/local.rules
```

Snort analisis PCAP:
```
sudo snort -q -l /var/log/snort -r file.pcap -A alert_fast -c /etc/snort/snort.lua -R /etc/snort/rules/local.rules
```

Rekam PCAP dengan tcpdump:
```
sudo tcpdump -i lo -w /tmp/file.pcap -c 20
```

### Flag Snort — Ringkas

- `-q` — quiet, sembunyikan banner
- `-l <dir>` — direktori simpan log
- `-i <iface>` — interface live yang dipantau
- `-r <file.pcap>` — baca dari file PCAP (bukan live)
- `-A alert_fast` — format alert ringkas
- `-c <snort.lua>` — path file konfigurasi
- `-R <rules>` — muat file rule tambahan

### Format Output Alert

```
[timestamp] [**] [gid:sid:rev] "pesan" [**] [Priority: X] {PROTO} src -> dst
```

- src = IP penyerang (arah kiri panah)
- dst = target (arah kanan panah, biasanya ada port spesifik seperti :22)
