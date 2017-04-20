<properties
    pageTitle="OLTP Speicher verbessert SQL Txn Perf | Microsoft Azure"
    description="Übernehmen im Arbeitsspeicher OLTP transaktionale Leistung in einer vorhandenen SQL-Datenbank."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor="MightyPen"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="use-in-memory-oltp-preview-to-improve-your-application-performance-in-sql-database"></a>Verwendung im Arbeitsspeicher OLTP (Vorschau) Ihre Anwendung Leistung in SQL-Datenbank

[OLTP Speicher](sql-database-in-memory.md) kann zur Verbesserung der Leistung der OLTP-Workload [Premium](sql-database-service-tiers.md) Azure SQL-Datenbanken ohne Erhöhung der Leistung verwendet werden.

Gehen Sie in der vorhandenen Datenbank im Arbeitsspeicher OLTP erlassen.

## <a name="step-1-ensure-your-premium-database-supports-in-memory-oltp"></a>Schritt 1: Sicherstellen Sie, dass Ihre Datenbank Premium OLTP Speicher unterstützt

Premium-Datenbanken erstellt wurden, im November 2015 oder höher unterstützen In-Memory-Funktion. Sie können prüfen, ob die Premium-Datenbank In-Memory-Funktion unterstützt die folgende Transact-SQL-Anweisung ausführen. Im Arbeitsspeicher wird unterstützt, ist das zurückgegebene Ergebnis 1 (nicht 0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* steht für *Extreme verarbeiten*

Wenn die vorhandene Datenbank in eine neue Datenbank V12 Premium verschoben werden muss, können Sie die folgenden Verfahren zum Exportieren und importieren die Daten.

#### <a name="export-steps"></a>Exportschritte

Exportieren Sie die Produktionsdatenbank entweder in ein Bacpac:

- [Export](sql-database-export.md) Funktion im [Portal](https://portal.azure.com/).

- **Export Datenebene** Anwendungsfunktionen in ein [Aktuelles SSMS.exe](http://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio).
 1. Erweitern Sie im **Objekt-Explorer**den Knoten **Datenbanken** .
 2. Klicken Sie auf den Datenbankknoten.
 3. Klicken Sie auf **Vorgänge** > **Datenebene Anwendung exportieren**.
 4. Betreiben Sie das Assistentenfenster angezeigt.


#### <a name="import-steps"></a>Importschritte

Importieren Sie die Bacpac in eine neue Premium-Datenbank.

1. In Azure- [portal](https://portal.azure.com/)
 - Navigieren Sie zu dem Server.
 - Aktivieren Sie die Option [Datenbank importieren](sql-database-import.md) .
 - Wählen Sie eine Prämie Tarif.

2. Verwenden Sie SSMS die Bacpac importieren:
 - Klicken Sie im **Objekt-Explorer**den Knoten **Datenbanken** .
 - Klicken Sie auf **Datenebene Anwendung importieren**.
 - Betreiben Sie das Assistentenfenster angezeigt.


## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Schritt 2: Identifizieren Sie Objekte im Speicher OLTP migrieren

SSMS enthält eine **Transaktion Performance Analysis** Übersichtsbericht, die für eine Datenbank mit einer aktiven Arbeitslast ausgeführt. Im Bericht werden Tabellen und gespeicherten Prozeduren, die Kandidaten für eine Migration zu OLTP Speicher.

In SSMS zum Generieren des Berichts:
- Klicken Sie im **Objekt-Explorer**den Datenbankknoten.
- Klicken Sie auf **Berichte** > **Berichte** > **Performance Analysis Übersicht**.

Weitere Informationen finden Sie unter [Determining bei einer Tabelle oder gespeicherten Prozedur sollte portiert werden, OLTP Speicher](http://msdn.microsoft.com/library/dn205133.aspx).


## <a name="step-3-create-a-comparable-test-database"></a>Schritt 3: Erstellen einer vergleichbaren Testdatenbank

Angenommen Sie, der Bericht gibt an, dass die Datenbank eine Tabelle enthält, die aus einer speicheroptimierten Tabelle konvertiert. Wir empfehlen, zunächst testen, um die Angabe von Tests bestätigen.

Sie benötigen eine Testkopie der Produktionsdatenbank. Die Datenbank muss sich auf derselben Ebene Dienstebene als Produktionsdatenbank.

Zur Erleichterung der Tests optimieren Sie Ihrer Testdatenbank wie folgt:

1. Verbinden Sie mit der Datenbank mit SSMS.

2. Um zu vermeiden, brauchen die Option mit (SNAPSHOT) Abfragen, richten Sie die wie die folgende T-SQL-Anweisung:
```
ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
```


## <a name="step-4-migrate-tables"></a>Schritt 4: Tabellen migrieren

Sie erstellen und füllen eine Speicheroptimierte Kopie der Tabelle, die Sie testen möchten. Sie können entweder:

- Praktische Speicher Optimierung der in SSMS.
- Manuelle T-SQL.


#### <a name="memory-optimization-wizard-in-ssms"></a>Speicher-Optimierung-Assistent in SSMS

Verwenden Sie diese Option für die migration

1. Verbinden Sie mit der Testdatenbank mit SSMS.

2. Im **Objekt-Explorer**mit der rechten Maustaste auf die Tabelle und dann auf **Speicher Optimierung Berater**.
 - **Tabelle Speicher Optimizer Advisor** -Assistent wird angezeigt.

3. Klicken Sie im Assistenten auf **Migration Validierung** (oder **Weiter** ), um festzustellen, ob die Tabelle nicht unterstützte Features enthält, die im Speicher optimiert Tabellen nicht unterstützt. Weitere Informationen finden Sie unter:
 - Der *Speicher Optimierung Prüfliste* im [Speicher Optimierung Berater](http://msdn.microsoft.com/library/dn284308.aspx).
 - [Transact-SQL-Konstrukten von OLTP Speicher nicht unterstützt](http://msdn.microsoft.com/library/dn246937.aspx).
 - [OLTP Speicher migrieren](http://msdn.microsoft.com/library/dn247639.aspx).

4. Wenn die Tabelle keine nicht unterstützten Features aufweist, kann Berater das aktuelle Schema und die Datenmigration für Sie durchführen.


#### <a name="manual-t-sql"></a>Manuelle T-SQL

Verwenden Sie diese Option für die migration

1. Verbinden Sie mit der Testdatenbank mit SSMS (oder einem ähnlichen Dienstprogramm).

2. Das vollständige T-SQL-Skript für die Tabelle und ihre Indizes zu erhalten.
 - Klicken Sie in SSMS auf die Knoten.
 - Klicken Sie auf **Skript für Tabelle als** > **erstellen, um** > **Neues Abfragefenster**.

3. Fügen Sie im Skriptfenster mit (MEMORY_OPTIMIZED = ON) der CREATE TABLE-Anweisung.

4. Wenn ein GRUPPIERTER Index ist NONCLUSTERED ändern.

5. Benennen Sie die vorhandene Tabelle mit SP_RENAME.

6. Erstellen Sie die neue Speicher optimiert Kopie der Tabelle mit der bearbeiteten CREATE TABLE-Skript

7. Die Daten der Tabelle Speicher optimiert mit INSERT... WÄHLEN SIE * IN:
    
```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>Schritt 5 (optional): Migrieren von gespeicherten Prozeduren

In-Memory-Feature können auch eine gespeicherte Prozedur zur Verbesserung der Leistung.


### <a name="considerations-with-natively-compiled-stored-procedures"></a>Bei nativ kompilierte gespeicherte Prozeduren

Nativ kompilierte gespeicherte Prozedur muss in der T-SQL WITH-Klausel die folgenden Optionen:

- NATIVE_COMPILATION

- SCHEMABINDING: Bedeutung Tabellen haben die gespeicherte Prozedur die Spaltendefinitionen geändert, die die gespeicherte Prozedur auswirkt, wenn die gespeicherte Prozedur löschen.


Ein systemeigenen Modul muss einen großen [ATOMAREN Blöcke](http://msdn.microsoft.com/library/dn452281.aspx) für Verwaltung verwenden. Es gibt keine Rolle für eine explizite BEGIN TRANSACTION oder ROLLBACK der Transaktion. Erkennt Code eine Verletzung einer Geschäftsregel, können atomaren Block mit einer [THROW](http://msdn.microsoft.com/library/ee677615.aspx) -Anweisung beendet werden.


### <a name="typical-create-procedure-for-natively-compiled"></a>Normalerweise erstellen Sie Verfahren für systemeigen kompiliert

T-SQL direkt kompilierte gespeicherte Prozedur erstellen entspricht normalerweise folgende Vorlage:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

- SNAPSHOT ist für die TRANSACTION_ISOLATION_LEVEL der häufigste Wert für systemeigene kompilierten gespeicherten Prozedur. Eine Teilmenge der anderen Werte werden jedoch ebenfalls unterstützt:
 - WIEDERHOLBARE LESEVORGÄNGE
 - SERIALISIERBAR


- Der Sprachwert muss in der sys.languages vorhanden sein.


### <a name="how-to-migrate-a-stored-procedure"></a>Migrieren eine gespeicherte Prozedur

Die Migrationsschritte sind:


1. Das Skript erstellen interpretierten regulären gespeicherten Prozedur zu erhalten.

2. Schreiben Sie Header entsprechend die vorherige Vorlage.

3. Prüfen Sie, ob die gespeicherte Prozedur T-SQL-Code Features verwendet, die für systemeigene kompilierte gespeicherte Prozeduren nicht unterstützt. Implementieren Sie ggf. Workarounds.
 - Einzelheiten finden Sie unter [Migrationsprobleme für gespeicherte Prozeduren systemeigen kompiliert](http://msdn.microsoft.com/library/dn296678.aspx).

4. Umbenennen der alten Prozedur SP_RENAME. Oder einfach löschen.

5. Das bearbeitete erstellen Verfahren T-SQL-Skript ausführen.


## <a name="step-6-run-your-workload-in-test"></a>Schritt 6: Workload Test ausführen

Führen Sie eine Arbeitslast in Ihrer Testdatenbank Arbeitslast ähnelt, die in der Datenbank ausgeführt wird. Dadurch sollte den Leistungsgewinn erreicht durch die Verwendung der Funktion im Arbeitsspeicher für Tabellen und gespeicherten Prozeduren angezeigt.

Wichtige Attribute der Arbeitslast sind:

- Anzahl der aktiven Verbindungen.

- Lese-/Schreibverhältnis.


Anpassen und die Arbeitslast Test ausführen, sollten die praktische ostress.exe Werkzeug [hier](sql-database-in-memory.md)dargestellt.


Netzwerklatenz zu minimieren, führen Sie den Test in der gleichen geografischen Region Azure, wo die Datenbank vorhanden ist.


## <a name="step-7-post-implementation-monitoring"></a>Schritt 7: Überwachung nach der Implementierung

Betrachten Sie leistungsbezogene Ihrer Implementierungen im Speicher in der Produktion überwachen:

- [Monitor In - Speicher](sql-database-in-memory-oltp-monitoring.md).

- [Überwachung über dynamische Verwaltungsansichten Azure SQL-Datenbank](sql-database-monitoring-with-dmvs.md)


## <a name="related-links"></a>Verwandte links

- [Speicher OLTP (In-Memory-Optimierung)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Einführung in nativ kompilierte gespeicherte Prozeduren](http://msdn.microsoft.com/library/dn133184.aspx)

- [Speicher-Optimierung Berater](http://msdn.microsoft.com/library/dn284308.aspx)

