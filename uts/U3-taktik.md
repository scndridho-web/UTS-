# U3. Taktik Arsitektur

**BAB III:** Taktik dan Pola Arsitektur (Bagian Taktik)  
**Penanggung jawab:** [Nama Anggota / Penanggung Jawab Bagian U3]  

**Ketentuan dosen:** Minimal 1 taktik tiap skenario: cara kerjanya dan harga yang dibayar[cite: 2].

---

## 3.1 Taktik untuk Skenario 1 (Performance / Waktu Respons Rekap)
* **Taktik yang Digunakan:** *Caching* dan *Read Model* Terisolasi.
* **Cara Kerjanya:** 
  Alih-alih melakukan *query* agregasi secara langsung ke basis data transaksional utama setiap kali mahasiswa meminta rekapitulasi, sistem memanfaatkan lapisan memori *cache* (*Cache-Aside pattern*)[cite: 3]. Data rekapitulasi per mahasiswa dihitung sebelumnya dan disimpan dalam model baca (*read model*) khusus yang dioptimalkan untuk operasi `GET`[cite: 3].
* **Harga yang Dibayar (Trade-off):** 
  * Memunculkan potensi jeda kesegaran data sesaat (*eventual consistency*) jika ada pembaruan presensi yang belum disinkronkan ke dalam *cache*[cite: 3].
  * Menambah kompleksitas pengelolaan memori dan strategi invalidasi *cache* pada sisi arsitektur backend.

---

## 3.2 Taktik untuk Skenario 2 (Usability / Kemudahan Akses Dasbor)
* **Taktik yang Digunakan:** *UI Component Aggregation* / *Single-Page Dashboard Design*.
* **Cara Kerjanya:** 
  Menggabungkan berbagai data kehadiran dari berbagai mata kuliah ke dalam satu antarmuka dasbor terpusat (*Single-Page Application*). Sistem menyediakan kolom pencarian (*search bar*) dan filter instan sehingga pengguna (mahasiswa) dapat langsung melihat ringkasan seluruh mata kuliah, persentase kehadiran, izin, sakit, dan alpa tanpa harus membuka menu per mata kuliah satu per satu[cite: 3, 5].
* **Harga yang Dibayar (Trade-off):** 
  * Payload data yang dikirimkan dari server ke klien menjadi lebih besar dalam satu kali muat (*initial load*).
  * Memerlukan desain tata letak antarmuka yang responsif agar tetap ringan ketika diakses melalui perangkat telepon seluler (*smartphone*).

---

## 3.3 Taktik untuk Skenario 3 (Availability & Consistency / Toleransi Jaringan)
* **Taktik yang Digunakan:** *Local Queue* / *Offline-First Architecture* (*Progressive Web App*).
* **Cara Kerjanya:** 
  Ketika jaringan internet mengalami gangguan atau terputus saat proses pencatatan presensi, sistem secara otomatis mengamankan data transaksi ke dalam penyimpanan lokal perangkat (*local storage*). Sebuah *sync engine* di latar belakang akan memantau status koneksi dan mengirimkan ulang data yang tertunda secara otomatis begitu koneksi internet kembali stabil[cite: 3].
* **Harga yang Dibayar (Trade-off):** 
  * Meningkatkan kompleksitas pada sisi klien untuk menangani potensi konflik data ganda (*duplicate submission*) saat sinkronisasi ulang dilakukan[cite: 3].
  * Data *offline* yang tersimpan di perangkat berisiko usang jika tidak disertai penanda waktu (*timestamp*) yang valid[cite: 3].
