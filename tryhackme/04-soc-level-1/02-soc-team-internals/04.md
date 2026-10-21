# Resume Room: SOC Metrics

Tanggal: 21 Oktober 2026

Room ini membahas cara mengukur efisiensi tim SOC lewat metrik internal (Alerts Count, False Positive Rate, Alert Escalation Rate, Threat Detection Rate) dan metrik performa yang biasa masuk SLA (MTTD, MTTA, MTTR), serta cara membaca masalah dari metrik tersebut dan memperbaikinya.

## 1. Gambaran Umum

### 1.1 Tujuan SOC dan Peran L1

Tujuan utama SOC adalah melindungi **confidentiality**, **integrity**, dan **availability** aset digital organisasi. SOC menjalankan tujuan ini dengan mengembangkan, menerima, dan melakukan triase alert.

Peran analyst **L1** dalam proses ini adalah melaporkan **True Positive** secara andal ke level di atasnya, yaitu **L2**.

### 1.2 Mengapa Metrik Penting bagi L1

Metrik dibuat agar SOC lebih efisien, sehingga serangan menjadi jauh lebih sulit berhasil. Metrik juga dipakai untuk mengevaluasi performa analyst; hasil yang baik membuka jalan ke pertumbuhan karier dan kenaikan ke posisi seperti L2.

Pelacakan metrik biasanya tugas SOC manager, tetapi L1 adalah orang pertama yang melihat jumlah alert berlebihan atau False Positive Rate tinggi. L1 perlu tahu cara mengomunikasikan masalah tersebut dan apa yang harus dilakukan untuk memperbaikinya.

### 1.3 Prasyarat

- Room sebelumnya: SOC Workbooks and Lookups.
- Memahami properti alert seperti **severity** dan **verdict**.
- Memahami perbedaan SOC in-house dan managed SOC.

## 2. Metrik Inti

### 2.1 Ringkasan Empat Metrik

| Metrik | Rumus | Mengukur |
|---|---|---|
| Alerts Count (AC) | Total alert yang diterima | Beban kerja keseluruhan analyst |
| False Positive Rate (FPR) | False Positives / Total Alerts | Tingkat noise pada alert |
| Alert Escalation Rate (AER) | Escalated Alerts / Total Alerts | Pengalaman analyst L1 |
| Threat Detection Rate (TDR) | Detected Threats / Total Threats | Keandalan tim SOC |

### 2.2 Alerts Count

Jumlah total alert yang diterima. Terlalu banyak (misalnya 80 alert belum terselesaikan di antrean) membuat analyst kewalahan dan ancaman nyata mudah tersembunyi di balik noise.

Terlalu sedikit juga mencurigakan. Satu minggu penuh tanpa alert bisa menandakan masalah pada **SIEM** atau kurangnya visibility, yang berujung pada breach tak terdeteksi.

Nilai ideal bergantung pada ukuran perusahaan. Acuan umum: **5 sampai 30 alert per hari per analyst L1**.

### 2.3 False Positive Rate

Perbandingan alert yang terbukti False Positive (noise sistem, aktivitas IT biasa) terhadap seluruh alert. Contoh: 75 dari 80 alert (94%) adalah False Positive, ini sinyal buruk.

Noise yang tinggi membuat analyst kurang waspada dan cenderung menganggap semua alert sebagai "spam lagi", sehingga ancaman nyata mudah terlewat.

FPR 0% adalah ideal yang tidak realistis, tetapi **80% atau lebih** sudah masalah serius. Perbaikannya biasanya dengan tuning tool dan detection rule, disebut **False Positive Remediation**.

Contoh hitungan: dari 50 alert hanya 10 yang ancaman nyata, berarti 40 False Positive, sehingga FPR = 40 / 50 = 80%. Perhatikan bahwa 20% adalah True Positive rate, bukan FPR.

### 2.4 Alert Escalation Rate

