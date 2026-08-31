# Resume: Vulnerability Scanning
Tanggal: 31 Agustus 2026

---

## 1. Konsep Dasar

### 1.1 Vulnerability

**Vulnerability** adalah kelemahan pada software atau hardware sebuah perangkat digital yang dapat dimanfaatkan (exploit) oleh attacker untuk membahayakan sistem. Berbeda dengan kerusakan fisik yang mudah terlihat, vulnerability tidak bisa disadari begitu saja — harus dicari secara aktif (hunting) sebelum bisa diperbaiki.

### 1.2 Patching

**Patching** adalah proses menerapkan perbaikan (fix) terhadap vulnerability yang sudah ditemukan. Patch dilakukan dengan mengubah software atau konfigurasi sistem agar celah keamanan tertutup.

### 1.3 Vulnerability Scanning

**Vulnerability scanning** adalah proses inspeksi sistem digital untuk menemukan kelemahan secara sistematis. Dilakukan secara rutin karena:

- Organisasi menyimpan informasi kritis yang harus dilindungi.
- Beberapa standar keamanan dan badan regulasi mewajibkan scanning berkala (umumnya per kuartal atau per tahun) sebagai bagian dari compliance.
- Scanning manual pada jaringan besar sangat lambat dan berisiko melewatkan vulnerability penting.

**Automated vulnerability scanning** mengatasi masalah ini: cukup instal tool, beri alamat IP host atau network range, tool akan memindai dan menghasilkan laporan detail berisi vulnerability yang ditemukan.

---

## 2. Klasifikasi Vulnerability Scan

### 2.1 Authenticated vs Unauthenticated Scan

**Authenticated scan** dilakukan dengan memberikan kredensial host target kepada scanner. Hasilnya lebih detail karena scanner bisa memeriksa konfigurasi dan aplikasi terinstal dari dalam. Cocok untuk mensimulasikan ancaman dari attacker yang sudah punya akses ke host.

**Unauthenticated scan** dilakukan tanpa kredensial, hanya membutuhkan alamat IP. Lebih ringan sumber daya dan mudah disiapkan. Cocok untuk mensimulasikan ancaman dari attacker eksternal yang belum punya akses.

Authenticated scan umumnya dipakai untuk internal scanning, sedangkan unauthenticated scan umumnya dipakai untuk external scanning.

### 2.2 Internal vs External Scan

**Internal scan** dilakukan dari dalam jaringan, berfokus pada vulnerability yang bisa dieksploitasi setelah attacker berhasil masuk ke jaringan.

**External scan** dilakukan dari luar jaringan, berfokus pada vulnerability yang terekspos ke attacker dari luar (misal, website publik).

Pemilihan jenis scan bergantung pada scope, resource, dan tujuan assessment yang ingin dicapai.

---

## 3. Tools Vulnerability Scanning

| Tool | Karakteristik Utama |
|---|---|
| Nessus | Awalnya open-source (1998), diakuisisi Tenable (2005), kini proprietary. Ada versi gratis (fitur terbatas) dan Professional (fitur lengkap, unlimited scan, support). Deployment on-premises. |
| Qualys | Subscription-based (1999), cloud-based platform. Selain vulnerability scanning, punya compliance check dan asset management, plus alert otomatis dari continuous monitoring. Tidak butuh infrastruktur fisik sendiri. |
| Nexpose | Dikembangkan Rapid7 (2005), subscription-based. Terus menemukan aset baru di jaringan dan men-scan otomatis. Memberi risk score berdasarkan nilai aset dan dampak vulnerability. Deployment on-premises atau hybrid. |
| OpenVAS | Open-source (Greenbone Security). Fitur dasar dengan database known vulnerability sendiri. Kurang lengkap dibanding tool komersial, tapi cukup untuk organisasi kecil/individu. |

Hampir semua vulnerability scanner menyediakan fitur reporting: daftar vulnerability, risk score, deskripsi detail, dan beberapa juga menyediakan metode remediasi serta export laporan dalam berbagai format.

