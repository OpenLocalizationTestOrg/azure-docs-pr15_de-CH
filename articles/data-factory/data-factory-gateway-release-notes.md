<properties 
    pageTitle="Versionsinformationen für Data Management Gateway | Azure Data Factory" 
    description="Data Management Gateway Geschichte-Versionsinformationen" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="spelluru"/>

# <a name="release-notes-for-data-management-gateway"></a>Versionshinweise für Data Management Gateway
Eine moderne Datenintegration ist nahtlos Daten zu und von Betrieben in die cloud verschieben. Data Factory ist Integration nahtlos mit Data Management Gateway ein Agent ist die Installation lokal Hybrid-Datentransfer aktivieren.

Finden Sie in folgenden Artikeln Informationen Data Management Gateway und seine Verwendung: 

- [Data Management Gateway](data-factory-data-management-gateway.md)
- [Verschieben von Daten zwischen lokalen und cloud mit Azure Data Factory](data-factory-move-data-between-onprem-and-cloud.md) 

## <a name="current-version-2260721"></a>Aktuelle Version (2.2.6072.1)

- Unterstützt das Festlegen von HTTP-Proxy für das Gateway über den Gateway-Konfigurations-Manager. Wenn konfiguriert, Azure Blob, Azure Tabelle Azure Data Lake und Dokument DB über HTTP-Proxy zugegriffen werden.
- Unterstützt Verfahrensweise für Header TextFormat beim Kopieren von Daten aus und in Azure Blob Datenspeicher See Azure lokalen Dateisystem und lokal bietet.
- Unterstützt das Kopieren von Daten aus Blob Anfügen und Seite Blob neben den bereits unterstützten Block-Blob.
- Stellt einen neuen Gateway Status **Online (begrenzt)**gibt an, dass die wichtigste Funktionen des Gateways außer interaktiver Vorgang Unterstützung für Assistenten zum Kopieren von funktioniert.
- Verbessert die Stabilität des Gateway Registrierung Registrierungsschlüssel.

## <a name="earlier-versions"></a>Frühere Versionen

## <a name="2160401"></a>2.1.6040.1

- DB2-Treiber ist nun in Gateway-Installationspaket enthalten. Sie müssen nicht separat installieren. 
- DB2-Treiber unterstützt nun Z/OS und DB2 für i (AS / 400) neben den bereits unterstützten Plattformen (Linux, Unix und Windows). 
- Unterstützt die Verwendung von DocumentDB als Quelle oder Ziel für lokalen Datenspeicher
- Kopieren von Daten von / / kalt unterstützt blob-Speicher mit den bereits unterstützten allgemeinen Speicherkonto. 
- Ermöglicht die Verbindung mit lokalen SQL Server über Gateway mit Remotebenutzernamen Berechtigungen.  

## <a name="2060131"></a>2.0.6013.1

- Wählen Sie die Sprache/Kultur während der manuellen Installation von Gateway verwendet werden.
- Gateway nicht erwartungsgemäß funktioniert, Sie können beim Microsoft vereinfachen die Fehlerbehebung des Problems Gateway Protokolle der letzten sieben Tage an. Wenn Gateway nicht mit Cloud-Dienst verbunden ist, können Sie speichern und archivieren Sie Gateway-Protokolle.  
- Verbesserte Benutzeroberfläche für Gateway-Konfigurations-Manager:
    - Machen Sie Gateway-Status auf der Registerkarte Startseite mehr sichtbar.
    - Steuerelemente neu strukturiert und vereinfacht.
