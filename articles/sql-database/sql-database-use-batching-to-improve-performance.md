 <properties
    pageTitle="Gewusst wie: Batchverarbeitung Azure SQL-Datenbank Anwendung Leistung verwenden"
    description="Das Thema belegt, Batchverarbeitung Datenbankoperationen erheblich Imroves Geschwindigkeit und Erweiterbarkeit Azure SQL-Datenbank Anwendung. Obwohl diese Batchverarbeitung Verfahren für jede SQL Server-Datenbank arbeiten, liegt der Schwerpunkt des Artikels Azure."
    services="sql-database"
    documentationCenter="na"
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="07/12/2016"
    ms.author="annemill" />

# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>Wie Batchverarbeitung SQL Datenbank Anwendung Leistung verwenden

Batchverarbeitung in Azure SQL-Datenbank erheblich verbessert die Leistung und Skalierung Ihrer Programme. Um die Vorteile umfasst der erste Teil dieses Artikels Beispieltestergebnisse, die sequenzielle und gespeicherten Anfragen mit einer SQL-Datenbank zu vergleichen. Die restlichen Artikel werden Techniken, Szenarien und Hinweise zu helfen, erfolgreich in Azure Applications Batchverarbeitung verwenden

**Autoren**: Jason Roth, Silvano Coriani, Trent Swanson (Inc Skalenendwertes 180)

**Bearbeiter**: Conor Cunningham, Michael Thomassy

## <a name="why-is-batching-important-for-sql-database"></a>Warum wichtig für SQL-Datenbank Batchverarbeitung?
Batchverarbeitung Aufrufe remote Service ist eine bekannte Strategie zur Erhöhung der Leistung und Erweiterbarkeit. Es werden Kosten zu Wechselwirkungen mit einen Remotedienst Netzwerk übertragen, Serialisierung und Deserialisierung behoben. Viele separate Transaktionen in einem einzigen Batch Verpackung minimiert diese.

In diesem Artikel möchten verschiedene SQL-Datenbank Batchverarbeitung Strategien und Szenarien zu untersuchen. Obwohl diese Strategien auch für lokale Anwendung wichtig, die SQL Server verwenden sind, gibt Gründe für die Verwendung der SQL-Datenbank Batchverarbeitung Hervorhebung:

- Ist möglicherweise größer Netzwerklatenz Zugriff auf SQL-Datenbank, insbesondere Zugriff auf SQL-Datenbank außerhalb der gleichen Microsoft Azure Rechenzentren.
- Mandantenfähigen Merkmale der SQL-Datenbank bedeutet, dass die Effizienz von Data Access Layer korreliert insgesamt Skalierbarkeit der Datenbank. SQL-Datenbank müssen verhindern, dass jeder einzelne Mieter/Benutzer beanspruchen Ressourcen gegenüber anderen Mandanten. Reaktion auf die Verwendung von vordefinierten Kontingente SQL Datenbank Durchsatz verringern oder Drosselung Ausnahmen reagieren. Effizienz wie Batchverarbeitung können weitere SQL Datenbank vor dieser Grenzen arbeiten. 
- Batchverarbeitung ist auch für Architekturen, die Datenbanken (Sharding) verwenden. Die Effizienz Ihrer Interaktion mit jeder Datenbank bleibt ein Schlüsselfaktor in der gesamten skalierbar. 

Einer der Vorteile der Verwendung von SQL-Datenbank ist, müssen Sie die Server verwalten, die die Datenbank hostet. Diese verwalteten Infrastruktur bedeutet jedoch auch anders überlegen Datenbank Optimierungen haben. Sie können nicht mehr zur Verbesserung der Datenbankinfrastruktur Hardware- oder suchen. Microsoft Azure steuert die Umgebung. Die Sie steuern können, ist wie die Anwendung mit SQL Datenbank interagiert. Batchverarbeitung gehört diese Optimierungen. 

Der erste Teil des Papiers untersucht verschiedene Batchverarbeitung Techniken für .NET Applications, die SQL-Datenbank verwenden. Die letzten beiden Abschnitten Batchverarbeitung Richtlinien und Szenarios.

## <a name="batching-strategies"></a>Batchverarbeitung Strategien

### <a name="note-about-timing-results-in-this-topic"></a>Hinweis zum Timing führt zu diesem Thema
>[AZURE.NOTE] Ergebnisse sind keine Benchmarks aber **relative Leistung**anzeigen sollen. Anzeigedauer basieren auf durchschnittlich 10 Testläufe. Fügt in eine leere Tabelle sind. Diese Tests wurden gemessenen Pre-V12 und sie entsprechen nicht unbedingt auf den Durchsatz in einer V12 mit neuen [Dienstebenen](sql-database-service-tiers.md)auftreten können. Der relative Vorteil der Batchverarbeitung Technik sollte ähnlich sein.

