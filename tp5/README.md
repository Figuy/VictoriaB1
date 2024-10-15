## TP5 : Un ptit LAN à nous

### I. Setup

️ Uniquement avec des commandes, prouvez-que :

- vous avez bien configuré les adresses IP demandées (un ip a suffit hein)

``` powershell
️[vicky@victoriarouter ~]$ ip a
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:13:32:54 brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.254/24 brd 10.5.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe13:3254/64 scope link
       valid_lft forever preferred_lft forever
```

``` powershell
victoria@victoriaclt1:~$ ip a
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:84:a7:9e brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.11/24 brd 10.5.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe84:a79e/64 scope link
       valid_lft forever preferred_lft forever
```

``` powershell
victoria@victoriaclt2:~$ ip a
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:1c:33:54 brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.12/24 brd 10.5.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe1c:3354/64 scope link
       valid_lft forever preferred_lft forever
```
️
️- vous avez bien configuré les hostnames demandés :

routeur :
```
[vicky@victoriarouter ~]$
```

client 1 :

```
victoria@victoriaclt1:~$
```

client 2 :
```
victoria@victoriaclt2:~$
```

️ping :

```powershell
[vicky@victoriarouter ~]$ ping 10.5.1.12
PING 10.5.1.12 (10.5.1.12) 56(84) bytes of data.
64 bytes from 10.5.1.12: icmp_seq=1 ttl=64 time=3.18 ms
64 bytes from 10.5.1.12: icmp_seq=2 ttl=64 time=2.77 ms
```


```powershell
[vicky@victoriarouter ~]$ ping 10.5.1.11
PING 10.5.1.11 (10.5.1.11) 56(84) bytes of data.
64 bytes from 10.5.1.11: icmp_seq=1 ttl=64 time=9.80 ms
64 bytes from 10.5.1.11: icmp_seq=2 ttl=64 time=2.40 ms
```

## II. Accès internet pour tous

### Déjà, prouvez que le routeur a un accès internet

``` powershell
[vicky@victoriarouter ~]$ ping www.google.com
PING www.google.com (172.217.20.196) 56(84) bytes of data.
64 bytes from waw02s08-in-f4.1e100.net (172.217.20.196): icmp_seq=1 ttl=116 time=25.1 ms
64 bytes from waw02s08-in-f4.1e100.net (172.217.20.196): icmp_seq=2 ttl=116 time=20.3 ms
```

### ️ Activez le routage
```powershell
[vicky@victoriarouter ~]$ sudo firewall-cmd --add-masquerade --permanent
[sudo] password for vicky:
success
[vicky@victoriarouter ~]$ sudo firewall-cmd --reload
success
```

### 2. Accès internet clients
### Prouvez que les clients ont un accès interne

client 2:
```powershell
victoria@victoriaclt2:~$ ping www.youtube.com
PING youtube-ui.l.google.com (172.217.18.238) 56(84) bytes of data.
64 bytes from par10s10-in-f238.1e100.net (172.217.18.238): icmp_seq=1 ttl=113 time=23.1 ms

--- youtube-ui.l.google.com ping statistics ---^C
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 23.117/26.113/30.977/3.470 ms
```

client 1:
```powershell
victoria@victoriaclt1:~$ ping www.youtube.com
PING youtube-ui.l.google.com (142.250.201.46) 56(84) bytes of data.
64 bytes from mrs08s20-in-f14.1e100.net (142.250.201.46): icmp_seq=1 ttl=113 time=21.9 ms
64 bytes from mrs08s20-in-f14.1e100.net (142.250.201.46): icmp_seq=2 ttl=113 time=23.7 ms
^C
--- youtube-ui.l.google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 21.886/22.789/23.693/0.903 ms
```


### Montrez-moi le contenu final du fichier de configuration de l'interface réseau


```powershell
victoria@victoriaclt2:~$ cat  /etc/netplan/01-netcfg.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses: [10.5.1.12/24]
      gateway4: 10.5.1.254
      nameservers:
        addresses: [1.1.1.1]
```

## III. Serveur SSH

