# Resume: Attacking and Defending the Human Element in SOC

Tanggal: 8 Agustus 2026

---

## 1. Konteks dan Tujuan Room

Room ini membahas mengapa manusia dianggap elemen paling lemah dalam keamanan siber, bagaimana attacker mengeksploitasi kelemahan tersebut, dan bagaimana peran **SOC (Security Operations Center) analyst** dalam mendeteksi serta mencegahnya.

Prinsip dasarnya: menyerang sistem secara teknis (bypass firewall, eksploitasi vulnerability) memakan waktu dan usaha besar. Menipu manusia agar memberi akses secara sukarela jauh lebih efisien. Manusia berperan sebagai "gatekeeper" dalam sebuah jaringan, dan gatekeeper ini bisa ditipu untuk membuka aksesnya sendiri.

---

## 2. Mengapa Manusia Ditarget

Manusia ditarget karena **akses** yang mereka miliki, baik ke website, mailbox, maupun database. Ada dua pola threat actor:

- **Targeted** — mengincar akses spesifik dari individu/perusahaan tertentu.
- **Opportunistic** — membobol akun sebanyak mungkin, baru memutuskan pemanfaatannya belakangan.

Pola umum tujuan akhir setelah kompromi awal terhadap manusia:

- Breach akun HR manager → mencuri dan menjual database karyawan.
- Menipu target menjalankan malware → membajak sesi web banking korban.
- Breach akun VPN administrator IT → akses ke jantung jaringan korporat (lateral movement).
- Menipu pekerja membocorkan info sensitif → mempermudah serangan lanjutan (recon).

Kompromi awal terhadap satu individu jarang berhenti di situ saja — biasanya menjadi entry point untuk serangan yang lebih besar.

---

## 3. Social Engineering sebagai Konsep Inti

**Social engineering** adalah taktik manipulasi psikologis yang membuat korban membantu attacker, baik disadari maupun tidak. Targetnya bukan celah teknis, melainkan celah cara berpikir manusia.

Dua syarat agar taktik ini berhasil:

- **Trustworthy** — attacker harus tampak legitimate agar korban percaya (menyamar sebagai bank, IT support, atasan, dll).
- **Emotional** — serangan harus memicu emosi seperti *urgency*, *fear*, atau *curiosity* agar korban bertindak cepat tanpa berpikir kritis.

---

## 4. Klasifikasi Teknik Serangan terhadap Manusia

### 4.1 Phishing

Teknik social engineering paling umum, dengan estimasi 3,4 miliar email berbahaya dikirim setiap hari.

Indikator kunci yang perlu dikenali:

- **Fake sender email** — alamat pengirim dibuat mirip domain asli, padahal berbeda (contoh: `alert@onepassword.top` menyamar sebagai 1Password).
- **URL to fake login portal** — link yang mengarah ke halaman login palsu untuk mencuri kredensial.
- **Malware attachment** — lampiran berbahaya, sering dienkripsi password (contoh: `TAX PENALTY.zip`) agar lolos dari email scanner otomatis, karena scanner tidak bisa membuka arsip terkunci password.

### 4.2 Malware Downloads

Korban tanpa sadar menginstal malware saat mencari/mengunduh aplikasi. Teknik yang dipakai attacker:

- **Fake website (brand impersonation)** — meniru tampilan situs resmi (contoh: halaman palsu "Update Firefox") untuk mendorong unduhan **data stealer**.
- **Fake CAPTCHA verification + ClickFix** — korban dituntun menekan Windows Key + R (membuka jendela Run), lalu diminta paste (Ctrl+V) sesuatu yang sebenarnya sudah disisipkan diam-diam ke clipboard korban saat mengunjungi halaman tersebut, lalu menekan Enter. Korban sendiri yang mengeksekusi command berbahaya tanpa sadar.
- **SEO poisoning** — memanipulasi hasil mesin pencari agar situs berbahaya muncul di posisi atas untuk kata kunci populer.
- **Malicious QR codes** — kode QR mengarahkan ke situs/unduhan berbahaya; URL asli tersembunyi di balik kode sehingga sulit diverifikasi sebelum diklik.

### 4.3 Deepfake dan Impersonation

