<properties
 pageTitle="Fahrzeug Sensor Daten mit Apache auf HDInsight | Microsoft Azure"
 description="Informationen Sie zum Fahrzeug Sensordaten von HDInsight mit Apache Storm Ereignis verarbeiten. Hinzufügen von Daten aus DocumentDB und Ausgabe Speicher speichern."
 services="hdinsight,documentdb,notification-hubs"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Verarbeiten Sie Sensordaten von Azure Ereignis Hubs mit Apache Storm HDInsight Fahrzeug

Informationen Sie zum Fahrzeug Sensordaten von Azure Ereignis Hubs mit Apache Storm HDInsight verarbeiten. Dieses Beispiel liest Daten aus Azure Ereignis Hubs, bereichert die Daten verweisen auf Daten in Azure DocumentDB und schließlich Datenspeicher in Azure-Speicher mit der Hadoop Datei System bietet.

![HDInsight und das Internet der Dinge (IoT)-Architekturdiagramm](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

##<a name="overview"></a>Übersicht

Fahrzeuge hinzufügen Sensoren kann Hardwareprobleme basierend auf Verlaufsdaten Trends Vorhersagen sowie Verbesserungen auf zukünftige Versionen basierend auf Verwendungsanalyse Muster. Während herkömmliche MapReduce Stapelverarbeitung für diese Analyse verwendet werden kann, müssen Sie möglicherweise schnell und effizient Laden der Daten aus allen Fahrzeugen in Hadoop MapReduce Verarbeitung vor. Darüber hinaus können Sie die Analyse für kritische Fehler Pfade (Motor, Bremsen, usw.) in Echtzeit.

Azure Ereignis Hubs überragender Sensoren erzeugte große Datenmengen verarbeiten und Apache Storm auf HDInsight wird geladen und verarbeitet die Daten vor dem Speichern in (unterstützt von Azure Storage) bietet zusätzliche MapReduce-Verarbeitung.

##<a name="solution"></a>Lösung

Telemetriedaten Motor, Temperatur und Geschwindigkeit werden von Sensoren an Event Hubs einen Zeitstempel sowie das Auto Fahrzeug-Identifizierungsnummer (FIN) gesendet. Storm-Topologie auf einem Apache HDInsight Cluster ausgeführt dort die Daten gelesen, verarbeitet und speichert in bietet.

Während der Verarbeitung dient die FIN Informationen von Azure DocumentDB abrufen. Dies wird Datenstrom hinzugefügt, bevor es gespeichert wird.

Die Komponenten der Storm-Topologie sind:

* **EventHubSpout** - liest Daten von Azure-Ereignis

* **TypeConversionBolt** - konvertiert die JSON-Zeichenfolge in ein Tupel mit einzelnen Datenwerte Motor, Temperatur, Geschwindigkeit, VIN und Zeitstempel von Ereignis

* **DataReferencBolt** - sucht das Modell von DocumentDB über die Fahrzeug-Identifizierungsnummer

* **WasbStoreBolt** - speichert die Daten bietet (Azure-Speicher)

Im folgenden finden ein Diagramm dieser Lösung:

![Storm-Topologie](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

> [AZURE.NOTE] Dies ist eine vereinfachte Darstellung, und jede Komponente der Projektmappe kann mehrere Instanzen haben. Mehrere Instanzen der einzelnen Komponenten in der Topologie sind z. B. die Knoten auf HDInsight Cluster verteilt.

##<a name="implementation"></a>Implementierung

Eine vollständige, automatisierte Lösung für dieses Szenario ist als Teil des [HDInsight-Storm-Beispiele](https://github.com/hdinsight/hdinsight-storm-examples) Repository auf GitHub. Um dieses Beispiel zu verwenden, gehen Sie in [IoTExample README. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiel Sturm Topologien finden Sie unter [Topologien für auf HDInsight](hdinsight-storm-example-topology.md).
