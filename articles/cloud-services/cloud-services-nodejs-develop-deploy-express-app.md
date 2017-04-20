<properties 
    pageTitle="Web-App mit Express (Node.js) | Microsoft Azure" 
    description="Ein Lernprogramm, in dem auf das Cloud-Dienst-Lernprogramm erstellt und veranschaulicht, wie das Express-Modul." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>






# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Erstellen einer Azure Cloud Service Express über Node.js Web-Anwendung

Node.js enthält einen minimalen Satz von Funktionen in der Core-Laufzeit.
Entwickler verwenden oft 3rd Party Module eine zusätzliche Funktionalität bei der Entwicklung einer Anwendung Node.js. In diesem Lernprogramm erstellen Sie eine neue Anwendung mit dem [Express][] -Modul, mit MVC Framework für ASP.NET-Webanwendungen Node.js erstellen.

Screenshot der abgeschlossenen Anwendung lautet wie folgt:

![Ein Webbrowser Willkommen Express in Azure anzeigen](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

##<a name="create-a-cloud-service-project"></a>Ein Cloud-Dienstprojekt erstellen

Die folgenden Schritte erstellen Sie ein neues clouddienstprojekt-Dienst mit dem Namen "Expressapp":

1. Suchen Sie aus dem **Startmenü** oder **Startbildschirm von** **Windows PowerShell**. Schließlich **Windows PowerShell** Maustaste, und wählen Sie **Als Administrator ausführen**.

    ![Azure PowerShell-Symbol](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)

    [AZURE.INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

2. Wechseln Sie in die **c:\\Knoten** Verzeichnis und geben Sie die folgenden Befehle, um eine neue Lösung mit dem Namen **Expressapp** und eine Webrolle mit dem Namen **WebRole1**erstellen:

        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21

    > [AZURE.NOTE] **Add-AzureNodeWebRole** verwendet eine ältere Version von Node.js. **Set AzureServiceProjectRole** oben angewiesen Azure v0.10.21 Knoten.  Hinweis: die Parameter beachtet werden.  Sie können überprüfen, dass die richtige Version von Node.js überprüft die **Module** -Eigenschaft im **WebRole1\package.json**ausgewählt wurde.

##<a name="install-express"></a>Express installieren

1. Installieren Sie Express-Generator durch Ausführen des folgenden Befehls:

        PS C:\node\expressapp> npm install express-generator -g

    Die Ausgabe des Befehls Npm sollte das Ergebnis unten ähneln. 

    ![Windows PowerShell zeigt die Ausgabe der Npm installieren express Befehl.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)

2. Wechseln Sie zum Verzeichnis **WebRole1** und Befehl express eine neue Anwendung erstellen:

        PS C:\node\expressapp\WebRole1> express

    Sie werden aufgefordert, die frühere Anwendung überschreiben. Geben Sie **y** oder **yes** fort. Express wird app.js Datei- und Ordnerstruktur für Ihre Anwendung generiert.

    ![Die Ausgabe des Befehls express](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)


5.  Um zusätzliche Abhängigkeiten definiert in der Datei package.json installieren, geben Sie folgenden Befehl ein:

        PS C:\node\expressapp\WebRole1> npm install

    ![Die Ausgabe der Npm Befehl installieren](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)

6.  Verwenden Sie folgenden Befehl die **Bin-Www** -Datei **server.js**kopieren. Dies ist der Cloud-Dienst den Einstiegspunkt für diese Anwendung finden.

        PS C:\node\expressapp\WebRole1> copy bin/www server.js

    Nach dieser Befehl abgeschlossen ist, müssen Sie eine **server.js** -Datei im Verzeichnis WebRole1.

7.  Ändern der **server.js** Entfernen eines der '.' Zeichen aus der folgenden Zeile.

        var app = require('../app');

    Nachdem Sie diese Änderung vorgenommen, sollte die Zeile wie folgt angezeigt werden.

        var app = require('./app');

    Diese Änderung ist erforderlich, da wir die Datei (früher **Bin-Www**) verschoben in dasselbe Verzeichnis wie die app-Datei erforderlich. Nach dieser Änderung wird speichern Sie die **server.js** -Datei.

8.  Verwenden Sie den folgenden Befehl zum Ausführen der Anwendung in Azure-Emulator:

        PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch

    ![Eine Webseite mit express Willkommen.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>Ändern der Ansicht

Jetzt ändern Sie die Ansicht, um die Meldung "Willkommen zu Express in Azure".

1.  Geben Sie den folgenden Befehl zum Öffnen der Datei index.jade:

        PS C:\node\expressapp\WebRole1> notepad views/index.jade

    ![Der Inhalt der Datei index.jade.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

    Jade ist das Standardansichtsmodul Express Applikationen verwendet. Weitere Informationen zu Jade Ansichtsmodul finden Sie unter [http://jade-lang.com][].

2.  Ändern Sie die letzte Textzeile durch Anhängen **in Azure**.

    ![Die Datei index.jade die letzte Zeile gelesen: Willkommen bei p \#{title} in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)

3.  Speichern Sie die Datei, und beenden Sie Notepad.

4.  Aktualisieren Sie Ihren Browser, und Sie sehen sich ändert.

    ![Ein Browser-Fenster enthält die Seite Willkommen Express in Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Nach dem Testen der Anwendungdes verwenden Sie das Cmdlet " **Stop AzureEmulator** " Emulator beenden.

##<a name="publishing-the-application-to-azure"></a>Veröffentlichen Sie die Anwendung in Azure

Verwenden Sie im Fenster Azure PowerShell **Veröffentlichen-AzureServiceProject** -Cmdlet zum Bereitstellen der Anwendung auf einen Clouddienst

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Nach Abschluss des Bereitstellungsvorgangs wird Ihr Browser öffnen und die Webseite anzuzeigen.

![Ein Webbrowser die Seite Express angezeigt. Die URL gibt an, dass es jetzt in Azure gehostet wird.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Node.js Developer Center](/develop/nodejs/).

  [Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Express]: http://expressjs.com/
  [http://Jade-lang.com]: http://jade-lang.com

 
