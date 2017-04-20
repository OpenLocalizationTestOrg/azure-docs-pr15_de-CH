<properties
   pageTitle="Sichern ein Service Fabric-Clusters | Microsoft Azure"
   description="Beschreibt die Szenarien Security Service Fabric-Cluster und die andere zur Implementierung dieser Szenarios."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/19/2016"
   ms.author="chackdan"/>

# <a name="service-fabric-cluster-security-scenarios"></a>Service Fabric-Cluster Sicherheitsszenarios

Ein Cluster Service Fabric ist eine Ressource, die Sie besitzen. Cluster sollten immer gesichert werden, um verhindern unbefugte Cluster, besonders wenn Produktionsarbeitslasten ausgeführt hat. Zwar kann ein unsicheren Cluster erstellen können dadurch so anonymen Benutzern eine Verbindung her, wenn Management-Endpunkte an das öffentliche Internet verfügbar macht. 

Dieser Artikel bietet eine Übersicht über Sicherheitsszenarios für Cluster unter Azure oder eigenständige und verschiedene Technologie zur Implementierung dieser Szenarios. Sicherheit-Clusterkonfigurationen sind:

- Knoten-Sicherheit
- Client-zu-Knoten-Sicherheit
- Rollenbasierte Zugriffskontrolle (RBAC)

## <a name="node-to-node-security"></a>Knoten-Sicherheit
Sichert die Kommunikation zwischen den virtuellen Computern oder Computern im Cluster. Dies gewährleistet, dass nur Computer den Cluster beitreten, Programme und Dienste im Cluster teilnehmen können.

![Diagramm der Kommunikation von Knoten zu Knoten][Node-to-Node]

