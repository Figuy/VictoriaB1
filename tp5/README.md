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

