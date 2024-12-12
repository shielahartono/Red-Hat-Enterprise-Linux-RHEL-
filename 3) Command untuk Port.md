# Command untuk Port

Pada **Red Hat Enterprise Linux (RHEL)**, port digunakan untuk menghubungkan aplikasi atau layanan ke jaringan. Setiap aplikasi atau layanan yang berjalan pada sistem RHEL membutuhkan port tertentu untuk komunikasi. Ada beberapa cara untuk mengonfigurasi atau memeriksa port yang terbuka di RHEL, salah satunya menggunakan **firewalld**, **iptables**, atau **ss/netstat** untuk memeriksa port yang aktif.

### Berikut penjelasan tentang beberapa perintah terkait port pada RHEL:

### 1. **Memeriksa Port yang Terbuka**
   Untuk memeriksa port yang terbuka pada sistem, Anda dapat menggunakan beberapa perintah:

>> ^For your information^ <br/>
>> Port yang terbuka (open port) merujuk pada port jaringan pada sebuah komputer atau server yang menerima koneksi dari perangkat lain di jaringan, baik itu lokal maupun internet. Port ini memungkinkan aplikasi atau layanan pada sistem untuk berkomunikasi dengan perangkat lain.
>>

   - **`ss`** (Socket Statictics):
     ```bash
     ss -tuln
     ```
     - `-t`: menampilkan port TCP
     - `-u`: menampilkan port UDP
     - `-l`: menampilkan port yang sedang mendengarkan
     - `-n`: menampilkan port dalam format numerik (tanpa nama)

     Contoh hasil output:
     ```bash
     Netstat -tuln
     ```
     Ini akan menunjukkan daftar port yang sedang aktif dan mendengarkan pada sistem.

   - **`netstat`** (Network Statistics) (Jika tersedia):
     ```bash
     netstat -tuln
     ```
     Sama seperti `ss`, perintah ini menunjukkan port yang sedang terbuka. Namun, `netstat` sudah tidak terlalu sering digunakan pada versi terbaru RHEL, dan `ss` lebih disarankan.

### 2. **Mengelola Firewall (firewalld) untuk Port**
   RHEL menggunakan `firewalld` untuk mengelola firewall. Anda bisa membuka atau menutup port tertentu dengan perintah `firewall-cmd`.

   - **Melihat port yang dibuka:**
     Untuk melihat port yang terbuka pada `firewalld`, gunakan perintah:
     ```bash
     firewall-cmd --list-ports
     ```
     Ini akan menampilkan daftar port yang saat ini terbuka.

   - **Membuka Port:**
     Untuk membuka port tertentu, misalnya port 8080, gunakan:
     ```bash
     firewall-cmd --zone=public --add-port=8080/tcp --permanent
     ```
     Penjelasan:
     - `--zone=public`: menentukan zona jaringan, yang biasanya `public` pada banyak sistem.
     - `--add-port=8080/tcp`: menambahkan port TCP 8080.
     - `--permanent`: agar perubahan bertahan setelah reboot. Jika tidak menggunakan opsi ini, perubahan hanya berlaku sementara.
   
     Setelah itu, reload firewall:
     ```bash
     firewall-cmd --reload
     ```

   - **Menutup Port:**
     Untuk menutup port yang sebelumnya dibuka:
     ```bash
     firewall-cmd --zone=public --remove-port=8080/tcp --permanent
     firewall-cmd --reload
     ```

### 3. **Menggunakan Iptables**
   `iptables` adalah alat lain untuk mengelola port dan aturan firewall di RHEL, meskipun `firewalld` lebih umum digunakan di versi terbaru.

   - **Melihat aturan iptables:**
     ```bash
     iptables -L
     ```
     Perintah ini menampilkan aturan firewall yang ada.

   - **Menambah port yang terbuka dengan iptables:**
     Untuk membuka port TCP 8080:
     ```bash
     iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
     ```
     Perintah ini menambahkan aturan untuk menerima koneksi pada port 8080. 

     Untuk membuat perubahan permanen, Anda perlu menyimpan konfigurasi:
     ```bash
     service iptables save
     ```

### 4. **Menampilkan Port dan Layanan dengan `systemctl`**
   Untuk mengetahui layanan yang berjalan dan port yang terkait, Anda bisa menggunakan `systemctl` untuk melihat status layanan.

   - **Melihat status layanan tertentu:**
     ```bash
     systemctl status <nama_layanan>
     ```
     Contoh:
     ```bash
     systemctl status httpd
     ```
     Ini akan menunjukkan apakah layanan seperti **Apache HTTP server** (httpd) berjalan dan pada port mana ia mendengarkan.

### 5. **Perintah lain untuk memeriksa port:**
   - **`lsof`** (List Open Files):
     ```bash
     lsof -i :8080
     ```
     Menampilkan proses yang menggunakan port 8080.
   
   - **`nc`** (Netcat) untuk tes port:
     ```bash
     nc -zv <ip_address> 80
     ```
     Menguji apakah port 80 pada IP tertentu terbuka.

### Ringkasan:

- **ss/netstat** untuk melihat port yang terbuka.
- **firewalld** untuk membuka dan menutup port.
- **iptables** untuk mengelola port menggunakan aturan firewall.
- **systemctl** untuk memeriksa status layanan yang terkait dengan port.
  
Dengan memahami cara mengelola port di RHEL menggunakan berbagai alat ini, Anda bisa memastikan keamanan sistem serta pengaturan port yang tepat sesuai kebutuhan.
