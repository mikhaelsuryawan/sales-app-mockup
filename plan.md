# Plan: dashboard mengikuti skill wit-mes-frontend secara penuh

Mulai: 11 Nov 2026 · Lingkup: `dashboard/index.html` saja. Mockup mobile tetap memakai gayanya sendiri sesuai AGENTS.md.

## Latar

Putaran pertama (commit `7d5d202`) baru mengganti shell dan Overview. Audit lengkap atas semua isi skill (`SKILL.md`, 9 file `references/`, folder `examples/`, dan 133 file `assets/`) menghasilkan 659 butir. Butir itu tercatat di empat lampiran:

| Lampiran | Isi | Kode butir |
|---|---|---|
| [docs/wit-mes/audit-visual-grafik.md](docs/wit-mes/audit-visual-grafik.md) | design-system, theme.css, index.html, favicon, charts.md, komponen chart | V-01 s.d. V-89, C-01 s.d. C-54 |
| [docs/wit-mes/audit-komponen.md](docs/wit-mes/audit-komponen.md) | components.md dan seluruh `packages/ui` | K-01 s.d. K-227 |
| [docs/wit-mes/audit-layout-resep.md](docs/wit-mes/audit-layout-resep.md) | layouts.md, patterns.md, shell admin, contoh halaman admin, peta 55 rute | L-01 s.d. L-54, P-01 s.d. P-69, F-01 s.d. F-14 |
| [docs/wit-mes/audit-ux-struktur.md](docs/wit-mes/audit-ux-struktur.md) | ux.md, structure.md, adapting.md, konvensi, fixtures, state, konfigurasi repo, aset mobile | U-01 s.d. U-53, S-01 s.d. S-131, M-01 s.d. M-29 |

Lampiran ditulis dalam bahasa Inggris karena merupakan keluaran audit mentah. Setiap butir punya kolom penerapan (Terapkan, Adaptasi, Tidak berlaku) dan status saat audit (Sudah, Sebagian, Belum) dengan nomor baris.

## Aturan yang tidak berubah

- Aksen tetap biru SIM (`#0A92F2`, `#0A6FE0`, `#0857B8`), tan tetap warna kedua, merah hanya untuk status bahaya.
- Rute, data contoh, dan alur bisnis tetap. Resep MES dipakai hanya untuk layar yang sudah ada padanannya.
- Satu file HTML tanpa build. Bagian skill yang berupa tooling (monorepo, TypeScript, ESLint, CI, tes) tidak dipindahkan sebagai file; maksudnya saja yang diterapkan.
- Hapus tetap soft delete dengan konfirmasi; data yang dipakai tidak bisa dihapus.
- Bila skill dan AGENTS.md bertentangan, AGENTS.md menang. Daftar konflik ada di akhir lampiran UX dan struktur.

## Keputusan yang perlu dikonfirmasi

Dikonfirmasi pengguna pada 11 Nov 2026: semua rekomendasi dipakai.

| No | Pertanyaan | Rekomendasi | Butir |
|---|---|---|---|
| D1 | Skill melarang gradient. Kolom grafik terpilih memakai gradient biru dari mobile. | Ganti dengan biru solid di dashboard; gradient tetap milik mobile. | V-33, C-03 |
| D2 | Skill membatasi grafik ke ink, warna kedua, aksen, dan abu. Sekarang ada oranye, navy, teal, ungu. | Urutan baru: ink, tan, biru, warning, success, abu. Kategori lebih dari enam digabung ke "Lainnya". | V-31, C-09 |
| D3 | Skill hanya mengizinkan transisi warna, fade, slide, dan pulse. Sekarang ada hitung naik angka, batang tumbuh, dan stagger. | Pertahankan skeleton dan fade. Hapus hitung naik, batang tumbuh, lift saat hover. Pengecualian 12 Nov 2026: hitung naik angka dipasang kembali atas permintaan pemilik produk, hanya saat pindah halaman; batang tumbuh, stagger berlebih, dan lift tetap dihapus. | V-83, F-10 |
| D4 | Penjaga izin per role (`can()`) dengan pengalih role demo. | Terapkan dengan dua role demo (Admin SIM, Supervisor) dan label "Usulan". Penjaga UI hanya kenyamanan; backend tetap yang memutuskan. | U-07, S-14 |
| D5 | Menu "Buat" global di rail dan header. | Terapkan, berisi alur buat yang sudah ada saja. | L-09, L-21, S-37 |
| D6 | Simpan perubahan data dashboard (bukan hanya data bersama) ke localStorage. | Terapkan dengan envelope berversi dan banner bila penyimpanan gagal. | S-17, U-14 |

## Fase

Setiap fase selesai dengan pemeriksaan "Selesai" dari AGENTS.md: tanpa error konsol, tanpa overflow horizontal di 375, 768, dan 1440, tema terang dan gelap. Satu fase satu commit di branch `feature/dashboard-wit-mes-penuh`.

