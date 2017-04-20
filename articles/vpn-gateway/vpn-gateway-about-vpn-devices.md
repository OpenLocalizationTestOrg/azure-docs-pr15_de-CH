<properties 
   pageTitle="VPN-Geräte für Standort-zu-Standort-VPN-Gateway Verbindungen für virtuelle Netzwerke Azure | Microsoft Azure"
   description="Dieser Artikel beschreibt VPN-Geräte und IPSec-Parameter für Verbindung S2S VPN-Gateway und enthält Links zu Konfigurationshinweise und Beispiele."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
  tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/13/2016"
   ms.author="yushwang;cherylmc" />

# <a name="about-vpn-devices-for-site-to-site-vpn-gateway-connections"></a>VPN-Geräte für Standort-zu-Standort-VPN-Gateway-Verbindung

Ein VPN-Gerät muss eine Standort-zu-Standort (S2S) VPN-Verbindung. Standort-zu-Standort-Verbindung wird eine Hybrid-Lösung erstellen, oder jederzeit eine sichere Verbindung zwischen dem lokalen Netzwerk und virtuelle Netzwerk. Dieser Artikel beschreibt VPN-Geräte und Konfigurationsparameter. 

>[AZURE.NOTE] Beim Konfigurieren einer Standort-zu-Standort-Verbindung muss eine öffentliche IPv4-Adresse für das VPN-Gerät.                                                                                                                                                                               

