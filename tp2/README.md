<H1> 1. Quelques pings </H1>
<H2> Prouvez que votre configuration est effective ! </H2>

(Je n'ai pas de port ethernet je me suis incrusté avec des gens qui en ont un..)

```powersell
PS C:\Windows\system32> Get-NetIPAddress -InterfaceAlias "Ethernet"


IPAddress         : fe80::72ea:a395:faf8:efcb%7
InterfaceIndex    : 7
InterfaceAlias    : Ethernet
AddressFamily     : IPv6
Type              : Unicast
PrefixLength      : 64
PrefixOrigin      : WellKnown
SuffixOrigin      : Link
AddressState      : Preferred
ValidLifetime     :
PreferredLifetime :
SkipAsSource      : False
PolicyStore       : ActiveStore

IPAddress         : 10.10.10.10
InterfaceIndex    : 7
InterfaceAlias    : Ethernet
AddressFamily     : IPv4
Type              : Unicast
PrefixLength      : 24
PrefixOrigin      : Manual
SuffixOrigin      : Manual
AddressState      : Preferred
ValidLifetime     :
PreferredLifetime :
SkipAsSource      : False
PolicyStore       : ActiveStore
```
<H2> Tester que votre LAN + votre adressage IP est fonctionnel </H2>

```powersell
PS C:\Windows\system32> ping 10.10.10.54

Envoi d’une requête 'Ping'  10.10.10.54 avec 32 octets de données :
Réponse de 10.10.10.54 : octets=32 temps<1ms TTL=128
Réponse de 10.10.10.54 : octets=32 temps<1ms TTL=128
Réponse de 10.10.10.54 : octets=32 temps<1ms TTL=128
Réponse de 10.10.10.54 : octets=32 temps=1 ms TTL=128

Statistiques Ping pour 10.10.10.54:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 1ms, Moyenne = 0ms
```
<H1> 1. Animal numérique </H1>
<H2> Sur le PC serveur <H2>

```powershell
PS C:\Users\hugos\Downloads\netcat-win32-1.11\netcat-1.11> .\nc.exe -l -p 7777
cc
hoo
Tous va bien ?
ouiii
```

<H2> Sur le PC serveur toujours </H2>

```powershell
TCP    10.10.10.54:139        0.0.0.0:0              LISTENING
 Can not obtain ownership information

``` 

<H2> Sur le PC client </H2>

```powershell
PS C:\Users\hugos\Downloads\netcat-win32-1.11\netcat-1.11> .\nc.exe 10.10.10.10 8888
```

<H2>Echangez-vous des messages </H2>

```powershell
PS C:\Users\hugos\Downloads\netcat-win32-1.11\netcat-1.11> .\nc.exe 10.10.10.10 8888
hello
yo bonjour
non
``` 

## Utilisez une commande qui permet de voir la connexion en cours

```powershell 
TCP    10.10.10.54:6611       10.10.10.10:8888       ESTABLISHED
 [nc.exe]
```
### Faites une capture Wireshark complète d'un échange 
voir netcat1.pcap

### Inversez les rôles 
### Sur le PC serveur

```powershell 
PS C:\Users\hugos\Downloads\netcat-win32-1.11\netcat-1.11> .\nc.exe -l -p 7777
cc
hoo
Tous va bien ?
ouiii
```
### Sur le PC serveur toujours

```powershell
TCP    10.10.10.54:139        0.0.0.0:0              LISTENING
 Can not obtain ownership information
```
### Utilisez une commande qui permet de voir la connexion en cours
```powershell 
TCP    10.10.10.54:7777       10.10.10.10:4763       ESTABLISHED
 [nc.exe]
```

### Faites une capture Wireshark complète d'un échange
voir netcat2.pcap

## III. Analyse de vos applications usuelles 

### Utilisez Wireshark pour capturer du trafic HTTP
voir _http.pcap_

### Pour les 5 applications

Tableau : _service1.pcap_

```powershell
  TCP    192.168.1.106:53204    162.247.243.29:443     ESTABLISHED
 [tableau.exe]
``` 

Outlook: _service2.pcap_

``` powershell
TCP    192.168.1.106:50942    20.189.173.3:443       ESTABLISHED
 [olk.exe]
``` 

Teams :  _service3.pcap_

```powershell
  TCP    192.168.1.106:51381    57.153.79.230:443      ESTABLISHED
 [ms-teams.exe]
```

Arc :  _service4.pcap_

```powershell
TCP    192.168.1.106:51993    99.86.90.76:443        ESTABLISHED
 [Arc.exe]
```

Steam : _service5.pcap_

```powershell
TCP    192.168.1.106:53067    155.133.248.43:443     ESTABLISHED
 [steam.exe]
``` 