### <a name="transactions"></a>Transaktionen
Merkwürdig ist eine Überprüfung der Batchverarbeitung von Transaktionen Diskussion beginnen. Aber die Verwendung von clientseitigen Transaktionen serverseitige Batchverarbeitung Effekte sehr, die Leistung verbessert. Und Transaktionen können mit nur wenigen Zeilen Code hinzugefügt werden, damit sie schnell sequenzielle Prozesse zu bieten.

Folgenden C# Code, der eine Sequenz von Insert enthält und Aktualisierungsvorgänge für eine einfache Tabelle.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

Der folgende Code ADO.NET ausgeführt sequenziell.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
    
        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

Die beste Möglichkeit, diesen Code optimieren werden einige clientseitige Batchverarbeitung dieser Aufrufe implementieren. Aber es gibt eine einfache Möglichkeit, die Leistung dieses Codes erhöhen, indem es einfach die Reihenfolge der Aufrufe in einer Transaktion. Hier ist der gleiche Code eine Transaktion verwendet.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();
    
        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }
    
        transaction.Commit();
    }

In beiden Beispielen wird tatsächlich Transaktionen verwendet. Im ersten Beispiel wird jedem Aufruf eine implizite Transaktion. Im zweiten Beispiel umschließt eine explizite Transaktion alle Aufrufe. Pro der Dokumentation für die- [Write-ahead Transaktionsprotokoll](https://msdn.microsoft.com/library/ms186259.aspx)werden Protokolleinträge auf den Datenträger geleert, wenn Commit für die Transaktion ausgeführt. So kann mit mehr Aufrufe in einer Transaktion der Schreibvorgang im Transaktionsprotokoll verzögert wird, bis die Transaktion. Aktivieren Sie Batchverarbeitung für Schreibvorgänge auf der Server-Transaktionsprotokoll.

Die folgende Tabelle zeigt einige Ad-hoc-Testergebnisse. Die Tests dieselbe sequenzielle fügt mit und ohne Transaktionen. Mehr Perspektive leitete der erste Satz von Tests Remote von einem Laptop aus Datenbank im Microsoft Azure. Der zweite Satz von Tests ausgeführt Cloud-Dienst und Datenbank, beide im gleichen Microsoft Azure-Rechenzentrum (US West) befand. Die folgende Tabelle zeigt die Dauer in Millisekunden sequenzielle fügt mit und ohne Transaktionen.

**Lokal in Azure**:

| Vorgänge | Keine Transaktion (ms) | Transaktion (ms) |
|---|---|---|
| 1 | 130 | 402 |
| 10 | 1208 | 1226 |
| 100 | 12662 | 10395 |
| 1000 | 128852 | 102917 |


**Azure Azure (demselben Datencenter)**:

| Vorgänge | Keine Transaktion (ms) | Transaktion (ms) |
|---|---|---|
| 1 | 21 | 26 |
| 10 | 220 | 56 |
| 100 | 2145 | 341 |
| 1000 | 21479 | 2756 |

>[AZURE.NOTE] Ergebnisse sind keine Benchmarks. Siehe [Hinweis Timing Ergebnisse in diesem Thema](#note-about-timing-results-in-this-topic).

Basierend auf die vorherigen Testergebnisse verringert umschließen einer einzelnen Operation in einer Transaktion tatsächlich Leistung. Aber erhöht die Anzahl der Vorgänge in einer einzigen Transaktion die Leistungssteigerung wird mehr markiert. Der Leistungsunterschied ist auch deutlicher, treten alle Operationen im Microsoft Azure-Rechenzentrum. Die Latenzzeit von außerhalb des Datencenters Microsoft Azure SQL-Datenbank mit überschattet Leistungsgewinn von Transaktionen.

Obwohl die Verwendung von Transaktionen zur Verbesserung der Leistung, weiterhin zu [best Practices für Transaktionen und Verbindungen](https://msdn.microsoft.com/library/ms187484.aspx). Halten Sie die Transaktion so kurz wie möglich, und schließen Sie die Verbindung nach Abschluss die Arbeit. Die mit der Anweisung im vorherigen Beispiel wird sichergestellt, dass die Verbindung nach Abschluss der nachfolgenden Codeblock.

Im vorherige Beispiel veranschaulicht das Hinzufügen einer lokalen Transaktions mit zwei Zeilen Code ADO.NET. Transaktionen bieten eine schnelle Möglichkeit, die Leistung des Codes zu verbessern, die sequenzielle INSERT-, Update- und delete-Operationen macht. Jedoch für höchste Leistung überlegen Sie, ob des Codes weiter nutzen clientseitige Batchverarbeitung wie Tabellenwertparametern.

Weitere Informationen über Transaktionen in ADO.NET finden Sie unter [Lokale Transaktionen in ADO.NET](https://msdn.microsoft.com/library/vstudio/2k2hy99x.aspx).

### <a name="table-valued-parameters"></a>Tabellenwertparameter
Tabellenwertparameter unterstützen benutzerdefinierte Tabelle als Parameter in Transact-SQL-Anweisung, gespeicherte Prozeduren und Funktionen. Diese clientseitige Batchverarbeitung Verfahren ermöglicht Ihnen, mehrere Zeilen innerhalb der Tabellenwertparameter. Zum Verwenden von Tabellenwertparametern zunächst einen Tabellentyp. Transact-SQL-Anweisung erstellt eine Tabelle namens **MyTableType**.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );
 

In Code erstellen Sie ein **DataTable** mit genau demselben Namen und Typen der Tabellentyp. Übergeben dieser **DataTable** Parameter in einer Abfrage oder gespeicherten Prozeduraufruf. Das folgende Beispiel veranschaulicht diese Technik:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }
    
        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);
                    
        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });
    
        cmd.ExecuteNonQuery();
    }

