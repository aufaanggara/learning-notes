# 📚 RESUME MATERI — Attacking & Defending the Human Element in SOC
**Tanggal:** 8 Agustus 2026

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📌 1. Konteks & Tujuan Room

Room ini membahas mengapa **manusia** dianggap sebagai elemen paling lemah dalam keamanan siber, bagaimana penyerang mengeksploitasi kelemahan ini, dan bagaimana peran seorang **SOC (Security Operations Center) analyst** dalam mendeteksi serta mencegah serangan tersebut.

Ide utamanya: menyerang sistem secara teknis (bypass firewall, eksploitasi vulnerability) itu memakan waktu dan usaha besar. Menipu manusia agar memberi akses secara sukarela jauh lebih efisien bagi penyerang. Karena itu, manusia sering disebut sebagai "gatekeeper" yang naif dalam sebuah jaringan — mereka memegang kunci akses tapi bisa ditipu untuk membukanya sendiri.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📌 2. Mengapa Manusia Ditarget

Manusia ditarget karena **akses** yang mereka miliki — ke website, mailbox, atau database perusahaan. Sebagian threat actor mengincar akses spesifik (targeted), sebagian lain membobol akun sebanyak mungkin lalu memutuskan cara memanfaatkannya belakangan (opportunistic).

Pola umum yang jadi tujuan penyerang saat membobol manusia:

- **Breach akun email/cloud milik HR** → tujuan akhirnya mencuri dan menjual database karyawan.
- **Menipu target bernilai tinggi menjalankan malware** → tujuan akhirnya membajak sesi perbankan online korban.
- **Breach akun VPN administrator IT** → tujuan akhirnya mengakses jantung jaringan korporat (lateral movement ke seluruh sistem).
- **Menipu pekerja agar membocorkan informasi sensitif** → tujuan akhirnya mempermudah serangan lanjutan (recon untuk serangan berikutnya).

Pola ini menunjukkan bahwa serangan terhadap manusia jarang berhenti di satu titik — kompromi awal biasanya jadi **entry point** untuk serangan yang lebih besar.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📌 3. Konsep Inti: Social Engineering

**Social engineering** adalah taktik manipulasi psikologis yang membuat korban membantu penyerang, baik secara sadar maupun tidak — bukan mengeksploitasi celah teknis, melainkan celah pada cara berpikir manusia.

Dua syarat agar taktik ini berhasil:

- **Trustworthy** — penyerang harus tampak legitimate/sah, sehingga korban percaya bahwa sumbernya asli (misalnya menyamar sebagai bank, IT support, atau atasan).
- **Emotional** — serangan harus memicu emosi kuat seperti *urgency* (rasa mendesak), *fear* (ketakutan), atau *curiosity* (rasa penasaran) — emosi ini membuat korban bertindak cepat tanpa berpikir kritis.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📌 4. Klasifikasi Teknik Serangan terhadap Manusia

### a. Phishing Attacks

Teknik paling umum dalam social engineering, dengan estimasi **3,4 miliar email berbahaya** dikirim setiap harinya. Mekanismenya: korban menerima pesan (biasanya email) yang tampak resmi, berisi ajakan untuk klik link atau membuka lampiran.

Ciri-ciri yang harus dikenali dari contoh kasus di room:

- **Fake sender email** — alamat pengirim yang mirip tapi bukan domain asli (contoh: `alert@onepassword.top` menyamar sebagai 1Password).
- **URL menuju fake login portal** — link yang mengarah ke halaman login palsu untuk mencuri kredensial saat korban login.
- **Malware attachment** — lampiran berbahaya (contoh: file ZIP terproteksi password bernama `TAX PENALTY.zip`) yang sengaja dienkripsi agar lolos dari scanning otomatis (misalnya Gmail scan), karena scanner tidak bisa membuka arsip terpassword.
- Skema emosional yang dipakai biasanya berupa ancaman finansial/hukum ("denda pajak", "tindakan legal") untuk memicu urgency.

### b. Malware Downloads

Korban tanpa sadar menginstal malware saat mencari dan mengunduh aplikasi dari sumber tidak resmi. Threat actor memakai teknik-teknik canggih untuk membuat unduhan ini terlihat sah:

- **Fake website (brand impersonation)** — meniru tampilan situs resmi (contoh: halaman palsu "Update Firefox") untuk mendorong korban mengunduh file berbahaya yang disamarkan sebagai update software (**data stealer**).
- **Fake CAPTCHA verification** — halaman verifikasi palsu yang meminta korban melakukan langkah-langkah manual.
- **ClickFix technique** — teknik di mana korban dituntun menekan Windows Key + R (membuka jendela Run), lalu diminta paste (Ctrl+V) sesuatu yang sebenarnya sudah disisipkan diam-diam ke clipboard korban saat mengunjungi halaman tersebut, kemudian menekan Enter. Korban sendiri yang tanpa sadar mengeksekusi command berbahaya, karena mereka yang "menekan tombol".
- **SEO poisoning** — memanipulasi hasil pencarian mesin pencari agar situs berbahaya muncul di posisi atas untuk kata kunci populer.
- **Malicious QR codes** — kode QR yang mengarahkan korban ke situs/unduhan berbahaya, sering dipakai karena URL asli tersembunyi di balik kode QR sehingga sulit diverifikasi sebelum diklik.

### c. Deepfakes & Impersonation

- **Deepfake** — video/audio hasil AI-generated yang menyamar sebagai orang yang dikenal korban (keluarga, kolega, mitra bisnis). Contoh kasus nyata di room: pekerja finance tertipu deepfake video call "bos"-nya dan mentransfer $25 juta untuk "urgent business deal".
- **Impersonation** — penyamaran identitas tanpa perlu teknologi deepfake, cukup lewat telepon atau chat. Contoh nyata: serangan ransomware yang dimulai dari telepon penyerang menyamar sebagai IT department, meminta korban menyerahkan akses akun demi "system repair" mendesak.

### d. Metode Lain (disebut, tidak dibahas mendalam)

