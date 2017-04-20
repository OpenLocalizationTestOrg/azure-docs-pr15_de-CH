<properties
    pageTitle="Facebook-Connector in Ihren Apps Logik hinzufügen | Microsoft Azure"
    description="Übersicht der Facebook-Connector mit REST-API"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-facebook-connector"></a>Erste Schritte mit der Facebook-connector
Verbinden mit Facebook und Buchen in ein Schnittfenster, eine Seite Feeds und mehr. 

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2015-08-01-Vorschau-Schemaversion.


Mit Facebook können Sie:

- Erstellen Sie Ihr Geschäftsablauf basierend auf den Daten von Facebook erhalten. 
- Verwenden eines Triggers, wenn eine neue Nachricht eingeht.
- Mit Aktionen, die auf der Zeitachse, erhalten eine Seite Feeds und mehr. Diese Aktionen eine Antwort erhalten und dann die Ausgabe für andere Aktionen verfügbar machen. Bei eine neue Nachricht auf der Zeitachse, können Sie beispielsweise nutzen, Post und Ihr Twitter-Feed schieben. 



Ein Vorgang in Logik-apps finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Facebook-Connector enthält folgende Trigger und Aktionen. 

| Trigger | Aktionen|
| --- | --- |
| <ul><li>Wird eine neue Nachricht auf meinem Zeitplan</li></ul> |<ul><li>Aus Zeitplan feed abrufen</li><li>Auf meinem Zeitplan buchen</li><li>Wird eine neue Nachricht auf meinem Zeitplan</li><li>Seite feed</li><li>Erhalten Benutzer Zeitachse</li><li>Senden an Seite</li></ul>

Alle Connectors unterstützen in JSON und XML-Daten.

## <a name="create-a-connection-to-facebook"></a>Erstellen Sie eine Verbindung zu Facebook
Beim Hinzufügen dieses Connectors zu Logik-apps müssen Sie Logik apps Verbindung zu Facebook autorisieren.

1. Ihr Facebook-Konto anmelden
2. Wählen Sie **Autorisieren**und können Sie Logik-apps zu Facebook verwenden. 

>[AZURE.INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]

>[AZURE.TIP] Dieselbe Facebook Verbindung können Sie in anderen apps Logik.

## <a name="swagger-rest-api-reference"></a>Stolz REST-API-Referenz
Gilt für Version: 1.0.

### <a name="get-feed-from-my-timeline"></a>Aus Zeitplan feed abrufen
Ruft die Feeds aus der angemeldete Benutzer Zeitachse ab.  
```GET: /me/feed```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Felder|Zeichenfolge|Nein|Abfrage|keine |Geben Sie die Felder, die zurückgegeben werden sollen. Beispiel (Id, Name und Bild).|
|Grenzwert|ganze Zahl|Nein|Abfrage| keine|Maximale Anzahl an Beiträgen abgerufen werden|
|mit|Zeichenfolge|Nein|Abfrage| keine|Schränken Sie die Liste der Stellen, die mit verbunden.|
|Filter|Zeichenfolge|Nein|Abfrage| keine|Rufen Sie nur Nachrichten, die einen bestimmten Streamfilter entsprechen.|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="post-to-my-timeline"></a>Auf meinem Zeitplan buchen
Nachricht Status des angemeldeten Benutzers Zeitplan.  
```POST: /me/feed```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Bereitstellen|Zeichenfolge |Ja|Körper|keine |Neue Nachricht gebucht werden|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="when-there-is-a-new-post-on-my-timeline"></a>Wird eine neue Nachricht auf meinem Zeitplan
Löst einen neuen Datenfluss wird eine neue Bereitstellung auf den angemeldeten Benutzer Zeitachse.  
```GET: /trigger/me/feed```

