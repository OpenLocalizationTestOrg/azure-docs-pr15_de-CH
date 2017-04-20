<properties
    pageTitle="Die Aktion Abfrage Logik Apps hinzufügen | Microsoft Azure"
    description="Übersicht der Abfragevorgang für Aktionen wie filtern."
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
   ms.date="07/20/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-query-action"></a>Erste Schritte mit der Abfrage

Mithilfe der Query-Aktion können Sie Stapel mit Arrays Workflows zu arbeiten:

- Erstellen einer Aufgabe für alle wichtigen Datensätze aus einer Datenbank.
- Speichern Sie alle PDF-Anlagen bei e-Mails in Azure Blob.

Zunächst mithilfe der Aktion Abfrage in einer Anwendung Logik finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-query-action"></a>Verwenden Sie die Aktion Abfrage

Eine Aktion ist eine Operation, die vom Workflow ausgeführt wird, die in einer Anwendung Logik definiert. [Weitere Informationen zu Aktionen](connectors-overview.md).  

Der Abfragevorgang wurde gerade einmal aufgerufen, die im Designer verfügbar Filter-Arrays. So können Sie ein Array Abfragen und gefilterte Ergebnisse zurückgeben.

Hier wird, wie Sie es in einer Anwendung Logik hinzufügen können:

1. Klicken Sie auf **Neuen Schritt** .
2. Wählen Sie **eine Aktion hinzufügen**.
3. Geben Sie im Suchfeld Aktion **Filter** **Filtern** Aktion aufgeführt.

    ![Wählen Sie die Aktion Abfrage](./media/connectors-native-query/using-action-1.png)

4. Wählen Sie ein Array filtern. (Der folgende Screenshot zeigt das Array der Ergebnisse einer Suche Twitter.)
5. Erstellen Sie eine Bedingung für jedes Element ausgewertet. (Der folgende Screenshot Filter Tweets von Benutzern 100 Anhänger.)

    ![Die Aktion Abfrage abschließen](./media/connectors-native-query/using-action-2.png)

    Die Aktion wird ein neues Array output mit nur Ergebnisse, die die Filter erfüllt.
6. Klicken Sie auf die linke obere Ecke der Symbolleiste speichern und Ihre Logik app sowohl speichern und veröffentlichen (aktivieren).

## <a name="query-action"></a>Die Aktion Abfrage ausführen

Hier werden die Details für die Aktion, die diesen Connector unterstützt. Der Connector wurde eine mögliche Aktion.

|Aktion|Beschreibung|
|---|---|
|Filter-array|Wertet eine Bedingung für jedes Element in einem Array und gibt die Ergebnisse zurück|

## <a name="action-details"></a>Aktionsdetails

Die Aktion Abfrage verfügt über eine mögliche Aktion. Die folgenden Tabellen beschreiben die erforderlichen und optionalen Eingabefelder für die Aktion und die entsprechende Ausgabedetails mit der Aktion verbunden.

### <a name="filter-array"></a>Filter-array
Im folgenden werden Felder für die Aktion, wodurch eine ausgehende HTTP-Anforderung.
Ein * ein Pflichtfeld ist.

|Angezeigter name|Eigenschaftenname|Beschreibung|
|---|---|---|
|Von *|Von|Das Array filtern|
|Bedingung *|wo|Für jedes Element auszuwertende Bedingung|
<br>

### <a name="output-details"></a>Ausgabedetails

Es folgen Ausgabedetails für die HTTP-Antwort.

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Gefiltertes array|Array|Ein Array, das ein Objekt für jedes gefilterte Ergebnis enthält|

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie die Plattform und [erstellen eine Logik](../app-service-logic/app-service-logic-create-a-logic-app.md). Andere verfügbare Anschlüsse Logik Apps erkunden unserer [APIs Liste](apis-list.md)ansehen.
