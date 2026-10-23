# Resume Room: SOC L1 Alert Reporting, Escalation, dan Communication (21 Oktober 2026)

Room lanjutan dari room "SOC L1 Alert Triage". Fokusnya: apa yang dilakukan analis L1 setelah triase alert selesai, yaitu **alert reporting**, **escalation**, dan **communication**.

Prasyarat room: sudah menyelesaikan room SOC L1 Alert Triage, paham serangan umum, dan paham tanggung jawab SOC L1.

Tujuan room: memahami kebutuhan reporting dan eskalasi, menulis alert comment atau case report dengan benar, mempelajari metode eskalasi dan praktik komunikasi, menerapkannya di lingkungan simulasi, serta lebih percaya diri di SOC Simulator dan sertifikasi SAL1.


## 1. Latar Belakang dan Konsep Inti

### 1.1 Mengapa Reporting, Escalation, dan Communication Dibutuhkan

Selama atau setelah triase, L1 bisa menghadapi dua situasi: tidak yakin cara mengklasifikasikan alert (butuh senior atau informasi dari system owner), atau berhadapan dengan serangan dan breach nyata yang butuh penanganan dan remediasi segera. Tiga skill berikut menjawab kedua situasi tersebut:

- **Alert reporting**: mendokumentasikan hasil investigasi agar konteks aktivitas terjaga dan tersedia bagi L2.
- **Alert escalation**: meneruskan alert True Positive ke L2 agar ancaman diremediasi tepat waktu.
- **Communication**: koordinasi SOC dengan departemen lain secara jelas dan efektif.

### 1.2 Alert Funnel (Corong Alert)

Alur alert dari atas ke bawah, semakin ke bawah jumlahnya makin sedikit tetapi makin penting:

- **L1**: menerima sekitar 100 SIEM alert dari SIEM, EDR, atau platform ticketing. Tugasnya noise filtering dan eskalasi ancaman nyata ke L2. Contoh alert masuk: Atypical VPN Login, MS Defender Alert, RDP Bruteforce.
- **L2**: menerima sekitar 10 True Positive. Tugasnya deep investigation dan remediasi, serta memulai IR (Incident Response) bila perlu.
- **DFIR**: menerima sekitar 1 incident. Tugasnya incident handling, forensik, dan komunikasi lintas tim.
- **Tujuan akhir**: melindungi data perusahaan.

Sebagian besar alert ditutup sebagai False Positive atau selesai di level L1. Alert kompleks dan berbahaya dikirim ke L2, yang menangani remediasi sebagian besar breach.


## 2. Alert Reporting

### 2.1 Definisi

Sebelum menutup alert atau meneruskannya ke L2, L1 mungkin harus membuat laporan. Tergantung standar tim dan severity, laporan bisa berupa komentar singkat atau dokumentasi investigasi yang detail beserta semua bukti relevan. Laporan detail sangat penting untuk alert True Positive yang membutuhkan eskalasi.

### 2.2 Tiga Tujuan Alert Report

**Memberi konteks untuk eskalasi.** Laporan yang baik menghemat waktu L2 dan membantu mereka cepat memahami kejadian.

**Menyimpan temuan sebagai arsip.** Log SIEM mentah disimpan 3 sampai 12 bulan, sedangkan alert disimpan tanpa batas waktu. Karena itu seluruh konteks sebaiknya ditulis di dalam alert.

**Meningkatkan kemampuan investigasi.** Jika tidak bisa menjelaskan sesuatu dengan sederhana, berarti belum cukup paham. Menulis laporan melatih kemampuan merangkum alert.

### 2.3 Format Laporan: Pendekatan Five Ws

Posisikan diri sebagai L2, anggota tim DFIR, atau profesional IT yang perlu memahami alert. Laporan minimal memuat:

- **Who**: user mana yang login, menjalankan command, atau mengunduh file.
- **What**: aksi atau urutan kejadian (event sequence) yang dilakukan.
- **When**: kapan aktivitas mencurigakan dimulai dan berakhir.
- **Where**: perangkat, IP, atau website yang terlibat.
- **Why**: alasan di balik verdict akhir. Ini W yang paling penting.

### 2.4 Alert Report Checklist

- Semua 5W tercakup dalam laporan.
- Laporan jelas dan presisi.
- Laporan membentuk satu timeline yang utuh.
- Bukti dan indikator dilampirkan.
- Alasan eskalasi ke L2 dijelaskan.

