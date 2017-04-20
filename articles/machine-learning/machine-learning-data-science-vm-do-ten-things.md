<properties
    pageTitle="Zehn Dinge auf Daten Virtual Machine kann | Microsoft Azure"
    description="Verschiedene Daten durchsuchen und Modellierung Aufgabe auf Daten virtuellen Computer ausführen."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="gokuma;weig;bradsev" />

# <a name="ten-things-you-can-do-on-the-data-science-virtual-machine"></a>Zehn Dinge auf Daten virtuellen Computer möglich

Microsoft Data Science Virtual Machine (DSVM) ist eine leistungsstarke Wissenschaft, die verschiedene Daten durchsuchen und Modellierung Aufgaben ermöglicht. Die Umgebung wird bereits erstellt und mit mehreren bekanntes Analysetools, die mit der Analyse für lokal Einstieg erleichtern Cloud und Hybrid-Installationen. Der DSVM arbeitet eng mit vielen Azure Services und Azure Azure SQL Data Warehouse Azure Data Lake, Azure Storage oder DocumentDB bereits Daten gelesen und verarbeitet kann. Es kann auch andere Analysetools wie Azure maschinelles lernen und Azure Data Factory nutzen.


In diesem Artikel gehen wir wie Ihre DSVM Daten Wissenschaft Aufgaben und Interaktion mit anderen Azure-Diensten verwendet. Hier sind einige Dinge auf der DSVM:

1. Daten und Modelle auf Microsoft R Server Python mit DSVM
2. Verwenden Sie ein Jupyter Notebook mit Daten in einem Browser Python 2, Python 3 Microsoft R mit eine Unternehmensversion bereit zur Skalierbarkeits- und r experimentieren
3. Durchsetzen Sie Modelle mit R und Python Azure maschinelles lernen, damit Clientanwendungen Modelle über eine einfache Weboberfläche Services zugreifen können
4. Verwalten Sie Azure-Portal oder in Powershell Azure Ressourcen
5. Erweitern des Speicherplatzes umfangreichen Datasets freigeben und für Ihr gesamtes Team erstellen eine Azure-Dateispeicher als bereitstellbar Laufwerk auf Ihrem DSVM code
6. Teams mit Github freigeben Sie Code und Zugriff auf das Repository mit vorinstallierten Git Clients - Git Bash Git GUI.
7. Zugriff auf verschiedene Azure Daten und Analysen Dienste wie Azure BLOB-Speicher Azure Data Lake Azure HDInsight (Hadoop), Azure DocumentDB Azure SQL Data Warehouse und Datenbanken
8. Erstellen von Berichten und Dashboards mit Power BI Desktop vorinstalliert auf der DSVM und in der Cloud bereitstellen
9. Skalieren Sie der DSVM Ihren Bedürfnissen Projekt dynamisch
10. Zusätzliche Tools auf dem virtuellen Computer installieren   


