# Resume: Security Principles (TryHackMe)
Tanggal: 22 Oktober 2026

## 1. Pendahuluan: Konsep Adversary dan Keamanan

Keamanan tidak bisa dinilai secara mutlak ("aman" atau "tidak aman") tanpa mempertimbangkan siapa **adversary** (musuh/pihak yang mengancam) yang dihadapi. Mekanisme proteksi harus disesuaikan dengan jenis ancaman yang dihadapi — mekanisme untuk mencegah akses anak kecil berbeda total dengan mekanisme untuk mencegah spionase industri.

Prinsip dasar: tidak ada sistem yang 100% aman. Tujuan keamanan adalah meningkatkan **security posture** (postur keamanan) agar semakin sulit bagi adversary mendapatkan akses, bukan mencapai kesempurnaan mutlak.

## 2. Triad CIA

Tiga pilar dasar untuk menilai keamanan suatu sistem:

- **Confidentiality** — memastikan hanya pihak yang berwenang yang bisa mengakses data.
- **Integrity** — memastikan data tidak bisa diubah secara tidak sah, dan jika terjadi perubahan, hal itu bisa dideteksi.
- **Availability** — memastikan sistem/layanan tetap bisa diakses saat dibutuhkan.

Penekanan terhadap ketiga pilar ini tidak harus sama rata; tergantung konteks. Contoh: pengumuman universitas biasanya tidak perlu confidential, tapi integritasnya kritis (tidak boleh diubah oleh pihak tak berwenang).

### 2.1 Contoh Penerapan CIA

**Belanja online:**
- Confidentiality: nomor kartu kredit hanya boleh diketahui pihak pemroses pembayaran.
- Integrity: alamat pengiriman tidak boleh diubah pihak tak sah.
- Availability: situs/aplikasi toko harus bisa diakses agar transaksi bisa berjalan.

**Rekam medis pasien:**
- Confidentiality: diwajibkan secara hukum di banyak negara; kebocoran berakibat tanggung jawab hukum.
- Integrity: perubahan rekam medis yang salah bisa berakibat pengobatan keliru dan mengancam nyawa.
- Availability: sistem harus tersedia agar diagnosis medis akurat dan tidak error-prone.

## 3. Beyond CIA: Authenticity dan Nonrepudiation

- **Authenticity** — memastikan dokumen/file/data benar-benar berasal dari sumber yang diklaim (bukan palsu/dipalsukan).
- **Nonrepudiation** — memastikan sumber data tidak bisa menyangkal bahwa dirinya adalah sumber dari data tersebut.

Kedua prinsip ini indispensable (tak tergantikan) untuk domain seperti belanja, diagnosis pasien, dan perbankan. Contoh kasus: perusahaan yang menerima pesanan 1000 mobil harus memastikan **authenticity** pesanan tersebut (benar dari pemesan yang sah) sekaligus **nonrepudiation** (pemesan tidak bisa menyangkal telah memesan).

## 4. Parkerian Hexad

Diusulkan oleh Donn Parker (1998), memperluas CIA menjadi enam elemen:

1. Availability
2. Utility
3. Integrity
4. Authenticity
5. Confidentiality
6. Possession

Dua elemen baru yang perlu dipahami:

- **Utility** — fokus pada kegunaan informasi. Data bisa tetap *available* tapi tidak berguna jika, misalnya, kunci dekripsinya hilang (disk terenkripsi tidak bisa diakses meski fisiknya utuh).
- **Possession** — perlindungan dari pengambilan/penyalinan/pengendalian data secara tidak sah. Contoh: backup drive dicuri, atau data dienkripsi ransomware — keduanya menyebabkan hilangnya *possession* meski data secara teknis masih "ada".

## 5. Triad DAD (Kebalikan dari CIA)

DAD merepresentasikan jenis serangan terhadap tiap pilar CIA:

- **Disclosure** — kebalikan dari confidentiality; pembocoran data rahasia.
- **Alteration** — kebalikan dari integrity; perubahan data tidak sah.
- **Destruction/Denial** — kebalikan dari availability; perusakan atau penolakan akses data/layanan.

### 5.1 Contoh Penerapan DAD (Rekam Medis)

- Disclosure: pencurian dan pembocoran publik rekam medis.
- Alteration: modifikasi rekam medis yang bisa mengakibatkan pengobatan salah.
- Destruction/Denial: database rekam medis dibuat tidak tersedia, melumpuhkan fasilitas kesehatan paperless.

Catatan penting: melindungi confidentiality dan integrity secara berlebihan bisa membatasi availability, dan sebaliknya. Implementasi keamanan yang baik membutuhkan **keseimbangan** di antara ketiganya.

## 6. Security Models

Model keamanan menjawab pertanyaan: bagaimana cara membangun sistem yang menjamin satu atau lebih fungsi keamanan (CIA)?

### 6.1 Bell-LaPadula Model

Tujuan: mencapai **confidentiality**. Tiga aturan:

- **Simple Security Property ("no read up")** — subjek level rendah tidak boleh membaca objek level tinggi.
- **Star Security Property ("no write down")** — subjek level tinggi tidak boleh menulis ke objek level rendah (mencegah kebocoran data sensitif).
- **Discretionary Security Property** — menggunakan access matrix (tabel subjek vs objek) untuk menentukan izin read/write, digunakan bersama dua aturan di atas.

Ringkasan: **"write up, read down"**. Keterbatasan: tidak dirancang untuk menangani file-sharing.

### 6.2 Biba Model

Tujuan: mencapai **integrity**. Dua aturan:

- **Simple Integrity Property ("no read down")** — subjek integritas tinggi tidak boleh membaca dari objek integritas rendah.
- **Star Integrity Property ("no write up")** — subjek integritas rendah tidak boleh menulis ke objek integritas tinggi.

Ringkasan: **"read up, write down"** — kebalikan dari Bell-LaPadula, karena fokus berbeda (integrity vs confidentiality). Keterbatasan: tidak menangani insider threat.

### 6.3 Clark-Wilson Model

Tujuan: mencapai **integrity**, dengan konsep:

- **Constrained Data Item (CDI)** — tipe data yang integritasnya ingin dijaga.
- **Unconstrained Data Item (UDI)** — semua data di luar CDI (misal input user/sistem).
- **Transformation Procedures (TPs)** — prosedur terprogram (read/write) yang menjaga integritas CDI.
- **Integrity Verification Procedures (IVPs)** — prosedur yang memeriksa validitas CDI.

### 6.4 Model Keamanan Lain (disebutkan, tidak dibahas detail)

Brewer and Nash model, Goguen-Meseguer model (terkait Domain Separation), Sutherland model, Graham-Denning model, Harrison-Ruzzo-Ullman model.

## 7. Defence-in-Depth (Multi-Level Security)

Prinsip membangun sistem keamanan dengan **banyak lapisan**, bukan mengandalkan satu lapisan tunggal. Analogi: laci terkunci yang berisi barang berharga sebaiknya juga dilindungi oleh ruangan terkunci, pintu utama terkunci, gerbang terkunci, dan kamera pengawas. Lapisan berganda tidak bisa menghentikan semua ancaman, tapi menghalangi sebagian besar dan memperlambat sisanya.

## 8. ISO/IEC 19249:2017

Standar dari ISO (International Organization for Standardization) dan IEC (International Electrotechnical Commission) berjudul *Information technology — Security techniques — Catalogue of architectural and design principles for secure products, systems and applications*.

### 8.1 Lima Prinsip Arsitektural

