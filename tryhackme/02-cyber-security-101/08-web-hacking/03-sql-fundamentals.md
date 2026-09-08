# Resume Materi: SQL Fundamentals
**Platform:** TryHackMe
**Tanggal Selesai:** 8 September 2026

---

## Daftar Isi

1. Konsep Dasar Database
2. Jenis-Jenis Database
3. Struktur Relational Database
4. Primary Key dan Foreign Key
5. SQL dan DBMS
6. Klasifikasi SQL Statement (DDL, DML, DQL, DCL)
7. Database Statements (DDL)
8. Table Statements (DDL)
9. CRUD Operations (DML & DQL)
10. Clauses
11. Operators
12. Functions
13. Glossary
14. Tools & Platform Rujukan
15. Catatan Ringkas untuk Ditulis Tangan

---

## 1. Konsep Dasar Database

**Database** adalah kumpulan terorganisir dari informasi atau data terstruktur yang mudah diakses, dapat dimanipulasi, dan dapat dianalisis.

Database digunakan hampir di semua aspek sistem digital, mulai dari autentikasi pengguna (username & password), data media sosial (postingan, komentar, likes), riwayat tontonan di platform streaming, hingga transaksi e-commerce.

Dalam konteks cyber security, database relevan di dua sisi:

- **Sisi ofensif** — memahami kerentanan SQL seperti SQL injection untuk mengeksploitasi atau mengambil data dari sistem yang disusupi.
- **Sisi defensif** — menelusuri database untuk menemukan aktivitas mencurigakan dan menerapkan restriksi untuk melindungi sistem.

---

## 2. Jenis-Jenis Database

Ada banyak jenis database, namun dua tipe utama yang perlu dipahami adalah:

### 2.1 Relational Database (SQL)

Menyimpan data dalam format **terstruktur** — data harus mengikuti skema yang sudah ditentukan (kolom, tipe data). Data disimpan dalam baris dan kolom di dalam tabel. Hubungan antar tabel bisa dibuat menggunakan keys.

Cocok digunakan ketika data yang masuk **konsisten formatnya** dan **akurasi sangat penting**, misalnya transaksi e-commerce.

Tipe penyimpanan: **Table-based**

### 2.2 Non-Relational Database (NoSQL)

Menyimpan data dalam format **non-tabular** — tidak ada skema kaku, data bisa bervariasi jenis dan jumlahnya antar record. Contoh format penyimpanannya seperti dokumen JSON/BSON.

Cocok digunakan ketika data yang masuk **bervariasi formatnya** namun tetap perlu dikumpulkan di satu tempat, misalnya konten media sosial.

Subtipe non-relational database:

- **Key-value** — data disimpan sebagai pasangan kunci-nilai
- **Graph** — data disimpan sebagai node dan relasi antar node
- **Wide-column** — data disimpan dalam kolom yang bisa sangat banyak dan bervariasi
- **Document** — data disimpan sebagai dokumen (JSON, BSON, XML)

---

## 3. Struktur Relational Database

### 3.1 Table

Semua data dalam relational database disimpan di dalam **table**. Satu tabel merepresentasikan satu entitas, misalnya tabel "Books" menyimpan data buku.

### 3.2 Column (Kolom)

Kolom mendefinisikan **struktur** tabel — apa saja informasi yang disimpan dan tipe datanya. Setiap kolom harus memiliki tipe data yang ditentukan saat pembuatan tabel.

Jika data yang dimasukkan tidak sesuai tipe data kolom, **record akan ditolak**.

Tipe data utama yang didukung:

- **String / VARCHAR** — teks dan karakter
- **Integer (INT)** — bilangan bulat
- **Float / Decimal** — angka dengan titik desimal
- **Date / DateTime** — tanggal dan waktu

### 3.3 Row (Baris)

Satu **row** merepresentasikan satu record data yang sudah dimasukkan ke dalam tabel. Kalau kolom adalah definisi struktur, row adalah isi nyatanya.

---

## 4. Primary Key dan Foreign Key

### 4.1 Primary Key

