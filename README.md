# Jarkom-Modul-2-D09-2022

Pembahasan soal praktikum Modul 2 Jaringan Komputer 2022

**Anggota Kelompok**:

- Monica Narda Davita - 5025201009
- Alya Shofarizqi Inayah - 5025201113
- Meisya Salsabila Indrijo Putri - 5025201114
---

## Topologi
![topologi d09](https://user-images.githubusercontent.com/96837287/198829490-0474bd38-4087-4563-9ab3-9e9ff0b3ebc8.jpg)

## Konfigurasi
- Ostania
```sh
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.189.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.189.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.189.3.1
	netmask 255.255.255.0
```

- Wise
```
auto eth0
iface eth0 inet static
	address 192.189.3.2
	netmask 255.255.255.0
	gateway 192.189.3.1
```

- SSS
```
auto eth0
iface eth0 inet static
	address 192.189.1.2
	netmask 255.255.255.0
	gateway 192.189.1.1
```

- Garden
```
auto eth0
iface eth0 inet static
	address 192.189.1.3
	netmask 255.255.255.0
	gateway 192.189.1.1
```

- Berlint
```
auto eth0
iface eth0 inet static
	address 192.189.2.2
	netmask 255.255.255.0
	gateway 192.189.2.1
```

- Eden
```
auto eth0
iface eth0 inet static
	address 192.189.2.3
	netmask 255.255.255.0
	gateway 192.189.2.1
```

## Soal Nomor 1
> WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden. Semua node terhubung pada router Ostania, sehingga dapat mengakses internet.

Melakukan pengecekan internet ke semua node dengan `bash nomor1.sh` setelah setting konfigurasi dan menjalankan start command. script tersebut terdapat pada root semua node.

```
echo -e "=============================================="
echo "PING GOOGLE: "
ping google.com -c 5
echo -e "=============================================="
```

### Dokumentasi
![Ostania no1](https://user-images.githubusercontent.com/96837287/198835583-c2a67b81-8b1d-4bb7-9453-a729ca75651a.jpg) 

![Wise no1](https://user-images.githubusercontent.com/96837287/198835591-46fd5ee8-b184-4536-8d2e-7a8364058665.jpg)

![Berlint no1](https://user-images.githubusercontent.com/96837287/198835595-21ae3742-0220-401f-b5c0-4fbcc97fa33e.jpg)

![Eden no1](https://user-images.githubusercontent.com/96837287/198835600-76568997-2991-41bd-ab8a-e567b879183e.jpg)

![Garden No 1](https://user-images.githubusercontent.com/96837287/198838558-01d4b882-56bb-40e3-8f31-4f08d031d8e8.jpg)

![sss no1](https://user-images.githubusercontent.com/96837287/198838560-2a3af9ad-23a1-420e-8134-674a4cc4610b.jpg)

## Soal Nomor 2
Untuk mempermudah mendapatkan informasi mengenai misi dari Handler, bantulah loid membuat website utama dengan akses wise.yyy.com dengan alias `www.wise.yyy.com`

Sebelum itu, konfigurasikan `/etc/bind/named.conf.local` pada DNS Mater yaitu node WISE dengan domain wise.D09.com. Setelah dikonfigurasikan, buatlah direktori `/etc/bind/wise`. Kemudian, buatlah file `wise.D09.com` setelah command `mkdir /etc/bind/wise` pada direktori yang baru saja dibuat dan isilah file seperti dibawah ini. Setelah selesai maka menambahkan command `service bind9 restart` untuk merestart bind9. Untuk menjalankannya gunakan command `bash nomor2.sh`

```
echo -e '
zone "wise.D09.com" {
        type master;
        file "/etc/bind/wise/wiseD09.com";
};
 ' > /etc/bind/named.conf.local

mkdir /etc/bind/wise

echo -e '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.D09.com. root.wise.D09.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      wise.D09.com.
@       IN      A       192.189.3.2	; IP WISE
www	IN	CNAME	wise.D09.com.
' > /etc/bind/wise/wise.D09.com

service bind9 restart
```

Kemudian setting nameserver pada node client yaitu node SSS dan node Garden. Lalu, melakukan pengecekan dengan `host =t CNAME www.wise.D09.com` dan `www.wise.D09.com -c 3`. Untuk menjalankannya gunakan command `bash nomor2.sh`

```
echo -e '
nameserver 192.189.2.2  ; IP WISE
nameserver 192.168.122.1
' > /etc/resolv.conf

echo -e "==============================="
host -t CNAME www.wise.D09.com
echo -e "TEST ALIAS wise.D09.com"
echo -e "==============================="
echo -e "TEST ARAH PING www.wise.D09.com (mengarah ke IP WISE)"
ping www.wise.D09.com -c 5
echo -e "==============================="
```

### Dokumentasi
![sss no2](https://user-images.githubusercontent.com/96837287/198838564-78848863-bf9f-4dbf-91e6-b31557c1e9a1.jpg)

![garden no2](https://user-images.githubusercontent.com/96837287/198838570-c6f43e96-cc68-49e9-bdc8-79412aed8aac.jpg)

## Soal Nomor 3
> Setelah itu ia juga ingin membuat subdomain `eden.wise.yyy.com` dengan alias `www.eden.wise.yyy.com` yang diatur DNS-nya di `WISE` dan mengarah ke `Eden`


## Soal Nomor 4
> Buat juga reverse domain untuk domain utama


## Soal Nomor 1
> Agar dapat tetap dihubungi jika server WISE bermasalah, buatlah juga Berlint sebagai DNS Slave untuk domain utama