Mengukur seberapa sering L1 memutuskan untuk meng-escalate alert, sehingga mencerminkan pengalaman dan kemandirian L1.

L2 mengandalkan L1 untuk menyaring noise dan hanya meng-escalate alert yang actionable dan mengancam. Di sisi lain, L1 tidak boleh terlalu percaya diri dan melakukan triase pada alert yang tidak sepenuhnya dipahami tanpa dukungan senior.

Target: **di bawah 50%**, lebih baik **di bawah 20%**.

### 2.5 Threat Detection Rate

Perbandingan ancaman yang terdeteksi terhadap seluruh ancaman yang terjadi. Contoh: dari 6 serangan pada 2025, 4 terdeteksi dan dicegah, 1 terlewat karena detection rule rusak, 1 terlewat karena analyst L1 salah mengklasifikasikannya sebagai False Positive. TDR = 4 / 6 = 67%, hasil yang sangat buruk.

Target: **selalu 100%**, karena satu ancaman yang terlewat bisa berakibat fatal seperti infeksi ransomware dan data exfiltration.

## 3. Metrik Triase dan SLA

### 3.1 Service Level Agreement (SLA)

Alert saja tidak menghentikan breach. Alert harus diterima tepat waktu, di-triase, dan direspons sebelum penyerang mencapai tujuannya. Persyaratan kecepatan ini dikelompokkan dalam **SLA**, yaitu dokumen yang ditandatangani antara tim SOC internal dan manajemen perusahaan, atau antara **MSSP** (managed SOC provider) dan kliennya.

SLA biasanya mensyaratkan tiga hal: deteksi ancaman yang cepat (**MTTD**), acknowledgement alert yang tepat waktu oleh L1 (**MTTA**), dan respons cepat seperti mengisolasi perangkat atau mengamankan akun yang dibobol (**MTTR**).

### 3.2 Alur Waktu Insiden

Urutan kejadian pada diagram room:

1. Kejadian berbahaya terjadi.
2. SOC menerima alert dari SIEM atau EDR. Jeda dari langkah 1 ke 2 adalah **MTTD** (+5 menit pada diagram).
3. Analyst L1 mulai triase. Jeda dari langkah 2 ke 3 adalah **MTTA** (+5 menit).
4. Eskalasi dan workflow lainnya, disebut **Internal Processes** (+20 menit).
5. SOC memitigasi breach sepenuhnya.

**MTTR** pada diagram dihitung dari titik SOC menerima alert sampai breach termitigasi, yaitu MTTA ditambah Internal Processes (+25 menit pada diagram). MTTD tidak termasuk dalam MTTR.

Catatan: tiap tim bisa punya definisi atau rumus metrik berbeda sesuai apa yang ingin diukur. Gunakan diagram dan tabel referensi room sebagai acuan.

### 3.3 Tabel Referensi SLA Umum

| Metrik | SLA umum | Deskripsi |
|---|---|---|
| SOC Team Availability | 24/7 | Jadwal kerja tim SOC, sering Senin-Jumat (8/5) atau mode 24/7 |
| MTTD | 5 menit | Rata-rata waktu antara serangan dan deteksinya oleh tool SOC |
| MTTA | 10 menit | Rata-rata waktu L1 mulai triase alert baru |
| MTTR | 60 menit | Rata-rata waktu SOC benar-benar menghentikan penyebaran breach |

Pada tim 8/5, alert kritis yang masuk hari Sabtu baru di-acknowledge pada hari kerja berikutnya, yaitu Senin.

### 3.4 Cara Menghitung dari Skenario

Contoh: alert diterima setelah 12 menit, L1 memindahkannya ke In Progress 10 menit kemudian, 6 menit setelahnya alert di-escalate ke L2, lalu L2 membersihkan malware selama 35 menit.

