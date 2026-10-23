# Resume Materi Room SOAR (Security Orchestration, Automation, and Response)

Tanggal: 22 Oktober 2026

Room ini tidak memuat command terminal. Seluruh isinya konseptual (SOC, SOAR, playbook) ditambah satu simulasi interaktif berbasis toggle. Karena itu resume ini tidak memiliki bagian command dan switch.

## 1. Gambaran Umum

Room ini membahas bagaimana tim SOC bekerja secara tradisional, tantangan yang mereka hadapi, dan bagaimana **SOAR** menyelesaikan tantangan tersebut melalui tiga kapabilitas: Orchestration, Automation, dan Response. Materi ditutup dengan dua contoh playbook nyata (Phishing dan CVE Patching) serta praktik menyusun workflow Threat Intelligence.

Tujuan pembelajaran:

- Memahami SOC tradisional dan tantangannya
- Memahami cara SOAR mengatasi tantangan tersebut
- Memahami Playbook SOAR
- Mempraktikkan alur kerja threat intelligence

Prasyarat yang disarankan: SOC Fundamentals, Junior Security Analyst Intro, dan Intro to Detection Engineering.

## 2. SOC Tradisional

### 2.1 Definisi

**Security Operations Center (SOC)** adalah lokasi terpusat untuk memantau dan melindungi aset digital organisasi. SOC berkembang dari generasi ke generasi dengan menambahkan teknologi baru. Keunggulan utamanya adalah penanganan insiden keamanan yang lebih baik lewat monitoring dan analisis berkelanjutan.

Efektivitas SOC bergantung pada kombinasi tiga unsur: **people**, **processes**, dan **technologies**.

### 2.2 Kapabilitas Utama SOC

**Monitoring and Detection**

Memindai dan menandai aktivitas mencurigakan di jaringan secara terus-menerus, sehingga ancaman baru dapat dikenali sejak tahap awal. Dilakukan terutama lewat **SIEM**. Contoh: banyak login gagal pada workstation kritis, atau login dari lokasi tidak dikenal.

**Recovery and Remediation**

SOC bertindak sebagai first responder saat ancaman teridentifikasi: mengisolasi atau mematikan endpoint terinfeksi, menghapus malware, dan menghentikan proses berbahaya. Prosesnya melibatkan solusi lain seperti **EDR**, **firewall**, dan **IAM**. Contoh: isolasi endpoint lewat EDR, blokir IP di firewall, nonaktifkan user di IAM.

**Threat Intelligence**

Monitoring membutuhkan aliran data ancaman yang konstan dan terbaru, berupa IP address, hash, domain, dan indikator lain. Contoh: memblokir domain berbahaya yang ditandai feed threat intelligence.

**Communication**

SOC berkoordinasi dengan tim IT dan manajemen agar ancaman dikomunikasikan dengan efektif dan insiden ditangani. Contoh: membuat tiket untuk tim IT guna memverifikasi patch yang baru di-deploy.

### 2.3 Tantangan SOC

**Alert Fatigue.** Banyaknya tools menghasilkan alert dalam jumlah besar. Sebagian besar berupa false positive atau tidak cukup untuk diinvestigasi, sehingga analis kewalahan dan tidak sempat menangani kejadian yang benar-benar serius.

**Too Many Disconnected Tools.** Tools keamanan sering dipasang tanpa integrasi. Analis harus membuka log dan rule firewall secara terpisah dari log endpoint security, yang berujung pada overload tools.

**Manual Processes.** Prosedur investigasi sering tidak terdokumentasi dan bergantung pada **tribal knowledge** milik analis senior. Akibatnya investigasi melambat dan waktu respons meningkat.

**Talent Shortage.** SOC sulit merekrut dan memperluas tim, sementara ancaman makin canggih. Beban alert yang berlebihan membuat analis semakin kewalahan, respons insiden melambat, dan celah bagi penyerang membesar.

## 3. SOAR

### 3.1 Definisi

