# Resume: FlareVM - Defensive Security Tooling

**Room:** FlareVM_Defensive_Security_Toolingv6
**Tanggal:** 31 Agustus 2026

---

## 1. Pengantar FlareVM

**1.1 Definisi**

**FlareVM** adalah singkatan dari "Forensics, Logic Analysis, and Reverse Engineering". Ini adalah environment Windows yang sudah dikurasi dan dilengkapi kumpulan tools khusus untuk reverse engineering, analisis malware, incident response, dan investigasi forensik. Dikembangkan oleh Tim FLARE dari FireEye.

**1.2 Tujuan Penggunaan**

FlareVM dipakai untuk membongkar (unravel) misteri digital, memahami perilaku malware, dan menyelami detail teknis di dalam file executable — tanpa harus membangun toolkit sendiri dari nol (yang bisa memakan waktu instalasi berjam-jam).

**1.3 Akses Lab**

Kredensial default untuk akses Remote Desktop ke mesin FlareVM:

```
Username: Administrator
Password: letmein123!
IP Address: MACHINE_IP
```

File-file sample yang dipakai untuk latihan umumnya berada di `C:\Users\Administrator\Desktop\Sample`.

**1.4 Catatan Keamanan Penting**

Mesin FlareVM berisi sample file **malicious** sebagai bahan latihan dan **tidak memiliki akses internet**. File-file ini tidak boleh didownload, dijalankan di luar VM, atau didistribusikan. Selalu tangani sample malware hanya di lingkungan yang terisolasi, terkontrol, dan aman.

---

## 2. Kategori Tools di FlareVM

FlareVM mengelompokkan tools berdasarkan fungsinya. Berikut kategori dan contoh tools di masing-masing kategori.

**2.1 Reverse Engineering & Debugging**

Reverse engineering adalah proses membongkar produk jadi untuk memahami cara kerjanya. Debugging adalah proses mengidentifikasi, memahami, dan memperbaiki error pada kode.

- **Ghidra** — suite reverse engineering open-source buatan NSA.
- **x64dbg** — debugger open-source untuk binary x64 dan x32.
- **OllyDbg** — debugger untuk reverse engineering di level assembly.
- **Radare2** — platform open-source untuk reverse engineering.
- **Binary Ninja** — tool disassembling dan decompiling binary.
- **PEiD** — tool deteksi packer, cryptor, dan compiler.

**2.2 Disassemblers & Decompilers**

Tools kategori ini membantu memahami perilaku, logika, dan control flow malware dengan mengubah kode mesin menjadi format yang lebih mudah dibaca.

- **CFF Explorer** — editor file PE (Portable Executable) untuk analisis dan edit.
- **Hopper Disassembler** — debugger, disassembler, dan decompiler.
- **RetDec** — decompiler open-source untuk machine code.

**2.3 Static & Dynamic Analysis**

**Static analysis** adalah pemeriksaan kode tanpa menjalankannya. **Dynamic analysis** adalah pengamatan perilaku program saat dijalankan.

- **Process Hacker** — memory editor dan process watcher.
- **PEview** — viewer portabel untuk file PE.
- **Dependency Walker** — menampilkan dependensi DLL dari sebuah executable.
- **DIE (Detect It Easy)** — deteksi packer, compiler, dan cryptor.

**2.4 Forensics & Incident Response**

**Digital Forensics** mencakup pengumpulan, analisis, dan pengamanan bukti digital. **Incident Response** fokus pada deteksi, containment, eradication, dan recovery dari serangan siber.

- **Volatility** — framework analisis dump RAM (memory forensics).
- **Rekall** — framework memory forensics untuk incident response.
- **FTK Imager** — akuisisi dan analisis disk image untuk forensik.

**2.5 Network Analysis**

Metode dan teknik untuk mempelajari jaringan guna mengungkap pola, mengoptimalkan performa, dan memahami perilaku jaringan.

- **Wireshark** — network protocol analyzer untuk merekam dan memeriksa traffic.
- **Nmap** — deteksi kerentanan dan pemetaan jaringan.
- **Netcat** — membaca dan menulis data melalui koneksi jaringan.

**2.6 File Analysis**

Teknik memeriksa file untuk potensi ancaman keamanan dan memastikan permission file.

- **FileInsight** — menelusuri dan mengedit binary file.
- **Hex Fiend** — hex editor ringan dan cepat.
- **HxD** — viewing dan editing binary file dengan hex editor.

