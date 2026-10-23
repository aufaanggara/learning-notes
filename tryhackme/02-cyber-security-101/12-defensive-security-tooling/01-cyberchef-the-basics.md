# Resume: CyberChef Basics
**Tanggal:** 31 Agustus 2026

---

## 1. Pengantar CyberChef

**CyberChef** adalah aplikasi web sederhana dan intuitif untuk berbagai tugas operasi cyber security langsung di dalam browser. Sering disebut sebagai "Swiss Army knife" untuk data karena menyediakan banyak alat kecil (operations) yang masing-masing dirancang untuk tugas spesifik.

Cakupan tugas yang bisa ditangani sangat luas: mulai dari encoding sederhana seperti **XOR** atau **Base64**, sampai operasi kompleks seperti enkripsi **AES** atau dekripsi **RSA**.

CyberChef bekerja berdasarkan konsep **recipe** — serangkaian operasi yang dijalankan secara berurutan (satu operasi memproses output operasi sebelumnya, seperti jalur perakitan).

### 1.1 Cara Mengakses

Terdapat dua metode akses:

- **Online Access** — hanya butuh browser dan koneksi internet, langsung buka lewat link resmi CyberChef.
- **Offline/Local Copy** — mengunduh file rilis terbaru dan menjalankannya secara lokal di Windows maupun Linux. Direkomendasikan mengunduh versi paling stabil.

---

## 2. Struktur Antarmuka (Interface)

CyberChef terdiri dari empat area utama yang saling terhubung dalam satu alur kerja.

### 2.1 Operations Area

Repositori lengkap semua operasi yang tersedia, dikelompokkan berdasarkan kategori dan dapat dicari lewat kolom search. Mengarahkan kursor (hover) ke suatu operasi akan menampilkan deskripsi, contoh penggunaan, dan link Wikipedia terkait.

Contoh operasi umum:

- **From Morse Code** — menerjemahkan Kode Morse menjadi karakter alfanumerik huruf besar.
- **URL Encode** — meng-encode karakter bermasalah menjadi percent-encoding (format URI/URL).
- **To Base64** — meng-encode data mentah menjadi string ASCII Base64.
- **To Hex** — mengonversi string menjadi byte heksadesimal dipisah delimiter tertentu.
- **To Decimal** — mengonversi data menjadi array bilangan bulat ordinal (kode ASCII per karakter).
- **ROT13** — cipher substitusi Caesar yang menggeser huruf alfabet sejumlah nilai tertentu (default 13).

### 2.2 Recipe Area

Dianggap sebagai jantung tool ini — tempat memilih, menyusun, dan mengonfigurasi argumen/opsi tiap operasi. Operasi disusun dengan cara drag-and-drop, dan **urutan penyusunan menentukan hasil akhir** karena tiap operasi memproses output dari operasi di atasnya.

Fitur utama:

- **Save recipe** — menyimpan susunan operasi yang sudah dipilih.
- **Load recipe** — memuat kembali recipe yang sebelumnya disimpan.
- **Clear Recipe** — menghapus recipe yang sedang aktif.
- **BAKE!** — tombol untuk memproses data sesuai recipe yang disusun.
- **Auto Bake** — opsi untuk memproses data secara otomatis tanpa perlu menekan BAKE! manual setiap perubahan.

### 2.3 Input Area

Ruang untuk memasukkan data lewat mengetik, paste, atau drag file. Fitur tambahan:

- **Add a new input tab** — membuat tab input baru untuk menyimpan set data berbeda.
- **Open folder as input** — mengunggah satu folder penuh sebagai input.
- **Open file as input** — mengunggah satu file sebagai input.
- **Clear input and output** — menghapus semua nilai input beserta output terkait.
- **Reset pane layout** — mengembalikan tampilan interface ke ukuran default.

### 2.4 Output Area

Menampilkan hasil pemrosesan data secara rapi. Fitur tambahan:

- **Save output to file** — menyimpan hasil ke file `.dat`.
- **Copy raw output to the clipboard** — menyalin hasil mentah langsung ke clipboard.
- **Replace input with output** — mengganti data input dengan hasil operasi yang sudah dijalankan, memungkinkan pemrosesan bertingkat.
- **Maximise output pane** — mengembalikan tampilan interface ke ukuran default.

---

## 3. Alur Berpikir (Thought Process) Menggunakan CyberChef

Proses kerja sistematis saat menggunakan tool ini terdiri dari empat tahap berurutan:

1. **Set a clear objective** — menentukan tujuan spesifik dan bisa dicapai; menjawab pertanyaan "apa yang ingin dicapai?".
2. **Put your data into the input area** — memasukkan data mentah (misalnya string gibberish yang ditemukan) ke kolom Input.
3. **Select the Operations you might want to use** — memilih operasi berdasarkan riset/analisis awal terhadap karakteristik data (misalnya jika dicurigai berkaitan dengan enkripsi, coba operasi di kategori Encryption/Encoding).
4. **Check the output** — mengevaluasi apakah hasil sudah sesuai tujuan. Jika belum, ulangi proses dari tahap 1 atau tahap 3.

