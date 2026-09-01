# Resume: Introduction to SIEM
**Platform:** TryHackMe
**Tanggal Penyelesaian:** 2026
**Status:** 100% Completed

---

## 1. Definisi & Peran SIEM

**SIEM** (Security Information and Event Management) adalah solusi keamanan inti yang digunakan oleh **SOC Analyst** (Security Operations Center Analyst) untuk memantau aktivitas di seluruh jaringan.

Fungsi utama SIEM mencakup empat hal:

- Mengumpulkan log dari berbagai sumber (**collect**)
- Menyeragamkan format log menjadi satu format konsisten (**normalize**)
- Menghubungkan log-log dari berbagai sumber untuk menemukan pola (**correlate**)
- Mendeteksi aktivitas mencurigakan menggunakan aturan deteksi (**detect**)

Siklus kerja SIEM digambarkan dalam empat tahap berurutan: collect data from sources → aggregate data → discover and detect threats → identify breaches and investigate alerts.


## 2. Jenis-Jenis Log Source

Setiap perangkat dalam jaringan menghasilkan log setiap kali ada aktivitas. Perangkat penghasil log disebut **log source**. Log source dibagi menjadi dua kategori utama.

### 2.1 Host-Centric Log Sources

Log yang berasal dari aktivitas di dalam host itu sendiri. Contoh perangkat: Windows, Linux, server.

Contoh event yang dicatat:

- Pengguna mengakses sebuah file
- Percobaan autentikasi (login)
- Eksekusi sebuah proses
- Penambahan/perubahan/penghapusan registry key atau value (Windows)
- Eksekusi PowerShell

### 2.2 Network-Centric Log Sources

Log yang dihasilkan ketika host berkomunikasi satu sama lain atau mengakses internet. Contoh perangkat: firewall, IDS/IPS, router.

Contoh event yang dicatat:

- Koneksi SSH
- Akses file via FTP
- Traffic web
- Akses resource perusahaan via VPN
- Aktivitas network file sharing


## 3. Tantangan Tanpa SIEM

Mengelola log tanpa SIEM menghadirkan lima hambatan utama bagi analis.

**Numerous Log Sources** — Sebuah jaringan menghasilkan ratusan event per detik dari banyak perangkat; memeriksa satu per satu sangat tidak praktis.

**No Centralization** — Log tersimpan di masing-masing mesin sehingga analis harus terhubung ke setiap perangkat secara individual (via SSH, RDP, dll) untuk menganalisis.

**Limited Context** — Log individual tidak menceritakan keseluruhan cerita. Sebuah event yang terlihat normal bisa jadi berbahaya ketika dikorelasikan dengan log dari sumber lain.

**Limited Analysis** — Volume log terlalu besar untuk dianalisis secara manual; analis pasti akan melewatkan banyak event penting.

**Format Issues** — Setiap log source menggunakan format yang berbeda-beda, membuat analisis lintas sumber menjadi sangat sulit.


## 4. Fitur-Fitur SIEM

### 4.1 Centralized Log Collection

SIEM menarik log dari semua sumber (endpoint, server, firewall, dll) melalui **lightweight agent** atau **API**, lalu memusatkannya di satu tempat. Analis tidak perlu lagi mengakses setiap mesin secara individual.

### 4.2 Normalization of Logs

Raw log memiliki format berbeda-beda antar platform. SIEM melakukan dua proses:

- **Parsing** — memecah satu entri log menjadi beberapa field terstruktur
- **Normalization** — mengonversi semua log dari berbagai sumber ke dalam satu format yang konsisten dan seragam

### 4.3 Correlation of Logs

SIEM menghubungkan log dari berbagai sumber untuk menemukan hubungan antar event. Contoh: akses file yang terlihat normal bisa terungkap sebagai eksfiltrasi data ketika dikorelasikan dengan log login VPN dari IP baru dan eksekusi PowerShell dalam rentang waktu yang berdekatan.

### 4.4 Real-time Alerting

SIEM memantau kondisi dari **detection rules** yang telah dikonfigurasi. Ketika kondisi terpenuhi, alert ter-trigger dan analis diberi notifikasi. Rules bisa berupa default bawaan SIEM atau dibuat sendiri oleh analis.

### 4.5 Dashboards and Reporting

