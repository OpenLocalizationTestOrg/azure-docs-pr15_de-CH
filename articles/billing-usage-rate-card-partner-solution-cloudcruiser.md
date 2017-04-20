<properties
   pageTitle="Kreuzer und Abrechnung API-Integration von Microsoft Azure Cloud | Microsoft Azure"
   description="Vertritt einen unverwechselbaren Standpunkt von Microsoft Azure Abrechnung Partner Cloud Kreuzer auf ihre Azure Abrechnung APIs in ihre Produkte integrieren.  Dies ist besonders für Azure und Cloud Kreuzer, die bei der Verwendung/Cloud Kreuzer für Microsoft Azure interessiert sind."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"
   />

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="09/08/2016"
   ms.author="mobandyo;sirishap;bryanla"/>

# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Kreuzer und Abrechnung API-Integration von Microsoft Azure Cloud

Dieser Artikel beschreibt, wie Informationen aus der neuen Microsoft Azure Abrechnung APIs Cloud Kreuzer Workflow Kosten Simulation und Analyse kann.

## <a name="azure-ratecard-api"></a>Azure RateCard-API
Die RateCard-API bietet Informationen von Azure. Nach der Authentifizierung mit den richtigen Anmeldeinformationen können Sie die API, um Metadaten über die verfügbaren Dienste auf Azure sowie Ihre bieten ID zugeordneten Sätze erfassen Abfragen.

Im folgenden Beispiel zeigt eine Antwort mit den Preisen für A0 API (Windows) Instanz:

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>Cloud Cruiser Azure RateCard-API-Schnittstelle
Cloud Kreuzer kann die RateCard-API-Informationen auf verschiedene Arten nutzen. In diesem Artikel zeigen wir Verwendung kann IaaS zu Arbeitslast Kosten Simulation und Analyse.

Demonstrieren Sie diesem Angenommen Sie Arbeitslast mehrere Instanzen auf Microsoft Azure Pack (WAP). Soll diese derselben Arbeitslast auf Azure simulieren und schätzen Sie die Kosten für diese Migration. Um diese Simulation erstellen, gibt es zwei Hauptaufgaben ausgeführt werden:

1. **Importieren Sie und Informationen Sie Service stammen aus der RateCard-API.** Diese ist auch auf die Arbeitsmappen Aufgabe, Auszug aus der RateCard-API transformiert und einen neuen Satz Plan veröffentlicht. Dieser neue Tarif auf Simulationen dienen Azure Preise schätzen.

2. **Normalisieren Sie WAP-Services und Azure für IaaS.** Standardmäßig WAP-Services basieren auf einzelne Ressourcen (CPU, Speicher, Größe usw.) bei Azure Services basieren Größe (A0, A1, A2 usw.). Zunächst Cloud Kreuzer ETL-Engine ausgeführt, Arbeitsmappen aufgerufen, in dem diese Ressourcen auf Instanz, analog zur Azure-Instanz Services gebündelt werden.

### <a name="import-data-from-the-ratecard-api"></a>Importieren von Daten aus der RateCard-API

Cloud Kreuzer Arbeitsmappen bieten eine automatisierte Möglichkeit zum Sammeln und Verarbeiten von Informationen aus der RateCard-API.  ETL (Extract Transformation laden) Arbeitsmappen können Sie Auflistung, Transformation und Veröffentlichung von Daten in die Cloud Kreuzer Datenbank konfigurieren.

Jede Arbeitsmappe können eine oder mehrere Sammlungen können Sie Informationen aus verschiedenen Quellen ergänzen oder Erweitern von Daten entsprechen. Die folgenden zwei Screenshots zeigen eine neue *Auflistung* in eine vorhandene Arbeitsmappe importieren von Informationen in der *Auflistung* der RateCard-API zu erstellen:

![Abbildung 1: Erstellen einer neuen Sammlung][1]

![Abbildung 2: Importieren von Daten aus der neuen Sammlung][2]

Nach dem Import der Daten in der Arbeitsmappe ist möglich, mehrere Schritte und transformationsprozesse ändern und die Daten zu erstellen. In diesem Beispiel, da wir nur Infrastructure-as-a-Service (IaaS interessieren) wir Schritte Transformation verwenden können, um nicht benötigte Zeilen zu entfernen, oder Datensätze, im Zusammenhang mit Services als IaaS.

Der folgende Screenshot zeigt die Transformation Schritte zum Verarbeiten von Daten aus RateCard-API:

![Abbildung 3: Transformation Schritte zum Verarbeiten von Daten aus RateCard-API][3]

### <a name="defining-new-services-and-rate-plans"></a>Definieren neuer Services und Rate Pläne

