# Resume Room: Cloud Hardening — 24 Oktober 2026

Cakupan room: video cloud network hardening, video Kelsey (Google Cloud), reading "Secure the cloud", reading "Cryptography and cloud security", dan quiz "Test your knowledge: Cloud hardening".

## 1. Gambaran Umum

Banyak organisasi kini memakai layanan jaringan di cloud, sehingga security analyst harus mengamankan **cloud network** selain jaringan **on-premises**. Cloud network adalah kumpulan server atau komputer yang menyimpan sumber daya dan data di data center jarak jauh, diakses melalui internet. Jaringan ini menyediakan penyimpanan, daya pemrosesan, dan analitik data secara on-demand.

### 1.1 Definisi Cloud Computing

**Cloud computing** adalah model yang memungkinkan akses jaringan yang nyaman dan on-demand ke sekumpulan sumber daya komputasi yang dapat dikonfigurasi dan digunakan bersama (shared pool). Sumber daya dapat dikonfigurasi dan dilepaskan dengan upaya manajemen atau interaksi minimal dengan penyedia layanan.

### 1.2 Alasan Organisasi Memilih Cloud

Organisasi memilih cloud karena kemudahan deployment, kecepatan deployment, penghematan biaya, dan skalabilitas. Di sisi lain, cloud membawa tantangan keamanan yang unik yang harus dipahami analyst.

## 2. Cloud Network Hardening

Server cloud, seperti web server biasa, tetap memerlukan pemeliharaan melalui prosedur security hardening. **Cloud service provider (CSP)** menghosting server tersebut, tetapi tidak dapat mencegah semua intrusi, terutama dari malicious actors baik internal maupun eksternal organisasi.

### 2.1 Server Baseline Image

Pembeda utama antara cloud network hardening dan network hardening tradisional adalah penggunaan **server baseline image** untuk semua instance server yang tersimpan di cloud.

Data pada server cloud dibandingkan dengan baseline image untuk memastikan tidak ada **unverified changes**. Perubahan yang tidak terverifikasi dapat menandakan adanya intrusi di jaringan cloud.

### 2.2 Pemisahan Berdasarkan Service Category

Seperti pada OS hardening, data dan aplikasi di cloud dipisahkan berdasarkan kategori layanannya:

- Aplikasi lama dipisahkan dari aplikasi baru.
- Software untuk fungsi internal dipisahkan dari aplikasi front-end yang dilihat pengguna.

Pernyataan bahwa pemisahan ini tidak diperlukan adalah salah (jawaban quiz: False).

## 3. Private Cloud vs Public Cloud

Materi ini disampaikan oleh Kelsey, distinguished engineer di Google Cloud yang menangani compute platforms dan topik keamanan.

### 3.1 Analogi Private vs Public Cloud

Sebelum era cloud, sebagian besar perusahaan memiliki data center sendiri. Model ini disebut **private cloud**.

> *Seperti tinggal sendirian di rumah: barang bebas ditaruh di mana saja, pintu dalam boleh tidak dikunci karena hanya ada satu penghuni.*

**Public cloud** berbagi sumber daya dengan pihak lain.

> *Seperti punya roommates: barang mulai dikunci bahkan saat berada di dalam rumah.*

Implikasinya, **security discipline** di public cloud harus jauh lebih ketat dibanding private cloud karena sumber daya digunakan bersama.

### 3.2 Pesan Karier dan Belajar

Sertifikasi dapat menjadi pintu masuk karier di teknologi. Bagi Kelsey, jarak menuju pekerjaan pertamanya hanya $35, yaitu harga sebuah buku sertifikasi pada tahun 1999.

Cara menguasai keterampilan cloud adalah hands-on: jalankan software yang sudah ada di cloud, lalu pakai tools yang menguji sistem tersebut untuk menemukan kelemahannya. Tools tersebut sama dengan yang dipakai profesional. Belajar disebut sebagai **superpower** yang membantu mendapat pekerjaan sekaligus menentukan pekerjaan berikutnya.

## 4. Pertimbangan Keamanan Cloud

### 4.1 Identity Access Management (IAM)

**IAM** adalah kumpulan proses dan teknologi yang membantu organisasi mengelola identitas digital di lingkungannya, sekaligus mengotorisasi cara pengguna memakai sumber daya cloud.

Masalah umum adalah konfigurasi cloud user role yang longgar. User role yang tidak dikonfigurasi dengan benar memungkinkan pengguna tidak berwenang mengakses operasi cloud yang kritis.

### 4.2 Konfigurasi

Setiap layanan cloud memerlukan konfigurasi presisi untuk menjaga standar keamanan dan compliance. Tantangan meningkat saat **cloud migration**, karena setiap proses yang dimigrasikan harus dikonfigurasi dengan akurat.

