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
- **Dashboard:** sejak 11 Nov 2026 mengikuti skill `wit-mes-frontend` (WIT MES) dengan **aksen tetap biru mobile**. Plan fase dan matriks cakupan ada di `plan.md`; audit butir per butir di `docs/wit-mes/`.
  - Kerangka: rail ink mengambang dengan satu baris per seksi (sub-halaman terbuka di tempat, ikon saja di tablet), header 18px dengan pencarian global langsung, tombol Buat, notifikasi turunan, dan menu akun. Di bawah 1280px halaman seksi tampil sebagai tab pill. Di HP: bar bawah ink dan semua menu sebagai bottom sheet.
  - Komponen dan perilaku: kit di bagian "KIT KOMPONEN" file dashboard (tombol, badge, form, combobox, tabel, dialog, menu, toast). Semua perubahan data lewat `act()` yang mencatat audit bercap pelaku dan jam aplikasi (26 Okt 2026, 09.41 WIB).
  - Izin: dua akun demo, Admin SIM dan Supervisor (Usulan). Penjaga di layar hanya kenyamanan; backend yang memutuskan izin.
  - Penyimpanan: kunci `salesapp.dashboard.*` (tema, rail, sesi, status baca, data) dan `salesapp.shared.v1` (dibagi dengan mobile). Reset demo di Pengaturan menghapus semuanya dan memuat ulang.
  - Aksen tetap biru, tan sebagai warna kedua, merah hanya untuk status bahaya.
- Fitur di luar BRD/URD diberi label **"Usulan"** di panel demo mobile, supaya client dapat membedakan.
- Warna merah hanya untuk status bahaya (ditolak, overdue, error).

## 4. Isi mockup (setelah revisi MOM 27-28 Okt 2026)

Acuan revisi: `../docs/00-sources/extracted/S5-mom-workshop-27-28-okt-2026.md`. Konflik dengan FSD dan hal yang ambigu ada di `../docs/00-sources/OPEN-QUESTIONS.md` (KONFLIK-001 sampai 016).

### Mobile (`mobile/index.html`)

13 user demo (password bebas). Panel demo juga bisa mengganti jadwal WFO, WFA, dan OFF.

| Username | Role |
|---|---|
| `spg.rina` | SPG / Sales Promotor Elektronik |
| `sales.budi` | Sales Canvassing |
| `sales.dimas` | Sales Taking Order |
| `spv.sari` | Team Leader (SPV 1), user client |
| `ffor.yoga` | FFOR (SPV 2) |
| `arco.maya` | Arco |
| `cbd.tono` | CBD / Mediator |
| `rea.nina` | REA / SPG Multi Finance |
| `fmcg.tari`, `md.rangga` | Promotor FMCG, Merchandising |
| `mobile.fikri` | Mobile Promotor |
| `bantuan.eko`, `bantuan.gita` | Promotor Bantuan |

