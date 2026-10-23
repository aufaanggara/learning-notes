# Resume Room: REMnux - Getting Started

**Tanggal penyelesaian:** 31 Agustus 2026
**Platform:** TryHackMe

---

## 1. Konsep Inti Room

### 1.1 Tujuan Room

Room ini memperkenalkan **REMnux VM**, sebuah distro Linux khusus untuk menganalisis software, dokumen, atau file yang berpotensi berbahaya (malware). REMnux menyediakan tools yang sudah terinstal lengkap sehingga analis tidak perlu setup manual.

Ruang lingkup yang dicakup:

- Analisis statis dokumen berbahaya (macro-based malware) dengan **oledump.py**
- Simulasi jaringan palsu untuk observasi perilaku jaringan malware dengan **INetSim**
- Preprocessing evidence memory forensics dengan **Volatility 3** dan **strings**

### 1.2 REMnux VM

REMnux adalah distro Linux yang berfokus pada analisis program, dokumen, atau file berpotensi berbahaya, memory image, dan objek serupa lainnya. Tools yang sudah tersedia di dalamnya antara lain: Volatility, YARA, Wireshark, oledump, dan INetSim.

Fungsi utamanya adalah menyediakan environment mirip **sandbox** — tempat file mencurigakan bisa dibedah tanpa membahayakan sistem utama analis.

### 1.3 Struktur Lab

Room ini memakai dua mesin virtual dengan peran berbeda:

- **Lab Machine (REMnux VM)** — "markas analisis", tempat semua tool investigasi dijalankan. Di Task 4, mesin ini juga berperan sebagai simulasi server/internet palsu (menjalankan INetSim).
- **AttackBox** — mesin serbaguna standar TryHackMe untuk melakukan aksi/interaksi dari sisi pengguna. Di Task 4, mesin ini dipakai untuk mensimulasikan perilaku malware yang mencoba menghubungi server luar (bukan berarti nama box menentukan peran tetap; peran ditentukan skenario simulasi yang sedang dijalankan).

Hampir semua file kerja room ini berada di direktori `Desktop/tasks`.

---

## 2. Analisis Statis Dokumen Berbahaya (Task 3)

### 2.1 Konsep OLE2 dan oledump.py

**OLE2** (Object Linking and Embedding, Compound File Binary Format / Structured Storage) adalah format proprietary Microsoft untuk menyimpan berbagai jenis data (dokumen, spreadsheet, presentasi) dalam satu file. Format ini dipakai file-file Office lama maupun sebagian file modern (docx, xlsm) yang menyimpan komponen internalnya dalam struktur OLE.

**oledump.py** adalah tool Python untuk menganalisis file OLE2, berguna untuk ekstraksi dan pemeriksaan isi file demi keperluan forensik dan deteksi malware.

### 2.2 Data Stream dan Indikator Macro

Saat oledump memindai file, setiap komponen internal disebut **data stream**, diberi indeks berupa huruf (misal `A`) diikuti nomor urut (`A1`, `A2`, dst).

Data stream yang ditandai huruf kapital **M** di depan nomornya menandakan **macro (VBA script)** ada di dalam stream tersebut — ini titik awal investigasi paling penting saat menganalisis dokumen Office yang dicurigai.

### 2.3 Command oledump.py

```
oledump.py <namafile>
```
Menampilkan daftar seluruh data stream dalam file beserta indeks dan ukurannya.

```
oledump.py <namafile> -s <nomor>
```
Parameter `-s` (singkatan dari `-select`) memilih dan menampilkan isi satu data stream spesifik berdasarkan nomor indeksnya. Tanpa parameter tambahan, hasil ditampilkan dalam format hex dump.

```
oledump.py <namafile> -s <nomor> --vbadecompress
```
Parameter `--vbadecompress` otomatis mendekompresi macro VBA yang terkompresi menjadi bentuk kode yang bisa dibaca langsung (bukan hex dump), sehingga isi script lebih mudah dianalisis.

### 2.4 Pola Umum Malware Dokumen (Macro-Downloader)

