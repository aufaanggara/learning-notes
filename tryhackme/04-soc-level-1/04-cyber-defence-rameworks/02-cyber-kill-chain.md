# Resume Room TryHackMe: Cyber Kill Chain (22 Oktober 2026)

Resume ini merangkum seluruh materi room Cyber Kill Chain: konsep dasar, tujuh fase serangan, studi kasus breach Target 2013, serta keterbatasan framework. Bagian akhir berisi glossary, tools rujukan, dan catatan ringkas untuk ditulis tangan.


## 1. Konsep Inti

### 1.1 Kill Chain

**Kill chain** adalah konsep militer yang menggambarkan struktur sebuah serangan: identifikasi target, keputusan dan perintah untuk menyerang, lalu penghancuran target.

### 1.2 Cyber Kill Chain

**Cyber Kill Chain** adalah framework keamanan siber yang dibuat oleh **Lockheed Martin** (perusahaan keamanan dan aerospace) pada tahun **2011**, diadaptasi dari konsep militer di atas. Framework ini mendefinisikan langkah-langkah yang dilalui adversary atau pelaku jahat di dunia siber. Agar berhasil, adversary harus melewati **seluruh fase** dalam chain. Artinya, jika satu fase berhasil diputus, serangan gagal.

### 1.3 Mengapa Penting Dipahami

- Membantu memahami dan bertahan dari **ransomware**, **security breach**, dan **Advanced Persistent Threat (APT)**.
- Dipakai untuk menilai keamanan jaringan dan sistem dengan mengidentifikasi **kontrol keamanan yang hilang** dan menutup celah sesuai infrastruktur perusahaan.
- Bagi SOC Analyst, Security Researcher, Threat Hunter, dan Incident Responder, framework ini membantu mengenali upaya intrusi serta memahami tujuan dan sasaran penyusup.

### 1.4 Tujuh Fase

Urutan fase yang dibahas di room ini:

1. Reconnaissance
2. Weaponization
3. Delivery
4. Exploitation
5. Installation
6. Command & Control
7. Actions on Objectives

Dalam skenario room, penyerang fiktif bernama **Megatron** dipakai untuk menggambarkan sudut pandang adversary di setiap fase.


## 2. Fase-Fase Cyber Kill Chain

### 2.1 Reconnaissance

Fase riset dan perencanaan. Adversary mengumpulkan informasi tentang target: detail infrastruktur, data karyawan, proses bisnis, dan teknologi yang terekspos. Recon biasanya bersifat **pasif dan tidak terdeteksi**. Recon yang buruk menghasilkan serangan asal-asalan, sedangkan recon yang baik memungkinkan payload yang sangat tertarget dan meyakinkan.

**OSINT (Open-Source Intelligence)** adalah pengumpulan insight dari informasi yang tersedia untuk umum. Sumber data OSINT:

- Search engine
- Media cetak dan online
- Akun media sosial
- Forum online dan blog
- Database catatan publik online
- WHOIS dan data teknis

**Jenis recon:**

- **Passive recon**: tanpa interaksi langsung dengan target. Contoh: WHOIS lookup, social media scraping, meninjau data breach.
- **Active recon**: kontak langsung dengan target. Contoh: social engineering, port scanning, banner grabbing, probing layanan yang terbuka.

**Email harvesting** adalah proses memperoleh alamat email dari sumber publik, berbayar, atau gratis. Hasilnya dipakai untuk serangan **phishing**, yaitu social engineering untuk mencuri data sensitif seperti kredensial login dan nomor kartu kredit. Tools yang disebut di room:

- **theHarvester**: mengumpulkan email, nama, subdomain, IP, dan URL dari berbagai sumber data publik.
- **Hunter.io**: tool pencarian email untuk mendapatkan informasi kontak yang terkait dengan sebuah domain.
- **OSINT Framework**: kumpulan tools OSINT yang dikelompokkan per kategori.

### 2.2 Weaponization

