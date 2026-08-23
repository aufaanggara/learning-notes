# Resume: Investigating Phishing Emails
**Tanggal:** 23 Agustus 2026

---

**1. Pendahuluan**

Room ini merupakan bagian kedua dari Phishing Analysis module pada SOC Level 1 Path. Fokus room ini adalah membangun kemampuan analisis email phishing secara lebih mendalam, mencakup pengumpulan artifact, penggunaan tools analisis header dan body email, reputasi IP/URL, malware sandbox, hingga platform investigasi terintegrasi seperti PhishTool. Materi ditutup dengan praktik langsung menganalisis tiga kasus phishing nyata.

---

**2. Artifact yang Dikumpulkan Saat Analisis Email**

**2.1 Header Artifacts**

Informasi yang perlu dicatat saat menganalisis header email:

- **Sender email address** — menentukan dari mana email berasal.
- **Sender IP address** — IP sumber pengirim, dan hasil reverse lookup-nya untuk mengetahui kepemilikan/lokasi.
- **Email subject line** — diperiksa apakah mengandung unsur urgensi atau call to action, ciri khas phishing.
- **Recipient email address** — penerima yang dituju (To/CC/BCC).
- **Reply-To email address** — ke mana balasan sebenarnya diarahkan; sering berbeda dari alamat pengirim yang tampil.
- **Date and time** — waktu pengiriman email.

**2.2 Body Analysis**

Artifact yang perlu diperiksa dari body email:

- **URLs and hyperlinks** — semua link diidentifikasi, termasuk shortened URL yang perlu di-expand untuk mengetahui tujuan aslinya.
- **Attachment name(s)** — nama file dan ekstensinya diperiksa apakah mencurigakan.
- **Attachment hash** — hash file (umumnya SHA256) dibuat untuk keperluan threat intelligence lookup.

---

**3. Analisis Header Email**

**3.1 Tools Analisis Header**

| Tool | Fungsi |
|---|---|
| Messageheader (Google Admin Toolbox) | Menganalisis raw header dengan cara paste header, menampilkan IP pengirim, routing path, dan misconfiguration (SPF/DKIM) |
| Message Header Analyzer | Fungsi serupa Messageheader, menampilkan summary, diagnostic report, received headers, dan other headers |

**3.2 Konsep Autentikasi Email**

- **SPF (Sender Policy Framework)** — memverifikasi apakah server pengirim berwenang mengirim atas nama domain tersebut. Hasil dapat berupa *pass*, *fail*, *none*, atau *permerror* (kesalahan konfigurasi SPF pada domain).
- **DKIM (DomainKeys Identified Mail)** — memverifikasi tanda tangan digital email untuk memastikan konten tidak diubah dalam perjalanan. Bisa gagal (*fail*) jika email dipalsukan atau domain tidak mengonfigurasinya dengan benar.
- **DMARC (Domain-based Message Authentication, Reporting & Conformance)** — kebijakan yang menentukan tindakan terhadap email yang gagal SPF/DKIM. Hasil yang umum: *pass* atau *fail*.

**3.3 Reputasi IP dan URL**

Tujuannya adalah menentukan apakah infrastruktur IP/domain yang teridentifikasi legitimate atau terkait aktivitas berbahaya.

| Tool | Fungsi |
|---|---|
| IPinfo | Mengambil informasi geografis dan organisasi dari sebuah IP address |
| URLScan.io | Menyimulasikan sesi browsing terhadap sebuah URL secara aman tanpa mengunjunginya langsung; merekam aktivitas dan menyediakan screenshot situs |
| Talos IP & Domain Reputation Center | Menilai reputasi IP, domain, dan network berdasarkan data threat intelligence Cisco |

---

**4. Analisis Body Email**

**4.1 Ekstraksi URL**

- Metode manual: klik kanan pada link, pilih **Copy link address** (nama opsi bervariasi tergantung browser/email client), untuk melihat tujuan URL tanpa mengunjunginya.
- Metode otomatis: menggunakan **URL extraction tool** yang mem-parsing seluruh link tertanam dari raw email content. **CyberChef** adalah salah satu tool serbaguna yang bisa menjalankan fungsi ini.

**4.2 Analisis Attachment**

Attachment sebaiknya hanya didownload di lingkungan terkontrol (lab machine/sandbox) untuk mencegah eksekusi tidak disengaja.

Menghasilkan hash file dengan command:

```
sha256sum nama_file.pdf
```

Command ini menghitung nilai hash SHA256 dari file yang diberikan sebagai argumen, digunakan untuk pengecekan reputasi file di platform threat intelligence tanpa perlu mengunggah file itu sendiri.

