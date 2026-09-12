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


---

## 🎟️ Jenis Undangan & Prefix Pesanan

| Prefix | Jenis | FIELD_JUDUL (judul acara) |
|--------|-------|---------------------------|
| `PSN` | Pernikahan | `namaPendek` |
| `KHT` | Khitan | `namaAnak` |
| `THL` | Tahlil / Haul | `namaAlm` |
| `PNG` | Pengajian | `namaMajelis` |
| `AQH` | Aqiqah | `namaBayi` |
| `WLS` | Walimah | `namaJamaah` |

**Penamaan repo terbit:** `[prefix-]slug(judul)-slug(noPesanan)`
- slug: huruf kecil, `&` → ` dan `, spasi → `-`
- contoh: `PSN-001` "Agus & Nina" → repo `agus-dan-nina-psn-001`

---

## 🔐 Login Pemesan

Form dilindungi login sederhana:
- **Username** + **Nomor pesanan** (diberikan admin, dibuat lewat generator-kode)
- Verifikasi: hash `hashKode(u + "|" + n + "|" + SECRET_KEY)` (FNV-style, hasil base36)
  dicocokkan terhadap array `DAFTAR_PESANAN` berisi `{kode, label(base64 "user|no"), exp}`
- Sesi login disimpan di `localStorage: sesiPesanan`
- Draft isian tersimpan otomatis di `localStorage: dataUndanganPrem`

> Kode pesanan dibuat admin via `generator-kode.html`, lalu ditempel ke array
> `DAFTAR_PESANAN` di dalam file form.

---

## ✨ Fitur Form

- **8 tema siap pakai** (array `THEMES`): `klasik`, `rosegold`, `sage`, `navy`,
  `lavender`, `maroon`, `gold`, `pastel` + **warna custom** bebas pilih
- **3 gaya sampul**: foto / elegan / minimalis
- **Avatar mempelai** (khusus jenis yang membutuhkan)
- **Peta lokasi**: Google Maps iframe (`maps.google.com/maps?q=...&output=embed`)
- **Upload foto** (prewedding/dokumentasi):
  - kompresi otomatis via canvas → lebar maks **1280px**, kualitas **0.78**
    (mencegah localStorage penuh)
  - tombol **⭐** = pilih sebagai foto sampul (`sampulIdx`)
  - tombol **🗑** = hapus foto
- **Koleksi Studio** (array `STUDIO_FOTO`, opsional): foto siap pakai;
  otomatis tersembunyi jika array kosong
- **Musik** latar undangan
- **Preview langsung** via `document.write` sebelum kirim
- **Tombol KIRIM** → mengunduh paket `.json` berisi `{jenis, pesanan, data, waktu}`
  → file ini dikirim ke admin via WhatsApp

---

## 🧩 Template & Titik Suntik Data

- Template undangan disimpan di dalam `<textarea id="tpl" style="display:none">`
- Tag penutup di dalam template ditulis **`</textareaX>`** lalu dipulihkan saat render:
  `.split("</textareaX>").join("</textarea>")` — trik agar preview tidak rusak separuh
- Titik suntik data saat penerbitan: string
  ```js
  var DATA_EMBED = null;




Parameter URL	Fungsi
 ?to=Nama_Tamu 	Nama tamu tampil di sampul + otomatis jadi default buku tamu (spasi pakai  _ ,  &  pakai  %26 )
 ?panel=1 	👑 Panel Pengantin — login pakai nomor pesanan ( data.pesananNo ); untuk membuat link tamu & kirim WA berurutan



