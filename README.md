# Sticker Job Tracker (Lajur Artboard)

App ringkas untuk tracking kerja printing sticker atas artboard **60 × 100 cm**, dari order sampai pos, dan tengok target artboard setiap shift.

## Aliran kerja

1. **Order** – tampal baris dari Excel (TikTok / Shopee) atau upload CSV.
   Susunan lajur: `No Order · Platform · Nama Pelanggan · Design/Produk · Kuantiti · Nota`
2. **Edit design** – designer tanda *Siap edit* pada order.
3. **Artboard 60×100** – designer buka artboard baru, masukkan order yang siap edit. Bila penuh: *Penuh, hantar print*.
4. **Print** – operator tanda *Siap print* bila output keluar. Artboard dikira "keluar" pada langkah ini.
5. **Packing** – *Siap potong & bungkus*, kemudian *Dah pos*.

Setiap langkah merekod nama, masa, tarikh dan shift.

## Tab

| Tab | Fungsi |
| --- | --- |
| Papan | Jumlah kerja di setiap peringkat + progress target shift semasa |
| Order | Import & senarai order, tanda siap edit |
| Artboard 60×100 | Susun order dalam artboard dan gerakkan ikut peringkat |
| Target | Artboard keluar vs target, ikut tarikh & shift |
| Tetapan | Target artboard setiap shift, nama pekerja, nama shift |

## Cara guna

`index.html` dibina sebagai **claude.ai Artifact** dan simpan data dalam database kongsi artifact (`db` capability).
Versi live: https://claude.ai/artifact/69Fwbtd26j2WiA8jvC5Ugu

Jika `index.html` dibuka terus dalam browser biasa, UI akan keluar tetapi data tidak boleh disimpan (database hanya tersedia dalam claude.ai). Untuk jalan sendiri di luar claude.ai, lapisan data (`claude.use("db")`) perlu ditukar kepada backend lain seperti Firebase atau Supabase.

### Struktur data

- `orders/{id}` – `no, platform, customer, item, qty, note, status (baru|siap), board, editedBy, createdAt`
- `boards/{id}` – `code, stage (susun|print|siap|packed|pos), designer, orders[], stamps{stage:{by,at,date,shift}}, createdAt`
- `config/main` – `workers[], shifts[], target`
