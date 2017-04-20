<properties
    pageTitle="Computerlernen Modelle programmgesteuert umschulen | Microsoft Azure"
    description="Informationen Sie zum programmgesteuerten Trainieren eines Modells und aktualisieren den Webdienst neu ausgebildete Modell in Azure Machine Learning verwenden."
    services="machine-learning"
    documentationCenter=""
    authors="raymondlaghaeian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="raymondl;garye;v-donglo"/>


# <a name="retrain-machine-learning-models-programmatically"></a>Umschulung maschinelles lernen Modelle programmgesteuert  

In dieser exemplarischen Vorgehensweise erfahren Sie, wie programmgesteuert eine Azure Computer Learning-Webdienst mit C# Computer Learning Batch Execution Service umschulen.

Nachdem Sie das Modell neu trainiert haben, veranschaulichen die folgenden exemplarischen Vorgehensweisen zum Aktualisieren des Modells im vorhersehbaren Webdienst:

- Bereitstellung eines klassischen Web Services im Computer lernen Web-Portal finden Sie [einen Webdienst Classic umschulen](machine-learning-retrain-a-classic-web-service.md). 
- Wenn Sie einen neuen Webdienst bereitgestellt, finden Sie unter [einen neuen Webdienst mit Machine Learning Management Cmdlets umschulen](machine-learning-retrain-new-web-service-using-powershell.md).

Eine Übersicht über die Umschulung finden Sie unter [umschulen Machine Learning-Modell](machine-learning-retrain-machine-learning-model.md).

