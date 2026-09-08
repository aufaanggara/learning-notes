# Resume Materi: Offensive Security Intro
**TryHackMe | 08 Sep 2026**

---

## 1. Konsep Inti Offensive Security

**Offensive security** adalah pendekatan pengujian sistem secara proaktif dari sudut pandang penyerang, dengan tujuan menemukan kelemahan sebelum hacker jahat mengeksploitasinya.

Cara kerjanya dimulai dari pertanyaan-pertanyaan berikut:
- Apa yang terekspos ke publik?
- Apa yang bisa diakses tanpa izin?
- Asumsi apa yang dibuat sistem ini yang bisa disalahgunakan?

Syarat mutlak: setiap aktivitas harus mendapat **izin (permission)** dari pemilik sistem. Tanpa izin, aktivitas yang sama menjadi ilegal — tidak peduli niatnya baik.

Dalam konteks room ini, kata "hacking" merujuk pada **penetration testing**: metode yang etis, legal, dan terstruktur untuk menemukan kelemahan. Hacker di sini adalah seseorang yang menggunakan keahliannya secara positif untuk meningkatkan keamanan sistem, bukan merusaknya.

---

## 2. Terminologi Kunci

**Red Teaming**
Metodologi serangan terstruktur dan terotorisasi yang mensimulasikan musuh nyata. Tujuannya menguji efektivitas pertahanan dan menemukan kerentanan dalam scope yang sudah ditentukan.

**Penetration Test**
Penilaian keamanan terstruktur oleh tester yang diotorisasi. Mencoba mengidentifikasi dan mengeksploitasi vulnerability dalam scope tertentu untuk memahami risiko di dunia nyata.

**Vulnerability**
Kelemahan atau cacat pada sistem, aplikasi, atau konfigurasi yang bisa disalahgunakan penyerang untuk masuk atau mengambil data.

**Exploit**
Teknik atau metode konkret yang memanfaatkan sebuah vulnerability untuk mencapai tujuan tertentu, seperti mengakses data yang dibatasi atau mengambil alih fungsi sistem.

**Scope**
Batasan yang mendefinisikan apa yang boleh diuji selama engagement — sistem mana, aplikasi mana, dan tindakan apa yang diizinkan. Segala sesuatu di luar scope adalah off-limits.

**Enumeration**
Proses mengumpulkan detail tentang sistem, pengguna, dan layanan yang berjalan. Output dari enumerasi digunakan untuk menemukan titik lemah yang bisa dieksploitasi.

**Credentials**
Detail login berupa username dan password yang membuka akses ke sistem atau aplikasi.

**Authentication**
Langkah verifikasi identitas saat seseorang atau sistem mencoba login — membuktikan bahwa mereka benar-benar siapa yang mereka klaim.

**Dictionary Attack**
Teknik serangan yang menggunakan wordlist yang sudah ditentukan sebelumnya untuk menebak password atau username secara sistematis.

---

## 3. Aturan Kritis: Permission

Semua istilah dan teknik dalam offensive security berbagi satu aturan yang tidak bisa dikompromikan: **permission (izin)**.

Ethical hacking adalah pengujian sistem secara terkontrol dan legal. Ethical hacker diizinkan secara eksplisit untuk menguji sistem dalam scope yang sudah disepakati — menjadikan seluruh aktivitas intentional dan safe.

Di dunia nyata, organisasi menyewa penetration tester atau red team untuk mensimulasikan serangan terhadap sistem mereka sendiri. Tujuannya bukan menyebabkan kerusakan, melainkan:
- Menguji kekuatan kontrol dan pertahanan keamanan yang ada
- Mengungkap celah yang belum diketahui
- Membantu tim meningkatkan postur keamanan secara keseluruhan

---

## 4. Mindset: Think Like a Hacker

Untuk efektif dalam offensive security, seseorang harus berpikir melampaui pertanyaan "apakah ini bekerja?" dan mulai bertanya "bagaimana ini bisa disalahgunakan?"

Ada empat poin kunci yang harus diinternalisasi:

**Ask questions**
Jangan asumsikan sebuah fitur bekerja persis seperti yang dimaksudkan developer. Selalu tanyakan: "Bagaimana jika fitur ini tidak bekerja seperti yang diharapkan?"

**Test the unexpected**
Coba input dan tindakan yang tidak dipertimbangkan oleh developer saat membangun sistem. Temukan celah di antara desain dan implementasi.

