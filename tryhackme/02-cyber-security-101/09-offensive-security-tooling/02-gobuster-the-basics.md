# Resume Materi: Gobuster — TryHackMe
**Tanggal:** 24 Juni 2026
**Room:** Gobuster (TryHackMe)

---

## 1. Konsep Dasar

### 1.1 Apa itu Gobuster

**Gobuster** adalah alat ofensif open-source yang ditulis dalam bahasa **Golang**. Fungsinya melakukan enumerasi direktori web, file, subdomain DNS, virtual host, Amazon S3 bucket, dan Google Cloud Storage secara brute force menggunakan wordlist.

Dalam fase ethical hacking, Gobuster berada di antara fase **reconnaissance** dan **scanning**.

Pengguna utama: penetration tester, bug bounty hunter, dan praktisi keamanan siber.

### 1.2 Enumeration

**Enumeration** adalah tindakan mendaftar semua sumber daya yang tersedia di sebuah sistem atau jaringan — baik yang bisa diakses maupun tidak. Contoh: mendaftar semua direktori yang ada di sebuah web server.

### 1.3 Brute Force

**Brute force** adalah metode mencoba setiap kemungkinan hingga ada yang cocok. Gobuster menggunakan **wordlist** sebagai daftar kemungkinan yang akan dicoba satu per satu.

### 1.4 Wordlist

**Wordlist** adalah file teks berisi daftar kata yang digunakan Gobuster sebagai amunisi untuk mencoba setiap kemungkinan nama direktori, file, atau subdomain. Tanpa wordlist, Gobuster tidak bisa beroperasi.

---

## 2. Mode-Mode Gobuster

Gobuster memiliki tiga mode utama yang dibahas dalam room ini:

| Mode | Fungsi |
|------|--------|
| `dir` | Enumerasi direktori dan file pada web server |
| `dns` | Enumerasi subdomain DNS |
| `vhost` | Enumerasi virtual host |

Mode lain yang tersedia (tidak dibahas mendalam): `fuzz`, `gcs`, `s3`, `tftp`.

---

## 3. Mode dir — Directory & File Enumeration

### 3.1 Konsep

Mode `dir` digunakan untuk menemukan direktori dan file tersembunyi di sebuah web server. Berguna saat melakukan penetration test untuk melihat struktur direktori target.

Struktur direktori web app sering mengikuti konvensi tertentu (contoh: WordPress selalu punya `/wp-admin`, `/wp-content`, `/wp-includes`) sehingga rentan terhadap brute force dengan wordlist.

Gobuster mengirim **GET request** ke setiap kombinasi URL + entri wordlist, lalu mengembalikan **status code** yang menunjukkan apakah direktori/file tersebut ada atau tidak.

Penting: Gobuster **tidak enumerate secara rekursif**. Jika menemukan direktori yang menarik, kamu harus menjalankan Gobuster lagi secara manual ke direktori tersebut.

### 3.2 Sintaks Dasar

```
gobuster dir -u "http://target.thm" -w /path/to/wordlist
```

Flag `-u` dan `-w` adalah **wajib** untuk mode dir.

### 3.3 Flag Penting Mode dir

**-u / --url**
Menentukan URL target. Harus menyertakan protokol (http/https). Bisa menggunakan IP atau hostname.

**-w / --wordlist**
Path ke file wordlist. Setiap entri wordlist ditambahkan ke URL untuk membentuk request baru.

**-x / --extensions**
Menentukan ekstensi file yang ingin dicari. Contoh: `-x .php,.js`

**-c / --cookies**
Mengonfigurasi cookie yang disertakan di setiap request, seperti session ID.

**-H / --headers**
Mengonfigurasi header HTTP yang disertakan di setiap request.

**-k / --no-tls-validation**
Melewati pengecekan sertifikat TLS/HTTPS. Berguna untuk CTF atau lab yang menggunakan self-signed certificate.

