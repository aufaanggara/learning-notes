# Resume Materi: Hydra — TryHackMe
Tanggal: 31 Agustus 2026


---


## 1. Konsep Dasar

### 1.1 Apa itu Hydra

**Hydra** adalah tools brute force online yang digunakan untuk menebak kredensial login (username dan/atau password) secara otomatis pada berbagai layanan jaringan.

Hydra bekerja dengan cara menjalankan daftar password (wordlist) secara berurutan dan mencobanya satu per satu ke target yang ditentukan hingga menemukan kombinasi yang berhasil login.

### 1.2 Mengapa Hydra Relevan

Banyak sistem — terutama perangkat CCTV, router, dan web framework — masih menggunakan kredensial default seperti `admin:password`. Hydra membuktikan betapa mudahnya sistem semacam itu dibobol jika tidak diubah dari default.

Password yang lemah (umum, pendek, tanpa karakter khusus) sangat rentan terhadap serangan brute force karena sudah masuk dalam wordlist publik seperti `rockyou.txt` yang berisi lebih dari 14 juta entri.


---


## 2. Protokol yang Didukung

Hydra mendukung brute force pada protokol berikut:

Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP, HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MEMCACHED, MONGODB, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, Radmin, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 dan v2), SSHKEY, Subversion, TeamSpeak (TS2), Telnet, VMware-Auth, VNC, dan XMPP.


---


## 3. Instalasi

Hydra sudah tersedia secara default di **Kali Linux** dan **AttackBox TryHackMe**.

Untuk distro Linux lain, instalasi bisa dilakukan dengan:

```bash
apt install hydra        # Ubuntu/Debian
dnf install hydra        # Fedora
```

Source code juga tersedia di repositori resmi Hydra di GitHub (github.com/vanhauser-thc/thc-hydra).


---


## 4. Command dan Switch

### 4.1 Struktur Umum Command Hydra

```
hydra [opsi] TARGET PROTOKOL
```

### 4.2 Switch Umum

| Switch | Fungsi |
|--------|--------|
| `-l <username>` | Menentukan satu username untuk dicoba |
| `-L <file>` | Menggunakan file berisi daftar username |
| `-p <password>` | Menentukan satu password secara langsung |
| `-P <file>` | Menggunakan file berisi daftar password (wordlist) |
| `-t <angka>` | Mengatur jumlah thread paralel yang berjalan bersamaan |
| `-V` | Verbose — menampilkan setiap percobaan login di terminal |
| `-s <port>` | Menentukan port non-default secara eksplisit |

Catatan penting: `-p` (huruf kecil) dan `-P` (huruf besar) berbeda fungsi. Salah menggunakan keduanya menyebabkan Hydra tidak membaca wordlist dengan benar.

### 4.3 Command untuk FTP

```bash
hydra -l user -P passlist.txt ftp://MACHINE_IP
```

### 4.4 Command untuk SSH

```bash
hydra -l <username> -P <full path to pass> MACHINE_IP -t 4 ssh
```

Contoh konkret:

```bash
hydra -l root -P passwords.txt MACHINE_IP -t 4 ssh
```

Penjelasan argumen:
- `-l root` — username yang digunakan adalah root
- `-P passwords.txt` — mencoba semua password yang ada di file passwords.txt
- `-t 4` — menjalankan 4 thread secara paralel

### 4.5 Command untuk Web Form (POST)

```bash
sudo hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "<path>:<login_credentials>:<invalid_response>"
```

Penjelasan parameter dalam tanda kutip:

| Parameter | Fungsi |
|-----------|--------|
| `<path>` | URL path halaman login, contoh: `/login` atau `/login.php` |
| `<login_credentials>` | Format field form, contoh: `username=^USER^&password=^PASS^` |
| `<invalid_response>` | Teks yang muncul di halaman ketika login gagal, diawali `F=` |

Penjelasan placeholder:
- `^USER^` — akan digantikan oleh username yang ditentukan
- `^PASS^` — akan digantikan oleh setiap password dari wordlist

