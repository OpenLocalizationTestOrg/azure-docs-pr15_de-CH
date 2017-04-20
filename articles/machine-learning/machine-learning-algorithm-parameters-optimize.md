<properties
    pageTitle="Wählen Sie Parameter zur Optimierung der Algorithmen in Azure Machine Learning | Microsoft Azure"
    description="Erläutert, wie optimalen Parametersatz für einen Algorithmus in Azure Machine Learning auswählen."
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


# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning"></a>Wählen Sie Parameter zur Optimierung der Algorithmen in Azure maschinelles lernen

Dieses Thema beschreibt die richtige Hyperparameter für einen Algorithmus in Azure Machine Learning festlegen wählen. Die meisten Computer lernalgorithmen müssen Parameter festgelegt. Beim Trainieren eines Modells müssen Sie Werte für diese Parameter bereitstellen. Die Wirksamkeit des Modells geschultes hängt die Parameter, die Sie auswählen. Finden Sie die optimale Gruppe von Parametern wird als *Auswahl*bezeichnet.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Es gibt verschiedene Methoden für die Auswahl Modell. Computer lernen Cross-Validierung ist eine der meistverwendeten Methoden für Auswahl und ist der Mechanismus für die Auswahl von Modell in Azure Machine Learning. Da Azure Machine Learning R und Python unterstützt, können Sie immer ihre eigenen Mechanismus Modell R oder Python implementieren.

Es gibt vier Schritte bei der Suche nach der besten Parameter festgelegt:

1.  **Definieren der Parameterraum**: für den Algorithmus zunächst entscheiden möchten genaue Parameterwerte.
2.  **Definieren die übergreifende Überprüfung Einstellungen**: entscheiden Sie, wie Falten übergreifende Überprüfung für das Dataset auswählen.
3.  **Definieren der Metrik**: entscheiden, welche Metrik zu verwenden, die optimale Gruppe von Parametern wie Genauigkeit, Root-Mean squared Fehler, Genauigkeit, Rückruf oder f-Wert.
4.  **Zug auswerten, und vergleichen**: für jede eindeutige Kombination der Parameterwerte übergreifende Überprüfung ist durchgeführten und Fehler Metrik definieren. Bewertung und Vergleich können Sie das beste Modell.

Die folgende Abbildung zeigt, zeigt, wie dies in Azure Machine Learning erzielt werden kann.

![Den besten Parametersatz suchen](./media/machine-learning-algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>Parameterbereich festlegen
Sie können den Parameter bei der Initialisierungsschritt definieren. Im Bereich Parameter aller Computer lernen Algorithmen verfügt über zwei Trainer-Modi: *Einzelne Parameter* und *Bereich*. Wählen Sie im Bereich. Im Bereich Modus können Sie mehrere Werte für jeden Parameter eingeben. Sie können durch Kommas getrennte Werte in das Textfeld eingeben.

![Zwei Klassen stärkere Entscheidungsstruktur einzelner parameter](./media/machine-learning-algorithm-parameters-optimize/fig2.png)

 Alternativ können Sie die Mindest- und Punkte des Rasters und die Gesamtzahl der Punkte mit der **Range-Generator verwenden**definieren. Standardmäßig werden die Parameterwerte auf einer linearen Skala generiert. Aber wenn **Maßstab** aktiviert ist, werden die Werte in der Protokolldatei generiert (d. h. das Verhältnis zwischen benachbarten Punkten ist konstant anstelle ihrer Differenz). Für ganzzahlige Parameter definieren Sie einen Bereich mit einem Bindestrich. Beispielsweise "1-10" bedeutet, dass alle ganzen Zahlen zwischen 1 und 10 (beide einschließlich) festgelegten Parameter bilden. Mischmodus wird ebenfalls unterstützt. Beispielsweise den Parameter "1 bis 10, 20, 50" zählen Ganzzahlen 1 bis 10, 20 und 50.

![Zwei Klassen stärkere Entscheidungsstruktur Bereich](./media/machine-learning-algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Übergreifende Überprüfung Falten definieren
[Partition und Beispiel] [ partition-and-sample] Modul kann die Daten nach dem Zufallsprinzip Falten zuweisen. In der folgenden Beispielkonfiguration des Moduls definieren wir fünf Falten und die Beispielinstanzen zufällig eine Faltung Nummer zugewiesen.

![Partition und Beispiel](./media/machine-learning-algorithm-parameters-optimize/fig4.png)


## <a name="define-the-metric"></a>Definieren der Metrik
[Optimieren Modell Hyperparameter] [ tune-model-hyperparameters] Modul unterstützt empirisch Auswahl der besten Parameter für einen vorgegebenen Algorithmus und Dataset. Neben anderen Informationen enthält zu trainieren, **Eigenschaftenbereich dieses Moduls** die Metrik für die Bestimmung der besten Parameter festgelegt. Es verfügt über zwei verschiedene Dropdown-Listenfelder für Klassifizierung und Regression Algorithmen. Der Algorithmus berücksichtigt klassifizierungsalgorithmus, Regression Metrik wird ignoriert und umgekehrt. In diesem Beispiel ist die **Genauigkeit**.   

![Sweep-Parameter](./media/machine-learning-algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>Schulen, auswerten und vergleichen  
Dieselbe [Optimierung Modell Hyperparameter] [ tune-model-hyperparameters] Modul bildet die Modelle, die den Parametersatz entsprechen, verschiedenen Kriterien ausgewertet und erstellt am besten qualifizierten Modells anhand der Metrik, die Sie auswählen. Diese Unterrichtseinheit enthält zwei Pflichtfelder:

* Ungeübte Teilnehmern
* Das dataset

Das Modul hat auch eine optionale Eingabe Dataset. Verbinden Sie Dataset mit Faltung Eingabe obligatorisch Dataset. Dataset nicht gefaltet Informationen zugewiesen ist, wird eine 10-divisibel Cross-Überprüfung automatisch standardmäßig ausgeführt. Faltung Zuweisung erfolgt nicht und ein Dataset Validierung erfolgt am Anschluss optional Dataset Zug Testmodus ausgewählt, und das erste Dataset zum Trainieren des Modells für jede Parameterkombination verwendet.

![Stärkere Entscheidung Struktur Klassifizierung](./media/machine-learning-algorithm-parameters-optimize/fig6a.png)

Das Modell wird für die Validierung Dataset ausgewertet. Linken Ausgang des Moduls zeigt verschiedene Metriken als Funktionen der Parameterwerte. Der richtige Port Gibt geschulte Modell entspricht leistungsfähigsten Modell nach ausgewählten Metrik (**Genauigkeit** in diesem Fall).  

![Validierung dataset](./media/machine-learning-algorithm-parameters-optimize/fig6b.png)

Sie sehen die genauen Parameter mit einer Visualisierung des richtige Ports ausgewählt. Dieses Modell kann nach dem Speichern als geschulte Modell Testsatz bewerten oder einen standardisierten Webdienst verwendet werden.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
