<properties
    pageTitle="Stream Analytics: Echtzeit-Erkennung | Microsoft Azure"
    description="Erfahren Sie, wie eine Lösung in Echtzeit Betrug Erkennung mit Stream Analytics erstellen. Verwenden Sie einen Ereignis-Hub für Echtzeit-Verarbeitung."
    keywords="Erkennen von Netzwerkanomalien Betrugsversuche, Erkennen von Netzwerkanomalien Echtzeit"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok" />



# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Erste Schritte mit Azure Stream Analytics: Echtzeit-Erkennung

Informationen Sie zum Erstellen einer End-to-End-Lösung für Echtzeit-Erkennung mit Azure Stream Analytics. Bringen Sie Ereignisse in Azure Ereignis Hubs, Schreiben Sie Stream Analyseabfragen Aggregation oder Warnung und senden Sie die Ergebnisse an eine Senke Ausgabe Einblicke über Daten in Echtzeit verarbeiten. Echtzeit Normalbetriebswerte für Telekommunikation erklärt, aber Technik wird auch für andere Betrugsversuche wie Kredit- oder Identität Diebstahl Szenarien geeignet.

Stream Analytics ist ein vollständig verwalteter Dienst niedriger Latenz hochverfügbaren, skalierbaren, komplexe Verarbeitung über Daten in der Cloud. Weitere Informationen finden Sie unter [Einführung in Azure Stream Analytics](stream-analytics-introduction.md).


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Szenario: Telekommunikations- und SIM Betrugsversuche in Echtzeit

Ein hat eine große Datenmengen für eingehende Anrufe. Das Unternehmen benötigt folgende Daten aus:

* Reduzieren von Daten zu einer überschaubaren und Erkenntnisse zur Verwendung des Kunden mit der Zeit und geografischen Regionen.
* Erkennen Sie SIM Betrug (mehrere Aufrufe von derselben Identität, gleichzeitig aber an geographisch verschiedenen Orten) in Echtzeit, damit das Unternehmen problemlos kann Kunden benachrichtigen oder Dienst heruntergefahren.

Kanonische Internet der Dinge (IoT) Szenarien haben viele Telemetrie oder Sensoren. Kunden möchten die Daten aggregieren oder Benachrichtigung über Anomalien in Echtzeit.

## <a name="prerequisites"></a>Erforderliche Komponenten

- [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) aus dem Microsoft Download Center herunterladen
- Optional: Quellcode Ereignisgenerator von [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator)

## <a name="create-azure-event-hubs-input-and-consumer-group"></a>Azure Ereignis Hubs Eingabe- und Consumer-Gruppe erstellen

Beispiel-Anwendung Ereignisse generieren und ein Ereignis Hubs zur Verarbeitung an Instanz in Echtzeit übertragen. Service Bus Ereignis Hubs sind die bevorzugte Methode der Einnahme für Stream Analytics Ereignis. Sie erhalten weitere Informationen zu Ereignis-Hubs in [Azure Service Bus-Dokumentation](/documentation/services/service-bus/).

So erstellen Sie einen Ereignis-hub