Im vorherigen Beispiel fügt das **SqlCommand** -Objekt Zeilen aus einem Tabellenwertparameter **@TestTvp**. Dieser Parameter wird das zuvor erstellte **DataTable** -Objekt mit der **SqlCommand.Parameters.Add** -Methode zugewiesen. Fügt in einem Aufruf wesentlich Batchverarbeitung steigt sequenzielle fügt die Leistung.

Textbasierte Befehl anstelle einer gespeicherten Prozedur, zum Beispiel weiter zu verbessern. Der folgende Transact-SQL-Befehl erstellt eine gespeicherte Prozedur, die die **SimpleTestTableType** Tabellenwertparameter akzeptiert.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Ändern Sie im vorherigen Codebeispiel die folgende Deklaration der **SqlCommand** .

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

In den meisten Fällen haben Tabellenwertparameter gleichwertiger oder besserer Performance als andere Batchverarbeitung. Tabellenwertparameter sind häufig vorzuziehen, sind flexibler als andere Optionen. Andere Techniken wie SQL Massenkopieren ermöglichen z. B. nur das Einfügen neuer Zeilen. Aber mit Tabellenrückgabe Parametern können Sie Logik in der gespeicherten Prozedur um zu bestimmen, welche Zeilen Updates und fügt die. Art der kann auch eine Spalte "Betrieb" enthält, die angibt, ob die angegebene Zeile eingefügt, aktualisiert oder gelöscht werden soll, geändert werden.

Die folgende Tabelle zeigt Ad-hoc-Testergebnisse für die Verwendung von Tabellenwertparametern in Millisekunden.

| Vorgänge | Lokal in Azure (ms)  | Azure demselben Datencenter (ms) |
|---|---|---|
| 1 | 124 | 32 |
| 10 | 131 | 25 |
| 100 | 338 | 51 |
| 1000 | 2615 | 382 |
| 10000 | 23830 | 3586 |

