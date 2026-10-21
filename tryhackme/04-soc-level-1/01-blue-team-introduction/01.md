# Resume Materi: Junior Security Analyst Intro
**Platform:** TryHackMe
**Tanggal:** 21 Oktober 2026

---

## 1. Security Operations Center (SOC)

### 1.1 Definisi

**Security Operations Center (SOC)** adalah tim keamanan yang bekerja nonstop 24/7 untuk memantau, mendeteksi, menganalisis, dan merespons ancaman siber terhadap perusahaan. SOC adalah garis pertahanan utama organisasi dari serangan siber.

### 1.2 Peran dalam Tim SOC

**SOC Level 1 Analyst (Junior Security Analyst)**
Analis garis depan yang pertama kali menerima dan meninjau peringatan keamanan. Bertugas melakukan analisis awal sebelum meneruskan ke senior jika diperlukan.

**Senior Analyst**
Menangani kasus kompleks setelah Junior Analyst selesai melakukan analisis awal. Menjadi rujukan ketika Junior Analyst menemui ketidakjelasan dalam investigasi.

**SOC Engineer**
Tidak memiliki shift dan tidak menganalisis alert. Bertugas memelihara dan mengkonfigurasi alat-alat keamanan agar pekerjaan analis menjadi lebih mudah dan efisien.

**SOC Manager**
Mengawasi seluruh operasi SOC, melaporkan hasil kepada manajemen puncak, dan memastikan setiap anggota tim tidak tersesat dalam kompleksitas dunia keamanan siber.

**Incident Responder**
Tidak hadir setiap hari. Dipanggil secara khusus hanya saat terjadi insiden besar yang memerlukan penanganan mendalam dan terstruktur.


## 2. Tugas Harian Junior Security Analyst

Seorang Junior Analyst bekerja dalam shift dan secara rutin melakukan hal-hal berikut:

- Memantau dan menginvestigasi berbagai peringatan keamanan yang masuk
- Berpartisipasi dalam sesi brainstorm dan workshop bersama tim SOC
- Bekerja sama lintas tim untuk menjaga keamanan perusahaan secara keseluruhan
- Terus belajar tentang teknik serangan dan pertahanan terbaru

Seiring pengalaman bertambah, seorang Junior Analyst juga akan terlibat dalam skenario yang lebih kompleks seperti:

- Mendeteksi dan mencegah infeksi data stealer pada perangkat internal
- Menganalisis dan menghentikan kampanye phishing yang menargetkan departemen tertentu
- Berpartisipasi dalam penanganan insiden besar seperti serangan ransomware skala penuh
- Membangun aturan deteksi dan otomatisasi bersama tim
- Memahami cara perusahaan beroperasi dari dalam, bukan hanya dari sisi teknis


## 3. Alur Kerja Investigasi Ancaman

Berikut adalah alur kerja standar yang dilakukan seorang Junior Analyst saat merespons alert:

### 3.1 Tahap 1 — Deteksi via SIEM Dashboard

**SIEM (Security Information and Event Management)** adalah platform terpusat yang mengumpulkan dan menampilkan seluruh log dan peringatan keamanan dari berbagai sumber dalam satu dashboard.

Analis membaca daftar alert berdasarkan:

- **Date** — waktu kejadian
- **Severity** — tingkat keparahan: Critical, Medium, atau Low
- **Message** — deskripsi singkat kejadian

Alert dengan severity **Critical** diprioritaskan untuk segera diinvestigasi.

### 3.2 Tahap 2 — Investigasi IP via IP Hunter

Setelah menemukan IP mencurigakan dari alert, analis melakukan pengecekan reputasi IP menggunakan database eksternal. Informasi yang dicari meliputi:

- **ISP** — penyedia layanan internet dari IP tersebut
- **Domain Name** — domain yang terkait
- **Country** — negara asal IP
- **Categories** — jenis aktivitas berbahaya yang pernah dilakukan (contoh: Port Scan, C2 Server, PlugX)

Jika IP ditemukan dalam database sebagai **malicious**, proses dilanjutkan ke tahap eskalasi.

### 3.3 Tahap 3 — Eskalasi (Escalation)

Eskalasi adalah proses meneruskan temuan kepada pihak yang berwenang dan relevan di dalam tim. Penting untuk memastikan eskalasi dilakukan ke orang yang **tepat** — yaitu yang bertanggung jawab atas tim atau departemen terkait, bukan sembarang staf.

Dalam konteks SOC, eskalasi dari Junior Analyst dilakukan kepada **Senior Analyst atau SOC Team Lead**.

