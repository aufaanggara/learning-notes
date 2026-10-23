# Resume Room: Elastic Stack (ELK) untuk Log Analysis dan Investigasi

Tanggal: 22 Oktober 2026

Cakupan: Task 1 sampai Task 8 (Introduction, Elastic Stack Overview, Lab Connection, Discover Tab, KQL Overview, Creating Visualizations, Creating Dashboards, Conclusion)

## 1. Gambaran Umum

### 1.1 Apa itu Elastic Stack (ELK)

**Elastic Stack (ELK)** awalnya dikembangkan untuk menyimpan, mencari, dan memvisualisasikan data dalam jumlah besar. Organisasi memakainya untuk memonitor performa aplikasi dan mencari data pada dataset besar.

Seiring waktu, kemampuan pencarian dan visualisasinya membuat ELK populer di operasi keamanan. ELK **bukan SIEM tradisional**, tetapi banyak tim **SOC** memakainya hampir seperti SIEM.

ELK adalah kumpulan komponen **open-source** yang bekerja sama untuk mengumpulkan data dari sumber apa pun, menyimpan dan mencarinya, lalu memvisualisasikannya secara **real-time**.

### 1.2 Peran Analis SOC terhadap ELK

Tanggung jawab utama analis SOC adalah memakai ELK untuk **log analysis** dan **investigasi**. Analis tidak perlu menjadi spesialis cara kerja internal tiap komponen, tetapi wajib memahami dasarnya.

### 1.3 Tujuan Pembelajaran

- Memahami komponen ELK dan penggunaannya di SOC
- Mengeksplorasi fitur-fitur ELK
- Mencari dan memfilter data
- Menginvestigasi log VPN untuk menemukan anomali
- Membuat visualisasi dan dashboard

## 2. Komponen Elastic Stack

Empat komponen inti: **Elasticsearch**, **Logstash**, **Beats**, dan **Kibana**.

### 2.1 Elasticsearch

Mesin pencarian dan analitik **full-text** untuk dokumen berformat **JSON**. Fungsinya menyimpan, menganalisis, dan mengkorelasikan data, serta menyediakan **RESTful API** untuk berinteraksi dengannya. Dalam alur kerja, Elasticsearch berperan sebagai database tempat data dicari dan dianalisis.

### 2.2 Logstash

Mesin pemrosesan data yang mengambil data dari berbagai sumber, memfilter atau menormalisasinya, lalu mengirimkannya ke tujuan (Kibana atau sebuah listening port). Logstash juga mem-parsing log menjadi pasangan **field-value** sebelum disimpan ke Elasticsearch.

File konfigurasi Logstash terdiri dari tiga bagian:

- **Input**: mendefinisikan sumber data yang di-ingest.
- **Filter**: menentukan opsi filter untuk menormalisasi log yang masuk.
- **Output**: menentukan tujuan data yang sudah difilter, yaitu listening port, antarmuka Kibana, database Elasticsearch, atau file.

Struktur dasarnya:

```
input {
}
filter {
}
output {
}
```

Logstash mendukung banyak plugin Input, Filter, dan Output.

### 2.3 Beats

**Beats** adalah agen berbasis host (**data shipper**) yang mengirim data dari endpoint ke Elasticsearch. Setiap beat adalah agen single-purpose yang mengirim satu jenis data tertentu.

- **Filebeat**: shipper ringan untuk log dan data file lainnya
- **Metricbeat**: shipper ringan untuk data metrik
- **Packetbeat**: shipper ringan untuk data jaringan (flow trafik)
- **Winlogbeat**: shipper ringan untuk event log Windows
- **Auditbeat**: shipper ringan untuk data audit
- **Heartbeat**: shipper ringan untuk monitoring uptime
- **Functionbeat**: shipper serverless untuk data cloud

### 2.4 Kibana

Alat visualisasi data berbasis web yang bekerja dengan Elasticsearch untuk menganalisis, menginvestigasi, dan memvisualisasikan aliran data secara real-time. Kibana memungkinkan pembuatan berbagai visualisasi dan dashboard, dan menjadi komponen front-end yang dioperasikan analis SOC.

### 2.5 Alur Kerja Bersama

