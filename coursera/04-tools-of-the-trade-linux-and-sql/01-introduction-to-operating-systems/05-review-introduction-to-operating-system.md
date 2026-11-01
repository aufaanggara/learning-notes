# Resume: Computer Basics & Operating Systems (Module 1)

**Course:** Google Cybersecurity Certificate — Course 4, Module 1
**Tanggal:** 20 September 2026

---

## 1. Konsep Inti: Operating System

### 1.1 Definisi

**Operating system (OS)** adalah antarmuka yang menghubungkan hardware komputer dengan pengguna. OS bertanggung jawab membuat komputer berjalan secara efisien dan memungkinkan banyak aplikasi berjalan dalam waktu bersamaan (bukan hanya satu aplikasi per waktu).

Setiap perangkat komputasi — komputer, smartphone, maupun tablet — memiliki sistem operasi masing-masing. Tanpa OS, hardware tidak bisa langsung dipakai oleh pengguna atau aplikasi.

### 1.2 Fungsi Utama OS

Fungsi utama OS mencakup dua hal berikut:

- Menjadi jembatan komunikasi antara aplikasi, pengguna, dan hardware.
- Melakukan **resource allocation** — memastikan kapasitas terbatas dari sistem komputer (CPU, memori, dsb.) digunakan di tempat yang paling membutuhkan pada saat itu, sehingga komputer tetap efisien meski menjalankan banyak proses sekaligus.

### 1.3 Jenis Operating System

Berdasarkan platform:

- **Desktop/Laptop:** Linux, macOS.
- **Mobile:** Android, iOS.

**Legacy operating system** adalah sistem operasi yang sudah ketinggalan zaman namun masih digunakan di sejumlah sistem, biasanya karena alasan kompatibilitas atau ketergantungan sistem lama.

---

## 2. Alur Kerja: OS, Aplikasi, dan Hardware

### 2.1 Relasi Tiga Komponen

OS, aplikasi, dan hardware bekerja sebagai satu kesatuan yang saling bergantung. Hubungan ini bisa dianalogikan sebagai berikut:

> Ketiganya bekerja seperti sebuah orkestra — OS berperan sebagai konduktor yang mengoordinasikan aplikasi (pemain musik) dan hardware (alat musik) agar menghasilkan hasil yang selaras.

### 2.2 Alur Request dari Aplikasi ke Hardware

Ketika pengguna menjalankan aplikasi, terjadi alur komunikasi tiga tahap:

1. Aplikasi mengirim permintaan pengguna ke **operating system**.
2. OS menginterpretasikan permintaan tersebut dan mengarahkannya ke komponen hardware yang sesuai.
3. Hardware mengirim hasil/informasi kembali ke OS, yang kemudian diteruskan kembali ke aplikasi.

Poin penting: aplikasi **tidak pernah** berkomunikasi langsung dengan hardware. Semua komunikasi wajib melalui OS sebagai perantara.

### 2.3 Proses Booting

Proses booting melibatkan dua komponen utama:

- **BIOS atau UEFI** — microchip yang menyimpan instruksi loading awal komputer.
- **Bootloader** — software yang bertugas menjalankan (boot) sistem operasi setelah BIOS/UEFI selesai bekerja.

GUI dan CLI **tidak terlibat** dalam proses booting karena keduanya adalah jenis antarmuka pengguna, bukan komponen sistem yang menjalankan proses booting.

---

## 3. Perbandingan BIOS vs UEFI

| Aspek | BIOS | UEFI |
|---|---|---|
| Era | Sistem lama (legacy) | Sistem modern |
| Fungsi | Microchip instruksi loading | Microchip instruksi loading (pengganti BIOS) |

---

## 4. User Interface: GUI vs CLI

### 4.1 Konsep User Interface

**User interface** adalah program yang memungkinkan pengguna mengontrol fungsi-fungsi dari operating system. Pengguna berkomunikasi dengan OS melalui user interface ini, baik dalam bentuk grafis maupun berbasis teks.

### 4.2 Graphical User Interface (GUI)

**GUI** adalah antarmuka pengguna berbasis ikon yang membantu pengguna mengelola berbagai tugas di komputer secara visual. GUI **bukan** antarmuka berbasis teks.