Studi kasus di room ini: file `agenttesla.xlsm` berisi macro pada event `Workbook_Open()` yang otomatis berjalan saat dokumen dibuka (jika macro di-enable). Alur serangannya:

1. Macro menyimpan command PowerShell dalam bentuk **ter-obfuscate** (disisipi karakter sampah seperti `*` dan `^`).
2. Command tersebut di-`Replace()` untuk membuang karakter sampah, menghasilkan command PowerShell asli.
3. Command PowerShell yang sudah bersih menjalankan `Invoke-WebRequest` untuk mendownload file dari server attacker.
4. File hasil download dieksekusi dengan `Start-Process`.

Teknik ini disebut **staged payload / staged malware**: payload berbahaya (file exe) tidak ditempel langsung di dokumen, melainkan didownload belakangan setelah macro aktif. Tujuannya agar file dokumen awal tetap terlihat "bersih" saat pemeriksaan awal (misal email gateway scanning), karena payload sebenarnya baru muncul setelah proses eksekusi berjalan.

### 2.5 Flag PowerShell yang Sering Dipakai untuk Menghindari Deteksi

- `-WindowStyle hidden` — menyembunyikan jendela PowerShell dari pengguna saat script berjalan.
- `-executionpolicy bypass` — mengesampingkan proteksi default Windows yang membatasi eksekusi script, sehingga script berjalan tanpa blokir/warning.
- `Invoke-WebRequest -Uri <url> -OutFile <path>` — mendownload resource dari URL tertentu dan menyimpannya ke path lokal.
- `Start-Process <path>` — menjalankan/mengeksekusi file yang tersimpan di path tersebut.

Teknik tambahan yang umum dipakai malware serupa: penamaan file yang menyamar (misal `Doc-3737122pdf.exe`, terlihat seperti PDF padahal ekstensi sebenarnya `.exe`), obfuscation string berlapis, junk code, embedded/encoded payload, dan anti-analysis (sandbox detection, delay execution).

### 2.6 Deobfuscation dengan CyberChef

Untuk membongkar string yang di-obfuscate, string tersebut disalin ke CyberChef, lalu diberi recipe **Find/Replace** sesuai pola pembersihan yang terlihat di script asli (misal ganti karakter `*` dan `^` dengan string kosong), menghasilkan command PowerShell yang bersih dan terbaca.

---

## 3. Simulasi Jaringan Palsu dengan INetSim (Task 4)

### 3.1 Konsep Dynamic Analysis dan INetSim

**Dynamic analysis** adalah pendekatan mengamati perilaku software mencurigakan secara langsung (termasuk aktivitas jaringannya), berbeda dengan analisis statis yang hanya membaca kode tanpa menjalankannya.

**INetSim (Internet Services Simulation Suite)** adalah tool bawaan REMnux yang mensimulasikan berbagai layanan internet (HTTP, HTTPS, DNS, FTP, SMTP, POP3, dll) sekaligus, tanpa koneksi internet asli. Tujuannya agar malware yang diuji "mengira" ia terhubung ke internet nyata, padahal semua request dijawab oleh server palsu dalam environment tertutup — aman dari risiko eksfiltrasi data asli atau penyebaran lebih lanjut.

### 3.2 Konfigurasi INetSim

Sebelum dijalankan, file konfigurasi `/etc/inetsim/inetsim.conf` perlu disesuaikan agar layanan simulasi menjawab menggunakan IP mesin REMnux yang sebenarnya (bukan default `0.0.0.0`).

Langkah konfigurasi:

1. Cek IP mesin dengan `ifconfig` atau lihat langsung dari prompt terminal (`ubuntu@<ip>`).
2. Edit file konfigurasi:
```
sudo nano /etc/inetsim/inetsim.conf
```
3. Cari baris `#dns_default_ip 0.0.0.0`, hapus tanda pagar (`#`), lalu ganti `0.0.0.0` dengan IP mesin REMnux. Simpan dengan `CTRL+O`, Enter, keluar dengan `CTRL+X`.
4. Verifikasi perubahan:
```
cat /etc/inetsim/inetsim.conf | grep dns_default_ip
```
5. Jalankan INetSim:
```
sudo inetsim
```

