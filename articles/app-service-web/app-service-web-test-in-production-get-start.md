<properties
    pageTitle="Erste Schritte mit Produktion für Web Apps"
    description="Lernen Sie den Test in Produktion (TiP) Funktion in Azure App Service Web Apps."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-test-in-production-for-web-apps"></a>Erste Schritte mit Produktion für Web Apps

Testen in der Produktion oder live-Tests Ihrer Anwendung über live Kundenverkehr ist ein Entwickler immer ihre [agile](https://en.wikipedia.org/wiki/Agile_software_development) Methodik integrieren Teststrategie. Sie können die Qualität Ihrer Apps mit live in Ihrem Produktionsumfeld künstliche Daten in einer Umgebung testen. Mithilfe der neuen app Benutzer, können Sie auf die tatsächlichen Probleme informiert stehen Ihre app möglicherweise nach bereitgestellt wird. Sie können überprüfen, Funktionalität, Performance und Wert Ihrer app Updates für Lautstärke, Geschwindigkeit und verschiedenen echten Datenverkehr nie in einer Umgebung schätzen können.

## <a name="traffic-routing-in-app-service-web-apps"></a>Verkehr in App Service webapps

Mit dem Feature Streckenführung in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)können direkte Teil live Benutzerverkehr an mindestens eine [Bereitstellung Steckplätze](web-sites-staged-publishing.md)und analysieren Ihre Anwendung mit [Azure Anwendung Einblicke](/services/application-insights/) [Azure HDInsight](/services/hdinsight/)oder ein Drittanbieter-Tool [Neue](/marketplace/partners/newrelic/newrelic/) Relikt die Änderung überprüfen. Sie können z. B. die folgenden Szenarien mit App implementieren:

- Funktionale Fehler entdeckt oder lokalisieren Leistungsengpässe in Ihre Updates vor der Bereitstellung der Website
- Führen Sie aus "kontrollierte testflüge" Änderungen durch Messung Usibility Metriken Beta-Anwendung
- Rampe schrittweise bis ein neues Update und ordnungsgemäß auf die aktuelle Version unten wieder bei Fehler 
- Optimieren Ihrer Anwendung Ergebnisse [A / B-tests](https://en.wikipedia.org/wiki/A/B_testing) oder [multivarianten-Tests](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) in mehreren Bereitstellung Steckplätze

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>Web Apps mit Datenverkehr weiterleiten an

- Ihrer Anwendung führen **Standard** oder **Premium** -Stufe für mehrere Bereitstellung Steckplätze erforderlich ist.
- Um die ordnungsgemäße Funktionsweise erfordert Streckenführung Cookies im Browser des Benutzers aktiviert sein. Routing von Datenverkehr verwendet Cookies, um einen Clientbrowser eine Bereitstellung Steckplatz für die Dauer der Clientsitzung fixieren.
- Datenverkehr Routing unterstützt erweiterte QuickInfo Szenarien durch Azure PowerShell-Cmdlets.

## <a name="route-traffic-segment-to-a-deployment-slot"></a>Datenverkehr Streckenabschnitt Deployment-Steckplatz

Auf der Basisebene in jedem Szenario Tipp Arbeitsplan vordefinierten Prozentsatz der live-Verkehr nicht Produktionsbetrieb Steckplatz. Zu diesem Zweck wie folgt:

>[AZURE.NOTE] Die Schritte wird angenommen haben Sie bereits einen [Steckplatz nicht Produktionsbetrieb](web-sites-staged-publishing.md) und gewünschten Webinhalte app bereits [bereitgestellt](web-sites-deploy.md) ist.

1. [Azure-Portal](https://portal.azure.com/)anmelden.
2. **Klicken Sie in Ihrem Web app Blade** > **Datenverkehr weiterleiten**.
  ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Markieren Sie das Feld, das Weiterleiten von Datenverkehr und den Prozentsatz des gesamten Datenverkehrs wünschen, dann klicken Sie auf **Speichern**.

    ![](./media/app-service-web-test-in-production/02-select-slot.png)

4. Fahren Sie mit der Bereitstellung Steckplatz Blade. Live-Verkehr weitergeleitet, sollte jetzt angezeigt werden.

    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

Nach Weiterleitung von Datenverkehr konfiguriert ist, wird der angegebene Prozentsatz der Clients zufällig der nicht produktiven Steckplatz weitergeleitet. Allerdings ist es wichtig zu beachten, dass nach ein Client automatisch in einen bestimmten Steckplatz geroutet wird, es "zu diesem Slot für die Dauer der Clientsitzung fixiert werden, wird". Dies mithilfe eines Cookies zum Fixieren der Sitzung. Wenn Sie HTTP-Anfragen untersuchen, finden Sie einen `TipMix` in jeder nachfolgenden Anfrage Cookie.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a>Clientanforderungen in einen bestimmten Steckplatz erzwingen

Neben Automatische Weiterleitung kann App Service Anfragen an einen bestimmten Steckplatz. Dies ist nützlich, wenn Benutzer können opt-in oder opt Out Ihrer Beta-App. Verwenden Sie dazu die `x-ms-routing-name` Parameter Abfragen.

Umleiten von Benutzern mit einem bestimmten Steckplatz `x-ms-routing-name`, achten Sie darauf, dass der Steckplatz Streckenführung Liste bereits hinzugefügt wird. Da Sie explizit in einen Steckplatz weiterleiten möchten, ist der tatsächliche routing Prozentsatz festgelegten unerheblich. Wenn Sie möchten, können Sie "Beta-Link" die Benutzer klicken können, um die Beta-app zuzugreifen erstellen.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>Wählen Sie Benutzer aus Beta-app

Damit Benutzer Ihre Beta-app deaktivieren können, z. B. setzen diesen Link Ihrer Webseite Sie:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>

Die Zeichenfolge `x-ms-routing-name=self` gibt den Steckplatz Produktion. Sobald der Clientbrowser auf den Link zugreifen, nicht Produktion Steckplatz umgeleitet nur jede nachfolgende Anforderung enthält die `x-ms-routing-name=self` Cookie Sitzung Produktion Steckplatz fixiert.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a>Wählen Sie Benutzer im Beta-app

Damit Benutzer Ihre app Beta teilnehmen, beispielsweise dieselbe Abfrage-Parameter auf den Namen des Feldes nicht produktiven festgelegt:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>Weitere Ressourcen ##

-   [Staging-Umgebungen für Web-apps in Azure App Service einrichten](web-sites-staged-publishing.md)
-   [Bereitstellen einer komplexen Anwendung vorhersehbar in Azure](app-service-deploy-complex-application-predictably.md)
-   [Agile-Softwareentwicklung mit Azure App Service](app-service-agile-software-development.md)
-   [Nutzen Sie DevOps-Umgebung für Ihre Web-apps](app-service-web-staged-publishing-realworld-scenarios.md)