## 3T Controller

Time, Territory, & Traffic Controller

Fokus: Time-based Bandwidth Allocation & Campus Network Governance

Dokumen: Project Brief & Product Requirements Document (PRD)

Tanggal: 14 Agustus 2026


## Bagian 1: Project Brief

## 1. Executive Summary

3T Controller (Time, Territory, & Traffic Controller) adalah platform web terpusat yang dirancang khusus untuk area pendidikan/kampus. Sistem ini memecahkan masalah alokasi bandwidth yang seringkali tidak efisien dengan mengotomatisasi distribusi jaringan internet berdasarkan jadwal kegiatan (Time) dan zona lokasi (Territory).

Dengan mengubah manajemen router tradisional yang rumit menjadi antarmuka visual interaktif, 3T Controller memungkinkan pengoptimalan Quality of Service (QoS) tanpa memerlukan intervensi manual setiap saat. Solusi ini menghadirkan efisiensi sumber daya, otomatisasi, dan kemudahan manajemen, menghilangkan kebutuhan sekolah untuk membeli bandwidth tambahan yang mahal.

## 2. Problem Statement

- Pemborosan Bandwidth: Pada jam sekolah, bandwidth sering tersedot oleh pengguna di area luar kelas (kantin, area umum), menyebabkan koneksi di lab atau ruang kelas melambat.

- Manajemen Manual yang Rumit: Mengubah pengaturan limitasi bandwidth harian memerlukan keahlian teknis (menggunakan Winbox/Terminal), sehingga rentan terhadap human error dan memberatkan tugas administrator jaringan.

- Kurangnya Visibilitas: Tidak adanya pemetaan visual yang memudahkan staf sekolah melihat beban trafik per gedung atau titik akses secara real-time.

## 3. Solution & Value Proposition ( Poin Plus)

Efisiensi Sumber Daya: Solusi hemat biaya. Sekolah tidak perlu upgrade langganan bandwidth mahal, cukup mengoptimalkan alokasi jaringan yang ada secara cerdas dan tepat sasaran.

Otomatisasi Penuh: Auto-Switch Mode yang menghilangkan kebutuhan intervensi manusia untuk mengubah profil jaringan harian.

UI/UX Interaktif & Mudah: Mengubah CLI/Winbox menjadi dasbor web visual (peta interaktif & grafik). Dapat dioperasikan oleh staf administrasi sekolah tanpa latar belakang Network Engineering yang mendalam.


## Bagian 2: Product Requirements Document (PRD)

## 1. Product Vision & Goals

Visi: Menjadi standar tata kelola jaringan cerdas (Smart Network Governance) untuk institusi pendidikan modern di Indonesia.

## Tujuan (Goals):

- Mendistribusikan 80% bandwidth ke zona esensial selama jam kegiatan utama.

- Mengurangi keluhan koneksi lambat di area akademik hingga 90%.

- Menyediakan waktu setup operasional harian (QoS Override) di bawah 1 menit melalui web master switch.

## 2. User Roles

| Role | Akses & Tanggung Jawab |
| --- | --- |
| Super Admin | Konfigurasi integrasi API Router, pengaturan Node/Access Point, setup |
| (Network Eng.) | Topologi, dan manajemen user. |
|   | Memantau dasbor trafik, melihat peta kampus, menggunakan fitur "Master |
| School Staff / |   |
|   | Switch" (Override) untuk event khusus, dan mengatur jadwal (Schedule |
| Operator |   |
|   | Presets). |

## 3. Key Features & Acceptance Criteria

## 1. Time-Based Automated Rules

Mengatur alokasi berdasarkan waktu tanpa campur tangan admin.

- Schedule Presets: Pembuatan profil jam operasional (misal: "Jam Sekolah" 07.00 - 15.00).

- Auto-Switch Mode: Trigger otomatis yang mengeksekusi profil QoS di router begitu jadwal aktif, dan mengembalikannya ke profil standar (dinamis/rata) setelah jam selesai.


