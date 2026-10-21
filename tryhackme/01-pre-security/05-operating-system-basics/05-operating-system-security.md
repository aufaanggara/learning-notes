# Resume Materi — OS Security (Beginner)
**TryHackMe | Tanggal: 21 Oktober 2026**

---

## 1. Konsep Dasar Sistem Operasi

### 1.1 Hardware

**Hardware** adalah semua komponen fisik komputer yang dapat disentuh secara langsung. Contohnya mencakup CPU, RAM, HDD/SSD, keyboard, layar, printer, dan USB flash memory.

Komponen utama yang ada di dalam desktop board:

- **CPU** (Central Processing Unit) — unit pemrosesan utama
- **RAM** (Random Access Memory) — memori sementara saat sistem berjalan
- **Storage** (HDD/SSD) — penyimpanan data permanen, biasanya terhubung ke desktop board

### 1.2 Software

**Software** terbagi menjadi dua lapisan:

- **Programs** — aplikasi yang dijalankan pengguna: Firefox, Chrome, MS Office, WhatsApp, Telegram
- **Operating System** — sistem yang menjadi perantara antara hardware dan program

### 1.3 Peran Operating System

**Operating System (OS)** adalah lapisan yang duduk di antara hardware dan program/aplikasi yang berjalan. Program tidak dapat mengakses hardware secara langsung — mereka harus melewati OS, yang kemudian mengatur akses ke hardware sesuai aturan tertentu.

Tanpa OS, komponen hardware tidak bisa digunakan untuk menjalankan program apapun.

---

## 2. Jenis-Jenis Operating System

OS dirancang untuk berbeda kebutuhan:

| Kategori | Contoh OS |
|---|---|
| Desktop / Laptop | MS Windows 11, macOS |
| Smartphone | Android, iOS |
| Server | MS Windows Server 2022, IBM AIX, Oracle Solaris |
| Universal (desktop & server) | Linux |

**Pangsa pasar OS untuk browsing internet (Januari 2022, Statcounter):**

- Android — 39.4%
- Windows — 32.1%
- iOS — 17.6%
- OSX — 6.7%
- Unknown — 1.7%

---

## 3. CIA Triad — Tiga Pilar Keamanan

Ketika membahas keamanan OS, tiga hal ini yang harus dilindungi:

**Confidentiality** — memastikan file dan informasi rahasia hanya dapat diakses oleh pihak yang memang berhak menerimanya.

**Integrity** — memastikan tidak ada pihak yang dapat merusak atau memodifikasi file yang tersimpan di sistem maupun saat file sedang ditransfer melalui jaringan.

**Availability** — memastikan laptop atau smartphone dapat digunakan kapanpun pemiliknya membutuhkannya.

Serangan siber pada umumnya menargetkan satu atau lebih dari ketiga pilar ini.

---

## 4. Tiga Kelemahan Utama yang Dieksploitasi

### 4.1 Authentication dan Weak Passwords

**Authentication** adalah proses memverifikasi identitas pengguna, baik di sistem lokal maupun sistem jarak jauh. Terdapat tiga metode autentikasi utama:

- **Something you know** — password atau PIN
- **Something you are** — sidik jari, pengenalan wajah
- **Something you have** — nomor HP yang menerima OTP/SMS

Password adalah metode autentikasi yang paling umum sekaligus yang paling sering diserang. Banyak pengguna menggunakan password yang mudah ditebak, mengulang password yang sama di banyak situs, atau menggunakan data pribadi seperti tanggal lahir dan nama hewan peliharaan.

**20 Password Paling Umum (NCSC — 100,000 Most Common Passwords):**

| Rank | Password | Rank | Password |
|---|---|---|---|
| 1 | 123456 | 11 | 1234567890 |
| 2 | 123456789 | 12 | 123123 |
| 3 | qwerty | 13 | 000000 |
| 4 | password | 14 | iloveyou |
| 5 | 111111 | 15 | 1234 |
| 6 | 12345678 | 16 | 1q2w3e4r5t |
| 7 | abc123 | 17 | qwertyuiop |
| 8 | 1234567 | 18 | 123 |
| 9 | password1 | 19 | monkey |
| 10 | 12345 | 20 | dragon |

Pola berbahaya yang wajib dihindari:

- Urutan angka berurutan: 123, 1234, 12345, dst
- Kata kamus umum: password, iloveyou, monkey, dragon
- Pola tata letak keyboard: qwerty, qwertyuiop, 1q2w3e4r5t — terlihat kompleks tapi sangat mudah diprediksi karena mengikuti urutan fisik tombol keyboard

Jika penyerang berhasil menebak password akun email atau media sosial seseorang, mereka mendapat akses penuh ke data pribadi korban. Wajib menggunakan password yang kompleks dan password yang berbeda untuk setiap akun.

### 4.2 Weak File Permissions

**Prinsip Least Privilege** — setiap pengguna hanya boleh memiliki akses ke file dan resource yang benar-benar mereka butuhkan untuk menyelesaikan pekerjaan mereka, tidak lebih.

