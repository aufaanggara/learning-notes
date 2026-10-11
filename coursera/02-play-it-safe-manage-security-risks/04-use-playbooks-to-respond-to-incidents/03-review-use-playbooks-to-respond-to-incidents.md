# Resume Materi: Playbooks dan Incident Response (Course 2, Module 4)

Tanggal: 11 Oktober 2026
Room: Google Cybersecurity Certificate, Course 2 "Play It Safe: Manage Security Risks", Module 4 (playbooks dan incident response)

## 1. Konsep Inti

### 1.1 Incident Response

**Incident response** adalah upaya cepat sebuah organisasi untuk mengidentifikasi serangan, menahan kerusakan, dan memperbaiki dampak dari **security breach**.

Tujuannya meminimalkan dampak dan kerusakan terhadap organisasi serta orang-orang yang dilayani organisasi tersebut.

### 1.2 Playbook

**Playbook** adalah manual yang memberikan detail tentang setiap tindakan operasional. Dalam konteks keamanan, playbook menjadi panduan terstruktur bagi **security analyst** saat menangani **security incident**.

Playbook adalah salah satu alat esensial seorang security analyst, bersama alat lain seperti **SIEM tools**.

### 1.3 Tujuan dan Manfaat Playbook

- Memberi pendekatan yang **terstruktur dan konsisten** dalam menangani security incident.
- Membantu tim keamanan **merespons situasi mendesak dengan cepat**.
- Meningkatkan **akurasi** saat mengidentifikasi dan memitigasi insiden.
- Memungkinkan analyst mengambil keputusan yang tepat tentang bagaimana dan kapan merespons.

### 1.4 Jenis Playbook

Organisasi menggunakan **berbagai jenis playbook untuk situasi yang berbeda**. Playbook tidak bersifat satu untuk semua kondisi, sehingga prosedur disesuaikan dengan jenis insiden atau peristiwa bisnis yang dihadapi.

### 1.5 Isi Incident Response Playbook

Incident response playbook menguraikan proses untuk **komunikasi** dan **dokumentasi** dari sebuah security breach. Komunikasi yang baik dengan tim, bersama kepatuhan pada langkah-langkah playbook, menjamin efektivitas seorang security analyst.

## 2. Enam Fase Incident Response Playbook

Incident response playbook terdiri dari enam fase. Setiap fase dikenali lewat ciri aktivitasnya.

### 2.1 Preparation

Fase ketika analyst **mendokumentasikan prosedur** yang harus diikuti saat terjadi security breach, **menetapkan staffing plans**, dan **mengedukasi karyawan**. Fase ini dikerjakan sebelum insiden terjadi.

### 2.2 Detection and Analysis

Fase untuk **menentukan apakah sebuah breach telah terjadi**. Fase ini memungkinkan bisnis memastikan ada atau tidaknya pelanggaran sebelum mengambil tindakan lanjutan.

### 2.3 Containment

Fase yang **mencegah risiko berkelanjutan terhadap aset dan data penting**. Containment menjadi prioritas tinggi bagi organisasi karena menghentikan insiden agar tidak terus merugikan.

### 2.4 Eradication and Recovery

Fase penghapusan penyebab insiden dan pemulihan kondisi normal, selaras dengan definisi incident response yaitu memperbaiki dampak dari security breach.

### 2.5 Post-incident Activity

Fase ketika tim keamanan dapat melakukan **analisis skala penuh untuk menentukan akar penyebab** insiden dan menggunakan hasilnya untuk **meningkatkan security posture** perusahaan secara keseluruhan.

Hasil pembelajaran dari insiden masa lalu, seperti pembahasan cara menyempurnakan strategi respons untuk insiden mendatang, berujung pada **pembaruan playbook**.

### 2.6 Coordination

Fase ketika analyst **membagikan informasi kepada pemangku kepentingan utama** (key stakeholders) berdasarkan **standar yang telah ditetapkan organisasi**, demi respons dan penyelesaian breach yang terorganisir.

