<properties 
    pageTitle="Fahrzeug Telemetrie Analytics Lösung Playbook: tief in die Lösung Eintauchen | Microsoft Azure" 
    description="Verwenden Sie die Cortana Intelligenz in Echtzeit und prädiktive Einblicke Fahrzeug Gesundheits-und fahren Gewohnheiten." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Fahrzeug Telemetrie Analytics Lösung Playbook: tiefen Einblick in die Lösung

Dieses **Menü** Links zu Abschnitten dieses Playbook: 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Dieser Abschnitt einen Drilldown in Stufen in der Lösungsarchitektur und Hinweise für die Anpassung dargestellt. 

## <a name="data-sources"></a>Datenquellen

Die Lösung verwendet zwei unterschiedliche Datenquellen:

- **simulierte fahrzeugsignale und Diagnose Dataset** und 
- **Fahrzeug-Katalog**

Ein Fahrzeug Telematik-Simulator ist Bestandteil dieser Lösung. Diagnoseinformationen ausgegeben und signalisiert Zeit des Fahrzeugs und treibenden Muster zu einem bestimmten Zeitpunkt entspricht. Klicken Sie auf [Fahrzeug Telematik Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) herunterladen **Fahrzeug Telematik Simulator Visual Studio-Lösung** für Ihre Bedürfnisse anpassen. Fahrzeug-Katalog enthält einem Verweisdataset mit VIN auf Zuordnung.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig2-vehicle-telematics-simulator.png)

*Abbildung 2 – Fahrzeug Telematik Simulator*

Dies ist eine JSON-formatierte Dataset, das folgende Schema enthält.

Spalte | Beschreibung | Werte 
 ------- | ----------- | --------- 
FAHRZEUG-IDENTIFIZIERUNGSNUMMER | Zufällig generierte Fahrzeug-Identifizierungsnummer | Dies ist eine Masterliste mit 10.000 zufällig generierten Fahrzeugnummer abgerufen.
Temperatur | Die Temperatur, in dem das Fahrzeug fahren | Zufällig generierte Zahl von 0 bis 100
Motor | Die Temperatur des Motors des Fahrzeugs | Zufällig generierte Zahl von 0 bis 500
Geschwindigkeit | Die Drehzahl des Motors an der Fahrzeug fahren | Zufällig generierte Zahl von 0 bis 100
Kraftstoff | Kraftstoffmenge des Fahrzeugs | Zufällig generierte Zahl von 0 bis 100 (Kraftstoff Prozentsatz angibt)
EngineOil | Motoröl des Fahrzeugs | Zufällig generierte Zahl von 0 bis 100 (Engine Oil Prozentsatz angibt)
Reifendruck | Reifendruck des Fahrzeugs | Zufällig generierte Zahl von 0 bis 50 (Prozentsatz Reifen Druck angibt)
Kilometerstand | Kilometerstand des Fahrzeugs | Zufällig generierte Zahl zwischen 0-200000
Accelerator_pedal_position | Fahrpedalstellung des Fahrzeugs | Zufällig generierte Zahl von 0 bis 100 (Accelerator Prozentsatz angibt)
Parking_brake_status | Gibt an, ob das Fahrzeug geparkt ist | True oder False
Headlamp_status | Gibt an, wo der Scheinwerfer auf ist | True oder False
Brake_pedal_status | Gibt an, ob das Bremspedal gedrückt wird | True oder False
Transmission_gear_position | Die Übertragung gangposition des Fahrzeugs | Status: zuerst zweiten Sie, dritten, vierten, fünften, sechsten, siebten achten
Ignition_status | Gibt an, ob das Fahrzeug ausgeführt oder beendet wird. | True oder False
Windshield_wiper_status | Gibt an, ob die Scheibenwischer aktiviert ist | True oder False
ABS | Gibt an, ob ABS aktiviert ist | True oder False
Zeitstempel | Der Zeitstempel der Datenpunkt Erstellung | Datum
Ort | Die Position des Fahrzeugs | 4 Städten in dieser Lösung: Bellevue, Redmond, Sammamish, Seattle


Fahrzeug Modell Verweisdataset enthält VIN der Zuordnung. 