Izin file yang lemah memungkinkan penyerang untuk:

- Menyerang **Confidentiality** — mengakses file yang seharusnya tidak bisa mereka lihat
- Menyerang **Integrity** — memodifikasi file yang seharusnya tidak bisa mereka edit

### 4.3 Malicious Programs

Bergantung pada jenisnya, program berbahaya dapat menyerang satu atau lebih pilar CIA Triad.

**Trojan Horse** — menyerang Confidentiality dan Integrity. Program ini memberi penyerang akses ke dalam sistem korban, sehingga penyerang dapat membaca maupun memodifikasi file.

**Ransomware** — menyerang Availability. Program ini mengenkripsi file korban sehingga tidak dapat dibaca tanpa mengetahui password enkripsinya (file menjadi tidak karuan/gibberish). Penyerang kemudian menawarkan pemulihan akses dengan memberikan password enkripsi jika korban bersedia membayar "ransom" (uang tebusan). Proses membalik enkripsi disebut **decryption**.

---

## 5. Akun Administrator Sistem

Akun dengan hak akses penuh tanpa batas ke seluruh sistem:

| OS | Nama Akun Admin |
|---|---|
| Linux, Android, Apple | `root` |
| MS Windows | `administrator` |

Akun ini menjadi target utama privilege escalation dalam skenario serangan — setelah berhasil masuk ke akun biasa, penyerang berusaha mendapatkan akses ke akun ini.

---

## 6. Command Linux Dasar (Lab Praktis)

Semua command berikut dijalankan melalui terminal.

```
whoami
```
Menampilkan nama user yang sedang aktif/login saat ini.

```
ssh USERNAME@MACHINE_IP
```
Melakukan remote login ke mesin lain melalui protokol SSH (Secure Shell). Username dan IP address mesin target wajib diisi.

```
ls
```
Menampilkan daftar semua file di direktori saat ini, kecuali file tersembunyi (hidden files).

```
cat FILENAME
```
Menampilkan isi konten dari sebuah file teks langsung ke layar terminal. Singkatan dari *concatenate*.

```
history
```
Menampilkan daftar semua perintah yang pernah diketik oleh user yang sedang aktif di terminal, beserta nomor urutnya.

```
su - USERNAME
```
Berpindah ke akun user lain (Switch User). Tanda `-` memastikan environment variable user tujuan juga dimuat sepenuhnya.

```
su - root
```
Berpindah ke akun root (administrator penuh sistem).

---

## 7. Skenario Lab: Attack Path Praktis

Skenario ini mendemonstrasikan bagaimana kelemahan password dan history command bisa dieksploitasi secara berantai.

**Langkah 1 — Masuk sebagai Sammie**

Ditemukan sticky note di layar monitor klien berisi dua kata: `sammie` dan `dragon`. Dari AttackBox, dijalankan:

```
ssh sammie@MACHINE_IP
```

Password `dragon` (ranking 20 password paling umum) berhasil. Login berhasil.

**Langkah 2 — Eksplorasi sistem Sammie**

Setelah login, dijalankan `whoami` untuk konfirmasi identitas, `ls` untuk melihat daftar file, dan `cat draft.md` untuk membaca isinya. Isi file berisi pesan tentang bahaya reuse password.

**Langkah 3 — Login ke akun Johnny**

Diketahui ada dua user lain: `johnny` dan `linda`. Password johnny berhasil ditebak menggunakan daftar top password umum — jawabannya adalah `abc123` (ranking 7).

**Langkah 4 — Eksploitasi history command**

Setelah login sebagai johnny, dijalankan `history`. Di dalam riwayat perintah ditemukan:

```
7   su - root
8   happyHack!NG
9   su - root
```

Johnny secara tidak sengaja mengetik password root-nya (`happyHack!NG`) sebagai perintah biasa alih-alih memasukkannya di prompt password. Akibatnya, password tersebut tersimpan permanen di history.

**Langkah 5 — Escalate ke root**

Dengan password yang ditemukan dari history, dijalankan `su - root` dan berhasil masuk sebagai root. Setelah masuk, `ls` menampilkan file `flag.txt`, dan `cat flag.txt` menampilkan isinya: `THM{YouGotRoot}`.

---

## 8. Catatan Penting SSH

Saat pertama kali terhubung ke server via SSH, sistem akan menampilkan peringatan tentang keaslian server dan SSH key. Jawaban yang diperlukan adalah `yes` untuk melanjutkan koneksi.

Ketika memasukkan password via SSH, **tidak ada indikator apapun yang muncul di layar** — tidak ada bintang, titik, atau karakter lain. Ini adalah perilaku normal SSH. Sistem tetap menerima input password yang diketik meskipun layar tidak menampilkan apapun.

---

## 9. Celah Keamanan History Command

Ini adalah celah yang sering diabaikan namun sangat berbahaya: jika seorang pengguna tidak sengaja mengetik password mereka sebagai perintah biasa (bukan di prompt password), password tersebut tersimpan dalam riwayat perintah dan dapat dibaca oleh siapapun yang berhasil mengakses akun tersebut.

