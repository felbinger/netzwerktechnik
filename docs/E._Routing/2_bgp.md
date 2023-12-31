# 2. Border Gateway Protocol

## Theoretische Grundlagen
!!! info "Was ist ein autonomes System?"
    Ein autonomes System (AS) ist eine Gruppe von IP-Netzen und Routern, welche unter einer einzigen administrativen 
    Kontrolle stehen und Routeninformationen über das Internet austauschen. Es wird durch eine eindeutige ASN 
    (Autonomous System Number) identifiziert.

## Szenario #1

<figure markdown>
  ![](../assets/img/routing/bgp/scenario-1.drawio.svg){ loading=lazy width="500px" }
</figure>

=== "VyOS"
    ```sh
    set interfaces ethernet eth0 address '10.0.0.1/30'

    # bis VyOS 1.3
    set protocols bgp 65000 neighbor 10.0.0.2 address-family ipv4-unicast
    set protocols bgp 65000 neighbor 10.0.0.2 remote-as 65100

    # ab VyOS 1.4
    set protocols bgp neighbor 10.0.0.2 address-family ipv4-unicast
    set protocols bgp neighbor 10.0.0.2 remote-as 65100
    set protocols bgp system-as 65000
    ```

=== "Mikrotik RouterOS"
    ```sh
    ip/address/add address=10.0.0.2/30 interface=ether1

    routing/bgp/connection/add as=65100 connect=yes local.role=ebgp name=ros-vyos \
        remote.address=10.0.0.1 .as=65000
    ```

Nun kann validiert werden, ob die BGP Session Established ist:

=== "VyOS"
    ```sh
    run show ip bgp summary
    ```
    ```
    IPv4 Unicast Summary (VRF default):
    BGP router identifier 10.0.0.1, local AS number 65000 vrf-id 0
    BGP table version 0
    RIB entries 0, using 0 bytes of memory
    Peers 1, using 20 KiB of memory

    Neighbor        V         AS   MsgRcvd   MsgSent   TblVer  InQ OutQ  Up/Down State/PfxRcd   PfxSnt Desc
    10.0.0.2        4      65100         3         3        0    0    0 00:00:17            0        0 N/A

    Total number of neighbors 1
    ```


=== "Mikrotik RouterOS"
    ```sh
    routing/bgp/session/print
    ```
    ```
    Flags: E - established 
    0 E name="ros-vyos-1" 
        remote.address=10.0.0.1 .as=65000 .id=10.0.0.1 .capabilities=mp,rr,em,gr,as4,ap,err,llgr,fqdn .messages=1 .bytes=23 
        .gr-time=120 .eor=ip 
        local.address=10.0.0.2 .as=65100 .id=10.0.0.2 .capabilities=mp,rr,gr,as4 .messages=1 .bytes=19 .eor="" 
        output.procid=20 
        input.procid=20 ebgp 
        hold-time=3m keepalive-time=1m uptime=6s460ms last-started=2023-10-03 09:42:43 prefix-count=0 

    ```

## Szenario #2

In diesem Szenario werden drei Router in einer Linie miteinander verbunden.  
Die äußeren Router (AS 65100 & AS 65200) announcen jeweils ein /24er IPv4 Präfix (10.1.100.0/24 & 10.1.200.0/24). 
Innerhalb von AS 65000 wurden BGP Importfilter implementiert, um sicherzustellen, dass keine falsche IPv4 Netze 
announct werden.

Die beiden äußeren Router verfügen über BGP Import- und Exportfilter, die verhindern das falsche Präfixe an AS 65000
announct werden und sicherstellen, dass nur Präfixe importiert werden die auf der Präfixliste stehen.

<figure markdown>
  ![](../assets/img/routing/bgp/scenario-2.drawio.svg){ loading=lazy width="600px" }
</figure>