Urutan alur data:

1. **Beats** (Data Collection Agent) mengumpulkan data dari berbagai agen. Contoh: Winlogbeat untuk event log Windows, Packetbeat untuk flow trafik jaringan.
2. **Logstash** (Data Input / Filter / Process and Output) mengumpulkan data dari beats, port, atau file, menormalisasinya menjadi pasangan field-value, lalu menyimpannya ke Elasticsearch.
3. **Elasticsearch** (Index and Store Data) menyimpan dan mengindeks data untuk pencarian dan analisis.
4. **Kibana** (Analysis and Visualization) menampilkan data dalam bentuk visualisasi, time chart, infografis, dan sejenisnya. Hubungan Elasticsearch dan Kibana bersifat dua arah (Kibana query ke Elasticsearch dan menampilkan hasilnya).

## 3. Lingkungan Lab

Room memerlukan dua mesin: **AttackBox** (jika tidak memakai VPN sendiri) dan **Lab Machine**. Lab Machine dinyalakan dengan tombol **Start Lab Machine**.

Elastic membutuhkan waktu sekitar **lima menit** untuk siap. Dashboard Kibana diakses lewat URL lab yang disediakan room, dengan kredensial yang tertera di materi (username `Analyst`).

Dataset lab adalah log VPN pada index pattern `vpn_connections`, dengan data berkisar sekitar Januari 2022 (total 2.861 hits pada rentang yang mencakup seluruh data).

## 4. Tab Discover

Tab **Discover** adalah tempat analis SOC menghabiskan sebagian besar waktunya. Di sini analis melihat log yang sudah di-ingest, search bar, dan field yang sudah dinormalisasi; mencari log, menginvestigasi anomali, dan menerapkan filter berdasarkan search term dan periode waktu.

### 4.1 Elemen Antarmuka Discover

- **Logs**: tiap baris adalah satu log berisi informasi event beserta field dan value-nya.
- **Fields Pane**: panel kiri berisi daftar field hasil parsing; klik field untuk menambah atau menghapusnya dari filter pencarian.
- **Index Pattern**: tiap jenis log disimpan dalam index pattern berbeda; dipilih sesuai sumber log yang dibutuhkan (misal log VPN).
- **Search Bar**: tempat query pencarian dan filter untuk mempersempit hasil.
- **Time Filter**: mempersempit hasil berdasarkan durasi waktu tertentu.
- **Time Interval**: chart yang menampilkan jumlah event sepanjang waktu.
- **TOP Bar**: opsi untuk menyimpan pencarian, membuka saved search, share, dan lainnya.
- **Discover Tab**: workspace utama Kibana untuk eksplorasi, pencarian, dan analisis raw data.
- **Add Filter**: menerapkan filter ke field tertentu tanpa mengetik query lengkap.

### 4.2 Index Pattern

Secara default Kibana memerlukan **index pattern** untuk mengakses data di Elasticsearch. Index pattern memberi tahu Kibana data Elasticsearch mana yang akan dieksplorasi, dan berkorespondensi dengan properti field tertentu. Satu index pattern dapat menunjuk ke beberapa index.

Setiap sumber log memiliki struktur berbeda, sehingga saat di-ingest log dinormalisasi menjadi field dan value melalui index pattern khusus untuk sumber tersebut. Pada lab, index pattern yang dipakai adalah `vpn_connections` (log VPN).

### 4.3 Fields Pane

Klik sebuah field untuk melihat **top 5 values** beserta persentase kemunculannya. Nilai-nilai ini dapat dipakai sebagai filter:

- Tombol **+** menambah filter untuk menampilkan log yang **mengandung** value tersebut.
- Tombol **-** menambah filter untuk menampilkan log yang **tidak mengandung** value tersebut.

Top values dihitung dari sampel record yang ditampilkan (misalnya 500 record), bukan selalu seluruh hasil. Untuk agregasi seluruh data, gunakan tombol **Visualize**.

### 4.4 Add Filter

Opsi **Add filter** di bawah search bar menerapkan filter pada field tertentu melalui form: **Field**, **Operator** (misalnya `is`), dan **Value**.

### 4.5 Time Filter