Faktor pemilihan tool: scope, resource, depth of analysis, dan kebutuhan spesifik organisasi.

---

## 4. CVE dan CVSS

### 4.1 CVE (Common Vulnerabilities and Exposures)

**CVE** adalah nomor unik yang diberikan pada sebuah vulnerability, dikembangkan oleh **MITRE Corporation**. Setiap vulnerability baru yang ditemukan pada software apapun akan mendapat nomor CVE unik dan dipublikasikan di database CVE agar publik bisa mengambil langkah protektif.

Format nomor CVE:

```
CVE-YYYY-NNNN
```

- **CVE** — prefix tetap di setiap nomor.
- **YYYY** — tahun ditemukannya vulnerability.
- **NNNN** — digit acak (minimal 4 digit) sebagai identifier unik.

### 4.2 CVSS (Common Vulnerability Scoring System)

**CVSS** adalah sistem skoring untuk menentukan tingkat keparahan (severity) sebuah vulnerability, dengan rentang skor 0.0 hingga 10.0. Skor dihitung berdasarkan berbagai faktor: dampak (impact), tingkat kemudahan eksploitasi (ease of exploitability), dan faktor lainnya.

Tabel severity berdasarkan skor CVSS:

| Rentang Skor | Severity |
|---|---|
| 0.0 – 3.9 | Low |
| 4.0 – 6.9 | Medium |
| 7.0 – 8.9 | High |
| 9.0 – 10.0 | Critical |

CVE mengidentifikasi *apa* vulnerability-nya, sedangkan CVSS mengukur *seberapa parah* vulnerability tersebut.

---

## 5. OpenVAS

### 5.1 Instalasi (via Docker)

OpenVAS punya banyak dependency sehingga instalasi manual cukup rumit. Solusinya menggunakan **Docker** — platform yang membungkus aplikasi beserta seluruh dependency-nya ke dalam satu paket yang disebut **container**.

Langkah instalasi:

```
sudo apt install docker.io
```

Menginstal docker engine pada sistem.

```
sudo docker run -d -p 443:443 --name openvas immauss/openvas
```

Fungsi tiap bagian command:

- `docker run` — membuat dan menjalankan container baru dari sebuah image.
- `-d` — detached mode, menjalankan container di background.
- `-p 443:443` — port mapping, menghubungkan port 443 host ke port 443 container (port HTTPS untuk web interface OpenVAS).
- `--name openvas` — memberi nama container agar mudah dikenali/dirujuk.
- `immauss/openvas` — nama image docker yang dipakai, dibuat oleh Immauss, menggabungkan semua dependency OpenVAS dalam satu image.

### 5.2 Mengelola Container yang Sudah Ada

Kalau container OpenVAS sudah pernah dibuat sebelumnya tapi statusnya berhenti (Exited), tidak perlu `docker run` ulang — cukup nyalakan kembali:

```
sudo docker start openvas
```

`docker start` menyalakan kembali container yang sudah ada, berbeda dengan `docker run` yang membuat container baru dari awal.

Command untuk memeriksa status container:

```
docker ps
```

Menampilkan container yang sedang berjalan (running) saja.

```
docker ps -a
```

Flag `-a` (all) menampilkan seluruh container, termasuk yang statusnya sudah exited/stopped.

Catatan penting: mengakses docker daemon butuh privilege tinggi. Jika muncul error "permission denied while trying to connect to the Docker daemon socket", tambahkan `sudo` di depan command.

### 5.3 Mengakses OpenVAS

Setelah container berjalan, akses web interface melalui browser:

```
https://127.0.0.1
```

atau spesifik ke halaman login:

```
https://127.0.0.1/login/login.html
```

Kredensial login default:

```
Username: admin
Password: admin
```

Dashboard yang muncul setelah login menampilkan overview seluruh vulnerability scan yang pernah dilakukan.

### 5.4 Alur Melakukan Vulnerability Scan

