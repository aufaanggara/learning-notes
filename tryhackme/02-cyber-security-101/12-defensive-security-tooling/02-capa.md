# Resume: CAPA — Identifying Malware Capabilities

**Room:** CAPA (Static Analysis Tool oleh FireEye Mandiant)
**Tanggal resume:** 31 Agustus 2026

---

## 1. Konsep Dasar

### 1.1 Analisis Malware: Static vs Dynamic

Ada dua pendekatan utama dalam menganalisis software yang berpotensi berbahaya:

- **Static analysis** — menganalisis file tanpa menjalankannya. Aman dari risiko infeksi karena kode tidak dieksekusi. Room ini fokus pada pendekatan ini.
- **Dynamic analysis** — menjalankan file di sandbox/environment terisolasi untuk mengamati perilakunya secara langsung.

Analisis malware idealnya dilakukan di **sandbox** atau environment terisolasi agar mesin analis tidak ikut terinfeksi.

### 1.2 Apa itu CAPA

**CAPA (Common Analysis Platform for Artifacts)** adalah tool buatan tim FireEye Mandiant untuk **mengidentifikasi kapabilitas** (capabilities) sebuah file executable — apa saja yang program tersebut *mampu* lakukan, tanpa perlu reverse engineering manual.

File yang bisa dianalisis: Portable Executables (PE), ELF binaries, modul .NET, shellcode, hingga laporan sandbox.

Cara kerja: CAPA menerapkan ribuan **rule** (dalam format `.yml`) bawaan yang mendeskripsikan perilaku umum malware (network communication, file manipulation, process injection, dll), lalu mencocokkan rule tersebut dengan isi file yang dianalisis.

Manfaat utama: merangkum pengetahuan reverse engineering ke dalam tool otomatis, sehingga analis SOC/incident response bisa cepat memahami fungsi file mencurigakan.

---

## 2. Menjalankan CAPA

### 2.1 Cara Dasar

Syarat: berada di direktori yang berisi file target, lalu jalankan tool mengarah ke binary.

```powershell
capa.exe .\cryptbot.bin
```

### 2.2 Parameter Penting

| Flag | Fungsi |
|---|---|
| `-h` / `--help` | Menampilkan pesan bantuan |
| `-v` / `--verbose` | Hasil lebih detail (verbose) |
| `-vv` / `--vverbose` | Hasil sangat detail (very verbose) — menunjukkan kondisi/string spesifik yang membuat sebuah rule ter-*match* |
| `-j` | Meng-output hasil dalam format `.json` (dibutuhkan untuk dibuka di CAPA Explorer Web) |

Contoh generate file JSON untuk dianalisis lebih lanjut:

```powershell
capa.bin -j -vv .\cryptbot.bin > cryptbot_vv.json
```

Proses analisis CAPA memakan waktu lama (bisa beberapa menit hingga puluhan menit tergantung ukuran & kompleksitas file), terutama dengan flag `-vv`.

---

## 3. Struktur Output CAPA

Output CAPA terbagi menjadi beberapa blok informasi berlapis.

### 3.1 General Information (Blok Pertama)

Informasi dasar file:

- **md5, sha1, sha256** — hash kriptografis file (identitas unik file)
- **analysis** — metode analisis yang dipakai (static)
- **os** — sistem operasi target dari kapabilitas yang teridentifikasi (contoh: windows)
- **format** — format file (contoh: pe)
- **arch** — arsitektur (contoh: i386)
- **path** — lokasi file yang dianalisis

### 3.2 MITRE ATT&CK

**MITRE ATT&CK** adalah framework global yang mendokumentasikan taktik dan teknik penyerang siber di setiap tahap serangan — mulai dari initial access hingga impact. Berfungsi sebagai "playbook" standar yang dipakai analis di seluruh dunia untuk memetakan perilaku file ke pola serangan yang sudah dikenal.

Format output CAPA untuk ATT&CK:

```
ATT&CK Tactic::ATT&CK Technique::Technique Identifier
ATT&CK Tactic::ATT&CK Technique::Sub-Technique::Technique[.]Sub-technique Identifier
```

Contoh: `Defense Evasion::Obfuscated Files or Information::Indicator Removal from Tools T1027.005`
→ Defense Evasion = Tactic, Obfuscated Files or Information = Technique, Indicator Removal from Tools = Sub-Technique, T1027 = Technique ID, 005 = Sub-technique ID.

Tidak semua hasil memiliki Sub-Technique Identifier.

### 3.3 MAEC (Malware Attribute Enumeration and Characterization)

