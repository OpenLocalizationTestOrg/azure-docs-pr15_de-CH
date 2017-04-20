<properties
pageTitle="Wunderlist | Microsoft Azure"
description="Erstellen von Logik apps mit Azure App. Wunderlist bieten einen Todo Liste und Task-Manager Menschen ihre erledigen.  Ob Sie eine Einkaufsliste mit freigeben, erleichtert an einem Projekt arbeiten, oder planen Sie einen Urlaub Wunderlist aufnehmen, weitergeben und Ausführen der To¬dos. Wunderlist synchronisiert sofort Ihr Telefon und Tablet-Computer, damit Sie Ihre Aufgaben von überall zugreifen können."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-wunderlist-connector"></a>Erste Schritte mit Wunderlist-Anschluss

Wunderlist bieten einen Todo Liste und Task-Manager Menschen ihre erledigen.  Ob Sie eine Einkaufsliste mit freigeben, erleichtert an einem Projekt arbeiten, oder planen Sie einen Urlaub Wunderlist aufnehmen, weitergeben und Ausführen der To¬dos. Wunderlist synchronisiert sofort Ihr Telefon und Tablet-Computer, damit Sie Ihre Aufgaben von überall zugreifen können.

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2015-08-01-Vorschau-Schemaversion. 

Sie können jetzt eine Logik-app erstellen machen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Wunderlist-Connector kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen in JSON und XML-Daten. 

 Wunderlist Konnektor hat folgende Aktionen und Triggern verfügbar:

### <a name="wunderlist-actions"></a>Wunderlist Aktionen
Sie können diese Aktionen durchführen:

|Aktion|Beschreibung|
|--- | ---|
|[RetrieveLists](connectors-create-api-wunderlist.md#retrievelists)|Abrufen von Listen mit Ihrem Konto verknüpft ist.|
|[CreateList](connectors-create-api-wunderlist.md#createlist)|Erstellen einer Liste.|
|[ListTasks](connectors-create-api-wunderlist.md#listtasks)|Aufgaben aus einer bestimmten Liste abrufen|
|[CreateTask](connectors-create-api-wunderlist.md#createtask)|Erstellen einer Aufgabe|
|[ListSubTasks](connectors-create-api-wunderlist.md#listsubtasks)|Teilvorgänge aus einer bestimmten Liste oder eine bestimmte Aufgabe abrufen|
|[CreateSubTask](connectors-create-api-wunderlist.md#createsubtask)|Erstellen Sie eine Unteraufgabe innerhalb einer bestimmten Aufgabe|
|[ListNotes](connectors-create-api-wunderlist.md#listnotes)|Notizen für eine bestimmte Liste oder eine bestimmte Aufgabe abrufen.|
|[CreateNote](connectors-create-api-wunderlist.md#createnote)|Hinzufügen einer Notiz zu einer bestimmten Aufgabe|
|[ListComments](connectors-create-api-wunderlist.md#listcomments)|Aufgabenkommentare für eine bestimmte Liste oder eine bestimmte Aufgabe abrufen|
|[CreateComment](connectors-create-api-wunderlist.md#createcomment)|Hinzufügen eines Kommentars zu einer bestimmten Aufgabe|
|[RetrieveReminders](connectors-create-api-wunderlist.md#retrievereminders)|Erinnerung für eine bestimmte Liste oder eine bestimmte Aufgabe abrufen|
|[CreateReminder](connectors-create-api-wunderlist.md#createreminder)|Erinnerung ein.|
|[RetrieveFiles](connectors-create-api-wunderlist.md#retrievefiles)|Dateien für eine bestimmte Liste oder eine bestimmte Aufgabe abrufen.|
|[GetList](connectors-create-api-wunderlist.md#getlist)|Ruft eine Liste ab|
|[DeleteList](connectors-create-api-wunderlist.md#deletelist)|Löscht eine Liste|
|[UpdateList](connectors-create-api-wunderlist.md#updatelist)|Eine bestimmte Liste aktualisieren|
|[GetTask](connectors-create-api-wunderlist.md#gettask)|Ruft eine bestimmte Aufgabe|
|[UpdateTask](connectors-create-api-wunderlist.md#updatetask)|Aktualisiert eine bestimmte Aufgabe|
|[DeleteTask](connectors-create-api-wunderlist.md#deletetask)|Löscht eine bestimmte Aufgabe|
|[GetSubTask](connectors-create-api-wunderlist.md#getsubtask)|Ruft einen bestimmten Teilvorgang|
|[UpdateSubTask](connectors-create-api-wunderlist.md#updatesubtask)|Aktualisiert einen bestimmten Teilvorgang|
|[DeleteSubTask](connectors-create-api-wunderlist.md#deletesubtask)|Löscht einen bestimmten Teilvorgang|
|[GetNote](connectors-create-api-wunderlist.md#getnote)|Abrufen einer bestimmten Notiz|
|[UpdateNote](connectors-create-api-wunderlist.md#updatenote)|Aktualisieren einer bestimmten Notiz|
|[DeleteNote](connectors-create-api-wunderlist.md#deletenote)|Löschen einer bestimmten Notiz|
|[GetComment](connectors-create-api-wunderlist.md#getcomment)|Abrufen eines bestimmten Aufgabe Kommentars|
|[UpdateReminder](connectors-create-api-wunderlist.md#updatereminder)|Aktualisieren einer bestimmten Erinnerungsdatums|
|[DeleteReminder](connectors-create-api-wunderlist.md#deletereminder)|Eine bestimmte Erinnerung löschen|
### <a name="wunderlist-triggers"></a>Wunderlist Trigger
Sie können diese Ereignisse überwachen:

|Trigger | Beschreibung|
|--- | ---|
|Wenn eine Aufgabe fällig ist|Löst einen neuen Fluss eine Aufgabe in der Liste fällig ist|
|Wenn ein neuer Vorgang erstellt|Löst einen neuen Datenfluss beim Erstellen eine neue Aufgabe in der Liste|
|Tritt eine Erinnerung|Löst einen neuen Fluss tritt eine Erinnerung|


## <a name="create-a-connection-to-wunderlist"></a>Erstellen einer Verbindung mit Wunderlist
Um Logik apps mit Wunderlist erstellen, müssen Sie zunächst eine **Verbindung** anschließend geben Sie die Details für die folgenden Eigenschaften: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Anmeldeinformationen für Wunderlist|
Nach dem Erstellen der Verbindung können sie Aktionen ausführen und warten Sie in diesem Artikel beschriebenen Trigger. 


>[AZURE.INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)] 


>[AZURE.TIP] Hierzu können Sie in anderen apps Logik.

## <a name="reference-for-wunderlist"></a>Referenz für Wunderlist
Gilt für Version: 1.0

## <a name="triggertaskdue"></a>TriggerTaskDue
Wenn eine Aufgabe fällig ist: löst einen neuen Fluss eine Aufgabe in der Liste fällig ist 

```GET: /trigger/tasksdue``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|


## <a name="triggertasknew"></a>TriggerTaskNew
Wenn ein neuer Vorgang erstellt: löst keinen neuen Datenfluss beim Erstellen eine neue Aufgabe in der Liste 

```GET: /trigger/tasksnew``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|


## <a name="triggerreminder"></a>TriggerReminder
Tritt eine Erinnerung: löst einen neuen Fluss tritt eine Erinnerung 

```GET: /trigger/reminders``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|TASK_ID|ganze Zahl|Nein|Abfrage|keine|Aufgaben-ID|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|


## <a name="retrievelists"></a>RetrieveLists
Listen abrufen: Abrufen von Listen mit Ihrem Konto verknüpft ist. 

```GET: /lists``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="createlist"></a>CreateList
Erstellen Sie eine Liste: eine Liste erstellen. 

```POST: /lists``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Bereitstellen| |Ja|Körper|keine|Neue Liste erstellt werden|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="listtasks"></a>ListTasks
Abrufen von Aufgaben: Aufgaben aus einer bestimmten Liste abrufen. 

```GET: /tasks``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|abgeschlossen|boolescher Wert|Nein|Abfrage|keine|Abgeschlossen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="createtask"></a>CreateTask
Erstellen Sie eine Aufgabe: eine Aufgabe erstellen 

```POST: /tasks``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Bereitstellen| |Ja|Körper|keine|Neue Aufgabe erstellt werden|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|201|Erstellt|


## <a name="listsubtasks"></a>ListSubTasks
Teilvorgänge erhalten: Teilvorgänge aus einer bestimmten Liste oder aus einer bestimmten Aufgabe abrufen. 

```GET: /subtasks``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|TASK_ID|ganze Zahl|Nein|Abfrage|keine|Aufgaben-ID|
|abgeschlossen|boolescher Wert|Nein|Abfrage|keine|Abgeschlossen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="createsubtask"></a>CreateSubTask
Erstellen Sie eine Unteraufgabe: Erstellen Sie eine Unteraufgabe innerhalb einer bestimmten Aufgabe 

```POST: /subtasks``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Bereitstellen| |Ja|Körper|keine|Neue Teilvorgang erstellt werden|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|201|Erstellt|


## <a name="listnotes"></a>ListNotes
Notizen: Notizen für eine bestimmte Liste oder eine bestimmte Aufgabe abrufen. 

```GET: /notes``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|TASK_ID|ganze Zahl|Nein|Abfrage|keine|Aufgaben-ID|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="createnote"></a>CreateNote
Erstellen Sie eine Anmerkung: Hinzufügen einer Notiz zu einer bestimmten Aufgabe 

```POST: /notes``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Bereitstellen| |Ja|Körper|keine|Neue Notiz erstellt werden|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|201|Erstellt|


## <a name="listcomments"></a>ListComments
Aufgabe Kommentare: Aufgabenkommentaren für eine bestimmte Liste oder eine bestimmte Aufgabe abrufen. 

```GET: /task_comments``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|TASK_ID|ganze Zahl|Nein|Abfrage|keine|Aufgaben-ID|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="createcomment"></a>CreateComment
Hinzufügen eines Kommentars zu einer Aufgabe: Hinzufügen eines Kommentars zu einer bestimmten Aufgabe 

```POST: /task_comments``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Bereitstellen| |Ja|Körper|keine|Neue Aufgabenkommentar erstellt werden|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|201|Erstellt|


## <a name="retrievereminders"></a>RetrieveReminders
Erinnerungen: Erinnerungen für eine Liste oder eine bestimmte Aufgabe abrufen. 

```GET: /reminders``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|TASK_ID|ganze Zahl|Nein|Abfrage|keine|Aufgaben-ID|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="createreminder"></a>CreateReminder
Einrichten einer Erinnerung: Erinnerung ein. 

```POST: /reminders``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Bereitstellen| |Ja|Körper|keine|Neue Erinnerung erstellt werden|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="retrievefiles"></a>RetrieveFiles
Dateien: Dateien für eine bestimmte Liste oder eine bestimmte Aufgabe abrufen. 

```GET: /files``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|TASK_ID|ganze Zahl|Nein|Abfrage|keine|Aufgaben-ID|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="getlist"></a>GetList
Liste: Ruft eine Liste ab 

```GET: /lists/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine|Listen-ID|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|


## <a name="deletelist"></a>DeleteList
Liste löschen: Löscht eine Liste 

```DELETE: /lists/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|Listen-ID|
|Revision|ganze Zahl|Ja|Abfrage|keine|Revision|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|204|Kein Inhalt|


## <a name="updatelist"></a>UpdateList
Aktualisieren einer Liste: eine spezifische Liste 

```PATCH: /lists/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|Listen-ID|
|Bereitstellen| |Ja|Körper|keine|Detailinformationen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|


## <a name="gettask"></a>GetTask
Aufgabe: eine bestimmte Aufgabe abgerufen 

```GET: /tasks/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|ID|ganze Zahl|Ja|Pfad|keine|Aufgaben-ID|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|


## <a name="updatetask"></a>UpdateTask
Aktualisieren einer Aufgabe: eine bestimmte Aufgabe aktualisiert 

```PATCH: /tasks/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|ID|ganze Zahl|Ja|Pfad|keine|Aufgaben-ID|
|Bereitstellen| |Ja|Körper|keine|Aufgabendetails|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|


## <a name="deletetask"></a>DeleteTask
Löschtask: Löscht eine bestimmte Aufgabe 

```DELETE: /tasks/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|ID|ganze Zahl|Ja|Pfad|keine|Aufgaben-ID|
|Revision|ganze Zahl|Ja|Abfrage|keine|Revision|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|204|Kein Inhalt|


## <a name="getsubtask"></a>GetSubTask
Unteraufgabe erhalten: Ruft einen bestimmten Teilvorgang 

```GET: /subtasks/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine|Unteraufgabe ID|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|


## <a name="updatesubtask"></a>UpdateSubTask
Eine Unteraufgabe aktualisieren: aktualisiert einen bestimmten Teilvorgang 

```PATCH: /subtasks/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|Unteraufgabe ID|
|Bereitstellen| |Ja|Körper|keine|Unteraufgabe details|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|


## <a name="deletesubtask"></a>DeleteSubTask
Eine Unteraufgabe löschen: Löscht einen bestimmten Teilvorgang 

```DELETE: /subtasks/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|Unteraufgabe ID|
|Revision|ganze Zahl|Ja|Abfrage|keine|Revision|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|204|Kein Inhalt|


## <a name="getnote"></a>GetNote
Erhalten Sie einen Hinweis: Abrufen eine bestimmte Notiz 

```GET: /notes/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine|Notiz ID|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|


## <a name="updatenote"></a>UpdateNote
Aktualisieren eine Notiz: Aktualisieren eine bestimmte Notiz 

```PATCH: /notes/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|Notiz ID|
|Bereitstellen| |Ja|Körper|keine|Anmerkung Weitere Informationen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|


## <a name="deletenote"></a>DeleteNote
Löschen einer Notiz: Löschen einer bestimmte Notiz 

```DELETE: /notes/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|Notiz ID|
|Revision|ganze Zahl|Ja|Abfrage|keine|Revision|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|204|Kein Inhalt|


## <a name="getcomment"></a>GetComment
Aufgabe Kommentar: einen bestimmte Aufgabenkommentar abrufen 

```GET: /task_comments/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine|Kommentar-ID|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|


## <a name="updatereminder"></a>UpdateReminder
Aktualisieren einer Erinnerung: Aktualisieren einer bestimmte Erinnerung 

```PATCH: /reminders/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|Erinnerung-ID|
|Bereitstellen| |Ja|Körper|keine|Erinnerung-details|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|


## <a name="deletereminder"></a>DeleteReminder
Löschen Erinnerung: eine bestimmte Erinnerung löschen 

```DELETE: /reminders/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|ID der Mahnung.|
|Revision|ganze Zahl|Ja|Abfrage|keine|Revision|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|204|Kein Inhalt|


## <a name="object-definitions"></a>Objektdefinitionen 

### <a name="list"></a>Liste


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|created_at|Zeichenfolge|Nein |
|Titel|Zeichenfolge|Nein |
|list_type|Zeichenfolge|Nein |
|Typ|Zeichenfolge|Nein |
|Revision|ganze Zahl|Nein |



### <a name="createdlist"></a>CreatedList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|created_at|Zeichenfolge|Nein |
|Titel|Zeichenfolge|Nein |
|Revision|ganze Zahl|Nein |
|Typ|Zeichenfolge|Nein |



### <a name="task"></a>Aufgabe


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|assignee_id|ganze Zahl|Nein |
|assigner_id|ganze Zahl|Nein |
|created_at|Zeichenfolge|Nein |
|created_by_id|ganze Zahl|Nein |
|Fälligkeitsdatum|Zeichenfolge|Nein |
|list_id|ganze Zahl|Nein |
|Revision|ganze Zahl|Nein |
|Favoriten|boolescher Wert|Nein |
|Titel|Zeichenfolge|Nein |



### <a name="subtask"></a>Unteraufgabe


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|TASK_ID|ganze Zahl|Nein |
|created_at|Zeichenfolge|Nein |
|created_by_id|ganze Zahl|Nein |
|Revision|Zeichenfolge|Nein |
|Titel|Zeichenfolge|Nein |



### <a name="note"></a>Hinweis


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|TASK_ID|ganze Zahl|Nein |
|Inhalt|Zeichenfolge|Nein |
|created_at|Zeichenfolge|Nein |
|updated_at|Zeichenfolge|Nein |
|Revision|ganze Zahl|Nein |



### <a name="comment"></a>Kommentar


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|TASK_ID|ganze Zahl|Nein |
|Revision|ganze Zahl|Nein |
|Text|Zeichenfolge|Nein |
|Typ|Zeichenfolge|Nein |
|created_at|Zeichenfolge|Nein |



### <a name="reminder"></a>Erinnerung


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|Datum|Zeichenfolge|Nein |
|TASK_ID|ganze Zahl|Nein |
|Revision|ganze Zahl|Nein |
|Typ|Zeichenfolge|Nein |
|created_at|Zeichenfolge|Nein |
|updated_at|Zeichenfolge|Nein |



### <a name="createdreminder"></a>CreatedReminder


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|Datum|Zeichenfolge|Nein |
|TASK_ID|ganze Zahl|Nein |
|Revision|ganze Zahl|Nein |
|created_at|Zeichenfolge|Nein |
|updated_at|Zeichenfolge|Nein |



### <a name="file"></a>Datei


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|URL|Zeichenfolge|Nein |
|TASK_ID|ganze Zahl|Nein |
|list_id|ganze Zahl|Nein |
|USER_ID|ganze Zahl|Nein |
|Dateiname|Zeichenfolge|Nein |
|content_type|Zeichenfolge|Nein |
|file_size|ganze Zahl|Nein |
|local_created_at|Zeichenfolge|Nein |
|created_at|Zeichenfolge|Nein |
|updated_at|Zeichenfolge|Nein |
|Typ|Zeichenfolge|Nein |
|Revision|ganze Zahl|Nein |



### <a name="newtask"></a>NewTask


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|list_id|ganze Zahl|Ja |
|Titel|Zeichenfolge|Ja |
|assignee_id|ganze Zahl|Nein |
|abgeschlossen|boolescher Wert|Nein |
|recurrence_type|Zeichenfolge|Nein |
|recurrence_count|ganze Zahl|Nein |
|Fälligkeitsdatum|Zeichenfolge|Nein |
|Favoriten|boolescher Wert|Nein |



### <a name="newlist"></a>NewList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Titel|Zeichenfolge|Ja |



### <a name="newsubtask"></a>NewSubtask


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|list_id|ganze Zahl|Ja |
|TASK_ID|ganze Zahl|Ja |
|Titel|Zeichenfolge|Ja |
|abgeschlossen|boolescher Wert|Nein |



### <a name="newnote"></a>NewNote


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|list_id|ganze Zahl|Ja |
|TASK_ID|ganze Zahl|Ja |
|Inhalt|Zeichenfolge|Ja |



### <a name="newcomment"></a>NeuerKommentar


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|list_id|ganze Zahl|Ja |
|TASK_ID|ganze Zahl|Ja |
|Text|Zeichenfolge|Ja |



### <a name="newreminder"></a>NewReminder


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|list_id|ganze Zahl|Ja |
|TASK_ID|ganze Zahl|Ja |
|Datum|Zeichenfolge|Ja |



### <a name="updatetask"></a>UpdateTask


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Revision|ganze Zahl|Nein |
|Titel|Zeichenfolge|Nein |
|assignee_id|ganze Zahl|Nein |
|abgeschlossen|boolescher Wert|Nein |
|recurrence_type|Zeichenfolge|Nein |
|recurrence_count|ganze Zahl|Nein |
|Fälligkeitsdatum|Zeichenfolge|Nein |
|Favoriten|boolescher Wert|Nein |



### <a name="updatelist"></a>UpdateList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Revision|ganze Zahl|Nein |
|Titel|Zeichenfolge|Nein |



### <a name="updatesubtask"></a>UpdateSubtask


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Revision|ganze Zahl|Nein |
|Titel|Zeichenfolge|Nein |
|abgeschlossen|boolescher Wert|Nein |



### <a name="updatenote"></a>UpdateNote


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Revision|ganze Zahl|Nein |
|Inhalt|Zeichenfolge|Nein |



### <a name="updatereminder"></a>UpdateReminder


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Datum|Zeichenfolge|Nein |
|Revision|ganze Zahl|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)