Setelah berjalan, terminal akan menampilkan daftar service yang berhasil di-fork (`dns_53_tcp_udp`, `https_443_tcp`, `ftps_990_tcp`, `pop3_110_tcp`, `smtp_25_tcp`, `ftp_21_tcp`, `pop3s_995_tcp`, `smtps_465_tcp`) dan pesan akhir **"Simulation running."** Service `http_80_tcp` yang gagal (`failed!`) bisa diabaikan.

### 3.3 Simulasi Perilaku Malware

Setelah INetSim aktif di REMnux, dari AttackBox bisa dilakukan akses ke IP REMnux via browser (`https://<ip_remnux>`) — akan muncul peringatan sertifikat self-signed yang perlu di-accept, lalu diarahkan ke homepage default INetSim.

Untuk mensimulasikan perilaku malware yang mendownload payload tambahan (**secondary/second-stage payload**), digunakan command `wget` dari AttackBox untuk meniru aksi "menghubungi server attacker dan minta file":

```
sudo wget https://<ip_remnux>/<namafile> --no-check-certificate
```

Flag `--no-check-certificate` diperlukan karena sertifikat INetSim bersifat self-signed sehingga tidak terverifikasi secara default. INetSim akan menjawab request apapun (file `.zip`, `.ps1`, `.exe`, dll) dengan response generik yang sama (biasanya file HTML/text default), karena tujuannya bukan menyediakan file asli, melainkan mencatat pola trafik yang terjadi.

Prinsip penting: peran AttackBox dan REMnux di skenario ini dipisah secara sengaja agar komunikasi jaringan (client meminta, server menjawab) bisa benar-benar terjadi dan diamati, meniru hubungan komputer korban dan server command-and-control (C2) attacker di dunia nyata.

### 3.4 Connection Report

Setelah simulasi selesai, INetSim dihentikan (`CTRL+C`), lalu otomatis menghasilkan report koneksi di direktori `/var/log/inetsim/report/`. Dibaca dengan:

```
sudo cat /var/log/inetsim/report/report.<id>.txt
```

Isi report mencatat: waktu koneksi, jenis koneksi (misal HTTPS), method HTTP yang dipakai (GET), URL yang diakses, dan nama file fake yang dikembalikan sebagai response. Report ini berguna sebagai bukti/log analisis trafik yang terjadi selama simulasi.

---

## 4. Memory Forensics dan Preprocessing Evidence (Task 5)

### 4.1 Konsep Preprocessing Evidence

**Preprocessing evidence** adalah praktik menjalankan tools analisis terlebih dahulu dan menyimpan hasilnya ke file teks/JSON, sehingga analis lain bisa langsung mencari dan menganalisis tanpa perlu menjalankan ulang tools dari awal. Ini mempercepat proses investigasi forensik, terutama pada kasus yang butuh kolaborasi tim atau analisis berulang.

Memory image (contoh: file `.mem`) adalah representasi kondisi RAM komputer pada momen tertentu, berisi rekaman seluruh proses, koneksi, dan aktivitas yang sedang berjalan saat capture dilakukan.

### 4.2 Volatility 3 dan Plugin Windows

**Volatility** adalah tool untuk menganalisis memory image, mengidentifikasi dan mengekstrak artefak forensik dari dalamnya. Command dasar:

```
vol3 -f <namafile.mem> <nama.plugin.Plugin>
```

Plugin Windows yang dipelajari di room ini:

- **windows.pstree.PsTree** — menampilkan daftar proses dalam bentuk tree berdasarkan parent process ID, sehingga terlihat hubungan proses induk-anak.
- **windows.pslist.PsList** — mendaftar semua proses yang sedang aktif di mesin pada saat capture.
- **windows.cmdline.CmdLine** — mendaftar argumen command line dari setiap proses, berguna untuk melihat parameter eksekusi mencurigakan.
- **windows.filescan.FileScan** — memindai objek file yang ada dalam memory image.
- **windows.dlllist.DllList** — mendaftar modul (DLL) yang di-load oleh proses tertentu, termasuk path lokasi binary-nya.
- **windows.malfind.Malfind** — mendaftar rentang memory proses yang berpotensi mengandung kode injeksi (indikasi proses disusupi/injected code).
- **windows.psscan.PsScan** — memindai proses yang ada dalam memory image (metode scan berbeda dari PsList/PsTree, bisa menemukan proses yang sudah exit atau disembunyikan).