### Fase 0. Persiapan

- Buat branch, ambil screenshot acuan semua rute di tiga lebar dan dua tema.
- Kunci keputusan D1 s.d. D6.
- Selesai bila: screenshot acuan tersimpan di scratchpad dan keputusan tertulis di plan ini.

### Fase 1. Fondasi visual dan data

Visual:
- Font lengkap (DM Sans 300 s.d. 800 dengan italic, JetBrains Mono 400 dan 500), fallback font, `viewport-fit=cover`, `theme-color` untuk tema gelap, favicon SVG tile ink dengan titik biru, logo rail dengan titik aksen. V-01 s.d. V-10, M-01, S-128, S-53.
- Lapisan dasar: smoothing, tap highlight, `::selection`, warna border bawaan, kursor, utilitas safe area dan tanpa scrollbar. V-37 s.d. V-43.
- Token: `--silver`, `--chart-muted`, `--shadow-up`, nilai `--shadow-glow`, alias nama kit (surface, surface-2, border), hapus abu karangan (`--void`, `--control`, `--hover`, `--line-strong`, `--subtle`) dan rona tambahan sesuai D2. V-13 s.d. V-36, K-02.
- Skala tipografi: judul header, judul dialog 18/600, header tabel 12/600, angka hero 30, unit di samping angka, `tabular-nums` di semua angka, input 16px di ponsel. V-44 s.d. V-61, M-04.
- Bentuk, bayangan, jarak, ikon (16 di kontrol, 20 di navigasi), gerak sesuai D3, permukaan ink dan aksen. V-62 s.d. V-89.

Data:
- Kunci penyimpanan berawalan `salesapp.dashboard.*`, envelope berversi, fungsi simpan mengembalikan boolean, banner bahaya di atas `main` bila gagal. U-14, U-32, S-17, S-30, S-49, S-83, S-107, S-108, S-113.
- Reset data demo: konfirmasi, hapus semua kunci aplikasi, muat ulang. S-18, M-21, M-22.
- Jam aplikasi dari `NOW` tetap (26 Okt 2026), dipakai `nowStamp` dan rekaman baru. S-15, S-101, S-102.
- `newId()` dan `nextCode()` berurutan. S-09, S-104.
- Formatter terpusat (tanggal, rentang, rupiah), tidak ada `toLocaleString` lepas. S-16, S-75, S-103.
- Peta label dan tone per status, objek `paths` untuk tautan. S-08, S-47, S-72, S-79, M-19.

Selesai bila: semua butir V berstatus Sudah atau Tidak berlaku, dan semua tampilan memakai token baru tanpa abu karangan.

### Fase 2. Kit komponen

Utilitas: focus ring dan focus field, press 0.98, ukuran ikon dari induk, overlay dengan focus trap, kunci scroll body, roving tabindex, `isPhone()`, reset halaman saat filter berubah. K-03 s.d. K-12.

Primitif:
- Tombol: varian primary, secondary, outline, ghost, link, danger, card, soft, onInk; ukuran sm, md, lg, icon; `loading`; tautan bergaya tombol. K-16 s.d. K-33.
- Kartu dan header kartu dengan deskripsi dan hitungan. K-35 s.d. K-46.
- IconTile, StatCard dengan tone mengikuti angka dan slot anak. K-47 s.d. K-56.
- Badge, CountBadge, StatusDot (pulse untuk status hidup), peta status ke tone. K-57 s.d. K-73.
- Switch, SegmentedControl, PillTabs, UnderlineTabs dengan tombol panah, Chip dan ChipRow, Steps horizontal dan vertikal, ProgressBar, SegmentBar, Avatar dan AvatarStack. K-109 s.d. K-137.

Form dan overlay:
- Input, Textarea, NativeSelect (enum enam atau kurang), FormField dengan hint, error, `aria-describedby`, tanda wajib biru. K-75 s.d. K-90.
- Combobox: pencocokan per kata, opsi nonaktif dengan alasan, baris hapus pilihan, multi, panel jadi bottom sheet di ponsel. K-91 s.d. K-108.
- DataTable: header sortable, tombol buka di sel pertama, pager, EmptyState, kolom sembunyi bertahap, kolom meter dan tren, aksi baris ikon. K-138 s.d. K-152.
- KeyValue, EmptyState, Banner empat tone, catatan blocker, SplitStats, PageHeader, Kicker. K-153 s.d. K-167.
- Dialog, ConfirmDialog (`role=alertdialog`, tombol nonaktif sampai isian wajib terisi), Sheet kanan dan bawah, ActionMenu (dropdown di desktop, sheet di ponsel), Popover, Tooltip, Toast dengan tone default dan tombol tutup, PhotoInput. K-168 s.d. K-203.

