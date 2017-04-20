<properties
pageTitle="Azure BLOB-Speicher mit Azure Suche indizieren"
description="Erfahren Sie, wie Azure BLOB-Speicher indiziert und extrahiert Text aus Dokumenten mit Azure suchen"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="10/16/2016"
ms.author="eugenesh" />

# <a name="indexing-documents-in-azure-blob-storage-with-azure-search"></a>Dokumente in Azure BLOB-Speicher mit Azure Suche indizieren

Dieser Artikel beschreibt, wie Sie Azure Suche zum Indizieren von Dokumenten (wie PDF-Dokumente, Microsoft Office-Dokumente und mehrere andere gängige Formate) in Azure BLOB-Speicher gespeichert. Neue Suche Azure Blob Indexer kann dabei nahtlos. 

> [AZURE.IMPORTANT] Diese Funktion ist derzeit in der Vorschau. Sie steht nur in der Version **2015-02-28-Vorschau**REST-API. Bitte denken Sie daran, Vorschau APIs für Test- und Evaluierungszwecke vorgesehen und sollte nicht in der Produktion verwendet werden.

## <a name="supported-document-formats"></a>Unterstützte Dokumentformate

BLOB-Indexer kann Text aus folgenden Dokumentformate zu extrahieren:

- PDF-DATEI
- Microsoft Office-Formate: DOCX/DOC, XLSX/XLS, PPT/PPTX, MSG (Outlook e-Mails)  
- HTML
- XML
- ZIP
- EML
- Textdateien  
- JSON (Einzelheiten [Indizierung JSON Blobs](search-howto-index-json-blobs.md) )
- CSV (Einzelheiten [Indizierung CSV-Blobs](search-howto-index-csv-blobs.md) )

## <a name="setting-up-blob-indexing"></a>Einrichten von BLOB-Indizierung

Sie können eine Azure BLOB-Speicher Indexer eingerichtet:

