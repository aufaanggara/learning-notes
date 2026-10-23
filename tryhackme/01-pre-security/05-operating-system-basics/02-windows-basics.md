# Resume Materi — Windows Basics
**TryHackMe | Windows Fundamentals Module**
Tanggal: 11 Mei 2026

---

## 1. Sejarah & Evolusi Windows

Windows bermula dari **MS-DOS** — sistem berbasis teks di mana semua interaksi dilakukan lewat perintah yang diketik di layar hitam, tanpa ikon atau mouse.

Pada 1985, Microsoft merilis **Windows 1.0**, sebuah **GUI (Graphical User Interface)** pertama yang dibangun di atas DOS. Ini memperkenalkan jendela, menu, dan kontrol mouse ke komputer pribadi. Sejak saat itu Windows terus berkembang hingga menjadi OS desktop paling banyak digunakan di dunia saat ini.

| Versi | Tahun | Catatan |
|---|---|---|
| MS-DOS | Sebelum 1985 | Berbasis teks, tidak ada GUI |
| Windows 1.0 | 1985 | GUI pertama, dibangun di atas DOS |
| Windows ME | 2000 | Era millennium, tampilan lebih modern |
| Windows 11 | 2021 | Tampilan terkini, desain bersih |

---

## 2. Autentikasi & Tipe Akun

Sebelum bisa menggunakan Windows Desktop, pengguna harus **mengautentikasi diri** — membuktikan identitas lewat password, PIN, atau metode verifikasi lain. Proses ini menentukan apa saja yang boleh dilakukan pengguna setelah masuk.

### 2.1 Tiga Tipe Akun Windows

**Guest**
Akun akses sementara dengan izin minimal. Tidak bisa mengubah pengaturan sistem sama sekali. Cocok untuk pengguna yang butuh akses singkat tanpa hak konfigurasi apapun.

**Standard**
Akun untuk penggunaan sehari-hari — menjalankan aplikasi, mengubah preferensi pribadi. Tidak punya akses ke perubahan yang mempengaruhi seluruh sistem.

**Administrator**
Akun dengan kendali penuh: instalasi software, perubahan konfigurasi sistem, manajemen akun pengguna lain. Di lab TryHatMe, login otomatis dilakukan sebagai Administrator agar eksplorasi tidak dibatasi izin.

---

## 3. Windows Desktop & Komponen Utama

Setelah login, pengguna langsung masuk ke **Desktop** — titik pertama yang bisa diakses, dari mana semua area lain bercabang.

Desktop terdiri dari dua area utama:
- **Desktop** — ruang kerja utama tempat file, folder, dan shortcut berada
- **Taskbar** — strip kontrol di bagian bawah layar

### 3.1 Delapan Elemen Taskbar

| No. | Elemen | Fungsi |
|---|---|---|
| 1 | Desktop Icons | Shortcut ke Recycle Bin, folder, app; fully customizable |
| 2 | Start Menu | Akses utama ke app, setting, power option (log out/restart/shutdown) |
| 3 | Search | Cari app, file, folder, setting secara cepat lewat kata kunci |
| 4 | Task View | Lihat semua window yang terbuka, switch antar window dengan cepat |
| 5 | Pinned Apps/Folders | App dan folder yang paling sering dipakai, disematkan untuk akses cepat |
| 6 | Network & Audio | Kustomisasi pengaturan jaringan dan suara |
| 7 | Date and Time | Buka kalender lengkap, akses pengaturan tanggal dan waktu |
| 8 | Notifications | Tampilkan notifikasi app dan sistem, akses pengaturan jaringan |

### 3.2 Start Menu

Diakses lewat ikon Windows di pojok kiri bawah taskbar. Start Menu adalah "papan informasi" — tempat melihat semua yang tersedia di sistem.

Tiga area di dalam Start Menu:
- **Kiri** — daftar semua aplikasi terinstal, diurutkan secara alfabetis
- **Kanan** — tile aplikasi yang sering dipakai untuk akses cepat
- **Bawah kiri** — user options, akses ke Documents, Pictures, Settings, dan Power

