# Resume: Perimeter Security & Network Monitoring

**Tanggal:** 23 September 2026

---

## 1. Konsep Dasar Jaringan

### 1.1 Jaringan sebagai Ekosistem

Jaringan komputer bukan kumpulan perangkat acak, melainkan struktur terorganisir di mana aset (server, workstation, aplikasi, perangkat keamanan) saling terhubung untuk komunikasi, berbagi sumber daya, dan konektivitas.

### 1.2 Komponen Utama Jaringan Enterprise

**User Workstations (Endpoints)** — PC/laptop tempat karyawan bekerja. Titik masuk paling umum bagi penyerang, biasanya lewat phishing atau malicious download. Kompromi di endpoint bisa jadi pijakan awal untuk lateral movement.

**File & Database Servers** — Menyimpan aset paling berharga: dokumen bersama dan data terstruktur (customer records, HR, finansial). Target utama ransomware dan kampanye eksfiltrasi data.

**Application Servers** — Web server (host website), email server (komunikasi korporat), VPN gateway (akses remote). Karena externally facing, jadi target scan konstan untuk kerentanan software atau misconfiguration.

**Active Directory (AD) / Authentication Servers** — Identity backbone: mengelola user, group, komputer, dan hak akses. Target utama untuk privilege escalation, persistence, dan lateral movement. Satu domain admin yang dikompromikan bisa melumpuhkan seluruh enterprise.

**Routers & Switches** — Router menghubungkan LAN ke internet; switch menghubungkan perangkat dalam jaringan yang sama. Jika dikompromikan, bisa dipakai untuk MITM, backdoor routing, atau membuka channel tersembunyi.

**Firewalls / Perimeter Devices** — Gateway keamanan utama yang menginspeksi traffic masuk/keluar dan memutuskan allow/block berdasarkan security rules. Mencatat setiap percobaan koneksi — sumber deteksi dini untuk port scan, brute-force, dan eksploitasi.

---

## 2. Network Perimeter

### 2.1 Definisi

Perimeter adalah batas yang memisahkan jaringan internal (trusted zone) dari internet eksternal (untrusted zone) — titik masuk/keluar data yang terkontrol. Seluruh traffic dari dan ke internet wajib melewati titik ini.

### 2.2 Komponen Perimeter

- **Firewalls** — gatekeeper yang menyaring traffic internal-eksternal.
- **Routers/Gateways** — merutekan traffic dan menegakkan access rules.
- **DMZ (Demilitarized Zone)** — segmen penyangga tempat server publik (web, mail, VPN) ditempatkan, terisolasi dari jaringan internal murni.
- **Remote Access Gateways / VPN** — titik masuk aman untuk karyawan remote.

### 2.3 Kenapa Perimeter Penting

Perimeter adalah garis pertahanan pertama; sering jadi tempat pertama SOC analyst melihat tanda aktivitas berbahaya. Perimeter yang lemah/misconfigured membuka risiko: eksploitasi layanan terekspos (RDP, MySQL, SMB), scanning/reconnaissance, brute-force login, dan eksfiltrasi data.

### 2.4 Peran Security Analyst dalam Monitoring Perimeter

- Meninjau log firewall untuk koneksi blocked/allowed.
- Mengenali pola scanning atau brute-force.
- Menandai traffic outbound tidak biasa (indikasi beaconing/eksfiltrasi).
- Memahami apa yang seharusnya (dan tidak seharusnya) terekspos di perimeter.

---

## 3. Network Visibility

### 3.1 Definisi

Kemampuan memantau dan memahami apa yang terjadi di seluruh jaringan. Prinsip inti: tidak bisa mempertahankan apa yang tidak bisa dilihat. Visibilitas memungkinkan deteksi anomali, investigasi insiden, threat hunting, dan compliance.

### 3.2 Dua Kategori Sumber Log

**Host-Centric Logs** — dihasilkan oleh perangkat individual (server, workstation, laptop). Memberi gambaran detail level-mesin tentang suatu kejadian.

Sumber: Operating System Logs (Windows Event Logs, Linux syslog, macOS logs — mencatat logon, pembuatan proses, failed login), Application Logs (Apache, Nginx, MySQL, MSSQL, dll), Security Tool Logs (antivirus, EDR, HIDS).

Kegunaan: forensic analysis detail, tracking eksekusi proses/script mencurigakan, user activity monitoring, malware impact assessment.

**Network-Centric Logs** — dihasilkan perangkat jaringan yang memantau traffic antar perangkat. Menampilkan source/destination IP, port, protokol, dan tindakan (allow/block).

Sumber: Firewalls (percobaan koneksi tidak sah dari internet), IDS/IPS (pola serangan/anomali real-time), Routers & Switches (flow data — siapa bicara ke siapa, berapa lama, berapa data), Web Proxies (kunjungan website, ancaman berbasis web), VPN (siapa connect, dari mana, kapan).

