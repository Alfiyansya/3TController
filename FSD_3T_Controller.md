# Functional Specification Document (FSD)
## Project: 3T Controller (Time, Territory, & Traffic Controller)
## Versi: 0.1
## Status: Draft awal berbasis PRD & BRD

## 1. Tujuan Dokumen
Dokumen ini mendefinisikan spesifikasi fungsional untuk sistem 3T Controller sebagai dasar bersama antara stakeholder bisnis, tim produk, tim UI/UX, dan tim engineering sebelum masuk ke tahap perancangan modul sistem dan arsitektur aplikasi.

## 2. Ringkasan Produk
3T Controller adalah platform web terpusat untuk institusi pendidikan yang mengotomatisasi distribusi bandwidth berdasarkan waktu operasional dan zona lokasi jaringan, serta menyediakan monitoring trafik secara visual dan real-time.

Sistem ini dirancang untuk menyelesaikan masalah pemborosan bandwidth, pengelolaan jaringan yang masih manual, dan rendahnya visibilitas terhadap kondisi trafik di area kampus atau sekolah.

## 3. Tujuan Bisnis
Berdasarkan dokumen sumber, sistem harus mendukung tujuan berikut:
- Mengalokasikan 80% bandwidth ke zona esensial selama jam kegiatan utama.
- Mengurangi keluhan koneksi lambat di area akademik hingga 90%.
- Menyediakan waktu setup operasional harian atau QoS override melalui web di bawah 1 menit.
- Mengeksekusi perubahan limitasi atau queue tree pada router kurang dari 5 detik setelah jadwal aktif.

## 4. Stakeholder dan Peran Pengguna
### 4.1 Super Admin / Network Engineer
Tanggung jawab utama:
- Konfigurasi integrasi router.
- Pengaturan node dan access point.
- Setup topologi.
- Manajemen pengguna.

### 4.2 School Staff / Operator
Tanggung jawab utama:
- Memantau dashboard trafik.
- Melihat peta kampus.
- Mengatur schedule presets.
- Menggunakan master switch untuk override kebutuhan khusus.

### 4.3 Manajemen Institusi
Kepentingan utama:
- Efisiensi biaya jaringan.
- Stabilitas layanan internet.
- Peningkatan kualitas operasional jaringan.

### 4.4 Pengguna Akhir
Penerima dampak utama berupa konektivitas yang lebih stabil di area pembelajaran.

## 5. Ruang Lingkup Fungsional
### 5.1 In Scope
- Pengaturan alokasi bandwidth berbasis waktu.
- Pengelompokan zona jaringan berdasarkan lokasi, access point, IP address, atau MAC address.
- Pemetaan kampus atau sekolah secara visual interaktif.
- Penerapan bandwidth shaping untuk zona prioritas dan zona publik.
- Dashboard analitik trafik real-time.
- Manual override melalui master switch.
- Logging eksekusi aturan jadwal.
- Autentikasi pengguna.

### 5.2 Out of Scope
- Penambahan kapasitas bandwidth dari ISP.
- Penggantian infrastruktur fisik jaringan kampus.
- Manajemen akademik di luar tata kelola jaringan internet.

## 6. Asumsi dan Ketergantungan
### 6.1 Asumsi
- Institusi sudah memiliki infrastruktur jaringan dan bandwidth aktif.
- Router mendukung integrasi yang dibutuhkan sistem.
- Staf operasional bersedia memakai dashboard web sebagai sarana utama pengendalian.

### 6.2 Ketergantungan
- Integrasi dengan MikroTik RouterOS API atau SSH.
- Akurasi data topologi, access point, dan klasifikasi zona.
- Koneksi aman antara backend dan router.

## 7. Proses Bisnis Tingkat Tinggi
1. Admin atau operator mendefinisikan topologi, node, access point, dan kelompok zona.
2. Admin atau operator menetapkan schedule presets berdasarkan jam kegiatan institusi.
3. Saat waktu jadwal tercapai, sistem mengeksekusi aturan QoS ke router secara otomatis.
4. Dashboard menampilkan status trafik per zona dan per node secara real-time.
5. Jika ada kebutuhan khusus, operator mengaktifkan master switch untuk override kebijakan otomatis.
6. Setelah override dinonaktifkan atau jadwal berakhir, sistem kembali ke aturan distribusi yang sesuai.