**-n / --no-status**
Menyembunyikan status code dari output. Berguna agar output lebih bersih.

**-P / --password**
Digunakan bersama `--username` untuk request yang memerlukan autentikasi.

**-U / --username**
Digunakan bersama `--password` untuk request yang memerlukan autentikasi.

**-s / --status-codes**
Menentukan status code mana yang ingin ditampilkan. Contoh: `-s 200` atau `-s 300-400`.

**-b / --status-codes-blacklist**
Menentukan status code yang tidak ingin ditampilkan. Menimpa flag `-s` jika keduanya digunakan.

**-r / --followredirect**
Menyuruh Gobuster mengikuti redirect (status code 301/302) yang diterima sebagai respons.

**-t / --threads**
Jumlah thread yang dijalankan secara bersamaan. Default: 10. Semakin besar, semakin cepat tapi semakin berat beban ke server.

**--delay**
Waktu tunggu antar request. Berguna untuk menghindari deteksi oleh mekanisme rate-limiting server.

**--debug**
Menampilkan output debug untuk troubleshooting error yang tidak terduga.

**-o / --output**
Menyimpan hasil enumerasi ke file.

### 3.4 Contoh Perintah Lengkap

```
gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js -t 64 -r
```

Penjelasan:
- `-u` — target URL
- `-w` — wordlist yang digunakan
- `-x .php,.js` — cari juga file berekstensi .php dan .js
- `-t 64` — gunakan 64 thread untuk mempercepat scan
- `-r` — ikuti redirect jika diterima

### 3.5 Catatan URL

- URL harus menyertakan protokol (`http://` atau `https://`)
- URL bisa menggunakan IP atau hostname
- Jika menggunakan IP, pastikan kamu menarget server yang benar (satu IP bisa hosting banyak website via virtual hosting)
- Untuk enumerate subdirektori, ubah URL-nya: `http://target.thm/direktori/`

---

## 4. Mode dns — Subdomain Enumeration

### 4.1 Konsep

Mode `dns` digunakan untuk menemukan subdomain dari sebuah domain. Penting karena subdomain yang berbeda bisa memiliki kerentanan yang berbeda meski domain utamanya sudah aman.

Contoh: `tryhackme.com` aman, tapi `mobile.tryhackme.com` mungkin belum di-patch.

Cara kerja: Gobuster melakukan **DNS lookup** untuk setiap kombinasi entri wordlist + domain. Jika subdomain tersebut resolve (DNS merespons), berarti subdomain itu ada.

### 4.2 Sintaks Dasar

```
gobuster dns -d example.thm -w /path/to/wordlist
```

Flag `-d` dan `-w` adalah **wajib** untuk mode dns.

### 4.3 Flag Penting Mode dns

**-d / --domain**
Menentukan domain target yang akan dienumerasi.

**-r / --resolver**
Mengonfigurasi DNS server kustom untuk melakukan resolving. Sangat berguna ketika domain target hanya bisa di-resolve oleh DNS server tertentu (contoh: domain `.thm` di TryHackMe hanya bisa di-resolve oleh DNS Lab Machine).

**-i / --show-ips**
Menampilkan IP address dari setiap subdomain yang ditemukan.

**-c / --show-cname**
Menampilkan CNAME records. Tidak bisa digunakan bersamaan dengan `-i`.

**-t / --threads**
Jumlah thread. Default: 10.

**--to / --timeout**
Timeout untuk setiap DNS query. Default: 1s.

### 4.4 Contoh Perintah Lengkap

```
gobuster dns -d offensivetools.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -r 10.49.147.126
```

Penjelasan:
- `-d offensivetools.thm` — domain yang dienumerasi
- `-w` — wordlist subdomain
- `-r 10.49.147.126` — gunakan DNS server Lab Machine untuk resolve domain `.thm`

---

## 5. Mode vhost — Virtual Host Enumeration

### 5.1 Konsep

