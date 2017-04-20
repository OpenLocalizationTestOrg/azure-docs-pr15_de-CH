## <a name="repeatability-during-copy"></a>Wiederholbarkeit beim Kopieren

Beim Kopieren von Daten von und zu relationalen Speicher müssen Sie Wiederholbarkeit unbeabsichtigte Ergebnisse zu bedenken. 

Ein Segment kann automatisch in Azure Data Factory gemäß der angegebenen wiederholen erneut ausgeführt werden. Wir empfehlen eine wiederholungsrichtlinie Schutz vor Ausfällen festlegen. Wiederholbarkeit ist daher ein wichtiger Aspekt beim Datentransfer kümmern. 

**Als Quelle:**

> [AZURE.NOTE] Die folgenden Beispiele sind für SQL Azure jedoch für beliebige Datenspeicher, die rechteckige Datasets unterstützt. Sie müssen der **Typ** der Quell- und **der Abfrageeigenschaft** anpassen (z. B.: Abfrage statt SqlReaderQuery) Daten speichern.   

In der Regel bei relationalen speichern möchten Sie nur die Daten für dieses Segment. Eine Möglichkeit wäre mit den WindowStart und WindowEnd-Variablen in Azure Data Factory. Informationen Sie zu Variablen und Funktionen in Azure Data Factory hier im Artikel der [Planung und Ausführung](../articles/data-factory/data-factory-scheduling-and-execution.md) . Beispiel: 
    
      "source": {
        "type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
      },

Diese Abfrage liest Daten aus "MyTable", die im Bereich Dauer Segment liegt. Wiederholung dieses Segment wird dies immer sichergestellt. 

In anderen Fällen können die gesamte Tabelle lesen (angenommen für nur einmal verschieben) und die SqlReaderQuery kann wie folgt definieren:

    
    "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
              },
    
