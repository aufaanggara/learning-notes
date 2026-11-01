# Resume: How Operating Systems Work
**Tanggal:** 19 September 2026

---

## 1. Konsep Dasar Sistem Operasi

### 1.1 Peran Operating System (OS)

OS adalah lapisan software yang membuat program lain bisa berjalan secara efisien tanpa pengguna perlu tahu detail teknis pengendalian hardware. OS mengurus seluruh komunikasi antara aplikasi dan komponen fisik komputer.

Analogi: OS itu seperti mesin pada mobil — pengemudi (user) cukup menginjak gas, tanpa perlu paham mekanisme di baliknya. Analogi lain: OS seperti dapur restoran — pelanggan (user) memesan lewat pelayan (aplikasi), dapur (OS) yang mengolah permintaan itu ke bahan mentah (hardware), lalu hasilnya dikembalikan.

### 1.2 Empat Bagian Proses Penyelesaian Tugas

Setiap tugas di komputer melewati alur: **User → Application → Operating System → Hardware**, lalu hasil dikembalikan lewat jalur yang sama secara terbalik.

- **User** — pihak yang memulai proses dengan sebuah kebutuhan/tugas.
- **Application** — software yang berinteraksi langsung dengan user (contoh: kalkulator, word processor, browser).
- **Operating System** — menerima request dari aplikasi, menerjemahkannya, lalu mengarahkan ke komponen hardware yang sesuai.
- **Hardware** — mengeksekusi pemrosesan nyata (contoh: CPU menghitung angka, hard drive menyimpan file), lalu mengirim hasil kembali ke OS untuk diteruskan ke aplikasi.

Contoh alur nyata yang dibahas:
- Kalkulator: user input angka → aplikasi kirim ke OS → OS kirim ke CPU → CPU hitung → hasil dikembalikan ke OS → ditampilkan di aplikasi.
- Download file dari browser: user klik download → browser lapor ke OS → OS teruskan ke hardware untuk proses download → hardware selesai unduh → OS informasikan ke browser → browser beri tahu user.
- Menyimpan dokumen: user ketik & save di word processor → aplikasi komunikasi ke OS → OS simpan file ke hard drive → hard drive konfirmasi tersimpan → OS teruskan konfirmasi ke aplikasi.

---

## 2. Proses Booting Komputer

### 2.1 Tahapan Booting

Ketika tombol power ditekan, urutan yang terjadi:

1. **BIOS** atau **UEFI** diaktifkan.
2. BIOS/UEFI menjalankan instruksi loading, termasuk memverifikasi kondisi hardware, lalu memuat **bootloader**.
3. Bootloader menjalankan (start) operating system.

Setelah OS selesai booting, komputer siap dipakai. Bootloader tidak pernah berjalan otomatis di awal — ia hanya aktif setelah dimuat oleh BIOS/UEFI.

### 2.2 BIOS vs UEFI

- **BIOS (Basic Input/Output System)** — microchip lama berisi instruksi loading komputer, jadi standar hingga tahun 2007.
- **UEFI (Unified Extensible Firmware Interface)** — microchip pengganti BIOS pada sistem modern, menawarkan fitur keamanan yang lebih baik. Sebagian besar komputer baru saat ini menggunakan UEFI.

Keduanya menjalankan fungsi dasar yang sama: memuat bootloader.

### 2.3 Relevansi Keamanan pada Tahap Booting

BIOS sering tidak ikut dipindai oleh antivirus, sehingga bisa menjadi titik rentan terhadap infeksi malware. Analis keamanan perlu memahami tahap ini untuk menelusuri sumber insiden yang mungkin terjadi sejak proses awal komputer menyala.

---

## 3. Resource Management oleh OS

### 3.1 OS sebagai Pengelola Sumber Daya

OS bertanggung jawab menyeimbangkan penggunaan sumber daya sistem (CPU, memori, storage, bandwidth input/output) agar digunakan secara efisien. Berbeda tugas membutuhkan sumber daya berbeda — misal antivirus scan memakai lebih banyak resource dibanding aplikasi kalkulator.

Analogi: OS seperti konduktor orkestra yang mengarahkan berbagai instrumen (program/proses) agar bekerja selaras sesuai kapasitas panggung (kapasitas sistem) yang terbatas.

### 3.2 Alokasi dan De-alokasi

Banyak program, task, dan proses bersaing memperebutkan CPU, memori, dan storage secara bersamaan. OS memastikan setiap program mendapat **allocation** (alokasi) sumber daya saat dibutuhkan dan **de-allocation** (pelepasan) saat tidak lagi dipakai, sehingga sistem tetap berjalan efisien meski banyak proses berjalan paralel.

### 3.3 Task Manager

**Task manager** adalah tool yang menampilkan daftar task yang sedang berjalan beserta penggunaan memori dan CPU masing-masing. Berguna bagi analis untuk:

- Troubleshooting aplikasi yang bermasalah.
- Incident response — misalnya mendeteksi komputer melambat karena resource dialokasikan ke malware.

---

## 4. Virtualisasi dan Virtual Machine

### 4.1 Definisi Virtual Machine (VM)

**Virtual machine (VM)** adalah versi virtual dari komputer fisik, salah satu bentuk dari **virtualisasi** — proses menggunakan software untuk menciptakan representasi virtual dari mesin fisik. VM tidak memakai hardware fisik dedicated; ia memakai versi software-defined dari CPU, storage, dan hardware lain. VM sepenuhnya berupa kode.

### 4.2 Cara Kerja Multi-VM pada Satu Host

Satu komputer fisik (host) bisa menjalankan beberapa VM sekaligus dengan cara membagi resource fisiknya. Contoh: host dengan RAM 16GB bisa menjalankan 3 VM sehingga host dan tiap VM masing-masing mendapat 4GB RAM. Setiap VM punya OS-nya sendiri dan berfungsi layaknya komputer independen.