FAHRZEUG-IDENTIFIZIERUNGSNUMMER | Modell |
--------------|------------------
FHL3O1SA4IEHB4WU1 | Limousine |
8J0U8XCPRGW4Z3NQE | Hybrid |
WORG68Z2PLTNZDBI7 | Familie Limousine |
JTHMYHQTEPP4WBMRN | Limousine |
W9FTHG27LZN1YWO0Y | Hybrid |
MHTP9N792PHK08WJM | Familie Limousine |
EI4QXI2AXVQQING4I | Limousine |
5KKR2VB4WHQH97PF8 | Hybrid |
W9NSZ423XZHAONYXB | Familie Limousine |
26WJSGHX4MA5ROHNL | Konvertiert werden kann. |
GHLUB6ONKMOSI7E77 | Kombi |
9C2RHVRVLMEJDBXLP | Kompakter PKW |
BRNHVMZOUJ6EOCP32 | Kleine Geländewagen |
VCYVW0WUZNBTM594J | Sportwagen |
HNVCE6YFZSA5M82NY | Mittlere Geländewagen |
4R30FOR7NUOBL05GJ | Kombi |
WYNIIY42VKV6OQS1J | Große Geländewagen |
8Y5QKG27QET1RBK7I | Große Geländewagen |
DF6OX2WSRA6511BVG | Coupé |
Z2EOZWZBXAEW3E60T | Limousine |
M4TV6IEALD5QDS3IR | Hybrid |
VHRA1Y2TGTA84F00H | Familie Limousine |
R0JAUHT1L1R3BIKI0 | Limousine |
9230C202Z60XX84AU | Hybrid |
T8DNDN5UDCWL7M72H | Familie Limousine |
4WPYRUZII5YV7YA42 | Limousine |
D1ZVY26UV2BFGHZNO | Hybrid |
XUF99EW9OIQOMV7Q7 | Familie Limousine
8OMCL3LGI7XNCC21U | Konvertiert werden kann. |
…….  |   |


### <a name="to-generate-simulated-data"></a>Simulierte Daten generieren
1.  Klicken Sie das Simulator-Paket den Pfeil oben rechts des Fahrzeugs Telematik Simulator-Knotens. Speichern Sie, und extrahieren Sie die Dateien lokal auf Ihrem Computer. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig3-vehicle-telemetry-blueprint.png)*Abbildung 3 – Fahrzeug Telemetrie Analytics Solution Blueprint*

2.  Gehen Sie auf dem lokalen Computer zu dem Ordner, in dem das Fahrzeug Telematik Simulator Paket extrahiert. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-simulator-folder.png)*Abbildung 4 – Fahrzeug Telematik Simulator-Ordner*

3.  Führen Sie die Anwendung **CarEventGenerator.exe**.

### <a name="references"></a>Referenzen

[Fahrzeug Telematik Simulator Visual Studio-Lösung](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Azure Event Hub](https://azure.microsoft.com/services/event-hubs/)

[Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)


## <a name="ingestion"></a>Aufnahme
Kombinationen von Azure Ereignis Hubs und Stream Analytics Data Factory genutzt, um fahrzeugsignale, die Diagnoseereignisse aufnehmen und und Analysen. Diese Komponenten werden erstellt und als Teil der Bereitstellung der Lösung. 

### <a name="real-time-analysis"></a>Echtzeit-Analysen
Fahrzeug Telematik Simulator generierten Ereignisse werden Event Hub mit dem Ereignis Hub SDK veröffentlicht. Stream Analytics-Auftrags nimmt diese Ereignisse vom Event Hub und verarbeitet die Daten in Echtzeit analysieren den Zustand des Fahrzeugs. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-event-hub-dashboard.png) 

*Abbildung 5: Event Hub-dashboard*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Abbildung 6: Stream Analytics Auftrag Datenverarbeitung*

Der Stream Analytics-Auftrag;

- nimmt Daten vom Event Hub 
- Führt einen Join mit Referenzdaten der entsprechenden Modell Fahrzeug-Fahrzeug-Identifizierungsnummer zuordnen 
- Speichert sie in Azure BLOB-Speicher für umfangreiche Batch Analysen. 

Stream Analytics Abfrage zum Beibehalten von Daten in Azure BLOB-Speicher. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Abbildung 7: Stream Analytics auftragsabfrage für Daten-Erfassung*

