<properties
    pageTitle="Node.js Getting Started Guide | Microsoft Azure"
    description="Informationen Sie zum Erstellen einer einfachen Node.js und Azure Cloud-Dienst bereitgestellt."
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
    ms.topic="hero-article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-and-deploy-a-nodejs-application-to-an-azure-cloud-service"></a>Erstellen und Bereitstellen einer Anwendung Node.js Azure Cloud Service

> [AZURE.SELECTOR]
- [Node.js](cloud-services-nodejs-develop-deploy-app.md)
- [.NET](cloud-services-dotnet-get-started.md)

Dieses Lernprogramm zeigt, wie eine einfache in Azure-Clouddienst ausgeführt Node.js-Anwendung erstellen. Cloud-Services sind die Bausteine der skalierbaren Cloudanwendungen in Azure. Sie ermöglichen die Trennung und unabhängige Verwaltung und Skalierung von Front-End- und Back-End-Komponenten der Anwendung.  Cloud-Dienste bieten einen robusten dedizierten virtuellen Computer für jede Rolle zuverlässig hosten.

Weitere Informationen zum Cloud-Services und Azure Websites und virtuelle Computer Vergleich finden Sie unter [virtuelle Computer Vergleich, Cloud-Services und Azure Websites].

>[AZURE.TIP] Sie möchten eine einfache Website erstellen? Sollten Sie Ihr Szenario nur eine einfache Website Front-End-beinhaltet, [eine einfache Web app verwenden]. Sie können einen Cloud-Dienst Upgrades auf Ihrer Anwendung wächst und Bedarf.

Anhand dieses Lernprogramms erstellen Sie eine einfache Anwendung in einer Webrolle gehostet. Sie verwenden, um die Anwendung lokal testen Serveremulator dann PowerShell Befehlszeilenprogramme bereitgestellt.

Die Anwendung ist eine einfache "hello World"-Anwendung:

![Web-Browser Hello World-Webseite anzeigen][A web browser displaying the Hello World web page]

## <a name="prerequisites"></a>Erforderliche Komponenten

> [AZURE.NOTE] Diese praktische Einführung verwendet die erfordert Windows Azure PowerShell.

- Installieren und Konfigurieren von [Azure Powershell].
- Herunterladen Sie und installieren Sie der [Azure SDK für .NET 2.7]. Wählen Sie im Setup installieren:
    - MicrosoftAzureAuthoringTools
    - MicrosoftAzureComputeEmulator


## <a name="create-an-azure-cloud-service-project"></a>Ein Azure Cloud Service-Projekt erstellen

Führen Sie die folgenden Aufgaben zum Erstellen eines neuen Azure Cloud Service-Projekts mit Node.js Grundgerüst:

1. **Windows PowerShell** als Administrator ausführen; Suchen Sie aus dem **Startmenü** oder **Startbildschirm von** **Windows PowerShell**.

2. [PowerShell verbinden] Ihres Abonnements.

3. Geben Sie das folgende PowerShell-Cmdlet zu erstellen, um das Projekt zu erstellen:

        New-AzureServiceProject helloworld

    ![Das Ergebnis des Befehls Helloworld neu AzureService][The result of the New-AzureService helloworld command]

    **Neu-AzureServiceProject** -Cmdlet generiert eine Grundstruktur für die Veröffentlichung einer Anwendung Node.js einen Cloud-Dienst. Es enthält Konfigurationsdateien für Azure veröffentlichen. Das Cmdlet wird ebenfalls Arbeitsverzeichnis auf das Verzeichnis für den Dienst.

    Das Cmdlet erstellt die folgenden Dateien:

    -   **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** und **ServiceDefinition.csdef**: Azure-Dateien für die Veröffentlichung der Anwendung erforderlich. Weitere Informationen finden Sie unter [Übersicht über das Erstellen eines gehosteten Diensts für Azure].

    -   **deploymentSettings.json**: lokale Einstellungen, die durch die Bereitstellung Azure PowerShell-Cmdlets verwendet werden.

4.  Geben Sie den folgenden Befehl zum Hinzufügen einer neuen Webrolle:

        Add-AzureNodeWebRole

    ![Die Ausgabe des Befehls Add-AzureNodeWebRole][The output of the Add-AzureNodeWebRole command]

    **Add-AzureNodeWebRole** -Cmdlet wird eine grundlegende Node.js-Anwendung erstellt. Es ändert auch die Dateien **.csfg** und **.csdef** Konfigurationseinträge für die neue Rolle hinzufügen.

    > [AZURE.NOTE] Wenn Sie einen Rollennamen nicht angeben, wird ein Standardname verwendet. Als erste Cmdlet-Parameter können Sie einen Namen angeben:`Add-AzureNodeWebRole MyRole`

Node.js-Anwendung ist in der Datei **server.js**, das Verzeichnis für die Webrolle (standardmäßig**WebRole1** ) definiert. Hier ist der Code:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Dieser Code entspricht im Wesentlichen der im Beispiel "Hello World" auf der Website [nodejs.org] außer zugewiesene Cloud-Umgebung verwendet.