### 2.5 Contoh Laporan: Double-Extension File Creation

Field alert: Status Closed, Verdict True Positive, Severity High, Assignee J.Adams (L2).

Isi komentar secara ringkas: pukul 13:56 UTC user S.Conway (HR Coordinator) mengakses website phishing `freecatvideoshd[.]monster` dari laptop Windows LPT-HR-009. Pukul 13:58 UTC user mengunduh file `cats2025.mp4.exe` (double extension, disangka video sah). Laporan VirusTotal mengonfirmasi file tersebut adalah malware LummaStealer yang bertujuan mengeksfiltrasi data sensitif dan membangun kanal C2. Alert dieskalasi ke L2 disertai tautan temuan SIEM.

Pola yang bisa ditiru: urutan waktu jelas, identitas user dan perangkat disebut, indikator (domain, nama file) dicantumkan, bukti dari tools eksternal disertakan, alasan eskalasi dinyatakan.

### 2.6 Praktik: Mengisi Alert di SOC Dashboard

Alert dikerjakan di TryHackMe SIEM dengan kolom Time, Name, Severity, Status, Verdict, Assignee, dan Actions. Field edit: Assignee, Status, Severity, Verdict, dan Analyst Comment. Alur kerjanya:

- Assign alert ke diri sendiri, pindahkan status ke **In Progress**.
- Tulis Analyst Comment berdasarkan Five Ws. Indikator kualitas ditampilkan sebagai **Authenticity** (persen).
- Set verdict (True Positive atau False Positive).

Catatan: dalam simulasi ini flag atau notifikasi keberhasilan muncul saat status alert berada di In Progress dengan komentar yang memenuhi kriteria, bukan saat status sudah Closed.

### 2.6.1 Contoh Analisis Alert Email Phishing

Alert "Email Marked as Phishing after Delivery": email dari `support@microsoft.com` ke IT Manager dengan subject "Important Update: Microsoft Teams Pricing Increase", body berisi "600% price increase; urgent notice; download the report", attachment `REPORT.rar`, dan hasil SPF/Fail serta DKIM/Fail.

Poin analisis: SPF dan DKIM gagal menandakan alamat pengirim dipalsukan (**spoofed**); kata-kata mendesak adalah **social engineering** kategori urgensi; attachment arsip `.rar` berpotensi berisi malware. Verdict: True Positive.

**SPF** (Sender Policy Framework) memverifikasi apakah server pengirim berhak mengirim email atas nama domain tersebut. **DKIM** (DomainKeys Identified Mail) memverifikasi tanda tangan digital email untuk memastikan keaslian dan integritas pesan.


## 3. Alert Escalation

### 3.1 Kapan Harus Eskalasi ke L2

Jawaban bisa berbeda antar tim, tetapi umumnya eskalasi dilakukan jika:

- Alert merupakan indikator serangan siber besar yang butuh investigasi lebih dalam atau DFIR.
- Diperlukan remediasi seperti penghapusan malware, isolasi host, atau reset password.
- Diperlukan komunikasi dengan pelanggan, partner, manajemen, atau penegak hukum.
- L1 belum sepenuhnya memahami alert dan butuh bantuan senior.

### 3.2 Langkah Eskalasi

Pada umumnya cukup **reassign alert ke L2 yang sedang on shift** lalu menghubungi mereka lewat chat perusahaan atau langsung. Sebagian tim mewajibkan formal escalation request tertulis dengan banyak field.

Pesan eskalasi yang baik singkat, menyebut siapa dan apa yang terjadi, tindakan yang disarankan, dan menyertakan tautan Alert Details.

Contoh alur (M.Clark dari HR terkena phishing dan memasukkan password ke halaman login palsu):

1. L1 mengirim pesan ke L2 beserta tautan alert.
2. L2 mengonfirmasi akan segera meninjau.
3. L2 merotasi kredensial user melalui Email Admin Panel.
4. L2 mengingatkan user tentang risiko phishing.

Setelah menerima tiket, L2 membaca laporan, menghubungi L1 bila ada pertanyaan, meneliti detail alert, memvalidasi True Positive, berkomunikasi dengan departemen lain, dan untuk insiden besar memulai proses formal Incident Response.

### 3.3 Meminta Dukungan L2

Wajar bagi L1 meminta bantuan senior saat alert tidak jelas, terutama di bulan-bulan awal. Lebih baik berdiskusi dan mengklarifikasi prosedur SOC daripada menutup alert yang tidak dipahami.

