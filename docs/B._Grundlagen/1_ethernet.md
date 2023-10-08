# 1. Ethernet

!!! info
    Ethernet ist das meist verwendete OSI-Layer 2 Protokoll, es ermöglicht die Kommunikation von Geräten in einem 
    lokalen Netzwerk. Ethernet verwendet MAC-Adressen (Media Access Control-Adressen), um Geräte in einem Netzwerk 
    eindeutig zu identifizieren und den Datenverkehr zwischen ihnen zu steuern.


<figure markdown>
  ![](../assets/img/basics/ethernet.drawio.svg){ loading=lazy width="500px" }
</figure>

=== "VyOS"
    ```sh
    config
    set interfaces ethernet eth0 address '10.0.0.1/30'
    save
    commit
    ```

    ```sh
    config
    ```
    ```sh
    set interfaces ethernet eth0 address '10.0.0.1/30'
    ```
    ```sh
    commit
    ```
    ```sh
    save
    ```


=== "Mikrotik RouterOS"
    ```sh
    ip/address/add address=10.0.0.2/30 interface=ether1
    ```

> TODO: ping -> In Wireshark ARP und ICMP betrachten und bilder machen
