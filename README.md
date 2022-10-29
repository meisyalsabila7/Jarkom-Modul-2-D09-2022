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
