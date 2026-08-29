=== Computer Fundamentals - Resume Materi ===
[29 Aug 2026]


1. MENGAPA PERLU PAHAM KOMPUTER

   Prinsip dasar dalam cyber security: tidak bisa mengamankan sesuatu
   yang tidak dipahami. Sebelum belajar cara mempertahankan sistem,
   kita harus tahu dulu apa yang sedang kita pertahankan — apa
   komponennya, bagaimana mereka bekerja, dan bagaimana mereka saling
   berinteraksi.

   Proses boot secara khusus akan sering relevan ke depannya, karena
   boot process adalah salah satu target serangan hacker.


2. KOMPONEN INTI SISTEM KOMPUTER

   Room ini menggunakan analogi tubuh manusia untuk menjelaskan setiap
   komponen. Semua komputer, apapun jenisnya, dibangun dari blok-blok
   dasar yang sama.

   2.1 Motherboard — Tulang & Sistem Saraf
       Papan utama yang menopang dan menghubungkan semua komponen.
       Di atasnya terdapat CPU socket, RAM slots, expansion slots, dan
       berbagai port. Semua komponen lain terhubung melalui motherboard
       — tanpanya tidak ada komponen yang bisa berkomunikasi satu sama
       lain.

   2.2 CPU (Central Processing Unit) — Otak
       Prosesor yang mengeksekusi semua instruksi di dalam komputer.
       CPU modern memiliki multiple cores yang memungkinkan instruksi
       diproses secara paralel. CPU terpasang ke motherboard melalui
       CPU socket.

   2.3 RAM (Random Access Memory) — Memori Jangka Pendek
       Menyimpan data yang sedang aktif digunakan CPU agar bisa diakses
       dengan cepat. Bersifat volatile: seluruh isinya hilang ketika
       daya dimatikan. Teknologi terkini menggunakan DDR5 atau DDR6.

   2.4 HDD / SSD — Memori Jangka Panjang
       Menyimpan data secara permanen (non-volatile). HDD menggunakan
       teknologi lama dengan bagian yang bergerak — kapasitas besar
       namun lambat. SSD tidak memiliki bagian bergerak, menggunakan
       chip memori, jauh lebih cepat. Keduanya terhubung ke motherboard
       via kabel SATA atau slot PCI Express.

   2.5 GPU (Graphics Card) — Korteks Visual
       Menerima data dari OS dan program, lalu mengolahnya menjadi
       output visual yang ditampilkan ke monitor. Terhubung ke
       motherboard melalui slot PCI Express.

   2.6 PSU (Power Supply Unit) — Jantung & Paru-paru
       Menyuplai daya listrik ke seluruh komponen sistem. Jika total
       kebutuhan daya komponen melebihi kapasitas PSU, sistem akan
       gagal. Mendistribusikan daya melalui konektor seperti main
       motherboard connector dan Molex connectors.

   2.7 Network Adapter — Pita Suara
       Memungkinkan komputer berkomunikasi dengan sistem lain melalui
       jaringan. Tersedia dalam varian wired dan wireless. Bisa sudah
       tertanam di motherboard, atau ditambahkan sebagai expansion card
       yang terhubung via PCI Express.

   2.8 I/O Devices (Input/Output) — Indra
       Input: keyboard, mouse, mikrofon, scanner.
       Output: monitor, printer, speaker.
       Konektor umum yang digunakan: USB, HDMI, DisplayPort.


3. VOLATILE vs NON-VOLATILE

   3.1 Volatile
       Data hilang saat daya dimatikan. Contoh utama adalah RAM.
       Dalam konteks cyber security, RAM bisa menyimpan data sensitif
       selama komputer hidup seperti password, encryption keys, dan
       session tokens — konsep ini relevan dalam memory forensics.

   3.2 Non-Volatile
       Data tetap tersimpan meskipun daya dimatikan. Contoh: HDD dan
       SSD. Keduanya menjadi target utama dalam kasus pencurian data
       maupun serangan ransomware.


