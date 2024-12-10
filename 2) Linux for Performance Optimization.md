# Linux for Performance Optimization

Berikut ini Penggnaan Linux untuk "Performance Optimization" dan Cara meng-analisa nya :

# A. Analyze System Resource Usage and Identify Performance Bottlenecks related to CPU, Memory, disk I/O, and Network Usage
 (Meng-analisa penggunaan "System Resource" dan Identifikasi Kendala Kinerja yang berhubungan dengan CPU, Memory, Disk I/O, dan Network Usage )

## 1. `top` :
### Menggunakan Command Linux `top`

Command `top` menunjukkan informasi real-time tentang penggunaan CPU, memori, proses yang berjalan.

### (1) Jalankan Command `top` :
```
top
```
Setelah itu, kita akan melihat output yang terus diperbarui secara otomatis, yang menunjukkan proses yang sedang berjalan di sistem kita.

### (2) Contoh Output untuk Command `top` :
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

### (3) Penjelasan Output untuk Command `top` :

#### 1. System Information :
#### (Informasi Sistem)

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
