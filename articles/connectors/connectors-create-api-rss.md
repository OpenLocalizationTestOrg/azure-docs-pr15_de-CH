<properties
pageTitle="RSS | Microsoft Azure"
description="Erstellen von Logik apps mit Azure App. RSS-Connector ermöglicht veröffentlichen und Futtermittel abgerufen. Außerdem ermöglicht die Vorgänge ausgelöst, wenn ein neues Element in den Feed veröffentlicht wird."
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

# <a name="get-started-with-the-rss-connector"></a>Erste Schritte mit RSS-connector
RSS ist eine gängige Web Veröffentlichungsformat verwendet häufig aktualisierte Inhalte – Schlagzeilen wie Blogeinträge veröffentlichen.  Viele Inhaltsherausgeber bereitstellen, um Benutzern abonnieren ein RSS-Feeds.  Verwenden Sie RSS-feed Informationen und Auslöser Flows abgerufen, wenn neue Elemente in einen RSS-feed veröffentlicht werden.

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2015-08-01-Vorschau-Schemaversion. 

Sie können jetzt eine Logik-app erstellen machen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

RSS-Connector kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen in JSON und XML-Daten. 

 RSS-Connector hat folgende Aktionen und Triggern verfügbar:

### <a name="rss-actions"></a>RSS-Aktionen
Sie können diese Aktionen durchführen:

|Aktion|Beschreibung|
|--- | ---|
|[ListFeedItems](connectors-create-api-rss.md#listfeeditems)|Alle RSS-feed Elemente zu erhalten.|
### <a name="rss-triggers"></a>RSS-Trigger
Sie können diese Ereignisse überwachen:

|Trigger | Beschreibung|
|--- | ---|
|Wenn ein neues Element Feeds veröffentlicht|Einen Workflow ausgelöst wird, wenn neuer Feed veröffentlicht wird|


## <a name="create-a-connection-to-rss"></a>Erstellen einer Verbindung mit RSS

>[AZURE.INCLUDE [Steps to create a connection to an RSS feed](../../includes/connectors-create-api-rss.md)]

>[AZURE.TIP] Hierzu können Sie in anderen apps Logik.

## <a name="reference-for-rss"></a>Referenz für RSS
Gilt für Version: 1.0

## <a name="onnewfeed"></a>OnNewFeed
Wenn ein neues Element Feeds veröffentlicht: löst einen Workflow beim Veröffentlichen eines neuen Feeds 

```GET: /OnNewFeed``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|feedUrl|Zeichenfolge|Ja|Abfrage|keine|Feed-url|

#### <a name="response"></a>Antwort

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


## <a name="listfeeditems"></a>ListFeedItems
Listen Sie alle RSS-feed Elemente.: Alle RSS-feed Elemente abzurufen. 

```GET: /ListFeedItems``` 

| Name| Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|feedUrl|Zeichenfolge|Ja|Abfrage|keine|Feed-url|

#### <a name="response"></a>Antwort

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


## <a name="object-definitions"></a>Objektdefinitionen 

### <a name="triggerbatchresponsefeeditem"></a>TriggerBatchResponse [FeedItem]


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Array|Nein |



### <a name="feeditem"></a>FeedItem


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Ja |
|Titel|Zeichenfolge|Ja |
|Inhalt|Zeichenfolge|Ja |
|Links|Array|Nein |
|updatedOn|Zeichenfolge|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)