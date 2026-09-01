# Resume Materi: Active Directory Basics
**Platform:** TryHackMe  
**Tanggal:** 1 September 2026  
**Status:** Selesai 100%

---

## 1. Konsep Dasar Active Directory

### 1.1 Windows Domain

**Windows Domain** adalah sekelompok users dan komputer yang berada di bawah administrasi satu entitas bisnis. Tujuan utamanya adalah memusatkan (centralise) manajemen jaringan ke satu repositori tunggal.

- Tanpa domain, setiap komputer harus dikonfigurasi secara manual satu per satu — tidak scalable untuk jaringan besar
- Dengan domain, semua konfigurasi, kebijakan, dan akses dikelola dari satu titik pusat

Dua keuntungan utama Windows Domain:

**Centralised Identity Management** — semua akun user di seluruh jaringan dikonfigurasi dari satu tempat (Active Directory) dengan usaha minimal.

**Managing Security Policies** — kebijakan keamanan dikonfigurasi langsung dari Active Directory dan diterapkan ke users dan komputer di seluruh jaringan.

### 1.2 Active Directory Domain Service (AD DS)

**AD DS** adalah inti dari setiap Windows Domain. Berfungsi sebagai katalog yang menyimpan informasi semua objek yang ada di jaringan — users, groups, machines, printers, shares, dan lainnya.

**Domain Controller (DC)** adalah server yang menjalankan layanan Active Directory. DC adalah komponen paling sensitif dalam jaringan karena menyimpan hashed passwords seluruh akun user.

---

## 2. Objek-Objek dalam Active Directory

### 2.1 Users

**Users** adalah objek paling umum di AD dan termasuk kategori **security principals** — artinya dapat diautentikasi oleh domain dan dapat diberikan hak akses atas resources seperti file atau printer.

Dua tipe entitas yang direpresentasikan oleh Users:

**People** — merepresentasikan orang/karyawan yang perlu mengakses jaringan.

**Services** — akun khusus untuk layanan seperti IIS atau MSSQL agar bisa berjalan. Service users hanya memiliki hak akses minimal sesuai kebutuhan layanannya saja.

### 2.2 Machines

**Machines** adalah objek yang dibuat otomatis setiap kali sebuah komputer bergabung ke domain AD. Machines juga termasuk security principals dengan akun tersendiri.

- Machine account adalah local administrator di komputer yang bersangkutan
- Tidak dimaksudkan untuk diakses manusia secara langsung
- Password machine account dirotasi otomatis dan terdiri dari 120 karakter acak
- Konvensi penamaan: nama komputer diikuti tanda `$` — contoh: komputer `DC01` → machine account `DC01$`

### 2.3 Security Groups

**Security Groups** digunakan untuk memberikan izin akses (permissions) atas resources ke sekumpulan users sekaligus. Groups juga termasuk security principals.

- Group dapat berisi users, machines, maupun group lain
- User yang ditambahkan ke group otomatis mewarisi semua hak akses group tersebut

Default security groups yang penting:

| Security Group | Fungsi |
|---|---|
| Domain Admins | Hak administratif atas seluruh domain, termasuk semua DC |
| Server Operators | Dapat mengelola DC, tidak bisa ubah keanggotaan group admin |
| Backup Operators | Dapat akses file apa pun untuk keperluan backup, mengabaikan permissions |
| Account Operators | Dapat membuat atau memodifikasi akun lain di domain |
| Domain Users | Mencakup semua akun user yang ada di domain |
| Domain Computers | Mencakup semua komputer yang ada di domain |
| Domain Controllers | Mencakup semua DC yang ada di domain |

### 2.4 Organizational Units (OUs)

**OUs** adalah container objects untuk mengklasifikasikan users dan machines. Digunakan terutama untuk menerapkan kebijakan (policies) ke sekumpulan users berdasarkan peran atau departemen mereka.

- Seorang user hanya bisa berada di satu OU pada satu waktu
- OUs biasanya mencerminkan struktur organisasi bisnis

Default containers yang dibuat Windows secara otomatis:

- **Builtin** — berisi default groups yang tersedia untuk semua Windows host
- **Computers** — semua mesin yang bergabung ke domain masuk sini secara default
- **Domain Controllers** — default OU untuk semua DC di jaringan
- **Users** — default users dan groups untuk konteks domain-wide
- **Managed Service Accounts** — menyimpan akun yang digunakan oleh services

### 2.5 Security Groups vs OUs

