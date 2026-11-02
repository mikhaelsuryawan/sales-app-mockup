# SIM Group Sales App: konteks proyek untuk dilanjutkan

Dokumen ini merangkum seluruh pekerjaan sampai 2 November 2026 supaya dapat dilanjutkan oleh orang lain (atau Claude lain) tanpa membaca riwayat percakapan.

## 1. Proyek

Aplikasi mobile dan dashboard web untuk **SIMGROUP (PT Swakarya Insan Mandiri)** untuk sales lapangan. Tahap sekarang: **mockup HTML** (belum ada backend). Login pegawai nanti lewat API SIM (BPR1).

Dokumen sumber kanonis ada di repo `docs` (`../docs/00-sources`, daftar di `REGISTER.md`):

| Dokumen | Isi |
|---|---|
| `BRD_Sales_App_v0.6.pdf` | BRD retail (Juni 2023): absensi, target, penjualan (Promotor, Canvasing, Taking Order), penugasan, SKU display, market share, quiz, promo, insentif, grading. 62 halaman |
| `URD_Sales_Automation.pdf` | URD Sales Automation (April 2026): leads pinjaman, assign manager ke member, status Deal/Prospect/Rejected, goal, kirim ke SIM, overdue. 12 halaman |
| `MOM_Gabungan_27-28_Okt_2026.docx` dan `Notes_Meeting_27-28_Okt_2026.docx` | Catatan workshop dengan client |

Rangkuman workshop yang sudah dirapikan: `../docs/00-sources/inbox/hasil-workshop-27-28-okt-2026.md` (juga `.docx`), menunggu intake. Pertanyaan yang disiapkan sebelum workshop: `../docs/00-sources/inbox/pertanyaan-workshop-requirement.md`.

## 2. Isi folder

```
index.html        halaman pemilih: Mobile atau Dashboard
mobile/           mockup mobile (tampil sebagai iPhone di desktop, panel demo di samping)
dashboard/        dashboard web (WIT style v2, tema terang/gelap)
assets/           logo SIMGROUP (dipakai halaman pemilih)
docs/             dokumen lokal repo ini (CONTEXT.md)
vercel.json       konfigurasi deploy Vercel
AGENTS.md         instruksi untuk agent (CLAUDE.md memuatnya)
```

Menjalankan: `python3 -m http.server 5173`, lalu buka `http://localhost:5173/` (pemilih), `/mobile/`, dan `/dashboard/`. Deploy: Vercel dari repo GitHub `mikhaelsuryawan/sales-app-mockup`, satu tautan untuk keduanya.

## 3. Keputusan desain

