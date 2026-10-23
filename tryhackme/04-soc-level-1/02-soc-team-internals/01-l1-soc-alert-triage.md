# Resume Room SOC Alert Triage (TryHackMe) - 21 Oktober 2026

Resume ini merangkum seluruh materi room: konsep alert, properti alert, prioritisasi, dan alur triase alert oleh analis SOC L1. Room ini tidak memakai command terminal, sehingga praktiknya dilakukan sepenuhnya melalui SOC dashboard (TryHackMe SIEM).

Tujuan pembelajaran room: memahami konsep alert SOC, mengenal field/status/klasifikasi alert, melakukan triase sebagai analis L1, berlatih dengan alert nyata dan alur kerja SOC, serta persiapan menuju SOC Simulator dan sertifikasi SAL1.

Prasyarat: memahami serangan umum pada network, Windows, dan Linux, serta mengetahui peran dan tugas SOC, terutama analis L1.


## 1. Konsep Dasar Alert

### 1.1 Dari Event Menjadi Alert

Alur terbentuknya alert berjalan berurutan:

1. **Event** terjadi, misalnya login user, peluncuran proses, atau download file.
2. Sistem (OS, firewall, atau penyedia cloud) **mencatat (log)** event tersebut.
3. Seluruh log dikirim ke solusi keamanan seperti **SIEM** atau **EDR**.
4. Solusi keamanan menghasilkan **alert** ketika sebuah event atau rangkaian event tertentu terjadi.

Tim SOC bisa menerima jutaan log per hari dari ribuan sistem. Sebagian besar event adalah aktivitas normal, hanya sebagian kecil yang perlu perhatian.

### 1.2 Definisi dan Fungsi Alert

**Alert** adalah notifikasi yang dihasilkan solusi keamanan saat event atau rangkaian event tertentu terjadi. Fungsinya menyaring log mentah sehingga analis hanya menangani event yang mencurigakan atau anomali. Dengan alert, analis melakukan triase puluhan alert per hari, bukan jutaan log mentah.

Alert Time biasanya muncul beberapa menit setelah waktu event sebenarnya (contoh: event 15:32, alert 15:35).

### 1.3 Alert Management Platforms

Alert dikelola melalui beberapa jenis platform:

- **SIEM** (contoh: Splunk ES, Elastic): memiliki kemampuan manajemen alert yang solid dan menjadi pilihan terbaik bagi sebagian besar tim SOC.
- **EDR atau NDR** (contoh: MS Defender, CrowdStrike): punya dashboard alert sendiri, tetapi lebih disarankan memakai SIEM atau SOAR.
- **SOAR** (contoh: Splunk SOAR, Cortex SOAR): dipakai tim SOC besar untuk mengumpulkan dan memusatkan alert dari banyak solusi.
- **ITSM** (contoh: Jira, TheHive): sistem tiket kustom yang dipakai sebagian tim. Board Kanban di materi diambil dari Trello, tool sederhana yang bisa diadaptasi untuk kebutuhan ITSM.

### 1.4 Alert Dashboard (Board Kanban)

Contoh dashboard berbentuk board dengan lima kolom: **New**, **In Progress**, **True Positives**, **False Positives**, dan **Archive**. Tiap kartu mewakili satu alert dengan informasi waktu pembuatan, prioritas, dan sumber (SIEM, IDS, Email). Contoh nama alert di board: Email Marked as Phishing, Unusual Gmail Login Location, Unapproved Mimikatz Usage, Windows RDP Bruteforce, Potential Data Exfiltration, Adware Infection.


## 2. Peran dalam Triase Alert

Analis L1 adalah garis pertahanan pertama dan paling banyak bekerja dengan alert. Jumlah alert yang diterima berkisar dari nol hingga ratusan per hari, dan setiap alert berpotensi mengungkap serangan. Seluruh tim SOC terlibat dalam triase:

- **SOC L1 analyst**: meninjau alert, membedakan yang berbahaya dari yang tidak, dan memberi tahu L2 jika ada ancaman nyata.
- **SOC L2 analyst**: menerima alert yang dieskalasi L1, melakukan analisis lebih dalam dan remediasi.
- **SOC engineer**: memastikan alert memuat informasi yang cukup agar triase efisien.
- **SOC manager**: memantau kecepatan dan kualitas triase agar serangan nyata tidak terlewat.


## 3. Properti Alert

Delapan properti utama yang ada pada hampir semua alert. Detail tampilan berbeda di tiap SIEM atau solusi keamanan.

### 3.1 Daftar Properti

**1. Alert Time.** Waktu pembuatan alert, biasanya beberapa menit setelah event asli.

**2. Alert Name.** Ringkasan kejadian, berdasarkan nama detection rule. Contoh: Unusual Login Location, Email Marked as Phishing, Windows RDP Bruteforce, Potential Data Exfiltration.

