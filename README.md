# Mockup Sales App

Mockup HTML Sales App SIMGROUP dalam satu deploy. Halaman awal memilih **Mobile** atau **Dashboard**.

| Jalur | Isi |
|---|---|
| `/login/` | Halaman masuk sebelum membuka mockup |
| `/` | Pemilih Mobile atau Dashboard |
| `/mobile/` | Aplikasi sales lapangan (frame iPhone di desktop) |
| `/dashboard/` | Dashboard web, tema terang dan gelap |

## Akses

Setiap halaman mengarahkan ke `/login/` bila belum masuk. Kredensial dibagikan tim proyek dan tercantum di FSD (repo `docs`), tidak ditulis di repo ini karena repo bersifat publik. Kode hanya menyimpan hash SHA-256 dari `username:password`.

Ini penghalang ringan agar tautan tidak dibuka sembarang orang, bukan pengamanan: pemeriksaan terjadi di browser. Sesi disimpan di key `mockupGate.auth` (localStorage bila "Ingat saya" dicentang, sessionStorage bila tidak). Tombol **Keluar dari mockup** di halaman pemilih menghapusnya. Reset data demo di dashboard tidak ikut menghapus sesi ini.

Mengganti kredensial: hitung `printf 'user:password' | shasum -a 256`, lalu ganti nilai `HASH` di `login/index.html`.

## Menjalankan lokal

```bash
python3 -m http.server 5173
```

Buka `http://localhost:5173/`. Tidak ada build step.

## Deploy (Vercel)

1. Di Vercel pilih **Add New Project**, impor repo ini.
2. Framework Preset: **Other**. Build Command dan Output Directory dikosongkan (akar repo disajikan apa adanya).
3. Setiap push ke `main` menghasilkan satu tautan produksi untuk Mobile dan Dashboard.

`vercel.json` mengaktifkan URL bersih dan menambah header `noindex` supaya mockup tidak muncul di mesin pencari.

## Dokumen

- `docs/CONTEXT.md`: konteks proyek, keputusan desain, isi mockup, hal terbuka.
- Requirement dan dokumen sumber ada di repo `docs` (`00-sources`, `01-product`).
- Aturan kerja agent: `AGENTS.md`.
