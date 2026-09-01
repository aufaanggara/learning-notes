# Resume Materi: Introduction to SOC
**Platform:** TryHackMe
**Tanggal Penyelesaian:** 1 September 2026

---

## 1. Definisi dan Tujuan SOC

**SOC** (Security Operations Center) adalah fasilitas khusus yang dioperasikan oleh tim keamanan terspesialisasi untuk memantau jaringan dan sumber daya organisasi secara terus-menerus, mengidentifikasi aktivitas mencurigakan, dan mencegah kerusakan akibat insiden keamanan.

Tim SOC beroperasi **24 jam sehari, 7 hari seminggu** (24/7) tanpa jeda.

Dua fungsi utama yang dijaga oleh SOC:
- **Detection** — mendeteksi ancaman, kerentanan, aktivitas tidak sah, pelanggaran kebijakan, dan penyusupan
- **Response** — merespons insiden yang terdeteksi, meminimalkan dampak, dan membantu proses pemulihan

Solusi keamanan yang dipakai SOC mengintegrasikan seluruh jaringan perusahaan agar dapat dipantau dari satu lokasi terpusat (**centralized monitoring**).


## 2. Empat Jenis Deteksi dalam SOC

2.1 **Detect Vulnerabilities**
Mendeteksi kelemahan (weakness) pada software atau sistem yang dapat dieksploitasi penyerang. Meskipun memperbaiki kerentanan tidak selalu menjadi tanggung jawab langsung SOC, kerentanan yang tidak diperbaiki tetap menurunkan postur keamanan seluruh organisasi.

2.2 **Detect Unauthorized Activity**
Mendeteksi aktivitas yang tidak memiliki izin, seperti penggunaan kredensial karyawan oleh pihak luar untuk login ke sistem. Clue seperti lokasi geografis login yang tidak wajar dapat membantu proses deteksi ini.

2.3 **Detect Policy Violations**
Mendeteksi pelanggaran terhadap **security policy** (aturan dan prosedur keamanan internal organisasi). Contoh pelanggaran: mengunduh file bajakan, mengirim data rahasia perusahaan melalui jalur yang tidak aman.

2.4 **Detect Intrusions**
Mendeteksi akses tidak sah ke sistem dan jaringan. Contoh: penyerang berhasil mengeksploitasi web application, atau pengguna mengunjungi situs berbahaya hingga komputernya terinfeksi.


## 3. Tiga Pilar SOC

SOC yang matang (mature) dibangun di atas tiga pilar yang harus berjalan bersamaan:

| Pilar | Peran |
|---|---|
| **People** | Tim manusia yang menganalisis, memutuskan, dan merespons |
| **Process** | Prosedur dan alur kerja yang diikuti tim dalam menangani insiden |
| **Technology** | Solusi-solusi keamanan yang mengotomatiskan deteksi dan respons |

Ketiga pilar ini harus ada secara bersamaan. Kekurangan di salah satu pilar akan melemahkan keseluruhan operasional SOC.


## 4. People — Struktur Tim SOC

Hierarki tim SOC dari atas ke bawah:

```
CISO
 └── SOC Manager
      ├── SOC Analyst Level 1
      ├── SOC Analyst Level 2
      ├── SOC Analyst Level 3
      ├── Security Engineer
      └── Detection Engineer
```

4.1 **SOC Analyst Level 1**
First responder untuk setiap deteksi. Melakukan **basic alert triage** untuk menentukan apakah suatu alert benar-benar berbahaya. Melaporkan deteksi melalui jalur yang sesuai.

4.2 **SOC Analyst Level 2**
Menangani deteksi yang memerlukan investigasi lebih dalam dari Level 1. Mengkorelasikan data dari berbagai log sources untuk analisis yang lebih akurat.

4.3 **SOC Analyst Level 3**
Profesional berpengalaman yang secara proaktif mencari **threat indicators** (indikator ancaman). Menangani deteksi dengan critical severity yang memerlukan respons mendetail: **containment** (isolasi dampak), **eradication** (penghapusan ancaman), dan **recovery** (pemulihan sistem).

4.4 **Security Engineer**
Bertanggung jawab men-deploy dan mengonfigurasi solusi-solusi keamanan yang dipakai oleh seluruh analis, serta memastikan operasionalnya berjalan lancar.

4.5 **Detection Engineer**
Membangun **detection rules** — logika yang ditanamkan dalam solusi keamanan untuk mendeteksi aktivitas berbahaya. Peran ini sering dirangkap oleh analis Level 2 dan Level 3, namun bisa juga diisi secara independen.

