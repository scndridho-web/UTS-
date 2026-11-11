# U2. Skenario Atribut Kualitas

**BAB II:** Skenario Atribut Kualitas  
**Penanggung jawab:** [Nendra Riskullah / Penanggung Jawab Bagian U2]  

**Ketentuan dosen:** 3 skenario (satu per karakteristik pendorong), masing-masing 6 komponen; ukuran respons berupa angka; minimal satu skenario menjawab masalah utama[cite: 4].

---

## 2.1 Skenario 1 (Karakteristik: Performance — Menjawab Masalah Utama)
* **Source (Sumber):** Mahasiswa aktif tingkat akhir yang ingin memantau status kehadiran.
* **Stimulus (Rangsangan):** Mengirimkan permintaan HTTP `GET /rekap` melalui portal mahasiswa untuk melihat rekapitulasi kehadiran seluruh mata kuliah secara terpusat[cite: 3, 5].
* **Artifact (Artefak):** Modul *Rekap Service*, *API Gateway*, dan lapisan *Cache*.
* **Environment (Lingkungan):** Kondisi jam sibuk perkuliahan dengan beban akses bersamaan sebanyak $\pm 1.000$ pengguna aktif[cite: 3].
* **Response (Respons):** Sistem mengambil data agregat dari *cache/read model* dan merender dasbor rekapitulasi secara utuh tanpa hambatan[cite: 3].
* **Response Measure (Ukuran Respons):** Waktu muat halaman rekapitulasi keseluruhan mata kuliah tampil dalam waktu **$\le 2$ detik**[cite: 3].

---

## 2.2 Skenario 2 (Karakteristik: Usability)
* **Source (Sumber):** Mahasiswa baru (Angkatan 2026) pengguna sistem akademik.
* **Stimulus (Rangsangan):** Melakukan navigasi ke dalam portal akademik untuk mencari informasi jumlah kehadiran, izin, sakit, alpa, dan persentase kehadiran suatu mata kuliah[cite: 3, 5].
* **Artifact (Artefak):** Antarmuka Pengguna (*UI Gateway* / Dasbor Presensi Tunggal).
* **Environment (Lingkungan):** Kondisi operasional normal pada perangkat telepon seluler (*smartphone*).
* **Response (Respons):** Sistem menampilkan menu pencarian dan menyajikan tabel rekap yang ringkas, intuitif, serta terpusat dalam satu halaman[cite: 3, 5].
* **Response Measure (Ukuran Respons):** Pengguna berhasil menemukan total kehadiran dan persentase seluruh mata kuliah dalam waktu **$\le 2$ klik** tanpa bantuan manual[cite: 3, 5].

---

## 2.3 Skenario 3 (Karakteristik: Availability & Consistency)
* **Source (Sumber):** Dosen pengampu mata kuliah.
* **Stimulus (Rangsangan):** Melakukan penyimpanan data presensi kelas saat kondisi jaringan internet seluler/kampus mengalami gangguan atau putus-putus[cite: 3, 5].
* **Artifact (Artefak):** Modul penyimpanan lokal (*Local Storage / Progressive Web App*) dan *Sync Engine*.
* **Environment (Lingkungan):** Jaringan internet kampus tidak stabil[cite: 3, 5].
* **Response (Respons):** Sistem mencatat data presensi secara lokal di perangkat, menampilkan indikator status *offline*, dan melakukan sinkronisasi otomatis begitu koneksi internet pulih[cite: 3].
* **Response Measure (Ukuran Respons):** Data presensi aman tanpa kehilangan catatan, dan proses sinkronisasi ke server pusat selesai dalam waktu **$\le 1$ menit** setelah jaringan tersambung kembali[cite: 3].
