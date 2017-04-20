<properties 
    pageTitle="Problembehandlung bei ohne Daten - Anwendung Einblicke für .NET" 
    description="Angezeigt Daten in Visual Studio Application Insights? Versuchen Sie es hier." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Problembehandlung bei ohne Daten - Anwendung Einblicke für .NET

## <a name="some-of-my-telemetry-is-missing"></a>Fehlen einige meiner Telemetrie

*Anwendung Erkenntnisse nur einen Bruchteil der Ereignisse, die von meiner app wird angezeigt.*

* Wenn den entsprechende Bruchteil ständig angezeigt wird, ist es wahrscheinlich durch adaptive [Sampling](app-insights-sampling.md). Zur Bestätigung Suche (aus dem Blade Overview) öffnen und eine Instanz einer Anforderung oder ein anderes Ereignis betrachten. Klicken Sie am unteren Rand des Eigenschaftenbereichs auf "...", um vollständige Angaben zu erhalten. Wenn Count > 1 anfordern und Sampling ist. 
* Andernfalls ist es möglich, dass Sie eine [Datenrate begrenzen](app-insights-pricing.md#limits-summary) für Ihre Zahlungsplan treffen. Diese Grenzwerte gelten pro Minute.

## <a name="no-data-from-my-server"></a>Keine Daten vom server

*Ich meine app auf meinem Webserver installiert, und jetzt keine Telemetriedaten aus angezeigt. Es funktioniert OK auf meinem Entwicklungscomputer.*

* Wahrscheinlich ein Firewallproblem. [Firewallausnahmen für Application Insights zum Senden von Daten festlegen](app-insights-ip-addresses.md).

*Kann ich auf meinem Webserver vorhandenen apps Überwachen [Statusmonitor installiert](app-insights-monitor-performance-live-website-now.md) . Keine Ergebnisse angezeigt.*

* Siehe [Problembehandlung überwachen](app-insights-monitor-performance-live-website-now.md#troubleshooting). 


## <a name="q01"></a>Keine Option "Application Insights hinzufügen" in Visual Studio

*Beim Erstellen eines neuen Projekts in Visual Studio oder beim Rechtsklick auf ein vorhandenes Projekt im Projektmappen-Explorer nicht Anwendung Einblicke Optionen angezeigt.*

+ Die Tools sind nicht alle Arten von .NET Projekt unterstützt. WCF und Projekte werden unterstützt. Für andere Projekttypen wie desktop oder Service können Sie [manuell eine Anwendung Insights SDK zum Projekt hinzufügen](app-insights-windows-desktop.md).
+ Stellen Sie sicher, dass Sie [Visual Studio 2013 Update 3 oder höher](http://go.microsoft.com/fwlink/?LinkId=397827). Es kommt mit Tools für Einblicke vorinstalliert.
+ Wählen Sie **Tools**und **Erweiterungen und Updates** und überprüfen Sie **Anwendung Einblicke Tools** installiert und aktiviert ist. Wenn Ja, klicken Sie auf **Updates** , ob ein Update verfügbar ist.
+ Öffnen Sie das Dialogfeld Neues Projekt, und wählen Sie ASP.NET Web-Anwendung. Wenn die Option Anwendung Einblicke, angezeigt und dann die Tools installiert sind. Wenn dies nicht der Fall ist, deinstallieren und Tools Einblicke Anwendung erneut installieren.


## <a name="q02"></a>Fehler beim Hinzufügen der Anwendung Einblicke

*Beim Erstellen eines neuen Webprojekts oder beim Anwendung Einblicke in ein vorhandenes Projekt hinzufügen, wird eine Fehlermeldung angezeigt.*

Mögliche Ursachen:

* Fehler bei der Kommunikation mit der Anwendung Erkenntnisse; oder
* Es gibt ein Problem mit Ihrem Azure-Konto;
* Sie haben nur [Lesezugriff auf das Abonnement oder die Gruppe, wo Sie die neue Ressource erstellen wollten](app-insights-resources-roles-access-control.md).

Update:

+ Überprüfen Sie Anmeldeinformationen für rechts ein Azure-Konto bereitgestellt. 
+ Überprüfen Sie in Ihrem Browser auf der [Azure-Portal](https://portal.azure.com)zugreifen. Öffnen Sie und sehen Sie, ob eine Einschränkung.
+ [Anwendung Einblicke in Ihr vorhandenes Projekt hinzufügen](app-insights-asp-net.md): im Projektmappen-Explorer das Projekt rechten Maustaste und wählen "Application Insights hinzufügen".
+ Wenn immer noch nicht funktioniert, verwenden Sie [manuelle Verfahren](app-insights-windows-services.md) Ressource im Portal hinzufügen und dann das SDK zum Projekt hinzufügen. 

## <a name="emptykey"></a>Eine Fehlermeldung "instrumentationsschlüssel darf nicht leer sein"

Sieht aus wie ein Fehler aufgetreten ist, während Sie Einblicke Anwendung oder vielleicht eine Protokollierung Adapter installieren.

Im Projektmappen-Explorer mit der rechten Maustaste `ApplicationInsights.config` und **Application Insights konfigurieren**. Sie erhalten einen Dialog, der lädt Azure anmelden und Application Insights-Ressource erstellen oder eine vorhandene erneut verwenden.


##<a name="NuGetBuild"></a>"NuGet-Pakete fehlen" auf dem Buildserver

*Alles erstellt OK, bin ich auf meinem Entwicklungscomputer debuggen, als Fehlermeldung ein NuGet auf dem Buildserver.*

Siehe [NuGet Package](http://docs.nuget.org/Consume/Package-Restore) und [Automatische Paket wiederherstellen](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Fehlender Befehl Anwendung Einblicke in Visual Studio öffnen

*Wenn ich mein Projekt Projektmappen-Explorer mit der rechten Maustaste, Application Insights Befehle nicht angezeigt oder einen offenen Anwendung Einblicke Befehl nicht angezeigt.*

Mögliche Ursachen:

* Application Insights-Ressource manuell erstellt oder das Projekt ist, die von Application Insights-Tools nicht unterstützt.
* Application Insights-Tools sind in Visual Studio deaktiviert.
* Visual Studio ist älter als 2013 Update 3.

Update:

* Stellen Sie sicher, dass Ihre Visual Studio 2013 Update 3 oder höher ist.
* Wählen Sie **Tools**und **Erweiterungen und Updates** und überprüfen Sie **Anwendung Einblicke Tools** installiert und aktiviert ist. Wenn Ja, klicken Sie auf **Updates** , ob ein Update verfügbar ist.
* Klicken Sie auf das Projekt im Projektmappen-Explorer. Wenn Befehl **Konfigurieren Anwendung Erkenntnisse**finden können sie das Projekt mit der Ressource im Application Insights-Dienst herstellen.


Andernfalls wird Projekttyp direkt von Application Insights-Tools nicht unterstützt. Anzeigen der Telemetrie [Azure-Portal](https://portal.azure.com)anmelden, wählen Sie Application Insights auf der linken Navigationsleiste und wählen Sie Ihre Anwendung.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>"Zugriff verweigert" Anwendung Einblicke in Visual Studio öffnen

*Menübefehl "Öffnen Anwendung Einblicke" werde auf Azure-Portal, aber eine Fehlermeldung "Zugriff verweigert".*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

Die Microsoft-Anmeldung, die zuletzt im Standardbrowser verwendet keinen Zugriff auf [die Ressource, die erstellt wurde, wenn diese app Anwendung Einblicke hinzugefügt wurde](app-insights-asp-net.md). Es gibt zwei Gründe: 

* Sie haben mehrere Microsoft - vielleicht Arbeit und persönlichen Microsoft-Konto? Das anmelden, das Sie zuletzt im Standardbrowser verwendet wurde für ein anderes Konto als das hinzuzufügende [Anwendung Einblicke in das Projekt](app-insights-asp-net.md)zugreifen. 

 * Update: Klicken Sie auf Ihren Namen oben rechts im Browserfenster und Abmelden. Melden Sie sich mit dem Konto, das Zugriff hat. Klicken Sie auf der linken Navigationsleiste auf Anwendung Einblicke und wählen Sie Ihre app.

* Jemand Anwendung Einblicke zum Projekt hinzugefügt, und vergessen sie [Ressource](app-insights-resources-roles-access-control.md) zugreifen in der es erstellt wurde. 

 * Update: Ob sie Organisationseinheiten verwendet, können sie Sie dem Team hinzufügen. oder können Sie individuellen Zugriff auf die Ressource gewähren.



## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>'Anlage nicht gefunden' Anwendung Einblicke in Visual Studio öffnen

*Menübefehl "Öffnen Anwendung Einblicke" werde auf Azure-Portal, aber eine Fehlermeldung "Anlage nicht gefunden".*

Mögliche Ursachen:

* Application Insights-Ressource für die Anwendung wurde gelöscht; oder
* Instrumentationsschlüssel wurde festgelegt oder geändert in ApplicationInsights.config bearbeiten, ohne die Projektdatei zu aktualisieren. 

Die instrumentationsschlüssel in ApplicationInsights.config Steuerelemente, die Telemetrie gesendet wird. Eine Zeile in der Projektdatei steuert die Ressource geöffnet wird, wenn der Befehl in Visual Studio. 

Update:

* Im Projektmappen-Explorer das Projekt Maustaste, und wählen Anwendung Einblicke Application Insights konfigurieren. Im Dialogfeld können Sie entweder senden Telemetrie einer vorhandenen Ressource oder eine neue erstellen. Oder:
* Öffnen Sie die Ressource direkt. Melden Sie [Azure-Portal an](https://portal.azure.com), klicken Sie auf der linken Navigationsleiste auf Anwendung Einblicke und wählen Sie Ihre app.



## <a name="where-do-i-find-my-telemetry"></a>Wo finde ich meine Telemetrie?

*[Microsoft Azure-Portal](https://portal.azure.com)signiert und suche Azure home Dashboard. Und wo finden ich meine Anwendung Statistikdaten?*

* Klicken Sie auf der linken Navigationsleiste auf Anwendung Einblicke und Namen. Haben Sie alle Projekte enthält, müssen Sie [Hinzufügen oder Konfigurieren von Anwendung Einblicke in das Webprojekt](app-insights-asp-net.md).

    Dort sehen Sie einige Zusammenfassung Diagramme. Sie können Sie weitere Details klicken.

* In Visual Studio beim Debuggen Ihrer Anwendung klicken Sie Anwendung Einblicke.


## <a name="q03"></a>Keine Serverdaten (oder gar keine Daten)

*Ich führte Meine app und Application Insights-Dienst in Microsoft Azure geöffnet, aber die Diagramme "Informationen sammeln..." oder "Nicht konfiguriert."* Oder *Seite und Benutzerdaten nur keine Serverdaten.*

+ Führen Sie die Anwendung im Debugmodus in Visual Studio (F5). Die Anwendung um einige Telemetriedaten zu generieren. Überprüfen Sie, dass im Visual Studio-Ausgabefenster protokollierten Ereignisse angezeigt. 

    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)

+ Öffnen Sie im Portal Application Insights [Diagnose suchen](app-insights-diagnostic-search.md). Daten in der Regel hier zunächst.
+ Klicken Sie auf die Schaltfläche Aktualisieren. Das Blade wird automatisch in regelmäßigen Abständen aktualisiert, aber Sie können auch manuell erledigen. Das Aktualisierungsintervall ist für größere Zeiträume.
+ Überprüfen Sie die Instrumentation übereinstimmen. Schauen Sie auf die wichtigsten für Ihre Anwendung im Portal Anwendung Einblicke in der Dropdownliste **Essentials** **instrumentationsschlüssel**. Projekt in Visual Studio öffnen ApplicationInsights.config und finden die `<instrumentationkey>`. Überprüfen, ob zwei Schlüssel gleich sind. Wenn nicht,:
 + Im Portal Anwendung Einblicke und auf die app-Ressource mit der rechten Taste; oder
 + Im Visual Studio-Projektmappen-Explorer Maustaste das Projekt, und wählen Sie Anwendung Einblicke konfigurieren. Zurücksetzen der app Telemetrie an die richtige Ressource senden.
 + Die passenden Schlüssel gefunden, sicher, dass in Visual Studio die gleichen Anmeldeinformationen zum Portal verwenden.

    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
    
+ Betrachten Sie im [Hause Microsoft Azure-Dashboard](https://portal.azure.com)Dienststatus zuordnen. Gibt Hinweise Warnung, warten sie OK zurückgegeben schließen und erneut öffnen der Anwendung Einblicke Anwendung Blade.
+ Auch [Status Blog](http://blogs.msdn.com/b/applicationinsights-status/)überprüfen.
+ Schrieben Sie Code für die [serverseitige SDK](app-insights-api-custom-events-metrics.md) , das ändern den instrumentationsschlüssel in `TelemetryClient` Instanzen oder `TelemetryContext`? Oder haben Sie [Filter oder Sampling-Konfiguration](app-insights-api-filtering-sampling.md) , das Filtern möglicherweise zu viel?
+ Wenn Sie ApplicationInsights.config bearbeitet, überprüfen Sie die Konfiguration von [TelemetryInitializers und TelemetryProcessors](app-insights-api-filtering-sampling.md). Eine falsch benannte oder Parametertyp kann das SDK keine Daten senden.

## <a name="q04"></a>Keine Daten Seitenaufrufe, Browser, Verwendung

*Daten in Antwortzeit und Anfragen, jedoch keine Daten angezeigt Seite Ansicht laden und Blades Browser oder Verwendung.*

Die Daten stammen aus Skripts in Webseiten. 

+ Wenn Sie einem vorhandenen Webprojekt [müssen die Skripts manuell hinzufügen](app-insights-javascript.md)Anwendung Einblicke hinzugefügt.
+ Stellen Sie sicher, dass Internet Explorer die Website im Kompatibilitätsmodus angezeigt.
+ Verwenden des Browsers Debug (F12 in einigen Browsern wählen Sie Netzwerk) zu überprüfen, ob Daten gesendet werden `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Keine Abhängigkeit oder Ausnahme Daten

Siehe [Abhängigkeit Telemetrie](app-insights-asp-net-dependencies.md) und [Ausnahme Telemetrie](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Keine Leistungsdaten

Leistungsdaten (CPU, e/a-Rate usw.) für [Java WebServices](app-insights-java-collectd.md), [Windows desktop-apps](app-insights-windows-desktop.md) [IIS web apps und Dienste bei der Installation überwachen](app-insights-monitor-performance-live-website-now.md)und [Azure Cloud Services](app-insights-azure.md)verfügbar ist. Sie finden es unter Server Settings.

Es ist nicht verfügbar für Azure Websites.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>Keine Daten (Server) seit der Veröffentlichung der Anwendung auf dem server

+ Überprüfen Sie, dass Sie tatsächlich alle Microsoft kopiert. ApplicationInsights-DLLs auf dem Server mit Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll
+ In Ihrer Firewall müssen Sie [einige TCP-Ports](app-insights-ip-addresses.md#data-access-api)geöffnet.
+ Haben Sie einen Proxy verwenden, um aus Ihrem Unternehmensnetzwerk senden, festlegen Sie [DefaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) in Web.config
+ WindowsServer 2008: Stellen Sie sicher, dass die folgenden Updates installiert haben: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).


## <a name="i-used-to-see-data-but-it-has-stopped"></a>Ich Daten anzeigen, aber wurde angehalten

* Überprüfen Sie den [Status Blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Haben Sie Ihre monatliche Kontingent von Datenpunkten getroffen? Öffnen Sie die Settings-Kontingent und Preise. So können Sie Ihren Plan aktualisieren oder für zusätzliche Kapazität bezahlt. [Preismodell](https://azure.microsoft.com/pricing/details/application-insights/)anzeigen


## <a name="i-dont-see-all-the-data-im-expecting"></a>Nicht alle Daten angezeigt, die ich erwarte

Wenn Ihre Anwendung große Datenmengen sendet und Application Insights SDK für ASP.NET Version 2.0.0-beta3 oder höher verwenden, kann [adaptive Sampling](app-insights-sampling.md) -Funktion ausgeführt werden und nur einen Teil der Telemetrie. 

Sie können es deaktivieren, aber nicht empfohlen. Sampling ist so konzipiert, dass verwandte Telemetrie für Diagnosezwecke korrekt übertragen wird. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>Falsche geographische Daten in Benutzer Telemetrie

Stadt, Region und Land Dimensionen von IP-Adressen stammen und nicht immer genau.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Ausnahme "Methode nicht gefunden" auf in Azure Cloud Services

Erstellen .NET 4.6? 4.6 wird nicht automatisch in Azure Cloud Services Rollen unterstützt. [4.6 jeder Rolle installieren](../cloud-services/cloud-services-dotnet-install-dotnet.md) bevor Sie Ihre App.

## <a name="still-not-working"></a>Immer noch funktioniert nicht.

* [Anwendung Einblicke forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