- **USB drop campaigns** — meninggalkan USB berisi malware di tempat yang mudah ditemukan karyawan, berharap mereka penasaran dan mencolokkannya ke komputer kantor.
- **Physical attacks** — serangan yang melibatkan akses fisik langsung ke lokasi/perangkat.
- **Insider threats** — ancaman yang berasal dari dalam organisasi sendiri (karyawan yang disusupi atau berniat jahat).
- **Fake job offers** — tawaran kerja palsu yang dipakai untuk menipu korban (misalnya mencuri data pribadi atau menyusupkan malware lewat "tes teknis").

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📌 5. Strategi Pertahanan: Mitigation vs Detection

Dua pilar utama pertahanan terhadap serangan berbasis manusia:

- **Mitigation** — upaya mencegah atau mengurangi peluang dan dampak serangan sebelum terjadi (contoh: training karyawan, deploy anti-phishing tool). Sifatnya proaktif/preventif.
- **Detection** — kemampuan mendeteksi dan menyelidiki serangan yang berhasil lolos dari mitigasi. Ini adalah tugas inti seorang SOC analyst, karena tidak ada mitigasi yang 100% sempurna — cepat atau lambat akan ada yang bypass.

Kedua pilar ini saling melengkapi: mitigasi mengurangi *volume* serangan yang harus dihadapi SOC, sementara deteksi menangkap sisa serangan yang lolos.

### Contoh Langkah Mitigasi Konkret

- **Anti-phishing solution** — tool yang mendeteksi dan otomatis memblokir email phishing sebelum sampai ke user, sehingga meringankan beban kerja SOC.
- **Antivirus / EDR (Endpoint Detection and Response) solution** — dipasang di seluruh host korporat untuk mencegah eksekusi malware yang berhasil diunduh karyawan.
- **"Trust but verify" principle** — kebijakan yang menginstruksikan karyawan (khususnya IT support) untuk selalu memverifikasi identitas sebelum memproses permintaan sensitif (reset password, akses akun) — terutama untuk melawan deepfake/impersonation "CEO" atau "IT".
- **Security awareness training** — pelatihan rutin (idealnya berkala/kuartalan) yang mengajarkan karyawan mengenali phishing, diperkuat dengan simulasi phishing berkala agar kebiasaan waspada terus terlatih.

### Peran SOC Analyst dalam Konteks Ini

Selain memantau alert teknis, seorang SOC analyst yang matang biasanya juga:

- Menjaga koneksi erat dengan tim lain (IT, HR) untuk koordinasi respons insiden.
- Mengusulkan perbaikan kebijakan keamanan dan menjalankan pelatihan perusahaan.
- Menjadi kontak (hotline) bagi karyawan yang mencurigai adanya serangan terhadap dirinya.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📌 6. Studi Kasus dari Lab Praktik (Employees at Risk)

Empat skenario investigasi yang mengasah kemampuan verdict SOC analyst:

1. **Kasus malware terblokir antivirus (Lucas Martinez)** — karyawan mengunduh installer dari situs freeware tidak resmi, dan antivirus memblokirnya berkali-kali. Verdict yang benar: **karantina file dan arahkan ke installer resmi**, bukan menambahkan file ke exclusion antivirus (karena itu justru menonaktifkan proteksi yang sudah bekerja dengan benar).

2. **Alert SIEM lampiran mencurigakan (invoice phishing)** — email "invoice pembayaran" dengan lampiran RAR terpassword dari domain mirip Stripe (`stripe-payments.xyz`, bukan domain resmi Stripe). Verdict yang benar: **blokir email dan mulai investigasi**, karena kombinasi domain palsu + attachment terenkripsi (untuk lolos scanning) adalah indikator phishing klasik.

3. **Kasus impersonation ke IT support (Isabella)** — "CEO" menelepon dari nomor tersembunyi jam 9 malam meminta reset password, dan IT support langsung menurutinya tanpa verifikasi. Login berikutnya tercatat dari negara yang sama dengan lokasi CEO asli, namun CEO tidak merespons konfirmasi. Verdict yang benar: **nonaktifkan akun Gmail milik CEO sampai ada konfirmasi login yang sah**, karena kombinasi permintaan mendesak + jam ganjil + nomor tersembunyi + tidak bisa dikonfirmasi adalah pola khas impersonation.

4. **Anomalous login location (Rose Lewis)** — user mengunjungi domain phishing (`micrsoft365-online.ru` — contoh **typosquatting**, huruf "o" pada "microsoft" dihilangkan, ditambah TLD `.ru` yang tidak masuk akal untuk layanan resmi Microsoft) tepat sebelum login sukses terjadi dari lokasi berbeda dari kebiasaannya (**typical location**) ke lokasi baru yang tercatat pada event saat ini (**login location**). Meski jarak geografis (London–Oxford) relatif dekat dan secara waktu tempuh "masuk akal", konteks kunjungan ke situs phishing sesaat sebelumnya mengindikasikan kredensial kemungkinan sudah dicuri. Verdict yang benar: **nonaktifkan akun sampai lebih yakin**, bukan menutup alert hanya karena jarak lokasinya dekat.

**Pelajaran pola dari keempat kasus:** jangan pernah menilai satu indikator secara terisolasi (misalnya "lokasi dekat" saja) — SOC analyst harus menggabungkan seluruh konteks yang tersedia (riwayat browsing, waktu kejadian, pola komunikasi) sebelum mengambil keputusan.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📌 7. Studi Kasus dari Lab Praktik (Security Policy)

Dari 9 opsi kebijakan yang tersedia, 4 kebijakan berikut dipilih sebagai yang paling relevan untuk melindungi manusia dari social engineering:

- **Antivirus Solution** — mahal namun respons kuat terhadap lampiran phishing berbahaya atau unduhan malware.
- **Anti-Phishing Solution** — agak mahal, tapi sangat efektif karena mayoritas ancaman datang lewat email.
- **Access Management Policy** — mendokumentasikan cara IT support memverifikasi permintaan sensitif (reset password, akses), membuat deepfake/impersonation lebih sulit menipu staf IT.
- **Security Awareness Program** — bukan solusi anti-peluru, tapi edukasi karyawan tetap ide yang selalu bernilai.

