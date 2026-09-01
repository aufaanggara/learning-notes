# Resume: Introduction to Incident Response
**Platform:** TryHackMe
**Tanggal Penyelesaian:** 2026-09-01

---

## 1. Konsep Dasar

### 1.1 Event dan Log

Setiap proses yang berjalan di perangkat — baik proses interaktif (dijalankan langsung oleh pengguna) maupun non-interaktif (berjalan di latar belakang) — menghasilkan **event**. Setiap event dicatat dalam bentuk **log**.

Event dihasilkan dalam jumlah sangat besar setiap saat. Untuk menyaringnya, event-event ini dimasukkan ke dalam **solusi keamanan (security solution)** yang menganalisis log dan mendeteksi aktivitas mencurigakan.

### 1.2 Alert: False Positive vs True Positive

Ketika solusi keamanan mendeteksi aktivitas mencurigakan, ia akan memicu **alert**. Alert terbagi menjadi dua jenis:

- **False Positive** — alert yang terlihat berbahaya tetapi sebenarnya tidak. Contoh: sistem memindai volume transfer data besar, ternyata itu proses backup ke cloud.
- **True Positive** — alert yang menunjukkan aktivitas yang benar-benar berbahaya. Contoh: sistem mendeteksi email phishing yang nyata dan berhasil mengkompromikan pengguna.

### 1.3 Incident

**Incident (Insiden)** adalah true positive alert yang dikonfirmasi sebagai ancaman nyata. Setelah dikategorikan sebagai insiden, tim keamanan memberikan **severity level (tingkat keparahan)**.

Tingkatan severity dari terendah ke tertinggi:

- Low (Rendah)
- Medium (Sedang)
- High (Tinggi)
- Critical (Kritis)

Insiden dengan severity **Critical** selalu menjadi prioritas penanganan tertinggi, diikuti High, Medium, dan Low.

---

## 2. Jenis-Jenis Insiden

### 2.1 Malware Infections (Infeksi Malware)

Program jahat yang merusak sistem, jaringan, atau aplikasi. Merupakan jenis insiden paling umum. Malware dapat masuk melalui file berupa teks, dokumen, executable, dan sebagainya.

### 2.2 Security Breaches (Pelanggaran Keamanan)

Terjadi ketika pihak tidak berwenang berhasil mengakses data rahasia yang hanya boleh diakses oleh personel tertentu. Bersifat intentional (disengaja oleh penyerang).

### 2.3 Data Leaks (Kebocoran Data)

Informasi rahasia milik individu atau organisasi terekspos ke pihak yang tidak berwenang. Berbeda dari Security Breach — data leak **bisa terjadi secara tidak sengaja** akibat human error atau misconfiguration. Sering digunakan penyerang untuk memeras atau merusak reputasi korban.

### 2.4 Insider Attacks (Serangan dari Dalam)

Serangan yang berasal dari dalam organisasi sendiri, dilakukan secara sengaja oleh orang yang memiliki akses ke sistem. Sangat berbahaya karena orang dalam memiliki akses lebih luas dibanding penyerang dari luar.

### 2.5 Denial of Service (DoS) Attacks

Penyerang membanjiri sistem, jaringan, atau aplikasi dengan permintaan palsu hingga sumber daya habis dan layanan tidak dapat diakses oleh pengguna yang sah. Menyerang **availability** — salah satu dari tiga pilar keamanan siber (CIA Triad).

> Catatan: Setiap jenis insiden memiliki dampak yang berbeda-beda tergantung konteks organisasi. Insiden yang berdampak besar bagi satu organisasi bisa berdampak kecil bagi organisasi lain.

---

## 3. Incident Response Frameworks

### 3.1 Framework SANS — PICERL

SANS menyusun incident response dalam **6 fase** yang disingkat **PICERL**:

| Fase | Penjelasan Singkat |
|---|---|
| **Preparation** | Membangun sumber daya: tim IR, rencana IR, dan solusi keamanan sebelum insiden terjadi |
| **Identification** | Mendeteksi perilaku abnormal menggunakan tools dan teknik pemantauan |
| **Containment** | Membatasi dampak insiden: isolasi mesin, nonaktifkan akun yang dikompromikan |
| **Eradication** | Menghapus ancaman dari lingkungan yang terinfeksi hingga bersih |
| **Recovery** | Memulihkan sistem dari backup atau membangun ulang, lalu menguji sebelum digunakan kembali |
| **Lessons Learned** | Mendokumentasikan celah deteksi dan pelajaran untuk meningkatkan proses ke depan |

