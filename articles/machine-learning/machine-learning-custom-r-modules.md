<properties 
    pageTitle="Erstellen Sie benutzerdefinierte R-Module in Azure maschinelles lernen | Microsoft Azure" 
    description="Schnellstart für benutzerdefinierte R-Module in Azure Machine Learning erstellen." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"  
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="tbd" 
    ms.date="08/19/2016" 
    ms.author="bradsev;ankarloff" />


# <a name="author-custom-r-modules-in-azure-machine-learning"></a>Autor benutzerdefinierte R-Module in Azure Machine Learning

Zum Erstellen und Bereitstellen eines benutzerdefinierten Moduls R in Azure Machine Learning beschrieben. Er erklärt benutzerdefinierte R-Module und welche Dateien verwendet werden, definieren. Wie Dateien erstellt, die ein Modul definieren und registrieren Sie das Modul für die Bereitstellung in einem Arbeitsbereich Computerlernen veranschaulicht. Die Elemente und Attribute in der Definition des benutzerdefinierten Moduls werden ausführlich beschrieben. Auch wird erläutert, wie Sie zusätzliche Funktionen und Dateien sowie mehrere Ausgaben. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="what-is-a-custom-r-module"></a>Was ist ein benutzerdefiniertes Modul F?

Ein **benutzerdefiniertes Modul** ist ein benutzerdefiniertes Modul, die zu Ihrem Workspace hochgeladen und als Teil des Experiments Azure Machine Learning ausgeführt werden kann. **Benutzerdefinierte R-Modul** ist ein benutzerdefiniertes Modul, das eine benutzerdefinierte R-Funktion ausgeführt wird. **R** ist eine Programmiersprache für statistische Berechnung und Grafiken Statistiker und Daten Wissenschaftler verbreitet ist für die Implementierung von Algorithmen. R ist derzeit die einzige Sprache unterstützt benutzerdefinierte Module, aber Unterstützung für weitere Sprachen ist für zukünftige Versionen geplant.

Benutzerdefinierte Module sind **erstrangige** in Azure Machine Learning insofern, als sie wie andere Modul verwendet werden können. Sie können mit anderen Modulen in veröffentlichten Experimente oder Visualisierung ausgeführt werden. Sie haben die Kontrolle über das Modul die Eingabe und Ausgangsports verwendet werden, Modellierung Parameter und andere Laufzeit Verhaltensweisen implementierte Algorithmus. Versuch, die benutzerdefinierte Module kann auch in Cortana Intelligence Gallery Druckausgabe veröffentlicht.


## <a name="files-in-a-custom-r-module"></a>Dateien in ein benutzerdefiniertes R-Modul

Benutzerdefiniertes R-Modul ist eine ZIP-Datei definiert, die mindestens zwei Dateien enthält:

* Eine **Datei** , die vom Modul R-Funktion implementiert
* Eine **XML-Definitionsdatei** , die benutzerdefinierte Schnittstelle beschreibt

Weitere Hilfsdateien können auch in der ZIP-Datei enthalten, die Funktionen bereitstellt, die das benutzerdefinierte Modul zugegriffen werden kann. Diese Option wird im Bereich **Argumente** der Referenzabschnitt **Elemente in der XML-Definitionsdatei** Schnellstart-Beispiel erläutert.


## <a name="quickstart-example-define-package-and-register-a-custom-r-module"></a>Schnellstart-Beispiel: definieren, Verpacken und Registrierung eines benutzerdefinierten Moduls R

In diesem Beispiel wird veranschaulicht, wie erstellen ein benutzerdefiniertes Modul R erforderlichen Dateien in eine Zip-Datei packen und registrieren Sie das Modul im Arbeitsbereich Computerlernen. Die Zip-Paket und Beispiel Beispieldateien können [CustomAddRows.zip Download-Datei](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409)heruntergeladen werden.