**3. Alert Severity.** Tingkat urgensi alert. Awalnya ditetapkan detection engineer, tetapi bisa diubah analis bila perlu. Tingkatannya: Low/Informational, Medium/Moderate, High/Severe, Critical/Urgent.

**4. Alert Status.** Menunjukkan apakah ada yang sedang menangani alert atau triase sudah selesai. Status umum: New/Unassigned, In Progress/Pending, Closed/Resolved, serta sering ada status kustom lain.

**5. Alert Verdict.** Disebut juga alert classification, menjelaskan apakah alert adalah ancaman nyata atau noise. Nilainya: True Positive/Real Threat, False Positive/No Threat, dan sering ada verdict kustom.

**6. Alert Assignee.** Analis yang ditugaskan atau menugaskan diri untuk meninjau alert. Sering disebut alert owner, dan bertanggung jawab atas alert yang ditanganinya.

**7. Alert Description.** Penjelasan isi alert, biasanya terdiri dari tiga bagian: logika rule pembuat alert, alasan aktivitas tersebut bisa mengindikasikan serangan, dan (opsional) cara melakukan triase alert tersebut.

**8. Alert Fields.** Komentar analis SOC dan nilai-nilai yang memicu alert, misalnya hostname terdampak, command line yang dimasukkan, dan banyak lagi tergantung jenis alert.

### 3.2 Contoh Alert dengan Field

Alert Double-Extension File Creation (severity High) memiliki description berupa deteksi pembuatan file berekstensi ganda seperti `*.pdf.exe` atau `*.gif.lnk`, yang sering dipakai hacker pada serangan phishing untuk mengelabui pengguna agar membuka executable berbahaya. Field-nya: Host, Process Name, Process User, Target File, File MotW, dan File MD5.

**MotW (Mark of the Web)** menunjukkan asal file diunduh, sehingga berguna untuk menilai sumber file.

### 3.3 Dua Istilah Hasil Verdict

- **True Positive**: alert benar-benar ancaman nyata.
- **False Positive**: alert salah deteksi atau aktivitas normal yang tidak berbahaya.


## 4. Prioritisasi Alert

**Alert Prioritisation** adalah proses menentukan alert mana yang diambil lebih dulu. Proses ini krusial agar ancaman terdeteksi tepat waktu saat antrian alert menumpuk. Aturan prioritisasi ditentukan masing-masing tim SOC dan biasanya diotomatisasi lewat logika pengurutan di SIEM atau EDR. Pendekatan umum yang paling sederhana ada tiga langkah.

### 4.1 Filter Alert

Hanya ambil alert yang baru, belum pernah dilihat, dan belum ditangani. Jangan mengambil alert yang sudah ditinjau analis lain atau sedang diinvestigasi rekan tim.

### 4.2 Urutkan Berdasarkan Severity

Urutan: Critical, High, Medium, lalu Low. Alasannya, detection engineer merancang rule sehingga alert critical lebih mungkin merupakan ancaman nyata dan berdampak jauh lebih besar dibanding medium atau low.

### 4.3 Urutkan Berdasarkan Waktu

Mulai dari alert paling lama hingga paling baru. Alasannya, jika dua alert sama-sama menyangkut breach, peretas pada breach yang lebih lama kemungkinan sudah mengambil data, sedangkan yang baru masih di tahap discovery.


## 5. Alert Triage

Peninjauan alert oleh analis SOC juga disebut alert handling, alert processing, alert investigation, atau alert analysis. Room ini memakai istilah **Alert Triage**.

### 5.1 Alur Referensi Triase

Alur ini merupakan referensi di TryHackMe labs dan bisa sedikit berbeda antar tim:

1. Prioritaskan alert dan pilih yang pertama.
2. Assign alert ke diri sendiri.
3. Pindahkan alert ke **In Progress**.
4. Baca nama dan description alert.
5. Catat field alert seperti host, IP, atau user.
6. Cek apakah **workbook** tersedia. Jika ya, ikuti langkah di workbook. Jika tidak, langsung investigasi aktivitas di SIEM.
7. Buat verdict akhir.
8. Putuskan apakah perlu eskalasi. Jika ya, eskalasi ke L2 lalu kembali memilih alert berikutnya. Jika tidak, tambahkan komentar alert.
9. Pindahkan alert ke **Closed**, lalu ulangi dari alert berikutnya.

### 5.2 Initial Actions

Tujuannya memastikan analis memegang kepemilikan alert, tidak mengganggu alert yang ditangani analis lain, dan siap untuk investigasi mendalam. Caranya: assign ke diri sendiri, pindahkan ke In Progress, lalu pelajari detail alert seperti nama, description, dan indikator kunci.

### 5.3 Investigation

