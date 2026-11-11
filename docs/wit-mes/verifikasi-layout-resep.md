# Verifikasi layout dan resep halaman (L-01..L-54, P-01..P-69, F-01..F-14)

Diperiksa ulang atas `dashboard/index.html` (4121 baris) pada 11 Nov 2026, hanya baca. Nomor baris menunjuk file itu. Selain membaca kode, dilakukan pengukuran di browser (render sinkron semua 53 rute): lebar 375 dan 768, `main.scrollWidth === clientWidth` dan tidak ada elemen melewati `innerWidth` di semua rute. Satu pengecualian di 375: tabel Gantt di `/absensi` (L2146) lebih lebar dari layar tetapi menggulir di dalam kontainernya sendiri, jadi `main` tidak meluap. Tema gelap dan lebar 1440 tidak diukur ulang.

## 1. Ringkasan

| Status | Jumlah | L | P | F |
|---|---|---|---|---|
| Sudah | 69 | 46 | 16 | 7 |
| Sebagian | 39 | 7 | 26 | 6 |
| Belum | 2 | 0 | 2 | 0 |
| N/A | 27 | 1 | 25 | 1 |
| Total | 137 | 54 | 69 | 14 |

Pola sisa yang paling besar, di luar baris yang diuraikan di bawah:

- Fase 6 baru menyentuh kerangka halaman daftar (kalimat tujuan, banner cakupan, tabel di kanvas, klik kedua chip). Isi tiap halaman daftar belum berubah: ubin stat masih `<button class="stat">` polos, `statCard()` (L987) tidak dipakai di halaman mana pun, `keyValue()` dan `rankedRow()` (L972, L976) juga tidak dipakai.
- Panel rincian baru diperbarui untuk pesanan (L2036), leads (L2470), dan klaim (L3236). Panel lain (promo L2318, penagihan L3214, approval L2258, usulan toko L3042, insentif, grading, absensi) masih memakai `.kv` dan `.tile`.
- Klaim di catatan fase 5 bahwa semua mutasi lewat `act()` belum sepenuhnya benar: `ordSave` (L2059), `djSave` (L2110), `visSave` (L2207), `approveToko` (L3049), dan tombol Tolak usulan (L3045) memutasi data langsung, tanpa konfirmasi atau alasan.

## 2. Baris Sebagian atau Belum

### Layout (L)

| ID | Syarat singkat | Yang kurang dan untuk rute mana | Bukti |
|---|---|---|---|
| L-05 | Ritme 16px antar blok halaman | Tidak ada pembungkus `space-y-4`. Jarak berasal dari `margin-bottom` per komponen (`.stats`, `.pagehead` 24px, `.banner.mb`) dan style inline per halaman, jadi tumpukan kartu di halaman tanpa `.stats` bergantung pada inline `margin-top:16px` (mis. L2152, L3233). Semua rute. | L291, L522, L620 |
| L-12 | Workspace: Keluar hanya di sini; baris punya sub muted | `Keluar` juga ada di `userPop` (L1765) dan footer sheet More (L1787). Baris workspace tidak punya teks sub muted dan ada tambahan "Aplikasi mobile sales". Semua rute. | `wsPop` L1766, `userPop` L1765 |
| L-14 | Satu sumber NAV untuk semua penanda navigasi | Rail, tab seksi, sheet More, judul header, dan hitungan sudah dari `NAV` dan `counts()`. Bar bawah menulis jalur, label, dan ikon langsung (`it('/','Overview','dash')` dst.), tidak diturunkan dari `NAV`. Semua rute di bawah 768. | `bottomBar` L1773-1778 |
| L-20 | Hasil pencarian terbaru di atas, 5 per grup | Grup, 5 per grup, semua kata cocok, panah/Enter/Esc, empty state sudah ada. Urutan "terbaru dulu" tidak diterapkan; hasil memakai urutan data (`searchAll` hanya `.slice(0,5)`). Semua rute. | `searchAll` L1722-1729 |
| L-30 | Pencarian, filter, sort, halaman disimpan per entri riwayat | Filter dan kueri ada di hash dan ikut `replaceState`, jadi aman saat Back. Halaman, per-halaman, dan seleksi tabel ada di satu objek global `ST.tb`/`ST.sel`, direset ke halaman 1 pada setiap PUSH (L1859) dan tidak disimpan per entri. Contoh: A (hal. 3) → B → A (reset ke 1) → Back ke B → Back ke A menampilkan halaman 1. Semua rute tabel. | L1859, `dataTable` L1615-1620 |
| L-44 | Tabel di ponsel: pengenal + satu angka; kolom `hideBelow` md/lg di tablet | Ponsel menampilkan 4 kolom pertama (aturan `i>=4` jadi `hide-m`, L1626) dan tiap kolom lain tampil sebagai baris "label: nilai" (L826-834). Penyembunyian bertahap di tablet (`hb-md`, `hb-lg`, L624-626) tidak dipakai: `hideBelow` hanya muncul sekali dan kosong. Semua rute dengan `dataTable`. | L1626, L624-626, L826-834 |
| L-45 | Status sebagai satu baris ringkas di bawah meta di ponsel | Di tabel, status tampil sebagai baris berlabel ("Status: ...") di antara kolom lain. Pola `.lr .ph` (L578) hanya ada di `listRow()` dan tidak dipakai tabel. Semua rute daftar. | L826-834, L578 |

