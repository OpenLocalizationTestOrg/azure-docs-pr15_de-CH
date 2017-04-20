<properties 
    pageTitle="Verwendung von io.js mit Azure App Service Web Apps" 
    description="Erfahren Sie mehr über das Web app in Azure App Service mit io.js verwenden." 
    services="app-service\web" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016"
    ms.author="robmcm" />

# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>Verwendung von io.js mit Azure App Service Web Apps

Beliebte Knoten Gabel [io.js] bietet verschiedene Unterschiede des Joyent Node.js Projekt, einschließlich offener Prozessführungsmodell, eine schnellere Releasezyklus und schnellere Einführung neuer und experimentelle JavaScript-Funktionen.

Während [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web-Apps viele Node.js Versionen vorinstalliert ist, können auch für eine vom Benutzer bereitgestellte Node.js Binärdatei. Dieser Artikel beschreibt zwei Methoden, die die Verwendung des io.js auf App Service Web Apps: die Verwendung eines erweiterten Bereitstellung, das Azure um die neueste Version verfügbar io.js, sowie der manuellen Upload einer binären io.js automatisch konfiguriert. 

<a id="deploymentscript"></a>
## <a name="using-a-deployment-script"></a>Bereitstellungsskript verwenden

Bei der Bereitstellung einer Anwendung Node.js führt App Service Web Apps kleine Befehle um sicherzustellen, dass die Umgebung ordnungsgemäß konfiguriert ist. Bereitstellungsskript verwenden, kann dieser Prozess den Download und Konfiguration der io.js angepasst werden.

[Io.js Bereitstellungsskript](https://github.com/felixrieseberg/iojs-azure) ist auf GitHub verfügbar. Io.js in Ihrer Anwendung, kopieren Sie einfach **darauf verweist das** **deploy.cmd** und **IISNode.yml** in das Stammverzeichnis der Anwendungsordner und Web Apps bereitstellen.  

Die erste Datei, **darauf verweist das**weist Web Apps **deploy.cmd** bei der Bereitstellung führen. Dieses Skript führt die üblichen Schritte zum Node.js-Anwendung jedoch auch downloads die neueste Version von io.js. Schließlich konfiguriert **IISNode.yml** Web Apps nur die heruntergeladenen io.js binäre statt vorinstallierte Node.js Binärdateien verwenden.

> [AZURE.NOTE] Aktualisieren der Binärdatei verwendeten io.js, einfach erneut die Anwendung - downloadet das Skript eine neue Version der io.js jedes Mal, der die Anwendung bereitgestellt wird.

<a id="manualinstallation"></a>
## <a name="using-manual-installation"></a>Verwenden der manuellen Installation

Die manuelle Installation von benutzerdefinierten io.js Version umfasst zwei Schritte. Zuerst herunterladen Sie binäre **Win-X64** direkt von der [io.js Verteilung] Erforderlich sind zwei Dateien: **iojs.exe** und **iojs.lib**. Speichern Sie beide Dateien in einem Ordner innerhalb Ihrer Anwendung, z. B. in das **Bin-Iojs**.

Konfigurieren Sie Web-Apps um eine vorinstallierte Version von Knoten **iojs.exe** anstelle erstellen Sie eine **IISNode.yml** -Datei im Stammverzeichnis der Anwendung und fügen Sie folgende Zeile.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>
## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel gelernt, verwenden bereitgestellten io.js mit App Service Web Apps mit Bereitstellungsskripts manuellen Installation. 

> [AZURE.NOTE] IO.js ist in Entwicklung und häufiger Node.js aktualisiert. Anzahl Node.js-Module funktionieren möglicherweise nicht mit io.js - Bitte finden Sie in [io.js auf GitHub] zur Problembehandlung.

## <a name="whats-changed"></a>Was hat sich geändert
* Eine Anleitung zur Änderung von Websites zu App Service finden Sie unter: [Azure App Service und seine Auswirkung auf vorhandene Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

[IO.js]: https://iojs.org
[IO.js Verteilung]: https://iojs.org/dist/
[IO.js auf GitHub]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
 