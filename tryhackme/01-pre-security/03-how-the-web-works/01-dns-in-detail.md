# Resume Materi: DNS (Domain Name System)

**Tanggal:** 05 Mei 2026

---

## 1. Konsep Dasar DNS

### 1.1 Definisi dan Fungsi

**DNS (Domain Name System)** adalah sistem yang menerjemahkan nama domain yang mudah diingat manusia (misal `tryhackme.com`) menjadi **IP address** yang dipakai komputer untuk saling berkomunikasi di internet.

Setiap perangkat di internet memiliki alamat unik berupa IP address, contohnya `104.26.10.229` — empat kelompok angka 0-255 yang dipisahkan titik. Tanpa DNS, pengguna harus menghafal deretan angka ini setiap kali ingin mengakses sebuah layanan. DNS berperan sebagai penerjemah antara nama domain dan alamat numerik tersebut.

### 1.2 Analogi

DNS bekerja mirip dengan sistem alamat rumah pada layanan pos: setiap rumah punya alamat unik agar surat sampai ke tujuan yang tepat. Begitu pula komputer di internet membutuhkan IP address agar data terkirim ke tujuan yang benar, dan DNS menghilangkan kebutuhan untuk menghafal alamat numerik tersebut.

---

## 2. Hierarki Domain

Struktur domain tersusun berjenjang dari atas ke bawah, dimulai dari **root domain** hingga **subdomain**.

### 2.1 Root Domain

Root domain dilambangkan dengan simbol titik (`.`) dan berada di puncak hierarki DNS. Root domain biasanya tidak terlihat secara eksplisit saat mengetik alamat website.

### 2.2 TLD (Top-Level Domain)

TLD adalah bagian paling kanan dari sebuah nama domain, contohnya `.com` pada `tryhackme.com`. Terdapat dua jenis TLD:

**gTLD (Generic Top-Level Domain)** — awalnya dibuat untuk menunjukkan tujuan domain, misalnya `.com` untuk komersial, `.org` untuk organisasi, `.edu` untuk pendidikan, `.gov` untuk pemerintah, `.mil` untuk militer. Seiring waktu muncul ribuan gTLD baru seperti `.online`, `.club`, `.website`, `.biz`.

**ccTLD (Country Code Top-Level Domain)** — menunjukkan lokasi geografis, misalnya `.ca` untuk Kanada, `.co.uk` untuk Inggris, `.id` untuk Indonesia.

### 2.3 Second-Level Domain

Second-level domain adalah nama yang didaftarkan pengguna, contohnya `tryhackme` pada `tryhackme.com`. Batasan penamaannya:

- Maksimal 63 karakter (belum termasuk TLD)
- Hanya boleh menggunakan huruf a-z, angka 0-9, dan tanda hubung
- Tidak boleh diawali atau diakhiri tanda hubung
- Tidak boleh memiliki tanda hubung berurutan

### 2.4 Subdomain

Subdomain berada di sisi kiri second-level domain, dipisahkan dengan titik. Contohnya `admin` pada `admin.tryhackme.com`. Aturan penamaan subdomain sama dengan second-level domain (maksimal 63 karakter, hanya a-z, 0-9, dan tanda hubung).

Subdomain dapat disusun berlapis, misalnya `jupiter.servers.tryhackme.com`, dengan batas total panjang nama domain 253 karakter. Tidak ada batas jumlah subdomain yang bisa dibuat untuk satu domain.

---

## 3. Jenis-Jenis DNS Record

DNS tidak hanya melayani resolusi alamat website, tetapi memiliki beberapa jenis record dengan fungsi berbeda.

### 3.1 A Record

Meresolusi domain ke **alamat IPv4**, contohnya `104.26.10.229`. Ini adalah jenis record paling dasar dan paling umum digunakan.

### 3.2 AAAA Record

Meresolusi domain ke **alamat IPv6**, contohnya `2606:4700:20::681a:be5`. Digunakan untuk domain yang mendukung format IP versi baru ini.

### 3.3 CNAME Record