**Chain small weaknesses**
Satu cacat kecil mungkin tidak berbahaya sendiri, tapi dua atau lebih cacat yang digabungkan bisa menciptakan dampak yang jauh lebih besar.

**Think like an adversary**
Selalu bertanya: "Bagaimana seorang pelaku jahat akan mendekati target ini?" Ini menggeser perspektif dari defensif ke ofensif.

---

## 5. Konsep Chaining Weaknesses

Salah satu konsep terpenting dalam offensive security adalah **merantai kelemahan** — menggabungkan beberapa kelemahan kecil untuk menghasilkan konsekuensi yang serius.

Analoginya seperti barisan domino: satu domino yang jatuh sendiri tidak menyebabkan kerusakan berarti. Tapi ketika banyak domino ditempatkan berdekatan, satu domino yang jatuh bisa memicu reaksi berantai yang menjatuhkan semuanya.

Contoh konkret dari room ini:
- **Domino 1**: Menemukan halaman login tersembunyi (`/admin`) yang tidak seharusnya bisa diakses publik
- **Domino 2**: Halaman tersebut menggunakan password yang lemah (`abc123`, `qwerty`, dll.)
- **Hasil**: Dua kelemahan yang masing-masing terlihat kecil, digabungkan menjadi akses penuh ke sistem

Ethical hacker bertugas mencari, mendemonstrasikan, dan melaporkan kombinasi kelemahan seperti ini — bukan hanya kelemahan individual.

---

## 6. Apa yang Bisa Diakses Setelah Masuk

Setelah penyerang berhasil mendapatkan kredensial yang valid, pintu berikut terbuka:

**Sensitive functionality**
Fitur-fitur esensial yang seharusnya hanya tersedia untuk pengguna terotorisasi — seperti memodifikasi data, melihat konten terbatas, atau memicu proses sistem kritis.

**User data**
Informasi personal milik pengguna: nama, email, detail akun. Data ini bisa dicuri, disalahgunakan, atau dijual.

**Administrative features**
Fungsionalitas hak istimewa tinggi yang memungkinkan penyerang mengelola pengguna lain, mengubah konfigurasi sistem, atau mendapatkan kontrol penuh atas aplikasi.

**Further attack opportunities**
Akses terautentikasi mengekspos lapisan kerentanan berikutnya — memungkinkan penyerang memperluas jangkauan atau bergerak lebih dalam ke infrastruktur yang terhubung (pivoting).

---

## 7. Tools & Command

### 7.1 Gobuster — Directory & File Enumeration

Gobuster adalah command-line tool untuk menemukan halaman, direktori, dan file tersembunyi di web server secara otomatis menggunakan wordlist.

```bash
gobuster dir --url http://www.onlineshop.thm/ -w /usr/share/wordlists/dirbuster/directory-list.txt
```

| Switch | Fungsi |
|---|---|
| `dir` | Aktifkan mode enumerasi direktori dan file |
| `--url` | Set target URL yang akan di-scan |
| `-w` | Tentukan path ke wordlist yang dipakai untuk menebak nama direktori/file |

Gobuster berguna ketika daftar halaman yang perlu diuji terlalu panjang untuk dicek manual satu per satu.

---

### 7.2 Hydra — Dictionary Attack Tool

Hydra adalah command-line tool untuk mengotomatiskan percobaan login (dictionary attack) terhadap form login target menggunakan wordlist password.

```bash
hydra -l admin -P passlist.txt www.onlineshop.thm http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V
```

| Switch | Fungsi |
|---|---|
| `-l admin` | Set username yang akan digunakan dalam percobaan login |
| `-P passlist.txt` | Set file wordlist berisi daftar password yang akan dicoba |
| `www.onlineshop.thm` | Set target website |
| `http-post-form` | Indikasikan bahwa target menggunakan HTTP POST request form |
| `"/login:username=^USER^&password=^PASS^:F=incorrect"` | Tentukan cara login request dikirim dan kondisi kegagalan login (F=incorrect berarti jika respons mengandung kata "incorrect", login dianggap gagal) |
| `-V` | Verbose mode — tampilkan setiap kombinasi username dan password yang sedang dicoba |

> Catatan praktis: Password yang valid biasanya muncul di baris kedua dari akhir output Hydra.

---

