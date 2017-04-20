<properties
    pageTitle="Erstellen eine Webanwendung von Azure Marketplace | Microsoft Azure"
    description="Erfahren Sie mehr über das neue WordPress Web app Azure Marketplace mithilfe der Azure-Portal."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

# <a name="create-a-web-app-from-the-azure-marketplace"></a>Erstellen einer Webanwendung von Azure Marketplace

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Azure-Markt wird eine Vielzahl von gängigen webapps entwickelt von Microsoft, Firmen und open-Source-Software Initiativen. Z. B. WordPress, Umbraco CMS Drupal usw. Diese Web-apps basieren auf eine Vielzahl beliebter Frameworks, wie [PHP] in diesem WordPress Beispiel, [.NET] [Node.js], [Java]und [Python]zu nennen. Erstellen eine Webanwendung von Azure Marketplace nur Software müssen Sie ist der Browser, den Verwendung der [Azure-Portal].

In diesem Lernprogramm erfahren Sie, wie:

* Suchen und Erstellen von Web-app in Azure App Service Azure Marketplace-Vorlage basiert.
* Konfigurieren Sie Azure App für neue Web app.
* Starten Sie und verwalten Sie Ihrer Anwendung.

Im Rahmen dieses Lernprogramms wird eine WordPress-Blog-Website von Azure Marketplace bereitgestellt. Wenn Sie die Schritte in diesem Lernprogramm abgeschlossen haben, müssen Sie eigene WordPress Website und in der Cloud ausgeführt.

![Beispiel WordPress Wep app dashboard][WordPressDashboard1]

WordPress-Website, die Sie in diesem Lernprogramm bereitstellen müssen verwendet MySQL für die Datenbank. SQL-Datenbank für die Datenbank verwenden, finden Sie [Projekt Nami], das ebenfalls über Azure Marketplace verfügbar ist.

> [AZURE.NOTE]
> Um dieses Lernprogramm benötigen Sie ein Microsoft Azure-Konto. Wenn Sie ein Konto haben, können Sie [Ihre Visual Studio-Abonnementvorteile aktivieren] [ activate] oder [Melden Sie sich für eine kostenlose Testversion][free trial].
>
> Wenn Sie mit Azure App Service beginnen, bevor Sie für ein Azure-Konto, gehen Sie [versuchen App]Service. Dort können Sie sofort eine kurzlebige Starter Web app im App Service erstellen, ist keine Kreditkarte erforderlich, und es gibt keine Zusagen.

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Suchen Sie und erstellen Sie eine Webanwendung in Azure App Service

1. [Azure-Portal]anmelden.

1. **Klicken Sie auf.**
    
    ![Erstellen einer neuen Azure Ressource][MarketplaceStart]
    
1. Suchen Sie **WordPress**, und klicken Sie dann auf **WordPress**. (Wenn Sie statt MySQL SQL-Datenbank verwenden möchten, suchen Sie **Project Nami**.)

    ![Suche nach WordPress auf dem Markt][MarketplaceSearch]
    
1. Lesen die Beschreibung der WordPress app, klicken Sie auf **Erstellen**.

    ![WordPress WebApp erstellen][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Konfigurieren Sie Azure App für Ihr neues Web App

1. Nach dem Erstellen einer neuen Web-Applikation wird WordPress Einstellungen-Blades angezeigt, die Sie mithilfe der folgenden Schritte:

    ![WordPress Web app konfigurieren][ConfigStart]

1. Geben Sie einen Namen für die Webanwendung im **WebApp** .

    Dieser Name muss in der Domäne *.azurewebsites.NET da URL Web App *{Name}*werden. *.azurewebsites.NET. Wenn der eingegebene Name nicht eindeutig ist, wird im Feld ein rotes Ausrufezeichen angezeigt.

    ![Konfigurieren Sie die WordPress Web app][ConfigAppName]

1. Wenn Sie über mehrere Abonnements verfügen, wählen Sie verwenden möchten. 

    ![Konfigurieren Sie das Abonnement für das Web app][ConfigSubscription]

1. Wählen Sie eine **Ressourcengruppe** oder erstellen Sie eine neue.

    Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über Azure Resource Manager][ResourceGroups].

    ![Konfigurieren Sie die Ressourcengruppe für Web app][ConfigResourceGroup]

