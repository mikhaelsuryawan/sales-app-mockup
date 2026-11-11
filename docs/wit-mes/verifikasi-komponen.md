# Verifikasi komponen: kit `packages/ui` vs `dashboard/index.html`

Diperiksa 11 Nov 2026 terhadap keadaan file saat ini (4121 baris), baris K-01 s.d. K-227 dari [audit-komponen.md](audit-komponen.md). Hanya membaca kode; tidak ada yang diubah. Nomor baris merujuk ke `dashboard/index.html`.

## 1. Ringkasan

| Status | Jumlah |
|---|---|
| Sudah | 116 |
| Sebagian | 91 |
| Belum | 11 |
| N/A | 9 |
| Total | 227 |

Aturan penilaian: Sudah bila CSS atau helper ada dan dipakai di tempat dashboard membutuhkannya. Helper atau varian yang didefinisikan tetapi tidak pernah dipanggil dihitung Sebagian. Baris Tidak berlaku tetap N/A. Baris Adaptasi dinilai terhadap bentuk adaptasinya (aksen tetap biru SIM, merah hanya untuk bahaya).

Pola umum: kit CSS dan helper hampir lengkap, tetapi sebagian besar halaman masih memakai markup lama yang ditulis tangan. Ada 25 helper yang tidak pernah dipanggil (bagian 5), jadi banyak baris berhenti di Sebagian.

Temuan di luar daftar baris: pada K-128, class `skip` pada `li` stepper bentrok dengan `.skip` tautan lewati (l.114: `position:absolute;left:-999px`). Pada pesanan berstatus Dibatalkan, semua langkah terlempar keluar layar. Perlu mengganti nama class di `.stepper li.skip` (l.374) dan di `stepper()` (l.958).

## 2. Baris Sebagian dan Belum

