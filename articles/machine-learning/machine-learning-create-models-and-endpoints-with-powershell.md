<properties
pageTitle="Erstellen Sie mehrere Modelle aus einem Experiment | Microsoft Azure"
description="Mithilfe von PowerShell mehrere Computerlernen Modelle und Web-Endpunkte mit denselben Algorithmus, aber unterschiedlichen Datasets erstellen."
services="machine-learning"
documentationCenter=""
authors="hning86"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="garye;haining"/>

# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Erstellen Sie viele Computerlernen Modelle und Web-Endpunkte aus einem Experiment mit PowerShell

Hier ist ein häufiges Problem der Computer lernen: Sie möchten viele Modelle, die dieselbe Ausbildung Workflow haben und den gleichen Algorithmus verwenden, aber unterschiedlichen Datasets als Eingabe. Dieser Artikel beschreibt, wie bei Skalierung in Azure Machine Learning Studio mit nur einem Experiment.

Angenommen, besitzen einen globalen Fahrrad Vermietung Franchise-Unternehmen. Erstellen ein Regressionsmodells die Vermietung Nachfrage Verlaufsdaten vorhersagen möchten. 1.000 Standorten weltweit haben und haben ein Dataset für jede Position, die wichtige Merkmale wie Datum, Uhrzeit, Wetter und Verkehr, die für jeden Standort gesammelt.

Sie können Ihr Modell einmal eine zusammengeführte Version aller Datasets an allen Standorten trainieren. Aber da Standorte eine Umgebung verfügen, ein besserer Ansatz wäre die Regression Modell mit dem Dataset separat für jeden Standort. So kann jeder qualifizierten Modell berücksichtigen *anderen Größen, Lautstärke, Geografie, Bevölkerung, Fahrrad-freundliche Datenverkehr Umgebung usw.*.

Vielleicht am besten, aber nicht soll 1.000 Training Experimente in Azure Machine Learning jeweils einen eindeutigen Speicherort darstellt. Sehr aufwendig, es ist auch scheint ziemlich ineffizient, da für jeden Versuch alle Komponenten außer Trainings-Dataset haben.

Glücklicherweise können wir hierzu [Azure Machine Learning Umschulung API](machine-learning-retrain-models-programmatically.md) verwenden und mit [Azure Machine Learning PowerShell](machine-learning-powershell-module.md)automatisieren.

> [AZURE.NOTE] Zu unserem Beispiel schneller werden wir die Anzahl der Positionen von 1.000 bis 10 reduzieren. Die gleichen Prinzipien und Verfahren gelten jedoch für 1.000 Standorten. Der einzige Unterschied ist, dass ggf. von 1.000 Datensätze Trainieren Sie wahrscheinlich betrachten Sie das folgende PowerShell-Skripts parallel ausgeführt. Dazu gehört nicht zum Gegenstand dieses Artikels, aber finden Sie Beispiele von PowerShell Multithreading im Internet.  

## <a name="set-up-the-training-experiment"></a>Einrichten des Experiments Schulung

