<properties 
    pageTitle="Anwendungsfälle für allgemeine DocumentDB | Microsoft Azure" 
    description="Erfahren Sie mehr über die oberen fünf Anwendungsfälle für DocumentDB: Benutzer Inhalt, Protokollierung Katalogdaten, Voreinstellungen Benutzerdaten und Internet der Dinge (IoT) generiert." 
    services="documentdb" 
    authors="h0n" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="hawong"/>

# <a name="common-documentdb-use-cases"></a>DocumentDB Einsatzbereiche
Dieser Artikel bietet eine Übersicht über verschiedene Einsatzbereiche für DocumentDB.  Die Ratschläge in diesem Artikel dienen als Ausgangspunkt bei der Entwicklung Ihrer Anwendung mit DocumentDB.   

Nach dem Lesen dieses Artikels werden Sie folgenden Fragen beantworten: 
 
- Was sind die Einsatzbereiche für DocumentDB?
- Was sind die Vorteile der Verwendung von DocumentDB als Benutzer generiert Content-Speicher?
- Was sind die Vorteile der Verwendung von DocumentDB als Katalogdaten speichern?
- Was sind die Vorteile der Verwendung von DocumentDB als ein Ereignisprotokoll speichern?
- Was sind die Vorteile der Verwendung von DocumentDB als Benutzer Voreinstellungen Daten speichern?
- Was sind die Vorteile der Verwendung von DocumentDB als Datenspeicher für Internet der Dinge (IoT) Systeme?

## <a name="common-use-cases-for-documentdb"></a>Einsatzbereiche für DocumentDB
Azure DocumentDB ist eine allgemeine NoSQL-Datenbank, die eine Vielzahl von Programmen und Anwendungsfälle verwendet wird. Es ist eine gute Wahl für jede Anwendung, die niedrige Antwortzeiten Reihenfolge Millisekunden und schnell skalieren. Nachfolgend einige Attribute des DocumentDB, die für leistungsfähige Anwendung erleichtern.

- DocumentDB Partitionen direkt die Daten für hohe Verfügbarkeit und Skalierung.
- Die DocumentDB hat SSD unterstützt Speicher mit geringer Latenz Reihenfolge Millisekunden Reaktionszeiten.
- DocumentDB Unterstützung für konsistenzebenen wie eventuelle, Sitzung begrenzt Veraltung ermöglicht niedrige Kosten, Performance-Verhältnis. 
- DocumentDB ist ein flexibles Daten geeignet Preismodell, das Speicher- und Durchsatz separat Meter.
- DocumentDB des können reservierten Durchsatz Sie Anzahl der Lese-und Schreibvorgänge anstelle der zugrunde liegenden Hardware CPU/Speicher/IO denken.
- DocumentDB Entwurf können massive Anforderung Volumes in Milliarden Anfragen pro Tag skalieren.

Diese Attribute sind besonders Web, mobil, Spiele und IoT-Anwendung, die niedrige Antwortzeiten und riesige Mengen von Lese- und Schreibvorgängen. 

## <a name="user-generated-content"></a>Benutzergenerierte Inhalte
Ein häufig verwendet für DocumentDB wird zum Speichern und Abfrage benutzergenerierter Inhalte (UGC) und Windows-Dienste, insbesondere sozialer Mediaprogramme.  Beispiele für UGC sind chatten, Tweets Blogbeiträge, Bewertung und Kommentare.  UGC in sozialer Mediaprogramme besteht häufig aus frei Formulartext Eigenschaften, Tags und Beziehungen, die starre Struktur nicht begrenzt.   

Inhalte wie Chats, können Kommentare und Beiträge in DocumentDB gespeichert werden ohne Transformationen oder komplexen Objekts in objektrelationalen zuordnungsebenen.  Eigenschaften können hinzugefügt oder Vorschriften entsprechen den Anwendungscode Entwickler durchlaufen so schnelle Entwicklung leicht geändert.  

Anwendung in verschiedenen Netzwerken integrieren müssen reagieren auf diesen Schemas ändern.  Wie Daten in DocumentDB standardmäßig automatisch indiziert ist, können Daten jederzeit abgefragt werden.  Daher haben diesen Flexibilität Projektionen nach ihren jeweiligen Bedürfnissen abrufen.       

Viele social Applications weltweit ausgeführt und können zu unvorhersehbaren Verwendungsmuster aufweisen.  Flexibilität bei der Skalierung des Datenspeichers ist wichtig wie die Anwendungsschicht Auslastung entsprechend skaliert.  Sie können Skalierung durch Hinzufügen zusätzlicher Partitionen unter einem DocumentDB-Konto.  Darüber hinaus können Sie zusätzliche DocumentDB Konten in mehreren Regionen erstellen. DocumentDB Bereich Verfügbarkeit finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/#services).   

