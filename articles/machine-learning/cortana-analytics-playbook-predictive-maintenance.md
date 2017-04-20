<properties
    pageTitle="Cortana Intelligence Lösung Vorlage Playbook für vorbeugende Wartung Luftfahrt und andere Unternehmen | Microsoft Azure"
    description="Eine Projektmappenvorlage mit Microsoft Cortana für die vorbeugende Wartung Luftfahrt, Dienstprogramme und Transport."
    services="cortana-analytics"
    documentationCenter=""
    authors="fboylu"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="fboylu" />

# <a name="cortana-intelligence-solution-template-playbook-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Cortana Intelligence Lösung Vorlage Playbook für vorbeugende Wartung Luftfahrt und andere Unternehmen

## <a name="executive-summary"></a>Zusammenfassung  
Vorbeugende Wartung ist eines der beliebtesten von Vorhersageanalysen mit unbestreitbar Vorteile enorme Kosten sparen. Diese Playbook soll eine vorbeugende Wartung Lösungen mit Schwerpunkt auf größere Anwendungsfälle.
Es ist bereit, dem Leser Kenntnisse der geschäftlichen häufigsten vorbeugende Wartung Probleme der qualifizierenden Geschäftsprobleme Lösung, Business, Modellen zum Erstellen von Projektmappen mit solchen Daten predictive lösen erforderlichen Daten und best practices mit Beispiel-Lösung.
Darüber hinaus werden die Besonderheiten des Vorhersagemodells wie Feature entwickelt, Entwicklung und Auswertung Modell. Im Wesentlichen bringt diese Playbook Geschäfts- und analytische Richtlinien für eine erfolgreiche Entwicklung und Bereitstellung von Projektmappen vorbeugende Wartung erforderlich. Diese Leitlinien sind das Publikum eine erste Lösung mit Cortana Intelligence Suite speziell Azure maschinelles lernen als Ausgangspunkt für ihre langfristige Wartungsstrategie vorbeugende. Die Dokumentation Cortana Intelligence Suite und Azure Machine Learning finden in [Cortana Analytics](http://www.microsoft.com/server-cloud/cortana-analytics-suite/overview.aspx) und [Azure maschinelles lernen](https://azure.microsoft.com/services/machine-learning/) .

>[AZURE.TIP]
Finden Sie einen technischen Leitfaden für die Implementierung dieser Lösungsvorlage [technischen Leitfaden der Vorlage Cortana Intelligence Lösung für die vorbeugende Wartung](cortana-analytics-technical-guide-predictive-maintenance.md).
Um ein Diagramm zu downloaden, die einen Überblick über diese Vorlage bietet finden Sie unter [Architektur der Vorlage Cortana Intelligence Lösung für die vorbeugende Wartung](cortana-analytics-architecture-predictive-maintenance.md).

## <a name="playbook-overview-and-target-audience"></a>Übersicht und Ziel Publikum Playbook  
Diese Playbook gliedert technische und nichttechnische Zielgruppen mit unterschiedlichen Hintergründen und Interessen vorbeugende Wartung Raum profitieren. Das Playbook Aspekte sowohl allgemeine verschiedener vorbeugende Wartung Solutions und Details der Implementierung. Der Inhalt auf beiden balanced Publikum, die nur Lösung Speicherplatz und den Typ der Anwendung sowie zum Implementieren dieser Lösung und damit die technischen Details interessiert sind.

Hauptteil des Inhalts in diesem Playbook übernimmt Daten Wissenschaft wissen und Know-how. Einige Teile des Playbook müssen jedoch etwas Vertrautheit mit Daten Wissenschaft Implementierungsdetails folgen können. Einführende Daten Wissenschaft wissen müssen aus den Abschnitten nutzen.

Im ersten Halbjahr Playbook umfasst eine Einführung in die vorbeugende Wartung wie eine vorbeugende Wartung zu Lösung eine Auflistung allgemeiner Anwendungsfälle mit den Details des Geschäftsproblems anhand der Daten um diese Anfragen und die Vorteile der Implementierung dieser Lösung vorbeugende Wartung. Diese Abschnitte müssen keine technische Kenntnisse im Bereich Vorhersageanalytik.

In der zweiten Hälfte des Playbook decken wir den Vorhersagemodellen Techniken für vorbeugende Wartung und diese Modelle Beispiele von Anwendungsfällen in der ersten Hälfte des Playbook beschrieben. Zum besseren Verständnis wird schrittweise preprocessing Daten wie Daten zu beschriften und Funktion engineering, Modell Auswahl, Training und Tests und Performance Evaluation best Practices. Diese Abschnitte sind für technische Publikum.


## <a name="predictive-maintenance-in-iot"></a>Vorbeugende Wartung IoT
Ungeplante Ausfallzeiten können äußerst destruktiv für Unternehmen auswirken. Ist wichtige Geräte um Nutzung und Leistung zu maximieren und minimieren teure, ungeplante Ausfallzeiten. Warten auf die Fehler ist einfach nicht in der geschäftlichen Vorgänge Szene erschwinglich. Um wettbewerbsfähig zu bleiben, suchen Unternehmen neue Methoden zur Maximierung der Leistung mit Verwendung der Daten aus verschiedenen Kanälen gesammelt. Eine wichtige Möglichkeit, Informationen analysieren ist prädiktive Analyseverfahren nutzen, die historisch Muster verwenden, um zukünftige Ergebnisse vorherzusagen. Eines der beliebtesten Lösungen heißt vorbeugende Wartung im Allgemeinen als u.a. in naher Zukunft Vorhersagen Versagen eines Vermögenswertes, dass Vermögenswerte überwacht werden können, um proaktiv Fehler erkennen und ergreifen, bevor Fehler auftreten. Folgenden erkennen Fehlermuster Elemente ermitteln, die am anfälligsten für Fehler. Diese frühzeitige Identifizierung von Problemen hilft beschränkt Wartungsressourcen mehr kostengünstige Bereitstellung und Qualität und supply Chain-Prozessen.

Mit dem Internet der Dinge (IoT) Anwendung vorbeugende Wartung wurde gewinnt zunehmend in der Branche die Datensammlung und Verarbeitung gereift zu generieren übertragen, speichern und Analysieren von Daten in Batches oder in Echtzeit. Einfache Entwicklung und Bereitstellung von End-to-End-Lösungen mit erweiterte Analyse mit vorbeugende wartungslösungen wohl des größten Vorteil dieser Technologie zu aktivieren.

Geschäftsprobleme vorbeugende Wartung Domäne zwischen hohen betrieblichen Risiken unerwartete Ausfälle und begrenzten Einblick in die Ursache von Problemen in einer komplexen Umgebung. Die meisten dieser Probleme kann unter folgenden Unternehmensfragen fallen kategorisiert werden:

-   Was ist die Wahrscheinlichkeit ein Geräts in naher Zukunft fehlschlägt?
-   Was ist die Restnutzungsdauer des Geräts?
-   Was sind die Ursachen der Fehler und welche Aktionen zur Wartung ausgeführt werden soll, um diese Probleme zu beheben?

Mithilfe dieser Fragen vorbeugende Wartung können Unternehmen:

-   Risiken Sie operative und erhöhen Sie Gesamtkapitalrendite durch Fehler erkennen, bevor sie auftreten
-   Senken Sie unnötige zeitbasierte Wartung und kontrollieren Sie Wartungskosten
-   Verbesserung der markenbild, Image und die resultierende entgangene Umsätze von Kunden Verluste.
-   Niedrigere Lagerkosten reduziert Lagerbestand den Minimalbestand Vorhersagen
-   Muster mit verschiedenen Wartungsprobleme verbunden

Vorbeugende Wartung Solutions bieten Unternehmen mit Kennzahlen wie Health Ergebnisse überwachen Echtzeit Anlagenzustand, geschätzte verbleibende Lebensdauer der Vermögenswerte, Empfehlung für proaktive Wartung und geschätzte Bestelldaten für den Austausch von Teilen.

## <a name="qualification-criteria-for-predictive-maintenance"></a>Qualifikationskriterien für die vorbeugende Wartung
Es ist sehr wichtig, dass nicht alle Anwendungsfälle oder Probleme können effektiv vorbeugende Wartung gelöst werden. Wichtige Qualifikationskriterien beinhalten, ob das Problem vorhersehbaren Natur, der klaren Aktion vorhanden ist, um Ausfälle zu vermeiden, wenn sie vorher und vor allem erkannt steht mit ausreichender Qualität zu Verwendung unterstützen. Hier liegt der Schwerpunkt auf die erforderlichen Daten für eine erfolgreiche vorbeugende Wartung Lösung.

Beim Erstellen der Vorhersagemodelle verwenden wir Daten zum Trainieren des Modells verborgene Muster erkennen und diese Muster in der Zukunft näher. Diese Modelle werden anhand ihrer Eigenschaften und das Ziel der Vorhersage beschrieben geschult. Geschulte Modell soll Vorhersagen für das Ziel nur die Funktionen der neuen Beispiele ansehen. Es ist wichtig, die Beziehung zwischen Funktionen und das Ziel der Vorhersage erfasst. Um eine effektive Maschine Lernmodell Schulen benötigen wir Daten enthält Features, die tatsächlich vorhersagekraft Ziel Vorhersage, d. h. Daten für Vorhersagen Ziel präzise vorhersagen zu erwarten ist.

Beispielsweise sollte soll Fehler Räder vorherzusagen, die Trainingsdaten RAD-Funktionen (z.B. Telemetrie spiegeln den Status der Räder der Kilometerleistung Auto laden, etc..) enthalten. Allerdings ist das Ziel Zug Engine Fehler vorhergesagt, benötigen wir wahrscheinlich Trainingsdaten, die Modul-Funktionen. Vor dem Erstellen der Vorhersagemodelle erwarten wir Unternehmen zu verstehen Anforderung Relevanz und Fachwissen, die benötigt wird, um wichtige Teilmengen von Daten für die Analyse auswählen.

Es gibt drei wichtige Datenquellen, auf die wir bei der Qualifizierung ein geschäftliches Problem für eine vorbeugende wartungslösung:

1.  Fehler-Geschichte: Normalerweise sind in vorbeugende Wartung Applikationen Fehlerereignisse selten. Jedoch muss beim Vorhersagemodelle, die Fehler vorhergesagt Algorithmus Normalbetrieb Muster sowie Muster Fehler durch den Erfassungsvorgang. Daher muss die Trainingsdaten genügend Beispiele in beiden Kategorien enthält, um diese zwei Mustern lernen. Aus diesem Grund müssen Daten genügend Ereignisse enthält. Fehlerereignisse in Daten finden und Teile ersetzen Verlauf oder Anomalien in den Trainingsdaten können auch verwendet werden als Fehler von Experten.
2.  Wartung, Reparatur Geschichte: Eine wichtige Datenquelle für vorbeugende Wartung Solutions ist die detaillierte Geschichte des Vermögenswertes mit Informationen über die Komponenten ersetzt, vorbeugende Wartung aktiviert usw. durchgeführt. Es ist sehr wichtig, diese Ereignisse als diese beeinflussen die Beeinträchtigung Muster und diese Informationen verursacht irreführende Ergebnisse.
3.  Computer-Bedingung: Um vorhersagen, wie viele Tage (Hours, Miles, Transaktionen, usw.) ein Computers dauert vor dem Ausfall, gehen wir Zustand des Computers mit der Zeit beim Betrieb beeinträchtigt. Daher erwarten wir Einbußen führt Daten zeitlich veränderlichen Funktionen enthält, die dieses Muster Alterung und Anomalien erfassen. In IoT Applikationen stehen Telemetriedaten aus verschiedenen Sensoren ein gutes Beispiel. Um vorhersagen sollte ein Gerät innerhalb fehlschlagen, sollten im Idealfall heruntergestuften Trend während dieses Zeitraums vor dem tatsächlichen Fehlerereignis Datenerfassung.

Außerdem benötigen wir Daten, die direkt auf die der Zielressource der Vorhersage. Die Entscheidung des Ziels basiert auf beide Geschäftsbetrieb muss und Daten. Unter Zug Rad Fehler Vorhersage beispielhaft, können wir Vorhersagen, "Wenn das Rad einen Fehler wird" oder "der gesamte Zug ein Fehler wird". Zuerst zielt mehr Komponenten während der zweiten Fehler des Zuges abzielt. Die zweite ist eine allgemeine Frage, die viel mehr verteilten Datenelemente als ersten erschweren das Erstellen eines Modells erforderlich sind. Umgekehrt Rad Fehler vorherzusagen an Daten das allgemeine Zug nicht durchführbar möglicherweise keine Informationen auf Komponentenebene enthalten sind. Im Allgemeinen ist es sinnvoller, bestimmte Ereignisse als allgemeine vorherzusagen.

Ist eine allgemeine Frage normalerweise über Fehlerdaten ist "wie viele Fehlerereignisse sind erforderlich, um ein Modell und wie viele als"ausreichend"gilt? Gibt es keine eindeutige Antwort auf diese Frage in vielen Situationen Vorhersageanalysen ist es normalerweise die Qualität der Daten, die bestimmt, was akzeptabel ist. Enthält das Dataset nicht Funktionen, die von Datenträgerausfällen relevant sind, dann auch wenn viele Fehlerereignisse erstellen ein gutes Modell möglicherweise nicht möglich. Jedoch ist Faustregel die weitere Fehlerereignisse bessere Modell ist und eine grobe Schätzung der Anzahl Fehler Beispiele erforderlich sind ein Kontext und Daten abhängige Messgröße. Dieses Problem wird im Abschnitt behandeln Arbeitsvorgänge Datasets erläutert, wo wir Methoden das Problem nicht genügend Fehler zu schlagen.

## <a name="sample-use-cases"></a>Fallbeispiele
Dieser Abschnitt behandelt eine Auflistung von Anwendungsfällen vorbeugende Wartung aus mehreren Bereichen Luft-und Raumfahrt, Dienstprogramme und Transport. Jeder Unterabschnitt erzeugt in diesen Bereichen gesammelt Anwendungsfälle und Geschäftsproblem Daten um das Geschäftsproblem und die Vorteile einer Lösung für die vorbeugende Wartung erläutert.

### <a name="aerospace"></a>Luftfahrt
#### <a name="use-case-1-flight-delay-and-cancellations"></a>Anwendungsbeispiel 1: Flugverspätung und absagen
##### <a name="business-problem-and-data-sources"></a>*Geschäftliche Problem und Datenquellen*
Eines der wichtige Probleme Airlines Gesicht erheblichen Kosten, die mit Verzögerung durch mechanische Probleme verbunden sind. Wenn mechanische Fehler repariert werden können, können auch Flüge gestrichen. Dies ist sehr teuer verzögert in Planung sowie Prozesse, Ursachen schlechten Ruf und Unzufriedenheit mit vielen Problemen zu Problemen führen. Airlines sind insbesondere solche mechanischen Fehler im Voraus Vorhersagen, Flug verzögert oder absagen zu reduzieren können. Vorbeugende Wartung Lösung in diesen Fällen soll die Wahrscheinlichkeit eines Luftfahrzeugs verzögert oder abgebrochen, basierend auf Datenquellen wie Geschichte und Flight Route Wartungsinformationen vorherzusagen. Die zwei wichtigsten Datenquellen diesem sind Flugteilstrecken und Seite Protokolle. Bein Daten enthält Daten über die Route Flugdaten wie Datum und Uhrzeit der Abfahrt, Ankunft, Abfahrt und ankunftsflughäfen. Seitendaten umfasst eine Reihe von Codes für Fehler und Wartung, die durch den Mitarbeiter aufgezeichnet werden.

##### <a name="business-value-of-the-predictive-model"></a>*Geschäftswert des Vorhersagemodells*
Mithilfe der verfügbaren Verlaufsdaten, wurde ein Vorhersagemodell erstellt mit einem Algorithmus mit mehreren Klassifizierung vorherzusagen Art des mechanischen Problems führt zu einer Verzögerung oder Stornierung eines Flugs innerhalb der nächsten 24 Stunden. Machen Sie diese Vorhersage, können Wartungsarbeiten Aktionen zu Risiken während eines Luftfahrzeugs gewartet wird somit Verzögerungen oder absagen. Azure Machine Learning-Webdienst verwenden, können die Vorhersagemodelle nahtlos und einfach in vorhandene Betriebssystemplattformen Airlines integriert werden. 

#### <a name="use-case-2-aircraft-component-failure"></a>Anwendungsbeispiel 2: Luftfahrzeug Komponentenfehler
##### <a name="business-problem-and-data-sources"></a>*Geschäftliche Problem und Datenquellen*
Flugzeugmotoren sind sehr empfindliche und teure Geräte und Teileaustausch Modul gehören die Wartungsaufgaben im Bereich Luftfahrt. Wartungslösungen für Luftfahrtunternehmen müssen sorgfältig verwaltet komponentenverfügbarkeit, und planen. Die Möglichkeit, sammeln Intelligence Komponente Zuverlässigkeit zu erheblichen Investitionskosten führt. Die wichtigsten Datenquelle für diesen Anwendungsfall ist Telemetriedaten aus einer Reihe von Sensoren im Flugzeug mit Informationen über den Zustand des Luftfahrzeugs. Daten wurden auch zur Identifizierung von Komponenten Fehler als Ersetzung vorgenommen.

##### <a name="business-value-of-the-predictive-model"></a>Geschäftswert des Vorhersagemodells
Ein klassifizierungsmodell mit mehreren Klasse wurde erstellt, die die Wahrscheinlichkeit eines Fehlers durch eine bestimmte Komponente innerhalb des nächsten Monats Vorhersagen. Mit dieser Lösung können Airlines Komponente reparieren senken, Verbesserung der Verfügbarkeit von Komponente reduzieren Lagerbestände verwandter Anlagen und Wartung Planung.

### <a name="utilities"></a>Dienstprogramme
#### <a name="use-case-1-atm-cash-dispense-failure"></a>Anwendungsbeispiel 1: ATM bankomaten Fehler
##### <a name="business-problem-and-data-sources"></a>*Geschäftliche Problem und Datenquellen*
Führungskräften Anlage intensive Branchen häufig Zustand ist primäre betriebliche Risiken für ihre Unternehmen unerwartete Fehler ihrer Ressourcen. Beispielsweise ist Fehler Maschinen wie Geldautomaten in Banken ein häufiges Problem häufig auftritt. Derartige Probleme stellen vorbeugende Wartung Solutions Betreiber Maschinen wünschenswert. Vorhersage Problem werden in diesem Fall verwenden die Wahrscheinlichkeit berechnen, dass ein ATM Bargeld Auszahlung durch einen Fehler in den Geldautomaten wie ein Papierstau oder Teil Fehler unterbrochen. Wichtige Datenquellen für diesen Fall sind Sensorwerte, die Maßangaben sammeln während Bargeld Notizen verzichtet werden und auch gesammelten Daten mit der Zeit. Daten enthalten Sensorwerte pro jede Transaktion abgeschlossen und Sensorwerte pro jedes verzichtet. Sensor Messwerte bereitgestellten Maße wie Lücken zwischen Notizen Stärke beachten Ankunft Abstand usw.. Wartung enthalten Fehlercodes und Informationen. Diese wurden zum Identifizieren von Fehlern verwendet.

##### <a name="business-value-of-the-predictive-model"></a>*Geschäftswert des Vorhersagemodells*
Zwei Vorhersagemodelle wurden erstellt, um Fehler in den Entzug Transaktionen und Fehler in den einzelnen verzichtet während einer Transaktion vorhergesagt. Transaktionsfehler im Voraus Vorhersagen, können Geldautomaten proaktiv verarbeitet werden, um Fehler zu vermeiden. Außerdem Datenträgerausfällen Hinweis eine Transaktion ist wahrscheinlich Fehler vorzeitig aufgrund einer Notiz verzichten Fehler möglicherweise sollten Sie den Prozess beenden und warnen Kunden für unvollständige Transaktion nicht warten die Wartungsarbeiten an nach einem Fehler die größere Unzufriedenheit führen kann.

#### <a name="use-case-2-wind-turbine-failures"></a>Anwendungsbeispiel 2: Wind Turbine Fehler
##### <a name="business-problem-and-data-sources"></a>*Geschäftliche Problem und Datenquellen*
Mit der des Umweltbewusstseins sind Anlagen eine der Hauptquellen für Energie und in der Regel Millionen von Dollar Kosten. Die wichtigsten Komponenten von Anlagen gehört Generator Motor ausgestattet mit vielen Sensoren, die Turbine Konditionen und Status überwachen können. Die Sensorwerte enthalten wertvolle Informationen zum Erstellen eines Vorhersagemodells wichtigen Key Performance Indicators (KPIs) Vorhersagen verwendet werden kann wie zu Fehler für Komponenten der Anlage. Daten für diesen Anwendungsfall stammen aus mehreren Anlagen, die in anderen Farm Standorten befinden. Maße nahe hundert Sensoren jede Anlage wurden alle 10 Sekunden ein Jahr. Diese Werte enthalten wie Temperatur, Generator Geschwindigkeit und Turbine Power Generator Wicklung.

##### <a name="business-value-of-the-predictive-model"></a>*Geschäftswert des Vorhersagemodells*
Vorhersagemodelle wurden erstellt, um verbleibende Nutzungsdauer für Generatoren und Temperatursensoren zu schätzen. Durch die Vorhersage der Wahrscheinlichkeit für den Ausfall, können Wartungstechniker verdächtigen Anlagen konzentrieren, die voraussichtlich bald nicht Mal Wartung Regime ergänzen.
Außerdem bringen Vorhersagemodelle Einblick auf Beitrag für verschiedene Faktoren die Wahrscheinlichkeit eines Ausfalls wodurch Unternehmen ein besseres Verständnis der Ursache der Probleme.

#### <a name="use-case-3-circuit-breaker-failures"></a>Anwendungsbeispiel 3: Schutzschalter Fehler
##### <a name="business-problem-and-data-sources"></a>*Geschäftliche Problem und Datenquellen*
Strom und Gas Operationen, die Erzeugung, Verteilung und Verkauf von Strom enthalten erfordern beträchtliche Wartung Stromleitungen Betrieb jederzeit Energie an private Haushalte gewährleisten sind. Solche Vorgänge ist äußerst wichtig, da fast jeder Entität erfolgt durch Probleme in den Bereichen, die sie auftreten. Die Leistungsschalter sind für solche Operationen sind Geräte, die elektrischen Strom bei Problemen und Kurzschluss Schäden Stromleitungen verhindern Ausschneiden. Das Geschäftsproblem zu diesem Anwendungsfall ist vorherzusagen Schutzschalter Fehler wartungsprotokolle, Befehle und technischen Spezifikationen angegeben.

Drei wichtige Datenquellen für diesen Fall wartungsprotokolle sind, die Maßnahmen und systematisch vorbeugende Maßnahmen, senden Betriebsdaten, die automatische und manuelle Befehle an Leistungsschalter wie öffnen und schließen Aktionen und technische Daten über die Eigenschaften der einzelnen Schutzschalter wie Jahr, Speicherort, Modell usw.

##### <a name="business-value-of-the-predictive-model"></a>*Geschäftswert des Vorhersagemodells*
Vorbeugende Wartung Solutions helfen reduzieren Kosten und erhöhen den Lebenszyklus von wie Leistungsschalter. Diese Modelle können auch Netz denn Modelle Warnungen voraus, die unerwartete Fehler reduzieren die weniger Unterbrechung des Dienstes zu verbessern.

#### <a name="use-case-4-elevator-door-failures"></a>Anwendungsbeispiel 4: Aufzug Tür Fehler
##### <a name="business-problem-and-data-sources"></a>*Geschäftliche Problem und Datenquellen*
Die meisten große aufzugsunternehmen haben Millionen Aufzüge weltweit ausgeführt. Um einen Wettbewerbsvorteil konzentrieren sie Zuverlässigkeit Kunden entscheidend ist. Auf das Internet der Dinge können verbinden ihrer Aufzüge in die Cloud und Datensammlung Aufzug Sensoren und Systeme sie in wertvolle Business Intelligence Datentransformation enorm Operationen mit vorhersehbaren und Präventive Wartung, die nicht, die die Wettbewerber noch verbessert. Die geschäftliche Anforderung für diesen Fall ist eine vorbeugende Wissensdatenbank-Anwendung bereitstellen, die sagt das Potenzial der Tür Fehler verursacht. Die erforderlichen Daten für diese Implementierung besteht aus drei Teilen die Aufzug statische Funktionen (z.B. Bezeichner Vertrag wartungshäufigkeit, Gebäude usw.), Verwendung (z. B. Anzahl der Tür Zyklen, durchschnittliche Tür schließen usw.) und Fehler (d. h. historisch Fehler Datensätze und deren Ursachen).

Multiclass logistische Regressionsmodell wurde mit Azure Computer zur Lösung des Problems Vorhersage mit integrierten statischen Funktionen und Daten als KEs und Ursachen historisch Fehler Datensätze als Klasse Etiketten erstellt. Dieses Vorhersagemodell wird von einer Anwendung auf einem mobilen Gerät genutzt durch Techniker dient zur Verbesserung der Leistung. Geht ein Techniker vor Ort Aufzug reparieren, steht er App empfohlene Ursachen und optimale Vorgehensweisen Wartungsarbeiten aufzugtüren so schnell wie möglich beheben.

### <a name="transportation-and-logistics"></a>Transport und Logistik
#### <a name="use-case-1-brake-disc-failures"></a>Anwendungsbeispiel 1: Bremse CD-Fehler
##### <a name="business-problem-and-data-sources"></a>*Geschäftliche Problem und Datenquellen*
Normale Wartungsrichtlinien für Fahrzeuge enthalten korrigierender und vorbeugender Wartungsarbeiten. Wartungsarbeiten impliziert, dass Fahrzeug repariert, nachdem ein Fehler verursachen schwere Unannehmlichkeiten für den Treiber durch ein unerwarteter Fehler aufgetreten ist und Zeitverschwendung bei einem Besuch Mechaniker. Die meisten Fahrzeuge unterliegen einer Richtlinie Wartung erfordert bestimmte Kontrollen in einem Zeitplan nicht den tatsächlichen Zustand der Auto-Subsysteme berücksichtigt durchführen. Keiner dieser Ansätze sind erfolgreich Probleme vollständig beseitigt. Speziell hier ist Bremse CD Datenträgerausfällen basierend auf Daten über Sensoren in der Reifen Auto welches historisch treibenden Muster und Sonstiges Auto ausgesetzt ist. Die wichtigste Datenquelle für diesen Fall ist die Daten, die beispielsweise Accelerations Bremsung Muster Abstände, Geschwindigkeit usw. messen. Diese Informationen zusammen mit anderen statischen Informationen Auto Funktionen Hilfe Build eine gute vorausgesagt festgelegt, die in ein Vorhersagemodell verwendet. Eine andere sind wichtige Informationen Fehlerdaten aus Teil Reihenfolge Datenbank (zum Teil Bestelldaten und Mengen möglichst Autos gewartet werden in der Händler) abgeleitet werden.

##### <a name="business-value-of-the-predictive-model"></a>*Geschäftswert des Vorhersagemodells*
Der Geschäftswert einen vorhersehbaren Ansatz ist erhebliche. Vorbeugende wartungssystem kann anhand eines Vorhersagemodells Händler besuchen planen. Das Modell kann sensorische Informationen basieren, die den Zustand und die treibende Geschichte darstellt. Dieser Ansatz minimiert das Risiko unerwarteter Fehler sowie vor der nächsten regelmäßigen Wartung auftreten.
Sie können auch die unnötige Wartung verringern. Treiber kann vorbeugend mitgeteilt, dass eine Änderung der Teile möglicherweise Wochen erforderlich und Händler mit diesen Informationen. Der Geber kann dann eine einzelne Wartungspaket für den Treiber vorbereiten.

#### <a name="use-case-2-subway-train-door-failures"></a>Anwendungsbeispiel 2: U-Bahn Zug Tür Fehler
##### <a name="business-problem-and-data-sources"></a>*Geschäftliche Problem und Datenquellen*
Hauptgründe verzögert und Probleme auf u-Bahn gehört Tür Fehler Wagen. Vorhersage haben einen Waggon Tür Fehler oder können die Zahl der Tage bis nebenan Fehler möglicherweise ist wichtig Weitsicht. Es bietet Ihnen die Möglichkeit zu optimieren Zug Tür Wartung des Zuges Zeit.

#### <a name="data-sources"></a>Datenquellen

Drei Quellen in diesem Anwendungsfall sind 

- **Ereignisdaten Schulen**die Verlaufsdaten Zug Ereignisse, 
- **Wartungsdaten** Wartungsarten Reihenfolge Typen und Prioritätscodes  
- **Datensätze von Fehlern**.

##### <a name="business-value-of-the-predictive-model"></a>*Geschäftswert des Vorhersagemodells*
Zwei Modelle wurden erstellt, zum nächsten Tag fehlerwahrscheinlichkeit binäre Klassifizierung mit Tage bis Regression mit Fehler vorhergesagt. Ähnlich wie bei früheren Fälle, erstellen die Modelle Perspektiven Verbesserte Servicequalität und Kundenzufriedenheit ergänzen die regelmäßige Wartung Regime

## <a name="data-preparation"></a>Vorbereiten der Daten
### <a name="data-sources"></a>Datenquellen
Allgemeine können Daten vorbeugende Wartung Probleme wie folgt zusammengefasst werden:

-   Fehler Geschichte: Fehler Verlauf eines Computers oder einer Komponente des Computers.
-   Wartungsgeschichte: Reparaturverlauf eines Computers, z. B. Fehlercodes, vorherige Wartungsaktivitäten oder Komponentenaustausch.
-   Maschine Bedingung und Verwendung: des Betriebszustandes Computer z.B. Daten von Sensoren gesammelt.
-   Maschine Funktionen: die Funktionen eines Computers, z. B. Hubraum Marke und das Modell, Standort.
-   Operator Funktionen: die Funktionen des Operators, z. B. Geschlecht, Erfahrung.

Es ist möglich und sofern Fehler Geschichte Wartung Geschichte wie in Form von Bestelldaten für Ersatzteile oder spezielle Fehlercodes enthalten ist. In diesen Fällen können Fehler Wartung Daten extrahiert werden. Darüber hinaus haben verschiedene Geschäftsbereiche eine Vielzahl anderer Datenquellen, die Fehlermuster beeinflussen die hier nicht umfassend aufgeführt sind. Diese sollten consulting Experten beim Vorhersagemodelle identifiziert werden.

Beispiele über Daten von Anwendungsfällen sind:

Fehler Geschichte: kämpfen Verzögerung Datumsangaben, Luftfahrzeuge Komponente Fehler Datumsangaben und Typen, ATM Cash Rücknahme Transaktionsfehler, Zug vorhanden Tür Fehler, Bremse Disk Replacement Bestelldaten, Wind Turbine Fehler Datumsangaben und Schutzschalter Befehl Fehler.

Wartungsgeschichte: Flug Fehlerprotokolle, Transaktionsprotokolle Fehler ATM-Daten einschließlich Wartung, kurze Beschreibung usw. und Schutzschalter Daten trainieren.

Maschine Bedingung und Verwendung: Flight leitet und, Schulen Sensordaten Flugzeugmotoren Sensorwerte ATM-Transaktionen Ereignisdaten, Sensorwerte Anlagen, Aufzüge und verbundenen Fahrzeuge.

Maschine Funktionen: Schutzschalter Spezifikationen wie Spannung, Geolocation oder Auto Features wie Fabrikat, Modell, Engine Größe Reifentypen, Produktion usw..

Angesichts die obigen Datenquellen, sind zwei wichtigsten Datentypen, die wir vorbeugende Wartung Domäne beobachten Zeitdaten und statische Daten.
Fehler Geschichte Computer Startbedingungen reparieren Geschichte Verwendung fast immer kommen mit Timestamps die Zeit der Auflistung für jedes Datenelement. Computer und Operator Funktionen sind im Allgemeinen statisch, da sie normalerweise die technischen Spezifikationen von Computern oder Betreibers Eigenschaften beschreiben. Kann diese Features ändern und in diesem Fall sollten sie als Zeitstempel Datenquellen behandelt werden.

### <a name="merging-data-sources"></a>Zusammenführen von Daten

Vor von Featureentwicklung oder Kennzeichnung Prozess, müssen zunächst Daten in das Formular zu Features vorbereiten. Das Ziel soll einen Datensatz für jede Zeiteinheit für jede Anlage mit den Funktionen und Etiketten den Algorithmus für maschinelles lernen zugeführt werden. Um das Bereinigen endgültige DataSet vorzubereiten, sollten einige vorverarbeitet Schritte. Zunächst wird die Dauer der Datensammlung in Einheiten unterteilen, jeder Datensatz eine Zeiteinheit für eine Anlage gehört. Daten kann auch andere Einheiten wie Aktionen aufgeteilt aber Einfachheit wir Einheiten für den Rest der Erklärung.

Die Maßeinheit für Zeit möglich Meilen oder Transaktionen abhängig von der Effizienz der Vorbereitung der Daten und die beobachteten in der Anlage in eine anderen oder andere Faktoren für die Domäne in Sekunden, Minuten, Stunden, Tage, Monate, Zyklen. Die Zeiteinheit müssen also nicht wie die Häufigkeit der Datensammlung in vielen Fällen Daten nicht keinen Unterschied von einem Gerät zum anderen zeigen können. Z. B. wenn die Werte 10 Sekunden gesammelt wurden, vergrößert Entnahme einer Einheit von 10 Sekunden für die gesamte Analyse einige Beispiele ohne weitere Angaben. Eine bessere Strategie wäre Durchschnitt Stunde als Beispiel verwendet.

Generische Datenschemas Beispiel für mögliche Datenquellen im vorigen Abschnitt beschrieben werden:

Daten: Diese Datensätze Wartungsaktionen ausgeführt werden. Raw Wartungsdaten stammen in der Regel mit Aktiva-ID Zeitstempel mit Informationen zu diesem Zeitpunkt welche Wartungsarbeiten durchgeführt wurden. Wartungsarbeiten müssen bei solchen Rohdaten kategorische Spalten jeder Kategorie entspricht einer Aktion Wartungsart übersetzt werden. Das grundlegende Schema für Daten zählen Anlage-ID, Zeit und Wartung Aktion Spalten.

Fehler Datensätze: Dies sind Datensätze, die auf Vorhersagen gehören nennen wir Fehler oder Fehlergrund. Ereignisse der spezifischen Bedingung definierten Fehler oder bestimmte Fehlercodes können. In einigen Fällen enthält Daten mehreren Fehlercodes die Ausfälle von Interesse entsprechen. Nicht alle Fehler sind Vorhersage Features erstellen, die mit Fehlern korrelieren können andere Fehler normalerweise verwendet werden. Das grundlegende Schema für Fehler Datensätze zählen Aktiva-ID, Zeit und Fehler oder Ausfall Grund Spalten, wenn verfügbar ist.

Maschine Zustände: vorzugsweise Echtzeit Überwachung von Daten des Betriebszustandes der Daten sind. Für Tür-Fehler sind Tür öffnen und schließen gute Indikatoren über den aktuellen Zustand der Türen. Das grundlegende Schema bei Computer würde Anlage-ID, Zeit und Bedingung Spalten enthalten.

Maschinen-und Operator: Maschinen-und Operator in einem Schema zu welchem mit Anlage und Operator Eigenschaften der Anlage betrieben wurde zusammengeführt werden. Z. B. ein Auto normalerweise einen Treiber mit Attributen wie Alter, Erfahrung etc. gehört. Wenn diese Daten mit der Zeit ändert, werden diese Daten sollte auch eine Zeitspalte und Zeit unterschiedliche Daten zur Generierung von Funktion behandelt werden. Das grundlegende Schema bei Computer zählen Aktiva-ID, Anlage Features Operator-ID und Operator Funktionen.

Die endgültige Tabelle vor beschriften und Funktion zur Erstellung von links verbinden Computer Tabelle mit Fehler Aktiva-ID und Felder generiert werden kann. Diese Tabelle kann anschließend mit Daten für Aktiva-ID verknüpft und Felder und schließlich mit Operator verfügt über Inventarnummer ein. Erste left Join lässt null-Werte für Fehler Spalte als normal ist, diese durch ein Indikatorwert für den normalen Betrieb verwertet werden können. Diese Fehler Spalte wird zum Erstellen von Etiketten für das Vorhersagemodell verwendet.

### <a name="feature-engineering"></a>Featureentwicklung
Der erste Schritt bei der Modellierung ist Featureentwicklung. Die Idee der KE-Erzeugung ist konzeptionell beschreiben und abstrakte Gesundheitszustand des Computers zu einem bestimmten Zeitpunkt mit Verlaufsdaten von gesammelten Daten, der Zeitpunkt. Im nächsten Abschnitt Überblick wir den Typ der Methoden für die vorbeugende Wartung und wie die Bezeichnung für jede Technik. Welche Technik verwendet werden soll hängt das Problem und. Jedoch können unten beschriebenen Verfahren Feature engineering als Grundlage zum Erstellen von Funktionen verwendet werden. Folgenden Diskutieren wir Lag-Funktionen, die von Datenquellen mit Zeitstempel und statische Daten aus Datenquellen erstellt statische Funktionen erstellt werden müssen und Beispiele von Anwendungsfälle.

#### <a name="lag-features"></a>Lag-Funktionen
Wie bereits erwähnt, vorbeugende Wartung kommt Verlaufsdaten normalerweise mit Zeitstempeln, die Zeit der Auflistung für jedes Datenelement. Es gibt vielfältige Funktionen aus den Daten mit Zeitstempel erstellen. In diesem Abschnitt besprechen wir einige dieser Methoden für die vorbeugende Wartung verwendet. Wir werden jedoch nicht durch diese Methoden begrenzt. Da Featureentwicklung eines der kreativen Vorhersagemodellen gilt, möglicherweise zu Features erstellen. Hier bieten wir einige allgemeinen Techniken.

##### <a name="rolling-aggregates"></a>*Paralleles Aggregate*
Für jeden Datensatz eine Anlage wählen wir ein paralleles Größe "W" ist die Anzahl der Zeiteinheiten, die wir historisch Aggregate für berechnen möchten. Berechnen Sie dann Rollen aggregate Funktionen W Perioden vor dem Datum des Datensatzes. Beispiele parallelen Aggregate kann Rollen zählt bedeutet Standardabweichungen, basierend auf Standardabweichungen Ausreißern, CUSUM Maßnahmen, minimalen und maximalen Werte für das Fenster. Eine andere interessante Technik ist Trend ändert, Spitzen und Algorithmen, die Anomalien Anomalie Algorithmen mit kurzen ändert.

Finden Sie unter Abbildung 1, wobei wir Sensorwerte für jede Zeiteinheit mit blauen Linien für eine Anlage erfasst und markieren die parallelen durchschnittliche Funktion Berechnung für W = 3 Datensätze t<sub>1</sub> und t<sub>2</sub> bzw. orange und grüne Gruppen angegeben sind.

![Abbildung 1. Paralleles aggregate features](media/cortana-analytics-playbook-predictive-maintenance/rolling-aggregate-features.png)

Abbildung 1. Paralleles aggregate features

Beispielsweise wurden für Luftfahrzeuge Komponentenausfall Sensorwerte aus letzte Woche, letzten drei Tage und der letzte Tag zum parallelen bedeutet, die Standardabweichung und Summe Funktionen erstellen. ATM-Fehlern, raw Sensorwerte und Rollen bedeuten, Median, Bereich, Standardabweichungen wurden entsprechend Nummer Ausreißer über drei Standardabweichungen, oberen und unteren CUMSUM Funktionen verwendet.

Flug Verspätung Vorhersage zählt Fehler verwendeten Codes letzte Woche wurden Funktionen erstellen. Zug Tür Fehler wurden zählt die Ereignisse am letzten Tag zählt der Ereignisse der letzten 2 Wochen und Varianz der Anzahl der Ereignisse der letzten 15 Tage zur Verzögerung Funktionen erstellen. Gleiche gezählt wurde für Wartung Ereignisse verwendet.

Darüber hinaus ist eine Berechnung, die sehr groß (ex. Jahre), kann sich die ganze Geschichte der Anlage wie alle Wartung Datensätze zählen Fehler, etc.. bis zum Zeitpunkt des Datensatzes. Diese Methode wurde für die Inventur Schutzschalter Fehlern für die letzten drei Jahre verwendet. Auch bei Fehlern Zug alle Wartungsereignisse gezählt erstellen eine Wartung Langzeitfolgen erfassen.

##### <a name="tumbling-aggregates"></a>*Tumbling Aggregate*
Für jeden Datensatz mit der Bezeichnung einer Anlage wählen wir Fenster Größe 'W-<sub>k'</sub>k ist die Anzahl oder Größe "W" Wir wollen Lag Funktionen für Windows. "k" kann zu langfristigen Verschlechterung Muster erfassen oder wenige kurzfristige Effekte erfassen entnommen werden. Anschließend verwenden wir k tumbling Windows W-<sub>k</sub> , W -<sub>(k-1)</sub>,..., W -<sub>2</sub> W -<sub>1</sub> zu aggregieren Features für Perioden vor dem Stichtag (siehe Abbildung 2). Diese auch Rollen Windows auf Datensatzebene für eine Zeiteinheit die nicht erfasst wird in Abbildung 2, aber die Idee ist die gleiche wie in Abbildung 1, t-<sub>2</sub> auch mit parallelen Effekt veranschaulicht.

![Abbildung 2. Tumbling aggregate features](media/cortana-analytics-playbook-predictive-maintenance/tumbling-aggregate-features.png)

Abbildung 2. Tumbling aggregate features

Beispiel für Anlagen, W = 1 und k = 3 Monate wurden zur Verzögerung Funktionen für jede der letzten 3 Monate mit oberen und unteren Ausreißer erstellen.

#### <a name="static-features"></a>Statische Funktionen
Technische Daten der Geräte wie Herstellungsdatum, Modellnummer, Ort sind. Verzögerung hauptsächlich numerische Natur sind, werden statische Funktionen normalerweise kategorischen Variablen in den Modellen. Als ein Beispiel Schutzschalter Eigenschaften wie Spannung, Strom und Stromversorgung neben Transformer wurden Stromquellen usw. verwendet. Bremse Datenträger Fehler Räder Typ Reifen so als ob sie aus oder Stahl wurden einige statischen Funktionen verwendet.

Während der KE-Erzeugung einige wichtige Schritte wie fehlende Werte und Normalisierung durchzuführen. Es gibt zahlreiche Methoden der fehlenden Wert Zuschreibung und datennormalisierung, die hier nicht beschrieben wird. Allerdings ist es vorteilhaft, probieren verschiedene Methoden Vorhersage Leistung möglich ist.

Letzte Feature-Tabelle nach KE engineering im vorigen Abschnitt beschriebenen Schritte in etwa das folgende Schema wird als Zeiteinheit ein Tag ist:

|Aktiva-ID|Zeit|Feature-Spalten|Bezeichnung|
|---|---|---|---|
|1|Tag1|||
|1|Tag2|||
|...|...|||
|2|Tag1|||
|2|Tag2|||
|...|...|||

## <a name="modeling-techniques"></a>Techniken
Vorbeugende Wartung ist eine sehr umfangreiche Domäne häufig mit geschäftlichen Fragen, die aus vielen verschiedenen Blickwinkeln Vorhersagemodellen Perspektive angegangen werden können. In den nächsten Abschnitten bieten wir die wichtigste Techniken, mit denen verschiedene Geschäftsfragen zu modellieren, die vorbeugende Wartung Lösungen beantwortet werden können. Zwar vergleichbar, kann jedes Modell eigene erstellen Etiketten die ausführlich beschrieben. Als zugehörige Ressource finden Sie vorbeugende Wartung-Vorlage, die im Beispiel Versuche innerhalb von Azure Machine Learning enthalten. Links zu Online-Material für diese Vorlage werden im Ressourcenabschnitt bereitgestellt. Sie sehen wie Feature engineering Techniken erläutert und modellierungstechnik, die in den folgenden Abschnitten beschriebenen Luftfahrzeuge Engine Fehler mit Azure Machine Learning Vorhersagen angewendet werden.

### <a name="binary-classification-for-predictive-maintenance"></a>Binäre Klassifizierung für die vorbeugende Wartung
Binäre Klassifizierung für die vorbeugende Wartung ist die Wahrscheinlichkeit Vorhersagen, die Geräte innerhalb eines zukünftigen Zeitraums nicht verwendet. Der Zeitraum bestimmt und anhand von Geschäftsregeln und die Daten. Einige Zeiträume sind minimale Zeit, Ersatzteile wahrscheinlich Zeitaufwand für Wartung Ressourcen so Instandhaltungsroutinen um das Problem zu beheben, das während dieses Zeitraums kann zu Schäden Komponenten ersetzen. Wir nennen diesen Zeitraum zukünftige Horizont "X".

Um binäre Klassifizierung zu verwenden, müssen wir zwei Arten Beispiele nennen wir positive und negative. Jedes Beispiel ist ein Datensatz, der eine Zeiteinheit konzeptionell beschreiben und abstrahiert die Geschäftsbedingungen bis diese Zeiteinheit Feature engineering mit Verlaufsdaten und anderen Datenquellen beschriebenen Anlage gehört. Im Kontext des binären Klassifizierung für die vorbeugende Wartung für positive Typ Fehler (Bezeichnung 1) und negative Typ für Normalbetrieb (Label = 0), Etiketten sind kategorischen. Das Ziel ist ein Modell, das identifiziert jedes neues Beispiel wahrscheinlich oder ordnungsgemäß innerhalb der nächsten X Zeiteinheiten.

#### <a name="label-construction"></a>Label-Konstruktion
Erstellung ein Vorhersagemodells zum Beantworten der Frage "Was die Wahrscheinlichkeit ist die Anlage in den nächsten X Zeiteinheiten?", Kennzeichnung erfolgt durch Aufnahme X Datensätze vor dem Ausfall einer Anlage und sie als "dabei, Fehler" (Label = 1) und alle Datensätze als "normal" bezeichnen (Label = 0). In dieser Methode sind Etiketten kategorischen Variablen (siehe Abbildung 3).

![Abbildung 3. Kennzeichnung für binäre Klassifizierung](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-binary-classification.png)

Abbildung 3. Kennzeichnung für binäre Klassifizierung

Für flugverspätungen und absagen wird X vorherzusagen verzögert in den nächsten 24 Stunden ein Tag ausgewählt. Alle Flüge, die innerhalb von 24 Stunden vor dem Fehler als 1 s gekennzeichnet wurden. Für ATM Bargeld verzichten Fehler, zwei binäre Klassifizierung erstellt wurden, die Wahrscheinlichkeit Fehler in den nächsten 10 Minuten eine Transaktion vorhergesagt und die Wahrscheinlichkeit für den Ausfall in den nächsten 100 verzichtet vorherzusagen. Alle Transaktionen, die innerhalb der letzten 10 Minuten des Fehlers sind 1 für das erste Modell gekennzeichnet. Und alle Notizen in die letzten 100 Notizen Fehler ausgegeben wurden 1 für das zweite Modell. Schutzschalter Fehler obliegt die Wahrscheinlichkeit der nächste Schutzschalter Befehl fehlschlägt Vorhersagen in dem Fall X eine zukünftige Befehl möchten. Zug Tür Fehler wurde das binäre klassifizierungsmodell Fehler innerhalb der nächsten 7 Tage Vorhersagen erstellt. Wind Turbine Fehler wurde X 3 Monate gewählt.

Wind Turbine und Schulen Tür Fällen auch verwendet werden, für verbleibende Nutzungsdauer mit denselben Daten Vorhersagen Regressionsanalyse jedoch mit einer anderen Kennzeichnung Strategie die im nächsten Abschnitt erläutert.

### <a name="regression-for-predictive-maintenance"></a>Regression für die vorbeugende Wartung
Regressionsmodelle vorbeugende Wartung wird die Restnutzungsdauer (RUL) der Anlage berechnen definiert als der Zeitraum, der die Anlage betriebsbereit ist, bevor der nächste Fehler auftritt. Wie binäre Klassifizierung, jedem Beispiel ist ein Datensatz, der auf ein "Y" für eine Anlage gehört. Im Kontext von Regressionstests ist das Ziel zu einem Modell, das ist der Zeitraum vor der verbleibenden Restnutzungsdauer jedes neue Beispiel als fortlaufende Zahl berechnet. Wir nennen diesen Zeitraum mehrfachen von Y. Jedes Beispiel hat auch eine Restnutzungsdauer Zeit, beispielsweise vor der nächsten Messung berechnet werden kann.

#### <a name="label-construction"></a>Label-Konstruktion
Erhält der Frage "Was die Restnutzungsdauer des Geräts?
", Etiketten für jeden Datensatz vor dem Ausfall und beschriften sie berechnen, wie viele Einheiten der Zeit vor der nächsten bleiben das Regressionsmodell konstruiert werden kann. In dieser Methode sind Etiketten kontinuierlicher Variablen (siehe Abbildung 4).

![Abbildung 4. Kennzeichnung für regression](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-regression.png)

Abbildung 4. Kennzeichnung für regression

Anders als binäre Klassifizierung für Regression können ohne Fehler in den Daten verwendet werden für die Modellierung Kennzeichnung auf einem Ausfallpunkt erfolgt und die Berechnung ist nicht möglich, ohne zu wissen, wie lange die Anlage vor erhalten. Dieses Problem wird am besten durch einen anderen statistischen Verfahren Analyse aufgerufen behoben.
Wir sind nicht Analyse dieser Playbook wegen der Komplikationen behandelt, die bei der Technik, vorbeugende Wartung Anwendungsfälle, die zeitabhängige Daten regelmäßig betreffen auftreten können.

### <a name="multi-class-classification-for-predictive-maintenance"></a>Multi-Klasse Klassifikation für die vorbeugende Wartung
Multi-Klasse Klassifikation für die vorbeugende Wartung dienen zwei zukünftige Ergebnisse vorherzusagen. Die erste ist eine der mehrere Zeitdauer zu einen Fehler für jede Anlage eine Anlage zuweisen. Die zweite ist die Wahrscheinlichkeit des Fehlers in einer zukünftigen Periode eine mehrere Ursachen zu identifizieren. Wartungspersonal diese Kenntnisse verfügt, Probleme im Voraus behandeln können. Anderen Multi-Klasse modellierungstechnik konzentriert sich auf bestimmen die wahrscheinlichste Ursache für einen angegebenen Fehler. Dadurch werden Vorschläge zu der Instandhaltung Maßnahmen um Fehler zu beheben.
Durch eine Rangliste der Stamm verursacht und Reparatur Aktionen,  
Techniker können bei ihrer ersten Reparaturaktionen nach Fehler sein.

#### <a name="label-construction"></a>Label-Konstruktion
Angesichts der zwei Fragen sind "Was die Wahrscheinlichkeit ist eine Anlage in den nächsten"aZ"Zeiteinheiten"a"ist die Anzahl der Perioden" und "Was die Wahrscheinlichkeit ist die Anlage in den nächsten X Zeiteinheiten Problem" P<sub>i</sub>","i"ist die Anzahl der möglichen Ursachen beschriften wie folgt zu Verfahren erfolgt.

Für die erste Frage Kennzeichnung erfolgt aZ Datensätze vor dem Ausfall einer Anlage und beschriften sie mit Buckets Zeit (3Z, 2Z, Z) als ihre Etiketten beim bezeichnen alle Datensätze als "normal" (Label = 0). Bei dieser Methode ist die Bezeichnung kategoriale Variable (siehe Abbildung 5).

![Abbildung 5. Kennzeichnung für multiklassenklassifizierung Datenträgerausfällen Zeit](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-failure-time-prediction.png)

Abbildung 5. Kennzeichnung für multiklassenklassifizierung Datenträgerausfällen Zeit

Die zweite Frage Kennzeichnung erfolgt durch Aufnahme X Datensätze vor dem Ausfall einer Anlage und sie als "etwa um Fehler aufgrund Problem P<sub>i"</sub>(Label = P<sub>i</sub>) beim bezeichnen alle Datensätze als "normal" (Label = 0). In dieser Methode sind Etiketten kategorischen Variablen (siehe Abbildung 6).

![Abbildung 6. Kennzeichnung für multiklassenklassifizierung bei Root Ursache](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-root-cause-prediction.png)

Abbildung 6. Kennzeichnung für multiklassenklassifizierung bei Root Ursache

Das Modell weist eine Wahrscheinlichkeit Fehler durch jeden P<sub>i</sub> sowie die Wahrscheinlichkeit, dass kein Fehler. Diese Wahrscheinlichkeiten Größenordnung bestellen zu Vorhersage der Probleme, die wahrscheinlich in der Zukunft. Luftfahrzeuge Komponente Fehler Anwendungsfall wurde als ein multiklassenklassifizierung Problem strukturiert. Die Wahrscheinlichkeit des Fehlers durch zwei unterschiedliche Ventile innerhalb des nächsten Monats vorhersagen können.

Labortests Wartungsarbeiten nach Fehlern, Kennzeichnung nicht zukünftigen Horizont entnommen werden benötigt. Ist das Modell rechnet nicht Fehler in Zukunft aber die wahrscheinlichste Ursache wird nur auftreten, wenn der Fehler bereits geschehen. Aufzug Tür Fehler fallen in dritten Fall das Ziel ist die Fehlerursache angegeben Vergangenheitsdaten in Betrieb vorherzusagen. Dieses Modell wird dann zum Vorhersagen, die am wahrscheinlichsten Ursachen, nachdem ein Fehler aufgetreten. Ein wichtiger Vorteil dieses Modells ist, dass unerfahrene Techniker leicht diagnostizieren und Beheben von Problemen, die andernfalls Jahren viel Erfahrung erleichtert.

## <a name="training-validation-and-testing-methods-in-predictive-maintenance"></a>Training, Überprüfung und Testmethoden vorbeugende Wartung
Normale Training und Routine müssen zu verallgemeinern Zeit unterschiedliche Aspekte besser in vorbeugende Wartung, ähnlich einem anderen Lösung Space mit Zeitstempel Daten unsichtbaren zukünftige Daten.

### <a name="cross-validation"></a>Cross-Validierung
Viele Algorithmen für maschinelles lernen hängt eine Anzahl von hyperparameter, die Modell-Performance erheblich ändern können. Die optimalen Werte für diese hyperparameter werden beim Trainieren des Modells nicht automatisch berechnet jedoch datenwissenschaftler festzulegen. Es gibt verschiedene Arten der Werte der hyperparameter finden. Die häufigste ist "k gefaltete übergreifende Überprüfung" der Beispiele "k" Falten zufällig aufgeteilt. Für jeden hyperparameter Werte ist Learning Algorithmus zur k. Bei jeder Iteration Beispiele in der aktuellen als Überprüfung verwendet, der Rest der Beispiele als Schulung verwendet werden. Validierung Beispiele Züge Algorithmus das Training und Leistungsdaten berechnet. Am Ende dieser Schleife für jede Hyperparameter Werte Wir k metrischen Leistungswerte Durchschnitt berechnen und Hyperparameter Werte, die die durchschnittliche Leistung wählen.

Wie bereits erwähnt, vorbeugende Wartung Probleme werden Daten als eine Zeitreihe von Ereignissen, die aus mehreren Datenquellen stammen. Diese Einträge können je nach Beschriftung eines Datensatzes oder ein Beispiel bestellt werden. Wenn wir das Dataset zufällig an Schulung und Validierung Teilen, sind einige Beispiele Training daher zeitlich als Beispiele für die Validierung. Dies führt bei der Schätzung der zukünftigen Leistung Hyperparameter Werte basierend auf den Daten vor Modell ausgebildet wurde. Abschätzung wäre optimistisch, besonders Zeitreihen sind nicht und ihr Verhalten ändern. Daher beeinträchtigt möglicherweise ausgewählten Hyperparameter Werte.

Eine bessere Möglichkeit Werte des hyperparameter ist die Beispiele Schulung und Validierung zeitabhängige so aufgeteilt, Validierung Beispiele zeitlich als Trainingsbeispiele sind. Anschließend für jeden der Werte der hyperparameter Algorithmus über Trainingssatz Schulen, über dieselben Validierung des Modells messen und Hyperparameter Werte, die die beste Leistung anzeigen auswählen. Zeitreihen-Daten nicht als im Laufe der Zeit entwickelt Hyperparameter Werten von Schulen und Validierung Teilen zu eine bessere Zukunft "des Modells als mit den Werten von übergreifende Überprüfung nach dem Zufallsprinzip ausgewählt.

Das fertige Modell entsteht durch Schulung lernalgorithmus gesamten Daten mit der besten Hyperparameter, die mit Schulung und Validierung Teilen oder übergreifende Überprüfung gefunden werden.

### <a name="testing-for-model-performance"></a>Leistungstests Modell
Nach dem Erstellen eines Modells müssen die künftige Leistung auf neue Daten schätzen. Die einfachste Schätzung könnte die Leistung des Modells über die Trainingsdaten. Aber diese Schätzung ist optimistisch, Modell an Daten angepasst, mit der Leistung zu schätzen. Eine bessere Schätzung möglicherweise Leistungsmetrik Hyperparameter Werte über dem Satz Validierung berechnet oder durchschnittliche Leistungsmetrik aus übergreifende Überprüfung berechnet. Aber aus den gleichen Gründen wie bereits erwähnt, sind diese Einschätzung noch optimistisch. Wir benötigen realistischere Ansätze für Modell Leistung.

Eine Möglichkeit ist die Daten nach dem Zufallsprinzip Training, Überprüfung und Test legt aufgeteilt. Schulung und Validierung legt zum Werte von hyperparameter und Trainieren Sie das Modell mit. Die Leistung des Modells wird über das Prüfgerät gemessen.

Anders die vorbeugende Wartung ist die Beispiele Training, Überprüfung und Test legt zeitabhängige so aufgeteilt alle Test zeitlich als Beispiele Schulung und Validierung gehören. Nach der Teilung Modell generieren und Performance-Messung erfolgt wie oben beschrieben.

Zeitreihen stationären vorherzusagen sind erzeugen beide Ansätze ähnliche Abschätzung der zukünftigen Leistung. Aber wenn Zeitreihen nicht stationären oder schwer vorherzusagen sind, generiert der zweite realistischere Schätzung der zukünftigen Leistung als die erste.

### <a name="time-dependent-split"></a>Zeitabhängige Teilen
Es wird empfohlen genauer in diesem Abschnitt wir zeitabhängige Teilen Implementierung. Erläutert eine Teilung der zeitabhängigen bidirektionale Training und testen, aber genau dasselbe für zeitabhängige für die Schulung und Validierung legt anzuwenden.

Nehmen Sie wir einen Stream von Ereignissen wie verschiedene Sensoren Zeitstempel. Features von Schulung und Beispiele für das Testen sowie ihre Etiketten werden über Zeiträume definiert, die mehrere Ereignisse enthalten.
Z. B. binäre Klassifizierung wie beschrieben in Feature Engineering und Modellierung Techniken Funktionen entstehen basierend auf vergangenen Ereignissen und Etiketten werden basierend auf zukünftige Ereignisse in "X" Einheiten in Zukunft erstellt. Daher kommt der Kennzeichnung Zeitrahmen Beispiel später Zeitrahmen Funktionen. Für zeitabhängige Teilen holen wir einen Zeitpunkt, an dem wir eine Modell mit optimierter hyperparameter mit Verlaufsdaten bis zu schulen, die zeigen. Um zukünftigen Etiketten auslaufen, die über Schulung Grenzwert in Daten, wählen wir den neuesten Zeitrahmen Bezeichnung Schulung Beispiele x vor dem Stichtag Training. In Abbildung 7 repräsentiert jede ausgefüllter Kreis eine Zeile in der endgültigen Daten Featuresatz für die Funktionen und Etiketten nach der oben beschriebenen Methode berechnet. Da zeigt die Abbildung die Datensätze, die Ausbildung und Prüfung legt beim Implementieren von zeitabhängigen Split X = 2 und W = 3 gehen soll:

![Abbildung 7. Abhängigkeit von der Zeit für die binäre Klassifizierung](media/cortana-analytics-playbook-predictive-maintenance/time-dependent-split-for-binary-classification.png)

Abbildung 7. Abhängigkeit von der Zeit für die binäre Klassifizierung

Grüne Quadrate dargestellt Datensätze für die Zeiteinheiten für die Schulung verwendet werden können. Wie bereits erwähnt, wird jedes Training wird der letzte Feature-Tabelle anhand vergangenen 3 Perioden Feature generiert und 2 zukünftige Perioden zum Beschriften von vor Training Tag abgeschnitten. Wir verwenden Beispiele in die Trainingsmenge wird Teil 2 zukünftigen Perioden, beispielsweise über dem Grenzwert Training da wir davon ausgehen, dass wir nicht über dem Grenzwert Training Sichtbarkeit. Aufgrund dieser Einschränkung sind Schwarz Beispiele die Datensätze des endgültigen bezeichneten Dataset, der nicht in der Trainingsmenge Daten verwendet werden soll. Diese Datensätze nicht verwendet werden, entweder Testdaten sind Training abgeschnitten und deren Kennzeichnung Zeitrahmen hängt teilweise Schulung Zeitrahmen das nicht der Fall sein sollte, wie wir Kennzeichnung Zeitrahmen für Schulung und Testen zum Verhindern von Datenverlust Bezeichnung vollständig getrennt.

Diese Technik ermöglicht Überlappung in der Funktion Erzeugung zwischen training und Testen der Beispiele in der Nähe der Schulung abgeschnitten. Verfügbarkeit der Daten eine noch stärkere Trennung erfolgt mithilfe nicht die Beispiele in den Test in W Einheiten Training abgeschnitten.

Unsere Arbeit fanden wir regressionsmodelle für Vorhersagen verbleibende Nutzungsdauer mehr von Datenlecks Problem betroffen sind und mit einer zufälligen Teilung extreme überanpassung. Ebenso sollte Regressionsprobleme, die Aufteilung, Datensätze mit Fehler vor abgeschnitten zur verwendet werden soll, für die Schulung und Ressourcen, die Fehler nach dem Grenzwert für Testsatz verwendet werden soll.

Als allgemeine Methode ist eine weitere wichtige Sicherheitstechnik für Aufteilen der Daten zum Trainieren und testen eine Teilung von Aktiva-ID verwenden, damit keine Schulung verwendeten Ressourcen zum Testen, da der Test soll sicherstellen, dass wenn eine neue Anlage zu Vorhersagen verwendet wird, das Modell realistische Ergebnisse bietet verwendet.

### <a name="handling-imbalanced-data"></a>Daten zur Behandlung von Arbeitsvorgänge
Klassifizierung Probleme gibt es weitere Beispiele für eine Klasse als die anderen Daten soll Arbeitsvorgänge. Idealerweise möchten wir genügend vertreten jeder Klasse in den Trainingsdaten zu verschiedenen Klassen unterscheiden. Wenn eine Klasse weniger als 10 % der Daten, können wir sagen Arbeitsvorgänge werden und wir die Minderheit unterrepräsentiert Dataset-Klasse. In vielen Fällen finden wir, Arbeitsvorgänge Datasets, einer stark unterrepräsentiert ist, im Vergleich zu anderen beispielsweise nur 0,001 % der Datenpunkte bilden. Klasse Ungleichgewicht liegt ein Problem in vielen Bereichen einschließlich Erkennung, eindringen und vorbeugende Wartung Fehler in der Regel selten in der Lebensdauer der Vermögenswerte sind Beispiele Klasse Minderheit machen.

Bei Klasse Ungleichgewicht leidet Performance standard Learning Algorithmen als Ziel die Gesamtrate Fehler minimieren. Zum Beispiel erhalten ein Dataset mit negativen Klasse 99 % und 1 % positive Klasse Beispiele wir 99 % Genauigkeit einfach Beschriften aller Instanzen als negativ. Jedoch fälschlicherweise dies alle Beispiele, damit der Algorithmus nicht nützlich ist, obwohl Genauigkeit Metrik sehr hoch ist. Folglich reichen konventionellen Bewertung Metriken wie Genauigkeit für Fehlerrate nicht bei Arbeitsvorgänge lernen. Andere Kriterien Genauigkeit und Rückruf, F1-Faktoren Kosten angepasste ROC Kurven für Produkttests bei Arbeitsvorgänge Datasets im Abschnitt Bewertung Metriken erläutert verwendet werden.

Es gibt jedoch einige Methoden, die Abhilfe Klasse Ungleichgewicht Problem. Die zwei wichtigsten Techniken Probenahme und Kosten vertrauliche lernen.

#### <a name="sampling-methods"></a>Probenahmeverfahren
Die Verwendung von Stichprobenverfahren Arbeitsvorgänge lernen besteht Änderung des Datasets durch Mechanismen um eine ausgewogene Dataset bereitstellen. Zwar viele verschiedene Probenahmetechniken, die meisten Geradlinig sind zufällige oversampling und unter sampling.

Einfach gesagt, Zufallszahlen erzeugen beim Auswählen einer Stichprobe aus Minderheit Klasse, repliziert diese Beispiele Trainingsdataset hinzufügen. Dies erhöht die Anzahl der insgesamt Beispiele Minderheit Klasse und schließlich Saldo die Anzahl der verschiedenen Klassen. Eine ist übersampelt, dass mehrere Instanzen bestimmter Beispiele Klassifizierung zu zu bestimmten zu überanpassung führen können.
Dadurch würde Training hohe Genauigkeit kann auf unsichtbaren Testdaten jedoch sehr schlecht. Dagegen ist zufällig unter Sampling eine Stichprobe Mehrheit Klasse und Trainingsdataset Beispiele entfernen auswählen. Allerdings kann Mehrheit Klasse Beispiele entfernt Klassifizierung wichtige Konzepte der meisten Versicherungszweigs übersehen. Hybride Probenahme, Minderheit Klasse überreferenziert meisten Klasse unter abgetastet gleichzeitig ist eine andere geeignete Ansatz. Gibt es andere anspruchsvolleren Probenahmetechniken verfügbar und wirksame Probenahmeverfahren für Klasse Ungleichgewicht ist eine beliebte Gebiet viele Kanäle ständige Aufmerksamkeit und Beiträge erhalten. Verschiedene Techniken zu entscheiden, die effektivste bleibt normalerweise Daten Wissenschaftler und experimentieren und hängt die Eigenschaften sind. Außerdem ist es wichtig, um sicherzustellen, dass nur auf Methoden angewendet werden die Schulung festlegen jedoch nicht testen.

#### <a name="cost-sensitive-learning"></a>Kosten vertrauliche lernen
In vorbeugende Wartung Fehler Minderheit Klasse bilden interessanter als normale Beispiele und daher konzentriert sich auf die Leistung des Algorithmus auf Fehler steht in der Regel. Dies wird gemeinhin als ungleich Verlust oder asymmetrische Kosten sicherzustellen Elemente verschiedener Klassen, wobei Vorhersage falsch Positive wie negative Kosten, über umgekehrt. Die gewünschte Klassifizierung sollte zu hohe vorhersagegenauigkeit Klasse Minderheit ohne stark auf die Genauigkeit der meisten Klasse.

Dies kann auf verschiedene Weise gibt. Fehlklassifikation der Minderheit Klasse hohe Kosten zuweisen und die Gesamtkosten zu minimieren versucht das Problem ungleich verliert können effektiv behandelt werden. Einige Algorithmen für maschinelles lernen verwenden diese Idee grundsätzlich wie SVMs (Vektor Maschinen), positive und negative Beispiele Kosten während Schulung integriert werden. Ebenso werden Steigerung Methoden und normalerweise gute Leistung bei Arbeitsvorgänge Daten wie verstärkt Entscheidung Algorithmen Struktur anzeigen.

## <a name="evaluation-metrics"></a>Bewertung Metriken
Wie bereits erwähnt, wird Klasse Ungleichgewicht Leistungseinbußen wie Algorithmen klassifizieren Mehrheit Beispiele für Benutzerklassen besser Kosten der Minderheit Klasse Fälle insgesamt fehlklassifikation Fehler Mehrheit Klasse ordnungsgemäß gekennzeichnet ist wesentlich verbessert. Dadurch niedrig Rückruf Preise und Kosten von Fehlalarmen Unternehmen sehr hoch ist ein größeres Problem wird. Genauigkeit ist die beliebteste verwendet eine Klassifizierung Leistung. Aber wie bereits dargelegt Genauigkeit ineffektiv und nicht die tatsächliche Leistung einer Klassifizierung Funktionen wie sehr empfindlich auf Daten. Stattdessen dienen Bewertung Metriken Arbeitsvorgänge lernprobleme bewerten. In diesen Fällen sollte Genauigkeit, Rückruf und F1-Faktoren der anfänglichen Metriken sich bei vorbeugende Wartung modellleistung. Vorbeugende Wartung bezeichnen Rückruf Sätze wie viele Fehler in den Test vom Modell erkannt wurden. Rückruf höher bedeutet, dass das Modell erfolgreich true Fehler abgefangen. Precision Metrik bezieht sich auf die Rate von Fehlalarmen, niedrigere Genauigkeit höher Fehlalarm entsprechen. F1-Bewertung berücksichtigt Genauigkeit und der Rückruf mit besten 1 und am schlechtesten 0.

Darüber hinaus sind für binäre Klassifizierung und dezil Tabellen Aufzug Diagramme sehr informativ beim Auswerten der Leistung. Sie betreffen die positive Klasse (Fehler) und ein komplexeres Bild der Algorithmus Leistung als gesehen ist nur ein Betriebssystem Fixpunkt auf der Kurve ROC (Receiver Operating Characteristic) ansehen.
Dezil Tabellen Test Beispiele nach ihrer prognostizierten Wahrscheinlichkeiten Fehler berechnet das Modell vor dem Schwellenwert entscheiden auf der letzten Bestellung erhalten. Geordnete Beispiele sind dann im dezile (d. h. mit der größten Wahrscheinlichkeit und dann 20 %, 30 % usw. Beispiele 10 %) gruppiert. Berechnen Sie das Verhältnis zwischen true positive jedes dezil und seine zufällige Grundlinie (z. B. 0.1, 0.2) kann eine schätzen, Algorithmus Leistung wie bei jeder dezil ändert. Heben Sie Diagramme dienen dezil Werte darstellen, indem Sie true positive Rate dezil gegen zufällige true-positive Rate-Paare für alle dezile zeichnen. Normalerweise sind die ersten dezile sich die Ergebnisse, da wir die größten Gewinne sehen. Erste dezile wird auch als für "Risiko" für vorbeugende Wartung angezeigt.

## <a name="sample-solution-architecture"></a>Beispiel-Lösungsarchitektur
Beim Bereitstellen einer wartungslösung für die vorbeugende interessieren wir eine durchgehende Lösung Prozessfluss Daten Erfassung, Speicherung Modelltraining, Funktion generiert, Vorhersage und Visualisierung der Ergebnisse mit einer Warnung zum Generieren von wie Vermögenswert Dashboard überwachen. Wir möchten eine Datenpipeline, die zukünftige Einblicke in automatisierte fortlaufend für den Benutzer bereitstellt. Eine Beispiel vorbeugende Wartung Architektur für solche ein IoT Datenpipeline ist in Abbildung 8 dargestellt. In der Architektur werden in Echtzeit Telemetrie in einem Ereignis der Daten gespeichert. Dieser Daten werden vom Stream Analytics für Echtzeit-Verarbeitung von Daten wie Feature aufgenommen. Funktionen werden verwendet, um den Vorhersagemodell-Webdienst aufrufen und Ergebnisse werden auf dem Dashboard angezeigt. Zur gleichen Zeit aufgenommene Daten ebenfalls eine Verlaufsdatenbank gespeichert und mit externen Datenquellen wie lokale zusammengeführt zu Schulung Beispiele für die Modellierung erstellen.
Dieselbe Datawarehouses können Sie zum Stapel Beispiele bewerten und Speichern der Ergebnisse die erneut zu vorhersehbaren Berichte auf dem Dashboard verwendet werden kann.

![Abbildung 8. Beispiel-Lösungsarchitektur für die vorbeugende Wartung](media/cortana-analytics-playbook-predictive-maintenance/example-solution-architecture-for-predictive-maintenance.png)

Abbildung 8. Beispiel-Lösungsarchitektur für die vorbeugende Wartung

Weitere Informationen zu den einzelnen Komponenten der Architektur finden Sie in [Azure](https://azure.microsoft.com/) -Dokumentation.
