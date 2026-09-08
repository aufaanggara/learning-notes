# Resume Materi — Introduction to Cybersecurity: CIA Triad
**Tanggal:** 01 September 2026
**Room:** Introduction to Cybersecurity — CIA Triad (TryHackMe)

---

## 1. Konteks: Mengapa Keamanan Data Dibutuhkan

Di masa lalu, informasi disimpan secara fisik (kertas). Sekarang informasi tersimpan sebagai **data digital** yang dikomunikasikan melalui jaringan — dan tanpa keamanan yang tepat, data ini rentan terhadap tiga ancaman utama:

- **Data Seen** — dilihat oleh pihak yang tidak berwenang
- **Data Changed** — dimodifikasi tanpa izin
- **Data Not Delivered** — tidak sampai ke tujuan atau tidak bisa diakses

Melindungi data digital dari ketiga ancaman ini bukan lagi pilihan — ini sudah menjadi **kebutuhan inti** bagi pemerintah, organisasi, maupun individu.

---

## 2. CIA Triad — Tiga Pilar Utama Cybersecurity

**CIA Triad** adalah kerangka fundamental yang menjelaskan *apa yang sebenarnya dilindungi* dalam cybersecurity. Hampir semua konsep, teknik, serangan, dan pertahanan dalam cybersecurity berputar di sekitar tiga pilar ini.

CIA bukan sekadar kumpulan definisi — ini adalah **security mindset**: cara berpikir profesional keamanan saat menganalisis dan merespons setiap insiden.

| Pilar | Inti Perlindungan |
|---|---|
| **Confidentiality** | Data hanya bisa diakses oleh pihak berwenang |
| **Integrity** | Data tidak boleh dimodifikasi tanpa izin |
| **Availability** | Data dan layanan harus tersedia saat dibutuhkan |

---

## 3. Confidentiality (Kerahasiaan)

**Confidentiality** memastikan bahwa data sensitif hanya dapat diakses oleh individu yang memiliki otorisasi. Jika pilar ini dilanggar, konsekuensinya bisa berupa kerugian finansial, pelanggaran privasi, atau tuntutan hukum.

### Contoh Pelanggaran

**Dunia nyata:** Seseorang mendengarkan diam-diam percakapan privat dan menggunakan informasi tersebut untuk memanipulasi. Informasi yang seharusnya rahasia diakses oleh pihak yang tidak berhak.

**Dunia digital:** Login ke akun media sosial menggunakan WiFi publik di kedai kopi. Kredensial dicegat oleh pihak di dalam jaringan — akun di-logout paksa dan tidak bisa diakses kembali.

### Mekanisme Perlindungan

- **Enkripsi** — mengacak data sehingga tidak bisa dibaca pihak yang mencegat
- **Access Control** — membatasi siapa yang boleh mengakses data tertentu

### Contoh Kasus

| Situasi | Confidentiality Tercapai? |
|---|---|
| Kredensial Gmail ditulis di sticky note di meja kantor | Tidak |
| Dokumen internal hanya tersedia untuk karyawan yang membutuhkan | Ya |
| Dokumen pribadi tersedia bebas di internet | Tidak |

---

## 4. Integrity (Integritas)

**Integrity** memastikan bahwa data tidak dimodifikasi oleh pihak yang tidak memiliki otorisasi. Tanpa integrity, data tidak bisa dipercaya — dan perubahan data yang tidak sah bisa berujung pada konsekuensi yang sangat berbahaya.

### Contoh Pelanggaran

**Dunia nyata:** Guru memberi nilai bagus pada ujian, namun seseorang mengubah nilai tersebut sebelum diserahkan ke otoritas ujian. Nilai akhir tidak lagi mencerminkan penilaian asli.

**Dunia digital:** Transfer bank dicegat di tengah jalan — nomor rekening tujuan diubah sehingga uang masuk ke rekening yang salah. Tidak ada yang tahu sampai kerusakan sudah terjadi.

### Mekanisme Perlindungan

Berbagai teknik digunakan untuk memastikan integrity di dunia digital, seperti hashing, digital signature, dan verifikasi transaksi — akan dipelajari lebih lanjut dalam perjalanan cybersecurity.

### Contoh Kasus

| Situasi | Integrity Tercapai? |
|---|---|
| Data diubah melalui persetujuan pihak berwenang | Ya |
| Catatan kehadiran siswa diubah setelah dikunci oleh guru | Tidak |
| Harga pesanan dimodifikasi sebelum checkout | Tidak |

---

## 5. Availability (Ketersediaan)

**Availability** memastikan bahwa data dan layanan selalu tersedia untuk pengguna yang berwenang pada saat mereka membutuhkannya. Meskipun merupakan pilar ketiga, availability sama pentingnya dengan dua pilar lainnya.

Bisnis modern sangat bergantung pada layanan digital. Bahkan **downtime yang singkat** — tanpa ada data yang bocor atau diubah sekalipun — bisa menyebabkan kerugian finansial yang besar.

### Contoh Pelanggaran

**Dunia nyata:** Uang tersimpan aman di bank, tapi bank tutup karena mati listrik tepat saat nasabah butuh uangnya. Data (uang) aman, tapi tidak bisa diakses — sama saja tidak berguna.

**Dunia digital:** Penyerang mengirimkan ribuan request secara serentak ke sebuah website hingga server tidak sanggup menangani beban tersebut dan **website down**. Ini dikenal sebagai serangan **DDoS (Distributed Denial of Service)**. Tidak ada data yang bocor atau diubah, namun bisnis tetap menderita kerugian besar.

