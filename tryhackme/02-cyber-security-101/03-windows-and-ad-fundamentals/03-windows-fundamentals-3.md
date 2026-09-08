# Windows Fundamentals 3 — Resume Materi
**TryHackMe | Windows Fundamentals 3**
Tanggal: 08 September 2026

---

## Daftar Isi

1. Windows Update
2. Windows Security
3. Virus & Threat Protection
4. Firewall & Network Protection
5. App & Browser Control
6. Device Security
7. BitLocker
8. Volume Shadow Copy Service (VSS)
9. Glossary
10. Tools & Platform Rujukan
11. Catatan Ringkas untuk Ditulis Tangan

---

## 1. Windows Update

**Windows Update** adalah layanan Microsoft untuk mendistribusikan security updates, feature enhancements, dan patches untuk Windows OS serta produk Microsoft lainnya seperti Microsoft Defender.

### 1.1 Jadwal Rilis

Update rutin dirilis setiap **Selasa kedua tiap bulan**, dikenal sebagai **Patch Tuesday**. Jika ada update bersifat kritis dan mendesak, Microsoft dapat mendorong update tersebut di luar jadwal Patch Tuesday kapan saja.

### 1.2 Cara Mengakses

- Melalui Settings: **Settings → Update & Security → Windows Update**
- Melalui Run dialog atau CMD:

```
control /name Microsoft.WindowsUpdate
```

### 1.3 Perilaku di Windows 10

Di Windows 10, update **tidak bisa diabaikan sepenuhnya**. Update hanya bisa ditunda sementara, tapi pada akhirnya sistem akan tetap mengunduh dan menginstal update, disertai restart otomatis. Ini berbeda dari versi Windows sebelumnya yang membiarkan user mengabaikan update tanpa batas.

### 1.4 Opsi yang Tersedia

- **Pause updates for 7 days** — tunda update selama 7 hari (bisa diubah di Advanced options)
- **Change active hours** — atur jam aktif agar restart tidak terjadi saat jam kerja (default: 7:00 AM – 12:00 AM)
- **View update history** — lihat riwayat update yang sudah terinstal
- **Advanced options** — pengaturan tambahan seperti periode pause lebih panjang
- **Check for updates** — cek update secara manual

### 1.5 Status "Managed"

Jika muncul pesan *"Some settings are managed by your organization"*, artinya pengaturan Windows Update dikontrol oleh administrator jaringan/domain, bukan oleh pengguna. Umumnya hanya terlihat di lingkungan perusahaan, bukan pada perangkat rumahan biasa.

---

## 2. Windows Security

**Windows Security** adalah dashboard terpusat untuk memantau dan mengelola semua fitur keamanan bawaan Windows.

Menurut Microsoft: *"Windows Security is your home to manage the tools that protect your device and your data."*

### 2.1 Cara Mengakses

- Melalui Settings: **Settings → Update & Security → Windows Security → Open Windows Security**

### 2.2 Empat Area Perlindungan Utama

| Area | Fungsi |
|---|---|
| Virus & threat protection | Deteksi dan penanganan malware |
| Firewall & network protection | Kontrol lalu lintas jaringan |
| App & browser control | Perlindungan dari app dan situs berbahaya |
| Device security | Keamanan berbasis hardware |

Di Windows 10 Home/Professional, tersedia tambahan area:
- **Account protection** — keamanan akun pengguna
- **Device performance & health** — laporan kesehatan perangkat
- **Family options** — manajemen penggunaan perangkat untuk keluarga

> Catatan: Windows Server 2019 menampilkan lebih sedikit area dibandingkan Windows 10 karena perbedaan kebutuhan penggunaan.

### 2.3 Arti Ikon Status

- **Hijau** — perangkat terlindungi, tidak ada tindakan yang perlu dilakukan
- **Kuning** — ada rekomendasi keamanan yang perlu ditinjau
- **Merah** — peringatan kritis, membutuhkan tindakan segera

---

## 3. Virus & Threat Protection

