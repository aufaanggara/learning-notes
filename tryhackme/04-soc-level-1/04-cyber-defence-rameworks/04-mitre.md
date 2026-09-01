# Resume Room: MITRE (ATT&CK, CAR, D3FEND, dan Framework Terkait)

**Tanggal selesai:** 1 September 2026

---

## 1. Pengantar MITRE

MITRE adalah organisasi nirlaba yang melakukan riset dan pengembangan di berbagai domain (cyber security, artificial intelligence, healthcare, space systems) dengan misi "menyelesaikan masalah demi dunia yang lebih aman". Di ranah cyber security, MITRE mengembangkan beberapa framework yang saling melengkapi: ATT&CK (memahami serangan), CAR (mendeteksi serangan), D3FEND (mempertahankan sistem), serta beberapa framework turunan/emerging (AADAPT, ATLAS).

---

## 2. MITRE ATT&CK Framework

### 2.1 Definisi

ATT&CK (Adversarial Tactics, Techniques & Common Knowledge) adalah knowledge base global tentang tactic dan technique adversary berdasarkan observasi dunia nyata. Framework ini lahir tahun 2013 dari kebutuhan mendokumentasikan TTP (Tactics, Techniques, Procedures) yang dipakai kelompok APT (Advanced Persistent Threat).

### 2.2 Struktur TTP

- **Tactic**: tujuan/objektif adversary — jawaban dari "mengapa" serangan dilakukan.
- **Technique**: cara adversary mencapai tujuannya — jawaban dari "bagaimana".
- **Sub-technique**: metode yang lebih spesifik dari sebuah technique.
- **Procedure**: implementasi nyata/detail eksekusi technique tersebut.

Contoh rantai: Tactic **Reconnaissance** → Technique **Active Scanning** → Sub-technique **Scanning IP Blocks / Vulnerability Scanning / Wordlist Scanning**.

### 2.3 Evolusi dan Cakupan

ATT&CK awalnya fokus pada Windows, lalu berkembang mencakup macOS, Linux, cloud platform (di bawah **Enterprise Matrix**). Selain itu ada matrix khusus:

- **Enterprise**: IT tradisional/korporat (Windows, Linux, macOS, cloud, SaaS, jaringan kantor).
- **Mobile**: platform perangkat mobile.
- **ICS (Industrial Control Systems)**: sistem kontrol industri seperti pabrik, SCADA, PLC, infrastruktur kritikal (listrik, air).

Setiap halaman technique menyediakan: ID unik, deskripsi, sub-technique, tactic terkait, platform, contoh prosedur (grup/software/campaign yang menggunakannya), mitigasi, deteksi, dan referensi.

### 2.4 ATT&CK Matrix dan Navigator

**ATT&CK Matrix** adalah representasi visual seluruh tactic (kolom) dan technique (baris di bawah tiap tactic) dalam framework. **ATT&CK Navigator** adalah tool untuk memberi anotasi dan mengeksplorasi matrix ini secara interaktif — biasa dipakai untuk memvisualisasikan technique yang dipetakan ke suatu grup/software/campaign tertentu (menggunakan highlight warna).

### 2.5 Kegunaan ATT&CK dan Penggunanya

ATT&CK memberikan bahasa standar dan ID unik sehingga memudahkan perbandingan data/insiden lintas organisasi, serta menjembatani gap antara threat intelligence (apa yang dilakukan attacker) dan operasi defensif (bagaimana mendeteksinya).

Pengguna utama ATT&CK di industri:

- **CTI (Cyber Threat Intelligence) Teams** — memetakan perilaku threat actor ke TTP untuk membuat profil ancaman yang actionable.
- **SOC Analysts** — menghubungkan alert ke tactic/technique untuk konteks dan prioritas insiden.
- **Detection Engineers** — memetakan rule SIEM/EDR ke ATT&CK untuk memastikan cakupan deteksi.
- **Incident Responders** — memetakan timeline insiden ke tactic/technique untuk visualisasi serangan.
- **Red & Purple Teams** — membangun rencana emulasi dan exercise yang selaras dengan technique grup nyata.

### 2.6 Alur Kerja Praktis (CTI Report ke ATT&CK Matrix)

Proses menerjemahkan laporan CTI (bahasa bebas) menjadi ID resmi ATT&CK: baca temuan di laporan (misal "reconnaissance", "initial access") lalu cocokkan dengan technique ID yang sesuai di matrix (misal T1595, T1190). Ini adalah skill inti threat intelligence: mengubah narasi insiden menjadi data terstruktur yang bisa dibandingkan dan ditindaklanjuti.

