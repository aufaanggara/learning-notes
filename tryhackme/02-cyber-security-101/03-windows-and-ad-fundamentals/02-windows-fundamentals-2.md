# Windows Fundamentals 2 — Resume Materi

**Tanggal:** 16 Mei 2026
**Room:** TryHackMe — Windows Fundamentals 2

---

## Daftar Isi

1. System Configuration (MSConfig)
2. User Account Control (UAC)
3. Advanced System Settings
4. Computer Management
5. System Information (msinfo32)
6. Resource Monitor (resmon)
7. Command Prompt (cmd)
8. Registry Editor (regedit)
9. Glossary
10. Tools & Platform Rujukan
11. Catatan Ringkas untuk Ditulis Tangan

---

## 1. System Configuration (MSConfig)

**MSConfig** adalah utilitas untuk advanced troubleshooting, dengan tujuan utama mendiagnosis masalah startup Windows.

Untuk membukanya: ketik `msconfig` di Start Menu atau Win+R.
Syarat akses: harus memiliki **local administrator rights**.

### 1.1 Lima Tab MSConfig

**Tab General** — memilih mode startup Windows saat boot:

- **Normal startup** — memuat semua device driver dan service
- **Diagnostic startup** — memuat komponen dasar saja (minimal)
- **Selective startup** — pilih manual: Load system services, Load startup items, Use original boot configuration

**Tab Boot** — mengatur opsi boot sistem operasi:

- Pilih OS default, atur timeout, aktifkan Safe boot
- Opsi tambahan: No GUI boot, Boot log, Base video, OS boot information
- Tombol Advanced options untuk konfigurasi lebih lanjut
- Centang "Make all boot settings permanent" kalau ingin perubahan permanen

**Tab Services** — menampilkan semua service yang terdaftar di sistem, baik yang sedang berjalan (Running) maupun berhenti (Stopped).

- Bisa filter dengan centang **Hide all Microsoft services**
- Service adalah aplikasi khusus yang berjalan di latar belakang

**Tab Startup** — di Windows Server, tab ini kosong.

- Microsoft menyarankan penggunaan **Task Manager (taskmgr)** untuk manage startup items
- MSConfig bukan program manajemen startup
- Di Windows Server, lihat startup items lewat: Win+R → `shell:startup`

**Tab Tools** — daftar utilitas yang bisa dijalankan langsung dari MSConfig.

- Setiap tool punya **Selected command** = path lengkap executable-nya
- Bisa dijalankan via Win+R atau CMD tanpa membuka MSConfig terlebih dahulu
- Klik **Launch** untuk jalankan langsung dari tab Tools

### 1.2 Ringkasan Tools di Tab Tools

| Tool | Command (Win+R) | Fungsi Singkat |
|---|---|---|
| About Windows | `winver` | Info versi & lisensi Windows |
| Change UAC Settings | via Control Panel | Ubah level UAC |
| Windows Troubleshooting | `control /name Microsoft.Troubleshooting` | Troubleshoot masalah |
| Computer Management | `compmgmt.msc` | Kelola sistem, storage, service |
| System Information | `msinfo32` | Info lengkap hardware & software |
| Event Viewer | `eventvwr` | Log kejadian sistem |
| System Properties | `control system` | Info dasar sistem |
| Internet Options | `inetcpl.cpl` | Pengaturan internet |
| Registry Editor | `regedit` | Edit Windows Registry |
| Resource Monitor | `resmon` | Monitor CPU/Disk/Network/Memory |
| Command Prompt | `cmd` | Terminal teks |
| Performance Monitor | `perfmon` | Monitor performa real-time |

---

## 2. User Account Control (UAC)

**UAC** adalah mekanisme keamanan Windows yang memperingatkan pengguna ketika ada perubahan di level sistem.

Pengaturan UAC bisa diubah melalui slider dengan empat level:

**Always notify** — level keamanan tertinggi. Windows memberi notifikasi untuk setiap perubahan, baik dari aplikasi maupun dari pengguna sendiri. Layar meredup (Secure Desktop) saat notifikasi muncul.

**Notify for apps** — level default. Windows hanya memberi notifikasi ketika aplikasi mencoba membuat perubahan, bukan ketika pengguna mengubah pengaturan Windows sendiri.

