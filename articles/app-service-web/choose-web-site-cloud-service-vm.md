<properties
    pageTitle="Azure App Service, virtuelle Computer Service Fabric und Clouddienste Vergleich | Microsoft Azure"
    description="Erfahren Sie, wie zwischen Azure App Service, virtuelle Computer Service Fabric und Cloud-Dienste für das Hosten von ASP.NET-Webanwendungen."
    services="app-service\web, virtual-machines, cloud-services"
    documentationCenter=""
    authors="tdykstra"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2016"
    ms.author="tdykstra"/>

# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Azure App Service, virtuelle Computer Service Fabric und Clouddienste Vergleich

## <a name="overview"></a>Übersicht

Azure bietet verschiedene Methoden zum Hosten von Websites: [Azure App Service][], [virtuelle Computer][] [Service Fabric][]und [Cloud-Dienste][]. Dieser Artikel soll Ihnen verstehen die Optionen und die richtige Wahl für Ihre Webanwendung.

Azure App Service ist die beste Wahl für die meisten webapps. Bereitstellung und Verwaltung in die Plattform integriert und eingebaute Load-balancing und Traffic Manager bieten hohe Verfügbarkeit Sites können schnell skaliert werden hohen Datenverkehr behandeln. Sie können vorhandene Websites Azure App Service problemlos mit einem [online-Migrationsprogramm](https://www.migratetoazure.net/)verschieben, eine Open-Source-Anwendung aus der Galerie Anwendung verwenden oder Erstellen einer neuen Website mit Tools Ihrer Wahl und Rahmen. [Webaufträge][] Funktion erleichtert Hintergrundauftrag Verarbeitung Ihrer App Service Anwendung hinzufügen.

Service Fabric ist eine gute Wahl, wenn Sie eine neue Anwendung erstellen oder eine vorhandene Anwendung auf eine Microservice Architektur verwenden erneut zu schreiben. Apps, die auf einem freigegebenen Computer ausgeführt, können klein anfangen und massiv mit Hunderten oder Tausenden von Computern nach Bedarf wachsen. Statusbehaftete Dienste erleichtern, konsistent und zuverlässig Anwendungszustand speichern und Fabric-Dienst verwaltet automatisch Service Partitionierung, Skalierung und Verfügbarkeit für Sie.  Fabric Service unterstützt auch WebAPI Open Web Interface owin .NET (-) und ASP.NET Core.  App Service bietet gegenüber Service Fabric auch mehr Kontrolle oder direkten Zugriff auf die zugrunde liegende Infrastruktur. Sie können Ihre Server remote oder Startaufgaben Server konfigurieren. Cloud-Dienste ähnelt Fabric Service in Grad an Kontrolle und Bedienung, aber ist jetzt eine ältere Service und Service Fabric Neuentwicklungen empfohlen.

Wenn Sie eine Anwendung, die wesentliche Änderungen App oder Service Fabric Ausführen benötigen haben, können Sie virtuelle Computer Vereinfachung der Migration zur Cloud auswählen. Ordnungsgemäß erfordert konfigurieren, Sichern und Verwalten von VMs jedoch mehr Zeit und IT-Expertenwissen im Vergleich zu Azure App Service und Service Fabric. Wenn Azure Virtual Machines erwägen, sicherzustellen Sie, dass Sie berücksichtigen Wartungsaufwand patch, aktualisieren und verwalten Ihre VMware-Umgebung. Azure Virtual Machines ist Infrastructure-as-a-Service (IaaS) App Service und Service Fabric Platform-as-a-Service (Paas). 

## <a name="features"></a>Vergleich der Features

Die folgende Tabelle vergleicht die Funktionen der App Service, Cloud-Services, virtuellen Maschinen und Service Fabric zu dem besten. Finden Sie aktuelle Informationen über die SLA für jede Option [Azure Service Level Agreements](/support/legal/sla/).

Funktion|App-Dienst (webapps)|Cloud-Dienste (Webrollen)|Virtuelle Computer|Service Fabric|Notizen
---|---|---|---|---|---
Nahezu sofortige Bereitstellung|X|||X|Eine Anwendung oder ein Cloud-Dienst bereitstellen oder erstellen einen virtuellen Computer dauert einige Minuten mindestens; Bereitstellen einer Anwendung zu einem Web app Sekunden.
Größere Maschinen ohne erneute Bereitstellung skalieren|X|||X|
Web Server-Instanzen Teilen Inhalts- und, was bedeutet, dass Sie nicht erneut oder konfigurieren Sie skalieren.|X|||X|
Umgebung mit mehreren Bereitstellung (Produktion und Staging)|X|X||X|Service Fabric können Sie mehrere Umgebungen für Ihre apps oder verschiedene Versionen Ihrer app Seite für Seite bereitstellen.
Automatische Verwaltung von OS|X|X|||Automatische Betriebssystemupdates sollen für zukünftige Service Fabric freizugeben.
Nahtlose Plattform wechseln (problemlos zwischen 32-Bit und 64-Bit)|X|X|||
Code mit GIT FTP bereitstellen|X||X||
Bereitstellen von Code mit Web Deploy|X||X||Cloud-Dienste unterstützt die Verwendung von Web Deploy Bereitstellen von Updates für einzelne Instanzen. Jedoch können für Ausbringung einer Rolle verwenden, und verwenden Sie Web Deploy Update müssen Sie separat für jede Instanz einer Rolle bereitgestellt. Mehrere Instanzen erforderlich für die Cloud Service SLA für Produktion qualifizieren.
WebMatrix unterstützt|X||X||
Zugriff auf Dienste wie Service Bus, Speicher, SQL-Datenbank|X|X|X|X|
Hostweb oder Web Services Tier einer Multi-Tier-Architektur|X|X|X|X|
Host mittlere Ebene einer Multi-Tier-Architektur|X|X|X|X|App Service webapps bietet einfach eine Zwischenebene REST-API und [Webaufträge](http://go.microsoft.com/fwlink/?linkid=390226) Feature bietet Hintergrund Aufträge. Sie können in eine Webseite zu unabhängige Skalierung der Ebene Webaufträge ausführen. Die Vorschaufunktion [API-apps](../app-service-api/app-service-api-apps-why-best-platform.md) bietet noch mehr Funktionen zum Hosten von REST-Diensten.
Integrierte Unterstützung für MySQL als Dienst|X|X|X||Cloud-Services integrieren MySQL als Dienst durch ClearDBs angeboten, aber nicht als Teil des Workflows Azure-Portal.
Unterstützung für ASP.NET klassischen ASP, Node.js, PHP, Python|X|X|X|X|Fabric Service unterstützt die Erstellung einer Web-Front-End mit [ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) oder Sie können jede Art von Anwendung (Node.js, Java usw.) als [Gast ausführbare](../service-fabric/service-fabric-deploy-existing-app.md)bereitstellen.
Skalieren Sie mehrerer Instanzen ohne erneute Bereitstellung|X|X|X|X|Virtuelle Computer können mehrere Instanzen skalieren, aber die Dienste ausgeführt werden müssen diese Skalierung behandelt geschrieben. Sie haben konfigurieren einen Lastenausgleich Weiterleiten von Anfragen über die Computer und einer Gruppe Affinität zum gleichzeitigen Neustart aller Instanzen durch Wartung oder Hardwarefehler verhindert.
Unterstützung für SSL|X|X|X|X|App Service Web Apps wird SSL für benutzerdefinierten Domänennamen nur für Basic und Standard unterstützt. Informationen zum Verwenden von SSL mit Web-apps finden Sie unter [konfigurieren ein SSL-Zertifikat für eine Azure-Website](../app-service-web/web-sites-configure-ssl-certificate.md).
Visual Studio-integration|X|X|X|X|
Remotedebuggen|X|X|X||
Code mit TFS bereitstellen|X|X|X|X|
Netzwerkisolation mit [Azure Virtual Network](/services/virtual-network/)|X|X|X|X|Siehe auch [virtuelle Vernetzung Azure Websites](/blog/2014/09/15/azure-websites-virtual-network-integration/)
Unterstützung für [Azure Traffic Manager](/services/traffic-manager/)|X|X|X|X|
Integrierter Endpunkt überwachen|X|X|X||
Remote desktop Zugriff auf Server||X|X|X|
Benutzerdefinierte MSI installiert||X|X|X|Fabric Service können Sie eine ausführbare Datei als [Gast ausführbare](../service-fabric/service-fabric-deploy-existing-app.md) oder Sie können jede Anwendung auf den virtuellen Computern.
Definieren/Startaufgaben, ausführen||X|X|X|
ETW-Ereignisse hören||X|X|X|

## <a name="scenarios"></a>Szenarien und Vorschläge

Hier sind einige gängige Anwendungsszenarien mit, welche Azure möglicherweise Webhosting-Option für jedes passende, Empfehlung.

- [Ich benötige ein Web-front-End mit Hintergrund Verarbeitung und Datenbank Back-End Business Applications integriert auf lokal ausgeführt.](#onprem)
- [Ich zuverlässig corporate Webseite hosten, die gut skaliert und bietet globale erreichen.](#corp)
- [Ich habe eine IIS6-Anwendung unter Windows Server 2003.](#iis6)
- [Ich bin Inhaber eines kleinen Unternehmens und benötige eine preiswerte Möglichkeit, Meine Website mit zukünftigen Wachstum berücksichtigt.](#smallbusiness)
- [Ich bin ein Web oder ein Grafikdesigner und möchte entwerfen und Erstellen von Websites für meine Kunden.](#designer)
- [Ich bin mit einem Web-Front-End-Anwendung mit mehreren Ebenen zur Cloud migriert.](#multitier)
- [Meine Anwendung hängt stark angepasste Windows oder Linux-Umgebung und in die Cloud verschieben möchten.](#custom)
- [Meine Website verwendet open Source-Software und in Azure gehostet werden soll.](#oss)
- [Ich habe eine LOB Anwendung, mit dem Unternehmensnetzwerk herstellen.](#lob)
- [Ein REST-API oder einen Webdienst für mobile Clients hosten soll.](#mobile)


### <a id="onprem"></a>Ich benötige ein Web-front-End mit Hintergrund Verarbeitung und Datenbank Back-End Business Applications integriert auf lokal ausgeführt.

Azure App Service ist eine hervorragende Lösung für komplexe. Sie können entwickeln apps, die auf eine ausgewogene Plattform Laden automatisch skalieren, mit Active Directory geschützt und Ihre lokalen Ressourcen. Verwaltung die apps durch eine erstklassige Portal und APIs, und erhalten Sie Einblick, wie Kunden diese app Insight Tools verwenden. [Webaufträge][] Feature kann Hintergrundprozesse ausgeführt und Aufgaben als Teil der Webebene Hybrid-Konnektivität und Funktionen VNET erleichtern Verbindung zu lokalen Ressourcen. Azure App Service bietet drei 9 SLA für Web-apps und ermöglicht:

* Anwendungsbetrieb zuverlässig Self-healing, Auto-patching Cloud-Plattform.
* Skalieren Sie Automatisches in einem globalen Netzwerk von Rechenzentren.
* Sicherung und Wiederherstellung für Disaster Recovery.
* ISO SOC2 und PCI-konform sein.
* Integration mit Active Directory

### <a id="corp"></a>Ich zuverlässig corporate Webseite hosten, die gut skaliert und bietet globale erreichen.

Azure App Service ist eine großartige Lösung für Unternehmen Websites hosten. Webapps schnell und problemlos in einem globalen Netzwerk von Rechenzentren Nachfrage zu können. Es bietet lokale erreichen, Fehlertoleranz und intelligenten Verwaltung. Auf einer Plattform, die branchenführenden Managementtools bereitstellt, so dass Sie Standortstatus Einblick in und Website-Verkehr schnell und einfach. Azure App Service bietet drei 9 SLA für Web-apps und ermöglicht:

* Führen Sie Ihre Websites zuverlässig Self-healing, Auto-patching Cloud-Plattform.
* Skalieren Sie Automatisches in einem globalen Netzwerk von Rechenzentren.
* Sicherung und Wiederherstellung für Disaster Recovery.
* Verwalten von Protokollen und Datenverkehr mit integrierten Tools.
* ISO SOC2 und PCI-konform sein.
* Integration mit Active Directory

### <a id="iis6"></a>Ich habe eine IIS6-Anwendung unter Windows Server 2003.

Azure App Service vereinfacht die Infrastrukturkosten im Zusammenhang mit Migration älterer IIS6 Anwendung zu vermeiden. Microsoft hat [benutzerfreundliche Migrationstools und detaillierte migrationsanleitung](https://www.movemetowebsites.net/) , mit denen Sie zu Kompatibilität überprüfen jede Änderung, die vorgenommen werden müssen. Integration mit Visual Studio TFS und CMS-Tools erleichtert die IIS6 Applikationen direkt in der Cloud bereitstellen. Nach der Bereitstellung bietet Azur Portal robuste Management-Tools, mit denen Sie Kosten und erfüllt bei Bedarf verkleinern. Mit dem Migrationstool können Sie:

* Schnell und einfach Ihre Windows Server 2003 Web Legacyanwendung zur Cloud migriert.
* Entscheiden Sie, Ihre angeschlossenen SQL Datenbank lokal eine hybridanwendung erstellen lassen.
* Die SQL-Datenbank sowie die ältere Anwendung verschieben automatisch.

### <a id="smallbusiness"></a>Ich bin Inhaber eines kleinen Unternehmens und benötige eine preiswerte Möglichkeit, Meine Website mit zukünftigen Wachstum berücksichtigt.

Azure App Service ist eine hervorragende Lösung für dieses Szenario verwenden sie kostenlos und bei Bedarf weitere Funktionen hinzufügen. Jeder kostenlose WebApp kommt mit einer Domäne von Azure bereitgestellt (*IHRE_FIRMA*. *.azurewebsites.NET), und die Plattform integrierte Bereitstellung und Management-Tools sowie eine Anwendung Galerie, die ersten Schritte erleichtern. Es gibt viele andere Dienste und Skalierungsoptionen, die die Website mit der erhöhte Bedarf. Mit Azure App Service können Sie:

- Mit der freien beginnen und dann Skalierung nach Bedarf.
- Verwenden der Galerie gängige Web Applications wie WordPress schnell einrichten.
- Fügen Sie zusätzliche Azure Dienste und Funktionen Ihrer Anwendung nach Bedarf hinzu.
- Sichern Sie Ihrer Anwendung mit HTTPS.

### <a id="designer"></a>Ich bin Web Grafiker und Entwerfen und Erstellen von Websites für meine Kunden möchte

Für Web-Entwickler und Designer Azure App Service Integration verschiedener Frameworks und Tools unterstützt die Bereitstellung Git und FTP, und bietet eine enge Integration mit Tools und Services wie Visual Studio und SQL-Datenbank. Mit App-Service können Sie:

- Befehlszeilentools für [automatisierte Aufgaben][scripting].
- Arbeiten mit gängigen Programmiersprachen wie [.net][dotnet], [PHP][], [Node.js][nodejs], und [Python][].
- Wählen Sie drei Skalierung Ebenen für sehr hohe Kapazität skalieren.
- Integrieren mit anderen Diensten Azure [SQL-Datenbank]wie[sqldatabase], [Service Bus] [ servicebus] und [Speicher][]oder Partnerangebote aus dem [Azure-Speicher][azurestore]MySQL und MongoDB.
- Integration mit Tools wie Visual Studio, Git WebMatrix, WebDeploy, TFS und FTP.

### <a id="multitier"></a>Ich bin mit einem Web-Front-End-Anwendung mit mehreren Ebenen in die Cloud migrieren.

Wenn Sie eine Anwendung mit mehreren Ebene wie einem Webserver ausführen, die mit einer Datenbank verbunden ist Azure App Service empfohlen, die enge Integration in Azure SQL-Datenbank bietet. Und Funktion der Webaufträge für Back-End-Prozesse ausgeführt.

Wählen Sie Service Fabric für eine oder mehrere der Ebenen benötigen Sie mehr Kontrolle über die Server-Umgebung wie remote Server oder Server Start-Tasks konfigurieren.

Wählen Sie virtuelle Computer für mindestens eine der Ebenen, wenn ein eigenes Computerabbild verwenden oder Server-Software oder Dienste, die Sie auf Dienst konfigurieren können.

### <a id="custom"></a>Meine Anwendung hängt stark angepasste Windows oder Linux-Umgebung und in die Cloud verschieben möchten.

Die Anwendung komplexe Installation oder Konfiguration von Software und Betriebssystem erfordert, virtuelle Computer ist wahrscheinlich die beste Lösung. Virtuelle Computer können Sie:

- Die VM-Galerie mit einem Betriebssystem wie Windows oder Linux zu verwenden und für Ihre Anforderungen anpassen.
- Erstellen Sie und Laden Sie ein benutzerdefiniertes Bild von einem vorhandenen lokalen Server auf einem virtuellen Computer in Azure ausgeführt.

### <a id="oss"></a>Meine Website verwendet open Source-Software und in Azure gehostet werden soll

Wenn die open-Source-Framework App-Dienst unterstützt wird, werden die Sprachen und Frameworks von Ihrer Anwendung benötigt automatisch konfiguriert. App Service können Sie:

- Verwenden Sie viele gängige open Source Sprachen wie [.NET][dotnet], [PHP][], [Node.js][nodejs], und [Python][].
- Richten Sie WordPress, Drupal Umbraco, DNN und viele andere Drittanbieter-Web Applications.
- Migrieren einer vorhandenen Anwendung, oder erstellen Sie eine neue aus der Galerie.

Wenn die open-Source-Framework App-Dienst unterstützt wird, können Sie es auf einem anderen Azure Webhosting-Optionen ausführen. Mit virtuellen Computern installieren und konfigurieren die Software auf Computerabbild die Windows oder Linux-basierten.

### <a id="lob"></a>Ich habe eine LOB Anwendung, die Verbindung mit dem Unternehmensnetzwerk

Möchten Sie eine LOB Anwendung erstellen, möglicherweise Ihre Website direkten Zugang zu Diensten oder im Unternehmensnetzwerk. Dies ist möglich, App Service Service Fabric und virtuelle Maschinen mit [Azure Virtual Network Service](/services/virtual-network/). App Service können [VNET Integrationsfeature](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)Sie Ihre Azure Applications ausführen, als befänden Sie sich im Unternehmensnetzwerk ermöglicht.

### <a id="mobile"></a>Ich möchte eine REST-API oder Webdienst für mobile clients

HTTP-basierte WebServices können Sie eine Vielzahl von Clients, einschließlich mobiler Clients. Wie ASP.NET Web API Integration mit Visual Studio erstellen und Verwenden von REST-Dienste erleichtern.  Diese Dienste sind Webdienst-Endpunkt ausgesetzt, so ist es möglich, alle Web-hosting auf Azure zur Unterstützung dieses Szenarios verwenden. App Service ist jedoch eine hervorragende Wahl für das Hosten von REST-APIs. Mit App-Service können Sie:

- Erstellen Sie eine [mobile Anwendung](../app-service-mobile/app-service-mobile-value-prop.md) oder [API-app](../app-service-api/app-service-api-apps-why-best-platform.md) HTTP-Webdienst in einer Azure Global verteilten Rechenzentren hosten.
- Migrieren Sie vorhandene Dienste oder erstellen Sie neue.
- Erreichen Sie SLA für Verfügbarkeit mit einer einzelnen Instanz oder mehreren dedizierten Computern skalieren.
- Mithilfe der veröffentlichten Website zu anderen APIs HTTP-Clients, einschließlich mobiler Clients.

> [AZURE.NOTE]
> Möchten Sie mit Azure App Service beginnen, bevor Sie sich für ein Konto, werden Sie <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, können Sie sofort eine kurzlebige Starter Anwendung in Azure App Service kostenlos erstellen. Keine Kreditkarte, keine Zusagen.

## <a id="nextsteps"></a>Nächste Schritte

Weitere Informationen zu den drei Hosting-Optionen finden Sie unter [Einführung in Azure](../fundamentals-introduction-to-azure.md).

Zunächst mit den Optionen für Ihre Anwendung wählen, finden Sie unter den folgenden Ressourcen:

* [Azure App Service](/documentation/services/app-service/)
* [Azure-Cloud-Dienste](/documentation/services/cloud-services/)
* [Azure Virtual Machines](/documentation/services/virtual-machines/)
* [Service Fabric](/documentation/services/service-fabric)

<!-- URL List -->

[Azure App Service]: /services/app-service/
[Cloud-Dienste]: http://go.microsoft.com/fwlink/?LinkId=306052
[Virtuelle Computer]: http://go.microsoft.com/fwlink/?LinkID=306053
[Service Fabric]: /services/service-fabric
[ClearDB]: http://www.cleardb.com/
[Webaufträge]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: http://www.windowsazure.com/develop/net/common-tasks/enable-ssl-web-site/
[azurestore]: http://www.windowsazure.com/gallery/store/
[scripting]: http://www.windowsazure.com/documentation/scripts/?services=web-sites
[dotnet]: http://www.windowsazure.com/develop/net/
[nodejs]: http://www.windowsazure.com/develop/nodejs/
[PHP]: http://www.windowsazure.com/develop/php/
[Python]: http://www.windowsazure.com/develop/python/
[servicebus]: http://www.windowsazure.com/documentation/services/service-bus/
[sqldatabase]: http://www.windowsazure.com/documentation/services/sql-database/
[Speicher]: http://www.windowsazure.com/documentation/services/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
