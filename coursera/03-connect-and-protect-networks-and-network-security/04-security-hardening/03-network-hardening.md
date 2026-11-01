# Resume Network Hardening — Coursera Cybersecurity (24 Oktober 2026)

Cakupan: video Network Hardening, bacaan Network Security Applications, aktivitas Analysis of Network Hardening beserta exemplar, dan kuis Test Your Knowledge: Network Hardening.

## 1. Konsep Inti

### 1.1 Network Hardening

**Network hardening** adalah security hardening yang berfokus pada aspek jaringan: **port filtering**, **network access privileges**, dan **encryption over networks**. Ini kelanjutan dari **OS hardening**, yang berfokus pada keamanan perangkat melalui patch updates, secure configuration, dan account access policies.

Praktik network hardening membantu organisasi memantau ancaman dan serangan potensial pada jaringan serta mencegah sebagian serangan terjadi.

### 1.2 Pembagian Tugas Berdasarkan Frekuensi

Tugas network hardening terbagi dua. Tugas **rutin** dilakukan berkala (harian, dua minggu sekali, atau sebulan sekali). Tugas **satu kali** dikonfigurasi sekali lalu diperbarui bila diperlukan.

### 1.3 Defense in Depth

**Defense in depth** adalah pendekatan menambahkan lapisan-lapisan keamanan pada jaringan. Setiap perangkat, tool, atau strategi memperkuat jaringan sampai pemilik jaringan puas dengan tingkat keamanannya.

Tingkat keamanan minimum diberikan oleh firewall saja. Tingkat tertinggi diperoleh dengan menggabungkan firewall, perangkat intrusion detection and prevention, dan security event monitoring. Profesional keamanan bebas memakai salah satu atau semuanya, tergantung tingkat keamanan yang dituju.

## 2. Tugas Network Hardening Rutin

### 2.1 Daftar Tugas

Tugas rutin meliputi **firewall rules maintenance**, **network log analysis**, **patch updates**, dan **server backups**.

### 2.2 Network Log Analysis

**Log** adalah catatan peristiwa yang terjadi di dalam sistem organisasi. **Network log analysis** adalah proses memeriksa network logs untuk mengidentifikasi **events of interest**. Tim keamanan melakukannya dengan log analyzer tool atau **SIEM**.

Laporan SIEM berisi daftar kerentanan jaringan baru maupun yang masih berlangsung, diurutkan dari prioritas tinggi ke rendah. Kerentanan berprioritas tinggi punya tenggat mitigasi yang jauh lebih singkat.

## 3. Tugas Network Hardening Satu Kali

### 3.1 Port Filtering

**Port filtering** adalah fungsi firewall yang memblokir atau mengizinkan nomor port tertentu untuk membatasi komunikasi yang tidak diinginkan.

Prinsip dasarnya: hanya port yang dibutuhkan operasi jaringan normal yang diizinkan. Port lain yang tidak dipakai harus dilarang (disallowed) agar terlindung dari port vulnerabilities. Prinsip ini juga keluar di kuis; jawaban yang benar adalah mengizinkan port yang digunakan operasi jaringan normal, bukan memblokir semua port.

### 3.2 Protokol Wireless

Jaringan harus memakai **wireless protocols** terbaru yang tersedia, dan protokol wireless lama harus dinonaktifkan.

### 3.3 Network Segmentation

**Network segmentation** membuat subnet terisolasi, misalnya satu untuk departemen marketing dan satu untuk finance. Tujuannya: masalah di satu subnet tidak menyebar ke seluruh perusahaan, dan hanya pengguna tertentu yang mendapat akses ke bagian jaringan sesuai perannya.

Segmentation juga dipakai untuk memisahkan **security zones**. **Restricted zone** yang berisi data sangat rahasia atau terklasifikasi harus dipisahkan dari bagian jaringan lainnya.

### 3.4 Encryption

