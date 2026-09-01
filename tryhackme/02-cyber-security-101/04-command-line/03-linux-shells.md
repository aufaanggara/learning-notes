# Resume Materi: Introduction to Linux Shells
**Platform:** TryHackMe | **Tanggal:** 1 September 2026

---

## 1. Konsep Dasar Shell

### 1.1 Apa itu Shell

**Shell** adalah program yang menerima perintah dari pengguna melalui **CLI (Command Line Interface)** dan meneruskannya ke sistem operasi untuk dieksekusi. Shell adalah alternatif dari **GUI (Graphical User Interface)** yang lebih efisien dan hemat sumber daya.

Shell bukan satu-satunya cara berinteraksi dengan OS, tapi cara ini memberikan lebih banyak kontrol dan fleksibilitas dibanding GUI — terutama untuk tugas-tugas yang berulang, otomatisasi, dan administrasi sistem.

### 1.2 Perbedaan Shell dan GUI

| Aspek | GUI | CLI via Shell |
|---|---|---|
| Cara kerja | Klik pada elemen visual | Ketik perintah teks |
| Efisiensi | Lebih lambat untuk tugas kompleks | Lebih cepat dan efisien |
| Kontrol | Terbatas pada fitur yang disediakan | Hampir tidak terbatas |
| Resource | Lebih berat | Lebih ringan |

---

## 2. Perintah Dasar Shell

### 2.1 Navigasi Direktori

**`pwd`** — Print Working Directory. Menampilkan path lengkap dari direktori kerja saat ini.

```bash
pwd
# Output: /home/user
```

**`cd`** — Change Directory. Berpindah dari satu direktori ke direktori lain.

```bash
cd Desktop          # pindah ke folder Desktop
cd ..               # mundur satu level ke direktori induk
cd /var/log         # pindah ke path absolut
```

### 2.2 Melihat dan Membaca File

**`ls`** — List. Menampilkan daftar file dan folder dalam direktori aktif.

```bash
ls
# Output: Desktop  Documents  Downloads  Music  Pictures
```

**`cat`** — Concatenate. Membaca dan menampilkan seluruh isi file ke layar.

```bash
cat filename.txt
# Output: isi file dari baris pertama sampai terakhir
```

**`grep`** — Global Regular Expression Print. Mencari kata atau pola tertentu di dalam file dan menampilkan hanya baris yang mengandung kata tersebut.

```bash
grep THM dictionary.txt
# Output: hanya baris yang mengandung kata "THM"
```

---

## 3. Jenis-Jenis Shell Linux

### 3.1 Melihat Shell yang Digunakan

```bash
echo $SHELL          # menampilkan shell yang sedang aktif
cat /etc/shells      # menampilkan semua shell yang terinstal di sistem
```

### 3.2 Berpindah dan Mengubah Shell Default

```bash
zsh                          # beralih ke zsh sementara
chsh -s /usr/bin/zsh         # ubah shell default secara permanen
```

### 3.3 Perbandingan Tiga Shell Utama

**Bash (Bourne Again Shell)**
- Shell default di sebagian besar distribusi Linux
- Memiliki scripting yang kompatibel luas dengan dokumentasi sangat lengkap
- Fitur tab completion dasar dan history command dengan tombol panah atas/bawah
- Perintah `history` untuk melihat semua riwayat perintah
- Syntax highlighting tidak tersedia secara built-in

**Fish (Friendly Interactive Shell)**
- Bukan default di sebagian besar distro; fokus pada kemudahan penggunaan
- Syntax paling sederhana — cocok untuk pemula
- Auto spell correction built-in
- Syntax highlighting built-in dengan pewarnaan berdasarkan peran tiap bagian perintah
- Tab completion canggih berdasarkan riwayat perintah sebelumnya
- Kustomisasi prompt dengan tema interaktif

**Zsh (Z Shell)**
- Bukan default di sebagian besar distro; dianggap sebagai shell modern
- Menggabungkan fitur dari beberapa shell sebelumnya
- Scripting setara Bash tapi dengan fitur tambahan
- Tab completion bisa diperluas dengan plugin
- Kustomisasi sangat luas melalui framework **oh-my-zsh**
- Auto spell correction tersedia
- Syntax highlighting bisa ditambahkan via plugin

---

## 4. Shell Scripting

### 4.1 Definisi dan Tujuan

**Shell script** adalah file berisi kumpulan perintah shell yang dieksekusi secara berurutan. Tujuan utamanya adalah **otomatisasi** — menggabungkan banyak perintah yang berulang ke dalam satu file agar bisa dijalankan sekaligus.

