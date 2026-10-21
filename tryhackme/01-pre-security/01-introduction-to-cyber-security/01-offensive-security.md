# Offensive Security Intro: Resume Materi

Room: Offensive Security Intro (TryHackMe)

Tanggal: 21 Oktober 2026


## 1. Konsep Inti

### 1.1 Offensive Security

**Offensive security** adalah pendekatan keamanan siber yang berpikir dan bertindak seperti penyerang untuk menemukan kelemahan sistem sebelum penyerang sungguhan menemukannya.

Sifatnya proaktif. Alih-alih menunggu serangan terjadi, kelemahan dicari dan diuji secara aktif.

> Analogi: seperti menyewa orang untuk mencoba membobol rumah sendiri, supaya pintu atau jendela yang lemah ketahuan dan diperbaiki sebelum pencuri asli datang.

Istilah untuk kegiatan mensimulasikan tindakan hacker guna menemukan kelemahan adalah offensive security itu sendiri.

### 1.2 Pembanding: Defensive Security

**Defensive security** berfokus pada mendeteksi, mencegah, dan merespons serangan terhadap sistem yang sudah berjalan.

Offensive mencari celah dengan menyerang, defensive menutup celah dan menangkal serangan. Keduanya saling melengkapi: temuan dari sisi offensive menjadi bahan perbaikan di sisi defensive.

### 1.3 Ethical Hacking dan Lingkungan Aman

Latihan pada room ini dilakukan di lingkungan yang aman dan legal, yaitu aplikasi simulasi yang memang disediakan untuk diserang.

Teknik yang sama terhadap sistem tanpa izin pemilik adalah tindakan ilegal. Izin dan lingkungan yang terkontrol yang membedakan ethical hacking dari kejahatan siber.


## 2. Metodologi yang Dipraktikkan

Room ini memperkenalkan dua tahap dasar serangan web, yaitu enumeration lalu exploitation.

### 2.1 Enumeration (Content Discovery)

Tujuannya memetakan bagian aplikasi yang tidak terlihat oleh pengguna biasa, terutama halaman yang tidak ditautkan di menu atau navigasi namun tetap bisa diakses kalau URL-nya diketahui.

Halaman seperti ini disebut **hidden pages**. Meninggalkan halaman sensitif tetap accessible adalah salah satu kesalahan paling umum pada aplikasi web.

Teknik yang dipakai adalah **directory brute force**: mencoba banyak kemungkinan nama direktori atau file terhadap URL target, lalu mencatat mana yang benar-benar ada di server.

### 2.2 Exploitation (Broken Access Control)

Setelah hidden page ditemukan, langkah berikutnya adalah menguji apakah halaman itu dilindungi autentikasi.

Jika fungsi yang seharusnya terbatas (misalnya admin panel) ternyata bisa diakses siapa saja yang tahu URL-nya, kondisi ini disebut **broken access control**.

Dampaknya langsung terasa: pengguna tanpa otorisasi bisa mengubah data penting, pada kasus room ini menambah saldo rekening.


## 3. Tools dan Command

### 3.1 Dirb

**Dirb** adalah tool directory brute force. Ia mencoba daftar nama path dari wordlist bawaannya terhadap sebuah URL, lalu melaporkan path yang ditemukan.

```
dirb http://fakebank.thm
```

Penjelasan bagian command:

- `dirb`: memanggil tool.
- `http://fakebank.thm`: URL target yang akan di-brute force.

Room ini hanya memakai bentuk dasar tanpa switch tambahan, sehingga tidak ada flag lain yang perlu dihapal.

Cara membaca output: baris yang diawali tanda `+` adalah halaman yang berhasil ditemukan. Pada target latihan, dirb menemukan dua URL, dan salah satunya adalah endpoint admin.

### 3.2 Browser

Hasil temuan dirb dibuka lewat browser dengan menambahkan path temuan ke URL utama. Pada kasus ini path-nya `/bank-transfer`, sehingga URL lengkapnya:

```
http://fakebank.thm/bank-transfer
```


## 4. Studi Kasus: FakeBank

### 4.1 Gambaran Target

FakeBank adalah simulasi aplikasi internet banking di `http://fakebank.thm`. Tujuan latihan: menemukan kelemahan dengan memanfaatkan halaman yang tidak dijaga.

### 4.2 Alur Serangan