Halaman ini dibagi menjadi dua bagian utama: **Current Threats** dan **Virus & Threat Protection Settings**.

### 3.1 Current Threats

Menampilkan status ancaman terkini, hasil scan terakhir, dan opsi untuk melakukan scan manual.

**Scan Options:**

| Tipe Scan | Cara Kerja |
|---|---|
| Quick scan | Hanya periksa folder-folder rawan yang biasa jadi target malware — cepat selesai |
| Full scan | Periksa semua file dan program yang berjalan di hard disk — bisa lebih dari 1 jam |
| Custom scan | Pengguna memilih sendiri file atau folder yang ingin diperiksa |

**Threat History:**

- **Last scan** — rekap scan otomatis terakhir yang dilakukan Windows Defender Antivirus
- **Quarantined threats** — ancaman yang sudah diisolasi dan dicegah dari berjalan; akan dihapus secara berkala
- **Allowed threats** — item yang terdeteksi sebagai ancaman namun secara eksplisit diizinkan oleh pengguna untuk tetap berjalan

> **Peringatan:** Hanya izinkan item yang teridentifikasi sebagai ancaman jika kamu **100% yakin** item tersebut aman. Kesalahan di sini bisa membahayakan seluruh sistem.

### 3.2 Virus & Threat Protection Settings

**Manage Settings** mencakup pengaturan-pengaturan berikut:

- **Real-time protection** — mendeteksi dan menghentikan malware secara aktif sebelum berhasil terinstal atau berjalan. Ini adalah lapisan perlindungan utama; jangan dimatikan tanpa alasan yang sangat kuat.
- **Cloud-delivered protection** — mengakses database ancaman terbaru di cloud Microsoft untuk respons perlindungan yang lebih cepat terhadap ancaman baru.
- **Automatic sample submission** — mengirim sampel file mencurigakan ke Microsoft secara otomatis untuk membantu memperbaiki database ancaman.
- **Controlled folder access** — melindungi folder tertentu dari modifikasi oleh aplikasi yang tidak dikenal atau tidak dipercaya. Hanya aplikasi yang disetujui yang bisa mengubah isi folder yang dilindungi. Aktifkan melalui tombol *Manage controlled folder access* di bawah menu Controlled Folder Access.
- **Exclusions** — mengecualikan file atau folder tertentu dari proses scan antivirus, biasanya untuk mengurangi false positive. Tambah atau hapus exclusion melalui tombol *Add or remove exclusions* di bawah menu Exclusions.

> **Peringatan:** Item yang dikecualikan tetap bisa mengandung ancaman. Gunakan fitur ini hanya jika **100% yakin** file tersebut aman.

- **Notifications** — Windows Defender akan mengirimkan notifikasi berisi informasi penting tentang kesehatan dan keamanan perangkat.

**Virus & Threat Protection Updates:**

- **Check for updates** — memperbarui definisi antivirus Windows Defender secara manual. Definisi adalah database pengenal ancaman; semakin baru definisinya, semakin banyak ancaman yang bisa dikenali.

**Ransomware Protection:**

Fitur ini bergantung pada **Controlled folder access**, yang secara otomatis membutuhkan **Real-time protection** aktif terlebih dahulu. Jika real-time protection mati, ransomware protection juga tidak bisa diaktifkan.

**Tips praktis:**

Klik kanan pada file atau folder mana saja di File Explorer, lalu pilih *"Scan with Microsoft Defender..."* untuk melakukan scan on-demand tanpa harus membuka antarmuka Windows Security.

---

## 4. Firewall & Network Protection

**Firewall** adalah sistem yang mengontrol lalu lintas data yang masuk dan keluar melalui port jaringan.

Menurut Microsoft: *"Traffic flows into and out of devices via what we call ports. A firewall is what controls what is — and more importantly isn't — allowed to pass through those ports."*

### 4.1 Tiga Profil Firewall

Windows Firewall beroperasi dalam tiga profil yang berbeda tergantung jenis jaringan yang terhubung:

