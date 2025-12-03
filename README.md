# Jarkom-Modul-5-2025-K09

Anggota Kelompok :
1. Angga Firmansyah - 5027241062

# Topologi

<img width="1497" height="813" alt="topologi" src="https://github.com/user-attachments/assets/ad50b97e-c2d6-4174-96b6-55601aee8d3e" />


# Pembagian Subnet

<img width="1497" height="813" alt="Subnet" src="https://github.com/user-attachments/assets/3abdff59-17fd-477b-877f-113732c364b2" />

# Tabel Rute 

<img width="1130" height="366" alt="image" src="https://github.com/user-attachments/assets/4d4b52ee-11da-4104-b301-0a11993b0320" />

# Misi 1
Konfig Semua Node
```
Osgiliath

# 1. Interface Internet (NAT)
auto eth0
iface eth0 inet dhcp
    # Nyalakan IP Forwarding otomatis saat interface ini nyala
    post-up sysctl -w net.ipv4.ip_forward=1
    
    # Pasang NAT (SNAT) otomatis
    
    post-up iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source 192.168.122.216

# 2. Interface ke MORIA (Kiri)
auto eth1
iface eth1 inet static
    address 10.68.1.233
    netmask 255.255.255.252

    # RUTE KE KIRI (Via Moria .234)
    # Subnet Durin & Khamul
    up ip route add 10.68.1.128/26 via 10.68.1.234
    # Subnet IronHills (Web Server)
    up ip route add 10.68.1.248/30 via 10.68.1.234
    # Link Moria-Wilderland (Opsional, agar traceroute lengkap)
    up ip route add 10.68.1.244/30 via 10.68.1.234

# 3. Interface ke RIVENDELL (Bawah)
auto eth2
iface eth2 inet static
    address 10.68.1.237
    netmask 255.255.255.252

    # RUTE KE BAWAH (Via Rivendell .238)
    # Subnet Vilya & Narya
    up ip route add 10.68.1.224/29 via 10.68.1.238

# 4. Interface ke MINASTIR (Kanan)
auto eth3
iface eth3 inet static
    address 10.68.1.241
    netmask 255.255.255.252

    # RUTE KE KANAN (Via Minastir .242)
    # Subnet Elendil
    up ip route add 10.68.0.0/24 via 10.68.1.242
    # Subnet Isildur & Palantir (Di belakang Pelargir)
    up ip route add 10.68.1.192/27 via 10.68.1.242
    # Subnet Gilgalad & Cirdan (Di belakang Anduin)
    up ip route add 10.68.1.0/25 via 10.68.1.242
    # Link Minastir-Pelargir
    up ip route add 10.68.1.252/30 via 10.68.1.242
    # Link Pelargir-Anduin
    up ip route add 10.68.2.0/30 via 10.68.1.242




Moria

# /etc/network/interfaces
auto eth0
iface eth0 inet static
	address 10.68.1.234
	netmask 255.255.255.252
	gateway 10.68.1.233

# Interface ke IronHills (Gateway Baru)
auto eth1
iface eth1 inet static
	address 10.68.1.249
	netmask 255.255.255.252

# Ke Wilderland (Pakai eth1 atau eth2 sesuai kabelmu)
# Asumsi eth2 sesuai request terakhir
auto eth2
iface eth2 inet static
	address 10.68.1.245
	netmask 255.255.255.252

# Routing
up ip route add 10.68.1.128/26 via 10.68.1.246  # Ke Durin, Khamul, IronHills

IronHills

auto eth0
iface eth0 inet static
	address 10.68.1.250
	netmask 255.255.255.252
	gateway 10.68.1.249

Wilderland

# /etc/network/interfaces
auto eth0
iface eth0 inet static
	address 10.68.1.246
	netmask 255.255.255.252
	gateway 10.68.1.245

# Gateway Subnet 10.68.1.128/26 (Durin + Khamul + IronHills)
auto eth1
iface eth1 inet static
	address 10.68.1.129
	netmask 255.255.255.192

Durin

auto eth0
iface eth0 inet static
	address 10.68.1.130
	netmask 255.255.255.192
	gateway 10.68.1.129

Khamul

auto eth0
iface eth0 inet static
	address 10.68.1.181
	netmask 255.255.255.192
	gateway 10.68.1.129

Rivendell

# /etc/network/interfaces
auto eth0
iface eth0 inet static
	address 10.68.1.238
	netmask 255.255.255.252
	gateway 10.68.1.237

# Gateway Server Area (Vilya & Narya)
auto eth1
iface eth1 inet static
	address 10.68.1.225
	netmask 255.255.255.248


Vilya

auto eth0
iface eth0 inet static
	address 10.68.1.226
	netmask 255.255.255.248
	gateway 10.68.1.225

Narya

auto eth0
iface eth0 inet static
	address 10.68.1.227
	netmask 255.255.255.248
	gateway 10.68.1.225

Minastir

# /etc/network/interfaces
auto eth0
iface eth0 inet static
	address 10.68.1.242
	netmask 255.255.255.252
	gateway 10.68.1.241

# Gateway Elendil
auto eth1
iface eth1 inet static
	address 10.68.0.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.68.1.253
	netmask 255.255.255.252

# Routing [FIX: Gateway Pelargir berubah jadi .254]
up ip route add 10.68.1.192/27 via 10.68.1.254  # Ke Isildur
up ip route add 10.68.1.0/25 via 10.68.1.254    # Ke Gilgalad
# Rute ke Anduin (Link baru)
up ip route add 10.68.2.0/30 via 10.68.1.254


Isildur

auto eth0
iface eth0 inet static
	address 10.68.1.194
	netmask 255.255.255.224
	gateway 10.68.1.193

Elendil

auto eth0
iface eth0 inet static
	address 10.68.0.2
	netmask 255.255.255.0
	gateway 10.68.0.1

Pelargir

# /etc/network/interfaces
auto eth0
iface eth0 inet static
	address 10.68.1.254
	netmask 255.255.255.252
	gateway 10.68.1.253

# Gateway Isildur + Palantir
auto eth1
iface eth1 inet static
	address 10.68.1.193
	netmask 255.255.255.224

auto eth2
iface eth2 inet static
	address 10.68.2.1
	netmask 255.255.255.252

# Routing [FIX: Gateway Anduin jadi 2.2]
up ip route add 10.68.1.0/25 via 10.68.2.2      # Ke Gilgalad


Palantir

auto eth0
iface eth0 inet static
	address 10.68.1.222
	netmask 255.255.255.224
	gateway 10.68.1.193

AnduinBanks

# /etc/network/interfaces
auto eth0
iface eth0 inet static
	address 10.68.2.2
	netmask 255.255.255.252
	gateway 10.68.2.1

# Gateway Gilgalad & Cirdan
auto eth1
iface eth1 inet static
	address 10.68.1.1
	netmask 255.255.255.128


Gigalad

auto eth0
iface eth0 inet static
	address 10.68.1.2
	netmask 255.255.255.128
	gateway 10.68.1.1


Cirdan

auto eth0
iface eth0 inet static
	address 10.68.1.3
	netmask 255.255.255.128
	gateway 10.68.1.1

```
Pemmbuatan server DNS dan DHCP 