**Misconfigured** cloud services adalah sumber umum pelanggaran keamanan. Karena itu administrator dan arsitek jaringan harus teliti selama migrasi dan pengelolaan berkelanjutan.

### 4.3 Attack Surface

CSP menawarkan banyak aplikasi dan layanan dengan biaya rendah. Setiap layanan membawa risiko dan kerentanan sendiri, sehingga menambah **attack surface** organisasi. Penambahan ini harus diimbangi dengan langkah keamanan yang lebih banyak.

Banyak layanan berarti banyak entry points yang dapat dipakai untuk memasukkan malware. Namun, bila jaringan dirancang dengan benar, memakai beberapa layanan tidak menambah entry points dalam desain jaringan. CSP cenderung mengutamakan opsi yang lebih aman dan mendapat pengawasan lebih ketat daripada jaringan on-premises tradisional.

### 4.4 Zero-Day Attacks

**Zero day** adalah exploit yang sebelumnya tidak diketahui. Risikonya berlaku bagi cloud maupun on-premise.

CSP lebih mungkin mengetahui serangan zero day lebih awal dibanding organisasi TI tradisional. CSP dapat melakukan patching pada hypervisors dan memigrasikan workload ke virtual machine lain agar pelanggan tidak terdampak. Tersedia juga tools patching di level sistem operasi yang dapat dipakai organisasi.

### 4.5 Visibility and Tracking

Pada jaringan on-premise maupun cloud, administrator dapat mengakses setiap paket data, melakukan sniffing, dan menginspeksinya untuk menilai performa atau mendeteksi ancaman.

Di cloud, visibilitas diberikan melalui **flow logs** dan tools seperti **packet mirroring**. CSP bertanggung jawab atas keamanan di cloud tetapi tidak mengizinkan pelanggan memantau traffic di server milik CSP. Hal ini dapat menjadi kekhawatiran bagi organisasi yang terbiasa memiliki akses penuh.

CSP membayar **third-party audits** untuk memverifikasi keamanan jaringan cloud dan mengidentifikasi kerentanan. Audit ini membantu organisasi menentukan apakah kerentanan berasal dari infrastruktur on-premise dan apakah ada compliance lapses dari CSP.

### 4.6 Perubahan Cepat di Cloud

CSP adalah organisasi besar yang terus mengikuti kemajuan teknologi. Pembaruan layanan dapat memengaruhi pertimbangan keamanan pelanggan, misalnya konfigurasi koneksi yang perlu diubah.

Organisasi biasanya harus memperbarui proses TI-nya. Best practices untuk perubahan dan konfigurasi tetap dapat dipakai, tetapi pendekatannya mungkin perlu disesuaikan dengan perubahan dari CSP.

Opsi cloud yang menarik bagi perusahaan kecil (yang tidak mampu membangunnya sendiri) menambah kompleksitas pada security profile. Setiap layanan membutuhkan personel keamanan untuk memantaunya.

## 5. Shared Responsibility Model

**Shared responsibility model** adalah prinsip keamanan cloud yang diterima luas. Pembagian tanggung jawabnya:

- **CSP** bertanggung jawab atas keamanan infrastruktur cloud: physical data centers, hypervisors, dan host operating systems.
- **Organisasi pengguna** bertanggung jawab atas aset dan proses yang disimpan atau dijalankan di cloud, termasuk aplikasi dan konfigurasi cloud.

Model ini memastikan CSP dan pengguna sepakat tentang di mana tanggung jawab keamanan masing-masing dimulai dan berakhir. Masalah muncul saat organisasi mengira CSP mengurus keamanan yang sebenarnya bukan tanggung jawab CSP. Contohnya, CSP mengamankan cloud, tetapi organisasi wajib memastikan layanan dikonfigurasi sesuai kebutuhan keamanannya.

Dalam quiz, pihak yang bertanggung jawab atas keamanan jaringan cloud: individual users, cloud service provider, dan security team (bukan research department).

## 6. Teknik Cloud Security Hardening

Teknik umum meliputi IAM, hypervisors, baselining, kriptografi, dan cryptographic erasure. IAM sudah dijelaskan pada bagian 4.1.

### 6.1 Hypervisors

**Hypervisor** mengabstraksi hardware host dari lingkungan sistem operasi. Ada dua tipe:

- **Type one**: berjalan langsung di atas hardware komputer host. Contoh: VMware ESXi. Tipe ini umum dipakai CSP.
- **Type two**: beroperasi di atas software komputer host. Contoh: VirtualBox.

