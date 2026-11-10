# Analisis konsolidasi notes SFA (MOM 9 Nov 2026) terhadap mockup

Sumber: `NOTES-KONSOLIDASI-SFA.docx` (gabungan NOTES 1 dan NOTES 2 meeting klien). Mockup dibandingkan pada commit `0f39343` (main setelah pull 10 Nov 2026).

## 1. Status per poin MOM

| No | Poin MOM | Status sebelum | Yang dikerjakan di branch `feature/konsolidasi-mom-sfa` |
|---|---|---|---|
| 1 | Hierarki Company → Client → Cabang client → Brand → Kategori → Produk | Belum ada Company dan Cabang client | Master **Company** dan **Cabang client** per client. Company adalah grup perusahaan client (konfirmasi 10 Nov), contoh Arunika Group. Client wajib punya company. Company user diturunkan dari client yang ditangani |
| 1 | Kompetitor terpisah dari brand | Sudah (tab di menu Brand) | Jadi menu sendiri **Kompetitor** |
| 2 | Sales bisa multi client | Sudah (field Client multi) | Dua sales demo kini multi client, kolom Company di user dihitung dari client |
| 3 | Mapping outlet ke client (menu baru) | Belum | Menu **Mapping outlet**: satu toko bisa dipetakan ke beberapa client. Penugasan hanya ke toko yang dipetakan ke client sales |
| 3 | Bedakan toko dari client dan toko baru dari sales | Belum | Field **Sumber toko** (Dari client, Input sales) dan filter di daftar toko |
| 3 | Toko canvassing temporary, free text, bisa jadi master | Sebagian (Usulan dari lapangan) | Tab **Toko canvassing (sementara)** dengan aksi Jadikan master |
| 3 | Assign sales jangan di halaman Toko | Kolom Sales masih tampil di Toko | Kolom Sales dihapus dari Toko, diarahkan ke Penugasan |
| 4 | Menu penugasan terpisah, termasuk penjadwalan | Penugasan ada, jadwal di Kunjungan | **Penugasan & jadwal** dengan tab Penempatan toko dan Jadwal kunjungan |
| 4 | Jadwal mingguan bisa pilih hari | Sudah | Tetap |
| 4 | Sales retail dan Sales automation keluar dari Client, pindah ke hak akses role | Belum (field Modul di Client) | Field **Modul** pindah ke Role (IAM). Filter laporan membaca modul dari role user client |
| 5 | Bedakan User dan People | Belum | Akses aplikasi **Tanpa akses** menandai people; filter User atau People |
| 5 | Pisah User management dan Sales management | User admin dan User sales | **User management** (Direktur, HR, Manager, admin) dan **Sales management**. Role baru Direktur, HR, Manager |
| 5 | Flag integrasi SIM atau input manual | Belum | Field **Sumber data**. Data integrasi terkunci, input manual bisa diubah. Tombol Input manual di Sales management |
| 6 | Kategori dinamis General, Company, Client | Belum | Field **Cakupan** di Kategori produk dengan filter |
| 6 | Kategori berjenjang seperti Si Gesit | Sudah | Tetap |
| 6 | Produk free text dari sales ke tabel temporary | Belum | Tab **Dari sales (sementara)** di Produk & harga dengan aksi **Jadikan master** (konfirmasi 10 Nov: produk boleh jadi master). Penjualan produk di luar master tetap tercatat: dari mobile (nama produk diketik SPG) maupun dari form Data penjualan, dan otomatis masuk tabel sementara |
| 6 | Master Satuan dan Konversi | Sudah | Tetap |
| 7 | Sidebar dikelompokkan | Sudah (per fungsi) | Grup disesuaikan dengan usulan MOM: Client management, Toko, Master data, Master data user, Pengaturan aplikasi |
| 8 | Menu Data penjualan, input dari dashboard jika gagal input di aplikasi | Belum | Menu **Data penjualan**: daftar transaksi, input manual dengan alasan, toko dari master atau teks bebas, hapus (soft delete) khusus input dashboard. Masuk ledger yang sama dengan laporan |

Branch `feat/client-hierarchy-produk-crud` (Yogie, 9 Nov) belum di-merge dan tidak dipakai: arahnya "client entitas tertinggi, 1 user 1 client" bertentangan dengan MOM ini (company di atas client, sales multi client). CRUD produk dan tree picker kategori di branch itu masih bisa diambil terpisah.

## 2. Hal terbuka yang tidak diimplementasikan

- Cara menarik data company, client, dan industri dari BPR; teknis multi company dan multi client (meeting tim BPR 1).
- Mapping SIM ke SIAP.
- Isi catatan "DASHBORD" di NOTES 2 belum ada rinciannya.

## 3. Modul umum Sales Force Automation dan posisi mockup

Dokumen notes hanya menyebut SFA sebagai referensi tanpa daftar modul SFA Tintin. Perbandingan di bawah memakai modul yang lazim di produk SFA ritel dan distribusi.

| Modul SFA umum | Ada di mockup | Catatan |
|---|---|---|
| Master organisasi, wilayah, outlet | Ya | Company, client, cabang, region, toko, mapping outlet |
| Master produk, harga, satuan | Ya | Harga per toko, satuan dan konversi |
| User, role, hak akses | Ya | IAM web dan mobile, user dan people |
| Absensi (GPS, selfie) | Ya | Face match, WFO radius |
| Rencana kunjungan (journey plan) | Ya | Penugasan & jadwal, periode dan hari |
| Eksekusi kunjungan (check-in, check-out, foto) | Ya | Radius toko, watermark |
| Order taking | Ya | Pemesanan |
| Penjualan langsung dan canvassing (van sales) | Ya | Stok bawaan, limit, retur |
| Registrasi outlet baru (NOO) | Ya | Usulan toko dan toko sementara |
| Merchandising (display, planogram, POSM) | Ya | SKU display, facing, campaign |
| Audit harga, promo, stok | Ya | Price promo, cek stok, stock opname |
| Aktivitas kompetitor dan market share | Ya | Market share dan master kompetitor |
| Survey dan quiz | Ya | |
| Target, KPI, insentif, grading | Ya | |
| Approval | Ya | |
| Leads atau opportunity (B2B) | Ya | Leads pinjaman |
| Laporan dan dashboard | Ya | |
| **Penagihan dan piutang (collection)** | Usulan | Menu Penagihan & piutang: tagihan per toko, umur piutang, catat pembayaran |
| **Klaim biaya dan reimburse** | Usulan | Menu Klaim biaya: bensin, parkir, tol, makan per kunjungan dengan foto bukti, limit, setujui atau tolak |
| **Live tracking peta sales** | Usulan | Menu Live tracking: posisi terakhir, status (di toko, perjalanan, offline), baterai |
| **Kepatuhan rencana kunjungan** | Usulan | Menu Kepatuhan kunjungan: rencana vs realisasi vs tepat waktu per sales |
| **Pengumuman (broadcast)** | Usulan | Menu Pengumuman: kirim ke role dan client, jumlah yang sudah membaca |
| **Status sinkron offline** | Usulan | Menu Status sinkron: antrean data per perangkat, versi aplikasi, pengingat. Absensi tidak masuk antrean |

Keenam modul di atas tidak disebut di BRD, URD, maupun MOM. Atas persetujuan 10 Nov 2026 modul ini ditampilkan di dashboard dalam grup sidebar **Usulan SFA**, dan tiap halaman memuat catatan Usulan. Belum ada layar mobile untuk modul ini.
