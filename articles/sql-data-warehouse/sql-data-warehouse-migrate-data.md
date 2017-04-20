<properties
   pageTitle="Migrieren Sie Ihre Daten mit SQL Data Warehouse | Microsoft Azure"
   description="Tipps zum Migrieren der Daten in Azure SQL Data Warehouse Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="migrate-your-data"></a>Migrieren von Daten
Daten können aus verschiedenen Quellen in SQL Data Warehouse mit verschiedenen Tools verschoben werden.  ADF kopieren, SSIS- und Bcp können alle dafür verwendet werden. Aber wie zunimmt sollten Sie über Sie Datenmigration in Schritte aufteilen. Dies bietet Ihnen die Möglichkeit, jeden Schritt für Leistung und Stabilität zur Sicherstellung einer reibungslosen Migration zu optimieren.

Einfache Migrationsszenarien ADF kopieren, SSIS-und Bcp zunächst erläutert. Dann suchen Sie tiefer in wie die Migration optimiert werden kann.

## <a name="azure-data-factory-adf-copy"></a>Azure Factory (ADF) kopieren
[ADF-Kopie][] ist Teil der [Azure Data Factory][]. ADF-Kopie können Sie Ihre Daten in Flatfiles auf lokalen Speicher zur remote Flatfiles in Azure BLOB-Speicher oder direkt in SQL Data Warehouse exportieren.

Wenn Ihre Daten in Flatfiles beginnt, müssen zunächst Übertragung vor dem Initiieren einer Last Azure-Speicher-Blob in SQL Data Warehouse. Nachdem die Daten in Azure BLOB-Speicher können Sie [ADF][] erneut Verwendung die Daten in SQL Data Warehouse übertragen.

PolyBase bietet auch eine leistungsstarke Option zum Laden der Daten. Allerdings bedeutet zwei Tools anstelle. Wenn Sie die beste Leistung PolyBase verwenden. Wenn Sie eine einziges Tool-Erfahrung (und die Daten nicht massive sind) ist ADF Ihre Antwort.

> [AZURE.NOTE] PolyBase muss die Dateien in UTF-8. Dies ist ADF kopieren Standard Codierung nichts ändern. Dies ist nur eine Erinnerung, das Standardverhalten des ADF Kopie nicht geändert.

Gehen Sie im folgenden Artikel einige hervorragende [Beispiele ADF][].

## <a name="integration-services"></a>Integrationsservices ##
Integration Services (SSIS) ist ein leistungsstarkes und flexibles extrahieren transformieren und laden (ETL) Tool, das komplexe Workflows, Datentransformations- und mehrere Optionen zum Laden von Daten unterstützt. Verwenden Sie SSIS einfach Daten in Azure oder als Teil einer größeren Migration übertragen.

> [AZURE.NOTE] SSIS können in UTF-8 ohne Bytereihenfolge in Datei exportieren. Konfigurieren müssen zunächst Hiermit die abgeleitete Spalte Komponente zu Zeichen im Datenfluss 65001 UTF-8-Codepage verwenden. Nach dem Konvertieren der Spalten schreiben Sie Daten in Ziel Flatfileadapter damit 65001 auch als Codepage für die Datei ausgewählt wurde.

SSIS verbindet mit SQL Data Warehouse, wie sie mit einer SQL Server-Bereitstellung. Allerdings müssen Ihre Verbindung einen Verbindungs-Manager für ADO.NET verwenden. Auch sorgfältig Konfigurieren der "verwenden Bulk Insert wenn verfügbar" Einstellung Durchsatz maximiert. [ADO.NET Zieladapter][] Artikel Weitere Informationen zu dieser Eigenschaft finden Sie

> [AZURE.NOTE] Verbindung mit Azure SQL Data Warehouse mittels OLEDB wird nicht unterstützt.

Darüber hinaus besteht immer die Möglichkeit ein Pakets durch Einschränkung oder Netzwerk fehlschlägt. Design verpackt, damit sie ohne Fehlerpunkt, fortgesetzt werden können vor der abgeschlossenen Arbeiten wiederholen.

Weitere Informationen finden Sie in [SSIS-Dokumentation][].

## <a name="bcp"></a>bcp
BCP ist ein Befehlszeilen-Dienstprogramm, die Flatfile-Daten importieren und exportieren soll. Einige Transformation möglich beim Datenexport. So verwenden einfache Transformationen eine Abfrage auswählen und Transformieren von Daten. Nach dem exportieren, können die Dateien dann direkt in das Ziel SQL Data Warehouse-Datenbank geladen werden.

> [AZURE.NOTE] Es ist häufig sinnvoll, die Transformationen exportieren der Daten in einer Ansicht auf dem Quellsystem kapseln. Dadurch bleibt die Logik und wiederholbar ist.

Bcp bietet folgende Vorteile:

- Der Einfachheit halber. Bcp-Befehle sind einfach zu erstellen und auszuführen
- Startfähige Ladevorgang. Einmal exportiert die Last kann beliebig oft ausgeführt

Nachteile von Bcp sind:

- Bcp arbeitet mit tabellarischen Flatfiles nur. Sie funktioniert nicht mit JSON oder XML-Dateien
- Bcp unterstützt exportieren in UTF-8 nicht. Dies verhindert möglicherweise die Bcp exportiert Daten mit PolyBase
- Transformationsfunktionen sind die Export-Phase auf und sind einfach in der Natur
- Bcp wurde nicht stabil sein, beim Laden von Daten über das Internet an. Instabilitäten Netzwerk möglicherweise ein Fehler beim Laden.
- Bcp verwendet das Schema in der Zieldatenbank vor dem Laden vorhanden

Weitere Informationen finden Sie unter [Bcp zum Laden von Daten in SQL Data Warehouse verwenden][].

## <a name="optimizing-data-migration"></a>Optimieren der Datenmigration
Eine SQLDW-Datenmigration kann effektiv in drei separate Schritte unterteilt werden:

1. Exportieren von Daten
2. Datenübertragung in Azure
3. Laden Sie in der Datenbank SQLDW

Jeder Schritt kann einzeln erstellen, der maximale Leistung bei jedem Schritt robust und stabil startfähige Migrationsprozess optimiert werden.

## <a name="optimizing-data-load"></a>Optimieren von Daten laden
Diese in umgekehrter Reihenfolge für einen Moment betrachten; die schnellste Möglichkeit zum Laden von Daten erfolgt über PolyBase. Optimierung für eine PolyBase Ladevorgang platziert Komponenten in den vorhergehenden Schritten zu verstehen ist im voraus. Sie sind:

1. Von Dateien mit Codierung
2. Dateiformat
3. Speicherort von Dateien

### <a name="encoding"></a>Codierung
PolyBase muss Datendateien UTF-8 codiert. Dies bedeutet, dass es beim export der Daten für diese Anforderung entsprechen muss. Enthält Ihre Daten nur einfache ASCII-Zeichen (keine erweiterten ASCII) dann diese Karte direkt auf UTF-8-Standard und müssen zu viel Codierung sorgen. Wenn Ihre Daten z. B. Umlaute, Akzente oder Symbole oder Ihre Daten nicht lateinischen Sprachen unterstützt Sonderzeichen enthält dann müssen Sie sicherstellen, dass die Exportdateien UTF-8 kodiert sind.

> [AZURE.NOTE] Bcp unterstützt exportiert Daten in UTF-8 nicht. Daher ist die beste Option mit Integration Services oder ADF kopieren für den Datenexport. Es ist erwähnenswert, dass UTF-8 Byte Order Mark (BOM) in der Datei nicht erforderlich ist.

Mit UTF-16 codierten Dateien müssen ***vor*** der Datenübertragung neu geschrieben werden.

### <a name="format-of-data-files"></a>Dateiformat
PolyBase erfordert eine feste Zeilenabschlusszeichen \n oder Zeilenvorschub. Dateien müssen diesem Standard entsprechen. Beschränkungen Abschlusswiderstände Zeichenfolge oder Spalte nicht.

Sie müssen jede Spalte in der Datei als Teil der externen Tabelle in PolyBase definieren. Stellen Sie sicher, dass alle exportierte Spalten erforderlich sind und welche erforderlichen Standards entsprechen.

Siehe die [migrieren Schema] Weitere Informationen zu unterstützten Datentypen.

### <a name="location-of-data-files"></a>Speicherort von Dateien
SQL Data Warehouse verwendet PolyBase Daten ausschließlich von Azure BLOB-Speicher geladen. Daher müssen die Daten zunächst in den BLOB-Speicher übertragen wurden.

## <a name="optimizing-data-transfer"></a>Optimieren von Datenübertragung
Langsamsten Teil der Datenmigration ist die Übertragung der Daten in Azure. Nicht nur Netzwerkbandbreite werden, sondern auch die Zuverlässigkeit des Netzwerks behindern kann ausgeführt. Standardmäßig Datenmigration in Azure deshalb über das Internet Übertragungsfehler wahrscheinlich sind. Diese Fehler erfordern jedoch Daten ganz oder teilweise erneut gesendet werden.

Zum Glück haben Sie mehrere Optionen zur Verbesserung der Geschwindigkeit und Stabilität des Prozesses:

### <a name="expressroute"></a>[ExpressRoute][]
Sie sollten erwägen, [ExpressRoute][] , um die Übertragung zu beschleunigen. [ExpressRoute][] bietet eine private Verbindung in Azure so dass Verbindung nicht über das Internet geht. Dies geht nicht obligatorisch. Aber es verbessert Durchsatz bei Daten in Azure aus einer lokalen oder gemeinsam.

[ExpressRoute][] bietet folgende Vorteile:

1. Erhöhte Zuverlässigkeit
2. Schneller Netzwerk
3. Niedrigere Netzwerklatenz
4. Netzwerksicherheit

[ExpressRoute][] ist für eine Reihe von Szenarien. nicht nur die Migration.

Interessiert? Weitere Informationen und Preise Bitte finden Sie auf [ExpressRoute-Dokumentation][].

