<properties
pageTitle=" In Ihren apps Logik verwenden Pufferzeit | Microsoft Azure"
description="Erste Schritte mit der Pufferzeit Connector in Microsoft Azure Service Anwendungslogik apps"
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

# <a name="get-started-with-the-slack-connector"></a>Erste Schritte mit der Pufferzeit connector

Pufferzeit ist Team Kommunikationswerkzeug, die vereint alle Kommunikation in Ihrem Team eine platzieren sofort durchsuchbar und überall verfügbar.

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2015-08-01-Vorschau-Schemaversion.

Mit Pufferzeit Connector können Sie:

* Logik-apps zu verwenden

Ein Vorgang in Logik-apps finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="lets-talk-about-triggers-and-actions"></a>Wissenswertes zu Triggern und Aktionen

Pufferzeit Connector kann als eine Aktion verwendet werden. Es gibt keine Trigger. Alle Connectors unterstützen in JSON und XML-Daten. 

 Pufferzeit Connector hat folgende Aktionen und Triggern verfügbar:

### <a name="slack-actions"></a>Pufferzeit Aktionen
Sie können diese Aktionen durchführen:

|Aktion|Beschreibung|
|--- | ---|
|PostMessage|Eine Nachricht an einen angegebenen Kanal.|
## <a name="create-a-connection-to-slack"></a>Erstellen einer Verbindung mit Puffer
Um die Pufferzeit Connector verwenden, erstellen Sie zunächst eine **Verbindung** Sie die Details für diese Eigenschaften bereit: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Pufferzeit Anmeldeinformationen|

Gehen Sie Pufferzeit anmelden und die Konfiguration der Pufferzeit **Verbindung** in Ihrer Anwendung Logik abgeschlossen:

1. Wählen Sie **Wiederholung**
2. Wählen Sie eine **Häufigkeit** und geben Sie ein **Intervall**
3. Wählen Sie **eine Aktion hinzufügen**  
![Konfigurieren der Pufferzeit][1]  
4. Puffer in das Suchfeld eingeben und Suche alle Einträge mit Pufferzeit im Namen zurückgeben warten
5. **Puffer - Beitrags** auswählen
6. Wählen Sie die **Pufferzeit anmelden**:  
![Konfigurieren der Pufferzeit][2]
7. Anmeldeinformationen Sie die Pufferzeit anmelden, um die Anwendung zu autorisieren    
![Konfigurieren der Pufferzeit][3]  
8. Sie werden zur Anmeldeseite Ihrer Organisation weitergeleitet. **Autorisieren** Pufferzeit Logik app interagieren:      
![Konfigurieren der Pufferzeit][5] 
9. Nach Abschluss die Autorisierung müssen Sie für Ihre Anwendung Logik an durch Konfigurieren des Abschnitts **Puffer - alle Nachrichten** weitergeleitet. Fügen Sie andere Auslöser und Aktionen, die Sie benötigen.  
![Konfigurieren der Pufferzeit][6]
10. Speichern Sie Ihre Arbeit **Auswählen der Menüleiste oben** .


>[AZURE.TIP] Hierzu können Sie in anderen apps Logik.

## <a name="slack-rest-api-reference"></a>Pufferzeit REST-API-Referenz
#### <a name="this-documentation-is-for-version-10"></a>Diese Dokumentation ist Version: 1.0


### <a name="post-a-message-to-a-specified-channel"></a>Eine Nachricht an einen angegebenen Kanal.
**```POST: /chat.postMessage```** 



| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Kanal|Zeichenfolge|Ja|Abfrage|keine|Kanal, privat oder IM Kanal senden. Ein Name (ex: #general) oder eine codierte-ID.|
|Text|Zeichenfolge|Ja|Abfrage|keine|Text der zu sendenden Nachricht. Formatierungsoptionen, finden Sie unter https://api.slack.com/docs/formatting.|
|Benutzername|Zeichenfolge|Nein|Abfrage|keine|Name des bot|
|as_user|boolescher Wert|Nein|Abfrage|keine|Übergeben Sie True, um die Nachricht als authentifizierter Benutzer statt als bot|
|Analysieren|Zeichenfolge|Nein|Abfrage|keine|Ändern Sie, wie Nachrichten behandelt werden. Weitere Informationen finden Sie unter https://api.slack.com/docs/formatting.|
|link_names|ganze Zahl|Nein|Abfrage|keine|Suchen und Verknüpfen von Namen und Benutzernamen.|
|unfurl_links|boolescher Wert|Nein|Abfrage|keine|True aktiviert Entfaltung hauptsächlich textbasierte Inhalte übergeben.|
|unfurl_media|boolescher Wert|Nein|Abfrage|keine|Übergeben Sie Deaktivieren der Medieninhalte Entfaltung False.|
|icon_url|Zeichenfolge|Nein|Abfrage|keine|URL zu einem Bild als Symbol für diese Nachricht|
|icon_emoji|Zeichenfolge|Nein|Abfrage|keine|Emoji als Symbol für diese Nachricht verwenden|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten:

|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|408|Timeout bei Anforderung|
|429|Zu viele Anfragen|
|500|Interner Serverfehler. Unbekannte Fehler|
|503|Pufferzeit Service nicht verfügbar|
|504|Gateway-Timeout|
|Standard|Vorgang ist fehlgeschlagen.|
------



## <a name="object-definitions"></a>Objekt-Definitionen: 

 **Meldung**: Yammer Nachricht

Erforderliche Eigenschaften für Nachricht:


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|ID|ganze Zahl|
|content_excerpt|Zeichenfolge|
|sender_id|ganze Zahl|
|replied_to_id|ganze Zahl|
|created_at|Zeichenfolge|
|network_id|ganze Zahl|
|message_type|Zeichenfolge|
|sender_type|Zeichenfolge|
|URL|Zeichenfolge|
|web_url|Zeichenfolge|
|group_id|ganze Zahl|
|Körper|nicht definiert|
|thread_id|ganze Zahl|
|direct_message|boolescher Wert|
|client_type|Zeichenfolge|
|client_url|Zeichenfolge|
|Sprache|Zeichenfolge|
|notified_user_ids|Array|
|Datenschutz|Zeichenfolge|
|liked_by|nicht definiert|
|system_message|boolescher Wert|



 **PostOperationRequest**: stellt eine Post-Anforderung für Yammer Connector buchen bei Yammer veröffentlichen

Erforderliche Eigenschaften für PostOperationRequest:

Körper

**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|Körper|Zeichenfolge|
|group_id|ganze Zahl|
|replied_to_id|ganze Zahl|
|direct_to_id|ganze Zahl|
|Übertragung|boolescher Wert|
|Thema 1|Zeichenfolge|
|topic2|Zeichenfolge|
|topic3|Zeichenfolge|
|topic4|Zeichenfolge|
|topic5|Zeichenfolge|
|topic6|Zeichenfolge|
|topic7|Zeichenfolge|
|topic8|Zeichenfolge|
|topic9|Zeichenfolge|
|topic10|Zeichenfolge|
|topic11|Zeichenfolge|
|topic12|Zeichenfolge|
|topic13|Zeichenfolge|
|topic14|Zeichenfolge|
|topic15|Zeichenfolge|
|topic16|Zeichenfolge|
|topic17|Zeichenfolge|
|topic18|Zeichenfolge|
|topic19|Zeichenfolge|
|topic20|Zeichenfolge|



 **MessageList**: Liste der Nachrichten

Erforderliche Eigenschaften für MessageList:


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|Nachrichten|Array|



 **MessageBody**: Nachrichtentext

Erforderliche Eigenschaften für MessageBody:


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|analysiert|Zeichenfolge|
|Ebene|Zeichenfolge|
|umfangreiche|Zeichenfolge|



 **LikedBy**: gefallen

Erforderliche Eigenschaften für LikedBy:


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|Anzahl|ganze Zahl|
|Namen|Array|



 **YammmerEntity**: gefallen

Erforderliche Eigenschaften für YammmerEntity:


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|Typ|Zeichenfolge|
|ID|ganze Zahl|
|full_name|Zeichenfolge|


## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)
## <a name="object-definitions"></a>Objekt-Definitionen: 

 **WebResultModel**: Bing-Suchergebnisse

Erforderliche Eigenschaften für WebResultModel:


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|Titel|Zeichenfolge|
|Beschreibung|Zeichenfolge|
|DisplayUrl|Zeichenfolge|
|ID|Zeichenfolge|
|FullUrl|Zeichenfolge|



 **VideoResultModel**: video Bing-Suchergebnisse

Erforderliche Eigenschaften für VideoResultModel:


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|Titel|Zeichenfolge|
|DisplayUrl|Zeichenfolge|
|ID|Zeichenfolge|
|MediaUrl|Zeichenfolge|
|Laufzeit|ganze Zahl|
|Miniaturansicht|nicht definiert|



 **ThumbnailModel**: das Multimediaelement Miniaturansicht Eigenschaften

Erforderliche Eigenschaften für ThumbnailModel:


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|MediaUrl|Zeichenfolge|
|ContentType|Zeichenfolge|
|Breite|ganze Zahl|
|Höhe|ganze Zahl|
|Dateigröße|ganze Zahl|



 **ImageResultModel**: Bing Ergebnissen

Erforderliche Eigenschaften für ImageResultModel:


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|Titel|Zeichenfolge|
|DisplayUrl|Zeichenfolge|
|ID|Zeichenfolge|
|MediaUrl|Zeichenfolge|
|SourceUrl|Zeichenfolge|
|Miniaturansicht|nicht definiert|



 **NewsResultModel**: Bing News-Suchergebnisse

Erforderliche Eigenschaften für NewsResultModel:


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|Titel|Zeichenfolge|
|Beschreibung|Zeichenfolge|
|DisplayUrl|Zeichenfolge|
|ID|Zeichenfolge|
|Quelle|Zeichenfolge|
|Datum|Zeichenfolge|



 **SpellResultModel**: Bing Rechtschreibprüfung Vorschläge Ergebnisse

Erforderliche Eigenschaften für SpellResultModel:


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|ID|Zeichenfolge|
|Wert|Zeichenfolge|



 **RelatedSearchResultModel**: Bing Suchergebnisse beziehen

Erforderliche Eigenschaften für RelatedSearchResultModel:


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|Titel|Zeichenfolge|
|ID|Zeichenfolge|
|BingUrl|Zeichenfolge|



 **CompositeSearchResultModel**: zusammengesetzte Bing-Suchergebnisse

Erforderliche Eigenschaften für CompositeSearchResultModel:


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|WebResultsTotal|ganze Zahl|
|ImageResultsTotal|ganze Zahl|
|VideoResultsTotal|ganze Zahl|
|NewsResultsTotal|ganze Zahl|
|SpellSuggestionsTotal|ganze Zahl|
|WebResults|Array|
|ImageResults|Array|
|VideoResults|Array|
|NewsResults|Array|
|SpellSuggestionResults|Array|
|RelatedSearchResults|Array|


## <a name="object-definitions"></a>Objekt-Definitionen: 

 **PostOperationResponse**: auf Post-Operation Pufferzeit Connectors auf Pufferzeit darstellt

Erforderliche Eigenschaften für PostOperationResponse:


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|Okay|boolescher Wert|
|Kanal|Zeichenfolge|
|TS|Zeichenfolge|
|Nachricht|nicht definiert|
|Fehler|Zeichenfolge|



 **"MessageItem"**: ein Kanal angezeigt.

Erforderliche Eigenschaften für "MessageItem":


Die Eigenschaften sind erforderlich. 


**Alle Eigenschaften**: 


| Name | Datentyp |
|---|---|
|Text|Zeichenfolge|
|ID|Zeichenfolge|
|Benutzer|Zeichenfolge|
|erstellt|ganze Zahl|
|Is_user löschen|boolescher Wert|


## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
