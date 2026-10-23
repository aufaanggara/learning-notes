# Linux Fundamentals Part 1: Resume Materi

Tanggal: 22 Oktober 2026
Platform: TryHackMe
Room: Linux Fundamentals Part 1

&nbsp;

## 1. Konsep Dasar Linux

### 1.1 Apa itu Linux

**Linux** adalah sistem operasi open-source yang berbasis **UNIX**. Posisinya sejajar dengan Windows dan macOS, tetapi jauh lebih ringan dan efisien.

Linux tidak selalu dilengkapi tampilan desktop. Karena itu, interaksi dengan sistemnya sebagian besar dilakukan lewat **Terminal**.

### 1.2 Di Mana Linux Digunakan

Linux menjalankan banyak hal yang kita temui sehari-hari:

- Website yang kita kunjungi
- Panel hiburan dan kontrol di mobil
- Sistem **Point of Sale (PoS)** seperti mesin kasir di toko
- Infrastruktur kritis seperti pengontrol lampu lalu lintas dan sensor industri
- Perangkat Android, superkomputer, peralatan rumah tangga, dan server perusahaan

### 1.3 Distribusi Linux (Flavours)

"Linux" adalah istilah payung untuk banyak OS berbasis UNIX. Karena bersifat **open-source**, siapa pun boleh memodifikasinya, sehingga lahir banyak varian yang disebut **distribusi**.

**Ubuntu** dan **Debian** adalah distribusi yang paling umum karena sangat extensible. Keduanya bisa berjalan sebagai server (website dan web application) maupun sebagai desktop lengkap. Seluruh seri room ini memakai **Ubuntu**.

Ubuntu Server bisa berjalan di sistem dengan RAM hanya 512MB.

> Analogi: Windows punya versi 7, 8, dan 10. Linux punya banyak distribusi, dan masing-masing dirancang untuk kebutuhan yang berbeda.

&nbsp;

## 2. Terminal dan Mesin Praktik

### 2.1 Menjalankan Mesin di Browser

Room menyediakan mesin Ubuntu yang bisa dipakai langsung di browser tanpa instalasi. Mesin dijalankan dengan tombol **Start Machine**.

Setelah aktif, muncul kartu **Active Machine Information** yang berisi:

- **Title**: nama mesin
- **IP Address**: alamat IP mesin
- **Expires**: sisa waktu sebelum mesin mati otomatis
- Tombol **Add 1 hour** untuk menambah waktu
- Tombol **Terminate** untuk mematikan mesin

Mesin wajib di-**Terminate** setelah selesai dipakai.

### 2.2 Terminal dan Prompt

**Terminal** adalah antarmuka berbasis teks. Awalnya terasa menakutkan, tetapi cepat familiar setelah dipakai rutin.

Tampilan prompt:

```
tryhackme@linux1:~$
```

Prompt menandakan terminal siap menerima perintah. Bagian `tryhackme` adalah username, `linux1` adalah nama mesin, dan `~` adalah lokasi folder saat ini (home directory).

&nbsp;

## 3. Perintah Dasar

### 3.1 Output dan Informasi User

**echo** menampilkan teks yang kita berikan.

```
echo Hello
echo "Hello Friend!"
```

Satu kata tidak perlu tanda kutip. Jika ada satu spasi atau lebih, teks wajib dibungkus **double quotes**.

**whoami** menampilkan username yang sedang login.

```
whoami
```

### 3.2 Navigasi Filesystem

Karena tidak ada tampilan visual, empat perintah ini menjadi dasar bernavigasi.

| Command | Kepanjangan | Fungsi |
|---|---|---|
| `ls` | listing | Menampilkan isi folder |
| `cd` | change directory | Pindah ke folder lain |
| `cat` | concatenate | Menampilkan isi file |
| `pwd` | print working directory | Menampilkan path lengkap folder saat ini |

#### ls

Menampilkan file dan folder di lokasi saat ini.

```
ls
ls Pictures
```

Dengan menambahkan nama folder, isi folder tersebut bisa dilihat tanpa harus masuk ke dalamnya.

#### cd

Berpindah ke folder tertentu. Setelah pindah, gunakan `ls` lagi untuk melihat isinya.

```
cd Pictures
```

#### cat

Mencetak isi file ke layar. Tidak terbatas pada file teks saja. Path lengkap bisa dipakai agar tidak perlu berpindah folder.

```
cat todo.txt
cat /home/ubuntu/Documents/todo.txt
```