Time filter mempunyai tiga mode: **Absolute** (tanggal dan jam pasti), **Relative** (relatif terhadap waktu sekarang), dan **Now**. Rentang dapat berupa preset (misalnya Last 15 years) atau tanggal absolut.

Jika Discover menampilkan "No results", penyebab umum adalah rentang waktu default (Last 15 minutes) yang tidak mencakup waktu data (Januari 2022). Perluas rentang waktu sebelum menganalisis.

### 4.6 Timeline

Timeline menampilkan jumlah event per periode waktu. Bar tertentu dapat dipilih untuk menampilkan log pada periode itu. Angka hits di kiri atas menunjukkan jumlah event pada rentang yang dipilih. Timeline berguna untuk mengidentifikasi **spike** (lonjakan tidak wajar) dalam log, misalnya lonjakan pada 11 Januari 2022.

### 4.7 Create Table

Secara default log tampil dalam bentuk raw. Dengan memilih field penting sebagai kolom, tampilan menjadi tabel yang mengurangi noise dan lebih mudah dibaca. Format tabel dapat **disimpan** (Save), sehingga field yang sama muncul setiap kali pengguna membuka dashboard.

## 5. KQL (Kibana Query Language)

**KQL** adalah bahasa query untuk mencari log atau dokumen yang di-ingest di Elasticsearch, dipakai di search bar. Terdapat dua cara pencarian: **free text search** dan **field-based search**.

### 5.1 Free Text Search

Mencari berdasarkan teks saja, di field mana pun.

```
"United States"
```

Mengembalikan semua log yang mengandung term tersebut di field apa pun. KQL mencocokkan **kata utuh**, sehingga pencarian `United` saja tidak mengembalikan hasil.

### 5.2 Wildcard

Tanda `*` mencocokkan sebagian kata.

```
United*
```

Mengembalikan semua hasil yang mengandung kata berawalan "United" dan kelanjutannya (termasuk misalnya "United Nations" jika ada).

### 5.3 Operator Logika

Tiga operator: **AND**, **OR**, **NOT**.

```
"United States" AND "Virginia"
```

AND: log harus mengandung kedua term.

```
"United States" OR "England"
```

OR: log mengandung salah satu term.

```
"United States" AND NOT ("Florida")
```

NOT: menyingkirkan term tertentu dari hasil (di sini log United States kecuali Florida).

### 5.4 Field-based Search

Sintaks: `Field: Value`, tanda titik dua memisahkan field dan value.

```
Source_ip : 238.163.231.224 AND UserName : Suleman
```

Menampilkan log dengan field `Source_ip` bernilai `238.163.231.224` dan `UserName` bernilai `Suleman`. Saat search bar diklik, Kibana menampilkan daftar field yang tersedia.

### 5.5 Catatan Penting Sintaks

- Tanda kutip menjadikan isinya **frasa literal**. `UserName : "James OR Albert"` mencari nama persis "James OR Albert", bukan James atau Albert.
- Untuk dua nilai pada field yang sama, tulis field pada tiap sisi: `UserName : "James" OR UserName : "Albert"`.
- **AND** berprioritas lebih tinggi dari **OR**. Gunakan **tanda kurung** agar maksud jelas:

```
Source_Country : "United States" AND (UserName : "James" OR UserName : "Albert")
```

- Term tanpa nama field (misal `OR "Albert"` tanpa `UserName :`) menjadi free text search ke semua field.
- KQL mendukung operator perbandingan `>`, `>=`, `<`, `<=` untuk field angka atau tanggal, misalnya:

```
UserName : "Johny Brown" AND EventTime > "2022-01-01"
```

Filter tanggal juga dapat dilakukan lewat Time filter; keduanya setara hasilnya, tetapi rentang Time filter harus tetap mencakup data.

## 6. Visualisasi

Tab visualisasi menampilkan data dalam bentuk tabel, pie chart, bar chart, dan lainnya.

### 6.1 Membuat Visualisasi

Salah satu cara masuk: klik sebuah field di Discover lalu pilih **Visualize**. Editor yang dipakai adalah **Lens**, tempat jenis visualisasi (donut, tabel, dan lain-lain) dapat dipilih.