### Resep halaman (P)

| ID | Syarat singkat | Yang kurang dan untuk rute mana | Bukti |
|---|---|---|---|
| P-01 | Caption "Semua klien · diperbarui 09:41" di Overview | Caption header `wsName · tanggal` ada dan tersembunyi di bawah 768, tetapi tidak memuat "diperbarui". Rute `/`. | L1847, L170 |
| P-05 | Ringkasan status per status | Tidak dikerjakan (opsional; dicatat plan fase 6). Rute `/`. | `pageOverview` L1908-1963 (tidak ada baris status) |
| P-06 | Daftar dan analisis `1fr 1fr 20rem` | Grid ada (L264). Top sales menampilkan 4 baris, bukan 5 (`slice(0,4)`, L1914); tidak ada empty state ringkas; kartu analisis tidak punya tautan ghost selebar kartu (hanya "Lihat semua" outline kecil di header); kartu Sell-out tetap span 2. Rute `/`. | L1908-1963 |
| P-09 | PageHeader: kalimat tujuan + aksi (pencarian + "Buat X") | Kalimat tujuan ada di semua rute daftar (`PAGE_DESC` L1803, `pageHead` L1856). Baris aksi di `pageHead` hanya berisi caption "Diperbarui"; pencarian dan tombol utama tetap di dalam kartu (`crudList` L2908, `pageLeads` L2390, `pageAbsensi` L2139, `pagePemesanan` L2128). Probe DOM: `.pagehead .pa` tidak berisi tombol atau input di rute mana pun. Semua rute daftar. | L1856, L2908 |
| P-11 | Pengalih tampilan dengan hitungan | `/toko`, `/task-pending`, `/iam`, `/produk`, `/grading` sudah ber-hitungan. `/penugasan` memakai chip "Per sales/Per toko" tanpa hitungan dan tab "Penempatan toko" tanpa hitungan (L3174, L3151); `/aktivitas` memakai pill tanpa hitungan (L4026). | L3151, L3174, L4026 |
| P-12 | Banner cakupan: info, atau warning bila rekaman hilang | Banner info "Daftar dipersempit" + "Tampilkan semua" muncul untuk `/leads?f=`, `/leads?q=`, `/absensi?q=`, `/approval?type=`, `/toko?q=`, `/grading?u=` (diuji di browser). Varian warning untuk rekaman yang tidak ada tidak ada. Nilai tampil mentah ("filter: unassigned", "sales: rina", kunci `type` tanpa label). | `scopeBanner` L1808, `SCOPE_LABEL` L1807 |
| P-13 | Ubin stat: nada mengikuti hitungan, klik memfilter, tile ganjil `col-span-2` | Ubin klik-filter ada di `/absensi`, `/leads`, `/pemesanan`, `/kunjungan`, `/klaim-biaya`, `/penagihan`. Nada mengikuti hitungan hanya di `/leads` (L2398) dan `/klaim-biaya` (L3229); sisanya tanpa nada, tanpa ikon. `/sinkron`, `/kepatuhan-kunjungan`, `/sell-in` memakai `<div class="stat">` yang tidak bisa diklik (L3299). Tile ganjil tidak `col-span-2` (mis. `/data-penjualan` 3 ubin). | L2398, L3229, L3299, L291 |
| P-14 | ChipRow dengan hitungan, klik kedua menghapus, picker data inline bisa dihapus | Hitungan ada di `/approval` (L2227), `/promo`, `crudList`, `/toko`. Tanpa hitungan: `/leads` (L2411), `/penagihan`, `/quiz`, `/survey`, `/penugasan`. Klik kedua menghapus hanya untuk chip bergaya `setQ({k:'v'})` lewat handler global (L1815). Filter data-backed masih `<select>` biasa, tanpa tombol hapus pilihan. | L1815, L2411 |
| P-15 | Tabel utama di kanvas tanpa kartu | `markTableCards` (L1816) hanya memakai daftar `LIST_CANVAS` (L1805). Probe DOM: `.tbl-card` ada di `/absensi`, `/kunjungan`, `/leads`, `/overdue`, `/pemesanan`, `/data-penjualan`, `/penagihan`, `/promo`, `/posm`, `/pengumuman`, `/toko`, `/task-pending`, `/klaim-biaya`. Tetap berkartu: `/penugasan`, `/quiz`, `/survey`, `/canvassing` (L3124, tabel dalam `g-side`). Reset halaman saat filter berubah sudah ada (`tbSig`, L1614-1620). | L1805, L1816 |
| P-16 | Pilihan tampilan penonton disimpan di storage | Kueri dan filter di URL (`setQ` L1093). Pilihan tampilan (mis. Per sales/Per toko, tab) tidak disimpan di storage; tidak dikerjakan (plan fase 6). | `setQ` L1093, L3151 |
| P-18 | Bilah aksi panel: langkah utama, outline, ActionMenu | `ordPanel` benar (utama "Tandai ...", "Ajukan batal" sudah outline, L2043). `actionMenu()` (L977) tidak dipakai di panel mana pun. `promoPanel` memakai tombol "Ubah" yang hanya toast "Perubahan diajukan" tanpa mengubah apa pun (L2323). `promoPanel`, `arPanel`, `usulanPanel`, `aprPanel` tidak punya langkah lifecycle. | L2043, L2323, L977 |
| P-19 | Catatan blocker dengan lead-in tebal | Hanya `ordPanel` ("Belum bisa dibatalkan:", L2039) dan `klaimPanel` ("Periksa bukti dulu:", L3239). `promoPanel` (L2321), `usulanPanel`, `arPanel` memakai `callout()` tanpa lead-in. | L2039, L3239, L2321 |
| P-20 | Hero ink di panel rekaman (pesanan, leads, insentif, grading, penagihan) | `panelHero` (L1893) dipakai di pesanan, leads, klaim saja. `incPanel` (L2366), `gradePanel` (L2384), `arPanel` (L3214), `promoPanel`, `trxPanel`, `attPanel`, `visPanel` tetap `.kv` + `.tile`. | L2037, L2471, L3237 |
| P-21 | Steps dari alur status di pesanan, leads, promo | Pesanan: `stepper` (L2037). Leads: `.steps` vertikal lama (L2473). Promo (Draft → Menunggu approval → Aktif): tidak ada Steps di `promoPanel`. | L2037, L2473, L2318 |
| P-22 | KeyValue, nilai kosong "Tidak ada"/"Belum" | `keyValue()` (L972) tidak dipakai; panel memakai `.kv` dan sel kosong tertulis "–" (`attPanel` L2158, `arPanel`, `usulanPanel`). | L972, L2158 |
| P-23 | Baris terkait, meter syarat, SplitStats | Baris item ada di `ordPanel` (L2038) dan riwayat bayar di `arPanel` (L3216). `splitStats()` (L971) tidak dipakai di panel; tidak ada meter syarat. | L2038, L3216, L971 |
| P-24 | Kartu riwayat per rekaman, terbaru dulu | `historyBlock` (L1895) hanya di `ordPanel` dan `klaimPanel`. Panel leads, promo, penagihan, usulan, absensi tidak punya riwayat per rekaman. | L1895, L2039, L3239 |
| P-26 | Empty state kartu untuk rute tak dikenal dan id hilang | Rute tak dikenal sudah benar (`pageNotFound` L1890 dengan judul, deskripsi, tombol kembali). Id hilang ditangani `panelMissing` (L1891) hanya di `trxPanel`, `ordPanel`, `leadPanel`, `klaimPanel`. `aprPanel` (L2258), `promoPanel`, `arPanel`, `usulanPanel`, `incPanel` langsung membaca properti dari `undefined`. | L1890-1891, L2258 |
| P-28 | EntityTable: header kartu dengan judul, deskripsi, pencarian `sm:w-56` + Tambah; Hapus nonaktif dengan alasan | Aksi ikon sudah (Ubah, Hapus), konfirmasi "Hapus {nama}?", soft delete, tooltip alasan, `aria-disabled`. Kurang: kartu tanpa deskripsi dan pencarian + chip duduk di badan kartu selebar penuh (bukan di header); tombol Hapus yang diblokir masih menerima klik dan membuka modal "Tidak bisa dihapus". Semua 19 rute master data. | `crudList` L2908-2919, `rowActs` L963, `delAsk` L2988 |
| P-38 | Satu ConfirmDialog untuk setujui dan tolak; tolak wajib catatan | `/approval` benar (`approveAsk` L2234, `rejectModal` L2245 wajib alasan). `/toko?tab=usulan`: Setujui (`approveToko` L3049) langsung tanpa konfirmasi dan Tolak (L3045) tanpa alasan. `/klaim-biaya`: `klaimSet` (L3241) langsung tanpa konfirmasi dan alasan tolak. `/task-pending` tidak diubah. | L3045, L3049, L3241 |
| P-49 | Picker di aksi halaman; pilihan di URL; empty state sebelum memilih | Pilihan di URL ada (`?u=` L3247, `?cl=` L3011). `/live-tracking` tidak punya picker di aksi halaman (memilih lewat klik peta atau baris) dan tidak punya empty state sebelum memilih. `/mapping-outlet` menaruh `select` klien di dalam kartu dan memilih klien pertama otomatis. | L3011, L3247 |
| P-50 | Side-list `20rem 1fr`, daftar `max-h-32rem` maks 60 baris, baris terpilih terisi | `/live-tracking` memakai `g-side g-flip` (daftar kiri 20rem di xl, L329, L3250); daftar mendahului peta di ponsel. Tanpa `max-h` 32rem dan batas 60 baris; baris terpilih hanya dibuat tebal (`font-weight:700`), tidak terisi. `/mapping-outlet` tidak memakai pola side-list (tabel penuh). | L3250, L3254 |
| P-51 | Hasil pencarian sebagai chip mono | Tidak dikerjakan (dicatat plan fase 7). `/mapping-outlet`. | `pageMapOutlet` L3011-3026 |
| P-53 | Ponsel: daftar dulu; setelah memilih, detail digulir ke tampilan | Daftar muncul lebih dulu (`order:2` pada kartu peta, L330). Tidak ada `scrollIntoView` setelah memilih sales (`setQ({u})`). `/live-tracking`. | L330, L3250 |
| P-54 | Tata letak analitik per charts.md | Grafik memakai helper baru dengan pilihan periode. Tata letak `1.4fr/1fr` (`g-rank`) hanya di `/penjualan` (L1997). `/sell-in`, `/target`, `/insentif`, `/grading`, `/sku-display`, `/market-share`, `/kepatuhan-kunjungan`, `/aktivitas` belum memakai layout analitik dan drill-down chip (dipindah ke fase 6-7 lalu tidak dikerjakan). | L1997, L3859, L2265 |
| P-55 | Baris insight: lencana faktor, judul, skor, alasan, meter, ≤4 tautan + `+N` | Hanya baris tautan dengan bar di Top sales (L1930) dan di `/penjualan` drill. Tanpa lencana faktor, alasan per baris, atau `+N`; alasan hanya di prosa `ovSummary` (L1904). `rankedRow()` tidak dipakai. Dicatat plan fase 7 sebagai tidak dikerjakan. | L1930, L1904, L976 |
| P-58 | Empty state analitik menyebut hasil baik dan kapan baris muncul | Beberapa ada ("Semua sales memenuhi syarat insentif", L1907). Empty state tabel memakai deskripsi generik "Data muncul di sini setelah dicatat dari aplikasi atau dashboard." (L1615) di semua rute. | L1615, L1907 |
| P-62 | Banner netral: sistem mana pemilik data | `/sinkron` sudah (banner di `simSyncCard`, L3295). `/user-sales` tidak punya banner kepemilikan data SIM; callout-nya (L3054) menjelaskan pemisahan role, bukan kepemilikan data. | L3295, L3054 |