### 4.3 Studi Kasus: WannaCry Memory Image

Memory image `wcry.mem` yang dianalisis menunjukkan indikasi infeksi ransomware WannaCry, ditandai proses mencurigakan **`@WanaDecryptor@`** (dan variannya `@WanaDecryptor@.exe`) yang muncul di beberapa plugin (PsTree, PsList, PsScan, CmdLine, DllList).

Dari plugin CmdLine dan DllList, ditemukan bahwa proses malware terkait dijalankan dari path `C:\Intel\ivecuqmanpnirkt615\taskche.exe` dan binary `@WanaDecryptor@.exe` berlokasi di `C:\Intel\ivecuqmanpnirkt615\@WanaDecryptor@.exe`. Dari plugin Malfind, dua proses pertama yang teridentifikasi memiliki indikasi kode injeksi adalah `csrss.exe` (PID 596) dan `winlogon.exe` (PID 620) — keduanya proses sistem legitimate Windows yang sering jadi target injeksi karena berjalan dengan privilese tinggi dan jarang dicurigai.

### 4.4 Batch Processing dengan For Loop (Bash)

Untuk menjalankan seluruh plugin sekaligus dan menyimpan hasilnya otomatis ke file, dipakai for loop bash:

```
for plugin in windows.malfind.Malfind windows.psscan.PsScan windows.pstree.PsTree windows.pslist.PsList windows.cmdline.CmdLine windows.filescan.FileScan windows.dlllist.DllList; do vol3 -q -f wcry.mem $plugin > wcry.$plugin.txt; done
```

Penjelasan struktur:

- `for plugin in <list>; do ... done` — pola for loop bash: variabel `plugin` diisi satu per satu dari daftar nilai, lalu blok perintah di antara `do` dan `done` dijalankan berulang untuk setiap nilai.
- `$plugin` — merujuk ke nilai variabel pada iterasi saat itu.
- `vol3 -q -f wcry.mem $plugin` — menjalankan Volatility dengan flag `-q` (quiet mode, tidak menampilkan progress bar di terminal) dan `-f` (menentukan file memory image yang dibaca).
- `> wcry.$plugin.txt` — redirect hasil output ke file teks baru, penamaan otomatis mengikuti nama plugin yang sedang diproses.

Hasil akhirnya berupa kumpulan file `.txt` terpisah untuk tiap plugin (contoh: `wcry.windows.malfind.Malfind.txt`), tanpa output apapun tampil langsung di terminal.

### 4.5 Preprocessing dengan Strings

**strings** adalah utility Linux untuk mengekstrak teks yang bisa dicetak (printable) dari file binary apapun, termasuk memory image. Berguna untuk menemukan artefak seperti URL, path, atau pesan yang tertanam dalam data mentah.

```
strings wcry.mem > wcry.strings.ascii.txt
```
Mengekstrak string ASCII standar.

```
strings -e l wcry.mem > wcry.strings.unicode_little_endian.txt
```
Flag `-e l` mengekstrak string 16-bit little-endian (umum dipakai encoding string Windows/Unicode).

```
strings -e b wcry.mem > wcry.strings.unicode_big_endian.txt
```
Flag `-e b` mengekstrak string 16-bit big-endian.

Ketiga format ini disimpan terpisah karena encoding string dalam memory bisa bervariasi tergantung bagaimana aplikasi/OS menyimpan teksnya.

---

## 5. Peran dalam Tim Keamanan

Analisis malware, simulasi jaringan, dan memory forensics seperti yang dilakukan di room ini merupakan pekerjaan **blue team** — khususnya peran Malware Analyst, Threat Intelligence Analyst, atau bagian dari tim SOC (Security Operations Center) dan DFIR (Digital Forensics & Incident Response).