## <a name="the-source-file"></a>Die Quelldatei
Beispiel eines **Benutzerdefinierten Add Rows** -Moduls, die standardmäßige Implementierung des Moduls **Fügen Sie Zeilen** zum Verketten von Zeilen (Messwerte) aus zwei Datasets (Datenrahmen) ändert. **Add Rows** Standardmodul fügt Zeilen des zweiten Eingabedatasets an das Ende der ersten Eingabe-Dataset mit den `rbind` Algorithmus. Die angepasste `CustomAddRows` Funktion ebenso akzeptiert zwei Datasets, sondern auch einen booleschen Swap-Parameter als zusätzliche Eingabe akzeptiert. Wenn der Swap-Parameter auf **FALSE**festgelegt ist, gibt dieselben Daten wie die standardmäßige Implementierung zurück. **Stimmt der Swap-Parameter**die Funktion hängt jedoch Zeilen der ersten Eingabedatasets bis zum Ende des zweiten Dataset stattdessen. CustomAddRows.R-Datei mit der Implementierung der R `CustomAddRows` Funktion vom **Benutzerdefinierten Add Rows** -Modul hat den folgenden R-Code.

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) 
    {
        if (swap)
        {
            return (rbind(dataset2, dataset1));
        }
        else
        {
            return (rbind(dataset1, dataset2));
        } 
    } 

### <a name="the-xml-definition-file"></a>Der XML-Definitionsdatei
Bereitgestellt `CustomAddRows` Funktion als Modul Azure Machine Learning, einer XML-Datei an das Modul **Benutzerdefinierte Add Rows** Aussehen und Verhalten sollte erstellt werden. 

    <!-- Defined a module using an R Script -->
    <Module name="Custom Add Rows">
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another. Dataset 2 is concatenated to Dataset 1 when Swap is FALSE, and vice versa when Swap is TRUE.</Description>
    
    <!-- Specify the base language, script file and R function to use for this module. -->      
        <Language name="R" 
         sourceFile="CustomAddRows.R" 
         entryPoint="CustomAddRows" />  
        
    <!-- Define module input and output ports -->
    <!-- Note: The values of the id attributes in the Input and Arg elements must match the parameter names in the R Function CustomAddRows defined in CustomAddRows.R. -->
        <Ports>
            <Input id="dataset1" name="Dataset 1" type="DataTable">
                <Description>First input dataset</Description>
            </Input>
            <Input id="dataset2" name="Dataset 2" type="DataTable">
                <Description>Second input dataset</Description>
            </Input>
            <Output id="dataset" name="Dataset" type="DataTable">
                <Description>The combined dataset</Description>
            </Output>
        </Ports>
        
    <!-- Define module parameters -->
        <Arguments>
            <Arg id="swap" name="Swap" type="bool" >
                <Description>Swap input datasets.</Description>
            </Arg>
        </Arguments>
    </Module>

 
Es ist wichtig zu beachten, dass der Wert der **ID-** Attribute in der XML-Datei Elemente **ein-** und **Arg** Parameternamen Funktion R Code in die Datei CustomAddRows.R genau übereinstimmen: (*dataset1* *Datensatz2*und *Swap* im Beispiel). Auch der Wert des Attributs **EntryPoint** **Sprachelement** muss der Name der Funktion in das Skript R EXAKT übereinstimmen: (im Beispiel*CustomAddRows* ). 

Im Gegensatz dazu entspricht das **ID-** Attribut für das **Output** -Element keine Variablen im Skript R. Wenn mehrere erforderlich ist, einfach eine Liste Zurückgeben von R-Funktion Ergebnisse *in der Reihenfolge* platziert, **Ausgaben** Elemente in der XML-Datei deklariert sind.

### <a name="package-and-register-the-module"></a>Packen Sie und registrieren Sie das Modul
Speichern Sie diese beiden Dateien *CustomAddRows.R* und *CustomAddRows.xml* , und komprimieren Sie die beiden Dateien zusammen in einer *CustomAddRows.zip* -Datei.

Registrieren sie im Arbeitsbereich Computerlernen, Gehe zu Arbeitsbereich Machine Learning Studio auf **+ neue** Schaltfläche unten und wählen Sie das neue **Benutzerdefinierte Add Rows** Modul hochladen **Modul -> aus ZIP-Paket** .

![Zip hochladen](./media/machine-learning-custom-r-modules/upload-from-zip-package.png)

**Benutzerdefinierte Add Rows** -Modul kann jetzt maschinelles lernen Experimente zugegriffen werden.


## <a name="elements-in-the-xml-definition-file"></a>Elemente in der XML-Definitionsdatei

