## TP6 : Des bo services dans des bo LANs

### 2. Marche à suivre

#### ️ Prouvez que...

Lan 1 :

```powershell
[vicky@dhcp ~]$ ping www.youtube.com
PING youtube-ui.l.google.com (172.217.18.46) 56(84) bytes of data.
64 bytes from ham02s12-in-f14.1e100.net (172.217.18.46): icmp_seq=1 ttl=112 time=26.4 ms
64 bytes from ham02s12-in-f14.1e100.net (172.217.18.46): icmp_seq=2 ttl=112 time=23.5 ms
^C
--- youtube-ui.l.google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1005ms
rtt min/avg/max/mdev = 23.460/24.906/26.353/1.446 ms
```

Lan 2 :
```powershell
[vicky@dns ~]$ ping www.youtube.com
PING youtube-ui.l.google.com (142.250.200.206) 56(84) bytes of data.
64 bytes from mrs08s17-in-f14.1e100.net (142.250.200.206): icmp_seq=1 ttl=112 time=22.4 ms
^C
--- youtube-ui.l.google.com ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 22.398/22.398/22.398/0.000 ms
```

lan1 vers lan2

```powershell
[vicky@dns ~]$ ping 10.6.1.253
PING 10.6.1.253 (10.6.1.253) 56(84) bytes of data.
64 bytes from 10.6.1.253: icmp_seq=1 ttl=63 time=3.22 ms
64 bytes from 10.6.1.253: icmp_seq=2 ttl=63 time=3.89 ms
^C
--- 10.6.1.253 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1003ms
rtt min/avg/max/mdev = 3.224/3.555/3.887/0.331 ms
```

## 2. Client
### Prouvez que...

- Le client a bien récupéré une adresse IP en DHCP
```powershell
victoria@client1:~$ ip a
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:49:b8:73 brd ff:ff:ff:ff:ff:ff
    inet 10.6.1.37/24 brd 10.6.1.255 scope global dynamic noprefixroute enp0s3
       valid_lft 42779sec preferred_lft 42779sec
    inet6 fe80::a00:27ff:fe49:b873/64 scope link
       valid_lft forever preferred_lft forever
```

- Vous avez bien 1.1.1.1 en DNS
```powershell
victoria@client1:~$ sudo resolvectl
[sudo] password for victoria:
Global
         Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
  resolv.conf mode: stub

Link 2 (enp0s3)
    Current Scopes: DNS
         Protocols: +DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 1.1.1.1
       DNS Servers: 1.1.1.1
```

- vous avez bien la bonne passerelle indiquée

```powershell
victoria@client1:~$ ip r s
default via 10.6.1.254 dev enp0s3 proto dhcp src 10.6.1.37 metric 100
10.6.1.0/24 dev enp0s3 proto kernel scope link src 10.6.1.37 metric 100
```
- que ça ping un nom de domaine public sans problème magueule

```powershell
victoria@client1:~$ ping www.google.com
PING www.google.com (142.251.37.164) 56(84) bytes of data.
64 bytes from mrs09s14-in-f4.1e100.net (142.251.37.164): icmp_seq=1 ttl=111 time=27.0 ms
64 bytes from mrs09s14-in-f4.1e100.net (142.251.37.164): icmp_seq=2 ttl=111 time=32.2 ms

--- www.google.com ping statistics ---^C
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 21.821/29.025/35.033/5.059 ms
```

# III. LAN serveurzzzz

## 1. Serveur Web

### 3. Analyse et test

Déterminer sur quel port écoute le serveur NGINX

```powershell
[vicky@web ~]$ sudo ss -lnpt | grep 80
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*    users:(("nginx",pid=1422,fd=6),("nginx",pid=1421,fd=6))
LISTEN 0      511             [::]:80           [::]:*    users:(("nginx",pid=1422,fd=7),("nginx",pid=1421,fd=7))
```

### Ouvrir ce port dans le firewall

```powershell
[vicky@web ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3
  sources:
  services: ssh
  ports: 80/tcp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

### Visitez le site web !

```powershell
victoria@client1:~$ curl http://10.6.2.11
<!doctype html>
<html>
  <head>
    <meta charset='utf-8'>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <title>HTTP Server Test Page powered by: Rocky Linux</title>
    <style type="text/css">
      /*<![CDATA[*/
```

# 2. Serveur DNS

## 4. Analyse du service
### Déterminer sur quel(s) port(s) écoute le service BIND9

```powershell 

