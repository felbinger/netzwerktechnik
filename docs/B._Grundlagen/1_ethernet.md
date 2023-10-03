# 1. Ethernet

<figure markdown>
  ![](../img/basics/ethernet.drawio.svg){ loading=lazy width="500px" }
</figure>

=== "VyOS"
    ```sh
    set interfaces ethernet eth0 address '10.0.0.1/30'
    ```

=== "Mikrotik RouterOS"
    ```sh
    ip/address/add address=10.0.0.2/30 interface=ether1
    ```

> TODO: ping -> In Wireshark ARP und ICMP betrachten und bilder machen
