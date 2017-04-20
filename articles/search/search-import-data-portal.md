<properties
    pageTitle="Importieren von Daten in Azure Search Indexer in Azure-Portal mit | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Verwenden Sie Azure Suche Datenimport-Assistenten in Azure-Portal Crawl Daten von Azure BLOB-Speicher, Tabelle weiteres SQL-Datenbank und SQL Server auf Azure VMs."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="heidist"/>

# <a name="import-data-to-azure-search-using-the-portal"></a>Importieren von Daten in Azure Suche über das portal

Azure-Portal bietet einen **Datenimport** -Assistenten für das Dashboard Azure Suche zum Laden von Daten in einen Index. 

  ![Importieren Sie Daten auf der Befehlsleiste][1]

Intern wird der Assistent konfiguriert und ruft einen *Indexer*mehrere Schritte des Indizierungsprozesses automatisieren: 

- Verbinden Sie mit einer externen Datenquelle in Azure Abonnement
- AutoGenerate ein IndexSchema basierend auf der Quelle Datenstruktur
- Erstellen Sie Dokumente aus der Datenquelle abgerufenen Rowsets
- Der Index im Suchdienst hochladen Sie Dokumente

Sie versuchen, dieses Workflows mit Beispieldaten in DocumentDB. Informationen finden Sie unter [Erste Schritte mit Azure Suche im Azure-Portal](search-get-started-portal.md) .

## <a name="data-sources-supported-by-the-import-data-wizard"></a>Der Datenimport-Assistent unterstützt Datenquellen

Der Import-Assistent unterstützt die folgenden Datenquellen: 

- SQL Azure-Datenbank
- Relationale Daten auf eine VM Azure SQL Server
- Azure DocumentDB
- Azure BLOB-Speicher (in der Vorschau)
- Azure Table Storage (in der Vorschau)

Vereinfachten Datasets ist eine Eingabe erforderlich. Sie können nur aus einer einzelnen Tabelle, eine Ansicht oder eine entsprechende Datenstruktur. Sie sollten diese Datenstruktur erstellen, vor dem Ausführen des Assistenten.

Beachten Sie, dass der Indexer noch in Vorschau sind d. h. Indexer Definition durch die Vorabversion der API unterstützt wird. Weitere Informationen und Links finden Sie unter [Übersicht über Indexer](search-indexer-overview.md) .

## <a name="connect-to-your-data"></a>Verbindung zu Ihren Daten

