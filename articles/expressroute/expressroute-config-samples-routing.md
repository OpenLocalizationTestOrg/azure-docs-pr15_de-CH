<properties
   pageTitle="Proben von ExpressRoute Kunden Router Konfiguration | Microsoft Azure"
   description="Diese Seite enthält Router Config Beispiele für Cisco und Juniper-Routern."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="router-configuration-samples-to-setup-and-manage-routing"></a>Router-Konfiguration Beispiele einrichten und Verwalten des Routings

Diese Seite enthält Schnittstelle und routing Konfiguration Beispiele für Cisco IOS-XE und Juniper MX-Serie Router. Diese Beispiele dienen nur zu dienen und müssen nicht verwendet werden. Sie können den Anbieter mit geeigneten Konfigurationen für Ihr Netzwerk arbeiten. 

>[AZURE.IMPORTANT] Beispiele auf dieser Seite sollen lediglich zur Orientierung. Sie müssen Ihr Netzwerkteam mit geeigneten Konfigurationen an Ihre Bedürfnisse mit Vertrieb und technische Team des Kreditors arbeiten. Microsoft unterstützt keine Fragen auf dieser Seite aufgeführten Konfigurationen. Bei Problemen müssen Sie den Gerätehersteller wenden.

Router Konfiguration folgenden Beispiele gelten für alle Peerings. Überprüfen Sie [ExpressRoute Peerings](expressroute-circuit-peerings.md) und [ExpressRoute Routinganforderungen](expressroute-routing.md) ausführliche routing.

## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS-XE-basierten Router

Die Beispiele in diesem Abschnitt gelten für alle Router unter IOS XE-Betriebssystemfamilie.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. Konfigurieren von Schnittstellen und Sub-Interfaces

Sie benötigen eine Sub-Schnittstelle pro peering in jedem Router die Verbindung an Microsoft. Eine Sub-Schnittstelle kann mit einer VLAN-ID oder eine gestapelte VLAN-IDs und eine IP-Adresse identifiziert werden.

#### <a name="dot1q-interface-definition"></a>Definition der Dot1Q-Schnittstelle

Dieses Beispiel stellt die untergeordneten Schnittstellendefinition für eine Unterschnittstelle mit einer einzigen VLAN-ID Die VLAN-ID ist eindeutig pro peering. Das letzte Oktett der IPv4-Adresse werden immer eine ungerade Zahl.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

#### <a name="qinq-interface-definition"></a>QinQ Schnittstellendefinition

Dieses Beispiel stellt die untergeordneten Schnittstellendefinition für eine Unterschnittstelle mit zwei VLAN-IDs. Äußere VLAN-ID (s-Tag), wenn bleibt in allen Peerings. Innere VLAN-ID (c-Tag) ist pro peering eindeutig. Das letzte Oktett der IPv4-Adresse werden immer eine ungerade Zahl.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>
    
### <a name="2-setting-up-ebgp-sessions"></a>2. einrichten eBGP sessions

Richten Sie eine Sitzung BGP mit Microsoft für jede peering. Im folgenden Beispiel können Sie eine Sitzung BGP mit Microsoft setup. Wurde die IPv4-Adresse für die Sub-Schnittstelle a.b.c.d, werden die IP-Adresse des Nachbarn BGP (Microsoft) a.b.c.d+1. Das letzte Oktett BGP Nachbarn IPv4-Adresse wird immer eine gerade Zahl sein.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. einrichten Präfixe über BGP-Sitzung angekündigt wird.

Sie können Microsoft select Präfixe ankündigen den Router konfigurieren. Sie können mit dem folgenden Beispiel tun.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. Route zugeordnet ist.

Karten verwenden und Präfix enthält Filter-Präfixen in das Netzwerk übertragen. Im folgenden Beispiel können Sie die Aufgabe. Sicherstellen Sie, dass Sie entsprechende Präfix Listen eingerichtet.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>Juniper MX Serie Router 

Die Beispiele in diesem Abschnitt gelten für Juniper MX-Serie Router.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. Konfigurieren von Schnittstellen und Sub-Interfaces

#### <a name="dot1q-interface-definition"></a>Definition der Dot1Q-Schnittstelle

Dieses Beispiel stellt die untergeordneten Schnittstellendefinition für eine Unterschnittstelle mit einer einzigen VLAN-ID Die VLAN-ID ist eindeutig pro peering. Das letzte Oktett der IPv4-Adresse werden immer eine ungerade Zahl.

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


#### <a name="qinq-interface-definition"></a>QinQ Schnittstellendefinition

Dieses Beispiel stellt die untergeordneten Schnittstellendefinition für eine Unterschnittstelle mit zwei VLAN-IDs. Äußere VLAN-ID (s-Tag), wenn bleibt in allen Peerings. Innere VLAN-ID (c-Tag) ist pro peering eindeutig. Das letzte Oktett der IPv4-Adresse werden immer eine ungerade Zahl.

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2. einrichten eBGP sessions

Richten Sie eine Sitzung BGP mit Microsoft für jede peering. Im folgenden Beispiel können Sie eine Sitzung BGP mit Microsoft setup. Wurde die IPv4-Adresse für die Sub-Schnittstelle a.b.c.d, werden die IP-Adresse des Nachbarn BGP (Microsoft) a.b.c.d+1. Das letzte Oktett BGP Nachbarn IPv4-Adresse wird immer eine gerade Zahl sein.

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. einrichten Präfixe über BGP-Sitzung angekündigt wird.

Sie können Microsoft select Präfixe ankündigen den Router konfigurieren. Sie können mit dem folgenden Beispiel tun.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4. Route zugeordnet ist.

Karten verwenden und Präfix enthält Filter-Präfixen in das Netzwerk übertragen. Im folgenden Beispiel können Sie die Aufgabe. Sicherstellen Sie, dass Sie entsprechende Präfix Listen eingerichtet.

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>Nächste Schritte

[ExpressRoute FAQ](expressroute-faqs.md) für mehr Details anzeigen
