<properties 
    pageTitle="Exemplarische Vorgehensweise: Überwachen von Microsoft Dynamics CRM-Anwendung Einblicke" 
    description="Erhalten Sie von Microsoft Dynamics CRM Online-Anwendung Einblicke mit Telemetrie. Exemplarische Vorgehensweise zur Installation Abrufen von Daten, Visualisierung und Export." 
    services="application-insights" 
    documentationCenter=""
    authors="mazharmicrosoft" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="awills"/>
 
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Exemplarische Vorgehensweise: Aktivieren der Telemetrie für Microsoft Dynamics CRM Online-Anwendung Einblicke mit

Dieser Artikel beschreibt, wie Daten aus [Microsoft Dynamics CRM Online](https://www.dynamics.com/) zu mit [Visual Studio Application Insights](https://azure.microsoft.com/services/application-insights/). Wir gehen erfassen Daten und Visualisierung von Daten durch den Vorgang der Anwendung Application Insights-Skript hinzufügen.

>[AZURE.NOTE] [Die Beispielprojektmappe durchsuchen](https://dynamicsandappinsights.codeplex.com/).

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Neue oder vorhandene CRM Online-Instanz Application Insights hinzufügen 

Überwachen die Anwendung wird die Anwendung Application Insights-SDK hinzufügen. Das SDK sendet Telemetrie an [Application Insights-Portal](https://portal.azure.com)können unsere leistungsstarken Analyse und Diagnose-Tools, oder exportieren Sie die Daten in den Speicher.

### <a name="create-an-application-insights-resource-in-azure"></a>Erstellen Sie eine Ressource Anwendung Einblicke in Azure

1. [Ein Konto in Microsoft Azure](http://azure.com/pricing)zu erhalten. 
2. [Azure-Portal](https://portal.azure.com) anmelden und eine neue Anwendung Einblicke Ressource hinzufügen. Dies ist, wo die Daten verarbeitet und angezeigt werden.

    ![Klicken Sie auf +, Developer Services Application Insights.](./media/app-insights-sample-mscrm/01.png)

    Wählen Sie den Anwendungstyp ASP.NET.

3. Öffnen Sie die Registerkarte Schnellstart und das Skript Code.

    ![](./media/app-insights-sample-mscrm/03.png)

**Die Codepage geöffnet lassen** zwar die nächste Schritt in einem anderen Browserfenster. Sie benötigen den Code schnell. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Erstellen Sie eine Webressource JavaScript in Microsoft Dynamics CRM

1. Öffnen Sie Ihr CRM Online-Instanz und Anmeldung mit Administratorrechten an.
2. Öffnen von Microsoft Dynamics CRM Einstellungen, Anpassungen des Systems

    ![](./media/app-insights-sample-mscrm/04.png)
    
    ![](./media/app-insights-sample-mscrm/05.png)


    ![](./media/app-insights-sample-mscrm/06.png)

3. Erstellen Sie eine JavaScript-Ressource.

    ![](./media/app-insights-sample-mscrm/07.png)

    Geben sie einen Namen und wählen Sie **Skript (JScript)** öffnen Sie Text-Editor.

    ![](./media/app-insights-sample-mscrm/08.png)
    
4. Kopieren Sie den Code von Anwendung. Beim Kopieren unbedingt Script-Tags ignoriert. Siehe Screenshot unten:

    ![](./media/app-insights-sample-mscrm/09.png)

    Der Code enthält instrumentationsschlüssel, der die Anwendungsressource Einblicke identifiziert.

5. Speichern und veröffentlichen.

    ![](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Instrument-Formulare

1. Öffnen Sie das Formular in Microsoft CRM Online

    ![](./media/app-insights-sample-mscrm/11.png)

2. Öffnen Sie das Formular Eigenschaften

    ![](./media/app-insights-sample-mscrm/12.png)

3. Fügen Sie JavaScript-Webressource, die Sie erstellt haben

    ![](./media/app-insights-sample-mscrm/13.png)

    ![](./media/app-insights-sample-mscrm/14.png)

4. Speichern Sie und veröffentlichen Sie der Form angepasst.


## <a name="metrics-captured"></a>Metriken erfasst

Sie haben jetzt Telemetrie Erfassung für das Formular eingerichtet. Wenn es verwendet wird, werden Daten für die Anwendung Einblicke Ressource gesendet.

Hier sind Beispiele für Daten, die Sie sehen.

#### <a name="application-health"></a>Anwendungszustands

![](./media/app-insights-sample-mscrm/15.png)

![](./media/app-insights-sample-mscrm/16.png)

Browser-Ausnahmen:

![](./media/app-insights-sample-mscrm/17.png)

Klicken Sie auf das Diagramm, um mehr Details zu sehen:

![](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Verwendung

![](./media/app-insights-sample-mscrm/19.png)

![](./media/app-insights-sample-mscrm/20.png)

![](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Browser

![](./media/app-insights-sample-mscrm/22.png)

![](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>GeoLocation

![](./media/app-insights-sample-mscrm/24.png)

![](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Anforderung innen anzeigen

![](./media/app-insights-sample-mscrm/26.png)

![](./media/app-insights-sample-mscrm/27.png)

![](./media/app-insights-sample-mscrm/28.png)

![](./media/app-insights-sample-mscrm/29.png)

![](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Beispielcode

[Den Beispielcode durchsuchen](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI

Sie können tiefere Analyse, wenn [die Daten nach Microsoft Power BI exportieren](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Microsoft Dynamics CRM-Beispielprojektmappe

[Hier ist der Beispielprojektmappe in Microsoft Dynamics CRM implementiert] (https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Weitere Informationen

* [Was ist Application Insights?](app-insights-overview.md)
* [Anwendung Einblicke für Webseiten](app-insights-javascript.md)
* [Beispiele und exemplarische Vorgehensweisen](app-insights-code-samples.md)

 
