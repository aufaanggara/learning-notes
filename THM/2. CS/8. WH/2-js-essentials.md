```
=== JavaScript Essentials - Resume Materi ===
[25 Mei 2026]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 APA ITU JAVASCRIPT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Definisi  : Bahasa scripting populer untuk menambah interaktivitas
            ke website yang sudah punya HTML & CSS
Sifat     : Interpreted → kode langsung dieksekusi browser,
            TANPA kompilasi terlebih dahulu
Digunakan : Terutama bersama HTML (validasi, animasi, onClick, dll)
Sudut     : Dipelajari dari perspektif SIBER → hacker manfaatkan
Pandang     fungsionalitas JS yang SAH untuk tujuan jahat
Analogi   : HTML = struktur rumah, CSS = cat/dekorasi,
            JS = sistem listrik & pipa (yang membuat segalanya hidup)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 VARIABEL & TIPE DATA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Variabel  : Wadah untuk menyimpan data, diberi nama agar bisa
            dipanggil kembali

3 CARA DEKLARASI :
  var   → Function-scoped (bisa diakses seluruh fungsi)
  let   → Block-scoped (hanya dalam blok { } tempat ia dideklarasi)
  const → Block-scoped + TIDAK BISA diubah setelah diisi

TIPE DATA :
  string    = teks           contoh: "Hello"
  number    = angka          contoh: 25
  boolean   = true / false
  null      = kosong (sengaja)
  undefined = belum diisi
  object    = data kompleks (array, objek)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 FUNGSI (FUNCTIONS)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Definisi  : Blok kode yang dirancang untuk menjalankan tugas tertentu,
            bisa dipanggil berkali-kali tanpa tulis ulang
Sintaks   :
  function namafungsi(parameter) {
      // isi kode
  }
  namafungsi(argumen);   ← cara memanggil

Contoh    :
  function PrintResult(rollNum) {
      alert("Roll number " + rollNum + " has passed");
  }
  PrintResult(101);

Analogi   : Seperti resep masak — tulis sekali, pakai berkali-kali

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 LOOPS (PERULANGAN)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Fungsi    : Menjalankan blok kode BERULANG selama kondisi = true

3 JENIS LOOP :
  for        → Paling umum, iterasi dengan counter
  while      → Selama kondisi true, terus jalan
  do...while → Minimal jalan 1x, baru cek kondisi

Sintaks for :
  for (let i = 0; i < 100; i++) {
      PrintResult(rollNumbers[i]);
  }
  → i mulai dari 0, jalan selama i < 100, i naik 1 tiap putaran

Analogi   : Mesin fotokopi — set jumlah, tekan tombol,
            mesin ulang sendiri sampai selesai

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 CONTROL FLOW (IF-ELSE)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Definisi  : Mengatur URUTAN eksekusi kode berdasarkan kondisi

Sintaks   :
  if (kondisi) {
      // jalankan jika kondisi TRUE
  } else {
      // jalankan jika kondisi FALSE
  }

Contoh    :
  if (age >= 18) {
      document.getElementById("message").innerHTML = "You are an adult.";
  } else {
      document.getElementById("message").innerHTML = "You are a minor.";
  }

Struktur lain :
  switch → untuk banyak kondisi sekaligus
  switch (nilai) {
      case "A": // kode jika nilai = A; break;
      case "B": // kode jika nilai = B; break;
      default:  // kode jika tidak ada yang cocok
  }

Analogi   : Seperti penjaga pintu — cek syarat dulu,
            baru putuskan boleh masuk atau tidak

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 DIALOGUE FUNCTIONS (FUNGSI DIALOG)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
3 FUNGSI BUILT-IN JS :

  alert("pesan")
  → Tampilkan pesan + tombol OK
  → Dipakai untuk notifikasi/peringatan
  → Disalahgunakan : spam alert ratusan kali (DoS pengalaman)

  prompt("pertanyaan")
  → Tampilkan kotak input untuk user
  → Return : nilai yang diketik user (OK) atau null (Cancel)
  → Disalahgunakan : kumpulkan data sensitif dari user yang tertipu

  confirm("pertanyaan")
  → Tampilkan pesan + tombol OK & Cancel
  → Return : true (OK) atau false (Cancel)
  → Disalahgunakan : manipulasi alur program berdasarkan jawaban

BAHAYA    : Jika tidak diimplementasikan dengan aman →
            rentan Cross-Site Scripting (XSS)

Contoh exploit :
  for (let i = 0; i < 500; i++) {
      alert("Hacked");           ← paksa user tutup 500x alert
  }

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 INTEGRASI JS DALAM HTML
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
2 CARA :

  INTERNAL JS
  → Kode JS ditulis LANGSUNG di dalam file HTML
  → Ditempatkan di antara tag <script> </script>
  → Bisa di <head> (load sebelum konten) atau
    di <body> (interaksi saat elemen dimuat)
  → Cocok untuk : pemula, script kecil

  EXTERNAL JS
  → Kode JS disimpan di FILE TERPISAH (.js)
  → Dihubungkan ke HTML dengan atribut src :
    <script src="script.js"></script>
  → File .js bisa di server yang sama atau server lain (CDN)
  → Cocok untuk : proyek besar, kode lebih rapi & mudah maintain

CARA CEK (perspektif pentester) :
  Klik kanan → View Page Source → cari tag <script>
  Tanpa src   = Internal JS
  Ada src     = External JS (perhatikan URL-nya!)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 MINIFIKASI & OBFUSKASI
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
MINIFIKASI
  Apa      : Kompres JS dengan hapus spasi, baris baru,
             komentar, dan persingkat nama variabel
  Tujuan   : Kurangi ukuran file → website lebih cepat load
  Hasil    : Kode tetap FUNGSIONAL tapi tidak readable

OBFUSKASI
  Apa      : Buat JS lebih sulit dipahami dengan cara :
             - Tambah kode tidak berguna (dummy code)
             - Ganti nama variabel/fungsi jadi tidak bermakna
             - Masukkan karakter acak
  Tujuan   : Lindungi logika kode dari pencurian/reverse engineering
  Fakta    : BISA DI-DEOBFUSCATE, tapi butuh usaha lebih

DEOBFUSKASI
  Apa      : Kebalikan obfuskasi → kembalikan kode ke bentuk
             yang bisa dibaca manusia
  Cara     : Pakai tool online (misal: obfuscator.io deobfuscator)
  Catatan  : Nama variabel asli tidak bisa kembali sempurna

PERBANDINGAN :
  Kode asli    : 58 Bytes   → mudah dibaca
  Kode obfusc  : 1.67 KB    → tidak bisa dibaca manusia,
                               tapi browser tetap bisa eksekusi

Analogi   : Minifikasi = ringkas buku jadi 1 hal tanpa spasi
            Obfuskasi  = tulis buku dalam kode rahasia yang
                         sengaja membingungkan

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 REQUEST-RESPONSE CYCLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Definisi  : Browser (client) kirim request ke web server →
            server balas dengan konten yang diminta
            (halaman web, data, atau resource lain)
Relevansi : JS dieksekusi di sisi CLIENT (browser) →
            mudah dilihat & dimanipulasi oleh user

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 BEST PRACTICES (WAJIB DIINGAT)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. JANGAN hanya andalkan client-side validation
   → User bisa disable/manipulasi JS di browser
   → Selalu validasi JUGA di sisi SERVER

2. JANGAN sembarangan include library dari internet
   → Pelaku jahat buat library palsu dengan nama mirip
   → Blindly include = buka pintu belakang sendiri
   → Selalu verifikasi sumber, pakai CDN resmi

3. JANGAN hardcode secrets di JS
   → API keys, access tokens, password = JANGAN taruh di JS
   → Source code JS bisa dilihat siapapun lewat browser
   → Contoh BURUK : const privateAPIKey = 'pk_TryHackMe-1337';

4. SELALU minify & obfuscate di production
   → Kurangi ukuran file + persulit reverse engineering
   → Penyerang tetap bisa deobfuscate, tapi butuh usaha & waktu

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 ISTILAH PENTING YANG WAJIB HAPAL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Interpreted      = kode langsung dijalankan tanpa kompilasi
Function-scoped  = variabel bisa diakses di seluruh fungsi
Block-scoped     = variabel hanya bisa diakses dalam blok { }
DOM              = Document Object Model, representasi HTML
                   yang bisa dimanipulasi JS
innerHTML        = properti untuk ubah/baca konten elemen HTML
getElementById   = fungsi untuk pilih elemen HTML berdasarkan id
src (atribut)    = atribut untuk tautkan file JS eksternal
XSS              = Cross-Site Scripting, serangan injeksi JS
                   berbahaya ke halaman web
Client-side      = dieksekusi di browser pengguna
Server-side      = dieksekusi di server, tidak terlihat user
Hardcode         = nilai ditulis langsung di kode (berbahaya
                   untuk data sensitif)
Attack Surface   = semua titik/celah yang bisa diserang
Minifikasi       = kompres kode → lebih kecil & cepat
Obfuskasi        = samarkan kode → lebih sulit dibaca
Deobfuskasi      = kembalikan kode obfuskasi ke bentuk readable
Supply Chain     = serangan via library/dependency pihak ketiga
```