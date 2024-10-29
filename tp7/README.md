# Serveur Web

## 1. HTTP

### B. Configuration

Lister les ports en écoute sur la machine

```powershell
[vicky@web conf.d]$ ss -lnpt | grep 80
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*
LISTEN 0      511             [::]:80           [::]:*
```

 Ouvrir le port dans le firewall de la machine

 ```powershell
[vicky@web conf.d]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3
  sources:
  services: cockpit dhcpv6-client ssh
  ports: 80/tcp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

### C. Tests client

 Vérifier que ça a pris effet.

 Faites un ping vers sitedefou.tp7.b1

```powershell
victoria@client1:~$ ping sitedefou.tp7.b1
PING sitedefou.tp7.b1 (10.7.1.11) 56(84) bytes of data.
64 bytes from sitedefou.tp7.b1 (10.7.1.11): icmp_seq=1 ttl=64 time=0.890 ms
64 bytes from sitedefou.tp7.b1 (10.7.1.11): icmp_seq=2 ttl=64 time=1.72 ms
64 bytes from sitedefou.tp7.b1 (10.7.1.11): icmp_seq=3 ttl=64 time=1.24 ms
^C
--- sitedefou.tp7.b1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 0.890/1.285/1.721/0.340 ms
```
visitez http://sitedefou.tp7.b1
```powershell
victoria@client1:~$ curl http://sitedefou.tp7.b1
meow!
```

### D. Analyze

- Capture tcp_http.pcap

voir [tcp_http.pcap](tcp_http.pcap)

- Voir la connexion établie

```powershell
victoria@client1:~$ ss -npt | grep "10.7.1.11:80"
ESTAB 0      0               10.7.1.101:50852         10.7.1.11:80    users:(("firefox",pid=5528,fd=123))
```

## 2. On rajoute un S

### A. Config

 Lister les ports en écoute sur la machine

```powershell
[vicky@web ~]$ ss -lnpt | grep 443
LISTEN 0      511        10.7.1.11:443       0.0.0.0:*
```
Gérer le firewall

```powershell
[vicky@web ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3
  sources:
  services: cockpit dhcpv6-client ssh
  ports: 443/tcp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
  ```
### B. Test test test analyyyze

```powershell
victoria@client1:~$ curl https://sitedefou.tp7.b1 -k
meow!
```
###  Capture tcp_https.pcap

voir [tcp_https.pcap](tcp_https.pcap)


## III. Serveur VPN

### 1. Install et conf Wireguard

####  Prouvez que vous avez bien une nouvelle carte réseau wg0

```powershell
[vicky@vpn ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:a6:9b:1f brd ff:ff:ff:ff:ff:ff
    inet 10.7.1.111/24 brd 10.7.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fea6:9b1f/64 scope link
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:d0:91:a5 brd ff:ff:ff:ff:ff:ff
    inet 10.7.2.111/24 brd 10.7.2.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fed0:91a5/64 scope link
       valid_lft forever preferred_lft forever
5: wg0: <POINTOPOINT,NOARP,UP,LOWER_UP> mtu 1420 qdisc noqueue state UNKNOWN group default qlen 1000
    link/none
    inet 10.7.200.1/24 scope global wg0
       valid_lft forever preferred_lft forever
```
#### Déterminer sur quel port écoute Wireguard

```powershell
[vicky@vpn ~]$ sudo ss -lnpu | grep 51820
UNCONN 0      0            0.0.0.0:51820      0.0.0.0:*
UNCONN 0      0               [::]:51820         [::]:*
```

#### Ouvrez ce port dans le firewall

```powershell
[vicky@vpn ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8 wg0
  sources:
  services: cockpit dhcpv6-client ssh
  ports: 51820/udp
  protocols:
  forward: yes
  masquerade: yes
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
  ```
### 3. Proofs

#### Ping ping ping !

Depuis le client, faites un ping vers l'IP du serveur VPN au sein du réseau virtuel

```powershell
victoria@client1:~$ ping 10.7.200.1
PING 10.7.200.1 (10.7.200.1) 56(84) bytes of data.
64 bytes from 10.7.200.1: icmp_seq=1 ttl=64 time=7.67 ms
64 bytes from 10.7.200.1: icmp_seq=2 ttl=64 time=3.85 ms
64 bytes from 10.7.200.1: icmp_seq=3 ttl=64 time=4.30 ms
^C
--- 10.7.200.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 3.854/5.276/7.671/1.703 ms
```

#### Capture ping1_vpn.pcap

voir [ping1_vpn.pcap](ping1_vpn.pcap)

#### Capture ping2_vpn.pcap

voir [ping2_vpn.pcap](ping2_vpn.pcap)

#### Prouvez que vous avez toujours un accès internet

```powershell
victoria@client1:~$ traceroute 1.1.1.1
traceroute to 1.1.1.1 (1.1.1.1), 30 hops max, 60 byte packets
 1  _gateway (10.7.200.1)  5.702 ms  8.675 ms  11.889 ms
 2  10.7.1.254 (10.7.1.254)  15.568 ms  23.948 ms  23.969 ms
 3  10.0.3.2 (10.0.3.2)  32.600 ms  32.655 ms  32.652 ms
 4  10.0.3.2 (10.0.3.2)  53.946 ms  53.904 ms  46.640 ms
 ```

### 4. Private service

####  Visitez le service Web à travers le VPN

```powershell
victoria@client1:~$ curl https://sitedefou.tp7.b1 -k
meow!
```


```powershell
victoria@client1:~$ traceroute 10.7.200.37
traceroute to 10.7.200.37 (10.7.200.37), 30 hops max, 60 byte packets
 1  _gateway (10.7.200.1)  4.230 ms  4.370 ms  10.313 ms
 2  sitedefou.tp7.b1 (10.7.200.37)  18.898 ms !X  34.170 ms !X  34.128 ms !X
```