### Finish checklist (F)

| ID | Syarat singkat | Yang kurang dan untuk rute mana | Bukti |
|---|---|---|---|
| F-01 | Satu aksen per wilayah; merah hanya untuk bahaya | Status "Belum Tertarik" bernada `danger` (merah) pada kolom status `/leads` (L2416), padahal itu hasil bisnis, bukan bahaya. Ikon Hapus di tiap baris master data berwarna merah permanen (`.danger-i`, L261, `rowActs` L963); tombol "Hapus" merah di footer `openForm` (L2941). Wilayah hero Overview benar (satu kartu aksen, sisanya ink). | L2416, L261, L963 |
| F-02 | Kartu tanpa border; tabel utama daftar di kanvas | `.fchip` di dalam kartu, panel, dan modal memakai border 1px (L364). Tabel `/penugasan`, `/quiz`, `/survey`, `/canvassing` masih di kartu (lihat P-15). | L364, L1805 |
| F-06 | Galat setelah submit pertama; tanda wajib, hint, default terlihat; submit menyebut kata kerja | Form master data (`saveForm` L2945, `showFormErrs` L2978) dan konfirmasi tolak benar. `ordSave` (L2059), `djSave` (L2110) menampilkan satu kalimat galat di satu elemen (`of_e`, `dj_e`), bukan per field dan tidak diperbarui saat mengetik; `visSave` (L2207) hanya toast. | L2059, L2110, L2207 |
| F-08 | Empty state punya judul, deskripsi, aksi lanjut; semua tombol melakukan sesuatu | `dataTable` empty state (L1629) punya judul dan deskripsi tetapi tanpa aksi lanjut (hanya `crudList` menambah "Hapus filter", L2917). Tombol yang hanya toast: ikon Ubah di `/parameter` (L3632, toast "Parameter disimpan" tanpa edit), "Ubah" di `promoPanel` (L2323), "Tambah" hari libur (L3639). | L1629, L3632, L2323 |
| F-09 | Daftar menjaga pencarian, filter, posisi tabel saat Back | Pencarian dan filter di URL, scroll `main` dipulihkan saat Back (L1857-1858). Posisi tabel (halaman) tidak per entri riwayat (lihat L-30). | L1859, L1857 |
| F-10 | Hover = surface; tekan = scale .98; fokus = ring aksen; gerak hanya fade, slide, pulse | Fokus (L64) dan hover benar; gerak Overview/tabel hanya fade 200 ms (L1830). Tekan tidak seragam: `.btn.icon-lg`, `.bell`, `.user-btn` .95 (L234), `.bb-i` .97 (L817), `.rowlink` .99 (L573). Ada angkat saat hover: `.collapsed .ra:hover` translateY(-1px) scale(1.05) (L128). | L234, L817, L573, L128 |