| Aspek | OUs | Security Groups |
|---|---|---|
| Tujuan | Menerapkan policies | Memberikan permissions atas resources |
| Keanggotaan user | Hanya satu OU | Bisa banyak groups |
| Contoh penggunaan | Kebijakan password per departemen | Akses ke shared folder atau printer |

---

## 3. Mengelola Users dan Komputer di AD

### 3.1 Active Directory Users and Computers

Tool untuk mengkonfigurasi users, groups, dan machines. Diakses dari Domain Controller melalui Start Menu → "Active Directory Users and Computers".

Dari tool ini admin dapat: membuat, menghapus, memodifikasi akun, serta mereset password.

### 3.2 Menghapus OU yang Dilindungi

Secara default, OUs dilindungi dari penghapusan tidak sengaja. Untuk menghapus OU:

1. Aktifkan **Advanced Features** di menu View
2. Klik kanan OU → Properties → tab Object
3. Uncheck **"Protect object from accidental deletion"**
4. Klik kanan OU → Delete

### 3.3 Delegation

**Delegation** adalah fitur AD yang memungkinkan pemberian kontrol spesifik kepada user tertentu atas sebuah OU tanpa perlu melibatkan Domain Administrator.

Contoh penggunaan paling umum: memberikan IT Support hak untuk mereset password users tanpa memberi mereka akses penuh ke AD.

Cara mendelegasikan kontrol:
- Klik kanan OU → **Delegate Control**
- Pilih user yang akan diberi delegasi
- Pilih task yang didelegasikan (misalnya: Reset user passwords)

Setelah didelegasikan, user tersebut bisa mereset password via PowerShell karena tidak punya akses ke GUI Active Directory Users and Computers.

Command reset password via PowerShell (dijalankan sebagai user yang sudah didelegasi):

```powershell
Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose
```

```powershell
Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose
```

Penjelasan parameter:

- `Set-ADAccountPassword` — cmdlet untuk mengubah password akun AD
- `sophie` — identity/username target
- `-Reset` — flag untuk mereset password (bukan sekadar mengubah)
- `-NewPassword` — nilai password baru, dimasukkan secara interaktif via `Read-Host`
- `-AsSecureString` — mengkonversi input menjadi secure string agar tidak tampil plain text
- `-Prompt 'New Password'` — teks yang ditampilkan saat meminta input
- `-Verbose` — menampilkan detail operasi yang sedang berjalan
- `Set-ADUser` — cmdlet untuk memodifikasi atribut akun user
- `-ChangePasswordAtLogon $true` — memaksa user untuk ganti password saat login berikutnya
- `-Identity sophie` — menentukan akun target

### 3.4 Mengorganisir Komputer

Secara default semua komputer (kecuali DC) masuk ke container **Computers**. Best practice adalah memisahkan komputer ke tiga kategori:

**Workstations** — komputer kerja sehari-hari karyawan. Tidak boleh ada privileged user yang login secara rutin di sini.

**Servers** — menyediakan layanan kepada users atau server lain.

**Domain Controllers** — mengelola Active Directory Domain. Perangkat paling sensitif karena menyimpan hashed passwords semua akun.

Cara memindahkan komputer ke OU yang sesuai: klik kanan komputer di panel kanan → **Move** → pilih OU tujuan. Bisa multi-select dengan menahan Ctrl.

---

## 4. Group Policy Objects (GPO)

### 4.1 Konsep GPO

**Group Policy Objects (GPO)** adalah kumpulan pengaturan yang dapat diterapkan ke OUs. GPO memungkinkan admin mendorong konfigurasi dan security baselines yang berbeda ke users dan komputer berdasarkan departemen atau peran mereka.

Alur kerja GPO:
1. GPO dibuat di bawah **Group Policy Objects**
2. GPO di-link ke OU yang diinginkan
3. GPO berlaku untuk semua objek di OU tersebut dan semua sub-OU di bawahnya (inheritance)

Setiap GPO memiliki dua bagian konfigurasi:

**Computer Configuration** — kebijakan berlaku pada mesin, siapapun yang login ke mesin tersebut kena.

**User Configuration** — kebijakan melekat pada user, ke mana pun user login kena kebijakan ini.

### 4.2 Security Filtering

GPO dapat dibatasi hanya berlaku pada users/komputer tertentu di dalam OU menggunakan **Security Filtering**. Secara default berlaku untuk grup **Authenticated Users** (semua users/PC).