## 8. Alur Praktik di Room Ini (Studi Kasus Mike)

Skenario: Mike ingin meluncurkan toko online dan meminta assessment untuk memastikan tidak ada halaman sensitif yang tidak sengaja bisa diakses publik.

**Step 1 — Manual Enumeration**
Coba URL berikut satu per satu di browser dengan menambahkannya di akhir `http://www.onlineshop.thm/`:

```
/sitemap
/mail
/register
/login    ← ditemukan (halaman tersembunyi yang aktif)
/admin
```

Jika halaman tidak ada, browser menampilkan Error 404.

**Step 2 — Automated Enumeration dengan Gobuster**
Jalankan Gobuster untuk scan semua kemungkinan direktori secara otomatis — jauh lebih efisien dari manual jika daftar kemungkinan sangat panjang.

**Step 3 — Manual Password Testing**
Setelah halaman `/login` ditemukan, coba kombinasi berikut dengan username `admin`:

```
abc123
123456
password
qwerty
654321
```

**Step 4 — Automated Attack dengan Hydra**
Jika daftar password panjang, gunakan Hydra untuk mengotomatiskan seluruh proses — lebih cepat dan sistematis.

**Step 5 — Berhasil Masuk**
Kombinasi yang tepat menghasilkan akses dan flag sebagai bukti keberhasilan assessment.

---

## 9. Karier di Offensive Security

**Penetration Tester / Ethical Hacker**
Berfokus pada eksplorasi vulnerability secara aman dalam scope yang ditentukan klien. Menghasilkan laporan temuan dan rekomendasi perbaikan.

**Vulnerability Researcher**
Mengidentifikasi dan memvalidasi kelemahan yang belum pernah ditemukan sebelumnya dalam software dan hardware — sering kali menghasilkan CVE baru.

**Red Team Operator**
Mensimulasikan musuh dunia nyata secara penuh untuk menguji kemampuan deteksi, respons, dan pertahanan organisasi secara menyeluruh.

---

## 10. Jalur Belajar Selanjutnya

- **Become a Defender** — pelajari teknik defensif (blue team) untuk mengamankan infrastruktur
- **Cyber Security 101** — perkuat fondasi yang lebih luas sebelum spesialisasi
- **Jr Penetration Tester** — perdalam offensive security secara lebih spesifik
- **SOC Level 1** — jalur karier Security Operations Center (blue team)

---

## 11. Glossary

| Istilah | Definisi Singkat |
|---|---|
| **Offensive Security** | Pengujian sistem dari sudut pandang penyerang untuk temukan kelemahan sebelum dieksploitasi |
| **Permission** | Izin eksplisit dari pemilik sistem — syarat mutlak semua aktivitas ethical hacking |
| **Red Teaming** | Simulasi serangan terotorisasi untuk uji efektivitas pertahanan |
| **Penetration Test** | Penilaian keamanan terstruktur oleh tester terotorisasi dalam scope tertentu |
| **Vulnerability** | Kelemahan/cacat pada sistem yang bisa disalahgunakan penyerang |
| **Exploit** | Teknik konkret yang memanfaatkan vulnerability untuk tujuan tertentu |
| **Scope** | Batasan sistem dan tindakan yang boleh diuji dalam engagement |
| **Enumeration** | Proses identifikasi detail sistem — port, service, user, direktori |
| **Credentials** | Username dan password yang memberikan akses ke sistem |
| **Authentication** | Verifikasi identitas pada proses login |
| **Dictionary Attack** | Serangan menebak password menggunakan wordlist yang sudah ditentukan |
| **Chaining Weaknesses** | Menggabungkan beberapa kelemahan kecil untuk menghasilkan dampak besar |
| **Flag** | Bukti keberhasilan dalam CTF atau latihan — diperoleh setelah berhasil mengeksploitasi target |
| **Pivoting** | Menggunakan sistem yang sudah dikuasai untuk menyerang sistem lain yang terhubung |
| **Attack Surface** | Semua titik/celah yang berpotensi diserang pada suatu sistem |
| **Wordlist** | Daftar kata (password/direktori) yang digunakan tool seperti Gobuster dan Hydra |

---

## 12. Tools & Platform Rujukan

**Gobuster**
Tool enumerasi direktori dan file untuk menemukan halaman tersembunyi di web server.
Tidak ada URL resmi yang disebutkan di materi; tersedia di sebagian besar distro Linux penetration testing.

