# Resume Materi — PowerShell
**Room:** Introduction to PowerShell (TryHackMe)
**Tanggal:** 08 Mei 2026

---

## 1. Apa Itu PowerShell

PowerShell adalah **task automation solution** dari Microsoft yang menggabungkan tiga komponen sekaligus: command-line shell, scripting language, dan configuration management framework.

Berbeda dari CMD yang berbasis teks, PowerShell dibangun di atas **.NET framework** dan bersifat **object-oriented** — artinya setiap output yang dihasilkan adalah object lengkap dengan properties dan methods, bukan sekadar teks mentah.

Awalnya eksklusif untuk Windows, sejak 2016 Microsoft merilis **PowerShell Core** yang bersifat open-source dan berjalan di Windows, macOS, dan Linux.

**Sejarah singkat:**
- Diciptakan oleh **Jeffrey Snover** (Microsoft engineer) pada 2006
- Latar belakang: CMD dan batch files tidak mampu menangani kebutuhan enterprise yang kompleks
- Snover menyadari Windows (berbasis structured data & API) dan Unix (berbasis text files) sangat berbeda — solusinya adalah pendekatan object-oriented yang menggabungkan scripting sederhana dengan kekuatan .NET

---

## 2. Konsep Object

Untuk memahami kekuatan PowerShell, konsep **object** harus dipahami lebih dulu.

Dalam pemrograman, object adalah representasi sebuah item yang memiliki:
- **Properties** — karakteristik/data dari object tersebut (contoh: `Name`, `Length`, `Status`)
- **Methods** — aksi yang bisa dilakukan terhadap object (contoh: `Drive()`, `Refuel()`)

Di PowerShell, setiap cmdlet yang dijalankan mengembalikan **object**, bukan teks. Artinya output bisa langsung dimanipulasi, difilter, dan diteruskan ke cmdlet berikutnya tanpa perlu parsing teks tambahan.

Perbandingan:

| Aspek | CMD (text-based) | PowerShell (object-based) |
|---|---|---|
| Output | Teks polos | Object dengan properties & methods |
| Manipulasi data | Perlu parsing manual | Langsung akses property |
| Fleksibilitas | Terbatas | Tinggi |

---

## 3. Cara Launch PowerShell

| Metode | Cara |
|---|---|
| Start Menu | Ketik `powershell` → klik Windows PowerShell |
| Run Dialog | `Win+R` → ketik `powershell` → Enter |
| File Explorer | Ketik `powershell` di address bar → Enter |
| Task Manager | File > Run new task → ketik `powershell` → Enter |
| Dari CMD | Ketik `powershell` → Enter |

Setelah PowerShell terbuka, prompt ditandai dengan **PS** di awal baris, diikuti working directory saat ini.

---

## 4. Basic Syntax: Verb-Noun

Semua perintah PowerShell disebut **cmdlets** (diucap: *command-lets*) dan mengikuti konvensi penamaan **Verb-Noun**:

- **Verb** — mendeskripsikan aksi yang dilakukan (`Get`, `Set`, `New`, `Remove`, `Copy`, `Move`, dll)
- **Noun** — menentukan objek yang dikenai aksi (`ChildItem`, `Location`, `Content`, `Service`, dll)

Contoh:
- `Get-Content` — mengambil isi file
- `Set-Location` — mengubah working directory
- `New-Item` — membuat file atau folder baru

Konvensi ini membuat setiap cmdlet langsung bisa ditebak fungsinya tanpa harus hafal satu per satu.

---

## 5. Cmdlets Dasar & Discovery

### 5.1 Get-Command

Menampilkan semua cmdlets, functions, aliases, dan scripts yang tersedia di sesi PowerShell saat ini.

```powershell
Get-Command
Get-Command -CommandType "Function"
Get-Command -Name "Remove*"
```

- `-CommandType "tipe"` — filter berdasarkan tipe (Function, Cmdlet, Alias, dll)
- `-Name "pola*"` — filter berdasarkan nama dengan wildcard `*`

### 5.2 Get-Help

Menampilkan dokumentasi lengkap sebuah cmdlet — cara penggunaan, parameter, dan contoh.