Dashboard merangkum hasil analisis dalam bentuk visual yang actionable. Contoh informasi yang biasanya tampil di dashboard:

- Alert Highlights
- System Notification
- Health Alert
- List of Failed Login Attempts
- Events Ingested Count
- Rules Triggered
- Top Domains Visited

Fitur lain yang tidak dibahas mendalam di room ini: integrasi threat intelligence feeds, extensive data retention, powerful searching capabilities.


## 5. Log Sources: Detail per Perangkat

### 5.1 Windows Machine

Windows mencatat setiap event melalui **Event Viewer**, sebuah tool bawaan yang dapat diakses dengan mengetik `Event Viewer` di search bar. Setiap jenis aktivitas log diberi **Event ID** unik untuk memudahkan pelacakan.

Log dari semua endpoint Windows diteruskan (forwarded) ke SIEM untuk monitoring terpusat.

Kategori log di Windows Logs (Event Viewer):

| Kategori | Tipe |
|---|---|
| Application | Administrative |
| Security | Administrative |
| Setup | Operational |
| System | Administrative |
| Forwarded Events | Operational |

### 5.2 Linux Machine

Linux menyimpan log di lokasi-lokasi file yang spesifik sesuai kategorinya:

- `/var/log/httpd` — HTTP request/response dan error log (web server Apache)
- `/var/log/cron` — Event terkait cron job (tugas terjadwal)
- `/var/log/auth.log` dan `/var/log/secure` — Log autentikasi (login, sudo, dll)
- `/var/log/kern` — Event terkait kernel

### 5.3 Web Server

Penting untuk memantau semua request dan response yang masuk/keluar dari web server guna mendeteksi potensi serangan web. Di Linux, log Apache tersimpan di `/var/log/apache` atau `/var/log/httpd`.

Contoh format satu baris Apache log:
```
192.168.21.200 - - [21/March/2022:10:17:10 -0300] "GET /cgi-bin/try/ HTTP/1.0" 200 3395 "-" "Mozilla/5.0 ..."
```

Field yang terkandung: IP sumber, timestamp, HTTP method + path, status code, ukuran response, User-Agent.


## 6. Metode Log Ingestion

Setiap SIEM memiliki cara tersendiri untuk menarik log masuk. Empat metode yang umum digunakan:

**Agent / Forwarder** — Tool ringan yang diinstal pada endpoint; dikonfigurasi untuk menangkap dan mengirim log ke server SIEM secara otomatis. Di Splunk, komponen ini disebut **Forwarder**.

**Syslog** — Protokol standar yang mengumpulkan data dari berbagai sistem (web server, database, dll) dan mengirimkannya secara real-time ke tujuan terpusat.

**Manual Upload** — Pengguna mengunggah file log secara offline untuk analisis cepat. Setelah di-ingest, data akan dinormalisasi dan tersedia untuk dianalisis. Didukung oleh platform seperti Splunk dan ELK.

**Port-Forwarding** — SIEM dikonfigurasi untuk mendengarkan (listen) pada port tertentu; endpoint kemudian meneruskan data log mereka ke port tersebut.


## 7. Detection Rules dan Alerting

### 7.1 Cara Kerja Detection Rule

Detection rules adalah ekspresi logika yang menentukan kondisi apa yang harus terpenuhi agar sebuah alert ter-trigger. Rules memantau nilai dari field-field tertentu dalam log yang sudah dinormalisasi — itulah mengapa normalisasi sangat penting sebelum log di-ingest.

Contoh kondisi yang bisa menjadi dasar rule:

- 5 kali login gagal dalam 10 detik → alert `Multiple Failed Login Attempts`
- Login berhasil setelah beberapa login gagal → alert `Successful Login After Multiple Login Attempts`
- Pengguna mencolokkan USB (jika USB dibatasi kebijakan perusahaan) → alert
- Outbound traffic > 25 MB → alert potensi data exfiltration

### 7.2 Contoh Pembuatan Rule (Use-Case Nyata)

**Use-Case 1: Event Log Cleared**

Penyerang sering menghapus log setelah eksploitasi untuk menghilangkan jejak. Windows mencatat tindakan ini dengan Event ID **104**.

```
Rule: IF Log_Source = WinEventLog AND EventID = 104
      THEN Trigger Alert "Event Log Cleared"
```

