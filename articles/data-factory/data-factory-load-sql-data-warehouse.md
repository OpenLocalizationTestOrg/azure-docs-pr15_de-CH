<properties 
    pageTitle="Terabyte Daten in SQL Data Warehouse laden | Microsoft Azure" 
    description="Veranschaulicht, wie 1 TB Daten in Azure SQL Data Warehouse unter 15 Minuten mit Azure Daten geladen werden können" 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="jingwang"/>

# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-azure-data-factory-copy-wizard"></a>Laden Sie 1 TB in Azure SQL Data Warehouse unter 15 Minuten Azure Data Factory [Copy Wizard]
[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) ist eine cloudbasierte, Scale-Out Datenbank verarbeiten große Mengen an relationalen und nicht relationalen Daten.  Baut auf parallele (MPP) Verarbeitungsarchitektur ist SQL Data Warehouse für Enterprise Data Warehouse-Arbeitslasten optimiert.  Es bietet Cloud Elastizität Flexibilität Speicher und unabhängig zu berechnen.

Erste Schritte mit Azure SQL Data Warehouse ist nun einfacher als jemals **Azure Data Factory**.  Azure Data Factory ist eine vollständig verwaltete Cloud-basierte Integration Datendienst die darin ein SQL Data Warehouse mit Daten aus Ihrem vorhandenen System und sparen Sie wertvolle Zeit beim Auswerten SQL Data Warehouse und die Analytics Lösungen darüber verwendet werden kann.  Hier sind die wichtigsten Vorteile Laden von Daten in Azure SQL Data Warehouse Azure Data Factory verwenden:

- **Einfach einrichten**: 5 intuitiver Assistent kein Skripting erforderlich.
- **Umfangreiche Datenspeicher Support**: Unterstützung für vielfältige lokalen und cloudbasierten Datenspeicher.
- **Sichere und kompatible**: Daten über HTTPS oder ExpressRoute und globalen Dienst Präsenz Daten bleibt immer geografische Begrenzung
- **Unvergleichliche Leistung mit PolyBase** -Polybase verwenden wird am effizientesten Daten in Azure SQL Data Warehouse verschieben. Staging BLOB-Funktion können Sie hohe Last Geschwindigkeit aus allen Arten von Datenspeichern neben Azure BLOB-Speicher erreichen die Polybase standardmäßig unterstützt.

Dieser Artikel beschreibt wie Sie Assistenten zum Kopieren von Factory um 1 TB Daten aus Azure BLOB-Speicher unter 15 Minuten bei über 1,2 Gbit/s Durchsatz in Azure SQL Data Warehouse zu laden.

Dieser Artikel enthält eine schrittweise Anleitung zum Verschieben von Daten in Azure SQL Data Warehouse mithilfe des Assistenten zum Kopieren. 

> [AZURE.NOTE] [Verschieben von Daten zwischen Azure SQL Data Warehouse mithilfe von Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) finden Sie im Artikel Weitere Informationen zu Funktionen Data Factory Daten und Azure SQL Data Warehouse verschieben. 
> 
> Sie können Rohrleitungen Azure-Portal Visual Studio PowerShell erstellen usw.. Siehe [Tutorial: Kopieren von Daten von Azure Blob in Azure SQL-Datenbank](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) eine kurze exemplarische Vorgehensweise Schritt für die Verwendung der Kopieraktivität in Azure Data Factory.  

