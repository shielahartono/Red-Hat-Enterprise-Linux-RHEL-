# Linux for Performance Optimization

Berikut ini Penggnaan Linux untuk "Performance Optimization" dan Cara meng-analisa nya :

# A. Analyze System Resource Usage and Identify Performance Bottlenecks related to CPU, Memory, disk I/O, and Network Usage
 (Meng-analisa penggunaan "System Resource" dan Identifikasi Kendala Kinerja yang berhubungan dengan CPU, Memory, Disk I/O, dan Network Usage )
----
# 1. `top` :
## Menggunakan Command Linux `top`

Command `top` menunjukkan informasi real-time tentang penggunaan CPU, memori, proses yang berjalan.

## (1) Jalankan Command `top` :
```
top
```
Setelah itu, kita akan melihat output yang terus diperbarui secara otomatis, yang menunjukkan proses yang sedang berjalan di sistem kita.

## (2) Contoh Output untuk Command `top` :
```
top - 15:30:12 up 10 days,  3:45,  3 users,  load average: 0.12, 0.05, 0.01
Tasks: 182 total,   1 running, 181 sleeping,   0 stopped,   0 zombie
%Cpu(s):  5.2 us,  3.4 sy,  0.0 ni, 91.1 id,  0.2 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   8000.0 total,   4123.3 free,   2201.5 used,   1675.2 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   5332.0 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
  763 root      20   0  151792  15236   8056 S   9.5  0.2   0:12.47 apache2
 1821 john      20   0  215680  23456  14768 S   5.3  0.3   0:04.20 mysql
  452 root      20   0  120112  10120   7968 S   2.5  0.1   0:01.78 systemd
  312 root      20   0  132400  12340   9204 S   1.1  0.1   0:00.78 sshd
 1563 john      20   0  210124  20456  13024 S   0.8  0.1   0:03.12 gnome-shell
  589 root      20   0   58656   4468   3124 S   0.3  0.1   0:00.34 rsyslogd
 1297 root      20   0   22048   2132   1836 S   0.2  0.0   0:00.15 crond
  314 root      20   0  106576   7544   6124 S   0.1  0.1   0:01.13 apache2
  781 root      20   0   50064   2688   2176 S   0.1  0.0   0:00.09 cron
 1805 root      20   0   49840   2520   2164 S   0.1  0.0   0:00.04 sshd

```

## (3) Penjelasan Output untuk Command `top` :

### A. System Information :
### (Informasi Sistem)

```
top - 15:30:12 up 10 days, 3:45, 3 users, load average: 0.12, 0.05, 0.01
```

[-] Waktu :
```
15:30:12
```
menunjukkan Waktu saat ini
<br/> <br/>

[-] Uptime :
```
up 10 days, 3:45
```
menunjukkan bahwa System telah berjalan selama :
- 10 hari
- 3 jam
- 45 menit
<br/> <br/>

[-] Users :  <br/>
ada 3 User (pengguna) yang sedang Login di System. <br/><br/>


[-] Load Average : 
```
load average: 0.12, 0.05, 0.01
```
-> Load average adalah ukuran seberapa sibuk sistem tersebut, dan semakin tinggi angkanya, semakin banyak proses yang antri untuk mendapatkan waktu CPU. <br/>

-> "Load Average" adalah angka yang menunjukkan rata-rata jumlah proses yang sedang aktif atau menunggu untuk diproses oleh CPU dalam waktu tertentu, <br/>
yaitu biasanya 1 menit terakhir, 5 menit terakhir, dan 15 menit terakhir. <br/>

-> Pada contoh diatas, dapat kita ketahui :
- Pada "1 menit" terakhir  ->  nilai "Load Average" adalah 0.12
- Pada "5 menit" terakhir  ->  nilai "Load Average" adalah 0.05
- Pada "15 menit" terakhir  ->  nilai "Load Average" adalah 0.01

-> Nilai load average dihitung berdasarkan jumlah proses yang "sedang berjalan" (running) atau Jumlah Proses yang sedang "menunggu giliran" untuk diproses oleh CPU. <br/>

-> Jika sistem memiliki lebih banyak proses daripada jumlah CPU, proses-proses itu harus mengantri untuk diproses, sehingga nilai load average akan meningkat. <br/>

