# Resume: Unified Kill Chain (UKC)

**Room:** Unified Kill Chain — TryHackMe
**Tanggal diselesaikan:** 1 September 2026

---

## 1. Konsep Dasar

### 1.1 Kill Chain

**Kill Chain** adalah istilah yang berasal dari dunia militer, digunakan untuk menjelaskan tahapan-tahapan sebuah serangan. Dalam konteks keamanan siber, istilah ini menggambarkan metodologi atau jalur yang digunakan penyerang (hacker, APT) untuk mendekati dan menyusup ke target.

Contoh sederhana sebuah kill chain: scanning → eksploitasi kerentanan web → privilege escalation.

Tujuan mempelajari kill chain adalah memahami pola serangan sehingga tim defensif bisa menerapkan langkah pencegahan (preemptive protection) atau mengganggu (disrupt) serangan di titik tertentu sebelum berhasil.

### 1.2 Threat Modelling

**Threat modelling** adalah proses untuk meningkatkan keamanan sistem dengan cara mengidentifikasi risiko. Terdiri dari 4 langkah inti:

1. Mengidentifikasi sistem/aplikasi yang perlu diamankan dan fungsinya (kritikal terhadap operasi? menyimpan data sensitif?).
2. Menilai kerentanan dan kelemahan yang mungkin dimiliki, serta cara eksploitasinya.
3. Membuat rencana tindakan (mitigasi) atas kerentanan yang ditemukan.
4. Menerapkan kebijakan pencegahan jangka panjang, contoh: **SDLC** (Software Development Life Cycle) atau pelatihan awareness **phishing**.

Threat modelling menghasilkan gambaran tingkat tinggi (high-level overview) atas aset IT organisasi (aset = perangkat lunak/keras) beserta prosedur mitigasinya. UKC mendorong proses ini karena membantu mengidentifikasi **attack surface**.

Framework lain yang khusus dipakai untuk threat modelling: **STRIDE**, **DREAD**, **CVSS**.

---

## 2. Unified Kill Chain (UKC)

### 2.1 Latar Belakang

UKC dibuat oleh **Paul Pols**, dipublikasikan tahun **2017** (diperbarui 2022). UKC dirancang untuk **melengkapi**, bukan bersaing dengan, framework kill chain lain seperti **Lockheed Martin Cyber Kill Chain** dan **MITRE ATT&CK**.

### 2.2 Keunggulan UKC Dibanding Framework Lain

- **Modern** — dirilis 2017/2022, sementara framework lain (mis. MITRE, 2013) dibuat saat lanskap ancaman siber masih berbeda.
- **Sangat detail** — punya 18 fase resmi, sementara framework lain umumnya hanya punya beberapa fase.
- **Cakupan penuh** — mencakup seluruh siklus serangan: reconnaissance, eksploitasi, post-eksploitasi, hingga motivasi penyerang.
- **Realistis** — mengakui bahwa penyerang tidak berjalan linear; fase-fase bisa terjadi berulang (misalnya setelah eksploitasi, penyerang kembali melakukan reconnaissance untuk pivot ke sistem lain).

### 2.3 18 Fase UKC

| # | Fase | Deskripsi Singkat |
|---|------|--------------------|
| 1 | Reconnaissance | Riset & identifikasi target (pasif/aktif) |
| 2 | Weaponization | Menyiapkan infrastruktur serangan |
| 3 | Delivery | Mengirim objek yang telah "dipersenjatai" ke target |
| 4 | Social Engineering | Manipulasi manusia agar melakukan tindakan tidak aman |
| 5 | Exploitation | Eksploitasi kerentanan untuk eksekusi kode |
| 6 | Persistence | Mempertahankan akses jangka panjang |
| 7 | Defense Evasion | Menghindari deteksi/pertahanan |
| 8 | Command & Control | Komunikasi dengan sistem yang dikuasai |
| 9 | Pivoting | Tunneling ke sistem lain yang tidak langsung dapat diakses |
| 10 | Discovery | Mengumpulkan informasi sistem & jaringan |
| 11 | Privilege Escalation | Meningkatkan level akses/izin |
| 12 | Execution | Menjalankan kode kendali penyerang |
| 13 | Credential Access | Mencuri/mengendalikan kredensial |
| 14 | Lateral Movement | Berpindah horizontal antar sistem |
| 15 | Collection | Mengumpulkan data sebelum eksfiltrasi |
| 16 | Exfiltration | Mengeluarkan data dari jaringan target |
| 17 | Impact | Memanipulasi/mengganggu/menghancurkan sistem atau data |
| 18 | Objectives | Tujuan strategis akhir dari serangan |