1. Buka menu **Scans → Tasks**.
2. Klik ikon bintang, pilih **New Task**.
3. Isi nama task, buat **Scan Target** baru (isi nama target dan alamat IP/hosts yang mau di-scan — bukan IP mesin attacker sendiri).
4. Pilih **Scan Config** sesuai kebutuhan (misal "Full and fast"), lalu klik Create.
5. Task akan muncul di dashboard Tasks. Klik tombol play pada kolom Actions untuk menjalankan scan.
6. Tunggu hingga status berubah menjadi **Done**. Durasi scan bisa memakan waktu beberapa menit tergantung scope.
7. Klik nama task untuk melihat detail: jumlah **Results** (total vulnerability ditemukan), durasi scan, target, scanner config yang dipakai.
8. Klik angka Results untuk melihat daftar lengkap vulnerability beserta kolom Severity, QoD (Quality of Detection), Host, Location (port/protocol), dan waktu ditemukan.
9. Setiap vulnerability bisa diklik untuk melihat detail lengkap: Summary, Vulnerability Detection Result, Impact, Solution, Vulnerability Insight, dan Vulnerability Detection Method.
10. Laporan hasil scan bisa diekspor dalam berbagai format dari dashboard Tasks.

### 5.5 Struktur Laporan Scan OpenVAS

Laporan hasil scan OpenVAS (baik dilihat lewat dashboard maupun file export .html) umumnya berisi:

- **Summary** — ringkasan umum: waktu scan mulai/selesai, jumlah total results, catatan filter yang diterapkan (misal threat level "Log"/"Debug" tidak ditampilkan, minimum QoD tertentu).
- **Host Summary** — tabel ringkasan per host: jumlah temuan berdasarkan severity (High, Medium, Low, Log, False Positive).
- **Results per Host** — detail tiap host: Port Summary (service/port beserta threat level tertinggi di port tersebut) dan Security Issues (daftar lengkap tiap vulnerability).

Tiap entry vulnerability dalam Security Issues berisi:

- **Severity + CVSS score** — level dan skor keparahan.
- **NVT (Network Vulnerability Test)** — nama pengujian yang mendeteksi vulnerability tersebut, beserta OID (identifier unik NVT).
- **Summary** — ringkasan singkat masalahnya.
- **Vulnerability Detection Result** — bukti/hasil konkret dari deteksi (misal kredensial yang berhasil dipakai login).
- **Impact** — dampak potensial jika dieksploitasi.
- **Solution** (beserta Solution type: Workaround/Mitigation/dst) — langkah perbaikan yang direkomendasikan.
- **Vulnerability Insight** — penjelasan teknis lebih dalam tentang penyebab vulnerability.
- **Vulnerability Detection Method** — metode yang dipakai scanner untuk mendeteksi.

---

## 6. Glossary

- **Vulnerability** — kelemahan pada software/hardware yang bisa dieksploitasi attacker.
- **Patching** — proses memperbaiki vulnerability yang ditemukan.
- **Exploit** — tindakan memanfaatkan vulnerability untuk membahayakan sistem.
- **Authenticated Scan** — scan dengan kredensial host target.
- **Unauthenticated Scan** — scan tanpa kredensial, hanya berbekal IP.
- **Internal Scan** — scan dari dalam jaringan.
- **External Scan** — scan dari luar jaringan.
- **CVE (Common Vulnerabilities and Exposures)** — sistem penomoran unik untuk tiap vulnerability, dikelola MITRE Corporation.
- **CVSS (Common Vulnerability Scoring System)** — sistem skoring 0.0–10.0 untuk mengukur tingkat keparahan vulnerability.
- **NVT (Network Vulnerability Test)** — modul pengujian yang dipakai OpenVAS untuk mendeteksi vulnerability tertentu.
- **OID** — identifier unik untuk sebuah NVT di OpenVAS.
- **QoD (Quality of Detection)** — persentase tingkat keyakinan scanner terhadap akurasi deteksi sebuah vulnerability.
- **Container (Docker)** — paket aplikasi beserta seluruh dependency-nya, terisolasi dari sistem host.
- **Docker Image** — template/blueprint yang dipakai untuk membuat container.
- **Compliance** — kepatuhan terhadap standar/regulasi keamanan yang berlaku.
- **Threat Surface** — area/permukaan yang berpotensi dieksploitasi attacker.