Mode `vhost` digunakan untuk menemukan **virtual host** — website-website berbeda yang berjalan di server fisik yang sama dengan satu IP.

Perbedaan vhost vs subdomain:
- **Subdomain** diatur di DNS dan dicari via DNS lookup
- **Virtual host** berbasis IP, dicari dengan mengirim HTTP request langsung ke server dan mengubah-ubah **Host header**

Perbedaan mode `vhost` vs `dns`:
- `vhost` → navigasi ke URL (IP) + ubah Host header per request
- `dns` → lakukan DNS lookup untuk setiap kombinasi wordlist + domain

### 5.2 Cara Kerja Request vhost

Gobuster mengirim GET request seperti ini untuk setiap entri wordlist:

```
GET / HTTP/1.1
Host: www.example.thm
User-Agent: gobuster/3.6
```

Nilai `Host` terdiri dari tiga bagian:
- **Subdomain** (www) — diisi dari wordlist
- **Second-level domain** (.example) — dikonfigurasi via `--domain`
- **Top-level domain** (.thm) — dikonfigurasi via `--domain`

### 5.3 Sintaks Dasar

```
gobuster vhost -u "http://target" -w /path/to/wordlist
```

Flag `-u` dan `-w` adalah **wajib** untuk mode vhost.

### 5.4 Flag Penting Mode vhost

**-u / --url**
Menentukan base URL target. Biasanya menggunakan IP karena domain `.thm` tidak bisa di-resolve oleh DNS publik.

**--domain**
Menambahkan domain ke setiap entri wordlist untuk membentuk hostname yang valid. Contoh: `--domain example.thm` akan membuat host header menjadi `word.example.thm`.

**--append-domain**
Menambahkan base domain ke setiap kata dalam wordlist. Wajib dikonfigurasi agar hostname terbentuk dengan benar. Tanpa flag ini, host header hanya berisi kata telanjang seperti `www` atau `blog` tanpa domain — menyebabkan false positive massal.

**-m / --method**
Menentukan metode HTTP yang digunakan. Default: GET.

**--exclude-length**
Mengecualikan response berdasarkan panjang body (dalam bytes). Berguna untuk menyaring **false positive** — response yang dilaporkan sebagai "Found" padahal sebenarnya hanya halaman error standar server dengan ukuran yang konsisten.

**-r / --follow-redirect**
Mengikuti HTTP redirect yang diterima sebagai respons.

**-t / --threads**
Jumlah thread. Default: 10.

### 5.5 Contoh Perintah Lengkap

```
gobuster vhost -u "http://10.49.147.126" --domain offensivetools.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 284,285,286,287,288
```

Penjelasan:
- `-u http://10.49.147.126` — target IP server langsung (bypass masalah DNS)
- `--domain offensivetools.thm` — domain untuk membentuk Host header
- `-w` — wordlist subdomain
- `--append-domain` — pastikan domain ditambahkan ke setiap kata wordlist
- `--exclude-length 284,285,286,287,288` — filter false positive berdasarkan ukuran response yang sudah diidentifikasi sebelumnya

### 5.6 Cara Mengidentifikasi False Positive

Jalankan dulu tanpa `--exclude-length`, amati hasil yang muncul. False positive biasanya memiliki **ukuran response yang seragam** (misalnya semua 284-288 bytes). Catat ukuran-ukuran tersebut, lalu jalankan ulang dengan menambahkan `--exclude-length` berisi ukuran-ukuran tersebut.

---

## 6. Flag Global (Berlaku di Semua Mode)

**-t / --threads**
Jumlah thread konkuren. Default: 10.

**-w / --wordlist**
Path ke wordlist.

**--delay**
Waktu tunggu antar request per thread. Contoh: `--delay 300ms`.

**--debug**
Aktifkan output debug untuk troubleshooting.

**-o / --output**
Simpan hasil ke file.

