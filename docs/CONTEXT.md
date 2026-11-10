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
| `bantuan.eko`, `reliever.gita` | Promotor Bantuan, Reliever |

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
- **Cabang dan akun:** leads melekat ke cabang dan hanya bisa di-assign ke promotor di cabang yang sama. Team Leader dan Arco adalah user client, Reliever dan Promotor Bantuan di luar BPR; Akun menampilkan tipe akun.
- **Alur approval:** kartu approval menampilkan urutan approver per role (Team Leader lalu FFOR, atau Arco lalu Team Leader).
- **Beranda dan target:** kartu target hari ini, sudah terjual, dan objektif. Estimasi insentif dan target per toko tidak ada. Target Multi Finance tanpa KPI.
- Lainnya: quiz, survey, notifikasi, akun (termasuk penugasan toko bergerak atau diam).

### Dashboard (`dashboard/index.html`)

- **Revisi MOM 9 Nov 2026 (konsolidasi notes SFA):** rincian status per poin dan perbandingan dengan modul SFA umum ada di `docs/ANALISIS-KONSOLIDASI-SFA.md`. Ringkasnya: hierarki Company (grup perusahaan client, dikonfirmasi 10 Nov; company user diturunkan dari client) → Client → Cabang client → Brand → Kategori → Produk; menu Company, Cabang client, Mapping outlet (satu toko bisa ke beberapa client), Kompetitor terpisah, Data penjualan (input dari dashboard bila gagal input di aplikasi, dengan alasan, masuk ledger `trxApp` dengan `src:'Dashboard'`); Penugasan & jadwal (tab penempatan dan jadwal); modul Sales retail dan Sales automation pindah dari Client ke Role; User management (Direktur, HR, Manager, admin; User atau People lewat akses Tanpa akses) dan Sales management; flag sumber Integrasi SIM atau Input manual; toko punya sumber (Dari client, Input sales) dan tab Toko canvassing (sementara) yang bisa dijadikan master; produk di luar master tetap bisa dijual (mobile dan form Data penjualan), masuk tab Dari sales (sementara), dan bisa dijadikan master (konfirmasi 10 Nov); kategori produk punya cakupan General, Company, atau Client. Enam modul SFA umum ditambahkan sebagai **Usulan** (disetujui 10 Nov, hanya dashboard): Penagihan & piutang, Klaim biaya, Live tracking, Kepatuhan kunjungan, Pengumuman, Status sinkron. Sidebar di bawah sudah diganti menjadi 12 grup: Ringkasan; Operasional lapangan (tanpa Penugasan); Penjualan (diawali Data penjualan); Kinerja & insentif; Leads pinjaman; Program & konten; Usulan SFA (enam modul di atas); Client management (Company, Client, Cabang client, Mapping outlet, Penugasan & jadwal); Toko (Toko, Region, Kategori toko); Master data (Brand, Kompetitor, Kategori produk, Produk & harga, Status produk, Satuan, Konversi); Master data user (User management, Sales management, IAM); Pengaturan aplikasi (Aktivitas per role, Form penjualan, Parameter).
- **Sidebar sebelum revisi 9 Nov (9 grup menurut fungsi):** Ringkasan (Overview); Operasional lapangan (Absensi, Penugasan, Kunjungan, Approval, Task pending, Aktivitas lapangan); Penjualan (Laporan penjualan, Pemesanan, Sell-in dealer, Canvassing, Laporan SKU display, Laporan market share); Kinerja & insentif (Target & sell-in, Insentif, Grading); Leads pinjaman (Leads pinjaman dengan filter periode, klien, cabang, produk, dan ditangani seperti Laporan penjualan; Overdue; Mitra); Program & konten (Promo, Event, Campaign/POSM/planogram, Quiz, Survey); Master toko & organisasi (Client, Region, Kategori toko, Toko); Master produk (Brand, Kategori produk, Produk & harga, Status produk, Satuan (Dus, Pack, Unit, Pcs; hanya nama dan kode, Pcs satuan dasar terkunci), Konversi (jumlah pcs per produk, bilangan bulat; dikunci setelah dipakai stock opname, ganti dengan menonaktifkan lalu membuat baru; angka pemakaian awal dummy, lalu bertambah dari stock opname yang dikirim di mobile). Di mobile, pilihan Satuan pada Cek stok dan Stock opname hanya Pcs dan satuan yang punya konversi untuk produk itu, dibaca dari master (dinamis), dan total pcs ditampilkan di bawah isian); Pengguna & pengaturan (User sales, User admin, IAM, Aktivitas per role, Form penjualan, Parameter). Rincian fitur di bawah dikelompokkan per topik, bukan per grup sidebar.

