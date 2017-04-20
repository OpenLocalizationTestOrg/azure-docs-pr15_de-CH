<properties 
   pageTitle="Alarme in Protokollanalyse | Microsoft Azure"
   description="Alarme in Protokollanalyse können wichtige Informationen in Ihrem Repository OMS identifizieren und proaktiv benachrichtigt Sie Probleme oder aufrufen Aktionen zu korrigieren.  Dieser Artikel beschreibt das Erstellen einer Warnregel und Details Aktionen können sie."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="bwren" />

# <a name="alerts-in-log-analytics"></a>Alarme in Protokollanalyse

Alarme in Protokollanalyse Identifizieren wichtiger Informationen im OMS-Repository.  Warnregeln automatisch Protokoll Suchvorgänge nach einem Zeitplan ausführen und einen Warnung Datensatz erstellen, wenn die Ergebnisse bestimmte Kriterien entsprechen.  Die Regel kann automatisch führen Aktionen proaktiv benachrichtigt Sie der Warnung oder einem anderen Prozess aufgerufen.   

![Analytics Alerts anmelden](media/log-analytics-alerts/overview.png)

## <a name="creating-an-alert-rule"></a>Erstellen einer Warnregel
Erstellen eine Warnregel zunächst erstellen Sie ein Protokoll Suchen der Datensätze, die die Warnung aufrufen soll.  **Warnung** -Schaltfläche werden so erstellen und konfigurieren Sie die Regel dann verfügbar.

1.  Die OMS-Übersichtsseite klicken Sie auf **Protokoll suchen**.
2.  Erstellen einer neuen Protokolldatei Suchabfrage oder ein gespeichertes Protokoll suchen. 
3.  Klicken Sie auf **Warnung** oben auf der Seite **Regel hinzufügen** Bildschirm öffnen.
4. Finden Sie in den Tabellen unten für Details zu den Optionen zum Konfigurieren der Warnung.
5. Wenn Sie das Zeitfenster für die Regel angeben, wird die Anzahl der vorhandenen Datensätze, die die Suchkriterien für dieses Zeitfenster angezeigt.  Dadurch können Sie feststellen, wie oft die Geben Sie die Anzahl der Ergebnisse, die Sie erwarten.
4.  Klicken Sie auf **Speichern** , um die Regel abzuschließen.  Es beginnt sofort.


![Regel hinzufügen](media/log-analytics-alerts/add-alert-rule.png)

| Eigenschaft | Beschreibung |
|:--|:--|
| **Warnungsinformationen** | |
| Name |  Eindeutigen Namen für die Regel ein. |
| Schweregrad | Schweregrad der Warnung, die von dieser Regel erstellt wird. |
| Suchabfrage | Wählen Sie die aktuelle Abfrage oder wählen eine vorhandene gespeicherte Suche aus der Liste **aktuelle Suchabfrage verwenden** .  Im Textfeld wird die Abfragesyntax bereitgestellt, wo Sie sie ggf. ändern können.  |
| Zeitfenster | Gibt den Zeitraum für die Abfrage.  Die Abfrage gibt nur Datensätze, die innerhalb dieses Bereichs der aktuellen Uhrzeit erstellt wurden.  Dies kann ein Wert zwischen 5 Minuten und 24 Stunden sein.  Es sollte größer oder gleich der Warnung Frequenz.  <br><br> Beispielsweise werden Wenn das Zeitfenster auf 60 Minuten festgelegt und die Abfrage um 15 Uhr nur 15 Uhr bis 12 Uhr erstellte Datensätze zurückgegeben. |
| **Zeitplan** |     
| Schwellenwert | Kriterien für eine Warnung erstellen.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze diese Kriterien erfüllt, wird eine Warnung erstellt. |
| Häufigkeit der Warnung | Gibt an, wie oft die Abfrage ausgeführt werden soll.  Sie kann ein Wert zwischen 5 Minuten und 24 Stunden.  Gleich oder kleiner als das Zeitfenster entsprechen. |
| Warnung unterdrücken | Beim Einschalten Unterdrückung für die Regel werden Aktionen für die Regel für einen definierten Zeitraum nach dem Erstellen einer neuen Warnung deaktiviert.  Die Regel wird noch ausgeführt und Warnungsdatensätze erstellen, wenn die Kriterien erfüllt.  Dadurch wird Sie Zeit, um das Problem zu beheben, ohne doppelte Aktionen. |
| **Aktionen** | |
| E-Mail-Benachrichtigung | Geben Sie **Ja** Wenn eine e-Mail gesendet werden, wenn der Alarm ausgelöst werden soll. |
| Betreff    | Betreff der e-Mail.  Sie können den Textkörper der Nachricht nicht ändern. |
| Empfänger | Adressen aller e-Mail-Empfänger.  Wenn Sie mehr als eine Adresse angeben, trennen Sie die Adressen durch ein Semikolon (;). |
| Webhook | Geben Sie **Ja** , ein Webhook aufrufen, wenn der Alarm ausgelöst wird. |
| Webhook URL | Die URL der Webhook. |
| Benutzerdefinierte JSON-Nutzlast enthalten | Wählen Sie diese Option, wenn die Standard-Nutzlast mit benutzerdefinierten ersetzen soll. |
| Geben Sie benutzerdefinierte JSON-Nutzlast | Die benutzerdefinierte Nutzlast für den Webhook.  Siehe vorherigen Abschnitt. |
| Runbook | Geben Sie **Ja** , ein Runbook Azure Automatisierung gestartet, wenn der Alarm ausgelöst wird. |
| Wählen Sie ein runbook | Wählen Sie Runbook Runbooks Automatisierung Konto konfiguriert darin Automatisierung ab. |
| Führen Sie auf | Wählen Sie **Azure** Runbook in Azure Cloud ausgeführt.  Wählen Sie **Hybrid Worker** Runbook auf [Hybrid Runbook Worker](..\automation\automation-hybrid-runbook-worker.md) in Ihrer lokalen Umgebung ausführen. |