- **Domain** — digunakan saat sistem terhubung ke jaringan yang bisa melakukan autentikasi ke domain controller (umumnya jaringan perusahaan enterprise)
- **Private** — profil yang ditetapkan pengguna sendiri untuk jaringan yang dipercaya, seperti jaringan rumah atau kantor kecil
- **Public** — profil default untuk jaringan publik seperti hotspot kafe, bandara, atau tempat umum lainnya; pengaturannya paling ketat

### 4.2 Opsi Per Profil

Ketika mengklik salah satu profil, tersedia dua opsi utama:
- **Turn the firewall on/off** — aktifkan atau nonaktifkan firewall untuk profil tersebut
- **Block all incoming connections** — blok semua koneksi masuk, termasuk yang sudah ada di daftar aplikasi yang diizinkan

> **Peringatan:** Kecuali kamu **100% yakin** dengan apa yang dilakukan, sangat direkomendasikan untuk membiarkan Windows Defender Firewall tetap aktif.

### 4.3 Allow an App Through Firewall

Halaman ini menampilkan daftar aplikasi yang diizinkan melewati firewall, dengan kontrol terpisah untuk profil **Private** dan **Public**. Setiap aplikasi bisa diizinkan untuk satu atau kedua profil sekaligus. Klik tombol **Details** untuk melihat informasi tambahan tentang suatu aplikasi.

### 4.4 Advanced Settings

Diakses melalui command:

```
WF.msc
```

Menampilkan kontrol firewall tingkat lanjut, termasuk:

- **Inbound Rules** — aturan untuk koneksi yang masuk ke perangkat. Default: koneksi masuk yang tidak cocok dengan rule mana pun akan **diblok**.
- **Outbound Rules** — aturan untuk koneksi yang keluar dari perangkat. Default: koneksi keluar yang tidak cocok dengan rule mana pun akan **diizinkan**.
- **Connection Security Rules** — aturan autentikasi antar komputer menggunakan IPsec (Internet Protocol Security)
- **Monitoring** — pantau status firewall dan rule yang aktif secara real-time

Panel **Actions** di Advanced Settings menyediakan:
- Import Policy / Export Policy
- Restore Default Policy
- Diagnose / Repair
- View, Refresh, Properties, Help

> Mengonfigurasi Windows Defender Firewall melalui Advanced Settings diperuntukkan bagi pengguna Windows tingkat lanjut.

---

## 5. App & Browser Control

Bagian ini mengelola pengaturan **Microsoft Defender SmartScreen** — sistem perlindungan terhadap phishing, situs berbahaya, aplikasi tidak dikenal, dan file yang berpotensi berbahaya dari internet.

Menurut Microsoft: *"Microsoft Defender SmartScreen protects against phishing or malware websites and applications, and the downloading of potentially malicious files."*

### 5.1 Check Apps and Files

SmartScreen memeriksa aplikasi dan file yang diunduh dari internet dengan tiga mode operasi:

- **Block** — langsung menolak aplikasi atau file yang tidak dikenal, tanpa memberi pilihan kepada pengguna
- **Warn** — menampilkan peringatan dan meminta konfirmasi pengguna sebelum melanjutkan (mode default yang direkomendasikan)
- **Off** — menonaktifkan SmartScreen sepenuhnya; tidak ada pemeriksaan dilakukan

Saat SmartScreen aktif dan mendeteksi aplikasi tidak dikenal, akan muncul popup *"Windows protected your PC"* dengan pesan bahwa aplikasi tersebut dicegah dari berjalan karena bisa membahayakan PC. Pengguna diberikan tombol **Don't run** dan opsi **More info** untuk melihat detail dan memaksa menjalankan jika yakin.

### 5.2 Exploit Protection

**Exploit protection** adalah sistem perlindungan bawaan Windows 10 (dan Windows Server 2019) yang dirancang melindungi perangkat dari serangan eksploitasi. Sudah aktif secara default dengan pengaturan yang optimal untuk sebagian besar pengguna.

Diakses melalui: *App & browser control → Exploit protection settings*

**System Settings** dalam Exploit protection:

- **Control flow guard (CFG)** — memastikan integritas alur kontrol untuk indirect calls, mencegah eksekusi kode di lokasi yang tidak terduga. Default: On.
- **Data Execution Prevention (DEP)** — mencegah kode dieksekusi dari halaman memori yang hanya berisi data, bukan kode program. Default: On.
- **Force randomization for images (Mandatory ASLR)** — memaksa relokasi image yang tidak dikompilasi dengan flag `/DYNAMICBASE`. Default: Off.
- **Randomize memory allocations (Bottom-up ASLR)** — mengacak lokasi alokasi virtual memory agar posisi modul tidak bisa diprediksi. Default: On (di kebanyakan sistem).

Tab **Program settings** memungkinkan pengaturan exploit protection per aplikasi secara individual.

> **Peringatan:** Kecuali **100% yakin**, disarankan untuk membiarkan semua pengaturan di nilai default-nya.

---

## 6. Device Security

**Device security** mencakup fitur-fitur keamanan yang sudah tertanam langsung di dalam hardware perangkat, bukan software yang ditambahkan belakangan.

### 6.1 Core Isolation

**Core isolation** menggunakan virtualization-based security untuk melindungi proses inti sistem operasi dari serangan. Fitur ini mengisolasi proses-proses kritis ke dalam lingkungan tervirtualisasi yang terlindungi.

Sub-fitur utamanya:

- **Memory integrity** — mencegah serangan yang mencoba menyisipkan kode berbahaya ke dalam proses dengan keamanan tinggi. Bisa diaktifkan atau dinonaktifkan melalui toggle. Pada VM yang dilampirkan, fitur ini dalam kondisi Off.

### 6.2 Security Processor (TPM)

**Trusted Platform Module (TPM)** adalah chip crypto-processor yang tertanam secara fisik di motherboard. TPM dirancang untuk menjalankan operasi kriptografi secara terisolasi dan tahan terhadap manipulasi (tamper-resistant) — bahkan malware yang sudah masuk ke sistem operasi tidak bisa mengubah fungsi keamanan TPM.

Menurut Microsoft: *"A TPM chip is a secure crypto-processor that is designed to carry out cryptographic operations. The chip includes multiple physical security mechanisms to make it tamper-resistant, and malicious software is unable to tamper with the security functions of the TPM."*

Spesifikasi TPM yang terlihat di Security processor details:

| Field | Keterangan |
|---|---|
| Manufacturer | Produsen chip TPM (contoh: Intel/INTC) |
| Specification version | Versi spesifikasi TPM (contoh: 2.0) |
| Status Attestation | Konfirmasi integritas hardware (Ready = normal) |
| Status Storage | Kapasitas penyimpanan kunci kriptografi (Ready = normal) |

> Catatan: Pada VM yang dilampirkan, tercatat *"Standard hardware security not supported"* karena VM tidak memiliki hardware TPM fisik.

---

## 7. BitLocker

**BitLocker Drive Encryption** adalah fitur enkripsi seluruh drive yang melindungi data dari pencurian atau paparan akibat komputer yang hilang, dicuri, atau dinonaktifkan secara tidak tepat.

Menurut Microsoft: *"BitLocker Drive Encryption is a data protection feature that integrates with the operating system and addresses the threats of data theft or exposure from lost, stolen, or inappropriately decommissioned computers."*

### 7.1 Cara Kerja

BitLocker mengenkripsi seluruh isi drive. Data yang terenkripsi tidak dapat dibaca tanpa kunci yang benar — bahkan jika drive dicabut dan dipasang ke komputer lain, isinya tetap tidak terbaca.

### 7.2 Integrasi dengan TPM

BitLocker memberikan perlindungan terbaik saat digunakan bersama **TPM versi 1.2 atau lebih baru**. TPM bekerja bersama BitLocker untuk:
- Melindungi data pengguna
- Memastikan komputer tidak dimodifikasi saat sistem sedang offline

### 7.3 Syarat Sistem