Alur ini bersifat **iteratif** — bukan proses linear sekali jalan, melainkan siklus coba-evaluasi-ulangi sampai tujuan tercapai.

---

## 4. Kategori Operasi Umum

### 4.1 Extractors

Operasi yang menyaring input dan hanya menampilkan bagian teks yang cocok dengan pola tertentu; sisanya dibuang.

- **Extract IP addresses** — mengekstrak semua alamat IPv4 dan IPv6 yang valid dari input.
- **Extract email addresses** — mengekstrak string berformat `anything@domain.com`.
- **Extract URLs** — mengekstrak URL dari input; protokol (HTTP, FTP, dll) wajib ada pada string sumber, jika tidak akan muncul terlalu banyak false positive.
- **Extract domains** — mengekstrak nama domain dari input teks.

### 4.2 Date and Time

- **From UNIX Timestamp** — mengonversi UNIX timestamp menjadi string datetime yang mudah dibaca.
- **To UNIX Timestamp** — mengurai string datetime (dalam UTC) menjadi UNIX timestamp yang sesuai.

**UNIX timestamp** adalah nilai 32-bit yang merepresentasikan jumlah detik sejak 1 Januari 1970 UTC (disebut UNIX epoch).

### 4.3 Data Format (Base Encodings)

Operasi encoding/decoding yang mengubah data biner menjadi representasi berbasis teks menggunakan set karakter ASCII tertentu, disebut **base encoding**.

- **From Base64 / To Base64** — mendekode/meng-encode data menggunakan notasi Base64.
- **From Base85 / To Base85** — notasi encoding yang biasanya lebih efisien dibanding Base64.
- **From Base58 / To Base58** — mirip Base64 namun menghilangkan karakter yang sering salah dibaca manusia (I, l, 0, O) untuk meningkatkan keterbacaan.
- **To Base62** — encoding dengan set simbol terbatas; basis angka tinggi menghasilkan string lebih pendek dibanding basis desimal/heksadesimal.
- **URL Decode** — mengonversi karakter percent-encoded kembali ke nilai aslinya. Character set default HTML5 adalah **UTF-8**.
- **URL Encode** — mengubah karakter bermasalah/spesial menjadi bentuk percent-encoded; opsi **"Encode all special chars"** perlu diaktifkan agar seluruh karakter spesial (termasuk `:` dan `/`) ikut ter-encode, bukan hanya karakter yang secara default dianggap problematic.

**Konvensi penamaan penting:** nama operasi ("From ..." / "To ...") menunjukkan **arah perjalanan data**, bukan arah kata di soal.

- "From [format]" = decode, dari format tersebut menuju bentuk asli/teks biasa.
- "To [format]" = encode, dari teks biasa menuju format tersebut.

Contoh referensi percent-encoding karakter umum di URL (UTF-8):

| Karakter | Percent-Encoding |
|---|---|
| `:` | `%3A` |
| `/` | `%2F` |
| `.` | `%2E` |
| `=` | `%3D` |
| `#` | `%23` |

---

## 5. Mekanisme Konversi Manual Base64

Memahami cara kerja Base64 di balik layar membantu memahami mengapa operasi otomatis CyberChef bisa menghasilkan output tertentu. Prosesnya:

1. **Convert to Binary and Merge** — setiap karakter dikonversi ke representasi biner 8-bit berdasarkan nilai desimal ASCII-nya, lalu digabung (concatenate) menjadi satu string biner panjang.
2. **Divide and Convert to Decimal** — string biner gabungan dipecah menjadi kelompok 6-bit, lalu tiap kelompok dikonversi ke desimal.
3. **Convert to Base64** — tiap nilai desimal dicocokkan dengan tabel indeks Base64 (0-25 = A-Z, 26-51 = a-z, 52-61 = 0-9, 62 = `+`, 63 = `/`) untuk mendapatkan karakter Base64 akhir.

Contoh: huruf "THM" → biner 24-bit → 4 kelompok 6-bit → nilai desimal 21, 4, 33, 13 → karakter Base64 `V`, `E`, `h`, `N` → hasil akhir `VEhN`.

---

## 6. Prinsip Praktis Penggunaan Recipe

- Urutan operasi dalam Recipe **wajib sesuai arah konversi yang dibutuhkan** — memasang dua operasi yang secara fungsi sama (misalnya "From Decimal" lalu "To Decimal") tidak akan menghasilkan transformasi yang diinginkan.
- Untuk mengubah **Decimal → Binary**, operasi yang tepat adalah "From Decimal" (membaca input sebagai angka desimal) diikuti "To Binary" (mengubah hasilnya menjadi representasi biner) — bukan "To Decimal" di kedua tahap.
- Ketelitian terhadap detail karakter pada Input sangat penting dalam operasi encoding — kekurangan satu karakter (misalnya tanda seru `!`) akan menghasilkan output Base64/encoding yang sepenuhnya berbeda dari yang diharapkan.
- Untuk pemrosesan data berskala besar, CyberChef sebaiknya dikombinasikan dengan dukungan tool lain — bukan pengganti penuh semua tool analisis data.