## Acceptance Criteria:

Sistem harus berhasil mengubah limitasi/queue tree di MikroTik dalam waktu kurang dari 5 detik setelah waktu jadwal terpenuhi, dan mencatat log eksekusi.

## Territory 2. Zone & Access Point Control

Pemetaan lokasi untuk alokasi terpusat.

- Interactive Campus Map: Visualisasi peta sekolah 2D (berbasis Leaflet) yang menampilkan letak gedung dan Access Point. Warna node berubah berdasarkan beban (Hijau = Normal, Merah = Overload).

- Zone Grouping: Fitur untuk mengelompokkan IP Address/MAC Address/AP ke dalam kategori zona (Misal: Zona A = Kelas & Lab, Zona B = Kantin & Lapangan).

## Traffic 3. Smart Traffic Management & Override

Eksekusi manajemen bandwidth dan mitigasi dinamis.

- Dynamic Bandwidth Shaper: Logika throttling (pembatasan) untuk zona publik saat jam krusial (80% lab, 20% luar). Di luar jam tersebut, sistem beralih ke Dynamic Throttling (pembagian rata).

- Master Switch (Manual Override): Tombol khusus di dasbor untuk menghentikan aturan jadwal secara instan ketika ada acara mendadak (misal: Ujian ANBK di aula luar jam biasa).

- Real-Time Analytics: Dasbor WebSocket yang menunjukkan pergerakan trafik (Tx/Rx) dari masing- masing zona secara live tanpa page reload.

## 4. System Architecture (Technical Specs)

Arsitektur ini dirancang tangguh, modern, dan scalable, sangat cocok untuk dipresentasikan di hadapan juri kompetisi teknis.


|   | Teknologi / Komponen Fungsi Utama Framework Next.js (React) + Menampilkan UI/UX ramah pengguna, merender Frontend (Web UI) Tailwind CSS SPA (Single Page Application) yang responsif. Interactive Map & Leaflet.js, Recharts / Visualisasi denah kampus interaktif (Zoning) dan Charts Chart.js grafik bandwidth real-time. Backend (Control Node.js (Express) / Memproses logika bisnis, manajemen database, dan Engine) Python (FastAPI) RESTful API untuk klien. |
| --- | --- |
|   | Celery (Python) / Mengeksekusi cron jobs (Auto-Switch QoS) dengan Task Scheduler Node-Cron + Redis akurasi detik sesuai jadwal (Time). Layer komunikasi yang bertugas mengeksekusi Network Integration MikroTik RouterOS script atau commands langsung ke perangkat Layer API / SSH jaringan (Router). Socket.io / Streaming data trafik harian (Tx/Rx) dari router ke Real-Time Comms WebSockets dasbor frontend. |

## 5. Flow / Cara Kerja Sistem

```
[ Jam Sekolah: 07.00 - 15.00 ]
├── Zona Sekolah / Lab --> Prioritas Utama (80% Bandwidth, Latensi
Rendah)
└── Zona Luar / Umum --> Restricted (20% Bandwidth, Shaping Aktif)
[ Jam Selesai: > 15.00 ]
└── Semua Zona --> Distribusi Rata & Dynamic Throttling
```

- \* OVERRIDE MODE (Jika aktif via Master Switch): Mengabaikan otomatisasi waktu dan menetapkan 100% prioritas pada Zona Custom (Misal: Ujian ANBK).

## 6. Non-Functional Requirements

- Security: Semua komunikasi antara Backend dan Router menggunakan koneksi aman (SSH/API with TLS). Aplikasi mengimplementasikan JWT untuk autentikasi sesi pengguna.


- Performance: Dasbor WebSocket harus sanggup me-render update grafik trafik minimal 1 detik sekali tanpa menyebabkan memory leak pada browser.

- Reliability: Scheduler memiliki mekanisme retry (maks. 3 kali) apabila router mengalami timeout saat konfigurasi diubah.