**Notify without dimming** — sama seperti Notify for apps, tetapi layar tidak meredup saat notifikasi muncul.

**Never notify** — semua notifikasi dimatikan. Windows tidak memperingatkan perubahan apapun dari pengguna maupun aplikasi. Tidak disarankan.

Posisi slider saat ini bisa dilihat di jendela **User Account Control Settings**.

---

## 3. Advanced System Settings

Diakses dengan mencari "View advanced system settings" di search bar, yang akan membuka panel **System Properties**.

### 3.1 Tab Hardware

**Device Manager** — menampilkan semua hardware yang terpasang di komputer. Digunakan untuk mengubah properties atau menonaktifkan perangkat.

**Device Installation Settings** — mengatur apakah Windows mengunduh driver dan ikon dari produsen secara otomatis.

### 3.2 Tab Advanced

Tab ini membutuhkan login sebagai Administrator untuk sebagian besar perubahan.

**Performance** — mengatur visual effects, processor scheduling, memory usage, dan virtual memory.

Untuk mengakses: klik **Settings** di bagian Performance, lalu buka tab **Advanced** di jendela Performance Options.

**Virtual Memory (Page File)**

Page file adalah area di hard disk yang digunakan Windows sebagai pengganti RAM ketika RAM fisik penuh. Mencegah crash atau perlambatan akibat kehabisan memori.

Informasi yang tersedia di pengaturan page file:
- Drive tempat page file disimpan
- Initial size (MB)
- Maximum size
- Apakah Windows mengelola ukurannya secara otomatis

**User Profiles** — mengatur desktop settings yang terkait dengan akun sign-in pengguna.

**Startup and Recovery**

Windows dapat membuat **crash dump file** ketika mengalami error kritis seperti Blue Screen of Death (BSOD). File ini membantu administrator atau analis memahami apa yang salah saat crash terjadi.

Akses: Tab Advanced → Settings di bagian Startup and Recovery.

Tipe crash dump yang tersedia (ditentukan oleh dropdown **Write debugging information**):

- **Automatic memory dump** — Windows menentukan sendiri apa yang disimpan
- **Kernel memory dump** — menyimpan data kernel saja
- **Small memory dump (256 KB)** — data minimal, ukuran paling kecil
- **Complete memory dump** — menyimpan seluruh isi RAM
- **None** — tidak membuat crash dump

File dump default disimpan di: `%SystemRoot%\MEMORY.DMP`

---

## 4. Computer Management

**Computer Management** (`compmgmt.msc`) adalah utilitas terpusat yang mengelola sistem lokal. Terdiri dari tiga bagian utama.

### 4.1 System Tools

**Task Scheduler**

Digunakan untuk membuat dan mengelola task yang berjalan otomatis pada waktu atau kondisi tertentu.

Task bisa dikonfigurasi untuk berjalan:
- Saat login atau logoff
- Pada jadwal tertentu (misal setiap hari pukul 10.00)
- Saat system startup
- Sekali pada waktu tertentu (non-recurring)

Untuk melihat task yang ada: klik **Task Scheduler Library** di panel kiri.
Untuk membuat task baru: klik **Create Basic Task** di panel Actions (kanan).

Setiap task memiliki tab detail: General, Triggers, Actions, Conditions, Settings, History.

**Event Viewer**

Digunakan untuk melihat semua event/kejadian yang telah terjadi di komputer. Bisa digunakan sebagai audit trail untuk mendiagnosis masalah dan menyelidiki aktivitas sistem.

Event Viewer memiliki tiga panel:
1. Panel kiri — hierarki tree dari event log providers
2. Panel tengah — overview dan ringkasan event
3. Panel kanan — actions pane

Lima tipe event yang dicatat:

| Tipe | Keterangan |
|---|---|
| **Error** | Masalah signifikan seperti service gagal load saat startup |
| **Warning** | Potensi masalah masa depan, misal disk space hampir habis |
| **Information** | Operasi berhasil, misal network driver berhasil dimuat |
| **Success Audit** | Akses security berhasil, misal login sukses |
| **Failure Audit** | Akses security gagal, misal gagal akses network drive |

Empat standard log di bawah **Windows Logs**:

| Log | Isi |
|---|---|
| **Application** | Event dari aplikasi; developer yang menentukan apa yang dicatat |
| **Security** | Logon attempt valid/invalid, akses dan modifikasi file/objek |
| **System** | Event dari komponen sistem, misal driver gagal load saat startup |
| **CustomLog** | Event dari aplikasi yang membuat log sendiri untuk kontrol ukuran/ACL |

**Shared Folders**

Menampilkan daftar lengkap semua folder yang dibagikan di jaringan.

Default shares yang dibuat Windows:
- **ADMIN$** → C:\Windows (Remote Admin)
- **C$** → C:\ (Default share)
- **IPC$** → Remote IPC

Sub-node:
- **Sessions** — daftar user yang sedang terhubung ke shares
- **Open Files** — file yang sedang dibuka oleh connected users

Untuk melihat permissions: klik kanan folder → Properties.

**Local Users and Groups** — sama seperti `lusrmgr.msc` yang dibahas di Windows Fundamentals 1.

**Performance** — berisi **Performance Monitor** (`perfmon`). Digunakan untuk melihat data performa secara real-time maupun dari log file, baik untuk komputer lokal maupun remote.

**Device Manager** — menampilkan dan mengkonfigurasi semua hardware yang terpasang, termasuk menonaktifkan perangkat tertentu jika diperlukan.

### 4.2 Storage

**Disk Management** — utilitas untuk advanced storage tasks.

Tugas yang bisa dilakukan:
- Set up a new drive
- Extend a partition
- Shrink a partition
- Assign or change a drive letter (contoh: E:)

Catatan: karena VM yang digunakan adalah Windows Server, ada utilitas storage tambahan yang biasanya tidak ada di Windows 10.

### 4.3 Services and Applications

**Services** — menampilkan semua service beserta display name, status, dan informasi lainnya.

Untuk detail lebih lanjut: klik kanan service → **Properties**.

Informasi yang tersedia di Properties:
- **Service name** — nama teknis (berbeda dari display name)
- **Display name** — nama yang terlihat di daftar
- **Description** — penjelasan fungsi service
- **Path to executable** — lokasi file executable service
- **Startup type** — menentukan kapan service dijalankan:
  - **Automatic** — berjalan otomatis setiap kali sistem boot
  - **Manual** — hanya berjalan ketika dipicu oleh proses lain atau pengguna
  - **Disabled** — tidak boleh berjalan sama sekali
- **Service status** — Running / Stopped

**WMI Control** — mengkonfigurasi dan mengontrol layanan **Windows Management Instrumentation** (WMI).

WMI memungkinkan scripting language seperti VBScript atau PowerShell untuk mengelola Windows secara lokal maupun remote. Microsoft juga menyediakan command-line interface bernama **WMIC** (Windows Management Instrumentation Command-line).

Catatan: WMIC sudah deprecated di Windows 10 versi 21H1. PowerShell menggantikan fungsinya untuk WMI.

---

## 5. System Information (msinfo32)

**msinfo32** adalah tool bawaan Windows yang mengumpulkan dan menampilkan informasi komprehensif tentang hardware, komponen, dan software environment. Berguna untuk mendiagnosis masalah komputer.

System Summary dibagi menjadi tiga bagian utama:

### 5.1 Hardware Resources

Ditujukan untuk pengguna tingkat lanjut, bukan untuk pengguna umum.

Sub-kategori: Conflicts/Sharing, DMA, Forced Hardware, I/O, IRQs, Memory.

### 5.2 Components

Menampilkan informasi spesifik tentang perangkat hardware yang terpasang.

Struktur lengkap:
- Multimedia (Audio Codecs, Video Codecs)
- CD-ROM, Sound Device, Display, Infrared
- Input (Keyboard, Pointing Device)
- Modem
- Network (Adapter, Protocol, WinSock)
- Ports, Storage (Drives, Disks, SCSI, IDE)
- Printing, Problem Devices, USB

Bagian yang biasanya memiliki data: **Display** dan **Input**.

### 5.3 Software Environment

Menampilkan informasi tentang software yang tertanam di OS maupun yang sudah diinstall.

Sub-kategori: System Drivers, **Environment Variables**, Print Jobs, Network Connections, Running Tasks, Loaded Modules, Services, Program Groups, Startup Programs, OLE Registration, Windows Error Reporting.

