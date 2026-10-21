# Resume Materi: SOC Blue Team Overview
**Tanggal:** 21 Oktober 2026
**Room:** SOC Blue Team (TryHackMe — Junior Security Analyst Path)

---

## 1. Konsep Inti

### 1.1 Blue Team

**Blue Team** adalah tim keamanan yang berfokus pada **defensive security** — terus-menerus memantau serangan dan meresponsnya dengan cepat.

Bergantung pada ukuran dan sektor perusahaan, Blue Team bisa terdiri dari 3 hingga 50 anggota dengan peran dan sub-departemen yang berbeda-beda.

### 1.2 Prioritas Keamanan per Industri

Setiap perusahaan punya prioritas keamanan yang berbeda:

- Firma hukum — privasi dokumen legal
- Pabrik — ketersediaan jalur produksi
- Rumah sakit — keselamatan pasien

Karena itu, setiap perusahaan punya pendekatan dan struktur tim keamanan yang unik.

---

## 2. Security Hierarchy (Hierarki Keamanan)

Struktur keamanan perusahaan besar berbentuk piramida dari atas ke bawah:

**Tingkat 1 — Executives (CEO / CFO / Company Owner)**
Fokus pada tujuan bisnis global, tidak mengelola aspek teknis secara langsung.

**Tingkat 2 — Security Leadership (CTO / CIO / CISO)**
Memimpin program IT atau security di seluruh perusahaan. CEO mempekerjakan **CISO** agar ada yang memahami kebutuhan bisnis dan membangun departemen keamanan yang sesuai.

**Tingkat 3 — Security Managers (Team Lead / SOC Manager / Red Team Lead)**
Mengelola satu tim atau departemen spesifik seperti SOC.

**Tingkat 4 — Technical (Analyst / Engineer / Red Teamer)**
Melakukan pekerjaan teknis langsung seperti analisis log, konfigurasi tools, dan pengujian keamanan.

---

## 3. Security Departments

Di perusahaan kecil, departemen IT yang menangani keamanan. Di perusahaan menengah, ada tim "Information Security" generik. Di perusahaan besar, CISO mengawasi beberapa tim yang masing-masing punya tugas spesifik:

**Red Team** — Offensive security: pentester dan ethical hacker yang mencari celah keamanan sebelum diserang musuh sungguhan.

**GRC Team** — Mengelola kebijakan dan memastikan kepatuhan terhadap regulasi seperti **PCI DSS**.

**Blue Team** — Defensive security: SOC analyst, engineer, dan incident responder yang menjaga sistem dari ancaman aktif.

---

## 4. Security Operations Center (SOC)

### 4.1 Definisi dan Peran

SOC adalah **pusat utama cyber security** sebuah organisasi — lini pertahanan pertama yang bekerja dengan berbagai alert dan menangani sebagian besar serangan.

SOC beroperasi **24/7** dan merupakan tempat paling umum untuk memulai karier cyber security.

### 4.2 Fungsi SOC

- Mengoperasikan sistem secara 24/7
- Mengumpulkan dan memantau log
- Membuat detection rules
- Menyelidiki security alert
- Bekerja sama dengan tim IT
- Menulis summary report

### 4.3 Peran dalam SOC

**L1 Analyst** — Anggota junior yang melakukan triage alert dan meneruskan kasus kompleks ke L2.

**L2 Analyst** — Anggota berpengalaman yang menyelidiki serangan lebih lanjut dan kasus yang dieskalasiakan dari L1.

**Engineer** — Pakar dalam mengonfigurasi dan merawat tools security seperti **EDR** dan **SIEM**.

**Manager** — Mengelola seluruh tim SOC, termasuk jadwal dan koordinasi antar anggota.

---

## 5. Cyber Incident Response Team (CIRT)

### 5.1 Definisi

**CIRT** (juga disebut **CSIRT** atau **CERT**) adalah tim yang dipanggil saat keahlian SOC tidak cukup atau insiden sudah di luar kendali. Dikenal sebagai "cyber firefighters" karena dipanggil on-demand saat situasi kritis.

Anggota CIRT harus punya pengetahuan luas tentang ancaman cyber dan mampu menangani breach **tanpa bergantung pada tools** seperti EDR atau SIEM.

### 5.2 Fungsi CIRT

