# Resume Materi: Defensive Security
**Platform:** TryHackMe — Pre Security Path
**Tanggal:** 08 September 2026

---

## 1. Konsep Inti Defensive Security

**Defensive security** adalah cabang keamanan siber yang berfokus pada memahami apa yang perlu dilindungi dan menerapkan langkah-langkah keamanan untuk mencegah, mendeteksi, dan memitigasi dampak dari potensi serangan.

Berbeda dari offensive security yang berupaya menerobos masuk, defensive security memastikan tidak ada pihak yang bisa masuk tanpa izin — dan jika pun terjadi insiden, sistem sudah siap merespons dengan cepat.

Pelaku utama defensive security disebut **Blue Team**, yaitu kelompok defender yang bertanggung jawab melindungi sistem dan merespons ancaman. Blue Team perlu memahami cara kerja penyerang bukan untuk menyerang, melainkan agar bisa membangun pertahanan yang benar-benar efektif.

---

## 2. CIA Triad — Fondasi Keamanan Informasi

Semua upaya defensive security berakar pada tiga prinsip yang dikenal sebagai **CIA Triad**:

- **Confidentiality** — Data hanya boleh diakses oleh pihak yang berwenang. Informasi sensitif tidak boleh bocor ke tangan yang salah.
- **Integrity** — Data tidak boleh diubah, dihapus, atau dimanipulasi tanpa izin. Apa yang tersimpan harus bisa dipercaya keasliannya.
- **Availability** — Sistem dan data harus selalu bisa diakses oleh pihak yang berwenang saat dibutuhkan. Gangguan terhadap ketersediaan sistem juga termasuk insiden keamanan.

---

## 3. Memahami Lingkungan (Understanding Your Environment)

### 3.1 Mengapa Visibilitas Itu Wajib

Sebelum bisa melindungi apapun, defender harus punya **visibilitas** yang jelas — tahu apa saja sistem yang ada, bagaimana mereka saling terhubung, dan di mana letak titik-titik rentannya.

Analogi yang dipakai: bayangkan infrastruktur klien seperti sebuah kota. Penjaga kota harus tahu tata letak kota, mengawasi masalah yang muncul, dan tahu cara merespons ancaman. Defender bekerja dengan cara yang sama.

### 3.2 Empat Pertanyaan Dasar Defender

| Pertanyaan | Setara di Dunia Nyata |
|---|---|
| Apa yang dilindungi? | Server, data, workstation, pengguna |
| Bisakah kamu melihatnya? | Logs, network traffic, alerts |
| Apa perilaku mencurigakan? | Login berulang gagal, IP tidak biasa |
| Bagaimana hentikan ancaman? | Firewall rules, IP address blocking |

### 3.3 Scope — Batas Tanggung Jawab Defender

Defender tidak bertanggung jawab melindungi semua hal di internet. Fokusnya hanya pada aset milik organisasi atau klien, yaitu:

- Perangkat yang digunakan karyawan sehari-hari
- Server yang meng-host aplikasi dan data
- Jaringan yang menghubungkan semua sistem tersebut

Sebelum pertahanan apapun diterapkan, defender harus terlebih dahulu tahu sistem apa saja yang ada, untuk apa digunakan, dan bagaimana posisinya dalam keseluruhan lingkungan.

### 3.4 Komponen Infrastruktur Klien

Berikut komponen yang paling umum ditemukan dalam infrastruktur klien:

**Employee Devices**
Perangkat tempat karyawan bekerja dan mengakses sumber daya perusahaan. Analoginya seperti rumah — tempat aktivitas utama sehari-hari.

**Web Server**
Server yang meng-host website atau aplikasi yang diakses oleh pengguna. Analoginya seperti toko atau gedung publik — tempat orang datang untuk mendapatkan layanan.

**Mail Server**
Server yang mengirim dan menerima email untuk seluruh organisasi. Analoginya seperti kantor pos — pusat komunikasi surat-menyurat.

**Firewall**
Sistem yang mengontrol traffic apa yang diizinkan masuk atau keluar dari jaringan. Analoginya seperti gerbang kota — penjaga yang memutuskan siapa boleh masuk dan keluar.

**Internet**
Jaringan eksternal yang tidak dikontrol oleh organisasi. Analoginya seperti wilayah di luar kota — tidak bisa dikontrol dan menjadi sumber ancaman eksternal.

---

## 4. Lima Konsep Fondasi Defensive Security

Setelah memahami lingkungan, defender mengorganisir pekerjaannya di sekitar lima konsep fundamental yang berlaku di hampir semua lingkungan.

**Prevention (Pencegahan)**
Menempatkan kontrol keamanan untuk menghentikan serangan sebelum terjadi. Implementasinya berupa firewall, antivirus, dan regular patching untuk menutup celah sebelum dieksploitasi.

