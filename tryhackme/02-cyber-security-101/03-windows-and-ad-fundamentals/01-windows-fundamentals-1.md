# Windows Fundamentals 1 — Resume Materi
**TryHackMe | 15 Mei 2026**

---

## 1. Windows Editions & Sejarah

Windows hadir sejak 1985 dan hingga kini menjadi OS paling dominan di lingkungan rumahan maupun korporat. Dominasi ini menjadikannya target utama hacker dan pembuat malware.

**Urutan versi penting:**

- XP — sangat populer, bertahan lama
- Vista — perombakan total, gagal diterima pasar, cepat ditinggalkan
- 7 — pengganti XP, stabil, langsung ditandai end-of-support setelah rilis
- 8.x — berumur pendek, bernasib mirip Vista
- 10 — support berakhir 14 Oktober 2025
- 11 — versi desktop terkini, dirilis 5 Oktober 2021, tersedia dalam varian **Home** dan **Pro**

**Versi server terkini:** Windows Server 2025

Pelajaran penting dari era XP: ketika Microsoft mengumumkan end-of-life, perusahaan dan rumah sakit panik karena harus memastikan semua vendor dan perangkat kompatibel dengan Windows 7 dalam waktu singkat. Microsoft mencatat ini sebagai pelajaran dan mulai menandai tanggal end-of-support lebih awal.

---

## 2. The Desktop (GUI)

**GUI** (Graphical User Interface) adalah tampilan yang muncul setelah login. Sebelum sampai ke desktop, pengguna harus melewati login screen dengan username dan password — bisa dari akun lokal atau **Active Directory** jika komputer tergabung dalam domain.

### 2.1 The Desktop

Area utama tempat shortcut program, folder, dan file diletakkan untuk akses cepat. Klik kanan di area kosong desktop memunculkan context menu dengan opsi:

- Mengubah ukuran dan susunan ikon
- Copy/paste item ke desktop
- Membuat item baru (folder, shortcut, dokumen teks)
- **Display Settings** — ubah resolusi, orientasi, konfigurasi multi-monitor
- **Personalize** — ubah wallpaper, font, tema, skema warna

Catatan: dalam sesi **Remote Desktop**, beberapa pengaturan display (Scale and Layout, Resolution) dinonaktifkan dan tidak bisa diubah dari sesi remote.

### 2.2 Start Menu

Sejak Windows 10, Start Menu ditandai dengan **logo Windows** (bukan tulisan "Start"). Fungsi utamanya tetap sama: akses ke semua app, file, dan utilitas.

Start Menu terbagi menjadi tiga bagian:

**Bagian kiri bawah (shortcuts akun):**
Berisi shortcut ke Documents, Pictures, Settings, dan Power. Ikon hamburger (≡) di bagian atas untuk expand/collapse panel. Power icon memberi opsi disconnect RDP, shutdown, atau restart.

**Bagian tengah (daftar aplikasi):**
Menampilkan Recently Added di bagian atas, diikuti semua app terinstal urut alfabetis. Klik heading huruf untuk membuka alphabet grid — berguna jika daftar app sangat panjang.

**Bagian kanan (Tiles):**
Ikon-ikon yang disebut **tiles**. Klik kanan tile untuk opsi: resize, unpin from Start, view Properties. App bisa ditambahkan ke sini dengan klik kanan app → **Pin to Start**.

### 2.3 Search Box (Cortana)

Kotak pencarian di taskbar untuk mencari app, file, dan pengaturan. Bisa disembunyikan sepenuhnya via klik kanan taskbar → Search → **Hidden**.

### 2.4 Task View

Tombol di taskbar untuk melihat semua window yang terbuka dan mengelola virtual desktop. Bisa dinonaktifkan via klik kanan taskbar → uncheck **Show Task View button**.

### 2.5 Taskbar

