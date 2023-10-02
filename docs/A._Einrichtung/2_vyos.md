# 2. VyOS
 
* vyos (kurze erklärung was es ist, wo man iso her bekommt)


> TODO @Luis Aufsetzen von VyOS VM mit VirtualBox unter Windows und Linux erläutern

## Einrichtung von VyOS
- iso runterladen (rolling release 1.5 oder stable release 1.3 (github.com/os-builds/vyos))
- vm mit diesem iso erstellen
- serielle konsole hinzufügen und network interface auf internal network stellen
- vm starten mit vyos / vyos einloggen
- `install image` - danach default werte wählen, außer override disk und vyos passwort

### Serielle Konsole konfigurieren
- Enable Serial Port:
  Port Mode: Host Pipe,
  Disable: Connect to existing pipe/socket,
  Path: `/tmp/vyos15serial`
- `socat UNIX-CONNECT:/tmp/vyos13serial -,b57600`
- Autocompletion aktivieren:
  <kbd>STRG<kbd> + <kbd>z<kbd> (background)
  `stty raw -echo` <kbd>ENTER<kbd> (alle tastatureingaben direkt an serielle konsole senden, nicht lokal verarbeiten)
  `fg` <kbd>ENTER<kbd><kbd>ENTER<kbd> (foreground)
