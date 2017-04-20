Ressource|Standardlimit|Maximale Anzahl
---|---|---
Azure Media Services (AMS) Konten in ein einzelnes Abonnement||25
Ressourcen pro AMS-Konto||1.000.000<sup>1</sup>
Verkettete Aufgaben pro Auftrag||30
Ressourcen pro Vorgang||50
Ressourcen pro Projekt||100
Aufträge pro AMS-Konto ||50.000<sup>2</sup>
Eindeutiger Zusammenhang mit einer Anlage gleichzeitig locators||5<sup>4</sup>
Live Kanäle pro AMS-Konto </p></td>|5</p></td>|N-<sup>1</sup>
Programme im Zustand pro Kanal </p></td>|50</p></td>|N-<sup>1</sup>
Programme im Ausführungszustand befindet pro Kanal </p></td>|3</p></td>|3
Streaming-Endpunkte im Ausführungszustand befindet pro AMS-Konto</p></td>|2</p></td>|N-<sup>1</sup>
Streaming-Einheiten pro Streaming-Endpunkt </p></td>|10 </p></td>|N-<sup>1</sup>
Codierung Einheiten pro AMS-Konto </p></td>|25</p></td>|N-<sup>1</sup>
Speicherkonten | |1.000<sup>5</sup>
Richtlinien || 1.000.000<sup>6</sup>

<sup>1</sup> möglich, um Grenzwerte für dieses Kontingent Öffnen einer Support-Ticket zu aktualisieren. Weitere AMS-Konten erhöhen, stattdessen ein Support Ticket nicht erstellt werden.

<sup>2</sup> diese Zahl schließt in der Warteschlange, Fertig, aktiven und stornierte Aufträge. Es enthält keine gelöschten Aufträge. Sie können die alten Aufträge mit **IJob.Delete** oder der HTTP-Anforderung **zu löschen** .

<sup>3</sup> bei einer Anforderung Liste Auftrag Entitäten werden maximal 1.000 pro Anforderung zurückgegeben. Benötigen alle gesendeten Aufträge an können Sie Top-überspringen, wie [OData](http://msdn.microsoft.com/library/gg309461.aspx)-Abfrage Optionen beschrieben.

<sup>4</sup> Locators dienen nicht zum Verwalten von benutzerspezifischen Zugriffskontrolle. Geben Sie unterschiedliche Zugriffsrechte für einzelne Benutzer verwenden Sie Solutions Digital Rights Management (DRM).

<sup>5</sup> Speicherkonten muss aus der gleichen Azure-Abonnement.

<sup>6</sup> gibt es ein Limit von 1.000.000 Richtlinien für verschiedene AMS-Richtlinien (z.B. Locator Richtlinie oder ContentKeyAuthorizationPolicy). Verwenden die gleichen Richtlinien-ID verwenden immer die gleichen Tage / Berechtigungen / etc.
