<properties
    pageTitle="Angeben einer Node.js-Version"
    description="Erfahren Sie, wie die Version von Node.js Azure und Cloud-Services verwendet"
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Angeben einer Version Node.js in Azure-Anwendung

Wenn Node.js-Anwendung hostet, sollten Sie sicherstellen, dass die Anwendung eine bestimmte Version von Node.js verwendet. Es gibt verschiedene dazu für Applikationen auf Azure.

##<a name="default-versions"></a>Standardversionen

Node.js Versionen von Azure bereitgestellt werden ständig aktualisiert. Sofern nicht anders angegeben, die Standardversion, die in der `WEBSITE_NODE_DEFAULT_VERSION` -Umgebungsvariable verwendet. Um diesen Wert zu überschreiben, führen Sie die Schritte in den folgenden Abschnitten dieses Artikels

> [AZURE.NOTE] Wenn Sie der Anwendung in einer Azure-Cloud-Dienst (Web oder Worker-Rolle hosten) der ersten der Anwendung Bereitstellung ist, versucht Azure verwenden dieselbe Version von Node.js auf Umgebung installiert haben, wenn sie eine Standardversionen von Azure übereinstimmt.

##<a name="versioning-with-packagejson"></a>Versionskontrolle mit package.json

Sie können die Version von Node.js verwendet werden, indem Sie Folgendes in die Datei **package.json** hinzufügen:

    "engines":{"node":version}

Hierbei steht *Version* Versionsnummer verwenden. Sie können komplexere Vorschriften für Version wie angeben:

    "engines":{"node": "0.6.22 || 0.8.x"}

Da 0.6.22 nicht die Versionen in der Hosting-Umgebung verfügbar ist, verwendet die höchste Version der Serie steht 0,8 - 0.8.4.

##<a name="versioning-websites-with-app-settings"></a>Versioning Websites mit App
Wenn Sie die Anwendung auf einer Website hosten, können Sie die Umgebungsvariable **WEBSITE_NODE_DEFAULT_VERSION** auf die gewünschte Version festlegen. 

##<a name="versioning-cloud-services-with-powershell"></a>Versioning Cloud-Diensten mit PowerShell

Wenn Sie die Anwendung in einem Cloud-Dienst hosten und mit Azure PowerShell Anwendung bereitstellen, können Sie die Standardversion Node.js mithilfe des **Set-AzureServiceProjectRole** PowerShell-Cmdlets überschreiben. Zum Beispiel:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Hinweis: die Parameter in der obigen Anweisung beachtet werden.  Sie können überprüfen, dass die richtige Version von Node.js der **Motoren** Eigenschaft in Ihrer Rolle **package.json**ausgewählt wurde.

**Get-AzureServiceProjectRoleRuntime** können Sie Node.js Versionen Applikationen als Cloud-Dienst abrufen.  Immer überprüfen Sie Version Node.js Projekts hängt in dieser Liste.

##<a name="using-a-custom-version-with-azure-websites"></a>Verwenden eine benutzerdefinierte Version mit Azure Websites

Azure bietet verschiedene Standardversionen von Node.js sollten Sie eine Version verwenden, die nicht standardmäßig bereitgestellt. Wenn die Anwendung als Website Azure gehostet, erreichen Sie dies mithilfe der Datei **iisnode.yml** . Schritte durch den Prozess der Azure-Website eine benutzerdefinierte Version von Node.Js mit:

1. Erstellen Sie ein neues Verzeichnis, und erstellen Sie eine **server.js** -Datei im Verzeichnis. Die Datei **server.js** sollte Folgendes enthalten:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    Beim Durchsuchen der Website verwendete Node.js-Version wird angezeigt.

2. Erstellen einer neuen Website, und notieren Sie den Namen der Website. Beispielsweise verwendet die folgenden [Azure Befehlszeilentools] zu erstellen eine neue Azure-Website **Mywebsite**namens Git Repository für die Website aktivieren.

        azure site create mywebsite --git

3. Erstellen Sie ein neues Verzeichnis **Bin** ein untergeordnetes Verzeichnis mit der Datei **server.js** .

4. Herunterladen der Version **node.exe** (die Windows-Version), die mit Ihrer Anwendung verwenden möchten. Beispielsweise verwendet die folgenden **Aufrollen** Version 0.8.1 herunterladen:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Speichern Sie die Datei **node.exe** im Ordner **Bin** zuvor erstellt.

5. Erstellen Sie eine **iisnode.yml** -Datei im gleichen Verzeichnis wie die Datei **server.js** , und fügen Sie folgenden Inhalt zur Datei **iisnode.yml** :

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Dieser Pfad wird **node.exe** Datei innerhalb des Projekts liegt nach der Anwendung der Azure-Website veröffentlicht haben.

6. Veröffentlichen Sie die Anwendung. Beispielsweise seit ich eine neue Website mit dem Parameter - Git zuvor erstellt haben, werden die folgenden Befehle Anwendungsdateien auf meinem lokalen Git Repository hinzufügen und drücken zum Website-Repository:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Nachdem die Anwendung veröffentlicht wurde, öffnen Sie die Website in einem Browser. Sieht eine Meldung "Hallo von Azure ausgeführte Knoten Version: v0.8.1".

##<a name="next-steps"></a>Nächste Schritte

Jetzt wissen zum Angeben von der Anwendung verwendeten Node.js erfahren, wie [Arbeiten mit Modulen] [Erstellen und Bereitstellen einer Website Node.js]und [zum Azure-Befehlszeilentools für Mac und Linux verwenden].

Weitere Informationen finden Sie unter [Node.js Developer Center](/develop/nodejs/).

[Verwendung der Azure-Befehlszeilentools für Mac und Linux]: xplat-cli-install.md
[Azure Befehlszeilentools]: xplat-cli-install.md
[Arbeiten mit Modulen]: nodejs-use-node-modules-azure-apps.md
[Erstellen und Bereitstellen einer Node.js-Website]: web-sites-nodejs-develop-deploy-mac.md
