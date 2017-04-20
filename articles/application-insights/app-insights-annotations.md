<properties
    pageTitle="Kommentare für Application Insights freigeben | Microsoft Azure"
    description="Bereitstellung hinzufügen oder zu Ihrer Metriken Explorer Diagramme in Application Insights erstellen."
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="awills"/>

# <a name="release-annotations-in-application-insights"></a>Kommentare in Application Insights freigeben

Lassen Sie Anmerkungen [Metriken Explorer](app-insights-metrics-explorer.md) Diagramme anzeigen, in dem Sie einen neuen Build bereitgestellt. Sie erleichtern die überprüfen, ob Änderungen Einfluss auf die Leistung der Anwendung haben. Sie können automatisch von [Visual Studio Team Services Buildsystem](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs)erstellt werden und können Sie auch [Erstellen sie PowerShell](#create-annotations-from-powershell).

![Beispiel für Kommentare mit sichtbaren Korrelation mit server](./media/app-insights-annotations/00.png)

Release Annotations eine cloudbasierte Builds und Service von Visual Studio Team Services. 

## <a name="install-the-annotations-extension-one-time"></a>Installieren Sie die Anmerkungen Erweiterung (einmal)

Um Version Kommentare erstellen können, müssen Sie eines der vielen Team Service-Erweiterung auf dem Markt für Visual Studio installieren.

1. Mit [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) Projekt anmelden.
2. In Visual Studio Marketplace [Release Annotations-Erweiterung abrufen](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), und Ihr Team Services-Konto hinzuzufügen.

![Bei rechts Team Services-Webseite öffnen Marketplace. Wählen Sie Visual Team Services und dann unter Build und Version mehr.](./media/app-insights-annotations/10.png)

Sie müssen nur einmal für das Visual Studio Team Services-Konto. Versionsanmerkungen können nun für jedes Projekt in Ihrem Konto konfiguriert werden. 

## <a name="get-an-api-key-from-application-insights"></a>Abrufen eines API-Schlüssels von Anwendung

Sie müssen dies für jede versionsvorlage die gewünschte Version Kommentare erstellen.


1. [Microsoft Azure-Portal](https://portal.azure.com) melden Sie an und öffnen Sie Application Insights-Ressource, die die Anwendung überwacht. (Oder [einen erstellen](app-insights-overview.md), wenn dies noch nicht geschehen).
2. **API-Zugriff**öffnen, und eine Kopie der **Anwendung Einblicke Id**.

    ![Portal.azure.com öffnen Sie Application Insights-Ressource zu, und wählen Sie Einstellungen. Öffnen Sie den API-Zugriff. Die ID kopieren](./media/app-insights-annotations/20.png)

2. In einem separaten Browserfenster öffnen (oder erstellen) die Version aus, die die Bereitstellung von Visual Studio Team Services verwaltet. 

    Hinzufügen einer Aufgabe und Menü Task Anwendung Einblicke Version Anmerkung.

    Fügen Sie die **Id der Anwendung** , die von den API-Zugriff Blade kopiert.

    ![Öffnen Sie in Visual Studio Team Services Version, wählen Sie eine Definition Version und wählen Sie bearbeiten. Klicken Sie auf Task hinzufügen, und wählen Sie Anwendung Einblicke Version Anmerkung. Fügen Sie der Anwendung Insights-ID](./media/app-insights-annotations/30.png)

3. Legen Sie das **APIKey** -Feld einer Variablen `$(ApiKey)`.

4. Das Blade API-Zugriff erstellen einen neuen API-Schlüssel und eine Kopie davon.

    ![Klicken Sie im Fenster Azure Blatt API-Zugriff auf API-Schlüssel erstellen. Ein Kommentar schreiben Kommentare überprüfen und klicken Sie auf Schlüssel generieren. Kopieren Sie den neuen Schlüssel.](./media/app-insights-annotations/40.png)

4. Öffnen der Registerkarte Konfiguration die Release-Vorlage.

    Erstellen eine Funktionsdefinition für `ApiKey`.

    Fügen Sie Ihre API-Schlüssel auf ApiKey Variablendefinition.

    ![Wählen Sie im Team Services die Registerkarte Konfiguration, und klicken Sie auf Variable hinzufügen. Der Name, ApiKey und in den Wert, fügen Sie den neu erstellten Schlüssel.](./media/app-insights-annotations/50.png)


5. **Speichern Sie** schließlich Release Definition.

## <a name="create-annotations-from-powershell"></a>Anmerkung von PowerShell erstellen

Sie können auch Kommentare aus jedem Prozess erstellen (ohne VS Team System) wie. 

[Powershell-Skript von GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)zu erhalten.

Verwenden sie z. b.:

    .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }

Abrufen der `applicationId` und `apiKey` aus der Anwendung Einblicke: Einstellungen öffnen, API-Zugriff und die Anwendung-ID kopieren Klicken Sie auf API-Schlüssel erstellen und kopieren Sie den Schlüssel. 

## <a name="release-annotations"></a>Freigeben von Notizen

Nun bei der Verwendung der Release-Vorlage zum Bereitstellen einer neuen Version eine Anmerkung an Application Insights gesendet. Die Strukturänderungen werden in den Diagrammen Metrik-Explorer angezeigt.

Auf einer Anmerkung Marke Details der Version Requestor Source Control Zweig öffnen, freizugeben Sie Definition und Umgebung.


![Klicken Sie auf eine beliebige Version Anmerkung Marke.](./media/app-insights-annotations/60.png)