- **Login dan absen:** foto wajah referensi wajib saat login pertama. Beranda terkunci sebelum absen masuk. WFO dalam radius 100 m, WFA bebas, OFF tidak bisa absen. Ada pengingat sebelum absen dengan pilihan suara.
- **Kunjungan:** jadwal dibuat Team Leader. Hanya Promotor FMCG dan MD yang membuat sendiri (1 hari 1 toko). Periode harian, mingguan, 2 mingguan, bulanan; untuk mingguan dan 2 mingguan wajib memilih hari (MOM 9 Nov). Check-in dan check-out wajib 1 foto ber-watermark. Lupa check-out ditutup otomatis. Sales Canvasing dan Taking Order check-in dan check-out di tiap toko, hanya dalam radius toko. Keluar radius tanpa check-out memunculkan notifikasi; saat check-in di toko berikutnya, sales boleh check-out toko sebelumnya dari luar radius (revisi Nov 2026, perlu dikonfirmasi ke klien). Quiz dan Survey bisa menjadi tugas kunjungan.
- **Penjualan (Canvassing, Mobile Promotor, Promotor Bantuan):** pilih toko → foto invoice dengan OCR (harga invoice, harga toko, dan harga principal sebagai pembanding) → foto dan data pelanggan wajib → konfirmasi. Mobile Promotor juga memilih toko lain dan event. Event berasal dari master Event di dashboard dan muncul selama masa acara.
- **Halaman Aktivitas (Canvassing, Taking Order, Promotor FMCG dan MD):** tab Aktivitas dipakai untuk pilih toko (jika lebih dari satu), check-in, dan check-out. Judul Aktivitas Saya untuk 1 toko, Aktivitas Toko untuk lebih dari 1 toko. Jika toko lebih dari satu, tombol Pilih toko membuka dialog. Daftar Pekerjaan (misalnya Cek stok, Price promo, Facing display, Jual dari stok, Stok bawaan, Catat retur, Buat pesanan) ada di halaman Aktivitas, tidak di Beranda. Beranda hanya punya menu Aktivitas. Pekerjaan terikat lokasi terkunci sampai check-in. Menu Aktivitas (dan tombol check-in dan check-out) terkunci sampai pengguna absen masuk. Wajib semua tugas selesai sebelum check-out hanya untuk Promotor FMCG dan MD.
- **Multi Finance, REA, dan CBD:** di mobile role Multi Finance tidak ada lagi; yang ada hanya REA (`rea.nina`, cabang Fatmawati) dan CBD (`cbd.tono`, cabang Cilandak). Tanpa menu Kunjungan, tanpa Aktivitas, dan tanpa check-in atau check-out ke nasabah. Tab bawah: Beranda, Leads, Absen, Target, Akun. Menu beranda memakai Leads (CBD juga Mitra saya dan Task Team Leader). Halaman Target tanpa bagian KPI. Di dashboard role Multi Finance (r6) dan user Andi serta Rudi juga dihapus; leads mereka dipindah ke REA, dan REA serta CBD menjadi penerima assign leads.
- **Canvassing:** limit nominal per hari, stok dari input manual atau foto invoice, foto tanda terima, retur layak dan tidak layak, rekap akhir hari (penjualan dihitung dari retur), outlet isi sendiri.
- **Promotor FMCG dan MD:** tanpa penjualan. Semua pekerjaan ada di satu halaman Aktivitas (tab bawah): pilih toko jika ditugaskan di lebih dari satu, check-in, kerjakan tugas, lalu check-out. Judulnya Aktivitas Saya untuk 1 toko dan Aktivitas Toko untuk lebih dari 1 toko. Beranda hanya punya satu menu Aktivitas (di urutan pertama), tanpa menu pekerjaan lain. Tugas terkunci sampai check-in; tugas kunjungan selalu semua aktivitas toko kecuali support task,  dan check-out hanya bisa setelah semuanya selesai. Status tugas hanya Belum Selesai, Draft, dan Selesai; tiap tugas punya tombol Simpan sementara yang membuatnya berstatus Draft. Lupa check-out: saat check-in di toko berikutnya muncul informasi, lalu toko sebelumnya ditutup otomatis. Cek stok multi satuan, tanpa foto, price promo (harga sebelum promo dari sistem, harga promo diinput promotor, persen diskon dihitung otomatis), facing display (isi menu seperti Campaign, POSM & planogram, item Rak Utama, Boot, Gudang), support task, dan market share harian. Campaign, POSM & planogram dan SKU display tidak ada di menu Promotor FMCG dan MD.
- **SPG Elektronik (`spg.rina`):** SPG hanya di satu lokasi, yaitu toko penempatan yang diatur admin di dashboard, jadi tidak ada pilihan toko. Menu **Aktivitas saya** langsung meminta check-in di lokasi itu. Check-in cukup sekali, lalu tiga aktivitas terbuka:
  - **Catat penjualan:** pilih Foto invoice atau Input manual. Satu transaksi satu produk: kategori produk (dropdown kategori brand), status produk (dropdown dari master Status produk), nama produk, harga, diskon, harga setelah diskon (otomatis), tanggal pembelian, jumlah, foto produk, nama pembeli, alamat pembeli (opsional), dan telepon pembeli. Foto invoice mengisi semua data produk lewat OCR; data pembeli dan foto produk tetap diisi SPG. Bisa banyak transaksi tanpa check-in ulang.
  - **SKU display:** tambah display (bisa lebih dari satu), pilih kategori, lalu tambah produk lewat Scan QR atau Input manual. Input manual berisi nama produk dan minimal 2 foto. Scan QR mengisi nama produk otomatis.
  - **Market share:** lihat butir Market share di bawah.
  Stock opname memakai halaman Cek stok Promotor FMCG dan juga butuh check-in. Target dan pencapaian SPG dihitung dari quantity produk terjual, dengan progress bar total, per kategori, dan hari ini. Akuisisi pelanggan baru tidak ditampilkan. Campaign, POSM, dan planogram tetap dengan nilai AI simulasi.