- MTTD = 12 menit (waktu sampai alert diterima).
- MTTA = 10 menit (alert diterima sampai L1 mulai triase; berhenti di sini, tidak ditambah waktu eskalasi).
- Internal Processes = 6 + 35 = 41 menit.
- MTTR = MTTA + Internal Processes = 10 + 41 = 51 menit.

## 4. Meningkatkan Metrik

### 4.1 Rekomendasi per Masalah

**False Positive Rate di atas 80%.** Tim menerima terlalu banyak noise. Langkah: kecualikan aktivitas tepercaya seperti system update dari detection rule EDR atau SIEM; pertimbangkan otomatisasi triase untuk alert paling umum menggunakan **SOAR** atau custom script.

**MTTD di atas 30 menit.** Ancaman terdeteksi dengan delay tinggi. Langkah: hubungi SOC engineer agar detection rule berjalan lebih cepat atau lebih sering; pastikan log SIEM dikumpulkan real-time tanpa delay 10 menit.

**MTTA di atas 30 menit.** L1 memulai triase terlalu lambat. Langkah: pastikan analyst mendapat notifikasi real-time saat ada alert baru; distribusikan alert secara merata di antrean antar analyst yang sedang shift.

**MTTR di atas 4 jam.** SOC tidak bisa menghentikan breach tepat waktu. Langkah: sebagai L1, lakukan segala yang mungkin untuk segera meng-escalate ancaman ke L2; pastikan tim sudah mendokumentasikan apa yang harus dilakukan pada berbagai skenario serangan.

### 4.2 Prinsip Penugasan Perbaikan

Pola yang terlihat pada latihan room: masalah dikaitkan ke metrik yang tepat, lalu tugas perbaikan diberikan ke peran yang sesuai.

- Respons lambat karena tidak ada panduan: buat workbook langkah penanganan (misalnya credential rotation), ditugaskan ke L2 yang menangani insiden.
- Deteksi lambat: atur ulang jadwal SIEM dan detection rule agar lebih sering berjalan, ditugaskan ke SOC engineer.
- Noise tinggi: jalankan proses False Positive Remediation dengan mengecualikan noise sistem dan IT dari rule, ditugaskan ke SOC engineer.

## 5. Glossary

**SOC** — Security Operations Center, tim yang memantau dan merespons ancaman keamanan.

**L1 / L2** — Level analyst; L1 melakukan triase awal, L2 menangani alert yang di-escalate.

**Alert** — Peringatan yang dihasilkan SIEM atau EDR untuk aktivitas yang dicurigai.

**Triage** — Proses menilai alert untuk menentukan valid atau tidaknya dan perlu di-escalate atau tidak.

**True Positive** — Alert yang benar-benar ancaman nyata.

**False Positive** — Alert yang ternyata bukan ancaman (noise sistem atau aktivitas IT biasa).

**False Positive Remediation** — Perbaikan tool dan detection rule untuk menurunkan False Positive.

**SLA** — Service Level Agreement, perjanjian target layanan antara SOC dan manajemen atau antara MSSP dan klien.

**MSSP** — Managed SOC provider.

**AC** — Alerts Count.

**FPR** — False Positive Rate.

**AER** — Alert Escalation Rate.

**TDR** — Threat Detection Rate.

**MTTD** — Mean Time to Detect, rata-rata waktu dari serangan sampai terdeteksi.

**MTTA** — Mean Time to Acknowledge, rata-rata waktu dari alert diterima sampai L1 mulai triase.

**MTTR** — Mean Time to Respond, rata-rata waktu SOC benar-benar menghentikan breach.

**Internal Processes** — Eskalasi dan workflow lain setelah L1 memulai triase hingga breach termitigasi.

**SOC Team Availability** — Jadwal kerja tim SOC (8/5 atau 24/7).

**SIEM** — Sistem pengumpul dan analisis log yang menghasilkan alert.

**EDR** — Tool deteksi dan respons di endpoint.

**SOAR** — Platform otomatisasi triase dan respons.

**Detection rule** — Aturan di SIEM atau EDR yang menghasilkan alert.