- Dipanggil on-demand saat insiden kritis
- Mendukung SOC dengan analisis mendalam
- Mengidentifikasi hidden threats
- Melakukan deep forensics
- Menangani critical incidents
- Memulihkan sistem yang telah dibobol (recovery)

### 5.3 Peran dalam CIRT

- **CSIRT Manager** — Memimpin tim respons insiden
- **Forensics Lead** — Memimpin investigasi forensik digital
- **Threat Intel Expert** — Ahli intelijen ancaman
- **Threat Hunting Expert** — Spesialis pencarian ancaman tersembunyi
- **Malware Analyst** — Menganalisis malware yang ditemukan

### 5.4 Contoh CIRT Nyata

- **JPCERT** — CERT nasional Jepang, menangani breach skala nasional
- **Mandiant** — Tim swasta yang merespons insiden cyber secara global
- **AWS CIRT** — Menyelidiki insiden keamanan pelanggan AWS

---

## 6. Specialized Defensive Roles

Perusahaan besar, startup teknologi, dan agensi pemerintah sering membutuhkan peran Blue Team yang lebih sempit dan terspesialisasi. Peran ini membutuhkan pengetahuan mendalam dan pengalaman luas di bidang SOC atau IT.

**DevSecOps** — Mengintegrasikan keamanan ke dalam proses pengembangan software secara menyeluruh.

**Penetration Tester** — Menguji sistem secara aktif untuk menemukan kerentanan sebelum dieksploitasi.

**GRC Auditor** — Memastikan kepatuhan terhadap regulasi dan standar keamanan.

**AppSec Engineer** — Menjaga siklus pengembangan software tetap aman (secure software development lifecycle).

**Threat Analyst / Threat Intelligence Analyst** — Mengumpulkan dan menganalisis data tentang kelompok ancaman yang baru muncul.

**Digital Forensics Analyst** — Mengungkap ancaman tersembunyi di disk dan memory.

**AI Researcher** — Mempelajari ancaman berbasis AI dan cara mempertahankan diri dari ancaman tersebut.

---

## 7. Internal SOC vs MSSP

Tidak semua organisasi mampu mengoperasikan SOC sendiri. Sebagian mengandalkan **MSSP (Managed Security Services Provider)**, yaitu perusahaan yang menyediakan layanan keamanan outsourced (paling umum SOC) kepada kliennya.

| Aspek | Internal SOC | MSSP |
|---|---|---|
| Scenario | Bekerja di SOC satu perusahaan, misal bank | Bekerja untuk MSSP global yang melindungi 60+ klien |
| Working Pace | Shift tenang, tekanan waktu lebih rendah | Shift dimulai dari antrean alert urgent |
| Security Tools | Sedikit tools, tapi dikuasai sangat dalam | Harus menguasai 60+ tools dan platform beragam |
| Incident Practice | Mungkin hanya 2 insiden besar per tahun | Setiap minggu berhadapan dengan attack dan breach |

Bekerja di MSSP bertekanan tinggi tapi mempercepat pertumbuhan pengalaman. Disarankan untuk melamar posisi SOC mana pun yang tersedia sebagai pekerjaan pertama.

---

## 8. SOC Career Path

### 8.1 Cara Memulai sebagai SOC L1

1. Pelajari skill inti SOC dan praktikkan. Skill terkait seperti red teaming atau IT umum juga membantu.
2. Bersikap proaktif: ikut **CTF**, terus update dengan berita cyber, dan pertimbangkan sertifikasi **SAL1**.
3. Persiapkan diri untuk interview, pahami perbedaan internal SOC vs MSSP, dan lamar pekerjaan.
4. Setelah beberapa waktu di posisi junior, persiapkan diri untuk naik ke peran yang lebih senior.

### 8.2 Next Steps setelah L1

Langkah natural setelah L1 adalah menjadi **SOC L2 Analyst**, namun jalur lain juga terbuka:

- Tertarik pada konfigurasi tools → jalur **SOC Engineer**
- Tertarik pada respons insiden besar → jalur **CIRT**
- Tertarik pada manajemen tim → jalur **SOC Manager** hingga **CISO**

### 8.3 Tips Efektif di Tahun Pertama

**Learn From Every Alert** — Pahami mengapa sebuah rule terpicu dan gunakan itu untuk mempertajam skill deteksi.