**Primary key** adalah kolom yang nilainya **unik untuk setiap record** dalam tabel — tidak boleh ada dua row dengan nilai primary key yang sama, dan nilainya tidak boleh NULL.

Fungsinya untuk mengidentifikasi setiap record secara unik. Hanya boleh ada **satu kolom primary key** per tabel.

Contoh: kolom `id` yang menggunakan `AUTO_INCREMENT` — setiap record baru otomatis mendapat nomor unik yang bertambah.

### 4.2 Foreign Key

**Foreign key** adalah kolom dalam satu tabel yang merujuk ke primary key di tabel lain. Fungsinya sebagai **penghubung antar tabel** sehingga data dari dua tabel bisa direlasikan.

Satu tabel boleh memiliki **lebih dari satu foreign key**. Inilah mekanisme utama yang membuat relational database bisa membentuk hubungan antar entitas.

Contoh: kolom `author_id` di tabel Books merujuk ke kolom `id` di tabel Authors.

---

## 5. SQL dan DBMS

### 5.1 DBMS (Database Management System)

**DBMS** adalah perangkat lunak yang berfungsi sebagai antarmuka antara pengguna dan database. DBMS memungkinkan pengguna untuk mengambil, memperbarui, dan mengelola data.

Contoh DBMS populer: **MySQL, MongoDB, Oracle Database, MariaDB**

### 5.2 SQL (Structured Query Language)

**SQL** adalah bahasa pemrograman yang digunakan untuk berinteraksi dengan relational database — untuk melakukan query, mendefinisikan struktur, dan memanipulasi data.

Keunggulan SQL:

- **Cepat** — dapat mengembalikan data dalam jumlah besar hampir secara instan
- **Mudah dipelajari** — ditulis dalam bahasa Inggris yang mudah dibaca
- **Reliable** — struktur ketat menjamin akurasi data
- **Fleksibel** — mendukung berbagai jenis operasi analisis data

---

## 6. Klasifikasi SQL Statement

| Kategori | Kepanjangan | Fungsi | Contoh Statement |
|---|---|---|---|
| **DDL** | Data Definition Language | Mendefinisikan/mengelola struktur database & tabel | CREATE, ALTER, DROP, SHOW, DESCRIBE |
| **DML** | Data Manipulation Language | Memanipulasi data dalam tabel | INSERT, UPDATE, DELETE |
| **DQL** | Data Query Language | Mengambil/membaca data dari tabel | SELECT |
| **DCL** | Data Control Language | Mengatur hak akses ke database | GRANT, REVOKE |

---

## 7. Database Statements (DDL)

### 7.1 CREATE DATABASE

Membuat database baru.

```sql
CREATE DATABASE database_name;
```

### 7.2 SHOW DATABASES

Menampilkan daftar semua database yang ada. Database default yang selalu ada: `mysql`, `information_schema`, `performance_schema`, `sys`.

```sql
SHOW DATABASES;
```

### 7.3 USE

Menetapkan database tertentu sebagai database aktif. Semua query selanjutnya akan dijalankan terhadap database ini.

```sql
USE database_name;
```

### 7.4 DROP DATABASE

Menghapus database beserta seluruh isinya secara permanen.

```sql
DROP database database_name;
```

---

## 8. Table Statements (DDL)

### 8.1 CREATE TABLE

Membuat tabel baru dalam database yang sedang aktif. Setiap kolom didefinisikan dengan nama dan tipe datanya.

```sql
CREATE TABLE table_name (
    column1 data_type constraints,
    column2 data_type constraints,
    column3 data_type
);
```

**Contoh nyata:**

```sql
CREATE TABLE book_inventory (
    book_id INT AUTO_INCREMENT PRIMARY KEY,
    book_name VARCHAR(255) NOT NULL,
    publication_date DATE
);
```

Penjelasan constraint yang digunakan:

- `INT` — tipe data bilangan bulat
- `AUTO_INCREMENT` — nilai otomatis bertambah untuk setiap record baru
- `PRIMARY KEY` — menetapkan kolom ini sebagai primary key tabel
- `VARCHAR(255)` — teks dengan panjang maksimal 255 karakter
- `NOT NULL` — kolom tidak boleh kosong; record akan ditolak jika kosong
- `DATE` — tipe data tanggal

