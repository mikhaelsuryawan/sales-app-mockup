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

Rangkuman workshop yang sudah dirapikan: `../docs/01-product/analysis/HASIL_WORKSHOP_27-28_OKT_2026.md`. Pertanyaan yang disiapkan sebelum workshop: `../docs/01-product/analysis/PERTANYAAN_WORKSHOP_REQUIREMENT.md`.

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

## 4. Isi mockup (setelah revisi MOM 27-28 Okt 2026)

Acuan revisi: `../docs/00-sources/extracted/S5-mom-workshop-27-28-okt-2026.md`. Konflik dengan FSD dan hal yang ambigu ada di `../docs/00-sources/OPEN-QUESTIONS.md` (KONFLIK-001 sampai 016).

### Mobile (`mobile/index.html`)

16 user demo (password bebas). Panel demo juga bisa mengganti jadwal WFO, WFA, dan OFF.

| Username | Role |
|---|---|
| `spg.rina` | SPG / Sales Promotor Elektronik |
| `sales.budi` | Sales Canvassing |
| `sales.dimas` | Sales Taking Order |
| `sales.andi`, `sales.rudi` | Multi Finance (Rudi di cabang Cilandak) |
| `spv.sari` | Team Leader (SPV 1), user client |
| `ffor.yoga` | FFOR (SPV 2) |
| `arco.maya` | Arco |
| `cbd.tono` | CBD / Mediator |
| `rea.nina` | REA / SPG Multi Finance |
| `fmcg.tari`, `md.rangga` | Promotor FMCG, Merchandising |
| `mobile.fikri` | Mobile Promotor |
| `bantuan.eko`, `reliever.gita` | Promotor Bantuan, Reliever |
| `trainer.joko` | Trainer (quiz, survey, kunjungan pelatihan) |

- **Login dan absen:** foto wajah referensi wajib saat login pertama. Beranda terkunci sebelum absen masuk. WFO dalam radius 100 m, WFA bebas, OFF tidak bisa absen. Ada pengingat sebelum absen dengan pilihan suara.
- **Kunjungan:** jadwal dibuat Team Leader. Hanya Promotor FMCG dan MD yang membuat sendiri (1 hari 1 toko). Periode harian, mingguan, 2 mingguan, bulanan. Check-in dan check-out wajib 1 foto ber-watermark. Lupa check-out ditutup otomatis. Canvassing cukup submit kunjungan. Quiz dan Survey bisa menjadi tugas kunjungan.
- **Penjualan (Canvassing, Mobile Promotor, Promotor Bantuan):** pilih toko → foto invoice dengan OCR (harga invoice, harga toko, dan harga principal sebagai pembanding) → foto dan data pelanggan wajib → konfirmasi. Mobile Promotor juga memilih toko lain dan event. Event berasal dari master Event di dashboard dan muncul selama masa acara.
- **Canvassing:** limit nominal per hari, stok dari input manual atau foto invoice, foto tanda terima, retur layak dan tidak layak, rekap akhir hari (penjualan dihitung dari retur), outlet isi sendiri.
- **Promotor FMCG dan MD:** tanpa penjualan. Wajib pilih toko di Kunjungan lalu check-in sebelum mengerjakan aktivitas toko (menu aktivitas, SKU display, dan market share terkunci sampai check-in); tugas kunjungan selalu semua aktivitas toko kecuali support task,  dan check-out hanya bisa setelah semuanya selesai. Status tugas hanya Belum Selesai, Draft, dan Selesai; tiap tugas punya tombol Simpan sementara yang membuatnya berstatus Draft. Lupa check-out: saat check-in di toko berikutnya muncul informasi, lalu toko sebelumnya ditutup otomatis. Cek stok multi satuan, tanpa foto, price promo (harga sebelum promo dari sistem, harga promo diinput promotor, persen diskon dihitung otomatis), facing display (isi menu seperti Campaign, POSM & planogram, item Rak Utama, Boot, Gudang), support task, dan market share harian. Campaign, POSM & planogram dan SKU display tidak ada di menu Promotor FMCG dan MD.
- **SPG Elektronik (`spg.rina`):** SPG hanya di satu lokasi, yaitu toko penempatan yang diatur admin di dashboard, jadi tidak ada pilihan toko. Menu **Aktivitas saya** langsung meminta check-in di lokasi itu. Check-in cukup sekali, lalu tiga aktivitas terbuka:
  - **Catat penjualan:** pilih Foto invoice atau Input manual. Satu transaksi satu produk: kategori produk (dropdown kategori brand), status produk (dropdown dari master Status produk), nama produk, harga, diskon, harga setelah diskon (otomatis), tanggal pembelian, jumlah, foto produk, nama pembeli, alamat pembeli (opsional), dan telepon pembeli. Foto invoice mengisi semua data produk lewat OCR; data pembeli dan foto produk tetap diisi SPG. Bisa banyak transaksi tanpa check-in ulang.
  - **SKU display:** tambah display (bisa lebih dari satu), pilih kategori, lalu tambah produk lewat Scan QR atau Input manual. Input manual berisi nama produk dan minimal 2 foto. Scan QR mengisi nama produk otomatis.
  - **Market share:** lihat butir Market share di bawah.
  Stock opname memakai halaman Cek stok Promotor FMCG dan juga butuh check-in. Target dan pencapaian SPG dihitung dari quantity produk terjual, dengan progress bar total, per kategori, dan hari ini. Akuisisi pelanggan baru tidak ditampilkan. Campaign, POSM, dan planogram tetap dengan nilai AI simulasi.