| ID | Komponen | Yang kurang | Bukti | Status |
|---|---|---|---|---|
| K-03 | Focus ring | Cincin global masih outline 2px accent, bukan box-shadow 0 0 0 2px rgb(10 146 242/.4); hanya .cc-hit dan .lcs memakai warna .4 | :focus-visible l.64, l.641, l.679 | Sebagian |
| K-07 | Overlay | Fade-in saja; closeLayerCore menghapus overlay seketika, tanpa fade-out | .overlay l.733 (.5 + blur 2px), closeLayerCore l.1016 | Sebagian |
| K-11 | Roving tabindex | rovingKey menangani role=tab/radio dan tab pills/utabs punya tabindex, tetapi helper radiogroup (segmented, choiceGrid) tidak pernah dipanggil; radio di form masih radio native dalam label .fchip | rovingKey l.1060, segmented l.956 (tidak dipakai), l.2094, l.2097 | Sebagian |
| K-24 | Tombol soft | .b-soft didefinisikan tetapi tidak ada pemanggil yang memakai v:'soft' | .b-soft l.249 | Sebagian |
| K-32 | loading | btn() punya opsi loading (spinner, disabled, aria-busy) tetapi tidak ada pemanggil yang mengirim loading | btn l.943 | Sebagian |
| K-33 | Tautan bergaya tombol | btn() punya opsi href yang tidak dipakai; tautan masih ditulis tangan sebagai <a class="btn b-onpop sm"> | btn l.943, l.1936 | Sebagian |
| K-41 | CardTitle | Judul menambah ikon aksen (.ch h2 svg) yang tidak ada di kit | .ch h2 svg l.275, card icon l.964 | Sebagian |
| K-43 | CardContent / CardFooter | card() tidak punya slot footer; totalFooter dan splitStats yang bisa jadi footer tidak dipakai | card l.964 | Sebagian |
| K-47 | IconTile tone | iconTile punya semua tone, tetapi stat di halaman tidak punya tile; statIcon memilih tone dari indeks, bukan status | iconTile l.938, statIcon l.988, dipakai l.1942-1943 | Sebagian |
| K-49 | IconTile round | .ic.round ada, satu-satunya pemanggil adalah settingsList yang tidak dipakai | .ic.round l.285, settingsList l.983 | Sebagian |
| K-50 | Number tile | iconTile(num) dan .ic.num ada tetapi tidak ada pemanggil; peringkat Top sales memakai angka polos | .ic.num l.288, l.1957 | Sebagian |
| K-51 | Tone bersama | STATUS_TONE hanya dijangkau lewat toneOf di dua panel; halaman memakai peta lokal STTONE dan ORD_TONE; statusBadge tidak dipakai | STATUS_TONE l.925, toneOf l.930, l.2471, l.3237, STTONE l.1289, ORD_TONE l.2033 | Sebagian |
| K-52 | StatCard layout | CSS sudah sesuai tetapi statCard() tidak dipanggil; 28 markup .stat ditulis tangan, hanya 2 di Overview yang punya tile ikon | statCard l.987, .stat l.295, l.1942-1943, l.2076, l.2145 | Sebagian |
| K-54 | Tone mengikuti angka | .stat.tn-* dipakai di Leads dan Klaim; Absensi menghitung tn lalu tidak memakainya, halaman stat lain tanpa tone | .stat.tn-* l.309, l.2399, l.3229, l.2145 | Sebagian |
| K-65 | Badge outline | .t-outline hanya terjangkau lewat toneOf di panel leads (status Baru) dan panel klaim; tidak ada pemanggil tag langsung | .t-outline l.386, STATUS_TONE l.929 | Sebagian |
| K-66 | Badge ink | .t-ink tidak dipakai di mana pun | .t-ink l.386 | Sebagian |
| K-67 | Peta status ke varian | Satu peta STATUS_TONE ada tetapi halaman masih memilih tone sendiri-sendiri | STATUS_TONE l.925, STTONE l.1289 | Sebagian |
| K-68 | Dot dan prioritas | statusDot tidak dipakai; spill memberi dot ke semua status, bukan hanya status hidup atau terblokir; tidak ada pemetaan prioritas | spill l.934, statusDot l.937 | Sebagian |
| K-71 | CountBadge | .nl .cnt 20px/11px menggantikan ukuran kit 18px/10px; sisanya (ring, 99+, sembunyi di 0) sesuai | .cnt l.388, .nl .cnt l.141, countBadge l.936 | Sebagian |
| K-73 | StatusDot | statusDot() tidak dipanggil; titik status lama .dotc masih dipakai di 7 tempat | .sdot l.391, statusDot l.937 | Sebagian |
| K-77 | Input soft | .inp.soft ada, tidak ada pemanggil | .inp.soft l.413 | Sebagian |
| K-80 | Slot kanan | .unit hanya dipakai satu kali (Harga Rp); tidak ada slot hapus atau satuan lain | .inp .unit l.422, l.2100 | Sebagian |
| K-81 | Ukuran input | .inp.lg hanya di login; kuantitas pesanan belum h48 18px tabular | .inp.lg l.417, l.1882-1883 | Sebagian |
| K-82 | Invalid | Gaya invalid ada; aria-invalid hanya diset di form master data, form pesanan dan penjualan memakai .ferr polos | l.418-419, showFormErrs l.2978, l.2057 | Sebagian |
| K-84 | Textarea soft + short | .ta.short dipakai, .ta.soft tidak punya pemanggil | textarea.ta.soft l.481 | Sebagian |
| K-85 | NativeSelect default | Gaya .sel select ada tetapi tidak pernah tampil karena semua select diganti enhancer (tidak ada pengecualian enum kecil) | .sel select l.424, enhance l.4041 | Sebagian |
| K-86 | NativeSelect soft | .sel.soft ada, tidak ada pemanggil | .sel.soft .dd-btn l.435 | Sebagian |
| K-87 | NativeSelect inline | .sel.inline ada, tidak ada pemanggil (baris per halaman memakai cls sm 36px) | .sel.inline l.438, dataTable pager l.1631 | Sebagian |
| K-88 | Placeholder select | Teks muted ada; opsi placeholder nonaktif saat wajib tidak ada | .dd-t.ph l.430, paint l.4040 | Sebagian |
| K-90 | Pesan dan wiring FormField | aria-describedby dan aria-invalid hanya di form master data; form lain tanpa wiring; formField() tidak dipanggil | .ferr l.407, fieldHTML l.2925-2932, formField l.973 | Sebagian |
| K-91 | Aturan combobox | Semua select dijadikan combobox berpencarian; enum enam atau kurang tidak tetap native | enhance l.4041 | Sebagian |
| K-92 | API combobox | Enhancer membaca data-desc dan data-reason tetapi tidak ada opsi yang memakainya; tanpa ikon, clearable, create | all() l.4066 | Sebagian |
| K-94 | Trigger soft | .sel.soft .dd-btn tidak punya pemanggil | l.435 | Sebagian |
| K-95 | Trigger inline | .sel.inline .dd-btn tidak punya pemanggil | l.438 | Sebagian |
| K-96 | Chip pada trigger multi | Select multiple dilewati enhancer | l.4041 | Belum |
| K-100 | Daftar dan baris | Opsi terpilih dicetak tebal 600, kit hanya memakai tanda centang | .dd-opt[aria-selected=true] l.449 | Sebagian |
| K-101 | Baris nonaktif | Mekanisme alasan dan lewati panah ada, tetapi tidak ada opsi yang diberi data-reason | .dd-opt[aria-disabled] l.454, enabledFrom l.4069 | Sebagian |
| K-103 | Baris hapus pilihan | Hanya ada CSS .dd-opt.clear, tidak ada kode yang membuat baris itu | .dd-opt.clear l.455 | Belum |
| K-104 | Baris buat baru | Tidak ada | - | Belum |
| K-105 | Footer multi | Tidak ada | - | Belum |
| K-108 | MultiCombobox | Select multiple dilewati | l.4041 | Belum |
| K-110 | Switch sm | .switch.sm dan opsi sm di swc ada, tidak ada pemanggil yang memakai sm | .switch.sm l.487 | Sebagian |
| K-112 | SegmentedControl | segmented() tidak dipanggil; tema memakai pills (role tab), pilihan sumber memakai .fchip radio | segmented l.956, l.3752, l.2094 | Sebagian |
| K-113 | Segmented sm/touch | CSS .seg.sm/.touch ada, helper tidak dipakai | .seg.sm l.356 | Sebagian |
| K-114 | Radiogroup roving | Hanya kolom grafik (cc-cols) yang memakai role radio dengan roving; radio form tetap native di .fchip | l.1531-1534, l.2097 | Sebagian |
| K-115 | Choice grid | choiceGrid() tidak dipanggil | choiceGrid l.957, .choice l.491 | Sebagian |
| K-118 | PillTabs sm | .pills.sm ada, tidak ada pemanggil yang mengirim size sm | .pills.sm l.342 | Sebagian |
| K-122 | Chip dasar | chip() tidak dipanggil, 21 chip ditulis tangan; tidak ada gaya disabled untuk .fchip | .fchip l.360, chip l.954 | Sebagian |
| K-125 | Hitungan chip + Semua | Pil hitungan .n dan klik-kedua-menghapus ada, tetapi hitungan hanya muncul di sebagian chip tulis tangan | .fchip .n l.343, handler l.1814 | Sebagian |
| K-126 | ChipRow | .chips.nowrap dan chipRow() tidak dipakai; semua .chips membungkus | .chips.nowrap l.358, chipRow l.955 | Sebagian |
| K-127 | Steps horizontal | Stepper dipakai satu kali (panel pesanan); tanpa pudar kanan 40px; panel leads masih .steps vertikal | .stepper l.366, l.2037, l.2474 | Sebagian |
| K-128 | Status step | Status skipped memakai class "skip" yang bentrok dengan .skip tautan lewati (position absolute, left -999px), jadi semua langkah pesanan dibatalkan terlempar keluar layar | .stepper li.skip l.374, .skip l.114 | Sebagian |
| K-130 | Penempatan dan penurunan steps | Diturunkan dari array di panel pesanan, tetapi tidak dalam kartu p20; aturan insentif masih .steps bukan daftar cek | l.2037, l.2368 | Sebagian |
| K-131 | ProgressBar | progressBar() dipakai lewat barList saja; 12 bar mentah (hero Overview, Top sales, achBar, shareBar) tanpa ARIA; isi default ink hanya di jalur helper | progressBar l.960, achBar l.1413, shareBar l.1414, l.1934 | Sebagian |
| K-132 | Caption progress | .bar-cap ada, caption masih di samping bar di achBar/shareBar | .bar-cap l.566, l.1413 | Sebagian |
| K-133 | SegmentBar + legenda | segmentBar() tidak dipanggil; donut dan funnel tetap | segmentBar l.962, .segbar l.567 | Sebagian |
| K-136 | Ring avatar | .av.ring dan opsi ring ada, tidak ada pemanggil | .av.ring l.397 | Sebagian |
| K-137 | AvatarStack | avatarStack() tidak dipanggil | avatarStack l.923, .avs l.398 | Sebagian |
| K-139 | Header sortable | Mesin sort dan aria-sort ada, tetapi tidak ada kolom di halaman mana pun yang mengirim sort, jadi tombol tidak pernah muncul | th() l.1626, tbSort l.1635 | Sebagian |
| K-143 | pageSize/reset/restore | Default 10 bukan 12; tidak ada opsi tanpa paginasi (0); reset dan pemulihan ada | dataTable l.1616-1618 | Sebagian |
| K-144 | Konvensi kolom pertama | Kolom prim berisi nama dan jabatan, bukan id mono 12px di atas nama | .mono l.66, l.2148 | Sebagian |
| K-145 | hideBelow | Kelas hb-xl/lg/md dan prop hideBelow ada, tidak ada kolom yang memakainya; ponsel hanya empat kolom pertama | .tbl th.hb-xl l.624, hc l.1625 | Sebagian |
| K-146 | Angka, satuan, batas, dash | .bad-n/.warn-n didefinisikan tetapi tidak dipakai di mana pun; tanda en dash dipakai | .bad-n/.warn-n l.556 | Sebagian |
| K-147 | Kolom meter | achBar/shareBar memakai bar mentah dengan caption di samping, tanpa ARIA, warna inline | achBar l.1413, shareBar l.1414 | Sebagian |
| K-148 | Sel tren | Tidak ada sparkline di sel tabel | - | Belum |
| K-149 | Tint baris menua | .tbl tr.aging dan parameter rowCls ada, tidak ada pemanggil yang mengirim rowCls | .aging l.547, rowCls l.1615 | Sebagian |
| K-151 | Urutan awal | Tidak ada tabel yang mengirim sort awal | dataTable l.1615 | Belum |
| K-153 | KeyValue | keyValue() dan .kvl tidak dipanggil; panel memakai tile .kv | keyValue l.970, .kvl l.607 | Sebagian |
| K-161 | Banner success | .banner.success ada, tidak ada pemanggil dengan tone success | .banner.success l.509 | Sebagian |
| K-165 | SplitStats | splitStats() tidak dipanggil | splitStats l.971, .split l.525 | Sebagian |
| K-169 | Header/footer dialog | .dlg-f/.dlg-d hanya di 4 modal; sisanya memakai footer flex inline dan tidak dibalik di ponsel | .dlg-f l.749, l.2125-2126, l.2219 | Sebagian |
| K-170 | Perilaku dialog | Focus trap, Esc, kembalikan fokus, aria-labelledby, kunci scroll dan inert ada; aria-describedby tidak pernah diset | trapTab l.1073, syncLayer l.1004, openModal l.1009 | Sebagian |
| K-171 | Dialog entitas | Form master data membangun ulang dan menampilkan error setelah submit pertama; form pesanan, penjualan, kunjungan memvalidasi saat klik saja | FORM.tried l.2964, ordSave l.2059 | Sebagian |
| K-172 | Layout form dialog | .g2 jatuh ke satu kolom di bawah 1024, kit dua kolom dari 640; gap 10-14 bukan 16 | .g2 l.267, media l.806, l.2054 | Sebagian |
| K-173 | Deskripsi menyebut aturan | Field terkunci tampil sebagai tile read-only di form master, tetapi deskripsi aturan tidak konsisten di semua dialog | fieldHTML l.2928 | Sebagian |
| K-176 | Chip multi dengan centang | Chip aktif tanpa ikon centang; chip() menerima icon tetapi tidak dipanggil | multiTog l.2923 | Belum |
| K-177 | Picker bergantung | Tidak ada "Pilih ... dulu" atau pengosongan anak | l.2446 (hanya komentar) | Belum |
| K-178 | Preview/catatan penghalang | djNote memberi banner info lalu warning, tetapi tidak menonaktifkan submit dan memakai tone warning, bukan danger | djNote l.2106-2109 | Sebagian |
| K-179 | Kuantitas | inputmode numeric dipakai, kit meminta decimal dan satuan di petunjuk | l.2056, l.2093 | Sebagian |
| K-180 | Satu dialog beberapa kata kerja | Setujui dan Tolak berbagi confirmAsk tetapi fungsi terpisah, tanpa tabel salinan | approveAsk l.2234, rejectModal l.2245 | Sebagian |
| K-181 | Footer dialog | "Batal" masih v:'card' di 24 tempat; hanya confirmAsk memakai outline | btn('Batal',{v:'card'}) l.2058, l.2126 | Sebagian |
| K-182 | ConfirmDialog | confirmAsk (alertdialog, 448) dipakai 4 kali; modal hapus tulis tangan (djDelAsk, visDel, cbd, pengumuman) masih role dialog | confirmAsk l.1019, djDelAsk l.2126 | Sebagian |
| K-183 | destructive | Konfirmasi lewat confirmAsk memakai .b-danger-solid; modal hapus tulis tangan memakai v:'danger' soft | confirmAsk l.1020, l.2126, l.2219 | Sebagian |
| K-193 | Semua menu ponsel jadi sheet | openPop, dd-pop, bel menjadi sheet, tetapi panel hasil pencarian global (.gs-panel) tetap dropdown di ponsel | openPop l.1029, .gs-panel l.175 | Sebagian |
| K-196 | Pemicu overflow rekaman | actionMenu() tidak dipanggil; panel rincian tidak punya "Aksi lain" | actionMenu l.1043 | Sebagian |
| K-197 | Perilaku menu | role menu/menuitem, haspopup+expanded, panah, Home/End, Esc kembali ke pemicu, fokus masuk ada; typeahead tidak ada | menuKey l.1067, openPop l.1025 | Sebagian |
| K-200 | Toaster | Tumpukan, posisi ponsel, maksimal 3, aria-live sesuai; tanpa role=region dan aria-label | .toasts l.772, html l.847 | Sebagian |
| K-202 | Salinan dan tone toast | Hapus lewat act memakai default, tetapi djDel, visDel, cbdDel, posmDel, mitraQDel masih toast sukses dengan tanda centang | toast l.991, l.2127, l.2220, l.3329 | Sebagian |
| K-213 | Baris daftar bersarang | listRow() tidak dipanggil; 35 .rowlink ditulis tangan tanpa .lr .t/.m dan kolom kanan | listRow l.975, .rowlink l.571, l.1957 | Sebagian |
| K-214 | Baris dengan aksi sendiri | Tidak ada | - | Belum |
| K-215 | Sequence tile | .seqt hanya dirender oleh listRow/rankedRow yang tidak dipakai | .seqt l.581 | Sebagian |
| K-216 | State terpilih | .stat.on ring ada; tidak ada baris picker ink atau sorotan deep-link | .stat.on l.299 | Sebagian |
| K-217 | Baris peringkat | rankedRow() tidak dipanggil; Top sales tulis tangan | rankedRow l.976, l.1957 | Sebagian |
| K-218 | Baris riwayat | historyRow dipakai di 3 panel lewat historyBlock; log aktivitas dan riwayat approval belum | historyRow l.977, historyBlock l.1895 | Sebagian |
| K-219 | Daftar cek | checkList() tidak dipanggil; aturan insentif masih .steps vertikal | checkList l.978, l.2368 | Sebagian |
| K-220 | Chip tautan | entityChip, codeLink, personChip tidak dipanggil | l.979-981 | Sebagian |
| K-221 | Timeline revisi | .steps vertikal dipakai di leads dan insentif, tanpa ring 4px dan badge Terkini | .steps l.785-790, l.2474 | Sebagian |
| K-222 | Tree row | Tidak ada pohon; hanya chip drill | - | Belum |
| K-223 | Flow chain | flowChain() tidak dipanggil; funnel bar tetap | flowChain l.982, .funnel l.697 | Sebagian |
| K-225 | Tile bacaan | readingTile() tidak dipanggil; tile .kv menutup tampilan tetapi tanpa "n/a" saat kosong | readingTile l.986, .kv l.612 | Sebagian |
| K-226 | Daftar pengaturan | settingsList() tidak dipanggil; Pengaturan masih kartu + .rowlink | settingsList l.983, l.3752 | Sebagian |
| K-227 | Footer total | totalFooter() tidak dipanggil; form dan panel pesanan tanpa footer total | totalFooter l.984, .totf l.605 | Sebagian |