Meresolusi domain ke **domain lain**, bukan langsung ke IP address. Contoh: `store.tryhackme.com` memiliki CNAME record yang mengarah ke `shops.shopify.com`. Setelah CNAME ditemukan, sistem akan melakukan DNS request tambahan ke domain hasil CNAME tersebut untuk mendapatkan IP address final.

### 3.4 MX Record

Meresolusi ke **alamat server yang menangani email** untuk sebuah domain. Contoh: MX record `tryhackme.com` mengarah ke `alt1.aspmx.l.google.com`. Setiap MX record memiliki **priority flag** — angka yang menentukan urutan server dicoba; server dengan angka prioritas lebih kecil dicoba lebih dulu, dan sisanya berfungsi sebagai backup jika server utama tidak dapat diakses.

### 3.5 TXT Record

Field teks bebas untuk menyimpan data berbasis teks apapun. Kegunaan umum:

- Mencantumkan server yang berwenang mengirim email atas nama domain, membantu mencegah spam dan email spoofing
- Memverifikasi kepemilikan domain saat mendaftar layanan pihak ketiga

---

## 4. Alur Resolusi DNS Request

Ketika pengguna meminta sebuah domain, proses pencarian IP address berjalan melalui beberapa tahap berurutan hingga jawaban ditemukan.

### 4.1 Local Cache

Komputer pertama-tama memeriksa **cache lokalnya** untuk melihat apakah domain tersebut pernah dicari sebelumnya. Jika ditemukan, proses berhenti di sini. Jika tidak, request diteruskan ke Recursive DNS Server.

### 4.2 Recursive DNS Server

Server ini umumnya disediakan oleh **ISP (Internet Service Provider)**, meski pengguna bisa memilih server rekursif lain secara manual. Server ini juga memiliki cache lokal dari domain yang baru-baru ini dicari. Jika hasil ditemukan di cache (umum terjadi untuk situs populer seperti Google atau Facebook), jawaban langsung dikirim ke komputer pengguna. Jika tidak, pencarian berlanjut ke Root DNS Server.

### 4.3 Root DNS Server

Root server berfungsi sebagai **tulang punggung DNS**, bertugas mengarahkan request ke **TLD Server** yang sesuai. Misalnya, untuk permintaan `www.tryhackme.com`, root server mengenali TLD `.com` dan mengarahkan ke server TLD yang menangani domain `.com`.

### 4.4 TLD Server

TLD server menyimpan informasi lokasi **authoritative server (nameserver)** untuk domain yang diminta. Contoh: nameserver untuk `tryhackme.com` adalah `kip.ns.cloudflare.com` dan `uma.ns.cloudflare.com`. Umumnya terdapat lebih dari satu nameserver sebagai cadangan jika salah satunya tidak aktif.

### 4.5 Authoritative DNS Server

Server ini menyimpan **DNS record asli** dari sebuah domain dan merupakan tempat semua perubahan record dilakukan. Server ini mengembalikan record yang diminta (sesuai jenis record: A, MX, TXT, dsb) ke Recursive DNS Server, yang kemudian menyimpan salinannya di cache dan meneruskan hasilnya ke komputer pengguna.

Setiap DNS record memiliki nilai **TTL (Time To Live)** dalam satuan detik, yang menentukan berapa lama hasil tersebut boleh disimpan di cache sebelum harus dicari ulang. Mekanisme caching ini menghemat waktu karena tidak perlu melakukan DNS request penuh setiap kali domain yang sama diakses.

---

## 5. Praktik: Query DNS

Pengujian dilakukan terhadap domain `website.thm` menggunakan tool query DNS berbasis web maupun command line.

### 5.1 Command nslookup

```
nslookup website.thm
```

`nslookup` adalah utilitas command line untuk melakukan query DNS dan menampilkan record yang dikembalikan oleh server DNS.

### 5.2 Hasil Query yang Diperoleh

