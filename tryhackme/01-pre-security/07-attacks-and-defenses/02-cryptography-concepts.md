# Resume Materi — Cryptography
**TryHackMe | Pre-Security Path**
**Tanggal: 08 September 2026**

---

## 1. Pengantar Kriptografi

Kriptografi adalah ilmu mengamankan komunikasi menggunakan aturan matematika dan kunci rahasia, sehingga hanya pihak yang berwenang yang bisa membaca atau memahami informasi tersebut.

Relevansinya langsung terhubung ke **CIA Triad**: kriptografi terutama melindungi **Confidentiality** (mencegah data dibaca pihak yang tidak berwenang) dan **Integrity** (mencegah data dimanipulasi selama pengiriman). Tanpa kriptografi, data yang melewati lusinan router dan komputer di internet bisa dibaca, diubah, atau diblokir oleh siapapun yang punya akses ke jalur tersebut.

Skenario nyata: sebuah klinik medis yang mengirim rekam medis pasien lewat internet. Data tersebut tidak berpindah langsung — ia melewati banyak node perantara. Kriptografi mengubah data itu menjadi sesuatu yang tidak bermakna bagi siapapun kecuali penerima yang sah.


## 2. Konsep dan Istilah Dasar

### 2.1 Empat Istilah Fondasi

**Plaintext** adalah data asli dalam bentuk yang bisa dibaca secara normal. Contoh: `HELLO` atau `Patient name: Alice Smith`. Ini adalah data sebelum disentuh proses enkripsi apapun.

**Ciphertext** adalah versi plaintext yang sudah diacak sehingga tidak bermakna bagi siapapun yang tidak punya kunci. Contoh: `KHOOR` atau `Sdwlhqw qdph: Dolfh Vplwk`. Ciphertext yang baik harus terlihat seperti noise acak tanpa pola yang mudah dikenali.

**Kunci (Key)** adalah informasi rahasia yang mengontrol cara enkripsi dan dekripsi bekerja. Analoginya seperti password yang digunakan oleh algoritma — tanpa kunci yang tepat, ciphertext tidak bisa dikembalikan menjadi plaintext yang bermakna.

**Algoritma** adalah metode atau langkah-langkah yang menjelaskan cara menggunakan kunci pada pesan. Algoritma bersifat publik dan boleh diketahui siapapun. **Prinsip terpenting:** keamanan sebuah sistem kriptografi tidak berasal dari kerahasiaan algoritmanya, melainkan dari kerahasiaan kuncinya.

### 2.2 Formula Dasar

Enkripsi: `plaintext + algoritma + key → ciphertext`

Dekripsi: `ciphertext + algoritma + key → plaintext`


## 3. Enkripsi Simetris

### 3.1 Definisi dan Cara Kerja

Enkripsi simetris menggunakan **satu kunci yang sama** untuk proses enkripsi maupun dekripsi. Pengirim dan penerima harus sama-sama memiliki salinan kunci tersebut, dan kunci itu harus dirahasiakan dari semua pihak lain.

Analogi yang digunakan adalah **kotak kunci (lockbox)**:
- Algoritma = cara kerja kunci (publik, semua orang bisa lihat)
- Kunci = kunci logam spesifik (hanya pemilik yang punya)
- Plaintext = surat di dalam kotak
- Ciphertext = kotak terkunci yang dikirim lewat pos

Alice mengunci kotak dengan kuncinya, mengirimkannya. Bob yang punya salinan kunci yang sama membukanya. Siapapun yang mencegat kotak di tengah jalan hanya melihat kotak terkunci yang tidak berguna tanpa kunci.

### 3.2 Caesar Cipher — Contoh Implementasi

Caesar Cipher adalah contoh paling sederhana dari enkripsi simetris. Setiap huruf digeser sejumlah posisi tetap dalam alfabet, dan angka geseran itu adalah kuncinya.

Dengan **kunci = 3**:
- A → D, B → E, C → F, ..., X → A, Y → B, Z → C

