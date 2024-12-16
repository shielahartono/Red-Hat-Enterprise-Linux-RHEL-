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
>> Secara default, tanpa pengaturan khusus, tugas-tugas yang tergolong **prioritas rendah** di sistem biasanya adalah tugas-tugas yang  tidak membutuhkan respons cepat atau tidak mendesak. Sistem operasi akan memberi prioritas lebih tinggi kepada proses yang membutuhkan interaksi langsung dengan pengguna atau yang lebih penting untuk kinerja sistem.
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
>>  --------
>> 
>> 
>> 
>> ### Apa itu `nice`?
>> 
>> **`nice`** adalah perintah di Linux yang digunakan untuk mengatur prioritas suatu proses. Semakin "tinggi" nilai nice, semakin **rendah** prioritas proses tersebut. Sebaliknya, semakin **rendah** nilai nice, semakin **tinggi** prioritasnya. 
>> 
>> ### Menggunakan `nice -n`
>> 
>> Sintaks perintah `nice` adalah seperti ini:
>> 
>> ```
>> nice -n <nilai> <perintah>
>> ```
>> 
>> Di sini:
>> - **`-n <nilai>`** mengatur tingkat prioritas.
>> - Nilai bisa berupa angka positif atau negatif.
>> 
>> ### 1. **`nice -n 10`** - **Prioritas Lebih Rendah**
>> 
>> Jika Anda menjalankan perintah seperti ini:
>> 
>> ```
>> nice -n 10 <perintah>
>> ```
>> 
>> Maka proses yang dijalankan dengan perintah tersebut akan memiliki **prioritas lebih rendah** dibandingkan dengan proses lain yang >> berjalan tanpa pengaturan `nice` (nilai nice standar = 0). 
>> 
>> **Penjelasan:**
>> - **Nilai `10`** berarti proses ini akan diberi prioritas rendah. Sistem operasi akan mencoba memberikan waktu CPU untuk proses yang lebih penting terlebih dahulu, dan hanya memberi proses ini waktu CPU jika tidak ada proses lain yang lebih penting.
>> - Biasanya, Anda memberikan prioritas rendah untuk proses yang tidak mendesak, seperti pengunduhan file besar, pencadangan data, atau operasi latar belakang.
>> 
>> Contoh:
>> 
>> ```
>> nice -n 10 tar -cvf backup.tar /home/user/
>> ```
>> 
>> Perintah di atas akan menjalankan proses pencadangan (backup) dengan prioritas lebih rendah, sehingga tidak mengganggu proses lain >> yang lebih penting.
>> 
>> ### 2. **`nice -n -10`** - **Prioritas Lebih Tinggi**
>> 
>> Jika Anda menjalankan perintah seperti ini:
>> 
>> ```
>> nice -n -10 <perintah>
>> ```
>> 
>> Maka proses tersebut akan memiliki **prioritas lebih tinggi** dibandingkan dengan proses lain yang berjalan dengan nilai nice standar (0).
>> 
>> **Penjelasan:**
>> - **Nilai `-10`** berarti proses ini diberi prioritas yang lebih tinggi, sehingga sistem akan memberikan waktu CPU lebih banyak untuk proses ini, bahkan jika ada proses lain yang lebih biasa.
>> - Biasanya, Anda memberi prioritas tinggi pada proses yang sangat penting atau yang memerlukan respons cepat, seperti menjalankan aplikasi yang mempengaruhi interaksi pengguna langsung.
>> 
>> Contoh:
>> 
>> ```
>> nice -n -10 ./game
>> ```
>> 
>> Perintah di atas akan menjalankan game dengan prioritas tinggi, sehingga game akan mendapatkan lebih banyak waktu CPU dibandingkan dengan proses lain yang berjalan di sistem.
>> 
>> ### Rangkuman:
>> 
>> - **`nice -n 10`**: Menjalankan proses dengan **prioritas rendah**. Proses ini akan dijalankan hanya jika tidak ada proses lain yang lebih penting.
>> - **`nice -n -10`**: Menjalankan proses dengan **prioritas tinggi**. Proses ini akan mendapatkan lebih banyak waktu CPU, dan dijalankan lebih cepat dibandingkan proses lainnya yang memiliki prioritas rendah atau normal.
>> 
>> Jadi, intinya:
>> - **Nilai positif (`10`)** = **prioritas rendah**.
>> - **Nilai negatif (`-10`)** = **prioritas tinggi**.
>>
>> ---------------------------------
>> Contoh :
>> ```
>> nice -n -7 ./game
>> nice -n -6 tar -cvf backup.tar /home/user/
>> ```
>> Perintah **`nice -n -7 ./game`** dan **`nice -n -6 tar -cvf backup.tar /home/user/`** keduanya menggunakan nilai **negative** untuk >> meningkatkan prioritas proses yang dijalankan. Namun, ada perbedaan antara nilai **-7** dan **-6**.
>> 
>> - **`nice -n -7 ./game`** memberikan prioritas lebih tinggi dibandingkan **`nice -n -6 tar -cvf backup.tar /home/user/`** karena **nilai -7 lebih kecil** daripada **nilai -6**. Dalam konteks nilai nice, semakin kecil angka (semakin negatif), semakin tinggi prioritasnya.
>> 
>> ### Kesimpulan:
>> - **`game`** akan lebih diprioritaskan dibandingkan **backup** karena nilai **`nice -n -7`** memberikan prioritas lebih tinggi daripada **`nice -n -6`**.
>> 
>> 
  #### **4. `id` (idle) - Waktu Menganggur (Tidak Digunakan)**  
  - **Apa itu?**  
    Ini adalah **persentase waktu CPU yang tidak digunakan**. Artinya CPU sedang santai dan tidak sibuk.
  
  - **Penjelasan Sederhana:**  
     Kalau angka ini tinggi, artinya CPU Anda **banyak waktu luang**, dan sistem tidak terbebani.
  
  - **Contoh:**  
     Jika `id = 80%`, berarti **80% dari CPU** tidak digunakan sama sekali. Ini biasanya tanda bahwa sistem dalam kondisi normal dan  tidak ada banyak proses berjalan.
  