Semua app/file yang sedang terbuka muncul di sini. Hover pada ikon menampilkan preview thumbnail dan tooltip — berguna untuk membedakan beberapa instance app yang sama. Menutup app menghilangkannya dari taskbar, kecuali jika di-pin secara eksplisit.

Klik kanan taskbar memunculkan context menu dengan opsi: Toolbars, Search, Show Task View button, Show Windows Ink Workspace button, Cascade windows, Show the desktop, Task Manager, Lock the taskbar, Taskbar settings.

### 2.6 Toolbars

Komponen opsional yang bisa ditampilkan/disembunyikan di taskbar. Diatur via klik kanan taskbar → Toolbars.

### 2.7 Notification Area

Terletak di pojok kanan bawah. Menampilkan jam, tanggal, dan ikon sistem seperti volume, network, dan Action Center. Ikon bisa ditambah/dihapus via: klik kanan taskbar → **Taskbar settings** → scroll ke bagian **Notification Area** → pilih salah satu dari dua opsi:

- **Select which icons appear on the taskbar**
- **Turn system icons on or off**

---

## 3. File System — NTFS

### 3.1 Evolusi File System

| File System | Keterangan |
|---|---|
| FAT16/FAT32 | File Allocation Table — file system lama, masih dipakai di USB dan MicroSD |
| HPFS | High Performance File System — pendahulu NTFS |
| NTFS | New Technology File System — standar Windows modern |

FAT masih umum ditemukan di perangkat portabel, tetapi secara tradisional tidak digunakan di PC, laptop, atau server Windows.

### 3.2 Keunggulan NTFS

NTFS adalah **journaling file system**: setiap perubahan dicatat di log file sebelum benar-benar dieksekusi. Jika terjadi kegagalan (misalnya mati listrik mendadak), sistem bisa otomatis memperbaiki folder/file menggunakan log tersebut. FAT tidak memiliki fitur ini.

Keunggulan NTFS dibanding file system lama:

- Support file berukuran lebih dari 4GB
- Set permission spesifik pada folder dan file
- Kompresi folder dan file
- Enkripsi melalui **EFS** (Encryption File System)

Cara cek file system yang digunakan: klik kanan drive C → Properties.

### 3.3 NTFS Permissions

Pada volume NTFS, permission bisa diatur untuk memberikan atau menolak akses ke file dan folder.

| Permission | Untuk Folder | Untuk File |
|---|---|---|
| Read | Lihat dan list isi folder | Lihat/akses isi file |
| Write | Tambah file dan subfolder | Tulis ke file |
| Read & Execute | Lihat+list+eksekusi; diwariskan file & folder | Lihat+akses+eksekusi file |
| List Folder Contents | Lihat+list+eksekusi; hanya diwariskan folder | N/A |
| Modify | Baca+tulis+hapus folder | Baca+tulis+hapus file |
| Full Control | Baca+tulis+ubah+hapus semua | Baca+tulis+ubah+hapus file |

**Cara melihat permission file/folder:**

1. Klik kanan file atau folder
2. Pilih Properties
3. Klik tab Security
4. Pilih user/group di daftar "Group or user names"

### 3.4 Alternate Data Streams (ADS)

**ADS** adalah atribut file yang spesifik untuk NTFS. Setiap file memiliki minimal satu data stream bernama **$DATA**. ADS memungkinkan satu file memiliki lebih dari satu stream data.

Windows Explorer secara default tidak menampilkan ADS. Untuk melihatnya dibutuhkan tool pihak ketiga atau PowerShell (dibahas lebih lanjut di room Windows PowerShell).

Penggunaan ADS:

- **Berbahaya:** dimanfaatkan malware untuk menyembunyikan data
- **Normal:** Windows menulis identifikasi ke ADS saat file diunduh dari internet, menandai asal file tersebut

---

## 4. Windows\System32 Folders

### 4.1 Folder Windows