Kebijakan yang **tidak dipilih** dan alasan mengapa kurang prioritas untuk konteks ini:

- **Manual Login Approval** — mewajibkan SOC analyst menyetujui setiap login email korporat secara manual. Tidak scalable untuk organisasi besar dan akan membebani SOC secara tidak proporsional dibanding manfaatnya.
- **Internet Restrictions** — membatasi akses internet karyawan hanya ke sumber daya korporat. Terlalu restriktif untuk operasional kantor normal, lebih cocok untuk lingkungan yang sangat sensitif/terisolasi.
- **Daily Vulnerability Scanning** — pemindaian kerentanan harian di server/workstation. Berguna untuk mitigasi celah teknis, tapi tidak secara langsung menyasar masalah manusia (social engineering) yang menjadi fokus utama room ini.

**Prinsip pemilihan:** kebijakan yang dipilih harus langsung menyasar akar masalah manusia (phishing, malware dari social engineering, impersonation, kurangnya awareness) — bukan sekadar kebijakan keamanan teknis yang bagus secara umum namun tidak relevan dengan konteks spesifik yang dihadapi.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📌 8. Rekomendasi Sumber Belajar Lanjutan

Ancaman siber terus berevolusi, sehingga SOC analyst perlu terus mengikuti tren serangan terbaru. Sumber yang direkomendasikan:

- **Krebs on Security** (krebsonsecurity.com) — investigasi mendalam soal insiden keamanan siber.
- **The Hacker News** (thehackernews.com) — berita keamanan siber terkini.
- **BleepingComputer** (bleepingcomputer.com) — berita dan panduan teknis seputar malware, ransomware, dan insiden keamanan.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📌 9. Glossary — Istilah Kunci yang Wajib Dihapal

- **SOC (Security Operations Center)** — tim/unit yang bertugas memantau, mendeteksi, dan merespons ancaman keamanan siber di sebuah organisasi.
- **Social Engineering** — manipulasi psikologis untuk membuat korban membantu penyerang secara sadar atau tidak sadar.
- **Phishing** — serangan lewat pesan (biasanya email) yang menyamar sebagai sumber terpercaya untuk mencuri kredensial atau menyebarkan malware.
- **Typosquatting** — teknik membuat domain yang mirip dengan domain asli (dengan huruf dihilangkan/diganti/ditukar) untuk menipu korban agar mengira itu situs resmi.
- **Malware** — perangkat lunak berbahaya yang dirancang untuk merusak, mencuri data, atau mengambil alih kontrol sistem korban.
- **ClickFix** — teknik social engineering di mana korban dituntun mengeksekusi command berbahaya sendiri lewat Run dialog (Win+R) menggunakan clipboard yang sudah disusupi.
- **SEO Poisoning** — memanipulasi hasil mesin pencari agar situs berbahaya muncul di posisi atas untuk kata kunci tertentu.
- **Deepfake** — konten audio/video hasil AI-generated yang menyamar sebagai orang nyata untuk menipu korban.
- **Impersonation** — penyamaran identitas (biasanya lewat telepon/chat) tanpa perlu teknologi deepfake.
- **Mitigation** — langkah pencegahan yang mengurangi peluang atau dampak serangan sebelum terjadi.
- **Detection** — kemampuan mendeteksi dan menyelidiki serangan yang berhasil lolos dari mitigasi.
- **EDR (Endpoint Detection and Response)** — solusi keamanan yang memantau dan merespons aktivitas mencurigakan di level endpoint (perangkat individual).
- **SIEM (Security Information and Event Management)** — sistem yang mengumpulkan dan menganalisis log/event keamanan dari berbagai sumber untuk mendeteksi anomali.
- **Typical Location vs Login Location** — baseline lokasi login normal seorang user (typical) dibandingkan dengan lokasi yang tercatat pada event login yang sedang dianalisis (login location); digunakan untuk mendeteksi login anomali.
- **Insider Threat** — ancaman keamanan yang berasal dari dalam organisasi sendiri (karyawan yang disusupi atau berniat jahat).
- **USB Drop Campaign** — teknik meninggalkan USB berisi malware di lokasi strategis agar ditemukan dan dicolokkan oleh korban.

# Attacking and Defending the Human Element: Catatan Belajar SOC Analyst

## Pengantar Room dan Konteks SOC

Setiap pembahasan tentang keamanan siber pada akhirnya akan sampai pada satu pertanyaan sederhana namun mendasar: apa sebenarnya yang dilindungi oleh sebuah Security Operations Center, atau yang biasa disingkat SOC? Jawabannya bukan sekadar server, database, atau infrastruktur jaringan. Room ini membingkai persoalan dengan cara yang cukup tajam, yaitu dengan mengajak kita berpikir seperti seorang attacker yang sedang mencoba membobol sebuah perusahaan. Ada dua jalan yang bisa ditempuh. Jalan pertama adalah menghabiskan berhari-hari untuk mengeksploitasi kerentanan teknis dan mencoba melewati pertahanan firewall satu per satu. Jalan kedua, yang jauh lebih sederhana, adalah cukup mengirim satu email phishing kepada administrator IT dan mendapatkan akses yang dibutuhkan tanpa perlu bersusah payah menembus lapisan pertahanan teknis apa pun.

