<properties
    pageTitle="Office 365 Outlook Connector in Ihren Apps Logik hinzufügen | Microsoft Azure"
    description="Erstellen von Logik apps mit Office 365 Interaktion mit Office 365. Beispiel: erstellen, bearbeiten und Aktualisieren von Kontakten und Kalenderelementen."
    services=""    
    documentationCenter=""     
    authors="MandiOhlinger"    
    manager="anneta"    
    editor="" 
    tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="10/18/2016"
ms.author="mandia"/>

# <a name="get-started-with-the-office-365-outlook-connector"></a>Erste Schritte mit Office 365 Outlook connector 

Office 365 Outlook Connector ermöglicht die Interaktion mit Outlook in Office 365. Verwenden Sie diesen Connector erstellen, bearbeiten und Aktualisieren von Kontakten und Kalenderelementen, auch, senden und Antworten auf e-Mail.

Mit Office 365 Outlook Sie:

- Erstellen Sie den Workflow mit e-Mail- und Kalender-Funktionen in Office 365. 
- Verwenden Sie Trigger den Workflow starten, wird eine neue e-Mail ein Kalenderelement aktualisiert wird und mehr.
- Verwenden Sie e-Mail, erstellen Sie ein neues Kalenderereignis und weitere Aktionen. Wird ein neues Objekt in Salesforce (Trigger), senden Sie eine e-Mail an Ihre Office 365 Outlook (Aktion) fest. 

Dieses Thema zeigt, wie eine Anwendung Logik mit Office 365 Outlook Connector und listet auch Auslöser und Aktionen.

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik Apps allgemein verfügbar (GA).

Weitere Logik Apps sehen Sie [Was Logik apps](../app-service-logic/app-service-logic-what-are-logic-apps.md) und [Logik app erstellen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-office-365"></a>Verbinden mit Office 365

Bevor Ihre Anwendung Logik einen Dienst zugreifen kann, erstellen Sie zuerst eine *Verbindung* zum Dienst. Eine Verbindung stellt eine Verbindung zwischen einer Anwendung Logik und ein weiterer. Beispielsweise benötigen Verbindung zu Office 365 Outlook Sie zuerst ein Office 365- *Verbindung*. Geben Sie zum Erstellen einer Verbindung die Anmeldeinformationen verwenden Sie normalerweise den Zugriff auf Dienste, die Sie herstellen möchten. Geben Sie die Anmeldeinformationen so mit Office 365 Outlook Ihre Office 365-Konto zum Herstellen die Verbindung.


## <a name="create-the-connection"></a>Erstellen der Verbindung

>[AZURE.INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]

## <a name="use-a-trigger"></a>Verwenden eines Triggers

Ein Trigger ist ein Ereignis, das zum Starten des Workflows definiert eine Logik-App verwendet werden kann. Trigger Abfragen"" den Service, Intervall und Häufigkeit. [Weitere Informationen zu Triggern](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Geben Sie der Anwendung Logik "Office 365", um eine Liste der Trigger:  

    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)

2. Wählen Sie **Office 365 Outlook - bald Beginn eine Veranstaltung**. Wenn bereits eine Verbindung vorhanden ist, wählen Sie einen Kalender aus der Dropdown-Liste aus.

    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)

    Wenn Sie aufgefordert werden, sich geben Sie die Zeichen Details zum Herstellen die Verbindung. [Erstellen der Verbindung](connectors-create-api-office365-outlook.md#create-the-connection) in diesem Thema sind die Schritte aufgeführt. 

    > [AZURE.NOTE] In diesem Beispiel führt die Anwendung Logik aktualisiert ein Kalenderereignis. Um diese Trigger zu erhalten, fügen Sie eine andere Aktion hinzu, die sendet eine Textnachricht Beispielsweise Aktion Twilio *Senden* , Texte hinzufügen, wenn das Ereignis in 15 Minuten gestartet wird. 

3. Wählen Sie die Schaltfläche **Bearbeiten** und legen Sie die **Häufigkeit** und **Intervall** . Beispielsweise soll den Trigger Abfragen alle 15 Minuten **Häufigkeit** auf **Minuten**festgelegt, und legen Sie das **Intervall** **15**. 

    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)

4. **Speichern Sie** die geänderte (obere linke Ecke der Symbolleiste). Ihre Anwendung Logik wird gespeichert und werden automatisch aktiviert.


## <a name="use-an-action"></a>Verwenden Sie eine Aktion