### 4.3 GPO Distribution via SYSVOL

GPOs didistribusikan ke seluruh jaringan melalui network share bernama **SYSVOL** yang disimpan di DC. Default path: `C:\Windows\SYSVOL\sysvol\`

- Semua users domain harus punya akses ke share ini untuk sinkronisasi GPO secara berkala
- Perubahan GPO membutuhkan hingga 2 jam untuk diterapkan ke semua komputer

Untuk memaksa sinkronisasi GPO segera:

```powershell
gpupdate /force
```

- `gpupdate` — tool untuk memperbarui Group Policy
- `/force` — memaksa refresh semua GPO segera tanpa menunggu interval normal

### 4.4 Contoh GPO yang Dibuat di Lab

**Restrict Control Panel Access**
- Tujuan: memblokir akses Control Panel untuk users non-IT
- Konfigurasi: User Configuration → Policies → Administrative Templates → Control Panel → **Prohibit access to Control Panel and PC settings** → Enabled
- Di-link ke: OU Marketing, Management, dan Sales

**Auto Lock Screen**
- Tujuan: mengunci layar otomatis setelah 5 menit tidak aktif
- Konfigurasi: Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Security Options → **Interactive logon: Machine inactivity limit** → 300 seconds
- Di-link ke: root domain thm.local (agar diwarisi Workstations, Servers, dan Domain Controllers)

---

## 5. Metode Autentikasi

### 5.1 Kerberos Authentication

**Kerberos** adalah protokol autentikasi default di semua versi Windows terbaru. Berbasis sistem tiket — user yang sudah pernah autentikasi mendapat tiket sebagai bukti, sehingga tidak perlu memasukkan kredensial berulang kali.

**Key Distribution Center (KDC)** adalah layanan yang biasanya berjalan di DC, bertugas membuat dan mendistribusikan tiket Kerberos.

Alur autentikasi Kerberos (5 langkah):

**Langkah 1 — Client Request TGT:**
User mengirim username dan timestamp (dienkripsi dengan kunci dari password) ke KDC.

**Langkah 2 — KDC Response TGT:**
KDC mengirim balik **Ticket Granting Ticket (TGT)** dan **Session Key**. TGT dienkripsi dengan hash akun **krbtgt** sehingga user tidak bisa membaca isinya. TGT berisi salinan Session Key di dalamnya.

**Langkah 3 — Client Request TGS:**
Ketika ingin akses layanan tertentu, user mengirim username, timestamp (dienkripsi dengan Session Key), TGT, dan **Service Principal Name (SPN)** yang menunjukkan layanan dan server tujuan.

**Langkah 4 — KDC Response TGS:**
KDC mengirim **Ticket Granting Service (TGS)** dan **Service Session Key**. TGS dienkripsi dengan **Service Owner Hash**. TGS hanya berlaku untuk satu layanan spesifik.

**Langkah 5 — Authenticate to Service:**
Client mengirim TGS ke layanan tujuan. Layanan mendekripsi TGS menggunakan password hash akunnya sendiri dan memvalidasi Service Session Key.

Istilah kunci Kerberos:

- **TGT (Ticket Granting Ticket)** — tiket utama yang membuktikan user sudah autentikasi; digunakan untuk meminta TGS
- **TGS (Ticket Granting Service)** — tiket spesifik untuk satu layanan tertentu
- **KDC (Key Distribution Center)** — layanan di DC yang mengelola distribusi tiket
- **SPN (Service Principal Name)** — identifikasi nama layanan dan server yang ingin diakses
- **Session Key** — kunci sesi yang diberikan bersama TGT untuk request selanjutnya
- **Service Session Key** — kunci sesi khusus untuk berkomunikasi dengan layanan tertentu
- **krbtgt** — akun sistem yang hash-nya digunakan untuk mengenkripsi TGT

### 5.2 NetNTLM Authentication

**NetNTLM** adalah protokol autentikasi legacy yang dipertahankan untuk kompatibilitas. Bekerja dengan mekanisme **challenge-response** — password tidak pernah dikirim langsung melalui jaringan.

Alur autentikasi NetNTLM (6 langkah):

1. Client mengirim authentication request ke server
2. Server membuat angka acak dan mengirimkannya sebagai **challenge** ke client
3. Client menggabungkan NTLM password hash dengan challenge untuk menghasilkan **response**, lalu dikirim ke server
4. Server meneruskan challenge dan response ke Domain Controller
5. DC menghitung ulang response menggunakan challenge yang sama, lalu membandingkan dengan response dari client — jika cocok, autentikasi berhasil
6. Server meneruskan hasil autentikasi ke client

Catatan penting: jika menggunakan local account (bukan domain account), server dapat memverifikasi sendiri tanpa perlu menghubungi DC karena password hash tersimpan lokal di **SAM (Security Account Manager)**.

### 5.3 Perbandingan Kerberos vs NetNTLM

| Aspek | Kerberos | NetNTLM |
|---|---|---|
| Status | Default, modern | Legacy, untuk kompatibilitas |
| Mekanisme | Berbasis tiket | Challenge-response |
| Keamanan | Lebih aman | Rentan capture & offline cracking |
| Password di jaringan | Tidak dikirim | Tidak dikirim (hanya response) |

---

## 6. Trees, Forests, dan Trust Relationships

### 6.1 Trees

**Tree** adalah kumpulan beberapa domain yang berbagi **namespace** yang sama, digabungkan dalam satu struktur hierarkis.

Contoh: domain thm.local berkembang menjadi tree dengan subdomain uk.thm.local dan us.thm.local. Setiap subdomain memiliki DC, computers, dan users sendiri yang dikelola secara independen.

Keuntungan Tree:
- Setiap domain dikelola secara independen oleh tim IT masing-masing
- Domain Admins hanya punya kontrol atas domain mereka sendiri
- Policies dapat dikonfigurasi berbeda untuk setiap domain

**Enterprise Admins** adalah security group khusus yang memberikan hak administratif atas semua domain di seluruh enterprise — berbeda dari Domain Admins yang hanya berlaku di satu domain.

### 6.2 Forests

**Forest** adalah gabungan beberapa trees dengan namespace yang berbeda ke dalam satu jaringan yang sama.

Contoh: perusahaan THM mengakuisisi MHT Inc. THM tree (thm.local) dan MHT tree (mht.local) digabungkan menjadi satu forest. Masing-masing tree tetap dikelola oleh IT department masing-masing.

### 6.3 Trust Relationships

**Trust Relationships** adalah mekanisme yang memungkinkan users dari satu domain untuk mengakses resources di domain lain.

**One-way trust** — jika Domain AAA mempercayai Domain BBB, maka user di BBB dapat diotorisasi untuk mengakses resources di AAA. Arah trust berlawanan dengan arah akses.

**Two-way trust** — kedua domain saling mempercayai satu sama lain. Bergabungnya domain dalam satu tree atau forest secara default membentuk two-way trust.

Catatan penting: trust relationship tidak otomatis memberikan akses ke semua resources. Trust hanya membuka kemungkinan otorisasi — admin tetap harus menentukan secara eksplisit apa yang boleh diakses.

---

## 7. Glossary

**Active Directory (AD)** — repositori terpusat yang menyimpan semua informasi objek di jaringan Windows Domain.

**AD DS (Active Directory Domain Service)** — layanan inti yang menjalankan Active Directory di Windows Domain.

**Authenticated Users** — grup default yang mencakup semua users dan komputer yang sudah terautentikasi di domain.

**Challenge-Response** — mekanisme autentikasi di mana server memberi soal acak (challenge) dan client harus menjawab (response) menggunakan turunan dari password-nya.

**Delegation** — pemberian kontrol spesifik kepada user atas sebuah OU tanpa memberi mereka akses admin penuh.

**Domain** — sekelompok users dan komputer yang berada di bawah administrasi satu entitas bisnis dalam jaringan Windows.

**Domain Controller (DC)** — server yang menjalankan AD DS; otak dari seluruh domain.

**Enterprise Admins** — security group dengan hak administratif atas semua domain di seluruh enterprise/forest.

**Forest** — gabungan beberapa trees dengan namespace berbeda dalam satu jaringan.

**GPO (Group Policy Object)** — kumpulan pengaturan yang diterapkan ke OUs untuk mengontrol konfigurasi users dan komputer.

**KDC (Key Distribution Center)** — layanan di DC yang membuat dan mendistribusikan tiket Kerberos.

**Kerberos** — protokol autentikasi default Windows berbasis sistem tiket.

**krbtgt** — akun sistem yang hash-nya digunakan untuk mengenkripsi TGT.

**Machine Account** — akun AD yang dibuat otomatis untuk setiap komputer yang bergabung ke domain; diidentifikasi dengan sufiks `$`.

**namespace** — ruang nama domain (contoh: thm.local) yang menjadi identitas unik sebuah domain atau tree.

**NetNTLM** — protokol autentikasi legacy Windows berbasis challenge-response.

**OU (Organizational Unit)** — container di AD untuk mengelompokkan users dan komputer; digunakan untuk menerapkan policies.

**SAM (Security Account Manager)** — database lokal di setiap mesin Windows yang menyimpan hash password akun lokal.

**Security Filtering** — pembatasan GPO agar hanya berlaku pada users/komputer tertentu dalam sebuah OU.

**Security Principal** — objek di AD yang dapat diautentikasi dan diberikan hak akses atas resources (users, machines, security groups).

**Session Key** — kunci sesi yang diberikan bersama TGT untuk digunakan dalam request Kerberos selanjutnya.

**SPN (Service Principal Name)** — identifikasi unik layanan dan server yang ingin diakses dalam protokol Kerberos.

**SYSVOL** — network share di DC yang digunakan untuk mendistribusikan GPO ke seluruh jaringan domain.

**TGS (Ticket Granting Service)** — tiket Kerberos yang berlaku untuk mengakses satu layanan spesifik.

**TGT (Ticket Granting Ticket)** — tiket Kerberos utama yang diperoleh setelah autentikasi pertama; digunakan untuk meminta TGS tanpa harus autentikasi ulang.

**Tree** — kumpulan beberapa domain yang berbagi namespace yang sama dalam hierarki AD.

**Trust Relationship** — mekanisme yang memungkinkan users dari satu domain mengakses resources di domain lain.

**Two-way Trust** — trust relationship di mana kedua domain saling mempercayai; default saat bergabung dalam tree atau forest.

**One-way Trust** — trust relationship satu arah; user dari domain yang dipercaya dapat akses resources di domain yang mempercayai.

**Windows Domain** — jaringan yang dikelola secara terpusat melalui Active Directory dengan satu atau lebih Domain Controllers.

---

## 8. Tools & Platform Rujukan

**Active Directory Users and Computers**
- Fungsi: GUI tool untuk mengelola users, groups, machines, dan OUs di AD
- Akses: Start Menu di Domain Controller → search "Active Directory Users and Computers"

**Group Policy Management**
- Fungsi: GUI tool untuk membuat, mengedit, dan me-link GPO ke OUs
- Akses: Start Menu di Domain Controller → search "Group Policy Management"

**Group Policy Management Editor**
- Fungsi: Editor khusus untuk mengkonfigurasi isi GPO (Computer Configuration & User Configuration)
- Akses: Klik kanan GPO di Group Policy Management → Edit

**PowerShell (Windows)**
- Fungsi: Command-line interface untuk menjalankan operasi AD via cmdlet seperti Set-ADAccountPassword dan Set-ADUser
- Akses: Start Menu → Windows PowerShell (Admin)

**xfreerdp**
- Fungsi: RDP client di Linux/Kali untuk terhubung ke mesin Windows secara remote
- Instalasi: `sudo apt install freerdp3-x11`

**Active Directory Hardening Room (TryHackMe)**
- Fungsi: Room lanjutan untuk mempelajari cara mengamankan instalasi Active Directory
- Platform: TryHackMe

**Compromising Active Directory module (TryHackMe)**
- Fungsi: Modul lanjutan untuk mempelajari teknik-teknik hacking dan eksploitasi miskonfigurasi AD
- Platform: TryHackMe

**Microsoft Documentation — Default Security Groups**
- Fungsi: Referensi lengkap daftar default security groups di Active Directory
- URL: https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-groups

---

## 9. Catatan Ringkas untuk Ditulis Tangan

### Active Directory Dasar

- **Windows Domain** — sekelompok users & komputer di bawah satu administrasi terpusat
- **AD DS** — layanan inti Active Directory, berfungsi sebagai katalog semua objek jaringan
- **DC (Domain Controller)** — server yang menjalankan AD; paling sensitif di jaringan
- **Keuntungan domain** — centralised identity management + managing security policies

### Objek AD

- **Users** — security principal; bisa People (manusia) atau Services (layanan)
- **Machines** — security principal; dibuat otomatis saat komputer join domain; nama: `NamaKomputer$`
- **Machine account password** — 120 karakter acak, dirotasi otomatis
- **Security Groups** — untuk grant permissions; user bisa di banyak group
- **OUs** — untuk apply policies; user hanya di 1 OU sekaligus

### Default Containers

- Builtin — default groups
- Computers — semua mesin baru masuk sini
- Domain Controllers — DC
- Users — default users & groups
- Managed Service Accounts — akun untuk services

### Security Groups Penting

- Domain Admins — admin seluruh domain + DC
- Server Operators — kelola DC, tidak bisa ubah group admin
- Backup Operators — akses semua file untuk backup
- Account Operators — buat/modifikasi akun
- Domain Users — semua user
- Domain Computers — semua komputer
- Domain Controllers — semua DC

### Mengelola AD

- Hapus OU — aktifkan Advanced Features → View → uncheck "Protect from accidental deletion"
- **Delegation** — beri kontrol spesifik user atas OU tanpa jadi Domain Admin
- Klik kanan OU → Delegate Control → pilih user → pilih task

### PowerShell Commands

- `Set-ADAccountPassword` — reset password akun AD
  - `-Reset` — flag untuk reset
  - `-NewPassword` — password baru (via Read-Host)
  - `-AsSecureString` — input jadi secure string
  - `-Verbose` — tampilkan detail operasi
- `Set-ADUser` — modifikasi atribut user
  - `-ChangePasswordAtLogon $true` — paksa ganti password saat login
  - `-Identity` — tentukan target user
- `gpupdate /force` — paksa sinkronisasi GPO segera

### Komputer di AD

- Workstations — komputer kerja harian; tidak untuk privileged user login
- Servers — penyedia layanan
- Domain Controllers — paling sensitif, simpan semua password hash

### GPO

- GPO dibuat di Group Policy Objects → di-link ke OU
- GPO berlaku ke OU + semua sub-OU (inheritance)
- **Computer Config** — berlaku ke mesin (siapapun yang login)
- **User Config** — berlaku ke user (ke mana pun login)
- **Security Filtering** — batasi GPO ke user/komputer tertentu
- **SYSVOL** — network share untuk distribusi GPO; path: `C:\Windows\SYSVOL\sysvol\`
- Update GPO butuh hingga 2 jam; paksa dengan `gpupdate /force`

### Contoh GPO

- Restrict Control Panel — User Config → Admin Templates → Control Panel → Prohibit access → Enabled → link ke Marketing, Management, Sales
- Auto Lock Screen — Computer Config → Windows Settings → Security Settings → Local Policies → Security Options → Interactive logon: Machine inactivity limit → 300 seconds → link ke root domain

### Kerberos

- Protokol autentikasi default Windows modern; berbasis tiket
- **KDC** — Key Distribution Center; berjalan di DC; buat & distribusikan tiket
- **TGT** — Ticket Granting Ticket; tiket utama setelah autentikasi pertama
- **TGS** — Ticket Granting Service; tiket untuk layanan spesifik
- **SPN** — Service Principal Name; identifikasi layanan yang ingin diakses
- **Session Key** — kunci sesi dari TGT untuk request selanjutnya
- **krbtgt** — akun sistem; hash-nya enkripsi TGT

### Alur Kerberos (5 Langkah)

1. Client → KDC: username + timestamp terenkripsi (request TGT)
2. KDC → Client: TGT + Session Key
3. Client → KDC: username + timestamp + TGT + SPN (request TGS)
4. KDC → Client: TGS + Service Session Key
5. Client → Service: TGS (autentikasi ke layanan)

### NetNTLM

- Protokol legacy; berbasis challenge-response
- Password tidak pernah dikirim langsung di jaringan
- Rentan: jika challenge+response di-capture → bisa crack offline

### Alur NetNTLM (6 Langkah)

1. Client → Server: authentication request
2. Server → Client: challenge (angka acak)
3. Client → Server: response (hash password + challenge)
4. Server → DC: teruskan challenge + response
5. DC: hitung ulang response, bandingkan → Allow/Deny
6. Server → Client: hasil autentikasi

### Trees, Forests, Trusts

- **Tree** — beberapa domain dengan namespace yang sama (misal: thm.local, uk.thm.local, us.thm.local)
- **Forest** — beberapa trees dengan namespace berbeda dalam satu jaringan
- **Enterprise Admins** — group dengan akses admin ke semua domain di enterprise
- **One-way trust** — AAA percaya BBB → user BBB bisa akses resource AAA (trust direction berlawanan dengan access direction)
- **Two-way trust** — keduanya saling percaya; default saat join tree/forest
- Trust tidak otomatis beri akses semua resource — admin tetap harus otorisasi secara eksplisit