```powershell
Get-Help Get-Date
Get-Help Get-Date -examples
Get-Help Get-Date -detailed
Get-Help Get-Date -full
Get-Help Get-Date -online
```

- `-examples` — tampilkan contoh-contoh penggunaan umum
- `-detailed` — informasi lebih lengkap dari default
- `-full` — informasi teknis selengkap-lengkapnya
- `-online` — buka halaman dokumentasi resmi di browser

### 5.3 Get-Alias

Menampilkan semua alias yang tersedia. Alias adalah nama pendek/alternatif untuk cmdlet, dibuat agar pengguna yang terbiasa dengan CMD atau Unix tidak perlu belajar dari nol.

```powershell
Get-Alias
```

Alias umum yang perlu dihapal:

| Alias | Cmdlet Asli |
|---|---|
| `dir` / `ls` | `Get-ChildItem` |
| `cd` / `chdir` | `Set-Location` |
| `cat` | `Get-Content` |
| `echo` | `Write-Output` |
| `%` | `ForEach-Object` |
| `?` | `Where-Object` |

### 5.4 Find-Module & Install-Module

Digunakan untuk mencari dan mengunduh modules tambahan dari repositori online seperti **PowerShell Gallery (PSGallery)**.

```powershell
Find-Module -Name "PowerShell*"
Install-Module -Name "PowerShellGet"
```

- `Find-Module -Name "pola*"` — cari module dengan nama yang cocok pola tertentu
- `Install-Module -Name "nama"` — download dan install module ke sistem

> Catatan: Kedua cmdlet ini membutuhkan koneksi internet aktif.

---

## 6. Navigasi File System

### 6.1 Melihat Isi Direktori

```powershell
Get-ChildItem
Get-ChildItem -Path ".\folder"
Get-ChildItem -Hidden
Get-ChildItem -Path C:\ -Recurse -Hidden
```

- Tanpa parameter — tampilkan isi working directory saat ini
- `-Path "lokasi"` — tentukan direktori yang ingin dilihat
- `-Hidden` — tampilkan file/folder yang tersembunyi
- `-Recurse` — masuk ke semua subfolder secara rekursif

### 6.2 Berpindah Direktori

```powershell
Set-Location -Path ".\Documents"
```

- `-Path "lokasi"` — path tujuan, bisa relatif (`.\folder`) atau absolut (`C:\Users\nama`)

### 6.3 Membuat Item

```powershell
New-Item -Path ".\captain-cabin" -ItemType "Directory"
New-Item -Path ".\file.txt" -ItemType "File"
```

- `-Path "lokasi"` — path lengkap item yang akan dibuat
- `-ItemType "Directory"` — buat folder
- `-ItemType "File"` — buat file

### 6.4 Menghapus Item

```powershell
Remove-Item -Path ".\file.txt"
Remove-Item -Path ".\folder"
```

Satu cmdlet untuk menghapus file maupun folder (berbeda dengan CMD yang pakai `del` dan `rmdir` secara terpisah).

### 6.5 Menyalin & Memindahkan

```powershell
Copy-Item -Path .\asal.txt -Destination .\salinan.txt
Move-Item -Path .\asal.txt -Destination .\tujuan\asal.txt
```

- `Copy-Item` — salin file/folder, original tetap ada (setara `copy`)
- `Move-Item` — pindah file/folder, original tidak ada lagi (setara `move`)
- `-Path` — sumber
- `-Destination` — tujuan

### 6.6 Membaca Isi File

```powershell
Get-Content -Path ".\file.txt"
```

Menampilkan seluruh isi file ke konsol (setara `type` di CMD atau `cat` di Unix).

---

## 7. Cara Membaca Kolom Mode

Kolom **Mode** pada output `Get-ChildItem` terdiri dari 6 karakter posisi:

```
Posisi 1 : d = directory     | - = file
Posisi 2 : r = read-only     | - = tidak
Posisi 3 : a = archive       | - = tidak
Posisi 4 : h = hidden        | - = tidak
Posisi 5 : s = system file   | - = tidak
Posisi 6 : l = symbolic link | - = tidak
```