- **Deepfake** — video/audio hasil AI-generated yang menyamar sebagai orang yang dikenal korban (keluarga, kolega, mitra bisnis). Contoh nyata: pekerja finance tertipu deepfake video call "bos"-nya, mentransfer 25 juta dolar untuk "urgent business deal".
- **Impersonation** — penyamaran identitas tanpa perlu deepfake, cukup lewat telepon/chat. Contoh: serangan ransomware dimulai dari telepon penyerang menyamar sebagai IT department, meminta korban menyerahkan akses akun demi "system repair" mendesak.

### 4.4 Metode Lain (disebut singkat)

- **USB drop campaign** — meninggalkan USB berisi malware di lokasi strategis, berharap korban penasaran dan mencolokkannya.
- **Physical attacks** — serangan yang melibatkan akses fisik langsung ke lokasi/perangkat.
- **Insider threats** — ancaman dari dalam organisasi sendiri.
- **Fake job offers** — tawaran kerja palsu untuk menipu korban.

---

## 5. Strategi Pertahanan

### 5.1 Mitigation vs Detection

- **Mitigation** — mencegah/mengurangi peluang dan dampak serangan sebelum terjadi. Sifatnya proaktif.
- **Detection** — mendeteksi dan menyelidiki serangan yang berhasil lolos dari mitigasi. Tugas inti SOC analyst, karena tidak ada mitigasi yang sempurna.

Kedua pilar saling melengkapi: mitigasi mengurangi volume serangan yang harus dihadapi SOC, deteksi menangkap sisanya.

### 5.2 Contoh Langkah Mitigasi

- **Anti-phishing solution** — mendeteksi dan otomatis memblokir email phishing sebelum sampai ke user.
- **Antivirus / EDR (Endpoint Detection and Response) solution** — dipasang di seluruh host korporat, mencegah eksekusi malware.
- **"Trust but verify" principle** — menginstruksikan karyawan (khususnya IT support) untuk memverifikasi identitas sebelum memproses permintaan sensitif, melawan deepfake/impersonation "CEO" atau "IT".
- **Security awareness training** — pelatihan rutin (idealnya kuartalan) mengenali phishing, diperkuat simulasi phishing berkala.

### 5.3 Peran SOC Analyst di Luar Deteksi Teknis

- Menjaga koneksi erat dengan tim lain (IT, HR) untuk koordinasi respons insiden.
- Mengusulkan perbaikan kebijakan keamanan dan menjalankan pelatihan perusahaan.
- Menjadi kontak (hotline) bagi karyawan yang mencurigai serangan terhadap dirinya.

---

## 6. Studi Kasus: Employees at Risk

Empat skenario investigasi verdict SOC analyst.

**Kasus 1 — Malware terblokir antivirus (Lucas Martinez).** Karyawan mengunduh installer dari situs freeware tidak resmi, diblokir antivirus berkali-kali. Verdict benar: karantina file, arahkan ke installer resmi — bukan menambahkan ke exclusion antivirus (menonaktifkan proteksi yang sudah bekerja benar).

**Kasus 2 — Invoice phishing.** Email "invoice pembayaran" dengan lampiran RAR terpassword dari domain mirip Stripe (`stripe-payments.xyz`, bukan domain resmi). Verdict benar: blokir email dan mulai investigasi — kombinasi domain palsu + attachment terenkripsi adalah indikator phishing klasik.

**Kasus 3 — Impersonation ke IT support (Isabella).** "CEO" menelepon dari nomor tersembunyi jam 9 malam minta reset password, langsung disetujui tanpa verifikasi. Login berikutnya dari negara sama dengan lokasi CEO asli, namun CEO tidak merespons konfirmasi. Verdict benar: nonaktifkan akun sampai ada konfirmasi login sah — kombinasi permintaan mendesak + jam ganjil + nomor tersembunyi + tidak bisa dikonfirmasi adalah pola khas impersonation.

**Kasus 4 — Anomalous login location (Rose Lewis).** User mengunjungi domain phishing (`micrsoft365-online.ru`) tepat sebelum login sukses terjadi dari lokasi berbeda dari kebiasaannya. Verdict benar: nonaktifkan akun sampai lebih yakin — bukan menutup alert hanya karena jarak lokasi dekat.

