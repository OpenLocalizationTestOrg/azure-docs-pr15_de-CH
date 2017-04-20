<properties
    pageTitle="Real-Time-Werte in Azure CDN | Microsoft Azure"
    description="Echtzeit-Statistiken bietet Echtzeitdaten über die Leistung von Azure CDN bei Ihren Kunden liefern."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="real-time-stats-in-microsoft-azure-cdn"></a>Echtzeit-Statistiken in Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Übersicht

Dieses Dokument erläutert in Echtzeit Statistiken in Microsoft Azure CDN.  Diese Funktion bietet Echtzeitdaten, Bandbreite, Cache-Status und gleichzeitige Verbindungen zu Ihrem Profil CDN bei Ihren Kunden liefern. Dies ermöglicht kontinuierliche Überwachung des Zustands des Service jederzeit, einschließlich Go-live-Events.

Die folgenden Diagramme sind verfügbar:

* [Bandbreite](#bandwidth)
* [Statuscodes](#status-codes)
* [Cache-Status](#cache-statuses)
* [Anschlüsse](#connections)


## <a name="accessing-real-time-stats"></a>Zugriff auf Statistiken in Echtzeit

1. Suchen Sie in [Azure-Portal](https://portal.azure.com)Ihres Profils CDN.

    ![CDN Profil blade](./media/cdn-real-time-stats/cdn-profile-blade.png)

2. Blatt Profil CDN klicken Sie auf **Verwalten** .

    ![CDN Profil Blade Schaltfläche verwalten](./media/cdn-real-time-stats/cdn-manage-btn.png)

    Das CDN-Verwaltungsportal wird geöffnet.

3. Hovern Sie über die Registerkarte **Analytics** und Maus über Flyout **In Echtzeit Statistiken** .  Klicken Sie auf **HTTP Large Object**.

    ![CDN-Verwaltungsportal](./media/cdn-real-time-stats/cdn-premium-portal.png)

    Diagramme in Echtzeit Statistiken werden angezeigt.
    
Die Diagramme werden in Echtzeit Statistiken für den ausgewählten Zeitraum beginnend beim Laden der Seite angezeigt.  Die Diagramme werden alle paar Sekunden automatisch aktualisiert.  Die Schaltfläche **Diagramm aktualisieren** löscht ggf. das Diagramm nach dem nur die ausgewählten Daten angezeigt werden.

## <a name="bandwidth"></a>Bandbreite

![Bandbreite: Grafik](./media/cdn-real-time-stats/cdn-bandwidth.png)

**Bandbreite** Graph zeigt Bandbreite über ausgewählte Zeitspanne für die aktuelle Plattform verwendet. Der schattierte Bereich des Diagramms zeigt Bandbreite an. Die Höhe der aktuell verwendete Bandbreite wird direkt unter das Liniendiagramm angezeigt.

## <a name="status-codes"></a>Statuscodes

![Codediagramm Status](./media/cdn-real-time-stats/cdn-status-codes.png)

**Statuscodes** Diagramm gibt an, wie oft bestimmte HTTP-Antwortcodes in ausgewählten Zeitraum auftreten.

> [AZURE.TIP]  Eine Beschreibung der einzelnen Optionen HTTP Status Codes finden Sie unter [Azure CDN HTTP-Statuscodes](https://msdn.microsoft.com/library/mt759238.aspx).

Eine Liste der HTTP-Statuscodes wird direkt über dem Diagramm angezeigt. Diese Liste gibt jeden Statuscode in das Liniendiagramm und die aktuelle Anzahl der Ereignisse pro Sekunde, Statuscode enthalten sein kann. Standardmäßig wird eine Zeile für jeden dieser Statuscodes im Diagramm angezeigt. Sie können nur die Statuscodes überwachen, besonderen Bedeutung für die CDN-Konfiguration. Dazu überprüfen Sie die gewünschten Statuscodes und deaktivieren Sie alle Optionen und klicken Sie auf **Diagramm aktualisieren**. 

Protokollierte Daten für einen bestimmten Statuscode können vorübergehend ausblenden.  Klicken Sie in der Legende unterhalb des Graphen Statuscode ausblenden möchten. Der Statuscode wird aus dem Diagramm sofort ausgeblendet. Dieser Statuscode erneutes bewirkt diese Option erneut angezeigt werden.

## <a name="cache-statuses"></a>Cache-Status

![Cache-Status-Diagramm](./media/cdn-real-time-stats/cdn-cache-status.png)

**Cache-Status** -Diagramm gibt an, wie oft bestimmte Cache-Status in den ausgewählten Zeitraum auftreten. 

> [AZURE.TIP]  Eine Beschreibung der einzelnen Status-Zwischenspeicherungsoption finden Sie unter [Azure CDN Cache-Statuscodes](https://msdn.microsoft.com/library/mt759237.aspx).

Eine Liste der Statuscodes Cache wird direkt über dem Diagramm angezeigt. Diese Liste gibt jeden Statuscode in das Liniendiagramm und die aktuelle Anzahl der Ereignisse pro Sekunde, Statuscode enthalten sein kann. Standardmäßig wird eine Zeile für jeden dieser Statuscodes im Diagramm angezeigt. Sie können nur die Statuscodes überwachen, besonderen Bedeutung für die CDN-Konfiguration. Dazu überprüfen Sie die gewünschten Statuscodes und deaktivieren Sie alle Optionen und klicken Sie auf **Diagramm aktualisieren**. 

Protokollierte Daten für einen bestimmten Statuscode können vorübergehend ausblenden.  Klicken Sie in der Legende unterhalb des Graphen Statuscode ausblenden möchten. Der Statuscode wird aus dem Diagramm sofort ausgeblendet. Dieser Statuscode erneutes bewirkt diese Option erneut angezeigt werden.

## <a name="connections"></a>Anschlüsse

![Connections-Diagramm](./media/cdn-real-time-stats/cdn-connections.png)

Dieses Diagramm zeigt an, wie viele Verbindungen mit Ihrer Edge-Server hergestellt wurde. Jede Anforderung für eine Anlage, die durch unsere CDN zu einer Verbindung verläuft.

## <a name="next-steps"></a>Nächste Schritte

- Benachrichtigt mit [Echtzeit-Alarme in Azure CDN](cdn-real-time-alerts.md)
- Tiefer [Erweiterte HTTP-](cdn-advanced-http-reports.md) Berichte
- Analysieren [von Verwendungsmustern](cdn-analyze-usage-patterns.md)