### 2.7 Cara Cepat Membedakan Fase

Skenario ujian biasanya menggambarkan satu aktivitas. Petunjuk pengenalnya:

- Dokumentasi prosedur, staffing plans, edukasi karyawan: **Preparation**
- Menentukan apakah breach terjadi: **Detection and analysis**
- Mencegah risiko berkelanjutan pada aset dan data: **Containment**
- Analisis akar penyebab dan peningkatan security posture: **Post-incident activity**
- Berbagi informasi dengan stakeholder sesuai standar organisasi: **Coordination**

## 3. Playbook dan SIEM Tools

**SIEM tools** dan playbook bekerja sama untuk menyediakan cara yang efisien dalam menangani security incident.

- **SIEM tools menganalisis data.**
- **SIEM alerts memberi tahu** tim keamanan tentang potensi ancaman.
- SIEM alerts **tidak** memberi langkah spesifik untuk mengidentifikasi dan merespons insiden. Langkah tersebut berasal dari **playbook**.

Pembagian peran: SIEM mendeteksi dan memberi peringatan, playbook menuntun tindakan.

## 4. Glossary

- **Incident response**: upaya cepat organisasi untuk mengidentifikasi serangan, menahan kerusakan, dan memperbaiki dampak security breach.
- **Playbook**: manual yang memberikan detail tentang setiap tindakan operasional.
- **Security incident**: peristiwa keamanan yang ditangani analyst menggunakan playbook.
- **Security breach**: pelanggaran keamanan yang dampaknya ditangani lewat incident response.
- **Security posture**: kondisi keamanan organisasi secara keseluruhan, ditingkatkan lewat post-incident activity.
- **Stakeholders**: pihak terkait yang menerima informasi pada fase coordination.
- **SIEM tools**: alat yang menganalisis data dan menghasilkan alerts atas potensi ancaman.
- **Preparation, Detection and analysis, Containment, Eradication and recovery, Post-incident activity, Coordination**: enam fase incident response playbook (lihat bagian 2).

## 5. Tools & Platform Rujukan

Materi room ini tidak menyebut link atau platform eksternal spesifik. Satu-satunya kategori alat yang disebut:

- **SIEM tools**: menganalisis data dan memberi alerts potensi ancaman (tanpa URL, tidak ada produk spesifik yang disebut).

Room ini bersifat teori murni, sehingga tidak ada command atau switch yang dibahas.

## 6. Catatan Ringkas untuk Ditulis Tangan

### Definisi

- Incident response — respons cepat: identifikasi serangan, tahan kerusakan, perbaiki dampak breach
- Playbook — manual berisi detail setiap tindakan operasional
- Security posture — kondisi keamanan organisasi secara umum

### Fungsi Playbook

- Terstruktur dan konsisten
- Respons cepat pada situasi mendesak
- Akurasi identifikasi dan mitigasi insiden
- Jenis berbeda untuk situasi berbeda
- Isi IR playbook — proses komunikasi dan dokumentasi breach

### Enam Fase

- Preparation — dokumentasi prosedur, staffing plans, edukasi karyawan
- Detection and analysis — tentukan apakah breach terjadi
- Containment — cegah risiko berkelanjutan ke aset dan data (prioritas tinggi)
- Eradication and recovery — hilangkan penyebab, pulihkan kondisi
- Post-incident activity — analisis akar penyebab, tingkatkan security posture, update playbook
- Coordination — bagi informasi ke stakeholder sesuai standar organisasi

### SIEM dan Playbook

- SIEM tools — menganalisis data
- SIEM alerts — memberi tahu potensi ancaman
- Langkah spesifik respons — dari playbook, bukan dari SIEM alerts
- SIEM + playbook — kerja sama, penanganan insiden efisien

### Command / Switch

- Tidak ada — room ini teori murni
