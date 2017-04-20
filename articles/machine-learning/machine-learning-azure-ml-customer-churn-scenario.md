<properties
    pageTitle="Kunden profitieren mit Machine Learning analysieren | Microsoft Azure"
    description="Fallstudie über ein integriertes Modell für die Analyse und Bewertung der kundenabwanderung"
    services="machine-learning"
    documentationCenter=""
    authors="jeannt"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="jeannt"/>

# <a name="analyzing-customer-churn-by-using-azure-machine-learning"></a>Analysieren von Azure Machine Learning mit Kunden profitieren

##<a name="overview"></a>Übersicht
Dieses Thema enthält eine referenzimplementierung eines kundenabwanderung Analyse, die mithilfe von Azure Machine Learning Studio erstellt. Es erläutert zugeordneten generischen Modelle für gewerbliche kundenabwanderung ganzheitlich Lösung. Messen wir auch die Genauigkeit von Modellen mit Computerlernen erstellt werden, und wir bewerten die Richtung für die Weiterentwicklung.  

### <a name="acknowledgements"></a>Danksagungen

Dieses Experiment entwickelt und getestet von Serge Berger Principal Data Scientist bei Microsoft und Roger Barga, früher Produktmanager für Microsoft Azure maschinelles lernen. Azure-Dokumentationsteam dankbar bestätigt ihre Fachkenntnisse und danke ihnen dieses White Paper.

