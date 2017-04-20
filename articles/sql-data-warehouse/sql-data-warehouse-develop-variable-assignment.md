<properties
   pageTitle="Zuweisen von Variablen im SQL Data Warehouse | Microsoft Azure"
   description="Tipps für das Zuweisen von Transact-SQL-Variablen in Azure SQL Data Warehouse Lösungen."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="assign-variables-in-sql-data-warehouse"></a>Zuweisen von Variablen im SQL Data Warehouse
Variablen im SQL Data Warehouse werden festgelegt, mit der `DECLARE` Anweisung oder `SET` Anweisung.

Die folgenden gibt gültige Variable festgelegt:

## <a name="setting-variables-with-declare"></a>Festlegen von Variablen deklarieren

Variablen mit DECLARE ist der flexibelste Weise einen Variablenwert im SQL Data Warehouse festlegen.

```sql
DECLARE @v  int = 0
;
```

DECLARE können Sie mehrere Variablen gleichzeitig festlegen. Sie können keine `SELECT` oder `UPDATE` dazu:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Sie können keine initialisieren und eine Variable in der gleichen Anweisung deklarieren. Zum Beispiel unten belegen darf **nicht** als @p1 initialisiert sowohl in derselben DECLARE-Anweisung verwendet. Dies führt zu einem Fehler.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Werte mit
Ist eine weit verbreitete Methode für eine Variable festlegen.

Alle der folgenden Beispiele sind gültige Arten Festlegen einer Variablen mit:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Eine Variable kann nur gleichzeitig mit festgelegt werden. Allerdings sind zusammengesetzte Operatoren wie oben ersichtlich ist zulässig.

## <a name="limitations"></a>Grenzen
Sie können nicht für Variable Zuordnung aktivieren oder UPDATE verwenden.


## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Anwendungsentwicklung][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Anwendungsentwicklung]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
