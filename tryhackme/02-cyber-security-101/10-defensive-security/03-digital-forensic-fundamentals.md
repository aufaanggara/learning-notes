# Resume — Introduction to Digital Forensics (TryHackMe)
**Tanggal:** 27 Juni 2026

---

## 1. Konsep Dasar

**Forensics** adalah penerapan metode dan prosedur ilmiah untuk menyelidiki dan menyelesaikan kejahatan.

**Digital forensics** adalah cabang forensik yang secara khusus menangani investigasi kejahatan siber — mencakup penemuan, pengumpulan, analisis, dan pelaporan bukti digital secara sah untuk keperluan hukum.

**Cyber crime** adalah segala aktivitas kriminal yang dilakukan pada atau menggunakan perangkat digital. Cakupannya luas: penipuan online, pencurian data, peretasan, distribusi malware, hingga kejahatan terorganisir melalui platform digital.

Prinsip mendasar: setiap tindakan di perangkat digital meninggalkan jejak — timestamp akses file, log koneksi jaringan, metadata tersembunyi dalam dokumen dan gambar, hingga sisa data dari file yang sudah "dihapus."

---

## 2. Empat Fase Digital Forensics (Standar NIST)

**NIST (National Institute of Standards and Technology)** mendefinisikan proses standar digital forensics dalam empat fase berurutan.

### 2.1 Collection (Pengumpulan)

Mengidentifikasi dan mengamankan semua perangkat yang menjadi sumber data (PC, laptop, USB, kamera digital, ponsel, dll.). Data asli tidak boleh dimanipulasi selama proses ini. Setiap barang yang dikumpulkan harus didokumentasikan secara rinci.

### 2.2 Examination (Pemeriksaan)

Menyaring data besar yang terkumpul untuk mengekstrak hanya bagian yang relevan. Contoh: dari ribuan file media di kamera seorang tersangka, investigator hanya mengambil file yang direkam pada tanggal dan waktu tertentu, atau data dari akun pengguna tertentu di sistem dengan banyak user.

### 2.3 Analysis (Analisis)

Fase paling kritis. Data yang sudah disaring dianalisis dengan mengkorelasikannya terhadap bukti-bukti lain untuk menarik kesimpulan. Tujuannya adalah menyusun kronologi aktivitas yang relevan dengan kasus secara objektif dan terverifikasi.

### 2.4 Reporting (Pelaporan)

Laporan rinci disusun mencakup metodologi investigasi, temuan, dan rekomendasi. Laporan ini disampaikan kepada penegak hukum (law enforcement) dan manajemen eksekutif. Wajib menyertakan **executive summary** — ringkasan non-teknis untuk pihak yang tidak memiliki latar belakang teknis.

---

## 3. Jenis-Jenis Digital Forensics

**Computer forensics** adalah payung utama dari bidang ini, dengan beberapa cabang spesialisasi:

**Computer forensics** — investigasi komputer secara umum; jenis yang paling umum karena komputer adalah perangkat yang paling sering terlibat dalam kejahatan.

**Mobile forensics** — investigasi perangkat seluler; mengekstrak call records, pesan teks, lokasi GPS, dan data aplikasi dari smartphone dan tablet.

**Network forensics** — investigasi pada level jaringan, bukan hanya perangkat individu. Sebagian besar bukti ditemukan dalam bentuk **network traffic logs**.

**Database forensics** — investigasi intrusi ke dalam database yang menyebabkan modifikasi atau eksfiltrasi (pencurian) data.

**Cloud forensics** — investigasi data yang tersimpan di infrastruktur cloud. Dianggap paling menantang karena bukti yang tersedia di cloud sangat terbatas dibandingkan storage fisik.

**Email forensics** — investigasi email untuk menentukan apakah email tersebut merupakan bagian dari kampanye phishing atau penipuan.

**Memory forensics** — investigasi data yang tersimpan di RAM; relevan untuk menangkap proses yang berjalan, koneksi aktif, dan file yang sedang terbuka pada saat perangkat dianalisis.

**Malware forensics** — investigasi keberadaan dan perilaku malware dalam suatu sistem.

---

## 4. Akuisisi Bukti — Prinsip-Prinsip Kritis

### 4.1 Proper Authorization

Tim forensik wajib memperoleh izin resmi dari otoritas berwenang sebelum mengumpulkan data apa pun. Instrumen hukumnya adalah **search warrant** (surat perintah penggeledahan).

Bukti yang dikumpulkan tanpa otorisasi yang sah dinyatakan **inadmissible** — tidak dapat diterima di pengadilan — sekalipun bukti tersebut secara teknis valid dan autentik.

### 4.2 Chain of Custody

