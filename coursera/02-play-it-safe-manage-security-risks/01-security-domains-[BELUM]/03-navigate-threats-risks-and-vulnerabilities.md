# Memahami Aset, Ancaman, Risiko, dan Kerentanan dalam Keamanan Siber

Sebagai seorang analis keamanan tingkat pemula, salah satu dari banyak peran yang akan dijalani adalah menangani aset digital dan fisik milik organisasi. Perlu diingat, aset adalah segala sesuatu yang dianggap memiliki nilai bagi organisasi. Sepanjang masa operasinya, organisasi mengumpulkan berbagai jenis aset, mulai dari ruang kantor fisik, komputer, data pribadi pelanggan (PII), hingga kekayaan intelektual seperti paten atau data berhak cipta, dan masih banyak lagi. Sayangnya, organisasi beroperasi dalam lingkungan yang menghadirkan berbagai ancaman, risiko, dan kerentanan keamanan terhadap aset-aset tersebut.

## Ancaman (Threat)

Ancaman adalah setiap keadaan atau peristiwa yang dapat berdampak negatif terhadap aset. Salah satu contoh ancaman adalah serangan rekayasa sosial atau *social engineering*. Social engineering merupakan teknik manipulasi yang mengeksploitasi kesalahan manusia untuk mendapatkan informasi pribadi, akses, atau sesuatu yang berharga. Salah satu metode social engineering adalah phishing, yaitu tautan berbahaya dalam pesan email yang tampak seolah-olah berasal dari perusahaan atau orang yang sah. Phishing sendiri adalah teknik yang digunakan untuk memperoleh data sensitif, seperti nama pengguna, kata sandi, atau informasi perbankan.

## Risiko (Risk)

Risiko berbeda dari ancaman. Risiko adalah segala sesuatu yang dapat memengaruhi kerahasiaan (*confidentiality*), integritas (*integrity*), atau ketersediaan (*availability*) suatu aset. Risiko dapat dianggap sebagai kemungkinan terjadinya suatu ancaman. Contoh risiko bagi sebuah organisasi misalnya tidak adanya protokol pencadangan (*backup*) untuk memastikan informasi yang tersimpan dapat dipulihkan jika terjadi kecelakaan atau insiden keamanan.

Organisasi biasanya menilai risiko dalam tingkatan yang berbeda-beda: rendah, sedang, dan tinggi, tergantung pada kemungkinan ancaman dan nilai dari aset tersebut. Berikut penjelasan masing-masing tingkatan risiko:

- **Risiko rendah**: informasi yang jika terganggu tidak akan merusak reputasi atau operasional organisasi, dan tidak menimbulkan kerugian finansial. Contohnya adalah informasi publik seperti konten situs web atau data hasil riset yang telah dipublikasikan.
- **Risiko sedang**: informasi yang tidak tersedia untuk publik dan berpotensi menyebabkan kerugian finansial, reputasi, atau operasional organisasi. Contohnya adalah kebocoran laporan pendapatan kuartalan perusahaan sebelum waktunya, yang dapat memengaruhi nilai sahamnya.
- **Risiko tinggi**: informasi apa pun yang dilindungi oleh regulasi atau undang-undang, yang jika terganggu akan berdampak negatif secara serius terhadap finansial, operasional, atau reputasi organisasi. Ini termasuk kebocoran aset yang berisi SPII (informasi pribadi sensitif), PII (informasi identitas pribadi), atau kekayaan intelektual.

## Kerentanan (Vulnerability) dan Dampaknya

Kerentanan adalah kelemahan yang dapat dieksploitasi oleh sebuah ancaman. Penting dicatat bahwa baik kerentanan maupun ancaman harus sama-sama ada agar suatu risiko dapat terjadi. Beberapa contoh kerentanan meliputi firewall, software, atau aplikasi yang sudah usang; kata sandi yang lemah; serta data rahasia yang tidak terlindungi. Selain faktor teknis, manusia juga bisa menjadi kerentanan. Tindakan manusia dapat memberikan dampak signifikan terhadap jaringan internal organisasi, baik itu klien, vendor eksternal, maupun karyawan — menjaga keamanan harus menjadi upaya bersama semua pihak.

## Peran Analis dalam Mitigasi Risiko dan Kerentanan

Analis tingkat pemula perlu mendidik dan memberdayakan orang-orang di sekitarnya agar lebih sadar akan keamanan. Sebagai contoh, memberikan edukasi tentang cara mengenali email phishing adalah langkah awal yang baik. Penggunaan kartu akses untuk memberikan izin masuk karyawan ke ruang fisik sekaligus membatasi akses pengunjung dari luar juga merupakan langkah keamanan yang baik.

Organisasi harus terus meningkatkan upayanya dalam mengidentifikasi dan memitigasi kerentanan guna meminimalkan ancaman dan risiko. Analis tingkat pemula dapat mendukung tujuan ini dengan cara mendorong karyawan untuk melaporkan aktivitas mencurigakan, serta secara aktif memantau dan mendokumentasikan akses karyawan terhadap aset-aset penting.

---