Enkripsi `HELLO` dengan kunci 3:
```
H → K
E → H
L → O
L → O
O → R
HELLO → KHOOR
```

Dekripsi `KHOOR` dengan kunci 3 (geser mundur):
```
K → H
H → E
O → L
O → L
R → O
KHOOR → HELLO
```

Dalam Caesar Cipher: algoritmanya (geser huruf) publik, kuncinya (angka 3) rahasia. Ini mengilustrasikan prinsip bahwa keamanan ada di kunci, bukan di algoritmanya.

**Catatan penting:** Caesar Cipher sama sekali tidak aman untuk penggunaan nyata. Hanya ada 25 kemungkinan kunci, dan komputer bisa mencoba semuanya dalam sekitar satu milidetik. Caesar Cipher dipakai di sini murni sebagai alat ilustrasi, bukan referensi implementasi.

Algoritma nyata yang digunakan industri: **AES (Advanced Encryption Standard)** — jauh lebih kompleks tapi mengikuti ide dasar yang sama.

### 3.3 Keunggulan dan Kelemahan

Keunggulan enkripsi simetris:
- **Cepat** — bisa memproses data dalam jumlah besar dengan sangat efisien
- **Efisien** — cocok untuk enkripsi file besar, hard drive, dan network traffic

Kelemahan utama — **Key Distribution Problem:**

Bagaimana Alice dan Bob berbagi kunci itu dengan aman sebelum komunikasi dimulai? Jika kunci dikirim lewat internet secara terbuka, penyadap bisa mengambilnya. Jika kunci dienkripsi dulu, dibutuhkan kunci lain untuk enkripsi kunci itu — dan kunci lain lagi untuk yang itu — mundur tanpa batas (*infinite regress*).

Ini adalah titik lemah fundamental enkripsi simetris ketika digunakan sendiri, dan inilah yang mendorong lahirnya enkripsi asimetris.


## 4. Enkripsi Asimetris

### 4.1 Dua Kunci yang Berbeda

Enkripsi asimetris menjawab Key Distribution Problem dengan menggunakan **dua kunci yang terhubung secara matematis** namun berbeda fungsi:

- **Public key** — boleh dibagikan ke siapapun, tidak perlu dirahasiakan
- **Private key** — hanya dipegang satu orang, tidak boleh keluar sama sekali

Aturan kerjanya:
- Data yang dienkripsi dengan **public key** seseorang hanya bisa didekripsi oleh **private key** orang tersebut
- Data yang dienkripsi dengan **private key** bisa didekripsi oleh siapapun yang punya **public key** yang bersesuaian (mekanisme ini digunakan untuk tanda tangan digital)

Kedua kunci terhubung secara matematis sedemikian rupa sehingga komputer biasa butuh ratusan hingga ribuan tahun untuk mendapatkan private key dari public key. Kesulitan komputasi inilah yang menjadi fondasi keamanannya.

### 4.2 Analogi Kotak Surat (Mailbox)

Bayangkan kotak surat fisik di sudut jalan:
- **Celah surat** = public key → siapapun bisa memasukkan surat, terbuka dan bisa diakses
- **Pintu terkunci** = private key → hanya pemilik kotak yang bisa membuka dan mengambil isi

Ketika Alice ingin mengirim pesan rahasia ke Bob:
1. Alice ambil public key Bob (bebas, bisa dari website Bob)
2. Alice enkripsi pesan menggunakan public key Bob
3. Alice kirim ciphertext
4. Hanya Bob yang bisa dekripsi menggunakan private key-nya

Bahkan jika penyerang mencegat ciphertext, tanpa private key Bob, pesan tidak bisa dibaca.

### 4.3 Alur Memecahkan Key Distribution Problem

1. Bob buat public key dan private key di komputernya
2. Bob simpan private key, bagikan public key ke publik (website, key server, dll.)
3. Alice ambil public key Bob
4. Alice enkripsi pesan dengan public key Bob, kirim
5. Bob dekripsi dengan private key-nya

