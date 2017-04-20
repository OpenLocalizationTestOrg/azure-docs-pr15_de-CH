<properties
    pageTitle="Logik-Apps Dynamics CRM Online Connector hinzufügen | Microsoft Azure"
    description="Erstellen von Logik apps mit Azure App. Dynamics CRM Online-Verbindungsanbieter stellt eine API zu mit Dynamics CRM Online."
    services="logic-apps"    
    documentationCenter=""     
    authors="MandiOhlinger"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/15/2016"
ms.author="mandia"/>

# <a name="get-started-with-the-dynamics-crm-online-connector"></a>Erste Schritte mit Dynamics CRM Online-connector
Verbinden von Dynamics CRM Online, erstellen Sie einen neuen Datensatz ein Element und mehr aktualisieren. Mit CRM Online können Sie:

- Erstellen Sie Ihr Geschäftsablauf anhand der Daten aus CRM Online erhalten. 
- Entitäten und mehr bekommen Sie Aktionen verwenden, die einen Datensatz zu löschen. Diese Aktionen eine Antwort erhalten und dann die Ausgabe für andere Aktionen verfügbar machen. Wenn ein Element in CRM aktualisiert wird, können Sie Office 365 per e-Mail senden.

In diesem Thema veranschaulicht die Verwendung von Dynamics CRM Online Connector Logik App und listet auch Auslöser und Aktionen.

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik Apps allgemein verfügbar (GA).

