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
WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden. Semua node terhubung pada router Ostania, sehingga dapat mengakses internet.

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