Baris komposit: baris daftar, baris dengan aksi, tile urutan, baris peringkat, baris riwayat, daftar cek, chip tautan, timeline, pohon, rantai alur, metrik di ink, tile bacaan, daftar pengaturan, footer total. K-213 s.d. K-227.

Aksesibilitas kit: label ikon dengan hitungan, `aria-current="step"`, `role=img` berlabel, kontras AA. U-48 s.d. U-52.

Selesai bila: semua helper lama (`btn`, `tag`, `spill`, `card`, `pills`, `utabs`, `sel`, `swc`, `toast`, `callout`, `dataTable`, `openModal`, `openPanel`) memakai kit baru dan tidak ada markup lama tersisa.

### Fase 3. Grafik

- Ukur lebar kontainer, gambar di piksel nyata. C-01, C-02.
- Warna: satu nilai disorot, sisanya ink atau abu, seri kedua tan, target sebagai garis acuan. C-03 s.d. C-12.
- Sumbu: `niceScale` dengan min dan max dari data, domain tetap untuk persen, grid solid, label kiri dengan lebar dari label terpanjang, penipisan label X. C-13 s.d. C-19.
- Garis acuan, zona, kolom 4px membulat ujungnya dan maksimal 24px, garis lurus 2px dengan clip, jeda untuk null, titik terakhir berlabel. C-20 s.d. C-25.
- Donut 148px cincin 16px, pusat nilai, cincin kosong, legenda oleh pemanggil. C-26 s.d. C-29.
- Tooltip HTML, crosshair, area sentuh, roving dengan Home dan End, hover, live region, focus ring, keadaan kosong. C-30 s.d. C-38, U-35.
- Kartu grafik, legenda, grafik di StatCard dan di kartu ink, drill-down dengan chip filter, SegmentBar. C-39 s.d. C-46.
- Tata letak analitik. C-47 s.d. C-54.
- Komponen baru: Sparkline, BarStrip, BarList, SegmentBar, ReferenceLine. Gantt absensi, meter skor, dan funnel leads tetap ada tetapi diberi warna dan gaya kit.

Selesai bila: semua grafik di `/`, `/penjualan`, `/leads`, `/sell-in`, `/target`, `/insentif`, `/grading`, `/sku-display`, `/market-share`, `/kepatuhan-kunjungan`, `/aktivitas` memakai helper baru.

### Fase 4. Shell dan navigasi

- Rail: nama produk dan tagline, tombol "Buat" (D5), satu baris per seksi NAV yang membuka sub-halaman di tempat, total hitungan di seksi tertutup, item 44px, tooltip bergaya, Keluar pindah ke menu workspace. L-07 s.d. L-14, K-205 s.d. K-209, S-33, S-40.
- SectionTabs untuk seksi berisi dua halaman atau lebih di bawah 1280px. L-15.
- Header: baris tanggal di bawah judul, judul dengan lebar maksimum, pencarian penuh lebar di ponsel, tombol Buat. L-16 s.d. L-19, S-39.
- Pencarian global: hasil langsung per grup (Sales, Toko, Leads, Produk), lima per grup, setiap kata harus cocok, panah, Enter, Esc. L-20, S-36.
- Menu Buat. L-21, S-37.
- Notifikasi: diturunkan dari data, status baca disimpan per pengguna dan workspace, panel 400px, header dengan "Tandai semua dibaca", baris bertone dengan waktu, keadaan kosong, bottom sheet di ponsel. L-22 s.d. L-28, S-24, S-35.
- Bar bawah dari NAV, aktif dengan batas `/`, More aktif saat sheet terbuka. L-32 s.d. L-35, M-09, S-38.
- Sheet More: pencarian halaman, Terakhir dibuka, seksi bisa dilipat, grid ikon tiga kolom, footer akun. L-36 s.d. L-42, U-37.
- Semua menu ponsel jadi bottom sheet terang dengan pegangan. L-43, U-38, M-14.
- Panel detail masuk riwayat (`?id=`), Back menutup panel, scroll `main` dipulihkan saat Back, halaman tabel tersimpan per entri riwayat. L-04, L-29, L-30, U-29, U-30, S-22, S-82, S-114, M-10, M-11.
- Footer sheet dengan safe area. M-15.
- Tabel responsif dan aturan keras tata letak. L-44 s.d. L-53, S-90.

Selesai bila: semua butir L berstatus Sudah atau Tidak berlaku.

### Fase 5. Aturan perilaku