## 8. Functional Requirements

Bagian ini merinci kebutuhan fungsional sistem yang wajib tersedia berdasarkan proses bisnis, ruang lingkup, dan kapabilitas yang disebutkan dalam PRD dan BRD.

### 8.1 Authentication and Authorization
**FR-AUTH-01** Sistem harus menyediakan mekanisme login untuk pengguna terdaftar.

**FR-AUTH-02** Sistem harus menerapkan autentikasi sesi pengguna untuk mengontrol akses aplikasi.

**FR-AUTH-03** Sistem harus membedakan hak akses minimal antara Super Admin / Network Engineer dan School Staff / Operator.

**FR-AUTH-04** Sistem harus membatasi fitur administrasi router, topologi, zoning, dan manajemen user hanya untuk peran yang berwenang.

### 8.2 Topology, Node, and Access Point Management
**FR-TOPO-01** Sistem harus memungkinkan Super Admin / Network Engineer menambahkan, mengubah, dan mengelola data node jaringan.

**FR-TOPO-02** Sistem harus memungkinkan Super Admin / Network Engineer menambahkan, mengubah, dan mengelola data access point.

**FR-TOPO-03** Sistem harus memungkinkan pengaitan node atau access point ke lokasi atau area tertentu pada peta kampus atau sekolah.

**FR-TOPO-04** Sistem harus menyediakan representasi topologi atau sebaran node sebagai dasar zoning dan monitoring.

### 8.3 Territory / Zone Grouping Management
**FR-ZONE-01** Sistem harus memungkinkan admin membuat kelompok zona jaringan.

**FR-ZONE-02** Sistem harus memungkinkan pengelompokan berdasarkan lokasi, access point, IP address, atau MAC address.

**FR-ZONE-03** Sistem harus memungkinkan pemberian label zona sesuai kebutuhan institusi, misalnya area akademik, laboratorium, kelas, area umum, kantin, atau lapangan.

**FR-ZONE-04** Sistem harus menyimpan relasi antara zona dan node atau access point yang termasuk di dalamnya.

**FR-ZONE-05** Sistem harus menyediakan data zona untuk dipakai oleh mekanisme schedule, shaping bandwidth, analytics, dan override.

### 8.4 Interactive Campus Map
**FR-MAP-01** Sistem harus menampilkan peta kampus atau sekolah dalam bentuk visual interaktif 2D.

**FR-MAP-02** Sistem harus menampilkan posisi gedung, node, atau access point pada peta.

**FR-MAP-03** Sistem harus menampilkan indikator status beban node atau zona melalui perubahan warna.

**FR-MAP-04** Sistem harus memungkinkan operator melihat kondisi area prioritas dan area umum dari satu tampilan terpusat.

### 8.5 Time Management / Schedule Presets
**FR-TIME-01** Sistem harus memungkinkan operator atau admin membuat schedule presets berdasarkan jam kegiatan institusi.

**FR-TIME-02** Sistem harus memungkinkan definisi jam aktif dan jam berakhir untuk setiap preset.

**FR-TIME-03** Sistem harus mengeksekusi profil QoS secara otomatis ketika waktu jadwal aktif tercapai.

**FR-TIME-04** Sistem harus mengembalikan profil distribusi bandwidth ke mode standar, dinamis, atau merata setelah jadwal berakhir.

**FR-TIME-05** Sistem harus menjalankan auto-switch tanpa campur tangan manual pada kondisi normal.

### 8.6 Traffic Management
**FR-TRAF-01** Sistem harus menerapkan dynamic bandwidth shaper untuk zona publik saat jam krusial.

**FR-TRAF-02** Sistem harus memprioritaskan zona esensial seperti kelas dan laboratorium selama jam kegiatan utama.

**FR-TRAF-03** Sistem harus membatasi bandwidth zona luar atau area umum saat jam kritis.

**FR-TRAF-04** Sistem harus mendukung distribusi bandwidth yang lebih merata di luar jam utama melalui dynamic throttling.

**FR-TRAF-05** Sistem harus mengeksekusi perubahan aturan bandwidth ke router melalui integrasi yang tersedia.

### 8.7 Manual Override / Master Switch
**FR-OVR-01** Sistem harus menyediakan master switch pada dashboard.