CSP mengelola hypervisor dan komponen virtualisasi lain, memastikan sumber daya dan lingkungan cloud tersedia, serta memberikan patch dan pembaruan berkala. Pelanggan jarang berurusan langsung dengan hypervisor.

Kerentanan atau misconfiguration pada hypervisor dapat menyebabkan **VM escape**, yaitu exploit di mana malicious actor mengakses hypervisor utama sehingga berpotensi mengakses komputer host dan VM lain.

### 6.2 Baselining

**Baseline** adalah titik referensi tetap untuk membandingkan perubahan pada lingkungan cloud. Konfigurasi dan setup yang tepat meningkatkan keamanan dan performa. Contoh baseline di cloud:

- Membatasi akses ke admin portal lingkungan cloud.
- Mengaktifkan password management.
- Mengaktifkan file encryption.
- Mengaktifkan threat detection services untuk database SQL.

## 7. Kriptografi dan Manajemen Kunci

### 7.1 Kriptografi di Cloud

Kriptografi mengamankan data yang diproses dan disimpan di cloud, memakai enkripsi dan sistem key management untuk menjamin **integrity** dan **confidentiality** data. Enkripsi adalah salah satu cara utama mengamankan data sensitif di cloud, terutama data at rest.

**Encryption** adalah proses mengacak informasi menjadi **ciphertext** yang tidak terbaca tanpa **encryption key**. Awalnya enkripsi berupa pengodean manual dengan algoritma yang mengubah huruf atau angka menjadi nilai baru. Enkripsi modern bergantung pada kerahasiaan **kunci**, bukan kerahasiaan algoritma.

### 7.2 Cryptographic Erasure

**Cryptographic erasure** adalah metode menghapus encryption key dari data terenkripsi. Metode penghancuran data tradisional kurang efektif di cloud.

**Crypto-shredding** adalah teknik baru di mana kunci kriptografis untuk dekripsi dihancurkan, sehingga data tidak dapat dipahami dan tidak dapat didekripsi siapa pun. Semua salinan kunci harus dihancurkan agar tidak ada yang bisa mengakses data di masa depan.

### 7.3 Key Management

Enkripsi modern bergantung pada keamanan kunci. Dua langkah perlindungan tambahan:

- **Trusted platform module (TPM)**: chip komputer yang menyimpan password, sertifikat, dan encryption keys secara aman.
- **Cloud hardware security module (CloudHSM)**: perangkat komputasi yang menyediakan penyimpanan aman untuk kunci kriptografis dan memproses operasi kriptografis seperti enkripsi dan dekripsi.

Pelanggan tidak punya akses langsung ke CSP, tetapi dapat meminta audit dan laporan keamanan. Pelanggan biasanya tidak punya akses ke kunci spesifik yang dipakai CSP untuk mengenkripsi data mereka, namun hampir semua CSP mengizinkan pelanggan membawa kunci sendiri, tergantung layanan.

Konsekuensinya, pelanggan bertanggung jawab atas kunci miliknya dan kerahasiaannya. CSP terbatas dalam membantu jika kunci pelanggan di-compromise atau dihancurkan. Manfaat shared responsibility: pelanggan tidak sepenuhnya bertanggung jawab atas pemeliharaan infrastruktur kriptografis. Organisasi dapat menilai risiko dengan meninjau audit dan kontrol keamanan CSP. Untuk federal contractors, **FedRAMP** menyediakan daftar CSP yang terverifikasi.

## 8. Command dan Switch

Room ini murni teori dan tidak membahas command, tool command-line, maupun switch/flag.

## 9. Glossary