## <a name="manage-alert-rules"></a>Warnregeln verwalten
Sie können alle Warnregeln im Menü **Alerts** Protokollanalyse **Einstellungen**aufzulisten.  

![Alerts verwalten](./media/log-analytics-alerts/configure.png)

1. Wählen Sie in der Konsole OMS Kachel **Einstellungen aus**
2. Wählen Sie **Alarmereignisse**.

In dieser Ansicht können Sie mehrere Aktionen ausführen.

- Deaktivieren einer Regel wählen **Sie** daneben.
- Bearbeiten einer Regel auf das Bleistiftsymbol neben.
- Entfernen Sie eine Warnregel **X** daneben klicken. 


## <a name="setting-time-windows"></a>Zeitfenster festlegen 

### <a name="event-alerts"></a>Ereignisalarmen

Ereignisse gehören Datenquellen wie Windows-Ereignisprotokolle, Syslog und benutzerdefinierte protokolliert.  Möglicherweise möchten eine Warnung erstellen, wenn ein bestimmtes Fehlerereignis erstellt oder Erstellung mehrerer Fehler Ereignisse in einem bestimmten Zeitfenster.

Um ein einzelnes Ereignis aufmerksam zu machen, Anzahl der Ergebnisse größer als 0 und Häufigkeit und Zeitfenster auf 5 Minuten.  Das Ausführen die Abfrage alle 5 Minuten und prüfen, ob das Vorkommen eines einzelnen Ereignisses, die seit dem letzten der Abfrage ausführen erstellt wurde.  Mehr Häufigkeit kann zwischen Ereignis gesammelt und die Warnung erstellt wird.

Einige Programme können gelegentlichen Fehler protokollieren, der unbedingt eine Warnung auslösen sollte nicht  Die Anwendung kann z. B. wiederholen den Prozess, der das Fehlerereignis erstellt und erfolgreich wieder.  In diesem Fall sollten Sie nicht die Benachrichtigung erstellt, wenn mehrere Ereignisse in einem bestimmten Zeitfenster erstellt werden.  

In einigen Fällen möchten Sie eine Warnung ohne ein Ereignis zu erstellen.  Beispielsweise kann ein Prozess reguläre Ereignisse protokollieren um anzugeben, dass es ordnungsgemäß funktioniert  Wenn sie eines dieser Ereignisse in einem bestimmten Zeitfenster nicht protokolliert, sollte eine Warnung erstellt.  In diesem Fall legen Sie den Schwellenwert *kleiner als*1.

### <a name="performance-alerts"></a>Performance-Alarme

[Leistungsdaten](log-analytics-data-sources-performance-counters.md) werden als Datensätze in ähnliche Ereignisse OMS-Repository gespeichert.  Der Wert in jedem Datensatz ist der Mittelwert über die letzten 30 Minuten gemessen.  Möchten Sie benachrichtigt werden, wenn ein Leistungsindikator einen bestimmten Schwellenwert überschreitet, sollte dieser Schwellenwert in der Abfrage enthalten.

