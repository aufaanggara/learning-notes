=== Offensive Security Introduction — Resume Materi ===
Room: Offensive Security Intro (TryHackMe)
Tanggal: 17 Agustus 2026

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. KONSEP INTI: OFFENSIVE SECURITY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Definisi
**Offensive security** adalah pendekatan keamanan siber di mana seseorang berpikir dan bertindak seperti penyerang dengan tujuan menemukan kelemahan pada sistem sebelum penyerang sungguhan menemukannya lebih dulu. Pendekatan ini bersifat proaktif — bukan menunggu serangan terjadi, melainkan secara aktif mencari celah.

Praktik dilakukan dalam lingkungan yang aman dan legal, artinya target yang diserang adalah sistem simulasi (seperti lab FakeBank pada room ini) atau sistem nyata yang sudah mendapat izin resmi dari pemiliknya. Tanpa izin, tindakan yang sama akan berubah status menjadi ilegal meskipun teknik yang dipakai identik.

Perbandingan dengan Defensive Security
**Offensive security** berfokus pada mencari dan mengeksploitasi celah (menyerang untuk menguji ketahanan), sedangkan **defensive security** berfokus pada mendeteksi, mencegah, dan merespons serangan (melindungi sistem yang sudah berjalan). Kedua pendekatan ini saling melengkapi: temuan dari sisi offensive menjadi dasar perbaikan di sisi defensive.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
2. METODOLOGI DASAR YANG DIPRAKTIKKAN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Room ini mempraktikkan dua tahap dasar dari sebuah serangan web sederhana, yaitu enumeration dan exploitation.

2.1 Enumeration (Content Discovery)
Tahap ini bertujuan memetakan sisi aplikasi yang tidak terlihat langsung oleh pengguna biasa, khususnya halaman-halaman yang tidak ditautkan di navigasi utama situs namun sebenarnya masih bisa diakses jika alamatnya diketahui. Kesalahan konfigurasi seperti ini disebut **hidden pages** dan menjadi salah satu penyebab kebocoran akses yang paling umum ditemukan di aplikasi web nyata.

Teknik yang dipakai untuk menemukan hidden pages disebut **directory brute force**: mencoba banyak kemungkinan nama direktori/file secara berurutan terhadap sebuah URL target, lalu mencatat mana saja yang merespons sebagai halaman valid.

2.2 Exploitation (Broken Access Control)
Setelah halaman tersembunyi ditemukan, tahap berikutnya adalah menguji apakah halaman tersebut benar-benar terlindungi. Pada studi kasus FakeBank, admin panel yang ditemukan ternyata dapat diakses langsung tanpa proses login atau autentikasi apa pun. Kelemahan jenis ini disebut **broken access control** — kondisi di mana suatu fungsi yang seharusnya dibatasi hanya untuk pengguna dengan hak akses tertentu (misalnya staff/admin) justru bisa diakses dan digunakan oleh siapa saja yang mengetahui URL-nya.

Dampak dari broken access control pada kasus ini bersifat langsung dan konkret: pengguna tanpa otorisasi dapat memanipulasi data finansial (menambah saldo rekening) hanya dengan mengetik URL tertentu dan mengisi form yang tersedia di halaman tersebut.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
3. STUDI KASUS: FAKEBANK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Alur Serangan
1. Target aplikasi: `http://fakebank.thm`, sebuah simulasi aplikasi perbankan.
2. Enumeration dijalankan terhadap domain target untuk menemukan halaman tersembunyi. Hasil scan menemukan dua URL, salah satunya adalah endpoint admin.
3. Endpoint admin diakses langsung melalui browser dengan menambahkan path `/bank-transfer` ke URL utama, tanpa proses login sama sekali.
4. Di dalam admin panel tersedia form untuk mendeposit uang ke nomor akun tertentu. Kelemahan dieksploitasi dengan memasukkan nomor akun milik sendiri dan mengisi nominal deposit, lalu menekan tombol submit.
5. Perubahan saldo pada akun membuktikan bahwa eksploitasi berhasil — ini adalah bentuk sederhana dari **proof of concept**, yaitu bukti nyata bahwa suatu kerentanan benar-benar bisa dimanfaatkan, bukan sekadar dugaan teoritis.

Inti Pelajaran dari Kasus Ini
Kelemahan yang dieksploitasi bukan berasal dari kerentanan teknis yang rumit (seperti bug pada kode), melainkan dari kesalahan desain akses: halaman sensitif yang seharusnya memerlukan autentikasi malah dibiarkan terbuka. Ini menegaskan bahwa proses enumeration — sekadar mencari halaman yang "tersembunyi" — sudah cukup untuk membuka jalan eksploitasi jika kontrol akses di baliknya tidak diterapkan dengan benar.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
4. TOOL & COMMAND YANG DIPAKAI
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Dirb
**Dirb** adalah tool untuk directory brute force: mencoba daftar nama direktori/file dari sebuah wordlist terhadap URL target, lalu melaporkan mana saja yang benar-benar ada di server.

```
dirb http://fakebank.thm
```

Penjelasan bagian command:
- `dirb` — memanggil tool dirb.
- `http://fakebank.thm` — argumen wajib berupa URL target yang akan di-brute force; dirb akan menggunakan wordlist bawaannya untuk mencoba berbagai kemungkinan path terhadap domain ini.

Cara membaca output: setiap baris hasil yang diawali tanda `+` menunjukkan sebuah path yang berhasil ditemukan (artinya server memberi respons valid, bukan halaman kosong/tidak ada). Baris tanpa tanda tersebut bukan hasil temuan dan bisa diabaikan.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
5. ISTILAH PENTING YANG WAJIB DIHAPAL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Offensive security** — pendekatan proaktif yang mencari kelemahan sistem dengan cara mensimulasikan serangan, dilakukan sebelum penyerang sungguhan menemukannya.

**Defensive security** — pendekatan yang berfokus pada mendeteksi, mencegah, dan merespons ancaman terhadap sistem yang sudah berjalan.

**Ethical hacking** — praktik hacking yang dilakukan secara legal, di lingkungan aman atau dengan izin resmi, untuk tujuan menemukan dan melaporkan kelemahan keamanan.

**Enumeration / content discovery** — proses aktif memetakan bagian-bagian sistem atau aplikasi yang belum diketahui, seperti halaman, direktori, atau service yang berjalan.

**Directory brute force** — teknik mencoba banyak kemungkinan nama direktori/file secara sistematis terhadap sebuah target untuk menemukan halaman yang tidak terlihat secara publik.

**Hidden pages** — halaman pada suatu aplikasi web yang tidak ditautkan di navigasi/menu utama, namun tetap dapat diakses apabila URL-nya diketahui.

**Broken access control** — kelemahan keamanan di mana kontrol pembatasan akses tidak diterapkan dengan benar, sehingga fungsi yang seharusnya terbatas (misalnya khusus admin) dapat diakses oleh pengguna yang tidak berwenang.

**Admin panel** — antarmuka pengelolaan sistem yang seharusnya hanya bisa diakses oleh administrator atau staf berwenang.

**Proof of concept** — bukti nyata dan terverifikasi bahwa suatu kerentanan memang bisa dieksploitasi, bukan sekadar asumsi teoritis.