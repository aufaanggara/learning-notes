# Resume: IAAA & OWASP Top 10:2025 (Identity, Authentication, Authorisation, Accountability)
**Tanggal:** 1 September 2026

---

## 1. Konsep Inti: IAAA

IAAA adalah kerangka berpikir untuk memahami bagaimana pengguna dan tindakannya diverifikasi pada sebuah aplikasi. Empat komponennya bersifat **berurutan dan tidak bisa dilewati** — jika satu tahap gagal diterapkan, tahap setelahnya tidak bisa berjalan dengan benar.

**1.1 Identity**
Akun unik yang merepresentasikan seseorang atau layanan, contohnya user ID atau email. Ini adalah "klaim" tentang siapa yang sedang berinteraksi dengan sistem.

**1.2 Authentication**
Proses membuktikan bahwa identitas tersebut benar adanya, misalnya lewat password, OTP, atau passkey. Ini menjawab pertanyaan "apakah kamu benar-benar orang itu?"

**1.3 Authorisation**
Menentukan apa saja yang boleh dilakukan oleh identitas yang sudah terverifikasi. Ini menjawab "apa yang boleh kamu akses/lakukan?"

**1.4 Accountability**
Mencatat dan memberi peringatan atas siapa melakukan apa, kapan, dan dari mana. Ini menjadi dasar untuk investigasi dan audit ketika terjadi insiden.

Ketiga kategori OWASP Top 10:2025 yang dibahas dalam materi ini (A01, A07, A09) semuanya berakar dari kegagalan menerapkan salah satu komponen IAAA di atas. Kelemahan pada lapisan ini berdampak besar karena memungkinkan threat actor mengakses data pengguna lain atau memperoleh privilege lebih tinggi dari yang seharusnya.

---

## 2. A01: Broken Access Control

**2.1 Definisi**
Terjadi ketika server tidak menerapkan dengan benar aturan siapa boleh mengakses apa pada setiap request. Akar masalahnya adalah aplikasi **terlalu mempercayai client** — pengecekan otorisasi yang seharusnya dilakukan di server justru diasumsikan sudah aman dari sisi client.

**2.2 IDOR (Insecure Direct Object Reference)**
Bentuk paling umum dari Broken Access Control. Terjadi ketika mengubah sebuah identifier di request (contoh: `?id=7` menjadi `?id=6`) memungkinkan pengguna melihat atau mengedit data milik pengguna lain, tanpa ada validasi kepemilikan data di sisi server.

**2.3 Jenis Privilege Escalation**

- **Horizontal privilege escalation** — pengguna tetap berada di level/role yang sama, tapi bisa mengakses data milik pengguna lain yang setingkat. Tidak ada perubahan role, hanya kebocoran data lintas akun.
- **Vertical privilege escalation** — pengguna berhasil melompat untuk melakukan aksi yang seharusnya hanya boleh dilakukan oleh role yang lebih tinggi (contoh: user biasa bisa mengakses fungsi admin).

**2.4 Studi Kasus**
Pada static site latihan, parameter `accountID` di URL bisa diubah bebas tanpa validasi kepemilikan akun. Dengan mengganti nilai `id`, seorang user bisa melihat data akun (termasuk saldo) milik user lain — ini adalah contoh konkret IDOR yang menghasilkan horizontal privilege escalation.

**2.5 Mitigasi**
- Menerapkan pengecekan otorisasi di sisi server pada **setiap** request (bukan hanya sekali di awal sesi).
- Memverifikasi bahwa user yang sedang login memang berhak atas resource spesifik yang diminta, bukan hanya login valid.
- Menggunakan referensi tidak langsung (indirect reference) atau token terenkripsi, alih-alih ID yang berurutan dan mudah ditebak.
- Logging dan monitoring terhadap upaya akses ke resource sensitif.

---

## 3. A07: Authentication Failures

**3.1 Definisi**
Terjadi ketika aplikasi tidak bisa secara andal memverifikasi atau mengikat (bind) identitas seorang pengguna ke akun yang benar.

**3.2 Masalah Umum**
- **Username enumeration** — sistem secara tidak sengaja membocorkan informasi username mana yang valid/terdaftar.
- **Weak/guessable passwords** — password lemah tanpa mekanisme lockout atau rate limit, membuka celah brute-force.
- **Logic flaws pada alur login/registrasi** — celah logika dalam proses pendaftaran atau login yang bisa dieksploitasi.
- **Insecure session/cookie handling** — penanganan sesi atau cookie yang tidak aman, memungkinkan sesi dibajak atau diikat ke akun yang salah.

**3.3 Studi Kasus: Case Sensitivity / Account Confusion**
Username asli `admin` bisa "ditiru" dengan mendaftarkan akun baru bernama `aDmiN` (kombinasi huruf besar-kecil berbeda). Ini berhasil karena ada **inkonsistensi validasi**:

- Saat **registrasi**, pengecekan keunikan username dilakukan secara *case-sensitive* — sistem menganggap `admin` dan `aDmiN` sebagai dua username berbeda, sehingga pendaftaran `aDmiN` diizinkan meski `admin` sudah terdaftar.
- Saat **login/autentikasi ke database**, perbandingan dilakukan secara *case-insensitive* (umum terjadi karena collation database seperti MySQL default bersifat *case-insensitive*), sehingga `aDmiN` dan `admin` dianggap sama — akibatnya sesi login malah terikat ke akun `admin` yang asli.

Bug jenis ini nyata terjadi di dunia industri (bukan cuma skenario CTF), sering dilaporkan dalam program bug bounty sebagai penyebab account takeover.

**3.4 Mitigasi**
- Terapkan **unique index** pada bentuk kanonikal (canonical form) dari username — misalnya selalu menyimpan dan membandingkan dalam huruf kecil semua, konsisten di semua lapisan sistem (registrasi maupun login).
- Rate-limit dan/atau lockout terhadap percobaan brute-force.
- Rotasi session setiap kali terjadi perubahan password atau privilege.

---

## 4. A09: Logging & Alerting Failures

**4.1 Definisi**
Terjadi ketika aplikasi tidak mencatat (log) atau tidak memberi peringatan (alert) terhadap kejadian-kejadian yang relevan dengan keamanan. Tanpa logging yang baik, defender tidak bisa mendeteksi atau menginvestigasi serangan. Logging yang baik adalah fondasi dari **accountability**.

**4.2 Bentuk Kegagalan**
- Event autentikasi (login berhasil/gagal) tidak tercatat.
- Log error yang samar/tidak informatif.
- Tidak ada alerting saat terjadi brute-force atau perubahan privilege.
- Masa retensi log yang terlalu singkat.
- Log disimpan di lokasi yang bisa diubah/dirusak (tamper) oleh penyerang.

**4.3 Metodologi Investigasi Log (dari studi kasus)**
Alur analisis log untuk mengidentifikasi serangan brute-force yang berhasil:

1. **Identifikasi pola brute-force** — cari satu IP sumber yang melakukan banyak request POST ke endpoint login secara berurutan dalam rentang waktu singkat, dengan payload password yang berbeda-beda di tiap percobaan.
2. **Temukan titik keberhasilan** — di antara deretan response `401 Unauthorized`, cari baris dengan status `200 OK` dari IP yang sama; ini menandakan percobaan yang akhirnya berhasil.
3. **Identifikasi username yang dibobol** — lihat parameter `user=` pada request yang berhasil tersebut.
4. **Lacak aksi lanjutan** — periksa request berikutnya dari IP yang sama untuk mengetahui endpoint apa yang diakses setelah berhasil login (indikasi tujuan akhir serangan, misalnya mengakses halaman khusus admin).

**4.4 Studi Kasus**
Log menunjukkan IP `203.0.113.45` melakukan serangkaian POST ke `/login?user=admin&pass=...` dengan berbagai password (`admin123`, `password`, `12345`, `letmein`, `qwerty`) yang semuanya gagal (`401`), hingga akhirnya berhasil dengan `Password123` (`200 OK`, ditandai sistem sebagai *security breach detected*). Selanjutnya IP yang sama mengakses endpoint `/supersecretadminstuff` dengan `GET`, juga bertanda breach — menunjukkan tujuan akhir serangan adalah mengeksploitasi akses admin yang baru didapat.

**4.5 Mitigasi**
- Log seluruh siklus hidup autentikasi: percobaan gagal/berhasil, perubahan password, perubahan 2FA, perubahan role, serta aksi-aksi yang dilakukan admin.
- Sentralisasikan log di luar host asal (off-host) dengan kebijakan retensi yang jelas, agar tidak mudah dihapus/dimanipulasi penyerang yang berhasil masuk.
- Buat alerting otomatis untuk anomali, misalnya lonjakan percobaan login (brute-force) atau eskalasi privilege mendadak.

---

## 5. Ringkasan Keterkaitan Ketiga Kategori

| Kategori | Komponen IAAA yang Gagal | Inti Masalah |
|---|---|---|
| A01 Broken Access Control | Authorisation | Server tidak memvalidasi kepemilikan resource pada tiap request |
| A07 Authentication Failures | Authentication | Identitas tidak terverifikasi/terikat secara andal ke akun yang benar |
| A09 Logging & Alerting Failures | Accountability | Aktivitas tidak tercatat sehingga serangan tidak terdeteksi/terinvestigasi |

Prinsip besar yang menghubungkan ketiganya: setiap lapisan IAAA harus divalidasi **di sisi server**, secara **konsisten** di seluruh bagian sistem, dan setiap aktivitas penting harus **tercatat** agar bisa dipertanggungjawabkan.

---

## 6. Glossary