1. **Domain Separation** — mengelompokkan komponen terkait sebagai satu entitas dengan domain dan atribut keamanan sendiri. Contoh: ring privilege x86 (ring 0 = kernel OS, ring 3 = aplikasi user-mode). Termasuk dalam Goguen-Meseguer Model.
2. **Layering** — struktur sistem menjadi level/layer abstrak agar kebijakan keamanan bisa diterapkan per level dan operasi lebih mudah divalidasi. Contoh: model OSI 7-layer; disk read/write function pada bahasa pemrograman tingkat tinggi. Berkaitan dengan Defence in Depth.
3. **Encapsulation** — menyembunyikan implementasi level-rendah, mencegah manipulasi langsung data, menyediakan metode akses khusus. Contoh: OOP clock object dengan method `increment()` (bukan akses langsung variabel `seconds`); penggunaan API untuk akses database.
4. **Redundancy** — memastikan availability dan integrity lewat duplikasi komponen. Contoh: dual power supply pada server; RAID 5 (toleransi kegagalan 1 drive dari 3, parity mendeteksi perubahan data tidak sah).
5. **Virtualization** — berbagi hardware di antara beberapa OS (relevan untuk cloud). Menyediakan sandboxing, secure detonation, dan pengamatan program berbahaya.

### 8.2 Lima Prinsip Desain

1. **Least Privilege** — "need-to-know basis"; beri hak akses minimum yang dibutuhkan untuk tugas tertentu, tidak lebih (misal: read-only tanpa write jika hanya perlu melihat dokumen).
2. **Attack Surface Minimisation** — meminimalkan vulnerability yang bisa dieksploitasi dengan menonaktifkan layanan/fitur yang tidak dibutuhkan (contoh: hardening Linux dengan disable service tak perlu).
3. **Centralized Parameter Validation** — validasi input dipusatkan pada satu library, mengingat input tak valid bisa dieksploitasi untuk DoS atau remote code execution.
4. **Centralized General Security Services** — memusatkan layanan keamanan (contoh: server autentikasi terpusat), dengan tetap menjaga availability dan menghindari single point of failure.
5. **Preparing for Error and Exception Handling** — sistem harus dirancang fail-safe terhadap error/exception yang pasti terjadi (contoh: firewall yang crash harus block semua traffic, bukan allow semua); pesan error tidak boleh membocorkan informasi sensitif (contoh: memory dump).

## 9. Trust: Zero Trust vs Trust but Verify

Trust adalah topik kompleks; kepercayaan mutlak maupun ketidakpercayaan mutlak sama-sama tidak praktis dalam bisnis, sehingga dibutuhkan prinsip panduan.

### 9.1 Trust but Verify

Selalu memverifikasi entitas (user/sistem) meski sudah dipercaya. Verifikasi dilakukan lewat logging dan review log. Karena tidak feasible memverifikasi semua aktivitas secara manual, dibutuhkan mekanisme otomatis: proxy, intrusion detection system (IDS), intrusion prevention system (IPS).

### 9.2 Zero Trust

Memperlakukan trust sebagai vulnerability, menyasar ancaman insider. Prinsip: "never trust, always verify" — setiap entitas dianggap adversarial sampai terbukti sebaliknya. Tidak memberi trust berdasarkan lokasi/kepemilikan device (berbeda dari model lama yang mempercayai jaringan internal/device perusahaan). Autentikasi dan otorisasi wajib sebelum akses resource apa pun. Jika terjadi breach, dampaknya lebih terkontain dengan arsitektur zero trust.

**Microsegmentation** — implementasi Zero Trust di mana segmen jaringan bisa sekecil satu host; komunikasi antar segmen butuh autentikasi, access control list check, dan syarat keamanan lain.

Catatan: ada batas penerapan zero trust agar tidak merusak operasional bisnis, tapi ini bukan alasan untuk tidak menerapkannya selama masih feasible.

## 10. Threat vs Risk vs Vulnerability

Tiga istilah yang sering tertukar:

- **Vulnerability** — kelemahan yang membuat sistem rentan terhadap serangan/kerusakan.
- **Threat** — potensi bahaya yang berkaitan dengan vulnerability tersebut.
- **Risk** — likelihood (kemungkinan) threat actor mengeksploitasi vulnerability, dikombinasikan dengan dampaknya terhadap bisnis.