## <a name="deploy-the-application-to-azure"></a>Bereitstellen der Anwendung in Azure

    [AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

### <a name="download-the-azure-publishing-settings"></a>Die Azure veröffentlichen Einstellungen herunterladen

Bereitstellung die Anwendung in Azure müssen Sie zunächst Veröffentlichung Einstellungen für Ihre Azure-Abonnement herunterladen.

1.  Führen Sie das folgende Azure PowerShell-Cmdlet:

        Get-AzurePublishSettingsFile

    Dies wird mithilfe des Browsers wechseln Sie zur Einstellungsseite Download veröffentlichen. Sie können mit Microsoft Account anmelden aufgefordert. In diesem Fall verwenden Sie das Konto der Azure-Abonnement zugeordnet.

    Speichern Sie das heruntergeladene Profil an eine Datei, die Sie leicht zugreifen können.

2.  Führen Sie folgende Cmdlet publishing Profilimport heruntergeladene:

        Import-AzurePublishSettingsFile [path to file]


    > [AZURE.NOTE] Nach dem Import Settings veröffentlichen sollten Sie heruntergeladenen PUBLISHSETTINGS-Datei löschen, da sie Informationen, die jemand enthält auf Ihr Konto zugreifen können.

### <a name="publish-the-application"></a>Veröffentlichen Sie die Anwendung

Führen Sie zum Veröffentlichen der folgenden Befehle ein:

    $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))   
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

- **ServiceName -** gibt den Namen für die Bereitstellung. Dies muss ein eindeutiger Name sein, andernfalls Veröffentlichungsvorgang fehl. Der Befehl **Get Date** Stifte auf eine Datum/Uhrzeit-Zeichenfolge, die den Namen eindeutig zu machen sollten.

- **-Speicherort** gibt das Rechenzentrum in die Anwendung gehostet wird. Um eine Liste der verfügbaren Rechenzentren anzuzeigen, verwenden Sie das Cmdlet " **Get-AzureLocation** ".

- **-Starten** ein Browserfenster geöffnet und navigiert zur gehosteten Dienst nach Abschluss der Bereitstellung.

Nach der Veröffentlichung erfolgreich ist, wird eine Antwort ähnlich der folgenden angezeigt:

![Die Ausgabe des Befehls veröffentlichen AzureService][The output of the Publish-AzureService command]

> [AZURE.NOTE]
> Es kann mehrere Minuten dauern die Anwendung bereitgestellt und sind verfügbar, wenn zuerst veröffentlicht.

Nachdem die Bereitstellung abgeschlossen ist, wird ein Browserfenster öffnen und Navigieren zum Cloud-Dienst.

![Ein Browser-Fenster Hello World anzuzeigen; die URL gibt an, dass die Seite Azure gehostet wird.][A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]

Die Anwendung wird jetzt in Azure ausgeführt.

Das Cmdlet **Veröffentlichen AzureServiceProject** folgende Schritte ausgeführt:

1.  Erstellt ein Paket bereitstellen. Das Paket enthält alle Dateien im Anwendungsordner.

2.  Erstellt ein neues **Konto** , wenn nicht vorhanden. Das Azure-Speicher-Konto zum Anwendungspaket während der Bereitstellung gespeichert. Sie können das Speicherkonto sicher, nach Abschluss der Bereitstellung.

3.  Erstellt einen neuen **Cloud-Dienst** , falls noch nicht vorhanden ist. **Cloud-Dienst** ist der Container, in dem die Anwendung gehostet wird, wenn sie in Azure bereitgestellt wird. Weitere Informationen finden Sie unter [Übersicht über das Erstellen eines gehosteten Diensts für Azure].

4.  Das Bereitstellungspaket veröffentlicht in Azure.


## <a name="stopping-and-deleting-your-application"></a>Anhalten und Löschen der Anwendung

Nach dem Bereitstellen der Anwendung möchten Sie deaktivieren Mehrkosten zu vermeiden. Azure Rechnungen web Instanzen pro Stunde Server verbraucht. Serverzeit verbraucht, nachdem die Anwendung bereitgestellt wird, auch wenn die Instanzen nicht ausgeführt und beendet werden.

1.  Stoppen Sie im Fenster Windows PowerShell Service-Bereitstellung, die im vorherigen Abschnitt mit dem folgenden Cmdlet erstellt:

        Stop-AzureService

    Beenden des Dienstes kann mehrere Minuten dauern. Wenn der Dienst angehalten wird, erhalten Sie eine Meldung beendet wurde.

    ![Der Status des Befehls beenden AzureService][The status of the Stop-AzureService command]

2.  Um den Dienst zu löschen, rufen Sie das folgende Cmdlet:

        Remove-AzureService

    Geben Sie bei Aufforderung **Y** ein, um den Dienst zu löschen.

    Löschen des Dienstes kann mehrere Minuten dauern. Nach dem Löschen des Dienstes erhalten Sie eine Meldung, dass der Dienst gelöscht wurde.

    ![Der Status des Befehls AzureService entfernen][The status of the Remove-AzureService command]

    > [AZURE.NOTE] Den Dienst löschen nicht das Speicherkonto erstellt wurde, wenn der Dienst ursprünglich veröffentlicht wurde und Sie weiterhin beanspruchte berechnet. Wenn nichts Speicher verwendet, möchten Sie es löschen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Node.js Developer Center].

<!-- URL List -->

[Azure Websites, Cloud-Diensten und virtuellen Maschinen-Vergleich]: ../app-service-web/choose-web-site-cloud-service-vm.md
[verwenden eine einfache Web app]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Azure Powershell]: ../powershell-install-configure.md
[Azure SDK für .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[PowerShell verbinden]: ../powershell-install-configure.md#how-to-connect-to-your-subscription
[nodejs.org]: http://nodejs.org/
[Übersicht über das Erstellen eines gehosteten Diensts für Azure]: https://azure.microsoft.com/documentation/services/cloud-services/
[Node.js-Entwicklercenter]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[The result of the New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[The output of the Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying the Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[The status of the Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[The status of the Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
