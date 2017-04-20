<properties
    pageTitle="Modellieren von Tenancy in Azure suchen | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Erfahren Sie mehr über gängiger Entwurfsmuster für mandantenfähigen SaaS-Applikationen mit Azure Search."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/26/2016"
    ms.author="ashmaka"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Entwerfen von Mustern mandantenfähigen SaaS-Applikationen und Azure Search

Mandantenfähige Anwendung ist eine dieselben Dienste und Funktionen auf beliebig viele Mandanten können angezeigt oder Daten von anderen Mandanten. Dieses Dokument beschreibt Mieter Isolationsstrategien mandantenfähigen Anwendungsmöglichkeiten Azure Suche integriert.

## <a name="azure-search-concepts"></a>Azure Search-Konzepte
Suche als Service-Lösung kann Azure Search Entwickler umfangreiche Suchfunktionen Applikationen ohne Infrastruktur verwalten oder ein Experte Suche hinzuzufügen. Daten an den Dienst übertragen und dann in der Cloud gespeichert. Einfache Anfragen Azure Such-API verwenden, können die Daten geändert und durchsucht werden. Ein Überblick finden in [diesem Artikel](http://aka.ms/whatisazsearch). Entwurfsmuster erläutert, ist es wichtig, einige Konzepte in Azure Suche erforderlich.

### <a name="search-services-indexes-fields-and-documents"></a>Suchdienste, Indizes, Felder und Dokumente
Bei Azure Suche abonniert einen einem _Suchdienst_. Daten nach Azure hochladen, wird es mit der Suchdienst einen _Index_ gespeichert. Es kann eine Anzahl von Indizes in einem einzigen Dienst. Um die vertrauten Konzepte Datenbanken verwenden, kann der Suchdienst mit einer Datenbank verglichen werden, während Indizes innerhalb eines Dienstes mit Tabellen in einer Datenbank verglichen werden können.

Jeder Index in einen Suchdienst hat ihr eigenes Schema Anzahl anpassbare _Felder_definiert. Ein Azure-Suchindex in Form von einzelnen _Dokumenten_Daten hinzugefügt. Jedes Dokument muss muss an einem bestimmten Index hochgeladen werden und dieses IndexSchema. Beim Durchsuchen von Daten mithilfe von Azure Suche erfolgt die Volltextsuche Abfragen mit einem bestimmten Index.  Zum Vergleichen dieser Konzepte der Datenbank Felder können Spalten in einer Tabelle verglichen und Dokumente können Zeilen verglichen werden.

### <a name="scalability"></a>Skalierung
Jede Azure-Suchdienst in der [Tarif](https://azure.microsoft.com/pricing/details/search/) kann in zwei Dimensionen skaliert: Speicher und Verfügbarkeit.
* _Partitionen_ kann zu Speicher ein hinzugefügt werden.
* _Replikate_ können an eine Erhöhung des Durchsatzes der Anfragen, die ein behandeln können hinzugefügt werden.

Hinzufügen und Entfernen von Partitionen und Replikate mit können die Kapazität des Suchdienstes zu der Datenmenge Datenverkehr die Anforderungen. Für einen Suchdienst zu lesen [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)muss es zwei Replikate. Um einen Service zu einer schreibgeschützten [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)muss drei Replikationen.


### <a name="service-and-index-limits-in-azure-search"></a>Grenzwerte in Azure Search Service und index
In Azure Search gibt einige unterschiedliche [Preise Ebenen](https://azure.microsoft.com/pricing/details/search/) , Ebenen jeweils unterschiedliche [Grenzwerte und Kontingente](search-limits-quotas-capacity.md). Einige dieser Grenzwerte sind auf der Dienstebene einige bei der- und Ebene der Partition einige.


|                                  | Grundlegende     | Standard1   | Standard2   | Standard3   | Standard3 HD  |
|----------------------------------|-----------|-------------|-------------|-------------|---------------|
| Maximale Replikate pro Dienst     | 3         | 12          | 12          | 12          | 12            |
| Maximale Partitionen pro Dienst   | 1         | 12          | 12          | 12          | 1             |
| Maximale Suche Einheiten (Replikate * Partitionen) pro Dienst | 3         | 36          | 36          | 36          | 36 (maximal 3 Partitionen)            |
| Maximale Dateien pro Dienst    | 1 million | 180 Millionen | 720 Millionen | 1,4 Mrd. | 600 Millionen   |
| Maximaler Speicher pro Dienst      | 2 GB      | 300 GB      | 1,2 TB      | 2.4 TB      | 600 GB        |
| Maximale Dateien pro Partition  | 1 million | 15 Millionen  | 60 Millionen  | 120 Millionen | 200 Millionen   |
| Maximaler Speicher pro Partition    | 2 GB      | 25 GB       | 100 GB      | 200 GB      | 200 GB        |
| Maximale Indizes pro Dienst      | 5         | 50          | 200         | 200         | 3000 (max. 1000 Indizes/Partition)          |


#### <a name="s3-high-density"></a>S3 Hohe Dichte "
In Azure Search S3 Tarif ist eine Option für hohe Dichte (HD) Modus für mandantenfähigen Szenarien entwickelt. In vielen Fällen muss eine große Anzahl von kleineren Mieter unter einen einzelnen Dienst die Vorteile der Einfachheit und Effizienz zu unterstützen.

S3 HD können viele kleine Indizes unter der Leitung von einem einzelnen Suchdienst Handel, Indizes mit Partitionen können mehrere Indizes in einem einzigen Dienst hosten skalieren gepackt werden.

Konkret hätte S3 Service zwischen 1 und 200, die zusammen bis zu 1,4 Mrd. Dokumente hosten kann. Eine HD S3 andererseits könnten einzelne Indizes nur bis zu 1 Million Dokumente gehen jedoch bis zu 1000 Indizes pro Partition (bis zu 3000 pro Service) mit 200 Mio. pro Partition insgesamt Dokument behandeln (bis zu 600 Millionen pro Service).



## <a name="considerations-for-multitenant-applications"></a>Aspekte der mandantenfähigen Applikationen
Mandantenfähige Applikationen müssen Ressourcen unter den Mietern Beibehaltung gewisse Datenschutz zwischen den verschiedenen effektiv verteilen. Es gibt einige Aspekte beim Entwerfen der Architektur einer Anwendung:

* _Isolierung Mieter:_ Anwendung Entwickler müssen geeignete Maßnahmen, um sicherzustellen, dass keine Mieter nicht autorisierte oder Zugriff auf die Daten anderer Mandanten unerwünschten. Über die Perspektive der Daten erfordern Mieter Isolationsstrategien effektives Management von Ressourcen und Schutz vor lauten Nachbarn.
* _Cloud Ressourcenkosten:_ Wie bei jeder anderen Anwendung müssen Software Solutions als Komponente einer mehrinstanzenfähigen Anwendung wettbewerbsfähig bleiben.
* _Einfache Operationen:_ Beim Entwickeln einer mandantenfähigen Architektur wirkt sich auf Operationen und Komplexität der Anwendung berücksichtigt. Azure Suche hat eine [SLA 99,9 %](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* _Weltweit:_ Mandantenfähige Applikationen müssen effektiv Mieter dienen die weltweit verteilt sind.
* _Skalierbar:_ Anwendungsentwickler müssen berücksichtigen, wie sie zwischen ausreichend geringe Anwendungskomplexität verwalten und Entwerfen der Anwendung mit Anzahl der Mandanten und die Größe der Daten und die Arbeitslast Mieter abstimmen.

Azure Suche bietet einige Grenzen, mit der Daten und die Arbeitslast Mieter isolieren.

## <a name="modeling-multitenancy-with-azure-search"></a>Modellieren von Tenancy mit Azure suchen
Bei einem Szenario mandantenfähigen Entwickler benötigt mindestens Suchdienste und Teilen Mieter unter Dienste oder Indizes. Azure Suche hat einige gemeinsame Muster mandantenfähigen Szenario modellieren:

1. _Index pro Mandant:_ Jeder hat einen eigenen Index in einen Suchdienst mit anderen gemeinsam genutzt wird.
1. _Pro Mieter:_ Jeder hat eine eigene dedizierte Azure Suchdienst bietet höchste Daten und Arbeitslast.
1. _Mischung beider:_ Größer, mehr aktiv Mieter werden Dienstleistungen zwar kleinere Mieter einzelnen Indizes in gemeinsamen Diensten zugeordnet sind.

## <a name="1-index-per-tenant"></a>1. index pro Mandant
![Eine Darstellung des Index pro Tenant-Modells](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

In einem Modell Index pro Mieter belegen mehrere Mandanten einen einzelnen Azure-Suchdienst hat jeder eigene Index.

Mieter erreichen Datenisolation, weil alle Anfragen suchen und Dokument-Operationen bei einem Azure Suche ausgegeben. In der Anwendungsebene ist Bewusstsein müssen die verschiedenen Mieter Datenverkehr die entsprechenden Indizes direkt und gleichzeitig Ressourcen auf Dienstebene für alle.

Ein Schlüsselattribut Index pro Tenant-Modells ist die Möglichkeit für Entwickler, die Kapazität der Suchdienst unter der Anwendung Mieter überzeichnen. Haben die Mieter eine ungleichmäßige Verteilung der Arbeitslast kann einen Suchdienst Indizes für eine Anzahl der aktiv, ressourcenintensive Mieter und gleichzeitig eine lange Ende weniger aktiven Mandanten die optimale Kombination Mieter verteilt. Der Kompromiss ist nicht das Modell Situationen jeder gleichzeitig aktiv ist.

Index pro Mieter Modell bildet die Grundlage für ein Modell Variable Kosten, wo ein gesamte Azure-Suchdienst vorab gekauft und dann später mit. Dadurch können ungenutzte Kapazität für Test- und kostenlose Konten bezeichnet werden.

Für Applikationen globale Index pro Mieter Modell am effizientesten möglicherweise nicht. Wenn eine Anwendung Mieter weltweit verteilt sind, separater Dienst für jede Region möglicherweise Kosten für jeden von ihnen duplizieren können.

Azure Suche ermöglicht die Skalierung der einzelnen Indizes und die Gesamtzahl der Indizes zu. Wenn eine entsprechende Preise wurde Ebene ausgewählt, Partitionen und Replikate der gesamte Dienst hinzugefügt werden können als ein einzelner Index innerhalb des Dienstes Speicher oder Datenverkehr zu groß wird.

Gesamtanzahl Indizes für einen einzelnen Dienst zu groß wird, ist ein anderer Dienst bereitgestellt werden, um neue Mieter aufzunehmen. Indizes müssen zwischen Suchdienste verschoben werden, neue Dienste hinzugefügt werden, hat die Daten aus dem Index manuell aus einem Index zum anderen kopiert werden da Azure Suche nicht für einen Index verschoben werden soll.


## <a name="2-service-per-tenant"></a>2. Service pro Mandant
![Eine Darstellung des Dienst pro Tenant-Modells](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

In einer Architektur Dienst pro Mieter hat jeder einen eigenen Suchdienst

Dieses Modell erreicht die Anwendung die maximale Isolierung Mieter. Jeder Dienst widmet Speicher- und Durchsatz für Suche sowie separate API-Schlüssel.

Anwendungsmöglichkeiten, wo jeder große Stellfläche oder die Arbeitslast etwas variieren Mieter, Pächter, entspricht Dienst pro Tenant-Modells geeigneten Ressourcen in verschiedenen Mieter Arbeitslasten nicht freigegeben werden.

Dienst pro Tenant-Modells bietet den Vorteil eines Kostenmodells vorhersehbare, feste. Gibt es keinen Vorabinvestitionen in einem gesamten Suchdienst bis sich Mieter auszufüllen, Kosten pro Mieter ein Index pro Mieter Modell größer ist.

Dienst pro Tenant-Modell ist eine globale Anwendungsmöglichkeiten. Mit geografisch verteilt ist es einfach, des Mieters Dienst in der entsprechenden Region.

Die Herausforderung in diesem Muster skalieren entstehen, wenn einzelne Mieter Dienst ausreicht. Azure Suche unterstützt derzeit nicht aktualisieren Tarif einen Suchdienst, so dass alle Daten einen neuen Dienst manuell kopiert werden.

## <a name="3-mixing-both-models"></a>3. mischen beide Modelle
Ein anderes Muster für die Modellierung von Tenancy ist Index pro Mieter und Dienst pro Mieter Strategien mischen.

Mischen Sie zwei Muster, können eine Anwendung größte Mieter Dienstleistungen belegen, während long Tail von weniger aktiven, kleinere Mieter Indizes in einem gemeinsam genutzten Dienst beschäftigen. Dieses Modell wird sichergestellt, dass die größte Mieter hohe Leistung vom Dienst kleinere Mieter laut Nachbarn schützen.

Diese Strategie hängt jedoch Zukunftsforschung Vorhersage der Mieter einen dedizierten Dienst im Vergleich zu Index freigegebener Dienst benötigen. Anwendungskomplexität nimmt mit beiden mandatenfähiges Modelle verwalten müssen.

## <a name="achieving-even-finer-granularity"></a>Auch eine feinere Granularität erreichen
Die obigen Entwurfsmuster in Azure Search mandantenfähigen Szenarien übernehmen einen einheitlichen Bereich jeder einer ganzen Instanz einer Anwendung ist. Clientanwendungen können jedoch manchmal viele kleinere Bereiche behandeln.

Wenn Dienst pro Mieter und Index pro Mieter nicht ausreichend kleine Bereiche sind, kann einen Index auch feinere Granularität zu modellieren.

Um einen einzelnen Index für andere Clientendpunkte Verhalten kann ein Feld ein Index hinzugefügt werden einen bestimmten Wert für jeden möglichen Client ausweist. Jedes Mal ein Client ruft Azure Suche einen Index zu Abfragen gibt den Code von der Clientanwendung den entsprechenden Wert für dieses Feld Abfragezeit [Filterfunktion Azure Suche](https://msdn.microsoft.com/library/azure/dn798921.aspx) mit.

Diese Methode kann verwendet werden, um Funktionalität separate Benutzerkonten verschiedene Berechtigungsebenen und komplett separate Programme.

## <a name="next-steps"></a>Nächste Schritte
Azure suchen ist eine hervorragende Wahl für häufig [mehr Informationen über den Dienst leistungsfähigen Funktionen](http://aka.ms/whatisazsearch). Bei verschiedenen Entwurfsmuster für mandantenfähigen Applikationen sollten Sie [verschiedene Tarifen](https://azure.microsoft.com/pricing/details/search/) und die entsprechenden [Service-Grenzwerte](search-limits-quotas-capacity.md) beste Schneider Azure Search Arbeitslasten und Architekturen aller Größen.

Fragen zur Azure Search und mandantenfähigen Szenarien zu richten azuresearch_contact@microsoft.com.