Weitere Logik Apps sehen Sie [Was Logik apps](../app-service-logic/app-service-logic-what-are-logic-apps.md) und [Logik app erstellen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-dynamics-crm-online"></a>Verbindung zu Dynamics CRM Online

Bevor Ihre Anwendung Logik einen Dienst zugreifen kann, erstellen Sie zuerst eine *Verbindung* zum Dienst. Eine Verbindung stellt eine Verbindung zwischen einer Anwendung Logik und ein weiterer. Beispielsweise zunächst Verbindung zu Dynamics Sie Dynamics CRM Online- *Verbindung*. Geben Sie zum Erstellen einer Verbindung die Anmeldeinformationen verwenden Sie normalerweise den Zugriff auf Dienste, die Sie herstellen möchten. Geben Sie die Anmeldeinformationen so mit Dynamics, die Dynamics CRM Online-Konto zum Herstellen die Verbindung.


### <a name="create-the-connection"></a>Erstellen der Verbindung

>[AZURE.INCLUDE [Steps to create a connection to Dynamics CRM Online Connection Provider](../../includes/connectors-create-api-crmonline.md)]

## <a name="use-a-trigger"></a>Verwenden eines Triggers

Ein Trigger ist ein Ereignis, das zum Starten des Workflows definiert eine Logik-App verwendet werden kann. Trigger Abfragen"" den Service, Intervall und Häufigkeit. [Weitere Informationen zu Triggern](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Geben Sie der Anwendung Logik "Dynamik", eine Liste der Trigger:  

    ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)

2. Wählen Sie **Dynamics CRM Online - Wenn ein Datensatz erstellt wird**. Wenn bereits eine Verbindung vorhanden ist, wählen einer Organisation und die Entität aus der Dropdown-Liste.

    ![](./media/connectors-create-api-crmonline/select-organization.png)

    Wenn Sie aufgefordert werden, sich geben Sie die Zeichen Details zum Herstellen die Verbindung. [Erstellen der Verbindung](connectors-create-api-crmonline.md#create-the-connection) in diesem Thema sind die Schritte aufgeführt. 

    > [AZURE.NOTE] In diesem Beispiel führt die Anwendung Logik beim Erstellen eines Datensatzes. Um die Ergebnisse dieses Triggers fügen Sie eine andere Aktion hinzu, die eine e-Mail gesendet. Fügen Sie z. B. Office 365 *E-Mail* -Aktion, die Sie e-Mails, wenn neuer Datensatz hinzugefügt wird. 

3. Wählen Sie die Schaltfläche **Bearbeiten** und legen Sie die **Häufigkeit** und **Intervall** . Beispielsweise soll den Trigger Abfragen alle 15 Minuten **Häufigkeit** auf **Minuten**festgelegt, und legen Sie das **Intervall** **15**. 

    ![](./media/connectors-create-api-crmonline/edit-properties.png)

4. **Speichern Sie** die geänderte (obere linke Ecke der Symbolleiste). Ihre Anwendung Logik wird gespeichert und werden automatisch aktiviert.


## <a name="use-an-action"></a>Verwenden Sie eine Aktion

Eine Aktion ist eine Operation in eine Logik-app definierten Workflow durchgeführt. [Weitere Informationen zu Aktionen](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Wählen Sie das Pluszeichen. Sie sehen verschiedene Optionen: **eine Aktion hinzufügen**, **Hinzufügen einer Bedingung**oder eine **Weitere** Optionen.

    ![](./media/connectors-create-api-crmonline/add-action.png)

2. Wählen Sie **eine Aktion hinzufügen**.

3. Geben Sie im Textfeld "Dynamik", eine Liste der verfügbaren Aktionen.

    ![](./media/connectors-create-api-crmonline/dynamics-actions.png)

4. Wählen Sie in unserem Beispiel **Dynamics CRM Online - ein Datensatz aktualisiert**. Besteht bereits eine Verbindung wählen Sie den **Organisationsnamen**, **Entitätsnamen**und andere Eigenschaften:  

    ![](./media/connectors-create-api-crmonline/sample-action.png)

    Wenn die Verbindungsinformationen aufgefordert werden, geben Sie die Details der Verbindung. [Erstellen der Verbindung](connectors-create-api-crmonline.md#create-the-connection) in diesem Thema werden diese Eigenschaften beschrieben. 

    > [AZURE.NOTE] In diesem Beispiel haben wir einen vorhandenen Datensatz in CRM Online aktualisieren. Ausgabe von einem anderen Trigger können Sie um den Datensatz zu aktualisieren. Beispielsweise SharePoint *Änderung ein vorhandenes Elements* Trigger hinzufügen Fügen Sie CRM Online *einen Datensatz aktualisieren,* die Aktion, die SharePoint-Felder verwendet, um den vorhandenen Datensatz in CRM Online aktualisieren. 

5. **Speichern Sie** die geänderte (obere linke Ecke der Symbolleiste). Ihre Anwendung Logik wird gespeichert und werden automatisch aktiviert.


## <a name="technical-details"></a>Technische Details

## <a name="triggers"></a>Trigger

|Trigger | Beschreibung|
|--- | ---|
|[Wenn ein Datensatz erstellt](connectors-create-api-crmonline.md#when-a-record-is-created)|Löst einen beim Erstellen eines Objekts in CRM.|
|[Beim Aktualisieren eines Datensatzes](connectors-create-api-crmonline.md#when-a-record-is-updated)|Löst einen, wenn ein Objekt in CRM geändert wird.|
|[Wenn ein Datensatz gelöscht](connectors-create-api-crmonline.md#when-a-record-is-deleted)|Löst einen beim Löschen eines Objekts in CRM.|


## <a name="actions"></a>Aktionen

|Aktion|Beschreibung|
|--- | ---|
|[Datensätze](connectors-create-api-crmonline.md#list-records)|Dieser Vorgang ruft Datensätze für eine Entität.|
|[Erstellen eines neuen Datensatzes](connectors-create-api-crmonline.md#create-a-new-record)|Dieser Vorgang erstellt einen neuen Datensatz einer Entität.|
|[Erhalten Sie](connectors-create-api-crmonline.md#get-record)|Mit diesem Vorgang wird den angegebenen Datensatz für eine Entität.|
|[Löschen eines Datensatzes](connectors-create-api-crmonline.md#delete-a-record)|Dieser Vorgang löscht einen Datensatz aus einer Entität.|
|[Aktualisieren eines Datensatzes](connectors-create-api-crmonline.md#update-a-record)|Dieser Vorgang aktualisiert einen vorhandenen Datensatz für eine Entität.|

### <a name="trigger-and-action-details"></a>Trigger und die Aktion

Finden Sie in diesem Abschnitt Einzelheiten zu jeder Trigger Maßnahmen erforderlichen oder optionalen Eingaben Eigenschaften und entsprechende Ausgabe der Connector zugeordnet.

#### <a name="when-a-record-is-created"></a>Wenn ein Datensatz erstellt
Löst einen beim Erstellen eines Objekts in CRM. 

|Eigenschaftenname| Angezeigter name|Beschreibung|
| ---|---|---|
|DataSet *|Name der Organisation|Name der CRM-Organisation wie Contoso|
|Tabelle *|Entitätsname|Name der Entität|
|$skip|Anzahl der Durchläufe|Anzahl der Einträge zu überspringen (Standard = 0)|
|$top|Maximale Get Count|Maximale Anzahl Einträge abrufen (Standard = 256)|
|$filter|Filterabfrage|Eine Filterabfrage ODATA zurückgegebenen Einträge beschränken|
|$orderby|Sortieren nach|Ein ODATA OrderBy-Abfrage zum Festlegen der Reihenfolge von Einträgen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
|---|---|
|Wert|Array|


#### <a name="when-a-record-is-updated"></a>Beim Aktualisieren eines Datensatzes
Löst einen, wenn ein Objekt in CRM geändert wird. 

|Eigenschaftenname| Angezeigter name|Beschreibung|
| ---|---|---|
|DataSet *|Name der Organisation|Name der CRM-Organisation wie Contoso|
|Tabelle *|Entitätsname|Name der Entität|
|$skip|Anzahl der Durchläufe|Anzahl der Einträge zu überspringen (Standard = 0)|
|$top|Maximale Get Count|Maximale Anzahl Einträge abrufen (Standard = 256)|
|$filter|Filterabfrage|Eine Filterabfrage ODATA zurückgegebenen Einträge beschränken|
|$orderby|Sortieren nach|Ein ODATA OrderBy-Abfrage zum Festlegen der Reihenfolge von Einträgen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
|---|---|
|Wert|Array|


#### <a name="when-a-record-is-deleted"></a>Wenn ein Datensatz gelöscht
Löst einen beim Löschen eines Objekts in CRM. 

|Eigenschaftenname| Angezeigter name|Beschreibung|
| ---|---|---|
|DataSet *|Name der Organisation|Name der CRM-Organisation wie Contoso|
|Tabelle *|Entitätsname|Name der Entität|
|$skip|Anzahl der Durchläufe|Anzahl der Einträge zu überspringen (Standard = 0)|
|$top|Maximale Get Count|Maximale Anzahl Einträge abrufen (Standard = 256)|
|$filter|Filterabfrage|Eine Filterabfrage ODATA zurückgegebenen Einträge beschränken|
|$orderby|Sortieren nach|Ein ODATA OrderBy-Abfrage zum Festlegen der Reihenfolge von Einträgen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
|---|---|
|Wert|Array|


#### <a name="list-records"></a>Datensätze
Dieser Vorgang ruft Datensätze für eine Entität. 

|Eigenschaftenname| Angezeigter name|Beschreibung|
| ---|---|---|
|DataSet *|Name der Organisation|Name der CRM-Organisation wie Contoso|
|Tabelle *|Entitätsname|Name der Entität|
|$skip|Anzahl der Durchläufe|Anzahl der Einträge zu überspringen (Standard = 0)|
|$top|Maximale Get Count|Maximale Anzahl Einträge abrufen (Standard = 256)|
|$filter|Filterabfrage|Eine Filterabfrage ODATA zurückgegebenen Einträge beschränken|
|$orderby|Sortieren nach|Ein ODATA OrderBy-Abfrage zum Festlegen der Reihenfolge von Einträgen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
|---|---|
|Wert|Array|


#### <a name="create-a-new-record"></a>Erstellen eines neuen Datensatzes
Dieser Vorgang erstellt einen neuen Datensatz einer Entität. 

|Eigenschaftenname| Angezeigter name|Beschreibung|
| ---|---|---|
|DataSet *|Name der Organisation|Name der CRM-Organisation wie Contoso|
|Tabelle *|Entitätsname|Name der Entität|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine.


#### <a name="get-record"></a>Erhalten Sie
Mit diesem Vorgang wird den angegebenen Datensatz für eine Entität. 

|Eigenschaftenname| Angezeigter name|Beschreibung|
| ---|---|---|
|DataSet *|Name der Organisation|Name der CRM-Organisation wie Contoso|
|Tabelle *|Entitätsname|Name der Entität|
|ID *|Bezeichner|Geben Sie den Bezeichner für den Datensatz|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine.


#### <a name="delete-a-record"></a>Löschen eines Datensatzes
Dieser Vorgang löscht einen Datensatz aus einer Entität. 

|Eigenschaftenname| Angezeigter name|Beschreibung|
| ---|---|---|
|DataSet *|Name der Organisation|Name der CRM-Organisation wie Contoso|
|Tabelle *|Entitätsname|Name der Entität|
|ID *|Bezeichner|Geben Sie den Bezeichner für den Datensatz|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.


#### <a name="update-a-record"></a>Aktualisieren eines Datensatzes
Dieser Vorgang aktualisiert einen vorhandenen Datensatz für eine Entität. 

|Eigenschaftenname| Angezeigter name|Beschreibung|
| ---|---|---|
|DataSet *|Name der Organisation|Name der CRM-Organisation wie Contoso|
|Tabelle *|Entitätsname|Name der Entität|
|ID *|Datensatz-ID|Geben Sie den Bezeichner für den Datensatz|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine.


## <a name="http-responses"></a>HTTP-Antworten

Aktionen und Triggern können eine oder mehrere der folgenden HTTP-Statuscodes zurück: 

|Name|Beschreibung|
|---|---|
|200|Okay|
|202|Akzeptiert|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Es ist ein Fehler aufgetreten.|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md). Durchsuchen der verfügbaren Connectors Logik Apps [APIs Liste](apis-list.md).

