# Panduan Instalasi Hermes CLI di Windows PowerShell

Panduan ini merangkum instalasi Hermes CLI native di Windows tanpa WSL. Langkah konfigurasi mengikuti sesi pada Windows 10 Pro x64. Versi Node.js yang terlihat saat itu adalah v26.7.0; keberhasilan startup tidak menjamin semua fitur atau dependency native kompatibel.

Dokumentasi Windows native:
https://github.com/nousresearch/hermes-agent/blob/main/website/docs/user-guide/windows-native.md

## 1. Siapkan PowerShell

Buka PowerShell atau Windows Terminal. Jika terminal masuk ke mode Select, tekan Esc sebelum mengetik perintah.

Panduan sesi ini menggunakan PowerShell pada Windows 10 Pro x64. Jangan anggap seluruh instruksi untuk Linux atau WSL berlaku pada instalasi native Windows.

## 2. Jalankan installer Windows

Perintah installer yang digunakan pada sesi sebelumnya tidak tercatat, sehingga panduan ini tidak menebak atau memberikan perintah instalasi yang belum terverifikasi.

Gunakan instruksi installer Windows native dari dokumentasi Hermes di atas. Baca perintah sebelum menjalankannya dan pastikan sumber unduhan memang proyek yang ingin dipasang.

Jangan menjalankan ulang installer untuk melanjutkan konfigurasi jika Hermes sudah terpasang dan perintah `hermes` sudah dikenali.

## 3. Pastikan CLI terbuka

Di PowerShell, jalankan:

    hermes

Pada sesi sebelumnya, perintah ini membuka antarmuka terminal Hermes dan memulai konfigurasi awal.

Untuk melihat bantuan CLI:

    hermes --help

Jika PowerShell tidak mengenali `hermes`, tutup dan buka kembali terminal terlebih dahulu. Jika tetap tidak dikenali, periksa langkah PATH dan troubleshooting pada dokumentasi Windows native.

## 4. Pilih telemetry

Saat wizard menanyakan telemetry, pilih sesuai preferensi. Pada sesi sebelumnya, telemetry dinonaktifkan dan terminal mengonfirmasi pengaturannya.

## 5. Konfigurasikan provider model

Jika belum ada provider inference, Hermes akan meminta konfigurasi model. Masukkan Base URL, API key, dan model sesuai informasi resmi provider API.

API key dimasukkan langsung di terminal. Jangan tempelkan API key ke chat, dokumentasi publik, screenshot, atau log. Jika key pernah terekspos, cabut atau rotasi melalui dashboard provider.

Untuk mengulang konfigurasi bagian model pada instalasi yang sudah ada:

    hermes setup model

Ikuti pertanyaan yang benar-benar tampil di terminal. Nama atau nomor pilihan provider dapat berubah; pilih berdasarkan label yang terlihat, bukan nomor dari panduan lama.

Untuk endpoint yang menggunakan format OpenAI-compatible standar, pilih “Chat Completions” jika opsi tersebut tersedia. Jangan memilih format Responses atau Anthropic kecuali provider secara eksplisit mendukung format itu.

Masukkan model ID persis seperti yang diberikan provider. Daftar model yang tampil dan model yang diizinkan untuk API key bisa berbeda. Jika Hermes tidak dapat mengambil daftar model, masukkan ID model yang sudah dikonfirmasi oleh provider.

Biarkan panjang konteks kosong untuk auto-detect jika kamu tidak mengetahui batas model. Ini tidak menjamin auto-detect berhasil atau mencerminkan kapasitas maksimum model.

## 6. Uji sesi chat

Setelah wizard selesai, jalankan:

    hermes

Kirim pesan singkat, misalnya “halo”. Startup Hermes saja tidak membuktikan koneksi model berhasil; keberhasilan baru terlihat jika permintaan chat menerima respons.

Jika muncul HTTP 401, periksa API key dan izin key pada dashboard provider. Jika muncul HTTP 402 atau pesan saldo/cadangan token tidak cukup, masalahnya berkaitan dengan saldo atau kebutuhan reservasi request; membuat sesi baru tidak menambah saldo.

## 7. Catatan troubleshooting

- `provider 'auto' has no endpoint configured`: konfigurasi provider atau mode API belum terkait dengan endpoint. Jalankan `hermes setup model`, pilih provider yang benar, dan ikuti menu compatibility yang ditampilkan.
- Gagal mengambil model dari endpoint: belum tentu Base URL salah. Verifikasi Base URL, autentikasi, dan kompatibilitas API berdasarkan petunjuk provider.
- HTTP 401: kredensial ditolak atau tidak memiliki akses. Jangan kirim key ke chat untuk diperiksa.
- HTTP 402: periksa saldo/kuota dan kebutuhan token yang disebutkan dalam pesan error.
- PowerShell tampak tidak merespons setelah klik atau seleksi teks: tekan Esc untuk keluar dari mode Select. Gunakan Ctrl+C hanya jika memang ingin menghentikan proses yang sedang berjalan.

## 8. Lingkup instalasi

Panduan ini untuk Hermes CLI/terminal. Build Web UI tidak diperlukan untuk menggunakan CLI. Pada sesi sebelumnya, CLI berhasil membuka antarmuka terminal, tetapi seluruh fitur dan kompatibilitas native tidak diverifikasi.