| Kondisi | Persyaratan |
|---|---|
| Perangkat dengan TPM 1.2+ | BitLocker bisa langsung diaktifkan dengan perlindungan penuh |
| Perangkat tanpa TPM | Wajib menyimpan **startup key** di removable drive (flashdisk) |

Jika perangkat tidak memiliki TPM, flashdisk berisi startup key harus selalu ditancapkan setiap kali komputer dinyalakan agar BitLocker bisa membuka enkripsi dan sistem bisa booting.

> Catatan: Fitur BitLocker **tidak tersedia** di VM yang dilampirkan dalam modul ini.

---

## 8. Volume Shadow Copy Service (VSS)

**Volume Shadow Copy Service (VSS)** adalah layanan Windows yang mengkoordinasikan pembuatan shadow copy — yaitu snapshot atau point-in-time copy dari data yang akan di-backup.

### 8.1 Penyimpanan

Shadow copies disimpan di folder **System Volume Information** pada setiap drive yang sudah diaktifkan proteksinya.

### 8.2 Yang Bisa Dilakukan Jika VSS Aktif

Jika VSS aktif (System Protection dinyalakan), pengguna bisa melakukan hal berikut melalui **Advanced System Settings**:

- Create a restore point — buat titik pemulihan baru secara manual
- Perform system restore — kembalikan sistem ke kondisi pada titik pemulihan tertentu
- Configure restore settings — atur pengaturan proteksi per drive
- Delete restore points — hapus titik pemulihan yang sudah tidak diperlukan

### 8.3 Cara Mengakses Configure Shadow Copies

1. Buka **This PC** di File Explorer
2. Klik kanan pada drive yang ingin dikonfigurasi (misal: Local Disk C:)
3. Pilih **Configure Shadow Copies...**
4. Pilih volume, klik **Enable** untuk mengaktifkan
5. Klik **Create Now** untuk membuat shadow copy secara manual

Di jendela Shadow Copies, tersedia kolom informasi: **Volume**, **Next Run Time**, **Shares**, dan **Used**.

### 8.4 Risiko Keamanan

Dari sudut pandang keamanan, ini adalah poin kritis yang perlu dipahami:

**Ransomware modern secara aktif menghapus shadow copies sebagai bagian dari serangan.** Para pembuat malware sudah mengetahui fitur ini dan menyertakan kode dalam malware mereka untuk mencari dan menghapus semua shadow copy sebelum mengenkripsi file. Akibatnya, pemulihan dari serangan ransomware menjadi tidak mungkin jika hanya mengandalkan shadow copies.

Solusi: selalu miliki **backup offline atau off-site** yang tidak terhubung langsung ke komputer yang terinfeksi.

---

## 9. Glossary

**Patch Tuesday** — jadwal rilis update rutin Microsoft, setiap Selasa kedua tiap bulan

**Shadow Copy** — snapshot kondisi data sistem pada titik waktu tertentu, dibuat oleh VSS

**VSS (Volume Shadow Copy Service)** — layanan Windows yang mengelola pembuatan dan penyimpanan shadow copy

**TPM (Trusted Platform Module)** — chip crypto-processor yang tertanam di hardware untuk menjalankan operasi kriptografi secara aman dan tahan manipulasi

**BitLocker** — fitur enkripsi seluruh drive bawaan Windows untuk melindungi data dari akses tidak sah

**SmartScreen** — sistem filter Microsoft Defender yang memeriksa reputasi aplikasi, file, dan situs web dari internet

**Real-time protection** — perlindungan aktif terus-menerus yang mendeteksi dan memblok malware saat mencoba masuk atau berjalan

**Quarantine** — proses isolasi ancaman yang terdeteksi agar tidak bisa dieksekusi, sambil menunggu penanganan lebih lanjut

**Exclusion** — pengecualian file atau folder tertentu dari proses scan antivirus untuk mengurangi false positive

**DEP (Data Execution Prevention)** — mekanisme yang mencegah kode dieksekusi dari area memori yang hanya berisi data