### 3.2 Framework NIST — 4 Fase

NIST menyederhanakan menjadi **4 fase** dengan menggabungkan beberapa fase SANS:

| NIST | Setara dengan SANS |
|---|---|
| Preparation | Preparation |
| Detection and Analysis | Identification |
| Containment, Eradication, and Recovery | Containment + Eradication + Recovery |
| Post-Incident Activity | Lessons Learned |

Kedua framework bersifat siklus — setelah fase terakhir selesai, proses kembali ke Preparation untuk perbaikan berkelanjutan.

---

## 4. Incident Response Plan

**Incident Response Plan (IR Plan)** adalah dokumen formal terstruktur yang menguraikan pendekatan organisasi dalam menangani insiden. Dokumen ini:

- Disetujui secara resmi oleh manajemen senior
- Berlaku sebelum, selama, dan setelah insiden

Komponen kunci IR Plan:

- **Roles and Responsibilities** — siapa yang bertanggung jawab atas apa
- **Incident Response Methodology** — metode yang digunakan tim
- **Communication Plan** — rencana komunikasi dengan stakeholder, termasuk penegak hukum
- **Escalation Path** — jalur eskalasi yang harus diikuti saat insiden terjadi

---

## 5. Tools Incident Response

### 5.1 SIEM (Security Information and Event Management)

Mengumpulkan semua log dari berbagai sumber ke satu lokasi terpusat, lalu mengkorelasikannya untuk mengidentifikasi insiden. Fungsi utama: **deteksi dan korelasi**.

### 5.2 AV (Antivirus)

Mendeteksi malware yang sudah dikenal berdasarkan database signature, dan secara rutin memindai sistem. Fungsi utama: **deteksi malware berbasis signature**.

### 5.3 EDR (Endpoint Detection and Response)

Diterapkan di setiap endpoint (perangkat individual). Mampu mendeteksi ancaman tingkat lanjut yang tidak dikenal AV, sekaligus dapat melakukan **containment dan eradication** secara otomatis.

---

## 6. Playbook dan Runbook

### 6.1 Playbook

Panduan langkah demi langkah untuk merespons jenis insiden tertentu. Bersifat **high-level** dan disesuaikan per tipe insiden.

Contoh Playbook untuk insiden **Phishing Email**:
1. Beritahu semua stakeholder tentang insiden
2. Analisis header dan isi email untuk memverifikasi apakah berbahaya
3. Cari dan analisis lampiran yang ada
4. Tentukan apakah ada pengguna yang membuka lampiran
5. Isolasi sistem yang terinfeksi dari jaringan
6. Blokir pengirim email

### 6.2 Runbook

Eksekusi **detail dan teknis** dari langkah-langkah spesifik selama insiden. Bersifat **low-level**, dapat bervariasi tergantung sumber daya yang tersedia untuk investigasi. Runbook adalah implementasi teknis dari Playbook.

---

## 7. Studi Kasus Lab

Skenario lab mensimulasikan penanganan insiden phishing email secara end-to-end:

- **Threat Vector:** Email Attachment (lampiran email berbahaya)
- **Pengirim berbahaya:** Jeff Johnson (j.johnson@ppc.com)
- **Jumlah perangkat yang mengunduh lampiran:** 3 host
- **Jumlah perangkat yang mengeksekusi file:** 1 host (Host-HKNV)
- **Tindakan pada host yang hanya mengunduh:** Quarantine (Host-ATYU, Host-IOPE)
- **Tindakan pada host yang mengeksekusi:** Investigate → Isolate
- **Process Timeline Host-HKNV:** explorer.exe → chrome.exe → File Downloaded → Payslip.pdf dieksekusi → Powershell.exe + Malicious DNS Query

Alur penanganan ini mencerminkan fase SANS: Identification → Containment → Eradication (isolasi dan karantina) → Recovery → Lessons Learned (analisis timeline).

---

## 8. Glossary

