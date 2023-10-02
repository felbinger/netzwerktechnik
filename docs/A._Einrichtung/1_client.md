# 1. Clients

Um die Pakete in den internen Netzwerken mittels Wireshark passiv betrachten zu können setzen wir uns auch noch eine 
Client VM auf.

> TODO @Luis Erklärung warum wir das wollen

Hierfür kann beispielsweise das Betriebssystem Linux Mint mit dem Desktopenvironment Mate verwendet werden, welches 
[hier](https://linuxmint.com/download.php) heruntergeladen werden kann.

> TODO @Luis Beschreiben das VM erstellt wird und Linux Mint installiert wird

Nachdem die virtuelle Maschine neu gestartet wurde, kann Wireshark über den Paketmanager installiert werden:
```shell
apt install -y wireshark
```

> TODO @Luis Erläutern, dass aufzeichnen für non-root nutzer erlaubt werden soll 

Damit unser Client keine eigenen Pakete in die internen Netzwerke unserer Router sendet, deaktivieren wir die IPv4 und 
IPv6 Netzwerkkonfiguration im NetworkManager.

> TODO @Luis Bilder für Linux Mint mit Mate

Anschließend fahren wir die virtuelle Maschine herunter und konfigurieren das Netzwerkinterface auf unser internes 
Netzwerk um. Den "Promiscuous Mode" setzen wir auf "Allow All", sodass die virtuelle Netzwerkkarte unserer virtuellen
Maschine nicht nur die Pakete empfängt, die für die eigene MAC Adresse oder alle Teilnehmer im Netz (Broadcast / 
Multicast) bestimmt sind.

> TODO @Luis Bild der VirtualBox Netzwerkkonfiguration

> TODO @Luis Verwendung von Wireshark grob erklären, im ersten Grundlagenkapitel wird das ganze an Ethernet gezeigt. 