### 8.2 SHOW TABLES

Menampilkan daftar semua tabel dalam database yang sedang aktif.

```sql
SHOW TABLES;
```

### 8.3 DESCRIBE / DESC

Menampilkan detail struktur sebuah tabel: nama kolom, tipe data, nullable, key, default value, dan extra.

```sql
DESCRIBE table_name;
-- atau
DESC table_name;
```

### 8.4 ALTER TABLE

Mengubah struktur tabel yang sudah ada — bisa menambah kolom, mengganti nama kolom, mengubah tipe data kolom, atau menghapus kolom.

```sql
-- Menambah kolom baru
ALTER TABLE table_name
ADD column_name data_type;
```

### 8.5 DROP TABLE

Menghapus tabel beserta seluruh datanya secara permanen.

```sql
DROP TABLE table_name;
```

---

## 9. CRUD Operations (DML & DQL)

**CRUD** adalah singkatan dari Create, Read, Update, Delete — empat operasi dasar dalam sistem manajemen data.

### 9.1 Create — INSERT INTO

Menambahkan record baru ke dalam tabel.

```sql
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);
```

### 9.2 Read — SELECT

Mengambil data dari tabel. Bisa mengambil semua kolom atau kolom tertentu saja.

```sql
-- Ambil semua kolom
SELECT * FROM table_name;

-- Ambil kolom tertentu
SELECT column1, column2 FROM table_name;
```

Simbol `*` berarti "semua kolom".

### 9.3 Update — UPDATE

Mengubah nilai pada record yang sudah ada. Clause `WHERE` wajib digunakan untuk menentukan record mana yang diubah — tanpa `WHERE`, semua row dalam tabel akan berubah.

```sql
UPDATE table_name
SET column_name = new_value
WHERE condition;
```

### 9.4 Delete — DELETE

Menghapus record dari tabel. Sama seperti UPDATE, `WHERE` wajib digunakan — tanpa `WHERE`, semua row akan terhapus.

```sql
DELETE FROM table_name WHERE condition;
```

---

## 10. Clauses

**Clause** adalah bagian dari statement SQL yang mendefinisikan kriteria data — bagaimana data harus diambil, difilter, diurutkan, atau dikelompokkan.

### 10.1 FROM

Menentukan tabel mana yang diakses oleh query.

```sql
SELECT * FROM books;
```

### 10.2 WHERE

Memfilter record berdasarkan kondisi tertentu. Hanya record yang memenuhi kondisi yang akan dikembalikan.

```sql
SELECT * FROM books WHERE id = 1;
```

### 10.3 DISTINCT

Menghilangkan duplikat dari hasil query — hanya mengembalikan nilai yang unik.

```sql
SELECT DISTINCT column_name FROM table_name;
```

### 10.4 GROUP BY

Mengagregasi data dari beberapa record dan mengelompokkan hasil berdasarkan kolom. Sering dipakai bersama fungsi agregasi seperti `COUNT()`.

```sql
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name;
```

### 10.5 ORDER BY

Mengurutkan hasil query secara ascending (ASC) atau descending (DESC). Default-nya adalah ASC jika tidak ditentukan.

```sql
-- Urutan naik (terkecil ke terbesar)
SELECT * FROM table_name ORDER BY column_name ASC;

-- Urutan turun (terbesar ke terkecil)
SELECT * FROM table_name ORDER BY column_name DESC;
```

### 10.6 HAVING

Memfilter hasil **setelah** agregasi dilakukan (berbeda dengan WHERE yang memfilter sebelum agregasi). Digunakan bersama GROUP BY.

```sql
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name
HAVING condition;
```

Perbedaan kunci `WHERE` vs `HAVING`:

- `WHERE` — menyaring data **sebelum** dikelompokkan
- `HAVING` — menyaring data **setelah** dikelompokkan

### 10.7 LIMIT

Membatasi jumlah baris yang dikembalikan oleh query.

