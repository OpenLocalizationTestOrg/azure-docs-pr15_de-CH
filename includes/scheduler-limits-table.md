Die folgende Tabelle beschreibt wichtige Kontingente, Grenzen, Standardwerte und Drosselungen in Azure Scheduler.

|Ressource|Limit-Beschreibung|
|---|---|
|**Größe**|Die maximale Größe beträgt 16 KB. Ergibt eine oder einen PATCH ein Auftrag diese Grenzwerte überschreitet, ist 400 Bad Request Status-Code zurückgegeben.|
|**Anfordern von URL-Größe**|Maximale Größe des angeforderten URLs ist 2048 Zeichen.|
|**Aggregierte Headergröße**|Maximale ist aggregate 4096 Zeichen.|
|**Anzahl**|Maximale Anzahl ist 50 Header.|
|**Textgröße**|Maximale Größe beträgt 8192 Zeichen.|
|**Serie Spanne**|Maximale Serie ist 18 Monate.|
|**Mal starten**|Maximale "Zeit gestartet" ist 18 Monate.|
|**Historie**|Maximale Antworttext Auftragsverlauf gespeichert ist 2048 Bytes.|
|**Häufigkeit**|Das Standardkontingent für die maximale Frequenz ist in einer freien Stelle Auflistung 1 Stunde und 1 Minute in einer standard-Job-Auflistung. Die maximale Häufigkeit lässt sich auf einen Auftrag niedriger als der Maximalwert. Alle Aufträge in der Job-Auflistung sind beschränkt auf den Job festgelegten Wert. Beim Erstellen eines Auftrags mit einer höheren Frequenz als die maximale Frequenz auf der Auftrag wird mit Statuscode 409 Konflikt Anforderung fehl.|
|**Aufträge**|Das Standardkontingent max Aufträge ist 5 Arbeitsplätze in einer freien Stelle Auflistung und 50 in einer standard-Job-Auflistung. Die maximale Anzahl von Einzelvorgängen lässt sich auf einen Auftrag. Alle Aufträge in der Job-Auflistung sind beschränkt auf den Job festgelegten Wert. Wenn Sie versuchen, mehr als das Kontingent Einzelvorgängen Arbeitsplätze, schlägt die Anforderung mit einem 409 Konflikt Statuscode.|
|**Auftrag Verlauf beibehalten**|Auftragsverlauf bleibt bis zu 2 Monate lang oder bis die letzten 1000 Einheiten.|
|**Abgeschlossene und fehlerhaften Auftrag Aufbewahrung**|Abgeschlossene und fehlerhafte Aufträge werden für 60 Tage beibehalten.|
|**Zeitlimit**|Es ist eine statische (nicht konfiguriert) Anforderungstimeout von 60 Sekunden für HTTP-Aktionen. Länger andauernde Vorgänge führen Sie asynchrone HTTP-Protokolle; beispielsweise eine 202 sofort, aber weiterhin im Hintergrund.|