- Satu aksi utama per wilayah; aksi lain outline atau masuk ActionMenu; menu hanya memuat aksi yang boleh. U-01, U-05, U-06.
- Penjaga izin per role (D4): peta izin, `can()`, sesi demo, penjaga rute ke `/login`, sembunyikan yang tidak boleh. U-07, S-14, S-42, S-57, S-77, S-106, S-112.
- Fungsi blocker per entitas yang dipakai tombol (nonaktif dengan alasan) dan aksinya. U-08, S-12, S-13, M-24.
- Satu fungsi `act(tipe, data)` yang mengubah data, mencap pelaku dan waktu, menulis audit, menyimpan, dan me-render. Nama tipe mengikuti kontrak di `docs/03-contracts`. S-02 s.d. S-06, S-10, S-11, S-31, S-73, S-74, S-109.
- Workspace klien benar-benar menyaring data. S-19, S-44, S-76, S-110.
- Toast: tense lampau, menyebut rekaman, tone default untuk hapus dan batal, tone bahaya untuk tolak. U-09 s.d. U-12, S-94, M-25.
- Banner aturan yang berubah tone saat mengetik. U-13.
- Form: error setelah submit pertama lalu langsung diperbarui, kalimat utuh, tanda wajib biru, unit di slot kanan, pilihan bergantung dengan "Pilih X dulu", field terkunci dijelaskan, tombol submit menyebut kata kerja. U-15 s.d. U-21.
- Picker dan filter: opsi nonaktif dengan alasan, klik chip aktif menghapus filter, stat tile sebagai filter. U-22 s.d. U-24.
- Konfirmasi: judul pertanyaan tanpa "?.", konsekuensi satu kalimat, tombol nonaktif sampai alasan diisi, approve tanpa catatan. U-25 s.d. U-27.
- Teks: tanpa em dash dan tanda seru, pemisah " · ", rentang "→", sel kosong en dash abu, label dari peta. U-44 s.d. U-47, S-58, S-100.

Selesai bila: setiap tombol mutasi melewati `act()` dan setiap tombol nonaktif menampilkan alasannya.

### Fase 6. Resep halaman: Overview dan halaman daftar

- Overview: caption konteks, StatCard ink dan info, tinjauan status, grid daftar dan analisis `1fr 1fr 20rem` dengan CountBadge dan "Lihat semua". P-01 s.d. P-06.
- Halaman daftar (`/absensi`, `/kunjungan`, `/leads`, `/overdue`, `/pemesanan`, `/data-penjualan`, `/penagihan`, `/promo`, `/posm`, `/canvassing`, `/pengumuman`, `/penugasan`, `/quiz`, `/survey`, `/toko`): baris PageHeader dengan deskripsi satu kalimat, pencarian, dan aksi utama; caption "Diperbarui" di papan langsung; tampilan dengan hitungan; banner cakupan saat tautan menyempitkan daftar; stat tile sebagai filter dengan tone; ChipRow; tabel di kanvas; keadaan disimpan di URL dan riwayat; "Muat lagi" di ponsel. P-09 s.d. P-16, U-33, U-34, S-86, S-95, S-97, M-23.

Selesai bila: semua rute daftar di peta rute (lampiran layout) mengikuti resep List page.

### Fase 7. Resep halaman lain

- Panel detail: baris aksi, catatan blocker, header ink dengan metrik, Steps, KeyValue dengan teks kosong yang benar, baris terkait, riwayat per rekaman, halaman tidak ditemukan. P-18 s.d. P-26, U-04, U-36, S-87.
- Master data (19 rute): domain lewat SectionTabs, EntityTable dengan aksi ikon, tombol Hapus nonaktif dengan alasan bila dipakai, EntityDialog dengan validasi kode unik. P-27 s.d. P-29.
- Antrean approval (`/approval`, `/toko?tab=usulan`, `/klaim-biaya`, `/task-pending`, `/insentif?tab=rekon`): kartu per jenis, tombol kata kerja per baris, ConfirmDialog bersama. P-36 s.d. P-38, P-41.
- Pemilih rekaman (`/live-tracking`, `/mapping-outlet`). P-49 s.d. P-51, P-53.
- Analitik dan baris insight. P-54, P-55, P-58.
- Integrasi (`/sinkron`, `/user-sales`). P-61 s.d. P-63.
- Login: kanvas dengan glow, kartu brand ink, kartu form dengan tombol besar, akun demo. P-65 s.d. P-68, V-47.

Selesai bila: setiap rute di peta rute memakai resep yang ditetapkan.

### Fase 8. QA dan dokumentasi

- Finish checklist skill. F-01 s.d. F-14.
- Pemeriksaan AGENTS.md: 55 rute di 375, 768, 1440, terang dan gelap, tanpa error konsol, tanpa `console.log`, halaman pemilih dan kedua tautan antar mockup berfungsi. L-54, U-53, S-64, S-119.
- Skrip perjalanan demo manual di `docs/CONTEXT.md` (contoh: approval, toast, daftar). S-130.
- Perbarui status semua butir di keempat lampiran dan catatan desain di AGENTS.md dan CONTEXT.md. S-67, S-71, S-131.

Selesai bila: tidak ada butir berstatus Sebagian atau Belum yang berlaku.