1. Wählen Sie eine **App Plan/Speicherort** oder erstellen Sie eine neue.

    Weitere Informationen zu App Service-Pläne Übersicht [Azure App Service-Pläne][AzureAppServicePlans]. 

    ![Konfigurieren der Serviceplan für Web app][ConfigServicePlan]

1. Klicken Sie auf **Datenbank**, und dann Blatt **Neue MySQL-Datenbank** die erforderlichen Werte für die MySQL-Datenbank konfigurieren.

    ein. Geben Sie einen neuen Namen, oder übernehmen Sie den Standardnamen.

    b. Lassen Sie die **Datenbanktyp** **freigegeben**.

    c. Wählen Sie dieselbe wie für das Web app haben.

    d. Wählen Sie einen Tarif. In diesem Lernprogramm ist **Quecksilber** - minimale und Speicherplatz frei - Ordnung.

    e. Blatt **Neue MySQL-Datenbank** akzeptieren Sie die Vertragsbedingungen, und klicken Sie auf **OK**. 

    ![Konfigurieren der Datenbank für das Web app][ConfigDatabase]

1. Blatt **WordPress** akzeptieren Sie die Vertragsbedingungen, und klicken Sie auf **Erstellen**. 

    ![Beenden Sie Web Appeinstellungen und klicken Sie auf OK][ConfigFinished]

    Azure App Service erstellt Web app normalerweise in weniger als einer Minute. Sie können den Fortschritt anschauen Glockensymbol oben auf der Portalseite.

    ![Statusanzeige][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Starten Sie und verwalten Sie Ihrer Anwendung WordPress
    
1. Abschließend Web app erstellen Navigieren in Azure-Verwaltungsportal Ressourcengruppe, in der die Anwendung erstellt, und sehen Sie die Webanwendung und Datenbank.

    Zusätzliche Ressourcen mit dem Symbol Glühbirne ist [Application Insights][ApplicationInsights], die Überwachungsdienste für Ihr Web app bietet.

1. Klicken Sie auf Web app Zeile Blatt **Ressourcengruppe** .

    ![Wählen Sie Ihre WordPress Web app][WordPressSelect]

1. Klicken Sie auf **Durchsuchen**, Web app Blatt.

    ![Wechseln Sie zu Ihrer Anwendung WordPress][WordPressBrowse]

1. Wenn Sie aufgefordert werden, wählen Sie die Sprache für Ihren Blog WordPress wählen Sie die gewünschte Sprache, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Sie die Sprache Ihrer Anwendung WordPress][WordPressLanguage]

1. Geben Sie auf der Seite WordPress **Willkommen** WordPress erforderliche Konfigurationsinformationen ein und dann auf **WordPress installieren**.

    ![Konfigurieren Sie das Ihrer Anwendung WordPress][WordPressConfigure]

1. Melden Sie sich mit den Anmeldeinformationen auf der Seite **Willkommen** erstellten.  

1. Die Seite Dashboard wird geöffnet und zeigt die von Ihnen bereitgestellten Informationen.    

    ![WordPress-Dashboard anzeigen][WordPressDashboard2]

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gesehen, wie das Erstellen und Bereitstellen eine Beispiel Web app Azure Marketplace.

Weitere Informationen zum Arbeiten mit App Service Web Apps finden Sie Links auf der linken Seite der Seite (für Breite Browserfenster) oder am oberen Rand der Seite (für schmale Browserfenster).

Weitere Informationen zum Entwickeln von WordPress webapps in Azure finden Sie unter [Entwickeln WordPress auf Azure App Service][WordPressOnAzure]. 

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[Versuchen Sie App Service]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure-Portal]: https://portal.azure.com/
[Projekt-Nami]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png