- **Multi Finance:** leads dengan status Baru, Prospect, Deal, Belum Tertarik (turunan tertarik, masih prospect, rejected). Buat leads sendiri. Deal memuat nominal, tenor, foto approval, dan dokumen pencairan. CBD punya mitra dan task dari Team Leader. Insentif REA cair setelah dokumen pencairan diunggah. Deal punya tiga dokumen: bukti approve, persetujuan pencairan dana, dan dokumen serah terima dana dengan nasabah.
- **Team Leader dan Arco:** assign leads per cabang (pilih user REA atau CBD dulu, lalu pilih satu leads atau lebih; import assign tidak ada di mobile), pindah cabang (menerima leads cabang baru), assign canvasser ke satu toko atau lebih, Task Pending sampai closed, approval.
- **Pilih toko saat check-in atau check-out (semua role):** tombol Check-in / Check-out di layar Kunjungan. Jika jadwal hari ini punya lebih dari satu toko, pengguna memilih toko dulu; jika hanya satu, langsung diproses.
- **Aturan lokasi dan form kunjungan:** check-in dan check-out harus di lokasi yang sama (cek lewat posisi GPS di panel demo). Promotor FMCG bisa check-in lagi di lokasi yang sama dalam sehari (sesi ke-2 dan seterusnya). Tiap role punya form kunjungan sendiri, kecuali Promotor FMCG dan MD yang tanpa form. Facing display dibandingkan dengan plan per tahap. Produk yang wajib difoto di SKU display ditentukan admin.
- **Market share:** filter berupa dropdown berjenjang: brand klien dulu, lalu kategori dalam brand itu, lalu sub kategori tanpa batas level (dropdown berikutnya muncul selama masih ada turunan). Di bawahnya produk klien sesuai sistem dan produk kompetitor yang diinput manual (brand, kategori, nama, qty, harga opsional). Master Kategori produk di dashboard mengizinkan sub kategori bertingkat, dan ada master baru Status produk.
- **Konfigurasi aplikasi (dashboard):** fitur SPG diatur dari dashboard dan dibaca mobile lewat data bersama, tanpa nilai tetap di kode.
  - **Aktivitas per role:** per role diatur sumber lokasi check-in (toko penempatan di User SIM atau pilih dari jadwal), wajib tidaknya check-in, modul di Aktivitas saya, menu terpisah yang ikut terkunci check-in, minimal foto dan cara tambah produk SKU display, boleh tidaknya lebih dari satu display, simpan sementara market share, serta metrik dan dasar pencapaian target. Role tanpa konfigurasi memakai alur bawaan. Role berlokasi penempatan wajib punya tepat satu toko di User SIM.
  - **Form penjualan:** field builder untuk Catat penjualan. Tiap field punya label, kunci, tipe isian, sumber pilihan (master Kategori produk, Status produk, Produk, atau opsi manual), rumus untuk tipe Hitung, bagian, wajib, terisi dari OCR, urutan, dan role. Peran field (kategori, nama produk, harga satuan, jumlah, pembeli) menghubungkan isian ke total, target, dan laporan; field berperan tidak bisa dihapus atau dinonaktifkan.
  - **Target quantity:** tab baru di Target & sell-in, target unit per user, kategori, dan bulan. Target harian di mobile dihitung prorata dari total ini.
  - Brand, kategori bertingkat, produk, dan status produk berasal dari Master data. Transaksi dari mobile masuk ke Laporan penjualan › Transaksi beserta semua isian form.
  - Yang tetap di kode hanya kemampuan aplikasi: daftar modul aktivitas yang bisa dirender, tipe field form, dan jenis metrik target.
- **Cabang dan akun:** leads melekat ke cabang dan hanya bisa di-assign ke promotor di cabang yang sama. Team Leader dan Arco adalah user client, Promotor Bantuan di luar BPR; Akun menampilkan tipe akun.
- **Alur approval:** kartu approval menampilkan urutan approver per role (Team Leader lalu FFOR, atau Arco lalu Team Leader).
- **Beranda dan target:** kartu target hari ini, sudah terjual, dan objektif. Estimasi insentif dan target per toko tidak ada. Target Multi Finance tanpa KPI.
- Lainnya: quiz, survey, notifikasi, akun (termasuk penugasan toko bergerak atau diam).

### Dashboard (`dashboard/index.html`)