**FR-OVR-02** Sistem harus memungkinkan operator menghentikan aturan jadwal otomatis secara instan.

**FR-OVR-03** Ketika override aktif, sistem harus mengabaikan otomatisasi berbasis waktu.

**FR-OVR-04** Sistem harus memungkinkan prioritas diterapkan ke zona custom yang ditentukan untuk kebutuhan khusus.

**FR-OVR-05** Setelah override dinonaktifkan, sistem harus kembali ke aturan distribusi yang sesuai dengan kondisi waktu aktif saat itu.

### 8.8 Real-Time Monitoring and Analytics
**FR-DASH-01** Sistem harus menampilkan dashboard trafik berbasis web.

**FR-DASH-02** Sistem harus menampilkan pergerakan trafik Tx/Rx secara live tanpa reload halaman.

**FR-DASH-03** Sistem harus menampilkan data trafik minimal per zona dan per node.

**FR-DASH-04** Sistem harus memberi informasi yang membantu operator mendeteksi area normal dan area overload.

**FR-DASH-05** Dashboard harus menjadi titik kontrol utama untuk monitoring dan override operasional.

### 8.9 Logging and Operational Audit
**FR-LOG-01** Sistem harus mencatat setiap eksekusi aturan jadwal.

**FR-LOG-02** Sistem harus mencatat perubahan yang dieksekusi sistem ke router.

**FR-LOG-03** Sistem harus mencatat aktivasi dan penonaktifan manual override.

**FR-LOG-04** Sistem harus menyediakan catatan yang dapat digunakan untuk verifikasi kejadian operasional.

### 8.10 Router Integration Execution
**FR-INT-01** Sistem harus mendukung integrasi ke perangkat MikroTik melalui RouterOS API atau SSH.

**FR-INT-02** Sistem harus mampu mengeksekusi perubahan limitasi atau queue tree sesuai aturan yang aktif.

**FR-INT-03** Sistem harus menggunakan koneksi aman untuk komunikasi antara backend dan router.

**FR-INT-04** Sistem harus menjadi lapisan eksekusi atas perintah yang berasal dari schedule, traffic policy, dan override.

## 9. Non-Functional Requirements

Bagian ini merangkum kebutuhan non-fungsional yang disebutkan langsung di PRD dan BRD atau diturunkan secara langsung dari target operasional yang dinyatakan.

**NFR-01 Security** Sistem harus menggunakan komunikasi aman antara backend dan router melalui koneksi aman berbasis SSH atau API dengan TLS.

**NFR-02 Session Security** Sistem harus menerapkan autentikasi sesi pengguna untuk menjaga integritas akses aplikasi.

**NFR-03 Performance - Router Execution** Sistem harus mengeksekusi perubahan limitasi atau queue tree pada router kurang dari 5 detik setelah jadwal aktif terpenuhi.

**NFR-04 Operational Efficiency** Penggunaan fitur operasional harian atau QoS override melalui web harus dapat dilakukan di bawah 1 menit.

**NFR-05 Real-Time Update** Dashboard harus mendukung pembaruan data real-time tanpa reload halaman.

**NFR-06 Usability** Antarmuka web harus cukup mudah digunakan oleh staf non-spesialis jaringan sebagai pengganti operasi manual via Winbox atau terminal.

## 10. Business Rules

Aturan bisnis berikut berasal langsung dari BRD dan PRD sebagai kebijakan inti sistem.

**BR-01** Pada jam sekolah atau jam kegiatan utama, zona esensial seperti kelas dan laboratorium harus mendapatkan prioritas bandwidth utama.

**BR-02** Zona luar atau area umum harus menerima pembatasan bandwidth saat jam kritis.

**BR-03** Setelah jam utama berakhir, sistem harus mengembalikan distribusi bandwidth ke mode dinamis, merata, atau profil standar yang berlaku.

**BR-04** Jika master switch diaktifkan, aturan jadwal otomatis harus diabaikan.

**BR-05** Saat override aktif, prioritas bandwidth harus diterapkan ke zona custom yang ditentukan untuk kebutuhan khusus.

**BR-06** Setiap perubahan aturan yang dieksekusi sistem harus tercatat dalam log.

**BR-07** Kebijakan bandwidth harus dijalankan berdasarkan pengelompokan zona yang dibangun dari lokasi, access point, IP address, atau MAC address.

