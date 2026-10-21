# 01 — Inside a Computer System
**TryHackMe | Computer Types (Sophia's Summer of Hidden Computers)**
*21 Oktober 2026*

---

## 1. Konsep Dasar

### 1.1 Apa Itu Komputer

Komputer adalah mesin yang memproses informasi berdasarkan instruksi. Definisi ini lebih luas dari yang kebanyakan orang bayangkan — komputer tidak terbatas pada laptop dan ponsel, melainkan hadir di dalam benda-benda sehari-hari seperti pintu otomatis, kulkas, lampu, dan mesin kopi.

Dua pelajaran utama yang perlu dipahami sejak awal:

- Tidak semua komputer dirancang untuk berpindah-pindah.
- Tidak semua komputer dirancang untuk dioperasikan langsung oleh manusia.

### 1.2 Prinsip Fundamental

Setiap desain komputer adalah **trade-off**. Tidak ada satu komputer yang bisa dioptimalkan sekaligus untuk semua kebutuhan. Yang menentukan bentuk dan kemampuan sebuah komputer adalah tujuan penggunaannya — bukan ukurannya, bukan harganya.

> "There is no best computer. There is only the right tool for the job."

Tiga trade-off utama yang selalu ada dalam desain komputer:

**Mobilitas vs Performa**
Semakin kecil dan portabel sebuah komputer, semakin sulit mempertahankan performa dalam waktu lama. Heat management menjadi kendala utama di bodi kecil berbasis baterai.

**Keandalan vs Biaya**
Sistem yang dirancang untuk tidak pernah mati membutuhkan komponen redundan berlapis. Tingkat keandalan yang lebih tinggi selalu berarti biaya yang lebih besar.

**Tujuan Menentukan Desain**
Ponsel dirancang untuk disentuh langsung. Server dirancang untuk menerima permintaan dari jaringan. IoT device dirancang untuk bekerja tanpa perhatian manusia sama sekali.

---

## 2. Klasifikasi Komputer

### 2.1 Komputer yang Dioperasikan Langsung

Keempat jenis ini digunakan oleh manusia yang berinteraksi langsung dengannya. Tiga memiliki layar dan keyboard; satu tidak.

| Jenis | Layar & Keyboard | Tujuan Utama |
|---|---|---|
| Laptop | Ada | Komputasi sehari-hari yang portabel |
| Desktop | Ada | Performa stabil di lokasi tetap |
| Workstation | Ada | Presisi dan keandalan untuk tugas profesional |
| Server | Tidak ada | Melayani banyak pengguna melalui jaringan |

**Laptop**
Dirancang untuk portabilitas — ringan, berbasis baterai, mudah dibawa. Konsekuensinya, menjaga suhu tetap rendah di bodi kecil adalah tantangan besar, sehingga performa akan menurun saat menjalankan beban kerja berat dalam durasi panjang.

**Desktop**
Menggunakan listrik langsung dari stopkontak dan memiliki ruang lebih besar untuk sistem pendingin. Performa lebih konsisten dan stabil untuk durasi panjang dibanding laptop. Tidak dirancang untuk berpindah tempat.

**Workstation**
Secara visual mirip desktop, tetapi dibangun dengan komponen khusus yang memprioritaskan akurasi dan keandalan — bukan sekadar kecepatan. Digunakan untuk simulasi ilmiah, rendering 3D, dan komputasi kompleks, di mana satu kesalahan kalkulasi bisa merusak keseluruhan hasil kerja.

**Server**
Tidak memiliki layar atau keyboard karena tidak dimaksudkan untuk dioperasikan langsung oleh manusia. Berjalan 24/7 dan merespons permintaan dari banyak pengguna secara bersamaan melalui jaringan. Manusia berinteraksi dengan hasil kerja server, bukan dengan server itu sendiri.

---

### 2.2 Komputer Tersembunyi (Hidden Computers)

Jenis-jenis ini jarang disadari sebagai "komputer" karena tidak tampak seperti komputer pada umumnya.

**Smartphone**
Komputer berukuran saku yang dioptimalkan untuk daya tahan baterai dan konektivitas. Contoh: iPhone, Android phone.

**Tablet**
Komputer yang mengutamakan interaksi layar sentuh dengan layar yang lebih besar dari smartphone. Contoh: iPad, drawing tablet.

**IoT Device (Internet of Things)**
Perangkat dengan satu fungsi khusus yang terhubung ke jaringan untuk melaporkan data atau menerima perintah dari luar. Contoh: thermostat, smart doorbell, fitness tracker.

**Embedded Computer**
Komputer yang ditanam di dalam perangkat lain dan bekerja secara otonom. Tidak harus terhubung ke jaringan apapun — tugasnya adalah menjalankan satu instruksi spesifik di dalam mesin tempat ia berada, sering kali selama bertahun-tahun tanpa ada yang menyadari keberadaannya. Contoh: controller mesin kopi, sensor pintu otomatis, chip dimmer lampu.

---

## 3. Perbedaan Kritis: IoT vs Embedded Computer

Keduanya bisa berukuran kecil dan memiliki satu fungsi saja, sehingga sering dikira sama. Perbedaannya ada pada satu hal: **konektivitas**.

| Aspek | IoT Device | Embedded Computer |
|---|---|---|
| Koneksi jaringan | Selalu terhubung | Tidak harus terhubung |
| Cara kerja | Kirim data / terima perintah | Bekerja sendiri di dalam mesin |
| Visibilitas | Ada antarmuka, biasanya lewat app | Tidak terlihat, tidak ada antarmuka |

**Ilustrasi nyata:**
Pintu otomatis di sebuah gedung menggunakan embedded computer — komputer kecil di dalam rangka pintu yang mendeteksi gerakan dan memberi sinyal ke motor untuk membuka. Ia tidak terhubung ke internet, tidak punya aplikasi, dan tidak ada yang tahu ia ada. Itulah embedded computing: *invisible, reliable, everywhere.*

---

## 4. Keandalan Server: Redundansi dan Uptime

### 4.1 Masalah yang Harus Dipecahkan

Server harus beroperasi 24/7 tanpa henti. Artinya, kerusakan satu komponen tidak boleh langsung mematikan seluruh sistem. Konsep yang menyelesaikan masalah ini adalah **redundansi**.

### 4.2 Redundansi

Redundansi adalah strategi membangun sistem dengan komponen cadangan berlapis, sehingga tidak ada **single failure point** — satu titik yang jika rusak langsung menjatuhkan keseluruhan sistem.

Pada server, redundansi paling umum diterapkan pada power supply:

| Kondisi | Status Server |
|---|---|
| Power A terhubung, Power B terhubung | ONLINE |
| Power A terputus, Power B terhubung | ONLINE |
| Power A terputus, Power B terputus | OFFLINE |

Ketika satu power supply gagal, server tetap berjalan dari yang lain. Sistem baru benar-benar mati jika semua sumber daya sekaligus gagal.

### 4.3 Uptime

**Uptime** mengukur seberapa lama sistem tetap aktif dan bisa diakses, biasanya dinyatakan dalam persentase. Uptime meningkat secara signifikan ketika redundansi dikombinasikan dengan:

- **Backup** — cadangan data secara berkala
- **Monitoring** — pemantauan sistem real-time untuk deteksi dini masalah

---

## 5. Panduan Cepat: Pilih Komputer yang Tepat

| Kebutuhan | Jenis Komputer |
|---|---|
| Dibawa bepergian setiap hari | Laptop / Smartphone |
| Kerja intensif di satu tempat | Desktop |
| Simulasi, 3D, komputasi berat | Workstation |
| Layani banyak user secara bersamaan | Server |
| Monitor kondisi lingkungan via aplikasi | IoT Device |
| Komponen internal mesin yang bekerja sendiri | Embedded Computer |
| Layar sentuh besar untuk kreativitas | Tablet |

---

## 6. Glossary

**Trade-off**
Kompromi desain — mendapatkan keunggulan di satu aspek berarti mengorbankan aspek lain. Konsep ini menjelaskan mengapa tidak ada "komputer serba bisa."

**Portability**
Kemampuan untuk dibawa berpindah tempat. Meningkatkan portabilitas biasanya menurunkan sustained performance.

**Sustained Performance**
Kemampuan mempertahankan performa tinggi dalam durasi panjang, bukan sekadar performa puncak sesaat.

**Redundancy**
Duplikasi komponen kritis sebagai cadangan, agar kegagalan satu komponen tidak mematikan keseluruhan sistem.

**Single Failure Point**
Satu komponen yang, jika rusak, dapat menjatuhkan seluruh sistem. Redundansi dirancang untuk mengeliminasi ini.

**Uptime**
Durasi sistem tetap aktif dan dapat diakses. Semakin tinggi persentasenya, semakin andal sistemnya.

**IoT (Internet of Things)**
Kategori perangkat dengan satu fungsi khusus yang terhubung ke jaringan untuk bertukar data atau menerima perintah.

**Embedded Computer**
Komputer yang tertanam di dalam perangkat lain, bekerja otonom menjalankan satu instruksi spesifik, dan tidak harus terhubung ke jaringan apapun.

**Connectivity**
Kemampuan suatu perangkat untuk terhubung ke jaringan atau internet. Ini adalah pembeda utama antara IoT device dan embedded computer.

**Workstation**
Komputer kelas profesional dengan komponen khusus yang mengutamakan akurasi dan keandalan untuk komputasi kompleks, bukan sekadar kecepatan umum.

---

## 7. Tools & Platform Rujukan

Tidak ada tools atau platform eksternal yang direferensikan dalam room ini.

---

## 8. Catatan Ringkas untuk Ditulis Tangan

### Prinsip Inti
- Trade-off — setiap desain korbankan sesuatu untuk dapat sesuatu
- Mobilitas vs Performa — makin portabel, makin terbatas sustained-nya
- Keandalan vs Biaya — redundansi = mahal tapi tidak mati
- Tujuan → Desain — bentuk komputer ditentukan fungsinya

### Jenis Komputer (Direct-Use)
- Laptop — portabel, baterai, lemah untuk beban berat lama
- Desktop — listrik stopkontak, pendingin baik, tidak mobile
- Workstation — komponen khusus, utamakan akurasi bukan kecepatan
- Server — no screen/keyboard, 24/7, layani banyak user via jaringan

### Jenis Komputer (Hidden)
- Smartphone — komputer saku, optimalkan baterai + konektivitas
- Tablet — touch-first, layar lebih besar dari HP
- IoT Device — 1 fungsi + terhubung jaringan, kirim/terima data
- Embedded Computer — ditanam di mesin lain, tidak harus online, tak terlihat

### IoT vs Embedded
- Sama: kecil, 1 fungsi
- Beda: IoT selalu konek jaringan; Embedded tidak harus
- Contoh IoT: thermostat, smart doorbell
- Contoh Embedded: sensor pintu otomatis, chip dimmer lampu

### Redundansi & Uptime (Server)
- Redundansi — komponen cadangan berlapis, hilangkan single failure point
- Single failure point — 1 titik rusak = seluruh sistem mati
- Uptime — seberapa lama sistem aktif (%)
- Power A off + Power B on → server tetap ONLINE (redundansi bekerja)
- Power A off + Power B off → server OFFLINE
- Uptime naik jika redundansi + backup + monitoring digabung

### Pilih Komputer yang Tepat
- Dibawa tiap hari → Laptop/Smartphone
- Kerja berat di satu tempat → Desktop
- Simulasi/3D/riset → Workstation
- Layani banyak user → Server
- Monitor kondisi via app → IoT Device
- Komponen internal mesin → Embedded Computer
