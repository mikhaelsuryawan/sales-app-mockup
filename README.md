# Mockup Sales App

Mockup HTML Sales App SIMGROUP dalam satu deploy. Halaman awal memilih **Mobile** atau **Dashboard**.

| Jalur | Isi |
|---|---|
| `/` | Pemilih Mobile atau Dashboard |
| `/mobile/` | Aplikasi sales lapangan (frame iPhone di desktop) |
| `/dashboard/` | Dashboard web, tema terang dan gelap |

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