**CFG (Control Flow Guard)** — mekanisme yang memastikan alur eksekusi program mengikuti jalur yang sudah didefinisikan, mencegah manipulasi indirect calls

**ASLR (Address Space Layout Randomization)** — teknik yang mengacak lokasi modul di memori agar posisinya tidak bisa diprediksi oleh penyerang

**Core Isolation** — fitur keamanan berbasis virtualisasi yang mengisolasi proses inti Windows dari ancaman

**Memory Integrity** — sub-fitur Core Isolation yang mencegah injeksi kode berbahaya ke proses dengan keamanan tinggi

**Startup Key** — kunci enkripsi BitLocker yang disimpan di removable drive, digunakan pada perangkat tanpa TPM

**Managed Settings** — pengaturan yang dikontrol oleh administrator organisasi, bukan pengguna individual

**Patch** — pembaruan kecil yang menambal kerentanan atau bug spesifik dalam software

**Living Off the Land (LotL)** — taktik penyerang yang menggunakan tools dan utilitas bawaan Windows agar aktivitas jahat tidak terdeteksi oleh sistem keamanan

**Point-in-time copy** — salinan data yang merepresentasikan kondisi data pada momen tertentu (sinonim: snapshot, shadow copy)

**Restore point** — titik pemulihan sistem yang tersimpan, bisa digunakan untuk mengembalikan kondisi Windows ke keadaan sebelumnya

---

## 10. Tools & Platform Rujukan

**Microsoft Security Update Guide**
Panduan resmi Microsoft untuk melihat semua security update yang dirilis, termasuk detail CVE dan tingkat keparahannya.
URL: https://msrc.microsoft.com/update-guide

**Windows Updates FAQ**
Halaman FAQ resmi Microsoft tentang cara kerja Windows Update, termasuk cara mengelola update di berbagai skenario.
URL: https://support.microsoft.com/en-us/windows/windows-update-faq

**Microsoft Dokumentasi BitLocker**
Dokumentasi resmi BitLocker dari Microsoft, mencakup persyaratan sistem, cara kerja, dan praktik terbaik deployment.
URL: https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-overview

**Microsoft Dokumentasi Windows Defender Firewall Best Practices**
Panduan resmi Microsoft untuk mengonfigurasi Windows Defender Firewall dengan Advanced Security secara aman dan optimal.
URL: https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-firewall/best-practices-configuring

**Microsoft Dokumentasi Microsoft Defender SmartScreen**
Penjelasan resmi tentang cara kerja SmartScreen, termasuk cara mengelola kebijakan SmartScreen di lingkungan enterprise.

**Antimalware Scan Interface (AMSI)**
Antarmuka yang memungkinkan aplikasi dan layanan berintegrasi dengan produk antimalware yang terpasang di Windows.

**Credential Guard**
Fitur Windows yang menggunakan virtualization-based security untuk melindungi kredensial dari pencurian.

**Windows Hello**
Sistem autentikasi biometrik dan PIN bawaan Windows sebagai alternatif password tradisional.

**TryHackMe — Advent of Cyber 2, Day 23**
Latihan hands-on untuk berinteraksi langsung dengan Volume Shadow Copy Service (VSS) dalam skenario praktis.
Platform: https://tryhackme.com

---

## 11. Catatan Ringkas untuk Ditulis Tangan

### Windows Update

- Patch Tuesday — Selasa ke-2 tiap bulan, rilis update rutin
- Update kritis/mendesak — bisa dirilis di luar jadwal kapan saja
- Win 10 — update tidak bisa diabaikan selamanya, hanya ditunda
- Akses — Settings > Update & Security > Windows Update
- CMD — `control /name Microsoft.WindowsUpdate`
- "Managed" — pengaturan dikontrol organisasi, bukan user

### Windows Security

- 4 area utama — Virus & threat, Firewall, App & browser, Device security
- Win 10 tambahan — Account protection, Device health, Family options
- Hijau = aman, Kuning = perlu ditinjau, Merah = tindakan segera
- Server 2019 vs Win 10 — tampilan berbeda, area lebih sedikit

