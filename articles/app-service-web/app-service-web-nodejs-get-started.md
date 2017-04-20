<properties
    pageTitle="Erste Schritte mit Node.js webapps in Azure App Service"
    description="Erfahren Sie, wie eine Anwendung Node.js zu Web app in Azure App Service bereitgestellt."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="get-started-article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Erste Schritte mit Node.js webapps in Azure App Service

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Dieses Lernprogramm zeigt, wie eine einfache [Node.js] -Anwendung erstellen und von einer Befehlszeilen-Umgebung wie cmd.exe [Azure App Service] bereitstellen oder bash. Die Anleitung in diesem Lernprogramm können auf jedem Betriebssystem folgen, die Node.js ausgeführt werden kann.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

<a name="prereq"></a>
## <a name="prerequisites"></a>Erforderliche Komponenten

- [Node.js]
- [Bower]
- [Yeoman]
- [Git]
- [Azure CLI]
- Ein Microsoft Azure-Konto. Haben Sie ein Konto, können Sie [sich für eine kostenlose Testversion] oder [die Visual Studio-Abonnementvorteile aktivieren].

## <a name="create-and-deploy-a-simple-nodejs-web-app"></a>Erstellen und Bereitstellen einer einfachen Node.js Web-Anwendung

1. Befehlszeile Terminal Ihrer Wahl öffnen und [Generator für Yeoman Express]installieren.

        npm install -g generator-express

