<properties
    pageTitle="Serie Trigger Logik Apps hinzufügen | Microsoft Azure"
    description="Übersicht über Trigger Serie und zum Azure Logik App verwenden."
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-recurrence-trigger"></a>Erste Schritte mit der Serie trigger

Erstellen Sie mithilfe des Serie Triggers leistungsstarke Workflows in der Cloud.

Beispielsweise können Sie:

- Planen Sie einen Workflow eine gespeicherten SQL-Prozedur täglich ausgeführt.
- Eine Zusammenfassung aller TWEETS innerhalb der letzten Woche zu einer bestimmten Hashtag eine e-Mail.

Zum Verwenden des Serie Triggers in einer Anwendung Logik, finden Sie unter [Erstellen einer Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Einen Serie Trigger verwenden

Ein Trigger ist ein Ereignis, das zum Starten des Workflows, die in einer Anwendung Logik definiert verwendet werden kann. [Weitere Informationen zu Triggern](connectors-overview.md).

Hier ist eine Beispiel-Sequenz Serie Trigger in einer Logik-app einrichten:

1. **Serie** Trigger als ersten Schritt in einer Anwendung Logik hinzufügen.
2. Geben Sie die Parameter für das Wiederholungsintervall.

Die Logik-app startet jetzt ausführen nach jeder Zeit.

![HTTP-trigger](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Triggerdetails

Der Trigger Serie hat die folgenden Eigenschaften, die Sie konfigurieren können.

Löst eine Logik-app nach einer festgelegten Zeitspanne.
Ein * ein Pflichtfeld ist.

|Angezeigter name|Eigenschaftenname|Beschreibung|
|---|---|---|
|Häufigkeit *|Häufigkeit|The unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.|
|Intervall *|Intervall|Das Intervall für die Wiederholung der angegebenen Frequenz.|
|Zeitzone|Zeitzone|Wenn eine Startzeit ohne einen UTC-Offset angegeben ist, wird diese Zone verwendet werden.|
|Startzeit|Startzeit|Die Startzeit im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).|
<br>


## <a name="next-steps"></a>Nächste Schritte

Probieren Sie die Plattform und [erstellen eine Logik](../app-service-logic/app-service-logic-create-a-logic-app.md). Andere verfügbare Anschlüsse Logik Apps erkunden unserer [APIs Liste](apis-list.md)ansehen.