Penyerang mengubah informasi hasil recon menjadi alat serangan yang siap pakai dengan membuat **malware** dan **exploit** ke dalam sebuah **payload**. Kebanyakan penyerang memakai tools otomatis atau membeli malware di **DarkWeb**. Aktor yang lebih canggih atau APT yang disponsori negara menulis **malware custom** agar sampelnya unik dan lolos deteksi.

Definisi kunci:

- **Malware**: program atau software yang dirancang untuk merusak, mengganggu, atau mendapatkan akses tidak sah ke komputer.
- **Exploit**: program atau kode yang memanfaatkan kerentanan atau kelemahan pada aplikasi atau sistem.
- **Payload**: kode berbahaya yang dijalankan penyerang di dalam sistem.

Taktik yang bisa dipakai di fase ini:

- Membuat dokumen Microsoft Office terinfeksi yang berisi **macro** berbahaya atau script **VBA (Visual Basic for Applications)**.
- Membuat payload berbahaya atau worm canggih, menanamkannya ke USB drive, lalu menyebarkannya ke publik.
- Menyiapkan infrastruktur **Command and Control (C2)** untuk mengeksekusi perintah di mesin korban atau mengirim payload tambahan.
- Menginfeksi host korban dengan **backdoor** untuk mengakses sistem sekaligus melewati mekanisme keamanan.
- Menyesuaikan template phishing atau aplikasi **OAuth-consent** agar tampak sah dan menipu korban.

### 2.3 Delivery

Penyerang memilih metode untuk mengirim payload atau malware ke lingkungan target. Tiga metode yang dibahas:

- **Phishing email**: email berbahaya yang menargetkan satu orang tertentu (**spear phishing**) atau banyak orang di perusahaan. Email berisi link berbahaya atau lampiran yang berujung pada kompromi.
- **USB drop**: media pengiriman fisik yang ditaruh di tempat umum seperti coffee shop, tempat parkir, atau jalanan. Varian canggihnya (**USB Drop Attack**) mencetak logo perusahaan pada USB lalu mengirimkannya ke perusahaan dengan menyamar sebagai pelanggan yang memberi hadiah.
- **Watering hole attack**: menyerang kelompok orang tertentu dengan mengkompromikan website yang biasa mereka kunjungi, lalu mengalihkan mereka ke website berbahaya. Korban tanpa sadar mengunduh malware, yang disebut **drive-by download**. Contoh: pop-up yang meminta pengunduhan ekstensi browser palsu.

### 2.4 Exploitation

Momen ketika kode penyerang benar-benar **dieksekusi** di target dengan memanfaatkan kerentanan yang diketahui. Teknik utama:

- **Malicious macro execution**: dikirim lewat email phishing dan menjalankan ransomware saat korban membukanya.
- **Zero-day exploit**: memanfaatkan celah yang belum diketahui dan belum ditambal, sehingga tidak ada peluang deteksi di awal.
- **Known CVE**: mengeksploitasi kerentanan publik yang belum ditambal pada lingkungan target.

Setelah mendapat akses, penyerang dapat mengeksploitasi kerentanan software, sistem, atau server untuk melakukan **privilege escalation** atau **lateral movement** di dalam jaringan.

Tanda-tanda exploitation yang perlu diwaspadai:

- Proses tak terduga yang muncul (unexpected process spawns).
- Perubahan registry atau service baru yang dibuat.
- Argumen command-line mencurigakan di system log.

### 2.5 Installation

Backdoor (disebut juga **access point**) memungkinkan penyerang melewati kontrol keamanan dan menyembunyikan akses. Penyerang perlu memasang **persistent backdoor** agar bisa kembali ke sistem jika koneksi terputus, akses awal dicabut karena terdeteksi, atau sistem di-patch. Ini disebut **persistence**.

Cara mencapai persistence:

- **Web shell**: script berbahaya di webserver yang ditulis dalam bahasa web development seperti ASP, PHP, atau JSP. Karena sederhana dan formatnya (.php, .asp, .aspx, .jsp) lazim, web shell sulit dideteksi dan bisa terklasifikasi sebagai file benign.
- **Backdoor di mesin korban**: contohnya memakai **Meterpreter**, yaitu payload Metasploit Framework yang memberi shell interaktif untuk berinteraksi dan mengeksekusi kode berbahaya di mesin korban secara remote.
- **Membuat atau memodifikasi Windows service** (teknik **T1543.003** pada MITRE ATT&CK): service dipakai untuk menjalankan script atau payload secara berkala. Tools yang dipakai: **sc.exe** (Create, Start, Stop, Query, atau Delete service Windows) dan **Reg** (memodifikasi konfigurasi service). Penyerang juga dapat melakukan **masquerade**, yaitu memakai nama service yang dikenal terkait OS atau software legitimate.
- **Run keys di Registry atau Startup Folder**: payload dieksekusi setiap user login. Ada startup folder per akun user dan startup folder system-wide yang dicek untuk semua akun.

**Timestomping** adalah teknik memodifikasi timestamp file (waktu modify, access, create, dan change) untuk menghindari deteksi forensic investigator dan membuat malware tampak seperti bagian dari program yang sah.

### 2.6 Command & Control (C2)

Setelah persistence dan eksekusi malware, penyerang membuka kanal **C2** untuk mengontrol dan memanipulasi korban dari jauh. Aktivitas ini juga disebut **C&C** atau **C2 beaconing**: host terinfeksi berkomunikasi terus-menerus dengan server C2 (asal istilah "beaconing"). Setelah koneksi terbentuk, penyerang memiliki kendali penuh atas mesin korban.

**IRC (Internet Relay Chat)** dulu menjadi kanal C2 tradisional, tetapi kini mudah dideteksi solusi keamanan modern. Kanal C2 yang paling umum saat ini:

- **HTTP (port 80) dan HTTPS (port 443)**: traffic berbahaya berbaur dengan traffic legitimate sehingga membantu menghindari firewall.
- **DNS**: mesin terinfeksi terus membuat request DNS ke DNS server milik penyerang. Teknik ini disebut **DNS Tunneling**.

Pemilik infrastruktur C2 bisa berupa adversary itu sendiri atau host lain yang sudah dikompromikan.

### 2.7 Actions on Objectives (Exfiltration)

Fase akhir ketika penyerang mencapai tujuan aslinya dengan akses **hands-on keyboard**. Aksi yang bisa dilakukan:

- Mengumpulkan kredensial user.
- **Privilege escalation**, misalnya dari workstation menjadi akses domain administrator dengan mengeksploitasi misconfiguration.
- **Internal reconnaissance**, misalnya berinteraksi dengan software internal untuk menemukan kerentanannya.
- **Lateral movement** di lingkungan perusahaan.
- Mengumpulkan dan **mengeksfiltrasi** data sensitif.
- Menghapus backup dan **shadow copy** (teknologi Microsoft untuk membuat backup copy, snapshot file komputer, atau volume).
- Menimpa atau merusak data.


## 3. Studi Kasus: Breach Target 2013

Serangan siber terhadap **Target** terjadi pada **27 November 2013** dan menjadi salah satu kebocoran data terbesar dalam sejarah. Pada **19 Desember 2013** Target merilis pernyataan resmi yang menyatakan sekitar **40 juta** akun kartu kredit dan debit terdampak antara 27 November dan 15 Desember 2013. Target membayar denda **$18,5 juta** dalam multistate settlement agreement, yang dianggap sebagai settlement data breach terbesar dalam sejarah.

Pemetaan teknik ke fase Kill Chain pada latihan room (enam dari tujuh fase relevan; Reconnaissance tidak dipakai):