Secara default berada di **C:\Windows**, tetapi bisa berada di drive atau folder lain. Karena lokasi bisa berubah, Windows menggunakan **environment variable** sebagai referensi konsisten.

**Environment variable** menyimpan informasi tentang lingkungan OS: path sistem, jumlah prosesor, lokasi folder sementara, dan sebagainya.

Variable untuk direktori Windows: `%windir%`

### 4.2 Folder System32

Berlokasi di **C:\Windows\System32**, berisi lebih dari 3.975 item. Folder ini menyimpan file-file yang **kritis** bagi operasi sistem.

Menghapus file atau folder di dalam System32 secara tidak sengaja bisa membuat Windows tidak bisa berjalan sama sekali. Harus ditangani dengan sangat hati-hati.

Banyak tools yang dibahas dalam seri Windows Fundamentals berada di dalam folder ini.

---

## 5. User Accounts, Profiles, dan Permissions

### 5.1 Dua Tipe Akun

**Administrator**
Bisa membuat perubahan di tingkat sistem: tambah/hapus user, modifikasi grup, modifikasi settings sistem, install program.

**Standard User**
Hanya bisa mengubah folder/file yang dimilikinya sendiri. Tidak bisa melakukan perubahan system-level seperti install program.

### 5.2 Cara Melihat Akun yang Ada

**Via Settings:**
Start Menu → ketik "Other User" → klik shortcut System Settings > Other users.

Karena login sebagai Administrator, tersedia opsi "Add someone else to this PC". Standard User tidak melihat opsi ini.

Klik akun yang ada → muncul tombol **Change account type** dan **Remove**. Change account type menampilkan dropdown untuk memilih antara Standard User atau Administrator.

**Via lusrmgr.msc:**
Klik kanan Start Menu → Run → ketik `lusrmgr.msc`

Menampilkan dua folder:

- **Users** — daftar semua akun lokal beserta deskripsi
- **Groups** — daftar semua grup lokal beserta deskripsi singkat

Setiap grup memiliki permission yang ditetapkan. User yang ditambahkan ke grup akan **mewarisi** permission grup tersebut. Satu user bisa masuk ke beberapa grup sekaligus.

Catatan: mengklik "Add someone else to this PC" dari halaman Other Users akan membuka Local Users and Management.

### 5.3 User Profile

Setiap kali akun baru dibuat dan login pertama kali, Windows otomatis membuat **user profile** untuk akun tersebut.

Lokasi: `C:\Users\[nama_user]` — contoh: `C:\Users\Max`

Saat login pertama, tampil pesan "Please wait for the User Profile Service" di layar, diikuti dialog "Personalized Settings — Setting up personalized settings for: [app]".

Setiap profil secara default berisi folder: Desktop, Documents, Downloads, Music, Pictures.

---

## 6. User Account Control (UAC)

### 6.1 Masalah yang Dipecahkan UAC

Sebagian besar pengguna rumahan login sebagai administrator lokal. Ini berarti semua proses yang berjalan — termasuk malware — mendapat hak akses penuh ke sistem. Pengguna tidak perlu privilege tinggi untuk tugas sehari-hari seperti browsing atau mengedit dokumen.

### 6.2 Cara Kerja UAC

**UAC** diperkenalkan pada Windows Vista dan terus ada di versi berikutnya. Ketika administrator login, sesinya tidak langsung berjalan dengan elevated permissions. Jika ada operasi yang membutuhkan privilege lebih tinggi, sistem menampilkan **prompt konfirmasi**.

Catatan: UAC secara default **tidak berlaku** untuk built-in local administrator account.

### 6.3 Indikator Visual UAC

Ketika login sebagai Standard User dan mencoba menjalankan program yang butuh privilege lebih tinggi, ikon program akan menampilkan **shield icon** (perisai kuning-biru) di atasnya. Ini tanda bahwa UAC akan meminta konfirmasi sebelum program bisa dijalankan.

### 6.4 Alur UAC saat Install Program