### 2.7 Kapan ATT&CK Dipakai di Real Life

ATT&CK tidak dipakai saat serangan sedang berlangsung real-time, melainkan pada momen:

1. **Sebelum insiden (proaktif)** — riset grup APT yang menargetkan sektor organisasi, untuk menyiapkan deteksi/pertahanan lebih dulu.
2. **Setelah insiden (forensik)** — menyusun kronologi serangan dan memetakan tiap langkah ke ID ATT&CK untuk laporan standar.
3. **Detection engineering** — mengecek apakah semua technique umum sudah punya deteksi (mencari gap).
4. **Red team exercise** — mensimulasikan gaya serangan grup tertentu untuk menguji pertahanan sendiri.

---

## 3. Cyber Analytics Repository (CAR)

### 3.1 Definisi

CAR adalah knowledge base analitik deteksi yang dikembangkan berdasarkan adversary model ATT&CK. CAR menyediakan analitik siap pakai (pseudocode dan implementasi ke tools spesifik seperti Splunk, EQL) yang tervalidasi dan dijelaskan operating theory-nya, sehingga defender bisa menerjemahkan ATT&CK TTP menjadi deteksi nyata di SIEM.

### 3.2 Struktur Halaman CAR

Setiap analitik CAR memiliki:

- **ID dan judul** (contoh: CAR-2020-09-001: Scheduled Task - File Access).
- **Deskripsi** perilaku adversary yang dideteksi.
- Info: Submission Date, Update Date, Information Domain, Data Subtypes, **Analytic Type** (misal Situational Awareness), Applicable Platforms, Contributors.
- Tabel **ATT&CK Detections**: Technique, Sub-technique(s), Tactic(s), Level of Coverage (Low/Moderate/High).
- Section **D3FEND Techniques** terkait (menghubungkan CAR ke D3FEND).
- Section **Implementations**: berisi Pseudocode dan query nyata (Splunk, LogPoint), kadang disertai Unit Tests untuk validasi analitik.

### 3.3 Konsep Pseudocode

Pseudocode adalah representasi instruksi/algoritma dalam bahasa yang mudah dibaca manusia, sebelum diterjemahkan ke query teknis spesifik tool (Splunk/LogPoint/EQL).

Contoh pseudocode CAR untuk deteksi pembuatan scheduled task:

```
files = search File:Create
task_files = filter files where (
   (file_path = "C:\Windows\System32\Tasks\*" or file_path = "C:\Windows\Tasks\*") and
   image_path != "C:\WINDOWS\system32\svchost.exe")
output task_files
```

Implementasi Splunk-nya:

```
index=__your_sysmon_index__ EventCode=11 Image!="C:\\WINDOWS\\system32\\svchost.exe" (TargetFilename="C:\\Windows\\System32\\Tasks\\*" OR TargetFilename="C:\\Windows\\Tasks\\*")
```

### 3.4 CAR Navigator

CAR juga memiliki ATT&CK Navigator layer sendiri, memetakan technique-technique yang sudah punya analitik CAR ke dalam matrix, mirip fungsinya dengan Navigator layer group yang dibahas di ATT&CK.

---

## 4. MITRE D3FEND Framework

### 4.1 Definisi

D3FEND (Detection, Denial, and Disruption Framework Empowering Network Defense) adalah framework yang memetakan defensive technique dan menetapkan bahasa umum untuk mendeskripsikan cara kerja kontrol keamanan. Jika ATT&CK menjelaskan bagaimana serangan terjadi, D3FEND menjelaskan bagaimana cara menghentikannya.

### 4.2 Tujuh Tactic D3FEND

D3FEND memiliki matrix sendiri dengan tujuh tactic:

1. **Model** — memodelkan sistem/aset untuk keperluan defense.
2. **Harden** — memperkuat sistem agar sulit diserang.
3. **Detect** — mendeteksi aktivitas mencurigakan.
4. **Isolate** — mengisolasi ancaman dari sistem lain.
5. **Deceive** — menyesatkan/menjebak attacker.
6. **Evict** — mengusir attacker dari sistem.
7. **Restore** — memulihkan sistem pasca insiden.

### 4.3 Struktur Halaman Technique D3FEND