- **Revisi MOM 9 Nov 2026 (konsolidasi notes SFA):** rincian status per poin dan perbandingan dengan modul SFA umum ada di `docs/ANALISIS-KONSOLIDASI-SFA.md`. Ringkasnya: hierarki Company (grup perusahaan client, dikonfirmasi 10 Nov; company user diturunkan dari client) → Client → Cabang client → Brand → Kategori → Produk; menu Company, Cabang client, Mapping outlet (satu toko bisa ke beberapa client), Kompetitor terpisah, Data penjualan (input dari dashboard bila gagal input di aplikasi, dengan alasan, masuk ledger `trxApp` dengan `src:'Dashboard'`); Penugasan & jadwal (tab penempatan dan jadwal); modul Sales retail dan Sales automation pindah dari Client ke Role; User management (Direktur, HR, Manager, admin; User atau People lewat akses Tanpa akses) dan Sales management; flag sumber Integrasi SIM atau Input manual; toko punya sumber (Dari client, Input sales) dan tab Toko canvassing (sementara) yang bisa dijadikan master; produk di luar master tetap bisa dijual (mobile dan form Data penjualan), masuk tab Dari sales (sementara), dan bisa dijadikan master (konfirmasi 10 Nov); kategori produk punya cakupan General, Company, atau Client. Enam modul SFA umum ditambahkan sebagai **Usulan** (disetujui 10 Nov, hanya dashboard): Penagihan & piutang, Klaim biaya, Live tracking, Kepatuhan kunjungan, Pengumuman, Status sinkron. Sidebar di bawah sudah diganti menjadi 12 grup: Ringkasan; Operasional lapangan (tanpa Penugasan); Penjualan (diawali Data penjualan); Kinerja & insentif; Leads pinjaman; Program & konten; Usulan SFA (enam modul di atas); Client management (Company, Client, Cabang client, Mapping outlet, Penugasan & jadwal); Toko (Toko, Region, Kategori toko); Master data (Brand, Kompetitor, Kategori produk, Produk & harga, Status produk, Satuan, Konversi); Master data user (User management, Sales management, IAM); Pengaturan aplikasi (Aktivitas per role, Form penjualan, Parameter).
- **Sidebar sebelum revisi 9 Nov (9 grup menurut fungsi):** Ringkasan (Overview); Operasional lapangan (Absensi, Penugasan, Kunjungan, Approval, Task pending, Aktivitas lapangan); Penjualan (Laporan penjualan, Pemesanan, Sell-in dealer, Canvassing, Laporan SKU display, Laporan market share); Kinerja & insentif (Target & sell-in, Insentif, Grading); Leads pinjaman (Leads pinjaman dengan filter periode, klien, cabang, produk, dan ditangani seperti Laporan penjualan; Overdue; Mitra); Program & konten (Promo, Event, Campaign/POSM/planogram, Quiz, Survey); Master toko & organisasi (Client, Region, Kategori toko, Toko); Master produk (Brand, Kategori produk, Produk & harga, Status produk, Satuan (Dus, Pack, Unit, Pcs; hanya nama dan kode, Pcs satuan dasar terkunci), Konversi (jumlah pcs per produk, bilangan bulat; dikunci setelah dipakai stock opname, ganti dengan menonaktifkan lalu membuat baru; angka pemakaian awal dummy, lalu bertambah dari stock opname yang dikirim di mobile). Di mobile, pilihan Satuan pada Cek stok dan Stock opname hanya Pcs dan satuan yang punya konversi untuk produk itu, dibaca dari master (dinamis), dan total pcs ditampilkan di bawah isian); Pengguna & pengaturan (User sales, User admin, IAM, Aktivitas per role, Form penjualan, Parameter). Rincian fitur di bawah dikelompokkan per topik, bukan per grup sidebar.

- **Operasional:** Absensi, Penugasan (menu baru sesuai MOM 9 Nov bagian 4: penempatan sales ke toko, dilihat per sales atau per toko, dengan tipe Diam atau Bergerak; hanya toko dan sales satu client yang bisa dipasangkan), Kunjungan (jadwal dibuat, diedit, dan dihapus di sini; jadwal Promotor FMCG dan MD tampil di mobile dan tidak bisa dihapus promotor; panel kunjungan FMCG menampilkan status tugas Belum Selesai, Draft, Selesai dari mobile), Approval, Task pending (produk rusak dan task CBD), Aktivitas lapangan (log FMCG dan MD dengan status tugas, langsung dari mobile; kinerja Promotor Bantuan). Aktivitas lapangan punya dua tab: Hasil kerja per sales (**Usulan**, belum ada di BRD/URD: rekap pekerjaan wajib dan input mobile per sales: klien, cabang, toko, total pekerjaan, selesai, belum; filter peran, klien, cabang, toko, dan tanggal seperti Laporan penjualan; panel rincian per peran dan tautan ke laporan asal; data aktivitas hanya ada sejak 25-26 Okt, retur dan sebagian stok bawaan dummy) dan Log aktivitas toko.
- **Penjualan:** Laporan penjualan (semua angka dihitung dari ledger transaksi, lihat bagian Sumber angka penjualan; kartu Filter di atas halaman memilih klien, cabang, toko, sales, dan rentang tanggal: preset Hari ini, 7 hari, 30 hari, Oktober, Mei–Okt, atau tanggal mulai dan akhir sendiri, dan berlaku untuk grafik, rincian, komposisi, dan semua tab; rentang lebih dari 31 hari ditampilkan per bulan), Pemesanan (halaman baru: PO sales taking order, dihitung terpisah dari penjualan ritel, pembatalan lewat Approval), Sell-in dealer (halaman baru: sell-in dan target per dealer per bulan, plus master Dealer), Target dan sell-in (target per sales dan per toko per bulan bisa diubah; tab REA & CBD tanpa KPI), Canvassing (limit harian; penempatan canvasser ke toko dipindah ke menu Penugasan), laporan SKU display, laporan market share (hanya data check-out, rata-rata antar penginput), Promo, Insentif, Grading (dengan formula per klien dan role).
- **Sales automation:** Leads pinjaman (Assign leads seperti mobile: pilih user REA atau CBD, lalu satu leads atau lebih di cabang user itu; penerima diambil dari role, jadi user REA atau CBD baru langsung muncul; import assign tetap ada di dashboard), Overdue.
- **Tabel CRUD:** semua tabel CRUD punya kolom Aksi dengan tombol Edit dan Hapus (hapus selalu soft delete dengan konfirmasi, data yang dipakai tidak bisa dihapus): master data, Kunjungan, Task CBD, Brief POSM, Promo (hapus perlu approval), Quiz dan Survey, Item facing display, Harga per toko, dan Price promo.
- **Master data dengan CRUD:** Client, Region, Kategori toko, Toko, Brand (dua tab dengan data terpisah: Client dan Kompetitor), Kategori produk, Produk dan harga (harga per toko dan cabang yang bisa diatur sendiri, mapping outlet untuk produk elektronik dan FMCG, produk price promo, produk difoto, plan display, produk Multi Finance), Satuan, Event, Mitra (dengan kode dan alamat), User SIM (jadwal off dan NPO dari BPR), IAM (15 role dengan segmen dan tipe, struktur approval, matrix approval, log), Parameter.
- **Konten:** Campaign, POSM, dan planogram, Quiz, Survey (jadwal berkala, tanpa opsi anonim).

