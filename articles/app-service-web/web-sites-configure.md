<properties 
    pageTitle="Webapps in Azure App Service konfigurieren" 
    description="Eine Webanwendung in Azure Anwendungsdienste konfigurieren" 
    services="app-service\web" 
    documentationCenter="" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="configure-web-apps-in-azure-app-service"></a>Webapps in Azure App Service konfigurieren #

In diesem Thema erläutert eine Web app mithilfe der [Azure-Portal]zu konfigurieren.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="application-settings"></a>Einstellungen

1. Öffnen Sie in [Azure-Portal]Blade für Web app.
2. Klicken Sie auf **Alle**.
3. Klicken Sie auf **Anwendung**.

![ApplicationSettings][configure01]

**ApplicationSettings** Blade hat Einstellungen in verschiedene Kategorien gruppiert.

### <a name="general-settings"></a>Allgemeine Einstellung

**Framework-Versionen**. Legen Sie diese Optionen, wenn Ihre Anwendung eine dieser Frameworks verwendet: 

- **.NET Framework**: Legen Sie die .NET Framework-Version. 
- **PHP**: Legen Sie die PHP-Version oder **aus** PHP deaktivieren. 
- **Java**: Wählen Sie die Java-Version oder **aus** Java deaktivieren. Die Option **Web Container** Tomcat und Steg Versionen wählen.
- **Python**: Python Version bzw. **aus** Python deaktivieren.

Aus technischen Gründen deaktiviert Java für Ihre Anwendung aktivieren .NET, PHP und Python-Optionen.

<a name="platform"></a>
**Plattform**. Wählt aus, ob Ihrer Anwendung in einer 32-Bit- oder 64-Bit-Umgebung ausgeführt wird. Die 64-Bit-Umgebung erfordert Basic oder Standard-Modus. Frei und Shared-Modus immer in einer 32-Bit-Umgebung.

**Web Sockets**. Festgelegt **auf** das WebSocket-Protokoll aktivieren. Wenn beispielsweise Ihrer Anwendung [ASP.NET SignalR] oder [socket.io]verwendet.

<a name="alwayson"></a>
**Immer auf**. Standardmäßig werden Web-apps entladen, wenn sie für einige Zeit im Leerlauf befinden. Dadurch kann Ressourcen. Basic oder Standard können Sie **Immer** die Anwendung geladenen halten. Läuft Ihre app fortlaufende Webaufträge soll **Ständig**oder Webaufträge möglicherweise nicht zuverlässig ausgeführt.

**Pipelineversion verwaltet**. Legt den IIS [Pipelinemodus]. Standardeinstellung integriert (Standard), wenn Sie eine ältere Anwendung verfügen, die eine ältere Version von IIS erforderlich sind.

**Automatisches austauschen**. Aktivieren Sie automatische Swap für eine Bereitstellung Steckplatz tauschen App Service Web app automatisch in die Produktion an, wenn Sie ein Update zu diesem Slot drücken. Weitere Informationen finden Sie unter [bereitstellen, staging-Steckplätze für Web-apps in Azure App Service] (Web-Sites-bereitgestellt-publishing.md).

### <a name="debugging"></a>Debuggen

Das **Remotedebuggen**. Aktiviert das Remotedebuggen. Wenn aktiviert, können remote Debugger in Visual Studio Sie direkt mit Ihrer Anwendung verbinden. Remotedebuggen bleibt 48 Stunden aktiviert. 

### <a name="app-settings"></a>App-Einstellungen

Dieser Abschnitt enthält Name-Wert-Paare, die Sie web app wird beim Start von geladen. 

- Für .NET apps werden diese Einstellungen in .NET Konfiguration eingefügt `AppSettings` zur Laufzeit überschreiben Einstellungen. 

- PHP, Python, Java und Knoten Applikationen können diese Einstellungen Umgebungsvariablen zur Laufzeit zugreifen. Für jede Einstellung app entstehen zwei Umgebungsvariablen. eine mit den app Einstellung und einem anderen mit dem Präfix APPSETTING_ angegebene. Beide enthalten denselben Wert.

