# System Dashboard - User Manual

Terima kasih telah membeli **System Dashboard**.
Berikut adalah panduan lengkap instalasi dan penggunaan.

## 1. Download & Persiapan
Silakan download file paket instalasi melalui link berikut:
**Download Link**: [Klik Disini untuk Download (Google Drive)](https://drive.google.com/file/d/1UMYgX7CV0tHk2AOH1uOQJNUMnd-O9Y_q/view?usp=drive_link)

**Persiapan Alat:**
1.  PC/Laptop (Windows/Mac/Linux).
2.  Server/STB/Mini PC target yang sudah terinstall Linux (Ubuntu/Debian/Armbian) atau OpenWRT (dengan Docker).
3.  Aplikasi Terminal (Putty, Powershell, atau Terminal bawaan).

## 2. Instalasi (Otomatis)
Dashboard ini dilengkapi dengan script installer otomatis yang akan mengurus semuanya (install Docker, konfigurasi Port, dll).

### Langkah 1: Upload File
Upload file `eka_dashboard_retail.zip` ke server Anda menggunakan SCP atau WinSCP.
Contoh (via Terminal):
```bash
scp eka_dashboard_retail.zip root@IP_SERVER_ANDA:/root/
```

### Langkah 2: Ekstrak & Install
Masuk ke server via SSH, lalu jalankan perintah berikut:

```bash
# 1. Install Unzip (jika belum ada)
apt-get update && apt-get install -y unzip

# 2. Ekstrak file
unzip eka_dashboard_retail.zip -d eka_dashboard

# 3. Masuk folder
cd eka_dashboard

# 4. Jalankan Installer
sudo bash install.sh
```

Tunggu hingga proses selesai. Installer akan otomatis:
*   Mengecek & install Docker.
*   Memastikan Port 80 kosong (Jika ada CasaOS, akan dipindah otomatis ke Port 9999).
*   Menjalankan Dashboard.

Jika sukses, akan muncul pesan:
> **INSTALASI SUKSES!**  
> Silakan akses Dashboard Admin di: `http://192.168.x.x`

## 3. Aktivasi Lisensi
Demi keamanan, server Anda akan terkunci saat pertama kali dibuka.
1.  Buka browser dan akses IP server Anda (misal `http://192.168.1.10`).
2.  Anda akan melihat halaman **Activation Required**.
3.  Salin **Hardware ID** yang muncul di layar.
4.  Hubungi Bot Aktivasi kami: [Klik Disini untuk Chat Bot](https://t.me/getreadyalert_bot)
5.  Kirim pesan dengan format:
    `/license <HARDWARE-ID> <ORDER-ID>`
    *(Order ID bisa dilihat di email pembelian Lynk.id)*
6.  Bot akan memberikan **License Key**.
7.  Masukkan Key ke kolom yang tersedia -> Klik **Activate**.

## 4. Login Pertama Kali
Setelah aktivasi berhasil, Anda akan diarahkan ke **Setup Wizard** (jika baru pertama install).
1.  Buat **Password Admin** baru Anda.
2.  Login menggunakan password tersebut.
3.  Selamat! Dashboard siap digunakan.

## FAQ & Troubleshooting

**Q: Saya pakai CasaOS, kok tidak bisa dibuka setelah install ini?**
A: Installer kami otomatis memindahkan CasaOS ke Port 9999 agar Port 80 bisa dipakai Dashboard. Silakan akses CasaOS di `http://IP-SERVER:9999`.

**Q: Saya ingin mengubah Port Dashboard (jangan 80).**
A: Bisa. Ikuti langkah ini:
1.  Masuk ke folder instalasi: `cd /root/eka_dashboard`
2.  Edit file docker-compose: `nano docker-compose.yml`
3.  Cari bagian `ports:`
    ```yaml
    ports:
      - "80:5000"  <-- Ubah 80 menjadi port yang diinginkan (misal 8080)
    ```
4.  Simpan (Ctrl+X, Y, Enter).
5.  Restart container: `docker compose up -d`
6.  Akses di `http://IP-SERVER:8080`.

**Q: Lupa Password?**
A: Hapus file `data/security_config.json` di dalam folder instalasi, lalu restart container. Password akan reset ke default.
