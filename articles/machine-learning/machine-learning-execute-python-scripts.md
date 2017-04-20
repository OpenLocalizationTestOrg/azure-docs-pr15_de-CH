<properties 
    pageTitle="Python Computer lernen Skripts ausführen | Microsoft Azure" 
    description="Konturen Entwurfsprinzipien Unterstützung für Python-Skripten in Azure Machine Learning und grundlegende Szenarien, Funktionen und Grenzen." 
    keywords="Python-Computer lernen, Panda, Panda Python-Skripten Python-Skripts ausführen"
    services="machine-learning"
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>In Azure Machine Learning Studio-Computer lernen Skripten ausführen

Dieses Thema beschreibt die aktuelle Unterstützung für Python-Skripten in Azure Machine Learning Entwurfsprinzipien. Die wichtigsten Funktionen ebenfallls, einschließlich Unterstützung für das Importieren von vorhandenem Code exportieren Visualisierung und schließlich Grenzen und Arbeit diskutiert.

[Python](https://www.python.org/) ist ein unverzichtbares Tool im Werkzeugkasten vieler Daten Wissenschaftler. Es hat:

-  eine elegante und präzise syntax 
-  Plattformübergreifende Unterstützung 
-  eine Sammlung von leistungsstarken Bibliotheken und 
-  ausgereifte Entwicklungstools. 

Python wird in allen Phasen des Workflows Computer lernen Modellierung verwendet, Daten erfassen und Funktion Konstruktion Modell Schulung und Prüfung und Bereitstellung der Modelle verarbeiten. 

Azure Machine Learning Studio unterstützt Einbetten von Python-Skripten in verschiedene Teile eines Computers dies erlernen und nahtlos als skalierbaren, standardisierten Webdienste auf Microsoft Azure veröffentlichen.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Entwurfsprinzipien von Python-Skripten Computer lernen
Die primäre Schnittstelle Python in Azure Machine Learning Studio erfolgt das [Python-Skript ausführen] [ execute-python-script] Modul in Abbildung 1 dargestellt.

![Bild1](./media/machine-learning-execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![Bild2](./media/machine-learning-execute-python-scripts/embedded-machine-learning-python-script.png)

Abbildung 1. Das Modul **Python-Skript ausführen** .

[Python-Skript ausführen] [ execute-python-script] Modul bis zu drei Eingaben akzeptiert und erzeugt zwei Ausgaben (siehe unten), wie dessen analoge R, [R-Skript ausführen] [ execute-r-script] Modul. Python-Code ausgeführt werden als speziell benannte Parameterfeld eingegebenen Einstiegspunkt Funktion `azureml_main`. Hier sind zur Implementierung dieser Unterrichtseinheit Entwurfsprinzipien:

1.  *Muss idiomatische Python-Benutzer.* Die meisten Python-Benutzer Faktor Code Funktionen in Modulen, so viel von ausführbaren Anweisungen der obersten Ebene relativ selten ist. Das Skript hat daher auch einer speziell benannten pythonfunktion nur eine Sequenz von Anweisungen. Objekte in der Funktion verfügbar gemacht werden Python-Bibliothek Standardtypen wie [Panda](http://pandas.pydata.org/) Datenrahmen und [NumPy](http://www.numpy.org/) arrays
2.  *Müssen High-Fidelity zwischen lokalen und Ausführung.* Python-Code ausgeführt Backend basiert auf [Anaconda](https://store.continuum.io/cshop/anaconda/) 2.1 ist eine weit verbreitete Plattform-wissenschaftlichen Python Verteilung. Es kommt mit fast 200 der am häufigsten verwendeten Python-Pakete. Daher können datenwissenschaftler Debuggen und ihren Code auf ihrer Umgebung Azure Machine Learning-kompatible Anaconda qualifizieren. Können Sie vorhandene Umgebungen [IPython](http://ipython.org/) Notebook oder [Python-Tools für Visual Studio](http://aka.ms/ptvs) als Teil des Experiments Azure Machine Learning mit ausführen. Außerdem die `azureml_main` ist Vanille Python und ohne bestimmte Azure Machine Learning-Code erstellt werden oder das SDK installiert.
3.  *Muss nahtlos zusammensetzbare mit anderen Azure Machine Learning-Modulen.* [Python-Skript ausführen] [ execute-python-script] Modul akzeptiert Eingaben und Ausgaben standard Azure Machine Learning Datasets. Das zugrunde liegende Framework überbrückt transparent und effizient die Laufzeiten Azure maschinelles lernen und Python (unterstützt Funktionen wie fehlende Werte). Python kann daher in Verbindung mit vorhandenen Azure Machine Learning-Workflows, einschließlich R und SQLite Aufrufen verwendet werden. Man kann Workflows daher vorsehen, dass:
  * Verwenden Sie Python und Panda Daten vorverarbeitet und gereinigt, 
  * Datenfeed an eine SQL-Transformation mehrere Datasets Formularfunktionen beitreten, 
  * Modelle mit der Sammlung von Algorithmen in Azure Machine Learning Schulen und 
  * Auswerten und die Ergebnisse mit r Nachbearbeitung


## <a name="basic-usage-scenarios-in-machine-learning-for-python-scripts"></a>Grundlegende Verwendungsszenarien für maschinelles lernen Python-Skripts
In diesem Abschnitt Umfrage einige Verwendungszwecke von [Python-Skript ausführen] [ execute-python-script] Modul.
Wie bereits erwähnt, werden Python-Modul Eingaben als Panda Datenrahmen verfügbar gemacht. Weitere Informationen zu Panda Python und wie es verwendet werden, um Daten effizient bearbeiten *Python für die Datenanalyse* (O' Reilly, 2012) von W. McKinney entnehmen. Die Funktion muss Panda Daten Einzelbild verpackt in einer Python- [Sequenz](https://docs.python.org/2/c-api/sequence.html) wie Tupel, Liste oder NumPy Array zurück. In der ersten Ausgabe des Moduls wird das erste Element der Sequenz zurückgegeben. Dieses Schema wird in Abbildung 2 dargestellt.

![image3](./media/machine-learning-execute-python-scripts/map-of-python-script-inputs-outputs.png)

Abbildung 2. Zuordnung von input Ports, Parameter und Rückgabewerte Ausgang.

Detaillierte Semantik wie die Eingabeanschlüsse Parametern zugeordnet der `azureml_main` -Funktion sind in Tabelle 1 dargestellt:

![image1T](./media/machine-learning-execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Tabelle 1. Zuordnung der Eingänge an Funktionsparametern.

Die Zuordnung zwischen Eingabe- und Funktionsparameter ist positionsabhängig. Erste verbundene Eingang an den ersten Parameter der Funktion zugeordnet, und die zweite Eingabe (falls), des zweiten Parameters der Funktion zugeordnet.

## <a name="translation-of-input-and-output-types"></a>Übersetzung von Eingabe- und Typen
Wie bereits erwähnt, sind input Datasets in Azure Machine Learning Daten konvertiert Bilder in Panda und Ausgabe Datenrahmen in Azure Machine Learning Datasets konvertiert werden. Die folgenden Konvertierungen durchgeführt werden:

1.  Zeichenfolgen und numerische Spalten werden als konvertiert-ist und fehlende Werte in einem Dataset "NA" Werte Panda. Dieselbe Konvertierung erfolgt auf (NA Werte Panda sind fehlende Werte in Azure Machine Learning konvertiert).
2.  Panda Index Vektoren werden in Azure maschinelles lernen nicht unterstützt. Alle Eingabedaten Frames in Python-Funktion haben immer einen 64-Bit-numerischen Index von 0 bis zur Anzahl der Zeilen minus 1. 
3.  Azure Machine Learning Datasets keinen doppelten Spaltennamen und Spaltennamen, die keine Zeichenfolgen sind. Wenn eine Ausgabe Datenrahmen numerische Spalten enthält, ruft das Framework `str` auf den Spaltennamen. Doppelte Spaltennamen sind ebenso automatisch geändert, um sicherzustellen, dass die Namen eindeutig sind. Suffix (2) ist das erste Duplikat (3) der zweiten duplizieren, usw. hinzugefügt.

## <a name="operationalizing-python-scripts"></a>Ausgewogene Python-Skripten
[Python-Skript ausführen] [ execute-python-script] als Webdienst veröffentlicht werden Module ein bewertungsexperiment aufgerufen. Abbildung 3 zeigt z. B. eine bewertungsexperiment mit dem Code zum Auswerten eines einzigen Ausdrucks Python. 

![image4](./media/machine-learning-execute-python-scripts/figure3a.png)

![image5](./media/machine-learning-execute-python-scripts/python-script-with-python-pandas.png)

Abbildung 3. Webdienst für die Auswertung eines Ausdrucks Python.

Dieses Experiment erstellt ein Webdienst akzeptiert als Eingabe eine Python (als Zeichenfolge), Python-Interpreter sendet und eine Tabelle mit den Ausdruck und das ausgewertete Ergebnis zurückgibt.

## <a name="importing-existing-python-script-modules"></a>Vorhandene Python-Skriptmodule importieren
Ein allgemeiner Anwendungsfall für viele datenwissenschaftler werden Azure Machine Learning Experimente vorhandene Python-Skripten berücksichtigt. Anstelle von verketten und Einfügen des Codes in ein Skript, das [Python-Skript ausführen] [ execute-python-script] Modul akzeptiert eine dritte Eingang, eine Zipdatei mit Python-Module angeschlossen werden. Die Datei wird dann vom Framework Ausführung zur Laufzeit entpackt und den Inhalt der Bibliothekspfad Python-Interpreter hinzugefügt. Die `azureml_main` Einstiegspunkt Funktion können dann diese Module direkt.

Betrachten Sie beispielsweise die Datei Hello.py mit einem einfachen "Hello, World"-Funktion.

![Image6](./media/machine-learning-execute-python-scripts/figure4.png)

Abbildung 4. Benutzerdefinierte Funktion.

Als Nächstes erstellen Sie eine Datei Hello.zip, die Hello.py enthält:

![image7](./media/machine-learning-execute-python-scripts/figure5.png)

Abbildung 5. ZIP-Datei mit benutzerdefinierten Python-Code.

Dann hochladen Sie diese als Dataset Azure Machine Learning Studio. Erstellen und Ausführen eines einfachen Experiments, das Python-Code in der Datei Hello.zip wird an Dritte Eingabeanschluss ausführen Python-Skript wie in dieser Abbildung.

![image8](./media/machine-learning-execute-python-scripts/figure6a.png)

![image9](./media/machine-learning-execute-python-scripts/figure6b.png)

Abbildung 6. Beispiel-Experiment mit benutzerdefinierter Python-Code als Zip-Datei hochgeladen.

Das Modul Ausgabe zeigt, dass die Zip-Datei entpackt und die Funktion `print_hello` tatsächlich ausgeführt wurde.
 
![image10](./media/machine-learning-execute-python-scripts/figure7.png)
 
Abbildung 7. Benutzerdefinierte Funktion im [Python-Skript ausführen] [ execute-python-script] Modul.

## <a name="working-with-visualizations"></a>Arbeiten mit Visualisierung
Flächen mit MatplotLib visualisiert werden im Browser erstellt zurückgegeben werden [Python-Skript ausführen][execute-python-script]. Aber die Plots werden nicht automatisch umgeleitet Bilder sind mit r Daher muss der Benutzer explizit alle Flächen auf PNG-Dateien speichern, Azure Machine Learning zurückgegeben werden. 

Um Bilder aus MatplotLib erstellen, müssen die folgende Prozedur konkurrieren:

* von der Standardrenderer Qt-basierte wechseln Sie Backend zu "AGG" 
* Erstellen eines neuen Objekts Abbildung 
* die Achse und erzeugen Sie alle hinein 
* Die Abbildung in eine PNG-Datei speichern 

Dieser Prozess ist in Abbildung 8 dargestellt, die eine XY Plot Matrix mit der Funktion Scatter_matrix in Pandas.
 
![image1v](./media/machine-learning-execute-python-scripts/figure-v1-8.png)

Abbildung 8. MatplotLib Zahlen, Bilder speichern.



Abbildung 9 zeigt ein Experiment, das das zuvor Skript zurückgegeben wird über den zweiten Ausgang zeichnet.

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9a.png) 
     
![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9b.png) 

Abbildung 9. Visualisieren Flächen aus Python-Code generiert.

Es ist möglich, mehrere Figuren in anderen Bildern speichern, Azure Machine Learning Runtime nimmt alle Bilder und Visualisierung verkettet.


## <a name="advanced-examples"></a>Erweiterte Beispiele
Installiert in Azure Machine Learning Anaconda-Umgebung enthält allgemeine Pakete wie NumPy, SciPy, Scikits-lernen und diese für verschiedene Datenverarbeitung Aufgaben in einem normalen Computer Learning Pipeline effektiv verwendet werden können. Beispielsweise veranschaulicht der folgende Versuch und den Ensemble Lernenden Feature Bedeutung Faktoren für ein Dataset berechnen Scikits lernen. Die Ergebnisse können dann verwendet werden, überwachte Featureauswahl bevor zu einem anderen Computer Lernmodell ausführen.

Nachfolgend finden Sie die Python-Funktion Funktionen basieren die Bedeutung Faktoren und das Reihenfolge berechnet:

![image11](./media/machine-learning-execute-python-scripts/figure8.png)

Abbildung 10. Funktion Rank Funktionen von Faktoren.
 Der folgende Versuch berechnet und Bedeutung unzählige Funktionen in das Dataset "Pima indischen Diabetes" in Azure Machine Learning zurückgegeben:

![image12](./media/machine-learning-execute-python-scripts/figure9a.png)
![image13](./media/machine-learning-execute-python-scripts/figure9b.png)    
    
Abbildung 11. Rang Features im Dataset Pima indischen Diabetes experimentieren.

## <a name="limitations"></a>Grenzen 
[Python-Skript ausführen] [ execute-python-script] derzeit hat die folgenden Nachteile:

1.  *Sandbox ausführen.* Python-Runtime ist derzeit Sandbox und daher erlaubt keinen Zugriff auf das Netzwerk oder das lokale Dateisystem dauerhaft. Alle lokal gespeicherte Dateien werden isoliert und beendet das Modul gelöscht. Python-Code kann nicht die meisten Verzeichnisse auf dem Computer zugreifen, wird das aktuelle Verzeichnis und dessen Unterverzeichnissen Ausnahme ausgeführt, werden soll.
2.  *Fehlende anspruchsvolle Entwicklung und debugging-Unterstützung.* Python-Modul unterstützt derzeit keine IDE-Features wie Intellisense sowie debugging. Wenn das Modul zur Laufzeit fehlschlägt, vollständige Python Stapelrahmen steht auch muss im Ausgabelog des Moduls angezeigt. Derzeit wird empfohlen, entwickeln und ihrer Python-Skripts in einer Umgebung wie IPython Debuggen und importieren Sie den Code in das Modul.
3.  *Single-Frame Ausgabe.* Python-Einstiegspunkt darf nur einen einzigen Frame als Ausgabe zurück. Es kann nicht derzeit beliebige Python-Objekte wie ausgebildeten direkt zum Azure Machine Learning Runtime zurückzukehren. [R-Skript ausführen]wie[execute-r-script], die dieselbe Einschränkung auf, ist jedoch in vielen Engpass Objekte in ein Bytearray und anschließend zurückkehren, innerhalb eines Fällen möglich.
4.  *Unmöglichkeit, Python-Installation anzupassen*. Derzeit können nur benutzerdefinierte Python Module mit der zuvor beschriebenen Zip-Datei. Kleine Module möglich ist, ist es für große Module (insbesondere mit systemeigenen DLLs) oder eine große Anzahl von Modulen. 


##<a name="conclusions"></a>Schlussfolgerungen
[Python-Skript ausführen] [ execute-python-script] Modul datenwissenschaftler in Cloud-gehosteten Learning Zustandsautomatworkflows in Azure Machine Learning vorhandenen Python-Code integrieren und nahtlos als Teil eines Web Service durchsetzen können. Python-Skript-Modul natürlich in andere Module in Azure Machine Learning zusammenarbeitet und für vielfältige Aufgaben Durchsuchen von Daten Vorbehandlung Feature Extraktion Bewertung und nach der Verarbeitung der Ergebnisse verwendet werden. Back-End-Laufzeit verwendet, die für die Ausführung basiert auf Anaconda, eine bewährte und weit verbreitete Python-Distribution. Dies vereinfacht Sie mit integrierten vorhandenen Code in der Cloud.

Wir erwarten eine zusätzliche Funktionalität [Python-Skript ausführen] [ execute-python-script] Modul wie die Fähigkeit und Modelle in Python durchsetzen und eine bessere Unterstützung für die Entwicklung und das Debuggen von Code in Azure Machine Learning Studio.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Python Developer Center](/develop/python/).

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