Setiap technique D3FEND memiliki ID (format D3-XXX), Definition (definisi), How it works (cara kerja), Considerations (pertimbangan implementasi, termasuk potensi false positive), dan **Artifact Relationships** — diagram yang menunjukkan hubungan technique tersebut dengan digital artifact tertentu (misal Password, Credential, Certificate, Network Traffic) beserta jenis relasinya (use-limits, regenerates, hardens, analyzes).

Contoh: **Credential Rotation (D3-CRO)** — merotasi password/API key/sertifikat secara berkala untuk meminimalkan risiko akses tidak sah dari kredensial yang dicuri. Terhubung ke artifact Password (use-limits), Credential (regenerates, hardens), Certificate (regenerates).

Contoh lain: **User Geolocation Logon Pattern Analysis (D3-UGLPA)** — sub-technique dari User Behavior Analysis; memonitor data geolocation logon attempt dan membandingkannya dengan baseline perilaku user untuk mendeteksi anomali (logon dari lokasi tak biasa, lokasi tanpa user terdaftar, atau logon yang secara fisik tidak mungkin terjadi dalam rentang waktu tertentu). Artifact yang dianalisis: **Network Traffic**.

### 4.4 Hubungan ATT&CK dan D3FEND dalam Praktik

Alur kerja umum defender: ATT&CK dipakai lebih dulu untuk **identifikasi** (memberi nama/label resmi pada perilaku serangan yang teramati, misal T1053 Scheduled Task/Job), lalu D3FEND dipakai untuk **rekomendasi mitigasi/response** (memilih defensive technique yang menutup celah tersebut). Ibarat ATT&CK adalah diagnosis, D3FEND adalah resepnya.

Dalam konteks investigasi insiden yang lebih luas (contoh: platform blue team seperti LetsDefend), framework yang biasa dipakai berurutan:

1. **Cyber Kill Chain** — gambaran besar fase serangan (Reconnaissance sampai Actions on Objectives).
2. **ATT&CK** — detail technique spesifik yang teramati.
3. **D3FEND** — rekomendasi defensive technique untuk menutup celah.
4. **NIST Incident Response Lifecycle** (Preparation, Detection & Analysis, Containment/Eradication/Recovery, Post-Incident Activity) — kerangka proses investigasi insiden secara keseluruhan.
5. **IOC (Indicator of Compromise)** — data teknis (hash, IP, domain) yang dicatat terpisah sebagai bukti forensik.

---

## 5. Studi Kasus: Mustang Panda (G0129)

Digunakan sebagai contoh threat group nyata untuk latihan pemetaan TTP di Task 3.

- **Asal**: China-based cyber espionage threat actor, aktif sejak minimal 2012.
- **Target**: entitas pemerintah, non-profit, dan NGO (banyak di Asia Tenggara).
- **Pola serangan (TTP khas)**:
  - Initial Access: **Phishing** (T1566), khususnya sub-technique Spearphishing Link (T1598.003 pada konteks reconnaissance/T1566.002 pada initial access — perlu dicek konteks tactic-nya).
  - Persistence: **Scheduled Task/Job**.
  - Defense Evasion: **Obfuscated Files or Information**.
  - Command and Control: **Ingress Tool Transfer**.
- Data ini didapat dari halaman Group resmi ATT&CK dan divisualisasikan lewat ATT&CK Navigator layer khusus grup tersebut.

---

## 6. Studi Kasus: APT33 (G0064)

Digunakan sebagai skenario CTI untuk sektor aviasi di Task 4.

- **Asal**: suspected Iranian threat group, aktif sejak minimal 2013.
- **Target**: organisasi di Amerika Serikat, Arab Saudi, Korea Selatan, dengan minat khusus pada sektor **aviasi** dan **energi**.
- **Technique kunci**: **Valid Accounts: Cloud Accounts (T1078.004)** — APT33 menggunakan akun Office 365 yang dikompromikan bersamaan dengan tool **Ruler (S0358)** untuk mendapatkan kontrol endpoint.
- **Mitigasi terkait**: **M1018 User Account Management** — meninjau dan menghapus akun yang tidak aktif/tidak diperlukan secara berkala.
- **Detection Strategy terkait**: **DET0546 — Detection of Abused or Compromised Cloud Accounts for Access and Persistence**, dengan beberapa Analytic ID turunan (AN1503–AN1506) yang mendeteksi anomali autentikasi, penggunaan API di luar scope normal, aktivitas cloud productivity tools yang tidak biasa, dan pola login yang menyimpang dari profil pengguna Microsoft 365/Google Workspace.

