<properties
   pageTitle="Transaktionen im SQL Datawarehouse | Microsoft Azure"
   description="Tipps für die Implementierung von Transaktionen in Azure SQL Data Warehouse Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/31/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="transactions-in-sql-data-warehouse"></a>Transaktionen im SQL Datawarehouse

Wie zu erwarten, unterstützt SQL Data Warehouse Transaktionen als Teil der Data Warehouse-Workloads. Sind jedoch um sicherzustellen, dass die Leistung von SQL Data Warehouse Ebene werden einige Features beschränkt im Vergleich zu SQL Server. In diesem Artikel werden die Unterschiede und andere Listen. 

## <a name="transaction-isolation-levels"></a>Isolationsstufen von Transaktionen
SQL Data Warehouse implementiert ACID-Transaktionen. Die Isolation der transaktionsbezogenen Unterstützung ist jedoch auf `READ UNCOMMITTED` und kann nicht geändert werden. Implementieren Sie verschiedene Methoden zum geänderter Daten zu verhindern, ist dies ein Problem für Sie programmieren. Die am häufigsten verwendeten Methoden nutzen CTAS und Tabelle Partition wechseln (häufig als gleitende Fenstermuster bezeichnet) verhindert das Abrufen von Daten noch vorbereitet. Ansichten, die die Daten vor-filter ist auch ein beliebter Ansatz.  

## <a name="transaction-size"></a>Transaktionsgröße
Eine einzige Änderung Transaktion ist Größe beschränkt. Das Limit wird heute "pro Verteilung". Daher kann insgesamt multipliziert den Grenzwert von der Verteilungsanzahl berechnet werden. Zum Teilen ungefähr der maximalen Anzahl der Zeilen in der Transaktion Cap Verteilung durch die Gesamtgröße der einzelnen Zeilen. Sollten Sie für Spalten mit variabler Länge eine durchschnittliche Spalte Länge anstatt die maximale Größe.

In der Tabelle die folgenden Annahmen wurden:

* Gleichmäßige Verteilung der Daten aufgetreten 
* Die durchschnittliche Zeilenlänge beträgt 250 Byte

| [DWU][]    | Cap pro Verteilung (GiB) | Anzahl der Verteilung | Max. Größe der Transaktion (GiB) | # Zeilen pro Verteilung | Maximale Zeilenanzahl pro Buchung |
| ------ | -------------------------- | ----------------------- | -------------------------- | ----------------------- | ------------------------ |
| DW100  |  1                         | 60                      |   60                       |   4.000.000             |    240,000,000           |
| DW200  |  1.5                       | 60                      |   90                       |   6.000.000             |    360,000,000           |
| DW300  |  2,25                      | 60                      |  135                       |   9.000.000             |    540,000,000           |
| DW400  |  3                         | 60                      |  180                       |  12.000.000             |    720,000,000           |
| DW500  |  3,75                      | 60                      |  225                       |  15.000.000             |    900.000.000           |
| DW600  |  4.5                       | 60                      |  270                       |  18,000,000             |  1,080,000,000           |
| DW1000 |  7.5                       | 60                      |  450                       |  30.000.000             |  1,800,000,000           |
| DW1200 |  9                         | 60                      |  540                       |  36.000.000             |  2,160,000,000           |
| DW1500 | 11,25                      | 60                      |  675                       |  45.000.000             |  2,700,000,000           |
| DW2000 | 15                         | 60                      |  900                       |  60.000.000             |  3,600,000,000           |
| DW3000 | 22,5                       | 60                      |  1.350                     |  90.000.000             |  5,400,000,000           |
| DW6000 | 45                         | 60                      |  2700                     | 180.000.000             | 10,800,000,000           |

Maximale Größe der Transaktion wird pro Transaktion oder Vorgang. Es wird nicht über alle gleichzeitigen Transaktionen angewendet. Deshalb darf jede Transaktion dieses Datenmengen in das Protokoll schreiben. 

Zu optimieren und die Menge der Daten in das Protokoll geschrieben finden Sie im Artikel [best Practices für Transaktionen][] .

> [AZURE.WARNING] Die maximale Transaktionsgröße lässt für HASH oder sogar verteilten ROUND_ROBIN Tabellen die Verbreitung von Daten ist. Die Transaktion auf die Daten verzerrt Weise schreiben ist das Limit vor Buchung maximale Größe erreicht.
<!--REPLICATED_TABLE-->

