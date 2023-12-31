# Jarkom-Modul-2-D24-2023

Perkenalkan saya dari kelas `Jaringan Komputer D` - Kelompok D24 :

| Nama                   | NRP        |
| ---------------------- | ---------- |
| Daffa Saskara          | 5025211249 |

Rekan kelompok saya Arundaya pada Modul 2 ini TIDAK BERKONTRIBUSI.

## Informations

Soal:

1. Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut 
2. Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.
3. Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.
4. Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.
5. Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)
6. Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.
7. Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.
8. Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.
9. Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.
10. Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003
11. Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy
12. Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.
13. Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy
14. ada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).
15. Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.
16. Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js 
17. Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.
18. Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.
19. Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)
20. Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

PS:
- yyy pada url adalah kode kelompok anda
- File requirement dapat diakses melalui drive https://drive.google.com/drive/folders/15Wr1eTQqn_vZzqkTXEAKF7tgULsxkxfe?usp=sharing .

## Topologi

Kelompok D24 Mendapatkan format topologi nomor 2. Berikut adalah setup topologi kelompok kami :

![Alt text](img/imgpraak.top.jpg)

Dengan IP masing-masing sebagai berikut :
```
NakulaClient = 109.203.3.2
SadewaClient = 109.203.3.3
AbimanyuWebServer = 109.203.2.4
PrabukusumaWebServer = 109.203.2.5
WisanggeniWebServer = 109.203.2.6

YudhistiraDNSMaster = 109.203.1.2
WerkudaraDNSSlave = 109.203.2.2
ArjunaLoadBalancer = 109.203.2.3
```

## Konfigurasi

### - Master ( Yudhistira )

- config `named.conf.local` pada master Yudhistira

![Alt text](img/imgprak.masternamed.jpg)


- config `zone arjuna.d24.com` pada master Yudhistira

![Alt text](img/imgprak.masterarjuna.jpg)

- config `resolv.conf` pada master Yudhistira

![Alt text](img/imgprak.masterresolv.jpg)

- config `zone abimanyu.d24.com` pada master Yudhistira

![Alt text](img/imgprak.masterabimanyu.jpg)

- config `named.conf.options` pada master Yudhistira

![Alt text](img/imgprak.masterforward.jpg)

- config `zone reverse DNS abimanyu` pada master Yudhistira

![Alt text](img/imgprak.masterreverse.jpg)

### - Slave ( Wekudara )

- config `named.conf.local` pada slave Wekudara

![Alt text](img/imgprak.slavenamed.jpg)


- config `named.conf.options` pada slave Wekudara

![Alt text](img/imgprak.slavedelegasi.jpg)

- config `zone delegasi baratayuda` pada slave Wekudara

![Alt text](img/imgprak.slavedelegasibarat.jpg)
