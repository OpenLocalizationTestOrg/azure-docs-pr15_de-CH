<properties
    pageTitle="Szenarien für die erweiterte Analyse Prozesse und Technologie in Azure Machine Learning | Microsoft Azure"
    description="Wählen Sie die Szenarios dafür erweiterte Vorhersageanalytik mit Team wissenschaftliche Daten."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na" 
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Szenarien für die erweiterte Analyse in Azure Machine Learning

Dieser Artikel beschreibt verschiedene Aufnahmequellen Daten und Szenarios, die vom [Team Daten Wissenschaft Prozess (TDSP)](data-science-process-overview.md)behandelt werden. Die TDSP bietet einen systematischen Ansatz für Teams zusammenarbeiten erstellen intelligente Clientanwendungen. Die hier vorgestellten Szenarien veranschaulichen Optionen im Workflow Datenverarbeitung, die die Merkmale Quellverzeichnisse und Ziel-Repositories in Azure abhängen.

Die **Entscheidungsstruktur** für die Auswahl der Beispielszenarios für Ihre Daten und Ziel wird im letzten Abschnitt dargestellt.

>[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Die folgenden Abschnitte stellt ein Beispielszenario. Für jedes Szenario datenwissenschaft oder erweiterte Analyse Fluss und Unterstützung von Azure Ressourcen aufgeführt.

>[AZURE.NOTE]**Für alle folgenden Szenarien müssen:**
><br/>
>* [Ein Speicherkonto erstellen](../storage/storage-create-storage-account.md)
><br/>
>* [Einen Azure Machine Learning-Arbeitsbereich erstellen](machine-learning-create-workspace.md)


## <a name="smalllocal"></a>Szenario \#1: kleine und mittlere tabellarische Dataset in lokalen Dateien

![Kleine und mittlere lokale Dateien][1]

#### <a name="additional-azure-resources-none"></a>Zusätzliche Azure Ressourcen: keine

1.  Melden Sie sich bei [Azure Machine Learning Studio](https://studio.azureml.net/).

2.  Uploaden Sie ein Dataset.

3.  Erstellen eines Azure Machine Learning Experiment Flows hochgeladene Datasets ab.

## <a name="smalllocalprocess"></a>Szenario \#2: kleine und mittlere Dataset lokaler Dateien, die bearbeitet werden müssen

![Kleine und mittlere lokale Dateien verarbeiten][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Azure Zusatzressourcen: Azure Virtual Machine (IPython Notebook-Server)

1.  Erstellen Sie ein IPython Notebook mit Azure Virtual Machine.

2.  Upload von Daten zu einem Container Azure-Speicher.

3.  Vor der Verarbeitung und Bereinigen von Daten in IPython Notebook Azure Speichercontainer Daten zugreifen.

4.  Transformieren von Daten bereinigt tabellarischen Form.

5.  Speichern Sie transformierte Daten in Azure-Blobs.

6.  Melden Sie sich bei [Azure Machine Learning Studio](https://studio.azureml.net/).

7.  Lesen Sie Daten von Azure-Blobs [Importdaten] [ import-data] Modul.

8. Einen Azure Machine Learning Experiment Flow mit aufgenommenen Datasets zu erstellen.

## <a name="largelocal"></a>Szenario \#3: große Datasets Azure-Blobs auf lokale Dateien

![Große lokale Dateien][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Azure Zusatzressourcen: Azure Virtual Machine (IPython Notebook-Server)

1.  Erstellen Sie ein IPython Notebook mit Azure Virtual Machine.

2.  Upload von Daten zu einem Container Azure-Speicher.

3.  Vor der Verarbeitung und Bereinigen von Daten in IPython Notebook Azure-Blobs Daten zugreifen.

4.  Transformieren von Daten bereinigt tabellarischen Form bei Bedarf.

5.  Daten und Funktionen nach Bedarf erstellen.

6.  Kleine bis mittlere Datenmenge zu extrahieren.

7.  Speichern Sie die gesammelten Daten in Azure-Blobs.

8. Melden Sie sich bei [Azure Machine Learning Studio](https://studio.azureml.net/).

9. Lesen Sie Daten von Azure-Blobs [Importdaten] [ import-data] Modul.

10. Azure Machine Learning Experiment Flow mit aufgenommenen Datasets zu erstellen.


## <a name="smalllocaltodb"></a>Szenario \#4: kleine und mittlere Dataset lokaler Dateien für SQL Server in einem Azure virtuellem Computer

![Kleine und mittlere lokale Dateien DB SQL Azure][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Azure Zusatzressourcen: Azure Virtual Machine (SQL Server / IPython Notebook-Server)

1.  Erstellen einer Azure Virtual Machine unter SQL Server + IPython Notebook.

2.  Upload von Daten zu einem Container Azure-Speicher.

3.  Vor der Verarbeitung und Bereinigen von Daten in Azure Box IPython Notebook verwenden.

4.  Transformieren von Daten bereinigt tabellarischen Form bei Bedarf.

5.  VM lokale Dateien speichern (IPython Notebook auf VM ausgeführt wird, finden Sie lokale Laufwerke in VM-Laufwerke).

6.  Laden Sie SQL Server-Datenbank auf eine Azure-VM.

    Option \#1: mit SQL Server Management Studio.

    - Anmeldung an SQL Server-VM
    - Führen Sie SQL Server Management Studio.
    - Datenbank und Zieltabellen zu erstellen.
    - Verwenden Sie eine der größte importieren Methoden, die Daten von VM-lokale Dateien laden.

    Option \#2: Verwendung IPython Notebook-mittleren und größeren Datasets nicht empfehlenswert<!-- -->    
    - Verwenden Sie ODBC-Verbindungszeichenfolge, um SQL Server auf VM zuzugreifen.
    - Datenbank und Zieltabellen zu erstellen.
    - Verwenden Sie eine der größte importieren Methoden, die Daten von VM-lokale Dateien laden.

7.  Daten, Funktionen nach Bedarf erstellen. Beachten Sie, dass die Funktionen nicht in den Datenbanktabellen materialisiert werden müssen. Beachten Sie nur die erforderliche Abfrage zu erstellen.

8. Legen Sie eine Stichprobengröße der Daten Wenn erforderlich oder gewünscht.

9. Melden Sie sich bei [Azure Machine Learning Studio](https://studio.azureml.net/).

10. Lesen Sie Daten von SQL Server mit den [Importierten Daten] direkt[ import-data] Modul. Fügen Sie die erforderliche Abfrage extrahiert Felder und Funktionen erstellt aufgenommene Daten direkt in den [Importierten Daten] ggf.[ import-data] Abfrage.

11. Azure Machine Learning Experiment Flow mit aufgenommenen Datasets zu erstellen.

## <a name="largelocaltodb"></a>Szenario \#5: großes Dataset in lokalen Dateien Ziel SQL Server in Azure VM

![Große lokale Dateien DB SQL Azure][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Azure Zusatzressourcen: Azure Virtual Machine (SQL Server / IPython Notebook-Server)

1.  Erstellen einer Azure virtuellen Computer mit SQL Server und IPython Notebook.

2.  Upload von Daten zu einem Container Azure-Speicher.

3.  (Optional) Vor der Verarbeitung und Bereinigen von Daten.

    ein.  Vor der Verarbeitung und Bereinigen von Daten in IPython Notebook Azure-Blobs Daten zugreifen.

    b.  Transformieren von Daten bereinigt tabellarischen Form bei Bedarf.

    c.  VM lokale Dateien speichern (IPython Notebook auf VM ausgeführt wird, finden Sie lokale Laufwerke in VM-Laufwerke).

4.  Laden Sie SQL Server-Datenbank auf eine Azure-VM.

    ein.  Bei SQL Server-VM.

    b.  Wenn nicht bereits gespeichert, Dateien Sie Daten von Azure Speichercontainer lokale VM-Ordner.

    c.  Führen Sie SQL Server Management Studio.

    d.  Datenbank und Zieltabellen zu erstellen.

    e.  Verwenden Sie eine der größte importieren Methoden, um die Daten zu laden.

    f.  Wenn gezählt erforderlich sind, erstellen Sie Indizes um Joins zu beschleunigen.

     > [AZURE.NOTE] Schnelleres Laden von großen Datenmengen wird empfohlen partitionierte Tabellen erstellen, die Daten parallel Massenimport. Weitere Informationen finden Sie unter [Parallele Datenimport SQL partitionierten Tabellen](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Daten, Funktionen nach Bedarf erstellen. Beachten Sie, dass die Funktionen nicht in den Datenbanktabellen materialisiert werden müssen. Beachten Sie nur die erforderliche Abfrage zu erstellen.

6.  Legen Sie eine Stichprobengröße der Daten Wenn erforderlich oder gewünscht.

7.  Melden Sie sich bei [Azure Machine Learning Studio](https://studio.azureml.net/).

8. Lesen Sie Daten von SQL Server mit den [Importierten Daten] direkt[ import-data] Modul. Fügen Sie die erforderliche Abfrage extrahiert Felder und Funktionen erstellt aufgenommene Daten direkt in den [Importierten Daten] ggf.[ import-data] Abfrage.

9. Einfache Azure Machine Learning Experiment Flow hochgeladene Dataset ab

## <a name="largedbtodb"></a>Szenario \#6: großes Dataset in einer SQL Server-Datenbank auf-Prem, auf SQL Server eine Azure Virtual Machine

![Große SQL DB auf-Prem SQL DB in Azure][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Azure Zusatzressourcen: Azure Virtual Machine (SQL Server / IPython Notebook-Server)

1.  Erstellen einer Azure virtuellen Computer mit SQL Server und IPython Notebook.

2.  Verwendet eine der Methoden zum Exportieren der Daten aus SQL Server Dump-Dateien exportieren

    > [AZURE.NOTE] Wenn Sie alle Daten aus der Datenbank auf Prem (schneller) Alternativ können die vollständige Datenbank der SQL Server-Instanz in Azure verschieben verschieben möchten. Überspringen Sie die Schritte zum Exportieren von Daten erstellen, und Last-Import-Daten in der Zieldatenbank und das alternative Verfahren.

3.  Hochladen Sie Dumpdateien auf Azure Speichercontainer.

4.  Laden Sie die Daten einer SQL Server-Datenbank eine Azure Virtual Machine.

    ein.  Anmeldung bei der SQL Server-VM.

    b.  Dateien aus einem Container Azure-Speicher in den Ordner Lokale VM herunterladen.

    c.  Führen Sie SQL Server Management Studio.

    d.  Datenbank und Zieltabellen zu erstellen.

    e.  Verwenden Sie eine der größte importieren Methoden, um die Daten zu laden.

    f.  Wenn gezählt erforderlich sind, erstellen Sie Indizes um Joins zu beschleunigen.

    > [AZURE.NOTE] Importieren Sie schnelleres Laden von großen Datenmengen erstellen partitionierte Tabellen und Bulk die Daten parallel. Weitere Informationen finden Sie unter [Parallele Datenimport SQL partitionierten Tabellen](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Daten, Funktionen nach Bedarf erstellen. Beachten Sie, dass die Funktionen nicht in den Datenbanktabellen materialisiert werden müssen. Beachten Sie nur die erforderliche Abfrage zu erstellen.

6.  Legen Sie eine Stichprobengröße der Daten Wenn erforderlich oder gewünscht.

7.  Melden Sie sich bei [Azure Machine Learning Studio](https://studio.azureml.net/).

8. Lesen Sie Daten von SQL Server mit den [Importierten Daten] direkt[ import-data] Modul. Fügen Sie die erforderliche Abfrage extrahiert Felder und Funktionen erstellt aufgenommene Daten direkt in den [Importierten Daten] ggf.[ import-data] Abfrage.

9. Einfache Azure Machine Learning experimentieren Fluss hochgeladene Dataset ab.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Alternative Methode zum Kopieren einer vollständigen Datenbank aus einer lokalen SQL Server in Azure SQL-Datenbank

![Trennen Sie lokale DB und DB SQL Azure zuordnen][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Azure Zusatzressourcen: Azure Virtual Machine (SQL Server / IPython Notebook-Server)

Replikation die gesamte SQL Server-Datenbank in der SQL Server-VM kopieren Sie eine Datenbank aus einem Standortserver, vorausgesetzt, die Datenbank vorübergehend offline geschaltet werden kann. Hierzu im Objekt-Explorer von SQL Server Management Studio oder die entsprechende Transact-SQL-Befehle.

1. Trennen Sie die Datenbank am Quellspeicherort. Weitere Informationen finden Sie unter [Trennen einer Datenbank](https://technet.microsoft.com/library/ms191491(v=sql.110).aspx).
2. Klicken Sie im Windows Explorer oder Windows-Befehlszeile kopieren Sie getrennten Datei oder Dateien und Protokolldatei oder Dateien an den Zielspeicherort für die SQL Server-VM in Azure
3. Legen Sie die kopierten Dateien der SQL Server-Zielinstanz. Weitere Informationen finden Sie unter [Anfügen einer Datenbank](https://technet.microsoft.com/library/ms190209(v=sql.110).aspx).

[Verschieben Sie eine Datenbank trennen und anfügen (Transact-SQL)](https://technet.microsoft.com/library/ms187858(v=sql.110).aspx)

## <a name="largedbtohive"></a>Szenario \#7: Big Data in lokalen Dateien Ziel Struktur Datenbank in Azure HDInsight Hadoop-Cluster

![Große Daten in lokales Ziel Struktur][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Azure Zusatzressourcen: Azure HDInsight Hadoop Cluster und Azure Virtual Machine (IPython Notebook-Server)

1.  Erstellen Sie ein IPython Notebook läuft Azure Virtual Machine.

2.  Einen Azure HDInsight Hadoop-Cluster zu erstellen.

3.  (Optional) Vor der Verarbeitung und Bereinigen von Daten.

    ein.  Vor der Verarbeitung und Bereinigen von Daten in IPython Notebook Azure-Blobs Daten zugreifen.

    b.  Transformieren von Daten bereinigt tabellarischen Form bei Bedarf.

    c.  VM lokale Dateien speichern (IPython Notebook auf VM ausgeführt wird, finden Sie lokale Laufwerke in VM-Laufwerke).

4.  Upload von Daten zum Standardcontainer Hadoop Cluster in Schritt 2 ausgewählt.

5.  Struktur-Datenbank in Azure HDInsight Hadoop-Cluster zu laden.

    ein.  Melden Sie sich bei dem Head-Knoten des Clusters Hadoop

    b.  Öffnen Sie die Befehlszeile Hadoop.

    c.  Geben Sie das Stammverzeichnis der Struktur vom Befehl `cd %hive_home%\bin` Hadoop Command Line.

    d.  Abfragen der Struktur Datenbank und Tabellen erstellen und Laden von Daten aus dem BLOB-Speicher Hive-Tabellen.

    > [AZURE.NOTE] Wenn Daten groß ist, können Benutzer die Tabelle Struktur Partitionen erstellen. Benutzer können dann eine `for` Schleife in der Hadoop Befehlszeile auf dem Head-Knoten zum Laden von Daten in der Partition partitioniert Hive-Tabelle.

6.  Daten und Funktionen nach Bedarf in Hadoop Befehlszeile erstellen. Beachten Sie, dass die Funktionen nicht in den Datenbanktabellen materialisiert werden müssen. Beachten Sie nur die erforderliche Abfrage zu erstellen.

    ein.  Melden Sie sich bei dem Head-Knoten des Clusters Hadoop

    b.  Öffnen Sie die Befehlszeile Hadoop.

    c.  Geben Sie das Stammverzeichnis der Struktur vom Befehl `cd %hive_home%\bin` Hadoop Command Line.

    d.  Abfragen der Struktur in Hadoop Befehlszeile auf dem Head-Knoten des Clusters Hadoop zu Daten Funktionen nach Bedarf erstellen.

7.  Wenn erforderlich oder gewünscht, Beispieldaten Sie die in Azure Machine Learning Studio passt.

8.  Melden Sie sich bei [Azure Machine Learning Studio](https://studio.azureml.net/).

9. Lesen Sie die Daten direkt aus der `Hive Queries` mit den [Importierten Daten] [ import-data] Modul. Fügen Sie die erforderliche Abfrage extrahiert Felder und Funktionen erstellt aufgenommene Daten direkt in den [Importierten Daten] ggf.[ import-data] Abfrage.

10. Einfache Azure Machine Learning experimentieren Fluss hochgeladene Dataset ab.

## <a name="decisiontree"></a>Entscheidungsstruktur für Szenario-Auswahl
------------------------

Im folgende Diagramm werden die oben beschriebenen Szenarios und erweiterte Analyseprozess und Technologie Entscheidungen zu detaillierten Szenarien zusammengefasst. Beachten Sie, dass Datenverarbeitung, Untersuchung Featureentwicklung und Probenahme können Methode/Umgebung an der Quelle, zwischen- oder zielumgebungen versehen und können nach Bedarf wiederholt. Das Diagramm dient zur Veranschaulichung einige der möglichen Flows nur und bietet nicht erschöpfende Aufzählung.

![DS Prozess Walkthrough Beispielszenarien][8]

### <a name="advanced-analytics-in-action-examples"></a>Erweiterte Analysen in Aktion Beispiele

End-to-End Azure Machine Learning exemplarischen Vorgehensweisen, in denen Analyseprozess erweitert und mit öffentlichen Datasets verwendet finden Sie unter:


* [Team wissenschaftliche Daten in Aktion: mit SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
* [Team wissenschaftliche Daten in Aktion: HDInsight Hadoop Cluster](machine-learning-data-science-process-hive-walkthrough.md).


[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