Eine Aktion ist eine Operation in eine Logik-app definierten Workflow durchgeführt. [Weitere Informationen zu Aktionen](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Wählen Sie das Pluszeichen. Sie sehen verschiedene Optionen: **eine Aktion hinzufügen**, **Hinzufügen einer Bedingung**oder eine **Weitere** Optionen.

    ![](./media/connectors-create-api-office365-outlook/add-action.png)

2. Wählen Sie **eine Aktion hinzufügen**.

3. Geben Sie im Textfeld "Office 365", um eine Liste der verfügbaren Aktionen.

    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 

4. Wählen Sie in unserem Beispiel **Office 365 Outlook - Kontakt erstellen**. Wenn bereits eine Verbindung vorhanden ist, wählen Sie die **Ordner-ID** **angegebenen Namen**und andere Eigenschaften:  

    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)

    Wenn die Verbindungsinformationen aufgefordert werden, geben Sie die Details der Verbindung. [Erstellen der Verbindung](connectors-create-api-office365-outlook.md#create-the-connection) in diesem Thema werden diese Eigenschaften beschrieben. 

    > [AZURE.NOTE] In diesem Beispiel erstellen wir einen neuen Kontakt in Outlook von Office 365. Ausgabe von einem anderen Trigger können Sie den Kontakt erstellen. Z. B. Trigger SalesForce *beim Erstellen eines Objekts* hinzufügen. Fügen Sie die Aktion Office 365 Outlook- *Kontakt erstellen* , die SalesForce-Felder verwendet, um den neuen neuen Kontakt in Office 365 erstellen. 

5. **Speichern Sie** die geänderte (obere linke Ecke der Symbolleiste). Ihre Anwendung Logik wird gespeichert und werden automatisch aktiviert.


## <a name="technical-details"></a>Technische Details

Hier werden Einzelheiten über Trigger, Aktionen und Reaktionen, die diese Verbindung unterstützt:

## <a name="office-365-triggers"></a>Office 365-Trigger

|Trigger | Beschreibung|
|--- | ---|
|[Wenn ein bevorstehendes Ereignis schnell starten](connectors-create-api-office365-outlook.md#when-an-upcoming-event-is-starting-soon)|Dieser Vorgang löst einen Beginn einer bevorstehenden Ereignis.|
|[Beim Eintreffen einer neuen e-Mails](connectors-create-api-office365-outlook.md#when-a-new-email-arrives)|Dieser Vorgang löst einen beim Eintreffen einer neuen e-Mails|
|[Erstellung ein neues Ereignisses](connectors-create-api-office365-outlook.md#when-a-new-event-is-created)|Dieser Vorgang löst einen beim Erstellen ein neues Ereignisses in einem Kalender.|
|[Wenn ein Ereignis geändert wird.](connectors-create-api-office365-outlook.md#when-an-event-is-modified)|Dieser Vorgang löst einen, wenn ein Ereignis in einem Kalender geändert wird.|


## <a name="office-365-actions"></a>Office 365-Aktionen

|Aktion|Beschreibung|
|--- | ---|
|[E-Mails](connectors-create-api-office365-outlook.md#get-emails)|Dieser Vorgang ruft e-Mails in einem Ordner ab.|
|[Senden Sie eine e-Mail](connectors-create-api-office365-outlook.md#send-an-email)|Dieser Vorgang sendet eine e-Mail-Nachricht.|
|[E-Mail löschen](connectors-create-api-office365-outlook.md#delete-email)|Dieser Vorgang löscht eine e-Mail ID.|
|[Als gelesen markieren](connectors-create-api-office365-outlook.md#mark-as-read)|Dieser Vorgang wird eine e-Mail als gelesen markiert.|
|[E-Mail beantworten](connectors-create-api-office365-outlook.md#reply-to-email)|Dieser Vorgang auf eine e-Mail antwortet.|
|[Anlagen abrufen](connectors-create-api-office365-outlook.md#get-attachment)|Mit diesem Vorgang wird eine e-Mail-Anlage nach Id.|
|[E-Mail mit Optionen](connectors-create-api-office365-outlook.md#send-email-with-options)|Dieser Vorgang sendet eine e-Mail mit mehreren Optionen und wartet, bis der Empfänger antwortet mit einer der Optionen.|
|[E-Mail-Genehmigung](connectors-create-api-office365-outlook.md#send-approval-email)|Dieser Vorgang sendet eine e-Mail mit der Genehmigung und wartet auf eine Antwort vom Empfänger.|
|[Abrufen von Kalendern](connectors-create-api-office365-outlook.md#get-calendars)|Dieser Vorgang führt verfügbaren Kalender.|
|[Ereignisse](connectors-create-api-office365-outlook.md#get-events)|Dieser Vorgang ruft Ereignisse in einem Kalender.|
|[Ereignis erstellen](connectors-create-api-office365-outlook.md#create-event)|Dieser Vorgang erstellt ein neues Ereignis in einem Kalender.|
|[Ereignis](connectors-create-api-office365-outlook.md#get-event)|Dieser Vorgang ruft ein bestimmtes Ereignis in einem Kalender.|
|[Ereignis löschen](connectors-create-api-office365-outlook.md#delete-event)|Dieser Vorgang löscht ein Ereignis in einem Kalender.|
|[Update-Ereignis](connectors-create-api-office365-outlook.md#update-event)|Dieser Vorgang aktualisiert ein Ereignis in einem Kalender.|
|[Kontakt Ordner](connectors-create-api-office365-outlook.md#get-contact-folders)|Dieser Vorgang führt verfügbaren Kontakte-Ordner.|
|[Kontakte](connectors-create-api-office365-outlook.md#get-contacts)|Dieser Vorgang ruft Kontakte aus einem Kontakteordner.|
|[Kontakt erstellen](connectors-create-api-office365-outlook.md#create-contact)|Dieser Vorgang erstellt einen neuen Kontakt im Kontakteordner.|
|[Kontakt](connectors-create-api-office365-outlook.md#get-contact)|Dieser Vorgang Ruft einen bestimmten Kontakt in einem Kontakteordner.|
|[Kontakt löschen](connectors-create-api-office365-outlook.md#delete-contact)|Dieser Vorgang löscht einen Kontakt in einem Kontakteordner.|
|[Kontakt aktualisieren](connectors-create-api-office365-outlook.md#update-contact)|Dieser Vorgang aktualisiert einen Kontakt im Kontakteordner.|

### <a name="trigger-and-action-details"></a>Trigger und die Aktion

Finden Sie in diesem Abschnitt Einzelheiten zu jeder Trigger Maßnahmen erforderlichen oder optionalen Eingaben Eigenschaften und entsprechende Ausgabe der Connector zugeordnet.

#### <a name="when-an-upcoming-event-is-starting-soon"></a>Wenn ein bevorstehendes Ereignis schnell starten
Dieser Vorgang löst einen Beginn einer bevorstehenden Ereignis. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Kalender-id|Eindeutiger Bezeichner des Kalenders|
|lookAheadTimeInMinutes|Vorschauzeit|Zeit (in Minuten) für Events blicken|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
CalendarItemsList: Die Liste der Elemente im Kalender

| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|Wert|Array|Liste der Elemente im Kalender|


#### <a name="get-emails"></a>E-Mails
Dieser Vorgang ruft e-Mails in einem Ordner ab. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|folderPath|Pfad|Pfad des Ordners e-Mails abrufen (Standard: 'Posteingang')|
|Nach oben|Nach oben|Anzahl der e-Mails abrufen (Standard: 10)|
|fetchOnlyUnread|Nur ungelesene Nachrichten abrufen|Abrufen nur ungelesene e-Mails?|
|includeAttachments|Anlagen|Wenn auf true Anlagen auch mit der e-Mail abgerufen werden|
|Einfach|Suchabfrage|Suchabfrage e-Mails filtern|
|Überspringen|Überspringen|Anzahl der e-Mails überspringen (Standard: 0)|
|skipToken|Skip-Token|Token Abrufen neuer Seite überspringen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
ReceiveMessage: E-Mail empfangen

| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|Von|Zeichenfolge|Von|
|An|Zeichenfolge|An|
|Betreff|Zeichenfolge|Betreff|
|Körper|Zeichenfolge|Körper|
|Bedeutung|Zeichenfolge|Bedeutung|
|HasAttachment|boolescher Wert|Anlage|
|ID|Zeichenfolge|Nachrichten-Id|
|IsRead|boolescher Wert|Wird gelesen|
|DateTimeReceived|Zeichenfolge|Datum Uhrzeit empfangen|
|Anlagen|Array|Anlagen|
|Cc|Zeichenfolge|Adressen Sie e-Mail-getrennt durch Semikolons wiesomeone@contoso.com|
|Bcc|Zeichenfolge|Adressen Sie e-Mail-getrennt durch Semikolons wiesomeone@contoso.com|
|IsHtml|boolescher Wert|HTML|


#### <a name="send-an-email"></a>Senden Sie eine e-Mail
Dieser Vorgang sendet eine e-Mail-Nachricht. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|EmailMessage *|E-Mail|E-Mail|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine.

#### <a name="delete-email"></a>E-Mail löschen
Dieser Vorgang löscht eine e-Mail ID. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|MessageId *|Nachrichten-Id|ID der e-Mail löschen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine.

#### <a name="mark-as-read"></a>Als gelesen markieren
Dieser Vorgang wird eine e-Mail als gelesen markiert. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|MessageId *|Nachrichten-Id|ID der e-Mail als gekennzeichnet werden gelesen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine.


#### <a name="reply-to-email"></a>E-Mail beantworten
Dieser Vorgang auf eine e-Mail antwortet. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|MessageId *|Nachrichten-Id|ID der e-Mail beantworten|
|Kommentar *|Kommentar|Bewertungskommentar|
|replyAll|Allen Antworten|Allen Empfängern Antworten|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine.


#### <a name="get-attachment"></a>Anlagen abrufen
Mit diesem Vorgang wird eine e-Mail-Anlage nach Id. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|MessageId *|Nachrichten-Id|ID der e-Mail|
|AttachmentId *|Anlage-Id|ID der Anlage herunterladen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine.


#### <a name="when-a-new-email-arrives"></a>Beim Eintreffen einer neuen e-Mails
Dieser Vorgang löst einen beim Eintreffen einer neuen e-Mails.

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|folderPath|Pfad|E-Mail-Ordner abrufen (Standard: Posteingang)|
|An|An|Empfänger e-Mail-Adressen|
|Von|Von|Absenderadresse|
|Bedeutung|Bedeutung|Bedeutung von e-Mails (hoch, Normal, Niedrig) (Standard: Normal)|
|fetchOnlyWithAttachment|Anlagen|Nur e-Mail-Nachrichten mit einer Anlage abrufen|
|includeAttachments|Anlagen|Anlagen|
|subjectFilter|Betreff filtern|Die Zeichenfolge, die in der Betreffzeile suchen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
TriggerBatchResponse [ReceiveMessage]

| Eigenschaftenname | Datentyp |
|---|---|
|Wert|Array|


#### <a name="send-email-with-options"></a>E-Mail mit Optionen
Dieser Vorgang sendet eine e-Mail mit mehreren Optionen und wartet, bis der Empfänger antwortet mit einer der Optionen. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|OptionsEmailSubscription *|Abonnementanforderung für die e-Mail-Optionen|Abonnementanforderung für die e-Mail-Optionen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
SubscriptionResponse: Modell für Genehmigung e-Mail-Abonnement

| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|ID|Zeichenfolge|Die Kennung des Dauerauftrags|
|Ressource|Zeichenfolge|Ressource der abonnementanforderung|
|notificationType|Zeichenfolge|Benachrichtigungstyp|
|notificationUrl|Zeichenfolge|Url für Benachrichtigung|


#### <a name="send-approval-email"></a>E-Mail-Genehmigung
Dieser Vorgang sendet eine e-Mail mit der Genehmigung und wartet auf eine Antwort vom Empfänger. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|ApprovalEmailSubscription *|Abonnementanforderung für e-Mail-Genehmigung|Abonnementanforderung für e-Mail-Genehmigung|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
SubscriptionResponse: Modell für Genehmigung e-Mail-Abonnement

| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|ID|Zeichenfolge|Die Kennung des Dauerauftrags|
|Ressource|Zeichenfolge|Ressource der abonnementanforderung|
|notificationType|Zeichenfolge|Benachrichtigungstyp|
|notificationUrl|Zeichenfolge|Url für Benachrichtigung|


#### <a name="get-calendars"></a>Abrufen von Kalendern
Dieser Vorgang führt verfügbaren Kalender. 

Es sind keine Parameter für diesen Aufruf.

##### <a name="output-details"></a>Ausgabedetails
TablesList

| Eigenschaftenname | Datentyp |
|---|---|
|Wert|Array|


#### <a name="get-events"></a>Ereignisse
Dieser Vorgang ruft Ereignisse in einem Kalender. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Kalender-id|Wählen Sie einen Kalender|
|$filter|Filterabfrage|Eine Filterabfrage ODATA zurückgegebenen Einträge beschränken|
|$orderby|Sortieren nach|Ein ODATA OrderBy-Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|Anzahl der Durchläufe|Anzahl der Einträge zu überspringen (Standard = 0)|
|$top|Maximale Get Count|Maximale Anzahl Einträge abrufen (Standard = 256)|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
CalendarEventList: Die Liste der Elemente im Kalender

| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|Wert|Array|Liste der Elemente im Kalender|


#### <a name="create-event"></a>Ereignis erstellen
Dieser Vorgang erstellt ein neues Ereignis in einem Kalender. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Kalender-id|Wählen Sie einen Kalender|
|Artikel *|Artikel|Ereignis erstellen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
CalendarEvent: Connector spezifischen Kalender Modell Ereignisklasse.

| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|ID|Zeichenfolge|Eindeutiger Bezeichner des Ereignisses.|
|Teilnehmer|Array|Liste der Teilnehmer für dieses Ereignis.|
|Körper|nicht definiert|Der Hauptteil der Nachricht mit dem Ereignis verknüpft.|
|BodyPreview|Zeichenfolge|Die Vorschau der Nachricht mit dem Ereignis verknüpft.|
|Kategorien|Array|Die Kategorien, die dem Ereignis zugeordnet.|
|ChangeKey|Zeichenfolge|Gibt die Version des Ereignisobjekts. Jedes Mal, wenn das Ereignis geändert wird, ändert ChangeKey.|
|DateTimeCreated|Zeichenfolge|Das Datum und die Uhrzeit, zu der das Ereignis erstellt wurde.|
|DateTimeLastModified|Zeichenfolge|Datum und Uhrzeit der letzten Änderung die Veranstaltung.|
|Ende|Zeichenfolge|Die Endzeit des Ereignisses.|
|EndTimeZone|Zeichenfolge|Gibt die Zeitzone der Sitzung Endzeit. Dieser Wert muss in Windows definiert sein (Beispiel: "Pacific Standard Time").|
|HasAttachments|boolescher Wert|Legen Sie auf true, wenn das Ereignis Anlagen hat.|
|Bedeutung|Zeichenfolge|Die Wichtigkeit des Ereignisses: niedrig, Normal oder hoch.|
|IsAllDay|boolescher Wert|Legen Sie auf true, wenn das Ereignis den ganzen Tag dauert.|
|IsCancelled|boolescher Wert|Auf true festgelegt, wenn das Ereignis abgebrochen wurde.|
|IsOrganizer|boolescher Wert|Legen Sie auf true, wenn der Absender auch Organizer.|
|Speicherort|nicht definiert|Der Speicherort des Ereignisses.|
|Organizer|nicht definiert|Der Organisator des Ereignisses.|
|Serie|nicht definiert|Das Serienmuster für das Ereignis.|
|Erinnerung|ganze Zahl|Zeit in Minuten, bevor Sie starten an.|
|ResponseRequested|boolescher Wert|Legen Sie auf true, wenn der Absender eine Antwort erhalten möchte, wenn das Ereignis akzeptiert oder abgelehnt wird.|
|ResponseStatus|nicht definiert|Gibt die Art der Reaktion auf eine Meldung gesendet.|
|SeriesMasterId|Zeichenfolge|Eindeutiger Bezeichner für Serie Master Ereignistyp.|
|ShowAs|Zeichenfolge|Frei oder gebucht sind.|
|Starten|Zeichenfolge|Die Startzeit des Ereignisses.|
|StartTimeZone|Zeichenfolge|Gibt die Startzeit der Besprechung Zone Zeit. Dieser Wert muss in Windows definiert sein (Beispiel: "Pacific Standard Time").|
|Betreff|Zeichenfolge|Betreff für ein Ereignis.|
|Typ|Zeichenfolge|Der Ereignistyp: Instanz, vorkommen, Ausnahme oder Serie Master.|
|WebLink|Zeichenfolge|Die Vorschau der Nachricht mit dem Ereignis verknüpft.|


#### <a name="get-event"></a>Ereignis
Dieser Vorgang ruft ein bestimmtes Ereignis in einem Kalender. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Kalender-id|Wählen Sie einen Kalender|
|ID *|Artikelnummer|Wählen Sie ein Ereignis|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
CalendarEvent: Connector spezifischen Kalender Modell Ereignisklasse.

| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|ID|Zeichenfolge|Eindeutiger Bezeichner des Ereignisses.|
|Teilnehmer|Array|Liste der Teilnehmer für dieses Ereignis.|
|Körper|nicht definiert|Der Hauptteil der Nachricht mit dem Ereignis verknüpft.|
|BodyPreview|Zeichenfolge|Die Vorschau der Nachricht mit dem Ereignis verknüpft.|
|Kategorien|Array|Die Kategorien, die dem Ereignis zugeordnet.|
|ChangeKey|Zeichenfolge|Gibt die Version des Ereignisobjekts. Jedes Mal, wenn das Ereignis geändert wird, ändert ChangeKey.|
|DateTimeCreated|Zeichenfolge|Das Datum und die Uhrzeit, zu der das Ereignis erstellt wurde.|
|DateTimeLastModified|Zeichenfolge|Datum und Uhrzeit der letzten Änderung die Veranstaltung.|
|Ende|Zeichenfolge|Die Endzeit des Ereignisses.|
|EndTimeZone|Zeichenfolge|Gibt die Zeitzone der Sitzung Endzeit. Dieser Wert muss in Windows definiert sein (Beispiel: "Pacific Standard Time").|
|HasAttachments|boolescher Wert|Legen Sie auf true, wenn das Ereignis Anlagen hat.|
|Bedeutung|Zeichenfolge|Die Wichtigkeit des Ereignisses: niedrig, Normal oder hoch.|
|IsAllDay|boolescher Wert|Legen Sie auf true, wenn das Ereignis den ganzen Tag dauert.|
|IsCancelled|boolescher Wert|Auf true festgelegt, wenn das Ereignis abgebrochen wurde.|
|IsOrganizer|boolescher Wert|Legen Sie auf true, wenn der Absender auch Organizer.|
|Speicherort|nicht definiert|Der Speicherort des Ereignisses.|
|Organizer|nicht definiert|Der Organisator des Ereignisses.|
|Serie|nicht definiert|Das Serienmuster für das Ereignis.|
|Erinnerung|ganze Zahl|Zeit in Minuten, bevor Sie starten an.|
|ResponseRequested|boolescher Wert|Legen Sie auf true, wenn der Absender eine Antwort erhalten möchte, wenn das Ereignis akzeptiert oder abgelehnt wird.|
|ResponseStatus|nicht definiert|Gibt die Art der Reaktion auf eine Meldung gesendet.|
|SeriesMasterId|Zeichenfolge|Eindeutiger Bezeichner für Serie Master Ereignistyp.|
|ShowAs|Zeichenfolge|Frei oder gebucht sind.|
|Starten|Zeichenfolge|Die Startzeit des Ereignisses.|
|StartTimeZone|Zeichenfolge|Gibt die Startzeit der Besprechung Zone Zeit. Dieser Wert muss in Windows definiert sein (Beispiel: "Pacific Standard Time").|
|Betreff|Zeichenfolge|Betreff für ein Ereignis.|
|Typ|Zeichenfolge|Der Ereignistyp: Instanz, vorkommen, Ausnahme oder Serie Master.|
|WebLink|Zeichenfolge|Die Vorschau der Nachricht mit dem Ereignis verknüpft.|


#### <a name="delete-event"></a>Ereignis löschen
Dieser Vorgang löscht ein Ereignis in einem Kalender. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Kalender-id|Wählen Sie einen Kalender|
|ID *|ID|Wählen Sie ein Ereignis|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine.


#### <a name="update-event"></a>Update-Ereignis
Dieser Vorgang aktualisiert ein Ereignis in einem Kalender. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Kalender-id|Wählen Sie einen Kalender|
|ID *|ID|Wählen Sie ein Ereignis|
|Artikel *|Artikel|Ereignis aktualisieren|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
CalendarEvent: Connector spezifischen Kalender Modell Ereignisklasse.

| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|ID|Zeichenfolge|Eindeutiger Bezeichner des Ereignisses.|
|Teilnehmer|Array|Liste der Teilnehmer für dieses Ereignis.|
|Körper|nicht definiert|Der Hauptteil der Nachricht mit dem Ereignis verknüpft.|
|BodyPreview|Zeichenfolge|Die Vorschau der Nachricht mit dem Ereignis verknüpft.|
|Kategorien|Array|Die Kategorien, die dem Ereignis zugeordnet.|
|ChangeKey|Zeichenfolge|Gibt die Version des Ereignisobjekts. Jedes Mal, wenn das Ereignis geändert wird, ändert ChangeKey.|
|DateTimeCreated|Zeichenfolge|Das Datum und die Uhrzeit, zu der das Ereignis erstellt wurde.|
|DateTimeLastModified|Zeichenfolge|Datum und Uhrzeit der letzten Änderung die Veranstaltung.|
|Ende|Zeichenfolge|Die Endzeit des Ereignisses.|
|EndTimeZone|Zeichenfolge|Gibt die Zeitzone der Sitzung Endzeit. Dieser Wert muss in Windows definiert sein (Beispiel: "Pacific Standard Time").|
|HasAttachments|boolescher Wert|Legen Sie auf true, wenn das Ereignis Anlagen hat.|
|Bedeutung|Zeichenfolge|Die Wichtigkeit des Ereignisses: niedrig, Normal oder hoch.|
|IsAllDay|boolescher Wert|Legen Sie auf true, wenn das Ereignis den ganzen Tag dauert.|
|IsCancelled|boolescher Wert|Auf true festgelegt, wenn das Ereignis abgebrochen wurde.|
|IsOrganizer|boolescher Wert|Legen Sie auf true, wenn der Absender auch Organizer.|
|Speicherort|nicht definiert|Der Speicherort des Ereignisses.|
|Organizer|nicht definiert|Der Organisator des Ereignisses.|
|Serie|nicht definiert|Das Serienmuster für das Ereignis.|
|Erinnerung|ganze Zahl|Zeit in Minuten, bevor Sie starten an.|
|ResponseRequested|boolescher Wert|Legen Sie auf true, wenn der Absender eine Antwort erhalten möchte, wenn das Ereignis akzeptiert oder abgelehnt wird.|
|ResponseStatus|nicht definiert|Gibt die Art der Reaktion auf eine Meldung gesendet.|
|SeriesMasterId|Zeichenfolge|Eindeutiger Bezeichner für Serie Master Ereignistyp.|
|ShowAs|Zeichenfolge|Frei oder gebucht sind.|
|Starten|Zeichenfolge|Die Startzeit des Ereignisses.|
|StartTimeZone|Zeichenfolge|Gibt die Startzeit der Besprechung Zone Zeit. Dieser Wert muss in Windows definiert sein (Beispiel: "Pacific Standard Time").|
|Betreff|Zeichenfolge|Betreff für ein Ereignis.|
|Typ|Zeichenfolge|Der Ereignistyp: Instanz, vorkommen, Ausnahme oder Serie Master.|
|WebLink|Zeichenfolge|Die Vorschau der Nachricht mit dem Ereignis verknüpft.|


#### <a name="when-a-new-event-is-created"></a>Erstellung ein neues Ereignisses
Dieser Vorgang löst einen beim Erstellen ein neues Ereignisses in einem Kalender. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Kalender-id|Wählen Sie einen Kalender|
|$filter|Filterabfrage|Eine Filterabfrage ODATA zurückgegebenen Einträge beschränken|
|$orderby|Sortieren nach|Ein ODATA OrderBy-Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|Anzahl der Durchläufe|Anzahl der Einträge zu überspringen (Standard = 0)|
|$top|Maximale Get Count|Maximale Anzahl Einträge abrufen (Standard = 256)|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
CalendarItemsList: Die Liste der Elemente im Kalender

| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|Wert|Array|Liste der Elemente im Kalender|


#### <a name="when-an-event-is-modified"></a>Wenn ein Ereignis geändert wird.
Dieser Vorgang löst einen, wenn ein Ereignis in einem Kalender geändert wird. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Kalender-id|Wählen Sie einen Kalender|
|$filter|Filterabfrage|Eine Filterabfrage ODATA zurückgegebenen Einträge beschränken|
|$orderby|Sortieren nach|Ein ODATA OrderBy-Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|Anzahl der Durchläufe|Anzahl der Einträge zu überspringen (Standard = 0)|
|$top|Maximale Get Count|Maximale Anzahl Einträge abrufen (Standard = 256)|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
CalendarItemsList: Die Liste der Elemente im Kalender


| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|Wert|Array|Liste der Elemente im Kalender|


#### <a name="get-contact-folders"></a>Kontakt Ordner
Dieser Vorgang führt verfügbaren Kontakte-Ordner. 

Es sind keine Parameter für diesen Aufruf.

##### <a name="output-details"></a>Ausgabedetails
TablesList

| Eigenschaftenname | Datentyp |
|---|---|
|Wert|Array|


#### <a name="get-contacts"></a>Kontakte
Dieser Vorgang ruft Kontakte aus einem Kontakteordner. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Ordner-id|Eindeutiger Bezeichner des Ordners Kontakte abrufen|
|$filter|Filterabfrage|Eine Filterabfrage ODATA zurückgegebenen Einträge beschränken|
|$orderby|Sortieren nach|Ein ODATA OrderBy-Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|Anzahl der Durchläufe|Anzahl der Einträge zu überspringen (Standard = 0)|
|$top|Maximale Get Count|Maximale Anzahl Einträge abrufen (Standard = 256)|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Kontaktliste: Die Liste der Kontakte

| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|Wert|Array|Liste der Kontakte|


#### <a name="create-contact"></a>Kontakt erstellen
Dieser Vorgang erstellt einen neuen Kontakt im Kontakteordner. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Ordner-id|Wählen Sie einen Kontakteordner|
|Artikel *|Artikel|Kontakt erstellen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Kontakt: Kontakt

| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|ID|Zeichenfolge|Eindeutiger Bezeichner des Kontakts.|
|ParentFolderId|Zeichenfolge|Die ID des übergeordneten Ordners des Kontakts|
|Geburtstag|Zeichenfolge|Das Geburtsdatum des Kontakts.|
|Speichern|Zeichenfolge|Der Name des Kontakts wird unter abgelegt.|
|DisplayName|Zeichenfolge|Anzeigename des Kontakts.|
|Vorname|Zeichenfolge|Vorname des Kontakts.|
|Initialen|Zeichenfolge|Die Initialen der Kontaktperson ein.|
|MiddleName|Zeichenfolge|Vorname des Kontakts.|
|Spitzname|Zeichenfolge|Spitzname des Kontakts.|
|Nachname|Zeichenfolge|Nachname des Kontakts.|
|Titel|Zeichenfolge|Titel des Kontakts.|
|Generation|Zeichenfolge|Generierung des Kontakts.|
|Emailadressen|Array|Der Kontakt e-Mail-Adressen.|
|ImAddresses|Array|Der Kontakt instant messaging (IM) Adressen.|
|JobTitle|Zeichenfolge|Position des Kontakts.|
|Firmenname|Zeichenfolge|Der Name des Unternehmens des Kontakts.|
|Abteilung|Zeichenfolge|Die Abteilung des Kontakts.|
|OfficeLocation|Zeichenfolge|Die Position des Kontakts Office.|
|Beruf|Zeichenfolge|Der Beruf des Kontakts.|
|BusinessHomePage|Zeichenfolge|Die Business-Homepage des Kontaktes.|
|AssistantName|Zeichenfolge|Der Name des Assistenten des Kontakts.|
|Manager|Zeichenfolge|Der Name des Vorgesetzten des Kontakts.|
|HomePhones|Array|Privaten Telefonnummern des Kontakts.|
|BusinessPhones|Array|Geschäftlichen Telefonnummer des Kontakts|
|MobilePhone1|Zeichenfolge|Die Mobiltelefonnummer der Kontaktperson.|
|HomeAddress|nicht definiert|Privatadresse des Kontakts.|
|BusinessAddress|nicht definiert|Geschäftsadresse für den Kontakt.|
|OtherAddress|nicht definiert|Andere Adressen für den Kontakt.|
|YomiCompanyName|Zeichenfolge|Phonetische Japanisch Firma des Kontakts.|
|YomiGivenName|Zeichenfolge|Der phonetische japanische angegebene Name (Vorname) des Kontakts.|
|YomiSurname|Zeichenfolge|Phonetische Japanisch Name (Nachname) des Kontakts|
|Kategorien|Array|Die Kategorien, die dem Kontakt zugeordnet.|
|ChangeKey|Zeichenfolge|Gibt die Version des Event-Objekts|
|DateTimeCreated|Zeichenfolge|Die Uhrzeit der Erstellung des Kontakts.|
|DateTimeLastModified|Zeichenfolge|Die Uhrzeit, die Änderung des Kontakts.|


#### <a name="get-contact"></a>Kontakt
Dieser Vorgang Ruft einen bestimmten Kontakt in einem Kontakteordner. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Ordner-id|Wählen Sie einen Kontakteordner|
|ID *|Artikelnummer|Eindeutiger Bezeichner des Kontakts abrufen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Kontakt: Kontakt

| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|ID|Zeichenfolge|Eindeutiger Bezeichner des Kontakts.|
|ParentFolderId|Zeichenfolge|Die ID des übergeordneten Ordners des Kontakts|
|Geburtstag|Zeichenfolge|Das Geburtsdatum des Kontakts.|
|Speichern|Zeichenfolge|Der Name des Kontakts wird unter abgelegt.|
|DisplayName|Zeichenfolge|Anzeigename des Kontakts.|
|Vorname|Zeichenfolge|Vorname des Kontakts.|
|Initialen|Zeichenfolge|Die Initialen der Kontaktperson ein.|
|MiddleName|Zeichenfolge|Vorname des Kontakts.|
|Spitzname|Zeichenfolge|Spitzname des Kontakts.|
|Nachname|Zeichenfolge|Nachname des Kontakts.|
|Titel|Zeichenfolge|Titel des Kontakts.|
|Generation|Zeichenfolge|Generierung des Kontakts.|
|Emailadressen|Array|Der Kontakt e-Mail-Adressen.|
|ImAddresses|Array|Der Kontakt instant messaging (IM) Adressen.|
|JobTitle|Zeichenfolge|Position des Kontakts.|
|Firmenname|Zeichenfolge|Der Name des Unternehmens des Kontakts.|
|Abteilung|Zeichenfolge|Die Abteilung des Kontakts.|
|OfficeLocation|Zeichenfolge|Die Position des Kontakts Office.|
|Beruf|Zeichenfolge|Der Beruf des Kontakts.|
|BusinessHomePage|Zeichenfolge|Die Business-Homepage des Kontaktes.|
|AssistantName|Zeichenfolge|Der Name des Assistenten des Kontakts.|
|Manager|Zeichenfolge|Der Name des Vorgesetzten des Kontakts.|
|HomePhones|Array|Privaten Telefonnummern des Kontakts.|
|BusinessPhones|Array|Geschäftlichen Telefonnummer des Kontakts|
|MobilePhone1|Zeichenfolge|Die Mobiltelefonnummer der Kontaktperson.|
|HomeAddress|nicht definiert|Privatadresse des Kontakts.|
|BusinessAddress|nicht definiert|Geschäftsadresse für den Kontakt.|
|OtherAddress|nicht definiert|Andere Adressen für den Kontakt.|
|YomiCompanyName|Zeichenfolge|Phonetische Japanisch Firma des Kontakts.|
|YomiGivenName|Zeichenfolge|Der phonetische japanische angegebene Name (Vorname) des Kontakts.|
|YomiSurname|Zeichenfolge|Phonetische Japanisch Name (Nachname) des Kontakts|
|Kategorien|Array|Die Kategorien, die dem Kontakt zugeordnet.|
|ChangeKey|Zeichenfolge|Gibt die Version des Event-Objekts|
|DateTimeCreated|Zeichenfolge|Die Uhrzeit der Erstellung des Kontakts.|
|DateTimeLastModified|Zeichenfolge|Die Uhrzeit, die Änderung des Kontakts.|


#### <a name="delete-contact"></a>Kontakt löschen
Dieser Vorgang löscht einen Kontakt in einem Kontakteordner. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Ordner-id|Wählen Sie einen Kontakteordner|
|ID *|ID|Eindeutiger Bezeichner des Kontakts löschen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Keine.


#### <a name="update-contact"></a>Kontakt aktualisieren
Dieser Vorgang aktualisiert einen Kontakt im Kontakteordner. 

|Eigenschaftenname| Angezeigter Name|Beschreibung|
| ---|---|---|
|Tabelle *|Ordner-id|Wählen Sie einen Kontakteordner|
|ID *|ID|Eindeutiger Bezeichner des Kontakts aktualisieren|
|Artikel *|Artikel|Kontaktelement aktualisieren|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Ausgabedetails
Kontakt: Kontakt

| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|ID|Zeichenfolge|Eindeutiger Bezeichner des Kontakts.|
|ParentFolderId|Zeichenfolge|Die ID des übergeordneten Ordners des Kontakts|
|Geburtstag|Zeichenfolge|Das Geburtsdatum des Kontakts.|
|Speichern|Zeichenfolge|Der Name des Kontakts wird unter abgelegt.|
|DisplayName|Zeichenfolge|Anzeigename des Kontakts.|
|Vorname|Zeichenfolge|Vorname des Kontakts.|
|Initialen|Zeichenfolge|Die Initialen der Kontaktperson ein.|
|MiddleName|Zeichenfolge|Vorname des Kontakts.|
|Spitzname|Zeichenfolge|Spitzname des Kontakts.|
|Nachname|Zeichenfolge|Nachname des Kontakts.|
|Titel|Zeichenfolge|Titel des Kontakts.|
|Generation|Zeichenfolge|Generierung des Kontakts.|
|Emailadressen|Array|Der Kontakt e-Mail-Adressen.|
|ImAddresses|Array|Der Kontakt instant messaging (IM) Adressen.|
|JobTitle|Zeichenfolge|Position des Kontakts.|
|Firmenname|Zeichenfolge|Der Name des Unternehmens des Kontakts.|
|Abteilung|Zeichenfolge|Die Abteilung des Kontakts.|
|OfficeLocation|Zeichenfolge|Die Position des Kontakts Office.|
|Beruf|Zeichenfolge|Der Beruf des Kontakts.|
|BusinessHomePage|Zeichenfolge|Die Business-Homepage des Kontaktes.|
|AssistantName|Zeichenfolge|Der Name des Assistenten des Kontakts.|
|Manager|Zeichenfolge|Der Name des Vorgesetzten des Kontakts.|
|HomePhones|Array|Privaten Telefonnummern des Kontakts.|
|BusinessPhones|Array|Geschäftlichen Telefonnummer des Kontakts|
|MobilePhone1|Zeichenfolge|Die Mobiltelefonnummer der Kontaktperson.|
|HomeAddress|nicht definiert|Privatadresse des Kontakts.|
|BusinessAddress|nicht definiert|Geschäftsadresse für den Kontakt.|
|OtherAddress|nicht definiert|Andere Adressen für den Kontakt.|
|YomiCompanyName|Zeichenfolge|Phonetische Japanisch Firma des Kontakts.|
|YomiGivenName|Zeichenfolge|Der phonetische japanische angegebene Name (Vorname) des Kontakts.|
|YomiSurname|Zeichenfolge|Phonetische Japanisch Name (Nachname) des Kontakts|
|Kategorien|Array|Die Kategorien, die dem Kontakt zugeordnet.|
|ChangeKey|Zeichenfolge|Gibt die Version des Event-Objekts|
|DateTimeCreated|Zeichenfolge|Die Uhrzeit der Erstellung des Kontakts.|
|DateTimeLastModified|Zeichenfolge|Die Uhrzeit, die Änderung des Kontakts.|



## <a name="http-responses"></a>HTTP-Antworten

Aktionen und Auslöser oben können eine oder mehrere der folgenden HTTP-Statuscodes zurück: 

|Name|Beschreibung|
|---|---|
|200|Okay|
|202|Akzeptiert|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md). Durchsuchen der verfügbaren Connectors Logik Apps [APIs Liste](apis-list.md).