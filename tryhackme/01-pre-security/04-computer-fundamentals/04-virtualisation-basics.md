# Resume Materi — Virtualization
**TryHackMe | 29 Agustus 2026**

---

## 1. Mengapa Virtualisasi Ada

Sebelum virtualisasi dikenal, standar industri IT adalah **"One Server = One Application"** — setiap aplikasi berjalan di server fisik yang berdedikasi. Pendekatan ini menimbulkan empat masalah utama:

- **High Cost** — Biaya tidak hanya dari hardware, tapi juga listrik, pendinginan, perawatan, dan ruang di data center.
- **Low Utilization** — Sebagian besar server hanya terpakai 5–20% dari kapasitasnya. CPU, RAM, dan storage mayoritas menganggur.
- **Slow Deployment** — Menyiapkan server fisik baru bisa memakan waktu berhari-hari hingga berminggu-minggu.
- **Hard to Scale** — Ketika aplikasi butuh lebih banyak resource, satu-satunya opsi adalah membeli server baru.

**Virtualisasi** hadir untuk menjawab semua masalah itu dengan memungkinkan satu server fisik menjalankan banyak komputer virtual secara bersamaan, masing-masing terisolasi satu sama lain.

---

## 2. Komponen Utama Virtualisasi

### 2.1 Hypervisor

**Hypervisor** adalah perangkat lunak inti yang menciptakan dan mengelola virtual machines. Ia bertindak sebagai lapisan antara hardware fisik dan VM yang berjalan di atasnya.

Fungsi hypervisor:
- Membagi satu komputer fisik menjadi beberapa komputer virtual.
- Mengalokasikan CPU, RAM, dan storage ke masing-masing VM secara terpisah.
- Menjaga isolasi antar VM sehingga satu VM tidak bisa mengganggu VM lain.
- Mengelola lifecycle VM: start, stop, pause, clone, dan delete.

Hypervisor dibagi menjadi dua tipe:

**Type 1 (Bare-Metal)**

Berjalan langsung di atas hardware fisik tanpa memerlukan OS host di bawahnya. Karena tidak ada lapisan OS tambahan, Type 1 lebih cepat dan efisien. Digunakan di lingkungan server profesional dan data center.

Contoh: VMware ESXi, Microsoft Hyper-V, Proxmox.

**Type 2 (Hosted)**

Berjalan di atas OS yang sudah ada (seperti Windows atau Linux). Lebih mudah dipasang dan dikonfigurasi, cocok untuk keperluan belajar, testing, dan setup skala kecil seperti home lab.

Contoh: Oracle VirtualBox, VMware Workstation.

**Panduan pemilihan tipe hypervisor:**

| Use Case | Tipe yang Tepat |
|---|---|
| Production Server | Type 1 |
| Database Server | Type 1 |
| Data Center | Type 1 |
| Test Malicious Files | Type 2 |
| Software Testing | Type 2 |
| Kali Linux (lab) | Type 2 |

Catatan penting untuk testing malicious files: pastikan mesin guest diisolasi penuh dari host. Cara yang direkomendasikan adalah menggunakan OS yang berbeda antara host dan guest, atau memutus koneksi jaringan mesin guest agar malware tidak menyebar.

---

### 2.2 Virtual Machine (VM)

**Virtual Machine** adalah komputer virtual yang dibuat oleh hypervisor. Meskipun bersifat virtual, setiap VM berperilaku seperti mesin fisik sungguhan karena memiliki:

- CPU virtual, RAM, storage, dan network interface sendiri.
- Sistem operasi yang berjalan secara penuh dan independen (Windows, Linux, macOS, dll).
- Isolasi total dari VM lain — jika satu VM crash, VM lainnya tidak terpengaruh.

VM berguna dalam skenario seperti:
- Butuh menjalankan OS tertentu (misal Kali Linux) tanpa membeli perangkat baru.
- Ingin menguji file yang dicurigai berbahaya tanpa risiko menginfeksi mesin utama.

---

### 2.3 Container

**Container** adalah lingkungan ringan dan terisolasi yang dirancang untuk menjalankan satu aplikasi beserta seluruh dependensinya (library, tools, versi runtime).

