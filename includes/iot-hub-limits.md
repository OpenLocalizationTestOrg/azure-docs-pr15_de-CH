Die folgende Tabelle enthält die Grenzen der verschiedenen Dienstebenen (S1, S2, S3, F1). Informationen über die Kosten jeder *Einheit* in einer Ebene finden Sie unter [IoT Hub Preise](https://azure.microsoft.com/pricing/details/iot-hub/).

| Ressource | S1 Standard | S2-Standard | S3-Standard | F1 kostenlose |
| -------- | ----------- | ----------- | ----------- | ------- |
| Nachrichten pro Tag | 400.000 | 6.000.000   | 300.000.000 | 8.000   |
| Maximale Einheiten | 200    | 200         | 200         | 1       |

> [AZURE.NOTE] Wenn Sie einen Hub S1 oder S2 oder S3-Tier mit 200 Einheiten erwarten, wenden Sie sich an Microsoft Support.

Die folgende Tabelle listet die Grenzwerte IoT Hub Ressourcen zuweisen:

| Ressource | Grenzwert |
| -------- | ----- |
| Maximale IoT Hubs pro Azure-Abonnement bezahlt | 10 |
| Maximale kostenlose IoT Hubs pro Azure-Abonnement | 1 |
| Maximale Anzahl der Geräte-Identitäten<br/>  in einem einzelnen Aufruf zurückgegeben | 1000 |
| IoT Hub Nachricht maximale Beibehaltungsdauer für Gerät Cloud-Nachrichten | 7 Tage |
| Maximale Größe der Gerät Cloud-Nachrichten | 256 KB |
| Maximale Größe des Geräts Cloud batch | 256 KB |
| Maximale Nachrichten im Gerät Cloud batch | 500 |
| Maximale Größe der Cloud-zu-Gerät-Nachricht | 64 KB |
| Maximale Gültigkeitsdauer für Cloud-zu-Gerät-Nachrichten | 2 Tage |
| Lieferung maximale Anzahl für Cloud-Gerät <br/> Nachrichten | 100 |
| Maximale Lieferung Count für Nachrichten feedback <br/> Cloud-zu-Gerät-Meldung | 100 |
| Maximale Gültigkeitsdauer für Nachrichten feedback <br/> Antwort auf eine Cloud-Gerät | 2 Tage |

> [AZURE.NOTE] Benötigen Sie mehr als 10 bezahlte IoT Hubs in Azure-Abonnement, wenden Sie sich an Microsoft Support Services.

IoT Hub Service Steuerung Anfragen Überschreitung die folgenden Vorgaben:

| Drosselklappe | Wert pro hub |
| -------- | ------------- |
| Identität Registrierungsvorgängen <br/> (erstellen, abrufen, auflisten, aktualisieren, löschen) <br/> einzelne oder Bulk Import/export | 5000/min/Einheit (für S3) <br/> 100/min/Einheit (für S1 und S2). |
| Verbindung | 6000/Sek. / (für S3) 120/Sek./Einheit (für S2) 12/Sek./Einheit (für S1). <br/>Mindestens 100/Sekunde. |
| Gerät Cloud sendet | 6000/Sek. / (für S3) 120/Sek./Einheit (für S2) 12/Sek./Einheit (für S1). <br/>Mindestens 100/Sekunde. |
| Cloud-Gerät sendet | 5000/min / (für S3) 100/min/Einheit (für S1 und S2). |
| Cloud-Gerät empfangen | 50000/min / (für S3) 1000/min/Einheit (für S1 und S2). |
| Datei-Upload-Vorgänge | 5000 Dateiupload Benachrichtigungen/min/Einheit (für S3), 100 Datei hochladen Benachrichtigungen/min/Einheit (für S1 und S2). <br/> 10000 SAS-URIs möglich, ein Azure Storage-Konto gleichzeitig.<br/> 10 SAS-URIs-Gerät kann gleichzeitig aus. |
