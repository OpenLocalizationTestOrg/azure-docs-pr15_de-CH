
<properties
    pageTitle="Problembehandlung beim Erstellen von RemoteApp Hybrid Sammlungen | Microsoft Azure"
    description="Erfahren Sie, wie RemoteApp Hybrid Auflistung erstellen Fehler beheben"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a>Problembehandlung beim Erstellen von Azure RemoteApp Hybrid-Sammlungen

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Hybrid-Sammlung wird und speichert Daten in der Azure-Cloud aber auch können Benutzer Zugriff auf Daten und Ressourcen im lokalen Netzwerk. Benutzer können sich mit ihren Unternehmensanmeldeinformationen synchronisiert oder Verbund mit Azure Active Directory apps zugreifen. Eine Hybrid-Auflistung, die eine vorhandene Azure virtuelle Netzwerk bereitstellen oder ein neues virtuelles Netzwerk erstellen. Wir empfehlen Sie erstellen oder ein virtuelles Netzwerk-Subnetz CIDR Bereich groß genug für erwartete Wachstum für Azure RemoteApp.

Werden Ihre Sammlung noch noch nicht erstellt? Die Schritte finden Sie unter [Erstellen einer hybridsammlung](remoteapp-create-hybrid-deployment.md) .

Wenn Sie Probleme beim Erstellen einer Sammlung oder wenn die Auflistung nicht so funktioniert Sie denken sollte, überprüfen Sie die folgenden Informationen.

## <a name="your-image-is-invalid"></a>Das Bild ist ungültig ##
Wenn eine Meldung wie "GoldImageInvalid" beim Warten Azure Bereitstellung Ihrer Sammlung, bedeutet dies, dass Ihre Vorlagenbild [Bild Anforderungen](remoteapp-imagereqs.md)entspricht. Also diese [Vorschriften](remoteapp-imagereqs.md)lesen, Ihr Bild korrigieren und versuchen, Ihre Sammlung zu erstellen.



## <a name="does-your-vnet-have-network-security-groups-defined"></a>Verfügt Ihr VNET netzwerksicherheitsgruppen definiert? ##
Haben Sie Network Security Gruppen im Subnetz für die Auflistung verwenden, stellen Sie sicher, diese [URLs und Ports](remoteapp-ports.md) sind über Ihr Subnetz.

Sie können die VMs bereitgestellt, indem Sie im Subnetz nach Kontrolle zusätzliche Sicherheitsgruppen hinzufügen.

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a>Verwenden Sie eigene DNS-Server? Und VNET Subnetz zugreifen? ##
>[AZURE.NOTE] Sie müssen sicherstellen, dass die DNS-Server in Ihrem VNET, immer und immer in der VNET gehosteten virtuellen Computer auflösen können. Verwenden Sie nicht Google DNS.


Hybrid-Sammlungen verwenden Sie eigene DNS-Server. Soll sie in Ihrem Netzwerk Konfigurationsschema oder über das Verwaltungsportal als virtuelle Netzwerk erstellen. DNS-Server werden in der Reihenfolge verwendet, dass sie auf Failover (im Gegensatz zu Round Robin) angegeben werden.  
[Auflösung für VMs und Instanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) , stellen Sie sicher, dass die DNS-Server konfigurierten Hintergrundsuche finden.

Stellen Sie sicher, die DNS-Server für die Auflistung zugegriffen werden und über das Subnetz VNET Sie für diese Auflistung.

Zum Beispiel:

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![Definieren von DNS](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a>Verwenden Sie Active Directory-Domänencontroller in Ihrer Sammlung? ##
Derzeit nur eine Active Directory-Domäne Azure RemoteApp zugeordnet werden kann. Die Hybrid-Auflistung unterstützt nur Azure Active Directory-Konten, die mit Dirsync-Tool von einer Windows Server Active Directory synchronisiert wurden. insbesondere oder synchronisiert mit Kennwortsynchronisation synchronisiert mit Active Directory-Verbunddienste (AD FS) konfiguriert. Sie müssen eine benutzerdefinierte Domäne, die das Domänensuffix UPN der lokalen Domäne entspricht und Verzeichnisintegration.

Weitere Informationen finden Sie unter [Konfigurieren von Active Directory für Azure RemoteApp](remoteapp-ad.md) .

Stellen Sie sicher Domäne Angaben sind gültig und des Domänencontrollers erreichbar VM in Azure RemoteApp zum Subnetz erstellt. Achten Sie auch Berechtigungen zum Hinzufügen von Computern zur angegebenen Domäne Dienstkonteninformationen angegeben und der AD-Name gemäß der VNET DNS aufgelöst werden kann.

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a>Welchen Domänennamen haben Sie angegeben, wenn Sie Ihre Sammlung erstellt? ##

Der Domänennamen erstellt oder hinzugefügt muss einen internen Domänennamen (nicht Azure AD Domänenname) und muß auflösbaren DNS-Format (contoso.local). Beispielsweise Sie haben internen Active Directory-Namen (contoso.local) und ein Active Directory-UPN (contoso.com) - Sie können den internen Namen der Sammlung.
