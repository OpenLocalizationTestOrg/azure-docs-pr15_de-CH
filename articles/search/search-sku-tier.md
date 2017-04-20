<properties
    pageTitle="Wählen Sie eine SKU oder Tarif suchen Azure | Microsoft Azure"
    description="Azure Suche auf diese SKUs bereitgestellt werden: frei, Basic und Standard, Standard ist in verschiedenen Ressourcenkonfigurationen und Kapazität."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/24/2016"
    ms.author="heidist"/>

# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Wählen Sie eine SKU oder Tarif Azure Suche

In Azure Search ein [Dienst bereitgestellt](search-create-service-portal.md) eine bestimmte Preisstufe oder SKU. Optionen **frei**, **Basic**oder **Standard**, **Standard** in mehreren Konfigurationen und Kapazität verfügbar ist. 

Dieser Artikel soll Sie eine Ebene auswählen. Stellt eine Ebene Kapazität heraus zu niedrig, müssen Sie einen neuen Dienst auf der höheren Ebene bereitstellen und Laden Ihre Indizes. Es gibt keine direkte Aktualisierung denselben Dienst von einer SKU auf eine andere. 

> [AZURE.NOTE] Nach Auswahl einer Ebene und [ein](search-create-service-portal.md)Replikat erhöhen und Partition innerhalb des Dienstes zählt. Siehe [Skalieren Ressourcenebenen für Abfrage und Indizierung Arbeitslasten](search-capacity-planning.md).

## <a name="how-to-approach-a-pricing-tier-decision"></a>Umgang mit einer Ebene Preisgestaltung

In Azure Suche bestimmt die Ebene Kapazität nicht zur Verfügbarkeit. Im Allgemeinen sind die Funktionen jeder Ebene, einschließlich der Vorschau verfügbar. Die Ausnahme wird nicht unterstützt für Indexer in S3 HD.

> [AZURE.TIP] Wir empfehlen immer **frei** Dienst (eine pro Abonnement permanenten) bereitstellen, damit seine verfügbar für leichte Projekte. Verwenden Sie den Dienst **kostenlos** für Test- und Evaluierungszwecke; Erstellen Sie einen zweiten fakturierbaren Service auf der Stufe **Basic** oder **Standard** für Produktion oder größere Arbeitslasten test

Kapazität und Kosten der Ausführung des Dienstes gehen Hand in Hand. In diesem Artikel können Sie entscheiden, welche SKU Gleichgewicht bietet, aber damit es nützlich sein, benötigen mindestens groben Schätzung auf:

- Anzahl und Größe der Indizes zu erstellen
- Anzahl und Größe der Dokumente hochladen
- Vorstellen der Abfrage Volume in Abfragen pro zweite (QPS)

Anzahl und Größe sind wichtig, da eine Obergrenze für die Anzahl der Indizes oder Dokumente in einem Dienst und vom Dienst verwendeten Ressourcen (Speicher oder Replikate) Grenzwerte erreicht werden. Der tatsächliche Grenzwert für den Dienst ist je nachdem, was zuerst verwendet: Ressourcen oder Objekte.

Mit Hand sollte folgendermaßen vereinfachen:

- **Schritt 1** Überprüfen Sie die SKU Beschreibungen verfügbaren Optionen.
- **Schritt 2** Fragen Sie eine vorläufige Entscheidung treffen.
- **Schritt 3** Schließen Sie Ihre Entscheidung harte Grenzen Speicher überprüfen und Preise.

## <a name="sku-descriptions"></a>SKU-Beschreibung

Die folgende Tabelle enthält eine Beschreibung der einzelnen Ebenen. 

