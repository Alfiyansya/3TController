# Technical / System Design Document (TDD / HLD)
## Project: 3T Controller (Time, Territory, & Traffic Controller)
## Versi: 0.1
## Status: DRAFT_PENDING_FINALIZATION
## Basis Dokumen: PRD, BRD, FSD yang tersedia

---

## 1. Tujuan Dokumen
Dokumen ini mendefinisikan rancangan teknis tingkat tinggi untuk sistem 3T Controller sebagai jembatan dari dokumen kebutuhan bisnis dan fungsional menuju tahap desain rinci, implementasi, testing, dan deployment.

Dokumen ini disusun hanya berdasarkan konteks yang tersedia pada:
- 3T_Controller_Project_Brief_PRD.md
- BRD_3T_Controller.md
- FSD_3T_Controller.md

Bagian yang belum disebutkan secara eksplisit pada dokumen sumber ditandai sebagai **TBD** dan harus diputuskan pada dokumen turunan berikutnya.

## 2. Ringkasan Solusi
3T Controller adalah platform web terpusat untuk institusi pendidikan yang mengotomatisasi distribusi bandwidth berdasarkan waktu kegiatan, zona lokasi jaringan, dan kebutuhan operasional khusus melalui override manual.

Secara teknis, sistem ini harus:
- Menyediakan antarmuka web untuk monitoring, konfigurasi, zoning, schedule, dan override.
- Mengeksekusi kebijakan bandwidth ke perangkat MikroTik melalui RouterOS API atau SSH.
- Menjalankan auto-switch QoS berbasis jadwal secara akurat.
- Menyediakan data trafik real-time per zona dan per node tanpa reload halaman.
- Menyimpan log eksekusi aturan, perubahan router, dan aktivitas override.

## 3. Driver Arsitektur

### 3.1 Driver Bisnis
Arsitektur harus mendukung sasaran bisnis berikut:
- Distribusi 80% bandwidth ke zona esensial selama jam kegiatan utama.
- Pengurangan keluhan koneksi lambat di area akademik hingga 90%.
- Waktu setup operasional atau QoS override melalui web di bawah 1 menit.
- Eksekusi perubahan queue tree atau limitasi router kurang dari 5 detik setelah jadwal aktif.

### 3.2 Driver Fungsional
Arsitektur harus mendukung kapabilitas berikut:
- Autentikasi dan otorisasi pengguna.
- Manajemen topologi, node, dan access point.
- Pengelompokan zona berdasarkan lokasi, access point, IP address, atau MAC address.
- Peta kampus interaktif.
- Schedule presets berbasis waktu.
- Traffic shaping untuk zona prioritas dan zona publik.
- Manual override melalui master switch.
- Dashboard trafik real-time.
- Logging dan audit operasional.
- Integrasi aman ke router MikroTik.

### 3.3 Driver Non-Fungsional
Arsitektur harus memenuhi kebutuhan berikut:
- Komunikasi aman backend ke router melalui SSH atau API dengan TLS.
- Autentikasi sesi pengguna berbasis JWT.
- Update dashboard real-time minimal setiap 1 detik tanpa reload halaman.
- Mekanisme retry scheduler maksimum 3 kali saat timeout router.
- Usability yang cukup sederhana untuk operator non-spesialis jaringan.

## 4. Prinsip Desain
Prinsip desain tingkat tinggi untuk sistem ini adalah:
- **Centralized control**: seluruh kontrol operasional dilakukan dari web dashboard.
- **Policy-driven execution**: schedule, zoning, shaping, dan override menjadi sumber kebijakan yang dieksekusi ke router.
- **Separation of concerns**: UI, business logic, scheduler, real-time streaming, dan network execution dipisahkan secara logis.
- **Secure by default**: seluruh akses aplikasi dan koneksi ke router harus melalui mekanisme aman.
- **Operational traceability**: seluruh perubahan penting harus tercatat pada log operasional.
- **Graceful failure**: kegagalan komunikasi router atau feed real-time tidak boleh menimbulkan perilaku sistem yang tidak terkontrol.

## 5. Stakeholder Teknis dan Pengguna

### 5.1 Super Admin / Network Engineer
Fokus utama:
- Konfigurasi integrasi router.
- Pengaturan node dan access point.
- Setup topologi.
- Manajemen user.