Setiap fase (kecuali Objectives) memiliki pemetaan ke **MITRE Tactic ID** masing-masing (lihat bagian 3–5).

### 2.4 Tiga Fase Besar (Goal)

UKC mengelompokkan 18 fase di atas menjadi tiga tujuan besar (goal) sepanjang serangan: **In**, **Through**, **Out**. Ketiganya bersifat siklikal — penyerang bisa bolak-balik antar fase dalam satu goal sebelum lanjut ke goal berikutnya.

---

## 3. Goal "In" — Initial Foothold

Fokus: mendapatkan akses awal ke sistem/jaringan target.

Fase-fase yang termasuk: Reconnaissance, Weaponization, Pivoting, Delivery, Social Engineering, Exploitation, Persistence, Defense Evasion, Command & Control.

**Reconnaissance** (MITRE TA0043)
Mengumpulkan informasi target lewat reconnaissance pasif/aktif. Output: sistem/service yang berjalan, daftar kontak/karyawan untuk phishing, kredensial potensial, topologi jaringan.

**Weaponization** (MITRE TA0001)
Menyiapkan infrastruktur serangan, misalnya server command and control atau sistem penangkap reverse shell.

**Social Engineering** (MITRE TA0001)
Memanipulasi karyawan lewat: lampiran berbahaya di email phishing, halaman web palsu (credential harvesting), atau impersonasi (telepon/kunjungan langsung, menyamar sebagai teknisi).

**Exploitation** (MITRE TA0002)
Menyalahgunakan kerentanan untuk eksekusi kode. Contoh: upload & jalankan reverse shell di aplikasi web, mengganggu script otomatis, mengeksploitasi kerentanan aplikasi web.

**Persistence** (MITRE TA0003)
Mempertahankan akses ke foothold awal. Contoh: membuat service backdoor, mendaftarkan sistem ke server C2, backdoor yang trigger pada event tertentu (mis. saat admin login).

**Defence Evasion** (MITRE TA0005)
Menghindari kontrol pertahanan: web application firewall, network firewall, antivirus, intrusion detection system. Fase ini berharga untuk membangun respons dan memperbaiki sistem pertahanan ke depan.

**Command & Control** (MITRE TA0011)
Menggabungkan hasil Weaponization untuk membangun komunikasi dua arah dengan sistem target. Digunakan untuk: eksekusi perintah, mencuri data/kredensial, pivot ke sistem lain.

**Pivoting** (MITRE TA0008)
Menjangkau sistem lain yang tidak terekspos langsung (mis. tidak terhubung internet), lewat sistem yang sudah dikuasai sebagai perantara. Sistem yang tidak langsung terjangkau ini sering menyimpan data berharga/keamanan lebih lemah.

---

## 4. Goal "Through" — Network Propagation

Fokus: setelah foothold berhasil, penyerang memperluas akses & privilege bila kontrol defensif menghalangi pencapaian tujuan akhir. Sistem yang dikuasai dijadikan **pivot point** untuk memetakan jaringan internal.

Fase-fase yang termasuk: Pivoting, Discovery, Access, Privilege Escalation, Execution, Credential Access, Lateral Movement.

**Pivoting** (MITRE TA0008)
Sistem yang dikuasai dijadikan staging site dan tunnel antara command operations penyerang dengan jaringan korban; juga jadi titik distribusi malware/backdoor tahap berikutnya.

**Discovery** (MITRE TA0007)
Mengungkap informasi sistem & jaringan: akun aktif, izin, aplikasi/software, aktivitas browser, file/direktori/network share, konfigurasi sistem.

**Privilege Escalation** (MITRE TA0004)
Meningkatkan akses ke salah satu level superior:
- SYSTEM/ROOT
- Local Administrator
- Akun dengan akses setara admin
- Akun dengan akses/fungsi spesifik

