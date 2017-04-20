<properties
    pageTitle="Flighting Bereitstellung in Azure App Service (Beta-Tests)"
    description="Informationen Sie zum Flug neuer Funktionen in Ihrer Anwendung oder Betatests die Updates in diesem Lernprogramm zu Ende. Es bringt App Servicefunktionen wie fortlaufende veröffentlichen, Steckplätze Streckenführung und Application Insights-Integration."
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
    ms.date="02/02/2016"
    ms.author="cephalin"/>
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Flighting Bereitstellung in Azure App Service (Beta-Tests)

Dieses Lernprogramm zeigt wie *flighting Bereitstellung* durch die Integration der verschiedenen Funktionen [Azure App](http://go.microsoft.com/fwlink/?LinkId=529714) und [Azure Anwendung Einblicke](/services/application-insights/). 

*Flighting* ist ein Bereitstellungsprozess, der neue KEs oder Änderung mit einer begrenzten Anzahl von Kunden überprüft und testet eine in Produktionsszenario. Es ähnelt der Betatest und wird manchmal als "gesteuerte testflug" bezeichnet. Viele große Unternehmen mit einer Webpräsenz verwenden dadurch ihre app-Updates in ihrer Praxis der [agilen Entwicklung](https://en.wikipedia.org/wiki/Agile_software_development)zu früh Validierung. Azure App Service können Sie Test in Produktion mit fortlaufenden veröffentlichen Anwendung derselbe DevOps Szenario implementieren integrieren. Dieser Ansatz bietet folgende Vorteile:

- **Gewinn real Feedback _vor_ Produktion aktualisiert werden** - das einzige besser Feedback erhalten, sobald Sie freigeben gewinnt Feedback, bevor Sie freigeben. Testen Sie Updates mit tatsächlichen Benutzern und Verhalten früh im Lebenszyklus eines Produkts wünschen.
- **Verbessern [kontinuierliche testgesteuerte Entwicklung (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development) ** - geschieht durch fortlaufende Integration und Instrumentation-Anwendung zum Testen in der Produktion integrieren Benutzer Validierung frühzeitig und automatisch in die Lebensdauer eines Produkts. Dadurch wird Zeit Investitionen in manuelle Ausführung verringert.
- **Optimieren testen Workflow** - durch die Automatisierung von Tests mit kontinuierlichen Überwachung Instrumentation möglicherweise die Ziele der verschiedenen Arten von Tests in einem einzigen Prozess wie [Integration](https://en.wikipedia.org/wiki/Integration_testing), [Regression](https://en.wikipedia.org/wiki/Regression_testing) [Verwendbarkeit](https://en.wikipedia.org/wiki/Usability_testing), Eingabehilfen, Lokalisierung, [Leistung](https://en.wikipedia.org/wiki/Software_performance_testing), [Sicherheit](https://en.wikipedia.org/wiki/Security_testing)und [Akzeptanz](https://en.wikipedia.org/wiki/Acceptance_testing)erreichen Sie.

Flighting Bereitstellung ist nicht nur Verkehrsinformationen routing. In eine solche Bereitstellung zu möglichst schnell Einblick, seien es einen unerwarteten Fehler, Leistung, Erfahrung Probleme. Beachten Sie, dass Sie mit Kunden arbeiten. So müssen Sie rechts und Sie sicherstellen, dass flighting Bereitstellung das Erfassen der Daten, die Sie eine fundierte Entscheidung für den nächsten Schritt müssen eingerichtet haben. In diesem Lernprogramm wird veranschaulicht, wie Daten Anwendung zum Sammeln, jedoch können Sie neue Relikt oder anderen Techniken, die das Szenario geeignet ist. 

## <a name="what-you-will-do"></a>Vorgehensweise

In diesem Lernprogramm erfahren Sie, wie die folgenden Szenarien zusammen, um Ihre App Service-Anwendung in der Produktion zu testen:

- Ihre Beta-app [Produktionsverkehr](app-service-web-test-in-production-get-start.md)
- [Instrument Ihrer app](../application-insights/app-insights-web-track-usage.md) zu nützliche Metriken
- Kontinuierlich Ihre Beta-app Bereitstellung und Verfolgung von live app Metriken
- Vergleichen Sie Metriken zwischen Produktion app und Beta anzeigen wie übersetzen ändert sich Ergebnisse

## <a name="what-you-will-need"></a>Sie benötigen

-   Ein Azure-Konto
-   [GitHub](https://github.com/) -Konto
- Sie können Visual Studio 2015 - [Community Edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx)herunterladen.
-   Git Shell (mit [GitHub für Windows](https://windows.github.com/)installiert) - dadurch der Git und PowerShell Befehle in derselben Sitzung ausführen
-   Neueste [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) -bits
-   Grundlegendes Verständnis der folgenden:
    -   Bereitstellung von [Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md) -Vorlage (siehe [Bereitstellen einer komplexen Anwendung in Azure vorhersehbar](app-service-deploy-complex-application-predictably.md))
    -   [Git](http://git-scm.com/documentation)
    -   [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Ein Azure-Konto zum Bearbeiten dieses Lernprogramms benötigen Sie:
> + Sie können [ein Azure-Konto frei](/pricing/free-trial/) - Sie erhalten ein Guthaben bezahlt Azure Services ausprobieren können und sogar nachdem sie verbraucht das Konto beibehalten mit kostenlosen Azure Services wie Web Apps.
> + Kann [Visual Studio Abonnementvorteile aktivieren](/pricing/member-offers/msdn-benefits-details/) - der Visual Studio-Abonnement bietet Ihnen Credits monatlich für bezahlte Azure Services verwendet werden können.
>
> Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="set-up-your-production-web-app"></a>Richten Sie Ihre Produktion Web app

>[AZURE.NOTE] In diesem Lernprogramm verwendeten Skripts konfiguriert automatisch fortlaufende Veröffentlichung von GitHub-Repository. Dies erfordert, dass Anmeldedaten GitHub sind bereits in Azure, andernfalls skriptbasierte Bereitstellung fehl beim Source Control für Web-apps konfigurieren.
>
>Erstellen Sie zum Speichern Ihrer Anmeldeinformationen GitHub in Azure Web app in [Azure-Portal](https://portal.azure.com/) und [GitHub Bereitstellung konfigurieren](app-service-continuous-deployment.md#Step7). Sie müssen nur einmal erforderlich.

In einer Situation DevOps eine Anwendung, die aktiv in Azure ausgeführt wird und Sie durch kontinuierliche Veröffentlichung ändern möchten. In diesem Szenario wird für die Produktion eine Vorlage bereitstellen, die Sie entwickelt und getestet.

1.  Erstellen Sie eigener Verzweigung des Repositorys [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) . Informationen zum Erstellen Ihrer Gabel anzeigen Sie [Gabel Repo](https://help.github.com/articles/fork-a-repo/) Nach Erstellung der Verzweigung es im Browser angezeigt.

    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Git Shell-Sitzung zu öffnen. Wenn Sie Git Shell noch nicht, installieren Sie jetzt [GitHub für Windows](https://windows.github.com/) .
3.  Klonen Sie lokalen Gabel durch den folgenden Befehl ausführen:

        git clone https://github.com/<your_fork>/ToDoApp.git

4.  Haben Sie Ihre lokalen Klon erstellen, navigieren Sie zu * &lt;Repository_root >*\ARMTemplates und die deploy.ps1 mit einem Skript mit ein eindeutiges Suffix wie folgt ausführen:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix>

4.  Wenn Sie aufgefordert werden, geben Sie den gewünschten Benutzernamen und Kennwort für den Zugriff auf. Beachten Sie Ihre Anmeldeinformationen, da diese Ressourcengruppe aktualisieren erneut angeben müssen.

    Bereitstellung Fortschritt der verschiedenen Azure Ressourcen sollte angezeigt werden. Nach Abschluss der Bereitstellung wird das Skript starten Sie die Anwendung im Browser und Ihnen angezeigten Signalton.
    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)

6.  Wieder in der Git Shell-Sitzung ausgeführt:

        .\swap –Name ToDoApp<your_suffix>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)

7.  Nach Abschluss des Skripts auf die Front-End-Adresse zurück (http://ToDoApp*&lt;Your_suffix >*.azurewebsites.net/) an die Anwendung in der Produktion.
5.  [Azure-Portal](https://portal.azure.com/) melden Sie an und einen Blick auf erstellten.

    Sie sollen finden zwei Web-apps in derselben Ressourcengruppe, mit der `Api` Suffix im Namen. Betrachten Sie die Ressourcenansicht Gruppe, sehen Sie auch die SQL-Datenbank und Server App Service-Plan und staging-Steckplätze für Web-apps. Die verschiedenen Ressourcen durchsuchen und vergleichen Sie sie mit * &lt;Repository_root >*\ARMTemplates\ProdAndStage.json finden Sie unter Konfiguration in der Vorlage.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

Sie haben die Produktions-app einrichten.  Nun stellen Sie Feedback erhalten, Verwendbarkeit für die Anwendung ist. Damit Sie untersuchen möchten. Sie wollen Ihre app Geben Sie Feedback zu instrumentieren.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Untersuchen: Instrumentieren Sie die Clientanwendung für Überwachung/Metriken

5. Open * &lt;Repository_root >*\src\MultiChannelToDo.sln in Visual Studio.
6. Nuget-Pakete mit der rechten Maustaste Lösung wiederherstellen > **NuGet-Pakete verwalten, Lösung** > **Wiederherstellen**.
6. Mit der rechten Maustaste **MultiChannelToDo.Web** > **Anwendung Einblicke Telemetrie hinzufügen** > **Configure Settings** > ändern Ressourcengruppe ToDoApp*&lt;Your_suffix >* > **Application Insights zu Projekt hinzufügen**.
7. Öffnen Sie in Azure-Verwaltungsportal Blade **MultiChannelToDo.Web** Application Insight-Ressource. Dann **Anwendungszustands** , klicken Sie auf **Informationen zum Browser Seite laden Datensammlung** > Code kopieren.
7. Fügen Sie den kopierten JS Instrumentationscode, * &lt;Repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml vor dem `<heading>` Tag. Es sollte eindeutig instrumentationsschlüssel der Ressource Application Insight.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>

11. Senden Sie benutzerdefinierte Ereignisse Anwendung Einblicke für Maus klickt unten folgenden Code hinzufügen:

        <script>
            $(document.body).find("*").click(function(event) {

                appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
            });
        </script>

    Dieser Ausschnitt JavaScript sendet ein benutzerdefiniertes Ereignis Einblicke Anwendung jedes Mal ein Benutzer überall im Web app klickt.

12. Git Shell commit und Änderungen zu Ihrem GitHub drücken. Warten Sie auf Clients Browser aktualisieren.

        git add -A :/
        git commit -m "add AI configuration for client app"
        git push origin master

6.  Austauschen der bereitgestellten Anwendung ändert sich in Produktion:

        .\swap –Name ToDoApp<your_suffix>

13. Wechseln Sie zu der Anwendung Insights-Ressource, die Sie konfiguriert. Klicken Sie auf benutzerdefinierte Ereignisse.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

    Wenn Metriken für benutzerdefinierte Ereignisse nicht angezeigt wird, warten Sie einige Minuten und klicken Sie auf **Aktualisieren**.

Angenommen, Sie finden Sie ein Diagramm wie folgt:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

Und das Ereignis darunter:

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

Nach ToDoApp Anwendungscode-Ereignis **der Schaltfläche** entspricht Absenden und **Eingabe** Ereignis entspricht dem Textfeld. Bisher sind Dinge sinnvoll. Es scheint jedoch gibt es viel Klicks und wenigen Mausklicks auf Aufgaben ( **LI** -Ereignisse).

Basierend auf diesem Formular Sie Ihre Hypothese einige Benutzer verwirrt welcher Teil der Benutzeroberfläche geklickt werden kann und, da der Cursor für die Textauswahl Listenelemente und deren Symbole Zeigt Stil.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

Dies kann eine erfundene Beispiel sein. Allerdings benötigen Sie eine Verbesserung Ihrer Anwendung, und führen Sie eine flighting Bereitstellung von live-Kunden Verwendbarkeit Feedback erhalten.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>Die Server-app für Überwachung/Metriken Instrument
Dies ist eine Tangente, da nur das Szenario in diesem Lernprogramm demonstriert die Clientanwendung behandelt. Jedoch werden der Vollständigkeit Sie serverseitige Anwendung einrichten.

6. Mit der rechten Maustaste **MultiChannelToDo** > **Anwendung Einblicke Telemetrie hinzufügen** > **Configure Settings** > ändern Ressourcengruppe ToDoApp*&lt;Your_suffix >* > **Application Insights zu Projekt hinzufügen**.
12. Git Shell commit und Änderungen zu Ihrem GitHub drücken. Warten Sie auf Clients Browser aktualisieren.

        git add -A :/
        git commit -m "add AI configuration for server app"
        git push origin master

6.  Austauschen der bereitgestellten Anwendung ändert sich in Produktion:

        .\swap –Name ToDoApp<your_suffix>

Das wars!

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a>Untersuchen: Client app metrischen Steckplatz-spezifische Tags hinzufügen

In diesem Abschnitt Konfigurieren Sie die verschiedenen Steckplätze Steckplatz-spezifische Telemetrie derselben Anwendung Einblicke Ressource senden. So können Sie Daten zwischen Datenverkehr von verschiedenen Steckplätze (Deployment-Umgebung) vergleichen, um Ihre app Änderungen leicht erkennen. Zur gleichen Zeit können Sie Produktionsverkehr vom Rest trennen, damit Sie Ihre app Produktion weiterhin können nach Bedarf.

Da beim Sammeln von Daten auf Client wird im index.cshtml [einen Initialisierer Telemetrie an JavaScript-Code hinzufügen](../application-insights/app-insights-api-custom-events-metrics.md#js-initializer) . Wenn Sie serverseitige Leistung testen möchten, z. B. auch möglich ebenso in Ihrem Servercode (siehe [Application Insights API und benutzerdefinierte Ereignisse](../application-insights/app-insights-api-custom-events-metrics.md).

1. Fügen Sie zunächst den Code zwischen den beiden `//` Kommentare unten in JavaScript zu blockieren, die Sie hinzugefügt die `<heading>` tag früher.

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    Dieser Initialisierung Code bewirkt die `appInsights` hinzuzufügende Objekt der benutzerdefinierten Eigenschaft `Environment` auf alle Telemetrie sendet.

2. Fügen Sie diese benutzerdefinierte Eigenschaft ein [Steckplatz Einstellung](web-sites-staged-publishing.md#AboutConfiguration) Ihrer Anwendung in Azure. Führen Sie hierzu die folgenden Befehle in der Git Shell-Sitzung.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    Die Web.config-Datei in Ihrem Projekt bereits definiert die `environment` Anwendung festlegen. Diese Einstellung, wenn Sie die Anwendung lokal testen metrischen markiert mit `VS Debugger`. Aber wenn push Änderungen in Azure Azure finden und verwenden die `environment` app-Einstellung in der Web-app-Konfiguration und die Metriken mit markierten `Production`.

3. Commit geändertem Code die Aufspaltung GitHub drücken, und warten Sie, bis die Benutzer mit der neuen app (Browser aktualisieren müssen). Ca. 15 Minuten für die neue Eigenschaft in Ihre Anwendung Erkenntnisse angezeigt `MultiChannelToDo.Web` Ressource.

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master

4. Jetzt wieder an die **benutzerdefinierte Ereignisse** Blade und Filtern die Metriken auf `Environment=Production`. Jetzt werden können Sie alle neuen benutzerdefinierten Ereignisse Produktions-Steckplatz mit diesem Filter.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)

5. Klicken Sie auf **Favoriten** , um etwas Metriken Explorer-Einstellungen speichern **benutzerdefinierte Ereignisse: Produktion**. Sie können zwischen dieser Ansicht und eine Bereitstellung Steckplatz später problemlos.

    > [AZURE.TIP] Leistungsstärkere Analysen sollten Sie [Anwendung Einblicke Ressource mit Power BI integrieren](../application-insights/app-insights-export-power-bi.md).

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a>Die Server app Metriken Steckplatz-spezifische Tags hinzufügen
Wieder werden Vollständigkeit Sie serverseitige Anwendung einrichten. Im Gegensatz zu der Clientanwendung, die in JavaScript instrumentiert, Steckplatz-spezifische Tags für die Serveranwendung mit instrumentiert.

1. Open * &lt;Repository_root >*\src\MultiChannelToDo\Global.asax.cs. Fügen Sie den Codeblock, unmittelbar vor dem schließenden geschweiften Klammer Namespace.

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }

2. Korrigieren der diesbezügliche durch Hinzufügen der `using` Aussagen unten am Anfang der Datei:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;

3. Fügen Sie den folgenden Code an die `Application_Start()` Methode:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());

3. Commit geändertem Code die Aufspaltung GitHub drücken, und warten Sie, bis die Benutzer mit der neuen app (Browser aktualisieren müssen). Ca. 15 Minuten für die neue Eigenschaft in Ihre Anwendung Erkenntnisse angezeigt `MultiChannelToDo` Ressource.

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>Update: Beta Zweigstellen einrichten

2. Open * &lt;Repository_root >*\ARMTemplates\ProdAndStagetest.json und finden die `appsettings` Ressourcen (Suchen nach `"name": "appsettings"`). Es sind 4, für jeden Steckplatz. 

2. Für jeden `appsettings` Ressource hinzufügen ein `"environment": "[parameters('slotName')]"` app-Einstellung am Ende der `properties` Array. Vergessen Sie nicht, die vorherige Zeile mit einem Komma.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)
    
    Sie haben soeben die `environment` app-Einstellung auf alle Slots in der Vorlage.
    
2. Suchen Sie in derselben Datei der `slotconfignames` Ressourcen (Suchen nach `"name": "slotconfignames"`). Es gibt 2, für jede Anwendung.

2. Für jeden `slotconfignames` Ressource hinzufügen `"environment"` am Ende der `appSettingNames` Array. Vergessen Sie nicht, die vorherige Zeile mit einem Komma.

    Sie haben nur die `environment` app Stick auf ihrem jeweiligen Bereitstellung Steckplatz für beide apps festlegen.  

3. Führen Sie die folgenden Befehle mit derselben Ressource Gruppe Suffix, dem Sie verwendet, in der Git Shell-Sitzung.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta

4. Geben Sie bei Aufforderung die gleichen SQL Anmeldeinformationen als vor. Befragt die Ressourcengruppe aktualisieren geben Sie `Y`, dann `ENTER`.

    Skripts, die Ressourcen in der ursprünglichen Ressourcengruppe werden beibehalten, aber ein neues Feld namens "Beta" wird es mit der Konfiguration "Staging" Steckplatz erstellt, die am Anfang erstellt wurde.

    >[AZURE.NOTE] Diese Methode zum Erstellen von verschiedenen Umgebungen unterscheidet die [Agile-Softwareentwicklung mit Azure App Service](app-service-agile-software-development.md)-Methode. Hier erstellen Sie Deployment-Umgebung mit der Bereitstellung, wo es bereitstellungsumgebungen Ressourcengruppen erstellen. Verwalten Bereitstellung Umgebung mit Ressourcengruppen ermöglicht Ihnen die produktionsumgebung gesperrt für Entwickler, aber es ist nicht einfach testen in der Produktion, Sie einfach mit der.

Wenn Sie möchten, können Sie auch eine alpha app mit erstellen

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

Für dieses Lernprogramm wird nur Ihre Beta-app Verwendung.

## <a name="update-push-your-updates-to-the-beta-app"></a>Update: Push Updates Beta-App

Um Ihre Anwendung zu verbessern.

1. Stellen Sie sicher, dass Sie jetzt in der Beta-Verzweigung

        git checkout beta

2. In * &lt;Repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, finden die `<li>` tag und Hinzufügen der `style="cursor:pointer"` Attribut, wie unten dargestellt.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)

3. Commit und Push in Azure.

4. Stellen Sie sicher, dass die Änderung jetzt im Beta-Steckplatz angezeigt durch Navigieren zur http://todoapp*&lt;Your_suffix >*-beta.azurewebsites.net/. Wenn Sie die Änderung noch nicht angezeigt wird, aktualisieren Sie Ihren Browser zu den Javascript-Code.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Jetzt haben Sie die Änderung im Steckplatz Beta können Sie flighting Bereitstellung durchführen.

## <a name="validate-route-traffic-to-the-beta-app"></a>Überprüfen: Datenverkehr zur Beta-app

In diesem Abschnitt werden Sie die Beta-app Datenverkehr. Aus Gründen der Übersichtlichkeit Demo benötigen Sie einen Großteil des Datenverkehrs an sie weiterleiten. In Wirklichkeit hängen den Datenverkehr weiterleiten möchten Ihre Situation. Beispielsweise wenn Ihre Website auf microsoft.com, müssen Sie weniger als ein Prozent des gesamten Datenverkehrs möglicherweise nützliche Daten erlangen.

1. Führen Sie die folgenden Befehle Hälfte Produktionsverkehr Beta-Steckplatz weitergeleitet, in der Git Shell-Sitzung:

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

  Die `ReroutePercentage=50` -Eigenschaft gibt an, dass 50 % des Produktion Beta app-URL weitergeleitet wird (durch die `ActionHostName` Eigenschaft).

2. Navigieren Sie nun zu http://ToDoApp*&lt;Your_suffix >*. *.azurewebsites.NET. 50 % des Datenverkehrs sollten jetzt Beta-Steckplatz umgeleitet werden.

3. Application Insights-Ressource filter die Metriken Umgebung = "Beta".

    > [AZURE.NOTE] Speichern diese gefilterte Ansicht einer anderen Favoriten können Sie metrische Explorer-Ansichten zwischen Produktions- und Beta Ansichten einfach umdrehen.

Angenommen, in Application Insights ähnlich der folgenden angezeigt:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Nicht nur Dies zeigt, dass es viele weitere Klicks auf die `<li>` Tags scheint zu einer Überspannung auf `<li>` Tags. Sie können dann feststellen, dass die Leute die neue entdeckt `<li>` Tags klicken, werden nun alle ihre zuvor abgeschlossenen Aufgaben in der Anwendung deaktivieren.

Basierend auf den Daten der flighting Bereitstellung entscheiden Sie, dass die neue Benutzeroberfläche für die Produktion bereit ist.

## <a name="go-live-move-your-new-code-into-production"></a>Produktivstart: Verschieben Sie den neuen Code in die Produktion

Sie nun können die Aktualisierung für die Produktion zu verschieben. Großartige ist, dass sie jetzt wissen, dass die Aktualisierung bereits überprüften _vor_ Produktion abgelegt ist. Nun können Sie sicher bereitstellen. Da Sie eine Clientanwendung AngularJS aktualisiert, werden nur den clientseitigen Code überprüft. Wäre die Backend-Web API-app ändern konnte Änderungen entsprechend und leicht überprüft werden.

1. Entfernen Sie in der Git Schale die Routingregel Datenverkehr durch den folgenden Befehl ausführen:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()

2. Git Befehle ausführen:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master

2. Warten Sie einige Minuten den neuen Code im staging-Steckplatz bereitgestellt werden, und starten Sie http://ToDoApp*&lt;Your_suffix >*-staging.azurewebsites.net überprüfen, ob das neue Update im staging-Steckplatz erwärmt wird. Beachten Sie, dass der master Zweig der Verzweigung staging-Steckplatz der Anwendung verknüpft ist.

3. Swap, staging-Steckplatz in Produktion

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Zusammenfassung ##

Azure App Service erleichtert für kleine, mittlere Unternehmen ihre apps Kunden etwas in der Produktion, testen die traditionell in großen Unternehmen. Hoffentlich haben Sie dieses Lernprogramm erhalten, wissen möchten App Service und Anwendung Einblicke zu flighting Bereitstellung und auch andere Szenarien testen Produktion Ihrer DevOps Welt. 

## <a name="more-resources"></a>Weitere Ressourcen ##

-   [Agile-Softwareentwicklung mit Azure App Service](app-service-agile-software-development.md)
-   [Staging-Umgebungen für Web-apps in Azure App Service einrichten](web-sites-staged-publishing.md)
-   [Bereitstellen einer komplexen Anwendung vorhersehbar in Azure](app-service-deploy-complex-application-predictably.md)
-   [Azure Resource Manager Vorlagen erstellen](../resource-group-authoring-templates.md)
-   [JSONLint - die JSON-Bestätigung](http://jsonlint.com/)
-   [Git Verzweigen – grundlegende Verzweigen und Zusammenführen](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [Azure PowerShell](../powershell-install-configure.md)
-   [Projekt Kudu Wiki](https://github.com/projectkudu/kudu/wiki)