- Sie können Daten aus einem Speicher mit [codefreien Vorschau kopieren](data-factory-copy-data-wizard-tutorial.md). Einzelheiten finden Sie unter [Kopieren bereitgestellt](data-factory-copy-activity-performance.md#staged-copy) darüber im Allgemeinen. 
- Data Management Gateway eingehende Daten direkt von einer lokalen SQL Server-Datenbank können in Azure maschinelles lernen.
- Verbesserte Leistung
    - Verbessern der Leistung bei Schema/Vorschau für SQL Server in Kopie codefreien Vorschau anzeigen.



## <a name="11259531"></a>1.12.5953.1
- Fehlerkorrekturen

## <a name="11159181"></a>1.11.5918.1

- Maximale Größe des Ereignisprotokolls Gateway wurde von 1 MB, 40 MB erhöht.
- Ein Warndialogfeld wird angezeigt, falls während Gateway automatische Aktualisierung ein Neustart erforderlich ist. Sie können rechts klicken oder später neu starten. 
- Bei automatische Aktualisierung fehlschlägt, wiederholt Gateway Installer dreimal maximal automatisch aktualisiert.
- Verbesserte Leistung
    - Verbesserung der Performance zum Laden großer Tabellen von lokalen Server kopieren codefreien Szenario.
- Fehlerkorrekturen

## <a name="11058921"></a>1.10.5892.1

- Verbesserte Leistung
- Fehlerkorrekturen

## <a name="1958652"></a>1.9.5865.2

- Zero Touch automatische Aktualisierungsfunktion
- Neues Taskleistensymbol mit Gateway-Statusanzeigen
- Möglichkeit vom Client auf "Jetzt aktualisieren"
- Aktualisierungszeit Zeitplan festlegen
- PowerShell-Skript für die automatische Aktualisierung ausschalten umschalten
- Unterstützung für JSON-format  
- Verbesserte Leistung
- Fehlerkorrekturen

## <a name="1858221"></a>1.8.5822.1

- Problembehandlung verbessern
- Verbesserte Leistung
- Fehlerkorrekturen

### <a name="1757951"></a>1.7.5795.1

- Verbesserte Leistung
- Fehlerkorrekturen

### <a name="1757641"></a>1.7.5764.1

- Verbesserte Leistung
- Fehlerkorrekturen

### <a name="1657351"></a>1.6.5735.1

- Support vor Ort bietet Quelle/Senke
- Verbesserte Leistung
- Fehlerkorrekturen

### <a name="1656961"></a>1.6.5696.1

- Verbesserte Leistung
- Fehlerkorrekturen

### <a name="1656761"></a>1.6.5676.1

- Support-Diagnosetools in Configuration Manager
- Spalten für tabellarische Datenquellen für Azure Data Factory unterstützt
- Unterstützung DW SQL Azure Data Factory
- Unterstützung für Azure Data Factory zurückgezogen in BlobSource und FileSource
- Unterstützung von CopyBehavior, MergeFiles, PreserveHierarchy und FlattenHierarchy in BlobSink und FileSink mit binären Kopie für Azure Data Factory
- Kopieren-Aktivität Fortschrittsbericht für Azure Data Factory unterstützt
- Unterstützt Konnektivität Überprüfung der Datenquelle für Azure Data Factory
- Fehlerkorrekturen


### <a name="1656721"></a>1.6.5672.1

- Name der ODBC-Datenquelle für Azure Data Factory unterstützt
- Verbesserte Leistung
- Fehlerkorrekturen

### <a name="1656581"></a>1.6.5658.1

- Unterstützungsdatei für Azure Data Factory Auffangen
- Support erhalten Hierarchie in binären Kopie für Azure Data Factory
- Unterstützung Kopie Aktivität Idempotenz Azure Data Factory
- Fehlerkorrekturen

### <a name="1656401"></a>1.6.5640.1

- Unterstützt 3 Weitere Datenquellen für Azure Data Factory (ODBC, OData, bietet)
- Unterstützung für Azure Data Factory Anführungszeichen im CSV-parser
- Unterstützung der Komprimierung (BZip2)
- Fehlerkorrekturen

### <a name="1556121"></a>1.5.5612.1

- Unterstützung von fünf relationalen Datenbanken für Azure Data Factory (MySQL, PostgreSQL DB2 Teradata und Sybase)
- Unterstützung der Komprimierung (Gzip und Komprimierungsalgorithmen)
- Verbesserte Leistung
- Fehlerkorrekturen


### <a name="1455491"></a>1.4.5549.1

- Unterstützen Sie Oracle Data Source Azure Data Factory
- Verbesserte Leistung
- Fehlerkorrekturen

### <a name="1454921"></a>1.4.5492.1

- Einheitliche Binärdatei, die Microsoft Azure Data Factory und Office 365 Power BI Services unterstützt
- Verbessern Sie den Prozess Konfigurationsbenutzeroberfläche und Registrierung
- Azure Data Factory – Azure Ingress- und Egress-Unterstützung für SQL Server-Datenquelle

### <a name="1253031"></a>1.2.5303.1

-   Beheben Sie Timeout Problem Unterstützung mehr zeitaufwändig Quellcode-Datenkabel. 
    
### <a name="1155268"></a>1.1.5526.8

- Erfordert.NET Framework 4.5.1 während der Installation als Voraussetzung.

### <a name="1051442"></a>1.0.5144.2

- Keine Änderung, die Azure Data Factory Szenarien betreffen. 
