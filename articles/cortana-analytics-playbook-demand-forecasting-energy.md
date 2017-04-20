<properties
    pageTitle="Cortana Intelligence Lösung Vorlage Playbook für Nachfrage prognostizieren Energie | Microsoft Azure"
    description="Eine Projektmappenvorlage mit Microsoft Cortana, die Nachfrage nach einem Energieversorgungsunternehmen Planung."
    services="cortana-analytics"
    documentationCenter=""
    authors="ilanr9"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/24/2016"
    ms.author="ilanr9;yijichen;garye"/>

# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>Cortana Intelligence Lösung Vorlage Playbook für Nachfrage prognostizieren Energie  

## <a name="executive-summary"></a>Zusammenfassung  

In den letzten Jahren haben Internet der Dinge (IoT), Energiequellen und big Data zusammengefasst Möglichkeiten im Dienstprogramm und Energie. Zur gleichen Zeit habe das Dienstprogramm und die gesamte Energiesektor Verbrauch anspruchsvolle besser steuern der Verwendung von Energie Verbraucher reduzieren. Daher sind das Dienstprogramm und intelligente Raster Unternehmen dringend zu erneuern selbst. Außerdem werden viele Stromversorgung und Utility Raster veraltet und sehr teuer zu verwalten. Im letzten Jahr hat das Team eine Reihe von Projekten im Bereich Energieeffizienz gearbeitet. In diesen Fällen sind häufig aufgetreten in denen Dienstprogramme oder ISVs (Independent Software Vendors) Suchen in Planung für zukünftige Energiebedarf. Diese Prognosen spielen eine wichtige Rolle in ihrer aktuellen und zukünftigen Unternehmen und die Grundlage für die verschiedenen Anwendungsfällen geworden. Dazu gehören kurz- und langfristigen Power Load Planung Handel Lastenausgleich, Raster Optimierung usw.. Große sind erweiterte Analytics AA Methoden wie Computer lernen (ML) wichtigen Faktoren für die Erstellung von Prognosen Genauigkeit und Zuverlässigkeit.  

In diesem Playbook wir Geschäfts- und analytische Richtlinien erforderlich für eine erfolgreiche Entwicklung und Bereitstellung des Energiebedarfs Planung Lösung. Die vorgeschlagenen Richtlinien helfen Dienstprogramme, datenwissenschaftler und Ingenieure Daten vollständig standardisierten cloudbasierte, Nachfrage prognostizieren Lösungen einrichten. Für Unternehmen, die nur große Daten und erweiterte Analyse Reise beginnen, kann eine solche Lösung Startwert in ihre langfristige Strategie smart Raster darstellen.

>[AZURE.TIP] Um ein Diagramm zu downloaden, die einen Überblick über diese Vorlage bietet anzeigen Sie [Cortana Intelligence Lösung Architektur für Nachfrage prognostizieren Energie](cortana-analytics-architecture-demand-forecasting-energy.md)  

## <a name="overview"></a>Übersicht  

Dieses Dokument umfasst Unternehmen, Daten und technischen Cortana Intelligence und in bestimmten Azure Machine Learning AML () für die Implementierung und Bereitstellung von Prognose Lösungen. Das Dokument besteht aus drei Teilen:  

1. Verständnis  
2. Daten verstehen  
3. Technische Umsetzung

**Verständnis** Teil stellt Business Aspekt verstehen und vor einer Entscheidung berücksichtigen muss. Es erläutert qualifizieren des Geschäftsproblems, Vorhersageanalytik und maschinelles lernen tatsächlich wirksam und sind anwendbar. Das Dokument weiter erläutert die Grundlagen der computerlernen und Verwendung Energie Prognosen Probleme. Die erforderlichen Komponenten und Qualifikationskriterien eines Anwendungsfalls beschreibt. Einige Beispiele verwenden Fällen und Geschäftskonzept auch Szenarien bereitgestellt werden.

Daten ist die für jeden Computer Lern-Lösung. **Daten verstehen** Teil dieses Dokuments werden einige wichtige Aspekte der Daten behandelt. Beschreibt die Art der Daten erforderlich ist, Energie Prognose datenanforderungen und welche Datenquellen in der Regel vorhanden. Zudem wird erläutert, wie die Rohdaten Datenfeatures vorbereiten, die Modellierung Teil tatsächlich Laufwerk verwendet werden.

Der dritte Teil des Dokuments behandelt die **Technische Implementierung** Aspekt einer Lösung. Featureentwicklung Modellierung Kern wissenschaftliche Daten und sind daher ausführlich erörtert. Das Konzept der Web Services ein wichtiges Instrument zur Bereitstellung von Vorhersageanalysen Solutions Cloud sind behandelt. Beschreiben wir auch eine normale Architektur einer standardisierten End-to-End-Lösung.

Darüber hinaus enthält das Dokument Referenzmaterial, mit denen Sie weitere und Technologie verstehen.

