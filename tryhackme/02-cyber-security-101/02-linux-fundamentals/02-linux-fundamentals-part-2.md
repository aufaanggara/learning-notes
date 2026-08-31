# Resume Materi — Linux Fundamentals Part 2
**Tanggal:** 23 April 2026

---

## 1. SSH (Secure Shell)

**SSH** (Secure Shell) adalah protokol komunikasi terenkripsi yang digunakan untuk terhubung dan mengontrol terminal mesin Linux dari jarak jauh. Ini adalah cara standar industri untuk remote access ke server Linux.

### 1.1 Cara Kerja

Setiap input yang dikirim dalam format terbaca manusia akan dienkripsi sebelum melintasi jaringan, lalu didekripsi kembali saat tiba di mesin tujuan. Artinya data aman selama perjalanan meski melewati jaringan publik seperti internet.

Dua hal yang wajib ada sebelum bisa SSH:

- **IP address** mesin remote yang ingin dituju
- **Kredensial valid** — username dan password akun di mesin tersebut

### 1.2 Sintaks dan Penggunaan

```bash
ssh username@IP_ADDRESS
```

Contoh konkret:

```bash
ssh tryhackme@10.10.10.10
```

Setelah perintah dijalankan, terminal akan meminta konfirmasi trust host (jawab `yes`), lalu meminta password.

**Catatan kritis:** Saat mengetik password di prompt SSH, tidak akan ada karakter atau simbol yang muncul di layar. Ini fitur keamanan bawaan, bukan error — tetap ketik password lalu tekan Enter.

Setelah berhasil terhubung, semua perintah yang dijalankan akan dieksekusi di **mesin remote**, bukan mesin lokal.

---

## 2. Flags dan Switches

Sebagian besar perintah Linux menerima **argumen tambahan** berupa flags atau switches untuk memperluas perilaku defaultnya. Tanpa flag, perintah berjalan sesuai perilaku bawaannya saja.

### 2.1 Format Penulisan

Flag ditulis dengan tanda **hyphen (-)** diikuti huruf atau kata kunci:

- Format pendek: `-a`
- Format panjang: `--all`
- Gabungan beberapa flag: `-lh` (setara dengan `-l -h`)

### 2.2 Cara Mengetahui Flag yang Tersedia

**--help**

```bash
ls --help
```

Menampilkan ringkasan semua opsi yang diterima perintah tersebut, beserta deskripsi singkat dan cara pakainya. Output ini adalah versi terformat dari man page.

**man (manual page)**

```bash
man ls
```

Membuka dokumentasi lengkap perintah. Man page lebih detail dari `--help` dan mencakup semua opsi, contoh, dan penjelasan mendalam.

### 2.3 Navigasi di Man Page

| Tombol | Fungsi |
|--------|--------|
| `j` / panah bawah | Scroll satu baris ke bawah |
| `k` / panah atas | Scroll satu baris ke atas |
| `Space` | Loncat satu halaman penuh ke bawah |
| `q` | Keluar dari man page |

---

## 3. Flag Penting Perintah ls

```bash
ls -a
```
Tampilkan **semua** file termasuk yang tersembunyi (file/folder berawalan titik `.`).

```bash
ls -l
```
Tampilkan isi direktori dalam format **list detail** — menampilkan permission, owner, group, ukuran, tanggal, dan nama file.

```bash
ls -h
```
Tampilkan ukuran file dalam format **human-readable** (contoh: 1K, 2M, 3G) alih-alih bytes mentah.

```bash
ls -lh
```
Gabungan `-l` dan `-h` — format list detail dengan ukuran yang mudah dibaca. Kombinasi yang paling sering dipakai.

### 3.1 Cara Membaca Output ls -lh

```
-rw-r--r-- 1 cmnatic cmnatic 0 Feb 19 10:37 file1
```

Urutan kolom dari kiri ke kanan:

| Kolom | Contoh | Keterangan |
|-------|--------|------------|
| Permission | `-rw-r--r--` | Hak akses file (10 karakter) |
| Hard link | `1` | Jumlah hard link |
| Owner | `cmnatic` | Pemilik file |
| Group | `cmnatic` | Grup yang memiliki akses |
| Size | `0` | Ukuran file |
| Tanggal | `Feb 19 10:37` | Terakhir dimodifikasi |
| Nama file | `file1` | Nama file/folder |

---

## 4. File Permissions

Linux mengatur hak akses setiap file dan folder secara granular melalui sistem permission. Tiga jenis tindakan yang bisa diatur:

- **Read (r)** — membaca isi file
- **Write (w)** — menulis/mengedit file
- **Execute (x)** — menjalankan file sebagai program/script

### 4.1 Format Symbolic (rwxrwxrwx)

Permission ditampilkan dalam 9 karakter yang dibagi ke tiga kelompok:

| Posisi | Berlaku Untuk | Contoh |
|--------|---------------|--------|
| 3 pertama | Owner (pemilik) | `rwx` |
| 3 tengah | Group | `r-x` |
| 3 terakhir | Others (semua orang lain) | `r--` |

Tanda `-` berarti tidak memiliki permission tersebut.

### 4.2 Konversi ke Format Numerik

Setiap permission memiliki nilai:

- **r = 4**
- **w = 2**
- **x = 1**

Nilai dijumlahkan per kelompok untuk mendapat satu digit angka. Tiga digit angka mewakili owner, group, dan others secara berurutan.

Contoh `rwxrwxrwx`:

- Owner: 4+2+1 = **7**
- Group: 4+2+1 = **7**
- Others: 4+2+1 = **7**
- Hasil: **777**

### 4.3 Referensi Cepat Kombinasi Umum

| Symbolic | Numeric | Arti |
|----------|---------|------|
| `rwxr-xr-x` | 755 | Owner full access; group & others hanya read+execute |
| `rw-r--r--` | 644 | Owner read+write; others hanya read |
| `rwx------` | 700 | Hanya owner yang punya akses apapun |

### 4.4 Mengubah Permission

```bash
chmod 750 namafile
```

Angka pertama (7) = owner, kedua (5) = group, ketiga (0) = others.

### 4.5 Users vs Groups

Linux memungkinkan permission yang **sangat granular**: sebuah file bisa punya permission berbeda untuk ownernya, grupnya, dan semua orang lain — tanpa saling mempengaruhi. Ini penting untuk keamanan, misalnya web server bisa punya akses baca/tulis ke file tertentu, sementara user lain tidak.

---

## 5. Perintah Interaksi Filesystem

### 5.1 Membuat File dan Folder

```bash
touch namafile
```
Membuat **file kosong** baru. Perintah ini hanya membuat file tanpa isi — untuk menambah konten gunakan `echo` atau text editor seperti `nano`.

```bash
mkdir namafolder
```
Membuat **direktori/folder** baru.

### 5.2 Menyalin dan Memindahkan

```bash
cp file_asal file_tujuan
```
Menyalin file. File asli **tetap ada**, hasil salinan muncul sebagai file baru. Menerima dua argumen: nama file sumber dan nama file hasil salinan.

```bash
mv file_asal file_tujuan
```
Memindahkan atau mengganti nama file. File asli **hilang dari lokasi semula** — setara dengan Cut di Windows. Bisa juga digunakan untuk rename.

```bash
mv namafile namafolder/
```
Memindahkan file ke dalam folder tertentu.

### 5.3 Menghapus File dan Folder

```bash
rm namafile
```
Menghapus file secara **permanen**. Tidak ada recycle bin di Linux — file langsung hilang.

```bash
rm -R namafolder
```
Menghapus folder beserta **seluruh isinya** secara rekursif. Flag `-R` wajib dipakai untuk menghapus direktori.

### 5.4 Menentukan Tipe File

```bash
file namafile
```
Menampilkan tipe/format sebuah file. File di Linux tidak harus punya extension — perintah ini membaca konten aktual file untuk menentukan jenisnya.

Contoh output:

```
note: ASCII text
```

**Protip:** Semua perintah di atas mendukung full path sebagai argumen, misalnya `cp directory1/directory2/note .`

---

## 6. Berpindah Antar User (su)

### 6.1 Sintaks Dasar

```bash
su namauser
```

Syarat yang diperlukan (kecuali kamu adalah root):

- Nama user yang ingin dituju
- Password user tersebut

### 6.2 Switch Penting

```bash
su -l namauser
```
atau

```bash
su --login namauser
```

Dengan flag `-l`, sesi baru **mewarisi properti lengkap** user tujuan (environment variables, home directory, dll) — seolah user tersebut yang baru saja login langsung ke sistem.

**Perbedaan perilaku:**

| Perintah | Direktori Setelah Switch |
|----------|--------------------------|
| `su user2` | Tetap di home directory user sebelumnya |
| `su -l user2` | Pindah ke home directory user2 (`/home/user2`) |

### 6.3 Catatan Tambahan

Man page `su` berisi switch-switch lain yang relevan seperti menentukan shell spesifik atau menjalankan perintah langsung setelah login. Dianjurkan untuk membacanya dengan `man su`.

