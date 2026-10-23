# Resume Materi: Introduction to Database & SQL
**TryHackMe — Pre-Security Path**
**Tanggal: 01 September 2026**

---

## 1. Konsep Dasar: Data dan Database

**Data** adalah fakta atau informasi mentah — berupa angka, teks, waktu, nama, atau nilai lain yang ketika diorganisir menjadi bermakna dan berguna untuk pengambilan keputusan.

**Database** adalah sistem penyimpanan data secara terstruktur di dalam komputer. Database bukan sekadar tempat menyimpan — ia dirancang agar data bisa dicari, difilter, dihitung, dan diurutkan dengan sangat cepat, bahkan ketika jumlah datanya mencapai ribuan atau jutaan entri.

Analogi yang digunakan di course ini: database seperti **buku catatan digital yang tidak pernah kehabisan halaman**. Berbeda dengan buku catatan fisik, database memungkinkan pencarian instan tanpa harus membaca satu per satu dari awal.

### 1.1 Mengapa File Biasa Tidak Cukup

Ketika bisnis masih kecil, mencatat data di buku tulis atau file biasa masih bisa ditoleransi. Namun ketika data bertambah banyak, metode ini menjadi:

- Lambat — harus membaca halaman demi halaman untuk menemukan satu informasi
- Tidak efisien — menghitung atau mengurutkan data harus dilakukan secara manual
- Rentan error — semakin banyak data, semakin mudah terjadi kesalahan pencatatan

Database hadir sebagai solusi: data tersimpan dalam struktur yang konsisten dan bisa dijawab dalam hitungan detik, tidak peduli seberapa banyak entrinya.

---

## 2. Struktur Database: Tabel, Kolom, dan Baris

Di dalam database, seluruh informasi disimpan dalam bentuk **tabel**. Tabel adalah unit penyimpanan utama dalam database relasional, dan strukturnya menyerupai spreadsheet.

### 2.1 Komponen Tabel

**Kolom** adalah judul-judul yang berada di bagian atas tabel. Setiap kolom merepresentasikan satu jenis informasi tertentu — misalnya kolom `price` hanya berisi data harga, tidak bercampur dengan data lain.

**Baris** membentang secara horizontal melintasi tabel. Setiap baris berisi satu set informasi yang lengkap tentang satu entri. Dalam konteks kafe, satu baris berarti satu pesanan.

Contoh tabel `Orders` dari kafe:

| id | drink  | price | time     |
|----|--------|-------|----------|
| 1  | Coffee | 2.00  | 11:00 AM |
| 2  | Tea    | 1.50  | 11:10 AM |
| 3  | Muffin | 2.50  | 11:15 AM |

### 2.2 Aturan Perubahan Data dalam Tabel

- Jika ada pesanan baru masuk, satu baris baru ditambahkan ke tabel
- Jika sebuah pesanan dihapus, hanya baris tersebut yang hilang — baris lainnya tidak terpengaruh
- Jumlah baris dalam tabel selalu mencerminkan jumlah entri yang tersimpan saat itu

---

## 3. SQL: Bahasa untuk Berkomunikasi dengan Database

**SQL (Structured Query Language)** adalah bahasa yang digunakan untuk berkomunikasi dengan database. SQL memungkinkan kita menyimpan, mengambil, mengubah, dan menghapus data tanpa harus membaca tabel secara manual baris per baris.

Perintah yang dikirimkan ke database disebut **query**. Penting untuk dipahami bahwa query SQL yang bersifat `SELECT` **tidak mengubah data sama sekali** — ia hanya menampilkan informasi yang diminta berdasarkan kondisi yang ditentukan.

---

## 4. Empat Perintah SQL Inti

### 4.1 SELECT

Menentukan kolom mana yang ingin ditampilkan dari hasil query.

```sql
SELECT *
SELECT drink, price
```

- `*` berarti semua kolom ditampilkan
- Menyebutkan nama kolom secara spesifik akan menampilkan hanya kolom tersebut

### 4.2 FROM

Menentukan tabel mana yang menjadi sumber data query.

```sql
FROM Orders
FROM Menu
```

Selalu digunakan berpasangan dengan `SELECT`. Tanpa `FROM`, database tidak tahu harus mengambil data dari mana.

### 4.3 WHERE

Memfilter baris berdasarkan kondisi tertentu. Hanya baris yang memenuhi kondisi yang akan ditampilkan; baris lainnya diabaikan.

```sql
WHERE drink = 'Coffee'
WHERE price = 1.50
```

Nilai teks dalam kondisi `WHERE` ditulis menggunakan **tanda kutip tunggal** `' '`.

### 4.4 ORDER BY

Mengurutkan hasil query berdasarkan kolom tertentu.

```sql
ORDER BY price
ORDER BY price DESC
```

- Default (tanpa tambahan apapun): **ascending** — dari nilai terendah ke tertinggi, atau A ke Z
- Tambahkan `DESC` untuk **descending** — dari nilai tertinggi ke terendah, atau Z ke A

---

## 5. Query SQL: Pola dan Kombinasi

Urutan penulisan yang benar dan tidak bisa ditukar:

```
SELECT → FROM → WHERE → ORDER BY
```

### 5.1 Daftar Query Beserta Fungsinya

Tampilkan seluruh data dari tabel Orders:

```sql
SELECT * FROM Orders;
```

Tampilkan hanya kolom drink dan price:

```sql
SELECT drink, price FROM Orders;
```

Filter hanya pesanan Coffee:

```sql
SELECT * FROM Orders WHERE drink = 'Coffee';
```

