# Resume Room: SOC Workbooks and Lookups

Tanggal: 21 Oktober 2026

Cakupan: Introduction, Assets & Identities, Network Diagrams, Workbooks Theory, Workbooks Practice, Conclusion.

Catatan: room ini tidak memuat command terminal atau switch/flag. Seluruh praktiknya berupa penyusunan workbook (drag and drop) pada situs interaktif.

---

## 1. Gambaran Umum Room

### 1.1 Tujuan Room

Triase alert adalah proses kompleks yang sering menuntut analis mengumpulkan informasi tambahan tentang karyawan atau server yang terdampak. Room ini membahas dua hal: **lookup** (cara cepat mengambil konteks user dan sistem) serta **SOC workbook** (panduan langkah investigasi yang terstruktur).

Learning objectives:

- Mengenal workbook investigasi SOC.
- Mengetahui di mana menemukan dan bagaimana memakai **asset inventory**.
- Memahami pentingnya **corporate network diagram**.
- Berlatih membangun workflow pada antarmuka interaktif.

### 1.2 Prasyarat

- Menyelesaikan room SOC L1 Alert Triage dan Alert Reporting.
- Berpengalaman menginvestigasi attack chain umum.
- Memahami dasar jaringan.
- Sebaiknya familiar dengan konsep **SOAR playbook**.

---

## 2. Identity dan Asset Inventory

### 2.1 Mengapa Konteks Dibutuhkan

Contoh skenario: pada shift malam muncul alert bahwa **G.Baker** login ke server **HQ-FINFS-02**, mengunduh file "Financial Report US 2024.xlsx", lalu membagikannya ke **R.Lund**. Untuk menentukan apakah aktivitas ini wajar, analis harus menjawab:

- Siapa G.Baker, apa jam kerja dan perannya?
- Apa fungsi dan lokasi HQ-FINFS-02, dan siapa yang boleh mengaksesnya?
- Mengapa R.Lund membutuhkan akses ke data finansial perusahaan?

Pertanyaan pertama dan ketiga dijawab oleh identity inventory, pertanyaan kedua oleh asset inventory.

### 2.2 Identity Inventory

**Identity inventory** adalah katalog akun karyawan (user account) dan akun layanan (service/machine account) beserta detailnya: privilege, kontak, dan peran di perusahaan. Gunanya memberi konteks pelaku aktivitas sehingga lebih mudah memutuskan aktivitas itu expected atau tidak.

Sumber identitas:

- **Active Directory** (on-prem AD, Entra ID): AD adalah database identitas dan paling umum dipakai SOC.
- **SSO Providers** (Okta, Google Workspace): alternatif cloud untuk AD, mudah untuk mengelola dan mencari user.
- **HR Systems** (BambooHR, SAP, HiBob): terbatas pada karyawan, tetapi biasanya memuat data karyawan paling lengkap.
- **Custom Solution** (CSV atau Excel): umum dipelihara sendiri oleh tim IT atau security.

Contoh data identitas dari room (kolom: nama, role, lokasi, akses):

- Gregory Baker (G.Baker): Chief Financial Officer, Europe/UK, akses VPN, HQ, FINANCE.
- Raymond Lund (R.Lund): US Financial Adviser, US/Texas, akses VPN, FINANCE.
- Kate Danner (K.Danner): Chief Technology Officer, Europe/UK, akses VPN, DA, HQ, AWS.
- svc-veeam-06: Backup Service Account, akses VEEAM, DMZ, HQ.
- svc-nginx-pp: Web App Service Account, akses DMZ.

Pada skenario di atas, CFO yang mengakses server finansial dan membagikan laporan ke US Financial Adviser konsisten dengan peran masing-masing, sehingga aktivitas tampak expected.

### 2.3 Asset Inventory

**Asset inventory** (disebut juga **asset lookup**) adalah daftar seluruh sumber daya komputasi dalam lingkungan IT organisasi. "Asset" bisa berarti software, hardware, atau karyawan, tetapi room ini hanya menekankan **server dan workstation**. Gunanya memberi konteks host, misalnya HQ-FINFS-02.

Sumber aset:

- **Active Directory**: selain identitas, juga database inventaris aset yang solid.
- **SIEM atau EDR** (Elastic, CrowdStrike): sebagian agent mengumpulkan informasi host yang dipantau.
- **MDM Solution** (MS Intune, Jamf MDM): kelas solusi khusus untuk mendata dan mengelola aset.
- **Custom Solution** (CSV atau Excel): sama seperti identity inventory, umum dipakai.

Contoh data aset dari room (kolom: hostname, lokasi, IP, OS, owner, fungsi):

- HQ-FINFS-02: UK Datacenter, 172.16.15.89, Windows Server 2022, Central IT, file server data finansial.
- HQ-ADDC-01: UK Datacenter, 172.16.15.10, Windows Server 2019, Central IT, primary AD domain controller.
- PC-891D: London Office, 192.168.5.13, Windows 11 Pro, Tech Support, PC stasioner untuk akuntan.
- L007694: Remote, MacOS 13, A.Kelly (DevOps), laptop korporat.
- L005325: Remote, MacOS 13, J.Eldridge (HR), laptop korporat.

---

## 3. Network Diagram

### 3.1 Konsep

**Network diagram** adalah skema visual yang menampilkan lokasi, subnet, dan koneksi di dalam organisasi. Fungsinya memberi makna pada IP yang muncul di log, khususnya pada perusahaan besar. Kompleksitas diagram berbeda-beda, tetapi tujuannya sama: membantu analis SOC memahami aktivitas jaringan yang mencurigakan.

### 3.2 Skenario Rangkaian Alert Firewall

Urutan kejadian pada skenario:

- 08:00: IP **103.61.240.174** berulang kali terhubung ke firewall korporat lewat port **TCP/10443**.
- 08:23: log firewall menunjukkan IP tersebut ditranslasi (NAT) menjadi IP internal **10.10.0.53**.
- 08:25: 10.10.0.53 memindai range **172.16.15.0/24**, tidak menemukan port terbuka.
- 08:32: IP yang sama memindai range **172.16.23.0/24**, serangan tampak masih berlangsung.

Pertanyaan yang harus dijawab: layanan apa di port 10443 dan mengapa ada yang terhubung ke sana; subnet mana tempat 10.10.0.53 berada dan mengapa IP itu menjangkau subnet lain.

### 3.3 Membaca Diagram

Elemen pada diagram room:

- Threat actor (103.61.240.174), VPN users, dan website visitors terhubung melalui internet ke **firewall**.
- Exposed ports pada firewall: `www.tryhatme.thm:80`, `www.tryhatme.thm:443`, `vpn.tryhatme.thm:10443`.
- Di balik firewall ada tiga subnet: **Database Subnet** (172.16.15.0/24), **VPN Subnet** (10.10.0.0/16), dan **Office Subnet** (172.16.23.0/24).

Karena port 10443 adalah layanan VPN dan 10.10.0.0/16 adalah VPN Subnet, jalur serangan direkonstruksi sebagai berikut:

- Threat actor melakukan **VPN brute force** ke `vpn.tryhatme.thm`.
- Setelah berhasil login VPN, ia mendapat IP dari VPN Subnet.
- Ia mencoba memindai Database Subnet, kemungkinan besar diblokir aturan firewall.
- Karena gagal, ia beralih ke Office Subnet untuk mencari target berikutnya.

---

## 4. SOC Workbook

### 4.1 Definisi

**SOC workbook** (disebut juga playbook, runbook, atau workflow) adalah dokumen terstruktur yang mendefinisikan langkah-langkah untuk menginvestigasi dan menanggulangi ancaman tertentu secara efisien dan konsisten.

Analis L1 dianggap junior dan tidak diharapkan mentriase semua skenario serangan dengan sempurna, sehingga analis senior menyiapkan workbook. L1 direkomendasikan, kadang diwajibkan, mengikuti workbook secara persis untuk menghindari kesalahan dan merampingkan analisis. Workbook memastikan semua langkah analisis selalu dijalankan dan keputusan tidak dibuat tanpa bukti cukup.

### 4.2 Tiga Tahap Workbook