---

## 7. Tools & Platform Rujukan

- **Nessus** — vulnerability scanner komersial (dulu open-source), oleh Tenable. Tidak disebutkan URL eksplisit di materi.
- **Qualys** — vulnerability management berbasis cloud/subscription. Tidak disebutkan URL eksplisit di materi.
- **Nexpose** — vulnerability management oleh Rapid7, deployment on-premises/hybrid. Tidak disebutkan URL eksplisit di materi.
- **OpenVAS (Greenbone Security Assistant)** — vulnerability scanner open-source, dipakai untuk praktik langsung di room ini. Diakses lokal melalui `https://127.0.0.1`.
- **Docker (immauss/openvas image)** — image docker siap pakai untuk instalasi OpenVAS dalam satu paket container.

---

## 8. Catatan Ringkas untuk Ditulis Tangan

**Konsep Dasar**
- Vulnerability — kelemahan software/hardware yang bisa dieksploitasi
- Patching — proses perbaikan vulnerability
- Vulnerability scanning — inspeksi sistem cari kelemahan, otomatis lebih efisien dari manual

**Jenis Scan**
- Authenticated — pakai kredensial, hasil detail, simulasi insider threat
- Unauthenticated — tanpa kredensial, hanya IP, simulasi outsider threat
- Internal — dari dalam jaringan
- External — dari luar jaringan

**Tools**
- Nessus — Tenable, free & pro version, on-premises
- Qualys — cloud, subscription, ada compliance check + asset management
- Nexpose — Rapid7, risk score berbasis asset value, on-premises/hybrid
- OpenVAS — open-source, Greenbone, cocok organisasi kecil

**CVE & CVSS**
- CVE — nomor unik vulnerability, format CVE-YYYY-NNNN, oleh MITRE
- CVSS — skor keparahan 0.0-10.0
  - 0.0-3.9 Low
  - 4.0-6.9 Medium
  - 7.0-8.9 High
  - 9.0-10.0 Critical

**Docker & OpenVAS Command**
- `sudo apt install docker.io` — instal docker engine
- `sudo docker run -d -p 443:443 --name openvas immauss/openvas` — buat & jalankan container baru
  - `-d` detached/background
  - `-p 443:443` port mapping
  - `--name` beri nama container
- `sudo docker start openvas` — nyalakan ulang container yang sudah ada (exited)
- `docker ps` — lihat container yang running
- `docker ps -a` — lihat semua container (termasuk exited)
- Akses OpenVAS: `https://127.0.0.1` atau `https://127.0.0.1/login/login.html`
- Default login: admin/admin

**Alur Scan OpenVAS**
1. Scans > Tasks > New Task
2. Isi nama, buat Scan Target (isi IP target, bukan IP attacker)
3. Pilih Scan Config (mis. Full and fast) > Create
4. Klik play untuk start scan
5. Tunggu status Done
6. Klik task > lihat jumlah Results
7. Klik Results > lihat daftar vulnerability + severity + host + location

**Struktur Laporan**
- Summary — waktu scan, filter
- Host Summary — jumlah temuan per severity
- Results per Host — Port Summary + Security Issues
- Tiap issue: Severity/CVSS, NVT + OID, Summary, Detection Result, Impact, Solution, Insight, Detection Method

**Istilah Kunci**
- NVT — modul test OpenVAS
- OID — ID unik NVT
- QoD — persentase keyakinan deteksi
- Container — paket app + dependency
- Docker Image — blueprint container
- Threat Surface — area rawan eksploitasi
