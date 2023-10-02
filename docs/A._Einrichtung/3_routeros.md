# 3. Mikrotik RouterOS

* routeros (kurze erklärung was es ist, chr ohne lizenz: max 1mbit/s, für testzwecke ausreichend, lizenzmodell erklären)

> TODO @Luis Aufsetzen von VyOS VM mit VirtualBox unter Windows und Linux erläutern

## Einrichtung von RouterOS
- chr von mikrotik als vdi runterladen: https://download.mikrotik.com/routeros/7.11.2/chr-7.11.2.vdi.zip
- vm anlegen -> do not add virtual hard disk
- serielle konsole hinzufügen und network interface auf internal network stellen
- vdi datei aus der zip datei entpacken, in virtualbox ordner (unter der jeweiligen vm anlegen)
- hard disk der vm hinzufügen (storage -> sata controller -> +)
- nach dem booten mit admin (leeres password) einloggen und passwort ändern

### Serielle Konsole konfigurieren
- Enable Serial Port:
  Port Mode: Host Pipe,
  Disable: Connect to existing pipe/socket,
  Path: `/tmp/vyos15serial`
- `socat UNIX-CONNECT:/tmp/vyos13serial -,b57600`  - TODO braucht ggf. andere werte?
- Autocompletion aktivieren:
  <kbd>STRG<kbd> + <kbd>z<kbd> (background)
  `stty raw -echo` <kbd>ENTER<kbd> (alle tastatureingaben direkt an serielle konsole senden, nicht lokal verarbeiten)
  `fg` <kbd>ENTER<kbd><kbd>ENTER<kbd> (foreground)
