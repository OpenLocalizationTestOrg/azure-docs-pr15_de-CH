<properties
    pageTitle="Hybrid-Verbindungen Übersicht | Microsoft Azure"
    description="Erfahren Sie mehr über Hybrid-Verbindungen, Sicherheit, TCP-Ports und unterstützte Konfigurationen. MAK, WABS."
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ccompy"/>


# <a name="hybrid-connections-overview"></a>Hybrid-Verbindungen (Übersicht)
Hybrid-Verbindungen Listet die unterstützten Konfigurationen und führt die erforderlichen TCP-Ports.


## <a name="what-is-a-hybrid-connection"></a>Was ist ein hybridverbindung

Hybridverbindungen sind Azure BizTalk Services. Hybridverbindungen bieten eine Möglichkeit einfache und bequeme zum lokalen Ressourcen Ihre Firewall Web Apps Features in Azure App Service (früher Websites) und Mobile Apps in Azure App Service (früher Mobile Dienste) herstellen.

![Hybridverbindungen][HCImage]

Hybrid-Verbindungen bietet folgende Vorteile:

- Webapps und Mobile Apps können sicher vorhandenen lokalen Daten und Dienste zugreifen.
- Mehrere Web Apps oder Mobile können Hybrid-Verbindung um eine lokale Ressource freigeben.
- Minimale TCP-Ports müssen auf das Netzwerk zugreifen.
- Hybride Verbindung Applikationen zugreifen Hybrid-Verbindung nur die Veröffentlichung bestimmte lokale Ressource.
- Alle lokalen Ressourcen möglich, die einen statischen TCP-Port wie SQL Server, MySQL, HTTP-Web-APIs und die meisten benutzerdefinierte Webdienste verwendet.

    > [AZURE.NOTE] TCP-basierte Dienste, die Verwendung von dynamischen Anschlüssen (wie passiven FTP-Modus oder Passivmodus erweitert) werden derzeit nicht unterstützt. LDAP wird ebenfalls nicht unterstützt. LDAP verwendet einen statischen TCP-Port, jedoch können auch UDP. Es wird daher nicht unterstützt.

- Kann mit allen unterstützt Web Apps (.NET, PHP, Java, Python, Node.js) und Mobile Apps (Node.js, .NET) verwendet werden.
- Webapps und Mobile Apps kann auf lokale Ressourcen auf genau die gleiche Weise wie zugreifen Web- oder Mobile-Anwendung auf Ihrem lokalen Netzwerk befindet. Beispielsweise können dieselbe Verbindung Zeichenfolge lokalen auch in Azure verwendet werden.


Hybridverbindungen bieten Organisationsadministratoren Kontrolle und Transparenz der Unternehmensressourcen zugreifen Hybrid Applications, einschließlich:

- Mit Gruppenrichtlinien können Hybrid-Verbindung im Netzwerk zulassen und außerdem festlegen, Ressourcen hybride Anwendung zugegriffen werden können.
- Ereignis und Audit-Protokolle im Unternehmensnetzwerk bieten Einblick in die Ressourcen von Hybriden zugegriffen.


## <a name="example-scenarios"></a>Beispielszenarien

Hybridverbindungen unterstützen die folgenden Kombinationen von Rahmen und Anwendung:

- .NET Framework Zugriff auf SQL Server
- .NET Framework Zugriff auf HTTP/HTTPS-Dienste mit WebClient
- Zugriff auf SQL Server, MySQL PHP
- Java-Zugriff auf SQL Server, MySQL und Oracle
- Java-Zugriff auf HTTP/HTTPS-Dienste

Beim Hybrid-Verbindungen mit lokalen SQL Server zuzugreifen, Folgendes:

- SQL Express benannte Instanzen müssen für statische Ports konfiguriert werden. Standardmäßig benannten SQL Express Instanzen Dynamische Anschlüsse verwenden.
- SQL Express standardmäßig Instanzen verwenden einen statischen Port, aber TCP aktiviert sein. TCP ist standardmäßig nicht aktiviert.
- Bei Verwendung von Clustering oder Verfügbarkeitsgruppen der `MultiSubnetFailover=true` Modus wird derzeit nicht unterstützt.
- Die `ApplicationIntent=ReadOnly` wird derzeit nicht unterstützt.
- SQL-Authentifizierung möglicherweise als die End-to-End-Autorisierungsmethode von Azure-Anwendung und den lokalen SQL Server unterstützt.


## <a name="security-and-ports"></a>Sicherheit und ports

Hybridverbindungen verwenden gemeinsame Zugriff Signatur (SAS) Autorisierung Verbindungen von Azure Applications und lokalen Hybrid Connection Manager Hybrid-Verbindung sichern. Separate Verbindungsschlüssel sind für die Anwendung und den lokalen Hybrid-Verbindungs-Manager erstellt. Diese Verbindungsschlüssel werden verlängerter und einzeln gesperrt.

Hybridverbindungen bieten nahtlose und sichere Verteilung der Schlüssel für die Anwendung und den lokalen Hybrid-Verbindungs-Manager.

Finden Sie unter [Erstellen und Verwalten von Hybridverbindungen](integration-hybrid-connection-create-manage.md).

*Anwendungsautorisierung Hybrid-Verbindung getrennt wird*. Jede Berechtigung-Methode kann verwendet werden. Die Autorisierung hängt der End-to-End Autorisierungsmethoden Azure Cloud und lokalen Komponenten unterstützt. Azure-Anwendung greift z. B. auf einen lokalen SQL Server. In diesem Fall kann SQL-Autorisierung die Autorisierungsmethode End-to-End unterstützt.

#### <a name="tcp-ports"></a>TCP-ports
Hybridverbindungen müssen nur ausgehenden TCP- oder HTTP-Verbindung aus dem privaten Netzwerk. Sie brauchen keine Firewallports öffnen oder ändern Perimeter Netzwerkkonfiguration um eingehende Konnektivität in Ihrem Netzwerk ermöglichen.

Die folgenden TCP-Ports werden Hybrid-Verbindungen verwendet:

Anschluss | Warum benötigen
--- | ---
9350 - 9354 | Diese Ports werden für die Datenübertragung verwendet. Service Bus Relay Manager Prüfpunkte Port 9350, ob TCP-Verbindung verfügbar ist. Falls verfügbar, nimmt er, dass Port 9352 ebenfalls verfügbar ist. Datenverkehr geht über Port 9352. <br/><br/>Ausgehende Verbindung auf diese Ports zulassen.
5671 | Wenn Port 9352 Datenverkehr verwendet wird, wird Anschluss 5671 als der Steuerungskanal verwendet. <br/><br/>Ausgehende Verbindungen auf diesem Anschluss zulassen
80, 443 | Diese Ports werden für einige Datenanfragen an Azure verwendet. Wenn Ports 9352 und 5671 nicht verwendet werden, sind *dann* die Ports 80 und 443 fallback Anschlüsse zur Datenübertragung und der Kanal.<br/><br/>Ausgehende Verbindung auf diese Ports zulassen. <br/><br/>**Hinweis** Es wird nicht empfohlen, als Alternative Ports anstelle der TCP-Ports verwenden. HTTP/WebSocket-Protokoll statt systemeigene TCP Daten dient. Sie führen geringere Leistung.



## <a name="next-steps"></a>Nächste Schritte

[Erstellen und Verwalten von Hybridverbindungen](integration-hybrid-connection-create-manage.md)<br/>
[Verbinden Sie Azure webapps auf eine lokale Ressource](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[Verbinden Sie mit lokalen SQL Server aus einer Azure Web app.](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>


## <a name="see-also"></a>Siehe auch

[REST-API zum Verwalten von BizTalk-Diensten auf Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[der BizTalk-Dienste: Editionen Diagramm](biztalk-editions-feature-chart.md)<br/>
[Erstellen einer BizTalk-Service mit Azure-portal](biztalk-provision-services.md)<br/>
[Der BizTalk-Dienste: Registerkarten Dashboard, Monitor und skalieren](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