## Matriks cakupan

Semua butir lampiran masuk ke satu fase atau tercatat tidak berlaku.

| Kode | Fase | Tidak berlaku (alasan singkat) |
|---|---|---|
| V-01 s.d. V-89 | 1; V-47 di 7 | V-11 (tanpa build Tailwind), V-46 dan V-77 (khusus aplikasi operator) |
| C-01 s.d. C-54 | 3 | C-11 (alokasi stok MES tidak ada di Sales App) |
| K-01 s.d. K-227 | K-02 di 1; K-205 s.d. K-212 di 4; sisanya 2 | K-01 dan K-15 (tanpa Tailwind dan barrel), K-13 (diganti ResizeObserver di fase 3), K-14 (diganti "Muat lagi" di fase 6), K-34 (ukuran operator), K-69 (dokumen berrevisi), K-74 (aplikasi operator), K-120 (tab operator), K-204 (tidak ada alur tanda tangan) |
| L-01 s.d. L-54 | 4; L-54 di 8 | L-26 (satu peran demo tanpa penugasan per pengguna) |
| P-01 s.d. P-69 | P-01 s.d. P-16 di 6; P-18 s.d. P-68 di 7 | P-07, P-08, P-17, P-30 s.d. P-35, P-39, P-40, P-42 s.d. P-48, P-52, P-56, P-57, P-59, P-60, P-64, P-69 (tidak ada layar padanan di dashboard Sales) |
| F-01 s.d. F-14 | 8 | F-11 (aturan aplikasi operator) |
| U-01 s.d. U-53 | U-14, U-32 di 1; U-48 s.d. U-52 di 2; U-35 di 3; U-28 s.d. U-31, U-37, U-38 di 4; U-33, U-34 di 6; U-04, U-36 di 7; U-53 di 8; sisanya 5 | U-03 (satu peran tanpa beranda per peran), U-40 s.d. U-43 (aplikasi operator) |
| S-01 s.d. S-131 | 1: S-08, S-09, S-15 s.d. S-18, S-30, S-47, S-49, S-53, S-72, S-75, S-79, S-83, S-101 s.d. S-104, S-107, S-108, S-113, S-128. 2: S-94. 4: S-22, S-24, S-33 s.d. S-40, S-82, S-90, S-114. 5: S-02 s.d. S-06, S-10 s.d. S-14, S-19, S-31, S-42, S-44, S-46, S-57, S-58, S-73, S-74, S-76 s.d. S-78, S-81, S-100, S-106, S-109, S-110, S-112. 6: S-86, S-95, S-97. 7: S-87, S-98. 8: S-64, S-67, S-71, S-119, S-130, S-131. Butir lain yang sudah Sudah diperiksa ulang di 8 | S-01, S-07, S-20, S-27 s.d. S-29, S-32, S-45, S-48, S-63, S-65, S-66, S-99, S-105, S-111, S-116 s.d. S-118, S-120 s.d. S-127, S-129 (tooling monorepo, TypeScript, CI, tes, atau aplikasi operator; dilarang AGENTS.md atau tidak relevan) |
| M-01 s.d. M-29 | 1: M-01, M-04, M-19, M-21, M-22. 4: M-09 s.d. M-11, M-14, M-15. 5: M-24, M-25. 6: M-23. Sudah: M-17, M-18, M-29 | M-02, M-03, M-05 s.d. M-08, M-12, M-13, M-16, M-20, M-26 s.d. M-28 (PWA operator; mockup mobile punya gaya sendiri) |

## Handoff

- Fase 0 selesai 11 Nov 2026. Acuan: 53 rute tanpa error konsol dan tanpa overflow di 1024, terang dan gelap.
- Fase 1 selesai 11 Nov 2026 (commit di branch `feature/dashboard-wit-mes-penuh`). Yang masuk:
  - Meta, font lengkap, favicon SVG, logo rail dengan titik aksen, `theme-color` ikut tema.
  - Token tangga abu kit dengan alias nama lama, `--silver`, `--chart-muted`, `--shadow-up`, `--shadow-glow` baru, slot grafik dan warna status tanpa rona tambahan (D2), kolom terpilih biru solid (D1).
  - Lapisan dasar, utilitas `safe-b`, `safe-t`, `no-scrollbar`, input 16px di ponsel.
  - Gerak dibatasi (D3): fade 200 ms saja, tekan 0.98, kelas `pulse`.
  - Kunci `salesapp.dashboard.*` dengan migrasi dari `sim-*`, snapshot data dashboard berversi (D6), banner bila penyimpanan gagal, reset demo dengan konfirmasi dan muat ulang, jam aplikasi 26 Okt 09.41 WIB, `newId()` berurutan, formatter `num()`.
