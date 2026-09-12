# 🐟 Lelang Ikan — Bot Lelang Otomatis via WhatsApp

**Sistem lelang ikan koi yang berjalan sepenuhnya lewat chat WhatsApp** — tanpa aplikasi tambahan, tanpa dashboard rumit. Peserta cukup mengetik pesan biasa untuk menawar, admin mengelola semuanya dari Google Sheets, dan bot menangani sisanya: validasi tawaran, hitung mundur waktu, anti-snipe, sampai rekap final — semua otomatis dan real-time.

Dibangun di atas [n8n](https://n8n.io) (workflow automation) yang terhubung ke Google Sheets sebagai database dan sebuah WhatsApp Gateway untuk mengirim & menerima pesan.

---

## ✨ Fitur Utama

### Untuk Peserta Lelang
- **Menawar langsung dari chat** — format fleksibel, bisa ditulis dua arah (`1 OB` maupun `OB 1`)
- **Jumping bid** ke nominal bebas, atau **kelipatan custom** per tawaran
- **Tawar banyak produk sekaligus**, termasuk mode `ALL` untuk semua produk yang sedang dibuka
- **Buy It Now (BIN)** — beli langsung tanpa menunggu lelang selesai
- Lihat foto, video, sertifikat, dan rekap harga tertinggi kapan saja lewat command sederhana
- **Sambutan otomatis** untuk anggota baru yang bergabung ke grup

### Untuk Admin
- Kelola seluruh jadwal & katalog produk dari **Google Sheets** — tidak perlu sentuh kode
- Buka/tutup sesi lelang lewat command WhatsApp (`MULAI PENAWARAN`, `TUTUP PAKSA`)
- Perpanjang waktu atau batalkan tawaran kapan saja
- Status produk **otomatis terbuka** begitu sesi dimulai — tidak perlu setup manual tiap hari

### Otomatisasi di Balik Layar
- ⏱️ **Countdown otomatis** menjelang buka & tutup sesi
- 🛡️ **Anti-snipe (Extra Time)** — waktu tutup otomatis diperpanjang kalau ada tawaran mendekati detik akhir
- 📊 **Rekap final otomatis** dikirim ke grup begitu sesi selesai
- 👋 **Pelacakan anggota grup** — member baru disambut otomatis, terlepas dari status lelang yang sedang berjalan

---

## 🧱 Arsitektur

```
WhatsApp Group  <──────────►  WhatsApp Gateway API
                                       │
                                       ▼
                              n8n Workflow (otak sistem)
                                       │
                                       ▼
                              Google Sheets (database)
                         ┌─────────┬─────────┬───────────────┐
                         Pengaturan   Produk    Log Bid   Anggota Grup
```

| Komponen | Peran |
|---|---|
| **Google Sheets** | Database utama — jadwal sesi, katalog produk, riwayat tawaran, tracking anggota grup |
| **n8n** | Mesin otomasi — memproses tiap pesan masuk, memvalidasi tawaran, mengirim notifikasi |
| **WhatsApp Gateway** | Jembatan pengiriman & penerimaan pesan WhatsApp |

Seluruh logika sistem ada dalam satu file workflow n8n: [`Bot Lelang Telegram - Fase 2 & 3 Combined (1).json`](./Bot%20Lelang%20Telegram%20-%20Fase%202%20%26%203%20Combined%20(1).json).

---

## 📘 Panduan Penggunaan

Dokumentasi lengkap cara pakai — untuk admin maupun peserta lelang — tersedia di:

**[📄 Panduan-Sistem-Bot-Lelang-WhatsApp.pdf](./Panduan-Sistem-Bot-Lelang-WhatsApp.pdf)**

Isinya mencakup struktur database, daftar lengkap command WhatsApp, dan penjelasan notifikasi otomatis.

---

## 🔐 Setup Kredensial

Workflow ini **tidak menyimpan API key apapun di dalam file** — semua node yang perlu memanggil WhatsApp Gateway membaca key-nya lewat environment variable `WA_API_KEY`, dan koneksi Google Sheets memakai OAuth credential n8n (bukan tertulis di file).

Setelah meng-import file `.json` ini ke instance n8n kamu:

1. Buka **Settings → Environment Variables** di n8n (self-hosted) atau pengaturan variabel di hosting n8n kamu, lalu tambahkan:
   ```
   WA_API_KEY = <apikey WhatsApp Gateway kamu>
   ```
2. Buka node **Google Sheets** manapun di workflow, lalu pasang ulang credential Google OAuth-nya (credential lama tidak ikut ter-export, ini demi keamanan).

Setelah dua langkah itu, workflow langsung berfungsi normal — dan tetap aman untuk terus di-edit, di-export ulang, lalu di-push ke repo publik ini kapan saja, karena key tidak pernah ikut tersimpan di file.

⚠️ Kalau kamu pernah meng-clone/download versi file ini **sebelum** perbaikan ini, API key lama yang sempat tertulis di situ harus dianggap bocor — segera regenerate key baru di dashboard WhatsApp Gateway kamu.
