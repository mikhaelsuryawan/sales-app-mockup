# Verifikasi visual dan grafik

Pemeriksaan ulang `dashboard/index.html` (4121 baris) terhadap baris V-01 s.d. V-89 dan C-01 s.d. C-54 di [audit-visual-grafik.md](audit-visual-grafik.md). Hanya baca; tidak ada kode yang diubah. Nomor baris mengacu pada kondisi file saat diperiksa.

## Ringkasan

| Status | Jumlah |
|---|---|
| Sudah | 110 |
| Sebagian | 23 |
| Belum | 5 |
| N/A | 5 |
| Total | 143 (V 89, C 54) |

Rincian: V = 75 Sudah, 8 Sebagian, 3 Belum, 3 N/A. C = 35 Sudah, 15 Sebagian, 2 Belum, 2 N/A.

## Baris Sebagian dan Belum

| ID | Syarat | Yang kurang | Bukti |
|---|---|---|---|
| V-30 | Tanpa abu karangan | Nama lama `--void`, `--control`, `--hover`, `--line-strong`, `--subtle` masih ada sebagai alias ke tangga kit dan masih dipakai di tiga tempat. Masih ada heksa di luar tangga: `#DDEBF6` dan `#1A2633` (peta), palet avatar sepuluh warna. | `:root` L20; pemakai L834, L2305, L2379; `.mapbox` L791-792; `AV_COLORS` L921 |
| V-31 | Satu aksen, satu ink, sisanya abu | Slot grafik dan warna status sudah dalam palet (D2), tetapi `--pop-tan/sky/mist/blue` masih didefinisikan (hanya `.login-art` mati yang memakai), avatar memakai sepuluh rona, peta berlatar biru. | `--pop-*` L25, L47; `.login-art` L796 (tidak ada markup yang memakainya); `AV_COLORS` L921 |
| V-44 | Judul halaman 24px/700 | Tidak ada judul 24px di dalam halaman. Judul pindah ke header 18px (V-45), dan `pageHead` hanya merender deskripsi dan aksi. | `h1.pt` L167; `.pagehead` L522; `pageHead` L972 |
| V-58 | Semua angka `tabular-nums` | Dipakai luas (116 kemunculan `tabular`), tetapi tidak ada aturan dasar. `.tbl td` L543, `.pager` L557, dan `.kvl dd` L611 bergantung pada kelas yang dipasang pemanggil. | `.tabular` L65; `.tbl td` L543; `.pager` L557 |
| V-59 | Unit di samping angka: rata atas di stat, rata bawah di hero | Hero di ink benar (`metricOnInk`: flex-end, gap 6, unit pb 2). Unit stat masih sebaris dengan `margin-left:4px`, bukan rata atas dengan pt 6px. `.hero-unit` tidak dipakai. Stat Overview memakai unit inline 16px. `statCard` tidak punya pemanggil. | `.stat b .u` L304; `.hero-unit` L711; `metricOnInk` L985; Overview L1942; `statCard` L987 |
| V-76 | Padding kartu: 24 untuk masuk, 32 untuk kartu brand | Kartu form login memakai 32, bukan 24. CSS login lama (`.login-form .card` 28px) masih ada sebagai kode mati. | `.lg-form` L324; `.login-form .card` L800 |
| V-78 | Ikon 16 di kontrol, 20 di navigasi | Navigasi 20 dan ikon dalam tombol dipaksa 16 lewat CSS. Banyak ikon di luar tombol masih 15 atau 18: judul kartu, baris tautan, kartu callout. | `.btn svg` L232; `I(SEC_ICON[sec],20)` L1693; `card()` L964; L1936, L1958, L2008 |
| V-79 | IconTile: nada lembut menurut status | `statIcon` memilih nada menurut indeks, bukan status. `POPS` (tile solid menurut indeks) masih dipakai di baris promo. | `statIcon` L988; `POPS` L914; pemakai L2009; Overview L1942-1943 |
| V-82 | `animate-pulse` hanya untuk titik hidup | Kelas `.pulse` dan opsi `pulse` di `statusDot` ada, tetapi tidak ada pemanggil yang menyalakannya. `statusDot` sendiri tidak punya pemanggil. Tidak ada titik hidup yang berdenyut di papan langsung. | `.pulse` L106; `.sdot.pulse` L392; `statusDot` L937 |
| V-83 | Hanya lebar rail yang bergerak | Hitung naik dan batang tumbuh sudah hilang. Masih ada angkat dan skala saat hover pada tombol Buat di rail yang diciutkan. | `.collapsed .ra:hover{transform:translateY(-1px) scale(1.05)}` L128 |
| V-87 | Catatan di kartu ink (risiko danger/20, jeda warning/20, terkunci white/10) | Tidak ada varian catatan. `.ink-panel` hanya satu netral white/.06 dan dipakai sekali. | `.ink-panel` L712; pemakai L1933 |
| C-04 | Sorot menurut aturan, kolom polos ink, nilai di atas kolom | Nilai dicetak di atas kolom (benar). Sorotan menurut pilihan (`sel`), bukan menurut aturan. Kolom lain abu `chart-muted`, tidak ada pilihan ink. | `columnChart` L1522-1539; `.cc-col` L642-644; `.cc-val` L645 |
| C-05 | Nada kolom ink, info, chart-muted | `columnChart` tidak punya parameter nada. | `columnChart` L1522 |
| C-07 | Garis atau sparkline satu seri, titik akhir aksen hanya untuk baris berisiko | Area 10% dan seri tunggal ink sudah. `highlightLast` diterima tetapi tidak dibaca `renderLine`. Titik akhir selalu berwarna seri. Satu grafik (Pengajuan vs deal) memuat tiga seri. | `sparkline` L1560; `renderLine` L1579-1581; L2405 |
| C-09 | Urutan part-to-whole: ink, info, aksen, warning, success, abu | Palet sudah dibatasi enam slot, tetapi urutan tidak ditegakkan. Pemanggil memilih slot menurut makna (donut kehadiran dimulai dari success). | `donut` pemanggil L1955, L2007, L2155, L2407; `sc()` L1519 |
| C-10 | Ramp empat pita: success, info, warning, danger | Tidak ada grafik pita empat tingkat. Meter skor hanya ink, warning, danger. | `meter` L1609 |
| C-14 | Domain tetap (persen 0-100) | Mesin menerima `cfg.domain`, tetapi tidak ada grafik yang memakainya, jadi belum terbukti. | `renderLine` L1567; tidak ada pemanggil `domain:` |
| C-21 | Zona di grafik garis 8% | Mesin menggambar zona, tetapi tidak ada pemanggil yang mengirim `zones`. | `renderLine` L1574; tidak ada pemanggil `zones:` |
| C-22 | Kolom: ujung 4px, maks 24px, nilai negatif tumbuh ke bawah | Ujung 4px, lebar 24px, jarak 2px sudah. Nilai negatif tidak didukung (skala mulai 0, tinggi memakai `value/top`). | `.cc-col` L642; `.cc-cols` L638; `columnChart` L1524, L1532 |
| C-29 | Legenda donut digambar pemanggil | Legenda masih ditanam di helper `donut`. | `donut` L1551 (`.dleg`) |
| C-30 | Tooltip HTML untuk semua grafik | Kolom dan garis sudah HTML, menjepit ke dalam plot, nilai lebih dulu. Donut hanya `<title>` bawaan. Tooltip kolom 6px di atas dan tidak berbalik ke bawah (syarat 10px). | `.cc-tip` L647-648; `lc-tip` L1590-1592; donut L1547 |
| C-33 | Roving fokus: satu tanda di urutan tab, panah, Home dan End | Kolom sudah roving dengan panah, Home, End (tetapi ikut mengubah pilihan). Segmen donut tidak fokus; tombol legenda semua masuk urutan tab. | `ccKey` L1540; `circle.hit` L1547; `.dleg button` L1551 |
| C-46 | SegmentBar dan ProgressBar dengan nada ambang | `segmentBar` dan `progressBar` ada. Nada `progressBar` dipilih pemanggil, dan `achBar` memakai success pada 100%, aksen pada 70%, warning di bawahnya, bukan ink, warning di atas 80%, aksen di atas 100%. | `segmentBar` L962; `progressBar` L960; `achBar` L1413 |
| C-47 | Tab jendela waktu di aksi PageHeader | Tab Harian, Mingguan, Bulanan ada di dalam kartu Sell-out, bukan di PageHeader. | Overview L1954 (`acts` kartu); `pageHead` L972 |
| C-48 | Bar filter: label "group by" 12px/600 plus pil kecil, drill sebagai chip dengan X | `fchip` ada. Drill berbentuk chip breadcrumb tanpa X, dan `repFilters` tidak memakai label "group by". | `.fchip` L360; L2002; `repFilters` L1449 |
| C-49 | Grid KPI 4 kolom dari md, nada menurut ambang | Empat kolom baru dari 1280px (dua kolom di bawahnya, termasuk md). Nada hanya di satu halaman (absensi); `toneNum` tidak punya pemanggil dan tidak ada pemanggil `statCard`. | `.stats` L291-293, L808; `toneNum` L941; L2145 |
| C-50 | Grid deret waktu: 1 kolom, 2 kolom dari xl (1280) | `.g2` sudah dua kolom dari 1024px, bukan 1280px. | `.g2` L267; L808 |
| C-53 | Baris peringkat: chip urutan 36px, baris di surface-2 | `rankedRow` dan `.seqt.rk` ada, tetapi tidak ada pemanggil. Top sales di Overview memakai angka polos 16px. | `rankedRow` L976; `.seqt.rk` L582; Overview L1957 |

