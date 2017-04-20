<properties 
    pageTitle="Problembehandlung bei Anwendung Einblicke in einem Java-Webprojekt" 
    description="Problembehandlung – live Java apps Anwendung zum Überwachen." 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/01/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Problembehandlung und f und A für Anwendung Einblicke für Java

Fragen oder Probleme mit [Visual Studio Anwendung Einblicke in Java][java]? Hier sind einige Tipps.


## <a name="build-errors"></a>Buildfehler

*In Eclipse beim Application Insights-SDK über Maven oder Gradle hinzufügen erhalte ich Build oder Prüfsumme Validierungsfehler.*

* Wenn die Abhängigkeit <version> wird mithilfe ein Musters mit Platzhalterzeichen (z.B. (Maven) `<version>[1.0,)</version>` oder (Gradle) `version:'1.0.+'`), geben Sie eine bestimmte Version stattdessen wie `1.0.2`. Die [Versionsinformationen](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) für die neueste Version anzeigen

## <a name="no-data"></a>Keine Daten 

*Ich Application Insights erfolgreich hinzugefügt und führte Meine app, aber ich habe nie Daten im Portal.*

* Warten Sie eine Minute, und klicken Sie auf aktualisieren. Die Diagramme aktualisieren sich regelmäßig, aber Sie können auch manuell aktualisieren. Das Aktualisierungsintervall hängt von den Zeitbereich des Diagramms.
* Überprüfen Sie, ob einen instrumentationsschlüssel definiert in der Datei ApplicationInsights.xml (im Ressourcenordner im Projekt)
* Überprüfen, dass keine `<DisableTelemetry>true</DisableTelemetry>` Knoten in der XML-Datei.
* In der Firewall möglicherweise öffnen Sie TCP-Ports 80 und 443 für ausgehenden Datenverkehr dc.services.visualstudio.com werden. Die [vollständige Liste der Firewallausnahmen](app-insights-ip-addresses.md)
* Starten Sie Microsoft Azure Board, Service Status Karte sehen. Gibt Hinweise Warnung, warten sie OK zurückgegeben schließen und erneut öffnen der Anwendung Einblicke Anwendung Blade.
* Aktivieren Sie Protokollierung im Konsolenfenster IDE durch Hinzufügen einer `<SDKLogger />` Element unter dem Stammknoten in der ApplicationInsights.xml Datei (im Ressourcenordner im Projekt) und Kontrollkästchen Einträge [Fehler] vorangestellt.
* Stellen Sie sicher, dass die richtige Datei ApplicationInsights.xml anhand der Konsole Ausgabenachrichten "Konfigurationsdatei wurde gefunden wurde" Anweisung erfolgreich von Java SDK geladen wurde.
* Die Config-Datei nicht gefunden wird, Nachrichten Sie die Ausgabe anzeigen, die Config-Datei gesucht wird und stellen Sie sicher, dass die ApplicationInsights.xml in einer dieser Positionen befindet. Eine Faustregel können Sie die Konfigurationsdatei in Application Insights SDK JARs platzieren. Beispiel: Dies bedeutet in Tomcat WEB-INF/Lib-Ordner.



#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Ich Daten anzeigen, aber wurde angehalten