Es gibt verschiedene Arten von Services auf Cloud. Eine der Optionen ist die Dienste von Daten importieren. Diese Methode wird häufig verwendet, wenn öffentliche Clouds gearbeitet, die Dienste bereits vom Anbieter definiert sind.

Rate Plan ist ein Satz von Preise oder Preise basierend auf den Gültigkeitsdaten oder Kunden unter anderem Dienste zugewiesen werden können. Tarifen auch auf Cloud dienen Simulation oder "Was-Wenn-Szenarien zu verstehen, wie Services die Gesamtkosten einer Arbeitslast beeinflussen können.

In diesem Beispiel nutzen wir die Dienstinformationen aus der RateCard-API neue Dienste Cloud Kreuzer definieren. Zugeordnete Dienste Preise können wir auf die gleiche Weise erstellen Sie einen neuen Satz Plan auf Cloud.

Am Ende des Umwandlungsprozesses ist einen neuen Schritt erstellen und die Daten aus der RateCard-API als neue und-Kosten veröffentlichen.

![Abbildung 4: Veröffentlichen der Daten aus der RateCard-API als neuer Services und Preise][4]

### <a name="verify-azure-services-and-rates"></a>Überprüfen von Azure und-Kosten

Nach der Veröffentlichung der Dienste und Preise, können Sie die Liste der importierten Dienste Kreuzer Cloud *Services* Registerkarte überprüfen:

![Abbildung 5: die neuen Dienste überprüfen][5]

Überprüfen Sie auf der Registerkarte *Plant die Rate* der neuen Tarif namens "AzureSimulation" mit den aus der RateCard-API eingeführt.

![Abbildung 6: überprüfen die neue Rate planen und zugeordneten][6]

### <a name="normalize-wap-and-azure-services"></a>Normalisieren Sie WAP und Azure Services

Standardmäßig informiert WAP Verbrauch basierend auf der Verwendung von Compute, Speicher und Netzwerk-Ressourcen. Cloud Kreuzer können Sie Ihre Dienstleistungen auf Reservierung oder gemessene Nutzung dieser Ressourcen. Angenommen, können Sie ein für jede CPU-Auslastung oder aufgeladen GB Speicher mit einer Instanz.

Beispielsweise müssen zwischen WAP und Azure, Vergleichen wir die Ressourcenverwendung WAP zu bündeln, aggregieren Azure Services zugeordnet werden können. Hierdurch kann leicht in Arbeitsmappen implementiert werden:

![Abbildung 7: WAP Datentransformation Normalisierung der Dienste][7]

Der letzte Schritt in der Arbeitsmappe werden die Daten in der Cloud Kreuzer Datenbank veröffentlichen. In diesem Schritt Daten jetzt Services (die Azure Services zuordnen) gebündelt und an Standardsätze die Zuschläge zu erstellen.

Nach Abschluss der Arbeitsmappe, können Sie die Verarbeitung der Daten automatisieren, indem Hinzufügen einer Aufgabe im Planer und Häufigkeit und Zeitpunkt für die Arbeitsmappe ausführen.