File sering menyimpan informasi sensitif seperti username, password, flag, atau konfigurasi, sehingga `cat` sering dipakai untuk mengambilnya.

#### pwd

Menampilkan **path** lengkap dari working directory, misalnya `/home/ubuntu/Documents`.

```
pwd
```

Hasilnya berguna untuk kembali ke lokasi tersebut kapan saja:

```
cd /home/ubuntu/Documents
```

&nbsp;

## 4. Pencarian File dan Konten

Memeriksa folder satu per satu dengan `cd` dan `ls` tidak efisien ketika direktori bersarang. Perintah `find` dan `grep` mengotomatiskan pencarian.

### 4.1 find: Mencari File

**find** mencari file berdasarkan nama.

```
find -name passwords.txt
```

Output berupa lokasi file, misalnya `./folder1/passwords.txt`.

| Switch | Fungsi |
|---|---|
| `-name` | Mencari berdasarkan nama file |

Untuk mencari berdasarkan ekstensi, gunakan **wildcard** `*` yang berarti "apa saja":

```
find -name *.txt
```

Perintah ini menampilkan semua file berakhiran `.txt` beserta lokasinya, misalnya `./folder1/passwords.txt` dan `./Documents/todo.txt`.

### 4.2 grep: Mencari Konten di Dalam File

**grep** mencari nilai tertentu di dalam isi file. Berbeda dengan `find` yang mencari nama file.

Skenarionya: sebuah `access.log` milik web server berisi 244 baris. Membacanya dengan `cat` tidak efisien kalau yang dicari hanya aktivitas satu IP address.

Untuk menghitung jumlah baris, dipakai `wc`:

```
wc -l access.log
```

| Switch | Fungsi |
|---|---|
| `-l` | Menghitung jumlah baris (lines) |

Untuk mencari entri milik satu IP:

```
grep "81.143.211.90" access.log
```

Hanya baris yang mengandung nilai tersebut yang ditampilkan.

### 4.3 grep -R: Pencarian Rekursif

Ketika informasi tersebar di banyak file dalam sebuah direktori, **grep** bisa dijalankan secara **recursive**.

```
grep -R "PRETTY_NAME" /etc/
```

| Switch | Fungsi |
|---|---|
| `-R` | Recursive: memeriksa semua file di direktori dan seluruh subdirektorinya |

Contoh output:

```
grep: /etc/sudoers: Permission denied
/etc/os-release:PRETTY_NAME="Ubuntu"
```

Path file ditampilkan di depan baris yang cocok, sehingga mudah diketahui sumbernya. Pesan `Permission denied` muncul untuk file yang tidak boleh dibaca oleh user saat ini.

&nbsp;

## 5. Shell Operators

**Shell operators** adalah simbol khusus yang memperluas kemampuan perintah.

### 5.1 Operator & (Background)

Menjalankan perintah di latar belakang. Berguna untuk proses lama, misalnya menyalin file besar, supaya terminal tidak terkunci dan masih bisa dipakai untuk perintah lain.

```
command &
```

### 5.2 Operator && (Combine)

Menggabungkan beberapa perintah dalam satu baris.

```
command1 && command2
```

`command2` hanya berjalan jika `command1` berhasil. Jangan tertukar dengan `&` yang fungsinya berbeda.

### 5.3 Operator > (Redirect Replace)

Mengarahkan output sebuah perintah ke tempat lain, biasanya file. Jika file sudah ada, **isinya ditimpa**.

```
echo hey > welcome
cat welcome
```

Output `cat`: `hey`.

### 5.4 Operator >> (Redirect Append)

Sama seperti `>`, tetapi output **ditambahkan di bagian akhir** file tanpa menghapus isi lama.

```
echo hello >> welcome
cat welcome
```

Output `cat`:

```
hey
hello
```

### 5.5 Perbandingan > dan >>

| Operator | Perilaku | Isi lama |
|---|---|---|
| `>` | Mengganti seluruh isi file | Hilang |
| `>>` | Menambah di bawah isi file | Tetap ada |

&nbsp;

## 6. Glossary

**Linux**: sistem operasi open-source berbasis UNIX.

**UNIX**: sistem operasi yang menjadi dasar Linux.

**Distribusi (distro)**: varian Linux yang disesuaikan untuk kebutuhan tertentu, misalnya Ubuntu dan Debian.

**Open-source**: kode sumber terbuka sehingga bebas dimodifikasi.

**GUI**: Graphical User Interface, tampilan desktop berbasis grafis.

