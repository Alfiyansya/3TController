# Business Requirements Document (BRD)
## 3T Controller — Time, Territory, & Traffic Controller

**Versi:** 1.0  
**Tanggal:** 2026-08-30  
**Sumber acuan:** 3T_Controller_Project_Brief_PRD.pdf

## 1. Ringkasan Eksekutif
3T Controller adalah platform web terpusat untuk institusi pendidikan yang bertujuan mengoptimalkan alokasi bandwidth berdasarkan waktu kegiatan dan zona lokasi. Solusi ini menggantikan pengelolaan router yang sebelumnya manual, teknis, dan rawan human error menjadi sistem visual, terotomatisasi, dan lebih mudah dioperasikan oleh pihak sekolah.

Secara bisnis, platform ini ditujukan untuk meningkatkan kualitas layanan internet di area akademik, menekan pemborosan bandwidth di area non-prioritas saat jam kritis, serta mengurangi kebutuhan penambahan bandwidth yang mahal.

## 2. Latar Belakang Bisnis
Berdasarkan PRD, institusi pendidikan menghadapi tiga persoalan utama:
1. Pemborosan bandwidth pada jam sekolah karena konsumsi tinggi di area umum seperti kantin dan ruang publik.
2. Pengaturan limitasi bandwidth masih dilakukan manual melalui Winbox atau terminal, sehingga bergantung pada tenaga teknis dan rentan kesalahan.
3. Tidak tersedia visibilitas trafik jaringan per gedung atau access point secara real-time dalam bentuk yang mudah dipahami.

Dampak bisnis dari kondisi tersebut adalah terganggunya kegiatan belajar mengajar, meningkatnya keluhan pengguna, rendahnya efisiensi operasional jaringan, dan potensi biaya tambahan untuk upgrade bandwidth yang sebenarnya dapat dihindari.

## 3. Tujuan Bisnis
Dokumen ini menetapkan kebutuhan bisnis untuk memastikan 3T Controller mampu:
- Mengoptimalkan distribusi bandwidth ke zona esensial pada jam kegiatan utama.
- Mengotomatisasi perubahan profil QoS sesuai jadwal tanpa intervensi manual rutin.
- Menyediakan antarmuka visual yang mudah digunakan oleh staf non-spesialis jaringan.
- Menyediakan kontrol cepat untuk kebutuhan khusus melalui mekanisme override.
- Meningkatkan visibilitas dan akuntabilitas pengelolaan trafik jaringan kampus.

## 4. Sasaran dan Indikator Keberhasilan
Mengacu pada PRD, sasaran bisnis utama adalah:
- Distribusi 80% bandwidth ke zona esensial selama jam kegiatan utama.
- Pengurangan keluhan koneksi lambat di area akademik hingga 90%.
- Waktu setup operasional harian atau QoS override melalui web di bawah 1 menit.
- Eksekusi perubahan limitasi atau queue tree pada router kurang dari 5 detik setelah jadwal aktif.

## 5. Stakeholder
**Super Admin / Network Engineer**  
Bertanggung jawab atas konfigurasi integrasi router, pengaturan node atau access point, setup topologi, serta manajemen pengguna.

**School Staff / Operator**  
Bertanggung jawab memantau dashboard trafik, melihat peta kampus, mengatur schedule presets, dan menggunakan master switch saat ada kebutuhan khusus.

**Manajemen Institusi Pendidikan**  
Berkepentingan pada efisiensi biaya, stabilitas layanan internet, dan peningkatan kualitas operasional jaringan.

**Pengguna Akhir (Siswa, Guru, Staf Akademik)**  
Menerima dampak langsung berupa konektivitas yang lebih stabil terutama di area pembelajaran.

## 6. Ruang Lingkup Bisnis
### In Scope
- Pengaturan alokasi bandwidth berbasis waktu.
- Pengelompokan zona jaringan berdasarkan lokasi, access point, IP address, atau MAC address.
- Pemetaan kampus/sekolah dalam tampilan visual interaktif.
- Penerapan bandwidth shaping untuk zona prioritas dan zona publik.
- Dashboard analitik trafik real-time.
- Fitur manual override melalui master switch.
- Pencatatan log eksekusi aturan jadwal.
- Autentikasi pengguna untuk akses aplikasi.

### Out of Scope
- Penambahan kapasitas bandwidth dari ISP.
- Penggantian infrastruktur fisik jaringan kampus.
- Manajemen akademik di luar tata kelola jaringan internet.

## 7. Pernyataan Kebutuhan Bisnis
### BR-01 Optimasi Bandwidth Berbasis Waktu
Bisnis membutuhkan sistem yang dapat mengalokasikan bandwidth secara otomatis berdasarkan jadwal operasional, sehingga area akademik memperoleh prioritas koneksi pada jam belajar.

### BR-02 Tata Kelola Bandwidth Berbasis Wilayah
Bisnis membutuhkan kemampuan untuk mengelompokkan area jaringan ke dalam zona prioritas dan non-prioritas agar kebijakan alokasi dapat diterapkan sesuai fungsi area.

