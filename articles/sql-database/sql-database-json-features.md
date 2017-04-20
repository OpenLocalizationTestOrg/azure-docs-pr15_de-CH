<properties
    pageTitle="Azure SQL-Datenbank JSON-Funktionen | Microsoft Azure"
    description="Azure SQL-Datenbank können Sie analysieren, Abfrage und Formatieren von Daten in JavaScript Object Notation (JSON)-Notation."
    services="sql-database"
    documentationCenter=""
    authors="jovanpop-msft"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/17/2016"
    ms.author="jovanpop"
   ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Erste Schritte mit JSON in Azure SQL-Datenbank

Azure SQL-Datenbank können Sie analysieren und Abfragen von Daten in JavaScript Object Notation [(JSON)](http://www.json.org/) -Format dargestellt und Ihren relationalen Daten als JSON-Text exportieren.

JSON ist ein bekanntes Format für den Datenaustausch in modernem Web und mobile verwendet. JSON dient auch zum Speichern von halbstrukturierten Daten in Protokolldateien oder NoSQL-Datenbanken wie [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). Viele REST-Webdienst zurückgegebenen Ergebnissen als JSON-Text formatiert oder Daten als JSON formatiert. Die meisten Azure Services [Azure Search](https://azure.microsoft.com/services/search/) [Azure Storage](https://azure.microsoft.com/services/storage/)und [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) haben REST-Endpunkte zurück oder JSON nutzen.

Azure SQL-Datenbank können Sie problemlos mit JSON-Daten arbeiten und modernen Dienste Ihre Datenbank integrieren.

## <a name="overview"></a>Übersicht

Azure SQL-Datenbank bietet die folgenden Funktionen für die Arbeit mit JSON-Daten:

![JSON-Funktionen](./media/sql-database-json-features/image_1.png)

Haben JSON-Text können Daten aus JSON extrahieren oder JSON ordnungsgemäß formatiert ist, die integrierte Funktionen [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx)und [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx)überprüfen. Die [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) -Funktion können Sie Wert in JSON-Text aktualisieren. Für Abfragen und Analysen erweiterte, verwandeln [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) Funktion Array von JSON-Objekten in einer Reihe von Zeilen. Eine SQL-Abfrage kann auf den zurückgegebenen Resultsets ausgeführt werden. Schließlich gibt es eine [Für JSON](https://msdn.microsoft.com/library/dn921882.aspx) -Klausel, Sie können, Daten in relationalen Tabellen als JSON-Text formatieren.

## <a name="formatting-relational-data-in-json-format"></a>Formatieren von relationalen Daten im JSON-format
Haben Sie einen Webdienst übernimmt Daten aus der Datenbank layer und werden im JSON-format oder clientseitige JavaScript-Frameworks oder Bibliotheken, die Daten als JSON formatiert, können Sie den Inhalt der Datenbank direkt in einer SQL-Abfrage als JSON formatieren. Sie Anwendungscode schreiben, die Ergebnisse von Azure SQL-Datenbank als JSON formatiert oder einige JSON Serialisierung Bibliothek konvertieren tabellarische Ergebnisse und Objekte in das JSON-Format zu serialisieren. Stattdessen können Sie für JSON-Klausel Abfrageergebnisse SQL JSON in Azure SQL-Datenbank und einen direkt in der Anwendung verwenden.

Im folgenden Beispiel werden Zeilen aus der Tabelle Sales.Customer mit der Klausel für JSON als JSON formatiert:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

-Klausel für JSON Pfad formatiert die Ergebnisse der Abfrage als JSON-Text. Spaltennamen werden als Schlüssel verwendet, während die Zellwerte als JSON-Werte generiert werden:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

Die Ergebnismenge wird als JSON-Array formatiert, in dem jede Zeile als separates Objekt JSON formatiert ist.

Pfad Gibt das Ausgabeformat der JSON-Ergebnis mit Punktnotation in Spaltenaliase angepasst werden kann. Die folgende Abfrage ändert den Namen des Schlüssels "Kundenname" im JSON-Ausgabeformat und setzt Telefon-und Faxnummern in das Unterobjekt "Kontakt":

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

Die Ausgabe der Abfrage sieht folgendermaßen aus:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

In diesem Beispiel wird zurückgegeben ein einzelnes JSON-Objekt anstelle eines Arrays von angeben [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) . Diese Option können Sie wissen, dass Sie ein einzelnes Objekt durch Abfrage zurückgeben.

Der wichtigste Wert für JSON-Klausel ist können Sie komplexe hierarchische Daten aus Ihrer Datenbank als geschachtelte JSON-Objekte oder Arrays zurück. Das folgende Beispiel zeigt, wie Aufträge, die dem Kunden als eine geschachtelte Orders gehören:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

Anstatt separate Abfragen Daten abrufen und dann zum Abrufen einer Liste der verknüpften Aufträge erhalten Sie alle erforderlichen Daten mit einer einzigen Abfrage wie der folgenden Beispielausgabe dargestellt:

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a>Arbeiten mit JSON-Daten

Haben Sie unbedingt strukturierte Daten haben komplexe untergeordnete Objekte, Arrays oder hierarchische Daten oder Ihre Datenstrukturen über die Zeit weiterentwickeln, hilft das JSON-Format Sie komplexe Datenstruktur darstellen.

JSON ist ein Textformat, das wie jede andere Zeichenfolge in Azure SQL-Datenbank verwendet werden kann. Sie senden oder JSON-Daten als standard NVARCHAR speichern:

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

In diesem Beispiel verwendete JSON-Daten werden mit dem Typ NVARCHAR(MAX) dargestellt. JSON kann in diese Tabelle eingefügt oder als Argument für die gespeicherte Prozedur mit Transact-SQL-Standardsyntax wie im folgenden Beispiel gezeigt:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Client-Sprache oder Bibliothek mit Zeichenfolgendaten in Azure SQL-Datenbank funktioniert auch mit JSON-Daten. JSON kann in einer Tabelle gespeichert, die den Typ NVARCHAR wie Speicher optimiert oder eine System-Version unterstützt. Einschränkung der clientseitigen Code oder in der Datenbankschicht vorstellen JSON nicht.

## <a name="querying-json-data"></a>JSON-Daten Abfragen

Daten als JSON in Azure SQL-Tabellen gespeichert haben, sollten JSON Funktionen dieser Daten in einer SQL-Abfrage verwenden.

JSON-Funktionen, die in Azure SQL-Datenbank mit behandeln Daten als JSON als anderen SQL-Datentyp. Sie können problemlos Extrahieren von Werten aus den JSON-Text und JSON-Daten in einer Abfrage verwenden:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

Die Funktion JSON_VALUE extrahiert einen Wert von JSON-Text in der Spalte gespeichert. Diese Funktion verwendet JavaScript wie Pfad auf einen Wert in JSON-Text extrahiert. Der extrahierte Wert kann in einem beliebigen Teil der SQL-Abfrage verwendet werden.

JSON_VALUE ähnelt die Funktion JSON_QUERY. Im Gegensatz zu JSON_VALUE extrahiert diese Funktion komplexe Unterobjekt Arrays oder Objekte, die in JSON-Text platziert.

JSON_MODIFY-Funktion können Sie die Pfadangabe des Werts in JSON-Text, der aktualisiert werden soll, sowie einen neuen Wert ein, der die alte Version überschreiben. Auf diese Weise können leicht JSON-Text Aktualisierung ohne Analyse der gesamten Struktur.

JSON in ein gespeichert ist, gibt keine Garantie, dass Spalten Werte korrekt formatiert sind. Sie können überprüfen, ob in JSON Spalte gespeicherten formatierten mit standardmäßigen Azure SQL-Datenbank Prüf-Integritätsregeln und die ISJSON-Funktion:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Wenn der eingegebene Text ordnungsgemäß formatiert ist JSON, ISJSON-Funktion gibt den Wert 1. Für alle INSERT- oder Update JSON-Spalte überprüft diese Einschränkung, dass neue Textwert nicht fehlerhafte JSON.

## <a name="transforming-json-into-tabular-format"></a>Transformieren von JSON in Tabellenform

Azure SQL-Datenbank können auch JSON-Sammlungen in tabellarischen Format und laden oder JSON-Daten transformieren.

OPENJSON ist eine Tabellenfunktion, die JSON-Text analysiert, sucht ein Array von JSON-Objekten, durchläuft die Elemente des Arrays und gibt eine Zeile im Ergebnis für jedes Element des Arrays zurück.

![Tabellarische JSON](./media/sql-database-json-features/image_2.png)

Im obigen Beispiel können wir angeben, wo das JSON-Array suchen, das geöffnet werden soll (in der $. Aufträge-Pfad) sollte Spalten als Ergebnis und wo zu Zellen zurückgegeben wird die JSON-Werte zurückgegeben werden.

Verwandeln wir JSON-Array in der @orders Variable in einer Reihe von Zeilen, analysieren diese Ergebnismenge oder eine Standardtabelle Zeilen einfügen:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
Die Auflistung der Aufträge als JSON-Array formatiert und wie Parameter der gespeicherten Prozedur analysiert und in der Orders-Tabelle eingefügt werden kann.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie JSON in Ihre Anwendung integrieren, diesen Ressourcen:

- [TechNet-Blog](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
- [MSDN-Dokumentation](https://msdn.microsoft.com/library/dn921897.aspx)
- [Channel 9 video](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

Informationen zu Szenarien zum Integrieren von JSON in Ihrer Anwendung sehen Sie die Demos in diesem [Channel 9 video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) oder ein Szenario, das Ihrem Anwendungsfall in [JSON Blogbeiträge](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/)entspricht.