## <a name="catalog-data"></a>Katalogdaten
Katalog Daten Verwendungsszenarios umfassen speichern und Abfragen einen Satz von Attributen für Entitäten wie Personen, Orte und Produkte.  Beispiele für Katalogdaten sind Benutzerkonten, Produktkataloge, Gerät Register IoT und Stücklisten Materialien Systeme.  Attribute für diese Daten variieren und Anwendung Anforderungen ändern können.  

Als Beispiel eines Produktkatalogs für einen automobilzulieferer. Alle Teile müssen eigene Attribute neben der allgemeinen Attribute, die alle Teile gemeinsam nutzen.  Darüber hinaus können Attribute für einen bestimmten Teil des folgenden Jahres ändern Wenn ein neues Modell freigegeben wird.  Als JSON Dokument Shop DocumentDB unterstützt flexible Schemas und Daten mit geschachtelten Eigenschaften ermöglicht und so eignet sich zum Speichern von Produktkatalogdaten.       

## <a name="logging-and-time-series-data"></a>Protokollierung und Zeitreihen-Daten
Anwendungsprotokollierung ist häufig in großen Mengen ausgegeben und veränderliche Attribute möglicherweise basierend auf die bereitgestellte Anwendung oder die Komponente Protokollereignisse.  Daten werden durch Zusammenhänge oder starre Strukturen nicht begrenzt. Zunehmend werden Daten im JSON-Format beibehalten, da JSON leicht und für Menschen lesbar ist.
   
Normalerweise sind zwei Hauptanwendungsfälle Ereignisprotokoll Daten.  Der erste Anwendungsfall ist Ad-hoc-Abfragen über eine Teilmenge der Daten zur Problembehandlung.  Während der Problembehandlung ist eine Teilmenge der Daten zunächst aus den Protokollen in der Regel von Zeitreihen abgerufen.  Dann wird ein Drilldown durch Filtern des Dataset mit Fehlermeldungen oder Fehlerstufen ausgeführt. Dies ist wo speichern Sie Ereignisprotokolle in DocumentDB von Vorteil. In DocumentDB gespeicherten Daten werden standardmäßig automatisch indiziert, und deshalb kann jederzeit abgefragt werden. Darüber hinaus können Daten als eine Zeitreihe über Partitionen beibehalten. Ältere Protokolle können Cold-Speicher pro Ihrer Aufbewahrungsrichtlinie eingeführt.          

Der zweite Anwendungsfall umfasst langer Analytics Arbeitsplätze über eine große Menge von Daten offline ausgeführt.  Diesem gehören Verfügbarkeit Analysis Server Anwendung Fehleranalyse und Clickstream-Analyse.  Normalerweise ist Hadoop verwendet, um diese Arten von Analysen durchführen.  Mit dem Hadoop Connector für DocumentDB als Datenquellen und senken für Schweine, Struktur und Map/Reduce-Funktion DocumentDB Datenbanken. Ausführliche Hadoop Connector für DocumentDB finden Sie unter [Ausführen ein Auftrags Hadoop mit DocumentDB und HDInsight](documentdb-run-hadoop-with-hdinsight.md).      

## <a name="gaming"></a>Spiele
Die Datenbankebene ist eine entscheidende Komponente der Anwendung. Moderne Spiele Grafiken verarbeiten Mobile-Konsole Kunden aber Cloud, angepasste und personalisierte Inhalte im Spiel Statistiken, sozialen Integration und eine Bestenliste Bestenlisten abhängig. Spiele können nur mit extrem niedriger Latenz für Lesevorgänge und Schreibvorgänge zu einer Zusammenarbeit im Spiel Erfahrung und die Datenbankebene muss Höhen und Tiefen bei Anforderung neues Spiel startet und Feature-Updates behandelt.

