<properties 
    pageTitle="Zugriff auf Datensätze mit Computer Learning Python-Clientbibliothek | Microsoft Azure" 
    description="Installieren und Verwenden der Python-Clientbibliothek verwaltet und Azure Machine Learning Daten sicher aus einem Python-Umgebung." 
    services="machine-learning" 
    documentationCenter="python" 
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
    ms.author="huvalo;bradsev" />


#<a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Zugriff auf Datensätze mit Python mit Azure Machine Learning Python-Clientbibliothek 

Vorschau der Microsoft Azure Machine Learning Python-Clientbibliothek kann sicheren Zugang zu Ihrem Azure Machine Learning Datasets aus einem Python-Umgebung und ermöglicht die Erstellung und Verwaltung von Datensätzen in einem Arbeitsbereich.

Dieses Thema enthält Informationen zum:

* die Computer lernen Python-Clientbibliothek installieren 
* Zugriff und Datasets, einschließlich Informationen zur Autorisierung auf Azure Machine Learning Datasets aus der Python-Umgebung hochladen
*  intermediären Datasets Access aus
*  Verwenden der Python-Clientbibliothek auflisten Datasets Metadaten zugreifen, lesen Sie den Inhalt eines Datasets, neue Datasets erstellen und vorhandene Datensätze aktualisieren

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

 
##<a name="prerequisites"></a>Erforderliche Komponenten

Python-Clientbibliothek wurde unter der folgenden Umgebung getestet:

 - Windows, Mac und Linux
 - Python 2.7, 3.3 und 3.4

Es besteht eine Abhängigkeit der folgenden Pakete:

 - Anfragen
 - Python-dateutil
 - Panda

