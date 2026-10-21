# Resume Materi — Windows Command Line (CMD)
**TryHackMe | Windows Command Line**
**Tanggal:** 21 Oktober 2026

---

## 1. Mengapa CLI

GUI lebih intuitif untuk pemula, tapi CLI unggul dalam empat hal:

- **Lower resource usage** — tidak butuh rendering grafis, cocok untuk hardware lama dan cloud computing (biaya lebih rendah)
- **Automation** — perintah bisa disusun ke dalam script/batch file untuk tugas berulang
- **Remote management** — SSH ke server/router/IoT bekerja baik bahkan di koneksi lambat
- **Speed & efficiency** — tangan tidak perlu meninggalkan keyboard

**Default interpreter Windows:** `cmd.exe`

---

## 2. Tips Umum CMD

Dua hal yang selalu berlaku di hampir semua perintah:

```
perintah /?
```
Menampilkan halaman bantuan lengkap untuk perintah tersebut, termasuk semua switch yang tersedia.

```
perintah | more
```
Menyalurkan output panjang agar tampil halaman per halaman. Spacebar untuk next page, Enter untuk next line, CTRL+C untuk keluar.

Perintah utilitas dasar:

| Perintah | Fungsi |
|----------|--------|
| `cls` | Bersihkan layar CMD |
| `help` | Tampilkan info bantuan untuk perintah tertentu |
| `more file.txt` | Baca isi file teks halaman per halaman |

---

## 3. System Information

### 3.1 Perintah Dasar

```
set
```
Menampilkan semua **environment variable** yang aktif di sistem, termasuk baris `Path=` yang menunjukkan direktori mana saja yang diakui Windows sebagai tempat mencari executable.

```
ver
```
Menampilkan versi OS Windows secara singkat.

```
systeminfo
```
Menampilkan informasi sistem secara mendalam: nama host, versi OS, manufacturer, konfigurasi, tipe build, prosesor, dan memori.

### 3.2 Konsep PATH

**Windows PATH** adalah daftar direktori tempat CMD mencari file executable. Perintah hanya bisa dijalankan langsung jika file-nya berada di salah satu direktori dalam PATH. Di luar itu, harus dituliskan path lengkapnya.

---

## 4. Network Configuration

### 4.1 ipconfig

```
ipconfig
```
Menampilkan informasi jaringan dasar: IP address, subnet mask, dan default gateway untuk setiap adapter.

```
ipconfig /all
```
Menampilkan informasi lengkap: semua yang ada di `ipconfig` ditambah MAC address (Physical Address), status DHCP, DNS server, waktu lease obtained dan expires, serta NetBIOS status.

### 4.2 Perbedaan ipconfig vs ipconfig /all

`ipconfig` cukup untuk cek IP cepat. `ipconfig /all` dipakai saat butuh troubleshoot DHCP, konfirmasi DNS server yang dipakai, atau verifikasi MAC address perangkat.

---

## 5. Network Troubleshooting

### 5.1 ping

```
ping target_name
```
Mengirimkan paket **ICMP** ke target dan menunggu balasan. Jika balasan diterima, koneksi ke target bisa dijangkau (dua arah). Output mencakup waktu round trip per paket dan statistik packet loss.

### 5.2 tracert

```
tracert target_name
```
Menelusuri rute jaringan yang dilalui untuk mencapai target, hop per hop. Setiap baris menunjukkan satu router yang dilewati beserta waktu responnya. Baris dengan `* * *` (Request timed out) berarti router tersebut tidak merespons — bukan berarti jalur terputus.

Singkatan dari: **trace route**

### 5.3 nslookup

```
nslookup domain
```
Mencari IP address dari sebuah domain menggunakan DNS server default sistem.

```
nslookup domain 1.1.1.1
```
Mencari IP address dari domain yang sama, tapi menggunakan DNS server yang ditentukan secara manual (dalam contoh ini: `1.1.1.1` / one.one.one.one). Hasil bisa identik, tapi sumber jawabannya berbeda — berguna untuk memverifikasi apakah DNS server default memberikan hasil yang benar.

### 5.4 netstat

```
netstat
```
Menampilkan koneksi jaringan yang sedang aktif (status ESTABLISHED). Tanpa switch, port LISTENING tidak ditampilkan dan alamat bisa tampil sebagai hostname.

```
netstat -h
```
Menampilkan semua opsi yang tersedia.

