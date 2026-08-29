Computer Types — Resume Materi
[29 Agustus 2026]
TryHackMe / Room: Computer Types (Sophia's Summer of Hidden Computers)


1. Konsep Dasar

1.1 Apa Itu Komputer

Komputer adalah mesin yang memproses informasi berdasarkan instruksi. Definisi ini
lebih luas dari yang kebanyakan orang bayangkan — komputer tidak terbatas pada
laptop dan ponsel, melainkan hadir di dalam benda-benda sehari-hari seperti pintu
otomatis, kulkas, lampu, dan mesin kopi.

1.2 Prinsip Fundamental

Setiap desain komputer adalah trade-off. Tidak ada satu komputer yang bisa
dioptimalkan sekaligus untuk semua kebutuhan. Yang menentukan bentuk dan
kemampuan sebuah komputer adalah tujuan penggunaannya — bukan ukurannya,
bukan harganya.

    "There is no best computer. There is only the right tool for the job."

Tiga trade-off utama yang selalu ada:

- Mobilitas vs Performa — semakin kecil dan portabel, semakin terbatas kemampuan
  mempertahankan performa dalam waktu lama (heat management menjadi kendala
  di bodi kecil berbasis baterai)

- Keandalan vs Biaya — sistem yang dirancang untuk tidak pernah mati membutuhkan
  komponen redundan berlapis, yang berarti biaya jauh lebih tinggi

- Cara Interaksi menentukan Desain — ponsel dirancang untuk disentuh langsung,
  server dirancang untuk menerima permintaan dari jaringan, IoT device dirancang
  untuk bekerja tanpa perlu perhatian manusia


2. Klasifikasi Komputer

2.1 Komputer yang Dioperasikan Langsung (Direct-Use Computers)

Keempat jenis ini memiliki kesamaan: digunakan oleh manusia yang berinteraksi
langsung dengannya. Tiga di antaranya memiliki layar dan keyboard; satu tidak.

| Jenis         | Layar & Keyboard | Tujuan Utama                                     |
|---------------|------------------|--------------------------------------------------|
| Laptop        | Ada              | Komputasi sehari-hari yang portabel              |
| Desktop       | Ada              | Performa stabil di lokasi tetap                  |
| Workstation   | Ada              | Presisi dan keandalan untuk tugas profesional    |
| Server        | Tidak ada        | Melayani banyak pengguna melalui jaringan        |

Penjelasan masing-masing:

**Laptop**
Dirancang untuk portabilitas — ringan, berbasis baterai, mudah dibawa. Konsekuensinya,
menjaga suhu tetap rendah di dalam bodi kecil adalah tantangan, sehingga performa
akan menurun saat menjalankan beban kerja berat dalam waktu lama.

**Desktop**
Menggunakan listrik langsung dari stopkontak dan memiliki ruang lebih untuk
sistem pendingin. Akibatnya, performa lebih konsisten dan stabil untuk durasi panjang.
Tidak dirancang untuk berpindah tempat.

**Workstation**
Secara visual mirip desktop, tetapi dibangun dengan komponen khusus yang
memprioritaskan akurasi dan keandalan — bukan sekadar kecepatan. Digunakan untuk
pekerjaan seperti simulasi ilmiah, rendering 3D, dan komputasi kompleks, di mana
satu kesalahan kalkulasi bisa merusak keseluruhan hasil kerja.

**Server**
Tidak memiliki layar atau keyboard karena tidak dimaksudkan untuk dioperasikan
langsung oleh manusia. Server berjalan 24/7 dan merespons permintaan dari banyak
pengguna secara bersamaan melalui jaringan. Manusia berinteraksi dengan hasil
kerja server, bukan dengan server itu sendiri.


2.2 Komputer Tersembunyi (Hidden Computers)

Jenis-jenis ini jarang disadari sebagai "komputer" karena tidak tampak seperti
komputer pada umumnya.

**Smartphone**
Komputer berukuran saku yang dioptimalkan untuk daya tahan baterai dan konektivitas.
Contoh: iPhone, Android phone.

**Tablet**
Komputer yang mengutamakan interaksi layar sentuh dengan layar yang lebih besar
dari smartphone. Contoh: iPad, drawing tablet.

**IoT Device (Internet of Things)**
Perangkat dengan satu fungsi khusus yang terhubung ke jaringan untuk melaporkan
data atau menerima perintah dari luar. Contoh: thermostat, smart doorbell,
fitness tracker.

**Embedded Computer**
Komputer yang ditanam di dalam perangkat lain dan bekerja secara otonom.
Tidak harus terhubung ke jaringan apapun — tugasnya adalah menjalankan satu
instruksi spesifik di dalam mesin tempat ia berada, sering kali selama bertahun-tahun
tanpa ada yang menyadari keberadaannya. Contoh: controller mesin kopi, sensor
pintu otomatis, chip dimmer lampu.


3. Perbedaan Kritis: IoT vs Embedded Computer

Keduanya bisa berukuran kecil dan memiliki satu fungsi saja, sehingga sering
dikira sama. Perbedaannya ada pada satu hal: **konektivitas**.

| Aspek              | IoT Device                          | Embedded Computer                    |
|--------------------|-------------------------------------|--------------------------------------|
| Koneksi jaringan   | Selalu terhubung ke jaringan        | Tidak harus terhubung ke apapun      |
| Cara kerja         | Kirim data / terima perintah        | Bekerja sendiri di dalam mesin       |
| Contoh             | Smart thermostat, fitness tracker   | Sensor pintu otomatis, chip dimmer   |
| Kesadaran publik   | Lebih terlihat (ada aplikasinya)    | Tidak terlihat, tidak ada antarmuka  |

Ilustrasi: Pintu otomatis di sebuah gedung menggunakan embedded computer —
komputer kecil di dalam rangka pintu yang mendeteksi gerakan dan memberi sinyal
ke motor untuk membuka. Ia tidak terhubung ke internet, tidak punya aplikasi,
dan tidak ada yang tahu ia ada. Itulah embedded computing: invisible, reliable,
everywhere.


4. Konsep Keandalan Server: Redundansi dan Uptime

4.1 Masalah yang Harus Dipecahkan

Server harus beroperasi 24/7 tanpa henti. Artinya, satu kerusakan komponen
tunggal tidak boleh langsung mematikan seluruh sistem. Konsep yang menyelesaikan
masalah ini adalah **redundansi**.

4.2 Redundansi

Redundansi adalah strategi membangun sistem dengan komponen cadangan berlapis,
sehingga tidak ada satu titik kegagalan (**single failure point**) yang bisa
menjatuhkan keseluruhan sistem.

Pada server, redundansi diterapkan pada power supply:

| Kondisi                               | Status Server |
|---------------------------------------|---------------|
| Power A terhubung, Power B terhubung  | ONLINE        |
| Power A terputus, Power B terhubung   | ONLINE        |
| Power A terputus, Power B terputus    | OFFLINE       |

Ketika satu power supply gagal, server tetap berjalan dari yang lain.
Sistem baru benar-benar mati hanya jika semua sumber daya sekaligus gagal.

4.3 Uptime

**Uptime** adalah ukuran seberapa lama sistem tetap aktif dan bisa diakses.
Uptime meningkat secara signifikan ketika redundansi dikombinasikan dengan:
- **Backup** — cadangan data secara berkala
- **Monitoring** — pemantauan sistem secara real-time untuk deteksi dini masalah


5. Panduan Cepat: Pilih Komputer yang Tepat

- Butuh dibawa bepergian setiap hari         → Laptop / Smartphone
- Kerja intensif di satu tempat              → Desktop
- Komputasi berat: simulasi, 3D, riset       → Workstation
- Layani ratusan/ribuan user secara bersamaan→ Server
- Monitor kondisi lingkungan via aplikasi    → IoT Device
- Komponen internal mesin yang bekerja sendiri → Embedded Computer
- Layar sentuh besar untuk kreativitas       → Tablet


6. Glossary — Istilah Wajib Hapal

**Trade-off**
Kompromi desain — mendapatkan keunggulan di satu aspek berarti mengorbankan
aspek lain. Konsep ini menjelaskan mengapa tidak ada "komputer serba bisa."

**Portability**
Kemampuan untuk dibawa berpindah tempat. Meningkatkan portabilitas biasanya
menurunkan performa sustained.

**Sustained Performance**
Kemampuan mempertahankan performa tinggi dalam durasi panjang, bukan sekadar
performa puncak sesaat.

**Redundancy**
Duplikasi komponen kritis sebagai cadangan, agar kegagalan satu komponen
tidak mematikan keseluruhan sistem.

**Single Failure Point**
Satu komponen yang, jika rusak, dapat menjatuhkan seluruh sistem. Redundansi
dirancang untuk mengeliminasi ini.

**Uptime**
Durasi sistem tetap aktif dan dapat diakses. Dinyatakan dalam persentase;
semakin tinggi semakin baik (contoh: 99.9% uptime).

**IoT (Internet of Things)**
Kategori perangkat dengan satu fungsi khusus yang terhubung ke jaringan
untuk bertukar data atau menerima perintah.

**Embedded Computer**
Komputer yang tertanam di dalam perangkat lain, bekerja otonom menjalankan
satu instruksi spesifik, dan tidak harus terhubung ke jaringan apapun.

**Connectivity**
Kemampuan suatu perangkat untuk terhubung ke jaringan atau internet.
Ini adalah pembeda utama antara IoT device dan embedded computer.

**Workstation**
Komputer kelas profesional dengan komponen khusus yang mengutamakan akurasi
dan keandalan untuk komputasi kompleks, bukan sekadar kecepatan umum.


[ Tidak ada tools atau platform eksternal yang direferensikan dalam room ini. ]