-> Contoh :
- `0.12` : berarti rata-rata hanya 0.12 proses aktif dalam 1 menit terakhir. Karena angka ini jauh di bawah 1, maka CPU tidak sibuk (hampir idle).
- `1.00` : berarti CPU bekerja penuh, dengan rata-rata 1 proses aktif setiap saat.
- `2.00` : berarti rata-rata ada 2 proses aktif, tetapi hanya 1 yang bisa diproses, sementara yang lain menunggu.


-> Misalnya, Jika sistem Anda memiliki 4 CPU, maka load average yang ideal adalah di bawah 4. Misalnya, jika load average Anda adalah 2, itu berarti bahwa rata-rata ada 2 proses yang sedang menunggu atau menggunakan CPU pada saat yang bersamaan. Sistem masih cukup mampu menangani lebih banyak proses tanpa masalah. <br/>
Jika load average Anda lebih tinggi dari jumlah CPU, misalnya 5 atau 6, ini berarti sistem Anda terlalu sibuk dan proses-proses akan mulai menunggu lebih lama untuk mendapatkan waktu CPU, yang dapat menyebabkan penurunan kinerja.

-> Kesimpulan :
- **Load average** yang lebih kecil dari jumlah CPU fisik berarti sistem Anda cukup longgar dan dapat menangani lebih banyak pekerjaan tanpa masalah.
- **Load average** yang lebih besar dari jumlah CPU fisik menunjukkan bottleneck dan kemungkinan penurunan kinerja karena CPU tidak dapat menangani banyak proses secara bersamaan.


### B. Statistik CPU :
```
%Cpu(s):  5.2 us,  3.4 sy,  0.0 ni, 91.1 id,  0.2 wa,  0.0 hi,  0.0 si,  0.0 st

```
-> Penjelasan Secara Garis besar (Overview) :

- `us` (user): Persentase waktu CPU yang digunakan oleh aplikasi pengguna (5.2%).
- `sy` (system): Persentase waktu CPU yang digunakan oleh proses sistem (3.4%).
- `ni` (nice): Persentase waktu CPU yang digunakan oleh proses dengan prioritas modifikasi (0%).
- `id` (idle): Persentase waktu CPU yang tidak digunakan (91.1%). Angka ini menunjukkan bahwa CPU tidak sibuk.
- `wa` (wait): Waktu CPU yang sedang menunggu operasi I/O (0.2%). Ini bisa mengindikasikan adanya masalah pada disk atau jaringan.
- `hi` , `si` , `st` : Ini adalah kategori CPU lainnya, tetapi tidak sering digunakan.

<br/> <br/>

-> Penjelasan Detail :



#### **1. `us` (user) - Waktu untuk Aplikasi Pengguna**  
- `us` berarti "User"

- **Apa itu?**  
   Ini adalah **persentase waktu CPU** yang digunakan oleh **aplikasi atau program** yang User jalankan.  
   Contohnya, membuka browser, Video Player, atau menjalankan program Microsoft Word.

- **Penjelasan Sederhana:**  
   Kalau angka ini tinggi, berarti CPU Anda sibuk menjalankan **program-program yang Kita (User) buka sendiri**.

- **Contoh:**  
   Jika `us = 10%`, artinya **10% dari kekuatan CPU** digunakan untuk program yang Anda jalankan.

>> ^For your Information^
>> ### Persentate Waktu CPU :
>> ### (apa maksud "Persentate Waktu CPU")
>> #### Contoh : <br/>
>>  Jika `us = 10%` , artinya:  <br/>
>> 10% dari waktu CPU digunakan untuk menjalankan program yang dibuka oleh User , seperti browser, Video Player, atau Text Editor. <br/>
>> Jadi, dalam periode pengukuran (misalnya 1 detik), CPU menghabiskan 10% waktunya untuk memproses program pengguna. <br/>  <br/>
>>
>> #### Bagaimana CPU membagi waktu?
>> Misalkan Anda memiliki CPU yang berjalan selama 1 detik: <br/>
>> 10% us → 0,1 detik digunakan untuk program yang Anda jalankan sendiri. <br/>
>> 3% sy (system) → 0,03 detik digunakan untuk proses sistem. <br/>
>>  87% id (idle) → 0,87 detik CPU tidak bekerja karena tidak ada tugas tambahan. <br/> <br/>
>>
>> #### Mengapa dalam persentase?
>> Mengukur dalam persentase memudahkan untuk mengetahui seberapa sibuk CPU dalam menangani tugas-tugasnya. <br/>
>> Jika us tinggi, berarti CPU lebih sibuk menjalankan aplikasi Anda. <br/> 
>> Jika id (idle) tinggi, berarti CPU sebagian besar sedang menganggur dan tidak memiliki banyak pekerjaan. <br/>