**Switch netstat:**

| Switch | Fungsi |
|--------|--------|
| `-a` | Tampilkan semua koneksi + semua port LISTENING |
| `-b` | Tampilkan nama executable/program di balik tiap koneksi |
| `-o` | Tampilkan PID (Process ID) tiap koneksi |
| `-n` | Tampilkan alamat dalam bentuk numerik murni (bukan hostname) |

Kombinasi paling lengkap:

```
netstat -abon
```

Menampilkan semua koneksi, nama program, PID, dan alamat numerik sekaligus.

> **Catatan:** `netstat -abon` membutuhkan hak **Administrator**. Jalankan CMD dengan Run as Administrator, atau akan muncul error "The requested operation requires elevation."

---

## 6. File & Directory Management

### 6.1 Navigasi Direktori

```
cd
```
Tanpa argumen: menampilkan posisi direktori saat ini (setara "where am I?").

```
cd target_directory
```
Pindah ke direktori yang dituju. Setara dengan double-click folder di File Explorer.

```
cd ..
```
Naik satu level ke direktori induk.

### 6.2 Melihat Isi Direktori

```
dir
```
Menampilkan isi direktori saat ini: nama file/folder, ukuran, tanggal modifikasi, dan ringkasan total.

**Switch dir:**

| Switch | Fungsi |
|--------|--------|
| `/a` | Tampilkan file tersembunyi dan system files |
| `/s` | Tampilkan file di direktori ini dan semua subdirektori |

```
tree
```
Menampilkan struktur direktori secara visual (hierarki folder tanpa detail ukuran/tanggal).

### 6.3 Membuat & Menghapus Direktori

```
mkdir directory_name
```
Membuat direktori baru. Singkatan dari **make directory**.

```
rmdir directory_name
```
Menghapus direktori. Singkatan dari **remove directory**. Direktori harus kosong agar bisa dihapus.

### 6.4 Operasi File

```
type file.txt
```
Menampilkan isi file teks langsung ke layar. Cocok untuk file pendek yang muat di jendela terminal. Untuk file panjang, gunakan `more file.txt`.

```
copy source destination
```
Menyalin file dari source ke destination. File asli tetap ada di lokasi asal.

```
move source destination
```
Memindahkan file dari source ke destination. File hilang dari lokasi asal.

```
del file
erase file
```
Menghapus file. Keduanya identik fungsinya.

### 6.5 Wildcard

Karakter `*` digunakan untuk merujuk ke banyak file sekaligus berdasarkan pola nama atau ekstensi.

Contoh:
```
copy *.md C:\Markdown
```
Menyalin semua file berekstensi `.md` ke direktori `C:\Markdown`.

---

## 7. Task & Process Management

### 7.1 tasklist

```
tasklist
```
Menampilkan semua proses yang sedang berjalan. Output mencakup kolom: Image Name, PID, Session Name, Session#, dan Mem Usage. Setara dengan Task Manager versi CLI.

```
tasklist /?
```
Menampilkan semua opsi dan filter yang tersedia.

**Filter tasklist:**

```
tasklist /FI "imagename eq nama.exe"
```
Memfilter output hanya untuk proses dengan nama yang sesuai. `/FI` = filter, `imagename eq` = kondisi "nama image sama dengan".

### 7.2 taskkill

```
taskkill /PID nomor_pid
```
Mengakhiri proses berdasarkan PID-nya. Contoh: `taskkill /PID 4567` akan mematikan proses dengan PID 4567.

---

## 8. Perintah Bonus (Tidak Dibahas Detail)

Disebutkan di kesimpulan room sebagai referensi tambahan:

| Perintah | Fungsi |
|----------|--------|
| `chkdsk` | Periksa file sistem dan volume disk dari error dan bad sector |
| `driverquery` | Tampilkan daftar semua driver perangkat yang terinstal |
| `sfc /scannow` | Scan file sistem, perbaiki korupsi jika ditemukan |

---

## 9. Glossary

**CMD / cmd.exe** — Command-line interpreter default di Windows, tempat perintah diketik dan dieksekusi.

**PATH** — Variabel sistem yang berisi daftar direktori tempat Windows mencari file executable saat perintah diketik.

**PID (Process ID)** — Nomor unik yang diberikan sistem ke setiap proses yang sedang berjalan.

**ICMP** — Internet Control Message Protocol; protokol yang dipakai `ping` untuk mengirim dan menerima paket tes koneksi.