## 11. System Behavior

Bagian ini menjelaskan perilaku sistem pada kondisi operasional utama.

### 11.1 Behavior Saat Login
- Pengguna melakukan login ke aplikasi.
- Sistem memverifikasi bahwa pengguna terdaftar dan memiliki peran yang sesuai.
- Sistem menampilkan fitur sesuai hak akses pengguna.

### 11.2 Behavior Saat Setup Topologi dan Zona
- Admin mendefinisikan node, access point, dan topologi jaringan.
- Admin mengaitkan komponen jaringan ke lokasi tertentu pada peta.
- Admin membentuk kelompok zona prioritas dan non-prioritas.
- Data tersebut menjadi dasar bagi schedule, traffic policy, monitoring, dan override.

### 11.3 Behavior Saat Schedule Aktif
- Operator atau admin membuat preset jadwal operasional.
- Ketika waktu aktif tercapai, sistem menjalankan auto-switch QoS secara otomatis.
- Sistem mengirim aturan shaping atau queue tree ke router.
- Sistem mencatat eksekusi tersebut dalam log operasional.

### 11.4 Behavior Saat Jam Prioritas Berakhir
- Sistem menghentikan profil prioritas yang aktif.
- Sistem mengembalikan distribusi bandwidth ke mode dinamis, merata, atau standar.
- Dashboard terus menampilkan dampak perubahan secara real-time.

### 11.5 Behavior Saat Override Aktif
- Operator mengaktifkan master switch dari dashboard.
- Sistem mengabaikan otomatisasi jadwal.
- Sistem menerapkan prioritas ke zona custom yang dibutuhkan untuk acara khusus.
- Semua aksi override harus dicatat dalam log.

### 11.6 Behavior Saat Monitoring Berjalan
- Dashboard menerima pembaruan trafik Tx/Rx secara live.
- Sistem menampilkan status zona dan node untuk membantu identifikasi area normal dan overload.
- Peta interaktif menampilkan indikasi warna berdasarkan status beban.

## 12. Validations

Bagian ini mendefinisikan validasi minimum yang diperlukan agar perilaku sistem tetap konsisten dengan ruang lingkup dan aturan bisnis yang tercantum di dokumen sumber.

**VAL-01 Login Validation** Sistem hanya boleh memberikan akses kepada pengguna terdaftar.

**VAL-02 Role Validation** Sistem harus memvalidasi peran pengguna sebelum menampilkan atau mengizinkan akses ke fungsi administrasi router, topologi, zoning, dan manajemen pengguna.

**VAL-03 Schedule Validation** Setiap schedule preset harus memiliki waktu mulai dan waktu berakhir yang terdefinisi.

**VAL-04 Zone Mapping Validation** Setiap zona harus memiliki dasar pengelompokan yang jelas, yaitu lokasi, access point, IP address, atau MAC address.

**VAL-05 Override Validation** Saat master switch diaktifkan untuk kebutuhan khusus, sistem harus memastikan zona custom tujuan telah ditentukan sebelum prioritas diterapkan.

**VAL-06 Topology Consistency Validation** Node atau access point yang dipakai untuk monitoring dan zoning harus telah terdaftar dalam data topologi.

**VAL-07 Logging Validation** Setiap perubahan aturan otomatis maupun manual harus menghasilkan catatan log operasional.

## 13. Integration Needs

Kebutuhan integrasi berikut disebutkan langsung dalam PRD dan BRD.

**INT-01 MikroTik Integration** Sistem memerlukan integrasi dengan perangkat MikroTik melalui RouterOS API atau SSH.

**INT-02 Secure Communication** Sistem memerlukan koneksi aman antara backend dan router untuk mengeksekusi perintah jaringan.

**INT-03 Scheduler Integration** Sistem memerlukan komponen scheduler untuk menjalankan auto-switch QoS sesuai jadwal secara akurat.

**INT-04 Real-Time Communication** Sistem memerlukan mekanisme komunikasi real-time agar data trafik Tx/Rx dapat dikirim ke dashboard tanpa reload halaman.

**INT-05 Web Application Layer** Sistem memerlukan backend control engine untuk memproses logika bisnis dan frontend web UI untuk operasional harian, visualisasi peta, serta monitoring trafik.

## 14. Reporting Needs

