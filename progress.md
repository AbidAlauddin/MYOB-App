# MYOB Smart-Guide — Project Progress

## 1. Current Status

- **Current Phase:** Fase 1 — Persiapan & Fondasi Sistem (Minggu 1)
- **Current Priority:** Inisialisasi proyek Laravel, autentikasi Breeze, dan migrasi database MySQL
- **Last Updated:** 2026-11-12
- **Overall Status:** In Progress (Perancangan Selesai, Memasuki Tahap Development)

> Dokumen ini melacak status implementasi aktual **MYOB Smart-Guide** berdasarkan jadwal R&D 7 minggu (ADDIE + ASSURE). Perbarui status hanya setelah fitur diimplementasikan dan diverifikasi.

---

## 2. Feature & R&D Milestone Progress

| Feature / Milestone | Target Minggu | Status | Completion Criteria | Notes |
|---|---|---|---|---|
| Spesifikasi Teknis & Desain Antarmuka | Minggu 1 | COMPLETED | Outline utama, storyboard, pohon keputusan, dan wireframe layout split-screen 35:65 selesai | Dokumen rancangan dasar & revisi desain modern minimalis disetujui |
| Fondasi Laravel & Autentikasi (`student` / `teacher`) | Minggu 1 | NOT STARTED | Proyek Laravel terpasang, Laravel Breeze aktif, role pengguna berjalan, dan koneksi MySQL terverifikasi | Prioritas utama saat ini |
| Skema Database & Seeder Master | Minggu 1 | NOT STARTED | Tabel `users`, `episodes`, `student_progress`, `interaction_logs`, `checkpoint_results` termigrasi | Menyertakan seeder contoh 1 modul lengkap |
| Dashboard Utama (Peta 6 Modul) | Minggu 1 | NOT STARTED | Menampilkan ringkasan progres, lencana modul, status kunci (`🔒`/`✓`), dan tombol akses cepat | Disesuaikan untuk layar 1366×768 |
| Vertical Slice (Modul 2 / Modul 3 Utuh) | Minggu 1 | NOT STARTED | Layout split-screen 35:65, video hotspot, simulasi hybrid 3 mode, Hint 1–3, Troubleshooting, dan Checkpoint berfungsi | Prototipe v0.1 untuk Review Expert Tahap 1 |
| Review Expert Tahap 1 (Kualitatif) | Minggu 2 | NOT STARTED | Telaah oleh 2 ahli materi, 2 ahli media, dan 2 guru praktisi selesai | Menghasilkan catatan masukan perbaikan |
| Revisi Tahap 1 & CRUD Ringan Guru | Minggu 3 | NOT STARTED | Perbaikan UI/materi v0.2 dan form edit teks soal, faktur, Hint 1–3, serta kunci Checkpoint | Menggunakan Resource Controller Laravel |
| Produksi Konten Penuh (6 Modul / 16–22 Episode) | Minggu 3–4 | NOT STARTED | Seluruh video screencast MP4, faktur kasus perusahaan fiktif, dan aturan JSON ter-seed di database | Mencakup Modul 1 s.d. Modul 6 |
| Dashboard Monitoring Guru & Ekspor CSV/Excel | Minggu 4 | NOT STARTED | Tabel monitoring progres kelas real-time, tombol manual unlock, dan ekspor data log ke CSV/XLSX | Menggunakan paket Laravel Excel |
| Validasi Expert Tahap 2 (Kuantitatif) | Minggu 4 | NOT STARTED | Skor kelayakan Likert (target ≥ 81%) dan Aiken's V (target ≥ 0,80) tercapai | Menggunakan 13 butir materi & 14 butir media |
| Revisi Tahap 2 & Deployment Prototipe v1.0 | Minggu 5 | NOT STARTED | Bug teknis diperbaiki, aplikasi online dengan SSL/HTTPS, dan uji beban jaringan lab selesai | Siap untuk uji coba lapangan |
| Uji Coba Terbatas & Analisis Data R&D | Minggu 6–7 | NOT STARTED | One-group pretest–posttest pada 15–20 siswa XI AKL, analisis N-Gain ($g \ge 0,30$), TAM, dan teacher-dependency | Penyusunan laporan akhir R&D |

---

## 3. Development Checklist (Teknis)