#### **2. `sy` (system) - Waktu untuk Sistem**  
- `sy` berarti "System"

- **Apa itu?**  
   Ini adalah **persentase waktu CPU** yang digunakan untuk **proses sistem atau kernel**. Kernel adalah inti dari sistem operasi yang mengatur semua aktivitas komputer.

- **Penjelasan Sederhana:**  
   Kalau angka ini naik, berarti CPU sedang sibuk mengurus pekerjaan **internal sistem**, seperti membaca file, mengelola memori, atau melakukan operasi di latar belakang.

- **Contoh:**  
   Jika `sy = 15%`, itu artinya **15% dari CPU** digunakan untuk menjalankan tugas-tugas sistem.

>> ^For your Information^
>> ## Proses Sistem atau Kernel :
>> 
>>  **Proses Sistem atau Kernel** adalah tugas-tugas yang dijalankan oleh sistem operasi, khususnya oleh **kernel**, yang merupakan inti dari sistem operasi.
>> <br/>  <br/>
>> Kernel bertugas mengatur bagaimana Hardware (seperti CPU, memori, dan perangkat I/O) berinteraksi dengan Software (program dan aplikasi). 
>> 
>> Dengan kata lain, **kernel** berfungsi sebagai jembatan antara **perangkat keras** komputer dan **program-program** yang Anda jalankan.
>> 
>> 
>> 
>> ### **Apa itu Proses Sistem atau Kernel?**
>> 
>> - **Kernel** adalah bagian dari sistem operasi yang **selalu aktif** dan berjalan di latar belakang.  
>> - Kernel menangani tugas-tugas penting, seperti:  
>>   - **Mengatur CPU** untuk menjalankan proses.  
>>   - **Mengelola memori** agar program dapat berjalan.  
>>   - **Mengatur perangkat input/output (I/O)**, seperti membaca data dari hard disk atau mengirim data ke printer.  
>>   - **Menangani keamanan**, seperti memberikan izin akses ke file atau program.  
>>   - **Menjadwalkan proses** (membagi waktu CPU di antara proses-proses yang berjalan).  
>> 
>> 
>> ### **Tugas-tugas yang Termasuk Proses Sistem atau Kernel**
>> 
>> Berikut ini adalah beberapa **contoh task (tugas)** yang dilakukan oleh kernel atau proses sistem:
>> 
>> 1. **Manajemen Proses**  
>>    - Kernel memastikan setiap program atau proses mendapatkan giliran waktu CPU.  
>>    - Contoh: Kernel menunda satu proses sejenak agar proses lain bisa berjalan.
>> 
>> 2. **Manajemen Memori**  
>>    - Kernel mengatur bagaimana memori RAM dibagi-bagi ke berbagai proses yang berjalan.  
>>    - Contoh: Kernel menyediakan ruang memori untuk aplikasi yang Anda buka, dan membebaskannya saat aplikasi ditutup.
>> 
>> 3. **Operasi Input/Output (I/O)**  
>>    - Kernel menangani operasi yang berhubungan dengan perangkat keras, seperti membaca file dari hard disk, menulis data, atau menangani keyboard dan mouse.  
>>    - Contoh: Saat Anda membuka file, kernel bertugas membaca file dari hard disk dan menampilkannya di layar.
>> 
>> 4. **Manajemen File System**  
>>    - Kernel mengatur bagaimana file disimpan, dibaca, dan dimodifikasi di sistem penyimpanan (seperti SSD atau hard disk).  
>>    - Contoh: Ketika Anda menyimpan dokumen, kernel memastikan data ditulis ke lokasi yang tepat.
>> 
>> 5. **Jaringan**  
>>    - Kernel menangani komunikasi data melalui jaringan (internet atau lokal).  
>>    - Contoh: Kernel mengatur bagaimana data dikirim atau diterima saat Anda membuka halaman web atau mengunduh file.
>> 
>> 6. **Keamanan dan Akses Kontrol**  
>>    - Kernel memeriksa izin akses ke file, direktori, atau perangkat keras untuk menjaga keamanan sistem.  
>>    - Contoh: Kernel membatasi akses pengguna biasa agar tidak bisa mengubah file sistem.
>> 
>> 7. **Interrupt Handling**  
>>    - Kernel menangani sinyal "interupsi" dari perangkat keras. Ini terjadi ketika perangkat keras memerlukan perhatian CPU.  
>>    - Contoh: Ketika Anda menekan tombol keyboard, perangkat keras mengirim sinyal interupsi, dan kernel akan menangani input itu.
>> 


