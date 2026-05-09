# Panduan Instalasi Eka Dashboard

Selamat datang di panduan instalasi **Eka Dashboard**. Panduan ini akan membantu Anda memasang dan menjalankan dashboard pada server atau mesin target Anda (STB, Mini PC, VPS, atau Local Server) yang berbasis sistem operasi Linux.

---

## 1. Persiapan Sistem
Sebelum memulai instalasi, pastikan Anda telah menyiapkan hal-hal berikut:
1. **Perangkat Target**: PC/Laptop/Mini PC/STB dengan OS Linux (Ubuntu, Debian, Armbian, atau OpenWRT dengan fitur Docker).
2. **Akses Terminal/SSH**: Aplikasi seperti PuTTY, PowerShell, atau Terminal bawaan untuk mengakses server Anda.
3. **Koneksi Internet**: Diperlukan untuk mengunduh *dependencies* dan image Docker.

---

## 2. Proses Instalasi (Otomatis)

Dashboard ini dilengkapi dengan *script installer* otomatis (`install.sh`) yang akan mengonfigurasi Docker, memetakan port, dan menjalankan container secara praktis.

### Pilihan A: Instalasi via Git Clone (Rekomendasi)
Jika server Anda sudah terpasang `git`, Anda dapat langsung mengkloning repositori ini:

```bash
# 1. Update sistem dan install git
sudo apt-get update && sudo apt-get install -y git

# 2. Clone repositori Eka Dashboard
git clone https://github.com/ekahr11/web_server.git eka_dashboard

# 3. Masuk ke direktori
cd eka_dashboard

# 4. Eksekusi Script Instalasi
sudo bash install.sh
```

### Pilihan B: Instalasi via File ZIP
Jika Anda memiliki file `.zip` (misalnya `eka_dashboard_retail.zip`):
1. **Upload** file `.zip` tersebut ke server Anda (menggunakan SCP, WinSCP, atau FTP).
   ```bash
   scp eka_dashboard_retail.zip root@IP_SERVER_ANDA:/root/
   ```
2. **Ekstrak dan Install** melalui SSH:
   ```bash
   # Install unzip jika belum ada
   sudo apt-get update && sudo apt-get install -y unzip
   
   # Ekstrak file
   unzip eka_dashboard_retail.zip -d eka_dashboard
   
   # Masuk ke folder & Install
   cd eka_dashboard
   sudo bash install.sh
   ```

---

## 3. Apa yang Dilakukan Script Installer?
Saat Anda menjalankan `sudo bash install.sh`, sistem akan melakukan beberapa hal:
- Mengecek dan memasang **Docker** (serta Docker Compose) secara otomatis jika belum ada.
- Memeriksa **Port 80**. Jika server Anda menggunakan *CasaOS* di port 80, port CasaOS akan dipindahkan otomatis ke `9999` untuk menghindari bentrok.
- Membangun dan menjalankan *container* Eka Dashboard secara otomatis.

Jika instalasi sukses, Anda akan melihat pesan seperti:
> **INSTALASI SUKSES!**  
> Silakan akses Dashboard Admin di: `http://<IP_SERVER_ANDA>`

---

## 4. Aktivasi Lisensi & Login Pertama
1. **Akses Dashboard**: Buka *browser* pada perangkat apa saja yang terhubung di jaringan yang sama, lalu masukkan IP Server Anda (misal `http://192.168.1.10`).
2. **Hardware ID**: Pada akses pertama, layar akan terkunci dengan pesan **Activation Required**. Salin **Hardware ID** Anda yang tertampil di layar.
3. **Mendapatkan License Key**: Hubungi pengelola atau Bot Aktivasi untuk menukarkan Hardware ID Anda dengan *License Key*.
4. **Setup Wizard**: Setelah lisensi dimasukkan dan valid, Anda akan diarahkan ke *Setup Wizard*. Buat **Password Admin** baru Anda.
5. **Login**: Masuk menggunakan password admin tersebut. Dashboard kini siap digunakan!

---

## 5. FAQ & Troubleshooting

**Tanya: Server saya menggunakan CasaOS, apakah akan bermasalah?**
**Jawab:** Tidak. Script installer otomatis mendeteksi CasaOS dan akan memindahkan port CasaOS dari `80` menjadi `9999`. Anda dapat mengakses CasaOS kembali di `http://IP-SERVER:9999`, sementara Eka Dashboard menggunakan port `80`.

**Tanya: Bagaimana cara mengganti Port Eka Dashboard agar tidak menggunakan Port 80?**
**Jawab:**
1. Masuk ke direktori instalasi: `cd /root/eka_dashboard`
2. Buka dan edit file `docker-compose.yml`:
   ```bash
   nano docker-compose.yml
   ```
3. Ubah bagian `ports:` dari `- "80:5000"` menjadi port yang diinginkan (misal `- "8080:5000"`).
4. Simpan perubahan (Ctrl+X, ketik Y, lalu Enter).
5. Terapkan perubahan: `docker compose up -d`
6. Akses dashboard di port baru: `http://IP-SERVER:8080`

**Tanya: Saya lupa Password Admin, bagaimana cara me-resetnya?**
**Jawab:**
Hapus file konfigurasi keamanan, lalu *restart container*.
```bash
cd /root/eka_dashboard
sudo rm data/security_config.json
sudo docker compose restart
```
Buka kembali dashboard di *browser*, sistem akan meminta Anda membuat password baru.
