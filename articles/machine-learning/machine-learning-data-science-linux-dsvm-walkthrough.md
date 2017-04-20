<properties 
    pageTitle="Datenwissenschaft auf Daten Wissenschaft virtuellen Linux-Maschine | Microsoft Azure" 
    description="Wie verschiedene allgemeine Daten Wissenschaft mit Linux Daten Wissenschaft VM auszuführen." 
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
    ms.author="bradsev;paulsh" />


# <a name="data-science-on-the-linux-data-science-virtual-machine"></a>Datenwissenschaft auf Daten Wissenschaft virtuellen Linux-Maschine

Diese exemplarische Vorgehensweise veranschaulicht die mehrere allgemeine Daten Wissenschaft mit Linux Daten Wissenschaft VM auszuführen. Linux Daten Wissenschaft virtuellen Computer (DSVM) ist ein VM auf Azure mit einer Sammlung von Tools, die häufig zur Datenanalyse und maschinelles lernen vorinstalliert ist. Die wichtigsten Software-Komponenten werden im Thema [Bereitstellung Daten Wissenschaft virtuellen Linux-Maschine](machine-learning-data-science-linux-dsvm-intro.md) aufgeschlüsselt. VM-Image macht einfach datenwissenschaft in Minuten ohne Installation und Konfiguration der Tools einzeln ausführen. Sie können problemlos die VM bei Bedarf skalieren und nicht beenden. Diese Ressource ist flexible und kostengünstige. 