Bahasa standar untuk meng-encode dan mengomunikasikan atribut, perilaku, dan keterkaitan antar-instance malware secara terstandarisasi lintas analis/organisasi.

Dua MAEC value yang paling umum muncul dari CAPA:

- **Launcher** — file memicu tindakan mirip malware: dropping payload tambahan, mengaktifkan persistence, menghubungi C2, mengeksekusi fungsi spesifik.
- **Downloader** — file mengunduh dan mengeksekusi file lain: fetch payload/resource dari internet, pull update, eksekusi secondary stage, ambil file konfigurasi. Biasanya terlihat pada malware yang lebih kompleks.

---

## 4. MBC (Malware Behavior Catalogue)

MBC adalah katalog yang mendokumentasikan **objective** dan **behavior** malware secara lebih detail dan teknis dibanding ATT&CK. MBC melengkapi (bukan menduplikasi) ATT&CK — behavior page MBC akan mereferensikan ATT&CK saat relevan.

### 4.1 Struktur Format MBC

```
OBJECTIVE::Behavior::Method[Identifier]
OBJECTIVE::Behavior::[Identifier]
```

Format pertama punya detail tambahan **Method** (bisa disebut sub-technique); format kedua tidak.

Hierarki MBC berjenjang dari umum ke spesifik:

```
OBJECTIVE (kategori besar)
  └── BEHAVIOR (lebih spesifik)
        └── METHOD (detail teknis, opsional)
              └── IDENTIFIER (kode referensi, tidak menambah informasi baru)
```

Satu Objective bisa punya banyak Behavior, dan satu Behavior bisa punya banyak Method — karena satu file bisa menunjukkan banyak macam perilaku sekaligus.

Contoh penerapan: `DATA::Encode Data::Base64 [C0026.001]` → Objective = DATA, Behavior = Encode Data, Method = Base64, Identifier = C0026.001. Dibaca: "file punya kemampuan encode data pakai Base64."

### 4.2 Objective (Kategori Besar)

Objective didasarkan pada ATT&CK tactic dalam konteks perilaku malware, ditambah dua kategori khusus MBC (Anti-Behavioral & Anti-Static Analysis).

- **Anti-Behavioral Analysis** — malware menghindari deteksi dengan menghalangi analisis perilaku (sandbox/debugger).
- **Anti-Static Analysis** — malware menambah kompleksitas agar sulit dianalisis secara statis.
- **Collection** — mengumpulkan informasi dari mesin/jaringan target.
- **Command and Control** — membangun komunikasi dengan sistem terkompromi (C2, P2P) untuk mengendalikan, eksekusi command, atau eksfiltrasi.
- **Credential Access** — mencuri kredensial (username, password).
- **Defense Evasion** — bypass mekanisme deteksi dan keamanan sistem.
- **Discovery** — mengumpulkan informasi konfigurasi sistem/jaringan target.
- **Execution** — mengeksekusi command/kode tanpa izin pengguna.
- **Exfiltration** — mencuri dan mengekstrak data sensitif dari sistem.
- **Impact** — memanipulasi, mengganggu, atau merusak sistem dan data.
- **Lateral Movement** — menyebar melalui jaringan (aktif via akses mesin, atau pasif via email).
- **Persistence** — tetap beroperasi dan tidak terdeteksi dalam jangka waktu lama.
- **Privilege Escalation** — meningkatkan hak akses/kontrol atas sistem target.

### 4.3 Micro-Objective

Micro-objective berkaitan dengan **micro-behavior** — tindakan yang tidak selalu bersifat jahat dan bisa dipakai software normal (contoh: aplikasi messaging), namun sering disalahgunakan malware sehingga tetap ditandai CAPA.

- **PROCESS** — Creating Process, Setting Thread Context, Terminating Process, Checking Mutex.
- **MEMORY** — Allocating Memory, Changing Memory Protection, Freeing Memory.
- **COMMUNICATION** — traffic DNS, FTP, HTTP, ICMP, SMTP.
- **DATA** — Checking strings, compressing, decoding, encoding data.

Catatan: output akhir CAPA menampilkan Objective dan Micro-Objective hanya di kolom **Objective**; Behavior dan Micro-Behavior hanya di kolom **Behavior**.

### 4.4 Contoh MBC Behaviors Kunci