**Chain of custody** adalah dokumen formal yang mencatat seluruh riwayat sebuah barang bukti dari saat ditemukan hingga digunakan di pengadilan. Fungsinya adalah membuktikan **integrity** (keaslian) dan **reliability** (keandalan) bukti.

Isi minimum dokumen chain of custody:

- Deskripsi bukti (nama dan jenis perangkat/file)
- Nama individu yang mengumpulkan bukti
- Tanggal dan waktu pengumpulan
- Lokasi penyimpanan setiap barang bukti
- Waktu akses dan identitas siapa saja yang mengakses bukti

Tanpa chain of custody yang terjaga, tidak ada satu pun individu yang bisa dimintai pertanggungjawaban jika bukti berubah atau hilang — dan seluruh penyelidikan bisa gugur di pengadilan.

### 4.3 Write Blocker

**Write blocker** adalah perangkat keras (hardware) yang ditempatkan di antara hard drive tersangka dan workstation forensik saat proses pengumpulan data berlangsung.

Tanpa write blocker, tugas-tugas latar belakang (background tasks) pada workstation forensik — seperti sistem operasi yang secara otomatis mencatat timestamp akses file — dapat mengubah data pada hard drive tersangka tanpa disengaja. Perubahan sekecil apa pun pada timestamp atau metadata bisa membatalkan keabsahan bukti.

Write blocker memblokir semua operasi penulisan ke drive tersangka, sehingga hard drive tersebut tetap dalam **original state** yang identik dengan kondisinya saat disita.

---

## 5. Windows Forensics

### 5.1 Dua Jenis Forensic Image

Saat menganalisis sistem Windows, dua jenis **forensic image** (salinan bit-by-bit dari sistem) diambil:

**Disk image** — salinan bit-by-bit dari seluruh storage (HDD/SSD). Bersifat **non-volatile**: data tetap ada meskipun sistem dimatikan atau di-restart. Berisi file media, dokumen, riwayat browser, dan seluruh data yang tersimpan di storage.

**Memory image** — salinan isi RAM pada saat tertentu. Bersifat **volatile**: data hilang permanen begitu sistem dimatikan atau di-restart. Berisi proses yang sedang berjalan, file yang sedang terbuka, koneksi jaringan aktif, dan data sesi yang belum disimpan ke disk.

**Urutan pengambilan yang wajib diikuti:** memory image harus diambil lebih dahulu sebelum disk image — karena data RAM hilang begitu sistem dimatikan, sedangkan data di disk akan tetap ada dan bisa diambil kapan pun.

### 5.2 Tools Utama Windows Forensics

**FTK Imager** — tool berbasis GUI untuk mengakuisisi disk image dari sistem Windows dalam berbagai format. Juga bisa digunakan untuk menganalisis isi disk image yang sudah diakuisisi.

**Autopsy** — platform open-source untuk analisis disk image. Fitur yang tersedia: keyword search, deleted file recovery, pembacaan file metadata, dan extension mismatch detection (mendeteksi file yang ekstensinya tidak sesuai dengan isi sebenarnya).

**DumpIt** — tool berbasis command-line untuk mengakuisisi memory image dari sistem Windows. Mendukung berbagai format output.

**Volatility** — tool open-source untuk menganalisis memory image. Bekerja menggunakan sistem plugin: setiap jenis artefak (artifact) dianalisis menggunakan plugin yang spesifik. Mendukung Windows, Linux, macOS, dan Android.

---

## 6. Analisis Metadata — Konsep dan Praktik

### 6.1 Metadata Dokumen

Saat sebuah file dibuat — terutama dengan editor seperti MS Word — sistem operasi dan aplikasi menyimpan metadata di luar konten yang terlihat. Metadata ini bisa mencakup nama penulis (author), tanggal pembuatan, software yang digunakan, dan informasi lainnya.

Saat file dikonversi ke format lain (misalnya dari .docx ke .pdf), sebagian besar metadata dari dokumen asli ikut terbawa ke format baru — bergantung pada perangkat lunak yang digunakan untuk konversi.

**Tool:** `pdfinfo` (bagian dari paket `poppler-utils`)

```
pdfinfo <namafile>.pdf
```

Tidak ada flag tambahan yang diperlukan untuk penggunaan dasar. Perintah ini menampilkan semua metadata yang tersedia dalam file PDF, termasuk: Title, Subject, **Author**, Creator, Producer, CreationDate, ModDate, jumlah halaman, ukuran file, dan versi PDF.

Instalasi di Kali/Debian:
```
sudo apt install poppler-utils
```

### 6.2 EXIF Data pada Gambar

**EXIF (Exchangeable Image File Format)** adalah standar penyimpanan metadata pada file gambar. Setiap foto yang diambil dengan kamera digital atau smartphone secara otomatis menyematkan metadata EXIF ke dalam file gambar.

