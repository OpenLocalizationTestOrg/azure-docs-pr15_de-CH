<properties
    pageTitle="Hinzufügen einer Verzögerung in Logik-apps | Microsoft Azure"
    description="Übersicht über die Verzögerung und Verzögerung-bis Aktionen in Verbindung mit einer Azure Logik app."
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

# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Erste Schritte mit Verzögerung Verzögerung-bis-Aktionen

Mithilfe der Verzögerung und "Delay-bis" Aktionen Workflowszenarios abzuschließen.

Beispielsweise können Sie:

- Warten Sie auf einen Wochentag Statusinformationen per e-Mail senden.
- Verzögern des Workflows bis ein HTTP-Aufruf Zeit bevor fortsetzen und Abrufen des Resultsets.

Zunächst mithilfe der Verzögerungsaktion in einer Anwendung Logik finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-delay-actions"></a>Verwenden Sie die Verzögerung Aktionen

Eine Aktion ist eine Operation, die vom Workflow ausgeführt wird, die in einer Anwendung Logik definiert. [Weitere Informationen zu Aktionen](connectors-overview.md).

Hier ist eine Sequenz wird wie einen verzögerungsschritt in eine Logik-app:

1. Nach dem Hinzufügen eines Triggers, klicken Sie auf **Schritt** , um eine Aktion hinzuzufügen.
2. **Verzögerung** Verzögerung Maßnahmen zu suchen. In diesem Beispiel wählen wir die **Verzögerung**.

    ![Verzögerung Aktionen](./media/connectors-native-delay/using-action-1.png)

3. Führen Sie die Eigenschaften zum Konfigurieren der Verzögerung.

    ![Verzögerung config](./media/connectors-native-delay/using-action-2.png)

4. Klicken Sie auf **Speichern** , um veröffentlichen und Aktivieren der Anwendung Logik.


## <a name="action-details"></a>Aktionsdetails

Der Trigger Serie hat die folgenden Eigenschaften, die konfiguriert werden können.

### <a name="delay-action"></a>Verzögerung

Dadurch verzögert die Ausführung für einen bestimmten Zeitraum.
Ein * ein Pflichtfeld ist.

|Angezeigter name|Eigenschaftenname|Beschreibung|
|---|---|---|
|Count *|Anzahl|Die Anzahl der Zeiteinheiten verzögert|
|Einheit *|Einheit|Zeiteinheit: `Second`, `Minute`, `Hour`, oder`Day`|
<br>

### <a name="delay-until-action"></a>Delay-erst Vorgang

Dadurch wird die Ausführung bis zu einem angegebenen Zeitpunkt verzögert.
Ein * ein Pflichtfeld ist.

|Angezeigter name|Eigenschaftenname|Beschreibung|
|---|---|---|
|Jahr *|Zeitstempel|Das Jahr (GMT) verschieben|
|Monat *|Zeitstempel|Monat (GMT) verschieben|
|Tag *|Zeitstempel|Tag verschieben (GMT)|
<br>


## <a name="next-steps"></a>Nächste Schritte

Probieren Sie die Plattform und [erstellen eine Logik](../app-service-logic/app-service-logic-create-a-logic-app.md). Andere verfügbare Anschlüsse Logik Apps erkunden unserer [APIs Liste](apis-list.md)ansehen.
