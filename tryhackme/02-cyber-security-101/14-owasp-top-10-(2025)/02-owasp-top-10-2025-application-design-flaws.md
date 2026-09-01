# Resume: OWASP Top 10 2025 — Application Architecture Flaws
**Tanggal:** 1 September 2026

---

## 1. Gambaran Umum Room

Room ini membahas empat kategori dari OWASP Top 10 2025 yang berkaitan dengan kegagalan arsitektur dan desain sistem:

1. **AS02 — Security Misconfigurations**
2. **AS03 — Software Supply Chain Failures**
3. **AS04 — Cryptographic Failures**
4. **AS06 — Insecure Design**

Kesimpulan besar dari seluruh room: keempat kategori ini punya akar penyebab yang sama, yaitu fondasi keamanan yang lemah sejak awal. Keamanan tidak bisa "ditempel" belakangan setelah sistem selesai dibangun — harus jadi bagian dari requirement, konfigurasi, dependency, dan pilihan kriptografi sejak tahap desain.

---

## 2. AS02 — Security Misconfigurations

### 2.1 Definisi

Kesalahan konfigurasi keamanan terjadi ketika sistem, server, atau aplikasi di-deploy dengan pengaturan default yang tidak aman, pengaturan yang belum lengkap, atau layanan yang terekspos ke publik. Ini bukan bug pada kode, melainkan kesalahan pada cara environment atau software disiapkan.

### 2.2 Kenapa Penting

Kesalahan konfigurasi sekecil apa pun bisa mengekspos data sensitif, memungkinkan eskalasi privilese, atau memberi penyerang pijakan awal ke sistem. Semakin kompleks stack aplikasi (cloud, API pihak ketiga, container), semakin banyak titik yang berpotensi salah konfigurasi.

### 2.3 Studi Kasus: Uber (2017)

Uber mengekspos backup **AWS S3 bucket** berisi data sensitif driver dan penumpang karena bucket tersebut dapat diakses publik tanpa kredensial. Ini menunjukkan bagaimana satu kesalahan deployment bisa berujung kebocoran data besar.

### 2.4 Pola Umum

- Kredensial default atau password lemah yang tidak diganti
- Layanan atau endpoint yang tidak perlu tapi terekspos ke internet
- Cloud storage/permission yang salah konfigurasi (S3, Azure Blob, GCP)
- API tanpa pembatasan akses atau autentikasi/otorisasi
- Pesan error yang terlalu detail (verbose), membocorkan stack trace atau path sistem
- Software/framework/container usang dengan kerentanan yang sudah diketahui
- Endpoint AI/ML tanpa kontrol akses yang layak

### 2.5 Cara Mencegah

- Harden konfigurasi default, hapus fitur/layanan yang tidak dipakai
- Autentikasi kuat dan prinsip least privilege di semua sistem
- Batasi eksposur jaringan, segmentasi resource sensitif
- Patch software/framework/container secara rutin
- Sembunyikan stack trace dan info sistem dari pesan error
- Audit konfigurasi cloud dan permission secara berkala
- Integrasikan review konfigurasi ke pipeline deployment

### 2.6 Kasus Praktik di Room

Target berupa **User Management API** dengan endpoint `GET /api/user/<user_id>` yang mensyaratkan ID numerik. Ketika ID non-numerik dikirim (misalnya string kosong atau karakter khusus), validasi input gagal ditangani dengan aman: aplikasi mengembalikan **stack trace lengkap** (`Traceback`, path file server) beserta objek `debug_info` yang secara eksplisit menyertakan flag rahasia di dalam pesan error.

Pelajaran kunci: verbose error message adalah kebocoran informasi yang serius — pesan error produksi harus generik, tidak boleh menampilkan detail internal sistem.

---

## 3. AS03 — Software Supply Chain Failures

### 3.1 Definisi

Kegagalan rantai pasok software terjadi ketika aplikasi bergantung pada komponen, library, layanan, atau model yang sudah dikompromikan, usang, atau tidak diverifikasi. Kelemahan bukan berasal dari kode sendiri, melainkan dari komponen pihak ketiga yang dipakai.

### 3.2 Kenapa Penting

Satu dependency yang terkompromi bisa mengkompromikan seluruh sistem tanpa penyerang perlu menyentuh kode aplikasi sama sekali. Serangan supply chain bisa diotomatisasi dan didistribusikan luas, sehingga sulit dideteksi.

### 3.3 Studi Kasus: SolarWinds (2021)