Wir empfehlen eine Python-Distribution installiert wie [Anaconda](http://continuum.io/downloads#all) oder [Himmelszelt](https://store.enthought.com/downloads/)mit Python, IPython und drei Pakete aufgeführt. Obwohl IPython nicht unbedingt erforderlich ist, ist eine optimale Umgebung zum Bearbeiten und Visualisieren von Daten interaktiv.


###<a name="installation"></a>Azure Machine Learning Python-Clientbibliothek installieren

Azure Machine Learning Python-Clientbibliothek muss auch installiert werden, um die in diesem Thema beschriebenen Aufgaben. Es ist [Python Package Index](https://pypi.python.org/pypi/azureml)ab. Um in Ihrer Python-Umgebung zu installieren, führen Sie den folgenden Befehl von der Python-Umgebung:

    pip install azureml

Alternativ können Sie herunterladen und Installieren von Quellen auf [Github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).

    python setup.py install

Haben Sie auf Ihrem Computer installierten Git können Pip Sie direkt über das Repository Git installieren:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


##<a name="datasetAccess"></a>Verwenden von Codeausschnitten Studio auf Datensätze

Python-Clientbibliothek können Sie den programmgesteuerten Zugriff auf vorhandene Datensätze aus, die ausgeführt wurden.

Über die Weboberfläche Studio können Sie Codeausschnitte erstellen, die alle notwendigen Informationen zum Laden und Deserialisieren von Datasets als Panda Datensatz Objekte auf Ihrem Computer Speicherort enthalten.

### <a name="security"></a>Sicherheit für Daten

Studio gestellt mit der Python-Clientbibliothek die Arbeitsplatz-Id und Autorisierung umfasst Codeausschnitte token. Diese bietet vollständigen Zugriff auf den Arbeitsbereich und geschützt werden müssen, wie ein Kennwort.

Aus Sicherheitsgründen ist die Funktionalität der Ausschnitt nur für Benutzer mit ihrer Rolle als **Besitzer** für den Arbeitsbereich. Die Rolle wird auf der Seite **Benutzer** unter **Einstellungen**in Azure Machine Learning Studio angezeigt.

![Sicherheit][security]

Wenn Ihre Rolle nicht als **Besitzer**festgelegt ist, können Sie entweder erneut eingeladen werden als Besitzer oder bitten Sie den Besitzer des Arbeitsbereichs Codeausschnitt bereitstellen.

Das Authentifizierungstoken erhalten, können Sie eines der folgenden ausführen:



- Fordern Sie ein Token von Besitzer. Besitzer können ihre Autorisierungstoken Einstellungen auf ihren Workspace Studio zugreifen. **Wählen Sie aus der linken** und auf **AUTORISIERUNGSTOKEN** um die primären und sekundären Token anzuzeigen.  Zwar den primären oder sekundären Autorisierungstoken im Codeausschnitt verwendet werden können, sollten Besitzer nur sekundäre Autorisierungstoken freigeben.

![](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

- Fragen Sie zur Rolle des Eigentümers gefördert werden.  Hierzu muss ein Aktueller Besitzer des Arbeitsbereichs zuerst aus dem Arbeitsbereich entfernt und erneut laden, als Besitzer.

Wenn Entwickler die Arbeitsplatz-Id und Autorisierung abgerufen haben token sind Arbeitsbereich Codeausschnitt unabhängig von ihrer Rolle zugreifen.

Autorisierungstoken werden auf der Seite **AUTORISIERUNGSTOKEN** unter **EINSTELLUNGEN**verwaltet. Regenerieren Sie diese jedoch dieses Verfahren auf dem vorherigen Token widerruft.

### <a name="accessingDatasets"></a>Access Datensätze aus einer lokalen Python-Anwendung

1. Machine Learning Studio klicken Sie auf **Datensätze** in der Navigationsleiste auf der linken Seite.

2. Wählen Sie das Dataset zugreifen möchte. Alle Datensätze können aus der Liste **Eigene DATASETS** oder aus **Beispiele** auswählen.

3. Die untere Symbolleiste klicken Sie auf **generieren Datenzugriffscode**. Wenn die Daten in einem Format mit der Python-Clientbibliothek nicht kompatibel sind, ist diese Schaltfläche deaktiviert.

    ![Datasets][datasets]

4. Wählen Sie den Codeausschnitt aus das Fenster wird angezeigt und in die Zwischenablage kopieren.

    ![Zugangscode][dataset-access-code]

5. Fügen Sie den Code in das Notebook der lokalen Python Anwendung.

    ![Notebook][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Intermediären Datasets Access aus maschinelles lernen

Nach dem Versuch in Machine Learning Studio ausgeführt wird, kann die intermediären Datasets Ausgabeknoten Module zugreifen. Intermediären Datasets sind Daten, die erstellt und zur Zwischenschritte beim Ausführen einer Modell-Tools.

Intermediären Datasets kann zugegriffen werden, wie das Format mit der Python-Clientbibliothek kompatibel ist.

Folgende Formate werden unterstützt (diese Konstanten sind in der `azureml.DataTypeIds` Klasse):

 - Klartext
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader

Ein Modul Ausgabeknoten Mauszeiger können Sie das Format bestimmen. Es wird mit den Knotennamen in einer QuickInfo angezeigt.

Einige Module wie [Split] [ split] Modul Ausgabe in ein Format mit dem Namen `Dataset`, die von der Python-Clientbibliothek nicht unterstützt.

![DataSet-Format][dataset-format]

Sie benötigen ein Modul Konvertierung wie [in CSV konvertiert][convert-to-csv], um eine Ausgabe in einem unterstützten Format.

![GenericCSV formatieren][csv-format]

Die folgenden Schritte zeigen ein Beispiel, das Experiment erstellt wird und intermediären Dataset zugreift.

1. Erstellen Sie einen neuen Test.

2. Einfügen eines Moduls **Erwachsenen Erhebung Einnahmen binäre Klassifizierung Dataset** .

3. Einfügen einer [geteilten] [ split] Modul und Eingabe Ausgabe Modul Dataset verbinden.

4. Einfügen einer [CSV konvertieren] [ convert-to-csv] Modul und die Eingabe eines [geteilten] [ split] Modul ausgegeben.

5. Speichern Sie des Experiments, führen Sie aus und warten Sie beendet.

6. Klicken Sie auf der Ausgabeknoten auf [in CSV konvertiert] [ convert-to-csv] Modul.

7. Wenn das Kontextmenü angezeigt wird, wählen Sie **Datenzugriffscode generieren**.

    ![Kontextmenü][experiment]

8. Wählen Sie den und aus dem Fenster in die Zwischenablage zu kopieren.

    ![Zugangscode][intermediate-dataset-access-code]

9. Fügen Sie den Code in Ihrem Notizbuch.

    ![Notebook][ipython-intermediate-dataset]

10. Sie können die Daten mit Matplotlib anzeigen. Im Histogramm für die Age-Spalte angezeigt:

    ![Histogramm][ipython-histogram]


##<a name="clientApis"></a>Verwenden der Computer lernen Python-Clientbibliothek Zugriff, lesen, erstellen und Verwalten von Datensätzen

### <a name="workspace"></a>Arbeitsbereich

Der Arbeitsbereich ist der Einstiegspunkt für die Python-Clientbibliothek. Bereitstellen der `Workspace` Klasse mit Ihrem Arbeitsbereich und Autorisierung token eine Instanz zu erstellen:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Datensätze auflisten

Alle Datensätze in einem bestimmten Workspace aufgelistet werden:

    for ds in ws.datasets:
        print(ds.name)

Nur die benutzerdefinierten Datensätze aufgelistet werden:

    for ds in ws.user_datasets:
        print(ds.name)

Nur die Beispiel-Datasets aufgelistet werden:

    for ds in ws.example_datasets:
        print(ds.name)

Sie können ein Dataset nach Namen zugreifen (die Groß-/Kleinschreibung):

    ds = ws.datasets['my dataset name']

Oder Sie können Index:

    ds = ws.datasets[0]


### <a name="metadata"></a>Metadaten

Datasets sind Metadaten neben. (Intermediären Datasets sind eine Ausnahme von dieser Regel und haben keine Metadaten.)

Einige Metadatenwerte werden vom Benutzer bei der Erstellung zugewiesen:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Andere sind Werte von Azure ML zugewiesen:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Siehe die `SourceDataset` über die Metadaten-Klasse.


### <a name="read-contents"></a>Inhalt lesen

Machine Learning Studio automatisch bereitgestellten Codeausschnitte herunterladen und das Dataset Panda Datensatz Objekt deserialisiert. Dies erfolgt mit der `to_dataframe` Methode:

    frame = ds.to_dataframe()

Wenn Sie Rohdaten laden und die Deserialisierung selbst durchführen möchten, ist eine Option. Derzeit ist dies die einzige Option für Formate wie "ARFF" den Python-Clientbibliothek deserialisieren kann nicht.

Um den Inhalt als Text zu lesen:

    text_data = ds.read_as_text()

Um den Inhalt als Binärdatei lesen:

    binary_data = ds.read_as_binary()

Sie können auch einen Stream zum Inhalt öffnen:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Erstellen eines neuen Datasets

Python-Clientbibliothek können Sie Datensätze aus Python-Programm hochladen. Diese Datensätze sind im Arbeitsbereich verfügbar.

Haben Sie Ihre Daten in einem Datensatz Panda, verwenden Sie folgenden Code:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Wenn die Daten bereits serialisiert werden, können Sie:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Python-Clientbibliothek kann ein Datensatz Panda in folgenden Formaten serialisiert (diese Konstanten sind in der `azureml.DataTypeIds` Klasse):

 - Klartext
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader


### <a name="update-an-existing-dataset"></a>Aktualisieren eines vorhandenen Datasets

Wenn Sie ein neues Dataset mit dem Namen hochladen, die ein vorhandenes Dataset übereinstimmt, erhalten Sie einen Ressourcenkonflikt-Fehler.

Aktualisieren ein vorhandenes Datasets müssen Sie zunächst einen Verweis auf das vorhandene Dataset abzurufen:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Dann `update_from_dataframe` serialisiert und Ersetzen Sie den Inhalt eines Datasets in Azure:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Die Daten in ein anderes Format serialisiert werden soll, geben Sie einen Wert für den optionalen `data_type_id` Parameter.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Optional können Sie eine neue Beschreibung festlegen, einen Wert für die `description` Parameter.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

Optional können Sie einen neuen Namen festlegen, einen Wert für die `name` Parameter. Von nun an wird das Dataset mit den neuen Namen abgerufen werden. Der folgende Code aktualisiert Daten, Name und Beschreibung.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

Die `data_type_id`, `name` und `description` Parameter sind optional und standardmäßig auf den vorherigen Wert. Die `dataframe` Parameter ist immer erforderlich.

Wenn Ihre Daten bereits serialisiert werden, verwenden Sie `update_from_raw_data` anstelle von `update_from_dataframe`. Übergeben Sie einfach im `raw_data` anstelle von `dataframe`, funktioniert ähnlich.



<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