### Mekanisme Perlindungan

- **Traffic load management** — mengelola distribusi request agar server tidak kewalahan
- **Rate limiting** — memblokir request ketika melampaui ambang batas tertentu
- **Redundansi** — generator cadangan, server backup, failover system

### Contoh Kasus

| Situasi | Availability Tercapai? |
|---|---|
| Layanan kritis terganggu karena instalasi software | Tidak |
| Website perusahaan offline selama jam kerja | Tidak |
| Semua sistem dapat diakses karyawan selama jam kerja | Ya |

---

## 6. Security Mindset — Cara Berpikir Profesional

**CIA Triad adalah cara berpikir, bukan sekadar hafalan.** Setiap kali sebuah insiden keamanan terjadi, seorang profesional akan menganalisisnya dengan tiga pertanyaan ini:

1. Apakah data sensitif terekspos ke pihak yang tidak berwenang?
   → Jika ya, **Confidentiality** terdampak

2. Apakah data dimodifikasi tanpa izin?
   → Jika ya, **Integrity** terdampak

3. Apakah sistem atau layanan tidak tersedia ketika dibutuhkan?
   → Jika ya, **Availability** terdampak

Memahami CIA Triad secara mendalam memungkinkan seseorang untuk **menilai dampak suatu insiden** dan **menentukan respons yang tepat** — inilah inti dari pekerjaan seorang profesional keamanan.

---

## 7. Glossary — Istilah Penting

**CIA Triad** — Kerangka tiga pilar utama cybersecurity: Confidentiality, Integrity, Availability. Fondasi dari hampir seluruh konsep keamanan siber.

**Confidentiality** — Memastikan informasi digital tidak dapat diakses oleh individu yang tidak berwenang.

**Integrity** — Memastikan informasi digital tidak dimodifikasi tanpa izin.

**Availability** — Memastikan informasi digital dan layanan tidak menjadi tidak tersedia ketika dibutuhkan.

**Enkripsi** — Teknik mengacak data sehingga hanya pihak yang memiliki kunci yang bisa membacanya. Salah satu mekanisme utama untuk menjaga confidentiality.

**Access Control** — Mekanisme pembatasan akses — menentukan siapa yang boleh mengakses data atau sistem tertentu.

**DDoS (Distributed Denial of Service)** — Serangan yang membanjiri server dengan request dalam jumlah besar secara serentak hingga layanan tidak bisa diakses. Serangan terhadap availability.

**Security Mindset** — Pola pikir berbasis CIA Triad yang digunakan profesional keamanan untuk menganalisis insiden dan menentukan respons yang tepat.

**Authorized/Otorisasi** — Memiliki izin resmi untuk mengakses atau melakukan tindakan tertentu terhadap suatu sistem atau data.

**Downtime** — Periode di mana sistem atau layanan tidak dapat diakses atau tidak berfungsi. Pelanggaran langsung terhadap availability.

**Data Breach** — Insiden di mana data sensitif diakses, dicuri, atau terekspos tanpa otorisasi. Umumnya pelanggaran terhadap confidentiality.

---

## 8. Tools & Platform Rujukan

Tidak ada tools atau platform eksternal spesifik yang disebutkan di room ini. Room ini bersifat konseptual dan berakhir dengan latihan hands-on drag-and-drop langsung di platform TryHackMe.

---

## 9. Catatan Ringkas untuk Ditulis Tangan

### CIA Triad — Inti

- CIA Triad — 3 pilar utama cybersecurity: Confidentiality, Integrity, Availability
- Bukan hanya definisi — ini adalah **security mindset** profesional
- Semua konsep, serangan, dan pertahanan dalam cybersecurity berputar di 3 pilar ini

### Confidentiality

- Definisi — data hanya bisa diakses pihak berwenang
- Dilanggar jika — data dilihat/dicuri pihak tidak berwenang
- Contoh ancaman — WiFi publik → kredensial dicegat (intersepsi)
- Mekanisme — enkripsi + access control

### Integrity

- Definisi — data tidak boleh dimodifikasi tanpa izin
- Dilanggar jika — data diubah oleh pihak tidak berwenang
- Contoh ancaman — transfer bank dicegat → rekening tujuan diubah
- Mekanisme — hashing, digital signature, verifikasi

### Availability

- Definisi — data/layanan harus tersedia saat dibutuhkan
- Dilanggar jika — sistem/layanan tidak bisa diakses
- Contoh ancaman — DDoS → server dibanjiri request → website down
- Mekanisme — load management, rate limiting, redundansi

### Security Mindset — 3 Pertanyaan Saat Insiden

- Apakah data terekspos ke pihak tidak berwenang? → Confidentiality
- Apakah data dimodifikasi tanpa izin? → Integrity
- Apakah sistem tidak tersedia saat dibutuhkan? → Availability

### Istilah Kunci

- Enkripsi — acak data agar tidak bisa dibaca pihak yang mencegat
- Access Control — batasi siapa yang boleh akses data
- DDoS — banjiri server dengan request → layanan down
- Downtime — periode sistem tidak bisa diakses (langgar Availability)
- Authorized — punya izin resmi untuk akses/tindakan tertentu
- Data Breach — data bocor/diakses tanpa izin (langgar Confidentiality)