Wenn Sie benachrichtigt werden, wenn der Prozessor führt beispielsweise über 90 % für 30 Minuten, verwenden Sie eine Abfrage wie *Typ = Perf ObjectName = Prozessor CounterName = "% Prozessorzeit" Gegenwert > 90* und den Schwellenwert für die Warnregel *größer als 0*.  

 Da [Leistungsnachweis](log-analytics-data-sources-performance-counters.md) aggregiert alle 30 Minuten, unabhängig davon, wie oft jeder Leistungsindikator sammeln, kann ein Zeitfenster von weniger als 30 Minuten keine Datensätze zurück.  Das Zeitfenster in 30 Minuten festlegen wird sichergestellt, dass Sie einen Datensatz für jede verbundene Datenquelle abzurufen, den Durchschnitt in dieser Zeit darstellt.

## <a name="alert-actions"></a>Warnaktionen

Konfigurieren Sie alert Datensatz, sondern die Regel eine oder mehrere Aktionen automatisch ausführen.  Aktionen können proaktiv benachrichtigt Sie der Warnung oder rufen Sie einen Prozess, der versucht, das Problem zu beheben, das erkannt wurde.  Den folgenden Abschnitten werden die Aktionen, die derzeit verfügbar sind.

### <a name="email-actions"></a>E-Mail-Aktionen
E-Mail-Aktionen senden eine E-mail mit den Details der Warnung an einen oder mehrere Empfänger.  Geben Sie den Betreff der e-Mail, aber der Inhalt ist ein Standardformat Protokollanalyse erstellt.  Sie enthält zusammenfassende Informationen wie den Namen der Warnung neben bis zu zehn Datensätze durchsucht Protokoll.  Darüber hinaus einen Link zu einer protokollsuche in Protokollanalyse, die den gesamten Satz der Datensätze aus der Abfrage zurück.   Der Absender der e-Mail ist *Microsoft Operations Management Suite Team &lt; noreply@oms.microsoft.com *. 


### <a name="webhook-actions"></a>Webhook Aktionen

