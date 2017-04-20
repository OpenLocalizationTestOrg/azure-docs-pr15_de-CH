<properties
    pageTitle="VPN-Verbindungen in Azure API Management einrichten"
    description="Informationen Sie zum Einrichten einer VPN-Verbindungs in Azure API Management und Web-Dienste durch sie."
    services="api-management"
    documentationCenter=""
    authors="antonba"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="antonba"/>

# <a name="how-to-setup-vpn-connections-in-azure-api-management"></a>VPN-Verbindungen in Azure API Management einrichten

API Management VPN-Unterstützung können Sie Ihre API Management Gateway zu einer Azure Virtual Network (klassisch) verbinden. Dadurch API Management Kunden sicher auf Back-End-Webdienste, die lokal oder anderweitig mit dem öffentlichen Internet zugegriffen werden.

>[AZURE.NOTE] Azure API Management arbeitet mit klassischen VNETs. Informationen zum Erstellen einer klassischen VNET finden Sie unter [Erstellen eines virtuellen Netzwerks (classic) mithilfe der Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Informationen auf ARM-VNETS mit klassischen VNETs finden Sie unter [Verbinden klassische VNets auf neue VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="enable-vpn"> </a>Aktivieren Sie VPN-Verbindungen

>VPN-Konnektivität ist nur in Ebenen **Premium** und **Entwickler** verfügbar. Wechseln zu öffnen Sie API Management-Dienst in [Azure-Verwaltungsportal][] und dann die Registerkarte **Skalierung** . Wählen Sie im Abschnitt **Allgemein** Premium-Ebene und klicken Sie auf Speichern.

Aktivieren Sie VPN-Konnektivität öffnen Sie API Management-Dienst in [Azure-Verwaltungsportal][] , und wechseln Sie zur Registerkarte **Konfigurieren** . 

Wechseln Sie im Abschnitt VPN- **VPN-Verbindung** **auf**.

![Registerkarte API Management Instanz konfigurieren][api-management-setup-vpn-configure]

Sie sehen jetzt, eine Liste aller Bereiche, in denen der API Management-Dienst bereitgestellt wird.

Wählen Sie VPN und Subnetz für jede Region. Die Liste von VPNs wird aufgefüllt basierend auf verfügbaren virtuellen Netzwerke in der Azure-Abonnement, das Setup in den Bereich, den Sie konfigurieren.

![Wählen Sie VPN][api-management-setup-vpn-select]

Klicken Sie am unteren Bildschirmrand **Speichern** . Sie dürfen andere Vorgänge API-Verwaltungsdienst von Azure-Verwaltungsportal ausführen, während er aktualisiert wird. Service-Gateway stehen und Common Language Runtime ruft sollten nicht betroffen.

Beachten Sie, dass die VIP-Adresse des Gateways jedes Mal ändert, VPN aktiviert oder deaktiviert ist.

## <a name="connect-vpn"> </a>Mit einem Webdienst hinter VPN verbinden

Nach der API-Verwaltungsdienst mit dem VPN verbunden ist, unterscheidet Zugriff auf Webdienste innerhalb des virtuellen Netzwerks nicht öffentliche Dienste. Geben Sie einfach die lokale Adresse oder den Hostnamen des Web Service (wenn ein DNS-Server für Azure Virtual Network konfiguriert wurde) in das Feld **Webdienst-URL** beim Erstellen einer neuen API oder eine vorhandene bearbeiten.

![API mit VPN hinzufügen][api-management-setup-vpn-add-api]

## <a name="required-ports-for-api-management-vpn-support"></a>Erforderliche Anschlüsse für VPN-API Management-Unterstützung

Wenn eine API Management Service-Instanz in einer VNET gehostet wird, werden die Ports in der folgenden Tabelle verwendet. Wenn diese Ports blockiert sind, kann der Dienst nicht ordnungsgemäß. Eine oder mehrere dieser Ports blockiert ist das häufigste Fehlkonfiguration Problem Management-API mit einem VNET.

| Anschlüsse                      | Richtung        | Übertragungsprotokoll | Zweck                                                          | Quelle / Ziel              |
|------------------------------|------------------|--------------------|------------------------------------------------------------------|-----------------------------------|
| 80, 443                      | Eingehende          | TCP                | Clientkommunikation API-Management                           | INTERNET / VIRTUAL_NETWORK        |
| 80,443                       | Ausgehende         | TCP                | API-Management Abhängigkeit von Azure-Speicher und Azure Servicebus | VIRTUAL_NETWORK / INTERNET        |
| 1433                         | Ausgehende         | TCP                | API Management Abhängigkeiten von SQL                               | VIRTUAL_NETWORK / INTERNET        |
| 9350, 9351, 9352, 9353, 9354 | Ausgehende         | TCP                | API Management Service Bus Abhängigkeit                       | VIRTUAL_NETWORK / INTERNET        |
| 5671                         | Ausgehende         | AMQP               | API Management Abhängigkeit Log-Ereignis Hub-Richtlinie            | VIRTUAL_NETWORK / INTERNET        |
| 6381, 6382, 6383             | Eingehend/ausgehend | UDP                | Abhängigkeiten von Redis Cache-API-Management                       | VIRTUAL_NETWORK / VIRTUAL_NETWORK |
| 445                          | Ausgehende         | TCP                | API-Management Abhängigkeit von Azure Dateifreigabe für GIT            | VIRTUAL_NETWORK / INTERNET        |

## <a name="custom-dns"> </a>Benutzerdefinierte DNS-Server einrichten

API Management hängt eine Anzahl von Azure Services. Wenn eine API Management Service-Instanz in einer VNET gehostet wird, in ein benutzerdefinierter DNS-Server verwendet wird, ist Hostnamen Azure Dienste beheben. Folgen Sie [dieser](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) Anleitung benutzerdefinierte DNS-Installation ein.  

## <a name="related-content"> </a>Inhalte


* [Erstellen Sie ein virtuelles Netzwerk mit einer Standort-zu-Standort-VPN-Verbindung mithilfe der Azure-Verwaltungsportal][]
* [Verwendung der API-Inspektor verfolgen ruft in Azure API Management][]

[api-management-setup-vpn-configure]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png
[api-management-setup-vpn-select]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[Azure-Verwaltungsportal]: https://manage.windowsazure.com/

[Erstellen Sie ein virtuelles Netzwerk mit einer Standort-zu-Standort-VPN-Verbindung mithilfe der Azure-Verwaltungsportal]: ../vpn-gateway/vpn-gateway-site-to-site-create.md
[Verwendung der API-Inspektor verfolgen ruft in Azure API Management]: api-management-howto-api-inspector.md