=== "VyOS 1.3"

    ```sh
    set interfaces ethernet eth0 address '10.0.0.1/30'
    
    set protocols bgp 65200 neighbor 10.0.0.2 address-family ipv4-unicast
    set protocols bgp 65200 neighbor 10.0.0.2 remote-as 65000
    ```

    BGP Announcements:

    ```sh
    # good practise, but not required in vyos
    set protocols static route 10.1.200.0/24 blackhole
    set protocols bgp 65200 address-family ipv4-unicast network 10.1.200.0/24
    ```

    BGP Importfilter:

    ```sh
    ### Präfixliste für BGP Peering mit VyOS 1.5 ###
    # nur Präfix von RouterOS importieren
    set policy prefix-list bgp-in-vyos15 rule 10 action permit
    set policy prefix-list bgp-in-vyos15 rule 10 prefix 10.1.100.0/24

    # alle anderen Präfixe ablehnen
    set policy prefix-list bgp-in-vyos15 rule 99 action deny
    set policy prefix-list bgp-in-vyos15 rule 99 prefix 0.0.0.0/0
    set policy prefix-list bgp-in-vyos15 rule 99 le 32

    # nur eigenes Präfix exportieren
    set policy prefix-list bgp-out-vyos15 rule 10 action permit
    set policy prefix-list bgp-out-vyos15 rule 10 prefix 10.1.200.0/24

    # alle anderen Präfixe ablehnen
    set policy prefix-list bgp-out-vyos15 rule 99 action deny
    set policy prefix-list bgp-out-vyos15 rule 99 prefix 0.0.0.0/0
    set policy prefix-list bgp-out-vyos15 rule 99 le 32

    set protocols bgp neighbor 10.0.0.1 address-family prefix-list import bgp-in-vyos13
    ```

=== "VyOS 1.5"

    ```sh
    set interfaces ethernet eth0 address '10.0.0.2/30'
    set interfaces ethernet eth1 address '10.0.0.4/31'

    set protocols bgp neighbor 10.0.0.1 address-family ipv4-unicast
    set protocols bgp neighbor 10.0.0.1 remote-as 65200
    set protocols bgp neighbor 10.0.0.5 address-family ipv4-unicast
    set protocols bgp neighbor 10.0.0.5 remote-as 65100
    set protocols bgp system-as 65000
    ```

    BGP Importfilter:

    ```sh
    ### Präfixliste für BGP Peering mit RouterOS ###
    set policy prefix-list bgp-in-ros rule 10 action permit
    set policy prefix-list bgp-in-ros rule 10 prefix 10.1.100.0/24

    # alle anderen Präfixe ablehnen
    set policy prefix-list bgp-in-ros rule 99 action deny
    set policy prefix-list bgp-in-ros rule 99 prefix 0.0.0.0/0
    set policy prefix-list bgp-in-ros rule 99 le 32

    ### Präfixliste für BGP Peering mit VyOS 1.3 ###
    set policy prefix-list bgp-in-vyos13 rule 10 action permit
    set policy prefix-list bgp-in-vyos13 rule 10 prefix 10.1.200.0/24

    # alle anderen Präfixe ablehnen
    set policy prefix-list bgp-in-vyos13 rule 99 action deny
    set policy prefix-list bgp-in-vyos13 rule 99 prefix 0.0.0.0/0
    set policy prefix-list bgp-in-vyos13 rule 99 le 32

    set protocols bgp neighbor 10.0.0.5 address-family prefix-list import bgp-in-ros
    set protocols bgp neighbor 10.0.0.1 address-family prefix-list import bgp-in-vyos13
    ```

=== "Mikrotik RouterOS"

    ```sh
    ip/address/add address=10.0.0.5/31 interface=ether1

    routing/bgp/connection/add as=65100 connect=yes local.role=ebgp name=ros-vyos \
        remote.address=10.0.0.4 .as=65000
    ```

    BGP Announcements:

    ```sh
    ip/route/add blackhole dst-address=10.1.100.0/24
    ip/firewall/address-list/add address=10.1.100.0/24 list=bgp4

    routing/filter/rule/add chain=bgp-in rule=accept
    routing/bgp/connection/set 0 output.network=bgp4 input.filter=bgp-in
    ```

    BGP Import- und Exportfilter:

    ```sh
    #routing/filter/rule/add chain=bgp-in rule=accept   # TODO
    #routing/filter/rule/add chain=bgp-out rule=accept  # TODO
    routing/bgp/connection/set 0 output.network=bgp4 output.filter=bgp-out
    ```    

## Weiterführende Links
- [DE-CIX BGP Webinar Series](https://www.de-cix.net/en/resources/videos-and-webinars/bgp-webinar-series)
- [VyOS 1.3 BGP Dokumentation](https://docs.vyos.io/en/equuleus/configuration/protocols/bgp.html), [VyOS 1.3 Policy Dokumentation](https://docs.vyos.io/en/equuleus/configuration/policy/index.html)
- [Mikrotik RouterOS BGP Dokumentation](https://help.mikrotik.com/docs/display/ROS/BGP)