```sql
-- Ambil 5 baris pertama
SELECT * FROM table_name LIMIT 5;

-- Offset: skip N baris, lalu ambil M baris
SELECT * FROM table_name LIMIT offset, count;
-- Contoh: LIMIT 1, 1 = skip 1 baris pertama, ambil 1 baris berikutnya
```

---

## 11. Operators

### 11.1 Logical Operators

Operator logika menguji kebenaran kondisi dan mengembalikan nilai boolean TRUE atau FALSE.

**LIKE**

Mencari pola tertentu dalam sebuah kolom. Menggunakan wildcard `%` (mewakili karakter apapun, sebanyak apapun) dan `_` (mewakili satu karakter).

```sql
WHERE column_name LIKE "%keyword%"
-- %keyword% = mengandung kata "keyword" di mana saja
-- keyword%  = dimulai dengan "keyword"
-- %keyword  = diakhiri dengan "keyword"
```

**AND**

Mengembalikan TRUE hanya jika **semua kondisi** terpenuhi.

```sql
WHERE condition1 AND condition2
```

**OR**

Mengembalikan TRUE jika **setidaknya satu kondisi** terpenuhi.

```sql
WHERE condition1 OR condition2
```

**NOT**

Membalikkan nilai boolean — mengecualikan kondisi yang ditentukan.

```sql
WHERE NOT condition
WHERE NOT column LIKE "%keyword%"
```

**BETWEEN**

Menguji apakah nilai berada dalam rentang tertentu (inklusif — nilai batas ikut dihitung).

```sql
WHERE column_name BETWEEN value1 AND value2
```

### 11.2 Comparison Operators

Operator perbandingan membandingkan dua nilai dan mengecek apakah kondisi terpenuhi.

| Operator | Nama | Fungsi |
|---|---|---|
| `=` | Equal To | Nilai harus sama persis |
| `!=` | Not Equal To | Nilai harus berbeda |
| `<` | Less Than | Nilai harus lebih kecil |
| `>` | Greater Than | Nilai harus lebih besar |
| `<=` | Less Than or Equal To | Nilai harus lebih kecil atau sama dengan |
| `>=` | Greater Than or Equal To | Nilai harus lebih besar atau sama dengan |

```sql
WHERE name = "Designing Secure Software"
WHERE category != "Offensive Security"
WHERE published_date < "2020-01-01"
WHERE published_date > "2020-01-01"
WHERE published_date <= "2021-11-15"
WHERE published_date >= "2021-11-02"
```

---

## 12. Functions

### 12.1 String Functions

Melakukan operasi pada string dan mengembalikan nilai hasil operasi tersebut.

**CONCAT()**

Menggabungkan dua string atau lebih menjadi satu. Berguna untuk menggabungkan teks dari kolom berbeda.

```sql
SELECT CONCAT(col1, " separator ", col2) AS alias FROM table_name;
```

**GROUP_CONCAT()**

Menggabungkan nilai dari beberapa baris dalam satu kelompok menjadi satu string. Digunakan bersama `GROUP BY`.

```sql
SELECT category, GROUP_CONCAT(name SEPARATOR ", ") AS books
FROM table_name
GROUP BY category;
```

**SUBSTRING()**

Mengambil sebagian karakter dari string, dimulai dari posisi tertentu dengan panjang yang ditentukan.

```sql
SELECT SUBSTRING(column_name, start_position, length) AS alias FROM table_name;
-- Contoh: SUBSTRING(published_date, 1, 4) → mengambil 4 karakter dari posisi 1 (tahun)
```

**LENGTH()**

Mengembalikan jumlah karakter dalam sebuah string, termasuk spasi dan tanda baca.

```sql
SELECT LENGTH(column_name) AS alias FROM table_name;
```

### 12.2 Aggregate Functions

Mengagregasi nilai dari beberapa baris menjadi satu hasil tunggal.

**COUNT()**

Menghitung jumlah record dalam sebuah ekspresi.

```sql
SELECT COUNT(*) AS total FROM table_name;
```

**SUM()**

Menjumlahkan semua nilai (tidak termasuk NULL) dari kolom yang ditentukan.

```sql
SELECT SUM(column_name) AS total FROM table_name;
```

**MAX()**