**Detection (Deteksi)**
Memonitor sistem dan jaringan secara aktif untuk mengidentifikasi aktivitas mencurigakan. Dilakukan melalui logs, alerts otomatis, dan security tools yang menganalisis pola traffic.

**Mitigation (Mitigasi)**
Mengambil tindakan cepat saat insiden berlangsung untuk membatasi kerusakan agar tidak meluas. Contohnya: memblokir IP penyerang, mengisolasi sistem yang terkompromikan, atau menonaktifkan akun yang dicuri kredensialnya.

**Analysis (Analisis)**
Menyelidiki secara mendalam apa yang terjadi, bagaimana bisa terjadi, dan sistem mana yang terdampak. Dilakukan dengan meninjau logs dan semua bukti digital yang tersedia setelah insiden.

**Response and Improvement (Respons dan Peningkatan)**
Memulihkan sistem dari insiden dan meningkatkan pertahanan agar kejadian serupa tidak terulang. Tahap ini menutup siklus dan memulai kembali dari posisi yang lebih kuat.

---

## 5. The Defender Mindset

### 5.1 Lihat Sistem sebagai Rantai, Bukan Bagian Terpisah

Penyerang jarang menarget satu sistem saja. Mereka mengkompromikan satu aset, lalu berpindah ke aset berikutnya, dan terus membangun hingga mencapai tujuan akhir.

Contoh nyata: lampiran email berbahaya menginfeksi workstation karyawan → penyerang mencuri kredensial dari workstation → masuk ke mail server → akhirnya mencapai database server yang menyimpan data sensitif.

Setiap langkah membangun dari langkah sebelumnya. Dan setiap langkah juga merupakan kesempatan bagi defender untuk memotong rantai serangan tersebut.

### 5.2 Empat Prinsip Utama Defender Mindset

**Threat Anticipation**
Tinjau setiap sistem yang dilindungi dan tanyakan secara aktif "bagaimana jika?" Bayangkan jalur-jalur realistis yang mungkin diambil penyerang, lalu persiapkan pertahanan di titik-titik tersebut sebelum dieksploitasi.

**Attack Awareness**
Serangan siber mengikuti tahapan-tahapan yang dapat dikenali, bukan acak. Dengan mempelajari rantai serangan umum dan framework yang digunakan penyerang, defender bisa lebih cepat mengenali tanda-tanda serangan sebelum mencapai tahap kritis.

**Risk Prioritization**
Tidak semua bagian sistem membawa risiko yang sama. Defender harus mengidentifikasi sistem dan aset yang bernilai tinggi agar alokasi sumber daya pertahanan bisa difokuskan di tempat yang paling penting.

**Continuous Adaptation**
Pertahanan bukan sesuatu yang dipasang sekali lalu ditinggalkan. Ancaman terus berkembang, teknik serangan berubah, dan kerentanan baru terus ditemukan. Defender harus selalu memperbarui pengetahuan dan pertahanannya.

---

## 6. Pertahanan Berlapis per Komponen

Tidak ada satu alat yang bisa menghentikan semua serangan. Prinsipnya adalah **layered defense** — seperti kunci di pintu, penjaga di gerbang, dan alarm di dalam gedung secara bersamaan. Semakin banyak lapisan, semakin sulit penyerang untuk berhasil.

| Komponen | Risiko Utama | Pertahanan |
|---|---|---|
| Employee Devices | Klik link jahat / download software berbahaya | Antivirus + update rutin |
| Web Server | Penyerang coba terobos website | Hanya izinkan traffic aman + enkripsi |
| Mail Server | Email phishing / lampiran jahat | Spam filter + scan lampiran |
| Firewall | Akses tidak sah dari internet | Firewall rules + blokir IP berbahaya |
| Internet | Semua ancaman eksternal | Batasi inbound traffic + monitoring aktif |

---

## 7. Jalur Karir Defensive Security

**Security Operations Center (SOC)**
Memantau jaringan dan sistem secara terus-menerus untuk mendeteksi dan menyelidiki aktivitas mencurigakan atau security alerts. Ini adalah garis terdepan dalam defensive security — tempat di mana deteksi dan respons awal terjadi.

**Threat Intelligence**
Meneliti ancaman terkini, mempelajari taktik dan teknik penyerang, serta menganalisis tren serangan. Tujuannya adalah membantu organisasi mempersiapkan pertahanan secara proaktif sebelum serangan benar-benar terjadi.

**Digital Forensics and Incident Response (DFIR)**
Menyelidiki insiden keamanan secara mendalam untuk memahami bagaimana serangan terjadi, seberapa jauh dampaknya, dan bagaimana memulihkan sistem yang terdampak agar kembali beroperasi normal.

---

## 8. Glossary — Istilah Penting