---

## 7. Glossary

- **Recipe** — susunan berurutan dari satu atau lebih operasi yang diterapkan pada data input.
- **Bake** — proses menjalankan/mengeksekusi recipe terhadap data input untuk menghasilkan output.
- **Operation** — satu unit fungsi/alat spesifik di CyberChef (contoh: To Base64, ROT13, Extract IP addresses).
- **Base encoding** — teknik merepresentasikan data biner sebagai teks menggunakan set karakter ASCII tertentu (contoh: Base64, Base58, Base62, Base85).
- **Percent-encoding (URL Encoding)** — format encoding untuk karakter yang tidak valid/bermasalah dalam URI/URL, direpresentasikan sebagai `%` diikuti dua digit heksadesimal.
- **UNIX Timestamp** — nilai numerik 32-bit yang menyatakan jumlah detik sejak UNIX epoch (1 Januari 1970 UTC).
- **False positive** — hasil ekstraksi/deteksi yang keliru dianggap cocok padahal bukan target sebenarnya (contoh: ekstraksi URL tanpa protokol eksplisit).
- **DFIR** — Digital Forensics and Incident Response, salah satu bidang penerapan CyberChef yang dicontohkan dalam materi.

---

## 8. Tools & Platform Rujukan

- **CyberChef** — tool utama yang dibahas di room ini; aplikasi web untuk transformasi, encoding/decoding, dan analisis data. Tersedia versi online maupun offline (link resmi ditampilkan langsung di dalam materi room, tidak disebutkan sebagai URL teks terpisah).
- **Networking Concepts (room TryHackMe)** — direkomendasikan sebagai bahan penyegaran seputar konsep jaringan, relevan untuk memahami operasi Extract IP addresses.
- **Web Applications Basics (room TryHackMe)** — direkomendasikan untuk mendalami konsep URL dan aplikasi web, relevan untuk memahami operasi Extract URLs dan URL Encode/Decode.
- **Hashing Basics (room TryHackMe)** — room terpisah yang membahas hashing secara lebih mendalam; disebut sebagai prasyarat opsional sebelum room ini.
- **Cryptography Basics (room TryHackMe)** — room terpisah yang membahas kriptografi secara lebih mendalam; disebut sebagai prasyarat opsional sebelum room ini.

---

## 9. Catatan Ringkas untuk Ditulis Tangan

**Konsep Dasar**
- CyberChef — web app, "Swiss Army knife" data, kerja pakai Recipe
- Recipe — urutan operations, urutan berpengaruh ke hasil
- Bake — proses/eksekusi recipe

**4 Area Interface**
- Operations — gudang semua alat, ada search & hover-info
- Recipe — susun & atur operasi, tombol BAKE!, Auto Bake
- Input — tempat masukin data (ketik/paste/upload file/folder)
- Output — hasil akhir; ada Save to file, Copy, Replace input with output

**Alur Berpikir (4 Step)**
1. Set objective — tujuan jelas
2. Input data
3. Pilih Operations — riset dulu kalau belum familiar
4. Cek output — sesuai? selesai. belum? ulangi step 1/3

**Extractors**
- Extract IP addresses — IPv4/IPv6
- Extract email addresses — format anything@domain.com
- Extract URLs — wajib ada protokol, else false positive
- Extract domains — nama domain

**Date/Time**
- From UNIX Timestamp — angka → datetime
- To UNIX Timestamp — datetime UTC → angka
- UNIX epoch — 1 Jan 1970 UTC

**Data Format / Base Encoding**
- From X — decode (X → teks asli)
- To X — encode (teks asli → X)
- Base64, Base58, Base85, Base62 — semua base encoding, beda alfabet/efisiensi
- Base58 — hilangkan I, l, 0, O (anti salah baca)
- URL Decode — %XX → karakter asli, default charset UTF-8
- URL Encode — centang "Encode all special chars" biar semua simbol ke-encode (termasuk : dan /)

**Konversi Manual Base64**
1. Karakter → biner 8-bit → gabung
2. Pecah 6-bit → ke desimal
3. Cocokkan ke tabel index Base64 (0-25=A-Z, 26-51=a-z, 52-61=0-9, 62=+, 63=/)

**Tabel Percent-Encoding Cepat**
- `:` = %3A
- `/` = %2F
- `.` = %2E
- `=` = %3D
- `#` = %23

**Perangkap Umum**
- Jangan salah pasang From/To — cek arah konversi yang diminta
- Teliti karakter di Input (spasi, tanda baca) — beda 1 karakter = beda total hasil encode
- "Decoded" di soal = pakai "From ..."; "Encoded" di soal = pakai "To ..."

**Referensi**
- Networking Concepts, Web Applications Basics, Hashing Basics, Cryptography Basics (semua room TryHackMe pendukung)
