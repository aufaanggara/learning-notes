# Resume Room: Playbooks, SIEM, dan SOAR dalam Incident Response (Coursera Cybersecurity) - 11 Oktober 2026

Cakupan: video playbook dan SIEM, bacaan "Playbooks, SIEM tools, and SOAR tools", latihan "Respond to a SIEM alert", video privacy engineer (keberagaman perspektif), dan kuis "Use a playbook to respond to an incident".

## 1. Konsep Inti

### 1.1 Incident Response Playbook

**Playbook** adalah panduan yang membantu profesional keamanan menangani masalah dengan urgensi tinggi tanpa mengorbankan akurasi. Playbook terkadang disebut **runbook**.

Fungsi utama playbook:

- Memberi struktur pada penanganan insiden.
- Memastikan kepatuhan (compliance).
- Menguraikan proses komunikasi dan dokumentasi.
- Menjamin daftar tindakan yang konsisten diikuti dengan cara yang telah ditentukan, siapa pun yang menangani kasus.

Playbook bisa sangat detail dan dapat memuat **flow chart** dan tabel untuk memperjelas tindakan apa yang diambil dan dalam urutan apa. Playbook juga mencakup prosedur pemulihan, misalnya saat serangan **ransomware**.

### 1.2 Playbook per Jenis Insiden

Setiap jenis insiden keamanan punya playbook sendiri yang merinci siapa mengambil tindakan apa dan kapan. Contoh serangan yang bisa punya playbook khusus: **ransomware**, **malware**, dan **distributed denial of service (DDoS)**.

### 1.3 Playbook sebagai Living Document

Playbook adalah **living documents**: tim keamanan sering mengubah, memperbarui, dan memperbaikinya untuk menghadapi ancaman dan kerentanan baru. Pernyataan "playbook bersifat permanen dan tidak boleh diubah" adalah salah.

Organisasi belajar dari insiden sebelumnya untuk meningkatkan **security posture**, menyempurnakan kebijakan dan prosedur, serta mengurangi kemungkinan dan dampak insiden berikutnya. Hasil pembelajaran itu dituangkan dengan memperbarui playbook.

Setiap organisasi mengembangkan prosedur internalnya sendiri, sehingga langkah playbook bisa berbeda antar organisasi. Yang konsisten adalah adanya proses yang baku.

## 2. Playbook bersama Tools Keamanan

### 2.1 Playbook dan SIEM

**SIEM** (security information and event management) mendeteksi aktivitas dan menghasilkan alert. Playbook memberi analis instruksi cara menangani alert tersebut.

Contoh: perilaku pengguna yang tidak biasa ditandai oleh SIEM, lalu analis mengikuti playbook untuk menanganinya.

### 2.2 Playbook dan SOAR

**SOAR** (security orchestration, automation, and response) mirip SIEM karena sama-sama untuk threat monitoring. Perbedaannya, SOAR adalah perangkat lunak untuk **mengotomatisasi tugas berulang** yang dihasilkan oleh tools seperti SIEM atau **managed detection and response (MDR)**.

Contoh: pengguna mencoba login terlalu sering dengan kata sandi salah. SOAR otomatis memblokir akun untuk menghentikan kemungkinan intrusi, lalu analis merujuk ke playbook untuk langkah penyelesaian.

### 2.3 Perbandingan Singkat

| Tool | Peran | Contoh |
|---|---|---|
| SIEM | Deteksi dan alert | Menandai perilaku pengguna tidak biasa |
| SOAR | Otomatisasi tugas berulang | Memblokir akun otomatis setelah login gagal berulang |
| Playbook | Panduan tindakan manusia | Langkah analis menyelesaikan insiden |

## 3. Alur Playbook untuk Alert SIEM

Contoh skenario: analis menerima alert SIEM tentang potensi serangan **malware** (di latihan: unduhan file mencurigakan).

### 3.1 Assess the Alert

Tentukan apakah alert valid dengan mengidentifikasi mengapa SIEM menghasilkannya, melalui analisis **log data** dan metrik terkait. Ini langkah pertama, sebelum containment atau pelaporan.

### 3.2 Containment