### <a name="connection-strings"></a>Verbindungszeichenfolgen

Verbindungszeichenfolgen für verknüpfte Ressourcen. 

Für .NET apps werden diese Verbindungszeichenfolgen in .NET Konfiguration eingefügt `connectionStrings` Einstellung zur Laufzeit überschreiben vorhandene Einträge, wobei der Schlüssel der Name der verknüpften Datenbank entspricht. 

PHP, Python, Java und Knoten verfügbar dazu als Variablen zur Laufzeit mit dem Präfix Verbindungstyp. Die Variable Präfixe Umgebung lauten wie folgt: 

- SQL Server:`SQLCONNSTR_`
- MySQL:`MYSQLCONNSTR_`
- SQL-Datenbank:`SQLAZURECONNSTR_`
- Benutzerdefiniert:`CUSTOMCONNSTR_`

Z. B. eine Verbindungszeichenfolge MySql Namen `connectionstring1`, wird durch die Umgebungsvariable zugegriffen werden `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Standarddokumente

Das Standarddokument ist die Webseite, die die Stamm-URL für eine Website angezeigt wird.  Die erste übereinstimmende Datei in der Liste verwendet. 

Webapps können Module, dass Route basierend auf URL statische Inhalte in diesem Fall gibt es ist kein Standarddokument so.    

### <a name="handler-mappings"></a>Handlerzuordnungen

Verwenden Sie diesen Bereich, benutzerdefiniertes Skriptprozessoren um Anfragen für bestimmte Dateitypen hinzufügen. 

- **Erweiterung**. Die Erweiterung *.PHP oder handler.fcgi behandelt werden. 
- **Pfad des Prozessors**. Der absolute Pfad der Prozessor. Der Prozessor verarbeitet Anfragen an Dateien mit der Erweiterung. Der Pfad `D:\home\site\wwwroot` Stammverzeichnis Ihrer Anwendung auf.
- **Zusätzliche Argumente**. Optionale Befehlszeilenargumente für den Prozessor 


### <a name="virtual-applications-and-directories"></a>Virtuelle Applikationen und Verzeichnisse 
 
Geben Sie zum Konfigurieren der virtuellen Anwendung und Verzeichnisse jedes virtuelle Verzeichnis und die entsprechenden physischen Pfad relativ zum Stammverzeichnis Website. Optional können Sie der **Anwendung** ein virtuelles Verzeichnis als Anwendung markiert aktivieren.


## <a name="enabling-diagnostic-logs"></a>Aktivieren von Diagnoseprotokollen

Ermöglichen von Diagnoseprotokollen

1. Ihrer Anwendung Blatt klicken Sie auf **Alle**.
2. Klicken Sie auf **Diagnoseprotokollen**. 

Optionen für das Schreiben von Diagnoseprotokollen über eine Anwendung, die eine Protokollierung unterstützt: 

- Die **Anwendung protokolliert**. Anwendungsprotokolle in das Dateisystem geschrieben. Anmeldung für einen Zeitraum von 12 Stunden. 

**Ebene**. Wenn anwendungsprotokollierung aktiviert ist, gibt diese Option an, die Informationen (Fehler, Warnung, Informationen oder Verbose) aufgezeichnet.

**Webserver anmelden**. Protokolle werden in den W3C-Protokolldateiformat gespeichert. 

**Detaillierte Fehlermeldungen**. Speichert Fehlermeldungen detaillierte beim htm-Dateien. Die Dateien werden unter /LogFiles/DetailedErrors gespeichert. 

**Fehler bei Anforderung verfolgen**. Protokolle Fehler Anfragen in XML-Dateien. Die Dateien werden unter /LogFiles/W3SVC*Xxx*gespeichert, wobei Xxx eine eindeutige Kennung. Dieser Ordner enthält eine XSL-Datei und eine oder mehrere XML-Dateien. Stellen Sie sicher, der XSL-Datei, da diese Funktionalität bereitstellt, für die Formatierung und den Inhalt der XML-Dateien filtern.

Zum Anzeigen der Protokolldateien müssen Sie FTP-Anmeldeinformationen wie folgt erstellen:

1. Ihrer Anwendung Blatt klicken Sie auf **Alle**.
2. Klicken Sie auf **Anmeldeinformationen für die Bereitstellung**.
3. Geben Sie einen Benutzernamen und ein Kennwort.
4. Klicken Sie auf **Speichern**.

![Bereitstellung Anmeldeinformationen festlegen][configure03]

Der vollständige FTP-Benutzername ist "App\username" wobei *app* der Name Ihrer Anwendung. Der Benutzername wird im Blatt app Web **Essentials**unter aufgeführt.  

![FTP-Anmeldeinformationen für die Bereitstellung][configure02]

## <a name="other-configuration-tasks"></a>Weitere Konfigurationsaufgaben

### <a name="ssl"></a>SSL 

Basic oder Standard können Sie Zertifikate für eine benutzerdefinierte Domäne hochladen. Weitere Informationen finden Sie unter [HTTPS Web App aktivieren]. 

Die hochgeladene Zertifikate klicken **Alle** > **Custom Domains und SSL**.

### <a name="domain-names"></a>Domänennamen

Benutzerdefinierte Domänennamen Ihrer Anwendung hinzufügen. Weitere Informationen finden Sie unter "Konfigurieren einer benutzerdefinierten Domänennamen für eine Webanwendung in Azure App Service".

Die Domänennamen klicken **Alle** > **Custom Domains und SSL**.

### <a name="deployments"></a>Bereitstellung

- Einrichten der kontinuierlichen Bereitstellung. Siehe [Verwenden Git Web Apps in Azure App Service bereitstellen](./web-sites-deploy.md).
- Deployment-Steckplätze. Siehe [Stagingumgebungen für Web-Apps in Azure App Service bereitstellen].


Ihre Bereitstellung Steckplätze klicken **Alle** > **Bereitstellung Steckplätze**.

### <a name="monitoring"></a>Überwachung

Basic oder Standard können Sie die Verfügbarkeit von HTTP oder HTTPS Endpunkte aus bis zu drei geografisch verteilten Standorten testen. Überwachung Test schlägt fehl, wenn der HTTP-Antwortcode Fehler (4xx oder 5xx) oder die Antwort mehr als 30 Sekunden. Ein Endpunkt gilt gelingt die Überwachung Tests aus den angegebenen Ordnern verfügbar. 

Weitere Informationen finden Sie unter [wie: Überwachen Endpunktstatus Web].

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service]sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren Sie einen benutzerdefinierten Domänennamen in Azure App Service]
- [Aktivieren Sie HTTPS für eine Anwendung in Azure App Service]
- [Eine Webanwendung in Azure App Service skalieren]
- [Grundlagen für Web-Apps in Azure App Service Monitoring]

<!-- URL List -->

[ASP.NET SignalR]: http://www.asp.net/signalr
[Azure-Portal]: https://portal.azure.com/
[Konfigurieren Sie einen benutzerdefinierten Domänennamen in Azure App Service]: ./web-sites-custom-domain-name.md
[Staging-Umgebung für Web-Apps in Azure App Service bereitstellen]: ./web-sites-staged-publishing.md
[Aktivieren Sie HTTPS für eine Anwendung in Azure App Service]: ./web-sites-configure-ssl-certificate.md
[Gewusst wie: Web Endpunktstatus überwachen]: http://go.microsoft.com/fwLink/?LinkID=279906
[Grundlagen für Web-Apps in Azure App Service Monitoring]: ./web-sites-monitor.md
[Pipelinemodus]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Eine Webanwendung in Azure App Service skalieren]: ./web-sites-scale.md
[Socket.IO]: ./web-sites-nodejs-chat-app-socketio.md
[Versuchen Sie App Service]: http://go.microsoft.com/fwlink/?LinkId=523751

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