- **Operasional:** Absensi, Penugasan (menu baru sesuai MOM 9 Nov bagian 4: penempatan sales ke toko, dilihat per sales atau per toko, dengan tipe Diam atau Bergerak; hanya toko dan sales satu client yang bisa dipasangkan), Kunjungan (jadwal dibuat, diedit, dan dihapus di sini; jadwal Promotor FMCG dan MD tampil di mobile dan tidak bisa dihapus promotor; panel kunjungan FMCG menampilkan status tugas Belum Selesai, Draft, Selesai dari mobile), Approval, Task pending (produk rusak dan task CBD), Aktivitas lapangan (log FMCG dan MD dengan status tugas, langsung dari mobile; kinerja Reliever dan Promotor Bantuan). Aktivitas lapangan punya dua tab: Hasil kerja per sales (**Usulan**, belum ada di BRD/URD: rekap pekerjaan wajib dan input mobile per sales: klien, cabang, toko, total pekerjaan, selesai, belum; filter peran, klien, cabang, toko, dan tanggal seperti Laporan penjualan; panel rincian per peran dan tautan ke laporan asal; data aktivitas hanya ada sejak 25-26 Okt, retur dan sebagian stok bawaan dummy) dan Log aktivitas toko.
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
- **Rekomendasi, dikonfirmasi di MOM hasil review berikutnya:** scan barcode hanya di SKU Display, foto submit Canvassing dihitung sebagai foto check-out, Reliever dan Promotor Bantuan dua role, badge sidebar dihapus, hierarki, format data mitra dan file leads, sinkron BPR harian, ambang nilai AI (80 lulus, 70 sampai 79 ditinjau, di bawah 70 foto ulang), sumber overdue dan goal.
- **Menunggu pihak lain:** API ganti password ke IOS 1 atau BPR, use case lengkap aturan leads Multi Finance.

Catatan: klien sudah mengesahkan MOM pertama. MOM hasil review di meeting berikutnya akan menjadi sumber baru dan bisa mengubah keputusan di atas.

## 7. Catatan teknis

- Tiap mockup satu file HTML mandiri (CSS dan JS inline, logo base64). Tidak ada build step.
- Mobile: router tumpukan (`go`, `back`, `reset`), layar didefinisikan sebagai `SCR['nama'] = props => ({html, note, tab, dark})`. `S` adalah state, `USERS`, `PRODUCTS`, `TOKO` adalah data dummy. `renderPanel()` membuat panel demo.
- Dashboard, animasi muat data: karena tidak ada backend, `render()` mensimulasikan fetch. Pindah halaman memuat penuh dengan skeleton yang mengikuti bentuk komponen; ganti query (filter, tab) memuat ulang bagian data saja (tabel dan grafik meredup, garis progres di header). Setelah data siap, tiap komponen punya animasi masuk sendiri: kartu naik bergantian, angka statistik menghitung naik, batang kolom tumbuh, garis tergambar, donut menyapu, baris tabel berurutan, progress bar terisi. Render karena state lokal (pilih baris, hover) tidak memicu animasi. Semua dimatikan bila `prefers-reduced-motion` aktif. Kode di `FX`, `fxBegin`, `fxAnimate`, `countUp`.
- Dashboard: router hash (`#/penjualan?tab=klien`), `ROUTES`, `ENT` (definisi CRUD generik: `openForm`, `saveForm`, `delAsk`), `dataTable`, `openPanel`, `card`, chart SVG buatan sendiri (`columnChart`, `donut`, `lineChart`).
- Preview di browser pane Claude memakai snapshot file sehingga navigasi hash tidak jalan; gunakan server lokal.
- Skill WIT (`wit-code-agent-2`) berasal dari `wit-skills.zip` milik tim dan tidak disertakan di sini.