### <a name="batch-analysis"></a>Batchanalyse
Wir sind auch zusätzliche Volume simulierte Fahrzeug und Diagnose Dataset für umfangreichere Batch Analytics generieren. Dies ist erforderlich, um eine gute repräsentative Volumen Stapelverarbeitung sicherzustellen. Zu diesem Zweck verwenden eine Pipeline mit dem Namen "PrepareSampleDataPipeline" im Workflow Azure Data Factory wir ein Jahr Wert simulierte Fahrzeug und Diagnose Dataset generieren. Klicken Sie auf [benutzerdefinierte Aktivität "Data Factory"](http://go.microsoft.com/fwlink/?LinkId=717077) herunterladen "Data Factory" benutzerdefinierte DotNet Aktivität Visual Studio-Lösung für Ihre Bedürfnisse anpassen. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Abbildung 8: Vorbereiten der Beispieldaten für Batch-Verarbeitung Workflow*

Die Pipeline besteht aus einer benutzerdefinierten ADF .net Aktivität hier:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Abbildung 9: PrepareSampleDataPipeline*

Sobald die Pipeline erfolgreich ausgeführt und Dataset "RawCarEventsTable" wird ein Jahr Wert simulierte Fahrzeug Signale und Diagnose "Bereit" markiert werden Daten erzeugt. Sie sehen die folgenden Ordner und das Speicherkonto unter dem Container "Connectedcar" erstellt:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Abbildung 10: PrepareSampleDataPipeline-Ausgabe*

### <a name="references"></a>Referenzen

[Azure Event Hub SDK Stream Einnahme](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Azure Data Factory Verlagerung Funktionen](../data-factory/data-factory-data-movement-activities.md)
[Azure Data Factory DotNet Aktivität](../data-factory/data-factory-use-custom-activities.md)

[Azure Data Factory DotNet Aktivität visual Studio-Projektmappe für die Vorbereitung von Beispieldaten](http://go.microsoft.com/fwlink/?LinkId=717077) 


## <a name="partition-the-dataset"></a>Partitionieren Sie das dataset

Der Schritt zur Vorbereitung in einem Jahr-Monat-Format partitioniert raw halbstrukturierten fahrzeugsignale und Diagnose Dataset. Diese Aufteilung fördert effizientere Abfragen und skalierbaren Langzeitspeicher Fehler über eine BLOB-Konto zu aktivieren, wie das erste Konto füllt. 

>[AZURE.NOTE] Dieser Schritt in der Lösung ist nur für Batch-Verarbeitung.

Eingabe und Ausgabe Datenmanagement-Daten:

- Die **Ausgabedaten** ( *PartitionedCarEventsTable*gekennzeichnet) ist für einen längeren Zeitraum als grundlegende / "roheste" aus Daten des Kunden "Daten am". 
- **Eingabedaten** für diese Pipeline würde normalerweise die Ausgabedaten hat originalgetreue zur Eingabe-nur gespeichert ist (partitioniert) besser zur nachfolgenden Verwendung verworfen.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-workflow.png)

*Abbildung 11 – Partition Veranstaltungen workflow*

Die Rohdaten ist partitioniert mit einer HDInsight Struktur Aktivität in "PartitionCarEventsPipeline". Für ein Jahr in Schritt 1 generierte Beispieldaten ist nach Jahr-Monat partitioniert. Partitionen werden zum fahrzeugsignale und Daten für jeden Monat eines Jahres (insgesamt 12 Partitionen) generieren. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partition-car-events-pipeline.png)

*Abbildung 12: PartitionCarEventsPipeline*

Das folgende Struktur Skript namens "partitioncarevents.hql" zum Partitionieren und befindet sich im Ordner "\demo\src\connectedcar\scripts" der heruntergeladenen Zip. 


    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string,
                YearNo                          int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

*Abbildung 13: PartitionConnectedCarEvents Hive-Skript*

Sobald die Pipeline erfolgreich ausgeführt wurde, sehen Sie die folgenden Partitionen in das Speicherkonto unter dem Container "Connectedcar" generiert.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-partitioned-output.png)

*Abbildung 14: partitionierte Ausgabe*

Daten jetzt optimiert, verwaltbare und zur Weiterverarbeitung rich Batch Einblicke. 

## <a name="data-analysis"></a>Datenanalyse

In diesem Abschnitt erfahren Sie, wie Azure Stream Analytics, Azure maschinelles lernen, Azure Data Factory und Azure HDInsight für Rich advanced Analytics Fahrzeug Gesundheit und fahrgewohnheiten kombinieren. Es gibt hier drei Unterabschnitte:

1.  **Computerlernen**: Dieser Unterabschnitt enthält Informationen zu Anomalien erkennen experimentieren, die in dieser Lösung verwendet haben Wartung Wartung erfordert und dass Rückrufe durch Sicherheit vorherzusagen.
2.  **Echtzeit-Analysen**: Dieser Unterabschnitt enthält Informationen zur Echtzeit-Analysen mithilfe der Abfragesprache Stream Analytics und operationalisierung des Experiments maschinelles lernen in Echtzeit mithilfe einer benutzerdefinierten Anwendung.
3.  **Batchanalyse**: Dieser Unterabschnitt enthält Informationen über das Transformieren und Verarbeitung der Stapeldaten Azure HDInsight mit Azure maschinelles lernen von Azure Data Factory operationalisiert.

### <a name="machine-learning"></a>Computerlernen

Hier soll Fahrzeuge vorherzusagen, die Wartung oder Rückruf anhand bestimmter Health Statistiken. Wir stellen die folgenden Annahmen

- Wenn eine der folgenden drei Ursachen zutrifft, daß Fahrzeuge **Wartung Wartung**
    - Reifendruck ist niedrig.
    - Motoröl ist niedrig
    - Motor ist hoch

- Wenn eine der folgenden Situationen zutrifft, können der Fahrzeuge ein **Sicherheitsproblem** und **Rückruf**erforderlich:
    - Motor hohe jedoch Temperatur niedrig
    - Motor niedrig aber Temperatur ist hoch

Anhand der vorherigen Anforderungen haben wir zwei separate Modelle zum Erkennen von Anomalien, Fahrzeug Wartung Erkennung und Fahrzeug Rückruf Erkennung erstellt. In beiden Modellen integrierte Principal Component Analysis (PCA) Algorithmus Normalbetriebswerte dient. 

**Wartungsmodell**

Eines drei Reifendruck, Motoröl oder Motor - die entsprechende Bedingung erfüllt, meldet das Wartungsmodell Anomalie. Daher müssen wir nur diese drei Variablen in das Modell. Im Experiment in Azure Machine Learning verwenden wir zunächst ein Modul **Wählen Sie Spalten im Dataset** extrahiert diese drei Variablen. Verwenden Sie weiter Erkennungsmodul Anomalie PCA-basierte das Anomalie Modell erstellen. 

Hauptkomponentenanalyse (PCA) ist eine etablierte Methode Computer lernen, die Featureauswahl, Klassifizierung und Erkennen von Netzwerkanomalien angewendet werden können. Der PCA konvertiert Kästen möglicherweise korrelierte Variablen in einer Gruppe von Werten, die wichtigsten Komponenten. Schlüssel soll der PCA-basierten Modellierung Daten auf einem niedrigeren dimensionalen Raum, Funktionen und Anomalien leichter identifiziert werden können.
 
Für jede neue Eingabe für das Modell Anomalie Detektor zuerst berechnet die Projektion auf die eigenvektoren und berechnet den normalisierten Wiederaufbau Fehler. Dieses standardisierte ist Anomalie Bewertung. Je höher der Fehler mehr außergewöhnlicher ist die Instanz. 

Problem Erkennung Wartung kann jeder Datensatz als ein Punkt in einem 3-dimensionalen Raum Reifendruck Motoröl und Motor Koordinaten definiert. Um diese Anomalien aufzuzeichnen, können wir die Originaldaten in 3-dimensionalen Raum auf eine 2-dimensionalen Raum mit PCA projizieren. So legen Sie den Parameter Anzahl der Komponenten in der PCA 2 verwenden. Dabei spielt eine wichtige Rolle im PCA-basierte Normalbetriebswerte anwenden. Projiziert Daten mit PCA können wir diese Anomalien leichter identifizieren.

**Zurückrufen Anomalie Modell** In den Rückruf Anomalie Modell, verwenden wir im Dataset und Anomalien PCA-basierte Spalten auswählen Erkennungsmodule ähnlich. Insbesondere extrahieren wir zunächst drei Variablen - Motor, Temperatur und Geschwindigkeit - Moduls **Spalten im Dataset auswählen** . Wir auch die Variable da Temperatur des Motors in der Regel die Geschwindigkeit korreliert. Weiter verwenden wir Erkennungsmodul Anomalie PCA-basierte Daten von 3-dimensionalen Raum auf einem 2-dimensionalen Raum projizieren. Rückruf Kriterien erfüllt sind und das Fahrzeug erfordert Rückruf beim Motor und Außentemperatur sehr negativ korreliert. Mit Anomalie PCA-basierte Erkennungsalgorithmus kann wir Anomalien nach dem Ausführen der PCA. 

