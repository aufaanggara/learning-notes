# Resume Materi — Linux CLI Basics
**TryHackMe | Linux Command Line Introduction**
**Tanggal:** 23 Oktober 2026

---

## 1. Konsep Dasar

### 1.1 Linux dan Perannya di Dunia Siber

**Linux** adalah sistem operasi open source yang mendominasi dunia keamanan siber karena sifatnya yang ringan, fleksibel, dan dapat dikustomisasi. Linux digunakan di server, perangkat jaringan, tools keamanan, hingga lingkungan hacking profesional.

Sebelum bisa mempertahankan sistem atau menyelidiki insiden, seorang profesional siber harus nyaman menavigasi Linux menggunakan **CLI (Command-Line Interface)**.

### 1.2 CLI dan Terminal

**CLI (Command-Line Interface)** adalah cara berinteraksi dengan sistem operasi sepenuhnya menggunakan teks — tidak ada antarmuka grafis, tidak ada tombol untuk diklik. Pengguna mengetik perintah, sistem merespons dengan teks.

**Terminal** adalah jendela atau aplikasi yang menjadi tempat pengguna mengetik perintah CLI. Di lingkungan Linux desktop, terminal bisa dibuka melalui shortcut atau diklik dari Desktop.

Alasan profesional keamanan siber menggunakan terminal:

- Lebih cepat dibandingkan navigasi grafis
- Memberikan kontrol lebih langsung dan granular terhadap sistem
- Banyak tools keamanan hanya tersedia atau optimal di lingkungan CLI

---

## 2. Navigasi Filesystem

### 2.1 Memahami Struktur Filesystem Linux

Filesystem Linux berbentuk hierarki seperti pohon terbalik, dimulai dari **root (`/`)** sebagai titik paling atas. Semua file dan direktori berada di bawah root.

Beberapa lokasi penting:

- `/` — Root, puncak dari seluruh hierarki filesystem
- `/home` — Berisi direktori pribadi setiap user
- `/home/ubuntu` — Direktori home milik user "ubuntu" (disingkat dengan simbol `~`)
- `/etc` — Menyimpan file konfigurasi dan informasi sistem

### 2.2 Command Navigasi

**Mengetahui posisi saat ini:**

```bash
pwd
```

Singkatan dari **Print Working Directory**. Menampilkan path lengkap dari direktori yang sedang aktif.

Contoh output: `/home/ubuntu`

---

**Melihat isi direktori:**

```bash
ls
```

Menampilkan daftar file dan folder di direktori saat ini.

```bash
ls -l
```

Menampilkan daftar dengan detail lengkap: izin akses, jumlah link, pemilik, ukuran, dan tanggal modifikasi.

```bash
ls -al
```

Menampilkan semua isi direktori termasuk **file tersembunyi**, beserta detailnya.

| Switch | Fungsi |
|--------|--------|
| `-l` | Format panjang dengan detail (long listing) |
| `-a` | Tampilkan semua file termasuk yang tersembunyi |
| `-al` | Kombinasi keduanya |

---

**Berpindah direktori:**

```bash
cd <nama_direktori>
```

Masuk ke direktori yang dituju. Contoh: `cd Documents` akan memindahkan posisi ke `/home/ubuntu/Documents`.

```bash
cd ..
```

Naik satu level ke direktori induk. Contoh: dari `/home/ubuntu/Documents` kembali ke `/home/ubuntu`.

---

### 2.3 File Tersembunyi

File tersembunyi di Linux bukan berarti dienkripsi atau dikunci — cukup diawali dengan karakter titik (`.`) dan Linux tidak menampilkannya secara default.

Contoh file tersembunyi: `.Xauthority`, `.Xresources`, `.apport-ignore.xml`

Untuk menampilkannya, gunakan `ls -al`. Tanda `.` (direktori saat ini) dan `..` (direktori induk) yang muncul di output juga termasuk kategori ini.

---

## 3. Mencari dan Membaca File

### 3.1 Mencari File dengan `find`

```bash
find <titik_awal> -name <nama_file>
```

Mencari file berdasarkan nama, dimulai dari direktori yang ditentukan dan menelusuri seluruh subdirektori di dalamnya.

| Bagian | Keterangan |
|--------|-----------|
| `<titik_awal>` | Direktori awal pencarian, gunakan `~` untuk mulai dari home |
| `-name` | Switch untuk filter berdasarkan nama file |
| `<nama_file>` | Nama file yang dicari, bisa pakai wildcard `*` |

Contoh:

```bash
find ~ -name mission_brief.txt
```