**Sumber angka penjualan (dashboard).** Ledger transaksi `TRX` (Mei sampai Okt 2026, dibangkitkan dari sales, toko, dan produk dummy) digabung dengan transaksi dari mobile (`trxApp`) adalah satu-satunya sumber angka penjualan. Grafik harian, mingguan, bulanan, komposisi kategori, rincian klien sampai sales, tab Per klien, cabang, toko, sales, Transaksi, laporan per sales, Overview, Target (aktual), Insentif, dan Grading membaca darinya lewat `trxIn`, `sumVal`, `dayTotals`, dan `repAtoms`. Target dibaca dari menu Target (`MD.tgtnom` per sales, `MD.tgttoko` per toko), sell-in dari `MD.sellin`, pesanan taking order dari `MD.order`. Pengajuan hapus penjualan dan batal pesanan di Approval mengubah ledger dan pesanan. Yang bukan dari ledger: skor quiz, SKU display, market share di Grading, absensi (`ATT`), dan data leads pinjaman.

Semua data dummy. CRUD bekerja di memori dan hilang saat reload, kecuali data yang dibagi antara mobile dan dashboard lewat `localStorage` browser yang sama (kunci `salesapp.shared.v1`, satu domain di Vercel):

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
| Pertanyaan monitoring mitra (teks bebas) | Dashboard (Mitra, tab Pertanyaan monitoring) | Mobile (CBD, detail Mitra saya) |
| Jawaban monitoring mitra | Mobile (CBD) | Dashboard (Mitra, tab Jawaban) |
| Item facing display (Rak Utama, Boot, Gudang) | Dashboard (Produk dan harga, tab Facing display) | Mobile (Facing display) |
| Harga sebelum promo | Dashboard (Produk dan harga, tab Price promo) | Mobile (Price promo; harga promo diinput promotor) |
| Jadwal kunjungan Promotor FMCG dan MD | Dashboard (Kunjungan) | Mobile (Kunjungan) |
| Status kunjungan dan tugas FMCG | Mobile (check-in, tugas) | Dashboard (Kunjungan) |
| Log aktivitas FMCG | Mobile (simpan sementara atau selesai) | Dashboard (Aktivitas lapangan) |

Datanya tersimpan di browser tiap pengguna, bukan di server. Tombol reset ada di panel demo mobile dan di Pengaturan dashboard. Hak akses per role di IAM dipisah per platform: website memakai Lihat, Buat, Ubah, Hapus, Approve per menu dashboard; mobile cukup centang Aktif per menu aplikasi. Bagian yang tampil mengikuti isian Platform role. Keduanya tetap hanya tampilan.

## 5. Hal yang tidak ada di mockup

- Rincian penjualan Mobile Promotor di event. Mockup hanya menampilkan event sebagai tujuan, dengan alur yang sama seperti toko.
- Nilai AI, OCR, pembacaan foto, dan KPI adalah simulasi.
- Tambahan yang tidak disebut MOM diberi label "Usulan" atau "Rekomendasi".

## 6. Keputusan dan hal yang masih terbuka

Keputusan 2 Nov 2026 (lengkap di `../docs/00-sources/OPEN-QUESTIONS.md` dan `../docs/01-product/fsd-revisi-mom.md`):

- **FSD direvisi mengikuti MOM** ([ADR-0015](../../docs/02-architecture/decisions/ADR-0015-fsd-ikut-mom-workshop.md)). Sepuluh konflik FSD dan MOM tidak lagi terbuka.
- **Dijawab MOM:** pembuat jadwal kunjungan (Team Leader, kecuali FMCG dan MD), dua dokumen Deal, bentuk leveling produk.
- **Rekomendasi, dikonfirmasi di MOM hasil review berikutnya:** scan barcode hanya di SKU Display, foto submit Canvassing dihitung sebagai foto check-out, Reliever dan Promotor Bantuan digabung menjadi satu role Promotor Bantuan (12 Nov 2026), badge sidebar dihapus, hierarki, format data mitra dan file leads, sinkron BPR harian, ambang nilai AI (80 lulus, 70 sampai 79 ditinjau, di bawah 70 foto ulang), sumber overdue dan goal.
- **Menunggu pihak lain:** API ganti password ke IOS 1 atau BPR, use case lengkap aturan leads Multi Finance.