>[AZURE.NOTE] Ergebnisse sind keine Benchmarks. Siehe [Hinweis Timing Ergebnisse in diesem Thema](#note-about-timing-results-in-this-topic).

Der Leistungsgewinn in Batchverarbeitung ist offensichtlich. In der vorherigen sequenziellen Test hat 1000 Vorgänge 129 Sekunden außerhalb des Datencenters und 21 Sekunden im Rechenzentrum. Aber mit Tabellenwertparameter, 1000 Vorgänge nur 2,6 Sekunden außerhalb des Datencenters und 0,4 Sekunden im Rechenzentrum.

Weitere Informationen zu Tabellenwertparameter finden Sie unter [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>SQL-Massenkopieren
Massenkopieren SQL ist, große Datenmengen in eine Datenbank einfügen. **SqlBulkCopy** -Klasse können .NET Applications Bulk Insert-Operationen. **SqlBulkCopy** ähnelt in Funktion des Befehlszeilenprogramms **Bcp.exe**oder die Transact-SQL-Anweisung **BULK INSERT**. Im folgenden Codebeispiel wird veranschaulicht, wie Massenkopieren Zeilen in der Quelle **DataTable**Tabelle in die Zieltabelle SQL Server MyTable.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

Es gibt Fälle, Massenkopieren über Tabellenwertparameter bevorzugt. Siehe Vergleich Tabellenwertparameter und BULK INSERT-Vorgängen im Thema [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).

Testergebnisse von Ad-hoc-zeigen die Leistung der Batchverarbeitung mit **SqlBulkCopy** in Millisekunden.

| Vorgänge | Lokal in Azure (ms)  | Azure demselben Datencenter (ms) |
|---|---|---|
| 1 | 433 | 57 |
| 10 | 441 | 32 |
| 100 | 636 | 53 |
| 1000 | 2535 | 341 |
| 10000 | 21605 | 2737 |

>[AZURE.NOTE] Ergebnisse sind keine Benchmarks. Siehe [Hinweis Timing Ergebnisse in diesem Thema](#note-about-timing-results-in-this-topic).

In kleinere Stapel hat die Verwendung Tabellenwertparameter **SqlBulkCopy** -Klasse. **SqlBulkCopy** , 12-31 % schneller als Tabellenwertparameter für Tests von 1.000 bis 10.000 Zeilen ausgeführt. Tabellenwertparameter ähnelt **SqlBulkCopy** eine gute Option für gespeicherten einfügen Vergleich zu Prozesse nicht zusammengefasst.

Weitere Informationen zu ADO.NET Massenkopiervorgang finden Sie [Massenkopiervorgänge in SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Mehrzeilen-parametrisierten legen-Anweisung
Eine Alternative für kleine Batches ist eine große parametrisierte INSERT-Anweisung, die mehrere Zeilen eingefügt. Im folgenden Codebeispiel wird dieses Verfahren veranschaulicht.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";
    
        SqlCommand cmd = new SqlCommand(insertCommand, connection);
    
        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }
    
        cmd.ExecuteNonQuery();
    }
 

In diesem Beispiel soll das grundlegende Konzept anzeigen. Die erforderlichen Elemente gleichzeitig die Abfragezeichenfolge und die Befehlsparameter erstellen würde ein realistischeres Szenario durchlaufen. Sie sind auf insgesamt 2100 Abfrageparameter, damit dies die Gesamtzahl der Zeilen beschränkt, die auf diese Weise verarbeitet werden können.

Testergebnisse von Ad-hoc-zeigen die Leistung dieses Insert-Anweisung in Millisekunden.

| Vorgänge | Tabellenwertparameter (ms) | Einfügen einer Anweisung (ms) |
|---|---|---|
| 1 | 32 | 20 |
| 10 | 30 | 25 |
| 100 | 33 | 51 |

>[AZURE.NOTE] Ergebnisse sind keine Benchmarks. Siehe [Hinweis Timing Ergebnisse in diesem Thema](#note-about-timing-results-in-this-topic).

Diese Vorgehensweise kann für Stapel etwas schneller sein, die weniger als 100 Zeilen. Auch die Verbesserung klein ist, ist diese Technik, die in dem jeweiligen Anwendungsszenario funktionieren.

### <a name="dataadapter"></a>DataAdapter
Die **DataAdapter** -Klasse können Sie ein **DataSet** -Objekt ändern und anschließend die Änderungen einfügen, aktualisieren und löschen. Bei Verwendung von **DataAdapter** auf diese Weise ist es wichtig zu beachten, dass separate für jede eindeutige Operation aufgerufen werden. Zum Verbessern der Leistung mithilfe der **UpdateBatchSize** -Eigenschaft auf die Anzahl der Vorgänge, die zu einem Zeitpunkt zusammengefasst werden sollen. Weitere Informationen finden Sie unter [Batch Vorgänge verwenden DataAdapters durchführen](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Entity framework
Entity Framework unterstützt derzeit Batchverarbeitung nicht. Andere Entwickler in der Gemeinschaft haben versucht, Workarounds wie überschreiben die **SaveChanges** -Methode veranschaulicht. Aber die normalerweise komplexe und benutzerdefinierte Anwendung und Datenmodell. Entity Framework Codeplex-Projekt hat derzeit eine Diskussionsseite auf diese Anforderung Feature. Diese Diskussion finden Sie [Besprechungsnotizen Design – 2. August 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML
Der Vollständigkeit halber Erachtens XML Batchverarbeitung Strategie sprechen wichtig. Die Verwendung von XML hat jedoch keine Vorteile gegenüber anderen Methoden und einige Nachteile. Die Vorgehensweise ähnelt Tabellenwertparameter jedoch eine XML-Datei oder eine Zeichenfolge wird an eine gespeicherte Prozedur anstelle einer benutzerdefinierten Tabelle übergeben. Die gespeicherte Prozedur analysiert die Befehle in der gespeicherten Prozedur.

Es gibt einige Nachteile dieses Ansatzes:

- Arbeiten mit XML kann mühsam und fehleranfällig.
- Analysieren von XML in der Datenbank kann CPU-intensiv.
- In den meisten Fällen ist diese Methode Tabellenwertparameter langsamer.

Aus diesen Gründen ist die Verwendung von XML für Batchabfragen nicht empfohlen.

## <a name="batching-considerations"></a>Batchverarbeitung Aspekte
Die folgenden Abschnitte enthalten weitere Informationen zur Verwendung der Batchverarbeitung in SQL-Datenbanken.

### <a name="tradeoffs"></a>Nachteile
Je nach Ihrer Architektur betreffen Batchverarbeitung einen Kompromiss zwischen Leistung und Stabilität. Angenommen Sie, das Szenario, unerwartet Ihre Rolle ausfällt. Wenn Sie eine Zeile mit Daten verlieren, wirkt die Auswirkung der Verlust einer großen Batch nicht übermittelten Zeilen kleiner. Ein Risiko wird Zeilen Puffern, bevor sie an die Datenbank in einem angegebenen Zeitfenster.

Aufgrund dieser Kompromiss Bewerten der Art der Vorgänge Sie Stapel. Aggressiver Batch (größere Chargen und mehr Zeitfenster) mit weniger kritische Daten.

### <a name="batch-size"></a>Batchgröße
In unseren Tests war in der Regel kein Vorteil großen Batches in kleinere Einheiten aufteilen. Tatsächlich führte dieses Unterfeld oft langsamer als Vorlage einer großen Partie. Betrachten Sie beispielsweise ein Szenario, in dem Sie 1000 Zeilen einfügen möchten. Die folgende Tabelle zeigt, wie lange es dauert, Tabellenwertparametern in kleinere Batches unterteilt 1000 Zeilen einzufügen.

| Batchgröße | Iterationen | Tabellenwertparameter (ms) |
| -------- | --- | --- |
| 1000 | 1 | 347 |
| 500 | 2 | 355 |
| 100 | 10 | 465 |
| 50 | 20 | 630 |

>[AZURE.NOTE] Ergebnisse sind keine Benchmarks. Siehe [Hinweis Timing Ergebnisse in diesem Thema](#note-about-timing-results-in-this-topic).

Sie können sehen, dass die optimale Leistung für 1000 Zeilen auf einmal einreichen. In anderen Tests (hier nicht gezeigt) war eine leichte Leistungssteigerung in zwei Batches 5000 10000 Zeile Batch unterbrochen. Aber das Schema für diese Tests ist relativ einfach, damit Sie Tests durchführen sollten, auf Ihre Daten und Batchgrößen Ergebnisse überprüfen.

In Erwägung ziehen ist, wird der gesamte Stapel groß, SQL-Datenbank möglicherweise drosseln Batch übernehmen wollen. Die besten Ergebnisse testen Sie um festzustellen, ob eine optimale Batchgröße Szenario. Gemacht die Batchgröße Laufzeit ermöglichen schnelle Anpassung basierend auf Leistung oder Fehler.

Saldo schließlich die Größe des Stapels mit Risiken Batchverarbeitung. Sollten Sie vorübergehende Fehler oder Rolle ausfällt, die folgen den Vorgang wiederholen oder Datenverlust im Stapel.

### <a name="parallel-processing"></a>Parallele Verarbeitung
Wenn Ansatz der verringert die Batchgröße aber verwendet mehrere Threads zum Ausführen der Arbeit? Unsere Tests zufolge wiederum mehrere kleinere multithreaded Blätter in der Regel schlimmer größere Partie durchgeführt. Der folgende Test versucht, in ein oder mehrere parallele Stapel 1000 Zeilen einfügen. Dieser Test zeigt, wie gleichzeitige Batches tatsächlich Leistung verringert.

| Batchgröße [Iterationen] | Zwei Threads (ms) | Vier Threads (ms) | Sechs Threads (ms) |
| -------- | --- | --- | --- |
| 1000 [1] | 277 | 315 | 266 |
| 500 [2] | 548 | 278 | 256 |
| 250 [4] | 405 | 329 | 265 |
| 100 [10] | 488 | 439 | 391 |

>[AZURE.NOTE] Ergebnisse sind keine Benchmarks. Siehe [Hinweis Timing Ergebnisse in diesem Thema](#note-about-timing-results-in-this-topic).

Es gibt mehrere mögliche Gründe für die Leistungseinbußen durch Parallelität:

- Es gibt mehrere gleichzeitige Netzwerkaufrufe statt einer.
- Mehrere Vorgänge für eine einzelne Tabelle führt zu Konflikten und blockieren.
- Gemeinkosten zugeordnet sind multithreading.
- Die Ausgaben für mehrere Verbindungen öffnen überwiegen die Vorteile der parallelen Verarbeitung.

Wenn Sie andere Tabellen oder Datenbanken abzielen, kann mit dieser Strategie erhalten Leistung finden Sie unter. Datenbanksharding oder Verbände wäre ein Szenario für diesen Ansatz. Sharding verwendet Datenbanken und andere Daten für die einzelnen Routen. Wenn jedes kleiner Batches zu einer anderen Datenbank, kann dann parallel Vorgänge effizienter sein. Der Leistungsgewinn ist jedoch nicht signifikant genug, als Grundlage für eine Entscheidung datenbanksharding in der Projektmappe verwendet.

In einigen Entwürfen führt parallele Ausführung von kleineren Chargen zu höherer Durchsatz von Anfragen in einem System unter Last. In diesem Fall, obwohl es schneller zu einer Partie größer ist, kann mehrere Batches parallel verarbeiten effizienter sein.

Verwenden Sie parallele Ausführung, sollten Sie die maximale Anzahl von Arbeitsthreads steuern. Weniger möglicherweise weniger Konflikte und eine kürzere Ausführungszeit. Berücksichtigen Sie die zusätzliche Last in der Zieldatenbank Verbindungen und Transaktionen dadurch.

### <a name="related-performance-factors"></a>Verwandte Leistungsfaktoren
Normalerweise Hinweise auf die Datenbankperformance beeinflusst auch die Batchverarbeitung. Legen Sie z. B. für Tabellen mit großen Primärschlüssel oder viele nicht gruppierte Indizes die Leistung reduziert wird.

Tabellenwertparameter eine gespeicherte Prozedur verwenden, können Sie den Befehl **SET NOCOUNT ON** am Anfang der Prozedur verwenden. Dies unterdrückt die Rückgabe der Anzahl der betroffenen Zeilen in der Prozedur. Aber in unseren Tests die Verwendung von **SET NOCOUNT ON** hatte keine Auswirkung oder Leistungseinbußen. Test gespeicherte Prozedur wurde mit einer einzelnen **Einfügen** der Tabellenwertparameter Befehl. Es ist möglich, dass diese Anweisung komplexere Prozeduren profitieren würden. Aber nicht automatisch die gespeicherte Prozedur **SET NOCOUNT ON** hinzufügen die Leistung verbessert. Um den Effekt zu verstehen, testen Sie die gespeicherte Prozedur mit und ohne **SET NOCOUNT ON** -Anweisung.

## <a name="batching-scenarios"></a>Batchverarbeitung von Szenarien
In den folgenden Abschnitten wird beschrieben, wie Tabellenwertparametern in drei Anwendungsszenarien verwendet. Das erste Szenario zeigt, wie Pufferung und Batchverarbeitung zusammenarbeiten können. Das zweite Szenario verbessert die Leistung von Master / Detail-Vorgänge in einem einzelnen gespeicherten Prozeduraufruf. Das letzte Szenario veranschaulicht, wie Tabellenwertparametern in einer Operation "UPSERT".

### <a name="buffering"></a>Pufferung
Zwar einige Szenarien Kandidat für die Batchverarbeitung, gibt es viele Szenarios, die verzögerte Verarbeitung Batchverarbeitung nutzen konnte. Verzögerte Verarbeitung wird jedoch auch ein höheres Risiko, dass die Daten bei einem unerwarteten Ausfall verloren. Es ist wichtig zu verstehen dieses Risiko und die folgen.

Angenommen Sie, eine Anwendung, die den Navigationsverlauf jedes Benutzers verfolgt. Bei jeder Seitenanforderung können die Anwendung der Datenbank Seitenansicht des Benutzers aufzuzeichnen. Aber mehr Performance und Skalierung Pufferung Benutzeraktivitäten Navigation und senden diese Daten mit der Datenbank im Stapel erreicht werden. Sie können das Datenbankupdate verstrichene Zeit und/oder Puffergröße auslösen. Regel kann zum Beispiel festlegen, dass der Stapel nach 20 Sekunden erreicht Puffer 1000 Elemente verarbeitet werden sollen.

Im folgenden Codebeispiel verwendet [Reaktiven Extensions - Rx](https://msdn.microsoft.com/data/gg577609) gepufferten Ereignisse durch eine Überwachung Klasse verarbeitet. Wenn der Puffer voll oder ein Zeitlimit erreicht, der Stapel von Benutzerdaten mit einem Tabellenwertparameter gesendet.

Die folgende NavHistoryData-Klasse modelliert die Benutzerdetails Navigation. Sie enthält grundlegende Informationen wie Benutzer-ID, den URL zugegriffen und die Zugriffszeit.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

NavHistoryDataMonitor-Klasse ist verantwortlich für Pufferung Navigationsdaten Benutzer mit der Datenbank. Er enthält eine Methode, RecordUserNavigationEntry, die durch ein **OnAdded** Ereignis reagiert. Der folgende Code zeigt Konstruktorlogik, die Rx verwendet eine ObservableCollection basierend auf das Ereignis zu erstellen. Dann abonnieren dieser ObservableCollection mit dem Puffer. Die Überladung gibt an, dass der Puffer alle 20 Sekunden oder 1000 Einträge gesendet werden soll.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

Der Handler alle zwischengespeicherten Elemente in einen Tabellenwert-Typ konvertiert und übergibt diesen Typ an eine gespeicherte Prozedur, die den Stapel verarbeitet. Der folgende Code zeigt die vollständige Definition der NavHistoryDataEventArgs und NavHistoryDataMonitor-Klassen.

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }
    
    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;
    
        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }
    
        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }
    
        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }
    
            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();
    
                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;
    
                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });
    
                cmd.ExecuteNonQuery();
            }
        }
    }

