# Panduan Menghubungkan NISA API ke Hermes Agent

Panduan ini menggunakan koneksi langsung:

    Hermes Agent → NISA API → Model

9Router tidak diperlukan untuk jalur ini.

Perintah Hermes mengacu pada README yang diberikan. Integrasi NISA API dengan Hermes belum terverifikasi berhasil; keberhasilan Chat Completions melalui 9Router sebelumnya tidak otomatis menjamin kompatibilitas tool calling Hermes.

## 1. Persiapan

Siapkan:

- Hermes Agent.
- Akun dan API key NISA API.
- Saldo sesuai tier model.
- Base URL kompatibel OpenAI dan ID model dari dashboard NISA API.

Buka [Dashboard NISA API](https://nisa-ai.my.id/api).

Alamat dashboard bukan otomatis Base URL API. Salin Base URL dari dokumentasi atau contoh request pada dashboard.

Catatan koreksi: alamat endpoint, nama model, dan rincian biaya yang disebut pada jawaban sebelumnya belum didukung hasil pembacaan halaman di percakapan ini. Gunakan informasi yang benar-benar tampil pada dashboard sebagai acuan.

## 2. Install Hermes Agent

Jika sudah terpasang, lanjut ke bagian konfigurasi.

### Linux, macOS, atau WSL2

README sumber menyediakan perintah:

    curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash

Perintah ini mengunduh dan langsung menjalankan installer. Untuk memeriksa isinya terlebih dahulu, contoh umum:

    curl -fsSL https://hermes-agent.nousresearch.com/install.sh -o hermes-install.sh
    less hermes-install.sh
    bash hermes-install.sh

Setelah instalasi, muat ulang konfigurasi shell yang digunakan. Untuk Bash:

    source ~/.bashrc

### Windows Native

Jalankan melalui PowerShell, bukan CMD. Perintah dari README sumber:

    iex (irm https://hermes-agent.nousresearch.com/install.ps1)

Perintah tersebut langsung menjalankan installer dari jaringan. Periksa sumber installer terlebih dahulu sebelum menjalankannya.

Menurut README, installer menangani dependency dan memakai Git yang tersedia atau Git Bash portabel.

### Android / Termux

README menyatakan Termux memakai dependency khusus `.[termux]`, bukan `.[all]`, karena sebagian dependency suara tidak kompatibel dengan Android.

Ikuti [panduan resmi Termux Hermes](https://hermes-agent.nousresearch.com/docs/getting-started/termux).

Jangan menerapkan langkah dependency Linux desktop secara sembarang pada Termux.

## 3. Konfigurasi Provider NISA API

Jalankan:

    hermes model

Menurut README, perintah ini digunakan untuk memilih provider dan model.

Cari opsi provider custom atau endpoint kompatibel OpenAI jika tersedia pada versi terpasang. Nama opsi dan urutan pertanyaannya belum terlihat pada README yang diberikan.

Isi konfigurasi berdasarkan data NISA API:

- Provider: koneksi custom kompatibel OpenAI.
- Base URL: salin persis dari dokumentasi NISA API.
- API key: key NISA API milik akun Anda.
- Model: ID model persis dari daftar model NISA API.

Jika diminta nama tampilan provider, gunakan NISA API.

Jangan:

- Memakai alamat dashboard sebagai Base URL.
- Menambahkan path Chat Completions jika kolom meminta Base URL.
- Memasukkan API key 9Router pada koneksi langsung NISA API.
- Memakai nama tampilan model jika API membutuhkan ID yang berbeda.

Jika pilihan custom endpoint tidak tersedia atau kolomnya berbeda, jangan menebak nama config atau environment variable.

Periksa [dokumentasi provider Hermes](https://hermes-agent.nousresearch.com/docs/integrations/providers) dan [konfigurasi Hermes](https://hermes-agent.nousresearch.com/docs/user-guide/configuration) untuk versi terpasang.

## 4. Tes Percakapan Dasar

Mulai Hermes:

    hermes

Kirim:

    Balas hanya: Koneksi NISA API berhasil.

Jika respons diterima tanpa error, itu menunjukkan request percakapan dasar berhasil. Hasil tersebut belum membuktikan tool calling, streaming, atau seluruh fitur agent kompatibel.

Cocokkan request dengan catatan penggunaan di NISA API.

Untuk memeriksa penggunaan dari sisi Hermes, README mencantumkan slash command:

    /usage

Angka penggunaan Hermes tidak harus identik dengan debit penyedia. Jadikan catatan penggunaan NISA API sebagai acuan pemotongan saldo.

## 5. Aktifkan dan Uji Tools Secara Terbatas

Buka konfigurasi tools:

    hermes tools

Aktifkan hanya tools yang diperlukan untuk pekerjaan Anda.

Kemudian jalankan Hermes dari direktori proyek uji. Contoh umum pada Linux, macOS, WSL2, atau Termux:

    mkdir -p ~/hermes-nisa-test
    cd ~/hermes-nisa-test
    hermes

Untuk tes awal, kirim:

    Gunakan tool yang tersedia untuk menampilkan direktori kerja saat ini.
    Jangan membuat, mengubah, atau menghapus berkas.
    Jika perlu menjalankan perintah, minta persetujuan terlebih dahulu.

Periksa apakah:

- Model benar-benar memanggil tool.
- Hermes menampilkan hasil tool.
- Model dapat melanjutkan setelah menerima hasilnya.
- Tidak terjadi error format pesan atau pengulangan tool tanpa akhir.

Respons teks seperti “saya sudah memeriksa” bukan bukti tool dijalankan. Perhatikan keluaran tool di Hermes.

Model yang bisa menjawab chat belum tentu mendukung tool calling sesuai kebutuhan agent.

Pengaturan approval dan pembatasan tools tetap diperlukan; permintaan dalam prompt bukan pengganti kontrol izin.

## 6. Menggunakan Hermes melalui Telegram

Setelah koneksi CLI dan tool calling dasar berhasil, konfigurasi gateway:

    hermes gateway setup

Pilih Telegram jika tersedia, lalu ikuti wizard untuk memasukkan token bot dan membatasi pengguna yang diizinkan.

Token bot Telegram berbeda dari API key NISA API:

- Token Telegram menghubungkan Hermes ke bot.
- API key NISA API digunakan untuk request model.

Mulai gateway sesuai quick reference pada README:

    hermes gateway start

Kirim pesan ke bot dari akun yang diizinkan.

Uji percakapan singkat terlebih dahulu sebelum mengaktifkan terminal, penjadwalan, atau automasi lainnya.

Jangan menjalankan beberapa instance gateway untuk bot yang sama tanpa memahami pengaturan deployment-nya.

## 7. Jika Ingin Tetap Memakai 9Router

Jalur alternatif:

    Hermes Agent → 9Router → NISA API → Model

Pada konfigurasi provider Hermes, gunakan:

- Base URL lokal yang ditampilkan 9Router.
- API key akses 9Router jika autentikasi diaktifkan.
- ID model atau alias yang tersedia melalui 9Router.

API key NISA API tetap dipasang pada upstream NISA API di 9Router.

Jika Hermes dan 9Router berada pada perangkat berbeda, localhost di Hermes merujuk ke perangkat Hermes, bukan perangkat 9Router.

Untuk koneksi lintas perangkat, gunakan jaringan yang dibatasi atau tunnel. Jangan membuka dashboard dan endpoint tanpa autentikasi ke internet.

Untuk setup paling sederhana, mulai dari koneksi langsung NISA API. Gunakan 9Router jika memang memerlukan routing atau pengelolaan beberapa upstream.

## 8. Troubleshooting

### API key ditolak

Periksa:

- Key masih aktif.
- Tidak ada spasi tambahan.
- Kredensial sesuai jalur koneksi langsung atau melalui 9Router.
- Base URL benar.
- Endpoint yang dipilih memang milik layanan yang menerbitkan key.

Jangan mengirim key lengkap ketika membagikan error.

### Model tidak ditemukan

Salin ulang ID model dari daftar NISA API.

Jika melalui 9Router, gunakan ID atau alias yang diekspos 9Router, bukan mengasumsikan selalu sama dengan ID upstream.

### Error 404

Kemungkinan Base URL atau path request tidak sesuai.

Bandingkan dengan contoh request pada dokumentasi NISA API. Jangan menambah atau menghapus segmen path secara acak.

### Chat berhasil, tetapi tools gagal

Kemungkinan model atau jalur API belum kompatibel dengan format tool calling yang digunakan Hermes.

Catat:

- ID model.
- Pesan error lengkap setelah menyamarkan rahasia.
- Apakah koneksi langsung atau melalui 9Router.
- Apakah kegagalan terjadi saat meminta tool atau saat mengirim hasil tool kembali.

Jangan langsung menyimpulkan API key bermasalah jika percakapan dasar sudah berhasil.

### Diagnosis Hermes

README menyediakan:

    hermes doctor

Periksa hasilnya sebelum membagikan karena keluaran diagnosis mungkin memuat informasi lingkungan.

Jangan memperbarui aplikasi atau mengubah banyak pengaturan sekaligus ketika sedang melacak satu masalah.

## 9. Keamanan dan Penggunaan Saldo

- Saldo Reguler dan Premium NISA API terpisah; gunakan tier sesuai model.
- Agent dapat melakukan beberapa request untuk satu tugas.
- Pemanggilan tools, percobaan ulang, dan konteks percakapan dapat menambah penggunaan.
- Pantau saldo sebelum dan sesudah tes.
- Jangan menyimpan key di README atau repository.
- Rotasi kredensial yang pernah terekspos.
- Gunakan direktori proyek khusus dan backup sebelum mengizinkan perubahan berkas.
- Batasi akses gateway ke pengguna yang diizinkan.
- Jangan menambahkan pengecualian antivirus untuk seluruh folder hanya berdasarkan klaim false positive; verifikasi file dan asalnya terlebih dahulu.

## 10. Referensi

- [Dashboard NISA API](https://nisa-ai.my.id/api)
- [Dokumentasi Hermes](https://hermes-agent.nousresearch.com/docs/)
- [Provider Hermes](https://hermes-agent.nousresearch.com/docs/integrations/providers)
- [Konfigurasi Hermes](https://hermes-agent.nousresearch.com/docs/user-guide/configuration)
- [Panduan Termux](https://hermes-agent.nousresearch.com/docs/getting-started/termux)
- [Messaging Gateway](https://hermes-agent.nousresearch.com/docs/user-guide/messaging)
- [Keamanan Hermes](https://hermes-agent.nousresearch.com/docs/user-guide/security)

Tautan di atas berasal dari alamat yang diberikan dalam percakapan dan belum diperiksa ulang.