Penyerang menyisipkan kode berbahaya ke dalam update resmi **SolarWinds Orion** yang otomatis terinstal di ribuan organisasi. Masalahnya bukan pada logika inti software, melainkan pada proses build, verifikasi, dan distribusi update.

Terkait AI: model pihak ketiga yang tidak diverifikasi atau dataset fine-tuning yang tidak aman bisa menyisipkan perilaku tersembunyi, backdoor, atau output bias.

### 3.4 Pola Umum

- Library/dependency yang tidak terverifikasi atau tidak terawat (unmaintained)
- Update otomatis tanpa verifikasi
- Ketergantungan berlebih pada model AI pihak ketiga tanpa monitoring
- Build pipeline / CI/CD yang tidak aman, rentan tampering
- Pelacakan lisensi dan provenance komponen yang buruk
- Kurangnya monitoring kerentanan dependency pasca-deployment

### 3.5 Cara Melindungi

- Verifikasi semua komponen/library/model AI sebelum digunakan
- Monitor dan patch dependency secara rutin
- Tanda tangani (sign) dan audit update/package
- Lock down pipeline CI/CD dan proses build
- Lacak provenance dan lisensi setiap dependency
- Monitoring runtime untuk perilaku tidak biasa dari dependency
- Integrasikan threat modelling supply chain ke seluruh SDLC

### 3.6 Kasus Praktik di Room

Target berupa Flask API ("Data Processing Service") dengan endpoint `POST /api/process` yang mengimpor fungsi dari library lokal tidak terverifikasi (`lib/vulnerable_utils.py`). Di dalam logika endpoint tersembunyi kondisi trigger:

```python
if data == 'debug':
    return jsonify(debug_info())
```

Mengirim request `POST /api/process` dengan body `{"data": "debug"}` memicu pemanggilan fungsi `debug_info()` dari library rentan tersebut, yang mengembalikan data sensitif (`admin_token`, `internal_secret`, dan flag).

Pelajaran kunci: developer sering mempercayai fungsi dari library pihak ketiga tanpa tahu isi implementasinya — inilah inti dari supply chain failure.

**Command/tool yang dipakai:**

Menyusun request POST manual di Burp Suite (tab **Repeater**) karena browser hanya bisa mengirim `GET` lewat address bar, sedangkan endpoint butuh method `POST` dengan body JSON.

Header penting: `Content-Type: application/json` — wajib disertakan agar server (Flask) berhasil melakukan parsing `request.json.get()`; tanpa header ini server mengembalikan error `415 Unsupported Media Type`.

Body request: `{"data": "debug"}` — format JSON dengan key `data` dan value `debug`, sesuai dengan kondisi yang dibaca kode Python di server.

Saat menyusun request baru di Repeater, Burp meminta konfigurasi target (host, port, protokol) melalui dialog **"Configure target details"** — host diisi IP target, port diisi sesuai port aplikasi, dan opsi HTTPS dimatikan jika target berjalan di HTTP biasa.

---

## 4. AS04 — Cryptographic Failures

### 4.1 Definisi

Kegagalan kriptografi terjadi ketika enkripsi digunakan secara tidak tepat atau tidak digunakan sama sekali — termasuk algoritma lemah, kunci hardcoded, penanganan kunci buruk, atau data sensitif yang tidak dienkripsi.

### 4.2 Kenapa Penting

Aplikasi web bergantung pada kriptografi untuk melindungi traffic, mengamankan data tersimpan, memverifikasi identitas, dan menjaga rahasia. Kegagalan di area ini bisa berujung pengambilalihan akun atau kebocoran besar-besaran. Penyerang bisa mengeksploitasinya lewat man-in-the-middle, brute-force kunci lemah, atau menemukan rahasia yang memang tidak pernah dilindungi.

### 4.3 Perbedaan Fundamental: Encoding vs Enkripsi vs Hash

Ini adalah konsep krusial yang sering disalahpahami (dan menjadi inti pelajaran kategori ini):

**Encoding** — mengubah representasi data agar kompatibel dengan format tertentu (misalnya biner menjadi teks). Tidak ada kunci rahasia. Siapa pun yang tahu metodenya bisa langsung membalikkan (decode) tanpa syarat apa pun. Contoh: **Base64**.

**Enkripsi** — mengunci data dengan kunci rahasia (key). Untuk membalikkan (dekripsi), kunci yang benar wajib dimiliki. Tanpa kunci, data tidak bisa dibaca meski algoritmanya diketahui.