### 5.2 School Staff / Operator
Fokus utama:
- Monitoring dashboard trafik.
- Pengamatan peta kampus.
- Pengelolaan schedule presets.
- Eksekusi manual override.

### 5.3 Manajemen Institusi
Fokus utama:
- Monitoring hasil operasional secara tidak langsung.
- Kepastian efisiensi bandwidth dan kualitas layanan.

## 6. Arsitektur Solusi Tingkat Tinggi
Berdasarkan PRD, solusi terdiri dari komponen utama berikut.

### 6.1 Web UI Layer
Komponen frontend web yang bertanggung jawab untuk:
- Login dan session handling.
- Dashboard trafik real-time.
- Peta kampus interaktif.
- Manajemen node, access point, zona, dan schedule.
- Aktivasi dan deaktivasi master switch.
- Menampilkan log operasional.

Teknologi kandidat yang disebut dalam dokumen sumber:
- Next.js (React)
- Tailwind CSS
- Leaflet.js
- Recharts atau Chart.js

### 6.2 Backend Control Engine
Komponen backend yang bertanggung jawab untuk:
- Menyediakan API aplikasi.
- Memproses logika bisnis.
- Menjalankan validasi aturan.
- Mengelola data master dan data operasional.
- Menjadi orkestrator antara UI, scheduler, logging, dan router integration layer.

Teknologi kandidat yang disebut dalam dokumen sumber:
- Node.js (Express) atau Python (FastAPI)

### 6.3 Scheduler / Automation Engine
Komponen yang bertanggung jawab untuk:
- Mengevaluasi schedule aktif.
- Men-trigger auto-switch QoS ketika waktu terpenuhi.
- Mengembalikan kebijakan bandwidth ke mode standar setelah jadwal berakhir.
- Melakukan retry maksimum 3 kali jika router timeout.

Teknologi kandidat yang disebut dalam dokumen sumber:
- Celery (Python)
- Node-Cron + Redis

### 6.4 Network Integration Layer
Komponen eksekusi yang bertanggung jawab untuk:
- Mengirim command atau script ke perangkat MikroTik.
- Menjalankan perubahan queue tree atau limitasi bandwidth.
- Menjadi lapisan eksekusi untuk schedule, traffic policy, dan override.
- Mencatat hasil sukses atau gagal eksekusi.

Metode integrasi yang disebut dalam dokumen sumber:
- MikroTik RouterOS API
- SSH

### 6.5 Real-Time Communication Layer
Komponen yang bertanggung jawab untuk:
- Men-stream data Tx/Rx ke dashboard frontend.
- Menyediakan update berkala tanpa reload halaman.
- Menjadi kanal untuk status real-time node atau zona.

Teknologi kandidat yang disebut dalam dokumen sumber:
- Socket.io
- WebSockets

### 6.6 Persistence Layer
Dokumen sumber menyebut backend mengelola database dan log operasional, sehingga sistem membutuhkan persistence layer untuk menyimpan data master, konfigurasi, log, dan data trafik. Jenis database dan desain fisiknya belum ditentukan pada dokumen sumber dan tetap **TBD**.

## 7. Logical Architecture
Secara logis, sistem dibagi ke dalam domain modul berikut.

### 7.1 Authentication & Authorization Module
Tanggung jawab:
- Login pengguna.
- Validasi sesi.
- Pemisahan hak akses minimal antara Super Admin dan Operator.
- Penolakan akses tidak sah ke fungsi terbatas.

### 7.2 Topology Management Module
Tanggung jawab:
- CRUD node jaringan.
- CRUD access point.
- Pengaitan node atau access point ke lokasi di peta.
- Menyediakan struktur dasar untuk zoning dan monitoring.

### 7.3 Zone Management Module
Tanggung jawab:
- Membuat dan mengelola kelompok zona.
- Mengelompokkan entitas berdasarkan lokasi, access point, IP, atau MAC.
- Menyediakan relasi zona terhadap node atau access point.
- Menyediakan data zona untuk schedule, shaping, analytics, dan override.

### 7.4 Interactive Map Module
Tanggung jawab:
- Menampilkan peta kampus atau sekolah dalam tampilan 2D.
- Menampilkan posisi node atau access point.
- Menunjukkan status normal atau overload melalui perubahan warna.

