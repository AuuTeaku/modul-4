# SUBNETTING & ROUTING

## PENGELALAN

## SUBNETTING
### Pengertian

<b>Subnet</b> adalah suatu sub jaringan dari jaringan yang lebih besar. Subnet diperlukan sebagai penanda suatu jaringan tertentu. Dengan adanya subnet, kita dapat melakukan manajemen suatu jaringan dengan lebih baik.

Tujuan utama kita belajar subnetting ini adalah pembagian alamat IP untuk kebutuhan tertentu. Sebagai contoh di gedung departemen informatika ini. Pada gedung departemen informatika terdapat beberapa laboratorium, dan setiap laboratorium memiliki lebih dari 1 komputer yang harus dikonfigurasikan sedemikian rupa sehingga dapat saling berkomunikasi bahkan hingga dapat mengakses internet.

Dari contoh diatas, muncullah salah satu konfigurasi paling dasar dalam penyelesaian permasalahan ini yaitu pembagian alamat IP untuk setiap laboratorium yang ada di gedung departemen informatika.

Contohnya :
1. Laboratorium LP memiliki jaringan dengan subnet 10.151.34.0/24
2. Laboratorium AJK memiliki jaringan dengan subnet 10.151.36.0/24

### Perhitungan Subnet
#### A. Classfull
#### B. Classless
<b>1. VLSM (Variable Length Subnet Masking)</b>
Inti utama dari penggunaan teknik VLSM adalah untuk mengefisienkan pembagian IP di dalam jaringan. Besar netmask disesuaikan dengan banyaknya komputer / host yang membutuhkan alamat IP. Berikut ini adalah cara menggunakan teknik VLSM<br>
Dengan menggunakan topologi yang ada sebelumnya, hitung terlebih dahulu total alamat IP yang dibutuhkan:
    - A1 = 100 host
    - A2 = 2 host
    - A3 = 50 host
    - A4 = 2 host
    - A5 = 2 host
    - A6 = 2 host
    - A7 = 10 host
    - A8 = 20 host

Total komputer / host yang ada di dalam topologi adalah 188. Oleh karena itu digunakan netmask /24 karena dengan netmask tersebut, jumlah 188 bisa tertampung semua. Selanjutnya subnet besar yang dibentuk memiliki NID 192.168.1.0 dengan netmask /24.

Setelah itu lakukan subnetting dengan menggunakan pohon pembagian IP sesuai dengan pohon dibawah ini.
![1](/assets/PohonVLSM.png)

Dari pohon di atas didapatkan pembagian IP sebagai berikut
![2](/assets/TabelVLSM.png)

<b>2. CIDR (Classless Inter Domain Routing)</b>
Perhitungan pada teknik CIDR didasarkan pada jumlah komputer / host yang ada di dalam subnet. Tetapi cara mendapatkan subnet besar tidak sama dengan VLSM. Berikut langkah-langkahnya:<br>
Pada awalnya sama dengan pembagian biasa, tentukan subnet yang ada tetapi dengan tambahan, cantumkan netmask terkecil yang paling memungkinkan untuk subnet tersebut. Lihat gambar dibawah.
![3](/assets/CIDR1.png)
<br>Kemudian gabungkan subnet paling bawah di dalam topologi. Paling bawah disini maksudnya adalah <b>Subnet yang paling bawah dari internet (gambar awan)</b>. Jika diperhatikan, subnet paling bawah yang bisa digabungkan adalah <b>A1</b> dengan <b>A2</b> dan juga subnet <b>A7</b> dengan <b>A8</b>. Setelah digabungkan, maka akan seperti pada gambar dibawah
![4](/assets/CIDR2.png)
<br>Subnet <b>B1</b> merupakan hasil penggabungan dari subnet A1 dan A2, Subnet <b>B2</b> merupakan hasil penggabungan dari subnet A7 dan A8.<br>
<b>Mengapa subnet B1 memiliki netmask /24? Dan subnet B2 memiliki netmask /26?</b>
Perhatikan subnet A1 dan A2. Subnet A1 memiliki netmask /25, dan subnet A2 memiliki netmask /30. Caranya adalah dengan <b>menaikkan netmask 1 tingkat</b> dari subnet yang terbesar diantara kedua subnet yang digabungkan (subnet A1 dan subnet A2). Karena subnet A1 memiliki netmask /25, <b>maka jika dilakukan penggabungan akan menjadi /24</b>. Begitu pula dengan subnet B2.

Setelah itu ulangi langkah sebelumnya, gabungkan lagi antara 2 subnet terbawah, maka akan seperti pada gambar dibawah.
![5](/assets/CIDR3.png)
<br>Dari gambar diatas dihasilkan subnet <b>C1</b> yang merupakan gabungan dari B1 dan A3 yang memiliki netmsak /23 dan subnet <b>C2</b> yang merupakan gabungan dari B2 dan A6. 

Gabungkan lagi 2 subnet terbawah, maka akan seperti pada gambar dibawah.
![6](/assets/CIDR4.png)
![7](/assets/CIDR5.png)

Dari proses penggabungan yang telah dilakukan, didapatkan sebuah subnet dengan netmask /21. Dengan NID 192.168.0.0, netmask 255.255.248.0, hitung pembagian IP dengan pohon seperti pada pembagian di VLSM.
![8](/assets/CIDR6.png)
<br>Perbedaan antara pohon VLSM dengan pohon CIDR adalah keika satu subnet diturunkan, netmask yang terbentuk langsung disesuaikan dengan gabungan yang sudah dilakukan sebelumnya. Sebagai contoh, dari netmask besar /21 yang seharusnya dibagi dua menjadi masing-masing /22. Namun karena sebelumnya /21 dihasilkan dari penggabungan /22 dan /24 maka subnet yang terbentuk memiliki netmask /22 dan /24.

Dari hasil penghitungan didapatkan pembagian IP sebagai berikut.
![9](/assets/CIDR7.png)

Jika kalian menggunakan CIDR maka netmask yang terbentuk akan menjadi lebih besar dibandingkan dengan menggunakan VLSM. Tetapi teknik CIDR tetap memiliki keunggulan, salah satunya adalah ketika muncul subnet baru di dalam topologi, tidak perlu dilakukan penghitungan lagi karena masih ada beberapa IP yang tidak terpakai. Keunggulan lainnya dari teknik CIDR ini adalah kemudahan dalam proses <i>routing</i>. Tabel routing pada teknik CIDR akan lebih simpel daripada dengan teknik VLSM. 
## ROUTING