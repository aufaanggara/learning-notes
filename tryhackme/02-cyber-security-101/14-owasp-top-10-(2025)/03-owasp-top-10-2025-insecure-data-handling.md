# Resume: OWASP Top 10 (2025) — Cryptographic Failures, Injection, dan Software/Data Integrity Failures

**Room:** TryHackMe — OWASP Top 10 (2025) Showcase
**Tanggal resume:** 31 Agustus 2026

---

## 1. Pendahuluan

Room ini membahas 3 elemen dari OWASP Top 10 (2025) yang berkaitan dengan perilaku aplikasi dan input pengguna:

- A04: Cryptographic Failures
- A05: Injection
- A08: Software or Data Integrity Failures

Setiap elemen dibahas dengan pola yang sama: definisi kerentanan, cara pencegahan, lalu praktik eksploitasi langsung di lab.

---

## 2. A04: Cryptographic Failures

### 2.1 Definisi

**Cryptographic Failures** terjadi ketika data sensitif tidak terlindungi secara memadai akibat kurangnya enkripsi, implementasi yang salah, atau langkah keamanan yang tidak memadai.

Contoh penyebab:

- Menyimpan password tanpa hashing.
- Menggunakan algoritma lemah/usang seperti **MD5**, **SHA1**, atau **DES**.
- Membocorkan encryption key.
- Tidak mengamankan data saat transmisi.
- "Rolling your own cryptography" — membuat algoritma enkripsi sendiri alih-alih memakai algoritma yang sudah teruji dan mapan.

### 2.2 Pencegahan

- Gunakan algoritma modern dan terbukti aman, jangan buat algoritma sendiri.
- Hash password dengan fungsi hashing lambat dan kuat: **bcrypt**, **scrypt**, atau **Argon2**.
- Jangan menyimpan kredensial (API key, password pihak ketiga, dll) langsung di source code, file konfigurasi, atau repository — gunakan sistem manajemen secret/key khusus.

### 2.3 Kasus di Lab: Weak XOR Cipher

Lab mendemonstrasikan layanan note-sharing yang mengenkripsi data dengan **XOR cipher** memakai key pendek (4 karakter) yang dipakai berulang (repeating key).

Kelemahan XOR cipher dengan key pendek:

- Key pendek mudah **di-brute-force** karena jumlah kombinasi terbatas.
- Pola key yang berulang memungkinkan **frequency analysis**.
- Tidak ada mekanisme autentikasi, sehingga sistem tidak bisa memverifikasi apakah hasil dekripsi benar tanpa konteks tambahan.
- Kriptografi buatan sendiri (homegrown crypto) tidak punya jaminan keamanan sekuat algoritma yang sudah teruji.

Fix yang direkomendasikan: pakai enkripsi standar industri seperti **AES-256-GCM** dengan manajemen key yang benar, jangan pernah membuat algoritma kriptografi sendiri.

---

## 3. A05: Injection

### 3.1 Definisi

**Injection** terjadi ketika aplikasi mengambil input pengguna dan menyalahgunakannya — input diteruskan langsung ke sistem yang mengeksekusi command atau query (database, shell, templating engine, API) tanpa diperlakukan sebagai data yang tidak terpercaya.

Contoh jenis injection klasik:

- **SQL Injection** — menyisipkan query SQL ke logika aplikasi (misal form login) karena input tidak disanitasi sebelum dipakai membangun query.
- **Command Injection** — menyisipkan command OS ke input yang diteruskan ke shell.
- **AI Prompt Injection** — menyisipkan instruksi berbahaya ke prompt AI.
- **Server Side Template Injection (SSTI)** — menyisipkan kode template yang dieksekusi oleh templating engine di server.

Injection tercatat sebagai kategori dengan **severity tinggi**, dan telah dua kali masuk OWASP Top 10 (2021 dan 2025).

### 3.2 Pencegahan

- Perlakukan semua input pengguna sebagai **untrusted**.
- Untuk query database: gunakan **prepared statements** dan **parameterized queries**, hindari membangun query lewat string concatenation.
- Untuk command OS: hindari fungsi yang meneruskan input langsung ke shell; gunakan API/proses yang tidak memanggil shell sama sekali.
- Terapkan validasi dan sanitasi input: escape karakter berbahaya, enforce tipe data ketat, filter input sebelum diproses aplikasi.