Langkah paling kompleks. Analis menerapkan pengetahuan teknis dan pengalaman untuk menilai keabsahan aktivitas di log SIEM atau EDR.

**Workbook** (disebut juga playbook atau runbook) adalah instruksi investigasi untuk kategori alert tertentu yang dibuat sebagian tim untuk membantu L1. Bila tidak ada workbook, rekomendasi umumnya:

1. Pahami siapa yang terancam: user, hostname, cloud, network, atau website yang terdampak.
2. Catat tindakan yang dijelaskan alert, misalnya login mencurigakan, malware, atau phishing.
3. Tinjau event di sekitar alert, cari tindakan mencurigakan sesaat sebelum atau sesudahnya.
4. Gunakan platform threat intelligence atau sumber lain untuk memverifikasi dugaan.

### 5.4 Final Actions

Keputusan di tahap ini menentukan apakah potensi serangan berhasil ditemukan atau terlewat. Urutannya:

1. Tentukan alert berbahaya (True Positive) atau tidak (False Positive).
2. Siapkan komentar rinci berisi langkah analisis dan alasan verdict.
3. Kembali ke dashboard dan pindahkan alert ke status **Closed**.

**Escalation** dan **Commenting** dibahas lebih dalam di room berikutnya.

### 5.5 Catatan SOC Dashboard

Jika flag tidak muncul setelah triase, nilai yang diisi salah. Dashboard dapat direset dengan tombol **Restart** di kanan atas TryHackMe SIEM.


## 6. Contoh Penerapan Penilaian Alert

Tiga alert pada dashboard latihan menunjukkan pola penalaran yang bisa dipakai kembali.

### 6.1 Potential Data Exfiltration (Critical)

Rule mendeteksi 5 GB atau lebih data dikirim dari satu perangkat ke satu tujuan dalam sehari. Penilaian: pola exfiltration biasanya satu arah (Sent jauh lebih besar dari Received), sedangkan video call dua arah dan seimbang. Tujuan adalah domain resmi Zoom, dan source network berasal dari ruang meeting. Verdict: **False Positive**.

### 6.2 Double-Extension File Creation (High)

File `cats2025.mp4.exe` diunduh oleh `chrome.exe` dari domain berTLD `.monster` yang menjanjikan konten gratis. Ekstensi ganda menyamarkan executable sebagai video, dan domain tidak tepercaya dipakai sebagai umpan. Verdict: **True Positive**.

### 6.3 Download from GitHub Repository (Low)

Rule mendeteksi semua download dari GitHub secara luas, sehingga L1 perlu menyaring kasus spesifiknya. URL adalah repositori resmi React, host berasal dari divisi IT, dan network berlabel VPN/Developers. Verdict: **False Positive**.

### 6.4 Pola Umum

Penilaian yang baik menggabungkan beberapa konteks: arah dan volume data, reputasi domain atau TLD, nama file dan ekstensi, proses pembuat, serta asal user, host, dan network.


## 7. Penutup Room

Menutup alert sebagai True Positive tidak otomatis menghentikan serangan, tetapi itu langkah awal yang penting. Room lanjutan membahas penulisan komentar alert dan case reporting, prosedur eskalasi yang benar, serta tindakan analis L2 setelah eskalasi.


## 8. Glossary

**Alert**: notifikasi dari solusi keamanan saat event atau rangkaian event tertentu terjadi.

**Alert Prioritisation**: proses menentukan alert mana yang ditangani lebih dulu.

**Alert Triage**: proses peninjauan alert oleh analis SOC (alert handling, processing, investigation, analysis).

**Assignee / Alert Owner**: analis yang bertanggung jawab atas alert.

**Closed**: status alert yang triasenya sudah selesai.

**EDR**: solusi deteksi dan respons pada endpoint.

**Escalation**: pengiriman alert ke L2 untuk analisis dan remediasi lebih dalam.

**False Positive**: alert yang bukan ancaman nyata.

**ITSM**: sistem manajemen tiket (contoh: Jira, TheHive).

**In Progress**: status alert yang sedang ditangani.

**L1 / L2 Analyst**: analis tier pertama (triase) dan tier kedua (analisis mendalam dan remediasi).

**MotW (Mark of the Web)**: informasi asal unduhan sebuah file.

**NDR**: solusi deteksi dan respons pada jaringan.

**SIEM**: sistem pengumpul dan penganalisis log yang menghasilkan alert.

**SOAR**: platform untuk mengumpulkan dan memusatkan alert dari banyak solusi.

**Severity**: tingkat urgensi alert (Low, Medium, High, Critical).

**True Positive**: alert yang merupakan ancaman nyata.

**Verdict**: klasifikasi akhir alert (True Positive atau False Positive).

**Workbook (Playbook / Runbook)**: panduan investigasi untuk kategori alert tertentu.


