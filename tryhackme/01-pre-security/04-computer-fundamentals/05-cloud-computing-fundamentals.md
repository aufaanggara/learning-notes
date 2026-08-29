# Resume Materi — Cloud Computing (TryHackMe)
**Tanggal:** 29 Agustus 2026

---

## 1. Konsep Dasar Cloud Computing

**Cloud computing** adalah model penggunaan sumber daya komputasi (server, storage, jaringan, aplikasi) melalui internet, tanpa harus memiliki atau mengelola infrastruktur fisik sendiri.

Masalah yang diselesaikan cloud:

- Pengguna dari lokasi jauh mengalami lag karena server hanya ada di satu negara
- Tidak mampu menampung banyak pengguna secara bersamaan
- Jika komputer mati, aplikasi ikut mati

Cloud dibangun di atas dua teknologi fondasi yang saling mendukung:

- **Virtualization** — memungkinkan satu mesin fisik menjalankan banyak sistem virtual secara bersamaan
- **Containers** — unit ringan untuk menjalankan aplikasi secara terisolasi, memudahkan pembuatan dan perubahan environment dengan cepat

---

## 2. Evolusi Server Menuju Cloud

| Era | Periode | Karakteristik |
|---|---|---|
| Physical Servers Era | 1960s – Early 2000s | 1 server = 1 job, mahal, lambat dikembangkan |
| Virtualization | 1999 – 2006 | Banyak VM di 1 mesin, hardware lebih efisien, provisioning lebih cepat |
| Automation & Remote Management | 2003 – 2006 | Server dikelola via internet, awal otomatisasi, infrastruktur makin fleksibel |
| Cloud Computing / AWS Launch | 2006 | Sewa VM & storage sesuai kebutuhan, tidak perlu punya hardware, elastic scaling |
| Modern Cloud Era | 2012 – Sekarang | AWS/Azure/GCP, fokus pada apps bukan server, containers, platform berskala global |

Pola yang selalu berulang di setiap transisi: bisnis mencari cara mengurangi biaya, menggunakan resource lebih efisien, dan membuat aplikasi lebih mudah dijalankan serta dikembangkan.

---

## 3. Manfaat dan Karakteristik Cloud

- **Scalability** — kapasitas naik atau turun otomatis sesuai kebutuhan aplikasi
- **On-demand self-service** — buat atau hapus server dan storage secara instan tanpa menunggu hardware fisik
- **Pay only for what you use** — biaya berdasarkan pemakaian nyata, bukan biaya tetap di muka
- **Security** — penyedia cloud melindungi infrastruktur fisik dengan langkah keamanan yang kuat
- **High availability** — aplikasi tetap berjalan meskipun sebagian komponen sistem mengalami kegagalan
- **Global access** — aplikasi dapat diakses pengguna dari seluruh dunia tanpa hambatan lokasi

---

## 4. Tipe Deployment Cloud

### 4.1 Public Cloud

Infrastruktur digunakan bersama oleh banyak pengguna dan perusahaan melalui internet.

Cocok untuk startup, website publik, dan aplikasi global karena terjangkau, mudah dikembangkan, dan tidak membutuhkan manajemen infrastruktur sendiri. Merupakan pilihan yang sesuai untuk hampir semua use case.

### 4.2 Private Cloud

Infrastruktur dibangun dan digunakan eksklusif oleh satu organisasi.

Cocok untuk bank, layanan kesehatan, dan instansi pemerintah karena menawarkan kontrol penuh, kustomisasi tinggi, dan kepatuhan terhadap regulasi data sensitif.

### 4.3 Hybrid Cloud

Kombinasi public cloud dan private cloud yang bekerja bersama dan dapat saling berbagi data.

Cocok untuk perusahaan e-commerce atau enterprise yang perlu menjaga data sensitif tetap di private cloud, sambil memanfaatkan public cloud untuk menangani lonjakan traffic saat permintaan tinggi.

---

## 5. Model Layanan Cloud

### 5.1 IaaS — Infrastructure as a Service

