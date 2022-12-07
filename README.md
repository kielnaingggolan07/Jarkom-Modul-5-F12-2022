# Jarkom-Modul-5-F12-2022

Nama Anggota | NRP
------------------- | --------------
Hesekiel Nainggolan | 5025201054
Khuria Khusna | 5025201053
Afiq Akram | 5025201270

## (A) Topologi
<img width="553" alt="awal" src="https://user-images.githubusercontent.com/94334247/206191274-b761a80e-5887-412f-93d2-61da3ca87559.PNG">

## Keterangan Node:

- Eden adalah DNS Server
- WISE adalah DHCP Server
- Garden dan SSS adalah Web Server
- Jumlah Host pada Forger adalah 62 host
- Jumlah Host pada Desmond adalah 700 host
- Jumlah Host pada Blackbell adalah 255 host
- Jumlah Host pada Briar adalah 200 host

## Penghitungan Jumlah Subnet
![image](https://user-images.githubusercontent.com/94334247/206191679-433c02ff-aca7-4ec1-9ea6-6e0e58efde50.png)

## Pembagian IP
![image](https://user-images.githubusercontent.com/94334247/206192599-125bf106-100a-4037-b67b-7a319657b17f.png)

## Network Configuration

### Strix

```
auto eth0
iface eth0 inet static
	address 192.168.122.11
	netmask 255.255.255.0
        gateway 192.168.122.1
        up echo nameserver 192.168.122.1 > /etc/resolv.conf

auto eth1
iface eth1 inet static
address 192.205.0.109
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 192.205.0.105
netmask 255.255.255.252

```

### Ostania

```
auto eth0
iface eth0 inet static
address 192.205.0.110
netmask 255.255.255.252
gateway 192.205.0.109
up echo nameserver 192.168.122.1 > /etc/resolv.conf

auto eth1
iface eth1 inet static
address 192.205.2.1
netmask 255.255.254.0

auto eth2
iface eth2 inet static
address 192.205.0.121
netmask 255.255.255.248

auto eth3
iface eth3 inet static
address 192.205.1.1
netmask 255.255.255.0
```

### Westalis

```
auto eth0
iface eth0 inet static
address 192.205.0.106
netmask 255.255.255.252
gateway 192.205.0.105
up echo nameserver 192.168.122.1 > /etc/resolv.conf

auto eth1
iface eth1 inet static
address 192.205.4.1
netmask 255.255.252.0

auto eth2
iface eth2 inet static
address 192.205.0.129
netmask 255.255.255.128

auto eth3
iface eth3 inet static
address 192.205.0.113
netmask 255.255.255.248
```

### Wise

```
auto eth0
iface eth0 inet static
address 192.205.0.115
netmask 255.255.255.248
gateway 192.205.0.113
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Eden

```
auto eth0
iface eth0 inet static
address 192.205.0.114
netmask 255.255.255.248
gateway 192.205.0.113
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Garden

```
auto eth0
iface eth0 inet static
address 192.205.0.122
netmask 255.255.255.248
gateway 192.205.0.121
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### SSS
```
auto eth0
iface eth0 inet static
address 192.205.0.123
netmask 255.255.255.248
gateway 192.205.0.121
up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Forger, Desmond, Briar, Blackbell

```
auto eth0
iface eth0 inet dhcp 
```

## (C) Routing

### Strix

```
route add -net 192.205.0.112 netmask 255.255.255.248 gw 192.205.0.106
route add -net 192.205.0.128 netmask 255.255.255.128 gw 192.205.0.106
route add -net 192.205.4.0 netmask 255.255.252.0 gw 192.205.0.106

route add -net 192.205.0.120 netmask 255.255.255.248 gw 192.205.0.110
route add -net 192.205.1.0 netmask 255.255.255.0 gw 192.205.0.110
route add -net 192.205.2.0 netmask 255.255.254.0 gw 192.205.0.110
``` 

### DHCP Server

1. Pada **WISE** sebagai DHCP Server, dilakukan instalasi DHCP Server dan netcat

```
apt-get update
apt-get install isc-dhcp-server -y
Apt-get install netcat
```

2. Konfigurasi file `/etc/default/isc-dhcp-server` pada **WISE** sebagai DHCP Server

```
INTERFACES="eth0"
```

3. Konfigurasi file `/etc/dhcp/dhcpd.conf` pada **WISE** sebagai DHCP Server

```
echo '
# blackbell(A7)
subnet 192.205.2.0 netmask 255.255.254.0 {
    range 192.205.2.2 192.205.3.254;
    option routers 192.205.2.1;
    option broadcast-address 192.205.3.255;
    option domain-name-servers 192.205.0.114;
    default-lease-time 360;
    max-lease-time 7200;
}

# A1
subnet 192.205.0.112 netmask 255.255.255.248 {
}

# Briar (A6)
subnet 192.205.1.0 netmask 255.255.255.0 {
    range 192.205.1.2 192.205.1.254;
    option routers 192.205.1.1;
    ption broadcast-address 192.205.1.255;
    option domain-name-servers 192.205.0.114;
    max-lease-time 7200;
}

# Forger (A2)
subnet 192.205.0.128 netmask 255.255.255.128 {
    range 192.205.0.130 192.205.0.254;
    option routers 192.205.0.129;
    option broadcast-address 192.205.0.255;
    option domain-name-servers 192.205.0.114;
    default-lease-time 360;
    max-lease-time 7200;
}

# Desmon(A3)
subnet 192.205.4.0 netmask 255.255.252.0 {
    range 192.205.4.2 192.205.7.254;
    option routers 192.205.4.1;
    option broadcast-address 192.205.7.255;
    option domain-name-servers 192.205.0.114;
    default-lease-time 360;
    max-lease-time 7200;
}
' > /etc/dhcp/dhcpd.conf
```
4. Menjalankan DHCP Server

```
service isc-dhcp-server start
```

### DHCP Relay

Dengan topologi yang ada, **Westalis** dan **Ostania**  dan **srrix** akan bekerja sebagai DHCP Relay

1. Pada **Westalis** dan **Ostania** dan **Strix** sebagai DHCP Relay, dilakukan instalasi DHCP Relay

```
apt-get update
apt-get install isc-dhcp-relay -y
apt-get install netcat
```

2. Konfigurasi file `/etc/default/isc-dhcp-relay` pada **Westalis** dan **Ostania** sebagai DHCP Relay

```
SERVERS="192.205.0.115"
INTERFACES="eth0 eth1 eth2 eth3"
OPTIONS=""
```

### DNS Forwarder

Pada **Eden** sebagai DNS Server, dilakukan instalasi Bind9

```
apt-get update
apt-get install bind9 -y
service bind9 start
```
Kemudian konfigurasi DNS Forwarder pada file `/etc/bind/named.conf.options`

```
echo 'options {
        directory "/var/cache/bind";

        forwarders{
                192.205.0.106;
        };

        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;     # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

```

Setelah itu restart bind9 dengan `service bind9 restart`


### Testing

#### Forgen
<img width="436" alt="testingdhcp1" src="https://user-images.githubusercontent.com/94334247/206196669-24cb4401-dae6-478e-b91a-2d4d95ca72bd.PNG">

#### Desmond
<img width="408" alt="testingdhcp2" src="https://user-images.githubusercontent.com/94334247/206196753-c77e948b-1f6a-42ab-8fe7-0fa82f2aca1e.PNG">

#### Briar
<img width="432" alt="testingdhcp3" src="https://user-images.githubusercontent.com/94334247/206196826-9fea6594-ec14-4555-8b78-9094cf7bfecf.PNG">

#### Blackbell
<img width="431" alt="testingdhcp4" src="https://user-images.githubusercontent.com/94334247/206196868-a5b27c3f-e53f-4b32-9c9d-2982c87c5167.PNG">





