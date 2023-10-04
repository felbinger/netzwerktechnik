# 3. QinQ (IEEE 802.1ad)

!!! info 
    QinQ-Tagging, auch als IEEE 802.1ad oder Provider-Bridging bekannt, wird in Netzwerken eingesetzt, um die Begrenzung 
    von VLAN-IDs zu überwinden. Es ermöglicht Dienstanbietern, VLAN-Frames von Kunden innerhalb ihrer eigenen VLAN-Tags 
    zu kapseln, wodurch der transparente Transport von VLAN-Verkehr mehrerer Kunden ermöglicht wird.

<figure markdown>
  ![](../assets/img/basics/qinq.drawio.svg){ loading=lazy width="500px" }
</figure>

Da wir keinen Switch haben, auf dem wir das ganze nachstellen können, konfigurieren wir dieses vereinfachte Beispiel:

<figure markdown>
  ![](../assets/img/basics/qinq-simple.drawio.svg){ loading=lazy width="500px" }
</figure>

=== "VyOS"
    ```sh
    set interfaces ethernet eth0 vif-s 20 vif-c 15 address '10.0.0.1/30'
    ```

=== "Mikrotik RouterOS"
    ```sh
    interface/vlan/add interface=ether1 vlan-id=20 name=ether1.20 use-service-tag=yes
    interface/vlan/add interface=ether1.20 vlan-id=15 name=ether1.20.15
    ip/address/add address=10.0.0.2/30 interface=ether1.20.15
    ```

![](../assets/img/basics/wikipedia/ieee8021ad.png)

> TODO muss noch erarbeitet werden.