Perbandingan ini membawa pada analogi yang dipakai sepanjang room, yaitu jaringan komputer sebagai sebuah benteng dengan tembok batu yang menjulang tinggi dan gerbang berlapis baja. Salah satu cara menaklukkan benteng semacam itu adalah dengan menerobos gerbangnya secara paksa, tetapi ada cara yang jauh lebih efisien, yakni cukup meminta si penjaga gerbang untuk membukakan pintu. Dalam dunia siber, manusia sering kali berperan sebagai penjaga gerbang yang naif ini. Mereka adalah mata rantai terlemah dalam keamanan siber, dan sayangnya justru merekalah yang paling sering membantu threat actor mencapai tujuannya, baik disadari maupun tidak. Room ini secara eksplisit menempatkan diri sebagai kelanjutan dari jalur SOC Level 1, dengan syarat telah menyelesaikan room Junior Security Analyst terlebih dahulu, dan tujuan pembelajarannya berpusat pada tiga hal: memahami peran elemen manusia dalam keamanan siber, memahami peran SOC dalam mendeteksi dan memitigasi serangan yang menyasar manusia, serta mempraktikkan pengetahuan tersebut lewat dua skenario yang dirancang realistis.

## Alasan Manusia Menjadi Sasaran Empuk

Pertanyaan mengapa manusia begitu sering menjadi target sebenarnya punya jawaban yang cukup lugas, yaitu akses. Manusia memegang akses ke website, mailbox, dan database, dan akses inilah yang menjadi barang berharga bagi threat actor. Menariknya, tidak semua threat actor punya pola pikir yang sama dalam berburu akses ini. Sebagian dari mereka sangat selektif dan mengincar akses yang spesifik, misalnya menargetkan satu orang tertentu di satu perusahaan tertentu karena tahu persis apa yang bisa mereka dapatkan dari sana. Sebagian yang lain justru tidak pilih-pilih sama sekali. Mereka membobol akun sebanyak mungkin terlebih dahulu, lalu baru memutuskan belakangan bagaimana cara memanfaatkan akses yang sudah mereka kumpulkan.

Room ini memberi empat contoh konkret yang menggambarkan pola pikir para threat actor ini, dan pola yang muncul cukup konsisten, yaitu kompromi awal terhadap satu individu jarang berhenti di situ saja melainkan menjadi pintu masuk menuju kerusakan yang jauh lebih besar. Contoh pertama adalah membobol akun Google milik seorang HR manager, yang jika berhasil akan membuka jalan bagi attacker untuk mencuri dan menjual seluruh database karyawan perusahaan. Contoh kedua adalah menipu seseorang yang kaya raya agar menjalankan malware, dengan tujuan akhir membajak sesi web banking dari komputer korban tersebut. Contoh ketiga adalah membobol akun VPN milik administrator IT, yang jika berhasil akan memberi attacker akses ke jantung dari jaringan korporat yang besar, sebuah posisi yang sangat strategis untuk bergerak lebih jauh ke seluruh sistem perusahaan. Contoh keempat adalah menipu seorang pekerja pemerintah agar membocorkan rahasia, di mana informasi yang didapat kemudian dipakai untuk mempermudah serangan-serangan berikutnya, semacam tahap reconnaissance untuk operasi yang lebih besar.

Empat contoh ini menunjukkan bahwa serangan terhadap manusia sebaiknya tidak pernah dipandang sebagai insiden yang berdiri sendiri. Sebuah SOC analyst yang baik perlu selalu bertanya, jika satu akun ini benar-benar sudah dikompromikan, langkah apa selanjutnya yang paling mungkin dilakukan attacker dengan akses tersebut.

## Anatomi Serangan terhadap Manusia

### Social Engineering sebagai Payung Besar

Semua teknik serangan yang menyasar manusia pada dasarnya bernaung di bawah satu istilah besar, yaitu social engineering. Definisi yang dipakai room ini cukup jelas, social engineering adalah taktik yang mengandalkan manipulasi terhadap korban agar korban membantu attacker, baik disadari maupun tidak. Yang membedakan taktik ini dari serangan teknis biasa adalah target eksploitasinya bukan celah pada kode atau konfigurasi sistem, melainkan celah pada cara berpikir dan bereaksi manusia. Agar taktik semacam ini bisa berhasil, ia harus dirancang dengan dua karakteristik utama. Pertama adalah trustworthy, di mana attacker harus tampak legitimate sehingga korban mempercayainya, entah dengan menyamar sebagai institusi resmi, atasan, atau rekan kerja. Kedua adalah emotional, di mana serangan harus mampu memicu perasaan seperti urgency atau rasa mendesak, fear atau ketakutan, maupun curiosity atau rasa penasaran, karena emosi-emosi inilah yang membuat korban bertindak cepat tanpa sempat berpikir kritis terlebih dahulu.

### Phishing sebagai Teknik Paling Umum

Phishing digambarkan sebagai bentuk social engineering yang paling umum, dengan estimasi sebanyak 3,4 miliar email berbahaya dikirim setiap harinya di seluruh dunia. Contoh paling sederhana yang diberikan adalah email dengan pesan seperti akun Anda telah dikompromikan, klik di sini untuk detail, di mana begitu link tersebut diklik korban akan diarahkan ke halaman login palsu yang tampilannya sangat mirip dengan aslinya, namun kredensial yang dimasukkan justru dikirim langsung ke attacker.

Room ini memberi dua contoh email phishing nyata yang masing-masing menyoroti indikator berbeda. Contoh pertama adalah email berjudul mendesak tentang login baru ke akun 1Password, yang dikirim dari alamat yang terlihat seperti resmi namun sebenarnya memakai domain palsu. Indikator kuncinya di sini adalah fake sender email, yaitu alamat pengirim yang dibuat semirip mungkin dengan yang asli namun sebenarnya berasal dari domain yang berbeda, serta URL to a fake login portal, yaitu link tombol yang jika diklik akan mengarahkan korban ke halaman login palsu untuk mencuri kredensial. Contoh kedua adalah email dengan judul notifikasi denda pajak yang mengharuskan pembayaran segera, lengkap dengan lampiran arsip terenkripsi password bernama TAX PENALTY.zip. Detail yang penting untuk dipahami di sini adalah alasan attacker sengaja mengenkripsi lampiran dengan password, yaitu agar file malware di dalamnya tidak bisa dipindai oleh sistem keamanan otomatis seperti Gmail scan, karena scanner tersebut tidak mampu membuka arsip yang terkunci password tanpa mengetahui passwordnya terlebih dahulu, yang biasanya justru dicantumkan di dalam badan email itu sendiri.