>[AZURE.NOTE] Die Daten für dieses Experiment ist nicht öffentlich. Ein Beispiel zum Erstellen einer Maschine Lernmodell Abwanderung Analyse finden Sie unter: [Cortana Intelligence](http://gallery.cortanaintelligence.com/) Galerie [Telco Service Model-Vorlage](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5)


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="the-problem-of-customer-churn"></a>Das Problem der kundenabwanderung
Unternehmen, Verbraucher und Unternehmen branchenübergreifend müssen Abwanderung befassen. Manchmal Abwanderung ist sehr laut und Entscheidungen beeinflusst. Die herkömmliche Lösung ist hoch Neigung kündiger Vorhersagen und ihre Bedürfnisse über einen Concierge, Marketingkampagnen oder spezielle Ausnahmen. Diese Ansätze können aus Industrie und auch aus einem Cluster bestimmte Verbraucher innerhalb einer Branche (z. B. Telekommunikation) variieren.

Allgemein gilt, dass Unternehmen dabei Aufbewahrung kundenspezifischen minimieren müssen. Folglich wäre eine natürliche Methode jeder Kunde mit der Abwanderung und Top N zu beheben. Top-Kunden möglicherweise die Profitabelste; DB-Funktion wird in anspruchsvollere Szenarien bei der Auswahl von Kandidaten für besondere Befreiung eingesetzt. Diese Aspekte sind jedoch nur einen Teil der ganzheitliche Strategie Abwanderung. Unternehmen müssen auch in Konto Risiko und zugeordneten Risikotoleranz der und die Kosten der Intervention und plausibel kundensegmentierung.  

##<a name="industry-outlook-and-approaches"></a>Geschäftsaussichten und Ansätze
Ausgefeilte Handhabung von Abwanderung zeigt entwickelte Branche. Das klassische Beispiel hierfür ist die Telekommunikationsbranche, Abonnenten bekanntermaßen häufig von einem Anbieter wechseln. Diese freiwillige Abwanderung ist ein Hauptanliegen. Darüber hinaus haben Anbieter angesammelt beträchtliche Kenntnisse über *Treiber profitieren*, sind die Faktoren, Kunden wechseln.

Hörer oder Gerät Wahl ist ein bekannter Abwanderung der Mobiltelefon-Branche. Daher ist eine beliebte Richtlinie für neue Abonnenten und eine vollständige Preis Kunden für ein Upgrade den Preis eines Hörers subventionieren. Früher führte diese Richtlinie Kunden hopping von einem Anbieter zu einem anderen Anbieter ihre Strategien zu verfeinern, veranlasst einen neuen Rabatt abgerufen.

Erhöhte Volatilität Hörer Angebote ist ein Faktor, der sehr schnell Modelle der Abwanderung ungültig, die auf aktuellen Hörer Modelle. Darüber hinaus sind Mobiltelefone nicht nur Telekommunikationsgeräte Mode-Anweisungen (Beachten Sie das iPhone) sind, und diese sozialen vorausgesagt werden reguläre Telekommunikation Datensätze.

Das Ergebnis für die Modellierung ist, dass Sie sound Richtlinie entwickeln können nicht einfach durch Eliminierung von bekannten Gründen Abwanderung. Tatsächlich ist eine kontinuierliche Modellierungsstrategie klassische Modelle, die Quantifizierung kategorische Variablen (z. B. Entscheidungsbäume) **obligatorisch**.

Mit großen Datenmengen auf Kunden, durchführen Organisationen große Datenanalyse (insbesondere Abwanderung Erkennung basierend auf big Data) als eine effektive Lösung des Problems. Sie finden mehr Datenverlustvorfalls Ansatz für das Problem der Abwanderung in Recommendations ETL-Abschnitt.  

##<a name="methodology-to-model-customer-churn"></a>Methodik Modell kundenabwanderung
Eine allgemeine Problemlösungsprozess zu kundenabwanderung ist in Zahlen 1 bis 3 dargestellt:  

1.  Ein Risikomodell können Sie Einfluss Aktionen Wahrscheinlichkeit und Risiken berücksichtigen.
2.  Eine Intervention können Sie prüfen, wie die Ebene der Intervention die Wahrscheinlichkeit der Änderung und die Kunden auswirken Gültigkeitsdauer (CLV).
3.  Diese Analyse eignet sich für eine Qualitative Analyse, die eine proaktive Marketingkampagne eskaliert wird, die auf Kundensegmenten optimale Angebot übermitteln.  

![][1]

Dieser zukunftsorientierten Ansatz ist am besten Abwanderung behandelt, aber es kommt mit Komplexität: mit mehreren Archetyp und Trace Abhängigkeiten Modelle entwickeln. Die Interaktion zwischen Modellen kann gekapselt werden, wie im folgenden Diagramm dargestellt:  

![][2]

*Abbildung 4: Multi-Modell-Urtyp Unified*  

Interaktion zwischen den Modellen ist Schlüssel zu einem ganzheitlichen Ansatz auf Kundenpflege. Jedes Modell nimmt mit der Zeit unbedingt ab; Daher ist die Architektur eine implizite Schleife (ähnlich Urtyp festgelegten SCHARFE DM Data Mining Standard, [***3***]).  

Gesamte Zyklus der Risiko-Entscheidung-Marketing Segmentierung/Zerlegung bleibt eine allgemeine Struktur für viele geschäftliche Probleme. Abwanderung Analyse ist einfach eine starke Vertreter dieser Probleme, weil weist alle Merkmale eines komplexen Problems, die eine vereinfachte vorhersehbare Lösung nicht zulässt. Die sozialen Aspekten der modernen Ansatz profitieren Ansatz nicht besonders hervorgehoben werden, aber die sozialen Aspekten werden wie in jedem Modell in Urtyp Modellierung gekapselt.  

Eine interessante Ergänzung ist big Data Analytics. Telekommunikation und Einzelhandel heute umfassende Daten über ihre Kunden sammeln, und wir können einfach sehen müssen mit mehreren Konnektivität einen allgemeinen Trend Tendenzen wie das Internet der Dinge weit verbreitete Geräte, denen Unternehmen intelligente Lösungen auf mehreren Ebenen angegeben werden.  

 
##<a name="implementing-the-modeling-archetype-in-machine-learning-studio"></a>Implementieren des Modellierung archetyps in Machine Learning Studio
Wenn das beschriebene Problem, was ist am besten integrierte Modellierung und Bewertung Ansatz In diesem Abschnitt zeigen wir, wie wir dies mithilfe von Azure Machine Learning Studio.  

Der Multi-Modell Ansatz ist beim Entwerfen einer globalen Vorbild für Abwanderung. Auch der Punktzahl (predictive) Teil des Ansatzes sollte Multi-Modell.  

Das folgende Diagramm zeigt dem Prototyp erstellten beschäftigt vier Punktzahl Algorithmen in Machine Learning Studio Abwanderung vorherzusagen. Der Grund für die Verwendung mit mehreren Ansatz ist nicht nur zum Erstellen einer Klassifizierung Ensemble Genauigkeit, aber auch über Formstück Schutz und unterstützende Featureauswahl verbessern.  

![][3]

*Abbildung 5: Prototyp einer Abwanderung Modellierung*  

Die folgenden Abschnitte enthalten weitere Informationen zu den Prototyp mit Machine Learning Studio implementiert Modell wird bewertet.  

###<a name="data-selection-and-preparation"></a>Auswahl und Vorbereitung
Daten zu den Modellen und Bewertung Kunden wurde von einer vertikalen CRM-Lösung mit den Daten verborgen, um Kundendaten zu schützen. Die Daten enthalten Informationen über 8.000 Abonnements in den USA und drei Quellen kombiniert: Bereitstellen von Daten (Abonnement Metadaten), Daten (Nutzung des Systems) und Daten der Kunden-Support. Die Daten umfassen keine Unternehmen Informationen über die Kunden; Es umfasst beispielsweise nicht Loyalität Metadaten oder Gutschrift Faktoren.  

Der Einfachheit halber ETL und Reinigung Prozesse Daten sind Gültigkeitsbereich da wir davon ausgehen, dass alles bereits an anderer Stelle durchgeführt.   

Featureauswahl für die Modellierung basiert auf vorläufigen Bedeutung Bewertung festgelegten vorausgesagt verwendet zufällige Gesamtstruktur Modul enthalten. Für die Implementierung in Computer lernen berechnet wir Mittel, Median und Bereiche für repräsentative Features. Beispielsweise haben wir Aggregate für die qualitativen Daten wie minimalen und maximalen Werte für Benutzeraktivitäten.    

Es erfasst auch Zeitinformationen für die letzten sechs Monate. Wir analysieren Daten für ein Jahr und wir, selbst wenn es statistisch Trends, die Auswirkung auf Abwanderung erheblich nach sechs Monaten verringert wird.  

Der wichtigste Punkt ist, der gesamte Prozess, einschließlich ETL Featureauswahl und Modellierung in Machine Learning Studio mit Datenquellen in Microsoft Azure implementiert wurde.   

Die folgenden Diagramme veranschaulichen die Daten.  

![][4]

*Abbildung 6: Auszug aus einer Datenquelle (verborgen)*  

![][5]


*Abbildung 7: Funktionen, die aus einer Datenquelle extrahiert*
 
> Beachten Sie, dass diese Daten ist daher das Modell und die Daten können nicht freigegeben werden.
> Jedoch ein ähnliches Modell öffentlich verfügbare Daten verwenden, finden Sie in diesem Beispiel [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/)experimentieren: [Telco Kunden profitieren](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Weitere Informationen zum Implementieren einer Abwanderung Modell mit Cortana Intelligence Suite, empfehlen wir [dieses Video](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) leitender Programmmanager Wee Hyong Tok. 
> 

###<a name="algorithms-used-in-the-prototype"></a>Der Prototyp verwendeten Algorithmen.

Verwendet die folgenden vier Computer lernalgorithmen Prototyp (keine Anpassung) erstellen:  

1.  Logistische Regression (KR)
2.  Stärkere Entscheidungsstruktur (BT)
3.  Durchschnittliche Perceptron (AP)
4.  Vektor Maschine (SVM)  


Die folgende Abbildung zeigt einen Teil der Entwurfsoberfläche experimentieren, die die Reihenfolge angibt, in der Modelle erstellt wurden:  

![][6]  


*Abbildung 8: Erstellen von Modellen in Machine Learning Studio*  

###<a name="scoring-methods"></a>Bewertungsmethoden
Wir erzielten vier Modelle mit gekennzeichneten Trainings-Dataset.  

Wir übermittelt Bewertungsmuster Dataset ein vergleichbares Modell mit der Desktopversion von SAS Enterprise Miner 12 erstellt. Wir messen die Genauigkeit des Modells SAS und alle vier Machine Learning Studio Modelle.  

##<a name="results"></a>Ergebnisse
In diesem Abschnitt stellen wir unsere Erkenntnisse über die Genauigkeit von Modellen basierend auf scoring Dataset.  

###<a name="accuracy-and-precision-of-scoring"></a>Genauigkeit und Präzision der Bewertung
Im Allgemeinen ist die Implementierung in Azure Machine Learning hinter SAS Genauigkeit von ca. 10 bis 15 % (Bereich unter der Kurve oder AUC).  

Aber die wichtigste Metrik Abwanderung beträgt fehlklassifikation: also von der Top N abwanderer wie erwartet von der Klassifizierung, die tatsächlich **nicht** Abwanderung und noch besondere Behandlung? Das folgende Diagramm vergleicht diese falschen Klassifizierung für die Modelle:  

![][7]


*Abbildung 9: Passau Prototyp Fläche unter der Kurve*

###<a name="using-auc-to-compare-results"></a>Verwenden AUC zum Vergleich
Bereich unter Kurve (AUC) ist ein, das globale Messgrößen der *Trennbarkeit* zwischen die Verteilung der Punkte für positive und negative Populationen darstellt. Ähnelt dem herkömmlichen Receiver Operator Merkmal (ROC) Graph ein wichtiger Unterschied ist jedoch, dass AUC Metrik einen Schwellenwert auswählen nicht erforderlich. Stattdessen sind die Ergebnisse über **Alle** möglichen Optionen aufgeführt. Im Gegensatz dazu traditionelle ROC Diagramm positive Rate auf der vertikalen Achse und die false-positive-Rate auf der horizontalen Achse und Klassifizierung Schwellwert variiert.   

AUC im Allgemeinen als Maßeinheit Wert für verschiedene Algorithmen (oder anderen Systemen) verwendet, da Modelle durch die AUC-Werte verglichen werden können. Dies ist eine gängige Vorgehensweise in METEOROLOGIE und Biowissenschaften. AUC stellt daher ein Tool für die Bewertung der Klassifizierung.  

###<a name="comparing-misclassification-rates"></a>Fehlklassifikation Vergleich
Fehlklassifikation Preise für den betreffenden Dataset verglichen mithilfe der CRM ungefähr 8.000 Abonnements.  

-   SAS lag fehlklassifikation die 10-15 %.
-   Machine Learning Studio lag fehlklassifikation die 15-20 % der Top-200-300 kündiger.  

In der Telekommunikationsbranche ist es wichtig, nur die Kunden, die das höchste Risiko ein Concierge oder andere besondere Behandlung mit Service. In dieser Hinsicht erreicht Machine Learning Studio-Implementierung mit SAS-Modell.  

Genauigkeit ist ebenso, wichtiger Genauigkeit, da hauptsächlich interessiert potenziellen kündiger korrekt klassifizieren.  

Das folgende Diagramm von Wikipedia zeigt die Beziehung in einer Grafik lebendiger, einfach zu verstehen:  

![][8]

*Abbildung 10: Kompromiss zwischen Genauigkeit und Präzision*

###<a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Genauigkeit und Präzision Ergebnisse für stärkere Entscheidungsbaummodell  

Das folgende Diagramm zeigt die raw Ergebnisse mit dem Computerlernen Prototyp für die stärkere Entscheidungsbaummodell, die genaueste vier Modelle zu bewerten:  

![][9]

*Abbildung 11: Stärkere Entscheidung Struktur Modelleigenschaften*

##<a name="performance-comparison"></a>Performance-Vergleich
Die Geschwindigkeit mit der Daten Machine Learning Studio Modelle mit einer vergleichbaren Modell erzielt wurde mit desktop Edition von SAS Enterprise Miner 12.1 verglichen.  

Die folgende Tabelle zeigt die Leistung der Algorithmen:  

*Tabelle 1. Allgemeine Leistung (Genauigkeit) der Algorithmen*

| LR|BLUETOOTH|AP|SVM|
|---|---|---|---|
|Modell|Das geeignetste Modell|Volle Leistung|Modell|

Die Modelle in Machine Learning Studio übertroffen SAS 15-25 % auf Ausführung aber Genauigkeit hängt Nennwert gehostet.  

##<a name="discussion-and-recommendations"></a>Diskussion und Vorschläge
In der Telekommunikationsbranche entstanden mehrere Methoden zum Analysieren der Abwanderung, einschließlich:  

-   Leiten Sie Metriken für grundlegende Kategorien:
    -   **Entität (z. B. ein Abonnement)**. Bereitstellen Sie grundlegende Informationen zu Abonnements oder Abwanderung ist Kunden.
    -   **Aktivität**. Alle möglichen Verwendungsinformationen zur Entität, z. B. die Anzahl zu erhalten.
    -   **Customer support**. Sammeln Sie Informationen von Kunden-Support-Protokollen an, ob das Abonnement Probleme oder Interaktionen mit Kunden-Support.
    -   **Wettbewerbsfähig und Geschäftsdaten**. Alle Informationen über den Debitor beziehen (z. B. kann nicht verfügbar oder schwer zu verfolgen).
-   Verwenden Sie Bedeutung Featureauswahl Laufwerk. Dies bedeutet, dass die stärkere Entscheidungsbaummodell immer vielversprechender Ansatz.  

Die Verwendung dieser vier Kategorien Tiefenwirkung, die ein einfache *deterministisch* Ansatz Indizes auf angemessenen Faktoren pro Kategorie gebildet ausreichen sollte, um Kunden Risiko einer Änderung. Obwohl diese plausibel scheint, ist es leider ein falsches Verständnis. Deshalb Abwanderung ist eine zeitliche Wirkung und Faktoren um zu profitieren in Übergangszustände. Was Kunden verlassen heute führt möglicherweise morgen, und es wird sicherlich anderen sechs Monate ab. Daher eine *probabilistisch* ist eine Notwendigkeit.  

Diese wichtige Beobachtung wird häufig übersehen, in Business Analytics einen Business Intelligence-Ansatz in der Regel lieber, meist einfacher verkaufen und einfache Automatisierung gibt.  

Allerdings ist das Versprechen des Selfservice mit Machine Learning Studio sortiert nach Abteilung bzw. vier Informationskategorien wertvolle für computerlernen Abwanderung werden.  

Eine weitere interessante Funktion im Azure Machine Learning ist die Möglichkeit, ein benutzerdefiniertes Modul zum Repository vordefinierte Module hinzufügen, die bereits verfügbar sind. Diese Funktion erstellt im Wesentlichen Gelegenheit wählen Bibliotheken und Vorlagen für vertikale Märkte. Es ist ein wichtiges Unterscheidungsmerkmal von Azure maschinelles lernen im Markt.  

Wir hoffen, dieses Thema in Zukunft insbesondere in Bezug auf big Data Analytics.
  
##<a name="conclusion"></a>Abschluss
Dieses Dokument beschreibt einen vernünftigen Ansatz gegen das auftretende Problem der kundenabwanderung über generische Framework. Wir als Prototyp für Modelle bewerten und mithilfe von Azure Machine Learning implementiert. Schließlich untersuchten wir die Genauigkeit und Leistungsfähigkeit der prototyplösung für vergleichbare Algorithmen SAS.  

**Weitere Informationen:**  

Hilfreich dieser Artikel für Sie? Bitte geben Sie uns Feedback. Teilen Sie uns auf einer Skala von 1 (schlecht) bis 5 (hervorragend), wie bewerten Sie dieses Dokument und warum haben Sie es diese Bewertung? Zum Beispiel:  

-   Sind Sie es Beispiele, hervorragende Screenshots deaktivieren schreiben oder aus anderen Gründen haben hohe Bewertung?
-   Sind Sie es aufgrund schlechter Beispiele fuzzy Screenshots oder unklar schreiben Bewertung?  

Dieses Feedback hilft uns Whitepapers Verbesserung, die wir veröffentlichen.   

[Feedback senden](mailto:sqlfback@microsoft.com).
 
##<a name="references"></a>Referenzen
Vorhersageanalytik [1] : über Vorhersagen W. McKnight Informationsmanagement, Juli und August 2011, 18 bis 20.  

Wikipedia-Artikel [2] : [Genauigkeit](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [SCHARFE DM 1.0: schrittweise Data Mining Guide](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [big Data Marketing: lassen Sie Ihre Kunden effektiver und Laufwerk Wert](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco Service Model-Vorlage](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) in [Cortana Intelligence Galerie](http://gallery.cortanaintelligence.com/) 
 
##<a name="appendix"></a>Anhang

![][10]

*Abbildung 12: Snapshot einer Präsentation Abwanderung Prototyp*


[1]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-10.png