Dokumen sumber tidak menyebut modul reporting analitis formal yang kompleks, tetapi menyebut kebutuhan visibilitas, logging, dan analytics real-time. Berdasarkan itu, kebutuhan pelaporan minimum sistem adalah sebagai berikut.

**REP-01 Real-Time Traffic View** Sistem harus menyediakan tampilan trafik real-time per zona dan per node di dashboard.

**REP-02 Zone Status Visibility** Sistem harus menyediakan tampilan status area atau zona untuk membedakan kondisi normal dan overload.

**REP-03 Execution Log Visibility** Sistem harus menyediakan informasi log atas eksekusi jadwal, perubahan aturan router, dan aktivitas override.

**REP-04 Visual Location Reporting** Sistem harus menyediakan visualisasi berbasis peta untuk menunjukkan posisi access point, node, dan status beban area.

## 15. Exception Handling

Bagian ini mendefinisikan respons minimum sistem terhadap kondisi gangguan yang secara langsung terkait dengan risiko dan ketergantungan yang disebut pada BRD/PRD.

**EXC-01 Router Communication Failure** Jika koneksi ke router tidak tersedia, sistem harus menggagalkan eksekusi perubahan aturan dan mencatat kegagalan tersebut pada log operasional.

**EXC-02 Invalid Zone Mapping** Jika pemetaan zona tidak akurat atau belum lengkap, sistem harus mencegah penerapan kebijakan yang bergantung pada zona tersebut sampai data topologi atau klasifikasi diperbaiki.

**EXC-03 Schedule Execution Failure** Jika aturan jadwal gagal dieksekusi pada waktunya, sistem harus mencatat kejadian tersebut sebagai kegagalan eksekusi untuk kebutuhan verifikasi operasional.

**EXC-04 Override Without Valid Target Zone** Jika override diaktifkan tanpa zona custom yang valid, sistem harus menolak penerapan prioritas override.

**EXC-05 Real-Time Feed Interruption** Jika pembaruan trafik real-time terputus, sistem setidaknya harus mempertahankan tampilan terakhir yang tersedia sampai data baru diterima kembali.

**EXC-06 Unauthorized Access Attempt** Jika pengguna tanpa hak mencoba mengakses fungsi terbatas, sistem harus menolak akses tersebut.

## 16. Risiko Fungsional
- Pemetaan zona yang tidak akurat dapat menyebabkan kebijakan bandwidth salah sasaran.
- Konfigurasi awal yang salah dapat memengaruhi kualitas otomatisasi.
- SOP override yang tidak jelas dapat menimbulkan kebingungan operasional.

## 17. Kriteria Keberterimaan Fungsional
Solusi dapat dianggap memenuhi spesifikasi fungsional awal apabila:
1. Pengguna berwenang dapat login dan mengakses fungsi sesuai peran.
2. Admin dapat mendefinisikan node, access point, dan kelompok zona.
3. Operator dapat membuat schedule preset dan sistem mengeksekusi aturan otomatis saat waktunya tiba.
4. Sistem berhasil menerapkan kebijakan shaping ke router sesuai zona prioritas dan non-prioritas.
5. Operator dapat mengaktifkan master switch untuk override kondisi khusus.
6. Dashboard menampilkan trafik zona atau node secara live.
7. Setiap perubahan aturan tercatat dalam log.
8. Eksekusi perubahan limitasi atau queue tree pada router terjadi kurang dari 5 detik setelah jadwal aktif.

## 18. Batasan Dokumen
Dokumen ini mendefinisikan kebutuhan fungsional dan kebutuhan sistem tingkat tinggi berdasarkan PRD dan BRD yang tersedia. Detail rancangan teknis seperti model data rinci, kontrak API detail, desain database, arsitektur deployment, dan struktur modul aplikasi rinci belum dibahas pada dokumen ini dan akan diturunkan pada tahap desain sistem berikutnya.

## 19. Kesimpulan
Dokumen ini sekarang telah berfungsi sebagai baseline SRS/FSD awal untuk 3T Controller dengan cakupan functional requirements, non-functional requirements, business rules, system behavior, validations, integration needs, reporting needs, dan exception handling. Isinya tetap dibatasi pada hal-hal yang dapat ditarik langsung dari PRD dan BRD, sehingga aman dipakai sebagai acuan tahap analisis dan desain berikutnya.