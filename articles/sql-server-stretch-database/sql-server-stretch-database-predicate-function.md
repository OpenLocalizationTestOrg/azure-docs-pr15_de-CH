<properties
    pageTitle="Wählen Sie migrieren mithilfe einer Filterfunktion (Stretch Datenbank) | Microsoft Azure"
    description="Informationen Sie zum Auswählen von Zeilen mithilfe einer Filterfunktion migrieren."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="douglasl"/>

# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>Wählen Sie migrieren mithilfe einer Filterfunktion (Stretch-Datenbank)

Wenn Sie kalte Daten in einer separaten Tabelle speichern, können Sie Strecken Datenbank, um die gesamte Tabelle migrieren konfigurieren. Wenn Ihre Tabelle und Daten enthält, auf der anderen Seite soll eine Filterfunktion, markieren Sie die Zeilen migrieren. Das Filterprädikat ist eine Inline-Tabelle\-Funktion bewertet. Dieses Thema beschreibt, wie eine Inline-Tabelle schreiben\-Wert Funktion Zeilen Migration auswählen.

>   [AZURE.NOTE] Wenn Sie eine Filterfunktion, die schlechte Leistung bereitstellen, führt die Datenmigration schlecht. Stretch-Datenbank weist die Filterfunktion die Tabelle mithilfe von CROSS APPLY-Operator.

Wenn Sie eine Filterfunktion angeben, wird die gesamte Tabelle migriert.

Beim Ausführen der Datenbank aktivieren für Strecken Assistenten Migrieren einer gesamten Tabelle oder eine einfache Filterfunktion im Assistenten angeben. Soll eine andere Art von Filterfunktion zu migrieren Zeilen auswählen, haben Sie die folgenden Aktionen.

-   Beenden Sie den Assistenten, und führen Sie die Anweisung ALTER TABLE Strecken für die Tabelle aktiviert und eine Filterfunktion.

-   Führen Sie die Anweisung ALTER TABLE, um eine Filterfunktion festzulegen, nachdem Sie den Assistenten beenden.

Die ALTER TABLE-Syntax zum Hinzufügen einer Funktion wird weiter unten in diesem Thema beschrieben.

## <a name="basic-requirements-for-the-filter-function"></a>Mindestanforderungen für die Filterfunktion
Inline-Tabelle\-wichtigen Funktion für ein Filterprädikat Stretch Datenbank folgendermaßen aussieht.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE <predicate>
```
Die Parameter für die Funktion müssen Bezeichner für Spalten aus der Tabelle.

Die Bindung des Schemas muss gegen die Filterfunktion gelöscht oder verändert verwendeten Spalten.

### <a name="return-value"></a>Rückgabewert
Wenn die Funktion nicht gibt\-leere Ergebnis Zeile migriert werden kann. Andernfalls \- ist, wenn die Funktion ein Ergebnis nicht \- die Zeile kann nicht migriert werden.

### <a name="conditions"></a>Startbedingungen
Die &lt; *Prädikat* &gt; besteht eine Bedingung oder mehrere Konditionen mit dem logischen Operator und verbunden.

```
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]
```
Jede Bedingung besteht wiederum einem ursprünglichen Zustand oder mehrere primitiven Konditionen mit dem logischen Operator OR verbunden.

```
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]
```

### <a name="primitive-conditions"></a>Primitive Startbedingungen
Die folgenden Vergleiche haben ursprünglichen Zustand.

```
<primitive_condition> ::=
{
<function_parameter> <comparison_operator> constant
| <function_parameter> { IS NULL | IS NOT NULL }
| <function_parameter> IN ( constant [ ,...n ] )
}
```

-   Vergleichen Sie ein Funktionsparameter auf einen konstanten Ausdruck Z. B. `@column1 < 1000`.

    Es folgt ein Beispiel, das überprüft, ob der Wert *einer Spalte* &lt; 1 1 2016.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
    GO

    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Ein Funktionsparameter Operators IS NULL oder IS NOT NULL zuweisen.

-   Verwenden Sie den Operator IN Funktionsparameter an eine Liste von konstanten Werten vergleichen.

    Hier ist ein Beispiel, das überprüft, ob der Wert der eine *Lieferung\_Status* Spalte ist `IN (N'Completed', N'Returned', N'Cancelled')`.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

### <a name="comparison-operators"></a>Vergleichsoperatoren
Die folgenden Vergleichsoperatoren werden unterstützt.

`<, <=, >, >=, =, <>, !=, !<, !>`

```
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }
```

### <a name="constant-expressions"></a>Konstante Ausdrücke
Die Konstanten in einer Filterfunktion kann deterministischen Ausdruck, der ausgewertet werden kann, wenn Sie die Funktion definieren. Konstante Ausdrücke können Folgendes enthalten.

-   Literale. Z. B. `N’abc’, 123`.

-   Algebraische Ausdrücke. Z. B. `123 + 456`.

-   Deterministische Funktionen. Z. B. `SQRT(900)`.

-   Deterministische Konvertierung, die CAST oder CONVERT verwenden. Z. B. `CONVERT(datetime, '1/1/2016', 101)`.

### <a name="other-expressions"></a>Andere Ausdrücke
Sie können zwischen und nicht zwischen Operatoren resultierende Funktion nach zwischen ersetzen und nicht mit entsprechenden und oder Ausdrücke beschriebenen Regeln entspricht.

Sie können keine Unterabfragen oder\-deterministische Funktionen ZUFALLSZAHL() oder GETDATE().

## <a name="add-a-filter-function-to-a-table"></a>Eine Filterfunktion zu einer Tabelle hinzufügen
Eine Filterfunktion zu einer Tabelle hinzufügen, **ALTER TABLE** -Anweisung und Angeben einer vorhandenen Tabelle Inline\-Funktion als Wert der Wert der **FILTER\_Prädikat** Parameter. Zum Beispiel:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
) )
```
Nach dem Binden der Funktion wie ein Prädikat gilt Folgendes.