- Dipindah dari fase 1:
  - Ke fase 2: ukuran ikon 16 dan 20 (V-78), IconTile per status dan palet avatar (V-79, V-30 sisa `--pop-*`), catatan di kartu ink (V-87), unit rata atas di StatCard (V-59).
  - Ke fase 4: judul header 18px (V-45), dipasangkan dengan PageHeader di fase 6.
  - Ke fase 5: peta label dan tone per status serta objek `paths` (S-08, S-47, S-72, S-79, M-19), karena harus diterapkan bersama perombakan aksi.
  - `nextCode()` (bagian S-104) baru dibuat bila ada alur yang membuat kode manusiawi.
- Fase 2 selesai 11 Nov 2026. Kit di `dashboard/index.html`:
  - CSS: tombol sembilan varian dan empat ukuran, kartu (default, ink, aksen, soft), tile ikon, StatCard (ikon di kanan, tone), badge, hitungan, avatar dan tumpukan, input (default, soft, pill, invalid), combobox, textarea, switch, segmented, chip, tab pill dan underline dengan pudar kanan, stepper, progress, segment bar, banner empat tone, keadaan kosong, tabel, dialog, sheet, menu, tooltip, toast, baris komposit.
  - JS: helper `btn`, `ibtn`, `tag`, `spill`, `badge`, `countBadge`, `statusDot`, `iconTile`, `statCard`, `pills`, `utabs`, `chip`, `segmented`, `choiceGrid`, `stepper`, `swc`, `progressBar`, `segmentBar`, `card`, `banner`, `callout`, `blocker`, `emptyState`, `keyValue`, `splitStats`, `pageHead`, `formField`, `listRow`, `rankedRow`, `historyRow`, `checkList`, `entityChip`, `codeLink`, `personChip`, `flowChain`, `settingsList`, `totalFooter`, `metricOnInk`, `readingTile`, `avatarStack`, `actionMenu`, `confirmAsk`, `toast` (tone default), peta `STATUS_TONE`.
  - Perilaku: fokus terkunci di dialog, latar inert, scroll `main` terkunci, panah di tablist dan radiogroup, menu dengan panah dan Esc, tooltip 200 ms, combobox per kata dengan sheet di ponsel dan `aria-activedescendant`, tabel dengan sort, tombol buka, reset halaman saat URL berubah, dan keadaan kosong. Panel dan semua menu berbentuk bottom sheet di ponsel.
  - Dipindah: pemakaian helper baru di tiap halaman (stepper di panel pesanan dan leads, banner blocker, `rowActs` dengan `block`, `statCard`, `emptyState`) ke fase 6 dan 7. Rail dan bar bawah (K-205 s.d. K-212) ke fase 4.
- Fase 3 selesai 11 Nov 2026. Grafik di `dashboard/index.html`:
  - Mesin baru: `niceTicks` (kelipatan 1, 2, 2.5, 5, 10, min dan max dari data), grafik garis digambar pada lebar piksel nyata (ResizeObserver), garis lurus 2px dengan jeda untuk data kosong, titik dan label nilai terakhir, garis acuan, zona, domain tetap, penipisan label X, grid solid, lebar label sumbu dari label terpanjang.
  - Interaksi: tooltip HTML yang tetap di dalam plot, crosshair, panah dan Home/End, fokus mulai di titik terakhir, echo live region, keadaan kosong "Belum ada data".
  - Kolom: satu kolom disorot aksen, sisanya abu, ujung data 4px, lebar maksimal 24px, nilai dicetak di atas kolom, target sebagai garis acuan, tanpa lantai 6%.
  - Donut: 148px, cincin 16px, jarak 2px, segmen terangkat 3px saat disorot, cincin kosong, legenda dengan persen dan jumlah.
  - Komponen baru: `sparkline` (putih di kartu ink), `barStrip` (di stat Goal pinjaman), `barList` (rincian Penjualan), `segmentBar`.
  - Grafik "Pengajuan vs deal" kini berskala data (sebelumnya datar karena skala minimal 1.000).
  - Dipindah ke fase 6 dan 7: tata letak analitik `1.4fr/1fr` dan `1fr/20rem` serta chip drill-down di halaman analitik lain (`/sell-in`, `/target`, `/insentif`, `/grading`, `/sku-display`, `/market-share`, `/kepatuhan-kunjungan`, `/aktivitas`).