- **Lab Machine Detection [B0009]** — malware memeriksa apakah berjalan di lingkungan virtual (anti-VM).
- **Executable Code Obfuscation [B0032]** — kode sengaja diobfuskasi untuk mencegah static analysis.
- **Command and Scripting Interpreter [E1059]** — mengeksploitasi interpreter (cmd.exe, PowerShell, Bash, Python) untuk menjalankan command/script berbahaya.
- **File and Directory Discovery [E1083]** — mencari file spesifik dengan enumerasi file/direktori.
- **Obfuscated Files or Information [E1027]** — mengobfuskasi file/informasi lewat encoding atau encryption.

### 4.5 Methods

Method adalah detail teknis tambahan di bawah suatu Behavior. Karena terikat ke behavior spesifik, untuk melihat semua Method perlu merujuk ke behavior/micro-behavior yang bersangkutan.

Contoh Method kunci:

- **Argument Obfuscation [B0032.020]** — argumen number/string API call dihitung saat runtime agar sulit dianalisis.
- **Stack Strings [B0032.017]** — membangun dan mendekripsi string di stack setiap kali dipakai, lalu dibuang, menghindari referensi statis yang jelas.
- **Read Header [C0002.014]** — membaca header HTTP.
- **Base64 [C0026.001]** — encode data pakai Base64.
- **XOR [C0026.002]** — encode data pakai XOR.
- **Encoding-Standard Algorithm [E1027.m02]** — encode file/informasi pakai algoritma standar (contoh: base64).

---

## 5. Namespace dan Capability

### 5.1 Namespace

**Namespace** dipakai CAPA untuk mengelompokkan rule-rule yang punya tujuan sama, mirip sistem folder.

Format:

```
Capability(Rule Name)::TLN(Top-Level Namespace)/Namespace
```

**Top-Level Namespace (TLN)** adalah kategori paling atas. Daftar TLN utama:

- **anti-analysis** — rule untuk deteksi perilaku menghindari analisis (obfuscation, packing, anti-debugging).
- **collection** — rule terkait data yang dikumpulkan/dienumerasi untuk eksfiltrasi.
- **communication** — rule terkait perilaku komunikasi jaringan (transmisi data, C2).
- **compiler** — rule pengenal environment build/compiler yang dipakai.
- **data-manipulation** — rule perilaku mengubah data dalam file executable (string encryption, data encoding).
- **executable** — rule terkait atribut file executable (section PE, debug info).
- **host-interaction** — rule interaksi dengan sistem host (baca/tulis/modifikasi file & direktori).
- **impact** — rule terkait potensi konsekuensi/efek perilaku program (remote access, eksfiltrasi, destruksi).
- **internal** — rule untuk keperluan internal CAPA, bukan untuk pelaporan analis.
- **lib** — building block untuk membuat rule lain.
- **linking** — rule pengenal linking/dynamic loading kode eksternal atau library saat eksekusi.
- **load-code** — rule terkait dynamic loading/eksekusi kode ("runtime code injection").
- **malware-family** — rule terkait karakteristik khas family/group malware tertentu.
- **nursery** — staging ground untuk rule yang belum sepenuhnya dipoles/matang.
- **persistence** — rule terkait mempertahankan akses/keberadaan di sistem terkompromi.
- **runtime** — rule pengenal bahasa/platform tempat program berjalan.
- **targeting** — rule terkait program yang berinteraksi dengan ATM.

Di dalam satu TLN, ada beberapa **Namespace** (sub-kategori), dan di dalam satu Namespace ada beberapa **rule** (`.yml`). Contoh: TLN `Anti-Analysis` → Namespace `anti-vm/vm-detection` → rule `reference-anti-vm-strings-targeting-virtualbox.yml`.

Struktur hierarki lengkap:

```
Top-Level Namespace
  └── Namespace 1 → rule-1.yml, rule-2.yml, ...
  └── Namespace 2 → rule-1.yml, rule-2.yml, ...
```

### 5.2 Capability dan Rule

**Capability** adalah nama hasil temuan yang ditampilkan CAPA, yang sebenarnya adalah nama dari **rule** yang matched — hanya spasi diganti karakter dash (`-`) saat jadi nama file `.yml`.

Contoh: Capability "**reference anti-VM strings**" ↔ file rule `reference-anti-vm-strings.yml`.

**Pengecualian penting:** kadang rule sebuah Capability tidak ditemukan di bawah TLN yang secara logis seharusnya (misal capability "reference cryptocurrency strings" seharusnya di bawah TLN `Impact`, tapi sebenarnya ada di TLN **Nursery** — karena rule tersebut belum sepenuhnya dipoles/matang).

### 5.3 Rule (.yml) — Detail

Rule adalah file bawaan CAPA (bukan dibuat/upload manual oleh user) yang berisi kondisi pencocokan. Struktur rule terdiri dari:

- **meta** — nama rule, namespace, authors, scope (static/dynamic), pemetaan att&ck, pemetaan mbc, references, examples.
- **features** — daftar kondisi (string, regex, match) yang dicek CAPA di dalam file target. Bisa pakai operator logika:
  - `or:` — cukup salah satu kondisi terpenuhi (contoh: rule anti-VM VMware, cukup ketemu satu dari puluhan string terkait VMware).
  - `and:` — semua kondisi harus terpenuhi sekaligus (contoh: rule "schedule task via schtasks" butuh kombinasi `host-interaction/process/create` DAN string `schtasks` DAN string `/create` bersamaan).

---

## 6. CAPA Explorer Web

Tool berbasis web untuk menjelajahi hasil CAPA secara interaktif, alternatif dari membaca file `.txt`/JSON mentah yang bisa mencapai ribuan baris.

### 6.1 Cara Pakai

1. Generate file JSON: `capa -j /path/to/file > result.json`
2. Buka CAPA Explorer Web (versi online via link resmi, atau versi offline `capa_web_explorer_offline.html`)
3. Upload file JSON via tombol **Upload from local**, atau **Load from URL**

### 6.2 Fitur Utama

- **Tabel hasil** — kolom Rule, Address, Namespace, ATT&CK Technique, Malware Behavior Catalog; setiap baris rule bisa di-*expand* untuk melihat detail kondisi yang matched serta alamat memori (`file+0x...`) tempat string ditemukan.
- **View Rule** — menampilkan isi lengkap file `.yml` dari rule yang matched, termasuk bagian `features` yang jadi dasar deteksi.
- **Show capabilities by function** — checkbox tampilan alternatif.
- **Show distinct library rules** — checkbox tampilan alternatif.
- **Show namespace chart** — checkbox tampilan alternatif.
- **Show column filters** — checkbox tampilan alternatif.
- **Global Search Box** — pencarian cepat rule/capability spesifik tanpa perlu scroll manual di data yang sangat banyak.

---

## 7. Alur Kerja Analisis dengan CAPA (Ringkasan Praktis)

1. Jalankan `capa.exe target.bin` untuk hasil dasar.
2. Kalau butuh detail lebih, tambahkan `-v` atau `-vv`.
3. Untuk analisis lebih mudah pada file besar, generate JSON: `capa -j -vv target.bin > hasil.json`.
4. Upload JSON ke CAPA Explorer Web untuk eksplorasi interaktif.
5. Baca hasil secara berlapis: **General Info** → **ATT&CK Tactic/Technique** → **MAEC category** → **MBC Objective/Behavior** → **Capability/Namespace** → (opsional) buka **rule .yml** untuk lihat kondisi teknis spesifik yang men-trigger deteksi.

---

## 8. Glossary

- **CAPA** — tool static analysis dari FireEye Mandiant untuk identifikasi kapabilitas file executable.
- **Static analysis** — analisis file tanpa mengeksekusinya.
- **Dynamic analysis** — analisis dengan menjalankan file di environment terisolasi.
- **Sandbox** — environment terisolasi untuk menganalisis file berbahaya dengan aman.
- **MITRE ATT&CK** — framework global taktik & teknik serangan siber.
- **Tactic** — kategori besar tujuan serangan dalam ATT&CK (contoh: Defense Evasion).
- **Technique** — cara spesifik mencapai tactic tersebut.
- **Sub-Technique** — varian lebih spesifik dari technique.
- **MAEC (Malware Attribute Enumeration and Characterization)** — bahasa standar untuk mendeskripsikan atribut malware.
- **Launcher** — MAEC value: file memicu aksi mirip malware (drop payload, aktifkan persistence, dsb).
- **Downloader** — MAEC value: file mengunduh & mengeksekusi file lain.
- **MBC (Malware Behavior Catalogue)** — katalog perilaku malware yang lebih detail dan melengkapi ATT&CK.
- **Objective** — kategori besar perilaku dalam MBC, mirip Tactic pada ATT&CK.
- **Behavior** — perilaku spesifik di bawah suatu Objective.
- **Method** — detail teknis di bawah suatu Behavior (sub-technique MBC).
- **Micro-Objective / Micro-Behavior** — perilaku level rendah (low-level) yang tidak selalu jahat tapi sering disalahgunakan malware.
- **Namespace** — pengelompokan rule berdasarkan tujuan yang sama.
- **Top-Level Namespace (TLN)** — kategori paling atas dari namespace.
- **Capability** — nama hasil temuan CAPA, identik dengan nama rule yang matched.
- **Rule (.yml)** — file konfigurasi bawaan CAPA berisi kondisi (features) untuk mendeteksi suatu capability.
- **Features** — bagian dalam rule yang berisi kondisi pencocokan (string, regex, match) dengan operator `or`/`and`.
- **Nursery** — TLN placeholder untuk rule yang belum sepenuhnya matang/dipoles.
- **CAPA Explorer Web** — tool web untuk menjelajahi hasil CAPA (format JSON) secara interaktif.