1. Jalankan dirb terhadap `http://fakebank.thm` untuk menemukan hidden pages.
2. Dari dua URL yang ditemukan, identifikasi endpoint admin (`/bank-transfer`).
3. Akses endpoint tersebut langsung lewat browser. Halaman Admin Portal terbuka tanpa proses login.
4. Isi form deposit: pilih nomor akun sendiri (8881) lalu masukkan nominal, kemudian kirim.
5. Cek halaman akun. Saldo yang berubah menjadi positif membuktikan eksploitasi berhasil, dan aplikasi menampilkan pop-up konfirmasi berwarna hijau.

### 4.3 Inti Pelajaran

Kelemahan ini bukan bug kode yang rumit, melainkan kesalahan desain akses: halaman sensitif dibiarkan terbuka tanpa autentikasi.

Artinya, enumeration yang sederhana pun sudah cukup membuka jalan eksploitasi bila kontrol akses tidak diterapkan.

Perubahan saldo tadi berfungsi sebagai **proof of concept**, yaitu bukti nyata bahwa kerentanan bisa dimanfaatkan, bukan sekadar teori.


## 5. Istilah Penting yang Wajib Dihapal

**Offensive security**: pendekatan proaktif mencari kelemahan sistem dengan mensimulasikan serangan sebelum penyerang asli melakukannya.

**Defensive security**: pendekatan yang berfokus mendeteksi, mencegah, dan merespons serangan pada sistem yang berjalan.

**Ethical hacking**: hacking legal, dilakukan dengan izin atau di lingkungan aman, untuk menemukan dan melaporkan kelemahan.

**Enumeration / content discovery**: proses memetakan bagian sistem atau aplikasi yang belum diketahui, seperti halaman, direktori, atau service.

**Directory brute force**: mencoba banyak kemungkinan nama direktori atau file secara sistematis terhadap target.

**Hidden pages**: halaman web yang tidak ditautkan di navigasi namun tetap bisa diakses bila URL-nya diketahui.

**Broken access control**: kontrol pembatasan akses tidak diterapkan dengan benar sehingga fungsi terbatas bisa dipakai pengguna tidak berwenang.

**Admin panel**: antarmuka pengelolaan sistem yang seharusnya hanya bisa diakses administrator atau staf berwenang.

**Proof of concept**: bukti terverifikasi bahwa suatu kerentanan benar-benar bisa dieksploitasi.


## 6. Tools & Platform Rujukan

- **TryHackMe**: platform belajar tempat room ini dijalankan, menyediakan lab dan mesin simulasi.
- **Dirb**: tool directory brute force untuk menemukan hidden pages.
- **Firefox (simulated desktop)**: browser di desktop simulasi untuk membuka halaman target.
- **Terminal (simulated desktop)**: tempat menjalankan dirb pada mesin lab.

Materi tidak menyebutkan URL eksternal tambahan, jadi tidak ada link yang dicantumkan.


## 7. Catatan Ringkas untuk Ditulis Tangan

### 7.1 Konsep

- Offensive security: pikir seperti penyerang, cari celah duluan (proaktif)
- Defensive security: deteksi, cegah, respons serangan
- Offensive + defensive: saling melengkapi
- Ethical hacking: legal, ada izin, lingkungan aman
- Tanpa izin: ilegal walau teknik sama

### 7.2 Tahapan

- Enumeration: petakan halaman/direktori tersembunyi
- Exploitation: manfaatkan celah yang ditemukan
- Urutan: enumeration dulu, baru exploitation

### 7.3 Kelemahan

- Hidden pages: tidak ada di menu, tetap bisa diakses via URL
- Broken access control: fungsi admin bisa diakses tanpa autentikasi
- Directory brute force: tebak banyak nama path ke server
- Proof of concept: bukti kerentanan benar-benar bisa dieksploitasi

### 7.4 Command

- dirb http://fakebank.thm — brute force direktori target
- dirb — nama tool
- http://fakebank.thm — URL target
- Baris output diawali + — path ditemukan
- Hasil dirb: 2 URL, salah satunya endpoint admin
- http://fakebank.thm/bank-transfer — admin panel (dibuka via browser)

### 7.5 Studi Kasus FakeBank

- Target: simulasi aplikasi banking
- Langkah: dirb, buka /bank-transfer, deposit ke akun 8881, cek saldo
- Admin panel terbuka tanpa login
- Saldo positif — pop-up hijau tanda berhasil
- Pelajaran: salah desain akses, bukan bug kode rumit

### 7.6 Istilah Lain

- Admin panel: antarmuka kelola sistem khusus admin
- Content discovery: sinonim enumeration untuk mencari konten tersembunyi