Catatan: klien sudah mengesahkan MOM pertama. MOM hasil review di meeting berikutnya akan menjadi sumber baru dan bisa mengubah keputusan di atas.

## 7. Catatan teknis

- Tiap mockup satu file HTML mandiri (CSS dan JS inline, logo base64). Tidak ada build step.
- Mobile: router tumpukan (`go`, `back`, `reset`), layar didefinisikan sebagai `SCR['nama'] = props => ({html, note, tab, dark})`. `S` adalah state, `USERS`, `PRODUCTS`, `TOKO` adalah data dummy. `renderPanel()` membuat panel demo.
- Dashboard, animasi muat data: karena tidak ada backend, `render()` mensimulasikan fetch. Pindah halaman memuat penuh dengan skeleton yang mengikuti bentuk komponen; ganti query (filter, tab) memuat ulang bagian data saja (tabel dan grafik meredup, garis progres di header). Setelah data siap, tiap komponen punya animasi masuk sendiri: kartu naik bergantian, angka statistik menghitung naik, batang kolom tumbuh, garis tergambar, donut menyapu, baris tabel berurutan, progress bar terisi. Render karena state lokal (pilih baris, hover) tidak memicu animasi. Semua dimatikan bila `prefers-reduced-motion` aktif. Kode di `FX`, `fxBegin`, `fxAnimate`, `countUp`.
- Dashboard: router hash (`#/penjualan?tab=klien`), `ROUTES`, `ENT` (definisi CRUD generik: `openForm`, `saveForm`, `delAsk`), `dataTable`, `openPanel`, `card`, chart SVG buatan sendiri (`columnChart`, `donut`, `lineChart`).
- Preview di browser pane Claude memakai snapshot file sehingga navigasi hash tidak jalan; gunakan server lokal.
- Skill WIT (`wit-code-agent-2`) berasal dari `wit-skills.zip` milik tim dan tidak disertakan di sini.


## 7. Skrip perjalanan demo (verifikasi manual)

Jalankan `python3 -m http.server 5173`, buka `http://localhost:5173/dashboard/`. Cek tiap langkah di lebar 1440, 768, dan 375, tema terang dan gelap.

1. Masuk sebagai Dewi Kartika (Admin SIM) lewat tombol akun demo. Overview menampilkan kartu "Perlu tindakan".
2. Buka Approval. Setujui satu pengajuan: konfirmasi muncul, toast menyebut jenis dan pengaju. Tolak satu pengajuan: tombol Tolak tidak aktif sampai alasan diisi.
3. Dari Overview buka "Leads belum di-assign": banner "Daftar dipersempit" muncul, "Tampilkan semua" melepasnya. Assign leads ke manager: toast menyebut jumlah dan penerima.
4. Buka Master data > Sektor. Tombol hapus pada sektor yang dipakai tidak aktif dan tooltip menyebut alasannya. Tambah sektor dengan kode kosong: galat berupa kalimat dan hilang saat mengetik.
5. Buka Pesanan, pilih satu pesanan: hero ink, tahapan, dan riwayat tampil. Ajukan batal untuk pesanan berstatus Dibuat: toast bernada default, pengajuan muncul di Approval.
6. Ganti akun ke Rudi Hartono (Supervisor) lewat menu akun. Menu Buat lebih pendek, master data tampil baca saja, dan Pengaturan tidak menampilkan Reset data demo.
7. Di lebar 375: bar bawah, menu Buat dan More sebagai bottom sheet, panel rincian membuka dari bawah dan tombol Back peramban menutupnya.
8. Pengaturan > Reset data demo: konfirmasi, halaman dimuat ulang dengan data awal.

9. Penugasan > Penempatan toko: tambahkan toko kedua ke Lestari Putri (Promotor FMCG) lewat Ubah. Ditolak dengan toast yang menyebut role satu toko. Rangga Saputra (MD) boleh banyak toko.
10. Penugasan > Journey plan, pilih Budi, Minggu 1: Kamis bertanda Off dan tombol tambah nonaktif. Tambah toko ke Senin sampai melewati kapasitas: tersimpan, ringkasan validasi memperingatkan. Ubah urutan toko Senin, lalu masuk ke mobile sebagai sales.budi: daftar Kunjungan hari ini mengikuti urutan itu.
11. Canvassing: kartu Sisa barang menampilkan batas 24 jam (ubah di kartu Batas pengembalian). Check-out tidak diblokir.
12. Mobile sebagai sales.budi, Usulkan toko: tombol bernama Simpan toko, toko langsung aktif dan masuk daftar kunjungan. Di dashboard, Toko > Toko Sementara menampilkan toko itu. Role lain tetap diverifikasi admin.