Ebene|Primäre Szenarien
----|-----------------
**Frei**|Eine gemeinsame Dienste kostenlos zur Evaluierung, Untersuchung oder kleinen Arbeitslasten. Freigegeben mit anderen Teilnehmern hängt abfragedurchsatz und Indizierung wer den Dienst verwendet. Kapazität ist klein (50 MB oder 3 Indizes mit 10.000 Dokumente jeweils).
**Grundlegende**|Kleine Arbeitslasten auf dedizierter Hardware. Hoher Verfügbarkeit. Beträgt 3 Replikate bis 1-Partition (2 GB).
**S1**|1-Standard unterstützt flexible Kombinationen von Partitionen (12) und Replikate (12) für mittlere Produktionsarbeitslasten auf dedizierter Hardware verwendet. Sie können Partitionen und Replikate in Kombinationen von maximal 36 berechenbare Suche Einheiten unterstützt zuweisen. Diese Partitionen sind 25 GB und QPS ist ca. 15 Abfragen pro Sekunde.
**S2**|Standard 2 führt größere Produktionsarbeitslasten mit den gleichen sucheinheiten 36 S1 als größere Größe Partitionen und Replikate. Diese Partitionen sind 100 GB und QPS ist ca. 60 Abfragen pro Sekunde.
**S3**|Standard 3 wird proportional größeren Produktionsarbeitslasten auf höhere Endsystemen in Konfigurationen mit bis zu 12 Partitionen oder 12 Replikate ausgeführt, die 36 Suche Einheiten. Diese Partitionen sind 200 GB und QPS ist 60 Abfragen pro Sekunde. 
**S3 HD**|Standard 3 HD wurde für eine große Anzahl kleiner Indizes. Sie können bis zu 3 Partitionen 200 GB. QPS ist 60 Abfragen pro Sekunde. 

> [AZURE.NOTE] Replikat und Partition Höchstwerte werden als Suche Einheiten (36 Einheit pro Service), eine effektive Untergrenze als impliziert die maximale Nennwert auferlegt, berechnet. Beispielsweise um maximal 12 Replikate verwenden, konnten Sie maximal 3 Partitionen (12 * 3 = 36 Einheiten). Reduzieren Sie entsprechend um maximale Partitionen verwenden, Replikate 3. Ein Diagramm auf zulässige Kombinationen finden Sie unter [Skalierung Ressourcenebenen für Abfrage und Indizierung Arbeitslasten in Azure Suche](search-capacity-planning.md) .

## <a name="review-limits-per-tier"></a>Überprüfen Sie die einzelnen Ebenen

Im folgende Diagramm ist eine Teilmenge der Grenzen von [Service in Azure Suche](search-limits-quotas-capacity.md). Wahrscheinlich eine SKU Entscheidung beeinflussen Faktoren aufgeführt. Dieses Diagramm finden Sie bei den folgenden Fragen.

Ressource|Frei|Grundlegende|S1|S2|S3 |S3 HD
---|---|---|---|----|---|----
Vereinbarung zum Servicelevel (SLA)|Nr. <sup>1</sup> |Ja |Ja  |Ja |Ja  |Ja 
Index-Grenzwerte|3|5|50|200|200|1000 <sup>2</sup>
Dokument-Grenzwerte|insgesamt 10.000|1 Million pro Dienst|15 Mio. pro partition |60 Millionen pro partition|120 Millionen pro partition |1 Million pro index
Maximale Partitionen|N/A |1 |12  |12 |12|3 <sup>2</sup>
Größe der Partition|50 MB gesamt|2 GB pro Dienst|25 GB pro partition |100 GB pro Partition (maximal 1,2 TB pro Dienst)|200 GB pro Partition (maximal 2,4 TB pro Dienst)|200 GB (maximal 600 GB pro Dienst)
Maximale Replikate|N/A |3 |12 |12 |12|12
Abfragen pro Sekunde|N/A|3 pro Replikat|~ 15 pro Replikat|60 pro Replikat|> 60 pro Replikat|> 60 pro Replikat

<sup>1</sup> frei und Vorschau SKUs SLAs kommen nicht. SLAs werden erzwungen, sobald eine SKU erhältlich ist.

<sup>2</sup> sind S3 und S3 HD durch dieselbe hohe Kapazität Infrastruktur jedoch jeweils unterschiedlich seine Obergrenze erreicht. S3 zielt auf eine kleinere Anzahl großer Indizes. Der Höchstwert ist Ressource gebunden (2,4 TB für jeden Dienst). S3 HD zielt auf eine große Anzahl sehr kleiner Indizes. 1.000 Indizes erreicht S3 HD Grenzen in Form eines Index-Einschränkungen. Wenden Sie ein S3 HD-Kunden, der mehr als 1.000 Indizes erfordert, Microsoft Support Informationen fortgesetzt.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>SKUs, die Anforderungen entfernen 

