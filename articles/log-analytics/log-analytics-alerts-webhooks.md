<properties
   pageTitle="Beispielprotokoll Analytics-Warnung webhook"
   description="Die Aktionen können Sie als Antwort auf eine Warnung Protokollanalyse gehört ein *Webhook*, der einen externen Prozess über eine einzelne HTTP-Anforderung aufrufen kann. Dieser Artikel führt durch eine Webhook-Aktion in einem Puffer mit Protokollanalyse Warnung erstellen."
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="webhooks-in-log-analytics-alerts"></a>Webhooks Protokollanalyse Warnungen

Die Aktionen können Sie als Antwort auf eine [Warnung Protokollanalyse](log-analytics-alerts.md) gehört ein *Webhook*, der einen externen Prozess über eine einzelne HTTP-Anforderung aufrufen kann.  Informationen zu Details der Alarme und Warnungen [in Protokollanalyse](log-analytics-alerts.md) webhooks

In diesem Artikel gehen wir über Webhook-Aktion in einer Protokollanalyse Warnung mit Pufferzeit ist ein messaging-Dienst erstellen.

>[AZURE.NOTE] Sie müssen ein Puffer Konto zum Ausführen dieses Beispiels.  Sie können ein kostenloses Konto bei [slack.com](http://slack.com)anmelden.

## <a name="step-1---enable-webhooks-in-slack"></a>Schritt 1: Webhooks im Puffer aktivieren
2.  Melden Sie am [slack.com](http://slack.com)Puffer an.
3.  Wählen Sie im Abschnitt **Kanäle** im linken Kanal.  Dies ist der Kanal, dem die Nachricht gesendet wird.  Sie können eine Standard-Kanäle oder **Allgemeine** **zufällig**auswählen.  In einem Produktionsszenario würden Sie wahrscheinlich einen speziellen Kanal wie **Criticalservicealerts**erstellen. <br>

    ![Pufferzeit Kanäle](media/log-analytics-alerts-webhooks/oms-webhooks01.png)

3. Klicken Sie auf **eine Anwendung hinzufügen oder benutzerdefinierte Integration** das Anwendungsverzeichnis öffnen.
3.  Geben Sie *Webhooks* in das Suchfeld ein, und wählen Sie dann **Eingehende WebHooks**. <br>

    ![Pufferzeit Kanäle](media/log-analytics-alerts-webhooks/oms-webhooks02.png)

4.  Klicken Sie neben dem Namen des Teams auf **Installieren** .
5.  Klicken Sie auf **Konfiguration**.
6.  Wählen Sie den Kanal, den Sie für dieses Beispiel verwenden und dann auf **eingehende WebHooks hinzufügen Integration**.  
6. Kopieren Sie den **URL Webhook**.  Sie werden diese in die Warnungskonfiguration einfügen. <br>

    ![Pufferzeit Kanäle](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Schritt 2 - Erstellen einer Warnregel im Protokollanalyse
1.  [Erstellen einer Warnregel](log-analytics-alerts.md) mit folgenden Optionen.
    - Abfrage:```    Type=Event EventLevelName=error ```
    - Diese Warnung prüfen alle: 5 Minuten
    - Die Anzahl der Ergebnisse: größer als 10
    - Über dieses Zeitfenster: 60 Minuten
    - Wählen Sie **Ja** für ** **Webhook** und** andere Aktionen.
7. Fügen Sie die Pufferzeit URL in das Feld **URL Webhook** .
8. Wählen Sie die Option **benutzerdefinierte JSON-Nutzlast enthalten**.
9. Pufferzeit erwartet eine Nutzlast in JSON mit einen Parameter namens *Text*formatiert.  Dies ist der Text, den in der Nachricht angezeigt wird erstellt.  Können Sie eine oder mehrere alert Parameter mit dem *#* symbol so wie im folgenden Beispiel.

    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```

    ![Beispiel JSON-Nutzlast](media/log-analytics-alerts-webhooks/oms-webhooks07.png)

9.  Klicken Sie auf **Speichern** , um die Regel zu speichern.

10. Warten Sie ausreichend Zeit für eine Warnung erstellt und anschließend eine Meldung ähnlich der folgenden werden Pufferzeit überprüfen.

    ![Beispiel Webhook in Puffer](media/log-analytics-alerts-webhooks/oms-webhooks08.png)


### <a name="advanced-webhook-payload-for-slack"></a>Erweiterte Webhook Nutzlast für Puffer

Sie können eingehende Nachrichten mit Pufferzeit umfassend anpassen. Weitere Informationen finden Sie auf der Website des Pufferzeit [Eingehende Webhooks](https://api.slack.com/incoming-webhooks) . Es folgt eine komplexere Nutzlast eine umfangreiche Nachricht mit Formatierung erstellen:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


Dies erzeugt eine Nachricht im Puffer ähnelt der folgenden.

![Beispielnachricht im Puffer](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Zusammenfassung

Mit dieser Regel vorhanden müssen Sie eine Nachricht an Pufferzeit jedes Mal, wenn die Kriterien erfüllt.  

Dies ist nur ein Beispiel einer Aktion als Antwort auf eine Warnung zu erstellen.  Sie können eine Aktion Webhook, die einem anderen externen Dienst aufruft, ein Runbook Aktion ein Runbook in Azure Automation starten oder eine e-Mail-Aktion eine Mail oder andere Empfänger erstellen.   

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie über [Alarme in Protokollanalyse](log-analytics-alerts.md) andere Aktionen.
- [Runbooks in Azure Automation erstellen](../automation/automation-webhooks.md) , die von einem Webhook aufgerufen werden kann.
