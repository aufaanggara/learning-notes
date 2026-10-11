# Resume Materi Room: Use the CIA Triad to Protect Organizations

Tanggal: 11 Oktober 2026

## 1. Konsep Inti Triad CIA

### 1.1 Definisi

**CIA triad** adalah model keamanan fundamental yang membantu organisasi mempertimbangkan risiko saat menyusun sistem dan kebijakan keamanan. Model ini terdiri dari tiga elemen yang terus diupayakan oleh analis keamanan siber dan organisasi: **confidentiality**, **integrity**, dan **availability**.

Bagi analis tingkat pemula, ketiga prinsip ini menjadi rujukan konstan dalam melindungi organisasi dan orang-orang yang dilayaninya dari berbagai ancaman, risiko, dan kerentanan, termasuk social engineering, malware, dan data theft.

### 1.2 Security Posture

**Security posture** adalah kemampuan organisasi untuk mengelola pertahanan aset dan data pentingnya serta bereaksi terhadap perubahan.

Menjaga tingkat risiko yang dapat diterima, serta merancang sistem dan kebijakan dengan ketiga elemen CIA, membantu membangun security posture yang berhasil.

## 2. Tiga Komponen Triad CIA

### 2.1 Confidentiality

**Confidentiality** berarti hanya pengguna yang berwenang (authorized users) yang dapat mengakses aset atau data tertentu. Data sensitif diberikan berdasarkan prinsip **need to know**: hanya orang yang memang berwenang menangani data tersebut yang mendapat akses.

Confidentiality dapat diperkuat dengan prinsip desain seperti **principle of least privilege**, yaitu membatasi akses pengguna hanya pada informasi yang dibutuhkan untuk menyelesaikan tugas kerjanya. Membatasi akses adalah salah satu cara menjaga kerahasiaan dan keamanan data privat.

Contoh: kewajiban menggunakan two-factor authentication sebelum masuk ke portal karyawan.

### 2.2 Integrity

**Integrity** berarti data secara terverifikasi benar, autentik, dan dapat diandalkan. Menentukan integritas data dan menganalisis bagaimana data digunakan membantu profesional keamanan memutuskan apakah data layak dipercaya. Karena itu, protokol untuk memverifikasi keaslian data sangat penting.

Dua cara menjaga integrity:

- **Cryptography**: mentransformasi data agar pihak yang tidak berwenang tidak dapat membaca atau mengubahnya (NIST, 2022).
- **Encryption**: proses mengubah data dari format yang dapat dibaca menjadi format terenkode. Dipakai untuk mencegah akses tidak sah dan memastikan data, misalnya pesan di platform chat internal organisasi, tidak dapat dimanipulasi.

Contoh: bila pola belanja atau lokasi transaksi seseorang berubah drastis, bank dapat menonaktifkan sementara aksesnya hingga terverifikasi bahwa pemilik akun yang asli, bukan threat actor, yang bertransaksi. Contoh lain: memverifikasi bahwa tagihan belanja di sebuah toko sudah benar.

### 2.3 Availability

**Availability** berarti data dapat diakses oleh pihak yang berwenang. Data yang tidak dapat diakses tidak berguna dan menghambat orang menjalankan pekerjaannya. Memastikan sistem, jaringan, dan aplikasi berfungsi baik agar akses tepat waktu dan andal dapat menjadi bagian dari tugas harian analis.

Ketika sistem memenuhi availability sekaligus confidentiality, data dapat dipakai saat dibutuhkan dengan tetap terlindungi. Contohnya, organisasi mengizinkan karyawan remote mengakses jaringan internal, tetapi akses tetap dibatasi sesuai peran. Karyawan akuntansi, misalnya, memerlukan akses ke akun korporat, bukan ke data proyek pengembangan yang sedang berjalan.

## 3. Studi Kasus: Sektor Perbankan

Bank menyimpan data pribadi dan finansial dalam jumlah besar, sehingga ketiga prinsip diterapkan sekaligus.