Scripting bisa dilakukan di berbagai bahasa pemrograman, tapi ruang lingkup ini adalah scripting menggunakan Bash.

### 4.2 Membuat File Script

Script disimpan dalam file dengan ekstensi `.sh`. Dibuat menggunakan teks editor seperti `nano`.

```bash
nano nama_script.sh
```

### 4.3 Shebang

**Shebang** adalah baris pertama yang wajib ada di setiap script. Formatnya `#!` diikuti path interpreter yang akan digunakan.

```bash
#!/bin/bash
```

Tanpa shebang yang benar, sistem tidak tahu program mana yang harus dipakai untuk membaca script — ini adalah penyebab error `bad interpreter: No such file or directory`.

### 4.4 Memberikan dan Menjalankan Script

```bash
chmod +x nama_script.sh     # memberi izin eksekusi pada file script
./nama_script.sh            # menjalankan script dari direktori saat ini
```

`./` wajib ditulis karena memberitahu shell untuk mencari file di direktori aktif. Tanpa `./`, shell akan mencari di PATH environment variable dan tidak akan menemukan script tersebut.

---

## 5. Komponen Shell Script

### 5.1 Variabel

Variabel menyimpan nilai yang bisa dipanggil berkali-kali. Saat mendefinisikan: tulis nama variabel saja. Saat memanggil: tambahkan `$` di depan nama variabel.

```bash
username=""                  # deklarasi variabel kosong
username="John"              # mengisi variabel langsung
read username                # mengisi variabel dari input pengguna
echo "Hello, $username"      # memanggil isi variabel
```

Tanda kutip ganda `" "` memproses variabel di dalamnya. Tanda kutip tunggal `' '` mencetak teks apa adanya tanpa memproses variabel.

### 5.2 Perintah `echo` dan `read`

**`echo`** — menampilkan teks atau nilai variabel ke layar.

**`read`** — mengambil input dari pengguna dan menyimpannya ke variabel yang ditentukan.

```bash
echo "Masukkan nama:"
read name
echo "Halo, $name"
```

### 5.3 Loop `for`

Digunakan untuk menjalankan blok kode secara berulang. Blok kode diapit oleh `do` (pembuka) dan `done` (penutup).

```bash
for i in {1..10}; do
    echo $i
done
```

`{1..10}` disebut **brace expansion** — cara Bash mendefinisikan range angka. Variabel `i` akan berisi nilai 1, 2, 3, ... 10 secara berurutan di setiap iterasi.

### 5.4 Conditional Statement

Digunakan untuk menjalankan kode hanya jika kondisi tertentu terpenuhi. Struktur lengkapnya:

```bash
if [ kondisi ]; then
    # kode jika kondisi benar
elif [ kondisi_lain ]; then
    # kode jika kondisi pertama salah tapi kondisi ini benar
else
    # kode jika semua kondisi salah
fi
```

`fi` adalah penutup wajib dari setiap blok `if` (kata `if` dibalik).

### 5.5 Operator Perbandingan

Di dalam `[ ]`, ada dua jenis operator:

Untuk **teks/string**: gunakan `=`

```bash
[ "$name" = "John" ]
```

Untuk **angka**: gunakan `-eq`

```bash
[ "$i" -eq 1 ]
```

### 5.6 Operator Logika `&&`

`&&` berarti **DAN** — semua kondisi yang dihubungkan harus benar secara bersamaan.

```bash
if [ "$a" = "x" ] && [ "$b" = "y" ]; then
    echo "Keduanya benar"
fi
```

### 5.7 Perintah `[ ]` (Test Command)

`[` bukan sekadar simbol — ini adalah **perintah** di Bash, sama seperti `echo` atau `ls`. Karena itu, **wajib ada spasi** setelah `[` dan sebelum `]`.

```bash
[ "$name" = "John" ]    # benar — ada spasi di dalam
["$name" = "John"]      # salah — tidak ada spasi, error
```

### 5.8 Titik Koma `;`

Di Bash, baris baru adalah pemisah perintah. Titik koma `;` berfungsi sebagai pengganti baris baru — digunakan ketika dua kata kunci ingin ditulis dalam satu baris.

```bash
# Tanpa titik koma — dua baris
if [ kondisi ]
then

# Dengan titik koma — satu baris
if [ kondisi ]; then
```

### 5.9 Komentar

Diawali dengan `#` — semua teks setelah `#` diabaikan sepenuhnya oleh Bash. Pengecualian: `#!` di baris pertama (shebang) punya makna khusus.

