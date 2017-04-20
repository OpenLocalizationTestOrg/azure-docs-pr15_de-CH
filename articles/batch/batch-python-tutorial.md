<properties
    pageTitle="Lernprogramm - Erste Schritte mit Azure Batch Python-Client | Microsoft Azure"
    description="Lernen Sie die grundlegenden Konzepte von Azure Batch und Batch-Dienst mit einem einfachen Szenario entwickeln"
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/27/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-python-client"></a>Erste Schritte mit Azure Batch Python-client

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Grundlagen der [Azure Batch] [ azure_batch] und [Batch Python] [ py_azure_sdk] Client wie wir eine kleine Stapel Anwendung in Python geschrieben wurden. Betrachten wir zwei Beispiel Skripts mit der Batch-Dienst verarbeitet eine parallele Arbeitslast auf virtuelle Linux-Computer in die Cloud und Interaktion mit [Azure Storage](./../storage/storage-introduction.md) Datei Staging und abrufen. Sie werden lernen einen häufige Stapelverarbeitung Anwendungsworkflow Basis verstehen die Hauptkomponenten der Batch Jobs, Aufgaben, Pools und compute-Knoten.

![Batch-Lösung Workflow (Basic)][11]<br/>

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Artikel werden Python und mit Linux Kenntnisse vorausgesetzt. Es wird davon ausgegangen, dass Sie Konto erstellen Anforderungen, die für Azure und den Stapel und Speicher unten angegeben werden können.

### <a name="accounts"></a>Konten

- **Ein Azure-Konto**: haben Sie bereits ein Azure-Abonnement [erstellen ein kostenloses Azure-Konto][azure_free_account].
- **Batch-Konto**: haben Sie ein Azure-Abonnement [Azure Batch registrieren](batch-account-create-portal.md).
- **Konto**: Siehe [erstellen ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) in [von Azure-Speicherkonten](../storage/storage-create-storage-account.md).

### <a name="code-sample"></a>Codebeispiel

Tutorial Python- [Beispielcode] [ github_article_samples] ein viele Batch Code befindet sich in [Azure-Batch-Beispiele] [ github_samples] GitHub-Repository. Sie können die Beispiele durch Klicken auf **Clone oder Download > ZIP herunterladen** auf der Seite Home Repository oder indem [Azure Batch-Beispiele master.zip] [ github_samples_zip] direkten Download-Link. Nachdem Sie den Inhalt der ZIP-Datei extrahiert haben, befinden sich zwei Skripts für dieses Lernprogramm der `article_samples` Verzeichnis:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Python-Umgebung

Führen Sie das Beispielskript *python_tutorial_client.py* auf der lokalen Arbeitsstation benötigen Sie **Python-Interpreter** mit Version **2.7** oder **3.3 +**. Das Skript wurde unter Linux und Windows getestet.

### <a name="cryptography-dependencies"></a>Kryptografie Abhängigkeiten

Installieren Sie die Abhängigkeit der [Kryptografie] [ crypto] Bibliothek erforderlich die `azure-batch` und `azure-storage` Python-Pakete. Führen Sie einen der folgenden Vorgänge für Ihre Plattform oder auf die [Kryptografie-Installation] [ crypto_install] Weitere Informationen:

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`

* CentOS

    `yum update && yum install -y gcc openssl-dev libffi-devel python-devel`

* SLES-OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`

* Windows

    `pip install cryptography`

>[AZURE.NOTE] Wenn Python 3.3 + unter Linux verwenden Sie python3 äquivalente Abhängigkeitsscan Python. Auf Ubuntu:`apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`

### <a name="azure-packages"></a>Azure-Pakete

Installieren Sie dann die Pakete **Azure Batch** und **Azure Storage** Python. Sie erreichen dies hier *requirements.txt* mit **Pip** :

`/azure-batch-samples/Python/Batch/requirements.txt`

Problem **Pip** Befehl Batch und Speicher Pakete installieren:

`pip install -r requirements.txt`

Oder Sie können [Azure Batch] [ pypi_batch] und [Azure Storage] [ pypi_storage] Python Pakete manuell:

`pip install azure-batch`<br/>
`pip install azure-storage`