**Use-Case 2: Deteksi Command whoami**

Penyerang sering menjalankan `whoami` setelah privilege escalation untuk memverifikasi level akses yang diperoleh. Windows mencatat eksekusi proses baru dengan Event ID **4688**.

```
Rule: IF Log_Source = WinEventLog
      AND EventCode = 4688
      AND NewProcessName CONTAINS "whoami"
      THEN Trigger Alert "WHOAMI Command Execution DETECTED"
```

**Use-Case 3: Deteksi CryptoMiner (dari Lab)**

```
Rule: Alert "Potential CryptoMiner Activity"
      IF EventID = 4688
      AND Log_Source = WindowsEventLogs
      AND ProcessName = (*miner* OR *crypt*)
```

Rule ini menggunakan wildcard (`*`) untuk mencocokkan nama proses yang mengandung kata "miner" atau "crypt" di bagian mana pun dalam string nama proses.

### 7.3 Field Penting untuk Rule Building

Saat membangun rule, field-field berikut umumnya relevan untuk dipertimbangkan:

- **Log_Source** — dari mana log tersebut berasal
- **EventID / EventCode** — jenis aktivitas yang terjadi
- **NewProcessName** — nama proses baru yang dieksekusi
- **UserName** — pengguna yang melakukan aksi
- **HostName** — perangkat tempat kejadian berlangsung


## 8. Alert Investigation: Alur Kerja Analis

Setelah alert ter-trigger, analis mengikuti alur investigasi berikut:

1. Periksa event dan flow yang terkait dengan alert di dashboard
2. Cek rule mana yang terpicu dan kondisi apa yang terpenuhi
3. Tentukan apakah alert adalah **True Positive** atau **False Positive**
4. Ambil tindakan yang sesuai berdasarkan hasil investigasi

### 8.1 Hasil Investigasi dan Tindakan Lanjutan

**Jika False Positive:** Lakukan tuning pada rule untuk mencegah false positive serupa muncul kembali di masa depan.

**Jika True Positive:** Lakukan investigasi lebih lanjut, kemudian pilih tindakan yang sesuai:

- Hubungi pemilik aset (asset owner) untuk mengonfirmasi aktivitas
- Isolasi host yang terinfeksi (isolate the infected host)
- Blokir IP yang mencurigakan (block the suspicious IP)

### 8.2 Studi Kasus: Lab Introduction to SIEM

Berikut adalah rekonstruksi alur investigasi yang dilakukan pada lab Task 6:

**Temuan di Dashboard:**
Setelah aktivitas mencurigakan dipicu, muncul proses baru `cudominer.exe` (count: 1) berwarna merah di tabel Process Name, disertai alert "Potential CryptoMiner Activity Observed".

**Investigasi Event Log:**
Dengan menelusuri halaman events (`/events`) dan melakukan scroll horizontal, ditemukan baris dengan detail berikut:

- HostName: `HR_02`
- UserName: `Chris`
- ProcessName: `C:\Users\Chris\temp\cudominer.exe`
- Channel: `THM_SIEM` (berbeda dari baris lain yang bernilai `Windows`)

**Rule yang Terpicu:**
```
Alert "Potential CryptoMiner Activity"
IF EventID = 4688 AND Log_Source = WindowsEventLogs
AND ProcessName = (*miner* OR *crypt*)
```

Term yang match: `miner` (dari nama proses `cudominer.exe`)

**Verdict:** True Positive — aktivitas cryptomining nyata terkonfirmasi

**Action yang Diambil:** True positive and isolate the host

**Flag:** `THM{000_SIEM_INTRO}`


## 9. Glossary

**SIEM** — Security Information and Event Management; solusi keamanan terpusat untuk pengumpulan, normalisasi, korelasi, dan deteksi ancaman dari log.

**SOC** — Security Operations Center; pusat operasi keamanan tempat analis memantau dan merespons insiden keamanan.

**SOC Analyst** — Analis yang bekerja di SOC, menghabiskan sebagian besar waktu memonitor dashboard SIEM dan menginvestigasi alert.

**Log Source** — Perangkat atau sistem yang menghasilkan log (endpoint, server, firewall, router, dll).

