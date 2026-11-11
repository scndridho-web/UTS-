
# U6. Evaluasi Arsitektur

**BAB V:** Evaluasi dan ADR (Bagian Evaluasi)

**Penanggung jawab:** [Muh. Iswwan / Penanggung Jawab Bagian U6]

**Ketentuan dosen:** Pohon utilitas mini (3 skenario); trade-off berbobot dua alternatif; analisis ala ATAM: 2 risiko, 1 non-risiko, 1 sensitivity point, 1 trade-off point; keputusan apakah masalah utama teratasi.

---

## 6.1 Pohon Utilitas Mini — Tiga Skenario

Pohon utilitas digunakan untuk menentukan atribut kualitas yang menjadi prioritas dalam pengembangan Layanan Rekap Kehadiran Smart Campus UMI.

```text
Utilitas Layanan Rekap Kehadiran Smart Campus UMI
│
├── Performa
│   └── SK-01 → [High, High]
│       └── Saat jam sibuk, sekitar 1.000 mahasiswa
│           membuka rekap secara bersamaan, seluruh
│           mata kuliah tampil dalam waktu maksimal 2 detik.
│
├── Usability
│   └── SK-02 → [High, Medium]
│       └── Mahasiswa baru dapat menemukan jumlah
│           dan persentase kehadiran seluruh mata kuliah
│           dalam maksimal 2 klik tanpa bantuan.
│
└── Availability dan Konsistensi
    └── SK-03 → [High, High]
        └── Setelah dosen menyimpan presensi, rekap
            diperbarui maksimal 1 menit. Jika jaringan
            terputus, presensi disimpan dalam antrean
            lokal dan disinkronkan otomatis saat online.
```

### Penilaian Prioritas

1. **SK-01 – Performa (High, High)**

   Memiliki tingkat kepentingan tinggi karena mahasiswa membutuhkan akses rekap kehadiran secara cepat, terutama ketika banyak pengguna mengakses sistem secara bersamaan. Tingkat kesulitannya juga tinggi karena sistem harus mampu menangani sekitar 1.000 mahasiswa dengan waktu respons maksimal 2 detik.

2. **SK-02 – Usability (High, Medium)**

   Memiliki tingkat kepentingan tinggi karena mahasiswa membutuhkan tampilan yang sederhana dan mudah dipahami. Tingkat kesulitannya sedang karena kemudahan akses dapat ditingkatkan melalui desain antarmuka yang sederhana dan penyajian informasi dalam satu halaman.

3. **SK-03 – Availability dan Konsistensi (High, High)**

   Memiliki tingkat kepentingan tinggi karena data kehadiran harus tersedia dan diperbarui dengan cepat. Tingkat kesulitannya tinggi karena sistem harus menangani jaringan tidak stabil, sinkronisasi data, serta kemungkinan keterlambatan pembaruan.

---

## 6.2 Dua Alternatif Rancangan yang Dibandingkan

### Alternatif A: Modul Rekap di Dalam SIAKAD

Alternatif A menggunakan modul rekap kehadiran yang terintegrasi langsung di dalam sistem SIAKAD. Data rekap diperoleh melalui query langsung ke database transaksional.

**Kelebihan:**
- Data rekap menggunakan sumber data utama secara langsung.
- Konsistensi data lebih terjaga.
- Tidak membutuhkan komponen sinkronisasi tambahan.
- Kompleksitas implementasi relatif lebih rendah.

**Kekurangan:**
- Query agregasi dapat membebani database utama.
- Performa berpotensi menurun ketika banyak mahasiswa mengakses sistem.
- Pengembangan fitur baru dapat memengaruhi modul SIAKAD yang sudah ada.

### Alternatif B: Layanan Rekap Terpisah dengan Read Model

Alternatif B menggunakan layanan rekap kehadiran yang terpisah dari sistem SIAKAD. Data presensi disinkronkan ke read model atau cache sehingga proses menampilkan rekap tidak perlu mengambil dan mengolah seluruh data langsung dari database transaksional.

**Kelebihan:**
- Performa penampilan rekap lebih cepat.
- Mengurangi beban database utama SIAKAD.
- Memudahkan pengembangan fitur pencarian, filter, notifikasi, dan ekspor laporan.
- Layanan rekap dapat dikembangkan dan dikelola secara terpisah.

**Kekurangan:**
- Data rekap berpotensi mengalami keterlambatan pembaruan.
- Membutuhkan mekanisme sinkronisasi data.
- Kompleksitas pengelolaan sistem lebih tinggi.

### Kriteria dan Bobot

Penilaian menggunakan skala 1–5, dengan ketentuan semakin tinggi skor berarti semakin baik.