Hash yang dihasilkan kemudian dicocokkan di platform seperti Talos atau VirusTotal untuk mengetahui apakah file tersebut sudah pernah teridentifikasi sebagai malicious, phishing, atau spam.

---

**5. Malware Sandbox**

Sandbox memungkinkan analis mengeksekusi file/URL mencurigakan dalam lingkungan terisolasi untuk mengamati perilakunya (URL yang dihubungi, payload tambahan yang di-download, indicator of compromise) tanpa mempertaruhkan sistem analis sendiri.

| Tool | Karakteristik |
|---|---|
| ANY.RUN | Sandbox interaktif real-time; analis bisa berinteraksi langsung dengan environment, memonitor proses, melihat network activity |
| Hybrid Analysis | Sandbox gratis dengan upload file atau submit URL, menghasilkan insight system changes, network activity, dan IOC |
| JOESandbox | Melakukan analisis statis dan dinamis; mendukung berbagai arsitektur (Windows, macOS, Android, Linux); menghasilkan laporan komprehensif |

**5.1 Struktur Analisis pada ANY.RUN**

Hasil analisis ANY.RUN menampilkan beberapa komponen kunci:

- **Verdict banner** — klasifikasi keseluruhan file/URL, contoh: *No threats detected*, *Suspicious activity*, atau *Malicious activity*.
- **Static Discovering** — popup detail file berisi TrID file identifier, hash lengkap (MD5, SHA1, SHA256, SSDEEP), serta metadata EXIF (author, creator, tanggal pembuatan). Metadata ini bisa menjadi indikator tambahan, misalnya author yang tidak sesuai dengan brand yang dipalsukan.
- **Processes** — daftar proses yang berjalan selama eksekusi file, lengkap dengan PID dan command line masing-masing.
- **HTTP Requests** — daftar request HTTP yang dilakukan proses, termasuk URL tujuan dan proses yang memicunya.
- **Connections** — daftar koneksi jaringan (IP, port, domain, ASN) beserta kolom **Rep** (reputation) yang menandai status koneksi.
- **DNS Requests** — daftar domain yang di-resolve beserta IP hasil resolusinya; ini menjadi tempat tercepat untuk mencari IP dari sebuah domain yang dicurigai.
- **Network Threats** — daftar koneksi yang secara eksplisit di-flag sebagai ancaman, disertai class (contoh: *Potentially Bad Traffic*) dan pesan detail (contoh: ET INFO TLS Handshake Failure).
- **Tags/Indicators** — label ringkas yang merangkum sifat file, contoh: *trojan*, *exploit*, atau nomor **CVE** spesifik yang dieksploitasi.

**5.2 Contoh Kerentanan yang Dieksploitasi**

**CVE-2017-11882** adalah kerentanan pada Microsoft Office Equation Editor (proses `EQNEDT32.EXE`) yang memungkinkan remote code execution. Meskipun tergolong lama, CVE ini masih sering dimanfaatkan dalam kampanye phishing karena banyak sistem belum di-patch. Pola serangan yang memanfaatkan CVE ini umumnya melibatkan file Office (xlsx/doc/rtf) yang memicu proses EQNEDT32.EXE untuk mengunduh payload dari domain eksternal, sering melalui rantai redirect (multiple domain redirect) sebelum payload akhir diunduh.

---

**6. PhishTool sebagai Platform Investigasi Terpusat**

PhishTool mengotomatisasi proses analisis email phishing dengan menggabungkan threat intelligence, OSINT, metadata email, dan automated workflow dalam satu platform.

**6.1 Tampilan dan Fitur Utama**

- **Rendered/HTML/Source view** — menampilkan email dalam tiga mode: tampilan seperti inbox (rendered), raw HTML, dan message source.
- **Authentication tab** — menampilkan hasil SPF, DKIM, DMARC.
- **Transmission tab** — menampilkan hop-hop routing email dari server asal hingga tujuan akhir.
- **URL Analysis tab** — menampilkan seluruh embedded URL beserta skor deteksi VirusTotal masing-masing.
- **Integrasi VirusTotal** — memungkinkan analis melihat reputasi dan hasil deteksi dari puluhan security vendor langsung di dalam PhishTool tanpa berpindah platform.

**6.2 Resolving the Case**

Setelah analisis selesai, PhishTool memungkinkan dokumentasi formal berupa:

- **Disposition** — status akhir kasus (contoh: Malicious).
- **Flagged artifacts** — daftar artifact yang ditandai (from address, from domain, return-path, originating IP, message URL).
- **Classification codes** — kode klasifikasi standar (contoh: MAL_URL untuk Malicious URL).
- **Notes** — catatan investigasi bebas.
- Diakhiri dengan tombol **Resolve** untuk menutup kasus, mencerminkan proses closure di lingkungan SOC nyata.

---

**7. Indikator Umum Email Phishing (dari Studi Kasus)**

Berdasarkan kasus-kasus yang dianalisis dalam room ini (email Netflix palsu, attachment PDF "Payment update", attachment Excel dengan CVE-2017-11882), pola indikator yang konsisten muncul:

- **Brand impersonation** — display name dan konten email meniru brand ternama (Netflix, PayPal) untuk membangun kepercayaan korban.
- **Domain mismatch** — domain di header From tidak konsisten dengan domain sebenarnya yang terlihat di field **Return-Path** atau hasil SPF check (`smtp.mailfrom`), yang lebih sulit dipalsukan dibanding tampilan From.
- **Call to action mendesak** — subjek dan isi email mendorong tindakan segera (update pembayaran, klaim hadiah, verifikasi akun).
- **Shortened/obfuscated URL** — tombol atau link menggunakan URL shortener (contoh: t.co) untuk menyembunyikan tujuan asli.
- **Metadata mencurigakan pada attachment** — author/creator file tidak sesuai dengan brand yang ditiru, indikasi file di-reuse dari template phishing kit lain.
- **Redirect chain** — payload berbahaya diakses melalui beberapa lapis redirect domain sebelum sampai ke tujuan akhir (download executable, halaman phishing).

---

**8. Glossary**

- **Artifact** — jejak/data spesifik dalam email (alamat, IP, URL, hash) yang dikumpulkan sebagai bahan investigasi.
- **SPF** — mekanisme validasi server pengirim yang berwenang atas suatu domain.
- **DKIM** — tanda tangan digital untuk memverifikasi integritas isi email.
- **DMARC** — kebijakan penindaklanjutan hasil SPF/DKIM.
- **Reverse lookup** — proses mencari informasi domain/hostname dari sebuah IP address.
- **IOC (Indicator of Compromise)** — jejak/bukti yang menunjukkan adanya aktivitas berbahaya dalam sistem.
- **Sandbox** — lingkungan terisolasi untuk mengeksekusi file/URL mencurigakan dengan aman.
- **Hash (SHA256, MD5, SHA1)** — nilai unik yang dihasilkan dari isi file, dipakai untuk identifikasi dan pengecekan reputasi tanpa membagikan file itu sendiri.
- **SSDEEP** — fuzzy hashing yang dipakai untuk mendeteksi file yang mirip meski tidak identik secara byte-per-byte.
- **CVE (Common Vulnerabilities and Exposures)** — identifikasi standar untuk kerentanan keamanan yang telah diketahui publik.
- **Return-Path** — header email yang menunjukkan alamat tujuan bounce/error message, sering mengungkap domain asli pengirim.
- **Typosquatting** — teknik membuat domain mirip dengan domain resmi (contoh: capitai-one.com meniru capital-one.com) untuk mengelabui korban.
- **Redirect chain** — rangkaian pengalihan (302 Found) dari satu URL ke URL lain sebelum mencapai tujuan akhir.
- **Disposition** — status klasifikasi akhir dari sebuah kasus investigasi (contoh: Malicious, Benign).

---

**9. Tools & Platform Rujukan**

- **Messageheader (Google Admin Toolbox)** — analisis header email untuk mengekstrak IP, routing path, dan misconfiguration.
- **Message Header Analyzer** — alternatif tool analisis header email dengan fungsi serupa.
- **IPinfo** — lookup informasi geografis dan organisasi dari sebuah IP address.
- **URLScan.io** — investigasi URL secara aman tanpa mengunjunginya langsung, disertai screenshot situs.
- **Talos IP & Domain Reputation Center** — pengecekan reputasi IP, domain, network, dan hash file dari Cisco.
- **VirusTotal** — pengecekan reputasi file, URL, IP, dan domain menggunakan data puluhan security vendor.
- **CyberChef** — tool serbaguna untuk ekstraksi URL dan berbagai transformasi data lainnya.
- **ANY.RUN** — malware sandbox interaktif real-time untuk analisis file dan URL mencurigakan.
- **Hybrid Analysis** — malware sandbox gratis untuk analisis file/URL.
- **JOESandbox** — sandbox untuk analisis statis dan dinamis lintas platform (Windows, macOS, Android, Linux).
- **PhishTool** — platform terpusat untuk investigasi phishing, mengintegrasikan header analysis, URL analysis, dan VirusTotal.