**SOAR (Security Orchestration, Automation, and Response)** adalah tool yang menyatukan seluruh tools keamanan dalam SOC ke dalam satu interface. Analis tidak perlu berpindah antara SIEM, EDR, firewall, dan tools lain. Selain itu SOAR menyediakan fitur **ticketing** dan **case management** untuk mendokumentasikan, melacak, dan menyelesaikan insiden secara terstruktur.

Sumber yang dihubungkan ke SOAR: SIEM, TI Feeds, Firewalls/IDS, dan IAM. Hasil yang diperoleh: automated response, reduced alert fatigue, dan consistent workflows.

### 3.2 Tiga Kapabilitas Inti

**Orchestration**

Mengoordinasikan banyak tools dari berbagai vendor di dalam satu interface SOAR. Alur investigasi untuk tiap jenis alert didefinisikan sebagai **Playbook**, yaitu langkah-langkah yang telah ditentukan untuk menginvestigasi sebuah alert.

Tanpa orchestration, analis yang menangani VPN brute force harus berpindah manual antara SIEM (apakah user biasa memakai IP tersebut), platform TI (reputasi IP), IAM (nonaktifkan user bila ada login berhasil), dan sistem ticketing (buka dan lacak insiden).

Contoh playbook VPN brute force:

1. Menerima alert dari SIEM
2. Query SIEM untuk memeriksa apakah user biasa memakai IP tersebut
3. Periksa reputasi IP di platform TI
4. Query SIEM untuk login yang berhasil
5. Eskalasi ke tindakan containment

Playbook bersifat dinamis dan bercabang: hasil tiap langkah menentukan langkah berikutnya. Jika user memang biasa memakai IP tersebut dan percobaan gagalnya minim, playbook dapat berhenti lebih awal.

**Automation**

Playbook dijalankan sendiri oleh SOAR tanpa klik manual analis. Pada contoh VPN brute force: SOAR menerima alert, otomatis query riwayat login di SIEM, otomatis memverifikasi reputasi IP lewat TI, otomatis menonaktifkan user di IAM bila IP berbahaya, lalu otomatis membuka tiket lengkap dengan detail. Hasilnya analis dapat menangani ratusan alert tanpa burnout.

**Response**

Kemampuan mengambil tindakan di berbagai tools dari satu interface, termasuk secara otomatis. Contoh: memblokir IP di firewall, menonaktifkan user di IAM, dan membuka tiket lengkap.

Ketiganya bersama-sama menjawab tantangan SOC: alert fatigue berkurang, proses diotomatisasi, dan tools saling terhubung.

### 3.3 Apakah Analis SOC Masih Dibutuhkan

Ya. SOAR mengotomatisasi tugas repetitif dan merapikan struktur kerja, tetapi tidak menggantikan analis. Investigasi kompleks tetap membutuhkan analis, SOAR tidak dapat membuat judgment call pada titik kritis, dan analis memahami ancaman dalam konteks bisnis yang lebih luas. Playbook sendiri dibuat oleh analis SOC.

## 4. Contoh Playbook

### 4.1 Phishing Playbook

Phishing adalah vektor serangan paling umum pada breach. Investigasinya memakan waktu karena melibatkan analisis attachment dan URL serta verifikasi lewat platform TI. SOAR menjalankan tugas ini di latar belakang dan dapat melakukan remediasi otomatis saat email phishing terkonfirmasi.

Alur playbook:

1. Mulai dari alert "Suspected email received". Dua jalur berjalan: pembuatan investigation ticket, dan pengecekan apakah email memuat URL atau attachment.
2. Jika tidak ada URL maupun attachment, pengguna diberi tahu lewat Slack atau channel lain, lalu alur selesai.
3. Jika ada attachment: hitung hash, kirim hash ke **VirusTotal**, lalu tentukan apakah hash berbahaya.
4. Jika ada URL: kirim URL ke VirusTotal, lalu tentukan apakah URL berbahaya.
5. Bila hash atau URL berbahaya, email dihapus. Bila tidak berbahaya, analis melakukan **manual analysis di sandbox**. Jika hasil analisis menyatakan berbahaya, email dihapus.
6. Setelah email dihapus, tiket diperbarui dengan **IOC** yang ditemukan.

### 4.2 CVE Patching Playbook