**Execution** (MITRE TA0002)
Menjalankan kode berbahaya menggunakan sistem pivot sebagai host: remote trojan, script C2, tautan berbahaya, scheduled task — untuk mempertahankan recurring presence & persistence.

**Credential Access** (MITRE TA0006)
Mencuri akun & password (keylogging, credential dumping), berjalan bersamaan dengan Privilege Escalation. Kredensial sah membuat aktivitas lebih sulit terdeteksi.

**Lateral Movement** (MITRE TA0008)
Berpindah ke sistem target lain menggunakan kredensial & privilege yang didapat. Semakin stealthy tekniknya, semakin baik bagi penyerang.

---

## 5. Goal "Out" — Action on Objectives

Fokus: tahap akhir, penyerang sudah punya akses ke aset kritikal dan mewujudkan tujuan serangan — biasanya mengkompromikan salah satu dari **CIA Triad** (Confidentiality, Integrity, Availability).

Fase-fase yang termasuk: Collection, Exfiltration, Impact, Objectives.

**Collection** (MITRE TA0009)
Mengumpulkan data berharga dari sumber utama: drive, browser, audio, video, email. Mengkompromikan confidentiality; mengarah ke Exfiltration.

**Exfiltration** (MITRE TA0010)
Mencuri data yang telah dikemas dengan enkripsi & kompresi untuk menghindari deteksi. Memanfaatkan channel C2 dan tunnel yang sudah dibangun sebelumnya.

**Impact** (MITRE TA0040)
Bila sasaran adalah integrity & availability: manipulasi, gangguan, atau penghancuran aset. Contoh: hapus akses akun, disk wipe, ransomware, defacement, DoS.

**Objectives**
Pencapaian tujuan strategis akhir. Contoh: motif finansial → ransomware + tebusan; motif reputasi → merilis data privat/rahasia ke publik.

---

## 6. Ringkasan Alur Menyeluruh

Urutan besar UKC secara konseptual: **0. Threat Modelling** (persiapan/analisis risiko sebelum serangan terjadi, dari sisi defender) → **1. Initial Foothold (In)** → **2. Network Propagation (Through)** → **3. Action on Objectives (Out)**.

Poin penting: proses ini **tidak linear**. Penyerang bisa berulang kali kembali ke fase sebelumnya (mis. Reconnaissance ulang untuk pivot baru) selama satu kampanye serangan berlangsung.

---

## 7. Glossary (Istilah Wajib Hapal)

- **Kill Chain** — tahapan sebuah serangan, istilah asal militer.
- **UKC (Unified Kill Chain)** — framework 18 fase oleh Paul Pols (2017), melengkapi framework lain.
- **Threat Modelling** — proses identifikasi risiko untuk memperkuat keamanan sistem.
- **Attack Surface** — seluruh titik/area yang berpotensi dieksploitasi penyerang.
- **Asset (IT)** — perangkat lunak atau perangkat keras milik organisasi.
- **SDLC** — Software Development Life Cycle, siklus pengembangan software yang bisa disisipi kontrol keamanan.
- **Phishing** — social engineering via email/pesan untuk menipu korban.
- **Reconnaissance** — pengumpulan informasi target (pasif/aktif).
- **Weaponization** — penyiapan infrastruktur serangan.
- **Delivery** — pengiriman objek berbahaya ke target.
- **Social Engineering** — manipulasi manusia untuk tindakan tidak aman.
- **Exploitation** — pemanfaatan kerentanan untuk eksekusi kode.
- **Persistence** — mekanisme mempertahankan akses jangka panjang.
- **Defense Evasion** — teknik menghindari deteksi/pertahanan.
- **Command & Control (C2)** — komunikasi kendali antara penyerang & sistem yang dikuasai.
- **Pivoting** — menggunakan sistem yang dikuasai sebagai batu loncatan ke sistem lain.
- **Discovery** — pengumpulan info sistem & jaringan pasca-akses.
- **Privilege Escalation** — peningkatan level izin/akses.
- **Execution** — eksekusi kode berbahaya yang dikendalikan penyerang.
- **Credential Access** — pencurian/pengambilalihan kredensial.
- **Lateral Movement** — perpindahan horizontal antar sistem dalam jaringan.
- **Collection** — pengumpulan data incaran sebelum eksfiltrasi.
- **Exfiltration** — pengeluaran data curian dari jaringan target.
- **Impact** — gangguan/kerusakan terhadap integrity & availability sistem/data.
- **Objectives** — tujuan strategis akhir serangan (finansial, reputasi, dll).
- **CIA Triad** — Confidentiality, Integrity, Availability — tiga pilar yang jadi sasaran kompromi.
- **APT** — Advanced Persistent Threat, kelompok penyerang canggih & persisten.
- **Ransomware** — malware yang mengenkripsi data dan meminta tebusan.
- **DoS (Denial of Service)** — serangan yang membuat layanan tidak dapat diakses.
- **Defacement** — perusakan tampilan/isi sebuah situs web oleh penyerang.