Tidak ada momen di mana kunci rahasia berpindah lewat jaringan. Satu-satunya yang berpindah secara publik adalah public key Bob, yang memang dirancang untuk tidak rahasia — itulah solusinya.

### 4.4 Keunggulan dan Kelemahan

Keunggulan:
- Memecahkan Key Distribution Problem
- Tidak perlu bertukar rahasia sebelum komunikasi dimulai

Kelemahan:
- Lebih lambat dari enkripsi simetris
- Tidak efisien untuk data dalam jumlah besar


## 5. Penggunaan Nyata: HTTPS dan Hybrid Approach

### 5.1 Apa yang Terjadi Saat Membuka HTTPS

Ikon gembok di browser menandakan koneksi HTTPS aktif. Ini yang sebenarnya terjadi ketika kamu membuka `https://google.com`:

1. Browser meminta public key dari server Google
2. Server mengirim public key-nya yang dibungkus dalam **sertifikat digital**
3. Browser dan server menggunakan enkripsi asimetris untuk menyepakati **shared secret** (kunci simetris bersama) tanpa ada pihak ketiga yang bisa melihatnya
4. Setelah kunci simetris disepakati, seluruh komunikasi selanjutnya menggunakan **enkripsi simetris** yang lebih cepat

### 5.2 Hybrid Approach

Kombinasi ini disebut **hybrid approach** dan digunakan oleh semua sistem komunikasi aman modern:

| Enkripsi | Peran dalam Hybrid |
|---|---|
| Asimetris | Menyelesaikan key distribution di awal koneksi |
| Simetris | Menangani semua data selama sesi karena lebih cepat |

Ini adalah cara HTTPS, VPN, dan aplikasi pesan terenkripsi (seperti WhatsApp) semuanya beroperasi.


## 6. Sertifikat Digital dan Certificate Authority

### 6.1 Masalah yang Diselesaikan

Enkripsi asimetris menimbulkan pertanyaan baru: bagaimana browsermu tahu bahwa public key yang diterima benar-benar milik Google, bukan milik penyerang yang berpura-pura menjadi Google? Jawaban: **sertifikat digital**.

### 6.2 Isi Sertifikat Digital

Sertifikat digital adalah dokumen yang berisi tiga hal:
- Public key pemiliknya
- Identitas pemilik kunci (misalnya `google.com`)
- Tanda tangan digital dari pihak ketiga terpercaya yang disebut **Certificate Authority (CA)**

### 6.3 Cara Kerja CA

**Certificate Authority (CA)** adalah otoritas terpercaya yang bertugas memverifikasi identitas pemilik domain dan menandatangani sertifikat mereka. Browser dan sistem operasi sudah dilengkapi sejak instalasi dengan daftar CA yang dipercaya (trusted CA list).

Ketika browser menerima sertifikat dari sebuah website:
1. Browser cek apakah sertifikat ditandatangani oleh CA yang terpercaya
2. Browser cek apakah sertifikat masih valid (belum expired atau dicabut/revoked)
3. Jika keduanya lolos → browser tampilkan ikon gembok, koneksi dipercaya
4. Jika ada yang gagal → browser tampilkan peringatan dan bisa menolak koneksi

### 6.4 Cara Melihat Sertifikat di Browser

1. Kunjungi situs HTTPS manapun
2. Klik ikon gembok di address bar
3. Pilih "Connection is secure" atau "View certificate"
4. Jendela sertifikat menampilkan:
   - **Issued To** — domain website
   - **Issued By** — CA yang menandatangani
   - **Valid From / Valid Until** — masa berlaku sertifikat

Contoh dari `tryhackme.com`: diterbitkan oleh Google Trust Services, berlaku dari November 2025 sampai Februari 2026.


## 7. Perbandingan Simetris vs Asimetris

