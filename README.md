# Jarkom-Modul-4-B23-2023
# **Praktikum Modul 4 Jaringan Komputer**
Berikut adalah Repository dari Kelompok B23 untuk pengerjaan Praktikum Modul 4. Dalam Repository ini terdapat daftar anggota kelompok, dokumentasi dan Penjelasan tiap soal, _Screenshot_, dan Kendala yang Dialami. 

# **Anggota Kelompok**
| Nama                      | NRP        | Kelas               |
| ------------------------- | ---------- | ------------------- |
| Armadya Hermawan Sarwono  | 5025211243 | Jaringan Komputer B |
| Dian Lies Seviona Dabukke | 5025211080 | Jaringan Komputer B |

# **Dokumentasi dan Penjelasan Soal**
Berikut adalah dokumentasi yang tiap soal dan penjelasan terkait perintah yang digunakan :

# **VLSM dengan GNS3**
Teknik ini pengefisiensian pembagian IP(subnetting) dengan menyesuaikan besar netmask sesuai banyaknya host/komputer yang membutuhkannya. 
![topologi](https://github.com/lalaladi/Jarkom-Modul-2-B23-2023/assets/90541607/6e39596b-bd32-4307-b7d6-b6b5de036e9e)
<br>
<br>
Langkah pertama tandai subnet pada gambar topologi anda 
![tandai](https://github.com/lalaladi/Jarkom-Modul-2-B23-2023/assets/90541607/c9cab4d2-326a-41f6-9471-8e36dff41596)
<br>                            
2. Langkah kedua yang dilakukan dapat dengan menentukan jumlah alamat IP yang dibutuhkan seperti tabel dibawah : 
|  Subnet |  Jumlah IP  |  Netmask  |
| ------  | ----- | ------- |
|   A1    |  512  |   /22   |
|   A2    |  31   |   /26   |
|   A3    |   2   |   /30   |
|   A4    |  255  |   /23   |
|   A5    |   2   |   /30   |
|   A6    |   2   |   /30   |
|   A7    |  251  |   /24   |
|   A8    | 1001  |   /22   |
|   A9    |   2   |   /30   |
|   A10   |   2   |   /30   |
|   A11   |  127  |   /24   |
|   A12   |   2   |   /30   |
|   A13   |   25  |   /27   |
|   A14   |   2   |   /30   |
|   A15   |   2   |   /30   |
|   A16   |  1023 |   /21   |
|   A17   |  1001 |   /22   |
|   A18   |   2   |   /30   |
|   A19   |   6   |   /29   |
|   A20   |   3   |   /29   |
|   A21   |   2   |   /30   |
|  **TOTAL**  |  **4255** |   **/19**   |

<br>
3. Sehingga akan dihasilkan sebuah tree yang dimana IP-nya akan diatur tiap interface sesuai dengan aturan subnetting yang darinya didapatkan Network ID, Broadcast Address, dan Available Hosts. Kita dapat lihat di bagian A1, switch8 akan terhubung ke 510 host-server Sein-Heiter sehingga total IP agar terhubung antar kakinya ada 512.
<img width="4539" alt="tree" src="https://github.com/lalaladi/Jarkom-Modul-2-B23-2023/assets/90541607/ca23f508-a991-40bb-a98b-880b42c22781">
<br>
Sebelum melakukan konfigurasi dan routing : <br>

* SIMPAN ROUTING DALAM : _/root/rute.sh_ 
* BUKA : _/etc/sysctl.conf_ -> UNCOMMENT : _net.ipv4.ip_forward=1_ (PADA SEMUA ROUTER) DAN SIMPAN DI _/root/sysctl.conf_  
* PADA _root/.bashrc_ di AURA TAMBAHKAN : _iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.20.0.0/16_ 
dan PADA _root/.bashrc_ di tiap node TAMBAHKAN : _echo nameserver 192.168.122.1 > /etc/resolv.conf_
<br>
<br>

**Aura (ROUTER)**
<br>
Configurasi : 
```bash
Aura -> Cloud
auto eth0
iface eth0 inet dhcp
 
Aura -> Denken
auto eth1
iface eth1 inet static
    	address 10.20.0.17
    	netmask 255.255.255.252
 
Aura -> Frieren
auto eth2
iface eth2 inet static
     	address 10.20.0.21
     	netmask 255.255.255.252
 
Aura -> Eisen
auto eth3
iface eth3 inet static
    	address 10.20.0.13
    	netmask 255.255.255.252
```
Routing :
```bash
route add -net 10.20.8.0 netmask 255.255.252.0 gw 10.20.0.14
route add -net 10.20.0.128 netmask 255.255.255.192 gw 10.20.0.14
route add -net 10.20.0.0 netmask 255.255.255.252 gw 10.20.0.14
route add -net 10.20.4.0 netmask 255.255.254.0 gw 10.20.0.14
route add -net 10.20.0.4 netmask 255.255.255.252 gw 10.20.0.14
route add -net 10.20.0.8 netmask 255.255.255.252 gw 10.20.0.14
route add -net 10.20.1.0 netmask 255.255.255.0 gw 10.20.0.14
route add -net 10.20.12.0 netmask 255.255.252.0 gw 10.20.0.14
route add -net 10.20.0.48 netmask 255.255.255.248 gw 10.20.0.14
route add -net 10.20.0.36 netmask 255.255.255.252 gw 10.20.0.14
route add -net 10.20.2.0 netmask 255.255.255.0 gw 10.20.0.18
route add -net 10.20.0.64 netmask 255.255.255.224 gw 10.20.0.22
route add -net 10.20.0.24 netmask 255.255.255.252 gw 10.20.0.22
route add -net 10.20.0.28 netmask 255.255.255.252 gw 10.20.0.22
route add -net 10.20.24.0 netmask 255.255.248.0 gw 10.20.0.22
route add -net 10.20.16.0 netmask 255.255.252.0 gw 10.20.0.22
route add -net 10.20.0.32 netmask 255.255.255.252 gw 10.20.0.22
route add -net 10.20.0.40 netmask 255.255.255.248 gw 10.20.0.22
```
**Denken (ROUTER)**
<br>
Config :
```bash
Denken -> Aura
auto eth0
iface eth0 inet static
    	address 10.20.0.18
    	netmask 255.255.255.252
   gateway 10.20.0.17
 
 
Denken -> RoyalCapital & WilleRegion
auto eth1
iface eth1 inet static
    	address 10.20.2.1
    	netmask 255.255.255.0
``` 
Routing :
```bash
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.20.0.17
 ```

**RoyalCapital (CLIENT)**
<br>
Config :
```bash
RoyalCapital -> Denken
auto eth0
iface eth0 inet static
      	address 10.20.2.2
      	netmask 255.255.255.0
      	gateway 10.20.2.1
 ```
 
**WillieRegion (CLIENT)**
<br>
Config :
```
WilleRegion -> Denken
auto eth0
iface eth0 inet static
      	address 10.20.2.3
      	netmask 255.255.255.0
      	gateway 10.20.2.1
```bash
 
**Eisen (ROUTER)**
<br>
Config :
```bash
Eisen -> Aura
auto eth0
iface eth0 inet static
      	address 10.20.0.14
      	netmask 255.255.255.252
       gateway 10.20.0.13
 
Eisen -> Stark
auto eth3
iface eth3 inet static
      	address 10.20.0.37
      	netmask 255.255.255.252
 
Eisen -> Linie
auto eth1
iface eth1 inet static
      	address 10.20.0.5
      	netmask 255.255.255.252
 
Eisen -> Richter & Revolte
auto eth2
iface eth2 inet static
      	address 10.20.0.49
      	netmask 255.255.255.248
 
Eisen -> Lugner
auto eth4
iface eth4 inet static
      	address 10.20.0.9
      	netmask 255.255.255.252
```
 
Routing :
```bash
route add -net 10.20.12.0 netmask 255.255.252.0 gw 10.20.0.10
route add -net 10.20.1.0 netmask 255.255.255.0 gw 10.20.0.10
route add -net 10.20.0.0 netmask 255.255.255.252 gw 10.20.0.6
route add -net 10.20.0.128 netmask 255.255.255.192 gw 10.20.0.6
route add -net 10.20.8.0 netmask 255.255.252.0 gw 10.20.0.6
route add -net 10.20.4.0 netmask 255.255.254.0 gw 10.20.0.6
 ```
 
**Stark (SERVER)**
<br>
Config :
```bash
Stark -> Eisen
auto eth0
iface eth0 inet static
      	address 10.20.0.38
      	netmask 255.255.255.252
      	gateway 10.20.0.37
 ```

**Lugner (ROUTER)**
<br>
Config :
```bash
Lugner -> Eisen
auto eth0
iface eth0 inet static
      	address 10.20.0.10
      	netmask 255.255.255.252
    	  gateway 10.20.0.9
 
Lugner -> TurkRegion
auto eth1
iface eth1 inet static
      	address 10.20.12.1
      	netmask 255.255.252.0
 
Lugner -> GrabeForest
auto eth2
iface eth2 inet static
      	address 10.20.1.1
      	netmask 255.255.255.0
``` 
Routing :
```bash
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.20.0.9
 ```
 
**TurkRegion (CLIENT)**
<br>
Config :
```bash
TurkRegion -> Lugner
auto eth0
iface eth0 inet static
      	address 10.20.12.2
      	netmask 255.255.252.0
      	gateway 10.20.12.1
```
 
**GrabeForest (CLIENT)**
<br>
Config :
```bash
GrabeForest -> Lugner
auto eth0
iface eth0 inet static
      	address 10.20.1.2
      	netmask 255.255.255.0
      	gateway 10.20.1.1
``` 
**Linie (ROUTER)**
<br>
Config :
```bash
Linie -> Eisen
auto eth0
iface eth0 inet static
      	address 10.20.0.6
      	netmask 255.255.255.252
    	  gateway 10.20.0.5
 
Linie -> Lawine
auto eth1
iface eth1 inet static
      	address 10.20.0.1
      	netmask 255.255.255.252
 
Linie -> GranzChannel
auto eth2
iface eth2 inet static
      	address 10.20.4.1
      	netmask 255.255.254.0
``` 
Routing :
```bash
 route add -net 10.20.0.128 netmask 255.255.255.192 gw 10.20.0.2
 route add -net 10.20.8.0 netmask 255.255.252.0 gw 10.20.0.2
 ```
**GranzChannel (CLIENT)**
<br>
Config :
```bash
GranzChannel -> Linie
auto eth0
iface eth0 inet static
      	address 10.20.4.2
      	netmask 255.255.254.0
      	gateway 10.20.4.1
 ```
**Lawine (ROUTER)**
<br>
Config :
```bash
Lawine -> Linie
auto eth0
iface eth0 inet static
      	address 10.20.0.2
      	netmask 255.255.255.252
    	  gateway 10.20.0.1
 
Lawine -> BredtRegion & Heiter
auto eth1
iface eth1 inet static
      	address 10.20.0.129
      	netmask 255.255.255.192
``` 
Routing :
```bash
 route add -net 10.20.8.0 netmask 255.255.252.0 gw 10.20.0.131
 ```

**BredtRegion (CLIENT)**
<br>
Config :
```bash
BredtRegion -> Lawine
auto eth0
iface eth0 inet static
      	address 10.20.0.130
      	netmask 255.255.255.192
      	gateway 10.20.0.129
 ```
**Heiter (ROUTER)**
<br>
Config :
```bash
Heiter -> Lawine
auto eth0
iface eth0 inet static
      	address 10.20.0.131
      	netmask 255.255.255.192
    	  gateway 10.20.0.129
 
Heiter -> RiegelCanyon & Sein
auto eth1
iface eth1 inet static
      	address 10.20.8.1
      	netmask 255.255.252.0
``` 
Routing :
```bash
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.20.0.129
``` 
**RiegelCanyon (CLIENT)**
<br>
Config :
```bash
RiegelCanyon -> Heiter
auto eth0
iface eth0 inet static
      	address 10.20.8.2
      	netmask 255.255.252.0
      	gateway 10.20.8.1
 ```
**Sein (SERVER)**
<br>
Config :
```bash
Sein -> Heiter
auto eth0
iface eth0 inet static
      	address 10.20.8.3
      	netmask 255.255.252.0
      	gateway 10.20.8.1
```
**Richter (SERVER)**
<br>
Config :
```bash
Richter -> Eisen
auto eth0
iface eth0 inet static
      	address 10.20.0.50
      	netmask 255.255.255.248
      	gateway 10.20.0.49
 ```
**Revolte (SERVER)**
<br>
Config :
```bash
Revolte -> Eisen
auto eth0
iface eth0 inet static
      	address 10.20.0.51
      	netmask 255.255.255.248
      	gateway 10.20.0.49
 ```
 
**Frieren (ROUTER)**
<br>
Config :
```bash
Frieren -> Aura
auto eth0
iface eth0 inet static
      	address 10.20.0.22
      	netmask 255.255.255.252
    	  gateway 10.20.0.21
 
Frieren -> LakeKorridor
auto eth2
iface eth2 inet static
      	address 10.20.0.65
      	netmask 255.255.255.224
 
Frieren -> Flamme
auto eth1
iface eth1 inet static
      	address 10.20.0.25
      	netmask 255.255.255.252
``` 
Routing :
```bash
 route add -net 10.20.0.28 netmask 255.255.255.252 gw 10.20.0.26
 route add -net 10.20.24.0 netmask 255.255.248.0 gw 10.20.0.26
 route add -net 10.20.16.0 netmask 255.255.252.0 gw 10.20.0.26
 route add -net 10.20.0.32 netmask 255.255.255.252 gw 10.20.0.26
 route add -net 10.20.0.40 netmask 255.255.255.248 gw 10.20.0.26
 ```
**Flamme (ROUTER)**
<br>
Config :
```bash
Flamme -> Frieren
auto eth0
iface eth0 inet static
      	address 10.20.0.26
      	netmask 255.255.255.252
    	  gateway 10.20.0.25
 
Flamme -> Fern
auto eth1
iface eth1 inet static
      	address 10.20.0.29
      	netmask 255.255.255.252
 
Flamme -> RohrRoad
auto eth2
iface eth2 inet static
      	address 10.20.16.1
      	netmask 255.255.252.0
 
Flamme -> Himmel
auto eth3
iface eth3 inet static
      	address 10.20.0.33
      	netmask 255.255.255.252
 ```
Routing :
```bash
 route add -net 10.20.24.0 netmask 255.255.248.0 gw 10.20.0.30
 route add -net 10.20.0.40 netmask 255.255.255.248 gw 10.20.0.34
 ```
**Himmel (ROUTER)**
<br>
Config :
```bash
Himmel  -> SchwerMountains
auto eth1
iface eth1 inet static
      	address 10.20.0.41
      	netmask 255.255.255.248
 
Himmel  -> Flamme
auto eth0
iface eth0 inet static
      	address 10.20.0.34
      	netmask 255.255.255.252
    	  gateway 10.20.0.33
``` 
Routing :
```bash
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.20.0.33
``` 
 
**SchwerMountains (CLIENT)**
<br>
Config :
```bash
SchwerMountains -> Himmel
auto eth0
iface eth0 inet static
      	address 10.20.0.42
      	netmask 255.255.255.248
      	gateway 10.20.0.41
``` 
**RohrRoad (CLIENT)**
<br>
Config :
```bash
RohrRoad -> Flamme
auto eth0
iface eth0 inet static
      	address 10.20.16.2
      	netmask 255.255.252.0
      	gateway 10.20.16.1
```
 
**Fern (ROUTER)**
<br>
Config :
```bash
Fern -> Flamme
auto eth0
iface eth0 inet static
      	address 10.20.0.30
      	netmask 255.255.255.252
    	  gateway 10.20.0.29
 
Fern -> LaubHills & AppetitRegion
auto eth1
iface eth1 inet static
      	address 10.20.24.1
      	netmask 255.255.248.0
``` 
Routing :
```bash
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.20.0.29
``` 
 
**AppetitRegion (CLIENT)**
<br>
Config :
```bash
AppetitRegion -> Fern
auto eth0
iface eth0 inet static
      	address 10.20.24.2
      	netmask 255.255.248.0
      	gateway 10.20.24.1
``` 
**LaubHills (CLIENT)**
<br>
Config :
```bash
LaubHills -> Fern
auto eth0
iface eth0 inet static
      	address 10.20.24.3
      	netmask 255.255.248.0
      	gateway 10.20.24.1
``` 
**LakeKorridor (CLIENT)**
<br>
Config :
```bash
LakeKorridor -> Frieren
auto eth0
iface eth0 inet static
      	address 10.20.0.66
      	netmask 255.255.255.224
      	gateway 10.20.0.65
``` 
# **VLSM Testing** 
| Aura - A1                 | Aura - A12   | Aura - A4           |
| ------------------------- | ------------ | ------------------- |
|![ping A1](https://github.com/lalaladi/Jarkom-Modul-2-B23-2023/assets/90541607/1a1d18c0-5265-4f04-8ec7-ad8aa9a4b123)   | ![ping A2](https://github.com/lalaladi/Jarkom-Modul-2-B23-2023/assets/90541607/77eb6a31-f560-497c-a8f7-4a4a4b361c3b) | ![ping A4](https://github.com/lalaladi/Jarkom-Modul-2-B23-2023/assets/90541607/57fc4538-4922-4b7d-be29-71448ff0244e) |

| BredtRegion -> SchwerMountains      | LaubHills -> GranzChannel   | RoyalCapital -> TurkRegion      |
| ----------------------------------- | --------------------------- | ------------------------------- |
|![BredtRegion - SchwerMountains](https://github.com/lalaladi/praksisop2/assets/90541607/b895d83c-4de3-4968-97ff-35d97b8c49af)   | ![LaubHills - GranzChannel](https://github.com/lalaladi/praksisop2/assets/90541607/33e01c6b-40c9-4223-ad5f-f25584358873) | ![RoyalCapital - TurkRegion](https://github.com/lalaladi/praksisop2/assets/90541607/4c04242e-057f-495c-940c-c0fc149a6b38) |

| Aura - Switch 9           | Frieren - Lugner  | Aura - A17          |
| ------------------------- | ----------------- | ------------------- |
| ![ping Switch 9](https://github.com/lalaladi/Jarkom-Modul-2-B23-2023/assets/90541607/41a2f3db-2c85-487e-bcb7-e4018d977b3b)   | ![ping Frieren - Lugner](https://github.com/lalaladi/Jarkom-Modul-2-B23-2023/assets/90541607/04e62753-e229-49c8-b003-febe0ffa2069) | ![ping A17](https://github.com/lalaladi/Jarkom-Modul-2-B23-2023/assets/90541607/4635bd3d-8062-4fcf-ad93-ddd2af1de851) |

| Aura - A7             | Aura - A8  | Aura - A13          |
| --------------------- | ---------- | ------------------- |
| ![ping A7](https://github.com/lalaladi/praksisop2/assets/90541607/a7f1e876-4d35-4c50-bb14-18c4e81bbee3)  | ![ping A8](https://github.com/lalaladi/praksisop2/assets/90541607/165051fa-3b4b-4b57-b653-8fef995c35fd) | ![ping A13](https://github.com/lalaladi/praksisop2/assets/90541607/09a8870e-cc34-4c0a-8253-3d5a61bc872a) |
                                                      
| GrobeForest               | LakeKorridor | LaubHills           |
| ------------------------- | ------------ | ------------------- |
| ![Grobeforest](https://github.com/lalaladi/praksisop2/assets/90541607/d268e8c1-f8f6-4366-baca-1b410f3774cf)  | ![Lakekorridor](https://github.com/lalaladi/praksisop2/assets/90541607/7b8a2dac-20c7-479d-9d7a-d676646e74b9) | ![Laubhills](https://github.com/lalaladi/praksisop2/assets/90541607/d4e6ab04-b8ff-4bc6-8524-a2c413a191d2) |

| RiegelCanyon              | RohrRoad     | RoyalCapital        |
| ------------------------- | ------------ | ------------------- |
| ![Riegelcanyon](https://github.com/lalaladi/praksisop2/assets/90541607/ed72cd00-0577-474b-8113-7a2f6fcfc450)  | ![Rohrroad](https://github.com/lalaladi/praksisop2/assets/90541607/5c186126-7d3d-439d-8cba-ade96ce8be22) | ![Royalcapital](https://github.com/lalaladi/praksisop2/assets/90541607/636f53c7-c664-4ba1-b11b-7fcb9eeabe6f) |

# **Kendala Selama Pengerjaan**
Berikut merupakan beberapa kendala dan kesulitan yang kami alami dalam pengerjaan Praktikum Modul 4 Jaringan Komputer, yakni :
 - butuh ketelitian penuh dalam pengerjaannya

# **The End**