- Fase 4 selesai 11 Nov 2026. Shell dan navigasi:
  - NAV satu sumber (13 seksi, termasuk Sistem) untuk rail, tab seksi, bar bawah, sheet More, dan judul header.
  - Rail: nama produk dan tagline, tombol Buat, satu baris per seksi yang membuka sub-halaman di tempat, hitungan total di seksi tertutup, tooltip bergaya, workspace dengan menu (Keluar ada di sini), ciutkan hanya di 1280px ke atas. Pengaturan dan tema pindah ke menu pengguna.
  - Header: judul 18px dengan keterangan klien dan tanggal (disembunyikan di ponsel), pencarian pil dari 768px, ikon cari di ponsel membuka baris penuh, tombol Buat dari 640px, lonceng dengan hitungan belum dibaca, menu pengguna.
  - Pencarian global langsung per grup (Sales, Toko, Leads, Produk), lima hasil per grup, semua kata harus cocok, panah, Enter, Esc.
  - Menu Buat berisi enam alur buat yang sudah ada. Notifikasi diturunkan dari data, status baca disimpan per pengguna dan workspace, panel 400px, "Tandai semua dibaca", keadaan kosong.
  - Tab seksi muncul di bawah 1280px atau saat rail diciutkan. Bar bawah: Overview, Approval, Buat (bulat), Absensi, Leads, More; More aktif saat sheet terbuka. Sheet More: pencarian, Terakhir dibuka, seksi yang bisa dilipat, grid tiga kolom, footer akun. Menu, bel, dan pencarian menjadi bottom sheet di ponsel.
  - Riwayat: tombol Back menutup dialog, scroll `main` dipulihkan saat Back (Navigation API), halaman tabel kembali ke 1 saat pindah rute. Tabel di ponsel menampilkan empat kolom pertama, sisanya di panel rincian.
  - Tidak dikerjakan: panel di URL (`?id=`) dan `?new=1`. Panel dibuka oleh puluhan fungsi dengan argumen berbeda, jadi deep link-nya perlu registri per panel. Dicatat sebagai sisa di fase 8.
- Fase 5 selesai 11 Nov 2026. Aturan perilaku:
  - Sesi demo dengan dua akun (Admin SIM, Supervisor berlabel Usulan), penjaga masuk (`#/approval` tanpa sesi dialihkan ke login lalu kembali), menu "Ganti akun demo", `can(perm)` yang menyembunyikan tombol dan item menu Buat. Penjaga di layar hanya kenyamanan, dicatat di UI.
  - `act(tipe,{perm,guard,run,audit,toast})` menjadi satu pintu: guard yang sama dengan UI, perubahan, audit bercap pelaku dan jam aplikasi, simpan, render, toast. Dipakai oleh approve, tolak, status entitas, hapus, simpan form, status pesanan, batal pesanan, assign leads, dan keputusan klaim. Tipe aksi berbentuk `entitas/kata-kerja`; folder `docs/03-contracts` belum berisi kontrak, jadi belum ada nama yang harus diikuti.
  - Tombol Hapus master data diblokir dengan alasan (tooltip dan modal), aksi menolak ulang bila data masih dipakai. Master data tampil baca saja untuk non-admin.
  - Konfirmasi berupa pertanyaan dengan konsekuensi satu kalimat; Tolak menahan tombol sampai alasan diisi. Toast: default untuk hapus, batal, nonaktif, tolak; sakelar notifikasi kini memberi toast.
  - Form: galat berupa kalimat utuh, tampil setelah submit pertama lalu diperbarui saat mengetik, `aria-invalid` dan `aria-describedby`, tombol submit bernama kata kerja, tanda wajib biru. Input penjualan menampilkan banner yang naik dari info ke peringatan saat harga melewati toleransi.
  - Salinan: tanpa em dash.
  - Tidak dikerjakan: filter workspace pada data (butuh field klien di tiap koleksi transaksi), objek `paths` (jalur sudah satu sumber di NAV), alasan pada opsi picker yang tidak bisa dipilih (dukungan `data-reason` sudah ada, belum ada opsi yang memakainya), chip filter yang menghapus diri saat diklik ulang, satuan di slot kanan, picker bergantung "Pilih X dulu". Dicatat sebagai sisa di fase 8.
- Fase 6 selesai 11 Nov 2026. Overview dan halaman daftar:
  - Overview: baris daftar dan analisis `1fr 1fr 20rem` (Top sales dan Kehadiran dengan hitungan dan "Lihat semua", Ringkasan otomatis, Leads pinjaman sebagai kartu analisis), stat Hadir bernada ink dan Goal pinjaman bernada info, sparkline putih di kartu ink dan bar strip di stat goal.
  - Semua rute (kecuali Overview) memakai kalimat tujuan satu baris di bawah header; papan langsung (`/absensi`, `/kunjungan`, `/live-tracking`, `/sinkron`) menambah "Diperbarui 09.41 WIB" yang tersembunyi di ponsel.
  - Banner "Daftar dipersempit" dengan "Tampilkan semua" saat tautan dari halaman lain menyempitkan daftar (`?f=`, `?q=`, `?u=`).
  - Klik kedua pada chip filter yang aktif menghapus filternya. Stat tile bernada mengikuti hitungan (Leads belum di-assign, Klaim menunggu).
  - Tabel utama halaman daftar duduk di kanvas tanpa kartu; di ponsel tabel menampilkan enam baris dengan tombol "N lagi · Muat" tanpa pager; keadaan kosong punya judul dan deskripsi.
  - Tidak dikerjakan: baris status overview enam kartu (opsional, tidak ada padanan yang tidak menggandakan Absensi), pilihan tampilan yang diingat antar sesi (tiap halaman punya fungsi tab sendiri). Dicatat di fase 8.