-   Die nächste Zeit Datenmigration wird nur die Zeilen, für die die Funktion nicht gibt\-leerer Wert migriert.

-   Die von der Funktion verwendeten Spalten sind Schema gebunden. Als eine Tabelle die Funktion als seine Filterprädikat verwenden, können Sie diese Spalten nicht ändern.

Inline-Tabelle kann nicht gelöscht\-Wert Funktion wie eine Tabelle die Funktion als seine Filterprädikat verwendet wird.

>   [AZURE.NOTE] Zum Verbessern der Leistung der Filter-Funktion von der Funktion verwendeten Spalten erstellen Sie einen Index.

### <a name="passing-column-names-to-the-filter-function"></a>Spaltennamen an die Filterfunktion übergeben
Wenn Sie einer Tabelle eine Filterfunktion zuweisen, geben Sie Spaltennamen an die Filterfunktion mit einem einteiligen Namen übergeben. Bei nachfolgende Abfragen der Strecken der dreiteiligen Namen übergeben die Spalte Namen Angabe\-aktivierte Tabelle fehl.

Wenn Sie drei Spaltennamen angeben, wie im folgenden Beispiel gezeigt, z. B. die Anweisung erfolgreich ausgeführt, aber nachfolgende Abfragen der Tabelle fehl.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

Geben Sie stattdessen die Filterfunktion mit einem einteiligen Spaltennamen wie im folgenden Beispiel gezeigt.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

## <a name="addafterwiz"></a>Fügen Sie eine Filterfunktion nach Ausführung des Assistenten hinzu  