1.  Klicken Sie im [Azure-Portal](https://manage.windowsazure.com/) **neu** > **Anwendungsdienste** > **SERVICE BUS** > **EVENT HUB** > **Schnell erstellen**. Geben Sie Name, Region und neuen oder vorhandenen Namespace erstellen neue Ereignis-Hub.  
2.  Als bewährte Methode sollte ein einzelnes Ereignis Hub Verbrauchergruppe Auftrags Stream Analytics gelesen. Wir gehen Sie durch eine später erstellen. [Erfahren Sie mehr über Verbrauchergruppen](https://msdn.microsoft.com/library/azure/dn836025.aspx). Erstellen Sie eine gehen Sie an den Hub neu erstellte Ereignis die Registerkarte **VERBRAUCHERGRUPPEN** und auf **Erstellen** am Ende der Seite Geben Sie einen Namen für Ihre consumergruppe.
3.  Um Ereignis-Hub gewähren, müssen wir einen freigegebenen Richtlinien erstellen. Klicken Sie auf die Registerkarte **Konfigurieren** des Haupt-Ereignis.
4.  Erstellen Sie unter **Gemeinsame Richtlinien**eine neue Richtlinie, die Berechtigungen **Verwalten** .

    ![Freigegebene Zugriffsrichtlinien, einer Richtlinie mit Berechtigungen verwalten erstellen.](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-shared-access-policies.png)

5.  Klicken Sie am unteren Rand der Seite **Speichern** .
6.  Gehe zu **Dashboards**, klicken Sie auf **Informationen** am unteren Rand der Seite kopieren und Speichern der Verbindungsinformationen.

## <a name="configure-and-start-the-event-generator-application"></a>Konfigurieren Sie und starten Sie die Anwendung Ereignis-generator

Wir haben eine Clientanwendung bereitgestellt, die Beispiel eingehenden Anruf Metadaten generieren und Ereignis Hubs drücken. Gehen Sie folgendermaßen vor, um diese Anwendung einzurichten.  

1.  Downloaden Sie die [Datei TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip), und in ein Verzeichnis entpacken.

    > [AZURE.NOTE] Windows blockiert die heruntergeladene Zip-Datei. Maustaste auf die Datei und wählen Sie **Eigenschaften**. Wenn die "Diese Datei wurde von einem anderen Computer und zum Schutz von diesem Computer blockiert" angezeigt, aktivieren Sie das Kontrollkästchen **Zulassen** , und klicken auf die Zip-Datei anwenden.

2.  Ersetzen Sie die Werte Microsoft.ServiceBus.ConnectionString und EventHubName in telcodatagen.exe.config mit Ereignis Hub-Verbindungszeichenfolge und Namen.

    Die Verbindungszeichenfolge, die von Azure-Portal kopiert fügt den Namen der Verbindung am Ende. Entfernen Sie "; EntityPath =<value>"aus dem" Schlüssel hinzufügen = "Feld.

3.  Starten Sie die Anwendung. Die Syntax lautet wie folgt:

    telcodatagen.exe [#NumCDRsPerHour] [SIM-Karte Betrug Wahrscheinlichkeit] [#DurationHours]

Im folgende Beispiel werden 1.000 Ereignisse mit 20 Prozent betrug im Laufe von zwei Stunden generieren.

    telcodatagen.exe 1000 .2 2

Sie sehen Einträge Haupt-Ereignis an. Einige Felder, die wir in dieser Anwendung in Echtzeit Betrug Erkennung verwendet werden hier definiert:

| Datensatz | Definition |
| ------------- | ------------- |
| CallrecTime | Zeitstempel für die Startzeit Aufruf. |
| SwitchNum | Vermittlungsstelle zum Anruf herstellen. |
| CallingNum | Telefonnummer des Anrufers. |
| CallingIMSI | International Mobile Subscriber Identity (IMSI).  Eindeutiger Bezeichner des Aufrufers. |
| CalledNum | Telefonnummer der Empfänger des Anrufs. |
| CalledIMSI | International Mobile Subscriber Identity (IMSI).  Eindeutiger Bezeichner der Empfänger des Anrufs. |


## <a name="create-a-stream-analytics-job"></a>Erstellen Sie einen Stream Analytics-Auftrag
Jetzt haben wir einen Stream Telekommunikation Ereignisse können wir einen Stream Analytics-Auftrag einrichten dieser Ereignisse in Echtzeit analysiert.

### <a name="provision-a-stream-analytics-job"></a>Bereitstellung eines Stream Analytics-Auftrags

1.  Klicken Sie im Azure-Portal **neu** > **DATA SERVICES** > **STREAM ANALYTICS** > **Schnell erstellen**.
2.  Geben Sie die folgenden Werte und dann auf **STREAM ANALYTICS-Auftrag erstellen**:

    * **AUFTRAGSNAME**: Geben Sie einen Auftragsnamen ein.

    * **REGION**: Markieren Sie den Bereich der Auftrag ausgeführt werden soll. Sollten Sie den Auftrag und dem Ereignis in derselben Region eine bessere Leistung zu gewährleisten und sicherzustellen, dass Sie keine zur Datenübertragung zwischen Zahlen.

    * **Konto**: Wählen Sie die Option der Azure-Speicher, die Daten für alle Stream Analytics speichern, die innerhalb dieses Bereichs ausgeführt werden soll. Sie können ein vorhandenes Speicherkonto oder einen neuen erstellen.

3.  Klicken Sie im linken Bereich der Stream Analytics-Aufträge auflisten **STREAM ANALYTICS** auf.

    ![Stream Analytics Service-Symbol](./media/stream-analytics-real-time-fraud-detection/stream-analytics-service-icon.png)

    Das neue Projekt erscheint mit dem Status **erstellt**. Beachten Sie, dass am Ende der Seite auf die Schaltfläche **START** deaktiviert ist. Sie müssen zunächst den Auftrag Auftrag Eingabe-, Ausgabe- und Abfrage konfigurieren.

### <a name="specify-job-input"></a>Job-Eingabe angeben
1.  In Ihrem Auftrag Stream Analytics auf **EINGABEN** am oberen Rand der Seite und klicken Sie dann auf **Hinzufügen**. Das Dialogfeld führt Sie durch mehrere Schritte, um Ihre Eingabe einzurichten.
2.  Klicken Sie mit der rechte Maustaste auf **DATENSTROM**
3.  Klicken Sie mit der rechte Maustaste auf **EVENT HUB**
4.  Eingeben oder auswählen auf der dritten Seite die folgenden Werte:

    * **Eingabe-ALIAS**: Geben Sie einen Anzeigenamen, wie *CallStream*für diesen Auftrag. Beachten Sie, dass Sie diesen Namen in der Abfrage später verwenden werden.

    * **Ereignis-HUB**: Ereignis-Hub, der Sie erstellt im gleichen Abonnement als Stream Analytics-Auftrags wählen Sie den Namespace im Ereignis ist.

        Haupt-Ereignis in einem anderen Abonnement ist wählen Sie **Mit Event Hub ein anderes Abonnement**und geben Sie dann manuell **SERVICE BUS-NAMESPACE**, **HUB EREIGNISNAMEN** **Ereignis HUB RICHTLINIENNAME**, **Ereignis HUB RICHTLINIENSCHLÜSSEL**und **HUB PARTITION EREIGNISANZAHL**.

    * **HUB EREIGNISNAME**: Wählen Sie Ereignis-Hub.

    * **Ereignis HUB RICHTLINIENNAME**: Wählen Sie Ereignis-Hub, der zuvor in diesem Lernprogramm erstellt.

    * **HUB CONSUMER Gruppe**: Geben Sie den Namen der Verbrauchergruppe, die zuvor in diesem Lernprogramm erstellt.

5.  Klicken Sie auf die rechte Maustaste.
6.  Geben Sie die folgenden Werte:

    * **Ereignis SERIALISIERUNGSPROGRAMM FORMAT**: JSON
    * **Codierung**: UTF8
7.  Klicken Sie **Überprüfen** diese Quelle hinzufügen und überprüfen, ob der Stream Analytics Ereignis-Hub herstellen können.

### <a name="specify-job-query"></a>Job-Abfrage

Stream Analytics unterstützt einen einfachen, deklarativen Query-Modell, die Transformationen in Echtzeit Verarbeitung beschreibt. Weitere Informationen über die Sprache finden Sie unter [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/dn834998.aspx). Dieses Lernprogramm hilft Ihnen erstellen und Testen Sie mehrere Abfragen über die Echtzeit-Daten-Stream.

#### <a name="optional-sample-input-data"></a>Optional: Beispiel Eingabedaten
Überprüfen Sie Ihre Abfrage aktuelle Daten Funktion der **BEISPIELDATEN** Ereignisse aus der Stream und erstellen ein. JSON-Datei Ereignisse zum Testen.  Die folgenden Schritte zeigen dazu. Wir haben auch eine Beispieldatei [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) zu Testzwecken bereitgestellt.

1.  Wählen Sie der Hub Ereigniseingang aus und dann auf **BEISPIELDATEN** am unteren Rand der Seite.
2.  Geben Sie im angezeigten Dialogfeld **STARTZEIT** um sammeln Daten und eine **Dauer** für wie viele zusätzlichen Daten verarbeiten.
3.  Klicken Sie **Überprüfen** Samplingdaten der Eingabe beginnen.  Es dauert ein oder zwei Minuten für die Datendatei erstellt werden.  Wenn der Vorgang abgeschlossen ist, klicken Sie auf **DETAILS**, herunterladen Sie die generierten. JSON-Datei und speichern Sie sie.

    ![Laden und Speichern von Daten in einer JSON-Datei](./media/stream-analytics-real-time-fraud-detection/stream-analytics-download-save-json-file.png)

#### <a name="pass-through-query"></a>Pass-Through-Abfrage

Wenn Sie alle Ereignisse archivieren möchten, können eine Pass-Through-Abfrage Sie alle Felder in der Nutzlast des Ereignisses oder der Nachricht lesen. Führen Sie, um einer einfachen Pass-Through-Abfrage, projiziert die Felder eines Ereignisses.

1.  Klicken Sie auf **Abfrage** vom oberen Seitenrand Job Stream Analytics.
2.  Fügen Sie folgenden Code-Editor:

        SELECT * FROM CallStream

    > [AZURE.IMPORTANT] Stellen Sie sicher, dass der Name der Datenquelle mit dem Namen der Eingabe übereinstimmt, den Sie zuvor angegeben.

3.  Klicken Sie unter Abfrage-Editor **Testen** .
4.  Geben Sie eine Testdatei. Verwenden Sie eine mit den vorherigen Schritten erstellt oder verwenden Sie [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/SampleDataFiles/Telco.json).
5.  Klicken Sie auf die Schaltfläche **Suchen** , und die Ergebnisse unterhalb der Abfragedefinition.

    ![Definition von Abfrageergebnissen](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Spalte-Projektion

Wir werden jetzt die zurückgegebenen Felder einer kleineren Gruppe reduzieren.

1.  Ändern Sie die Abfrage in den Code-Editor:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  Der Abfrage-Editor die Ergebnisse der Abfrage klicken Sie auf **erneut ausführen** .

    ![Ausgabe im Abfrage-Editor.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Anzahl der eingehenden Aufrufe Region: Tumbling Fenster mit

Um die Anzahl der Anrufe pro Region zu vergleichen, verwenden wir [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) , die Anzahl der Anrufe alle fünf Sekunden nach **SwitchNum** gruppiert werden.

1.  Ändern Sie die Abfrage in den Code-Editor:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Diese Abfrage verwendet das Schlüsselwort **Zeitstempel von** an ein Timestamp-Feld in der Nutzlast in die zeitliche Berechnung verwendet werden. Wenn dieses Feld nicht angegeben wurde, würde Windowing-Vorgang ausgeführt werden, mithilfe der Zeit, die jedes Ereignis Event Hub eingetroffen. Siehe ["Ankunft Vs Anwendungszeit" im Stream Analytics Abfragen Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Beachten Sie, dass Sie einen Zeitstempel für das Ende jedes Fenster können mithilfe der **System.Timestamp** -Eigenschaft.

2.  Der Abfrage-Editor die Ergebnisse der Abfrage klicken Sie auf **erneut ausführen** .

    ![Abfrageergebnisse für Timestand](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>SIM-Erkennung mit einem selbst-Join

Zum Identifizieren von betrügerischen Verwendung erfahren für Aufrufe, die von demselben Benutzer jedoch an verschiedenen Orten in 5 Sekunden stammen.  Wir [Verknüpfung](https://msdn.microsoft.com/library/azure/dn835026.aspx) dem Aufruf Stream mit sich selbst in diesen Fällen überprüfen.

1.  Ändern Sie die Abfrage in den Code-Editor:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  Der Abfrage-Editor die Ergebnisse der Abfrage klicken Sie auf **erneut ausführen** .

    ![Abfrageergebnisse einer Verknüpfung](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Ausgabe-Senke erstellen

Haben wir eine Ereignisstream ein Ereignis Hub Ereignisse und eine Abfrage über den Stream Umwandlung Aufnahme Eingabe definiert ist der letzte Schritt eine Ausgabe Senke für das Projekt definiert. Ereignisse für betrügerische Verhalten schreiben Azure Blob-Speicher.

Gehen Sie zum Erstellen eines Containers für BLOB-Speicher, wenn Sie noch keines haben.

1.  Ein Storage-Konto verwenden oder erstellen Sie ein neues Speicherkonto durch Klicken auf **Neu > DATA SERVICES > STORAGE > SCHNELLERFASSUNGSFORMULAR**, und folgen.
2.  Wählen Sie das Speicherkonto, **Container** am oberen Rand der Seite auf und klicken Sie auf **Hinzufügen**.
3.  Geben Sie einen **Namen** für den Container und auf **Blob für den öffentlichen** **Zugriff** festgelegt.

## <a name="specify-job-output"></a>Auftrag Ausgabe

1.  In Ihrem Auftrag Stream Analytics auf **Ausgabe** am oberen Rand der Seite und klicken Sie dann auf **Ausgabe hinzufügen**. Das Dialogfeld führt Sie durch mehrere Schritte zum Einrichten der Ausgabe.
2.  Klicken Sie mit der rechte Maustaste auf **BLOB-Speicher**
3.  Eingeben oder auswählen auf der dritten Seite die folgenden Werte:

    * **AUSGABEALIAS**: Geben Sie einen Anzeigenamen für diese Auftragsausgabe.
    * **Abonnement**: BLOB-Speicher, die Sie erstellt im gleichen Abonnement als Stream Analytics-Auftrag, klicken Sie auf **Storage-Konto von Abonnements verwenden**. Ist Ihr Speichersystem in einem anderen Abonnement auf **Speicherkonto aus einem anderen Abonnement verwenden**und geben Sie Informationen für das **SPEICHERKONTO** **SPEICHERSCHLÜSSEL Konto**und **CONTAINER**manuell ein.
    * **Konto**: Wählen Sie den Namen des Speicherkontos.
    * **CONTAINER**: Wählen Sie den Namen des Containers.
    * **Präfix für DATEINAMEN**: Geben Sie ein Präfix beim Schreiben von BLOB-Ausgabe verwendet.

4.  Klicken Sie auf die rechte Maustaste.
5.  Geben Sie die folgenden Werte:

    * **Ereignis SERIALISIERUNGSPROGRAMM FORMAT**: JSON
    * **Codierung**: UTF8

6.  Klicken Sie **Überprüfen** diese Quelle hinzufügen und überprüfen, ob der Stream Analytics das Speicherkonto herstellen können.

## <a name="start-job-for-real-time-processing"></a>Auftrag für Echtzeit-Verarbeitung starten

Da ein Auftrag Eingabe, Abfrage und Ausgabe alle angegeben wurden, sind wir der Auftrag Stream Analytics für Echtzeit-Erkennung gestartet.

1.  **DASHBOARD**Auftrag klicken Sie am unteren Rand der Seite **Starten** .
2.  Klicken Sie im Dialogfeld, das geöffnet wird auf **Auftrag starten**und klicken Sie am unteren Rand des Dialogfelds auf die Schaltfläche **Suchen** . Der Status wechselt zu **Starten** und ändert in Kürze **ausgeführt**.

## <a name="view-fraud-detection-output"></a>Betrug Erkennung Ausgabe anzeigen

Verwenden Sie ein Tool wie [Azure Storage Explorer](http://storageexplorer.com/) oder [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) betrügerische Ereignisse schreiben Ihre Ausgabe in Echtzeit anzeigen.  

![Erkennung: betrügerische Ereignisse in Echtzeit angezeigt](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Support erhalten
Versuchen Sie [Azure Stream Analytics Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)für weitere Unterstützung.


## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