Wir werden eine [Schulung experimentieren](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) Beispiel verwenden, die wir bereits in der [Cortana Intelligence Katalog](http://gallery.cortanaintelligence.com)erstellt. Öffnen Sie dieses Experiment im Arbeitsbereich [Azure Machine Learning Studio](https://studio.azureml.net) .

>[AZURE.NOTE] Um mit diesem Beispiel ausführen, sollten Sie einem Standardarbeitsbereich anstelle einer freien Arbeitsbereich verwenden. Wir erstellen einen Endpunkt für jeden Debitor - insgesamt 10 Endpunkte - und einem Standardarbeitsbereich freier Arbeitsbereich auf 3 Endpunkte ist erforderlich. Wenn Sie nur einen freien Arbeitsbereich nur Skripts unten nur 3 Positionen zu ändern.

Das Experiment verwendet ein Modul **Import Data** Training Dataset *customer001.csv* von Azure Storage-Konto importieren. Nehmen wir an, wir haben alle Stationen Fahrrad Training Datasets gesammelt und in den gleichen Speicherort Blob mit Dateinamen von *rentalloc001.csv* bis *rentalloc10.csv*gespeichert.

![Bild](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

Beachten Sie, dass ein **Web Service** -Ausgabemodul Moduls **Zug Modell** hinzugefügt wurde.
Bei der Bereitstellung dieses Experiments als Webdienst zugeordnet Endpunkt Ausgabe ausgebildete Modell in einer .ilearner Datei zurückzugeben.

Beachten Sie, dass wir einen Webdienstparameter URL einrichten, die das Modul **Import Data** verwendet. Dadurch mit dem Parameter an individuelle Datensätze zum Trainieren des Modells für jeden Standort.
Es sind wie, die wir, mit einer SQL-Abfrage einen Webdienstparameter Daten aus einer SQL Azure-Datenbank abrufen oder einfach mit **Web Service Input** -Modul ein Dataset an den Webdienst übergeben hätten.

![Bild](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

Nun führen wir dieses trainingsexperiment mit Standard-Wert *rental001.csv* als das Trainings-Dataset. Die Ausgabe des Moduls **Evaluate** anzeigen klicken Sie auf die Ausgabe und **Visualisierung**wählen sehen Sie eine ordentliche Leistung der *AUC* = 0.91. An dieser Stelle können wir einen Webdienst aus dieser Schulung Experiment.

## <a name="deploy-the-training-and-scoring-web-services"></a>Bereitstellen der Ausbildung und Bewertung von Webdiensten

Training-Webdienst bereitstellen, klicken Sie unterhalb der Leinwand Experiment **Web-Service** , und wählen Sie **Web Service bereitstellen**. Rufen Sie diesen Webdienst "" Fahrrad Vermietung Training".

Nun müssen wir scoring Web Service bereitstellen.
Hierzu können wir Leinwand **Web-Service** klicken und **Vorhersehbare Webdienst**auswählen. Ein bewertungsexperiment wird erstellt.
Wir müssen einige kleinere Anpassungen zu arbeiten als Webdienst wie Label Spalte "Cnt" Entfernen von Eingabedaten und begrenzen die Ausgabe nur die Instanz-Id und dem entsprechenden Wert vorhergesagt.

Um die Arbeit zu sparen, können Sie einfach [vorhersageexperiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) im Katalog öffnen, die bereits vorbereitet wurde.

Um den Web Service bereitstellen führen Sie vorhersageexperiment, und klicken Sie unter der Leinwand **Webdienst bereitstellen** . Punktwertung Webdienst "Fahrrad Vermietung Bewertung" Name ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>Erstellen von 10 identischen Webdienstendpunkten mit PowerShell

Dieser Webdienst wird mit einem Standardendpunkt. Aber wir sind nicht der Standardendpunkt interessiert, da aktualisiert werden kann. Was wir tun müssen, ist 10 zusätzliche Endpunkte für jeden Standort erstellen. Wir tun dies mit PowerShell.

Zunächst stellen wir unseren PowerShell-Umgebung:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Führen Sie den folgenden PowerShell-Befehl:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Jetzt haben wir 10 Endpunkte erstellt und enthalten das gleiche ausgebildete Modell am *customer001.csv*. Sie können sie in Azure-Verwaltungsportal anzeigen.

![Bild](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>Aktualisieren Sie die Endpunkte um separate Training Datasets mit PowerShell verwenden

Im nächste Schritt ist die Endpunkte mit eindeutig für jede einzelne Kundendaten geschult aktualisieren. Aber zunächst müssen wir diese Modelle vom Webdienst **Fahrrad Vermietung Training** . Gehen Sie wir zurück zum Webdienst **Fahrrad Vermietung Training** . Wir müssen Endpunkts BES 10 mit 10 verschiedenen Datasets um 10 verschiedene Modelle aufrufen. Wir verwenden das PowerShell-Cmdlet **InovkeAmlWebServiceBESEndpoint** dazu.

Sie benötigen auch Anmeldeinformationen für das Speicherkonto Blob in `$configContent`, nämlich auf die `AccountName`, `AccountKey` und `RelativeLocation`. Die `AccountName` werden Ihrem Kontonamen wie im **Klassischen Azure-Verwaltungsportal** (Registerkarte "*Speicher* "). Sobald Sie auf ein Speicherkonto seine `AccountKey` mit **Zugriffstasten verwalten** Taste unten kopieren *Primärschlüssel Zugriff*finden. Die `RelativeLocation` ist der Pfad relativ zu den Speicher, in dem ein neues Modell gespeichert. Beispielsweise der Pfad `hai/retrain/bike_rental/` im Skript unter Punkt in einen Container mit dem Namen `hai`, und `/retrain/bike_rental/` Unterordner. Derzeit können keine Unterordner über das Portal Benutzeroberfläche erstellen, aber es gibt [Mehrere Azure Storage Explorer](../storage/storage-explorers.md) , mit denen Sie zu tun. Es wird empfohlen, einen neuen Container in die zur Speicherung von neuen ausgebildeten Modelle (.ilearner-Dateien) wie folgt erstellen: von Ihrer Speicherplatzseite klicken Sie unten auf **Hinzufügen** , und nennen Sie es `retrain`. Zusammenfassend zu folgenden Skript beziehen sich auf `AccountName`, `AccountKey` und `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

>[AZURE.NOTE] Bus-Endpunkt ist der einzige unterstützte Modus für diesen Vorgang. Ressourceneinträge können für geschulte Modelle verwendet werden.

Siehe oben statt 10 verschiedene BES Auftrag Json-Konfigurationsdateien erstellen wir dynamisch erstellen die Konfigurationszeichenfolge stattdessen und *JobConfigString* Parameter des Cmdlets **InvokeAmlWebServceBESEndpoint** feed, da gibt es wirklich keine Notwendigkeit, eine Kopie auf dem Datenträger.

Wenn alles gut geht, sollte nach einer Weile 10 .ilearner-Dateien aus *model001.ilearner* , *model010.ilearner*in Azure Storage-Konto angezeigt. Jetzt können wir unsere 10 bewertet Webdienstendpunkten mit dieser Modelle mit **Patch AmlWebServiceEndpoint** -PowerShell-Cmdlet zu aktualisieren. Beachten Sie wieder, dass wir nur die Standardendpunkte patch können wir bereits programmgesteuert erstellt.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

Dies sollte relativ schnell ausgeführt. Nach Abschluss die Ausführung werden wir 10 vorhersehbaren Webdienst-Endpunkte mit einem ausgebildeten Modell eindeutig bestimmten Datasets an einer Vermietung, aus einzelnen trainingsexperiment geschult erfolgreich erstellt. Um dies zu überprüfen, versuchen Sie mithilfe des **InvokeAmlWebServiceRRSEndpoint** -Cmdlets Endpunkte Aufrufen mit gleichen Eingabedaten und erwarten verschiedene Vorhersageergebnisse sehen, da die Modelle mit verschiedenen geschult sind.

## <a name="full-powershell-script"></a>Vollständige PowerShell-Skript

Hier ist die Liste der vollständige Quellcode:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
