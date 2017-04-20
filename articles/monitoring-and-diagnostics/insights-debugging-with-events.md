<properties
    pageTitle="Anzeigen von Ereignissen und Überwachungsprotokolle"
    description="Informationen Sie zum Anzeigen aller Ereignisse, die in Azure-Abonnement auftreten."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/28/2015"
    ms.author="robb"/>

# <a name="view-events-and-audit-logs"></a>Anzeigen von Ereignissen und Überwachungsprotokolle

Alle Operationen auf Azure-Ressourcen überwacht werden vollständig von Azure Ressourcenmanager und Löschvorgänge zu erteilen oder widerrufen des Zugriffs. Diese Protokolle in Azure-Portal durchsuchen und Sie können auch die [REST-API](https://msdn.microsoft.com/library/azure/dn931927.aspx) oder [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) programmgesteuert auf den vollständigen Satz von Ereignissen.

## <a name="browse-the-events-impacting-your-azure-subscription"></a>Beeinträchtigung der Azure-Abonnement Ereignissen suchen

1. [Azure-Portal](https://portal.azure.com/)anmelden.
2. Klicken Sie auf **Durchsuchen** , und wählen Sie **Überwachungsprotokolle**.  
    ![Hub durchsuchen](./media/insights-debugging-with-events/Insights_Browse.png)
3. Dies öffnet ein Blatt mit allen Ereignissen, die die Abonnements für die letzten 7 Tage beeinträchtigt werden. Oben ist ein Diagramm mit Daten nach Ebene und darunter die vollständige Protokolle:  ![alle Ereignisse](./media/insights-debugging-with-events/Insights_AllEvents.png)

>[AZURE.NOTE] Sie können nur 500 jüngsten Ereignisse für das angegebene Abonnement im Azure-Portal anzeigen.

4. Klicken Sie auf einen beliebigen Protokolleintrag, um die Ereignisse anzuzeigen, die sie aus. Z. B. Wenn Sie eine Ressourcengruppe bereitstellen, können viele verschiedene Ressourcen erstellt oder geändert werden. Für jeden Eintrag angezeigt:
    * Der **Level** - könnte es zum Beispiel, nur nachverfolgen (**Information**) oder wenn etwas schief geht, müssen Sie wissen (**Fehler**).
    * **Status** - der letzte Status werden im Allgemeinen, **erfolgreich** oder **fehlgeschlagen**kann, es jedoch auch **akzeptiert** für zeitintensive Vorgänge.
    * *Wenn* das Ereignis aufgetreten ist.
    * *Die* den Vorgang ausgeführt, wenn jemand. Nicht alle Vorgänge werden von Benutzern, einige von Back-End-Services durchgeführt werden, damit keine **Aufrufer**werden.
    * **Korrelations-ID** des Ereignisses - Dies ist der eindeutige Bezeichner für diesen Satz von Operationen.

5. Dort können Sie Details Blade die Einzelheiten des Ereignisses zu wechseln.

    ![Ressourcengruppen](./media/insights-debugging-with-events/Insights_EventDetails.png)

    **Fehler** -Ereignisse enthält diese Seite normalerweise **Unterstatus** und keinen Abschnitt für **Eigenschaften** , die nützliche Informationen für das Debuggen enthalten.

## <a name="filter-to-specific-logs"></a>Filter für bestimmte Protokolle

Um Ereignisse anzuzeigen, die auf eine bestimmte Entität oder eines bestimmten Typs anwenden, können Sie das Blade Audit-Protokolle auf **Filter** filtern. Sie verwenden auch Blade Filter ändern der **Zeitspanne** des Blades Audit-Protokolle.

Sobald Sie auf diesen Befehl klicken, wird ein neues Blatt geöffnet:

![Filter](./media/insights-debugging-with-events/Insights_EventFilter.png)

Es gibt vier Arten von Filtern:

1. Abonnement
2. Eine **Ressourcengruppe**
3. Ein **Ressourcentyp**
4. Eine bestimmte **Ressource** - müssen für diese Sie über vollständige *Ressourcen-ID* der Ressource, der Sie interessieren

Darüber hinaus können Sie auch Filtern Ereignisse durch, die das Ereignis ausgeführt oder durch die Ebene des Ereignisses.

Einmal haben Sie möchten zu sehen, klicken Sie am unteren Rand das Blade **Update** auswählen.

## <a name="monitor-events-impacting-specific-resources"></a>Überwachen von Ereignissen Beeinträchtigung der Ressourcen

1. Klicken Sie auf **wechseln** zu der Ressource, die, der Sie interessieren. Sie können auch alle Protokolle für eine **Ressourcengruppe**anzeigen
2. Scrollen Sie auf die Ressource, bis die Kachel **Ereignisse** finden.  
    ![Ereignisse nebeneinander](./media/insights-debugging-with-events/Insights_EventsTile.png)
3. Klicken Sie auf die Kachel Ereignisse gefiltert, dass nur die ausgewählte Ressource. **Filter** -Befehl können Sie den Zeitraum ändern oder weitere Filter anwenden.

## <a name="next-steps"></a>Nächste Schritte

* [Benachrichtigung erhalten](insights-receive-alert-notifications.md) , wenn ein Ereignis eintritt.
* [Dienstmetrik überwachen](insights-how-to-customize-monitoring.md) um sicherzustellen, dass der Dienst verfügbar ist und reagiert wird.
* [Track-Dienststatus](insights-service-health.md) wann Azure Leistung Abbau- oder Service-Unterbrechung aufgetreten ist.  
