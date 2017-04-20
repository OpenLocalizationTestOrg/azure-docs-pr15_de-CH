<properties
   pageTitle="Migrieren den SQL-Code zu SQL Data Warehouse | Microsoft Azure"
   description="Tipps für das Migrieren von Code SQL Azure SQL Data Warehouse Lösungen."
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
   ms.date="08/02/2016"
   ms.author="lodipalm;barbkess;sonyama;jrj"/>

# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>Migrieren des SQL-Codes zu SQL Data Warehouse

Beim Migrieren von Code aus einer anderen Datenbank SQL Data Warehouse müssen Sie wahrscheinlich Ihre Codebasis zu ändern. Einige SQL Data Warehouse-Funktionen können die Leistung erheblich verbessern, sie verteilte arbeiten sollen. Allerdings sind Leistung und Skalierung einige Features auch nicht verfügbar.

## <a name="common-t-sql-limitations"></a>Allgemeine T-SQL-Grenzen

Die folgende Liste enthält die am häufigsten verwendete Funktion Azure SQL Data Warehouse nicht unterstützt werden. Die Links gelangen Sie zu Abhilfemaßnahmen für nicht unterstützte Funktion:

- [ANSI-Joins Updates][]
- [ANSI-Joins löschen][]
- [MERGE-Anweisung][]
- datenbankübergreifende joins
- [Cursor][]
- [WÄHLEN SIE... IN][]
- [EINFÜGEN... EXEC][]
- OUTPUT-Klausel
- Benutzerdefinierte Inlinefunktionen
- Multi-Anweisung funktioniert
- [Allgemeine Tabellenausdrücke](#Common-table-expressions)
- [rekursive allgemeine Tabellenausdrücke (CTE)] (#Recursive-common-table-expressions-(CTE)
- CLR-Funktionen und Prozeduren
- $partition-Funktion
- Tabellenvariablen
- Werteparameter
- verteilte Transaktionen
- Commit / Rollback Arbeit
- Transaktion speichern
- Ausführungskontexte (Ausführen als)
- [Group by-Klausel mit / cube / Legt Optionen gruppieren][]
- [Schachtelungsebenen über 8][]
- [Sichten aktualisieren][]
- [Wählen Sie für die Variable Zuweisung verwenden][]
- [kein MAX Datentyp für dynamische SQL-Zeichenfolgen][]

Glücklicherweise können die meisten dieser Einschränkungen umgangen werden. Erläuterung werden im genannten Artikel relevanten Entwicklung bereitgestellt.

## <a name="supported-cte-features"></a>CTE nutzen

Allgemeine Tabellenausdrücke (CTEs) werden im SQL Data Warehouse unterstützt.  Die folgenden CTE Funktionen werden derzeit unterstützt:

- Ein CTE kann in einer SELECT-Anweisung angegeben werden.
- Ein CTE kann in einer CREATE VIEW-Anweisung angegeben werden.
- Ein CTE kann in einer Anweisung erstellen Tabelle als auswählen (CTAS) angegeben werden.
- Ein CTE kann in einer Anweisung erstellen ENTFERNTE Tabelle als auswählen (CRTAS) angegeben werden.
- Ein CTE kann in einer Anweisung erstellen externe Tabelle als auswählen (CETAS) angegeben werden.
- Eine Remotetabelle kann über einen CTE verwiesen werden.
- Eine externe Tabelle kann aus einem CTE verwiesen werden.
- Mehrere Abfragedefinitionen CTE können in einen CTE definiert werden.

## <a name="cte-limitations"></a>CTE Grenzen

Allgemeine Tabellenausdrücke weisen einige Nachteile im SQL Data Warehouse einschließlich:

- Ein CTE muss von einer SELECT-Anweisung folgen. INSERT, UPDATE, DELETE und MERGE-Anweisung nicht unterstützt.
- Ein allgemeiner Tabellenausdruck, die Verweise auf sich selbst (einen rekursiven allgemeinen Tabellenausdrucks) wird nicht unterstützt (siehe unten).
- Mehr als eine Klausel ist in einen CTE nicht zulässig. Beispielsweise ein CTE_query_definition eine Unterabfrage enthält, kann nicht diese Unterabfrage eine geschachtelte-Klausel enthalten, einem anderen CTE definiert.
- ORDER BY-Klausel kann in der CTE_query_definition nur verwendet werden, wenn eine TOP-Klausel angegeben ist.
- Wenn ein CTE in einer Anweisung, die Teil eines Stapels ist verwendet, muss die Anweisung vor ein Semikolon folgen.
- Bei Abschlüssen Sp_prepare verhält CTEs wie anderen SELECT-Anweisungen in PDW. Jedoch als Teil des CETAS Sp_prepare vorbereitet CTEs verwendet, kann das Verhalten von SQL Server und anderen PDW aufgrund der zurückstellen Bindung für Sp_prepare implementiert. Wenn Sie Verweise, die CTE eine falsche Spalte verwendet, die in den CTE nicht vorhanden ist, übergibt der Sp_prepare ohne den Fehler zu ermitteln, dass der Fehler stattdessen während der Sp_execute ausgelöst.

## <a name="recursive-ctes"></a>Rekursive CTEs

Rekursive CTEs sind im SQL Data Warehouse nicht unterstützt.  Umstellung der rekursive CTE kann etwas abgeschlossen und des optimale Prozess brechen die in mehreren Schritten. Normalerweise können Sie eine temporäre Tabelle füllen, wie die vorläufige rekursive Abfragen durchlaufen und verwenden Sie eine Schleife. Wenn die temporäre Tabelle aufgefüllt können Sie die Daten als ein einzelnes Resultset zurückgeben. Ein ähnlicher Ansatz wird zu `GROUP BY WITH CUBE` in der [group by-Klausel mit / cube / Gruppierungsoptionen legt][] Artikel.

## <a name="unsupported-system-functions"></a>Nicht unterstützte Funktionen

Es gibt auch einige Funktionen, die nicht unterstützt werden. Einige der wichtigsten finden Sie in der Regel könnte in Data Warehouse verwendet werden:

- 'NEWSEQUENTIALID()'
- @@NESTLEVEL()
- @@IDENTITY()
- @@ROWCOUNT()
- ROWCOUNT_BIG
- ERROR_LINE()

Einige dieser Probleme können umgangen werden.

## <a name="rowcount-workaround"></a>@@ROWCOUNTAbhilfe

Fehlende Unterstützung für umgehen @@ROWCOUNT, Erstellen einer gespeicherten Prozedur, die die letzte Zeilenanzahl aus sys.dm_pdw_request_steps abgerufen und dann ausführen `EXEC LastRowCount` nach einer DML-Anweisung.

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a>Nächste Schritte
Eine vollständige Liste aller unterstützten T-SQL-Anweisung finden Sie unter [Transact-SQL-Themen][].

<!--Image references-->

<!--Article references-->
[ANSI-Joins Updates]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[ANSI-Joins löschen]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[MERGE-Anweisung]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[EINFÜGEN... EXEC]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Transact-SQL-Themen]: ./sql-data-warehouse-reference-tsql-statements.md

[Cursor]: ./sql-data-warehouse-develop-loops.md
[WÄHLEN SIE... IN]: ./sql-data-warehouse-develop-ctas.md#selectinto
[Group by-Klausel mit / cube / Legt Optionen gruppieren]: ./sql-data-warehouse-develop-group-by-options.md
[Schachtelungsebenen über 8]: ./sql-data-warehouse-develop-transactions.md
[Sichten aktualisieren]: ./sql-data-warehouse-develop-views.md
[Wählen Sie für die Variable Zuweisung verwenden]: ./sql-data-warehouse-develop-variable-assignment.md
[kein MAX Datentyp für dynamische SQL-Zeichenfolgen]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->