### **Contoh Nyata dari Task Proses Sistem**  

1. **Ketika Anda Menyalakan Komputer**  
   - Kernel mengatur proses booting dan memastikan sistem operasi dimuat ke memori.

2. **Ketika Anda Membuka File**  
   - Kernel membaca file dari hard disk dan menyediakan datanya ke aplikasi yang membutuhkannya.

3. **Ketika Anda Menjalankan Program**  
   - Kernel memberikan alokasi CPU dan memori agar program dapat berjalan dengan baik.

4. **Ketika Anda Mengunduh File**  
   - Kernel mengatur transfer data dari jaringan ke perangkat penyimpanan.

5. **Ketika Anda Menekan Tombol Keyboard**  
   - Kernel menerima input dari keyboard dan meneruskannya ke program yang sedang Anda gunakan.

6. **Ketika Banyak Program Berjalan Sekaligus**  
   - Kernel membagi waktu CPU di antara program-program tersebut agar semuanya berjalan dengan lancar.



### **Kesimpulan Sederhana**  
- **Proses Sistem atau Kernel** adalah tugas-tugas inti yang dijalankan oleh sistem operasi untuk memastikan semua komponen komputer bekerja dengan baik.  
- Kernel melakukan pekerjaan "di balik layar" seperti mengatur CPU, memori, I/O, dan keamanan.  
- Jika **`sy`** (system) tinggi, itu berarti CPU sedang sibuk melakukan tugas-tugas sistem, seperti membaca file, menangani input/output, atau membagi waktu CPU ke banyak proses.

#### **3. `ni` (nice) - Waktu untuk Proses Prioritas Rendah**  
- **Apa itu?**  
   Ini adalah **persentase waktu CPU** yang digunakan untuk proses dengan **prioritas rendah**. Prioritas bisa diatur dengan perintah "nice" di Linux.

- **Penjelasan Sederhana:**  
   Jika ada program yang tidak mendesak (misalnya download file besar), kita bisa atur prioritasnya jadi rendah agar tidak mengganggu program lain. CPU akan mengerjakan ini **jika ada waktu luang**.

- **Contoh:**  
   Jika `ni = 2%`, artinya **2% dari CPU** digunakan untuk program dengan prioritas rendah.