- **Enrichment**: memakai Threat Intelligence dan identity inventory untuk mengumpulkan informasi tentang entitas terdampak.
- **Investigation**: berdasarkan data yang terkumpul dan log SIEM, menentukan verdict apakah aktivitas expected.
- **Escalation**: mengeskalasi alert ke L2 atau mengonfirmasi aktivitas kepada user bila perlu.

Sebagian besar diagram workbook dilengkapi panduan tekstual rinci dan tautan ke sumber daya yang disebut.

### 4.3 Contoh: Unusual Login Location Workbook

Enrichment:

- Terima alert.
- Gunakan BambooHR untuk mengetahui lokasi user yang seharusnya.
- Lookup IP login di layanan Threat Intelligence.
- Lookup IP login di layanan anonymisation detection.

Investigation:

- Keputusan: apakah IP login terkonfirmasi malicious? Jika ya, menuju Escalation.
- Jika tidak, gunakan Splunk untuk melihat aksi user setelah login.
- Keputusan: ada aksi mencurigakan seperti MFA reset? Jika ya, menuju Escalation.
- Jika tidak, jalankan dashboard Splunk "Login Timeline" untuk 90 hari.
- Keputusan: login via VPN atau dari lokasi atipikal? Jika ya, hubungi user dan lanjutkan sesuai responsnya.
- Jika tidak, keputusan: waktu login atipikal bagi user? Jika tidak, tutup sebagai **False Positive**; jika ya, hubungi user.

Escalation:

- Tulis alert report dan eskalasikan ke analis L2.
- Hasil kontak user dapat berujung ke Escalation atau penutupan sebagai False Positive, tergantung respons user.

### 4.4 Pendekatan Tim

Setiap tim berbeda dalam menyusun workbook. Sebagian punya ratusan workbook kompleks per detection rule (lebih mirip SOAR playbook ketimbang panduan manusia); sebagian hanya punya beberapa workbook tingkat tinggi untuk vektor serangan paling umum dan lebih mengandalkan pengalaman L1. Dalam kedua kasus, analis L1 harus mampu membagi investigasi menjadi blok modular dan membangun workbook sederhana.

---

## 5. Praktik: Tiga Workbook

Ketiga workbook memiliki pola yang sama: Start, tiga langkah linear, satu keputusan, cabang YES (kumpulkan bukti, tulis laporan, Escalate to L2), dan cabang NO (tulis komentar, Close as FP). Pada setiap workbook tersedia opsi lebih banyak daripada slot, sehingga ada distractor.

### 5.1 Workbook 1: Email Analysis (External Email with Script or Binary Attachment)

Alur linear:

- Ambil alih alert, gunakan identity inventory untuk konteks penerima email.
- Investigasi email dengan EML analyser: SPF/DKIM, konten, reputasi domain pengirim.
- Analisis lampiran: sandbox untuk binary, manual code review untuk script.

Keputusan: lampiran berbahaya, asal dipalsukan, atau email tidak expected untuk peran penerima?

- YES: kumpulkan triage evidence (daftar penerima, laporan sandbox, hasil EML analyser, indikator lain), tulis alert report untuk L2 dengan bukti terlampir, Escalate to L2.
- NO: tulis komentar singkat yang menjelaskan keyakinan bahwa email aman dan expected, Close as FP.

### 5.2 Workbook 2: PowerShell Analysis (Executable File Download using PowerShell)

Alur linear:

- Assign alert ke diri sendiri, ambil konteks mesin terdampak dari asset inventory.
- Gunakan threat intel untuk menganalisis URL, lakukan analisis statis pada executable yang diunduh.
- Bangun process tree untuk mengetahui parent process dan akun yang menjalankan PowerShell.

Keputusan: file berbahaya, diunduh dari sumber tidak tepercaya, atau berasal dari proses mencurigakan?

- YES: simpan hasil pencarian SIEM "[WIN] Process Tree" dan "[WIN] Login Timeline", tulis alert report untuk L2 dengan bukti, asumsi, dan hasil SIEM, Escalate to L2.
- NO: tulis komentar yang menjelaskan verdict dan hubungi SOC engineer untuk tuning detection rule, Close as FP.

Distractor: menghubungi user untuk konfirmasi penggunaan PowerShell; assign alert ke system owner dan menaikkan severity ke Critical.

### 5.3 Workbook 3: Network Analysis (Port Scanning From Internal IP)

