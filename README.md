# 🤖 Asisten Pencatat Pengeluaran Presales Otomatis

**Final Project - PPKD Jakarta Barat AI Bootcamp (KODE by Hacktiv8)**  
**Author:** Shergy Diardhan (Gio)

---

## 📖 Deskripsi Proyek
Proyek ini adalah sebuah sistem otomatisasi alur kerja (workflow) berbasis AI yang dibangun menggunakan **n8n**. Sistem ini dirancang untuk menyelesaikan masalah pencatatan keuangan manual yang sering dialami oleh tim *Presales* di lapangan. Dengan bot Telegram sebagai antarmuka utama, pengguna dapat mencatat pengeluaran secara *real-time* hanya dengan mengirimkan pesan teks kasual atau mengunggah foto struk belanja.

## 🎯 Latar Belakang Masalah
* Tim *presales* seringkali harus menalangi pembelian barang mendadak (alat demo, konsumsi klien, perlengkapan) menggunakan uang pribadi.
* **Risiko Data Hilang:** Struk fisik mudah pudar, terselip, atau hilang sebelum sempat direkap.
* **Tidak Efisien:** Proses rekapitulasi data ke *spreadsheet* secara manual memakan waktu dan rawan terjadi *human error* (salah ketik nominal, lupa memasukkan pajak/diskon).

## ✨ Fitur Utama
1. 📸 **Smart Receipt Reader (OCR & Parsing):** Mengunggah foto struk ke bot akan otomatis memicu Google Gemini untuk mengekstrak data (tanggal, nama toko, rincian barang, harga, pajak, total) menjadi format JSON terstruktur.
2. 💬 **Natural Language Input:** Pengguna dapat mengetik pengeluaran dengan bahasa santai/slang (contoh: *"Beli 2 pak kertas A4 goceng di fotokopi depan, kena PPN 10%"*). AI Agent otomatis menghitung kalkulasi matematis (termasuk diskon dan pajak) secara presisi.
3. 📊 **Laporan Fleksibel (*On-Demand*):** Dukungan perintah (*command*) untuk menarik rekap data pengeluaran harian, mingguan, bulanan, atau keseluruhan langsung dari Telegram.
4. ✅ **Sistem Approval Interaktif:** Sebelum laporan direkap dan dikirimkan ke atasan, bot Telegram akan mengirimkan tombol konfirmasi persetujuan (Ya/Tidak).
5. 📧 **Automated Email Reporting:** Jika disetujui, laporan rekap akan langsung diteruskan (*forward*) secara otomatis ke Gmail atasan.

## 🛠️ Arsitektur & Teknologi (Tools)
Proyek ini memenuhi syarat teknis integrasi minimal 2 eksternal *tools*, *AI model*, dan implementasi *conditional logic*:
* **Platform Automation:** [n8n](https://n8n.io/) (Menggunakan node *Switch, If, Filter, Summarize*).
* **AI Model:** Google Gemini API (Chat Model & Image Analysis).
* **User Interface:** Telegram Bot API.
* **Database / Penyimpanan:** Google Sheets.
* **Distribusi Laporan:** Gmail.

## ⚙️ Alur Kerja (Workflow)
1. **Trigger:** Pesan atau foto masuk ke Telegram Bot.
2. **Routing (Switch Node):** n8n mendeteksi jenis input (apakah itu foto struk, pesan teks kasual, atau *command* laporan).
3. **Pemrosesan (AI Node):** 
   * Jika **Foto**: Diproses oleh node *Analyze an image* untuk mengekstrak teks.
   * Jika **Teks**: Diproses oleh *AI Agent* dengan *System Prompt* ketat untuk mengonversi teks bebas menjadi JSON.
4. **Penyimpanan:** Data diinjeksi (*Append row*) ke kolom-kolom Google Sheets secara berurutan.
5. **Pelaporan:** Jika user meminta rekap data, node *Summarize* akan menggabungkan data dari Google Sheets, lalu node *Wait for response* menahan alur hingga user menekan tombol setuju di Telegram.
6. **Finalisasi:** Node *If* mengevaluasi jawaban. Jika disetujui, node *Gmail* akan mengirimkan email laporan.

## 🚀 Cara Menjalankan (Prasyarat)
Untuk mengimpor dan menjalankan workflow ini di *instance* n8n kamu sendiri, kamu memerlukan kredensial (*credentials*) berikut:
1. **Telegram Bot Token** (dibuat via BotFather).
2. **Google Gemini API Key** (dari Google AI Studio).
3. **Google OAuth2 Credentials** (untuk akses Google Sheets dan Gmail).
4. Buat sebuah *spreadsheet* di Google Sheets dengan kolom header: `tanggal`, `nama_toko`, `nama_barang`, `harga_satuan`, `kuantitas_barang`, `total_pengeluaran`, `diskon`, `pajak`, dan `total_akhir`.

*(Catatan: File JSON dari workflow ini dapat di-import langsung ke dalam kanvas n8n)*

---
*Dibuat untuk keperluan evaluasi Capstone Project - AI Automation Engineer 2026.*

LINK n8n : https://diardhans.app.n8n.cloud/workflow/Lf1JN9NoKj6gtnKh