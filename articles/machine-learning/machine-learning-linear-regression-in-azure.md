<properties 
    pageTitle="Lineare Regression Computer Lernen mit | Microsoft Azure" 
    description="Einen Vergleich der linearen regressionsmodelle in Excel und Azure Machine Learning Studio" 
    metaKeywords="" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="kbaroni;garye" />

# <a name="using-linear-regression-in-azure-machine-learning"></a>Linearen Regression verwenden in Azure Machine Learning

> *Kate Baroni* und *Peter Boatman* sind Enterprise Lösungsarchitekten in Microsoft Daten Einblicke Center of Excellence. In diesem Artikel beschreiben sie ihre Erfahrung eine vorhandene Regression Analysis Suite eine cloudbasierte Lösung mit Azure Machine Learning migrieren.  
 
&nbsp; 
  
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  
 
## <a name="goal"></a>Ziel

Unser Projekt mit zwei Ziele vor Augen:  

1. Verwenden Sie Vorhersageanalytik zur Verbesserung der Genauigkeit unserer Organisation monatlichen einnahmenprojektionen  
2. Verwenden von Azure ML bestätigen optimieren, Beschleunigung, und der Ergebnisse.  

Wie viele Unternehmen durchläuft eine monatliche Einnahmen Prognoseprozess unserer Organisation. Unser kleine Team Geschäftsanalysten wurde mit Computerlernen zu unterstützen die Planung Genauigkeit beauftragt.  Das Team Monate mehrere Sammeln von Daten aus mehreren Quellen und die Attribute durch statistische Analyse Services Umsatzprognose für wichtige Attribute identifiziert.  Nächste Schritte wurde Prototypentwicklung statistischen regressionsmodelle auf die Daten in Excel zu starten.  Innerhalb weniger Wochen haben wir eine Excel Regressionsmodell, die die aktuellen und Prognose Prozesse Finanzen übertroffen wurde. Geplante Vorhersageergebnis wurde.  


Wir haben dann Nächstes Übergang unserer Vorhersageanalysen Azure ml herausfinden, wie Azure ML vorhersehbare Performance verbessern konnte.


## <a name="achieving-predictive-performance-parity"></a>Vorhersehbare Performance Parity erreichen

Vordergrund war zwischen Azure ML und Excel regressionsmodelle.  Wenn genauen derselben Daten und dieselbe Aufteilung für Schulung und Testen von Daten predictive Leistung Parität zwischen Excel und Azure ML erreicht.   Zunächst haben wir. Excel-Objektmodell hat Azure ML-Modell.   Der Fehler wurde aufgrund mangelnden Verständnis der Basis Tool Einstellung in Azure ML. Nach einer Synchronisierung mit dem Produktteam Azure ML wir erhalten Einblick in die Basis für unsere Datensätze festlegen, und zwischen den beiden Modellen.  

### <a name="create-regression-model-in-excel"></a>Regressionsmodell in Excel erstellen
Unsere Excel Regression verwendet das standardmäßige lineare Regressionsmodell in Excel Analyse-Funktionen. 

*Meine Absolute % Fehler* berechnet, und als das Leistungsmeasure für das Modell verwendet.  Es dauerte 3 Monate mit Excel Arbeitsmodell ankommen.  Wir haben viel lernen in Azure ML Experiment letztendlich Verständnis Anforderungen hilfreich war.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Vergleichbare Experiment in Azure Machine Learning erstellen  
Wir folgen Experiment in Azure ML zu erstellen:  

1.  Hochgeladen von Dataset als CSV-Datei in Azure ML (kleine Datei)
2.  Erstellt einen neuen Test und [Spalten im Dataset auswählen] [ select-columns] Modul auf der gleichen Datenfunktionen in Excel   
3.  Die [Aufgeteilten Daten] [ split] (mit *Relativen Ausdruck* Modus) genau gleichen Züge die Daten aufgeteilt werden, wie in Excel  
4.  Die [Lineare Regression] experimentiert[ linear-regression] Modul (nur Standardoptionen), dokumentiert und Excel Regression Modell verglichen

### <a name="review-initial-results"></a>Ergebnisse überprüfen
Zunächst übertroffen Excel-Objektmodell deutlich Azure ML-Modell:  