Catatan Usulan (konfirmasi klien 11 Nov 2026, lihat `OPEN-QUESTIONS.md` TL-013 sampai TL-017): batas penempatan per role, journey plan, frekuensi per kategori, sisa barang canvassing, dan toko baru canvassing langsung aktif. Data bersama lewat `salesapp.shared.v1` dengan kunci `jpPlan` (memuat nama toko, karena ID toko mobile dan dashboard berbeda), `visitFreq`, `jpKap`, `canvasSisaJam`, `cvSisaOk`, `tokoBaruSales`.
13. Penugasan > Journey plan, pilih Budi: kartu Due, overdue, dan validasi menampilkan Toko Maju Elektronik Cinere sebagai Overdue (0 dari 1). Pilih Minggu 2, tekan Salin dari minggu 1: kunjungan tersalin, yang sudah ada dilewati, dan toast menyebut jumlahnya. Minggu 1 tidak punya tombol salin aktif.
14. Penugasan > Frekuensi > Frekuensi khusus per toko: ubah Toko Maju Elektronik Cinere ke 3 kali. Di Journey plan, target toko itu menjadi 3 dan bertanda khusus toko. Kosongkan isian untuk kembali ke default kategori.
15. Kepatuhan kunjungan > Plan vs actual minggu lalu: Terlewat dan Dijadwal ulang tampil terpisah. Dijadwal ulang tidak dihitung sebagai kelalaian. Angkanya contoh sampai mobile mencatat keduanya.
16. Mobile sebagai sales.budi (absen masuk dulu), tab Kunjungan: banner amber "Kunjungan ke Toko Cahaya Baru terlewat". Buka, simpan tanpa alasan (galat), pilih alasan, simpan. Banner hilang. Di dashboard, Kepatuhan kunjungan > Pengecualian dari lapangan menampilkan baris Terlewat dengan alasannya.
17. Mobile, buka detail Toko Makmur Abadi (belum check-in): ikon kalender di appbar membuka sheet Jadwal ulang. Kamis tidak muncul karena hari off Budi. Pilih hari dan alasan, simpan: status Dijadwal ulang, tombol check-in diganti tombol nonaktif. Di dashboard, kartu Plan vs actual hari ini menambah satu pada Dijadwal ulang, dan kartu pengecualian menampilkan "26 Okt → Rabu 28 Okt".
18. Penugasan > Journey plan: filter berurutan Role, Sales, lalu Minggu. Ganti role ke Taking Order: hanya sales di role itu yang tampil. Tiap toko di hari tampil sebagai card putih bernomor dengan kategori, dengan tombol naik, turun, dan hapus.
19. Mobile sebagai sales.budi, tab Kunjungan: tombol Kunjungan di luar rencana membuka sheet pilih toko dan alasan wajib. Kunjungan baru bertanda Unplanned di daftar dan detail. Usulkan toko baru oleh canvasser juga tercatat sebagai Unplanned.
20. Dashboard: lonceng notifikasi menampilkan jadwal ulang, kunjungan terlewat, dan Unplanned dari mobile (maksimal lima terbaru). Klik membuka Kepatuhan kunjungan. Kolom Di luar rencana di Plan vs actual hari ini ikut bertambah.
21. Mobile sebagai sales.budi, Stok bawaan: kartu Sisa barang dari check-out menampilkan sisa waktu atau "Lewat N jam" dengan banner. Ubah batas jam di dashboard (Canvassing > Batas pengembalian) atau tandai satu sisa dikembalikan: mobile ikut berubah (status Dikembalikan, sisa waktu baru). Role lain tidak melihat kartu ini.
22. IAM > Role: ada dua role, Canvasing FMCG dan Canvasing Elektronik. Form role punya atribut Membawa barang dari gudang (Ya atau Tidak). Ubah Canvasing Elektronik ke Ya, lalu buka Canvassing: Kevin Halim muncul di rekap dan daftar sisa barang. Kembalikan ke Tidak, Kevin hilang. Atribut ditulis ke data bersama (userBawa) untuk mobile.
23. Mobile sebagai sales.kevin (Canvassing Elektronik): aktivitas hanya Usulkan toko, tanpa Jual dari stok, Stok bawaan, Catat retur, dan kartu Sisa barang. Daftar kunjungan Kevin terpisah dari Budi. Sebagai sales.budi (Canvassing FMCG) semua fitur barang tetap ada.
24. Penugasan > Jadwal kunjungan atau menu Kunjungan: jadwal Rangga Saputra ke Electronic City Kemang bertanda Dibuat Merchandising (MD). Buka rinciannya: ada catatan Dibuat sendiri, tanpa persetujuan Team Leader. Jadwal yang dibuat promotor FMCG atau MD di mobile (hanya bila belum ada jadwal hari itu) juga masuk dengan penanda sama.
25. Ganti akun dashboard ke Rudi Hartono (Supervisor, Team Leader Sari): lonceng menampilkan jadwal ulang, terlewat, dan Unplanned dari sales di timnya saja. Akun Admin tidak menerima notifikasi itu, tetapi kartu Pengecualian dari lapangan tetap menampilkan semuanya.
26. Mobile sebagai spg.rina atau fmcg.tari (satu toko): tab Kunjungan tidak punya tombol Tambah kunjungan, ada petunjuk "Kamu ditugaskan di satu toko saja". Sebagai sales.budi, sales.kevin, sales.dimas, atau md.rangga (lebih dari satu toko): tombol Tambah kunjungan muncul bersama Kunjungan di luar rencana. Form hanya menawarkan toko yang ditugaskan, boleh lebih dari satu jadwal per hari, dan toko yang sama ditolak. Jumlah toko mengikuti Penugasan > Penempatan toko di dashboard (kunci penugasanToko).
27. Mobile sebagai sales.kevin (Canvassing Elektronik): aktivitas Catat penjualan, Usulkan toko, dan Riwayat penjualan, sama seperti canvassing lain tetapi tanpa stok bawaan. Penjualan tidak dibatasi stok dan tidak mengurangi stok Budi. Layar Penjualan dan Beranda Kevin tidak menampilkan stok.
28. Kunjungan buatan sales sendiri (lewat Tambah kunjungan) hanya bisa dihapus oleh pembuatnya. Kunjungan dari plan atau Team Leader tidak punya tombol hapus untuk sales.
29. Penugasan > Journey plan: dropdown Siklus (Mingguan, 2 mingguan, Bulanan) mengatur jumlah minggu yang tampil, rentang tanggal, dan target coverage (target 4 minggu dikali panjang siklus, dibagi 4, dibulatkan ke atas). Contoh: Toko Berkah (Semi grosir, 2 kali per 4 minggu) menjadi target 1 pada siklus mingguan.
30. Parameter > Kunjungan per hari: batas per role (bawaan 5). Ubah Canvasing FMCG ke 2, lalu buka Journey plan Budi: Senin dan Rabu menampilkan 3/2 dan peringatan melebihi kapasitas 2. Tab Frekuensi hanya menampilkan petunjuk menuju Parameter.
31. Kepatuhan kunjungan: kartu Plan vs actual per kategori toko (hari ini dan minggu lalu). Klik baris kategori atau baris sales membuka Kunjungan yang terfilter, dengan chip yang bisa dihapus.
32. Kepatuhan kunjungan > Kepatuhan REA dan CBD: Mobile sebagai rea.nina, buka kunjungan ke nasabah dan tekan Tandai selesai. Dashboard menampilkan Nina 1 dari 2 (50%). CBD dihitung dari mitra yang monitoringnya dijawab di mobile (Mitra saya). Tanpa check-in dan radius.
33. IAM > Role: field Boleh menambah kunjungan sendiri. Bawaan Ya untuk Canvasing FMCG, Canvasing Elektronik, Taking Order, dan MD, Tidak untuk promotor satu toko dan Mobile Promotor. Hak menambah jadwal di mobile butuh flag Ya dan lebih dari satu toko; flag yang sama membuka Kunjungan di luar rencana. Rangga (MD) melihat kedua tombol, Tari (Promotor FMCG) tidak.
34. Parameter > Kunjungan per hari: bawaan Canvasing FMCG, Canvasing Elektronik, dan Taking Order 8, MD dan Mobile Promotor 5, role lain mengikuti bawaan 5.
35. Grading > Formula: ada bobot Kepatuhan kunjungan, total bobot tetap 100%. Panel grade menampilkan skor kepatuhan kunjungan dengan bobot dari formula.
36. Kepatuhan kunjungan kini ada di Operasional lapangan, bukan Usulan SFA. Catatan di halaman menyebutnya requirement.
37. Kepatuhan kunjungan > Scheduler akhir hari: Jalankan simulasi 23.59 mengubah kunjungan yang belum check-in menjadi Terlewat, tanpa menyentuh absensi. Mobile menampilkan banner alasan, check-in ditolak, dan panel demo mobile punya tombol simulasi yang sama untuk akun yang login. Check-in kunjungan boleh kapan saja sampai 23.59, check-in absensi terlambat tetap boleh.
38. Journey plan: tiap hari menampilkan tanggal. Jumat 30 Okt bertanda Libur (peringatan, tetap boleh dijadwalkan). Rabu 4 Nov minggu 2 bertanda Cuti untuk Budi dan menolak penambahan toko. Menyetujui cuti di Approval ikut menutup hari itu.
39. Mobile, jadwal ulang: pilihan hari berupa hari kerja dalam siklus sales, tanpa hari off, cuti, dan Minggu, hari libur diberi tanda. Satu kunjungan hanya bisa dijadwal ulang satu kali.
40. Penugasan > Frekuensi: perubahan frekuensi tersimpan untuk siklus berikutnya (23 Nov 2026) dan tidak mengubah coverage yang berjalan. Tombol Mulai siklus berikutnya menerapkannya.
41. Mobile: daftar Hari ini menampilkan jarak ke toko (posisi simulasi). Notifikasi H-1 memakai journey plan dan jam dari Parameter > Pengingat kunjungan H-1. CBD mengunjungi mitra, dan kunjungan Selesai saat jawaban monitoring disubmit.
