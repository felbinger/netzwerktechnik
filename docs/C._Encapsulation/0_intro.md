# Encapsulation, Tunnel und VPN Protokolle

> TODO Tabelle mit vergleich

| Protokoll                                             | Verschlüsselung | OSI Schicht |
|-------------------------------------------------------|-----------------|:-----------:|
| Generic Routing Encapsulation (GRE)                   | Nein            |      3      |
| Virtual Extensible LAN VXLAN                          | Nein            |      4      |
| Generic Network Virtualization Encapsulation (Geneve) | Nein            |      4      |
| Layer 2 Tunneling Protocol (L2TP)                     | Ja mit IPsec    |      2      |
| Point-to-Point Tunneling Protocol (PPTP)              | Ja (unsicher)   |     3?      |
| IP Security (IPsec)                                   | Ja              |      3      |
| Wireguard                                             | Ja (ECC)        |     3/7     |
| Secure-Socket Tunneling Protocol (SSTP)               | Ja              |      7      |
| OpenVPN                                               | Ja              |      7      |
| OpenConnect                                           | Ja              |      7      |
 