|   |Excel|Azure ML|
|---|:---:|:---:|
|Leistung|   |  |
|<ul style="list-style-type: none;"><li>R-Quadrat angepasst</li></ul>| 0,96 |N/A|
|<ul style="list-style-type: none;"><li>Bestimmtheitsmaß <br />Bestimmung</li></ul>|N/A|   0,78<br />(Genauigkeit)|
|Meine Absolute Fehler |  $9. 5M|  19.4 $ M|
|Meine Absolute Error (%)|   6.03 %|  12,2 %

Wir unser Verfahren und unsere Ergebnisse von Entwicklern und Datenanalysten Azure ML-Team hat sie schnell einige nützliche Tipps bereitgestellt.  

* Bei Verwendung der [Linearen Regression] [ linear-regression] Modul in Azure ML zwei Methoden werden bereitgestellt:
    *  Online Farbverlauf Abstieg: Möglicherweise besser geeignet für größere Probleme
    *  Normale Fehlerquadrate: Dies ist die Methode, der denkt hört linearen Regression. Für kleine Datasets können normale Fehlerquadrate eine optimale Wahl sein.
*  Betrachten Sie Parameters L2 Regularisierung Gewicht zur Verbesserung der Leistung optimieren. Festlegen 0,001 standardmäßig und unser kleines Dataset, wir 0,005 Leistung festgelegt.    

### <a name="mystery-solved"></a>Das Rätsel gelöst.
Wenn wir die Empfehlung angewendet, haben wir dieselbe Baseline Performance in Azure ML mit Excel:   

|| Excel|Azure ML (erste)|Azure ML mit kleinsten Quadrate|
|---|:---:|:---:|:---:|
|Mit der Bezeichnung Wert  |Aktuelle Werte (numerisch)|gleiche|gleiche|
|Teilnehmern  |Excel-Daten > Analyse -> Regression|Lineare Regression.|Lineare Regression|
|Teilnehmern Optionen|N/A|Standardeinstellungen|normale kleinsten Quadrate<br />L2 = 0,005|
|Daten|26 Zeilen 3 Merkmale, 1 Beschriftung.   Alle numerischen.|gleiche|gleiche|
|Split: Zug|Excel trainiert für die ersten 18 Zeilen in den letzten 8 Zeilen geprüft.|gleiche|gleiche|
|Split: Test|Excel Regressionsformel auf die letzten 8 Zeilen angewendet|gleiche|gleiche|
|**Leistung**||||
|R-Quadrat angepasst|0,96|N/A||
|Bestimmtheitsmaß|N/A|0,78|0.952049|
|Meine Absolute Fehler |$9. 5M|19.4 $ M|$9. 5M|
|Meine Absolute Error (%)|<span style="background-color: 00FF00;">6.03 %</span>|12,2 %|<span style="background-color: 00FF00;">6.03 %</span>|

Darüber hinaus gegenüber Koeffizienten Excel auch Gewichte Feature in Azure ausgebildeten Modell:

||Excel-Koeffizienten|Gewicht der Azure-Funktion|
|---|:---:|:---:|
|Intercept-Verschiebung|19470209.88|19328500|
|Funktion A|0.832653063|0.834156|
|Funktion B|11071967.08|11007300|
|Funktion C|25383318.09|25140800|

## <a name="next-steps"></a>Nächste Schritte

Wir möchten Azure ML-Webdienst in Excel verwenden.  Unsere Business Analysten auf Excel und mussten wir einen Weg Aufrufen des Webdienstes Azure ML mit Excel-Daten und den vorhergesagten Wert an Excel zurückgeben.   

Wir möchten auch unser Modell optimieren mit den Optionen in Azure ML verfügbaren Algorithmen.

### <a name="integration-with-excel"></a>Integration mit Excel
Unsere Lösung wurde unsere Azure ML Regressionsmodell durch Erstellen eines Webdiensts ausgebildeten Modell durchsetzen.  Innerhalb weniger Minuten der Webdienst erstellt wurde, und konnte nennen wir direkt in Excel prognostizierten Einnahmen zurück.    

*Web Services Dashboard* finden eine downloadbare Excel-Arbeitsmappe.  Die Arbeitsmappe wird mit dem Web Service-API und Schema eingebettet vorformatiert.   Wenn Sie auf *Excel-Arbeitsmappe downloaden*öffnen und auf Ihrem lokalen Computer speichern.    

![][1]
 
