# 1. Ethernet

!!! info
    Ethernet ist das meist verwendete OSI-Layer 2 Protokoll, es ermöglicht die Kommunikation von Geräten in einem 
    lokalen Netzwerk. Ethernet verwendet MAC-Adressen (Media Access Control-Adressen), um Geräte in einem Netzwerk 
    eindeutig zu identifizieren und den Datenverkehr zwischen ihnen zu steuern.

In diesem Beispiel werden, unter Verwendung einer IP Adresse, Ethernet Frames und ARP sowie NDP Pakete erläutert.

Zwar ist es, in einem Layer 2 Netzwerk, prinzipiell möglich Ethernet-Frames direkt zu versenden, allerdings wird
in den meisten Fällen dennoch die Layer 3 (IPv4 / IPv6) Adresse zur Addressierung von Mitglieder verwendet.

<figure markdown>
  ![](../assets/img/basics/ethernet.drawio.svg){ loading=lazy width="500px" }
</figure>

=== "VyOS"
    ```sh
    config
    set interfaces ethernet eth0 address '10.0.0.1/30'
    set interfaces ethernet eth0 address 'fd00::1/64'
    commit
    save
    ```

=== "Mikrotik RouterOS"
    ```sh
    ip/address/add address=10.0.0.2/30 interface=ether1
    ipv6/address/add address=fd00::2/64 interface=ether1
    ```

<!-- TODO
Wireshark öffnen, Konfiguration anwenden, Warten bis NDP (fe80::) fertig
ping und ping6
ICMP, ARP, NDP (fd00::) betrachten und analysieren

Erklären was fe80:: Adresse ist
-->
