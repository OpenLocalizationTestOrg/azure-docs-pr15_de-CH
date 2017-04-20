<properties
    pageTitle="Was ist R auf HDInsight? Einführung in R Server auf HDInsight (Vorschau) | Microsoft Azure"
    description="Was ist R Server HDInsight (Vorschau) und R-Server verwenden für Anträge auf große Analyse erstellen."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/17/2016"
   ms.author="jeffstok"/>


# <a name="overview-of-r-server-on-hdinsight-preview"></a>Übersicht über Server auf HDInsight R \(Vorschau\)

Mit Microsoft Azure HDInsight Premium wird Microsoft R Server jetzt optional in Azure HDInsight-Cluster erstellen. Diese neue Funktion bietet datenwissenschaftler, Statistiker und R Programmierer bei Bedarf Zugriff auf skalierbare Methoden der Analyse auf HDInsight verteilt.

Cluster können Projekte und Aufgaben angepasst und abgerissen, wenn sie nicht mehr benötigt werden. Teil Azure HDInsight sind, kommen diese Cluster auf Unternehmensebene 24/7 Unterstützung einer SLA von 99,9 % Verfügbarkeit und Flexibilität mit anderen Komponenten in Azure Ökosystem integriert.

>[AZURE.NOTE] R-Server ist nur mit HDInsight verfügbar. HDInsight Premium ist derzeit nur für Hadoop und Spark-Cluster verfügbar. So können aktuell R Server nur mit Hadoop und Spark auf HDInsight Sie. Weitere Informationen finden Sie unter [die verschiedenen Service-Level und Hadoop Komponenten mit HDInsight?](hdinsight-component-versioning.md).

R-Server auf HDInsight bietet die neuesten Funktionen für R-basierte Analysen für große Datasets Azure BLOB-Speicher geladen werden. Da open Source F R Server aufbaut, profitieren R-basierte Anwendung Erstellen eines 8000 + open-Source-R Pakete sowie die Routinen ScaleR Microsoft big Data Analytics Paket R Server enthalten.

Kantenknoten Premium Cluster bietet eine bequeme Möglichkeit zum Cluster herstellen und die R-Skripts ausgeführt. Mit einen Kantenknoten haben Sie die Möglichkeit des ScaleR parallelisierten verteilten Funktionen über Kerne Knoten Edgeserver. Sie haben die Möglichkeit, mithilfe des ScaleR Hadoop Map Reduce Knoten des Clusters ausgeführt oder Funken berechnen Kontexte.

