# Resume: Course 2 - Module 2 (Security Frameworks, Controls & Konsep Dasar Keamanan)

**Tanggal penyelesaian:** 1 September 2026

---

## 1. Security Framework dan Security Control

### 1.1 Security Framework

Security framework adalah pedoman (guidelines) yang digunakan untuk menyusun rencana guna membantu memitigasi risk dan threat terhadap data serta privasi organisasi. Security professional menggunakan framework ini untuk membuat guidelines dan rencana yang mengedukasi karyawan tentang cara membantu melindungi organisasi.

Security posture adalah kemampuan organisasi dalam mengelola pertahanan terhadap aset dan data kritikalnya serta bereaksi terhadap perubahan. Posture ini dibentuk dan ditingkatkan melalui penerapan framework dan control secara konsisten.

### 1.2 Security Control

Security control adalah safeguard (langkah pengamanan) yang dirancang untuk mengurangi risiko keamanan tertentu. Control ini terbagi menjadi beberapa kategori berdasarkan bentuk implementasinya:

- **Technical controls** — pengamanan berbasis teknologi, misalnya sistem autentikasi, firewall, enkripsi.
- **Physical controls** — pengamanan berbentuk fisik, misalnya surveillance camera, kunci ruangan, akses gerbang.
- **Administrative controls** — pengamanan berbasis kebijakan, prosedur, dan pelatihan.
- **Communication controls** — pengamanan terkait bagaimana informasi disampaikan dan dijaga selama proses komunikasi.

Ketiga istilah kunci yang berkaitan dengan control adalah:

- **Risk** — segala sesuatu yang dapat memengaruhi confidentiality, integrity, atau availability dari sebuah aset.
- **Threat** — segala keadaan atau peristiwa yang dapat berdampak negatif terhadap aset.
- **Asset** — item yang dianggap memiliki nilai bagi organisasi.

---

## 2. CIA Triad

CIA triad adalah model inti yang membantu organisasi mempertimbangkan risiko saat menyusun sistem dan kebijakan keamanan. Tiga prinsipnya:

- **Confidentiality** — hanya pengguna yang berwenang yang dapat mengakses aset atau data tertentu. Ini adalah prinsip yang dipakai ketika tujuan utamanya menjaga data pribadi tetap tidak bocor ke pihak tak berwenang.
- **Integrity** — data bersifat benar, autentik, dan dapat diandalkan (tidak diubah tanpa izin).
- **Availability** — data dapat diakses oleh pihak yang berwenang mengaksesnya kapan pun dibutuhkan.

Terkait autentikasi identitas, ada dua konsep yang berkaitan erat dengan Confidentiality:

- **Authentication** — proses verifikasi siapa seseorang sebenarnya.
- **Authorization** — pemberian akses ke resource tertentu dalam sistem, setelah identitas terverifikasi.
- **Biometrics** — karakteristik fisik unik untuk memverifikasi identitas seseorang. Contohnya: eye scan, fingerprint, dan palm scan (password bukan termasuk biometrics karena bukan karakteristik fisik).
- **Encryption** — proses mengubah data dari format yang dapat dibaca menjadi format terenkode, digunakan untuk menjaga confidentiality data.

---

## 3. NIST Cybersecurity Framework (CSF)

NIST CSF adalah framework sukarela (voluntary) yang terdiri dari standar, pedoman, dan best practice untuk mengelola risiko cybersecurity. CSF tersusun atas enam core function yang saling berkaitan:

- **Govern** — memastikan organisasi menetapkan, mengawasi, dan meningkatkan strategi cybersecurity, kebijakan, peran, serta proses manajemen risiko agar selaras dengan tujuan bisnis dan regulasi.
- **Identify** — manajemen risiko cybersecurity dan dampaknya terhadap orang-orang serta aset organisasi.
- **Protect** — penerapan kebijakan, prosedur, pelatihan, dan tools untuk memitigasi ancaman cybersecurity.
- **Detect** — mengidentifikasi potensi insiden keamanan dan meningkatkan kemampuan monitoring untuk mempercepat serta memperefisienkan deteksi.
- **Respond** — memastikan prosedur yang tepat digunakan untuk contain, netralisir, dan menganalisis insiden keamanan, termasuk menginvestigasi bagaimana threat terjadi, apa yang terdampak, dan asal serangan, serta menerapkan perbaikan pada proses keamanan.
- **Recover** — mengembalikan sistem yang terdampak kembali ke kondisi operasional normal.