Beim Trainieren Muster müssen wir normale Daten verwenden, die keine Wartung oder Rückruf als Eingabedaten zum Trainieren des Modells Anomalie PCA-basierte Erkennung erforderlich ist. Punktwertung Experiment verwenden wir das ausgebildeten Anomalie Modell um zu erkennen, ob das Fahrzeug Wartung oder Rückruf erforderlich ist. 


### <a name="real-time-analysis"></a>Echtzeit-Analysen

Die folgende Stream Analytics SQL-Abfrage zum Abrufen des Durchschnitt aller wichtigen Parameter wie Geschwindigkeit, Kraftstoffmenge Motor, Kilometerstand, Reifendruck, Motoröl und andere. Die Durchschnittswerte werden Anomalien erkennen, Alerts und allgemeinen Gesundheitszustand von Fahrzeugen in bestimmten Region bestimmt und dann auf Demographie Korrelation verwendet. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig15-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Abbildung 15-Stream Analytics Abfrage für Echtzeit-Verarbeitung

Der Durchschnitt über 3 Sekunden TumblingWindow berechnet. Wir verwenden TubmlingWindow in diesem Fall, da wir nicht überlappende und zusammenhängenden Zeitintervalle erforderlich. 

Erfahren Sie mehr über die "Windows" Funktionen in Azure Stream Analytics, klicken Sie auf [Fenster (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**Echtzeit-Vorhersage**

Eine Anwendung ist Bestandteil der Lösung Computer Lernmodell in Echtzeit durchsetzen. Diese Anwendung mit dem Namen "RealTimeDashboardApp" erstellt und als Teil der Bereitstellung der Lösung. Die Anwendung führt folgende Funktionen aus:

1.  Wartet auf eine Event Hub-Instanz dem Stream Analytics Ereignisse in einem Muster kontinuierlich veröffentlicht. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-stream-analytics-query-for-publishing.png)*Abbildung 16-Stream Analytics Abfrage für die Veröffentlichung von Daten der Ausgabe Event Hub-Instanz* 

2.  Für jedes Ereignis, das diese Anwendung empfängt: 

    - Verarbeitet die Daten mit Computer lernen Anforderungsantwort bewerten (RRS) Endpunkt. RRS-Endpunkt wird automatisch als Teil der Bereitstellung veröffentlicht.
    - Ressourceneinträge Ausgabe erscheint eine PowerBI DataSet mithilfe den Push-APIs.

Dieses Muster gilt auch für Szenarios, in denen Line of Business (LoB)-Anwendung mit dem Flow Echtzeitanalysen für Szenarien wie Alarme, Benachrichtigung und messaging integriert.

Klicken Sie zum Herunterladen der Lösung RealtimeDashboardApp Visual Studio angepasst [RealtimeDashboardApp herunterladen](http://go.microsoft.com/fwlink/?LinkId=717078) . 

**Echtzeit-Dashboard-Anwendung ausführen**

1.  Klicken Sie auf die PowerBI Knoten in der Ansicht, und klicken Sie im Eigenschaftenbereich "Echtzeit-Dashboardanwendung herunterladen". ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17-vehicle-telematics-powerbi-dashboard-setup.png)*Abbildung 17 – PowerBI Dashboard zur Einrichtung*
2.  Extrahieren und lokal speichern ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-realtimedashboardapp-folder.png) *Abbildung 18 RealtimeDashboardApp Ordner*
3.  Führen Sie die Anwendung RealtimeDashboardApp.exe
4.  Gültige Power BI Anmeldeinformationen anmelden, und klicken Sie auf annehmen ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png)![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Abbildung 19 – RealtimeDashboardApp: Melden Sie sich bei PowerBI*

>[AZURE.NOTE] Wenn PowerBI Dataset geleert werden sollen, führen Sie RealtimeDashboardApp mit dem Parameter "Flushdata": 

    RealtimeDashboardApp.exe -flushdata

### <a name="batch-analysis"></a>Batchanalyse

Hier soll zeigen, wie Contoso Motors Azure Compute-Funktionen zu Datenverlustvorfalls um umfassende Einblicke auf Muster Nutzungsverhalten und Fahrzeug Zustand verwendet. Dies ermöglicht:

- Verbesserung der Kundenzufriedenheit und machen es billiger von Einblicke Gewohnheiten und Kraftstoff effizient treibenden Verhalten
- Lernen Sie proaktiv Kunden und deren treibende Muster Entscheidungen steuern und bieten Produkte und services

In dieser Lösung zielen wir die folgenden Metriken:

1.  **Aggressive Fahrverhalten**: bezeichnet den Trend für die Modelle, Standorte, Zustände und Zeit des Jahres auf aggressive treibenden Einblicke. Contoso Motoren dieser Erkenntnisse können marketing Kampagnen neue personalisierte Funktionen sowie Verwendungsbasierte Versicherung.
2.  **Effiziente Fahrverhalten Kraftstoff**: bezeichnet den Trend für die Modelle, Standorte, Vorschriften und Zeit des Jahres auf Kraftstoff effizient treibenden Einblicke. Contoso Motors können diese Erkenntnisse für Marketingkampagnen, fördern neue Features und proaktive reporting zu den Treibern für Kosten und Umgebung angezeigte Fahrverhalten. 
3.  **Zurückrufen Modelle**: Modelle erfordern Rückrufe von Anomalien erkennen Machine learning-Experiment operationalisierung bezeichnet

Schauen Sie in den Details dieser Metriken,


**Aggressives fahrmuster**

Partitionierte fahrzeugsignale und Daten werden in der Pipeline mit dem Namen "AggresiveDrivingPatternPipeline" mit Struktur bestimmt die Modelle, Speicherort Fahrzeug, Fahrt und andere Parameter, die aggressive treibenden Muster zeigt.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-aggressive-driving-pattern.png) 
*Abbildung 20 – Aggressive Muster Workflow gesteuert*

