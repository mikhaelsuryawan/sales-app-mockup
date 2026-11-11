# Verifikasi ulang UX, struktur, dan aset mobile dashboard

Tanggal: 11 Nov 2026. Target: `dashboard/index.html` (4121 baris). Acuan baris: `docs/wit-mes/audit-ux-struktur.md` (U-01 s.d. U-53, S-01 s.d. S-131, M-01 s.d. M-29). Nomor baris di bawah menunjuk ke berkas saat ini; banyak yang bergeser dari audit awal karena berkas bertambah. Sudah hanya bila kodenya terlihat melakukan hal itu.

## 1. Ringkasan hitungan

**Semua 213 baris: Sudah 95 · Sebagian 63 · Belum 4 · N/A 51**

| Kelompok | Sudah | Sebagian | Belum | N/A | Jumlah |
|---|---|---|---|---|---|
| UX (U-01 s.d. U-53) | 23 | 25 | 0 | 5 | 53 |
| Struktur (S-01 s.d. S-131) | 62 | 33 | 3 | 33 | 131 |
| Aset mobile (M-01 s.d. M-29) | 10 | 5 | 1 | 13 | 29 |
| Total | 95 | 63 | 4 | 51 | 213 |

Catatan metode: kode dibaca langsung. Untuk U-53 dan S-64 saya juga menjalankan `render()` untuk 53 rute di 375, 768, dan 1440 px, tema terang dan gelap, lewat server lokal sementara (sudah dihentikan): selisih `scrollWidth` dengan `clientWidth` pada `main` dan dokumen = 0 di semua kombinasi (318 render), tidak ada galat tertangkap, konsol browser tanpa galat. Yang tidak diperiksa: tangkapan layar visual, halaman pemilih (`../index.html`) dan dua tautan antar mockup, serta layar di bawah keadaan terbuka (panel, menu). Kontras (U-48) dihitung dari token, bukan diukur di layar.

Temuan paling berbobot: plan fase 5 menyatakan selesai bila "setiap tombol mutasi melewati `act()`", tetapi hanya 9 titik panggil memakai `act()` (lihat S-10); pemakaian helper kit juga belum menyeluruh (`actionMenu` dan `statCard` didefinisikan tapi tidak dipanggil).

## 2. Baris yang masih Sebagian atau Belum

