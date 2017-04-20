## <a name="repeatability-during-copy"></a>Wiederholbarkeit beim Kopieren

Beim Kopieren von Daten auf Azure SQL-SQL Server von anderen Daten speichert muss eine Wiederholbarkeit unbeabsichtigte Ergebnisse zu bedenken. 

Beim Kopieren von Daten in Azure SQL-SQL Server-Datenbank werden kopieraktivität standardmäßig Anhängen Datensatz der Tabelle Senke standardmäßig. Beispielsweise sieht beim Kopieren von Daten aus einer CSV (durch Kommas getrennte Werte) Datei, zwei Datensätze Azure SQL-SQL Server-Datenbank enthält diese Tabelle:
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00

Angenommen Sie, Sie Fehler in Quelldatei gefunden und die Menge des unten aus 2 bis 4 in der Quelldatei aktualisiert. Wenn Sie den Datenslice Periode erneut ausführen, finden Sie zwei neue Datensätze Azure SQL-SQL Server-Datenbank angefügt. Die folgenden übernimmt die Spalten in der Tabelle keine primary Key-Einschränkung.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Um dies zu vermeiden, müssen UPSERT-Semantik angeben, indem Sie eines der unten angegebenen 2 Mechanismen.

> [AZURE.NOTE] Ein Segment kann automatisch in Azure Data Factory gemäß der angegebenen wiederholen erneut ausgeführt werden.

### <a name="mechanism-1"></a>Verfahren 1

Nutzen Sie **SqlWriterCleanupScript** Eigenschaft zuerst Cleanup Aktion Wenn ein Slice ausgeführt wird. 

    "sink":  
    { 
      "type": "SqlSink", 
      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
    }

Das Bereinigungsskript ausgeführt erste während der Kopie eines bestimmten Segments werden die Daten aus der SQL-Tabelle für dieses Segment gelöscht wird. Die Aktivität wird anschließend Daten in der SQL-Tabelle eingefügt. 

Das Segment ist nun erneut ausführen, können Sie die Menge als aktualisiert Wunsch.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Angenommen Sie, der ursprünglichen Csv Unterlegscheibe Datensatz entfernt. Das Segment erneut ausführen, erzeugt das folgende Ergebnis: 
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    7   Down Tube   4           2015-05-01 00:00:00

Keine neuen musste erfolgen. Die kopieraktivität ausgeführt Bereinigungsskript um die entsprechenden Daten für dieses Segment löschen. Die Eingabe aus der CSV-Datei gelesen wird (die dann nur 1 Datensatz enthalten) und in die Tabelle eingefügt. 

### <a name="mechanism-2"></a>Methode 2
> [AZURE.IMPORTANT] SliceIdentifierColumnName wird für Azure SQL Data Warehouse zu diesem Zeitpunkt nicht unterstützt. 

Einen anderen Mechanismus zu Wiederholbarkeit ist durch eine spezielle Spalte (**SliceIdentifierColumnName**) im Ziel-Tabelle. Diese Spalte wird von Azure Data Factory verwendet werden, um sicherzustellen, dass Quelle und Ziel synchronisiert bleiben. Dieser Ansatz funktioniert bei Flexibilität beim Ändern oder das Ziel SQL Schema definieren. 

In dieser Spalte würde Zwecken von Azure Data Factory Wiederholbarkeit und dabei Azure Data Factory wird keine Änderungen Schema der Tabelle. Dieses Verfahren verwenden können:

1.  Definieren Sie eine Spalte vom Typ binary (32) im Ziel SQL-Tabelle. Es sollte keine Einschränkungen für diese Spalte. Wir nennen Sie diese Spalte als 'ColumnForADFuseOnly' für dieses Beispiel.
2.  Verwenden Sie diese Aktivität kopieren:

        "sink":  
        { 
          "type": "SqlSink", 
          "sliceIdentifierColumnName": "ColumnForADFuseOnly"
        }

Azure Data Factory füllt diese Spalte nach seiner müssen sicherstellen, dass Quelle und Ziel synchronisiert bleiben. Die Werte dieser Spalte sollte nicht außerhalb dieser vom Benutzer verwendet werden. 

Ähnlich Verfahren 1 Kopieraktivität automatisch zuerst die Daten für das angegebene Segment aus dem Ziel-SQL-Tabelle bereinigen und führen dann kopieraktivität normalerweise, um die Daten aus Quelle Ziel für dieses Segment einzufügen. 
