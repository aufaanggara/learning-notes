# Linux Fundamentals Part 3 — Resume Materi
**TryHackMe | 25 April 2026**

---

## 1. Terminal Text Editors

Sebelumnya, teks disimpan ke file menggunakan kombinasi `echo` dan operator pipe (`>` / `>>`). Cara ini tidak efisien untuk file multi-baris. Solusinya adalah menggunakan terminal text editor.

### 1.1 Nano

Editor terminal yang ramah pemula — langsung bisa dipakai tanpa perlu belajar mode atau konfigurasi khusus.

Membuka atau membuat file:

```bash
nano filename
```

Navigasi: tombol panah untuk pindah baris, Enter untuk baris baru.

Shortcut (tanda `^` = Ctrl, `M-` = Alt):

| Shortcut | Fungsi |
|---|---|
| `^G` | Get Help |
| `^O` | Simpan file (Write Out) |
| `^X` | Keluar |
| `^W` | Cari teks (Where Is) |
| `^K` | Potong teks (Cut) |
| `^U` | Tempel teks (Paste) |
| `^R` | Baca file lain |
| `^\` | Ganti teks (Replace) |
| `^C` | Lihat posisi kursor |
| `^_` | Loncat ke nomor baris |
| `M-U` | Undo |
| `M-E` | Redo |
| `M-A` | Tandai teks (Mark) |
| `M-6` | Salin teks (Copy) |

### 1.2 VIM

Editor yang jauh lebih advanced. Tidak wajib dikuasai penuh, tapi penting dikenal karena tersedia di hampir semua sistem Linux, termasuk yang tidak punya Nano.

Keunggulan VIM dibanding Nano:

- **Customisable** — shortcut keyboard bisa diubah sepenuhnya sesuai preferensi
- **Syntax Highlighting** — kode diwarnai otomatis per bahasa pemrograman, populer di kalangan developer
- **Universal** — berjalan di semua terminal, bahkan yang tidak memiliki Nano terinstall
- Tersedia banyak cheatsheet, tutorial, dan referensi online

TryHackMe memiliki room khusus untuk belajar VIM lebih dalam.

---

## 2. General Utilities

### 2.1 Wget — Download File dari Internet

Mengunduh file dari web via HTTP, setara dengan mengakses file lewat browser tetapi dari terminal.

```bash
wget [URL]
```

Contoh:

```bash
wget https://assets.tryhackme.com/additional/linux-fundamentals/part3/myfile.txt
```

### 2.2 SCP — Secure Copy (Transfer File Antar Mesin)

Transfer file antara dua komputer menggunakan protokol **SSH**, yang memberikan autentikasi sekaligus enkripsi. Berbeda dengan `cp` biasa yang hanya untuk lokal.

SCP bekerja dengan model **SOURCE → DESTINATION**.

Mengirim file dari lokal ke remote:

```bash
scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt
```

Mengambil file dari remote ke lokal:

```bash
scp ubuntu@192.168.1.30:/home/ubuntu/documents.txt notes.txt
```

Format umum:

```bash
scp [user@IP:/path/file] [tujuan]
```

Syarat: harus mengetahui username dan password dari mesin remote.

### 2.3 Python3 HTTP Server — Jadikan Mesin sebagai Web Server

Ubuntu/Kali sudah memiliki Python3 secara bawaan. Modul `http.server` mengubah mesin menjadi web server sederhana sehingga file dapat diunduh oleh mesin lain menggunakan `wget` atau `curl`.

Menjalankan server (dari direktori yang ingin disajikan):

```bash
python3 -m http.server
```

Server aktif di port **8000** secara default. Mengunduh file dari mesin lain:

```bash
wget http://[IP_SERVER]:8000/namafile
```

Catatan penting:

- Buka **terminal baru** untuk menjalankan `wget` — terminal yang menjalankan server harus tetap aktif
- Server menyajikan file dari direktori tempat perintah dijalankan
- **Tidak ada fitur indexing** — harus tahu nama dan lokasi file secara persis
- Alternatif lebih canggih: **Updog** (web server ringan dengan fitur tambahan)

---

## 3. Processes 101

### 3.1 Apa Itu Proses

**Proses** adalah program yang sedang berjalan di mesin. Setiap proses dikelola oleh **kernel** dan memiliki **PID (Process ID)** — nomor unik yang bertambah secara urut sesuai urutan proses dimulai. Proses ke-60 akan memiliki PID 60.

### 3.2 Melihat Proses

Menampilkan proses sesi user saat ini:

```bash
ps
```

Kolom output: `PID`, `TTY`, `TIME`, `CMD`

Menampilkan semua proses termasuk milik user lain dan proses sistem:

```bash
ps aux
```

Kolom tambahan: `USER`, `%CPU`, `%MEM`, `VSZ`, `RSS`, `STAT`, `START`, `COMMAND`

Menampilkan statistik proses secara real-time (refresh tiap 10 detik):

```bash
top
```

Navigasi di dalam `top` menggunakan tombol panah.

### 3.3 Menghentikan Proses

```bash
kill [PID]
```

Contoh: `kill 1337`

Tiga sinyal utama yang bisa dikirim:

| Sinyal | Perilaku |
|---|---|
| **SIGTERM** | Hentikan proses, tapi izinkan cleanup terlebih dahulu |
| **SIGKILL** | Hentikan paksa, tanpa cleanup apapun |
| **SIGSTOP** | Pause/tunda proses (belum dihentikan) |

### 3.4 Bagaimana Proses Dimulai

Sistem Operasi menggunakan **namespaces** untuk membagi sumber daya (CPU, RAM, prioritas) ke proses-proses. Proses dalam namespace yang sama dapat saling melihat satu sama lain — ini menjadi dasar isolasi dan keamanan antar proses.

Proses pertama yang berjalan saat sistem boot memiliki **PID 0**, yang kemudian melahirkan **PID 1** yaitu **systemd** (pada Ubuntu). systemd berfungsi sebagai pengelola proses pengguna dan perantara antara OS dan user.

Semua program yang dijalankan setelah boot berjalan sebagai **child process** dari systemd — dikendalikan olehnya, berbagi sumber dayanya, tapi berjalan sebagai proses mandiri.

### 3.5 Systemctl — Kontrol Service

`systemctl` digunakan untuk berinteraksi dengan proses/daemon **systemd**.

```bash
systemctl [option] [service]
```

| Option | Fungsi |
|---|---|
| `start` | Jalankan service sekarang |
| `stop` | Hentikan service sekarang |
| `enable` | Aktifkan agar otomatis jalan saat boot |
| `disable` | Nonaktifkan dari boot otomatis |
| `status` | Cek kondisi service saat ini |

Contoh:

```bash
systemctl start apache2
systemctl enable apache2
```

Perbedaan `start` vs `enable`: `start` hanya menyalakan saat itu, sementara `enable` mendaftarkan service agar otomatis aktif setiap kali sistem boot.

### 3.6 Background & Foreground

Proses bisa berjalan dalam dua kondisi:

- **Foreground** — proses aktif dan output tampil langsung di terminal; terminal tidak bisa dipakai untuk hal lain
- **Background** — proses berjalan di belakang; terminal tetap bebas digunakan

Menjalankan langsung ke background dengan operator `&`:

```bash
echo "Hi THM" &
```

Output yang dikembalikan adalah **PID proses**, bukan output perintah itu sendiri.

Memindahkan proses yang sedang berjalan ke background (sekaligus mem-pause-nya):

```bash
Ctrl + Z
```

Membawa proses background kembali ke foreground:

```bash
fg
```

Mengecek proses background yang berjalan:

```bash
ps aux
```

---

## 4. Automation — Crontab

### 4.1 Konsep Dasar

**Cron** adalah proses yang dimulai saat boot dan bertanggung jawab mengelola semua **cron job** (tugas terjadwal otomatis). **Crontab** adalah file khusus berisi daftar tugas yang harus dijalankan beserta jadwalnya.

### 4.2 Format Crontab

Setiap baris crontab terdiri dari 6 nilai:

```
MIN  HOUR  DOM  MON  DOW  CMD
```

| Field | Keterangan | Rentang |
|---|---|---|
| MIN | Menit | 0–59 |
| HOUR | Jam | 0–23 |
| DOM | Tanggal dalam bulan | 1–31 |
| MON | Bulan | 1–12 |
| DOW | Hari dalam seminggu | 0–7 (0 & 7 = Minggu) |
| CMD | Perintah yang dijalankan | — |

**Wildcard `*`** berarti "setiap nilai" — tidak ada batasan untuk field tersebut.

Contoh — backup folder Documents setiap 12 jam:

```
0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/
```

**`@reboot`** — shortcut khusus yang berarti "jalankan sekali saat sistem boot":

```
@reboot /var/opt/processes.sh
```

### 4.3 Mengelola Crontab

Membuka dan mengedit crontab:

```bash
crontab -e
```

Sistem akan meminta memilih editor (misalnya Nano). Setelah disimpan, cron otomatis mengikuti jadwal baru.

---

## 5. Package Management

### 5.1 Konsep Repository & APT

Ketika developer ingin mendistribusikan software ke komunitas Linux, mereka mengirimkannya ke **repositori apt**. Jika disetujui, software tersebut bisa diinstall oleh semua pengguna.

**apt** adalah package manager Ubuntu/Debian yang menyediakan rangkaian tool lengkap untuk mengelola paket dan sumber software. Keunggulan utamanya: setiap kali sistem diupdate, semua repositori yang terdaftar — termasuk yang ditambahkan secara manual — ikut dicek untuk pembaruan.

File dan direktori penting:

| Path | Fungsi |
|---|---|
| `/etc/apt/sources.list` | Daftar repositori utama sistem |
| `/etc/apt/sources.list.d/` | Direktori untuk file repositori tambahan |
| `/etc/apt/trusted.gpg.d/` | Direktori GPG key yang dipercaya sistem |

### 5.2 GPG Key

**GPG (Gnu Privacy Guard) key** adalah kunci kriptografi dari developer sebagai bukti keaslian software mereka. Jika kunci tidak cocok dengan yang dipercaya sistem, software ditolak dan tidak akan didownload. Ini adalah mekanisme keamanan utama saat menambahkan repositori pihak ketiga.

### 5.3 Perintah APT

```bash
apt update                              # Perbarui daftar package dari semua repo
apt install [nama-software]             # Install software
apt remove [nama-software]              # Hapus software
add-apt-repository [repo]              # Tambah repositori baru
add-apt-repository --remove ppa:PPA_Name/ppa   # Hapus repositori
```

### 5.4 Cara Menambah Repositori Pihak Ketiga (Manual)

Langkah-langkah, menggunakan Sublime Text sebagai contoh:

**Langkah 1** — Download dan simpan GPG key:

```bash
wget -qO /etc/apt/trusted.gpg.d/sublimehq.gpg https://download.sublimetext.com/sublimehq-pub.gpg
```

**Langkah 2** — Buat file list untuk repositori baru:

```bash
touch /etc/apt/sources.list.d/sublime-text.list
```

**Langkah 3** — Isi file dengan alamat repositori menggunakan Nano:

```bash
nano /etc/apt/sources.list.d/sublime-text.list
```

Isi yang dimasukkan:

```
deb https://download.sublimetext.com/ apt/stable/
```

**Langkah 4** — Update apt agar mengenali repositori baru:

```bash
apt update
```

**Langkah 5** — Install software:

```bash
apt install sublime-text
```

Catatan: Pada sistem modern (Debian/Kali terbaru), `apt-key` sudah deprecated. Gunakan metode simpan langsung ke `/etc/apt/trusted.gpg.d/` seperti pada Langkah 1.

---

## 6. Logs

### 6.1 Lokasi dan Fungsi

Semua file log sistem tersimpan di direktori `/var/log/`. File dan folder di sini mencatat aktivitas semua aplikasi dan layanan yang berjalan di sistem.

Sistem Operasi mengelola log secara otomatis melalui proses yang disebut **rotating** — log lama diarsipkan dan digantikan log baru agar tidak memenuhi storage.

### 6.2 Log Penting yang Perlu Diketahui

| File Log | Fungsi |
|---|---|
| `apache2/access.log` | Setiap request yang masuk ke web server Apache |
| `apache2/error.log` | Semua error yang terjadi di web server Apache |
| `fail2ban.log` | Memantau dan mencatat percobaan brute force |
| `ufw.log` | Aktivitas firewall UFW |
| `auth.log` | Percobaan autentikasi user (login, sudo, dll) |
| `syslog` | Aktivitas OS secara umum |

### 6.3 Format access.log

```
[IP] - - [tanggal waktu timezone] "METHOD /file HTTP/versi" [status] [ukuran]
```

Contoh:

```
10.9.232.111 - - [04/May/2021:18:18:16 +0000] "GET /catsanddogs.jpg HTTP/1.1" 200 51395 ...
```

Dari baris ini bisa dibaca: IP pengunjung, waktu akses, file yang diakses, dan status HTTP.

### 6.4 Konvensi Penamaan File Log

- File tanpa ekstensi (misal `access.log`) — log aktif saat ini
- File `.1` (misal `access.log.1`) — log hari sebelumnya
- File `.2`, `.3`, dst — log yang lebih lama
- File `.gz` — log lama yang sudah dikompresi

### 6.5 Permission & Akses

Di terminal, perintah `ls` menampilkan warna berbeda untuk file berdasarkan permission-nya. File berwarna **hijau** umumnya bersifat **world-readable** (dapat dibaca semua user). File tanpa warna khusus atau yang merah mungkin memerlukan akses root.

Jika kena `Permission denied` saat membaca log:

```bash
sudo cat /var/log/apache2/access.log
```

Atau masuk sebagai root terlebih dahulu:

```bash
sudo su
```

---

## 7. Glossary

| Istilah | Definisi |
|---|---|
| **PID** | Process ID — nomor unik yang dimiliki setiap proses, bertambah urut sesuai urutan proses dimulai |
| **Kernel** | Inti sistem operasi yang mengelola semua proses dan sumber daya hardware |
| **systemd** | Proses pertama (PID 1) yang berjalan saat boot di Ubuntu; induk dari semua proses lainnya |
| **Child Process** | Proses yang dilahirkan dari dan dikendalikan oleh proses lain (umumnya systemd) |
| **Namespace** | Mekanisme OS untuk membagi dan mengisolasi sumber daya antar proses |
| **Foreground** | Kondisi proses aktif dan menguasai terminal — output tampil langsung |
| **Background** | Kondisi proses berjalan di belakang tanpa memblokir terminal |
| **Rotating** | Proses OS mengarsipkan log lama dan membuat log baru secara otomatis |
| **GPG Key** | Kunci kriptografi dari developer untuk memverifikasi keaslian software |
| **Repository** | Server yang menyimpan kumpulan software yang bisa diinstall via apt |
| **Cron Job** | Tugas yang dijadwalkan untuk dijalankan otomatis oleh proses cron |
| **Crontab** | File konfigurasi yang berisi daftar cron job beserta jadwalnya |
| **World-readable** | File yang permission-nya memungkinkan semua user membacanya |
| **SIGTERM** | Sinyal untuk menghentikan proses dengan izin cleanup terlebih dahulu |
| **SIGKILL** | Sinyal untuk menghentikan proses paksa tanpa cleanup |
| **SIGSTOP** | Sinyal untuk menjeda/menunda proses |
| **Wildcard** | Tanda `*` di crontab yang berarti "semua nilai" untuk field tersebut |
| **HTTP Server** | Server yang melayani file via protokol HTTP |

---

## 8. Tools & Platform Rujukan

| Nama | Fungsi | URL |
|---|---|---|
| Crontab Generator | Generate format crontab via antarmuka visual | https://crontab-generator.org |
| Cron Guru | Referensi dan validator jadwal crontab | https://crontab.guru |
| TryHackMe — Bash Scripting | Room lanjutan untuk belajar otomatisasi dengan Bash | https://tryhackme.com/room/bashscripting |
| TryHackMe — Regular Expressions | Room lanjutan untuk belajar regex di Linux | https://tryhackme.com/room/catregex |
| TryHackMe — VIM | Room khusus untuk belajar VIM secara mendalam | (tersedia di TryHackMe) |
| Updog | Web server ringan alternatif python3 http.server dengan fitur indexing | (install via pip) |

---

## 9. Catatan Ringkas untuk Ditulis Tangan

### Terminal Text Editors

- nano filename — buka/buat file dengan Nano
- ^O — simpan | ^X — keluar | ^W — cari | ^K — cut | ^U — paste
- ^_ — loncat ke baris | M-U — undo | M-E — redo
- VIM — lebih advanced, syntax highlighting, customisable, universal

### Utilities

- wget [URL] — download file dari internet
- scp file user@IP:/path/tujuan — kirim file ke remote via SSH
- scp user@IP:/path/file lokal.txt — ambil file dari remote
- python3 -m http.server — jadikan mesin sebagai web server port 8000
- wget http://IP:8000/file — download dari python http server
- Buka terminal baru untuk wget, server harus tetap aktif

### Processes

- ps — lihat proses sesi aktif
- ps aux — lihat semua proses (semua user + sistem)
- top — monitor proses real-time (refresh 10 detik)
- kill [PID] — hentikan proses
- SIGTERM — hentikan + izinkan cleanup
- SIGKILL — hentikan paksa tanpa cleanup
- SIGSTOP — pause/tunda proses
- PID 1 = systemd = induk semua proses di Ubuntu
- Namespace = isolasi sumber daya antar proses

### Systemctl

- systemctl start [service] — nyalakan sekarang
- systemctl stop [service] — matikan sekarang
- systemctl enable [service] — aktifkan saat boot
- systemctl disable [service] — nonaktifkan dari boot
- systemctl status [service] — cek kondisi

### Background & Foreground

- [perintah] & — jalankan langsung ke background
- Ctrl + Z — background + pause proses yang sedang jalan
- fg — bawa kembali ke foreground
- ps aux — cek proses yang berjalan di background

### Crontab

- Format: MIN HOUR DOM MON DOW CMD
- * (wildcard) = setiap nilai, tidak ada batasan
- @reboot = jalankan saat sistem boot
- crontab -e — edit crontab
- Contoh setiap 12 jam: 0 */12 * * * [command]

### Package Management

- apt update — perbarui daftar package
- apt install [nama] — install software
- apt remove [nama] — hapus software
- add-apt-repository [repo] — tambah repo
- /etc/apt/sources.list — daftar repo utama
- /etc/apt/sources.list.d/ — repo tambahan (1 file per repo)
- /etc/apt/trusted.gpg.d/ — GPG key terpercaya
- GPG key = kunci verifikasi keaslian software dari developer
- apt-key sudah deprecated di sistem modern, simpan langsung ke trusted.gpg.d

### Logs

- Lokasi: /var/log/
- Rotating = OS arsipkan log lama otomatis
- access.log = semua request ke web server (IP, waktu, file, status)
- error.log = semua error web server
- fail2ban.log = percobaan brute force
- ufw.log = aktivitas firewall
- auth.log = percobaan login/autentikasi
- Format access.log: IP - - [waktu] "METHOD /file HTTP/x" status ukuran
- File .1 = kemarin | .2 = dua hari lalu | .gz = terkompresi
- File hijau di ls = world-readable (bisa dibaca semua user)
