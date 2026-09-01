# Resume Materi: Logs (TryHackMe)
**Tanggal:** 31 Agustus 2026

---

## 1. Konsep Dasar Log

### 1.1 Definisi Log

**Log** adalah jejak digital yang ditinggalkan oleh setiap aktivitas dalam sebuah sistem atau perangkat. Aktivitas ini bisa berupa aktivitas normal maupun aktivitas dengan niat jahat (malicious). Log tersimpan di berbagai file, terpisah berdasarkan kategori informasi yang diberikan.

Prinsip dasarnya: penyerang berusaha meminimalkan jejak agar tidak terdeteksi, tapi log tetap merekam aktivitas sehingga tim keamanan bisa merekonstruksi kejadian dan menentukan siapa pelakunya — mirip seperti tim forensik yang menyatukan berbagai jejak fisik (jejak kaki, kerusakan, rekaman CCTV) untuk menyimpulkan kronologi sebuah kejadian.

### 1.2 Use Case Log

| Use Case | Fungsi |
|---|---|
| Security Events Monitoring | Mendeteksi perilaku anomali lewat pemantauan real-time |
| Incident Investigation & Forensics | Root cause analysis insiden berdasarkan jejak aktivitas |
| Troubleshooting | Diagnosis error sistem/aplikasi |
| Performance Monitoring | Insight performa aplikasi |
| Auditing & Compliance | Membangun jejak rekam (trail) untuk kebutuhan kepatuhan |

---

## 2. Jenis-Jenis Log

Log dipisahkan ke dalam beberapa kategori sesuai jenis informasi yang diberikan, sehingga investigasi bisa langsung fokus ke file log yang relevan tanpa harus menyisir seluruh log sistem.

**System Logs** — mencatat aktivitas operasi sistem operasi: startup/shutdown, driver loading, error sistem, hardware.

**Security Logs** — log terpenting dari sisi keamanan: autentikasi, otorisasi, perubahan kebijakan keamanan, perubahan akun pengguna, aktivitas abnormal.

**Application Logs** — mencatat aktivitas spesifik sebuah aplikasi (interaktif maupun non-interaktif): interaksi pengguna, perubahan aplikasi, update aplikasi, error aplikasi.

**Audit Logs** — detail perubahan sistem dan event pengguna, penting untuk compliance dan juga bisa mendukung pemantauan keamanan: akses data, perubahan sistem, aktivitas pengguna, penegakan kebijakan.

**Network Logs** — mencatat lalu lintas jaringan masuk/keluar, penting untuk troubleshooting jaringan dan investigasi insiden: traffic masuk/keluar, koneksi jaringan, log firewall.

**Access Logs** — detail akses ke berbagai resource (webserver, database, aplikasi, API).

Jenis log lain bisa bervariasi tergantung aplikasi dan layanan yang digunakan.

**Log Analysis** adalah teknik mengekstrak data berharga dari log dengan mencari tanda-tanda aktivitas abnormal. Karena volume log terlalu besar untuk diperiksa dengan mata telanjang, dibutuhkan teknik manual maupun otomatis.

---

## 3. Windows Event Logs Analysis

### 3.1 Kategori Log Windows

Windows OS mencatat aktivitas ke dalam beberapa file log terpisah:

**Application** — informasi terkait aplikasi yang berjalan: error, warning, masalah kompatibilitas.

**System** — informasi operasi sistem operasi itu sendiri: masalah driver, masalah hardware, startup/shutdown, informasi service.

**Security** — file log paling penting dari sisi keamanan: autentikasi pengguna, perubahan akun, perubahan kebijakan keamanan.

### 3.2 Event Viewer

**Event Viewer** adalah utilitas bawaan Windows dengan GUI untuk melihat dan mencari log, berbeda dari log jenis lain yang tidak punya aplikasi bawaan untuk melihatnya.

Cara membuka: klik Start Windows, ketik "Event Viewer".

Struktur navigasi Event Viewer:
- Panel kiri berisi kategori log (Windows Logs → Application, Security, Setup, System, Forwarded Events), Custom Views, Applications and Services Logs, Saved Logs, Subscriptions.
- Panel tengah menampilkan daftar event pada log yang dipilih.
- Panel kanan (Actions) berisi opsi analisis: Filter Current Log, Find, Save Filtered Log File As, Properties, dsb.