## <a name="transaction-state"></a>Transaktion
SQL Data Warehouse verwendet die XACT_STATE() Funktion eine fehlgeschlagene Transaktion mit dem Wert-2 zu melden. Dies bedeutet, dass die Transaktion fehlgeschlagen und nur Rollback markiert

> [AZURE.NOTE] Die Verwendung von-2 von der XACT_STATE-Funktion auf eine fehlgeschlagene Transaktion kennzeichnen stellt SQL Server anders dar. SQL Server verwendet den Wert-1 nicht commitfähige Transaktion darstellen. SQL Server tolerieren können einige Fehler innerhalb einer Transaktion ohne als nicht commitfähige gekennzeichnet werden. Beispielsweise `SELECT 1/0` verursachen Fehler jedoch nicht erzwingen eine Transaktion in einen Commit nicht ausgeführt werden. SQL Server lässt auch Lesevorgänge in nicht commitfähige Transaktion. Jedoch lässt SQL Data Warehouse nicht dazu. Bei einem in einer Transaktion SQL Data Warehouse Fehler-2 Zustand wird automatisch eingefügt, und Sie werden nicht jede weitere Anweisungen auswählen, bis die Anweisung ein Rollback ausgeführt wurde. Daher ist es wichtig, dass Anwendungscode auf XACT_STATE() wie verwendet Code ändern muss.

Beispielsweise kann in SQL Server eine Transaktion angezeigt, die folgendermaßen aussieht:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Wenn Sie Code lassen sich über erhalten Sie folgende Fehlermeldung:

Msg 111233, Ebene 16, Status 1, Zeile 1 111233. Die aktuelle Transaktion wurde abgebrochen, und alle ausstehenden Änderungen haben wurde zurückgesetzt. Ursache: Rollback einer Transaktion in einem Rollback nur Zustand wurde nicht explizit vor einer DDL und DML-SELECT-Anweisung.

Sie erhalten auch nicht die Ausgabe der ERROR_ *-Funktionen.

Der Code muss im SQL Data Warehouse leicht geändert werden:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Jetzt wird das erwartete Verhalten beobachtet. Fehler in der Transaktion verwaltet und ERROR_ * Funktionen geben Werte wie erwartet.

Alle geänderten ist, die `ROLLBACK` der Transaktion hatte vor das Lesen der Informationen in der `CATCH` Block.

## <a name="errorline-function"></a>Error_Line()-Funktion
Es ist auch erwähnenswert, dass SQL Data Warehouse nicht implementiert oder ERROR_LINE() Funktion unterstützt. Haben Sie dies im Code müssen Sie mit SQL Data Warehouse zu entfernen. Stattdessen Sie Abfrage Etiketten im Code entsprechende Funktionalität implementieren. [Bezeichnung][] Artikel Weitere Informationen zu dieser Funktion finden.

## <a name="using-throw-and-raiserror"></a>THROW und RAISERROR verwenden
Moderne Implementierung zum Auslösen von Ausnahmen im SQL Data Warehouse ist RAISERROR wird jedoch ebenfalls unterstützt. Es gibt einige Unterschiede, die Aufmerksamkeit auf jedoch.

- Benutzerdefinierte Fehlermeldungen werden Zahlen im Bereich 100.000 150.000 THROW nicht möglich
- RAISERROR-Fehlermeldungen werden bei 50.000 behoben.
- Verwendung von sys.messages wird nicht unterstützt.

## <a name="limitiations"></a>Limitiations
SQL Data Warehouse verfügt über einigen Einschränkungen, die sich auf Transaktionen beziehen.

Sie sind wie folgt:

- Keine verteilten Transaktionen
- Keine geschachtelten Transaktionen zulässig
- Nr. speichern Punkte zulässig
- Keine benannten Transaktionen
- Keine markierten Transaktionen
- Keine Unterstützung für DDL wie `CREATE TABLE` innerhalb von einem Benutzer definiert Transaktion

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Optimieren von Transaktionen finden Sie unter [best Practices für Transaktionen][].  Zur optimalen SQL Data Warehouse finden Sie unter [SQL Data Warehouse best Practices][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[development overview]: ./sql-data-warehouse-overview-develop.md
[Best Practices für Transaktionen]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Bewährte SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[BEZEICHNUNG]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
