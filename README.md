# SUMBER — Direct Wire

Web app satu-halaman yang menampilkan berita **langsung dari newsroom/press-release resmi perusahaan
maupun proyek crypto** — tanpa lewat redaksi seperti Bloomberg atau Reuters. Klik judul mana pun akan
membawamu ke halaman resmi di domain perusahaan/proyek tersebut.

**Sumber teknologi:** AMD, Intel, Samsung, NVIDIA, Ethereum Foundation, Apple, Google, Microsoft, Meta, Amazon.

**Sumber crypto/stablecoin (~55 proyek):** Tether (USDT), BNB Chain, Circle (USDC), Ripple (XRP), Solana,
TRON DAO, Hyperliquid, Dogecoin Foundation, UNUS SED LEO (Bitfinex), Cardano, Monero, Chainlink, Stellar,
Sky/MakerDAO (DAI), Ethena (USDe), Canton Network, Litecoin, Hedera (HBAR), Avalanche, PayPal (PYUSD),
Polkadot, Cosmos, Sui, Aptos, Toncoin, NEAR Protocol, Algorand, Filecoin, Celestia, Injective, Polygon,
Arbitrum, Optimism, Starknet, ZKsync, Zilliqa, The Graph, Kava, Celo, Arweave, Uniswap, Aave, Lido, Curve
Finance, Compound, Synthetix, Sushi, Pendle, THORChain, Balancer, Rocket Pool, GMX, OKX, Bitget, Crypto.com,
Kraken, Coinbase, MetaMask, Ledger.

> Catatan: **Hyperliquid** dan **Canton Network** belum punya blog RSS resmi yang terpisah dari halaman
> pengumuman/media resource-nya — untuk keduanya, `feed` dan `home` mengarah ke halaman yang sama, jadi
> situs akan langsung menampilkan tombol link langsung ke sana alih-alih mencoba parse RSS.

## Soal cakupan "top 500"

Situs ini awalnya diminta mencakup proyek crypto dari ranking top 8 sampai top 500 by market cap.
Setelah riset, ini **tidak realistis untuk sebagian besar proyek di luar top ~100–150**: mayoritas
token di ranking menengah-bawah (meme coin, token exchange kecil, fork, proyek DeFi niche) **tidak
punya newsroom atau blog RSS resmi sama sekali** — komunikasi mereka umumnya hanya lewat X/Twitter,
Discord, atau Telegram, yang tidak punya feed RSS publik yang stabil. Contoh: Kaspa hanya punya wiki
komunitas, tanpa blog resmi terpisah.

Karena situs ini punya prinsip dasar **hanya menautkan ke sumber resmi milik proyek itu sendiri**
(bukan agregator pihak ketiga seperti CoinDesk/Cointelegraph), memaksakan 500 entri akan membuat
sebagian besar daftar berisi tautan yang gagal atau tebakan URL yang tidak terverifikasi — bertentangan
dengan tujuan situs ini.

Sebagai gantinya, daftar `SOURCES` diperluas ke ~55 proyek crypto tambahan yang mencakup mayoritas
top ~100 market cap dan punya blog/RSS resmi yang terverifikasi (L1/L2 besar, protokol DeFi utama,
exchange, dan wallet/infra papan atas).

## Saham Nasdaq-100 & S&P 100

Daftar juga sekarang mencakup ~17 saham blue-chip AS dari indeks Nasdaq-100 dan S&P 100 (di luar yang
sudah ada sejak awal: AMD, Intel, NVIDIA, Apple, Google, Microsoft, Meta, Amazon, PayPal). Sama seperti
crypto, prinsipnya tetap: **hanya tautan resmi milik perusahaan itu sendiri**, bukan agregator berita
seperti Yahoo Finance atau Reuters.

Untuk saham, sebagian besar newsroom perusahaan besar memang ada, tapi **tidak semua punya feed RSS/XML
asli yang bisa diverifikasi** — beberapa newsroom modern hanya me-render halaman lewat JavaScript tanpa
endpoint RSS publik (misalnya Visa, Mastercard, Walmart, JPMorgan Chase, Adobe, Broadcom, Disney, Costco,
AbbVie, Home Depot, Cisco pada penulisan ini). Untuk itu, entri-entri ini memakai pola yang sama dengan
Hyperliquid/Canton Network: `feed` dan `home` mengarah ke halaman newsroom yang sama, sehingga situs
langsung menampilkan tombol link daripada mencoba parse RSS yang tidak ada.

Feed RSS yang **sudah terverifikasi asli** (XML valid, dites langsung): Tesla, Netflix, Qualcomm,
Starbucks, Johnson & Johnson, IBM. Kebanyakan situs IR (`investor.*` atau `ir.*`) perusahaan publik AS
memakai platform "Q4 Inc" dengan pola `[domain-ir]/rss/PressRelease.aspx` — pola ini terbukti valid untuk
beberapa nama di atas, tapi tidak digeneralisasi ke semua nama lain tanpa verifikasi eksplisit, supaya
tidak ada tautan RSS yang menebak dan berisiko 404 diam-diam.

**Kalau kamu mau memperkuat cakupan saham ini** (menambah verifikasi RSS untuk nama yang masih pakai
newsroom-only, atau menambah nama baru dari Nasdaq-100/S&P 100 yang belum ada), coba pola
`[domain-ir-perusahaan]/rss/PressRelease.aspx` atau `/rss-feeds` dulu — kalau valid, ganti `feed` entri
itu ke URL RSS asli; kalau tidak ada, biarkan `feed` sama dengan `home`.

Total sekarang 86 sumber (10 teknologi + ~59 crypto + ~17 saham).

**Kalau kamu mau menambah proyek spesifik di luar daftar ini** (rank 100–500), cara paling aman:
1. Cari apakah proyek itu punya blog resmi di domain sendiri (bukan Medium/Twitter saja) — cek halaman
   "Blog", "News", atau "Press" di situs resminya.
2. Kalau ada, cek apakah ada feed RSS/Atom — coba `/feed`, `/rss.xml`, `/feed.xml`, atau `/rss/` di
   belakang URL blog-nya.
3. Kalau tidak ada RSS sama sekali, tetap bisa ditambahkan dengan `feed` dan `home` mengarah ke URL yang
   sama (seperti pola Hyperliquid/Canton) — situs akan menampilkan tombol link langsung tanpa mencoba
   parse RSS.

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
itu prinsip dasar situs ini. Beberapa proyek crypto hanya punya blog di Medium (bukan domain sendiri) —
itu masih dianggap "resmi" selama akunnya memang dikelola langsung oleh tim proyek (misalnya
`medium.com/feed/nama-proyek-resmi`), bukan akun fan/komunitas.

## Catatan tentang proxy CORS publik

Proxy publik gratis (allorigins, corsproxy.io, codetabs) bisa saja rate-limited atau down sewaktu-waktu
karena dipakai banyak orang di internet. Untuk penggunaan serius/produksi, disarankan bikin proxy
sendiri yang ringan (misalnya Cloudflare Worker gratis) supaya tidak bergantung pada layanan pihak ketiga.
Kalau butuh bantuan itu, tinggal minta.