---

## 4. File Explorer & Struktur Folder

### 4.1 Konsep Struktur Folder

Windows menggunakan **struktur folder hierarkis** — folder bisa berisi folder lain dan file di dalamnya. Ini membuat data tetap terorganisir dan mudah ditemukan seiring sistem berkembang.

Lokasi umum yang berfungsi sebagai direktori utama:
- Desktop
- Documents
- Downloads

Di dalam lokasi-lokasi ini, subfolder digunakan untuk mengelompokkan file-file yang berkaitan.

### 4.2 File Path

Lokasi setiap file di Windows dinyatakan dalam bentuk **path** — alamat lengkap dari root drive hingga file tersebut.

Contoh: `C:\Users\Administrator\Desktop\TryHatMe Onboarding`

Cara melihat path: klik nama folder di address bar File Explorer — path lengkap akan langsung tampil dan bisa disalin.

### 4.3 Elemen File Explorer

| No. | Elemen | Keterangan |
|---|---|---|
| 1 | Folder di Desktop | Titik awal, double-click untuk membuka |
| 2 | File Explorer terbuka | Folder tampil dalam antarmuka File Explorer |
| 3 | Ribbon atas | Opsi untuk view, share, dan edit folder/file |
| 4 | Address bar | Menampilkan path lengkap folder aktif |
| 5 | Panel isi folder | Menampilkan semua file dan subfolder di dalam |
| 6 | Search bar | Fungsi pencarian bawaan untuk cari file di dalam folder |

---

## 5. Informasi Sistem (About Your PC)

Windows menyediakan halaman **About your PC** di dalam aplikasi Settings — satu halaman yang memuat semua informasi penting tentang sistem.

Cara akses:
- Shortcut di Desktop, atau
- Settings → About your PC, atau
- Search bar → ketik "About your PC"

Halaman ini terbagi tiga kategori:

**Security Details** — status perlindungan aktif (Firewall & Network Protection, App & browser control, Device security)

**Device Details** — nama perangkat, processor, installed RAM, Device ID, Product ID, system type, pen and touch

**OS Details** — edisi Windows, versi, tanggal instalasi, OS build

---

## 6. Manajemen Aplikasi

### 6.1 Memperbarui Sistem (Windows Update)

**Windows Update** adalah tool bawaan yang menjaga OS dan beberapa aplikasi native tetap up to date. Diakses lewat Settings → Update & Security → Windows Update.

Pembaruan umumnya berisi:
- Security patches (tambal celah keamanan)
- Performance updates (peningkatan performa)
- Bug fixes (perbaikan error)

Windows Update bisa dikonfigurasi agar berjalan otomatis atau harus dijalankan manual, tergantung kebijakan organisasi.

### 6.2 Memperbarui Aplikasi

Cara pembaruan aplikasi berbeda-beda tergantung bagaimana software diinstal:

- Aplikasi bawaan Windows — bisa update otomatis di latar belakang
- Aplikasi pihak ketiga — biasanya punya mekanisme update sendiri
- Beberapa aplikasi — meminta update saat pertama kali dibuka
- Beberapa aplikasi — harus cek manual atau download installer baru

### 6.3 Menginstal Aplikasi

Dua metode instalasi utama di Windows:

**Microsoft Store**
Opsi terkurasi dan aman — semua aplikasi telah diverifikasi sebelum tersedia. Tidak tersedia secara default di Windows Server.

**Dari Internet**
Download installer langsung dari website vendor tepercaya. File installer biasanya berekstensi `.exe` atau `.msi`, dan akan memandu pengguna melalui proses instalasi step-by-step.

### 6.4 Menghapus Aplikasi

Empat cara uninstall aplikasi di Windows:

- Lewat **Microsoft Store** — untuk app yang diinstal dari sana
- **Settings → Apps & Features** — fitur Add or Remove Programs
- **Control Panel → Programs → Uninstall a program** — cara lama (legacy)
- **Built-in uninstaller** — uninstaller bawaan milik aplikasi itu sendiri