---

## 9. Tools & Platform Rujukan

- **CAPA** — tool static analysis untuk identifikasi kapabilitas malware. Install: `pip install flare-capa` atau download standalone executable release.
- **CAPA Explorer Web** — tool web untuk eksplorasi hasil CAPA (`.json`) secara interaktif; tersedia versi online dan offline (`capa_web_explorer_offline.html`).
- **CAPA GitHub Repository** — sumber informasi lebih detail mengenai tool dan seluruh rule bawaan CAPA.
- **MITRE ATT&CK Framework** — referensi framework taktik & teknik serangan siber; direkomendasikan dipelajari sebagai prasyarat sebelum room ini.
- **MBC Summary** — referensi daftar lengkap seluruh konten Malware Behavior Catalogue (Objective, Behavior, Method, Identifier).

---

## 10. Catatan Ringkas untuk Ditulis Tangan

**Konsep Dasar**
- Static analysis — analisis tanpa jalankan file
- Dynamic analysis — analisis dengan jalankan file (sandbox)
- CAPA — tool identifikasi kapabilitas file, buatan Mandiant
- File didukung — PE, ELF, .NET, shellcode, sandbox report

**Command CAPA**
- `capa.exe file.bin` — analisis dasar
- `-h` — help
- `-v` — verbose
- `-vv` — very verbose (tampilkan kondisi matched detail)
- `-j` — output JSON
- `capa -j -vv file.bin > hasil.json` — generate JSON detail

**General Info Output**
- md5, sha1, sha256 — hash file
- analysis — metode analisis
- os — OS target
- format — format file (pe)
- arch — arsitektur (i386, x86, dst)
- path — lokasi file

**MITRE ATT&CK**
- Format: Tactic::Technique::ID
- Tactic — kategori besar (misal Defense Evasion)
- Technique — cara spesifik
- Sub-technique — varian lebih detail

**MAEC**
- Launcher — trigger aksi mirip malware
- Downloader — download & eksekusi file lain

**MBC — hierarki**
- Objective → Behavior → Method → Identifier
- Objective = kategori besar (Anti-Behavioral Analysis, Anti-Static Analysis, Discovery, Execution, Persistence, dst)
- Micro-Objective — PROCESS, MEMORY, COMMUNICATION, DATA
- Format: OBJECTIVE::Behavior::Method[ID]

**MBC Behavior kunci**
- Lab Machine Detection [B0009] — deteksi VM
- Executable Code Obfuscation [B0032] — obfuscate kode
- Command and Scripting Interpreter [E1059] — pakai cmd/PowerShell/bash
- File and Directory Discovery [E1083] — enumerasi file
- Obfuscated Files or Information [E1027] — encode/encrypt file

**Method kunci**
- Argument Obfuscation [B0032.020]
- Stack Strings [B0032.017]
- Base64 [C0026.001]
- XOR [C0026.002]

**Namespace**
- TLN — Top-Level Namespace, kategori paling atas
- TLN utama — anti-analysis, collection, communication, compiler, data-manipulation, executable, host-interaction, impact, internal, lib, linking, load-code, malware-family, nursery, persistence, runtime, targeting
- Nursery — rule belum matang, exception dari kategori seharusnya

**Capability & Rule**
- Capability = nama rule (spasi jadi dash di file .yml)
- Rule .yml isi: meta (name, namespace, att&ck, mbc, examples) + features (kondisi)
- Operator features: or (salah satu cukup), and (semua wajib)

**CAPA Explorer Web**
- Upload from local / Load from URL — cara load JSON
- Global Search Box — cari rule cepat
- View Rule — lihat isi .yml rule matched
- Fitur checkbox: show capabilities by function, show distinct library rules, show namespace chart, show column filters

**Alur kerja**
1. capa file.bin
2. tambah -v/-vv kalau perlu detail
3. capa -j -vv file.bin > hasil.json
4. upload ke CAPA Explorer Web
5. baca: General Info → ATT&CK → MAEC → MBC → Capability/Namespace → View Rule
