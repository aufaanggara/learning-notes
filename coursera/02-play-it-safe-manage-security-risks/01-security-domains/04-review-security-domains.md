# Resume Course 2 "Play It Safe: Manage Security Risks" - Module 1 (3 November 2026)

## 1. Gambaran Modul

Module 1 membahas fondasi manajemen risiko keamanan: **CISSP's eight security domains**, hubungan antara **threats, risks, and vulnerabilities**, **ransomware**, **three layers of the web**, dan **seven steps of the NIST Risk Management Framework (RMF)**.

Modul berikutnya akan memperkenalkan tools yang umum dipakai security analyst tingkat pemula untuk menganalisis data guna menemukan risks, threats, atau vulnerabilities, serta penggunaan playbook untuk incident response.

## 2. Konsep Inti Manajemen Risiko

### 2.1 Hubungan Asset, Threat, Vulnerability, dan Risk

Sebuah **asset** menjadi berisiko ketika ada **vulnerability** yang dapat dieksploitasi oleh **threat**. **Risk** adalah dampak potensial terhadap **confidentiality**, **integrity**, atau **availability** dari asset tersebut.

Threat dibagi menjadi dua sumber: **external threat** (dari luar organisasi) dan **internal threat** (dari karyawan saat ini atau mantan karyawan, vendor eksternal, atau mitra tepercaya).

### 2.2 Tingkat Risiko

Tingkat risiko (low, medium, high) ditentukan oleh threat yang mungkin terjadi dan asset yang terlibat.

- **Low risk**: tetap memerlukan pemantauan dan tindakan berkelanjutan jika terkompromi.
- **Medium risk**: jika terkompromi, dapat menyebabkan kerusakan tertentu pada reputasi organisasi.
- **High risk**: umumnya asset yang mengandung **SPII**, **PII**, atau **intellectual property**.

### 2.3 Konsekuensi Serangan

Dampak serangan dapat berupa kerugian finansial, kerusakan reputasi, dan **identity theft**. Contoh: **SPII** yang bocor melalui **dark web** setelah sebuah bisnis diserang menimbulkan risiko identity theft bagi pemilik datanya.

### 2.4 Social Engineering dan Ransomware

**Social engineering** menyerang faktor manusia, bukan celah teknis, sehingga pelaku memperoleh informasi pribadi, akses, atau barang berharga melalui manipulasi.

**Ransomware** adalah serangan yang mengenkripsi data organisasi lalu menuntut pembayaran untuk memulihkan akses. Dampak utamanya mengenai availability data dan kelangsungan operasional.

### 2.5 Security Posture, Mitigasi, dan Tanggung Jawab

**Security posture** mencerminkan seberapa baik organisasi mengelola pertahanan asset dan data pentingnya serta bereaksi terhadap perubahan.

**Risk mitigation** dilakukan melalui prosedur dan aturan yang siap pakai agar dampak risiko seperti breach dapat segera dikurangi. **Business continuity** menjaga produktivitas harian melalui rencana pemulihan bencana.

**Shared responsibility** berarti setiap individu di organisasi berperan aktif menurunkan risiko dan menjaga keamanan fisik maupun virtual.

## 3. CISSP's Eight Security Domains

Delapan domain ini adalah kerangka pembagian area kerja keamanan. Pemetaan tugas ke domain sering diuji, sehingga yang perlu dihapal adalah fokus tiap domain.

### 3.1 Daftar Domain dan Fokusnya

| Domain | Fokus |
|---|---|
| Security and risk management | Mitigate risk, follow legal regulations, maintain business continuity |
| Asset security | Penyimpanan, pemeliharaan, dan penghapusan data secara aman |
| Security architecture and engineering | Desain sistem aman; shared responsibility sebagai konsep inti |
| Communication and network security | Pengamanan jaringan fisik dan komunikasi |
| Identity and access management | Verifikasi pengguna dan pemantauan login attempts |
| Security assessment and testing | Security audit untuk mengidentifikasi vulnerabilities |
| Security operations | Investigasi insiden dan pencegahan lanjutan |
| Software development security | Security review di setiap fase software development lifecycle |

### 3.2 Pola Skenario yang Perlu Dikenali

- Memverifikasi pengguna dan memantau upaya login karyawan: **Identity and access management**.
- Melakukan security audit untuk mengidentifikasi vulnerabilities: **Security assessment and testing**.
- Mengintegrasikan keamanan penuh ke produk perangkat lunak lewat setiap fase lifecycle: **Software development security**.

## 4. Three Layers of the Web

Web dibagi menjadi tiga lapisan: **surface web** (dapat diakses dan diindeks mesin pencari umum), **deep web** (butuh autentikasi atau tidak terindeks), dan **dark web** (hanya dapat diakses dengan perangkat lunak khusus; sering menjadi tempat data curian beredar).

## 5. NIST Risk Management Framework (RMF)

RMF terdiri dari tujuh langkah berurutan untuk mengelola risiko keamanan dan privasi.

