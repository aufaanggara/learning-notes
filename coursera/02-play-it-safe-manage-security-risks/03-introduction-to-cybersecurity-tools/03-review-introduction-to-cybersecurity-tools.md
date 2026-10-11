# Resume Materi Room: Log dan SIEM (Course 2, Module 3)

Tanggal: 11 Oktober 2026

Cakupan: pentingnya log, jenis-jenis log, SIEM beserta dashboard dan manfaatnya, jenis tool SIEM berdasarkan model hosting, contoh tool (Splunk dan Chronicle), skenario analisis insiden, serta pengantar menuju playbook.

## 1. Konsep Dasar

### 1.1 Keamanan Siber dan Operasional Bisnis

Keamanan siber melindungi operasional bisnis dengan menjaga proses, teknologi, dan strategi yang melindungi aset digital organisasi (data pelanggan, sistem internal, toko online) dari ancaman yang dapat mengganggu operasional.

Contoh dampak: web server utama toko online yang crash berulang membuat pelanggan tidak bisa menyelesaikan pembelian, sehingga langsung menghambat bisnis.

### 1.2 Log

**Log** adalah catatan peristiwa yang terjadi di dalam sistem sebuah organisasi. Log merekam hal seperti siapa yang login, file apa yang diakses, dan apakah sebuah layanan mengalami error.

Log menjadi dasar investigasi karena menunjukkan apa yang dilakukan sistem sebelum sebuah masalah terjadi.

### 1.3 Metrics

**Metrics** adalah atribut teknis utama seperti response time, availability, dan failure rate. Metrics dipakai untuk menilai kinerja sebuah aplikasi perangkat lunak.

Jangan tertukar: log adalah catatan peristiwa, sedangkan metrics adalah ukuran kinerja.

## 2. Jenis-Jenis Log

Tiga jenis log yang dibahas, masing-masing memberi wawasan berbeda tentang postur keamanan (security posture).

| Jenis log | Isi yang dicatat | Contoh kegunaan |
|---|---|---|
| Server log | Login karyawan, permintaan username, error, warning, lalu lintas tinggi | Mencari penyebab server crash |
| Network log | Koneksi antara perangkat dan layanan di jaringan | Melihat pola lalu lintas |
| Firewall log | Aktivitas pada firewall | Melengkapi gambaran keamanan |

Catatan penting: permintaan username dicatat di **server log**, bukan di network log. Ini adalah jebakan pada soal.

Untuk analisis network log secara mendalam, dipakai **network protocol analyzer (packet sniffer)** seperti Wireshark, yang melakukan analisis paket secara detail.

## 3. SIEM

### 3.1 Definisi

**Security information and event management (SIEM)** adalah aplikasi yang mengumpulkan dan menganalisis data log untuk memantau aktivitas kritis dalam sebuah organisasi.

**SIEM tools** adalah platform perangkat lunak yang mengumpulkan, menganalisis, dan mengorelasikan data keamanan dari berbagai sumber di seluruh infrastruktur IT. Fungsinya membantu mengidentifikasi dan merespons ancaman secara real-time, menyelidiki insiden keamanan, dan mematuhi regulasi keamanan.

### 3.2 Mengapa Investigasi Manual Sulit

Menyelidiki satu insiden memerlukan log dari banyak sumber (web server, firewall, jaringan). Mengecek tiap log satu per satu secara manual lambat dan melelahkan.

SIEM memusatkan semua log ke satu tempat, sehingga pencarian dan penyaringan (filter) bisa dilakukan sekaligus dan cepat.

### 3.3 Manfaat Utama SIEM

- Mengumpulkan data log dari berbagai sumber.
- Menyediakan pemantauan dan analisis peristiwa (event).
- Menghemat waktu.

SIEM tidak berarti menghilangkan kebutuhan peninjauan log secara manual. Pernyataan itu salah pada soal.

### 3.4 Dashboard SIEM

Dashboard SIEM memakai representasi visual untuk memberi tim keamanan wawasan yang cepat dan jelas tentang postur keamanan organisasi.

Kegunaan dashboard dalam investigasi:

- Memberi tampilan terpusat sehingga pola lebih mudah terlihat.
- Menampilkan timeline percobaan login, lokasi, dan waktu aktivitas. Contoh: ratusan percobaan login dari lokasi geografis tidak biasa dalam beberapa menit terakhir.
- Mendukung penyaringan berdasarkan **severity** untuk memprioritaskan error atau alert paling kritis lebih dulu.

### 3.5 Skenario: Web Server Crash

Web server toko online crash berulang. Langkah analisis:

1. Periksa server log untuk melihat apa yang terjadi sebelum server gagal (error, warning, lalu lintas tinggi).
2. Periksa network log untuk melihat pola lalu lintas, dan analisis paket dengan Wireshark bila perlu.
3. Gunakan SIEM untuk mengumpulkan log dari web server, firewall, dan jaringan di satu tempat.
4. Buka dashboard dan filter berdasarkan severity.
5. Bedakan gangguan teknis (technical glitch) dengan serangan jahat seperti **Denial of Service (DoS)**.

Indikator serangan DoS: lonjakan traffic yang masif dan terjadi bersamaan, atau permintaan identik yang berulang. Ini berbeda dari error teknis yang bersifat acak.

### 3.6 Area Pengembangan Lanjutan

Dari umpan balik sesi: pelajari lebih jauh fungsi SIEM seperti incident response otomatis dan visualisasi lanjutan untuk memangkas waktu mitigasi ancaman, serta jenis-jenis alert keamanan spesifik yang dapat dihasilkan SIEM untuk membedakan berbagai jenis insiden.

## 4. Jenis Tool SIEM

### 4.1 Berdasarkan Model Hosting

| Jenis | Ciri | Contoh kasus |
|---|---|---|
| Cloud-hosted | Berjalan di cloud | Splunk Cloud |
| Self-hosted | Dipasang di infrastruktur milik sendiri, data privat di server internal | Splunk Enterprise |
| Hybrid | Memerlukan infrastruktur on-site sekaligus solusi berbasis internet | Gabungan keduanya |

### 4.2 Tool yang Dibahas

**Splunk Cloud** adalah tool yang di-host di cloud untuk mengumpulkan, mencari, dan memantau data log.

**Splunk Enterprise** adalah tool yang di-host sendiri untuk menyimpan, menganalisis, dan mencari data log organisasi guna menyediakan informasi keamanan dan alert secara real-time.

**Chronicle** adalah tool cloud-native yang dirancang untuk menyimpan, menganalisis, dan mencari data.

### 4.3 Tool Open-Source

Eksposur yang luas dan akses langsung ke source code pada tool open-source membuat kemungkinan terjadinya masalah menjadi **lebih kecil**, karena banyak pihak dapat menemukan dan memperbaikinya.

## 5. Incident Response, Playbook, dan SOAR

**Incident response** adalah upaya cepat organisasi untuk mengidentifikasi serangan, membatasi kerusakan, dan memperbaiki dampak pelanggaran keamanan (security breach).

**Playbook** adalah manual yang menyediakan detail tentang setiap tindakan operasional. Materi berikutnya membahas bagaimana playbook membantu profesional keamanan merespons secara tepat terhadap ancaman, risiko, dan kerentanan.

**Security orchestration, automation, and response (SOAR)** adalah kumpulan aplikasi, tool, dan alur kerja yang memakai otomatisasi untuk merespons peristiwa keamanan.

Perbedaan inti: SIEM mengumpulkan dan menganalisis data log, sedangkan SOAR mengotomatiskan respons terhadap peristiwa keamanan.

## 6. Glossary