Alur linear:

- Assign alert ke diri sendiri, ambil konteks IP dari network map dan asset inventory.
- Daftar port yang dipindai IP, tentukan layanan yang mungkin berjalan di port tersebut.
- Jika sumber adalah tool korporat (Nessus, Zabbix, dan sejenisnya), verifikasi apakah pola scan expected.

Keputusan: IP sumber tidak terdaftar, atau pola scan menyerupai upaya Discovery yang berbahaya?

- YES: kumpulkan bukti (rentang waktu serangan, port yang dipindai, konteks IP), tulis alert report berisi verdict dan bukti untuk review L2, Escalate to L2.
- NO: tulis alert comment berisi verdict dan hubungi SOC engineer untuk tuning rule, Close as FP.

Distractor: memakai EDR untuk mencari alert exfiltration; memindai port terbuka pada IP sumber.

### 5.4 Pola yang Dapat Dipakai Ulang

- Langkah awal selalu mengambil kepemilikan alert dan mengumpulkan konteks dari inventory atau network map.
- Langkah tengah adalah analisis teknis spesifik jenis alert (email, proses, jaringan).
- Jalur eskalasi selalu mengumpulkan bukti lalu menulis laporan untuk L2.
- Jalur False Positive selalu mendokumentasikan alasan dan, bila relevan, meminta tuning rule.

---

## 6. Kesimpulan

Gunakan lookup yang tersedia seperti asset inventory dan network map untuk memahami alert dengan lebih baik, serta dorong tim untuk menerapkan dan memelihara workbook agar operasi SOC lebih ramping dan sederhana.

---

## 7. Glossary

- **Alert triage**: proses menilai alert untuk menentukan apakah aktivitas aman atau perlu ditindaklanjuti.
- **Identity inventory**: katalog akun user dan service account beserta privilege, kontak, dan peran.
- **Asset inventory / asset lookup**: daftar server dan workstation (dan sumber daya komputasi lain) organisasi.
- **Active Directory (AD)**: database identitas sekaligus inventaris aset; varian: on-prem AD, Entra ID.
- **SSO Provider**: layanan identitas berbasis cloud, contoh Okta dan Google Workspace.
- **HR System**: sistem data karyawan, contoh BambooHR, SAP, HiBob.
- **MDM**: solusi pendataan dan pengelolaan perangkat, contoh MS Intune dan Jamf MDM.
- **SIEM**: platform pengumpul dan pencarian log keamanan.
- **EDR**: agent pemantau endpoint yang juga dapat menyediakan data host.
- **Service account**: akun mesin atau layanan, bukan akun manusia.
- **Network diagram**: skema visual lokasi, subnet, dan koneksi jaringan organisasi.
- **Subnet**: segmen jaringan, contoh Database, VPN, dan Office Subnet.
- **Exposed ports**: port yang dibuka firewall ke publik.
- **NAT / translation**: penerjemahan IP eksternal menjadi IP internal pada firewall.
- **VPN brute force**: percobaan tebak kredensial berulang ke layanan VPN.
- **SOC workbook (playbook, runbook, workflow)**: dokumen langkah investigasi dan remediasi yang terstruktur.
- **SOAR playbook**: playbook otomatisasi respons keamanan.
- **Enrichment, Investigation, Escalation**: tiga tahap logis workbook.
- **Verdict**: keputusan akhir atas sebuah alert.
- **False Positive (FP)**: alert yang ternyata bukan ancaman.
- **L1 / L2**: analis SOC tingkat pertama (junior, triase) dan tingkat kedua (menerima eskalasi).
- **Triage evidence**: bukti yang dikumpulkan saat triase untuk dilampirkan ke laporan.
- **EML analyser**: tool untuk menganalisis file email.
- **Sandbox**: lingkungan terisolasi untuk menganalisis binary.
- **Process tree**: pohon hubungan parent-child proses.
- **Detection rule tuning**: penyesuaian rule deteksi untuk mengurangi false positive.
- **Discovery**: taktik penyerang memetakan jaringan, contoh port scanning.

---

## 8. Tools & Platform Rujukan

Room tidak menyebutkan URL eksplisit untuk satu pun tool di bawah ini.