### 3.3 Konsep Kunci: Jinja2 dan SSTI

**Jinja2** adalah templating engine milik framework Flask (Python), berfungsi menggabungkan HTML statis dengan data dinamis dari backend. Normalnya, hanya *nilai variabel* yang disisipkan ke template — bukan struktur template itu sendiri.

**SSTI (Server Side Template Injection)** terjadi ketika input mentah dari user diperlakukan sebagai **kode template** (bukan sekadar data), sehingga ekspresi `{{ ... }}` yang disisipkan user benar-benar dievaluasi oleh Jinja2 sebagai ekspresi Python, bukan ditampilkan sebagai teks biasa.

Karena Jinja2 berjalan di atas Python, ekspresi yang bisa dieksekusi lewat SSTI adalah ekspresi Python penuh — jauh lebih kuat dibanding SQL Injection yang terbatas pada manipulasi query database.

### 3.4 Alur Eksploitasi SSTI (Lab: Injection Showcase)

Tahapan umum eksploitasi SSTI:

1. **Membuktikan eksekusi kode** — kirim ekspresi matematika sederhana untuk memastikan input dievaluasi sebagai template, bukan ditampilkan mentah.

```
{{ 7 * 7 }}
```

Jika output `49`, aplikasi terbukti rentan.

2. **Mengenali konteks/objek yang bisa diakses** — mengeksplorasi objek Python bawaan yang otomatis tersedia di dalam template Jinja2/Flask.

```
{{ config.items() }}
```

`config` adalah objek konfigurasi Flask; `.items()` menampilkan seluruh key-value konfigurasi aplikasi (DEBUG, SECRET_KEY, dll).

```
{{ request.__dict__ }}
```

`request` merepresentasikan HTTP request yang sedang berjalan; `__dict__` menampilkan seluruh atributnya.

3. **Mengeksekusi kode arbitrer untuk membaca file** — memanfaatkan rantai atribut Python untuk keluar dari sandbox template dan mengakses fungsi built-in.

```
{{ request.application.__globals__.__builtins__.open('flag.txt').read() }}
```

Penjelasan rantainya:

- `request.application` — mengakses objek aplikasi Flask dari objek request.
- `__globals__` — mengakses namespace global Python tempat semua modul/builtin terdaftar.
- `__builtins__` — mengakses fungsi built-in Python (termasuk `open()`) yang biasanya tersembunyi dari sandbox template.
- `open('flag.txt').read()` — membuka dan membaca isi file di server.

---

## 4. A08: Software or Data Integrity Failures

### 4.1 Definisi

**Software or Data Integrity Failures** terjadi ketika aplikasi mengasumsikan kode, update, atau data yang diterima aman **tanpa memverifikasi keaslian, integritas, atau asal-usulnya**.

Contoh penyebab:

- Mempercayai update software tanpa verifikasi.
- Memuat script/file konfigurasi dari sumber tidak terpercaya.
- Gagal memvalidasi data yang memengaruhi logika aplikasi.
- Menerima data (binary, template, JSON) tanpa memastikan apakah sudah diubah pihak lain.

### 4.2 Pencegahan

- Bangun **trust boundary** yang jelas — kode/update/data penting tidak boleh otomatis dianggap sah, integritasnya harus diverifikasi.
- Gunakan pengecekan kriptografis (checksum, digital signature) untuk paket update.
- Pastikan hanya sumber terpercaya yang bisa memodifikasi artefak kritikal.
- Terapkan trust boundary juga pada proses build seperti **CI/CD**.

### 4.3 Konsep Kunci: Serialisasi, Deserialisasi, dan Pickle

**Serialisasi**: mengubah objek Python (dictionary, list, instance class, dll) menjadi rangkaian byte agar bisa disimpan atau dikirim.

**Deserialisasi**: proses kebalikannya — membangun kembali objek Python dari rangkaian byte.

**Module `pickle`** di Python menangani kedua proses ini:

- `pickle.dumps(objek)` — serialisasi (objek → byte).
- `pickle.loads(byte)` — deserialisasi (byte → objek).

