<properties
    pageTitle="Ziehen von Daten in Azure Ereignis Hubs | Microsoft Azure"
    description="Übersicht der Veranstaltung aus Web importieren"
    services="event-hubs"
    documentationCenter="na"
    authors="spyrossak"
    manager="timlt"
    editor=""/>

<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/25/2016"
    ms.author="spyros;sethm" />

# <a name="pulling-public-data-into-azure-event-hubs"></a>Ziehen von Daten in Azure Ereignis Hubs

In normalen Internet der Dinge (IoT) Szenarien haben Sie Geräte, die Sie Push Daten in Azure Azure Event Hub oder IoT Hub programmieren können. Sowohl die Hubs sind in Azure zum Speichern, analysieren und Visualisieren mit einer Vielzahl von Tools Microsoft Azure zur Verfügung gestellt. Sie müssen jedoch Senden der Daten an als JSON formatiert und spezifisch gesichert. Dadurch wird die folgende Frage. Was tun Sie, um Daten aus öffentlichen und privaten Quellen, die Daten werden als ein Webdienst oder Futtermittel irgendeiner aber Ihnen nicht ändern, wie Daten veröffentlicht werden, soll? Betrachten Sie das Wetter und Verkehr oder Aktienkurse - kann nicht sagen, NOAA, WSDOT oder NASDAQ zu konfigurieren Sie einen Push auf Ihrem Haupt-Ereignis. Zur Lösung dieses Problems haben wir geschrieben und eine kleine Beispiel, die Sie ändern und bereitstellen können, die Daten aus einer Quelle ziehen und schieben Sie ihn auf Ihrem Haupt-Ereignis Open Source. Von dort möglich beliebig, Betreff, um den Lizenzvertrag des Herstellers. Sie finden die Anwendung [hier](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/).

Beachten Sie, dass der Code in diesem Beispiel nur um Daten vom Webdienst Feeds und zum Azure Event Hub schreiben. Dies nicht soll eine produktionsanwendung und keine versucht wurde, in einer solchen Umgebung einsetzbar machen – Strictfly BASTELN, z.B. Entwickler. Außerdem ist das Vorhandensein dieses Beispiel einer Empfehlung, dass **Daten** in Azure als **Push** sollte es nicht. Sie sollten überprüfen, Sicherheit, Leistung, Funktionalität und Kostenfaktoren vor End-to-End-Architektur.

## <a name="application-structure"></a>Anwendung

Die Anwendung ist in C# geschrieben und [Sample Beschreibung](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) enthält alle Informationen müssen Sie erstellen, ändern und veröffentlichen Sie die Anwendung. Die folgenden Abschnitte bieten einen allgemeinen Überblick darüber, was die Anwendung macht.

Wir beginnen mit der Annahme, dass Sie auf Daten zugreifen. Sie möchten z. B. die Daten aus der Washington State Department of Transportation oder Daten von NOAA, benutzerdefinierte Berichte anzeigen oder diese Daten mit anderen Daten in der Anwendung ziehen. Sie müssen auch zu Azure Event Hub eingerichtet haben, die Verbindung zum Zugriff erforderliche Zeichenfolge.

Die Lösung GenericWebToEH startet, liest eine Konfigurationsdatei (App.config) eine Reihe von Dingen zu:

1. Die URL oder eine Liste von URLs für Website veröffentlichen der Daten. Idealerweise ist dies eine Website, die Daten in JSON, wie auf die WSDOT veröffentlicht [hier](http://www.wsdot.wa.gov/Traffic/api/). 
2. Anmeldeinformationen für die URL, falls erforderlich. Viele öffentliche Quellen ohne Anmeldeinformationen oder setzen die Anmeldeinformationen in der URL-Zeichenfolge. Andere müssen separat angeben. (Beachten Sie, dass nur ein Satz von Anmeldeinformationen in dieser Anwendung angeben können, sodass es nur funktioniert, wenn Sie nur eine URL keine Liste von URLs angeben.)
3. Die Verbindungszeichenfolge und den Namen des Ereignisses in diesem Ereignis Hubs Namespace, die Daten übertragen werden. Sie finden diese Informationen im Azure-Portal.
4. Ein Ruheintervall, in Millisekunden für das Intervall zwischen die Website Daten abrufen. Diese Einstellung erfordert einige Gedanken. Wenn Sie zu selten Abfragen können Sie Daten vermissen; Andererseits, wenn Sie zu häufig abrufen, erhalten viele sich wiederholende Daten oder schlimmer noch, Sie möglicherweise blockiert schädliche bot. Berücksichtigen Sie, wie oft die Datenquelle aktualisiert wird - Wetter Daten Verkehr können alle 15 Minuten aktualisiert, aber Aktienkurse vielleicht alle paar Sekunden, je nachdem, wo Sie sie erhalten. 
5. Ein Flag, um festzulegen, ob die Daten als JSON oder XML kommt. Da Sie Daten an einen Hub Ereignis drücken müssen, muss die Anwendung ein Modul XML vor dem Senden in JSON konvertiert.

Nach dem Lesen der Konfigurationsdatei, geht die Anwendung in eine Schleife - öffentliche Website, konvertiert die Daten ggf. auf Ihrem Haupt-Ereignis schreiben und anschließend das Ruheintervall wartet, bevor dabei ganz erneut. Insbesondere:

  * Lesen der öffentlichen Websites. Die Instanz der RawXMLWithHeaderToJsonReader-Klasse ist für Datenempfang bereit zum Senden von Azure/GenericWebToEH/ApiReaders/RawXMLWithHeaderToJsonReader.cs verwendet. Quelldatenstrom Iservice1 Methode liest und teilt es kleinere Einheiten (z.B. Datensätze) mit GetXmlFromOriginalText. 
  Diese Methode liest XML wohlgeformt JSON oder JSON-array. Verarbeitung MergeToXML Konfiguration in App.config gestartet wird (Standard = leer).
  * Die sendenden und empfangenden Daten werden als Schleife Process()-Methode in Program.cs implementiert. 
  Nach Iservice1 Ergebnisse erhalten, getrennte Methode reiht Ereignis-Hub.

## <a name="next-steps"></a>Nächste Schritte

Zum Bereitstellen der Projektmappe Klonen oder [GenericWebToEH](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) -Anwendung herunterladen, bearbeiten Sie die Datei App.config erstellt und schließlich veröffentlichen. Nach der Anmeldung sehen Sie im klassischen Azure-Portal unter Cloud-Dienste ausgeführt und einige Konfigurationsinformationen (wie das Ereignis Hub-Ziel und das Ruheintervall) in der Registerkarte **Konfigurieren** ändern.

Unter finden Sie weitere Ereignis Hubs in [Azure Samples Gallery](https://azure.microsoft.com/documentation/samples/?service=event-hubs) und [MSDN](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5).