Di dunia nyata, analisis jarang murni manual dari awal sampai akhir. Alur umum yang dipakai:

1. **Automated sandbox/tools terlebih dahulu** (contoh: Any.Run, Joe Sandbox, Cuckoo Sandbox, VirusTotal) untuk mendapat gambaran cepat perilaku file (koneksi jaringan, file yang di-drop, perubahan registry).
2. **Static analysis tools yang lebih canggih** (contoh: ViperMonkey untuk emulasi VBA, olevba dari paket oletools untuk ekstraksi IOC otomatis) sebagai pelengkap tool dasar seperti oledump.
3. **Manual deep-dive** hanya dilakukan saat tools otomatis gagal/tidak lengkap, untuk keperluan laporan forensik detail, atau untuk menulis signature/deteksi baru.
4. **Scripting custom** (Python, regex) untuk membantu deobfuscation berulang tanpa membaca kode satu per satu secara manual.

---

## 6. Glossary

**OLE2 (Object Linking and Embedding)** — format penyimpanan proprietary Microsoft untuk menyimpan berbagai jenis data dalam satu file (Compound File Binary Format / Structured Storage).

**Data stream** — komponen internal dalam file OLE2 yang diberi indeks oleh oledump (misal `A1`, `A2`); stream bertanda huruf `M` mengindikasikan adanya macro.

**Macro (VBA)** — script otomatis yang tertanam dalam dokumen Office, bisa dieksploitasi untuk menjalankan kode berbahaya saat dokumen dibuka.

**Staged payload / staged malware** — teknik di mana file berbahaya utama tidak langsung ditempel di file awal, melainkan didownload secara terpisah setelah trigger awal (misal macro) aktif, untuk menghindari deteksi dini.

**Obfuscation** — teknik penyamaran kode/string agar sulit dikenali oleh signature-based detection atau analis yang membaca sekilas.

**Dynamic analysis** — metode analisis malware dengan mengamati perilakunya saat dijalankan (berbeda dari static analysis yang hanya membaca kode).

**Sandbox** — environment terisolasi untuk menjalankan file mencurigakan tanpa risiko terhadap sistem produksi/asli.

**INetSim** — tool simulasi berbagai layanan internet (HTTP, HTTPS, DNS, dll) untuk keperluan dynamic analysis tanpa koneksi internet nyata.

**Second-stage / secondary payload** — file tambahan yang didownload setelah payload/malware awal berhasil berjalan, biasanya berisi malware inti atau instruksi lanjutan.

**Memory image / memory dump** — representasi beku dari isi RAM komputer pada momen tertentu, dipakai sebagai bukti forensik.

**Volatility** — framework analisis memory forensik untuk mengekstrak artefak (proses, DLL, file, koneksi) dari memory image.

**Malfind** — istilah/plugin yang merujuk pada deteksi proses dengan indikasi kode injeksi (injected code) dalam memory.

**Injected code** — kode berbahaya yang disisipkan ke dalam proses lain yang sah (legitimate), teknik umum malware untuk bersembunyi dari deteksi.

**IOC (Indicator of Compromise)** — jejak/bukti spesifik (IP, domain, hash, path file) yang mengindikasikan adanya kompromi/infeksi malware.

**C2 (Command and Control)** — server yang dikendalikan attacker untuk mengirim instruksi lanjutan atau menerima data dari malware yang sudah terinstal di korban.

---

## 7. Tools & Platform Rujukan

**oledump.py** — tool Python untuk analisis statis file OLE2 (dokumen Office lama/macro-enabled), digunakan untuk menemukan dan membongkar isi macro VBA.

**INetSim** — Internet Services Simulation Suite, bawaan REMnux, untuk mensimulasikan layanan internet saat dynamic analysis malware.

**Volatility 3 (vol3)** — framework analisis memory forensik, digunakan untuk ekstraksi artefak dari memory image.

**strings** — utility Linux bawaan sistem untuk mengekstrak teks printable dari file binary.