**📌 Poin Kunci dari Video Ini:**
- Aset adalah segala hal yang bernilai bagi organisasi — mencakup ruang fisik, perangkat komputer, data PII pelanggan, hingga kekayaan intelektual.
- Ancaman adalah kejadian atau keadaan yang berpotensi merugikan aset, contohnya social engineering seperti phishing.
- Risiko adalah kemungkinan terjadinya ancaman yang berdampak pada confidentiality, integrity, atau availability aset, dan dinilai dalam tiga tingkat: rendah, sedang, tinggi.
- Kerentanan adalah kelemahan yang bisa dieksploitasi oleh ancaman; risiko hanya muncul jika ancaman dan kerentanan sama-sama ada.
- Manusia (karyawan, vendor, klien) juga dapat menjadi sumber kerentanan, sehingga edukasi keamanan menjadi bagian penting dari pekerjaan analis.
- Analis tingkat pemula berperan dalam edukasi phishing, penerapan kontrol akses fisik, serta pemantauan dan dokumentasi akses ke aset penting.

# Ransomware dan Dampak Threats, Risks, serta Vulnerabilities terhadap Organisasi

## Apa Itu Ransomware?

Ransomware adalah salah satu jenis malware yang tergolong mahal dan merugikan bagi organisasi. Ransomware merupakan serangan berbahaya di mana pelaku ancaman (threat actor) mengenkripsi data milik organisasi, kemudian menuntut pembayaran agar akses terhadap data tersebut dapat dipulihkan kembali. Begitu ransomware berhasil disebarkan oleh penyerang, ia dapat membekukan sistem jaringan, membuat perangkat tidak dapat digunakan, serta mengenkripsi atau mengunci data rahasia sehingga perangkat menjadi tidak dapat diakses sama sekali.

Setelah itu, pelaku ancaman akan meminta tebusan (ransom) sebelum memberikan kunci dekripsi (decryption key) yang memungkinkan organisasi kembali beroperasi secara normal.

> *Bayangkan decryption key seperti sebuah kata sandi (password) yang diberikan untuk mendapatkan kembali akses ke data Anda.*

Perlu dicatat, ketika negosiasi tebusan berlangsung atau ketika data hasil curian bocor dan disebarkan oleh pelaku ancaman, peristiwa-peristiwa ini seringkali terjadi melalui dark web.

## Tiga Lapisan Web: Surface Web, Deep Web, dan Dark Web

Banyak orang menggunakan mesin pencari (search engine) untuk mengakses akun media sosial atau berbelanja daring, namun sebenarnya itu hanyalah sebagian kecil dari apa yang disebut "web" secara keseluruhan. Web sesungguhnya adalah sebuah jaringan konten daring yang saling terhubung (interlinked network), yang terdiri dari tiga lapisan, yaitu surface web, deep web, dan dark web.

- **Surface web** — lapisan yang paling umum digunakan oleh kebanyakan orang. Lapisan ini berisi konten yang dapat diakses menggunakan web browser biasa.
- **Deep web** — lapisan yang umumnya membutuhkan otorisasi untuk dapat diakses. Contohnya adalah intranet milik sebuah organisasi, yang hanya dapat diakses oleh karyawan atau pihak-pihak lain yang telah diberikan izin akses.
- **Dark web** — lapisan yang hanya dapat diakses dengan menggunakan perangkat lunak (software) khusus.

Dark web umumnya memiliki konotasi negatif karena menjadi lapisan web pilihan bagi para pelaku kriminal, disebabkan oleh sifat kerahasiaan (secrecy) yang ditawarkannya.

## Tiga Dampak Utama dari Threats, Risks, dan Vulnerabilities

Setelah memahami ransomware dan lapisan-lapisan web, penting untuk membahas tiga dampak utama yang ditimbulkan oleh threats (ancaman), risks (risiko), dan vulnerabilities (kerentanan) terhadap operasional organisasi.

### 1. Dampak Finansial

Ketika aset milik organisasi disusupi oleh sebuah serangan, seperti penggunaan malware, konsekuensi finansial yang ditimbulkan bisa sangat signifikan karena berbagai alasan. Konsekuensi ini dapat mencakup terganggunya proses produksi dan layanan, biaya yang harus dikeluarkan untuk memperbaiki masalah tersebut, serta denda yang dikenakan apabila aset yang disusupi ternyata disebabkan oleh ketidakpatuhan terhadap hukum dan regulasi yang berlaku.

### 2. Pencurian Identitas (Identity Theft)

Organisasi harus memutuskan apakah akan menyimpan data pribadi milik pelanggan, karyawan, dan vendor eksternal, serta berapa lama data tersebut akan disimpan. Menyimpan segala jenis data sensitif pada dasarnya membawa risiko tersendiri bagi organisasi.

Data sensitif ini dapat mencakup personally identifiable information (PII), yaitu informasi yang dapat mengidentifikasi identitas seseorang secara pribadi. Data semacam ini berpotensi dijual atau dibocorkan melalui dark web. Hal ini terjadi karena dark web menawarkan rasa kerahasiaan, sehingga pelaku ancaman berpotensi menjual data curian tersebut tanpa harus menghadapi konsekuensi hukum.