### <a name="module-elements"></a>Modulelemente
Das **Module** -Element wird zum Definieren eines benutzerdefinierten Moduls in der XML-Datei. Mehrere Module können in einer XML-Datei mehrere **Module** Elemente definiert werden. Jedes Modul in Ihrem Arbeitsbereich muss einen eindeutigen Namen haben. Registrieren ein benutzerdefiniertes Modul mit demselben Namen wie eine vorhandene benutzerdefinierte Modul und das vorhandene Modul durch den neuen ersetzt. Benutzerdefinierte Module können jedoch denselben Namen wie ein vorhandenes Azure Machine Learning-Modul registriert. In diesem Fall werden sie in der Kategorie **benutzerdefinierte** Modul Palette angezeigt.

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another...</Description>/> 


Im **Module** -Element können Sie zwei weitere optionale Elemente angeben:

- **in dem Modul eingebettet Besitzerelement**  
- ein **Description** -Element, das Text enthält, der in Schnellhilfe für das Modul und Mauszeiger das Modul Machine Learning-Benutzeroberfläche angezeigt wird.


Regeln für Zeichen Grenzwerte in den Modul-Elementen:

* Der Wert des **Name** -Attributs im **Module** -Element darf 64 Zeichen nicht überschreiten. 
* Der Inhalt des Elements **Beschreibung** darf 128 Zeichen nicht überschreiten.
* Der Inhalt des Elements **Besitzer** darf 32 Zeichen nicht überschreiten.


Ein Modul nur deterministisch oder nondeterministic.* * standardmäßig, alle Module werden als deterministisch sein. Also sollte einen unveränderlichen Satz von Eingabeparametern und Daten, das Modul zurück die gleichen Ergebnisse EacRAND oder ein Functionh Mal ausgeführt wird. Dieses Verhalten, Azure Machine Learning Studio führt nur deterministisch, wenn ein Parameter als Module oder eingegebenen Daten wurde geändert. Die zwischengespeicherten Ergebnisse bietet auch viel schnellere Durchführung von Experimenten.

Gibt es Funktionen, nicht deterministische RAND oder eine Funktion, die das aktuelle Datum oder die Uhrzeit zurückgibt. Wenn das Modul eine nicht deterministische Funktion verwendet, können Sie angeben, dass das Modul nicht deterministisch ist der optionale **IsDeterministic** auf **FALSE**. Dadurch wird sichergestellt, dass das Modul erneut ausgeführt wird Wenn das Experiment ausgeführt wird, auch wenn das Modul input und Parameter nicht geändert haben. 

### <a name="language-definition"></a>Sprachdefinition
**Sprachelement in der XML-Definitionsdatei** wird zum benutzerdefinierten Modul Sprache angeben. R ist derzeit die einzige unterstützte Sprache. Der Wert des Attributs **SourceFile** muss den Namen R-Datei sein, die Funktion des Moduls ausgeführt wird enthält. Diese Datei muss im Zip-Paket. Der Wert des Attributs **EntryPoint** ist der Name der aufgerufenen Funktion und muss eine gültige Funktion in der Quelldatei definiert.

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### <a name="ports"></a>Anschlüsse
Die Eingabe- und Ports für ein benutzerdefiniertes Modul werden in untergeordneten Elementen des Abschnitts **Ports** der XML-Datei angegeben. Die Reihenfolge der Elemente bestimmt das Layout erfahrenen (UX) Benutzer. Die erste untergeordnete **Eingabe** oder **Ausgabe** im **Ports** Bestandteil der XML-Datei wird links-Eingangs-Port im Computer Learning UX
Jede Eingabe und Ausgangs-Port kann ein optionales **Beschreibung** untergeordnetes Element, das Text angezeigt, wenn den Mauszeiger über den Anschluss der Computer lernen Benutzeroberfläche bewegen angibt.

**Regeln für die Ports**:

* Maximale Anzahl von **Eingabe- und Ports** ist 8 für jeden.

### <a name="input-elements"></a>Input-Elemente
Eingänge können Sie Daten auf Arbeitsbereich und R-Funktion übergeben. Die **Datentypen** , die für Eingänge wie folgt unterstützt werden: 

**DataTable:** Dieser Typ wird als eine data.frame der R-Funktion übergeben. Tatsächlich werden Typen (z. B. CSV-Dateien oder ARFF-Dateien) von Computer Learning unterstützt werden und sind kompatibel mit **DataTable** automatisch in ein data.frame konvertiert. 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
        </Input>

