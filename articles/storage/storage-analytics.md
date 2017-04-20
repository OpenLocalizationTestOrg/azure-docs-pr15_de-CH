<properties
    pageTitle="Mit der Datensammlung Protokolle und Metriken Speicheranalyse | Microsoft Azure"
    description="Speicheranalyse können Sie Metrikdaten für Speicher-Services verfolgen und Protokolle für BLOBs, Warteschlangen und Tabelle sammeln."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="storage-analytics"></a>Speicheranalyse

## <a name="overview"></a>Übersicht

Azure Speicheranalyse führt Protokollierung sowie Metrikdaten für ein Speicherkonto. Diese Daten können Sie Anfragen verfolgen, Verwendungstrends analysieren und diagnostizieren Probleme mit Ihrem Speicherkonto.

Um Speicheranalyse verwenden, müssen Sie sie einzeln für jeden Dienst aktivieren Sie überwachen möchten. Sie können es aus dem [Azure-Portal](https://portal.azure.com). Details finden Sie unter [Monitor ein Speicherkonto in Azure-Portal](storage-monitor-storage-account.md). Sie können auch programmgesteuert über die REST-API oder die Clientbibliothek Speicheranalyse. Verwenden Sie die Operationen [Blob-Eigenschaften abzurufen](https://msdn.microsoft.com/library/hh452239.aspx), [Eigenschaften für Warteschlange abrufen](https://msdn.microsoft.com/library/hh452243.aspx) [Eigenschaften für Tabelle abrufen](https://msdn.microsoft.com/library/hh452238.aspx)und [Eigenschaften für Datei erhalten](https://msdn.microsoft.com/library/mt427369.aspx) Speicheranalyse für jeden Dienst zu aktivieren.

Die aggregierten Daten werden in einem bekannten Blob (für Protokollierung) und bekannten Tabellen (Metriken), die mit der BLOB-Service Dienst APIs zugegriffen werden kann.

Speicheranalyse darf maximal 20 TB auf die Datenmenge, die unabhängig vom Speicherplatz für das Speicherkonto. Finden Sie weitere Informationen über Fakturierung und Datenaufbewahrungsrichtlinien [Speicheranalyse und Abrechnung](https://msdn.microsoft.com/library/hh360997.aspx). Weitere Informationen zu Konto Speichergrenzwerte finden Sie unter [Azure Storage Skalierbarkeits- und Performance-Ziele](storage-scalability-targets.md).

Ein detailliertes Handbuch mit Speicheranalyse und andere Tools identifizieren, diagnostizieren und beheben Probleme mit der Azure-Speicher finden Sie unter [Überwachung, diagnose und Problembehandlung von Microsoft Azure-Speicher](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="about-storage-analytics-logging"></a>Informationen zur Speicheranalyse Protokollierung

Speicheranalyse meldet Informationen über erfolgreiche und fehlgeschlagene Anfragen eines. Diese Informationen kann einzelne Anforderungen überwacht und zur Problemdiagnose mit einem Speicher verwendet werden. Anfragen werden jeweils bestmöglichen protokolliert.

Protokolleinträge werden nur dann, wenn Speicher Serviceaktivität erstellt. Beispielsweise verfügt ein Speicherkonto Aktivität im BLOB-Dienst jedoch nicht in der Tabelle oder Warteschlange Dienste, nur Protokolle für den BLOB-Dienst erstellt.

Storage Analytics Protokollierung ist nicht verfügbar für den Dienst Azure.

### <a name="logging-authenticated-requests"></a>Protokollierung authentifizierte Anfragen

Folgende authentifizierte Anfragen werden protokolliert:

- Erfolgreiche Anfragen.

- Fehler bei Anfragen Timeout Drosselung, Netzwerk, Autorisierung und andere Fehler.

- Anfragen mit einer freigegebenen Access Signatur (SAS), fehlgeschlagene und erfolgreiche Anfragen.

- Analysedaten aufgefordert.

Anfragen Speicheranalyse, Protokoll erstellen oder löschen, werden nicht protokolliert. Eine vollständige Liste der protokollierten Daten belegt [Speicher Analytics protokolliert und Statusnachrichten](https://msdn.microsoft.com/library/hh343260.aspx) und [Storage Analytics Protokollformat](https://msdn.microsoft.com/library/hh343259.aspx) Themen.

### <a name="logging-anonymous-requests"></a>Anonyme Anfragen protokollieren
Folgende anonyme Anfragen werden protokolliert:

- Erfolgreiche Anfragen.

- Server-Fehler.

- Timeoutfehler für Client und Server.

- Fehlgeschlagene GET-Anfragen mit Fehlercode 304 (nicht verändert).

Alle anderen fehlgeschlagenen anonymen Anfragen werden nicht protokolliert. Eine vollständige Liste der protokollierten Daten belegt [Speicher Analytics protokolliert und Statusnachrichten](https://msdn.microsoft.com/library/hh343260.aspx) und [Storage Analytics Protokollformat](https://msdn.microsoft.com/library/hh343259.aspx) Themen.

### <a name="how-logs-are-stored"></a>Speichern von Protokollen
Alle Protokolle werden im Block-Blobs in einem Container mit dem Namen $logs, die automatisch erstellt wird, wenn ein Speicherkonto Speicheranalyse kann gespeichert. $Logs Container befindet sich im Namespace BLOB-Speicher-Konto zum Beispiel: `http://<accountname>.blob.core.windows.net/$logs`. Dieser Container kann nicht gelöscht werden, nach dem Aktivieren der Speicheranalyse, obwohl sein Inhalt gelöscht werden können.

>[Azure.NOTE] $Logs Container wird nicht angezeigt, wenn ein Container mit Vorgang wie die [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) -Methode ausgeführt wird. Es muss direkt zugegriffen werden. Beispielsweise können Sie die [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) -Methode auf die Blobs in der `$logs` Container.
Wie Anfragen angemeldet sind, lädt Speicheranalyse Zwischenergebnisse als Blöcke. Regelmäßig wird Speicheranalyse commit dieser Blöcke und als Blob verfügbar machen.

Doppelte Datensätze können in derselben Stunde erstellten Protokolle vorhanden. Sie können feststellen, ob ein Datensatz ein Duplikat ist die Anzahl **RequestId** und **Betrieb** .

### <a name="log-naming-conventions"></a>Protokollieren von Namenskonventionen
Jedes Protokoll werden im folgenden Format geschrieben.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

Die folgende Tabelle beschreibt jedes Attribut in der Protokollname.

| Attribut         | Beschreibung                                                                                                                                                                                   |
|----------------   |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------   |
| < Dienstname >    | Der Name der Speicherdienst. Beispiel: Blob, Tabelle oder Warteschlange.                                                                                                                          |
| JJJJ              | Die Jahreszahl für das Protokoll. Beispiel: 2011.                                                                                                                                           |
| MM                | Zweistellige Monatszahl für das Protokoll. Beispiel: 07.                                                                                                                                             |
| DD                | Zweistellige Monatszahl für das Protokoll. Beispiel: 07.                                                                                                                                             |
| hh                | Die Stunde in zwei Ziffern, die erste Stunde Protokolle im 24-Stundenformat UTC angibt. Beispiel: 18.                                                                                     |
| mm                | Die zweistellige Zahl, die erste Minute für die Protokolle angibt. Dieser Wert wird nicht in der aktuellen Version der Speicheranalyse unterstützt und dessen Wert werden immer 00.     |
| <counter>         | Ein nullbasierter Zähler mit sechs Ziffern, der die Anzahl der Protokoll-Blobs hinsichtlich der Speicherdienst Stunde Zeit angibt. Dieser Zähler beginnt bei 000000. Beispiel: 000001.     |

Im folgenden finden einen vollständigen Protokollnamen, der in den vorherigen Beispielen kombiniert.

    blob/2011/07/31/1800/000001.log

Das folgende ist ein Beispiel URI, Zugriff auf das vorherige Protokoll verwendet werden kann.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Wenn ein protokolliert wird, entspricht der resultierende Protokollname Stunde, wenn der Vorgang abgeschlossen. Beispielsweise, 6:30 Uhr getBlob-Anforderung abgeschlossen wurde auf 7 31 2011 würde mit dem folgenden Präfix Protokoll geschrieben werden:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Protokollmetadaten
Protokoll-Blobs werden Metadaten gespeichert, die verwendet werden können, welche Daten im Blob enthaltenen identifizieren. Die folgende Tabelle beschreibt jedes Metadatenattribut.

| Attribut     | Beschreibung                                                                                                                                                                                                                                                   |
|------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| LogType       | Beschreibt, ob das Protokoll Informationen zum Lesen enthält, schreiben oder löschen. Dieser Wert kann eine oder eine Kombination aus drei, durch Kommas getrennt enthalten. Beispiel 1: schreiben. Beispiel 2: Lesen, schreiben; Beispiel 3: Lesen, schreiben, löschen.    |
| Startzeit     | Das früheste eines Eintrags in das Protokoll im Format YYYY-MM-: ssZ. Beispiel: 2011-07-31T18:21:46Z.                                                                                                                                             |
| Endzeit       | Spätestens ein Eintrag im Protokoll im Format YYYY-MM-: ssZ. Beispiel: 2011-07-31T18:22:09Z.                                                                                                                                               |
| LogVersion    | Die Version des Protokollformat. Derzeit ist der einzige unterstützte Wert 1.0.                                                                                                                                                                                     |

Die folgende Liste zeigt die vollständigen Metadaten mit den vorherigen Beispielen.

- LogType = schreiben

- StartTime = 2011-07-31T18:21:46Z

- EndTime = 2011-07-31T18:22:09Z

- LogVersion = 1,0

### <a name="accessing-logging-data"></a>Zugreifen auf Daten

Alle Daten in der `$logs` Container mit dem BLOB-APIs möglich, einschließlich der Azure bereitgestellten APIs der .NET verwaltete Bibliothek. Konto Speicheradministrator kann gelesen und Protokolle löschen, aber nicht erstellen oder aktualisieren sie. Namen und das Protokoll Metadaten können verwendet werden, wenn für eine Abfrage. Ist einer bestimmten Uhrzeit Protokolle angeordnet werden Metadaten gibt immer Timespan Einträge in ein Protokoll. Daher können Sie eine Kombination aus Namen und Metadaten bei der Suche nach einem bestimmten Protokoll.

## <a name="about-storage-analytics-metrics"></a>Zur Speicheranalyse Metriken

Speicheranalyse speichert Metriken, die aggregierte Statistiken und Kapazität Transaktionsdaten zu Anfragen eines enthalten. Transaktionen werden auf API-Vorgang sowie auf Service-Ebene, und Kapazität auf Service-Ebene gemeldet. Codemetrikdaten können verwendet werden, speichernutzung analysieren, Probleme mit Anfragen für den Speicherdienst diagnostizieren und die Leistung der Anwendung verbessern, die einen Dienst verwenden.

Um Speicheranalyse verwenden, müssen Sie sie einzeln für jeden Dienst aktivieren Sie überwachen möchten. Sie können es aus dem [Azure-Portal](https://portal.azure.com). Details finden Sie unter [Monitor ein Speicherkonto in Azure-Portal](storage-monitor-storage-account.md). Sie können auch programmgesteuert über die REST-API oder die Clientbibliothek Speicheranalyse. Verwenden Sie die **Eigenschaften abrufen** Operationen Speicheranalyse für jeden Dienst zu aktivieren.

### <a name="transaction-metrics"></a>Transaktion Metriken

Ein stabilen Satz von Daten werden in Intervallen von Stunden oder Minuten für jeden Speicherdienst angeforderte API Operation, einschließlich Ingress-Ausgang, Verfügbarkeit, Fehler und Anforderung Prozentsätze kategorisiert. Eine vollständige Liste der Buchungsdetails im Thema [Storage Analytics Metriken Tabellenschema](https://msdn.microsoft.com/library/hh343264.aspx) sehen.

Daten werden auf zwei Ebenen – Service und Betriebsebene API. Auf der Dienstebene angeforderte Statistik zusammenfassen alle API-Vorgänge in Tabellenentität stündlich geschrieben werden, auch wenn keine Anfragen an den Dienst wurden. Auf der Arbeitsgangsebene API Statistiken nur einer Entität geschrieben, wenn der Vorgang in dieser Stunde angefordert wurde.

Beispielsweise wenn eine Operation **GetBlob** BLOB-Dienst führen wird Storage Analytics-Metriken protokolliert die Anforderung und es in die aggregierten Daten sowohl BLOB-Dienst als auch den **GetBlob** -Vorgang. Jedoch wird kein **GetBlob** -Vorgang während der Stunde angefordert, eine Entität wird nicht geschrieben werden die `$MetricsTransactionsBlob` für diesen Vorgang.

Transaktion Metriken erfasst für Anfragen und Anträge Speicheranalyse selbst. Beispielsweise werden Anfragen von Speicheranalyse Protokolle und Tabellenentitäten aufgezeichnet. Weitere Informationen, wie diese Anfragen berechnet werden finden Sie unter [Speicheranalyse und Abrechnung](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Kapazität Metriken

>[AZURE.NOTE] Kapazität Metriken sind derzeit nur für BLOB-Dienst verfügbar. Kapazität Metriken für den Dienst und Warteschlangendienst werden in zukünftigen Versionen der Speicheranalyse.

Leistungsdaten werden täglich für ein Speicherkonto BLOB-Dienst und zwei Tabellenentitäten geschrieben. Eine Entität enthält Statistiken für Daten und anderen stellt Statistiken über die `$logs` BLOB-Container Speicheranalyse verwendet. Die `$MetricsCapacityBlob` Tabelle enthält die folgenden Statistiken:

- **Kapazität**: verwendet das Speicherkonto BLOB-Dienst in Byte Speicherplatz.

- **ContainerCount**: die Anzahl der BLOB-Container in das Speicherkonto BLOB-Dienst.

- **ObjectCount**: die Anzahl der engagierte und nicht festgeschriebene Blöcke oder Seiten-Blobs in das Speicherkonto BLOB-Dienst.

Weitere Informationen über die Kapazität Metriken finden Sie unter [Storage Analytics Metriken Tabellenschema](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Speicherung von Metriken

Alle Metrikdaten für jeden Speicher-Services werden in drei Tabellen für diesen Dienst reserviert: eine Tabelle für Informationen, eine Tabelle für Minute Informationen und einer anderen Tabelle Informationen. Buchung und Minute Buchungsinformationen besteht aus Anfragen und Antworten, Informationen aus Daten speichern. Metriken Stunde, Minute Metriken und Kapazitäten für ein Speicherkonto BLOB-Dienst in Tabellen möglich, die benannt sind, wie in der folgenden Tabelle beschrieben.

| Metriken Ebene                         | Tabellennamen                                                                                                                   | Unterstützte Versionen                                                                                                                        |
|------------------------------------   |-----------------------------------------------------------------------------------------------------------------------------  |---------------------------------------------------------------------------------------------------------------------------------------------- |
| Stündliche Metriken, primärer Standort      |  $MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue                                                  | Versionen vor 2013-08-15 nur. Während dieser Namen weiterhin unterstützt, empfiehlt es sich, wechseln die Tabellen unten.  |
| Stündliche Metriken, primärer Standort      | $MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue               | Alle Versionen einschließlich 2013-08-15.                                                                                                               |
| Minute Metriken, primärer Standort      | $MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue         | Alle Versionen einschließlich 2013-08-15.                                                                                                               |
| Stündliche Metriken sekundären Standort    | $MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue         | Alle Versionen einschließlich 2013-08-15. Lesezugriff Geo redundante Replikation muss aktiviert sein.                                                    |
| Minute Metriken sekundären Standort    | $MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue   | Alle Versionen einschließlich 2013-08-15. Lesezugriff Geo redundante Replikation muss aktiviert sein.                                                    |
| Kapazität (nur Blob-Dienst)          | $MetricsCapacityBlob                                                                                                          | Alle Versionen einschließlich 2013-08-15.                                                                                                               |


Diese Tabellen werden automatisch erstellt, wenn ein Speicherkonto Speicheranalyse kann. Sie können über den Namespace das Speicherkonto beispielsweise aufgerufen werden:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Metriken Datenzugriff

Alle Daten in den metriktabellen erfolgt unter Verwendung von Tabelle APIs, einschließlich der Azure bereitgestellten APIs der .NET verwalteten Bibliothek. Konto Speicheradministrator kann gelesen und Tabellenentitäten löschen, aber nicht erstellen oder aktualisieren sie.

## <a name="billing-for-storage-analytics"></a>Abrechnung für Speicheranalyse

Speicheranalyse ist durch ein Kontoinhaber Speicher aktiviert; Es ist nicht standardmäßig aktiviert. Alle Metrikdaten geschrieben Dienststellen ein Speicherkonto. Daher ist jeder Schreibvorgang Speicheranalyse durchgeführte berechenbarer. Darüber hinaus ist die beanspruchte Metrikdaten auch berechenbarer.

Die folgenden Aktionen Speicheranalyse sind berechenbare:

- Anfragen zum Erstellen von Blobs für die Protokollierung. 

- Anträge auf Tabellenentitäten für Metriken erstellen.

Wenn Sie eine Datenaufbewahrungsrichtlinie konfiguriert haben, Sie löschen bei Zahlen Speicheranalyse alte Protokollierung und Metriken Daten löscht. Von Löschtransaktionen von einem Client sind jedoch berechenbarer. Weitere Informationen zu Richtlinien finden Sie unter [Festlegen einer Aufbewahrungsrichtlinie Storage Analytics Daten](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Grundlegendes zu berechenbarer Anfragen

Jede Anforderung an ein Konto Speicherdienst ist abzurechnende und nicht abzurechnende. Speicheranalyse protokolliert jede einzelne Anforderung an einen Dienst auch einen Status, der angibt, wie die Anforderung verarbeitet wurde. Ebenso speichert Speicheranalyse Metriken für einen Dienst und API-Vorgänge des Dienstes, einschließlich der Prozentsätze und Anzahl bestimmter Nachrichten. Diese Features helfen Ihnen berechenbaren Anfragen analysieren, verbessern in der Anwendung und Anfragen mit Ihren Services Probleme diagnostizieren. Weitere Informationen zur Abrechnung finden Sie unter [Grundlegendes zu Azure Storage Billing - Bandbreite, Transaktionen und Kapazität](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Bei der Speicheranalyse Daten können Sie Tabellen im Thema [Speichervorgänge Analytics protokolliert und Statusnachrichten](https://msdn.microsoft.com/library/azure/hh343260.aspx) bestimmen, welche abrechenbar sind. Vergleichen Sie dann die Protokolle und Metrikdaten Statusnachrichten um festzustellen, ob eine bestimmte Anforderung berechnet wurden. Auch können die Tabellen im vorherigen Thema zu Verfügbarkeit für einen Speicherdienst oder einzelne API-Vorgang.

## <a name="next-steps"></a>Nächste Schritte

### <a name="setting-up-storage-analytics"></a>Einrichten von Speicheranalyse
- [Ein Speicherkonto in Azure-Portal überwachen](storage-monitor-storage-account.md)
- [Aktivieren und Konfigurieren der Speicheranalyse](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Storage Analytics Protokollierung  
- [Über Storage Analytics Protokollierung](https://msdn.microsoft.com/library/hh343262.aspx)
- [Storage Analytics Protokollformat](https://msdn.microsoft.com/library/hh343259.aspx)
- [Vorgänge und Statusnachrichten Speicheranalyse angemeldet](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Storage Analytics-Metriken
- [Über Storage Analytics-Metriken](https://msdn.microsoft.com/library/hh343258.aspx)
- [Storage Analytics Metriken Tabellenschema](https://msdn.microsoft.com/library/hh343264.aspx)
- [Vorgänge und Statusnachrichten Speicheranalyse angemeldet](https://msdn.microsoft.com/library/hh343260.aspx)  
