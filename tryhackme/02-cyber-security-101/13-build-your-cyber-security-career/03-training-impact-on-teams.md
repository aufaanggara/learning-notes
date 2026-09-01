# Resume: Cyber Security Training

**Tanggal:** 1 September 2026

---

## 1. Mengapa Training Penting

### 1.1 Training sebagai Fondasi Skill

Tidak ada orang yang lahir langsung menjadi ahli cyber security. Skill harus dibangun lewat latihan dan pembelajaran berkelanjutan. Praktik cyber security memiliki kebutuhan khusus: umumnya memerlukan lab environment terpisah agar eksperimen tidak berdampak ke sistem produksi (sistem yang benar-benar dipakai secara live). Ini menjadikan training platform elemen penting dalam pengembangan skill seorang praktisi.

Prinsip "mencegah lebih baik daripada mengobati" berlaku di sini: belajar di lingkungan training jauh lebih aman dan efektif dibanding belajar di tengah insiden nyata. Training yang baik juga meningkatkan kualitas kerja tim dan menurunkan kemungkinan terjadinya insiden.

### 1.2 Dampak Training terhadap Organisasi

Training meningkatkan kapasitas tim tanpa harus menambah headcount, karena tim menjadi siap menghadapi ancaman tanpa harus belajar konsep baru di tengah insiden yang sedang berlangsung. Manfaat lainnya:

- Mempermudah proses onboarding karyawan junior sehingga bisa cepat produktif (ramp up).
- Mengurangi beban staf senior yang harus terus-menerus mengulang pengajaran materi yang sama.
- Menciptakan **baseline** yang konsisten untuk menilai skill dan perkembangan karyawan, menggantikan label subjektif seperti "junior" atau "senior" dengan deskripsi kompetensi yang lebih spesifik — ini memudahkan pengambilan keputusan dalam penugasan kerja.
- Membangun teamwork, misalnya lewat pengalaman kolaboratif seperti mengikuti tantangan **CTF (Capture the Flag)**.

---

## 2. Strategi Training di Organisasi Besar

### 2.1 Off-the-Shelf vs Kustomisasi

Untuk tim kecil, training off-the-shelf (paket training siap pakai) umumnya sudah cukup dan menjadi pilihan paling efisien. Namun, ketika tim melewati ambang tertentu (umumnya di atas dua puluh karyawan) atau kebutuhan trainingnya sangat spesifik, kustomisasi menjadi lebih relevan.

**Content Studio** (fitur TryHackMe) memungkinkan organisasi memodifikasi modul yang sudah ada atau membuat modul baru sesuai kebutuhan dan prioritas internal, sehingga meningkatkan efektivitas training.

### 2.2 Kebutuhan Integrasi Perusahaan Besar

Perusahaan besar tidak menginginkan solusi training yang berdiri sendiri (standalone), melainkan solusi yang terintegrasi dengan ekosistem software yang sudah mereka gunakan. Dua kebutuhan integrasi utama:

- **SSO (Single Sign-On)** — memungkinkan satu kredensial login dipakai untuk mengakses banyak sistem sekaligus.
- **API yang terdokumentasi dengan baik** — memastikan sistem training bisa terhubung secara mulus (seamless) dengan software lain yang sudah dipakai organisasi.

---

## 3. Menghitung Dampak Finansial Training (ROI)

### 3.1 Konsep Dasar

Membangun proposal investasi training yang meyakinkan membutuhkan justifikasi finansial yang konkret, bukan sekadar klaim kualitatif. Perhitungan **Return On Investment (ROI)** menjadi alat argumentasi utama untuk meyakinkan pihak manajemen/CFO.

### 3.2 Contoh Perhitungan (Studi Kasus)

Parameter kasus:

- Tim cyber security terdiri dari **10 karyawan**.
- Biaya per karyawan: **$80,000/tahun**.
- Asumsi peningkatan produktivitas dari training: **4%**.
- Biaya training: **$500/karyawan**.

Langkah perhitungan:

1. **Penghematan (gain) dari training** = jumlah karyawan × persentase peningkatan produktivitas × biaya per karyawan
 → 10 × 4% × $80,000 = **$32,000**

2. **Total biaya training** = jumlah karyawan × biaya training per karyawan
 → 10 × $500 = **$5,000**

3. **ROI** = penghematan ÷ total biaya
 → $32,000 ÷ $5,000 = **640%**

Angka ROI yang besar ini menunjukkan bahwa manfaat produktivitas dari training jauh melampaui biaya yang dikeluarkan, menjadikannya argumen kuat untuk pengajuan budget.

### 3.3 Menulis Proposal Training

Proposal investasi training sebaiknya menonjolkan penghematan biaya (cost savings) hasil dari perhitungan ROI di atas, karena pendekatan ini terbukti meningkatkan peluang proposal disetujui oleh pihak yang mengelola budget.

---

## 4. Pemilihan Vendor Training

Sebelum memilih vendor training, ada sejumlah pertanyaan kunci yang harus dijawab oleh pihak yang bertanggung jawab atas keputusan ini:

- Training ini ditujukan untuk siapa (target peserta)?
- Bagaimana pengalaman, latar belakang, peran, dan topik yang relevan dengan karyawan yang akan dilatih?
- Apakah vendor punya pengalaman melatih organisasi dengan karakteristik serupa?
- Bagaimana keluasan (breadth), kedalaman (depth), dan kualitas konten pada topik yang menjadi prioritas?
- Apakah user bisa belajar, berlatih, dan praktik dalam satu platform yang sama (tanpa berpindah-pindah tool)?
- Bagaimana posisi biaya training relatif terhadap manfaat produktivitas? Meskipun biaya penting bagi CFO, biaya training biasanya jauh lebih kecil dibanding manfaat produktivitas tim yang dihasilkan.

Menjawab pertanyaan-pertanyaan ini secara sistematis adalah langkah krusial untuk memastikan vendor yang dipilih benar-benar sesuai kebutuhan organisasi dan tim.

---

## 5. Kesimpulan Room

Training adalah komponen yang tidak terpisahkan dari performa tim cyber security. Filosofi **lifelong learning** relevan khususnya bagi knowledge worker, dan banyak organisasi mendorong atau mewajibkan training rutin karena kebijakan ini memberi hasil yang saling menguntungkan (rewarding) baik bagi karyawan (trainee) maupun perusahaan (employer).

---

## 6. Glosarium

- **Training Platform** — Sistem/lab environment khusus untuk berlatih skill cyber security tanpa berisiko merusak sistem produksi.
- **Content Studio** — Fitur TryHackMe yang memungkinkan organisasi memodifikasi atau membuat modul training sesuai kebutuhan internal.
- **SSO (Single Sign-On)** — Mekanisme autentikasi yang memungkinkan satu login digunakan untuk mengakses banyak sistem/aplikasi sekaligus.
- **API (Application Programming Interface)** — Antarmuka yang memungkinkan sistem training terintegrasi dengan software lain milik organisasi.
- **ROI (Return On Investment)** — Rasio antara keuntungan/penghematan yang diperoleh dengan biaya yang dikeluarkan, dipakai untuk mengukur nilai investasi training.
- **CFO (Chief Financial Officer)** — Pejabat perusahaan yang bertanggung jawab atas keputusan finansial, termasuk persetujuan budget training.
- **CTF (Capture the Flag)** — Tantangan/kompetisi cyber security yang sering dipakai sebagai metode training kolaboratif sekaligus pembangun teamwork.
- **Baseline (skill baseline)** — Standar acuan yang konsisten untuk menilai skill dan perkembangan karyawan di seluruh organisasi.
- **Vendor Selection** — Proses evaluasi dan pemilihan penyedia layanan training berdasarkan kriteria relevansi, pengalaman, kualitas konten, dan biaya.

---

## 7. Tools & Platform Rujukan

- **TryHackMe Content Studio** — Fitur untuk membuat/memodifikasi modul training kustom sesuai kebutuhan organisasi.
- **TryHackMe Business** — Platform training untuk organisasi/perusahaan. URL: business.tryhackme.com
- **TryHackMe Classrooms** — Layanan TryHackMe khusus untuk institusi pendidikan (sekolah, kampus, universitas).

---

## 8. Catatan Ringkas untuk Ditulis Tangan

**Konsep Dasar Training**
- Skill cyber security dibangun lewat praktik & training, bukan bawaan
- Training platform — lab terpisah, tidak ganggu sistem produksi
- "Prevention > cure" — belajar di training env, bukan saat insiden nyata
- Manfaat organisasi: tambah kapasitas tim tanpa nambah headcount
- Training percepat onboarding junior, kurangi beban ajar senior
- Baseline skill — standar penilaian konsisten, ganti label junior/senior
- CTF — bangun teamwork sekaligus skill praktik

**Strategi Training Perusahaan Besar**
- Tim kecil (< 20 orang) → off-the-shelf training cukup
- Tim besar / kebutuhan spesifik → kustomisasi (Content Studio)
- Perusahaan besar butuh integrasi: SSO + API terdokumentasi baik

**Perhitungan ROI Training**
- Gain = jumlah karyawan × % peningkatan produktivitas × biaya/karyawan
- Total cost = jumlah karyawan × biaya training/karyawan
- ROI = gain ÷ total cost × 100%
- Contoh: 10 org, $80k/th, naik 4%, training $500/org → gain $32k, cost $5k, ROI 640%
- Proposal training kuat = tonjolkan cost savings / ROI

**Vendor Selection — Pertanyaan Kunci**
- Training untuk siapa?
- Pengalaman/background/role/topik karyawan relevan?
- Vendor pernah tangani organisasi serupa?
- Breadth, depth, quality konten sesuai topik?
- Bisa belajar+praktik dalam 1 platform?
- Biaya training vs manfaat produktivitas (biaya biasanya jauh lebih kecil)

**Istilah Kunci**
- ROI — return on investment, ukur nilai investasi
- SSO — single sign-on, 1 login banyak sistem
- CFO — penanggung jawab keputusan finansial/budget
- Content Studio — fitur kustomisasi modul TryHackMe
- Baseline — standar acuan skill karyawan