**Hash** — fungsi satu arah, tidak bisa "dibalik" sama sekali. Untuk menemukan input asli dari sebuah hash, harus dilakukan tebak-tebakan (brute-force/cracking) menggunakan tool seperti John the Ripper atau Hashcat — berbeda total dengan encoding maupun enkripsi.

Kesalahan umum developer: menyebut sesuatu "encrypted" padahal implementasinya cuma encoding (seperti Base64), sehingga data yang diklaim rahasia sebenarnya bisa dibaca siapa saja tanpa kunci apa pun.

### 4.4 Pola Umum

- Algoritma usang/lemah: **MD5**, **SHA-1**, mode **ECB**
- Rahasia (secrets) yang di-hardcode di kode atau konfigurasi
- Praktik rotasi/manajemen kunci yang buruk
- Data sensitif tidak dienkripsi saat disimpan (at rest) atau saat transit
- Sertifikat TLS self-signed atau tidak valid
- Sistem AI/ML tanpa penanganan rahasia yang layak untuk parameter model

### 4.5 Cara Mencegah

- Gunakan algoritma modern: **AES-GCM**, **ChaCha20-Poly1305**, TLS 1.3 dengan sertifikat valid
- Gunakan layanan manajemen kunci: Azure Key Vault, AWS KMS, HashiCorp Vault
- Rotasi kunci/rahasia secara berkala mengikuti periode kripto yang ditentukan
- Dokumentasikan kebijakan manajemen siklus hidup kunci
- Inventarisasi lengkap sertifikat, kunci, dan pemiliknya
- Pastikan model AI tidak pernah mengekspos rahasia tanpa enkripsi

### 4.6 Kasus Praktik di Room

Target berupa "Secure Document Viewer" yang menampilkan dokumen "terenkripsi" dalam bentuk string Base64. Decode Base64 awal menghasilkan data biner acak (bukan teks), menandakan lapisan Base64 hanya pembungkus luar dari data yang benar-benar dienkripsi di dalamnya.

Investigasi lebih lanjut (Inspect Element / cek file JavaScript yang dimuat halaman) menemukan file `decrypt.js` berisi konfigurasi hardcoded:

```javascript
const SECRET_KEY = "my-secret-key-16";
const ENCRYPTION_MODE = "ECB";
const KEY_SIZE = 128;
```

Ini adalah kombinasi dua pelanggaran cryptographic failure sekaligus: penggunaan **mode ECB** (mode AES paling lemah, pola blok yang sama menghasilkan ciphertext yang sama) dan **kunci hardcoded** yang bisa diakses siapa saja lewat source code sisi client.

**Alur teknis penyelesaian dan command yang dipakai:**

1. **Decode Base64 ke file biner:**
```bash
echo "<string_base64>" | base64 -d > encrypted.bin
```
`base64 -d` — melakukan decode dari representasi Base64 kembali ke data biner asli. Hasil disimpan ke file (bukan dicetak langsung) karena isinya data biner.

2. **Verifikasi ukuran file:**
```bash
ls -la encrypted.bin
```
Ukuran file harus kelipatan 16 byte (block size AES 128-bit) sebagai indikasi bahwa data memang terenkripsi AES.

3. **Konversi key string ke representasi hex:**
```bash
echo -n "my-secret-key-16" | xxd -p
```
`echo -n` — mencetak string tanpa newline di akhir, penting agar tidak ada karakter tambahan yang ikut terkonversi. `xxd -p` — mengubah data menjadi representasi hex mentah (plain, tanpa spasi/formatting).

4. **Dekripsi AES dengan OpenSSL:**
```bash
openssl enc -aes-128-ecb -d -K <key_hex> -in encrypted.bin -out decrypted.bin
```
`-aes-128-ecb` — menentukan cipher, key size, dan mode sekaligus dalam satu nama flag. `-d` — mode decrypt. `-K` (huruf besar) — key dalam bentuk hex, dipakai langsung sebagai raw bytes (berbeda dari `-k` huruf kecil yang menerima passphrase teks biasa dan melewati proses derivasi tambahan). `-in` — file input (harus berupa file, bukan string data langsung). `-out` — file output hasil dekripsi.

5. **Membaca hasil dekripsi:**
```bash
cat decrypted.bin
```

Konsep permission file Linux yang relevan saat verifikasi file (`ls -la`): notasi `-rw-rw-r--` terdiri dari 10 karakter — karakter pertama menandakan tipe file (`-` = file biasa, `d` = direktori), diikuti 3 kelompok 3 karakter untuk **owner**, **group**, dan **others**, masing-masing merepresentasikan izin **read (r)**, **write (w)**, **execute (x)**.

