
<properties
    pageTitle="Fügen Sie HTTP + stolz Aktion Logik apps | Microsoft Azure"
    description="Übersicht über HTTP + stolz Aktion und Vorgänge"
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http--swagger-action"></a>Erste Schritte mit HTTP + stolz Aktion

HTTP + stolz Aktion kann ein erstklassiges Anschluss REST-Endpunkt durch ein [Swagger Dokument](https://swagger.io)erstellen. Sie können auch eine Anwendung Logik rufen alle REST-Endpunkt mit einer erstklassigen Logik App Designer erweitern.

Erste Schritte mit HTTP + stolz Aktion in einer Anwendung Logik finden Sie unter [Erstellen einer neuen Applikation Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

---

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Mit HTTP + stolz eines Triggers oder einer Aktion

HTTP + stolz Trigger und Aktion funktionieren genauso wie die [http-Aktion](connectors-native-http.md) aber Design optimal von [Metadaten Swagger](https://swagger.io)Form der API und Ausgaben im Designer anzeigen. Darüber hinaus können Sie HTTP + Swagger als Trigger. Wenn Sie Polling Trigger implementieren möchten, sollten es abrufen Muster folgen, das Erstellen [einer benutzerdefinierten API Logik apps mit](../app-service-logic/app-service-logic-create-api-app.md#polling-triggers)beschrieben.

[Erfahren Sie mehr über Logik app Auslöser und Aktionen.](connectors-overview.md)

Hier ist ein Beispiel verwenden HTTP + stolz Vorgang als Aktion in einem Workflow in einer Anwendung Logik.

1. Klicken Sie auf **Neuen Schritt** .
2. Wählen Sie **eine Aktion hinzufügen**.
3. Geben Sie im Suchfeld Aktion Liste HTTP + stolz Aktion **swagger** .

    ![Wählen Sie HTTP + stolz Aktion](./media/connectors-native-http-swagger/using-action-1.png)

4. Geben Sie die URL für ein Dokument stolz:
    - Logik App-Designer arbeiten, muss die URL eine HTTPS-Endpunkt und CORS aktiviert haben.
    - Wenn das Dokument stolz dieser Anforderung entspricht, können [Azure-Speicher mit CORS aktiviert](#hosting-swagger-from-storage) Sie das Dokument gespeichert.
5. Klicken Sie auf **Weiter** um gelesen und stolz Dokument dargestellt.
6. Alle Parameter, die für die HTTP-Aufruf hinzufügen.

    ![HTTP-Aktivität](./media/connectors-native-http-swagger/using-action-2.png)

1. **Klicken Sie auf oben links die Symbolleiste und die Logik app werden speichern** und veröffentlichen (aktivieren).

### <a name="host-swagger-from-azure-storage"></a>Host stolz von Azure-Speicher

Möglicherweise möchten verweisen auf ein Dokument stolz nicht gehostet oder die Sicherheit und Cross-Ursprung für den Designer nicht entsprechen. Um dieses Problem zu beheben, können Sie stolz Dokument in Azure-Speicher speichern und CORS auf das Dokument.  

Hier werden die Schritte zum Erstellen, konfigurieren und stolz Dokumente in Azure-Speicher speichern:

1. [Erstellen einer Firma Azure-Speicher mit Azure BLOB-Speicher](../storage/storage-create-storage-account.md). (Um diese Berechtigungen **öffentlicher**Zugriff erforderlich ist.)
2. Das Blob aktivieren Sie CORS. [PowerShell-Skript](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1) können Sie die Einstellung automatisch konfigurieren.
3. Das Blob hochladen Sie stolz Datei. Dies ist aus dem [Azure-Portal](https://portal.azure.com) oder ein Tool wie [Azure Storage Explorer](http://storageexplorer.com/)möglich.
1. Verweisen auf eine HTTPS-Link zum Dokument in Azure BLOB-Speicher. (Die Verknüpfung entspricht dem Format `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`.)



## <a name="technical-details"></a>Technische details

Folgende sind die Einzelheiten der Auslöser und Aktionen, diesem HTTP + stolz Connector unterstützt.

## <a name="http--swagger-triggers"></a>HTTP + stolz Trigger

Ein Trigger ist ein Ereignis, das zum Starten des Workflows, die in einer Anwendung Logik definiert verwendet werden kann. [Weitere Informationen zu Triggern](connectors-overview.md) HTTP + stolz Connector hat einen Trigger.

|Trigger|Beschreibung|
|---|---|
|HTTP + stolz|HTTP-aufrufen und der Antwortinhalt zurück|

## <a name="http--swagger-actions"></a>HTTP + stolz Aktionen

Eine Aktion ist eine Operation, die vom Workflow ausgeführt wird, die in einer Anwendung Logik definiert. [Weitere Informationen über Aktionen.](connectors-overview.md) HTTP + stolz Connector wurde eine mögliche Aktion.

|Aktion|Beschreibung|
|---|---|
|HTTP + stolz|HTTP-aufrufen und der Antwortinhalt zurück|

### <a name="action-details"></a>Aktionsdetails

HTTP + stolz Connector enthält eine mögliche Aktion. Informationen über die Aktionen, die erforderlichen und optionalen Felder und die entsprechenden Details zur, die mit der Verwendung folgt.

#### <a name="http--swagger"></a>HTTP + stolz

Stellen Sie eine ausgehende HTTP-Anforderung mit stolz Metadaten.
Ein Sternchen (*) ist ein obligatorisches Feld.

|Angezeigter name|Eigenschaftenname|Beschreibung|
|---|---|---|
|Methode *|-Methode|HTTP-Verb verwendet.|
|URI *|URI|URI für die HTTP-Anforderung.|
|Kopfzeilen|Kopfzeilen|Ein JSON-Objekt von HTTP-Headern enthalten.|
|Körper|Körper|Der Hauptteil der HTTP-Anforderung.|
|Authentifizierung|Authentifizierung|Authentifizierung für Anforderung. [Weitere Informationen finden Sie unter HTTP](./connectors-native-http.md#authentication).|

**Ausgabedetails**

HTTP-Antwort

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Kopfzeilen|Objekt|Antwort-Header|
|Körper|Objekt|Response-Objekt|
|Statuscode|int|HTTP-Statuscode|

### <a name="http-responses"></a>HTTP-Antworten

Verschiedene Aktionen aufrufen, können Sie bestimmte Antworten erhalten. Es folgt eine Tabelle, die entsprechenden Antworten und Beschreibung erläutert.

|Name|Beschreibung|
|---|---|
|200|Okay|
|202|Akzeptiert|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Es ist ein Fehler aufgetreten.|

---

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie die Plattform und [erstellen eine Logik](../app-service-logic/app-service-logic-create-a-logic-app.md) . Andere verfügbare Anschlüsse Logik Apps erkunden anhand der [Liste der APIs](apis-list.md).
