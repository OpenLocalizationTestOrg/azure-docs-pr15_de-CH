<properties
pageTitle="Verwenden der Office 365-Video-Anschluss in Logik-apps | Microsoft Azure"
description="Erste Schritte mit Office 365-Video-Anschluss in Microsoft Azure App Service Logik-apps"
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

# <a name="get-started-with-the-office365-video-connector"></a>Erste Schritte mit der Office365-Video-Anschluss
Verbinden Sie mit Office 365 Video erhalten Informationen zu Office 365 video, eine Liste von Videos und mehr. Office 365-Video-Anschluss kann vorgesehen:

- Logik-apps 

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2015-08-01-Vorschau-Schemaversion. Dieser Connector wird nicht auf früheren Schemaversionen unterstützt.

Mit Office 365 Video können Sie:

- Erstellen Sie Ihr Geschäftsablauf anhand der Daten aus Office 365 Video erhalten. 
- Mithilfe von Aktionen, die eine Liste der in einem Kanal und alle video überprüfen video Portal. Diese Aktionen eine Antwort erhalten und dann die Ausgabe für andere Aktionen verfügbar machen. Beispielsweise Sie können den Suche in Bing Connector nach Office 365 Videos suchen, und verwenden Sie den Office 365-Videoanschluss Informationen zu Videos. Wenn das Video Ihren Anforderungen entspricht, können Sie dieses Video auf Facebook buchen. 

Ein Vorgang in Logik-apps finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Office 365-Video-Anschluss hat die folgenden Aktionen zur Verfügung. Es gibt keine Trigger.

| Trigger | Aktionen|
| --- | --- |
| Keine | <ul><li>Video Portal überprüft</li><li>Alle sichtbaren Kanäle</li><li>Wiedergabe-Url des Manifests Azure Media Services ein Video zu erhalten</li><li>Rufen Sie Zugriff auf das Video entschlüsseln trägertoken ab</li><li>Ruft Informationen zu einem bestimmten office365 video</li><li>Listet alle im Kanal office365 videos</li></ul>

Alle Connectors unterstützen in JSON und XML-Daten. 

## <a name="create-a-connection-to-office365-video-connector"></a>Erstellen einer Verbindung mit Office365-Video-Anschluss
Beim Hinzufügen dieser Connector zu Logik-apps Sie Ihrem Office 365 Video Konto anmelden und Logik-apps zu Ihrem Konto herstellen können.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Nach dem Erstellen der Verbindung die Videoeigenschaften wie Mieter Office 365 eingeben oder channel-ID Die **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

>[AZURE.TIP] Office 365 Video dieselbe Verbindung können Sie in anderen apps Logik.

## <a name="swagger-rest-api-reference"></a>Stolz REST-API-Referenz
Gilt für Version: 1.0.

### <a name="checks-video-portal-status"></a>Video Portal überprüft 
Überprüft video Portal Videodienste aktiviert werden.  
```GET: /{tenant}/IsEnabled``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Mieter|Zeichenfolge|Ja|Pfad|keine|Der Mieter Name für das Verzeichnis der Benutzer gehört|


#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|



### <a name="get-all-viewable-channels"></a>Alle sichtbaren Kanäle 
Ruft alle Kanäle, die der Benutzer Zugriff auf anzeigen.  
```GET: /{tenant}/Channels``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Mieter|Zeichenfolge|Ja|Pfad|keine|Der Mieter Name für das Verzeichnis der Benutzer gehört|


#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|




### <a name="lists-all-the-office365-videos-present-in-a-channel"></a>Listet alle im Kanal office365 videos 
Listet alle im Kanal office365 Videos.  
```GET: /{tenant}/Channels/{channelId}/Videos``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Mieter|Zeichenfolge|Ja|Pfad|keine|Der Mieter Name für das Verzeichnis der Benutzer gehört|
|channelId|Zeichenfolge|Ja|Pfad|keine|Kanal-Id aus der Videos abgerufen werden müssen|


#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|




### <a name="gets-information-about-a-particular-office365-video"></a>Ruft Informationen zu einem bestimmten office365 video 
Ruft Informationen zu einem bestimmten office365 video.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Mieter|Zeichenfolge|Ja|Pfad|keine|Der Mieter Name für das Verzeichnis der Benutzer gehört|
|channelId|Zeichenfolge|Ja|Pfad|keine|Kanal-id|
|videoId|Zeichenfolge|Ja|Pfad|keine|Die video-id|


#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|




### <a name="get-playback-url-of-the-azure-media-services-manifest-for-a-video"></a>Wiedergabe-Url des Manifests Azure Media Services ein Video zu erhalten 
Wiedergabe-Url des Manifests Azure Media Services ein Video zu erhalten.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/playbackurl``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Mieter|Zeichenfolge|Ja|Pfad|keine|Der Mieter Name für das Verzeichnis der Benutzer gehört|
|channelId|Zeichenfolge|Ja|Pfad|keine|Kanal-id|
|videoId|Zeichenfolge|Ja|Pfad|keine|Die video-id|
|streamingFormatType|Zeichenfolge|Ja|Abfrage|keine|Streaming Formattyp. 1 - Smooth Streaming oder MPEG-DASH. 0 - HLS Streaming|


#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|




### <a name="get-the-bearer-token-to-get-access-to-decrypt-the-video"></a>Rufen Sie Zugriff auf das Video entschlüsseln trägertoken ab 
Rufen Sie trägertoken Zugriff auf Video entschlüsseln ab.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/token```

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Mieter|Zeichenfolge|Ja|Pfad|keine|Der Mieter Name für das Verzeichnis der Benutzer gehört|
|channelId|Zeichenfolge|Ja|Pfad|keine|Kanal-id|
|videoId|Zeichenfolge|Ja|Pfad|keine|Die video-id|


#### <a name="response"></a>Antwort

|Name|Beschreibung|
|---|---|
|200|Vorgang war erfolgreich|
|400|BadRequest|
|401|Nicht autorisiert|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="object-definitions"></a>Objektdefinitionen

#### <a name="channel-channel-class"></a>Kanal: Channel-Klasse

| Name | Datentyp | Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|Titel|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|


#### <a name="video"></a>Video 

| Name | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|Titel|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|
|CreationDate|Zeichenfolge|Nein|
|Besitzer|Zeichenfolge|Nein|
|ThumbnailUrl|Zeichenfolge|Nein|
|VideoUrl|Zeichenfolge|Nein|
|VideoDuration|ganze Zahl|Nein|
|VideoProcessingStatus|ganze Zahl|Nein|
|Aufrufstatistik|ganze Zahl|Nein|


## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).
