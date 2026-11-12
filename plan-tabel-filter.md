# Plan: tabel dan filter konsisten di dashboard

Acuan desain: halaman **Laporan penjualan**. Filter punya kartu sendiri di atas, tabel punya kartu sendiri di bawahnya.

## 1. Hasil audit (53 rute, 58 pemanggilan `dataTable`, 28 `crudList`)

Hanya Laporan penjualan dan filter Leads yang sudah memakai kartu "Filter" terpisah. Sisanya jatuh ke tiga pola:

| Pola | Kondisi sekarang | Halaman |
|---|---|---|
| A. Filter menempel di kartu tabel | Kotak cari, chip status, select ada di dalam kartu yang sama dengan tabel (di atas baris data) | Semua halaman master lewat `crudList` (28: Client, Toko, Kategori toko, Brand, Region, Cabang client, Kompetitor, Satuan, Konversi, Event, Status produk, Form penjualan, Konfigurasi aktivitas, Company SIM, Sektor, User sales, User admin, Kategori produk, Produk, Sell-in, Target, Grading, IAM, Mitra), serta Data penjualan, Leads (cari dan chip), Kunjungan, Approval, Promo, Penagihan, Pemesanan, Penugasan, Mapping outlet, Absensi |
| B. Tabel tanpa filter | Hanya kartu tabel, tanpa cari atau filter | Overdue, POSM, Klaim biaya, Live tracking, Kepatuhan kunjungan (7 tabel), Pengumuman, Sinkron, SKU display, Market share, Canvassing, Task pending, Insentif |
| C. Acuan | Kartu Filter terpisah, ringkasan "Menampilkan ...", tombol Hapus filter | Laporan penjualan, filter Leads (`ldFilters`) |

Ketidakkonsistenan yang terlihat di tangkapan layar:
- Kotak cari ada di dalam kartu tabel di Leads, Data penjualan, Toko, dan Kategori produk, tetapi di kartu Filter di Laporan penjualan.
- Pemilih periode ada di judul kartu (Data penjualan), sebagai pil (Laporan), atau tidak ada.
- Chip status berdampingan dengan kotak cari di satu halaman dan di baris terpisah di halaman lain.
- Tidak ada tombol "Hapus filter" dan ringkasan hasil di luar Laporan penjualan dan Leads.
- Tombol aksi (Tambah, Template, Unggah) bercampur dengan pemilih periode di kepala kartu tabel.

## 2. Standar yang berlaku (satu aturan untuk semua halaman)

**Kartu "Filter"** (komponen baru `filterCard`), di atas kartu tabel:
1. Baris 1: kotak cari (lebar fleksibel), lalu pemilih periode atau rentang tanggal bila relevan.
2. Baris 2: chip status (satu grup), lalu select dimensi (klien, cabang, toko, sales, dan sebagainya), lalu tombol "Hapus filter" yang hanya muncul saat ada filter aktif.
3. Baris ringkasan: "Menampilkan **...**: N data." Filter berlaku untuk tabel di bawahnya.
4. Judul kartu selalu "Filter". Jarak ke kartu berikutnya 16px.

**Kartu tabel**, di bawah kartu Filter:
1. Judul = nama data (Daftar leads, Transaksi penjualan, ...), jumlah baris di sebelah judul.
2. Kepala kartu hanya berisi aksi ke data (Tambah, Template, Unggah, Ekspor). Tidak ada kotak cari, chip, atau select di sini.
3. Isi: bulk bar (bila ada pilihan), `dataTable`, pager "Baris" dan prev/next, status kosong yang sama.

**Aturan kapan filter wajib:**
- Tabel yang bisa lebih dari satu halaman (lebih dari 12 baris) atau punya dimensi status: wajib ada kartu Filter.
- Tabel kecil, tetap, di bawah 12 baris tanpa dimensi status: cukup kartu tabel, tanpa kartu Filter kosong.
- Halaman dengan tab: kartu Filter berada di atas tab bila berlaku untuk semua tab, atau di dalam tab bila hanya untuk tab itu.

**Mobile (375):** kartu Filter bisa dilipat ke satu baris "Cari dan filter" dengan jumlah filter aktif. Kontrol minimal 44px. Tidak boleh ada overflow horizontal.

## 3. Fase

### Fase 0. Komponen dan gaya dasar
- Tulis `filterCard({search, period, chips, selects, summary, dirty, onClear})` di dekat `card()`.
- Tulis gaya `.fcard` (jarak, baris, ringkasan, lipat di HP) dan tampilan kepala kartu tabel (judul, jumlah, aksi).
- Ubah `card('Filter', ...)` di Laporan penjualan dan `ldFilters` agar memakai komponen yang sama (tampilan tetap).
- Pindahkan helper `setQ` pencarian ke satu fungsi `searchBox(q,{ph})` agar placeholder, label sr-only, dan tinggi sama di semua halaman.
- Selesai bila: Laporan penjualan dan Leads tampil sama seperti sekarang, dan komponen terpakai di keduanya.

