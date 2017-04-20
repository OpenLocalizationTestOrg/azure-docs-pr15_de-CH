<properties
    pageTitle="Erstellen Sie eine Chat-Anwendung Node.js mit Socket.IO in Azure App Service"
    description="Ein Lernprogramm demonstriert die Verwendung von socket.io in node.js Web app auf Azure gehostet."
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

# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Erstellen Sie eine Chat-Anwendung Node.js mit Socket.IO in Azure App Service

Socket.IO bietet Echtzeitkommunikation zwischen node.js Server und Clients mit WebSockets. Zu anderen Transporte (z. B. long Polling), die mit älteren Browsern unterstützt. Dieses Lernprogramm ein Socket.IO basierte Chat-Anwendung als Azure Web app durchgehen und zeigen, wie die Anwendung [Azure Redis Cache]skaliert. Weitere Informationen zu Socket.IO finden Sie unter <http://socket.io/>.

> [AZURE.NOTE] Die Verfahren in dieser Aufgabe gelten für [App Service Web Apps]. Cloud-Dienste finden Sie unter [Erstellen einer Node.js Chat-Anwendung mit Socket.IO Azure Cloud Service].

## <a name="download-the-chat-example"></a>Laden Sie das Chat-Beispiel

Für dieses Projekt wird Chat Beispiel [Socket.IO GitHub Repository]verwendet. Die folgenden Schritte das Beispiel herunterladen und dem zuvor erstellten Projekt hinzufügen.

1.  Herunterladen einer [ZIP- oder GZ archivierte Version] des Projekts Socket.IO (Version 1.3.5 wurde für dieses Dokument verwendet)

1.  Extrahieren Sie das Archiv und die Kopie der **Beispiele\\Chat** Verzeichnis an einen neuen Speicherort. Beispielsweise ** \\Knoten\\Chat**.

## <a name="modify-appjs-and-install-modules"></a>Ändern von app.js und Module installieren

1.  Benennen Sie die Datei **index.js** in **app.js**. Dies ermöglicht Azure erkennen, dass dies eine Node.js-Anwendung.

1.  Öffnen Sie die Datei **app.js** in einem Texteditor. Ändern Sie die Zeile mit `var io = require('../..')(server);` wie folgt:

        var express = require('express');
        var app = express();
        var server = require('http').createServer(app);
        // var io = require('../..')(server);
        // New:
        var io = require('socket.io')(server);
        var port = process.env.PORT || 3000;

1. Öffnen Sie die Datei **package.json** und fügen einen Verweis auf socket.io unter `dependencies`, wie nachfolgend dargestellt:

        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }

1. Ändern Sie in der Befehlszeile in der ** \\Knoten\\Chat** Npm Verzeichnis und verwenden diese Anwendung erforderlichen Module installiert:

        npm install

    Dies installiert die Module in einem Unterordner mit dem Namen **Node_modules**.

## <a name="create-an-azure-web-app"></a>Azure Web App erstellen

Gehen Sie Azure Web app erstellen, Git Veröffentlichung aktivieren und anschließend aktivieren WebSocket-Unterstützung für Web-app.

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure-Testversion</a>.

1. Installieren Sie der Azure-Befehlszeilenschnittstelle (CLI Azure) und Azure-Abonnement an. [Installieren und Konfigurieren von Azure CLI](../xplat-cli-install.md)anzeigen

1. Ist dies der erste Systemzeit Repository in Azure, müssen Sie Anmeldeinformationen erstellen. Geben Sie von der CLI Azure den folgenden Befehl ein:

        azure site deployment user set [username] [password]

1. Ändern Sie in der ** \\Node\chat** Verzeichnis und verwenden den folgenden zum Erstellen einer neuen Azure Befehl web app und ein lokales Git Repository. Dieser Befehl erstellt außerdem eine Git remote benannte "Azure".

        azure site create mysitename --git

    Sie müssen 'Mysitename' mit einem eindeutigen Namen für Ihre Web-app ersetzen.

1. Bestätigen Sie die vorhandenen Dateien im lokalen Repository mithilfe der folgenden Befehle:

        git add .
        git commit -m "Initial commit"

1. Drücken Sie die Dateien im Repository Azure Web Apps mit dem folgenden Befehl:

        git push azure master

    Wenn Sie aufgefordert werden, geben Sie Ihre Anmeldeinformationen aus Schritt 2. Sie erhalten Statusnachrichten Module auf den Server importiert werden. Nach Abschluss dieses Vorgangs wird die Anwendung auf Ihrer Azure Anwendung gehostet werden.

    > [AZURE.NOTE] Während der Installation des Moduls können Sie Fehler feststellen, "... das importierte Projekt wurde nicht gefunden". Dies können ignoriert werden.

