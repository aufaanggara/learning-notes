# Resume: Pengantar Security Hardening (21 Oktober 2026)

Materi: dua video pengantar Security Hardening, Coursera Cybersecurity (bagian Network Security).

## 1. Konteks dan Arah Pembelajaran

### 1.1 Posisi Topik dalam Kursus

Security hardening adalah tahap yang datang setelah beberapa topik sebelumnya, dengan urutan sebagai berikut:

- **Network operations**
- **Tools** dan **protocols** yang membuat sistem jaringan berfungsi
- **Vulnerabilities** jaringan yang membuka celah bagi **security intrusions**
- **Security hardening** (topik saat ini)

### 1.2 Tiga Topik Lanjutan

Setelah pengantar ini, pembahasan dilanjutkan secara berurutan ke:

- **OS hardening**
- **Network hardening**
- **Cloud hardening**

### 1.3 Relevansi bagi Security Analyst

Hardening menjadi bagian besar dari tugas harian **security analyst**. Dua contoh tugas yang disebut adalah **patch updates** dan **backups**.

## 2. Konsep Inti

### 2.1 Definisi Security Hardening

**Security hardening** adalah proses memperkuat sebuah sistem untuk mengurangi kerentanan (**vulnerability**) dan **attack surface**-nya. Pendekatannya **proaktif**: organisasi memperkuat sistem sebelum terjadi serangan, bukan setelahnya.

### 2.2 Attack Surface

**Attack surface** adalah seluruh kerentanan potensial pada sistem yang dapat dieksploitasi oleh **threat actor**.

> *Jaringan dianalogikan sebagai rumah, dan attack surface adalah semua pintu dan jendela yang bisa dipakai perampok untuk masuk.*

Hardening sama seperti memasang kunci pada semua pintu dan jendela tersebut. Tujuannya meminimalkan attack surface agar jaringan setara aman mungkin.

### 2.3 Cakupan Hardening

Hardening dapat diterapkan pada perangkat atau sistem apa pun yang bisa disusupi:

- **Hardware**
- **Operating systems**
- **Applications**
- **Computer networks**
- **Databases**
- **Cloud infrastructure**

**Physical security** juga termasuk hardening, misalnya mengamankan ruang fisik dengan **security cameras** dan **security guards**.

### 2.4 Pemeliharaan Rutin

Security analyst melakukan prosedur pemeliharaan rutin agar perangkat jaringan dan sistem tetap berfungsi secara aman dan optimal.

## 3. Prosedur Hardening yang Umum

### 3.1 Software Updates (Patches)

**Patches** adalah pembaruan perangkat lunak yang memperbaiki kerentanan keamanan pada jaringan.

### 3.2 Perubahan Konfigurasi Keamanan

Perubahan konfigurasi aplikasi pada perangkat dilakukan untuk meningkatkan keamanan. Contohnya:

- Mewajibkan **password** yang lebih panjang atau lebih sering diganti, sehingga kredensial login lebih sulit diperoleh pelaku jahat.
- Memperbarui standar **encryption** untuk data yang tersimpan di **database** (disebut sebagai contoh **configuration check**), sehingga database lebih sulit diakses.

### 3.3 Mengurangi Attack Surface

Langkah-langkah yang mengecilkan attack surface:

- Menghapus atau menonaktifkan aplikasi dan **services** yang tidak digunakan
- Menonaktifkan **ports** yang tidak digunakan
- Mengurangi **access permissions** di seluruh perangkat dan jaringan

Meminimalkan jumlah aplikasi, perangkat, port, dan access permissions membuat **monitoring** lebih efisien dan memperkecil attack surface. Ini disebut sebagai salah satu cara terbaik untuk mengamankan organisasi.

## 4. Penetration Testing

**Penetration test (pen test)** adalah serangan simulasi untuk mengidentifikasi kerentanan pada sistem, jaringan, website, aplikasi, dan proses.

Alurnya:

- **Penetration testers** mendokumentasikan temuan dalam sebuah laporan.
- Dari bagian yang gagal dalam pengujian, tim keamanan menentukan jenis kerentanan yang perlu diperbaiki.
- Organisasi meninjau kerentanan tersebut dan menyusun rencana perbaikan.

Pen test dilakukan secara rutin sebagai bagian dari strategi hardening.

## 5. Peran dalam Network Security

Security hardening adalah bagian fundamental dari **network security**. Tujuannya memperkuat jaringan agar jumlah serangan yang berhasil berkurang.

## 6. Glossary

- **Security hardening**: proses memperkuat sistem untuk mengurangi vulnerability dan attack surface.
- **Attack surface**: seluruh kerentanan potensial yang dapat dieksploitasi threat actor.
- **Threat actor**: pihak yang mengeksploitasi kerentanan sistem.
- **Vulnerability**: kelemahan sistem yang dapat dieksploitasi.
- **Patch**: pembaruan perangkat lunak untuk memperbaiki kerentanan.
- **Configuration change**: perubahan pengaturan aplikasi/perangkat untuk meningkatkan keamanan.
- **Penetration test (pen test)**: serangan simulasi untuk menemukan kerentanan.
- **Penetration tester**: pihak yang melakukan pen test dan menulis laporan temuan.
- **Physical security**: pengamanan ruang fisik, misalnya dengan kamera dan penjaga.
- **Access permissions**: hak akses pengguna atau perangkat yang sebaiknya dikurangi seminimal mungkin.

## 7. Tools & Platform Rujukan

Tidak ada tools atau platform eksternal yang disebutkan dalam materi ini.

## 8. Catatan Ringkas untuk Ditulis Tangan

### 8.1 Alur Kursus

- Network operations, tools & protocols, vulnerabilities, lalu security hardening
- Berikutnya: OS hardening, network hardening, cloud hardening

### 8.2 Konsep Inti

- Security hardening: perkuat sistem, kurangi vulnerability dan attack surface (proaktif)
- Attack surface: semua kerentanan potensial yang bisa dieksploitasi threat actor
- Analogi: jaringan = rumah, attack surface = pintu dan jendela, hardening = pasang kunci

### 8.3 Cakupan

- Hardware, OS, applications, networks, databases, cloud infrastructure
- Physical security: security cameras, security guards

### 8.4 Prosedur

- Patches: update software untuk menambal kerentanan
- Konfigurasi: password lebih panjang / lebih sering diganti
- Configuration check: update standar encryption pada database
- Hapus atau nonaktifkan aplikasi dan services yang tidak dipakai
- Nonaktifkan ports yang tidak dipakai
- Kurangi access permissions
- Manfaat: monitoring lebih efisien, attack surface lebih kecil

### 8.5 Pen Test

- Pen test: serangan simulasi untuk cari kerentanan (sistem, jaringan, website, aplikasi, proses)
- Hasil: laporan oleh penetration tester
- Tindak lanjut: tim review, susun rencana perbaikan
- Dilakukan rutin

### 8.6 Tugas Analyst

- Patch updates dan backups
- Pemeliharaan rutin agar sistem aman dan optimal