Um diese Pufferung Klasse verwenden, erstellt die Anwendung ein statisches NavHistoryDataMonitor-Objekt. Jedes Mal ein Benutzer auf eine Seite zugreift, ruft die Anwendung die NavHistoryDataMonitor.RecordUserNavigationEntry-Methode. Die Pufferung Logik wird für diese Posten in Batches an die Datenbank senden.

### <a name="master-detail"></a>Master-detail
Tabellenwertparameter eignen sich für einfache Szenarien mit einfügen. Es kann jedoch schwieriger Batch eingefügt, die mehrere Tabellen umfassen. Das Szenario "Master-Detail" ist ein gutes Beispiel. Die Mastertabelle kennzeichnet die primäre Einheit. Eine oder mehrere Tabellen mehr Daten über die Entität gespeichert. In diesem Szenario erzwingen Fremdschlüssel die Beziehung enthält eindeutige master Entität. Sollten Sie eine vereinfachte Version einer Bestellung Tabelle und der zugehörigen OrderDetail-Tabelle. Die folgenden Transact-SQL PurchaseOrder Tabelle mit vier Spalten erstellt: OrderID, OrderDate, CustomerID und Status.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Jeder Auftrag enthält mindestens Produktkäufe. Diese Informationen werden in der Tabelle PurchaseOrderDetail. Die folgenden Transact-SQL erstellt die PurchaseOrderDetail-Tabelle mit fünf Spalten: OrderID, OrderDetailID, ProductID, Einzelpreis und OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