Jeden Eingabeanschluss **DataTable** zugeordnete **Id** -Attribut muss einen eindeutigen Wert, und dieser Wert muss entsprechenden benannten Parameters der R-Funktion entsprechen.
Optionale **DataTable** -Ports, die nicht übergeben werden, wie in einem Experiment haben den Wert **NULL** an R-Funktion und optionale Zip-Ports werden ignoriert, wenn die Eingabe nicht verbunden ist. **IsOptional** -Attribut ist optional für **DataTable** und **Zip** und Standardwert ist *false* .
       
**Komprimieren:** Benutzerdefinierte Module können eine Zip-Datei als Eingabe akzeptiert. Diese Eingabe ist in das Arbeitsverzeichnis R Ihrer Funktion entpackt.

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files to be extracted to the R working directory.</Description>
        </Input>

Benutzerdefinierte R-Module Id für Zip-Anschluss keinen Parameter R Funktion übereinstimmen. Ist die Zip-Datei in das Arbeitsverzeichnis R automatisch extrahiert wird.

**Regeln für die Eingabe:**

* Der Wert des **Id** -Attributs des **Input** -Elements muss gültiger Variablenname R.
* Wert des **Id** -Attributs des **Input** darf nicht länger als 64 Zeichen sein.
* Wert des **Name** -Attributs des **Input** darf nicht länger als 64 Zeichen sein.
* Der Inhalt des Elements **Beschreibung** darf nicht länger als 128 Zeichen sein.
* Der Wert des **Type** -Attributs des **Input** -Elements muss *ZIP-* oder *DataTable*.
* Der Wert des Attributs **IsOptional** das **Input** -Element muss nicht (und Standardwert ist *false,* Wenn nicht angegeben) aber wenn es angegeben wird, muss es *true* oder *false*.

### <a name="output-elements"></a>Ausgabeelemente

**Standard Ausgangsports:** Ausgänge sind die Rückgabewerte von R-Funktion zugeordnet, die durch nachfolgende Module verwendet werden kann. *DataTable* ist nur Standardausgabe Anschluss zurzeit unterstützt. (Unterstützung für *Lernende* und *transformiert* wird demnächst.) Eine *DataTable* -Ausgabe ist wie folgt definiert:

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

Für Ausgaben in benutzerdefinierte R-Module der Wert des **ID-** Attributs muss nicht mit dem R-Skript entsprechen muss eindeutig sein. Für eine Ausgabe-Modul muss der Rückgabewert der Funktion R *data.frame*. Um mehrere Objekte einen unterstützten Datentyp ausgeben, die Ausgabe-Ports in der XML-Definitionsdatei angegeben werden müssen und die Objekte als Liste zurückgegeben werden müssen. Die Ausgabeobjekte zugewiesen Ausgangsports von links nach rechts spiegeln die Reihenfolge in der die Objekte in der zurückgegebenen Liste platziert werden.

Beispielsweise möchten Sie **Benutzerdefinierte Add Rows** Modul ursprünglichen zwei Datasets Ausgabe ändern, *dataset1* und *Datensatz2*neben neuen verknüpft Dataset *Dataset*(in Reihenfolge von links nach rechts als: *Datasets*, *dataset1*, *Datensatz2*), Ausgangsports wird folgendermaßen in der Datei CustomAddRows.xml definiert:

    <Ports> 
        <Output id="dataset" name="Dataset Out" type="DataTable"> 
            <Description>New Dataset</Description> 
        </Output> 
        <Output id="dataset1_out" name="Dataset 1 Out" type="DataTable"> 
            <Description>First Dataset</Description> 
        </Output> 
        <Output id="dataset2_out" name="Dataset 2 Out" type="DataTable"> 
            <Description>Second Dataset</Description> 
        </Output> 
        <Input id="dataset1" name="Dataset 1" type="DataTable"> 
            <Description>First Input Table</Description>
        </Input> 
        <Input id="dataset2" name="Dataset 2" type="DataTable"> 
            <Description>Second Input Table</Description> 
        </Input> 
    </Ports> 


Und die Liste der Objekte in einer Liste in der richtigen Reihenfolge in 'CustomAddRows.R':

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 
    