Implikasinya: setelah berhasil login ke akun seseorang via SSH, penyerang dapat langsung menjalankan `history` dan memeriksa seluruh riwayat aktivitas user tersebut, termasuk kemungkinan adanya password yang tidak sengaja diketik sebagai command.

---

## 10. Glossary

**Authentication** — proses verifikasi identitas pengguna sebelum diberikan akses ke sistem.

**Availability** — pilar CIA Triad; memastikan sistem dapat diakses kapanpun dibutuhkan oleh pemiliknya.

**Confidentiality** — pilar CIA Triad; memastikan informasi hanya dapat diakses oleh pihak yang berhak.

**Decryption** — proses membalik enkripsi agar file kembali dapat dibaca.

**Encryption** — proses mengacak data sehingga tidak dapat dibaca tanpa mengetahui kunci enkripsinya.

**Hardware** — komponen fisik komputer yang dapat disentuh (CPU, RAM, layar, keyboard, dll).

**Integrity** — pilar CIA Triad; memastikan tidak ada pihak yang dapat merusak atau memodifikasi data tanpa izin.

**Least Privilege** — prinsip keamanan yang menyatakan setiap pengguna hanya boleh mendapat akses minimum sesuai kebutuhan kerjanya.

**Malware** — istilah umum untuk program berbahaya (Trojan, Ransomware, Virus, dll).

**Operating System** — lapisan software yang menjadi perantara antara hardware dan program aplikasi.

**Ransomware** — jenis malware yang mengenkripsi file korban dan meminta tebusan untuk pemulihan akses.

**Root / Administrator** — akun dengan hak akses penuh tanpa batas ke seluruh sistem.

**SSH (Secure Shell)** — protokol untuk melakukan remote login ke sistem lain secara terenkripsi.

**Trojan Horse** — jenis malware yang menyamar sebagai program normal namun memberi penyerang akses ke sistem korban.

---

## 11. Tools & Platform Rujukan

**NCSC (National Cyber Security Centre)**
Menerbitkan daftar 100,000 password paling umum yang digunakan sebagai referensi evaluasi kelemahan password.
URL: https://www.ncsc.gov.uk

**Statcounter**
Platform statistik yang menyediakan data pangsa pasar OS dan browser berdasarkan data browsing global.
URL: https://statcounter.com

---

## 12. Catatan Ringkas untuk Ditulis Tangan

### Definisi Dasar

- Hardware — komponen fisik komputer (CPU, RAM, storage, keyboard, layar)
- OS — lapisan antara hardware & program; mengatur akses program ke hardware
- Software — 2 jenis: Programs (Chrome, WhatsApp) & OS (Windows, Android)

### Jenis OS

- Desktop: Windows 11, macOS
- Smartphone: Android, iOS
- Server: Windows Server 2022, IBM AIX, Oracle Solaris
- Universal: Linux

### Market Share OS (Jan 2022)

- Android 39.4% | Windows 32.1% | iOS 17.6% | OSX 6.7%

### CIA Triad

- Confidentiality — hanya orang berhak yang bisa akses
- Integrity — tidak bisa diubah tanpa izin
- Availability — sistem harus bisa diakses kapanpun

### 3 Kelemahan Utama

- Authentication & Weak Passwords
- Weak File Permissions
- Malicious Programs

### 3 Metode Autentikasi

- Something you know — password, PIN
- Something you are — sidik jari
- Something you have — OTP/SMS

### Pola Password Berbahaya

- Urutan angka: 123, 1234, 12345, dst
- Kata kamus: password, iloveyou, monkey, dragon
- Pola keyboard: qwerty, qwertyuiop, 1q2w3e4r5t

### Akun Admin Sistem

- root — admin di Linux/Android/Apple
- administrator — admin di Windows
- Keduanya: akses penuh tanpa batas

### Least Privilege

- Prinsip: user hanya boleh akses apa yang mereka butuhkan
- Izin lemah → serang Confidentiality (baca) & Integrity (modifikasi)

### Malicious Programs

- Trojan Horse — beri akses penyerang, serang C & I
- Ransomware — enkripsi file, minta tebusan, serang Availability
- Decryption — proses membalik enkripsi

### Command Linux (Lab)

- whoami — tampilkan user aktif saat ini
- ssh USER@IP — remote login ke mesin lain
- ls — list semua file di direktori
- cat FILENAME — tampilkan isi file ke layar
- history — tampilkan riwayat semua perintah user
- su - USERNAME — pindah ke akun user lain
- su - root — pindah ke akun root

### Celah History

- Password salah ketik sebagai command → tersimpan di history
- Setelah login akun orang via SSH → jalankan history → bisa temukan password

### SSH Notes

- Koneksi pertama: jawab yes untuk konfirmasi
- Saat ketik password via SSH: tidak ada indikator di layar, tapi sistem tetap terima input