---

## 7. Framework dan Project Lain dari MITRE

### 7.1 Adversary Emulation Library

Resource gratis (dikelola Center for Threat Informed Defense/CTID) berisi rencana emulasi (emulation plan) langkah demi langkah untuk meniru serangan nyata dari grup threat tertentu.

### 7.2 Caldera

Tool automated adversary emulation berbasis ATT&CK. Memungkinkan simulasi perilaku attacker dunia nyata secara otomatis, sehingga defender bisa mengevaluasi metode deteksi dan berlatih incident response dalam environment terkontrol. Mendukung baik operasi red team maupun blue team.

### 7.3 AADAPT (Adversarial Actions in Digital Asset Payment Technologies)

Knowledge base dengan matrix sendiri, mengikuti struktur mirip ATT&CK, fokus pada tactic dan technique adversary yang menargetkan sistem manajemen aset digital: jaringan blockchain, smart contract, dompet digital (digital wallets), dan teknologi aset digital lainnya.

### 7.4 ATLAS (Adversarial Threat Landscape for Artificial-Intelligence Systems)

Knowledge base dan framework dengan matrix sendiri, fokus pada ancaman yang menargetkan sistem artificial intelligence dan machine learning. Mendokumentasikan teknik serangan nyata, vulnerability, dan mitigasi spesifik teknologi AI.

---

## 8. Glossary Istilah Kunci

**ATT&CK** — Adversarial Tactics, Techniques & Common Knowledge; knowledge base global TTP adversary.

**Tactic** — tujuan/objektif adversary (kenapa).

**Technique** — cara adversary mencapai tujuan (bagaimana).

**Sub-technique** — metode spesifik dari sebuah technique.

**Procedure** — implementasi nyata dari technique oleh grup/malware tertentu.

**TTP** — Tactics, Techniques, Procedures.

**APT** — Advanced Persistent Threat; kelompok penyerang canggih dan persisten.

**Enterprise Matrix** — domain ATT&CK untuk IT tradisional/korporat.

**ICS Matrix** — domain ATT&CK untuk Industrial Control Systems.

**ATT&CK Navigator** — tool visualisasi dan anotasi matrix ATT&CK.

**CTI** — Cyber Threat Intelligence; disiplin mengumpulkan dan menganalisis informasi ancaman.

**CAR** — Cyber Analytics Repository; kumpulan analitik deteksi siap pakai berbasis ATT&CK.

**Pseudocode** — representasi instruksi/algoritma dalam bahasa mudah dibaca manusia.

**D3FEND** — Detection, Denial, and Disruption Framework Empowering Network Defense; framework defensive technique.

**Digital Artifact** — objek/data digital (password, credential, network traffic, dsb) yang menjadi target analisis/defense technique.

**Emulation Plan** — panduan langkah demi langkah meniru perilaku grup threat tertentu.

**Caldera** — tool automated adversary emulation berbasis ATT&CK.

**AADAPT** — framework MITRE untuk ancaman pada sistem aset digital (blockchain, wallet, smart contract).

**ATLAS** — framework MITRE untuk ancaman pada sistem AI/machine learning.

**IOC** — Indicator of Compromise; data teknis (hash, IP, domain) bukti insiden.

**SIEM** — Security Information and Event Management; platform pengumpulan dan analisis log keamanan.

**SOC** — Security Operations Center; tim yang memonitor dan merespons alert keamanan.

---

## 9. Tools & Platform Rujukan

- **MITRE ATT&CK** — knowledge base TTP adversary dan matrix visualnya. https://attack.mitre.org
- **ATT&CK Navigator** — visualisasi dan anotasi matrix ATT&CK/CAR. https://mitre-attack.github.io/attack-navigator/
- **MITRE CAR (Cyber Analytics Repository)** — analitik deteksi siap pakai berbasis ATT&CK. https://car.mitre.org
- **MITRE D3FEND** — framework defensive technique dan matrix-nya. https://d3fend.mitre.org
- **MITRE Engage** — disebut sebagai salah satu resource MITRE terkait deception/adversary engagement (dibahas sebagai pengantar, tidak dieksplorasi detail di room ini).
- **Adversary Emulation Library** — kumpulan emulation plan dikelola Center for Threat Informed Defense (CTID).
- **Caldera** — tool automated adversary emulation berbasis ATT&CK.
- **AADAPT** — matrix ancaman untuk sistem aset digital (blockchain/wallet/smart contract).
- **MITRE ATLAS** — matrix ancaman untuk sistem AI/machine learning.
- **Splunk** — SIEM tool, dipakai sebagai salah satu contoh implementasi query analitik CAR.
- **LogPoint** — SIEM tool, dipakai sebagai salah satu contoh implementasi query analitik CAR.

