# 📦 Sistem Undangan Digital — Form Pengisian (Serverless, GitHub Pages)

Form pengisian undangan digital berbasis **HTML + JavaScript polos** (tanpa framework,
tanpa build, tanpa server). Berjalan sepenuhnya di browser dan di-hosting gratis via
**GitHub Pages**.

---

## 🏗️ Arsitektur Sistem (4 Komponen)

| # | Komponen | Lokasi | Keterangan |
|---|----------|--------|------------|
| 1 | **Form** (repo ini) | `index.html` di GitHub Pages | Diisi pemesan; hasil akhir = unduhan paket `.json` |
| 2 | **pusat.html** | ⚠️ FILE LOKAL ADMIN — tidak pernah di-upload | Menerbitkan website dari `.json`; satu-satunya tempat token GitHub disimpan |
| 3 | **generator-kode.html** | File lokal admin | Membuat kode pesanan (hash) + label base64 + masa berlaku |
| 4 | **Website terbit** | Repo GitHub Pages terpisah per pesanan | Hasil akhir yang disebar ke tamu |

**Alur bisnis:**