**2.7 Scripting & Automation**

Menggunakan script (PowerShell, Python) untuk mengotomatiskan tugas repetitif.

- **Python** — otomatisasi berbasis module dan tools Python.
- **PowerShell Empire** — framework post-exploitation berbasis PowerShell.

**2.8 Sysinternals Suite**

Kumpulan utilitas sistem lanjutan untuk mengelola, troubleshoot, dan mendiagnosis sistem Windows.

- **Autoruns** — menampilkan executable yang dikonfigurasi berjalan saat boot-up.
- **Process Explorer** — informasi proses yang sedang berjalan.
- **Process Monitor** — memantau dan mencatat aktivitas process/thread secara real-time.

---

## 3. Tools Inti untuk Investigasi

Room ini fokus pada tujuh tools utama sebagai standar investigasi awal.

**3.1 Procmon (Process Monitor)**

Mencatat dan melacak aktivitas sistem serta file Windows secara real-time. Berguna untuk riset malware, troubleshooting, dan forensik. Memantau file system, registry, serta aktivitas thread/process.

Contoh use case: memantau proses `lsass.exe` (Local Security Authority Subsystem Service) yang menangani autentikasi dan sering berkomunikasi dengan `lsasrv.dll`. LSASS sering jadi target credential dumping (misalnya oleh Mimikatz), sehingga pola akses tak wajar ke `lsass.exe` patut dicurigai.

Cara filter di Procmon untuk mempersempit tampilan proses:

1. Buka filter dengan menekan `CTRL + L` atau klik ikon filter.
2. Pilih kolom **Process Name**.
3. Pilih relasi **contains**.
4. Isi value yang berkaitan dengan proses target (misalnya `cobalt`).
5. Set action ke **Include**.
6. Klik **Add**, lalu klik **Apply**.

**3.2 Process Explorer (Procexp)**

Menampilkan insight mendalam terhadap proses aktif, termasuk hubungan parent-child, DLL yang di-load, dan path proses. Berguna untuk mengetahui program apa yang mengakses file/folder tertentu, dan untuk melacak proses apa saja yang di-spawn dari dokumen Word, file LNK, atau ISO — teknik yang sering disalahgunakan threat actor.

Untuk melihat konektivitas jaringan sebuah proses: klik kanan proses, pilih **Properties**, buka tab **TCP/IP**. Di sana tampil Local Address, Remote Address, dan State koneksi (misalnya `SYN_SENT`).

**3.3 HxD (Hex Editor)**

Hex editor cepat dan fleksibel untuk mengedit file, memori, dan drive. Dipakai untuk forensik, data recovery, debugging, dan manipulasi data binary presisi.

Fitur **Data Inspector** memungkinkan pemeriksaan byte individual dalam berbagai tipe data (integer, float, dsb).

Signature `4D 5A` (little-endian, terbaca "MZ") di awal file mengindikasikan file tersebut sebenarnya adalah **executable**, walaupun ekstensinya bukan `.exe` (misalnya `.txt`).

**3.4 CFF Explorer**

Menampilkan informasi file PE secara komprehensif: file hash (MD5, SHA-1), metadata (CompanyName, FileDescription, OriginalFilename), dan struktur header PE (DOS Header, NT Headers, dsb). Dipakai untuk verifikasi integritas file, autentikasi sumber file sistem, dan validasi keasliannya.

Field **e_magic** pada DOS Header menunjukkan nilai `5A4D` (word, little-endian) — representasi lain dari signature MZ yang sama dengan yang terlihat di HxD.

Malware sering menyamar sebagai tool sistem legitimate (misalnya REGEDIT) dengan mengisi metadata palsu pada FileDescription/CompanyName, meski nama filenya berbeda dari aslinya.

**3.5 Wireshark**

Network protocol analyzer untuk memburu koneksi mencurigakan, memeriksa protokol, dan mendeteksi kemungkinan serangan atau eksfiltrasi data. Traffic terenkripsi (TLSv1.2) bisa menyamarkan aktivitas berbahaya maupun traffic sah.

Filter dasar untuk fokus pada IP tertentu:

```
ip.addr == <IP_ADDRESS>
```

Dari kolom Info pada packet TCP, format `source_port → destination_port` menunjukkan port asal (biasanya port ephemeral/acak) dan port tujuan di server remote.

**3.6 PEStudio**

Tool untuk **static analysis** — mempelajari properti file executable tanpa menjalankannya (menghindari risiko eksekusi malware). Menampilkan informasi seperti:

- **md5 / sha1 / sha256** — hash file untuk identifikasi dan pencocokan database (VirusTotal, dsb).
- **entropy** — ukuran keacakan data file (skala 0–8). Nilai mendekati 8 (misalnya 7.999) mengindikasikan kemungkinan file di-pack, di-compress, atau dienkripsi.
- **imphash** — import hash, digunakan untuk mengidentifikasi kesamaan pola import antar-malware family.
- **rich-header** — ketiadaan rich-header (n/a) mengindikasikan kemungkinan file di-pack/obfuscated untuk menghindari deteksi static analysis.
- **manifest** — berisi konfigurasi seperti `requestedExecutionLevel` (misalnya `requireAdministrator`), yang menunjukkan privilege yang diminta aplikasi saat dijalankan.
- **functions** — daftar API calls yang di-import (IAT/Import Address Table). Tab **blacklist** mengurutkan fungsi-fungsi mencurigakan ke atas.

Fungsi mencurigakan penting yang sering muncul:

- `set_UseShellExecute` — memungkinkan proses menggunakan shell OS untuk menjalankan proses lain; sering dipakai malware untuk men-spawn proses tambahan.
- `RijndaelManaged`, `CryptoStream`, `CipherMode`, `CreateDecryptor` — mengindikasikan penggunaan fungsi kriptografi (Rijndael/AES), berpotensi untuk enkripsi komunikasi, file, atau fungsi ransomware.

**3.7 FLOSS (FLARE Obfuscated String Solver)**

Sebelumnya bernama FireEye Labs Obfuscated String Solver. Mengekstrak dan men-deobfuscate string dari program malware secara otomatis menggunakan teknik static analysis lanjutan — meningkatkan kemampuan dasar `strings.exe`. Menyertakan script Python tambahan untuk load hasil ke tools lain (IDA Pro, Binary Ninja).

Command dasar:

```
floss .\<nama_file.exe>
```

Bisa dijalankan dengan output ke file:

```
FLOSS.exe .\<nama_file.exe> > output.txt
```

Catatan: FLOSS tidak mendukung deobfuscation string spesifik-.NET — untuk binary .NET, FLOSS hanya melakukan ekstraksi static string biasa tanpa decoding.

Hasil ekstraksi string bisa berisi informasi hardcoded seperti file paths, URL (kemungkinan C2), IP address, API calls, error messages, registry keys, dan encryption keys.

---

## 4. Alur Kerja Analisis Malware (Studi Kasus)

**4.1 Skenario**

File mencurigakan `windows.exe` didownload user dan ditandai sebagai potensi ancaman. File dikirim ke folder `C:\Users\Administrator\Desktop\Sample` untuk dianalisis.

**4.2 Tahap 1 — Static Analysis dengan PEStudio**

Buka file dengan PEStudio, periksa hash, entropy, manifest, dan daftar function/blacklist. Pada kasus `windows.exe`:

- Entropy tinggi (7.999) → indikasi packing/enkripsi.
- Manifest meminta `requireAdministrator` → butuh privilege tinggi.
- Metadata mengklaim sebagai REGEDIT, tapi berlokasi di folder download (bukan `C:\Windows\System32` seperti REGEDIT asli) → tanda penyamaran (masquerading).
- Metadata mengandung teks berbahasa Rusia (mis. "Редактор реестра") — mencurigakan jika organisasi tidak beroperasi di lingkungan berbahasa Rusia.
- Fungsi blacklist `set_UseShellExecute` dan fungsi kriptografi `RijndaelManaged` terdeteksi.

**4.3 Tahap 2 — Static Analysis dengan FLOSS**

Ekstraksi string dari `windows.exe` menghasilkan string yang konsisten dengan temuan PEStudio (RijndaelManaged, CipherMode, CreateDecryptor, dsb) — dua tools berbeda saling mengonfirmasi temuan yang sama, memperkuat validitas analisis.

**4.4 Tahap 3 — Analisis Konektivitas dengan Process Explorer & Procmon**

Studi kasus kedua menggunakan file `cobaltstrike.exe` untuk memeriksa konektivitas jaringan:

1. Jalankan file, buka Process Explorer, konfirmasi hubungan parent-child (`cobaltstrike.exe` child dari `explorer.exe`).
2. Cek tab TCP/IP pada Properties proses untuk melihat Remote Address dan state koneksi (SYN_SENT).
3. Cross-check temuan menggunakan Procmon dengan filter Process Name contains "cobalt" — hasil menunjukkan operation **TCP Reconnect/Disconnect** berulang ke IP yang sama, dengan Result **SUCCESS**.
4. Verifikasi tambahan lewat Wireshark dengan filter `ip.addr == <IP>` untuk melihat detail port sumber dan tujuan pada level packet.

**4.5 Prinsip Validasi Silang**

Analisis malware yang baik tidak bergantung pada satu tool saja. Temuan dari satu tool (misalnya Process Explorer) sebaiknya diverifikasi ulang dengan tool lain (Procmon, Wireshark) untuk memastikan akurasi sebelum disimpulkan.

---

## 5. Glossary

- **FlareVM** — Forensics, Logic Analysis, and Reverse Engineering; VM berisi kumpulan tools untuk analisis malware dan forensik, dibuat oleh Tim FLARE (FireEye).
- **Reverse Engineering** — membongkar produk jadi untuk memahami cara kerjanya.
- **Static Analysis** — memeriksa file/kode tanpa menjalankannya.
- **Dynamic Analysis** — mengamati perilaku file/kode saat dijalankan.
- **PE (Portable Executable)** — format file executable pada Windows (.exe, .dll, dsb).
- **Entropy** — ukuran keacakan data; nilai tinggi mengindikasikan kemungkinan packing/enkripsi.
- **Imphash** — hash dari Import Address Table, dipakai mengidentifikasi kesamaan pola import antar file.
- **IAT (Import Address Table)** — daftar API/fungsi yang di-import oleh sebuah executable.
- **Rich Header** — bagian dari PE header berisi metadata compiler; ketiadaannya bisa mengindikasikan file di-pack/obfuscated.
- **Masquerading** — teknik malware menyamar sebagai file/tool legitimate.
- **C2 (Command and Control)** — server yang dipakai attacker untuk mengendalikan malware/implant dari jarak jauh.
- **Defanging** — teknik menulis IP/URL berbahaya dengan cara "dijinakkan" (mis. `47[.]120[.]46[.]210`) agar tidak ter-klik/ter-trigger otomatis.
- **LSASS (Local Security Authority Subsystem Service)** — proses Windows yang menangani autentikasi; target umum credential dumping.
- **Credential Dumping** — teknik mengekstrak kredensial (password, hash) dari memori sistem, mis. lewat LSASS.
- **Rijndael** — algoritma dasar dari AES (Advanced Encryption Standard).
- **Parent-Child Process** — hubungan proses yang men-spawn (parent) dan proses yang di-spawn (child).
- **String Deobfuscation** — proses mengungkap string yang sengaja disamarkan/dienkripsi oleh malware.
- **Blacklist Function** — fungsi/API yang ditandai berisiko tinggi karena sering disalahgunakan malware.

---

## 6. Tools & Platform Rujukan

- **Ghidra** — suite reverse engineering open-source buatan NSA.
- **x64dbg** — debugger open-source untuk binary x64/x32.
- **OllyDbg** — debugger untuk reverse engineering level assembly.
- **Radare2** — platform reverse engineering open-source.
- **Binary Ninja** — tool disassembling dan decompiling binary.
- **PEiD** — deteksi packer, cryptor, dan compiler.
- **CFF Explorer** — editor file PE, generate hash, cek metadata dan struktur header.
- **Hopper Disassembler** — debugger, disassembler, decompiler.
- **RetDec** — decompiler open-source untuk machine code.
- **Process Hacker** — memory editor dan process watcher.
- **PEview** — viewer portabel untuk file PE.
- **Dependency Walker** — menampilkan dependensi DLL sebuah executable.
- **DIE (Detect It Easy)** — deteksi packer, compiler, dan cryptor.
- **Volatility** — framework analisis dump RAM (memory forensics).
- **Rekall** — framework memory forensics untuk incident response.
- **FTK Imager** — akuisisi dan analisis disk image forensik.
- **Wireshark** — network protocol analyzer untuk capture dan analisis traffic.
- **Nmap** — deteksi kerentanan dan pemetaan jaringan.
- **Netcat** — membaca/menulis data lewat koneksi jaringan.
- **FileInsight** — menelusuri dan mengedit binary file.
- **Hex Fiend** — hex editor ringan dan cepat.
- **HxD** — hex editor untuk viewing dan editing binary file.
- **Python** — otomatisasi berbasis module/tools Python.
- **PowerShell Empire** — framework post-exploitation berbasis PowerShell.
- **Autoruns** — menampilkan executable yang berjalan saat boot-up.
- **Process Explorer** — informasi mendalam proses aktif dan hubungan parent-child.
- **Process Monitor (Procmon)** — memantau aktivitas file system, registry, process/thread real-time.
- **PEStudio** — static analysis properti file executable tanpa eksekusi.
- **FLOSS** — ekstraksi dan deobfuscation string dari malware (FLARE Obfuscated String Solver).
- **VirusTotal** — database publik untuk mencocokkan hash file (MD5/SHA-1/SHA256) dengan hasil deteksi antivirus/vendor keamanan lain.

