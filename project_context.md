# MYOB Smart-Guide — Project Context

## 1. Product Overview

**MYOB Smart-Guide** adalah aplikasi web e-modul interaktif berbasis simulasi dan video tutorial yang dirancang sebagai asisten praktikum mandiri komputer akuntansi (**MYOB Accounting v18**) untuk siswa kelas XI SMK jurusan Akuntansi dan Keuangan Lembaga (AKL).

Aplikasi ini dikembangkan untuk mengatasi tingginya ketergantungan siswa pada guru (*teacher-dependency*) dan antrean praktikum (*bottleneck*) di laboratorium komputer (30–36 siswa per kelas), sehingga guru dapat beralih peran dari instruktur navigasi menjadi fasilitator analisis logika akuntansi.

### Product Goals

- Menyediakan panduan praktikum mikro (*self-paced learning*) yang dapat dijeda, diulang, dan dikendalikan siswa.
- Mengurangi ketergantungan siswa kepada guru (*teacher-dependency*) melalui siklus belajar bertahap (*guidance fading*).
- Menyediakan simulasi antarmuka MYOB v18 berbasis web sebelum siswa berlatih pada aplikasi desktop sesungguhnya.
- Membantu siswa melacak dan memperbaiki kesalahan transaksi (*Trial Balance out of balance*) secara mandiri melalui pohon keputusan (*Self-Troubleshooting*).
- Memverifikasi ketepatan angka kunci laporan keuangan secara otomatis (*Self-Assessment Checkpoint*).
- Menyediakan pemantauan progres belajar *real-time* serta log interaksi digital bagi guru untuk evaluasi pembelajaran dan penelitian R&D.

### Target Users

- **Siswa (`student`):** Mengakses peta modul praktikum, mengikuti siklus belajar per episode (*Lihat → Coba Terpandu → Uji Mandiri*), memverifikasi angka kunci laporan keuangan, dan menggunakan panduan pemecahan masalah mandiri.
- **Guru (`teacher`):** Memantau progres belajar siswa secara *real-time*, membuka kunci progres modul jika diperlukan, mengelola teks petunjuk/soal kasus melalui CRUD ringan, dan mengekspor log aktivitas/skor ke CSV/Excel.

---

## 2. Product Scope and Pedagogical Decisions

### Four Core Interactive Features

1. **Interactive Screencast Video:** Video rekaman layar antarmuka MYOB per episode transaksi pendek (2–3 menit) dengan lapisan *hotspot* interaktif yang menjeda video hingga siswa mengklik area menu yang benar.
2. **Guided Simulation Module:** Simulasi antarmuka MYOB v18 tiruan berbasis HTML5/CSS/JS (*hybrid image-map & form overlay*) dengan tiga mode:
   - *Mode 1: Lihat* (demonstrasi video *hotspot*).
   - *Mode 2: Coba Terpandu* (latihan input dengan bantuan bertingkat *Hint 1: Lokasi*, *Hint 2: Konsep*, *Hint 3: Solusi Bertahap*).
   - *Mode 3: Uji Mandiri* (latihan tanpa *hint* sebelum praktik di aplikasi MYOB v18 asli).
3. **Self-Troubleshooting Guide:** Pohon keputusan (*decision tree*) interaktif berbentuk modal pop-up untuk melacak penyebab ketidakseimbangan neraca (*out of balance*) atau kesalahan input jurnal.
4. **Self-Assessment Checkpoint:** Lembar verifikasi otomatis angka kunci laporan keuangan yang memvalidasi hasil input siswa pada sisi *server*, dilengkapi umpan balik korektif jika tidak sesuai.

### Curriculum Structure (6 Modules)

Konten praktikum disusun secara prosedural dan hierarkis ke dalam 6 modul utama (estimasi 16–22 episode pendek sesuai ATP SMK):

1. **Modul 1:** Setup Data Awal (Data perusahaan, daftar akun, kode pajak, kartu pembantu, saldo awal).
2. **Modul 2:** Pembelian / *Purchase* (Pembelian tunai, pembelian kredit dengan PPN, pembayaran utang).
3. **Modul 3:** Penjualan / *Sales* (Penjualan tunai, penjualan kredit dengan PPN, penerimaan piutang).
4. **Modul 4:** Perbankan / *Banking* (Penerimaan kas/bank, pengeluaran kas/bank).
5. **Modul 5:** Memorial dan Penyesuaian (Jurnal umum, penyesuaian akhir periode).
6. **Modul 6:** Laporan Keuangan (Menyusun & memverifikasi laporan, penelusuran *out of balance*).

---

## 3. Technology and Architecture Decisions

### Established Stack

| Technology | Responsibility |
|---|---|
| Laravel (PHP 8.x) | Application framework, routing, authentication, and backend API |
| MySQL / MariaDB | Relational database for users, episodes, progress, and research logs |
| Laravel Blade | Server-rendered layout and templates |
| Laravel Breeze | Authentication scaffolding and session management |
| Tailwind CSS | Utility-first styling for responsive laboratory monitors |
| Alpine.js & Vanilla JS | Client-side split-screen interactivity, video hotspots, and simulation state |
| Laravel Excel | Exporting student progress and hint usage logs to CSV/XLSX |

### Architectural Constraints