Seluruh komunikasi jaringan harus dienkripsi dengan **encryption standards** terbaru. Encryption standards adalah aturan atau metode untuk menyembunyikan data keluar dan membuka (decrypt) data masuk. Data di restricted zones membutuhkan standar enkripsi yang jauh lebih tinggi agar lebih sulit diakses.

## 4. Perangkat dan Tools Keamanan Jaringan

### 4.1 Firewall

Firewall mengizinkan atau memblokir traffic berdasarkan sekumpulan aturan. Header packet diperiksa dan diizinkan atau ditolak berdasarkan nomor port. Tiga jenis yang sudah dipelajari: **stateless firewall**, **stateful firewall**, dan **next-generation firewall (NGFW)**. NGFW juga mampu memeriksa **packet payload**.

Setiap sistem sebaiknya punya firewall sendiri, terlepas dari adanya network firewall.

### 4.2 Intrusion Detection System (IDS)

**IDS** memantau aktivitas sistem dan mengirim alert tentang kemungkinan intrusi berdasarkan signature dari malicious traffic. IDS mendeteksi serangan yang dikenal dengan melakukan sniffing terhadap packets yang melintas. Sebagian IDS juga mendeteksi **anomali** yang bisa menjadi tanda aktivitas berbahaya, lalu mengirim alert ke network administrator untuk diselidiki.

Keterbatasan IDS ada dua. Pertama, hanya bisa memindai serangan yang dikenal atau anomali yang jelas, sehingga serangan baru dan canggih bisa lolos. Kedua, IDS tidak menghentikan traffic yang masuk; administrator yang harus menangkap aktivitas berbahaya sebelum merusak.

Penempatan: IDS diletakkan di belakang firewall dan sebelum LAN, sehingga menganalisis aliran data setelah traffic yang ditolak firewall tersaring. Ini mengurangi noise alert yang disebut **false positives**.

### 4.3 Intrusion Prevention System (IPS)

**IPS** memantau aktivitas sistem untuk mendeteksi aktivitas intrusif dan mengambil tindakan menghentikannya. IPS lebih kuat dari IDS karena aktif menghentikan anomali, sedangkan IDS hanya melapor. IPS mencari signature serangan dikenal dan anomali data, melapor ke security analyst, lalu memblokir pengirim tertentu atau men-drop network packets yang mencurigakan.

IPS juga berada di belakang firewall. Aliran data berisiko diputus sebelum mencapai bagian jaringan sensitif.

Keterbatasan IPS: bersifat **inline**, sehingga jika IPS rusak, koneksi antara jaringan privat dan internet ikut putus. Selain itu, false positives dapat menyebabkan traffic sah ikut di-drop.

### 4.4 Full Packet Capture Devices

**Full packet capture devices** merekam dan menganalisis seluruh data yang ditransmisikan melalui jaringan. Perangkat ini juga membantu menyelidiki alert yang dihasilkan IDS.

### 4.5 Security Information and Event Management (SIEM)

**SIEM** adalah aplikasi yang mengumpulkan dan menganalisis data log untuk memantau aktivitas kritis organisasi. SIEM bekerja real time dan melaporkan aktivitas mencurigakan pada dashboard terpusat. Sumber datanya: log dari IDS, IPS, firewall, VPN, proxy, dan DNS.

SIEM mengagregasi security event data sehingga semuanya tampil di satu tempat, disebut **single pane of glass**. Analis dapat memeriksa, menganalisis, dan merespons event berdasarkan prioritas.

SIEM tidak menggantikan keahlian analis maupun aktivitas hardening, melainkan dipakai bersama metode keamanan lain. Analis sering bekerja di **Security Operations Center (SOC)**, lalu memakai keahliannya untuk memutuskan respons dan kapan sebuah event harus dieskalasi ke oversight.

Contoh dashboard **Chronicle** (SIEM Google Cloud) menampilkan Ingested Events 18 M, Throughput 2.07 GB, Alerts 32.660 K, grafik Events Over Time, dan Global Threat Map berisi IOC IP Matches.

### 4.6 Posisi Tiap Perangkat pada Topologi