| Kriteria | Bobot (%) | Skor A | Bobot × A | Skor B | Bobot × B | Dasar Penilaian |
|---|---:|---:|---:|---:|---:|---|
| Performa | 25 | 2 | 0,50 | 5 | 1,25 | Kemampuan sistem menampilkan rekap saat jam sibuk. |
| Usability | 20 | 4 | 0,80 | 4 | 0,80 | Kemudahan mahasiswa mengakses informasi kehadiran. |
| Availability | 20 | 3 | 0,60 | 4 | 0,80 | Kemampuan layanan menyediakan data rekap. |
| Konsistensi Data | 15 | 5 | 0,75 | 3 | 0,45 | Ketepatan data rekap terhadap sumber presensi. |
| Modifiability | 10 | 2 | 0,20 | 4 | 0,40 | Kemudahan pengembangan dan penambahan fitur. |
| Biaya dan Kompleksitas | 10 | 4 | 0,40 | 2 | 0,20 | Kebutuhan komponen tambahan dan pengelolaan sistem. |
| **Jumlah** | **100** | — | **3,25** | — | **3,90** | — |

### Rumus Skor Berbobot

Rumus perhitungan:

`Skor Total = Σ (Bobot × Skor Alternatif)`

Karena bobot menggunakan bentuk desimal, perhitungannya sebagai berikut.

**Alternatif A:**

```text
(0,25 × 2) + (0,20 × 4) + (0,20 × 3)
+ (0,15 × 5) + (0,10 × 2) + (0,10 × 4)

= 0,50 + 0,80 + 0,60 + 0,75 + 0,20 + 0,40

= 3,25
```

**Alternatif B:**

```text
(0,25 × 5) + (0,20 × 4) + (0,20 × 4)
+ (0,15 × 3) + (0,10 × 4) + (0,10 × 2)

= 1,25 + 0,80 + 0,80 + 0,45 + 0,40 + 0,20

= 3,90
```

### Alternatif Terpilih dan Alasannya

Berdasarkan hasil perhitungan, Alternatif A memperoleh skor 3,25, sedangkan Alternatif B memperoleh skor 3,90.

Dengan demikian, **Alternatif B dipilih sebagai rancangan arsitektur yang lebih sesuai** untuk Layanan Rekap Kehadiran Smart Campus UMI.

Alternatif B memiliki keunggulan pada aspek performa dan availability. Penggunaan read model dan cache memungkinkan data rekap ditampilkan lebih cepat tanpa membebani database transaksional SIAKAD.

Namun, alternatif ini memiliki konsekuensi berupa kemungkinan keterlambatan sinkronisasi data dan meningkatnya kompleksitas pengelolaan sistem.

---

## 6.3 Analisis ala ATAM

ATAM (Architecture Tradeoff Analysis Method) digunakan untuk mengidentifikasi risiko, non-risiko, sensitivity point, dan trade-off point pada rancangan arsitektur yang dipilih.

| Jenis Temuan | Temuan Spesifik Rancangan | Dampak / Tindak Lanjut |
|---|---|---|
| Risiko 1 | Read model menggunakan eventual consistency sehingga data rekap dapat mengalami keterlambatan dibandingkan data presensi asli. | Diperlukan sinkronisasi berbasis event dan rekonsiliasi harian untuk mendeteksi serta memperbaiki ketidaksesuaian data. |
| Risiko 2 | Rekap kehadiran tidak lengkap apabila dosen tidak mengisi presensi melalui SIAKAD. | Diperlukan kebijakan kampus yang mewajibkan pengisian presensi melalui SIAKAD serta penanda mata kuliah yang belum memiliki data presensi. |
| Non-risiko 1 | Layanan rekap bersifat read-only sehingga gangguan pada layanan rekap tidak secara langsung menghambat proses pencatatan presensi dosen di SIAKAD. | Proses pencatatan presensi utama tetap berjalan secara terpisah dari layanan rekap. |
| Sensitivity point 1 | Interval sinkronisasi data dan TTL (Time To Live) cache sangat memengaruhi kesegaran informasi rekap dan beban sistem. | Interval sinkronisasi perlu diatur agar pembaruan data dapat dilakukan maksimal 1 menit tanpa memberikan beban berlebihan pada sistem. |
| Trade-off point 1 | Penggunaan read model dan cache meningkatkan performa serta availability, tetapi mengurangi konsistensi data secara langsung dan menambah kompleksitas operasional. | Diperlukan mekanisme pemantauan, penanganan event gagal, dan rekonsiliasi data agar performa tetap seimbang dengan ketepatan informasi. |

---

## 6.4 Apakah Masalah Utama Teratasi?

Berdasarkan hasil evaluasi terhadap rancangan Layanan Rekap Kehadiran Smart Campus UMI, sebagian besar masalah utama yang ditemukan dari hasil kuesioner dapat diatasi melalui Alternatif B.

Beberapa permasalahan yang dapat diatasi antara lain:

1. **Rekap kehadiran yang terpisah-pisah**

   Mahasiswa dapat melihat seluruh informasi kehadiran mata kuliah dalam satu halaman, termasuk jumlah hadir, izin, sakit, alpa, dan persentase kehadiran.

2. **Kesulitan mengakses informasi**

   Sistem dirancang dengan tampilan yang lebih sederhana sehingga mahasiswa dapat menemukan informasi kehadiran seluruh mata kuliah dalam maksimal dua klik.

3. **Performa sistem saat banyak pengguna**

   Penggunaan read model dan cache dapat mengurangi beban query pada database utama SIAKAD sehingga proses menampilkan rekap diharapkan lebih cepat.

4. **Jaringan yang tidak stabil**

   Penggunaan antrean lokal dan caching dapat membantu menyimpan data sementara ketika koneksi terputus. Data dapat disinkronkan kembali ketika jaringan tersedia.

5. **Kebutuhan dosen terhadap laporan kehadiran**

   Sistem menyediakan rancangan fitur rekap otomatis, filter, pencarian, notifikasi kehadiran rendah, serta unduh laporan.

### Keterbatasan yang Masih Ada

Meskipun sebagian besar masalah dapat diatasi melalui rancangan arsitektur, masih terdapat beberapa keterbatasan.

- Mata kuliah yang tidak memiliki data presensi karena dosen belum mengisi presensi di SIAKAD tidak dapat dilengkapi secara otomatis oleh layanan rekap.
- Read model berpotensi mengalami keterlambatan sinkronisasi sehingga data rekap belum tentu selalu sama secara langsung dengan data sumber.
- Jaringan yang tidak stabil tetap dapat memengaruhi proses pengiriman dan pembaruan data.

Masalah pengisian presensi membutuhkan kebijakan kampus yang mewajibkan dosen mengisi presensi melalui SIAKAD. Sistem juga dapat memberikan tanda pada mata kuliah yang data presensinya belum tersedia.

### Validasi Hasil Evaluasi

Hasil evaluasi ini masih berupa analisis berdasarkan rancangan arsitektur dan asumsi kebutuhan dari hasil kuesioner. Target waktu respons maksimal 2 detik, pembaruan maksimal 1 menit, dan kemampuan menangani sekitar 1.000 pengguna secara bersamaan belum dapat dinyatakan berhasil sebelum dilakukan pengujian nyata.

Oleh karena itu, diperlukan pengujian performa, pengujian sinkronisasi, dan evaluasi penggunaan secara langsung untuk memastikan rancangan dapat memenuhi seluruh kebutuhan.

**Kesimpulan:** Alternatif B berpotensi mengatasi sebagian besar masalah utama Layanan Rekap Kehadiran Smart Campus UMI. Namun, masih diperlukan pengujian dan perbaikan pada aspek konsistensi data, sinkronisasi, serta pengelolaan jaringan.

---

## 6.5 Keterkaitan dengan ADR

Berdasarkan hasil evaluasi arsitektur, terdapat dua keputusan penting yang perlu didokumentasikan lebih lanjut pada U7 (Architecture Decision Records).

### ADR-001: Layanan Rekap Kehadiran Terpisah dengan Read Model

**Keputusan:** Membangun layanan rekap kehadiran secara terpisah menggunakan read model.

**Alasan:** Query agregasi langsung pada database transaksional SIAKAD berpotensi menurunkan performa ketika banyak mahasiswa mengakses rekap secara bersamaan.

**Dampak:** Proses menampilkan rekap menjadi lebih cepat dan pengembangan fitur lebih fleksibel, tetapi terdapat tambahan komponen yang harus dikelola serta kemungkinan keterlambatan data.

### ADR-002: Sinkronisasi Berbasis Event dengan Rekonsiliasi Harian

**Keputusan:** Menggunakan mekanisme event-driven untuk memperbarui read model maksimal 1 menit setelah presensi disimpan, disertai proses rekonsiliasi harian.

**Alasan:** Data rekap harus diperbarui dengan cepat dan ketidaksesuaian akibat event yang gagal atau hilang harus dapat dideteksi.

**Dampak:** Data rekap menjadi hampir real-time dan selisih data dapat diperbaiki, tetapi sistem membutuhkan message queue serta penanganan event gagal dan duplikat.

### Kesimpulan U6

Berdasarkan hasil evaluasi menggunakan Utility Tree, Trade-off Berbobot, dan ATAM, Alternatif B yaitu Layanan Rekap Kehadiran Terpisah dengan Read Model dipilih sebagai rancangan arsitektur yang lebih sesuai.

Alternatif B memperoleh skor tertinggi sebesar 3,90 dibandingkan Alternatif A sebesar 3,25.

Hasil evaluasi U6 menjadi dasar pengambilan keputusan arsitektur pada U7, khususnya mengenai pemisahan layanan rekap kehadiran dan mekanisme sinkronisasi berbasis event dengan rekonsiliasi harian.