Contoh pembacaan:
- `d--h--` — folder yang disembunyikan (hidden)
- `-a-hs-` — file archive, hidden, dan system
- `d--hsl` — folder hidden, system, symbolic link (junction point)

---

## 8. Piping, Filtering, dan Sorting

### 8.1 Piping

**Piping** adalah teknik meneruskan output satu cmdlet sebagai input cmdlet berikutnya, menggunakan simbol `|`.

Di PowerShell, pipe meneruskan **object** (bukan teks), sehingga properties dan methods object tetap terbawa dan bisa langsung digunakan oleh cmdlet berikutnya.

```powershell
Get-ChildItem | Sort-Object Length
```

### 8.2 Sort-Object

Mengurutkan objects berdasarkan property yang ditentukan.

```powershell
Sort-Object Length
Sort-Object Length -Descending
```

- Default: **ascending** (kecil ke besar)
- `-Descending` — balik urutan menjadi besar ke kecil

### 8.3 Where-Object

Memfilter objects berdasarkan kondisi — hanya object yang memenuhi kriteria yang diteruskan.

```powershell
Get-ChildItem | Where-Object -Property "Extension" -eq ".txt"
Get-ChildItem | Where-Object -Property "Name" -like "ship*"
```

- `-Property "nama"` — property yang dijadikan dasar filter
- Diikuti **comparison operator** dan nilai yang dibandingkan

**Comparison operators:**

| Operator | Arti |
|---|---|
| `-eq` | equal to |
| `-ne` | not equal |
| `-gt` | greater than (strict, tidak termasuk nilai sama) |
| `-ge` | greater than or equal to |
| `-lt` | less than (strict) |
| `-le` | less than or equal to |
| `-like` | cocokkan pola dengan wildcard `*` |

### 8.4 Select-Object

Memilih property tertentu yang ditampilkan, atau membatasi jumlah object yang dikembalikan.

```powershell
Get-ChildItem | Select-Object Name,Length
Get-ChildItem | Sort-Object Length -Descending | Select-Object -First 1
```

- `Name,Length` — pilih kolom yang ingin ditampilkan
- `-First 1` — ambil hanya 1 object pertama dari hasil

### 8.5 Select-String

Mencari pola teks di dalam file (setara `grep` di Unix atau `findstr` di CMD).

```powershell
Select-String -Path ".\file.txt" -Pattern "kata"
```

- `-Path "lokasi"` — file yang dicari
- `-Pattern "teks"` — pola yang dicari, mendukung **regex** untuk pencocokan kompleks

Output menampilkan: nama file, nomor baris, dan isi baris yang cocok.

### 8.6 Contoh Pipeline Lengkap

```powershell
# Tampilkan file terbesar di direktori
Get-ChildItem | Sort-Object Length -Descending | Select-Object -First 1

# Tampilkan hanya file .txt dengan nama dan ukurannya
Get-ChildItem | Where-Object -Property "Extension" -eq ".txt" | Select-Object Name,Length
```

---

## 9. System & Network Information

### 9.1 Get-ComputerInfo

Menampilkan informasi sistem yang komprehensif: OS, hardware, BIOS, versi Windows, dan lainnya. Jauh lebih lengkap dari `systeminfo` di CMD.

```powershell
Get-ComputerInfo
```

### 9.2 Get-LocalUser

Menampilkan semua akun user lokal di sistem beserta status dan deskripsinya. Penting untuk memahami security configuration mesin.

```powershell
Get-LocalUser
Get-LocalUser | Select-Object Name,Description
```

Output default: Name, Enabled (True/False), Description.

### 9.3 Get-NetIPConfiguration

Menampilkan konfigurasi jaringan: IP address, DNS server, dan default gateway. Setara `ipconfig` di CMD.

```powershell
Get-NetIPConfiguration
```

### 9.4 Get-NetIPAddress

Menampilkan detail semua IP address yang dikonfigurasi di sistem, termasuk yang sedang tidak aktif (berbeda dari `Get-NetIPConfiguration` yang hanya menampilkan yang aktif).

```powershell
Get-NetIPAddress
```

---

## 10. Real-Time System Analysis

### 10.1 Get-Process

