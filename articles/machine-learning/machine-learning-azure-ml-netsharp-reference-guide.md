<properties 
    pageTitle="Leitfaden für die Net-neuronale Netzwerke Specification Language | Microsoft Azure" 
    description="Syntax für die Net-neuronale Netzwerke Spezifikationssprache Beispiele zum Erstellen eines benutzerdefinierten neuronales Netzwerk-Modells in Microsoft Azure ML Verwendung#" 
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
    ms.date="09/12/2016" 
    ms.author="jeannt"/>



# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning"></a>Leitfaden Sie für Net-neuronale Spezifikationssprache für Azure maschinelles lernen

## <a name="overview"></a>Übersicht
NET-ist eine Sprache entwickelt von Microsoft in Microsoft Azure Machine Learning neuronale Netzwerkarchitekturen neuronales Netzwerk Module definiert. In diesem Artikel lernen Sie:  

-   Grundlegende Konzepte im Zusammenhang mit neuronalen Netzen
-   Neuronales Netzwerk sowie zum Definieren der Hauptkomponenten
-   Syntax und Schlüsselwörter der Net-Spezifikationssprache
-   Beispiele für benutzerdefinierte Verwendung erstellt neuronale Netzwerke# 
    
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

## <a name="neural-network-basics"></a>Neuronales Netzwerk-Grundlagen
Eine Struktur des neuronalen Netzwerks besteht aus ***Knoten*** , die in ***Ebenen***und gewichtete ***Verbindung*** (oder ***Kanten***) angeordnet werden zwischen den Knoten. Anschlüsse sind Richtungspfeile und jede Verbindung verfügt über eine ***Quell-*** und ***Zielknoten*** .  

Jede ***lernfähig Ebene*** (eine versteckte oder eine Ausgabeschicht) hat mindestens eine ***Verbindung Pakete***. Verbindung-Bundle besteht aus Quellebene und Spezifikation der Verbindungen von dieser Quellebene. Alle Verbindungen in einem bestimmten Bündel Teilen derselben ***Quellebene*** und ***Zielebene***derselben. Net-gilt ein Bündel Verbindung zu dem Bündel Zielebene.  
 
NET-unterstützt verschiedene Verbindung Pakete können Sie anpassen, wie Eingaben in ausgeblendeten Ebenen zugeordnet und die Ausgaben zugeordnet.   

Standard oder standard-Paket ist eine **vollständige Bündel**, in der jeder Knoten in der Quellebene für jeden Knoten in der Zielebene verbunden ist.  

Darüber hinaus unterstützt Net-vier folgende erweiterte Verbindung Pakete:  

-   **Gefilterte Pakete**. Benutzer kann ein Prädikat mit die Positionen der Knoten Ebene Quelle und Ziel Ebene definieren. Knoten verbunden sind, wenn das Prädikat True ist.
-   **Faltenden Pakete**. Der Benutzer können kleine Themenwelten Knoten in der Quellebene. Jeder Knoten in der Zielebene ist ein Viertel der Knoten in der Quellebene verbunden.
-   **Bündel-Pooling** und **Antwort Normalisierung Bündel**. Diese ähneln faltenden Pakete, der Benutzer in der Quellebene kleine Themenwelten Knoten definiert. Der Unterschied ist, dass die Gewichte der Kanten in diesen Paketen nicht trainiert. Stattdessen werden die Quellwerte Knoten Knoten Zielwert bestimmen eine vordefinierte Funktion zugewiesen.  

Mit Net-definieren die Struktur des neuronalen Netzwerks ermöglicht definieren komplexe Strukturen tief neuronale Netzwerke oder Wellen beliebige Dimensionen zu lernen Daten wie Bild-, Audio- oder Videodateien bekannt sind.  

## <a name="supported-customizations"></a>Unterstützte Anpassungen
Die Architektur des neuronalen Netzwerkmodelle, die in Azure Machine Learning erstellen kann umfassend mit Net-angepasst werden. Sie können:  

-   Ausgeblendete Ebenen erstellen und die Anzahl der Knoten in jeder Ebene.
-   Geben Sie an wie Ebenen miteinander verbunden werden.
-   Definieren Sie spezielle Verbindung Strukturen wie Wellen und Freigabe Pakete.
-   Geben Sie unterschiedliche Aktivierungsfunktionen.  