Das Hive-Skript mit dem Namen "aggresivedriving.hql" aggressive treibenden Bedingung Muster analysiert ist heruntergeladenen Zip-Ordner "\demo\src\connectedcar\scripts". 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                vin                         string, 
                model                       string,
                timestamp                   string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,
                brake_pedal_status          string,
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'

*Abbildung 21 – Aggressive fahren Muster Hive-Abfrage*

Kombination des Fahrzeugs Übertragung gangposition Bremse Pedal Status und Geschwindigkeit verwendet erkennen halsbrecherischen/aggressive Fahrverhalten basierend auf Bremsen Muster mit hoher Geschwindigkeit. 

Sobald die Pipeline erfolgreich ausgeführt wurde, sehen Sie die folgenden Partitionen in das Speicherkonto unter dem Container "Connectedcar" generiert.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Abbildung 22-AggressiveDrivingPatternPipeline-Ausgabe*


**Effiziente treibenden Muster Kraftstoff**

Partitionierte fahrzeugsignale und Daten werden in der Pipeline mit dem Namen "FuelEfficientDrivingPatternPipeline". Struktur dient, Modelle, Speicherort, Fahrzeug, Fahrt und andere Eigenschaften, die Kraftstoff effizient treibenden Muster aufweisen.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Abbildung 23 – Kraftstoff effizient treibenden Muster workflow*

Das Hive-Skript mit dem Namen "fuelefficientdriving.hql" aggressive treibenden Bedingung Muster analysiert ist heruntergeladenen Zip-Ordner "\demo\src\connectedcar\scripts". 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                vin                         string, 
                model                       string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,                
                brake_pedal_status          string,            
                accelerator_pedal_position  string,                             
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


*Abbildung 24-Kraftstoff effizient treibenden Muster Hive-Abfrage*

Verwendet die Kombination die Übertragung gangposition und Bremse Pedal Status, Geschwindigkeit und Accelerator pedal Kraftstoff effizient Fahrverhalten basierend auf Beschleunigung, Bremsen, erkennen Muster speed. 

Sobald die Pipeline erfolgreich ausgeführt wurde, sehen Sie die folgenden Partitionen in das Speicherkonto unter dem Container "Connectedcar" generiert.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Abbildung 25-FuelEfficientDrivingPatternPipeline-Ausgabe*


**Vorhersagen zurückrufen**