4. PROSES BOOTING

   4.1 Gambaran Umum
       Booting adalah proses yang dilalui komputer dari keadaan mati
       hingga OS siap digunakan. Analoginya seperti manusia yang bangun
       tidur — menyalakan organ, memeriksa kondisi tubuh, lalu menjadi
       sadar penuh. Urutan lima langkah berikut wajib dihapal.

   4.2 Langkah 1 — Press Power Button
       Sinyal dikirim ke PSU untuk mengalirkan daya ke seluruh sistem.
       Komponen mulai menerima daya dan proses power-up dimulai.

   4.3 Langkah 2 — Firmware Starts (UEFI / BIOS)
       Software pertama yang berjalan, bahkan sebelum OS aktif. UEFI
       menginisialisasi dan mengkoordinasikan semua komponen agar siap
       digunakan. BIOS adalah versi lama yang sudah sebagian besar
       digantikan UEFI, namun istilah keduanya masih sering dipakai
       secara bergantian.

   4.4 Langkah 3 — POST (Power-On Self Test)
       UEFI menjalankan POST untuk memverifikasi setiap hardware yang
       diperlukan: apakah komponen hadir, dikonfigurasi dengan benar,
       dan berfungsi normal. Jika ada yang bermasalah, sistem memicu
       bunyi beep atau peringatan (alert).

   4.5 Langkah 4 — Select Boot Device
       UEFI mengikuti daftar prioritas untuk menentukan perangkat mana
       yang akan digunakan sebagai sumber booting — biasanya SSD atau
       HDD yang sudah terinstal OS.

   4.6 Langkah 5 — Initiate Bootloader
       Bootloader di perangkat yang dipilih dijalankan. Ia memuat OS
       dari storage ke dalam RAM. Setelah OS berhasil dimuat, UEFI
       menyerahkan kendali penuh kepada OS — boot sequence selesai.


5. UEFI & BIOS

   BIOS (Basic Input/Output System) adalah firmware generasi lama.
   UEFI (Unified Extensible Firmware Interface) adalah penggantinya
   yang lebih modern. Keduanya memiliki fungsi yang sama: inisialisasi
   hardware, menjalankan POST, dan menentukan boot device.

   Dalam praktiknya istilah BIOS masih sering disebut meskipun yang
   dimaksud sebenarnya adalah UEFI — pahami keduanya sebagai konsep
   yang sama dengan generasi berbeda.

   Relevansi keamanan: jika UEFI dikompromikan oleh malware seperti
   bootkit atau rootkit level firmware, ancamannya sangat berbahaya
   karena berjalan sebelum OS dan antivirus aktif, sehingga sulit
   terdeteksi dan dihapus.


6. GLOSSARY

   Booting          Proses menyalakan komputer dari keadaan mati hingga
                    OS siap digunakan.

   Firmware         Software tingkat rendah yang tertanam di hardware,
                    berjalan pertama kali saat komputer dinyalakan,
                    sebelum OS apapun aktif.

   UEFI             Unified Extensible Firmware Interface. Firmware
                    modern pengganti BIOS.

   BIOS             Basic Input/Output System. Firmware generasi lama,
                    sudah digantikan UEFI namun istilahnya masih umum
                    dipakai.

   POST             Power-On Self Test. Rutinitas pengecekan hardware
                    yang dijalankan UEFI setiap kali komputer dinyalakan.

   Bootloader       Program kecil yang bertugas memuat OS dari storage
                    ke dalam RAM sebagai langkah terakhir proses boot.

   Volatile         Sifat memori yang kehilangan seluruh datanya saat
                    daya dimatikan. Contoh: RAM.

   Non-Volatile     Sifat memori yang mempertahankan datanya meskipun
                    daya mati. Contoh: HDD, SSD.

   CPU Socket       Slot khusus di motherboard tempat CPU dipasang dan
                    terhubung secara fisik ke sistem.

   PCI Express      Jalur koneksi kecepatan tinggi di motherboard,
                    digunakan oleh GPU, Network Card, dan SSD NVMe.

   SATA             Antarmuka koneksi standar untuk HDD dan SSD ke
                    motherboard.

   DDR5 / DDR6      Generasi terkini standar teknologi RAM, menawarkan
                    kecepatan dan performa lebih tinggi dari generasi
                    sebelumnya.

   Cores            Inti pemrosesan di dalam CPU. Lebih banyak core
                    berarti lebih banyak instruksi yang bisa diproses
                    secara paralel dalam waktu bersamaan.

   Expansion Card   Komponen tambahan yang dipasang ke slot di
                    motherboard untuk menambah kapabilitas sistem,
                    seperti GPU atau Network Adapter terpisah.

   Molex Connector  Konektor daya dari PSU yang digunakan untuk
                    menyuplai listrik ke komponen tertentu seperti
                    storage dan kipas.


7. TOOLS & PLATFORM RUJUKAN

   Room ini tidak menyebutkan tools atau platform eksternal apapun.
   Tidak ada link, software, atau layanan online yang direkomendasikan
   dalam materi Computer Fundamentals ini — room bersifat konseptual
   murni tanpa praktik tooling.