Menampilkan semua proses yang sedang berjalan beserta detail penggunaan CPU, memori, dan PID-nya.

```powershell
Get-Process
```

Output: Handles, NPM(K), PM(K), WS(K), CPU(s), Id, SI, ProcessName.

### 10.2 Get-Service

Menampilkan status semua services di sistem. Berguna untuk troubleshooting oleh sysadmin, maupun untuk forensics analyst yang mencari anomalous service.

```powershell
Get-Service
Get-Service | Where-Object -Property "DisplayName" -eq "nama service"
```

Output: Status (Running/Stopped/Paused), Name, DisplayName.

### 10.3 Get-NetTCPConnection

Menampilkan semua koneksi TCP aktif — sangat berguna saat incident response untuk mendeteksi backdoor atau koneksi mencurigakan ke server attacker.

```powershell
Get-NetTCPConnection
Get-NetTCPConnection | Select-Object LocalAddress,LocalPort,RemoteAddress,RemotePort,State,OwningProcess
```

Output penting:
- **State** — Listen (menunggu koneksi) atau Established (sudah terhubung)
- **OwningProcess** — PID proses yang membuka koneksi ini

Untuk tahu proses apa yang punya PID tertentu, kombinasikan dengan `Get-Process`:

```powershell
Get-Process | Where-Object -Property Id -eq 1444
```

### 10.4 Get-FileHash

Menghasilkan hash kriptografis dari sebuah file. Digunakan untuk memverifikasi integritas file dan mendeteksi tampering (modifikasi tidak sah).

```powershell
Get-FileHash -Path ".\file.txt"
```

- Default algoritma: **SHA256**
- Jika ada satu byte saja yang berubah di file, hash-nya akan berubah total

### 10.5 Get-Item dengan -Stream (Alternate Data Streams)

Menampilkan semua **Alternate Data Streams (ADS)** yang terlampir pada sebuah file NTFS.

```powershell
Get-Item -Path "C:\file.txt" -Stream *
```

- `-Stream *` — tampilkan semua stream, termasuk yang tersembunyi

**Penjelasan ADS:**
- Setiap file NTFS memiliki stream default bernama `:$DATA` yang berisi isi normal file
- ADS adalah stream tambahan yang bisa ditempel ke file tanpa mengubah ukuran file yang terlihat di Explorer
- ADS sering disalahgunakan untuk menyembunyikan malware atau data tersembunyi
- Tidak terlihat di File Explorer maupun `dir` biasa di CMD

---

## 11. Scripting & Remote Execution

### 11.1 Apa Itu Scripting

**Scripting** adalah proses menulis kumpulan perintah PowerShell ke dalam sebuah file teks (ekstensi `.ps1`) untuk dieksekusi secara otomatis dan berurutan.

Manfaat utama:
- Menghemat waktu — tugas berulang cukup ditulis sekali
- Mengurangi error — tidak ada langkah yang terlewat
- Memungkinkan otomasi tugas kompleks di banyak sistem sekaligus

### 11.2 Kegunaan Scripting per Peran

**Blue Team** (incident responder, malware analyst, threat hunter):
- Otomasi analisis log
- Deteksi anomali dan ekstraksi IOC
- Reverse-engineer malware
- Otomasi scanning sistem untuk tanda intrusi

**Red Team** (penetration tester, ethical hacker):
- Otomasi system enumeration
- Eksekusi remote commands
- Membuat obfuscated scripts untuk bypass pertahanan
- Simulasi serangan nyata

**System Administrator:**
- Otomasi integrity checks
- Manajemen konfigurasi sistem
- Enforcement security policies
- Monitoring kesehatan sistem secara real-time

### 11.3 Invoke-Command

Cmdlet untuk mengeksekusi perintah di sistem remote — fundamental bagi sysadmin, security engineer, maupun penetration tester.

```powershell
# Jalankan script lokal di komputer remote
Invoke-Command -FilePath c:\scripts\test.ps1 -ComputerName Server01

# Jalankan perintah langsung di komputer remote dengan kredensial
Invoke-Command -ComputerName Server01 -Credential Domain01\User01 -ScriptBlock { Get-Culture }

# Tanpa kredensial (asumsi sudah punya akses)
Invoke-Command -ComputerName RoyalFortune -ScriptBlock { Get-Service }
```