Deserialisasi pickle bukan sekadar membaca data — proses ini bisa **mengeksekusi kode Python**, karena pickle mendukung method spesial `__reduce__`.

**`__reduce__`** adalah method yang didefinisikan di dalam sebuah class dan dipanggil **saat serialisasi**, untuk menentukan instruksi "cara membangun ulang objek" yang akan dijalankan otomatis saat data tersebut nanti di-deserialize. Method ini harus mengembalikan tuple berformat `(fungsi_yang_dipanggil, tuple_argumen)`.

Karena instruksi ini otomatis dijalankan saat unpickle, penyerang bisa membuat class palsu dengan `__reduce__` yang diarahkan memanggil fungsi berbahaya (menjalankan command, membaca file, dll). Korban yang men-deserialize data tersebut tanpa verifikasi integritas akan otomatis menjalankan instruksi itu.

### 4.4 Karakteristik Fungsi Callable dalam Payload

Poin penting: tidak semua fungsi yang dipanggil lewat `__reduce__` akan menampilkan hasilnya sebagai teks yang bisa dibaca balik oleh aplikasi korban. Yang menentukan adalah **nilai return** dari fungsi tersebut:

- `os.system(command)` — menjalankan command, tapi hanya mengembalikan **exit code** (angka status), bukan isi output.
- `os.popen(command)` — mengembalikan **objek file** (`os._wrap_close`), bukan string isi output secara langsung; perlu `.read()` tambahan yang tidak bisa dirangkai langsung dalam satu pemanggilan `__reduce__`.
- `subprocess.check_output(args)` — mengembalikan output command dalam bentuk bytes, namun tetap perlu format argumen yang tepat.
- `eval(kode_string)` — menjalankan string sebagai ekspresi Python dan **mengembalikan hasil akhir ekspresi tersebut secara langsung**. Payload yang efektif memanfaatkan ini:

```python
return (eval, ("open('flag.txt').read()",))
```

Kode ini membuat `eval` menjalankan `open('flag.txt').read()`, sehingga hasil unpickle langsung berupa string isi file — bukan objek atau status.

### 4.5 Fix untuk Insecure Deserialization

- Gunakan format serialisasi aman: **JSON**, atau **YAML dengan `safe_load`**.
- Verifikasi digital signature sebelum melakukan deserialize.
- Terapkan whitelist tipe objek yang diizinkan.
- Gunakan restricted unpickler atau environment sandbox.
- Jangan pernah men-deserialize data yang berasal dari sumber tidak terpercaya.

---

## 5. Glosarium

- **Cryptographic Failures** — kegagalan melindungi data sensitif akibat enkripsi yang lemah, salah implementasi, atau tidak ada sama sekali.
- **Hashing** — proses satu arah mengubah data (misal password) menjadi representasi tetap yang tidak bisa dikembalikan ke bentuk asli.
- **bcrypt / scrypt / Argon2** — algoritma hashing lambat yang dirancang tahan terhadap brute-force, direkomendasikan untuk password.
- **XOR Cipher** — metode enkripsi sederhana berbasis operasi XOR; lemah jika key pendek atau berulang.
- **Injection** — kerentanan akibat input pengguna diteruskan tanpa validasi ke sistem yang mengeksekusi command/query.
- **SQL Injection** — injection yang menargetkan query database.
- **Command Injection** — injection yang menargetkan command shell/OS.
- **SSTI (Server Side Template Injection)** — injection yang menargetkan templating engine di server.
- **Jinja2** — templating engine Python yang dipakai Flask untuk merender konten dinamis.
- **`render_template_string`** — fungsi Flask yang merender string sebagai template Jinja2; berbahaya jika string tersebut berasal dari input user mentah.
- **`__globals__`** — atribut yang menyimpan referensi ke namespace global Python dari sebuah fungsi/objek.
- **`__builtins__`** — namespace yang berisi seluruh fungsi built-in Python (termasuk `open`), biasanya "tersembunyi" dari sandbox template.
- **Software/Data Integrity Failures** — kerentanan akibat aplikasi mempercayai kode/data tanpa verifikasi keaslian.
- **Trust Boundary** — batas kepercayaan yang menentukan data/kode mana yang harus diverifikasi sebelum digunakan.
- **Serialisasi** — mengubah objek Python menjadi rangkaian byte.
- **Deserialisasi** — membangun kembali objek Python dari rangkaian byte.
- **Pickle** — module Python untuk serialisasi/deserialisasi objek; rentan eksekusi kode arbitrer jika data yang di-unpickle tidak terpercaya.
- **`__reduce__`** — method class Python yang dipanggil saat serialisasi untuk menentukan instruksi pembangunan ulang objek saat deserialize; bisa disalahgunakan untuk memicu eksekusi kode.
- **`eval()`** — fungsi Python yang mengeksekusi string sebagai ekspresi kode dan mengembalikan hasil evaluasinya.
- **CI/CD** — proses build dan deployment otomatis; perlu trust boundary agar tidak menerima kode/dependency yang tidak terverifikasi.

