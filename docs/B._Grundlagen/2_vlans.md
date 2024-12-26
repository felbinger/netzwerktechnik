# 2. Virtual LAN (IEEE 802.1q)

!!! info
    Ein Virtual Local Area Network ist ein logisches Netzwerk, welches in einem physischen Netzwerk erstellt wird, um 
    Gruppen von Geräten zu isolieren und zu organisieren. VLANs ermöglichen es, den Datenverkehr in einem Netzwerk zu 
    segmentieren und zu steuern, indem sie Geräte in unterschiedliche virtuelle Gruppen aufteilen, unabhängig von ihrer
    physischen Position im Netzwerk.

<figure markdown>
  ![](../assets/img/basics/2_vlan.drawio.svg){ loading=lazy width="500px" }
</figure>

=== "VyOS"
    ```sh
    configure
    delete interfaces ethernet eth0 address
    set interfaces ethernet eth0 vif 15 address '10.0.0.1/30'
    commit
    save
    ```

=== "Mikrotik RouterOS"
    ```sh
    interface/vlan/add interface=ether1 vlan-id=15 name=ether1.15
    ip/address/add address=10.0.0.2/30 interface=ether1.15
    ```

Im Vergleich zum Paketmitschnitt aus dem [vorherigen Kapitel (Ethernet)](./1_ethernet.md) beinhaltet der Ethernet Header
vier weitere Bytes nach der Quell-MAC-Adresse, bevor der EtherType beginnt. Darin befindet sich der 802.1Q Header mit der
gesetzten VLAN ID 15.

<figure markdown>
  ![](../assets/img/basics/2_vlan-frame.drawio.svg){ loading=lazy width="500px" }
</figure>

<!-- TODO @Nico
Lediglich Screenshot aus Wireshark von Ethernet Paket für VLAN ID
-->