**Host-Centric Log** — Log yang mencatat aktivitas di dalam host itu sendiri.

**Network-Centric Log** — Log yang mencatat aktivitas komunikasi antar perangkat dalam jaringan.

**Ingestion** — Proses memasukkan log dari berbagai sumber ke dalam sistem SIEM.

**Parsing** — Proses memecah satu entri log menjadi field-field terstruktur yang terpisah.

**Normalization** — Proses mengonversi log dari berbagai format berbeda menjadi satu format seragam dan konsisten.

**Correlation** — Proses menghubungkan event dari berbagai log source untuk menemukan pola atau hubungan yang menunjukkan aktivitas mencurigakan.

**Detection Rule** — Ekspresi logika yang mendefinisikan kondisi kapan sebuah alert harus ter-trigger.

**Alert** — Notifikasi yang muncul ketika kondisi dari sebuah detection rule terpenuhi.

**True Positive** — Alert yang valid; aktivitas mencurigakan yang terdeteksi memang benar-benar terjadi dan merupakan ancaman nyata.

**False Positive** — Alert yang tidak valid; aktivitas yang terdeteksi sebagai mencurigakan ternyata merupakan aktivitas normal/tidak berbahaya.

**Lateral Movement** — Teknik yang digunakan penyerang untuk bergerak dari satu mesin yang sudah dikompromikan ke mesin lain dalam jaringan yang sama.

**Data Exfiltration** — Pencurian data dari jaringan dengan cara mentransfer data sensitif keluar ke pihak yang tidak berwenang.

**Privilege Escalation** — Teknik untuk mendapatkan level akses yang lebih tinggi dari yang dimiliki saat ini di sebuah sistem.

**Post-Exploitation** — Fase setelah penyerang berhasil mengeksploitasi sebuah sistem, biasanya mencakup penghapusan jejak, lateral movement, dan eksfiltrasi data.

**Event ID** — Nomor identifikasi unik yang diberikan Windows untuk setiap jenis aktivitas yang dicatat dalam log.

**Event ID 104** — Event ID Windows yang dicatat setiap kali event log dihapus atau di-clear.

**Event ID 4688** — Event ID Windows yang dicatat setiap kali sebuah proses baru dibuat/dieksekusi.

**Forwarder** — Komponen agent ringan dari Splunk yang diinstal pada endpoint untuk mengumpulkan dan mengirimkan log ke Splunk server.

**Syslog** — Protokol standar untuk pengiriman log antar sistem secara real-time.

**IDS/IPS** — Intrusion Detection System / Intrusion Prevention System; sistem yang mendeteksi dan/atau mencegah aktivitas berbahaya di jaringan.

**CryptoMiner** — Program yang memanfaatkan sumber daya komputasi (CPU/GPU) korban tanpa izin untuk menambang mata uang kripto demi keuntungan penyerang.

**MITRE ATT&CK** — Framework yang mendokumentasikan taktik, teknik, dan prosedur (TTP) yang digunakan oleh penyerang nyata; digunakan SIEM untuk mengklasifikasikan ancaman.

**TTP** — Tactics, Techniques, and Procedures; cara-cara spesifik yang digunakan penyerang dalam melancarkan serangan.

**Wildcard** — Karakter khusus (biasanya `*`) dalam ekspresi pencarian atau rule yang mewakili satu atau lebih karakter sembarang; contoh `*miner*` cocok dengan string apa pun yang mengandung kata "miner".

**Asset Owner** — Pemilik atau penanggung jawab dari aset (perangkat/sistem) tertentu di dalam organisasi.


## 10. Tools & Platform Rujukan

**Event Viewer**
Fungsi: Tool bawaan Windows untuk melihat dan menelusuri log event yang dicatat oleh sistem operasi Windows.
Cara akses: Ketik `Event Viewer` di search bar Windows.

**Splunk**
Fungsi: Platform SIEM komersial yang banyak digunakan; mendukung ingestion via Forwarder, Syslog, Manual Upload, dan Port-Forwarding. Menyediakan dashboard, alerting, dan pencarian log yang powerful.
URL: https://www.splunk.com

**ELK Stack (Elastic Stack)**
Fungsi: Platform SIEM open-source yang terdiri dari Elasticsearch, Logstash, dan Kibana; mendukung ingestion data secara manual maupun real-time.
URL: https://www.elastic.co

