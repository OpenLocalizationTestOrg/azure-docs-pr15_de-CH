<properties
    pageTitle="Das Debuggen einer Node.js Web app in Azure App Service"
    description="Informationen Sie zum Debuggen einer Node.js Web app in Azure App Service."
    tags="azure-portal"
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
    ms.author="robmcm"/>

# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a>Das Debuggen einer Node.js Web app in Azure App Service

Azure bietet integrierte Diagnose beim Debuggen Node.js in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps gehostet. In diesem Artikel erfahren Sie, wie Protokollierung Stdout und Stderr Fehlerinformationen im Browser angezeigt und wie zu Protokolldateien anzeigen.

Diagnose für Node.js-Applikationen in Azure wird vom [IISNode]bereitgestellt. Während die häufigsten Einstellungen zum Sammeln von Diagnoseinformationen erläutert, bietet keine vollständige Referenz für die Arbeit mit IISNode. Weitere Informationen zum Arbeiten mit IISNode finden Sie in [IISNode Readme] auf GitHub.

<a id="enablelogging"></a>
## <a name="enable-logging"></a>Aktivieren der Protokollierung

Standardmäßig zeichnet eine App Service Web app nur Diagnoseinformationen zur Bereitstellung wie beim Web-app mit Git bereitstellen. Diese Informationen sind nützlich, wenn Sie Probleme bei der Bereitstellung, z. B. ein Fehler bei der Installation eines Moduls in **package.json**referenziert haben oder bei Verwendung ein benutzerdefinierten Bereitstellungsskripts.

Zum Aktivieren der Protokollierung des Stdout und Stderr muss eine **IISNode.yml** -Datei im Stammverzeichnis der Anwendung Node.js erstellen und fügen Sie Folgendes hinzu:

    loggingEnabled: true

Aktiviert die Protokollierung Stderr und Stdout aus der Node.js-Anwendung.

**IISNode.yml** -Datei kann auch Steuerelement verwendet werden, ob angezeigte oder Entwicklerfehler tritt ein Fehler an den Browser zurückgegeben werden. Um Entwicklerfehler zu aktivieren, fügen Sie die folgende Zeile in die Datei **IISNode.yml** :

    devErrorsEnabled: true

Wenn diese Option aktiviert ist, zurück IISNode letzte 64 KB an Stderr anstelle einer Fehlermeldung wie "ein interner Serverfehler" gesendet.

> [AZURE.NOTE] DevErrorsEnabled beim Diagnostizieren von Problemen während der Entwicklung hilfreich ist, führen aktivieren es in einer produktiven Umgebung Entwicklung Fehler an Endbenutzer.

Die Datei **IISNode.yml** in der Anwendung nicht vorhanden, müssen Sie Ihrer Anwendung neu starten nach dem Veröffentlichen der aktualisierten Anwendung. Wenn Sie einfach in eine vorhandene **IISNode.yml** -Datei ändern, die zuvor veröffentlicht wurde, ist kein Neustart erforderlich.

> [AZURE.NOTE] Ihrer Anwendung mit Befehlszeilentools Azure oder Azure PowerShell-Cmdlets erstellt wurde, wird eine Standard- **IISNode.yml** -Datei automatisch erstellt.