Kegunaan: early threat detection, identifikasi C2, tracking lateral movement, deteksi data exfiltration, broad context serangan.

### 3.3 Kenapa Keduanya Harus Dikombinasikan

Host log memberi tahu apa yang terjadi di dalam satu mesin; network log memberi tahu apa yang terjadi antar mesin. Kombinasi keduanya memungkinkan analyst merangkai timeline insiden secara menyeluruh dan akurat.

---

## 4. Pola Deteksi Serangan pada Log Perimeter

### 4.1 Prinsip Umum Membaca Log

- Repetisi dari satu sumber ke banyak tujuan/port berbeda = **scanning**.
- Repetisi dari satu sumber ke satu tujuan (banyak percobaan) = **brute-forcing**.
- Traffic dengan interval waktu yang sangat teratur/konsisten = **malware beaconing**.
- Konteks (jenis serangan dari IDS) lebih berharga daripada sekadar status block/allow dari firewall.
- Tugas analyst: memisahkan traffic normal dari aktivitas mencurigakan.

### 4.2 Skenario Port Scanning

Ciri di firewall log: satu IP eksternal mencoba banyak port berbeda ke satu/beberapa IP internal dalam waktu singkat, sebagian besar berstatus BLOCK.

### 4.3 Skenario Web Attack (WAF/IDS Logs)

WAF/IDS memberi detail jenis serangan lewat field `attack_type`:
- **SQL Injection** — percobaan dump data database.
- **XSS (Cross-Site Scripting)** — penyisipan script berbahaya.
- **Directory Traversal** — percobaan membaca file sensitif di server (contoh pola: `../../../etc/passwd`).

Filter `action=BLOCK` langsung menampilkan ancaman yang berhasil ditangkis WAF.

### 4.4 Skenario VPN Brute-Force

Log dibanjiri `FAILED_AUTH` dari berbagai username umum (admin, root, test, guest) yang berasal dari satu IP sumber yang sama, diselingi `SUCCESS_AUTH` acak dari user lain (traffic wajar karyawan). Sinyal serangan: volume kegagalan besar dari satu IP dalam waktu singkat, mencoba banyak username berbeda ke satu target.

---

## 5. Metodologi Investigasi Insiden (Manual Log Analysis)

### 5.1 Alur Investigasi Bertahap

Investigasi breach mengikuti rantai serangan (attack chain) berurutan, di mana setiap tahap dikonfirmasi lewat command sebelum pivot ke tahap berikutnya:

1. **Reconnaissance** — identifikasi IP eksternal yang melakukan scanning lewat firewall log (block terbanyak).
2. **Initial Access** — cek apakah IP yang sama pernah berhasil ALLOW (menembus firewall), lalu telusuri credential access lewat VPN log (brute-force).
3. **Foothold** — identifikasi IP internal yang diberikan ke attacker setelah berhasil login (assigned_ip pertama setelah rangkaian FAIL yang diikuti SUCCESS).
4. **Lateral Movement** — dari IP internal attacker, cek traffic ALLOW ke host-host lain, dikonfirmasi jenis eksploitasinya lewat IDS log (misalnya SMB exploit di port 445).
5. **C2 Beaconing** — cari IDS alert terkait C2 pada host yang mungkin sudah terinfeksi; ciri khas: koneksi ke IP eksternal yang sama dengan interval waktu sangat teratur.
6. **Data Exfiltration** — cari IDS alert "HTTP POST Large Upload"/"Potential Data Exfiltration" dari host (biasanya web/app server) ke IP eksternal yang sama dengan C2.

Catatan penting: attacker infrastructure bisa terdiri dari beberapa IP berbeda dengan fungsi berbeda (satu IP untuk entry point/scanning, IP lain untuk C2 server) — jangan asumsikan satu attacker selalu satu IP.

Catatan penting lain: kompromi bisa melibatkan lebih dari satu host internal dengan peran berbeda — satu host untuk C2 command channel (koneksi TCP polos, port non-standar), host lain untuk exfiltration data (traffic HTTP POST besar lewat port 80/8080).

### 5.2 Command-Line Tools untuk Log Analysis

**head [file]** — menampilkan baris awal file, untuk eksplorasi cepat format log.

**cat [file]** — menampilkan seluruh isi file.

**grep "[pola]" [file]** — mencari baris yang mengandung teks tertentu.
- `-c` — hanya menampilkan jumlah baris yang cocok (count), bukan isinya.
- `-o` — hanya menampilkan bagian teks yang cocok dengan pola, bukan seluruh baris.
- `-E` — mengaktifkan extended regex (mendukung pola seperti `{1,3}` atau `+`).
- `-n` — menampilkan nomor baris beserta hasil match.
- `-i` — pencarian case-insensitive.