## 3. Baris Sudah

K-02, K-04, K-05, K-06, K-08, K-09, K-10, K-12, K-16, K-17, K-18, K-19, K-20, K-21, K-22, K-23, K-25, K-26, K-27, K-28, K-29, K-30, K-31, K-35, K-36, K-37, K-38, K-39, K-40, K-42, K-44, K-45, K-46, K-48, K-53, K-55, K-56, K-57, K-58, K-59, K-60, K-61, K-62, K-63, K-64, K-70, K-72, K-75, K-76, K-78, K-79, K-83, K-89, K-93, K-97, K-98, K-99, K-102, K-106, K-107, K-109, K-111, K-116, K-117, K-119, K-121, K-123, K-124, K-129, K-134, K-135, K-138, K-140, K-141, K-142, K-150, K-152, K-154, K-155, K-156, K-157, K-158, K-159, K-160, K-162, K-163, K-164, K-166, K-167, K-168, K-174, K-175, K-184, K-185, K-186, K-187, K-188, K-189, K-190, K-191, K-192, K-194, K-195, K-198, K-199, K-201, K-203, K-205, K-206, K-207, K-208, K-209, K-210, K-211, K-212, K-224

## 4. Baris N/A

- K-01 (cn() merge): Tanpa Tailwind; helper menyusun class string manual
- K-13 (useElementSize): Ukuran dasar diganti ResizeObserver di grafik (chRO)
- K-14 (useLazyList): Diganti tombol "N lagi · Muat" (tbMore l.1634)
- K-15 (Barrel index.ts): Satu file
- K-34 (Ukuran operator h-14): Khusus layar operator
- K-69 (Badge Released terkunci): Tidak ada dokumen terkunci
- K-74 (StatusText): Khusus aplikasi operator
- K-120 (SegmentedTabs urgent): Khusus tab operator; dashboard memakai PillTabs di ponsel
- K-204 (SignaturePad): Tidak ada alur tanda tangan