### 4.3 Manfaat Virtual Machine

**Security (Keamanan)**
VM menyediakan **isolated environment** (sandbox) di dalam host. VM disebut "guest" yang terisolasi dari host dan dari VM lain. Jika satu VM terinfeksi malware, penanganannya lebih aman karena terisolasi. Analis keamanan juga bisa sengaja menaruh malware di VM untuk dianalisis dengan aman.

Catatan risiko: malware bisa "escape" dari VM dan mengakses host, sehingga sistem virtual tidak boleh sepenuhnya dipercaya sebagai lingkungan yang 100% aman.

**Efficiency (Efisiensi)**
Banyak VM bisa dibuka dan dipindah-pindah dengan mudah untuk keperluan testing dan eksplorasi aplikasi, tanpa perlu banyak mesin fisik terpisah.

Analogi: VM seperti bus kota — satu bus mengangkut banyak orang sekaligus secara efisien, dibanding tiap orang mengendarai mobil sendiri (memakai lebih banyak resource). Begitu pula banyak VM bisa di-host di satu mesin fisik yang sama.

### 4.4 Hypervisor

**Hypervisor** adalah software untuk mengelola beberapa VM sekaligus, menghubungkan hardware virtual dengan hardware fisik, serta mengalokasikan shared resource dari host ke VM-VM tersebut.

**KVM (Kernel-based Virtual Machine)** — hypervisor open-source yang didukung mayoritas distribusi Linux, built-in di dalam kernel Linux, sehingga bisa membuat VM di mesin Linux tanpa software tambahan.

### 4.5 Bentuk Virtualisasi Lain

Tidak semua virtualisasi menggunakan OS penuh seperti VM. Contoh lain:

- **Virtual server** — beberapa server virtual dibuat dari satu server fisik.
- **Virtual network** — jaringan virtual dibuat untuk memakai hardware jaringan fisik secara lebih efisien.

---

## 5. Glossary

- **BIOS (Basic Input/Output System)** — microchip lama yang memuat instruksi booting komputer.
- **UEFI (Unified Extensible Firmware Interface)** — pengganti BIOS pada sistem modern, dengan fitur keamanan lebih baik.
- **Bootloader** — program software yang dijalankan oleh BIOS/UEFI untuk memulai OS.
- **Operating System (OS)** — software yang mengelola komunikasi antara aplikasi dan hardware, serta mengelola resource sistem.
- **Application** — program yang menjalankan tugas spesifik yang diminta user.
- **CPU (Central Processing Unit)** — komponen hardware yang melakukan pemrosesan/kalkulasi.
- **RAM (Random Access Memory)** — hardware untuk memori jangka pendek.
- **Resource management** — pengelolaan alokasi dan de-alokasi sumber daya (CPU, memori, storage, bandwidth) di antara program yang berjalan.
- **Task manager** — tool untuk memantau task, penggunaan memori, dan CPU yang sedang berjalan.
- **Virtualization** — proses menggunakan software untuk membuat representasi virtual dari mesin fisik.
- **Virtual Machine (VM)** — versi virtual dari komputer fisik dengan CPU, storage, dan hardware virtualnya sendiri.
- **Host** — mesin fisik yang menjalankan satu atau lebih VM.
- **Guest** — VM yang berjalan di dalam host, terisolasi dari host dan VM lain.
- **Sandbox** — lingkungan terisolasi (bisa berupa VM) untuk menjalankan/menguji sesuatu dengan aman.
- **Hypervisor** — software untuk mengelola VM dan mengalokasikan resource host ke VM.
- **KVM (Kernel-based Virtual Machine)** — hypervisor open-source built-in di kernel Linux.
- **VM escape** — kondisi di mana malware/program berbahaya berhasil keluar dari isolasi VM dan mengakses host.

---

## 6. Tools & Platform Rujukan

- **KVM (Kernel-based Virtual Machine)** — hypervisor open-source untuk membuat dan mengelola VM di sistem Linux, built-in di kernel Linux sehingga tidak butuh software tambahan.

---

## 7. Catatan Ringkas untuk Ditulis Tangan

**OS Dasar**
- OS — bantu program lain jalan efisien, urus hardware detail
- Alur tugas: User → Application → OS → Hardware → balik lagi
- Analogi: OS = mesin mobil / dapur restoran

**Booting**
- Power on → BIOS/UEFI aktif → load bootloader → bootloader start OS
- BIOS — chip lama, standar sampai 2007
- UEFI — pengganti BIOS, lebih aman, dipakai sistem modern
- BIOS sering tidak discan antivirus — celah keamanan

**Resource Management**
- OS = konduktor orkestra, atur CPU/memori/storage/I-O bandwidth
- Allocation — kasih resource ke proses yang butuh
- De-allocation — lepas resource yang sudah tidak dipakai
- Task manager — lihat task + pemakaian CPU/memori, buat troubleshoot & incident response

**Virtualisasi**
- VM — versi virtual komputer fisik, full software (CPU/storage virtual)
- 1 host bisa jalankan banyak VM — resource fisik dibagi (contoh: 16GB RAM / 3 VM = 4GB tiap sisi)
- Manfaat: Security (isolasi/sandbox, guest terpisah dari host) + Efficiency (banyak VM di 1 mesin, hemat resource — analogi bus kota)
- Risiko — VM escape, malware bisa lolos ke host, jangan full trust virtualized system
- Hypervisor — software kelola VM + alokasi resource
- KVM — hypervisor open-source, built-in kernel Linux
- Virtualisasi lain tanpa OS — virtual server, virtual network