Contoh alur: L1 menyebut alert yang membuatnya buntu ("Reflective DLL Injection") beserta tautan alert, L2 menjawab dan mengajak panggilan singkat, lalu L2 merencanakan sesi knowledge sharing untuk seluruh analis L1.

### 3.4 Eskalasi di SOC Dashboard

1. Pindahkan alert ke status **In Progress** dan lakukan analisis.
2. Tulis alert report dan tentukan **verdict** (misalnya True Positive).
3. Jika eskalasi diperlukan, **assign alert ke L2 on shift**.
4. L2 menerima notifikasi dan memulai dari alert report tersebut.

Verdict yang diberikan L1 saat eskalasi bersifat sementara (intermediate verdict, TP atau FP); validasi akhir dilakukan L2.

### 3.5 Contoh Kasus: Spike of Domain Discovery Commands

Alert mendeteksi lonjakan command seperti `whoami`, `net user`, dan `Get-ADUser` yang lazim dipakai pada AD domain discovery. Kecuali dikonfirmasi sebagai aktivitas IT sah atau script legitimate, perangkat kemungkinan sudah terkompromi dan butuh containment segera.

Data alert: host DMZ-MSEXCHANGE-2013 (Windows Server 2012 R2), user `NT AUTHORITY\SYSTEM`, dengan rantai proses:

- **Grandparent process**: `C:\Windows\System32\inetsrv\w3wp.exe` (IIS worker process, menjalankan aplikasi web).
- **Parent process**: `C:\Users\Public\revshell.exe` (nama mencurigakan, berada di folder yang bisa ditulis semua user).
- **Source process**: `C:\Windows\System32\cmd.exe` (proses yang langsung menjalankan command).

**Process tree** menunjukkan hubungan parent-child antar proses, yaitu siapa menjalankan siapa, sehingga asal-usul aktivitas mencurigakan dapat dilacak. Pada kasus ini web application di server Exchange kemungkinan dieksploitasi, attacker menjatuhkan dan menjalankan reverse shell, lalu memakai `cmd.exe` untuk enumerasi Active Directory sebagai langkah awal lateral movement. Hak akses `NT AUTHORITY\SYSTEM` (tertinggi di Windows) memperparah dampaknya. Verdict: True Positive dan dieskalasi ke L2 untuk isolasi host dan DFIR.

### 3.6 Command pada Alert Domain Discovery

Command yang tercatat di alert: `dir hostname whoami /priv net group "Domain Admins" /domain nltest /dclist:tryhackme.thm`. Ini gabungan beberapa command:

```
dir
```

Menampilkan isi direktori saat ini.

```
hostname
```

Menampilkan nama host mesin.

```
whoami /priv
```

`whoami` menampilkan user yang sedang dipakai; switch `/priv` menampilkan daftar privilege milik user tersebut.

```
net group "Domain Admins" /domain
```

`net group` menampilkan anggota sebuah group; `"Domain Admins"` adalah group target; `/domain` menjalankan query terhadap domain controller, bukan mesin lokal.

```
nltest /dclist:tryhackme.thm
```

`nltest` adalah utilitas pengujian jalur komunikasi domain; `/dclist:<domain>` menampilkan daftar domain controller pada domain tersebut.

Command lain yang disebut di deskripsi alert: `net user` (menampilkan atau mengelola akun user) dan `Get-ADUser` (cmdlet PowerShell untuk query user Active Directory).


## 4. Communication

### 4.1 Crisis Communication

Idealnya SOC memiliki prosedur **Crisis Communication** sendiri. Jika tidak ada, analis perlu menguasai penanganan kasus-kasus kritis berikut.

### 4.2 Kasus Komunikasi dan Penanganannya

**L2 tidak tersedia selama 30 menit saat alert urgent.** Ketahui lokasi emergency contacts. Urutan menghubungi: L2, lalu L3, lalu manajer.

**Alert kompromi akun Slack atau Teams yang perlu validasi ke user.** Jangan hubungi user lewat chat yang dibobol; gunakan metode alternatif seperti telepon.

**Alert membludak dalam waktu singkat, sebagian kritis.** Prioritaskan sesuai workflow dan beri tahu L2 on shift tentang situasinya.

**Menyadari salah klasifikasi setelah beberapa hari.** Segera hubungi L2 dan jelaskan kekhawatiran. Threat actor bisa diam berminggu-minggu sebelum dampak muncul.