> [AZURE.TIP] Sie müssen die Befehle mit Präfix `sudo` verwenden ein Konto ohne Berechtigungen. Z. B. `sudo pip install -r requirements.txt`. Weitere Informationen zur Installation von Python-Pakete finden Sie unter [Pakete installieren] [ pypi_install] auf readthedocs.io.

## <a name="batch-python-tutorial-code-sample"></a>Stapel Tutorial Codebeispiel werden Python

Im Batch Python Tutorial Codebeispiel besteht aus zwei Python-Skripten und einige Datendateien.

- **python_tutorial_client.py**: Interaktion mit den Stapel und Speicher parallele Arbeitslast auf Compute-Knoten (virtuelle Maschinen) ausführen. Das *python_tutorial_client.py* -Skript wird auf der lokalen Arbeitsstation ausgeführt.

- **python_tutorial_task.py**: das Skript läuft auf compute-Knoten in Azure die Arbeit ausführen. Im Beispiel analysiert *python_tutorial_task.py* den Text in einer Downloaddatei aus dem Azure-Speicher (Eingabedatei). Dann erzeugt eine Textdatei (Ausgabedatei) mit einer Liste der oberen drei Wörter, die in der Eingabedatei angezeigt. Nach Erstellung die Ausgabedatei hochgeladen *python_tutorial_task.py* die Datei auf Azure-Speicher. Dadurch Clientskript auf Ihre Arbeitsstation heruntergeladen. Das *python_tutorial_task.py* -Skript führt parallel auf mehreren Computeknoten im Batch-Dienst.

- **./data/taskdata\*.txt**: Diese drei Dateien ermöglichen die Eingabe für die Aufgaben, die auf den Compute-Knoten ausgeführt.

Das folgende Diagramm zeigt wichtige Vorgänge, die von Client und Task-Skripts ausgeführt werden. Dieser grundlegende Workflow ist typisch für viele Compute-Lösungen, die mit erstellt werden. Während sie nicht jede Funktion in der Batch-Dienst zeigen, sind fast jeder Batch Szenario Teile dieses Workflows.

![Batch Beispielworkflow][8]<br/>