**-q / --quiet**
Sembunyikan banner dan informasi tambahan.

**-v / --verbose**
Tampilkan output verbose (termasuk error).

**--no-color**
Nonaktifkan pewarnaan output.

**--no-error**
Sembunyikan pesan error.

**-z / --no-progress**
Sembunyikan progress bar.

---

## 7. Konfigurasi Environment (TryHackMe)

### 7.1 Setup DNS untuk Domain .thm

Domain `.thm` tidak terdaftar di DNS publik — hanya ada di jaringan internal TryHackMe. Untuk bisa resolve domain `.thm`, DNS server Lab Machine harus dikonfigurasi sebagai nameserver.

Cara konfigurasi di AttackBox:

```
sudo nano /etc/resolv-dnsmasq
```

Tambahkan sebagai baris pertama:

```
nameserver <IP_LAB_MACHINE>
```

Simpan, lalu restart dnsmasq:

```
/etc/init.d/dnsmasq restart
```

### 7.2 Alternatif: Gunakan --resolver

Jika konfigurasi DNS sistem tidak memungkinkan, gunakan flag `--resolver` langsung di perintah Gobuster untuk menentukan DNS server yang dipakai:

```
gobuster dns -d offensivetools.thm -w /wordlist -r <IP_LAB_MACHINE>
```

### 7.3 Alternatif: Gunakan IP Langsung

Untuk mode `dir` dan `vhost`, DNS tidak diperlukan jika menggunakan IP langsung di flag `-u`:

```
gobuster dir -u http://<IP_LAB_MACHINE> -w /wordlist
```

---

## 8. Perbandingan Mode dns vs vhost

| Aspek | dns | vhost |
|-------|-----|-------|
| Cara kerja | DNS lookup | HTTP request dengan Host header |
| Target di -u / -d | Domain | IP atau URL |
| Kebutuhan DNS | Ya | Tidak (bisa langsung pakai IP) |
| Yang ditemukan | Subdomain terdaftar di DNS | Virtual host di server yang sama |

---

## 9. Glossary

**Enumeration** — Tindakan mendaftar semua sumber daya yang tersedia, baik yang bisa diakses maupun tidak.

**Brute Force** — Metode mencoba setiap kemungkinan hingga ada yang cocok.

**Wordlist** — File teks berisi daftar kata yang digunakan sebagai input brute force.

**Virtual Host (vhost)** — Beberapa website berbeda yang berjalan di satu server fisik dengan satu IP yang sama.

**Subdomain** — Bagian dari domain yang berada satu level di bawah domain utama, diatur via DNS.

**FQDN (Fully Qualified Domain Name)** — Nama domain lengkap termasuk semua levelnya. Contoh: `www.example.thm.`

**CNAME (Canonical Name Record)** — Record DNS yang memetakan satu nama domain ke nama domain lain (alias).

**Status Code** — Kode numerik dalam response HTTP yang menunjukkan hasil request. Contoh: 200 (OK), 301 (Redirect), 403 (Forbidden), 404 (Not Found).

**False Positive** — Hasil yang dilaporkan sebagai "ditemukan" padahal sebenarnya bukan hasil yang valid.

**DNS Resolver** — Server yang bertugas mentranslasikan nama domain menjadi IP address.

**Self-signed Certificate** — Sertifikat TLS yang ditandatangani sendiri (bukan oleh Certificate Authority resmi), umum digunakan di lingkungan lab/CTF.

**Reconnaissance** — Fase pertama dalam ethical hacking — pengumpulan informasi tentang target.

**Penetration Testing** — Pengujian keamanan terotorisasi untuk menemukan kerentanan sebelum penyerang menemukannya.

**Thread** — Unit eksekusi yang berjalan secara bersamaan. Semakin banyak thread, semakin cepat scan tapi semakin berat beban ke server.

---

## 10. Tools & Platform Rujukan