Kopieren Sie mit der geöffneten Arbeitsmappe vordefinierten Parameter in Blau Abschnitt Parameter wie folgt.  Eingabe der Parameter Excel Ruft den AzureML-Webdienst und die vorhergesagte erzielte Zeigt Etiketten im Abschnitt vorhergesagte Werte grün.  Die Arbeitsmappe weiterhin Vorhersagen für geschulte Modell für alle Zeile unter Parameter eingegebenen Parameter erstellen.   Weitere Informationen zur Verwendung dieser Funktion finden Sie unter [Nutzung einer Azure Machine Learning Webdienstes von Excel aus](machine-learning-consuming-from-excel.md). 

![][2]
 
### <a name="optimization-and-further-experiments"></a>Optimierung und weitere Versuche
Nun, wir einen Basisplan mit Excel-Objektmodell hatten zogen wir weiter optimieren unsere Azure ML linearen Regressionsmodell.  Verwendet das Modul [filterbasierte Featureauswahl] [ filter-based-feature-selection] auf Auswahl der Ausgangsdaten Elemente und half uns eine Leistungssteigerung von 4,6 % erreichen Absolute Fehler bedeuten.   Für zukünftige Projekte verwenden wir diese Funktion die Wochen Rettung konnte in Attribute des richtigen Satzes an Funktionen für die Modellierung verwendet zu durchlaufen.  

Als Nächstes wollen wir zusätzliche Algorithmen wie [Bayes] [ bayesian-linear-regression] oder [Entscheidungsstrukturen verstärkt] [ boosted-decision-tree-regression] im Experiment zu vergleichen.    

Wenn Sie Regression experimentieren möchten, ist ein gute Dataset versuchen numerische Attribute viele Energie-Effizienz Regression-Dataset Sample. Das Dataset wird als Teil des Datasets Probe in ML Studio bereitgestellt.  Zahlreiche Module lernen können Heizung laden oder Kühlung laden vorherzusagen.  In der folgenden Tabelle ist erfährt ein Leistungsvergleich der unterschiedlichen Regression gegen Energieeffizienz Dataset Vorhersage für die Zielvariable Kühlung laden: 

|Modell|Meine Absolute Fehler|Root-Mean Squared Fehler|Relativen Absolute Fehler|Relativer Wert Fehler|Bestimmtheitsmaß
|---|---|---|---|---|---
|Stärkere Entscheidungsstruktur|0.930113|1.4239|0.106647|0.021662|0.978338
|Lineare Regression (Gradientenabstieg)|2.035693|2.98006|0.233414|0.094881|0.905119
|Neuronales Netzwerk Regression|1.548195|2.114617|0.177517|0.047774|0.952226
|Lineare Regression (normale Least Squares)|1.428273|1.984461|0.163767|0.042074|0.957926  

## <a name="key-takeaways"></a>Hauptargumente 

Wir viel gelernt von laufenden Excel Regression und Azure Machine Learning Experimente parallel. Das ursprüngliche Modell in Excel erstellen und Vergleichen mit den Modellen mit Azure ML [Lineare Regression] [ linear-regression] half uns erfahren Azure ML und entdeckten wir Verkaufschancen Daten Auswahl und das Modell Leistung.         

Wir festgestellt, dass es ratsam [filterbasierte Featureauswahl] [ filter-based-feature-selection] Vorhersagen zukünftigen Projekte beschleunigen.  Featureauswahl auf Ihre Daten anwenden, können Sie ein verbessertes Modell in Azure ML mit Erhöhung der Leistung erstellen. 

Die Fähigkeit, vorbeugende analytische Prognosen von Azure Excel systemisch ermöglicht deutlich in Ergebnisse erfolgreich einem breiten Benutzer Publikum bereitstellen.     


## <a name="resources"></a>Ressourcen
Helfen mit Regression arbeiten, sind einige Ressourcen aufgeführt:  

* Regression in Excel.  Wenn Sie nie Regression in Excel versucht, dieses Lernprogramm erleichtert: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Regression Vs Prognosen.  Tyler Diskussion hat einen Blogartikel Zeitreihen in Excel enthält eine gute Anfänger Beschreibung der linearen Regression Planung erläutert. [http://sqlmag.com/SQL-Server-Analysis-Services/Understanding-Time-Series-Forecasting-Concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts)  
*   Normale am wenigsten linearen Regression: Fehler, Probleme und fallen.  Eine Einführung und Beschreibung der Regression: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ )

[1]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