### Malware Downloads dan Berbagai Variannya

Selain lewat email, manusia juga sering tanpa sadar menginstal malware ketika mereka mencari dan mengunduh aplikasi di internet. Untuk membuat serangan jenis ini lebih meyakinkan, threat actor memakai berbagai teknik inovatif. Salah satu contoh yang diberikan adalah halaman palsu yang meniru tampilan resmi Firefox dengan pesan bahwa versi browser yang dipakai sudah usang dan perlu diperbarui, lengkap dengan tombol update yang sebenarnya mengunduh file bernama seperti Firefox.Update.4ce488.zip, yang fungsinya justru sebagai data stealer atau pencuri data.

Teknik lain yang lebih canggih dan patut dipahami secara mendalam adalah fake CAPTCHA verification yang menggunakan mekanisme bernama ClickFix. Dalam skenario ini korban disodori halaman verifikasi palsu yang memintanya melakukan serangkaian langkah, mulai dari menekan tombol Windows plus R untuk membuka jendela Run pada sistem operasi Windows, kemudian menekan Ctrl plus V di jendela tersebut, lalu menekan Enter, dan terakhir mengklik tombol verify untuk menyelesaikan proses. Yang membuat teknik ini berbahaya adalah pada saat korban mengunjungi halaman tersebut, clipboard mereka sebenarnya sudah disisipi command berbahaya secara diam-diam oleh script di halaman itu. Sehingga ketika korban menekan Ctrl plus V mengikuti instruksi yang tampak seperti prosedur verifikasi normal, mereka sebenarnya sedang menempelkan command jahat ke jendela Run, dan begitu menekan Enter, merekalah sendiri yang mengeksekusi malware tersebut tanpa sadar. Selain ClickFix, room ini juga menyebut dua teknik lain yaitu malicious QR codes, di mana kode QR dipakai untuk mengarahkan korban ke situs berbahaya karena URL aslinya tersembunyi di balik kode dan sulit diverifikasi sebelum dipindai, serta SEO poisoning, yaitu upaya memanipulasi hasil mesin pencari agar situs berbahaya muncul di posisi teratas untuk kata kunci pencarian yang populer.

### Deepfake dan Impersonation

Perkembangan teknologi AI-generated video dan audio menghadirkan dimensi baru dalam social engineering, yaitu deepfake. Room ini memberi satu contoh kasus nyata yang cukup mencolok, di mana seorang pekerja bagian finance menerima panggilan video deepfake dari sosok yang tampak persis seperti bosnya sendiri, dan berhasil ditipu untuk mentransfer dana sebesar dua puluh lima juta dolar Amerika untuk sebuah kesepakatan bisnis yang diklaim mendesak. Menariknya, room ini juga menegaskan bahwa attacker sebenarnya tidak selalu membutuhkan teknologi deepfake untuk berhasil melakukan impersonation atau penyamaran identitas. Banyak serangan ransomware belakangan ini justru dimulai dari sesuatu yang jauh lebih sederhana, yaitu sebuah panggilan telepon biasa dari seseorang yang menyamar sebagai departemen IT korporat, meminta korban untuk menyerahkan akses ke akun mereka dengan dalih perbaikan sistem yang mendesak.

### Metode Lain yang Disebutkan Secara Singkat

Meskipun room ini berfokus pada phishing, malware download, dan deepfake atau impersonation sebagai contoh utama, disebutkan pula bahwa masih ada banyak metode social engineering lain yang menjadi risiko konstan bagi karyawan. Di antaranya adalah USB drop campaign, yaitu meninggalkan USB berisi malware di lokasi yang mudah ditemukan dengan harapan rasa penasaran korban mendorong mereka untuk mencolokkannya ke komputer kantor, physical attacks atau serangan yang melibatkan akses fisik langsung, insider threats atau ancaman yang datang dari dalam organisasi itu sendiri, serta fake job offers atau tawaran pekerjaan palsu yang dipakai sebagai umpan untuk menipu korban.

## Strategi Pertahanan: Mitigasi versus Deteksi

Setelah memahami lanskap serangan, room ini beralih ke sisi pertahanan dengan membingkai peran SOC analyst ke dalam dua tugas besar, yaitu mitigation dan detection. Mitigasi bertujuan untuk mencegah atau mengurangi peluang dan dampak dari serangan sebelum serangan itu benar-benar terjadi, misalnya lewat pelatihan karyawan atau penerapan solusi anti-phishing. Namun betapapun baiknya langkah mitigasi yang diterapkan, suatu saat pasti akan ada serangan yang berhasil melewatinya, dan di titik inilah kemampuan deteksi menjadi vital untuk menyelidiki serangan-serangan canggih yang lolos dari lapisan mitigasi.

Sebagai seorang SOC analyst, tugas utama memang berpusat pada mendeteksi dan menyelidiki serangan, dan seluruh jalur SOC Level 1 dirancang untuk mengasah kemampuan ini. Namun room ini memberi catatan yang cukup realistis, bahwa seorang analyst pada akhirnya akan merasa lelah jika terus-menerus menganalisis serangan yang tidak ada habisnya, dan mulai berpikir bagaimana caranya mencegah ancaman umum secara otomatis sejak awal. Di sinilah pentingnya memahami langkah-langkah mitigasi utama, karena jika ide mitigasi yang diajukan disetujui oleh pihak IT dan manajemen puncak, rutinitas SOC akan menjadi lebih ringan dan perusahaan pun menjadi lebih aman secara keseluruhan.