### 3. Kerusakan Reputasi (Reputational Damage)

Basis pelanggan yang solid mendukung tercapainya misi, visi, dan tujuan finansial sebuah organisasi. Sebuah vulnerability yang berhasil dieksploitasi dapat menyebabkan pelanggan beralih mencari hubungan bisnis baru dengan kompetitor, atau memicu pemberitaan negatif yang menyebabkan kerusakan permanen terhadap reputasi organisasi.

Kehilangan data pelanggan tidak hanya berdampak pada reputasi dan finansial organisasi, tetapi juga berpotensi menimbulkan penalti hukum dan denda.

## Pentingnya Kesiapan Keamanan

Organisasi sangat dianjurkan untuk mengambil langkah-langkah keamanan yang tepat serta mengikuti protokol tertentu guna mencegah dampak signifikan dari threats, risks, dan vulnerabilities. Dengan memanfaatkan seluruh perangkat (tools) yang tersedia dalam toolkit mereka, tim keamanan akan lebih siap dalam menangani insiden, seperti misalnya serangan ransomware.

---

**📌 Poin Kunci dari Video Ini:**

- Ransomware adalah serangan yang mengenkripsi data organisasi, lalu menuntut tebusan sebelum decryption key diberikan untuk memulihkan akses.
- Web terdiri dari tiga lapisan: surface web (dapat diakses browser biasa), deep web (butuh otorisasi, contoh: intranet), dan dark web (butuh software khusus, sering dikaitkan dengan aktivitas ilegal).
- Negosiasi tebusan dan kebocoran data akibat ransomware sering terjadi melalui dark web karena sifat kerahasiaannya.
- Dampak finansial dari serangan siber meliputi terganggunya produksi/layanan, biaya perbaikan, dan denda akibat ketidakpatuhan regulasi.
- Penyimpanan data sensitif (PII) menimbulkan risiko identity theft karena data tersebut bisa dijual atau bocor di dark web.
- Eksploitasi vulnerability dapat merusak reputasi organisasi, membuat pelanggan beralih ke kompetitor, serta memicu penalti hukum dan denda.
- Kesiapan keamanan yang baik dan penggunaan seluruh tools yang tersedia membantu tim keamanan menangani insiden seperti serangan ransomware secara lebih efektif.

# Perjalanan Herbert Menuju Karier di Bidang Keamanan Siber

## Awal Ketertarikan pada Dunia Security

Herbert, seorang Security Engineer di Google, menceritakan bahwa ketertarikannya pada keamanan siber sudah tumbuh sejak masa SMA. Saat itu, sekolahnya membagikan laptop Dell berukuran besar kepada para siswa, namun laptop-laptop tersebut tidak memiliki sistem keamanan yang memadai. Banyak teman-temannya memanfaatkan celah ini dengan memasang versi bajakan (cracked version) dari video game seperti Halo di laptop sekolah tersebut. Dari situlah Herbert mulai belajar bagaimana caranya memanipulasi komputer agar bisa berjalan sesuai dengan apa yang ia inginkan — sebuah pengalaman yang tanpa disadari menjadi fondasi awal minatnya di bidang keamanan siber.

## Dari Kedai Pizza Menuju Google

Salah satu hal yang paling menarik dari perjalanan karier Herbert adalah titik awalnya yang jauh dari dunia teknologi. Sepuluh tahun sebelum menjadi Security Engineer di Google, ia bekerja di sebuah kedai pizza.

> *Sepuluh tahun lalu saya bekerja di kedai pizza. Sepuluh tahun kemudian, di sinilah saya, sebagai Security Engineer di Google.*

Herbert menekankan bahwa jika ia bercerita kepada dirinya yang berusia 16 tahun tentang pencapaian ini, ia sendiri mungkin tidak akan mempercayainya. Pesan yang ingin ia sampaikan adalah bahwa perjalanan karier semacam ini benar-benar mungkin dicapai oleh siapa saja, terlepas dari titik awal yang tampak jauh dari industri teknologi.

## Tanggung Jawab Harian Seorang Security Engineer

Secara umum, pekerjaan Herbert sehari-hari berkisar pada dua hal utama: menganalisis risiko keamanan dan menyediakan solusi atas risiko-risiko tersebut. Salah satu tugas yang paling umum dikerjakan oleh seorang cybersecurity analyst adalah menangani **exception request**, yaitu permintaan pengecualian akses. Dalam praktiknya, ini berarti menganalisis apakah seseorang benar-benar membutuhkan akses khusus terhadap suatu perangkat atau dokumen, dengan mempertimbangkan peran (role) orang tersebut atau proyek yang sedang ia kerjakan. Proses ini penting karena pemberian akses yang tidak sesuai dengan kebutuhan aktual seseorang dapat membuka celah keamanan yang tidak perlu.

## Ancaman Keamanan yang Sering Ditemui

### Misconfiguration dan Permintaan Akses Berlebih