1. Double-click program → muncul prompt UAC
2. Prompt menampilkan nama program, publisher, dan asal file
3. Sistem meminta username dan password Administrator
4. Jika password tidak dimasukkan dalam waktu tertentu → prompt menghilang dan program tidak terinstall

Fitur ini secara signifikan mengurangi kemungkinan malware berhasil mengkompromikan sistem.

---

## 7. Settings dan Control Panel

### 7.1 Dua Lokasi Pengaturan Utama

**Control Panel**
Sudah ada sejak lama. Tempat utama untuk perubahan sistem kompleks: tambah printer, uninstall program, konfigurasi jaringan tingkat lanjut, dll.

**Settings Menu**
Diperkenalkan pada Windows 8 (dirancang untuk touchscreen tablet). Di Windows 10, Settings sudah menjadi lokasi utama yang dituju pengguna. Kategori yang tersedia: System, Devices, Network & Internet, Personalization, Apps, Accounts, Time & Language, Ease of Access, Privacy, Update & Security, Search.

Keduanya bisa diakses dari Start Menu. Catatan: ikon Settings bisa berbeda tergantung versi Windows.

### 7.2 Hubungan Keduanya

Dalam beberapa kasus, alur pengaturan dimulai dari Settings dan berakhir di Control Panel. Contoh: Settings → Network & Internet → Change adapter options → jendela yang terbuka berasal dari Control Panel (Network Connections).

Control Panel adalah tempat untuk pengaturan yang lebih kompleks dan tidak tersedia di Settings.

### 7.3 Melihat Aplikasi Terinstall

Control Panel → **Programs** → **Programs and Features**

Menampilkan daftar semua aplikasi terinstall beserta nama, publisher, dan tanggal instalasi. Berguna untuk memverifikasi software apa saja yang ada di sistem.

### 7.4 Tips Navigasi

Jika tidak yakin harus membuka Settings atau Control Panel, gunakan **Start Menu search**. Ketik nama pengaturan yang dicari — sistem akan menampilkan hasil yang paling relevan dari kedua lokasi tersebut.

---

## 8. Task Manager

### 8.1 Fungsi

Task Manager memberikan informasi tentang aplikasi dan proses yang sedang berjalan, termasuk penggunaan CPU dan RAM secara real-time.

### 8.2 Cara Membuka

- Klik kanan taskbar → **Task Manager**
- Keyboard shortcut: `Ctrl + Shift + Esc`

### 8.3 Dua Mode Tampilan

**Simple View**
Tampilan minimal — hanya menampilkan app yang sedang terbuka. Terdapat tombol "More details" di bagian bawah.

**More Details View**
Klik "More details" untuk tampilan lengkap. Terdiri dari tab-tab berikut:

| Tab | Fungsi |
|---|---|
| Processes | Daftar Apps dan Background Processes beserta penggunaan CPU dan Memory |
| Performance | Grafik real-time penggunaan CPU, RAM, Disk, dan Network |
| Users | Siapa saja yang login dan berapa resource yang mereka gunakan |
| Details | Informasi teknis tiap proses: PID, status, username |
| Services | Daftar Windows services yang sedang berjalan |

---

## 9. Remote Desktop (RDP)

**Remote Desktop Protocol (RDP)** memungkinkan pengguna mengakses dan mengontrol komputer lain dari jarak jauh. Di lingkungan TryHackMe, RDP digunakan untuk mengakses VM yang di-deploy.

**Cara menggunakan Remote Desktop Connection:**

1. Buka Remote Desktop Connection
2. Isi kolom Computer dengan IP target
3. Klik Show Options → isi User name
4. Klik Connect → masukkan password saat diminta
5. Accept certificate jika diminta → masuk ke sistem remote

Untuk berpindah akun di environment VM/TryHackMe: buka sesi RDP baru dengan kredensial akun yang berbeda (bukan sign out dari dalam VM).

---