Urutkan semua pesanan berdasarkan harga (termurah dulu):

```sql
SELECT * FROM Orders ORDER BY price;
```

Urutkan semua pesanan berdasarkan harga (termahal dulu):

```sql
SELECT * FROM Orders ORDER BY price DESC;
```

Filter Coffee, urutkan dari harga tertinggi:

```sql
SELECT * FROM Orders WHERE drink = 'Coffee' ORDER BY price DESC;
```

Lihat daftar minuman yang tersedia di menu:

```sql
SELECT * FROM Menu;
```

---

## 6. Tabel yang Digunakan dalam Latihan (Café SQL)

Dua tabel tersedia di lingkungan latihan Café SQL:

- **Orders** — kolom: `id`, `drink`, `price`, `time`
- **Menu** — kolom: `drink`, `price`

Lingkungan ini bersifat aman — kesalahan query tidak akan merusak data permanen. Tombol **Reset Data** tersedia untuk mengembalikan data ke kondisi awal kapan saja.

---

## 7. Integritas Data dan Keamanan Database

Course ini diakhiri dengan pertanyaan refleksi: *"Apa yang bisa terjadi jika seseorang diizinkan mengubah atau menghapus data tanpa izin?"*

Pertanyaan ini menyentuh konsep **integritas data** — prinsip bahwa data dalam database harus akurat, konsisten, dan hanya bisa dimodifikasi oleh pihak yang berwenang.

Jika kontrol akses tidak diterapkan:

- Data bisa diubah atau dihapus secara sembarangan
- Catatan keuangan atau operasional bisa dimanipulasi
- Keputusan bisnis bisa didasarkan pada data yang tidak valid

Inilah mengapa sistem database nyata selalu menerapkan **kontrol akses (access control)** — mekanisme yang membatasi siapa yang boleh melakukan operasi apa terhadap data mana.

---

## 8. Glossary: Istilah Penting

**Database** — sistem penyimpanan data terstruktur yang memungkinkan pencarian, pengurutan, dan pengelolaan data secara efisien.

**Tabel** — unit penyimpanan utama dalam database, terdiri dari baris dan kolom yang terorganisir seperti spreadsheet.

**Kolom** — kategori atau jenis data tertentu dalam tabel, berada di bagian atas (vertikal).

**Baris** — satu entri atau rekaman data lengkap dalam tabel (horizontal).

**SQL** — Structured Query Language; bahasa standar untuk berkomunikasi dengan database.

**Query** — perintah yang dikirim ke database untuk mengambil atau memanipulasi data.

**SELECT** — perintah SQL untuk memilih kolom yang ingin ditampilkan.

**FROM** — perintah SQL untuk menentukan tabel sumber data.

**WHERE** — perintah SQL untuk memfilter baris berdasarkan kondisi.

**ORDER BY** — perintah SQL untuk mengurutkan hasil query.

**ASC** — ascending; urutan dari nilai terkecil ke terbesar (default ORDER BY).

**DESC** — descending; urutan dari nilai terbesar ke terkecil.

**Integritas Data** — prinsip bahwa data harus akurat, konsisten, dan hanya dapat dimodifikasi oleh pihak yang berwenang.

**Kontrol Akses** — mekanisme yang membatasi hak pengguna dalam melakukan operasi terhadap data di dalam database.

---

## 9. Tools & Platform Rujukan

**Café SQL**
- Fungsi: database client berbasis browser untuk berlatih menulis query SQL secara interaktif dalam simulasi kafe virtual
- URL: disediakan langsung di dalam room TryHackMe (split view)

---

## 10. Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar

- Data — fakta mentah (angka, teks, waktu, dll)
- Database — penyimpanan data terstruktur, bisa dicari/filter/urutkan cepat
- File biasa — lambat, tidak efisien saat data banyak
- Database = solusi: jawab pertanyaan dalam detik meski ribuan entri

### Struktur Tabel

- Tabel — unit penyimpanan utama, mirip spreadsheet
- Kolom — jenis/kategori data (vertikal, di atas)
- Baris — satu entri/rekaman lengkap (horizontal)
- Tambah pesanan = tambah baris; hapus pesanan = hapus baris itu saja

### SQL

- SQL — Structured Query Language, bahasa komunikasi dengan database
- Query — perintah yang dikirim ke database
- SELECT query — tidak mengubah data, hanya menampilkan

### 4 Perintah Inti (urutan wajib)

- SELECT — pilih kolom apa yang ditampilkan (* = semua kolom)
- FROM — pilih dari tabel mana
- WHERE — filter baris berdasarkan kondisi (nilai teks pakai ' ')
- ORDER BY — urutkan hasil (default: ASC, tambah DESC untuk terbalik)

### Contoh Query Kunci

- SELECT * FROM Orders; — semua data semua kolom
- SELECT drink, price FROM Orders; — hanya kolom drink dan price
- SELECT * FROM Orders WHERE drink = 'Coffee'; — filter Coffee saja
- SELECT * FROM Orders ORDER BY price; — urutkan harga terendah
- SELECT * FROM Orders ORDER BY price DESC; — urutkan harga tertinggi
- SELECT * FROM Orders WHERE drink = 'Coffee' ORDER BY price DESC; — filter + urutkan

### Tabel Latihan (Café SQL)

- Orders: id, drink, price, time
- Menu: drink, price

### Integritas Data

- Integritas data — data harus akurat dan hanya diubah oleh pihak berwenang
- Kontrol akses — batasi siapa yang boleh ubah/hapus data
- Tanpa kontrol akses — data bisa dimanipulasi sembarangan
