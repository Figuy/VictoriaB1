<h1>  Adresses IP de ta machine </h1>

 ``` powershell
 C:\Users\vicky> ipconfig

Carte réseau sans fil Wi-Fi :

 Suffixe DNS propre à la connexion. . . :
  Adresse IPv6 de liaison locale. . . . .: fe80::c127:166:3c80:8571%10
  Adresse IPv4. . . . . . . . . . . . . .: 10.33.77.170
  Masque de sous-réseau. . . . . . . . . : 255.255.240.0
  Passerelle par défaut. . . . . . . . . : 10.33.79.254

   ```

Je suis nul je n'ai pas de carte réseaux ethernet..

<h1> Si t'as un accès internet normal </h1>

``` powershell
C:\Users\vicky> ipconfig /all

Carte réseau sans fil Wi-Fi :

   Passerelle par défaut. . . . . . . . . : 10.33.79.254
   
   ```

``` powershell
C:\Users\vicky> ipconfig /all

Carte réseau sans fil Wi-Fi :

   Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
                                       1.1.1.1
   
   ```

``` powershell
C:\Users\vicky> ipconfig /all

Carte réseau sans fil Wi-Fi :

   PServeur DHCP . . . . . . . . . . . . . : 10.33.79.254
   
   ```

 <h1> BONUS : Détermine s'il y a un pare-feu actif sur ta machine</h1>

``` powershell
C:\Users\vicky> Get-NetFirewallProfile | ft Name,Enabled

Name    Enabled
----    -------
Domain     True
Private    True
Public     True  
   ```

<h1> Envoie un ping vers...</h1>

```  powershell
 C:\Users\vicky> ping 10.33.77.170

  Envoi d’une requête 'Ping'  10.33.77.170 avec 32 octets de données :
  Réponse de 10.33.77.170 : octets=32 temps<1ms TTL=128
  Réponse de 10.33.77.170 : octets=32 temps<1ms TTL=128
```

```  powershell
 C:\Users\vicky> ping 127.0.0.1

Envoi d’une requête 'Ping'  127.0.0.1 avec 32 octets de données :
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128

Statistiques Ping pour 127.0.0.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```
<h1>On continue avec ping</h1>


``` powershell
:\Users\vicky> ping 10.33.79.254

Envoi d’une requête 'Ping'  10.33.79.254 avec 32 octets de données :
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.

Statistiques Ping pour 10.33.79.254:
    Paquets : envoyés = 4, reçus = 0, perdus = 4 (perte 100%),
   ```

``` powershell
C:\Users\vicky> ping 10.33.77.156

Envoi d’une requête 'Ping'  10.33.77.156 avec 32 octets de données :
Réponse de 10.33.77.156 : octets=32 temps=9 ms TTL=128
Réponse de 10.33.77.156 : octets=32 temps=9 ms TTL=128
Réponse de 10.33.77.156 : octets=32 temps=10 ms TTL=128
Réponse de 10.33.77.156 : octets=32 temps=9 ms TTL=128

Statistiques Ping pour 10.33.77.156:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 9ms, Maximum = 10ms, Moyenne = 9ms
```

``` Powershell

 C:\Users\vicky> ping instagram.com

Envoi d’une requête 'ping' sur instagram.com [185.60.219.174] avec 32 octets de données :
Réponse de 185.60.219.174 : octets=32 temps=16 ms TTL=55
Réponse de 185.60.219.174 : octets=32 temps=16 ms TTL=55
Réponse de 185.60.219.174 : octets=32 temps=14 ms TTL=55
Réponse de 185.60.219.174 : octets=32 temps=13 ms TTL=55

Statistiques Ping pour 185.60.219.174:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 13ms, Maximum = 16ms, Moyenne = 14ms
```

<h1> Faire une requête DNS à la main </h1>

``` powershell
 C:\Users\vicky> Resolve-DnsName www.wikileaks.org

Name                           Type   TTL   Section    NameHost
----                           ----   ---   -------    --------
www.wikileaks.org              CNAME  1469  Answer     wikileaks.org

Name       : wikileaks.org
QueryType  : A
TTL        : 1468
Section    : Answer
IP4Address : 51.159.197.136


Name       : wikileaks.org
QueryType  : A
TTL        : 1468
Section    : Answer
IP4Address : 80.81.248.21


Name                   : wikileaks.org
QueryType              : SOA
TTL                    : 1469
Section                : Authority
NameAdministrator      : root.wlinfra.org
SerialNumber           : 2022082502
TimeToZoneRefresh      : 21600
TimeToZoneFailureRetry : 3600
TimeToExpiration       : 604800
DefaultTTL             : 1800
```

``` powershell
C:\Users\vicky> Resolve-DnsName www.torproject.org

Name                                           Type   TTL   Section    IPAddress
----                                           ----   ---   -------    ---------
www.torproject.org                             AAAA   150   Answer     2a01:4f8:fff0:4f:266:37ff:fe2
                                                                       c:5d19
www.torproject.org                             AAAA   150   Answer     2620:7:6002:0:466:39ff:fe7f:1
                                                                       826
www.torproject.org                             AAAA   150   Answer     2620:7:6002:0:466:39ff:fe32:e
                                                                       3dd
www.torproject.org                             AAAA   150   Answer     2a01:4f9:c010:19eb::1
www.torproject.org                             AAAA   150   Answer     2a01:4f8:fff0:4f:266:37ff:fea
                                                                       e:3bbc
www.torproject.org                             A      139   Answer     204.8.99.146
www.torproject.org                             A      139   Answer     95.216.163.36
www.torproject.org                             A      139   Answer     204.8.99.144
www.torproject.org                             A      139   Answer     116.202.120.166
www.torproject.org                             A      139   Answer     116.202.120.165




```

```powershell
 Resolve-DnsName www.thinkerview.com

Name                                           Type   TTL   Section    IPAddress
----                                           ----   ---   -------    ---------
www.thinkerview.com                            AAAA   300   Answer     2a06:98c1:3121::7
www.thinkerview.com                            AAAA   300   Answer     2a06:98c1:3120::7
www.thinkerview.com                            A      300   Answer     188.114.97.7
www.thinkerview.com                            A      300   Answer     188.114.96.7

```