1. Socket.IO verwendet WebSockets Azure standardmäßig nicht aktiviert. Um Web Sockets zu aktivieren, verwenden Sie den folgenden Befehl ein:

        azure site set -w

    Wenn Sie aufgefordert werden, geben Sie den Namen Web App.

    >[AZURE.NOTE]
    >"Azure Site set -w-Befehl wird nur mit Version 0.7.4 oder höher der Azure-Befehlszeilenschnittstelle. Sie können auch mithilfe der [Azure-Portal](https://portal.azure.com)WebSocket-Unterstützung.
    >
    >Um WebSockets mithilfe der Azure-Portal aktivieren möchten, klicken Sie auf Webanwendung aus dem Web Apps Blade, **Alle** > **ApplicationSettings**. **Klicken Sie unter **Web Sockets**auf.** Klicken Sie auf **Speichern**.

1. Um Azure Web app anzuzeigen, verwenden Sie folgenden Befehl auf den Webbrowser, und navigieren Sie zu der gehosteten Web app:

        azure site browse

Ihre app in Azure ausgeführt wird und Nachrichten zwischen verschiedenen Clients mit Socket.IO weiterleiten kann.

## <a name="scale-out"></a>Dezentrales Skalieren

Socket.IO Applikationen können mit einem __Adapter__ zum Verteilen von Nachrichten und zwischen mehreren Instanzen skaliert werden. Zwar gibt es verschiedene Adapter, kann [socket.io Redis] Adapter problemlos mit Azure Redis Cache-Funktion verwendet.

> [AZURE.NOTE] Eine weitere Anforderung Socket.IO Projektmappen skalieren unterstützt sticky Sessions. Sticky Sessions sind für Azure Web Apps durch Azure Routing Anforderung standardmäßig aktiviert. Weitere Informationen finden Sie unter [Affinität in Azure-Websites].

### <a name="create-a-redis-cache"></a>Erstellen einer Redis

Schritte in [einem Cache in Azure Redis Cache erstellen] einen neuen Cache zu erstellen.

> [AZURE.NOTE] Speichern Sie __Hostnamen__ und __Primärschlüssel__ für den Cache wie diese in den nächsten Schritten benötigen.

### <a name="add-the-redis-and-socketio-redis-modules"></a>Redis und Redis socket.io Module hinzufügen

1. Ändern von einer Befehlszeile aus in die __ \\Knoten\\Chat__ Verzeichnis und mit dem folgenden Befehl.

        npm install socket.io-redis@0.1.4 redis@0.12.1 --save

    > [AZURE.NOTE] In diesem Befehl angegebenen Versionen sind die Versionen beim Testen dieser Artikel verwendet.

1. Der __app.js__ Datei die folgenden Zeilen direkt nach`var io = require('socket.io')(server);`

        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});

        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));

    Ersetzen Sie __Redishostname__ und __Rediskey__ mit dem Hostnamen und Schlüssel für die Redis-Cache.

    Erstellt einen veröffentlichen und abonnieren Client zuvor erstellte Redis-Cache. Clients werden dann mit dem Adapter zum Socket.IO Verwendung Redis Cache für Nachrichten und Ereignisse zwischen den Instanzen der Anwendung konfigurieren

    > [AZURE.NOTE] Der Adapter __socket.io Redis__ an Redis kommunizieren kann, unterstützt die aktuelle Version nicht Azure Redis Cache erforderliche Authentifizierung. So wird die Verbindung mit dem Modul __redis__ aufgebaut und Client __socket.io Redis__ Adapter übergeben.
    >
    > Azure Redis Cache sichere Verbindung über Port 6380 unterstützt in diesem Beispiel verwendeten Module nicht sichere Verbindungen 2014 7/14/unterstützt. Der obige Code verwendet die Standard, unsicheren Port 6379.

1. Speichern Sie die geänderten __app.js__

### <a name="commit-changes-and-redeploy"></a>Änderungen und erneut bereitstellen

In der Befehlszeile in der __ \\Knoten\\Chat__ Verzeichnis, verwenden Sie die folgenden Befehle ändern und die Anwendung erneut bereitstellen.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Sobald die Änderung auf dem Server abgelegt wurde, können Sie Ihre Website mithilfe des folgenden Befehls über mehrere Instanzen skalieren.

    azure site scale instances --instances #

Wo __#__ ist die Anzahl der Instanzen zu erstellen.

Verbindung zu Ihrer Anwendung über mehrere Browser oder Computer überprüfen, ob Nachrichten ordnungsgemäß an alle Clients gesendet werden.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="connection-limits"></a>Verbindungslimits

Azure Web Apps steht in mehrere SKUs, die auf der Site verfügbaren Ressourcen zu bestimmen. Diese enthält die Anzahl der zulässigen WebSocket-Verbindung. Weitere Informationen finden Sie auf der [Seite Web Apps Preise].

### <a name="messages-arent-being-sent-using-websockets"></a>Nachrichten werden nicht versendet mit WebSockets

Wenn Clientbrowser zurück halten anstatt WebSockets long polling, möglicherweise wegen einer der folgenden.

* **Versuchen Sie, den Transport, WebSockets nur eingeschränkt**

    Damit Socket.IO WebSockets messaging Transport verwenden müssen Server und Client WebSockets unterstützen. Eine nicht verhandeln Socket.IO einen anderen Transport wie long Polling. Die Standardliste der Transporte von Socket.IO verwendet ` websocket, htmlfile, xhr-polling, jsonp-polling`. Sie können erzwingen nur WebSockets durch den folgenden Code nach der Zeile mit der Datei **app.js** hinzufügen `, nicknames = {};`.

        io.configure(function() {
          io.set('transports', ['websocket']);
        });

    > [AZURE.NOTE] Beachten Sie, dass ältere Browser, die keine WebSockets unterstützen das während der obige Code aktiv ist, da nur Kommunikation mit WebSockets beschränkt Website herzustellen.