| Fitur | Simetris | Asimetris |
|---|---|---|
| Jumlah kunci | 1 (sama untuk enkripsi & dekripsi) | 2 (public + private) |
| Berbagi kunci | Kedua pihak harus punya kunci rahasia yang sama | Public key bebas dibagikan |
| Kecepatan | Sangat cepat | Lebih lambat |
| Kegunaan utama | Data massal — file, disk, network traffic | Key exchange & sertifikat digital |
| Kelemahan | Key Distribution Problem | Lambat untuk data besar |
| Analogi | Satu kunci buka dan kunci kotak | Kotak surat: siapapun kirim, hanya pemilik ambil |

Dalam praktik nyata, sistem selalu menggunakan keduanya: asimetris untuk memulai koneksi dan menyepakati kunci, simetris untuk menangani semua data selanjutnya.


## 8. Konteks Lebih Luas: Kriptografi dalam Keamanan Siber

Kriptografi melindungi dua pilar CIA Triad:
- **Confidentiality** — data tidak bisa dibaca pihak yang tidak berwenang meski dicegat
- **Integrity** — perubahan pada data terenkripsi akan terdeteksi

Namun kriptografi bukan solusi tunggal. Ia hanya satu lapisan dalam sistem keamanan yang lebih besar. Layer lain yang sama pentingnya:
- Praktik password yang kuat — password lemah membuat enkripsi apapun tidak berarti
- Penyimpanan kunci yang aman — kunci yang bocor sama dengan enkripsi yang gagal
- Kesadaran dan pelatihan pengguna — manusia sering menjadi titik lemah terbesar
- Pembaruan software secara rutin — kerentanan di software membuka jalan masuk tanpa harus membobol enkripsi
- Pemantauan dan respons insiden — mendeteksi dan merespons serangan yang sudah terjadi


## 9. Glossary

**Plaintext** — data asli dalam bentuk yang bisa dibaca sebelum dienkripsi.

**Ciphertext** — data yang sudah dienkripsi; terlihat seperti noise acak bagi yang tidak punya kunci.

**Enkripsi** — proses mengubah plaintext menjadi ciphertext menggunakan algoritma dan kunci.

**Dekripsi** — proses mengubah ciphertext kembali menjadi plaintext menggunakan algoritma dan kunci.

**Kunci (Key)** — informasi rahasia yang mengontrol cara enkripsi dan dekripsi bekerja.

**Algoritma** — metode/langkah publik yang mendefinisikan cara menggunakan kunci pada data.

**Enkripsi Simetris** — skema enkripsi yang menggunakan satu kunci yang sama untuk enkripsi dan dekripsi.

**Enkripsi Asimetris** — skema enkripsi yang menggunakan dua kunci berbeda: public key (untuk enkripsi) dan private key (untuk dekripsi).

**Public Key** — kunci yang boleh dibagikan ke siapapun; digunakan untuk mengenkripsi data yang ditujukan kepada pemilik private key-nya.

**Private Key** — kunci rahasia yang hanya dipegang satu orang; satu-satunya yang bisa mendekripsi data yang dienkripsi dengan public key bersesuaian.

**Key Distribution Problem** — masalah fundamental enkripsi simetris: bagaimana dua pihak bisa berbagi kunci rahasia secara aman sebelum komunikasi terenkripsi dimulai.

**Caesar Cipher** — contoh enkripsi simetris sederhana di mana setiap huruf digeser sejumlah posisi tetap dalam alfabet. Tidak aman untuk penggunaan nyata; dipakai sebagai ilustrasi konsep.

**AES (Advanced Encryption Standard)** — algoritma enkripsi simetris yang digunakan di sistem nyata; jauh lebih kompleks dan aman dari Caesar Cipher.

**HTTPS** — protokol web aman yang menggunakan hybrid approach (asimetris + simetris) untuk mengamankan komunikasi browser-server; ditandai dengan ikon gembok di browser.

**Hybrid Approach** — kombinasi enkripsi asimetris (untuk key exchange di awal) dan enkripsi simetris (untuk data selama sesi) yang digunakan sistem komunikasi aman modern.