Web app um neu, wählen Sie Web app in [Azure-Portal](https://portal.azure.com), und klicken Sie dann **neu starten** :

![Schaltfläche Starten][restart-button]

Azure-Befehlszeilentools in Umgebung installiert sind, können Sie den folgenden Befehl verwenden Web app neu starten:

    azure site restart [sitename]

> [AZURE.NOTE] LoggingEnabled und DevErrorsEnabled sind die am häufigsten IISNode.yml Optionen zum Erfassen von Diagnoseinformationen verwendeten, IISNode.yml lässt sich eine Vielzahl von Optionen für die Hosting-Umgebung konfigurieren. Eine vollständige Liste der verfügbaren Konfigurationsoptionen finden Sie in der Datei [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) .

<a id="viewlogs"></a>
## <a name="accessing-logs"></a>Zugreifen auf Protokolle

Diagnoseprotokolle können auf drei Arten zugegriffen werden. Mit dem Protokoll FTP (File Transfer), einem Zip-Archiv heruntergeladen oder live aktualisiert Stream des Protokolls (auch bekannt als Schwanz). Das Zip-Archiv der Dateien herunterladen oder Anzeigen von live-Streams erforderlich Azure-Befehlszeilentools. Dies können mithilfe des folgenden Befehls installiert werden:

    npm install azure-cli -g

Nach der Installation der Tools erfolgt mit dem Befehl "Azure". Die Befehlszeilentools müssen zuerst konfiguriert werden Ihre Azure-Abonnement. Informationen zu dieser Aufgabe finden Sie unter der **herunterladen und importieren Einstellungen** Abschnitt [wie Verwenden der Azure-Befehlszeilentools](../xplat-cli-connect.md) .

###<a name="ftp"></a>FTP

Für den Zugriff auf die Diagnoseinformationen über FTP finden Sie auf der [Azure-Portal](https://portal.azure.com)aktivieren Sie Ihrer Anwendung und wählen Sie das **DASHBOARD**. Im Bereich **quick Links** verweisen die **DIAGNOSEPROTOKOLLE für FTP** und **FTPS DIAGNOSEPROTOKOLLE** Zugriff auf die Protokolle über das FTP-Protokoll.

> [AZURE.NOTE] Wenn Sie nicht zuvor Benutzername und Kennwort für FTP oder Bereitstellung konfiguriert haben, können Sie auf Management **QuickStart** dazu auswählen **Bereitstellung Anmeldeinformationen eingerichtet**.

Der FTP-URL zurückgegeben im Dashboard ist **das Verzeichnis, die folgenden Unterverzeichnisse enthalten** :

* [Bereitstellungsmethode](web-sites-deploy.md) - verwenden Sie eine Bereitstellungsmethode wie Git ein Verzeichnis mit demselben Namen erstellt werden, und enthält Informationen zur Bereitstellung.

* Nodejs - Stdout und Stderr Informationen aus allen Instanzen der Anwendung (wenn LoggingEnabled true ist).

###<a name="zip-archive"></a>ZIP-Archiv

Um die Diagnoseprotokolle ein Zip-Archiv herunterzuladen, verwenden Sie den folgenden Befehl aus dem Azure-Befehlszeilentools:

    azure site log download [sitename]

Es wird ein **diagnostics.zip** im aktuellen Verzeichnis herunterladen. Dieses Archiv enthält die folgende Verzeichnisstruktur:

* Bereitstellung - Protokolldatei Informationen zur Bereitstellung der Anwendung

* LogFiles

    * [Bereitstellungsmethode](web-sites-deploy.md) - verwenden Sie eine Bereitstellungsmethode wie Git ein Verzeichnis mit demselben Namen erstellt werden, und enthält Informationen zur Bereitstellung.

    * Nodejs - Stdout und Stderr Informationen aus allen Instanzen der Anwendung (wenn LoggingEnabled true ist).

###<a name="live-stream-tail"></a>Live-Streams (Schwanz)

Um einen Livestream diagnostische Informationen anzuzeigen, verwenden Sie den folgenden Befehl aus dem Azure-Befehlszeilentools:

    azure site log tail [sitename]

Einen Stream von Protokollereignissen gibt zurück, die aktualisiert werden, sobald sie auf dem Server auftreten. Dieser Stream liefert Informationen zur Bereitstellung sowie Stdout und Stderr Informationen (wenn LoggingEnabled true ist).

<a id="nextsteps"></a>
## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt und Diagnoseinformationen für Azure zugreifen. Zwar diese Informationen verstehen Probleme hilfreich, die mit der Anwendung auftreten, kann es auf ein Problem mit einem Modul zeigen Sie verwenden oder die Version von Node.js App Service Web Apps verwendet unterscheidet sich in Ihrer Deployment-Umgebung verwendet.

Informationen an Module auf Azure finden Sie unter [Using Node.js Modules mit Azure](../nodejs-use-node-modules-azure-apps.md).

Informationen zum Angeben einer Node.js-Version der Anwendung finden Sie unter [eine Node.js Azure-Anwendung angeben].

Weitere Informationen finden Sie auch im [Node.js Developer Center](/develop/nodejs/).

## <a name="whats-changed"></a>Was hat sich geändert
* Eine Anleitung zur Änderung von Websites zu App Service finden Sie unter: [Azure App Service und seine Auswirkung auf vorhandene Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

[IISNode]: https://github.com/tjanczuk/iisnode
[IISNode Abschnitt]: https://github.com/tjanczuk/iisnode#readme
[How to Use The Azure Command-Line Interface]: ../xplat-cli-install.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[Angeben einer Version Node.js in Azure-Anwendung]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png
 