---

## 8. Tools & Platform Rujukan

Room ini tidak menyebutkan tools analisis eksternal (seperti Talos, VirusTotal, Shodan, dll) — materi berfokus murni pada penjelasan konseptual framework UKC. Rekomendasi yang diberikan justru berupa room lanjutan di TryHackMe untuk memperdalam pemahaman framework keamanan siber:

- **Principles of Security** — room TryHackMe, membahas prinsip dasar keamanan siber termasuk framework threat modelling (STRIDE, DREAD, CVSS).
- **Pentesting Fundamentals** — room TryHackMe, dasar-dasar metodologi pentest.
- **Cyber Kill Chain** — room TryHackMe, membahas framework Lockheed Martin Cyber Kill Chain sebagai pembanding UKC.

---

## 9. Catatan Ringkas untuk Ditulis Tangan

**Konsep Dasar**
- Kill Chain — tahapan serangan, asal istilah militer
- Threat Modelling — identifikasi risiko: (1) identifikasi aset (2) nilai kerentanan (3) rencana mitigasi (4) kebijakan pencegahan (SDLC, phishing awareness)
- STRIDE, DREAD, CVSS — framework threat modelling

**UKC Umum**
- UKC — Paul Pols, 2017 (update 2022), melengkapi Lockheed Martin & MITRE ATT&CK
- Keunggulan — modern, 18 fase (detail), full cycle, realistis (non-linear)
- 3 Goal besar — In, Through, Out

**Goal In (Initial Foothold)**
- Reconnaissance (TA0043) — kumpul info target
- Weaponization (TA0001) — siapkan infrastruktur (server C2, dsb)
- Social Engineering (TA0001) — manipulasi manusia (phishing, fake page, impersonasi)
- Exploitation (TA0002) — eksploitasi kerentanan → code execution
- Persistence (TA0003) — pertahankan akses (service, C2 registration, trigger backdoor)
- Defence Evasion (TA0005) — hindari WAF, firewall, AV, IDS
- Command & Control (TA0011) — kendali sistem via C2
- Pivoting (TA0008) — loncat ke sistem lain via sistem yang dikuasai

**Goal Through (Network Propagation)**
- Pivoting (TA0008) — staging site + tunnel
- Discovery (TA0007) — akun, izin, software, file, konfigurasi
- Privilege Escalation (TA0004) — naik ke SYSTEM/ROOT/Admin/user khusus
- Execution (TA0002) — trojan, script C2, scheduled task
- Credential Access (TA0006) — keylogging, credential dumping
- Lateral Movement (TA0008) — pindah sistem pakai kredensial curian, makin stealthy makin baik

**Goal Out (Action on Objectives)**
- Collection (TA0009) — kumpul data (drive, browser, audio, video, email)
- Exfiltration (TA0010) — curi data terenkripsi+kompres, pakai channel C2
- Impact (TA0040) — rusak integrity/availability: disk wipe, ransomware, defacement, DoS
- Objectives — tujuan akhir: finansial (ransomware+tebusan) / reputasi (bocorkan data)

**CIA Triad** — Confidentiality, Integrity, Availability — target kompromi utama

**Alur besar** — 0. Threat Modelling → 1. In (foothold) → 2. Through (propagation) → 3. Out (objectives), non-linear/berulang

**Room lanjutan** — Principles of Security, Pentesting Fundamentals, Cyber Kill Chain