Einzelheiten der Sprachsyntax Spezifikation finden Sie unter [Strukturspezifikation](#Structure-specifications).  
 
[Beispiele für neuronale Netzwerke für einige allgemeine Aufgaben von Simplex und komplexe lernen Maschine definieren Beispiele.](#Examples-of-Net#-usage)  

## <a name="general-requirements"></a>Allgemeine Vorschriften
-   Es muss genau ein Ausgabe-Schicht mindestens Eingabeebene und oder mehr verdeckte Schichten. 
-   Jede Ebene hat eine feste Anzahl von Knoten im Prinzip ein rechteckiges Array von beliebiger Größe angeordnet. 
-   Input Ebenen haben keine zugeordneten ausgebildeten Parameter und den Punkt, wo Daten im Netzwerk gibt. 
-   Lernfähig Ebenen (Layer ausgeblendet und Ausgabe) sind ausgebildete Parameter als Gewicht und Biases zugeordnet. 
-   Die Quell- und Ziel-Knoten müssen separate Ebenen. 
-   Azyklische müssen sein; Anders gesagt kann keine Kette von Verbindungen zurück zu den ursprünglichen Knoten vorhanden sein.
-   Die Ausgabeschicht Quellebene Verbindung Bundle nicht möglich.  

## <a name="structure-specifications"></a>Struktur-Spezifikationen
Neuronales Netzwerk Strukturspezifikation besteht aus drei Abschnitten: **Konstantendeklaration** **Layer-Deklaration**, die **Verbindungsdeklaration**. Außerdem wird ein Abschnitt optional **Erklärung freigeben** . Die Abschnitte können in beliebiger Reihenfolge angegeben werden.  

## <a name="constant-declaration"></a>Deklaration von Konstanten 
Eine Konstantendeklaration ist optional. Es bietet die Möglichkeit, an anderer Stelle in der Definition des neuronalen Netzwerks verwendeten Werte definieren. Die Anweisung für die Attributdeklaration besteht aus einem Identifier, gefolgt von einem Gleichheitszeichen und ein.   

Die folgende Anweisung definiert eine Konstante **X**:  


    Const X = 28;  

Um gleichzeitig zwei oder mehr Konstanten definieren, Bezeichnernamen und Werte in Klammern eingeschlossen und durch Semikolons trennen. Zum Beispiel:  

    Const { X = 28; Y = 4; }  

Rechts jeder Zuweisung kann eine ganze Zahl, eine reelle Zahl, einen booleschen Wert (True oder False) oder einen mathematischen Ausdruck. Zum Beispiel:  

    Const { X = 17 * 2; Y = true; }  

## <a name="layer-declaration"></a>Layer-Deklaration
Die Ebene ist erforderlich. Die Größe und die Quelle der Ebene einschließlich Verbindung Pakete und Attribute definiert. Erklärung-Anweisung beginnt mit dem Namen der Ebene (input, ausgeblendet oder Ausgabe), gefolgt von den Dimensionen der Ebene (ein Tupel von positiven ganzen Zahlen). Zum Beispiel:  

    input Data auto;
    hidden Hidden[5,20] from Data all;
    output Result[2] from Hidden all;  

-   Das Produkt der Dimensionen ist die Anzahl der Knoten in der Ebene. In diesem Beispiel werden zwei Dimensionen [5,20], d. h. 100 Knoten gibt es in der Ebene.
-   Die Ebenen können in einer beliebigen Reihenfolge deklariert: Wenn mehrere Eingabeebene definiert ist, muss die Reihenfolge, in der sie deklariert, die Reihenfolge der Funktionen in den Eingabedaten.  


Um anzugeben, dass die Anzahl der Knoten in einer Ebene automatisch bestimmt werden soll, verwenden Sie **Auto** -Schlüsselwort. **Auto** -Schlüsselwort ist unterschiedlich, je nach:  

-   In einer Deklaration Eingabeebene ist die Anzahl der Knoten die Anzahl der Funktionen in den Eingabedaten.
-   In einer Deklaration verborgenen Ebene ist die Anzahl der Knoten die Anzahl, die durch den Parameterwert für **Ausgeblendete Knoten**angegeben wird. 
-   In der Deklaration eines Ausgabe-Ebene ist die Anzahl der Knoten 2 für zwei Klassen Klassifikation 1 Regression, und die Anzahl der Ausgabeknoten für multiklassenklassifizierung.   

Beispielsweise kann die folgenden Netzwerk-Definition die Größe aller Ebenen automatisch bestimmt werden soll:  

    input Data auto;
    hidden Hidden auto from Data all;
    output Result auto from Hidden all;  


Eine Ebene Deklaration lernfähig Ebene (Ebenen ausgeblendeten oder Ausgabe) kann optional die ausgangsfunktion (auch eine Aktivierungsfunktion genannt), die standardmäßig enthalten **Sigmoidfunktion** für klassifizierungsmodelle und **lineare** Regression Modelle. (Auch bei Verwendung der Standard können Sie die Aktivierungsfunktion ausdrücklich ggf. zur Verdeutlichung.)

Die folgende Ausgabefunktionen werden unterstützt:  

-   Sigmoidfunktion
-   Linear
-   Softmax
-   rlinear
-   Quadrat
-   Wurzel
-   srlinear
-   Abs
-   tanh 
-   brlinear  

Beispielsweise die folgende Deklaration die **Softmax** -Funktion:  

    output Result [100] softmax from Hidden all;  

## <a name="connection-declaration"></a>Verbindungsdeklaration
Sobald lernfähig Ebene definieren, deklarieren Sie verbundene Ebenen definiert haben. Die Verbindungsdeklaration Bundle beginnt mit Schlüsselwort **aus**, gefolgt vom Namen der Quellebene für das Paket und die Art der Verbindung Paket erstellen.   

Derzeit sind fünf Verbindung Pakete unterstützt:  

-   **Vollständige** Pakete vom Schlüsselwort **Alle**
-   **Gefilterte** Pakete durch das Schlüsselwort **,**gefolgt von einem Prädikatausdruck angegeben
-   **Convolutional** -Pakete durch das Schlüsselwort **rollst**, gefolgt von der Convolution-Attribute angegeben
-   **Pooling** Bündel Schlüsselwörter **max Pool** oder **Pool bedeuten** angegeben
-   **Antwort Normalisierung** Pakete vom Schlüsselwort **Antwort norm**      

## <a name="full-bundles"></a>Vollständige Pakete  

Eine vollständige Verbindungszeichenfolge Bundles eine Verbindung von jedem Knoten in der Quellebene für jeden Knoten in der Zielebene. Dies ist die Netzwerk-Verbindung.  

## <a name="filtered-bundles"></a>Gefilterte Pakete
Eine gefilterte Verbindung Bundle-Spezifikation enthält ein Prädikat, ausgedrückt syntaktisch, ähnlich wie ein Lambda-Ausdruck von C#. Das folgende Beispiel definiert zwei gefilterte Pakete:  

    input Pixels [10, 20];
    hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  

-   Das Prädikat für _ByRow_ **s** ist ein rechteckiges Array Knoten der Eingabeebene _Pixel_Index darstellt und **d** ist ein Index in das Array der verborgenen Ebene _ByRow_Knoten darstellt. **S** und **d** ist ein Tupel mit Länge zwei Ganzzahlen. Konzeptionell **s** Bereiche über alle Paare von Ganzzahlen mit _0 < = s [0] < 10_ und _0 < = s[1] < 20_, und **d** über alle Paare von Ganzzahlen mit _0 < = [0] d < 10_ und _0 < = d[1] < 12_. 
-   Auf der rechten Seite der Prädikatausdruck ist eine Bedingung. In diesem Beispiel für jeden Wert von **s** und **d** , die Bedingung zutrifft, befindet sich eine Kante vom Quellknoten Ebene zum Zielknoten Ebene. Daher wird diese Filterausdruck enthält das Paket eine Verbindung von **s** **d** in allen Fällen gleich d [0] [0] s ist festgelegten Knoten definierten Knoten.  

Optional können Sie eine Reihe von Gewicht für gefilterte Bundle angeben. Der Wert für das Attribut **Gewicht** muss ein Tupel von Gleitkommawerten mit einer Länge, die die Anzahl der Verbindungen, die das Paket definiert entspricht. Gewichte werden standardmäßig nach dem Zufallsprinzip generiert.  

Die Gewichtungswerte werden nach Zielindex Knoten gruppiert. Wenn erste Zielknoten K Quellknoten verbunden ist, sind die ersten _K_ Elemente des Tupels **Gewicht** das Gewicht für den ersten Zielknoten in der Reihenfolge des Index. Das gleiche gilt für die verbleibenden Zielknoten.  

Es ist möglich, Gewichte direkt als Konstante Werte angeben. Wenn Sie zuvor das Gewicht haben, können Sie diese z. B. als Konstanten mithilfe dieser Syntax angeben:

    const Weights_1 = [0.0188045055, 0.130500451, ...]


## <a name="convolutional-bundles"></a>Faltenden Pakete
Wenn die Trainingsdaten eine homogene Struktur hat, sind faltenden Verbindungen häufig zum allgemeinen Funktionen der Daten. Z. B. Bild, audio oder video Daten, räumliche oder zeitliche Dimensionalität können ziemlich.  

Faltenden Pakete verwenden rechteckigen **Kerne** , die Dimensionen geschoben werden. Im Wesentlichen definiert jeder Kernel Gewichte in der lokalen Umgebung angewendet als **Kernel-Applikationen**. Jede Anwendung Kernel entspricht einem Knoten in der Quellebene **zentralen Knoten**genannt. Das Gewicht einer Kernel sind viele Verbindungen verteilt. In einem Bündel faltenden jeder Kernel ist rechteckig und alle Kernel sind die gleiche Größe.  

Faltenden Pakete unterstützen die folgenden Attribute:

**InputShape** definiert die Dimensionalität der Quellebene für die Zwecke dieses faltenden Paket. Der Wert muss ein Tupel von positiven ganzen Zahlen. Das Produkt Zahlen muss gleich der Anzahl der Knoten in der Quellebene, aber, es muss nicht mit der Dimensionalität für die Quellebene deklariert. Die Länge dieses Tupel wird der **Stelligkeit** faltenden Bundle. (In der Regel bezeichnet Stelligkeit Anzahl an Argumenten oder Operanden, die eine Funktion ausführen können.)  

Verwenden Sie zum Definieren der Form und Speicherorte der Kernel **KernelShape**, **Stride** **Padding**, **LowerPad**und **UpperPad**-Attribute:   

-   **KernelShape**: (erforderlich) definiert die Dimensionalität jedes Kernel faltenden Bundle. Der Wert muss ein Tupel von positiven ganzen Zahlen mit einer Länge, die der Stelligkeit des Pakets entspricht. Jede Komponente dieses Tupel darf nicht größer als die entsprechende Komponente des **InputShape**sein. 
-   **Schritt**: (optional) definiert die gleitende Schrittweiten Convolution (schrittweise Größe für jede Dimension), die Entfernung zwischen den zentralen Knoten. Der Wert muss ein Tupel von positiven ganzen Zahlen mit einer Länge, die gleich der Stelligkeit des Pakets. Jede Komponente dieses Tupel darf nicht größer als die entsprechende Komponente des **KernelShape**sein. Der Standardwert ist ein Tupel mit allen Komponenten gleich eins. 
-   **Freigabe**: (optional) definiert das Gewicht für jede Dimension der Faltung. Der Wert kann ein boolescher Wert oder ein Tupel von booleschen Werten mit einer Länge gleich der Stelligkeit des Pakets. Ein boolescher Wert wird erweitert, um ein Tupel mit der richtigen Länge mit allen Komponenten gleich dem angegebenen Wert. Der Standardwert ist ein Tupel, das alle Werte True aus. 
-   **MapCount**: (optional) definiert die Anzahl der Funktion faltenden Bundle zugeordnet. Der Wert kann eine positive Ganzzahl oder ein Tupel von positiven ganzen Zahlen mit einer Länge gleich der Stelligkeit des Pakets. Ein ganzzahligen Wert ein Tupel die richtige Länge mit der ersten gleich dem angegebenen Wert zu erweitert, und die verbleibenden Komponenten zu entsprechen. Der Standardwert ist 1. Die Gesamtzahl der Funktion zugeordnet ist das Produkt der Komponenten des Tupels. Diese Gesamtanzahl der Komponenten factoring bestimmt wie die Zuordnungswerte Feature in den Zielknoten gruppiert werden. 
-   **Gewicht**: (optional) definiert die anfängliche Gewichte für das Paket. Der Wert muss ein Tupel von Gleitkommawerte mit einer Länge, die die Anzahl der Kerne die Anzahl Gewicht pro-Kernel gemäß der in diesem Artikel. Standard Gewichte werden nach dem Zufallsprinzip generiert.  

Es gibt zwei Eigenschaften, Abstand die Eigenschaften gegenseitig steuern:

-   **Padding**: (optional) bestimmt, ob die Eingabe mit **Polsterung Standardschema**aufgefüllt werden soll. Der Wert kann ein boolescher Wert sein, oder es ein Tupel von booleschen Werten mit einer Länge gleich der Stelligkeit des Pakets. Ein boolescher Wert wird erweitert, um ein Tupel mit der richtigen Länge mit allen Komponenten gleich dem angegebenen Wert. Wenn für eine Dimension der Wert True ist, wird die Quelle, zentralen Knoten der ersten und letzten Kernel in dieser Dimension die ersten und letzten Knoten in dieser Dimension in der Quellebene sind logisch in dieser Dimension Weitere Applikationen unterstützen mit NULL-Werten aufgefüllt. Damit die Anzahl der Knoten in jeder Dimension "dummy" wird automatisch ermittelt, exakt _(InputShape [d] - 1) / Stride [d] + 1_ Kernel in gepolsterten Quellebene. Für eine Dimension der Wert False ist, gelten die Körner, damit die Anzahl der Knoten auf jeder Seite, links (bis ein Unterschied von 1) übereinstimmt. Der Standardwert dieses Attributs ist ein Tupel mit allen Komponenten gleich.
-   **UpperPad** und **LowerPad**: (optional) bieten mehr Kontrolle über die Anzahl von Leerstellen zu verwenden. **Wichtig:** Diese Attribute können definiert werden, wenn die oben genannte Eigenschaft **Padding** ist ***nicht*** definiert. Die Werte sollten Tupel mit Ganzzahlwerten sein Stelligkeit Bündel. Wenn diese Attribute angegeben sind, werden die untere und obere Ende jeder Dimension der Eingabeebene "dummy" Knoten hinzugefügt. Anzahl der oberen und unteren enden in jeder Dimension hinzugefügt wird **LowerPad**[i] bis **UpperPad**[i] bestimmt. Um sicherzustellen, dass Kernels nur auf "echten" und nicht "dummy" Knoten entsprechen, müssen Folgendes erfüllt sein:
    -   Jede Komponente des **LowerPad** muss kleiner als KernelShape [d] / 2. 
    -   Jede Komponente des **UpperPad** darf nicht größer sein als KernelShape [d] / 2. 
    -   Der Standardwert dieser Attribute ist ein Tupel mit allen Komponenten gleich 0. 

Die Einstellung **Padding** = True können Sie so viel Abstand zu den "Center" des Kernels in "Real" Eingabe benötigt wird. Mathematik etwas zum Berechnen der Größe geändert wird. Normalerweise berechnet die Größe _D_ als _D = (I - K) / S + 1_ _I_ ist die Eingabegröße _kernelgröße ist_ , _ist die Schrittweite_ und _/_ Ganzzahldivision (Runden in Richtung Null) ist. Wenn Sie UpperPad = [1, 1] _ich_ Eingabegröße ist 29 und _D = (29-5) / 2 + 1 = 13_. Aber wenn **Padding** = true im Wesentlichen _ich_ ruft stieß bis _K - 1_; Daher _D = ((28 + 4) - 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14_. Durch Angeben von Werten für **UpperPad** und **LowerPad** erhalten Sie mehr Kontrolle über den Abstand als nur **Abstand** festlegen = True.

Weitere Informationen zu faltenden Netzwerken und deren Anwendung finden Sie in diesen Artikeln:  

-   [http://deeplearning.NET/Tutorial/lenet.HTML](http://deeplearning.net/tutorial/lenet.html )
-   [http://Research.Microsoft.com/pubs/68920/icdar03.PDF](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
-   [http://people.csail.mit.edu/jvb/Papers/cnn_tutorial.PDF](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Pooling Pakete
Ein **pooling Bundle** Geometrie faltenden Konnektivität ähnlich gilt jedoch verwendet vordefinierte Funktionen zum Knoten Quellwerte Knoten Zielwert abgeleitet. Pooling Pakete haben daher keine lernfähig Zustand (Gewicht oder Biases). Pooling Pakete unterstützen die faltenden Attribute außer **Freigabe**, **MapCount**und **Gewicht**.  

In der Regel überlappen angrenzenden pooling Einheiten zusammengefasst Kernels. Stride [d] ist KernelShape [d] in jeder Dimension gleich, erhaltene Schicht traditionelle lokale pooling Layer, die häufig in faltenden neuronalen Netzwerken eingesetzt. Jeder Zielknoten berechnet die maximale oder den Mittelwert der Aktivitäten der Kernel in der Quellebene.  

Das folgende Beispiel veranschaulicht ein pooling Bundle: 

    hidden P1 [5, 12, 12]
      from C1 max pool {
        InputShape  = [ 5, 24, 24];
        KernelShape = [ 1,  2,  2];
        Stride      = [ 1,  2,  2];
      }  

-   Der Stelligkeit Bündel ist 3 (die Länge von Tupeln, **InputShape**, **KernelShape**und **Stride**). 
-   Die Anzahl der Knoten in der Quellebene _5 *24* 24 = 2880_. 
-   Ist eine traditionelle lokale pooling Ebene **KernelShape** und **Stride** gleich sind. 
-   Die Anzahl der Knoten in der Zielebene _5 *12* 12 = 1440_.  
    
Weitere Informationen zum pooling Ebenen finden Sie in diesen Artikeln:  

-   [http://www.cs.Toronto.edu/~hinton/absps/imagenet.PDF](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (Abschnitt 3.4)
-   [http://cs.Nyu.edu/~koray/Publis/lecun-iscas-10.PDF](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
-   [http://cs.Nyu.edu/~koray/Publis/jarrett-iccv-09.PDF](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)
    
## <a name="response-normalization-bundles"></a>Antwort Normalisierung Pakete
**Antwort Normalisierung** ist ein lokaler Normalisierung von Geoffrey Hinton Einführung Et al. in der [ImageNet gehört mit tief Faltenden neuronalen Netzwerken](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf). Antwort Normalisierung wird zur Generalisierung neuronaler Netze unterstützen. Wenn ein Neuron Ebene hohe Aktivierung auslösen, unterdrückt eine lokale Normalisierung Ebene Aktivierung Maß umgebenden Neuronen. Dies erfolgt mithilfe von drei Parametern (***α***, ***β***und ***k***) und eine faltenden Struktur (oder Nachbarschaft Form). Jedes Neuron in der Zielebene ***y*** entspricht einem Neuron ***X*** in der Quellebene. Aktivierung auf ***y*** anhand der folgenden Formel, wobei ***f*** ist die Aktivierung eines Neurons und ***Nx*** Kernel (oder die Menge mit den Neuronen der Nachbarschaft von ***X***), gemäß der folgenden faltenden Struktur:  

![][1]  

Antwort Normalisierung Pakete unterstützen die faltenden Attribute außer **Freigabe**, **MapCount**und **Gewicht**.  
 
-   Kernel Neuronen der Karte ***X***enthält, ist das Normalisierung Schema **dieselbe Zuordnung Normalisierung**genannt. Um dieselbe Zuordnung Normalisierung zu definieren, muss die erste Koordinate in **InputShape** den Wert 1.
-   Wenn der Kernel die räumliche Position ***X***Neuronen enthält, aber die Neuronen in anderen Karten, heißt Normalisierung Schema **über Karten Normalisierung**. Dieser Typ der Antwort Normalisierung implementiert eine Form der seitlichen Hemmung von Wettbewerb für große Ebenen unter Neuron Ausgaben berechnet auf verschiedenen Karten in echten Neuronen enthaltenen Typ. Über Karten Normalisierung definieren, muss die erste Koordinate eine ganze Zahl größer als 1 und nicht größer als die Anzahl der Karten und der Rest der Koordinaten muss den Wert 1.  

Da Antwort Normalisierung Bündel Knoten Quellwerte Knotenwert Ziel bestimmen eine vordefinierte Funktion zuweisen, können sie keine lernfähig Zustand (Gewicht oder Biases).   

**Warnung**: die Knoten in der Zielebene entsprechen den zentralen des Kerns Knoten Neuronen. Beispielsweise ist KernelShape [d] ungerade, _KernelShape [d] / 2_ entspricht dem zentralen Kern-Knoten. _KernelShape [d]_ auch der zentrale Knoten ist am _KernelShape [d] / 2 1_. Daher ist **Padding**[d] falsch, die erste und die letzte _KernelShape [d] / 2_ Knoten keinen entsprechende Knoten in der Zielebene. Um diese Situation zu vermeiden, definieren Sie als **Abstand** [true, True, true].  

Neben den oben beschriebenen vier Attributen unterstützen Antwort Normalisierung Pakete die folgenden Attribute:  

-   **Alpha**: (erforderlich) gibt ein Gleitkommawert, der in der vorherigen Formel ***α*** entspricht. 
-   **Beta**: (erforderlich) gibt ein Gleitkommawert, der in der vorherigen Formel ***β*** entspricht. 
-   **Offset**: (optional) gibt ein Gleitkommawert, der in der vorherigen Formel ***k*** entspricht. Der Standardwert 1.  

Das folgende Beispiel definiert eine Antwort Normalisierung Bundle mit diesen Attributen:  

    hidden RN1 [5, 10, 10]
      from P1 response norm {
        InputShape  = [ 5, 12, 12];
        KernelShape = [ 1,  3,  3];
        Alpha = 0.001;
        Beta = 0.75;
      }  

-   Die Quellebene enthält fünf Karten mit Aof 12 x 12 insgesamt 1440 Knoten. 
-   Der Wert **KernelShape** gibt an, dass eine gleiche Karte Normalisierung Ebene ist die Nachbarschaft eine 3 x 3-Rechteck. 
-   **Abstand** der Standardwert ist False, so hat der Zielebene nur 10 Knoten in jeder Dimension. Fügen Sie auf einem Knoten in der Zielebene, die jeden Knoten in der Quellebene entspricht, Abstand = [True, True, True]; und ändern Sie die Größe der RN1 [5, 12 12].  

## <a name="share-declaration"></a>Freigabe-Deklaration 
NET-unterstützt optional mehrere Pakete mit freigegebenen definieren. Das Gewicht der zwei Pakete können freigegeben werden, wenn die Strukturen gleich sind. Die folgende Syntax definiert mit freigegebenen Gewicht:  

    share-declaration:
        share    {    layer-list    }
        share    {    bundle-list    }
       share    {    bias-list    }
    
    layer-list:
        layer-name    ,    layer-name
        layer-list    ,    layer-name
    
    bundle-list:
       bundle-spec    ,    bundle-spec
        bundle-list    ,    bundle-spec
    
    bundle-spec:
       layer-name    =>     layer-name
    
    bias-list:
        bias-spec    ,    bias-spec
        bias-list    ,    bias-spec
    
    bias-spec:
        1    =>    layer-name
    
    layer-name:
        identifier  

Die Freigabe-Deklaration gibt z. B. die Ebenennamen, dass Gewicht und Biases freigegeben werden sollten:  

    Const {
      InputSize = 37;
      HiddenSize = 50;
    }
    input {
      Data1 [InputSize];
      Data2 [InputSize];
    }
    hidden {
      H1 [HiddenSize] from Data1 all;
      H2 [HiddenSize] from Data2 all;
    }
    output Result [2] {
      from H1 all;
      from H2 all;
    }
    share { H1, H2 } // share both weights and biases  

-   Die Funktionen sind in zwei gleich große input Ebenen aufgeteilt. 
-   Ausgeblendeten Ebenen berechnen dann höhere Funktionen der beiden Eingabe Ebenen. 
-   Die Freigabe-gibt an, dass _H1_ und _H2_ in gleicher Weise mit ihren jeweiligen Eingaben berechnet werden müssen.  
 
Alternativ könnte dies mit zwei separaten Teilen Deklarationen wie folgt angegeben werden:  

    share { Data1 => H1, Data2 => H2 } // share weights  

<!-- -->

    share { 1 => H1, 1 => H2 } // share biases  

Die Kurzform können nur, wenn die Ebenen ein Bundle enthalten. Im Allgemeinen kann Freigabe nur, wenn die entsprechende Struktur identisch, so dass sie dieselbe Größe, faltenden Geometrie und So weiter.  

## <a name="examples-of-net-usage"></a>Beispiele für Net-Verwendung
Dieser Abschnitt enthält Beispiele der Verwendung Net-Ausgeblendete Ebenen hinzufügen, definieren, wie die ausgeblendete Ebenen mit anderen Ebenen interagieren und faltenden Netzwerke.   

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Definieren ein einfaches benutzerdefiniertes neuronales Netzwerks: Beispiel "Hello World"
Dieses Beispiel veranschaulicht, wie ein neuronales Netzwerkmodell erstellen eine einzige verdeckten Schicht.  

    input Data auto;
    hidden H [200] from Data all;
    output Out [10] sigmoid from H all;  

Das Beispiel veranschaulicht einige grundlegende Befehle wie folgt:  

-   Die erste Zeile definiert die Eingabeebene ( _Daten_bezeichnet). Wenn Sie **Auto** -Schlüsselwort verwenden, enthält das neuronale Netzwerk automatisch alle Funktion Spalten input Beispiele. 
-   Die zweite Zeile wird die verborgene Ebene erstellt. Der Namen _H_ erhält der verborgenen Ebene 200 Knoten hat. Diese Ebene ist die Eingabeebene vollständig verbunden.
-   Die dritte Zeile definiert die Ausgabeschicht ( _O_bezeichnet) 10 Ausgabeknoten enthält. Wenn das neuronale Netzwerk für die Klassifizierung verwendet wird, wird eine Ausgabeknoten pro Klasse. Das Schlüsselwort **Sigmoidfunktion** gibt an, dass die ausgangsfunktion der Ausgabeschicht angewendet wird.   

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Definieren Sie mehrere ausgeblendete Ebenen: Computer Vision wird
Im folgenden Beispiel wird veranschaulicht, wie ein komplexeres neuronales Netz mit mehreren benutzerdefinierten Ausgeblendete Ebenen definieren.  

    // Define the input layers 
    input Pixels [10, 20];
    input MetaData [7];
    
    // Define the first two hidden layers, using data only from the Pixels input
    hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;
    
    // Define the third hidden layer, which uses as source the hidden layers ByRow and ByCol
    hidden Gather [100] 
    {
      from ByRow all;
      from ByCol all;
    }
    
    // Define the output layer and its sources
    output Result [10]  
    {
      from Gather all;
      from MetaData all;
    }  

Dieses Beispiel zeigt mehrere Features des Spezifikationssprache neuronale Netzwerke:  

-   Die Struktur hat zwei input Ebenen _Pixel_ und _Metadaten_.
-   Die _Pixel_ -Ebene ist Quellebene für zwei Verbindung Pakete mit Ziel, _ByRow_ und _ByCol_.
-   Die Ebenen _erfassen_ und das _Ergebnis_ sind Zielebene in mehreren verbindungsbündeln.
-   Die Ausgabeschicht _Ergebnis_ist eine Zielebene in zwei verbindungsbündeln. eine mit der zweiten Ebene ausgeblendet (Gather) als eine Zielebene und andere mit der Eingabeebene (Metadaten) als Zielebene.
-   Ausgeblendeten Ebenen, _ByRow_ und _ByCol_angeben gefilterten Konnektivität mit Prädikatsausdrücke. Genauer gesagt, den Knoten im _ByRow_ am [x, y] Knoten in _Pixel_ , die den ersten Index gleich den Knoten erste Koordinate Koordinate besteht X. Entsprechend der Knoten im _ByCol am [x, y] Knoten in _Pixel_ , die den zweiten Index innerhalb des Knotens zweite Koordinate koordinieren verbunden ist y.  

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Definieren eines faltenden für multiklassenklassifizierung: Ziffer Anerkennung wird
Die Definition der folgenden Netzwerk soll Zahlen erkennen und veranschaulicht einige erweiterten Techniken zum Anpassen eines neuronalen Netzwerks.  

    input Image [29, 29];
    hidden Conv1 [5, 13, 13] from Image convolve 
    {
       InputShape  = [29, 29];
       KernelShape = [ 5,  5];
       Stride      = [ 2,  2];
       MapCount    = 5;
    }
    hidden Conv2 [50, 5, 5]
    from Conv1 convolve 
    {
       InputShape  = [ 5, 13, 13];
       KernelShape = [ 1,  5,  5];
       Stride      = [ 1,  2,  2];
       Sharing     = [false, true, true];
       MapCount    = 10;
    }
    hidden Hid3 [100] from Conv2 all;
    output Digit [10] from Hid3 all;  


-   Die Struktur hat eine einzelne Eingabe, _Bild_.
-   **Rollst** Schlüsselwort Gibt an, dass die Ebenen mit den Namen _Conv1_ und _Conv2_ convolutional Ebenen. Jede dieser Deklarationen Ebene folgt eine Liste der Convolution-Attribute.
-   Netz hat eine dritte Ebene _Hid3_ausgeblendet, die vollständig mit der zweiten verborgenen Ebene _Conv2_verbunden ist.
-   Die Ausgabeschicht _Ziffer_ist nur der dritte verborgenen Ebene _Hid3_verbunden. Das Schlüsselwort **all** gibt an, dass die Ausgabeschicht vollständig mit _Hid3_verbunden ist.
-   Der Stelligkeit der Faltung ist drei (die Länge von Tupeln, **InputShape**, **KernelShape**, **Stride**und **Freigabe**). 
-   Die Anzahl der Gewichte pro Kernel ist _1 + **KernelShape**\[0] * * *KernelShape**\[1]* **KernelShape**\[2] = 1 + 1 *5* 5 = 26. Oder 26 * 50 = 1300_.
-   Sie können Knoten im einzelnen ausgeblendeten Ebenen wie folgt berechnen:
    -   **NodeCount**\[0] = (5 - 1) / 1 + 1 = 5.
    -   **NodeCount**\[1] = (13-5) / 2 + 1 = 5. 
    -   **NodeCount**\[2] = (13-5) / 2 + 1 = 5. 
-   Die Gesamtzahl der Knoten mit der deklarierten Dimensionalität der Ebene berechnet [50, 5, 5] wie folgt: _ **MapCount** * * *NodeCount**\[0]* **NodeCount**\[1] * * *NodeCount**\[2] = 10* 5 *5* 5_
-   **Freigabe**[d] ist False für _d == 0_, die Anzahl der Kerne _ **MapCount** * * *NodeCount**\[0] = 10* 5 = 50_. 


## <a name="acknowledgements"></a>Danksagungen

Net-Sprache für die Anpassung der Architektur neuronaler Netze wurde von Shon Katzenberger (Architekt, maschinelles lernen) und Alexey Kamenev (Software Engineer, Microsoft Research) bei Microsoft entwickelt. Sie wird intern für maschinelles lernen, Projekte und Programme bis bilderkennung Textanalyse verwendet. Weitere Informationen finden Sie unter [Neuronale Netze in Azure ML - Net-Einführung](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)


[1]:./media/machine-learning-azure-ml-netsharp-reference-guide/formula_large.gif
 
