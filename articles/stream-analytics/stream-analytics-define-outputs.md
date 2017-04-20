<properties
    pageTitle="Stream Analytics Ausgaben: Optionen für Speicher Analyse | Microsoft Azure"
    description="Enthält Informationen Sie zu Ausrichtungsoptionen Stream Analytics Data Ausgaben einschließlich Power BI Analyseergebnissen."
    keywords="Datentransformation Analyseergebnisse, Datenspeicheroptionen"
    services="stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage"
    documentationCenter="" 
    authors="jeffstokes72"
    manager="jhubbard" 
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Stream Analytics Ausgaben: Optionen für die Speicherung, Analyse

Beim Erstellen eines Auftrags Stream Analytics berücksichtigen Sie wie die Daten verwendet werden. Wie sehen Sie die Ergebnisse des Auftrags Stream Analytics und wo speichern Sie?

Um eine Vielzahl von Mustern Anwendung aktivieren, hat Azure Stream Analytics verschiedene Optionen zum Speichern der Ausgabe und Ergebnisse anzeigen. Dies erleichtert Auftragsausgabe anzuzeigen und Flexibilität Verbrauch und Speicher der Auftragsausgabe für Data warehousing und andere Zwecke. Ausgabe im Auftrag konfiguriert muss vorhanden sein, bevor der Auftrag gestartet und Ereignisse fließen. Beispielsweise verwenden Sie BLOB-Speicher als Ausgabe erstellt der Auftrag kein Speicherkonto automatisch. Sie muss vom Benutzer erstellt werden, bevor der ASA-Auftrag gestartet wird.

## <a name="azure-data-lake-store"></a>Azure See Datenspeicher