| Langkah | Nama | Fungsi |
|---|---|---|
| 1 | **Prepare** | Aktivitas untuk mengelola risiko sebelum breach terjadi |
| 2 | **Categorize** | Menyusun proses dan tugas manajemen risiko |
| 3 | **Select** | Memilih, menyesuaikan, dan mendokumentasikan kontrol |
| 4 | **Implement** | Menerapkan rencana keamanan dan privasi |
| 5 | **Assess** | Menentukan apakah kontrol sudah diterapkan dengan benar |
| 6 | **Authorize** | Menyetujui sistem beroperasi dan bertanggung jawab atas risikonya |
| 7 | **Monitor** | Memantau bagaimana sistem beroperasi |

Pegangan untuk ujian: **Authorize** adalah langkah yang melibatkan persetujuan resmi sistem untuk beroperasi sekaligus akuntabilitas atas risiko yang mungkin ada.

## 6. Glossary

- **Assess**: langkah ke-5 NIST RMF; menentukan apakah kontrol yang ditetapkan diterapkan dengan benar.
- **Authorize**: langkah ke-6 NIST RMF; bertanggung jawab atas risiko keamanan dan privasi di organisasi.
- **Business continuity**: kemampuan mempertahankan produktivitas harian melalui rencana pemulihan bencana.
- **Categorize**: langkah ke-2 NIST RMF; menyusun proses dan tugas manajemen risiko.
- **External threat**: segala sesuatu di luar organisasi yang berpotensi membahayakan asset.
- **Implement**: langkah ke-4 NIST RMF; menerapkan rencana keamanan dan privasi.
- **Internal threat**: karyawan saat ini atau mantan karyawan, vendor eksternal, atau mitra tepercaya yang menimbulkan risiko keamanan.
- **Monitor**: langkah ke-7 NIST RMF; memantau bagaimana sistem beroperasi.
- **Prepare**: langkah ke-1 NIST RMF; aktivitas pengelolaan risiko sebelum breach.
- **Ransomware**: serangan yang mengenkripsi data organisasi dan menuntut pembayaran untuk memulihkan akses.
- **Risk**: segala sesuatu yang dapat memengaruhi confidentiality, integrity, atau availability suatu asset.
- **Risk mitigation**: prosedur dan aturan yang siap agar dampak risiko seperti breach cepat berkurang.
- **Security posture**: kemampuan organisasi mengelola pertahanan asset dan data penting serta bereaksi terhadap perubahan.
- **Select**: langkah ke-3 NIST RMF; memilih, menyesuaikan, dan mendokumentasikan kontrol.
- **Shared responsibility**: semua individu berperan aktif menurunkan risiko dan menjaga keamanan fisik dan virtual.
- **Social engineering**: teknik manipulasi yang mengeksploitasi human error untuk memperoleh informasi pribadi, akses, atau barang berharga.
- **Vulnerability**: kelemahan yang dapat dieksploitasi oleh threat.

## 7. Tools & Platform Rujukan

Materi yang dikirim untuk modul ini tidak menyebutkan tools atau platform eksternal tertentu. Tools yang akan dipakai security analyst tingkat pemula diperkenalkan pada video-video berikutnya.

## 8. Catatan Ringkas untuk Ditulis Tangan

### 8.1 Konsep Dasar

- Asset — sesuatu yang dilindungi
- Threat — penyebab potensial bahaya
- Vulnerability — kelemahan yang bisa dieksploitasi threat
- Risk — dampak ke confidentiality, integrity, availability
- External threat — dari luar organisasi
- Internal threat — karyawan/mantan, vendor, mitra tepercaya
- Social engineering — manipulasi human error
- Ransomware — enkripsi data + minta bayaran
- Security posture — kemampuan jaga pertahanan + bereaksi pada perubahan
- Risk mitigation — prosedur/aturan untuk kurangi dampak risiko
- Business continuity — jaga produktivitas via disaster recovery
- Shared responsibility — semua orang berperan turunkan risiko

### 8.2 Tingkat Risiko dan Dampak

- Low — tetap butuh monitoring/tindakan
- Medium — bisa rusak reputasi
- High — SPII, PII, intellectual property
- Penentu level — threat + asset
- SPII bocor di dark web — identity theft

### 8.3 Delapan Domain CISSP

- Security and risk management — mitigate risk, regulasi, business continuity
- Asset security — simpan, jaga, hapus data aman
- Security architecture and engineering — desain aman, shared responsibility
- Communication and network security — amankan jaringan
- Identity and access management — verifikasi user, pantau login
- Security assessment and testing — audit cari vulnerabilities
- Security operations — investigasi dan pencegahan
- Software development security — security review tiap fase lifecycle

### 8.4 Three Layers of the Web

- Surface web — publik, terindeks
- Deep web — butuh autentikasi/tidak terindeks
- Dark web — butuh software khusus

### 8.5 NIST RMF (7 Langkah)

- 1 Prepare — kelola risiko sebelum breach
- 2 Categorize — susun proses dan tugas
- 3 Select — pilih, sesuaikan, dokumentasikan kontrol
- 4 Implement — terapkan rencana
- 5 Assess — cek kontrol benar atau tidak
- 6 Authorize — setujui sistem + tanggung jawab risiko
- 7 Monitor — pantau operasi sistem

### 8.6 Preview Modul Berikutnya

- Tools security analyst pemula — analisis data cari threat/vulnerability
- Playbook — respons insiden