Perbedaan mendasar container dari VM: container tidak membawa sistem operasi penuh. Sebaliknya, container **berbagi kernel** dari OS host yang sudah berjalan. Kernel adalah bagian inti OS yang berkomunikasi langsung dengan hardware dan mengelola resource seperti memori dan proses yang berjalan.

Karena berbagi kernel, container:
- Start hampir instan (hitungan detik, bukan menit).
- Menggunakan resource jauh lebih sedikit dari VM.
- Tetap terisolasi satu sama lain — satu container bermasalah tidak mempengaruhi container lain.
- Bisa berjalan konsisten di mesin mana pun yang menjalankan OS yang kompatibel.

**Batasan penting:** Container harus cocok dengan tipe kernel host-nya. Container berbasis Linux tidak bisa berjalan di atas host Windows, dan sebaliknya.

Container ideal untuk: development, testing, dan scalable deployment.

**Docker** adalah platform open-source paling populer untuk membangun, mendistribusikan, dan menjalankan container. Docker menyederhanakan seluruh alur kerja containerisasi menjadi perintah-perintah yang mudah dipakai.

---

## 3. Perbandingan VM vs Container

| Aspek | Virtual Machine | Container |
|---|---|---|
| OS | Membawa OS lengkap sendiri | Berbagi kernel host |
| Isolasi | Maksimal | Sedang |
| Boot speed | Menit | Detik |
| Konsumsi resource | Berat | Ringan |
| Cocok untuk | Full isolation, multi-OS | Scalable, fast deployment |

---

## 4. Struktur Layer Virtualisasi

Hierarki komponen dari bawah ke atas:

```
Physical Server
  └── Hypervisor
        ├── Virtual Machine A
        └── Virtual Machine B
              ├── Container A
              └── Container B
```

Container berjalan di dalam VM. VM berjalan di atas hypervisor. Hypervisor berjalan di atas server fisik. Ketiganya bisa digunakan secara bersamaan — tidak harus memilih salah satu.

---

## 5. Manajemen Virtual Machine

### 5.1 Dashboard Virtualization Manager

Dalam praktik nyata, VM dikelola melalui platform manajemen terpusat. Umumnya platform tersebut memiliki tiga bagian utama:

- **Summary** — Tampilan umum kondisi keseluruhan lingkungan virtual.
- **Virtual Machines** — Daftar detail setiap VM beserta opsi aksi yang bisa dijalankan.
- **Hosts** — Statistik penggunaan resource dan performa tiap server fisik.

### 5.2 Status VM

| Status | Arti |
|---|---|
| Running | VM aktif berjalan normal |
| Stopped | VM tidak berjalan |
| Error | VM mengalami kesalahan, perlu direstart |

### 5.3 Aksi pada VM

Setiap VM biasanya memiliki tombol aksi berikut:

- **Start** — Menjalankan VM yang sedang stopped.
- **Stop** — Menghentikan VM yang sedang berjalan.
- **Restart / Rerun** — Merestart VM, biasanya dipakai untuk menyelesaikan VM yang masuk status Error.
- **Delete** — Menghapus VM secara permanen.

### 5.4 Membuat VM Baru

Saat membuat VM baru, parameter yang wajib diisi:

- **Name** — Nama identifikasi VM.
- **CPU Cores** — Jumlah core CPU virtual yang dialokasikan.
- **Memory (GB)** — Kapasitas RAM virtual.
- **Disk Size (GB)** — Kapasitas storage virtual.

---

## 6. Monitoring Server Fisik (Host)

Selain mengelola VM, administrator bertanggung jawab memantau kesehatan server fisik yang menjadi fondasi seluruh lingkungan virtual.

Metrik yang dipantau untuk setiap host:
- CPU Usage
- Memory Usage
- Storage Usage
- Jumlah VM yang berjalan di host tersebut
- Status koneksi (Connected / Disconnected)

**Panduan interpretasi:**

- Usage di bawah ~80% — kondisi normal, masih ada kapasitas.
- Usage mendekati atau di atas 90% — kondisi kritis, perlu segera dilaporkan dan ditindaklanjuti karena berisiko menyebabkan kegagalan sistem.
- Status Disconnected dengan usage 0% — server tidak berjalan sama sekali.