---

## 6. Tools & Platform Rujukan

- **Insecure Deserialisation (TryHackMe Module)** — modul pembelajaran lanjutan tentang kerentanan deserialisasi.
- **Supply Chain Attack: Lottie (TryHackMe Module)** — modul pembelajaran tentang serangan supply chain terkait integritas software.
- **Injection Attacks (TryHackMe Module)** — modul pembelajaran lanjutan tentang berbagai jenis injection.
- **Command Injection (TryHackMe Module)** — modul pembelajaran lanjutan khusus command injection.
- **Cryptographic Failures (TryHackMe Module)** — modul pembelajaran lanjutan tentang kegagalan kriptografi.

*(Semua modul di atas disebutkan sebagai link rekomendasi di dalam materi room, tanpa URL eksplisit yang terlihat pada screenshot.)*

---

## 7. Catatan Ringkas untuk Ditulis Tangan

**A04 — Cryptographic Failures**
- Definisi — data sensitif tidak terlindungi: no encryption, faulty implementation, weak algo
- Algo lemah — MD5, SHA1, DES
- Rolling your own crypto — bikin algoritma sendiri = bahaya
- Hash password — bcrypt, scrypt, Argon2 (lambat & kuat)
- Jangan hardcode credential di source code/config/repo
- Fix — AES-256-GCM + key management yang benar
- Lab — XOR cipher, key pendek 4 karakter, brute-forceable

**A05 — Injection**
- Definisi — input user diteruskan mentah ke sistem eksekusi (DB, shell, template, API)
- Jenis — SQL Injection, Command Injection, AI Prompt Injection, SSTI
- Fix — prepared statements, parameterized query, no string concat, validasi & sanitasi input
- Jinja2 — templating engine Flask, gabungkan HTML + data dinamis
- SSTI — input user diperlakukan sebagai kode template, bukan data
- `render_template_string` — bahaya kalau isi dari input mentah
- Payload test — `{{7*7}}` → 49 = vulnerable
- Payload enum — `{{config.items()}}`, `{{request.__dict__}}`
- Payload baca file — `{{request.application.__globals__.__builtins__.open('flag.txt').read()}}`
- Rantai akses — application → __globals__ → __builtins__ → open()

**A08 — Software/Data Integrity Failures**
- Definisi — percaya kode/data/update tanpa verifikasi keaslian
- Fix — trust boundary, checksum/signature, whitelist sumber, integritas di CI/CD
- Serialisasi — objek Python → byte (`pickle.dumps`)
- Deserialisasi — byte → objek Python (`pickle.loads`)
- `__reduce__` — dipanggil saat serialize, tentukan instruksi rebuild saat deserialize
- Return format — `(fungsi, (argumen,))`
- os.system — cuma return exit code, BUKAN output teks
- os.popen — return objek file, butuh .read() terpisah, gak bisa dirantai
- subprocess.check_output — return bytes output, perlu format argumen tepat
- eval — jalankan string sebagai kode, return hasil evaluasi langsung — PALING EFEKTIF
- Payload final — `(eval, ("open('flag.txt').read()",))`
- Fix deserialization — JSON/YAML safe_load, verify signature, whitelist type, restricted unpickler, never deserialize untrusted data