| ID | Status | Persyaratan | Yang kurang | Bukti (baris) |
|---|---|---|---|---|
| U-01 | Sebagian | Satu aksen per wilayah | Bar bawah memuat dua elemen aksen sekaligus: item aktif `.bb-i.on` dan tombol bulat `.bb-act`, keduanya accent-strong. | CSS 818-819 |
| U-04 | Sebagian | Header ink di detail rekaman | Hero ink (`panelHero`) hanya di 3 panel: ordPanel, leadPanel, klaimPanel. trxPanel, attPanel, visPanel, promoPanel, incPanel, aprPanel masih `kv` atau baris biasa. | 1893, 2037, 2471, 3237; 2018, 2159, 2183, 2319, 2367, 2259 |
| U-05 | Sebagian | Satu primary per wilayah, sisanya outline atau menu "⋯" | `actionMenu` didefinisikan tapi tidak dipanggil di mana pun. `rowActs` masih menaruh Ubah dan Hapus inline di tiap baris; footer `openForm` menampilkan Hapus danger dan Simpan berdampingan. Footer `ordPanel` sudah satu primary plus outline. | 1043, 963, 2931-2933, 2040 |
| U-06 | Sebagian | Menu dibangun dari `cond ? item : null` | Hapus selalu dirender (diberi gaya blocked), tidak dihilangkan. Hanya `createItems` yang menyaring per izin; tidak ada menu aksi per rekaman. | 963, 1710, 1035 |
| U-08 | Sebagian | Nonaktif hanya untuk blocker, dengan alasan yang sama dengan aksi | Master data benar: `rowActs(block)` dan guard `act` memakai kalimat `E.inUse`. Kurang: Hapus di footer `openForm` tanpa alasan; kunjungan, penjualan dashboard, promo, pengumuman tidak punya fungsi blocker bersama (kunjungan hanya cek di modal). | 963, 2983, 2996, 2931; 2216-2220, 2125-2127, 2324-2326 |
| U-10 | Sebagian | Toast lampau yang menyebut rekaman | Sebagian toast generik tanpa nama rekaman: "Pertanyaan dihapus", "Aturan disimpan". `saveForm`, `doDel`, `approve` sudah menyebut rekaman. | 3388, 2282-2283 |
| U-11 | Sebagian | Tone toast: success/default/danger sesuai makna | `toast()` kini punya tone `default` (991) dan jalur `act` memakainya. Hapus atau batal di luar `act` masih tone success (centang): djDel, visDel, infoDel, hapus task. | 2127, 2220, 3291, 3103 |
| U-16 | Sebagian | Galat setelah submit pertama lalu live | Live hanya di form `ENT` (`FORM.tried`, listener input, `showFormErrs`). djSave, visSave, form task CBD, limit canvassing, pengumuman, brief, pertanyaan menampilkan galat sekali (toast atau teks) tanpa pembaruan live. | 2964, 2978, 2979-2980; 2110-2116, 2211-2212, 3096, 3191, 3220, 3286, 3322, 3380 |
| U-17 | Sebagian | Galat kalimat utuh, tanda wajib aksen, opsional dinyatakan, default dengan sumber | Galat kalimat dan tanda `*` aksen (`.field .req` accent-text) sudah. Kurang: label "opsional" hanya di 4 tempat, tidak ada placeholder default yang menyebut sumbernya. | 405, 2955-2960; 3518 |
| U-18 | Sebagian | Satuan di slot kanan, jumlah mulai dari sisa, inputMode | `inputmode` ada. Slot `.unit` hanya dipakai 1 kali; tidak ada default "sisa jumlah". | 422, 2100 |
| U-19 | Sebagian | Picker bergantung: anak dikosongkan, anak nonaktif "Pilih X dulu" | Anak disinkronkan (mptSync, pmKatSync, katcSync) tapi tidak ada keadaan nonaktif dengan teks "Pilih X dulu" (grep nol). | 3512, 3469, 3625 |
| U-20 | Sebagian | Mode ubah: deskripsi menyebut yang terkunci, field baca saja | Tile baca saja ada (`fieldHTML` tipe ro). Penjelasan terkunci hanya di sebagian field (hint 2808), tidak di `lock:satLocked`. | 2924-2927, 2808, 2877 |
| U-21 | Sebagian | Submit bernama kata kerja, Batal outline | Kata kerja sudah di banyak tempat (Setujui, Tolak, Hapus x, Ajukan hapus) tapi "Simpan perubahan" masih generik, dan Batal masih `b-card` di djDelAsk, visDelAsk, promoDelAsk, resetAsk. | 2933, 2126, 2218, 2325, 4114 |
| U-23 | Sebagian | Opsi tak bisa dipilih tetap tampil dengan alasan | Combobox mendukung `data-reason` (dataset.reason) dan `aria-disabled`, tapi tidak ada satu pun `<option data-reason=...>` di file (grep nol). | 4067, 4073 |
| U-25 | Sebagian | Konfirmasi: judul pertanyaan, konsekuensi satu kalimat, tombol menunggu isian | Hanya 3 pemakai `confirmAsk` (approve, reject, delAsk master). djDelAsk, visDelAsk, promoDelAsk memakai `openModal` lama dengan `<i>.</i>` setelah "?" sehingga judul "?.". | 1019, 2236, 2247, 2992; 2126, 2218, 2325 |
| U-28 | Sebagian | Rail dan bar bawah dari satu NAV; tab pill di bawah xl | Rail, tab seksi, dan sheet More dari `NAV`; `bottomBar` memakai shortcut tulis tangan (Overview, Approval, Buat, Absensi, Leads). | 1648, 1688, 1769, 1792; 1773-1778 |
| U-29 | Sebagian | Back kembali ke halaman sebelumnya; panel di URL (`?id=`) | Back menutup panel lewat `history.pushState({layer})` dan popstate, tapi panel tidak ada di URL, jadi tidak bisa deep link atau bookmark. | 1005, 1016-1017 |
| U-30 | Sebagian | Daftar menyimpan cari/filter/sort/halaman lewat Back; scroll pulih | Cari dan filter di URL (`setQ`), scroll dipulihkan via Navigation API. Halaman tabel hanya di `ST.tb` memori, direset saat push, hilang saat reload; tidak terikat entri riwayat. | 1093, 1096-1097, 1865, 1859, 1090 |
| U-31 | Sebagian | Filter tertaut di URL dengan replace; `?new=1` dan `?id=` | `?tab`, `?f`, `?q`, `?u`, `?type` memakai replaceState. `?id=` dan `?new=1` tidak ada (kata `new` hanya di `SCOPE_SKIP`). | 1093, 1807 |
| U-33 | Sebagian | EmptyState: judul, cara data masuk, aksi lanjut | `dataTable` mengisi judul dan deskripsi tapi tanpa aksi; `crudList` menaruh "Hapus filter" di dalam teks judul; sebagian empty masih teks saja. | 1628, 2917 |
| U-36 | Sebagian | Detail rekaman hilang: "... tidak ditemukan" dengan Back | `panelMissing` hanya dipakai 4 panel (trx, ord, lead, klaim). visPanel, attPanel, promoPanel, aprPanel, incPanel tanpa guard; gradePanel hanya `return` diam. | 1891; 2016, 2036, 2470, 3236; 2182, 2158, 2318, 2258, 2366 |
| U-39 | Sebagian | Target sentuh 44/48/56px, input 16px | Input 16px sudah (media coarse). `.bb-i` 44 dan `.bb-act` 48. Tidak ada penaikan untuk `.btn.sm` 32px, `.btn.icon-sm` 32px, `.fchip` 40px; aksi utama 56px tidak ada. | 107, 816-819, 236, 238, 360 |
| U-45 | Sebagian | Meta dengan " · ", rentang "mulai → akhir" | " · " luas. Panah "→" hanya di 2 tempat; rentang tanggal memakai "–" (mis. "1–30 Nov", "27–28 Okt"). | 1442, 3106; 1217, 1221 |
| U-47 | Sebagian | Label dari peta `*_LABEL`, tidak ditulis di tempat | Satu peta tone `STATUS_TONE` ada, tapi peta lokal ORD_TONE, STTONE, FM_ST_TONE masih berdiri dan label status banyak yang string inline. | 925, 2033, 1289, 2167 |
| U-51 | Sebagian | Grafik, avatar, segmen `role=img`; echo nilai di live region | av, donut, garis berlabel `role=img`; echo live region ada di donut dan garis. `columnChart` (radiogroup) tidak punya live region. | 922, 1550, 1582, 1585-1594; 1522-1538 |
| S-02 | Sebagian | Urutan file: token, helper bebas domain, data/derive, halaman | Helper bebas domain rapi (905-990), tapi blok derive/data diselipkan di antara halaman: INC/REKON 1481-1500 sebelum kit grafik; `MD`, `ENT`, `act` (2520-2998) setelah halaman Overview sampai Overdue. | 1481, 2520, 2760 |
| S-04 | Sebagian | Halaman tidak punya aturan bisnis | Fungsi derive ada (repRange 1367, deriveInc 1487, refreshDerived 1495) tapi ordSave, djSave, visSave, tgtSave mengubah data langsung. | 2059-2066, 2110-2123, 2207-2214, 2298-2303 |
| S-05 | Sebagian | Satu dialog per entitas | Master data satu pintu `ENT` + `openForm`/`saveForm`; entitas lain modal tersendiri: ordForm, djForm, visForm, promoForm, infoForm, posmModal. | 2760-2998; 2050, 2088, 2194, 2327, 3279 |
| S-06 | Sebagian | Aksi per rekaman dari satu fungsi (rows dan footer panel) | `rowActs` generik, tapi footer panel disusun per entitas dan `actionMenu` tidak dipakai. | 963, 1043 |
| S-08 | Sebagian | Union dengan label/flow dan subset filter | `ORD_NEXT`, `TASK_FLOW`, `STATUS_TONE` ada; tidak ada peta label atau alur per union secara sistematis. | 2034, 3077, 925 |
| S-09 | Sebagian | `newId(p)` dan `nextCode(list,prefix,n)` | `newId` berurutan (ID_SEQ) sudah; `nextCode` tidak ada, kode manusiawi dibuat inline (PO-2610-0.., TRX-2610-D..). | 2751, 2065, 2121 |
| S-10 | Sebagian | Satu `act(type,payload)` untuk semua mutasi | `act` hanya dipanggil di 9 tempat (order advance, request-cancel, approve, reject, leads assign, form entitas, toggle status, remove, klaim). Mutasi di luar `act`: ordSave, djSave, djDel, visSave, visDel, tgtSave, promoDel, taskNext, task CBD, assign toko, limit canvassing, pembayaran, pengumuman. | 2758; 2043, 2047, 2240, 2254, 2468, 2967, 2983, 2996, 3243; 2059, 2110, 2127, 2207, 2220, 2298, 2326, 3080, 3103, 3143, 3191, 3221, 3287-3291 |
| S-12 | Sebagian | Guard gagal tidak mengubah state | Guard diulang di dalam aksi untuk toggleStatus dan doDel (guard `act`). visDel, djDel, promoDel tidak mengecek ulang. | 2983, 2996; 2220, 2127, 2326 |
| S-13 | Sebagian | Satu fungsi blocker dipakai aksi dan UI | Master data: `inUse`/`lockStatus` dipakai tombol dan aksi. Kunjungan: status hanya dicek di modal visDelAsk, tidak di visDel. | 963, 2996; 2217, 2220 |
| S-17 | Sebagian | Envelope berversi, fallback seed, simpan mengembalikan boolean | Snapshot dashboard sudah (DASH_VER, loadDash fallback, writeKey boolean). `salesapp.shared.v1` tanpa versi; perlu perubahan bersama di mobile. | 4097-4112, 909; 1103 |
| S-19 | Belum | Workspace menyaring data | Belum ada penyaringan: `wsPop` hanya mengganti `ST.ws`; dipakai untuk label dan kunci notifikasi. Toast: "Data mockup tetap sama". | 1766, 1684, 1745 |
| S-22 | Sebagian | Keadaan daftar bertahan lewat Back | Lihat U-30. | 1090, 1859 |
| S-30 | Sebagian | Penamaan: storage, izin, kode | Storage `salesapp.dashboard.*` dan izin `area.verb` sesuai. Kode: `newId` menghasilkan `ap-0001`, bukan `PREFIX-tahun-urut`. | 913, 1665, 2751 |
| S-31 | Sebagian | Nama `act()` mengikuti `docs/03-contracts` | Tipe `entitas/kata-kerja` (mis. `order/advance`, `approval/approve`) sudah, tapi `docs/03-contracts` kosong sehingga tidak ada nama kontrak, dan mutasi di luar `act` tidak bertipe. | 2043, 2240 |
| S-38 | Sebagian | Pintasan per peran dengan cadangan | Pintasan bar bawah tetap; tidak berubah menurut peran atau izin. | 1773-1778 |
| S-44 | Sebagian | Koleksi, peta, helper nama | `live`, `byU`, `tokoName`, `userName` ada dengan cadangan "–"; tidak ada koleksi yang disaring workspace (lihat S-19). | 2735, 1120, 2747 |
| S-46 | Sebagian | Adaptasi permissions, persistence, clock | Permissions dan clock Sudah (S-14, S-15); persistence Sebagian (S-17). | 1665-1668, 2749-2754, 4097 |
| S-47 | Sebagian | Tulis baru: store, derive, router, badges, links, pickers, create | `ROUTES` (53 rute), badge `tag`/`spill`, menu Buat ada; objek `paths` tidak ada (S-79). | 4030, 932-935, 1710 |
| S-59 | Sebagian | Tabel kosong dengan EmptyState plus aksi | Lihat U-33: EmptyState tanpa aksi lanjut. | 1628 |
| S-69 | Sebagian | Ringkasan aturan UX (semua baris U) | 25 baris U masih Sebagian. | lihat U-01 s.d. U-51 |
| S-70 | Sebagian | Ringkasan aturan struktur | S-10, S-13, S-17 Sebagian; S-14, S-15 Sudah. | lihat baris S terkait |
| S-72 | Sebagian | Peta `*_LABEL` untuk enum | Lihat U-47. | 925, 2033 |
| S-73 | Sebagian | Ubah data hanya lewat dispatch | Lihat S-10. | 2758 |
| S-74 | Sebagian | Derive bersama di derive, lokal di lib | Lihat S-04. | 1487, 2059 |
| S-76 | Belum | Halaman tidak menyaring store mentah per situs/workspace | Belum: tidak ada saringan workspace sama sekali; halaman menyaring dengan filter cabang/klien sendiri, bukan lewat lapisan scoped. | 1766, 1684 |
| S-78 | Sebagian | Satu badge per union status | `statusBadge`/`spill` + `STATUS_TONE` dipakai luas, tapi peta tone lokal masih berdampingan. | 935, 925, 2033, 1289, 2167 |
| S-79 | Belum | Pembangun jalur dan chip tautan (`paths`) | Belum: tautan berupa string `#/...` dan `go('/...')` tersebar; tidak ada objek `paths`. | grep: tidak ada `paths.` |
| S-82 | Sebagian | Keadaan daftar bertahan lewat Back | Lihat U-30. | 1090, 1859 |
| S-87 | Sebagian | Resep halaman detail (hero ink, Steps, KeyValue, terkait, riwayat) | Hero di 3 panel; `stepper` hanya di ordPanel; `historyBlock` hanya di ordPanel dan klaimPanel; leadPanel tanpa riwayat. | 2037, 2039, 3239, 2471 |
| S-98 | Sebagian | Dialog dengan `tried`; deep link | `tried` ada di form `ENT`; deep link `?id=`/`?new=1` tidak ada. | 2964, 1807 |
| S-102 | Sebagian | Matematika waktu WIB dan konversi input tanggal | `dateLabel`/`nowStamp` memakai offset +7 jam. Tetap ada `new Date(...)` waktu lokal: `repDays`, `repSeries`, `hkDays`, ATT, TRX. | 1683, 2754; 1376, 1384, 3903, 1277, 1314 |
| S-103 | Sebagian | Formatter terpusat (`fmt*`, rupiah, plural, initials) | `rp`, `jt`, `num`, `pct`, `initials`, `fmtIso` ada; tidak ada formatter tanggal tunggal (mis. `day+' Okt'` inline) dan tidak ada `plural`. | 905-908, 920, 1372, 2118 |
| S-104 | Sebagian | `newId`, `nextCode` | Lihat S-09. | 2751 |
| S-109 | Sebagian | Dispatch mencap pelaku dan jam | `audit()` memakai `ME.name` dan jam aplikasi; `djSave` menulis `by:'Dewi Kartika'` tulis tangan dan tidak lewat `act`. | 2755, 2121 |
| S-110 | Sebagian | Peta dan helper nama dengan cadangan | Cadangan "–" ada (tokoName, userName); panel tanpa guard rekaman hilang (U-36). | 2747-2748, 2182, 2258 |
| S-114 | Sebagian | Keadaan per entri riwayat | Scroll per entri via `SCR[navigation.currentEntry.id]`; halaman tabel global `ST.tb`, bukan per entri. | 1096-1097, 1090 |
| M-09 | Sebagian | Tab bar: label dengan hitungan, aria-current, item 56px | Label berhitungan dan `aria-current` sudah; item 44/48px, bukan 56px. | 1775-1776, 816, 819 |
| M-10 | Sebagian | Bar bawah tersembunyi saat panel terbuka; pemulihan scroll | Pemulihan scroll sudah. Bar tidak disembunyikan: overlay z-index 61 hanya menutupinya, tidak ada aturan `.lock .bbar`. | 1096, 1865, 735, 815 |
| M-11 | Sebagian | Tombol Back ke riwayat atau daftar induk | Tautan breadcrumb induk ada di `hcap`, tapi hanya halaman ber-`crumb` (IAM); tidak ada tombol Back. | 1847, 3071 |
| M-19 | Belum | Pembangun jalur (`paths`) | Belum: lihat S-79. | - |
| M-23 | Sebagian | Chip, daftar malas dengan sentinel, EmptyState | Chip dan "N lagi · Muat" sudah; EmptyState tanpa aksi (U-33). | 1633-1634, 1628 |
| M-25 | Sebagian | Sheet aksi dengan toast yang menyebut rekaman | Lihat U-10. | 3388, 2282 |