Poin yang sering tertukar: fungsi yang berkaitan dengan **contain, netralisir, dan analisis insiden** adalah **Respond**, bukan Detect. Detect berfokus pada identifikasi dan monitoring, bukan penanganan insiden.

---

## 4. NIST Special Publication (S.P.) 800-53

S.P. 800-53 adalah framework terpadu (unified) untuk melindungi keamanan sistem informasi di lingkungan pemerintah federal Amerika Serikat. Berbeda dengan CSF yang bersifat sukarela dan umum untuk organisasi mana pun, S.P. 800-53 berisi katalog security control yang lebih rinci dan menjadi acuan formal di sektor pemerintahan federal AS.

---

## 5. OWASP Secure Design Principles

OWASP (Open Web Application Security Project / Open Worldwide Application Security Project) adalah organisasi nirlaba yang berfokus pada peningkatan keamanan software. OWASP menetapkan sejumlah prinsip desain aman, di antaranya:

- **Defense in depth** — menetapkan beberapa security control (termasuk permission settings) sehingga tercipta banyak security point berlapis yang harus dilewati threat actor untuk membobol organisasi. Prinsip ini tidak mengandalkan satu titik pengamanan saja, melainkan banyak lapisan pertahanan sekaligus.
- **Principle of least privilege** — memberikan akses seminimal mungkin yang dibutuhkan oleh pengguna atau sistem untuk menjalankan fungsinya.
- **Separation of duties** — membagi tanggung jawab atau tugas kritikal ke lebih dari satu orang/pihak agar tidak ada satu pihak yang memegang kendali penuh.
- **Keep security simple** — merancang solusi keamanan yang sederhana dan mudah dipahami, menghindari kompleksitas berlebihan yang justru membuka celah kesalahan.

---

## 6. Security Audit

Security audit adalah peninjauan terhadap security control, kebijakan, dan prosedur organisasi dibandingkan dengan seperangkat ekspektasi (standar) tertentu. Materi ini berfokus pada **internal security audit**, yaitu audit yang dilakukan oleh pihak internal organisasi.

### 6.1 Objective Utama Internal Security Audit

- Meningkatkan security posture organisasi.
- Membantu tim security mengidentifikasi risiko organisasi.
- (Bukan termasuk objective langsung: mengembangkan guiding security statement untuk bisnis — ini bukan tujuan utama internal audit, melainkan bagian dari proses penyusunan kebijakan/framework secara umum.)

### 6.2 Tahapan Dalam Internal Security Audit

- **Establishing the scope and goals** — menetapkan cakupan dan tujuan audit sejak awal.
- **Conducting a risk assessment** — proses identifikasi potensi threat, risk, dan vulnerability guna menentukan security measure apa yang perlu diterapkan.
- **Assessing compliance** — memeriksa kepatuhan terhadap standar/regulasi yang berlaku.
- **Communicating to stakeholders** — menyampaikan hasil audit kepada pihak-pihak terkait.

### 6.3 Informasi yang Dikomunikasikan ke Stakeholder Setelah Audit

- Strategi untuk meningkatkan security posture.
- Ringkasan (summary) dari goals audit.
- Risiko yang sudah ada dan perlu ditangani sekarang atau di masa depan.
- (Bukan termasuk: data detail mengenai insiden cybersecurity di masa lalu — ini bukan output standar yang dikomunikasikan ke stakeholder setelah internal audit.)

### 6.4 Contoh Penerapan Control dari Hasil Audit

Ketika seorang security analyst melakukan internal audit dan menyimpulkan bahwa organisasi perlu memasang surveillance camera di lokasi toko, ini merupakan penerapan **physical controls** — karena pengamanannya berbentuk fisik, bukan kebijakan (administrative) atau sistem teknis (technical).

---

## 7. Glossary

