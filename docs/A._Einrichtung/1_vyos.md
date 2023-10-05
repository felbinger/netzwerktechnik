# 1. VyOS

VyOS ist ein CLI basiertes Open-Source-Netzwerk-Betriebssystem, welches auf dem Debian GNU/Linux basiert.
Die aus den Nightly Build resultierenden ISO's (Rolling Release) können direkt von der [offiziellen Website](https://vyos.net/get/nightly-builds/)
heruntergeladen werden.

## Wahl des VyOS ISO Images

Wenn Sie die LTS-Version (derzeit VyOS 1.3) verwenden möchten, müssen Sie das
ISO-Image [selbst bauen](https://docs.vyos.io/en/latest/contributing/build-vyos.html). Wir haben diesen Prozess innerhalb einer GitHub Action Pipeline umgesetzt und das ISO
Image um eigene Pakete (u. A. Prometheus Exporter) ergänzt [github.com/os-builds/vyos](https://github.com/os-builds/vyos).

## Einrichtung der virtuellen Maschine

Um VyOS innerhalb einer VirtualBox VM aufzusetzen, muss zunächst das gewünschte ISO Image heruntergeladen werden.
Anschließend wird eine neue virtuelle Maschine mit diesem erstellt (512 MB RAM, 1 Kern, 5 GB Speicher sind ausreichend)
und gestartet. Sobald die Login-Maske sichtbar ist, kann man sich mit den Zugangsdaten `vyos` / `vyos` einloggen 
(Hinweis: amerikanisches Tastaturlayout!).

Nun kann der Installationsprozess mit dem Befehl `install image` gestartet werden, bei dem primär die vorausgewählten
Optionen gewählt werden können.

<asciinema-player src="../../assets/cast/vyos-install.cast"></asciinema-player>

### Netzwerk und serielle Konsole konfigurieren

Nachdem die Installation abgeschlossen ist, wird die VM heruntergefahren und das Netzwerk umkonfiguriert (Internal
Network). Außerdem wird eine serielle Schnittstelle hinzugefügt (Port Mode: Host Pipe, Disable: Connect to existing
pipe/socket), welche die Interaktion mit der CLI vereinfacht.

![](../../assets/img/setup/virtualbox/vyos-internal-network.png){ lazy-loading }
![](../../assets/img/setup/virtualbox/vyos-serial.png){ lazy-loading }

> TODO @Luis Beschreibung wie Netzwerk umkonfiguriert wird verbessern

Im Anschluss kann die virtuelle Maschine wieder gestartet werden, wodurch die serielle Konsole initialisiert wird.

=== "Linux"
    Um sich mit dem UNIX Socket, der die serielle Schnittstelle zur virtuellen Maschine darstellt, zu verbinden kann die 
    Software socat verwendet werden:

    ```sh
    socat UNIX-CONNECT:/tmp/vyos -,b57600
    ```

    Anschließend kann die Shell verbessert werden, indem wir unser Terminal so umkonfigurieren, dass alle
    Tastatureingaben direkt an den Socket gesendet werden und nicht zunächst lokal gepuffert werden. Dadurch ist dann 
    auch die Automatische Vervollständigung mit <kbd>&#x21b9;</kbd> (Tabulator) möglich.

    Dazu wird zunächst die serielle Konsole in den Hintergrund geschickt: <kbd>STRG</kbd> + <kbd>z</kbd>

    Anschließend wird stty umkonfiguriert:

    ```sh
    stty raw -echo
    ```

    Im letzten Schritt kann die serielle Konsole wieder in den Vordergrund geholt werden.  
    Dazu wird `fg` getippt und zweimal <kbd>&#x21b5;</kbd> (Enter) gedrückt.

=== "Windows"
    > TODO @Luis Beschreibung für Verwendung von Putty oder ähnlichem einfügen
