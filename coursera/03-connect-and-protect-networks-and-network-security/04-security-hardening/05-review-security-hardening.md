# Resume Review Modul 4: Security Hardening dan Pertahanan Terhadap Kerentanan (Course 3, Coursera Cybersecurity) - 25 Oktober 2026

Cakupan: Coach dialogue (Reinforce system hardening and vulnerability defense), video wrap-up security hardening, glossary Modul 4, Module 4 challenge, dan Portfolio activity (NIST Cybersecurity Framework) beserta exemplar-nya.

Catatan: bagian ini murni konsep dan latihan, tidak ada command atau switch terminal yang dibahas.

## 1. Konsep Inti Security Hardening

### 1.1 Definisi dan tujuan

**Security hardening** adalah proses memperkuat sebuah sistem untuk mengurangi kerentanan (vulnerability) dan **attack surface**-nya. Hasilnya, kemungkinan serangan berhasil menurun.

Hubungannya dengan attack surface: hardening **mengurangi** (diminishes) attack surface. Hardening tidak menghilangkannya secara permanen dan tidak menambah atau memperluasnya.

Security analyst bekerja dengan tiga lingkungan: **operating system**, **on-premise network**, dan **cloud network**. Hardening diterapkan pada ketiganya.

### 1.2 Hardening fisik

Hardening tidak hanya digital. Mempekerjakan satpam adalah contoh praktik hardening yang bersifat **physical**.

### 1.3 Pembagian area hardening

- **OS hardening**: memperkuat sistem operasi.
- **Network hardening**: memperkuat jaringan, termasuk jaringan on-premise.
- **Cloud network hardening**: memperkuat jaringan cloud, dengan tanggung jawab bersama.

## 2. OS Hardening

### 2.1 Praktik utama

- **Patch update**: pembaruan software dan OS yang menangani kerentanan keamanan pada program atau produk.
- **Baseline configuration (baseline image)**: kumpulan spesifikasi terdokumentasi dalam sebuah sistem, dipakai sebagai dasar untuk build, rilis, dan pembaruan berikutnya.
- **Hardware and software disposal**: pembuangan perangkat keras dan perangkat lunak secara aman.

Memeriksa baseline configuration berarti analyst membandingkan konfigurasi OS saat ini dengan dokumentasi yang sudah ada tentang OS tersebut.

### 2.2 Metode OS hardening (soal challenge)

Tiga metode yang benar:

- Menghapus software yang tidak terpakai untuk membatasi kerentanan yang tidak perlu.
- Menjaga daftar pengguna yang diotorisasi tetap up-to-date.
- Mengonfigurasi pengaturan perangkat agar sesuai standar enkripsi yang aman.

Implementasi **IDS** bukan metode OS hardening, melainkan praktik network hardening.

**Multi-factor authentication (MFA)** dikategorikan sebagai praktik OS hardening.

### 2.3 Kerentanan OS dan pertahanannya (Coach dialogue)

Coach dialogue memetakan lima jenis kerentanan OS: kelemahan software yang bisa dieksploitasi, izin pengguna yang berlebihan, kredensial lemah atau dicuri, program atau layanan yang tidak diperlukan, dan risiko kehilangan perangkat. Tiga yang dibahas mendalam:

**Exploitable software flaws**: pertahanannya adalah **patch management**. Developer yang menemukan kerentanan merilis patch yang menutup celah masuk bagi penyerang.

**Excessive user permissions**: pertahanannya adalah **least privilege**. Jika akun diambil alih, penyerang hanya memiliki akses sebatas akun korban sehingga kerusakan diminimalkan dan tidak bisa menyebar ke akun lain.

**Weak or stolen credentials**: pertahanannya adalah **hashing** dan **MFA**. Hashing membuat penyerang butuh waktu lama untuk membalikkan kredensial tersimpan menjadi teks yang terbaca. MFA menambah faktor verifikasi kedua (misalnya kode dari aplikasi atau physical key) yang tidak dimiliki penyerang meskipun password sudah dicuri.

## 3. Network Hardening

### 3.1 Praktik utama

