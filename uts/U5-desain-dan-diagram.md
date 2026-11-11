# U5. Desain dan Diagram

**BAB IV:** Desain  
**Penanggung jawab:** [Nama Anggota / Penanggung Jawab Bagian U5]  

**Ketentuan dosen:** minimal **3 diagram** (konteks, arsitektur yang menerapkan pola, UML), masing-masing berketerangan, serta **3 prinsip desain** yang diterapkan.

**PENTING:** Diagram PNG dan `.drawio` yang disertakan adalah **CONTOH RANCANGAN AWAL**. Semua komponen/arah hubungan harus disepakati kelompok, konsisten dengan U3/U4/U6/U7, dan belum berarti telah diimplementasikan.

---

## 4.1 Diagram Konteks

![Diagram Konteks](diagram/konteks-rekap-kehadiran.png)

**Keterangan Komponen Konteks:**
* **Mahasiswa:** Pengguna yang mengakses sistem untuk melihat ringkasan/rekapitulasi kehadiran lintas mata kuliah secara terpusat.
* **Dosen:** Pengguna yang memantau tingkat keaktifan mahasiswa, melakukan validasi presensi, serta mengunduh laporan rekap kehadiran kelas.
* **Layanan Rekap Kehadiran:** Subsistem usulan yang bertugas mengumpulkan, memproses, dan menyajikan data presensi secara terpadu.
* **SIAKAD UMI:** Sistem akademik utama universitas yang bertindak sebagai sumber data master presensi dan jadwal perkuliahan.
* **Notifikasi:** Modul pendukung untuk mengirimkan *alert* atau peringatan otomatis apabila tingkat kehadiran mahasiswa berada di bawah batas minimum.

---

## 4.2 Diagram Arsitektur yang Menerapkan Pola

![Diagram Arsitektur](diagram/arsitektur-microservices.png)

*Gambar 4.2 Arsitektur microservices Layanan Rekap Kehadiran*

**Keterangan Alur Arsitektur:**
Permintaan dari Aplikasi Klien masuk melalui **API Gateway** (ungu) ke **Rekap Service** (teal). Rekap Service memeriksa **Cache Rekap** (kuning) terlebih dahulu. Jika kosong (*cache miss*), ia memanggil **Presensi Service**, **Mata Kuliah Service**, dan **Mahasiswa Service**, yang mengakses SIAKAD UMI hanya melalui **Adapter SIAKAD** (merah muda). **Notifikasi Service** dipicu oleh Rekap Service saat kehadiran mahasiswa terdeteksi di bawah batas minimum.

---

## 4.3 Diagram UML (Sequence): Melihat Rekap Kehadiran

![Diagram UML Sequence](diagram/uml-sequence-rekap.png)

**Keterangan Alur Sequence:**
1. **Mahasiswa** mengirimkan permintaan HTTP `GET /rekap` melalui **API Gateway**.
2. **API Gateway** meneruskan permintaan beserta token autentikasi ke **Rekap Service**.
3. **Rekap Service** melakukan pengecekan ke **Cache Rekap**. 
4. Jika terjadi *cache miss*, **Rekap Service** mengambil data mentah ke **Presensi Service**.
5. Setelah data presensi diterima, **Rekap Service** menyimpan kembali data tersebut ke cache menggunakan strategi *Time-To-Live* (TTL).
6. Data rekap dan persentase kehadiran dikembalikan ke **API Gateway** lalu dirender menjadi respons JSON ke perangkat **Mahasiswa**.

---

## 4.4 Tiga Prinsip Desain yang Diterapkan

1. **Single Responsibility Principle (SRP):** Setiap layanan mikro (*Presensi Service*, *Mata Kuliah Service*, *Mahasiswa Service*, dan *Notifikasi Service*) memiliki satu fokus tanggung jawab fungsional yang spesifik.
2. **Layered & Decoupled Access (Pemisahan Melalui Adapter):** Komunikasi langsung dengan SIAKAD monolitik universitas diisolasi melalui *Adapter SIAKAD* guna melindungi integritas sistem utama dari lonjakan trafik layanan rekap.
3. **Performance Optimization via Caching (Caching Strategy):** Penerapan *Cache-Aside* dengan *TTL* pada data rekap guna memangkas waktu akses (*response time*) agar halaman rekap kehadiran dapat dimuat secara instan tanpa membebani basis data utama.
