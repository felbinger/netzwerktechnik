# Einleitung

## VyOS
VyOS verfügt über eine Befehl-Autovervollständigung mit <kbd>&#x21b9;</kbd> (Tabulator).
Des Weiteren kann man Befehle ähnlich wie bei Cisco verkürzen, sofern diese Eindeutig sind.
Der Befehl `configuration` kann auch einfach verkürzt als `conf` formuliert werden.
Ein extremes Beispiel zeigt `set interface ethernet eth0 address 10.10.10.10/24`,
welches auch mit  `set i e eth0 a 10.10.10.10/24` abgekürzt werden kann.
Existieren für die Eingegebenen Zeichen mehrere Möglichkeiten, so werden diese dargestellt, 
sodass der Nutzer das nächste passende Zeichen für den gewünschten Befehl ergänzen kann.

### Operator vs. Configuration-Mode
VyOS hat zwei Modi, einen für die Administration und einen für die Konfiguration.

Der Operator-Mode dient beispielsweise dem Ausführen von Statusbefehle wie 
`show interfaces`, der Konfigurationsmodus wie der Name bereits sagt, der Konfiguration
des Systems mit Befehlen wie `set` und `delete`.

Zwischen den beiden Modi lässt sich mit den Befehlen `configure` (Operator -> Config)
und `exit` (Config -> Operator) wechseln. Mit `run` können Befehle im Config Mode als
Operator ausgeführt werden (Vgl. Cisco `do`)

Innerhalb des Konfigurationsmodus gibt es drei Konfigurationsumgebungen:

| <!-- -->       | <!-- -->                                                 |      
|:---------------|:---------------------------------------------------------|
| config         | wurde konfiguriert, aber noch nicht angewandt (commited) |
| running-config | wird aktuell von VyOS verwendet                          |      
| startup-config | wird beim Start von VyOS geladen                         |

Die Konfiguration aus der Umgebung `config` kann mit dem Befehl `commit` in die Umgebung `running-config` geschrieben 
werden. Die Konfiguration aus der Umgebung `running-config` kann mit dem Befehl `save` in die Umgebung `startup-config` geschrieben
werden.

<!-- TODO asciinema -->
```shell
vyos@vyos:~$ show interfaces
Codes: S - State, L - Link, u - Up, D - Down, A - Admin Down
Interface        IP Address                        S/L  Description
---------        ----------                        ---  -----------
eth0             192.168.122.2/24                  u/u  
lo               127.0.0.1/8                       u/u  
                 ::1/128                                
vyos@vyos:~$ configure
[edit]
vyos@vyos# set int eth eth0 addr 10.10.10.10/24
[edit]
vyos@vyos# do sh run
vbash: syntax error near unexpected token `do'
[edit]
vyos@vyos# run show int
Codes: S - State, L - Link, u - Up, D - Down, A - Admin Down
Interface        IP Address                        S/L  Description
---------        ----------                        ---  -----------
eth0             192.168.122.2/24                  u/u  
lo               127.0.0.1/8                       u/u  
                 ::1/128                                
[edit]
vyos@vyos# commit
[edit]
vyos@vyos# run show int
Codes: S - State, L - Link, u - Up, D - Down, A - Admin Down
Interface        IP Address                        S/L  Description
---------        ----------                        ---  -----------
eth0             192.168.122.2/24                  u/u  
                 10.10.10.10/24                         
lo               127.0.0.1/8                       u/u  
                 ::1/128                                
[edit]
vyos@vyos# save
Saving configuration to '/config/config.boot'...
Done
[edit]
vyos@vyos# exit
exit
vyos@vyos:~$ 
```

Mithilfe von `edit` können Subelemente addressiert werden.

```shell
[ edit ]
vyos@vyos−XXX:∼$ delete system ntp server 0.pool.ntp.org
```

```shell
# in subelement ‘system ntp‘ wechseln
[ edit ]
vyos@vyos−XXX:∼$ edit system ntp
[ edit system ntp ]
vyos@vyos−XXX:∼$ set server 0.pool.ntp.org
```

## MikroTik RouterOS (ros)
In RouterOS gibt es lediglich eine Umgebung. Gesetzte Konfigurationen werden sofort aktiv. Wie bei anderen
Systemen gibt es eine Befehl-Autovervollständigung mit <kbd>&#x21b9;</kbd> (Tabulator).

Konfigurationselemente sind in Kategorien angeordnet (z. B. `ip` und `ipv6`), die mit Slashes (`/`) voneinander
getrennt sind. Das Setzen von Konfigurationen ist durch den `add`-Befehl innerhalb der gewünschten Kategorie
möglich. Zum Löschen muss zunächst mit dem `print`-Befehl ermittelt werden, welche ID das Element hat. Anschließend
kann es mit `remote <ID>` gelöscht werden

<!-- TODO asciinema -->