**CVE (Common Vulnerabilities and Exposures)** adalah kerentanan yang diumumkan publik dan diberi nomor CVE. Tim SOC harus memverifikasi apakah CVE baru ada di jaringan dan melakukan patching bila ada. Karena CVE dirilis sering, prosesnya memakan banyak waktu dan sumber daya, menimbulkan backlog, dan membuat lingkungan tetap rentan.

Playbook menganalisis detail CVE, menilai ambang risiko, membuat tiket patching, dan menguji patch sebelum didorong ke production.

Alur ringkas:

1. Memantau advisory dan mengekstrak data CVE baru
2. SOAR memeriksa ke patch management apakah CVE sudah ditangani dan apakah berlaku di lingkungan
3. Jika berlaku, dibuat ticket CVE dan analis meninjau tiket
4. SOAR mengidentifikasi aset terdampak, mencari patch di configuration management, lalu menguji patch di mesin test
5. SOC men-deploy patch ke aset dan memverifikasi rollout
6. SOAR memindai ulang aset untuk memeriksa apakah masih rentan
7. Jika masih rentan, SOC mengembangkan **mitigation plan** dan men-deploy mitigasi. Jika tidak, tiket ditutup dan patch management diperbarui bahwa CVE telah ditangani

Pada kedua playbook, sebagian besar langkah otomatis tetapi analis SOC tetap muncul di titik-titik krusial. Ini menegaskan bahwa peran analis tetap esensial untuk keputusan dan verifikasi.

## 5. Praktik: Threat Intel Workflow

Simulasi meminta penentuan bagian workflow mana yang **manual** dan mana yang **automated** lewat toggle, mengikuti checklist: Integrate Case Management, Integrate Threat Intel Feed, Extract Threat Data, dan Actions on Threat Data. Hasilnya diperiksa dengan tombol RUN.

### 5.1 Case Management

Automated: Create Case Ticket (TheHive, ServiceNow), Assign Case Ticket, Communicate Case Ticket (Email, Slack), Update Case Ticket (push API).

Manual: Delete Case Ticket.

### 5.2 Threat Intelligence Feeds

Automated: Fetch New Incident Alerts, Set Fetch Intervals (pull API), Failed Fetch Notifications.

Manual: Discard Old Alerts.

### 5.3 Incident Data Extraction

Automated: Extract Domains, Extract URLs, Extract IPs (untuk lookup ancaman yang sudah dikenal di VirusTotal).

Manual: Analyst Extraction (untuk ancaman yang tidak dikenal atau baru).

### 5.4 Reputation Checks

Automated: Reputation Results Output (hasil terkompilasi dari VirusTotal).

Manual: Sandbox Testing (Any.Run, Hybrid Analysis) dan Analyst Validation (analisis hasil dan konfirmasi kasus).

### 5.5 Course of Action

Automated: Block Domains, Block IPs, Block URLs (firewall, network blacklist rules), dan Update Case Tickets.

Manual: Analyst Approve COA (konfirmasi tindakan dan remediasi).

### 5.6 Pola Umum

Pekerjaan repetitif dan berbasis data yang sudah dikenal diotomatisasi. Tindakan destruktif (penghapusan), analisis terhadap ancaman baru atau tidak dikenal, validasi hasil, dan persetujuan tindakan akhir tetap manual.

## 6. Glossary

- **SOC**: Security Operations Center, pusat pemantauan dan perlindungan aset digital
- **SOAR**: Security Orchestration, Automation, and Response
- **SIEM**: platform pengumpul dan korelasi log untuk monitoring dan deteksi
- **EDR**: solusi keamanan endpoint, dipakai antara lain untuk isolasi endpoint
- **IAM**: manajemen identitas dan akses, dipakai antara lain untuk menonaktifkan user
- **TI (Threat Intelligence)**: data ancaman seperti IP, hash, domain, dan indikator lain
- **Playbook**: workflow terdefinisi yang memberi tahu SOAR cara menginvestigasi alert
- **Alert Fatigue**: kelelahan analis akibat alert berlebihan, banyak di antaranya false positive
- **Tribal Knowledge**: pengetahuan tidak tertulis milik analis berpengalaman
- **First Responder**: peran SOC saat ancaman teridentifikasi
- **IOC**: indikator compromise, misalnya hash, URL, IP, domain berbahaya
- **CVE**: kerentanan yang diumumkan publik dan diberi nomor
- **Sandbox**: lingkungan terisolasi untuk menganalisis email atau file mencurigakan
- **COA (Course of Action)**: rangkaian tindakan dan remediasi yang diambil setelah analisis
- **Orchestration**: koordinasi banyak tools dalam satu interface SOAR
- **Automation**: eksekusi playbook oleh SOAR tanpa klik manual
- **Response**: pengambilan tindakan dari satu interface, termasuk otomatis