- **Network log analysis**: memeriksa log jaringan untuk mengidentifikasi events of interest. Biasanya dibantu **SIEM**.
- **Firewall rule maintenance**: merawat dan memperbarui aturan firewall agar sesuai kebutuhan keamanan.
- **Port filtering**: memblokir atau mengizinkan nomor port tertentu untuk membatasi komunikasi yang tidak diinginkan. Port yang tidak diperlukan ditutup sehingga attack surface mengecil.
- **Network segmentation**: membagi jaringan menjadi subnet terisolasi, misalnya per departemen. Penyerang yang sudah masuk lebih sulit bergerak lateral ke area lain.
- **IDPS (Intrusion Detection and Prevention System)**: IDS mendeteksi dan memonitor percobaan masuk, bagian prevention membuat rule untuk memblokir adversary.

### 3.2 Hubungan dengan SIEM

**SIEM** adalah aplikasi yang mengumpulkan dan menganalisis data log untuk memantau aktivitas penting bagi organisasi. Fungsi ini berbeda dari port filtering. Menurut soal challenge, membantu analyst memeriksa, menganalisis, dan bereaksi terhadap security event berdasarkan prioritas adalah ciri SIEM, bukan port filtering.

## 4. Cloud Network Hardening

### 4.1 Cloud network

**Cloud network** adalah kumpulan server atau komputer yang menyimpan resource dan data di data center jarak jauh yang bisa diakses lewat internet.

### 4.2 Tanggung jawab bersama

Keamanan cloud adalah tanggung jawab bersama antara organisasi dan **cloud service provider**. Provider mengamankan infrastruktur dasar, organisasi bertanggung jawab atas konfigurasi dan penggunaan layanan yang dijalankannya.

### 4.3 Security groups

**Security groups** berfungsi seperti firewall virtual untuk resource cloud. Mereka mengontrol lalu lintas inbound dan outbound pada level instance berdasarkan aturan yang ditentukan. Hanya trafik yang diizinkan aturan yang boleh lewat.

## 5. Investigasi, Pencegahan, dan Respons

### 5.1 Audit dan penetration testing

**Penetration testing (pen test)** adalah serangan simulasi yang membantu mengidentifikasi kerentanan pada sistem, jaringan, website, aplikasi, dan proses. Dilakukan dengan persetujuan, sehingga penguji bertindak seperti adversary sungguhan tanpa niat jahat.

Contoh soal: tim keamanan internal yang disetujui mensimulasikan serangan untuk menemukan kerentanan pada proses bisnis adalah penetration testing. Packet sniffing, DDoS, dan Ping of Death adalah serangan, bukan aktivitas pengujian yang disetujui.

### 5.2 Skenario penanganan aktivitas mencurigakan

Alur dari Coach dialogue ketika ditemukan pola lalu lintas tidak biasa:

1. **Deteksi dan investigasi**: IDPS memberi alert dan monitoring.
2. **Containment**: blokir IP penyerang di level jaringan untuk menghentikan ancaman yang sedang berlangsung.
3. **Eradication**: ganti password atau kredensial agar penyerang tidak bisa kembali memakai akses yang sama.

## 6. Portfolio Activity: NIST Cybersecurity Framework

### 6.1 NIST CSF

**NIST Cybersecurity Framework (CSF)** adalah framework sukarela berisi standar, panduan, dan best practice untuk mengelola risiko keamanan siber. Bersifat scalable dan bisa dipakai di berbagai konteks. Penggunaannya menunjukkan pendekatan proaktif, meningkatkan komunikasi dengan stakeholder, dan memperbaiki praktik keamanan.

### 6.2 Lima fungsi CSF

- **Identify**: mengidentifikasi risiko lewat audit rutin terhadap jaringan internal, sistem, perangkat, dan access privileges.
- **Protect**: melindungi aset internal lewat kebijakan, prosedur, pelatihan, dan tools.
- **Detect**: mendeteksi potensi insiden dan meningkatkan kecepatan serta efisiensi monitoring.
- **Respond**: mengontain, menetralkan, dan menganalisis insiden, lalu memperbaiki proses keamanan.
- **Recover**: mengembalikan sistem ke operasi normal dan memulihkan data atau aset yang terdampak.