### Fase 1: Fondasi & Database (Minggu 1)
- [ ] Instalasi Laravel baru & konfigurasi `.env` MySQL
- [ ] Pemasangan Laravel Breeze & penambahan kolom `role` (`student`, `teacher`) serta `kelas` pada tabel `users`
- [ ] Pembuatan migration & model: `Episode`, `StudentProgress`, `InteractionLog`, `CheckpointResult`
- [ ] Pembuatan halaman Dashboard Utama (`/dashboard`) dengan grid Peta 6 Modul

### Fase 2: Prototipe Fungsional *Vertical Slice* (Minggu 1)
- [ ] Pembuatan layout *split-screen* 35:65 (`resources/views/praktikum/show.blade.php`)
- [ ] Implementasi Tab Panel Kiri (`Faktur` | `Langkah` | `Glosarium`) dan Laci `Hint 1-2-3` dengan Alpine.js
- [ ] Implementasi pemutar video HTML5 dengan *overlay hotspot* berbasis *timestamp*
- [ ] Implementasi simulasi MYOB *hybrid* (gambar antarmuka MYOB v18 + input transparan & validasi aturan)
- [ ] Pembuatan API endpoint (`/api/log-aktivitas`, `/api/update-progres`, `/api/cek-checkpoint`)
- [ ] Pembuatan pop-up pohon keputusan *Self-Troubleshooting* & modal *Self-Assessment Checkpoint*

### Fase 3 & 4: CRUD Guru, Konten Penuh & Monitoring (Minggu 3–4)
- [ ] Form CRUD ringan bagi guru untuk edit teks soal, upload gambar faktur, teks *Hint 1–3*, dan angka kunci *Checkpoint*
- [ ] Produksi & seeding lengkap 6 modul praktikum MYOB Accounting v18
- [ ] Halaman monitoring guru (`/guru/monitoring`) dengan fitur *manual unlock* dan ekspor CSV/XLSX

### Fase 5 & 6: Pengujian, Deployment & Uji Lapangan (Minggu 5–7)
- [ ] Pengujian otomatis PHPUnit untuk otorisasi role, validasi checkpoint, dan pencatatan log
- [ ] Pengujian responsivitas browser pada resolusi 1366×768 dan perangkat mobile
- [ ] *Deployment* ke server hosting dengan HTTPS dan uji akses simultan 36 siswa

---

## 4. Recent Changes

### 2026-11-12 — Finalisasi Rancangan Arsitektur Online & Desain Antarmuka
- Memutuskan migrasi arsitektur dari situs statis lokal menjadi aplikasi web dinamis **Laravel + MySQL** untuk menjamin persistensi data progres siswa di PC laboratorium, keamanan kunci modul, dan pemantauan *real-time* oleh guru.
- Menyusun rancangan skema database 5 tabel (`users`, `episodes`, `student_progress`, `interaction_logs`, `checkpoint_results`).
- Merancang revisi *wireframe* modern minimalis: tata letak *split-screen* asimetris **35% : 65%**, navigasi tab pada panel kiri, laci bawah *Hint 1–2–3*, dan *stepper* siklus belajar di bagian atas area kerja kanan.
- Menyusun roadmap pengembangan 6 fase yang diselaraskan dengan jadwal penelitian R&D 7 minggu.

---

## 5. Known Issues & Risks

- **Dataset Studi Kasus Perusahaan:** Naskah transaksi lengkap (profil perusahaan dagang fiktif, neraca saldo awal, dan kumpulan faktur Modul 1–6) baru tersedia dalam bentuk contoh ilustratif (Episode 2.2) dan perlu disusun utuh sebelum validasi ahli materi di Minggu 2.
- **Bandwidth Video di Laboratorium:** Pemutaran video MP4 oleh 30–36 siswa secara bersamaan berpotensi memperlambat jaringan jika ukuran file terlalu besar. Video wajib dikompres menggunakan *codec* H.264 resolusi 720p dengan *bitrate* efisien (800–1000 kbps).

---

## 6. Next Steps

1. Menginisialisasi proyek Laravel dan menjalankan migrasi tabel database MySQL (`episodes`, `student_progress`, `interaction_logs`, `checkpoint_results`).
2. Menyusun satu paket studi kasus perusahaan dagang lengkap beserta seeder untuk 1 modul *Vertical Slice* (Modul 3: Penjualan).
3. Membangun halaman Dashboard Utama dan antarmuka *split-screen* 35:65.