**Data exfiltration** — Pencurian data keluar dari organisasi.

## 6. Tools & Platform Rujukan

Room ini tidak menyertakan URL eksplisit untuk tools berikut.

- **SIEM** — Sumber alert dan log; detection rule dan jadwal eksekusinya bisa di-tuning.
- **EDR** — Sumber alert dari endpoint; aktivitas tepercaya bisa dikecualikan dari rule.
- **SOAR** — Mengotomatisasi triase alert yang paling umum.
- **Custom script** — Alternatif otomatisasi triase selain SOAR.
- **Room "SOC Workbooks and Lookups"** (TryHackMe) — Prasyarat room ini; membahas workbook dan lookup SOC.

## 7. Catatan Ringkas untuk Ditulis Tangan

### Dasar SOC dan L1

- SOC — lindungi CIA (confidentiality, integrity, availability)
- SOC kerja — kembangkan, terima, triase alert
- L1 — laporkan True Positive ke L2
- Metrik — SOC efisien, evaluasi performa, jalur karier ke L2
- L1 — orang pertama sadar alert berlebihan / FPR tinggi
- Prasyarat — SOC Workbooks and Lookups, severity, verdict, in-house vs managed

### Empat Metrik Inti

- AC — total alert diterima; ukur beban analyst
- FPR = False Positives / Total Alerts; ukur noise
- AER = Escalated / Total Alerts; ukur pengalaman L1
- TDR = Detected Threats / Total Threats; ukur keandalan SOC

### Target dan Ambang

- AC — 5-30 alert/hari/L1; terlalu banyak = kewalahan, nol = cek SIEM/visibility
- FPR — 0% mustahil; 80%+ = masalah serius
- AER — target di bawah 50%, lebih baik di bawah 20%
- TDR — harus 100%
- Contoh FPR — 50 alert, 10 real, FP 40 → 80% (20% = TP rate)
- Contoh TDR — 4 dari 6 = 67%, terlewat karena rule rusak dan salah klasifikasi L1
- Dampak miss — ransomware, data exfiltration

### SLA dan Triase

- SLA — perjanjian SOC-manajemen atau MSSP-klien
- MTTD — serangan sampai terdeteksi; SLA 5 menit
- MTTA — alert diterima sampai L1 mulai triase; SLA 10 menit
- MTTR — SOC hentikan penyebaran breach; SLA 60 menit
- Availability — 24/7 atau 8/5 (Sen-Jum)
- Alert kritis Sabtu di tim 8/5 — di-acknowledge Senin
- Diagram — MTTD (+5), MTTA (+5), Internal Processes (+20), MTTR (+25)
- Rumus — MTTR = MTTA + Internal Processes (tanpa MTTD)
- Contoh soal 12 / 10 / 6 / 35 → MTTD 12, MTTA 10, MTTR 10 + (6 + 35) = 51

### Perbaikan Metrik

- FPR di atas 80% — exclude aktivitas tepercaya dari rule EDR/SIEM; otomatisasi triase dengan SOAR/script
- MTTD di atas 30 menit — SOC engineer percepat rule; cek log SIEM real-time tanpa delay 10 menit
- MTTA di atas 30 menit — notifikasi real-time; distribusi alert merata antar analyst
- MTTR di atas 4 jam — L1 cepat escalate ke L2; dokumentasikan skenario serangan (workbook)
- Penugasan — MTTR → workbook oleh L2; MTTD → jadwal rule oleh SOC engineer; FPR → exclusion rule oleh SOC engineer

### Istilah Kunci

- True Positive — ancaman nyata
- False Positive — bukan ancaman, noise
- False Positive Remediation — tuning tool dan rule
- MSSP — managed SOC provider
- SIEM — log dan alert
- EDR — alert endpoint
- SOAR — otomatisasi triase
- Internal Processes — eskalasi dan workflow setelah triase dimulai