<img width="637" height="194" alt="image" src="https://github.com/user-attachments/assets/2cf91cdb-1566-4fde-810f-5530597e1b7d" />

Sukses Ping : 

<img width="679" height="116" alt="image" src="https://github.com/user-attachments/assets/47f8f233-c989-485a-b995-785dc9b977af" />

# Misi 2

Blokir ping untuk semua node ke vilya, contoh node elendil :

<img width="661" height="145" alt="image" src="https://github.com/user-attachments/assets/67a6446b-d5b9-4b99-96fe-018ee62cde11" />

Namun vilya dapat ping ke google : 

<img width="726" height="263" alt="image" src="https://github.com/user-attachments/assets/e319666a-3c65-4e93-aa74-2cd81404416b" />

Sukses NC dan ping ironhills.com : 

<img width="634" height="128" alt="image" src="https://github.com/user-attachments/assets/ac70b9ff-bb63-4ec0-b250-cd4986e52d11" />

Hanya bisa node vilya yang connect : 

<img width="627" height="193" alt="image" src="https://github.com/user-attachments/assets/a6f88960-1b14-48af-b80c-5676acd5f82b" />

<img width="548" height="98" alt="image" src="https://github.com/user-attachments/assets/beba16d9-10b9-4fef-8053-0aebd1ae13e3" />

Bukti gagal bekerja : 

<img width="729" height="74" alt="image" src="https://github.com/user-attachments/assets/fd0e6946-48ee-4c41-88e7-be0f25c2f17e" />

<img width="741" height="81" alt="image" src="https://github.com/user-attachments/assets/1c7bddeb-d5ed-4c5d-80c6-a34e9240f0a4" />


# Misi 3

