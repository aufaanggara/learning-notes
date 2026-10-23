# Resume Materi: SQL Injection
**Room:** SQL Injection (TryHackMe)
**Tanggal Selesai:** 25 Juni 2026

---

## 1. Konsep Dasar

### 1.1 Database

**Database** adalah kumpulan data yang bisa disimpan, dimodifikasi, dan diambil kembali dalam format yang terstruktur. Website menggunakan database untuk menyimpan informasi user, produk, atau data lain yang perlu diakses secara dinamis.

Setiap kali user berinteraksi dengan website (login, pencarian, registrasi), website akan mengirimkan permintaan ke database untuk mengambil atau menyimpan data yang relevan.

### 1.2 DBMS dan SQL

**Database Management System (DBMS)** adalah sistem yang mengelola database. Contoh DBMS yang umum dipakai: MySQL, PostgreSQL, SQLite, Microsoft SQL Server, MariaDB.

**SQL (Structured Query Language)** adalah bahasa yang digunakan aplikasi dan website untuk berkomunikasi dengan database. Setiap kali ada interaksi antara aplikasi dan database, SQL query-lah yang dikirimkan.

Contoh query SQL yang dikirim ketika user login:

```sql
SELECT * FROM users WHERE username = 'John' AND password = 'Un@detectable444';
```

Query ini memerintahkan database untuk mencari user bernama John dengan password tersebut. Jika keduanya cocok (karena dipisahkan oleh operator AND), database akan mengembalikan data user tersebut ke aplikasi.

### 1.3 Alur Interaksi Web-Database

Tiga komponen utama dalam alur ini:

- **User** — pihak yang berinteraksi dengan website melalui browser.
- **Webserver** — menerima input dari user, membentuk SQL query, dan mengirimkannya ke database.
- **Database Server** — menerima query, mengeksekusinya, dan mengembalikan hasilnya ke webserver.

---

## 2. SQL Injection Vulnerability

### 2.1 Definisi

**SQL Injection (SQLi)** adalah serangan yang terjadi ketika input dari user tidak divalidasi atau disanitasi dengan benar, sehingga attacker bisa menyisipkan SQL query berbahaya ke dalam input yang dikirim ke database. Query tersebut kemudian ikut dieksekusi oleh database, dan attacker bisa memanipulasi hasilnya.

SQL injection sangat berbahaya karena hampir semua organisasi menyimpan data kritis mereka di database — mulai dari data user, kredensial, hingga informasi sensitif lainnya.

### 2.2 Syarat Terjadinya SQL Injection

Dua kondisi yang harus terpenuhi agar SQL injection bisa terjadi:

- **Input tidak divalidasi** — website tidak memeriksa apakah input yang diberikan user aman atau mengandung karakter/sintaks berbahaya.
- **Input tidak disanitasi** — website tidak membersihkan atau menetralisir karakter spesial dalam input sebelum memasukkannya ke dalam SQL query.

### 2.3 Cara Kerja SQL Injection (Contoh Login Bypass)

Anggap sebuah halaman login yang tidak memiliki input validation. Attacker tidak tahu password user John, lalu memasukkan input berikut:

```
Username: John
Password: abc' OR 1=1;-- -
```

Query yang terbentuk di database akan menjadi:

```sql
SELECT * FROM users WHERE username = 'John' AND password = 'abc' OR 1=1;-- -';
```

Penjelasan cara kerjanya:

- Database mengecek apakah username John ada — ya, ada.
- Database mengecek apakah passwordnya `abc` — tidak cocok, kondisi ini gagal.
- Karena ada operator **OR**, database mengecek kondisi berikutnya: `1=1`.
- `1=1` selalu bernilai **true**, sehingga seluruh query dianggap berhasil.
- Bagian `-- -` adalah **comment** di SQL, yang membuat semua karakter setelahnya diabaikan oleh database.
- Hasilnya: attacker berhasil login ke akun John tanpa mengetahui passwordnya sama sekali.

### 2.4 Peran Single Quote dalam SQL Injection