Die folgenden Fragen helfen Ihnen die richtige Entscheidung SKU für Ihre Arbeitslast ankommen.

1. Haben **Service Level Agreement (SLA)** Vorschriften? SKU Entscheidung Basis- oder -Vorschau Standard eingrenzen.
2. **Wie viele Indizes** sind Sie benötigen? Einer der größten Einteilung in einer SKU ist die Anzahl der Indizes von Lagerhaltungsdaten unterstützt. Index-Unterstützung ist deutlich hinsichtlich der unteren Tarifen. Anforderungen an die Anzahl der Indizes möglicherweise eine primäre Determinante einer SKU.
3. **Wie viele Dokumente** wird in jedem Index geladen werden? Die Anzahl und Größe der Dokumente bestimmen die tatsächliche Größe des Index. Wenn die voraussichtliche Größe des Indexes zu schätzen, vergleichen Sie die Zahl mit der Partitionsgröße pro SKU erweitert, indem die Anzahl der Partitionen erforderlich, um einen Index der Größe speichern. 
4. **Was ist die erwartete Auslastung**? Speicherbedarf erfasst, sollten Sie die Abfrage Arbeitslasten. S2 und beide S3-SKUs bieten nahezu äquivalente Durchsatz jedoch SLA-Ansprüche werden Vorschau SKUs ausschließen. 
5. Wenn Sie, S2 oder S3-Ebene erwägen, feststellen Sie, ob [Indexer](search-indexer-overview.md)benötigen. Indexer sind noch nicht für die S3 HD-Ebene. Alternativer Ansatz ist ein Push-Modell für Index-Updates verwenden, Schreiben Sie Anwendungscode müssen ein Dataset einen Index.

Die meisten Kunden können bestimmte Lagerhaltungsdaten oder Regel basierend auf ihren Antworten auf die obigen Fragen. Wenn Sie weiterhin die SKU mit wissen, können Sie Fragen zu MSDN oder StackOverflow Foren oder Azure Support Weitere.

## <a name="decision-validation-does-the-sku-offer-sufficient-storage-and-qps"></a>Validierung Entscheidung: SKU bietet ausreichend Speicherplatz und QPS?

Als letzter Schritt erneut [pro und pro Index Abschnitte Service Grenzen](search-limits-quotas-capacity.md) und [Preisseite](https://azure.microsoft.com/pricing/details/search/) um Schätzung für Abonnements und Service Grenzen überprüfen. 

Wenn der Preis oder Speicher überschritten sind, empfiehlt es sich, der Arbeitslast auf mehrere kleinere Dienste (zum Beispiel) Umgestalten. Auf präziser konnte Indizes werden neu entwerfen oder Filter verwenden, um Abfragen effizienter zu gestalten.

> [AZURE.NOTE] Speicherbedarf können stark vergrößerte, wenn Dokumente überflüssige Daten enthalten. Dokumente im Idealfall enthalten nur durchsuchbare Daten oder Metadaten. Binäre Daten können nicht durchsucht werden und sollte (vielleicht in einem Azure Tabelle oder BLOB-Speicher) mit einem Feld im Index zu einen URL-Verweis auf externen Daten separat gespeichert. Die maximale Größe eines einzelnen Dokuments beträgt 16 MB (oder weniger, wenn Sie mehrere Dokumente in einer Anforderung Bulk). Weitere Informationen finden Sie unter [Service Grenzwerte in Azure Suche](search-limits-quotas-capacity.md) .

## <a name="next-step"></a>Nächstes

Wenn Sie die SKU passen kennen, fahren Sie mit folgendermaßen:

- [Erstellen Sie einen Suchdienst im portal](search-create-service-portal.md)
- [Ändern der Zuweisung von Partitionen und Replikaten Ihren Dienst skalieren](search-capacity-planning.md)