```bash
# Ini adalah komentar — tidak dieksekusi
echo "Ini dieksekusi"
```

Praktik terbaik: tambahkan komentar di bagian-bagian utama dan kompleks dari script, bukan di setiap baris.

---

## 6. Studi Kasus: Locker Script

Skenario: script verifikasi identitas pengguna sebelum membuka akses loker bank. Menggabungkan variabel, loop, conditional, dan operator logika dalam satu script utuh.

Alur kerja script:
1. Deklarasi variabel kosong untuk username, company name, dan PIN
2. Loop 3 kali — setiap iterasi meminta satu data berbeda menggunakan `elif`
3. Setelah loop selesai, cek ketiga variabel sekaligus menggunakan `&&`
4. Tampilkan pesan sukses atau gagal berdasarkan hasil pengecekan

```bash
#!/bin/bash

username=""
companyname=""
pin=""

for i in {1..3}; do
    if [ "$i" -eq 1 ]; then
        echo "Enter your Username:"
        read username
    elif [ "$i" -eq 2 ]; then
        echo "Enter your Company name:"
        read companyname
    else
        echo "Enter your PIN:"
        read pin
    fi
done

if [ "$username" = "John" ] && [ "$companyname" = "Tryhackme" ] && [ "$pin" = "7385" ]; then
    echo "Authentication Successful. You can now access your locker, John."
else
    echo "Authentication Denied!!"
fi
```

---

## 7. Practical Exercise: Flag Hunt Script

Script `flag_hunt.sh` mencari kata kunci tertentu di semua file `.log` dalam direktori yang ditentukan, menggunakan `grep` di dalam loop.

Langkah pengerjaan:
1. Jadi root user dengan `sudo su` untuk akses penuh ke direktori sistem
2. Buka script dan isi variabel `directory` dan `flag` yang masih kosong
3. Pastikan tidak ada spasi yang tidak disengaja di dalam tanda kutip
4. Beri permission eksekusi dan jalankan script

```bash
sudo su                              # jadi root user
nano /home/user/flag_hunt.sh        # edit script
chmod +x /home/user/flag_hunt.sh    # beri izin eksekusi
./flag_hunt.sh                      # jalankan script
```

Pola umum script pencarian flag:

```bash
#!/bin/bash

directory="/var/log"
flag="thm-flag01-script"

echo "Flag search in directory: $directory in progress ..."

for file in "$directory"/*.log; do
    if grep -q "$flag" "$file"; then
        echo "Flag found in: $(basename "$file")"
    fi
done
```

`grep -q` — mode quiet, tidak mencetak hasil tapi mengembalikan status benar/salah untuk dipakai di kondisi `if`.

`$(basename "$file")` — mengambil nama file saja tanpa path lengkapnya.

---

## 8. Kesalahan Umum dan Solusinya

| Kesalahan | Penyebab | Solusi |
|---|---|---|
| `bad interpreter: No such file or directory` | Shebang salah, misal `#!bin/bash` tanpa `/` | Ganti menjadi `#!/bin/bash` |
| `command not found` di dalam `if` | Tidak ada spasi di dalam `[ ]` | Pastikan ada spasi setelah `[` dan sebelum `]` |
| Script tidak jalan | Belum diberi permission eksekusi | Jalankan `chmod +x namafile.sh` |
| Variabel loop salah | Pakai `$1`/`$2` padahal variabelnya `i` | Ganti dengan `$i` |
| Direktori tidak terbaca | Ada spasi tidak disengaja di dalam tanda kutip | Hapus spasi ekstra dalam nilai variabel |

---

## 9. Glossary

**Shell** — program antarmuka yang menerima dan memproses perintah teks dari pengguna ke sistem operasi.

**CLI** — Command Line Interface; antarmuka berbasis teks untuk berinteraksi dengan OS.

**GUI** — Graphical User Interface; antarmuka berbasis visual dengan elemen klik.

**Bash** — Bourne Again Shell; shell default di sebagian besar distro Linux.

**Fish** — Friendly Interactive Shell; shell yang fokus pada kemudahan dan user-friendliness.

**Zsh** — Z Shell; shell modern yang menggabungkan fitur dari berbagai shell sebelumnya.

**Shebang** — baris pertama script (`#!/bin/bash`) yang menentukan interpreter yang digunakan.

**Variabel** — wadah penyimpan nilai yang bisa dipanggil berulang kali dengan `$namaVariabel`.

