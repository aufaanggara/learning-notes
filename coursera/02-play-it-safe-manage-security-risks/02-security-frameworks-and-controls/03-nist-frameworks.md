# Resume Materi: NIST Frameworks

**Tanggal: 11 Oktober 2026**
**Room: NIST Frameworks (NIST CSF dan NIST SP 800-53)**

&nbsp;

## 1. Konsep Dasar Framework

### 1.1 Definisi dan Tujuan

**Framework** adalah titik awal bagi organisasi untuk menyusun rencana yang mengurangi risiko, ancaman, dan kerentanan terhadap data dan aset sensitif. Framework bukan solusi jadi, melainkan fondasi yang diadaptasi sesuai kebutuhan organisasi.

### 1.2 NIST sebagai Penyusun Framework

**National Institute of Standards and Technology (NIST)** adalah organisasi berbasis Amerika Serikat yang menerbitkan panduan keamanan siber. Panduannya dipakai analis di seluruh dunia, baik di organisasi profit, nonprofit, maupun lembaga pemerintah.

Dua framework NIST yang dibahas di room ini adalah **NIST Cybersecurity Framework (CSF)** dan **NIST Special Publication (SP) 800-53**.

&nbsp;

## 2. NIST Cybersecurity Framework (CSF)

### 2.1 Definisi

**NIST CSF** adalah framework **sukarela (voluntary)** yang berisi standar, pedoman (guidelines), dan praktik terbaik (best practices) untuk mengelola risiko keamanan siber. Framework ini sangat dihormati dan relevan bagi organisasi jenis apa pun.

### 2.2 Jumlah Fungsi Inti

Materi video menyebutkan **lima fungsi inti**: Identify, Protect, Detect, Respond, Recover. Soal kuis menyebutkan **enam fungsi inti** dengan tambahan **Govern**: Govern, Identify, Protect, Detect, Respond, Recover.

Perbedaan ini kemungkinan mencerminkan **CSF 2.0**, yang menambahkan Govern sebagai fungsi keenam. Untuk ujian, ikuti versi enam fungsi, dan ingat bahwa Govern tidak dijelaskan perannya secara rinci di video.

### 2.3 Penjelasan Tiap Fungsi

**Identify**
Mengelola risiko keamanan siber beserta dampaknya terhadap **people** dan **assets** organisasi. Fokusnya memahami aset, risiko, dan kebijakan organisasi sebagai fondasi pengelolaan ancaman. Contoh tugas analis: memantau sistem dan perangkat di jaringan internal untuk menemukan potensi masalah keamanan.

**Protect**
Strategi melindungi organisasi lewat penerapan **policies**, **procedures**, **training**, dan **tools** untuk memitigasi ancaman. Ketika muncul ancaman yang belum dikenal, analis mempelajari data historis dan memperbaiki kebijakan serta prosedur.

**Detect**
Mengidentifikasi potensi insiden keamanan dan meningkatkan kemampuan monitoring agar deteksi lebih cepat dan efisien. Contoh tugas analis: meninjau setup tool keamanan baru agar mampu menandai (**flagging**) risiko level low, medium, atau high, lalu mengirim alert ke tim keamanan.

**Respond**
Memastikan prosedur yang tepat digunakan untuk **contain** (menahan), **neutralize** (menetralkan), dan menganalisis insiden, serta menghasilkan perbaikan proses. Contoh tugas analis: mengumpulkan dan mengorganisasi data untuk mendokumentasikan insiden dan mengusulkan perbaikan agar tidak terulang.

**Recover**
Mengembalikan sistem yang terdampak ke operasi normal. Contoh tugas analis entry-level: memulihkan sistem, data, dan aset (misalnya berkas finansial atau legal) setelah insiden seperti **breach**.

### 2.4 Sifat Kelima Fungsi

Fungsi-fungsi ini saling melengkapi dan mencakup pendekatan **proaktif** (Identify, Protect) hingga **reaktif** (Detect, Respond, Recover). Insiden pasti terjadi, sehingga kemampuan pulih dengan cepat dari kerusakan adalah kunci meminimalkan risiko.

&nbsp;

## 3. Studi Kasus: Workstation yang Disusupi

Seorang analis menerima notifikasi berisiko tinggi bahwa sebuah workstation telah disusupi. Penanganannya selaras dengan fungsi inti CSF:

1. **Identify:** menemukan workstation bermasalah dan perangkat tak dikenal yang terpasang padanya.
2. **Protect:** memblokir perangkat tak dikenal secara remote untuk menghentikan potensi ancaman.
3. **Detect:** melepas workstation terinfeksi dari jaringan agar kerusakan tidak menyebar, lalu memakai tools untuk mendeteksi perilaku **threat actor** tambahan dan mengidentifikasi perangkat tersebut.
4. **Respond:** menyelidiki siapa yang memakai perangkat, bagaimana ancaman terjadi, apa yang terdampak, dan dari mana asalnya. Hasilnya, seorang karyawan mengisi daya ponsel terinfeksi lewat port USB laptop kerjanya.
5. **Recover:** memulihkan file dan data yang terdampak serta memperbaiki kerusakan pada workstation.

Pelajaran utamanya: fungsi inti CSF memberi panduan spesifik untuk menangani insiden secara tepat dan cepat, menurunkan risiko, dan menutup potensi kerentanan.

