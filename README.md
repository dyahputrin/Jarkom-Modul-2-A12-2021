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
- Pada Water7 edit file ```/etc/bind/sunnygo/mecha.franky.a12.com``` sebagai berikut
![Screenshot (1920)](https://user-images.githubusercontent.com/71380876/139353234-4754addd-73ff-44e7-822c-479682fd1d5d.png)

- Restart bind9 dengan ```service bind9 restart```

- Testing pada Loguetown dengan comman ```ping general.mecha.franky.a12.com``` dan ```www.general.mecha.franky.a12.com```
![Screenshot (1921)](https://user-images.githubusercontent.com/71380876/139353323-41ff19fc-01d3-480b-a8c9-c5644d2c874b.png)