Metadata yang umum tersimpan dalam EXIF:

- Model kamera atau smartphone
- Tanggal dan waktu pengambilan gambar
- Pengaturan teknis foto: focal length, aperture, shutter speed, ISO
- **Koordinat GPS** (latitude dan longitude) — jika perangkat memiliki sensor GPS dan izin lokasi diaktifkan

Koordinat GPS dalam EXIF adalah salah satu informasi paling berharga dalam forensik, karena secara langsung menunjukkan lokasi fisik di mana foto diambil.

**Tool:** `exiftool` (bagian dari paket `libimage-exiftool-perl`)

```
exiftool <namafile>.jpg
```

Tidak ada flag tambahan yang diperlukan untuk penggunaan dasar. Perintah ini menampilkan seluruh metadata EXIF yang tertanam, termasuk GPS Latitude, GPS Longitude, GPS Position, Camera Model Name, Make, Lens ID, dan puluhan field lainnya.

Instalasi di Kali/Debian:
```
sudo apt install libimage-exiftool-perl
```

### 6.3 Konversi Format Koordinat GPS

Output `exiftool` untuk koordinat GPS menggunakan format DMS (Degrees, Minutes, Seconds) yang tidak langsung diterima oleh sebagian besar layanan peta:

Format mentah dari exiftool:
```
GPS Position : 51 deg 30' 51.90" N, 0 deg 5' 38.73" W
```

Format yang diterima Google Maps / Bing Maps — ganti `deg` dengan simbol derajat `°` dan hapus spasi ekstra:
```
51°30'51.9"N 0°5'38.73"W
```

---

## 7. Studi Kasus: Ransom Letter (Penculikan "Gado")

Skenario praktis yang digunakan dalam room ini: seekor kucing bernama Gado dilaporkan "diculik," dan penculik mengirimkan surat tebusan berupa file MS Word (yang dikonversi ke PDF) beserta gambar terlampir.

**Temuan dari pdfinfo pada ransom-letter.pdf:**
- Author: **Ann Gree Shepherd**
- Creator & Producer: Microsoft Word 2016
- Tanggal pembuatan: 23 Februari 2022

**Temuan dari exiftool pada letter-image.jpg:**
- Camera Make: **Canon**
- Camera Model Name: **Canon EOS R6**
- Lens ID: Canon EF 50mm f/1.8 STM
- GPS Position: 51 deg 30' 51.90" N, 0 deg 5' 38.73" W

**Geolokasi koordinat GPS:**
Koordinat dimasukkan ke Bing Maps menggunakan format `51°30'51.9"N 0°5'38.73"W` dan menghasilkan lokasi: **8 Russia Row, EC2V 8BL, London, United Kingdom** — tepat di jalan bernama **Milk Street**.

**Kesimpulan:** Dengan hanya dua perintah terminal dan satu pencarian peta, identitas penulis dokumen, perangkat yang digunakan, dan lokasi fisik pengambilan foto berhasil diidentifikasi — semua dari metadata yang tertanam tanpa disadari oleh pembuat file.

---

## 8. Glossary

**Forensics** — penerapan metode ilmiah untuk menyelidiki dan menyelesaikan kejahatan.

**Digital forensics** — cabang forensik khusus untuk investigasi kejahatan siber.

**Cyber crime** — aktivitas kriminal yang dilakukan pada atau menggunakan perangkat digital.

**NIST** — National Institute of Standards and Technology; badan standar AS yang mendefinisikan empat fase digital forensics.

**Collection, Examination, Analysis, Reporting** — empat fase standar proses digital forensics menurut NIST.

**Search warrant** — surat perintah penggeledahan; izin hukum yang wajib dimiliki sebelum mengumpulkan bukti.

**Inadmissible** — status bukti yang tidak dapat diterima di pengadilan karena prosedur pengumpulannya tidak sah.

**Chain of custody** — dokumen formal yang mencatat seluruh riwayat sebuah barang bukti.

**Integrity** — keaslian bukti; jaminan bahwa bukti tidak berubah sejak saat ditemukan.

**Reliability** — keandalan bukti; jaminan bahwa bukti dapat dipercaya sebagai representasi fakta.

**Write blocker** — perangkat keras yang mencegah operasi penulisan ke drive tersangka selama proses akuisisi.

**Original state** — kondisi data yang identik dengan kondisinya saat pertama kali diamankan.

**Disk image** — salinan bit-by-bit dari seluruh storage; bersifat non-volatile.

**Memory image** — salinan isi RAM pada waktu tertentu; bersifat volatile.

**Volatile data** — data yang hilang saat sistem dimatikan atau di-restart (contoh: isi RAM).