Salah satu ancaman yang paling sering dihadapi timnya adalah **misconfiguration**, atau kesalahan konfigurasi sistem, termasuk di dalamnya kasus di mana seseorang meminta akses terhadap sesuatu yang sebenarnya tidak ia butuhkan. Herbert memberikan contoh kasus nyata yang baru-baru ini ia tangani: sebuah vendor yang bekerja sama dengan timnya mengubah **OAuth scope request** mereka. Artinya, vendor tersebut meminta izin (permission) yang lebih besar untuk mengakses layanan Google dibandingkan dengan apa yang mereka minta sebelumnya. Karena situasi seperti ini belum pernah mereka hadapi sebelumnya, tim Herbert sempat tidak yakin bagaimana cara menanganinya dengan tepat. Kasus ini masih berjalan hingga saat ini, dan mereka bekerja sama dengan tim-tim mitra lainnya untuk mengembangkan solusi yang sesuai.

### Sistem yang Sudah Usang (Outdated Systems)

Ancaman lain yang kerap muncul adalah sistem yang sudah usang, yaitu mesin atau perangkat yang belum mendapatkan patch atau pembaruan keamanan. Herbert menjelaskan bahwa meskipun isu ini terdengar seperti masalah IT semata, sebenarnya ini juga merupakan isu cybersecurity yang serius. Perangkat yang tidak diperbarui, ditambah dengan tidak adanya kebijakan device management yang memadai, dapat menjadi titik lemah yang berpotensi dieksploitasi.

## Pentingnya Kolaborasi Lintas Tim

Herbert menggarisbawahi bahwa bekerja dengan satu tim saja tidak cukup dalam pekerjaan ini — kolaborasi dengan banyak tim adalah bagian besar dari pekerjaannya sehari-hari. Untuk benar-benar menyelesaikan suatu pekerjaan, seseorang perlu berkomunikasi bukan hanya dengan timnya sendiri, tetapi juga dengan tim-tim lain di luar timnya. Hal ini terlihat jelas dari contoh kasus OAuth scope di atas, di mana penyelesaiannya membutuhkan kerja sama dengan tim mitra (partner teams) untuk merumuskan solusi bersama.

---

**📌 Poin Kunci dari Video Ini:**
- Ketertarikan pada cybersecurity bisa tumbuh dari pengalaman sehari-hari yang tidak terduga, seperti memanipulasi perangkat sekolah yang minim sistem keamanan.
- Karier di bidang cybersecurity terbuka bagi siapa saja, tidak peduli seberapa jauh titik awalnya dari dunia teknologi.
- Tugas utama Security Engineer mencakup analisis risiko keamanan dan penanganan exception request untuk akses perangkat atau dokumen.
- Misconfiguration, termasuk perubahan OAuth scope request yang meminta permission lebih besar dari sebelumnya, merupakan ancaman umum yang harus dianalisis secara hati-hati.
- Sistem yang outdated dan tidak adanya device management yang baik adalah risiko cybersecurity, bukan sekadar masalah IT.
- Kolaborasi lintas tim sangat penting karena banyak masalah keamanan tidak bisa diselesaikan hanya oleh satu tim saja.

# Kerangka Kerja Manajemen Risiko NIST (RMF)

## Pengantar: Mengapa Framework Risiko Ini Penting

Seperti yang mungkin sudah dipelajari sebelumnya dalam program ini, National Institute of Standards and Technology (NIST) menyediakan banyak framework yang digunakan oleh para profesional keamanan untuk mengelola risiko, ancaman, dan kerentanan. Materi ini akan berfokus pada salah satu framework NIST, yaitu Risk Management Framework atau RMF. Sebagai analis tingkat pemula, mungkin tidak semua tahapan dalam framework ini akan langsung dikerjakan, namun tetap penting untuk memahami keseluruhan kerangka kerjanya. Pemahaman dasar yang kuat tentang cara memitigasi dan mengelola risiko dapat menjadi nilai lebih yang membedakan diri dari kandidat lain saat memulai pencarian kerja di bidang keamanan.

RMF terdiri dari tujuh tahapan, yaitu: *prepare* (persiapan), *categorize* (kategorisasi), *select* (pemilihan), *implement* (implementasi), *assess* (penilaian), *authorize* (otorisasi), dan *monitor* (pemantauan).

## Tahap 1: Prepare (Persiapan)

Prepare merujuk pada aktivitas-aktivitas yang diperlukan untuk mengelola risiko keamanan dan privasi sebelum sebuah breach (pelanggaran keamanan) terjadi. Sebagai analis tingkat pemula, tahap ini biasanya digunakan untuk memantau risiko yang ada serta mengidentifikasi kontrol-kontrol yang dapat digunakan untuk mengurangi risiko tersebut.

## Tahap 2: Categorize (Kategorisasi)

Categorize digunakan untuk mengembangkan proses dan tugas manajemen risiko. Para profesional keamanan menggunakan proses tersebut dan mengembangkan tugas dengan mempertimbangkan bagaimana confidentiality, integrity, dan availability (kerahasiaan, integritas, dan ketersediaan) dari sistem serta informasi dapat terdampak oleh risiko. Sebagai analis tingkat pemula, kemampuan yang dibutuhkan adalah memahami cara mengikuti proses-proses yang sudah ditetapkan oleh organisasi untuk mengurangi risiko terhadap aset-aset kritis, seperti informasi pribadi pelanggan.