### 3.4 Tahap 4 — Pemblokiran via Firewall

Setelah mendapat izin atau konfirmasi dari senior, analis memblokir IP berbahaya melalui **Firewall Block List**. Proses ini menghentikan atau memperlambat serangan dengan mencegah IP tersebut mengakses sistem perusahaan.

Data yang dimasukkan saat memblokir:

- IP Address yang akan diblokir
- Komentar opsional yang menjelaskan alasan pemblokiran


## 4. Konsep dan Istilah Teknis Penting

### 4.1 Jenis Ancaman yang Relevan

**Phishing** — serangan yang mengelabui pengguna agar menyerahkan informasi sensitif melalui email atau halaman palsu.

**Ransomware** — malware yang mengenkripsi data korban dan meminta tebusan untuk pemulihannya.

**Data Stealer** — malware yang mencuri data sensitif dari perangkat yang terinfeksi.

**Brute Force / Unauthorized Login Attempt** — upaya masuk paksa ke sistem dengan mencoba kombinasi kredensial berulang kali.

**C2 Server (Command and Control)** — server yang digunakan penyerang untuk mengendalikan malware atau sistem yang sudah dikompromikan.

**PlugX** — jenis malware Remote Access Trojan (RAT) yang sering digunakan oleh kelompok ancaman tingkat lanjut (APT).

**Supply Chain Attack** — serangan yang menyusup ke sistem target melalui pihak ketiga yang dipercaya, seperti library atau software yang digunakan target.

**APT (Advanced Persistent Threat)** — kelompok penyerang terorganisir, biasanya disponsori negara, yang melancarkan serangan jangka panjang dan tersembunyi.

**Backdoor** — akses tersembunyi yang ditanamkan di sistem untuk memungkinkan penyerang masuk kembali kapan saja tanpa terdeteksi.

### 4.2 Konsep Operasional

**Alert / Ticket** — peringatan atau tugas yang dihasilkan sistem keamanan dan harus ditangani oleh analis dalam waktu tertentu.

**Eskalasi** — proses meneruskan insiden atau alert kepada pihak yang lebih berwenang untuk penanganan lebih lanjut.

**Severity Level** — tingkat keparahan suatu alert: Critical (kritis, segera ditangani), Medium (sedang), Low (rendah, bisa ditunda).

**Reputation Check** — pengecekan apakah suatu IP, domain, atau file pernah terlibat aktivitas berbahaya menggunakan database publik.

**Block List** — daftar IP atau domain yang dilarang mengakses sistem, diterapkan di level firewall.

**SSH (Secure Shell)** — protokol untuk akses jarak jauh ke server secara terenkripsi. Login SSH dari IP tak dikenal adalah indikator kompromi yang serius.

**Port 22** — port default untuk layanan SSH. Upaya koneksi ke port ini dari IP mencurigakan perlu segera diinvestigasi.


## 5. Studi Kasus: Investigasi Alert di Lab

Berikut adalah skenario nyata yang dipraktikkan dalam room ini:

**Situasi:** Dashboard SIEM menampilkan dua alert Critical dari IP yang sama pada tanggal 10 Juni 2026.

- Pukul 19:12 — Unauthorized login attempts dari IP 221.181.185.159 ke port 22
- Pukul 19:16 — Successful SSH login dari IP 221.181.185.159

**Hasil investigasi IP Hunter:**
- IP: 221.181.185.159
- ISP: China Mobile Communications Corporation
- Domain: chinamobile.thm
- Negara: China
- Kategori: Port Scan, C2 Server, PlugX
- Status: Malicious — terlibat dalam 4 serangan siber

**Eskalasi:** Alert diteruskan kepada Will Griffin selaku SOC Team Lead karena beliau bertanggung jawab atas analisis alert tingkat lanjut.

**Tindakan:** IP 221.181.185.159 dimasukkan ke Firewall Block List untuk menghentikan akses lebih lanjut dari IP tersebut.

**Pelajaran kunci dari skenario ini:**
Serangan ini menunjukkan pola klasik — penyerang melakukan brute force terlebih dahulu (Unauthorized login attempts), lalu berhasil masuk (Successful SSH login). Keberhasilan login dari IP yang sudah diketahui berbahaya adalah indikator kompromi aktif yang harus segera direspons.


## 6. Glossary

**SOC** — Security Operations Center; pusat operasi keamanan yang bekerja 24/7 memantau dan merespons ancaman siber.

**SIEM** — Security Information and Event Management; platform yang mengumpulkan dan menampilkan log serta alert keamanan dari seluruh infrastruktur.