## <a name="prerequisites"></a>Erforderliche Komponenten
- Azure BLOB-Speicher: dieses Experiment verwendet Azure BLOB-Speicher (GRS) zum Speichern von TPC-H-Test-Dataset.  Wenn Sie nicht über ein Azure-Speicherkonto [ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account)erstellen verfügen.
- [TPC-H](http://www.tpc.org/tpch/) -Daten: wir TPC-H als Test-Dataset verwendet werden.  Dazu müssen mit `dbgen` TPC-H-Toolkits können Sie das Dataset generieren.  Sie können entweder Quellcode für `dbgen` [TPC](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) -Tools selbst kompilieren und die kompilierte Binärdatei von [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools)herunterladen.  Führen Sie dbgen.exe mit den folgenden Befehlen zu 1 TB Flatfile für `lineitem` Tabelle Verbreitung über 10 Dateien:
    - `Dbgen -s 1000 -S **1** -C 10 -T L -v`
    - `Dbgen -s 1000 -S **2** -C 10 -T L -v`
    - …
    - `Dbgen -s 1000 -S **10** -C 10 -T L -v` 

    Kopieren Sie jetzt die generierten Dateien in Azure Blob.  [Verschieben von Daten in und aus einem lokalen Dateisystem mit Azure Data Factory](data-factory-onprem-file-system-connector.md) für dazu mit ADF kopieren.   
- Azure SQL Datawarehouse: dieses Experiment lädt Daten in Azure SQL Data Warehouse mit 6.000 DWUs erstellt

    Informationen zum Erstellen einer SQL Data Warehouse-Datenbank finden Sie unter [Erstellen einer Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision/) .  Für die optimale Last Leistung in SQL Data Warehouse mit Polybase wählen wir die maximale Anzahl der Data Warehouse-Einheiten (DWUs) in der Leistung 6.000 DWUs ist zulässig.

    > [AZURE.NOTE] 
    > Beim Laden von Azure Blob ist die Leistung beim Laden von Daten direkt proportional zur Anzahl der DWUs im SQL Data Warehouse konfigurieren:
    > 
    > Laden von 1 TB in 1.000 DWU SQL Data Warehouse enthält 87min (~ 200MBps Durchsatz) 1 TB in 2.000 laden DWU SQL Data Warehouse enthält 46min (~ 380MBps Durchsatz) 1 TB in 6.000 laden DWU SQL Data Warehouse enthält 14min (~1.2GBps Durchsatz) 

    Zum Erstellen einer SQL Data Warehouse mit 6.000 DWUs den Schieberegler Leistung nach rechts:

    ![Performance-Schieberegler](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    Für eine vorhandene Datenbank, die nicht mit 6.000 DWUs konfiguriert ist, können Sie es skalieren, mit Azure-Portal.  Navigieren Sie zu der Datenbank in Azure-Portal und befindet sich eine Schaltfläche **Maßstab** in **den Übersichtsbereich in der folgenden Abbildung dargestellt** :

    ![Schaltfläche skalieren](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Verschieben Sie den Regler auf den Höchstwert der **Skalierung** klicken, um die folgenden Fenster, und klicken Sie auf **Speichern** .

    ![Dialogfeld Skalierung](media/data-factory-load-sql-data-warehouse/scale-dialog.png)
    
    Dieses Experiment lädt Daten in Azure SQL Data Warehouse mit `xlargerc` Klasse.

    Um beste Durchsatz zu erreichen, Kopie muss mit einer SQL Data Warehouse Benutzer `xlargerc` Klasse.  Erfahren Sie, wie durch folgende [Beispiel ein Benutzer Ressource ändern](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example).  

- Erstellen Sie Ziel Tabellenschema in Azure SQL Data Warehouse-Datenbank durch folgende DDL-Anweisung ausführen:

        CREATE TABLE [dbo].[lineitem]
        (
            [L_ORDERKEY] [bigint] NOT NULL,
            [L_PARTKEY] [bigint] NOT NULL,
            [L_SUPPKEY] [bigint] NOT NULL,
            [L_LINENUMBER] [int] NOT NULL,
            [L_QUANTITY] [decimal](15, 2) NULL,
            [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
            [L_DISCOUNT] [decimal](15, 2) NULL,
            [L_TAX] [decimal](15, 2) NULL,
            [L_RETURNFLAG] [char](1) NULL,
            [L_LINESTATUS] [char](1) NULL,
            [L_SHIPDATE] [date] NULL,
            [L_COMMITDATE] [date] NULL,
            [L_RECEIPTDATE] [date] NULL,
            [L_SHIPINSTRUCT] [char](25) NULL,
            [L_SHIPMODE] [char](10) NULL,
            [L_COMMENT] [varchar](44) NULL
        )
        WITH
        (
            DISTRIBUTION = ROUND_ROBIN,
            CLUSTERED COLUMNSTORE INDEX
        )

Die erforderlichen Schritte abgeschlossen sind wir jetzt mithilfe des Assistenten zum Kopieren kopieraktivität konfigurieren.

## <a name="launch-copy-wizard"></a>Starten des Assistenten zum Kopieren von

1.  Auf der [Azure-Portal](https://portal.azure.com)anmelden.
2.  Klicken Sie auf **+ neu** von der linken oberen Ecke und anschließend auf **Intelligenz + Analytik**, **Data Factory**. 
6. Im **neuen Data Factory** -Blade:
    1. Geben Sie den **Namen** **LoadIntoSQLDWDataFactory** .
        Der Name der Azure Data Factory muss eindeutig sein. Wenn Sie die Fehlermeldung: Ändern Sie den Namen der Data Factory (z. B. YournameLoadIntoSQLDWDataFactory) **Data Factory Name "LoadIntoSQLDWDataFactory" ist nicht verfügbar**, und versuchen Sie es erneut erstellen. Benennungskonventionen für Artefakte Data Factory finden Sie [Data Factory - Namenskonventionen](data-factory-naming-rules.md) .  
     
    2. Wählen Sie Ihre Azure- **Abonnement**.
    3. Ressourcengruppe führen Sie die folgenden Schritte aus: 
        1. Wählen Sie eine vorhandene Ressourcengruppe auswählen **vorhandenen verwenden** .
        2. Wählen Sie **neu erstellen** , geben Sie einen Namen für eine Ressourcengruppe.
    3. Wählen Sie einen **Speicherort** für die Daten-Factory.
    4. Kontrollkästchen Sie **Pin Dashboard** -am unteren Rand das Blade.  
    5. Klicken Sie auf **Erstellen**.
10. Nach Abschluss die Erstellung Siehe Blatt **Data Factory** , wie in der folgenden Abbildung dargestellt:

    ![Data Factory-Homepage](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
11. Klicken Sie auf der Seite Data Factory **Daten** **Kopieren-Assistenten**starten. 

    > [AZURE.NOTE] Wenn Sie sehen, dass der Webbrowser "Autorisieren..." fest, deaktivieren Sie **Drittanbieter-Cookies blockieren und Sitedaten** Einstellung deaktivieren bzw. (oder) behalten Sie aktiviert bei erstellen Sie eine Ausnahme für **login.microsoftonline.com** und versuchen Sie den Assistenten erneut starten.


## <a name="step-1-configure-data-loading-schedule"></a>Schritt 1: Konfigurieren von Zeitplan Laden von Daten
Der erste Schritt ist das Laden Zeitplan konfigurieren.  

Die Seite **Eigenschaften** :
1. Geben Sie **CopyFromBlobToAzureSqlDataWarehouse** als **Aufgabe ein**
2. Wählen Sie die Option **jetzt ausführen** .   
3. Klicken Sie auf **Weiter**.  

![Wizard - Seite kopieren](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>Schritt 2: Konfigurieren
In diesem Abschnitt werden die Schritte zum Konfigurieren der Quelle: Azure Blob mit 1 TB TPC-H-Zeile Artikel Dateien.

Wählen Sie **Azure BLOB-Speicher** die Daten speichern, und klicken Sie auf **Weiter**.

![Wizard - Seite Select Quelle kopieren](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

Geben Sie die Verbindungsinformationen für Azure BLOB-Speicher-Konto, und klicken Sie auf **Weiter**.

![Assistent zum Kopieren von-Datenquellen-Verbindungsinformationen](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

Wählen Sie den **Ordner** mit den TPC-H-Zeile, und klicken Sie auf **Weiter**.

![Assistent zum Kopieren von-Eingabeordner auswählen](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

Auf **Weiter**klicken, werden die formateinstellungen automatisch erkannt.  Stellen Sie sicher, dass diese Spaltentrennzeichen ' |'statt standardmäßige Komma",".  Klicken Sie auf **Weiter** , nachdem Sie die Daten in der Vorschau angezeigt haben.

![Wizard - formateinstellungen kopieren](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>Schritt 3: Ziel konfigurieren
Dieser Abschnitt veranschaulicht das Ziel konfigurieren: `lineitem` Tabelle in Azure SQL Data Warehouse-Datenbank.

Wählen Sie den Zielspeicher **Azure SQL Data Warehouse** und klicken Sie auf **Weiter**.

![Assistent zum Kopieren von-Datenspeicher auswählen](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

Geben Sie die Verbindungsinformationen für Azure SQL Data Warehouse.  Sicherzustellen, geben Sie den Benutzer, der Mitglied der Rolle `xlargerc` (siehe Abschnitt **erforderliche** Informationen), und klicken Sie auf **Weiter**. 

![Wizard - Verbindungsinformationen Ziel kopieren](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

Wählen Sie die Zieltabelle, und klicken Sie auf **Weiter**.

![Wizard - Mapping-Tabelle kopieren](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

Übernehmen Sie die Standardeinstellungen für die spaltenzuordnung, und klicken Sie auf **Weiter**.

![Wizard - Schema Zuordnungsseite kopieren](media/data-factory-load-sql-data-warehouse/schema-mapping.png)


## <a name="step-4-performance-settings"></a>Schritt 4: Leistung settings

**Zulassen Polybase** ist standardmäßig aktiviert.  Klicken Sie auf **Weiter**.

![Wizard - Schema Zuordnungsseite kopieren](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>Schritt 5: Bereitstellen Sie und überwachen Sie der Ergebnisse
Klicken Sie auf **Fertig stellen,** bereitstellen. 

![Wizard - Seite kopieren](media/data-factory-load-sql-data-warehouse/summary-page.png)

Nachdem die Bereitstellung abgeschlossen ist, klicken Sie auf `Click here to monitor copy pipeline` Kopie ausführen Fortschritt überwachen.

Wählen Sie in der **Aktivität** erstellte Kopie Pipeline.

![Wizard - Seite kopieren](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

Sie können die Kopie ausführen Details im **Fenster Explorer Aktivität** im rechten Fensterbereich angezeigt, einschließlich Datenvolumen Quelle lesen und Schreiben in Ziel, Dauer und der durchschnittliche Durchsatz für die Ausführung anzeigen.

Wie aus der folgenden Abbildung sehen können, hat 1 TB in SQL Data Warehouse von Azure BLOB-Speicher kopieren 14 Minuten 1,22 Gbit/s Durchsatz erzielen!

![Wizard - erfolgreich Dialogfeld Kopieren](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>Bewährte Methoden
Hier sind einige Tipps zur Ausführung von Azure SQL Data Warehouse-Datenbank:

- Verwenden Sie eine größere Ressourcenklasse in einem gruppierten INDEX COLUMNSTORE zu laden.
- Erwägen Sie für eine effizientere Joins hashverteilung durch eine Spalte anstelle round Robin-Verteilung.
- Beschleunigt laden sollten Sie in der Heap für vorübergehende Daten verwenden.
- Erstellen Sie Statistiken, nachdem Sie Azure SQL Data Warehouse geladen haben.

Einzelheiten finden Sie unter [Best Practices für Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) . 

## <a name="next-steps"></a>Nächste Schritte
- [Assistent zum Kopieren von Factory](data-factory-copy-wizard.md) - dieser Artikel enthält Details über den Assistenten zum Kopieren. 
- [Kopieraktivität Performance und tuning Guide](data-factory-copy-activity-performance.md) - enthält die Referenz Leistung Maße und tuning Guide.

