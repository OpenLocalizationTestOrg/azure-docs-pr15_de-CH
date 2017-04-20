<properties 
    pageTitle="Eine Webanwendung in Azure App Service verwalten" 
    description="Links zu Ressourcen für die Verwaltung von Web-app in Azure App Service." 
    services="app-service\web" 
    documentationCenter="" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="rachelap"/>

# <a name="manage-a-web-app-in-azure-app-service"></a>Eine Webanwendung in Azure App Service verwalten

Dieses Thema enthält Links zu Ressourcen für die Verwaltung von Web-app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Management umfasst alle Aufgaben, die Ihrer Anwendung reibungslos zu halten. 

Im Laufe einer Web App führen Sie verschiedene Verwaltungsaufgaben anfänglichen Bereitstellung normalen Betrieb, Wartung und Updates verschieben.

Viele Web app Verwaltungsaufgaben können im Azure-Portal ausgeführt werden.

## <a name="before-you-deploy-your-web-app-to-production"></a>Vor der Bereitstellung Ihrer Anwendung für die Produktion

### <a name="choose-a-tier"></a>Wählen Sie eine Ebene

Azure App angeboten auf fünf Ebenen: Shared, Basic, Standard und Premium frei. Informationen zu den Funktionen und Preisen für jede Ebene finden Sie unter [Preise](/pricing/details/app-service/). 

- [App Service-Pläne](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) können Sie mehrere Web apps unter der gleichen Ebene.
- Sie können jederzeit [wechseln Ebenen](web-sites-scale.md) nach Ihrer Anwendung erstellen.

### <a name="configuration"></a>Konfiguration

Mit der [Azure-Portal](https://portal.azure.com/) Optionen fest. Details finden Sie unter [Configure webapps in Azure App Service](web-sites-configure.md). Hier ist eine kurze Checkliste:

- Wählen Sie ggf. für .NET, PHP, Java oder Python **Laufzeitversionen** aus.
- Aktivieren Sie **WebSockets** Ihrer Anwendung das WebSocket-Protokoll verwendet. (Einschließlich apps, die [ASP.NET SignalR](http://www.asp.net/signalr) oder [socket.io](web-sites-nodejs-chat-app-socketio.md).)
- Verwenden Sie fortlaufende Webaufträge? In diesem Fall aktivieren Sie **Immer auf**.
- Legen Sie das **Standarddokument**wie index.html.

Neben diesen grundlegenden Konfigurationen sollten Sie Folgendes konfigurieren:

- **Secure Socket Layer (SSL)** -Verschlüsselung. Verwendung von SSL mit einem benutzerdefinierten Domänennamen müssen Sie ein SSL-Zertifikat und Konfigurieren Ihrer Anwendung zu verwenden. [HTTPS aktivieren für eine Webanwendung in Azure App Service](web-sites-configure-ssl-certificate.md)anzeigen
- **Domain-Namen.** Ihrer Anwendung wurde automatisch eine Subdomäne unter *.azurewebsites.NET. Sie können einen benutzerdefinierten Domänennamen, z. B. contoso.com zuordnen. [Konfigurieren einer benutzerdefinierten Domänennamen in Azure App Service](web-sites-custom-domain-name.md)anzeigen

Sprachspezifische Konfiguration:

- **PHP**: [PHP in Azure App Service webapps konfigurieren](web-sites-php-configure.md).
- **Python**: [Konfigurieren von Python mit Azure App Service webapps](web-sites-python-configure.md)


## <a name="while-your-web-app-is-running"></a>Während der Ausführung Ihrer Anwendung

Während der Ausführung Ihrer Anwendung verfügbar ist, und, um Benutzer Verkehr skaliert werden soll. Sie müssen auch Fehler beheben.

### <a name="monitoring"></a>Überwachung

- Azure-Portal können Sie [Leistungsindikatoren hinzufügen](web-sites-monitor.md) , wie CPU-Auslastung und Anzahl der Clientanforderungen.
- [Skalierung Ihrer Anwendung](web-sites-scale.md) auf Datenverkehr. Je nach der Ebene können Sie die Anzahl der VMs und der Größe der VM-Instanzen skalieren. Im Standard- und Premium-Ebenen können Sie auch automatische Skalierung, so einrichten Ihrer Anwendung skaliert automatisch, nach einem festen Zeitplan oder auf laden.  
 
### <a name="backups"></a>Backups

- [Automatische Sicherung](web-sites-backup.md) Ihrer Anwendung festlegen. Erfahren Sie mehr über Backups in [diesem Video](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).
- Erfahren Sie mehr über die Optionen für die [Wiederherstellung der Datenbank](../sql-database/sql-database-business-continuity.md) in Azure SQL-Datenbank.

### <a name="troubleshooting"></a>Problembehandlung

- Wenn etwas schief geht, können Sie [in Visual Studio behandeln](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)mit Diagnoseprotokolle Livedebuggen in der Cloud. 
- Außerhalb von Visual Studio sind verschieden Diagnoseprotokolle sammeln. [Aktivieren Sie das Diagnoseprotokoll für Web-apps in Azure App Service](web-sites-enable-diagnostic-log.md)anzeigen
- Node.js-Anwendung finden Sie unter [Debuggen eine Node.js Web app in Azure App Service](web-sites-nodejs-debug.md).

### <a name="restoring-data"></a>Wiederherstellen von Daten

- [Wiederherstellen](web-sites-restore.md) einer zuvor gesicherten Web-app.


## <a name="when-you-update-your-web-app"></a>Beim Aktualisieren Ihrer Anwendung

Wenn Sie automatische Backups nicht aktiviert haben, können Sie eine [manuelle Sicherung](web-sites-backup.md)erstellen.

Erwägen Sie [stufenweise Bereitstellung](web-sites-staged-publishing.md). Diese Option staging-Bereitstellung Updates veröffentlicht, die Side-by-Side führt mit der Bereitstellung in der Produktion. 

Wenn Sie Visual Studio Team Services verwenden, können Sie kontinuierliche Bereitstellung vom Datenquellen-Steuerelement einrichten:

- [Mithilfe von Team Foundation-Versionskontrolle (TFVC)](../cloud-services/cloud-services-continuous-delivery-use-vso.md) 
- [Git verwenden](../cloud-services/cloud-services-continuous-delivery-use-vso-git.md)
 
<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website

  