### 4.3 Command-Line Interface (CLI)

**CLI** adalah antarmuka pengguna berbasis teks yang menggunakan perintah (commands) untuk berinteraksi dengan sistem operasi. CLI dapat menyelesaikan banyak tugas secara lebih efisien dibanding GUI, dan pemahaman terhadap CLI menjadi sangat penting untuk pekerjaan security analyst.

### 4.4 Relevansi CLI dalam Konteks Keamanan

**Studi kasus:** Tim keamanan mendapat peringatan kemungkinan breach di sistem. Satu analis menjalankan instruksi dari playbook dengan memasukkan perintah di command line. Analis lain kemudian memeriksa ulang perintah-perintah tersebut untuk memastikan sudah dimasukkan dengan benar.

Skenario ini menggambarkan aktivitas **reviewing a history file in a CLI** — yaitu meninjau riwayat perintah yang sudah dijalankan di command line. Ini menunjukkan bahwa penguasaan CLI dan kemampuan membaca history file merupakan bagian nyata dari kerja verifikasi tim security analyst.

---

## 5. Glossary — Istilah Kunci

- **Application** — Program yang menjalankan tugas tertentu.
- **Basic Input/Output System (BIOS)** — Microchip berisi instruksi loading komputer, umum pada sistem lama.
- **Bootloader** — Software yang menjalankan (boot) sistem operasi.
- **Command-line interface (CLI)** — Antarmuka berbasis teks yang menggunakan command untuk berinteraksi dengan komputer.
- **Graphical user interface (GUI)** — Antarmuka berbasis ikon untuk mengelola tugas di komputer.
- **Hardware** — Komponen fisik dari komputer.
- **Legacy operating system** — Sistem operasi yang sudah usang tapi masih dipakai.
- **Operating system (OS)** — Antarmuka penghubung hardware komputer dengan pengguna.
- **Random Access Memory (RAM)** — Komponen hardware untuk memori jangka pendek.
- **Resource allocation** — Proses OS memastikan kapasitas sistem yang terbatas digunakan di tempat yang paling dibutuhkan.
- **Unified Extensible Firmware Interface (UEFI)** — Microchip instruksi loading yang menggantikan BIOS pada sistem modern.
- **User interface** — Program yang memungkinkan pengguna mengontrol fungsi OS.
- **Virtual machine (VM)** — Versi virtual dari sebuah komputer fisik.

---

## 6. Tools & Platform Rujukan

Tidak ada tools atau platform eksternal (seperti VirusTotal, Shodan, dsb.) yang disebutkan pada materi room ini. Room ini murni membahas konsep dasar komputer dan sistem operasi tanpa referensi ke tools analisis keamanan.

---

## 7. Catatan Ringkas untuk Ditulis Tangan

**Definisi OS**
- OS — interface hardware ↔ user
- OS bikin komputer efisien, multitasking (banyak app jalan bareng)
- Semua device (PC, HP, tablet) punya OS

**Jenis OS**
- Desktop: Linux, macOS
- Mobile: Android, iOS
- Legacy OS — OS usang tapi masih dipakai

**Fungsi OS**
- Resource allocation — kapasitas terbatas → dialokasikan ke yang paling butuh

**Alur App → OS → Hardware**
- App kirim request → OS
- OS interpretasi → arahkan ke hardware
- Hardware balas → OS → balik ke App
- App TIDAK PERNAH langsung ke hardware

**Booting**
- BIOS/UEFI — microchip instruksi loading awal
- Bootloader — jalankan OS
- BIOS = lama, UEFI = pengganti modern
- GUI/CLI TIDAK terlibat di booting

**User Interface**
- User interface — cara user kontrol OS
- GUI — ikon, visual, BUKAN teks
- CLI — teks, pakai command, lebih efisien multitask

**Studi Kasus Security**
- Analis A: input command sesuai playbook
- Analis B: cek ulang command → ini disebut "reviewing history file in a CLI"

**Istilah lain**
- Application — program jalankan tugas spesifik
- Hardware — komponen fisik
- RAM — memori jangka pendek
- VM — versi virtual dari komputer fisik