- **CNAME** dari `shop.website.thm` mengarah ke `shops.myshopify.com`
- **TXT record** dari `website.thm` berisi flag verifikasi/data teks
- **MX record** dari domain memiliki nilai priority tertentu yang menentukan urutan server email dicoba
- **A record** dari `www.website.thm` mengarah ke alamat IPv4 tertentu

---

## 6. Glosarium

**DNS (Domain Name System)** — sistem yang menerjemahkan nama domain menjadi IP address.

**IP address** — alamat numerik unik sebuah perangkat di internet.

**Root domain** — titik teratas hierarki DNS, dilambangkan dengan tanda titik.

**TLD (Top-Level Domain)** — bagian paling kanan nama domain, terbagi menjadi gTLD (generic) dan ccTLD (country code).

**Second-level domain** — nama domain yang didaftarkan pengguna, terletak sebelum TLD.

**Subdomain** — bagian di sisi kiri second-level domain, dipisahkan titik, digunakan untuk memisahkan layanan dalam satu domain.

**A record** — DNS record yang meresolusi domain ke alamat IPv4.

**AAAA record** — DNS record yang meresolusi domain ke alamat IPv6.

**CNAME record** — DNS record yang meresolusi domain ke domain lain (alias).

**MX record** — DNS record yang menunjuk ke server penanganan email suatu domain, dilengkapi nilai priority.

**TXT record** — DNS record berisi data teks bebas, dipakai untuk verifikasi domain dan konfigurasi anti-spam.

**TTL (Time To Live)** — durasi dalam detik suatu DNS record boleh disimpan di cache sebelum harus dicari ulang.

**Recursive DNS Server** — server (umumnya milik ISP) yang menjalankan proses pencarian penuh atas nama pengguna hingga menemukan jawaban.

**Root DNS Server** — server yang mengarahkan request ke TLD server yang sesuai.

**TLD Server** — server yang menyimpan informasi lokasi nameserver (authoritative server) suatu domain.

**Authoritative DNS Server / Nameserver** — server yang menyimpan DNS record asli suatu domain dan menjadi tempat perubahan record dilakukan.

**Cache** — penyimpanan sementara hasil DNS lookup, baik di komputer pengguna maupun di recursive DNS server, untuk mempercepat request berikutnya.

**nslookup** — command line tool untuk melakukan query DNS dan menampilkan hasil record.

---

## 7. Tools & Platform Rujukan

- **nslookup** — command line tool bawaan OS untuk melakukan query DNS records secara manual.

---

## 8. Catatan Ringkas untuk Ditulis Tangan

**Definisi**
- DNS — terjemahkan nama domain jadi IP address
- IP address — 4 angka 0-255 dipisah titik, alamat unik perangkat

**Hierarki Domain**
- Root domain — titik paling atas, simbol "."
- TLD — bagian paling kanan domain (.com, .id, .gov)
  - gTLD — generic (.com, .org, .edu, .gov, .mil)
  - ccTLD — country code (.id, .uk, .ca)
- Second-level domain — nama yang didaftarkan (max 63 karakter, a-z 0-9 -)
- Subdomain — kiri second-level domain, dipisah titik, bisa berlapis, total max 253 karakter

**Jenis DNS Record**
- A — domain ke IPv4
- AAAA — domain ke IPv6
- CNAME — domain ke domain lain (alias)
- MX — domain ke mail server, ada priority (angka kecil = prioritas tinggi)
- TXT — teks bebas, verifikasi domain & anti-spam

**Alur Resolusi DNS (5 tahap)**
1. Local cache komputer — cek dulu, kalau ada selesai
2. Recursive DNS Server (dari ISP) — cek cache, kalau tidak ada lanjut cari
3. Root DNS Server — arahkan ke TLD server yang sesuai
4. TLD Server — kasih tahu lokasi nameserver (authoritative)
5. Authoritative DNS Server — simpan record asli, kasih jawaban final, hasil di-cache di recursive server

**Istilah Kunci**
- TTL — lama record boleh di-cache (detik)
- Cache — simpanan sementara hasil lookup
- Nameserver — authoritative DNS server

**Command**
- nslookup [domain] — query DNS record dari command line