Mengembalikan nilai terbesar dari kolom yang ditentukan.

```sql
SELECT MAX(column_name) AS alias FROM table_name;
```

**MIN()**

Mengembalikan nilai terkecil dari kolom yang ditentukan.

```sql
SELECT MIN(column_name) AS alias FROM table_name;
```

---

## 13. Glossary

**Database** — kumpulan terorganisir dari informasi atau data terstruktur yang mudah diakses, dapat dimanipulasi dan dianalisis.

**DBMS (Database Management System)** — perangkat lunak yang berfungsi sebagai antarmuka antara pengguna dan database.

**SQL (Structured Query Language)** — bahasa pemrograman untuk berinteraksi dengan relational database.

**Relational Database** — database yang menyimpan data terstruktur dalam tabel dengan baris dan kolom; antar tabel bisa saling terhubung.

**Non-Relational Database (NoSQL)** — database yang menyimpan data dalam format non-tabular; lebih fleksibel untuk data yang bervariasi.

**Table** — struktur penyimpanan data dalam relational database yang terdiri dari kolom dan baris.

**Column** — mendefinisikan jenis informasi dan tipe data yang disimpan dalam tabel.

**Row** — satu record data yang sudah dimasukkan ke dalam tabel.

**Primary Key** — kolom dengan nilai unik yang mengidentifikasi setiap record secara unik dalam tabel; hanya boleh ada satu per tabel.

**Foreign Key** — kolom yang merujuk ke primary key di tabel lain; menghubungkan dua tabel; bisa lebih dari satu per tabel.

**DDL (Data Definition Language)** — kategori SQL statement untuk mendefinisikan dan mengelola struktur database dan tabel.

**DML (Data Manipulation Language)** — kategori SQL statement untuk memanipulasi data dalam tabel.

**DQL (Data Query Language)** — kategori SQL statement untuk mengambil data dari tabel.

**DCL (Data Control Language)** — kategori SQL statement untuk mengatur hak akses ke database.

**CRUD** — singkatan dari Create, Read, Update, Delete; empat operasi dasar manajemen data.

**Clause** — bagian dari SQL statement yang menentukan kriteria data yang dimanipulasi atau diambil.

**Operator** — simbol atau kata kunci yang digunakan untuk memfilter dan membandingkan nilai dalam query.

**Function** — perintah bawaan SQL yang melakukan operasi tertentu pada data dan mengembalikan nilai hasilnya.

**AUTO_INCREMENT** — constraint yang membuat nilai kolom bertambah otomatis setiap kali record baru dimasukkan.

**NOT NULL** — constraint yang memastikan kolom tidak boleh diisi dengan nilai kosong.

**VARCHAR(n)** — tipe data teks dengan panjang variabel, maksimal n karakter.

**Wildcard (%)** — karakter khusus dalam LIKE yang mewakili karakter apapun sebanyak apapun.

**Aggregate** — operasi yang menggabungkan nilai dari beberapa baris menjadi satu hasil.

**SQL Injection** — teknik serangan yang memasukkan query SQL berbahaya ke dalam input aplikasi untuk memanipulasi database.

---

## 14. Tools & Platform Rujukan

**TryHackMe**
Platform belajar cyber security berbasis hands-on dan room interaktif. Room SQL Fundamentals ada di platform ini.
URL: https://tryhackme.com

**MySQL**
DBMS relational database open-source yang paling banyak digunakan. Digunakan sebagai environment praktik di room ini.
URL: https://www.mysql.com

**MariaDB**
Fork dari MySQL yang kompatibel penuh; disebutkan sebagai salah satu contoh DBMS.
URL: https://mariadb.org

**MongoDB**
DBMS non-relational (NoSQL) berbasis dokumen; disebutkan sebagai contoh DBMS non-relational.
URL: https://www.mongodb.com

**Oracle Database**
DBMS enterprise dari Oracle Corporation; disebutkan sebagai contoh DBMS.
URL: https://www.oracle.com/database

---

## 15. Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar

- Database — kumpulan data terorganisir yang mudah diakses & dimanipulasi
- DBMS — software antarmuka antara user dan database (MySQL, MongoDB, dll)
- SQL — bahasa untuk berinteraksi dengan relational database
- Relational DB — data terstruktur, tabel, cocok untuk data konsisten & akurasi tinggi
- Non-relational DB (NoSQL) — non-tabular, fleksibel, cocok untuk data bervariasi
- Subtipe NoSQL — Key-value, Graph, Wide-column, Document

### Struktur Relational DB

- Table — tempat penyimpanan data (satu entitas = satu tabel)
- Column — definisi struktur & tipe data
- Row — satu record data yang sudah dimasukkan
- Tipe data utama — STRING/VARCHAR, INT, FLOAT/DECIMAL, DATE
- Primary Key — unik per record, hanya 1 per tabel, tidak boleh NULL
- Foreign Key — merujuk primary key tabel lain, bisa lebih dari 1 per tabel

### Klasifikasi SQL

- DDL — struktur (CREATE, ALTER, DROP, SHOW, DESCRIBE)
- DML — manipulasi data (INSERT, UPDATE, DELETE)
- DQL — baca data (SELECT)
- DCL — hak akses (GRANT, REVOKE)

### Database Statements (DDL)

- CREATE DATABASE nama — buat database baru
- SHOW DATABASES — lihat daftar database
- USE nama — pilih database aktif
- DROP database nama — hapus database permanen

### Table Statements (DDL)

- CREATE TABLE nama (kolom tipe constraint) — buat tabel baru
- SHOW TABLES — lihat daftar tabel
- DESCRIBE / DESC nama — lihat struktur tabel
- ALTER TABLE nama ADD kolom tipe — tambah kolom baru
- DROP TABLE nama — hapus tabel permanen
- Constraint: INT, VARCHAR(255), DATE, NOT NULL, AUTO_INCREMENT, PRIMARY KEY

### CRUD Operations

- INSERT INTO tabel (kol1, kol2) VALUES (val1, val2) — tambah record
- SELECT * FROM tabel — ambil semua kolom
- SELECT kol1, kol2 FROM tabel — ambil kolom tertentu
- UPDATE tabel SET kol=nilai WHERE kondisi — ubah record (WAJIB WHERE)
- DELETE FROM tabel WHERE kondisi — hapus record (WAJIB WHERE)

### Clauses

- FROM — tentukan tabel yang diakses
- WHERE — filter record sebelum agregasi
- DISTINCT — hilangkan duplikat, hanya nilai unik
- GROUP BY — kelompokkan hasil berdasarkan kolom
- ORDER BY kolom ASC/DESC — urutkan hasil naik/turun
- HAVING — filter setelah agregasi (beda dari WHERE)
- LIMIT n — batasi jumlah baris hasil
- LIMIT offset, count — skip offset baris, ambil count baris

### Logical Operators

- LIKE '%keyword%' — mengandung kata tersebut di mana saja
- LIKE 'keyword%' — dimulai dengan kata tersebut
- LIKE '%keyword' — diakhiri dengan kata tersebut
- AND — semua kondisi harus TRUE
- OR — minimal satu kondisi TRUE
- NOT — balikkan kondisi (kecualikan)
- BETWEEN val1 AND val2 — nilai dalam rentang (inklusif)

### Comparison Operators

- = sama persis
- != tidak sama
- < lebih kecil dari
- > lebih besar dari
- <= lebih kecil atau sama dengan
- >= lebih besar atau sama dengan

### String Functions

- CONCAT(kol1, " teks ", kol2) AS alias — gabung string dari beberapa kolom
- GROUP_CONCAT(kolom SEPARATOR ", ") — gabung nilai banyak baris jadi satu string
- SUBSTRING(kolom, posisi_mulai, panjang) AS alias — ambil sebagian karakter
- LENGTH(kolom) AS alias — hitung jumlah karakter (termasuk spasi & tanda baca)

### Aggregate Functions

- COUNT(*) AS alias — hitung jumlah record
- SUM(kolom) AS alias — jumlahkan semua nilai kolom
- MAX(kolom) AS alias — nilai terbesar dalam kolom
- MIN(kolom) AS alias — nilai terkecil dalam kolom
