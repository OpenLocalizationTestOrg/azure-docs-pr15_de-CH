<properties
   pageTitle="Filtern von Zeichenfolgen für den Tabellen-Designer erstellen | Microsoft Azure"
   description="Erstellen von Filterzeichenfolgen für den Tabellen-designer"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="constructing-filter-strings-for-the-table-designer"></a>Erstellen von Filterzeichenfolgen für den Tabellen-Designer

## <a name="overview"></a>Übersicht

Zum Filtern von Daten in Azure-Tabelle, die im Visual Studio- **Tabellen-Designer**angezeigt wird, eine Filterzeichenfolge erstellen und geben Sie in das Feld "Filter". Syntax der Filter festgelegten WCF-Datendienste und ist ähnlich einer SQL WHERE-Klausel jedoch Tabelle Service über eine HTTP-Anforderung gesendet wird. Der **Tabellen-Designer** behandelt die richtige Codierung für Sie, um eine gewünschte Eigenschaftenwert filtern, müssen Sie nur die Eigenschaftennamen Vergleichsoperator, Wert und gegebenenfalls Boolean eingeben im Feld Operator. Sie müssen nicht die Abfrageoption $filter enthalten, wie eine URL zum Abfragen der Tabelle über [Storage Services REST-API-Referenz](http://go.microsoft.com/fwlink/p/?LinkId=400447)erstellt wurden.

WCF-Datendienste basieren auf [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Ausführliche Informationen über die Option Filter System Abfrage (**$filter**) finden Sie unter [OData-URI-Konventionen Spezifikation](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Vergleichsoperatoren

Die folgenden logischen Operatoren werden für alle Typen unterstützt:

|Logischer operator|Beschreibung|Beispiel-Filterzeichenfolge|
|---|---|---|
|EQ|Gleich|City Eq "Redmond"|
|gt|Größer als|Preis Gt 20|
|ge|Größer als oder gleich|Preis ge 10|
|lt|Kleiner als|Preis Lt 20|
|LE|Kleiner als oder gleich|Preis le 100|
|ne|Ungleich|Stadt Ne "London"|
|und|Und|Preis le 200 und Preis Gt 3.5|
|oder|Oder|Preis le 3.5 oder Preis Gt 200|
|nicht|Nicht|nicht isAvailable|

Beim Erstellen einer Filterzeichenfolge zählen die folgenden Regeln:

- Verwenden Sie die logischen Operatoren, eine Eigenschaft mit einem Wert vergleichen. Beachten Sie, dass es nicht möglich, eine Eigenschaft, einen dynamischen Wert vergleichen. eine Seite des Ausdrucks muss eine Konstante sein.

- Alle Teile der Filterzeichenfolge Groß-/Kleinschreibung.

- Der Konstante Wert muss den gleichen Datentyp wie die Reihenfolge der Filter-Eigenschaft gültige Ergebnisse zurückgeben. Weitere Informationen zu unterstützten Typen finden Sie unter [Understanding Tabelle Service-Datenmodell](http://go.microsoft.com/fwlink/p/?LinkId=400448).

## <a name="filtering-on-string-properties"></a>Filtern von Eigenschaften

Beim Filtern auf Zeichenfolgeneigenschaften setzen Sie die Zeichenfolgenkonstante in Anführungszeichen.

Die folgenden Beispiel Filter auf die **PartitionKey** und **RowKey** ; Weitere nicht-Schlüsseleigenschaften konnte die Filterzeichenfolge ebenfalls hinzugefügt:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

Sie können jedes Filter-Ausdruck in Klammern setzen, obwohl dies nicht erforderlich ist:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Beachten Sie, dass der Dienst keine Platzhalterabfragen, und sie nicht im Tabellen-Designer entweder unterstützt werden. Allerdings können Sie mithilfe von Vergleichsoperatoren auf dem gewünschten Präfix mit Präfix durchführen. Das folgende Beispiel gibt Entitäten LastName-Eigenschaft beginnt mit dem Buchstaben "A":

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Numerische Eigenschaften filtern

Um eine Ganzzahl oder eine Gleitkommazahl filtern, geben Sie ohne Anführungszeichen.

Dieses Beispiel gibt alle Entitäten mit einer ALTER-Eigenschaft, deren Wert größer als 30 ist:

    Age gt 30

Dieses Beispiel gibt alle Entitäten mit einer AmountDue Eigenschaft, deren Wert kleiner oder gleich 100.25:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Boolesche Eigenschaften filtern

Um einen booleschen Wert filtern, geben Sie **true** oder **false** ohne Anführungszeichen.

Das folgende Beispiel gibt alle Entitäten, IsActive-Eigenschaft auf **true**festgelegt ist:

    IsActive eq true

Sie können auch diese Filterausdruck ohne logischen Operator schreiben. Im folgenden Beispiel wird der Dienst auch alle Entitäten zurückgegeben IsActive **true**:

    IsActive

Um alle Entitäten zurückzugeben IsActive false ist, können Sie nicht Operator:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Filtern nach DateTime-Eigenschaften

Um einen DateTime-Wert filtern, geben Sie **Datetime** -Schlüsselwort, gefolgt von Zeitpunkt Konstante in Anführungszeichen an Die Konstante muss kombinierten UTC-Format unter [Formatierung DateTime-Eigenschaftswerte](http://go.microsoft.com/fwlink/p/?LinkId=400449).

Das folgende Beispiel gibt Entitäten ist die Eingabeformat-Eigenschaft gleich 10. Juli 2008:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