Ketika satu host sudah hampir penuh, VM baru sebaiknya ditempatkan di host lain yang masih punya kapasitas cukup. Penempatan VM ke host yang tepat adalah bagian penting dari manajemen kapasitas.

---

## 7. Manfaat Utama Virtualisasi

- **Cost Savings** — Satu server fisik menggantikan puluhan server terpisah; biaya hardware, listrik, dan ruang data center berkurang drastis.
- **Better Resource Usage** — Resource fisik dimanfaatkan secara maksimal, bukan terbuang di 5–20% utilization.
- **Safe Testing** — VM terisolasi memungkinkan pengujian malware atau exploit tanpa risiko ke sistem utama.
- **Faster Deployment** — VM atau container baru bisa dibuat dalam hitungan menit, bukan hari.
- **Flexibility** — Satu mesin fisik bisa menjalankan berbagai OS sekaligus sesuai kebutuhan.
- **Portability** — VM dan container bisa dipindahkan antar host dengan relatif mudah.
- **Scalability** — Resource bisa ditambah atau dikurangi sesuai kebutuhan beban kerja.
- **Centralized Management** — Seluruh VM dan host dikelola dari satu platform yang sama.

---

## 8. Glossary

**Virtualization** — Teknologi yang memungkinkan satu server fisik menjalankan banyak komputer virtual secara bersamaan.

**Hypervisor** — Software yang menciptakan, menjalankan, dan mengelola virtual machines.

**Virtual Machine (VM)** — Komputer virtual lengkap dengan OS sendiri, CPU, RAM, dan storage virtual.

**Container** — Lingkungan ringan dan terisolasi untuk satu aplikasi yang berbagi kernel OS host.

**Container Image** — Template atau blueprint yang sudah dikemas sebelumnya dan digunakan untuk membuat container baru.

**Kernel** — Komponen inti OS yang berkomunikasi langsung dengan hardware dan mengelola resource sistem.

**Docker** — Platform open-source untuk membangun, mendistribusikan, dan menjalankan container.

**Host** — Server fisik yang menjalankan hypervisor dan menjadi fondasi VM-VM di atasnya.

**Guest** — VM yang berjalan di atas hypervisor, berkebalikan dari host.

**Bare-Metal** — Mengacu pada software yang berjalan langsung di atas hardware tanpa OS perantara.

**Snapshot** — Rekaman kondisi VM pada titik waktu tertentu; bisa digunakan untuk rollback jika terjadi masalah.

**Network Port** — Titik masuk bernomor yang digunakan aplikasi untuk berkomunikasi melalui jaringan.

**Scalability** — Kemampuan sistem untuk tumbuh atau menyusut mengikuti kebutuhan beban kerja.

**Lifecycle (VM)** — Keseluruhan siklus hidup sebuah VM: mulai dari create, start, stop, pause, clone, hingga delete.

---

## 9. Tools & Platform Rujukan

**Oracle VirtualBox**
Hypervisor Type 2 open-source. Digunakan untuk menjalankan VM di atas OS host yang sudah ada; cocok untuk home lab dan pembelajaran.
URL: https://www.virtualbox.org

**VMware Workstation**
Hypervisor Type 2 komersial dari VMware. Alternatif VirtualBox dengan fitur lebih lengkap untuk environment testing dan development.
URL: https://www.vmware.com/products/workstation-pro.html

**Docker**
Platform open-source untuk containerisasi. Digunakan untuk membangun, mendistribusikan, dan menjalankan container secara konsisten di berbagai mesin.
URL: https://www.docker.com

**VMware ESXi**
Hypervisor Type 1 bare-metal dari VMware. Digunakan di lingkungan server profesional dan data center production.
URL: https://www.vmware.com/products/esxi-and-esx.html

**Proxmox VE**
Hypervisor Type 1 open-source berbasis Linux. Mendukung VM (KVM) dan container (LXC) dalam satu platform manajemen terpadu.
URL: https://www.proxmox.com
