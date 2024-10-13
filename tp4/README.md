## TP4 : DHCP et accès internet

### 1. Les mains dans le capot
###  Capturez un échange DHCP complet

voir : 
[dhcp.pcapng](dhcp.pcapng)

### Directement dans Wireshark, vous pouvez voir toutes les infos que vous donne  le serveur DHCP

Adresse IP proposée :
```
Option: (50) Requested IP Address (10.33.77.170)
```

DNS :
```
Option: (6) Domain Name Server
 Domain Name Server: 8.8.8.8
    Domain Name Server: 1.1.1.1
```

Passerelle du réseau :
```
Option: (54) DHCP Server Identifier (10.33.79.254)
```