Cluster unter unter Windows Azure oder eigenständige Cluster können [Zertifikatschutz](https://msdn.microsoft.com/library/ff649801.aspx) oder [Windows-Sicherheit](https://msdn.microsoft.com/library/ff649396.aspx) für Windows Server-Computer.
### <a name="node-to-node-certificate-security"></a>Zertifikatschutz Knoten
Service Fabric verwendet x. 509-Serverzertifikate, die Sie als Teil der Knotentyp Konfigurationen angeben, wenn Sie einen Cluster erstellen. Überblick was diese Zertifikate sind und wie Sie abrufen oder sie erstellen erfolgt am Ende dieses Artikels.

Zertifikatschutz wird beim Erstellen des Clusters Azure-Portal, Azure-Ressourcen-Manager Vorlagen oder eine eigenständige JSON-Vorlage konfiguriert. Sie können eine primäre Zertifikat und eine optionale sekundäre, das Zertifikat Rollover verwendet wird. Angegebenen primären und sekundären Zertifikate sollte sich Verwaltungsclients und schreibgeschützte Clientzertifikate für [Client-zu-Knoten-Sicherheit](#client-to-node-security)angeben.

Azure finden Sie Informationen zum zertifikatsicherheit in einem Cluster konfigurieren [Einrichten eines Clusters mithilfe einer Vorlage Azure-Ressourcen-Manager](service-fabric-cluster-creation-via-arm.md) .

Lesen für eigenständige Windows Server [sichere eigenständige Cluster mit x. 509-Zertifikate unter Windows](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Knoten-Windows-Sicherheit
Lesen für eigenständige Windows Server [Sichern Sie eigenständige Cluster mit Windows-Sicherheit unter Windows](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Client-zu-Knoten-Sicherheit
Authentifiziert Clients und sichert die Kommunikation zwischen einem Client und einzelne Knoten im Cluster. Diese Art der Sicherheit authentifiziert und sichert die Client-Kommunikation, der sicherstellt, dass nur autorisierte Benutzer zugreifen können, der Cluster und die Anwendung auf dem Cluster bereitgestellt. Clients werden durch ihre Windows-Anmeldeinformationen oder ihre Zertifikatanmeldeinformationen eindeutig identifiziert.

![Diagramm der Kommunikation von Client zu Knoten][Client-to-Node]

Cluster unter Windows Azure oder eigenständige Cluster unter können [Zertifikatschutz](https://msdn.microsoft.com/library/ff649801.aspx) oder [Windows-Sicherheit](https://msdn.microsoft.com/library/ff649396.aspx)verwenden.

### <a name="client-to-node-certificate-security"></a>Client-zu-Knoten-zertifikatsicherheit
 Client-zu-Knoten-zertifikatsicherheit ist beim Erstellen des Clusters über Azure-Portal Ressourcenmanager Vorlagen oder eine eigenständige JSON-Vorlage durch Angabe einer Admin-Clientzertifikat oder ein Benutzerzertifikat Client konfiguriert.  Admin Client- und Clientzertifikate angegebenen sollte anders als die primären und sekundären Zertifikate aus Sicherheitsgründen [Knoten](#node-to-node-security)angeben.

Clients verbinden mit Admin-Zertifikat haben vollen Zugriff auf Verwaltungsfunktionen.  Clients verbinden mit Clientzertifikat schreibgeschützt Benutzer haben Lesezugriff auf Verwaltungsfunktionen. Mit anderen Worten sind diese Zertifikate für die Rolle Basen Zugriffskontrolle (RBAC) in diesem Artikel beschriebenen verwendet.

Azure finden Sie Informationen zum zertifikatsicherheit in einem Cluster konfigurieren [Einrichten eines Clusters mithilfe einer Vorlage Azure-Ressourcen-Manager](service-fabric-cluster-creation-via-arm.md) .

Lesen für eigenständige Windows Server [sichere eigenständige Cluster mit x. 509-Zertifikate unter Windows](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Client-zu-Knoten-Azure Active Directory (AAD) Sicherheit für Azure
Auf Windows Azure ausgeführte Cluster können auch Zugriff auf die Management-Endpunkte mithilfe von Azure Active Directory (AAD) sichern. Informationen werden bei der Clustererstellung Auffüllen erforderlich AAD Artefakte zu erstellen und anschließend die Cluster Verbindung finden Sie unter [Einrichten eines Clusters durch mithilfe einer Vorlage Azure-Ressourcen-Manager](service-fabric-cluster-creation-via-arm.md) .

## <a name="security-recommendations"></a>Sicherheitsaspekte
Azure Clustern wird empfohlen, dass AAD Sicherheit zur Authentifizierung von Clients und Zertifikate für Knoten-Sicherheit verwenden.

Für eigenständige Windows Server verwalteten Cluster empfohlen wird die Verwendung von Windows-Sicherheit mit Konten (GMA) haben Sie Windows Server 2012 R2 und Active Directory. Sonst noch verwenden Sie Windows-Sicherheit mit Windows-Konten.

## <a name="role-based-access-control-rbac"></a>Rollenbasierte Zugriffskontrolle (RBAC)
Zugriffskontrolle kann Clusteradministrator bestimmte Clustervorgänge für verschiedene Benutzergruppen, Sicherheit von Cluster begrenzen. Zwei verschiedenen Steuerelementtypen mit einem Cluster unterstützt: Administratorrolle und Benutzerrolle.

Administratoren haben vollen Zugriff auf Management-Funktionen (einschließlich Schreib-/Lesezugriff). Benutzer haben standardmäßig nur Lesezugriff auf Management-Funktionen (z. B. Abfragefunktionen) und auch aufgelöst.

Soll die Administrator- und Client-Rollen zum Zeitpunkt der Erstellung des Clusters indem separate Identitäten (Zertifikate AAD usw.) für jedes. Weitere Informationen zu den Standard-Einstellungen und ändern finden Sie unter [Rollenbasierte Zugriffskontrolle für Fabric Service Clients](service-fabric-cluster-security-roles.md).


## <a name="x509-certificates-and-service-fabric"></a>X. 509-Zertifikate und Service Fabric
X. 509-Zertifikate werden häufig zum Authentifizieren von Clients und Servern und verschlüsseln und Signieren von Nachrichten. Weitere Informationen zu diesen Zertifikaten finden Sie unter [Arbeiten mit Zertifikaten](http://msdn.microsoft.com/library/ms731899.aspx).

Einige wichtige Punkte berücksichtigen:

- Zertifikate im Cluster unter Produktionsarbeitslasten sollte mit einem ordnungsgemäß konfigurierten Zertifikat Serverdienst von Windows erstellt oder zugelassene [Zertifizierungsstelle (Certificate Authority, CA)](https://en.wikipedia.org/wiki/Certificate_authority)abgerufen.
- Verwenden Sie niemals alle temporären oder Testzertifikate Produktion mit Tools wie MakeCert.exe erstellt werden.
- Sie können ein selbstsigniertes Zertifikat jedoch nur tun dies für Test-Cluster und nicht in der Produktion.

### <a name="server-x509-certificates"></a>Server x. 509-Zertifikate

Serverzertifikate sollen primäre Server (Knoten) Clients authentifizieren oder einen Server (Knoten) auf einen Server (Knoten) authentifizieren. Anfängliche überprüft, wenn ein Client oder Knoten ein Knotens authentifiziert ist der allgemeine Name im Feld Betreff den Wert überprüfen. Diesen oder eines alternativen Antragstellernamen der Zertifikate muss in der Liste der zulässigen allgemeinen Namen vorhanden sein.

Der Artikel beschreibt, wie Zertifikate mit alternativen Antragstellernamen (SAN) generiert: [einen Alternativer Antragstellername, ein gesichertes LDAP-Zertifikat hinzufügen](http://support.microsoft.com/kb/931351).

Das Feld Betreff kann mehrere Werte enthalten, jeweils eine Initialisierung angegeben Wert vorangestellt. In den meisten Fällen ist die Initialisierung "CN" Gemeinsamer Name; z. B. "CN = www.contoso.com". Es kann auch für das Feld Betreff leer zu sein. Falls das optionale Feld Alternativer Antragstellername aufgefüllt wird, muss sie den gemeinsamen Namen des Zertifikats und einem Eintrag pro alternativen Antragstellernamen enthalten. Diese werden als DNS-Namen eingegeben.

Der Wert im Feld Beabsichtigte Zwecke des Zertifikats sollte einen geeigneten Wert wie "Authentifizierung" oder "Clientauthentifizierung" enthalten.

### <a name="client-x509-certificates"></a>X. 509-Clientzertifikate

Clientzertifikate werden normalerweise nicht von einer Drittanbieter-Zertifizierungsstelle ausgestellt. Stattdessen enthält der persönliche Speicher der aktuellen Benutzer normalerweise Clientzertifikate von einer Stammzertifizierungsstelle mit beabsichtigten Zweck "Clientauthentifizierung" abgelegten. Der Client können ein Zertifikat gegenseitige Authentifizierung erforderlich ist.

>[AZURE.NOTE] Sämtliche Verwaltungsvorgänge auf einem Cluster Service Fabric erfordern Serverzertifikate. Clientzertifikate können für die Verwaltung verwendet werden.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel enthält grundlegende Informationen zur Clustersicherheit. Anschließend [Erstellen Sie einen Cluster in Azure Ressourcenmanager Vorlage](service-fabric-cluster-creation-via-arm.md) oder durch den [Azure-Portal](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