## 3. Baris Sudah

- UX: U-02, U-07, U-09, U-12, U-13, U-14, U-15, U-22, U-24, U-26, U-27, U-32, U-34, U-35, U-37, U-38, U-44, U-46, U-48, U-49, U-50, U-52, U-53
- Struktur: S-03, S-11, S-14, S-15, S-16, S-18, S-21, S-23, S-24, S-25, S-26, S-33, S-34, S-35, S-36, S-37, S-39, S-40, S-41, S-42, S-43, S-49, S-50, S-51, S-52, S-53, S-54, S-55, S-57, S-58, S-60, S-61, S-62, S-64, S-68, S-75, S-77, S-80, S-81, S-83, S-84, S-85, S-86, S-88, S-89, S-90, S-91, S-92, S-93, S-94, S-95, S-96, S-97, S-100, S-101, S-106, S-107, S-108, S-112, S-113, S-115, S-128
- Mobile: M-01, M-04, M-14, M-15, M-17, M-18, M-21, M-22, M-24, M-29

Dasar bukti utama untuk yang Sudah ada di kode: `attn` Overview (U-02, 1918-1923, filter `>0` dan urut hitungan), `can`/`btn(perm)` (U-07, S-14, S-57: 1668, 943), `act` guard + toast danger (U-12, 2758-2763), `djNote` (U-13, 2106-2109), banner `ST.storageErr` (U-14, 1856), `confirmAsk` (U-26, 1019-1022), pemisah `dataTable` (U-34, 1615-1634), `rovingKey` dan `ccKey` (U-49, 1060, 1540), `rowopen` (U-52, 1628), `resetAsk`/`resetDemo` (S-18, M-021/M-022, 4114-4115), `ACCOUNTS`/`render` guard (S-42, 1664, 1836), `loadDash`/`saveDash` (S-107, 4103-4113).