Room lanjutan yang direkomendasikan oleh materi ini (tersedia di TryHackMe):

- Junior Security Analyst Intro
- Splunk: The Basics
- Incident Handling with Splunk
- Benign
- Investigating with Splunk
- Investigating with ELK
- ItsyBitsy


## 11. Catatan Ringkas untuk Ditulis Tangan

### Definisi Inti

- SIEM — collect + normalize + correlate + detect log dari seluruh jaringan
- SOC Analyst — analis yang monitor dashboard SIEM & investigasi alert
- Log Source — perangkat penghasil log (endpoint, server, firewall, router)

### Jenis Log Source

- Host-Centric — aktivitas dalam host (file access, login, proses, registry, PowerShell)
- Network-Centric — aktivitas antar perangkat (SSH, FTP, web traffic, VPN, file sharing)
- Perangkat host-centric: Windows, Linux, server
- Perangkat network-centric: firewall, IDS/IPS, router

### Masalah Tanpa SIEM

- Numerous Log Sources — terlalu banyak, ratusan event/detik
- No Centralization — harus cek satu-satu via SSH/RDP
- Limited Context — log satuan tidak ceritakan keseluruhan insiden
- Limited Analysis — terlalu banyak untuk dianalisis manual
- Format Issues — tiap sumber punya format berbeda

### Proses Ingestion Log ke SIEM

- Parsing — pecah log jadi field-field terstruktur
- Normalization — seragamkan semua log ke format tunggal

### Metode Ingestion

- Agent/Forwarder — tool ringan di endpoint (Splunk: Forwarder)
- Syslog — protokol real-time ke tujuan terpusat
- Manual Upload — upload offline (Splunk, ELK)
- Port-Forwarding — SIEM listen di port, endpoint kirim ke sana

### Lokasi Log Linux

- /var/log/httpd — HTTP request/response & error (Apache)
- /var/log/cron — cron job events
- /var/log/auth.log / /var/log/secure — autentikasi
- /var/log/kern — kernel events

### Event ID Windows Penting

- 104 — event log di-clear/dihapus
- 4688 — proses baru dieksekusi (Process Creation)

### Struktur Detection Rule

- Bentuk dasar: IF [kondisi field] AND [kondisi field] THEN Trigger Alert
- Wildcard: tanda * untuk cocokkan sebagian string (contoh: *miner*)
- Field umum: Log_Source, EventID, NewProcessName, UserName, HostName

### Contoh Rule Penting

- EventID=104 → "Event Log Cleared" (jejak dihapus penyerang)
- EventID=4688 + NewProcessName=whoami → "WHOAMI Command Execution DETECTED"
- EventID=4688 + ProcessName=(*miner* OR *crypt*) → "Potential CryptoMiner Activity"

### Alur Investigasi Alert

1. Cek event/flow yang terkait di dashboard
2. Identifikasi rule mana yang terpicu
3. Tentukan: True Positive atau False Positive
4. Ambil tindakan:
   - False Positive → tune the rule
   - True Positive → investigasi lanjut, isolate host, block IP, hubungi asset owner

### Istilah Penting

- True Positive — alert valid, ancaman nyata
- False Positive — alert tidak valid, aktivitas normal
- Lateral Movement — penyerang bergerak antar mesin dalam jaringan
- Data Exfiltration — pencurian data keluar jaringan
- Privilege Escalation — eskalasi hak akses ke level lebih tinggi
- Post-Exploitation — fase setelah eksploitasi berhasil
- CryptoMiner — program tambang kripto yang curi resource korban
- MITRE ATT&CK — framework klasifikasi TTP penyerang
- Asset Owner — penanggung jawab perangkat/sistem tertentu

### Fitur Dashboard SIEM (Contoh di Splunk)

- Alert Highlights, System Notification, Health Alert
- List of Failed Login Attempts, Events Ingested Count
- Rules Triggered, Top Domains Visited

### Studi Kasus Lab (Task 6)

- Proses mencurigakan: cudominer.exe
- User: Chris | HostName: HR_02
- Term match rule: miner (dari *miner*)
- Verdict: True Positive
- Action: Isolate the host
- Flag: THM{000_SIEM_INTRO}
