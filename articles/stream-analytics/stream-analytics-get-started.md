<properties
    pageTitle="Erste Schritte mit Stream Analytics: Echtzeit-Erkennung | Microsoft Azure"
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

Informationen Sie zum Erstellen einer End-to-End-Lösung für Echtzeit-Erkennung mit Azure Stream Analytics. Bringen Sie Ereignisse an einen Hub Azure Ereignis schreiben Stream Analyseabfragen Aggregation oder Warnung und sendet die Ergebnisse an eine Senke Ausgabe Einblick über Daten in Echtzeit verarbeiten. Echtzeit Normalbetriebswerte für Telekommunikation fällt jedoch Technik wird auch für andere Betrugsversuche wie Kredit- oder Identität Diebstahl Szenarien geeignet.

Stream Analytics ist ein vollständig verwalteter Dienst niedriger Latenz hoch verfügbare, skalierbare komplexe Verarbeitung über Daten in der Cloud bereitstellen. Weitere Informationen finden Sie unter [Einführung in Azure Stream Analytics](stream-analytics-introduction.md).


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Szenario: Telekommunikations- und SIM Betrugsversuche in Echtzeit

Ein hat eine große Datenmengen für eingehende Anrufe. Das Unternehmen benötigt folgende Daten aus:
* Vergleichen Sie diese Daten auf eine verwaltbare Menge sowie Einsichten zur Verwendung des Kunden und Regionen.
* Erkennen Sie SIM Betrug (mehrere Aufrufe von derselben Identität, gleichzeitig aber an geographisch verschiedenen Orten) in Echtzeit, damit sie problemlos können Kunden benachrichtigen oder Dienst heruntergefahren.

Kanonische es Internet der Dinge (IoT) Szenarien unzählige Telemetrie Daten Sensor generiert wird, und Kunden sie aggregieren oder Warnung über Anomalien in Echtzeit.

## <a name="prerequisites"></a>Erforderliche Komponenten

- [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) aus dem Microsoft Download Center herunterladen 
- Optional: Quellcode Ereignisgenerator von [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/DataGenerators/TelcoGenerator)

## <a name="create-an-azure-event-hubs-input-and-consumer-group"></a>Erstellen Sie ein Azure Ereignis Hubs und Consumer Group

Beispiel-Anwendung Ereignisse generieren und ein Ereignis Hub zur Verarbeitung an Instanz in Echtzeit übertragen. Service Bus Ereignis Hubs sind die bevorzugte Methode des Ereignisses Einnahme für Stream Analytics und erfahren Sie mehr über Ereignis Hubs in [Azure Service Bus-Dokumentation](/documentation/services/service-bus/).

So erstellen Sie einen Ereignis-Hub