**Environment Variables**

Variabel yang menyimpan informasi tentang environment sistem operasi, seperti path OS, jumlah prosesor, dan lokasi folder temporary.

Contoh:
- `%WINDIR%` → lokasi direktori instalasi Windows (biasanya C:\Windows)
- `%TEMP%` → lokasi folder temporary
- `%SystemRoot%` → sama dengan %WINDIR%

Dua jenis environment variables:
- **User variables** — berlaku hanya untuk user yang sedang login
- **System variables** — berlaku untuk semua user di komputer tersebut

Cara mengakses selain via msinfo32:
- Control Panel → System and Security → System → Advanced system settings → Environment Variables
- Settings → System → About → system info → Advanced system settings → Environment Variables

### 5.4 Fitur Search di msinfo32

Di bagian bawah msinfo32 terdapat search bar (**Find what**).

Gunakan centang **Search selected category only** untuk membatasi pencarian hanya pada kategori yang sedang dipilih di panel kiri, sehingga hasil lebih spesifik dan relevan.

---

## 6. Resource Monitor (resmon)

**resmon** menampilkan informasi penggunaan CPU, memory, disk, dan network per proses maupun secara agregat. Juga menampilkan detail tentang proses mana yang menggunakan file handles dan modules tertentu.

Kemampuan lain: start, stop, pause, resume service; menutup aplikasi yang tidak responsif; mengidentifikasi deadlocked processes dan file locking conflicts.

Ditujukan untuk pengguna tingkat lanjut yang membutuhkan advanced troubleshooting.

### 6.1 Struktur Tab

**Tab Overview** — ringkasan empat bagian sekaligus (CPU, Disk, Network, Memory).

**Tab CPU** — terbagi menjadi empat sub-bagian:
- **Processes** — daftar proses beserta status, threads, CPU usage, dan average CPU
- **Services** — service yang berjalan di bawah proses tersebut
- **Associated Handles** — file handles yang digunakan proses
- **Associated Modules** — DLL/modul yang dimuat proses

**Tab Memory** — terbagi menjadi dua sub-bagian:
- **Processes** — penggunaan memori per proses (Hard Faults/sec, Commit, Working Set, Shareable, Private)
- **Physical Memory** — representasi visual keseluruhan RAM:
  - Hardware Reserved — dicadangkan untuk hardware
  - In Use — sedang digunakan
  - Modified — sedang dalam proses ditulis ke disk
  - Standby — siap digunakan, bisa dilepas kapan saja
  - Free — benar-benar kosong

**Tab Disk** — terbagi menjadi tiga sub-bagian:
- **Processes with Disk Activity** — proses yang sedang aktif baca/tulis disk
- **Disk Activity** — detail file yang sedang dibaca/ditulis beserta kecepatan
- **Storage** — info logical disk (active time, available space, total space)

**Tab Network** — terbagi menjadi empat sub-bagian:
- **Processes with Network Activity** — proses yang menggunakan jaringan
- **Network Activity** — detail traffic per proses (send/receive/total)
- **TCP Connections** — koneksi TCP aktif (local address, remote address, port, latency)
- **Listening Ports** — port yang sedang mendengarkan koneksi masuk

Panel di sisi paling kanan menampilkan grafik real-time untuk setiap bagian (tidak terlihat di screenshot, tapi selalu ada).

---

## 7. Command Prompt (cmd)

Command prompt tersedia sejak sebelum GUI ada. Meskipun GUI adalah cara utama berinteraksi dengan Windows, CMD tetap bisa digunakan dan sering lebih efisien untuk tugas-tugas tertentu.

### 7.1 Perbedaan \ dan /

Ini adalah konsep fundamental yang sering membingungkan:

- **Backslash `\`** — pemisah antar folder dalam sebuah path: `C:\Windows\System32\cmd.exe`
- **Forward slash `/`** — parameter atau opsi yang ditambahkan ke sebuah command: `ipconfig /all`

### 7.2 Command Dasar

```
hostname
```
Menampilkan nama komputer.

```
whoami
```
Menampilkan nama user yang sedang login, dalam format `domain\username`.

```
cls
```
Membersihkan/mengosongkan layar terminal.

### 7.3 Command Jaringan & Troubleshooting

**ipconfig** — menampilkan konfigurasi network address komputer.

```
ipconfig
```
Tampilkan info dasar: IPv4 address, subnet mask, default gateway.

```
ipconfig /all
```
Tampilkan semua informasi konfigurasi network secara lengkap.

```
ipconfig /flushdns
```
Bersihkan DNS cache.

```
ipconfig /renew
```
Perbarui IP address dari DHCP server.

```
ipconfig /release
```
Lepaskan IP address yang didapat dari DHCP.

```
ipconfig /displaydns
```
Tampilkan isi DNS cache.

```
ipconfig /registerdns
```
Daftarkan ulang DNS records.

```
ipconfig /?
```
Tampilkan help manual lengkap untuk ipconfig.

---

**netstat** — menampilkan protocol statistics dan koneksi TCP/IP yang aktif.

```
netstat
```
Tampilkan koneksi aktif saat ini.

Syntax lengkap dari help manual:
```
NETSTAT [-a] [-b] [-e] [-f] [-n] [-o] [-p proto] [-r] [-s] [-x] [-t] [interval]
```

Switch-switch netstat:

| Switch | Fungsi |
|---|---|
| `-a` | Tampilkan semua koneksi dan listening ports |
| `-b` | Tampilkan executable yang membuat setiap koneksi |
| `-e` | Tampilkan Ethernet statistics |
| `-f` | Tampilkan fully qualified domain name untuk foreign addresses |
| `-n` | Tampilkan alamat dan port dalam bentuk numerik |
| `-o` | Tampilkan process ID (PID) yang memiliki koneksi |
| `-p proto` | Tampilkan koneksi untuk protokol tertentu (TCP, UDP, TCPv6, UDPv6) |
| `-r` | Tampilkan routing table |
| `-s` | Tampilkan statistics per protokol |
| `-x` | Tampilkan koneksi NetworkDirect |
| `-t` | Tampilkan TCP connection offload state |
| `interval` | Refresh statistik setiap N detik |

---

**net** — command serbaguna untuk mengelola network resources. Mendukung sub-commands.

```
net
```
Tanpa sub-command, menampilkan daftar sub-command yang tersedia.

Penting: untuk command `net`, `/? tidak bekerja`. Gunakan:

```
net help
```
Tampilkan daftar semua sub-command yang tersedia.

```
net help user
```
Tampilkan help untuk sub-command user (ganti "user" dengan sub-command lain sesuai kebutuhan).

Sub-commands net yang penting:

| Sub-command | Fungsi |
|---|---|
| `net user` | Kelola user accounts; tanpa argumen = tampilkan daftar user |
| `net localgroup` | Kelola local groups |
| `net use` | Kelola koneksi ke network resources |
| `net share` | Kelola shared resources |
| `net session` | Lihat sesi aktif |
| `net start` | Start sebuah service |
| `net stop` | Stop sebuah service |
| `net accounts` | Kelola kebijakan password dan lockout |
| `net computer` | Tambah/hapus komputer dari domain |
| `net config` | Tampilkan/ubah konfigurasi service |
| `net statistics` | Tampilkan statistik penggunaan jaringan |
| `net time` | Sinkronisasi waktu dengan komputer lain |
| `net view` | Tampilkan daftar komputer atau resources yang dibagikan |

### 7.4 Cara Menggunakan Help Manual

Untuk hampir semua command:
```
command /?
```

Khusus untuk `net`:
```
net help
net help [sub-command]
```

---

## 8. Registry Editor (regedit)

**Windows Registry** adalah database hierarkis terpusat yang menyimpan informasi konfigurasi sistem untuk pengguna, aplikasi, dan perangkat hardware.

Registry berisi informasi yang terus-menerus direferensikan Windows selama beroperasi:
- Profiles untuk setiap user
- Aplikasi yang terinstall dan tipe dokumen yang bisa dibuat
- Property sheet settings untuk folder dan application icons
- Hardware yang ada di sistem
- Port-port yang sedang digunakan

**Peringatan:** Registry ditujukan untuk pengguna tingkat lanjut. Perubahan yang salah dapat merusak operasi normal komputer.

### 8.1 Lima Hive Utama

**HKEY_CLASSES_ROOT** — menyimpan file associations. Menentukan aplikasi apa yang membuka tipe file tertentu (contoh: .txt dibuka dengan Notepad).

**HKEY_CURRENT_USER** — menyimpan konfigurasi dan preferensi untuk user yang sedang aktif login.

**HKEY_LOCAL_MACHINE** — menyimpan konfigurasi hardware dan software untuk seluruh sistem, berlaku untuk semua user.

**HKEY_USERS** — menyimpan profil semua user yang pernah login di komputer tersebut.

**HKEY_CURRENT_CONFIG** — menyimpan informasi hardware profile yang digunakan sistem pada saat boot ini.

Registry Editor bisa diakses via: Win+R → `regedit`, atau melalui MSConfig → Tab Tools → Registry Editor → Launch.

---

## 9. Glossary

**Service** — aplikasi khusus yang berjalan di background, tidak memerlukan interaksi pengguna untuk beroperasi.

**Hive** — folder root/utama dalam struktur Windows Registry yang mengelompokkan data berdasarkan kategori.

**Page file** — area di hard disk yang digunakan Windows sebagai RAM cadangan ketika RAM fisik penuh.

**Crash dump** — file yang dibuat Windows saat terjadi error kritis (BSOD), berisi snapshot kondisi memori saat crash untuk keperluan analisis.

**Audit trail** — catatan kronologis semua aktivitas dan kejadian sistem, disimpan dalam Event Viewer.

**Environment variable** — variabel yang menyimpan informasi path atau konfigurasi yang digunakan OS dan program secara referensial.

**Startup type** — pengaturan yang menentukan kapan sebuah service dijalankan: Automatic (saat boot), Manual (saat dipicu), atau Disabled (tidak pernah).

**Deadlock** — kondisi di mana dua atau lebih proses saling menunggu satu sama lain, sehingga keduanya tidak bisa melanjutkan eksekusi.

**Deadlocked process** — proses yang terjebak dalam kondisi deadlock dan tidak bisa diselesaikan tanpa intervensi eksternal.

**File locking conflict** — kondisi di mana sebuah file dikunci oleh satu proses sehingga proses lain tidak bisa mengaksesnya.

**.msc file** — Microsoft Console file, yaitu shortcut untuk membuka panel manajemen Windows tertentu (contoh: `compmgmt.msc`, `lusrmgr.msc`).

**Selected command** — path lengkap executable sebuah tool yang ditampilkan di Tab Tools MSConfig; bisa dijalankan langsung via Win+R atau CMD.

**WMI (Windows Management Instrumentation)** — infrastruktur Microsoft untuk manajemen dan monitoring sistem Windows secara lokal maupun remote via scripting.

**WMIC** — command-line interface untuk WMI; sudah deprecated di Windows 10 21H1, digantikan oleh PowerShell.

**CNI (Critical National Infrastructure)** — infrastruktur nasional kritis seperti sistem listrik, air, dan telekomunikasi yang memerlukan keamanan ekstra.

**Leet speak** — gaya penulisan yang mengganti huruf dengan angka atau simbol yang mirip bentuknya (a=4, e=3, o=0), sering dipakai untuk menyamarkan nama file atau folder.

---

## 10. Tools & Platform Rujukan

**Windows Event Log documentation** — dokumentasi resmi Microsoft tentang Event Viewer dan Event Logs.
URL: docs.microsoft.com (cari "Windows Event Log")

**Microsoft System Information (msinfo32) documentation** — referensi resmi tentang tool msinfo32 dan interpretasi datanya.
URL: docs.microsoft.com

**Windows Registry documentation** — dokumentasi resmi Microsoft tentang struktur dan penggunaan Windows Registry.
URL: docs.microsoft.com

**Comprehensive list of CMD commands** — daftar lengkap command yang tersedia di Command Prompt Windows.
URL: disebutkan sebagai "here" di materi (link tersedia di room TryHackMe)

---

## 11. Catatan Ringkas untuk Ditulis Tangan

### MSConfig

- msconfig — System Configuration, diagnosa startup, butuh admin rights
- 5 tab: General, Boot, Services, Startup, Tools
- Normal/Diagnostic/Selective startup — mode load saat boot
- Tab Tools → Selected command = path lengkap, bisa dijalankan di Win+R
- Windows Server: Tab Startup kosong → pakai shell:startup

### UAC Levels (atas ke bawah)

- Always notify — semua perubahan + layar redup (max security)
- Notify for apps — default, hanya notif kalau app yang ubah
- Notify w/o dimming — sama tapi layar tidak redup
- Never notify — tidak ada notifikasi sama sekali (berbahaya)

### Advanced System Settings

- Page file — RAM cadangan di hard disk, cegah crash
- Crash dump — rekaman kondisi RAM saat BSOD
- Dump types: Automatic / Kernel / Small (256KB) / Complete / None
- Dump file default: %SystemRoot%\MEMORY.DMP

### Computer Management (compmgmt.msc)

- 3 bagian: System Tools, Storage, Services and Applications
- Task Scheduler — task otomatis berdasarkan trigger (startup/jadwal/login)
- Event Viewer — audit trail, 3 panel, 5 tipe event, 4 standard log
- Shared Folders — daftar share; default: ADMIN$ / C$ / IPC$
- Disk Management — extend/shrink/set up drive/assign drive letter
- Service startup type: Automatic / Manual / Disabled
- WMI Control — deprecated di Win10 21H1, diganti PowerShell

### 5 Tipe Event (Event Viewer)

- Error — masalah signifikan
- Warning — potensi masalah
- Information — operasi berhasil
- Success Audit — akses security berhasil
- Failure Audit — akses security gagal

### 4 Windows Logs

- Application — event dari aplikasi
- Security — logon, akses file
- System — komponen sistem (driver dll)
- CustomLog — app yang bikin log sendiri

### msinfo32

- 3 bagian: Hardware Resources / Components / Software Environment
- Environment Variables — %WINDIR%, %TEMP%, %SystemRoot%
- User variables — hanya untuk user aktif
- System variables — untuk semua user
- Search bar bawah — centang "Search selected category only"

### resmon

- 5 tab: Overview, CPU, Memory, Disk, Network
- Physical Memory: Hardware Reserved / In Use / Modified / Standby / Free
- TCP Connections — koneksi aktif beserta local & remote port
- Listening Ports — port yang menunggu koneksi masuk
- Panel kanan = grafik real-time

### CMD — Command & Switch

- hostname — nama komputer
- whoami — nama user aktif (format domain\user)
- cls — bersihkan layar
- ipconfig — info network dasar
- ipconfig /all — info network lengkap
- ipconfig /flushdns — bersihkan DNS cache
- ipconfig /renew — perbarui IP dari DHCP
- ipconfig /release — lepaskan IP DHCP
- ipconfig /? — help manual ipconfig
- netstat — koneksi TCP/IP aktif
- netstat -a — semua koneksi + listening ports
- netstat -b — tampilkan executable per koneksi
- netstat -n — format numerik
- netstat -o — tampilkan PID
- netstat -r — routing table
- netstat -s — statistik per protokol
- net — kelola network resources (pakai sub-command)
- net help — daftar semua sub-command (bukan net /?)
- net help user — help untuk sub-command user
- net user — kelola user accounts
- net localgroup — kelola local groups
- net use — kelola koneksi network
- net share — kelola shared resources
- net start/stop — start atau stop service
- \ — pemisah folder di path
- / — parameter/opsi command

### Registry Editor (regedit)

- Database hierarkis terpusat, konfigurasi sistem
- WARNING: ubah registry bisa rusak sistem
- 5 Hive:
  - HKEY_CLASSES_ROOT — file associations (.txt → Notepad)
  - HKEY_CURRENT_USER — config user aktif
  - HKEY_LOCAL_MACHINE — config hardware/software seluruh sistem
  - HKEY_USERS — profil semua user
  - HKEY_CURRENT_CONFIG — hardware profile saat boot ini

### Shortcut Penting (Win+R)

- msconfig — System Configuration
- compmgmt.msc — Computer Management
- msinfo32 — System Information
- eventvwr — Event Viewer
- resmon — Resource Monitor
- perfmon — Performance Monitor
- regedit — Registry Editor
- control — Control Panel
- control system — System Properties
- lusrmgr.msc — Local Users and Groups
- shell:startup — folder Startup (untuk Windows Server)
- winver — info versi & lisensi Windows