---

## 7. Direktori Penting di Linux

### 7.1 /etc

Kependekan dari **etcetera**. Lokasi penyimpanan file-file konfigurasi dan sistem yang digunakan oleh operating system.

File-file kunci di `/etc`:

- **sudoers** — daftar user dan group yang boleh menjalankan perintah sebagai root via `sudo`
- **passwd** — daftar semua user di sistem
- **shadow** — password setiap user dalam format enkripsi **sha512**

```bash
ls /etc
# shadow passwd sudoers sudoers.d
```

### 7.2 /var

Kependekan dari **variable data**. Menyimpan data yang sering diakses atau ditulis oleh services dan aplikasi yang berjalan di sistem.

Subfolder penting:

- **/var/log** — semua log file dari services dan aplikasi disimpan di sini
- **/var/backups**, **/var/opt**, **/var/tmp** — data lain yang tidak terikat user tertentu

```bash
ls /var
# backups log opt tmp
```

### 7.3 /root

Home directory khusus milik **root user** (superuser/administrator sistem). Berbeda dari `/home` yang dipakai user biasa — root tidak menggunakan `/home/root`, melainkan langsung `/root`.

```bash
# Hanya bisa diakses oleh root user
ls /root
# myfile myfolder passwords.xlsx
```

### 7.4 /tmp

Kependekan dari **temporary**. Direktori unik yang bersifat **volatile** — seluruh isinya terhapus otomatis setiap kali sistem di-restart, mirip cara kerja RAM.

Karakteristik penting: **semua user bisa menulis ke `/tmp` secara default**, tanpa perlu permission khusus.

Relevansi dalam pentesting: `/tmp` sering digunakan sebagai lokasi penyimpanan sementara untuk tools atau enumeration scripts setelah mendapat akses ke mesin target.

```bash
ls /tmp
# todelete trash.txt rubbish.bin
```

---

## 8. Ringkasan Seluruh Perintah

| Perintah | Fungsi |
|----------|--------|
| `ssh user@IP` | Remote login ke mesin Linux via SSH |
| `ls` | Tampilkan isi direktori |
| `ls -a` | Tampilkan semua file termasuk tersembunyi |
| `ls -l` | Tampilkan format list detail |
| `ls -lh` | Format list detail dengan ukuran human-readable |
| `man perintah` | Buka manual lengkap sebuah perintah |
| `perintah --help` | Tampilkan ringkasan opsi perintah |
| `touch namafile` | Buat file kosong baru |
| `mkdir namafolder` | Buat folder baru |
| `cp asal tujuan` | Salin file (asli tetap ada) |
| `mv asal tujuan` | Pindah/rename file (asli hilang) |
| `rm namafile` | Hapus file secara permanen |
| `rm -R namafolder` | Hapus folder beserta seluruh isinya |
| `file namafile` | Cek tipe/format sebuah file |
| `chmod 755 namafile` | Ubah permission file dengan format numerik |
| `su namauser` | Pindah ke user lain |
| `su -l namauser` | Pindah ke user lain dengan full login environment |

---

## 9. Glossary — Istilah Kunci

**SSH (Secure Shell)** — Protokol komunikasi terenkripsi untuk remote access ke mesin Linux.

**Flag / Switch** — Argumen tambahan pada perintah, ditulis dengan tanda `-`, untuk mengubah atau memperluas perilaku default perintah tersebut.

**Man Page** — Dokumentasi lengkap built-in Linux untuk setiap perintah dan aplikasi. Diakses dengan perintah `man`.

**Hidden File** — File atau folder yang namanya diawali titik (`.`). Tidak muncul di output `ls` biasa, hanya terlihat dengan flag `-a`.

**Permission** — Hak akses (read/write/execute) yang menentukan siapa yang boleh melakukan apa terhadap sebuah file atau folder.

**Owner** — User pemilik sebuah file atau folder di Linux.

**Group** — Kumpulan user yang berbagi hak akses yang sama terhadap sebuah file atau folder.

**Execute** — Hak untuk menjalankan file sebagai program atau script.

**Recursive** — Operasi yang mencakup sebuah direktori beserta seluruh isi di dalamnya (subfolder dan file). Ditandai dengan flag `-R`.

**Volatile** — Data atau direktori yang bersifat sementara dan hilang saat sistem di-restart. Contoh: `/tmp`.

**Environment Variables** — Variabel konfigurasi yang aktif di sesi user tertentu, diwarisi saat login. Penting saat berpindah user dengan `su -l`.