Der Computer lernen Experiment bereitgestellt und als Web Service als Teil der Bereitstellung der Lösung. Bewertung Endpunkt Batch wird in diesem Workflow als verknüpfte Factorydienst Daten und operationalisiert Daten Factory Stapel Bewertung Aktivität genutzt.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-machine-learning-endpoint.png) 

*In Abbildung 26 – Computer lernen Endpunkt als verknüpfte Dienst in Data Factory registriert*

Registrierte verknüpfte Dienst wird in der DetectAnomalyPipeline, die Daten mit der Anomalie Modell verwendet. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-aml-batch-scoring.png) 

*Abbildung 27-Aktivität in "Data Factory" Azure Machine Learning Batch bewerten* 

Es gibt einige Schritte in dieser Pipeline für die Vorbereitung der Daten, damit es mit dem Webdienst Bewertung operationalisiert werden. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-pipeline-predicting-recalls.png) 

*Abbildung 28-DetectAnomalyPipeline für die Vorhersage von Fahrzeugen Rückrufe erfordern* 

Abschluss der Bewertung wird eine HDInsight-Aktivität zum Verarbeiten und die Daten, die als Modell mit Wahrscheinlichkeit 0,60 Anomalien oder höher.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                         string,
                model                       string,
                gendate                     string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                fuel                        string,
                engineoil                   string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position  string,
                parking_brake_status        string,
                headlamp_status             string,
                brake_pedal_status          string,
                transmission_gear_position  string,
                ignition_status             string,
                windshield_wiper_status     string,
                abs                         string,
                maintenanceLabel            string,
                maintenanceProbability      string,
                RecallLabel                 string,
                RecallProbability           string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                         string,
                model                       string,
                city                        string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                Year                        string,
                Month                       string              
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Sobald die Pipeline erfolgreich ausgeführt wurde, sehen Sie die folgenden Partitionen in das Speicherkonto unter dem Container "Connectedcar" generiert.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Abbildung 30 – Abbildung 30 – DetectAnomalyPipeline-Ausgabe*


## <a name="publish"></a>Veröffentlichen

### <a name="real-time-analysis"></a>Echtzeit-Analysen

Eine der Abfragen im Stream Analytics Auftrag veröffentlicht die Ereignisse Ausgabe Event Hub-Instanz. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig31-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*In Abbildung 31-Stream Analytics Auftrag eine Ausgabe veröffentlicht Event Hub-Instanz*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig32-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Abbildung 32-Stream Analytics Abfragen, um die Ausgabe veröffentlichen Event Hub-Instanz*

Dieser Stream von Ereignissen wird von der RealTimeDashboardApp in der Projektmappe enthaltenen genutzt. Diese Anwendung nutzt den Computer lernen Anforderungsantwort Webdienst Echtzeit bewerten und die resultierenden Daten in ein Dataset PowerBI für den Verzehr veröffentlicht. 

### <a name="batch-analysis"></a>Batchanalyse

Azure SQL-Datenbanktabellen für Verbrauch werden die Ergebnisse der Stapelverarbeitung und in Echtzeit veröffentlicht. Azure SQL Server-Datenbank und die Tabellen werden automatisch als Teil des Setup-Skript erstellt. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig33-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Abbildung 33-Stapelverarbeitung in Datamart Workflow Ergebnisse kopieren*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig34-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Abbildung 34-Stream Analytics-Auftrag Datamart veröffentlicht*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig35-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Abbildung 35-Datamart Stream Analytics Projekt festlegen*


## <a name="consume"></a>Nutzen

Power BI bietet diese Lösung rich Dashboard für Echtzeitdaten und Vorhersageanalysen Visualisierung. 

Hier finden Sie detaillierte Informationen zum Einrichten von PowerBI Berichten und das Dashboard. Das endgültige Dashboard sieht folgendermaßen aus:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig36-vehicle-telematics-powerbi-dashboard.png)

*Abbildung 36 - PowerBI-Dashboard*

## <a name="summary"></a>Zusammenfassung

Dieses Dokument enthält eine ausführliche Drilldown Fahrzeug Telemetrie Analytics Lösung. Dies zeigt ein Lambda-Architekturmuster in Echtzeit und Analytics Vorhersagen mit Aktionen. Dieses Muster gilt für eine Vielzahl von Anwendungsfälle, die langsamsten Pfad (Echtzeit) und kalte Pfad (Stapelverarbeitung). 