Detail kasus 4 dijelaskan pada bagian 6.1 karena mengandung dua konsep teknis kunci.

### 6.1 Detail Kasus Rose Lewis: Typosquatting dan Login Anomaly

- **Typical location** — lokasi/kota kebiasaan login user, dicatat sistem berdasarkan histori (contoh: Oxford, UK).
- **Login location** — lokasi yang terdeteksi pada event login yang sedang dianalisis (contoh: London, UK).
- **Typosquatting** — domain dibuat mirip domain asli dengan huruf dihilangkan/diganti/ditukar. Pada kasus ini, `micrsoft365-online.ru` kehilangan huruf "o" dari "microsoft", ditambah TLD `.ru` yang tidak masuk akal untuk layanan resmi Microsoft.
- **Alur serangan**: korban mengunjungi halaman login palsu → memasukkan kredensial asli → kredensial dicuri attacker → attacker memakai kredensial tersebut login ke akun asli dari lokasi berbeda.
- **Pelajaran kunci**: jarak geografis semata (London-Oxford relatif dekat) tidak cukup untuk menutup alert. Konteks tambahan (kunjungan ke situs phishing sesaat sebelumnya) mengubah kesimpulan verdict.

**Pola umum dari 4 kasus**: jangan menilai satu indikator secara terisolasi — gabungkan seluruh konteks (riwayat browsing, waktu kejadian, pola komunikasi) sebelum mengambil keputusan.

---

## 7. Studi Kasus: Menyusun Security Policy

Dari 9 opsi kebijakan, 4 berikut dipilih sebagai yang paling relevan melindungi manusia dari social engineering:

- **Antivirus Solution** — mahal namun respons kuat terhadap lampiran phishing berbahaya/unduhan malware.
- **Anti-Phishing Solution** — agak mahal, sangat efektif karena mayoritas ancaman lewat email.
- **Access Management Policy** — mendokumentasikan cara IT support memverifikasi permintaan sensitif, membuat deepfake/impersonation lebih sulit menipu staf IT.
- **Security Awareness Program** — bukan solusi anti-peluru, tapi edukasi karyawan selalu bernilai.

Kebijakan yang **tidak dipilih** dan alasannya:

- **Manual Login Approval** — mewajibkan SOC analyst menyetujui setiap login email korporat secara manual. Tidak scalable untuk organisasi besar, membebani SOC secara tidak proporsional.
- **Internet Restrictions** — membatasi akses internet karyawan hanya ke sumber daya korporat. Terlalu restriktif untuk operasional kantor normal.
- **Daily Vulnerability Scanning** — pemindaian kerentanan harian di server/workstation. Berguna untuk celah teknis, tapi tidak langsung menyasar masalah manusia yang jadi fokus room ini.

**Prinsip pemilihan**: kebijakan harus langsung menyasar akar masalah manusia (phishing, malware dari social engineering, impersonation, kurangnya awareness), bukan sekadar kebijakan keamanan teknis yang bagus secara umum namun tidak relevan dengan konteks spesifik.

---

## 8. Glossary

- **SOC (Security Operations Center)** — tim/unit yang memantau, mendeteksi, dan merespons ancaman keamanan siber organisasi.
- **Social Engineering** — manipulasi psikologis agar korban membantu attacker, sadar atau tidak.
- **Phishing** — serangan lewat pesan (biasanya email) menyamar sebagai sumber terpercaya untuk mencuri kredensial/menyebarkan malware.
- **Typosquatting** — domain mirip domain asli (huruf dihilangkan/diganti/ditukar) untuk menipu korban.
- **Malware** — perangkat lunak berbahaya yang merusak, mencuri data, atau mengambil alih kontrol sistem.
- **ClickFix** — teknik social engineering yang menuntun korban mengeksekusi command berbahaya sendiri lewat Run dialog (Win+R) memakai clipboard yang sudah disusupi.
- **SEO Poisoning** — memanipulasi hasil mesin pencari agar situs berbahaya muncul di posisi atas.
- **Deepfake** — konten audio/video hasil AI-generated menyamar sebagai orang nyata.
- **Impersonation** — penyamaran identitas (telepon/chat) tanpa perlu deepfake.
- **Mitigation** — langkah pencegahan yang mengurangi peluang/dampak serangan sebelum terjadi.
- **Detection** — kemampuan mendeteksi dan menyelidiki serangan yang lolos dari mitigasi.
- **EDR (Endpoint Detection and Response)** — solusi keamanan yang memantau dan merespons aktivitas mencurigakan di level endpoint.
- **SIEM (Security Information and Event Management)** — sistem yang mengumpulkan dan menganalisis log/event keamanan dari berbagai sumber untuk mendeteksi anomali.
- **Typical Location vs Login Location** — baseline lokasi login normal user vs lokasi yang tercatat pada event login yang dianalisis; dipakai mendeteksi login anomali.
- **Insider Threat** — ancaman keamanan dari dalam organisasi sendiri.
- **USB Drop Campaign** — meninggalkan USB berisi malware di lokasi strategis agar dicolokkan korban.