**cut -d'[delimiter]' -f[kolom]** — memotong baris berdasarkan pemisah (`-d`) dan mengambil kolom tertentu (`-f`), misalnya memisahkan IP dari port menggunakan delimiter `:`.

**sort** — mengurutkan baris.
- `-n` — mengurutkan secara numerik (bukan alfabetis).
- `-r` — mengurutkan terbalik (descending).

**uniq** — menghapus baris duplikat yang berurutan (harus di-sort dulu).
- `-c` — menghitung jumlah kemunculan tiap baris unik.

**awk -F'[pola]' '{print $N}'** — memecah baris berdasarkan field separator (`-F`) dan mencetak kolom ke-N.

**tail** — menampilkan baris akhir file (berguna untuk melihat kejadian terbaru dalam urutan kronologis).

Pola umum pipeline untuk "cari IP/nilai paling sering muncul":
```
cat [file] | grep "[filter]" | cut/awk (ekstrak field) | sort | uniq -c | sort -nr
```

Pola untuk mengekstrak IPv4 langsung dari baris log tanpa bergantung pada format delimiter tertentu:
```
grep -oE "[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+" [file]
```

### 5.3 Analisis Log via Splunk (Alternatif Manual)

Splunk digunakan sebagai alternatif saat volume log besar agar analisis tidak tedious secara manual. Log pre-ingested ke index tertentu (contoh: `index="network_logs"`), diakses lewat tab **Search & Reporting**. Field `sourcetype` memisahkan jenis log (firewall_logs, ids_logs, vpn_logs) dalam satu index yang sama.

---

## 6. Studi Kasus: Investigasi Breach Initech Corp

### 6.1 Konteks Skenario

Perusahaan jasa keuangan memasang firewall dan IDS baru. Tim SOC kewalahan sehingga analisis mendalam terlewat selama sebulan. Tugas: telusuri satu bulan log perimeter (firewall, WAF/IDS, VPN) untuk merekonstruksi teknik serangan dan menentukan apakah perimeter berhasil ditembus.

### 6.2 Rekonstruksi Rantai Serangan

1. IP eksternal melakukan **reconnaissance/port scanning** ke beberapa host internal — mayoritas diblokir firewall, tapi ditemukan juga baris ALLOW dari IP yang sama, mengindikasikan sebagian percobaan berhasil.
2. IP yang sama melakukan **brute-force** terhadap akun VPN tertentu (banyak FAILED_AUTH berturut-turut, lalu SUCCESS_AUTH).
3. Setelah login berhasil, attacker diberi **IP internal ephemeral** oleh VPN gateway (assigned_ip pertama pasca brute-force berhasil) — ini titik **initial access**.
4. Dari IP internal tersebut, attacker melakukan **lateral movement** ke beberapa host internal lain lewat port SMB/RDP/SSH, dikonfirmasi IDS sebagai "Possible MS-SMB Lateral Movement" di port 445.
5. Salah satu host yang tersentuh lateral movement (workstation) mulai menunjukkan **C2 Beaconing** — koneksi TCP berulang dengan interval sangat teratur (misal tiap 6 jam) ke satu IP eksternal tertentu di port non-standar.
6. Host lain yang tersentuh (web/app server) menunjukkan **data exfiltration** — alert "HTTP POST Large Upload"/"Potential Data Exfiltration" berulang ke IP eksternal yang sama (port 80/8080).

### 6.3 Insight Kunci dari Studi Kasus

Satu insiden breach melibatkan multiple host dengan peran berbeda: satu host sebagai entry point (VPN client ephemeral), satu host sebagai C2 channel, host lain sebagai jalur exfiltration. IP attacker untuk fase awal (scanning, brute-force) bisa berbeda dari IP C2 server pada fase lanjut — mencerminkan pemisahan infrastruktur attacker di dunia nyata (entry point vs C2 dipisah agar C2 tetap aman meski entry point terdeteksi).

---

## 7. Glossary

**Network Perimeter** — batas antara jaringan internal (trusted) dan internet eksternal (untrusted); titik kontrol masuk/keluar seluruh traffic.

**DMZ (Demilitarized Zone)** — segmen jaringan penyangga untuk server publik, terisolasi dari jaringan internal murni.

**Host-Centric Log** — log yang dihasilkan satu perangkat individual, memberi gambaran detail level-mesin.

**Network-Centric Log** — log yang dihasilkan perangkat jaringan, memberi gambaran traffic antar perangkat.

**IDS/IPS (Intrusion Detection/Prevention System)** — sistem yang memantau traffic untuk pola serangan dikenal (signature) atau perilaku anomali.