### 6.3 Skenario

Perusahaan multimedia (web design, desain grafis, social media marketing untuk usaha kecil) mengalami serangan **DoS** yang mengganggu jaringan internal selama dua jam. Penyebabnya banjir paket **ICMP** (ICMP ping flood) melalui **firewall yang tidak dikonfigurasi**.

Respons awal tim incident management: memblokir paket ICMP masuk, mematikan layanan jaringan non-kritis, dan memulihkan layanan kritis.

Perbaikan oleh tim network security:

- Aturan firewall baru untuk membatasi laju (rate) paket ICMP masuk.
- Verifikasi source IP address pada firewall untuk memeriksa spoofed IP address.
- Software network monitoring untuk mendeteksi pola lalu lintas abnormal.
- Sistem IDS/IPS untuk menyaring sebagian lalu lintas ICMP berdasarkan karakteristik mencurigakan.

### 6.4 Langkah pengerjaan incident report analysis

- **Ringkasan**: security event, penyebab, dampak, respons; boleh menyertakan sistem target, sumber serangan, dan estimasi dampak.
- **Identify**: jenis serangan dan sistem yang terdampak.
- **Protect**: rencana aksi segera, yaitu sistem atau prosedur apa yang perlu diperbarui atau diubah.
- **Detect**: cara memantau lalu lintas jaringan dan aplikasi, melacak pengguna authorized vs unauthorized, dan mendeteksi aktivitas tidak biasa pada akun. Contoh pemantauan: ICMP eksternal dari IP non-trusted yang mencoba melewati firewall.
- **Respond**: cara containment, prosedur neutralization, data untuk analisis, dan perbaikan proses recovery.
- **Recover**: informasi yang dibutuhkan agar bisa pulih segera dan proses pemulihan organisasi.

### 6.5 Kriteria penilaian

Incident report harus: meringkas security event, mengidentifikasi jenis serangan dan sistem terdampak, menawarkan rencana perlindungan, menjelaskan metode deteksi, menyertakan rencana respons, dan menguraikan rencana recovery. Nilai lulus minimal 80% (4,8 dari 6 poin), dan target sebelum lanjut adalah minimal 5 poin.

### 6.6 Exemplar

Exemplar adalah satu contoh incident report analysis yang lengkap, bukan jawaban tunggal. Ia menetapkan: jenis serangan, cakupan, dan dampak; potensi kerentanan dan langkah perlindungan; tools deteksi; cara merespons insiden di masa depan; dan rencana recovery.

## 7. Glossary

- **Baseline configuration (baseline image)**: kumpulan spesifikasi terdokumentasi dalam sistem sebagai dasar build, rilis, dan pembaruan.
- **Hardware**: komponen fisik komputer.
- **Multi-factor authentication (MFA)**: verifikasi identitas dengan dua cara atau lebih untuk mengakses sistem atau jaringan.
- **Network log analysis**: memeriksa log jaringan untuk mengidentifikasi events of interest.
- **Operating system (OS)**: antarmuka antara perangkat keras komputer dan pengguna.
- **Patch update**: pembaruan software dan OS yang menangani kerentanan keamanan.
- **Penetration testing (pen test)**: serangan simulasi untuk mengidentifikasi kerentanan sistem, jaringan, website, aplikasi, dan proses.
- **Security hardening**: memperkuat sistem untuk mengurangi kerentanan dan attack surface.
- **Security information and event management (SIEM)**: aplikasi yang mengumpulkan dan menganalisis data log untuk memantau aktivitas penting organisasi.
- **World-writable file**: file yang bisa diubah oleh siapa saja.
- **Attack surface**: seluruh titik yang bisa dimanfaatkan penyerang; dikurangi oleh hardening.
- **Cloud network**: kumpulan server atau komputer di data center jarak jauh yang diakses lewat internet.
- **Least privilege**: akun hanya diberi izin sebatas yang dibutuhkan.
- **Network segmentation**: pembagian jaringan menjadi subnet terisolasi.
- **Port filtering**: pemblokiran atau pengizinan port tertentu.
- **Security groups**: firewall virtual pada level instance di cloud.
- **IDPS**: sistem yang mendeteksi sekaligus mencegah intrusi.
- **Containment dan eradication**: menghentikan ancaman yang berlangsung, lalu menghapus akses penyerang.

