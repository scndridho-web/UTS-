# U7. Architecture Decision Records (ADR)

**BAB V:** Evaluasi dan ADR (Bagian ADR)  
**Penanggung jawab:** [Nama Anggota / Penanggung Jawab Bagian U7]  

**Ketentuan dosen:** Minimal 2 ADR baru (mencakup Title, Status, Context, Decision, Consequences) yang mendasari keputusan rancangan arsitektur kelompok.

---

## ADR-001: Layanan Rekap Kehadiran Terpisah dengan Read Model

* **Status:** Accepted
* **Context:** Mahasiswa mengeluhkan harus membuka halaman kehadiran satu per satu untuk setiap mata kuliah, dan dosen membutuhkan rekapitulasi secara terpusat[cite: 15]. Namun, melakukan *query* agregasi secara langsung ke basis data transaksional SIAKAD berisiko tinggi memperlambat kinerja sistem utama universitas saat jam sibuk[cite: 15].
* **Decision:** Membangun *Layanan Rekap Kehadiran* tersendiri yang menyimpan data agregat (jumlah hadir, izin, sakit, alpa, dan persentase per mahasiswa per mata kuliah) dalam *read model* terpisah. Layanan ini dirancang khusus untuk mendukung operasi baca (*read-only*) yang cepat[cite: 15].
* **Consequences:**
  * (+) Halaman rekap kehadiran menjadi sangat cepat diakses tanpa membebani basis data utama SIAKAD[cite: 15].
  * (+) Mempermudah penambahan fitur baru seperti filter pencarian, notifikasi, dan ekspor laporan[cite: 15].
  * (−) Data rekap berpotensi mengalami jeda sesaat (*eventual consistency*) dari sumber utamanya[cite: 15].
  * (−) Terdapat penambahan komponen layanan baru yang harus dikelola dan dipantau oleh tim pengelola[cite: 15].

---

## ADR-002: Sinkronisasi Berbasis Event dengan Rekonsiliasi Harian

* **Status:** Accepted
* **Context:** Dosen melaporkan adanya potensi ketidaksesuaian antara data presensi asli dengan rekapitulasi yang ditampilkan[cite: 15]. Pembaruan data harus berlangsung cepat, tetapi kehilangan *event* pesan tidak boleh membuat data permanen menjadi salah[cite: 15].
* **Decision:** Setiap data presensi yang disimpan akan memancarkan *event* (`PresensiDisimpan`) yang memperbarui *read model* dalam waktu singkat ($\le 1$ menit)[cite: 15]. Selain itu, dijalankan *job* rekonsiliasi otomatis setiap hari (pada dini hari) untuk membandingkan *read model* dengan data sumber asli serta memperbaiki selisih data jika ditemukan[cite: 15].
* **Consequences:**
  * (+) Rekapitulasi data kehadiran dapat disajikan secara *near real-time* dengan tingkat konsistensi yang terjaga[cite: 15].
  * (+) Selisih atau inkonsistensi data dapat terdeteksi dan dikoreksi secara otomatis[cite: 15].
  * (−) Memerlukan infrastruktur *message queue* serta penanganan khusus untuk mengantisipasi *event* gagal atau duplikat[cite: 15].
  * (−) Beban komputasi sistem meningkat saat proses rekonsiliasi harian dijalankan[cite: 15].

---

## ADR-003 (Opsional / Pendukung): Antrean Presensi Lokal untuk Jaringan Tidak Stabil

* **Status:** Proposed
* **Context:** Mahasiswa dan dosen sama-sama mengeluhkan kendala jaringan internet yang tidak stabil saat mengakses sistem perkuliahan[cite: 5, 15].
* **Decision:** Mengembangkan aplikasi web berbasis *Progressive Web App (PWA)*. Presensi yang gagal terkirim akibat gangguan jaringan akan disimpan sementara pada penyimpanan lokal perangkat (*local storage*) dan dikirim ulang secara otomatis setelah koneksi pulih. Halaman rekap terakhir juga disimpan dalam bentuk *cache* agar tetap dapat diakses secara *offline*[cite: 15].
* **Consequences:**
  * (+) Data presensi tidak hilang meskipun koneksi internet pengguna terputus secara mendadak[cite: 15].
  * (+) Mahasiswa tetap dapat memantau riwayat rekap kehadiran terakhir tanpa koneksi internet yang aktif[cite: 15].
  * (−) Memerlukan mekanisme penanganan konflik data ganda saat proses sinkronisasi ulang dilakukan[cite: 15].
  * (−) Data *offline* berisiko usang, sehingga memerlukan penanda waktu pembaruan (*timestamp*) yang jelas[cite: 15].