Contoh konkret:

```bash
hydra -l molly -P /usr/share/wordlists/rockyou.txt MACHINE_IP http-post-form "/login:username=^USER^&password=^PASS^:F=Your username or password is incorrect." -V
```

Command dengan port custom:

```bash
hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -s <port> -V
```


---


## 5. Metodologi Penggunaan Hydra pada Web Form

Sebelum menjalankan Hydra pada web form, ada tiga hal yang wajib diketahui:

**1. Path halaman login**
Navigasikan ke halaman login target, lihat URL-nya. Contoh: jika URL-nya `http://10.49.189.83/login`, maka path-nya adalah `/login`.

**2. Nama field username dan password**
Buka halaman login di browser, klik kanan, pilih Inspect (Developer Tools), masuk ke tab Elements, cari tag `<input>` dan lihat atribut `name`-nya. Dua field yang dicari biasanya berupa `name="username"` dan `name="password"`, tapi bisa berbeda tiap aplikasi.

**3. Teks pesan error saat login gagal**
Coba login dengan password yang jelas salah, lalu salin teks error yang muncul persis kata per katanya — termasuk tanda baca dan ejaan. Teks ini dimasukkan ke parameter `F=` di command. Jika teks `F=` tidak cocok, Hydra tidak bisa membedakan login gagal dan login berhasil, sehingga semua percobaan dianggap valid.


---


## 6. Setup Virtual Environment (TryHackMe)

Untuk menjalankan room Hydra di TryHackMe, dibutuhkan dua mesin:

**AttackBox (Attacker Machine)** — mesin penyerang yang sudah terinstall Hydra dan tools lainnya. Dijalankan dengan tombol "Start AttackBox". Bisa diakses dalam tampilan split view langsung di browser.

**Target Machine** — mesin yang menjadi sasaran brute force. Dijalankan dengan tombol "Start Machine". Setelah aktif, diakses melalui IP yang diberikan (`MACHINE_IP`) dari browser di AttackBox.

Proses booting Target Machine bisa memakan waktu hingga 3 menit.


---


## 7. Keterbatasan Hydra di Sistem Modern

Hydra efektif pada sistem yang tidak memiliki proteksi modern. Pada platform seperti Instagram atau Facebook, Hydra tidak efektif karena:

- **Rate limiting** — IP diblokir setelah beberapa kali percobaan gagal dalam waktu singkat
- **CAPTCHA** — sistem meminta verifikasi manusia setelah percobaan mencurigakan
- **Two-Factor Authentication (2FA)** — meski password ditemukan, tetap butuh kode OTP
- **Device fingerprinting** — login dari perangkat/lokasi baru langsung diflag sebagai mencurigakan
- **Token CSRF dan sesi dinamis** — form login tidak sederhana dan berubah setiap sesi

Hydra paling efektif digunakan pada: SSH server tanpa konfigurasi rate limiting, router/CCTV dengan password default, web app sederhana tanpa proteksi modern, dan environment lab/CTF.


---


## 8. Glossary

**Brute Force** — metode serangan yang mencoba semua kemungkinan kombinasi password secara otomatis hingga menemukan yang benar.

**Wordlist** — file teks berisi daftar password yang akan dicoba satu per satu oleh Hydra.

**rockyou.txt** — wordlist paling umum digunakan dalam CTF dan pentesting, berisi lebih dari 14 juta password umum. Lokasi default di Kali Linux: `/usr/share/wordlists/rockyou.txt`.

**Thread** — proses yang berjalan secara paralel. Semakin banyak thread, semakin cepat Hydra mencoba password, tapi semakin besar kemungkinan terdeteksi atau menyebabkan error.

**HTTP POST Form** — metode pengiriman data form (termasuk login) ke server. Data dikirim di body request, bukan di URL.

**HTTP GET** — metode pengiriman data di URL. Kurang umum untuk form login.

**`^USER^`** — placeholder di command Hydra yang akan digantikan oleh nilai username saat dijalankan.

