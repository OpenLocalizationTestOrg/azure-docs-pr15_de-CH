<properties 
    pageTitle="Versionshinweise für Anwendung Einblicke für Windows" 
    description="Die neuesten Updates für Windows Store-SDK." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/12/2016" 
    ms.author="joshweb"/>
 
# <a name="release-notes-for-application-insights-sdk-for-windows-phone-and-store"></a>Versionshinweise für Application Insights SDK für Windows Phone und speichern

Application Insights SDK sendet Telemetriedaten live App [Anwendung Einblicke](https://azure.microsoft.com/services/application-insights/), wo Sie deren Verwendung und Leistung analysieren können.


## <a name="version-111"></a>Version 1.1.1

### <a name="windows-sdk"></a>Windows SDK

- Beheben Sie hängen beim Absturz Verwendung Windows Phone Silverlight-SDK. Nach dieser Änderung jeder Absturz spätestens ca. 2 Sekunden nach dem Aufruf von WindowsAppInitialier.InitializeAsync(...) erfolgt beibehalten werden Datenträger und erhalten den nächsten Start die Anwendung. Tritt ein Absturz vor ca. 2 Sekunden nach dem Aufruf wird ignoriert.  
- Festlegen der NuGet Abhängigkeiten auf eine bestimmte Version und Microsoft.ApplicationInsights.PersistenceChannel (v1.2.3).   

### <a name="core-sdk"></a>Core SDK

- Core erfolgt in Github. Zukünftige Versionsinformationen des Core SDK finden Sie [im github](http://github.com/Microsoft/ApplicationInsights-dotnet/releases)

## <a name="version-11"></a>Version 1.1

### <a name="core-sdk"></a>Core SDK

- SDK jetzt führt neue Telemetrie ```DependencyTelemetry``` enthält Informationen über Abhängigkeit von Anwendung
- Neue Methode ```TelemetryClient.TrackDependency``` Informationen zur Abhängigkeit Aufrufe von Anwendung senden können

## <a name="version-100"></a>Version 1.0.0

### <a name="windows-app-sdk"></a>Windows App SDK

- Neue Initialisierung für Windows-Apps. Neue `WindowsAppInitializer` Klasse mit `InitializeAsync()` -Methode bootstrapping Initialisierung der SDK-Auflistung ermöglicht. Diese Änderung zulassen mehr Kontrolle und erhebliche app Initialisierung Leistungssteigerungen über vorherige ApplicationInsights.config Verfahren
- DeveloperMode wird nicht mehr automatisch festgelegt. DeveloperMode Verhalten ändern müssen Sie im Code angeben.
- NuGet-Paket fügt nicht mehr ApplicationInsights.config. Empfehlen Sie neue WindowsAppInitializer verwenden, wenn Sie NuGet-Paket manuell hinzufügen.
- Lesezugriff auf ApplicationInsights.config `<InstrumentationKey>`, alle anderen Einstellungen zugunsten WindowsAppInitializer Einstellungen ignoriert.
- Markt wird automatisch vom SDK gesammelt werden.
- Viele Bugfixes, Stabilität und Performance verbessert.

### <a name="core-sdk"></a>Core SDK

- ApplicationInsights.config Datei ist nicht mehr Requiered. Und das NuGet-Paket nicht hinzugefügt. Konfiguration kann vollständig in Code angegeben werden.
- NuGet-Paket wird die Projektmappe nicht mehr eine Targets-Datei hinzufügen. Automatische Einstellung der DeveloperMode in einem Debugbuild wird entfernt. DeveloperMode muss manuell im Code festgelegt werden.

## <a name="version-017"></a>Version 0,17

### <a name="windows-app-sdk"></a>Windows App SDK

- Windows App SDK unterstützt universelle Windows-Apps für Windows 10 technical Preview und mit VS 2015 RC.

### <a name="core-sdk"></a>Core SDK

- TelemetryClient wird standardmäßig mit der InMemoryChannel initialisiert.
- Neue API hinzugefügt, `TelemetryClient.Flush()`. Flush-Methode löst sofort blockieren Upload alle Telemetrie für den Client protokolliert. Dadurch werden manuelle Auslösung des Uploads vor dem Beenden des Prozesses.
- NuGet-Paket hinzugefügt .net 4.5 Ziel. Dieses Ziel wurde keine externen Abhängigkeiten (entfernte BCL und EventSource Abhängigkeiten).

## <a name="version-016"></a>Version 0,16 

2015-04-28-Vorschau

- SDK unterstützt jetzt DNX Zielplattform zum Aktivieren der Überwachung des [zentralen .NET Framework](http://www.dotnetfoundation.org/NETCore5) Applications.
- Instanzen von ```TelemetryClient``` Instrumentationsschlüssel nicht mehr zwischengespeichert. Instrumentationsschlüssel festgelegt wurde nun ```TelemetryClient``` explizit ```InstrumentationKey``` null zurück. Behebt ein Problem, bei ```TelemetryConfiguration.Active.InstrumentationKey``` einige Telemetriedaten zusammengestellt wurde, Telemetrie-Module wie Abhängigkeit Collector, Web-Anfragen Collection und Leistung Leistungsindikatoren Datensammler neuen instrumentationsschlüssel verwendet.

## <a name="version-015"></a>Version 0,15

- Neue Eigenschaft ```Operation.SyntheticSource``` jetzt verfügbar auf ```TelemetryContext```. Sie können Ihre telemetrieelemente als "keine reale Benutzer-Datenverkehr" markieren und angeben, wie dieser Datenverkehr generiert wurde. Als Beispiel durch Festlegen dieser Eigenschaft können Sie die Automatisierung von Load Test Datenverkehr Datenverkehr unterscheiden.
- Channel-Logik wurde in separate NuGet namens Microsoft.ApplicationInsights.PersistenceChannel verschoben. Standardkanal heißt jetzt InMemoryChannel
- Neue Methode ```TelemetryClient.Flush``` können synchron telemetrieelemente aus dem Puffer leeren

## <a name="version-013"></a>Version 0,13

Keine Versionsinformationen für ältere Versionen. 