- **IAAA** — kerangka Identity, Authentication, Authorisation, Accountability untuk memverifikasi pengguna dan tindakannya.
- **Broken Access Control** — kegagalan server menegakkan aturan akses pada setiap request.
- **IDOR (Insecure Direct Object Reference)** — kerentanan akibat identifier objek (ID) bisa diubah untuk mengakses data milik pihak lain tanpa validasi kepemilikan.
- **Horizontal privilege escalation** — mengakses data milik pengguna lain pada level/role yang sama.
- **Vertical privilege escalation** — mendapatkan akses ke aksi/level yang lebih tinggi dari yang seharusnya (misal user ke admin).
- **Authentication Failure** — kegagalan sistem memverifikasi/mengikat identitas pengguna secara andal.
- **Username Enumeration** — kebocoran informasi validitas username melalui respons sistem.
- **Case Sensitivity Vulnerability / Account Confusion** — bug akibat inkonsistensi pengecekan besar-kecil huruf antara proses registrasi dan proses login/autentikasi.
- **Canonical form** — bentuk baku/standar suatu data (misal selalu huruf kecil) yang dipakai konsisten untuk validasi dan pembandingan.
- **Brute-force attack** — upaya login dengan mencoba banyak kombinasi kredensial secara berurutan hingga berhasil.
- **Rate limiting / Lockout** — mekanisme pembatasan jumlah percobaan (misal login) dalam rentang waktu tertentu untuk mencegah brute-force.
- **Logging & Alerting Failure** — kegagalan mencatat dan memberi peringatan atas kejadian yang relevan dengan keamanan.
- **Accountability** — kemampuan membuktikan siapa melakukan apa, kapan, dan dari mana, berbasis catatan log.
- **Off-host logging** — penyimpanan log terpusat di luar server asal agar tidak mudah dimanipulasi penyerang yang sudah masuk.
- **Session rotation** — penggantian sesi (session token) setelah terjadi perubahan sensitif seperti password atau privilege, untuk mencegah penyalahgunaan sesi lama.

---

## 7. Tools & Platform Rujukan

Materi ini tidak menyebutkan tools atau platform eksternal pihak ketiga (seperti Talos, VirusTotal, Shodan, AbuseIPDB, dsb). Praktik dilakukan langsung pada static site latihan bawaan masing-masing task, tanpa memerlukan tools analisis tambahan.

Room lanjutan yang direkomendasikan untuk pendalaman (bagian dari platform pembelajaran yang sama):
- **Broken Access Control** — pendalaman variasi Broken Access Control.
- **Insecure Direct Object References** — pendalaman variasi IDOR (encoded ID, hashed ID, dll).
- **Authentication Bypass Room** — teknik bypass autentikasi lebih luas.
- **Multi-Factor Authentication** — pendalaman MFA.
- **Authentication Module** — modul autentikasi secara umum.
- **Room tentang logging untuk accountability** (disebut sebagai "this room" pada materi, tanpa nama eksplisit) — pendalaman logging.
- **Application Design Flaws** — room lanjutan berikutnya dalam modul yang sama.

---

## 8. Catatan Ringkas untuk Ditulis Tangan

**IAAA**
- Identity — akun unik (user ID/email)
- Authentication — bukti identitas (password, OTP, passkey)
- Authorisation — hak akses identitas
- Accountability — catat siapa-apa-kapan-dari mana
- Urutan wajib berurutan, tidak bisa skip level

**A01 — Broken Access Control**
- Definisi — server gagal cek "siapa boleh akses apa" tiap request
- IDOR — ubah ID (?id=7→6) → akses data orang lain
- Horizontal PE — role sama, lihat data user lain
- Vertical PE — lompat ke aksi admin
- Fix — cek server-side tiap request, verifikasi kepemilikan resource, pakai indirect ref/token, log akses sensitif

**A07 — Authentication Failures**
- Definisi — gagal verifikasi/bind identitas dgn benar
- Masalah — username enumeration, weak password no lockout, logic flaw login/register, session/cookie tidak aman
- Case Sensitivity Bug — daftar "aDmiN" saat "admin" sudah ada → lolos karena cek registrasi case-sensitive, tapi login/DB case-insensitive → login jadi ke akun admin asli
- Istilah — Account Confusion
- Fix — unique index pakai canonical form (lowercase konsisten), rate-limit/lockout brute-force, rotasi session saat ganti password/privilege

**A09 — Logging & Alerting Failures**
- Definisi — gagal catat/alert event keamanan
- Gagal jika — auth event hilang, log samar, no alert brute-force, retensi pendek, log bisa ditamper
- Metode investigasi log:
  1. cari 1 IP, banyak POST login beruntun, password beda-beda
  2. cari baris 200 OK di antara 401 → titik berhasil
  3. cek param user= → username dibobol
  4. cek request setelahnya dari IP sama → endpoint tujuan attacker
- Fix — log full auth lifecycle (fail/success, pw/2FA/role change, admin action), simpan off-host + retensi jelas, alert anomali (brute-force burst, privilege escalation)

**Studi kasus log (contoh angka)**
- IP attacker: 203.0.113.45
- Username dibobol: admin
- Password berhasil: Password123
- Endpoint diakses setelah breach: /supersecretadminstuff

**Relasi kategori-IAAA**
- A01 → gagal di Authorisation
- A07 → gagal di Authentication
- A09 → gagal di Accountability