- Active Directory (on-prem AD, Entra ID): sumber identitas dan aset.
- Okta: SSO provider untuk identitas.
- Google Workspace: SSO provider untuk identitas.
- BambooHR: HR system untuk data karyawan dan lokasi yang diharapkan.
- SAP: HR system.
- HiBob: HR system.
- Elastic: SIEM atau EDR yang menyediakan informasi host.
- CrowdStrike: EDR yang menyediakan informasi host.
- MS Intune: MDM untuk inventaris aset.
- Jamf MDM: MDM untuk inventaris aset.
- Splunk: SIEM untuk daftar aksi user dan dashboard "Login Timeline".
- Threat Intelligence services: lookup reputasi IP, URL, dan domain.
- Anonymisation detection services: mengecek apakah IP berasal dari VPN, proxy, atau sejenisnya.
- EML analyser: analisis SPF/DKIM, konten, dan reputasi domain email.
- Sandbox: analisis dinamis file binary.
- Nessus: scanner kerentanan korporat yang dapat menjadi sumber scan sah.
- Zabbix: tool monitoring korporat yang dapat menjadi sumber scan sah.
- SIEM searches "[WIN] Process Tree" dan "[WIN] Login Timeline": pencarian SIEM untuk process tree dan timeline login Windows.
- Excel / CSV: inventory buatan sendiri.

---

## 9. Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar
- Alert triage — kumpulkan konteks user/host sebelum verdict
- Lookup — cara cepat ambil konteks user dan sistem
- Workbook — panduan langkah investigasi terstruktur

### Identity Inventory
- Identity inventory — katalog user + service account, privilege, kontak, peran
- Tujuan — tahu siapa pelaku, jam kerja, peran
- Active Directory — database identitas, paling umum di SOC
- SSO (Okta, Google Workspace) — AD versi cloud
- HR (BambooHR, SAP, HiBob) — hanya karyawan, data paling lengkap
- Custom (CSV/Excel) — dibuat tim IT/security
- Service account — akun mesin, bukan manusia

### Asset Inventory
- Asset inventory/lookup — daftar server + workstation
- Tujuan — tahu fungsi, lokasi, owner, OS, IP host
- Active Directory — juga inventaris aset
- SIEM/EDR (Elastic, CrowdStrike) — agent kumpulkan info host
- MDM (Intune, Jamf) — khusus kelola aset
- Custom (CSV/Excel) — umum dipakai
- Contoh — HQ-FINFS-02, file server finansial, UK Datacenter, 172.16.15.89

### Network Diagram
- Network diagram — peta lokasi, subnet, koneksi
- Exposed ports — 80, 443 (www), 10443 (vpn)
- Subnet — Database 172.16.15.0/24, VPN 10.10.0.0/16, Office 172.16.23.0/24
- NAT — 103.61.240.174 jadi 10.10.0.53
- Attack path — VPN brute force, dapat IP VPN, scan Database (diblok), pindah Office

### SOC Workbook
- Nama lain — playbook, runbook, workflow
- Pembuat — analis senior, untuk L1
- L1 — ikuti workbook persis
- Enrichment — TI + identity inventory
- Investigation — data + log SIEM, buat verdict
- Escalation — ke L2 atau konfirmasi ke user
- Unusual Login — IP malicious? aksi mencurigakan (MFA reset)? Login Timeline 90 hari, VPN/lokasi atipikal? waktu atipikal?, kontak user atau FP

### Workbook Praktik
- WB1 Email — identity inventory, EML analyser, sandbox/code review, kumpulkan evidence, report L2, atau komentar lalu FP
- WB2 PowerShell — asset inventory, TI + static analysis, process tree, simpan SIEM results, report L2, atau komentar + tuning lalu FP
- WB3 Network — network map + asset inventory, list port + service, cek Nessus/Zabbix, kumpulkan evidence, report L2, atau komentar + tuning lalu FP
- Pola — ambil alert, konteks, analisis, keputusan, YES eskalasi, NO FP
- Distractor — ada opsi tak terpakai di tiap workbook

### Command
- Tidak ada command/switch di room ini

### Prinsip Penutup
- Pakai lookup (asset inventory, network map)
- Dorong tim buat dan rawat workbook