- `-ComputerName "nama"` — tentukan target komputer remote
- `-Credential "Domain\User"` — gunakan kredensial tertentu untuk autentikasi
- `-FilePath "path\script.ps1"` — jalankan file script yang ada di komputer lokal ke remote
- `-ScriptBlock { perintah }` — jalankan satu perintah atau serangkaian perintah langsung tanpa file script

**Poin penting:** Dengan `-ScriptBlock`, tidak perlu tahu cara membuat script — cukup tulis perintah di dalam `{ }` dan hasilnya sama persis seperti mengetiknya langsung di komputer remote.

---

## 12. Glossary

**Cmdlet** — Perintah bawaan PowerShell dengan format Verb-Noun, diucap "command-let"

**Alias** — Nama pendek atau alternatif untuk sebuah cmdlet (contoh: `dir` untuk `Get-ChildItem`)

**Pipeline** — Rangkaian cmdlets yang dihubungkan dengan pipe `|`, meneruskan object dari satu cmdlet ke berikutnya

**Object** — Unit data di PowerShell yang memiliki properties (data) dan methods (aksi)

**Properties** — Atribut/karakteristik dari sebuah object (contoh: Name, Length, Status)

**Methods** — Fungsi/aksi yang bisa dilakukan terhadap object (contoh: `Drive()`, `Stop()`)

**ADS (Alternate Data Stream)** — Stream tersembunyi yang bisa dilampirkan ke file NTFS tanpa mengubah ukuran file yang terlihat; sering disalahgunakan untuk menyembunyikan data atau malware

**Hash** — Nilai kriptografis unik dari sebuah file; berubah total jika isi file dimodifikasi sedikit pun

**IOC (Indicator of Compromise)** — Bukti atau artefak digital yang mengindikasikan sistem telah dikompromasi

**Wildcard** — Karakter `*` yang digunakan untuk mencocokkan pola nama (contoh: `Remove*` = semua yang diawali "Remove")

**PSGallery (PowerShell Gallery)** — Repositori online resmi untuk menemukan dan mengunduh modules PowerShell tambahan

**ScriptBlock** — Blok satu atau lebih perintah yang ditulis di dalam `{ }`, digunakan terutama dengan `Invoke-Command`

**PID (Process ID)** — Nomor identifikasi unik untuk setiap proses yang berjalan di sistem

**OwningProcess** — Property di `Get-NetTCPConnection` yang berisi PID proses yang membuka koneksi TCP tersebut

**Tampering** — Modifikasi tidak sah terhadap isi sebuah file

**Obfuscation** — Teknik menyamarkan kode script agar tidak mudah dibaca atau dideteksi oleh sistem keamanan

**.NET Framework** — Platform pengembangan perangkat lunak Microsoft yang menjadi fondasi PowerShell

---

## 13. Tools & Platform Rujukan

**PowerShell Gallery (PSGallery)**
- Fungsi: Repositori resmi untuk mencari, mengunduh, dan menginstal modules PowerShell tambahan
- URL: https://www.powershellgallery.com

**Microsoft PowerShell Documentation**
- Fungsi: Dokumentasi resmi lengkap semua cmdlets, parameter, dan contoh penggunaan
- URL: https://learn.microsoft.com/en-us/powershell

**Remmina**
- Fungsi: Remote desktop client di Linux untuk koneksi SSH atau RDP ke mesin target di lab environment
- Tersedia sebagai aplikasi bawaan di AttackBox TryHackMe

---

## 14. Catatan Ringkas untuk Ditulis Tangan

### Definisi Dasar

- PowerShell — automation shell Microsoft, berbasis .NET, object-oriented
- Cmdlet — perintah PS format Verb-Noun, diucap "command-let"
- Object — unit data dengan properties (atribut) + methods (aksi)
- ADS — stream tersembunyi di file NTFS, tidak terlihat di Explorer
- Hash — sidik jari file, berubah jika file dimodifikasi
- IOC — bukti sistem telah dikompromasi
- Pipeline — rangkaian cmdlets dihubungkan `|`, meneruskan object
- PSGallery — repositori online modules PowerShell
- PID — nomor ID unik setiap proses
- OwningProcess — PID proses yang buka koneksi TCP