## Tahap 3: Select (Pemilihan)

Select berarti memilih, menyesuaikan (customize), dan mendokumentasikan kontrol-kontrol yang melindungi organisasi. Salah satu contoh dari tahap select ini adalah menjaga sebuah playbook agar tetap up-to-date, atau membantu mengelola dokumentasi lain yang memungkinkan diri sendiri dan tim untuk menangani masalah dengan lebih efisien.

## Tahap 4: Implement (Implementasi)

Tahap ini melibatkan implementasi rencana keamanan dan privasi bagi organisasi. Memiliki rencana yang baik sangat penting untuk meminimalkan dampak dari risiko keamanan yang sedang berlangsung. Sebagai contoh, jika terlihat pola di mana karyawan terus-menerus membutuhkan reset password, maka mengimplementasikan perubahan pada persyaratan password dapat membantu menyelesaikan masalah tersebut.

## Tahap 5: Assess (Penilaian)

Assess berarti menentukan apakah kontrol-kontrol yang sudah ditetapkan telah diimplementasikan dengan benar. Sebuah organisasi selalu ingin beroperasi seefisien mungkin, sehingga penting untuk meluangkan waktu menganalisis apakah protokol, prosedur, dan kontrol yang telah diterapkan sudah memenuhi kebutuhan organisasi. Selama tahap ini, analis mengidentifikasi potensi kelemahan dan menentukan apakah tools, prosedur, kontrol, dan protokol organisasi perlu diubah untuk lebih baik dalam mengelola potensi risiko.

## Tahap 6: Authorize (Otorisasi)

Authorize berarti bertanggung jawab atas risiko keamanan dan privasi yang mungkin ada dalam organisasi. Sebagai analis, tahap otorisasi ini bisa melibatkan pembuatan laporan, pengembangan rencana tindakan (plans of action), serta penetapan milestone proyek yang selaras dengan tujuan keamanan organisasi.

## Tahap 7: Monitor (Pemantauan)

Monitor berarti memantau bagaimana sistem beroperasi. Menilai dan memelihara operasional teknis adalah tugas yang dikerjakan analis setiap hari. Bagian dari menjaga tingkat risiko tetap rendah bagi organisasi adalah dengan mengetahui bagaimana sistem yang berjalan saat ini mendukung tujuan keamanan organisasi. Jika sistem yang ada tidak memenuhi tujuan tersebut, maka perubahan mungkin diperlukan. Meskipun menetapkan prosedur-prosedur ini mungkin bukan tugas seorang analis pemula, tetap diperlukan untuk memastikan bahwa prosedur tersebut bekerja sebagaimana mestinya, sehingga risiko terhadap organisasi itu sendiri, maupun terhadap orang-orang yang dilayaninya, dapat diminimalkan.

---

**📌 Poin Kunci dari Video Ini:**
- RMF (Risk Management Framework) adalah salah satu framework dari NIST yang terdiri dari tujuh tahapan berurutan: prepare, categorize, select, implement, assess, authorize, dan monitor.
- Tahap **prepare** berfokus pada aktivitas sebelum breach terjadi, sedangkan **categorize** menekankan dampak risiko pada confidentiality, integrity, dan availability (CIA).
- Tahap **select** berkaitan dengan pemilihan dan dokumentasi kontrol (misalnya playbook), sementara **implement** adalah penerapan rencana keamanan secara nyata.
- Tahap **assess** menilai apakah kontrol yang diterapkan sudah efektif, dan **authorize** berkaitan dengan akuntabilitas melalui laporan dan rencana tindakan.
- Tahap **monitor** adalah proses berkelanjutan (harian) untuk memastikan sistem tetap mendukung tujuan keamanan organisasi.
- Sebagai analis tingkat pemula, tidak semua tahapan RMF akan dikerjakan langsung, namun memahami keseluruhan kerangka kerja ini penting sebagai nilai tambah dalam mencari pekerjaan di bidang keamanan.

## Materi: Mengelola Ancaman, Risiko, dan Kerentanan Umum

Sebelumnya kamu sudah belajar bahwa keamanan (security) itu intinya melindungi organisasi dan orang-orang dari ancaman (threats), risiko (risks), dan kerentanan (vulnerabilities). Dengan memahami lanskap ancaman yang ada sekarang, organisasi jadi bisa membuat kebijakan dan proses untuk mencegah serta mengurangi dampak masalah keamanan semacam ini. Di materi ini kamu akan belajar lebih dalam soal cara mengelola risiko, serta beberapa taktik dan teknik umum yang dipakai pelaku ancaman (threat actor), supaya kamu lebih siap melindungi organisasi dan orang-orang yang mereka layani saat nanti terjun ke dunia keamanan siber.

### Manajemen Risiko

Salah satu tujuan utama organisasi adalah melindungi aset. **Aset** adalah segala sesuatu yang dianggap punya nilai bagi organisasi. Aset bisa berupa aset digital maupun aset fisik.

Contoh aset digital termasuk data pribadi karyawan, klien, atau vendor, seperti:

- Nomor Jaminan Sosial (SSN) atau nomor identifikasi nasional unik yang diberikan ke individu
- Tanggal lahir
- Nomor rekening bank
- Alamat surat-menyurat

Contoh aset fisik termasuk:

- Kios pembayaran (payment kiosk)
- Server
- Komputer desktop
- Ruang kantor

> Bayangkan organisasi itu seperti rumah. Aset digital itu ibarat dokumen penting dan uang tunai yang disimpan di dalam brankas, sedangkan aset fisik itu ibarat rumah, perabotan, dan kendaraan itu sendiri. Keduanya sama-sama perlu dijaga, cuma caranya beda.

Beberapa strategi umum untuk mengelola risiko meliputi:

- **Acceptance (Penerimaan)**: Menerima suatu risiko demi menjaga kelangsungan bisnis tetap berjalan tanpa gangguan
- **Avoidance (Penghindaran)**: Membuat rencana untuk menghindari risiko tersebut sepenuhnya
- **Transference (Pengalihan)**: Mengalihkan risiko ke pihak ketiga untuk dikelola
- **Mitigation (Mitigasi)**: Mengurangi dampak dari suatu risiko yang sudah diketahui

> Ini mirip seperti kamu punya motor. Acceptance itu seperti "ya sudah, risiko motor lecet dijalan itu wajar, saya terima saja". Avoidance itu seperti "saya tidak akan pakai motor sama sekali biar tidak lecet". Transference itu seperti membeli asuransi motor supaya kalau rusak, pihak asuransi yang menanggung. Mitigation itu seperti memasang pelindung bodi supaya kalau tergores, kerusakannya tidak parah.

Selain itu, organisasi juga menerapkan proses manajemen risiko berdasarkan kerangka kerja (framework) yang sudah diakui secara luas untuk membantu melindungi aset digital dan fisik dari berbagai ancaman, risiko, dan kerentanan. Contoh framework yang umum dipakai di industri keamanan siber adalah National Institute of Standards and Technology Risk Management Framework (NIST RMF) dan Health Information Trust Alliance (HITRUST).

Berikut adalah beberapa jenis ancaman, risiko, dan kerentanan umum yang akan kamu bantu kelola sebagai profesional keamanan.

### Ancaman, Risiko, dan Kerentanan Paling Umum Saat Ini

**Ancaman (Threats)**

**Ancaman** adalah setiap keadaan atau kejadian yang berpotensi berdampak negatif terhadap aset. Sebagai analis keamanan tingkat pemula, tugasmu adalah membantu melindungi aset organisasi dari ancaman yang berasal dari dalam maupun luar. Karena itu, memahami jenis-jenis ancaman umum penting untuk pekerjaan sehari-hari seorang analis. Sebagai pengingat, ancaman umum meliputi:

- **Insider threats (Ancaman dari dalam)**: Staf atau vendor menyalahgunakan akses resmi yang mereka miliki untuk memperoleh data yang bisa merugikan organisasi.
- **Advanced persistent threats/APT (Ancaman persisten tingkat lanjut)**: Pelaku ancaman berhasil mempertahankan akses tanpa izin ke suatu sistem dalam jangka waktu yang lama.

> Insider threat itu seperti pegawai toko yang justru diam-diam mencuri barang dari gudang karena dia sendiri yang pegang kuncinya. Sedangkan APT itu seperti pencuri yang berhasil masuk ke rumahmu, tapi bukan langsung mengambil barang lalu kabur — dia malah bersembunyi diam-diam di loteng selama berbulan-bulan sambil terus mengintai dan mengambil barang sedikit demi sedikit tanpa ketahuan.

**Risiko (Risks)**

**Risiko** adalah segala sesuatu yang bisa berdampak pada kerahasiaan (confidentiality), integritas (integrity), atau ketersediaan (availability) suatu aset. Rumus dasar untuk menentukan tingkat risiko adalah: risiko setara dengan kemungkinan (likelihood) suatu ancaman terjadi. Salah satu cara memahaminya, risiko itu seperti kemungkinan kamu terlambat kerja, dan ancamannya adalah hal-hal seperti macet, kecelakaan, atau ban kempes.

Ada beberapa faktor berbeda yang bisa memengaruhi kemungkinan suatu risiko terjadi pada aset organisasi, di antaranya:

- **External risk (Risiko eksternal)**: Segala sesuatu di luar organisasi yang berpotensi merugikan aset organisasi, contohnya pelaku ancaman yang berusaha mendapatkan akses ke informasi pribadi.
- **Internal risk (Risiko internal)**: Karyawan (baik yang masih aktif maupun mantan), vendor, atau mitra terpercaya yang menimbulkan risiko keamanan.
- **Legacy systems (Sistem lawas)**: Sistem lama yang mungkin tidak lagi terpantau atau tidak diperbarui, tapi tetap bisa memengaruhi aset, seperti workstation atau sistem mainframe lama. Misalnya, sebuah organisasi mungkin punya mesin penjual otomatis (vending machine) lama yang menerima pembayaran kartu kredit, atau workstation yang masih terhubung ke sistem akuntansi lawas.
- **Multiparty risk (Risiko multipihak)**: Bekerja sama dengan vendor pihak ketiga bisa memberi mereka akses ke kekayaan intelektual, seperti rahasia dagang, desain software, dan hasil temuan/inovasi.
- **Software compliance/licensing (Kepatuhan/lisensi software)**: Software yang tidak diperbarui atau tidak sesuai standar kepatuhan, atau patch yang tidak dipasang tepat waktu.

