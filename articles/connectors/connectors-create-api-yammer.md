<properties
pageTitle="Yammer-Verbindung in Ihren Apps Logik hinzufügen | Microsoft Azure"
description="Übersicht über Yammer Connector mit REST-API"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-yammer-connector"></a>Erste Schritte mit Yammer-connector

Verbinden Sie mit Yammer Zugriff Gespräche in Ihrem Unternehmensnetzwerk.

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2015-08-01-Vorschau-Schemaversion.

Mit Yammer können Sie:

- Erstellen Sie Ihr Geschäftsablauf basierend auf den Daten erhalten von Yammer. 
- Verwendung löst für wird eine neue Nachricht in einer Gruppe oder einem Feed der folgenden.
- Verwenden Sie Aktionen zu einer Nachricht alle Nachrichten. Diese Aktionen eine Antwort erhalten und dann die Ausgabe für andere Aktionen verfügbar machen. Wenn eine neue Nachricht angezeigt wird, können Sie Office 365 per e-Mail senden.

Ein Vorgang in Logik-apps finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Yammer umfasst folgende Auslöser und Aktionen. 

Trigger | Aktionen
--- | ---
<ul><li>Bei eine neue Nachricht in einer Gruppe</li><li>Bei eine neue Nachricht in meiner folgenden feed</li></ul>| <ul><li>Alle Nachrichten</li><li>Ruft Nachrichten in einer Gruppe</li><li>Ruft Nachrichten von meinen folgenden feed</li><li>Nachricht bereitstellen</li><li>Bei eine neue Nachricht in einer Gruppe</li><li>Bei eine neue Nachricht in meiner folgenden feed</li></ul>

Alle Connectors unterstützen in JSON und XML-Daten. 

## <a name="create-a-connection-to-yammer"></a>Verbindung mit Yammer erstellen
Um die Yammer-Connector verwenden, erstellen Sie zunächst eine **Verbindung** Sie die Details für diese Eigenschaften bereit: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Yammer-Anmeldeinformationen|

>[AZURE.INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]


>[AZURE.TIP] Hierzu können Sie in anderen apps Logik.

## <a name="yammer-rest-api-reference"></a>Yammer REST-API-Referenz
Diese Dokumentation ist Version: 1.0


### <a name="get-all-public-messages-in-the-logged-in-users-yammer-network"></a>Alle öffentliche Nachrichten in Yammer-Netzwerk angemeldete Benutzer abrufen
"Alle" Gespräche in Yammer-Weboberfläche entspricht.  
```GET: /messages.json```

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|older_then|ganze Zahl|Nein|Abfrage|keine|Nachrichten, die älter als die Nachricht-ID als eine numerische Zeichenfolge zurückgibt. Dies ist nützlich zum Paginieren Nachrichten. Wenn Sie derzeit 20 Nachrichten anzeigen und die ältesten ist z. B. 2912, konnte Sie anfügen "? Older_than = 2912″ auf Ihre Anforderung zu 20 Nachrichten vor den Sie sehen.|
|newer_then|ganze Zahl|Nein|Abfrage|keine|Gibt Nachrichten neuer als die Nachricht-ID als numerische Zeichenfolge angegeben. Dies sollte verwendet werden, wenn neue Nachrichten abrufen. Wenn Sie Nachrichten suchen und die letzte Nachricht zurückgegebene 3516 können Sie eine Anforderung mit dem Parameter "? Newer_than = 3516″, um sicherzustellen, dass Sie keine Kopien der Nachrichten bereits auf der Seite erhalten.|
|Grenzwert|ganze Zahl|Nein|Abfrage|keine|Die angegebene Anzahl von Nachrichten zurück.|
|Seite|ganze Zahl|Nein|Abfrage|keine|Die angegebene Seite zu erhalten. Wenn Daten größer als der Grenzwert ist, können Sie dieses Feld verwenden, auf nachfolgenden Seiten|


### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|408|Timeout bei Anforderung|
|429|Zu viele Anfragen|
|500|Interner Serverfehler. Unbekannte Fehler|
|503|Yammer-Dienst ist nicht verfügbar|
|504|Gateway-Timeout|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="post-a-message-to-a-group-or-all-company-feed"></a>Eine Nachricht an einer Gruppe oder alle Unternehmen Feed
Sofern Gruppen-ID wird Nachricht der angegebenen Gruppe anderen bereitgestellt werden, die in allen Unternehmen Feed gebucht wird.    
```POST: /messages.json``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Eingabe| |Ja|Körper|keine|POST-Anforderung|


### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|201|Erstellt|



## <a name="object-definitions"></a>Objektdefinitionen

#### <a name="message-yammer-message"></a>: Nachricht Yammer

| Name | Datentyp | Erforderlich |
|---|---| --- | 
|ID|ganze Zahl|Nein|
|content_excerpt|Zeichenfolge|Nein|
|sender_id|ganze Zahl|Nein|
|replied_to_id|ganze Zahl|Nein|
|created_at|Zeichenfolge|Nein|
|network_id|ganze Zahl|Nein|
|message_type|Zeichenfolge|Nein|
|sender_type|Zeichenfolge|Nein|
|URL|Zeichenfolge|Nein|
|web_url|Zeichenfolge|Nein|
|group_id|ganze Zahl|Nein|
|Körper|nicht definiert|Nein|
|thread_id|ganze Zahl|Nein|
|direct_message|boolescher Wert|Nein|
|client_type|Zeichenfolge|Nein|
|client_url|Zeichenfolge|Nein|
|Sprache|Zeichenfolge|Nein|
|notified_user_ids|Array|Nein|
|Datenschutz|Zeichenfolge|Nein|
|liked_by|nicht definiert|Nein|
|system_message|boolescher Wert|Nein|

#### <a name="postoperationrequest-represents-a-post-request-for-yammer-connector-to-post-to-yammer"></a>PostOperationRequest: Stellt eine Post-Anforderung für Yammer Connector buchen bei Yammer veröffentlichen

| Name | Datentyp | Erforderlich |
|---|---| --- | 
|Körper|Zeichenfolge|Ja|
|group_id|ganze Zahl|Nein|
|replied_to_id|ganze Zahl|Nein|
|direct_to_id|ganze Zahl|Nein|
|Übertragung|boolescher Wert|Nein|
|Thema 1|Zeichenfolge|Nein|
|topic2|Zeichenfolge|Nein|
|topic3|Zeichenfolge|Nein|
|topic4|Zeichenfolge|Nein|
|topic5|Zeichenfolge|Nein|
|topic6|Zeichenfolge|Nein|
|topic7|Zeichenfolge|Nein|
|topic8|Zeichenfolge|Nein|
|topic9|Zeichenfolge|Nein|
|topic10|Zeichenfolge|Nein|
|topic11|Zeichenfolge|Nein|
|topic12|Zeichenfolge|Nein|
|topic13|Zeichenfolge|Nein|
|topic14|Zeichenfolge|Nein|
|topic15|Zeichenfolge|Nein|
|topic16|Zeichenfolge|Nein|
|topic17|Zeichenfolge|Nein|
|topic18|Zeichenfolge|Nein|
|topic19|Zeichenfolge|Nein|
|topic20|Zeichenfolge|Nein|

#### <a name="messagelist-list-of-messages"></a>MessageList: Liste der Nachrichten

| Name | Datentyp | Erforderlich |
|---|---| --- | 
|Nachrichten|Array|Nein|


#### <a name="messagebody-message-body"></a>MessageBody: Text

| Name | Datentyp | Erforderlich |
|---|---| --- | 
|analysiert|Zeichenfolge|Nein|
|Ebene|Zeichenfolge|Nein|
|umfangreiche|Zeichenfolge|Nein|

#### <a name="likedby-liked-by"></a>LikedBy: Gefallen

| Name | Datentyp | Erforderlich |
|---|---| --- | 
|Anzahl|ganze Zahl|Nein|
|Namen|Array|Nein|

#### <a name="yammmerentity-liked-by"></a>YammmerEntity: Gefallen

| Name | Datentyp | Erforderlich |
|---|---| --- | 
|Typ|Zeichenfolge|Nein|
|ID|ganze Zahl|Nein|
|full_name|Zeichenfolge|Nein|


## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

[1]: ./media/connectors-create-api-yammer/connectionconfig1.png
[2]: ./media/connectors-create-api-yammer/connectionconfig2.png 
[3]: ./media/connectors-create-api-yammer/connectionconfig3.png
[4]: ./media/connectors-create-api-yammer/connectionconfig4.png
[5]: ./media/connectors-create-api-yammer/connectionconfig5.png