Kamu menyewa sumber daya komputasi dasar: server virtual, storage, dan jaringan.

- **Kamu kelola:** sistem operasi dan aplikasi
- **Provider kelola:** hardware fisik

Analogi: menyewa **apartemen kosong** — kamu sendiri yang pilih furnitur, pasang peralatan, dan urus perawatan di dalam.

### 5.2 PaaS — Platform as a Service

Provider mengelola infrastruktur dan sistem operasi. Kamu cukup fokus membangun, men-deploy, dan menjalankan aplikasi.

- **Kamu kelola:** kode aplikasi dan konfigurasi
- **Provider kelola:** infrastruktur + OS

Analogi: menyewa **apartemen semi-furnished** — fasilitas dasar sudah tersedia, tinggal fokus pada kehidupan sehari-hari tanpa harus pasang semua infrastruktur dari nol.

### 5.3 SaaS — Software as a Service

Kamu menggunakan aplikasi lengkap yang sudah jadi melalui browser atau aplikasi. Provider mengelola segalanya.

- **Kamu:** cukup akses dan pakai
- **Provider kelola:** seluruh stack (infra, OS, aplikasi, data)
- Contoh: Gmail, Zoom

Analogi: **menginap di hotel** — semua sudah disiapkan, kebersihan dan perawatan ditangani, kamu tinggal menikmati.

---

## 6. Terminologi AWS yang Perlu Dipahami

**EC2 (Elastic Compute Cloud)** adalah layanan komputer virtual (server virtual) milik Amazon di cloud. Setiap instans EC2 memiliki CPU, RAM, dan bisa menjalankan aplikasi, persis seperti komputer fisik biasa.

**Instance Type** mendeskripsikan seberapa powerful sebuah VM. Semakin besar instance type, semakin banyak CPU dan RAM yang dimiliki, semakin tinggi pula biayanya.

Prinsip pemilihan instance type:

- Instance lebih besar = lebih powerful + biaya lebih tinggi
- Instance lebih kecil = less powerful + biaya lebih rendah

**Region** adalah lokasi geografis tempat server/resource berada, misalnya `us-east-1` (North Virginia) atau wilayah Eropa. Pilihan region berpengaruh pada latensi akses pengguna.

---

## 7. Praktik Deploy: Simulasi Cloud Environment

### 7.1 Konteks dan Alasan Pakai IaaS

Untuk meng-host aplikasi pelatihan keamanan siber, digunakan model **IaaS** karena praktik cyber security membutuhkan akses penuh ke sistem operasi — untuk menginstal tools, mengonfigurasi sistem, dan mensimulasikan serangan dan pertahanan secara bebas.

### 7.2 VM yang Dibuat

| Nama | Instance Type | Biaya/Bulan | Fungsi |
|---|---|---|---|
| application-interface | t3.micro | 10 kredit | Antarmuka aplikasi utama |
| study-machine-1 | m5.large | 70 kredit | Mesin latihan cyber security |
| study-machine-2 | m5.large | 70 kredit | Mesin latihan cyber security |

Total saat semua VM running: **170 kredit/bulan**

### 7.3 Optimasi Biaya

Karena aplikasi masih dalam tahap development dan pengguna belum aktif menggunakan platform, study-machine-1 dan study-machine-2 tidak perlu terus berjalan. Dengan men-stop kedua VM tersebut, biaya turun drastis:

- VM berstatus **stopped** tidak dikenakan biaya sama sekali
- Total setelah optimasi: **30 kredit/bulan** (turun dari 170)

Ini mendemonstrasikan salah satu keunggulan utama cloud dibanding infrastruktur fisik: fleksibilitas pengelolaan biaya secara real-time hanya dengan menghentikan resource yang tidak dipakai.

---

## 8. Vendor Cloud Utama

**AWS (Amazon Web Services)** adalah pemimpin industri dengan penawaran layanan paling luas dan jangkauan global terbesar. Paling populer karena mendukung bisnis dari semua ukuran.

Vendor lain yang perlu dikenali:

- **Microsoft Azure** — kuat di lingkungan enterprise dan hybrid cloud
- **Google Cloud Platform (GCP)** — unggul di data analytics, AI, dan machine learning
- **Alibaba Cloud** — pemain dominan di Asia dengan layanan kompetitif secara global
- **IBM Cloud** — fokus pada hybrid cloud dan solusi berbasis AI untuk bisnis
- **Oracle Cloud** — fokus pada aplikasi enterprise dan database

### 8.1 Contoh Penggunaan Cloud oleh Perusahaan Besar

- **Netflix** menjalankan seluruh platformnya di AWS agar bisa scale secara global, tetap online saat permintaan puncak, dan streaming ke jutaan pengguna secara bersamaan
- **Spotify** menggunakan cloud untuk menangani jutaan lagu dan pengguna, scale dengan cepat saat ada rilis musik atau fitur baru
- **Instagram** mengandalkan cloud untuk menyimpan foto dan video dalam jumlah masif dan mengirimkannya dengan cepat ke pengguna di seluruh dunia
- **Toko online** menggunakan cloud untuk menangani lonjakan traffic saat Black Friday tanpa harus membeli infrastruktur permanen yang hanya dipakai sesekali

Kesamaan alasan: cloud memungkinkan mereka **scale dengan mudah, mengurangi biaya, tetap andal, dan fokus mengembangkan produk** alih-alih mengurus hardware.

---

## 9. Glossary — Istilah Wajib Hapal

| Istilah | Definisi |
|---|---|
| **Instance** | Satu unit VM/server virtual yang berjalan di cloud |
| **Scalability** | Kemampuan sistem untuk naik atau turun kapasitas sesuai kebutuhan |
| **Elastic** | Bisa berkembang dan menyusut secara otomatis sesuai beban |
| **Provisioning** | Proses menyiapkan dan mengaktifkan server atau resource cloud |
| **Shared Infrastructure** | Infrastruktur fisik yang digunakan bersama oleh banyak pengguna cloud |
| **High Availability** | Sistem tetap berjalan meskipun ada komponen yang mengalami kegagalan |
| **Pay-as-you-go** | Model bayar berdasarkan penggunaan nyata, bukan biaya tetap |
| **Deployment** | Proses meluncurkan dan menjalankan aplikasi ke server/environment |
| **Region** | Lokasi geografis tempat resource cloud berada |
| **EC2** | Layanan komputer virtual (server virtual) dari Amazon Web Services |
| **Instance Type** | Kategori yang mendeskripsikan spesifikasi power sebuah VM (CPU, RAM) |
| **Stopped VM** | VM yang dimatikan sementara dan tidak dikenakan biaya selama stopped |
| **Billing** | Sistem penagihan berdasarkan resource yang sedang berjalan (running) |
| **IaaS** | Model layanan cloud di mana pengguna menyewa infrastruktur dasar |
| **PaaS** | Model layanan cloud di mana platform sudah disiapkan, pengguna fokus ke aplikasi |
| **SaaS** | Model layanan cloud di mana pengguna langsung pakai software jadi via internet |
| **Public Cloud** | Cloud yang digunakan bersama oleh banyak pengguna via internet |
| **Private Cloud** | Cloud eksklusif untuk satu organisasi, kontrol dan keamanan lebih tinggi |
| **Hybrid Cloud** | Gabungan public dan private cloud yang dapat bekerja bersama |

---

## 10. Tools & Platform Rujukan

| Nama | Fungsi |
|---|---|
| **AWS (Amazon Web Services)** | Platform cloud terbesar, layanan IaaS/PaaS/SaaS — aws.amazon.com |
| **Microsoft Azure** | Platform cloud enterprise dan hybrid dari Microsoft — azure.microsoft.com |
| **Google Cloud Platform** | Platform cloud Google, unggul di AI dan data analytics — cloud.google.com |
| **TryHackMe Cloud Console** | Simulasi console cloud mirip AWS untuk latihan deploy VM (tersedia di dalam platform TryHackMe) |
