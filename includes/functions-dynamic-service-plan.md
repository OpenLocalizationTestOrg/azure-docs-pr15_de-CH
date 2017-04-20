## <a name="dynamic-service-plan"></a>Dynamische Service-Plan

In der dynamischen Service-Plan werden Ihre Funktion apps eine Funktion app-Instanz zugewiesen werden. Wenn mehrere Instanzen Bedarf dynamisch hinzugefügt werden.
Die Instanzen können über mehrere Computerressourcen, die aus Azure Infrastruktur erstrecken. Darüber hinaus führt Funktionen parallel Minimierung der Gesamtdauer Anfragen verarbeiten. Ausführungszeit für jede Funktion, in Sekunden hinzugefügt und der enthaltenden Funktion Anwendung zusammengefasst. Kosten aufgrund der Anzahl von Instanzen, Speichergröße und gesamt-Ausführungszeit gemessen in Gigabyte Sekunden. Dies ist eine hervorragende Option Compute Bedarf zeitweise oder Ihre Arbeit Zeiten sehr kurz, da können Sie Serverressourcen nur bezahlen, wenn sie tatsächlich verwendet werden.   

### <a name="memory-tier"></a>Speicher-tier

Je nach Ihrer Funktion muss in der App-Funktion (Container Funktionen) ausführen erforderliche Speichermenge auswählen können.
Speicheroptionen variieren von **128 MB, 1536 MB**. Die ausgewählten Speichergröße entspricht Workingset benötigt von den Funktionen, die Teil der Funktion app. Wenn der Code mehr Arbeitsspeicher als die gewählte Größe erfordert, wird Funktion app-Instanz aufgrund mangelnden Arbeitsspeichers heruntergefahren.

### <a name="scaling"></a>Skalierung

Funktionen der Azure-Plattform wird der Datenverkehr Bedürfnisse konfigurierten Trigger, wann oder unten ausgewertet. Die Funktion ist der Granularität der Skalierung. Zentrales Skalieren bedeutet in diesem Fall weitere Instanzen einer Funktion app. Umgekehrt Datenverkehr geht, sind Funktion app-Instanzen deaktiviert schließlich Herunterskalieren 0 Wenn keine ausgeführt werden.  

### <a name="resource-consumption-and-billing"></a>Ressourcenverbrauch und Abrechnung

Im dynamischen Modus Zuweisung erfolgt anders als standard App Service-Plan, Verbrauch Modell ist daher auch für "Pay-per-Use" Modell unterschiedlich. Verbrauch wird pro Funktion app nur gemeldet, wenn Code ausgeführt wird.  
Es berechnet die Speichergröße (in GB) durch den Gesamtbetrag der Ausführungszeit (in Sekunden) für alle Funktionen in Funktion app. Der Verbrauch wird **GB-s (Gigabyte Sekunden)**sein.