Die Wissenschaft Aufgaben veranschaulicht in dieser exemplarischen Vorgehensweise führen Sie die Schritte im [Team wissenschaftliche Daten](https://azure.microsoft.com/documentation/learning-paths/data-science-process/). Dieser Prozess bietet einen systematischen Ansatz zur datenwissenschaft, die Teams Wissenschaftler Daten über den Lebenszyklus erstellen intelligente Clientanwendungen Zusammenarbeit ermöglicht. Wissenschaftliche Daten bietet auch eine iterative Framework für datenwissenschaft, die eine Person folgen kann.

Wir analysieren das [Spambase](https://archive.ics.uci.edu/ml/datasets/spambase) Dataset in dieser exemplarischen Vorgehensweise. Dies ist ein Satz von e-Mails als Spam oder Schinken (d. h., sie sind nicht Spam) gekennzeichnet und enthält einige Statistiken über den Inhalt der e-Mail. Die Statistiken werden in der nächsten aber einen Abschnitt erläutert. 


## <a name="prerequisites"></a>Erforderliche Komponenten

Vor der Verwendung einer Daten Wissenschaft virtuellen Linux-Maschine benötigen Sie Folgendes:

- Ein **Azure-Abonnement**. Wenn Sie nicht bereits eine haben, finden Sie unter [erstellen Ihre kostenlose Azure heute](https://azure.microsoft.com/free/).
- [**Linux datenwissenschaft VM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). Informationen zu dieser VM-Bereitstellung finden Sie unter [Bereitstellung Daten Wissenschaft virtuellen Linux-Maschine](machine-learning-data-science-linux-dsvm-intro.md). 
- [X2Go](http://wiki.x2go.org/doku.php) auf Ihrem Computer installiert und eine XFCE-Sitzung geöffnet. Informationen zur Installation und Konfiguration einer **X2Go Client**finden Sie unter [Installieren und Konfigurieren von X2Go-Client](machine-learning-data-science-linux-dsvm-intro.md#Installing-and-configuring-X2Go-client). 
- Ein **AzureML Konto**. Haben Sie bereits eine für neue [AzureML Homepage](https://studio.azureml.net/)anmelden. Gibt es einen kostenlosen Verwendung Tier zum Einstieg.


## <a name="download-the-spambase-dataset"></a>Das Dataset Spambase herunterladen

[Spambase](https://archive.ics.uci.edu/ml/datasets/spambase) Dataset ist eine relativ kleine Gruppe, nur 4601 Beispiele enthält. Dies ist eine Größe, dass einige der Hauptfeatures der VM wissenschaftliche Daten wie hält den Ressourcenbedarf bescheidene können.

>[AZURE.NOTE] In dieser exemplarischen Vorgehensweise wurde auf eine D2 v2 Größe Daten Wissenschaft virtuellen Linux-Maschine. Diese Größe DSVM kann in dieser exemplarischen Vorgehensweise.

Benötigen Sie mehr Speicherplatz, können Sie zusätzliche Laufwerke erstellen sowie VM zuordnen. Diese Datenträger verwenden Azure Speicherung, damit ihre Daten gewahrt bleibt, selbst wenn der Server aufgrund der Größe ausgebessert oder wird heruntergefahren. Um einen Datenträger hinzufügen und die VM an, führen Sie die Schritte [einer Linux VM Datenträger hinzufügen](../virtual-machines/virtual-machines-linux-add-disk.md). Diese Schritte verwenden der Azure-Befehlszeilenschnittstelle (Azure CLI), die auf der DSVM installiert. Damit diese Verfahren vollständig aus der VM selbst erfolgt. Eine weitere Option zur Erhöhung des Speichers ist [Azure Dateien](../storage/storage-how-to-use-files-linux.md)verwenden.

Daten herunterladen, öffnen ein terminal-Fenster und führen Sie diesen Befehl:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

Die Datei eine Kopfzeile keinen also eine andere Datei erstellen, die einen Header verfügt. Führen Sie diesen Befehl, um eine Datei mit der entsprechenden Header erstellen:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

Verketten Sie die beiden Dateien zusammen mit dem Befehl:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

Das Dataset hat verschiedene Statistiken für jede e-Mail: 

- Spalten wie ***Word\_x\_WORD*** den Prozentwert der Wörter in der e-Mail, die *WORD*entsprechen. Beispielsweise wenn *Word\_x\_stellen* ist 1, dann 1 % aller Wörter in der e-Mail *Stellen*. 
- Spalten wie ***Char\_x\_CHAR*** Prozent aller Zeichen in der e-Mail, die *Zeichen*anzugeben. 
- ***Kapital\_ausführen\_Länge\_längste*** die längste Länge einer Folge von Großbuchstaben. 
- ***Kapital\_ausführen\_Länge\_durchschnittlichen*** die durchschnittliche Länge aller Sequenzen von Großbuchstaben. 
- ***Kapital\_ausführen\_Länge\_total*** die Gesamtlänge aller Sequenzen von Großbuchstaben. 
- ***Spam*** gibt an, ob die e-Mail Spam angesehen wurde (1 = Spam, 0 = kein Spam).


## <a name="explore-the-dataset-with-microsoft-r-open"></a>Untersuchen Sie das Dataset mit Microsoft R öffnen

Wir prüfen die Daten und einige grundlegende Computer mit R. VM wissenschaftliche Daten im Lieferumfang von [Microsoft R öffnen](https://mran.revolutionanalytics.com/open/) vorinstalliert. Multithreaded mathematischen Bibliotheken in dieser Version von R bieten eine bessere Leistung als Singlethread-Varianten. Microsoft R öffnen hinaus Vergleichbarkeit mit Snapshot CRAN-Paket-Repository.

Um die Codebeispiele in dieser exemplarischen Vorgehensweise erhalten, Klonen Sie mit Git auf die VM **Azure-Machine-Learning-Daten-Science** -Repository. Aus der Git Befehlszeile ausführen:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Öffnen Sie ein Terminalfenster und anfangen Sie F R interaktive Konsole.

>[AZURE.NOTE] Sie können auch RStudio für die folgenden Verfahren. Um RStudio zu installieren, führen Sie diesen Befehl auf einem Terminal:`./Desktop/DSVM\ tools/installRStudio.sh`

Importieren Sie die Daten der Umgebung einrichten und ausführen:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

Zusammenfassende Statistiken zu jeder Spalte anzeigen:

    summary(data)

Eine andere Ansicht der Daten:

    str(data)

Dies zeigt den Typ der einzelnen Variablen und die ersten Werte im Dataset. 

*Spam* -Spalte als Ganzzahl gelesen wurde, aber es ist tatsächlich eine kategoriale Variable (oder Faktor). Den Typ festlegen:

    data$spam <- as.factor(data$spam)

Verwenden Sie einige Analyse das [ggplot2](http://ggplot2.org/) -Paket beliebte Diagrammbibliothek r auf dem virtuellen Computer installiert ist. Hinweis der zusammengefassten Daten angezeigt, haben wir zusammenfassende Statistiken über die Häufigkeit der Ausrufezeichen Zeichen. Wir zeichnen die Frequenzen mit folgenden Befehlen:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Da NULL Bar Plots neigen, entfernen Sie sie:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Eine nicht-triviale Dichte interessantes 1 ist. Nur die Daten betrachten:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Teilen Sie es dann von Spam Vs Ham:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

Diese Beispiele sollten ähnliche Flächen der anderen Spalten zu den darin enthaltenen Daten zu aktivieren.


## <a name="train-and-test-an-ml-model"></a>Trainieren und Testen eines Modells ML

Jetzt trainieren wir ein paar Learning Modelle e-Mails im Dataset enthält Span oder Schinken klassifizieren. Wir schulen einem Entscheidungsbaummodell und zufällige Gesamtstrukturmodell in diesem Abschnitt und Testen Sie ihre Genauigkeit ihrer Vorhersagen. 

>[AZURE.NOTE] Im folgenden Code verwendete Rpart (Rekursive Partitionierung und Regressionsstrukturen)-Paket ist auf Daten Wissenschaft VM installiert.


Zunächst teilen wir in Schulung und legt das Dataset:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

Und erstellen Sie eine Entscheidungsstruktur, um e-Mails zu klassifizieren.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Hier ist das Ergebnis:

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

Ermitteln, wie gut im Trainingssatz führt, verwenden Sie den folgenden Code:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Um zu bestimmen, wie auf dem Test führt:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Versuchen Sie eine zufällige Gesamtstrukturmodell auch aus. Zufällige Wälder Schulen eine Vielzahl von Entscheidungsstrukturen und Ausgabe eine Klasse, die die Klassifizierung aller einzelnen Entscheidungsbäume ist. Sie bieten einen leistungsfähigeren Computer lernen, wie sie die Tendenz einem Entscheidungsbaummodell Trainings-Dataset anzahlbasierte korrigieren. 

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-ml"></a>Bereitstellen eines Modells Azure ml

[Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) ist ein Clouddienst, der leicht erstellen und Bereitstellen von Vorhersageanalysen Modelle. Einer der Vorteile des AzureML besteht darin, alle R-Funktion als Webdienst veröffentlichen. AzureML R-Paket erleichtert die Bereitstellung direkt aus unserer R-Sitzung auf der DSVM. 

Zum Bereitstellen des Entscheidung Struktur Codes aus dem vorherigen Abschnitt müssen Sie Azure Machine Learning Studio anmelden. Sie benötigen die Arbeitsplatz-ID und Authentifizierungstoken in Seufzer. Diese Werte und Initialisieren von Variablen AzureML mit:

**Wählen Sie auf der linken Seite.** Beachten Sie die **Arbeitsplatz-ID**. ![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)

Menü **Autorisierungstoken** Overhead und beachten die- **Primäre Autorisierung Token**. ![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)

Laden Sie das **AzureML** -Paket und anschließend Werte der Variablen mit der Token und Arbeitsbereich in der R-Sitzung für die DSVM:


    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Wir vereinfachen des Modells zu dieser Demo einfacher zu implementieren. Wählen Sie die drei Variablen in der Entscheidungsstruktur Stamm am nächsten, und erstellen Sie eine neue Struktur mit nur drei Variablen:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

Wir benötigen eine Vorhersagefunktion, die Funktionen als Eingabe und gibt die vorhergesagten Werte zurück:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

AzureML mit der **PublishWebService** -Funktion veröffentlichen Sie die Funktion PredictSpam: 

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

Diese Funktion hat die **PredictSpam** -Funktion erstellt einen Webdienst namens **SpamWebService** mit definierten Eingaben und Ausgaben, und gibt Informationen zu den neuen Endpunkt.

Detailansicht veröffentlichten Webdienst, einschließlich API-Endpunkt und Zugriffstasten mit dem Befehl:

    spamWebService[[2]]

Legen zum Ausprobieren der ersten 10 Zeilen des Tests:

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Andere verfügbare Tools verwenden

Die übrigen Abschnitte Verwendungsmöglichkeiten von Tools auf Linux Daten Wissenschaft VM installiert. Hier ist die Liste der Tools beschrieben:

- XGBoost
- Python
- Jupyterhub
- Klappern
- PostgreSQL & Eichhörnchen SQL
- SQL Server Datawarehouse


## <a name="xgboost"></a>XGBoost

[XGBoost](https://xgboost.readthedocs.org/en/latest/) ist ein Tool, das eine schnelle und genaue stärkere Struktur Implementierung bereitstellt.

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

XGBoost kann auch Python oder eine Befehlszeile aufrufen.

## <a name="python"></a>Python

Für die Entwicklung mit Python wurden Anaconda Python-Distributionen 2.7 und 3.5 in der DSVM installiert. 

>[AZURE.NOTE] Anaconda Verteilung enthält [Condas](http://conda.pydata.org/docs/index.html), dem benutzerdefinierten Umgebung für Python erstellen, unterschiedliche Versionen und Pakete installiert werden können.

Wir einige Spambase Dataset lesen und klassifizieren Sie e-Mails mit Maschinen im Scikit Vektor-Informationen:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Vorhersagen:

    clf.predict(X.ix[0:20, :])

Zeigen Sie einen Endpunkt AzureML veröffentlichen lassen einem einfacheren Modell drei Variablen wie wir R Modell bereits veröffentlichte. 

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Das Modell AzureML veröffentlichen:

    # Publish the model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about the resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call the model
    predictSpam.service(1, 1, 1)

>[AZURE.NOTE] Nur für Python 2.7 und noch nicht auf 3.5 unterstützt. Führen Sie mit **/anaconda/bin/python2.7**.


## <a name="jupyterhub"></a>Jupyterhub

Anaconda Verteilung in der DSVM enthält ein Jupyter Notebook eine plattformübergreifende Umgebung Python, R bzw. Julia und Analyse. Jupyter Notebook erfolgt über JupyterHub. Anmeldung mit Ihrem lokalen Linux-Benutzernamen und Kennwort bei ***https://\<VM DNS-Name oder IP-Adresse\>: 8000 /***. Alle Konfigurationsdateien JupyterHub befinden sich im Verzeichnis **/etc/jupyterhub**.

Einige Beispiel-Notebooks sind bereits auf dem virtuellen Computer installiert:

- [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) für ein Beispiel Python-Notebooks anzeigen
- Ein Beispiel **F** Notebook finden Sie unter [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) .
- [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) für ein anderes Beispiel **Python** Notizbuch anzeigen

>[AZURE.NOTE] Julia Sprache steht auch über die Befehlszeile auf Linux Daten Wissenschaft VM.


## <a name="rattle"></a>Klappern

[Klappern](https://cran.r-project.org/web/packages/rattle/index.html) (die R Analytical Tool zu lernen leicht) ist ein Grafiktool R für Datamining. Es verfügt über eine intuitive Benutzeroberfläche, die es einfach, untersuchen und Transformieren von Daten und erstellen und auswerten Modelle.  Artikel [Rattern: ein Data Mining-GUI für R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) enthält eine exemplarische Vorgehensweise, die Funktionen veranschaulicht.

Installieren Sie und starten Sie Klappern mit folgenden Befehlen:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

>[AZURE.NOTE] Auf die DSVM ist keine Installation erforderlich. Aber Klappern möglicherweise aufgefordert, zusätzliche Pakete installieren geladen wird.

Klappern verwendet Registerkarte Benutzeroberfläche. Die meisten Registerkarten entsprechen Schritte im [Wissenschaftlichen Daten](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), wie Daten geladen oder erkunden. Wissenschaft-Prozess fließt von links nach rechts über die Registerkarten. Aber die letzte Registerkarte enthält ein Protokoll Klappern ausführen R-Befehle. 


Laden und Konfigurieren des Datasets:

- Um die Datei zu laden, wählen Sie die Registerkarte **Daten** 
- Wählen Sie den Selektor neben **Dateiname** und **spambaseHeaders.data**. 
- Laden Sie die Datei. Wählen Sie in der obersten Zeile von Schaltflächen **Ausführen** . Sehen Sie eine Zusammenfassung der einzelnen Spalten einschließlich identifizierten Daten, sei es ein Ziel oder anderen Typ und die Anzahl der eindeutigen Werte.
- Klappern wurde **Spam** -Spalte als Ziel erkannt. Wählen Sie unter Spam-Spalte der **Zieldatentyp** auf **Categoric**festgelegt.

Zum Durchsuchen der Daten: 

- Wählen Sie die Registerkarte **Durchsuchen** . 
- Klicken Sie auf **Zusammenfassung**und **Ausführen**, um Informationen über die Variablentypen und zusammenfassenden Statistiken angezeigt. 
- Um andere Arten von Statistiken zu einzelnen Variablen anzuzeigen, wählen Sie weitere Optionen wie **beschreiben** oder **Grundlagen**.

Registerkarte **Explorer** können Sie viele nützliche Flächen erzeugen. So plotten Sie ein Histogramm der Daten


- Wählen Sie die **Verteilung**.
- Überprüfen Sie **Histogramm** für **Word_freq_remove** und **Word_freq_you**.
- Wählen Sie **Ausführen**. Beide Flächen Dichte in einem einzigen Graphfenster sollte, das Wort "Sie" e-Mails ist "gelöscht" häufiger werden angezeigt werden.

Die Korrelation Beobachtungsflächen sind interessant. Erstellen:


- Wählen Sie dann **Korrelation** als **Typ** 
- Wählen Sie **Ausführen**. 
- Rattle warnt Sie maximal 40 Variablen empfiehlt. Wählen Sie **Ja** den Plot anzeigen. 

Es gibt einige interessante Zusammenhänge kommen: "Technologie" steht in engen "HP" und "Labs" angenommen. Es wird auch "650" korreliert, weil der der Geber Dataset 650 ist.

Die numerischen Werte für die Korrelationen zwischen Wörtern stehen im Explorer-Fenster. Es ist interessant, z. B., dass negativ korreliert "Technologie" mit "Ihr" und "Geld".

Klappern verwandeln das Dataset, um einige Probleme zu behandeln. Beispielsweise können Sie Features skalieren unterstellen fehlende Werte, Ausreißer zu behandeln und Variablen oder mit fehlenden Daten entfernen. Rattle kann auch Zuordnungsregeln zwischen Messwerte oder Variablen angeben. Diese Registerkarten werden Gültigkeitsbereich dieser einführenden exemplarischen Vorgehensweise.

Rattle kann auch Cluster-Analyse durchführen. Wir schließen einige Features, die Ausgabe übersichtlicher zu gestalten. Wählen Sie auf der Registerkarte **Daten** **ignorieren** neben Variablen außer diese zehn Elemente:

- word_freq_hp
- word_freq_technology
- word_freq_george
- word_freq_remove
- word_freq_your
- word_freq_dollar
- word_freq_money
- capital_run_length_longest
- word_freq_business
- Spam

Gehen Sie zurück zur Registerkarte **Cluster** wählen Sie **KMeans**und die *Anzahl der Cluster* auf 4 festgelegt. Sie **führen**. Die Ergebnisse werden im Ausgabefenster angezeigt. Ein Cluster hat hohe Frequenz "Georg" und "hp" und ist wahrscheinlich eine legitime geschäftliche e-Mail-Adresse.

So erstellen Sie einen einfachen Rechner Learning Entscheidungsbaummodell: 

- Wählen Sie die Registerkarte **Modell** , 
- Wählen Sie als **Typ** **Struktur** . 
- Wählen Sie **Ausführen** , um die Struktur in Form von Text im Ausgabefenster angezeigt. 
- Wählen Sie **Zeichnen** eine grafisch anzeigen. Das sieht ähnlich der Struktur erhalten wir über *Rpart*.

Einer der Vorteile von Rasseln besteht darin zu Methoden mehrere Rechner schnell auswerten. Hier ist das Verfahren:

- Wählen Sie **Alle** für den **Typ**. 
- Wählen Sie **Ausführen**. 
- Nach Abschluss können Sie auf einzelnen **Typ**, wie **SVM**und die Ergebnisse. 
- Sie können auch die Leistung der Modelle für die Validierung **Auswerten** der Registerkarte festgelegt vergleichen. Beispielsweise zeigt **Fehler Matrix** Auswahl Verwirrung Matrix, allgemeine Fehler und die durchschnittliche Klassenfehler für jedes Modell Überprüfung festlegen. 
- Sie können auch ROC Kurven zeichnen Sensitivitätsanalyse durchführen und andere Arten von Modell Produkttests.

Wenn Sie Modelle haben, wählen Sie die Registerkarte **Protokoll** Klappern während der Sitzung ausführen R-Code anzeigen. Wählen Sie die Schaltfläche **Exportieren** speichern. 

>[AZURE.NOTE] Es ist ein Fehler in der aktuellen Version Rasseln. Um das Skript ändern oder später wiederholen der Schritte verwenden, müssen Sie ein #-Zeichen vor *diesem Protokoll exportieren* im Text des Protokolls einfügen. 


## <a name="postgresql--squirrel-sql"></a>PostgreSQL & Eichhörnchen SQL

Die DSVM kommt mit PostgreSQL installiert. PostgreSQL ist eine anspruchsvolle, Open-Source-relationale Datenbank. Dieser Abschnitt zeigt, wie unser Spam-Dataset in PostgreSQL und dann Abfragen.

Vor dem Laden der Daten müssen Sie Kennwortauthentifizierung vom Localhost zulassen. Befehlszeile:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

Am unteren Rand der Konfigurationsdatei sind mehrere Linien, die zulässigen Verbindungen beschrieben:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

Ändern Sie die Zeile "IPv4-lokale Verbindungen" Verwendung von md5 statt Ident, damit wir mit einem Benutzernamen und Kennwort anmelden können:

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

Und starten Sie den Dienst Postgres:

    sudo systemctl restart postgresql

Starten Sie Psql ein interaktives Terminal PostgreSQL als integrierte Postgres Benutzer führen den folgenden Befehl aus einem:

    sudo -u postgres psql

Erstellen Sie ein neues Benutzerkonto mit demselben Benutzernamen als Linux-Konto sind derzeit angemeldet als und geben ein Kennwort:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Melden Sie sich Psql als Benutzer:

    psql

Und die Daten in eine neue Datenbank importieren:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Nun betrachten Sie die Daten und führen einige Abfragen mit **Eichhörnchen SQL**, einem Grafiktool, mit dem Sie mit Datenbanken über einen JDBC-Treiber interagieren.

Starten Sie zunächst Eichhörnchen SQL aus der. So richten den Treiber

- Wählen Sie **Windows**anschließend **Treiber anzeigen**. 
- Maustaste auf **PostgreSQL** und wählen Sie **Treiber ändern**. 
- Wählen Sie **zusätzliche Pfad**dann **Hinzufügen**. 
- Geben Sie den **Namen** ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** und 
- Klicken Sie auf **Öffnen**.
- Wählen Sie Liste Treiber und wählen Sie **org.postgresql.Driver** im **Klassennamen**und **OK**.

So richten die Verbindung zum lokalen server
 
- Wählen Sie **Windows** **Aliase anzuzeigen.** 
- Wählen Sie die **+** Schaltfläche einen neuen Alias. 
- Namen Sie *Spam-Datenbank*, wählen Sie im Dropdownmenü **Treiber** **PostgreSQL** .
- Legt die URL auf *Jdbc:postgresql://localhost/spam*. 
- Geben Sie Ihren *Benutzernamen* und *Ihr Kennwort*ein. 
- Klicken Sie auf **OK**. 
- Öffnen Sie das Fenster **Verbindung** Doppelklicken Sie auf den ***Spam-Datenbank*** -Alias. 
- Wählen Sie **Verbinden**.

Einige Abfragen ausführen:

- Wählen Sie die Registerkarte **SQL** .
- Geben Sie eine einfache Abfrage wie `SELECT * from data;` Abfrage Textfeld oben auf der Registerkarte SQL. 
- Drücken Sie **STRG-EINGABETASTE** ausführen. Eichhörnchen SQL gibt standardmäßig die ersten 100 Zeilen aus der Abfrage. 

Es gibt viele weitere Abfragen ausgeführt werden können, um diese Daten zu analysieren. Beispielsweise wie die Häufigkeit von Word *Stellen* Spam und Schinken unterscheiden?

    SELECT avg(word_freq_make), spam from data group by spam;

Oder Was sind die Merkmale der e-Mail, die häufig *3d*enthalten?

    SELECT * from data order by word_freq_3d desc;

Die meisten e-Mails, hohe Vorkommen von *3d* sind offensichtlich spam, so könnte es nützlich für die Erstellung eines Vorhersagemodells zum Klassifizieren von e-Mails.

Wenn Sie computerlernen PostgreSQL-Datenbank durchführen, sollten Sie [MADlib](http://madlib.incubator.apache.org/)verwenden.

## <a name="sql-server-data-warehouse"></a>SQL Server Datawarehouse

Azure SQL Data Warehouse ist eine cloudbasierte, Scale-Out Datenbank verarbeiten große Mengen an relationalen und nicht relationalen Daten. Weitere Informationen finden Sie unter [Neuigkeiten Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Verbindung zum Datawarehouse die Tabelle erstellen und führen Sie den folgenden Befehl aus:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Dann an die Sqlcmd:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Daten Sie mit Bcp:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

>[AZURE.NOTE] Die Zeilenenden in der heruntergeladenen Datei sind Windows-Stil Bcp muss Bcp, die mit dem Flag - R UNIX-Format erwartet

Und mit Sqlcmd:

    select top 10 spam, char_freq_dollar from spam;
    GO

Sie können auch mit Eichhörnchen SQL Abfragen. Ähnliche Schritte für PostgreSQL Microsoft MSSQL Server JDBC-Treiber verwenden, die in ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***finden.

## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über die Themen, die mit Vorgehensweisen durch die Aufgaben, die den Prozess Datenwissenschaft in Azure umfassen finden Sie unter [Team wissenschaftliche Daten](http://aka.ms/datascienceprocess).

Eine andere End-to-End-Schritte im Team wissenschaftliche Daten für bestimmte Szenarien exemplarischen Vorgehensweisen für finden Sie unter [Exemplarische Vorgehensweisen für Team wissenschaftliche Daten](data-science-process-walkthroughs.md). Die exemplarischen Vorgehensweisen veranschaulichen auch Cloud und lokalen Tools und Dienste in einen Workflow oder eine Pipeline erstellt eine intelligente Anwendung kombiniert.