2. `CD`in einem Verzeichnis und eine express-Anwendung mithilfe der folgenden Syntax:

        yo express
        
    Optionen Sie die folgenden Aufforderung:  

    `? Would you like to create a new directory for your project?`**Ja**  
    `? Enter directory name`**{Appname}**  
    `? Select a version to install:`**MVC**  
    `? Select a view engine to use:`**Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):`**Keine**  
    `? Select a database to use:`**Keine**  
    `? Select a build tool to use:`**Schwierige**

3. `CD`in das Stammverzeichnis der neuen Anwendung und sicherzustellen, dass in der Umgebung ausgeführt:

        npm start

    Navigieren Sie in Ihrem Browser zu <http://localhost: 3000> zu Express-Homepage angezeigt. Sobald Sie Ihre app wird ordnungsgemäß überprüft haben, können `Ctrl-C` anhalten.
    
1. (Sie benötigen [Azure CLI](#prereq)) Azure anzumelden und ASM-Modus zu ändern:

        azure config mode asm
        azure login

    Folgen Sie der Aufforderung weiterhin den Benutzernamen in einem Browser mit einem microsoftkonto, das Ihr Azure-Abonnement verfügt.

2. Überprüfen Sie sind trotzdem im Stammverzeichnis der Anwendung und die App Service app Ressource in Azure mit einer eindeutigen Anwendungsname den nächsten Befehl zu erstellen. Beispiel: http://{appname}.azurewebsites.net

        azure site create --git {appname}

    Führen Sie die Aufforderung Azure Region bereitstellen zu wählen. Wenn Sie für Ihre Azure-Abonnement nie Git/FTP-Bereitstellung Anmeldeinformationen eingerichtet haben, werden Sie auch aufgefordert erstellen.

3. Öffnen Sie die Datei./config/config.js aus dem Stammverzeichnis der Anwendung und ändern den Produktion Port `process.env.port`; die `production` -Eigenschaft in der `config` Objekt sieht wie folgt aus:

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    Dadurch Node.js app reagieren, um web-Anfragen den Standardport, Iisnode überwacht.
    
4. Öffnen Sie./package.json und fügen die `engines` -Eigenschaft [die gewünschte Node.js-Version](#version).

        "engines": {
            "node": "6.6.0"
        }, 

4. Speichern und Bereitstellen Ihrer Anwendung in Azure Git verwenden:

        git add .
        git commit -m "{your commit message}"
        git push azure master

    Express-Generator enthält bereits eine .gitignore Datei, sodass die `git push` der Node_modules uploaden Bandbreite verbraucht / Directory.

5. Schließlich starten Sie Ihre live Azure app im Browser:

        azure site browse

    Leben in Azure App Service ausgeführt Node.js Web-app sollte jetzt angezeigt werden.
    
    ![Beispiel für die bereitgestellte Anwendung durchsuchen.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Aktualisieren Sie Ihrer Anwendung Node.js

Um Updates zu Ihrer App Service ausgeführt Node.js Anwendung machen, führen Sie einfach `git add`, `git commit`, und `git push` wie bei der ersten Bereitstellung Ihrer Anwendung.
     
## <a name="how-app-service-deploys-your-nodejs-app"></a>Wie App Service Ihre app Node.js bereitstellt

Azure App Service mithilfe [Iisnode] ausgeführt Node.js apps. Azure-CLI und das Kudu-Modul (Git Deployment) zusammenarbeiten, um Ihnen eine optimierte Erfahrung beim Entwickeln und Bereitstellen von Node.js-apps in der Befehlszeile. 

- `azure site create --git`Allgemeine Node.js Muster eines server.js oder app.js erkennt und ein iisnode.yml im Root-Verzeichnis erstellt. Diese Datei können Sie die Iisnode anpassen.
- Am `git push azure master`, Kudu folgende Bereitstellungsaufgaben automatisiert:

    - Wenn package.json in das Repository-Stammverzeichnis ist `npm install --production`.
    - Erstellen Sie eine Web.config-Datei auf Ihr Skript starten in package.json (server.js oder app.js) Iisnode.
    - Passen Sie Web.config, um Ihre Anwendung Debuggen mit Knoten Inspektor bereit.
    
## <a name="use-a-nodejs-framework"></a>Node.js-Framework verwendet

Verwenden Sie eine beliebte Node.js-Framework wie [Sails.js] [ SAILSJS] oder [MEAN.js] [ MEANJS] entwickeln apps können die App Service bereitstellen. Beliebte Node.js-Frameworks haben ihre spezifischen Eigenarten und kompilierbar Paket aktualisiert zu halten. Jedoch wird App Service Stdout und Stderr Protokolle Sie wissen genau, was Ihre App passiert und Änderungen vornehmen. Weitere Informationen finden Sie unter [Stdout und Stderr Protokolle von Iisnode](#iisnodelog).

Die folgenden Lernprogramme zeigen Ihnen, wie ein bestimmtes Framework in App Service arbeiten:

- [Bereitstellen Sie Sails.js Web app für Azure App Service]
- [Erstellen Sie eine Chat-Anwendung Node.js mit Socket.IO in Azure App Service]
- [Verwendung von io.js mit Azure App Service Web Apps]

<a name="version"></a>
## <a name="use-a-specific-nodejs-engine"></a>Verwenden Sie ein bestimmtes Node.js-Modul

In den normalen Workflow weisen Sie App Service eine bestimmte Node.js-Engine verwenden Sie wie gewohnt in package.json.
Zum Beispiel:

    "engines": {
        "node": "6.6.0"
    }, 

Das Bereitstellungsmodul Kudu bestimmt die Node.js-Software in der folgenden Reihenfolge:

- Betrachten Sie zunächst iisnode.yml finden Sie unter `nodeProcessCommandLine` angegeben ist. Wenn Ja, dann verwenden.
- Als Nächstes betrachten package.json finden Sie unter `"node": "..."` wird gemäß der `engines` Objekt. Wenn Ja, dann verwenden.
- Wählen Sie eine standardmäßige Node.js standardmäßig.

>[AZURE.NOTE] Es wird empfohlen, dass Sie explizit Node.js-Modul definieren. Die Standardversion Node.js können und Sie erhalten Fehler in Ihrer Anwendung Azure da Node.js-Standardversion nicht für Ihre Anwendung geeignet ist.

<a name="iisnodelog"></a>
## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>Iisnode Stdout und Stderr Protokolle erhalten

Gehen Sie folgendermaßen vor, um Iisnode Protokolle lesen.

> [AZURE.NOTE] Nach Abschluss dieser Schritte möglicherweise Protokolldateien nicht vorhanden, bis ein Fehler auftritt.

1. Die Datei iisnode.yml, die die CLI Azure bietet.

2. Festlegen Sie die zwei folgenden Parameter: 

        loggingEnabled: true
        logDirectory: iisnode
    
    Gemeinsam Erzählen sie Iisnode in App Service der Stdout und Stderror in der D:\home\site\wwwroot ausgegeben\**Iisnode** Verzeichnis.

3. Speichern Sie, und drücken Sie die Änderungen in Azure mit den folgenden Befehlen Git:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    Iisnode ist jetzt konfiguriert. Die nächsten Schritte zeigen, wie diese Protokolle auf.
     
4. In Ihrem Browser Zugriff auf Kudu der Debug-Konsole für Ihre Anwendung an:

        https://{appname}.scm.azurewebsites.net/DebugConsole 

    Diese URL unterscheidet sich von Web app-URL hinzufügen "*haben.SCM.*" DNS-Namen. Wenn, dazu die URL auslassen erhalten Sie Fehler 404.

5. Navigieren Sie zu D:\home\site\wwwroot\iisnode

    ![Navigieren zum Speicherort der Protokolldateien Iisnode.][iislog-kudu-console-find]

6. Klicken Sie auf das Symbol " **Bearbeiten** " für das Protokoll zu lesen. Sie können auch **herunterladen** oder **Löschen** klicken, wenn Sie möchten.

    ![Öffnen einer Protokolldatei Iisnode.][iislog-kudu-console-open]

    Jetzt sehen Sie das Protokoll zu Debuggen die App Service-Bereitstellung.
    
    ![Untersuchen eine Protokolldatei Iisnode.][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Debuggen Sie Ihrer Anwendung mit Knoten-Inspektor

Wenn Sie Knoten Inspektor Node.js apps Debuggen verwenden, können Sie es für Ihre live App Service. Knoten-Inspektor ist in der Installation Iisnode App Service vorinstalliert. Und Bereitstellung über Git automatisch generierten Web.config aus Kudu bereits das gesamte Konfiguration, die Sie Knoten Inspektor aktivieren müssen.

Um Knoten Inspektor zu aktivieren, gehen Sie folgendermaßen vor:

1. Öffnen Sie iisnode.yml Stammverzeichnis Repository, und geben Sie folgende Parameter: 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. Speichern Sie, und drücken Sie die Änderungen in Azure mit den folgenden Befehlen Git:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. Nur navigieren Sie nun zur app Startdatei Skript starten Ihre package.json mit/Debug zur URL hinzugefügt. Zum Beispiel

        http://{appname}.azurewebsites.net/server.js/debug
    
    Oder
    
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Weitere Ressourcen

- [Angeben einer Version Node.js in Azure-Anwendung](../nodejs-specify-node-version-azure-apps.md)
- [Best Practices und Fehlerbehebung für Node.js Applikationen auf Azure](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
- [Das Debuggen einer Node.js Web app in Azure App Service](web-sites-nodejs-debug.md)
- [Node.js-Module verwenden mit Azure](../nodejs-use-node-modules-azure-apps.md)
- [Azure App Service webapps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Node.js-Entwicklercenter](/develop/nodejs/)
- [Erste Schritte mit Web-apps in Azure App Service](app-service-web-get-started.md)
- [Großes Geheimnis Kudu der Debug-Konsole durchsuchen]

<!-- URL List -->

[Azure CLI]: ../xplat-cli-install.md
[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[Aktivieren Sie Ihre Visual Studio-Abonnementvorteile]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Erstellen Sie eine Chat-Anwendung Node.js mit Socket.IO in Azure App Service]: ./web-sites-nodejs-chat-app-socketio.md
[Bereitstellen Sie Sails.js Web app für Azure App Service]: ./app-service-web-nodejs-sails.md
[Großes Geheimnis Kudu der Debug-Konsole durchsuchen]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Generator für Yeoman Express]: https://github.com/petecoop/generator-express
[Git]: http://www.git-scm.com/downloads
[Verwendung von io.js mit Azure App Service Web Apps]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[Melden Sie sich für eine kostenlose Testversion]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png