### Virus & Threat Protection

- Quick scan — cek folder rawan, cepat
- Full scan — semua file & program, >1 jam
- Custom scan — pilih sendiri folder yang dicek
- Quarantined — diisolasi, tidak bisa jalan, dihapus berkala
- Allowed threats — hanya izinkan jika 100% yakin!
- Real-time protection — deteksi & hentikan malware aktif
- Cloud-delivered protection — proteksi via database cloud Microsoft
- Controlled folder access — lindungi folder dari app tidak dikenal
- Exclusions — kecualikan dari scan, berbahaya jika salah pakai!
- Ransomware protection — butuh Controlled folder access + Real-time ON
- Tip — klik kanan file > "Scan with Microsoft Defender"

### Firewall & Network Protection

- Domain — jaringan dengan domain controller (enterprise)
- Private — jaringan dipercaya pengguna (rumah/kantor kecil)
- Public — jaringan publik (kafe, bandara), profil default, paling ketat
- Per profil — bisa on/off firewall, block all incoming connections
- Inbound rules default — tidak cocok rule = BLOK
- Outbound rules default — tidak cocok rule = IZIN
- WF.msc — buka Windows Defender Firewall Advanced Settings

### App & Browser Control

- SmartScreen — cegah phishing, malware, file berbahaya dari internet
- Block — tolak langsung tanpa pilihan
- Warn — beri peringatan, user putuskan (default)
- Off — SmartScreen mati total
- CFG — jaga alur kontrol eksekusi program, default On
- DEP — cegah eksekusi kode dari memori data, default On
- Mandatory ASLR — paksa relokasi image, default Off
- Bottom-up ASLR — acak lokasi alokasi memori
- Jangan ubah exploit protection kecuali 100% yakin!

### Device Security

- Core isolation — virtualization-based security lindungi proses inti
- Memory integrity — cegah injeksi kode ke proses keamanan tinggi
- TPM — chip crypto-processor tertanam di hardware
- TPM — tamper-resistant, malware tidak bisa ubah fungsinya
- Attestation Ready & Storage Ready — status TPM normal

### BitLocker

- Enkripsi seluruh drive, data tidak terbaca tanpa kunci
- Dengan TPM 1.2+ — perlindungan terbaik
- Tanpa TPM — wajib startup key di removable drive (flashdisk)
- Flashdisk startup key — harus ditancapkan setiap kali booting
- BitLocker tidak ada di VM yang dilampirkan

### Volume Shadow Copy Service (VSS)

- VSS — buat snapshot/point-in-time copy data untuk backup
- Disimpan di — System Volume Information tiap drive
- Jika aktif bisa — create, perform, configure, delete restore points
- Akses — klik kanan drive di This PC > Configure Shadow Copies
- BAHAYA — ransomware modern menghapus shadow copies dulu sebelum enkripsi
- Solusi — selalu backup offline/off-site yang terpisah dari PC

### Istilah Kunci

- Patch Tuesday — update rutin Selasa ke-2 tiap bulan
- TPM — chip keamanan hardware, tamper-resistant
- BitLocker — enkripsi drive bawaan Windows
- VSS — layanan shadow copy/snapshot
- SmartScreen — filter app & situs berbahaya
- Real-time protection — perlindungan antivirus aktif 24 jam
- Quarantine — isolasi ancaman agar tidak bisa jalan
- Exclusion — pengecualian file dari scan antivirus
- DEP — cegah eksekusi kode dari memori data
- CFG — jaga integritas alur kontrol program
- ASLR — acak lokasi modul di memori
- Core isolation — isolasi proses inti via virtualisasi
- Memory integrity — cegah injeksi kode ke proses inti
- Startup key — kunci BitLocker di flashdisk (pengganti TPM)
- LotL (Living Off the Land) — penyerang pakai tools bawaan Windows agar tidak terdeteksi
- Restore point — titik simpan kondisi sistem untuk pemulihan
- Shadow copy — snapshot data di titik waktu tertentu
