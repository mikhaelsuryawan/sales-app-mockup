# Agent Instructions: mockup

<!-- BEGIN SHARED-CORE v1 -->
## Konteks
Sales App: SaaS multi-klien. Repo: docs (kanonis), backend (Go, PostgreSQL), dashboard (React, Vite), mobile (Flutter).
Workspace memakai repo bersaudara. Jika `../docs` tidak ada, berhenti dan minta pengguna menjalankan `docs/scripts/workspace-setup.sh`.

## Urutan baca (berhenti begitu cukup)
1. `docs/INDEX.md`
2. `docs/04-features/<modul>/context.md`
3. File yang ditunjuk context pack
4. Kode terkait

Jangan membaca PDF, docx, atau `00-sources/originals`. Jangan membaca seluruh BRD atau FSD: cari ID di `docs/01-product/requirements.md`, lalu buka baris `S3:L<n>` yang ditunjuk.

## Invariant (tidak boleh dilanggar tanpa ADR)
- Semua data, cache, draft, dan berkas terikat konteks klien. Query tanpa filter klien adalah bug.
- Backend yang memutuskan izin. Guard UI hanya kenyamanan.
- Absensi hanya online (FR-ATT-001, FR-ATT-002); tidak masuk antrean offline.
- Operasi yang mengubah data memakai ID operasi stabil untuk deduplikasi (BR-005, BR-017).
- Jangan mencampur Order, Penjualan, Deal, dan Absensi dalam satu model.
- Jangan menaruh secrets, data klien nyata, atau foto dan data biometrik wajah di repo, fixture, atau log.
- Jangan mengirim data nyata (nama klien, data pengguna, kredensial) ke layanan eksternal mana pun, termasuk kueri yang dikirim skill atau tool. Pakai data fiktif atau istilah generik.
- Kode hasil generate (kontrak, mock) tidak diedit tangan.
- Aturan bisnis berstatus TBD tidak diimplementasikan. Catat di `docs/00-sources/OPEN-QUESTIONS.md` dan tanyakan.

## Alur kerja
Spesifikasi (`04-features`), kontrak (`03-contracts`), plan, implementasi, verifikasi, MR. Perubahan perilaku tanpa perubahan spesifikasi ditolak saat review.
Tugas kecil (sekitar 10 baris atau kurang, tanpa perubahan perilaku atau kontrak) boleh langsung dikerjakan.

## Skill dan hemat token
Ikuti `docs/05-engineering/agent-kit/skill-routing.md` dan `token-rules.md`.
Ringkas: panggil skill sesuai tahap (maksimal 2 per tugas teknis), tidak untuk tugas trivial. Wajib `anti-ui-slop` untuk UI baru, serta `no-ai-slop` lalu `humanizer` untuk setiap prosa dokumen. Baca sedikit dan cari dulu. Satu slice per sesi; tulis handoff di `plan.md`.

## Selesai berarti
Tes, lint, dan build repo ini lulus (perintah di bagian Perintah), dokumen yang terdampak diperbarui atau MR docs ditautkan, dan laporan menyebut apa yang belum diverifikasi.

## Kolaborasi
Satu orang per plan, tertulis di `plan.md` dan issue. Jangan mengubah file di luar lingkup plan tanpa izin. Perubahan dokumen dan kontrak lewat MR ke repo docs. Branch: hanya `feature/<slug>` atau `fix/<slug>` (huruf kecil, tanda hubung; awali slug dengan kode modul bila ada, mis. `feature/att-validasi-radius`). Dokumen, chore, dan refactor memakai `feature/`; perbaikan bug dan koreksi memakai `fix/`.

## Bahasa
Dokumen, komentar kode, dan pesan commit berbahasa Indonesia. Awalan Conventional Commits (`feat`, `fix`, `docs`, dan seterusnya) tetap bahasa Inggris, deskripsinya Indonesia, mis. `feat(ATT): tambah validasi radius lokasi`. Nama identifier kode mengikuti konvensi bahasa dan pustaka (Inggris).
<!-- END SHARED-CORE -->

## Repo ini
Mockup HTML Sales App SIMGROUP untuk demo ke klien, bukan kode produk. Dua mockup mandiri dalam satu deploy Vercel: `mobile/index.html` (aplikasi sales lapangan) dan `dashboard/index.html` (dashboard web). `index.html` di akar hanya halaman pemilih. Tanpa build step, tanpa dependensi, semua data fiktif. Baca `docs/CONTEXT.md` untuk keputusan desain, isi mockup, dan hal yang masih terbuka. Requirement ada di `../docs` (mulai `../docs/INDEX.md`).

## Perintah
- Setup: tidak ada.
- Jalankan: `python3 -m http.server 5173`, buka `http://localhost:5173/`.
- Tes, lint, build: tidak ada. Verifikasi manual (lihat Selesai di bawah).
- Deploy: Vercel dari branch `main`. Jangan deploy atau publikasi tanpa konfirmasi pengguna.

## Peta
- `index.html`: pemilih Mobile atau Dashboard. `assets/`: logo.
- `mobile/index.html`: satu file, tampil sebagai frame iPhone di desktop dan layar penuh di HP.
- `dashboard/index.html`: satu file, tema terang dan gelap.
- Tautan antar mockup memakai jalur relatif (`../dashboard/`, `../mobile/`).

## Aturan khusus mockup
- Bahasa antarmuka dan dokumen: Bahasa Indonesia, kalimat aktif dan sederhana.
- Landasan requirement: BRD, URD, dan hasil workshop (yang terbaru menang). Fitur di luar dokumen boleh jika sederhana dan lazim, beri label "Usulan".
- Tiap mockup tetap satu file HTML mandiri.
- Mobile: biru bergradient (#21CFF7, #0A92F2, #3466FF), aksen tan #EFA477, glassmorphic, font Plus Jakarta Sans. Jangan kembali ke gaya datar.
- Dashboard: gaya WIT MES (skill `wit-mes-frontend`): kanvas abu dengan glow biru, rail ink mengambang (ikon saja di bawah 1280px), kartu putih tanpa border, kartu hero ink dan kartu aksen di Overview, kontrol pill, bar bawah ink di HP. Aksen tetap biru SIM. Merah hanya untuk status bahaya.
- Setiap kartu, baris, dan tile mengarah ke sesuatu. Setiap aksi ditutup toast bertense lampau.
- Hapus selalu soft delete dengan konfirmasi; data yang masih dipakai tidak boleh dihapus.
- Jangan memakai data nyata (nama klien, pengguna, kredensial, foto wajah). Pengguna demo hanya fiktif.
- Selesai berarti: semua layar tanpa error konsol dan tanpa overflow horizontal di lebar 375, 768, 1440, tema terang dan gelap (dashboard), serta halaman pemilih dan kedua tautan antar mockup berfungsi.