Topologi dasar: Hosts, LAN, Router, Firewall, Modem, Web service. Pada topologi dengan IDS, urutannya Hosts, LAN, IDS, Switch, Router, Firewall, Modem, Web service. Pada topologi dengan IPS, urutannya Hosts, LAN, IPS, Router, Firewall, Modem, Web service. Security analyst wajib memahami topologi ini.

### 4.7 Perbandingan Kelebihan dan Kekurangan

| Perangkat | Kelebihan | Kekurangan |
|---|---|---|
| Firewall | Mengizinkan atau memblokir traffic berdasarkan aturan | Hanya menyaring berdasarkan informasi di header packet |
| IDS | Mendeteksi dan memberi alert soal intrusi dan traffic berbahaya | Hanya mengenali serangan dikenal atau anomali jelas; tidak menghentikan traffic |
| IPS | Memantau lalu bertindak menghentikan intrusi dan anomali | Inline (jika gagal, koneksi putus); false positives bisa memblokir traffic sah |
| SIEM | Mengumpulkan dan menganalisis log dari banyak mesin pada satu dashboard | Hanya melapor; tidak menghentikan atau mencegah event mencurigakan |

### 4.8 Pertimbangan Biaya

Setiap perangkat atau tool butuh biaya untuk dibeli, dipasang, dan dipelihara. Organisasi mungkin perlu personel tambahan untuk memantau tool tertentu, seperti SIEM. Pengambil keputusan memilih tingkat keamanan berdasarkan biaya dan risiko bagi organisasi.

## 5. Studi Kasus: Analysis of Network Hardening

### 5.1 Skenario

Kamu adalah security analyst di organisasi media sosial yang baru mengalami data breach besar. Informasi pribadi pelanggan seperti nama dan alamat terancam. Organisasi ingin praktik network hardening yang kuat dan bisa dilakukan konsisten.

Empat vulnerabilities yang ditemukan:

1. Karyawan saling berbagi password.
2. Password admin database masih default.
3. Firewall tidak punya rules untuk menyaring traffic masuk dan keluar.
4. Multifactor authentication (MFA) tidak digunakan.

### 5.2 Tugas pada Worksheet

Output berupa **security risk assessment report**. Part one: pilih maksimal tiga hardening tools dan metode yang paling efektif. Part two: berikan satu sampai dua rekomendasi dan jelaskan mengapa efektif serta seberapa sering perlu diterapkan.

Kriteria jawaban: satu sampai tiga tools/metode, dan alasan mengapa pilihan tersebut efektif.

### 5.3 Pemetaan Vulnerability ke Praktik Hardening

Pemetaan berikut adalah penalaran dari materi, bukan isi exemplar resmi.

Password dibagi dan password default ditangani lewat kebijakan password dan akses. MFA yang tidak ada ditangani dengan menerapkan MFA. Firewall tanpa rules ditangani lewat firewall rules maintenance dan port filtering. Praktik rutin seperti network log analysis dengan SIEM membantu mendeteksi aktivitas mencurigakan sejak dini.

## 6. Glossary

- **Network hardening**: security hardening untuk aspek jaringan (port filtering, network access privileges, encryption).
- **Defense in depth**: menambah lapisan keamanan secara bertahap pada jaringan.
- **Log**: catatan peristiwa dalam sistem organisasi.
- **Network log analysis**: memeriksa network logs untuk menemukan events of interest.
- **SIEM**: aplikasi yang mengumpulkan dan menganalisis log pada dashboard terpusat.
- **Single pane of glass**: dashboard tunggal yang menampilkan seluruh security event data.
- **Port filtering**: fungsi firewall yang mengizinkan atau memblokir nomor port tertentu.
- **Network segmentation**: pembagian jaringan menjadi subnet terisolasi atau security zones.
- **Restricted zone**: zona jaringan berisi data sangat rahasia yang dipisahkan dari jaringan lain.
- **Encryption standards**: aturan atau metode untuk menyembunyikan dan membuka data.
- **Stateless firewall, stateful firewall, NGFW**: jenis firewall; NGFW juga memeriksa packet payload.
- **IDS**: mendeteksi dan memberi alert intrusi, tanpa menghentikan traffic.
- **IPS**: mendeteksi dan menghentikan intrusi secara aktif.
- **Inline**: perangkat berada langsung di jalur traffic.
- **False positive**: alert atau pemblokiran atas traffic yang sebenarnya sah.
- **Full packet capture device**: perangkat perekam seluruh data jaringan.
- **SOC (Security Operations Center)**: tempat analis memantau aktivitas jaringan.
- **Event of interest**: peristiwa dalam log yang perlu diperhatikan.