>> ^for your information^
>>
>> **Proses Prioritas Rendah** merujuk pada proses yang tidak memerlukan banyak perhatian dari CPU atau proses yang tidak mendesak untuk segera diselesaikan. 
>> 
>> ### Penjelasan Sederhana:
>> - **Prioritas Rendah** berarti proses tersebut bisa dijalankan ketika CPU tidak sibuk mengerjakan tugas-tugas yang lebih penting.
>> - CPU akan memberi **waktu** untuk proses dengan prioritas rendah hanya ketika tidak ada proses penting yang perlu dijalankan.
>> - Ini menghindari gangguan pada aplikasi atau proses yang lebih penting (misalnya, program yang Anda gunakan saat itu).
>> - Di Linux, Anda bisa mengatur prioritas proses menggunakan perintah `nice`, yang memberi tahu sistem seberapa penting atau tidak penting sebuah proses dibandingkan dengan proses lain yang berjalan.
>> 
>> ### Contoh Proses Prioritas Rendah:
>> 1. **Download File Besar**:
>>    - Misalnya, Anda sedang mendownload file besar menggunakan browser atau aplikasi download. Proses ini tidak terlalu mendesak, jadi Anda dapat memberi prioritas rendah padanya supaya tidak mengganggu pekerjaan lain yang Anda lakukan di komputer.
>>    
>> 2. **Backup Data**:
>>    - Jika Anda sedang melakukan backup data di latar belakang, ini adalah proses yang bisa diberi prioritas rendah, karena tidak mempengaruhi penggunaan aplikasi lain secara langsung.
>>    
>> 3. **Proses Pembersihan Disk**:
>>    - Membersihkan file sampah atau log di komputer biasanya juga bisa dilakukan dengan prioritas rendah, karena itu bukan tugas yang harus diselesaikan segera.
>>    
>> ### **Bagaimana Prioritas Rendah Diberikan?**
>> Di Linux, kita bisa mengubah prioritas suatu proses dengan menggunakan perintah `nice`. Semakin tinggi nilai **nice**, semakin rendah prioritasnya. Contoh:
>> - Jika Anda menjalankan perintah dengan `nice -n 10`, itu berarti proses tersebut akan memiliki prioritas lebih rendah daripada proses lainnya yang berjalan dengan nilai nice standar (biasanya 0).
>> - Sebaliknya, jika Anda ingin memberikan prioritas lebih tinggi, Anda bisa menggunakan nilai negative, misalnya `nice -n -10`.
>> 
>> ### **Bagaimana Proses Prioritas Rendah Bekerja?**
>> Proses dengan prioritas rendah tidak memblokir atau menunggu selama proses penting berjalan. Jadi, ketika CPU sedang sibuk menangani aplikasi atau tugas penting, proses dengan prioritas rendah akan menunggu sampai CPU bebas. Namun, jika CPU sedang idle (tidak ada tugas penting), proses dengan prioritas rendah bisa dieksekusi.
>>
>>  -----
>> Secara default, tanpa pengaturan khusus, tugas-tugas yang tergolong **prioritas rendah** di sistem biasanya adalah tugas-tugas yang >> tidak membutuhkan respons cepat atau tidak mendesak. Sistem operasi akan memberi prioritas lebih tinggi kepada proses yang membutuhkan interaksi langsung dengan pengguna atau yang lebih penting untuk kinerja sistem.
>> 
>> Beberapa contoh **tugas prioritas rendah** yang biasanya berjalan secara otomatis di sistem tanpa perlu pengaturan khusus adalah:
>> 
>> 1. **Proses Pencadangan (Backup)**:
>>    - Proses pencadangan data yang berjalan secara terjadwal (misalnya, menggunakan `cron` di Linux). Backup ini membutuhkan waktu dan sumber daya, tetapi tidak perlu segera selesai, sehingga sering kali diberi prioritas rendah.
>> 
>> 2. **Download atau Pembaruan Otomatis**:
>>    - Download file besar atau pembaruan perangkat lunak yang dijadwalkan (misalnya, menggunakan aplikasi seperti `apt` di Linux atau `Windows Update`). Meskipun penting, proses ini dapat berjalan di latar belakang dengan prioritas rendah agar tidak mengganggu penggunaan sistem.
>> 
>> 3. **Indeksasi File**:
>>    - Proses yang dilakukan oleh aplikasi seperti mesin pencari (misalnya `tracker` di Linux atau `Spotlight` di macOS) untuk membuat indeks file agar pencarian menjadi lebih cepat. Ini bisa memakan waktu, tetapi tidak memerlukan respons cepat.
>> 
>> 4. **Pemeliharaan Sistem (Maintenance)**:
>>    - Proses pemeliharaan seperti defragmentasi disk atau pembersihan file sampah yang terjadi secara berkala. Ini juga bisa berjalan dengan prioritas rendah karena proses ini tidak perlu segera selesai.
>> 
>> 5. **Proses Pengiriman atau Penerimaan Data di Latar Belakang**:
>>    - Proses-proses seperti sinkronisasi data dengan cloud atau pengunduhan dan pengunggahan file besar melalui jaringan. Meskipun membutuhkan sumber daya, mereka biasanya tidak memerlukan prioritas tinggi dan dapat berjalan di latar belakang.
>> 
>> ### Bagaimana Sistem Menentukan Prioritas Proses?
>> 
>> Sistem operasi secara otomatis memberi prioritas rendah pada tugas-tugas yang tidak mendesak dengan cara mengatur **nilai "nice"**. >> Semakin tinggi nilai "nice", semakin rendah prioritasnya. Proses dengan prioritas rendah akan menunggu hingga CPU memiliki waktu luang untuk menyelesaikannya.
>> 
>> Misalnya, sebuah aplikasi unduhan yang berjalan di latar belakang mungkin memiliki nilai "nice" lebih tinggi (misalnya, +10), yang >> berarti CPU akan mengerjakan proses utama terlebih dahulu (seperti menjalankan aplikasi yang aktif) sebelum menjalankan unduhan.
>> 
>> Dengan pengaturan default, proses semacam ini tidak akan mempengaruhi pengalaman pengguna yang sedang berinteraksi langsung dengan komputer.
>>
>>  
#### **4. `id` (idle) - Waktu Menganggur (Tidak Digunakan)**  
- **Apa itu?**  
   Ini adalah **persentase waktu CPU yang tidak digunakan**. Artinya CPU sedang santai dan tidak sibuk.

- **Penjelasan Sederhana:**  
   Kalau angka ini tinggi, artinya CPU Anda **banyak waktu luang**, dan sistem tidak terbebani.

