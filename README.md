# Sticker Job Tracker (Lajur Artboard)

App ringkas untuk tracking kerja printing sticker atas artboard **60 × 100 cm**, dari order sampai pos, dan tengok target artboard setiap shift. Semua staff dan boss guna satu link, tanpa perlu buat akaun.

Link (selepas GitHub Pages dihidupkan): `https://zulfadilla.github.io/sticker-job-tracker/`

## Aliran kerja

1. **Order**: tampal baris dari Excel (TikTok / Shopee) atau upload CSV.
   Susunan lajur: `No Order · Platform · Nama Pelanggan · Design/Produk · Kuantiti · Nota`
2. **Edit design**: designer tanda *Siap edit* pada order.
3. **Artboard 60×100**: designer buka artboard baru dan masukkan order yang siap edit. Bila penuh, tekan *Penuh, hantar print*.
4. **Print**: operator tanda *Siap print* bila output keluar. Artboard dikira "keluar" pada langkah ini.
5. **Packing**: *Siap potong & bungkus*, kemudian *Dah pos*.

Setiap langkah merekod nama, masa, tarikh dan shift.

| Tab | Fungsi |
| --- | --- |
| Papan | Jumlah kerja di setiap peringkat + progress target shift semasa |
| Order | Import & senarai order, tanda siap edit |
| Artboard 60×100 | Susun order dalam artboard dan gerakkan ikut peringkat |
| Target | Artboard keluar vs target, ikut tarikh & shift |
| Tetapan | Target artboard setiap shift, nama pekerja, nama shift |

## Setup (sekali sahaja, ±10 minit)

### 1. Buat projek Firebase (percuma)

1. Pergi ke https://console.firebase.google.com dan log masuk dengan akaun Google.
2. **Add project** → beri nama (contoh `sticker-tracker`) → Google Analytics boleh dimatikan → **Create project**.
3. Menu kiri **Build → Firestore Database** → **Create database** → pilih lokasi `asia-southeast1 (Singapore)` → **Start in production mode** → **Create**.
4. Dalam Firestore, buka tab **Rules**, padam semua dan tampal isi fail [`firestore.rules`](firestore.rules) → **Publish**.
5. Menu kiri **Build → Authentication** → **Get started** → tab **Sign-in method** → pilih **Anonymous** → **Enable** → **Save**.
6. Klik ikon gear (⚙) → **Project settings** → bahagian **Your apps** → klik ikon web **`</>`** → beri nama → **Register app**.
7. Salin objek `firebaseConfig` yang keluar (apiKey, authDomain, projectId, …).

### 2. Isi config

Buka fail [`firebase-config.js`](firebase-config.js) dalam GitHub (ikon pensel untuk edit), gantikan nilai `ISI_...` dengan config tadi, dan **Commit changes**.

### 3. Hidupkan GitHub Pages

Dalam repo ini: **Settings → Pages** → *Source*: **Deploy from a branch** → Branch **main**, folder **/(root)** → **Save**. Selepas 1–2 minit, link akan aktif.

### 4. Tetapan awal dalam app

Buka link, pergi ke tab **Tetapan**:
- Masukkan nama pekerja (satu baris satu nama) dan nama shift.
- Set target artboard setiap shift.

Kongsi link kepada staff dan boss. Setiap orang pilih nama sendiri di bar atas (telefon akan ingat).

## Nota keselamatan

Sesiapa yang ada link boleh baca dan ubah data. Jangan kongsi link di tempat awam. Jika perlu kawalan lebih ketat (login staff, boss sahaja boleh padam), ia boleh ditambah kemudian.

## Struktur data (Firestore)

- `orders/{id}`: `no, platform, customer, item, qty, note, status (baru|siap), board, editedBy, createdAt`
- `boards/{id}`: `code, stage (susun|print|siap|packed|pos), designer, orders[], stamps{stage:{by,at,date,shift}}, createdAt`
- `config/main`: `workers[], shifts[], target`
