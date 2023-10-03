# 2. Virtual LAN (IEEE 802.1q)

<figure markdown>
  ![](../img/basics/vlan.drawio.svg){ loading=lazy width="500px" }
</figure>

=== "VyOS"
    ```sh
    set interfaces ethernet eth0 vif 15 address '10.0.0.1/30'
    ```

=== "Mikrotik RouterOS"
    ```sh
    interface/vlan/add interface=ether1 vlan-id=15 name=ether1.15
    ip/address/add address=10.0.0.2/30 interface=ether1.15
    ```

![](../img/basics/wikipedia/ieee8021q.png)

> TODO: ping -> In Wireshark ARP und ICMP betrachten und bilder machen (unterschied zu ethernet (vlan tag) aufzeigen)