![Abbildung 8: Planen der Arbeitsmappe][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Erstellen von Berichten für Arbeitsauslastungsanalyse Kosten Simulation

Die Verwendung erfasst und Zuschläge in die Cloud Kreuzer Datenbank geladen sind, nutzen wir Cloud Kreuzer Einblicke Modul erstellen wir wollen Simulation Kosten Arbeitslast.

Um dieses Szenario zu demonstrieren, haben wir den folgenden Bericht:

![Kostenvergleich][9]

Das obere Diagramm zeigt einen Kostenvergleich Dienststellen Vergleich des Preises mit der Arbeitslast für jeden bestimmten Dienst zwischen WAP (Dunkelblau) und Azure (hellblau).

Das Diagramm unten zeigt die gleichen Daten aber nach Abteilung. Dies zeigt die Kosten für jede Abteilung der Arbeitslast auf WAP und Azure sowie die Unterschiede in der Leiste Spareinlagen (Grün) ausgeführt.

## <a name="azure-usage-api"></a>Nutzung der Azure-API


### <a name="introduction"></a>Einführung

Microsoft hat vor kurzem Azure Verwendung API Abonnenten programmgesteuert Daten Einblicke in ihren Verbrauch ziehen. Dies sind gute Nachrichten für Cloud Kreuzer-Kunden, die mehr über diese API Dataset nutzen können.

Kreuzer Cloud kann Integration API Verwendung auf verschiedene Weise nutzen. Die Granularität (stündlich Verwendungsinformationen) und Ressource Metadateninformationen über die API erforderliche Dataset für flexible Kostenauflistung oder Chargeback Modelle. 

In diesem Lernprogramm präsentieren wir ein Beispiel wie Cloud Kreuzer Verwendung API Informationen profitieren. Genauer gesagt, wir Azure eine Ressourcengruppe erstellen, ordnen Sie Tags für die Kontostruktur wird beschreiben und verarbeiten die Tag-Informationen in Cloud Kreuzer.
 
Ziel ist, dass Berichte wie den folgenden erstellen und Kosten und Verbrauch anhand der Tags bevölkert Kontostruktur analysieren.

![Abbildung 10: mit Strukturpläne tags][10]

### <a name="microsoft-azure-tags"></a>Microsoft Azure-Tags

Daten über Azure Verwendung API enthält nicht nur Informationen, sondern auch Ressourcenmetadaten einschließlich Tags zugeordnet. Tags bieten eine einfache Möglichkeit zum Organisieren Ihrer Ressourcen jedoch um wirksam, müssen Sie sicherstellen, dass:

- Tags werden ordnungsgemäß auf Ressourcen zum Zeitpunkt der Bereitstellung angewendet.
- Tags korrekt Kostenauflistung-Chargeback-Prozess wird die Verwendung der Organisation Kontostruktur binden.

Beide dieser Vorschriften kann schwierig, vor allem bei der Bereitstellung bzw. Aufladen Seite manuell. Tippfehler, falsche oder auch fehlende Tags sind häufig Beschwerden von Kunden, wenn Tags mit diesen Fehlern Leben Laden auf sehr erschweren kann.

Mit der neuen Azure Verwendung API Cloud Kreuzer Ressource tagging Informationen und durch das hochentwickelte ETL-Tool Arbeitsmappen, korrigieren diese gemeinsamen tagging. Durch Umwandlung mithilfe regulärer Ausdrücke und Datenkorrelation können Cloud Kreuzer falsch gekennzeichnete Ressourcen und die richtigen Tags Zuordnung der Ressourcen mit den Verbraucher gewährleisten.

Auf laden können Cloud Kreuzer Kostenauflistung-Chargeback automatisiert und die Tag-Informationen um die Verwendung der entsprechenden Verbraucher (Abteilung, Abteilung, Projekt usw.) binden. Diese Automatisierung bietet eine enorme Verbesserung und gewährleistet eine konsistente und überprüfbaren Ladevorgang.
 

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Erstellen einer Ressourcengruppe mit Microsoft Azure
Der erste Schritt in diesem Lernprogramm ist die Erstellung einer Ressourcengruppe im Azure-Portal erstellen Sie neue Tags auf Ressourcen zuordnen. In diesem Beispiel schaffen wir die folgenden Tags:, Umgebung, Abteilungsleiter, Projekt.

Der folgende Screenshot zeigt ein Beispiel Ressourcengruppe mit den entsprechenden Tags.

![Abbildung 11: Ressourcengruppe mit zugeordneten Azure-Portal][11]

Der nächste Schritt besteht darin Informationen Verwendung API in Cloud Kreuzer. Verwendung-API bietet derzeit Antworten im JSON-Format. Hier ist ein Beispiel für die Daten:


    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Importieren von Daten aus der Verwendung API in Cloud Kreuzer

Cloud Kreuzer Arbeitsmappen bieten ein automatisiertes Verfahren zum Sammeln und verarbeiten Informationen von der Verwendung API. Eine Arbeitsmappe ETL (Extract Transformation Load) können Sie Auflistung, Transformation und Veröffentlichung von Daten in die Cloud Kreuzer Datenbank konfigurieren.

Jede Arbeitsmappe können eine oder mehrere Sammlungen. Dadurch werden Informationen aus verschiedenen Quellen ergänzen oder Erweitern von Daten entsprechen. Für dieses Beispiel erstellen wir ein neues Blatt in der Arbeitsmappe Azure Vorlage (_UsageAPI)_ und eine neue _Auflistung_ Daten von der Verwendung API importieren.

![Abbildung 3: API Daten importiert das Blatt UsageAPI][12]

Beachten Sie, dass diese Arbeitsmappe bereits andere Blätter importieren Services von Azure (_ImportServices_) und Verbrauch Informationen aus der Abrechnung-API (_PublishData_).

Weiter Wir Verwendung API verwenden, um das Blatt _UsageAPI_ füllen und diese Informationen den Verbrauchsdaten aus der Abrechnung-API auf dem Blatt _PublishData_ .

### <a name="processing-the-tag-information-from-the-usage-api"></a>Verarbeiten der Tag Informationen von API-Verwendung

Nach dem Import der Daten in der Arbeitsmappe schaffen Transformation Schritte im Blatt _UsageAPI_ wir um die Informationen aus der API. Zunächst wird mit einem Prozessor "JSON split" Tags aus einem Feld Extrahieren und Felder jeweils (Abteilung, Projekt, Besitzer und Umgebung) erstellen.

![Abbildung 4: Erstellen Sie neue Felder für die Tag-Informationen][13]

Beachten Sie den Dienst "Netzwerke" fehlt die Tag-Informationen (Frage), aber wir können ist Teil der gleichen Ressourcengruppe anhand der im Feld _ResourceGroupName_ überprüfen. Da die Tags für die anderen Ressourcen aus dieser Ressourcengruppe vorliegt, können wir diese Informationen fehlende Tags auf diese Ressource später anzuwenden.

Der nächste Schritt ist die Erstellung eine Nachschlagetabelle, die Informationen über die Tags _ResourceGroupName_zuordnen. Diese Nachschlagetabelle wird im nächsten Schritt zum Verbrauchsdaten mit Markierungsinformationen bereichern.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>Die Daten hinzufügen die Tag-Informationen

Wir können, das Verbrauchsinformationen vom API-Abrechnung verarbeitet Blatt _PublishData_ wechseln und fügen Sie die Felder aus den Tags extrahiert. Dieser Vorgang erfolgt anhand der Nachschlagetabelle im vorherigen Schritt _ResourceGroupName_ als Schlüssel für die Suchvorgänge mit erstellt.

![Abbildung 5: Auffüllen der Kontostruktur mit den Informationen aus der Suche][14]

Beachten Sie, dass die Strukturfelder geeignetes Konto für den Dienst "Netzwerke" angewendet wurden, beheben das Problem mit den fehlenden Tags. Wir aufgefüllt auch Kontofelder Struktur für Ressourcen als Ziel Ressourcengruppe mit"", um diese Berichte zu unterscheiden.

Jetzt müssen wir nur einen Schritt zum Veröffentlichen von Daten hinzufügen. In diesem Schritt werden die Verwendungsinformationen die entsprechenden Preise für jeden Dienst auf unsere Gebührenübersicht definiert mit der resultierenden Datenbank geladen angewendet werden.

Das beste ist, dass Sie nur einmal mit diesem Prozess. Wenn die Arbeitsmappe abgeschlossen ist, müssen Sie nur Planer hinzufügen und wird stündlich oder täglich zur geplanten Zeit ausgeführt. Dann ist es nur eine Frage der neue Berichte erstellen oder vorhandene anpassen, um die Datenanalyse zu sinnvollen Einblicke aus der Cloud.

### <a name="next-steps"></a>Nächste Schritte

+ Detaillierte Informationen zum Cloud Kreuzer Arbeitsmappen und Berichte finden Sie in der Cloud Kreuzer online- [Dokumentation](http://docs.cloudcruiser.com/) (gültige Anmeldung erforderlich).  Weitere Informationen zu Cloud Kreuzer wenden Sie sich an [info@cloudcruiser.com](mailto:info@cloudcruiser.com).
+ Übersicht der Azure-Ressourcenverwendung und RateCard-APIs finden Sie unter [Einblicke in Ihre Microsoft Azure Ressourcenverbrauch](billing-usage-rate-card-overview.md) .
+ [Azure Abrechnung REST-API-Referenz](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) für Weitere Informationen zu beiden APIs sind Teil der APIs von Azure-Managers anzeigen
+ Checken Sie rechts in der Beispielcode befassen möchten, unserer Microsoft Azure Abrechnung API Codebeispiele für [Azure Codebeispiele](https://azure.microsoft.com/documentation/samples/?term=billing).

### <a name="learn-more"></a>Weitere Informationen
+ Anzeigen Sie [Azure Ressourcenmanager](azure-resource-manager/resource-group-overview.md) Übersichtsartikel zu Azure-Ressourcen-Manager weitere

<!--Image references-->
 
[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Abbildung 1: Erstellen einer neuen Sammlung"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Abbildung 2: Importieren von Daten aus der neuen Sammlung"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Abbildung 3: Transformation Schritte zum Verarbeiten von Daten aus RateCard-API"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Abbildung 4: Veröffentlichen der Daten aus der RateCard-API als neuer Services und Preise"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Abbildung 5: die neuen Dienste überprüfen"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Abbildung 6: überprüfen die neue Rate planen und zugeordneten"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Abbildung 7: WAP Datentransformation Normalisierung der Dienste"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Abbildung 8: Planen der Arbeitsmappe"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Abbildung 9: Beispielbericht Arbeitslast Kosten Vergleich Szenario"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Abbildung 10: mit Strukturpläne tags"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Abbildung 11: Ressourcengruppe mit zugeordneten Azure-Portal"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Abbildung 12: API Daten importiert das Blatt UsageAPI"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Abbildung 13: Erstellen Sie neue Felder für die Tag-Informationen"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Abbildung 14: Auffüllen der Kontostruktur mit den Informationen aus der Suche"
