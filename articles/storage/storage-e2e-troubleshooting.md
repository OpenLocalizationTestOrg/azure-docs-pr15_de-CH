<properties
    pageTitle="End-to-End-Problembehandlung mit Azure Storage Metriken und Protokollierung, AzCopy und Message Analyzer | Microsoft Azure"
    description="Eine Anleitung zur Problembehandlung End-to-End Azure Speicheranalyse AzCopy und Microsoft Message Analyzer"
    services="storage"
    documentationCenter="dotnet"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>End-to-End-Problembehandlung mit Azure Storage Metriken und Protokollierung, AzCopy und Message Analyzer

[AZURE.INCLUDE [storage-selector-portal-e2e-troubleshooting](../../includes/storage-selector-portal-e2e-troubleshooting.md)]

## <a name="overview"></a>Übersicht

Diagnose und Fehlerbehebung ist eine wichtige Fähigkeit erstellen und unterstützt Clientanwendungen mit Microsoft Azure-Speicher. Aufgrund der verteilten Azure-Anwendung diagnostizieren und Beheben von Fehlern und Leistungsproblemen komplexer als in einer traditionellen Umgebung möglicherweise.

In diesem Tutorial zeigen wir Client bestimmte Fehler identifizieren, die die Leistung beeinträchtigen, und die Fehlerbehebung von End-to-End mit Tools von Microsoft und Azure-Speicher, um die Clientanwendung zu optimieren.