Playbook menunjuk tindakan dan tools untuk membatasi insiden dan mengurangi kerusakan. Pada contoh malware, sistem jaringan yang terinfeksi **diisolasi** (diputus koneksinya) agar malware tidak menyebar.

### 3.3 Eradication and Recovery

Hapus semua jejak insiden (artefak) dan pulihkan lingkungan terdampak ke kondisi aman dan operasi normal. Contoh: pulihkan sistem operasi yang terdampak, lalu pulihkan data dari **clean backup** yang dibuat sebelum malware menyebar.

### 3.4 Post-Incident Activity dan Coordination

Setelah insiden selesai, analis melakukan **post-incident activities** dan upaya **coordination** dengan tim keamanan:

- Membuat laporan akhir untuk **stakeholders**.
- Melaporkan insiden ke otoritas berwenang, misalnya **Federal Bureau of Investigation (FBI)** di AS atau lembaga lain yang menyelidiki kejahatan siber.

Koordinasi mencakup pelaporan insiden dan berbagi informasi berdasarkan standar yang telah ditetapkan.

### 3.5 Urutan Ringkas

Assess the alert, lalu containment, lalu eradication and recovery, lalu post-incident activity dan coordination. Dalam latihan, setelah containment dan eliminasi jejak, langkah berikutnya adalah memulihkan sistem terdampak; setelah seluruh langkah playbook selesai, langkah terakhir adalah mengomunikasikan insiden ke stakeholders.

### 3.6 Dashboard SIEM pada Latihan

Dashboard menampilkan dua bagian: **IOC domain matches** (kolom: domain, categories, sources, assets, confidence, severity, IOC ingest time, first seen, last seen) dan **recent alerts** (kolom: alerts, asset, user, alert names, first seen, last seen, severity, sources).

## 4. Fase Playbook yang Muncul di Kuis

Kuis menggunakan nama fase berikut:

- **Preparation**
- **Detection and analysis**: fase menilai alert, termasuk analisis log data dan metrik.
- **Containment**: isolasi sistem terinfeksi.
- **Eradication and recovery**: menghapus artefak insiden dan memulihkan lingkungan, termasuk restore dari clean backup.
- **Post-incident activity**: laporan akhir, pelaporan ke otoritas, dan koordinasi.

Kunci jawaban kuis: skenario restore data dengan clean backup adalah eradication and recovery; analis yang menilai alert SIEM menganalisis log data dan metrik terkait.

## 5. Perspektif Privacy Engineer: Soft Skills dan Keberagaman

### 5.1 Peran Privacy Engineer

**Privacy engineer** (contoh dalam video: Erin, Google) memastikan privasi tertanam sejak awal pada teknologi baru, termasuk **speculative and emerging technology** yang diperkirakan hadir dalam dua hingga lima tahun. Fokusnya adalah **user trust**, perlindungan data yang tidak ingin dibagikan pengguna, dan pengguna yang terinformasi sebelum menyentuh produk.

### 5.2 Soft Skills

**Soft skills** dinilai lebih penting daripada keterampilan teknis, karena keterampilan teknis bisa diajarkan sedangkan kemampuan berelasi dengan orang tidak.

### 5.3 Diversity of Thought dan Perspectives

**Diversity of thought** dan **diversity of perspectives** diperlukan agar produk untuk orang sehari-hari mencerminkan beragam pengalaman. Tim dengan latar belakang berbeda menghadirkan lebih banyak **equity** dan kedalaman.

Contoh dari jurnalisme: untuk meyakinkan tim produk menolak suatu fitur, tanyakan "apakah kita benar-benar ingin ini muncul di The Times?". Pertanyaan ini membuat dampak publik langsung terbayang.

Ajakan: terlibat di **STEM**; bidang product security, privacy, dan software engineering membutuhkan suara yang beragam.

### 5.4 Relevansi bagi Analis Pemula

Entry-level security analyst sering memakai playbook saat memantau jaringan dan merespons insiden. Memahami alasan pentingnya playbook menunjang keberhasilan kerja. Mengetahui siapa melakukan apa dan kapan mengurangi dampak insiden dan risiko kerusakan pada aset penting organisasi.

## 6. Command dan Tools