**Visualisierung Ausgabe:** Sie können auch einen Ausgabeanschluss Typ *Visualisierung*, die Ausgabe von R Grafikgerät und Ausgabe in der Konsole angezeigt. Dieser Port ist nicht Teil der R-Funktion Ausgabe und beeinträchtigt nicht die Reihenfolge der anderen Ausgabe Porttypen. Um eine Visualisierung Port benutzerdefinierte Module hinzufügen, fügen Sie **ein Ausgabeelement Wert *Visualisierung* für dessen **Type** -Attribut** :

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View the R console graphics device output.</Description>
    </Output>
    
**Ausgabe-Regeln:**

* Der Wert der **ID-** Attribut des **Ausgabeelements** muss ein gültiger Variablenname R.
* Der Wert der **ID-** Attribut des **Ausgabeelements** muss nicht länger als 32 Zeichen sein.
* Wert des **Name** -Attributs des **Ausgabeelements** darf nicht länger als 64 Zeichen sein.
* Der Wert des **Type** -Attributs des **Ausgabeelements** muss *Visualisierung*.

### <a name="arguments"></a>Argumente
Zusätzliche Daten können über Parameter für das Modul im **Argumente** -Element definiert, der R-Funktion übergeben werden. Diese Parameter werden im rechten Eigenschaften der Computer lernen Benutzeroberfläche Anwahl des Moduls. Argumente werden die unterstützten Typen oder bei Bedarf eine benutzerdefinierte Enumeration erstellen. Wie die **Ports** Elemente **Argumente** Elemente ein optionales Element **Beschreibung** haben, das den Text, der angezeigt wird angibt, wenn die Maus über den Parameternamen.
Optionale Eigenschaften für ein Modul DefaultValue MinValue und MaxValue können ein Argument **Eigenschaften** Element Attribute hinzugefügt werden. Gültige Eigenschaften für das Element **Eigenschaften** hängen Argument und mit den unterstützten Argumenttypen im nächsten Abschnitt beschrieben. Argumente die **IsOptional** -Eigenschaft auf **"true"** erfordern keine Benutzer einen Wert eingeben. Wenn das Argument kein Wert bereitgestellt wird, wird das Argument nicht an die Einstiegspunktfunktion übergeben. Argumente die Einstiegspunktfunktion optionale Funktion z. B. in der Funktionsdefinition Eintrag zeigen den Standardwert NULL zugewiesen explizit behandelt werden müssen. Ein optionales Argument erzwingt nur andere Argument Nebenbedingungen min bzw. Max, wenn Wert vom Benutzer bereitgestellt wird.
Wie bei Eingaben und Ausgaben ist es wichtig, dass die Parameter UID-Werte zugeordnet. In unserem ersten Beispiel wurde der zugeordnete Id-Parameter *austauschen*.

### <a name="arg-element"></a>Arg-element
Das untergeordnete Element **Arg** **Argumente** -Abschnitt der XML-Datei mit ein Modulparameter definiert. Definiert mit untergeordneten Elementen im Abschnitt **Ports** die Reihenfolge der Parameter im **Argumente** -Abschnitt Layout in der UX gefunden Die Parameter werden von oben nach unten in der Benutzeroberfläche in der Reihenfolge, in der die XML-Datei definiert sind. Indem Computer für Parameter unterstützt werden hier aufgelistet. 

**Int** -eine Ganzzahl (32 Bit)-Typparameter.

    <Arg id="intValue1" name="Int Param" type="int">
        <Properties min="0" max="100" default="0" />
        <Description>Integer Parameter</Description>
    </Arg>


* *Optionale Eigenschaften*: **min**, **max**, **Standard** und **IsOptional**

**double** -Typ double-Parameter.

    <Arg id="doubleValue1" name="Double Param" type="double">
        <Properties min="0.000" max="0.999" default="0.3" />
        <Description>Double Parameter</Description>
    </Arg>


* *Optionale Eigenschaften*: **min**, **max**, **Standard** und **IsOptional**

**Bool** – einen booleschen Parameter, die durch ein Kontrollkästchen im UX dargestellt

    <Arg id="boolValue1" name="Boolean Param" type="bool">
        <Properties default="true" />
        <Description>Boolean Parameter</Description>
    </Arg>



* *Optionale Eigenschaften*: **Standard** - false nicht festgelegt

**Zeichenfolge**: eine Standardzeichenfolge

    <Arg id="stringValue1" name="My string Param" type="string">
        <Properties isOptional="true" />
        <Description>String Parameter 1</Description>
    </Arg>  