Es sind keine Parameter. 

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="get-page-feed"></a>Seite feed
Feed für eine angegebene Seite erhalten Sie Beiträge.  
```GET: /{pageId}/feed```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|pageId|Zeichenfolge|Ja|Pfad| keine|Die ID der Seite aus der Beiträge abgerufen werden.|
|Grenzwert|ganze Zahl|Nein|Abfrage| keine|Maximale Anzahl an Beiträgen abgerufen werden|
|include_hidden|boolescher Wert|Nein|Abfrage|keine |Ob Beiträge enthalten, die von der Seite ausgeblendet wurden|
|Felder|Zeichenfolge|Nein|Abfrage|keine |Geben Sie die Felder, die zurückgegeben werden sollen. Beispiel (Id, Name und Bild).|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="get-user-timeline"></a>Erhalten Benutzer Zeitachse
Zeitplan für einen Benutzer erhalten Sie Beiträge.  
```GET: /{userId}/feed```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Benutzer-ID|Zeichenfolge|Ja|Pfad|keine |ID des Benutzers, dessen Zeitplan abgerufen werden müssen.|
|Grenzwert|ganze Zahl|Nein|Abfrage|keine |Maximale Anzahl an Beiträgen abgerufen werden|
|mit|Zeichenfolge|Nein|Abfrage|keine |Schränken Sie die Liste der Stellen, die mit verbunden.|
|Filter|Zeichenfolge|Nein|Abfrage| keine|Rufen Sie nur Nachrichten, die einen bestimmten Streamfilter entsprechen.|
|Felder|Zeichenfolge|Nein|Abfrage| keine|Geben Sie die Felder, die zurückgegeben werden sollen. Beispiel (Id, Name und Bild).|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="post-to-page"></a>Senden an Seite
Bereitstellen einer Nachricht in eine Facebook Page des angemeldeten Benutzers.  
```POST: /{pageId}/feed```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|pageId|Zeichenfolge|Ja|Pfad|keine |ID der Seite.|
|Bereitstellen|viele |Ja|Körper|keine |Neue Nachricht gebucht werden.|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="object-definitions"></a>Objektdefinitionen

#### <a name="getfeedresponse"></a>GetFeedResponse

|Eigenschaftenname | Datentyp | Erforderlich|
|---|---|---|
|Daten|Array|Nein|

#### <a name="triggerfeedresponse"></a>TriggerFeedResponse

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Daten|Array|Nein|

#### <a name="postitem-a-single-entry-in-a-profiles-feed"></a>PostItem: Ein einzelnen Eintrag im Profil des Feeds
Das Profil möglicherweise Benutzer, Seite, Anwendung oder Gruppe. 

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|admin_creator|Array|Nein|
|Beschriftung|Zeichenfolge|Nein|
|created_time|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|
|feed_targeting|nicht definiert|Nein|
|Von|nicht definiert|Nein|
|Symbol|Zeichenfolge|Nein|
|is_hidden|boolescher Wert|Nein|
|is_published|boolescher Wert|Nein|
|Link|Zeichenfolge|Nein|
|Nachricht|Zeichenfolge|Nein|
|Name|Zeichenfolge|Nein|
|object_id|Zeichenfolge|Nein|
|Bild|Zeichenfolge|Nein|
|Ort|nicht definiert|Nein|
|Datenschutz|nicht definiert|Nein|
|Eigenschaften|Array|Nein|
|Quelle|Zeichenfolge|Nein|
|status_type|Zeichenfolge|Nein|
|Geschichte|Zeichenfolge|Nein|
|Zielgruppenadressierung|nicht definiert|Nein|
|An|Array|Nein|
|Typ|Zeichenfolge|Nein|
|updated_time|Zeichenfolge|Nein|
|with_tags|nicht definiert|Nein|

#### <a name="triggeritem-a-single-entry-in-a-profiles-feed"></a>TriggerItem: Ein einzelner Eintrag in einem Profil des Feeds
Das Profil möglicherweise Benutzer, Seite, Anwendung oder Gruppe.

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|created_time|Zeichenfolge|Nein|
|Von|nicht definiert|Nein|
|Nachricht|Zeichenfolge|Nein|
|Typ|Zeichenfolge|Nein|

#### <a name="adminitem"></a>AdminItem

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|Link|Zeichenfolge|Nein|

#### <a name="propertyitem"></a>PropertyItem

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Name|Zeichenfolge|Nein|
|Text|Zeichenfolge|Nein|
|href|Zeichenfolge|Nein|

#### <a name="userpostfeedrequest"></a>UserPostFeedRequest

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Nachricht|Zeichenfolge|Ja|
|Link|Zeichenfolge|Nein|
|Bild|Zeichenfolge|Nein|
|Name|Zeichenfolge|Nein|
|Beschriftung|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|
|Ort|Zeichenfolge|Nein|
|Tags|Zeichenfolge|Nein|
|Datenschutz|nicht definiert|Nein|
|object_attachment|Zeichenfolge|Nein|

#### <a name="pagepostfeedrequest"></a>PagePostFeedRequest

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Nachricht|Zeichenfolge|Ja|
|Link|Zeichenfolge|Nein|
|Bild|Zeichenfolge|Nein|
|Name|Zeichenfolge|Nein|
|Beschriftung|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|
|Aktionen|Array|Nein|
|Ort|Zeichenfolge|Nein|
|Tags|Zeichenfolge|Nein|
|object_attachment|Zeichenfolge|Nein|
|Zielgruppenadressierung|nicht definiert|Nein|
|feed_targeting|nicht definiert|Nein|
|veröffentlicht|boolescher Wert|Nein|
|scheduled_publish_time|Zeichenfolge|Nein|
|backdated_time|Zeichenfolge|Nein|
|backdated_time_granularity|Zeichenfolge|Nein|
|child_attachments|Array|Nein|
|multi_share_end_card|boolescher Wert|Nein|