### 6.2 Correlation Option

Untuk menghubungkan beberapa field, seret field tambahan ke area tengah visualisasi. Contoh: menambahkan **Source_Country** sebagai field kedua pada **Source_IP** menghasilkan donut dua lapis yang menunjukkan korelasi IP klien dengan negara asalnya.

### 6.3 Visualisasi Tabel

Visualisasi dapat berbentuk **Table** dengan kolom seperti Top values of Source_ip, Top values of Source_Country, dan Count of records.

### 6.4 Menyimpan Visualisasi

Langkah terpenting adalah menyimpan:

1. Klik **Save** di pojok kanan atas.
2. Isi **Title** dan **Description** (opsional Tags; opsi dashboard: Existing, New, None).
3. Klik **Save and add to library**.

### 6.5 Failed Connection Attempts

Untuk menampilkan percobaan koneksi VPN gagal: gunakan data view `vpn_connections`, atur time picker agar mencakup Januari 2022, lalu filter dengan query `action: failed`. Event failed tidak boleh dikecualikan. Gunakan **UserName** dan **Source_ip** sebagai kolom tabel untuk melihat user dan IP yang terlibat.

## 7. Dashboard

**Dashboard** menggabungkan saved search dan visualisasi menjadi satu tampilan untuk visibilitas log, dalam materi ini untuk log VPN. Seorang pengguna dapat membuat banyak dashboard sesuai kebutuhan.

Langkah membuat dashboard custom:

1. Buka **Dashboard tab**, klik **Create dashboard** (**Create new dashboard**).
2. Klik **Add from Library**.
3. Klik visualisasi dan saved search yang ingin ditambahkan.
4. Sesuaikan ukuran dan posisi tiap item.
5. **Simpan** dashboard setelah selesai.

## 8. Kesimpulan Materi

ELK bukan SIEM tradisional tetapi luas dipakai tim SOC sebagai solusi SIEM. Komponennya mencakup pengumpulan, parsing, pencarian, dan penampilan log dalam jumlah besar. Dengan ELK, analis dapat menginvestigasi log VPN, serta membuat visualisasi dan dashboard yang menjadi **single pane of glass** untuk mendeteksi pola berbahaya.

## 9. Glossary

- **ELK / Elastic Stack**: kumpulan komponen open-source untuk mengumpulkan, menyimpan, mencari, dan memvisualisasikan data.
- **Elasticsearch**: mesin pencarian dan analitik full-text untuk dokumen JSON, dengan RESTful API.
- **Logstash**: mesin pemrosesan data dengan bagian Input, Filter, Output.
- **Beats**: agen data shipper berbasis host, single-purpose.
- **Kibana**: antarmuka web untuk analisis dan visualisasi data.
- **Data shipper**: agen yang mengirim data dari endpoint ke Elasticsearch.
- **Index pattern**: definisi yang memberi tahu Kibana data Elasticsearch mana yang dieksplorasi.
- **Field normalization**: pengubahan log mentah menjadi pasangan field-value terstruktur.
- **KQL**: Kibana Query Language.
- **Free text search**: pencarian berdasarkan teks tanpa menyebut field.
- **Field-based search**: pencarian dengan sintaks `Field: Value`.
- **Wildcard (`*`)**: pencocokan sebagian kata.
- **Hits**: jumlah dokumen atau log yang cocok dengan pencarian.
- **Spike**: lonjakan jumlah event yang tidak wajar pada timeline.
- **Lens**: editor visualisasi Kibana.
- **Dashboard**: kumpulan saved search dan visualisasi dalam satu tampilan.
- **Single pane of glass**: satu tampilan terpadu untuk memantau dan mendeteksi pola berbahaya.
- **SOC**: Security Operations Center.
- **SIEM**: sistem pengelolaan informasi dan event keamanan.

## 10. Tools dan Platform Rujukan