DocumentDB wird durch massiv Spiele wie [gehen toten: Niemandsland](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) [Weiter spielen](http://www.nextgames.com/)und [Halo 5: Wächter](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Verwenden in beiden Fällen wurden die Vorteile des DocumentDB Folgendes:

- DocumentDB ermöglicht skaliert werden nach oben oder unten elastisch. Dadurch können Spiele aktualisieren Profil und Statistiken von Dutzenden Millionen von gleichzeitigen Spieler behandeln einzelne API aufrufen.
- DocumentDB unterstützt Millisekunde liest und schreibt alle lag während des Spiels zu vermeiden.
- Filtern anhand mehrerer Eigenschaften in Echtzeit ermöglicht DocumentDB die automatische Indizierung, z.B. Suchen Spieler ihre internen Player IDs, ihre GameCenter, Facebook, Google-IDs oder Abfrage basierend auf der Mitgliedschaft in einer Guild Player. Dies ist möglich, ohne komplexe indizieren oder Sharding Infrastruktur.
- Features für soziale einschließlich im Spiel Chatnachrichten, Player Guild Mitgliedschaften Probleme abgeschlossen, eine Bestenliste Bestenlisten und sozialen Diagramme sind einfacher mit einem flexiblen Schema.
- DocumentDB als ein verwalteten Plattform-as-a-Service (PaaS) erforderliche minimale Installation ermöglichen schnelle Iteration arbeiten, und schneller auf den Markt.


## <a name="user-preferences-data"></a>Voreinstellungen für Benutzerdaten
Heute kommen modernste Web- und mobile Applications Erfahrungen mit komplexen Sichten. Diese Ansichten und Erfahrungen sind normalerweise dynamisch catering Benutzervoreinstellungen oder Stimmung und branding benötigt.  Clientanwendungen müssen daher personalisierte Einstellungen effektiv abrufen um Benutzeroberflächenelemente und Erlebnisse schnell darstellen. 

JSON ist ein effektive Benutzeroberfläche Layoutdaten darstellen, wie es nicht nur einfacher ist, sondern auch kann leicht von JavaScript interpretiert werden.  DocumentDB bietet einstellbare Konsistenz, die Lesevorgänge mit geringer Latenz Schreibvorgänge zu ermöglichen. Speicherung UI Layout einschließlich personalisierte als JSON-Dokumente in DocumentDB ist daher ein effektives Mittel diese Daten über das Netzwerk abgerufen.

## <a name="iot-and-device-sensor-data"></a>IoT und Gerät Daten
IoT Anwendungsfälle freigeben häufig einige Muster aufnehmen, verarbeiten und speichern.  Zunächst ermöglichen diese Systeme Daten Aufnahme, die Bursts Gerät Sensoren verschiedene Gebietsschemas aufnehmen kann.  Diese Systeme anschließend verarbeiten und analysieren Daten um Echtzeit Einblicke. Und nicht zuletzt, die meisten wenn nicht alle Daten in einem Datenspeicher für Ad-hoc-Abfragen und offline Analytics schließlich landen.    

Microsoft Azure bietet umfassende Services für IoT genutzt werden Anwendungsfälle.  Azure stehen IoT eine Reihe von Services einschließlich Azure Ereignis Hubs, Azure DocumentDB Azure Stream Analytics, Azure Notification Hub, Azure maschinelles lernen, Azure HDInsight und PowerBI. 

Bursts Daten können von Azure Ereignis Hubs aufgenommen werden, bietet hohen Durchsatz Daten Einnahme mit geringer Latenz. Daten aufgenommen, die für Echtzeit-Einblick verarbeitet werden können, Azure Stream Analytics für Echtzeit-Analysen geleitet. Für Ad-hoc-Abfragen können Daten in DocumentDB geladen werden. Sobald Daten in DocumentDB geladen werden, können die Daten abgefragt werden.  Die Daten in DocumentDB können als Daten im Rahmen von Echtzeit-Analysen verwendet werden. Darüber hinaus können Daten weiter verfeinert und durch die Verbindung DocumentDB mit HDInsight Schwein, Struktur oder Map/Reduce Aufträge verarbeitet.  Raffinierte Daten werden dann für Berichte zu DocumentDB geladen.   

Ein Beispiel IoT Lösung mit DocumentDB, EventHubs und Sturm finden Sie [Hdinsight-Storm-Beispiele GitHub-Repository](https://github.com/hdinsight/hdinsight-storm-examples/).

Weitere Informationen zu Azure IoT-Angebote finden Sie unter [erstellen das Internet der Dinge](http://www.microsoft.com/en-us/server-cloud/internet-of-things.aspx).

## <a name="next-steps"></a>Nächste Schritte
 
Zunächst mit DocumentDB können Sie ein [Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen und führen Sie unsere [Lernpfad](https://azure.microsoft.com/documentation/learning-paths/documentdb/) zu DocumentDB lernen benötigten Informationen. 

Oder möchten Sie mehr über Kunden, DocumentDB, sind die folgenden Kundenberichte verfügbar:

- [Weiter spielen](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). Walking Dead: # 1 von Azure DocumentDB unterstützt Niemandsland Spiel steigt.
- [Halo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Wie Halo 5 sozialen Spiel mit Azure DocumentDB implementiert.
- [Cortana Analytics-Katalog](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana Analytics Galerie - skalierbare Community-Site für Azure DocumentDB erstellt.
- [Zum Kinderspiel](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Führende Integrator Einblick multinationalen Unternehmen globale Minuten Flexible Technologie.
- [News Republik](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Hinzufügen von Intelligenz Nachrichten mit Zweck engagierte Bürger bereitstellen. 
- [SGS International](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Einheitliche Farbe weltweit finden Marken SGS. Und SGS Azure.
- [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Führender Telenor verwendet die Cloud mit einem Start verschieben. 
- [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). Die Zukunft Informationsspeicher auf schnelle und einfache Datenfluss.
