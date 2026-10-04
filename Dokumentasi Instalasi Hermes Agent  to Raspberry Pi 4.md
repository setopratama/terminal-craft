
```markdown
# Dokumentasi Instalasi Hermes Agent

Panduan ini merangkum langkah-langkah instalasi, konfigurasi model gratis (OpenRouter), serta pemecahan masalah (*troubleshooting*) saat memasang Hermes Agent di perangkat berbasis Linux (seperti Armbian, Ubuntu, Debian, atau WSL di Windows).

## 1. Prasyarat Sistem

Sebelum menginstal, pastikan perangkat Anda telah terpasang beberapa utilitas dasar berikut:
* `git` (untuk mengkloning repositori)
* `curl` (untuk mengunduh skrip instalasi)

Jika belum ada, instal melalui *package manager* (misal di Ubuntu/Armbian):
```bash
sudo apt update
sudo apt install git curl

```

## 2. Proses Instalasi Utama

Jalankan skrip instalasi otomatis resmi dari Nous Research langsung di terminal Anda:

```bash
curl -fsSL [https://hermes-agent.nousresearch.com/install.sh](https://hermes-agent.nousresearch.com/install.sh) | bash

```

Skrip ini akan secara otomatis:

* Mengkloning repositori ke direktori konfigurasi (`~/.hermes`).
* Mengunduh manajer paket Python cepat `uv` untuk arsitektur sistem Anda.
* Menyiapkan dependensi lingkungan agen.

Setelah instalasi selesai, muat ulang konfigurasi *shell* Anda agar perintah `hermes` bisa dikenali:

```bash
source ~/.bashrc

```

## 3. Konfigurasi Model AI (Menggunakan OpenRouter Gratis)

Hermes membutuhkan *provider* model sebagai "otak" utamanya. Jika Anda ingin menggunakan opsi bebas biaya menggunakan model gratis dari OpenRouter:

1. **Dapatkan API Key OpenRouter:**
Buat akun di OpenRouter dan ambil kunci API Anda (`sk-or-...`).
2. **Masukkan API Key ke File Environment (`.env`):**
Buka file `.env` melalui editor teks `nano`:
```bash
nano /root/.hermes/.env

```


Tambahkan baris berikut:
```env
OPENROUTER_API_KEY=sk-or-v1-xxxxxxxxxxxxxxxxxxxxxxxx

```


*(Simpan dengan tekan `Ctrl + O`, lalu `Enter`, dan keluar dengan `Ctrl + X`).*
3. **Atur Model Default ke Opsi Gratis:**
Buka file konfigurasi utama:
```bash
nano /root/.hermes/config.yaml

```


Cari bagian `model:`, lalu ubah parameter `default:` menjadi:
```yaml
model:
  default: "openrouter/auto:free"

```


*(Simpan kembali dengan `Ctrl + O` dan keluar dengan `Ctrl + X`).*

## 4. Cara Menjalankan Hermes

* **Masuk ke Mode Chat / TUI (Terminal UI):**
```bash
hermes --tui

```


* **Menjalankan Pesan / Chat CLI Biasa:**
```bash
hermes

```


* **Menjalankan Layanan Integrasi Gateway (Misal untuk Bot Telegram):**
```bash
hermes gateway

```



## 5. Ringkasan Lokasi File Penting

Seluruh direktori dan data konfigurasi Hermes tersimpan secara lokal di folder tersembunyi berikut:

* **Pengaturan Utama:** `/root/.hermes/config.yaml`
* **Kunci API / Token:** `/root/.hermes/.env`
* **Log, Cron, & Sesi Data:** `/root/.hermes/cron/`, `sessions/`, `logs/`

## 6. Pemecahan Masalah (Troubleshooting)

* **Error `-bash: hermes: perintah tidak ditemukan`:**
Pastikan *path* direktori biner lokal sudah masuk ke `PATH` *shell* Anda, atau jalankan perintah `source ~/.bashrc` kembali.
* **Tidak Ada Model yang Dikonfigurasi:**
Pastikan Anda telah mengubah baris `default:` di dalam `config.yaml` menjadi string model yang valid (seperti `openrouter/auto:free`) dan menyertakan `OPENROUTER_API_KEY` yang benar pada file `.env`.

```

```