---

## 7. Konfigurasi Sistem: Settings & Control Panel

Ada dua pusat konfigurasi di Windows — keduanya aktif dan saling melengkapi.

### 7.1 Windows Settings

Antarmuka modern dan terpusat untuk konfigurasi sehari-hari. Shortcut tersedia di Desktop maupun Start Menu.

Kategori utama di Windows Settings:

| Kategori | Isi |
|---|---|
| System | Display, sound, notifications, power |
| Devices | Bluetooth, printers, mouse |
| Network & Internet | Wi-Fi, airplane mode, VPN |
| Personalization | Background, lock screen, colors |
| Apps | Uninstall, defaults, optional features |
| Accounts | Email, sync, work, other people |
| Time & Language | Speech, region, date |
| Ease of Access | Narrator, magnifier, high contrast |
| Privacy | Location, camera |
| Update & Security | Windows Update, recovery, backup |
| Search | Language, permissions, history |

### 7.2 Control Panel

Antarmuka lama (legacy) yang masih dibutuhkan untuk tugas-tugas administratif tertentu yang belum sepenuhnya dipindah ke Windows Settings.

Kategori utama di Control Panel:

| Kategori | Isi |
|---|---|
| System and Security | Review status komputer, event logs |
| Network and Internet | Status jaringan dan task |
| Hardware | Devices and printers, tambah perangkat |
| Programs | Uninstall program, Windows features |
| User Accounts | Ganti tipe akun |
| Appearance and Personalization | Tampilan dan tema |
| Clock and Region | Tanggal, waktu, format angka |
| Ease of Access | Optimize untuk aksesibilitas |

---

## 8. Task Manager

**Task Manager** adalah tool monitoring bawaan Windows yang memantau kondisi sistem secara real time — proses yang berjalan, penggunaan resource, hingga layanan aktif.

Cara akses:
- Shortcut di Desktop
- Ctrl + Shift + Esc
- Klik kanan Taskbar → Task Manager

### 8.1 Lima Tab Task Manager

**Processes**
Menampilkan semua aplikasi dan background process yang sedang berjalan beserta penggunaan resource masing-masing (CPU, memory). Dari tab ini bisa langsung mengakhiri proses yang bermasalah lewat tombol "End Task".

**Performance**
Grafik dan statistik real-time untuk CPU, memori, disk, dan jaringan. Berguna untuk mendeteksi bottleneck atau penggunaan resource yang tidak wajar.

**Users**
Daftar pengguna yang sedang login beserta resource yang mereka konsumsi.

**Details**
Tampilan teknis lebih dalam dari setiap proses — termasuk **PID (Process ID)**, status, dan nama file executable yang menjalankannya.

**Services**
Daftar semua Windows services beserta status masing-masing (running atau stopped). Berguna untuk memeriksa layanan sistem yang seharusnya aktif.

---

## 9. Keamanan Bawaan Windows

### 9.1 Windows Security

**Windows Security** adalah dashboard terpusat untuk semua tools perlindungan bawaan Windows. Aktif secara default dan bisa diakses lewat shortcut Desktop atau Start Menu.

Terbagi menjadi empat bagian:

**Virus & threat protection**
Mendeteksi dan menghapus malware menggunakan real-time protection yang berjalan terus di background, plus opsi untuk menjalankan scan manual dengan berbagai mode.

**Firewall & network protection**
Mengontrol lalu lintas jaringan masuk dan keluar. Mencegah koneksi tidak sah dari dan ke sistem.

**App & browser control**
Melindungi pengguna dari aplikasi, file, dan website yang berpotensi berbahaya.

**Device security**
Perlindungan berbasis hardware untuk lapisan keamanan tambahan di level perangkat.

### 9.2 Jenis Scan Virus & Threat Protection

| Jenis Scan | Cara Kerja | Keterangan |
|---|---|---|
| Quick scan | Periksa folder umum tempat ancaman biasa bersembunyi | Cepat, cocok untuk pemeriksaan rutin |
| Full scan | Periksa semua file dan program di hard disk | Bisa memakan waktu lebih dari 1 jam |
| Custom scan | Pilih sendiri file atau lokasi yang ingin diperiksa | Paling fleksibel, digunakan di lab ini |