---

## 10. Catatan Ringkas untuk Ditulis Tangan

**Konsep Inti**
- MITRE — nirlaba, riset banyak bidang, misi dunia lebih aman
- ATT&CK — cara serangan terjadi
- CAR — cara deteksi serangan (analitik siap pakai)
- D3FEND — cara bertahan/mitigasi

**Struktur TTP**
- Tactic — tujuan (kenapa)
- Technique — cara (gimana)
- Sub-technique — metode spesifik
- Procedure — implementasi nyata oleh grup/malware

**Matrix ATT&CK**
- Enterprise — IT korporat (Windows/Linux/macOS/cloud)
- Mobile — perangkat mobile
- ICS — sistem kontrol industri (pabrik, SCADA, PLC)
- Navigator — tool visualisasi/anotasi matrix

**Pengguna ATT&CK**
- CTI Team — profil ancaman dari TTP
- SOC Analyst — konteks & prioritas alert
- Detection Engineer — mapping SIEM/EDR ke ATT&CK
- Incident Responder — timeline insiden ke TTP
- Red/Purple Team — emulasi serangan nyata

**Kapan ATT&CK dipakai**
- Proaktif — riset grup APT sebelum diserang
- Forensik — kronologi insiden dipetakan ke TTP
- Detection engineering — cek gap deteksi
- Red team — simulasi gaya serangan grup

**CAR**
- Analitik siap pakai berbasis ATT&CK
- Isi halaman: ID, deskripsi, Analytic Type, ATT&CK Detections, D3FEND Techniques, Implementations (pseudocode + Splunk/LogPoint query, kadang Unit Test)
- Pseudocode — instruksi bahasa manusia sebelum jadi query teknis

**D3FEND — 7 Tactic**
- Model — pemodelan sistem
- Harden — perkuat sistem
- Detect — deteksi aktivitas
- Isolate — isolasi ancaman
- Deceive — jebak attacker
- Evict — usir attacker
- Restore — pulihkan sistem

**Struktur halaman D3FEND**
- ID (D3-XXX), Definition, How it works, Considerations, Artifact Relationships
- Contoh: Credential Rotation (D3-CRO) — rotasi password/key/sertifikat
- Contoh: User Geolocation Logon Pattern Analysis (D3-UGLPA) — analisis geolocation logon vs baseline, artifact: Network Traffic

**Alur kerja defender**
- Kill Chain (gambaran besar) → ATT&CK (detail technique) → D3FEND (mitigasi) → NIST IR Lifecycle (proses investigasi) → IOC (bukti teknis)

**Studi kasus Mustang Panda (G0129)**
- Asal: China, aktif sejak 2012
- Target: pemerintah, NGO, non-profit Asia Tenggara
- Initial Access: Phishing (T1566)
- Persistence: Scheduled Task/Job
- Defense Evasion: Obfuscated Files or Information
- C2: Ingress Tool Transfer

**Studi kasus APT33 (G0064)**
- Asal: Iran, aktif sejak 2013
- Target: sektor aviasi & energi (AS, Saudi, Korsel)
- Technique: Valid Accounts: Cloud Accounts (T1078.004), pakai tool Ruler (S0358) untuk kompromi Office 365
- Mitigasi: M1018 User Account Management
- Detection Strategy: DET0546 (Analytic ID: AN1503–AN1506)

**Project MITRE lain**
- Adversary Emulation Library — panduan step-by-step tiru grup threat (CTID)
- Caldera — automated adversary emulation tool berbasis ATT&CK
- AADAPT — matrix ancaman aset digital (blockchain, wallet, smart contract)
- ATLAS — matrix ancaman sistem AI/machine learning

**ID format cepat**
- Technique ATT&CK — Txxxx (sub: Txxxx.00x)
- Mitigation ATT&CK — Mxxxx
- Software ATT&CK — Sxxxx
- Group ATT&CK — Gxxxx
- Analytic CAR — CAR-YYYY-MM-xxx
- Technique D3FEND — D3-XXX
- Detection Strategy — DETxxxx, Analytic ID — ANxxxx