**Blue Team** — Kelompok defender keamanan siber yang bertugas melindungi sistem dan merespons ancaman.

**Client Infrastructure** — Keseluruhan jaringan, server, perangkat, dan aplikasi milik sebuah organisasi yang membutuhkan perlindungan.

**Visibility** — Kemampuan untuk melihat dan memantau aktivitas di seluruh sistem sehingga potensi masalah bisa terdeteksi sedini mungkin.

**Threat** — Bahaya potensial seperti hacker atau malware yang bisa merusak sistem atau mencuri data.

**Prevention** — Menghentikan ancaman sebelum bisa menyebabkan kerusakan dengan cara memblokir atau mengurangi peluang serangan terjadi.

**Detection** — Proses mengidentifikasi ancaman atau aktivitas mencurigakan yang sedang berlangsung di jaringan dan sistem.

**Mitigation** — Tindakan yang diambil untuk mengurangi atau menghentikan dampak dari sebuah ancaman setelah teridentifikasi.

**Risk** — Kemungkinan dan potensi dampak dari sebuah ancaman yang berhasil merugikan organisasi.

**Attack Surface** — Semua titik dan celah yang berpotensi bisa diserang dalam suatu sistem atau lingkungan.

**Layered Defense** — Strategi pertahanan berlapis yang menggunakan kombinasi beberapa kontrol keamanan sehingga tidak bergantung pada satu alat saja.

**Logs** — Catatan aktivitas sistem yang digunakan untuk audit, investigasi, dan deteksi aktivitas mencurigakan.

**Patching** — Pembaruan software untuk menutup celah keamanan yang ditemukan sebelum bisa dieksploitasi.

**Firewall Rules** — Aturan yang menentukan traffic jaringan mana yang diizinkan masuk atau keluar dari sistem.

**Incident Response** — Respons terstruktur yang dilakukan saat terjadi insiden keamanan, mulai dari deteksi hingga pemulihan.

**SOC** — Security Operations Center; unit yang memantau dan merespons ancaman keamanan secara real-time.

**DFIR** — Digital Forensics and Incident Response; kombinasi investigasi forensik digital dan penanganan insiden keamanan.

**CIA Triad** — Tiga prinsip fondasi keamanan informasi: Confidentiality, Integrity, dan Availability.

---

## 9. Tools & Platform Rujukan

Room ini tidak menyebutkan tools atau platform eksternal spesifik secara eksplisit. Materi bersifat konseptual dan tidak mencakup demonstrasi penggunaan tools tertentu.

---

## 10. Catatan Ringkas untuk Ditulis Tangan

### Konsep Inti

- Defensive security — cegah, deteksi, mitigasi serangan
- Blue Team — kelompok defender
- Tujuan — siap hadapi & respons insiden

### CIA Triad

- Confidentiality — data hanya untuk yang berwenang
- Integrity — data tidak boleh diubah sembarangan
- Availability — sistem harus selalu bisa diakses

### 5 Konsep Fondasi

- Prevention — hentikan sebelum terjadi (firewall, antivirus, patching)
- Detection — monitor & identifikasi (logs, alerts, tools)
- Mitigation — batasi kerusakan saat insiden (isolasi, blokir, nonaktifkan akun)
- Analysis — selidiki apa & bagaimana (review logs & bukti)
- Response & Improvement — pulihkan & perkuat pertahanan

### Komponen Infrastruktur

- Employee Devices — tempat kerja karyawan → Antivirus + update
- Web Server — host website/aplikasi → Traffic aman + enkripsi
- Mail Server — kirim/terima email → Spam filter + scan lampiran
- Firewall — kontrol traffic masuk/keluar → Rules + blokir IP
- Internet — jaringan eksternal → Batasi inbound + monitoring

### Defender Mindset

- Lihat sistem sebagai rantai, bukan bagian terpisah
- Threat Anticipation — tanya "bagaimana jika?"
- Attack Awareness — pelajari rantai & tahapan serangan
- Risk Prioritization — fokus ke aset bernilai tinggi
- Continuous Adaptation — ancaman terus berkembang

### Pertahanan Berlapis

- Layered defense — kombinasi banyak kontrol, tidak bergantung 1 alat
- Tidak ada 1 tool yang cukup untuk semua ancaman

### Jalur Karir

- SOC — monitor & deteksi ancaman real-time
- Threat Intelligence — riset ancaman & taktik penyerang
- DFIR — investigasi insiden & pemulihan sistem

### Istilah Kunci

- Visibility — kemampuan lihat & pantau aktivitas sistem
- Attack Surface — semua titik yang bisa diserang
- Logs — catatan aktivitas untuk deteksi & investigasi
- Patching — update software tutup celah keamanan
- Scope — batas aset yang jadi tanggung jawab defender
