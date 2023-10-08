# Einleitung

## VyOS
VyOS verfügt über eine Befehl-Autovervollständigung mit <kbd>&#x21b9;</kbd> (Tabulator). Des weiteren kann man Befehle
ähnlich wie bei Cisco verkürzen.  
Aus `configuration` kann `conf` und `set interface ethernet eth0 address 10.10.10.10/24` kann man mit 
`set i e eth0 a 10.10.10.10/24` abgekürzt werden. Die Befehl-Autovervollständigung ist dann verfügbar, wenn mit der
Reihenfolge an eingegebenen Buchstaben nur noch ein Befehl verfügbar ist. Wenn kein direkt passender Befehl vorhanden ist
oder mehrere Befehle die eingegebene Zeichenreihenfolge haben, wird eine Liste an verfügbaren Befehlen mit dieser 
Zeichenfolge ausgegeben.


### Operator-Mode vs. Configuration-Mode
Der Operator-Mode (Operationsmodus) hat den nutzen, Konfigurationselemente mit `set` zu setzen bzw. mit `delete` zu löschen.
Mithilfe von `edit` können Subelemente addressiert werden.

```shell
[ edit ]
vyos@vyos−XXX:∼$ set system host−name example
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


Der Configuration-Mode (Konfigurationsmodus) hat drei Konfigurationsumgebungen:  

| <!-- -->       | <!-- -->                                                 |      
|:---------------|:---------------------------------------------------------|
| config         | wurde konfiguriert, aber noch nicht angewandt (commited) |        
| running-config | wird aktuell von VyOS verwendet                          |        
| startup-config | wird beim Start von VyOS geladen                         |  

Die Konfiguration aus der Umgebung `config` kann mit dem Befehl `commit` in die Umgebung `running-config` geschrieben 
werden. Die Konfiguration aus der Umgebung `running-config` kann mit dem Befehl `save` in die Umgebung `startup-config` geschrieben
werden.


Das wechseln zwischen den Umgebungen funktioniert wie folgt:
```shell
vyos@vyos-XXX:~$
# In Configuration-Modus wechseln.
vyos@vyos-XXX:~$ configure
[edit]
vyos@vyos-XXX#
# In Operator-Modus wechseln.
[edit]
vyos@vyos-XXX# exit
exit
vyos@vyos-XXX:~$ 
```

## MikroTik RouterOS (ros)
RouterOS hat - ähnlich wie bei VyOS - eine Befehl-Autovervollständigung. Die Befehle sind bei RouterOS aber etwas anders
aufgebaut. Zwischen den einzelnen Argumenten der Befehle sind keine Leerzeichen, sondern Slashes `/`. Ansonsten sind die
Befehle ähnlich aufgebaut.

# TODO @Nico Weiter ausführen (gerne in Stichpunkten formulieren kann ich übernehmen)