**Gobuster**
Alat utama yang dibahas di room ini. Untuk enumerasi direktori, file, subdomain DNS, dan virtual host.
URL: https://github.com/OJ/reeves/gobuster

**SecLists**
Koleksi wordlist untuk berbagai keperluan security testing, termasuk subdomain enumeration.
Path di Kali/AttackBox: `/usr/share/wordlists/SecLists/`
URL: https://github.com/danielmiessler/SecLists

**DirBuster Wordlists**
Wordlist untuk enumerasi direktori web.
Path di Kali/AttackBox: `/usr/share/wordlists/dirbuster/`

**Kali Linux**
Distribusi Linux yang sudah menyertakan Gobuster secara default.
URL: https://www.kali.org

---

## 11. Catatan Ringkas untuk Ditulis Tangan

### Konsep Inti

- Gobuster — alat brute force enumerasi (direktori, file, subdomain, vhost)
- Enumeration — daftar semua resource, bisa diakses atau tidak
- Brute Force — coba semua kemungkinan sampai cocok
- Wordlist — file daftar kata sebagai amunisi Gobuster
- Gobuster posisi di fase: antara reconnaissance dan scanning

### 3 Mode Utama

- dir — enumerasi direktori & file web
- dns — enumerasi subdomain via DNS lookup
- vhost — enumerasi virtual host via HTTP Host header

### Mode dir — Flag Kunci

- -u — URL target (wajib, harus ada protokol http/https)
- -w — path wordlist (wajib)
- -x — ekstensi file yang dicari (contoh: -x .php,.js)
- -t — jumlah thread, default 10
- -r — ikuti redirect
- -k — skip TLS validation (untuk https dengan self-signed cert)
- -s — tampilkan hanya status code tertentu
- -b — blacklist status code tertentu
- --delay — jeda antar request (contoh: --delay 300ms)
- -o — simpan output ke file
- Gobuster dir TIDAK rekursif — harus manual per direktori

### Mode dns — Flag Kunci

- -d — domain target (wajib)
- -w — wordlist (wajib)
- -r — DNS resolver kustom (penting untuk domain .thm)
- -i — tampilkan IP subdomain
- -c — tampilkan CNAME (tidak bisa barengan -i)
- --to — timeout DNS query, default 1s

### Mode vhost — Flag Kunci

- -u — URL/IP target (wajib, pakai IP untuk bypass DNS)
- -w — wordlist (wajib)
- --domain — domain untuk Host header (contoh: --domain example.thm)
- --append-domain — wajib agar wordlist + domain terbentuk dengan benar
- --exclude-length — filter false positive berdasarkan ukuran response
- -m — metode HTTP (default GET)

### vhost vs dns

- dns → DNS lookup → butuh DNS bisa resolve
- vhost → ubah Host header → langsung ke IP, tidak butuh DNS

### Setup DNS .thm

- Edit /etc/resolv-dnsmasq → tambah nameserver IP_LAB
- Restart: /etc/init.d/dnsmasq restart
- Atau pakai: gobuster dns -r IP_LAB_MACHINE langsung di command

### Cara Handle False Positive (vhost)

- Jalankan dulu tanpa --exclude-length
- Perhatikan ukuran response yang seragam di false positive
- Jalankan ulang dengan --exclude-length ukuran1,ukuran2,...

### Wordlist yang Umum Dipakai

- dir: /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
- dns/vhost: /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
- dirb: /usr/share/wordlists/dirb/common.txt, big.txt

### Istilah Kunci

- FQDN — nama domain lengkap semua level
- CNAME — alias DNS (satu nama → nama lain)
- Status 200 — OK / ditemukan
- Status 301/302 — redirect
- Status 403 — forbidden
- Status 404 — not found
- False positive — dilaporkan "found" tapi bukan hasil valid
- Self-signed cert — pakai -k untuk bypass
- Thread — eksekusi paralel, lebih banyak = lebih cepat tapi beban lebih berat