Stream Analytics unterstützt [Azure See Datenspeicher](https://azure.microsoft.com/services/data-lake-store/). Diese Speicher können Sie jeder Größe, Typ und Aufnahme Geschwindigkeit für betriebliche und explorative Analysen speichern. Erstellung und Konfiguration der See Datenspeicher Ausgaben wird zu diesem Zeitpunkt nur in Azure-Verwaltungsportal unterstützt. Stream Analytics muss außerdem berechtigt, See Datenspeicher zugreifen. Informationen zur Autorisierung und so Daten See Shop Vorschau (bei Bedarf) [Daten See Ausgabe Artikel](stream-analytics-data-lake-output.md)behandelt.

### <a name="authorize-an-azure-data-lake-store"></a>Autorisieren eines Datenspeichers See Azure

Wenn See Datenspeicher als Ausgabe in der Azure-Verwaltungsportal aktiviert ist, werden Sie aufgefordert, eine Verbindung zu einem vorhandenen Datenspeicher See autorisieren.  

![Autorisieren See Datenspeicher](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Füllen Sie dann die Eigenschaften für die Ausgabe See Datenspeicher wie folgt:

![Autorisieren See Datenspeicher](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

Folgende Tabelle enthält die Eigenschaftennamen und deren Beschreibung zum Erstellen einer Ausgabe See Datenspeicher benötigt.

<table>
<tbody>
<tr>
<td><B>EIGENSCHAFTENNAME</B></td>
<td><B>BESCHREIBUNG</B></td>
</tr>
<tr>
<td>Ausgabealias</td>
<td>Dies ist ein Anzeigename in Abfragen die Abfrageausgabe dieser Datenspeicher See leiten.</td>
</tr>
<tr>
<td>Kontoname</td>
<td>Der Name des Kontos See Datenspeicher, in dem Sie die Ausgabe senden. Eine Dropdownliste der See Datenspeicher Konten wird angezeigt werden, das Portal angemeldete Benutzer auf zugreifen.</td>
</tr>
<tr>
<td>Pfad-Präfix Muster [<I>optional</I>]</td>
<td>Der Pfad der Datei zum Schreiben von Dateien in der angegebenen Daten See Store-Konto verwendet. <BR>{Datum} {Time}<BR>Beispiel 1: Ordner1/Protokolle / {Datum} und {Uhrzeit}<BR>Beispiel 2: Ordner1/Protokolle / {Datum}</td>
</tr>
<tr>
<td>[<I>Optional</I>] Datumsformat</td>
<td>Wenn Datum Token im Präfix Pfad verwendet wird, können Sie das Datumsformat in dem Organisation Ihrer Dateien. Beispiel: JJJJ/MM/TT</td>
</tr>
<tr>
<td>[<I>Optional</I>] Zeitformat</td>
<td>Wenn Zeit Token im Präfix Pfad verwendet wird, geben Sie das Format in dem Organisation Ihrer Dateien an Derzeit ist der einzige unterstützte Wert HH.</td>
</tr>
<tr>
<td>Ereignis-Serialisierungsformat</td>
<td>Serialisierungsformat für Daten. JSON, CSV und Avro unterstützt.</td>
</tr>
<tr>
<td>Codierung</td>
<td>Wenn CSV oder JSON formatiert eine Codierung anzugeben. UTF-8 ist derzeit das einzige unterstützte Codierungsformat.</td>
</tr>
<tr>
<td>Trennzeichen</td>
<td>Gilt nur für CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen für die CSV-Daten serialisieren. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Registerkarte und Balken.</td>
</tr>
<tr>
<td>Format</td>
<td>Gilt nur für JSON-Serialisierung. Getrennte Zeile gibt an, dass die Ausgabe mit jeder neuen Zeile JSON-Objekt formatiert wird. Array gibt an, dass die Ausgabe als Objektarray JSON formatiert werden.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Datenspeicher See Autorisierung erneuern

Sie müssen Ihr Konto See Datenspeicher erneut authentifizieren, wenn das Kennwort geändert hat, seit Ihrem Auftrag erstellt oder zuletzt authentifiziert wurde.

![Autorisieren See Datenspeicher](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  


## <a name="sql-database"></a>SQL-Datenbank

[Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) kann als Ausgabe verwendet werden, Daten Relational Natur oder Programme, die in einer relationalen Datenbank gehostet abhängen. Stream Analytics-Aufträge werden zu einer vorhandenen Tabelle in einer Azure SQL-Datenbank geschrieben.  Beachten Sie, dass das Schema der Felder und deren Typen wird Ihr Auftrag genau übereinstimmen muss. Ein [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) können auch als Ausgabe über die SQL-Datenbank-Ausgabeoption sowie angegeben werden (Dies ist eine Vorschau). Die folgende Tabelle enthält die Namen und die Beschreibung zum Erstellen einer SQL-Datenbank-Ausgabe.

| Eigenschaftenname | Beschreibung |
|---------------|-------------|
| Ausgabealias | Dies ist ein Anzeigename die Abfrageausgabe auf diese Datenbank direkt in Abfragen verwendet. |
| Datenbank | Der Name der Datenbank, in dem Sie die Ausgabe senden |
| Servername | Der SQL Server-Datenbankname |
| Benutzername | Der Benutzername hat Schreibzugriff auf die Datenbank |
| Kennwort | Das Kennwort für die Datenbank |
| Tabelle | Der Tabellenname in die Ausgabe geschrieben wird. Der Tabellenname wird zwischen Groß- und das Schema dieser Tabelle sollte genau auf die Anzahl der Felder und deren Typen durch die Auftragsausgabe übereinstimmen. |

> [AZURE.NOTE] Angebot Azure SQL-Datenbank wird momentan Auftragsausgabe in Stream Analytics unterstützt. Ein Azure Virtual Machine unter SQL Server mit einer Datenbank verbunden ist, nicht unterstützt. Dies ist in zukünftigen Versionen geändert.

## <a name="blob-storage"></a>BLOB-Speicher

BLOB-Speicher bietet eine kostengünstige und skalierbare Lösung für große Mengen an unstrukturierten Daten in der Cloud speichern.  Einführung in Azure BLOB-Speicher und ihrer Verwendung finden Sie in der Dokumentation von [Blobs verwenden](../storage/storage-dotnet-how-to-use-blobs.md).

Folgende Tabelle enthält die Eigenschaftennamen und die Beschreibung zum Erstellen einer BLOB-Ausgabe.

<table>
<tbody>
<tr>
<td>EIGENSCHAFTENNAME</td>
<td>BESCHREIBUNG</td>
</tr>
<tr>
<td>Ausgabealias</td>
<td>Dies ist ein Anzeigename in Abfragen verwendet, um die Abfrage Ausgabe dieses Blob-Speicher.</td>
</tr>
<tr>
<td>Konto</td>
<td>Der Name des Speicherkontos, in dem Sie die Ausgabe senden.</td>
</tr>
<tr>
<td>Speicherkontoschlüssel</td>
<td>Der geheime Schlüssel Storage-Konto zugeordnet.</td>
</tr>
<tr>
<td>Behälter</td>
<td>Container stellen eine logische Gruppierung für Blobs in Microsoft Azure Blob-Dienst gespeichert. Beim Hochladen eines BLOBs auf BLOB-Dienst müssen Sie einen Container für dieses Blob angeben.</td>
</tr>
<tr>
<td>Pfad-Präfix Muster [optional]</td>
<td>Der Dateipfad der Blobs im angegebenen Container geschrieben.<BR>In diesem Pfad können Sie eine oder mehrere Instanzen der folgenden 2 Variablen verwenden, um die Häufigkeit anzugeben, die Blobs geschrieben werden:<BR>{Datum} {Time}<BR>Beispiel 1: cluster1/Protokolle / {Datum} und {Uhrzeit}<BR>Beispiel 2: cluster1/Protokolle / {Datum}</td>
</tr>
<tr>
<td>[Optional] Datumsformat</td>
<td>Wenn Datum Token im Präfix Pfad verwendet wird, können Sie das Datumsformat in dem Organisation Ihrer Dateien. Beispiel: JJJJ/MM/TT</td>
</tr>
<tr>
<td>[Optional] Zeitformat</td>
<td>Wenn Zeit Token im Präfix Pfad verwendet wird, geben Sie das Format in dem Organisation Ihrer Dateien an Derzeit ist der einzige unterstützte Wert HH.</td>
</tr>
<tr>
<td>Ereignis-Serialisierungsformat</td>
<td>Serialisierungsformat für Daten.  JSON, CSV und Avro unterstützt.</td>
</tr>
<tr>
<td>Codierung</td>
<td>Wenn CSV oder JSON formatiert eine Codierung anzugeben. UTF-8 ist derzeit das einzige unterstützte Codierungsformat.</td>
</tr>
<tr>
<td>Trennzeichen</td>
<td>Gilt nur für CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen für die CSV-Daten serialisieren. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Registerkarte und Balken.</td>
</tr>
<tr>
<td>Format</td>
<td>Gilt nur für JSON-Serialisierung. Getrennte Zeile gibt an, dass die Ausgabe mit jeder neuen Zeile JSON-Objekt formatiert wird. Array gibt an, dass die Ausgabe als Objektarray JSON formatiert werden.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Ereignis-Hub

[Ereignis-Hubs](https://azure.microsoft.com/services/event-hubs/) ist eine hochgradig skalierbare Veröffentlichen-Abonnieren Ereignis Ingestor. Sie können Millionen Ereignisse pro Sekunde erfassen.  Eine Event-Hubs als Ausgabe ist die Ausgabe eines Auftrags Stream Analytics die Eingabe von einem anderen Stream Auftrag werden.

Es gibt einige Parameter sind Event Hub-Datenströmen als Ausgabe konfigurieren.

| Eigenschaftenname | Beschreibung |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ausgabealias | Dies ist ein Anzeigename in Abfragen verwendet, um die Abfrage Ausgabe dieses Ereignis-Hub. |
| Service Bus-Namespace | Service Bus-Namespace ist ein Container für eine Gruppe von Entitäten messaging. Beim Erstellen neuer Ereignis-Hub erstellt Sie außerdem einen Service Bus-namespace |
| Ereignis-Hub | Der Name der Ihr Event Hub |
| Ereignisname Hub-Richtlinie | Die SAS-Richtlinie auf der Registerkarte Ereignis Hub konfigurieren. Jede freigegebene Richtlinie einen Namen, Berechtigungen festlegen und Zugriffstasten |
| Ereignis-Hub Richtlinienschlüssel | Freigegebene Zugriffstaste Zugriffsberechtigung Servicebus-Namespace |
| Partition Key Column [optional] | Diese Spalte enthält den Partition für Event Hub-Ausgabe. |
| Ereignis-Serialisierungsformat | Serialisierungsformat für Daten.  JSON, CSV und Avro unterstützt. |
| Codierung | Für CSV- und JSON ist UTF-8 nur unterstütztes Codierungsformat zurzeit |
| Trennzeichen | Gilt nur für CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen für Serialisieren der Daten im CSV-Format. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Registerkarte und Balken. |
| Format | Gilt nur für JSON-Typ. Getrennte Zeile gibt an, dass die Ausgabe mit jeder neuen Zeile JSON-Objekt formatiert wird. Array gibt an, dass die Ausgabe als Objektarray JSON formatiert werden. |

## <a name="power-bi"></a>Power BI

[Power BI](https://powerbi.microsoft.com/) kann eine umfassende Visualisierung Erfahrung der Analyseergebnisse zu als Ausgabe für einen Stream Analytics-Auftrag verwendet werden. Diese Funktion kann für betriebliche Dashboards, Berichten und datengesteuerte reporting Metrik verwendet.

### <a name="authorize-a-power-bi-account"></a>Autorisieren von Power BI-Konto

1.  Wenn Power BI als Ausgabe in der Azure-Verwaltungsportal aktiviert ist, werden Sie eine vorhandene Hauptbenutzer BI autorisieren oder Erstellen eines neuen Kontos Power BI aufgefordert.  

    ![BI Benutzer autorisieren](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  

2.  Erstellen Sie ein neues Konto, wenn nicht noch haben, dann klicken Sie auf jetzt autorisieren.  Ein Bildschirm ähnlich dem folgenden angezeigt.  

    ![Ein Azure-Konto Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  

3.  Geben Sie in diesem Schritt die Arbeit oder Schule für die Autorisierung der Power BI-Ausgabe. Wenn Sie nicht bereits für Power BI angemeldet werden, wählen Sie Zeichen jetzt. Das Arbeits- oder Schulcomputer verwendete Konto für Power BI könnte von Azure Abonnement Sie derzeit angemeldet sind.

### <a name="configure-the-power-bi-output-properties"></a>Konfigurieren Sie die Ausgabeeigenschaften Power BI

Haben Sie das Power BI-Konto authentifiziert, können Sie die Eigenschaften für die Power BI-Ausgabe konfigurieren. In der folgenden Tabelle wird die Liste der Eigenschaftennamen und deren Beschreibung die Power BI-Ausgabe konfigurieren.

| Eigenschaftenname | Beschreibung |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ausgabealias | Dies ist ein Anzeigename in Abfragen die Abfrageausgabe PowerBI zu leiten. |
| Gruppe-Arbeitsbereich | Zum Freigeben von Daten mit anderen Benutzern Power BI können Sie Gruppen in Ihrem Power BI-Konto auswählen oder wählen Sie "Arbeitsbereich", wenn Sie nicht in einer Gruppe zu schreiben.  Aktualisieren einer vorhandenen Gruppe erfordert erneuern Power BI-Authentifizierung. | 
| DataSet-Name | Geben Sie einen Datasetnamen, die gewünscht für Power BI-Ausgabe verwenden |
| Tabellenname | Geben Sie einen Tabellennamen unter Dataset Power BI-Ausgabe. Derzeit können Power BI-Ausgabe von Stream Analytics stellen nur eine Tabelle in einem dataset |

Konfigurieren eines Power BI-Ausgabe und Dashboard Blättern finden Sie im Artikel [Azure Stream Analytics & Power BI](stream-analytics-power-bi-dashboard.md) .

> [AZURE.NOTE] Nicht erstellen Sie explizit Dataset und Tabelle im Power BI-Dashboard. Das Dataset und die Tabelle werden automatisch ausgefüllt werden, wenn der Einzelvorgang gestartet und der Einzelvorgang Pumpen Ausgabe in Power BI. Hinweis: Wenn die auftragsabfrage Ergebnisse generiert, das Dataset und die Tabelle nicht erstellt werden. Beachten Sie auch, wenn Power BI bereits ein Dataset und mit demselben Namen wie die gemäß diesen Stream Analytics-Auftrag, die vorhandenen Daten überschrieben.

### <a name="renew-power-bi-authorization"></a>Power BI Autorisierung erneuern

Sie müssen Ihre Power BI-Konto erneut authentifizieren, wenn das Kennwort geändert hat, seit Ihrem Auftrag erstellt oder zuletzt authentifiziert wurde. Wenn mehrstufige Authentifizierung (MFA) auch Power BI Autorisierung alle 2 Wochen erneuern müssen auf Ihrem Mandanten Azure Active Directory (AAD) konfiguriert ist. Ein Symptom dieses Problems ist keine Auftragsausgabe und authentifizieren Benutzerfehler"" in der Protokolle:

  ![Power BI aktualisieren token-Fehler](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Um dieses Problem zu beheben, beenden Sie ausgeführte Auftrag und zur Power BI-Ausgabe.  Klicken Sie auf "Erneuern Autorisierung" und starten Sie Ihr Projekt von der letzten Ausführung beendet um Datenverluste zu vermeiden.

  ![Power BI erneuern Autorisierung](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Tabelle speichern

[Azure-Tabellenspeicher](../storage/storage-introduction.md) bietet hochverfügbaren, massiv skalierbaren Speicher, so dass eine Anwendung automatisch Benutzer Bedarf skalieren kann. Tabellenspeicher ist Microsofts NoSQL Key-Attribut Speicher welche mit weniger Einschränkungen auf dem Schema für strukturierte Daten nutzen kann. Azure Table Storage kann zum Speichern von Daten für Persistenz und effizienten Abruf verwendet werden.

Die folgende Tabelle enthält die Namen und die Beschreibung zum Erstellen einer Tabellenausgabe.

| Eigenschaftenname | Beschreibung |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ausgabealias | Dies ist ein Anzeigename in Abfragen die Abfrageausgabe auf diese Tabellenspeicher angewiesen. |
| Konto | Der Name des Speicherkontos, in dem Sie die Ausgabe senden. |
| Speicherkontoschlüssel | Die Zugriffstaste Storage-Konto zugeordnet. |
| Tabellenname | Der Name der Tabelle. Die Tabelle wird erstellt, wenn er nicht vorhanden ist. |
| Partitionsschlüssel | Der Name der Ausgabespalte mit der Partitionsschlüssel. Der Partitionsschlüssel ist ein eindeutiger Bezeichner für die Partition innerhalb einer Tabelle, die der erste des Primärschlüssels einer Entität Teil. Es ist eine Zeichenfolge, die bis zu 1 KB. |
| Zeilenschlüssel. | Der Name der Ausgabespalte mit der Zeilenschlüssel. Der Zeilenschlüssel ist ein eindeutiger Bezeichner für eine Entität in einer Partition. Es ist der zweite Teil des Primärschlüssels einer Entität. Der Zeilenschlüssel ist ein Zeichenfolgenwert, der bis zu 1 KB sein kann. |
| Batchgröße | Die Anzahl der Datensätze für Batch-Betrieb. In der Regel Standard reicht für die meisten Projekte finden Sie in [Tabelle Batchvorgang Spezifikation](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) für Weitere Informationen zum Ändern dieser Einstellung. |

## <a name="service-bus-queues"></a>Service Bus-Warteschlangen

[Service Bus Warteschlangen](https://msdn.microsoft.com/library/azure/hh367516.aspx) bieten einen ersten, Nachrichtenübermittlung an einen oder mehrere konkurrierende Consumer erste Out (FIFO). Normalerweise sollten Nachrichten empfangen und vom Empfänger in der zeitlichen Reihenfolge sie der Warteschlange hinzugefügt wurden, und jede Nachricht empfangen und verarbeitet nur eine Nachricht Verbraucher verarbeitet werden.

Die folgende Tabelle enthält die Namen und die Beschreibung zum Erstellen einer Warteschlange Ausgabe.

| Eigenschaftenname | Beschreibung |
|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ausgabealias | Dies ist ein Anzeigename in Abfragen verwendet, um die Abfrage Ausgabe dieses Service Bus-Warteschlange. |
| Service Bus-Namespace | Service Bus-Namespace ist ein Container für eine Gruppe von Entitäten messaging. |
| Name der Warteschlange | Der Name des Service Bus-Warteschlange. |
| Richtlinienname Warteschlange | Beim Erstellen einer Warteschlange können Sie auch auf der Registerkarte Warteschlange Konfigurieren gemeinsamer Zugriffsrichtlinien erstellen. Jede freigegebene Richtlinie einen Namen, Berechtigungen festlegen und Zugriffstasten. |
| Warteschlange Richtlinienschlüssel | Freigegebene Zugriffstaste Zugriffsberechtigung Servicebus-Namespace |
| Ereignis-Serialisierungsformat | Serialisierungsformat für Daten.  JSON, CSV und Avro unterstützt. |
| Codierung | Für CSV- und JSON ist UTF-8 nur unterstütztes Codierungsformat zurzeit |
| Trennzeichen | Gilt nur für CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen für Serialisieren der Daten im CSV-Format. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Registerkarte und Balken. |
| Format | Gilt nur für JSON-Typ. Getrennte Zeile gibt an, dass die Ausgabe mit jeder neuen Zeile JSON-Objekt formatiert wird. Array gibt an, dass die Ausgabe als Objektarray JSON formatiert werden. |

## <a name="service-bus-topics"></a>Service Bus Topics

Service Bus Warteschlangen eine Methode eins-Absenders Empfänger bereit, Themen [Service Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) eine n-Form der Kommunikation.

Die folgende Tabelle enthält die Namen und die Beschreibung zum Erstellen einer Tabellenausgabe.

| Eigenschaftenname | Beschreibung |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ausgabealias | Dies ist ein Anzeigename in Abfragen verwendet, um die Abfrage Ausgabe dieses Service Bus-Thema. |
| Service Bus-Namespace | Service Bus-Namespace ist ein Container für eine Gruppe von Entitäten messaging. Beim Erstellen neuer Ereignis-Hub erstellt Sie außerdem einen Service Bus-namespace |
| Thema | Themen sind Entitäten, ähnlich wie Ereignis-Hubs und Warteschlangen messaging. Sie sind zum Sammeln von Ereignisstreams aus einer Reihe von anderen Geräten und Diensten entwickelt. Beim Erstellen eines Themas wird auch einen bestimmten Namen gegeben. Ein Thema gesendeten Nachrichten nicht zur Verfügung, wenn ein Abonnement erstellt wird, so werden ein oder mehrere Abonnements unter dem Thema |
| Richtlinienname Thema | Beim Erstellen eines Themas können Sie freigegebene Richtlinien auf der Registerkarte Konfigurieren Thema erstellen. Jede freigegebene Richtlinie einen Namen, Berechtigungen festlegen und Zugriffstasten |
| Thema Richtlinienschlüssel | Freigegebene Zugriffstaste Zugriffsberechtigung Servicebus-Namespace |
| Ereignis-Serialisierungsformat | Serialisierungsformat für Daten.  JSON, CSV und Avro unterstützt. |
| Codierung | Wenn CSV oder JSON formatiert eine Codierung anzugeben. UTF-8 ist derzeit das einzige unterstützte Codierungsformat |
| Trennzeichen | Gilt nur für CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen für Serialisieren der Daten im CSV-Format. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Registerkarte und Balken. |

## <a name="documentdb"></a>DocumentDB

[Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) ist eine vollständig verwaltete NoSQL Dokument Datenbankdienst, der Abfrage und Transaktionen über Schema-Daten, voraussehbare und zuverlässige Performance und schnelle Entwicklung bietet.

Folgende Tabelle enthält die Eigenschaftennamen und deren Beschreibung zum Erstellen einer DocumentDB-Ausgabe.

<table>
<tbody>
<tr>
<td>EIGENSCHAFTENNAME</td>
<td>BESCHREIBUNG</td>
</tr>
<tr>
<td>Kontoname</td>
<td>Der Name des DocumentDB-Kontos.  Dies ist auch der Endpunkt für das Konto.</td>
</tr>
<tr>
<td>Kennwort</td>
<td>Die freigegebenen Zugriffstaste für das Konto DocumentDB.</td>
</tr>
<tr>
<td>Datenbank</td>
<td>Der Name der DocumentDB.</td>
</tr>
<tr>
<td>Auflistung Muster</td>
<td>Die Auflistung Muster für Sammlungen verwendet werden. Das Format der Auflistung kann mithilfe des Tokens optionale {Partition} Partitionen von 0 beginnt erstellt werden.<BR>Z.b. Der gültige Eingaben sind:<BR>MyCollection {Partition}<BR>MyCollection<BR>Beachten Sie, dass Sammlungen vorhanden, bevor der Stream Analytics-Auftrag sein müssen wird gestartet und nicht automatisch erstellt.</td>
</tr>
<tr>
<td>Partitionsschlüssel</td>
<td>Der Name des Felds im Ausgabeereignisse verwendet, um den Schlüssel für die Partitionierung Ausgabe über Sammlungen.</td>
</tr>
<tr>
<td>Dokument-ID</td>
<td>Der Name des Feldes zur Angabe des Primärschlüssels einfügen oder Ausgabeereignisse Aktualisierungsvorgänge basieren.</td>
</tr>
</tbody>
</table>


## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
Sie haben einen verwalteten Service für streaming-Daten über das Internet der Dinge Analytics Stream Analytics eingeführt. Weitere Informationen zu diesem Dienst finden Sie unter:

- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