> Legacy system itu seperti kunci pintu belakang rumah tua yang sudah jarang dipakai dan hampir dilupakan — kamu fokus mengunci pintu depan dengan gembok super canggih, padahal pintu belakang yang usang itu justru jadi celah paling mudah untuk dibobol maling.

Ada banyak sumber, seperti NIST, yang menyediakan daftar risiko keamanan siber. Selain itu, Open Web Application Security Project (OWASP) menerbitkan dokumen standar kesadaran tentang 10 risiko keamanan paling kritis untuk aplikasi web, yang diperbarui secara berkala.

**Catatan:** Daftar jenis serangan umum dari OWASP memuat tiga risiko baru untuk periode 2017 sampai 2021, yaitu: insecure design (desain yang tidak aman), software and data integrity failures (kegagalan integritas software dan data), dan server-side request forgery. Pembaruan ini menegaskan bahwa keamanan adalah bidang yang terus berkembang. Hal ini juga menunjukkan pentingnya selalu mengikuti perkembangan taktik dan teknik terbaru dari pelaku ancaman, supaya kamu lebih siap mengelola jenis-jenis risiko ini.

Gambar yang ditampilkan menunjukkan perbandingan daftar OWASP Top 10 tahun 2017 dengan yang terbaru, di mana ada tiga kategori baru yang ditandai dengan label "New":

Daftar OWASP 2017 (di kiri):
- A01:2017 - Injection
- A02:2017 - Broken Authentication
- A03:2017 - Sensitive Data Exposure
- A04:2017 - XML External Entities (XXE)
- A05:2017 - Broken Access Control
- A06:2017 - Security Misconfiguration
- A07:2017 - Cross-Site Scripting (XSS)
- A08:2017 - Insecure Deserialization
- A09:2017 - Using Components with Known Vulnerabilities
- A10:2017 - Insufficient Logging & Monitoring

Ketiganya dipetakan (dengan panah) ke tiga kategori baru di sisi kanan yang bertanda "New" — sesuai dengan tiga risiko baru yang disebutkan tadi (insecure design, software and data integrity failures, dan server-side request forgery).

> Ini mirip kayak daftar lagu terpopuler yang di-update tiap beberapa tahun. Beberapa lagu lama digabung ulang atau dianggap sudah termasuk kategori lain, sementara ada genre-genre baru yang bermunculan dan masuk tangga lagu untuk pertama kalinya.

**Kerentanan (Vulnerabilities)**

**Kerentanan** adalah kelemahan yang bisa dimanfaatkan (dieksploitasi) oleh sebuah ancaman. Karena itu, organisasi perlu rutin memeriksa kerentanan dalam sistem mereka. Beberapa contoh kerentanan meliputi:

- **ProxyLogon**: Kerentanan pra-autentikasi (pre-authenticated) yang memengaruhi server Microsoft Exchange. Artinya, pelaku ancaman bisa menyelesaikan proses autentikasi pengguna untuk menjalankan kode berbahaya dari lokasi jarak jauh.
- **ZeroLogon**: Kerentanan pada protokol autentikasi Netlogon milik Microsoft. Netlogon sendiri adalah layanan yang berfungsi memverifikasi identitas seseorang sebelum mengizinkan akses ke lokasi sebuah website.
- **Log4Shell**: Memungkinkan penyerang menjalankan kode Java di komputer orang lain atau membocorkan informasi sensitif. Caranya, dengan memungkinkan penyerang jarak jauh mengambil alih kendali perangkat yang terhubung ke internet dan menjalankan kode berbahaya.
- **PetitPotam**: Memengaruhi Windows New Technology LAN Manager (NTLM). Ini adalah teknik pencurian yang memungkinkan penyerang berbasis LAN untuk memicu (initiate) permintaan autentikasi.
- **Security logging and monitoring failures (Kegagalan pencatatan log dan pemantauan keamanan)**: Kemampuan pencatatan log dan pemantauan yang tidak memadai, sehingga penyerang bisa mengeksploitasi celah tanpa diketahui oleh organisasi.
- **Server-side request forgery**: Memungkinkan penyerang memanipulasi aplikasi sisi server untuk mengakses dan memperbarui sumber daya backend. Ini juga bisa memungkinkan pelaku ancaman mencuri data.

> Kerentanan itu seperti jendela rumah yang lupa dikunci. Ancamannya adalah si pencuri, tapi jendela yang tidak terkunci itulah kerentanannya — celah yang membuat si pencuri bisa masuk.