- **Contoh:**  
   Jika `id = 80%`, berarti **80% dari CPU** tidak digunakan sama sekali. Ini biasanya tanda bahwa sistem dalam kondisi normal dan tidak ada banyak proses berjalan.



#### **5. `wa` (I/O wait) - Waktu Menunggu Input/Output**  
- **Apa itu?**  
   Ini adalah **persentase waktu CPU** yang dihabiskan untuk **menunggu perangkat I/O** seperti **hard disk atau jaringan** selesai membaca/menulis data.

- **Penjelasan Sederhana:**  
   Kalau angka ini tinggi, CPU sedang menunggu perangkat keras yang lambat, seperti hard disk yang butuh waktu lama untuk memproses data.

- **Contoh:**  
   Jika `wa = 10%`, itu artinya CPU menghabiskan **10% waktunya hanya untuk menunggu** hard disk atau perangkat lain menyelesaikan tugas.



#### **6. `hi` (hardware interrupts) - Waktu Menangani Perangkat Keras**  
- **Apa itu?**  
   Ini adalah **persentase waktu CPU** yang digunakan untuk menangani **interrupt hardware**. Contohnya ketika keyboard ditekan atau mouse digerakkan.

- **Penjelasan Sederhana:**  
   CPU harus merespons sinyal dari perangkat keras. Biasanya, ini hanya memakan waktu sedikit.

- **Contoh:**  
   Jika `hi = 1%`, artinya **1% dari CPU** digunakan untuk menangani sinyal dari perangkat keras.


#### **7. `si` (software interrupts) - Waktu Menangani Perangkat Lunak**  
- **Apa itu?**  
   Ini adalah **persentase waktu CPU** yang digunakan untuk menangani **interrupt dari software**. Software interrupt terjadi ketika aplikasi meminta CPU untuk segera melakukan sesuatu.

- **Penjelasan Sederhana:**  
   Kalau angka ini tinggi, mungkin ada aplikasi yang sering memanggil CPU untuk menangani tugas tertentu.

- **Contoh:**  
   Jika `si = 0.5%`, berarti **0.5% dari CPU** digunakan untuk menangani permintaan dari aplikasi.



#### **8. `st` (steal) - Waktu CPU yang Dipakai Mesin Virtual**  
- **Apa itu?**  
   Ini adalah **persentase waktu CPU** yang digunakan oleh mesin virtual atau hypervisor. Ini terjadi jika Anda menjalankan sistem operasi di dalam mesin virtual (VM).

- **Penjelasan Sederhana:**  
   Kalau angka ini tinggi, itu berarti CPU "dicuri" oleh mesin virtual dan tidak bisa digunakan oleh sistem utama.

- **Contoh:**  
   Jika `st = 5%`, artinya **5% dari CPU** digunakan oleh mesin virtual.



## **Ringkasan Mudah:**
| Statistik | Arti Sederhana | Jika Angka Tinggi |
|-----------|----------------|------------------|
| **`us`** | CPU sibuk menjalankan aplikasi Anda | Terlalu banyak aplikasi berat |
| **`sy`** | CPU sibuk menjalankan tugas sistem | Sistem sibuk mengelola proses internal |
| **`ni`** | CPU menjalankan program prioritas rendah | Ada proses prioritas rendah berjalan |
| **`id`** | CPU tidak sibuk (idle) | Sistem santai, CPU punya banyak waktu luang |
| **`wa`** | CPU menunggu perangkat I/O | Perangkat keras (disk/jaringan) lambat |
| **`hi`** | CPU menangani sinyal perangkat keras | Perangkat keras mungkin bermasalah |
| **`si`** | CPU menangani sinyal perangkat lunak | Aplikasi sering meminta CPU |
| **`st`** | CPU digunakan oleh mesin virtual | CPU "dipakai" oleh mesin virtualisasi |



Dengan memahami angka-angka ini, Anda bisa mendiagnosis **kenapa CPU bekerja lambat atau sibuk**. Misalnya:  
- Kalau **`us`** tinggi → Banyak program pengguna yang berat.  
- Kalau **`sy`** tinggi → Sistem sibuk.  
- Kalau **`wa`** tinggi → Ada masalah I/O (disk atau jaringan lambat).  
- Kalau **`id`** rendah → CPU sedang **sangat sibuk**.

Harapannya penjelasan ini membuatnya **lebih mudah dipahami**! Jika ada bagian yang ingin diperjelas lagi, beri tahu saya. 😊