Finden Sie mit den vorhandenen Webdienst neue Azure Ressourcen-Manager basiert starten in der [einen vorhersehbaren Webdienst umschulen](machine-learning-retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Erstellen eines Experiments Schulung
 
Beispielsweise verwenden Sie "Beispiel 5: Zug Test auswerten binäre Klassifizierung: Erwachsene Dataset" aus der Microsoft Azure maschinelles lernen. 
    
So erstellen Sie das experiment

1.  Melden Sie sich bei Microsoft Azure maschinelles lernen Studio. 
2.  Klicken Sie rechts unten im Dashboard auf **neu**.
3.  Wählen Sie Microsoft Samples Beispiel 5.
4.  Umbenennen des Experiments am oberen Rand der Leinwand Experiment wählen Sie Experiment "Beispiel 5: Zug Test auswerten binäre Klassifizierung: Erwachsene Dataset".
5.  Erhebung Modell.
6.  Am Ende des Experiments Leinwand, klicken Sie auf **Ausführen**.
7.  Klicken Sie auf **Set Up Web** und wählen Sie **Umschulung Webdienst aus**. 

    ![Ersten Versuch.][2]

Abbildung 2: Erste experimentieren.

## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Ein vorhersageexperiment erstellen und als Webdienst veröffentlichen  

Danach erstellen Sie ein Prädikativen experimentieren.

1.  Am unteren Rand der Leinwand Experiment auf **Web-Service** und wählen Sie **Vorhersehbare Webdienst**. Dies speichert das Modell als ausgebildeten und Web Service Input und Output Module hinzugefügt. 
2.  Klicken Sie auf **Ausführen**. 
3.  Nachdem das Experiment abgeschlossen wurde, klicken Sie auf **Webdienst bereitstellen [Classic]** oder **[New] Webdienst bereitstellen**.

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>Bereitstellen des Experiments Schulung als Webdienst Schulung

Um geschulte Modell umschulen müssen Sie trainingsexperiment bereitstellen, das als Webdienst Umschulung erstellt. Dieser Webdienst ein *Web Service* -Ausgabemodul verbunden die * [Zug Modell] [ train-model] * Modul neue ausgebildeten Modelle können.

1. Zurückgeben des Experiments Training Experimente Symbol im linken Bereich klicken namens Erhebung Modell experimentieren.  
2. Geben Sie im Suchfeld Suchbegriffe Experiment Webdienst. 
3. Ziehen Sie Modul *Web Service Input* Experiment Leinwand und *saubere fehlt* Moduls Ausgabe an.  Dadurch Ihre umschulungen Daten genauso wie Ihre ursprünglichen Daten verarbeitet.
4. Die Leinwand Experiment ziehen Sie zwei *Webdienst Output* Module. Verbinden Sie die Ausgabe des Moduls *Zug Modell* und die Ausgabe des Moduls *Auswerten Modell* zum anderen. Die Web Service-Ausgabe für **Zug** gibt uns neue ausgebildete Modell. Die Ausgabe **Auswerten** Modell zugeordnet sieht des Moduls, das die Ergebnisse.
5. Klicken Sie auf **Ausführen**. 

Weiter muss Experiment Schulung als Webdienst bereitstellen, die geschulten Model und Modellergebnisse erzeugt. Zu diesem Zweck sind die nächste Gruppe von Aktionen abhängig, ob Sie einen Webdienst Classic oder einen neuen Webdienst verwenden.  
  
**Standardansicht WebService**

Am unteren Rand der Leinwand Experiment auf **Web-Service** und wählen Sie **Webdienst bereitstellen [Classic]**. Webdienst- **Dashboard** wird der API-Hilfeseite mit der API-Schlüssel für die Batchausführung angezeigt. Die Batchausführung-Methode kann zum Erstellen von qualifizierten Modelle verwendet werden.

**Neuen Webdienst**

Am unteren Rand der Leinwand Experiment auf **Web-Dienst** , und wählen Sie **[New] Webdienst bereitstellen**. Das Web Service Azure Machine Learning Web Portal öffnet die Webseite Service bereitstellen. Geben Sie einen Namen für den Webdienst, und klicken Sie auf **Deploy**wählen Sie Zahlungsplan aus. Nur die Batchausführung-Methode kann verwendet werden, zum Erstellen von qualifizierten Modelle

Nach dem Experiment, abgeschlossen wurde, sollte der resultierende Workflow in beiden Fällen wie folgt aussehen:

![Resultierende Workflow nach.][4]

Abbildung 3: Was Workflow nach.

## <a name="retrain-the-model-with-new-data-using-bes"></a>Trainieren Sie das Modell mit neuen Daten BES

In diesem Beispiel verwenden C# Sie die Umschulung Anwendung erstellen. Den Beispielcode Python oder R können Sie diese Aufgabe.

Umschulung APIs aufrufen:

1. Erstellen Sie eine C# in Visual Studio (Neu -> Projekt -> Windows Desktop -> Console Application).
2.  Mit Computer lernen Web Service Portal anmelden.
3.  Einen Webdienst Classic arbeiten, klicken Sie auf **Klassische Webdienste**.
    1.  Klicken Sie auf den Webdienst, mit dem Sie arbeiten.
    2.  Klicken Sie auf der Standardendpunkt.
    3.  Klicken Sie auf **verwenden**.
    4.  Klicken Sie am unteren Rand der Seite **verbrauchen** im Abschnitt **Beispielcode** auf **Stapel**.
    5.  Weiter mit Schritt 5 dieses Verfahrens.
4.  Wenn Sie einen neuen Webdienst arbeiten, klicken Sie auf **Webdienste**.
    1.  Klicken Sie auf den Webdienst, mit dem Sie arbeiten.
    2.  Klicken Sie auf **verwenden**.
    3.  Klicken Sie am unteren Rand der Seite verbrauchen im Abschnitt **Beispielcode** auf **Stapel**.
5.  C# Beispielcode für die Batchausführung kopieren und Einfügen der Datei Program.cs sicherstellen, dass der Namespace erhalten bleibt.

Nuget-Paket Microsoft.AspNet.WebApi.Client gemäß Kommentare hinzufügen. Fügen Sie den Verweis auf Microsoft.WindowsAzure.Storage.dll müssen Sie zuerst die Clientbibliothek für Microsoft Azure-Speicherdienste zu installieren. Weitere Informationen finden Sie unter [Windows Storage Services](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-the-apikey-declaration"></a>Aktualisiert die Apikey-Deklaration

Suchen Sie die **Apikey** Deklaration.

    const string apiKey = "abc123"; // Replace this with the API key for the web service

Suchen Sie im **grundlegenden Verbrauch** Abschnitt der Seite **verbrauchen** den Primärschlüssel und die **Apikey** Deklaration.

### <a name="update-the-azure-storage-information"></a>Azure-Speicher aktualisieren

Der Beispielcode BES eine Datei von einem lokalen Laufwerk (z. B. "C:\temp\CensusIpnput.csv") in den Azure-Speicher, verarbeitet und schreibt die Ergebnisse in Azure-Speicher zurück.  

Zum Ausführen dieser Aufgabe müssen Sie den Speicher Name, Schlüssel und Container für das Speicherkonto aus der Azure-Verwaltungsportal und die entsprechenden Werte aktualisieren im Code Kontoinformationen. 

1. Melden Sie sich bei der Azure-Verwaltungsportal an.
1. Klicken Sie in der Spalte links auf **Speicher**.
1. Die Liste der Speicherkonten wählen Sie trainierte Modell speichern.
1. Klicken Sie am unteren Rand der Seite auf **Zugriffstasten verwalten**.
1. Kopieren speichern Sie der **Primärschlüssel Zugriff** und schließen Sie das Dialogfeld. 
1. Klicken Sie am oberen Rand der Seite auf **Container**.
1. Wählen Sie einen vorhandenen Container oder erstellen Sie einen neuen und speichern Sie den Namen.

Suchen Sie *StorageAccountName*, *StorageAccountKey*und *StorageContainerName* -Deklarationen und aktualisieren Sie die Werte, die in Azure-Portal gespeichert.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name
            
Außerdem müssen die ist die Eingabedatei an der Stelle im Code angeben. 

### <a name="specify-the-output-location"></a>Geben Sie den Ausgabespeicherort

Beim Angeben des Ausgabespeicherort in Request Payload muss die Erweiterung in *RelativeLocation* angegebene Datei als Ilearner angegeben werden. 

Siehe folgendes Beispiel:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

>[AZURE.NOTE] Die Namen der Ausgabespeicherort möglicherweise anders in dieser exemplarischen Vorgehensweise basierend auf der Reihenfolge, in der die Ausgabemodule Web Service hinzugefügt. Da dieses trainingsexperiment mit zwei Ausgaben einrichten enthalten die Ergebnisse Speicherortinformationen für beide Speicher.  

![Umschulung Ausgabe][6]

Abbildung 4: Umschulung Ausgabe.

## <a name="evaluate-the-retraining-results"></a>Die Umschulung Ergebnisse auswerten
 
Wenn Sie die Anwendung ausführen, enthält die Ausgabe Ergebnisse Zugriff auf URLs und SAS-Token.

Die Leistungsergebnisse trainierte Modell sehen durch Kombination von *BaseLocation*, *RelativeLocation*und *SasBlobToken* aus *ausgang2* Ausgabe (siehe vorherigen Umschulung Bild) und Einfügen vollständige URL in die Adressleiste des Browsers.  

Untersuchen Sie die Ergebnisse, um festzustellen, ob das neu ausgebildete Modell gut zum Ersetzen der vorhandenen ist.

*BaseLocation*, *RelativeLocation*und *SasBlobToken* aus der Ergebnisse kopieren, verwenden Sie sie bei der Umschulung.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie vorbeugende Webdienst auf **Webdienst bereitstellen [Classic]**bereitgestellt, finden Sie [einen Webdienst Classic umschulen](machine-learning-retrain-a-classic-web-service.md).

Wenn Sie vorbeugende Webdienst auf **Webdienst bereitstellen [New]**bereitgestellt, finden Sie unter [einen neuen Webdienst mit Machine Learning Management Cmdlets umschulen](machine-learning-retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/