Sebagai analis keamanan tingkat pemula, kamu mungkin akan bekerja di bidang manajemen kerentanan (vulnerability management), yaitu memantau sistem untuk mengidentifikasi dan mengurangi kerentanan. Meskipun patch dan pembaruan sudah tersedia, jika tidak diterapkan, intrusi (serangan masuk) tetap bisa terjadi. Karena itu, pemantauan yang terus-menerus itu penting. Semakin cepat organisasi mengidentifikasi kerentanan dan menanganinya lewat patching atau update sistem, semakin cepat pula kerentanan itu bisa diatasi, sehingga mengurangi risiko organisasi terpapar celah tersebut.

Untuk mempelajari lebih lanjut soal kerentanan-kerentanan yang dijelaskan di bagian ini serta kerentanan lainnya, kamu bisa mengeksplorasi NIST National Vulnerability Database dan CISA Known Exploited Vulnerabilities Catalog.

### Kesimpulan Penting (Key Takeaways)

Di materi ini kamu sudah belajar tentang beberapa strategi dan framework manajemen risiko yang bisa digunakan untuk membuat kebijakan dan proses di seluruh organisasi guna mengurangi dampak ancaman, risiko, dan kerentanan. Kamu juga sudah belajar soal ancaman, risiko, dan kerentanan paling umum saat ini dalam operasi bisnis. Memahami konsep-konsep ini bisa membuatmu lebih siap tidak hanya untuk melindungi, tapi juga mengurangi dampak dari jenis-jenis masalah terkait keamanan yang bisa merugikan organisasi maupun orang-orang di dalamnya.

### Sumber untuk Informasi Lebih Lanjut

Untuk belajar lebih lanjut, kamu bisa mengeksplorasi situs-situs berikut:

- OWASP Top Ten
- NIST RMF

---

**📋 Daftar Screenshot — Task Ini:**

Tidak ada langkah praktek (hands-on) yang perlu dilakukan di materi ini — ini murni bacaan (reading) tentang konsep manajemen risiko, jadi tidak ada instruksi "Lakukan ini" maupun screenshot yang perlu diambil.

## Task: Kuis Keamanan Siber — Vulnerability, Risk, dan RMF

Nilai kamu di kuis ini **100%** (skor terakhir 100%, skor tertinggi 100%, syarat lulus minimal 75%). Berikut penjelasan lengkap tiap soalnya:

**Soal 1: Apa itu vulnerability (kerentanan)?**

Jawaban yang benar adalah "kelemahan yang bisa dieksploitasi oleh sebuah threat (ancaman)". Jadi vulnerability itu bukan ancamannya sendiri, bukan juga dampaknya, melainkan celah atau titik lemah pada sistem/aset yang berpotensi dimanfaatkan oleh pihak yang berniat jahat.

> Bayangkan vulnerability seperti pintu rumah yang kuncinya rusak. Pintu itu sendiri bukan pencurinya, tapi dia jadi celah yang bisa dimanfaatkan pencuri (threat) untuk masuk.

**Soal 2: Isian rumpang — Informasi yang dilindungi oleh regulasi atau hukum**

Jawabannya adalah **high-risk asset**. Jika informasi jenis ini dibocorkan atau disalahgunakan, kemungkinan besar akan berdampak negatif yang parah terhadap keuangan, operasional, atau reputasi organisasi.

> Ini seperti dokumen rahasia negara — kalau bocor, dampaknya bukan cuma malu-maluin, tapi bisa kena sanksi hukum, rugi besar, sampai reputasi hancur.

**Soal 3: Apa saja dampak utama dari threats, risks, dan vulnerabilities? (Pilih tiga jawaban)**

Tiga jawaban yang benar adalah:
- Kerugian finansial (financial damage)
- Kerusakan reputasi (damage to reputation)
- Pencurian identitas (identity theft)

Sedangkan "employee retention" (retensi karyawan) bukan termasuk dampak utama dari ketiga hal tersebut.

> Anggap saja seperti efek domino: satu insiden keamanan bisa memicu kerugian uang, nama baik perusahaan tercoreng, dan data pribadi orang bisa dicuri lalu disalahgunakan — semuanya saling berkaitan.

**Soal 4: Isian rumpang — Tahapan Risk Management Framework (RMF)**

Urutan lengkap tahapan RMF adalah: **prepare (persiapan) → categorize (kategorisasi) → select (pemilihan) → implement (implementasi) → assess (penilaian) → authorize (otorisasi) → monitor (pemantauan)**.

Jadi jawaban yang tepat untuk mengisi bagian kosong adalah "categorize". Pada tahap categorize ini, para profesional keamanan menyusun proses dan tugas terkait manajemen risiko.

> RMF ini mirip seperti tahapan membangun rumah: kamu harus siap-siap dulu (prepare), kelompokkan kebutuhan ruangan (categorize), pilih bahan bangunan (select), baru bangun (implement), cek kualitasnya (assess), baru dapat izin huni (authorize), dan terus dirawat (monitor).

Catatan: karena ini adalah kuis pilihan ganda yang sudah otomatis dinilai sistem (bukan lab praktik seperti terminal atau Burp Suite), tidak ada langkah "Lakukan ini" yang perlu dijalankan — semua soal sudah terjawab benar dengan skor sempurna.