#### <a name="postfeedresponse"></a>PostFeedResponse

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|

#### <a name="profilecollection"></a>ProfileCollection

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Daten|Array|Nein|

#### <a name="useritem"></a>UserItem

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|Vorname|Zeichenfolge|Nein|
|Nachname|Zeichenfolge|Nein|
|Name|Zeichenfolge|Nein|
|Geschlecht|Zeichenfolge|Nein|
|über|Zeichenfolge|Nein|

#### <a name="actionitem"></a>ActionItem

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Name|Zeichenfolge|Nein|
|Link|Zeichenfolge|Nein|

#### <a name="targetitem"></a>TargetItem

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Länder|Array|Nein|
|Länderinformationen|Array|Nein|
|Regionen|Array|Nein|
|Städte|Array|Nein|

#### <a name="feedtargetitem-object-that-controls-news-feed-targeting-for-this-post"></a>FeedTargetItem: Objekt, News steuert, feed Zielgruppenadressierung für diesen Beitrag
Jeder dieser Gruppen ist eher diesen Beitrag, andere sind weniger wahrscheinlich. Gilt nur für Seiten.

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Länder|Array|Nein|
|Regionen|Array|Nein|
|Städte|Array|Nein|
|age_min|ganze Zahl|Nein|
|age_max|ganze Zahl|Nein|
|Geschlecht|Array|Nein|
|relationship_statuses|Array|Nein|
|interested_in|Array|Nein|
|college_years|Array|Nein|
|Interessen|Array|Nein|
|relevant_until|ganze Zahl|Nein|
|education_statuses|Array|Nein|
|Länderinformationen|Array|Nein|

#### <a name="placeitem"></a>PlaceItem

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|Name|Zeichenfolge|Nein|
|overall_rating|Anzahl|Nein|
|Speicherort|nicht definiert|Nein|

#### <a name="locationitem"></a>LocationItem

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Ort|Zeichenfolge|Nein|
|Land|Zeichenfolge|Nein|
|Latitude|Anzahl|Nein|
|located_in|Zeichenfolge|Nein|
|Länge|Anzahl|Nein|
|Name|Zeichenfolge|Nein|
|Region|Zeichenfolge|Nein|
|Zustand|Zeichenfolge|Nein|
|Straße|Zeichenfolge|Nein|
|ZIP|Zeichenfolge|Nein|

#### <a name="privacyitem"></a>PrivacyItem

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Beschreibung|Zeichenfolge|Nein|
|Wert|Zeichenfolge|Ja|
|zulassen|Zeichenfolge|Nein|
|Verweigern|Zeichenfolge|Nein|
|Freunde|Zeichenfolge|Nein|

#### <a name="childattachmentsitem"></a>ChildAttachmentsItem

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Link|Zeichenfolge|Nein|
|Bild|Zeichenfolge|Nein|
|image_hash|Zeichenfolge|Nein|
|Name|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|

#### <a name="postphotorequest"></a>PostPhotoRequest

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|URL|Zeichenfolge|Ja|
|Beschriftung|Zeichenfolge|Nein|

#### <a name="postphotoresponse"></a>PostPhotoResponse

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Ja|
|post_id|Zeichenfolge|Ja|

#### <a name="postvideorequest"></a>PostVideoRequest

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|videoData|Zeichenfolge|Ja|
|Beschreibung|Zeichenfolge|Ja|
|Titel|Zeichenfolge|Ja|
|uploadedVideoName|Zeichenfolge|Nein|

#### <a name="getphotoresponse"></a>GetPhotoResponse

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Daten|nicht definiert|Ja|

#### <a name="getphotoresponseitem"></a>GetPhotoResponseItem

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|URL|Zeichenfolge|Ja|
|is_silhouette|boolescher Wert|Ja|
|Höhe|Zeichenfolge|Nein|
|Breite|Zeichenfolge|Nein|

#### <a name="geteventresponse"></a>GetEventResponse

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Daten|Array|Ja|

#### <a name="geteventresponseitem"></a>GetEventResponseItem

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Ja|
|Name|Zeichenfolge|Ja|
|start_time|Zeichenfolge|Nein|
|end_time|Zeichenfolge|Nein|
|Zeitzone|Zeichenfolge|Nein|
|Speicherort|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|
|ticket_uri|Zeichenfolge|Nein|
|rsvp_status|Zeichenfolge|Ja|


## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).