**`^PASS^`** — placeholder di command Hydra yang akan digantikan oleh setiap password dari wordlist.

**`F=`** — prefix dalam parameter `invalid_response` yang menandakan teks yang dicari di halaman ketika login gagal.

**Verbose (`-V`)** — mode di mana Hydra menampilkan setiap percobaan login secara real-time di terminal.

**Rate Limiting** — mekanisme proteksi server yang membatasi jumlah request dari satu sumber dalam rentang waktu tertentu.

**CSRF Token** — token acak yang dihasilkan server untuk memverifikasi bahwa request berasal dari sesi yang sah, bukan dari bot atau script.

**AttackBox** — mesin virtual yang disediakan TryHackMe sebagai mesin penyerang, sudah dilengkapi tools keamanan siber termasuk Hydra.

**Target Machine** — mesin virtual yang sengaja dibuat rentan di TryHackMe sebagai target latihan.

**Flag** — string khusus dalam format `THM{...}` yang ditemukan setelah berhasil menyelesaikan task di TryHackMe, digunakan sebagai bukti keberhasilan.

**Split View** — tampilan layar terbagi di TryHackMe yang memungkinkan AttackBox dan browser berjalan berdampingan.


---


## 9. Tools & Platform Rujukan

**Hydra (THC-Hydra)**
Fungsi: Tools brute force utama yang dibahas di room ini.
URL: https://github.com/vanhauser-thc/thc-hydra

**Kali Hydra Tool Page**
Fungsi: Dokumentasi resmi opsi dan penggunaan Hydra di Kali Linux.
URL: https://www.kali.org/tools/hydra/

**rockyou.txt**
Fungsi: Wordlist password paling populer untuk CTF dan pentesting.
Lokasi default: `/usr/share/wordlists/rockyou.txt` (sudah tersedia di Kali Linux dan AttackBox)

**TryHackMe**
Fungsi: Platform belajar keamanan siber berbasis lab interaktif, tempat room Hydra ini dijalankan.
URL: https://tryhackme.com


---


## 10. Catatan Ringkas untuk Ditulis Tangan

### Konsep Inti

- Hydra — tools brute force otomatis untuk menebak kredensial login
- Brute force — coba semua kemungkinan password dari wordlist
- Wordlist — file berisi daftar password untuk dicoba
- rockyou.txt — wordlist standar CTF, lokasi: /usr/share/wordlists/rockyou.txt

### Switch Penting

- -l — satu username langsung
- -L — file berisi daftar username
- -p — satu password langsung (BUKAN file)
- -P — file wordlist (WAJIB huruf besar)
- -t — jumlah thread paralel
- -V — verbose, tampilkan semua percobaan
- -s — tentukan port non-default

### Command Kunci

- FTP: hydra -l user -P wordlist.txt ftp://IP
- SSH: hydra -l root -P passwords.txt IP -t 4 ssh
- Web POST: hydra -l user -P wordlist IP http-post-form "/path:user=^USER^&pass=^PASS^:F=teks_error" -V
- Web POST + port: tambah -s PORT di akhir command

### Placeholder Web Form

- ^USER^ — diganti username saat dijalankan
- ^PASS^ — diganti tiap password dari wordlist
- F= — teks yang muncul di halaman saat login GAGAL

### 3 Hal Wajib Sebelum Hydra Web Form

- Path login (misal /login atau /login.php)
- Nama field input di HTML (name="username", name="password")
- Teks error saat login gagal (salin persis, termasuk tanda baca)

### Setup TryHackMe

- AttackBox — mesin penyerang, start dulu
- Target Machine — mesin target, start setelah AttackBox
- Akses target via http://MACHINE_IP di browser AttackBox

### Keterbatasan Hydra

- Rate limiting — IP diblokir setelah beberapa gagal
- CAPTCHA — butuh interaksi manusia
- 2FA — butuh OTP meski password ketemu
- CSRF token — form modern berubah tiap sesi

### Instalasi

- Ubuntu/Debian: apt install hydra
- Fedora: dnf install hydra