**Loop** — struktur yang mengulang blok kode sejumlah iterasi yang ditentukan.

**Conditional Statement** — struktur yang menjalankan kode hanya jika kondisi tertentu terpenuhi.

**`elif`** — else if; kondisi tambahan yang dicek jika kondisi `if` pertama tidak terpenuhi.

**Tab completion** — fitur yang melengkapi perintah secara otomatis saat tombol Tab ditekan.

**PATH** — environment variable yang berisi daftar direktori tempat shell mencari program/perintah.

**`chmod +x`** — perintah untuk memberi izin eksekusi pada sebuah file.

**`./`** — notasi untuk menjalankan file dari direktori aktif saat ini.

**`sudo su`** — perintah untuk berpindah menjadi root user (superuser dengan akses penuh).

**Brace expansion** — sintaks `{1..10}` untuk mendefinisikan range angka di Bash.

**`-eq`** — operator perbandingan angka di dalam `[ ]`.

**`&&`** — operator logika AND; semua kondisi harus benar agar blok dieksekusi.

**`grep -q`** — mode quiet pada grep; tidak mencetak output, hanya mengembalikan status benar/salah.

**`basename`** — perintah untuk mengambil nama file saja dari path lengkap.

**oh-my-zsh** — framework populer untuk kustomisasi Zsh secara ekstensif.

---

## 10. Tools & Platform Rujukan

**TryHackMe** — platform pembelajaran cybersecurity berbasis lab interaktif tempat room ini berada. URL: https://tryhackme.com

**GNU nano** — teks editor terminal sederhana yang digunakan sepanjang room untuk membuat dan mengedit file script. Tersedia secara default di sebagian besar distro Linux.

---

## 11. Catatan Ringkas untuk Ditulis Tangan

### Shell & CLI

- Shell — program penerima perintah teks ke OS
- CLI vs GUI — CLI lebih efisien & ringan, GUI lebih mudah
- Bash = Bourne Again Shell, default di kebanyakan Linux
- Fish = Friendly Interactive Shell, paling user-friendly
- Zsh = Z Shell, modern, kustomisasi luas via oh-my-zsh

### Perintah Dasar

- `pwd` — tampilkan direktori aktif saat ini
- `cd nama` — pindah ke direktori
- `cd ..` — mundur satu level
- `ls` — tampilkan isi direktori
- `cat file` — tampilkan isi file
- `grep kata file` — cari kata di dalam file
- `echo $SHELL` — lihat shell aktif
- `cat /etc/shells` — lihat semua shell yang terinstal
- `chsh -s /usr/bin/zsh` — ubah shell default permanen

### Script

- Ekstensi script Bash: `.sh`
- Buat file: `nano nama.sh`
- Shebang wajib di baris pertama: `#!/bin/bash`
- Beri izin: `chmod +x nama.sh`
- Jalankan: `./nama.sh`
- `./` wajib — tanpanya shell cari di PATH, tidak ketemu

### Variabel

- Definisi: `nama="nilai"` atau `nama=""`
- Input dari user: `read nama`
- Panggil: `$nama`
- Kutip ganda `" "` — variabel diproses
- Kutip tunggal `' '` — variabel TIDAK diproses

### Loop

- `for i in {1..10}; do` — iterasi angka 1-10
- `do` — buka blok loop
- `done` — tutup blok loop
- `{1..10}` — brace expansion (range angka)

### Conditional

- `if [ kondisi ]; then` — mulai kondisi
- `elif [ kondisi ]; then` — kondisi tambahan
- `else` — jika semua kondisi gagal
- `fi` — WAJIB tutup blok if (if dibalik)
- `[ ]` adalah PERINTAH — wajib spasi dalam dan luar
- `=` — bandingkan teks/string
- `-eq` — bandingkan angka
- `&&` — AND, semua kondisi harus benar

### Komentar

- `#` — komentar, diabaikan Bash
- `#!` — pengecualian: shebang, bukan komentar

### Root User

- `sudo su` — jadi root user
- Root user punya akses ke semua direktori sistem

### grep dalam script

- `grep -q "kata" "$file"` — cari kata, mode quiet (untuk kondisi if)
- `$(basename "$file")` — ambil nama file tanpa path

### Kesalahan Umum

- `bad interpreter` — shebang salah, cek `#!/bin/bash` (ada `/` di depan `bin`)
- `command not found` di if — tidak ada spasi di `[ ]`
- Script tidak jalan — belum `chmod +x`
- Variabel loop salah — pastikan pakai `$i` bukan `$1`/`$2`