---

## 7. Catatan Ringkas untuk Ditulis Tangan

**FlareVM**
FlareVM — Forensics, Logic Analysis, Reverse Engineering, dibuat FLARE Team (FireEye)
Kredensial default — Administrator / letmein123!
Sample file lokasi — C:\Users\Administrator\Desktop\Sample
Catatan — sample malicious, no internet, jangan dijalankan di luar VM

**Kategori Tools**
Reverse Engineering — Ghidra, x64dbg, OllyDbg, Radare2, Binary Ninja, PEiD
Disassembler/Decompiler — CFF Explorer, Hopper, RetDec
Static/Dynamic Analysis — Process Hacker, PEview, Dependency Walker, DIE
Forensics/IR — Volatility, Rekall, FTK Imager
Network Analysis — Wireshark, Nmap, Netcat
File Analysis — FileInsight, Hex Fiend, HxD
Scripting — Python, PowerShell Empire
Sysinternals — Autoruns, Process Explorer, Process Monitor

**Procmon**
Fungsi — record real-time file system/registry/thread activity
Filter — CTRL+L → Process Name contains [value] → Include → Add → Apply
LSASS — proses autentikasi, target credential dumping (Mimikatz)

**Process Explorer**
Fungsi — lihat proses aktif, parent-child, DLL loaded, path
TCP/IP tab — klik kanan proses → Properties → TCP/IP → lihat Remote Address, State

**HxD**
Fungsi — hex editor, forensik, data recovery
Signature 4D 5A (hex) / 5A4D (word LE) = "MZ" — tanda file executable

**CFF Explorer**
Fungsi — generate hash, cek metadata file PE, struktur header
e_magic (DOS Header) — 5A4D, sama dengan signature MZ
Masquerading — metadata palsu (FileDescription/CompanyName) menyamar sistem asli

**Wireshark**
Fungsi — capture & analisis traffic jaringan
Filter IP — ip.addr == [IP]
Info packet — source_port → destination_port

**PEStudio**
Fungsi — static analysis tanpa eksekusi file
md5/sha1/sha256 — hash file, cek ke VirusTotal
entropy — dekat 8 = kemungkinan packed/encrypted
imphash — import hash, identifikasi malware family
rich-header n/a — indikasi packing/obfuscation
manifest requestedExecutionLevel — requireAdministrator = butuh privilege tinggi
functions/blacklist — API mencurigakan
set_UseShellExecute — jalankan proses lain lewat shell OS
RijndaelManaged, CryptoStream, CipherMode, CreateDecryptor — fungsi kriptografi (AES)

**FLOSS**
Fungsi — ekstrak & deobfuscate string malware
Command — floss .\namafile.exe
Command output ke file — FLOSS.exe .\namafile.exe > output.txt
Catatan — tidak deobfuscate string .NET

**Alur Analisis Malware**
1. Static analysis PEStudio — cek hash, entropy, manifest, functions
2. Static analysis FLOSS — ekstrak string, cross-check dengan PEStudio
3. Process Explorer — cek parent-child, TCP/IP properties
4. Procmon — filter process name, cek TCP Reconnect/Disconnect
5. Wireshark — verifikasi packet level (source/destination port)
6. Prinsip — selalu cross-check pakai lebih dari 1 tool

**Istilah Kunci**
PE — Portable Executable (format exe Windows)
Entropy — ukuran keacakan data
Imphash — hash Import Address Table
IAT — Import Address Table
Rich Header — metadata compiler di PE header
C2 — Command and Control server
Defanging — IP/URL "dijinakkan" mis. 47[.]120[.]46[.]210
Credential Dumping — ekstrak kredensial dari memori
Rijndael — algoritma dasar AES
