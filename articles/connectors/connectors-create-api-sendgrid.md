<properties
pageTitle="SendGrid | Microsoft Azure"
description="Erstellen von Logik apps mit Azure App. SendGrid Verbindungsanbieter können Sie e-Mail und Empfängerlisten verwalten."
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

# <a name="get-started-with-the-sendgrid-connector"></a>Erste Schritte mit SendGrid-Anschluss

SendGrid Verbindungsanbieter können Sie e-Mail und Empfängerlisten verwalten.

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2015-08-01-Vorschau-Schemaversion. 

Sie können jetzt eine Logik-app erstellen machen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

SendGrid-Connector kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen in JSON und XML-Daten. 

 SendGrid Konnektor hat die folgenden Aktionen zur Verfügung. Es gibt keine Trigger.

### <a name="sendgrid-actions"></a>SendGrid Aktionen
Sie können diese Aktionen durchführen:

|Aktion|Beschreibung|
|--- | ---|
|[SendEmail](connectors-create-api-sendgrid.md#sendemail)|Sendet eine e-Mail mit SendGrid-API (maximal 10.000 Empfänger)|
|[AddRecipientToList](connectors-create-api-sendgrid.md#addrecipienttolist)|Einen einzelnen Empfänger Empfängerliste hinzufügen|


## <a name="create-a-connection-to-sendgrid"></a>Erstellen einer Verbindung mit SendGrid
Um Logik apps mit SendGrid erstellen, müssen Sie zunächst eine **Verbindung** anschließend geben Sie die Details für die folgenden Eigenschaften: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|ApiKey|Ja|Die SendGrid-API-Schlüssel|
 

>[AZURE.INCLUDE [Steps to create a connection to SendGrid](../../includes/connectors-create-api-sendgrid.md)]

>[AZURE.TIP] Hierzu können Sie in anderen apps Logik.

Nach dem Erstellen der Verbindung können sie Aktionen ausführen und warten Sie in diesem Artikel beschriebenen Trigger.

## <a name="reference-for-sendgrid"></a>Referenz für SendGrid
Gilt für Version: 1.0

## <a name="sendemail"></a>SendEmail
E-Mail: sendet eine e-Mail mit SendGrid-API (maximal 10.000 Empfänger) 

```POST: /api/mail.send.json``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Anforderung| |Ja|Körper|keine|E-Mail senden|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|429|Zu viele Anforderung|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="addrecipienttolist"></a>AddRecipientToList
Empfänger hinzufügen: einen einzelnen Empfänger zu einer Empfängerliste hinzufügen 

```POST: /v3/contactdb/lists/{listId}/recipients/{recipientId}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|listId|Zeichenfolge|Ja|Pfad|keine|Eindeutige Id der Empfängerliste|
|recipientId|Zeichenfolge|Ja|Pfad|keine|Eindeutige Id des Empfängers|

#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannte Fehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="object-definitions"></a>Objektdefinitionen 

### <a name="emailrequest"></a>EmailRequest


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Von|Zeichenfolge|Ja |
|FromName|Zeichenfolge|Nein |
|An|Zeichenfolge|Ja |
|ToName|Zeichenfolge|Nein |
|Betreff|Zeichenfolge|Ja |
|Körper|Zeichenfolge|Ja |
|ishtml|boolescher Wert|Nein |
|cc|Zeichenfolge|Nein |
|ccname|Zeichenfolge|Nein |
|Bcc|Zeichenfolge|Nein |
|bccname|Zeichenfolge|Nein |
|ReplyTo|Zeichenfolge|Nein |
|Datum|Zeichenfolge|Nein |
|Kopfzeilen|Zeichenfolge|Nein |
|Dateien|Array|Nein |
|Dateinamen|Array|Nein |



### <a name="emailresponse"></a>EmailResponse


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Nachricht|Zeichenfolge|Nein |



### <a name="recipientlists"></a>RecipientLists


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Listen|Array|Nein |



### <a name="recipientlist"></a>RecipientList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|Name|Zeichenfolge|Nein |
|recipient_count|ganze Zahl|Nein |



### <a name="recipients"></a>Empfänger


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Empfänger|Array|Nein |



### <a name="recipient"></a>Empfänger


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|E-Mail|Zeichenfolge|Nein |
|Nachname|Zeichenfolge|Nein |
|Vorname|Zeichenfolge|Nein |
|ID|Zeichenfolge|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)