<h1>I. Récolte d'informations </h1>
<h2>  Adresses IP de ta machine </h2>

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

<h2> Si t'as un accès internet normal </h2>

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

<h2> BONUS : Détermine s'il y a un pare-feu actif sur ta machine</h2>

``` powershell
C:\Users\vicky> Get-NetFirewallProfile | ft Name,Enabled

Name    Enabled
----    -------
Domain     True
Private    True
Public     True  
   ```

<h1> II. Utiliser le réseau</h1>
<h2> Envoie un ping vers...</h2>

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
<h2>On continue avec ping</h2>


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

<h2> Faire une requête DNS à la main </h2>

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

<h1>IV. Network scanning et adresses IP </h1>

<h2>Effectue un scan du réseau auquel tu es connecté </h2>

``` powershell
PS C:\Users\vicky> nmap -sn -PR 192.168.1.0/24
Starting Nmap 7.95 ( https://nmap.org ) at 2024-10-06 13:34 Paris, Madrid (heure dÆÚtÚ)
Nmap scan report for ALG7 (192.168.1.1)
Host is up (0.023s latency).
MAC Address: B0:B3:69:94:E8:EE (Shenzhen Sdmc Technology)
Nmap scan report for 192.168.1.66
Host is up (0.097s latency).
MAC Address: F4:64:12:F3:EE:DD (Sony Interactive Entertainment)
Nmap scan report for 192.168.1.82
Host is up (0.049s latency).
MAC Address: 4E:37:34:FA:5F:10 (Unknown)
Nmap scan report for STB (192.168.1.170)
Host is up (0.059s latency).
MAC Address: CC:2D:1B:D2:74:BC (SFR)
Nmap scan report for DESKTOP-NI9KRV7 (192.168.1.106)
Host is up.
Nmap done: 256 IP addresses (5 hosts up) scanned in 2.80 seconds
```
```powershell
PS C:\Users\vicky> Get-NetAdapter
Name                      InterfaceDescription                    ifIndex Status       MacAddress             LinkSpeed
----                      --------------------                    ------- ------       ----------             ---------
Wi-Fi                     Qualcomm Atheros QCA9377 Wireless Ne...      10 Up           10-63-C8-68-8C-21     433.3 Mbps

```
<h2>Changer d'adresse IP </h2> 

```powershell
PS C:\WINDOWS\system32>  New-NetIPAddress –InterfaceIndex 10 –IPAddress 192.168.1.83 –PrefixLength 24 –DefaultGateway 192.168.1.1


IPAddress         : 192.168.1.83
InterfaceIndex    : 10
InterfaceAlias    : Wi-Fi
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24 
PrefixOrigin      : Manual
SuffixOrigin      : Manual
AddressState      : Tentative
ValidLifetime     :
PreferredLifetime :
SkipAsSource      : False
PolicyStore       : ActiveStore
```
```powershell
PS C:\Users\vicky> ipconfig
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6. . . . . . . . . . . . . .: 2a02:8428:f996:7001:8665:5a02:4a19:9912
   Adresse IPv6 temporaire . . . . . . . .: 2a02:8428:f996:7001:1c6e:feb3:4236:afce
   Adresse IPv6 de liaison locale. . . . .: fe80::c127:166:3c80:8571%10
   Adresse IPv4. . . . . . . . . . . . . .: 192.168.1.83
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . : fe80::1%10
                                       192.168.1.1


```