* *Optionale Eigenschaften*: **Standard** und **IsOptional**

**ColumnPicker**: Parameter Auswahl Spalte. Dieser Typ macht die UX als eine Spaltenauswahl. **Das Eigenschaftenelement** dient hier die Kennung des Anschlusses angeben, in der Spalten ausgewählt werden, der Port Zieltyp *DataTable*sein muss. Das Ergebnis der ausgewählten Spalte wird als eine Liste von Zeichenfolgen mit den Namen der ausgewählten Spalte R-Funktion übergeben. 

        <Arg id="colset" name="Column set" type="ColumnPicker">   
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* *Erforderliche Eigenschaften*: **PortId** - Id eines Input-Elements mit *DataTable*entspricht.
* *Optionale Eigenschaften*:
    * **AllowedTypes** - Filter Spaltentypen, die Sie auswählen können. Gültige Werte sind: 
        *   Numerische
        *   Boolescher Wert
        *   Kategorieliste
        *   Zeichenfolge
        *   Bezeichnung
        *   Funktion
        *   Bewertung
        *   Alle

    * **Standard** - sind gültige Standardauswahl für die Spaltenauswahl: 
        * Keine
        * NumericFeature
        * NumericLabel
        * NumericScore
        * NumericAll
        * BooleanFeature
        * BooleanLabel
        * BooleanScore
        * BooleanAll
        * CategoricalFeature
        * CategoricalLabel
        * CategoricalScore
        * CategoricalAll
        * StringFeature
        * StringLabel
        * StringScore
        * StringAll
        * AllLabel
        * AllFeature
        * AllScore
        * Alle

                                                        
**DropDown**: eine benutzerdefinierte aufgelisteten () Dropdownliste. Dropdown-Elemente werden in **Eigenschaften** Element mithilfe der **Item** -Elements angegeben. **Id** für jedes **Element** muss eindeutig sein und eine gültige R. Der Wert der **Name** eines **Elements** ist der Text, der und der Wert, der R-Funktion übergeben.

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>  

* *Optionale Eigenschaften*:
    * **Standard** - muss der Wert für die Eigenschaft Standardwert mit der Id eines **Elements** Elemente entsprechen.


### <a name="auxiliary-files"></a>Hilfsdateien

Eine Datei, die in Ihrer benutzerdefinierten Moduls ZIP-Datei platziert wird während der Ausführungszeit werden. Alle Verzeichnisstrukturen erhalten. Daher funktioniert sourcing Datei gleich lokal und Azure Machine Learning Ausführung. 

> [AZURE.NOTE] Beachten Sie, dass alle Pfade müssen alle "Src" Verzeichnis extrahiert werden ' Src /' Präfix.

Beispielsweise Zeilen mit NAs aus dem Dataset entfernen und auch alle doppelten Zeilen entfernen, bevor Sie in CustomAddRows ausgeben möchten, und Sie haben bereits eine R-Funktion, die das in einer Datei RemoveDupNARows.R geschrieben:

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
Sie können die Hilfsdatei RemoveDupNARows.R in der Funktion CustomAddRows Quelle:

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) {
        source("src/RemoveDupNARows.R")
            if (swap) { 
                dataset <- rbind(dataset2, dataset1))
            } else { 
                dataset <- rbind(dataset1, dataset2)) 
            } 
        dataset <- removeDupNARows(dataset)
        return (dataset)
    }

Anschließend Uploaden einer Zip-Datei mit 'CustomAddRows.R', 'CustomAddRows.xml' und 'RemoveDupNARows.R' als benutzerdefinierte R-Modul.


## <a name="execution-environment"></a>Umgebung

Die Umgebung für das R-Skript verwendet die gleiche Version von R als Skriptmodul **Ausführen R** und kann die gleichen Standardpakete. Sie können ein Modul auch zusätzliche R-Pakete hinzufügen, durch Einschließen in benutzerdefinierten Moduls Zip-Paket. Nur Laden Sie im R-Skript wie in Ihrer eigenen Umgebung R. 

**Nachteile der Umgebung** umfassen:

* Nicht persistent Dateisystem: Dateien bei der Ausführung des benutzerdefinierten Moduls über mehrere Läufe desselben Moduls nicht beibehalten werden.
* Kein Netzwerkzugriff



 