| Fase | Teknik | Makna |
|---|---|---|
| Weaponization | powershell | Tool scripting yang dipersenjatai |
| Delivery | spearphishing attachment | Lampiran email phishing tertarget |
| Exploitation | exploit public-facing application | Eksploitasi aplikasi yang terbuka ke publik |
| Installation | dynamic linker hijacking | Membajak proses loading shared library untuk persistence |
| Command & Control | fallback channels | Jalur komunikasi cadangan bila kanal utama terblokir |
| Actions on Objectives | data from local system | Pengambilan data dari sistem lokal |


## 4. Keterbatasan Traditional Cyber Kill Chain

Kill Chain adalah tool bagus untuk memperkuat pertahanan jaringan, tetapi **bukan satu-satunya yang bisa diandalkan**.

- **Tidak pernah diperbarui sejak 2011.** Ketiadaan pembaruan menciptakan security gap.
- **Fokus pada perimeter jaringan dan ancaman malware.** Ancaman modern jauh lebih berkembang: adversary mengombinasikan banyak **TTP (tactics, techniques, and procedures)** dan bisa mengalahkan threat intelligence dengan mengubah file hash dan alamat IP. Vendor keamanan menjawabnya dengan teknologi seperti **AI** dan berbagai algoritma untuk mendeteksi perubahan sekecil apa pun.
- **Tidak dapat mengidentifikasi insider threat**, karena fokus utamanya pada malware delivery dan keamanan jaringan. Menurut **CISA**, insider threat adalah potensi seorang insider memakai akses resmi atau pemahamannya tentang organisasi untuk merugikan organisasi tersebut.

Rekomendasi room: padukan dengan **MITRE ATT&CK** dan **Unified Kill Chain** untuk pendekatan pertahanan yang lebih komprehensif.


## 5. Glossary

- **Kill chain**: konsep militer tentang struktur serangan: identifikasi target, keputusan menyerang, penghancuran target.
- **Cyber Kill Chain**: framework tujuh fase buatan Lockheed Martin (2011) yang memetakan langkah adversary.
- **Adversary**: pelaku jahat atau penyerang di dunia siber.
- **APT (Advanced Persistent Threat)**: kelompok ancaman canggih, termasuk yang disponsori negara, yang sering menulis malware custom.
- **Reconnaissance**: fase riset dan pengumpulan informasi tentang target.
- **OSINT**: intelijen dari sumber terbuka yang tersedia untuk umum.
- **Passive recon**: recon tanpa kontak langsung dengan target.
- **Active recon**: recon dengan kontak langsung ke target.
- **Email harvesting**: pengumpulan alamat email dari sumber publik, berbayar, atau gratis.
- **Phishing**: social engineering untuk mencuri data sensitif seperti kredensial dan nomor kartu kredit.
- **Spear phishing**: phishing yang menargetkan orang tertentu.
- **Weaponization**: fase membuat malware dan exploit menjadi payload siap pakai.
- **Malware**: software yang dirancang merusak, mengganggu, atau mengakses komputer tanpa izin.
- **Exploit**: kode yang memanfaatkan kerentanan pada aplikasi atau sistem.
- **Payload**: kode berbahaya yang dijalankan penyerang di sistem target.
- **Macro / VBA**: script otomatis di dokumen Office yang bisa disalahgunakan sebagai malware.
- **C2 (Command and Control)**: infrastruktur dan kanal untuk mengontrol mesin korban dari jauh.
- **Backdoor**: akses tersembunyi yang melewati kontrol keamanan; disebut juga access point.
- **Delivery**: fase pengiriman payload ke lingkungan target.
- **USB drop**: pengiriman malware lewat USB yang ditaruh atau dikirim ke target.
- **Watering hole attack**: kompromi website yang sering dikunjungi kelompok target.
- **Drive-by download**: unduhan malware tanpa disadari korban saat mengunjungi situs berbahaya.
- **Exploitation**: fase eksekusi kode penyerang di target lewat kerentanan.
- **Zero-day exploit**: eksploitasi celah yang belum diketahui dan belum ditambal.
- **CVE**: kerentanan publik yang sudah diketahui.
- **Privilege escalation**: menaikkan hak akses di sistem.
- **Lateral movement**: berpindah antar sistem di dalam jaringan.
- **Installation**: fase memasang akses permanen di sistem korban.
- **Persistence**: kemampuan mempertahankan akses meski koneksi putus atau sistem di-patch.
- **Persistent backdoor**: backdoor yang bertahan untuk akses ulang di masa depan.
- **Web shell**: script berbahaya di webserver (ASP, PHP, JSP) untuk menjaga akses.
- **Meterpreter**: payload Metasploit yang memberi shell interaktif remote.
- **Masquerade**: menyamarkan payload dengan nama service atau software yang sah.
- **Run keys**: entri Registry yang menjalankan program saat user login.
- **Timestomping**: memodifikasi timestamp file untuk menghindari forensik.
- **C2 beaconing**: komunikasi berkala host terinfeksi ke server C2.
- **DNS Tunneling**: C2 lewat request DNS terus-menerus ke DNS server penyerang.
- **IRC**: Internet Relay Chat; kanal C2 tradisional yang kini mudah terdeteksi.
- **Exfiltration**: pencurian data sensitif keluar dari jaringan target.
- **Shadow copy**: teknologi Microsoft untuk backup atau snapshot file dan volume.
- **Insider threat**: ancaman dari orang dalam yang menyalahgunakan akses resmi.
- **TTP**: tactics, techniques, and procedures adversary.
- **MITRE ATT&CK**: knowledge base taktik dan teknik adversary berdasarkan skenario dunia nyata.