**Sertifikat Digital** — dokumen digital berisi public key, identitas pemilik, dan tanda tangan CA yang membuktikan keaslian identitas sebuah server.

**Certificate Authority (CA)** — otoritas terpercaya yang memverifikasi identitas pemilik domain dan menandatangani sertifikat digital mereka.

**Trusted CA List** — daftar CA yang sudah dipercaya oleh browser/OS sejak instalasi; digunakan sebagai dasar validasi sertifikat.

**Confidentiality** — pilar CIA Triad yang memastikan data hanya bisa diakses oleh pihak yang berwenang.

**Integrity** — pilar CIA Triad yang memastikan data tidak dimanipulasi selama penyimpanan atau pengiriman.


## 10. Tools & Platform Rujukan

**Caesar Cipher Game ("Secret Message Rescue")** — game interaktif bawaan room TryHackMe untuk mempraktikkan enkripsi dan dekripsi Caesar Cipher secara langsung. Diakses via tombol "View Site" di dalam room.


---


## 11. Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar

- Kriptografi — amankan komunikasi pakai matematika & kunci rahasia
- Tujuan — lindungi Confidentiality & Integrity (2 dari 3 CIA Triad)
- Plaintext — data asli, bisa dibaca siapapun
- Ciphertext — data teracak, tidak bermakna tanpa kunci
- Key — rahasia yang kontrol enkripsi/dekripsi
- Algoritma — metode publik cara pakai kunci (boleh diketahui umum)
- Prinsip utama — keamanan dari kerahasiaan KUNCI, bukan algoritma

### Formula

- Enkripsi: plaintext + algoritma + key → ciphertext
- Dekripsi: ciphertext + algoritma + key → plaintext

### Enkripsi Simetris

- 1 kunci yang sama untuk enkripsi & dekripsi
- Cepat & efisien untuk data besar
- Analogi: kotak kunci (lockbox) — 1 kunci buka & kunci
- Kelemahan: Key Distribution Problem (bagaimana berbagi kunci dengan aman?)
- Algoritma nyata: AES

### Caesar Cipher (Contoh Simetris)

- Geser tiap huruf sejumlah posisi = kunci
- Key 3: A→D, B→E, ..., X→A, Y→B, Z→C
- HELLO (key 3) → KHOOR
- Dekripsi: geser mundur
- TIDAK AMAN — hanya 25 kemungkinan, pecah dalam 1 milidetik

### Enkripsi Asimetris

- 2 kunci: public key (bebas dibagi) + private key (rahasia mutlak)
- Enkripsi pakai public key → hanya private key yang bisa dekripsi
- Solusi untuk Key Distribution Problem
- Lebih lambat → hanya untuk key exchange & sertifikat
- Analogi: kotak surat — celah surat = public key, pintu = private key

### Alur Alice → Bob (Asimetris)

1. Bob buat public+private key, bagikan public key
2. Alice enkripsi pakai public key Bob
3. Bob dekripsi pakai private key-nya
4. Tidak ada kunci rahasia yang berpindah lewat jaringan

### HTTPS & Hybrid Approach

- Hybrid = asimetris (awal/key exchange) + simetris (data sesi)
- HTTPS flow: browser minta public key → terima sertifikat → sepakati shared secret (asimetris) → lanjut pakai simetris
- Dipakai: HTTPS, VPN, app chat terenkripsi

### Sertifikat Digital & CA

- Sertifikat berisi: public key + identitas pemilik + tanda tangan CA
- CA (Certificate Authority) — otoritas terpercaya yang verifikasi & tanda tangani sertifikat
- Browser cek: tanda tangan CA valid? → sertifikat belum expired? → tampilkan gembok
- Gagal → browser tampilkan WARNING

### Perbandingan

- Simetris: 1 kunci, cepat, data massal, lemah di key distribution
- Asimetris: 2 kunci, lambat, key exchange, selesaikan key distribution
- Nyata: pakai keduanya (hybrid)