## 9. Tools & Platform Rujukan

Room tidak mencantumkan URL eksplisit untuk tools di bawah, sehingga hanya nama dan fungsinya yang ditulis.

- **TryHackMe SIEM / SOC Dashboard**: dashboard latihan untuk triase alert di room ini.
- **SOC Simulator (TryHackMe)**: simulasi kerja SOC, target lanjutan setelah room ini.
- **SAL1 (TryHackMe)**: sertifikasi analis SOC level 1 yang disiapkan oleh room ini.
- **Splunk ES**: SIEM untuk manajemen alert.
- **Elastic**: SIEM untuk manajemen alert.
- **MS Defender**: EDR dengan dashboard alert sendiri.
- **CrowdStrike**: EDR dengan dashboard alert sendiri.
- **Splunk SOAR**: SOAR untuk memusatkan alert dari banyak solusi.
- **Cortex SOAR**: SOAR untuk memusatkan alert dari banyak solusi.
- **Jira**: ITSM untuk manajemen tiket alert.
- **TheHive**: ITSM untuk manajemen tiket alert.
- **Trello**: tool board sederhana yang dipakai sebagai contoh dashboard alert.
- **Platform threat intelligence**: disebut secara umum untuk memverifikasi dugaan saat investigasi, tanpa nama spesifik di materi.


## 10. Catatan Ringkas untuk Ditulis Tangan

### 10.1 Konsep Alert

Event — kejadian (login, proses, download)
Log — catatan event oleh OS/firewall/cloud
Alur — event, log, kirim ke SIEM/EDR, alert
Alert — notifikasi saat event/rangkaian event terjadi
Fungsi alert — saring jutaan log jadi puluhan alert
Alert Time — beberapa menit setelah event asli

### 10.2 Platform

SIEM — Splunk ES, Elastic; terbaik untuk kebanyakan SOC
EDR/NDR — MS Defender, CrowdStrike; lebih baik pakai SIEM/SOAR
SOAR — Splunk SOAR, Cortex SOAR; pusatkan alert tim besar
ITSM — Jira, TheHive; tiket kustom (Trello sebagai contoh)
Kolom board — New, In Progress, True Positives, False Positives, Archive

### 10.3 Peran Tim

L1 — tinjau alert, bedakan baik/buruk, lapor L2 jika nyata
L2 — terima eskalasi, analisis dalam, remediasi
Engineer — pastikan info alert cukup
Manager — pantau kecepatan dan kualitas triase

### 10.4 Properti Alert

1 Time — waktu alert dibuat
2 Name — ringkasan dari nama rule
3 Severity — urgensi (Low, Medium, High, Critical), bisa diubah analis
4 Status — New, In Progress, Closed
5 Verdict — True Positive / False Positive
6 Assignee — owner alert
7 Description — logika rule, alasan bahaya, cara triase
8 Fields — nilai pemicu (host, user, command line)
MotW — asal unduhan file
True Positive — ancaman nyata
False Positive — bukan ancaman

### 10.5 Prioritisasi

Langkah 1 Filter — hanya alert baru, belum ditangani
Langkah 2 Severity — Critical, High, Medium, Low
Langkah 3 Waktu — dari yang paling lama
Alasan waktu — breach lama sudah dump data, yang baru baru discovery

### 10.6 Alur Triase

1 Prioritaskan dan pilih
2 Assign ke diri sendiri
3 Pindah In Progress
4 Baca name dan description
5 Catat field (host, IP, user)
6 Workbook ada? ya: ikuti, tidak: investigasi di SIEM
7 Verdict akhir
8 Perlu eskalasi? ya: ke L2, tidak: tulis komentar
9 Closed, lanjut alert berikutnya

### 10.7 Investigasi

Workbook — playbook/runbook, panduan per kategori alert
Rekomendasi 1 — siapa terdampak (user, host, cloud, network, web)
Rekomendasi 2 — tindakan apa (login, malware, phishing)
Rekomendasi 3 — event sebelum dan sesudah alert
Rekomendasi 4 — threat intelligence untuk verifikasi
Final — tentukan verdict, tulis komentar, Closed
Restart — tombol reset dashboard jika flag tidak muncul

### 10.8 Pola Penilaian

Exfiltration — Sent jauh lebih besar dari Received (satu arah)
Video call — Sent dan Received seimbang (dua arah)
Double extension — .mp4.exe, executable menyamar jadi video
TLD murahan dan kata "free" — umpan malware
Rule luas (GitHub) — saring konteks: URL, user, host, network

### 10.9 Lanjutan

Task selesai — True Positive tidak menghentikan serangan
Room berikutnya — komentar alert, case reporting, eskalasi, aksi L2
Target — SOC Simulator dan sertifikasi SAL1