## 10. Glossary

**Active Directory** — sistem manajemen akun terpusat untuk jaringan berbasis domain

**ADS (Alternate Data Streams)** — fitur NTFS yang memungkinkan satu file menyimpan lebih dari satu stream data; tidak terlihat di Windows Explorer

**$DATA** — nama stream data default yang dimiliki setiap file di NTFS

**Domain-joined** — kondisi komputer yang tergabung dalam jaringan domain dan menggunakan Active Directory untuk autentikasi

**EFS (Encryption File System)** — fitur enkripsi bawaan NTFS

**Elevated Permissions** — hak akses tingkat administrator yang dibutuhkan untuk operasi sistem tertentu

**End-of-life / End-of-support** — tanggal berakhirnya dukungan resmi (update keamanan) untuk suatu produk

**Environment Variable** — variabel yang menyimpan informasi tentang lingkungan OS seperti path, jumlah prosesor, dan lokasi folder sementara

**FAT (File Allocation Table)** — file system lama; masih digunakan di USB dan MicroSD

**GUI (Graphical User Interface)** — antarmuka visual sistem operasi

**Journaling File System** — file system yang mencatat perubahan di log sebelum dieksekusi, memungkinkan auto-repair jika terjadi kegagalan

**lusrmgr.msc** — tool Local User and Group Management untuk melihat dan mengelola akun dan grup lokal

**NTFS (New Technology File System)** — file system standar untuk Windows modern

**RDP (Remote Desktop Protocol)** — protokol untuk mengakses dan mengontrol komputer lain dari jarak jauh

**Shield Icon** — ikon perisai kuning-biru yang menandakan program membutuhkan konfirmasi UAC untuk dijalankan

**Tiles** — ikon-ikon di sisi kanan Start Menu yang bisa di-pin, di-resize, atau di-unpin

**UAC (User Account Control)** — fitur keamanan Windows yang meminta konfirmasi sebelum operasi yang membutuhkan elevated permissions dijalankan

**%windir%** — environment variable yang selalu menunjuk ke lokasi folder Windows, terlepas dari drive atau path aktualnya

---

## 11. Tools & Platform Rujukan

**Microsoft Documentation (FAT, HPFS, NTFS)**
Dokumentasi resmi Microsoft tentang perbandingan file system Windows.
URL tidak dicantumkan secara eksplisit di materi, dapat ditemukan di docs.microsoft.com.

**Microsoft Documentation (UAC)**
Penjelasan resmi tentang cara kerja User Account Control.
URL tidak dicantumkan secara eksplisit di materi, dapat ditemukan di docs.microsoft.com.

**Microsoft Documentation (NTFS Special Permissions)**
Referensi untuk memahami Special Permissions di luar enam permission standar NTFS.
URL tidak dicantumkan secara eksplisit di materi.

**Microsoft Documentation (Start Menu)**
Dokumentasi singkat tentang Start Menu Windows.
URL tidak dicantumkan secara eksplisit di materi (disebutkan sebagai hyperlink "here" di materi asli).

**Microsoft Documentation (Notification Area)**
Dokumentasi singkat tentang Notification Area.
URL tidak dicantumkan secara eksplisit di materi (disebutkan sebagai hyperlink "here" di materi asli).

**MalwareBytes — ADS Article**
Artikel tentang Alternate Data Streams dari perspektif keamanan.
URL tidak dicantumkan secara eksplisit di materi (disebutkan sebagai hyperlink "here" di materi asli).

**TryHackMe — Core Windows Processes Room**
Room lanjutan untuk mempelajari core Windows processes dan tanggung jawab masing-masing proses.
Platform: tryhackme.com

**TryHackMe — Advent of Cyber 2, Day 21**
Latihan hands-on untuk berinteraksi langsung dengan ADS.
Platform: tryhackme.com