Webhook Aktionen können Sie einen externen Prozess über eine HTTP POST-Anforderung aufgerufen.  Aufgerufenen Dienst unterstützen Webhooks und bestimmen, wie die Payload verwendet werden sollte es empfängt.  Sie können auch eine REST-API aufrufen, die speziell Webhooks nicht unterstützt, solange die Anforderung in einem Format, das die API versteht.  Beispiele für die Verwendung einer Webhook als Antwort auf eine Warnung verwenden einen Dienst wie [Puffer](http://slack.com) zum Senden einer Nachricht mit den Details der Warnung oder ein Ereignis in einem Dienst wie [PagerDuty](http://pagerduty.com/).  

Erstellen einer Warnregel mit Webhook aufrufen, ein Beispieldienst umfassende steht unter [Webhooks Protokollanalyse Warnungen](log-analytics-alerts-webhooks.md).

Webhooks enthalten eine URL und eine Nutzlast in JSON zum externen Dienst gesendeten Daten formatiert.  Standardmäßig wird die Nutzlast, die Werte in der folgenden Tabelle enthalten.  Sie können eine eigene benutzerdefinierte diese Nutzlast ersetzen.  In diesem Fall können die Variablen in der Tabelle für jeden Parameter Sie ihren Wert in benutzerdefinierte Nutzlast enthalten.


| Parameter | Variable | Beschreibung |
|:--|:--|:--|
| AlertRuleName | #alertrulename | Name der Warnregel. |
| AlertThresholdOperator | #thresholdoperator | Schwellenwert-Operator für die Regel.  *Größer* oder *kleiner*. |
| AlertThresholdValue | #thresholdvalue | Schwellenwert für die Regel. |
| LinkToSearchResults | #linktosearchresults | Verknüpfen Sie mit Protokollanalyse Protokoll suchen, die Abfrage Datensätze zurückgibt, die die Warnung erstellt. |
| ResultCount  | #searchresultcount | Anzahl der Datensätze in den Suchergebnissen. |
| SearchIntervalEndtimeUtc  | #searchintervalendtimeutc | Endzeit für die Abfrage im UTC-Format. |
| SearchIntervalInSeconds | #searchinterval | Zeitfenster für die Regel. |
| SearchIntervalStartTimeUtc  | #searchintervalstarttimeutc | Startzeit für die Abfrage im UTC-Format. |
| Einfach | #einfach | Protokoll der Suchabfrage Warnregel verwendet. |
| Suchergebnisse | Siehe unten | Datensätze, die von der Abfrage im JSON-Format zurückgegeben.  Auf die ersten 5.000 Einträge beschränkt. |
| WorkspaceID | #workspaceid | ID des Arbeitsbereichs OMS. |


Beispielsweise können Sie die folgende benutzerdefinierte Nutzlast angeben, die einen einzelnen Parameter namens *Text*enthält.  Der Dienst diese Webhook ruft würde dieser Parameter erwartet.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Dieses Beispiel Nutzlast würde Folgendes beim Senden an die Webhook aufgelöst.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Um die Suchergebnisse in eine benutzerdefinierte Nutzlast enthalten, fügen Sie die folgende Zeile als Eigenschaft oberster Ebene in der Json-Nutzlast.  

    "IncludeSearchResults":true

Beispielsweise Erstellen benutzerdefinierte Nutzlast, die nur den Namen der Warnung und die Suchergebnisse enthält können folgende Sie. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Sie können ein vollständiges Beispiel zum Erstellen einer Warnregel mit Webhook zu einem externen Dienst am [Protokollanalyse Warnung Webhook Beispiel](log-analytics-alerts-webhooks.md)durchlaufen.

### <a name="runbook-actions"></a>Runbook Aktionen

Runbook Aktionen starten ein Runbook in Azure Automation.  Um diese Aktion zu verwenden, müssen Sie die [Automatisierung](log-analytics-add-solutions.md) in Ihrem Arbeitsbereich OMS installiert und konfiguriert.  Sie haben es installiert, wenn Sie eine neue Regel erstellen, wird eine Verknüpfung zu dessen Installation angezeigt.  Sie können Runbooks im Automation-Konto auswählen, die in der automatisierungslösung konfiguriert.

Starten mit [Webhook](../automation/automation-webhooks.md)Runbook Runbook Aktionen.  Beim Erstellen der Regel wird automatisch eine neue Webhook für Runbooks mit Namen erstellt **OMS Warnung Remediation** GUID folgen.  

Sie können nicht direkt Parameter des Runbooks füllen, aber [$WebhookData Parameter](../automation/automation-webhooks.md) enthält die Details der Warnung an, einschließlich der Ergebnisse der protokollsuche, die es erstellt.  Runbooks müssen **$WebhookData** als Parameter, die Eigenschaften der Warnung definieren.  Die Warnungsdaten steht im Json-Format in einer einzelnen Eigenschaft **Suchergebnisse** die **RequestBody** -Eigenschaft des **$WebhookData**.  Diese müssen mit den Eigenschaften in der folgenden Tabelle.


| Knoten | Beschreibung |
|:--|:--|
| ID         | Pfad und GUID der Suche. |
| __metadata | Informationen einschließlich der Anzahl der Datensätze und Status der Suchergebnisse. |
|  Wert     |  Separater Eintrag für jeden Datensatz in den Suchergebnissen.  Die Details des Eintrags entspricht die Eigenschaften und Werte des Datensatzes.   |

Beispielsweise würde folgende Runbook protokollsuche zurückgegebenen Datensätze extrahieren und verschiedene Eigenschaften basierend auf jeden Datensatz.  Beachten Sie, dass das Runbook **RequestBody** von Json konvertieren, sodass es als Objekte in PowerShell gearbeitet werden kann.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value
    
    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer
        
        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }
        
        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="alert-records"></a>Warnungsdatensätze

Warnungsdatensätze Warnregeln Protokollanalyse erstellt haben einen **Typ** der **Warnung** und eine **SourceSystem** von **OMS**.  Sie haben die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ          | *Warnung* |
| SourceSystem  | *OMS* |
| AlertSeverity | Der Schweregrad der Warnung. |
| RepeatCount     | Name der Warnung. |
| Abfrage         | Text der Abfrage, die ausgeführt wurde.  |
| QueryExecutionEndTime   | Ende des Zeitraums für die Abfrage. |
| QueryExecutionStartTime | Beginn des Zeitraums für die Abfrage.  |
| TimeGenerated | Datum und Uhrzeit der Erstellung die Benachrichtigung. |

Es gibt andere Warnungsdatensätze durch [Alert-Management-Lösung](log-analytics-solution-alert-management.md) und [Power BI exportiert](log-analytics-powerbi.md).  Diese alle **Typ** **Warnung** aber zeichnen sich durch ihre **SourceSystem**.




## <a name="next-steps"></a>Nächste Schritte

- Installation der [Alert-Management-Lösung](log-analytics-solution-alert-management.md) analysieren Alerts gegründet Protokollanalyse und Alarme von System Center Operations Manager (SCOM) erfasst.
- Weitere Informationen zur [Protokolldatei sucht](log-analytics-log-searches.md) , die Warnungen generiert werden können.
- Eine exemplarische Vorgehensweise für das [Konfigurieren einer Webook](log-analytics-alerts-webhooks.md) mit einer Regel abgeschlossen.  
- Informationen Sie zum [Runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) zur Behebung der Probleme Alerts zu schreiben.