- **Multi Finance:** leads dengan status Baru, Prospect, Deal, Belum Tertarik (turunan tertarik, masih prospect, rejected). Buat leads sendiri. Deal memuat nominal, tenor, foto approval, dan dokumen pencairan. CBD punya alur Follow Up → Input Order → Verify Dokumen → Scoring → Survey, mitra, dan task dari Team Leader. Insentif REA cair setelah dokumen pencairan diunggah.
- **Team Leader dan Arco:** assign leads per cabang, import assign, pindah cabang (menerima leads cabang baru), assign canvasser, Task Pending sampai closed, approval.
- **Pilih toko saat check-in atau check-out (semua role):** tombol Check-in / Check-out di layar Kunjungan. Jika jadwal hari ini punya lebih dari satu toko, pengguna memilih toko dulu; jika hanya satu, langsung diproses.
- **Aturan lokasi dan form kunjungan:** check-in dan check-out harus di lokasi yang sama (cek lewat posisi GPS di panel demo). Promotor FMCG bisa check-in lagi di lokasi yang sama dalam sehari (sesi ke-2 dan seterusnya). Tiap role punya form kunjungan sendiri, kecuali Promotor FMCG dan MD yang tanpa form. Facing display dibandingkan dengan plan per tahap. Produk yang wajib difoto di SKU display ditentukan admin.
- **Market share:** filter berupa dropdown berjenjang: brand klien dulu, lalu kategori dalam brand itu, lalu sub kategori tanpa batas level (dropdown berikutnya muncul selama masih ada turunan). Di bawahnya produk klien sesuai sistem dan produk kompetitor yang diinput manual (brand, kategori, nama, qty, harga opsional). Master Kategori produk di dashboard mengizinkan sub kategori bertingkat, dan ada master baru Status produk.
- **Cabang dan akun:** leads melekat ke cabang dan hanya bisa di-assign ke promotor di cabang yang sama. Team Leader dan Arco adalah user client, Reliever dan Promotor Bantuan di luar BPR; Akun menampilkan tipe akun.
- **Alur approval:** kartu approval menampilkan urutan approver per role (Team Leader lalu FFOR, atau Arco lalu Team Leader).
- **Beranda dan target:** kartu target hari ini, sudah terjual, dan objektif. Estimasi insentif dan target per toko tidak ada. Target Multi Finance memuat KPI.
- Lainnya: quiz, survey, notifikasi, akun (termasuk penugasan toko bergerak atau diam).

### Dashboard (`dashboard/index.html`)

- **Operasional:** Absensi, Kunjungan (Team Leader membuat jadwal), Approval, Task pending (produk rusak dan task CBD), Aktivitas lapangan (log FMCG dan MD, kinerja Reliever dan Promotor Bantuan).
- **Penjualan:** Laporan penjualan, Target dan sell-in (termasuk Multi Finance), Canvassing, laporan SKU display, laporan market share (hanya data check-out, rata-rata antar penginput), Promo, Insentif, Grading (dengan formula per klien dan role).
- **Sales automation:** Leads pinjaman (import assign), Overdue.
- **Master data dengan CRUD:** Client, Region, Kategori toko, Toko, Brand, Kategori produk, Produk dan harga (harga per toko dan cabang yang bisa diatur sendiri, mapping outlet untuk produk elektronik dan FMCG, produk price promo, produk difoto, plan display, produk Multi Finance), Satuan, Event, Mitra (dengan kode dan alamat), User SIM (jadwal off dan NPO dari BPR), IAM (16 role dengan segmen dan tipe, struktur approval, matrix approval, log), Parameter.
- **Konten:** Campaign, POSM, dan planogram, Quiz, Survey (jadwal berkala, tanpa opsi anonim).