Modelle oder Vorhersagen Ergebnis von Analysen für heruntergeladen werden verwenden lokal. Sie können auch an einer anderen Stelle in Azure wie ein [Azure Machine Learning Studio](http://studio.azureml.net) [Webdienst](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md)operationalisiert werden.

## <a name="get-started-with-r-on-hdinsight"></a>Erste Schritte mit R HDInsight

Um R Server in einem Cluster HDInsight aufzunehmen, legen Sie Hadoop oder Funken Cluster mit Premium-Option beim Erstellen eines Clusters mithilfe des Azure-Portals. Beide Cluster verwenden dieselbe Konfiguration, einschließlich R Server auf Datenknoten eines Clusters und eines als Zielseite Zone R serverbasierte Analytics. Eine ausführliche Anleitung zum Erstellen eines Clusters finden Sie unter [Erste Schritte mit R Server HDInsight](hdinsight-hadoop-r-server-get-started.md) .

## <a name="learn-about-data-storage-options"></a>Erfahren Sie mehr über Data Storage-Optionen

Standardspeicher für HDInsight-Cluster ist BLOB-Speicher mit dem BLOB-Container zugeordnet bietet. Dies gewährleistet, dass Daten auf den Clusterspeicher hochgeladen oder im Verlauf der Analyse Clusterspeicher geschrieben ist dauerhaft. Das Dienstprogramm [AzCopy](../storage/storage-use-azcopy.md) können Daten kopieren und aus dem Blob.

Neben BLOB-Speicher verwenden, können Sie Cluster mit [Azure See Datenspeicher](https://azure.microsoft.com/services/data-lake-store/) . Verwenden Sie Daten-See können Sie BLOB-Speicher und Daten See für bietet Speicher verwenden.

Auch können [Azure-Dateien](../storage/storage-how-to-use-files-linux.md) als eine Speicheroption für auf dem edgeknoten. Azure Dateien können Sie eine Dateifreigabe bereitstellen, die in Azure Storage auf Linux erstellt wurde. Weitere Informationen zu Speicheroptionen für R Server Daten auf HDInsight Cluster finden Sie unter [Speicheroptionen für R HDInsight-Cluster Server](hdinsight-hadoop-r-server-storage.md).

## <a name="access-r-server-on-the-cluster"></a>Access-R Server im cluster

Nach dem Erstellen eines Clusters mit R-Server können Sie die Konsole R auf dem edgeknoten des Clusters über SSH-kitten verbinden. Sie können auch über einen Browser verbinden, wenn Sie optionale RStudio Server IDE auf dem edgeknoten installieren. Weitere Informationen zu RStudio Installation finden Sie unter [HDInsight-Cluster RStudio Server installieren](hdinsight-hadoop-r-server-install-r-studio.md).   

## <a name="develop-and-run-r-scripts"></a>Entwickeln und Ausführen von R-Skripts

R-Skripts erstellen und ausführen können open-Source-R Pakete in 8000 + neben der parallelisierten und verteilten Routinen ScaleR-Bibliothek. Im Allgemeinen wird Skript, R-Server auf dem edgeknoten ausgeführt wird, innerhalb der R-Interpreter auf diesem Knoten ausgeführt. Die Ausnahme ist die Schritte, die ein ScaleR aufrufen Compute-Kontext, der Satz Hadoop Karte (RxHadoopMR) oder Funken (RxSpark) funktioniert.

In diesen Fällen führt die Funktion verteilt die Daten (Task) Knoten des Clusters, die den referenzierten Daten zugeordnet sind. Weitere Informationen zu den unterschiedlichen berechnen Kontextoptionen finden Sie unter [Kontextoptionen R Server auf HDInsight Premium zu berechnen](hdinsight-hadoop-r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Ein Modell durchsetzen

Nach Abschluss der Datenmodelle können das Modell Vorhersagen neuer Daten in Azure und lokalen durchsetzen. Dieser Prozess wird als Bewertung bezeichnet. Hier sind einige Beispiele.

### <a name="score-in-hdinsight"></a>Bewertung in HDInsight

Schreiben Sie in HDInsight erzielen eine R Funktion, die Modell zu, das Speicherkonto laden Vorhersagen für eine neue Datendatei aufruft. Speichern Sie die Vorhersagen an das Speicherkonto. Sie können Routineaufgaben auf Anforderung Edge-Knoten des Clusters oder eines geplanten Auftrags ausführen.  

### <a name="score-in-azure-machine-learning"></a>Faktor in Azure maschinelles lernen

Erzielen Sie mit einer Azure Machine Learning-Webdienst, können Sie open Source Azure Machine Learning R Paket als [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) Modell als Webdienst Azure veröffentlichen. Der Einfachheit halber ist dieses Paket auf den edgeknoten. Anschließend nutzen Sie Computer lernen zum Erstellen einer Benutzeroberfläche für den Webdienst, und rufen Sie den Webdienst für bewerten.

Wenn Sie diese Option auswählen, müssen Sie entsprechende Open-Source-Modellobjekte für die Verwendung mit dem Webdienst ScaleR Modellobjekte konvertieren. Dies kann durch ScaleR Umwandlung Funktionen wie `as.randomForest()` für Ensemble Modelle.


### <a name="score-on-premises"></a>Bewertung vor Ort

Lokal nach dem Erstellen Ihres Modells erzielen können Sie serialisieren Modell r, herunterladen, deserialisieren und zur Bewertung neuer Daten verwenden. Sie können neue Daten mithilfe des oben beschriebenen [Punkte zählen in HDInsight](#scoring-in-hdinsight) Ansatzes oder [DeployR](https://deployr.revolutionanalytics.com/)erhalten.

## <a name="maintain-the-cluster"></a>Verwalten des Clusters

### <a name="install-and-maintain-r-packages"></a>Installieren und Verwalten von Paketen F

R-Pakete, mit denen Sie auf dem edgeknoten müssen, da die meisten Skripts R es ausgeführt werden. Zusätzliche R-Pakete auf dem edgeknoten installieren, können Sie die üblichen `install.packages()` -Methode in R.

In den meisten Fällen müssen Sie zusätzliche R-Pakete auf Datenknoten installieren, verwenden Sie nur Programme aus der Bibliothek ScaleR im Cluster. Jedoch möglicherweise zusätzliche Pakete für Unterstützung von **RxExec** oder **RxDataStep** Ausführung auf Datenknoten.

In diesen Fällen müssen zusätzliche Pakete mithilfe einer Aktion Skript angegeben werden, nach dem Erstellen des Clusters. Weitere Informationen finden Sie unter [Erstellen eines HDInsight-Clusters mit R-Server](hdinsight-hadoop-r-server-get-started.md).   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Hadoop Map Reduce Speicher ändern

Ein Cluster kann geändert werden, ändern die Speichermenge, die R-Server zur Ausführung eines Auftrags Karte reduzieren. Verwenden Sie zum Ändern eines Clusters Apache Ambari Benutzeroberfläche, die über Azure Portal-Blade für den Cluster verfügbar ist. Hinweise auf Ambari UI für Ihren Cluster finden Sie unter [Verwalten HDInsight Cluster mit Ambari Web-Benutzeroberfläche](hdinsight-hadoop-manage-ambari.md).

Es ist auch möglich, die Speichermenge ändern R Server verfügbare mit Hadoop Switches im Aufruf an **RxHadoopMR** wie folgt:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Cluster skalieren

Ein vorhandenen Cluster kann über das Portal nach oben oder unten skaliert werden. Durch Skalieren, erhalten die zusätzliche Kapazität für größere Verarbeitungsaufgaben benötigen, oder Sie skalieren wieder einen Cluster wird im Leerlauf. Informationen zum Skalieren eines Clusters finden Sie unter [Verwalten HDInsight-Cluster](hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Wartung des Systems

Wartung erfolgt die zugrunde liegenden Linux VMs in einem Cluster HDInsight tagsüber Betriebssystem-Patches und andere Updates. In der Regel Wartung um 3:30 Uhr (basierend auf der lokalen Zeit für die VM) jeden Montag und Donnerstag erfolgt. Updates werden so ausgeführt, dass sie nicht mehr als ein Viertel des Clusters gleichzeitig auswirken.  

Hauptknoten sind redundant, und nicht alle Datenknoten beeinträchtigt, verlangsamen Aufträge, die während dieser Zeit laufen. Sie sollten jedoch trotzdem vollständig ausführen. Benutzerdefinierte Software oder lokale Daten über diese Wartungsereignisse bleibt, wenn ein schwerwiegender Fehler auftritt, die einen Cluster erstellen muss.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Erfahren Sie mehr über IDE-Optionen für R Server in einem Cluster HDInsight

Linux Rand Knoten eines Clusters HDInsight Premium ist die Zielseite Zone R-Analyse. Nach der Verbindung mit dem Cluster können Sie durch Eingabe von **R** der Linux-Befehlszeile Konsolenschnittstelle R Server starten. Verwendung der Benutzeroberfläche der Konsole wird erweitert, führen einen Texteditor R Skriptentwicklung in einem anderen Fenster Ausschneiden und R-Konsole nach Bedarf Abschnitte des Skripts einfügen.

Anspruchsvollere Tool für die Entwicklung von R-Skript ist R-basierten IDE auf dem Desktop wie Microsoft [R-Tools für Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (FORMAL) angekündigt. Dies ist eine Familie von Desktop- und Server-Tools von [RStudio](https://www.rstudio.com/products/rstudio-server/). Sie können auch Malwares Eclipse-basierten [angegeben](http://www.walware.de/goto/statet).

Eine weitere Option ist die IDE auf Linux Kantenknoten selbst installieren.  Beliebt ist [RStudio Server](https://www.rstudio.com/products/rstudio-server/)eine Browser-basierte für remote Clients enthält. RStudio-Installation auf dem Rand Knoten eines Clusters HDInsight Premium bietet vollständige IDE für die Entwicklung und Ausführung von Skripts F R Server im Cluster. Es ist wesentlich produktiver als R-Konsole.  RStudio-Server verwenden, finden Sie unter [HDInsight-Cluster RStudio Server installieren](hdinsight-hadoop-r-server-install-r-studio.md).

## <a name="learn-about-pricing"></a>Erfahren Sie mehr über Preise

Die Gebühren, die mit einem HDInsight Premium-Cluster mit R Server werden Gebühren für die standardmäßige HDInsight-Cluster ähnlich strukturiert. Sie basieren auf die Größe des zugrunde liegenden virtuellen Computer Namen, Daten und Edge-Knoten mit Core Stunde Anhub Prämie. Weitere Informationen zu HDInsight Premium Preise einschließlich Preise Public Preview und Verfügbarkeit eine 30-tägige kostenlose Testversion finden Sie unter [HDInsight Preisgestaltung](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Nächste Schritte

Folgen Sie den Links unten, um weitere Informationen zur Verwendung von R-Server mit HDInsight.

- [Erste Schritte mit R Server auf HDInsight](hdinsight-hadoop-r-server-get-started.md)

- [HDInsight Premium RStudio Server hinzufügen](hdinsight-hadoop-r-server-install-r-studio.md)

- [Berechnen Sie Kontextoptionen R Server HDInsight (Vorschau)](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure Speicheroptionen für R Server HDInsight Premium](hdinsight-hadoop-r-server-storage.md)