Room ini memberi empat contoh konkret langkah mitigasi yang bisa diterapkan untuk melindungi karyawan. Yang pertama adalah anti-phishing solution, sebuah tool yang mendeteksi dan secara otomatis memblokir sebagian besar email phishing sebelum bahkan sempat disadari oleh pengguna, sehingga rutinitas SOC menjadi lebih mudah. Yang kedua adalah antivirus atau EDR solution, di mana EDR sendiri merupakan singkatan dari Endpoint Detection and Response, sebuah solusi keamanan yang andal dipasang di seluruh host korporat sebagai langkah bagus untuk mencegah manusia menjalankan malware secara tidak sengaja. Yang ketiga adalah prinsip trust but verify, yaitu menginstruksikan karyawan bagaimana cara mendeteksi deepfake dan memverifikasi permintaan yang mencurigakan yang mengatasnamakan CEO atau IT. Yang keempat adalah security awareness training, yaitu mengajarkan karyawan cara mendeteksi phishing dan memperkuat pelatihan tersebut lewat simulasi phishing secara berkala.

Di luar tugas deteksi dan mitigasi teknis, room ini juga menyinggung bahwa peran SOC analyst di lapangan sebenarnya bisa jauh lebih luas dari sekadar memantau alert. Setiap organisasi menghadapi serangan yang terus-menerus menargetkan karyawannya, namun cara SOC merespons hal ini bisa sangat bervariasi antar tim. Di beberapa tim, analyst memang hanya bertugas memantau alert saja. Namun di tim lain, mereka terlibat jauh lebih dalam pada proses perusahaan, misalnya dengan menjaga koneksi yang erat dengan tim lain seperti IT atau HR, mengusulkan peningkatan keamanan dan menjalankan pelatihan yang menjangkau seluruh perusahaan, bahkan sampai menjawab panggilan hotline dari karyawan yang mencurigai bahwa mereka sedang menjadi sasaran serangan.

## Praktik Investigasi Employees at Risk

Bagian praktik dari room ini menghadirkan sebuah dashboard keamanan interaktif bernama TryHackMe Security Dashboard, yang dirancang untuk menempatkan peserta pada posisi seorang SOC analyst sungguhan yang harus mengambil keputusan atas empat skenario nyata di tab Employees at Risk. Empat skenario ini masing-masing menguji kemampuan berpikir kritis dalam menilai konteks, bukan sekadar mengenali indikator serangan yang sudah jelas terlihat.

Skenario pertama melibatkan seorang software engineer baru bernama Lucas Martinez, yang mengeluh lewat chat bahwa ia urgently membutuhkan software 7-Zip namun aplikasi tersebut tidak mau berjalan. Setelah diselidiki, ternyata Lucas mengunduh sebuah file bernama Setup.exe dari situs bernama best-freeapps-2025.top, sebuah situs freeware hosting yang baru dan tidak dikenal, dan file tersebut sudah dicoba dijalankan sebanyak enam kali namun selalu diblokir oleh antivirus. Verdict yang tepat dalam kasus ini adalah mengkarantina file Setup.exe tersebut dan mengarahkan Lucas untuk memakai installer resmi 7-Zip, bukan sebaliknya menambahkan file itu ke dalam exclusion list antivirus. Pilihan menambahkan ke exclusion list terlihat menggoda karena seolah menyelesaikan keluhan pengguna dengan cepat, namun sebenarnya justru menonaktifkan proteksi yang sedang bekerja dengan benar terhadap kemungkinan file berbahaya.

Skenario kedua berupa sebuah alert SIEM mengenai lampiran email yang mencurigakan, dengan judul Stripe Invoice nomor 38291 yang berisi konfirmasi pembayaran sebesar 23.650 dolar dari TryHackMe, lengkap dengan lampiran Invoice.rar yang terkunci password 1111. Alamat pengirimnya adalah noreply@stripe-payments.xyz, yang jika diperhatikan bukan merupakan domain resmi Stripe. Verdict yang tepat adalah memblokir email tersebut dan memulai analisis, karena ini merupakan upaya email phishing, dengan indikator kombinasi domain palsu dan lampiran terenkripsi password sebagai ciri khas untuk menghindari deteksi otomatis.

Skenario ketiga melibatkan sebuah chat dari staf IT Support bernama Isabella, yang menceritakan bahwa ia baru saja menerima telepon dari CEO perusahaan bernama Ben, yang meminta password Gmail-nya direset karena mengalami masalah login. Isabella sudah menyetujui permintaan tersebut, namun ia sendiri merasa ragu karena telepon itu masuk pada pukul 9 malam dan berasal dari nomor tersembunyi. Ketika diperiksa lebih lanjut, memang ditemukan ada login dari Amerika Serikat, negara yang sama dengan tempat tinggal Ben, namun Ben sendiri tidak merespons pesan maupun panggilan telepon konfirmasi. Verdict yang tepat adalah menonaktifkan akun Gmail Ben sampai ia sendiri mengonfirmasi login tersebut atau kembali ke kantor, bukan menunggu sampai hari kerja berikutnya untuk menanyakan detailnya secara langsung. Kombinasi antara jam telepon yang ganjil, nomor tersembunyi, dan ketidakmampuan mengonfirmasi identitas asli Ben menjadi indikator kuat dari upaya impersonation.

### Studi Kasus Rose Lewis dan Anatomi Credential Phishing

Skenario keempat inilah yang paling banyak dibahas secara mendalam dalam diskusi, karena pada awalnya memunculkan kebingungan yang cukup wajar mengenai bagaimana sebenarnya alur kejadian yang sedang dianalisis. Kasus ini berupa alert SIEM tentang anomalous login location yang menimpa seorang HR Assistant bernama Rose Lewis. Datanya menunjukkan bahwa Rose login ke Microsoft 365 dari lokasi London, Inggris, sementara lokasi kebiasaannya, yang dalam sistem SIEM disebut typical location, adalah Oxford, Inggris. Selain itu tercatat pula daftar visited URLs before login, yaitu tiga alamat yang dikunjungi Rose sebelum event login tersebut terjadi, salah satunya adalah sebuah alamat yang tampak seperti portal login Microsoft.

