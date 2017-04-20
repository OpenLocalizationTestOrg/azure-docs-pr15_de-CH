<properties
    pageTitle="Erste Schritte mit Azure Stream Analytics Daten von IoT-Geräten. | Microsoft Azure"
    description="IoT Sensor Tags und Datenströme Stream Analytics mit Echtzeit-"
    keywords="IOT-Lösung Iot Einstieg"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/19/2016"
    ms.author="jeffstok"
/>

# <a name="get-started-with-azure-stream-analytics-to-process-data-from-iot-devices"></a>Erste Schritte mit Azure Stream Analytics Daten von IoT-Geräten

In diesem Lernprogramm erfahren Sie, wie Stream-Verarbeitungslogik zum Sammeln von Daten von Internet der Dinge (IoT) Geräten erstellt. Wie schnell und kostengünstig erstellen, verwenden Sie einen Anwendungsfall realen Internet der Dinge (IoT).

## <a name="prerequisites"></a>Erforderliche Komponenten

-   [Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/)
-   Abfrage und Beispieldateien von [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## <a name="scenario"></a>Szenario

Contoso ist ein Unternehmen im Bereich Automatisierung ist Herstellungsverfahren vollständig automatisiert werden. Die Maschinen in dieser Anlage wurde Sensoren emittieren Datenströme in Echtzeit. In diesem Szenario wollen Boden Produktionsleiter Echtzeit Erkenntnisse Sensordaten Muster und Aktionen auf. Wir verwenden Stream Analytics Query Language (SAQL) über die Sensordaten, interessante Muster aus den eingehenden Datenstrom zu.

Hier werden Daten von einem Gerät Texas Instruments Sensor-Tag generiert.

![Texas Instruments Sensor-tag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Die Nutzlast der Daten im JSON-Format ist und sieht folgendermaßen aus:


    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

In einem realen Szenario könnten Sie Hunderte von Sensoren als Stream Ereignisse generiert haben. Idealerweise würde ein Gatewaygerät Code, um diese Ereignisse zu [Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/) oder [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/)push ausgeführt. Stream Analytics-Auftrag würde diese Ereignisse von Ereignis aufnehmen und Datenströme Echtzeitanalysen Abfragen ausführen. Die Ergebnisse dann konnte auf einen der [unterstützten Ausgaben](stream-analytics-define-outputs.md)senden.

Für einfache Bedienung bietet diese erste Schritte eine Beispiel-Datei, die von realen Tag Sensoren aufgezeichnet wurde. Sie können Abfragen für die Beispieldaten ausführen und Ergebnisse. In nachfolgenden Lernprogramme Sie lernen Eingaben Beruf Verbindung gibt und Azure Service bereitstellen.

## <a name="create-a-stream-analytics-job"></a>Erstellen Sie einen Stream Analytics-Auftrag

1. Klicken Sie in [Azure-Portal](http://portal.azure.com)auf das Pluszeichen, und geben Sie **STREAM ANALYTICS** im Text rechts. Wählen Sie in der Ergebnisliste **Stream Analytics-Auftrag** .

    ![Erstellt einen neuen Stream Analytics-Auftrag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

2. Geben Sie einen eindeutigen Namen, und überprüfen Sie, ob das Abonnement für Ihren Auftrag richtig ist. Dann erstellen Sie eine neue Ressourcengruppe oder wählen Sie eine vorhandene Abonnements.

3. Wählen Sie einen Speicherort für Ihr Projekt. Beschleunigen und Verringerung der Kosten im Data Transfer am gleichen Ressourcengruppe und beabsichtigte Speicherkonto auswählen empfiehlt.

    ![Erstellen Sie einen neuen Auftrag für Stream Analytics details](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)

    > [AZURE.NOTE] Sie sollten diese Speicherkonto nur einmal pro Region erstellen. Diese Speicher werden für alle Stream Analytics-Aufträge freigegeben, die in diesem Bereich erstellt wurden.

4. Kontrollkästchen Sie, um Ihre Arbeit auf dem Dashboard platzieren und dann auf **Erstellen**.

    ![Auftrag erstellt](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)

5. Eine 'Bereitstellung wurde gestartet..."sollte angezeigt werden. oben rechts im Browserfenster angezeigt. Bald ändert zu einem abgeschlossenen Fenster sich wie unten dargestellt.

    ![Auftrag erstellt](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

### <a name="create-an-azure-stream-analytics-query"></a>Erstellen einer Abfrage Azure Stream Analytics

Nachdem Ihr Auftrag erstellt Zeit es öffnen und eine Abfrage erstellen. Sie können Ihre Arbeit problemlos Kachel klicken zugreifen.

![Job-Kachel](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Klicken Sie im **Auftrag Topologie** auf **Abfrage** auf den Abfrage-Editor zu wechseln. Der **Abfrage** -Editor können Sie eine T-SQL-Abfrage eingeben, die die Transformation für die eingehenden Daten ausführt.

![Abfrage](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Abfrage: Raw archivieren

Die einfachste Form der Abfrage wird eine Pass-Through-Abfrage, die alle eingegebenen Daten bestimmten Ausgabe archiviert. Herunterladen der Beispieldatei [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) an einem Speicherort auf Ihrem Computer. 

1. Fügen Sie die Abfrage aus der Datei PassThrough.txt. 

    ![Test-Eingabestream](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

2. Klicken Sie auf die drei Punkte neben der Eingabe und Kontrollkästchen Sie **Beispieldaten aus Datei laden** .

    ![Test-Eingabestream](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)

3. Ein Bereich auf der rechten Seite als Ergebnis öffnet, diese Datendatei HelloWorldASA InputStream.json aus dem heruntergeladenen Speicherort auswählen und klicken Sie auf **OK** unten im Bereich.

    ![Test-Eingabestream](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)

4. Ausrüstung **Testen** im linken Bereich des Fensters klicken und Ihr Testabfrage der Stichprobendataset verarbeiten. Ein Fenster öffnet unterhalb der Abfrage die Verarbeitung abgeschlossen ist.

    ![Testergebnisse](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-the-data-based-on-a-condition"></a>Abfrage: Filtern der Daten auf der Grundlage einer Bedingung

Wir versuchen, die Ergebnisse auf der Grundlage einer Bedingung filtern. Wir möchten Ergebnisse für diese Ereignisse anzuzeigen, die aus "SensorA." Die Abfrage ist in der Datei Filtering.txt.

![Datenstrom filtern](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Beachten Sie, dass die Groß-/Kleinschreibung Abfrage einen Zeichenfolgenwert vergleicht. Klicken Sie auf **Test** Gang erneut, um die Abfrage auszuführen. Die Abfrage sollte 389 Zeilen aus 1860 Ereignisse zurück.

![Zweite Ausgabeergebnisse aus Abfrage](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-to-trigger-a-business-workflow"></a>Abfrage: Warnung auslösen eines

Lassen Sie die Abfrage weitere. Für jeden Sensor soll Durchschnittstemperatur pro 30 Sekunden überwachen und Ergebnisse nur, wenn die durchschnittliche Temperatur über 100 Grad. Wir schreiben die folgende Abfrage, und klicken Sie auf **Test** , um die Ergebnisse anzuzeigen. Die Abfrage ist in der Datei ThresholdAlerting.txt.

![30 Sekunden Filterabfrage](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Sie sollten jetzt sehen Ergebnisse mit nur 245 Zeilen und Namen der Sensoren ist der Durchschnitt gemäßigten größer als 100. Abfrage gruppiert nach **Dspl**, über ein **Fenster Tumbling** 30 Sekunden ist der Sensorname Stream von Ereignissen. Zeitliche Abfragen anzugeben wollen wir an den Fortschritt. Mithilfe der **TIMESTAMP** by haben wir die Spalte **OUTPUTTIME** , um zeitliche Berechnung aller Zeiten zuordnen angegeben. Ausführliche Informationen, die MSDN-Artikel [Zeitmanagement](https://msdn.microsoft.com/library/azure/mt582045.aspx) und [Windowing-Funktionen](https://msdn.microsoft.com/library/azure/dn835019.aspx).

### <a name="query-detect-absence-of-events"></a>Abfrage: Erkennen Sie keine Ereignisse

Wie können wir eine Abfrage zu wenige Ereignisse schreiben? Entdecken Sie das letzte Mal ein Sensor Daten gesendet und dann für die nächsten Minute nicht Ereignisse senden. Die Abfrage ist in der Datei AbsenseOfEvent.txt.

![Keine Ereignisse erkennen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

Hier verwenden wir eine **Linke ÄUßERE** Verknüpfung zu demselben Datenstream (Self-join). Für einen **inneren** Join ist ein Ergebnis nur zurückgegeben, wenn eine Übereinstimmung gefunden wird.  Wenn ein Ereignis von der linken Seite des Joins unerreicht ist, wird für eine **Linke ÄUßERE** Verknüpfung eine Zeile mit NULL für alle Spalten rechts zurückgegeben. Diese Technik ist sehr nützlich, keine Ereignisse gefunden. Sehen Sie unsere MSDN-Dokumentation für Informationen zu [Verknüpfen](https://msdn.microsoft.com/library/azure/dn835026.aspx).

## <a name="conclusion"></a>Abschluss

Dieses Lernprogramm soll veranschaulichen verschiedene Stream Analytics Abfragesprache Abfragen schreiben und Ergebnisse im Browser. Allerdings ist dies gerade erst. Mit Stream Analytics so viel mehr möglich. Stream Analytics unterstützt eine Vielzahl von Eingaben und Ausgaben und können sogar Funktionen in Azure Machine Learning zu einem robusten Werkzeug für die Analyse von Daten. Sie können zu mehr über Stream Analytics mithilfe unserer [learning Map](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/)starten. Weitere Informationen Abfragen finden Sie im Artikel zu [allgemeinen Abfragemuster](./stream-analytics-stream-analytics-query-patterns.md).