>[AZURE.NOTE] Zusätzliche Nutzungsgebühren gelten für viele zusätzliche Speicher und Analysen Datendienste in diesem Artikel aufgeführt. Siehe Seite [Azure Preise](https://azure.microsoft.com/pricing/) für Details.


**Erforderliche Komponenten**

- Sie benötigen ein Azure-Abonnement. Sie können sich für eine kostenlose Testversion [hier](https://azure.microsoft.com/free/)anmelden.

- Bereitstellung eines Daten Wissenschaft virtuellen Computers Azure-Portal finden auf [einem virtuellen Computer](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).

## <a name="1-explore-data-and-develop-models-using-microsoft-r-server-or-python"></a>1. Daten und Modellen mit Microsoft R Server oder Python

Sprachen wie R und Python können Sie Ihre Daten Analysen auf der DSVM.

R können Sie für eine IDE "Revolution R Enterprise 8.0" bezeichnet, das auf das Startmenü oder den Desktop. Microsoft hat zusätzliche Bibliotheken auf Open Source/CRAN-r skalierbare Analytics und Analysieren von Daten parallel segmentierten Analyse zulässige Speichergröße überschreitet. Sie können auch eine R IDE Ihrer Wahl wie [RStudio](https://www.rstudio.com/products/rstudio-desktop/).

Für Python können Sie eine IDE wie Visual Studio Community Edition Python-Tools für Visual Studio (PTVS) Erweiterung bereits installiert hat. Standardmäßig wird nur eine einfache Python 2.7 auf PTVS (ohne jede Analytics-Bibliothek wie SciKit, Panda) konfiguriert. Damit Anaconda Python 2.7 und 3.5, müssen Sie Folgendes tun:

* Erstellen von benutzerdefinierten Umgebung für jede Version zu **Tools** -> **Python Tools** -> **Python-Umgebung** und dann auf "**+ benutzerdefinierte**" in Visual Studio 2015 Community Edition
* Geben Sie eine Beschreibung und Umgebung Präfix Pfade als *c:\anaconda* für Anaconda Python 2.7 oder *c:\anaconda\envs\py35* für Anaconda Python 3.5
* Klicken Sie auf **Automatische Erkennung** **Übernehmen** , die Umgebung speichern.

Hier sieht die Installation der benutzerdefinierten Umgebung in Visual Studio.

![PTVS einrichten](./media/machine-learning-data-science-vm-do-ten-things/PTVSSetup.png)

[PTVS Dokumentation](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) Weitere Informationen zum Erstellen von Python-Umgebung anzeigen

Jetzt werden Sie zum Erstellen eines neuen Projekts Python eingerichtet. Navigieren Sie zur **Datei** -> **neu** -> **Projekt** -> **Python** und dann den Python-Anwendung, die Sie erstellen. Festlegen die Python-Umgebung für das aktuelle Projekt auf die gewünschte Version (Anaconda 2.7 oder 3.5): Maustaste **Python-Umgebung** **Software Python-Umgebung**aktivieren und wählen Sie die gewünschte Umgebung, die dem Projekt zugeordnet. Weitere Informationen zum Arbeiten mit PTVS auf der Seite [Dokumentation](https://github.com/Microsoft/PTVS/wiki) finden.

## <a name="2-using-a-jupyter-notebook-to-explore-and-model-your-data-with-python-or-r"></a>2 verwenden ein Jupyter Notebook zu Modelldaten Python oder R

Das Jupyter Notebook ist eine leistungsstarke Umgebung, die eine Browser-basierte "" zum Durchsuchen von Daten und Modellierung enthält. Python-2, Python 3 oder R (Open Source und Microsoft R Server) können in einem Notizbuch Jupyter.

Einführung Jupyter Notebook auf das Start Menüsymbol / desktop-Symbol mit der Bezeichnung **Jupyter Notebook**. Auf der DSVM können auch Sie suchen "Https://localhost:9999 /" Jupiter Notebook auf. Wenn Sie nach einem Kennwort gefragt werden, mit der Anweisungen im Abschnitt ***Erstellen eines sicheren Kennworts auf dem Server Jupyter Notebook*** Thema [Bereitstellung Microsoft Data Science Virtual Machine](machine-learning-data-science-provision-vm.md) Jupyter Notebook Zugriff auf ein sicheres Kennwort erstellt. 

Nach der Arbeitsmappe öffnen sehen Sie ein Verzeichnis, das wenigen Beispiel Notebooks enthält vorkonfigurierte in der DSVM. Jetzt können Sie:

- Klicken Sie auf das Notizbuch um den Code anzuzeigen.
- Führen Sie jede Zelle durch Drücken von **UMSCHALT + EINGABETASTE**.
- Führen Sie das Notizbuch auf **Zelle** -> **Ausführen**
- Erstellen Sie ein neues Notizbuch durch Klicken auf das Symbol Jupyter (oben links) und rechts auf die Schaltfläche **neu** klicken und dann Notebook Sprache (auch bekannt als Kerne).   


>[AZURE.NOTE] Derzeit unterstützt Python 2.7 und Python 3.5 R. R-Kernel unterstützt das Programmieren in Open Source R auch als Unternehmen skalierbare Microsoft R Server.   


Sobald werden in der Arbeitsmappe, die Sie durchsuchen können Ihre Daten Modell erstellen, testen Sie das Modell mit den gewünschten Bibliotheken.


## <a name="3-build-models-using-r-or-python-and-operationalize-them-using-azure-machine-learning"></a>3. erstellen mit R oder Python und durchsetzen mit Azure maschinelles lernen

Erstellt und überprüft Ihr Modell besteht der nächste Schritt in der Regel in die Produktion bereitstellen. Dies ermöglicht die Clientanwendungen modellvorhersagen in Echtzeit oder Batch-Modus aufgerufen. Azure Machine Learning bietet einen Mechanismus zum Durchsetzen von einem Modell in R oder Python.

Wenn Sie Ihr Modell in Azure Machine Learning durchsetzen, ist ein Webdienst verfügbar gemacht, durch die Clients zu REST-Aufrufe, die Eingabeparameter übergeben und empfangen Vorhersagen des Modells als Ausgaben.   


>[AZURE.NOTE] Wenn Sie noch nicht angemeldet für AzureML, Sie erhalten eine kostenlose Workspace oder einem Standardarbeitsbereich [AzureML Studio](https://studio.azureml.net/) -Homepage besuchen und auf "Erste Schritte".   


### <a name="build-and-operationalize-python-models"></a>Erstellen und durchsetzen Python-Modelle

Hier ist ein Codeausschnitt entwickelt ein Python Jupyter Notebook, das ein einfaches Modell mit der Bibliothek SciKit Informationen erstellt.

    #IRIS classification
    from sklearn import datasets
    from sklearn import svm
    clf = svm.SVC()
    iris = datasets.load_iris()
    X, y = iris.data, iris.target
    clf.fit(X, y)

Die Methode zur Bereitstellung von Modellen Python Azure Computer lernen umschließt die Vorhersage des Modells in einer Funktion und wird mit Attributen vorinstallierte Azure Machine Learning Python-Bibliothek, die kennzeichnen Mitgliedsnamen Arbeitsbereich Azure maschinelles lernen, API-Schlüssel und die Eingabe und Rückgabeparameter.  

    from azureml import services
    @services.publish(workspaceid, auth_token)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(int) #0, or 1, or 2
    def predictIris(sep_l, sep_w, pet_l, pet_w):
    inputArray = [sep_l, sep_w, pet_l, pet_w]
    return clf.predict(inputArray)

Ein Client kann jetzt den Webdienst aufrufen. Es sind bequem Wrapper, die REST API-Anfragen erstellen. Hier ist ein Beispielcode an den Webdienst.

    # Consume through web service URL and keys
    from azureml import services
    @services.service(url, api_key)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(float)
    def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
    pass

    IrisPredictor(3,2,3,4)


>[AZURE.NOTE] Azure Machine Learning-Bibliothek ist derzeit nur auf Python 2.7 unterstützt.   


### <a name="build-and-operationalize-r-models"></a>Modelle erstellen und durchsetzen R

Sie können R Modelle auf Daten Wissenschaft virtuellen Computer oder auf Azure maschinelles lernen in einer Weise ähnelt der Vorgehensweise für Python bereitstellen. Ihre Schritte aus:

- Erstellen Sie eine settings.json-Datei wie folgt die Arbeitsplatz-ID und Auth token.
- Erstellen eines Wrappers für das Modell Funktion Vorhersagen.
- Rufen Sie ```publishWebService``` in Azure Machine Learning Bibliothek Wrapper Funktion übergeben.  

Hier ist die Prozedur und Codeausschnitte, die zum Einrichten, erstellen, veröffentlichen und verwenden ein Modell als Webdienst in Azure maschinelles lernen.

#### <a name="setup"></a>Setup

1.  Installieren Sie das Paket AzureML R eingeben ```install.packages("AzureML")``` Revolution R Enterprise 8.0 IDE oder R-IDE.
2.  RTools [hier](https://cran.r-project.org/bin/windows/Rtools/)herunterladen Sie benötigen das Zip-Dienstprogramm in dem Pfad und benannte zip.exe R-Paket in AzureML durchsetzen.
3.  Erstellen Sie eine settings.json-Datei unter einem Verzeichnis namens ```.azureml``` Ihr Basisverzeichnis und geben Sie die Parameter aus dem Azure ML Arbeitsbereich:

Settings.JSON Struktur:

    {"workspace":{
    "id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
    "authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
    }}


#### <a name="build-a-model-in-r-and-publish-it-in-azure-ml"></a>Ein Modell r und Veröffentlichen in Azure ML

    library(AzureML)
    ws <- workspace(config="~/.azureml/settings.json")

    if(!require("lme4")) install.packages("lme4")
    library(lme4)
    set.seed(1)
    train <- sleepstudy[sample(nrow(sleepstudy), 120),]
    m <- lm(Reaction ~ Days + Subject, data = train)

    # Define a prediction function to publish based on the model:
    sleepyPredict <- function(newdata){
        predict(m, newdata=newdata)
    }

    ep <- publishWebService(ws, fun = sleepyPredict, name="sleepy lm", inputSchema = sleepstudy, data.frame=TRUE)

#### <a name="consume-the-model-deployed-in-azure-ml"></a>Verwenden Sie das Modell in Azure ML bereitgestellt

Um das Modell aus einer Clientanwendung zu verarbeiten, verwenden wir Azure Machine Learning Library veröffentlichten Webdienst mit Namen Nachschlagen der `services` API-Aufruf um den Endpunkt zu bestimmen. Dann Sie rufen die `consume` funktionieren und in Datenrahmen vorhergesagt werden.
Im folgende Code wird verwendet, verwendet das Modell als Webdienst Azure Machine Learning veröffentlicht.


    library(AzureML)
    library(lme4)
    ws <- workspace(config="~/.azureml/settings.json")

    s <-  services(ws, name = "sleepy lm")
    s <- tail(s, 1) # use the last published function, in case of duplicate function names

    ep <- endpoints(ws, s)

    # OK, try this out, and compare with raw data
    ans = consume(ep, sleepstudy)$ans

Weitere Informationen zu Azure Machine Learning R-Bibliothek finden Sie [hier](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).


## <a name="4-administer-your-azure-resources-using-azure-portal-or-powershell"></a>4. mit Azure-Portal oder Powershell Azure Ressourcen verwalten

Die DSVM nicht können Sie Analytics lokal auf dem virtuellen Computer erstellen, nur Zugriff auf Microsoft Azure Cloud Services ermöglicht. Azure bietet mehrere Compute, Speicher, Datendienste Analytics und andere Dienste, die Verwaltung und die DSVM zugreifen.

Azure-Abonnement und Cloud Ressourcen verwalten können Ihrem Browser und [Azure-Portal](https://portal.azure.com)zeigen. Azure Powershell können Sie Ihre Azure-Abonnement und Ressourcen über ein Skript.
Sie können eine Verknüpfung auf dem Desktop oder im Startmenü mit dem Titel "Microsoft Azure Powershell" Azure Powershell ausführen. [Microsoft Azure Powershell](../powershell-azure-resource-manager.md) -Dokumentation finden Sie weitere Hinweise, wie Sie Ihre Azure-Abonnement und Ressourcen mithilfe von Windows Powershell-Skripts verwalten können.


## <a name="5-extend-your-storage-space-with-a-shared-file-system"></a>5. Erweitern Sie Speicherplatz mit einem freigegebenen Dateisystem

Datenwissenschaftler können große Datasets, Code oder andere Ressourcen im Team freigeben. DSVM selbst hat ca. 70GB Speicherplatz. Um den Speicher zu erweitern, können Sie den Dienst Azure und entweder auf den DSVM bereitstellen oder über eine REST-API zugreifen.   


>[AZURE.NOTE] Der maximale Speicherplatz der Azure-Dateidienst Freigabe ist 5TB und Datei Größe beträgt 1TB.   


Azure Powershell können eine Azure-Dateidienst Freigabe erstellt. Hier ist das Skript unter Azure PowerShell Azure Service Dateifreigabe erstellen.

    # Authenticate to Azure.
    Login-AzureRmAccount
    # Select your subscription
    Get-AzureRmSubscription –SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
    # Create a new resource group.
    New-AzureRmResourceGroup -Name <dsvmdatarg>
    # Create a new storage account. You can reuse existing storage account if you wish.
    New-AzureRmStorageAccount -Name <mydatadisk> -ResourceGroupName <dsvmdatarg> -Location "<Azure Data Center Name For eg. South Central US>" -Type "Standard_LRS"
    # Set your current working storage account
    Set-AzureRmCurrentStorageAccount –ResourceGroupName "<dsvmdatarg>" –StorageAccountName <mydatadisk>

    # Create a Azure File Service Share
    $s = New-AzureStorageShare <<teamsharename>>
    # Create a directory under the FIle share. You can give it any name
    New-AzureStorageDirectory -Share $s -Path <directory name>
    # List the share to confirm that everything worked
    Get-AzureStorageFile -Share $s


Erstellung eine Azure Dateifreigabe können Sie es in einer virtuellen Maschine in Azure bereitstellen. Es wird dringend empfohlen die VM in Azure Speicherkonto Latenz und Data transfergebühren zu demselben Rechenzentrum ist. Hier ist die Befehle, das Laufwerk auf die DSVM, die in Azure Powershell ausgeführt werden können.


    # Get storage key of the storage account that has the Azure file share from Azure portal. Store it securely on the VM to avoid prompted in next command.
    cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

    # Mount the Azure file share as Z: drive on the VM. You can chose another drive letter if you wish
    net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>


Jetzt können Sie dieses Laufwerk zugreifen, wie alle normales Laufwerk des virtuellen Computers.

## <a name="6-share-code-with-your-team-using-github"></a>6. Freigeben von Code mit Ihrem Team mit Github

Github ist ein viel Beispielcode und Quellen für verschiedene Technologien von Entwicklern gemeinsam verwendet verschiedene Tools finden Sie Code-Repository. Git verwendet als Technologie zu speichern Versionen des Codes. Github ist auch eine Plattform, erstellen Sie eigene Repository Speichern Ihres Teams freigegebenen Code und Dokumentation, Versionskontrolle implementieren und auch steuern, die Zugang zu Code beitragen. Besuchen Sie die [Hilfeseiten Github](https://help.github.com/) Informationen mit Git. Github können als eine Möglichkeit, mit Ihrem Team zusammenarbeiten, verwenden Sie Code der Gemeinschaft entwickelt und Code in der Gemeinschaft beitragen.

Die DSVM ist bereits mit Clienttools auf beide Befehlszeile auch GUI Github Repository zugreifen. Das Befehlszeilentool von Git und Github heißt Git Bash. Auf dem DSVM installiert Visual Studio hat Extensions Git Start-Symbole finden Sie diese Tools auf das Startmenü und den Desktop.

Zum Herunterladen von Code aus einem Github Repository Sie verwenden die ```git clone``` Befehl. Zum Beispiel in das aktuelle Verzeichnis von Microsoft veröffentlichten Daten Wissenschaft Repository herunterladen können Sie folgenden Befehl ausführen, einmal in ```git-bash```.

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

In Visual Studio können Sie die gleichen Klonvorgang tun. Der Screenshot unten veranschaulicht Git und Github Tools in Visual Studio zugreifen.

![Git in Visual Studio](./media/machine-learning-data-science-vm-do-ten-things/VSGit.PNG)


Weitere Informationen zur Verwendung von Git zu mit dem Github Repository aus mehreren verfügbaren github.com finden. [Spickzettel:](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) ist eine hilfreiche Referenz.


## <a name="7-access-various-azure-data-and-analytics-services"></a>7. Datenzugriffsdienste verschiedene Azure Daten und Analysen

### <a name="azure-blob"></a>Azure Blob

Azure Blob ist eine zuverlässige und ökonomische Cloud-Speicher für große und kleine. Wir betrachten Sie, wie Access-Daten in Azure Blob zu Azure BLOB-Daten verschoben werden können.

**Voraussetzung**

- **Erstellen Sie Ihr Konto Azure BLOB-Speicher von [Azure-Portal](https://portal.azure.com).**

![Create_Azure_Blob](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)


- Bestätigen Sie, dass das vorinstallierte AzCopy Befehlszeilenprogramm am befindet ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```. Sie können das Verzeichnis azcopy.exe auf die Umgebungsvariable PATH vermeiden Sie vollständigen Befehlspfad beim Ausführen dieses Tools hinzufügen. Weitere Informationen über AzCopy-Tool finden Sie in [AzCopy-Dokumentation](../storage/storage-use-azcopy.md)

- Starten Sie mit dem Azure-Speicher-Explorer. Sie können [Microsoft Azure Storage](http://storageexplorer.com/)Explorer heruntergeladen werden. 

![AzureStorageExplorer_v4](./media/machine-learning-data-science-vm-do-ten-things/AzureStorageExplorer_v4.png)


**Verschieben von Daten von VM in Azure Blob: AzCopy**

Zum Verschieben von Daten zwischen lokalen Dateien und BLOB-Speicher können Sie AzCopy in der Befehlszeile oder PowerShell:

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

Ersetzen Sie den Pfad, wo die Datei gespeichert ist, **Mystorageaccount** auf die BLOB-speicherkontoname **Mycontainer** auf den Containernamen **Speicherschlüssel Konto** den Zugriffsschlüssel BLOB-Speicher **C:\myfolder** . Ihre Anmeldeinformationen Speicher finden Sie in [Azure-Portal](https://portal.azure.com).

![StorageAccountCredential_v2](./media/machine-learning-data-science-vm-do-ten-things/StorageAccountCredential_v2.png)

AzCopy Befehl in PowerShell oder von einer Befehlszeile ausführen. Hier ist einige Verwendungsbeispiel AzCopy Befehl:


    # Copy *.sql from local machine to a Azure Blob
    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

    # Copy back all files from Azure Blob container to Local machine

    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S



Nach Ausführen den Befehl AzCopy ein Azure Blob kopiert wird Ihre Datei zeigt sich in Azure Storage Explorer kurz angezeigt.

![AzCopy_run_finshed_Storage_Explorer_v3](./media/machine-learning-data-science-vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)


**Verschieben von Daten von VM in Azure Blob: Azure-Speicher-Explorer**

Außerdem können Sie Daten aus der lokalen Datei in die VM mit Azure Storage Explorer:

- Zum Hochladen von Daten in einen Container wählen Sie den Zielcontainer aus und auf die Schaltfläche **Hochladen** .![](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)
- **...** Rechts neben **dem Feld** auf, wählen Sie eine oder mehrere Dateien aus dem Dateisystem hochladen und klicken Sie auf **Hochladen** , zunächst die Dateien hochladen.![](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)


**Lesen von Daten aus Azure Blob: AML Reader-Modul**

Ein **Datenimport-Modul** können Sie in Azure Machine Learning Studio das Blob Daten gelesen.


![AML_ReaderBlob_Module_v3](./media/machine-learning-data-science-vm-do-ten-things/AML_ReaderBlob_Module_v3.png)


**Lesen von Daten aus Azure Blob: Python ODBC**

**BlobService** -Bibliothek können Sie Daten direkt aus BLOBs in einem Jupyter Notebook oder Python lesen.

Zunächst importieren Sie erforderliche Pakete:

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random

Schließen Sie Ihre Azure Blob-Anmeldeinformationen und Lesen Sie Daten von Blob:

    CONTAINERNAME = 'xxx'
    STORAGEACCOUNTNAME = 'xxxx'
    STORAGEACCOUNTKEY = 'xxxxxxxxxxxxxxxx'
    BLOBNAME = 'nyctaxidataset/nyctaxitrip/trip_data_1.csv'
    localfilename = 'trip_data_1.csv'
    LOCALDIRECTORY = os.getcwd()
    LOCALFILE =  os.path.join(LOCALDIRECTORY, localfilename)

    #download from blob
    t1 = time.time()
    blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILE)
    t2 = time.time()
    print(("It takes %s seconds to download "+BLOBNAME) % (t2 - t1))

    #unzipping downloaded files if needed
    #with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
    #    z.extractall(LOCALDIRECTORY)

    df1 = pd.read_csv(LOCALFILE, header=0)
    df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
    print 'the size of the data is: %d rows and  %d columns' % df1.shape

Die Daten werden in als Datenrahmen gelesen:

![IPNB_data_readin](./media/machine-learning-data-science-vm-do-ten-things/IPNB_data_readin.PNG)


### <a name="azure-data-lake"></a>Azure Data Lake

Azure Datenspeicher See ist ein Repository Großkonzerne für big Data Analytics Arbeitslasten und mit Hadoop verteilt Datei System bietet. Es arbeitet mit Hadoop-Ökosystem und Azure Data Lake Analytics. Zum Verschieben von Daten in den Datenspeicher See Azure und führen Analysen mithilfe von Azure Data Lake Analytics erfahren.

**Voraussetzung**

- Erstellen Sie Ihre Azure Data Lake Analytics in [Azure-Portal](https://portal.azure.com).

![Azure_Data_Lake_Create_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_Create_v2.png)


- **Azure Data Lake-Tools** in **Visual Studio** finden Sie auf diesen [Link](https://www.microsoft.com/download/details.aspx?id=49504) wird im Visual Studio Community Edition installiert auf dem virtuellen Computer. Starten Visual Studio und Azure-Abonnement anmelden sehen Sie Ihre Azure Data Analytics Konto und Speicher im linken Bereich von Visual Studio.

![Azure_Data_Lake_PlugIn_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)


**Verschieben von Daten von VM Daten See: Azure Data Lake Explorer**

**Azure Data Lake Explorer** können Sie Daten aus lokalen Dateien in Ihrem System Data Lake Speicher hochladen.

![Azure_Data_Lake_UploadData](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_UploadData.PNG)

Sie können auch eine Datenpipeline um Ihre Datentransfers zu oder von Azure Data Lake productionize erstellen mit [Azure Data Factory(ADF)](https://azure.microsoft.com/services/data-factory/). Wir bezeichnen diese [Artikel](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) , der Sie durch die Schritte zum Erstellen der Datenpipelines führt.

**Lesen von Daten aus Azure Blob Daten See: U-SQL**

Wenn Ihre Daten in Azure BLOB-Speicher befindet, können Sie Daten direkt von Azure-Speicher Blob in U-SQL-Abfrage lesen. Vor dem Verfassen der U-SQL-Abfrage sicher Azure Data Lake die BLOB-Speicher-Konto verknüpft ist. **Azure**-Portal gehen finden Sie Azure Data Lake Analytics Dashboard, auf **Datenquelle hinzufügen**, wählen Sie Speicher in **Azure-Speicher** und schließen Sie Ihren Azure Storage Benutzernamen und Schlüssel. Klicken Sie dann können auf die Daten im Speicherkonto Sie.

![](./media/machine-learning-data-science-vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)


In Visual Studio können Sie Daten aus dem BLOB-Speicher lesen, sind einige Datenmanipulation Featureentwicklung und resultierenden Daten oder Azure Data Lake Azure BLOB-Speicher. Wenn Sie die Daten im BLOB-Speicher, verwenden **Wasb: / /**; Wenn Sie die Daten in Azure Data Lake, verwenden **Swbhdfs: / /**

![](./media/machine-learning-data-science-vm-do-ten-things/USQL_Read_Blob_v2.PNG)

Sie können die folgenden U-SQL-Abfragen in Visual Studio:

    @a =
        EXTRACT medallion string,
                hack_license string,
                vendor_id string,
                rate_code string,
                store_and_fwd_flag string,
                pickup_datetime string,
                dropoff_datetime string,
                passenger_count int,
                trip_time_in_secs double,
                trip_distance double,
                pickup_longitude string,
                pickup_latitude string,
                dropoff_longitude string,
                dropoff_latitude string

        FROM "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Input Data File Name>"
        USING Extractors.Csv();

    @b =
        SELECT vendor_id,
        COUNT(medallion) AS cnt_medallion,
        SUM(passenger_count) AS cnt_passenger,
        AVG(trip_distance) AS avg_trip_dist,
        MIN(trip_distance) AS min_trip_dist,
        MAX(trip_distance) AS max_trip_dist,
        AVG(trip_time_in_secs) AS avg_trip_time
        FROM @a
        GROUP BY vendor_id;

    OUTPUT @b   
    TO "swebhdfs://<Azure Data Lake Storage Account Name>.azuredatalakestore.net/<Folder Name>/<Output Data File Name>"
    USING Outputters.Csv();

    OUTPUT @b   
    TO "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Output Data File Name>"
    USING Outputters.Csv();



Nach die Abfrage an den Server gesendet wird, wird ein Diagramm des Status des Auftrags angezeigt.

![](./media/machine-learning-data-science-vm-do-ten-things/USQL_Job_Status.PNG)


**Abfragen von Daten aus Data Lake: U-SQL**

Nachdem das Dataset in Azure Data See aufgenommen wird, können Sie [U-SQL-Sprache](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) Abfragen und Durchsuchen der Daten. U-SQL-Sprache T-SQL ähnelt, aber einige Features von C# kombiniert, sodass Benutzer angepasste Module, benutzerdefinierten Funktionen usw. schreiben können. Die Skripts können Sie im vorherigen Schritt.

Nach die Abfrage an Tripdata_summary-Server übermittelt. CSV finden Sie in Kürze in **Azure Data Lake Explorer**möglicherweise Vorschau Daten durch Rechtsklick Datei;

![](./media/machine-learning-data-science-vm-do-ten-things/USQL_create_summary.png)

Um die Informationen anzuzeigen:

![](./media/machine-learning-data-science-vm-do-ten-things/USQL_tripdata_summary.png)


### <a name="hdinsight-hadoop-clusters"></a>HDInsight Hadoop-Cluster

Azure HDInsight ist ein verwalteter Apache Hadoop, Funken, HBase und Sturm-Dienst in der Cloud. Sie können problemlos mit Azure HDInsight Daten Wissenschaft virtuellen Computer arbeiten.

**Voraussetzung**

- Erstellen Sie Ihr Konto Azure BLOB-Speicher von [Azure-Portal](https://portal.azure.com). Dieses Speicherkonto zum Speichern von Daten für HDInsight-Cluster.

![](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

- Azure HDInsight Hadoop Cluster von [Azure-Portal](machine-learning-data-science-customize-hadoop-cluster.md) anpassen

  - Verknüpfen Sie das Speicherkonto bei der Erstellung mit HDInsight Cluster erstellt. Dieser Speicher-Konto dient für den Zugriff auf Daten innerhalb des Clusters verarbeitet werden können.

![](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_v4.PNG)

  - Sie müssen der Head-Knoten des Clusters **RAS** aktivieren, nachdem es erstellt wurde. Sie hier geben (anders als für den Cluster erstellen) RAS-Anmeldeinformationen speichern: Sie benötigen sie unten.

![](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

  - Erstellen Sie einen Arbeitsbereich Azure ML. In diesem Arbeitsbereich ML werden Computer lernen Experimente. Wählen Sie die hervorgehobenen Optionen Portal wie im folgenden Screenshot gezeigt.

![](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space.PNG)


  - Geben Sie die Parameter für den Arbeitsbereich Azure ML

![](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

  - Upload von Daten mit IPython Notebook. Zunächst die erforderlichen Pakete importieren, schließen Sie Anmeldeinformationen, Db in das Speicherkonto erstellen und Laden von Daten auf HDI Cluster.


        #Import required Packages
        import pyodbc
        import time as time
        import json
        import os
        import urllib
        import urllib2
        import warnings
        import re
        import pandas as pd
        import matplotlib.pyplot as plt
        from azure.storage.blob import BlobService
        warnings.filterwarnings("ignore", category=UserWarning, module='urllib2')


        #Create the connection to Hive using ODBC
        SERVER_NAME='xxx.azurehdinsight.net'
        DATABASE_NAME='nyctaxidb'
        USERID='xxx'
        PASSWORD='xxxx'
        DB_DRIVER='Microsoft Hive ODBC Driver'
        driver = 'DRIVER={' + DB_DRIVER + '}'
        server = 'Host=' + SERVER_NAME + ';Port=443'
        database = 'Schema=' + DATABASE_NAME
        hiveserv = 'HiveServerType=2'
        auth = 'AuthMech=6'
        uid = 'UID=' + USERID
        pwd = 'PWD=' + PASSWORD
        CONNECTION_STRING = ';'.join([driver,server,database,hiveserv,auth,uid,pwd])
        connection = pyodbc.connect(CONNECTION_STRING, autocommit=True)
        cursor=connection.cursor()


        #Create Hive database and tables
        queryString = "create database if not exists nyctaxidb;"
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.trip
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            rate_code string,
                            store_and_fwd_flag string,
                            pickup_datetime string,
                            dropoff_datetime string,
                            passenger_count int,
                            trip_time_in_secs double,
                            trip_distance double,
                            pickup_longitude double,
                            pickup_latitude double,
                            dropoff_longitude double,
                            dropoff_latitude double)  
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.fare
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            pickup_datetime string,
                            payment_type string,
                            fare_amount double,
                            surcharge double,
                            mta_tax double,
                            tip_amount double,
                            tolls_amount double,
                            total_amount double)
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)


        #Upload data from blob storage to HDI cluster
        for i in range(1,13):
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxitripraw2/trip_data_%d.csv' INTO TABLE nyctaxidb2.trip PARTITION (month=%d);"%(i,i)
            cursor.execute(queryString)
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxifareraw2/trip_fare_%d.csv' INTO TABLE nyctaxidb2.fare PARTITION (month=%d);"%(i,i)  
            cursor.execute(queryString)


- Alternativ können Sie diese [Exemplarische Vorgehensweise](machine-learning-data-science-process-hive-walkthrough.md) zum Hochladen von NYC Taxi Daten HDI Cluster ausführen. Wichtige Schritte umfassen:

    - AzCopy: ZIP-CSV aus Blob für den öffentlichen Ordner Lokale herunterladen
    - AzCopy: Hochladen Sie unzipped CSV von lokalen Ordner HDI Cluster
    - Melden Sie sich bei dem Head-Knoten des Clusters Hadoop und explorativen Datenanalyse vorbereiten

Nach dem Laden der Daten HDI Cluster können Sie Ihre Daten in Azure Storage Explorer überprüfen. Und Sie haben eine Datenbank Nyctaxidb HDI Clusters erstellt.


**Durchsuchen von Daten: Struktur Abfragen in Python**

Da in Hadoop Cluster Daten können das Pyodbc-Paket Sie Hadoop Cluster und Abfrage Struktur durchsuchen und Featureentwicklung mit Datenbank verbinden. Sie können die vorhandenen Tabellen erforderliche Schritt erstellten anzeigen.

    queryString = """
        show tables in nyctaxidb2;
        """
    pd.read_sql(queryString,connection)


![](./media/machine-learning-data-science-vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

Betrachten wir die Anzahl der Datensätze in jedes Monats und die Häufigkeit der Spitzen oder nicht in der Tabelle Reise:

    queryString = """
        select month, count(*) from nyctaxidb.trip group by month;
        """
    results = pd.read_sql(queryString,connection)

    %matplotlib inline

    results.columns = ['month', 'trip_count']
    df = results.copy()
    df.index = df['month']
    df['trip_count'].plot(kind='bar')


![](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Number_Records_by_Month_v3.PNG)


    queryString = """
        SELECT tipped, COUNT(*) AS tip_freq
        FROM
        (
            SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
            FROM nyctaxidb.fare
        )tc
        GROUP BY tipped;
        """
    results = pd.read_sql(queryString,connection)

    results.columns = ['tipped', 'trip_count']
    df = results.copy()
    df.index = df['tipped']
    df['trip_count'].plot(kind='bar')


![](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Frequency_tip_or_not_v3.PNG)

Wir können auch berechnet den Abstand Abholort Dropoff Position und Abstand Reise vergleichen.

    queryString = """
                    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
                        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
                        *radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
                        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
                        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
                        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*
                        pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance
                        from nyctaxidb.trip
                        where month=1
                            and pickup_longitude between -90 and -30
                            and pickup_latitude between 30 and 90
                            and dropoff_longitude between -90 and -30
                            and dropoff_latitude between 30 and 90;
                """
    results = pd.read_sql(queryString,connection)
    results.head(5)


![](./media/machine-learning-data-science-vm-do-ten-things/Exploration_compute_pickup_dropoff_distance_v2.PNG)

    results.columns = ['pickup_longitude', 'pickup_latitude', 'dropoff_longitude',
                       'dropoff_latitude', 'trip_distance', 'trip_time_in_secs', 'direct_distance']
    df = results.loc[results['trip_distance']<=100] #remove outliers
    df = df.loc[df['direct_distance']<=100] #remove outliers
    plt.scatter(df['direct_distance'], df['trip_distance'])


![](./media/machine-learning-data-science-vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)


Jetzt bereite Down-sampled (1 %) verschiedene Daten für die Modellierung. Wir können diese Daten in AML Reader Modul.


        queryString = """
        create  table if not exists nyctaxi_downsampled_dataset_testNEW (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\\n'
        stored as textfile;
        """
        cursor.execute(queryString)

        --- now insert contents of the join into the above internal table

        queryString = """
        insert overwrite table nyctaxi_downsampled_dataset_testNEW
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class
        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance,
        rand() as sample_key

        from trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01
        """
        cursor.execute(queryString)

Nach einer Weile sehen Sie, dass die Daten in Clustern Hadoop geladen wurde:

    queryString = """
        select * from nyctaxi_downsampled_dataset limit 10;
        """
    cursor.execute(queryString)
    pd.read_sql(queryString,connection)


![](./media/machine-learning-data-science-vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)


**Lesen von Daten aus HDI mit AML: Reader-Modul**

Auch können das **Reader** -Modul in AML Studio Zugriff auf die Datenbank Hadoop Cluster. Die Anmeldeinformationen der HDI Cluster und Azure Storage-Konto schließen und Sie lernmodelle mit Datenbank in HDI Cluster erstellen.

![](./media/machine-learning-data-science-vm-do-ten-things/AML_Reader_Hive.PNG)

Erzielte Dataset kann dann angezeigt:

![](./media/machine-learning-data-science-vm-do-ten-things/AML_Model_Results.PNG)


### <a name="azure-sql-data-warehouse--databases"></a>Azure SQL Data Warehouse und Datenbanken

Azure SQL Data Warehouse ist eine flexible Datawarehouse als Dienst mit SQL Server Enterprise-Klasse.

Sie können Ihre Azure SQL Data Warehouse gemäß der Anleitung in diesem [Artikel](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)beschriebenen bereitstellen. Nach der Bereitstellung von Azure SQL Data Warehouse können Sie Daten hochladen, Untersuchung und Modellierung mit Daten im SQL Data Warehouse dieser [exemplarischen Vorgehensweise](machine-learning-data-science-process-sqldw-walkthrough.md) verwenden.

#### <a name="azure-documentdb"></a>Azure DocumentDB

Azure DocumentDB ist eine NoSQL-Datenbank in der Cloud. Sie können Dokumente wie JSON und ermöglicht das Speichern und Abfragen der Dokumente.

Pro Komponenten so die DSVM DocumentDB zugreifen möchten.

1. DocumentDB Python SDK installieren (führen Sie ```pip install pydocumentdb``` über Befehlszeile)
1. Erstellen Sie DocumentDB-Konto und Dokument-DB-Datenbank von [Azure-portal](https://portal.azure.com)
1. "DocumentDB-Migrationstools" [hier](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) herunterladen und in ein Verzeichnis Ihrer Wahl zu extrahieren
1. Importieren von JSON (Vulkan) Daten mit Befehlsparameter Migration Tool (dtui.exe aus dem Verzeichnis, das Migrationstool DocumentDB installiert) auf ein [Blob für den öffentlichen](https://cahandson.blob.core.windows.net/samples/volcano.json) in DocumentDB gespeichert. Geben Sie die Quelle und Ziel Positionsparameter von unten.

    /s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/; AccountKey = [[Schlüssel]; Datenbank = Vulkan /t.Collection:volcano1

Nach dem Datenimport können Sie zur Jupyter und öffnen Sie das Notizbuch mit dem Namen *DocumentDBSample* enthält Python-Code Zugriff auf DocumentDB und einige grundlegende Abfragen. Sie erfahren mehr über DocumentDB Service [-Dokumentation](https://azure.microsoft.com/documentation/learning-paths/documentdb/)


## <a name="8-build-reports-and-dashboard-using-the-power-bi-desktop"></a>8. erstellen Sie Berichte und Dashboards mit Power BI Desktop

Wir visualisieren Sie Vulkan JSON-Datei sahen wir im obigen Beispiel DocumentDB, Power BI visual Einblicke in den Daten. Detaillierte Schritte sind [Power BI-Artikel](../documentdb/documentdb-powerbi-visualize.md). Die Schritte sind unten:

1. Öffnen Sie Power BI Desktop, und führen Sie "Daten abrufen". Geben Sie die URL als: https://cahandson.blob.core.windows.net/samples/volcano.json
2. Die JSON-Datensätze importiert als Liste sollte angezeigt werden.
3. Konvertieren der Liste in eine Tabelle mit demselben PowerBI arbeiten können
4. Erweitern Sie die Spalten durch Klicken auf das Erweiterungssymbol (eine mit dem Symbol "Pfeil nach links und Pfeil nach rechts" auf der rechten Seite der Spalte)
5. Beachten Sie, dass ein "Datensatz" Feld befindet. Erweitern Sie den Datensatz, und wählen Sie die Koordinaten. Koordinate ist einer Listenspalte
6. Hinzufügen einer neuen Spalte-Koordinate Listenspalte in eine durch Kommas getrennte LatLong Spalte verkettet zwei Elemente in der Formel-Koordinate Liste konvertieren ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.
7. Außerdem konvertieren die ```Elevation``` Spalte Dezimal- und wählen Sie **Schließen** und **anwenden**.

Statt Schritte können Sie folgenden Code einfügen, dass Skripts Schritte im erweiterten Editor in PowerBI heraus, die die Datentransformationen in einer Abfragesprache schreiben ermöglicht.


    let
        Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
        #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
        #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
        #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
        #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
        #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
    in
        #"Changed Type"



Sie haben nun die Daten in Ihrem Power BI-Datenmodell. Der Power BI Desktop sollte wie folgt aussehen.

![](./media/machine-learning-data-science-vm-do-ten-things/PowerBIVolcanoData.png)

Erstellen von Berichten und Visualisierung mit dem Datenmodell kann gestartet werden. Sie können die Schritte in diesem [Artikel Power BI](../documentdb/documentdb-powerbi-visualize.md#build-the-reports) erstellen folgen. Dadurch werden ein Bericht, der wie folgt aussieht.

![Power BI Desktop Berichtsansicht - Power BI-connector](./media/machine-learning-data-science-vm-do-ten-things/power_bi_connector_pbireportview2.png)

## <a name="9-dynamically-scale-your-dsvm-to-meet-your-project-needs"></a>9. dynamisch skaliert die DSVM Ihren Bedürfnissen Projekt

Sie können nach Bedarf Projekt DSVM skalieren. Möchten Sie nicht die VM abends oder am Wochenende verwenden, können Sie nur die VM aus dem [Azure-Portal](https://portal.azure.com)Herunterfahren.

>[AZURE.NOTE]  Berechnen Gebühren fallen nur Schaltfläche Herunterfahren des Betriebssystems auf dem virtuellen Computer verwenden.  

Benötigen Sie eine umfangreiche Analyse und weitere CPU oder Arbeitsspeicher oder Datenträger Kapazität finden Sie große Auswahl VM CPU-Kerne, Speicher und Datenträgertypen (einschließlich Solid State-Laufwerke), die Ihre Compute Budget und. Die vollständige Liste der VMs mit ihren stündlich compute-Preise auf [Azure VMs Preise](https://azure.microsoft.com/pricing/details/virtual-machines/) verfügbar ist.

Entsprechend vermindert die Notwendigkeit Verarbeitungskapazität VM (Beispiel: eine große Arbeitslast Spark-Cluster zu einem Hadoop verschoben), Cluster von [Azure-Portal](https://portal.azure.com) und gehen die VM-Instanz zu verkleinern. Hier ist ein Screenshot.


![](./media/machine-learning-data-science-vm-do-ten-things/VMScaling.PNG)


## <a name="10-install-additional-tools-on-your-virtual-machine"></a>10. installieren Sie zusätzlicher Tools auf dem virtuellen Computer

Wir haben mehrere Tools, die wir werden glauben zu viele allgemeinen Daten Analytics muss verpackt und Zeit sparen sollten vermeiden zu installieren und Konfigurieren Ihrer Umgebung jeweils sparen Sie bezahlen nur für Ressourcen, die Sie verwenden.

Sie können andere Azure Daten und Analysen Dienste, die in diesem Artikel zu Ihrer Umgebung Analytics Profil nutzen. Wir wissen, dass in einigen Fällen Bedarf zusätzliche Tools wie einige proprietäre Drittanbieter-Tools erfordern. Sie haben volle Administratorrechte auf dem virtuellen Computer neue Tools installieren, die Sie benötigen. Sie können auch zusätzliche Pakete installieren, Python und R nicht vorinstalliert. Für Python können entweder ```conda``` oder ```pip```. Für R können Sie die ```install.packages()``` R console oder mithilfe der IDE und wählen Sie "**Pakete** -> **Pakete installieren**".

## <a name="summary"></a>Zusammenfassung
Dies sind nur einige der Dinge, die Sie unter Microsoft Data Science Virtual Machine können. Es gibt viele weitere Maßnahmen ergreifen, um eine effektive Analyse-Umgebung erstellen.