Die OrderID-Spalte in der PurchaseOrderDetail Tabelle muss eine Bestellung aus der Tabelle "PurchaseOrder" verweisen. Die folgende Definition eines Fremdschlüssels erzwingt diese Einschränkung.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

Tabellenwertparameter verwenden möchten, müssen Sie eine benutzerdefinierte Tabelle für jede Zieltabelle haben.

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO
    
    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

Anschließend definieren Sie eine gespeicherte Prozedur, die Tabellen dieser Typen akzeptiert. Dieses Verfahren kann eine Anwendung eine Reihe von Aufträgen und Auftragsdetails in einem einzelnen Aufruf lokal Batch. Die folgenden Transact-SQL bietet vollständige Prozedur Erklärung für dieses Beispiel Purchase Order.

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;
    
    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );
     
          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;
    
    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;
    
    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

In diesem Beispiel werden die lokal definierten @IdentityLink Tabelle speichert die aktuelle OrderID Werte neu eingefügten Zeilen. Diese IDs unterscheiden sich von temporären OrderID Werte in die @orders und @details Tabellenwertparametern. Aus der @IdentityLink Tabelle verbindet die OrderID-Werte aus den @orders Parameter reale OrderID Werte für neuen Zeilen in der Tabelle "Bestellung". Nach diesem Schritt der @IdentityLink Tabelle ermöglichen einfügen Auftragsdetails mit tatsächlichen OrderID, die foreign Key-Einschränkung erfüllt.

