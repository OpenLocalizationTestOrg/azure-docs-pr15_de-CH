<properties
    pageTitle="Konsistenzebenen DocumentDB | Microsoft Azure"
    description="DocumentDB weist vier Konsistenz Saldo eventual Consistency, Availability, und Wartezeit Kompromisse."
    keywords="Eventual Consistency Documentdb, Azure, Microsoft azure"
    services="documentdb"
    authors="syamkmsft"
    manager="jhubbard"
    editor="cgronlun"
    documentationCenter=""/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="syamk"/>

# <a name="consistency-levels-in-documentdb"></a>Konsistenzebenen in DocumentDB

Azure DocumentDB wird vom Boden mit globalen dabei entwickelt. Es bietet vorhersagbare Latenzzeit garantiert eine SLA 99,99 % Verfügbarkeit und mehrere klar definierte entspannt Konsistenz Modelle. Derzeit DocumentDB bietet vier Konsistenz: starke, begrenzt Veraltung Sitzung und. **Starke** und **eventual Consistency** häufig angeboten von anderen NoSQL-Datenbanken DocumentDB bietet zwei sorgfältig kodifiziert und standardisierten Konsistenz – **begrenzt Alterung** und **Sitzung**auch bestätigt ihre Nützlichkeit gegen Praxis verwenden. Gemeinsam können diese vier konsistenzebenen begründeten Kompromisse zwischen Konsistenz, Verfügbarkeit und Wartezeit. 

## <a name="scope-of-consistency"></a>Umfang der Konsistenz

Die Granularität der Konsistenz wird auf Anforderung eines einzelnen Benutzer beschränkt. Schreiben kann ein einfügen, ersetzen, Upsert entsprechen oder Transaktion (mit oder ohne die Ausführung einer zugeordneten Trigger vor oder nach) löschen. Oder schreiben kann die Ausführung von Transaktionen einer JavaScript gespeicherte Prozedur über mehrere Dokumente innerhalb einer Partition. Mit schreibt, wird eine Transaktion lesen-Abfrage auch einzelnen Benutzern beschränkt. Der Benutzer möglicherweise paginieren über einen großen Resultset, umfasst mehrere Partitionen, aber jede Transaktion für eine einzelne Seite begrenzt und bedient wurden, in eine einzelne Partition lesen.

## <a name="consistency-levels"></a>Konsistenzebenen