**Log SIEM tidak ter-parsing atau tidak bisa dicari.** Jangan skip alert; investigasi sebisanya dan laporkan masalah ke L2 on shift atau SOC engineer.

### 4.3 Komunikasi oleh L2

Contoh: developer backend baru ingin update Python tetapi mengunduh data stealer, dan laptopnya berisi data pelanggan sehingga dikhawatirkan terjadi eksfiltrasi. Alur: L1 melapor beserta tautan alert, L2 segera menginvestigasi, L2 menginisiasi Incident Response bersama tim DFIR, dan L2 menghubungi tim legal dan PR melalui para manajer.

Departemen lain yang lazim dihubungi selama analisis: tim IT (misalnya konfirmasi pemberian hak admin ke user) dan HR (informasi tentang karyawan yang baru direkrut).


## 5. Glossary

- **Alert reporting**: dokumentasi hasil investigasi alert sebelum ditutup atau dieskalasi.
- **Alert escalation**: penerusan alert True Positive ke L2 untuk tindakan atau investigasi lanjutan.
- **Communication**: koordinasi dengan L2, departemen lain, atau pihak terkait selama atau setelah analisis.
- **Crisis Communication**: prosedur komunikasi SOC untuk situasi kritis.
- **Alert funnel**: alur penyaringan alert dari L1 ke L2 ke DFIR.
- **True Positive (TP)**: alert yang benar-benar menandakan aktivitas berbahaya.
- **False Positive (FP)**: alert yang ternyata bukan ancaman.
- **Intermediate verdict**: verdict sementara dari L1 saat eskalasi (TP atau FP).
- **Five Ws**: Who, What, When, Where, Why; kerangka isi laporan.
- **Authenticity**: indikator kualitas komentar alert di SOC Dashboard.
- **L1 / L2 / L3**: tingkat analis SOC; L1 triase awal, L2 investigasi dan remediasi, L3 level senior berikutnya.
- **On shift**: analis yang sedang bertugas pada jam tersebut.
- **DFIR**: Digital Forensics and Incident Response.
- **IR**: Incident Response, proses formal penanganan insiden.
- **SIEM**: platform pengumpul dan korelasi log serta penghasil alert.
- **EDR**: Endpoint Detection and Response, deteksi dan respons di endpoint.
- **System owner**: pemilik sistem yang bisa memberi informasi tambahan.
- **Spoofing**: pemalsuan identitas pengirim, misalnya alamat email.
- **SPF / DKIM**: mekanisme autentikasi email (lihat bagian 2.6.1).
- **Social engineering**: manipulasi psikologis korban, misalnya lewat urgensi.
- **C2 (Command and Control)**: kanal komunikasi attacker ke sistem korban.
- **Exfiltration**: pencurian dan pengiriman data keluar dari organisasi.
- **Double extension**: file berekstensi ganda (misal `.mp4.exe`) untuk menyamar.
- **Reverse shell**: koneksi balik dari korban ke attacker yang memberi akses command line jarak jauh.
- **Process tree**: hierarki proses (grandparent, parent, source).
- **AD domain discovery**: enumerasi struktur Active Directory oleh attacker atau admin.
- **Lateral movement**: pergerakan attacker ke sistem lain dalam jaringan.
- **Containment / host isolation**: membatasi penyebaran dengan mengisolasi host terdampak.
- **Credential rotation**: penggantian kredensial yang berpotensi bocor.
- **DLP**: Data Loss Prevention, solusi pencegah kebocoran data.
- **Compliance department**: pihak yang menerima laporan pelanggaran kebijakan, misalnya kebocoran dokumen.


## 6. Tools & Platform Rujukan

- **SOC Simulator (TryHackMe)**: lingkungan simulasi untuk berlatih triase alert. URL tidak tertera eksplisit di materi.
- **SAL1 (sertifikasi TryHackMe)**: sertifikasi level SOC analyst 1. URL tidak tertera eksplisit di materi.
- **SOC L1 Alert Triage (room)**: room prasyarat. URL tidak tertera eksplisit di materi.
- **TryHackMe SIEM / SOC Dashboard**: dashboard simulasi untuk menulis laporan dan eskalasi alert. Diakses lewat tautan "SOC dashboard" pada room.
- **VirusTotal**: layanan analisis file dan indikator; dipakai pada contoh laporan untuk mengonfirmasi LummaStealer. URL tidak tertera eksplisit di materi.
- **SIEM, EDR, ticketing platform**: sumber alert yang diterima L1. Tidak disebut produk spesifik.


