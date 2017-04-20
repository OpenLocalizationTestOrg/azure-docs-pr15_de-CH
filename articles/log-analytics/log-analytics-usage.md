<properties
    pageTitle="Analysieren der Datenverwendung in Protokollanalyse | Microsoft Azure"
    description="Sie können die Verwendungsseite in Protokollanalyse anzeigen, wie viele Daten an den OMS-Dienst gesendet wird."
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
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="analyze-data-usage-in-log-analytics"></a>Analysieren der Datenverwendung in Protokollanalyse

Protokollanalyse in Operations Management Suite (OMS) sammelt und in regelmäßigen Abständen an den OMS-Dienst sendet.  **Die Verwendungsseite** können Sie anzeigen, wie viele Daten an den OMS-Dienst gesendet wird. **Die Verwendungsseite** zeigt Ihnen auch, wie viele Daten täglich von Projektmappen gesendet wird und wie oft der Server Daten senden.

>[AZURE.NOTE] Wenn Sie ein kostenloses Konto mit [OMS-Website](http://www.microsoft.com/oms)erstellt haben, können Sie auf 500 MB an Daten täglich an den OMS-Dienst senden. Das Tageslimit erreicht, wird die Datenanalyse anhalten und fortsetzen am Anfang des nächsten Tages. Sie müssen auch Daten senden, die nicht zugelassen oder vom OMS verarbeitet.

**Übersicht** -Dashboard in OMS Kachel **Verwendung** mit können Sie die Auslastung anzeigen.

![Verwendung-Kachel](./media/log-analytics-usage/usage-tile.png)

Wenn Sie die tägliche Verwendung überschritten haben oder der Grenzwert sind, können Sie optional eine Lösung, um die Datenmenge zu reduzieren, die an den OMS-Dienst entfernen. Weitere Informationen zum Entfernen von Projektmappen finden Sie unter [Lösungen Lösungskatalog Protokollanalyse hinzufügen](log-analytics-add-solutions.md).

![Dashboard für die Nutzung](./media/log-analytics-usage/usage-dashboard.png)

**Die Verwendungsseite** zeigt die folgenden Informationen:

- Durchschnittliche Verwendung pro Tag
- Verwendung von Daten für jede Lösung innerhalb der letzten 30 Tage
- Wie viel sind die Server in Ihrer Umgebung OMS-Dienst in den letzten 30 Tagen Senden von Daten
- Ihre Daten Tarif planen und Kosten
- Informationen zu der Vereinbarung zum Servicelevel (SLA) einschließlich OMS Ihre Daten Dauer

## <a name="to-work-with-usage-data"></a>Arbeiten mit Daten

1. Klicken Sie auf der Seite **Übersicht** **Verwendung** .
2. Zeigen Sie auf **der Verwendungsseite** Nutzungskategorien an, die Bereiche zeigen, dass Sie besorgt
3. Haben Sie eine Lösung, die zu viele Ihrer täglichen Upload Quote in Anspruch nimmt, sollten Sie die Lösung entfernen.

## <a name="to-view-your-estimated-cost-and-billing-information"></a>Die geschätzten Kosten und Abrechnungsinformationen
1. Klicken Sie auf der Seite **Übersicht** **Verwendung** .
2. Die **Verwendung** **Verwendung**klicken Sie auf das Chevron (**>**) neben **geschätzte Kosten**.
3. In den erweiterten Details **Ihres** sehen Sie Ihre geschätzte monatliche Kosten.  
    ![Datenvolumen](./media/log-analytics-usage/usage-data-plan.png)
4. Wenn Sie Ihre Abrechnungsinformationen anzeigen möchten, klicken Sie auf **Ansicht Rechnung** um Ihre Abonnementinformationen anzuzeigen.
    - Klicken Sie auf der Abonnementseite auf Ihr Abonnement, um Details und einzelpostenliste Verwendung anzeigen.  
        ![Abonnement](./media/log-analytics-usage/usage-sub01.png)
    - Auf der Seite Zusammenfassung für Ihr Abonnement können Sie eine Vielzahl von Aufgaben verwalten und weitere Einzelheiten zu Ihrem Abonnement ausführen.  
        ![Angaben zum Abonnement](./media/log-analytics-usage/usage-sub02.png)

## <a name="to-view-data-batches-for-your-sla"></a>Stapel für SLA Daten anzeigen
1. Klicken Sie auf der Seite **Übersicht** **Verwendung** .
2. Klicken Sie unter **Service Level Agreement** **SLA herunterladen Details**.
3. Sie überprüfen wird Excel XLSX-Datei heruntergeladen.  
    ![SLA-details](./media/log-analytics-usage/usage-sla-details.png)

## <a name="next-steps"></a>Nächste Schritte

- Siehe [Protokoll durchsucht Protokollanalyse](log-analytics-log-searches.md) Solutions ausführlichen Informationen anzeigen.