### Cara Baca Mode File

- Posisi 1: d=folder, -=file
- Posisi 2: r=read-only
- Posisi 3: a=archive
- Posisi 4: h=hidden
- Posisi 5: s=system
- Posisi 6: l=symlink

### Syntax Dasar

- Format — Verb-Noun
- Verb contoh — Get, Set, New, Remove, Copy, Move
- Noun contoh — ChildItem, Location, Content, Service, Process

### Alias Penting

- dir/ls — Get-ChildItem
- cd — Set-Location
- cat — Get-Content
- echo — Write-Output
- % — ForEach-Object
- ? — Where-Object

### Discovery Commands

- Get-Command — list semua cmdlets tersedia
- Get-Command -CommandType "Function" — filter tipe
- Get-Command -Name "Remove*" — filter nama + wildcard
- Get-Help cmdlet — dokumentasi cmdlet
- Get-Help cmdlet -examples — tampilkan contoh
- Get-Alias — list semua alias

### Navigasi File System

- Get-ChildItem — list isi direktori
- Get-ChildItem -Hidden — tampilkan file tersembunyi
- Get-ChildItem -Recurse — masuk subfolder
- Set-Location -Path ".\folder" — pindah direktori
- New-Item -Path ".\nama" -ItemType "Directory" — buat folder
- New-Item -Path ".\nama.txt" -ItemType "File" — buat file
- Remove-Item -Path ".\nama" — hapus file/folder
- Copy-Item -Path .\asal -Destination .\tujuan — salin
- Move-Item -Path .\asal -Destination .\tujuan — pindah
- Get-Content -Path ".\file.txt" — baca isi file

### Piping & Filtering

- Sort-Object Property — urutkan ASC (default)
- Sort-Object Property -Descending — urutkan DESC
- Where-Object -Property "nama" -eq "nilai" — filter
- Select-Object Prop1,Prop2 — pilih kolom
- Select-Object -First 1 — ambil 1 object pertama
- Select-String -Path ".\file" -Pattern "kata" — cari teks
- -like "pola*" — filter dengan wildcard

### Comparison Operators

- -eq — sama dengan
- -ne — tidak sama
- -gt — lebih besar (strict)
- -ge — lebih besar atau sama
- -lt — lebih kecil (strict)
- -le — lebih kecil atau sama

### System Information

- Get-ComputerInfo — info OS + hardware lengkap
- Get-LocalUser — list user lokal + status
- Get-NetIPConfiguration — IP, DNS, gateway aktif
- Get-NetIPAddress — semua IP termasuk tidak aktif

### Real-Time Analysis

- Get-Process — list proses berjalan + CPU/RAM
- Get-Service — status services Running/Stopped/Paused
- Get-NetTCPConnection — koneksi TCP aktif
- Get-NetTCPConnection kolom penting — State, OwningProcess
- Get-FileHash -Path ".\file" — generate SHA256 hash
- Get-Item -Path ".\file" -Stream * — lihat semua ADS

### Scripting & Remote

- Script — file .ps1 berisi kumpulan perintah PS
- Invoke-Command -ComputerName nama — eksekusi di remote
- Invoke-Command ... -Credential Domain\User — pakai kredensial
- Invoke-Command ... -FilePath path\script.ps1 — kirim script ke remote
- Invoke-Command ... -ScriptBlock { Get-Service } — jalankan perintah langsung di remote

### Kegunaan Scripting

- Blue team — log analysis, deteksi anomali, ekstrak IOC
- Red team — enumeration, bypass defence, remote execution
- SysAdmin — integrity check, enforce policy, monitor health

### Pipeline Contoh Penting

- Cari file terbesar:
  Get-ChildItem | Sort-Object Length -Descending | Select-Object -First 1
- Filter file .txt:
  Get-ChildItem | Where-Object -Property "Extension" -eq ".txt"
- Cari service berdasarkan DisplayName:
  Get-Service | Where-Object -Property "DisplayName" -eq "nama"