Diese gespeicherte Prozedur kann von Code oder anderen Transact-SQL-Aufrufe verwendet werden. Siehe Abschnitt Tabellenwertparameter dieses Dokuments ein Codebeispiel. Die folgenden Transact-SQL veranschaulicht den Sp_InsertOrdersBatch.

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType
    
    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')
    
    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)
    
    exec sp_InsertOrdersBatch @orders, @details

Diese Lösung ermöglicht jeder Batch OrderID Werte verwenden, die mit 1 beginnen. Diese temporäre OrderID Werte beschreiben die Beziehung im Stapel aber OrderID Istwerte werden zum Zeitpunkt des Einfügevorgangs bestimmt. Sie wiederholt dieselbe Anweisung im vorherigen Beispiel ausgeführt und Generieren eindeutiger Orders in der Datenbank. Deshalb erwägen Sie mehr Code oder Datenbank Logik, bei dem doppelte Aufträge bei Verwendung dieser Technik Batchverarbeitung.

Dieses Beispiel zeigt, dass auch komplexere Datenbankoperationen wie das Master / Detail-Operationen Tabellenwertparameter zusammengefasst werden können.

### <a name="upsert"></a>UPSERT
Batchverarbeitung Gauntlet beinhaltet vorhandene Zeilen und das Einfügen von neuen Zeilen gleichzeitig aktualisieren. Dieser Vorgang wird manchmal als "UPSERT" (Update + EINFG) Vorgang bezeichnet. Anstatt separate Aufrufe zum Einfügen und aktualisieren, wird die MERGE-Anweisung für diese Aufgabe am besten geeignet. Die MERGE-Anweisung kann beide Einfügen ausführen und Vorgänge in einem einzelnen Aufruf aktualisieren.