**Hydra**
Tool dictionary attack untuk mengotomatiskan percobaan login ke berbagai jenis form dan protokol.
Tidak ada URL resmi yang disebutkan di materi; tersedia di Kali Linux dan distro serupa.

**TryHackMe — Become a Defender**
Platform pembelajaran blue team yang direkomendasikan sebagai kelanjutan room ini.
URL: https://tryhackme.com

**Wordlist: dirbuster/directory-list.txt**
Wordlist bawaan yang umum dipakai untuk enumerasi direktori web.
Lokasi default di sistem Linux: `/usr/share/wordlists/dirbuster/directory-list.txt`

---

## 13. Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar

- Offensive security — uji sistem dari sisi penyerang, temukan celah sebelum hacker jahat
- Permission — syarat mutlak semua aktivitas, tanpa izin = ilegal
- Penetration testing = hacking yang etis, legal, terstruktur

### Terminologi Wajib Hapal

- Red Teaming — simulasi musuh nyata yang terotorisasi, dalam scope
- Pentest — penilaian terstruktur oleh tester terotorisasi
- Vulnerability — kelemahan sistem yang bisa disalahgunakan
- Exploit — teknik memanfaatkan vulnerability
- Scope — batasan target yang boleh diuji
- Enumeration — kumpulkan detail sistem (port, service, user, dir)
- Credentials — username + password yang buka akses
- Authentication — verifikasi identitas saat login
- Dictionary attack — tebak password pakai wordlist
- Chaining weaknesses — gabungkan kelemahan kecil = dampak besar
- Flag — bukti keberhasilan eksploitasi di CTF/lab
- Pivoting — pakai sistem yang dikuasai untuk serang sistem lain
- Attack surface — semua titik yang bisa diserang pada suatu sistem

### Mindset Hacker (4 Poin)

- Ask questions — "What if it doesn't work as intended?"
- Test the unexpected — coba input yang developer tidak pikirkan
- Chain small weaknesses — cacat kecil + cacat kecil = dampak besar
- Think like an adversary — "bagaimana pelaku jahat dekati target ini?"

### Konsep Chaining (Efek Domino)

- 1 kelemahan = tidak kritis
- Kelemahan A + Kelemahan B = konsekuensi serius
- Contoh: halaman tersembunyi + password lemah = akses penuh

### Apa yang Bisa Diakses Setelah Masuk

- Sensitive functionality — fitur khusus user terotorisasi
- User data — nama, email, detail akun (bisa dicuri/dijual)
- Administrative features — kontrol penuh aplikasi
- Further attack opportunities — pivot lebih dalam ke sistem lain

### Gobuster — Directory Enumeration

- gobuster — tool penemuan konten web
- dir — mode enumerasi direktori & file
- --url [target] — set target yang di-scan
- -w [file] — set wordlist untuk tebak nama direktori/file

Command:
gobuster dir --url http://www.onlineshop.thm/ -w /usr/share/wordlists/dirbuster/directory-list.txt

### Hydra — Dictionary Attack

- hydra — tool dictionary attack
- -l [user] — set username
- -P [file] — set file wordlist password
- [target] — set website target
- http-post-form — indikasikan HTTP POST form
- "..." — cara request dikirim + kondisi gagal (F=incorrect)
- -V — verbose, tampilkan setiap percobaan
- Tips: password valid = baris kedua dari akhir output

Command:
hydra -l admin -P passlist.txt www.onlineshop.thm http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V

### Alur Praktik (Studi Kasus Mike)

1. Manual enum — coba /sitemap /mail /register /login /admin di browser
2. Error 404 = halaman tidak ada
3. Gobuster — scan otomatis jika daftar panjang
4. Manual password test — admin + abc123/123456/password/qwerty/654321
5. Hydra — otomasi jika wordlist panjang
6. Berhasil masuk = dapat flag

### Karier Offensive Security

- Penetration Tester / Ethical Hacker — eksplor vulnerability dalam scope
- Vulnerability Researcher — temukan kelemahan baru di software/hardware
- Red Team Operator — simulasi musuh nyata, uji deteksi & respons

### Jalur Belajar Selanjutnya

- Become a Defender — blue team / defensive
- Cyber Security 101 — fondasi luas
- Jr Penetration Tester — offensive lebih dalam
- SOC Level 1 — karier Security Operations Center