&nbsp;

## 4. NIST Special Publication (SP) 800-53

### 4.1 Definisi

**NIST SP 800-53** adalah kerangka kerja terpadu (**unified**) untuk melindungi keamanan sistem informasi di **pemerintah federal Amerika Serikat**, termasuk sistem yang disediakan perusahaan swasta untuk dipakai pemerintah federal.

### 4.2 Tujuan Kontrol Keamanan

Kontrol keamanan di SP 800-53 dipakai untuk menjaga **CIA triad** pada sistem pemerintah:

- **Confidentiality:** data hanya dapat diakses pihak yang berwenang.
- **Integrity:** data tidak diubah secara tidak sah.
- **Availability:** data dan sistem tersedia saat dibutuhkan.

&nbsp;

## 5. Perbandingan CSF dan SP 800-53

| Aspek | NIST CSF | NIST SP 800-53 |
|---|---|---|
| Sifat | Sukarela, panduan umum | Kerangka kontrol terpadu |
| Cakupan | Organisasi apa pun | Sistem informasi pemerintah federal AS |
| Relevansi | Dikenal hampir semua profesional keamanan | Krusial bagi yang ingin bekerja di pemerintah federal AS |

Keduanya saling melengkapi sebagai bagian dari ekosistem framework dan kontrol keamanan yang bekerja bersama.

&nbsp;

## 6. Glosarium Istilah Kunci

- **Framework:** kerangka berisi standar, pedoman, dan praktik terbaik sebagai titik awal strategi keamanan.
- **NIST:** organisasi AS penyedia panduan keamanan siber yang dipakai global.
- **NIST CSF:** framework sukarela untuk mengelola risiko keamanan siber.
- **Govern:** fungsi inti keenam CSF (CSF 2.0) yang muncul di kuis.
- **Identify:** fungsi untuk memahami aset, risiko, dan kebijakan organisasi.
- **Protect:** fungsi penerapan kebijakan, prosedur, pelatihan, dan tools untuk mitigasi.
- **Detect:** fungsi identifikasi insiden dan peningkatan monitoring.
- **Respond:** fungsi menahan, menetralkan, dan menganalisis insiden serta memperbaiki proses.
- **Recover:** fungsi memulihkan sistem, data, dan aset ke kondisi normal.
- **NIST SP 800-53:** kerangka kontrol terpadu untuk sistem informasi pemerintah federal AS.
- **CIA triad:** Confidentiality, Integrity, Availability.
- **Threat actor:** pelaku yang melakukan aktivitas berbahaya terhadap sistem atau data.
- **Breach:** insiden kebocoran atau penyusupan data atau sistem.
- **Flagging:** penandaan tingkat risiko (low, medium, high) oleh tool keamanan.

&nbsp;

## 7. Tools & Platform Rujukan

Tidak ada tools atau platform eksternal yang disebutkan dalam materi video maupun kuis pada room ini. Materi bersifat konseptual mengenai framework NIST.

&nbsp;

## 8. Catatan Ringkas untuk Ditulis Tangan

### Framework dan NIST
- Framework — titik awal susun rencana kurangi risiko, ancaman, kerentanan
- NIST — lembaga AS, panduan dipakai global
- NIST punya 2 framework utama — CSF dan SP 800-53

### NIST CSF
- CSF — voluntary; standar + guidelines + best practices kelola risiko siber
- Video — 5 fungsi; kuis — 6 fungsi (CSF 2.0 tambah Govern)
- Urutan 6 fungsi — Govern, Identify, Protect, Detect, Respond, Recover
- Proaktif — Identify, Protect
- Reaktif — Detect, Respond, Recover

### Fungsi Inti
- Govern — fungsi ke-6 (CSF 2.0), tidak dirinci di video
- Identify — kelola risiko thd people + assets; paham aset, risiko, kebijakan
- Protect — policies, procedures, training, tools; pelajari data historis, perbaiki kebijakan
- Detect — identifikasi insiden, tingkatkan monitoring; tool flag low/medium/high lalu alert
- Respond — contain, neutralize, analyze; dokumentasi + usul perbaikan proses
- Recover — kembalikan sistem ke normal; pulihkan sistem, data, aset (file finansial/legal)

### Studi Kasus Workstation
- Notifikasi high-risk — workstation compromised
- Identify — temukan workstation + perangkat tak dikenal
- Protect — blokir perangkat secara remote
- Detect — lepas workstation, deteksi aktivitas threat actor
- Respond — investigasi siapa, bagaimana, apa terdampak, asal
- Penyebab — karyawan charge ponsel terinfeksi via USB laptop kerja
- Recover — pulihkan file/data, perbaiki workstation

### NIST SP 800-53
- SP 800-53 — kerangka terpadu, sistem informasi pemerintah federal AS
- Termasuk sistem perusahaan swasta untuk pemerintah federal
- Kontrol — jaga CIA triad
- Confidentiality — kerahasiaan
- Integrity — data tidak diubah tanpa izin
- Availability — tersedia saat dibutuhkan
- Penting — CSF untuk semua profesional; SP 800-53 untuk kerja di pemerintah federal AS

### Istilah Lain
- Threat actor — pelaku ancaman
- Breach — kebocoran/penyusupan data
- Flagging — penandaan level risiko

### Command/Tools
- Tidak ada command, switch, maupun tools eksternal di room ini
