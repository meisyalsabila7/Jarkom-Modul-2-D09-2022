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

### Pengerjaan
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

### Pengerjaan
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

* Wise
```
echo -e '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     wise.D09.com.   root.wise.D09.com. (
                                2       ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800  )       ; Negative Cache TTL
;
@               IN      NS      wise.D09.com.
@               IN      A       192.189.2.2             ; IP WISE
www             IN      CNAME   wise.D09.com.
eden            IN      A       192.189.3.3             ; IP Eden
www.eden        IN      CNAME   eden.wise.D09.com.      
@               IN      AAAA    ::1
' > /etc/bind/wise/wise.D09.com

service bind9 restart
```

* SSS & Garden
```
echo -e "======================================"
echo -e "TEST PING SUBDOMAIN (mengarah ke host IP Eden)"
ping eden.wise.D09.com -c 5
echo -e "======================================"
echo -e "TEST ALIAS dari eden.wise.D09.com"
host -t CNAME www.eden.wise.D09.com
echo -e "======================================"
```

### Dokumentasi
<img width="480" alt="Screenshot_20221029_215522" src="https://user-images.githubusercontent.com/94627623/198839272-0ce7ca29-b5bd-464c-83f3-baeb9bf7ce21.png">
<img width="479" alt="Screenshot_20221029_215650" src="https://user-images.githubusercontent.com/94627623/198839273-1466e21f-934b-47fd-a292-42ba21ca93d7.png">
<img width="479" alt="Screenshot_20221029_215703" src="https://user-images.githubusercontent.com/94627623/198839277-4106ae6b-8bfe-4e36-860d-e6228eeae688.png">


## Soal Nomor 4
> Buat juga reverse domain untuk domain utama

* Wise
```
echo -e '
zone "wise.D09.com" {
        type master;
        file "/etc/bind/wise/wise.D09.com";
};

zone "2.189.192.in-addr.arpa"{
        type master;
        file "/etc/bind/wise/2.189.192.in-addr.arpa";
};

' > /etc/bind/named.conf.local

echo -e '
$TTL    604800
@       IN      SOA     wise.D09.com.   root.wise.D09.com. (
                                2       ; Serial
                        604800          ; Refresh
                        86400           ; Retry
			2419200         ; Expire
                        604800 )        ; Negative Cache TTL
;
2.189.192.in-addr.arpa. IN      NS      wise.D09.com.
2                       IN      PTR     wise.D09.com.
' > /etc/bind/wise/2.189.192.in-addr.arpa

service bind9 restart
```

* SSS & Garden
```
echo -e "=================================="
echo -e "TES REVERSE DOMAIN: "
host -t PTR 192.189.2.2
echo -e "=================================="
```

### Dokumentasi
<img width="929" alt="Screenshot_20221029_220243" src="https://user-images.githubusercontent.com/94627623/198839360-26349099-5184-47da-9369-be743ad3cc97.png">
<img width="930" alt="Screenshot_20221029_220255" src="https://user-images.githubusercontent.com/94627623/198839361-c7281d90-ecb7-41c8-bce7-3a3de70ec3f3.png">
<img width="930" alt="Screenshot_20221029_220306" src="https://user-images.githubusercontent.com/94627623/198839365-1e025083-76d9-4428-b2f8-51d8fc46de63.png">


## Soal Nomor 5
> Agar dapat tetap dihubungi jika server WISE bermasalah, buatlah juga Berlint sebagai DNS Slave untuk domain utama

* Wise
```
echo -e '
zone "wise.D09.com" {
        type master;
        notify yes;
        also-notify { 192.189.3.2; };
        allow-transfer { 192.189.3.2; };
        file "/etc/bind/wise/wise.D09.com";
};

zone "2.189.192.in-addr.arpa"{
        type master;
        file "/etc/bind/wise/2.189.192.in-addr.arpa";
};
' > /etc/bind/named.conf.local

service bind9 restart
```

* Berlint
```
echo -e '
zone "wise.D09.com" {
        type slave;
        masters { 192.189.2.2; };
        file "/var/lib/bind/wise.D09.com";
};
' > /etc/bind/named.conf.local

service bind9 restart
```

* SSS & Garden
```
echo -e '
nameserver 192.189.2.2  ; IP WISE
nameserver 192.189.3.2  ; IP Berlint
nameserver 192.168.122.1
' > /etc/resolv.conf

echo -e "========================================="
echo "TES SLAVE DNS, DNS MASTER DIBERHENTIKAN"
ping wise.D09.com -c 5
echo -e "========================================="
```

### Dokumentasi
<img width="742" alt="Screenshot_20221029_220829" src="https://user-images.githubusercontent.com/94627623/198839449-baeba9a2-9b73-4e49-8fcb-8746883fdf2d.png">
<img width="742" alt="Screenshot_20221029_220838" src="https://user-images.githubusercontent.com/94627623/198839460-f8ed8f4a-feb5-4a8e-b9ba-082d0bf612d9.png">
<img width="757" alt="Screenshot_20221029_220900" src="https://user-images.githubusercontent.com/94627623/198839464-f0eab11d-af22-45ca-98fe-fb2f4c424f4a.png">
<img width="758" alt="Screenshot_20221029_220910" src="https://user-images.githubusercontent.com/94627623/198839467-f7330612-2739-4fa9-88fa-d3313c71467f.png">



## Kendala
* Jumlah soal dan waktu kurang seimbang
* Waktu pengerjaan praktikum kurang tepat karena dibarengi dengan minggu2 ETS