## 6. Tools & Platform Rujukan

- **theHarvester**: mengumpulkan email, nama, subdomain, IP, dan URL dari sumber publik.
- **Hunter.io**: pencarian email dan informasi kontak berdasarkan domain (hunter.io).
- **OSINT Framework**: kumpulan tools OSINT per kategori.
- **Varonis, artikel "What is OSINT?"**: bacaan lanjutan tentang OSINT.
- **Meterpreter (Metasploit Framework)**: payload shell interaktif untuk backdoor.
- **sc.exe**: mengelola Windows service (Create, Start, Stop, Query, Delete).
- **Reg**: memodifikasi konfigurasi service dan Registry.
- **TryHackMe Windows Persistence Room**: mempelajari persistence di Windows.
- **Artikel Microsoft tentang serangan web shell**: referensi berbagai serangan web shell.
- **MITRE ATT&CK (T1543.003; Registry Run Keys / Startup Folder)**: referensi teknik persistence.
- **Unified Kill Chain**: model rujukan pelengkap Cyber Kill Chain.
- **CISA**: sumber definisi insider threat.
- **Pernyataan resmi Target dan settlement agreement**: dokumen rujukan kasus breach Target 2013.


## 7. Catatan Ringkas untuk Ditulis Tangan

### 7.1 Konsep Dasar

- Kill chain — konsep militer: identifikasi target, perintah serang, hancurkan
- Cyber Kill Chain — Lockheed Martin, 2011, 7 fase
- Syarat sukses adversary — lewati semua fase; putus satu fase, serangan gagal
- Manfaat — lawan ransomware, breach, APT; cari kontrol yang hilang
- Dipakai oleh — SOC Analyst, Security Researcher, Threat Hunter, Incident Responder
- Megatron — penyerang fiktif di skenario room

### 7.2 Urutan 7 Fase

- 1 Reconnaissance
- 2 Weaponization
- 3 Delivery
- 4 Exploitation
- 5 Installation
- 6 Command & Control
- 7 Actions on Objectives

### 7.3 Reconnaissance

- Recon — riset dan perencanaan, biasanya pasif
- OSINT — intel dari info publik
- Sumber OSINT — search engine, media cetak/online, sosmed, forum/blog, database publik, WHOIS
- Passive recon — tanpa kontak: WHOIS, scraping sosmed, data breach
- Active recon — kontak langsung: social engineering, port scan, banner grabbing, probing
- Email harvesting — kumpulkan email untuk phishing
- theHarvester — email, nama, subdomain, IP, URL
- Hunter.io — email berdasarkan domain
- OSINT Framework — kumpulan tools OSINT per kategori

