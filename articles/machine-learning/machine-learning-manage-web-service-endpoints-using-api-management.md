<properties
    pageTitle="AzureML WebServices API Management mit verwalten | Microsoft Azure"
    description="Ein Leitfaden zum AzureML WebServices API Management mit verwalten."
    keywords="Computer lernen, API-management"
    services="machine-learning"
    documentationCenter=""
    authors="roalexan"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="roalexan" />


# <a name="learn-how-to-manage-azureml-web-services-using-api-management"></a>Verwalten Sie AzureML WebServices API Management mit

##<a name="overview"></a>Übersicht

Dieses Handbuch veranschaulicht den schnellen Einstieg API Management Ihre Webdienste AzureML verwalten.

##<a name="what-is-azure-api-management"></a>Was ist Azure API Management?

Azure API Management ist ein Azure, die REST API Endpunkte definieren Benutzerzugriff, Verwendung Einschränkung und Überwachung Dashboard verwalten können. Klicken Sie [hier](https://azure.microsoft.com/services/api-management/) für Informationen zur Azure API Management. Klicken Sie [hier](../api-management/api-management-get-started.md) für eine Anleitung zum Einstieg in Azure API Management. Andere dabei, dem dieses Handbuchs basiert, umfasst weitere Themen Benachrichtigung Konfigurationen Tier Preise Antwort Behandlung, Benutzerauthentifizierung Produkte, Entwickler Abonnements und Verwendung Dashboarding erstellen.

##<a name="what-is-azureml"></a>Was ist AzureML?

AzureML ist ein Azure für maschinelles lernen, mit der Sie auf einfache Weise erstellen, bereitstellen und modernste Analytik Lösungen. Klicken Sie [hier](https://azure.microsoft.com/services/machine-learning/) für Informationen über AzureML.

##<a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Handbuch abzuschließen, müssen wie folgt vor:

* Ein Azure-Konto. Wenn ein Azure-Konto haben, klicken Sie [hier](https://azure.microsoft.com/pricing/free-trial/) für Informationen über ein kostenloses Testabo erstellen.
* Ein AzureML-Konto. Wenn Sie ein AzureML-Konto haben, klicken Sie auf [hier](https://studio.azureml.net/) ausführliche Informationen über ein kostenloses Testabo erstellen.
* Arbeitsbereich, Service und Api_key für ein AzureML Experiment als Webdienst bereitgestellt. Klicken Sie [hier](machine-learning-create-experiment.md) für Informationen zum Versuch AzureML erstellen. Klicken Sie [hier](machine-learning-publish-a-machine-learning-web-service.md) für Informationen zum Versuch AzureML als Web Service bereitstellen. Anhang A hat auch Hinweise zum Erstellen und Testen einer einfachen AzureML experimentieren und als Web Service bereitstellen.

##<a name="create-an-api-management-instance"></a>Eine API Management-Instanz erstellen

Im folgenden sind die Schritte für die Verwendung von API-Management zu den AzureML-Webdienst. Erstellen Sie zunächst eine Instanz. Melden Sie sich im [Verwaltungsportal](https://manage.windowsazure.com/) und klicken Sie auf **neu** > **App Services** > **API Management** > **Erstellen**.

![Instanz erstellen](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

Geben Sie eine eindeutige **URL**. Verwendet **Demoazureml** -Sie müssen etwas anderes auswählen. Wählen Sie das gewünschte **Abonnement** und die **Region** für die Dienstinstanz. Klicken Sie nach der Auswahl auf die Schaltfläche Weiter.

![Erstellen-Service-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

Geben Sie einen Wert für den **Organisationsnamen**. Verwendet **Demoazureml** -Sie müssen etwas anderes auswählen. Geben Sie Ihre e-Mail-Adresse im Feld **e-Mail-Administrator** . Diese e-Mail-Adresse wird für Benachrichtigungen aus der System-API verwendet.

![Erstellen-Service-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

Klicken Sie auf das Kontrollkästchen, um die Dienstinstanz erstellen. *Bis zu 30 Minuten einen neuen Dienst erstellt werden*.

##<a name="create-the-api"></a>Die API erstellen

Nachdem die Instanz erstellt wurde, besteht der nächste Schritt die API erstellen. Eine API besteht aus einer Reihe von Operationen, die von einer Clientanwendung aufgerufen werden kann. API-Vorgänge sind als vorhandene Webdienste. Dieses Handbuch erstellt APIs, Proxy vorhandene AzureML RRS und BES Webdienste.

APIs erstellt und Azure-Verwaltungsportal zugegriffen API-Portal Verleger konfiguriert. Wählen Sie die Dienstinstanz zu Publisher-Portal.

![Auswählen der Dienstinstanz](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

Klicken Sie auf **Manage** in Azure-Verwaltungsportal für Ihre API-Verwaltungsdienst.

![Management-service](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

Klicken Sie auf **APIs** der **API-** Menü auf der linken Seite und dann auf **API hinzufügen**.

![API-Management-Menü](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

Geben Sie **AzureML Demo-API** als **Web API-Namen**. Geben Sie **https://ussouthcentral.services.azureml.net** als **Webdienst-URL**. Geben Sie **Azureml-Demo** **Web API URL-Suffix**. Aktivieren Sie **HTTPS** als **Web API-URL** -Schema. **Starter** **Produkte**auswählen Klicken Sie abschließend auf **Speichern** , um die API erstellen.

![Hinzufügen-neue-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

##<a name="add-the-operations"></a>Fügen Sie Vorgänge hinzu

Klicken Sie auf **Hinzufügen** , um API-Vorgänge hinzufügen.

![Vorgang hinzufügen](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

Der **neue Vorgang** angezeigt, und die Registerkarte **Signatur** standardmäßig aktiviert.

##<a name="add-rrs-operation"></a>Ressourceneinträge hinzufügen

Erstellen Sie zunächst einen Vorgang für den Dienst AzureML RRS. Wählen Sie das **http-Verb** **POST** . Typ **/workspaces/ {Arbeitsbereich} /services/ {Service} / Execute?api Version = {Anforderungsparameter} & Details = {Details}** **URL-Vorlage**. Geben Sie als **Anzeigename** **Ressourceneinträge ausführen** .

![Ressourceneinträge-Operation-Signatur hinzufügen](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Klicken Sie auf **Antworten** > auf der linken Seite und wählen**Hinzufügen** **200 OK**. Klicken Sie auf **Speichern** , um diesen Vorgang zu speichern.

![Ressourceneinträge-Operation-Antwort hinzufügen](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

##<a name="add-bes-operations"></a>BES Vorgänge hinzufügen

Screenshots sind nicht für Bus-Vorgänge sind sehr ähnlich den Ressourceneinträge Vorgang hinzufügen.

###<a name="submit-but-not-start-a-batch-execution-job"></a>Batchausführung Auftrag senden (jedoch nicht gestartet)

Klicken Sie auf die API AzureML BES Vorgang hinzufügen **Hinzufügen** . Wählen Sie für das **http-Verb** **POST** . Typ **/workspaces/ {Arbeitsbereich} /services/ {Service} / Jobs?api Version = {Anforderungsparameter}** für die **URL-Vorlage**. Geben Sie **Anzeigename** **BES senden** . Klicken Sie auf **Antworten** > auf der linken Seite und wählen**Hinzufügen** **200 OK**. Klicken Sie auf **Speichern** , um diesen Vorgang zu speichern.

###<a name="start-a-batch-execution-job"></a>Die Batchausführung Auftrag starten

Klicken Sie auf die API AzureML BES Vorgang hinzufügen **Hinzufügen** . Wählen Sie für das **http-Verb** **POST** . Typ **/workspaces/ {Arbeitsbereich} /services/ {Service} /jobs/ {Jobid} / Start?api Version = {Anforderungsparameter}** für die **URL-Vorlage**. Geben Sie **Anzeigename** **BES starten** . Klicken Sie auf **Antworten** > auf der linken Seite und wählen**Hinzufügen** **200 OK**. Klicken Sie auf **Speichern** , um diesen Vorgang zu speichern.

###<a name="get-the-status-or-result-of-a-batch-execution-job"></a>Erhalten Sie den Status oder das Ergebnis eines Auftrags Batchausführung

Klicken Sie auf die API AzureML BES Vorgang hinzufügen **Hinzufügen** . Wählen Sie für das **http-Verb** **GET** . Typ **/workspaces/ {Arbeitsbereich} /services/ {Service} /jobs/ {Jobid} ?api Version = {Anforderungsparameter}** für die **URL-Vorlage**. Geben Sie **BES Status** **Anzeigenamen**. Klicken Sie auf **Antworten** > auf der linken Seite und wählen**Hinzufügen** **200 OK**. Klicken Sie auf **Speichern** , um diesen Vorgang zu speichern.

###<a name="delete-a-batch-execution-job"></a>Löschen eines Auftrags Batchausführung

Klicken Sie auf die API AzureML BES Vorgang hinzufügen **Hinzufügen** . Wählen Sie für das **http-Verb** **Löschen** . Typ **/workspaces/ {Arbeitsbereich} /services/ {Service} /jobs/ {Jobid} ?api Version = {Anforderungsparameter}** für die **URL-Vorlage**. Geben Sie den **Anzeigenamen** **BES löschen** . Klicken Sie auf **Antworten** > auf der linken Seite und wählen**Hinzufügen** **200 OK**. Klicken Sie auf **Speichern** , um diesen Vorgang zu speichern.

##<a name="call-an-operation-from-the-developer-portal"></a>Rufen Sie eine Operation Entwicklerportal

Vorgänge können direkt aus dem Entwicklerportal bietet eine bequeme Möglichkeit zu testen die Vorgänge einer API aufgerufen werden. In diesem Handbuch Schritt rufen Sie die Methode **Execute Ressourceneinträge** , die **AzureML Demo-API**hinzugefügt wurde. Klicken Sie im Menü oben **Entwicklerportal** neben dem Verwaltungsportal.

![Entwickler-portal](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

Klicken Sie im oberen Menü auf **APIs** , und dann auf **AzureML Demo-API** verfügbaren Vorgänge angezeigt.

![Demoazureml-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

Wählen Sie für den Vorgang **RRS ausführen** . Klicken Sie auf **ausprobieren**.

![Try it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

Anforderungsparameter Geben Sie Ihr **Arbeitsbereich** **Service**, **2.0** für **Anforderungsparameter**und **true** für **Details**. **Arbeitsbereich** und **Service** finden Sie im AzureML Web Service-Dashboard (siehe Anhang A **Test den Webdienst** ).

Anforderungsheader, klicken Sie auf **Add Header** **Content-Type** und **Application/Json**und klicken Sie auf **Hinzufügen** und geben geben **Autorisierung** und **Träger <YOUR AZUREML SERVICE API-KEY> **. Finden Sie den **API-Schlüssel** in AzureML Web Service-Dashboard (siehe Anhang A **Test den Webdienst** ).

Typ **{"Inputs": {"input1": {"ColumnNames": ["SP2"], "Werte": [["Dies ist ein guter Tag"]]}}, "GlobalParameters": {}}** für den Hauptteil der Anforderung.

![Azureml-Demo-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Klicken Sie auf **Senden**.

![Senden](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

Nachdem ein Vorgang aufgerufen wird, zeigt Entwicklerportal **Angeforderte URL** vom Back-End-Dienst, der **Antwortstatus**der **Antwortheader**und **Antwortinhalt**.

![Antwort-status](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

##<a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Anhang A - Webdienst erstellen und Testen einer einfachen AzureML

###<a name="creating-the-experiment"></a>Erstellen des Experiments

Nachfolgend sind die Schritte zum Erstellen eines einfachen AzureML Experiments und als Web Service bereitstellen. Web Service akzeptiert als Eingabe eine Textspalte beliebigen und gibt einen Satz von Funktionen als ganze Zahlen dargestellt. Zum Beispiel:

Text | Hash Text
--- | ---
Guten Tag | 1 1 2 2 0 2 0 1

Zunächst einen Browser Ihrer Wahl wechseln Sie zu: [https://studio.azureml.net/](https://studio.azureml.net/) und geben Sie Ihre Anmeldeinformationen anmelden. Als Nächstes erstellen Sie ein neues leeres Experiment.

![Experiment Suchvorlagen](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Benennen Sie sie in **SimpleFeatureHashingExperiment**. Erweitern Sie **Datasets gespeichert** und das Experiment ziehen Sie **Buchbesprechungen von Amazon** .

![einfache Funktion hashing experiment](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Erweitern Sie **Data Transformation** und **Bearbeitung** und ziehen Sie das Experiment **Spalten im Dataset auswählen** . Verbinden **von Amazon Buchbesprechungen** **Spalten im Dataset**auswählen.

![Spalten auswählen](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

**Wählen Sie** Spalten im Dataset klicken Sie auf **Starten Spaltenauswahl** und **SP2**aktivieren. Klicken Sie auf die Markierung zu ändern.

![Spalten auswählen](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

Erweitern Sie **Textanalyse** und ziehen Sie **Feature Hashing** des Experiments. **Wählen Sie Spalten im Dataset** an schließen Sie **Feature Hashing an**

![Verbinden-Projekt-Spalten](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Geben Sie **3** für das **Hashing-Bitsize**. Dadurch 8 (23) Spalten.

![Hashing-bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

An diesem Punkt möchten Sie zum Testen des Experiments **Ausführen** klicken.

![Ausführen](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

###<a name="create-a-web-service"></a>Erstellen Sie einen Webdienst

Nun erstellen Sie einen Webdienst. Erweitern Sie **Webdienst** und das Experiment ziehen Sie **Eingabe** . **Eingabe** zum **Feature Hashing**anschließen Das Experiment auch ziehen Sie **Ausgabe** . **Ausgabe** an **Feature Hashing**anschließen

![Ausgabe-zu-Feature-hashing](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Klicken Sie auf **Webdienst veröffentlichen**.

![Veröffentlichen-Webdienst](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

Klicken Sie auf **Ja,** um das Experiment zu veröffentlichen.

![Ja veröffentlichen](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

###<a name="test-the-web-service"></a>Testen des Webdienstes

Ein AzureML-Webdienst besteht aus RSS (Anforderung-Antwort-Dienst) und Bus (Batch Execution)-Endpunkte. RSS ist für synchrone Ausführung. BES wird asynchroner Auftrag ausgeführt. Testen Sie den Webdienst mit dem Beispiel Python unten möglicherweise herunterladen und Installieren von Azure SDK für Python (siehe: [Python installieren](../python-how-to-install.md)).

Sie benötigen auch **Arbeitsbereich**, **Service-**und **Api_key** des Tests für die Aufnahmequelle unten. Arbeitsbereich und Service finden Sie auf **Anforderung-Antwort** oder **Batchausführung** für das Experiment im Web Service-Dashboard.

![Suchen-Workspace-und-service](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

Durch Klicken auf das Experiment im Web Service-Dashboard finden Sie **Api_key** .

![Suchen-api-Schlüssel](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

####<a name="test-rrs-endpoint"></a>Test-Ressourceneinträge Endpunkt

#####<a name="test-button"></a>Schaltfläche "testen"

So testen Sie Ressourceneinträge Endpunkt wird **Testen** auf dem Web Service-Dashboard.

![Testen](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

Geben Sie **einen guten Tag** für **SP2**. Klicken Sie auf das Häkchen.

![Daten eingeben](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

Sie sehen etwa

![Beispiel-Ausgabe](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

#####<a name="sample-code"></a>Beispielcode

So testen Sie die Ressourceneinträge werden vom Clientcode. Wenn auf **Anforderung/Antwort** auf dem Dashboard und scrollen Sie nach unten sehen Beispielcode Sie für C#, Python und R. Außerdem sehen Sie die Syntax der Ressourceneinträge Anforderung, einschließlich des URI, Header und Text.

Dieses Handbuch zeigt ein funktionierendes Python-Beispiel. Sie müssen mit **Workspace**, **Service**und **Api_key** des Tests ändern.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

####<a name="test-bes-endpoint"></a>Test BES Endpunkt
Klicken Sie auf **die Batchausführung** Dashboard und scrollen Sie nach unten. Sie sehen Beispielcode für C#, Python und R. Sie sehen auch die Syntax BES Anfragen zu senden eines Auftrags, Starten eines Einzelvorgangs, den Status oder die Ergebnisse eines Auftrags, Löschen eines Auftrags.

Dieses Handbuch zeigt ein funktionierendes Python-Beispiel. Sie müssen mit **Workspace**, **Service**und **Api_key** des Tests ändern. Außerdem müssen Sie **speicherkontonamen**, **Speicherschlüssel Konto**und **Speicher Containernamen**ändern. Schließlich müssen Sie den Speicherort der **Datei** und den Speicherort der **Ausgabedatei**ändern.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