### Sur routeur.tp5.b1, déterminer sur quel port écoute le serveur SSH

``` powershell
[vicky@victoriarouter ~]$ sudo ss -lnpt | grep 22
LISTEN 0      128          0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=686,fd=3))
LISTEN 0      128             [::]:22           [::]:*    users:(("sshd",pid=686,fd=4))
```

### Sur routeur.tp5.b1, vérifier que ce port est bien ouvert

``` powershell
[vicky@victoriarouter ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: ssh
  ports: 22/tcp
  protocols:
  forward: yes
  masquerade: yes
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

## IV. Serveur DHCP

### A. Installation et configuration du serveur DHCP

️Installez et configurez un serveur DHCP sur la machine routeur.tp5.b1

```powershell
 sudo dnf -y install dhcp-server

 sudo nano /etc/dhcp/dhcpd.conf

 systemctl enable --now dhcpd
```


``` powershell
[vicky@victoriarouter ~]$ sudo cat /etc/dhcp/dhcpd.conf

# specify DNS server's hostname or IP address
option domain-name-servers     1.1.1.1;
# this DHCP server to be declared valid
authoritative;
# specify network address and subnetmask
subnet 10.5.1.0 netmask 255.255.255.0 {
    # specify the range of lease IP address
    range dynamic-bootp 10.5.1.137 10.5.1.237;
    # specify broadcast address
    option broadcast-address 10.5.1.255;
    # specify gateway
    option routers 10.5.1.254;
}

```

### Test avec un nouveau client

### Créez une nouvelle machine client client3.tp5.b1

définissez son hostname
``` powershell
victoria@victoriaclt3:~$ sudo hostnamectl
 Static hostname: victoriaclt3
```

définissez une IP en DHCP
``` powershell
victoria@victoriaclt3:~$ ip a
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:99:06:8e brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.137/24 metric 100 brd 10.5.1.255 scope global dynamic enp0s3
       valid_lft 42990sec preferred_lft 42990sec
    inet6 fe80::a00:27ff:fe99:68e/64 scope link
       valid_lft forever preferred_lft forever
```
Prouvez qu'il a immédiatement un accès internet

```powershell
victoria@victoriaclt3:~$ ping 1.1.1.1
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=54 time=24.0 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=54 time=24.5 ms
^C
--- 1.1.1.1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 23.962/24.228/24.494/0.266 ms
```
### C. Consulter le bail DHCP

```powershell
[vicky@victoriarouter dhcpd]$ cat dhcpd.leases
# The format of this file is documented in the dhcpd.leases(5) manual page.
# This lease file was written by isc-dhcp-4.4.2b1

# authoring-byte-order entry is generated, DO NOT DELETE
authoring-byte-order little-endian;

server-duid "\000\001\000\001.\240Mf\010\000'\017\262\200";

lease 10.5.1.137 {
  starts 1 2024/10/14 21:45:26;
  ends 2 2024/10/15 09:45:26;
  cltt 1 2024/10/14 21:45:26;
  binding state active;
  next binding state free;
  rewind binding state free;
  hardware ethernet 08:00:27:99:06:8e;
  client-hostname "victoriaclt1";
}
lease 10.5.1.137 {
  starts 1 2024/10/14 21:51:54;
  ends 2 2024/10/15 09:51:54;
  cltt 1 2024/10/14 21:51:54;
  binding state active;
  next binding state free;
  rewind binding state free;
  hardware ethernet 08:00:27:99:06:8e;
  uid "\377\3424?>\000\002\000\000\253\021\006k\304*\257 \246\331";
  client-hostname "victoriaclt1";
}
```
### ️ Confirmez qu'il s'agit bien de la bonne adresse MAC

```powershell
victoria@victoriaclt3:~$ ip a
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:99:06:8e brd ff:ff:ff:ff:ff:ff
    inet 10.5.1.137/24 metric 100 brd 10.5.1.255 scope global dynamic enp0s3
       valid_lft 42495sec preferred_lft 42495sec
    inet6 fe80::a00:27ff:fe99:68e/64 scope link
       valid_lft forever preferred_lft forever
```