- **Attack surface**: seluruh titik dan layanan yang dapat dieksploitasi; bertambah seiring jumlah layanan.
- **Baseline**: titik referensi tetap untuk membandingkan perubahan konfigurasi.
- **Cloud computing**: model akses on-demand ke shared pool sumber daya komputasi yang dapat dikonfigurasi.
- **Cloud network**: kumpulan server/komputer di data center jarak jauh yang diakses lewat internet.
- **Cloud service provider (CSP)**: penyedia layanan cloud yang mengelola infrastruktur.
- **CloudHSM**: perangkat penyimpanan aman dan pemrosesan operasi kriptografis.
- **Ciphertext**: hasil enkripsi yang tidak terbaca tanpa kunci.
- **Cryptographic erasure**: penghapusan encryption key untuk data terenkripsi.
- **Crypto-shredding**: penghancuran semua salinan kunci dekripsi agar data tidak dapat dibuka.
- **Encryption**: proses mengacak informasi menjadi ciphertext.
- **FedRAMP**: penyedia daftar CSP terverifikasi bagi federal contractors.
- **Flow logs**: log yang memberi visibilitas traffic di cloud.
- **Hypervisor**: lapisan yang mengabstraksi hardware host dari lingkungan OS (type one dan type two).
- **IAM**: proses dan teknologi pengelolaan identitas digital dan otorisasi akses sumber daya cloud.
- **Misconfiguration**: kesalahan konfigurasi layanan; sumber umum masalah keamanan cloud.
- **Packet mirroring**: tool visibilitas paket di cloud.
- **Private cloud**: data center milik sendiri, dipakai satu organisasi.
- **Public cloud**: cloud yang sumber dayanya dibagi dengan pihak lain.
- **Server baseline image**: image acuan untuk semua instance server cloud guna mendeteksi unverified changes.
- **Shared responsibility model**: pembagian tanggung jawab keamanan antara CSP dan pelanggan.
- **Third-party audits**: audit pihak ketiga yang dibayar CSP untuk memverifikasi keamanan cloud.
- **TPM**: chip yang menyimpan password, sertifikat, dan encryption keys secara aman.
- **Unverified changes**: perubahan tak terverifikasi yang dapat menandakan intrusi.
- **VM escape**: exploit untuk mengakses hypervisor utama dari VM.
- **Zero day**: exploit yang sebelumnya tidak diketahui.

## 10. Tools & Platform Rujukan

Tidak ada URL eksplisit di materi; hanya nama yang disebutkan.

- **VMware ESXi**: contoh hypervisor type one.
- **VirtualBox**: contoh hypervisor type two.
- **Trusted platform module (TPM)**: chip penyimpan kunci, password, dan sertifikat.
- **Cloud hardware security module (CloudHSM)**: penyimpanan kunci dan pemrosesan operasi kriptografis.
- **FedRAMP**: daftar CSP terverifikasi untuk federal contractors.
- **Flow logs dan packet mirroring**: sarana visibilitas traffic di cloud.

## 11. Catatan Ringkas untuk Ditulis Tangan

### Dasar Cloud
- Cloud computing — akses on-demand ke shared pool sumber daya
- Cloud network — server di data center jarak jauh via internet
- Alasan pakai cloud — mudah, cepat, hemat biaya, scalable
- Private cloud — data center sendiri (tinggal sendiri)
- Public cloud — berbagi sumber daya (roommates), disiplin keamanan lebih ketat

### Cloud vs Tradisional
- CSP — tidak bisa cegah semua intrusi (malicious actors internal/eksternal)
- Server baseline image — bandingkan server dengan image acuan
- Unverified changes — tanda intrusi
- Pemisahan — aplikasi lama vs baru; fungsi internal vs front-end

### Pertimbangan Keamanan
- IAM — kelola identitas digital + otorisasi akses
- Role longgar — risiko akses tak berwenang
- Konfigurasi — presisi; migrasi rawan; misconfigured = sumber breach umum
- Attack surface — tiap layanan nambah risiko, perlu keamanan lebih
- Zero day — exploit tak dikenal; CSP patch hypervisor, pindah workload ke VM lain
- Visibility — flow logs, packet mirroring; pelanggan tak boleh monitor server CSP
- Third-party audits — CSP bayar; cek kerentanan dan compliance lapses
- Cloud berubah cepat — update CSP bisa ubah konfigurasi; sesuaikan proses TI

### Shared Responsibility
- CSP — physical data centers, hypervisors, host OS
- Pelanggan — aset, proses, aplikasi, konfigurasi di cloud
- Kesalahan umum — asumsi CSP urus semuanya
- Quiz — user, CSP, security team (bukan research department)

### Hardening
- Teknik — IAM, hypervisors, baselining, kriptografi, cryptographic erasure
- Hypervisor — abstraksi hardware host dari OS
- Type one — di atas hardware (VMware ESXi), dipakai CSP
- Type two — di atas software host (VirtualBox)
- VM escape — akses hypervisor utama, bisa kena host dan VM lain
- Baseline — titik referensi tetap
- Contoh baseline — batasi admin portal, password management, file encryption, threat detection SQL

### Kriptografi
- Fungsi — integrity dan confidentiality data
- Encryption — acak data jadi ciphertext, butuh kunci
- Enkripsi modern — rahasia di kunci, bukan algoritma
- Cryptographic erasure — hapus kunci enkripsi
- Crypto-shredding — hancurkan semua salinan kunci, data tak terbaca
- TPM — chip simpan password, sertifikat, kunci
- CloudHSM — simpan kunci + proses enkripsi/dekripsi
- Kunci pelanggan — tanggung jawab pelanggan; CSP terbatas jika kunci bocor/hilang
- FedRAMP — daftar CSP terverifikasi (federal contractors)

### Command
- Tidak ada command/switch di room ini