## Baris Sudah

V-01, V-02, V-03, V-04, V-05, V-06, V-07, V-08, V-09, V-10, V-12, V-13, V-14, V-15, V-16, V-17, V-18, V-19, V-20, V-21, V-22, V-23, V-24, V-25, V-26, V-27, V-28, V-29, V-32, V-33, V-34, V-35, V-36, V-37, V-38, V-39, V-40, V-41, V-42, V-43, V-45, V-47, V-48, V-49, V-50, V-51, V-52, V-53, V-54, V-55, V-56, V-57, V-60, V-61, V-62, V-63, V-64, V-65, V-66, V-67, V-68, V-69, V-70, V-71, V-72, V-73, V-74, V-75, V-80, V-81, V-84, V-85, V-86, V-88, V-89, C-01, C-02, C-03, C-06, C-08, C-12, C-13, C-15, C-16, C-17, C-18, C-19, C-20, C-23, C-24, C-25, C-26, C-27, C-28, C-31, C-32, C-34, C-35, C-36, C-37, C-38, C-39, C-40, C-41, C-42, C-43, C-44, C-45, C-51, C-52.

Catatan atas beberapa baris Sudah yang bersyarat:
- V-33: `linear-gradient` hanya tersisa untuk pudar tepi tab (L334, L623), skeleton (L82, L89), dan hatch; tidak ada isian gradient. `--accent-grad` sudah hilang.
- V-06: `theme-color` ikut tema saat awal (L9) dan saat ganti tema (`setTheme` L1706).
- C-02: grafik garis diukur dengan `ResizeObserver` dan digambar ulang pada lebar piksel (L1599-1606); kolom memakai tata letak flex pada lebar nyata dengan penipisan label lewat `thinColLabels` L1600. Donut tetap 148px sesuai syarat C-26.
- C-12: bila target kolom berbeda-beda, yang tergambar adalah tanda per kolom `.cc-cap` (L1533), bukan seri kedua; bila sama, garis acuan `.cc-ref` (L1530).
- C-20: label garis acuan rata kanan di grafik garis (L1575). Label di `.cc-ref` kolom rata kiri (L637); tetap dihitung Sudah karena garis acuan dipakai nyata oleh kolom Overview.
- C-36: gema `aria-live` ada di grafik garis (L1582) dan donut (L1550); kolom memakai label ARIA per tombol.
- C-43: `.kids` di stat Goal pinjaman memuat `barStrip` (L1943); jarak 12px ditambah gap 4px menjadi 16px.
- C-45: baris `barList` memicu `drillTo` dan chip breadcrumb muncul di kartu (L2002, L2004).

## Baris N/A

- V-11: tanpa build Tailwind; token ada di `:root`.
- V-46: judul layar ponsel 28px, khusus aplikasi operator.
- V-77: jarak aplikasi ponsel, khusus aplikasi operator.
- C-11: ramp alokasi stok MES, tidak ada alokasi di Sales App.
- C-54: pewarnaan varians MES, tidak ada layar padanan; audit sebelumnya sudah menandai "tidak diverifikasi / n/a".
