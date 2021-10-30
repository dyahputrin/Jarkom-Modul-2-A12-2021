# Jarkom-Modul-2-A12-2021

**Praktikum Modul 2 - Jaringan Komputer 2021**
**A12**
-   Fiqey Indriati Eka Sari (05111940000015)
-   Dyah Putri Nariswari (05111940000047)
-   Muhammad Farrel Abhinaya (05111940000173)

### Soal 1
Membuat topologi dengan EniesLobby sebagai DNS Master, Water7 sebagai DNS Slave, dan Skypie sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet.

![image](https://user-images.githubusercontent.com/71380876/139344764-d4bdc841-1233-4b85-a0b5-fac23e2ca06e.png)

Setting konfigurasi masing-masing node sebagai berikut:<br>

**Foosha**<br>
auto eth0<br>
iface eth0 inet dhcp<br>

auto eth1<br>
iface eth1 inet static<br>
  address 10.5.1.1<br>
  netmask 255.255.255.0<br>

auto eth2<br>
iface eth2 inet static<br>
  address 10.5.2.1<br>
  netmask 255.255.255.0<br>
  
**Loguetown**<br>
auto eth0<br>
iface eth0 inet static<br>
	address 10.5.1.2<br>
	netmask 255.255.255.0<br>
	gateway 10.5.1.1<br>

**Alabasta**<br>
auto eth0<br>
iface eth0 inet static<br>
	address 10.5.1.3<br>
	netmask 255.255.255.0<br>
	gateway 10.5.1.1<br>

**EniesLobby**<br>
auto eth0<br>
iface eth0 inet static<br>
	address 10.5.2.2<br>
	netmask 255.255.255.0<br>
	gateway 10.5.2.1<br>

**Water7**<br>
auto eth0<br>
iface eth0 inet static<br>
	address 10.5.2.3<br>
	netmask 255.255.255.0<br>
	gateway 10.5.2.1<br>

**Skypie**<br>
auto eth0<br>
iface eth0 inet static<br>
	address 10.5.2.4<br>
	netmask 255.255.255.0<br>
	gateway 10.5.2.1<br>

Lalu pada node Foosha tuliskan command `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.5.0.0/16` dan tuliskan command `echo nameserver 192.168.122.1 > /etc/resolv.conf` pada node lainnya.

--------------------------------

### Soal 2
Membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku
**Jawaban**
- Lakukan instalasi bind9 pada EniesLobby dengan command `apt-get install bind9 -y`

- Edit file `/etc/bind/named.conf.local` seperti berikut
![Screenshot (1904)](https://user-images.githubusercontent.com/71380876/139346965-c21ec33d-7807-410a-83f4-4c6dc920202a.png)

- Buat folder kaizoku dengan `mkdir /etc/bind/kaizoku`

- Copy kan db.local ke dalam folder kaizoku dan ubah nama nya menjadi ```franky.a12.com```<br>
```cp /etc/bind/db.local /etc/bind/kaizoku/franky.a12.com```

- Edit file ```/etc/bind/kaizoku/franky.a12.com```
![Screenshot (1751)](https://user-images.githubusercontent.com/71380876/139347507-7831af9d-2df5-44c2-a40e-103794ebcacf.png)

- Restart bind9 dengan ```service bind9 restart```

- Pada Loguetown edit file ```etc.resolv.conf```
![Screenshot (1905)](https://user-images.githubusercontent.com/71380876/139347702-ade203d9-be91-4f99-aa71-58e027fdac39.png)

- Testing dengan cara `ping franky.a12.com` dan `ping www.franky.a12.com` pada Loguetown
![Screenshot (1756)](https://user-images.githubusercontent.com/71380876/139347787-8984ff51-37a1-4d90-a40b-1d600a7077fa.png)

----------------------------------------

## Soal 3
Buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie

**Jawaban**
- Edit file ```/etc/bind/kaizoku/franky.a12.com```
![Screenshot (1761)](https://user-images.githubusercontent.com/71380876/139348273-46d5f341-6421-4ece-940d-ee6c9a77b624.png)

- Restart bind9 dengan ```service bind9 restart```

- Testing dengan cara `ping super.franky.a12.com` dan `ping www.super.franky.a12.com` pada Loguetown
![Screenshot (1762)](https://user-images.githubusercontent.com/71380876/139348441-a5da0d7c-ec68-47b7-889d-6a17db77aca8.png)

----------------------------------------

## Soal 4
Buat reverse domain untuk domain utama

**Jawaban**
- Edit file `/etc/bind/named.conf.local` seperti berikut
![Screenshot (1906)](https://user-images.githubusercontent.com/71380876/139348547-941c06a9-fe1d-47ab-9bd6-56a9182e85ad.png)

- Copy kan db.local ke dalam folder kaizoku dan ubah nama nya menjadi ```2.5.10.in-addr.arpa```<br>
```cp /etc/bind/db.local /etc/bind/kaizoku/2.5.10.in-addr.arpa```

- Edit file ```/etc/bind/kaizoku/2.5.10.in-addr.arpa```
![Screenshot (1907)](https://user-images.githubusercontent.com/71380876/139348737-787dd4bd-9916-4769-af84-4d4ee0c3f1f1.png)

- Restart bind9 dengan ```service bind9 restart```

- Testing pada Loguetown dengan command ```host -t PTR 10.5.2.2```<br>
![Screenshot (1898)](https://user-images.githubusercontent.com/71380876/139348912-e76888fd-2431-4677-8bda-6a0e94a425ce.png)

------------------------------------------

## Soal 5
Buat Water7 sebagai DNS Slave untuk domain utama

**Jawaban**
- Edit file `/etc/bind/named.conf.local` seperti berikut
![Screenshot (1909)](https://user-images.githubusercontent.com/71380876/139349292-b2244ef8-a844-4cd5-8db6-1d2c261fbddf.png)

- Restart bind9 dengan ```service bind9 restart```

- Lakukan instalasi bind9 pada Water7 dengan command `apt-get install bind9 -y`

- Edit file `/etc/bind/named.conf.local` seperti berikut
![Screenshot (1910)](https://user-images.githubusercontent.com/71380876/139349634-ec84d59e-eb6c-4792-8fe6-1d2be989ea5c.png)

- Restart bind9 dengan ```service bind9 restart```

- Matikan service bind9 pada EniesLobby dengan comman ```service bind9 stop```

- Pada Loguetown edit file ```etc.resolv.conf```
![Screenshot (1911)](https://user-images.githubusercontent.com/71380876/139349822-10d57089-9d1b-4b3b-bcf7-e5feb9450cb6.png)

- Testing pada Loguetown dengan comman ```ping franky.a12.com```
![Screenshot (1912)](https://user-images.githubusercontent.com/71380876/139349943-1182e88c-463d-4fdd-aa16-925d78cfd1bc.png)

------------------------------------------

## Soal 6
Buat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo


**Jawaban**
- Pada EniesLobby edit file ```/etc/bind/kaizoku/franky.a12.com``` sebagai berikut
![Screenshot (1913)](https://user-images.githubusercontent.com/71380876/139352033-a5c24de1-0815-4b51-ab1e-63cf92205bd7.png)

- Edit file ```/etc/bind/named.conf.options``` sebagai berikut
![Screenshot (1914)](https://user-images.githubusercontent.com/71380876/139352205-eb58e7f5-ff87-4f8b-b9df-1a8ba0241ee7.png)

- Edit file ```/etc/bind/named.conf.local``` sebagai berikut
![Screenshot (1915)](https://user-images.githubusercontent.com/71380876/139352328-0fd65502-d937-4a4f-83fd-bd76142d7c43.png)

- Restart bind9 dengan ```service bind9 restart```

- Pada Water7 edit file ```/etc/bind/named.conf.options``` sebagai berikut
![Screenshot (1917)](https://user-images.githubusercontent.com/71380876/139352545-9215d2bc-b487-432a-a10d-df020d80f888.png)

- Edit file ```/etc/bind/named.conf.local``` sebagai berikut
![Screenshot (1918)](https://user-images.githubusercontent.com/71380876/139352680-6f246992-de6a-4be0-8871-242de7e572dc.png)

- Buat directory sunnygo dengan command ```mkdir /etc/bind/sunnygo```

- Copy kan db.local ke dalam folder sunygo dan ubah nama nya menjadi ```mecha.franky.a12.com```<br>
```cp /etc/bind/db.local /etc/bind/sunnygo/mecha.franky.a12.com```

- Edit file ```/etc/bind/sunnyg0/mecha.franky.a12.com``` sebagai berikut
![Screenshot (1916)](https://user-images.githubusercontent.com/71380876/139352497-b9d8c9ed-9758-4ed3-ac8c-d8000db8d571.png)

- Restart bind9 dengan ```service bind9 restart```

- Testing pada Loguetown dengan comman ```ping mecha.franky.a12.com``` dan ```www.mecha.franky.a12.com```
![Screenshot (1919)](https://user-images.githubusercontent.com/71380876/139352886-2dddace4-2529-4e13-ace0-f214744846d9.png)

-------------------------------------------

## Soal 7
Buat subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie

**Jawaban**
### Pada Water7
- Pada Water7 edit file ```/etc/bind/sunnygo/mecha.franky.a12.com``` sebagai berikut
![Screenshot (1920)](https://user-images.githubusercontent.com/71380876/139353234-4754addd-73ff-44e7-822c-479682fd1d5d.png)

- Restart bind9 dengan ```service bind9 restart```

- Testing pada Loguetown dengan comman ```ping general.mecha.franky.a12.com``` dan ```www.general.mecha.franky.a12.com```
![Screenshot (1921)](https://user-images.githubusercontent.com/71380876/139353323-41ff19fc-01d3-480b-a8c9-c5644d2c874b.png)


## Soal 8

Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com.

**Jawaban**
### Pada Skype

__pada console skype__ Install aplikasi apache, PHP, dan libapache2-mod-php7.0.

lakukan ```apt-get update``` sebelumnya, lalu install aplikasi di atas

```apt-get install apache2 -y```

```apt-get install php -y```

```apt-get install libapache2-mod-php7.0 -y```

masuk ke dalam directory :

```cd /etc/apache2/sites-available.```


Copy file 000-default.conf dan ubah nama menjadi file franky.a12.com.conf.

```cp 000-default.conf franky.a12.com.conf```

Edit file franky.a12.com.conf seperti gambar berikut ini:

![image](https://user-images.githubusercontent.com/81466736/139534155-402f19e9-d899-458c-9cb5-a3e69adad284.png)

pada directory /var/www.

```cd /var/www.```

Download file zip yang telah diberikan : ```https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/franky.zip```
dengan menggunakan wget ``` apt-get install wget -y ``` dan lakukan zip ``` apt-get install unzip -y ``` install aplikasi tersebut sebelumnya

```unzip franky.zip```

Rename folder franky menjadi franky.a12.com

``` mv franky franky.a12.com ```


Aktifkan konfigurasi pada franky.a12.com dengan command:

```a2ensite franky.a12.com```

lakukan restart pada apache

``` service apache2 restart ```

__pada Loguetown__

install aplikasi lynx

``` apt-get install lynx ```

buka franky.a12.com dengan menggunakan lynx

```lynx franky.a12.com```

![8_3](https://user-images.githubusercontent.com/81466736/139532192-8551fd1d-2499-4b06-ba3b-4ff46afb948f.JPG)


## Soal 9

**Jawaban**

Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home. 

__Pada Skypie__

tuliskan command 

```a2enmod rewrite``` 

untuk dapat mengaktifkan module rewrite.


Restart apache dengan command 
```service apache2 restart.```

buat dan edit file baru .htaccess di folder /var/www/franky.a12.com seperti berikut :

```vim /var/www/franky.a12.com/.htaccess```

![image](https://user-images.githubusercontent.com/81466736/139532385-15eb6170-8203-40f3-be27-c97a0e248f73.png)

pada directory 

```/etc/apache2/sites-available.```

edit file franky.a12.com.conf dan tambahkan beberapa line sebagai berikut agar file .htaccess yang tadi dibuat dapat diakses:
```vim /etc/apache2/sites-available/franky.a12.com.conf```

![image](https://user-images.githubusercontent.com/81466736/139532491-f5dc214f-1943-432d-841c-ead6a4851679.png)

Restart apache dengan command 

```service apache2 restart.```

__pada Loguetown__

buka franky.a12.com/home dengan menggunakan lynx

```lynx franky.a12.com/home```

![8_3](https://user-images.githubusercontent.com/81466736/139532532-2aa4fbb0-6965-48c6-a64b-8b7637a73bbf.JPG)


## Soal 10
Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com


**Jawaban**

__pada console skype__ 

masuk ke dalam directory :

```cd /etc/apache2/sites-available.```


Copy file 000-default.conf dan ubah nama menjadi file super.franky.a12.com.conf.

```cp 000-default.conf super.franky.a12.com.conf```

Edit file super.franky.a12.com.conf seperti gambar berikut ini:

``` vim super.franky.a12.com.conf ```

![10_1](https://user-images.githubusercontent.com/81466736/139532766-e10f7c89-4297-4988-bead-bd503a086beb.JPG)


pada directory /var/www.

```cd /var/www.```

Download file zip yang telah diberikan : ```hwget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/super.franky.zip```
dan lakukan unzip 

```unzip super.franky.zip```

Rename folder franky menjadi super.franky.a12.com

``` mv super.franky super.franky.a12.com ```


Aktifkan konfigurasi pada franky.a12.com dengan command:

```a2ensite super.franky.a12.com```

lakukan restart pada apache

``` service apache2 restart ```

__pada Loguetown__

buka super.franky.a12.com dengan menggunakan lynx

```lynx super.franky.a12.com```

![10_2](https://user-images.githubusercontent.com/81466736/139532812-d41d28ac-1094-40b2-9580-c779832cb9ba.JPG)


## Soal 11

Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja.

**Jawaban**

__pada console skype__ 
 ketikan : ```cd /etc/apache2/sites-available``` untuk pindah ke directory tersebut
 
 di directory tersebut edit file super.franky.a12.com.conf

```vim super.franky.a12.com.conf``` seperti gambar berikut :

![image](https://user-images.githubusercontent.com/81466736/139533051-76ea3157-d490-459f-8cf5-e8f809bcd92d.png)
 
 lakukan restart pada apache

``` service apache2 restart ```

buka super.franky.a12.com/public dengan menggunakan lynx

```lynx super.franky.a12.com/public```

![image](https://user-images.githubusercontent.com/81466736/139533138-54a0f68c-23e5-40b4-9981-274a427bdd9a.png)


dan kemudian coba untuk membuka lynx ```super.franky.a12.com/public/css``` dan kemudian /js lalu /images

![image](https://user-images.githubusercontent.com/81466736/139533227-cc19f48a-c4e6-4d76-b5f0-1f0258f1398b.png)


## Soal 12

Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache 

**Jawaban**

__pada console skype__ 
 ketikan : ```cd /etc/apache2/sites-available``` untuk pindah ke directory tersebut
 
 di directory tersebut edit file super.franky.a12.com.conf

```vim super.franky.a12.com.conf``` seperti gambar berikut :

![12_conf](https://user-images.githubusercontent.com/81466736/139533397-1ff3af9c-5b62-4977-ac96-3d207fd2b0f9.JPG)

lakukan restart pada apache

``` service apache2 restart ```

buka super.franky.a12.com/pubji dengan menggunakan lynx maka akan menampilkan pesan error 404

``` lynx super.franky.a12.com/pubji ```

![image](https://user-images.githubusercontent.com/81466736/139533498-6ea11eca-22a5-445c-ac9a-881155d15d45.png)

## Soal 13
Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js.
**Jawaban**

__pada console skype__ 

ketikan : ```cd /etc/apache2/sites-available``` untuk pindah ke directory tersebut
 
 di directory tersebut edit file super.franky.a12.com.conf

```vim super.franky.a12.com.conf``` seperti gambar berikut :

![13_conf](https://user-images.githubusercontent.com/81466736/139533584-a297e5af-2a8c-40d3-8117-a2a1d538f612.JPG)

  lakukan restart pada apache

``` service apache2 restart ```

buka super.franky.a12.com/js dengan menggunakan lynx

```lynx super.franky.a12.com/js```

![image](https://user-images.githubusercontent.com/81466736/139533666-e986e179-eadf-4b88-8d47-e822cae62fa8.png)

## Soal 14

Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500

**Jawaban**

__pada console skype__ 
masuk ke dalam directory :

```cd /etc/apache2/sites-available.```


Copy file 000-default.conf dan ubah nama menjadi file general.mecha.franky.a12.com.conf.

```cp 000-default.conf general.mecha.a12.com.conf```

Edit file general.mecha.franky.a12.com.conf seperti gambar berikut ini:

``` vim general.mecha.franky.a12.com.conf ```

![image](https://user-images.githubusercontent.com/81466736/139533876-d59b5eeb-a299-487b-b15e-8101b6721bc6.png)


Edit file /etc/apache2/ports.conf untuk dapat mengaktifkan port 15000 dan port 15500 dan membuat comment pada port 80 seperti pada gambar berikut:

``` vim /etc/apache2/ports.conf ```

![image](https://user-images.githubusercontent.com/81466736/139533914-18740004-524f-4c61-a111-7bfe3a3a03be.png)


pada directory /var/www.

```cd /var/www.```

Download file zip yang telah diberikan : ```wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/general.mecha.franky.zip```
dan lakukan unzip 

```unzip general.mecha.franky.zip```

Rename folder franky menjadi general.mecha.franky.a12.com

```mv general.mecha.franky general.mecha.franky.a12.com ```


Aktifkan konfigurasi pada general.mecha.franky.a12.com dengan command:

```a2ensite general.mecha.franky.a12.com```

lakukan restart pada apache

``` service apache2 restart ```

__pada Loguetown__

buka general.mecha.franky.a12.com:15000 dengan menggunakan lynx

![image](https://user-images.githubusercontent.com/81466736/139534083-9964e4e3-7879-4088-af73-7d2de311929f.png)



## Kendala
 pada soal nomor 15 tidak dapat memasukkan new password untuk username luffy
 
 masih sering terjadi error sehingga sering mengulang konfigurasi dari awal dan tidak sempat mengerjakan nomor 16 dan 17