Soll eine Funktion des **Aktivieren für Stretch** -Assistenten erstellen können, führen Sie die Anweisung ALTER TABLE, um eine Funktion anzugeben, nachdem Sie den Assistenten beenden. Anwenden eine Funktion müssen jedoch, Sie zu stoppen der Datenmigrations ist wieder migrierten Daten. (Weitere Informationen dazu, warum dies finden Sie unter [Ersetzen eine vorhandenen Filterfunktion](#replacePredicate).  

1. Umkehren der Richtung der Migration und bringen die Daten bereits migriert. Dieser Vorgang kann nicht abgebrochen werden, nach dem Start. Sie auch Kosten in Azure für ausgehende Datenübertragungen \(Ausgang\). Weitere Informationen finden Sie unter [wie Azure Preise funktioniert](https://azure.microsoft.com/pricing/details/data-transfers/).  

    ```tsql  
    ALTER TABLE <table name>  
         SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  

2. Wartezeit für die Migration zu beenden. Überprüfen Sie den Status im **Abschnitt Datenbank überwachen** von SQL Server Management Studio oder die Ansicht **sys.dm_db_rda_migration_status** Abfragen. Weitere Informationen finden Sie [Überwachen und Problembehandlung Datenmigration von](sql-server-stretch-database-monitor.md) oder [sys.dm_db_rda_migration_status](https://msdn.microsoft.com/library/dn935017.aspx).  

3. Erstellen Sie die Filterfunktion, die auf die Tabelle angewendet werden soll.  

4. Die Funktion der Tabelle hinzufügen und Datenmigration in Azure starten.  

    ```tsql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
        );   
    ```  

## <a name="filter-rows-by-date"></a>Zeilen nach Datum filtern
Im folgende Beispiel werden Zeilen **die Spalte** einen Wert vor dem 1 Januar 2016 enthält migriert.

```tsql
-- Filter by date
--
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)
GO
```

## <a name="filter-rows-by-the-value-in-a-status-column"></a>Filtern von Zeilen nach dem Wert in einer Spalte
Im folgenden Beispiel wird migriert Zeilen die Spalte **Status** der angegebenen Werte enthält.

```tsql
-- Filter by status column
--
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')
GO
```

## <a name="filter-rows-by-using-a-sliding-window"></a>Filtern von Zeilen in einem verschiebbaren Fenster
Um Zeilen zu filtern, indem ein Schiebefenster dabei folgende für die Filterfunktion.

-   Die Funktion muss deterministisch sein. Daher kann keine Funktion erstellen, die gleitende Fenster Zeit berechnet.

-   Die Funktion verwendet die Bindung des Schemas. Daher kann nicht einfach die Funktion "an der Stelle" jeden Tag Aktualisierung **ALTER FUNCTION** zum Verschieben des verschiebbaren Fensters.

Beginnen Sie mit einer Filterfunktion wie beispielsweise die Zeilen, in denen die **SystemEndTime** Spalte vor 1 Januar 2016 enthält, migriert.

```tsql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING  
AS  
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;
```

Gelten Sie die Filterfunktion Tabelle.

```tsql
ALTER TABLE <table name>
SET (
        REMOTE_DATA_ARCHIVE = ON
                (
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101 (SysEndTime)
                                , MIGRATION_STATE = OUTBOUND
                )
        )
;
```

Gehen Sie folgendermaßen vor, wenn Sie das gleitende Fenster aktualisieren möchten.

1.  Erstellen Sie eine neue Funktion, die das neue Schiebefenster angibt. Das folgende Beispiel wählt Datumsangaben vor dem 2. Januar 2016 statt 1 Januar 2016.

2.  Ersetzen Sie vorherige Filterfunktion durch die neue aufrufende **ALTER TABLE**wie im folgenden Beispiel gezeigt.

3. Legen Sie gegebenenfalls die vorherigen Filter-Funktion, die Sie nicht mehr verwenden von **DROP FUNCTION**. (Im Beispiel wird dieser Schritt nicht angezeigt.)

```tsql
BEGIN TRAN
GO
        /*(1) Create new predicate function definition */
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)
        RETURNS TABLE
        WITH SCHEMABINDING
        AS
        RETURN SELECT 1 AS is_eligible
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)
        GO

        /*(2) Set the new function as the filter predicate */
        ALTER TABLE <table name>
        SET
        (
               REMOTE_DATA_ARCHIVE = ON
               (
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),
                       MIGRATION_STATE = OUTBOUND
               )
        )
COMMIT ;
```

## <a name="more-examples-of-valid-filter-functions"></a>Weitere Beispiele für gültige Filterfunktionen

-   Im folgende Beispiel kombiniert zwei Bedingungen mit dem logischen Operator.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Das folgende Beispiel verwendet verschiedene Fehlerzustände und eine deterministische Konvertierung mit konvertieren.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL)AND @column3 IN (N'Completed', N'Returned', N'Cancelled')
    GO
    ```

