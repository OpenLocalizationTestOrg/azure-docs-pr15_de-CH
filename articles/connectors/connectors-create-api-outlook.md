<properties
pageTitle="Outlook.com | Microsoft Azure"
description="Erstellen von Logik apps mit Azure App. Outlook.com-Connector können Sie Ihre e-Mail, Kalender und Kontakte verwalten. Sie können Aktionen wie e-Mail, Termine, Kontakte usw.."
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

# <a name="get-started-with-the-outlookcom-connector"></a>Erste Schritte mit Outlook.com-Anschluss

Outlook.com-Connector können Sie Ihre e-Mail, Kalender und Kontakte verwalten. Sie können Aktionen wie e-Mail, Termine, Kontakte usw..

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2015-08-01-Vorschau-Schemaversion. 

Sie können jetzt eine Logik-app erstellen machen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Outlook.com-Connector kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen in JSON und XML-Daten. 

 Outlook.com Konnektor hat folgende Aktionen und Triggern verfügbar:

### <a name="outlookcom-actions"></a>Outlook.com Aktionen
Sie können diese Aktionen durchführen:

|Aktion|Beschreibung|
|--- | ---|
|[GetEmails](connectors-create-api-outlook.md#GetEmails)|Ruft e-Mails in einem Ordner|
|[SendEmail](connectors-create-api-outlook.md#SendEmail)|Sendet eine e-Mail|
|[DeleteEmail](connectors-create-api-outlook.md#DeleteEmail)|Löscht einen e-Mail-id|
|[MarkAsRead](connectors-create-api-outlook.md#MarkAsRead)|Eine e-Mail als gelesen markiert|
|[ReplyTo](connectors-create-api-outlook.md#ReplyTo)|Antworten auf eine e-Mail-Nachricht|
|[GetAttachment](connectors-create-api-outlook.md#GetAttachment)|Ruft e-Mail-Anhang nach id|
|[SendMailWithOptions](connectors-create-api-outlook.md#SendMailWithOptions)|Senden Sie eine e-Mail mit mehreren Optionen und warten Sie, bis des Empfängers antwortet mit einer der Optionen|
|[SendApprovalMail](connectors-create-api-outlook.md#SendApprovalMail)|Senden Sie eine e-Mail mit der Genehmigung und Warten auf eine Antwort vom Empfänger|
|[CalendarGetTables](connectors-create-api-outlook.md#CalendarGetTables)|Ruft Kalender|
|[CalendarGetItems](connectors-create-api-outlook.md#CalendarGetItems)|Ruft Elemente in einem Kalender|
|[CalendarPostItem](connectors-create-api-outlook.md#CalendarPostItem)|Erstellt ein neues Ereignis|
|[CalendarGetItem](connectors-create-api-outlook.md#CalendarGetItem)|Ruft ein bestimmtes Element in einem Kalender|
|[CalendarDeleteItem](connectors-create-api-outlook.md#CalendarDeleteItem)|Löscht ein Kalenderelement|
|[CalendarPatchItem](connectors-create-api-outlook.md#CalendarPatchItem)|Teilweise aktualisiert ein Kalenderelement|
|[ContactGetTables](connectors-create-api-outlook.md#ContactGetTables)|Ruft Kontakteordner|
|[ContactGetItems](connectors-create-api-outlook.md#ContactGetItems)|Kontakte aus einem Kontakteordner abgerufen|
|[ContactPostItem](connectors-create-api-outlook.md#ContactPostItem)|Erstellt einen neuen Kontakt|
|[ContactGetItem](connectors-create-api-outlook.md#ContactGetItem)|Ruft einen bestimmten Kontakt in einem Kontakteordner|
|[ContactDeleteItem](connectors-create-api-outlook.md#ContactDeleteItem)|Wird ein Kontakt gelöscht|
|[ContactPatchItem](connectors-create-api-outlook.md#ContactPatchItem)|Teilweise aktualisiert einen Kontakt|
### <a name="outlookcom-triggers"></a>Outlook.com Trigger
Sie können diese Ereignisse überwachen:

|Trigger | Beschreibung|
|--- | ---|
|Ereignis schnell gestartet|Löst einen Beginn einer bevorstehenden Ereignis|
|Neue e-Mail|Löst einen beim Eintreffen einer neuen e-Mails|
|Auf neue Elemente|Wird ausgelöst, wenn ein neues Kalenderelement erstellt wird|
|Auf aktualisierte Elemente|Wird ausgelöst, wenn ein Kalenderelement geändert wird|


## <a name="create-a-connection-to-outlookcom"></a>Erstellen Sie eine Verbindung zu Outlook.com
Um Logik apps mit Outlook.com erstellen, müssen Sie zunächst eine **Verbindung** anschließend geben Sie die Details für die folgenden Eigenschaften: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Anmeldeinformationen für Outlook.com|
Nach dem Erstellen der Verbindung können sie Aktionen ausführen und warten Sie in diesem Artikel beschriebenen Trigger.

>[AZURE.INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)] 

>[AZURE.TIP] Hierzu können Sie in anderen apps Logik.  

## <a name="reference-for-outlookcom"></a>Referenz für Outlook.com
Gilt für Version: 1.0

## <a name="onupcomingevents"></a>OnUpcomingEvents
Ereignis schnell gestartet: löst einen Beginn einer bevorstehenden Ereignis 

```GET: /Events/OnUpcomingEvents``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Abfrage|keine|Eindeutiger Bezeichner des Kalenders|
|lookAheadTimeInMinutes|ganze Zahl|Nein|Abfrage|15|Zeit (in Minuten) für Events blicken|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|202|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="getemails"></a>GetEmails
E-Mails: Ruft e-Mails in einem Ordner 

```GET: /Mail``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|folderPath|Zeichenfolge|Nein|Abfrage|Posteingang|Pfad des Ordners e-Mails abrufen (Standard: 'Posteingang')|
|Nach oben|ganze Zahl|Nein|Abfrage|10|Anzahl der e-Mails abrufen (Standard: 10)|
|fetchOnlyUnread|boolescher Wert|Nein|Abfrage|true|Abrufen nur ungelesene e-Mails?|
|includeAttachments|boolescher Wert|Nein|Abfrage|falsch|Wenn auf true Anlagen auch mit der e-Mail abgerufen werden|
|Einfach|Zeichenfolge|Nein|Abfrage|keine|Suchabfrage e-Mails filtern|
|Überspringen|ganze Zahl|Nein|Abfrage|0|Anzahl der e-Mails überspringen (Standard: 0)|
|skipToken|Zeichenfolge|Nein|Abfrage|keine|Token Abrufen neuer Seite überspringen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="sendemail"></a>SendEmail
E-Mail: sendet eine e-Mail 

```POST: /Mail``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|emailMessage| |Ja|Körper|keine|E-Mail|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="deleteemail"></a>DeleteEmail
E-Mail löschen: Löscht eine e-Mail nach Id 

```DELETE: /Mail/{messageId}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|messageId|Zeichenfolge|Ja|Pfad|keine|ID der e-Mail löschen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="markasread"></a>MarkAsRead
Als gelesen markieren: eine e-Mail als gelesen markiert 

```POST: /Mail/MarkAsRead/{messageId}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|messageId|Zeichenfolge|Ja|Pfad|keine|ID der e-Mail als gekennzeichnet werden gelesen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="replyto"></a>ReplyTo
Antwort auf e-Mail: Antworten auf eine e-Mail-Nachricht 

```POST: /Mail/ReplyTo/{messageId}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|messageId|Zeichenfolge|Ja|Pfad|keine|ID der e-Mail beantworten|
|Kommentar|Zeichenfolge|Ja|Abfrage|keine|Bewertungskommentar|
|replyAll|boolescher Wert|Nein|Abfrage|falsch|Allen Empfängern Antworten|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="getattachment"></a>GetAttachment
Anlagen abrufen: Ruft e-Mail-Anhang nach Id 

```GET: /Mail/{messageId}/Attachments/{attachmentId}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|messageId|Zeichenfolge|Ja|Pfad|keine|ID der e-Mail|
|attachmentId|Zeichenfolge|Ja|Pfad|keine|ID der Anlage herunterladen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="onnewemail"></a>OnNewEmail
Neue e-Mail: löst einen beim Eintreffen einer neuen e-Mails 

```GET: /Mail/OnNewEmail``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|folderPath|Zeichenfolge|Nein|Abfrage|Posteingang|E-Mail-Ordner abrufen (Standard: Posteingang)|
|An|Zeichenfolge|Nein|Abfrage|keine|Empfänger e-Mail-Adressen|
|Von|Zeichenfolge|Nein|Abfrage|keine|Absenderadresse|
|Bedeutung|Zeichenfolge|Nein|Abfrage|Normal|Bedeutung von e-Mails (hoch, Normal, Niedrig) (Standard: Normal)|
|fetchOnlyWithAttachment|boolescher Wert|Nein|Abfrage|falsch|Nur e-Mail-Nachrichten mit einer Anlage abrufen|
|includeAttachments|boolescher Wert|Nein|Abfrage|falsch|Anlagen|
|subjectFilter|Zeichenfolge|Nein|Abfrage|keine|Die Zeichenfolge, die in der Betreffzeile suchen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|202|Akzeptiert|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="sendmailwithoptions"></a>SendMailWithOptions
E-Mail mit Optionen: eine E-Mail mit mehreren Optionen und warten Sie, bis der Empfänger antwortet mit einer der Optionen 

```POST: /mailwithoptions/$subscriptions``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|optionsEmailSubscription| |Ja|Körper|keine|Abonnementanforderung für die e-Mail-Optionen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|201|Abonnement|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="sendapprovalmail"></a>SendApprovalMail
Genehmigung senden: Senden Sie eine e-Mail mit der Genehmigung und Warten auf eine Antwort vom Empfänger 

```POST: /approvalmail/$subscriptions``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|approvalEmailSubscription| |Ja|Körper|keine|Abonnementanforderung für e-Mail-Genehmigung|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|201|Abonnement|
|400|BadRequest|
|401|Nicht autorisiert|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="calendargettables"></a>CalendarGetTables
Kalender erhalten: Ruft Kalender 

```GET: /datasets/calendars/tables``` 

Es gibt keine Parameter für diesen Aufruf
#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="calendargetitems"></a>CalendarGetItems
Ereignisse: Ruft Elemente in einem Kalender 

```GET: /datasets/calendars/tables/{table}/items``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des Kalenders abrufen|
|$filter|Zeichenfolge|Nein|Abfrage|keine|Eine Filterabfrage ODATA beschränkt die Anzahl der Einträge|
|$orderby|Zeichenfolge|Nein|Abfrage|keine|Ein ODATA OrderBy-Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|ganze Zahl|Nein|Abfrage|keine|Anzahl der Einträge zu überspringen (Standard = 0)|
|$top|ganze Zahl|Nein|Abfrage|keine|Maximale Anzahl Einträge abrufen (Standard = 256)|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="calendarpostitem"></a>CalendarPostItem
Ereignis erstellen: erstellt ein neues Ereignis 

```POST: /datasets/calendars/tables/{table}/items``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner eines Kalenders|
|Artikel| |Ja|Körper|keine|Kalenderelement erstellen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="calendargetitem"></a>CalendarGetItem
Ereignis: Ruft ein bestimmtes Element in einem Kalender 

```GET: /datasets/calendars/tables/{table}/items/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner eines Kalenders|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner eines Kalenderelements abrufen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="calendardeleteitem"></a>CalendarDeleteItem
Ereignis löschen: Löscht einen Kalendereintrag 

```DELETE: /datasets/calendars/tables/{table}/items/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner eines Kalenders|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des Kalenderelements löschen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="calendarpatchitem"></a>CalendarPatchItem
Update-Ereignis: ein Kalenderelement teilweise aktualisiert 

```PATCH: /datasets/calendars/tables/{table}/items/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner eines Kalenders|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des Kalenderelements aktualisieren|
|Artikel| |Ja|Körper|keine|Kalenderelement aktualisieren|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="calendargetonnewitems"></a>CalendarGetOnNewItems
Auf neue Elemente: wird ausgelöst, wenn ein neues Kalenderelement erstellt wird 

```GET: /datasets/calendars/tables/{table}/onnewitems``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner eines Kalenders|
|$filter|Zeichenfolge|Nein|Abfrage|keine|Eine Filterabfrage ODATA beschränkt die Anzahl der Einträge|
|$orderby|Zeichenfolge|Nein|Abfrage|keine|Ein ODATA OrderBy-Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|ganze Zahl|Nein|Abfrage|keine|Anzahl der Einträge zu überspringen (Standard = 0)|
|$top|ganze Zahl|Nein|Abfrage|keine|Maximale Anzahl Einträge abrufen (Standard = 256)|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="calendargetonupdateditems"></a>CalendarGetOnUpdatedItems
Auf Artikel aktualisiert: wird ausgelöst, wenn ein Kalenderelement geändert wird 

```GET: /datasets/calendars/tables/{table}/onupdateditems``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner eines Kalenders|
|$filter|Zeichenfolge|Nein|Abfrage|keine|Eine Filterabfrage ODATA beschränkt die Anzahl der Einträge|
|$orderby|Zeichenfolge|Nein|Abfrage|keine|Ein ODATA OrderBy-Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|ganze Zahl|Nein|Abfrage|keine|Anzahl der Einträge zu überspringen (Standard = 0)|
|$top|ganze Zahl|Nein|Abfrage|keine|Maximale Anzahl Einträge abrufen (Standard = 256)|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="contactgettables"></a>ContactGetTables
Kontakt Ordner: Ruft Kontakte Ordner 

```GET: /datasets/contacts/tables``` 

Es gibt keine Parameter für diesen Aufruf
#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="contactgetitems"></a>ContactGetItems
Kontakte: Kontakte aus einem Kontakteordner abgerufen 

```GET: /datasets/contacts/tables/{table}/items``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des Ordners Kontakte abrufen|
|$filter|Zeichenfolge|Nein|Abfrage|keine|Eine Filterabfrage ODATA beschränkt die Anzahl der Einträge|
|$orderby|Zeichenfolge|Nein|Abfrage|keine|Ein ODATA OrderBy-Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|ganze Zahl|Nein|Abfrage|keine|Anzahl der Einträge zu überspringen (Standard = 0)|
|$top|ganze Zahl|Nein|Abfrage|keine|Maximale Anzahl Einträge abrufen (Standard = 256)|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="contactpostitem"></a>ContactPostItem
Kontakt: erstellt einen neuen Kontakt 

```POST: /datasets/contacts/tables/{table}/items``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner der Kontakteordner|
|Artikel| |Ja|Körper|keine|Kontakt erstellen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="contactgetitem"></a>ContactGetItem
Kontakt: Ruft einen bestimmten Kontakt in einem Kontakteordner 

```GET: /datasets/contacts/tables/{table}/items/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner der Kontakteordner|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des Kontakts abrufen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="contactdeleteitem"></a>ContactDeleteItem
Kontakt löschen: Löscht einen Kontakt 

```DELETE: /datasets/contacts/tables/{table}/items/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner der Kontakteordner|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des Kontakts löschen|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="contactpatchitem"></a>ContactPatchItem
Kontakt aktualisieren: teilweise aktualisiert einen Kontakt 

```PATCH: /datasets/contacts/tables/{table}/items/{id}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner der Kontakteordner|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des Kontakts aktualisieren|
|Artikel| |Ja|Körper|keine|Kontaktelement aktualisieren|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="object-definitions"></a>Objektdefinitionen 

### <a name="triggerbatchresponseidictionarystringobject"></a>TriggerBatchResponse [IDictionary [String, Object]]


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Array|Nein |



### <a name="object"></a>Objekt


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|



### <a name="sendmessage"></a>SendMessage


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Anlagen|Array|Nein |
|Von|Zeichenfolge|Nein |
|Cc|Zeichenfolge|Nein |
|Bcc|Zeichenfolge|Nein |
|Betreff|Zeichenfolge|Ja |
|Körper|Zeichenfolge|Ja |
|Bedeutung|Zeichenfolge|Nein |
|IsHtml|boolescher Wert|Nein |
|An|Zeichenfolge|Ja |



### <a name="sendattachment"></a>SendAttachment


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|@odata.type|Zeichenfolge|Nein |
|Name|Zeichenfolge|Ja |
|ContentBytes|Zeichenfolge|Ja |



### <a name="receivemessage"></a>ReceiveMessage


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|IsRead|boolescher Wert|Nein |
|HasAttachment|boolescher Wert|Nein |
|DateTimeReceived|Zeichenfolge|Nein |
|Anlagen|Array|Nein |
|Von|Zeichenfolge|Nein |
|Cc|Zeichenfolge|Nein |
|Bcc|Zeichenfolge|Nein |
|Betreff|Zeichenfolge|Ja |
|Körper|Zeichenfolge|Ja |
|Bedeutung|Zeichenfolge|Nein |
|IsHtml|boolescher Wert|Nein |
|An|Zeichenfolge|Ja |



### <a name="receiveattachment"></a>ReceiveAttachment


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Ja |
|ContentType|Zeichenfolge|Ja |
|@odata.type|Zeichenfolge|Nein |
|Name|Zeichenfolge|Ja |
|ContentBytes|Zeichenfolge|Ja |



### <a name="digestmessage"></a>DigestMessage


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Betreff|Zeichenfolge|Ja |
|Körper|Zeichenfolge|Nein |
|Bedeutung|Zeichenfolge|Nein |
|Übersicht|Array|Ja |
|Anlagen|Array|Nein |
|An|Zeichenfolge|Ja |



### <a name="triggerbatchresponsereceivemessage"></a>TriggerBatchResponse [ReceiveMessage]


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Array|Nein |



### <a name="datasetsmetadata"></a>DataSetsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|tabellarische|nicht definiert|Nein |
|BLOB|nicht definiert|Nein |



### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Quelle|Zeichenfolge|Nein |
|displayName|Zeichenfolge|Nein |
|urlEncoding|Zeichenfolge|Nein |
|tableDisplayName|Zeichenfolge|Nein |
|tablePluralName|Zeichenfolge|Nein |



### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Quelle|Zeichenfolge|Nein |
|displayName|Zeichenfolge|Nein |
|urlEncoding|Zeichenfolge|Nein |



### <a name="tablemetadata"></a>TableMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Name|Zeichenfolge|Nein |
|Titel|Zeichenfolge|Nein |
|X-ms-Berechtigung|Zeichenfolge|Nein |
|X-ms-Funktionen|nicht definiert|Nein |
|Schema|nicht definiert|Nein |



### <a name="tablecapabilitiesmetadata"></a>TableCapabilitiesMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|sortRestrictions|nicht definiert|Nein |
|filterRestrictions|nicht definiert|Nein |
|filterFunctions|Array|Nein |



### <a name="tablesortrestrictionsmetadata"></a>TableSortRestrictionsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|sortierbar|boolescher Wert|Nein |
|unsortableProperties|Array|Nein |
|ascendingOnlyProperties|Array|Nein |



### <a name="tablefilterrestrictionsmetadata"></a>TableFilterRestrictionsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Filtern|boolescher Wert|Nein |
|nonFilterableProperties|Array|Nein |
|requiredProperties|Array|Nein |



### <a name="optionsemailsubscription"></a>OptionsEmailSubscription


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|NotificationUrl|Zeichenfolge|Nein |
|Nachricht|nicht definiert|Nein |



### <a name="messagewithoptions"></a>MessageWithOptions


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Betreff|Zeichenfolge|Ja |
|Optionen|Zeichenfolge|Ja |
|Körper|Zeichenfolge|Nein |
|Bedeutung|Zeichenfolge|Nein |
|Anlagen|Array|Nein |
|An|Zeichenfolge|Ja |



### <a name="subscriptionresponse"></a>SubscriptionResponse


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|Ressource|Zeichenfolge|Nein |
|notificationType|Zeichenfolge|Nein |
|notificationUrl|Zeichenfolge|Nein |



### <a name="approvalemailsubscription"></a>ApprovalEmailSubscription


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|NotificationUrl|Zeichenfolge|Nein |
|Nachricht|nicht definiert|Nein |



### <a name="approvalmessage"></a>ApprovalMessage


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Betreff|Zeichenfolge|Ja |
|Optionen|Zeichenfolge|Ja |
|Körper|Zeichenfolge|Nein |
|Bedeutung|Zeichenfolge|Nein |
|Anlagen|Array|Nein |
|An|Zeichenfolge|Ja |



### <a name="approvalemailresponse"></a>ApprovalEmailResponse


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Winkelseite|Zeichenfolge|Nein |



### <a name="tableslist"></a>TablesList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Array|Nein |



### <a name="table"></a>Tabelle


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Name|Zeichenfolge|Nein |
|DisplayName|Zeichenfolge|Nein |



### <a name="item"></a>Artikel


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ItemInternalId|Zeichenfolge|Nein |



### <a name="calendaritemslist"></a>CalendarItemsList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Array|Nein |



### <a name="calendaritem"></a>CalendarItem


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ItemInternalId|Zeichenfolge|Nein |



### <a name="contactitemslist"></a>ContactItemsList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Array|Nein |



### <a name="contactitem"></a>ContactItem


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ItemInternalId|Zeichenfolge|Nein |



### <a name="datasetslist"></a>DataSetsList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Array|Nein |



### <a name="dataset"></a>DataSet


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Name|Zeichenfolge|Nein |
|DisplayName|Zeichenfolge|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)