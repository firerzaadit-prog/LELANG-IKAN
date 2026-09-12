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

## 🔐 Catatan Keamanan

Workflow ini terhubung ke kredensial pihak ketiga (Google Sheets, WhatsApp Gateway). **Jangan pernah meng-commit API key atau kredensial asli** ke repository publik ini — gunakan environment variable atau credential store n8n untuk itu.