- Gunakan **Laravel** sebagai kerangka kerja utama dengan arsitektur *client-server*.
- Pertahankan **Laravel Blade + Alpine.js** untuk antarmuka pengguna; jangan bermigrasi ke SPA terpisah (React/Vue/Inertia) tanpa keputusan eksplisit.
- Konten konfigurasi kompleks (koordinat *hotspot* video, aturan validasi form simulasi, dan struktur pohon keputusan) disimpan dalam kolom `JSON` pada tabel `episodes` melalui **Laravel Seeder**.
- Fitur CRUD di halaman guru dibatasi pada konten teks/angka yang sering direvisi saat validasi ahli (judul, deskripsi, gambar faktur, teks *Hint 1–3*, dan kunci angka *Checkpoint*).
- Video MP4 (H.264/AAC, resolusi 720p/1080p) disimpan sebagai file statis/CDN terpisah dari database untuk menjaga performa jaringan laboratorium saat 30–36 siswa mengakses secara bersamaan.

### Authentication, Authorization, and Data Privacy (UU PDP)

- Aplikasi menegakkan dua peran resmi (`users.role`): `student` (default) dan `teacher`.
- Kepatuhan perlindungan data pribadi siswa (UU PDP): sistem hanya mengumpulkan identitas minimal yang dibutuhkan untuk pembelajaran dan evaluasi kelas (nama/kode siswa, kelas, progres, dan log interaksi).
- Seluruh validasi pembukaan kunci episode (*unlocking*) dan pencocokan kunci angka *Self-Assessment Checkpoint* diproses di sisi *server* (Controller/Service Laravel) untuk mencegah manipulasi di *browser*.

---

## 4. Design System & UI Principles

### Visual Direction & Layout

- **Minimalis, Bersih, dan Bebas Gangguan (*Coherence Principle*):** Menghilangkan elemen dekoratif yang tidak relevan (animasi berlebihan, musik latar, gradien mencolok) untuk menekan beban kognitif ekstrinsik siswa.
- **Target Resolusi Utama:** Layar monitor PC laboratorium sekolah minimal **1366×768**, sekaligus tetap responsif pada perangkat lain.
- **Tata Letak Praktikum *Split-Screen* Asimetris (35% : 65%):**
  - **Panel Kiri (35%):** Navigasi tab ringkas (`Faktur` | `Langkah` | `Glosarium`) dan laci bawah (*docked drawer*) untuk tombol petunjuk bertingkat **Hint 1–2–3**.
  - **Panel Kanan (65%):** *Stepper* siklus belajar di bagian atas (`1. Lihat Video` → `2. Coba Terpandu` → `3. Uji Mandiri`), area kerja simulasi MYOB/pemutar video interaktif di tengah, dan *floating dock* kontrol bawah (`Self-Troubleshooting`, `Checkpoint Mandiri`, navigasi episode).
- **Kedekatan Spasial (*Spatial Contiguity*):** Dokumen bukti transaksi (faktur) dan instruksi ditampilkan berdampingan langsung dengan layar kerja MYOB tanpa mengharuskan siswa berpindah tab.
- **Aksesibilitas:** Menyediakan *subtitle* video, kontras warna memadai, dan navigasi papan ketik (*keyboard navigation*) mengacu pada pedoman WCAG 2.2.

### Color Palette

| Token | Hex | Intended Use |
|---|---|---|
| Primary Navy | `#1E3A5F` | Header bar, active step indicators, primary structural emphasis |
| Action Blue | `#2563EB` | Active mode buttons, primary interactive actions |
| Warning / Hotspot Orange | `#D97706` | Video interactive hotspots, Self-Troubleshooting trigger |
| Success Green | `#16A34A` | Completed badges, Self-Assessment Checkpoint trigger |
| Surface Background | `#F8FAFC` | Main canvas background |
| Text Primary | `#0F172A` | Primary readable text |

### Anti-AI-Slop Constraints

- Dilarang menggunakan gradien dekoratif, efek *glassmorphism*, atau bayangan (*shadow*) tebal berlebihan.
- Dilarang menampilkan angka progres, lencana, atau riwayat aktivitas palsu (*dummy*) sebagai data produksi; tampilkan *empty state* yang jujur bila siswa belum memulai episode.

---

## 5. Data Integrity & Research Logging

Aplikasi ini merupakan instrumen penelitian R&D (model ADDIE + ASSURE). Integritas pencatatan data sangat krusial:

- Setiap klik pada `Hint 1`, `Hint 2`, `Hint 3`, kesalahan klik menu, pembukaan *Self-Troubleshooting*, dan percobaan *Checkpoint* wajib dicatat secara akurat ke tabel `interaction_logs` dan `checkpoint_results` melalui *endpoint* API asynchronous.
- Penurunan jumlah penggunaan *hint* antar-episode dan waktu penyelesaian menjadi data kuantitatif pendamping untuk mengukur kemandirian belajar dan penurunan ketergantungan pada guru (*teacher-dependency index*).

---

## 6. Context Boundaries & Source of Truth

- `PROJECT_CONTEXT.md`: Konteks produk, prinsip pedagogis, arsitektur teknis, dan standar desain (dokumen ini).
- `PROGRESS.md`: Status aktual pengerjaan fitur, jadwal 7 minggu R&D, riwayat perubahan, dan prioritas berikutnya.
- `AGENTS.md`: Panduan wajib bagi AI coding agent selama pengembangan dan pengujian aplikasi.