## 3. Baris Sudah

L-01, L-02, L-03, L-04, L-06, L-07, L-08, L-09, L-10, L-11, L-13, L-15, L-16, L-17, L-18, L-19, L-21, L-22, L-23, L-24, L-25, L-27, L-28, L-29, L-31, L-32, L-33, L-34, L-35, L-36, L-37, L-38, L-39, L-40, L-41, L-42, L-43, L-46, L-47, L-48, L-49, L-50, L-51, L-52, L-53, L-54, P-02, P-03, P-04, P-10, P-25, P-27, P-29, P-36, P-37, P-41, P-61, P-63, P-65, P-66, P-67, P-68, F-03, F-04, F-05, F-07, F-12, F-13, F-14

Catatan untuk beberapa baris Sudah yang punya selisih kecil:

- L-04: pemulihan scroll hanya saat jalur berubah dan Navigation API tersedia (L1095, L1857-1858); Back di jalur yang sama tidak mengubah scroll.
- L-17: lebar maksimum judul 14rem/20rem (L166), resep 14rem/18rem.
- L-29: panel menutup lewat Back (`popstate`, L1017) dan riwayat `layer`; tidak ada `?id=` (dicatat plan fase 4).
- L-43 dan F-12: menu bel, pengguna, workspace, Buat, dan hasil pencarian menjadi bottom sheet berpegangan di ponsel (`openPop` L1025, `.sheet` L752, `.dd-sheet` L461-462); `.modal` tetap di tengah karena itu dialog.
- L-50: dua grid inline memakai `1fr 1fr` dan `1fr 90px` tanpa `minmax` (funnel Overview L1962, dan satu lagi), tetapi bukan grid khusus breakpoint.
- L-54: diukur lewat render sinkron semua 53 rute di 375 dan 768; lihat catatan di atas.
- P-36: adaptasi "tabel dengan chip jenis" diterapkan di `/approval` (L2227); antrean lain (`/toko?tab=usulan`, `/klaim-biaya`, `/task-pending`) hanya berisi satu jenis. Kekurangan konfirmasi di antrean itu dicatat di P-38.
- P-61: kartu koneksi SIM ada (L3295) tanpa endpoint mono, karena tidak ada data endpoint.

## 4. Baris N/A

- L-26: satu akun admin demo tanpa penugasan per pengguna; toggle "For me/Site alerts" tidak relevan.
- P-07, P-08: tidak ada tampilan kedua dan tidak ada beranda per role.
- P-17: tidak ada tampilan kartu.
- P-30: peran admin tunggal; penjaga izin per role ada lewat `can()` tetapi bukan resep ini.
- P-31, P-32, P-33, P-34, P-35: tidak ada dokumen berrevisi.
- P-39, P-40, P-42, P-43: tidak ada papan kolom.
- P-44: tidak ada pemantauan mesin; `/live-tracking` mengikuti P-50.
- P-45, P-46, P-47, P-48: `/penugasan` Jadwal berupa tabel, tidak ada Gantt.
- P-52: tidak ada pohon penelusuran atau rantai alur.
- P-56: tidak ada fitur rekomendasi.
- P-57: `tgtModal` hanya isian biasa.
- P-59, P-60: layar operator dan entri pengukuran tidak ada di dashboard.
- P-64: tidak ada pemantauan perangkat dengan grafik.
- P-69: aplikasi PWA operator; mockup mobile punya gaya sendiri.
- F-11: aturan aplikasi operator.
