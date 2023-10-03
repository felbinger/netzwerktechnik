# 2. Mikrotik RouterOS

* routeros (kurze erklärung was es ist, chr ohne lizenz: max 1mbit/s, für testzwecke ausreichend, lizenzmodell erklären)

> TODO @Luis Aufsetzen von VyOS VM mit VirtualBox unter Windows und Linux erläutern

## Einrichtung der virtuellen Maschine

- system von mikrotik download seite als vdi runterladen: https://mikrotik.com/download (unter Cloud Hosted Router)
- vm anlegen -> do not add virtual hard disk
- serielle konsole hinzufügen und network interface auf internal network stellen
- vdi datei aus der zip datei entpacken, in virtualbox ordner (unter der jeweiligen vm anlegen)
- hard disk der vm hinzufügen (storage -> sata controller -> +)
- nach dem booten mit admin (leeres password) einloggen und passwort ändern

### Serielle Konsole konfigurieren
- Enable Serial Port:  
  Port Mode: Host Pipe,  
  Disable: Connect to existing pipe/socket,  
  Path: `/tmp/routeros7serial`  

Im Anschluss kann die virtuelle Maschine wieder gestartet werden, wodurch die Serielle Konsole initialisiert wird.

=== "Linux"
    Um sich mit dem UNIX Socket, der die Serielle Schnittstelle zur virtuellen Maschine darstellt, zu verbinden kann die Software socat verwendet werden:

    ```sh
    socat UNIX-CONNECT:/tmp/routeros7serial -,b9600,echo=0,raw
    ```

=== "Windows"
    > TODO @Luis Beschreibung für Verwendung von Putty oder ähnlichem einfügen