### 10.1 Contoh Kasus

**Showroom berpintu kaca:** vulnerability = material kaca rapuh; threat = potensi pintu/jendela dipecahkan; risk = kemungkinan kejadian tersebut plus dampaknya ke bisnis.

**Database rumah sakit:** diketahui vulnerable dan sudah ada proof-of-concept exploit code yang dirilis — ini mengonfirmasi threat adalah nyata, sehingga risk harus dipertimbangkan untuk menentukan langkah mitigasi selanjutnya.

## 11. Shared Responsibility Model

Framework keamanan cloud yang memastikan tanggung jawab keamanan terbagi jelas antara **cloud service provider** dan **customer/user**, mencakup aspek hardware, network infrastructure, operating system, dan applications.

Level tanggung jawab user berbeda tergantung jenis layanan cloud:

- **IaaS (Infrastructure as a Service)** — user punya kendali (dan tanggung jawab) penuh atas operating system.
- **SaaS (Software as a Service)** — user tidak punya akses langsung ke operating system yang mendasarinya; sebagian besar tanggung jawab ada di provider.

Keamanan cloud yang baik membutuhkan kedua pihak (provider dan user) menjalankan bagian tanggung jawab masing-masing.

## 12. Glossary

- **CIA Triad** — Confidentiality, Integrity, Availability; tiga pilar dasar keamanan.
- **DAD Triad** — Disclosure, Alteration, Destruction/Denial; kebalikan dari CIA, merepresentasikan jenis serangan.
- **Confidentiality** — kerahasiaan; hanya pihak berwenang yang bisa akses.
- **Integrity** — keutuhan data; tidak bisa diubah tanpa terdeteksi.
- **Availability** — ketersediaan sistem/layanan saat dibutuhkan.
- **Authenticity** — kepastian bahwa data berasal dari sumber yang diklaim.
- **Nonrepudiation** — sumber data tidak bisa menyangkal keterlibatannya.
- **Parkerian Hexad** — enam elemen keamanan (CIA + Utility, Authenticity, Possession).
- **Utility** — kegunaan data meski secara teknis masih tersedia.
- **Possession** — kendali/penguasaan atas data, terlepas dari isinya.
- **Bell-LaPadula Model** — model keamanan fokus confidentiality ("write up, read down").
- **Biba Model** — model keamanan fokus integrity ("read up, write down").
- **Clark-Wilson Model** — model keamanan fokus integrity berbasis CDI, UDI, TPs, IVPs.
- **Defence-in-Depth** — strategi keamanan berlapis (multi-level security).
- **ISO/IEC 19249** — standar prinsip arsitektural dan desain untuk sistem aman.
- **Domain Separation** — pengelompokan komponen dalam domain dan atribut keamanan tersendiri.
- **Layering** — struktur sistem berlapis untuk penerapan kebijakan keamanan per level.
- **Encapsulation** — penyembunyian implementasi internal, akses hanya lewat metode/API tertentu.
- **Redundancy** — duplikasi komponen untuk menjaga availability dan integrity.
- **Virtualization** — berbagi hardware fisik di antara beberapa OS/sistem.
- **Least Privilege** — pemberian hak akses seminimal mungkin sesuai kebutuhan tugas.
- **Attack Surface Minimisation** — mengurangi jumlah titik yang bisa dieksploitasi.
- **Centralized Parameter Validation** — validasi input dipusatkan pada satu titik/library.
- **Centralized General Security Services** — pemusatan layanan keamanan (contoh: autentikasi).
- **Fail-safe** — sistem dirancang gagal secara aman saat terjadi error.
- **Trust but Verify** — tetap verifikasi meski sudah mempercayai entitas.
- **Zero Trust** — tidak pernah percaya entitas apa pun secara default; "never trust, always verify".
- **Microsegmentation** — segmentasi jaringan hingga level host tunggal sebagai implementasi Zero Trust.
- **Vulnerability** — kelemahan sistem yang berpotensi dieksploitasi.
- **Threat** — potensi bahaya yang berkaitan dengan vulnerability.
- **Risk** — likelihood eksploitasi vulnerability dikombinasikan dampaknya terhadap bisnis.
- **Shared Responsibility Model** — pembagian tanggung jawab keamanan antara cloud provider dan user.
- **IaaS** — Infrastructure as a Service; user bertanggung jawab penuh atas OS.
- **SaaS** — Software as a Service; user tidak mengakses OS yang mendasarinya.
- **Proxy / IDS / IPS** — mekanisme otomatis untuk mendukung prinsip Trust but Verify (proxy, intrusion detection system, intrusion prevention system).