1. Melden Sie sich bei [Azure-Portal](https://portal.azure.com) und offenen Service Dashboard. **Suchdienste** klicken Sie in der Navigationsleiste Dienste in das aktuelle Abonnement anzeigen. 

2. Klicken Sie auf der Befehlsleiste auf Folie öffnen Blade-Daten importieren **Daten importieren** .  

3. Klicken Sie auf **Verbinden, um Daten** zum Angeben einer Datenquellendefinition Indexer verwendet. Intra-Abonnement Datenquellen kann Assistenten normalerweise erkennen und Lesen Verbindungsinformationen Gesamtbedarf Konfiguration minimiert.

| | |
|--------|------------|
|**Datenquelle** | Haben Sie bereits Indexer Suchdienst definiert, können Sie eine vorhandene Datendefinition für einen weiteren Import auswählen.|
|**SQL Azure-Datenbank** | Dienstnamen, Anmeldeinformationen für einen Datenbankbenutzer mit Leseberechtigung und Datenbanknamen können auf der Seite oder über eine ADO.NET Verbindungszeichenfolge angegeben werden. Die Option Verbindung Zeichenfolge anzeigen oder Eigenschaften anpassen. <br/><br/>Die Tabelle oder Ansicht, die das Rowset bietet auf der Seite anzugeben. Diese Option wird nach erfolgreicher Verbindung damit eine Dropdown Liste eine Auswahl treffen kann.|
|**SQL Server auf Azure VM** | Geben Sie einen vollqualifizierten Namen, Benutzer-ID und Kennwort und Datenbank als Verbindungszeichenfolge. Um diese Datenquelle zu verwenden, müssen Sie zuvor ein Zertifikat im lokalen Speicher installiert haben, die die Verbindung verschlüsselt. <br/><br/>Die Tabelle oder Ansicht, die das Rowset bietet auf der Seite anzugeben. Diese Option wird nach erfolgreicher Verbindung damit eine Dropdown Liste eine Auswahl treffen kann.
|**DocumentDB** |Dazu gehören Konto, Datenbank und Auflistung. Alle Dokumente in der Auflistung werden im Index enthalten. Sie können eine Abfrage zu reduzieren oder Rowset filtern oder geänderte Dokumente für nachfolgende Datenaktualisierungsvorgänge erkennen.|
|**Azure BLOB-Speicher** | Dazu gehören das Speicherkonto und Container. Optional, wenn BLOB-Namen eine virtuelle Gruppierung Zwecken Namenskonvention, soll virtuelles Verzeichnis Teil des Namens als Container Ordner. Weitere Informationen finden Sie unter [Indizierung BLOB-Speicher (Vorschau)](search-howto-indexing-azure-blob-storage.md) . |
|**Azure Table Storage** | Dazu gehören das Speicherkonto und ein Tabellenname. Optional können Sie eine Abfrage, um eine Teilmenge der Tabellen abrufen. Weitere Informationen finden Sie unter [Indizierung Tabellenspeicher (Vorschau)](search-howto-indexing-azure-tables.md) . |

## <a name="customize-target-index"></a>Zielindex anpassen

Vorläufiger Index wird in der Regel aus dem Dataset abgeleitet. Hinzufügen, bearbeiten oder Löschen von Feldern zum Vervollständigen des Schemas. Außerdem legen Sie Attribute auf Feldebene seiner nachfolgenden Suchverhalten fest.

1. **Zielindex anpassen**Geben Sie den Namen und einem **Schlüssel** jedes Dokument eindeutig identifiziert. Der Schlüssel muss eine Zeichenfolge sein. Feldwerte enthält Leerzeichen oder Bindestriche müssen Sie erweiterte Optionen **Importieren der Daten** die Überprüfung für diese Zeichen zu festzulegen.

2. Überprüfen Sie und Überarbeiten Sie die verbleibenden Felder. Feldname und-Typ sind in der Regel für Sie ausgefüllt. Sie können den Datentyp ändern.

3. Legen Sie Indexattribute für jedes Feld:

 - Abrufbar zurückgegeben das Feld in den Suchergebnissen.
 - Filterbaren Feld Filterausdrücke referenziert werden können.
 - Sortierbare Feld sortiert verwendet werden können.
 - Facetable kann das Feld für die facettierte Navigation.
 - Volltext-Suche durchsucht werden können.
  
4. Wenn einen Sprache Analyzer auf Feldebene angeben möchten, klicken Sie auf Registerkarte **Analyzer** . Sprache-Analyzer können zu diesem Zeitpunkt angegeben werden. Mithilfe von benutzerdefinierten Analyzer oder eine andere Sprache Analyzer Schlüsselwort, Muster und So weiter ist Code erforderlich.

 - Klicken Sie auf **durchsuchbar** zu bestimmen Volltextsuche im Feld Liste Analyzer aktivieren.
 - Wählen Sie den gewünschte Analyzer. Einzelheiten finden Sie unter [Erstellen eines Indexes für Dokumente in mehreren Sprachen](search-language-support.md) .

5. Klicken Sie auf **Suggester** um Typeahead-Abfragevorschläge für ausgewählte Felder zu aktivieren.


## <a name="import-your-data"></a>Daten importieren

1. Geben Sie einen Namen für den Indexer **Import Ihrer Daten**. Beachten Sie, dass das Produkt des Datenimport-Assistenten einen Indexer. Später, wenn Sie anzeigen oder bearbeiten möchten, wählen Sie es über das Portal nicht durch den Assistenten erneut aus. 

2. Geben Sie den Zeitplan auf regionale Zeitzone basiert, in der der Dienst bereitgestellt wird.

3. Festlegen Sie erweiterter Optionen an Schwellenwerte auf, ob die Indizierung fortgesetzt werden kann ein Dokument abgelegt. Darüber hinaus können Sie angeben, ob **Schlüsselfelder** Leerzeichen und Schrägstriche enthalten dürfen.  

## <a name="edit-an-existing-indexer"></a>Vorhandenen Index bearbeiten

Doppelklicken Sie im Dashboard Service auf Indexer Tile Folie eine Liste aller Indexer für Ihr Abonnement erstellt. Doppelklicken Sie auf einen Indexer ausführen, bearbeiten oder löschen. Sie können einen vorhandenen Index ersetzen die Datenquelle ändern und Festlegen von Optionen für Schwellenwerte für Fehler bei der Indizierung.

## <a name="edit-an-existing-index"></a>Einen vorhandenen Index bearbeiten

In Azure Suche erfordert strukturelle Updates auf einen Index, Index neu zu erstellen Index löschen und Neuerstellen des Indexes Neuladen der Daten besteht. Strukturelle Updates umfassen Daten ändern und Umbenennen oder Löschen eines Felds.

Änderungen, die eine Wiederherstellung benötigen enthalten Hinzufügen eines neuen Felds ändern scoring Profile Suggesters ändern oder Analysatoren Sprache ändern. Weitere Informationen finden Sie unter [Index aktualisieren](https://msdn.microsoft.com/library/azure/dn800964.aspx) .

## <a name="next-step"></a>Nächstes

Überprüfen Sie diese Links, um weitere Informationen über Indexer:

- [Indizierung von Azure SQL-Datenbank](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB Indizierung](../documentdb/documentdb-search-indexer.md)
- [Indizierung BLOB-Speicher (Vorschau)](search-howto-indexing-azure-blob-storage.md)
- [Indizierung Tabellenspeicher (Vorschau)](search-howto-indexing-azure-tables.md)



<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