---

## 5. AS06 — Insecure Design

### 5.1 Definisi

Desain tidak aman terjadi ketika logika atau arsitektur yang cacat sudah tertanam dalam sistem sejak awal — akibat threat modelling yang dilewatkan, tidak adanya requirement/review desain, atau kesalahan tidak disengaja. Kehadiran AI assistant memperparah masalah ini karena developer sering berasumsi model AI aman, benar, atau bisa diprediksi.

### 5.2 Kenapa Penting

Desain tidak aman tidak bisa di-patch — ia sudah tertanam dalam alur kerja, logika, dan trust boundaries sistem. Memperbaikinya berarti memikirkan ulang cara sistem (dan AI di dalamnya) mengambil keputusan.

### 5.3 Studi Kasus: Clubhouse

Desain awal Clubhouse mengasumsikan user hanya akan berinteraksi lewat aplikasi mobile resmi, sehingga **backend API tidak diberi autentikasi yang layak**. Siapa saja bisa melakukan query langsung terhadap data user, informasi room, bahkan percakapan privat, karena backend memercayai begitu saja bahwa hanya app resmi yang akan memanggilnya.

### 5.4 Pola Insecure Design yang Umum (2025)

- Kontrol logika bisnis lemah (alur recovery/approval)
- Asumsi cacat tentang perilaku user atau model AI
- Komponen AI dengan otoritas/akses yang tidak diperiksa
- Guardrail hilang untuk LLM dan agen otomasi
- Bypass testing/debug yang tertinggal di production
- Tidak ada review abuse-case atau threat modelling AI yang konsisten

### 5.5 Insecure Design di Era AI

**Prompt injection** — input user dicampur dengan system prompt, memungkinkan penyerang membajak konteks atau mengekstrak data tersembunyi. Kepercayaan buta pada output model menciptakan sistem rapuh yang bertindak tanpa validasi/pengawasan manusia. Model yang "diracuni" (poisoned) — diambil dari sumber tidak terverifikasi atau di-fine-tune dengan data tidak aman — bisa menanamkan perilaku tersembunyi/backdoor.

### 5.6 Cara Mendesain dengan Aman

- Perlakukan setiap model AI sebagai **tidak terpercaya** sampai terbukti sebaliknya
- Validasi dan filter semua input/output model
- Pisahkan system prompt dari konten user
- Jauhkan data sensitif dari prompt kecuali benar-benar diperlukan
- Wajibkan review manusia untuk aksi AI berisiko tinggi
- Log provenance model, monitor perilaku, terapkan differential privacy
- Threat modelling spesifik-AI (serangan prompt, risiko inferensi, penyalahgunaan agen, supply chain) di sepanjang siklus desain, bukan hanya di awal
- Definisikan requirement keamanan sebelum implementasi setiap fitur
- Terapkan least privilege di semua user/API/layanan
- Pastikan autentikasi, otorisasi, dan manajemen sesi yang layak
- Verifikasi dan perbarui dependency serta sumber supply chain
- Monitor dan uji berkelanjutan untuk cacat logika dan abuse path

### 5.7 Kasus Praktik di Room

Target berupa aplikasi "SecureChat" yang mengklaim "designed exclusively for mobile devices" dan memblokir akses browser desktop dengan halaman landing statis ("Download the App"). Klaim ini murni asumsi tekstual di halaman depan — tidak ada mekanisme teknis nyata yang memvalidasinya (percobaan mengubah header `User-Agent` ke berbagai variasi mobile/Android/iPhone terbukti tidak berpengaruh sama sekali, karena backend memang tidak memeriksa User-Agent).

Masalah sebenarnya ada di **backend API** yang sama sekali tidak dilindungi autentikasi:

- `GET /api/users` — ditemukan lewat directory brute-forcing, mengembalikan daftar user beserta role (`admin`, `user1`, `user2`)
- `GET /api/messages/<username>` — pola **IDOR (Insecure Direct Object Reference)**: endpoint menerima username sebagai parameter path tanpa validasi otorisasi, sehingga siapa pun bisa mengakses pesan milik user mana pun (termasuk `admin`) hanya dengan menebak/mengetahui username-nya. Flag ditemukan di dalam pesan milik `admin` dan `user1`.

