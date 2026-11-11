# U1. Konteks dan ASR

**BAB I:** Pendahuluan  
**Penanggung jawab:** [Nendra Riskullah / Penanggung Jawab Bagian U1]  

---

## 1.1 Ringkasan Layanan
Subsistem **Attendance Management** (Layanan Presensi dan Rekap Kehadiran Smart Campus UMI) adalah modul inti yang bertanggung jawab untuk mencatat, mengelola, memvalidasi, serta menyajikan rekapitulasi kehadiran perkuliahan bagi mahasiswa dan dosen secara terpadu[cite: 3]. Layanan ini dirancang untuk mengatasi fragmentasi data kehadiran lintas mata kuliah dan meningkatkan keandalan sistem akademik universitas.

---

## 1.2 Masalah Utama Layanan dan Bukti Wawancara
* **Masalah Utama:** Ketidaksesuaian, lambatnya sinkronisasi, dan fragmentasi informasi rekap kehadiran mahasiswa antar-mata kuliah (harus dibuka satu per satu), serta ketidaklengkapan status presensi akibat beberapa perkuliahan belum/terlambat ter-input ke SIAKAD/Kalam[cite: 3, 5].
* **Dasar Bukti Wawancara Responden (R1–R5):**
  * **R1 (Firsa Amelia Putri):** Kendala jaringan atau gangguan sistem sering menghambat proses pengisian presensi[cite: 4]. Dosen membutuhkan rekap kehadiran otomatis, fitur pencarian, filter mata kuliah, notifikasi kehadiran rendah, serta unduh laporan (PDF/Excel)[cite: 4].
  * **R2 (Mahasiswa - Wahyuningsih):** Informasi rekap harus dibuka per mata kuliah satu per satu dan terkendala jaringan internet[cite: 5].
  * **R3 (Mahasiswa - Harry):** Dasbor saat ini terlalu rumit dan tidak simpel[cite: 5], serta sering ditemukan mata kuliah yang tidak menampilkan data kehadiran[cite: 5].
  * **R4 (Mahasiswa - Salwa):** Kesulitan memantau presensi karena tidak semua dosen mengabsen langsung di SIAKAD UMI[cite: 5], memerlukan pencarian mata kuliah dan perbaikan tampilan rekap[cite: 5].
  * **R5 (Mahasiswa - Putri Zalsabila):** Membutuhkan waktu lama karena harus memeriksa kehadiran per mata kuliah satu per satu[cite: 5]; belum tersedia rekap keseluruhan (persentase, izin, sakit, alpa) dalam satu halaman[cite: 5].

---

## 1.3 Empat Stakeholder Utama
1. **Mahasiswa:** Pengguna yang memantau ringkasan rekap kehadiran dan persentase kehadiran seluruh mata kuliah dalam satu halaman[cite: 5].
2. **Dosen:** Pengguna yang mengelola presensi kelas, memantau keaktifan mahasiswa, serta mengunduh laporan rekap[cite: 4].
3. **Program Studi / BTIK UMI:** Pihak pengelola yang memantau tingkat kepatuhan dan keaktifan proses perkuliahan secara institusional.
4. **Bagian Administrasi Akademik:** Pihak pengelola data master mahasiswa, dosen, KRS, dan penjadwalan akademik.

---

## 1.4 Tiga Architectural Significant Requirements (ASR)
1. **ASR-01 (Performa Rekapitulasi):** Halaman rekapitulasi seluruh mata kuliah harus dapat dimuat dalam waktu $\le 2$ detik meskipun diakses secara bersamaan oleh $\pm 1.000$ pengguna pada jam sibuk perkuliahan[cite: 3].
2. **ASR-02 (Ketersediaan & Toleransi Jaringan):** Sistem harus tetap dapat diakses dan responsif di tengah kondisi jaringan internet seluler/kampus yang tidak stabil melalui strategi penyimpanan lokal (*offline-first*) dan *caching*[cite: 3, 5].
3. **ASR-03 (Modifiabilitas & Integrasi Aman):** Penambahan fitur baru (seperti notifikasi otomatis atau ekspor laporan) tidak boleh mengganggu kestabilan basis data transaksional utama SIAKAD UMI[cite: 3].

---

## 1.5 Tiga Karakteristik Pendorong (Quality Attribute Drivers)
1. **Performance / Latency:** Menjamin kecepatan respons akses data rekap saat beban puncak.
2. **Usability / Accessibility:** Menyediakan antarmuka yang sederhana, intuitif, dan terpusat (*single-page dashboard*).
3. **Availability & Consistency:** Menjaga ketersediaan layanan dan konsistensi data di tengah kendala konektivitas yang fluktuatif[cite: 5].