- **Chronicle**: tool cloud-native untuk menyimpan, menganalisis, dan mencari data.
- **Incident response**: upaya cepat mengidentifikasi serangan, membatasi kerusakan, dan memperbaiki dampak pelanggaran keamanan.
- **Log**: catatan peristiwa yang terjadi di sistem organisasi.
- **Metrics**: atribut teknis utama (response time, availability, failure rate) untuk menilai kinerja aplikasi.
- **Operating system (OS)**: antarmuka antara perangkat keras komputer dan pengguna.
- **Playbook**: manual yang menyediakan detail tentang setiap tindakan operasional.
- **Security information and event management (SIEM)**: aplikasi yang mengumpulkan dan menganalisis data log untuk memantau aktivitas kritis.
- **Security orchestration, automation, and response (SOAR)**: kumpulan aplikasi, tool, dan workflow yang memakai otomatisasi untuk merespons peristiwa keamanan.
- **SIEM tools**: platform yang mengumpulkan, menganalisis, dan mengorelasikan data keamanan dari berbagai sumber untuk deteksi dan respons ancaman real-time, investigasi insiden, dan kepatuhan regulasi.
- **Splunk Cloud**: tool berbasis cloud untuk mengumpulkan, mencari, dan memantau data log.
- **Splunk Enterprise**: tool self-hosted untuk menyimpan, menganalisis, dan mencari data log, dengan informasi keamanan dan alert real-time.

## 7. Tools & Platform Rujukan

Materi ini tidak mencantumkan URL eksplisit, sehingga hanya nama yang ditulis.

- **Splunk Cloud**: SIEM berbasis cloud untuk mengumpulkan, mencari, dan memantau data log.
- **Splunk Enterprise**: SIEM self-hosted untuk menyimpan, menganalisis, dan mencari data log.
- **Chronicle**: SIEM cloud-native untuk menyimpan, menganalisis, dan mencari data.
- **Wireshark**: network protocol analyzer (packet sniffer) untuk analisis paket mendalam pada network log.

## 8. Catatan Ringkas untuk Ditulis Tangan

### Dasar

- Keamanan siber — jaga operasional bisnis dari ancaman terhadap aset digital
- Log — catatan peristiwa di sistem organisasi
- Metrics — ukuran kinerja aplikasi: response time, availability, failure rate
- OS — antarmuka hardware dan pengguna

### Jenis Log

- Server log — login karyawan, permintaan username, error, warning, traffic tinggi
- Network log — koneksi antar perangkat dan layanan
- Firewall log — aktivitas firewall
- Wireshark — network protocol analyzer (packet sniffer), analisis paket mendalam
- Jebakan: username request masuk server log, bukan network log

### SIEM

- SIEM — kumpulkan dan analisis log untuk pantau aktivitas kritis
- SIEM tools — kumpulkan, analisis, korelasikan data dari banyak sumber; deteksi dan respons real-time, investigasi, kepatuhan
- Alasan SIEM — manual cek tiap log lambat; SIEM satu tempat plus filter cepat
- Manfaat — kumpulkan log berbagai sumber, monitoring dan analisis event, hemat waktu
- Bukan manfaat — menghilangkan peninjauan manual
- Dashboard — visual, insight cepat dan jelas, tampilan terpusat
- Dashboard dipakai untuk — timeline login, lokasi, waktu; filter severity

### Analisis Insiden

- Skenario — web server crash, pelanggan gagal beli
- Alur — server log, network log, SIEM, dashboard, filter severity
- Technical glitch vs DoS — cek pola
- Ciri DoS — lonjakan masif, simultan, request identik berulang
- Alert login dari lokasi geografis tak biasa — cek di dashboard SIEM
- Pendalaman — incident response otomatis, visualisasi lanjutan, jenis alert SIEM

### Jenis Tool dan Contoh

- Cloud-hosted — berjalan di cloud (Splunk Cloud)
- Self-hosted — di infrastruktur sendiri, data privat di server internal (Splunk Enterprise)
- Hybrid — on-site plus internet
- Chronicle — cloud-native, simpan, analisis, cari data
- Open-source — eksposur luas dan akses source code, masalah lebih kecil kemungkinannya

### Respons dan Playbook

- Incident response — identifikasi serangan, batasi kerusakan, perbaiki dampak breach
- Playbook — manual detail tiap tindakan operasional
- SOAR — aplikasi, tool, workflow dengan otomatisasi untuk respons peristiwa keamanan
- SIEM vs SOAR — SIEM analisis log, SOAR otomatisasi respons