## 7. Catatan Ringkas untuk Ditulis Tangan

### 7.1 Konsep Inti
- 3 skill L1 — Reporting, Escalation, Communication
- Reporting — simpan konteks untuk L2
- Escalation — ancaman diremediasi tepat waktu
- Communication — koordinasi SOC dengan departemen lain
- L1 ragu — butuh senior atau system owner

### 7.2 Alert Funnel
- L1 — 100 SIEM alert, noise filtering, eskalasi ancaman nyata
- L2 — 10 TP, deep investigation, remediasi, mulai IR
- DFIR — 1 incident, forensik, komunikasi lintas tim
- Tujuan akhir — lindungi data perusahaan
- Sumber alert — SIEM, EDR, ticketing
- Mayoritas alert — FP atau selesai di L1

### 7.3 Reporting
- Tujuan 1 — konteks eskalasi (hemat waktu L2)
- Tujuan 2 — arsip (log mentah 3-12 bulan, alert selamanya)
- Tujuan 3 — asah skill (tak bisa jelaskan sederhana = belum paham)
- Who — user login, jalankan command, unduh file
- What — aksi atau urutan event
- When — mulai dan akhir aktivitas
- Where — device, IP, website
- Why — alasan verdict (paling penting)
- Checklist — 5W lengkap, jelas, satu timeline, bukti terlampir, alasan eskalasi
- Contoh — Double-Extension File Creation, LummaStealer, cats2025.mp4.exe

### 7.4 Eskalasi
- Eskalasi jika — serangan besar/DFIR
- Eskalasi jika — butuh remediasi (hapus malware, isolasi host, reset password)
- Eskalasi jika — butuh komunikasi eksternal/manajemen/hukum
- Eskalasi jika — L1 belum paham alert
- Cara — reassign ke L2 on shift + ping chat atau langsung
- Dashboard — In Progress, tulis report + verdict, assign L2, L2 dapat notifikasi
- Verdict L1 — sementara (TP/FP), L2 memvalidasi
- Minta bantuan — wajar, terutama bulan awal
- L2 setelah terima — baca report, validasi TP, komunikasi, mulai IR jika besar

### 7.5 Kasus Email Phishing
- Spoofed — SPF/Fail + DKIM/Fail
- Social engineering — urgensi (600% price increase, urgent notice)
- REPORT.rar — attachment berbahaya
- SPF — verifikasi server pengirim sah
- DKIM — tanda tangan digital email

### 7.6 Kasus Domain Discovery
- Process tree — w3wp.exe (grandparent), revshell.exe (parent), cmd.exe (source)
- w3wp.exe — IIS worker process
- C:\Users\Public — folder writable semua user
- NT AUTHORITY\SYSTEM — privilege tertinggi Windows
- Dugaan — web app dieksploitasi, reverse shell, enumerasi AD
- Tindak lanjut — host isolation, DFIR, eskalasi L2

### 7.7 Command
- dir — isi direktori
- hostname — nama host
- whoami — user saat ini
- whoami /priv — daftar privilege user
- net user — info/kelola akun user
- net group "Domain Admins" /domain — anggota group, /domain = query DC
- nltest /dclist:tryhackme.thm — daftar domain controller, /dclist:<domain>
- Get-ADUser — query user AD (PowerShell)

### 7.8 Communication Cases
- L2 diam 30 menit — hubungi L2, L3, lalu manajer
- Akun Slack/Teams bocor — validasi via telepon, bukan chat yang dibobol
- Alert membludak — prioritas sesuai workflow + info L2
- Salah klasifikasi — segera lapor L2 (attacker bisa diam berminggu-minggu)
- Log tak ter-parsing — jangan skip, lapor L2 atau SOC engineer
- Komunikasi L2 — DFIR (IR), legal dan PR via manajer
- Lintas departemen — IT (konfirmasi hak admin), HR (info karyawan baru)

### 7.9 Istilah Cepat
- IR — Incident Response
- DFIR — Digital Forensics and Incident Response
- C2 — Command and Control
- Exfiltration — data dicuri keluar
- Lateral movement — pindah antar sistem dalam jaringan
- Containment — batasi penyebaran
- DLP — cegah kebocoran data
- Intermediate verdict — verdict sementara
- Crisis Communication — prosedur komunikasi darurat