Adaptasi N/A sudah diterapkan: K-13 lewat ResizeObserver grafik (`chRO`, l.1556), K-14 lewat tombol "N lagi · Muat" (`tbMore`, l.1634), K-120 dengan PillTabs di ponsel.

## 5. Helper dan varian yang didefinisikan tetapi tidak pernah dipanggil

Helper (dicek dengan menelusuri semua rujukan di luar definisinya):

| Helper | Definisi | Catatan |
|---|---|---|
| `chip(), chipRow()` | l.954-955 | Chip ditulis tangan (21 `.fchip`); tidak ada `.chips.nowrap` |
| `segmented(), choiceGrid()` | l.956-957 | Tema memakai `pills`, pilihan sumber memakai `.fchip` radio native |
| `segmentBar()` | l.962 | Donut dan funnel tetap dipakai |
| `keyValue()` | l.970 | Panel rincian memakai `.kv` |
| `splitStats()` | l.971 |  |
| `formField()` | l.973 | Form menulis `.field` sendiri |
| `listRow(), rankedRow()` | l.975-976 | Baris `.rowlink` ditulis tangan |
| `checkList()` | l.978 | Aturan insentif masih `.steps` |
| `entityChip(), codeLink(), personChip()` | l.979-981 |  |
| `flowChain()` | l.982 | Funnel leads masih bar |
| `settingsList()` | l.983 | Pengaturan masih kartu |
| `totalFooter()` | l.984 | Form dan panel pesanan tanpa footer total |
| `readingTile()` | l.986 | Tile `.kv` dipakai |
| `statCard()` | l.987 | 28 `.stat` ditulis tangan |
| `avatarStack()` | l.923 |  |
| `badge()` | l.933 | Alias `tag`, tidak dipanggil |
| `statusBadge()` | l.935 | Pemanggil memakai `spill(..., toneOf())` atau peta lokal |
| `statusDot()` | l.937 | Titik lama `.dotc` masih dipakai |
| `toneNum()` | l.941 |  |
| `actionMenu() beserta openMenuOf(), MENUS, ID_MENU` | l.1043-1045 | Tidak ada panel rincian yang memakai "Aksi lain" |
| `POPS (konstanta)` | l.914 | Sisa palet lama, tidak dirujuk |

Opsi dan varian yang ada tetapi tidak ada pemanggil yang memakainya:

| Opsi atau varian | Lokasi |
|---|---|
| btn({loading}) | l.943 |
| btn({href}) | l.943 |
| iconTile({round}) dan iconTile({num}) | l.938 (round hanya dipanggil settingsList yang tidak dipakai) |
| av({ring}) | l.922 |
| swc({sm}) | l.959 |
| pills({size:'sm'}) | l.952 |
| dataTable({sort}) dan kolom `sort` | l.1626, tidak ada kolom yang mengirimnya |
| dataTable kolom `hideBelow` | l.1625 |
| dataTable({rowCls}) | l.1615 |
| Varian CSS tanpa pemanggil: `.b-soft`, `.inp.soft`, `.ta.soft`, `.sel.soft`, `.sel.inline`, `.t-ink`, `.banner.success`, `.bad-n`/`.warn-n`, `.boolrow` | l.249, 413, 481, 435, 438, 386, 509, 556, 489 |