### 9.3 Cara Menjalankan Custom Scan

1. Buka **Windows Security** lewat shortcut Desktop atau Start Menu
2. Pilih **Virus & threat protection**
3. Klik **Scan options**
4. Pilih **Custom scan**
5. Klik **Scan now**
6. Pilih folder target → klik **Select Folder**
7. Tunggu scan selesai
8. Klik **See details** untuk melihat detail ancaman yang ditemukan

### 9.4 EICAR Test File

File uji standar industri keamanan siber yang sengaja dibuat agar terdeteksi oleh antivirus — tanpa mengandung kode berbahaya sungguhan. Digunakan para profesional untuk memverifikasi bahwa sistem deteksi ancaman berfungsi dengan benar.

Nama yang muncul saat terdeteksi:
- `Tool:Win32/EICAR_Test_File`
- `Virus:DOS/EICAR_Test_File`

Tingkat ancaman yang ditampilkan: **Severe** (ini memang disengaja sebagai bagian dari desain test file).

Opsi tindakan yang tersedia setelah terdeteksi:
- **Allow on device** — izinkan file tetap ada (tidak disarankan kecuali untuk testing)
- **Quarantine** — isolasi file agar tidak bisa dieksekusi (default dan direkomendasikan)
- **Remove** — hapus file permanen

Jika file sudah dikarantina otomatis sebelum scan dijalankan, cek di bagian **Threat history** atau **Protection history** di Virus & threat protection.

---

## 10. Windows Defender Firewall

**Windows Defender Firewall** adalah firewall bawaan yang memantau semua koneksi jaringan masuk dan keluar, lalu menerapkan aturan (rules) untuk menentukan koneksi mana yang diizinkan atau diblokir.

### 10.1 Tiga Profil Jaringan

**Domain**
Aktif ketika sistem terhubung ke jaringan domain organisasi/kantor. Pengaturan dikelola oleh administrator jaringan.

**Private**
Untuk jaringan tepercaya seperti jaringan rumah atau lab. Level kepercayaan lebih tinggi dari Public.

**Public**
Untuk jaringan tidak tepercaya seperti Wi-Fi di kafe, bandara, atau tempat umum. Setting paling ketat.

### 10.2 Allowed Applications

Di dalam Firewall Settings terdapat daftar **Allowed Apps** — aplikasi yang secara eksplisit diizinkan berkomunikasi melewati firewall. Setiap aplikasi bisa diatur izinnya per profil (Private dan/atau Public).

Cara akses: Control Panel → System and Security → Windows Defender Firewall → Allow an app or feature through Windows Defender Firewall.

### 10.3 Advanced Settings

Di **Advanced Settings** (Windows Defender Firewall with Advanced Security) tersedia tiga hal:

1. Gambaran menyeluruh aturan inbound, outbound, dan connection security rules
2. Detail setiap aturan — nama, grup, profil jaringan, status (enabled/disabled), dan action (allow/block)
3. Kemampuan membuat aturan baru atau memfilter tampilan daftar aturan yang ada

Tiga kategori rules di panel kiri:
- **Inbound Rules** — mengatur koneksi yang masuk ke sistem
- **Outbound Rules** — mengatur koneksi yang keluar dari sistem
- **Connection Security Rules** — mengatur autentikasi dan enkripsi koneksi

---

## 11. Glossary

**GUI** — Graphical User Interface; antarmuka berbasis visual yang berinteraksi lewat klik, ikon, dan menu — bukan perintah teks.

**CLI** — Command-Line Interface; antarmuka berbasis teks di mana semua interaksi dilakukan dengan mengetik perintah. Dibahas di modul lanjutan.

**Desktop** — Ruang kerja utama Windows, area pertama yang muncul setelah login.

**Taskbar** — Strip kontrol di bagian bawah layar yang menyediakan akses ke semua komponen utama sistem.