Wenn Ihr Gerät in der Tabelle [überprüft VPN-Geräten](#devicetable) angezeigt wird, finden Sie unter Abschnitt [nicht geprüfte VPN-Geräte](#additionaldevices) . Es ist möglich, dass Ihr Gerät mit Azure weiterhin funktionieren. VPN-Unterstützung erhalten Sie von Ihrem Gerätehersteller.

**Beachten Sie beim Anzeigen der Tabellen Elemente:**

- Terminologie für statische und dynamische routing geändert wurde. Sie werden wahrscheinlich in beiden Begriffe ausgeführt. Es ist keine Änderung der Funktionalität, die Namen verändern.
    - Statisches Routing = Richtlinien
    - Dynamisches Routing = RouteBased
- Spezifikationen für hohe Leistung VPN-Gateway und RouteBased VPN-Gateways entsprechen, sofern nichts anderes angegeben ist. Überprüfte VPN-Geräte mit RouteBased VPN-Gateways sind z. B. auch mit Azure hohe Leistung VPN-Gateway kompatibel. 


## <a name="devicetable"></a>Überprüfte VPN-Geräte 

Wir haben eine Reihe von standard-VPN-Geräte in Partnerschaft mit Gerätehersteller überprüft. Azure VPN-Gateways müssen die Geräte in der folgenden Liste enthaltenen gerätefamilien arbeiten. Finden Sie [Über VPN-Gateway](vpn-gateway-about-vpngateways.md) , Gateway-Typ überprüfen, die Sie für die Projektmappe erstellen, die Sie konfigurieren möchten. 

Der VPN-Konfiguration unterstützen, finden Sie unter Links, die entsprechende Produktfamilie entsprechen. VPN-Unterstützung erhalten Sie von Ihrem Gerätehersteller.



| **Kreditor**                      | **Familie**                                        | **Minimale Version des Betriebssystems**                             | **Richtlinien**                                                                                                                                                                                                             | **RouteBased**                                                                                                                                                                    |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Allied Telesis                  | AR-Serie VPN-Router                                    | 2.9.2                                              | Demnächst                                                                                                                                                                                                                                          | Nicht kompatibel                                                                                                                                                                                               |
| Barracuda Networks, Inc.        | Barracuda NextGen Firewall F-Serie             | Richtlinien: 5.4.3 RouteBased: 6.2.0  | [Hinweise zur Konfiguration](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) | [Hinweise zur Konfiguration](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW)                                                                                                                                                                                              |
| Barracuda Networks, Inc.        |  Barracuda NextGen Firewall X-Serie                 | Barracuda Firewall 6.5 | [Barracuda Firewall](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | Nicht kompatibel                                                                                                                                                                                               |
| Brocade                         | Vyatta 5400 vRouter                                      | Virtueller Router 6.6R3 GA                            | [Hinweise zur Konfiguration](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html)                                       | Nicht kompatibel                                                                                                                                                                                               |
| Check Point                     | Sicherheitsgateway                                         | R75.40 R75.40VS                                     | [Hinweise zur Konfiguration](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275)                                         | [Hinweise zur Konfiguration](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco                           | ASA                                                      | 8.3                                                | [Cisco-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA)                                                                                                                                                                        | Nicht kompatibel                                                                                                                                                                                               |
| Cisco                           | ASR                                                      | IOS 15.1 (Richtlinien) IOS 15.2 (RouteBased)                | [Cisco-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                                                        | [Cisco-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                 |
| Cisco                           | ISR                                                      | IOS 15,0 (Richtlinien) IOS 15.1 (RouteBased *)               | [Cisco-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                                                        | [Cisco Beispiele *](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                |
| Citrix                          | NetScaler MPX SDX, VPX      |10.1 und höher                                           | [Integration Anleitung](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html)                                                                                                                                                                            | Nicht kompatibel                                                                                                                                                                                               |
| Dell SonicWALL                  | TZ Serie NSA Serie, SuperMassive, E-Klasse NSA | SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646 )          | [Anleitung - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Anleitung - SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                   | [Anleitung - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Anleitung - SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                                                                      |
| F5                              | BIG-IP-Serie                                 |           12.0                                            | [Hinweise zur Konfiguration](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip)                                                                                                                                                                          | [Hinweise zur Konfiguration](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling)                                                                                                                                                                                         |
| Fortinet                        | FortiGate                                                | FortiOS 5.2.7                                      | [Hinweise zur Konfiguration](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                                                          | [Hinweise zur Konfiguration](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                  |
| Internet Initiative Japan (IIJ) | Ende-Serie                                              | Ende / X 4,60 Ende-B1 4,60 3,20 SEIL/X86            | [Hinweise zur Konfiguration](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf)                                                                                                                                                                   | Nicht kompatibel                                                                                                                                                                                               |
| Wacholder                         | SRX                                                      | JunOS 10.2 (Richtlinien) JunOS 11.4 (RouteBased)            | [Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                                                                      | [Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                              |
| Wacholder                         | J-Baureihe                                                 | JunOS 10.4r9 (Richtlinien) JunOS 11.4 (RouteBased)          | [Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                                                                 | [Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                         |
| Wacholder                         | ISG                                                      | ScreenOS 6.3 (Richtlinien und RouteBased)                  | [Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                                                                      | [Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                              |
| Wacholder                         | SSG                                                      | ScreenOS 6.2 (Richtlinien und RouteBased)                  | [Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                                                                      | [Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                              |
| Microsoft                       | Routing und RAS-Dienst                        | Windows Server 2012                                | Nicht kompatibel                                                                                                                                                                                                                                       | [Microsoft-Beispiele](http://go.microsoft.com/fwlink/p/?LinkId=717761)                                                                                           |
| Open Systems AG | Mission-Steuerelements Sicherheitsgateway | N/A | [Installationshandbuch](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) | [Installationshandbuch](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan                        | Openswan                                                 | 2.6.32                                             | (In Kürze verfügbar)                                                                                                                                                                                                                                        | Nicht kompatibel                                                                                                                                                                                               |
| Palo Alto Networks              | Alle Geräte PAN-Betriebssystem     | PAN-OS 6.1.5 oder höher (Richtlinien), PAN-OS 7.0.5 oder höher (RouteBased)       | [Hinweise zur Konfiguration]( https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065)                                                                                                                                                                                          | [Hinweise zur Konfiguration](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340)                                                                                                                                                                                    |
| WatchGuard                      | Alle                                                      | Fireware XTM v11.x                                 | [Hinweise zur Konfiguration](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/)                                                                                                                                                                          | Nicht kompatibel                                                                                                                                                                                               |

(*) ISR 7200 Serie Router unterstützen nur Richtlinien VPNs.

## <a name="additionaldevices"></a>VPN-Geräte nicht validiert

Wenn Sie das Gerät gemäß der Tabelle überprüft VPN-Geräte sehen, kann dennoch eine Standort-zu-Standort-Verbindung funktioniert. Stellen Sie sicher, dass VPN-Gerät im Abschnitt Gateway Vorschriften [Über VPN-Gateways](vpn-gateway-about-vpngateways.md#gateway-requirements) Artikel beschriebenen Mindestanforderungen erfüllt. Geräte die Mindestanforderungen sollten auch mit VPN-Gateways arbeiten. Wenden Sie sich an den Gerätehersteller Hinweise zusätzlicher Support und Konfiguration.


## <a name="editing-device-configuration-samples"></a>Gerätebeispiele Konfiguration bearbeiten

Nach dem Herunterladen der bereitgestellten VPN-Gerät Konfigurationsbeispiel müssen Sie einige Werte, um die Einstellungen für Ihre Umgebung zu ersetzen. 

**So bearbeiten Sie ein Beispiel:**

1. Öffnen Sie mit Notepad. 
1. Suchen Sie und Ersetzen Sie alle <*Text*> Zeichenfolgen mit den Werten, die für Ihre Umgebung beziehen. Geben Sie < und >. Wenn ein Name angegeben wird, sollte der gewählte Name eindeutig sein. Wenn ein Befehl nicht funktioniert, finden Sie in der Gerätedokumentation des Herstellers.

| **Beispieltext**                | **Ändern in**                                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP_OnPremisesNetwork&gt;           | Der gewählte Name für dieses Objekt. Beispiel: MyOnPremisesNetwork                                                       |
| &lt;RP_AzureNetwork&gt;                | Der gewählte Name für dieses Objekt. Beispiel: MyAzureNetwork                                                            |
| &lt;RP_AccessList&gt;                  | Der gewählte Name für dieses Objekt. Beispiel: MyAzureAccessList                                                         |
| &lt;RP_IPSecTransformSet&gt;           | Der gewählte Name für dieses Objekt. Beispiel: MyIPSecTransformSet                                                       |
| &lt;RP_IPSecCryptoMap&gt;              | Der gewählte Name für dieses Objekt. Beispiel: MyIPSecCryptoMap                                                          |
| &lt;SP_AzureNetworkIpRange&gt;         | Geben Sie an. Beispiel: 192.168.0.0                                                                                  |
| &lt;SP_AzureNetworkSubnetMask&gt;      | Geben Sie die Subnetzmaske. Beispiel: 255.255.0.0                                                                            |
| &lt;SP_OnPremisesNetworkIpRange&gt;    | Geben Sie lokal an. Beispiel: 10.2.1.0                                                                         |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; | Lokale Subnetzmaske angeben. Beispiel: 255.255.255.0                                                              |
| &lt;SP_AzureGatewayIpAddress&gt;       | Diese Informationen zu virtuellen Netzwerk und befindet sich im Verwaltungsportal als **Gateway-IP-Adresse**. |
| &lt;SP_PresharedKey&gt;                | Diese Informationen ist spezifisch für das virtuelle Netzwerk und befindet sich im Verwaltungsportal als Schlüssel verwalten.          |



## <a name="ipsec-parameters"></a>IPSec-Parameter

>[AZURE.NOTE] Obwohl in der folgenden Tabelle aufgeführten Werte von Azure VPN-Gateway unterstützt werden, besteht derzeit keine Möglichkeit geben oder wählen Sie eine bestimmte Kombination von Azure VPN-Gateway. Geben Sie Einschränkungen aus lokalen VPN-Gerät. Darüber hinaus müssen Sie MSS auf 1350 klemmen.

### <a name="ike-phase-1-setup"></a>IKE Phase 1-setup

| **Eigenschaft**                                       | **Richtlinien** | **RouteBased und Standard oder hohe Leistung VPN-gateway** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| IKE-Version                                        | IKEv1                          | IKEv2                                                            |
| Diffie-Hellman-Gruppe                               | Gruppe 2 (1024 Bit)             | Gruppe 2 (1024 Bit)                                               |
| Authentifizierungsmethode                              | Vorinstallierter Schlüssel                 | Vorinstallierter Schlüssel                                                   |
| Verschlüsselungsalgorithmen                              | AES256 AES128 3DES             | AES256 3DES                                                      |
| Hashing-Algorithmus                                  | SHA1(SHA128)                   | SHA1(SHA128) SHA2(SHA256)                                                     |
| Phase 1 Security Association (SA) Lebensdauer (Zeit) | 28.800 Sekunden                 | 10.800 Sekunden                                                   |


### <a name="ike-phase-2-setup"></a>IKE Phase 2 setup

| **Eigenschaft**                                                             | **Richtlinien**                 | **RouteBased und Standard oder hohe Leistung VPN-gateway**   |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| IKE-Version                                                              | IKEv1                                          | IKEv2                                                              |
| Hashing-Algorithmus                                                        | SHA1(SHA128)                                   | SHA1(SHA128)                                                       |
| Phase 2 Security Association (SA) Lebensdauer (Zeit)                        | 3.600 Sekunden                                  | 3.600 Sekunden                                                                  |
| Phase 2 Security Association (SA) Lebensdauer (Durchsatz)                  | 102,400,000 KB                                 | -                                                                  |
| IPSec-SA-Verschlüsselung und Authentifizierung bietet (Rangfolge) | 1. 2 ESP-AES256. ESP-AES128 3. ESP-3DES 4. N/A | Sie *RouteBased-Gateway IPsec Security Association (SA) bietet* (siehe unten) |
| Perfect Forward Secrecy (PFS)                                            | Nein                                             | Keine (*)|
| Dead Peer-Erkennung                                                      | Nicht unterstützt                                  | Unterstützt                                                          |

(*) Azure Gateway als IKE-Antwort akzeptiert PFS DH-Gruppe 1, 2, 5, 14, 24.

### <a name="routebased-gateway-ipsec-security-association-sa-offers"></a>Bietet RouteBased Gateway IPsec Security Association (SA)

Die folgende Tabelle listet IPSec-SA-Verschlüsselung und Authentifizierung bietet. Angebote finden Sie die Reihenfolge, dass das Angebot oder zugelassen ist.

| **IPSec-SA-Verschlüsselung und Authentifizierung bietet** | **Azure Gateway als initiator**                               | **Azure Gateway als responder**                                   |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1                                                 | ESP AES_256 SHA                                              | ESP AES_128 SHA                                              |
| 2                                                 | ESP AES_128 SHA                                              | ESP 3_DES MD5                                                |
| 3                                                 | ESP 3_DES MD5                                                | ESP 3_DES SHA                                                |
| 4                                                 | ESP 3_DES SHA                                                | AH SHA1 mit ESP-AES_128 mit null HMAC                      |
| 5                                                 | AH SHA1 mit ESP AES_256 mit null HMAC                      | AH SHA1 mit ESP 3_DES mit null HMAC                        |
| 6                                                 | AH SHA1 mit ESP-AES_128 mit null HMAC                      | AH MD5 mit ESP 3_DES mit null HMAC keine Gültigkeitsdauer vorgeschlagen |
| 7                                                 | AH SHA1 mit ESP 3_DES mit null HMAC                        | AH SHA1 mit ESP 3_DES SHA1 keine Gültigkeitsdauer                    |
| 8                                                 | AH MD5 mit ESP 3_DES mit null HMAC keine Gültigkeitsdauer vorgeschlagen | AH MD5 mit ESP 3_DES MD5 keine Gültigkeitsdauer                     |
| 9                                                 | AH SHA1 mit ESP 3_DES SHA1 keine Gültigkeitsdauer                    | ESP DES MD5                                                  |
| 10                                                | AH MD5 mit ESP 3_DES MD5 keine Gültigkeitsdauer                     | ESP DES SHA1 keine Gültigkeitsdauer                                   |
| 11                                                | ESP DES MD5                                                  | AH vorgeschlagen mit DES ESP null HMAC SHA1 keine Gültigkeitsdauer        |
| 12                                                | ESP DES SHA1 keine Gültigkeitsdauer                                   | AH vorgeschlagen MD5 mit ESP DES null-HMAC keine Gültigkeitsdauer        |
| 13                                                | AH vorgeschlagen mit DES ESP null HMAC SHA1 keine Gültigkeitsdauer        | AH SHA1 mit ESP DES SHA1 keine Gültigkeitsdauer                      |
| 14                                                | AH vorgeschlagen MD5 mit ESP DES null-HMAC keine Gültigkeitsdauer        | AH MD5 mit ESP DES MD5 keine Gültigkeitsdauer                       |
| 15                                                | AH SHA1 mit ESP DES SHA1 keine Gültigkeitsdauer                      | ESP SHA keine Gültigkeitsdauer                                        |
| 16                                                | AH MD5 mit ESP DES MD5 keine Gültigkeitsdauer                       | ESP MD5 keine Gültigkeitsdauer                                        |
| 17                                                | -                                                            | AH SHA keine Gültigkeitsdauer                                         |
| 18                                                | -                                                            | AH MD5 keine Gültigkeitsdauer                                        |


- Sie können IPsec ESP NULL-Verschlüsselung mit RouteBased und hohe Leistung VPN-Gateways. NULL serverbasierte Verschlüsselung bietet keinen Schutz für Daten bei der Übertragung und sollte nur verwendet werden, wenn der maximale Durchsatz und minimale Latenz ist erforderlich.  Clients können Hiermit Kommunikationsszenarios VNet VNet oder wenn Verschlüsselung an anderer Stelle in der Lösung angewendet wird.

- Standortübergreifende Verbindung über das Internet verwenden Sie standardmäßig Azure VPN-Gateway-Verschlüsselung und Sicherheit Ihrer kritischen Kommunikation in der Tabelle oben aufgeführten Hashalgorithmen.





