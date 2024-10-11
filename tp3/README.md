## TP 3

## I. ARP basics

### Avant de continuer..
affichez l'adresse MAC de votre carte WiFi !



```powershell 
PS C:\Users\vicky> ipconfig /all
Carte réseau sans fil Wi-Fi :

Adresse physique . . . . . . . . . . . : 10-63-C8-68-8C-21
```

### ️ Affichez votre table ARP
allez vous commencez à devenir grands, je vous donne pas la commande, demande à m'sieur internet !



```powershell
PS C:\Users\vicky> arp -a

Interface : 10.33.77.170 --- 0xa
  Adresse Internet      Adresse physique      Type
  10.33.65.63           44-af-28-c3-6a-9f     dynamique
  10.33.66.78           e4-b3-18-48-36-68     dynamique
  10.33.73.77           98-8d-46-c4-fa-e5     dynamique
  10.33.77.160          c8-94-02-f8-ab-97     dynamique
  10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique
  ```

### Déterminez l'adresse MAC de la passerelle du réseau de l'école
La passerelle, vous connaissez son adresse IP normalement 
```powershell 
PS C:\Users\vicky> ipconfig
  Passerelle par défaut. . . . . . . . . : 10.33.79.254
```

Si vous avez un accès internet, votre PC a forcément l'adresse MAC de la passerelle dans sa table ARP

```powershell
PS C:\Users\vicky> arp -a
10.33.79.254          7c-5a-1c-d3-d8-76     dynamique
```

### Supprimez la ligne qui concerne la passerelle

une commande pour supprimer l'adresse MAC de votre table ARP

```powershell
PS C:\WINDOWS\system32>  arp -d 10.33.79.254

```
###  Prouvez que vous avez supprimé la ligne dans la table ARP

```powershell
PS C:\WINDOWS\system32> arp  -a

Interface : 10.33.77.170 --- 0xa
  Adresse Internet      Adresse physique      Type
  10.33.65.63           44-af-28-c3-6a-9f     dynamique
  10.33.66.78           e4-b3-18-48-36-68     dynamique
  10.33.73.77           98-8d-46-c4-fa-e5     dynamique
  10.33.77.160          c8-94-02-f8-ab-97     dynamique
  10.33.79.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique
```
### Wireshark

[arp1.pcap](arp1.pcap)


## II. ARP dans un réseau local

### 1. Basics

### Déterminer
son adresse IP au sein du réseau local formé par le partage de co
```powershell
PS C:\WINDOWS\system32> ipconfig /all
Adresse IPv4. . . . . . . . . . . . . .: 172.20.10.2(préféré)
```

son adresse MAC
```powershell
PS C:\WINDOWS\system32> arp  -a

Interface : 172.20.10.2 --- 0xa
  Adresse Internet      Adresse physique      Type
  172.20.10.1           0e-db-ea-5b-13-64     dynamique
``` 

### DIY

```powershell
PS C:\Users\vicky> ipconfig
Adresse IPv4. . . . . . . . . . . . . .: 172.20.10.8
```

### Pingz !

vérifiez que vous pouvez tous vous ping avec ces adresses IP

Teddy
```powershell
PS C:\Users\vicky> ping 172.20.10.10

Envoi d’une requête 'Ping'  172.20.10.10 avec 32 octets de données :
Réponse de 172.20.10.10 : octets=32 temps=8 ms TTL=128
Réponse de 172.20.10.10 : octets=32 temps=27 ms TTL=128
Réponse de 172.20.10.10 : octets=32 temps=10 ms TTL=128
Réponse de 172.20.10.10 : octets=32 temps=9 ms TTL=128

Statistiques Ping pour 172.20.10.10:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 8ms, Maximum = 27ms, Moyenne = 13ms
```
Hugo
```powershell
PS C:\Users\vicky> ping 172.20.10.11

Envoi d’une requête 'Ping'  172.20.10.11 avec 32 octets de données :
Réponse de 172.20.10.11 : octets=32 temps=17 ms TTL=128
Réponse de 172.20.10.11 : octets=32 temps=22 ms TTL=128
Réponse de 172.20.10.11 : octets=32 temps=38 ms TTL=128
Réponse de 172.20.10.11 : octets=32 temps=29 ms TTL=128

Statistiques Ping pour 172.20.10.11:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 17ms, Maximum = 38ms, Moyenne = 26ms
```
Ulysse
```powershell
PS C:\Users\vicky> ping 172.20.10.9

Envoi d’une requête 'Ping'  172.20.10.9 avec 32 octets de données :
Réponse de 172.20.10.9 : octets=32 temps=21 ms TTL=128
Réponse de 172.20.10.9 : octets=32 temps=21 ms TTL=128
Réponse de 172.20.10.9 : octets=32 temps=126 ms TTL=128
Réponse de 172.20.10.9 : octets=32 temps=130 ms TTL=128

Statistiques Ping pour 172.20.10.9:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 21ms, Maximum = 130ms, Moyenne = 74ms
```
Vérifiez avec une commande ping que vous avez bien un accès internet :

```powershell
PS C:\Users\vicky> ping instagram.com

Envoi d’une requête 'ping' sur instagram.com [2a03:2880:f27b:1e4:face:b00c:0:4420] avec 32 octets de données :
Réponse de 2a03:2880:f27b:1e4:face:b00c:0:4420 : temps=44 ms
Réponse de 2a03:2880:f27b:1e4:face:b00c:0:4420 : temps=57 ms
Réponse de 2a03:2880:f27b:1e4:face:b00c:0:4420 : temps=49 ms
Réponse de 2a03:2880:f27b:1e4:face:b00c:0:4420 : temps=41 ms

Statistiques Ping pour 2a03:2880:f27b:1e4:face:b00c:0:4420:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 41ms, Maximum = 57ms, Moyenne = 47ms
```
## 2. ARP

### Affichez votre table ARP !

```powershell

PS C:\Users\vicky> arp -a

Interface : 172.20.10.8 --- 0xa
  Adresse Internet      Adresse physique      Type
  172.20.10.9           90-09-df-a4-67-21     dynamique
  172.20.10.10          d4-3b-04-ff-44-7a     dynamique
  172.20.10.11          50-5a-65-50-4a-c9     dynamique
  172.20.255.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
```

### Capture arp2.pcap

[arp2.pcapng](arp2.pcapng)