### <a name="azure-import-and-export-service"></a>Azure Import / Export-Dienst
Azure Import / Export-Dienst ist eine riesige (TB-) überträgt Daten in Azure für große (GB++) zur Datenübertragung. Dies umfasst Daten auf Datenträger geschrieben und Lieferung an den Azure-Rechenzentrum. Der Festplatteninhalt werden in Azure Storage Blobs in Ihrem Auftrag geladen.

Ein Überblick der Exportvorgang importieren lautet wie folgt:

1. Konfigurieren Sie einen Container Azure BLOB-Speicher zum Empfangen von Daten
2. Exportieren Sie die Daten in den lokalen Speicher
2. Daten Sie die auf 3,5-Zoll SATA II/III-Festplattenlaufwerke mit [Azure Import/Export Tool]
3. Erstellen Sie ein Importauftrag mit Azure Import / Export-Service bietet die Buch.-Dateien [Azure Import/Export Tool]
4. Liefern Sie Datenträger benannte Azure-Rechenzentrum
5. Die Daten in Ihrem Container Azure BLOB-Speicher
6. Laden Sie die Daten in SQLDW mit PolyBase

### <a name="azcopy-utility"></a>[AZCopy][] -Dienstprogramm
[AZCopy][] -Dienstprogramm eignet sich hervorragend für Ihre Daten in Azure Storage Blobs übertragen. Es ist für kleine (MB-), große (GB-) Datenübertragungen entwickelt. [AZCopy] auch soll guten stabilen Durchsatz beim Übertragen von Daten in Azure und so eine hervorragende Wahl für die Übertragung Schritt ist. Einmal übertragen PolyBase in SQL Data Warehouse mit Daten geladen. Sie können auch AZCopy in SSIS-Paketen mit einem Task "Prozess ausführen" integrieren.

Verwendung von AZCopy müssen Sie zuerst herunterladen und installieren. Gibt eine [Produktionsversion][] und einer [Vorschau][] .

Hochladen eine Datei aus dem Dateisystem benötigen Sie einen Befehl wie den folgenden:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Eine allgemeine Zusammenfassung kann:

1. Konfigurieren eines Azure-Speicher BLOB-Containers zum Empfangen von Daten
2. Exportieren Sie die Daten in den lokalen Speicher
3. AZCopy Daten in Azure BLOB-Speicher-container
4. Laden Sie die Daten in SQL Data Warehouse mit PolyBase

Vollständige Dokumentation verfügbar: [AZCopy][].

## <a name="optimizing-data-export"></a>Optimieren von Daten exportieren
Zusätzlich sicherstellen, dass die Anforderungen von PolyBase Export entspricht können Sie auch versuchen, den Export von Daten den Prozess zu optimieren.

> [AZURE.NOTE] PolyBase muss die Daten in UTF-8-Format ist es unwahrscheinlich, dass verwenden Bcp Sie den Export der Daten ausführen. Bcp unterstützt nicht die Ausgabe Dateien in UTF-8. SSIS oder ADF Kopie sind viel besser auf diese Art von Daten exportieren.

### <a name="data-compression"></a>Datenkompression
PolyBase kann mit Gzip komprimierte Daten lesen. Wenn Sie Ihre Daten mit Gzip-Dateien komprimieren können dann minimieren Sie die Datenmenge, die über das Netzwerk.

### <a name="multiple-files"></a>Mehrere Dateien
Große Tabellen in mehrere Dateien unterteilen hilft nicht nur um Export-Geschwindigkeit zu erhöhen, sondern auch mit Übertragung Neustarts und allgemeine Verwaltung der Daten einmal in Azure BLOB-Speicher. Einer der vielen Vorteile der PolyBase ist, dass alle Dateien in einem Ordner lesen als Tabelle behandelt. Deshalb sollten Sie die Dateien für jede Tabelle in einem eigenen Ordner zu isolieren.

PolyBase unterstützt auch das Feature "rekursive Ordner Traversal" genannt. Dieses Feature können Sie weiter die Organisation die exportierten Daten zu Daten-Management.

Finden Sie Informationen zum Laden von Daten mit PolyBase [PolyBase zum Laden von Daten in SQL Data Warehouse verwenden][].


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Migration finden Sie unter [Migrieren der Lösung zu SQL Data Warehouse][].
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Anwendungsentwicklung][].

<!--Image references-->

<!--Article references-->
[AZCopy]: ../storage/storage-use-azcopy.md
[ADF-Kopie]: ../data-factory/data-factory-data-movement-activities.md 
[ADF-Beispiele]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[Übersicht über die Anwendungsentwicklung]: sql-data-warehouse-overview-develop.md
[Migrieren der Lösung zu SQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Verwenden Sie Bcp zum Laden von Daten in SQL Data Warehouse]: sql-data-warehouse-load-with-bcp.md
[Verwenden Sie zum Laden von Daten in SQL Data Warehouse PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute-Dokumentation]: http://azure.microsoft.com/documentation/services/expressroute/

[Produktionsflussversion]: http://aka.ms/downloadazcopy/
[Vorschau]: http://aka.ms/downloadazcopypr/
[Zieladapter ADO.NET]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS-Dokumentation]: https://msdn.microsoft.com/library/ms141026.aspx