Output berupa **full path** ke file jika ditemukan. Proses bisa memakan waktu karena Linux memeriksa setiap folder dalam direktori awal yang ditentukan.

---

### 3.2 Membaca File dengan `cat`

```bash
cat <nama_file>
```

Menampilkan seluruh isi file langsung ke layar terminal dari baris pertama hingga terakhir.

Contoh:

```bash
cat mission_brief.txt
```

Perintah ini berguna untuk membaca file teks, file konfigurasi, atau dokumen sistem.

---

## 4. Informasi Sistem

### 4.1 Identitas User

```bash
whoami
```

Menampilkan **username** dari user yang sedang aktif/login di sesi terminal saat ini. Berguna saat bekerja di banyak sistem untuk memastikan identitas aktif.

---

### 4.2 Informasi Sistem dan Kernel

```bash
uname -a
```

Menampilkan satu baris lengkap berisi semua informasi sistem.

| Switch | Fungsi |
|--------|--------|
| (tanpa switch) | Hanya nama OS, contoh output: `Linux` |
| `-a` | Semua informasi sistem secara lengkap |

Contoh output `uname -a`:

```
Linux tryhackme <versi_kernel>-aws #17-Ubuntu SMP Mon Sep 2 13:48:07 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
```

Breakdown output:

- **Linux** — Nama kernel yang berjalan
- **tryhackme** — Hostname (nama komputer dalam jaringan)
- **\<versi\>-aws** — Versi kernel yang terinstal
- **x86\_64** — Arsitektur hardware (64-bit)
- **GNU/Linux** — Tipe OS: kernel Linux ditambah tools GNU

---

### 4.3 Informasi Disk dan Storage

```bash
df -h
```

Menampilkan informasi penggunaan dan kapasitas disk seluruh filesystem yang ter-mount.

| Switch | Fungsi |
|--------|--------|
| `-h` | Human-readable: tampilkan ukuran dalam format mudah dibaca (2G, 500M) bukan bytes mentah |

Breakdown output `df -h`:

- **Filesystem** — Nama partisi/filesystem
- **Size** — Total kapasitas
- **Used** — Kapasitas yang sudah terpakai
- **Avail** — Kapasitas yang masih tersedia
- **Use%** — Persentase pemakaian
- **Mounted on** — Titik mount di filesystem Linux

Komponen penting dalam output:

- `/dev/root` — Disk utama fisik sistem (partisi root)
- `tmpfs` — Filesystem sementara yang disimpan di RAM, bukan disk fisik
- `/dev/shm` — Area shared memory

---

### 4.4 Membaca File Informasi Distribusi

```bash
cat /etc/os-release
```

Membaca file sistem yang berisi detail lengkap tentang distribusi Linux yang digunakan. Memberikan informasi yang lebih jelas dan spesifik dibanding `uname`.

Informasi yang tersedia di file ini:

- `PRETTY_NAME` — Nama lengkap distribusi beserta versinya
- `NAME` — Nama distribusi singkat
- `VERSION_ID` — Nomor versi
- `VERSION_CODENAME` — Nama kode rilis
- `ID` — Identifier singkat distro
- `ID_LIKE` — Distro yang menjadi basis (misal: debian)

---

## 5. Direktori Konfigurasi Sistem (`/etc`)

Direktori `/etc` adalah lokasi standar Linux untuk menyimpan **file konfigurasi dan informasi sistem**. Hampir semua service dan komponen sistem menyimpan konfigurasinya di sini.

Cara mengaksesnya:

```bash
cd /etc
ls
```

Beberapa file penting yang ada di `/etc`:

- `os-release` — Informasi identitas distribusi Linux
- `hosts` — Pemetaan hostname ke IP secara lokal
- `fstab` — Konfigurasi filesystem yang di-mount otomatis saat booting
- `resolv.conf` — Konfigurasi DNS resolver

---

## 6. Alur Kerja Standar di Terminal

Urutan logis yang umum dipakai saat pertama kali masuk ke terminal:

1. `pwd` — Cek posisi awal
2. `ls -al` — Lihat semua isi direktori termasuk file tersembunyi
3. `find ~ -name <file>` — Cari file yang tidak diketahui lokasinya
4. `cd <path>` — Navigasi ke lokasi file yang ditemukan
5. `cat <file>` — Baca isi file
6. `whoami` — Konfirmasi identitas user aktif
7. `uname -a` — Cek info sistem dan kernel
8. `df -h` — Cek status dan kapasitas disk
9. `cat /etc/os-release` — Cek nama dan versi distribusi Linux

---