- [Azure-portal](https://ms.portal.azure.com)
- Azure Suche [REST-API](https://msdn.microsoft.com/library/azure/dn946891.aspx)
- Azure Suche .NET SDK, [Version 2.0-Vorschau](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) 

> [AZURE.NOTE] Einige Funktionen (z. B. Feld Zuordnung) noch nicht im Portal und programmgesteuert verwendet werden. 

In diesem Artikel richten wir einen Indexer, der REST-API drei Schritte: eine Datenquelle erstellen, erstellen Sie einen Index, Indexer konfigurieren.

### <a name="step-1-create-a-data-source"></a>Schritt 1: Erstellen einer Datenquelle

Eine Datenquelle gibt die Daten für den index, Anmeldeinformationen Zugriff auf Daten und Richtlinien effizient identifizieren, Änderungen in den Daten (neue, geänderte oder gelöschte Zeilen). Eine Datenquelle kann durch mehrere Indexer in derselben Suchdienst verwendet werden.

BLOB-Indizierung müssen die Datenquelle erforderlichen Eigenschaften: 

- **Name** ist der eindeutige Name der Datenquelle im Suchdienst. 

- **Typ** muss `azureblob`. 

- **Anmeldeinformationen** stellt die Storage-Konto Verbindungszeichenfolge der `credentials.connectionString` Parameter. Erhalten Sie die Verbindungszeichenfolge von Azure-Portal zu gewünschten Speicher Konto Blade > **Einstellungen** > **Schlüssel** und Wert "Primäre Verbindungszeichenfolge" oder "Sekundäre Verbindungszeichenfolge" verwenden. 

- **Container** gibt einen Container in Ihres Speicherkontos. Standardmäßig können alle Blobs im Container abgerufen werden. Möchten Sie nur Index Blobs in einem bestimmten virtuellen Verzeichnis, soll das Verzeichnis den optionalen **Abfrage** -Parameter verwenden. 

Das folgende Beispiel veranschaulicht eine Datenquellendefinition:

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Weitere API Datasource erstellen finden Sie unter [Datenquelle erstellen](search-api-indexers-2015-02-28-preview.md#create-data-source).

### <a name="step-2-create-an-index"></a>Schritt 2: Erstellen eines Indexes 

Der Index gibt die Felder in einem Dokument Attribute und andere Konstrukte, die shape-Suche Erfahrung.  

Für BLOB-Indizierung werden Sie sicher, dass der Index eine durchsuchbare `content` Feld zum Speichern von BLOBs.

    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-target-index",
        "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
        ]
    }

Weitere API Index erstellen finden Sie unter [Create Index](https://msdn.microsoft.com/library/dn798941.aspx)

### <a name="step-3-create-an-indexer"></a>Schritt 3: Erstellen eines Indexers 

Indexer Datenquellen mit Ziel Suchindizes verbindet und Planungsinformationen bietet, sodass Daten aktualisieren zu automatisieren. Nachdem der Index und die Datenquelle erstellt wurde, ist relativ einfach, Indexer erstellen, der die Datenquelle und einen Zielindex verweist. Zum Beispiel:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Mit diesem Indexer führen alle zwei Stunden (Intervall auf "PT2H" festgelegt ist). Führen Sie einen Indexer Intervall alle 30 Minuten zu "PT30M". Kürzeste unterstützten Intervall beträgt 5 Minuten. Optional – nicht angegeben ist, ein Indexer wird nur einmal beim. Jedoch können Sie ein Indexer bei Bedarf jederzeit ausführen.   

Weitere Informationen zu API Indexer erstellen anzeigen Sie [Indexer erstellen](search-api-indexers-2015-02-28-preview.md#create-indexer)


## <a name="document-extraction-process"></a>Dokument-Extraktion

Azure Suche indiziert jedes Dokument (Blob) wie folgt:

- Der gesamte Inhalt des Dokuments wird in ein Zeichenfolgenfeld mit dem Namen `content`. Wir unterstützen nicht aktuell mehrere Dokumente über ein einzelnes Blob extrahieren:
    - Beispielsweise wird eine CSV-Datei als ein einzelnes Dokument indiziert. Möchten Sie jede Zeile in eine CSV-Datei als separates Dokument behandelt, stimmen Sie für [UserVoice Vorschlag](https://feedback.azure.com/forums/263029-azure-search/suggestions/13865325-please-treat-each-line-in-a-csv-file-as-a-separate).
    - Verknüpfte oder eingebettete Dokument (z. B. einem ZIP-Archiv oder ein Word-Dokument mit eingebetteten Outlook e-Mail mit Anlagen) wird auch als ein einzelnes Dokument indiziert.

- Benutzerdefinierte Metadaten-Eigenschaften auf das Blob werden ggf. wörtlich extrahiert. Metadaten-Eigenschaften können auch bestimmte Aspekte der Extraktion Dokument steuern – Näheres [Verwenden benutzerdefinierte Metadaten für Steuerelement Dokument](#CustomMetadataControl) verwendet werden.

- Standard-Blob Metadateneigenschaften sind in die folgenden Felder extrahiert:

    - **Metadaten\_Speicher\_Namen** (Edm.String) - der Dateiname des Blob. Beispielsweise haben Sie eine BLOB-/my-container/my-folder/subfolder/resume.pdf der Wert dieses Felds ist `resume.pdf`.

    - **Metadaten\_Speicher\_Pfad** (Edm.String) – die vollständige URI des BLOBs, einschließlich das Speicherkonto. Zum Beispiel`https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`

    - **Metadaten\_Speicher\_Inhalt\_Typ** (Edm.String) - Inhaltstyp laut Blob hochladen verwendeten Code. Z. B. `application/octet-stream`.

    - **Metadaten\_Speicher\_letzten\_geändert** (Edm.DateTimeOffset) - Zeitstempel für das Blob geändert. Azure Suche wird dieser Timestamp zu geänderten Blobs vermeiden Indizierung alles nach der anfänglichen Indizierung verwendet.

    - **Metadaten\_Speicher\_Größe** (Int64) - BLOB-Größe in Bytes.

    - **Metadaten\_Speicher\_Inhalt\_md5** (Edm.String) - MD5-Hash BLOB-Inhalt, falls verfügbar.

- Jedes Dokumentformat Metadateneigenschaften werden in der aufgelisteten [hier](#ContentSpecificMetadata)extrahiert.

Sie müssen Felder für alle oben genannten Eigenschaften im Suchindex definieren – erfassen Sie die Eigenschaften für die Anwendung. 

> [AZURE.NOTE] Häufig werden die Feldnamen in den vorhandenen Index von Feldnamen bei Dokument generiert. **Feld Zuordnung** können Sie die Feldnamen im Suchindex von Azure Search zur Eigenschaftennamen zugeordnet. Sie sehen ein Beispiel für Feld Zuordnung dafür verwenden. 

## <a name="picking-the-document-key-field-and-dealing-with-different-field-names"></a>Feld Schlüssel Entnahme und Umgang mit anderen Feldnamen

In Azure Search identifiziert die Dokumentschlüssel eindeutig ein Dokument. Jede Suchindex muss genau ein Feld vom Typ Edm.String. Das Feld ist für jedes Dokument, das den Index hinzugefügt wird (es ist tatsächlich das einzige Pflichtfeld) erforderlich.  
   
Sie sollten sorgfältig überlegen, das Feld für den Index extrahierten Feld zuordnen soll. Die Bewerber sind:

- **Metadaten\_Speicher\_Namen** - praktische Kandidat möglicherweise jedoch (1) die Namen nicht eindeutig sein können wie Sie Blobs mit demselben Namen in unterschiedlichen Ordnern, 2) der Name enthält Zeichen, die im Dokument Tasten, z. B. Bindestriche ungültig sind. Ungültige Zeichen beschäftigen, mit der `base64Encode` [Feld Zuordnungsfunktion](search-indexer-field-mappings.md#base64EncodeFunction) - Wenn Sie dies tun, denken Sie daran, Dokumentschlüssel codieren ruft wie Lookup-API übergeben. (Z. B. in .NET [UrlTokenEncode Methode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) für diesen Zweck können Sie).

- **Metadaten\_Speicher\_Pfad** - Eindeutigkeit gewährleistet mit dem vollständigen Pfad jedoch definitiv enthält der Pfad `/` Zeichen [in einem Dokumentschlüssel ungültig](https://msdn.microsoft.com/library/azure/dn857353.aspx).  Wie oben beschrieben, haben Sie die Möglichkeit Schlüssel mit Codierung der `base64Encode` [Funktion](search-indexer-field-mappings.md#base64EncodeFunction).

- Wenn keine der obigen Optionen für Sie arbeiten, können Sie eine benutzerdefinierte Metadateneigenschaft Blobs hinzufügen. Diese Option jedoch erfordert Ihre Blob Uploadvorgang alle Blobs, Metadateneigenschaft hinzufügen. Da der Schlüssel eine erforderliche Eigenschaft ist fehl alle Blobs, die diese Eigenschaft nicht indiziert werden.

> [AZURE.IMPORTANT] Ist keine explizite Zuordnung für das Schlüsselfeld im Index, verwendet Azure Suche automatisch `metadata_storage_path` als Schlüssel und Base64-codiert Schlüsselwerte (zweite Option oben).

Nehmen wir beispielsweise die `metadata_storage_name` Feld als Dokumentschlüssel. Nehmen wir ferner an Index ist ein Schlüsselfeld mit dem Namen `key` und `fileSize` zum Speichern der Dokumentgröße. Um Dinge wie gewünscht zu verbinden, geben Sie das folgende Feld Zuordnung beim Erstellen oder Aktualisieren der Indexer:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

Zu dieser insgesamt ist hier das Feld hinzufügen und Base64-Codierung der Schlüssel für einen vorhandenen Index zu aktivieren:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }

> [AZURE.NOTE] Informationen zum Feld Zuordnung finden Sie [in diesem Artikel](search-indexer-field-mappings.md).

## <a name="incremental-indexing-and-deletion-detection"></a>Inkrementell indizieren und löschen-Erkennung

Beim Einrichten der nach einem Zeitplan ausführen ein BLOB-Index neu indiziert nur die geänderten Blobs gemäß des BLOBs `LastModified` Zeitstempel.

> [AZURE.NOTE] Müssen eine Erkennung Politik angeben – inkrementell indizieren Sie automatisch aktiviert.

Gehen Sie zum Löschen von Dokumenten unterstützen vor "weichen löschen". Beim Löschen sofort Blobs werden Unterlagen nicht aus dem Suchindex entfernt. Verwenden Sie stattdessen die folgenden Schritte aus:  

1. Das Blob Azure Search an, dass es logisch gelöscht wird fügen Sie eine benutzerdefinierte Metadateneigenschaft hinzu
2. Konfigurieren einer Richtlinie für weiche löschen Erkennung der Datenquelle
3. Nachdem der Indexer Blob (dargestellt durch den Indizierungsstatus API) verarbeitet hat, können Sie physisch Blob löschen

Beispielsweise die folgende Richtlinie betrachtet ein Blob gelöscht werden, hat eine Metadateneigenschaft `IsDeleted` mit dem Wert `true`:

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",   
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }   

<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>Content-spezifischen Metadaten-Eigenschaften

In der folgenden Tabelle zusammengefasst Verarbeitungsschritte für jedes Dokument und von Azure Suche extrahierten Metadateneigenschaften beschreibt.

Dokument / Inhaltstyp | Inhaltstyp bestimmte Metadaten-Eigenschaften | Verarbeitungsdetails
-------------------------------|-------------------------------------------|-------------------
HTML (`text/html`) | `metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` | Entfernen Sie HTML-Code und extrahieren text
PDF (`application/pdf`) | `metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title`| Extrahieren von Text, z. B. eingebettete Dokumente (außer Bilder)
DOCX-Format (application/vnd.openxmlformats-officedocument.wordprocessingml.document) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` | Extrahieren Sie Text mit eingebetteten Dokumenten
DOC (Application/Msword) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` | Extrahieren Sie Text mit eingebetteten Dokumenten
XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` | Extrahieren Sie Text mit eingebetteten Dokumenten
XLS (Application/vnd.ms-excel) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` | Extrahieren Sie Text mit eingebetteten Dokumenten
PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` | Extrahieren Sie Text mit eingebetteten Dokumenten
PPT (Application/vnd.ms-Powerpoint) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` | Extrahieren Sie Text mit eingebetteten Dokumenten
MSG (Application/vnd.ms-Outlook) | `metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` | Extrahiert Text, einschließlich Anlagen
ZIP (Anwendung/Zip) | `metadata_content_type` | Extrahiert Text aus allen Dateien im Archiv
XML (Application/Xml) | `metadata_content_type`</br>`metadata_content_encoding`</br> | XML-Markup entfernt und Text extrahieren
JSON (Application/Json) | `metadata_content_type`</br>`metadata_content_encoding` | Text extrahieren<br/>Hinweis: Benötigen Sie mehrere Dokument von einem Blob JSON extrahieren, Einzelheiten Sie [Indizierung JSON blobs](search-howto-index-json-blobs.md)
EML (Message/rfc822) | `metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` | Extrahiert Text, einschließlich Anlagen
Nur Text (Text/Plain) | `metadata_content_type`</br>`metadata_content_encoding`</br> | 

<a name="CustomMetadataControl"></a>
## <a name="using-custom-metadata-to-control-document-extraction"></a>Benutzerdefinierte Metadaten zum Steuerelement Dokument extrahieren

Sie können bestimmte Aspekte des BLOB-Indizierung und Dokument extrahieren Blob Metadateneigenschaften hinzufügen. Derzeit werden die folgenden Eigenschaften unterstützt:

Eigenschaftenname | Eigenschaftswert | Erklärung
--------------|----------------|------------
AzureSearch_Skip | "true" | Wird den Indexer Blob Blob komplett überspringen; Metadaten weder Inhalt Extraktion wird versucht. Dies empfiehlt sich, wenn Sie bestimmte Inhaltstypen überspringen möchten oder wenn ein Blob wiederholt und unterbricht die Indizierung.
AzureSearch_SkipContent | "true" | Weist den Indexer BLOB-Index nur die Metadaten und Extrahieren von Inhalt des BLOBs überspringen. Dies ist nützlich, wenn BLOB-Inhalt nicht interessant ist, aber trotzdem index Metadaten Blob zugeordnet.

<a name="IndexerParametersConfigurationControl"></a>
## <a name="using-indexer-parameters-to-control-document-extraction"></a>Verwenden Indexerparameter Dokument extrahieren steuern

Mehrere Indexer-Konfiguration sind Parameter zur Steuerung der blobs und welche Teile eines BLOBs und Metadaten werden indiziert. 

### <a name="index-only-the-blobs-with-specific-file-extensions"></a>Die Blobs mit bestimmten Dateitypen indizieren

Indizieren Sie nur die Blobs mit Dateinamenerweiterungen mit Angabe der `indexedFileNameExtensions` Indexer Konfigurationsparameter. Der Wert ist eine Zeichenfolge mit einer kommagetrennten Liste Datei Extensions (mit einem vorangestellten Punkt). Auf Index nur die. PDF und. DOCX Blobs folgendermaßen vor: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions-from-indexing"></a>Indizierung Blobs mit bestimmten Dateitypen ausschließen

Sie ausschließen, dass Blobs mit Namen Datei mithilfe der `excludedFileNameExtensions` Konfigurationsparameter. Der Wert ist eine Zeichenfolge mit einer kommagetrennten Liste Datei Extensions (mit einem vorangestellten Punkt). Auf alle Blobs solchen mit einem außer Index der. PNG und. JPEG-Erweiterungen folgendermaßen vor: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Wenn beide `indexedFileNameExtensions` und `excludedFileNameExtensions` Parameter vorhanden sind, Azure Suche sucht zunächst am `indexedFileNameExtensions`, dann `excludedFileNameExtensions`. Das heißt, wenn die Erweiterung in beiden Listen vorhanden ist, es von der Indizierung ausgeschlossen wird. 

### <a name="index-storage-metadata-only"></a>Index-Speichermetadaten

Indizieren Sie nur die Speichermetadaten und komplett überspringen Dokument extrahieren Prozess verwendet das `indexStorageMetadataOnly` Eigenschaft. Dies ist nützlich, wenn Sie brauchen nicht den Dokumentinhalt benötigen Sie Content-spezifischen Metadaten-Eigenschaften. Legen Sie dazu die `indexStorageMetadataOnly` -Eigenschaft auf `true`: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexStorageMetadataOnly" : true } }
    }

### <a name="index-both-storage-and-content-type-metadata-but-skip-content-extraction"></a>Speicher und Inhaltstyp Metadaten indizieren, aber Inhalt Extraktion überspringen

Möchten Sie alle Metadaten extrahieren aber Content Extraktion für alle Blobs überspringen, können dieses Verhalten mit der Konfiguration Indexer hinzufügen müssen anfordern `AzureSearch_SkipContent` Metadaten zu jedem blob einzeln. Legen Sie dazu die `skipContent` Konfiguration Indexereigenschaft, `true`: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "skipContent" : true } }
    }

## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns, Azure Suche zu verbessern.

Wenn Sie Wünsche oder Verbesserungsvorschläge haben, bitte erreichen Sie uns auf unserer [Website UserVoice](https://feedback.azure.com/forums/263029-azure-search/).