Kebingungan yang muncul dalam diskusi berpusat pada pertanyaan, jika Rose sebelumnya mengunjungi sebuah domain phishing, bagaimana ceritanya ia bisa tetap berhasil login, dan login ke mana sebenarnya yang dimaksud. Penjelasannya adalah bahwa daftar visited URLs before login bukanlah bagian dari proses login yang sama, melainkan riwayat browsing Rose sebelum event login yang tercatat di SIEM terjadi. Urutan kejadian yang paling masuk akal adalah sebagai berikut. Pertama, Rose mengunjungi halaman login palsu yang dibuat sangat mirip dengan Microsoft 365, kemungkinan besar tanpa menyadari bahwa itu bukan halaman asli. Kedua, di halaman palsu tersebut Rose memasukkan username dan password akun Microsoft 365-nya, dan begitu ia menekan submit, kredensial tersebut langsung dicuri oleh attacker yang mengoperasikan domain palsu itu. Ketiga, setelah kredensial berhasil dicuri, sistem SIEM kemudian mendeteksi ada login yang benar-benar terjadi ke akun Microsoft 365 asli milik Rose, namun dari lokasi yang berbeda dari kebiasaannya. Dari sini muncul dua kemungkinan mengenai siapa yang sebenarnya melakukan login tersebut, yaitu bisa saja Rose sendiri yang kebetulan sedang berada di London, misalnya karena perjalanan dinas, atau bisa juga attacker yang sudah berhasil mencuri kredensialnya dan langsung memakainya untuk login ke akun asli Rose.

Salah satu poin paling penting yang terungkap dari diskusi ini adalah pengenalan terhadap teknik bernama typosquatting. Domain phishing yang dikunjungi Rose tertulis sebagai micrsoft365-online.ru. Jika dieja huruf demi huruf dan dibandingkan dengan nama perusahaan aslinya, Microsoft, akan terlihat bahwa ada satu huruf yang hilang, yaitu huruf o pada bagian micr yang seharusnya micro. Perbedaan sekecil ini sangat mudah luput dari perhatian seseorang yang sedang terburu-buru atau tidak cukup teliti membaca alamat domain. Indikator kedua yang tak kalah penting adalah top level domain atau TLD yang dipakai, yaitu .ru, yang jelas tidak masuk akal untuk sebuah portal login resmi milik Microsoft, yang seharusnya memakai domain seperti .com atau subdomain resmi dari microsoft.com. Kombinasi huruf yang hilang dan TLD yang mencurigakan inilah yang menjadi tanda kuat bahwa domain tersebut adalah hasil typosquatting, sebuah teknik di mana attacker sengaja membuat domain yang tampak sangat mirip dengan domain asli namun dengan sedikit perubahan huruf, agar korban yang kurang teliti tidak menyadari perbedaannya.

Diskusi juga menyinggung perbedaan mendasar antara dua istilah, yaitu typical location dan login location. Typical location adalah lokasi atau kota yang biasanya dipakai seorang pengguna untuk login, yang dicatat oleh sistem monitoring atau SIEM berdasarkan histori login sebelumnya sebagai semacam baseline kebiasaan normal. Sementara itu login location adalah lokasi yang terdeteksi pada event login yang sedang dianalisis saat itu juga. Sistem semacam ini biasanya memiliki fitur bernama impossible travel atau anomalous login detection, yang berfungsi membandingkan lokasi login sekarang dengan lokasi kebiasaannya, atau bahkan dengan lokasi login sebelumnya dikombinasikan dengan selisih waktu antar login, untuk mendeteksi apakah ada sesuatu yang janggal. Jika selisih lokasi tersebut masih masuk akal secara jarak dan waktu tempuh, biasanya akan dianggap wajar. Namun jika tidak masuk akal, misalnya login dari satu kota lalu beberapa menit kemudian login dari kota lain yang jauh sekali jaraknya, hal itu jelas patut dicurigai.

Poin krusial yang muncul dari diskusi kasus Rose Lewis adalah bahwa jarak geografis semata tidak boleh dijadikan satu-satunya dasar pengambilan keputusan. Secara jarak, London dan Oxford sebenarnya cukup dekat, hanya berjarak sekitar satu jam perjalanan, sehingga jika dilihat sendirian tanpa konteks lain, perbedaan lokasi ini bisa saja dianggap wajar dan tidak mencurigakan. Namun ketika digabungkan dengan fakta bahwa Rose baru saja mengunjungi situs phishing tepat sebelum event login tersebut terjadi, kombinasi ini menjadi indikasi kuat bahwa kredensialnya kemungkinan besar sudah dicuri dan sedang dipakai oleh pihak yang tidak berwenang. Karena itulah verdict yang tepat untuk kasus ini adalah menonaktifkan akun Rose Lewis sampai ada keyakinan yang lebih kuat mengenai keabsahan login tersebut, bukan menutup begitu saja alert dengan alasan bahwa jarak lokasinya masih wajar. Pelajaran besar yang bisa ditarik dari keempat skenario Employees at Risk ini secara keseluruhan adalah bahwa seorang SOC analyst tidak boleh pernah menilai satu indikator secara terisolasi. Setiap keputusan verdict harus mempertimbangkan seluruh konteks yang tersedia secara bersamaan, mulai dari riwayat browsing, waktu kejadian, sampai pola komunikasi yang menyertainya, sebelum akhirnya mengambil kesimpulan.

## Praktik Menyusun Security Policy

Bagian praktik kedua dari dashboard berada di tab Security Policy, di mana peserta diminta membangun kembali kebijakan keamanan organisasi dengan memilih empat dari sembilan kebijakan yang tersedia, kebijakan mana yang dianggap paling memberi manfaat bagi organisasi. Setelah proses seleksi, hasil pilihan akan dibagikan ke kolega dan departemen lain, yang kemudian memberi umpan balik atas setiap kebijakan yang dipilih.