Room ini bersifat konsep murni dan tidak membahas command, switch, atau syntax teknis.

## 7. Glossary

- **Playbook / runbook**: panduan tindakan rinci tim keamanan saat insiden.
- **Living document**: dokumen yang terus diperbarui menghadapi ancaman baru.
- **SIEM**: tool deteksi yang mengumpulkan data dan menghasilkan alert.
- **SOAR**: perangkat lunak yang mengotomatisasi tugas berulang dari SIEM atau MDR.
- **MDR (managed detection and response)**: sumber alert yang tugas turunannya dapat diotomatisasi SOAR.
- **Assess the alert**: menilai validitas alert melalui log data dan metrik.
- **Log data**: catatan aktivitas yang dianalisis untuk memvalidasi alert.
- **Containment**: membatasi penyebaran insiden, mis. isolasi sistem terinfeksi.
- **Eradication and recovery**: menghapus jejak insiden dan memulihkan sistem.
- **Clean backup**: cadangan data yang dibuat sebelum insiden.
- **Post-incident activity**: laporan akhir dan tindak lanjut setelah insiden selesai.
- **Coordination**: pelaporan dan berbagi informasi sesuai standar.
- **Stakeholders**: pihak berkepentingan yang menerima laporan akhir.
- **Security posture**: tingkat kesiapan keamanan organisasi.
- **Ransomware, malware, DDoS**: contoh jenis serangan dengan playbook khusus.
- **IOC**: indikator yang dicocokkan pada dashboard SIEM (IOC domain matches).
- **Privacy engineer**: peran yang menanamkan privasi sejak desain produk.
- **Soft skills**: kemampuan berelasi; lebih penting dari skill teknis menurut video.
- **Diversity of thought**: keberagaman cara berpikir yang memperkaya desain produk.

## 8. Tools & Platform Rujukan

Materi tidak menyertakan URL eksplisit.

- **SIEM tools**: deteksi dan alert keamanan; contoh perilaku pengguna tidak biasa.
- **SOAR tools**: otomatisasi tugas berulang dan respons awal.
- **MDR**: layanan deteksi dan respons terkelola yang menjadi sumber alert.
- **Federal Bureau of Investigation (FBI)**: otoritas AS tujuan pelaporan kejahatan siber.

## 9. Catatan Ringkas untuk Ditulis Tangan

### 9.1 Playbook

- Playbook / runbook — panduan tindakan saat insiden
- Fungsi — struktur, compliance, komunikasi, dokumentasi
- Isi — flow chart, tabel, siapa-apa-kapan
- Per jenis insiden — ransomware, malware, DDoS
- Living documents — sering diupdate, bukan permanen
- Belajar dari insiden lama — update playbook, security posture naik

### 9.2 Tools

- SIEM — deteksi dan alert
- SOAR — otomatisasi tugas berulang (dari SIEM / MDR)
- MDR — managed detection and response
- Contoh SOAR — login gagal berulang, akun diblokir otomatis, lalu analis ikut playbook
- Contoh SIEM — perilaku pengguna tidak biasa, lalu analis ikut playbook

### 9.3 Alur Alert SIEM (Malware)

- 1 Assess the alert — validasi via log data dan metrik
- 2 Containment — isolasi sistem terinfeksi
- 3 Eradication and recovery — hapus jejak, restore OS, data dari clean backup
- 4 Post-incident activity — laporan akhir ke stakeholders, lapor FBI / otoritas
- Coordination — pelaporan dan berbagi informasi sesuai standar
- Langkah pertama — selalu assess, bukan containment atau lapor

### 9.4 Dashboard SIEM

- IOC domain matches — domain, categories, sources, assets, confidence, severity
- Recent alerts — alerts, asset, user, alert names, first seen, last seen, severity, sources

### 9.5 Privacy Engineer

- Privacy engineer — privasi tertanam sejak awal produk
- Fokus — user trust, data sensitif, user informed
- Soft skills — lebih penting dari skill teknis
- Diversity of thought — produk lebih equitable
- Contoh jurnalisme — "ingin muncul di The Times?"
- STEM — butuh suara beragam
- Analis pemula — sering pakai playbook
