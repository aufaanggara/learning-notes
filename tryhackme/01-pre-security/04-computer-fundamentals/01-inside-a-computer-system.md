=== Computer Fundamentals - Resume Materi ===
[17 Aug 2026]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. MENGAPA PERLU PAHAM KOMPUTER
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Prinsip dasar dalam cyber security: tidak bisa mengamankan sesuatu yang
tidak dipahami. Sebelum belajar cara mempertahankan sistem, kita harus
tahu dulu apa yang sedang kita pertahankan — apa komponennya, bagaimana
mereka bekerja, dan bagaimana mereka saling berinteraksi.

Proses boot secara khusus disebut sebagai konsep yang akan sering
relevan ke depannya, karena boot process adalah salah satu target
serangan hacker.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
2. KOMPONEN INTI SISTEM KOMPUTER
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Room ini menggunakan analogi tubuh manusia untuk menjelaskan setiap
komponen. Semua komputer, apapun jenisnya, dibangun dari blok-blok
dasar yang sama.

  MOTHERBOARD — Tulang & Sistem Saraf
    Papan utama yang menopang dan menghubungkan semua komponen.
    Di atasnya terdapat CPU socket, RAM slots, expansion slots, dan
    berbagai port. Semua komponen lain terhubung melalui motherboard —
    tanpanya tidak ada komponen yang bisa berkomunikasi satu sama lain.

  CPU (Central Processing Unit) — Otak
    Prosesor yang mengeksekusi semua instruksi di dalam komputer.
    CPU modern memiliki multiple cores yang memungkinkan instruksi
    diproses secara paralel. CPU terpasang ke motherboard melalui
    CPU socket.

  RAM (Random Access Memory) — Memori Jangka Pendek
    Menyimpan data yang sedang aktif digunakan oleh CPU agar bisa
    diakses dengan cepat. Bersifat volatile: seluruh isinya hilang
    ketika daya dimatikan. Teknologi terkini menggunakan DDR5 atau DDR6.

  HDD / SSD — Memori Jangka Panjang
    Menyimpan data secara permanen (non-volatile). HDD menggunakan
    teknologi lama dengan bagian yang bergerak, kapasitas besar namun
    lambat. SSD tidak memiliki bagian bergerak, menggunakan chip
    memori, jauh lebih cepat. Keduanya terhubung ke motherboard via
    kabel SATA atau slot PCI Express.

  GPU (Graphics Card) — Korteks Visual
    Menerima data dari OS dan program, lalu mengolahnya menjadi
    output visual yang ditampilkan ke monitor. Terhubung ke
    motherboard melalui slot PCI Express.

  PSU (Power Supply Unit) — Jantung & Paru-paru
    Menyuplai daya listrik ke seluruh komponen sistem. Jika total
    kebutuhan daya komponen melebihi kapasitas PSU, sistem akan gagal.
    Mendistribusikan daya melalui konektor seperti main motherboard
    connector dan Molex connectors.

  Network Adapter — Pita Suara
    Memungkinkan komputer berkomunikasi dengan sistem lain melalui
    jaringan. Tersedia dalam varian wired dan wireless. Bisa sudah
    tertanam di motherboard, atau ditambahkan sebagai expansion card
    yang terhubung via PCI Express.

  I/O Devices (Input/Output) — Indra
    Input: keyboard, mouse, mikrofon, scanner.
    Output: monitor, printer, speaker.
    Konektor umum yang digunakan: USB, HDMI, DisplayPort.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
3. VOLATILE vs NON-VOLATILE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Volatile     Data hilang saat daya dimatikan. Contoh: RAM.
             Dalam konteks cyber security, RAM bisa menyimpan data
             sensitif selama komputer hidup seperti password, encryption
             keys, dan session tokens — relevan dalam memory forensics.

Non-Volatile Data tetap tersimpan meskipun daya dimatikan.
             Contoh: HDD, SSD. Target utama pencurian data dan ransomware.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