Es ist zu beachten, dass wir nicht in diesem Dokument die tiefere wissenschaftliche Daten abdecken möchten die mathematische und technische Aspekte. Diese in [Azure ML Dokumentation](http://azure.microsoft.com/services/machine-learning/) und [Blogs](http://blogs.microsoft.com/blog/tag/azure-machine-learning/)finden.

### <a name="target-audience"></a>Zielgruppe   
Die Zielgruppe für dieses Dokument ist geschäftliche und technische Mitarbeiter möchten wissen und Verständnis der Computer Learning Solutions und wie diese speziell im Bereich Energie Prognosen verwendet werden.

Datenwissenschaftler profitieren auch Lesen dieses Dokuments zu hohe Prozess besser zu verstehen, die die Bereitstellung einer Lösung Prognosen Energie steuert. In diesem Kontext es kann auch verwendet werden, eine solide herzustellen und Weitere detaillierte und erweiterte Material.

### <a name="industry-trends"></a>Branchentrends  
In den letzten Jahren haben IoT, Energiequellen und big Data zusammengefasst Möglichkeiten im Dienstprogramm und Energie. Zur gleichen Zeit habe das Dienstprogramm und die gesamte Energie Verbrauch anspruchsvolle besser steuern der Verwendung von Energie Verbraucher reduzieren.

Viele Dienstprogramm und intelligente Unternehmen haben Pionierarbeit [smart Raster](https://en.wikipedia.org/wiki/Smart_grid) durch eine Anzahl von Fällen stellen aus dem Raster verwenden mit bereitstellen. Viele Anwendungsbeispiele umkreisen inhärenten Eigenschaften der Stromerzeugung: nicht aufgezeichnet oder als Lager beiseite gespeichert. Also, was muss verwendet werden. Dienstprogramme, die zu effizienter Energieverbrauch einfach Planung müssen denn damit sie zu **Saldo Angebot und Nachfrage**, wodurch energieverschwendung, **Reduzierung von Treibhausgasemissionen**und Kostenkontrolle.

Wenn Kosten, ist ein weiterer wichtiger Aspekt, der Preis ist. Neue Fähigkeiten Handel zwischen Dienstprogramme brachten dringend **Bedarf und künftigen Preis von Elektrizität**zu prognostizieren. Dadurch können Unternehmen ihre Produktions-Volumes zu bestimmen.

Wenn wir das Wort "smart" verwenden, verweisen wir tatsächlich ein Raster, das Lernen und anschließend Vorhersagen. Sie rechnen Jahreszeiten in sowie **vorübergehenden Überlastung Situationen vorhersehen und automatisch für sie**. Geregelt Remote (mit Hilfe dieser smart Meter), können lokalisierte Überladung Situationen behandelt werden. **Zuerst Vorhersage und dann**wird das Raster intelligenter langfristig.

Für den Rest dieses Dokuments wir konzentrieren uns auf eine bestimmte Anwendungsfälle, die Planung der Zukunft decken kurz- und langfristigen Energiebedarf. Wir arbeiten in diesen Monaten und haben wissen und Qualifikation, die Branche Besoldungsgruppe Ergebnisse ermöglichen. Andere Anwendungsfälle werden in naher Zukunft auch im Dokument behandelt.

## <a name="business-understanding"></a>Verständnis

### <a name="business-goals"></a>Unternehmensziele
**Energie Demo** Ziel ist eine normale Vorhersageanalytik und Computer Lern-Lösung in sehr kurzer Zeit bereitgestellt werden kann. Insbesondere ist unsere aktuelle Fokus auf Anforderung Planung Lösungen, damit der Geschäftswert schnell erkannt und auf genutzt. Die Informationen in diesem Playbook helfen den Kunden die folgenden Ziele:
-   Kurze Zeit Wert der computerlernen basierte Lösung
-   Erweitern ein Pilotprojekts Anwendungsfall andere Anwendungsfälle oder Unternehmertums basierend auf ihren geschäftlichen Bedarf
-   Schnell Kenntnisse Cortana Intelligence Suite-Produkt

Mit unter Berücksichtigung dieser Ziele soll diese Playbook die geschäftlichen und technischen Kenntnisse, die bei Erreichen dieser Ziele.

### <a name="power-load-and-demand-forecasting"></a>Stromzufuhr und Nachfrage prognostizieren
Im Energiesektor möglicherweise viele Arten der Nachfrage prognostizieren helfen wichtige Geschäftsprobleme lösen. Tatsächlich kann bei bedarfsplanung die Grundlage für viele Core Anwendungsfälle in der Branche angesehen werden. Wir berücksichtigen im Allgemeinen zwei Arten Energie bedarfsprognosen: kurz- und langfristig. Jeweils einen anderen Zweck dienen und anders nutzen. Der Hauptunterschied zwischen den beiden ist Planungshorizont, d. h. die Zeitspanne in Zukunft für die wir planen möchten.

#### <a name="short-term-load-forecasting"></a>Kurze Begriff laden Prognosen
Im Rahmen des Energiebedarfs ist Short Term laden Planung (STLF) als aggregierte Last definiert, die künftig für verschiedene Teile des Rasters (oder das gesamte Datenblatt) prognostiziert. In diesem Zusammenhang ist kurzfristig definiert Zeithorizont innerhalb des Bereichs von 1 Stunde auf 24 Stunden. In einigen Fällen ist ein Horizont 48 Stunden möglich. Deshalb STLF im Einzelfall Handhabung des Rasters. Einige Beispiele von Anwendungsfällen gesteuerte STLF:
-   Angebot und Nachfrage Netzwerklastenausgleich
-   Handelspartner Stromversorgung
-   Markt machen (Einstellung macht Preis)
-   Raster betriebliche Optimierung
-   [Anforderung-Antwort](https://en.wikipedia.org/wiki/Demand_response)
-   Maximale bedarfsplanung
-   Seite Steuerung
-   Lastenausgleich und Prävention überladen
-   Langfristige Planung laden
-   Fehler und Anomalien-Erkennung
-   Maximale Einschränkung/Abgleich 

STLF Modell basieren hauptsächlich auf Zeit (letzte Tag oder Woche) Daten und Verwendung wie wichtiger Indikator prognostiziert. Planung für die nächste Stunde Temperatur beziehen und von 24 Stunden wird weniger Herausforderung heute. Diese Modelle sind weniger empfindlichen saisonale Muster oder langfristige Trends zum Speicherbedarf.

SLTF Solutions werden umfangreiche Vorhersage Aufrufe (Service-Requests) generieren, da sie stündlich und in einigen Fällen sogar mit aufgerufen werden. Es ist auch sehr häufig, Implantation, jede einzelne Unterwerk oder Transformator als eigenständiges Modell dargestellt und daher Vorhersage Abfragen sind.

#### <a name="long-term-load-forecasting"></a>Langfristige Planung laden
Der Long Term laden Planung (LTLF) soll Leistungsbedarf Zeithorizont von 1 Woche, mehrere Monate (und in einigen Fällen für eine Anzahl von Jahren) planen. Dieser Horizont ist hauptsächlich für Planung und Investitionen Anwendungsfälle.

Langfristige Szenarien unbedingt hochwertige Daten verfügen, die mehrere Jahre (3 Jahre) behandelt. Diese Modelle in der Regel extrahiert saisonbedingten Muster aus der Verlaufsdaten und externen Predicators nutzen als Wetter- und Muster.

Es ist wichtig zu klären, je länger der Planungshorizont weniger genaue Planung möglicherweise. Deshalb können einige Konfidenzintervall mit der tatsächlichen Planung Menschen auf mögliche Variante in ihre Planung erstellt wird.

Da meist Verbrauch Szenario für LTLF Planung erwarten wir viel niedrigeren Vorhersage Volumes (als STLF). Es sieht in der Regel diese Vorhersagen Visualisierungstools PowerBI oder Excel integriert und manuell vom Benutzer aufgerufen werden.

### <a name="short-term-vs-long-term-prediction"></a>Short Term und Long Term Vorhersage
Die folgende Tabelle vergleicht die STLF und LTLF in Bezug auf die wichtigsten Attribute:

|Attribut|Load kurzfristige Planung|Langfristige Planung laden|
|---|---|---|
|Planen von Horizont|Von 1 Stunde bis 48 Stunden|1 bis 6 Monate|
|Daten-Granularität|Stundenlohn|Stündlich oder täglich|
|Normale Anwendungsfälle|<ul><li>Nachfrage/Netzwerklastenausgleich</li><li>Wählen Sie Planung Stunde</li><li>Anforderung-Antwort</li></ul>|<ul><li>Langfristige Planung</li><li>Grid-Ressourcen planen</li><li>Ressourcen planen</li></ul>|
|Normale vorausgesagt|<ul><li>Tag oder Woche</li><li>Stunde des Tages</li><li>Stündliche Temperatur</li></ul>|<ul><li>Monat des Jahres</li><li>Tag des Monats</li><li>Langfristige Temperatur und Klima</li></ul>|
|Verlaufsdaten Bereich|Zwei bis drei Jahre Wert der Daten|5 bis 10 Jahren Wert der Daten|
|Normale Genauigkeit|MAPE * 5 % oder weniger|MAPE * 25 % oder weniger|
|Häufigkeit planen|Erzeugt stündlich oder täglich|Einmal monatlich, vierteljährlich oder jährlich erzeugten|
\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – mittlere durchschnittliche prozentuale Fehler

Wie aus der Tabelle ersichtlich, unbedingt Recht unterscheiden zwischen kurz- und langfristige Prognose Szenarien möglicherweise andere Bereitstellung und Verbrauchsmuster und anderen Unternehmen darstellen.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Anwendungsbeispiel 1: eSmart Systeme – Optimierung der Überlast
Eine wichtige Rolle ein [smart Raster](https://en.wikipedia.org/wiki/Smart_grid) wird dynamisch ständig optimieren und für veränderliche Verbrauchsmuster anpassen. Energieverbrauch kann kurzfristige belastet, die hauptsächlich von Temperaturschwankungen verursacht werden (*z. B.*mehr Leistung für Klimaanlage oder Heizung verwendet). Zur gleichen Zeit wird auch Energieverbrauch durch langfristiger Trends beeinflusst. Dazu gehören saisonbedingten Effekte, Feiertage, langfristig Verbrauch und sogar wirtschaftlichen Faktoren Verbraucherpreisindex, Öl und BIP.

In diesem Fall verwenden wollte [eSmart](http://www.esmartsystems.com/) eine cloudbasierte Lösung bereitstellen, die Vorhersage von vornherein eine Überlastung herbeiführen auf bestimmten Unterwerk des Rasters ermöglicht. Insbesondere wollte eSmart Unterwerke identifizieren, die innerhalb der nächsten Stunde überladen, damit eine sofortige Maßnahmen ergriffen werden könnten, zu vermeiden oder beheben diese Situation.

Eine präzise und schnell ausgeführt Prediction erfordert die Implementierung drei Vorhersagemodelle:
-   Lange Begriff ermöglicht die Planung des Energieverbrauchs auf jeder Station während der nächsten Wochen oder Monaten
-   Kurzfristige Modell, die Vorhersage der Überlastung herbeiführen auf bestimmten Unterwerk während der nächsten Stunde ermöglicht
-   Temperatur-Modell, das stellt Prognosen der zukünftigen Temperatur über mehrere Szenarien

Die langfristige Modell soll die Neigung während der nächsten Woche oder des Monats (wenn die Leistung für die Übertragung) überladen die Unterwerken Rang. Dies ermöglicht das Erstellen einer kurzen Liste Unterwerke als Eingabe für die kurzfristige Prognose dienen. Wie Temperatur wichtiger Indikator für langfristige Modell ist müssen ständig Multi-Szenario Temperatur Prognosen zu erstellen und Sie als Eingabe für die langfristige Modell. Die kurzfristige Prognose wird um vorherzusagen, welche Unterwerk voraussichtlich in der nächsten Stunde Überladung aufgerufen.

Kurzfristige und langfristige Modelle werden individuell pro jedes Unterwerk bereitgestellt. Daher erfordert der praktischen Umsetzung dieser Modelle umfassende Orchestrierung. Um kurzfristig vorhersagegenauigkeit zu erhalten, ist eine detailliertere Modell für jede Stunde des Tages gewidmet. Diese Modelle werden stündlich ausgeführt und Ausführung innerhalb weniger Minuten ausreichend Zeit und ggf. vorbeugende Aktionen abgeschlossen. Diese Sammlung von Modellen ist regelmäßige Umschulung mit den aktuellsten Daten aktuell gehalten.

Weitere Informationen zu diesem finden Sie [hier](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Verwenden Sie Groß-Qualifikationskriterien – Komponenten
Die Stärke der Cortana Intelligence ist darin leistungsstarke bereitstellen und Computer lernen orientierte Lösungen. Es soll auf Tausende von Vorhersagen, die gleichzeitig ausgeführt werden. Sie können automatisch ändern Verbrauchstyp zu skalieren. Eine Lösung Schwerpunkt liegt daher auf Genauigkeit und rechenleistung. Beispielsweise ist ein interessiert genaue Energiebedarf für die nächste Stunde und für jede Stunde des Tages planen. Andererseits, wir interessieren weniger Beantwortung der Frage, warum die Anforderung vorhergesagt werden soll (das Modell kümmern, die).

Deshalb nicht alle Anwendungsfälle und Unternehmen effektiv lösen mit computerlernen.

Cortana Intelligenz und maschinelles lernen kann äußerst effektiv Lösung Unternehmen die folgenden Kriterien erfüllt sind:
-   Das geschäftliche Problem Hand **vorhersehbaren** Natur. Fallbeispiel vorhersehbaren Verwendung ist ein Dienstprogramm, das macht einen bestimmten Unterwerk Belastung während der nächsten Stunde vorhersagen möchten. Auf der anderen Seite würde analysieren und Rangfolge der Treiber der bisherige Bedarfsmengen **beschreibenden** Natur und daher weniger geeignet sein.
-   Es ist **Pfad der Aktion** werden nach die Vorhersage verfügbar ist. Beispielsweise kann Vorhersage einer Überlastung einer Unterwerk während der nächsten Stunde Triggeraktion eine proaktive laden, Unterwerk zugeordnet ist, und möglicherweise eine Überladung verhindert.
-   Der Anwendungsfall stellt eine **normale Art von Problem** so gelöst, dass bei den Weg ebnen Lösung ähnlichen Anwendungsfälle.
-   Der Kunden kann **quantitative und qualitative Ziele** zeigen eine erfolgreiche Implementierung festlegen. Zum Beispiel wäre ein gutes Quantitatives Ziel Energie bedarfsplanung Genauigkeit Schwellenwert (*z. B.*bis zu 5 % Fehler ist zulässig) oder Vorhersage Unterwerk dann die Genauigkeit (true positive Rate) überladen und Rückruf (soweit true positive) sollte über einen bestimmten Schwellenwert. Diese Ziele sollten die geschäftlichen Ziele des Kunden ableiten.
-   Es gibt klare **Integrationsszenarios** des Unternehmens Business Workflow. Beispielsweise kann Unterwerk laden Planung in Grid Control Center Überladung Aktivitäten können integriert werden.
-   Der Kunde hat einsatzbereit **mit ausreichender Qualität** zu verwenden (siehe nächsten Abschnitt **Qualität**dieser Playbook mehr) unterstützen.
-   Der Kunde umfasst Architektur der Cloud-orientierte Daten oder **Cloud - Computer lernen**, einschließlich Azure ML und andere Cortana Intelligence Suite.
-   Der Kunde ist **eine durchgehende Datenfluss** Einrichtungen herstellen möchte die Übermittlung von Daten in der Cloud laufend, und zum **durchsetzen** die Lösung bereit.
-   Der Kunde kann **Mittel** , die während der ersten Pilottest aktiv sein wird, wissen und die Lösung an den Kunden bei übertragen werden können.
-   Kunden-Ressource sollte ein **erfahrenen professionellen Daten**, vorzugsweise datenwissenschaftler.

Qualifizierung eines Anwendungsfalls basierend auf Kriterien kann erheblich verbessern die Erfolgsraten eines Anwendungsfalls und guten brückenkopf zur Durchführung von zukünftigen Anwendungsfällen einrichten.

### <a name="cloud-based-solutions"></a>Cloud-Lösungen
Cortana Intelligence Suite auf Azure ist eine integrierte Umgebung in der Cloud befindet. Die Bereitstellung einer Lösung erweiterte Analyse in einer Cloud-Umgebung enthält erhebliche Vorteile für Unternehmen und gleichzeitig große Änderung für Unternehmen bedeuten können, die noch lokale IT Solutions. Im Energiebereich ist ein Trend der schrittweisen Migration von Vorgängen in der Cloud. Dies geht einher mit der Entwicklung von smart Grid wie oben beschrieben in **Branchentrends**. Wie diese Playbook eine cloudbasierte Lösung im Bereich Energieeffizienz, unbedingt Erläutern Sie die Vorteile und weitere Aspekte der Bereitstellung einer Cloud-basierte Lösung.

Vielleicht ist der größte Vorteil von Cloud-basierten Lösung die Kosten. Als Lösung nutzt Cloud bereitgestellte Komponenten kein Investitionskosten oder Kosten COGS (Wareneinsatz) Komponente zugeordnet. Das heißt, keine Notwendigkeit besteht zu Hardware, Software und IT-Verwaltung und deshalb eine erhebliche geschäftliche Risiken.

Ein weiterer wichtiger Vorteil ist die Kostenstruktur Bedarfsbasis von Cloud-basierten Lösungen. Cloud-basierten Servern Computing-Segment oder Speicher können bereitgestellt und jeweils nur nach Bedarf skaliert werden. Dies ist die Effizienz profitieren von Cloud-basierten Lösung.

Schließlich besteht keine Notwendigkeit für Investitionen in IT-Verwaltung oder Entwicklung der zukünftigen alle Teil der Cloud-basiertes Angebot handelt. Insofern Cortana Intelligence Suite enthält die besten in Klasse und der Wegweiser weiterentwickelt hält. Neue Features, Komponenten und Funktionen werden ständig eingeführt und weiterzuentwickeln.

Für ein Unternehmen, das erst den Übergang in die Cloud, empfehlen dringend wir zu schrittweise durch Cloud Migration Straßenkarte implementieren. Wir glauben, dass Dienstprogramme und Unternehmen im Bereich Energieeffizienz in diesem Playbook beschriebenen Anwendungsfälle Gelegenheit für Pilottests Vorhersageanalysen Solutions in der Cloud darstellen.

#### <a name="business-case-justification-considerations"></a>Geschäftliche Rechtfertigung Aspekte
In vielen Fällen kann der Kunde sein eine geschäftliche Begründung für einen bestimmten Anwendungsfall mit einer Cloud-Lösung und maschinelles lernen wichtige Komponenten sind interessiert. Im Gegensatz zu einer Lösung vor Ort bei cloudbasierte Lösung Kostenkomponente ist minimal, und die meisten der Bestandteile sind mit dem tatsächlichen Verbrauch. Beim Bereitstellen einer Energie von Prognosen auf Cortana Intelligence Suite, können mehrere Dienste mit einer einzigen gemeinsamen Kostenstruktur integriert. Beispielsweise Datenbanken (*z. B.*SQL Azure) wird die unformatierten Daten speichern und dann für die eigentliche Trends Azure ML werden die Prognose Dienste hosten. In diesem Beispiel konnte die Kostenstruktur Speicher- und Transaktionskomponenten enthalten.

Auf der anderen Seite müssen eine gute Kenntnisse der geschäftlichen Wert der Betrieb eines Energiebedarfs Planung (kurz- oder langfristig). Tatsächlich ist es wichtig zu jedem geplanten Vorgang den geschäftlichen Nutzen. Beispielsweise genau Planung Stromzufuhr für 24 Stunden kann Überproduktion oder Overloads im Raster verhindern helfen und dies quantifizierbar in Kostenersparnis täglich.

Eine grundlegende Formel für die Berechnung des finanziellen Vorteil bei Bedarf Planung wäre: ![einfache Formel zur Berechnung des finanziellen Vorteil bei Bedarf Planung Lösung](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Da Cortana Intelligence Suite Bedarfsbasis Preismodell bereitstellt, besteht keine Notwendigkeit für eine feste Kostenkomponente dieser Formel anfallen. Diese Formel kann, monatliche oder jährliche täglich berechnet.

Aktuelle Cortana Intelligence Suite und Azure ML Preisübersicht finden Sie [hier](http://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Lösung-Entwicklungsprozess
Entwicklungszyklus Energie Nachfrage prognostizieren Lösung beinhaltet in der Regel 4 Phasen, die wir machen Verwenden von Cloud-Technologie und Services Cortana Intelligence Suite.

Dies wird im folgenden Diagramm veranschaulicht:

![Smart-Grid-Zyklus](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

Der folgende Absatz beschreibt diese 4 Schritten:

1.  **Daten** – eine erweiterte Analyse basierte Lösung abhängig (siehe **Daten verstehen**). Insbesondere setzen Vorhersageanalytik und Prognose betrifft, wir auf kontinuierlichen, dynamische Daten. Bei Energy Nachfrage prognostizieren, diese Daten direkt von smart Metern bezogen werden oder bereits in einer Datenbank auf Prem aggregiert werden. Wir setzen auf anderen externen Datenquellen wie Wetter und Temperatur. Diese fortlaufenden Datenfluss muss koordiniert, geplant und gespeichert. [Azure Data Factory](http://azure.microsoft.com/services/data-factory/) (ADF) ist unsere wichtigsten Arbeitstier für diese Aufgabe.
2.  **Modellieren** – für genaue und zuverlässige Prognosen, eine (Zug) entwickeln und verwalten ein großes Modell, macht der Verlaufsdaten und aussagekräftige und vorausschauende Muster in den Daten extrahiert. Der Bereich der Computer lernen (ML) wächst schnell mit erweiterten Algorithmen werden routinemäßig. Azure ML Studio bietet hervorragende, mit der fortschrittlichsten ML Algorithmen innerhalb einer vollständigen Workflow verwenden. Der Workflow in einer intuitiven Flussdiagramm dargestellt und umfasst alles, Feature Extraktion, Modellierung und auswerten. Der Benutzer kann in Hunderten von verschiedenen Modellen ziehen, die in dieser Umgebung enthalten sind. Am Ende dieser Phase haben datenwissenschaftler ein, die vollständig ausgewertet und einsatzbereit ist.

    Im folgenden Diagramm ist eine Darstellung der Workflow normalerweise:

    ![Modellierung von Arbeitsabläufen](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)

3.  **Bereitstellung** – besteht der nächste Schritt Bereitstellung mit einem Arbeitsmodell in der Hand. Hier wird das Modell in einem Webdienst konvertiert, die eine RESTful API verfügbar macht, die über das Internet mit verschiedenen Verbrauch Clients gleichzeitig aufgerufen werden können. Azure ML bietet eine einfache Möglichkeit zum Deployment eines Modells direkt von Azure ML Studio mit einem einzigen Klick einer Schaltfläche. Der gesamten Bereitstellungsprozess hinter den Kulissen geschieht. Diese Lösung kann automatisch erforderliche Verbrauch zu skalieren.

4.  **Verbrauch** – In dieser Phase wir eigentlich machen das Vorhersagemodell Vorhersagen zu verwenden. Der Verbrauch kann von einer Anwendung (*z.*B. Dashboard) oder direkt aus einem laufenden System z. B./Nachfrage Netzwerklastenausgleich System oder eine Lösung zur Optimierung Raster. Mehrere Anwendungsfälle können aus einem einzelnen Modell gesteuert werden.

## <a name="data-understanding"></a>Daten verstehen
Nach der Behandlung (siehe **Verständnis**) Business Aspekte Energie Nachfrage prognostizieren Lösung, können wir nun das Webpart zu. Jeder Vorhersageanalytik Lösung beruht auf Daten. Energie Nachfrage prognostizieren, setzen wir auf Verlaufsdaten Daten mit unterschiedlichen Granularitäten. Verlaufsdaten als Rohmaterial verwendet werden. Es wird eine sorgfältige Analyse werden in der Daten bei vorausgesagt (auch als Eigenschaften bezeichnet) identifiziert, die sich in einem Modell die erforderlichen Prognosen schließlich generiert.

Im Rest dieses Abschnitts beschreiben wir die verschiedenen Schritte und Aspekte für das Verständnis der Daten und zum Format bringen.

### <a name="the-model-development-cycle"></a>Modell-Entwicklungszyklus
Gute Modelle erfordert einige sorgfältiger Planung und Planung erzeugen. Aufschlüsselung des Modellierungsprozesses in mehrere Schritte und auf einen Schritt gleichzeitig konnte der gesamte Prozess deutlich verbessern.

Das folgende Diagramm zeigt, wie Modellierungsprozesses in mehrere Schritte unterteilt werden kann:

![Modell-Entwicklungszyklus](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Wie kann der Zyklus besteht aus sechs Schritten:
-   Problemformulierung
-   Erfassung von Daten und Durchsuchen von Daten
-   Vorbereitung und Featureentwicklung
-   Modellierung
-   Auswerten
-   Entwicklung

Im Rest dieses Abschnitts werden die einzelnen Schritte und Aspekte bei jedem Schritt beschrieben.

### <a name="problem-formulation"></a>Problemformulierung
Wir können problemformulierung der wichtigste Schritt muss vor der Implementierung von Vorhersageanalysen zu betrachten. Hier würden wir das Geschäftsproblem transformieren und Zerlegen bestimmte Elemente von Daten und Modellen behoben werden können. In diesem Zusammenhang empfiehlt es sich, das Problem als Satz von Fragen formulieren möchten wir beantworten. Hier sind einige möglichen Fragen im Rahmen der Energie Nachfrage prognostizieren erforderlich sein können:
-   Was ist die erwartete Last auf eine individuelle Station in der nächsten Stunde oder Tag?
-   Zu welcher Tageszeit erleben mein Raster Spitzenzeiten?
-   Wie wahrscheinlich ist mein Raster zu Spitzenzeiten erwartet?
-   Wie viel Energie sollte das Kraftwerk während jeder Stunde des Tages generieren?

Diese Fragen formulieren erlaubt uns, die richtigen Daten und Implementierung einer Lösung, die vollständig mit den geschäftlichen Problems ausgerichtet ist. Darüber hinaus können wir einige Schlüsselkriterien festlegen, die Bewertung die Leistung des Modells zu ermöglichen. Angenommen, wie genau sollte die Planung und was liegt der Fehler immer noch vom Unternehmen akzeptabel wäre?

### <a name="data-sources"></a>Datenquellen
Moderne smart Grid sammelt Daten aus verschiedenen Teilen und Komponenten des Rasters. Diese Daten stellt verschiedene Aspekte des Vorgangs und die Nutzung des Netzes. Im Rahmen der Planung Energiebedarf beschränken wir die Diskussion auf Datenquellen, die den tatsächlichen Bedarf Verbrauch. Eine wichtige Quelle des Energieverbrauchs sind smart Meter. Dienstprogramme auf der ganzen Welt bereitstellen schnell smart Meter für ihre Kunden. Den tatsächliche Energieverbrauch aufzeichnen und ständig übertragen dieser Daten an die Versorgungsunternehmen Smart. Daten werden gesammelt und in festen Intervallen von 5 Minuten bis 1 Stunde gesendet. Erweiterte smart Meter können remote steuern und die tatsächliche innerhalb eines Haushalts programmiert werden. Intelligente Zählerdaten relativ zuverlässig und schließt einen Zeitstempel. Dadurch wichtige Zutat Nachfrage prognostizieren. Zählerdaten können zusammengefasst (zusammengezählten) auf verschiedenen Ebenen in der Topologie Raster: *Transformator, Unterwerk, Region*. Wir können die erforderlichen Aggregationsebene ein Prognosemodell dafür erstellen wählen. Beispielsweise möchten das Versorgungsunternehmen Planung der zukünftigen Auslastung aller seiner Raster Unterwerke können alle Messer Daten für jede einzelne Unterwerk zusammengefasst und werden als Eingabe für das Vorhersagemodell verwendet. Wir bezeichnen smart Meter als interne Datenquelle.

Prognose bei Bedarf zuverlässigen verlassen auch auf anderen externen Datenquellen. Ein wichtiger Faktor Stromverbrauch ist das Wetter oder genauer die Temperatur. Verlaufsdaten zeigt starke Wechselbeziehung zwischen Temperatur und Energieverbrauch. Tagen heißen Verbraucher stellen die Klimaanlagen und Winter schalten Heizung verwenden. Zuverlässige historisch Temperaturen an Raster ist daher Schlüssel. Außerdem setzen wir auch auf genaue Planung der Temperatur als Indikator für den Energieverbrauch.

Andere externe Datenquellen können auch helfen, Energie Bedarf Planzahlenmodelle erstellen. Dazu gehören langfristig Klimawandel und wirtschaftliche Indizes (*z.B.*BIP). In diesem Dokument werden wir nicht dazu andere Datenquellen gehören.

### <a name="data-structure"></a>Datenstruktur
Nach dem Ermitteln der erforderlichen Daten, möchten wir sicherstellen, dass Rohdaten gesammelt wurden die richtigen Datenfunktionen enthält. Wir müssen sicherstellen, dass die Daten Daten, die helfen enthält, den zukünftigen Bedarf Vorhersagen, zum Erstellen eines Planzahlenmodells zuverlässig bei Bedarf. Hier sind einige über die Datenstruktur (Schema) der Rohdaten.

Die Rohdaten besteht aus Zeilen und Spalten. Jede Messung wird als eine einzelne Datenzeile dargestellt. Jede Datenzeile enthält mehrere Spalten (auch als Eigenschaften oder als Felder bezeichnet).

1.  **Zeitstempel** -Timestamp-Feld stellt die tatsächliche Zeit, wenn die Messung aufgezeichnet wurde. Es sollte eine allgemeine Datums-/Zeitformate entsprechen. Datum und Uhrzeit sollte enthalten. In den meisten Fällen besteht keine Notwendigkeit für die Zeit, bis der zweiten Ebene der Granularität aufgezeichnet werden. Es ist wichtig, die Zeitzone angeben, in dem die Daten erfasst.
2.  **Anzeige-ID** – dieses Feld gibt die Anzeige oder das Messgerät. Es kann ist eine kategoriale Variable und eine Kombination aus Ziffern und Zeichen.
3.  **Verbrauch** – Dies ist die tatsächliche zu einem bestimmten Zeitpunkt. Der Verbrauch gemessen in kWh (Kilowattstunde) oder andere bevorzugte Einheiten. Es ist wichtig zu beachten, dass die Maßeinheit über alle Maße in den Daten konsistent bleiben. In einigen Fällen kann Verbrauch 3 Phasen bereitgestellt. In diesem Fall müssten wir alle Phasen der unabhängigen Verbrauch erfassen.
4.  **Temperatur** -die Temperatur ist normalerweise eine unabhängige Quelle entnommen. Allerdings sollte mit den Daten kompatibel sein. Es sollen einen Zeitstempel wie oben beschrieben, der sie mit den tatsächlichen Daten synchronisiert werden können. Die Temperatur in Grad Celsius oder Fahrenheit angegeben werden aber sollte über alle Maße konsistent bleiben.
5.  **Ort –** Das Feld Speicherort wird typischerweise mit, wo die Daten gesammelt wurden. Es kann als Zahl Postleitzahl oder Breiten-und Längengrad (Lat/Long) Format dargestellt werden.

Die folgenden Tabellen zeigt Beispiele für eine gute Verbrauch und Temperatur Datenformat:

|**Datum**|**Zeit**|**Anzeige-ID**|**Phase 1**|**Phase 2**|**Phase 3**|
|--------|--------|------------|-----------|-----------|-----------|
|7/1/2015|10:00:00|ABC1234     |7.0        |2.1        |5.3        |
|7/1/2015|10:00:01|ABC1234     |7.1        |2.2        |4.3        |
|7/1/2015|10:00:02|ABC1234     |6.0        |2.1        |4.0        |

|**Datum**|**Zeit**|**Speicherort**|**Temperatur**|
|--------|--------|-------------|---------------|
|7/1/2015|10:00:00|11242        |24,4           |
|7/1/2015|10:00:01|11242        |24,4           |
|7/1/2015|10:00:02|11242        |24,5           |

Wie oben erwähnt, umfasst dieses Beispiel 3 verschiedene Werte für Verbrauch 3 Phasen zugeordnet. Beachten Sie, dass die Felder Datum und Zeit getrennt, jedoch sie auch in einer einzigen Spalte zusammengefasst werden können. In diesem Fall wird die Spalte 5-stellige Postleitzahl und die Temperatur in Grad Celsius Format dargestellt.

### <a name="data-format"></a>Datenformat
Cortana Intelligence Suite unterstützt die gängigsten Formate *CSV, TSV, JSON*. Es ist wichtig, dass das Datenformat für den gesamten Lebenszyklus des Projekts konsistent bleibt.

### <a name="data-ingestion"></a>Erfassung von Daten
Da Energie bedarfsplanung ständig und häufig vorhergesagt, müssen wir die unformatierten Daten durch eine solide und zuverlässige Daten-Aufnahmevorgangs fließt. Erfassung-Prozess muss garantieren, dass die Rohdaten für die Prognoseprozess zum gewünschten Zeitpunkt. Das bedeutet, dass Daten Einnahme Frequenz größer als die Prognose Frequenz sein soll.

Beispiel: Wenn unsere Lösung Planung Bedarf eine neue Prognose um 8:00 Uhr täglich erzeugt müssen wir sicherstellen, dass die während der letzten 24 Stunden gesammelten Daten bis dahin vollständig aufgenommen worden und wurde auch die letzte Stunde Daten enthalten.

Cortana Intelligence Suite bietet hierfür verschiedene Arten eine zuverlässige Aufnahme unterstützen. Dies wird ausführlicher im Abschnitt **Bereitstellung** dieses Dokuments behandelt.

### <a name="data-quality"></a>Qualität der Daten
Rohdaten-Quelle die verlässlichen und präzisen Nachfrage prognostizieren durchführen muss einige grundlegende Daten Qualitätskriterien. Obwohl erweiterte statistische Methoden Ausgleich für einige Qualitätsproblems Daten verwendet werden können, müssen wir sicherstellen, dass wir einige Daten Qualität Schwellenwert überschreiten, wenn neue Daten Einnahme. Hier sind einige Aspekte der Rohdaten Qualität:
-   **Fehlender Wert** – Dies bezieht sich auf die Situation bei bestimmten Wert nicht erfasst wurde. Die Grundvoraussetzung ist, dass die fehlenden Wert Rate nicht mehr als 10 % für einen bestimmten Zeitraum. Im Fall, dass ein einzelner Wert es fehlt anzugeben mit einem vordefinierten Wert (z.B.: 9999') und '0' die ein gültiges Maß.
-   **Genauigkeit** – der tatsächliche Wert der Verbrauch oder Temperatur sollte genau aufgezeichnet. Ungenaue Messungen erzeugt falsche Prognosen. Normalerweise sollte der Meßfehler niedriger als 1 % der Wert true.
-   **Zeitpunkt der Messung** ist erforderlich, dass der aktuelle Zeitstempel der Daten gesammelt wird nicht mehr als 10 Sekunden gegenüber den tatsächlichen Zeitpunkt der aktuellen Messung abweichen.
-   **Synchronisierung** – Wenn mehrere Datenquellen verwendet werden (*z. B.*Verbrauch und Temperatur) Wir müssen sicherstellen, dass es keine Zeit zwischen ihnen Synchronisierungsprobleme sind. Dies bedeutet, dass die Zeit zwischen der gesammelten Zeitstempel aus zwei unabhängigen Datenquellen nicht länger als 10 Sekunden überschreitet.
-   **Wartezeit** - wie oben beschrieben in **Daten Einnahme**hängen wir eine zuverlässige Fluss und Aufnahme von Daten. Steuerelement, wir müssen sicherstellen, dass wir die Wartezeiten steuern. Dies wird angegeben, wie die Zeit zwischen dem der tatsächliche Messung und gleichzeitig mit der Einlagerung Cortana Intelligence Suite geladen wurde und einsatzbereit ist. Für kurzfristige laden die Gesamtwartezeit Planung nicht mehr als 30 Minuten sollte. Für langfristige laden die Gesamtwartezeit Planung nicht größer als 1 Tag sollte.

### <a name="data-preparation-and-feature-engineering"></a>Vorbereiten der Daten und Featureentwicklung
Rohdaten wurde aufgenommenen (siehe **Daten Ingestion**) und sicher gespeichert wurde, kann er verarbeitet werden. Vorbereitung der Daten im Grunde die unformatierten Daten und konvertieren (Umwandlung umformen) in einem Formular für die Entwurfsphase. Das kann einfache Vorgänge wie der Rohdaten-Spalte wie der aktuelle Messwert, standardisierte Werte komplexere Vorgänge wie [Zeit zurück](https://en.wikipedia.org/wiki/Lag_operator)und andere enthalten. Die neu erstellten Spalten werden als Funktionen bezeichnet und diese generieren als Feature Engineering bezeichnet. Ende dieses Vorgangs müssten wir ein neues Dataset, das den unformatierten Daten abgeleitet und zur Modellierung verwendet werden. Darüber hinaus muss der Vorbereitung der Daten über fehlende Werte (siehe **Datenqualität**) und kompensieren. In einigen Fällen auch müssten wir normalisieren die Daten, um sicherzustellen, dass alle Werte in der gleichen Skala dargestellt werden.

In diesem Abschnitt wir einige allgemeine Datenfeatures auflisten, Energie Planzahlenmodelle Bedarf

**Getrieben Funktionen:** Diese Funktionen werden von der Datums-/Zeitstempel Daten abgeleitet. Diese werden extrahiert und kategorische Features wie umgewandelt:
-   Zeit Tag ist die Tageszeit der Werte von 0 bis 23
-   Tag Woche – Dies stellt den Tag der Woche und Werte von 1 (Sonntag) bis 7 (Samstag)
-   Tag des Monats – Dies stellt das Datum dar und können Werte von 1 bis 31
-   Monat des Jahres – Dies stellt den Monat und Werte von 1 (Januar) bis 12 (Dezember)
-   Wochenende – Dies ist ein Binärwert, der Wert 0 für Wochentage oder 1 Wochenende akzeptiert
-   Urlaub - Dies ist ein Binärwert, die die Werte 0 für normalen oder 1 für einen Feiertag akzeptiert
-   Fourier Begriffe – gelten Fourier Gewichte, die den Zeitstempel abgeleitet und saisonbedingten (Zyklen) erfassen der Daten. Da wir mehrere Jahreszeiten Daten möglicherweise benötigen wir mehrere Fourier-Begriffe. Beispielsweise müssen bei bedarfswerte jährliche, wöchentliche oder tägliche Jahreszeiten/Zyklen 3 Fourier Begriffe führt.

**Unabhängige Messfunktionen:** Die unabhängigen Funktionen umfassen alle Datenelemente, die wir als vorausgesagt Modell verwenden möchten. Hier schließen wir das abhängige Feature die wir Vorhersagen.
-   Lag-Funktion – diese Zeit verschoben werden Werte des tatsächlichen Bedarfs. Beispielsweise enthält Lag 1 Funktionen den Wert bei Bedarf in der letzten Stunde (sofern stündlich) gegenüber den aktuellen Zeitstempel. Ebenso können wir Lag 2 hinzufügen, lag 3 *usw.*. Tatsächliche Kombination Lag Funtkionen werden während der Entwurfsphase durch Auswertung Modell bestimmt.
-   Langfristige Trends – diese Funktion die lineare Zunahme zwischen Jahren darstellt.

**Abhängige Funktion:** Das abhängige Feature ist die Datenspalte wir unser Modell vorhersagen möchten. [Überwachte Computer lernen](https://en.wikipedia.org/wiki/Supervised_learning)müssen wir zuerst die abhängigen Elemente mit Schulen (die auch als Beschriftung bezeichnet). Dadurch kann das Modell zu den Mustern in der abhängigen Funktion zugeordnet. Energiebedarf planen wir normalerweise den aktuellen Bedarf vorhersagen möchten und daher verwenden sie als abhängige Feature.

**Behandlung fehlender Werte:** Während der Vorbereitung der Daten müssten wir die beste Strategie für fehlende Werte ermitteln. Dies geschieht hauptsächlich mit verschiedenen statistischen [Methoden Zuschreibung](https://en.wikipedia.org/wiki/Imputation_(statistics)). Bei Energy Nachfrage prognostizieren unterstellen wir normalerweise fehlende Werte mit Gleitender Durchschnitt aus vorherigen verfügbaren Daten.

**Datennormalisierung:** Datennormalisierung ist eine andere Art der Transformation der bringt alle numerische Daten wie Planung in einer ähnlichen Skala verwendet wird. Dies verbessert in der Regel modellgenauigkeit und Präzision. Wir normalerweise dazu durch Division des tatsächlichen Werts durch den Datenbereich.
Dadurch wird den ursprünglichen Wert in einen kleineren Bereich in der Regel zwischen-1 und 1 verkleinern.

## <a name="modeling"></a>Modellierung
Der Entwurfsphase erfolgt die Konvertierung der Daten in einem Modell. In diesem Prozess werden Algorithmen erweiterte scan historical Data (Daten), die Muster zu extrahieren und Modell. Dieses Modell kann später verwendet werden, neue Daten vorherzusagen, die nicht für die Erstellung des Modells verwendet wurde.

Haben wir eine zuverlässige Arbeitsmodell wir es verwenden können, um neue Daten zu erhalten, die den erforderlichen Funktionen (X) strukturiert ist. Des Bewertungsprozesses machen beibehaltenes Modell (Objekt von der Ausbildung) verwenden und Vorhersagen Zielvariable, die durch Ŷ gekennzeichnet ist.

### <a name="demand-forecasting-modeling-techniques"></a>Nachfrage prognostizieren Modellen
Bei Bedarf stellen wir Prognosen Verlaufsdaten Zeit bestellt verwenden. Wir bezeichnen Daten die Zeitdimension als [Zeitreihe](https://en.wikipedia.org/wiki/Time_series). Das Ziel bei der Time Series-Modellierung ist zu Zeit verwandte Trends saisonbedingten, Automatische Korrelation (Korrelation mit der Zeit) und in einem Modell zu formulieren.

In den letzten Jahren wurden Algorithmen Zeitreihenvorhersage und Prognosen präziser entwickelt. Erläutert kurz einige davon hier.

> [AZURE.NOTE] Dieser Abschnitt kann nicht verwendet werden, als Computer lernen und Prognosen (Übersicht), sondern als eine Umfrage von Modellen, die häufig für Anforderung-Planung verwendet werden. Weitere Informationen und Lehrmaterial über Zeitreihenvorhersage empfehlen wir das online-Buch [Planung: Grundsätze und Verfahren](https://www.otexts.org/book/fpp).

#### <a name="ma-moving-averagehttpswwwotextsorgfpp62"></a>[**MA (Durchschnitt)**](https://www.otexts.org/fpp/6/2)
Gleitender Durchschnitt ist eines der ersten Analysemethoden, die für Zeitreihenvorhersage verwendet und ist weiterhin eines der Verfahren heute häufig verwendet. Es ist auch die Grundlage für erweiterte Verfahren zur Vorhersage. -Durchschnitt prognostizieren wir den nächsten Datenpunkt von durchschnittlich über letzten K Punkte, K die Reihenfolge der Berechnung des gleitenden Durchschnitts bezeichnet.

Gleitende Durchschnitt Verfahren wirkt der Glättung der Planung und auch große Volatilität Daten möglicherweise nicht verarbeitet.

#### <a name="ets-exponential-smoothinghttpswwwotextsorgfpp75"></a>[**ETS (exponentielle Glättung)**](https://www.otexts.org/fpp/7/5)
Exponentielles Glätten (ETS) ist eine Reihe von verschiedenen Methoden die gewichteten Durchschnitt der aktuellsten Datenpunkte verwenden, um den nächsten Datenpunkt vorherzusagen. Soll aktueller Werte höhere Gewichte zuweisen und allmählich verringern dieses Gewicht für ältere Messwerte. Gibt es eine Reihe von verschiedenen Methoden Familie, einige Behandlung saisonbedingten Daten wie [Holt Winter saisonale Methode](https://www.otexts.org/fpp/7/5)enthalten.

Einige dieser Methoden berücksichtigen auch saisonbedingten Daten.

#### <a name="arima-auto-regression-integrated-moving-averagehttpswwwotextsorgfpp8"></a>[**ARIMA (Auto Regression integrierter gleitender Durchschnitt)**](https://www.otexts.org/fpp/8)
Automatische Regression integrierte verschieben durchschnittliche (ARIMA) ist eine weitere Familie von Methoden der häufig für Prognosen Zeitreihe verwendet wird. Praktisch wird automatisch Regression Methoden-Durchschnitt kombiniert. Automatische Regression Methoden verwenden regressionsmodelle mit vorherigen Zeitwerte Reihe um den nächsten Punkt Datum berechnen. ARIMA Methoden gelten auch differenzierende Methoden, die die Differenz zwischen den Datenpunkten und verwenden diese anstelle der ursprüngliche gemessene Wert. Schließlich ARIMA verwendet gleitenden Durchschnitt Techniken der oben beschriebenen. Die Kombination aller dieser Methoden auf unterschiedliche Weise ist was Familie ARIMA Methoden erstellt.

ETS und ARIMA sind heute Energie Nachfrage prognostizieren und viele andere Prognosen Probleme verbreitet. In vielen Fällen sind diese kombiniert zu sehr präzise Ergebnisse liefern.

**Allgemeine mehrere Regression** Regressionsmodelle möglicherweise wichtigste beruhender Ansatz im Bereich der computerlernen und Statistiken. Im Kontext der Zeitreihe mithilfe Regression (*z. B.*Nachfrage) die zukünftige Werte vorherzusagen. Regression wir Linear aus der vorausgesagt und das Gewicht (auch als Koeffizienten bezeichnet) von diesen vorausgesagt während des Trainings. Soll eine Regressionsgeraden erzeugt, die unserer Vorhersagewert prognostiziert. Regressionsmethoden sind geeignet, wenn die Zielvariable numerisch und daher auch Zeitreihenvorhersage passt. Es gibt eine Vielzahl von regressionsmethoden sehr einfache regressionsmodelle wie [Lineare Regression](https://en.wikipedia.org/wiki/Linear_regression) sowie erweiterte, wie Entscheidungsbäumen, [Random Gesamtstrukturen](https://en.wikipedia.org/wiki/Random_forest) [Neuronale Netzwerke](https://en.wikipedia.org/wiki/Artificial_neural_network)und Entscheidungsstrukturen erhöht.

Energiebedarf als regressionsproblem Prognose erstellen gibt uns Flexibilität in der tatsächliche Bedarf zeitreihendaten und externe Faktoren wie unsere Datenfeatures kombinierbar auswählen. Weitere Informationen über die ausgewählten Features werden in Engineering ( **und Funktion Engineering**) Siehe dieses Playbook Feature erörtert.

Aus unserer Erfahrung mit der Implementierung und Bereitstellung von Energie bei Bedarf Prognosen Pilot, wir fanden, dass erweiterte regressionsmodelle Azure ML für die besten Ergebnisse und wir stellen nutzen.

## <a name="model-evaluation"></a>Auswerten
Auswerten des hat einer wichtigen Rolle innerhalb des **Entwicklungszyklus Modell**. In diesem Schritt Prüfen des Modells und seine Leistung mit echten Daten überprüft. Bei der Modellierung verwenden wir einen Teil der verfügbaren Daten zum Trainieren des Modells. Während der Auswertungsphase nehmen wir die restlichen Daten zum Testen des Modells. Praktisch bedeutet dies, dass wir die neuen Daten Fütterung, die wurde neu strukturiert und enthält die gleichen Funktionen wie das Trainings-Dataset. Verwenden während der Validierung wir jedoch das Modell Vorhersagen Zielvariable als Variable verfügbaren Ziel angeben. Wir nennen diesen Prozess Modell zu bewerten. Wir würden die true Zielwerte und vergleichen sie die vorhergesagte. Das Ziel ist zu messen und minimieren Vorhersagefehler, d. h. die Differenz zwischen der Vorhersagen und den Wert true. Quantifizierung der Fehler Messung ist Schlüssel, da wir das Modell und überprüfen, ob der Fehler tatsächlich verringern möchten. Feinabstimmung des Modells möglich Modellparameter, die lernen steuern, ändern oder hinzufügen oder Entfernen von Funktionen (bezeichnet als [Parameter Sweep](https://channel9.msdn.com/Blogs/Windows-Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Praktisch bedeutet, dass wir müssen zwischen Featureentwicklung Modellierung, durchlaufen und Auswertung Phasen mehrmals bis wir den Fehler auf die erforderliche Ebene reduzieren können.

Unbedingt hervorgehoben werden die Vorhersagefehler nicht gleich NULL ist nie ein Modell, das perfekt jedes Ergebnis Vorhersagen kann. Allerdings ist eine bestimmte Größe an, der vom Unternehmen akzeptabel ist. Während der Validierung möchten wir sicherstellen, dass unser Modell Vorhersagefehler Ebene oder der Ebene der Toleranz besser. Deshalb die tolerierbare Fehler am Anfang des Zyklus Phase **Problemformulierung** festlegen.

### <a name="typical-evaluation-techniques"></a>Normale Evaluierungsverfahren
Es gibt verschiedene Arten der Vorhersage Fehler gemessen und quantifiziert werden kann. In diesem Abschnitt die Diskussion zur Auswertung Techniken Zeitreihen und spezifische Energiebedarf Planung der Schwerpunkt.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE steht für Absolute Prozentsatz Fehler bedeuten. Mit MAPE, die wir Berechnen der Unterschiede jedes prognostizierte zeigen und den tatsächlichen Wert des Punkts. Wir bewerten dann den Fehler pro durch Berechnung der Differenz gegenüber den tatsächlichen Wert. Im letzten Schritt Durchschnittswerte wir diese. Für MAPE verwendete mathematische Formel lautet wie folgt:

![MAPE Formel](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*<sub>t</sub> ist der eigentliche Wert, F<sub>t</sub> ist der vorhergesagte Wert und n ist der Prognosezeitraum.*

## <a name="deployment"></a>Bereitstellung
Der Entwurfsphase festgenagelt und überprüft die modellleistung sind wir in der Bereitstellungsphase gehen. In diesem Kontext bedeutet Bereitstellung ermöglicht Kunden das Modell mit tatsächlichen Vorhersagen auf in großem Umfang nutzen. Das Konzept der Bereitstellung ist Schlüssel in Azure ML, da unser Hauptziel ist ständig Vorhersagen und nicht nur aus den Daten erhalten den Einblick aufrufen. Die Bereitstellungsphase gehört, das Modell in großem Umfang genutzt werden können.

Rahmen Energie bedarfsplanung soll unsere aufrufen kontinuierlichen und regelmäßige prognostiziert, dass neue Daten für das Modell und die Prognosedaten an den verwendeten Client gesendet werden.

### <a name="web-services-deployment"></a>Bereitstellung von Services
Bereitstellung Hauptbaustein in Azure ML ist der Webdienst. Dies ist die effektivste Möglichkeit, Verbrauch ein Vorhersagemodell in der Cloud zu aktivieren. Webdienst kapselt das Modell und schließt ihn mit einer [RESTful](http://www.restapitutorial.com/) API (Application Programming Interface). Die API dient als Teil jeder Client-Code wie im folgenden Diagramm dargestellt.

![Bereitstellung und Verbrauch](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Ersichtlich, der Webdienst in die Cloud Cortana Intelligence Suite bereitgestellt sowie über die verfügbar gemachte REST API Endpunkt aufgerufen werden kann. Arten von Clients in verschiedenen Domänen kann gleichzeitig den Dienst über die Web-API aufrufen. Der Webdienst kann auch skalieren, um Tausende gleichzeitige Aufrufe unterstützt.

### <a name="a-typical-solution-architecture"></a>Normale Lösungsarchitektur
Beim Bereitstellen eines Lösung Prognosen Energiebedarfs interessiert uns eine durchgehende Lösung, die über den Webdienst Vorhersage und vereinfacht den gesamten Datenfluss. Gleichzeitig eine neue Prognose aufgerufen, müssten wir sicherstellen, dass das Modell mit den aktuellen Daten gefüllt wird. Das bedeutet, dass die neu gesammelten Rohdaten ist ständig aufgenommen, verarbeitet und verwandelt die erforderlichen Funktionen für das Modell erstellt wurde. Gleichzeitig möchten wir die Prognosedaten für Benutzerclients Ende verfügbar zu machen. Ein Beispiel fortlaufenden Zyklus (oder Datenpipeline) wird im folgenden Diagramm dargestellt:

![Nachfrage prognostizieren End to End-Datenfluss](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Dies sind die Schritte, die im Rahmen der Planung Bedarf Energiezyklus:
1.  Millionen von bereitgestellten Daten Meter generieren macht Daten kontinuierlich in Echtzeit.
2.  Diese Daten werden gesammelt und in einer Cloud-Repository (*z. B.*Azure Blob) hochgeladen werden.
3.  Vor der Verarbeitung, die Rohdaten zu einem Unterwerk oder regionaler Ebene aggregiert vom Unternehmen definiert.
4.  Feature-Verarbeitung (siehe **und Funktion verarbeitet**) dann erfolgt und erzeugt das die Datenmodell Schulung oder Bewertung Feature Set-Daten in einer Datenbank (*z.B.*SQL Azure) gespeichert.
5.  Umschulung Dienst aufgerufen Prognosemodell – neu trainieren, aktualisierte Version des Modells beibehalten werden, sodass vom Punktwertung Webdienst verwendet werden kann.
6.  Punktwertung Webdienst wird nach einem Zeitplan aufgerufen, die die erforderliche Planung Häufigkeit entspricht.
7.  Die Prognose ist in einer Datenbank gespeichert, die dem Verbrauch Endbenutzer zugreifen können.
8.  Verbrauch-Client Ruft die Prognosen, in das Raster angewendet und nach den erforderlichen Anwendungsfall verbraucht.

Es ist wichtig zu beachten, dass dieses gesamte vollständig automatisiert und wird ausgeführt. Die gesamte Orchestrierung des Datenzyklus erfolgt mithilfe von Tools wie [Azure Data Factory](http://azure.microsoft.com/services/data-factory/).

### <a name="end-to-end-deployment-architecture"></a>Durchgehende Bereitstellungsarchitektur
Um eine Anforderung Prognose Lösung auf Cortana Intelligence praktisch bereitstellen, müssen wir sicherstellen, dass die erforderlichen Komponenten eingerichtet und ordnungsgemäß konfiguriert.

Das folgende Diagramm zeigt eine normale Cortana Intelligenz-Architektur, die implementiert und koordiniert den Flow-Zyklus, die oben beschrieben:

![Durchgehende Bereitstellungsarchitektur](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Weitere Informationen über die Komponenten und die gesamte Architektur finden Sie Lösungsvorlage Energie.
