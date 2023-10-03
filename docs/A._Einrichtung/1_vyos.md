# 1. VyOS

VyOS ist ein CLI basiertes Open-Source-Netzwerk-Betriebssystem, das auf dem Debian GNU/Linux basiert. 
Die aus den Nightly Build resultierenden ISO's (Rolling Release) können direkt von der [offiziellen Website](https://vyos.net/get/nightly-builds/) heruntergeladen werden.

## Wahl des VyOS ISO Images
Wenn Sie die LTS-Version (derzeit VyOS 1.3) verwenden möchten, müssen Sie das ISO-Image [selbst bauen](https://docs.vyos.io/en/latest/contributing/build-vyos.html). Wir haben diesen Prozess innerhalb einer GitHub Action Pipeline umgesetzt und das ISO Image um eigene Pakete (u. A. Prometheus Exporter) ergänzt [github.com/os-builds/vyos](https://github.com/os-builds/vyos).

## Einrichtung der virtuellen Maschine
Um VyOS innerhalb einer VirtualBox VM aufzusetzen, muss zunächst das gewünschte ISO Image heruntergeladen werden. Anschließend wird eine neue virtuelle Maschine mit diesem erstellt (512 MB RAM, 1 Kern, 5 GB Speicher sind ausreichend) und gestartet. Sobald die Login Maske sichtbar ist, ist ein Login mit den Zugangsdaten `vyos` / `vyos` möglich.

Nun kann der Installationsprozess mit dem Befehl `install image` gestartet werden, bei dem primär die vorausgewählten Optionen gewählt werden können.

### Netzwerk und Serielle Konsole konfigurieren

Nachdem die Installation abgeschlossen ist, wird die VM heruntergefahren und das Netzwerk umkonfiguriert (Internal Network). Außerdem wird eine Serielle Schnittstelle hinzugefügt (Port Mode: Host Pipe, Disable: Connect to existing pipe/socket), die die Interaktion mit der CLI vereinfacht.

> TODO @Luis Bilder VirtualBox Konfiguration: Netzwerk und Serielle Konsole

> TODO @Luis Beschreibung wie Netzwerk umkonfiguriert wird verbessern

Im Anschluss kann die virtuelle Maschine wieder gestartet werden, wodurch die Serielle Konsole initialisiert wird.

=== "Linux"
    Um sich mit dem UNIX Socket, der die Serielle Schnittstelle zur virtuellen Maschine darstellt, zu verbinden kann die Software socat verwendet werden:

    ```sh
    socat UNIX-CONNECT:/tmp/vyos -,b57600
    ```

    Anschließend kann die Shell verbessert werden, indem wir unser Termin so umkonfigurieren, dass alle Tastatureingaben direkt an den Socket gesendet werden und nicht zunächst lokal gepuffert werden. Dadurch ist dann auch die Automatische Vervollständigung mit <kbd>&#x21b9;</kbd> (Tabulator) möglich.

    Dazu wird zunächst die Serielle Konsole in den Hintergrund geschickt: <kbd>STRG</kbd> + <kbd>z</kbd>

    Anschließend wird stty umkonfiguriert:

    ```sh
    stty raw -echo
    ```

    Im letzten Schritt kann die Serielle Konsole wieder in den Vordergrund geholt werden.  
    Dazu wird `fg` getippt und zweimal <kbd>&#x21b5;</kbd> (Enter) gedrückt.

=== "Windows"
    > TODO @Luis Beschreibung für Verwendung von Putty oder ähnlichem einfügen