**Think Like An Attacker** — Tanyakan "mengapa attacker melakukan ini" sebelum melakukan triage terhadap bagaimana mereka melakukannya.

**Verify Everything** — Jangan pernah berasumsi. Selalu verifikasi alert dan perilaku mencurigakan di log.

**Get Involved in Incidents** — Serangan nyata mengajarkan pelajaran yang tidak bisa didapat dari lab. Sepadan dengan malam tanpa tidur.

---

## 9. Skenario Penugasan Peran (Final Challenge)

Kemampuan kunci seorang CISO atau analis senior: mengetahui **siapa yang tepat untuk menangani insiden apa**.

| Skenario Insiden | Peran yang Tepat |
|---|---|
| Triage alert brute-force dari SIEM | SOC L1 Analyst |
| Deep analysis phishing malware | SOC L2 Analyst |
| Ransomware aktif, butuh respons segera | CERT Lead / CIRT |
| Audit kepatuhan PCI DSS | GRC Auditor |
| Cek vulnerability versi baru aplikasi | Penetration Tester |
| SIEM tidak bisa digunakan karena storage penuh | SOC Engineer |
| Analisis taktik kelompok ancaman (mis. FIN7) | Threat Researcher / Threat Intelligence Analyst |

---

## 10. Glossary

**SOC (Security Operations Center)** — Pusat utama cyber security organisasi, bertanggung jawab memantau, mendeteksi, dan merespons ancaman secara 24/7.

**CISO (Chief Information Security Officer)** — Pemimpin tertinggi fungsi keamanan informasi di sebuah perusahaan, melapor ke CEO/eksekutif.

**CIRT (Cyber Incident Response Team)** — Tim yang dipanggil saat insiden kritis tidak bisa ditangani SOC sendiri. Dikenal juga sebagai CSIRT atau CERT.

**MSSP (Managed Security Services Provider)** — Perusahaan yang menyediakan layanan keamanan outsourced kepada klien lain, paling umum dalam bentuk SOC.

**EDR (Endpoint Detection and Response)** — Tools untuk mendeteksi dan merespons ancaman di endpoint (laptop, server, dll).

**SIEM (Security Information and Event Management)** — Platform yang mengumpulkan dan menganalisis log dari berbagai sumber untuk mendeteksi aktivitas mencurigakan.

**GRC (Governance, Risk, Compliance)** — Fungsi yang memastikan perusahaan mematuhi regulasi dan standar keamanan seperti PCI DSS.

**PCI DSS (Payment Card Industry Data Security Standard)** — Standar keamanan untuk organisasi yang menyimpan, memproses, atau mengirimkan data kartu pembayaran.

**Red Team** — Tim offensive security yang mensimulasikan serangan untuk menemukan kelemahan pertahanan.

**Blue Team** — Tim defensive security yang memantau, mendeteksi, dan merespons serangan nyata.

**Triage** — Proses memilah dan memprioritaskan alert untuk menentukan mana yang perlu ditangani segera dan mana yang bisa dilewati.

**CTF (Capture The Flag)** — Kompetisi keamanan siber berbasis tantangan yang digunakan untuk mengasah skill praktis.

**SAL1** — Sertifikasi entry-level yang direkomendasikan untuk calon SOC analyst.

**FIN7** — Kelompok ancaman (threat group) terorganisir yang dikenal aktif menargetkan sektor keuangan dan retail.

**AppSec (Application Security)** — Praktik memastikan keamanan dalam siklus pengembangan software.

**DevSecOps** — Integrasi keamanan ke dalam pipeline DevOps agar keamanan menjadi bagian dari setiap tahap pengembangan.

**Forensics** — Proses investigasi mendalam pada sistem yang telah dibobol untuk menemukan bukti dan memahami cara kerja serangan.

**Threat Hunting** — Aktivitas proaktif mencari ancaman yang mungkin sudah masuk ke sistem tapi belum terdeteksi oleh alert otomatis.

**Incident Response** — Proses terstruktur untuk menangani dan memulihkan sistem dari insiden keamanan.

---

## 11. Tools & Platform Rujukan

**TryHackMe** — Platform belajar cyber security berbasis room dan lab interaktif, tempat room ini berada.
URL: https://tryhackme.com

