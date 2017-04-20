<properties
   pageTitle="Microsoft Azure-Verwendung und RateCard-APIs ermöglichen Cloudyn ITFM Kunden | Microsoft Azure"
   description="Vertritt einen unverwechselbaren Standpunkt von Microsoft Azure Abrechnung Partner Cloudyn, auf ihre Azure Abrechnung APIs in ihre Produkte integrieren.  Dies ist besonders für Azure und Cloudyn, die mit/versucht Cloudyn Azure Services interessiert sind."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-to-provide-itfm-for-customers"></a>Microsoft Azure-Verwendung und RateCard-APIs ermöglichen Cloudyn ITFM für Kunden

Cloudyn, Microsoft Development Partner und ein führender Anbieter von Cloud-Verwaltungsfunktionen wurde eine private Vorschau Microsoft Azure Ressourcenverwendung und RateCard APIs ausgewählt.  Verwendung-API bietet Zugriff auf geschätzte Azure Daten für ein Abonnement. Die RateCard-API bietet vollständige Preisinformationen alle Azure Services für nicht - Konzernvertrag EA-Kunden. Diese APIs bieten integriert zusammen die Grundlage Informationen für Eingaben in Tools wie Cloudyn IT Financial Management (ITFM).

## <a name="introduction"></a>Einführung

Die so genannte "Multiplikation" Daten Verwendung API mit der RateCard-API (Preis Verbrauch [Einheiten] [$unit] = detaillierte Verwendung und Kosten) die präzise, exakte und zuverlässige Abrechnungsinformationen verfügbar für Azure heute erstellt.

![ITFM (Übersicht)][1]

Diese APIs nutzen bietet Schlüsselinformationen Verwendung und Kosten Cloudyn Kunden einfach und programmgesteuerte Analysieren und für seine Kunden verschiedene ITFM Aufgaben ermöglichen.

