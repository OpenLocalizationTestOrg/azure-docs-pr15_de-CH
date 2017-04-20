<properties
    pageTitle="Erstellen Sie ein benutzerdefiniertes Dashboard in Protokollanalyse | Microsoft Azure"
    description="Dieses Handbuch hilft Ihnen zu verstehen, wie Protokolldateien Analytics Dashboards alle Ihre Suchanfragen gespeichertes Protokoll visualisieren können mit einem objektiv an Ihre Umgebung."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="create-a-custom-dashboard-in-log-analytics"></a>Erstellen Sie ein benutzerdefiniertes Dashboard in Protokollanalyse

Dieses Handbuch hilft Ihnen zu verstehen, wie Protokollanalyse Dashboards alle Ihre Suchanfragen gespeichertes Protokoll visualisieren können mit einem objektiv Ihrer Umgebung anzeigen.

![Beispiel-Dashboard](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

Die benutzerdefinierte Dashboards in OMS-Portal erstellen stehen in OMS-Mobile-Anwendung. Finden Sie unter den folgenden Seiten Weitere Informationen über die Datei apps.

- [OMS mobile Anwendung Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
- [OMS-mobile-app von Apple iTunes](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![Mobile Dashboards](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Erstellen mein dashboard

Gehen Sie zunächst die OMS-Übersichtsseite. Sie sehen die Kachel **Mein Dashboard** auf der linken Seite. Klicken Sie auf das Dashboard Drilldown.

![Übersicht](./media/log-analytics-dashboards/oms-dashboards-overview.png)


## <a name="adding-a-tile"></a>Hinzufügen einer Kachel

Dashboards sind gespeicherte Protokoll Suchvorgängen Kacheln angetrieben. OMS enthält viele vorgefertigte gespeichertes Protokoll suchen, so können Sie sofort beginnen. Gehen Sie folgendermaßen vor, die beschreiben, wie Sie beginnen.

In der Mein Dashboard klicken **Anpassen** eingeben Modus anpassen.

![Bildliche](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Der Bereich auf der rechten Seite der Seite geöffnet enthält den Arbeitsbereich gespeicherte Protokoll suchen. Um ein gespeichertes Protokoll suchen als Kachel veranschaulichen, Mauszeiger eine gespeicherte Suche, und klicken Sie auf das **plus** -Symbol.

![Kacheln 1 hinzufügen](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Wenn Sie auf das **plus** -Symbol klicken, wird eine neue Tile an Mein Dashboard angezeigt.

![Kacheln 2 hinzufügen](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)


## <a name="edit-a-tile"></a>Bearbeiten einer Kachel

In der Mein Dashboard klicken **Anpassen** eingeben Modus anpassen. Klicken Sie bearbeiten möchten. Rechten Bereich ändert sich zu bearbeiten, und bietet eine Auswahl an:

![Kachel bearbeiten](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Kachel bearbeiten](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Kachel-Visualisierung#
Es gibt drei Arten von Kachel Visualisierung auswählen:

|Diagrammtyp|Funktionsweise|
|---|---|
|![Balkendiagramm](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png)|Zeigt eine Zeitskala Ihr gespeichertes Protokoll Suchergebnisse ein Balkendiagramm oder eine Liste der Ergebnisse anhand eines Felds je nach, wenn Ihr Protokoll suchen Ergebnisse anhand eines Felds aggregiert.
|![Metrik](./media/log-analytics-dashboards/oms-dashboards-metric.png)|Zeigt Treffer insgesamt Protokoll Ergebnis als Zahl in einer. Metrische Kacheln können Sie einen Schwellenwert festzulegen, der die Kachel hervorgehoben wird, wenn der Schwellenwert erreicht wird.|
|![Zeile](./media/log-analytics-dashboards/oms-dashboards-line.png)|Zeigt eine Zeitskala der gespeicherte Protokoll Ergebnis Treffer mit Werten als Liniendiagramm.|

### <a name="threshold"></a>Schwellenwert
Sie können einen Schwellenwert auf einer Kachel mit metrischen Visualisierung erstellen. Wählen Sie einen Schwellenwert auf der Kachel zu erstellen. Wählen Sie aus, ob die Kachel hervorzuheben, wenn der Wert über oder unter dem ausgewählten Schwellenwert dann unten Schwellenwert.

## <a name="organizing-the-dashboard"></a>Dashboard verwalten
Organisieren Ihr Dashboard, Mein Dashboard-Ansicht wechseln und auf Anpassen **Anpassen** , geben Modus. Klicken Sie und ziehen Sie die Kachel zu verschieben und an die gewünschte Stelle der Kachel zu verschieben.

![Organisieren Sie Ihr Dashboard](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Entfernen einer Kachel
Um eine Kachel, Mein Dashboard-Ansicht wechseln und auf Anpassen **Anpassen** , geben Modus. Wählen Sie das Musterelement, das Sie entfernen möchten und dann auf das rechte Fenster **Nebeneinander entfernen**.

![Entfernen einer Kachel](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie [Alarme](log-analytics-alerts.md) in Protokollanalyse Benachrichtigung generiert und Probleme zu beheben.