- Fase 7 selesai 11 Nov 2026. Resep halaman lain:
  - Login: kanvas dengan glow, kartu brand ink dengan tiga pilar, kartu form dengan input 48px, tombol besar berpanah, dua akun demo, kembali ke halaman tujuan setelah masuk. Bug `logout()` yang memanggil dirinya sendiri sejak fase 5 diperbaiki.
  - Panel rincian pesanan, leads, dan klaim: hero ink dengan kode, nama, status, dan tiga metrik; stepper tahapan pada pesanan; catatan blocker bertebal di awal; riwayat per rekaman dari log audit; panel "tidak ditemukan" bila rekaman hilang; halaman tidak ditemukan memakai keadaan kosong.
  - Approval: setujui dan tolak memakai konfirmasi yang sama (tolak menahan tombol sampai alasan diisi). Layout tetap tabel dengan chip jenis, bukan kartu per jenis, karena pilihan massal lintas jenis dipakai.
  - Analitik: grafik dan rincian Penjualan memakai `1.4fr/1fr`, kolom samping 20rem. Pemilih rekaman (`/live-tracking`): daftar di kiri 20rem dan peta di kanan, daftar lebih dulu di ponsel.
  - Integrasi: kartu koneksi SIM (BPR1) di `/sinkron` dengan status, sinkron otomatis, "Sinkron sekarang", dan catatan kepemilikan data.
  - Master data sudah dicakup fase 2 sampai 5 (tabel, aksi ikon, hapus diblokir, dialog dengan validasi, domain lewat tab seksi).
  - Tidak dikerjakan: baris insight dengan lencana faktor (tidak ada fitur rekomendasi, jangan dibuat), hasil pencarian sebagai chip di `/mapping-outlet`, panel hero untuk transaksi, absensi, kunjungan, promo, dan penagihan (kini masih `kv`). Dicatat di fase 8.
- Fase 8 selesai 11 Nov 2026. QA, verifikasi independen, dan perbaikan penutup:
  - Empat agen memeriksa ulang setiap butir terhadap kode (laporan di `docs/wit-mes/verifikasi-*.md`). Hasil sebelum perbaikan penutup: visual dan grafik 110 Sudah, 23 Sebagian, 5 Belum, 5 N/A; komponen 116, 91, 11, 9; layout dan resep 69, 39, 2, 27; UX dan struktur 95, 63, 4, 51.
  - Temuan penting dan perbaikannya: stepper pesanan dibatalkan terlempar keluar layar karena kelas `skip` bentrok dengan tautan lewati (diganti `sk`); klaim "semua mutasi lewat `act()`" tidak benar, kini enam aksi hapus dan semua aksi persetujuan (usulan toko, klaim, parameter) lewat `act()`, sisanya dilindungi pembungkus izin; helper mati dihapus; Profil mengikuti akun masuk; workspace kini menyaring master data dan penjualan; parameter bisa diubah sungguhan; tombol palsu di panel promo dihapus.
  - Ditambahkan di putaran penutup: hero ink di panel promo, tagihan, transaksi, insentif, dan absensi; daftar cek aturan insentif; KeyValue di usulan toko; total pesanan dan menu "Aksi lain" di pesanan; konfirmasi dan alasan wajib untuk usulan toko dan klaim; blocker bersama untuk hapus kunjungan, task, brief, dan pertanyaan mitra; nada toast default untuk hapus; StatCard di Insentif dan Sinkron; sparkline target di grafik harian; titik berdenyut di Live tracking; kartu risiko di hero; bar bawah diturunkan dari NAV; ukuran sentuh 44px di layar sentuh.
  - Deviasi yang diterima (dengan alasan): judul halaman 18px di header, bukan 24px di badan halaman, karena header memegang judul; tab periode Overview tetap di kartu Sell-out; tabel di ponsel menampilkan empat kolom pertama dan sisanya di panel; panel di URL (`?id=`, `?new=1`) tidak dibuat karena butuh registri per panel; objek `paths` tidak dibuat karena NAV sudah satu sumber jalur; baris insight dan rekomendasi tidak dibuat karena tidak ada fitur itu di Sales App; filter workspace belum menjangkau leads, absensi, dan klaim; halaman tabel tidak disimpan per entri riwayat; pilihan tampilan antar sesi tidak disimpan.
  - Pembaruan status per butir di keempat lampiran audit tidak ditulis ulang satu per satu. Sumber kebenaran status adalah laporan verifikasi ditambah daftar perbaikan penutup di atas.