Tabellenwertparameter können Updates und fügt mit der MERGE-Anweisung verwendet werden. Angenommen, eine vereinfachte Mitarbeitertabelle, die folgenden Spalten enthält: EmployeeID, FirstName, LastName, SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))
 
In diesem Beispiel können Sie die Tatsache, dass die SocialSecurityNumber für mehrere Mitarbeiter Zusammenführen eindeutig ist. Erstellen Sie zuerst den benutzerdefinierten Tabellentyp:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

Anschließend erstellen Sie eine gespeicherte Prozedur oder Schreiben Sie Code, der die MERGE-Anweisung ausführen und Einfügen verwendet. Im folgenden Beispiel wird die MERGE-Anweisung für einen Tabellenwertparameter @employees, vom Typ EmployeeTableType. Der Inhalt der @employees Tabelle hier nicht angezeigt.

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

Weitere Informationen finden Sie in der Dokumentation und Beispiele für die MERGE-Anweisung. Zwar die gleiche Arbeit in einer mehrstufigen gespeicherten konnte Prozeduraufruf mit trennen einfügen und Aktualisierungsvorgänge, die MERGE-Anweisung ist effizienter. Datenbankcode kann Transact-SQL-Aufrufe auch erstellen, mit dem die MERGE-Anweisung direkt ohne zwei Datenbankaufrufe für INSERT und UPDATE.

## <a name="recommendation-summary"></a>Empfehlung Zusammenfassung

Die folgende Liste enthält eine Zusammenfassung der Batchverarbeitung Recommendations in diesem Thema erläutert:

- Pufferung und Batchverarbeitung die Leistung und Skalierung von SQL-Datenbanken verwenden.
- Verstehen Sie die Kompromisse zwischen Batchverarbeitung/Pufferung und Stabilität. Bei einem Ausfall Funktion kann das Risiko des Verlustes unverarbeitete Anzahl Unternehmensdaten Leistungsvorteil der Batchverarbeitung überwiegen.
- Alle Aufrufe an die Datenbank in einem einzigen Rechenzentrum Latenz zu versuchen.
- Wählt eine Batchverarbeitung Technik bieten Tabellenwertparameter, die beste Performance und Flexibilität.
- Für die schnellste Leistung mit einfügen die folgenden allgemeinen Richtlinien, aber Ihr Szenario testen:
    - Verwenden Sie < 100 Zeilen einen parametrisierten INSERT-Befehl.
    - Verwenden Sie < 1000 Zeilen Tabellenwertparametern.
    - Für > = 1000 Zeilen SqlBulkCopy verwenden.
- Für update und delete-Operationen, Tabellenwertparameter mit gespeicherten Prozedur, die den ordnungsgemäßen Betrieb auf jede Zeile in der Tabelle Parameter bestimmt.
- Richtlinien für Batch-Größe:
    - Verwenden der größten Batch, die für die Anwendung und Unternehmen sinnvoll.
    - Saldo den Leistungsgewinn großen Batches mit temporären oder schwerwiegenden Fehler. Was ist die Folge von Wiederholungsversuchen oder Daten im Stapel? 
    - Testen Sie den größten Batch, um sicherzustellen, dass SQL-Datenbank nicht zurückgewiesen werden.
    - Erstellen Sie Konfigurationen Batchverarbeitung dieses Steuerelement die Batchgröße oder das Zwischenspeichern Zeitfenster. Diese Standardeinstellungen bieten Flexibilität. Sie können das Batchverarbeitung Verhalten in der Produktion ohne erneute Bereitstellung Cloud-Dienst ändern.
- Vermeiden Sie parallele Ausführung von Batches, die auf einer einzelnen Tabelle in einer Datenbank. Wenn Sie eine Charge über mehrere Arbeitsthreads Teilen wählen, führen Sie Tests aus, um die ideale Anzahl der Threads zu bestimmen. Nach einer nicht angegebenen Schwellenwert werden mehr Threads die Performance beeinträchtigen anstatt erhöhen.
- Pufferung Größe und so implementieren Batchverarbeitung für Weitere Szenarien berücksichtigt.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel konzentriert sich wie Datenbank Entwurfs- und Techniken Batchverarbeitung Anwendungsleistung und Erweiterbarkeit verbessern können. Aber das ist nur ein Faktor in Ihre Gesamtstrategie. Weitere Methoden zum Verbessern der Leistungsfähigkeit und Erweiterbarkeit finden Sie unter [Azure SQL-Datenbank-Performance-Leitfaden für einzelne Datenbanken](sql-database-performance-guidance.md) und [Preis und Leistung für einen Datenbankpool elastische](sql-database-elastic-pool-guidance.md).
