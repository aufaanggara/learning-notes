# Resume Materi — Windows CLI Basics
**TryHackMe | 1 November 2026**

## 1. Konsep Dasar

### 1.1 Windows Command Prompt (CMD)

**Command Prompt** adalah antarmuka berbasis teks untuk berinteraksi langsung dengan sistem operasi Windows. Alih-alih mengklik folder dan menu grafis, pengguna mengetik perintah yang dieksekusi langsung oleh sistem.

Sering disebut **CMD** atau **terminal**.

### 1.2 Mengapa CMD Penting dalam Cybersecurity

- Lebih cepat dari antarmuka grafis untuk tugas berulang
- Memberikan kontrol lebih besar atas sistem
- Banyak security tools hanya bisa dijalankan melalui CLI
- Di dunia nyata, tidak semua hal terlihat di antarmuka grafis — file bisa tersembunyi atau terkubur jauh di dalam sistem
- Analis sering butuh jawaban cepat tanpa klik menu berlapis

### 1.3 Struktur Filesystem Windows

Windows menggunakan **drive letter** sebagai titik awal navigasi, berbeda dari Linux yang menggunakan `/`.

Format path di Windows: `C:\Users\NamaUser\Documents\file.txt`

- `C:` adalah drive letter (partisi utama)
- `\` adalah pemisah antar folder (bukan `/` seperti Linux)
- Path dimulai dari drive, bukan dari root `/`

## 2. Navigasi Filesystem

### 2.1 Cek Posisi Saat Ini

```
cd
```

Tanpa argumen, `cd` menampilkan **full path** direktori tempat kamu berada sekarang. Di Windows, default saat membuka CMD biasanya adalah **user folder** (`C:\Users\NamaUser`).

> Catatan: `cd` di Windows berbeda dari `pwd` di Linux — satu perintah, dua fungsi (cek posisi dan berpindah folder).

### 2.2 Berpindah Folder

```
cd nama_folder
```

Masuk ke folder bernama `nama_folder` yang ada di dalam direktori saat ini.

```
cd ..
```

Kembali satu level ke atas (ke folder induk). Dua titik (`..`) selalu berarti "satu level di atas".

```
cd C:\Users\Administrator\Documents
```

Navigasi langsung ke path absolut tanpa harus masuk satu per satu.

### 2.3 Melihat Isi Folder

```
dir
```

Menampilkan daftar file dan folder yang **terlihat secara default** di direktori saat ini. Output mencakup:

- Tanggal dan waktu modifikasi
- `<DIR>` untuk folder, ukuran bytes untuk file
- Ringkasan jumlah file, folder, dan ruang kosong di akhir

```
dir /a
```

Menampilkan **semua** isi direktori termasuk item yang ditandai sebagai hidden. Switch `/a` berarti *all attributes*. Penting karena Windows menyembunyikan banyak folder sistem secara default (AppData, folder `.logs`, dll).

## 3. Menemukan File

### 3.1 Pencarian Rekursif

```
dir /s nama_file.txt
```

Mencari file bernama `nama_file.txt` di **semua subfolder** mulai dari direktori saat ini ke bawah. Switch `/s` berarti *subdirectories* — perintah ini akan menelusuri seluruh struktur folder di bawah posisi sekarang dan menampilkan **full path** jika file ditemukan.

Contoh penggunaan:

```
dir /s task_brief.txt
```

Output akan menunjukkan direktori lengkap tempat file berada.

### 3.2 Membaca Isi File

```
type nama_file.txt
```

Mencetak isi file teks langsung di terminal. Ekuivalen dengan `cat` di Linux. Digunakan setelah berhasil menavigasi ke folder yang berisi file tersebut.

## 4. Alur Kerja: Menemukan File Tanpa Tahu Lokasinya

Ini adalah workflow standar yang dipakai analis saat harus menemukan file dengan nama yang diketahui tapi lokasi tidak diketahui:

1. `cd` — konfirmasi posisi saat ini (pastikan di user folder)
2. `dir` — lihat isi folder yang terlihat
3. `dir /a` — cek apakah ada file atau folder tersembunyi
4. `dir /s nama_file.txt` — biarkan Windows cari di semua subfolder
5. `cd <path_hasil_pencarian>` — navigasi ke folder tempat file ditemukan
6. `dir` — konfirmasi file memang ada di sini
7. `type nama_file.txt` — baca isi file

## 5. Mengumpulkan Informasi Sistem

Perintah-perintah berikut adalah yang pertama kali dijalankan analis dan staf IT saat pertama kali mengakses mesin Windows yang tidak dikenal.

### 5.1 Identitas User

```
whoami
```

Menampilkan **username akun yang sedang aktif**. Penting karena user yang berbeda memiliki permission yang berbeda — mengetahui kamu login sebagai siapa menentukan apa yang bisa dan tidak bisa kamu lakukan di sistem.

Output format: `domain\username` atau `hostname\username`

### 5.2 Nama Komputer

```
hostname
```

Menampilkan **nama mesin** ini. Di lingkungan jaringan (kantor, perusahaan), setiap komputer punya nama unik untuk memudahkan identifikasi dalam jaringan.

### 5.3 Versi dan Detail OS

```
systeminfo
```

Menampilkan informasi lengkap tentang sistem operasi dan hardware. Output-nya panjang — fokus pada tiga bagian utama:

| Field | Keterangan |
|---|---|
| OS Name | Nama lengkap Windows yang terinstall |
| OS Version | Nomor versi spesifik Windows |
| System Type | Apakah arsitektur 32-bit atau 64-bit |

Field lain yang berguna: `System Manufacturer`, `Total Physical Memory`, `Domain`, `Hotfix(s)`.

### 5.4 Informasi Jaringan

```
ipconfig
```

Menampilkan **konfigurasi jaringan** mesin. Dari output ini, cari dua hal utama:

- **IPv4 Address** — alamat IP mesin ini dalam jaringan
- **Default Gateway** — IP router/pintu keluar ke jaringan yang lebih luas
- **Subnet Mask** — menentukan batas jaringan lokal

Output dibagi per adapter jaringan (Ethernet, Wi-Fi, dll). Dalam investigasi, IPv4 Address adalah yang paling sering dibutuhkan pertama kali.

## 6. Tentang Hidden Files

**Hidden** di Windows tidak berarti terenkripsi atau dilindungi — hanya berarti Windows tidak menampilkannya secara default di antarmuka grafis maupun perintah `dir` biasa.

Banyak folder sistem penting yang hidden secara default, seperti `AppData`, folder konfigurasi tersembunyi, log sistem, dan junction point.

Untuk melihatnya, gunakan `dir /a`. Di room ini, `dir` biasa menampilkan 16 direktori, sedangkan `dir /a` menampilkan lebih dari 12 folder tersembunyi tambahan.

## 7. Tabel Ringkas Semua Perintah

| Perintah | Fungsi |
|---|---|
| `cd` | Tampilkan direktori saat ini |
| `cd nama_folder` | Masuk ke folder tertentu |
| `cd ..` | Naik satu level ke folder induk |
| `cd C:\path\lengkap` | Navigasi ke path absolut |
| `dir` | Tampilkan isi folder (visible only) |
| `dir /a` | Tampilkan semua isi termasuk hidden |
| `dir /s nama_file` | Cari file di semua subfolder |
| `type nama_file` | Baca dan cetak isi file di terminal |
| `whoami` | Tampilkan username yang sedang login |
| `hostname` | Tampilkan nama komputer |
| `systeminfo` | Tampilkan detail lengkap OS dan hardware |
| `ipconfig` | Tampilkan konfigurasi jaringan |

## 8. Glossary

**Directory** — istilah teknis untuk folder dalam struktur filesystem

**Path** — alamat lengkap lokasi file atau folder (contoh: `C:\Users\Admin\Documents`)

**Absolute path** — path yang dimulai dari drive letter, menunjuk lokasi pasti tanpa bergantung posisi saat ini

**Relative path** — path yang ditulis relatif terhadap posisi direktori saat ini

**Hidden file** — file atau folder yang ditandai untuk tidak ditampilkan secara default; bukan berarti terlindungi

**Drive letter** — penanda partisi di Windows (`C:`, `D:`, dll); titik awal navigasi filesystem

**Switch / Flag** — argumen tambahan setelah perintah untuk mengubah perilakunya (contoh: `/a`, `/s`)

**Subfolder** — folder yang berada di dalam folder lain (nested directory)

**Permission** — hak akses yang dimiliki akun tertentu di sistem; menentukan file/folder mana yang bisa dibuka, dimodifikasi, atau dieksekusi

**IPv4 Address** — alamat numerik unik mesin dalam jaringan (format: `10.x.x.x` atau `192.168.x.x`)

**Default Gateway** — IP router yang menjadi pintu keluar dari jaringan lokal ke jaringan yang lebih luas

**Subnet Mask** — nilai yang menentukan batas jaringan lokal dan membedakannya dari jaringan luar

**Junction point** — semacam shortcut/symlink di Windows yang mengarah ke folder lain; muncul di output `dir /a` dengan label `<JUNCTION>`

**CLI** — Command Line Interface; antarmuka berbasis teks untuk berinteraksi dengan sistem

**GUI** — Graphical User Interface; antarmuka grafis berbasis klik (kebalikan dari CLI)

## 9. Tools & Platform Rujukan

Tidak ada tools atau platform eksternal yang disebutkan secara eksplisit dalam room ini. Semua perintah yang digunakan adalah bawaan Windows (built-in commands) yang tidak memerlukan instalasi tambahan.

## 10. Catatan Ringkas untuk Ditulis Tangan

**NAVIGASI**

- `cd` — cek posisi saat ini
- `cd nama` — masuk ke folder
- `cd ..` — naik satu level
- `cd C:\path` — navigasi ke path absolut

**MELIHAT ISI FOLDER**

- `dir` — isi folder (visible)
- `dir /a` — semua isi + hidden (`/a` = all attributes)

**MENCARI & MEMBACA FILE**

- `dir /s nama_file` — cari di semua subfolder rekursif (`/s` = subdirectories)
- `type nama_file` — cetak isi file di terminal

**INFO SISTEM**

- `whoami` — username yang sedang login
- `hostname` — nama komputer ini
- `systeminfo` — detail OS + hardware (fokus: OS Name, OS Version, System Type)
- `ipconfig` — info jaringan (fokus: IPv4 Address, Default Gateway, Subnet Mask)

**ALUR MENEMUKAN FILE TANPA TAHU LOKASI**

1. `cd` — konfirmasi posisi
2. `dir` — lihat yang visible
3. `dir /a` — cek hidden
4. `dir /s nama_file` — cari rekursif
5. `cd <path hasil>` — navigasi ke lokasi file
6. `dir` — konfirmasi ada
7. `type nama_file` — baca isi

**KONSEP KUNCI**

- Hidden ≠ rahasia; hanya tidak ditampilkan secara default
- `dir` biasa: 16 direktori; `dir /a`: tambah 12+ folder hidden
- Windows path: `C:\folder\subfolder` (pakai backslash)
- Switch = tambahan argumen untuk ubah perilaku perintah
- Permission berbeda per user — itulah kenapa `whoami` penting
- IPv4 = alamat mesin; Default Gateway = pintu keluar jaringan

**ISTILAH SINGKAT**

- Directory — folder
- Path — alamat lokasi file/folder
- Absolute path — path dari drive letter (`C:\...`)
- Relative path — path dari posisi saat ini
- Switch/flag — argumen tambahan perintah (`/a`, `/s`)
- Junction point — semacam symlink/shortcut Windows
- CLI — antarmuka teks; GUI — antarmuka grafis klik
