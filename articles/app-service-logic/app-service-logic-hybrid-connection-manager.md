<properties 
    pageTitle="Mit der Hybrid-Verbindungsmanager | Microsoft Azure" 
    description="Installieren und Konfigurieren der Hybrid-Verbindungs-Manager und Verbinden mit lokalen Connectors Logik Apps" 
    services="app-service\logic" 
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/>

# <a name="connect-to-on-premises-connectors-using-the-hybrid-connection-manager"></a>Verbinden Sie mit lokalen Connectors mit der Hybrid-Verbindungs-Manager

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2014-12-01-Vorschau-Schemaversion. Logik Apps allgemein verfügbar (GA) verwendet ein Gateway für lokale Konnektivität. Weitere Informationen über das neue [Gateway](app-service-logic-gateway-connection.md) und [Logik Apps GA](https://azure.microsoft.com/documentation/services/logic-apps/).

Für die Verwendung ein lokalen Systems verwendet Logik Apps Hybrid Connection Manager. Einige Anschlüsse können sich auf einem lokalen System wie SQL Server, SAP, SharePoint. 

Hybrid Connection Manager (HCM) ist ein Klick-einmal Installationsprogramm, das auf einem IIS-Server in Ihrem Netzwerk hinter der Firewall installiert ist. HCM authentifiziert mit einer Azure Service Bus Relay, das lokale System mit dem Connector in Azure. 

> [AZURE.NOTE] Hybrid-Verbindungs-Manager ist erforderlich, wenn Sie eine lokale Ressource Ihre Firewall herstellen. Wenn Sie nicht auf einem lokalen System verbinden, dann brauchen Sie nicht den Hybrid-Verbindungs-Manager.

Zunächst müssen wie folgt vor:

- Azure Service Bus Relay-Namespace SAS-Verbindungszeichenfolge. Anzeigen Sie bestimmt die Ebene Relays umfasst [Service Bus Preise](https://azure.microsoft.com/pricing/details/service-bus/)
- Lokal anmelden Systeminformationen wie Benutzernamen und Kennwort. Wenn Sie mit einer lokalen SQL Server verbinden, benötigen Sie z. B. die SQL Server-Anmeldekonto und Kennwort.
- Lokale Server-Informationen einschließlich Anschluss Nummer und Server. Wenn Sie mit einer lokalen SQL Server verbinden, benötigen Sie z. B. die SQL Server-Namen und die TCP-Anschlussnummer.

## <a name="get-the-service-bus-connection-string"></a>Abrufen der Verbindungszeichenfolge Service Bus

Kopieren Sie im Portal Azure Service Bus Stamm SAS-Verbindungszeichenfolge. Diese Verbindungszeichenfolge verbunden Azure Connector mit Ihrem lokalen System. 

1. In [klassischen Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)Service Bus-Namespace, und wählen Sie **Verbindungsinformationen**:

    ![][SB_ConnectInfo]

2. Kopieren Sie die SAS-Verbindungszeichenfolge:

    ![][SB_SAS]

## <a name="install-the-hybrid-connection-manager"></a>Der Hybrid-Verbindungs-Manager installieren

1. Wählen Sie in [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)Connector erstellten. Zum Öffnen Sie wählen Sie **Durchsuchen**, **API-Apps**auswählen und wählen Sie die Connectors oder API-App. 
<br/><br/>
**Hybrid-Verbindung**ist die Installation **unvollständig**:
<br/>
![][2] 

2. Wählen Sie **Hybrid-Verbindung**. Die zuvor eingegebenen Service Bus-Verbindungszeichenfolge aufgeführt.
3. Kopieren der **primären Verbindungszeichenfolge**:
<br/>
![][PrimaryConfigString]

4. Unter **Lokale Hybrid-Verbindungs-Manager**können Sie downloaden Hybrid Connection Manager oder direkt aus dem Portal installieren. 
<br/><br/>
Wechseln Sie direkt über das Portal installieren auf dem lokalen IIS-Server auf das Portal durchsuchen und wählen **herunterladen und konfigurieren**.
<br/><br/>
Zu Hybrid Connection Manager herunterzuladen, gehen zu der lokalen IIS-Server, der **ClickOnce-Anwendung** (http://hybridclickonce.azurewebsites.net/install/Microsoft.Azure.BizTalk.Hybrid.ClickOnce.application). Die Installation wird automatisch gestartet, damit es ausgeführt werden kann.

5. **Listener-Setup** -Fenster geben Sie die **Primäre Konfigurationszeichenfolge** vorher (in Schritt 3) eingefügt, und wählen Sie **Installieren**.

Wenn die Installation abgeschlossen ist, folgenden angezeigt:
<br/>
![][3] 

Wenn Sie den Connector erneut suchen, ist Hybrid-Verbindungsstatus **verbunden**. Sie müssen den Connector zu schließen und erneut öffnen:
<br/>
![][4] 

> [AZURE.NOTE] Wechseln zur sekundären Verbindungszeichenfolge führen Sie Hybrid-Verbindung Setup erneut aus und geben Sie die **Sekundäre Konfigurationszeichenfolge**.


## <a name="tcp-ports-and-security"></a>TCP-Ports und Sicherheit

Beim Erstellen einer hybridverbindung ist eine Website auf dem lokalen lokalen IIS-Server erstellt. Der IIS-Server kann in einer DMZ. Der TCP-Port auf dem IIS-Server vorsehen

TCP-Port | Warum
--- | ---
 | Keine eingehenden TCP-Ports sind erforderlich.
9350 - 9354 | Diese Ports werden für die Datenübertragung verwendet. Service Bus Relay Manager Prüfpunkte Port 9350, ob TCP-Verbindung verfügbar ist. Falls verfügbar, nimmt er, dass Port 9352 ebenfalls verfügbar ist. Datenverkehr geht über Port 9352. <br/><br/>Ausgehende Verbindung auf diese Ports zulassen.
5671 | Port 9352 Datenverkehr wird als Port 5671 verwendet die der Kanal. <br/><br/>Ausgehende Verbindungen auf diesem Anschluss zulassen 
80, 443 | Wenn Ports 9352 und 5671 nicht verwendet werden, sind *dann* die Ports 80 und 443 fallback Anschlüsse zur Datenübertragung und der Kanal.<br/><br/>Ausgehende Verbindung auf diese Ports zulassen.
Auf Prem Systemanschluss | Auf dem lokalen System geöffnet vom System verwendet. SQL Server verwendet z. B. in der Regel Port 1433. Öffnen Sie diese TCP-Port.

[Hinter einer Firewalls mit Servicebus-Hosting](http://msdn.microsoft.com/library/azure/ee706729.aspx)

## <a name="troubleshooting"></a>Problembehandlung

![][HCMFlow]

### <a name="on-premises-troubleshooting"></a>Fehlerbehebung vor Ort

1. Bestätigen Sie auf dem IIS-Server IIS Web-Rolle installiert und alle IIS-Dienste werden gestartet.
2. Der IIS-Server bestätigen Sie, dass der Hybrid-Verbindungs-Manager installiert und ausgeführt wird:
 - ***MicrosoftAzureBizTalkHybridListener*** Website im IIS-Manager (Inetmgr) sind und ausgeführt werden. 
 - Diese Website verwendet die ***HybridListenerAppPool*** , die als *lokale integrierte Benutzer Netzwerkdienstkonto* ausgeführt wird. Diese AppPool sollte auch gestartet werden.
3. Auf dem IIS-Server zu bestätigen, dass der Connector installiert ist und ausgeführt wird: 
 - Für den Connector wird eine Website erstellt. Wenn Sie eine SQL-Verbindung erstellt, gibt z. B. eine Webseite ***MicrosoftSqlConnector_nnn*** . Bestätigen Sie im IIS-Manager (Inetmgr) diese Website aufgelistet und gestartet ist. 
 - Diese Website verwendet einen eigenen IIS-Anwendungspool mit dem Namen ***HybridAppPoolnnn***. Diese AppPool wird als *das Netzwerkdienstkonto lokale integrierte Benutzer* ausgeführt. Diese Website und AppPool sollten beide gestartet. 
 - Durchsuchen des lokalen Connectors. Z. B. wenn die Connector Website Port 6569 verwendet, wechseln Sie zu Http://localhost:6569. Ein Standarddokument ist nicht konfiguriert, einen `HTTP Error 403.14 - Forbidden error` wird erwartet.
4. Bestätigen Sie Ihre Firewall in diesem Thema aufgeführten TCP-Ports geöffnet sind.
5. Betrachten Sie das Quell- oder Ziel-System:
 - Einige lokale Systeme erfordern zusätzliche Abhängigkeitsdateien. Beispielsweise müssen SAP Dateien Wenn Sie lokalen SAP herstellen, auf dem IIS-Server installiert.
 - Überprüfen Sie die Verbindung zu dem System mit dem Anmeldekonto. Beispielsweise muss der TCP-Port vom System verwendet wie SQL Server Port 1433 geöffnet sein. Das Azure-Portal eingegebene Konto muss Zugriff auf das System.
6. Überprüfen Sie auf dem IIS-Server die Ereignisprotokolle nach Fehlern. 
7. Bereinigen und der Hybrid-Verbindungs-Manager installieren: 
 - Löschen Sie in IIS Connector-Website und der Anwendungspool. 
 - Der Hybrid-Verbindungs-Manager erneut und bestätigen, dass Sie die richtige **Primäre Konfigurationszeichenfolge** des Connectors eingeben.



### <a name="in-the-azure-classic-portal"></a>Im klassischen Azure-portal

1. Bestätigen Sie, dass der Service Bus-Namespace **aktiv** ist.
2. Wenn Sie den Connector erstellen, geben Sie die Verbindungszeichenfolge Service Bus SAS. Geben Sie die ACS-Verbindungszeichenfolge.


## <a name="faq"></a>Häufig gestellte Fragen

**Frage**: sind zwei Hybrid-Verbindungs-Manager. Was ist der Unterschied? 

**Antwort**: der [Hybridverbindungen](../biztalk-services/integration-hybrid-connection-overview.md) Technologie, Web Apps (früher Websites) und Mobile Apps (früher mobile Dienste) mit der lokalen Verbindung. Diese Hybrid-Verbindung-Manager ist eine eigene [Setup](../biztalk-services/integration-hybrid-connection-create-manage.md) und verwendet einen Azure BizTalk-Dienst (im Hintergrund). TCP und HTTP-Protokolle unterstützt.

Mit Azure App Service haben wir auch ein Hybrid-Verbindungs-Manager.  Diese Hybrid-Verbindungs-Manager ist *nicht* mit einer Azure BizTalk Service (im Hintergrund) und unterstützt die Protokolle TCP und HTTP. [Anschlüsse und API-Liste](app-service-logic-connectors-list.md)anzeigen

Verbindung zum lokalen System verwenden beide Azure Service Bus.

**Frage**: beim Erstellen einer benutzerdefinierten API App kann ich verwenden App Service Hybrid Connection Manager für lokale Verbindung? 

**Antwort**: nicht im herkömmlichen Sinn. Sie verwenden einen integrierten Anschluss, App Service Hybrid Connection Manager für die Verbindung zum lokalen System konfigurieren. Verwenden Sie diesen Connector mit benutzerdefinierten API-App möglicherweise über eine Logik-App. Derzeit nicht entwickeln oder Erstellen eigener Hybrid API App (wie SQL Connector oder Datei-Connector).

Wenn Ihre benutzerdefinierte API TCP- oder HTTP-Port verwendet, können Sie [Hybridverbindungen](../biztalk-services/integration-hybrid-connection-overview.md) und seine Hybriden Verbindungs-Manager. In diesem Szenario wird ein Azure BizTalk-Dienst verwendet. [Mit lokalen SQL Server-Web-App verbinden](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md) können.  


## <a name="read-more"></a>Lesen Sie mehr

[Überwachen von Logik-Apps](app-service-logic-monitor-your-logic-apps.md)<br/>
[Service Bus Preise](https://azure.microsoft.com/pricing/details/service-bus/)



[SB_ConnectInfo]: ./media/app-service-logic-hybrid-connection-manager/SB_ConnectInfo.png
[SB_SAS]: ./media/app-service-logic-hybrid-connection-manager/SB_SAS.png
[PrimaryConfigString]: ./media/app-service-logic-hybrid-connection-manager/PrimaryConfigString.png
[HCMFlow]: ./media/app-service-logic-hybrid-connection-manager/HCMFlow.png
[2]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupIncomplete.jpg
[3]: ./media/app-service-logic-hybrid-connection-manager/HybridSetup.jpg
[4]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupComplete.jpg

 