## 8. Tools & Platform Rujukan

- **NIST Cybersecurity Framework (NIST CSF)**: framework acuan untuk mengelola risiko siber (Identify, Protect, Detect, Respond, Recover).
- **Incident report analysis (template)**: worksheet untuk menyusun analisis insiden berdasarkan CSF; tersedia lewat Google Docs (Use Template) atau file DOCX.
- **Applying the NIST CSF**: materi pendukung penerapan CSF pada aktivitas.
- **Example of an incident report analysis**: contoh incident report analysis yang sudah selesai.
- **Incident report analysis exemplar**: exemplar portfolio activity untuk membandingkan hasil kerja.

URL eksplisit tidak terlihat di materi, sehingga hanya nama yang dicantumkan.

## 9. Catatan Ringkas untuk Ditulis Tangan

### 9.1 Dasar hardening

Security hardening — perkuat sistem, kurangi vulnerability + attack surface
Hardening vs attack surface — mengurangi (bukan eliminasi permanen)
Satpam — contoh hardening physical
3 lingkungan — OS, on-premise network, cloud network

### 9.2 OS hardening

Patch update — update OS/software tutup vulnerability
Baseline configuration — spesifikasi terdokumentasi, dasar build/rilis/update
Cek baseline — bandingkan konfigurasi sekarang vs dokumentasi OS
Disposal — buang hardware/software dengan aman
Hapus software tak terpakai — kurangi vulnerability
Daftar user authorized — selalu up-to-date
Enkripsi — setting perangkat sesuai standar aman
MFA — verifikasi 2+ cara, dikategorikan OS hardening
IDS — bukan OS hardening, tapi network hardening

### 9.3 Kerentanan OS dan pertahanan

Exploitable software flaws — patch management
Excessive user permissions — least privilege (dampak dibatasi ke akun korban)
Weak/stolen credentials — hashing (lama dibalik) + MFA (faktor kedua)
Lainnya — program/layanan tak perlu, kehilangan perangkat

### 9.4 Network hardening

Network log analysis — periksa log, cari events of interest
Firewall rule maintenance — rawat aturan firewall
Port filtering — blokir/izinkan port, tutup yang tak perlu
Network segmentation — subnet terisolasi per departemen, cegah lateral movement
IDPS — IDS deteksi + monitor, prevention blokir via rule
SIEM — kumpul + analisis log, prioritas security event

### 9.5 Cloud hardening

Cloud network — server/komputer di data center jauh via internet
Shared responsibility — provider: infrastruktur; organisasi: konfigurasi
Security groups — firewall virtual, inbound/outbound, level instance

### 9.6 Investigasi dan respons

Pen test — serangan simulasi dengan izin, cari vulnerability
Alur insiden — IDPS alert, blokir IP (containment), ganti password (eradication)
World-writable file — file bisa diubah siapa saja

### 9.7 NIST CSF

NIST CSF — framework sukarela, scalable
Identify — audit jaringan/sistem/perangkat/access privileges
Protect — kebijakan, prosedur, pelatihan, tools
Detect — monitoring, deteksi cepat dan efisien
Respond — contain, neutralize, analyze, perbaiki proses
Recover — pulihkan sistem, data, aset

### 9.8 Skenario ICMP DoS

Serangan — DoS via ICMP ping flood
Penyebab — firewall tidak dikonfigurasi
Dampak — jaringan internal terganggu 2 jam
Respons awal — blokir ICMP, matikan layanan non-kritis, pulihkan layanan kritis
Perbaikan 1 — rule firewall batasi rate ICMP
Perbaikan 2 — verifikasi source IP (spoofing)
Perbaikan 3 — network monitoring (pola abnormal)
Perbaikan 4 — IDS/IPS filter ICMP mencurigakan

### 9.9 Penilaian portfolio

Isi report — ringkasan, Identify, Protect, Detect, Respond, Recover
Lulus — minimal 80% (4,8 dari 6), target minimal 5 poin
Exemplar — satu contoh, bukan jawaban tunggal