#### **5. `wa` (I/O wait) - Waktu Menunggu Input/Output**  
- **Apa itu?**  
   Ini adalah **persentase waktu CPU** yang dihabiskan untuk **menunggu perangkat I/O** seperti **hard disk atau jaringan** selesai membaca/menulis data.

- **Penjelasan Sederhana:**  
   Kalau angka ini tinggi, CPU sedang menunggu perangkat keras yang lambat, seperti hard disk yang butuh waktu lama untuk memproses data. <br/>
  (kalau angka ini tinggi, artinya terdapat Bottleneck) <br/>

- **Contoh:**  
   Jika `wa = 10%`, itu artinya CPU menghabiskan **10% waktunya hanya untuk menunggu** hard disk atau perangkat lain menyelesaikan tugas.

>> ^For your Information^
>> ## `wa` (I/O wait)
>> **`wa` (I/O wait)** adalah waktu di mana **CPU sedang menunggu** perangkat keras lainnya, seperti **hard disk** atau **jaringan**, untuk selesai melakukan tugasnya, seperti membaca atau menulis data.
>> 
>> ### Penjelasan Sederhana:
>> - **I/O (Input/Output)** adalah proses komunikasi antara CPU dan perangkat keras lainnya, seperti hard disk, SSD, atau perangkat jaringan.
>> - Ketika CPU membutuhkan data dari perangkat keras (misalnya membaca file dari hard disk) dan perangkat keras tersebut memerlukan waktu untuk memproses data, maka CPU harus **menunggu**.
>> - **`wa`** menunjukkan **persentase waktu** CPU yang digunakan hanya untuk menunggu proses I/O selesai. Jika nilai ini tinggi, berarti **CPU sedang menunggu perangkat keras** yang lambat, seperti hard disk, untuk menyelesaikan tugasnya.
>> 
>> ### Contoh:
>> - Jika **`wa = 10%`**, itu berarti **10% dari waktu CPU** dihabiskan hanya untuk **menunggu** hard disk atau perangkat lain untuk selesai membaca/menulis data.
>> - Jika nilai **`wa`** tinggi, itu bisa menandakan bahwa ada **bottleneck** atau **kemacetan** di perangkat I/O (misalnya hard disk yang lambat atau jaringan yang tersumbat), yang membuat CPU harus menunggu lebih lama untuk mendapatkan data yang dibutuhkan.
>> 
>> ### Mengapa Ini Terjadi?
>> - **Perangkat keras lambat**: Jika perangkat seperti hard disk terlalu lambat dalam memproses data, CPU akan menghabiskan banyak waktu menunggu.
>> - **Beban tinggi pada I/O**: Jika ada banyak operasi baca/tulis yang terjadi secara bersamaan, CPU mungkin harus menunggu lebih lama.
>> 
>> Jika angka **`wa`** sangat tinggi, misalnya lebih dari 20%, itu bisa menjadi tanda bahwa sistem **terhambat oleh masalah I/O** dan perlu diperiksa apakah perangkat keras (seperti hard disk atau SSD) berfungsi dengan baik atau tidak.
>> 
>> -------------
>>  **I/O (Input/Output)** tidak selalu berarti perangkat keras (hardware). Meskipun sering dikaitkan dengan komunikasi antara CPU dan >> perangkat keras (seperti hard disk, SSD, atau perangkat jaringan), I/O juga dapat merujuk pada komunikasi antara program/software dan perangkat keras atau antar perangkat lunak itu sendiri.
>> 
>> Penjelasan lebih lanjut : 
>> 
>> ### 1. **I/O pada Perangkat Keras (Hardware I/O)**
>>    - **Contoh:** Pembacaan atau penulisan data ke **hard disk**, **SSD**, **memori**, atau **perangkat jaringan**.
>>    - **Penjelasan:** Ketika komputer membaca data dari disk atau menulis data ke disk, itu adalah **I/O perangkat keras**. I/O ini melibatkan **operasi fisik** antara CPU dan perangkat penyimpanan atau perangkat eksternal lainnya.
>>    
>> ### 2. **I/O pada Perangkat Lunak (Software I/O)**
>>    - **Contoh:** Interaksi antara program dan sistem operasi untuk membaca atau menulis data ke file, komunikasi antar proses, atau pengiriman dan penerimaan data di aplikasi.
>>    - **Penjelasan:** Tidak hanya perangkat keras yang terlibat dalam I/O. **Aplikasi software** juga dapat melakukan I/O dengan berinteraksi dengan file di sistem operasi, jaringan, atau layanan lainnya. Misalnya, sebuah program bisa melakukan **I/O** dengan **membaca data dari file** atau **mengirim data ke server** melalui jaringan.
>> 
>> ### Contoh I/O yang Melibatkan Hanya Software:
>>    - **File I/O:** Aplikasi yang membuka, membaca, menulis, atau mengubah file di sistem file.
>>    - **Inter-process Communication (IPC):** Proses-proses di dalam sistem bisa saling berkomunikasi dan bertukar data tanpa melibatkan perangkat keras eksternal.
>>    - **Database I/O:** Aplikasi yang berkomunikasi dengan **sistem database** untuk mengambil atau memperbarui data, ini adalah I/O yang melibatkan **software dan sistem database** (meskipun database sendiri ada di perangkat keras).
>> 
>> ### Kesimpulannya:
>> I/O memang **sering berhubungan dengan perangkat keras** (seperti disk atau jaringan), tetapi juga dapat melibatkan **komunikasi antar program atau proses perangkat lunak** itu sendiri. Jadi, **I/O tidak selalu berarti perangkat keras**, bisa juga berarti pertukaran data antar perangkat lunak di dalam sistem.
>> -----------
>> **I/O (Input/Output)** adalah istilah yang digunakan untuk menggambarkan proses komunikasi atau pertukaran data antara **sistem komputer** (terutama CPU) dan **perangkat lainnya**. I/O mengacu pada bagaimana data dimasukkan ke dalam sistem (input) atau dikeluarkan dari sistem (output). Proses ini melibatkan baik perangkat keras (hardware) maupun perangkat lunak (software).
>> 
>> Untuk menjelaskan dengan lebih sederhana, kita bisa membagi **I/O** ke dalam dua kategori utama: **Input** dan **Output**.
>> 
>> ### 1. **Input (Masukan)**
>> Input adalah proses di mana data dimasukkan ke dalam sistem komputer. Misalnya:
>> - **Keyboard**: Ketika Anda mengetik sesuatu, keyboard mengirimkan data ke komputer. Ini adalah contoh **input**.
>> - **Mouse**: Gerakan dan klik mouse juga menghasilkan input untuk sistem.
>> - **Sensor atau perangkat lain**: Data dari sensor suhu, kamera, mikrofon, atau perangkat lainnya juga merupakan input yang masuk ke sistem.
>> 
>> ### 2. **Output (Keluaran)**
>> Output adalah proses di mana sistem komputer mengirimkan atau menampilkan data ke perangkat lain. Misalnya:
>> - **Monitor**: Saat Anda membuka aplikasi atau menonton video, data dikirimkan ke monitor untuk ditampilkan. Ini adalah contoh **output**.
>> - **Printer**: Ketika Anda mencetak dokumen, data dikirim ke printer untuk diproses dan dicetak. Ini juga merupakan **output**.
>> - **Perangkat lain**: Suara yang dikeluarkan oleh speaker atau data yang dikirim melalui jaringan juga merupakan **output**.
>> 
>> ### 3. **I/O pada Perangkat Keras (Hardware)**
>> Sebagian besar proses I/O melibatkan **perangkat keras** yang digunakan untuk input atau output data. Beberapa contoh perangkat keras yang terlibat dalam I/O adalah:
>> - **Disk keras (HDD atau SSD)**: Saat data dibaca atau ditulis ke dalam disk, itu adalah operasi I/O. Misalnya, saat Anda membuka file, data dibaca dari disk (input), dan saat Anda menyimpan file, data ditulis ke disk (output).
>> - **Jaringan**: Mengirim atau menerima data melalui jaringan (misalnya internet) adalah bentuk I/O. Ketika Anda mengunduh file, data diterima dari server (input), dan saat Anda mengunggah file, data dikirim ke server (output).
>> 
>> ### 4. **I/O pada Perangkat Lunak (Software)**
>> Selain perangkat keras, I/O juga terjadi di tingkat **perangkat lunak**. Berikut adalah beberapa contoh I/O yang terjadi antara perangkat lunak dan sistem operasi atau aplikasi:
>> - **File I/O**: Aplikasi yang membuka, membaca, menulis, atau mengubah file di sistem adalah contoh I/O. Misalnya, aplikasi pengolah kata yang membuka file untuk dibaca atau disunting.
>> - **Database I/O**: Sistem manajemen basis data (DBMS) sering berinteraksi dengan aplikasi dan sistem operasi untuk mengakses atau menyimpan data dalam database.
>> 
>> ### 5. **Contoh Proses I/O**
>> - **Membaca data dari disk**: Ketika Anda membuka dokumen, komputer akan melakukan **I/O** untuk membaca data dari hard disk atau SSD (ini adalah **input**).
>> - **Menulis data ke disk**: Ketika Anda menyimpan file, sistem menulis data ke disk (**output**).
>> - **Mengirim data melalui jaringan**: Ketika Anda mengirim email atau mengunggah foto, data dikirim melalui jaringan (ini adalah **output**). Sebaliknya, saat Anda menerima email atau mengunduh foto, data diterima melalui jaringan (**input**).
>> 
>> ### 6. **Kenapa I/O Penting?**
>> I/O sangat penting karena hampir semua interaksi dengan sistem komputer, baik dari pengguna maupun aplikasi, melibatkan I/O. Misalnya, tanpa I/O, Anda tidak akan dapat berinteraksi dengan aplikasi atau menyimpan file.
>> 
>> ### 7. **Masalah yang Sering Ditemui dalam I/O**
>> Terkadang, proses I/O dapat menjadi **bottleneck** atau hambatan dalam kinerja sistem. Misalnya, jika hard disk Anda sangat lambat, >> maka proses **pembacaan dan penulisan data** menjadi lebih lama, yang dapat memperlambat kinerja sistem. Ini sering disebut **I/O >> bottleneck**.
>> 
>> ### Kesimpulannya:
>> **I/O (Input/Output)** adalah mekanisme untuk **memasukkan** data ke dalam sistem atau **mengeluarkan** data dari sistem. Ini bisa >> melibatkan perangkat keras seperti keyboard, mouse, disk, atau perangkat jaringan, serta perangkat lunak yang berinteraksi dengan >> sistem untuk mengakses atau menyimpan data. I/O adalah bagian penting dalam setiap interaksi dengan komputer, baik itu untuk aplikasi yang kita gunakan, file yang kita simpan, atau komunikasi yang terjadi melalui jaringan.
>>   
>> 



