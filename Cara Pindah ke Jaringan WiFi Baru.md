# Cara Pindah ke Jaringan WiFi Baru (menggunakan wpa_supplicant)

## Prasyarat
- Interface WiFi biasanya bernama `wlp3s0` (lihat dengan `ip link`).
- Pastikan `wpa_supplicant` berjalan (biasanya aktif secara otomatis).

## Langkah-langkah

### 1. Scan jaringan WiFi di sekitar
```bash
sudo wpa_cli -i wlp3s0 scan
```

### 2. Lihat hasil scan
```bash
sudo wpa_cli -i wlp3s0 scan_results
```
Catatan: Output akan menunjukkan BSSID, frekuensi, tingkat sinyal, dan SSID.

### 3. Tambahkan network baru ke konfigurasi wpa_supplicant
```bash
sudo wpa_cli -i wlp3s0 add_network
```
Perintah ini akan mengembalikan sebuah `network_id` (misalnya `0`, `1`, `2`, ...). Catat id tersebut.

### 4. Set SSID jaringan baru
```bash
sudo wpa_cli -i wlp3s0 set_network <network_id> ssid '"NAMA_WIFI_BARU"'
```
- Ganti `<network_id>` dengan id yang didapat dari langkah 3.
- Nama WiFi (SSID) harus diapit dengan tanda kutip ganda dua kali: `"NAMA_WIFI_BARU"`.

### 5. Set password (jika menggunakan WPA/WPA2-PSK)
```bash
sudo wpa_cli -i wlp3s0 set_network <network_id> psk '"PASSWORD_WIFI_BARU"'
```
- Ganti `PASSWORD_WIFI_BARU` dengan password sebenarnya.
- Jika jaringan terbuka (tanpa password), gunakan:
  ```bash
  sudo wpa_cli -i wlp3s0 set_network <network_id> key_mgmt NONE
  ```

### 6. Aktifkan network tersebut
```bash
sudo wpa_cli -i wlp3s0 enable_network <network_id>
```

### 7. Simpan konfigurasi agar tetap setelah reboot
```bash
sudo wpa_cli -i wlp3s0 save_config
```

### 8. Paksa koneksi ulang (reassociate)
```bash
sudo wpa_cli -i wlp3s0 reassociate
```

### 9. Verifikasi koneksi baru
```bash
ip addr show wlp3s0   # cek IP address baru
sudo wpa_cli -i wlp3s0 status   # lihat status koneksi
```

## Tips & Troubleshooting
- **Permission denied**: Pastikan kamu menjalankan perintah dengan `sudo` atau sebagai root.
- **SSID dengan spasi atau karakter khusus**: Selalu pakai kutip ganda dua kali seperti contoh di atas.
- **Mengalihkan ke jaringan lain tanpa memutus**: Kamu bisa menambahkan multiple network; wpa_supplicant akan otomatis memilih yang terbaik berdasarkan prioritas.
- **Melihat daftar network yang sudah disimpan**:
  ```bash
  sudo wpa_cli -i wlp3s0 list_networks
  ```
- **Menghapus network yang tidak diperlukan**:
  ```bash
  sudo wpa_cli -i wlp3s0 remove_network <network_id>
  ```

## Contoh lengkap
Misalkan SSID baru adalah `RumahKu` dan passwordnya `rahasia123`:
```bash
sudo wpa_cli -i wlp3s0 scan
sudo wpa_cli -i wlp3s0 scan_results
# ambil network_id, misalnya 2
sudo wpa_cli -i wlp3s0 add_network   # misalnya mengembalikan 2
sudo wpa_cli -i wlp3s0 set_network 2 ssid '"RumahKu"'
sudo wpa_cli -i wlp3s0 set_network 2 psk '"rahasia123"'
sudo wpa_cli -i wlp3s0 enable_network 2
sudo wpa_cli -i wlp3s0 save_config
sudo wpa_cli -i wlp3s0 reassociate
```

Semoga membantu! Jika ada kendala, beri tahu saya nama SSID dan password-nya (atau tanya lebih spesifik) dan saya bisa menjalankan perintah langsung.