**Single quote (')** setelah input acak (misalnya `abc'`) berfungsi untuk **menutup string** dalam query SQL lebih awal dari yang diharapkan sistem. Tanpa single quote, seluruh string `abc OR 1=1;-- -` hanya akan dianggap sebagai nilai password biasa oleh database. Dengan menambahkan single quote setelah `abc`, bagian `OR 1=1;-- -` menjadi bagian dari logika SQL query, bukan sekadar nilai string.

---

## 3. Teknik-Teknik SQL Injection

### 3.1 Boolean-Based Blind

Query SQL dimodifikasi dengan menyertakan ekspresi boolean (benar/salah). Tidak ada output langsung — attacker membaca respons sistem (misalnya konten halaman berubah atau tidak) untuk menyimpulkan data secara bertahap.

Contoh payload: `cat=1 AND 2175=2175`

### 3.2 Error-Based

Beberapa query sengaja dimodifikasi untuk memancing database menghasilkan pesan error. Pesan error tersebut seringkali mengandung informasi berharga tentang struktur atau isi database.

Contoh payload: `cat=1 AND EXTRACTVALUE(1846,CONCAT(...))`

### 3.3 Time-Based Blind

Attacker menyisipkan perintah seperti `SLEEP(5)` ke dalam query. Jika database menunggu 5 detik sebelum merespons, artinya kondisi yang diuji bernilai true. Teknik ini dipakai ketika tidak ada output atau error yang bisa dibaca.

Contoh payload: `cat=1 AND SLEEP(5)`

### 3.4 UNION Query

Attacker menggunakan perintah UNION untuk menggabungkan hasil query asli dengan query tambahan buatan attacker, sehingga data dari tabel lain bisa ikut ditampilkan bersama hasil query normal.

Contoh payload: `cat=1 UNION ALL SELECT CONCAT(...)`

---

## 4. SQLMap — Automated SQL Injection Tool

### 4.1 Tentang SQLMap

**SQLMap** adalah tool open-source untuk mendeteksi dan mengeksploitasi SQL injection secara otomatis pada aplikasi web. Tool ini sudah tersedia di beberapa distribusi Linux (termasuk Kali Linux) dan bisa diinstall manual jika belum ada. SQLMap berjalan di terminal (command-line).

Dokumentasi resmi: https://sqlmap.org

### 4.2 Flag dan Fungsinya

| Flag | Fungsi |
|---|---|
| `-u 'URL'` | Menentukan target URL yang akan diuji |
| `--dbs` | Mengekstrak semua nama database yang tersedia |
| `-D nama_database` | Menentukan database spesifik yang menjadi target |
| `--tables` | Menampilkan semua nama tabel dalam database yang dipilih |
| `-T nama_tabel` | Menentukan tabel spesifik yang menjadi target |
| `--dump` | Mengekstrak semua isi/record dari tabel yang dipilih |
| `--help` | Menampilkan semua flag yang tersedia |
| `--wizard` | Mode interaktif bertahap, cocok untuk pemula |
| `--level=5` | Menaikkan kedalaman scan (1-5); level 5 mencoba lebih banyak teknik/payload |
| `--cookie="..."` | Menyertakan session cookie untuk pengujian yang membutuhkan autentikasi |
| `-r file.txt` | Membaca dan menguji request dari file teks (untuk POST-based testing) |
| `--tamper` | Menyamarkan payload untuk melewati WAF/IPS |
| `--random-agent` | Mengganti user-agent secara acak untuk menghindari deteksi |
| `--time-sec` | Menentukan durasi waktu tunggu untuk time-based testing (disarankan 10+ jika ada lag) |

### 4.3 Alur Penggunaan SQLMap (GET-Based Testing)

Ini adalah urutan flag yang dipakai untuk mengekstrak data dari database secara bertahap:

**Langkah 1 — Konfirmasi vulnerability dan lihat semua database:**

```bash
sqlmap -u 'http://TARGET_IP/path?parameter=nilai' --dbs --level=5
```

**Langkah 2 — Lihat tabel dalam database target:**

```bash
sqlmap -u 'http://TARGET_IP/path?parameter=nilai' -D nama_database --tables --level=5
```

**Langkah 3 — Dump isi tabel target:**

```bash
sqlmap -u 'http://TARGET_IP/path?parameter=nilai' -D nama_database -T nama_tabel --dump --level=5
```

### 4.4 GET-Based Testing vs POST-Based Testing

**GET-Based Testing** digunakan ketika aplikasi mengirimkan data melalui parameter yang terlihat langsung di URL (contoh: `?email=test&password=test`). Parameter ini bisa langsung dimasukkan ke flag `-u` SQLMap.

URL yang mengandung GET parameter selalu berpotensi vulnerable terhadap SQL injection dan wajib diuji.

**POST-Based Testing** digunakan ketika data dikirim melalui body request, bukan URL (misalnya form login atau registrasi standar). Untuk pendekatan ini:

1. Intercept POST request menggunakan browser DevTools atau Burp Suite.
2. Simpan request tersebut sebagai file teks (misalnya `intercepted_request.txt`).
3. Jalankan SQLMap dengan flag `-r`:

```bash
sqlmap -r intercepted_request.txt
```

### 4.5 Cookie-Based Testing

Beberapa aplikasi web menggunakan cookie (seperti **PHPSESSID**, **JSESSIONID**, atau authentication token) untuk menjaga sesi user. Jika endpoint yang ingin diuji hanya bisa diakses setelah login, SQLMap perlu diberikan cookie session tersebut agar bisa berinteraksi dengan aplikasi sebagai user yang sudah terautentikasi:

```bash
sqlmap -u 'http://TARGET/path?param=nilai' --cookie="SESSIONID=abcdef123456"
```

### 4.6 Menggunakan --wizard (Mode Interaktif)

Jalankan SQLMap dengan flag `--wizard` untuk masuk ke mode interaktif. Tool akan menanyakan URL target dan parameter lainnya secara bertahap:

```bash
sqlmap --wizard
```

Mode ini cocok untuk pemula yang belum hafal semua flag.

---

## 5. Cara Menemukan GET Parameter di URL Tersembunyi

Tidak semua GET parameter terlihat langsung di URL browser. Beberapa aplikasi menggunakan JavaScript/AJAX (XHR) untuk mengirimkan request, sehingga parameter tidak muncul di address bar. Untuk menemukannya:

1. Klik kanan di halaman target dan pilih **Inspect** (Developer Tools).
2. Buka tab **Network**.
3. Isi form (misalnya login) dan klik tombol submit.
4. Di tab Network, akan muncul request baru — klik request tersebut.
5. Salin URL lengkap beserta parameter-parameternya dari bagian Headers.

Alternatif lain: gunakan **Burp Suite** untuk mencegat (intercept) request secara langsung, yang memberikan visibilitas lebih lengkap terhadap seluruh isi request.

> Catatan penting: Saat memasukkan URL ke SQLMap di terminal, selalu bungkus URL dengan tanda kutip tunggal (`'`). Karakter `&` di dalam URL punya makna khusus di bash (menjalankan command di background), sehingga tanpa tanda kutip URL akan dipecah menjadi beberapa command terpisah oleh shell.

---

## 6. Membaca Output Log SQLMap

Setiap baris output SQLMap diawali label tingkat kepentingan:

- **[INFO]** — informasi proses normal, tidak ada masalah. Hanya laporan progres apa yang sedang dikerjakan SQLMap.
- **[WARNING]** — peringatan bahwa ada sesuatu yang perlu diperhatikan, tetapi proses scan tetap lanjut. Contoh: parameter tidak terlihat dinamis, reflective value ditemukan.
- **[CRITICAL]** — kesimpulan penting atau masalah besar. Bisa berarti vulnerability ditemukan, atau sebaliknya: semua parameter sudah diuji dan tidak ada yang vulnerable.

---

## 7. Studi Kasus Praktis (ChatAI Login Page)

### 7.1 Skenario

Target: aplikasi web **ChatAI** di `http://MACHINE_IP/ai/login`, sebuah halaman login yang vulnerable terhadap SQL injection. Parameter GET ditemukan melalui tab Network browser, menghasilkan URL:

```
http://MACHINE_IP/ai/includes/user_login?email=test&password=test
```

### 7.2 Temuan

Setelah menjalankan SQLMap dengan flag bertahap, ditemukan:

- Parameter **email** vulnerable dengan teknik: boolean-based blind, error-based, dan time-based blind.
- Back-end DBMS: MySQL >= 5.0 (MariaDB fork).
- Web server OS: Windows, web technology: Apache 2.4.53.
- Terdapat **6 database**: `ai`, `information_schema`, `mysql`, `performance_schema`, `phpmyadmin`, `test`.
- Database `ai` memiliki **1 tabel**: `user`.
- Tabel `user` berisi kolom: `id`, `email`, `created`, `password`.
- Kredensial yang berhasil diekstrak: `test@chatai.com` / `12345678`.

### 7.3 Mengapa Parameter email Vulnerable

Form login ChatAI menggunakan GET request (dikirim via jQuery AJAX/XHR) alih-alih POST. Ini adalah **bad practice** karena data sensitif seperti password ikut nempel di URL, yang berisiko tersimpan di browser history, server log, atau proxy log. Ditambah lagi, input dari parameter tersebut tidak disanitasi, sehingga SQLMap berhasil menyisipkan payload injection.

---

## 8. Glossary

**SQL (Structured Query Language)** — bahasa standar untuk berkomunikasi dengan database.

**DBMS (Database Management System)** — sistem yang mengelola database; contoh: MySQL, PostgreSQL, MariaDB.

**SQL Injection (SQLi)** — serangan yang menyisipkan query SQL berbahaya ke dalam input yang tidak disanitasi, sehingga attacker bisa memanipulasi database.

**Input Validation** — proses memeriksa apakah input dari user sesuai format/aturan yang diharapkan sebelum diproses.

**Input Sanitization** — proses membersihkan karakter spesial dari input user agar tidak bisa diinterpretasikan sebagai bagian dari query SQL.

**Boolean-Based Blind SQLi** — teknik injection yang mengandalkan ekspresi benar/salah untuk mengekstrak informasi secara bertahap tanpa output langsung.

**Error-Based SQLi** — teknik injection yang memanfaatkan pesan error dari database untuk mendapatkan informasi.

**Time-Based Blind SQLi** — teknik injection yang menggunakan perintah delay (SLEEP) untuk mengonfirmasi kondisi ketika tidak ada output yang bisa dibaca.

**UNION Query SQLi** — teknik injection yang menggabungkan hasil query attacker dengan query asli menggunakan perintah UNION.

**GET Parameter** — parameter yang dikirim melalui URL, terlihat langsung di address bar browser.

**POST Request** — request yang mengirimkan data melalui body, tidak terlihat di URL.

**WAF (Web Application Firewall)** — sistem keamanan yang mendeteksi dan memblokir request berbahaya sebelum sampai ke server.

**XHR (XMLHttpRequest)** — mekanisme JavaScript untuk mengirimkan request HTTP di background tanpa reload halaman.

**Session Cookie** — token yang disimpan di browser untuk menjaga sesi user yang sudah login; contoh: PHPSESSID, JSESSIONID.

**Payload** — string/input berbahaya yang disisipkan oleh attacker ke dalam parameter yang vulnerable.

**Dump** — proses mengekstrak seluruh isi tabel/database ke output yang bisa dibaca.

**Intercept** — proses mencegat request HTTP sebelum dikirimkan ke server, biasanya menggunakan proxy tool seperti Burp Suite.

---

## 9. Tools & Platform Rujukan

**SQLMap**
Automated tool untuk mendeteksi dan mengeksploitasi SQL injection vulnerability.
URL: https://sqlmap.org

**Burp Suite**
Tool intercept proxy untuk mencegat, menganalisis, dan memodifikasi request HTTP/HTTPS secara manual maupun otomatis. Sangat berguna untuk POST-based testing dan menemukan parameter tersembunyi.
URL: https://portswigger.net/burp

**TryHackMe**
Platform pembelajaran cyber security berbasis lab virtual interaktif, tempat room SQL Injection ini diakses.
URL: https://tryhackme.com

---

## 10. Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar

- Database — tempat penyimpanan data terstruktur milik aplikasi/website
- DBMS — sistem yang mengelola database (MySQL, PostgreSQL, MariaDB, dll)
- SQL — bahasa untuk berkomunikasi dengan database
- Alur: User → Webserver (SQL query) → Database Server

### SQL Injection

- SQLi — menyisipkan query SQL berbahaya lewat input yang tidak disanitasi
- Syarat: input tidak divalidasi DAN tidak disanitasi
- Single quote (') — menutup string lebih awal, memungkinkan penyisipan logika SQL
- OR 1=1 — kondisi yang selalu true, dipakai untuk bypass login
- -- - — comment SQL, mengabaikan sisa query setelah titik ini

### Teknik SQLi

- Boolean-based blind — ekspresi benar/salah, tanpa output langsung
- Error-based — pancing error database untuk bocorkan informasi
- Time-based blind — pakai SLEEP(), ukur waktu respons
- UNION query — gabungkan hasil query asli dengan query attacker

### SQLMap — Flag Penting

- -u 'URL' — tentukan target URL (selalu pakai tanda kutip tunggal)
- --dbs — tampilkan semua nama database
- -D nama_db — pilih database spesifik
- --tables — tampilkan semua tabel dalam database yang dipilih
- -T nama_tabel — pilih tabel spesifik
- --dump — ekstrak seluruh isi tabel
- --level=5 — tingkatkan kedalaman scan
- --cookie="..." — sertakan session cookie untuk endpoint yang butuh auth
- -r file.txt — baca POST request dari file (POST-based testing)
- --wizard — mode interaktif bertahap untuk pemula
- --tamper — samarkan payload dari WAF
- --random-agent — ganti user-agent secara acak

### Alur Penggunaan SQLMap

1. -u 'URL' --dbs → cari semua database
2. -D target_db --tables → cari semua tabel
3. -D target_db -T target_table --dump → ambil semua isi tabel

### GET vs POST Testing

- GET — parameter di URL, langsung pakai -u
- POST — data di body request, intercept dulu → simpan ke .txt → pakai -r

### Menemukan GET Parameter Tersembunyi

- Browser: Inspect → tab Network → submit form → salin URL dari Headers
- Burp Suite: intercept request langsung

### Log Level SQLMap

- INFO — proses normal berjalan
- WARNING — ada anomali, scan tetap lanjut
- CRITICAL — kesimpulan penting (vulnerability ditemukan atau tidak ada)

### Catatan Keamanan

- Tanda & di URL tanpa kutip = dipecah jadi 2 command oleh bash → wajib pakai ' '
- GET untuk password = bad practice (tersimpan di log/history)
- SQLMap hanya boleh dipakai pada target yang sudah ada izin (consent) dari pemiliknya