**APT** — Advanced Persistent Threat; kelompok penyerang terorganisir, biasanya disponsori negara, dengan kemampuan serangan tingkat tinggi.

**C2 Server** — Command and Control Server; server pengendali yang digunakan penyerang untuk berkomunikasi dengan malware di sistem korban.

**PlugX** — Remote Access Trojan yang sering digunakan kelompok APT, terutama yang berafiliasi dengan China.

**Phishing** — teknik rekayasa sosial melalui email atau situs palsu untuk mencuri kredensial atau data sensitif.

**Ransomware** — malware yang mengenkripsi data korban dan meminta tebusan.

**Data Stealer** — malware yang dirancang untuk mengekstrak data sensitif dari perangkat korban.

**Backdoor** — akses tersembunyi ke sistem yang memungkinkan penyerang masuk kembali tanpa terdeteksi.

**Supply Chain Attack** — serangan yang masuk melalui celah di pihak ketiga yang dipercaya oleh target.

**Brute Force** — metode serangan dengan mencoba banyak kombinasi kredensial secara otomatis hingga berhasil masuk.

**Eskalasi** — proses meneruskan temuan kepada pihak yang lebih berwenang dalam hierarki tim.

**Severity** — tingkat keparahan alert: Critical, Medium, atau Low.

**Port 22** — port default protokol SSH yang sering menjadi target serangan brute force.

**Reputation Check** — pengecekan reputasi IP/domain menggunakan database threat intelligence publik.

**Firewall Block List** — daftar IP/domain yang diblokir di level firewall agar tidak bisa mengakses sistem.

**SSH** — Secure Shell; protokol akses jarak jauh terenkripsi ke server atau perangkat jaringan.


## 7. Tools & Platform Rujukan

**AbuseIPDB**
Fungsi: Database publik untuk mengecek dan melaporkan IP berbahaya berdasarkan laporan komunitas global.
URL: https://www.abuseipdb.com

**Cisco Talos Intelligence**
Fungsi: Platform threat intelligence dari Cisco untuk mengecek reputasi IP, domain, dan file.
URL: https://talosintelligence.com

**TryHackMe Security Lab (simulasi)**
Fungsi: Platform lab interaktif untuk simulasi SIEM Dashboard, IP Hunter, Escalation, dan Firewall yang digunakan dalam room ini.
URL: https://security.tryhackme.thm/alerts (environment lab internal TryHackMe)


## 8. Catatan Ringkas untuk Ditulis Tangan

### SOC & Tim

- SOC — Security Operations Center, kerja 24/7
- L1 Analyst — review alert awal, eskalasi ke senior
- Senior Analyst — tangani kasus kompleks, bantu Junior
- SOC Engineer — konfigurasi tools, tidak analisis alert
- SOC Manager — lapor ke manajemen, koordinasi tim
- Incident Responder — dipanggil saat insiden besar saja

### Alur Kerja Investigasi

- SIEM Dashboard — lihat alert, prioritaskan Critical
- IP Hunter — cek reputasi IP (ISP, negara, kategori)
- Escalation — teruskan ke orang yang tepat (bukan sembarang staf)
- Firewall — blokir IP berbahaya, isi IP + komentar

### Jenis Ancaman

- Phishing — tipu user lewat email/situs palsu
- Ransomware — enkripsi data, minta tebusan
- Data Stealer — curi data dari perangkat
- Brute Force — coba banyak password sampai berhasil
- Backdoor — akses tersembunyi ke sistem
- Supply Chain Attack — masuk lewat pihak ketiga
- C2 Server — server kendali malware
- PlugX — RAT, sering dipakai kelompok APT

### Istilah Kunci

- APT — kelompok penyerang canggih, biasanya disponsori negara
- Alert/Ticket — peringatan keamanan yang harus ditangani
- Severity — Critical / Medium / Low
- Eskalasi — teruskan ke senior yang berwenang
- Port 22 — default SSH, target brute force
- Reputation Check — cek IP/domain di database publik
- Block List — daftar IP diblokir di firewall

### Studi Kasus Lab

- IP berbahaya: 221.181.185.159
- Asal: China, ISP China Mobile, kategori Port Scan + C2 + PlugX
- Alert: brute force port 22 → berhasil SSH login (Critical)
- Eskalasi ke: Will Griffin (SOC Team Lead)
- Tindakan: blokir di Firewall Block List

### Tools Rujukan

- AbuseIPDB — cek/laporkan IP berbahaya — abuseipdb.com
- Cisco Talos — threat intelligence IP & domain — talosintelligence.com