## 7. Tools & Platform Rujukan

- **Chronicle**: SIEM cloud-native milik Google Cloud untuk menyimpan, menganalisis, dan mencari data. URL tidak disebutkan di materi.
- **Splunk**: tool SIEM umum dengan pilihan Splunk Enterprise dan Splunk Cloud, dilengkapi dashboard detail. URL tidak disebutkan di materi.

## 8. Catatan Ringkas untuk Ditulis Tangan

Room ini tidak memuat command atau switch.

### 8.1 Dasar

- Network hardening — hardening aspek jaringan: port filtering, network access privileges, encryption
- OS hardening — patch updates, secure configuration, account access policies
- Defense in depth — lapisan keamanan bertingkat
- Keamanan minimum — firewall saja
- Keamanan tertinggi — firewall + IDS/IPS + SIEM

### 8.2 Tugas Rutin

- Firewall rules maintenance
- Network log analysis
- Patch updates
- Server backups
- Log — catatan peristiwa sistem
- Network log analysis — periksa log cari events of interest
- Alat — log analyzer atau SIEM
- Laporan SIEM — urut prioritas tinggi ke rendah; prioritas tinggi tenggat mitigasi lebih singkat

### 8.3 Tugas Satu Kali

- Port filtering — firewall blokir/izinkan nomor port
- Prinsip port filtering — izinkan hanya port yang dipakai operasi normal
- Wireless — pakai protokol terbaru, matikan yang lama
- Network segmentation — subnet terisolasi per departemen
- Tujuan segmentation — masalah tidak menyebar, akses sesuai peran
- Security zones — restricted zone dipisah dari jaringan lain
- Encryption — semua komunikasi pakai standar terbaru
- Restricted zone — standar enkripsi jauh lebih tinggi

### 8.4 Perangkat

- Firewall — izin/blokir berdasarkan rules dan header packet
- Jenis firewall — stateless, stateful, NGFW
- NGFW — juga periksa packet payload
- Tiap sistem — sebaiknya punya firewall sendiri
- IDS — deteksi dan alert; tidak menghentikan traffic
- IDS kelemahan — hanya serangan dikenal/anomali jelas
- IDS posisi — di belakang firewall, sebelum LAN
- False positives — noise alert
- IPS — deteksi dan hentikan (blokir pengirim, drop packets)
- IPS kelemahan — inline (rusak = koneksi putus), false positives
- Full packet capture — rekam semua data, bantu selidiki alert IDS
- SIEM — kumpul dan analisis log, dashboard terpusat
- Sumber SIEM — IDS, IPS, firewall, VPN, proxy, DNS
- SIEM kelemahan — hanya melapor, tidak bertindak
- Single pane of glass — satu dashboard untuk semua event
- SOC — tempat analis memantau jaringan
- Chronicle — SIEM Google Cloud
- Splunk — SIEM; Enterprise dan Cloud
- Biaya — beli, pasang, pelihara, kadang personel tambahan

### 8.5 Studi Kasus

- Skenario — media sosial, data breach (nama, alamat pelanggan)
- Vulnerability 1 — password dibagi antar karyawan
- Vulnerability 2 — password admin database default
- Vulnerability 3 — firewall tanpa rules
- Vulnerability 4 — MFA tidak dipakai
- Worksheet part one — pilih maks 3 tools/metode
- Worksheet part two — 1-2 rekomendasi, alasan efektif, seberapa sering diterapkan
- Output — security risk assessment report