**WAF (Web Application Firewall)** — firewall khusus yang menginspeksi dan memblokir serangan level aplikasi web (SQLi, XSS, directory traversal).

**Port Scanning** — teknik reconnaissance dengan mencoba banyak port pada target untuk menemukan layanan terbuka.

**Brute-Force** — percobaan login berulang dengan menebak kredensial (username/password) hingga berhasil.

**Lateral Movement** — pergerakan attacker dari satu mesin yang dikompromikan ke mesin lain di jaringan internal.

**C2 (Command & Control) Server** — server eksternal milik attacker yang mengendalikan malware dari jarak jauh.

**Beaconing** — koneksi berkala dari host terinfeksi ke C2 server dengan interval waktu yang konsisten, sebagai sinyal "lapor diri".

**Data Exfiltration** — proses mengirim/mencuri data keluar dari jaringan menuju lokasi eksternal.

**Persistence** — teknik attacker mempertahankan akses jangka panjang pada sistem yang sudah dikompromikan.

**Privilege Escalation** — teknik attacker menaikkan level hak akses dari yang awalnya dimiliki.

**SOC (Security Operations Center)** — tim/fungsi yang bertanggung jawab memantau dan merespons insiden keamanan.

**Flow Data** — ringkasan percakapan traffic (siapa bicara ke siapa, durasi, volume data) yang dihasilkan router/switch tanpa logging tradisional.

---

## 8. Tools & Platform Rujukan

**Splunk** — platform SIEM untuk mengumpulkan, mengindeks, dan mencari log dalam jumlah besar; dipakai sebagai alternatif analisis manual command-line pada studi kasus ini. Diakses lewat `localhost:8000` pada VM lab.

---

## 9. Catatan Ringkas untuk Ditulis Tangan

**Komponen Jaringan**
- Endpoint — entry point umum (phishing)
- File/DB Server — target ransomware & eksfiltrasi
- App Server (web/email/VPN) — externally facing, target scan
- AD — identity backbone, target privesc/persistence
- Router/Switch — MITM, backdoor kalau kompromi
- Firewall — gatekeeper, log tiap koneksi

**Perimeter**
- Definisi — batas trusted vs untrusted network
- Komponen — Firewall, Router/Gateway, DMZ, VPN gateway
- Peran analyst — review log, kenali scan/brute-force, cek outbound aneh

**Network Visibility**
- Host-centric log — OS log, App log, Security tool log (per mesin)
- Network-centric log — Firewall, IDS/IPS, Router/Switch flow, Proxy, VPN (antar mesin)
- Kombinasi keduanya = timeline lengkap

**Pola Deteksi**
- 1 sumber → banyak tujuan/port = scanning
- 1 sumber → 1 tujuan berulang = brute-force
- Interval waktu teratur = beaconing
- WAF attack_type: SQL Injection, XSS, Directory Traversal

**Attack Chain (urutan investigasi)**
1. Recon (firewall BLOCK, cari IP terbanyak)
2. Initial Access (cek ALLOW dari IP sama + VPN brute-force FAIL→SUCCESS)
3. Foothold (assigned_ip pertama setelah SUCCESS)
4. Lateral Movement (ALLOW dari IP internal attacker, cek IDS: SMB port 445)
5. C2 Beaconing (IDS alert C2, interval waktu konsisten)
6. Data Exfiltration (IDS "HTTP POST Large Upload", port 80/8080)
- Catatan: IP entry point bisa beda dari IP C2 server
- Catatan: host C2 channel bisa beda dari host exfiltration

**Command Cheatsheet**
- head [file] — lihat baris awal
- cat [file] — tampilkan semua isi
- grep "pola" — cari baris cocok; -c count, -o hanya match, -E regex, -n nomor baris, -i case-insensitive
- cut -d'delim' -f[n] — potong kolom berdasar pemisah
- sort — urutkan; -n numerik, -r descending
- uniq -c — hitung duplikat (butuh sort dulu)
- awk -F'pola' '{print $N}' — pecah field custom, ambil kolom N
- Pipeline umum: grep | cut/awk | sort | uniq -c | sort -nr
- Regex ekstrak IP: grep -oE "[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+"

**Glossary Singkat**
- DMZ — zona penyangga server publik
- WAF — firewall level aplikasi web
- IDS/IPS — deteksi/cegah pola serangan
- Lateral Movement — pindah mesin ke mesin dalam internal
- C2 — server pengendali malware
- Beaconing — lapor diri berkala ke C2
- Persistence — pertahankan akses lama
- Privilege Escalation — naikkan hak akses
- Flow Data — ringkasan percakapan traffic router/switch

**Tools**
- Splunk — SIEM, search & analisis log skala besar (localhost:8000)