-   Im folgenden Beispiel mathematischen Operatoren und Funktionen.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < SQRT(400) + 10
    GO
    ```

-   Im folgenden Beispiel zwischen und nicht zwischen Operatoren. Diese Verwendung ist gültig, da die resultierende Funktion entspricht nach zwischen ersetzen und nicht mit entsprechenden und oder Ausdrücke beschriebenen Regeln.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 BETWEEN 0 AND 100
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)
    GO
    ```
    Die vorhergehende Funktion entspricht der folgenden Funktion nach zwischen und nicht zwischen Operatoren entspricht und oder Ausdrücke ersetzen.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 >= 0 AND @column1 <= 100AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)
    GO
    ```

## <a name="examples-of-filter-functions-that-arent-valid"></a>Filter-Funktionen, die nicht gültig sind

-   Die folgende Funktion ist ungültig, da er nicht enthält\-deterministische Konvertierung.

    ```tsql
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016')
    GO
    ```

-   Die folgende Funktion ist ungültig, da er nicht enthält\-deterministischen Funktionsaufruf.

    ```tsql
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < DATEADD(day, -60, GETDATE())
    GO
    ```

-   Die folgende Funktion ist ungültig, da er eine Unterabfrage enthält.

    ```tsql
    CREATE FUNCTION dbo.fn_example7(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct'))
    GO
    ```

-   Die folgenden Funktionen sind ungültig da Ausdrücke, algebraischen Operatoren oder erstellt\-Funktionen müssen zu einer Konstanten auswerten beim Definieren der Funktion. Sie können keine Spaltenverweise algebraische Ausdrücke oder Funktionsaufrufe enthalten.

    ```tsql
    CREATE FUNCTION dbo.fn_example8(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 % 2 =  0
    GO

    CREATE FUNCTION dbo.fn_example9(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE SQRT(@column1) = 30
    GO
    ```

-   Die folgende Funktion ist ungültig, da es verletzt die Regeln der entsprechende und Ausdruck den Operator BETWEEN ersetzen hier beschrieben.

    ```tsql
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```
    Die vorhergehende Funktion entspricht der folgenden Funktion nach der entsprechende und Ausdruck den Operator BETWEEN ersetzen. Diese Funktion ist nicht zulässig, da primitive Umständen nur den logischen oder-Operator verwenden können.

    ```tsql
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```

## <a name="how-stretch-database-applies-the-filter-function"></a>Wie die Filterfunktion gilt Stretch-Datenbank
Dehnen Datenbank gilt die Filterfunktion für die Tabelle und können Zeilen mit CROSS APPLY-Operator bestimmt. Zum Beispiel:

```tsql
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)
```
Wenn die Funktion nicht gibt\-leere Ergebnis für die Zeile die Zeile darf migriert werden.

## <a name="replacePredicate"></a>Ersetzen Sie eine vorhandenen Filterfunktion
Ersetzen eine zuvor festgelegten Filterfunktion von **ALTER TABLE** -Anweisung erneut ausführen und einen neuen Wert für die **FILTER\_Prädikat** Parameter. Zum Beispiel:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
```
Neue Inline-Tabelle\-bewertet Funktion hat folgende.

-   Die neue Funktion muss weniger restriktiv als die vorherige Funktion.

-   Alle in der alten Funktion war Operatoren müssen in der neuen Funktion vorhanden.

-   Die neue Funktion kann keine Operatoren enthalten, die nicht in der alten Funktion vorhanden.

-   Operator-Argumente kann nicht ändern.

-   Nur konstante Werte, die Teil einer `<, <=, >, >=` Vergleich in einer Weise, die die Funktion weniger restriktive geändert werden.

### <a name="example-of-a-valid-replacement"></a>Beispiel für einen gültigen Ersatz
Angenommen Sie, die folgende Funktion die aktuelle Filterprädikat.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_old (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Die folgende Funktion ist gültig Ersatz, denn die neue Datumskonstante (die später Stichtag angibt) die Funktion weniger restriktiv.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_new (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)
            AND (@column2 < -50 OR @column2 > 50)
GO
```

### <a name="examples-of-replacements-that-arent-valid"></a>Beispiele für Ersatzteile, die nicht gültig sind
Die folgende Funktion ist kein gültiger Ersatz da neue Datumskonstante (die eine zuvor Stichtag angibt) die restriktivste funktionieren nicht.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_1 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Die folgende Funktion ist kein gültiger Ersatz da einer der Vergleichsoperatoren entfernt.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_2 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -50)
GO
```
Die folgende Funktion ist kein gültiger Ersatz eine neue Bedingung mit dem logischen Operator hinzugefügt.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_3 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
            AND (@column2 <> 0)
GO
```

## <a name="remove-a-filter-function-from-a-table"></a>Eine Filterfunktion aus einer Tabelle entfernen
Entfernen, um die gesamte Tabelle statt markierten Zeilen zu migrieren, die vorhandene Funktion indem **FILTER\_Prädikat** auf Null. Zum Beispiel:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = NULL,
    MIGRATION_STATE = <desired_migration_state>
) )
```
Nach dem Entfernen der Filter-Funktion werden alle Zeilen in der Tabelle für die Migration. Daher können nicht Sie eine Filterfunktion für die gleiche Tabelle später angeben, wenn Sie wieder alle remote-Daten für die Tabelle von Azure zuerst bringen Diese Einschränkung um zu vermeiden, in denen Zeilen, die nicht für die Migration beim bieten einer neue Filterfunktion in Azure, bereits migriert sind, vorhanden ist.

## <a name="check-the-filter-function-applied-to-a-table"></a>Überprüfen Sie die Filterfunktion auf eine Tabelle angewendet
Überprüfen Sie die Filterfunktion auf eine Tabelle angewendet Öffnen der Katalogansicht **sys.remote\_Daten\_Archiv\_Tabellen** und überprüfen Sie den Wert der **Filter\_Prädikat** Spalte. Wenn der Wert null ist, wird die gesamte Tabelle für die Archivierung. Weitere Informationen finden Sie unter [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="security-notes-for-filter-functions"></a>Sicherheitshinweise für Filterfunktionen  
Einem kompromittierten Konto mit Db_owner-Berechtigungen kann folgende Aktionen ausführen.  

-   Erstellen und Anwenden einer Tabellenwertfunktion, die große Mengen von Serverressourcen verbraucht oder für einen längeren Zeitraum, was zu einem Denial of Service wartet.  

-   Erstellen und Anwenden einer Tabellenwertfunktion, die es ermöglicht, den Inhalt einer Tabelle abzuleiten für die der Benutzer explizit Zugriff verweigert wurde.  

## <a name="see-also"></a>Siehe auch

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