4.6 **SOC Manager**
Mengatur proses-proses yang diikuti tim SOC. Menjadi jembatan komunikasi antara tim SOC dengan **CISO** (Chief Information Security Officer) untuk melaporkan postur keamanan dan upaya-upaya tim.

> Catatan: jumlah dan jenis peran dalam tim SOC bisa bertambah atau berkurang tergantung ukuran dan tingkat kekritisan organisasi.


## 5. Process — Alur Kerja SOC

### 5.1 Alert Triage

Alert triage adalah proses dasar yang selalu dijalankan pertama kali saat ada alert masuk. Tujuannya: menentukan **severity** (tingkat keparahan) alert dan memprioritaskan respons.

Triage dilakukan dengan menjawab **5 Ws**:

| W | Pertanyaan |
|---|---|
| **What** | Aktivitas apa yang memicu alert? |
| **When** | Kapan aktivitas itu terjadi? |
| **Where** | Di mana (host/IP tujuan) aktivitas itu terjadi? |
| **Who** | Siapa (host/IP/user sumber) yang melakukannya? |
| **Why** | Mengapa aktivitas itu terjadi? Disengaja (intended) atau berbahaya (malicious)? |

Contoh penerapan 5 Ws pada alert nyata (dari lab praktik room ini):

- **Alert:** Port Scanning Activity Detected from IP: 10.0.0.8
- What: Port scanning activity
- When: June 12, 2024 17:24
- Where: 10.0.0.3 (JOE PC — destination host)
- Who: NESSUS (source host, IP 10.0.0.8)
- Why: Intended — vulnerability assessment team sudah memberi notifikasi ke SOC sebelumnya

### 5.2 Reporting

Alert berbahaya yang sudah dianalisis harus di-escalate ke analis level lebih tinggi dalam bentuk **ticket**. Laporan wajib mencakup:
- Jawaban seluruh 5 Ws
- Analisis menyeluruh (thorough analysis)
- Screenshot sebagai bukti (evidence) aktivitas

### 5.3 Incident Response dan Forensics

Jika deteksi mengarah ke aktivitas kritis, tim level tinggi akan memulai proses **incident response** (IR). Dalam beberapa kasus, diperlukan juga aktivitas **forensics** — menganalisis artifacts dari sistem atau jaringan untuk menemukan **root cause** (akar permasalahan) insiden.


## 6. Process — Klasifikasi Hasil Investigasi

Setelah investigasi selesai, analis menentukan apakah alert tersebut:

- **True Positive** — alert valid, benar-benar merupakan insiden keamanan nyata yang harus ditangani
- **False Positive** — alert muncul, tetapi setelah diselidiki ternyata bukan insiden; aktivitas yang memicu alert bersifat sah/diizinkan


## 7. Technology — Solusi Keamanan dalam SOC

### 7.1 SIEM (Security Information and Event Management)

Tool paling umum di hampir semua lingkungan SOC. Cara kerjanya:
- Mengumpulkan **logs** dari berbagai perangkat jaringan (**log sources**)
- Menjalankan **detection rules** yang berisi logika untuk mengidentifikasi aktivitas mencurigakan
- Mengkorelasikan data dari berbagai log sources dan mengeluarkan alert bila ada kecocokan dengan rules
- SIEM modern juga mendukung **user behavior analytics** dan **threat intelligence**, diperkuat dengan **machine learning**

> Penting: SIEM hanya menyediakan kemampuan **Detection**. Kemampuan respons otomatis bukan domain SIEM.

### 7.2 EDR (Endpoint Detection and Response)

Memberikan visibilitas real-time dan historis terhadap aktivitas di level **endpoint** (laptop, komputer pengguna, server). Kemampuan utama:
- Deteksi ancaman di endpoint secara mendalam
- Dapat melakukan **automated response** (respons otomatis)
- Memungkinkan investigasi detail dan respons cepat hanya dengan beberapa klik

### 7.3 Firewall

Berfungsi sebagai penghalang antara jaringan internal dan jaringan eksternal (internet). Cara kerjanya:
- Memantau traffic jaringan yang masuk dan keluar
- Memfilter traffic yang tidak sah (unauthorized)
- Menerapkan detection rules untuk memblokir traffic mencurigakan sebelum masuk ke jaringan internal

### 7.4 Solusi Keamanan Lainnya

Selain ketiga di atas, beberapa solusi lain yang umum dipakai dalam ekosistem SOC:

- **Antivirus** — mendeteksi dan menghapus malware di endpoint
- **EPP** (Endpoint Protection Platform) — perlindungan endpoint berbasis signature dan heuristik
- **IDS/IPS** (Intrusion Detection/Prevention System) — mendeteksi dan/atau mencegah penyusupan di jaringan
- **XDR** (Extended Detection and Response) — integrasi deteksi dan respons lintas endpoint, jaringan, dan cloud
- **SOAR** (Security Orchestration, Automation and Response) — mengotomatiskan dan mengorkestrasikan proses respons insiden

Pemilihan teknologi yang akan dipakai SOC ditentukan berdasarkan **threat surface** (luas permukaan ancaman) dan **sumber daya yang tersedia** di organisasi.


## 8. Ringkasan Praktik Lab (Task 6)

Skenario: Menerima alert port scanning activity dari IP 10.0.0.8 di SIEM. Berperan sebagai Level 1 Analyst.

Alur yang dijalankan:
1. Buka SIEM Alerts dashboard
2. Klik **Acknowledge** pada alert #167 untuk mengambil ownership
3. Masuk ke halaman investigasi SIEM Solution, baca log-log yang terkait
4. Jawab 5 Ws berdasarkan analisis log
5. Catat additional investigation notes (ada atau tidaknya respons balik dari target)
6. Klik **Complete Investigation**, pilih **True Positive** atau **False Positive**
7. Klik **Close Alert** untuk menutup investigasi dan mendapatkan flag

Hasil investigasi:
- Aktivitas adalah port scanning yang dilakukan oleh vulnerability assessment team (host NESSUS, IP 10.0.0.8) terhadap JOE PC (IP 10.0.0.3)
- Port yang direspons oleh target: port 22 (SSH)
- Kesimpulan: **False Positive** — bukan insiden, karena aktivitas sudah dinotifikasikan sebelumnya oleh tim yang berwenang
- Flag: `THM{000_INTRO_TO_SOC}`


## 9. Glossary

| Istilah | Definisi |
|---|---|
| **SOC** | Security Operations Center — tim dan fasilitas yang memantau keamanan organisasi 24/7 |
| **CISO** | Chief Information Security Officer — pimpinan tertinggi keamanan informasi dalam organisasi |
| **Alert Triage** | Proses penyortiran dan analisis awal sebuah alert untuk menentukan severity dan prioritasnya |
| **5 Ws** | Kerangka analisis: What, When, Where, Who, Why — digunakan dalam proses triage |
| **Log Sources** | Perangkat jaringan yang menghasilkan log dan mengirimkannya ke SIEM |
| **Detection Rules** | Logika yang dikonfigurasi dalam solusi keamanan untuk mengidentifikasi aktivitas mencurigakan |
| **True Positive** | Alert valid yang merupakan insiden keamanan nyata |
| **False Positive** | Alert yang ternyata bukan insiden setelah diselidiki; aktivitasnya sah/diizinkan |
| **Threat Surface** | Keseluruhan area yang berpotensi menjadi target atau titik masuk serangan |
| **Root Cause Analysis** | Investigasi untuk menemukan penyebab mendasar dari sebuah insiden |
| **Containment** | Tindakan mengisolasi dampak insiden agar tidak menyebar lebih luas |
| **Eradication** | Tindakan menghapus/menghilangkan ancaman dari sistem |
| **Recovery** | Tindakan memulihkan sistem ke kondisi normal setelah insiden ditangani |
| **Escalation/Escalate** | Meneruskan alert atau kasus ke analis yang levelnya lebih tinggi |
| **Ticket** | Laporan formal yang dibuat untuk mendokumentasikan dan menugaskan penanganan sebuah alert |
| **Vulnerability** | Kelemahan pada sistem atau software yang dapat dieksploitasi penyerang |
| **Threat Actor** | Pihak yang melakukan atau berpotensi melakukan serangan terhadap sistem |
| **Incident Response (IR)** | Proses terstruktur untuk menangani insiden keamanan yang sudah dikonfirmasi |
| **Forensics** | Analisis mendalam terhadap artifacts sistem atau jaringan untuk menemukan root cause insiden |
| **Port Scanning** | Teknik pemindaian untuk mengecek port-port yang terbuka/aktif pada sebuah host |
| **Artifacts** | Jejak digital (file, log, registry entry, dll) yang ditemukan selama investigasi forensik |
| **Security Policy** | Dokumen aturan dan prosedur yang dibuat untuk melindungi organisasi dari ancaman keamanan |
| **Compliance** | Kepatuhan terhadap aturan atau regulasi keamanan yang berlaku |
| **Threat Intelligence** | Informasi yang dikumpulkan dan dianalisis tentang ancaman yang ada atau yang mungkin terjadi |
| **User Behavior Analytics** | Analisis pola perilaku pengguna untuk mendeteksi anomali yang mengindikasikan ancaman |
| **SOAR** | Security Orchestration, Automation and Response — platform otomatisasi dan orkestrasi respons insiden |
| **XDR** | Extended Detection and Response — solusi deteksi dan respons lintas endpoint, jaringan, dan cloud |
| **EPP** | Endpoint Protection Platform — perlindungan endpoint berbasis signature dan heuristik |
| **IDS/IPS** | Intrusion Detection/Prevention System — sistem deteksi dan/atau pencegahan penyusupan |