**TTL (Time To Live)** — Batas maksimal jumlah router yang boleh dilewati satu paket sebelum dibuang.

**Hop** — Satu titik router yang dilewati dalam perjalanan paket ke tujuan, tampak sebagai satu baris di output `tracert`.

**DHCP** — Protokol yang secara otomatis memberikan IP address kepada perangkat yang terhubung ke jaringan.

**LISTENING** — Status port yang aktif dan menunggu koneksi masuk.

**ESTABLISHED** — Status koneksi yang sudah terbentuk dan sedang aktif berlangsung.

**Environment Variable** — Variabel yang menyimpan konfigurasi sistem seperti PATH, nama user, jumlah prosesor, dll. Bisa dilihat dengan `set`.

**Executable** — File program yang bisa langsung dijalankan (berekstensi .exe, .cmd, .bat, dll).

**Wildcard** — Karakter khusus (`*`) yang mewakili satu atau lebih karakter dalam nama file, digunakan untuk operasi pada banyak file sekaligus.

**Elevation** — Hak akses Administrator yang dibutuhkan untuk menjalankan perintah tertentu di Windows. Diperoleh dengan Run as Administrator.

---

## 10. Tools & Platform Rujukan

Tidak ada tools/platform eksternal yang secara eksplisit dirujuk dengan URL di room ini. Room ini murni berbasis perintah bawaan Windows (built-in CMD commands).

---

## 11. Catatan Ringkas untuk Ditulis Tangan

### Kenapa CLI

- Lower resource — tidak butuh grafis
- Automation — bisa dijadikan script
- Remote mgmt — SSH ke server/IoT
- Speed — tangan tidak perlu angkat dari keyboard
- Default CMD Windows — cmd.exe

### Tips Umum

- perintah /? — tampilkan bantuan perintah
- perintah | more — output panjang halaman per halaman
- Spacebar = next page, Enter = next line, CTRL+C = keluar
- cls — bersihkan layar
- help — info bantuan

### System Info

- set — lihat semua env variable + PATH
- ver — versi Windows
- systeminfo — info lengkap OS, hardware, memori
- PATH — daftar folder tempat CMD cari executable

### Network Config

- ipconfig — IP, subnet mask, gateway
- ipconfig /all — + MAC, DHCP status, DNS, lease time

### Network Troubleshoot

- ping target — cek koneksi pakai ICMP
- tracert target — lacak rute hop per hop, * = router diam bukan putus
- nslookup domain — cari IP via DNS default
- nslookup domain 1.1.1.1 — cari IP via DNS manual
- netstat — koneksi ESTABLISHED saja
- netstat -a — + port LISTENING
- netstat -b — + nama program/executable
- netstat -o — + PID tiap koneksi
- netstat -n — alamat numerik (bukan hostname)
- netstat -abon — semua sekaligus, butuh Admin

### File & Directory

- cd — posisi saat ini
- cd target — pindah ke folder
- cd .. — naik satu level
- dir — isi direktori
- dir /a — + file tersembunyi & system
- dir /s — + semua subdirektori
- tree — struktur folder visual
- mkdir nama — buat folder
- rmdir nama — hapus folder (harus kosong)
- type file.txt — tampilkan isi file
- copy A B — salin file (asli tetap ada)
- move A tujuan — pindah file (asli hilang)
- del / erase file — hapus file
- * — wildcard, contoh: copy *.md C:\Markdown

### Task & Process

- tasklist — semua proses berjalan (= Task Manager CLI)
- tasklist /? — lihat semua opsi
- tasklist /FI "imagename eq sshd.exe" — filter by nama program
- taskkill /PID 4567 — matikan proses by PID

### Bonus Command

- chkdsk — periksa disk dari error/bad sector
- driverquery — daftar driver terinstal
- sfc /scannow — scan & perbaiki file sistem rusak

### Istilah Kunci

- PID — nomor unik tiap proses
- ICMP — protokol yang dipakai ping
- TTL — batas hop paket sebelum dibuang
- Hop — satu router yang dilewati di tracert
- DHCP — protokol pemberi IP otomatis
- LISTENING — port nunggu koneksi masuk
- ESTABLISHED — koneksi aktif terbentuk
- Elevation — butuh Run as Administrator
- Wildcard * — mewakili banyak karakter/file
- PATH — daftar direktori pencarian executable