---

## 9. Tools dan Platform Rujukan

- **Krebs on Security** — investigasi mendalam insiden keamanan siber. `https://krebsonsecurity.com`
- **The Hacker News** — berita keamanan siber terkini. `https://thehackernews.com`
- **BleepingComputer** — berita dan panduan teknis seputar malware, ransomware, insiden keamanan. `https://www.bleepingcomputer.com`

---

## 10. Catatan Ringkas untuk Ditulis Tangan

**Konsep dasar**

- Human element — mata rantai terlemah cyber security
- Social engineering — manipulasi psikologis, syarat: trustworthy + emotional (urgency/fear/curiosity)
- Kenapa ditarget — akses (website/mailbox/database); targeted vs opportunistic

**Teknik serangan**

- Phishing — 3.4 miliar email/hari; ciri: fake sender email, fake login URL, malware attachment (sering di-zip password biar lolos scan)
- Malware download — fake website (brand impersonation), fake CAPTCHA + ClickFix (Win+R, Ctrl+V, Enter = jalankan malware lewat clipboard), SEO poisoning, malicious QR code
- Deepfake — AI video/audio palsu (contoh: kasus $25 juta)
- Impersonation — nyamar tanpa deepfake, cukup telepon/chat
- Lainnya — USB drop, physical attack, insider threat, fake job offer

**Pertahanan**

- Mitigation (cegah) vs Detection (deteksi yang lolos)
- Mitigasi: anti-phishing tool, antivirus/EDR, trust but verify, security awareness training
- Peran SOC luas: koordinasi IT/HR, usul kebijakan, hotline karyawan

**Studi kasus Employees at Risk**

- Kasus 1 Lucas — malware diblokir AV → karantina, jangan exclusion
- Kasus 2 Invoice — domain palsu stripe-payments.xyz + RAR password → blokir & investigasi
- Kasus 3 Isabella — telpon "CEO" jam 9 malam nomor hidden → nonaktifkan akun sampai konfirmasi
- Kasus 4 Rose Lewis — kunjungi domain phishing micrsoft365-online.ru (typosquat: huruf "o" hilang + TLD .ru salah) → login anomali London vs typical Oxford → nonaktifkan akun
- Pola: jangan nilai 1 indikator sendirian, gabung semua konteks

**Security Policy — dipilih**

- Antivirus Solution
- Anti-Phishing Solution
- Access Management Policy (verifikasi request IT support)
- Security Awareness Program

**Security Policy — ditolak**

- Manual Login Approval — gak scalable, beban SOC meledak
- Internet Restrictions — terlalu ekstrem buat kantor biasa
- Daily Vulnerability Scanning — bagus tapi gak fokus ke masalah manusia

**Istilah wajib hapal**

- Typosquatting — domain mirip asli, huruf diubah/hilang
- ClickFix — korban jalankan malware sendiri via Run dialog
- SEO poisoning — racuni hasil search engine
- EDR — Endpoint Detection and Response
- SIEM — kumpulin & analisis log buat deteksi anomali
- Typical location vs login location — baseline vs lokasi event sekarang
- Insider threat — ancaman dari dalam
- USB drop campaign — sebar USB berisi malware

**Sumber belajar lanjut**

- Krebs on Security
- The Hacker News
- BleepingComputer