Dieses Lernprogramm bietet eine praktische Untersuchung einer Problembehandlung End-to-End-Szenario. Eine detaillierte konzeptionelle Anleitung zur Problembehandlung Azure-Speicher-Anwendung finden Sie unter [Überwachung, diagnose und Problembehandlung von Microsoft Azure-Speicher](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Tools zur Problembehandlung bei Azure Storage-Applikationen

Clientanwendungen mit Microsoft Azure-Speicher zu beheben, können Sie eine Kombination verschiedener Tools, wenn ein Problem aufgetreten ist und was die Ursache des Problems möglicherweise. Dazu zählen:

- **Azure Speicheranalyse**. [Azure Storage Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx) stellt Metriken und Protokollierung für den Azure-Speicher.
    - **Speicher-Metriken** verfolgt Transaktion Metriken und Kapazität Metriken für das Speicherkonto. Mit Metriken bestimmen Sie Leistung Ihrer Anwendung nach verschiedenen Maßnahmen. Weitere Informationen zu den vom Speicheranalyse Metriken finden Sie unter [Storage Analytics Metriken Tabellenschema](http://msdn.microsoft.com/library/azure/hh343264.aspx) .

    - **Speicher Protokollierung** protokolliert jede Anforderung in Azure Storage Services eine serverseitige Protokoll. Das Protokoll verfolgt Detaildaten für jede Anforderung, einschließlich der Operation ausgeführt wird, den Status der Operation und Wartezeit Informationen. Weitere Informationen zu der Anforderung und Antwort Daten, die in den Protokollen Speicheranalyse steht finden Sie unter [Storage Analytics-Protokollformat](http://msdn.microsoft.com/library/azure/hh343259.aspx) .

> [AZURE.NOTE] Speicherkonten mit Replikation Zone redundanten Speicher (ZRS) müssen nicht Metriken oder Protokollfunktion zurzeit aktiviert. 

- **Azure-Portal**. Für das Speicherkonto in [Azure-Portal](https://portal.azure.com)können Sie Metriken und Protokollierung konfigurieren. Sie können auch anzeigen von Diagrammen und Graphen, die anzeigen, wie die Anwendung mit der Zeit arbeitet und Serveradministrator Sie benachrichtigt, wenn Ihre Anwendung anders als erwartet Produktlinien durchführt.

    Informationen zum Konfigurieren der Überwachung in der Azure-Portal finden Sie unter [Monitor ein Speicherkonto in Azure-Portal](storage-monitor-storage-account.md) .

- **AzCopy**. AzCopy Verwendung Protokoll Blobs in ein lokales Verzeichnis für Microsoft Message Analyzer mit kopieren werden als Blobs Serverprotokollen Azure Storage gespeichert. Weitere Informationen zu AzCopy finden Sie unter [Datenübertragung mit AzCopy Befehlszeilenprogramm](storage-use-azcopy.md) .

- **Microsoft Message Analyzer**. Message Analyzer ist ein Tool, die Protokolldateien und zeigt Daten in einem visuellen Format, das Filtern, suchen und Gruppendaten in nützliche Gruppen erleichtert, mit denen Sie Fehler und Leistungsprobleme analysieren. [Microsoft Analyzer Betrieb für](http://technet.microsoft.com/library/jj649776.aspx) Nachricht Analyzer weitere Informationen anzeigen

## <a name="about-the-sample-scenario"></a>Über das Beispielszenario

In diesem Lernprogramm wird ein Szenario untersucht, Azure Storage Metriken eine niedrigen Prozentsatz Erfolgsrate für eine Anwendung gibt, der Azure-Speicher aufruft. Die niedrigen Prozentsatz Erfolg Rate Metrik (dargestellt als **PercentSuccess** in der [Azure-Portal](https://portal.azure.com) und metriktabellen) verfolgt Operationen, die erfolgreich, jedoch einen HTTP-Statuscode, der größer als 299 zurückgeben. Diese Vorgänge werden in den Protokolldateien serverseitige Speicher Buchungsstatus der **ClientOtherErrors**erfasst. Finden Sie weitere Informationen über die niedrigen Prozentsatz Erfolg Metrik [Metriken zeigen niedrige PercentSuccess oder Analytics Protokolleinträge arbeiten mit Transaktion ClientOtherErrors](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Azure Speichervorgänge können HTTP-Statuscodes als normale Funktionen von größer als 299 zurück. Aber diese Fehler in einigen Fällen werden Sie die Clientanwendung zur Verbesserung der Leistung optimieren.

Dabei betrachten wir eine niedrige prozentuale Erfolgsrate unter 100 % sein. Sie können eine andere metrische Ebene jedoch nach Belieben auswählen. Wir empfehlen, beim Testen der Anwendung eine Baseline-Toleranz für die wichtigsten Kennzahlen einrichten. Beispielsweise können Sie ermitteln, basierend auf Tests, dass Ihre Anwendung eine konsistente Prozent Erfolgsquote von 90 % und 85 % sollten. Wenn die Metrikdaten, dass zeigt die Anwendung diese Nummer abweichende und untersuchen Sie die Ursache des Anstiegs.

Unsere Beispielszenario Sobald wir festgestellt haben, dass Prozent Erfolg Rate Metrik unter 100 % untersuchen wir die Protokolle, die Fehler finden, die den Kriterien entsprechen und herauszufinden, niedrigere Prozent Erfolgsquote wodurch verwenden. Wir sehen auf Fehler im Bereich von 400. Anschließend werden wir Fehlern vom Typ 404 (nicht gefunden) genauer untersuchen.

### <a name="some-causes-of-400-range-errors"></a>Einige Ursachen für Fehler 400-Bereich

Die Beispiele zeigt Beispiele für Abfragen Azure BLOB-Speicher und deren mögliche Ursachen Fehler 400-Bereich. Dieser Fehler sowie Fehler in 300 und 500 Bereich können zum einen niedrigen Prozentsatz Erfolgsrate beitragen.

Beachten Sie, dass die folgenden Listen bei weitem nicht vollständig. Einzelheiten finden Sie unter [Status und Fehlercodes](http://msdn.microsoft.com/library/azure/dd179382.aspx) auf MSDN allgemeine Azure Storage-Fehler und Fehler für jeden Speicher-Services.

**Beispiele für Status-Code 404 (nicht gefunden)**

Tritt auf, wenn ein Lesevorgang für einen Container oder Blob fehlschlägt, weil das Blob oder Container nicht gefunden wird.

- Tritt auf, wenn ein Container oder Blob von einem anderen Client diese Anforderung gelöscht wurde.
- Tritt auf, wenn Sie einen API-Aufruf, der erstellt den Container oder Blob nach dem Überprüfen verwenden, ob er vorhanden ist. APIs CreateIfNotExists anrufen Kopf zuerst überprüft, ob der Container oder Blob; Wenn es nicht vorhanden ist, Fehler 404 zurückgegeben und dann eine zweite PUT aufgerufen Container oder Blob schreiben.

**Status Code 409 (Konflikt)-Beispiele**

- Tritt auf, wenn eine API erstellen zum Erstellen eines neuen Container oder Blob ohne Vorhandensein zuerst verwenden und einen Container oder Blob mit diesem Namen ist bereits vorhanden.
- Tritt ein, wenn der Container gelöscht wird und Sie versuchen, einen neuen Container mit demselben Namen erstellen, bevor der Löschvorgang abgeschlossen ist.
- Tritt auf, wenn eine Lease für einen Container oder Blob angeben und bereits eine Lease vorhanden ist.

**Statuscode 412 (Voraussetzung fehlgeschlagen) Beispiele**

- Tritt ein, wenn Sie einen bedingten Header angegebene Bedingung nicht erfüllt wurde.
- Tritt ein, wenn die angegebene Lease-ID nicht die Lease-ID auf den Container oder Blob.

## <a name="generate-log-files-for-analysis"></a>Generieren von Protokolldateien für die Analyse

In diesem Lernprogramm verwenden Message Analyzer wir arbeiten mit drei verschiedenen Protokolldateien, obwohl konnte diese arbeiten möchten:

- **Serverprotokoll**bei Azure Storage Protokollierung erstellt wird. Das Serverprotokoll enthält Daten über jeden Vorgang gegen eines der Azure-Speicher - BLOBs, Warteschlangen, Tabelle und Datei. Das Serverprotokoll gibt an, welcher Vorgang wurde und welche Statuscode zurückgegebenen sowie andere Details der Anforderung und Antwort.
- **.NET Client Protokolldateien**, die beim Aktivieren der Protokollierung von clientseitigen Anwendung .NET erstellt. Der Client enthält detaillierte Informationen wie der Client die Anforderung empfängt und verarbeitet die Antwort.
- **Http-Netzwerk-Ablaufverfolgungsprotokoll**Daten über HTTP/HTTPS-Anforderung und Antwort Daten erfasst, einschließlich für Operationen mit Azure-Speicher. In diesem Lernprogramm generieren wir Netzwerk-Trace über Message Analyzer.

### <a name="configure-server-side-logging-and-metrics"></a>Konfigurieren von serverseitigen Protokollierung und Metriken

Zunächst müssen wir Azure Storage Protokollierung und Metriken konfigurieren, haben wir Daten aus der Clientanwendung analysieren. Sie können Protokollierung und Metriken auf verschiedene Arten - [Azure-Portal](https://portal.azure.com)mit PowerShell oder programmgesteuert. Einzelheiten finden Sie unter [Speichermetriken aktivieren und Metrikdaten anzeigen](http://msdn.microsoft.com/library/azure/dn782843.aspx) und [Aktivieren der Protokollierung Speicher Zugriff auf Daten](http://msdn.microsoft.com/library/azure/dn782840.aspx) auf MSDN zum Konfigurieren der Protokollierung und Metriken.

**Der Azure-Portal**

Zum Konfigurieren der Protokollierung und Metriken für das Speicherkonto [Azure-Portal](https://portal.azure.com)mit Anweisungen Sie den auf [ein Speicherkonto in Azure-Portal](storage-monitor-storage-account.md).

> [AZURE.NOTE] Es ist nicht möglich die Minute Kennzahlen über das Azure-Portal. Jedoch wird empfohlen, sie im Rahmen dieses Lernprogramms und Untersuchung von Leistungsproblemen in der Anwendung festzulegen. Sie können minutenmetriken mithilfe von PowerShell wie folgt oder programmgesteuert über die Speicher-Clientbibliothek festlegen.
>
> Beachten Sie, dass der Azure-Verwaltungsportal Minute Metriken stündlich Metriken anzeigen kann.

**Über PowerShell**

Zunächst mit PowerShell für Azure finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

1. Verwenden Sie das Cmdlet [Hinzufügen AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) im PowerShell Azure Benutzerkonto hinzu:

    ```
    Add-AzureAccount
    ```

2. Geben Sie im Fenster **Microsoft Azure anmelden** die e-Mail-Adresse und das Kennwort für Ihr Konto ein. Azure authentifiziert und die Anmeldeinformationen speichert und schließt das Fenster.
3. Richten Sie das Standardkonto Storage Storage-Konto verwendeten für das Lernprogramm durch Ausführen dieser Befehle im PowerShell-Fenster

    ```
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount'
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

4. Aktivieren der speicherprotokollierung für den BLOB-Dienst:

    ```
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0
    ```
5. Aktivieren Sie speichermetriken für den BLOB-Dienst **- MetricsType** festlegen `Minute`:

    ```
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0
    ```

### <a name="configure-net-client-side-logging"></a>.NET clientseitige Protokollierung

Aktivieren Sie zum Konfigurieren der Protokollierung für eine clientseitige .NET Diagnose in der Anwendungskonfigurationsdatei (web.config oder app.config). Einzelheiten finden Sie [clientseitige Protokollierung mit .NET Speicher-Clientbibliothek](http://msdn.microsoft.com/library/azure/dn782839.aspx) und [clientseitige Protokollierung mit Microsoft Azure Storage SDK für Java](http://msdn.microsoft.com/library/azure/dn782844.aspx) auf MSDN.

Die clientseitige enthält detaillierte Informationen wie der Client die Anforderung empfängt und verarbeitet die Antwort.

Der Speicher-Clientbibliothek speichert clientseitige Protokolldaten in der Konfigurationsdatei der Anwendung (web.config oder app.config) angegebenen Position.

### <a name="collect-a-network-trace"></a>Sammeln von Netzwerk-trace

Message Analyzer können eine HTTP/HTTPS-Netzwerk-Trace sammeln, während die Clientanwendung ausgeführt wird. Message Analyzer verwendet [Fiddler](http://www.telerik.com/fiddler) Back-End. Vor dem Sammeln von Netzwerk-Trace empfohlen Fiddler aufzeichnen unverschlüsselten HTTPS-Datenverkehr zu konfigurieren:

1. Installieren Sie [Fiddler](http://www.telerik.com/download/fiddler).
2. Starten Sie Fiddler.
2. Wählen Sie **| Fiddler-Optionen**.
3. Sicherstellen Sie im Dialogfeld Optionen, dass **HTTPS-Verbindung aufnehmen** und **HTTPS-Datenverkehr entschlüsseln** ausgewählt werden, wie unten dargestellt.

![Fiddler-Optionen konfigurieren](./media/storage-e2e-troubleshooting/fiddler-options-1.png)

Das Lernprogramm sammeln Erstellen einer Analyse Sitzung Trace und die Protokolle zu analysieren und einen Netzwerk-Trace Analyzer Nachricht zuerst speichern. Netzwerk-Trace in Nachricht Analyzer zu sammeln:

1. Wählen Sie Nachricht Analyzer **Datei | Schnelle Trace | Unverschlüsselt HTTPS**.
2. Trace beginnt sofort. Wählen Sie **Beenden** , Trace beenden, damit wir Trace-Speicherdatenverkehr konfigurieren können.
3. Wählen Sie die Verfolgung Sitzung bearbeiten **Bearbeiten** .
4. Klicken Sie auf **Konfigurieren** , rechts von der **Microsoft-Pef-WebProxy** ETW-Anbieter.
5. Klicken Sie im Dialogfeld **Advanced Settings** auf die Registerkarte **Provider** .
6. Geben Sie im Feld **Hostname-Filter** Speicher Endpunkte, getrennt durch Leerzeichen. Beispielsweise können Sie Ihre Endgeräte wie folgt angeben. Ändern `storagesample` auf den Namen Ihres Speicherkontos:

    ```
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net
    ```

7. Schließen Sie das Dialogfeld und auf erfassen Trace mit den Hostname-Filter an, **Starten** , so dass nur Netzwerkverkehr Azure-Speicher in der Verfolgung enthalten ist.

>[AZURE.NOTE] Nach dem Sammeln von Netzwerk-Trace wird dringend empfohlen, die Standardeinstellungen wiederherstellen, die Sie in Fiddler entschlüsseln HTTPS-Datenverkehr möglicherweise geändert. Deaktivieren Sie im Dialogfeld Optionen Fiddler Kontrollkästchen **HTTPS-Verbindung aufnehmen** und **HTTPS-Datenverkehr entschlüsseln** .

Näheres [mithilfe der Network Tracing](http://technet.microsoft.com/library/jj674819.aspx) auf Technet.

## <a name="review-metrics-data-in-the-azure-portal"></a>Daten Sie Metriken in Azure-Portal

Wenn Ihre Anwendung für einen Zeitraum ausgeführt wurde, können Sie Diagramme Metriken überprüfen, die auf der [Azure-Portal](https://portal.azure.com) zu beobachten, wie der Dienst durchgeführt hat. Erstens das Speicherkonto in Azure-Portal navigieren Sie und fügen Sie ein Diagramm für die Metrik **Prozentsatz erfolgreich** .

Im Azure-Portal sehen Sie nun **Erfolg Prozentsatz** Überwachung Diagramm zusammen mit anderen Daten, die Sie möglicherweise hinzugefügt haben. In dem Szenario wir untersuchen anschließend anhand der Protokolle in der Nachricht Analyzer ist die prozentuale Erfolgsrate etwas unter 100 %.

Weitere Informationen zum Hinzufügen von Kriterien auf der Seite Überwachung finden Sie unter [wie: metriktabelle Kennzahlen hinzufügen](storage-monitor-storage-account.md#how-to-add-metrics-to-the-metrics-table).

> [AZURE.NOTE] Es einige Zeit dauern die Metrikdaten in Azure-Portal nach speichermetriken aktivieren. Deswegen stündliche Metriken für die letzte Stunde in Azure-Portal nicht angezeigt werden, bis die aktuelle Stunde vergangen ist. Minute Metriken werden auch nicht gerade im Azure-Portal angezeigt. Je nach Wenn Metriken aktivieren, es Metriken Daten bis zu zwei Stunden dauern.

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>Verwenden Sie AzCopy Server-Protokolle in einem lokalen Verzeichnis kopieren

Azure-Speicher schreibt Server-Protokolldaten in Blobs Metriken Tabellen geschrieben werden. Protokoll-Blobs stehen in der `$logs` Container für das Speicherkonto. Protokoll-Blobs heißen hierarchisch nach Jahr, Monat, Tag und Stunde, sodass leicht die Zeitspanne finden können, die Sie untersuchen möchten. In der `storagesample` Konto der Container für die Protokoll-Blobs 01/02/2015, 8 und 9 Uhr ist `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. Einzelne Blobs in diesem Container aufeinander folgend benannt, beginnend mit `000000.log`.

Das Befehlszeilentool AzCopy können diese serverseitige Protokolldateien an einen Speicherort Ihrer Wahl auf dem lokalen Computer herunterladen. Beispielsweise können Sie Folgendes herunterladen Protokolldateien für BLOB-Operationen, die in den Ordner am 2. Januar 2015 stattgefunden `C:\Temp\Logs\Server`; Ersetzen Sie `<storageaccountname>` mit dem Namen Ihres Speicherkontos und `<storageaccountkey>` mit Ihrem Konto zugreifen:

    AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V

AzCopy ist der [Azure](https://azure.microsoft.com/downloads/) -Downloadseite heruntergeladen. Informationen zur Verwendung von AzCopy finden Sie unter [Datenübertragung mit AzCopy Befehlszeilenprogramm](storage-use-azcopy.md).

Weitere Informationen zum Download von serverseitigen Protokolle anzeigen Sie [Protokolldaten herunterladen Speicher Protokollierung](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata)

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Verwenden von Microsoft Message Analyzer Daten analysieren

Microsoft Message Analyzer ist ein Tool zum Aufzeichnen, anzeigen und Analysieren von messaging-Verkehr, Ereignisse und andere Nachrichten System- oder zur Problembehandlung und Diagnose Protokoll. Message Analyzer können Sie laden, zusammenfassen und Analysieren von Daten aus Protokolldateien auch Trace-Dateien gespeichert. Weitere Informationen zu Message Analyzer finden Sie unter [Microsoft Message Analyzer Betrieb Guide](http://technet.microsoft.com/library/jj649776.aspx).

Message Analyzer enthält Ressourcen für Azure-Speicher, mit denen Sie Server, Clients und Netzwerk-Protokolle analysieren. In diesem Abschnitt besprechen wir, wie Sie diese Tools Problems niedrigen Prozentsatz Erfolg in Storage-Protokolle.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>Message Analyzer und Azure Storage-Ressourcen

1. Herunterladen Sie [Message Analyzer](http://www.microsoft.com/download/details.aspx?id=44226) aus dem Microsoft Download Center, und führen Sie das Installationsprogramm.
2. Message Analyzer zu starten.
3. Wählen Sie im Menü **Extras** **Asset Manager**. Wählen Sie im Dialogfeld **Ressourcen-Manager** **Downloads**und Filtern Sie **Azure Storage**. Speicherressourcen Azure wird angezeigt, wie in der folgenden Abbildung dargestellt.
4. Klicken Sie auf **Alle angezeigten Elemente synchronisieren** um Speicherressourcen Azure installieren. Die verfügbaren Ressourcen zählen:
    - **Azure-Speicher Farbe Regeln:** Azure Storage Farbe Regeln aktivieren Sie spezielle Filter definieren, mit denen Farbe, Text und Schriftarten Nachrichten markieren, die bestimmte Informationen in eine Spur enthalten.
    - **Azure-Speicher Diagramme:** Azure sind Storage vordefinierte Diagramme, die Serverdaten grafisch darstellen. Beachten Sie, dass für die Verwendung der Azure-Speicher Diagramme zu diesem Zeitpunkt nur das Serverprotokoll in der Analyse laden kann.
    - **Azure-Speicher Parser:** Azure Storage-Parser analysiert Azure Storage Client, Server und HTTP-Protokolle im Analyseraster angezeigt.
    - **Filter Azure-Speicher:** Azure Storage-Filter sind vordefinierte Kriterien, mit denen Sie Daten im Analyseraster.
    - **Ansichtslayouts Azure-Speicher:** Azure Storage Ansichtslayouts sind vordefinierte Spalte Gruppen in der Analysetabelle.
4. Nach Installation die Anlagen Message Analyzer neu.

![Message Analyzer Asset Manager](./media/storage-e2e-troubleshooting/mma-start-page-1.png)

> [AZURE.NOTE] Installieren Sie alle Azure-Speicher für die Zwecke dieses Lernprogramms angezeigt.

### <a name="import-your-log-files-into-message-analyzer"></a>Message Analyzer Protokolldateien importieren

Sie können Ihre gespeicherten Protokolldateien (serverseitige clientseitige und Netzwerk) in einer einzigen Sitzung im Microsoft Message Analyzer für die Analyse.

1. Microsoft Message Analyzer im Menü **Datei** auf **Neue Sitzung**und dann auf **Leere Sitzung**. Geben Sie im Dialogfeld **Neue Sitzung** einen Namen für Ihre Analyse-Sitzung. Klicken Sie auf die **Dateien** im Bereich **Sitzungsdetails** .
1. Netzwerk verfolgen Daten Message Analyzer laden, klicken Sie auf **Dateien hinzufügen**, suchen Sie den Speicherort die Datei .matp Web Tracing Sitzung wählen Sie die Datei .matp und klicken Sie auf **Öffnen**.
1. Um die serverseitige Daten zu laden, klicken Sie auf **Dateien hinzufügen**, navigieren Sie zum Speicherort, in dem die serverseitige Protokolle heruntergeladen, Protokolldateien für den Zeitraum, den Sie analysieren möchten, und klicken Sie auf **Öffnen**aus. Legen Sie im Bereich **Sitzung** **Text Protokollkonfiguration** Dropdown für jede Protokolldatei serverseitige, **AzureStorageLog** , um sicherzustellen, dass die Protokolldatei von Microsoft Message Analyzer korrekt analysiert werden kann.
1. Um die clientseitigen Daten zu laden, klicken Sie auf **Dateien hinzufügen**, suchen Sie den Speicherort die clientseitigen Protokollen wählen Sie Protokolldateien analysieren möchten und klicken Sie auf **Öffnen**. Legen Sie im Bereich **Sitzung** **Text Protokollkonfiguration** Dropdown für jede Protokolldatei clientseitige auf **AzureStorageClientDotNetV4** , um sicherzustellen, dass die Protokolldatei von Microsoft Message Analyzer korrekt analysiert werden kann.
1. Klicken Sie im Dialogfeld **Neue Sitzung** laden und Analysieren der Daten **beginnen** . Die Protokolldaten zeigt in der Nachricht Analyzer Analysetabelle.

Die folgende Abbildung zeigt eine Sitzung mit Server, Client und Netzwerk Verlaufsprotokolldateien konfiguriert.

![Konfigurieren Sie Nachricht Analyzer-Sitzung](./media/storage-e2e-troubleshooting/configure-mma-session-1.png)

Beachten Sie, dass Message Analyzer Protokolldateien in den Speicher geladen. Haben Sie einen großen Satz von Daten sollten Sie filter verwenden, um die Leistung von Message Analyzer erhalten.

Zunächst bestimmen Sie des Zeitrahmens, der Sie interessiert sind und halten Sie dieses Zeitrahmens möglichst gering zu. In vielen Fällen möchten Sie einen Zeitraum von Minuten oder Stunden höchstens überprüfen. Importieren Sie den kleinsten Satz von Protokollen, die Ihre Bedürfnisse erfüllen können.

Wenn noch eine große Menge von Daten sollten Sie eine Sitzung zu Filtern geben Sie die Daten vor dem Laden. Klicken Sie im **Sitzung Filter** **Bibliothek** auswählen vordefinierten Filter; Beispielsweise wählen Sie **globale Zeit Filter I** Azure Storage Filter Filter in einem Zeitintervall. Dann können Filterkriterien angeben das Start- und Enddatum der Zeitstempel für das Intervall, das Sie anzeigen möchten. Sie können auch über einen bestimmten Status filtern. Sie können z. B. nur Einträge Laden der Statuscode 404 ist.

Weitere Informationen zum Importieren von Daten in Microsoft Message Analyzer finden Sie unter [Abrufen von Daten](http://technet.microsoft.com/library/dn772437.aspx) auf TechNet.

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>Verwenden Sie die Client-ID, um Daten zu korrelieren

Azure Storage-Clientbibliothek generiert automatisch eine Anforderung eindeutige Client-ID für jede Anforderung. Dieser Wert wird Clientprotokoll, das Serverprotokoll und Netzwerk-Trace geschrieben, so dass Sie um Daten über alle drei Protokollen Message Analyzer zu korrelieren können. Finden Sie unter [Clientkennung](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) für Weitere Informationen über die Clientanforderung ID

In den folgenden Abschnitten wird beschrieben, wie Layout vordefinierten und benutzerdefinierten Ansichten Korrelieren und Daten auf die Client-Anfrage-ID

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Wählen Sie ein Layout anzeigen im Analyseraster anzeigen

Speicherressourcen für Message Analyzer enthalten Azure Storage Ansichtslayouts sind vorkonfigurierte Ansichten, mit denen Sie Ihre Daten hilfreich zum Gruppieren mit Spalten für unterschiedliche Szenarien. Sie können auch benutzerdefinierte Ansichtslayouts erstellen und zur Wiederverwendung speichern.

Die folgende Abbildung zeigt im Menü **Ansichtslayout** der Symbolleiste Multifunktionsleiste **Ansichtenlayout** auswählen. Die Ansichtslayouts Azure Storage sind unter **Azure Storage** Node im Menü gruppiert. Suchen `Azure Storage` im Suchfeld Azure Storage Filtern Layouts nur anzeigen. Sie können auch auswählen, dass den Stern neben Ansichtslayout Favoriten und oben im Menü angezeigt.

![Zeigen Sie im Menü Layout](./media/storage-e2e-troubleshooting/view-layout-menu.png)

Wählen Sie zunächst **gruppiert nach ClientRequestID und Modul**. Dieses Layout Gruppen anzeigen Protokolldaten aus allen drei Protokolle zuerst vom Clientkennung und Quelldatei (oder **Modul** Message Analyzer). Mit dieser Ansicht können Sie Drilldown in bestimmten Clientkennung und Daten alle drei Protokolldateien für diese Clientanforderung-ID.

Das Bild unten zeigt diese Ansicht auf Protokolldaten Beispiel mit einer Teilmenge der Spalten angewendet. Sie können sehen, dass für einen bestimmten Client-ID, der Analysetabelle Daten aus dem Clientprotokoll Serverprotokoll und Netzwerk-Trace angezeigt.

![Ansichtslayout Azure-Speicher](./media/storage-e2e-troubleshooting/view-layout-client-request-id-module.png)

>[AZURE.NOTE] Verschiedenen Protokolldateien haben unterschiedliche Spalten, wenn Daten aus mehreren Protokolldateien im Analyseraster angezeigt werden, enthalten einige Spalten keine Daten für eine bestimmte Zeile möglicherweise. Z. B. in der obigen Abbildung zeigen Client Protokoll Zeilen Daten für **Zeitstempel**, **TimeElapsed**, **Quelle**und **Ziel** Spalten nicht diese Spalten existieren nicht im Clientprotokoll, da im Netzwerk-Trace existieren. Ebenso die **Timestamp** -Spalte zeigt Timestamps aus dem Serverprotokoll, aber keine Daten für Spalten **TimeElapsed**, **Quelle**und **Ziel** sind nicht Teil des Server-Protokoll angezeigt.

Neben Ansichtslayouts Azure-Speicher, können Sie auch definieren und speichern eigene Layouts anzeigen. Sie können anderen gewünschten Felder zum Gruppieren von Daten auswählen und die Gruppierung als Teil des benutzerdefinierten Layouts sowie gespeichert.

### <a name="apply-color-rules-to-the-analysis-grid"></a>Analyseraster Farbe-Regeln

Die Speicherressourcen auch Farbe Regeln eine visuelle bedeutet bieten, verschiedene Arten von Fehlern in der Analysetabelle. Die vordefinierten Farbe gilt für HTTP-Fehler so nur für Server protokollieren und Netzwerk-Trace erscheinen.

Um Farbe gilt die Multifunktionsleiste Symbolleiste wählen Sie **Farbe Regeln aus** . Sie sehen die Azure-Speicher Farbe Regeln im Menü. Wählen Sie für das Lernprogramm **Clientfehler (StatusCode 400 bis 499)**wie in der folgenden Abbildung dargestellt.

![Ansichtslayout Azure-Speicher](./media/storage-e2e-troubleshooting/color-rules-menu.png)

Zusätzlich zur Verwendung der Azure-Speicher Farbe, Sie definieren auch eigene Farbe Regeln speichern.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>Gruppieren und Filtern Daten, 400 Bereich Fehler

Wir werden gruppieren und Filtern die Daten um alle Fehler 400 finden.

1. Die **StatusCode** Spalte im Analyseraster suchen, mit der rechten Maustaste in der Spaltenüberschrift und wählen Sie **aus**.
2. Gruppe in der Spalte **ClientRequestId** weiter. Sie sehen, dass die Daten im Analyseraster Statuscode und Client-ID Anforderung jetzt organisiert
1. Anzuzeigen Sie das Toolfenster Ansichtsfilter, wenn es nicht bereits angezeigt wird. Wählen Sie auf der Multifunktionsleiste Symbolleiste **Toolfenster**dann **Ansichtsfilter**.
2. Zum Filtern der Protokolldaten anzeigen nur 400 Bereich Fehler Fenster **Filter** die folgenden Filterkriterien hinzu, und klicken Sie auf **Übernehmen**:

        (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)

Die folgende Abbildung zeigt die Ergebnisse dieser gruppieren und filtern. Erweitern unter Gruppierung für Statuscode 409 Feld **ClientRequestID** , enthält z. B. einen Vorgang, Statuscode führte.

![Ansichtslayout Azure-Speicher](./media/storage-e2e-troubleshooting/400-range-errors1.png)

Nach Anwendung dieses Filters sehen Sie Zeilen aus dem Clientprotokoll ausgeschlossen, wie des Clients Protokoll enthält keinen **StatusCode** Spalte. Wir werden Server und Netzwerk-Ablaufverfolgungsprotokolle 404-Fehler gefunden und dann kehren wir in das Clientlog Client Vorgänge überprüfen, die dazu geführt haben.

>[AZURE.NOTE] **StatusCode** Spalte gefiltert, und alle drei Protokolle, einschließlich Clientprotokoll hinzufügen einen Ausdruck zum Filter, Protokolleinträge enthält null der Statuscode ist, weiterhin anzeigen. Zum Erstellen dieser Filterausdruck verwenden:
>
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code>
>
> Dieser Filter gibt alle Zeilen vom Client Protokoll und nur Zeilen aus dem Serverprotokoll und HTTP-Protokoll ist der Statuscode 400 größer. Wenn Sie Clientkennung und Modul gruppiert anzeigen-Layout anwenden, können Sie suchen oder Blättern durch die Protokolleinträge zu, wo alle drei Protokolle dargestellt.   

### <a name="filter-log-data-to-find-404-errors"></a>Filtern von Daten zu 404-Fehlern

Die Speicherressourcen enthalten vordefinierte Filter, mit denen Sie Daten zu Fehlern oder Trends für Suchen eingrenzen. Als Nächstes wenden wir zwei vordefinierte Filter:, Server und Netzwerk-Ablaufverfolgungsprotokolle 404 Fehler Filter, und das Filtern von Daten auf einen angegebenen Zeitraum.

1. Anzuzeigen Sie das Toolfenster Ansichtsfilter, wenn es nicht bereits angezeigt wird. Wählen Sie auf der Multifunktionsleiste Symbolleiste **Toolfenster**dann **Ansichtsfilter**.
2. Im Fenster Filter **Bibliothek**und Suche `Azure Storage` den Azure-Speicher zu filtern. Wählen Sie den Filter für **404 (nicht gefunden) Nachrichten in allen Protokollen**.
3. Anzeigen des **Bibliothek** im Menüs, und wählen Sie **Globale Zeitfilter**.
4. Bearbeiten Sie die Zeitstempel angezeigt im angegebenen Bereich anzeigen möchten. Dadurch eingeengt Daten analysieren.
5. Der Filter sollte ähnlich wie im Beispiel unten angezeigt. Klicken Sie auf **Übernehmen** , um der Analysetabelle den Filter anwenden.

        ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And
        (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)

![Ansichtslayout Azure-Speicher](./media/storage-e2e-troubleshooting/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Analysieren der Daten

Da Sie gruppierte und gefilterte Daten, können Sie die Details der einzelnen Anfragen überprüfen, die 404-Fehler generiert. Im aktuellen Ansichtslayout werden die Daten vom Clientkennung, dann nach Protokollquelle gruppiert. Da wir auf Anfragen filtern, enthält das Feld StatusCode 404, sehen wir nur die Server und Netzwerk-Ablaufverfolgungsdaten, nicht die Client-Protokolldaten.

Die folgende Abbildung zeigt einer bestimmten Anforderung, Get Blob-Vorgang 404 zurückgegeben, da das Blob nicht vorhanden ist. Beachten Sie, dass einige Spalten in der Standardansicht entfernt wurden, um die entsprechenden Daten anzuzeigen.

![Gefilterte Server- und Netzwerk-Ablaufverfolgungsprotokolle](./media/storage-e2e-troubleshooting/server-filtered-404-error.png)

Anschließend werden wir entsprechen dieser Anforderung Kundennummer Client-Protokolldaten zu Aktionen sehen Client wurde bei den Fehler. Sie können eine neue Analyse Rasteransicht für diese Sitzung eine zweite Registerkarte eröffnet Client-Protokolldaten anzeigen anzeigen:

1. Kopieren Sie den Wert des Felds **ClientRequestId** , in die Zwischenablage. Hierzu auswählen entweder Feld **ClientRequestId** suchen, mit der rechten Maustaste auf den Wert und **Kopieren 'ClientRequestId'**auswählen.
1. Auf der Multifunktionsleiste Symbolleiste wählen Sie **Neue Viewer**, dann **Analysetabelle** eine neue Registerkarte öffnen. Die neue Registerkarte zeigt alle Daten in den Protokolldateien ohne gruppieren, filtern oder Farbe Regeln.
2. Auf der Multifunktionsleiste Symbolleiste **Ansichtenlayout**auswählen, und wählen Sie dann **Alle .NET Client Spalten** Abschnitt **Azure-Speicher** . Dieses Layout anzeigen zeigt Daten vom Client Protokoll sowie Server- und Ablaufverfolgungsprotokolle. Standardmäßig ist es für die **MessageNumber** Spalte sortiert.
3. Nächsten suchen Sie Clientprotokoll für die Client-Anfrage-ID Wählen Sie auf der Multifunktionsleiste Symbolleiste **Nachrichten suchen**, und geben Sie einen benutzerdefinierten Filter auf die Client-ID im Feld **Suchen** . Verwenden Sie folgende Syntax für den Filter eigene Clientanfragen-ID angeben:

        *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"

Message Analyzer sucht und markiert den ersten Protokolleintrag den Suchkriterien entspricht, in dem die Client-Anfrage-ID Im Clientprotokoll sind mehrere Einträge für jedes Client-ID, eventuell Gruppieren im Feld **ClientRequestId** zu erleichtern, sie zusammen zu sehen. Die folgende Abbildung zeigt alle Nachrichten im Clientprotokoll für den angegebenen Client Anforderung-ID.

![Client-Protokoll zeigt 404-Fehler](./media/storage-e2e-troubleshooting/client-log-analysis-grid1.png)

Mithilfe der Daten in Layouts anzeigen in diesen zwei Registerkarten angezeigt, können Sie die Anforderungsdaten um festzustellen, was den Fehler verursacht haben analysieren. Sie können auch Anfragen anzeigen, die vor dieser, um festzustellen, ob ein Ereignis der 404-Fehler geführt haben kann. Beispielsweise können Sie Client-Protokolleinträge vor dieser Anforderung Kundennummer entscheiden, ob das Blob gelöscht wurden oder durch die Clientanwendung Aufrufen einer API CreateIfNotExists in einem Container oder BLOB-Fehler überprüfen. Im Clientprotokoll finden Sie im Feld **Beschreibung** der Blob-Adresse; Server und Netzwerk-Ablaufverfolgungsprotokolle werden diese Informationen im Feld **Zusammenfassung** angezeigt.

Wenn Sie die Adresse des BLOBs, die den Fehler 404 zurückgegeben kennen, können Sie weitere untersuchen. Suchen Sie die Protokolleinträge für andere Nachrichten auf das gleiche Blob zugeordnet, können Sie überprüfen, ob der Client zuvor die Entität gelöscht.

## <a name="analyze-other-types-of-storage-errors"></a>Andere Typen von Speicherfehlern analysieren

Mit Message Analyzer Ihre Daten analysieren können, können Sie weitere Typen von Fehlern mit der Ansicht analysieren Layouts Farbe Regeln und Suche und Filter. Die Tabelle listet einige Probleme, die auftreten können, und die Filterkriterien, die Sie verwenden können, finden sie. Informationen auf Filter und Message Analyzer Sprache Filtern finden Sie unter [Filtern von Nachrichtendaten](http://technet.microsoft.com/library/jj819365.aspx).

|    Zu...                                                                                               |    Verwenden Sie Filterausdruck...                                                                                                                                                                                                                                        |    Ausdruck gilt für Protokoll (Client, Server, Netzwerk, alle)    |
|------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
|    Unerwarteter bei Zustellung von Nachrichten in einer Warteschlange                                                              |    AzureStorageClientDotNetV4.Description enthält "Erneuter Fehler Vorgang."                                                                                                                                                                                |    Client                                                        |
|    HTTP-Zunahme der PercentThrottlingError                                                                       |    HTTP. Response.StatusCode == 500 & #124; & #124; HTTP. Response.StatusCode == 503                                                                                                                                                                                          |    Netzwerk                                                       |
|    PercentTimeoutError erhöhen                                                                               |    HTTP. Response.StatusCode == 500                                                                                                                                                                                                                             |    Netzwerk                                                       |
|    Zunahme der PercentTimeoutError (alle)                                                                         |    * StatusCode == 500                                                                                                                                                                                                                                          |    Alle                                                           |
|    PercentNetworkError erhöhen                                                                               |    AzureStorageClientDotNetV4.EventLogEntry.Level < 2                                                                                                                                                                                                          |    Client                                                        |
|    HTTP 403 (Verboten) Nachrichten                                                                                 |    HTTP. Response.StatusCode == 403                                                                                                                                                                                                                             |    Netzwerk                                                       |
|    HTTP 404 (nicht gefunden) Nachrichten                                                                                 |    HTTP. Response.StatusCode == 404                                                                                                                                                                                                                             |    Netzwerk                                                       |
|    404 (alle)                                                                                                     |    * StatusCode 404 ==                                                                                                                                                                                                                                          |    Alle                                                           |
|    Shared Access Signatur (SAS) Autorisierung Problem                                                             |    AzureStorageLog.RequestStatus == "SASAuthorizationError"                                                                                                                                                                                                     |    Netzwerk                                                       |
|    -409 (Konflikt) HTTP-Nachrichten                                                                                  |    HTTP. Response.StatusCode == 409                                                                                                                                                                                                                             |    Netzwerk                                                       |
|    409 (alle)                                                                                                     |    * StatusCode == 409                                                                                                                                                                                                                                          |    Alle                                                           |
|    Niedrige PercentSuccess oder Analytics Protokolleinträge arbeiten mit Buchungsstatus der ClientOtherErrors    |    AzureStorageLog.RequestStatus == "ClientOtherError"                                                                                                                                                                                                         |    Server                                                        |
|    Nagle-Warnung                                                                                               |    ((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1.5)) und (AzureStorageLog.RequestPacketSize < 1460) und (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200)        |    Server                                                        |
|    Zeitraum in Server- und Protokolle                                                                    |    #Timestamp > = 2014-10-20T16:36:38 und #Timestamp < = 2014-10-20T16:36:39                                                                                                                                                                                     |    Server, Netzwerk                                               |
|    Zeitraum in Server-Protokolle                                                                                |    AzureStorageLog.Timestamp > = 2014-10-20T16:36:38 und AzureStorageLog.Timestamp < = 2014-10-20T16:36:39                                                                                                                                                     |    Server                                                        |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum End-to-End-Szenarios in Azure Storage finden Sie hier:

- [Überwachung, diagnose und Problembehandlung von Microsoft Azure-Speicher](storage-monitoring-diagnosing-troubleshooting.md)
- [Speicheranalyse](http://msdn.microsoft.com/library/azure/hh343270.aspx)
- [Ein Speicherkonto in Azure-Portal überwachen](storage-monitor-storage-account.md)
- [Daten mit AzCopy-Befehlszeilenprogramm](storage-use-azcopy.md)
- [Für Microsoft Message Analyzer-Betrieb](http://technet.microsoft.com/library/jj649776.aspx)