- **Asset** — item yang dianggap memiliki nilai bagi organisasi.
- **Attack vectors** — jalur yang digunakan attacker untuk menembus pertahanan keamanan.
- **Authentication** — proses verifikasi siapa seseorang.
- **Authorization** — pemberian akses ke resource tertentu dalam sistem.
- **Availability** — data dapat diakses oleh pihak yang berwenang.
- **Biometrics** — karakteristik fisik unik untuk verifikasi identitas.
- **Confidentiality** — hanya pengguna berwenang yang dapat mengakses aset/data.
- **CIA Triad** — model yang membantu organisasi mempertimbangkan risiko dalam menyusun sistem dan kebijakan keamanan.
- **Detect** — core function NIST terkait identifikasi insiden dan monitoring.
- **Encryption** — proses mengubah data dari format terbaca menjadi format terenkode.
- **Govern** — core function NIST terkait strategi, kebijakan, dan manajemen risiko cybersecurity organisasi.
- **Identify** — core function NIST terkait manajemen risiko cybersecurity dan dampaknya pada aset/orang.
- **Integrity** — data bersifat benar, autentik, dan dapat diandalkan.
- **NIST CSF** — framework sukarela berisi standar, pedoman, dan best practice manajemen risiko cybersecurity.
- **NIST S.P. 800-53** — framework terpadu untuk melindungi sistem informasi pemerintah federal AS.
- **OWASP** — organisasi nirlaba fokus pada peningkatan keamanan software.
- **Protect** — core function NIST terkait implementasi kebijakan, prosedur, pelatihan, dan tools mitigasi ancaman.
- **Recover** — core function NIST terkait pemulihan sistem ke kondisi normal.
- **Respond** — core function NIST terkait contain, netralisir, analisis insiden, dan perbaikan proses.
- **Risk** — segala sesuatu yang dapat memengaruhi confidentiality, integrity, atau availability aset.
- **Security audit** — peninjauan security control, kebijakan, prosedur terhadap ekspektasi tertentu.
- **Security controls** — safeguard untuk mengurangi risiko keamanan tertentu.
- **Security frameworks** — pedoman untuk menyusun rencana mitigasi risk dan threat.
- **Security posture** — kemampuan organisasi mengelola pertahanan aset/data dan bereaksi terhadap perubahan.
- **Threat** — keadaan atau peristiwa yang dapat berdampak negatif terhadap aset.

---

## 8. Tools & Platform Rujukan

Materi Module 2 ini tidak menyebutkan tools teknis eksternal (seperti VirusTotal, Shodan, dsb) untuk dipakai langsung. Yang disebutkan justru dua badan/organisasi standar yang menjadi rujukan framework:

- **NIST (National Institute of Standards and Technology)** — badan yang menerbitkan CSF dan S.P. 800-53, dipakai sebagai acuan manajemen risiko dan security control.
- **OWASP (Open Web Application Security Project / Open Worldwide Application Security Project)** — organisasi nirlaba yang menerbitkan secure design principles untuk keamanan software.

Tidak ada URL eksplisit yang disebutkan dalam materi/screenshot untuk kedua rujukan ini.

---

## 9. Catatan Ringkas untuk Ditulis Tangan

**Framework & Control**
- Security framework — pedoman mitigasi risk/threat data & privasi
- Security control — safeguard kurangi risiko: technical, physical, administrative, communication
- Security posture — kemampuan kelola pertahanan & respons perubahan
- Risk — hal yang bisa pengaruhi CIA aset
- Threat — kejadian yang bisa berdampak negatif ke aset
- Asset — item bernilai bagi organisasi

**CIA Triad**
- Confidentiality — hanya pihak berwenang akses data
- Integrity — data benar, autentik, reliable
- Availability — data bisa diakses pihak berwenang saat dibutuhkan
- Authentication — verifikasi identitas
- Authorization — pemberian akses resource
- Biometrics — eye scan, fingerprint, palm scan (bukan password)
- Encryption — ubah data readable jadi encoded

**NIST CSF (6 core function)**
- Govern — strategi, kebijakan, manajemen risiko
- Identify — manajemen risiko & dampak ke aset/orang
- Protect — kebijakan, prosedur, training, tools mitigasi
- Detect — identifikasi insiden & monitoring
- Respond — contain, netralisir, analisis insiden, investigasi asal serangan
- Recover — kembalikan sistem ke normal

**NIST S.P. 800-53**
- Framework terpadu, security control sistem info pemerintah federal AS

**OWASP Secure Design Principles**
- Defense in depth — banyak lapisan security point
- Principle of least privilege — akses seminimal mungkin
- Separation of duties — bagi tanggung jawab ke banyak pihak
- Keep security simple — hindari kompleksitas berlebihan

**Internal Security Audit**
- Definisi — review control/policy/prosedur vs ekspektasi
- Objective — improve security posture, identify risiko organisasi
- Tahapan — establish scope & goals, conduct risk assessment, assess compliance, communicate stakeholder
- Output ke stakeholder — strategi improve posture, summary goals, existing risk (bukan detail insiden lampau)
- Contoh: pasang CCTV toko = physical control

**Rujukan Organisasi**
- NIST — penerbit CSF & S.P. 800-53
- OWASP — penerbit secure design principles