* Überprüfen Sie den [Status Blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Haben Sie Ihre monatliche Kontingent von Datenpunkten getroffen? Settings-Kontingent und Preise zu öffnen. So können Sie Ihren Plan aktualisieren oder für zusätzliche Kapazität bezahlt. [Preismodell](https://azure.microsoft.com/pricing/details/application-insights/)anzeigen

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Nicht alle Daten angezeigt, die ich erwarte

* Kontingente und Preise Blade und überprüfen, ob die [Probenahme](app-insights-sampling.md) wird geöffnet. (100 % Transmission bedeutet, dass die Probenahme nicht in Betrieb ist.) Application Insights-Dienst lassen nur einen Bruchteil der Telemetrie akzeptieren, die von Ihrer Anwendung eintreffen. Dies hilft Ihnen in Ihre monatliche Kontingent Telemetrie. 

## <a name="no-usage-data"></a>Keine Daten

*Daten zu Anfragen und Antwortzeiten, aber keine Seitenansicht, Browser oder Benutzerdaten angezeigt.*

Sie soll erfolgreich Ihre app Telemetrie vom Server zu senden. Im nächste Schritt zum [Einrichten von Webseiten Telemetrie im Webbrowser senden]ist[usage].

Auch wenn Ihr Client eine Anwendung in einem [Telefon oder einem anderen Gerät ist][platforms], senden Telemetrie aus. 

Verwenden Sie desselben Schlüssels Instrumentation der Client- und Telemetrie einrichten. Die Daten erscheinen in der gleichen Anwendung Einblicke und korrelieren Sie Ereignisse von Client und Server können.



## <a name="disabling-telemetry"></a>Telemetrie deaktivieren

*Wie kann Telemetrie Auflistung deaktivieren?*

Im Code:

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);


**Oder** 

Aktualisieren Sie ApplicationInsights.xml (im Ressourcenordner im Projekt). Fügen Sie Folgendes unter dem Stammknoten:

    <DisableTelemetry>true</DisableTelemetry>

Die XML-Methode müssen Sie die Anwendung neu starten, wenn Sie den Wert ändern.

## <a name="changing-the-target"></a>Ziel ändern

*Wie kann ich ändern, welche Azure Ressource Projekt Daten sendet?*

* [Den instrumentationsschlüssel der neuen Ressource abrufen.][java]
* Application Insights dem Projekt hinzugefügt, mit dem Azure-Toolkit für Eclipse rechts Webprojekt Klick **Azure** **Konfigurieren Anwendung Einblicke**wählen Sie und ändern Sie den Schlüssel.
* Andernfalls aktualisieren Sie den Schlüssel in ApplicationInsights.xml im Ressourcenordner im Projekt.


## <a name="the-azure-start-screen"></a>Azure-Startbildschirm

*Suche in [Azure-Portal](https://portal.azure.com). Die Karte mir etwas an meiner Anwendung?*

Nein, wird die Integrität der Azure-Server auf der ganzen Welt.

*Azure Start-Platine (Startbildschirm) wie finde Daten über meine app ich?*

Angenommen, Sie [richten Ihre app für Application Insights][java], klicken Sie auf Durchsuchen, wählen Sie Application Insights und die app-Ressource für Ihre Anwendung erstellt. Zu können es in Zukunft schneller Sie Ihre Anwendung an das Startmenü anheften.

## <a name="intranet-servers"></a>Intranet-Server

*Kann ich einen Server Meine Intranet überwachen?*

Ja, vorausgesetzt, Ihr Server Telemetrie an Application Insights-Portal über das Internet senden kann. 

In der Firewall möglicherweise öffnen Sie TCP-Ports 80 und 443 für ausgehenden Datenverkehr an dc.services.visualstudio.com und f5.services.visualstudio.com werden.

## <a name="data-retention"></a>Datenaufbewahrung 

*Wie lange werden Daten im Portal beibehalten? Ist sie sicher?*

[Data Retention und Datenschutz]Siehe[data].

## <a name="next-steps"></a>Nächste Schritte

*Ich einrichten Anwendung Einblicke für meiner Java-Server-Anwendung. Was kann ich tun?*

* [Überwachen der Verfügbarkeit von Webseiten][availability]
* [Webseite überwachen][usage]
* [Nutzung verfolgen und Probleme in Ihren apps Gerät diagnostizieren][platforms]
* [Schreiben Sie Code zum Nachverfolgen der Verwendung Ihrer Anwendung][track]
* [Erfassen von Diagnoseprotokollen][javalogs]


## <a name="get-help"></a>Hilfe

* [Stack-Überlauf](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 