<properties 
    pageTitle="Node.js-Anwendung Socket.io | Microsoft Azure" 
    description="Informationen Sie zum socket.io eine node.js-Anwendung auf Azure verwenden." 
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

# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Erstellen einer Anwendung Node.js Chat mit Socket.IO Azure Cloud Service

Socket.IO bietet Echtzeit-Kommunikation zwischen node.js Server und Clients. Dieses Lernprogramm führt Sie durch einen Socket hosten. IO basierte Chat-Anwendung in Azure. Weitere Informationen zu Socket.IO finden Sie unter <http://socket.io/>.

Screenshot der abgeschlossenen Anwendung lautet wie folgt:

![Ein Browserfenster anzeigen des Diensts auf Azure][completed-app]  

## <a name="prerequisites"></a>Erforderliche Komponenten

Stellen Sie sicher, dass die folgenden Produkte und Versionen, zum Beispiel in diesem Artikel erfolgreich abgeschlossen installiert sind:

* Installieren Sie [Visual Studio 2013](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)
* [Node.js](https://nodejs.org/download/) installieren
* Installieren Sie [Python Version 2.7.10](https://www.python.org/)

## <a name="create-a-cloud-service-project"></a>Ein Cloud-Dienstprojekt erstellen

Die folgenden Schritte erstellen Projekt für den Cloud-Dienst, der Socket.IO Anwendung gehostet wird.

1. Suchen Sie aus dem **Startmenü** oder **Startbildschirm von** **Windows PowerShell**. Schließlich **Windows PowerShell** Maustaste, und wählen Sie **Als Administrator ausführen**.

    ![Azure PowerShell-Symbol][powershell-menu]

2. Erstellen Sie ein Verzeichnis namens **c:\\Knoten**. 
 
        PS C:\> md node

3. Wechseln Sie in die **c:\\Knoten** Verzeichnis
 
        PS C:\> cd node

4. Geben Sie die folgenden Befehle, um eine neue Lösung mit dem Namen **Chatapp** und eine Worker-Rolle mit dem Namen **WorkerRole1**erstellen:

        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole

    Sie sehen die folgende Antwort:

    ![Die Ausgabe des neuen Azureservice und Azurenodeworkerrolecmdlets hinzufügen](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>Laden Sie das Chat-Beispiel

Für dieses Projekt wird Chat Beispiel [Socket.IO GitHub Repository]verwendet. Die folgenden Schritte das Beispiel herunterladen und dem zuvor erstellten Projekt hinzufügen.

1.  Erstellen Sie eine lokale Kopie des Repository mithilfe der Schaltfläche **Klon** . Die **ZIP-** Schaltfläche können Sie das Projekt.

    ![Ein Browser-Fenster https://github.com/LearnBoost/socket.io/tree/master/examples/chat, mit dem ZIP-Download-Symbol hervorgehoben anzeigen][chat-example-view]

3.  Die Verzeichnisstruktur des lokalen Repository navigieren, bis Sie erreichen der **Beispiele\\Chat** Verzeichnis. Kopieren Sie den Inhalt dieses Verzeichnisses der **C:\\Knoten\\Chatapp\\WorkerRole1** zuvor erstellte Verzeichnis.

    ![Anzeigen der Beispiele-Explorer\\Chat Verzeichnis extrahiert aus dem Archiv][chat-contents]

    Die markierten Elemente in der Abbildung oben sind aus kopierten Dateien der **Beispiele\\Chat** Verzeichnis

4.  In der **C:\\Knoten\\Chatapp\\WorkerRole1** Verzeichnis **server.js** löschen und benennen Sie die Datei **app.js** in **server.js**. Dieses **Add-AzureNodeWorkerRole** -Cmdlet zuvor erstellte **server.js** -Standarddatei entfernt und ersetzt die Anwendungsdatei aus dem Chat-Beispiel.

### <a name="modify-serverjs-and-install-modules"></a>Ändern von Server.js und Module installieren

Vor dem Testen der Anwendung in Azure-Emulator, müssen wir einige kleinere Änderungen. Führen Sie die folgenden Schritte in der Datei server.js:

1.  Öffnen Sie die Datei **server.js** in Visual Studio oder einem Texteditor.

2.  **Modul Abhängigkeiten** Abschnitt am Anfang des server.js suchen und ändern Sie die Zeile mit **Sio = Require('.. //.. LIB//Socket.IO')** auf **Sio = require('socket.io')** wie folgt:

        var express = require('express')
        , stylus = require('stylus')
        , nib = require('nib')
        //, sio = require('..//..//lib//socket.io'); //Original
        , sio = require('socket.io');                //Updated

3.  Um sicherzustellen, dass die Anwendung den richtigen Port überwacht, öffnen Sie server.js im Editor oder in Ihrem bevorzugten Editor, und ändern Sie die folgende Zeile ersetzen **3000** mit **process.env.port** wie folgt:

        //app.listen(3000, function () {            //Original
        app.listen(process.env.port, function () {  //Updated
          var addr = app.address();
          console.log('   app listening on http://' + addr.address + ':' + addr.port);
        });

Nach dem Speichern der Änderungen auf **server.js**gehen Sie Module installieren und Testen Sie die Anwendung in Azure-Emulator:

1.  Mithilfe von **Azure PowerShell**, wechseln Sie in die **C:\\Knoten\\Chatapp\\WorkerRole1** Verzeichnis und verwenden die folgenden Module dieser Anwendung installiert:

        PS C:\node\chatapp\WorkerRole1> npm install

    Dies wird in der Datei package.json aufgeführten Module installiert. Nach Abschluss der Befehl sollte eine Ausgabe ähnlich der folgenden angezeigt werden:

    ![Die Ausgabe der Npm Befehl installieren][The-output-of-the-npm-install-command]

4.  Da dabei ursprünglich Teil Socket.IO GitHub Repository war und Socket.IO Bibliothek direkt mit relativen Pfad verwiesen wurde Socket.IO nicht in der Datei package.json verwiesen, damit wir sie durch Ausführen des folgenden Befehls installieren müssen:

        PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Testen und bereitstellen

1.  Starten Sie den Emulator, den folgenden Befehl:

        PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch

2.  Öffnen Sie einen Browser, und navigieren Sie zu **http://127.0.0.1**.

3.  Das Browser-Fenster einen Spitznamen eingeben und dann EINGABETASTE.
    Dies wird alle Nachrichten einer bestimmten Spitznamen zu buchen. Multi-User-Funktionen, zusätzliche Browserfenster mit der gleichen URL öffnen und andere Benutzernamen eingeben.

    ![Zwei Browserfenster Chatnachrichten Benutzer1 und Benutzer2 anzeigen](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)

3.  Nach dem Testen der Anwendungdes stoppen Sie Emulator des folgenden Befehls:

        PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator

4.  Verwenden Sie zum Bereitstellen der Anwendung in Azure **Veröffentlichen-AzureServiceProject** -Cmdlet. Zum Beispiel:

        PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch

    > [AZURE.IMPORTANT] Verwenden Sie einen eindeutigen Namen, andernfalls Veröffentlichungsvorgang schlägt fehl. Nach Abschluss die Bereitstellung wird der Browser öffnen und navigieren Sie zu den bereitgestellten Dienst.
    > 
    > Wenn eine angezeigt Fehlermeldung, dass der bereitgestellten Namen in importierten Veröffentlichungsprofil vorhanden ist, müssen Sie herunterladen und importieren die Präsentationsprofil für Ihr Abonnement vor der Bereitstellung in Azure. Abschnitt **Bereitstellen der Anwendung in Azure** [Erstellen und Bereitstellen eine Anwendung Node.js Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

    ![Ein Browserfenster anzeigen des Diensts auf Azure][completed-app]

    > [AZURE.NOTE] Wenn eine angezeigt Fehlermeldung, dass der bereitgestellten Namen in importierten Veröffentlichungsprofil vorhanden ist, müssen Sie herunterladen und importieren die Präsentationsprofil für Ihr Abonnement vor der Bereitstellung in Azure. Abschnitt **Bereitstellen der Anwendung in Azure** [Erstellen und Bereitstellen eine Anwendung Node.js Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

Die Anwendung wird jetzt in Azure ausgeführt und kann Nachrichten zwischen verschiedenen Clients mit Socket.IO weiterleiten.

> [AZURE.NOTE] In diesem Beispiel beträgt der Einfachheit halber Chat zwischen Benutzern in derselben Instanz verbunden. Dies bedeutet, dass Cloud-Dienst Arbeitskraft zwei Instanzen erstellt, Benutzer nur andere auf dieselbe Instanz der Arbeitskraft Rolle chatten können. Skalieren Sie die Anwendung mit mehreren Instanzen können Sie eine Technologie wie Service Bus Speicherzustand Socket.IO Instanzen freigeben. Beispiele finden Sie in den Service Bus Warteschlangen und Themen Verwendung Beispielen in [Azure SDK Node.js GitHub Repository](https://github.com/WindowsAzure/azure-sdk-for-node).

##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie eine grundlegende chatanwendung in Azure Cloud Service erstellt. Wie diese Anwendung in Azure-Website finden Sie unter [Erstellen einer Node.js Chat-Anwendung mit Socket.IO eine Azure-Website][chatwebsite].

Weitere Informationen finden Sie auch im [Node.js Developer Center](/develop/nodejs/).

  [chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

  [Azure SLA]: http://www.windowsazure.com/support/sla/
  [Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
  [completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
  [Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
  [Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Socket.IO GitHub repository]: https://github.com/LearnBoost/socket.io/tree/0.9.14
  [Azure Considerations]: #windowsazureconsiderations
  [Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
  [Summary and Next Steps]: #summary
  [powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

  [chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
  [chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png
  
  
  [chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
  [The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
  [The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png
  
 