- **Elasticsearch**: menyimpan dan mencari data log (JSON).
- **Logstash**: memproses, memfilter, dan menormalisasi data sebelum disimpan.
- **Beats (Filebeat, Metricbeat, Packetbeat, Winlogbeat, Auditbeat, Heartbeat, Functionbeat)**: mengirim data dari endpoint.
- **Kibana**: menganalisis, memvisualisasikan, dan membuat dashboard.
- **Kibana Query Language (KQL)**: bahasa query pencarian di Kibana; materi menautkan dokumentasinya tanpa URL yang terlihat jelas.
- **AttackBox**: mesin virtual penyerang yang disediakan platform room.
- **Lab Machine**: mesin lab berisi Elastic dan data log VPN; diakses lewat URL lab yang disediakan room.

## 11. Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar

- ELK — Elasticsearch, Logstash, Beats, Kibana
- Awal ELK — simpan, cari, visualisasi data besar
- ELK vs SIEM — bukan SIEM tradisional, dipakai SOC seperti SIEM
- Analis SOC — fokus log analysis dan investigasi di ELK

### Komponen

- Elasticsearch — search dan analytics engine, JSON, RESTful API, database
- Logstash — proses data: ambil, filter/normalisasi, kirim
- Logstash Input — sumber data
- Logstash Filter — normalisasi log
- Logstash Output — tujuan: port, Kibana, Elasticsearch, file
- Beats — agen data shipper, single-purpose
- Filebeat — log dan file
- Metricbeat — metrik
- Packetbeat — data jaringan
- Winlogbeat — event log Windows
- Auditbeat — data audit
- Heartbeat — uptime
- Functionbeat — serverless/cloud
- Kibana — visualisasi web, dashboard

### Alur

- Beats, lalu Logstash, lalu Elasticsearch, lalu Kibana
- Beats — kumpul data
- Logstash — proses dan field-value
- Elasticsearch — index dan simpan
- Kibana — analisis dan visual

### Lab

- AttackBox — mesin penyerang (jika tanpa VPN)
- Start Lab Machine — nyalakan lab
- Tunggu 5 menit — Elastic startup
- Index pattern lab — vpn_connections (log VPN)
- No results — cek time range (default Last 15 minutes)

### Discover

- Logs — satu baris satu event
- Fields Pane — daftar field, klik untuk filter
- Index Pattern — pilih jenis log
- Search Bar — query dan filter
- Time Filter — Absolute / Relative / Now
- Time Interval/Timeline — event per waktu, cari spike
- TOP Bar — save, open, share
- Add Filter — Field + Operator + Value
- Top 5 values — persentase kemunculan (sampel)
- Tombol + — include value
- Tombol - — exclude value
- Visualize — agregasi seluruh data
- Create Table — pilih kolom, simpan dengan Save

### KQL

- KQL — bahasa query Kibana
- Free text — cari teks di semua field
- Field-based — Field: Value
- Kata utuh — "United" tidak cocok "United States"
- Wildcard — United* cocok awalan
- AND — dua syarat harus ada
- OR — salah satu cukup
- NOT — singkirkan term
- Tanda kutip — frasa literal
- Kurung — atur prioritas (AND di atas OR)
- Perbandingan — > >= < <= untuk angka/tanggal

Query contoh:

- "United States" — free text
- United* — wildcard
- "United States" AND "Virginia" — AND
- "United States" OR "England" — OR
- "United States" AND NOT ("Florida") — NOT
- Source_ip : 238.163.231.224 AND UserName : Suleman — field-based
- Source_Country : "United States" AND (UserName : "James" OR UserName : "Albert") — AND dengan OR
- UserName : "Johny Brown" AND EventTime > "2022-01-01" — perbandingan tanggal
- action: failed — percobaan gagal

### Visualisasi dan Dashboard

- Lens — editor visualisasi
- Masuk visualisasi — klik field di Discover, pilih Visualize
- Correlation — seret field kedua ke tengah (contoh Source_IP + Source_Country)
- Table — kolom top values + Count of records
- Save visualisasi — Save, isi Title dan Description, Save and add to library
- Failed attempts — vpn_connections, Januari 2022, action: failed, kolom UserName dan Source_ip
- Dashboard — gabungan saved search dan visualisasi
- Buat dashboard — Dashboard tab, Create new dashboard
- Add from Library — tambah item, atur posisi, Save
- Single pane of glass — satu tampilan untuk deteksi pola berbahaya