### Fase 1. Halaman master (`crudList`, 28 halaman)
- Ubah `crudList` agar mengembalikan kartu Filter (cari dan chip Semua/Aktif/Nonaktif, select tambahan dari `E.filter` bila ada) lalu kartu tabel. Satu perubahan berlaku untuk 28 halaman.
- Halaman yang menempelkan filter sendiri sebelum `crudList` (Client dan `simcoFilter`, Toko, Kategori produk, Produk, Sell-in, User admin, Konversi) dipindahkan ke slot select di kartu Filter.
- Selesai bila: semua halaman master punya Filter + tabel terpisah dan perilaku cari, chip, tambah, ubah, hapus tidak berubah.

### Fase 2. Halaman transaksi dan antrean dengan filter
Data penjualan, Leads (pindahkan cari dan chip ke kartu `ldFilters`), Kunjungan, Approval, Penagihan, Pemesanan, Penugasan, Promo, Mapping outlet, Absensi.
- Pemilih periode berpindah dari judul kartu ke kartu Filter.
- Kartu stat yang berfungsi sebagai filter (Data penjualan, Leads) tetap, tetapi status aktifnya tampil juga sebagai chip di kartu Filter dan terhitung di "Hapus filter".
- Selesai bila: tiap halaman punya kartu Filter di atas, kepala kartu tabel hanya berisi aksi.

### Fase 3. Halaman bertab dan campuran
Penjualan, Target, Insentif, Grading, IAM, Produk, Sell-in, Kategori produk, Mitra, Task pending.
- Tentukan per halaman: Filter di atas tab (berlaku semua tab) atau di dalam tab.
- Tabel di dalam tab memakai kepala kartu yang sama.
- Selesai bila: tidak ada kotak cari atau chip yang tersisa di dalam kartu tabel.

### Fase 4. Tabel tanpa filter
Overdue, POSM, Klaim biaya, Live tracking, Kepatuhan kunjungan (7 tabel), Pengumuman, Sinkron, SKU display, Market share, Canvassing.
- Terapkan aturan "wajib filter bila lebih dari 12 baris atau punya status": tambah kartu Filter minimal (cari, status) pada yang memenuhi syarat. Sisanya tetap kartu tabel dengan kepala yang seragam.
- Selesai bila: setiap tabel mengikuti salah satu dari dua bentuk baku, tidak ada bentuk ketiga.

### Fase 5. Verifikasi dan dokumen
- Skrip pemeriksa di konsol: untuk setiap rute, pastikan tidak ada `.card` yang berisi `input[name=q]` atau `.chips` bersama `table.tbl` (tidak ada filter di dalam kartu tabel).
- Telusuri 53 rute di 375, 768, 1440, tema terang dan gelap: tanpa error konsol, tanpa overflow horizontal.
- Uji interaksi: cari, chip, select, Hapus filter, pager, bulk select, buka panel dari baris, simpan di Leads, Data penjualan, Toko, Kategori produk.
- Tangkapan layar sebelum dan sesudah untuk enam halaman contoh dari pengguna.
- Tambah langkah demo di `docs/CONTEXT.md`, naikkan `DASH_VER` bila bentuk data berubah (seharusnya tidak).

## 4. Urutan kerja dan risiko

Urutan: 0, 1, 2, 3, 4, 5. Satu commit per fase, satu PR untuk seluruhnya, dengan verifikasi di setiap fase.

Risiko:
- `crudList` dipakai 28 halaman: uji tiga halaman dengan bentuk berbeda (Client dengan filter SIM, Toko dengan chip khusus, User admin) sebelum lanjut.
- Kartu stat yang menjadi filter perlu keadaan aktif yang terbaca dari kartu Filter agar tidak ada dua sumber kebenaran. Satu sumber: parameter `q`.
- Penjagaan fokus: setelah `setQ` render ulang, kotak cari kehilangan fokus. Pertahankan fokus dan posisi kursor di `searchBox`.

## 5. Keputusan untuk pengguna

1. Kartu Filter pada tabel kecil (di bawah 12 baris tanpa status): tidak dibuat (rekomendasi) atau tetap dibuat di semua halaman agar seragam mutlak.
2. Chip status: tetap chip (rekomendasi, seperti sekarang) atau pil seperti pemilih periode di Laporan penjualan.

## 6. Status

Semua fase selesai dan diverifikasi:
- Fase 0 sampai 4: kode di `dashboard/index.html` (komponen `filterCard`, `searchBox`, kelas `.fcard` dan `.fstack`; `crudList` mengembalikan kartu Filter dan kartu tabel).
- Fase 5: 53 rute plus Quiz dan Survey diperiksa tanpa error konsol, tanpa overflow horizontal, tanpa kotak cari atau chip di dalam kartu tabel, di lebar 375, 768, dan 1440. Tema terang dan gelap sudah dicoba (375 dan 768 gelap, 1440 terang).
- Pengecualian yang disengaja: Matrix approval (matriks konfigurasi tetap) dan tabel kecil tanpa status (Live tracking, Sinkron, POSM, SKU display, Market share, Kepatuhan kunjungan, Canvassing, Task pending, Insentif, Target) tetap hanya kartu tabel.
- Perbaikan sampingan: chip Semua di Klaim biaya sebelumnya kembali ke Menunggu; sekarang menampilkan semua klaim.