## <a name="integrating-cloudyn-with-the-ratecard-and-usage-apis"></a>RateCard und Verwendung APIs integrieren Cloudyn
Der RateCard-API erfordert mehrere Eingabeparameter - Region Info, Währung und Gebietsschema – aber der OfferDurableID gibt der Typ von Azure bietet den Kunden (nutzungsbasierte, legacy 6 bis 12 Monaten Engagement Pläne, MSDN bietet, MPN Angebote, Sonderangebote und andere) verwendet wird. Die OfferDurableID kann in [Azure und Abrechnung Portal](https://account.windowsazure.com/Subscriptions), unter der"bieten" für das angegebene Abonnement gefunden werden.

Anmeldung für [Cloudyn für Azure](https://www.cloudyn.com/microsoft-azure/) Services können Kunden ihren Code OfferDurableID hinzufügen Cloudyn ihre Preise Informationen über die RateCard-API abrufen können.  Für verschiedene Angebote finden eine Detailseite [Microsoft Azure bieten](https://azure.microsoft.com/support/legal/offer-details/) .

![Cloudyn ITFM-Modul (Übersicht)][2]

Cloudyn wird die Verwendung und RateCard APIs neben Leistung Azure-API zusätzliche Visualisierung, Analytics Warnung, Berichterstattung, Kosten und umsetzbare Vorschläge erstellen Kunden Azure eine zuverlässige Enterprise Cloud ITFM Tool.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Cloudyn ITFM Fällen durch Nutzung und RateCard-API-Integration aktiviert
Allgemeine Cloudyn ITFM Verwendung aktivierte Anwendungsfälle und RateCard APIs umfassen:

+ **Kostenanalyse** - cloud-Kosten auf eine systemeigene identifizierende Dimension (Provider Service, Konto, Region usw.). Der Azure-Nutzung RateCard APIs machen dies einfach, die detaillierte Aufschlüsselung der Nutzung und Kosten pro Konto gruppiert und nach Cloudyn gefiltert und Benutzer Grafik oder in tabellarischer Form.

![Kosten Analyse Kreisdiagramm][3]

+ **Cost Allocation 360** - ermöglicht Finanz- und IT-Managern die Aufschlüsselung der tatsächlichen Kosten, Treiber und ihre Cloudbereitstellung Trends. Es kann weitere Manager leicht Bereitstellung Ausgaben Unternehmenseinheiten, Abteilungen, Bereiche und beispiellose Einblicke in Cloud Kosten und erleichtert Unternehmen Rückbuchungen und Showbacks zuordnen. Azure Verbrauch und RateCard APIs dienen als Eingabe für Cloudyns Kosten Zuteilung Engine ergänzt die APIs Methoden und Geschäftslogik für ohne Tags oder untaggable Ressourcen.

![Umlage 360-Diagramm][4]

+ **Kostengünstige Sizing** - Leitfaden Dimensionierung für unzureichend genutzte virtuelle Maschinen reduziert der Kunde Ausgaben auf zu große oder zu bereitgestellt. Dies geschieht mithilfe virtueller Computer CPU und RAM-Metriken (über Performance-API) Stunden Laufzeit (über Verwendung API) und Kosten (über RateCard-API). Cloudyn Dimensionierung Leitfaden ausgelasteter CPU oder Ressourcen (Leistung) und geschätzte Spareinlagen multipliziert Preis Delta (RateCard) zwischen den VMs durch die tatsächliche Zeit-Nutzung (Verwendung) unzureichend genutzter Computer berechnet.

![Kostengünstige anpassen][5]

+ **Portieren Recommendations Cloud** - Beratung Cloud bietet portieren. Es untersucht ein Benutzer aktuelle Kosten Cloud-Ressourcen die wichtigsten Cloud-Anbieter bereitgestellt werden und Kosten eine entsprechende Weitergabe auf Azure vergleicht. Bietet er dann präzise, pro Ressource, finanziell basierende Vorschläge in Azure portieren. Nach Beurteilung entsprechende Weitergabe auf Azure (basierend auf Kennzahlen und Benutzer Leistungsvoreinstellungen) erforderlich, verwendet Cloudyn RateCard-API, die Kosten der Bereitstellung in Azure entspricht.

+ **Leistungsberichte** - Leistung der Azure-API die Berichte bieten eine Reihe von Features von CPU und RAM-Nutzung, Optimierung Recommendations. Es folgt Instanz Auslastung Bericht beispielsweise sowohl Instanz Aufschlüsselung nach durchschnittliche CPU-Auslastung.

![Performance-Berichte][6]

+ **Kategorie-Manager** - eine leistungsfähige Funktion in Cloudyn, die nicht organisierte Cloudressourcen Reihenfolge bringt. Es bietet Benutzern die Möglichkeit zu eigenen eindeutigen Kategorien (Tags) effektiv messen und reporting Geschäftspraktiken entspricht. Außerdem können einfache Regeln und kategorisieren inkonsistente tagging (Tippfehler und andere Diskrepanzen) und ohne Tags Ressourcen für genaue Kostenanalyse Zuordnung automatisch.

![Kategorie-Manager][7]

## <a name="video"></a>Video

Hier ist ein kurzes Video, wie ein Azure Kunden Cloudyn für Azure und Azure Abrechnung APIs zeigt Einblicke in ihre Azure Daten.

> [AZURE.VIDEO cloudyn-provides-cloud-itfm-tools-via-microsoft-azure-apis]


## <a name="next-steps"></a>Nächste Schritte

+ Testen Sie kostenlos [Cloudyn für Azure](https://www.cloudyn.com/microsoft-azure/) darauf, wie Sie Kostentransparenz mit benutzerdefinierten Berichten und Analysen für die Microsoft Azure-Cloud-Bereitstellung erhalten können.
+ Übersicht der Azure-Ressourcenverwendung und RateCard-APIs finden Sie unter [Einblicke in Ihre Microsoft Azure Ressourcenverbrauch](billing-usage-rate-card-overview.md) .
+ [Azure Abrechnung REST-API-Referenz](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) für Weitere Informationen zu beiden APIs sind Teil der APIs von Azure-Managers anzeigen
+ Checken Sie rechts in der Beispielcode befassen möchten, unserer Microsoft Azure Abrechnung API Codebeispiele für [Azure Codebeispiele](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>Weitere Informationen
+ Erfahren Sie mehr über Microsoft Azure Konzernvertrag (EA) Angebote finden Sie auf [Lizenzierung Azure für Unternehmen] (https://azure.microsoft.com/pricing/enterprise-agreement/)
+ Anzeigen Sie [Azure Ressourcenmanager](azure-resource-manager/resource-group-overview.md) Übersichtsartikel zu Azure-Ressourcen-Manager weitere
+ Weitere Informationen zu den Tools zu Verständnis der Cloud verbringen, finden Sie im Artikel Gartner [Marktübersicht IT Financial Management (ITFM) Tools](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb)erforderlich.

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