Setiap event bisa dibuka dengan double-click untuk melihat detailnya.

### 3.3 Struktur Sebuah Event Log

Field utama dalam sebuah Windows event log:

- **Description** — informasi rinci tentang aktivitas.
- **Log Name** — nama file log tempat event tercatat.
- **Logged** — waktu terjadinya aktivitas.
- **Event ID** — identifier unik untuk jenis aktivitas tertentu, digunakan untuk mencari aktivitas spesifik tanpa perlu membaca semua log satu per satu.

### 3.4 Event ID Penting

| Event ID | Arti |
|---|---|
| 4624 | User berhasil login |
| 4625 | User gagal login |
| 4634 | User berhasil logoff |
| 4720 | Akun user dibuat |
| 4722 | Akun user diaktifkan (enabled) |
| 4724 | Percobaan reset password akun |
| 4725 | Akun user dinonaktifkan (disabled) |
| 4726 | Akun user dihapus |
| 4672 | Special Logon — privilege khusus diberikan ke sesi logon (sering muncul berpasangan dengan 4624 dari proses SYSTEM/service background) |

Tidak perlu menghafal semua Event ID yang ada, cukup Event ID krusial saja.

### 3.5 Struktur Detail Event: Subject vs Target

Pada tab **Details → Friendly View**, sebuah event menunjukkan dua pihak:

- **Subject** — pelaku yang melakukan aksi (`SubjectUserName`, `SubjectUserSid`).
- **Target** — objek yang dikenai aksi (`TargetUserName`, `TargetSid`).

Contoh: pada event 4720 (account created), `TargetUserName` adalah nama akun baru yang dibuat, sedangkan `SubjectUserName` adalah akun yang melakukan pembuatan tersebut.

**SID berakhiran `-500`** selalu merujuk ke akun **Administrator bawaan (built-in)** di sistem Windows manapun — ini konsisten di semua instalasi Windows, terlepas dari nama komputernya.

Prinsip penting: ketika hasil filter Event ID menampilkan lebih dari satu event, jangan langsung asumsikan event teratas adalah yang relevan. Selalu cross-check `TargetUserName`/`TargetSid` di tab Details untuk memastikan event tersebut memang berkaitan dengan akun/objek yang sedang diselidiki, karena Event Viewer menampilkan event dari berbagai akun berbeda dalam satu hasil filter.

### 3.6 Filter Current Log

Fitur **Filter Current Log** (di panel Actions) digunakan untuk mencari event berdasarkan Event ID tertentu tanpa harus menyisir semua log.

Cara pakai: klik Filter Current Log → isi kolom "Includes/Excludes Event IDs" dengan nomor Event ID (contoh `4624`) → klik OK. Hasilnya langsung menampilkan hanya event dengan ID tersebut. Proses filter ini instan, tidak butuh waktu tunggu.

Untuk filter berdasarkan field yang lebih spesifik dan tidak tersedia di tab Filter biasa (misalnya **Logon Type**), gunakan tab **XML** pada jendela yang sama:
1. Centang **"Edit query manually"**.
2. Tulis query XPath custom, contoh untuk memfilter Event ID 4624 dengan Logon Type 3 (Network):

```xml
<QueryList>
  <Query Id="0" Path="Security">
    <Select Path="Security">
      *[System[EventID=4624]] and *[EventData[Data[@Name='LogonType']='3']]
    </Select>
  </Query>
</QueryList>
```

Angka pada `@Name='LogonType']='3'` bisa diganti sesuai Logon Type yang ingin dicari.

**Tabel Logon Type penting:**

| Logon Type | Arti |
|---|---|
| 2 | Interactive — login langsung di keyboard/layar |
| 3 | Network — akses lewat jaringan (misal share folder) |
| 4 | Batch |
| 5 | Service — proses background |
| 7 | Unlock — buka kunci layar |
| 10 | RemoteInteractive — RDP |

### 3.7 Log Rotation & Retention Policy

Setiap file log Windows punya batas ukuran maksimum (**Maximum log size**), dicek lewat klik kanan pada log → **Properties**. Ketika ukuran log mencapai batas, kebijakan retensi menentukan apa yang terjadi:

- **Overwrite events as needed (oldest events first)** — event terlama otomatis ditimpa oleh event baru (default paling umum).
- **Archive the log when full, do not overwrite events** — log diarsipkan, tidak ditimpa.
- **Do not overwrite events (clear logs manually)** — log berhenti mencatat sampai dibersihkan manual.

Implikasi penting: pada sistem dengan volume event tinggi (misalnya noise dari antivirus/Windows Defender berupa Event ID 4658, 4656, 4690 yang jumlahnya bisa ratusan ribu), event lama yang relevan untuk investigasi (misalnya bukti pembuatan akun) bisa **hilang karena tertimpa (overwritten)** sebelum sempat dianalisis. Ini adalah keterbatasan nyata dalam forensik log berbasis retensi otomatis.

---

## 4. Web Server Access Logs Analysis

### 4.1 Struktur Access Log

Access log mencatat semua request yang dibuat ke sebuah website. Contoh lokasi file pada Apache: `/var/log/apache2/access.log`.

Field-field dalam satu baris access log:

- **IP Address** — alamat IP pembuat request. Contoh: `172.16.0.1`.
- **Timestamp** — waktu request dibuat. Format: `[06/Jun/2024:13:58:44]`.
- **Request** — terdiri dari:
  - **HTTP Method** (contoh: `GET`) — aksi yang diminta terhadap resource.
  - **URL** (contoh: `/`) — resource yang diminta.
- **Status Code** (contoh: `200`) — hasil respons server terhadap request tersebut.
- **User-Agent** — informasi OS, browser, dan perangkat pembuat request.

### 4.2 Tools Command-Line untuk Analisis Log Manual

**`cat`** — menampilkan isi file log secara penuh sekaligus.

```bash
cat access.log
```

Juga bisa dipakai menggabungkan beberapa file log hasil rotasi menjadi satu file:

```bash
cat access1.log access2.log > combined_access.log
```

**`grep`** — mencari string/pola tertentu di dalam file log dan menampilkan hanya baris yang cocok.

```bash
grep "192.168.1.1" access.log
```

`grep` bisa di-chain (pipe) untuk mempersempit hasil pencarian dengan lebih dari satu kriteria sekaligus, contoh mencari POST request dari IP tertentu:

```bash
grep "POST" access.log | grep "172.16.0.1"
```

**`less`** — menampilkan file log satu halaman per satu waktu, berguna untuk file besar atau multi-file.

```bash
less access.log
```

Navigasi dalam `less`:
- `spacebar` — pindah ke halaman berikutnya.
- `b` — kembali ke halaman sebelumnya.
- `/pattern` lalu Enter — mencari pola tertentu di dalam file.
- `n` — pindah ke kemunculan berikutnya dari hasil pencarian.
- `N` — pindah ke kemunculan sebelumnya dari hasil pencarian.

### 4.3 Prinsip Analisis Manual: Jangan Asumsi Urutan

Ketika hasil `grep` menampilkan banyak baris, urutan tampilan **tidak otomatis berarti urutan kronologis (terbaru ke terlama)**. Timestamp pada setiap baris harus dibandingkan secara manual satu per satu (tanggal dulu, baru jam) untuk memastikan mana yang benar-benar paling baru/terlama, karena grep hanya menampilkan baris yang cocok pola sesuai urutan asli di file, bukan hasil yang sudah di-sort berdasarkan waktu.

---

## 5. Glossary

**Log** — jejak digital dari sebuah aktivitas (normal atau malicious) yang tercatat dalam file oleh sistem/aplikasi.

**Log Analysis** — teknik mengekstrak data/pola aktivitas abnormal dari log, baik manual maupun otomatis.

**Event Viewer** — aplikasi GUI bawaan Windows untuk melihat, mencari, dan memfilter log sistem.

**Event ID** — identifier unik numerik untuk jenis aktivitas tertentu dalam Windows event log.

**Logon Type** — kode numerik yang menjelaskan cara sebuah sesi logon terjadi (interactive, network, service, RDP, dll).

**Subject** — dalam struktur event Windows, pihak yang melakukan sebuah aksi.

**Target** — dalam struktur event Windows, objek/akun yang dikenai aksi tersebut.