[vicky@dns ~]$ ss -lnpt | grep 53
LISTEN 0      10         127.0.0.1:53        0.0.0.0:*
LISTEN 0      4096       127.0.0.1:953       0.0.0.0:*
LISTEN 0      10         10.6.2.12:53        0.0.0.0:*
LISTEN 0      4096           [::1]:953          [::]:*
LISTEN 0      10             [::1]:53           [::]:*
```

### Ouvrir ce(s) port(s) dans le firewall

```powershell
[vicky@dns ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3
  sources:
  services: cockpit dhcpv6-client ssh
  ports: 53/tcp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

## 5. Tests manuels

###  Effectuez des requêtes DNS manuellement depuis le serveur DNS lui-même dans un premier temps

```powershell
[vicky@dns ~]$ dig web.tp6.b1 @10.6.2.12
```
```powershell
; <<>> DiG 9.16.23-RH <<>> web.tp6.b1 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 29865
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: beafca7e3e5ed9ec01000000670fd799dd454e744fdb91b3 (good)
;; QUESTION SECTION:
;web.tp6.b1.                    IN      A

;; ANSWER SECTION:
web.tp6.b1.             86400   IN      A       10.6.2.11

;; Query time: 10 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Wed Oct 16 17:11:22 CEST 2024
;; MSG SIZE  rcvd: 83
```
```powershell
[vicky@dns ~]$ dig dns.tp6.b1 @10.6.2.12
```
```powershell
; <<>> DiG 9.16.23-RH <<>> dns.tp6.b1 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 9885
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 74ff152824032b5d01000000670fd80bc7a2ca7adeee5b7e (good)
;; QUESTION SECTION:
;dns.tp6.b1.                    IN      A

;; ANSWER SECTION:
dns.tp6.b1.             86400   IN      A       10.6.2.12

;; Query time: 9 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Wed Oct 16 17:13:15 CEST 2024
;; MSG SIZE  rcvd: 83
```

```powershell
[vicky@dns ~]$ dig ynov.com @10.6.2.12
```

```powershell
; <<>> DiG 9.16.23-RH <<>> ynov.com @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 58995
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 76b83dbb6830721d01000000670fd854c6532c06d35bfe28 (good)
;; QUESTION SECTION:
;ynov.com.                      IN      A

;; ANSWER SECTION:
ynov.com.               300     IN      A       104.26.10.233
ynov.com.               300     IN      A       104.26.11.233
ynov.com.               300     IN      A       172.67.74.226

;; Query time: 1044 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Wed Oct 16 17:14:28 CEST 2024
;; MSG SIZE  rcvd: 113
```

```powershell
[vicky@dns ~]$ dig -x 10.6.2.11 @10.6.2.12
```

```powershell
; <<>> DiG 9.16.23-RH <<>> -x 10.6.2.11 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 22309
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 31a8e2069bf9582901000000670fd8bd63ca7f7a3f71fc61 (good)
;; QUESTION SECTION:
;11.2.6.10.in-addr.arpa.                IN      PTR

;; ANSWER SECTION:
11.2.6.10.in-addr.arpa. 86400   IN      PTR     web.tp6.b1.

;; Query time: 6 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Wed Oct 16 17:16:13 CEST 2024
;; MSG SIZE  rcvd: 103
```

```powershell
[vicky@dns ~]$ dig -x 10.6.2.12 @10.6.2.12
```

```powershell
; <<>> DiG 9.16.23-RH <<>> -x 10.6.2.12 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 36666
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 4d52d27ae67044b901000000670fd912cf0204fde8ade492 (good)
;; QUESTION SECTION:
;12.2.6.10.in-addr.arpa.                IN      PTR

;; ANSWER SECTION:
12.2.6.10.in-addr.arpa. 86400   IN      PTR     dns.tp6.b1.

;; Query time: 0 msec
;; SERVER: 10.6.2.12#53(10.6.2.12)
;; WHEN: Wed Oct 16 17:17:38 CEST 2024
;; MSG SIZE  rcvd: 103
```

### Effectuez une requête DNS manuellement depuis client1.tp6.b1

```powershell
victoria@client1:~$ dig web.tp6.b1 @10.6.2.12
```

```powershell
; <<>> DiG 9.18.28-0ubuntu0.24.04.1-Ubuntu <<>> web.tp6.b1 @10.6.2.12
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 54343
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: bfb3eaa90497e9c401000000671602d7adbbf994ebe3e1f2 (good)
;; QUESTION SECTION:
;web.tp6.b1.                    IN      A

;; ANSWER SECTION:
web.tp6.b1.             86400   IN      A       10.6.2.11

;; Query time: 5 msec
;; SERVER: 10.6.2.12#53(10.6.2.12) (UDP)
;; WHEN: Mon Oct 21 09:29:27 CEST 2024
;; MSG SIZE  rcvd: 83
```

##Capturez une requête DNS et la réponse de votre serveur

[tcpdump.pcap](tcpdump.pcap)

## 3. Serveur DHCP

Créez un nouveau client client2.tp6.b1 vitefé

```powershell
victoria@clt2:~$ ip a
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:ef:fd:c3 brd ff:ff:ff:ff:ff:ff
    inet 10.6.1.38/24 metric 100 brd 10.6.1.255 scope global dynamic enp0s3
       valid_lft 42739sec preferred_lft 42739sec
    inet6 fe80::a00:27ff:feef:fdc3/64 scope link
       valid_lft forever preferred_lft forever
```

```powershell
victoria@clt2:~$ resolvectl
Global
         Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
  resolv.conf mode: stub

Link 2 (enp0s3)
    Current Scopes: DNS
         Protocols: +DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 10.6.2.12
       DNS Servers: 10.6.2.12
```