[**Schritt 1.**](#step-1-create-storage-containers) Erstellen Sie **Container** in Azure BLOB-Speicher.<br/>
[**Schritt 2.**](#step-2-upload-task-script-and-data-files) Hochladen Sie Skript und Eingabe Dateien auf Container.<br/>
[**Schritt 3.**](#step-3-create-batch-pool) Erstellen Sie Batch- **pool**<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** Pool **StartTask** downloads Aufgabe Skript (python_tutorial_task.py) Knoten, wie sie das Pool.<br/>
[**Schritt 4.**](#step-4-create-batch-job) Erstellen eines Batch **Job**.<br/>
[**Schritt 5.**](#step-5-add-tasks-to-job) Das Projekt **Vorgänge** hinzufügen.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Die Aufgaben sollen Knoten ausführen.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5 b.** Jede Aufgabe seiner Eingabedaten aus dem Azure-Speicher herunter und startet die Ausführung.<br/>
[**Schritt 6.**](#step-6-monitor-tasks) Überwachen Sie Aufgaben.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Aufgaben abgeschlossen sind, laden sie die Ausgabedaten in Azure-Speicher.<br/>
[**Schritt 7.**](#step-7-download-task-output) Taskausgabe vom Speicher herunterladen

Wie erwähnt, nicht jeder Batch-Lösung diese Schritte führt und kann veranschaulicht viele weitere, aber dieses Beispiel gemeinsame Prozesse in einer Batch-Lösung.

## <a name="prepare-client-script"></a>Vorbereiten von Clientskript

Fügen Sie *python_tutorial_client.py*vor dem Ausführen des Beispiels die Kontoanmeldeinformationen Stapel und Speicher hinzu. Wenn dies noch nicht getan haben, öffnen Sie die Datei in Ihrem bevorzugten Editor, und aktualisieren Sie die folgenden Zeilen mit Ihren Anmeldeinformationen.

```python
# Update the Batch and Storage account credential strings below with the values
# unique to your accounts. These are used when constructing connection strings
# for the Batch and Storage client objects.

# Batch account credentials
batch_account_name = "";
batch_account_key  = "";
batch_account_url  = "";

# Storage account credentials
storage_account_name = "";
storage_account_key  = "";
```

Finden Sie Ihre Anmeldeinformationen Stapel und Speicher in das Konto Blade jedes Dienstes in [Azure-Portal][azure_portal]:

![Batch-Anmeldeinformationen im Portal][9]
![Speicher Anmeldeinformationen im Portal][10]<br/>

In den folgenden Abschnitten analysieren wir die Schritte eine Arbeitslast im Batch-Dienst verarbeitet die Skripts verwendet. Wir empfehlen Ihnen, regelmäßig finden die Skripten im Editor während der Arbeit Ihren Weg durch den Rest des Artikels.

Wechseln Sie in die folgende Zeile in **python_tutorial_client.py** mit Schritt 1 beginnen:

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>Schritt 1: Speicher-Container erstellen

![Container in Azure-Speicher erstellen][1]
<br/>

Stapel enthält integrierten Unterstützung für die Interaktion mit Azure-Speicher. Container in das Speicherkonto stellen in Ihrem Konto Stapel ausgeführte Aufgaben erforderlichen Dateien. Der Container stellen auch die Ausgabedaten speichern, die die Aufgaben erstellen. Zunächst das *python_tutorial_client.py* Skript ist drei in [Azure BLOB-Speicher](../storage/storage-introduction.md#blob-storage)erstellen:

- **Anwendung**: Container speichert das Python-Skript ausführen, indem Sie die Aufgaben *python_tutorial_task.py*.
- **Eingabe**: Aufgaben downloadet Dateien aus der *Eingabe* verarbeitet.
- **Ausgabe**: Aufgaben Eingabedatei verarbeiten sie lädt die Ergebnisse dem *Ausgabe* -Container.

Um ein Speicherkonto interagieren und Container erstellen, verwenden wir den [Azure-Speicher] [ pypi_storage] Paket zu [BlockBlobService] [ py_blockblobservice] Objekt "BLOB-Client". Wir erstellen Sie drei Container im Speicherkonto BLOB-Client verwenden.

```python
 # Create the blob client, for use in obtaining references to
 # blob storage containers and uploading files to containers.
 blob_client = azureblob.BlockBlobService(
     account_name=_STORAGE_ACCOUNT_NAME,
     account_key=_STORAGE_ACCOUNT_KEY)

 # Use the blob client to create the containers in Azure Storage if they
 # don't yet exist.
 app_container_name = 'application'
 input_container_name = 'input'
 output_container_name = 'output'
 blob_client.create_container(app_container_name, fail_on_exist=False)
 blob_client.create_container(input_container_name, fail_on_exist=False)
 blob_client.create_container(output_container_name, fail_on_exist=False)
```

Nach die Containern erstellt wurden, kann die Anwendung jetzt Dateien hochladen, die von Aufgaben verwendet werden.

> [AZURE.TIP] [Wie Azure BLOB-Speicher aus Python](../storage/storage-python-how-to-use-blob-storage.md) bietet eine gute Übersicht über Azure Storage Container mit Blobs arbeiten. Es sollten oben Leseliste mit Batch arbeiten.

## <a name="step-2-upload-task-script-and-data-files"></a>Schritt 2: Hochladen Sie Vorgang Skripts und Dateien

![Upload Aufgabe Anwendung und Container den Eingabedateien (Daten)][2]
<br/>

In den Dateiupload definiert *python_tutorial_client.py* zuerst Sammlungen von **Anwendung** und **input** Dateipfade auf dem lokalen Computer vorhanden. Dann lädt es diese Dateien auf die Container, die Sie im vorherigen Schritt erstellt haben.

```python
 # Paths to the task script. This script will be executed by the tasks that
 # run on the compute nodes.
 application_file_paths = [os.path.realpath('python_tutorial_task.py')]

 # The collection of data files that are to be processed by the tasks.
 input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                     os.path.realpath('./data/taskdata2.txt'),
                     os.path.realpath('./data/taskdata3.txt')]

 # Upload the application script to Azure Storage. This is the script that
 # will process the data files, and is executed by each of the tasks on the
 # compute nodes.
 application_files = [
     upload_file_to_container(blob_client, app_container_name, file_path)
     for file_path in application_file_paths]

 # Upload the data files. This is the data that will be processed by each of
 # the tasks executed on the compute nodes in the pool.
 input_files = [
     upload_file_to_container(blob_client, input_container_name, file_path)
     for file_path in input_file_paths]
```

Verständnis der Liste mit den `upload_file_to_container` Funktion für jede Datei die Sammlungen und zwei [ResourceFile] [ py_resource_file] -Auflistung aufgefüllt. Die `upload_file_to_container` Funktion wird unten angezeigt:

```
def upload_file_to_container(block_blob_client, container_name, file_path):
    """
    Uploads a local file to an Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: The name of the Azure Blob storage container.
    :param str file_path: The local path to the file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """
    blob_name = os.path.basename(file_path)

    print('Uploading file {} to container [{}]...'.format(file_path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a>ResourceFiles

[ResourceFile] [ py_resource_file] enthält Aufgaben im Stapel mit URL zu einer Datei in Azure-Speicher, der einem Compute-Knoten vor diesem Task ausgeführt wird. [ResourceFile][py_resource_file]. **Blob_source** -Eigenschaft gibt die vollständige URL der Datei im Azure-Speicher vorhanden. Die URL kann auch SAS (SAS) enthalten den sicheren Zugriff auf die Datei. Die meisten Vorgangsarten Batch enthalten eine Eigenschaft *ResourceFiles* einschließlich:

- [CloudTask][py_task]
- [StartTask][py_starttask]
- [JobPreparationTask][py_jobpreptask]
- [JobReleaseTask][py_jobreltask]

In diesem Beispiel die JobPreparationTask oder JobReleaseTask Vorgangsarten, jedoch nicht mehr über sie [Ausführen Vorbereitung und Durchführung Aufgaben in Azure Batch compute-Knoten](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>SAS (SAS)

SAS sind Zeichenfolgen, die sicheren auf Container und Blobs in Azure-Speicher Zugriff. *Python_tutorial_client.py* Skript verwendet sowohl Blob und Container shared Access Signaturen und veranschaulicht, wie diese gemeinsamen Zugriff Signaturzeichenfolgen von Storage Service erhalten.

- **BLOB shared Access Signaturen**: den Pool StartTask verwendet blob SAS beim Herunterladen der Aufgabendateien Skript und Eingabe Daten aus dem Speicher (siehe [Schritt 3](#step-3-create-batch-pool) ). Die `upload_file_to_container` *python_tutorial_client.py* Funktion enthält Code, der jedes Blob SAS erhält. Dies geschieht durch Aufrufen von [BlockBlobService.make_blob_url] [ py_make_blob_url] in das Speichermodul.

- **Container-SAS**: er seine Ausgabedatei wie jede Aufgabe seine Arbeit auf Compute-Knoten beendet, *Ausgabe* Container in Azure-Speicher hochgeladen. Hierzu verwendet *python_tutorial_task.py* eine Container-SAS, die Schreibzugriff auf den Container bereitstellt. Die `get_container_sas_token` Funktion in *python_tutorial_client.py* erhält der Container SAS, die dann als Befehlszeilenargument an Aufgaben übergeben. Schritt #5 [Aufgaben zu einem Auftrag hinzufügen](#step-5-add-tasks-to-job), erläutert die Verwendung des Containers SAS.

> [AZURE.TIP] Auschecken der zweiteiligen Reihe SAS, [Teil 1: Grundlegendes zu SAS-Modell](../storage/storage-dotnet-shared-access-signature-part-1.md) und [Teil2: Erstellen und Verwenden von SAS mit BLOB-Dienst](../storage/storage-dotnet-shared-access-signature-part-2.md), sicheren Zugriff auf Daten in das Speicherkonto erfahren.

## <a name="step-3-create-batch-pool"></a>Schritt 3: Batch-Pool erstellen

![Erstellen eines Batch-Pools][3]
<br/>

Batch- **Pool** ist eine Sammlung von Compute-Knoten (virtuelle Maschinen) auf denen Stapel eine Aufgaben ausführt.

Nachdem er das Speicherkonto Aufgabe Skripts und Dateien hochgeladen, startet *python_tutorial_client.py* seiner Interaktion mit der Stapelverarbeitung mit dem Batch Python-Modul. Dazu [BatchServiceClient] [ py_batchserviceclient] erstellt:

```python
 # Create a Batch service client. We'll now be interacting with the Batch
 # service in addition to Storage.
 credentials = batchauth.SharedKeyCredentials(_BATCH_ACCOUNT_NAME,
                                              _BATCH_ACCOUNT_KEY)

 batch_client = batch.BatchServiceClient(
     credentials,
     base_url=_BATCH_ACCOUNT_URL)
```

Anschließend wird ein Pool von Compute-Knoten in Batch-Konto mit einem Aufruf erstellt `create_pool`.

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with the specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for the new pool.
    :param list resource_files: A collection of resource files for the pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify the commands for the pool's start task. The start task is run
    # on each node as it joins the pool, and when it's rebooted or re-imaged.
    # We use the start task to prep the node for running our task script.
    task_commands = [
        # Copy the python_tutorial_task.py script to the "shared" directory
        # that all tasks that run on the node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and the dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install the azure-storage module so that the task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get the node agent SKU and image reference for the virtual machine
    # configuration.
    # For more information about the virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Beim Erstellen eines Pools definieren ein [PoolAddParameter] [ py_pooladdparam] mehrere Eigenschaften für den Pool angibt:

- **ID** des Pools (*Id* - erforderlich)<p/>Wie bei den meisten Entitäten im Stapel müssen das neue Pool eine eindeutige ID in Ihrem Konto Batch. Code bezieht sich auf diesen Pool mithilfe der ID und ist wie den Pool im Azure- [Portal]erkennen[azure_portal].

- **Anzahl von Compute-Knoten** (*Target_dedicated* - erforderlich)<p/>Diese Eigenschaft gibt an, wie viele virtuelle Computer im Pool bereitgestellt werden soll. Es ist wichtig zu beachten, dass alle Stapel Konten ein **Kontingent** , das die Anzahl der **Kerne** (und folglich Compute-Knoten) in einem Batch-Konto beschränkt. Standardkontingente und Informationen (z. B. die maximale Anzahl der Kerne in Ihrem Konto Batch) [eine Quote](batch-quota-limit.md#increase-a-quota) [Quoten](batch-quota-limit.md)und Grenzwerte für Azure Batch-Dienst gefunden werden. Wenn Sie sich Fragen "Warum wird nicht Pool erreichen mehr als X Knoten?" Dieses Kontingent Core könnte die Ursache sein.

- **Betriebssystem** für Knoten (*Virtual_machine_configuration* **oder** *Cloud_service_configuration* - erforderlich)<p/>Im *python_tutorial_client.py*, erstellen wir einen Pool von Linux-Nodes mit [VirtualMachineConfiguration][py_vm_config]. Die `select_latest_verified_vm_image_with_node_agent_sku` in Funktion `common.helpers` vereinfacht das Arbeiten mit [Azure VMs Marketplace] [ vm_marketplace] Bilder. Weitere Informationen zum Marketplace Bilder finden Sie unter [Bereitstellung Linux Knoten in Azure Batch Pools berechnet](batch-linux-nodes.md) .

- **Größe von Compute-Knoten** (*Vm_size* - erforderlich)<p/>Da wir für unsere [VirtualMachineConfiguration]Linux-Knoten angeben[py_vm_config], eine VM-Größe angegeben (`STANDARD_A1` in diesem Beispiel) [für virtuelle Computer in Azure](../virtual-machines/virtual-machines-linux-sizes.md)aus. Wieder finden Sie unter [Bereitstellung Linux Computeknoten in Azure Batch-Pools](batch-linux-nodes.md) für Weitere Informationen.

- **Aufgabe starten** (*Start_task* - nicht erforderlich)<p/>Mit den über physischen Knoteneigenschaften kann auch ein [StartTask] angegeben[ py_starttask] für den Pool (es ist nicht erforderlich). Der StartTask führt auf jedem Knoten dieses Knotens verknüpft Pool und jedem Knoten neu gestartet. Der StartTask eignet sich besonders zum Vorbereiten von Compute-Knoten für die Ausführung von Aufgaben wie das Installieren von Applikationen, die Ihre Aufgaben ausgeführt werden.<p/>In dieser beispielanwendung kopiert die StartTask Dateien, die es downloads aus dem Speicher (die mit der StartTask **Resource_files** -Eigenschaft angegeben sind) *Arbeitsverzeichnis* StartTask auf das *freigegebene* Verzeichnis, das alle auf dem Knoten zugreifen können. Im Wesentlichen kopiert `python_tutorial_task.py` auf das freigegebene Verzeichnis auf jedem Knoten wie der Knoten den Pool verknüpft, damit alle Tasks ausgeführt, die auf dem Knoten darauf zugreifen können.

Fällt den Aufruf der `wrap_commands_in_shell` -Hilfsfunktion. Diese Funktion nimmt eine Auflistung von Befehlen und erstellt eine Befehlszeile für eine Aufgabe Befehlszeileneigenschaft.

Bemerkenswert im Codeausschnitt ist auch die Verwendung von zwei Umgebungsvariablen in der **Befehlszeile** -Eigenschaft die StartTask: `AZ_BATCH_TASK_WORKING_DIR` und `AZ_BATCH_NODE_SHARED_DIR`. Jeder Compute-Knoten in einem Batch-Pool wird automatisch mit mehreren Variablen, die Stapel sind konfiguriert. Alle Prozesse, die durch eine Aufgabe ausgeführt hat Zugriff auf Umgebungsvariablen.

> [AZURE.TIP] Erfahren Sie mehr über die Umgebungsvariablen, die Computeknoten in eine Stapelverarbeitung Pool sowie Informationen zur Aufgabe Arbeitsverzeichnisse verfügbar sind, finden Sie in der [Übersicht über Azure Batch Funktionen](batch-api-basics.md) **umgebungseinstellungen für Aufgaben** und **Dateien und Verzeichnisse** .

## <a name="step-4-create-batch-job"></a>Schritt 4: Stapelverarbeitungsauftrag erstellen

![Stapelverarbeitungsauftrag erstellen][4]<br/>

Ein **Auftrag** ist eine Sammlung von Aufgaben und einen Pool von Compute-Knoten zugeordnet ist. Die Aufgaben eines Projekts ausführen auf Compute-Knoten zugeordneten Pool.

Können Sie ein Projekt organisieren und Nachverfolgen von Aufgaben verwandte Arbeitslasten, sondern auch für bestimmte Auflagen – wie die maximale Laufzeit für den Auftrag (und seine Aufgaben) und Priorität andere Batch-Konto. In diesem Beispiel ist die Aufgabe jedoch nur in Schritt #3 erstellte Pool zugeordnet. Es sind keine zusätzlichen Eigenschaften konfiguriert.

Alle Stapelverarbeitungsaufträge werden einem bestimmten Pool zugeordnet. Diese Zuordnung gibt an, welche Knoten auf die Aufgaben ausführen. Angeben den Pool mit [PoolInformation] [ py_poolinfo] -Eigenschaft, wie im folgenden Codeausschnitt gezeigt.

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with the specified ID, associated with the specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID for the job.
    :param str pool_id: The ID for the pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Ein Auftrag erstellt, werden Aufgaben verrichten hinzugefügt.

## <a name="step-5-add-tasks-to-job"></a>Schritt 5: Hinzufügen von Aufgaben zum Projekt

![Aufgaben zum Auftrag hinzufügen][5]<br/>
*(1) Aufgaben werden dem Projekt hinzugefügt und (2) die Aufgaben auf Knoten ausführen soll (3) die Aufgaben die Datendateien zu downloaden*

Batchverarbeitung von **Aufgaben** werden die einzelnen Arbeitseinheiten, die Compute-Knoten ausgeführt. Eine Aufgabe ist eine Befehlszeile und führt die Skripts oder ausführbare Dateien, die Sie in die Befehlszeile angeben.

Um tatsächlich Aufgaben müssen ein Auftrag Aufgaben hinzugefügt werden. Jede [CloudTask] [ py_task] mit Befehlszeileneigenschaft [ResourceFiles] konfiguriert[ py_resource_file] (wie dem Pool StartTask), die die Aufgabe downloadet zum Knoten vor die Befehlszeile automatisch ausgeführt wird. Im Beispiel verarbeitet jeden Vorgang nur eine Datei. So enthält die ResourceFiles-Auflistung ein einzelnes Element.

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in the collection to the specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID of the job to which to add the tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: The ID of an Azure Blob storage container to
    which the tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    the specified Azure Blob storage container.
    """

    print('Adding {} tasks to job [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [AZURE.IMPORTANT] Wenn sie Zugriff auf Umgebungsvariablen wie `$AZ_BATCH_NODE_SHARED_DIR` oder eine Anwendung in des Knotens nicht gefunden `PATH`, Befehlszeilen müssen die Shell aufrufen, z. B. mit `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. Dies ist unnötig, wenn Ihre Aufgaben einer Anwendung in des Knotens ausführen `PATH` und Umgebungsvariablen nicht verweisen.

In der `for` Schleife im Codeausschnitt, Sie sehen, dass die Befehlszeile für den Task mit fünf Befehlszeilenargumente erstellt wird, die an *python_tutorial_task.py*übergeben werden:

1. **FilePath**: Dies ist der lokale Pfad zu der Datei auf den Knoten. Wenn die ResourceFile Objekt `upload_file_to_container` wurde in Schritt2 oben der Name für diese Eigenschaft verwendet wurde (die `file_path` Parameter im Konstruktor ResourceFile). Dies bedeutet, dass die Datei in demselben Verzeichnis auf dem Knoten als *python_tutorial_task.py*finden.

2. **NumWords**: Top *N* Wörter in die Ausgabedatei geschrieben werden soll.

3. **StorageAccount**: der Name des Speicherkontos, der den Container besitzt, die Aufgabe Ausgabe hochgeladen werden soll.

4. **Storagecontainer**: der Name der Behälter, die Ausgabedateien hochgeladen werden soll.

5. **Sastoken**: SAS (SAS), die Schreibzugriff auf den Container **Ausgabe** in Azure Storage bietet. Das *python_tutorial_task.py* -Skript verwendet diese SAS bei seinen BlockBlobService Verweis erstellt. Dies bietet Schreibzugriff auf den Container, ohne eine Zugriffstaste für das Speicherkonto.

```python
# NOTE: Taken from python_tutorial_task.py

# Create the blob client using the container's SAS token.
# This allows us to create a client that provides write
# access only to the container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>Schritt 6: Überwachen von Aufgaben

![Überwachen von Aufgaben][6]<br/>
*Skript (1) überwacht die Aufgaben Abschlussstatus und (2) die Aufgaben Daten in Azure-Speicher hochladen*

Wenn Aufgaben mit einem Projekt hinzugefügt werden, automatisch in die Warteschlange gestellt und auf Compute-Knoten mit dem Auftrag assoziierten Pool für die Ausführung geplant. Basierend auf der angegebenen behandelt Batch alle Warteschlangen Tasks planen, Wiederholung und Abgaben Verwaltung Aufgabe für Sie.

Es gibt viele Ansätze für die Überwachung der Ausführung der Aufgabe. Die `wait_for_tasks_to_complete` in *python_tutorial_client.py* -Funktion bietet ein einfaches Beispiel Überwachung Aufgaben für einen bestimmten Zustand in diesem Fall [abgeschlossen] [ py_taskstate] Zustand.

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in the specified job reach the Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The id of the job whose tasks should be to monitored.
    :param timedelta timeout: The duration to wait for task completion. If all
    tasks in the specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a>Schritt 7: Aufgabe Ausgabe herunterladen

![Speicher Aufgabe Ausgabe herunterladen][7]<br/>

Jetzt, dass der Auftrag abgeschlossen ist, kann die Ausgabe von Aufgaben Azure-Speicher heruntergeladen. Dies erfolgt mit einem Aufruf von `download_blobs_from_container` in *python_tutorial_client.py*:

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from the specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: The Azure Blob storage container from which to
     download files.
    :param directory_path: The local directory to which to download the files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] to {}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [AZURE.NOTE] Der Aufruf von `download_blobs_from_container` in *python_tutorial_client.py* gibt an, dass die Dateien in Ihrem Basisverzeichnis gedownloadet werden sollen. Diese Ausgabespeicherort ändern können.

## <a name="step-8-delete-containers"></a>Schritt 8: Delete Container

Da Sie für Daten, die in Azure-Speicher befindet berechnet, empfiehlt es immer eine Blobs zu entfernen, die für die Stapelverarbeitungsaufträge nicht mehr benötigt werden. In *python_tutorial_client.py*dazu drei Aufrufe von [BlockBlobService.delete_container][py_delete_container]:

```
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Schritt 9: Löschen Sie den Auftrag und pool

Im letzten Schritt werden Sie aufgefordert, den Auftrag und *python_tutorial_client.py* Skript erstellte Pool löschen. Obwohl Sie nicht für Projekte und Aufgaben, die *Sie* berechnet Compute-Knoten berechnet. Daher wird empfohlen, nur nach Bedarf Knoten zuordnen. Löschen nicht verwendeter Pools kann Teil der Wartung.

Die BatchServiceClient [JobOperations] [ py_job] und [PoolOperations] [ py_pool] beide verfügen über entsprechende Löschmethoden aufgerufen werden, wenn Sie den Löschvorgang bestätigen:

```python
# Clean up Batch resources (if the user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [AZURE.IMPORTANT] Denken Sie daran, die Sie Zahlen für computeressourcen löschen nicht verwendeter Pools Kosten minimieren. Beachten Sie auch, dass einen Pool Löschen aller Computeknoten in diesem Pool und werden alle Daten auf dem Knoten nicht behebbarer gelöschten Pool.

## <a name="run-the-sample-script"></a>Führen Sie das Beispielskript

Beim Ausführen des *python_tutorial_client.py* -Skripts aus dem Lernprogramm [Beispielcode][github_article_samples], die Konsolenausgabe ähnelt der folgenden. Eine Pause am `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` des Pools Compute-Knoten erstellt, gestartet, und die Befehle in den Pool Start-Aufgabe ausgeführt. Verwenden der [Azure-Portal] [ azure_portal] zum Überwachen des Pools zu berechnen Knoten, Auftrag und Aufgaben während und nach der Ausführung. Verwenden der [Azure-Portal] [ azure_portal] oder [Microsoft Azure Storage Explorer] [ storage_explorer] die Speicherressourcen (Container und Blobs) anzeigen, die von der Anwendung erstellt werden.

>[AZURE.TIP] Führen Sie das Skript *python_tutorial_client.py* aus der `azure-batch-samples/Python/Batch/article_samples` Verzeichnis. Verwendet einen relativen Pfad für die `common.helpers` Modul importieren, so dass Sie sehen können `ImportError: No module named 'common'` läuft nicht das Skript in diesem Verzeichnis.

Normale dauert Ausführung **ca. 5 bis 7 Minuten** beim Ausführen des Beispiels in der Standardkonfiguration.

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py to container [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt to container [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks to job [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached the 'Completed' state within the specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] to /home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] to /home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] to /home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER to exit...
```

## <a name="next-steps"></a>Nächste Schritte

*Python_tutorial_client.py* und *python_tutorial_task.py* mit unterschiedlichen berechnen Szenarien experimentieren ändern können. Versuchen Sie beispielsweise *python_tutorial_task.py* im Portal überwachen und zeitintensiven Aufgaben simulieren eine Ausführung Verzögerung hinzuzufügen. Versuchen Sie, weitere Aufgaben hinzufügen oder die Anzahl von Compute-Knoten. Fügen Sie Logik zum Überprüfen und ermöglichen das Verwendung eines vorhandenen Geschwindigkeit Ausführungszeit hinzu.

Damit Sie grundlegenden Arbeitsablauf von Batch-Lösung kennen, ist es Zeit, auf zusätzliche Features von Batch-Dienst zu erhalten.

- Lesen Sie den Artikel [Übersicht Azure Stapelverarbeitungsfunktionen](batch-api-basics.md) empfehlenswert, wenn Sie den Dienst haben.
- Starten auf anderen Stapel Entwicklung Artikel **Entwicklung detaillierte** [Batch Lernpfad][batch_learning_path].
- Auschecken einer anderen Implementierung verarbeitet "Top N Wörter" Arbeitslast mit in [TopNWords] [ github_topnwords] Beispiel.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage

[pypi_install]: http://python-packaging-user-guide.readthedocs.io/en/latest/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Container in Azure-Speicher erstellen"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Upload Aufgabe Anwendung und Container den Eingabedateien (Daten)"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Batch-Pool erstellen"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Stapelverarbeitungsauftrag erstellen"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Aufgaben zum Auftrag hinzufügen"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Überwachen von Aufgaben"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Speicher Aufgabe Ausgabe herunterladen"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Batch-Lösung Workflow (vollständiges Diagramm)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Batch-Anmeldeinformationen im Portal"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Speicherung von Anmeldeinformationen im Portal"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Batch-Lösung Workflow (minimale Diagramm)"