## 10. Tools & Platform Rujukan

**SIEM**
Kategori tools, bukan satu produk tunggal. Contoh produk populer di industri: Splunk, IBM QRadar, Microsoft Sentinel. Digunakan untuk agregasi log dan deteksi berbasis rules.

**NESSUS**
Tools vulnerability scanner yang dipakai oleh vulnerability assessment team untuk melakukan port scanning dan pemindaian kerentanan dalam jaringan. Platform: tenable.com

**EDR (berbagai vendor)**
Contoh produk: CrowdStrike Falcon, Microsoft Defender for Endpoint, SentinelOne. Digunakan untuk visibilitas dan respons di level endpoint.

**IDS/IPS**
Contoh produk: Snort, Suricata. Digunakan untuk mendeteksi dan/atau mencegah penyusupan di level jaringan.


## 11. Catatan Ringkas untuk Ditulis Tangan

### Definisi Inti
- SOC — tim keamanan yang monitor jaringan 24/7, fokus deteksi & respons
- CISO — pimpinan keamanan informasi tertinggi, di atas SOC Manager
- Security Policy — aturan & prosedur internal untuk lindungi organisasi

### Tiga Pilar SOC
- People — manusia yang analisis & putuskan
- Process — prosedur yang diikuti
- Technology — tools keamanan yang dipakai

### Empat Jenis Deteksi
- Vulnerabilities — kelemahan sistem
- Unauthorized Activity — aktivitas tanpa izin
- Policy Violations — pelanggaran aturan internal
- Intrusions — akses tidak sah ke sistem/jaringan

### Struktur Tim (atas ke bawah)
- CISO → SOC Manager → Level 1 / Level 2 / Level 3 / Security Engineer / Detection Engineer
- Level 1 — triage awal, first responder
- Level 2 — investigasi lebih dalam, korelasi log
- Level 3 — proaktif cari threat, tangani critical severity
- Security Engineer — deploy & konfigurasi tools
- Detection Engineer — buat detection rules
- SOC Manager — atur proses, lapor ke CISO

### Alert Triage — 5 Ws
- What — aktivitas apa?
- When — kapan?
- Where — destination host/IP?
- Who — source host/IP/user?
- Why — intended (sah) atau malicious (jahat)?

### Alur Proses SOC
1. Alert masuk → triage (5 Ws)
2. Tentukan severity & prioritas
3. Buat ticket → escalate ke level lebih tinggi
4. Incident response (jika kritis)
5. Forensics jika perlu → cari root cause
6. Containment → Eradication → Recovery

### Klasifikasi Hasil Investigasi
- True Positive — insiden nyata, harus ditangani
- False Positive — bukan insiden, aktivitas sah

### Teknologi Utama
- SIEM — kumpulkan log, jalankan detection rules, beri alert (hanya Detection)
- EDR — visibilitas & automated response di level endpoint
- Firewall — filter traffic masuk/keluar, blokir yang tidak sah
- IDS/IPS — deteksi/cegah penyusupan di jaringan
- XDR — deteksi & respons lintas endpoint + jaringan + cloud
- SOAR — otomatisasi & orkestrasi respons insiden
- EPP — perlindungan endpoint berbasis signature

### Istilah Kunci Lainnya
- Log Sources — perangkat yang kirim log ke SIEM
- Detection Rules — logika di dalam tools untuk tandai aktivitas mencurigakan
- Artifacts — jejak digital (file, log) yang dianalisis saat forensik
- Root Cause Analysis — investigasi untuk cari penyebab utama insiden
- Threat Surface — seluruh area yang berpotensi diserang
- Containment — isolasi dampak insiden
- Eradication — hapus ancaman dari sistem
- Recovery — pulihkan sistem ke kondisi normal

### Flag Lab Praktik
- THM{000_INTRO_TO_SOC}
- Kasus: port scan dari NESSUS (10.0.0.8) ke JOE PC (10.0.0.3), June 12 2024 17:24
- Kesimpulan: False Positive (vulnerability assessment team sudah notifikasi SOC)