## 4. Baris N/A (alasan)

- U-03: satu akun tanpa beranda per peran (dua akun demo hanya beda izin)
- U-40, U-41, U-42, U-43: aplikasi operator (tab bar detail, badge/PIN, antrean operator, cek pra-mulai); dashboard memakai login username
- S-01, S-116, S-117, S-118, S-120, S-121, S-122, S-124, S-125, S-126, S-127, S-129: monorepo, TypeScript, Turbo, pnpm, ESLint/Prettier, CI, tsconfig, Vite; AGENTS.md: satu HTML tanpa build
- S-07, S-20, S-45, S-48, S-65, S-66: konvensi pustaka, scope operator, adaptasi mobile operator, rename scope, stack React/Radix, alur produk baru; tidak ada padanan di satu file
- S-27, S-28, S-29, S-99, S-105, S-123: chunk seed, tes `node:test`, gerbang kualitas TS, typecheck, JSON typed, PRNG seeder; AGENTS.md: tanpa tes dan lint (data memakai LCG `rnd` 1273 tanpa generator)
- S-32, S-63, S-111: salin berkas sebagai file, dependensi tak terpakai, `useNow` (tidak ada label waktu relatif)
- S-56: tidak ada Gantt dengan header sticky di dalam overflow-x
- S-67, S-71: aturan proses (resep layar baru, daftar sebelum selesai); tidak bisa diverifikasi dari berkas
- S-119: tanpa ESLint; maksudnya (tanpa `console.log`) terpenuhi, lihat bagian 5
- S-130: tes perjalanan; penggantinya, skrip perjalanan manual, ada di `docs/CONTEXT.md` bagian 7 (baris 159)
- S-131: README contoh repo; berkas di luar target (ada `mockup/README.md`, `AGENTS.md`, `docs/CONTEXT.md`), tidak diaudit
- M-02, M-03: PWA manifest dan ikon operator; demo statis tanpa service worker
- M-05, M-06, M-07, M-08: provider, auth, store, scope operator; proyek sengaja berbagi `salesapp.shared.v1`
- M-12, M-13: header sapaan operator dan LogoMark; dashboard memakai header dan logo SIM sendiri
- M-16, M-20: stepper jumlah potong dan sapaan jam aplikasi; tak ada padanan
- M-26, M-27, M-28: login badge/PIN, beranda operator, inspeksi dan instruksi; domain operator