## 7. Glossary — Istilah Wajib Hapal

**CLI (Command-Line Interface)** — Antarmuka berbasis teks untuk mengontrol sistem operasi tanpa GUI.

**Terminal** — Aplikasi/jendela tempat pengguna mengetik dan menjalankan perintah CLI.

**Directory** — Istilah Linux untuk folder/wadah yang berisi file atau direktori lain.

**Path** — Alamat lengkap lokasi file atau direktori dalam hierarki filesystem. Contoh: `/home/ubuntu/Documents/file.txt`.

**Root (`/`)** — Titik puncak dari seluruh hierarki filesystem Linux; semua file dan direktori berada di bawahnya.

**Home Directory** — Direktori pribadi setiap user, biasanya di `/home/<username>`, disingkat dengan simbol `~`.

**Kernel** — Inti dari sistem operasi; bertindak sebagai jembatan antara hardware dan software.

**Hostname** — Nama unik yang mengidentifikasi komputer dalam sebuah jaringan.

**Distro (Distribution)** — Varian sistem operasi berbasis Linux. Contoh: Ubuntu, Debian, Kali Linux.

**Switch / Flag** — Argumen tambahan yang ditulis setelah command menggunakan tanda `-` untuk mengubah atau memperluas perilaku command tersebut. Contoh: `-h`, `-a`, `-l`.

**Filesystem** — Cara sistem operasi mengorganisir, menyimpan, dan mengakses data di storage.

**tmpfs** — Temporary filesystem yang disimpan di RAM, bukan pada disk fisik; data hilang saat sistem dimatikan.

**Human-readable** — Format output yang menampilkan ukuran data dalam satuan mudah dibaca manusia (KB, MB, GB) dibanding angka bytes mentah.

**Hidden File** — File yang namanya diawali titik (`.`) sehingga tidak tampil dalam listing default; bukan berarti dienkripsi atau dilindungi.

**Mount** — Proses menghubungkan sebuah storage device atau filesystem ke titik tertentu dalam hierarki direktori Linux agar bisa diakses.

---

## 8. Tools & Platform Rujukan

Tidak ada tools atau platform eksternal yang secara eksplisit dirujuk dengan URL dalam room ini. Seluruh materi menggunakan command bawaan Linux tanpa dependensi tools pihak ketiga.

---

## 9. Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar

- CLI — kontrol komputer pakai teks, tanpa GUI
- Terminal — jendela tempat ketik perintah CLI
- Linux — OS open source, dominan di dunia siber
- / (root) — puncak hierarki filesystem Linux
- ~ — simbol shortcut untuk home directory user

### Navigasi

- pwd — tampilkan posisi direktori saat ini
- ls — list isi direktori
- ls -l — list dengan detail lengkap
- ls -al — list semua termasuk file tersembunyi
- cd \<dir\> — masuk ke direktori
- cd .. — naik satu level ke direktori induk

### File Tersembunyi

- Diawali titik: .namafile
- Tidak muncul di ls biasa, muncul di ls -al
- Bukan dikunci, cuma disembunyikan Linux secara default

### Cari & Baca File

- find ~ -name \<file\> — cari file mulai dari home directory
- cat \<file\> — tampilkan isi file ke layar

### Informasi Sistem

- whoami — tampilkan username aktif saat ini
- uname — nama OS saja
- uname -a — info sistem lengkap (kernel, hostname, arsitektur)
- df -h — info disk semua filesystem, format mudah dibaca
- -h — human readable (GB/MB bukan bytes)
- cat /etc/os-release — detail distro Linux yang dipakai

### Breakdown uname -a

- Linux — kernel aktif
- hostname — nama komputer di jaringan
- versi-aws — versi kernel terinstal
- x86\_64 — arsitektur 64-bit
- GNU/Linux — tipe OS

### Breakdown df -h

- /dev/root — disk fisik utama sistem
- tmpfs — filesystem sementara di RAM
- /dev/shm — shared memory area

### Direktori Penting

- /etc — folder konfigurasi sistem
- /home — folder home semua user
- ~ — home directory user aktif

### Istilah Kunci

- Path — alamat lengkap lokasi file
- Kernel — inti OS, jembatan hardware & software
- Hostname — nama komputer dalam jaringan
- Distro — varian Linux (Ubuntu, Kali, dll)
- Switch/Flag — argumen tambahan pakai -, ubah perilaku command
- tmpfs — filesystem di RAM, hilang saat reboot
- Mount — hubungkan storage ke titik direktori Linux
- Hidden file — diawali titik, tersembunyi secara default