- **Mobile:** biru cerah dengan gradient (#21CFF7 → #0A92F2 → #3466FF), aksen tan/peach (#EFA477) dari logo SIMGROUP, gaya **glassmorphic** (kartu transparan blur, latar mesh gradient). Font Plus Jakarta Sans. Di desktop tampil sebagai frame iPhone dengan panel demo.
- **Dashboard:** memakai skill `wit-code-agent-2` (WIT house style v2) dengan **merah WIT diganti biru mobile**. Sidebar putih menempel, breadcrumb, judul berakhiran titik berwarna aksen, kartu putih, tab pill, grafik kolom kapsul bergaris. Font DM Sans. Tema terang dan gelap.
- Fitur di luar BRD/URD diberi label **"Usulan"** di panel demo mobile, supaya client dapat membedakan.
- Warna merah hanya untuk status bahaya (ditolak, overdue, error).

## 4. Isi mockup (kondisi sebelum revisi workshop)

### Mobile (`mobile/index.html`)

5 user demo (password bebas): `spg.rina`, `sales.budi` (Canvasing), `sales.dimas` (Taking Order), `sales.andi` (Leads), `spv.sari` (Supervisor).

- **Login** (validasi ke API SIM), **foto wajah referensi wajib saat login pertama**, beranda terkunci sebelum absen masuk.
- **Absen:** lokasi dan jadwal (WFO dalam radius 100 m, WFA bebas), selfie face match, terlambat (alasan, approval), pulang cepat, pengajuan izin, rekap bulanan.
- **Kunjungan:** ad hoc, terencana (approval), berkala, check-in/out dengan geotag, foto dokumentasi.
- **Penjualan (SPG, Canvasing):** pilih toko → **foto invoice + OCR** (harga acuan = harga invoice, harga sistem sebagai pembanding, selisih lebih dari 10% ditandai) → data pelanggan → konfirmasi. Promo, backdate maksimal 2 hari, harga per kategori toko, riwayat berfilter, hapus dengan approval, mode offline, scan barcode.
- **Canvasing:** stok bawaan, retur layak/tidak layak, penyesuaian, usulan toko baru.
- **Taking Order:** pesanan, follow up, batal dengan approval, upload follow up dari file.
- **Leads (pinjaman):** daftar, status Deal/Prospect/Rejected, goal, kirim ke SIM, hasil kontak, pengingat.
- **Supervisor:** dashboard tim, approval, assign leads, data overdue.
- Lainnya: target, SKU display, market share, quiz, survey, notifikasi, akun. Estimasi insentif (SPG, Canvasing) ada di mockup tetapi **akan dihapus** menurut workshop.
- Panel demo: ganti user, WFO/WFA, dalam/luar radius, face match cocok/tidak, absen tepat/telat, pulang normal/cepat, online/offline, hasil OCR berhasil/buram.

### Dashboard (`dashboard/index.html`)

Overview, Absensi, Kunjungan, Approval, **Laporan penjualan** (per klien, cabang, toko, sales; transaksi dengan selisih invoice; drill-down), Target & sell-in, Promo, Insentif (perhitungan, rekonsiliasi, parameter), Grading, Leads, Overdue, **Master data dengan CRUD** (Client, Kategori toko, Toko + usulan dari lapangan, Produk & harga, User SIM, IAM dengan role, hak akses, matrix approval, log aktivitas), Parameter, **Quiz & Survey dengan CRUD** dan assign by role, Pengaturan.

Semua data dummy. CRUD bekerja di memori (hilang saat reload). Mobile dan dashboard belum berbagi data.

## Status revisi MOM 27-28 Okt 2026

Acuan revisi: `../docs/00-sources/inbox/MOM-SIM-Group-Sales-App-27-28-Oktober-2026.docx`.

- **Tahap 1 (ubah layar yang ada): selesai.** Bagian 4 di atas masih menggambarkan kondisi sebelum revisi. Yang berubah: estimasi insentif dan target per toko dihapus dari mobile; Rejected menjadi Belum Tertarik dengan tiga turunan; proses sendiri dihapus; Sales Member menjadi Multi Finance; jadwal kunjungan dibuat Team Leader; check-in dan check-out memakai 2-3 foto ber-watermark dengan auto check-out; SKU display dan market share harian; harga principal dan harga toko di OCR; foto pelanggan wajib; scan barcode pindah dari penjualan ke SKU display; menu Quiz dan Survey dipisah; harga per toko dan cabang; badge sidebar dihapus.
- **Tahap 2 (role, hak akses, master data, laporan SKU dan market share): belum.**
- **Tahap 3 (alur FMCG/MD, Canvassing, Multi Finance, Task Pending, POSM dan planogram, notifikasi absen): belum.** Task perbaikan SKU sudah tercatat di state mobile, tetapi belum tampil di layar mana pun.

## 5. Hasil workshop yang belum masuk ke mockup

Daftar perubahan lengkap ada di bagian 14 dokumen workshop. Ringkasnya:

- Tambah role: Team Leader, FFOR, Arco, **CBD**, **REA / SPG Multi Finance**, Promotor FMCG/MD, Mobile Promotor, Reliever, Trainer. Ganti nama Sales Member menjadi Multi Finance.
- **CBD memonitor mitra dan boleh menjual**; mitra melekat pada CBD; REA dan SPG Multi Finance boleh membuat leads sendiri.
- Jadwal kunjungan dibuat Team Leader (kecuali FMCG dan MD). Check-in/out dengan 2–3 foto ber-watermark.
- **Hapus estimasi insentif** dari mobile. Hapus "Proses sendiri" pada leads.
- Status leads: Rejected menjadi **Belum Tertarik** dengan 3 turunan. Leads Deal wajib foto approval dan dokumen pencairan (REA).
- Alur Promotor FMCG tanpa penjualan (cek stok multi satuan, price promo, facing display, support task, stock opname).
- SKU display diinput harian oleh promotor, SKU rusak otomatis membuat task perbaikan. Market share dengan draft.
- Pisahkan menu Quiz dan Survey; hapus opsi anonim; jadwal berkala.
- Master data baru: Region, Brand, level produk berjenjang, satuan, harga per toko dan cabang.

## 6. Hal yang masih terbuka

- Hierarki lengkap CBD, mitra, REA, SPG Multi Finance, Arco, Team Leader (catatan terpotong).
- Apakah mitra punya akun login atau hanya dipantau CBD.
- 12 perbedaan antar catatan di bagian 13 dokumen workshop (misalnya arti "takeout", CBD bergerak atau diam).
- Sumber data overdue dan sumber nominal goal pinjaman (URD tidak konsisten).
- Apakah laporan memakai harga invoice atau harga sistem.
- Pertanyaan P1 lain di `docs/pertanyaan-workshop-requirement.md`.

## 7. Catatan teknis

- Tiap mockup satu file HTML mandiri (CSS dan JS inline, logo base64). Tidak ada build step.
- Mobile: router tumpukan (`go`, `back`, `reset`), layar didefinisikan sebagai `SCR['nama'] = props => ({html, note, tab, dark})`. `S` adalah state, `USERS`, `PRODUCTS`, `TOKO` adalah data dummy. `renderPanel()` membuat panel demo.
- Dashboard: router hash (`#/penjualan?tab=klien`), `ROUTES`, `ENT` (definisi CRUD generik: `openForm`, `saveForm`, `delAsk`), `dataTable`, `openPanel`, `card`, chart SVG buatan sendiri (`columnChart`, `donut`, `lineChart`).
- Preview di browser pane Claude memakai snapshot file sehingga navigasi hash tidak jalan; gunakan server lokal.
- Skill WIT (`wit-code-agent-2`) berasal dari `wit-skills.zip` milik tim dan tidak disertakan di sini.
