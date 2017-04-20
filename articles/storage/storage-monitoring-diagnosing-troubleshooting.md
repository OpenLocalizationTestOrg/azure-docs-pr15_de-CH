<properties
    pageTitle="Überwachung, diagnose und Problembehandlung Speicher | Microsoft Azure"
    description="Verwenden Sie Funktionen wie Speicheranalyse, clientseitige Protokollierung und andere Tools von Drittanbietern zu identifizieren, diagnostizieren und Probleme mit der Azure-Speicher beheben."
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="jahogg"/>

# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Überwachung, diagnose und Problembehandlung von Microsoft Azure-Speicher

[AZURE.INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Übersicht

Diagnose und Behebung von Problemen in einer verteilten Anwendung in einer Cloud-Umgebung kann komplexer als in einer traditionellen Umgebung. Clientanwendungen können in einer PaaS oder IaaS-Infrastruktur lokal auf einem mobilen Gerät oder in einer Kombination bereitgestellt werden. Normalerweise Netzwerkverkehr für Ihre Anwendung möglicherweise öffentliche und private Netzwerke durchlaufen und mehrere Technologien wie Microsoft Azure Tabellen, Blobs, Warteschlangen verwenden oder Dateien andere Daten speichert diese als relationale und Datenbanken.

Anträge erfolgreich verwalten Sie proaktiv überwachen und diagnostizieren und beheben alle Aspekte und deren abhängige Technologie verstehen. Als Benutzer von Azure Storage Services sollten Sie kontinuierlich überwachen Storage Services nutzt die Anwendung Änderungen unerwartetes Verhalten (z. B. langsamer als gewohnt Antwortzeiten) und Protokollierung verwenden, um detailliertere Daten sammeln und ein Problem im Detail analysieren. Diagnose-Informationen erhalten Sie von Überwachung und Protokollierung können Sie die Ursache des Problems bestimmen die Anwendung festgestellt. Anschließend beheben Sie das Problem und die entsprechenden Schritte können Sie es beheben. Azure-Speicher ist ein Core Azure Service und ist ein wichtiger Teil der meisten Projektmappen, die Kunden der Azure-Infrastruktur bereitstellen. Azure-Speicher enthält Funktionen zur Vereinfachung der Überwachung, Diagnose und Problembehandlung Speicher in der Cloud-basierte Anwendung.

> [AZURE.NOTE] Speicherkonten mit Replikation Zone redundanten Speicher (ZRS) müssen nicht Metriken oder Protokollfunktion zurzeit aktiviert. Azure Dateien Service unterstützt auch keine Protokollierung zu diesem Zeitpunkt.

Eine praktische Anleitung zu Ende-Problembehandlung in Azure Storage-Anwendung finden Sie unter [End-to-End-Problembehandlung mit Azure Storage Metriken und Protokollierung, AzCopy, und Nachricht](storage-e2e-troubleshooting.md).

+ [Einführung]
    + [Aufbau dieses Handbuchs]
+ [Der Speicherdienst überwacht]
    + [Dienststatus überwachen]
    + [Überwachen der Kapazität]
    + [Überwachen der Verfügbarkeit]
    + [Überwachen der Leistung]
+ [Diagnose von Problemen Speicher]
    + [Service Gesundheitsrisiken]
    + [Performance-Probleme]
    + [Diagnostizieren von Fehlern]
    + [Emulator Speicherprobleme]
    + [Protokollierung-Datenspeicher]
    + [Netzwerk-Protokollierung verwenden]
+ [End-to-End-tracing]
    + [Korrelieren von Daten]
    + [Client-Anfrage-ID]
    + [Server-Kennung]
    + [Zeitstempel]
+ [Hinweise zur Problembehandlung]
    + [Hohe AverageE2ELatency und niedrigen AverageServerLatency anzeigen Metriken]
    + [Metriken niedrige AverageE2ELatency und niedrigen AverageServerLatency, sondern der Client treten Wartezeiten]
    + [Metriken zeigen hohe AverageServerLatency]
    + [Es treten unerwartete bei Zustellung von Nachrichten in einer Warteschlange]
    + [Metriken zeigen eine PercentThrottlingError]
    + [Metriken zeigen eine PercentTimeoutError]
    + [Metriken zeigen eine PercentNetworkError]
    + [Der Client empfängt Nachrichten HTTP 403 (Verboten)]
    + [Der Client empfängt Nachrichten HTTP 404 (nicht gefunden)]
    + [Der Client ist HTTP 409 (Konflikt) Nachrichten empfängt.]
    + [Metriken zeigen niedrige PercentSuccess oder Analytics Protokolleinträge arbeiten mit Buchungsstatus der ClientOtherErrors]
    + [Kapazität Metriken zeigen ein unerwarteter Speicher-Auslastung]
    + [Unerwarteter Neustart virtueller Maschinen auftreten, die eine große Anzahl von angeschlossenen Festplatten]
    + [Das Problem entsteht Speicheremulator für Entwicklungs- oder Testzwecke verwenden]
    + [Probleme bei der Installation von Azure SDK für .NET stoßen]
    + [Sie haben ein anderes Problem mit einem Storage-Dienst]
    + [Problembehandlung bei Azure Dateien mit Windows und Linux](storage-troubleshoot-file-connection-problems.md)
+ [Anhänge]
    + [Anhang 1: Mithilfe von Fiddler HTTP- und HTTPS-Datenverkehr]
    + [Anlage 2: Mithilfe von Wireshark Netzwerkverkehr]
    + [Anhang 3: Mithilfe von Microsoft Message Analyzer Netzwerkverkehr]
    + [Anhang 4: Verwendung von Excel zu Metriken Protokolldaten]
    + [Anhang 5: Überwachung Anwendung Einblicke für Visual Studio Team Services]

## <a name="introduction"></a>Einführung

Dieses Handbuch Sie Funktionen wie Azure Storage Analytics veranschaulicht Fragen clientseitige Protokollierung in Azure Storage-Clientbibliothek und andere Tools von Drittanbietern zu identifizieren, diagnostizieren und Beheben von Azure Storage.

![][1]

*Abbildung 1 Überwachung, Diagnose und Problembehandlung*

Dieses Handbuch soll hauptsächlich von Entwicklern von Onlinediensten gelesen werden, die Azure-Speicherdienste und IT-Experten diese online-Dienste verwalten. Die Ziele dieses Handbuchs sind:

- Zustand und Leistung von Azure-Speicherkonten verwalten können.
- Zur Verfügung stellen die notwendigen Prozesse und Tools zur Entscheidung, ob Probleme in einer Anwendung in Azure-Speicher bezieht.
- Umsetzbare Anleitung zum Lösen von Problemen im Zusammenhang mit Azure-Speicher bieten.

### <a name="how-this-guide-is-organized"></a>Aufbau dieses Handbuchs

Abschnitt "[Überwachung der Speicherdienst]" beschreibt den Zustand und die Leistung Ihrer Azure Storage Services Azure Storage Analytics Metriken (Storage Metriken) überwachen.

Abschnitt "[Diagnose Speicherprobleme]" beschreibt die Problemdiagnose Azure Storage Analytics Protokollierung (Speicher Protokollierung) verwenden. Er beschreibt auch clientseitige mithilfe der Funktionen in einer Client-Bibliotheken wie der Speicher-Clientbibliothek für .NET oder Azure SDK für Java-Protokollierung.

Im Abschnitt "[Ende - Verfolgung]" werden wie die Informationen in verschiedenen Protokolldateien und Metrikdaten zuordnen können.

Im Abschnitt "[Problembehandlung Anleitung]" enthält Hinweise zur Problembehandlung für einige der speicherbezogene Probleme auftreten.

"[Anhänge]" enthalten Informationen über andere Tools wie Wireshark und Netmon für Paket Netzwerkdaten Fiddler für die Analyse von HTTP/HTTPS-Nachrichten und Microsoft Message Analyzer zum Korrelieren von Daten analysieren.


## <a name="monitoring-your-storage-service"></a>Der Speicherdienst überwacht

Wenn Sie mit Windows-Leistungsindikatoren vertraut sind, können Sie Speichermetriken als Azure Storage Entsprechung eines Windows-Leistungsindikatoren vorstellen. In Speichermetriken finden Sie eine umfassende Reihe von Metriken (Leistungsindikatoren im Systemmonitor Terminologie) wie Service, Gesamtanzahl der Anfragen Service oder Prozentsatz der erfolgreichen Anfragen Service. Eine vollständige Liste der verfügbaren Metriken finden Sie unter [Storage Analytics Metriken Tabellenschema](http://msdn.microsoft.com/library/azure/hh343264.aspx). Sie können angeben, ob den Speicherdienst sammeln und aggregieren Metriken pro Stunde oder pro Minute. Weitere Informationen zum Aktivieren von Metriken und Speicherkonten überwachen finden Sie unter [speichermetriken aktivieren und Metrikdaten](http://go.microsoft.com/fwlink/?LinkId=510865).

Sie können die stündlichen Metriken in [Azure-Portal](https://portal.azure.com) anzeigen und Konfigurieren von Regeln, die Administratoren benachrichtigen sollen e-Mail immer eine stündliche Metrik einen bestimmten Schwellenwert überschreitet. Weitere Informationen finden Sie in der [Benachrichtigung erhalten](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). 

Der Speicherdienst sammelt Metriken anhand einer jedoch jedem Speichervorgang kann nicht aufzeichnen.

Im Azure-Verwaltungsportal wie Verfügbarkeit, Anfragen insgesamt und durchschnittliche Wartezeit Zahlen für ein Speicherkonto angezeigt. Eine Benachrichtigungsregel wurde auch für die Verfügbarkeit unter ein bestimmtes Niveau fällt ein Administrator eine Warnung eingerichtet. Anzeigen dieser Daten eine mögliche Untersuchung ist der Tabelle Service Erfolg Prozentsatz unter 100 % (Weitere Informationen finden Sie im Abschnitt "[Metriken zeigen niedrige PercentSuccess oder Analytics Protokolleinträge arbeiten mit Transaktion ClientOtherErrors]").

Sie sollten ständig überwachen Ihre Azure Applications fehlerfrei und Performance wie erwartet werden:

- Einige grundlegenden Faktoren für die Anwendung, die Ihnen ermöglichen, aktuelle Daten vergleichen und alle Änderungen im Verhalten der Azure-Speicher und Ihre Anwendung zu identifizieren. Die Werte der grundlegenden Faktoren in vielen Fällen werden anwendungsspezifisch und Sie sollte sie Leistungstests für die Anwendung sind.
- Minute Metriken aufzeichnen, unerwartete Fehler und Anomalien wie Spitzen Fehler Überwachen mithilfe zählt und Preise anfordern.
- Stündliche Metriken aufzeichnen und damit Durchschnittswerte überwachen durchschnittliche Anzahl und Preise anfordern.
- Mögliche Fragen mit Diagnose-Tools, wie weiter unten im Abschnitt "[Diagnose Speicherprobleme]."

Die Diagramme in Abbildung 3 unten zeigen wie Mittelwert, für die stündliche Metriken eintritt Spitzen Aktivität ausblenden kann. Stündlichen Metriken sein scheinen stetig Anfragen während der Minute Metriken Fluktuationen anzuzeigen, die wirklich sind.

![][3]

Der Rest dieses Abschnitts wird beschrieben, welche Metriken sollten Sie überwachen und warum.

### <a name="monitoring-service-health"></a>Dienststatus überwachen

[Azure-Portal](https://portal.azure.com) können Sie den Zustand der Speicherdienst (und andere Azure-Dienste) in Azure Regionen auf der ganzen Welt anzeigen. Dadurch können Sie sofort sehen, ob das Problem außerhalb der Kontrolle den Speicherdienst in der Region beeinflusst für Ihre Anwendung verwenden.

[Azure-Portal](https://portal.azure.com) bieten auch Benachrichtigung von Vorfällen, die die verschiedenen Azure Dienste auswirken.
Hinweis: Diese Angaben waren zuvor mit Verlaufsdaten [Azure Service Dashboard](http://status.azure.com)zur Verfügung.

Während der [Azure-Portal](https://portal.azure.com) Gesundheitsinformationen in Azure-Datencentern (Überwachen von innen nach außen) erfasst, können Sie ggf. auch außen Ansatz synthetische Transaktionen generiert, die regelmäßig Ihre Azure gehosteten Web-Anwendung von mehreren Speicherorten zugreifen. [Dynatrace](http://www.dynatrace.com/en/synthetic-monitoring) und Anwendung Einblicke für Visual Studio Team Services angebotenen Dienste sind Beispiele für diesen Ansatz außen. Weitere Informationen zu Application Insights für Visual Studio Team Services finden Sie im Anhang "[Anhang 5: Anwendung Einblicke für Visual Studio Team Services Überwachung](#appendix-5)."

### <a name="monitoring-capacity"></a>Überwachen der Kapazität

Speichermetriken nur speichert Kapazität für den BLOB-Dienst denn Blobs normalerweise den größten Anteil der Daten (Zeitpunkt schreiben kann nicht mit Speichermetriken Kapazität der Tabellen und Warteschlangen überwachen). Diese Daten finden in der Tabelle **$MetricsCapacityBlob** Sie, wenn der BLOB-Dienst Überwachung aktiviert ist. Storage-Metriken werden diese Daten einmal pro Tag und können Sie den Wert des **RowKey** bestimmen, ob die Zeile eine Entität, die auf Daten ( **Data**) oder Analysedaten (Wert **Analytics**). Jede gespeicherte Entität enthält Informationen über die Menge des verwendeten Speichers (**Kapazität** gemessen in Bytes) und die aktuelle Anzahl von Containern (**ContainerCount**) und Blobs (**ObjectCount**) in das Speicherkonto verwendet. Weitere Informationen über die Kapazität Metriken in der **$MetricsCapacityBlob** -Tabelle gespeicherten anzeigen Sie [Storage Analytics Metriken Tabellenschema](http://msdn.microsoft.com/library/azure/hh343264.aspx)

> [AZURE.NOTE] Sie sollten diese Werte für eine frühzeitige Warnung überwachen, der Kapazitätsgrenze Ihres Speicherkontos erreichen. In der Azure-Verwaltungsportal fügen Sie Warnregeln zu benachrichtigen, wenn aggregate Speicher verwenden überschreitet oder Schwellenwerte, die Sie angeben.

Schätzen der Größe verschiedener Speicher Objekte wie Blobs Hilfe finden Sie im Blogbeitrag [Verständnis Azure Storage Billing-Bandbreite, Transaktionen und Kapazität](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

### <a name="monitoring-availability"></a>Überwachen der Verfügbarkeit

Überwachen der Verfügbarkeit Speicher-Services in das Speicherkonto sollte den Wert in **die Spalte in den Tabellen pro Stunde oder Minute Metriken** Überwachen – **$MetricsHourPrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$MetricsCapacityBlob**. **Die Spalte** enthält einen Prozentwert, der die Verfügbarkeit des Dienstes oder der Zeile (die **RowKey** zeigt enthält die Zeile Kriterien für den Dienst als Ganzes oder für eine bestimmte API-Operation) API-Vorgang angibt.

Jeder Wert kleiner als 100 % bedeutet, dass einige Speicheranfragen fehlschlagen. Sie können sehen, warum sie andernfalls anhand der Spalten in der Metriken, die die Anzahl der Anfragen mit verschiedenen Fehlertypen wie **ServerTimeoutError**angezeigt. Sie sollten die **Verfügbarkeit** vorübergehend Gründen wie transiente Servertimeout unter 100 % liegen während der Dienst Partitionen zu besseren Lastenausgleich Anforderung wechselt; die Wiederholungslogik in der Clientanwendung sollte diese zeitweise zu verarbeiten. [Storage Analytics protokolliert und Statusnachrichten](http://msdn.microsoft.com/library/azure/hh343260.aspx) Listet die Buchungsarten Speichermetriken in seiner Berechnung **Verfügbarkeit** enthält.

In [Azure-Portal](https://portal.azure.com)fügen Sie Warnregeln zu benachrichtigen, wenn **Verfügbarkeit** für einen Dienst unter einem Schwellenwert liegt.

Der Abschnitt "[Problembehandlung Anleitung]" beschreibt einige Storage Serviceprobleme hinsichtlich Verfügbarkeit.

### <a name="monitoring-performance"></a>Überwachen der Leistung

Zum Überwachen der Leistung von Speicher-Services können Sie die folgenden Metriken aus den metriktabellen pro Stunde und Minute.

- Die Werte in den Spalten **AverageE2ELatency** und **AverageServerLatency** zeigen der Durchschnittsdauer den Speicherdienst oder API-Vorgangstyp ist in Anfragen. **AverageE2ELatency** ist eine End-to-End-Latenz, die die Zeit lesen die Anfrage und sendet die Antwort neben der Verarbeitungszeit für die Anforderung enthält (daher umfasst Netzwerklatenz erreicht die Anforderung den Speicherdienst); **AverageServerLatency** zeigt nur die Verarbeitungszeit und daher schließt alle Netzwerklatenz zur Kommunikation mit dem Client verknüpft. Abschnitt "[hohe AverageE2ELatency und niedrigen AverageServerLatency Metriken anzeigen]" in diesem Handbuch Informationen daher möglicherweise eine erhebliche Differenz zwischen diesen beiden Werten.
- Die Werte in den Spalten **TotalIngress** und **TotalEgress** anzeigen die gesamte Datenmenge Bytes, um eingehenden und ausgehenden Ihr Speicher oder durch eine bestimmte API Vorgangstyp
- Die Werte in der Spalte **TotalRequests** anzeigen Gesamtzahl der Speicherdienst API Operation empfängt Anfragen **TotalRequests** ist die Gesamtzahl der Anfragen, die Speicherdienst empfängt.

In der Regel überwachen für unerwartete Änderung dieser Werte als Indikator Sie haben Sie ein Problem, das untersucht werden muss.

In [Azure-Portal](https://portal.azure.com)können Sie Warnregeln um benachrichtigen Wenn Leistungsdaten für diesen Dienst unterschreiten hinzufügen oder überschritten, die Sie angeben.

Der Abschnitt "[Problembehandlung Anleitung]" beschreibt einige allgemeine Storage Serviceprobleme mit Leistung.


## <a name="diagnosing-storage-issues"></a>Diagnose von Problemen Speicher

Es gibt verschiedene Arten, dass Sie ein Problem oder eine Frage in der Anwendung bewusst möglicherweise dazu gehören:

- Eines schwerwiegenden Fehlers, bei dem die Anwendung abstürzt oder nicht mehr funktioniert.
- Änderungen von Basisplanwerte Metriken beobachten Sie wie im vorherigen Abschnitt "[Überwachung der Speicherdienst]."
- Berichte von Benutzern der Anwendung, die ein bestimmter Vorgang nicht wie erwartet abgeschlossen oder nicht funktionieren einige Features.
- Fehler in der Anwendung angezeigten Protokolldateien oder durch andere Benachrichtigungsmethode.

In der Regel fallen Fragen Azure-Speicherdienste in vier Kategorien:

- Die Anwendung hat ein Leistungsproblem, von Ihren Benutzern gemeldet oder Änderung Leistungsdaten angezeigt.
- Es ist ein Problem mit der Azure-Speicher-Infrastruktur in einem oder mehreren Bereichen.
- Die Anwendung auftreten eines Fehlers, von Ihren Benutzern gemeldet oder festgestellte Zunahme eines Fehler Anzahl Metriken überwacht werden.
- Bei Entwicklung und Test können Sie lokalen Speicheremulator verwenden. Einige Probleme auftreten, die speziell für die Verwendung der Speicheremulator beziehen.

Die folgenden Abschnitte beschreiben die Schritte Sie befolgen diagnostizieren und Beheben von Problemen in jede dieser vier Kategorien. Im Abschnitt "[Problembehandlung Anleitung]" in diesem Handbuch werden detaillierte für einige Probleme, die auftreten können.

### <a name="service-health-issues"></a>Service Gesundheitsrisiken

Service sind Health normalerweise außerhalb Ihrer Kontrolle. [Azure-Portal](https://portal.azure.com) bietet Informationen über anhaltende Probleme mit Azure Storage Services einschließlich. Bei Lesezugriff Geo redundanter Speicher Wenn das Speicherkonto erstellt, kann dann im Fall der primäre Speicherort nicht verfügbar ist die Anwendung vorübergehend schreibgeschützte Kopie am sekundären Standort wechseln. Dazu muss die Anwendung wechseln zwischen primären und sekundären Speicherorte und im reduzierten Funktionsmodus mit schreibgeschützten Daten arbeiten. Azure Storage Clientbibliotheken können wiederholen Namensrichtlinie, die bei Ausfall ein Lesevorgang vom primären Speicher von sekundären Speicher lesen kann. Ihre Anwendung muss ebenfalls beachtet werden, dass die Daten an den sekundären Standort schließlich konsistent sind. Weitere Informationen finden Sie im Blogbeitrag [Azure Storage Redundanz und Lesezugriff Geo redundanten Speicher](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

### <a name="performance-issues"></a>Performance-Probleme

Die Leistung einer Anwendung kann subjektiv, insbesondere aus Sicht des Benutzers. Deshalb muss grundlegenden Faktoren können Sie ermitteln, wo möglicherweise ein Leistungsproblem. Viele Faktoren können die Leistung eines Azure-Speicher aus der Perspektive der Clientanwendung beeinträchtigen. Diese Faktoren können in der Speicherdienst, der Client oder der Netzwerkinfrastruktur verwendet werden. Daher ist es wichtig, eine Strategie für den Ursprung des Problems ermitteln.

Nachdem Sie wahrscheinlich Speicherort der Ursache des Leistungsproblems von Metriken identifiziert haben, dann können die Protokolldateien Sie ausführliche Informationen zum Diagnostizieren und beheben Sie das Problem weiter.

Im Abschnitt "[Problembehandlung Anleitung]" in diesem Handbuch bietet Informationen über einige allgemeine Leistung Probleme auftreten.

### <a name="diagnosing-errors"></a>Diagnostizieren von Fehlern

Benutzer der Anwendung möglicherweise Fehler gemeldet von der Clientanwendung benachrichtigt. Speichermetriken hinaus zählt verschiedene Fehlertypen von Storage-Services wie **NetworkError**, **ClientTimeoutError**oder **AuthorizationError**. Beim Speichermetriken zählt verschiedene Fehlertypen nur Datensätze erhalten Sie ausführlicher Wünsche, serverseitige, clientseitige, und Netzwerk-Protokolle. Normalerweise wird der HTTP-Statuscode der Speicherdienst zurückgegebene Hinweise der Anforderung fehlschlagen.

> [AZURE.NOTE] Beachten Sie, dass Sie erwarten gelegentlich Fehler: z. B. Fehler durch vorübergehende Netzwerkprobleme oder Anwendungsfehler.

Die folgenden Ressourcen sind hilfreich speicherbezogenen Status und Fehlercodes:

- [Allgemeine Fehlercodes für REST-API](http://msdn.microsoft.com/library/azure/dd179357.aspx)
- [BLOB-Dienst-Fehlercodes](http://msdn.microsoft.com/library/azure/dd179439.aspx)
- [Warteschlange Service-Fehlercodes](http://msdn.microsoft.com/library/azure/dd179446.aspx)
- [Tabelle Service-Fehlercodes](http://msdn.microsoft.com/library/azure/dd179438.aspx)
- [Datei Service-Fehlercodes](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a>Emulator Speicherprobleme

Azure SDK enthält Speicheremulator auf einer Entwicklungsarbeitsstation ausführen. Dieser Emulator simuliert Großteil des Verhaltens des Azure-Speicherdienste und ist hilfreich beim Entwickeln und Testen Programme ausführen, die Azure-Speicherdienste ohne Azure-Abonnement sowie ein Azure-Speicher verwenden.

Der Abschnitt "[Problembehandlung Anleitung]" beschreibt einige häufig auftretende Probleme mit der Speicheremulator.

### <a name="storage-logging-tools"></a>Protokollierung-Datenspeicher

Speicherprotokollierung bietet serverseitige Protokollierung von Speicheranfragen Kontos Azure-Speicher. Weitere Informationen zu serverseitigen Protokollierung auf die Daten zugreifen finden Sie unter [Aktivieren von Speicher und den Zugriff auf Daten](http://go.microsoft.com/fwlink/?LinkId=510867).

Der Speicher-Clientbibliothek für .NET können Sie clientseitige Daten sammeln, die von der Anwendung durchgeführten Speichervorgänge auf. Weitere Informationen finden Sie in der [clientseitigen Protokollierung mit .NET Speicher-Clientbibliothek](http://go.microsoft.com/fwlink/?LinkId=510868).

> [AZURE.NOTE] In einigen Fällen (z. B. Autorisierungsfehler SAS) kann ein Benutzer eine Fehlermeldung für die Sie keine Daten in die serverseitigen Speicherprotokollen finden. Sie können Protokollierungsfunktionen der Speicher-Clientbibliothek auf dem Client ist die Ursache des Problems zu oder Netzwerk-Überwachungstools im Netzwerk zu.

### <a name="using-network-logging-tools"></a>Netzwerk-Protokollierung verwenden

Sie können den Datenverkehr zwischen Client und Server enthalten ausführliche Informationen über den Client und Server Austauschen von Daten und zugrunde liegende Netzwerk erfassen. Protokollierung hilfreich Netzwerktools gehören:

- [Fiddler](http://www.telerik.com/fiddler) ist eine kostenlose Web debugging Proxy, der die Header und die Nutzlastdaten HTTP- und HTTPS-Anforderung und Antwort Nachrichten untersuchen kann. Weitere Informationen finden Sie unter [Anlage 1: Fiddler HTTP- und HTTPS-Datenverkehr erfassen mithilfe von](#appendix-1).
- [Microsoft Network Monitor (Netmon)](http://www.microsoft.com/download/details.aspx?id=4865) und [Wireshark](http://www.wireshark.org/) sind kostenlose Netzwerk Protokollanalyseprogrammen, die Ihnen ermöglichen, detaillierte Paketinformationen für eine Vielzahl von Netzwerkprotokollen anzuzeigen. Weitere Informationen zu Wireshark finden Sie unter "[Anlage 2: mithilfe von Wireshark von Netzverkehr](#appendix-2)".
- Microsoft Message Analyzer ist ein Tool von Microsoft, die Netmon und dass zusätzlich zum Sammeln von Netzwerkdaten Paket ersetzt, hilft Ihnen, anzeigen und Analysieren der Daten von anderen Tools erfasst. Weitere Informationen finden Sie unter "[Anlage 3: verwenden Microsoft Message Analyzer von Netzverkehr](#appendix-3)".
- Wenn Sie grundlegende Konnektivität Testen der Clientcomputer den Azure-Speicher-Dienst über das Netzwerk Verbindung überprüfen möchten, Sie nicht mit dem standard- **Ping** -Tool auf dem Client. Das [ **Tcping** -Tool](http://www.elifulkerson.com/projects/tcping.php) können Sie jedoch Konnektivität überprüfen.

In vielen Fällen die Protokolldaten Protokollierung Speicher und die Speicher-Clientbibliothek ausreichen Problem diagnostizieren, aber in einigen Szenarios möglicherweise Informationen, die diese Protokollierungstools Netzwerk bereitstellen können. Beispielsweise kann mit Fiddler HTTP- und HTTPS-Nachrichten anzeigen, Header und Nutzlast Daten zu und von Storage Services ermöglicht Sie untersuchen, wie eine Clientanwendung Speichervorgänge wiederholt gesendet. Protokollanalyseprogrammen wie Wireshark arbeiten auf Paketebene können Sie verloren gegangene Pakete und Verbindungsprobleme behandeln können TCP-Daten anzeigen. Message Analyzer lässt sich auf HTTP und TCP.

## <a name="end-to-end-tracing"></a>End-to-End-tracing

End-to-End-Tracing mithilfe verschiedener Protokolldateien ist ein nützliches Verfahren zur Untersuchung von Problemen. Können Sie die Informationen aus den Metrikdaten als Hinweis, wo die Suche in den Protokolldateien für detaillierte Informationen, die Sie zur Problembehandlung.

### <a name="correlating-log-data"></a>Korrelieren von Daten

Wenn Protokolle von Clientanwendungen anzeigen Netz verfolgt und serverseitige Speicher-Protokollierung ist für korrelieren können über verschiedene Protokolldateien fordert. Die Protokolldateien enthalten eine Reihe verschiedener Felder als Korrelations-IDs verwendet werden. Die Client-Id ist das nützlichste mit Posten in den verschiedenen Protokollen korrelieren. Aber manchmal kann es serverkennung oder Zeitstempel verwenden. Die folgenden Abschnitte enthalten weitere Informationen zu diesen Optionen.

### <a name="client-request-id"></a>Client-Anfrage-ID

Der Speicher-Clientbibliothek generiert automatisch eine Anforderung eindeutige Client-Id für jede Anforderung.

- Auf das Client-seitige Speicher-Clientbibliothek erstellt, wird die Client-Id im Feld **Clientanfragen-ID** jedes Protokolleintrags für die Anforderung.
- In einer Netzwerk-Trace wie Fiddler erfasst ist die Client-Id in Anforderungsnachrichten als **X-ms-Client-Anfrage-Id** -HTTP-Header-Wert.
- Im serverseitigen Speicher Protokollierung Protokoll wird die Client-Id in der Spalte Client-Anforderung angezeigt.

> [AZURE.NOTE]Es ist möglich, mehrere Anfragen die gleichen Client Id freigeben, da der Client diesen Wert zuweisen kann (obwohl Speicher-Clientbibliothek automatisch einen neuen Wert zugewiesen). Bei Wiederholungsversuchen vom Client teilen alle Versuche die gleichen Client-Id. Bei einem Stapel vom Client gesendet werden muss die Stapelverarbeitung einzelnen Kennung.


### <a name="server-request-id"></a>Server-Kennung

Der Speicherdienst generiert automatisch Server Anforderung Ids.

- Im serverseitigen Speicher Protokollierung Protokoll wird die Server-Id der Spalte **Kennung Kopfzeile** angezeigt.
- In einer Netzwerk-Trace wie Fiddler erfasst wird die Server-Id in Antwortnachrichten als **X-ms-Anfrage-Id** -HTTP-Header-Wert.
- Auf das Client-seitige Speicher-Clientbibliothek erstellt, wird die Server-Id in der Spalte **Vorgangstext** mit Details der Serverantwort Protokolleintrag angezeigt.

> [AZURE.NOTE] Der Speicherdienst weist immer einen eindeutigen Server Kennung jeder Anforderung erhält, so alle Wiederholungsversuch vom Client und jeden Vorgang in einem Stapel enthalten eindeutige Kennung.

Wenn der Speicher-Clientbibliothek auf dem Client **StorageException** auslöst, enthält die Eigenschaft **RequestInformation** ein **RequestResult** -Objekt, das eine **ServiceRequestID** -Eigenschaft enthält. Eine **OperationContext** -Instanz kann auch ein **RequestResult** -Objekt zugreifen.

Das folgende Codebeispiel veranschaulicht, wie eine benutzerdefinierte **ClientRequestId** Wert von der Speicherdienst **OperationContext** -Objekt der Anforderung zuweisen. Es wird gezeigt, wie den Wert **ServerRequestId** aus der Antwortnachricht abzurufen.

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
    OperationContext oc = new OperationContext();
    oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

    try
    {
        CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
        ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
        var downloadToPath = string.Format("./{0}", blob.Name);
        using (var fs = File.OpenWrite(downloadToPath))
        {
            blob.DownloadToStream(fs, null, null, oc);
            Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
        }
    }
    catch (StorageException storageException)
    {
        Console.WriteLine("Storage exception {0} occurred", storageException.Message);
        // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
        foreach (var result in oc.RequestResults)
        {
                Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
        }
    }


### <a name="timestamps"></a>Zeitstempel

Sie können auch Zeitstempel verwandte Einträge suchen, aber Vorsicht jede Uhr Neigung zwischen Client und Server, die möglicherweise. Sie sollten plus oder minus 15 Minuten übereinstimmende serverseitige Einträge basierend auf der Zeitstempel auf dem Client suchen. Beachten Sie, dass die BLOB-Metadaten für die Blobs mit Metriken den Zeitraum für die Metriken in dem Blob gespeichert angibt; Dies ist nützlich, wenn viele Metriken Blobs für den gleichen Minuten oder Stunden.

## <a name="troubleshooting-guidance"></a>Hinweise zur Problembehandlung

Dieser Abschnitt hilft Ihnen bei der Diagnose und Problembehandlung Ihrer Anwendung einige der Probleme auftreten, wenn Azure-Speicherdienste zu verwenden. Suchen Sie Informationen zu Ihrem speziellen Problem mit der Liste.

**Entscheidungsstruktur**

----------

Bezieht sich das Problem auf die Leistung eines Speicher-Services?

- [Hohe AverageE2ELatency und niedrigen AverageServerLatency anzeigen Metriken]
- [Metriken niedrige AverageE2ELatency und niedrigen AverageServerLatency, sondern der Client treten Wartezeiten]
- [Metriken zeigen hohe AverageServerLatency]
- [Es treten unerwartete bei Zustellung von Nachrichten in einer Warteschlange]

----------

Bezieht sich das Problem auf die Verfügbarkeit von Speicher-Services?

- [Metriken zeigen eine PercentThrottlingError]
- [Metriken zeigen eine PercentTimeoutError]
- [Metriken zeigen eine PercentNetworkError]

----------

Erhält die Clientanwendung eine HTTP-Antwort 4XX (z. B. 404) von einem Storage-Service?

- [Der Client empfängt Nachrichten HTTP 403 (Verboten)]
- [Der Client empfängt Nachrichten HTTP 404 (nicht gefunden)]
- [Der Client ist HTTP 409 (Konflikt) Nachrichten empfängt.]

----------

[Metriken zeigen niedrige PercentSuccess oder Analytics Protokolleinträge arbeiten mit Buchungsstatus der ClientOtherErrors]

----------

[Kapazität Metriken zeigen ein unerwarteter Speicher-Auslastung]

----------

[Unerwarteter Neustart virtueller Maschinen auftreten, die eine große Anzahl von angeschlossenen Festplatten]

----------

[Das Problem entsteht Speicheremulator für Entwicklungs- oder Testzwecke verwenden]

----------

[Probleme bei der Installation von Azure SDK für .NET stoßen]

----------

[Sie haben ein anderes Problem mit einem Storage-Dienst]

----------

### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Hohe AverageE2ELatency und niedrigen AverageServerLatency anzeigen Metriken

Die Abbildung unten aus dem [Azure-Portal](https://portal.azure.com) Überwachungstool Beispiel ein deutlich höher als die **AverageServerLatency**ist **AverageE2ELatency** .

![][4]

Beachten Sie, dass der Speicherdienst berechnet nur die Metrik **AverageE2ELatency** für erfolgreiche und im Gegensatz zu **AverageServerLatency**enthält die Zeit, die der Client die Daten senden und empfangen Bestätigung aus der Speicherdienst. Daher könnte ein Unterschied zwischen **AverageE2ELatency** und **AverageServerLatency** durch die Clientanwendung wird langsam oder bedingt im Netzwerk.

> [AZURE.NOTE] Sie können auch **E2ELatency** und **ServerLatency** für einzelne Speichervorgänge im Speicher Protokollierung Protokolldaten anzeigen.

#### <a name="investigating-client-performance-issues"></a>Untersuchung von Leistungsproblemen client

Gründe für den Client reagiert langsam sind eine begrenzte Anzahl von verfügbaren Verbindungen oder Threads oder niedrig auf Ressourcen wie CPU, Arbeitsspeicher oder Netzwerk-Bandbreite. Sie können das Problem durch ändern den Clientcode effizienter (z. B. mit asynchronen Aufrufe der Speicherdienst) oder mit einem größeren virtuellen Computer (mehr Kerne und mehr Arbeitsspeicher) sein.

Für die Tabelle und Warteschlange der Nagle-Algorithmus kann auch hohe **AverageE2ELatency** gegenüber **AverageServerLatency**: Weitere Informationen finden Sie im Eintrag [Nagles-Algorithmus wird nicht Freundlich zu kleine Anfragen](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx). Sie können den Nagle-Algorithmus im Code mithilfe der Klasse **ServicePointManager** im **System.NET-Namespace** deaktivieren. Sie sollten dies tun, bevor Sie zur Tabelle Anrufe oder Warteschlangendienste in der Anwendung, da dies nicht Verbindungen, die bereits geöffnet. Das folgende Beispiel stammt aus der **Application_Start** -Methode in eine Worker-Rolle.

    var storageAccount = CloudStorageAccount.Parse(connStr);
    ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
    tableServicePoint.UseNagleAlgorithm = false;
    ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
    queueServicePoint.UseNagleAlgorithm = false;

Überprüfen Sie verwandte clientseitigen Protokolle zu sehen, wie viele Anfragen, die Clientanwendung senden und prüfen, allgemeine .NET Leistungsengpässe in Ihrem Client wie CPU, .NET Garbagecollection, Netzwerklast oder Speicher. Als Ausgangspunkt für die Problembehandlung bei Clientanwendungen .NET finden Sie unter [Debuggen, Verfolgung und Profiling](http://msdn.microsoft.com/library/7fe0dd2y).

#### <a name="investigating-network-latency-issues"></a>Untersuchen von Netzwerkproblemen Latenz

End-to-End-Wartezeiten im Netzwerk zurückzuführen ist normalerweise durch vorübergehende Zustände. Mithilfe von Tools wie Wireshark oder Microsoft Message Analyzer können Sie beide transiente und permanente Netzwerkprobleme wie Verworfene Pakete überprüfen.

Weitere Informationen zur Verwendung von Wireshark Netzwerkproblemen finden Sie unter "[Anlage 2: mithilfe von Wireshark Netzwerkverkehr]."

Weitere Informationen über Microsoft Message Analyzer Netzwerkproblemen finden Sie unter "[Anlage 3: mithilfe von Microsoft Message Analyzer Netzwerkverkehrs]."

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Metriken niedrige AverageE2ELatency und niedrigen AverageServerLatency, sondern der Client treten Wartezeiten

In diesem Szenario ist die wahrscheinlichste Ursache eine Verzögerung beim Erreichen des Speicherdienst Speicheranfragen. Sie sollten untersuchen, warum die Clientanforderungen nicht durch mit BLOB-Dienst herstellen.

Mögliche Ursache für den Client verzögern Anfragen ist eine begrenzte Anzahl verfügbarer Verbindungen oder Threads gibt.

Sie sollten auch überprüfen, ob der Client führt Startversuch und untersuchen Sie die Ursache, wenn dies der Fall ist. Um festzustellen, ob der Client Startversuch ausführt, können Sie:

- Untersuchen Sie die Protokolle Speicheranalyse. Wenn mehrere Versuche auftreten, sehen Sie mehrere Arbeitsgänge die gleiche Client-ID, aber unterschiedlichen Serveranfrage IDs.
- Untersuchen Sie die Clientprotokolle. Die ausführliche Protokollierung bedeutet, dass ein erneuter Versuch aufgetreten ist.
- Debuggen Sie des Codes, und überprüfen Sie die Eigenschaften der Anforderung zugeordnete **OperationContext** -Objekt. Wenn der Vorgang wiederholt wurde, ist die Eigenschaft **RequestResults** mehrere eindeutige Vorgangs-IDs enthalten. Sie können auch die Start- und Endzeiten für jede Anforderung überprüfen. Weitere Informationen finden Sie im Codebeispiel im Abschnitt [Serverkennung].

Wenn der Client keine Probleme vorliegen, sollten Sie potentielle Netzwerkprobleme wie Paketverlust untersuchen. Tools wie Wireshark oder Microsoft Message Analyzer können Sie Netzwerkprobleme überprüfen.

Weitere Informationen zur Verwendung von Wireshark Netzwerkproblemen finden Sie unter "[Anlage 2: mithilfe von Wireshark Netzwerkverkehr]."

Weitere Informationen über Microsoft Message Analyzer Netzwerkproblemen finden Sie unter "[Anlage 3: mithilfe von Microsoft Message Analyzer Netzwerkverkehrs]."

### <a name="metrics-show-high-AverageServerLatency"></a>Metriken zeigen hohe AverageServerLatency

Speicher protokollieren Protokolle verwenden Sie bei hoher **AverageServerLatency** Blob Download Anfragen gibt wiederholte Abfragen für die gleiche Blob (oder Blobs). Blob hochladen Anfragen, sollten Sie untersuchen, Block Größe Client verwenden (z. B. Blöcke kleiner als 64 KB Gemeinkosten führen kann, wenn die Lesevorgänge auch kleiner als 64 KB Segmenten sind), und mehrere Clients das gleiche BLOB parallel Blöcke hochladen. Überprüfen Sie auch pro Minute Metriken Spitzen in die Anzahl der Anfragen, die in mehr als die pro zweite Skalierbarkeitsziele: Siehe auch "[Metriken zeigen einen Anstieg der PercentTimeoutError]".

Wenn Sie hohe **AverageServerLatency** für Blob Anfragen herunterladen wiederholte Abfragen gibt, die denselben Blob oder Blobs angezeigt werden, dann sollten Sie diese Blobs mit Azure Cache oder Azure Content Delivery Network (CDN) zwischenspeichern. Upload-Anfragen können Sie den Durchsatz verbessern, indem Block größer. Für Abfragen auf Tabellen kann auch clientseitiges Zwischenspeichern für Clients, die Operationen derselben Abfrage, die Daten häufig ändern nicht implementiert.

Höchstwerte **AverageServerLatency** kann auch ein Symptom mangelhaft Tabellen oder Abfragen führen Scanvorgänge oder die Append/vorangestellt Antimusters befolgen. Weitere Informationen finden Sie unter "[Metriken Zunahme PercentThrottlingError anzeigen]".

> [AZURE.NOTE] Finden Sie eine umfassende Performance Prüfliste hier: [Microsoft Azure Storage Performance und Skalierung Prüfliste](storage-performance-checklist.md).

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>Es treten unerwartete bei Zustellung von Nachrichten in einer Warteschlange

Wenn Sie eine Verzögerung zwischen dem Zeitpunkt eine Anwendung fügt eine Nachricht an eine Warteschlange und die Zeit aus der Warteschlange gelesen wird haben, sollten Sie die folgenden Schritte, das Problem zu diagnostizieren nutzen:

- Stellen Sie sicher, dass die Anwendung erfolgreich Nachrichten der Warteschlange hinzugefügt werden. Überprüfen Sie, dass die Anwendung die **AddMessage** -Methode mehrmals vor dem nachfolgenden Vorgang ist. Die Speicher-Clientbibliothek Protokolle zeigen wiederholte Wiederholungsversuche Speichervorgänge.
- Überprüfen Sie keine Uhr Neigung zwischen Worker-Rolle, die die Meldung in der Warteschlange hinzugefügt wird und die workerrolle, die die Nachricht aus der Warteschlange liest, mit dem Anschein entsteht eine Verzögerung Verarbeitung.
- Überprüfen Sie, ob die Worker-Rolle, die die Nachrichten aus der Warteschlange liest fehlschlägt. Wenn ein Warteschlange Client antwortet mit einer Bestätigung **GetMessage** -Methode aufruft, wird die Nachricht unsichtbar in der Warteschlange bis **InvisibilityTimeout** Zeitraum abläuft. In diesem Fall wird die Nachricht zur Verarbeitung wieder.
- Überprüfen Sie, ob die Warteschlangenlänge mit der Zeit wächst. Dies kann auftreten, wenn Sie keinen ausreichenden Arbeitskräfte verfügbar aller Nachrichten, die andere Arbeitskräfte in der Warteschlange platziert. Sie sollten auch überprüfen, Metriken auf Löschen fordert nicht und der Zähler Dequeue Nachrichten hinweisen wiederholt fehlgeschlagene Versuche zum Löschen der Nachricht.
- Untersuchen Sie die Protokolle Speicher Protokollierung für Warteschlangenoperationen, die höher als die erwarteten Werte für **E2ELatency** und **ServerLatency** über einen längeren Zeitraum als üblich.


### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Metriken zeigen eine PercentThrottlingError

Übersteigen die Skalierbarkeitsziele eines auftreten Drosselung Fehler. Der Speicherdienst wird keine einzelnen Client oder Mieter auf Kosten anderer Dienst verwenden kann. Weitere Informationen finden Sie ausführliche Zielvorgaben Skalierbarkeit für Speicherkonten und Leistungsziele für Partitionen in Speicherkonten [Azure Storage Skalierbarkeits- und Performance-Ziele](storage-scalability-targets.md) .

**PercentThrottlingError** Metrik eine Erhöhung in Prozent der Anfragen anzuzeigen, die mit einer Drosselung Fehler fehlschlagen, müssen Sie eine der beiden Szenarien untersuchen:

- [Vorübergehende Erhöhung PercentThrottlingError]
- [Permanente Zunahme PercentThrottlingError Fehler]

Zunahme **PercentThrottlingError** kommt häufig eine Zunahme von Speicheranfragen gleichzeitig, oder wird Sie zunächst laden Testen der Anwendung. Dies kann auch auf dem Client als "503 Server ausgelastet" oder "500 Operation Timeout" HTTP-Statusnachrichten Speichervorgänge manifestieren.

#### <a name="transient-increase-in-PercentThrottlingError"></a>Vorübergehende Erhöhung PercentThrottlingError

Wenn Spitzen im Wert **PercentThrottlingError** , die mit Zeiten hoher Aktivität für die Anwendung angezeigt, sollten Sie eine exponentielle (nicht lineare) Backoff-Strategie für Wiederholungsversuche im Client implementieren: Dies wird sofort die Partition reduzieren und die Ihr um Spitzen im Datenverkehr zu glätten. Weitere Informationen zu Maßnahmen wiederholen mit Speicher-Clientbibliothek finden Sie im [Microsoft.WindowsAzure.Storage.RetryPolicies-Namespace](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx).

> [AZURE.NOTE] Sie sehen auch Spitzen im Wert des **PercentThrottlingError** mit hoher Aktivität für die Anwendung nicht übereinstimmen: die wahrscheinlichste Ursache ist der Speicherdienst Partitionen Lastenausgleich zu verschieben.

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Permanente Zunahme PercentThrottlingError Fehler

Wenn man einen konstant hohen Wert für **PercentThrottlingError** folgende dauerhafte Erhöhung der Transaktionsmengen oder bei Ihrem ersten Laden in der Anwendung überprüft, müssen Sie wie die Anwendung Speicher Partitionen verwendet und ob die Skalierbarkeitsziele für ein Speicherkonto nähert. Beispielsweise wenn Drosselung Fehler auf eine Warteschlange (die als eine einzige Partition gezählt), sollten dann Sie zusätzliche Warteschlangen sich Transaktionen über mehrere Partitionen verwenden. Wenn Drosselung Fehler in einer Tabelle angezeigt werden, müssen Sie ein anderes Partitionierungsschema sich Transaktionen über mehrere Partitionen mit zahlreichen Partitionswerte verwenden. Eine häufige Ursache dieses Problems ist die vorangestellten/append Antimusters, wählen Sie das Datum als Partitionsschlüssel und alle Daten an einem bestimmten Tag in einer Partition geschrieben: ausgelastet ist, kann dadurch ein Engpass schreiben. Sie sollten verwenden Sie einen anderen Partitionierung Entwurf oder auswerten, ob BLOB-Speicher besser sein kann. Auch prüfen, ob die Einschränkung durch Spitzen in Ihren Datenverkehr stattfindet und Arten der Glättung des Musters Anfragen überprüfen.

Transaktionen über mehrere Partitionen verteilen, müssen Sie weiterhin über die Skalierungseigenschaften für das Speicherkonto festgelegt sein. Wenn Sie zehn Warteschlangen jedes maximal 2.000 1KB-Nachrichten pro Sekunde verarbeiten, werden Sie z. B. bei insgesamt 20.000 Nachrichten pro Sekunde für das Speicherkonto sein. Benötigen Sie mehr als 20.000 Einheiten pro Sekunde verarbeiten, sollten Sie mehrere Speicherkonten verwenden. Sie sollten auch berücksichtigen, dass die Größe Ihrer Anfragen und Entitäten sich auf bei der Speicherdienst Clients Steuerung: haben Sie mehr Anfragen und Entitäten, Sie können gedrosselt werden schneller.

Ineffizienten Entwurf kann auch die Skalierungseigenschaften Tabellenpartitionen erreicht. Beispielsweise wird eine Abfrage mit einem Filter, der wählt nur ein Prozent der Entitäten in einer Partition aber, die alle Entitäten in einer Partition überprüft, jede Entität zugreifen müssen. Jede Entität lesen zählen für die Gesamtzahl der Transaktionen in dieser Partition; Sie können daher Skalierbarkeitsziele erreichen.

> [AZURE.NOTE] Leistungstests sollten ineffizient Abfrage Designs in der Anwendung angezeigt werden.

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Metriken zeigen eine PercentTimeoutError

Die Metriken zeigen eine **PercentTimeoutError** eines Speicher-Services. Zur gleichen Zeit erhält der Kunde eine große Anzahl von "500 Operation Timeout" HTTP-Nachrichten von Speicherbetrieb.

> [AZURE.NOTE] Sie sehen Timeoutfehler vorübergehend als der Speicherdienst Salden Anfragen laden, indem Sie eine Partition auf einen neuen Server verschieben.

**PercentTimeoutError** -Metrik ist die Zusammenfassung die folgenden Metriken: **ClientTimeoutError**, **AnonymousClientTimeoutError**, **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**und **SASServerTimeoutError**.

Das Zeitlimit des Servers werden durch Fehler auf dem Server verursacht. Client-Timeouts kommen, eine Operation auf dem Server vom Client angegebene Zeitlimit überschritten hat. Beispielsweise kann ein Client die Clientbibliothek Speicher einen Timeout für einen Vorgang fest mithilfe der **ServerTimeout** -Eigenschaft der **QueueRequestOptions** -Klasse

Servertimeout Problem mit Speicher-Service, die weitere Untersuchung erfordert. Können Sie Metriken auf schlagen die Skalierungseigenschaften für den Dienst und Spitzen im Datenverkehr identifizieren, die dieses Problem verursachen. Wenn das Problem nur gelegentlich auftritt, möglicherweise aufgrund des Lastenausgleichs Tätigkeit im Dienst. Wenn das Problem ist und nicht von der Anwendung auf die Skalierungseigenschaften des Dienstes verursacht, müssen Sie ein supportproblem auslösen. Client-Timeouts müssen Sie entscheiden, ob das Timeout auf einen geeigneten Wert auf dem Client und ändern Sie entweder den Timeoutwert im Client festzulegen oder Überprüfen Sie die Leistung von Operationen in der Speicherdienst beispielsweise Verbesserung Optimieren Ihrer Tabellenabfragen oder Reduzierung der Größe der Nachrichten.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Metriken zeigen eine PercentNetworkError

Die Metriken zeigen eine **PercentNetworkError** eines Speicher-Services. **PercentNetworkError** -Metrik ist die Zusammenfassung die folgenden Metriken: **NetworkError**, **AnonymousNetworkError**und **SASNetworkError**. Diese treten der Speicherdienst einen Fehler erkennt, wenn der Client ein.

Die häufigste Ursache dieses Fehlers ist ein Client trennen in der Speicherdienst Ablauf ein Timeouts. Überprüfen Sie den Code in Ihrem Client zu verstehen, warum und wann der Client aus der Speicherdienst getrennt wird. Wireshark, Microsoft Message Analyzer oder Tcping können auch um vom Client Netzwerkverbindungsprobleme zu untersuchen. Diese Tools sind in den [Anhängen]beschrieben.

### <a name="the-client-is-receiving-403-messages"></a>Der Client empfängt Nachrichten HTTP 403 (Verboten)

Die Clientanwendung HTTP 403 (Verboten) Fehler auslöst, ist wahrscheinlich, dass der Client eine abgelaufene Shared Access Signatur (SAS) sendet es ein (auch andere möglichen Ursachen Clock skew ungültigen Schlüssel und leeren Header enthalten). Abgelaufener Schlüssel SAS verursacht, sehen Sie keine Einträge in der serverseitige Speicher Protokollierung Daten. Die folgende Tabelle zeigt ein Beispiel aus dem clientseitigen Protokoll generiert von Speicher-Clientbibliothek, die veranschaulicht dieses Problem auftritt:

Quelle|Ausführlichkeit|Ausführlichkeit|Client-Anfrage-id|Operation text
---|---|---|---|---
Microsoft.WindowsAzure.Storage|Informationen|3|85d077ab-...|Inbetriebnahme mit primären pro Standort-Modus PrimaryOnly.
Microsoft.WindowsAzure.Storage|Informationen|3|85d077ab-...|Synchrone Anforderung https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14 gestartet&amp;sr = c&amp;Si MeineRichtlinie =&amp;Sig = OFnd4Rd7z01fIvh 2BmcR6zbudIH2F5Ikm % 2FyhNYZEmJNQ % 3D&amp;API-Version = 2014-02-14.
Microsoft.WindowsAzure.Storage|Informationen|3|85d077ab-...|Warten auf Antwort.
Microsoft.WindowsAzure.Storage|Warnung|2|85d077ab-...|Ausnahme beim Warten auf Antwort: der Remoteserver hat einen Fehler zurückgegeben: (403) Forbidden...
Microsoft.WindowsAzure.Storage|Informationen|3|85d077ab-...|Antwort erhalten. Statuscode 403 Serviceanfrage-ID = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, Content-MD5 = = ETag =.
Microsoft.WindowsAzure.Storage|Warnung|2|85d077ab-...|Ausnahme während des Vorgangs: der Remoteserver hat einen Fehler zurückgegeben: (403) Forbidden...
Microsoft.WindowsAzure.Storage|Informationen|3 |85d077ab-...|Geprüft, ob der Vorgang wiederholt werden soll. Anzahl der Wiederholungsversuche = 0, HTTP-Statuscode 403, Ausnahme = = den Remoteserver hat einen Fehler zurückgegeben: (403) Forbidden...
Microsoft.WindowsAzure.Storage|Informationen|3|85d077ab-...|Primäre, abhängig vom Standort-Modus wurde die nächste Position fest.
Microsoft.WindowsAzure.Storage|Fehler|1|85d077ab-...|Wiederholungsrichtlinie lässt sich nicht für eine Wiederholung. Mit dem remote-Server hat einen Fehler zurückgegeben: (403) Forbidden.

In diesem Szenario sollten Sie untersuchen, warum das SAS-Token abläuft, bevor der Client das Token an den Server sendet:

- Normalerweise sollten Sie keine Startzeit festlegen beim SAS für einen Client sofort erstellen. Wenn kleine Uhr Unterschiede bestehen zwischen dem Host mit der aktuellen Zeit und dem Speicherdienst SAS erzeugen, ist es möglich, dass der Speicherdienst SAS empfangen, die noch nicht gültig ist.
- Sie sollten nicht auf einem SAS kurzer Ablaufzeit festlegen. Uhr Unterschiede zwischen SAS und der Speicherdienst generiert Host führt wieder zu SAS offensichtlich ablaufen früher als erwartet.
- Versionsparameters die SAS-Taste ist (z. B. **PA 2015-04-05 =**) entspricht die Version der Clientbibliothek Speicher Sie verwenden? Wir empfehlen immer die neueste Version der [Speicher-Clientbibliothek](https://www.nuget.org/packages/WindowsAzure.Storage/)verwenden.
- Wenn die Zugriffstasten Speicher Regenerieren können alle vorhandenen SAS-Token ungültig werden. Dieses eventuell Problem SAS-Token mit einer langen Ablaufzeit für Clientanwendungen Cache generieren.

Wenn Sie die Speicher-Clientbibliothek SAS-Token zu verwenden, ist einfach ein gültiges Token. Wenn Sie die REST-API und die SAS-Token von hand erstellen sollten Sie das Thema [Delegieren Zugriff mit einem SAS](http://msdn.microsoft.com/library/azure/ee395415.aspx)sorgfältig lesen.

### <a name="the-client-is-receiving-404-messages"></a>Der Client empfängt Nachrichten HTTP 404 (nicht gefunden)
Wenn die Clientanwendung eine HTTP 404 (nicht gefunden) Nachricht vom Server empfängt, bedeutet dies, dass das Objekt, das den Versuch, (z. B. eine Entität, Tisch, Blob, Container oder Warteschlange) nicht in der Speicherdienst vorhanden ist. Es gibt zahlreiche Gründe, wie:

- [Der Client oder ein anderer Prozess bereits das Objekt gelöscht]
- [Eine Autorisierung Problem Shared Access Signatur (SAS)]
- [Clientseitige JavaScript-Code hat keinen Zugriff auf das Objekt]
- [Netzwerkfehler]

#### <a name="client-previously-deleted-the-object"></a>Der Client oder ein anderer Prozess bereits das Objekt gelöscht
In Szenarien, in denen der Client versucht, lesen, aktualisieren und Löschen von Daten in einem Speicherdienst, lässt normalerweise ein vorherigen Vorgangs in die serverseitige Protokolle identifizieren, das das betreffende Objekt aus der Speicherdienst gelöscht. Die Protokolldaten zeigt oft, dass ein anderer Benutzer oder Prozess das Objekt gelöscht. Im serverseitigen Speicher Protokollierung Protokoll der Typ Operation und angeforderte Objektschlüssel Spalten anzeigen wenn ein Client ein Objekt gelöscht.

In dem Szenario, in dem ein Client versucht, ein Objekt einzufügen, kann es nicht offensichtlich, warum dadurch auf einen HTTP 404 (nicht gefunden), vorausgesetzt, dass der Client ein neues Objekt erstellen. Jedoch muss, wenn der Client erstellt einen Blob zu Blob-Container können muss, wenn der Client eine Nachricht zu einer Warteschlange können muss erstellt und der Client eine Zeile hinzufügen es Tabelle finden können.

Können Sie Client-Serverprotokoll aus Speicher-Clientbibliothek verstehen detailliertere Client sendet Anfragen an der Speicherdienst.

Das folgende clientseitige Protokoll generiert von Speicher-Clientbibliothek veranschaulicht das Problem, wenn der Client den Container für das Blob nicht finden es erstellt. Dieses Protokoll enthält Details zu folgenden Speichervorgänge:

Anfrage-ID|Vorgang
---|---
07b26a5d-...|**DeleteIfExists** -Methode, um BLOB-Container löschen. Beachten Sie, dass dieser Vorgang eine **HEAD** -Anforderung überprüft, ob der Container.
e2d06d78...|**CreateIfNotExists** -Methode den BLOB-Container erstellen. Beachten Sie, dass dieser Vorgang eine **HEAD** -Anforderung, die das Vorhandensein des Containers überprüft. **Kopf** gibt 404 Meldung jedoch weiterhin.
de8b1c3c-...|**UploadFromStream** -Methode, um das Blob erstellen. **PUT** -Anforderung schlägt mit 404-Nachricht

Einträge:

Anfrage-ID |  Operation Text
---|---
07b26a5d-...|Synchrone Anforderung zum https://domemaildist.blob.core.windows.net/azuremmblobcontainer wird gestartet.
07b26a5d-...|StringToSign = HEAD...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 Jun 2014 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-...|Warten auf Antwort.
07b26a5d-... | Antwort erhalten. Statuscode = 200 Serviceanfrage-ID = eeead849... Content-MD5 = ETag = &quot;0x8D14D2DC63D059B&quot;.
07b26a5d-... | Antwort-Header wurden, verarbeitet der Operation fortzusetzen.
07b26a5d-... | Antworttext herunterladen.
07b26a5d-... | Der Vorgang wurde erfolgreich abgeschlossen.
07b26a5d-... | Synchrone Anforderung zum https://domemaildist.blob.core.windows.net/azuremmblobcontainer wird gestartet.
07b26a5d-... | StringToSign = DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-... | Warten auf Antwort.
07b26a5d-... | Antwort erhalten. Statuscode = 202 Serviceanfrage-ID = 6ab2a4cf..., Content-MD5 = ETag =.
07b26a5d-... | Antwort-Header wurden, verarbeitet der Operation fortzusetzen.
07b26a5d-... | Antworttext herunterladen.
07b26a5d-... | Der Vorgang wurde erfolgreich abgeschlossen.
e2d06d78-... | Asynchrone Anforderung https://domemaildist.blob.core.windows.net/azuremmblobcontainer gestartet.</td>
e2d06d78-... | StringToSign = HEAD...x-ms-client-request-id:e2d06d78-...x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-...| Warten auf Antwort.
de8b1c3c-... | Synchrone Anforderung zum https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt wird gestartet.
de8b1c3c-... |  StringToSign = speichern... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-ms-BLOB-Type:BlockBlob.x-ms-Client-Request-ID:de8b1c3c-...x-ms-Date:tue 03-Jun-2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... | Vorbereiten der Daten schreiben.
e2d06d78-... | Ausnahme beim Warten auf Antwort: der Remoteserver hat einen Fehler zurückgegeben: (404) nicht gefunden...
e2d06d78-... | Antwort erhalten. Statuscode 404 Serviceanfrage-ID = = 353ae3bc..., Content-MD5 = ETag =.
e2d06d78-... | Antwort-Header wurden, verarbeitet der Operation fortzusetzen.
e2d06d78-... | Antworttext herunterladen.
e2d06d78-... | Der Vorgang wurde erfolgreich abgeschlossen.
e2d06d78-... | Asynchrone Anforderung https://domemaildist.blob.core.windows.net/azuremmblobcontainer gestartet.
e2d06d78-...|StringToSign = speichern... 0...x-ms-Client-Request-ID:e2d06d78-...x-ms-Date:tue 03-Jun-2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-... | Warten auf Antwort.
de8b1c3c-... | Schreiben von Daten.
de8b1c3c-... | Warten auf Antwort.
e2d06d78-... | Ausnahme beim Warten auf Antwort: der Remoteserver hat einen Fehler zurückgegeben: (409) Conflict...
e2d06d78-... | Antwort erhalten. Statuscode = 409 Serviceanfrage-ID = c27da20e..., Content-MD5 = ETag =.
e2d06d78-... | Fehler-Antworttext herunterladen.
de8b1c3c-... | Ausnahme beim Warten auf Antwort: der Remoteserver hat einen Fehler zurückgegeben: (404) nicht gefunden...
de8b1c3c-... | Antwort erhalten. Statuscode 404 Serviceanfrage-ID = = 0eaeab3e..., Content-MD5 = ETag =.
de8b1c3c-...| Ausnahme während des Vorgangs: der Remoteserver hat einen Fehler zurückgegeben: (404) nicht gefunden...
de8b1c3c-... | Wiederholungsrichtlinie lässt sich nicht für eine Wiederholung. Mit dem remote-Server hat einen Fehler zurückgegeben: (404) nicht gefunden...
e2d06d78-... | Wiederholungsrichtlinie lässt sich nicht für eine Wiederholung. Mit dem remote-Server hat einen Fehler zurückgegeben: (409) Conflict...

In diesem Beispiel zeigt das Protokoll, der Client Anfragen aus der **CreateIfNotExists** -Methode (Anforderung Id e2d06d78...) mit der **UploadFromStream** -Methode (de8b1c3c...); interleaving Dies geschieht, weil die Clientanwendung diese Methoden asynchron aufrufen. Ändern Sie den asynchronen Code auf dem Client um sicherzustellen, dass der Container erstellt wird, bevor Sie Daten in ein Blob im Container hochladen. Im Idealfall sollten Sie alle Container im Voraus erstellen.

#### <a name="SAS-authorization-issue"></a>Eine Autorisierung Problem Shared Access Signatur (SAS)

Wenn die Clientanwendung versucht, einen SAS-Schlüssel verwenden, die nicht die erforderlichen Berechtigungen für den Vorgang, zurück der Speicherdienst Fehlermeldung HTTP 404 (nicht gefunden) an den Client. Zur gleichen Zeit sehen Sie auch einen NULL Wert für **SASAuthorizationError** in der Metriken.

Die folgende Tabelle zeigt eine Beispiel serverseitige Nachricht von Protokolldatei Speicher protokollieren:

<table>
  <tr>
    <td>Startzeit der Anfrage</td>
    <td>2014-05-30T06:17:48.4473697Z</td>
  </tr>
  <tr>
    <td>Vorgangstyp</td>
    <td>GetBlobProperties</td>
  </tr>
  <tr>
    <td>Anforderung: status</td>
    <td>SASAuthorizationError</td>
  </tr>
  <tr>
    <td>HTTP-Statuscode</td>
    <td>404</td>
  </tr>
  <tr>
    <td>Authentifizierungstyp</td>
    <td>SAS</td>
  </tr>
  <tr>
    <td>Diensttyp</td>
    <td>BLOB</td>
  </tr>
  <tr>
    <td>Request-URL</td>
    <td>
    https://domemaildist.BLOB.Core.Windows.NET/azureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;Amp; sr = c&amp;Amp; Si MeineRichtlinie =&amp;Amp; Sig = XXXXX&amp;Amp; API-Version 2014-02-14 =&amp;Amp;</td>
  </tr>
  <tr>
    <td>Anfrage-Id-header</td>
    <td>a1f348d5-8032-4912-93ef-b393e5252a3b</td>
  </tr>
  <tr>
    <td>Client-Anfrage-ID</td>
    <td>2d064953-8436-4ee0-aa0c-65cb874f7929</td>
  </tr>
</table>

Sie sollten untersuchen, warum die Clientanwendung versucht, einen Vorgang auszuführen, dem ihm keine Berechtigungen gewährt wurden.

#### <a name="JavaScript-code-does-not-have-permission"></a>Clientseitige JavaScript-Code hat keinen Zugriff auf das Objekt

Wenn Sie einen JavaScript-Client und der Speicherdienst HTTP 404 Nachrichten gibt, prüfen Sie die folgenden JavaScript-Fehler im Browser.

    SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
    SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.

> [AZURE.NOTE] Sie können F12 Developer Tools in Internet Explorer verfolgen Sie clientseitige JavaScript Problemen zwischen dem Browser und dem Speicherdienst ausgetauschten Nachrichten.

Diese Fehler treten da Webbrowser [dieselbe Herkunft Richtlinie](http://www.w3.org/Security/wiki/Same_Origin_Policy) sicherheitsbedingten Einschränkung implementiert, die verhindert, dass eine Webseite Aufrufen einer API in einer anderen Domäne aus der Domäne die Seite kommt.

Um das JavaScript Problem zu umgehen, können Sie Cross Ursprung Ressource freigeben (CORS) für Storage Service konfigurieren, auf die der Client zugreifen. Weitere Informationen finden Sie unter [Unterstützung für Azure-Speicherdienste Cross-Ursprung Ressource freigeben (CORS)](http://msdn.microsoft.com/library/azure/dn535601.aspx).

Das folgende Codebeispiel veranschaulicht das Konfigurieren des BLOB-Diensts in der Domäne Contoso ausgeführtes JavaScript einen Blob im BLOB-Speicherdienst zugreifen können:

    CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
    // Set the service properties.
    ServiceProperties sp = client.GetServiceProperties();
    sp.DefaultServiceVersion = "2013-08-15";
    CorsRule cr = new CorsRule();
    cr.AllowedHeaders.Add("*");
    cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
    cr.AllowedOrigins.Add("http://www.contoso.com");
    cr.ExposedHeaders.Add("x-ms-*");
    cr.MaxAgeInSeconds = 5;
    sp.Cors.CorsRules.Clear();
    sp.Cors.CorsRules.Add(cr);
    client.SetServiceProperties(sp);

#### <a name="network-failure"></a>Netzwerkfehler

Verlorene Pakete führt unter Umständen zu der Speicherdienst HTTP 404 Nachrichten an den Client zurückgegeben. Z. B. wenn die Clientanwendung eine Entität aus der Tabelle Service löschen Sie finden Sie unter Client auslösen Speicher Ausnahme reporting eine "HTTP 404 (nicht gefunden)" Statusnachricht aus dem Dienst. Wenn Sie die Tabelle in der tabellenspeicherdienst untersuchen, sehen Sie, dass der Dienst die Entität gelöscht haben, wie.

Die Ausnahmedetails im Client enthalten die Id (7e84f12d...) Tabelle Service für die Anforderung zugewiesen: mithilfe dieser Informationen die Details der Anforderung in serverseitigen Speicherprotokollen finden in der **Anforderungsheader-Id** -Spalte in der Protokolldatei. Die Metriken können Sie ermitteln, wenn Fehler wie diese auftreten, und suchen Sie basierten auf der Uhrzeit, die Metriken Fehlerursache aufgezeichnete, Protokolldateien. Dieser Eintrag zeigt, dass mit einer Statusnachricht "HTTP (404) Client Fehler" der Löschvorgang fehlgeschlagen ist. Der gleichen Protokolleintrag auch die Id vom Client in der **Anforderung-Id** -Spalte (813ea74f...) generiert.

Das serverseitige enthält auch einen anderen Eintrag mit demselben **Client-Anfrage-ID-** Wert (813ea74f...) für Löschvorgangs für dieselbe Entität und vom selben Client. Diese Löschvorgangs wurden kurz bevor die fehlgeschlagene Anforderung löschen.

Die wahrscheinlichste Ursache dieses Szenarios werden Client eine Löschanfrage für die Entität Tabelle Service gesendet war erfolgreich, aber erhielten eine Bestätigung vom Server (z. B. aufgrund eines vorübergehenden Netzwerkproblems). Der Client dann automatisch wiederholt den Vorgang (mit derselben **Anforderung-Id**), und diese Wiederholung ist fehlgeschlagen, da das Element bereits gelöscht wurde.

Wenn dieses Problem häufig auftritt, sollten Sie untersuchen, warum der Client nicht den Dienst Empfangsbestätigungen erhalten. Wenn das Problem nur gelegentlich auftritt, sollte, die "HTTP (404) Not Found" Fehler und Client anmelden aber Client zum Fortsetzen.

### <a name="the-client-is-receiving-409-messages"></a>Der Client ist HTTP 409 (Konflikt) Nachrichten empfängt.

Die folgende Tabelle zeigt einen Auszug aus der serverseitigen Protokoll zwei Client-Operationen: **DeleteIfExists** und unmittelbar durch **CreateIfNotExists** mit demselben Namen der BLOB-Container. Notiz, die Ergebnisse jeder Client zwei Anfragen den Server zuerst an eine **GetContainerProperties** -Anforderung zu überprüfen, ob der Container vorhanden ist, gefolgt von **DeleteContainer** oder **CreateContainer** Anforderung.

Zeitstempel|Vorgang|Ergebnis|Containername|Client-Anfrage-id
---|---|---|---|---
05:10:13.7167225|GetContainerProperties|200|mmcont|c9f52c89-...
05:10:13.8167325|DeleteContainer|202|mmcont|c9f52c89-...
05:10:13.8987407|GetContainerProperties|404|mmcont|bc881924-...
05:10:14.2147723|CreateContainer|409|mmcont|bc881924-...

Der Code in der Clientanwendung löscht und BLOB-Container mit demselben Namen sofort neu: (Client anfordern) ID-bc881924- **CreateIfNotExists** -Methode schlägt unter Umständen fehl mit Fehler HTTP 409 (Konflikt). Wenn ein Client löscht Blob-Container, Tabellen oder Warteschlangen gibt es kurz vor dem Namen wird wieder verfügbar.

Die Clientanwendung sollten eindeutige Containernamen verwenden, wenn neue Container erstellt wird, wenn das Muster löschen/neu ist.

### <a name="metrics-show-low-percent-success"></a>Metriken zeigen niedrige PercentSuccess oder Analytics Protokolleinträge arbeiten mit Buchungsstatus der ClientOtherErrors

**PercentSuccess** Metrik erfasst den Prozentsatz der Vorgänge erfolgreich basierend auf ihre HTTP-Statuscode waren. Operationen mit Statuscodes 2XX zählen als erfolgreich mit Statuscodes 3XX 4XX und 5XX Bereiche als erfolglos gezählt und **PercentSucess** metrische Wert. Diese Vorgänge werden in den Protokolldateien serverseitige Speicher Buchungsstatus der **ClientOtherErrors**erfasst.

Es ist wichtig zu beachten, dass diese Vorgänge erfolgreich abgeschlossen wurden und daher haben keinen Einfluss auf andere Kriterien wie. Einige Beispiele für Vorgänge erfolgreich ausgeführt, aber nicht erfolgreich HTTP-Statuscodes führen:
- **ResourceNotFound** (Nicht gefunden 404), z. B. von einer GET-Anforderung an ein Blob, das nicht vorhanden ist.
- **ResouceAlreadyExists** (Konflikt 409), z. B. von einer **Wertereihe** Vorgang, die Ressource bereits vorhanden ist.
- **ConditionNotMet** (Nicht verändert 304), z. B. von einer bedingten Operation wie wenn ein Client sendet einen **ETag** -Wert und einen HTTP- **If-None-Match** -Header auf ein Bild nur anfordern, wenn es seit dem letzten Vorgang aktualisiert wurde.

Finden Sie eine Liste der allgemeinen REST API-Fehlercodes, die Speicher-Services auf der Seite [Allgemeine REST API Fehlercodes](http://msdn.microsoft.com/library/azure/dd179357.aspx)zurück.

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Kapazität Metriken zeigen ein unerwarteter Speicher-Auslastung


Wenn plötzlich angezeigt wird, können unerwartete Änderung Kapazitätsauslastung in das Speicherkonto Sie Gründe untersuchen anhand erste metrischen Verfügbarkeit; beispielsweise eine Zunahme der Anzahl der fehlgeschlagenen Anfragen erhöht BLOB-Speicher führen als Anwendung bestimmte Bereinigungsvorgänge, die Sie erwarten zu sein Speicherplatz nicht funktioniert wie erwartet (zum Beispiel weil der Speicherplatz zum SAS-Token abgelaufen) verwendeten löschen.

### <a name="you-are-experiencing-unexpected-reboots"></a>Unerwartete Neustarts von Azure virtuellen Computern auftreten, die eine große Anzahl von angeschlossenen Festplatten

Ein Azure Virtual Machine (VM) eine große Anzahl von angeschlossenen Festplatten hat, die in demselben Speicherkonto konnte Skalierbarkeitsziele für eine einzelne Speicherkonto verursacht die VM nicht überschreiten. Überprüfen Sie die Minuten Metriken für das Speicherkonto (**TotalRequests**/**TotalIngress**/**TotalEgress**) für Spitzen, die für ein Speicherkonto Skalierbarkeit übertreffen. Siehe Abschnitt "[Metriken Zunahme PercentThrottlingError anzeigen]" zu ermitteln, ob die Einschränkung auf das Speicherkonto aufgetreten ist.

Im Allgemeinen entspricht jedes einzelnen Input oder Output-Operation auf einer virtuellen Festplatte mit einem virtuellen Computer **Seite abrufen** oder **Setzen** Operationen auf der zugrunde liegenden Seitenblob. Daher können Sie die geschätzte IOPS für Ihre Umgebung wie viele Festplatten optimieren Sie in ein einzelnes Speicherkonto das spezifische Verhalten der Anwendung auf haben können. Wir empfehlen nicht mehr als 40 Laufwerke unter ein einzelnes Speicherkonto. Einzelheiten finden Sie unter [Azure Storage Skalierbarkeits- und Performance-Ziele](storage-scalability-targets.md) der aktuellen Skalierbarkeitsziele für Speicherkonten, insbesondere insgesamt Anforderungsrate und Gesamtbandbreite für den Typ des verwendeten Speicherkonto.
Wenn Sie Skalierbarkeitsziele für das Speicherkonto überschreiten, sollten Sie mehrere verschiedene Konten der Aktivität in jedem einzelnen Konto die VHDs versehen.

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Das Problem entsteht Speicheremulator für Entwicklungs- oder Testzwecke verwenden

Normalerweise Speicheremulator während der Entwicklung verwenden und Testen eines Kontos Azure-Speicher umgehen. Probleme, die auftreten können, wenn Sie Speicheremulator verwenden werden:

- [Funktion "X" funktioniert in der Speicheremulator nicht]
- [Fehlermeldung "der Wert für einen HTTP-Header ist nicht im richtigen Format" Verwendung des Speicheremulators]
- [Ausführen des Speicheremulators sind Administratorrechte erforderlich]

#### <a name="feature-X-is-not-working"></a>Funktion "X" funktioniert in der Speicheremulator nicht

Der Speicheremulator unterstützt nicht alle Features von Azure-Speicherdienste wie die Datei. Weitere Informationen finden Sie unter [Verwenden der Speicheremulator Azure für Entwicklung und Testing](storage-use-emulator.md).

Verwenden Sie für diese Funktionen Speicheremulator nicht unterstützt den Azure-Speicherdienst in der Cloud.

#### <a name="error-HTTP-header-not-correct-format"></a>Fehlermeldung "der Wert für einen HTTP-Header ist nicht im richtigen Format" Verwendung des Speicheremulators

Sie testen die Anwendung, der Speicher-Clientbibliothek mit lokalen Speicheremulator und Methodenaufrufe wie **CreateIfNotExists** schlagen mit der Fehlermeldung "der Wert für einen HTTP-Header nicht im richtigen Format ist." Dies bedeutet, dass die Version der Speicheremulator verwendeten verwendeten Version der Speicher-Clientbibliothek nicht unterstützt. Der Speicher-Clientbibliothek hinzugefügt alle Anfragen macht Header **X-ms-Version** . Der Speicheremulator Wert im Header **X-ms-Version** nicht erkannt, wird die Anforderung abgelehnt.

Speicher-Library-Client-Protokolle können er sendet den Wert des **X-ms-Version-Header** anzuzeigen. Sie sehen auch den Wert des **X-ms-Version-Header** verwenden Sie Fiddler Anfragen in der Clientanwendung verfolgen.

Dieses Szenario tritt normalerweise auf, wenn Sie installieren und die neueste Version der Clientbibliothek Speicher ohne Aktualisieren des Speicheremulators. Sie sollten die neueste Version der Speicheremulator oder Emulator für Entwicklungs- und Testzwecke Cloud-Speicher anstelle.

#### <a name="storage-emulator-requires-administrative-privileges"></a>Ausführen des Speicheremulators sind Administratorrechte erforderlich

Beim Ausführen des Speicheremulators werden Sie Administratoranmeldeinformationen aufgefordert. Dies tritt nur bei Speicheremulator zum ersten Mal initialisiert werden. Nach Initialisierung Speicheremulator benötigen Sie keine Administratorrechte erneut ausführen.

Weitere Informationen finden Sie unter [Verwenden der Speicheremulator Azure für Entwicklung und Testing](storage-use-emulator.md). Beachten Sie, dass Sie auch Speicheremulator in Visual Studio initialisieren können auch Administratorrechte benötigen.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>Probleme bei der Installation von Azure SDK für .NET stoßen

Beim Installieren des SDK fehlschlägt Versuch, es Speicheremulator auf dem lokalen Computer installieren. Das Installationsprotokoll enthält eine der folgenden Fehlermeldungen:

- CAQuietExec: Fehler: Zugriff auf SQL-Instanz kann nicht
- CAQuietExec: Fehler: keine Datenbank erstellen

Die Ursache ist ein Problem bei der Installation von LocalDB. Standardmäßig verwendet der Speicheremulator LocalDB Daten beibehalten, wenn der Azure-Speicherdienste simuliert. Mit folgenden Befehlen in einem Eingabeaufforderungsfenster vor dem Installieren des SDK können Sie Ihre LocalDB Instanz zurücksetzen.

    sqllocaldb stop v11.0
    sqllocaldb delete v11.0
    delete %USERPROFILE%\WAStorageEmulatorDb3*.*
    sqllocaldb create v11.0

Der Befehl **Löschen** entfernt alle alten Datenbankdateien aus früheren Installationen der Speicheremulator

### <a name="you-have-a-different-issue-with-a-storage-service"></a>Sie haben ein anderes Problem mit einem Storage-Dienst

Wenn das Problem nicht die vorherigen Abschnitte zur Problembehandlung enthalten, den Sie eines mit sollte Diagnose und Problembehandlung das Problem wie folgt vorgehen festlegen.

- Überprüfen Sie die Metriken auf Änderungen von der Grundlinie erwartet. Die Metriken können möglicherweise bestimmen, ob das Problem vorübergehend oder dauerhaft die Speichervorgänge das Problem auswirkt.
- Metriken Informationen können suchen die serverseitige Daten für ausführlichere Informationen zu Fehlern, die auftreten können. Diese Information kann Ihnen zu beheben das Problem.
- Wenn die Informationen in die serverseitige Protokolle erfolgreich Problembehandlung nicht ausreicht, können Speicher-Clientbibliothek clientseitigen Protokolle Sie um das Verhalten Ihrer Clientanwendung und Tools Fiddler Wireshark und Microsoft Message Analyzer Netzwerk zu untersuchen.

Weitere Informationen über Fiddler finden Sie unter "[Anlage 1: mithilfe von Fiddler HTTP- und HTTPS-Datenverkehr]."

Weitere Informationen zur Verwendung von Wireshark finden Sie unter "[Anlage 2: mithilfe von Wireshark Netzwerkverkehr]."

Weitere Informationen über Microsoft Message Analyzer finden Sie unter "[Anlage 3: mithilfe von Microsoft Message Analyzer Netzwerkverkehrs]."

## <a name="appendices"></a>Anhänge

Die Anhänge beschreiben verschiedene Tools nützlich sein können bei der Diagnose und Fehlerbehebung bei Azure Storage (und andere Dienste). Diese Tools sind nicht Bestandteil der Azure-Speicher und einige Drittanbieter-Produkte. Als solche in den Anhängen aufgeführten Tools unterliegen keine Support-Vertrag mit Microsoft Azure oder Azure Storage haben und daher im Rahmen einer Evaluierung prüfen die Lizenz- und Optionen die Anbieter dieser Tools.

### <a name="appendix-1"></a>Anhang 1: Mithilfe von Fiddler HTTP- und HTTPS-Datenverkehr

[Fiddler](http://www.telerik.com/fiddler) ist ein nützliches Tool für die Analyse von HTTP und HTTPS-Datenverkehr zwischen der Clientanwendung und der Azure-Speicher verwendeten.

> [AZURE.NOTE] Fiddler kann HTTPS-Datenverkehr decodieren. Lesen Sie Fiddler Dokumentation sorgfältig nicht kennen und Verstehen der Sicherheitsaspekte.

Dieser Anhang enthält eine kurze exemplarische Vorgehensweise Fiddler um Datenverkehr zwischen dem lokalen Computer Fiddler Installationsort und Azure-Speicher konfigurieren.

Nach dem Start Fiddler beginnt HTTP- und HTTPS-Datenverkehr auf dem lokalen Computer aufzeichnen. Es folgen einige nützliche Befehle zum Steuern der Fiddler:

- Beenden und Starten von Datenverkehr erfassen. Im Hauptmenü auf **Datei** und dann auf **Datenverkehr erfassen** erfassen und aus zu wechseln.
- Speichern Sie die erfassten Daten. Gehen Sie im Hauptmenü auf **Datei**, klicken Sie auf **Speichern**und dann auf **Alle Sessions**: Dadurch können Sie den Datenverkehr in einer Sitzung Archivdatei speichern. Sie laden eine Sitzung Archiv später zur Analyse oder ggf. an den Microsoft Support senden.

Um die Datenmenge einzuschränken, die Fiddler erfasst, können Sie Filter, die Sie auf der Registerkarte **Filter** konfigurieren. Der folgende Screenshot zeigt einen Filter, der nur Datenverkehr an **contosoemaildist.table.core.windows.net** Speicher Endpunkt erfasst:

![][5]

### <a name="appendix-2"></a>Anlage 2: Mithilfe von Wireshark Netzwerkverkehr

[Wireshark](http://www.wireshark.org/) ist ein Netzwerkprotokoll-Analyseprogramm, das Anzeigen von detaillierten Paketinformationen für eine Vielzahl von Netzwerkprotokollen ermöglicht.

Das folgende Verfahren veranschaulicht erfassen detaillierte Paketinformationen für Datenverkehr aus dem lokalen Computer, installiert Sie Wireshark den Dienst in Ihrem Konto Azure-Speicher.

1.  Wireshark auf Ihrem lokalen Computer zu starten.
2.  Wählen Sie im Abschnitt **Starten** LAN-Schnittstelle oder Schnittstellen, die mit dem Internet verbunden sind.
3.  Klicken Sie auf **Optionen erfassen**.
4.  **Sammlungsfilter** Textfeld einen Filter hinzufügen. **Host-contosoemaildist.table.core.windows.net** wird z. B. Wireshark erfassen nur Pakete in oder aus der Tabelle im Speicherkonto **Contosoemaildist** Endpunkt konfigurieren. Sehen Sie sich die [vollständige Liste der Filter erfasst](http://wiki.wireshark.org/CaptureFilters).

    ![][6]

5.  Klicken Sie auf **Start**. Wireshark wird jetzt alle erfassen die Pakete in oder aus der Tabelle Endpunkt senden Sie Ihre Client-Anwendung auf dem lokalen Computer.
6.  Sobald Sie abgeschlossen haben, klicken Sie im Hauptmenü auf **erfassen** und dann auf **Beenden**.
7.  Klicken Sie die erfassten Daten in einer Sammlungsdatei Wireshark im Hauptmenü **Datei** und **Speichern**.

WireShark markiert alle Fehler, die im Fenster **Packetlist** vorhanden sind. Sie können auch das Informationsfenster **Expert** (klicken Sie auf **Analyse**und **Expert Info**) eine Zusammenfassung der Fehler und Warnungen anzeigen.

![][7]

Sie können auch wählen, die TCP-Daten anzeigen die Anwendungsschicht sieht der TCP-Daten und wählen **folgen TCP-Datenstrom**. Dies ist besonders nützlich, wenn das Abbild ohne einen Sammlungsfilter erfasst. Weitere Informationen finden Sie unter [nach TCP-Datenströme] (http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html).

![][8]

> [AZURE.NOTE] Weitere Informationen zur Verwendung von Wireshark finden Sie im [Benutzerhandbuch Wireshark](http://www.wireshark.org/docs/wsug_html_chunked).

### <a name="appendix-3"></a>Anhang 3: Mithilfe von Microsoft Message Analyzer Netzwerkverkehr

Sie können verwenden Microsoft Message Analyzer ähnlich Fiddler HTTP- und HTTPS-Datenverkehr erfassen und Aufzeichnen des Netzwerkverkehrs in Wireshark ähnlich.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Konfigurieren einer Web Tracing-Sitzung mittels Microsoft Message Analyzer

Konfigurieren eine Sitzung Web Verfolgung für HTTP- und HTTPS-Datenverkehr mithilfe von Microsoft Message Analyzer führen Sie die Anwendung Microsoft Message Analyzer, und klicken Sie im Menü **Datei** auf **Spur /**. Wählen Sie in der Liste der verfügbaren Trace Szenarien **WebProxy**. Fügen Sie im Bereich **Ablaufverfolgungskonfiguration Szenario** im Textfeld **HostnameFilter** die Namen der Speicher Endpunkte (Sie können diese Namen im [Azure-Portal](https://portal.azure.com)nachschlagen). Wenn der Name Ihres Kontos Azure-Speicher **Contosodata**ist, sollten Sie folgende **HostnameFilter** Textfeld hinzufügen:

    contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net

> [AZURE.NOTE] Ein Leerzeichen trennt die Hostnamen.

Wenn Sie Daten sammeln möchten, klicken Sie **Beginnen mit** .

Weitere Informationen über Microsoft Message Analyzer **WebProxy** Trace finden Sie unter [Microsoft-PEF-WebProxy Anbieter](http://technet.microsoft.com/library/jj674814.aspx).

Integrierte **Web-Proxy** -Trace Microsoft Message Analyzer basiert auf Fiddler. Es kann clientseitige HTTPS-Datenverkehr erfassen und unverschlüsselte HTTPS-Nachrichten anzeigen. Trace **-WebProxy** konfigurieren lokalen Proxy für alle HTTP- und HTTPS-Datenverkehr, der Zugriff auf unverschlüsselte Nachrichten ermöglicht wird.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Diagnose von Netzwerkproblemen mithilfe von Microsoft Message Analyzer

Neben mithilfe von Microsoft Message Analyzer **WebProxy** Trace Details der HTTP/HTTPs-Datenverkehr zwischen der Clientanwendung und der Speicherdienst auch können integrierten **Lokale Verbindungsschicht** Trace Sie Paketinformationen erfassen. Ähnlich Datenerfassung mit Wireshark erfassen und analysieren Sie Probleme wie Netzwerk kann Verworfene Pakete.

Der folgende Screenshot zeigt ein Beispiel **Lokale Verbindungsschicht** Trace mit **Informationen** in der **DiagnosisTypes** -Spalte. Klicken auf ein Symbol in der **DiagnosisTypes** -Spalte zeigt die Details der Nachricht. In diesem Beispiel übertragen Server Message #305, da keine Bestätigung vom Client erhalten:

![][9]

Beim Erstellen der Sitzung Trace in Microsoft Message Analyzer können Sie Filter für Rauschen in der Verfolgung. Der **Erfassung / Trace** Seite Trace definieren klicken Sie auf den Link neben **Microsoft-Windows-NDIS-PacketCapture** **Konfigurieren** . Der folgende Screenshot zeigt eine Konfiguration, die TCP-Datenverkehr für IP-Adressen der drei Speicherdienste zu filtern:

![][10]

Weitere Informationen über Microsoft Message Analyzer lokalen Verbindungsschicht Trace finden Sie unter [Microsoft-PEF-NDIS-PacketCapture-Anbieter](http://technet.microsoft.com/library/jj659264.aspx).

### <a name="appendix-4"></a>Anhang 4: Verwendung von Excel zu Metriken Protokolldaten

Viele Programme können Azure-Tabellenspeicher durch Trennzeichen getrennte Lagerung Metrikdaten herunterladen, die die Daten für die Anzeige und Analyse in Excel laden erleichtert. Protokollierung der Speicherdaten von Azure BLOB-Speicher ist bereits ein begrenztes Format in Excel geladen werden kann. Allerdings müssen Sie entsprechenden Spaltenüberschriften Informationen [Storage Analytics Protokollformat](http://msdn.microsoft.com/library/azure/hh343259.aspx) und [Storage Analytics Metriken Tabellenschema](http://msdn.microsoft.com/library/azure/hh343264.aspx)basierend hinzufügen.

Nach dem Herunterladen von BLOB-Speicher Speicher Protokollierung Daten in Excel importieren:

- Klicken Sie im Menü **Daten** auf **Aus Text**.
- Wechseln Sie in die Protokolldatei, die Sie anzeigen möchten und klicken Sie auf **Importieren**.
- Wählen Sie in Schritt 1 des **Textimport-Assistenten** **getrennt**.

In Schritt 1 des **Textimport-Assistenten**, **Semikolon** als Trennzeichen nur und wählen Sie doppelte Anführungszeichen als **Texterkennungszeichen**. Klicken Sie auf **Fertig stellen,** und wählen Sie die Daten in Ihrer Arbeitsmappe angelegt.

### <a name="appendix-5"></a>Anhang 5: Überwachung Anwendung Einblicke für Visual Studio Team Services

Auch können Application Insights-Funktion für Visual Studio Team Services als Teil der Performance und Verfügbarkeit überwachen. Dieses Tool kann:

- Stellen Sie sicher, der Webdienst verfügbar und Reaktionsfähigkeit. Ob Ihre Anwendung ist eine Website oder eine geräteanwendung, die einen Webdienst verwendet, können sie Ihre URL Minuten von Standorten auf der ganzen Welt testen und können Sie feststellen, ob ein Problem vorliegt.
- Schnell Diagnostizieren von Leistungsproblemen oder Ausnahmen in Ihrem Web Service. Finden Sie Wenn CPU oder andere Ressourcen gedehnt werden, Stack-Traces Ausnahmen und Protokoll Spuren durchsuchen. Wenn die App akzeptablen Grenzen unterschreitet, schicken wir Ihnen eine e-Mail. Sie können .NET und Java WebServices überwachen.

Finden Sie weitere Informationen [Neuigkeiten Application Insights?](../application-insights/app-insights-overview.md).

<!--Anchors-->
[Einführung]: #introduction
[Aufbau dieses Handbuchs]: #how-this-guide-is-organized

[Der Speicherdienst überwacht]: #monitoring-your-storage-service
[Dienststatus überwachen]: #monitoring-service-health
[Überwachen der Kapazität]: #monitoring-capacity
[Überwachen der Verfügbarkeit]: #monitoring-availability
[Überwachen der Leistung]: #monitoring-performance

[Diagnose von Problemen Speicher]: #diagnosing-storage-issues
[Service Gesundheitsrisiken]: #service-health-issues
[Performance-Probleme]: #performance-issues
[Diagnostizieren von Fehlern]: #diagnosing-errors
[Emulator Speicherprobleme]: #storage-emulator-issues
[Protokollierung-Datenspeicher]: #storage-logging-tools
[Netzwerk-Protokollierung verwenden]: #using-network-logging-tools

[End-to-End-tracing]: #end-to-end-tracing
[Korrelieren von Daten]: #correlating-log-data
[Client-Anfrage-ID]: #client-request-id
[Server-Kennung]: #server-request-id
[Zeitstempel]: #timestamps

[Hinweise zur Problembehandlung]: #troubleshooting-guidance
[Hohe AverageE2ELatency und niedrigen AverageServerLatency anzeigen Metriken]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Metriken niedrige AverageE2ELatency und niedrigen AverageServerLatency, sondern der Client treten Wartezeiten]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Metriken zeigen hohe AverageServerLatency]: #metrics-show-high-AverageServerLatency
[Es treten unerwartete bei Zustellung von Nachrichten in einer Warteschlange]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Metriken zeigen eine PercentThrottlingError]: #metrics-show-an-increase-in-PercentThrottlingError
[Vorübergehende Erhöhung PercentThrottlingError]: #transient-increase-in-PercentThrottlingError
[Permanente Zunahme PercentThrottlingError Fehler]: #permanent-increase-in-PercentThrottlingError
[Metriken zeigen eine PercentTimeoutError]: #metrics-show-an-increase-in-PercentTimeoutError
[Metriken zeigen eine PercentNetworkError]: #metrics-show-an-increase-in-PercentNetworkError

[Der Client empfängt Nachrichten HTTP 403 (Verboten)]: #the-client-is-receiving-403-messages
[Der Client empfängt Nachrichten HTTP 404 (nicht gefunden)]: #the-client-is-receiving-404-messages
[Der Client oder ein anderer Prozess bereits das Objekt gelöscht]: #client-previously-deleted-the-object
[Eine Autorisierung Problem Shared Access Signatur (SAS)]: #SAS-authorization-issue
[Clientseitige JavaScript-Code hat keinen Zugriff auf das Objekt]: #JavaScript-code-does-not-have-permission
[Netzwerkfehler]: #network-failure
[Der Client ist HTTP 409 (Konflikt) Nachrichten empfängt.]: #the-client-is-receiving-409-messages

[Metriken zeigen niedrige PercentSuccess oder Analytics Protokolleinträge arbeiten mit Buchungsstatus der ClientOtherErrors]: #metrics-show-low-percent-success
[Kapazität Metriken zeigen ein unerwarteter Speicher-Auslastung]: #capacity-metrics-show-an-unexpected-increase
[Unerwarteter Neustart virtueller Maschinen auftreten, die eine große Anzahl von angeschlossenen Festplatten]: #you-are-experiencing-unexpected-reboots
[Das Problem entsteht Speicheremulator für Entwicklungs- oder Testzwecke verwenden]: #your-issue-arises-from-using-the-storage-emulator
[Funktion "X" funktioniert in der Speicheremulator nicht]: #feature-X-is-not-working
[Fehlermeldung "der Wert für einen HTTP-Header ist nicht im richtigen Format" Verwendung des Speicheremulators]: #error-HTTP-header-not-correct-format
[Ausführen des Speicheremulators sind Administratorrechte erforderlich]: #storage-emulator-requires-administrative-privileges
[Probleme bei der Installation von Azure SDK für .NET stoßen]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Sie haben ein anderes Problem mit einem Storage-Dienst]: #you-have-a-different-issue-with-a-storage-service

[Anhänge]: #appendices
[Anhang 1: Mithilfe von Fiddler HTTP- und HTTPS-Datenverkehr]: #appendix-1
[Anlage 2: Mithilfe von Wireshark Netzwerkverkehr]: #appendix-2
[Anhang 3: Mithilfe von Microsoft Message Analyzer Netzwerkverkehr]: #appendix-3
[Anhang 4: Verwendung von Excel zu Metriken Protokolldaten]: #appendix-4
[Anhang 5: Überwachung Anwendung Einblicke für Visual Studio Team Services]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png