**SID (Security Identifier)** — identifier unik untuk akun/grup di Windows; RID `-500` selalu merujuk ke akun Administrator built-in.

**Retention Policy** — kebijakan yang menentukan apa yang terjadi pada log lama ketika ukuran maksimum log tercapai (overwrite, archive, atau manual clear).

**Access Log** — file log yang mencatat semua request HTTP ke sebuah web server, mencakup IP, timestamp, method, URL, status code, dan user-agent.

**Status Code** — kode numerik HTTP yang menunjukkan hasil respons server terhadap sebuah request (contoh: 200 sukses, 404 not found, 500 server error).

**Log Rotation** — praktik memecah log menjadi file-file terpisah berdasarkan rentang waktu tertentu, alih-alih menyimpan semua dalam satu file besar.

---

## 6. Tools & Platform Rujukan

Room ini tidak menyebutkan tools/platform eksternal pihak ketiga (seperti Talos, VirusTotal, Shodan, AbuseIPDB, dsb). Seluruh analisis dalam room ini dilakukan menggunakan tools bawaan sistem:

- **Event Viewer** — aplikasi bawaan Windows OS untuk analisis Windows Event Logs.
- **cat, grep, less** — utility command-line bawaan Linux untuk analisis Web Server Access Logs.

---

## 7. Catatan Ringkas untuk Ditulis Tangan

**Konsep Dasar**
- Log — jejak digital aktivitas (normal/malicious)
- Log Analysis — ekstrak info berharga dari log, manual/otomatis
- Use case log: security monitoring, forensik insiden, troubleshooting, performance, audit/compliance

**Jenis Log**
- System — startup/shutdown, driver, hardware
- Security — autentikasi, akun, kebijakan (paling penting)
- Application — error, update, interaksi app
- Audit — perubahan sistem, compliance
- Network — traffic masuk/keluar, firewall
- Access — akses resource (web, db, API)

**Windows Event Viewer**
- Buka: Start → ketik "Event Viewer"
- Windows Logs → Application / Security / Setup / System / Forwarded Events
- Field event: Description, Log Name, Logged, Event ID

**Event ID Kunci**
- 4624 — login sukses
- 4625 — login gagal
- 4634 — logoff sukses
- 4672 — special logon (privilege, sering bareng 4624 dari SYSTEM/service)
- 4720 — akun dibuat
- 4722 — akun di-enable
- 4724 — reset password
- 4725 — akun disabled
- 4726 — akun dihapus

**Logon Type**
- 2 — Interactive
- 3 — Network
- 4 — Batch
- 5 — Service
- 7 — Unlock
- 10 — RemoteInteractive (RDP)

**Subject vs Target**
- Subject — pelaku aksi (SubjectUserName/Sid)
- Target — objek kena aksi (TargetUserName/Sid)
- SID akhiran -500 — selalu Administrator built-in

**Filter Current Log**
- Filter tab: isi Event ID langsung → OK, hasil instan
- XML tab: centang "Edit query manually" untuk filter field custom (misal LogonType)
- Query XPath: *[System[EventID=xxxx]] and *[EventData[Data[@Name='LogonType']='x']]

**Retention Policy**
- Cek: klik kanan log → Properties → Maximum log size
- Overwrite as needed (oldest first) — default, event lama bisa hilang
- Archive when full — tidak overwrite
- Do not overwrite (clear manually)
- Implikasi: bukti lama bisa ke-timpa noise (AV scan dll: 4658, 4656, 4690)

**Web Access Log Fields**
- IP Address, Timestamp [dd/Mon/yyyy:hh:mm:ss], Method+URL, Status Code, User-Agent

**Command Linux**
- cat access.log — tampilkan isi file
- cat file1 file2 > combined.log — gabung log
- grep "pattern" access.log — cari baris cocok pola
- grep "A" file | grep "B" — chain, filter ganda
- less access.log — baca per halaman (spacebar next, b back, /pattern cari, n/N next/prev match)

**Prinsip Penting**
- Hasil grep TIDAK otomatis terurut waktu — bandingkan timestamp manual
- Hasil filter Event ID bisa multi-akun — selalu cross-check Target di tab Details, jangan asal ambil teratas
