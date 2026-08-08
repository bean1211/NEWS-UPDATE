# SUMBER — Direct Wire

Web app satu-halaman yang menampilkan berita **langsung dari newsroom/press-release resmi perusahaan
maupun proyek crypto** — tanpa lewat redaksi seperti Bloomberg atau Reuters. Klik judul mana pun akan
membawamu ke halaman resmi di domain perusahaan/proyek tersebut.

**Sumber teknologi:** AMD, Intel, Samsung, NVIDIA, Ethereum Foundation, Apple, Google, Microsoft, Meta, Amazon.

**Sumber crypto/stablecoin:** Tether (USDT), BNB Chain, Circle (USDC), Ripple (XRP), Solana, TRON DAO,
Hyperliquid, Dogecoin Foundation, UNUS SED LEO (Bitfinex), Cardano, Monero, Chainlink, Stellar,
Sky/MakerDAO (DAI), Ethena (USDe), Canton Network, Litecoin, Hedera (HBAR), Avalanche, PayPal (PYUSD).

> Catatan: **Hyperliquid** dan **Canton Network** belum punya blog RSS resmi yang terpisah dari halaman
> pengumuman/media resource-nya — untuk keduanya, `feed` dan `home` mengarah ke halaman yang sama, jadi
> situs akan langsung menampilkan tombol link langsung ke sana alih-alih mencoba parse RSS.

Tidak ada build step, tidak ada dependency. Cukup satu berkas `index.html`.

## Cara deploy ke GitHub Pages

1. Buat repository baru di GitHub (public), misalnya `sumber`.
2. Upload berkas `index.html` ini ke root repository (lewat web UI "Add file → Upload files",
   atau lewat git):
   ```bash
   git init
   git add index.html README.md
   git commit -m "init: SUMBER direct wire"
   git branch -M main
   git remote add origin https://github.com/USERNAME/sumber.git
   git push -u origin main
   ```
3. Di repo GitHub, buka **Settings → Pages**.
4. Pada **Build and deployment → Source**, pilih **Deploy from a branch**.
5. Pilih branch **main** dan folder **/ (root)**, lalu **Save**.
6. Tunggu 1–2 menit, situs akan aktif di:
   `https://USERNAME.github.io/sumber/`

Setiap kali kamu push perubahan ke `main`, GitHub Pages otomatis re-deploy.

## Cara kerja

- Daftar sumber ada di `index.html`, variabel `SOURCES` di bagian `<script>`. Setiap entri berisi
  `feed` (URL RSS resmi perusahaan) dan `home` (link fallback newsroom).
- Karena kebanyakan newsroom perusahaan belum mengizinkan `fetch()` langsung dari browser (CORS),
  situs mencoba mengambil feed lewat beberapa proxy CORS publik secara berurutan
  (`allorigins.win` → `corsproxy.io` → `codetabs.com`). Kalau satu down, otomatis coba yang lain.
- Kalau satu sumber tetap gagal dimuat, situs tetap menampilkan tombol link langsung ke
  newsroom resminya — jadi tidak pernah diam-diam menyembunyikan sumber yang gagal.
- Tombol **REFRESH ALL** menarik ulang semua feed dari browser pengunjung, kapan pun.

## Menambah / mengganti sumber

Edit array `SOURCES` di `index.html`:

```js
{ id:'nama-unik', name:'Nama Ditampilkan', color:'#HEXCOLOR', feed:'URL_RSS_RESMI', home:'URL_NEWSROOM' }
```

Pastikan `feed` adalah URL RSS/Atom **resmi milik perusahaan itu sendiri**, bukan agregator pihak ketiga —
itu prinsip dasar situs ini.

## Catatan tentang proxy CORS publik

Proxy publik gratis (allorigins, corsproxy.io, codetabs) bisa saja rate-limited atau down sewaktu-waktu
karena dipakai banyak orang di internet. Untuk penggunaan serius/produksi, disarankan bikin proxy
sendiri yang ringan (misalnya Cloudflare Worker gratis) supaya tidak bergantung pada layanan pihak ketiga.
Kalau butuh bantuan itu, tinggal minta.