4. PROSES BOOTING — URUTAN & PENJELASAN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Booting adalah proses yang dilalui komputer dari keadaan mati hingga
OS siap digunakan. Analoginya seperti manusia yang bangun tidur —
menyalakan organ, memeriksa kondisi tubuh, lalu menjadi sadar penuh.

Urutan wajib dihapal:

  1. Press Power Button
     Sinyal dikirim ke PSU untuk mengalirkan daya ke seluruh sistem.

  2. Firmware Starts (UEFI / BIOS)
     Software pertama yang berjalan, bahkan sebelum OS aktif. UEFI
     menginisialisasi dan mengkoordinasikan semua komponen agar siap
     digunakan. BIOS adalah versi lama yang sudah sebagian besar
     digantikan UEFI, namun istilahnya masih sering dipakai secara
     bergantian.

  3. POST (Power-On Self Test)
     UEFI menjalankan POST untuk memverifikasi setiap hardware yang
     diperlukan: apakah komponen hadir, dikonfigurasi dengan benar,
     dan berfungsi normal. Jika ada yang bermasalah, sistem memicu
     bunyi beep atau peringatan (alert).

  4. Select Boot Device
     UEFI mengikuti daftar prioritas untuk menentukan perangkat mana
     yang akan digunakan sebagai sumber booting — biasanya SSD atau
     HDD yang sudah terinstal OS.

  5. Initiate Bootloader
     Bootloader di perangkat yang dipilih dijalankan. Ia memuat OS
     dari storage ke dalam RAM. Setelah OS berhasil dimuat, UEFI
     menyerahkan kendali penuh kepada OS — boot sequence selesai.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
5. UEFI & BIOS — PERBEDAAN DAN FUNGSI
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BIOS (Basic Input/Output System) adalah firmware generasi lama.
UEFI (Unified Extensible Firmware Interface) adalah penggantinya yang
lebih modern. Keduanya memiliki fungsi yang sama: inisialisasi hardware,
menjalankan POST, dan menentukan boot device.

Penting: dalam praktiknya istilah BIOS masih sering disebut meskipun
yang dimaksud sebenarnya adalah UEFI. Pahami keduanya sebagai konsep
yang sama dengan generasi berbeda.

Relevansi keamanan: jika UEFI dikompromikan oleh malware (misalnya
bootkit atau rootkit level firmware), ancamannya sangat berbahaya
karena berjalan sebelum OS dan antivirus aktif, sehingga sulit
terdeteksi.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
6. GLOSSARY — ISTILAH WAJIB HAPAL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Booting          Proses menyalakan komputer dari keadaan mati hingga
                 OS siap digunakan.

Firmware         Software tingkat rendah yang tertanam di hardware,
                 berjalan pertama kali saat komputer dinyalakan.

UEFI             Unified Extensible Firmware Interface. Firmware modern
                 pengganti BIOS.

BIOS             Basic Input/Output System. Firmware generasi lama,
                 sudah digantikan UEFI namun istilahnya masih umum dipakai.

POST             Power-On Self Test. Rutinitas pengecekan hardware yang
                 dijalankan UEFI setiap kali komputer dinyalakan.

Bootloader       Program kecil yang bertugas memuat OS dari storage ke RAM.

Volatile         Sifat memori yang kehilangan datanya saat daya dimatikan.

Non-Volatile     Sifat memori yang mempertahankan datanya meskipun daya mati.

CPU Socket       Slot khusus di motherboard tempat CPU dipasang.

PCI Express      Jalur koneksi kecepatan tinggi di motherboard, digunakan
                 oleh GPU, Network Card, dan SSD NVMe.

SATA             Antarmuka koneksi untuk HDD dan SSD ke motherboard.

DDR5 / DDR6      Generasi terkini standar teknologi RAM, lebih cepat dari
                 generasi sebelumnya.

Cores            Inti pemrosesan di dalam CPU. Lebih banyak core berarti
                 lebih banyak instruksi yang bisa diproses secara paralel.

Expansion Card   Komponen tambahan yang dipasang ke slot di motherboard,
                 seperti GPU atau Network Adapter terpisah.