**Task Manager Blog Post**
Blog post eksternal tentang Task Manager secara lebih mendalam (disebutkan sebagai hyperlink "blog post" di Task 9, URL tidak terlihat jelas di materi).

---

## 12. Catatan Ringkas untuk Ditulis Tangan

### Windows Editions

- Windows sejak 1985 — OS paling dominan → target utama malware
- Urutan: XP → Vista (gagal) → 7 → 8.x (gagal) → 10 → 11
- Win 10 end-of-support: 14 Okt 2025
- Win 11 rilis: 5 Okt 2021 — varian: Home & Pro
- Server terkini: Windows Server 2025

### Desktop GUI — 7 Komponen

- Desktop — shortcut, klik kanan → Display Settings / Personalize
- Start Menu — 3 bagian: shortcuts akun | daftar app | tiles
- Search Box — Hidden = sembunyikan sepenuhnya
- Task View — uncheck "Show Task View button" = hilang
- Taskbar — app terbuka muncul di sini, hover = preview thumbnail
- Toolbars — opsional, aktif/nonaktif via klik kanan taskbar
- Notification Area — pojok kanan bawah, atur via Taskbar settings

### File System

- FAT — lama, untuk USB/MicroSD, tidak support file >4GB, tidak ada journaling
- NTFS — modern, journaling, support >4GB, permission, kompresi, EFS
- Journaling — catat perubahan di log → auto-repair jika gagal
- Cek file system — klik kanan drive C → Properties
- NTFS permission: Read | Write | Read & Execute | List Folder | Modify | Full Control
- Lihat permission — klik kanan → Properties → Security → pilih user
- ADS — stream data tersembunyi di file NTFS, tidak terlihat di Explorer
- $DATA — stream default setiap file
- ADS dipakai malware untuk sembunyikan data

### System32

- C:\Windows — folder OS utama, bisa di drive lain
- %windir% — environment variable → selalu tunjuk ke folder Windows
- C:\Windows\System32 — 3.975+ item, file kritis OS, JANGAN hapus sembarangan

### User Accounts

- 2 tipe: Administrator (ubah sistem) | Standard User (folder sendiri saja)
- Lihat akun: Start Menu → ketik "Other User"
- Lihat & kelola: klik kanan Start Menu → Run → lusrmgr.msc
- lusrmgr: folder Users + Groups
- Group — punya permission sendiri, user yang masuk mewarisi permission group
- User profile — dibuat saat pertama login, lokasi: C:\Users\[nama]
- Folder default profil: Desktop, Documents, Downloads, Music, Pictures

### UAC

- UAC — minta konfirmasi sebelum operasi elevated dijalankan
- Sejak Windows Vista
- Default: TIDAK berlaku untuk built-in local administrator
- Shield icon — tanda program butuh UAC untuk jalan
- Jika password tidak diisi → prompt hilang sendiri → program tidak install

### Settings vs Control Panel

- Settings — diperkenalkan Win8, sekarang lokasi utama pengaturan
- Control Panel — lama, untuk pengaturan kompleks
- Keduanya bisa diakses dari Start Menu
- Settings bisa berujung ke Control Panel (contoh: Change adapter options)
- Tips: bingung buka yang mana? → search di Start Menu saja
- Lihat app terinstall: Control Panel → Programs → Programs and Features

### Task Manager

- Fungsi — monitor app/proses, CPU & RAM usage
- Buka: klik kanan taskbar → Task Manager
- Shortcut: Ctrl + Shift + Esc
- Simple View → More details untuk tampilan lengkap
- Tab: Processes | Performance | Users | Details | Services

### Remote Desktop

- RDP — akses & kontrol komputer lain dari jarak jauh
- Tool: Remote Desktop Connection
- Isi: IP target + username + password
- Accept certificate saat diminta → masuk ke sistem
- Ganti akun di VM TryHackMe → buka sesi RDP baru, bukan sign out
- Di sesi RDP: Scale/Resolution tidak bisa diubah
