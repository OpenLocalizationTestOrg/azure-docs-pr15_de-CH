<properties 
    pageTitle="Erstellen und Bereitstellen einer Node.js Web app in Azure mithilfe von WebMatrix" 
    description="Ein Tutorial erklärt, die Sie mit WebMatrix Node.js-Anwendung entwickeln und in Azure App Service Web Apps bereitstellen." 
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


# <a name="build-and-deploy-a-nodejs-web-app-to-azure-using-webmatrix"></a>Erstellen und Bereitstellen einer Node.js Web app in Azure mithilfe von WebMatrix

In diesem Lernprogramm wird veranschaulicht, wie mit WebMatrix Node.js-Anwendung entwickeln und in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps bereitstellen. WebMatrix ist eine kostenlose Webentwicklungstool von Microsoft, die alles enthält, Sie für die Website oder app-Entwicklung müssen. WebMatrix umfasst zahlreiche Funktionen, die einschließlich Code vervollständigen, vordefinierte Vorlagen und Editor-Unterstützung für Jade, weniger benutzerfreundlich Node.js und CoffeeScript machen. Erfahren Sie mehr über [WebMatrix](https://www.microsoft.com/web/webmatrix/).

Nach Abschluss dieses Handbuchs, haben Sie in Azure App Service ausgeführt Node.js Web-app.
 
Screenshot der abgeschlossenen Anwendung lautet wie folgt:

![Azure Knoten-Website][webmatrix-node-completed]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="sign-into-azure"></a>Melden Sie sich bei Azure

Gehen Sie in Azure App Service Web app erstellen.

1. WebMatrix starten
2. Wenn das erste Mal haben Sie WebMatrix verwendet Sie werden aufgefordert, Azure anmelden.  Sie können andernfalls klicken Sie auf die Schaltfläche **Anmelden** und **Konto hinzufügen**wählen.  Wählen Sie Ihren Microsoft Account **Anmelden** .

    ![Konto hinzufügen][addaccount]

3. Wenn Sie sich für ein Azure-Konto angemeldet haben, können Sie mit Ihrem Microsoft Account anmelden:

    ![Melden Sie sich bei Azure][signin]  


## <a name="create-a-site-using-a-built-in-template-for-azure"></a>Erstellen einer Website mithilfe einer integrierten Vorlage für Azure

1. Klicken Sie auf dem Startbildschirm auf die Schaltfläche **neu** , und wählen Sie **Vorlagen** aus den Office-Vorlagen eine neue Website erstellen:

    ![Neue Website aus Office-Vorlagen][sitefromtemplate]

2. Klicken Sie im Dialogfeld **Site Vorlage** wählen Sie **Knoten** und dann **Express Website**. Klicken Sie auf **Weiter**. Wenn Sie alle erforderlichen Komponenten für die Vorlage **Express** fehlt, werden Sie aufgefordert zu installieren.

    ![Wählen Sie express aus][webmatrix-templates]

3. Wenn Sie in Azure angemeldet sind, können Sie jetzt eine App Service Web app für die lokale Website erstellen.  Wählen Sie einen eindeutigen Namen und Datacenter, Ihrer App Service Anwendung erstellt werden soll: 

    ![Website auf Azure erstellen][nodesitefromtemplateazure]
    
4. Nach Abschluss von WebMatrix Ort und App Service Web app wird der WebMatrix-IDE angezeigt.

    ![WebMatrix ide][webmatrix-ide]

##<a name="publish-your-application-to-azure"></a>Veröffentlichen Sie die Anwendung in Azure

1. Klicken Sie in WebMatrix **Veröffentlichen** aus dem Menüband **Home** **Veröffentlichung** Vorschaubereich für die Website angezeigt.

    ![Vorschau veröffentlichen][webmatrix-node-publishpreview]

2. Klicken Sie auf **Weiter**. Wenn die Veröffentlichung abgeschlossen ist, wird der URL für die App Service Web app am unteren Rand der IDE WebMatrix angezeigt.

    ![Veröffentlichung abgeschlossen][webmatrix-publish-complete]

3. Klicken Sie um App Service Web app im Browser zu öffnen.

    ![Express WebApp][webmatrix-node-express-site]

##<a name="modify-and-republish-your-application"></a>Ändern Sie und veröffentlichen Sie die Anwendung

Sie können problemlos ändern und veröffentlichen Sie die Anwendung. Hier machen Sie einfach die Überschrift in der Datei **index.jade** ändern und veröffentlichen Sie die Anwendung.

1. Wählen Sie in WebMatrix **Dateien aus**und dann den Ordner " **Ansichten** ". Öffnen Sie die Datei **index.jade** durch Doppelklicken.

    ![WebMatrix anzeigen index.jade][webmatrix-modify-index]

2. Ändern Sie die Zeile Absatz wie folgt:

        p Welcome to #{title} with WebMatrix on Azure!

3. Speichern Sie, und klicken Sie dann auf das Symbol "Veröffentlichen". Schließlich klicken Sie auf **Weiter** im Dialogfeld **Vorschau veröffentlichen** und warten Sie, bis das Update veröffentlicht werden.

    ![Vorschau veröffentlichen][webmatrix-republish]

4. Wenn die Veröffentlichung abgeschlossen ist, verwenden Sie den Link zurückgegeben, wenn der Veröffentlichungsprozess abgeschlossen ist, um die aktualisierte Anwendung Service Web App ist.

    ![Azure Knoten WebApp][webmatrix-node-completed]

##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Versionen von Node.js, Azure und die Version mit der Anwendung bereitgestellt werden, finden Sie [eine Node.js Azure-Anwendung angeben](../nodejs-specify-node-version-azure-apps.md).

Wenn Sie mit der Anwendung Probleme nach der Bereitstellung in Azure, finden Sie unter [Debuggen eine Node.js Web app in Azure App Service](web-sites-nodejs-debug.md) Informationen zur Problemdiagnose.

## <a name="whats-changed"></a>Was hat sich geändert
* Eine Anleitung zur Änderung von Websites zu App Service finden Sie unter: [Azure App Service und seine Auswirkung auf vorhandene Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[WebMatrix WebSite]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png
[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png

[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png

[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png
 