Sie können eine Standardsicherheitsstufe Konsistenz auf Ihrem Konto konfigurieren, die für alle Sammlungen (aller Datenbanken) unter Ihrem Konto gilt. Standardmäßig verwendet alle Lese- und Abfragen in der benutzerdefinierten Ressourcen die Konsistenz Standardstufe für das Konto angegeben. Allerdings lädt auf Konsistenz einer Anforderung bestimmte lesen-Abfrage durch Angabe des Anforderungsheaders [[X-ms-Konsistenz-Ebene]](https://msdn.microsoft.com/library/azure/mt632096.aspx) . Es gibt vier Typen Konsistenz Ebenen Replikationsprotokoll DocumentDB unterstützt, die klaren Kompromiss zwischen bestimmten Konsistenzgarantien und Performance bieten wie unten beschrieben.

![DocumentDB bietet mehrere definiert auch Konsistenz (relaxed) Modelle][1]

**Starke**: 

- Starker Konsistenz bietet eine [Linearizability](https://aphyr.com/posts/313-strong-consistency-models) mit der die neueste Version eines Dokuments zurückgeben. 
- Starker Konsistenz gewährleistet, dass Schreibvorgänge nur nach dem dauerhaft vom Kollegium Mehrheit der Replikationen engagiert sichtbar ist. Ein Schreibvorgang entweder synchron tritt als die primäre und sekundäre Kollegium oder abgebrochen wird. Lesen immer lesen Quorum Mehrheit bestätigt, ein Client sehen nie einen Schreibvorgang nicht festgeschriebenen oder teilweisen garantiert immer die neuesten anerkannte schreiben lesen. 
- Starken Konsistenz konfiguriert sind DocumentDB-Konten können nicht mehrere Azure-Region DocumentDB Konto zuordnen. 
- Die Kosten für einen Lesevorgang (in [anforderungseinheiten](documentdb-request-units.md) verbraucht) mit starker Konsistenz ist höher als Sitzung und eventuelle, aber begrenzten Veraltung identisch.
 

**Bounded Alterung**: 

- Alterung Konsistenz gewährleistet, die die Lesevorgänge von höchstens *K* Versionen oder Präfixe eines Dokuments oder *t* schreibt hinterherhinken können begrenzt Zeitintervall. 
- Wenn Veraltung Auswahl begrenzt werden, kann folglich "Alterung" auf zwei Arten konfiguriert werden: 
    - Anzahl der Dokumentversionen *K* , die Lesevorgänge die Schreibvorgänge Rückstand
    - Zeitintervall *t* 
- Alterung bietet globale Auftragssumme außer im Fenster"Alterung" begrenzt. Hinweis monotone lesen garantiert innerhalb eines Bereichs innerhalb und außerhalb der "Alterung Fenster" ist vorhanden. 
- Begrenzte Veraltung bietet eine Garantie für erhöhte Konsistenz als Sitzung oder eventual Consistency. Für Global verteilten Anwendung empfehlen wir Sie begrenzte Alterung für Szenarios verwenden, möchten Sie starke Konsistenz aber auch 99,99 % Verfügbarkeit und niedrige Latenz. 
- DocumentDB-Konten mit begrenzten Veraltung Konsistenz können eine beliebige Anzahl von Azure-Regionen mit seinem DocumentDB-Konto zuordnen. 
- Die Kosten für einen Lesevorgang (in RUs verbraucht) begrenzte Alterung ist höher als Sitzung und eventual Consistency aber starke Konsistenz identisch.

**Sitzung**: 

- Session-Konsistenz ist anders als die globale Konsistenz Modelle starke und begrenzte Veraltung konsistenzebenen auf eine Clientsitzung beschränkt. 
- Session-Konsistenz ist ideal für alle Szenarien, Gerät oder Benutzer-Sitzung handelt, da garantiert monotonen liest, monotonen schreibt und Lesen eigener Schreibvorgänge (RYW) gewährleistet. 
- Session-Konsistenz bietet vorhersagbare Konsistenz für eine Sitzung und maximale Durchsatz die niedrigste Latenz schreibt und liest gelesen. 
- DocumentDB-Konten mit Sitzung Konsistenz können eine beliebige Anzahl von Azure-Regionen mit seinem DocumentDB-Konto zuordnen. 
- Die Kosten für einen Lesevorgang (in RUs verbraucht) mit Sitzung Konsistenz ist weniger als starke und begrenzte Alterung aber über eventual Consistency
 

**Eventuelle**: 

- Eventual Consistency sichergestellt, dass keine weiteren schreibt, schließlich von Replikaten in der Gruppe innerhalb. 
- Eventual Consistency ist die schwächste Form von Konsistenz kann ein Client die Werte erhalten, die älter als die zuvor gesehen hatte.
- Eventual Consistency bietet die schwächste gelesen aber bietet die niedrigste Latenz für Lese- und Schreibvorgänge.
- DocumentDB-Konten mit mit eventual Consistency können eine beliebige Anzahl von Azure-Regionen mit seinem DocumentDB-Konto zuordnen. 
- Die Kosten für einen Lesevorgang (in RUs verbraucht) mit eventual Consistency ist der niedrigste aller DocumentDB Konsistenz Ebenen.


## <a name="consistency-guarantees"></a>Konsistenzgarantien

In der folgenden Tabelle werden verschiedene Konsistenzgarantien entspricht vier konsistenzebenen erfasst.

| Garantie                                                         |    Starke                                       |    Begrenzte Alterung                                                                           |    Sitzung                                       |    Eventuelle                                 |
|----------------------------------------------------------|-------------------------------------------------|------------------------------------------------------------------------------------------------|--------------------------------------------------|--------------------------------------------------|
|    **Globale Auftragssumme**                                |    Ja                                          |    Ja, außerhalb der"Alterung"                                                      |    Nein, "Session" partielle Reihenfolge                   |    Nein                                            |
|    **Einheitliche Präfix Garantie**                       |    Ja                                          |    Ja                                                                                         |    Ja                                           |    Ja                                           |
|    **Monotonen Lesevorgänge**                                   |    Ja                                          |    Ja, in Regionen außerhalb der Alterung und innerhalb eines Bereichs ständig.     |    Ja, für die Sitzung                    |    Nein                                            |
|    **Monotonen schreibt**                                  |    Ja                                          |    Ja                                                                                         |    Ja                                           |    Ja                                           |
|    **Lesen Sie die Schreibvorgänge**                                  |    Ja                                          |    Ja                                                                                         |    Ja (im Bereich schreiben)                      |    Nein                                            |


## <a name="configuring-the-default-consistency-level"></a>Konfigurieren die Standardstufe Konsistenz

1.  Klicken Sie im [Azure-Portal](https://portal.azure.com/), in dem Indexleiste auf **DocumentDB (NoSQL)**.

2. Blatt **DocumentDB (NoSQL)** wählen Sie das Konto ändern.

3. Blatt Konto klicken Sie auf **Konsistenz**.


4. Blatt **Standardkonsistenz** wählen Sie die neue Konsistenz und klicken Sie auf **Speichern**.

    ![Screenshot Symbol und Standardkonsistenz Eintrag markieren](./media/documentdb-consistency-levels/database-consistency-level-1.png)

## <a name="consistency-levels-for-queries"></a>Konsistenzebenen für Abfragen

Benutzerdefinierte Ressourcen wird standardmäßig auf Konsistenz für Abfragen Konsistenz Niveau für Lesevorgänge. Standardmäßig ist der Index synchron auf jede einfügen, ersetzen oder Löschen eines Dokuments, das die Auflistung aktualisiert. Dadurch können Abfragen der Konsistenz Niveau des Dokuments lautet berücksichtigt. DocumentDB ist optimiert und unterstützt nachhaltig Volumes Dokument schreibt synchron Indexwartung und konsistente Abfragen dienen, können Sie bestimmte Sammlungen Index verzögert aktualisieren konfigurieren. Verzögerte Indizierung weiter steigert die Schreib-Performance, und eignet sich für Massenkopieren Einnahme Szenarien wird eine Arbeitslast hauptsächlich Lesevorgänge.  

Indizierung Modus|  Liest|  Abfragen  
-------------|-------|---------
Konsistente (Standard)|   Wählen Sie sicheres, begrenzte Veraltung Sitzung oder tatsächlichen|    Wählen Sie sicheres, begrenzte Veraltung Sitzung oder tatsächlichen|
Verzögerte|   Wählen Sie sicheres, begrenzte Veraltung Sitzung oder tatsächlichen|    Eventuelle  

Wie können Leseanfragen, Sie Konsistenz Maß eine bestimmte Abfrage senken durch [X ms Konsistenz-auf -](https://msdn.microsoft.com/library/azure/mt632096.aspx) Anfrage-Header angeben.

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie mehr lesen konsistenzebenen und Kompromisse, empfehlen wir die folgenden Ressourcen:

-   Doug Terry. Konsistenz der replizierten Daten erklärt durch Baseball (Video).   
[https://www.YouTube.com/watch?v=gluIh8zd26I](https://www.youtube.com/watch?v=gluIh8zd26I)
-   Doug Terry. Konsistenz der replizierten Daten erklärt durch Baseball.   
[http://Research.Microsoft.com/pubs/157411/ConsistencyAndBaseballReport.PDF](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
-   Doug Terry. Sitzung Garantien für schwach konsistente replizierten Daten.   
[http://DL.ACM.org/Citation.cfm?id=383631](http://dl.acm.org/citation.cfm?id=383631)
-   Daniel Abadi. Konsistenz vor-und Nachteile in modernen verteilte Systeme Datenbankentwurf: CAP ist nur ein Teil der Wahrheit ".   
[http://Computer.org/CSDL/mags/Co/2012/02/mco2012020037-Abs.HTML](http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html)
-   Peter Bailis Shivaram Venkataraman, Michael J. Franklin, Joseph M. Hellerstein, Ion Stoica. Probabilistisch Alterung (PBS) für praktische teilweise Quorumdatenträger begrenzt.   
[http://VLDB.org/pvldb/vol5/p776_peterbailis_vldb2012.PDF](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
-   Werner Vogels. Eventuelle einheitlich – Revisited.    
[http://allthingsdistributed.com/2008/12/eventually_consistent.HTML](http://allthingsdistributed.com/2008/12/eventually_consistent.html)


[1]: ./media/documentdb-consistency-levels/consistency-tradeoffs.png
