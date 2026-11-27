# Desain: halaman Summary (mockup mobile)

Tanggal: 27 November 2026. Ukuran: perubahan kecil di satu file (`mobile/index.html`).

## Maksud

Sales membuka Summary dan langsung tahu dua hal: apa yang sudah dikerjakan dan target apa yang masih tersisa. Summary menggantikan halaman Target & pencapaian untuk semua role.

## Tujuan

- Tab bawah semua role: Beranda, Aktivitas Saya / Aktivitas Toko, Absen, Summary, Akun.
- Wording menu "Target" menjadi "Summary" di semua tempat (menu Beranda, tautan section, tab bawah).
- Summary punya filter **Hari ini | Bulan ini**, default Hari ini.
- Isi Summary berasal dari pekerjaan di Aktivitas Saya / Aktivitas Toko tiap role.

## Bukan tujuan

- Menghapus kode halaman Target. Halaman itu hanya tidak bisa dibuka dari menu.
- Mengubah dashboard.
- Menambah sumber data baru. Angka Bulan ini yang belum ada di aplikasi memakai data contoh, seperti halaman Target sekarang.

## Anti-tujuan

- Halaman penuh angka yang harus di-scroll dulu sebelum tahu status.
- Tampilan berbeda-beda per role.

## Pendekatan

- **A (dipilih):** satu halaman Summary dengan tata letak tetap; tiap role hanya menyediakan isinya dalam bentuk data yang sama. Konsisten, filter dibuat sekali, sejalan dengan kartu Pencapaian di Beranda.
- **B:** halaman per role seperti Target sekarang. Ditolak: kode dan tampilan mudah menyimpang, filter dibuat berulang untuk 8 role.

## Tata letak (atas ke bawah)

1. **Filter** Hari ini | Bulan ini.
2. **Kartu utama:** persen capaian (ring), jumlah selesai dan belum, satu kalimat sisa ("3 toko lagi", "Rp45 jt lagi"). Di bawahnya satu baris kecil progres periode lain (di Hari ini tampil progres bulan; di Bulan ini tampil hari ini).
3. **Yang perlu dikerjakan:** maksimal 4 baris sisa pekerjaan, diurutkan dari yang paling jauh dari target. Tiap baris bisa diketuk ke halaman kerjanya. Jika semua selesai, tampil pesan tuntas.
4. **Rincian:** satu baris per aktivitas dengan bar progres dan angka selesai/target. Baris yang punya rincian tambahan (misal penjualan per kategori, status Direct Sales) bisa dibuka-tutup.
5. **Pencapaian kamu** (peringkat, tepat waktu, badge) di paling bawah, dipindah dari halaman Target.

## Isi per role

Bentuk data tiap item: label, ikon, selesai, target, satuan, tujuan ketuk, rincian opsional.

| Role | Hari ini | Bulan ini |
|---|---|---|
| SPG / Promotor / Mobile | transaksi, modul aktivitas toko, quiz & survey | penjualan vs target (amount), quantity per kategori, akuisisi |
| Canvas / MD | kunjungan toko, pekerjaan toko, quiz & survey | penjualan vs target, kunjungan, akuisisi toko |
| Taking Order | kunjungan, pesanan per toko | nilai pesanan vs target, toko aktif memesan |
| FMCG | kunjungan, tugas aktivitas, support task | kunjungan, tugas aktivitas |
| CBD / REA | Direct Sales dihubungi, Monitoring Mitra, Activity | goal pencairan, deal, mitra dipantau, activity |
| SPV / Arco | capaian tim hari ini | target tim per sales |

Hari ini memakai data yang sama dengan kartu Pencapaian di Beranda (`capItems`). Bulan ini memakai angka di halaman Target sekarang plus data contoh.

## Keputusan yang saya ambil (bisa dikoreksi)

- SPV dan Arco: tab kedua tetap **Tim**, karena tidak punya Aktivitas Saya; Summary mereka berisi capaian tim.
- Tab bawah role yang sekarang menampilkan Penjualan, Pesanan, Kunjungan, atau Direct Sales diganti Aktivitas Saya / Aktivitas Toko. Halaman tujuannya halaman aktivitas yang sudah ada untuk role itu. Riwayat penjualan, Pesanan, dan Direct Sales tetap bisa dibuka dari menu Beranda.
- Label "Aktivitas Toko" dipakai bila sales ditugaskan ke lebih dari 1 toko, mengikuti aturan yang sudah ada.

## Untuk builder

Gaya visual mengikuti mockup (glass, gradasi biru, aksen tan). Uji di lebar 375 tanpa overflow dan tanpa error konsol.
