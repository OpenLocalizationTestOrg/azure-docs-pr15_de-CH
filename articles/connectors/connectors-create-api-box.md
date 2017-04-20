<properties
    pageTitle="Logik-Apps im Feld Anschluss hinzufügen | Microsoft Azure"
    description="Übersicht der Connector im Feld REST API-Parameter"
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

# <a name="get-started-with-the-box-connector"></a>Erste Schritte mit dem Connector im Feld
Feld und erstellen Sie Dateien und Dateien löschen. 

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2015-08-01-Vorschau-Schemaversion.

Im Feld können Sie:

- Erstellen Sie Ihr Geschäftsablauf basierend auf den Daten im Feld erhalten. 
- Verwenden Sie Trigger, wenn eine Datei erstellt oder aktualisiert wird.
- Mithilfe von Aktionen, die eine Datei kopieren, Löschen einer Datei und mehr. Diese Aktionen eine Antwort erhalten und dann die Ausgabe für andere Aktionen verfügbar machen. Z. B. wenn auf eine Datei geändert wird, können Sie diese Datei und e-Mail mit Office 365.

Ein Vorgang in Logik-apps finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Die folgenden Trigger und Aktionen enthält.

| Trigger | Aktionen|
| --- | --- |
|<ul><li>Wenn eine Datei erstellt</li><li>Wenn eine Datei geändert</li></ul> | <ul><li>Datei erstellen</li><li>Wenn eine Datei erstellt</li><li>Datei kopieren</li><li>Datei löschen</li><li>Archiv Ordner</li><li>Datei-Inhalte mit id</li><li>Datei-Inhalte mit Pfad</li><li>Abrufen von Metadaten mit id</li><li>Abrufen von Metadaten mit Pfad</li><li>Datei aktualisieren</li><li>Wenn eine Datei geändert</li></ul>

Alle Connectors unterstützen in JSON und XML-Daten.

## <a name="create-a-connection-to-box"></a>Erstellen Sie eine Verbindung zum Feld
Beim Hinzufügen dieses Connectors zu Logik-apps müssen Sie Logik apps an Ihre Verbindung autorisieren.

>[AZURE.INCLUDE [Steps to create a connection to box](../../includes/connectors-create-api-box.md)]

Nachdem Sie eine Verbindung erstellen, geben Sie die Eigenschaften. Die **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

>[AZURE.TIP] Dieselbe Verbindung im Feld können Sie in anderen apps Logik.

## <a name="swagger-rest-api-reference"></a>Stolz REST-API-Referenz
Gilt für Version: 1.0.

### <a name="create-file"></a>Datei erstellen
Lädt eine Datei im Feld.  
```POST: /datasets/default/files```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|folderPath|Zeichenfolge|Ja|Abfrage|Keine |Pfad im Feld hochladen|
|Name|Zeichenfolge|Ja|Abfrage|Keine |Name der Datei im Feld erstellen|
|Körper|String(Binary) |Ja|Körper|Keine |Inhalt der Datei im Feld|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="when-a-file-is-created"></a>Wenn eine Datei erstellt
Löst einen, wenn eine neue Datei in einem Ordner erstellt wird.  
```GET: /datasets/default/triggers/onnewfile```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Ordner-ID|Zeichenfolge|Ja|Abfrage|Keine |Eindeutiger Bezeichner des Ordners im Feld|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="copy-file"></a>Datei kopieren
Kopiert eine Datei in ein.  
```POST: /datasets/default/copyFile```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Quelle|Zeichenfolge|Ja|Abfrage|Keine |URL, Datei|
|Ziel|Zeichenfolge|Ja|Abfrage| Keine|Zieldateipfad im Feld Zieldateiname|
|Überschreiben|boolescher Wert|Nein|Abfrage| Keine|Überschreibt die Zieldatei, wenn auf "True" gesetzt|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="delete-file"></a>Datei löschen
Löscht eine Datei aus.  
```DELETE: /datasets/default/files/{id}```


| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|Keine |Eindeutiger Bezeichner der Datei im Feld löschen|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="extract-archive-to-folder"></a>Archiv Ordner
In einem Ordner im Feld Archivdatei extrahiert (Beispiel: ZIP).  
```POST: /datasets/default/extractFolderV2```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Quelle|Zeichenfolge|Ja|Abfrage| |Pfad der Archivdatei|
|Ziel|Zeichenfolge|Ja|Abfrage| |Pfad im Feld Archiv extrahieren|
|Überschreiben|boolescher Wert|Nein|Abfrage| |Überschreibt die Zieldateien, wenn auf "True" gesetzt|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="get-file-content-using-id"></a>Datei-Inhalte mit id
Feld Id entnimmt Dateiinhalt.  
```GET: /datasets/default/files/{id}/content```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|Keine |Eindeutiger Bezeichner der Datei im|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="get-file-content-using-path"></a>Datei-Inhalte mit Pfad
Inhalt aus Pfad abgerufen.  
```GET: /datasets/default/GetFileContentByPath```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Pfad|Zeichenfolge|Ja|Abfrage|Keine |Eindeutige Pfad zur Datei im|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="get-file-metadata-using-id"></a>Abrufen von Metadaten mit id
Ruft Metadaten aus Datei-Id verwenden.  
```GET: /datasets/default/files/{id}```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad| Keine|Eindeutiger Bezeichner der Datei im|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="get-file-metadata-using-path"></a>Abrufen von Metadaten mit Pfad
Dateimetadaten im Feld Pfad abgerufen.  
```GET: /datasets/default/GetFileByPath```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Pfad|Zeichenfolge|Ja|Abfrage|Keine |Eindeutige Pfad zur Datei im|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="update-file"></a>Datei aktualisieren
Aktualisiert eine Datei im.  
```PUT: /datasets/default/files/{id}```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad| Keine|Eindeutiger Bezeichner der Datei im Feld aktualisieren|
|Körper|String(Binary) |Ja|Körper|Keine |Inhalt der Datei im Feld aktualisieren|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


### <a name="when-a-file-is-modified"></a>Wenn eine Datei geändert
Löst einen beim Ändern einer Datei in einem Ordner.  
```GET: /datasets/default/triggers/onupdatedfile```

| Name|Datentyp|Erforderlich|Gespeichert In|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Ordner-ID|Zeichenfolge|Ja|Abfrage|Keine |Eindeutiger Bezeichner des Ordners im Feld|

#### <a name="response"></a>Antwort
|Name|Beschreibung|
|---|---|
|200|Okay|
|Standard|Vorgang ist fehlgeschlagen.|


## <a name="object-definitions"></a>Objektdefinitionen

#### <a name="datasetsmetadata"></a>DataSetsMetadata

|Eigenschaftenname | Datentyp | Erforderlich|
|---|---|---|
|tabellarische|nicht definiert|Nein|
|BLOB|nicht definiert|Nein|

#### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Quelle|Zeichenfolge|Nein|
|displayName|Zeichenfolge|Nein|
|urlEncoding|Zeichenfolge|Nein|
|tableDisplayName|Zeichenfolge|Nein|
|tablePluralName|Zeichenfolge|Nein|

#### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|Quelle|Zeichenfolge|Nein|
|displayName|Zeichenfolge|Nein|
|urlEncoding|Zeichenfolge|Nein|

#### <a name="blobmetadata"></a>BlobMetadata

|Eigenschaftenname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|Name|Zeichenfolge|Nein|
|DisplayName|Zeichenfolge|Nein|
|Pfad|Zeichenfolge|Nein|
|LastModified|Zeichenfolge|Nein|
|Größe|ganze Zahl|Nein|
|Medientyp|Zeichenfolge|Nein|
|IsFolder|boolescher Wert|Nein|
|ETag|Zeichenfolge|Nein|
|FileLocator|Zeichenfolge|Nein|

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).