- **Confidentiality**: menjaga informasi pribadi dan finansial nasabah tetap aman.
- **Integrity**: mendeteksi perubahan drastis pada perilaku transaksi, lalu menonaktifkan akses sementara sampai pemilik akun terverifikasi.
- **Availability**: memastikan nasabah dapat mengakses informasi akun dengan mudah melalui web.

Bank juga memakai proses validasi untuk meminimalkan kerugian bila ada akun nasabah yang dicurigai telah disusupi (compromised).

## 4. Latihan Pencocokan Skenario

Skenario berikut dipakai untuk membedakan ketiga komponen:

- Sering login ke akun bank untuk mengecek saldo: **Availability**.
- Wajib two-factor authentication sebelum masuk ke portal karyawan: **Confidentiality**.
- Memverifikasi bahwa tagihan di Store Y sudah benar: **Integrity**.
- Analis memperbaiki aplikasi agar data yang tidak bisa diakses karyawan kembali dapat diakses secara tepat waktu dan andal: **Availability**.

## 5. Glosarium

- **CIA triad**: model keamanan fundamental (confidentiality, integrity, availability) untuk menyusun kebijakan dan sistem keamanan.
- **Confidentiality**: hanya pengguna berwenang yang dapat mengakses data atau aset tertentu.
- **Integrity**: data terverifikasi benar, autentik, dan dapat diandalkan.
- **Availability**: data dapat diakses oleh pihak berwenang saat dibutuhkan.
- **Need to know**: akses data sensitif hanya untuk yang berwenang menanganinya.
- **Principle of least privilege**: akses dibatasi hanya pada yang dibutuhkan untuk tugas kerja.
- **Security posture**: kemampuan organisasi mengelola pertahanan aset dan data kritis serta bereaksi terhadap perubahan.
- **Cryptography**: transformasi data agar tidak dapat dibaca atau diubah pihak tidak berwenang.
- **Encryption**: konversi data dari format terbaca ke format terenkode.
- **Threat actor**: pihak yang berupaya mengakses atau menyalahgunakan aset organisasi secara tidak sah.
- **Compromised**: akun atau sistem yang telah disusupi.

## 6. Tools & Platform Rujukan

Materi room ini tidak menyebutkan tools atau platform eksternal. Satu-satunya rujukan adalah sumber kutipan NIST (2022) pada definisi cryptography, tanpa URL eksplisit.

## 7. Catatan Ringkas untuk Ditulis Tangan

### Model dasar
- CIA triad — model keamanan untuk pertimbangan risiko saat menyusun sistem dan kebijakan
- 3 elemen — Confidentiality, Integrity, Availability
- Security posture — kemampuan kelola pertahanan aset/data dan bereaksi terhadap perubahan

### Confidentiality
- Confidentiality — hanya authorized users yang boleh akses
- Need to know — akses sesuai kewenangan
- Least privilege — akses hanya sebatas kebutuhan tugas
- Contoh — two-factor authentication ke portal karyawan

### Integrity
- Integrity — data benar, autentik, reliable
- Cryptography — ubah data agar tak bisa dibaca/diubah pihak tak berwenang
- Encryption — data terbaca menjadi terenkode, cegah akses dan manipulasi
- Contoh — bank blokir akun saat pola belanja berubah drastis; cek tagihan benar

### Availability
- Availability — data bisa diakses pihak berwenang tepat waktu
- Data tak bisa diakses — tidak berguna, hambat pekerjaan
- Remote access — boleh, tetap dibatasi sesuai peran
- Contoh — cek saldo kapan saja; analis perbaiki aplikasi bermasalah

### Studi kasus bank
- Confidentiality — jaga data pribadi dan finansial nasabah
- Integrity — verifikasi transaksi mencurigakan
- Availability — akses akun online mudah
- Validasi — minimalkan kerugian jika akun compromised

### Ancaman terkait
- Social engineering, malware, data theft — dilawan dengan menjaga ketiga prinsip CIA
- Threat actor — pelaku ancaman