## 13. Tools & Platform Rujukan

Room ini tidak menyebutkan tools eksternal atau platform analis spesifik (seperti Talos, VirusTotal, Shodan, AbuseIPDB, dll) secara eksplisit. Materi bersifat konseptual/teoritis mengenai prinsip-prinsip keamanan, bukan praktik investigasi menggunakan tools pihak ketiga.

## 14. Catatan Ringkas untuk Ditulis Tangan

**CIA Triad**
- Confidentiality — hanya pihak sah yang akses
- Integrity — data tak berubah / perubahan terdeteksi
- Availability — sistem tersedia saat dibutuhkan

**Beyond CIA**
- Authenticity — data dari sumber asli
- Nonrepudiation — sumber tak bisa menyangkal

**Parkerian Hexad**
- CIA + Authenticity + Utility (kegunaan data) + Possession (kendali/penguasaan data)

**DAD Triad (kebalikan CIA)**
- Disclosure — lawan confidentiality
- Alteration — lawan integrity
- Destruction/Denial — lawan availability
- Balance CIA penting — proteksi berlebihan di satu sisi ganggu sisi lain

**Security Models**
- Bell-LaPadula — confidentiality — no read up, no write down ("write up read down")
- Biba — integrity — no read down, no write up ("read up write down")
- Clark-Wilson — integrity — CDI (data dijaga), UDI (data luar), TPs (prosedur jaga integritas), IVPs (verifikasi validitas)
- Model lain (sebut saja): Brewer-Nash, Goguen-Meseguer, Sutherland, Graham-Denning, Harrison-Ruzzo-Ullman

**Defence-in-Depth**
- Keamanan berlapis, bukan satu lapis saja

**ISO/IEC 19249 — Prinsip Arsitektural (5)**
1. Domain Separation — entitas + domain keamanan sendiri (ring 0 vs ring 3)
2. Layering — layer abstrak, kebijakan per level (OSI 7-layer)
3. Encapsulation — sembunyikan implementasi, akses via method/API
4. Redundancy — duplikasi komponen (dual PSU, RAID 5)
5. Virtualization — share hardware, sandboxing

**ISO/IEC 19249 — Prinsip Desain (5)**
1. Least Privilege — akses minimum sesuai tugas
2. Attack Surface Minimisation — kurangi celah (disable service tak perlu)
3. Centralized Parameter Validation — validasi input satu titik
4. Centralized General Security Services — layanan keamanan terpusat (hindari single point of failure)
5. Preparing for Error/Exception Handling — fail-safe, jangan bocorkan info di pesan error

**Trust**
- Trust but Verify — percaya tapi tetap cek log (proxy/IDS/IPS)
- Zero Trust — never trust, always verify; insider threat; microsegmentation (per-host segment + auth)

**Threat vs Risk**
- Vulnerability — kelemahan sistem
- Threat — potensi bahaya dari vulnerability
- Risk — likelihood eksploitasi x dampak bisnis

**Shared Responsibility Model**
- Cloud security dibagi provider vs user
- IaaS — user tanggung jawab penuh OS
- SaaS — user tak akses OS, provider handle sebagian besar