| Istilah | Definisi |
|---|---|
| **Event** | Setiap aktivitas yang dilakukan oleh proses di sistem, dicatat dalam log |
| **Log** | Catatan rekaman dari setiap event yang terjadi di sistem |
| **Alert** | Notifikasi yang dipicu solusi keamanan ketika mendeteksi aktivitas mencurigakan |
| **False Positive** | Alert yang terlihat berbahaya namun sebenarnya bukan ancaman nyata |
| **True Positive** | Alert yang terkonfirmasi sebagai ancaman nyata |
| **Incident** | True positive alert yang dikonfirmasi dan memerlukan penanganan |
| **Severity Level** | Tingkat keparahan insiden: Low, Medium, High, Critical |
| **IR Plan** | Dokumen formal yang mengatur prosedur penanganan insiden |
| **SIEM** | Security Information and Event Management — platform agregasi dan korelasi log |
| **AV** | Antivirus — mendeteksi malware berbasis signature yang sudah dikenal |
| **EDR** | Endpoint Detection and Response — perlindungan tingkat lanjut per endpoint |
| **Playbook** | Panduan high-level langkah demi langkah untuk jenis insiden tertentu |
| **Runbook** | Eksekusi teknis detail dari langkah-langkah dalam playbook |
| **Threat Vector** | Media atau jalur yang digunakan malware untuk masuk ke sistem |
| **Quarantine** | Tindakan mengisolasi file berbahaya agar tidak menyebar |
| **Insider Attack** | Serangan yang dilakukan oleh orang dari dalam organisasi |
| **DoS** | Denial of Service — serangan yang membuat layanan tidak tersedia |
| **PICERL** | Akronim 6 fase SANS IR: Preparation, Identification, Containment, Eradication, Recovery, Lessons Learned |
| **CIA Triad** | Tiga pilar keamanan siber: Confidentiality, Integrity, Availability |

---

## 9. Tools & Platform Rujukan

- **SANS Institute** — organisasi penyedia kursus, sertifikasi, dan framework incident response; https://www.sans.org
- **NIST (National Institute of Standards and Technology)** — organisasi pemerintah AS yang mengembangkan standar dan panduan keamanan siber termasuk framework IR; https://www.nist.gov

---

## 10. Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar

- Event — aktivitas proses di sistem, dicatat sebagai log
- Alert — notifikasi dari security solution saat temukan anomali
- False Positive — alert tidak berbahaya, terlihat mencurigakan
- True Positive — alert yang benar-benar berbahaya = Incident
- Incident — true positive yang dikonfirmasi, perlu ditangani
- Severity — Low → Medium → High → Critical (Critical = prioritas tertinggi)

### Jenis Insiden

- Malware Infection — program jahat merusak sistem, masuk via file
- Security Breach — akses tidak sah ke data rahasia, disengaja
- Data Leak — data rahasia terekspos, bisa tidak sengaja (human error/misconfigurasi)
- Insider Attack — serangan dari dalam organisasi, akses lebih luas dari outsider
- DoS Attack — banjir request palsu → layanan tidak tersedia (menyerang availability)

### Framework SANS — PICERL

- P — Preparation: bangun tim, rencana, dan tools sebelum insiden
- I — Identification: deteksi perilaku abnormal via security tools
- C — Containment: batasi dampak, isolasi mesin/akun
- E — Eradication: hapus ancaman dari sistem hingga bersih
- R — Recovery: pulihkan dari backup, uji sebelum digunakan
- L — Lessons Learned: dokumentasikan celah, perbaiki proses

### Framework NIST — 4 Fase

- Preparation
- Detection and Analysis (= Identification SANS)
- Containment, Eradication, and Recovery (3 fase SANS digabung)
- Post-Incident Activity (= Lessons Learned SANS)

### IR Plan — Komponen Kunci

- Roles and Responsibilities
- IR Methodology
- Communication Plan (termasuk law enforcement)
- Escalation Path

### Tools

- SIEM — kumpulkan & korelasikan log, deteksi insiden
- AV — deteksi malware berbasis signature
- EDR — proteksi endpoint tingkat lanjut, bisa containment & eradication otomatis

### Playbook vs Runbook

- Playbook — panduan high-level per jenis insiden
- Runbook — eksekusi teknis detail dari playbook

### Threat Vector

- Jalur/media masuknya ancaman ke sistem
- Contoh: Email Attachment, Phishing Email, USB

### Lab — Urutan Tindakan IR

- Download lampiran → Malware detected
- Cek hosts terdampak di EDR
- Host hanya download → Quarantine
- Host execute file → Investigate → Isolate → Lihat process timeline
- Analisis timeline → Finish Case → Flag