#### **6. `hi` (hardware interrupts) - Waktu Menangani Perangkat Keras**  
- **Apa itu?**  
   Ini adalah **persentase waktu CPU** yang digunakan untuk menangani **interrupt hardware**. Contohnya ketika keyboard ditekan atau mouse digerakkan.

- **Penjelasan Sederhana:**  
   CPU harus merespons sinyal dari perangkat keras. Biasanya, ini hanya memakan waktu sedikit.

- **Contoh:**  
   Jika `hi = 1%`, artinya **1% dari CPU** digunakan untuk menangani sinyal dari perangkat keras.

>> ^For your Information^
>> ## Hardware Interrupts 
>> 
>> **`hi` (hardware interrupts)** adalah tentang bagaimana CPU merespons **permintaan (interrupt)** dari perangkat keras. 
>> 
>> 
>> 
>> ### **Apa Itu Interrupt?**  
>> Interrupt adalah **sinyal atau pemberitahuan** dari perangkat keras (seperti keyboard, mouse, atau perangkat jaringan) ke CPU untuk >> meminta perhatian. Perangkat keras "menyela" CPU agar tugas tertentu bisa diproses.
>> 
>> Bayangkan CPU seperti seseorang yang sedang bekerja (misalnya menulis dokumen), dan tiba-tiba ada **telepon berdering** (interrupt). Orang tersebut akan berhenti menulis sebentar untuk menjawab telepon (menangani interrupt), lalu kembali ke pekerjaan aslinya.
>> 
>> 
>> ### **Apa Itu `hi` (Hardware Interrupts)?**  
>> `hi` adalah **persentase waktu CPU** yang digunakan **khusus untuk menangani sinyal dari perangkat keras**. Sinyal ini bisa datang dari:
>> - Keyboard (misalnya ketika Anda menekan tombol).
>> - Mouse (misalnya saat Anda menggerakkan kursor).
>> - Perangkat jaringan (misalnya saat ada paket data masuk).
>> - Disk I/O (operasi baca/tulis dari hard disk atau SSD).
>> - Perangkat keras lainnya (printer, USB, dll).
>> 
>> CPU harus berhenti sebentar dari tugas utamanya untuk **menangani sinyal tersebut**, menyelesaikan permintaan, lalu melanjutkan >> pekerjaannya.
>> 
>> 
>> 
>> ### **Penjelasan Sederhana**
>> Ketika nilai **`hi`** terlihat di output sistem (misalnya **`hi = 1%`**), itu berarti **1% dari waktu CPU** digunakan untuk memproses **interrupt hardware**.
>> 
>> Biasanya, **nilai `hi` sangat kecil**, karena perangkat keras jarang memerlukan perhatian CPU dalam waktu lama. Kebanyakan perangkat keras hanya membutuhkan sedikit waktu CPU untuk memproses sinyal mereka.
>> 
>> ### **Contoh Sederhana**
>> 1. Anda menekan tombol **keyboard**.  
>>    - Keyboard mengirim **interrupt** ke CPU untuk memberi tahu bahwa ada tombol yang ditekan.  
>>    - CPU **berhenti sebentar**, memproses sinyal dari keyboard, lalu kembali ke tugasnya semula.
>> 
>> 2. Anda menggerakkan **mouse**.
>>    - Mouse mengirim sinyal ke CPU untuk memperbarui posisi kursor di layar.  
>>    - CPU menangani permintaan itu sebentar,
>> 
>> lalu kembali bekerja menyelesaikan tugas-tugas lain.
>> 


### **Kenapa Ini Penting?**  
- **Normal**: Nilai `hi` yang rendah (misalnya di bawah 1-2%) menunjukkan bahwa perangkat keras berjalan normal dan tidak membebani CPU.
- **Tinggi**: Jika **`hi`** sangat tinggi (misalnya > 5%), itu bisa berarti ada **masalah dengan perangkat keras** atau ada terlalu banyak interrupt dari suatu perangkat (contoh: jaringan, disk, atau perangkat USB). Ini bisa menyebabkan CPU sibuk hanya untuk menangani interrupt, bukan menjalankan aplikasi utama.



### **Kesimpulan**  
**`hi`** (hardware interrupts) adalah waktu CPU yang digunakan untuk menangani **sinyal dari perangkat keras** seperti keyboard, mouse, atau jaringan.  
- **Nilai kecil** = normal, perangkat keras bekerja wajar.  
- **Nilai besar** = perlu diperiksa, bisa jadi ada masalah dengan perangkat keras yang mengganggu CPU.

Apakah penjelasan ini lebih mudah dipahami? 😊

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