Semua data dummy. CRUD bekerja di memori dan hilang saat reload, kecuali sepuluh data yang dibagi antara mobile dan dashboard lewat `localStorage` browser yang sama (kunci `salesapp.shared.v1`, satu domain di Vercel):

| Data | Dibuat di | Dipakai di |
|---|---|---|
| Task SKU rusak dan statusnya | Mobile (SKU display), dashboard (Task pending) | Keduanya |
| Task CBD dan jawabannya | Dashboard | Mobile (CBD) |
| Limit canvassing | Dashboard (Canvassing) | Mobile (Stok bawaan) |
| Event | Dashboard (master Event) | Mobile (tujuan penjualan Mobile Promotor) |
| Izin membuat jadwal per role | Dashboard (IAM) | Mobile (Kunjungan) |
| Produk wajib difoto | Dashboard (Produk dan harga) | Mobile (SKU display) |
| Harga per toko | Dashboard (Produk dan harga, tab Harga) | Mobile (harga di penjualan) |
| Produk price promo | Dashboard (Produk dan harga) | Mobile (Price promo FMCG) |
| Mapping produk per outlet | Dashboard (Produk dan harga) | Mobile (Cek stok) |
| Plan tata letak display | Dashboard (Produk dan harga) | Mobile (Facing display) |

Datanya tersimpan di browser tiap pengguna, bukan di server. Tombol reset ada di panel demo mobile dan di Pengaturan dashboard. Menu per role di IAM (Lihat, Buat, Ubah, Hapus, Approve) tetap hanya tampilan.

## 5. Hal yang tidak ada di mockup

- Rincian penjualan Mobile Promotor di event. Mockup hanya menampilkan event sebagai tujuan, dengan alur yang sama seperti toko.
- Nilai AI, OCR, pembacaan foto, dan KPI adalah simulasi.
- Tambahan yang tidak disebut MOM diberi label "Usulan" atau "Rekomendasi".

## 6. Keputusan dan hal yang masih terbuka

Keputusan 2 Nov 2026 (lengkap di `../docs/00-sources/OPEN-QUESTIONS.md` dan `../docs/01-product/fsd-revisi-mom.md`):

- **FSD direvisi mengikuti MOM** ([ADR-0015](../../docs/02-architecture/decisions/ADR-0015-fsd-ikut-mom-workshop.md)). Sepuluh konflik FSD dan MOM tidak lagi terbuka.
- **Dijawab MOM:** pembuat jadwal kunjungan (Team Leader, kecuali FMCG dan MD), dua dokumen Deal, bentuk leveling produk.
- **Rekomendasi, dikonfirmasi di MOM hasil review berikutnya:** scan barcode hanya di SKU Display, foto submit Canvassing dihitung sebagai foto check-out, Reliever dan Promotor Bantuan dua role, badge sidebar dihapus, hierarki, tugas Trainer, format data mitra dan file leads, sinkron BPR harian, ambang nilai AI (80 lulus, 70 sampai 79 ditinjau, di bawah 70 foto ulang), sumber overdue dan goal.
- **Menunggu pihak lain:** API ganti password ke IOS 1 atau BPR, use case lengkap aturan leads Multi Finance.

Catatan: klien sudah mengesahkan MOM pertama. MOM hasil review di meeting berikutnya akan menjadi sumber baru dan bisa mengubah keputusan di atas.

## 7. Catatan teknis

- Tiap mockup satu file HTML mandiri (CSS dan JS inline, logo base64). Tidak ada build step.
- Mobile: router tumpukan (`go`, `back`, `reset`), layar didefinisikan sebagai `SCR['nama'] = props => ({html, note, tab, dark})`. `S` adalah state, `USERS`, `PRODUCTS`, `TOKO` adalah data dummy. `renderPanel()` membuat panel demo.
- Dashboard: router hash (`#/penjualan?tab=klien`), `ROUTES`, `ENT` (definisi CRUD generik: `openForm`, `saveForm`, `delAsk`), `dataTable`, `openPanel`, `card`, chart SVG buatan sendiri (`columnChart`, `donut`, `lineChart`).
- Preview di browser pane Claude memakai snapshot file sehingga navigasi hash tidak jalan; gunakan server lokal.
- Skill WIT (`wit-code-agent-2`) berasal dari `wit-skills.zip` milik tim dan tidak disertakan di sini.