Semua alasan N/A dari audit awal masih berlaku; tidak ada baris N/A yang saya ubah.

## 5. Hasil grep

| Pola | Jumlah | Rincian |
|---|---|---|
| `console.log` (dan `console.warn/error/info/debug`) | 0 | Satu kecocokan kata "console." hanya teks UI `pageNotFound`: "Alamat ini tidak ada di console" (1890) |
| Em dash U+2014 di seluruh berkas | 0 | Audit awal: 8 |
| Tanda seru di string UI | 0 | Dicari `!` setelah huruf/angka/kutip/kurung dan `!` sebelum spasi, kutip, tanda baca; semua sisa `!` adalah operator JS |
| "Dewi Kartika" di luar tabel akun (`ACCOUNTS`, 1664) | 8 baris | Tulis tangan pelaku/profil: 2121 (`djSave`, `by:'Dewi Kartika'`), 3749 dan 3750 (kartu Profil di Pengaturan). Data contoh: 1221 (seed approval `a7`), 2567, 2568, 2570 (seed log audit `MD.log`), 2580 (master user `dewi`) |

Catatan: tidak ada pelanggaran untuk `console.log`, em dash, dan tanda seru. Untuk nama, baris 2121 dan 3749-3750 adalah pelanggaran nyata atas "pelaku dari sesi" karena tidak membaca `ME`; sisanya data fiktif yang wajar.