**Start Menu** — Menu utama akses ke semua aplikasi, setting, dan power option, ditandai dengan logo Windows.

**File Explorer** — Tool bawaan Windows untuk menjelajahi, mengelola, dan mengorganisir file dan folder.

**File Path** — Alamat lengkap lokasi sebuah file atau folder di sistem, dimulai dari drive root. Contoh: `C:\Users\Administrator\Desktop\`.

**Windows Update** — Tool bawaan untuk memperbarui OS, aplikasi native, dan fitur keamanan Windows.

**Microsoft Store** — Toko aplikasi resmi Windows dengan kurasi keamanan bawaan.

**Windows Settings** — Pusat konfigurasi modern Windows yang terpusat dan intuitif.

**Control Panel** — Antarmuka konfigurasi lama (legacy) yang masih digunakan untuk task administratif tertentu.

**Task Manager** — Tool monitoring bawaan untuk memantau proses, performa, dan layanan sistem secara real time.

**PID** — Process ID; nomor unik yang diassign sistem ke setiap proses yang sedang berjalan.

**Windows Security** — Dashboard terpusat untuk semua tool keamanan bawaan Windows.

**Quarantine** — Proses mengisolasi file berbahaya ke area terpisah sehingga tidak bisa dieksekusi, tanpa langsung menghapusnya.

**Real-time protection** — Pemantauan ancaman yang berjalan terus-menerus di background tanpa perlu scan manual.

**Firewall** — Sistem yang mengontrol lalu lintas jaringan masuk dan keluar berdasarkan aturan yang telah ditetapkan.

**Inbound rules** — Aturan firewall yang mengatur koneksi yang masuk ke sistem.

**Outbound rules** — Aturan firewall yang mengatur koneksi yang keluar dari sistem.

**EICAR Test File** — File uji standar industri untuk memverifikasi fungsi antivirus, tidak berbahaya namun sengaja dirancang agar terdeteksi sebagai ancaman.

**Domain profile** — Profil jaringan firewall untuk koneksi ke domain organisasi.

**Private profile** — Profil jaringan firewall untuk jaringan tepercaya (rumah/lab).

**Public profile** — Profil jaringan firewall untuk jaringan tidak tepercaya (Wi-Fi publik).

**Authentication** — Proses membuktikan identitas pengguna sebelum diberi akses ke sistem.

**Permission level** — Tingkat izin yang ditetapkan ke setiap akun pengguna, menentukan apa yang boleh dan tidak boleh dilakukan di sistem.

**.exe** — Format file executable Windows; digunakan sebagai installer aplikasi yang diunduh dari internet.

**.msi** — Format installer Windows Installer; alternatif dari .exe untuk instalasi aplikasi.

---

## 12. Tools & Platform Rujukan

**TryHatMe (platform lab)**
Platform yang digunakan dalam skenario lab room ini. Menyediakan virtual environment Windows Server 2019 untuk praktik langsung.

**Windows Update**
Tool bawaan Windows (bukan platform eksternal) — diakses lewat Settings → Update & Security → Windows Update.

**Microsoft Store**
Toko aplikasi resmi Microsoft — https://apps.microsoft.com (versi web) atau langsung dari aplikasi Microsoft Store di Windows.

**Windows Security**
Aplikasi bawaan Windows — diakses lewat Start Menu atau Search, tidak memerlukan instalasi terpisah.

**Windows Defender Firewall**
Tersedia melalui Control Panel → System and Security → Windows Defender Firewall, atau langsung dari Windows Security.

---

## 13. Catatan Ringkas untuk Ditulis Tangan

### Sejarah Windows
- MS-DOS — layar hitam, ketik perintah, tanpa ikon
- Win 1.0 (1985) — GUI pertama, mouse + menu, di atas DOS
- Win ME (2000) — lebih modern
- Win 11 (2021) — tampilan terkini
- GUI — antarmuka grafis (klik & ikon)
- CLI — antarmuka teks (ketik perintah), dipakai di modul lanjutan

### Tipe Akun
- Guest — akses sementara, izin minimal, tidak bisa ubah sistem
- Standard — harian, jalankan app, ubah setting pribadi saja
- Administrator — kontrol penuh: install, konfigurasi, kelola user

### Komponen Desktop
- Desktop — ruang kerja utama, tempat file/folder/shortcut
- Taskbar — strip bawah, akses semua komponen sistem
- 8 elemen taskbar: Desktop Icons, Start Menu, Search, Task View, Pinned Apps, Network & Audio, Date/Time, Notifications

### Start Menu
- Klik ikon Windows pojok kiri bawah
- Kiri = daftar semua app (alfabetis)
- Kanan = tile app favorit (quick access)
- Bawah = user options + power

### File Explorer & Path
- Struktur hierarkis: Drive → Folder → Subfolder → File
- Path contoh: C:\Users\Administrator\Desktop\NamaFolder
- Klik address bar → path lengkap tampil dan bisa disalin
- Lokasi utama: Desktop, Documents, Downloads

### About Your PC
- Akses: Settings → About your PC / Search "About your PC"
- Security Details — status firewall & perlindungan
- Device Details — nama PC, processor, RAM, Device ID
- OS Details — edisi, versi, tanggal install, OS build

### Manajemen Aplikasi
- Update OS — Windows Update (Settings → Update & Security)
  - Isi: security patch, performance update, bug fix
- Install app — 2 cara: Microsoft Store / download .exe atau .msi dari internet
- Uninstall app — 4 cara: Microsoft Store / Settings Apps & Features / Control Panel Programs / built-in uninstaller

### Settings vs Control Panel
- Windows Settings — modern, terpusat, sehari-hari
- Control Panel — legacy, untuk task admin spesifik
- Keduanya: kelola display, audio, user, app, network, security

### Task Manager
- Akses: Ctrl+Shift+Esc / shortcut Desktop / klik kanan Taskbar
- 5 tab:
  - Processes — app & background process + resource usage
  - Performance — grafik CPU, memori, jaringan
  - Users — user login + resource usage
  - Details — tampilan teknis + PID
  - Services — layanan Windows + status (running/stopped)
- PID — Process ID, nomor unik tiap proses

### Windows Security (4 bagian)
- Virus & threat protection — deteksi & hapus malware, real-time + scan
- Firewall & network protection — kontrol traffic jaringan
- App & browser control — lindungi dari app/file/web berbahaya
- Device security — perlindungan berbasis hardware

### Jenis Scan
- Quick scan — folder umum, cepat
- Full scan — semua file, bisa >1 jam
- Custom scan — pilih folder spesifik sendiri

### Cara Custom Scan
1. Buka Windows Security
2. Virus & threat protection
3. Scan options
4. Custom scan → Scan now
5. Pilih folder → Select Folder
6. See details untuk lihat hasil

### EICAR Test File
- File uji standar industri, tidak berbahaya
- Sengaja terdeteksi antivirus sebagai ancaman
- Nama: Tool:Win32/EICAR_Test_File atau Virus:DOS/EICAR_Test_File
- Status ancaman: Severe (disengaja)
- Action: Allow / Quarantine (default) / Remove
- Kalau tidak muncul di scan → cek Threat history / Protection history

### Windows Defender Firewall
- Monitor koneksi masuk & keluar, terapkan rules allow/deny
- 3 profil: Domain (kantor/organisasi), Private (rumah/lab), Public (Wi-Fi umum)
- Inbound rules — koneksi masuk ke sistem
- Outbound rules — koneksi keluar dari sistem
- Connection Security Rules — autentikasi & enkripsi
- Advanced Settings: lihat/buat rules, filter tampilan, detail per rule

### Istilah Wajib Hapal
- Authentication — verifikasi identitas sebelum akses
- Permission level — tingkat izin per akun
- Quarantine — isolasi file berbahaya tanpa hapus
- Real-time protection — pemantauan aktif terus-menerus
- PID — Process ID, nomor unik tiap proses berjalan
- File path — alamat lengkap lokasi file di sistem
- .exe / .msi — format file installer Windows