### BR-03 Visibilitas Operasional Real-Time
Bisnis membutuhkan tampilan visual yang memperlihatkan kondisi trafik per gedung, per access point, atau per zona secara langsung untuk mempercepat pengambilan keputusan.

### BR-04 Penyederhanaan Operasional
Bisnis membutuhkan antarmuka web yang mengurangi ketergantungan pada konfigurasi manual berbasis terminal atau Winbox, sehingga staf operasional dapat menjalankan proses harian dengan lebih cepat dan aman.

### BR-05 Respons Cepat untuk Kebutuhan Khusus
Bisnis membutuhkan fitur override agar kebijakan otomatis dapat dihentikan atau diubah seketika untuk mendukung acara mendadak seperti ujian atau kegiatan khusus di luar jadwal normal.

### BR-06 Efisiensi Biaya Jaringan
Bisnis membutuhkan solusi yang mengoptimalkan pemanfaatan bandwidth yang sudah tersedia sehingga institusi tidak perlu segera melakukan upgrade langganan bandwidth yang mahal.

### BR-07 Keamanan dan Kontrol Akses
Bisnis membutuhkan mekanisme autentikasi sesi pengguna dan komunikasi aman ke router untuk menjaga integritas pengelolaan jaringan.

## 8. Kapabilitas Bisnis yang Harus Tersedia
- Pengelolaan schedule presets untuk jam operasional.
- Auto-switch mode untuk mengeksekusi profil QoS saat jadwal aktif dan mengembalikan profil standar setelah jam selesai.
- Interactive campus map untuk memvisualisasikan node jaringan dan status beban.
- Zone grouping untuk pemetaan logis area akademik dan area umum.
- Dynamic bandwidth shaper untuk pembatasan zona publik saat jam krusial.
- Dynamic throttling di luar jam utama untuk distribusi bandwidth yang lebih merata.
- Manual override melalui master switch.
- Real-time analytics tanpa reload halaman.
- Logging atas eksekusi aturan jaringan.

## 9. Aturan Bisnis Utama
- Pada jam sekolah, zona esensial seperti kelas dan laboratorium harus mendapatkan prioritas bandwidth utama.
- Zona luar atau area umum harus menerima pembatasan bandwidth saat jam kritis.
- Setelah jam utama berakhir, sistem harus mengembalikan distribusi bandwidth ke mode dinamis atau merata.
- Jika master switch diaktifkan, aturan jadwal otomatis harus diabaikan dan prioritas diterapkan ke zona yang ditentukan.
- Setiap perubahan aturan yang dieksekusi sistem harus tercatat dalam log.

## 10. Proses Bisnis Tingkat Tinggi
1. Admin atau operator mendefinisikan topologi, node, access point, dan kelompok zona.
2. Admin atau operator menetapkan schedule presets berdasarkan jam kegiatan institusi.
3. Saat waktu jadwal tercapai, sistem mengeksekusi aturan QoS ke router secara otomatis.
4. Dashboard menampilkan status trafik per zona dan per node secara real-time.
5. Jika ada kebutuhan khusus, operator mengaktifkan master switch untuk override kebijakan otomatis.
6. Setelah override dinonaktifkan atau jadwal berakhir, sistem kembali ke aturan distribusi yang sesuai.

## 11. Risiko dan Ketergantungan Bisnis
### Ketergantungan
- Ketersediaan integrasi dengan perangkat MikroTik RouterOS API atau SSH.
- Akurasi data topologi, pemetaan access point, dan klasifikasi zona.
- Ketersediaan koneksi aman antara backend dan router.

### Risiko
- Pemetaan zona yang tidak akurat dapat menyebabkan alokasi bandwidth tidak sesuai kebutuhan akademik.
- Ketergantungan pada konfigurasi awal yang salah dapat memengaruhi kualitas implementasi otomatisasi.
- Resistensi operasional dapat terjadi apabila SOP penggunaan override dan jadwal belum ditetapkan dengan jelas.

## 12. Asumsi Bisnis
- Institusi telah memiliki infrastruktur jaringan dan bandwidth aktif yang dapat dioptimalkan.
- Perangkat router mendukung integrasi yang dibutuhkan oleh sistem.
- Staf operasional bersedia menggunakan dashboard web sebagai sarana utama pengendalian jaringan.

## 13. Nilai Bisnis yang Diharapkan
Implementasi 3T Controller diharapkan memberi nilai bisnis berupa peningkatan kualitas koneksi di area pembelajaran, efisiensi pemanfaatan bandwidth, percepatan operasional pengelolaan jaringan, penurunan ketergantungan pada tenaga teknis khusus, dan penghematan biaya dibanding pendekatan upgrade bandwidth sebagai solusi utama.

## 14. Kesimpulan
Berdasarkan PRD, 3T Controller merupakan inisiatif transformasi tata kelola jaringan kampus yang berfokus pada otomatisasi, visualisasi, dan efisiensi. Dari perspektif bisnis, solusi ini relevan karena langsung menjawab masalah pemborosan bandwidth, kompleksitas operasional, dan kurangnya visibilitas jaringan. BRD ini menjadi dasar untuk menyelaraskan kebutuhan stakeholder bisnis dengan implementasi solusi pada tahap desain dan pengembangan berikutnya.
