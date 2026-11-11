# U4. Pola Arsitektur

**BAB III:** Taktik dan Pola (Bagian Pola)  
**Penanggung jawab:** [Nama Anggota / Penanggung Jawab Bagian U4]  

**Ketentuan dosen:** Minimal dua pola dari kategori berbeda; untuk tiap pola: alasan memilih, kelebihan, kekurangan pada layanan UMI. Tambahkan satu perbedaan gaya dan pola arsitektur.

---

## 4.1 Pola P-01 — Microservices Architecture (Layered & Component-Based)
- **Kategori yang diusulkan:** Struktur/pembagian lapisan dan distribusi layanan (*Structural / Distribution*).
- **Definisi singkat:** Pola arsitektur yang menyusun aplikasi sebagai kumpulan layanan kecil yang independen, di mana setiap layanan berjalan dalam prosesnya sendiri dan berkomunikasi melalui mekanisme ringan (biasanya API HTTP/REST).
- **Alasan sesuai kasus Attendance Management UMI:** Mengatasi masalah utama fragmentasi rekap presensi dan kelambatan sinkronisasi data dari berbagai mata kuliah di SIAKAD, sehingga modul presensi dapat dikembangkan, diskalakan, dan dipelihara secara terisolasi tanpa mengganggu modul akademik lainnya.
- **Cara diterapkan:** [Aplikasi Klien / Portal → API Gateway → Rekap Service / Presensi Service → Adapter SIAKAD → SIAKAD UMI; dicocokkan dengan diagram U5].
- **Kelebihan:** Memungkinkan skalabilitas mandiri pada layanan rekapitulasi presensi yang sering diakses mahasiswa secara bersamaan, serta melokalisasi dampak kesalahan (*fault isolation*) agar gangguan pada modul lain tidak merusak fitur absensi.
- **Kekurangan:** Meningkatkan kompleksitas operasional, pengelolaan jaringan antar-layanan (*network latency*), serta overhead koordinasi transaksi data lintas layanan.

---

## 4.2 Pola P-02 — Publish–Subscribe (Event-Driven Architecture)
- **Kategori yang diusulkan:** Komunikasi/integrasi berbasis *event* (*Messaging / Communication*).
- **Definisi singkat:** Pola komunikasi asinkron di mana pengirim (*publisher*) memancarkan event ke sebuah *broker*, tanpa harus mengetahui siapa penerima (*subscriber*) yang mendengarkannya.
- **Alasan sesuai kasus Attendance Management UMI:** Dibutuhkan untuk mengirimkan notifikasi instan atau peringatan (*alert*) ketika tingkat kehadiran mahasiswa terdeteksi berada di bawah batas minimum, tanpa membuat proses utama pemuatan rekap menjadi lambat.
- **Cara diterapkan:** [Rekap Service / Presensi Service mempublikasikan event pelanggaran/kehadiran rendah ke Message Broker → Notifikasi Service mendengarkan event tersebut dan mengirimkan alert ke mahasiswa/dosen; dicocokkan dengan diagram U5].
- **Kelebihan:** Mengurangi keterkuatan langsung (*loose coupling*) antar-komponen, sehingga penambahan fitur notifikasi atau pelaporan baru tidak mengganggu kinerja inti layanan presensi.
- **Kekurangan:** Potensi keterlambatan sinkronisasi sesaat (*eventual consistency delay*) pada pengiriman notifikasi akibat antrean pesan, serta kesulitan dalam melacak urutan *debugging* pesan asinkron.

*Perhatian: Dua pola di atas adalah pilihan rancangan usulan kelompok yang konsisten dengan diagram U5. Kelompok menyetujui kesesuaian kategori ini untuk diterapkan pada subsistem Attendance Management.*

---

## 4.3 Satu perbedaan gaya dan pola arsitektur
- **Gaya (style):** Karakteristik umum atau cetak biru tingkat makro mengenai cara keseluruhan sistem diorganisasikan (misalnya gaya monolitik, terdistribusi, atau *layered*).
- **Pola (pattern):** Solusi berulang yang lebih spesifik pada tingkat desain komponen atau taktik komunikasi untuk mengatasi masalah rekayasa perangkat lunak tertentu dalam konteks tertentu.
- **Perbedaan pada contoh kelompok:** Gaya arsitektur sistem UMI secara umum menggunakan gaya terdistribusi berbasis *Microservices*, sedangkan pola arsitektur yang diterapkan di dalamnya adalah pola *API Composition* dan *Publish-Subscribe* untuk menyelesaikan spesifik masalah fragmentasi rekap presensi.

---

## 4.4 Hubungan dengan taktik, diagram, dan ADR
Rancangan pola di atas merealisasikan taktik mitigasi latensi (seperti *Caching* dan *Asynchronous Processing*) di U3, digambarkan secara visual pada diagram komponen dan sekuens di U5, serta dilegalkan melalui keputusan formal pada dokumen *Architecture Decision Records* (ADR) di U7.