**JPCERT/CC** — CERT nasional Jepang untuk koordinasi respons insiden skala nasional.
URL: https://www.jpcert.or.jp/english/

**Mandiant** — Perusahaan respons insiden dan threat intelligence global (kini bagian dari Google).
URL: https://www.mandiant.com

**AWS CIRT** — Tim respons insiden keamanan Amazon Web Services untuk pelanggan AWS.
URL: https://aws.amazon.com/security/

---

## 12. Catatan Ringkas untuk Ditulis Tangan

### Hierarki Keamanan

- CEO/CFO — fokus bisnis global, tidak urus teknis
- CISO — pemimpin keamanan seluruh perusahaan, direkrut CEO
- SOC Manager / Red Team Lead — kelola satu tim/departemen
- Analyst / Engineer / Red Teamer — pekerjaan teknis lapangan

### Tiga Tim Utama Blue Team

- Red Team — offensive, cari celah, ethical hacker
- GRC Team — kebijakan & kepatuhan regulasi (PCI DSS dll)
- Blue Team — defensive, SOC analyst, engineer, incident responder

### SOC

- Beroperasi 24/7, lini pertahanan pertama
- L1 — triage alert, eskalasi ke L2
- L2 — investigasi kasus lebih kompleks
- Engineer — konfigurasi EDR, SIEM
- Manager — kelola seluruh tim SOC

### Fungsi SOC

- Collect & monitor logs
- Create detection rules
- Investigate security alerts
- Cooperate with IT Team
- Write summary reports

### CIRT / CSIRT / CERT

- Dipanggil on-demand saat insiden kritis
- Tidak bergantung pada EDR/SIEM
- Fungsi: forensics, hidden threat hunting, recovery
- Peran: CSIRT Manager, Forensics Lead, Threat Intel Expert, Threat Hunting Expert, Malware Analyst
- Contoh: JPCERT, Mandiant, AWS CIRT

### Specialized Roles

- DevSecOps — keamanan dalam pipeline pengembangan
- Penetration Tester — cari vuln sebelum diserang
- GRC Auditor — pastikan compliance regulasi
- AppSec Engineer — amankan software development lifecycle
- Threat Intelligence Analyst — analisis kelompok ancaman
- Digital Forensics Analyst — investigasi disk & memory
- AI Researcher — pelajari ancaman AI

### Internal SOC vs MSSP

- Internal SOC — 1 perusahaan, tools sedikit tapi dalam, insiden jarang, ritme tenang
- MSSP — banyak klien, 60+ tools, insiden tiap minggu, bertekanan tinggi

### Cara Mulai Karier SOC

1. Kuasai skill inti SOC + skill pendukung (IT, red team)
2. Ikut CTF, ikuti berita cyber, ambil SAL1
3. Pahami beda Internal SOC vs MSSP, lamar kerja
4. Naik ke L2 atau pilih jalur lain setelah cukup pengalaman

### Tips Tahun Pertama

- Learn From Every Alert — pahami kenapa rule terpicu
- Think Like An Attacker — tanya "kenapa attacker lakukan ini" sebelum triage
- Verify Everything — jangan asumsikan, selalu validasi di log
- Get Involved in Incidents — insiden nyata = pelajaran terbaik

### Penugasan Peran Berdasarkan Insiden

- Triage alert SIEM → SOC L1 Analyst
- Deep analysis malware/phishing → SOC L2 Analyst
- Ransomware aktif → CERT Lead
- Audit PCI DSS → GRC Auditor
- Cek vulnerability aplikasi → Penetration Tester
- SIEM rusak/storage penuh → SOC Engineer
- Analisis taktik threat group → Threat Intelligence Analyst

### Istilah Kunci

- SOC — pusat cyber security, 24/7
- CISO — pemimpin keamanan perusahaan
- CIRT/CSIRT/CERT — tim respons insiden kritis
- MSSP — penyedia SOC outsourced
- EDR — deteksi & respons ancaman di endpoint
- SIEM — kumpulkan & analisis log dari banyak sumber
- GRC — governance, risk, compliance
- PCI DSS — standar keamanan data kartu pembayaran
- Triage — pilah & prioritaskan alert
- CTF — kompetisi hacking untuk asah skill
- SAL1 — sertifikasi entry-level SOC analyst
- FIN7 — threat group terorganisir