Empat kebijakan yang dipilih dan terbukti tepat adalah sebagai berikut. Yang pertama adalah Antivirus Solution, yaitu membeli dan memasang antivirus yang andal di seluruh workstation karyawan, dengan umpan balik bahwa solusi ini memang mahal namun merupakan respons yang kuat terhadap lampiran phishing berbahaya atau unduhan malware, sebuah kaitan langsung dengan kasus Lucas Martinez yang dibahas sebelumnya. Yang kedua adalah Anti-Phishing Solution, yaitu membeli dan menerapkan solusi yang mendeteksi serta secara otomatis memblokir sebagian besar email phishing, dengan umpan balik bahwa meski agak mahal, ini merupakan respons yang sangat baik karena banyak ancaman datang lewat email. Yang ketiga adalah Access Management Policy, yaitu mendokumentasikan bagaimana tim IT support seharusnya memverifikasi permintaan seperti reset password atau persetujuan akses, dengan umpan balik bahwa ini merupakan inisiatif bagus karena kebijakan yang baik akan membuat deepfake lebih sulit menipu pihak IT support, sebuah kaitan langsung dengan kasus Isabella yang menerima telepon tanpa verifikasi yang memadai. Yang keempat adalah Security Awareness Program, yaitu menyiapkan pelatihan kuartalan bagi seluruh karyawan tentang cara mendeteksi dan melaporkan teknik phishing modern, dengan umpan balik bahwa meski bukan solusi yang bulletproof, mendidik karyawan selalu merupakan ide yang bagus.

Sementara itu ada tiga kebijakan yang tidak dipilih, dan masing-masing memiliki alasan tersendiri mengapa kurang tepat untuk konteks ini. Yang pertama dan paling banyak dibahas dalam diskusi adalah Manual Login Approval, yaitu kebijakan yang mewajibkan SOC analyst untuk menyetujui setiap login ke email korporat secara manual satu per satu. Penjelasannya adalah bahwa kebijakan semacam ini secara konsep sebenarnya bisa terdengar aman, karena ada manusia yang mengawasi setiap login secara langsung. Namun jika dibayangkan dalam skala organisasi yang sesungguhnya, di mana ada ratusan bahkan ribuan karyawan yang masing-masing login berkali-kali dalam sehari, beban kerja yang dihasilkan akan sangat tidak proporsional. SOC analyst akan kewalahan hanya untuk menyetujui login satu per satu, sehingga tidak sempat mengerjakan tugas-tugas lain yang jauh lebih penting. Kebijakan ini pada dasarnya tidak scalable, dan persoalan yang ingin diselesaikannya sebenarnya bisa dijawab dengan cara yang jauh lebih efisien, misalnya lewat penerapan multi-factor authentication atau sistem deteksi anomali otomatis. Kebijakan kedua yang tidak dipilih adalah Internet Restrictions, yaitu membatasi akses internet seluruh karyawan hanya pada sumber daya korporat saja, yang dianggap terlalu ekstrem untuk operasional kantor pada umumnya karena karyawan tetap membutuhkan akses internet luas untuk riset dan komunikasi eksternal, dan kebijakan semacam ini sebenarnya lebih cocok diterapkan pada lingkungan yang sangat sensitif atau terisolasi seperti jaringan militer. Kebijakan ketiga yang tidak dipilih adalah Daily Vulnerability Scanning, yaitu melakukan pemindaian kerentanan harian pada seluruh server dan workstation korporat, yang meskipun merupakan praktik keamanan teknis yang baik, tidak secara langsung menyasar akar masalah manusia yang menjadi fokus utama room ini, karena vulnerability scanning lebih relevan untuk mendeteksi celah teknis pada sistem, bukan untuk mencegah karyawan tertipu oleh phishing atau deepfake.

Prinsip besar yang bisa disarikan dari proses pemilihan kebijakan ini adalah bahwa kebijakan keamanan yang baik untuk satu konteks belum tentu menjadi prioritas untuk konteks yang lain. Empat kebijakan yang terpilih semuanya langsung menyasar akar masalah manusia, mulai dari phishing, malware hasil social engineering, impersonation lewat deepfake, sampai kurangnya kesadaran keamanan pada karyawan. Sementara tiga kebijakan yang tidak dipilih, meskipun sebagian di antaranya merupakan praktik keamanan yang secara umum baik, tidak cukup relevan dengan persoalan spesifik yang sedang dihadapi organisasi dalam skenario ini, atau bahkan berpotensi menimbulkan masalah operasional baru seperti pada kasus Manual Login Approval.

## Penutup dan Sumber Belajar Lanjutan

Room ini ditutup dengan sebuah refleksi singkat namun penting, bahwa perjalanan belajar mengenai serangan terhadap manusia sebagai elemen terlemah dalam keamanan siber tidak seharusnya berhenti begitu saja setelah room ini selesai. Ancaman terus berevolusi, dan seiring perkembangan tersebut, kemampuan untuk tetap mengikuti tren serangan terbaru menjadi kunci kesuksesan seorang SOC analyst dalam menjalankan tugasnya sehari-hari. Untuk itu, room ini merekomendasikan tiga sumber bacaan yang bisa diikuti secara berkelanjutan, yaitu Krebs on Security di krebsonsecurity.com yang dikenal dengan investigasi mendalamnya terhadap insiden keamanan siber, The Hacker News di thehackernews.com yang menyajikan berita keamanan siber terkini, dan BleepingComputer di bleepingcomputer.com yang menyediakan berita serta panduan teknis seputar malware, ransomware, dan berbagai insiden keamanan lainnya.

Secara keseluruhan, room ini berhasil menyampaikan sebuah pesan yang cukup konsisten dari awal sampai akhir, bahwa pertahanan siber yang kuat tidak bisa hanya bertumpu pada solusi teknis semata. Firewall yang kokoh dan sistem yang terpatch dengan baik akan menjadi kurang berarti jika satu orang di dalam organisasi bisa ditipu untuk membukakan pintu bagi attacker. Karena itu, kemampuan seorang SOC analyst untuk membaca konteks, menggabungkan berbagai indikator yang tersebar, dan mengambil keputusan yang tepat di tengah ketidakpastian, seperti yang tercermin dalam keempat studi kasus Employees at Risk, menjadi keterampilan yang sama pentingnya dengan penguasaan tool dan teknologi keamanan itu sendiri.