**CyberChef** — tool web/offline untuk deobfuscation, encoding/decoding, dan manipulasi data teks; tersedia lokal di REMnux VM atau versi online (disebutkan ada room TryHackMe terpisah berjudul "CyberChef: The Basics").

**ViperMonkey** — emulator VBA untuk menjalankan/mensimulasikan macro secara otomatis tanpa membaca kode manual satu per satu (disebutkan sebagai referensi tools real-world, bukan dipraktikkan langsung di room ini).

**olevba (paket oletools)** — tool ekstraksi IOC otomatis dari macro VBA (disebutkan sebagai referensi tools real-world, bukan dipraktikkan langsung di room ini).

**Any.Run, Joe Sandbox, Cuckoo Sandbox, VirusTotal** — platform automated sandbox untuk analisis malware cepat (disebutkan sebagai referensi tools real-world, bukan dipraktikkan langsung di room ini).

---

## 8. Catatan Ringkas untuk Ditulis Tangan

**Konsep Dasar**
- REMnux — distro Linux khusus analisis malware, tools sudah lengkap
- Sandbox — environment aman buat jalanin file mencurigakan
- Static analysis — baca kode tanpa dijalankan
- Dynamic analysis — amati perilaku pas dijalankan

**oledump.py (Task 3)**
- OLE2 — format file Microsoft (compound file/structured storage)
- Data stream — komponen dalam file OLE2, kode A1, A2, dst
- Huruf M di depan nomor stream — ada macro
- `oledump.py file` — lihat semua stream
- `-s <no>` — pilih stream tertentu (select)
- `--vbadecompress` — dekompres macro biar kebaca
- Staged payload — file jahat didownload belakangan, bukan nempel di dokumen awal
- Invoke-WebRequest — command PowerShell buat download file
- Start-Process — command PowerShell buat jalanin file
- -WindowStyle hidden — sembunyiin jendela PowerShell
- -executionpolicy bypass — skip proteksi eksekusi script
- CyberChef Find/Replace — buat bongkar obfuscation string

**INetSim (Task 4)**
- INetSim — simulasi internet palsu (HTTP, HTTPS, DNS, dll)
- Edit config: sudo nano /etc/inetsim/inetsim.conf → ganti dns_default_ip jadi IP mesin
- sudo inetsim — jalanin simulasi
- "Simulation running" — tanda sukses, abaikan http_80_tcp failed
- wget https://ip/file --no-check-certificate — simulasi malware download payload
- --no-check-certificate — skip validasi sertifikat self-signed
- Report ada di /var/log/inetsim/report/, baca pakai sudo cat
- Report isinya: method, URL, waktu, file yang diakses

**Volatility 3 / Memory Forensics (Task 5)**
- Memory image — rekaman isi RAM di momen tertentu
- vol3 -f file.mem plugin — command dasar
- PsTree — proses bentuk tree (parent-child)
- PsList — daftar proses aktif
- PsScan — scan proses (metode beda, bisa nemu yang hidden/exited)
- CmdLine — argumen command line tiap proses
- FileScan — scan objek file dalam memory
- DllList — daftar DLL yang di-load + path binary
- Malfind — deteksi proses kena code injection
- Studi kasus: wcry.mem — WannaCry, proses @WanaDecryptor@
- Proses pertama & kedua kena injeksi: csrss.exe, winlogon.exe
- For loop bash: for x in list; do command; done
- vol3 -q -f — flag -q = quiet mode (silent progress)
- strings file.mem — ekstrak ASCII text
- strings -e l — ekstrak little-endian unicode
- strings -e b — ekstrak big-endian unicode
- grep "kata" file.txt — filter cari baris tertentu di file besar

**Peran & Alur Kerja**
- Analisis malware = kerjaan blue team (SOC/DFIR/Malware Analyst), bukan red team
- Alur real-world: sandbox otomatis dulu → static tool canggih (ViperMonkey/olevba) → baru manual deep-dive kalau perlu
- IOC — bukti kompromi (IP, domain, hash, path)
- C2 — server kontrol milik attacker