### 7.5 Schedule Management Module
Tanggung jawab:
- Membuat schedule presets.
- Menentukan jam aktif dan jam selesai.
- Mengaktifkan auto-switch QoS berbasis waktu.
- Mengembalikan sistem ke distribusi standar setelah jadwal berakhir.

### 7.6 Traffic Policy Module
Tanggung jawab:
- Menentukan kebijakan shaping untuk zona prioritas dan publik.
- Mengaktifkan pembatasan untuk area umum saat jam kritis.
- Mengaktifkan distribusi lebih merata di luar jam utama.
- Menerjemahkan kebijakan ke format eksekusi router.

### 7.7 Override Management Module
Tanggung jawab:
- Menyediakan master switch operasional.
- Menonaktifkan aturan jadwal otomatis saat override aktif.
- Menetapkan prioritas ke zona custom yang valid.
- Mengembalikan sistem ke aturan sesuai kondisi waktu aktif setelah override selesai.

### 7.8 Router Execution Module
Tanggung jawab:
- Menjadi adapter antara policy internal dan command router.
- Menjalankan perubahan queue tree atau limitasi.
- Menjaga keamanan koneksi ke router.
- Menghasilkan hasil eksekusi untuk logging dan monitoring.

### 7.9 Real-Time Monitoring Module
Tanggung jawab:
- Mengambil atau menerima data trafik Tx/Rx.
- Menyajikan data real-time per zona dan per node.
- Menjaga tampilan terakhir saat feed sementara terputus.

### 7.10 Logging & Audit Module
Tanggung jawab:
- Mencatat eksekusi jadwal.
- Mencatat perubahan router.
- Mencatat aktivasi dan deaktivasi override.
- Menyediakan jejak verifikasi kejadian operasional.

## 8. Alur Utama Sistem

### 8.1 Schedule-Driven QoS Flow
1. Admin atau operator membuat schedule preset.
2. Scheduler mengevaluasi waktu aktif.
3. Saat waktu terpenuhi, scheduler memanggil traffic policy yang relevan.
4. Backend memvalidasi zona dan aturan yang akan diterapkan.
5. Router execution module mengirim perubahan ke MikroTik.
6. Hasil eksekusi dicatat pada log.
7. Dashboard menampilkan status terbaru.
8. Saat jadwal berakhir, sistem mengembalikan distribusi bandwidth ke mode standar atau dinamis.

### 8.2 Manual Override Flow
1. Operator mengaktifkan master switch.
2. Sistem memvalidasi target zona custom.
3. Jika valid, aturan otomatis berbasis waktu diabaikan.
4. Policy override diterapkan ke router melalui router execution module.
5. Aktivasi override dicatat pada log.
6. Saat override dimatikan, sistem kembali ke aturan yang sesuai dengan kondisi waktu aktif saat itu.

### 8.3 Real-Time Monitoring Flow
1. Sistem menerima atau mengambil data trafik dari sumber integrasi jaringan.
2. Data diproses pada backend atau layer real-time.
3. Data dikirim ke frontend melalui WebSocket.
4. Dashboard dan peta memperbarui status zona atau node secara live.
5. Jika feed terputus, tampilan terakhir dipertahankan sampai data baru tersedia.

## 9. Context Interaksi Antar Komponen
Interaksi tingkat tinggi antar komponen adalah sebagai berikut:
- Web UI berkomunikasi dengan Backend Control Engine melalui API aplikasi.
- Web UI menerima update trafik dan status melalui kanal WebSocket.
- Backend Control Engine membaca dan menulis data ke Persistence Layer.
- Scheduler memanggil Backend Control Engine atau Router Execution Module untuk menjalankan kebijakan terjadwal.
- Backend Control Engine meneruskan perintah ke Network Integration Layer.
- Network Integration Layer berkomunikasi aman dengan perangkat MikroTik.
- Logging & Audit Module menerima event dari schedule, override, dan router execution.

## 10. Logical Data Domains
Dokumen sumber belum memiliki ERD atau desain database rinci, tetapi secara logis data yang harus ada minimal meliputi:
- User
- Role
- Session atau token context
- Node
- Access Point
- Zone
- Zone Mapping
- Schedule Preset
- Active Policy
- Override State
- Router Integration Config
- Traffic Metric
- Execution Log
- Audit Log

Struktur tabel, relasi, indeks, dan strategi retensi data tetap **TBD** dan harus diturunkan pada dokumen desain database.