**Non-volatile data** — data yang tetap ada meskipun sistem dimatikan (contoh: isi hard drive).

**Forensic image** — salinan bit-by-bit dari media penyimpanan atau RAM untuk keperluan forensik.

**EXIF** — Exchangeable Image File Format; standar metadata yang tersimpan dalam file gambar.

**Artifact** — jejak atau potongan bukti digital yang ditemukan selama investigasi.

**Eksfiltrasi** — pencurian data dari suatu sistem ke pihak yang tidak berwenang.

**Extension mismatch** — kondisi di mana ekstensi sebuah file tidak sesuai dengan format sebenarnya dari isi file tersebut.

---

## 9. Tools & Platform Rujukan

**pdfinfo** — membaca dan menampilkan metadata dari file PDF. Bagian dari paket `poppler-utils`.

**exiftool** — membaca dan menulis metadata EXIF pada berbagai format file gambar (JPEG, RAW, dll.). Bagian dari paket `libimage-exiftool-perl`.

**FTK Imager** — akuisisi dan analisis disk image untuk sistem Windows; berbasis GUI.

**Autopsy** — platform open-source untuk analisis disk image; mendukung keyword search, file recovery, dan metadata analysis. URL: https://www.autopsy.com

**DumpIt** — akuisisi memory image dari sistem Windows via command-line.

**Volatility** — analisis memory image berbasis plugin; mendukung berbagai sistem operasi. URL: https://volatilityfoundation.org

**Google Maps** — geolokasi koordinat GPS dari metadata EXIF. URL: https://maps.google.com

**Microsoft Bing Maps** — alternatif Google Maps untuk geolokasi koordinat GPS. URL: https://www.bing.com/maps

---

## 10. Catatan Ringkas untuk Ditulis Tangan

### Konsep Dasar
- Forensics — metode ilmiah untuk selesaikan kejahatan
- Digital forensics — forensics khusus untuk cyber crime
- Cyber crime — kejahatan yang dilakukan pada/via perangkat digital
- Jejak digital — setiap tindakan di perangkat digital meninggalkan trace

### 4 Fase NIST
- Collection — kumpulkan semua perangkat, jangan tamper data asli
- Examination — saring data besar, ambil yang relevan saja
- Analysis — korelasikan bukti, susun kronologi
- Reporting — laporan rinci + executive summary untuk non-teknis

### Jenis-Jenis Digital Forensics
- Computer — investigasi komputer (paling umum)
- Mobile — call records, SMS, GPS dari HP
- Network — investigasi jaringan, bukti = network traffic logs
- Database — intrusi ke DB, modifikasi/eksfiltrasi data
- Cloud — sulit, bukti sangat terbatas
- Email — deteksi phishing/fraud campaigns
- Memory — analisis RAM (volatile)
- Malware — analisis malware di sistem

### Akuisisi Bukti
- Search warrant — wajib, tanpa ini bukti inadmissible
- Chain of custody — dokumen riwayat bukti (siapa, kapan, di mana)
- Write blocker — hardware, blokir penulisan ke drive tersangka → jaga original state

### Windows Forensics
- Disk image — copy bit-by-bit storage, non-volatile, ambil kedua
- Memory image — copy RAM, volatile, HARUS ambil PERTAMA sebelum mati
- FTK Imager — akuisisi disk image (GUI)
- Autopsy — analisis disk image (open-source)
- DumpIt — akuisisi memory image (CLI)
- Volatility — analisis memory image, pakai plugin per artifact

### Commands
- pdfinfo namafile.pdf — baca semua metadata PDF (author, creator, tanggal, dll)
- exiftool namafile.jpg — baca semua EXIF metadata gambar (GPS, kamera, tanggal)
- sudo apt install poppler-utils — install pdfinfo di Kali/Debian
- sudo apt install libimage-exiftool-perl — install exiftool di Kali/Debian
- sudo apt update — sebelum install, update dulu kalau ada error 404

### Konversi GPS
- Format mentah exiftool: 51 deg 30' 51.90" N
- Format Maps: 51°30'51.9"N 0°5'38.73"W (ganti deg → °, hapus spasi ekstra)

### Glossary Cepat
- Inadmissible — bukti tidak sah di pengadilan
- Integrity — keaslian bukti tidak berubah
- Reliability — bukti dapat dipercaya sebagai fakta
- Volatile — hilang saat sistem mati (RAM)
- Non-volatile — tetap ada meski sistem mati (HDD/SSD)
- Artifact — jejak/potongan bukti digital
- Eksfiltrasi — pencurian data keluar sistem
- EXIF — metadata standar dalam file gambar
- Extension mismatch — ekstensi file tidak sesuai isi sebenarnya