**Terminal**: antarmuka berbasis teks untuk berinteraksi dengan sistem.

**Prompt**: tanda terminal siap menerima perintah, contohnya `tryhackme@linux1:~$`.

**Command**: perintah yang diketik di terminal.

**Filesystem**: struktur penyimpanan file dan folder.

**Directory**: istilah lain untuk folder.

**Working directory**: folder tempat kita berada saat ini.

**Path**: alamat lengkap lokasi file atau folder, contohnya `/home/ubuntu/Documents`.

**Wildcard (`*`)**: simbol yang mewakili karakter apa saja dalam pencarian.

**Recursive**: proses yang mencakup semua subfolder.

**Concatenate**: menggabungkan dan menampilkan isi file.

**Redirect**: mengalihkan output perintah ke file atau tujuan lain.

**Background process**: proses yang berjalan tanpa mengunci terminal.

**Shell operator**: simbol khusus untuk mengendalikan atau menggabungkan perintah.

**Access log**: catatan permintaan yang masuk ke sebuah web server.

**PoS**: Point of Sale, sistem kasir di toko.

&nbsp;

## 7. Tools & Platform Rujukan

**TryHackMe**: platform belajar yang menyediakan mesin Linux interaktif di browser untuk praktik room ini.

**Ubuntu**: distribusi Linux yang dipakai di seluruh seri Linux Fundamentals.

**Debian**: distribusi Linux umum lain yang disebut sebagai contoh.

**Tutorial room TryHackMe**: room panduan untuk cara mengelola mesin, termasuk Terminate. Tidak ada URL eksplisit di materi.

**Linux Fundamentals Part 2**: room lanjutan yang direkomendasikan setelah menyelesaikan Part 1. Tidak ada URL eksplisit di materi.

&nbsp;

## 8. Catatan Ringkas untuk Ditulis Tangan

### Konsep Linux

- Linux: OS open-source berbasis UNIX
- Sifat: ringan, efisien
- Dipakai di: website, mobil, PoS, lampu lalu lintas, sensor industri, Android, server
- Distro: varian Linux hasil modifikasi (open-source)
- Ubuntu dan Debian: distro paling umum, bisa server atau desktop
- Seri ini: pakai Ubuntu
- Ubuntu Server: cukup RAM 512MB
- Tanpa GUI: interaksi lewat terminal

### Mesin Praktik

- Start Machine: jalankan mesin di browser
- Kartu info: Title, IP Address, Expires
- Add 1 hour: tambah waktu mesin
- Terminate: matikan mesin, wajib setelah selesai

### Terminal

- Terminal: antarmuka teks
- Prompt: tryhackme@linux1:~$ (siap terima perintah)

### Perintah Dasar

- echo teks: tampilkan teks
- echo "teks spasi": pakai double quotes kalau ada spasi
- whoami: user yang sedang login
- ls: lihat isi folder
- ls NamaFolder: lihat isi folder lain tanpa masuk
- cd NamaFolder: pindah folder
- cat namafile: tampilkan isi file
- cat /path/lengkap/file: baca file tanpa pindah folder
- pwd: path lengkap folder saat ini
- cd /path/lengkap: kembali ke lokasi tersimpan
- Isi file bisa berisi username, password, flag, config

### Pencarian

- find -name namafile: cari file berdasarkan nama
- find -name *.txt: cari semua file berekstensi .txt
- * (wildcard): mewakili apa saja
- grep "kata" namafile: cari kata di dalam file
- grep -R "kata" /dir/: cari rekursif di semua file dan subfolder
- -R: recursive
- wc -l namafile: hitung jumlah baris
- -l: hitung baris
- Permission denied: user tidak berhak baca file itu
- Hasil grep -R: path file tampil sebelum baris yang cocok

### Shell Operators

- command &: jalankan di background
- command1 && command2: command2 jalan hanya jika command1 berhasil
- command > file: output ke file, isi lama DITIMPA
- command >> file: output ke file, DITAMBAH di bawah
- Perbedaan: > ganti, >> tambah

### Istilah Kunci

- Path: alamat lengkap file atau folder
- Working directory: folder saat ini
- Filesystem: struktur file dan folder
- Redirect: alihkan output
- Recursive: termasuk semua subfolder
- Concatenate: gabung dan tampilkan isi file
- GUI: tampilan desktop grafis
- PoS: sistem kasir toko
- Access log: catatan request ke web server

### Langkah Berikutnya

- Latihan sampai jadi kebiasaan
- Lanjut: Linux Fundamentals Part 2