## 7. Tools & Platform Rujukan

Tidak ada URL eksplisit yang ditampilkan di materi, sehingga hanya nama yang dicantumkan.

- **VirusTotal**: pengecekan reputasi hash, URL, domain, dan IP
- **Any.Run**: sandbox untuk analisis manual
- **Hybrid Analysis**: sandbox untuk analisis manual
- **TheHive**: platform ticketing dan case management
- **ServiceNow**: platform ticketing dan case management
- **Slack**: kanal notifikasi dan komunikasi tim
- **Email**: kanal komunikasi tiket kasus dan notifikasi pengguna

Room rujukan di TryHackMe: SOC Fundamentals, Junior Security Analyst Intro, Intro to Detection Engineering.

## 8. Catatan Ringkas untuk Ditulis Tangan

### SOC

- SOC — pusat monitoring dan proteksi aset digital
- 3 unsur — people, processes, technologies
- Monitoring and Detection — scan aktivitas mencurigakan, via SIEM
- Recovery and Remediation — isolasi endpoint, hapus malware, stop proses (EDR, firewall, IAM)
- Threat Intelligence — feed IP, hash, domain, indikator
- Communication — koordinasi IT dan manajemen, tiket

### Tantangan SOC

- Alert Fatigue — alert banyak, false positive
- Disconnected Tools — tools tidak terintegrasi
- Manual Processes — tribal knowledge, tidak terdokumentasi
- Talent Shortage — sulit rekrut, analis kewalahan

### SOAR

- SOAR — satu interface untuk semua tools, plus ticketing dan case management
- Input — SIEM, TI Feeds, Firewalls/IDS, IAM
- Output — automated response, reduced alert fatigue, consistent workflows
- Orchestration — koordinasi tools multi-vendor, definisi playbook
- Automation — playbook jalan otomatis tanpa klik
- Response — aksi dari satu interface (blokir IP, disable user, buka tiket)
- Playbook — langkah investigasi terdefinisi, bercabang, hasil langkah menentukan langkah berikut
- Analis tetap perlu — judgment call, konteks bisnis, pembuat playbook

### Playbook Phishing

- Alert — Suspected email received, buat ticket
- Tanpa URL atau attachment — notifikasi Slack, selesai
- Attachment — compute hash, cek VirusTotal
- URL — kirim ke VirusTotal
- Berbahaya — hapus email
- Tidak berbahaya — analisis manual di sandbox
- Akhir — update tiket dengan IOC

### Playbook CVE Patching

- CVE — kerentanan publik bernomor
- Sumber — advisory, ekstrak CVE baru
- Cek — sudah ditangani? berlaku?
- Tiket — buat ticket CVE, analis review
- Patch — cari patch, uji di mesin test, deploy, verifikasi
- Scan ulang — masih rentan, SOC kembangkan mitigation plan
- Tidak rentan — tutup tiket, update patch management

### Praktik Threat Intel Workflow

- Checklist — Case Management, TI Feed, Extract Data, Actions
- Manual — Delete Case Ticket, Discard Old Alerts, Analyst Extraction, Sandbox Testing, Analyst Validation, Analyst Approve COA
- Automated — sisanya (create, assign, communicate, update ticket; fetch alerts, interval, fail notification; extract domain, URL, IP; reputation output; block domain, IP, URL; update tickets)
- Pola — repetitif dan data dikenal otomatis; hapus, baru, validasi, approve manual
- Tools — TheHive, ServiceNow, VirusTotal, Any.Run, Hybrid Analysis, Slack, Email