### 7.4 Weaponization

- Tujuan — ubah hasil recon jadi payload
- Malware — software perusak atau akses ilegal
- Exploit — kode pemanfaat kerentanan
- Payload — kode jahat yang dijalankan di sistem
- Sumber malware — tools otomatis, DarkWeb, custom (APT)
- Taktik — macro/VBA di Office, USB + worm, infrastruktur C2, backdoor, template phishing/OAuth-consent

### 7.5 Delivery

- Phishing email — link/lampiran; spear phishing = satu target
- USB drop — USB di tempat umum; USB Drop Attack = logo perusahaan + kirim pos
- Watering hole — kompromi situs langganan target, redirect
- Drive-by download — unduh malware tanpa sadar (contoh: ekstensi browser palsu)

### 7.6 Exploitation

- Exploitation — kode penyerang tereksekusi lewat kerentanan
- Malicious macro — via phishing, jalankan ransomware
- Zero-day — celah belum diketahui, belum di-patch
- Known CVE — kerentanan publik belum di-patch
- Pasca akses — privilege escalation, lateral movement
- Tanda — proses tak terduga, perubahan registry/service baru, command-line mencurigakan di log

### 7.7 Installation

- Tujuan — persistence lewat persistent backdoor
- Web shell — script ASP/PHP/JSP di webserver; sulit dideteksi
- Meterpreter — payload Metasploit, shell interaktif remote
- T1543.003 — buat/modifikasi Windows service (MITRE ATT&CK)
- sc.exe — Create, Start, Stop, Query, Delete service
- Reg — ubah konfigurasi service/Registry
- Masquerade — pakai nama service sah
- Run keys / Startup Folder — payload jalan tiap login; folder per-user dan system-wide
- Timestomping — ubah timestamp (modify, access, create, change) untuk kelabui forensik

### 7.8 Command & Control

- C2 — kendali remote mesin korban
- C2 beaconing — host infeksi lapor berkala ke server C2
- IRC — C2 lama, kini mudah terdeteksi
- HTTP 80 / HTTPS 443 — berbaur dengan traffic sah, lolos firewall
- DNS — request terus-menerus ke DNS penyerang = DNS Tunneling
- Pemilik C2 — adversary atau host lain yang sudah dikompromikan

### 7.9 Actions on Objectives

- Akses — hands-on keyboard
- Aksi — curi kredensial, privilege escalation, internal recon, lateral movement, eksfiltrasi data, hapus backup dan shadow copy, timpa/rusak data
- Shadow copy — fitur Microsoft untuk backup/snapshot file dan volume

### 7.10 Kasus Target 2013

- Tanggal serangan — 27 November 2013
- Pengumuman Target — 19 Desember 2013
- Dampak — sekitar 40 juta akun kartu kredit/debit (27 Nov - 15 Des 2013)
- Denda — $18,5 juta, multistate settlement, terbesar dalam sejarah
- Weaponization — powershell
- Delivery — spearphishing attachment
- Exploitation — exploit public-facing application
- Installation — dynamic linker hijacking
- C2 — fallback channels
- Actions on Objectives — data from local system
- Reconnaissance — tidak dipakai di latihan (6 dari 7 fase)

### 7.11 Keterbatasan

- Terakhir diubah 2011 — tidak update, ada security gap
- Fokus perimeter dan malware — kalah oleh TTP modern
- Adversary ubah hash dan IP — kelabui threat intelligence
- Deteksi modern — AI dan algoritma
- Tidak bisa deteksi insider threat (CISA: orang dalam salahgunakan akses resmi)
- Solusi — gabung dengan MITRE ATT&CK dan Unified Kill Chain