1.  Klicken Sie in der [Azure-Portal](https://manage.windowsazure.com/) **neu** > **App Services** > **Service Bus** > **Event Hub** > **Schnell erstellen**. Geben Sie Name, Region und neuen oder vorhandenen Namespace erstellen neue Ereignis-Hub.  
2.  Als bewährte Methode sollte ein einzelnes Ereignis Hub Verbrauchergruppe Auftrags Stream Analytics gelesen. Es führt Sie durch den Prozess der Erstellung einer Gruppe Consumer und [Verbrauchergruppen mehr lernen](https://msdn.microsoft.com/library/azure/dn836025.aspx)können. Erstellen Sie eine navigieren Sie zu der neu erstellten Event Hub klicken Sie **Verbrauchergruppen** , dann klicken Sie auf **Erstellen** am Ende der Seite und geben Sie einen Namen für Ihre Gruppe Consumer.
3.  Zugriff auf den Ereignis-Hub gewähren wir einen freigegebenen Richtlinien erstellen müssen.  Klicken Sie auf die Registerkarte **Konfigurieren** des Haupt-Ereignis.
4.  Erstellen Sie unter **Gemeinsame Richtlinien**eine neue Richtlinie mit Berechtigungen **Verwalten** .

    ![Freigegebene Zugriffsrichtlinien, einer Richtlinie mit Berechtigungen verwalten erstellen.](./media/stream-analytics-get-started/stream-ananlytics-shared-access-policies.png)

5.  Klicken Sie am unteren Rand der Seite **Speichern** .
6.  Navigieren Sie zum **Dashboard** klicken Sie auf **Informationen** am unteren Rand der Seite kopieren und Speichern der Verbindungsinformationen.

## <a name="configure-and-start-event-generator-application"></a>Konfigurieren Sie und Ereignis-Generator Anwendung

Wir haben eine Clientanwendung bereitgestellt, die Beispiel eingehenden Anruf Metadaten generieren und Event Hub drücken. Gehen Sie zum Einrichten dieser Anwendung.  

1.  Downloaden Sie die [Datei TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip). Entpacken Sie es in ein Verzeichnis.

    **Hinweis**: Windows blockiert die heruntergeladene Zip-Datei. Rechten Maustaste auf die Datei, und wählen Sie Eigenschaften. Wenn die Meldung "Diese Datei von einem anderen Computer wurde und zum Schutz dieses Computers blockiert." dann aktivieren Sie das Kontrollkästchen "Zulassen" und klicken Sie auf die Zip-Datei.

2.  Ersetzen Sie die Werte Microsoft.ServiceBus.ConnectionString und EventHubName in **telcodatagen.exe.config** mit Event Hub-Verbindungszeichenfolge und Namen.

    **Hinweis**: die Verbindungszeichenfolge kopiert von Azure Portal stellen den Namen der Verbindung am Ende. Entfernen Sie die "; EntityPath =<value>"aus dem Schlüssel hinzufügen = Feld.

3.  Starten Sie die Anwendung. Die Syntax lautet wie folgt:

   telcodatagen.exe [#NumCDRsPerHour] [SIM-Karte Betrug Wahrscheinlichkeit] [#DurationHours]

Im folgende Beispiel wird 1000 Ereignisse mit 20 Prozent betrug über 2 Stunden generieren.

    telcodatagen.exe 1000 .2 2

Datensätze an Ihrem Haupt-Ereignis wird angezeigt. Einige Felder, die wir in dieser Anwendung in Echtzeit Betrug Erkennung verwendet werden hier definiert:

| Datensatz | Definition |
| ------------- | ------------- |
| CallrecTime | Zeitstempel für die Startzeit Aufruf. |
| SwitchNum | Vermittlungsstelle zum Anruf herstellen. |
| CallingNum | Telefonnummer des Anrufers. |
| CallingIMSI | International Mobile Subscriber Identity (IMSI).  Eindeutiger Bezeichner des Aufrufers. |
| CalledNum | Telefonnummer der Empfänger des Anrufs. |
| CalledIMSI | International Mobile Subscriber Identity (IMSI).  Eindeutiger Bezeichner der Empfänger des Anrufs. |


## <a name="create-stream-analytics-job"></a>Stream Analytics-Auftrag erstellen
Jetzt haben wir einen Stream Telekommunikation Ereignisse können wir einen Stream Analytics-Auftrag einrichten dieser Ereignisse in Echtzeit analysiert.

### <a name="provision-a-stream-analytics-job"></a>Bereitstellung eines Stream Analytics-Auftrags

1.  Klicken Sie in der Azure-Portal auf **Neu > Data Services > Stream Analytics > Schnellerfassungsformular**.
2.  Geben Sie die folgenden Werte und dann auf **Stream Analytics-Auftrag erstellen**:

    * **Auftragsname**: Geben Sie einen Auftragsnamen ein.

    * **Region**: Markieren Sie den Bereich der Auftrag ausgeführt werden soll. Sollten Sie den Auftrag und dem Ereignis in derselben Region eine bessere Leistung zu gewährleisten und sicherzustellen, dass Sie keine zur Datenübertragung zwischen Zahlen.

    * **Konto**: Wählen Sie das Konto Azure-Speicher, der Daten für alle Stream Analytics innerhalb dieses Bereichs speichern möchten. Sie können ein vorhandenes Speicherkonto auswählen oder eine neue zu erstellen.

3.  Klicken Sie im linken Bereich der Stream Analytics-Aufträge auflisten **Stream Analytics** auf.

    ![Stream Analytics Service-Symbol](./media/stream-analytics-get-started/stream-analytics-service-icon.png)

4.  Das neue Projekt erscheint mit dem Status **erstellt**. Beachten Sie, dass am Ende der Seite auf die Schaltfläche **Start** deaktiviert ist. Sie müssen zunächst den Auftrag Auftrag Eingabe-, Ausgabe- und Abfrage konfigurieren.

### <a name="specify-job-input"></a>Job-Eingabe angeben
1.  Der Stream Analytics-Auftrags auf **Eingaben** vom oberen Rand der Seite und klicken Sie dann auf **Hinzufügen**. Das Dialogfeld führt Sie durch mehrere Schritte, um Ihre Eingabe einzurichten.
2.  Wählen Sie **Datenstrom aus**, und klicken Sie mit der rechte Maustaste.
3.  Wählen Sie **Ereignis-Hub**aus, und klicken Sie mit der rechte Maustaste.
4.  Eingeben oder auswählen auf der dritten Seite die folgenden Werte:

    * **Input Alias**: Geben Sie einen Anzeigenamen für diesen Auftrag wie *CallStream*eingeben. Beachten Sie, dass Sie diesen Namen in der Abfrage später verwenden werden.
    * **Ereignis-Hub**: Event Hub erstellen im gleichen Abonnement als Stream Analytics-Auftrags wählen Sie den Namespace im Ereignis ist.

    Ist Haupt-Ereignis in einem anderen Abonnement **Mit Event Hub ein anderes Abonnement** wählen und Informationen für **Service Bus Namespace**, **Hub Ereignisnamen** **Ereignis Hub Richtlinienname**, **Ereignis Hub Richtlinienschlüssel**und **Ereignisanzahl Hub Partition**manuell eingeben.

    * **Hub Ereignisname**: Wählen Sie den Namen des Ereignisses.

    * **Ereignis Hub Richtlinienname**: Wählen Sie Ereignis-Hub in diesem Lernprogramm erstellt.

    * **Hub Consumer Gruppe**: Geben Sie den Verbraucher Gruppe zuvor in diesem Lernprogramm erstellt.
5.  Klicken Sie auf die rechte Maustaste.
6.  Geben Sie die folgenden Werte:

    * **Ereignis Serialisierungsprogramm Format**: JSON
    * **Codierung**: UTF8
7.  Klicken Sie Kontrollkästchen dieser Quelle hinzufügen und überprüfen, ob der Stream Analytics Ereignis-Hub herstellen können.

### <a name="specify-job-query"></a>Job-Abfrage

Stream Analytics unterstützt einen einfachen, deklarativen Query-Modell zum Beschreiben von Umwandlungen in Echtzeit verarbeiten. Weitere Informationen über die Sprache finden Sie unter [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/dn834998.aspx). Dieses Lernprogramm hilft Ihnen erstellen und Testen Sie mehrere Abfragen über die Echtzeit-Daten-Stream.

#### <a name="optional-sample-input-data"></a>Optional: Beispiel Eingabedaten
Überprüfen Sie Ihre Abfrage aktuelle Daten Funktion der **Beispieldaten** Ereignisse aus der Stream und erstellen ein. JSON-Datei Ereignisse zum Testen.  Die folgenden Schritte zeigen, wie diese, und wir haben auch eine [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) Probe zu Testzwecken bereitgestellt.

1.  Wählen Sie Ihre Eingabe Event Hub und auf **Beispieldaten** am unteren Rand der Seite.
2.  Geben Sie im Dialogfeld eine **Startzeit** sammeln Daten und eine **Dauer** für wie viele zusätzlichen Daten verarbeiten.
3.  Klicken Sie auf Kontrollkästchen, um Stichproben von Daten aus der Eingabe beginnen.  Es dauert ein oder zwei Minuten für die Datendatei erstellt werden.  Wenn der Vorgang abgeschlossen ist, klicken Sie auf **Details** und downloaden und Speichern der. JSON-Datei, die generiert wird.

    ![Laden und Speichern von Daten in einer JSON-Datei](./media/stream-analytics-get-started/stream-analytics-download-save-json-file.png)

#### <a name="passthrough-query"></a>Pass-Through-Abfrage

Wenn Sie alle Ereignisse archivieren möchten, können eine Pass-Through-Abfrage Sie alle Felder in der Nutzlast des Ereignisses oder der Nachricht lesen. Führen Sie einer einfache Passthrough-Abfrage, Projekte alle Felder eines Ereignisses.

1.  Klicken Sie auf **Abfrage** vom oberen Seitenrand Job Stream Analytics.
2.  Fügen Sie folgenden Code-Editor:

        SELECT * FROM CallStream

    > Stellen Sie sicher, dass Name der Eingabequelle den Namen der Eingabe entspricht, die Sie zuvor angegeben.

3.  Klicken Sie unter Abfrage-Editor **Testen** .
4.  Geben Sie eine Testdatei eine, die mit den vorherigen Schritten erstellt oder verwenden Sie [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json).
5.  Klicken Sie auf die Schaltfläche Überprüfen und die Ergebnisse unterhalb der Abfragedefinition.

    ![Definition von Abfrageergebnissen](./media/stream-analytics-get-started/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Spalte-Projektion

Wir werden nun unten die zurückgegebenen Felder in eine kleinere Gruppe vergleichen.

1.  Ändern Sie die Abfrage in den Code-Editor:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  Der Abfrage-Editor die Ergebnisse der Abfrage klicken Sie auf **erneut ausführen** .

    ![Ausgabe im Abfrage-Editor.](./media/stream-analytics-get-started/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Anzahl der eingehenden Aufrufe Region: Tumbling Fenster mit

Um den Betrag zu vergleichen werden wir diese Anrufe pro Region eine [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) die Anzahl der Anrufe nach SwitchNum gruppiert alle 5 Sekunden zu nutzen.

1.  Ändern Sie die Abfrage in den Code-Editor:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Diese Abfrage verwendet das Schlüsselwort **Zeitstempel von** an ein Timestamp-Feld in der Nutzlast in die zeitliche Berechnung verwendet werden. Wenn dieses Feld nicht angegeben wurde, würde der Windowing Operation werden jedes Ereignis am Ereignis kamen. Siehe ["Ankunft Vs Anwendungszeit" im Stream Analytics Abfragen Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Beachten Sie, dass Sie einen Zeitstempel für das Ende jedes Fenster können mithilfe der **System.Timestamp** -Eigenschaft.

2.  Der Abfrage-Editor die Ergebnisse der Abfrage klicken Sie auf **erneut ausführen** .

    ![Abfrageergebnisse für Timestand](./media/stream-analytics-get-started/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>SIM-Erkennung mit einem selbst-Join

Um potenziell betrügerische Verwendung erkennen sehen wir für Anrufe aus demselben Benutzer jedoch an verschiedenen Orten in 5 Sekunden.  Wir [Verknüpfung](https://msdn.microsoft.com/library/azure/dn835026.aspx) dem Aufruf Stream mit sich selbst in diesen Fällen überprüfen.

1.  Ändern Sie die Abfrage in den Code-Editor:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  Der Abfrage-Editor die Ergebnisse der Abfrage klicken Sie auf **erneut ausführen** .

    ![Abfrageergebnisse einer Verknüpfung](./media/stream-analytics-get-started/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Ausgabe-Senke erstellen

Haben wir eine Ereignisstream ein Ereignis Hub input Events und eine Abfrage aufnehmen über den Stream Umwandlung definiert ist der letzte Schritt eine Ausgabe Senke für das Projekt definiert.  Ereignisse für betrügerische Verhalten schreiben Blob-Speicher.

Gehen Sie folgendermaßen vor, um einen Container für BLOB-Speicher erstellen, haben Sie bereits eine.

1.  Ein Speicherkonto verwenden oder erstellen Sie ein neues Speicherkonto durch Klicken auf **Neu > DATA SERVICES > STORAGE > SCHNELLERFASSUNGSFORMULAR** und wie.
2.  Wählen Sie das Speicherkonto, **Container** am oberen Rand der Seite auf und klicken Sie auf **Hinzufügen**.
3.  Geben Sie einen **Namen** für den Container und auf Blob für den öffentlichen **Zugriff** festgelegt.

## <a name="specify-job-output"></a>Auftrag Ausgabe

1.  Dem Stream Analytics-Auftrag auf **Ausgabe** vom oberen Rand der Seite und klicken Sie dann auf **Ausgabe hinzufügen**. Das Dialogfeld führt Sie durch eine Reihe von Schritten zum Einrichten der Ausgabe.
2.  Wählen Sie **BLOB-Speicher**, und klicken Sie mit der rechte Maustaste.
3.  Eingeben oder auswählen auf der dritten Seite die folgenden Werte:

    * **AUSGABEALIAS**: Geben Sie einen Anzeigenamen für diese Auftragsausgabe.
    * **Abonnement**: BLOB-Speicher erstellen im gleichen Abonnement als Stream Analytics-Auftrags wählen Sie **Storage-Konto von Abonnements verwenden**. Ist Ihr Speichersystem in einem anderen Abonnement **Speicherkonto verwenden ein anderes Abonnement** wählen und **SPEICHERKONTO** **STORAGE Konto KEY** **CONTAINER**Informationen manuell eingeben.
    * **Konto**: Wählen Sie den Namen des Speicherkontos.
    * **CONTAINER**: Wählen Sie den Namen des Containers.
    * **Präfix für DATEINAMEN**: Geben Sie ein Präfix beim Schreiben von BLOB-Ausgabe verwendet.

4.  Klicken Sie auf die rechte Maustaste.
5.  Geben Sie die folgenden Werte:

    * **Ereignis SERIALISIERUNGSPROGRAMM FORMAT**: JSON
    * **Codierung**: UTF8

6.  Klicken Sie Kontrollkästchen dieser Quelle hinzufügen und überprüfen, ob der Stream Analytics das Speicherkonto herstellen können.

## <a name="start-job-for-real-time-processing"></a>Auftrag für Echtzeit starten

Da ein Auftrag Eingabe, Abfrage und Ausgabe alle angegeben wurden, sind wir der Auftrag Stream Analytics für Echtzeit-Erkennung gestartet.

1.  **DASHBOARD**Auftrag klicken Sie am unteren Rand der Seite **Starten** .
2.  Wählen Sie im Dialogfeld **Auftrag starten** , und klicken Sie dann unten im Dialogfeld Kontrollkästchen. Der Status wechselt zu **Starten** und verschiebt in Kürze **ausgeführt**.

## <a name="view-fraud-detection-output"></a>Betrug Erkennung Ausgabe anzeigen

Verwenden Sie ein Tool wie [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/) oder [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) betrügerische Ereignisse schreiben Ihre Ausgabe in Echtzeit anzeigen.  

![Erkennung: betrügerische Ereignisse in Echtzeit angezeigt](./media/stream-analytics-get-started/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Support erhalten
Versuchen Sie [Azure Stream Analytics Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)für weitere Unterstützung.


## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