**SHA512** — Algoritma enkripsi yang digunakan Linux untuk menyimpan password user di file `/etc/shadow`.

**Enumeration** — Proses identifikasi detail sistem seperti port terbuka, service yang berjalan, user yang ada, dan struktur direktori.

**Foothold** — Akses awal yang berhasil didapat ke sebuah sistem target dalam konteks pentesting.

**Privilege Escalation** — Proses menaikkan level akses dari user biasa menjadi administrator/root setelah mendapat foothold.

---

## 10. Tools & Platform Rujukan

**TryHackMe AttackBox**
Mesin Ubuntu Linux berbasis cloud yang disediakan TryHackMe, bisa diakses langsung lewat browser. Digunakan untuk berinteraksi dengan mesin target tanpa perlu setup VPN.
URL: https://tryhackme.com

**OpenVPN**
Tool untuk terhubung ke jaringan internal TryHackMe dari mesin lokal sendiri (seperti Kali Linux di VM). Diperlukan agar bisa SSH ke mesin target dari luar AttackBox.
Paket: `openvpn` — jalankan dengan `sudo openvpn namafile.ovpn`

**Nano**
Text editor berbasis terminal bawaan Linux, disebutkan sebagai salah satu cara untuk menambahkan konten ke file yang dibuat dengan `touch`.

**Linux Man Pages Online**
Versi online dari man pages Linux, bisa diakses dari browser sebagai alternatif `man` di terminal.
URL: https://man7.org/linux/man-pages/

---

## 11. Catatan Ringkas untuk Ditulis Tangan

### SSH

- SSH — remote login terenkripsi ke mesin Linux
- Butuh: IP address + username + password
- `ssh user@IP` — sintaks dasar login
- Password tidak muncul saat diketik — normal, lanjut Enter
- Setelah konek: semua perintah jalan di mesin REMOTE
- Dari VM sendiri: harus konek VPN TryHackMe dulu

### Flags & Switches

- Flag — argumen tambahan perintah, diawali `-`
- `-a` vs `--all` — versi pendek vs panjang, fungsi sama
- Gabung flag: `-lh` = `-l` + `-h` sekaligus
- `--help` — ringkasan opsi perintah
- `man perintah` — manual lengkap

### Navigasi Man Page

- `j` / panah bawah — scroll bawah
- `Space` — loncat satu halaman
- `q` — keluar

### ls Flags

- `ls -a` — tampilkan file tersembunyi (berawalan `.`)
- `ls -l` — format list detail
- `ls -h` — ukuran human-readable (K, M, G)
- `ls -lh` — keduanya sekaligus (paling sering dipakai)

### Baca Output ls -lh

- Kolom: permission | link | owner | group | size | tanggal | nama
- Contoh: `-rw-r--r-- 1 user group 0 Feb 19 file1`

### File Permission

- Format: `rwxrwxrwx` — 9 karakter, 3 kelompok
- Kelompok: 3 pertama=owner, 3 tengah=group, 3 terakhir=others
- `r`=baca, `w`=tulis, `x`=jalankan, `-`=tidak punya
- Nilai numerik: r=4, w=2, x=1 → jumlahkan per kelompok
- `rwx` = 4+2+1 = 7
- `rw-` = 4+2+0 = 6
- `r--` = 4+0+0 = 4
- `rwxr-xr-x` = 755
- `rw-r--r--` = 644
- `rwx------` = 700
- `chmod 750 file` — ubah permission

### Perintah Filesystem

- `touch namafile` — buat file kosong
- `mkdir namafolder` — buat folder
- `cp asal tujuan` — salin (asli tetap)
- `mv asal tujuan` — pindah/rename (asli hilang = Cut)
- `mv file folder/` — pindah file ke folder
- `rm namafile` — hapus file PERMANEN
- `rm -R namafolder` — hapus folder + isinya
- `file namafile` — cek tipe file

### Berpindah User (su)

- `su user2` — pindah user, direktori tetap di user lama
- `su -l user2` — pindah user + masuk home directory user baru
- `-l` / `--login` — inherit environment user tujuan

### Direktori Penting

- `/etc` — konfigurasi sistem OS (sudoers, passwd, shadow)
- `/etc/shadow` — password terenkripsi sha512
- `/var` — data dinamis (log, backup, database)
- `/var/log` — semua log file disimpan di sini
- `/root` — home directory root user (BUKAN /home/root)
- `/tmp` — sementara, hapus otomatis saat restart, semua user bisa tulis