* **SSL verwenden**

    WebSockets basiert auf einige weniger verwendete HTTP-Header, wie der **Upgrade** -Header. Einige zwischengeschaltete Netzwerkgeräte wie Web-Proxys können diese Header entfernen. Zur Vermeidung dieses Problems können Sie die WebSocket-Verbindung über SSL herstellen.

    Eine einfache Möglichkeit dazu ist Socket.IO zu konfigurieren `match origin protocol`. Dies weist Socket.IO zum Sichern der Kommunikation WebSockets sendenden HTTP/HTTPS-Anforderung für die Webseite identisch. Browser HTTPS-URL verwendet, die Ihre Website besuchen, werden nachfolgende WebSocket-Kommunikation über Socket.IO über SSL gesichert.

    Ändern Sie dabei zum Aktivieren dieser Konfiguration fügen Sie den folgenden Code in die Datei **app.js** nach der Zeile mit `, nicknames = {};`.

        io.configure(function() {
          io.set('match origin protocol', true);
        });

* **Überprüfen Sie web.config settings**

    Azure webapps, die Node.js-Anwendung hosten verwenden zum Weiterleiten von Anfragen an Node.js-Anwendung die Datei **web.config** . Für WebSockets ordnungsgemäß mit Node.js-Anwendung muss **web.config** den folgenden Eintrag enthalten.

        <webSocket enabled="false"/>

    IIS WebSockets Modul beinhaltet eine eigene Implementierung von WebSockets und Konflikte mit bestimmten WebSocket-Module wie Socket.IO Node.js deaktiviert. Wenn diese Zeile nicht vorhanden ist oder wird `true`, dies ist möglicherweise der Grund für die WebSocket-Transport für die Anwendung nicht funktioniert.

    In der Regel umfassen Node.js Applikationen damit Azure Websites automatisch für Node.js Applikationen generiert werden keine **web.config** -Datei. Da diese Datei automatisch auf dem Server generiert wird, müssen Sie zum Anzeigen dieser Datei FTP oder FTPS URL für Ihre Website verwenden. FTP und FTPS URLs finden für Ihre Website im klassischen Portal Sie durch Auswählen von Ihrer Anwendung und die **Dashboard** -Verknüpfung. Die URLs werden in Abschnitt **kurz** angezeigt.

    > [AZURE.NOTE] Wenn Ihre Anwendung nicht zur Verfügung stellt nur die Datei **web.config** von Azure Websites generiert. Wenn Sie eine Datei **web.config** im Stammverzeichnis der Anwendungsprojekt bereitstellen, wird von Azure Web Apps verwendet werden.

    Wenn der Eintrag nicht vorhanden ist, oder den Wert `true`, sollten Sie **"Web.config"** im Stammverzeichnis der Anwendung Node.js erstellen und einen Wert von `false`.  Für Referenz der unten ist eine standardmäßige **web.config** für eine Anwendung, die **app.js** als Einstiegspunkt verwendet.

        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:

             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->

        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>

                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>

                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled

              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>

    Wenn die Anwendung einen Einstiegspunkt als **app.js**verwendet, müssen Sie alle Vorkommen der **app.js** mit den richtigen Eintrag ersetzen. **Server.js**beispielsweise ersetzen **app.js** .

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service]sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie eine Chat-Anwendung in einer Azure Web app erstellen. Sie können auch diese Anwendung als Azure Cloud-Dienst hosten. Schritte dazu finden Sie unter [Erstellen einer Node.js Chat-Anwendung mit Socket.IO Azure Cloud Service].

Weitere Informationen finden Sie auch im [Node.js Developer Center].

## <a name="whats-changed"></a>Was hat sich geändert

* Eine Anleitung zur Änderung von Websites zu App Service finden Sie unter: [Azure App Service und seine Auswirkung auf vorhandene Azure Services].

<!-- URL List -->

[Azure Redis Cache]: /documentation/services/redis-cache/
[App Service webapps]: http://go.microsoft.com/fwlink/?LinkId=529714
[Webseite Apps Preise]: http://go.microsoft.com/fwlink/?LinkId=511643
[Erstellen einer Anwendung Node.js Chat mit Socket.IO Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure the Azure CLI]: ../xplat-cli-install.md
[Azure App Service und ihre Auswirkung auf vorhandene Azure Services]: http://go.microsoft.com/fwlink/?LinkId=529714
[Node.js-Entwicklercenter]: /develop/nodejs/
[Versuchen Sie App Service]: http://go.microsoft.com/fwlink/?LinkId=523751
[Affinität in Azure Websites]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[Erstellen Sie einen Cache in Azure Redis Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[Socket.IO redis]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub repository]: https://github.com/socketio/socket.io
[ZIP oder GZ archivierte Version]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
