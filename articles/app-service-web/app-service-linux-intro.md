<properties 
    pageTitle="Einführung in App Service unter Linux | Microsoft Azure" 
    description="Enthält Informationen Sie zu App Service unter Linux." 
    keywords="Azure app Service, Linux Betriebssysteme"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="introduction-to-app-service-on-linux"></a>Einführung in App Service unter Linux
App Service unter Linux ist derzeit Public Preview und ausgeführten webapps direkt unter Linux unterstützt. 

## <a name="overview"></a>Übersicht ##
Kunden können App Service unter Linux Host webapps nativ auf Linux für unterstützte Anwendung Stapel. Die folgenden Funktionen sind derzeit unterstützte Anwendung Stapel aufgeführt.

## <a name="features"></a>Funktionen ##
App Service unter Linux unterstützt derzeit die folgenden Anwendung Stapel

- Node.js
- PHP

Kunden können ihre Anwendung mit bereitstellen.

- FTP.
- Lokale Git.
- GitHub oder BitBucket.

Für die Anwendungsskalierung


- Kunden können nach oben und unten skalieren WebApp Ebene in ihrer App Plan ändern. 
- Kunden können ihre Anträge, skalieren und Ausführen ihrer Anwendung über mehrere Instanzen innerhalb ihrer SKU.

Kudu werden einige grundlegende Funktionen

- Umgebung.
- Bereitstellung.
- Basic-Konsole.

## <a name="limitations"></a>Grenzen ##

Azure-Verwaltungsportal derzeit unterstützten Features App für Linux zeigen nur und alles Übrige auszublenden. Als unser Team mehr Features aktivieren werden wir dies Verwaltungsportal Reflektion beibehalten. Einige Funktionen wie VNET-Integration und AAD / Drittanbieter-Authentifizierung oder Kudu Website Extensions derzeit funktionieren nicht. Aber wie wir diese arbeiten werden wir aktualisieren, Dokumentation und Blogs zu ändern.

Öffentliche Vorschau steht derzeit nur in den folgenden Bereichen

-   Westen der USA.
-   Westeuropa.
-   Südostasien.

WebApp unter Linux nur unterstützt dedizierte App Service-Pläne und keinen frei oder Shared-Ebene. Auch gegenseitig app Pläne für reguläre und Linux webapps, daher können Sie in einem nicht-Linux app Service-Plan Linux Web app erstellen.

WebApp unter Linux muss in einer Ressourcengruppe erstellt werden, die nicht-Linux webapps in derselben Region enthält.

Mangels überlappende Wiederverwendung von Web apps sollte erwarten, dass kleine Ausfallzeiten bei Web app gestartet haben. 

## <a name="next-steps"></a>Nächste Schritte ##

Führen Sie die folgenden Links Einstieg in App Service unter Linux. Bitte veröffentlichen Sie Fragen im [Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Erstellen von Web-Apps in App auf Linux](./app-service-linux-how-to-create-a-web-app.md)
* [Verwenden der PM2-Konfiguration für Node.js in Web-Apps unter Linux](./app-service-linux-using-nodejs-pm2.md)

