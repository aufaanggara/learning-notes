# Offensive Security Introduction — Resume Materi

Room: Offensive Security Intro (TryHackMe)
Tanggal: 17 Agustus 2026

---

## 1. Konsep Inti: Offensive Security

### 1.1 Definisi

**Offensive security** adalah pendekatan keamanan siber di mana seseorang berpikir dan bertindak seperti penyerang dengan tujuan menemukan kelemahan pada sistem sebelum penyerang sungguhan menemukannya lebih dulu. Pendekatan ini bersifat proaktif — bukan menunggu serangan terjadi, melainkan secara aktif mencari celah.

Praktik dilakukan dalam lingkungan yang aman dan legal, artinya target yang diserang adalah sistem simulasi (seperti lab FakeBank pada room ini) atau sistem nyata yang sudah mendapat izin resmi dari pemiliknya. Tanpa izin, tindakan yang sama akan berubah status menjadi ilegal meskipun teknik yang dipakai identik.

### 1.2 Perbandingan dengan Defensive Security

**Offensive security** berfokus pada mencari dan mengeksploitasi celah, yaitu menyerang untuk menguji ketahanan sistem. Sebaliknya, **defensive security** berfokus pada mendeteksi, mencegah, dan merespons serangan pada sistem yang sudah berjalan.

Kedua pendekatan ini saling melengkapi. Temuan dari sisi offensive menjadi dasar perbaikan yang dilakukan di sisi defensive, sehingga keduanya bukan pendekatan yang bersaing melainkan dua sisi dari satu siklus keamanan yang sama.

---

## 2. Metodologi Dasar yang Dipraktikkan

Room ini mempraktikkan dua tahap dasar dari sebuah serangan web sederhana, yaitu enumeration dan exploitation.

### 2.1 Enumeration (Content Discovery)

Tahap ini bertujuan memetakan sisi aplikasi yang tidak terlihat langsung oleh pengguna biasa, khususnya halaman-halaman yang tidak ditautkan di navigasi utama situs namun sebenarnya masih bisa diakses jika alamatnya diketahui. Kesalahan konfigurasi seperti ini disebut **hidden pages**, dan menjadi salah satu penyebab kebocoran akses yang paling umum ditemukan di aplikasi web nyata.

Teknik yang dipakai untuk menemukan hidden pages disebut **directory brute force**: mencoba banyak kemungkinan nama direktori atau file secara berurutan terhadap sebuah URL target, lalu mencatat mana saja yang merespons sebagai halaman valid.

### 2.2 Exploitation (Broken Access Control)

Setelah halaman tersembunyi ditemukan, tahap berikutnya adalah menguji apakah halaman tersebut benar-benar terlindungi. Pada studi kasus FakeBank, admin panel yang ditemukan ternyata dapat diakses langsung tanpa proses login atau autentikasi apa pun.

Kelemahan jenis ini disebut **broken access control** — kondisi di mana suatu fungsi yang seharusnya dibatasi hanya untuk pengguna dengan hak akses tertentu (misalnya staff atau admin) justru bisa diakses dan digunakan oleh siapa saja yang mengetahui URL-nya.

Dampak dari broken access control pada kasus ini bersifat langsung dan konkret: pengguna tanpa otorisasi dapat memanipulasi data finansial, dalam hal ini menambah saldo rekening, hanya dengan mengetik URL tertentu dan mengisi form yang tersedia di halaman tersebut.

---

## 3. Studi Kasus: FakeBank

### 3.1 Alur Serangan

1. Target aplikasi adalah `http://fakebank.thm`, sebuah simulasi aplikasi perbankan.
2. Enumeration dijalankan terhadap domain target untuk menemukan halaman tersembunyi. Hasil scan menemukan dua URL, salah satunya adalah endpoint admin.
3. Endpoint admin diakses langsung melalui browser dengan menambahkan path `/bank-transfer` ke URL utama, tanpa proses login sama sekali.
4. Di dalam admin panel tersedia form untuk mendeposit uang ke nomor akun tertentu. Kelemahan dieksploitasi dengan memasukkan nomor akun milik sendiri dan mengisi nominal deposit, lalu menekan tombol submit.
5. Perubahan saldo pada akun membuktikan bahwa eksploitasi berhasil. Ini adalah bentuk sederhana dari **proof of concept**, yaitu bukti nyata bahwa suatu kerentanan benar-benar bisa dimanfaatkan, bukan sekadar dugaan teoritis.

### 3.2 Inti Pelajaran dari Kasus Ini

Kelemahan yang dieksploitasi bukan berasal dari kerentanan teknis yang rumit seperti bug pada kode, melainkan dari kesalahan desain akses: halaman sensitif yang seharusnya memerlukan autentikasi malah dibiarkan terbuka.

Ini menegaskan bahwa proses enumeration, yang secara sederhana berarti mencari halaman yang tersembunyi, sudah cukup untuk membuka jalan eksploitasi jika kontrol akses di baliknya tidak diterapkan dengan benar.

---

## 4. Tool dan Command yang Dipakai

### 4.1 Dirb

**Dirb** adalah tool untuk directory brute force, yaitu mencoba daftar nama direktori atau file dari sebuah wordlist terhadap URL target, lalu melaporkan mana saja yang benar-benar ada di server.

```
dirb http://fakebank.thm
```

Penjelasan bagian command:

- `dirb` — memanggil tool dirb.
- `http://fakebank.thm` — argumen wajib berupa URL target yang akan di-brute force; dirb akan menggunakan wordlist bawaannya untuk mencoba berbagai kemungkinan path terhadap domain ini.

Cara membaca output: setiap baris hasil yang diawali tanda `+` menunjukkan sebuah path yang berhasil ditemukan, artinya server memberi respons valid dan bukan halaman kosong atau tidak ada. Baris tanpa tanda tersebut bukan hasil temuan dan bisa diabaikan.

---

## 5. Istilah Penting yang Wajib Dihapal

**Offensive security** — pendekatan proaktif yang mencari kelemahan sistem dengan cara mensimulasikan serangan, dilakukan sebelum penyerang sungguhan menemukannya.

**Defensive security** — pendekatan yang berfokus pada mendeteksi, mencegah, dan merespons ancaman terhadap sistem yang sudah berjalan.

**Ethical hacking** — praktik hacking yang dilakukan secara legal, di lingkungan aman atau dengan izin resmi, untuk tujuan menemukan dan melaporkan kelemahan keamanan.

**Enumeration / content discovery** — proses aktif memetakan bagian-bagian sistem atau aplikasi yang belum diketahui, seperti halaman, direktori, atau service yang berjalan.

**Directory brute force** — teknik mencoba banyak kemungkinan nama direktori atau file secara sistematis terhadap sebuah target untuk menemukan halaman yang tidak terlihat secara publik.

**Hidden pages** — halaman pada suatu aplikasi web yang tidak ditautkan di navigasi atau menu utama, namun tetap dapat diakses apabila URL-nya diketahui.

**Broken access control** — kelemahan keamanan di mana kontrol pembatasan akses tidak diterapkan dengan benar, sehingga fungsi yang seharusnya terbatas, misalnya khusus admin, dapat diakses oleh pengguna yang tidak berwenang.

**Admin panel** — antarmuka pengelolaan sistem yang seharusnya hanya bisa diakses oleh administrator atau staf berwenang.

**Proof of concept** — bukti nyata dan terverifikasi bahwa suatu kerentanan memang bisa dieksploitasi, bukan sekadar asumsi teoritis.