## 11. Security Design Tingkat Tinggi
Berdasarkan dokumen sumber, kontrol keamanan minimum adalah:
- Autentikasi sesi pengguna dengan JWT.
- Otorisasi berbasis peran minimal untuk Super Admin dan Operator.
- Komunikasi aman antara backend dan router melalui SSH atau API dengan TLS.
- Penolakan akses tidak sah terhadap fungsi administrasi dan fungsi terbatas.
- Pencatatan percobaan akses tidak sah sebagai bagian dari exception handling dan audit.

Detail lanjutan seperti manajemen secret, rotasi kredensial, kebijakan password, session expiration, dan hardening deployment belum tersedia pada dokumen sumber dan tetap **TBD**.

## 12. Reliability dan Exception Design
Berdasarkan PRD dan FSD, perilaku minimum saat gangguan adalah:
- Jika koneksi ke router gagal, sistem menggagalkan eksekusi perubahan dan mencatat kegagalan.
- Jika pemetaan zona tidak valid, sistem mencegah penerapan kebijakan terkait zona tersebut.
- Jika eksekusi jadwal gagal, sistem mencatat kejadian sebagai kegagalan operasional.
- Jika override diaktifkan tanpa target zona valid, sistem menolak penerapan override.
- Jika feed real-time terputus, sistem mempertahankan tampilan terakhir.
- Scheduler harus memiliki retry maksimum 3 kali saat timeout router.

Strategi fallback teknis yang lebih rinci tetap **TBD**.

## 13. Deployment View Tingkat Tinggi
Dokumen sumber menyebut kebutuhan arsitektur aplikasi, scheduler, komunikasi real-time, dan integrasi router, namun belum menetapkan topologi deployment rinci. Maka deployment view tingkat tinggi yang dapat diturunkan saat ini adalah:
- Satu komponen Web UI untuk operasional pengguna.
- Satu komponen Backend Control Engine untuk API dan orkestrasi bisnis.
- Satu komponen Scheduler/Worker untuk auto-switch berbasis waktu.
- Satu komponen Real-Time channel untuk streaming trafik.
- Satu Persistence Layer untuk data master dan log.
- Secure connectivity dari backend ke perangkat MikroTik.

Pilihan environment, containerization, reverse proxy, segmentasi jaringan, HA, dan backup-restore tetap **TBD**.

## 14. Traceability Tingkat Tinggi
Pemetaan kebutuhan ke modul teknis adalah sebagai berikut:
- FR-AUTH → Authentication & Authorization Module
- FR-TOPO → Topology Management Module
- FR-ZONE → Zone Management Module
- FR-MAP → Interactive Map Module
- FR-TIME → Schedule Management Module + Scheduler
- FR-TRAF → Traffic Policy Module + Router Execution Module
- FR-OVR → Override Management Module
- FR-DASH → Real-Time Monitoring Module + Web UI
- FR-LOG → Logging & Audit Module
- FR-INT → Network Integration Layer
- NFR Security → Security Design
- NFR Performance & Reliability → Scheduler, Real-Time Layer, Router Execution Layer

## 15. Open Design Decisions dan Dokumen Turunan yang Dibutuhkan
- Final choice backend utama: Express atau FastAPI.
- Final choice scheduler: Celery atau Node-Cron + Redis.
- Kontrak API detail dan event WebSocket.
- Desain database rinci dan ERD.
- Definisi policy model untuk queue tree atau limitasi router.
- Desain deployment environment dev, staging, dan production.
- Observability, alerting, dan monitoring teknis.
- SOP operasional override dan schedule.
- Strategi testing unit, integration, system, dan UAT.
- [OPEN_ITEMS_TO_BE_REFINED]

## 16. Kesimpulan
Berdasarkan PRD, BRD, dan FSD yang ada, 3T Controller secara arsitektural adalah sistem web terpusat dengan pola kontrol berbasis kebijakan yang menghubungkan UI operasional, backend orchestration, scheduler otomatis, integrasi aman ke router MikroTik, dan monitoring real-time.

Dokumen ini dapat dijadikan baseline TDD/HLD awal untuk memulai tahap desain rinci berikutnya, dengan catatan bahwa keputusan implementasi detail yang belum ada pada dokumen sumber harus dituangkan pada dokumen turunan seperti API Specification, Database Design, Deployment Architecture, dan Test Plan.