Pelajaran kunci: ini persis pola Clubhouse — developer membangun "penjagaan" di pintu depan (landing page desktop-blocker) tapi lupa bahwa "pintu belakang" (backend API) sama sekali tidak dijaga. Insecure design tidak selalu soal mekanisme deteksi yang lemah, tapi soal asumsi keliru mengenai siapa saja yang bisa dan akan mengakses sistem.

**Command/tool yang dipakai:**

```bash
gobuster dir -u http://<target>:<port> -w /usr/share/wordlists/dirb/common.txt -t 50
```

`gobuster dir` — menjalankan gobuster dalam mode pencarian direktori/path (gobuster juga punya mode lain: `dns` untuk subdomain enumeration, `vhost` untuk virtual host enumeration). `-u` — URL target lengkap dengan port. `-w` — path ke wordlist yang dipakai (berisi daftar nama path umum untuk dicoba). `-t 50` — jumlah thread paralel (default 10), mempercepat proses scanning dengan mengirim banyak request bersamaan.

Flag tambahan yang relevan (opsional, memperlambat proses): `-x php,html,txt,json` — mencoba variasi ekstensi file di setiap kata wordlist.

Tools sejenis yang setara secara fungsi: **dirb**/**dirbuster** (directory brute-forcing generasi lebih lama, lebih lambat dari gobuster), **ffuf** (fuzzing serba guna — bisa dipakai untuk path, parameter, header, lebih fleksibel tapi lebih kompleks).

Catatan: **Metasploit** bukan tool yang tepat untuk directory enumeration — Metasploit adalah framework eksploitasi kerentanan yang sudah diketahui (CVE), bukan tool untuk menemukan endpoint tersembunyi di aplikasi web custom.

---

## 6. Glossary (Istilah Wajib Dihapal)

**Security Misconfiguration** — kesalahan pada pengaturan/deployment sistem (bukan bug kode) yang menciptakan celah keamanan.

**Supply Chain Failure** — kerentanan yang berasal dari komponen/dependency pihak ketiga yang tidak terverifikasi, bukan dari kode sendiri.

**Cryptographic Failure** — kegagalan penggunaan enkripsi: algoritma lemah, kunci hardcoded, atau data sensitif tanpa proteksi.

**Insecure Design** — cacat arsitektur/logika yang tertanam sejak proses desain, tidak bisa diperbaiki hanya dengan patch.

**Encoding** — perubahan representasi data yang reversibel tanpa kunci (contoh: Base64).

**Enkripsi** — penguncian data dengan kunci rahasia; hanya bisa dibalik dengan kunci yang benar.

**Hash** — fungsi satu arah; tidak bisa dibalik, hanya bisa ditebak lewat brute-force.

**ECB (Electronic Codebook)** — mode enkripsi blok yang lemah karena blok data identik menghasilkan ciphertext identik, memudahkan analisis pola.

**Hardcoded Secret** — kunci/kredensial rahasia yang ditulis langsung di source code, bukan dikelola lewat sistem manajemen rahasia terpisah.

**Verbose Error Message** — pesan error yang membocorkan detail internal sistem (stack trace, path file) yang seharusnya disembunyikan dari pengguna.

**IDOR (Insecure Direct Object Reference)** — kerentanan di mana sebuah endpoint menerima identifier (seperti username/ID) langsung dari client tanpa validasi otorisasi, sehingga bisa dipakai mengakses data milik pihak lain.

**Directory/Endpoint Brute-forcing** — teknik mencoba banyak kemungkinan nama path secara otomatis untuk menemukan endpoint yang tidak terdaftar di halaman publik.

**Prompt Injection** — teknik serangan pada sistem AI di mana input user dicampur dengan system prompt untuk membajak konteks atau mengekstrak data tersembunyi.

**Model Poisoning** — penyisipan perilaku tersembunyi/backdoor ke dalam model AI melalui sumber data yang tidak terverifikasi atau proses fine-tuning yang tidak aman.

**Trust Boundary** — batas kepercayaan dalam sistem yang menentukan data/aksi mana yang boleh dipercaya tanpa validasi tambahan.

**Least Privilege** — prinsip memberikan akses seminimal mungkin sesuai kebutuhan, bukan akses penuh secara default.

**Threat Modelling** — proses identifikasi potensi ancaman terhadap sistem sejak tahap desain, dilakukan berkelanjutan bukan hanya sekali di awal.

---

## 7. Tools & Platform Rujukan

**Burp Suite (Repeater)** — menyusun dan mengirim ulang request HTTP secara manual (termasuk mengubah method, header, dan body) untuk menguji perilaku server. Tidak ada URL spesifik disebutkan di materi (tool lokal Kali Linux).

**OpenSSL** — command-line tool untuk operasi kriptografi, termasuk enkripsi/dekripsi data (`openssl enc`). Tool bawaan sistem, tidak ada URL.

**Gobuster** — tool directory/subdomain/vhost brute-forcing berbasis Go, digunakan untuk menemukan path tersembunyi di web server. Tool bawaan Kali Linux.

**Dirb / Dirbuster** — tool directory brute-forcing generasi lebih lama, fungsi setara gobuster mode `dir`. Tool bawaan Kali Linux.

**FFUF (Fuzz Faster U Fool)** — tool fuzzing serba guna untuk path, parameter, dan header. Tool bawaan Kali Linux.

**CyberChef** — tool web untuk eksperimen encoding/decoding dan operasi data (Base64, hex, dsb) secara visual. Tidak disertakan URL eksplisit di materi, umumnya diakses di gchq.github.io/CyberChef.

**xxd** — utility Linux untuk konversi data ke/dari representasi hex.

**base64 (CLI)** — utility Linux bawaan untuk encode/decode Base64.

---

## 8. Catatan Ringkas untuk Ditulis Tangan

**AS02 — Security Misconfigurations**
- Definisi — kesalahan setup/deployment, bukan bug kode
- Contoh nyata — Uber 2017, S3 bucket publik
- Pola — kredensial default, service exposed, verbose error, software usang
- Fix — harden default, least privilege, patch rutin, sembunyikan stack trace
- Praktik — API ID non-numerik bocorkan stack trace + flag di debug_info

**AS03 — Software Supply Chain Failures**
- Definisi — kerentanan dari dependency pihak ketiga, bukan kode sendiri
- Contoh nyata — SolarWinds 2021, update resmi disusupi
- Pola — library unverified, auto-update tanpa cek, CI/CD tidak aman
- Fix — verifikasi dependency, sign & audit update, lock CI/CD
- Praktik — trigger tersembunyi `if data == 'debug'` di endpoint, panggil fungsi vulnerable_utils
- Tool — Burp Repeater, header wajib `Content-Type: application/json`, body `{"data":"debug"}`

**AS04 — Cryptographic Failures**
- Definisi — enkripsi salah pakai atau tidak dipakai sama sekali
- Contoh nyata — konsep umum (bukan studi kasus perusahaan spesifik di sini)
- Encoding vs Enkripsi vs Hash — encoding reversibel tanpa kunci; enkripsi butuh kunci; hash satu arah/tidak bisa dibalik
- Pola — MD5/SHA-1/ECB, hardcoded key, TLS self-signed
- Fix — AES-GCM/ChaCha20, TLS 1.3, key management service (Vault/KMS)
- Praktik — Base64 luar, AES-128-ECB di dalam, key hardcoded di JS
- Command — `base64 -d`, `xxd -p`, `openssl enc -aes-128-ecb -d -K <hex> -in -out`
- Permission file — `-rw-rw-r--` = tipe file + owner + group + others (r/w/x)

**AS06 — Insecure Design**
- Definisi — cacat arsitektur tertanam sejak desain, tidak bisa di-patch
- Contoh nyata — Clubhouse, backend API tanpa auth karena asumsi cuma app mobile
- Pola — logika bisnis lemah, asumsi user/model salah, guardrail AI hilang
- Era AI — prompt injection, blind trust ke output model, model poisoning
- Fix — treat model untrusted, validasi input/output, human review aksi high-risk
- Praktik — landing page blokir desktop cuma tampilan; backend `/api/users` & `/api/messages/<user>` tanpa auth = IDOR
- Tool — `gobuster dir -u <url> -w /usr/share/wordlists/dirb/common.txt -t 50`

**Istilah wajib hapal**
- IDOR — akses data lewat identifier tanpa cek otorisasi
- Trust boundary — batas kepercayaan sistem
- Least privilege — akses minimal sesuai kebutuhan
- Threat modelling — identifikasi ancaman sejak desain, berkelanjutan
- Verbose error — pesan error bocorkan detail internal

**Kesimpulan besar room**
- 4 kategori (AS02/AS03/AS04/AS06) akar masalahnya sama: fondasi lemah
- Security requirement, threat assumption realistis, config terkontrol, dependency vetted, pilihan kripto tepat
- Treat default suspicious, treat dependency as risk, keep design simple
