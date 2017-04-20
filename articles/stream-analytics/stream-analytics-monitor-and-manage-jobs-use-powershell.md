<properties 
    pageTitle="Überwachen und Verwalten von Aufträgen mit PowerShell Stream Analytics | Microsoft Azure" 
    description="Erfahren Sie, wie Sie Azure PowerShell und Cmdlets zum Überwachen und Verwalten von Stream Analytics-Aufträge." 
    keywords="Azure Powershell Azure Powershell-Cmdlets, Powershell-Befehl Powershell-Skripting" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Überwachen und Verwalten von Projekten mit Azure PowerShell-Cmdlets Stream Analytics

Informationen Sie zum Überwachen und verwalten Stream Analytics Azure PowerShell-Cmdlets und Powershell Skripts, die grundlegende Stream Analytics Aufgaben ausführen.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Voraussetzung für die Ausführung von Azure PowerShell-Cmdlets für Stream Analytics

 - Erstellen Sie eine Ressourcengruppe Azure in Ihrem Abonnement. Das folgende ist Skript ein Beispielskript Azure PowerShell. Azure PowerShell-Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md);  

Azure PowerShell 0.9.8:  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>
 
        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure PowerShell 1.0:  

        # Log in to your Azure account
        Login-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
        
        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
        


> [AZURE.NOTE] Stream Analytics Arbeitsplätze programmgesteuert haben keine Überwachung standardmäßig aktiviert.  Sie können auch manuell überwachen im Azure-Portal mit den Auftrag Monitor Seite navigieren und auf die Schaltfläche aktivieren oder können Sie dies programmgesteuert tun Schritte am [Azure Stream Analytics - Monitor Stream Analytics Aufträge programmgesteuert](stream-analytics-monitor-jobs.md).

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Azure PowerShell-Cmdlets für Stream Analytics
Überwachen und Verwalten von Azure Stream Analytics-Aufträge können die folgenden Azure PowerShell-Cmdlets verwendet werden. Beachten Sie, dass Azure PowerShell Varianten. 
**In den aufgeführten Beispielen ist der erste Befehl Azure PowerShell 0.9.8, der zweite Befehl ist für Azure PowerShell 1.0.** Azure PowerShell 1.0 Befehle haben immer "AzureRM" in dem Befehl.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | AzureRMStreamAnalyticsJob abrufen
Listet alle Stream Analytics-Aufträge in der Azure-Abonnement oder angegebene Ressourcengruppe definiert oder Auftrag Informationen zu einem bestimmten Auftrag innerhalb einer Ressourcengruppe.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

PowerShell-Befehl gibt Informationen über alle Stream Analytics-Aufträge in Azure-Abonnement.

**Beispiel 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

PowerShell-Befehl gibt Informationen über alle Stream Analytics-Aufträge in der Ressourcengruppe StreamAnalytics Standard-Central US.

**Beispiel 3**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

PowerShell-Befehl gibt Informationen über den Auftrag für Stream Analytics StreamingJob in der Ressourcengruppe StreamAnalytics Standard-Central US.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | AzureRMStreamAnalyticsInput abrufen
Listet alle Eingaben, die in einem angegebenen Stream Analytics-Auftrag definiert sind, oder ruft Informationen über eine bestimmte Eingaben.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

PowerShell-Befehl gibt Informationen über alle Eingaben im Auftrag StreamingJob definiert.

**Beispiel 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

PowerShell-Befehl gibt Informationen über die Eingabe mit dem Namen EntryStream im Auftrag StreamingJob definiert.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | AzureRMStreamAnalyticsOutput abrufen
Listet alle Ausgaben, die in einem angegebenen Stream Analytics-Auftrag definiert sind, oder ruft Informationen über eine bestimmte Ausgabe.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

PowerShell-Befehl gibt Informationen über die Ausgaben gemäß Auftrag StreamingJob.

**Beispiel 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

PowerShell-Befehl gibt Informationen über die Ausgabe Ausgabe im Auftrag StreamingJob definierten Namen.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | AzureRMStreamAnalyticsQuota abrufen
Ruft Informationen über das Kontingent von streaming-Einheiten in einem angegebenen Bereich ab.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

PowerShell-Befehl gibt Informationen über Kontingent und Nutzung der Streaming-Einheiten in der Region USA.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Ruft Informationen über eine bestimmte Transformation in einem Stream Analytics-Auftrag definiert.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

PowerShell-Befehl gibt Informationen über Transformation namens StreamingJob in das Projekt StreamingJob.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Neue AzureStreamAnalyticsInput | Neue AzureRMStreamAnalyticsInput
Eine neue Eingabe in einen Stream Analytics-Auftrag erstellt oder eine vorhandene Eingabe aktualisiert.
  
Der Name der Eingabe kann in der JSON-Datei oder in der Befehlszeile angegeben werden. Wenn beide angegeben sind, muss der Name in der Befehlszeile in die Datei identisch sein.

Wenn Sie eine Eingabe, die bereits geben – Parameter Force das Cmdlet Fragen ob vorhandene Eingabe ersetzt.

Wenn Sie angeben – erzwingen Parameter, und geben Sie einen vorhandenen Namen eingeben, ohne Bestätigung die Eingabe ersetzt werden.

Ausführliche Informationen zu JSON Struktur und Inhalt finden Sie [Erstellen Input (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].

**Beispiel 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

PowerShell-Befehl erstellt eine neue Eingabe aus der Datei Input.json. Wenn vorhandene Eingabe mit der Eingabe Definitionsdatei angegebene Name bereits definiert ist, wird das Cmdlet ob ersetzen lassen.

**Beispiel 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

PowerShell-Befehl erstellt eine neue Eingabe in das Projekt mit der Bezeichnung EntryStream. Wenn vorhandene Eingabe mit diesem Namen bereits definiert ist, wird das Cmdlet ob ersetzen lassen.

**Beispiel 3**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

PowerShell-Befehl ersetzt die Definition der vorhandenen Eingabequelle mit der Definition aus der Datei EntryStream aufgerufen.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Neue AzureStreamAnalyticsJob | Neue AzureRMStreamAnalyticsJob
Erstellt einen neuen Stream Analytics-Auftrag in Microsoft Azure oder die Definition eines vorhandenen angegebenen Auftrag aktualisiert.

Der Name des Auftrags, kann in der JSON-Datei oder in der Befehlszeile angegeben werden. Wenn beide angegeben sind, muss der Name in der Befehlszeile in die Datei identisch sein.

Wenn Sie ein Projekt bereits geben die Parameter Force das Cmdlet fragt, ob er das vorhandene Projekt zu ersetzen.

Wenn Sie angeben – erzwingen Parameter, und geben Sie einen vorhandenen Auftrag an die Auftragsdefinition ohne Bestätigung ersetzt werden.

Detaillierte Informationen zu JSON Struktur und Inhalt finden Sie in den [Stream Analytics-Auftrag erstellen] [ msdn-rest-api-create-stream-analytics-job] [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].

**Beispiel 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Dieser Befehl PowerShell erstellt ein neues Projekt aus der Definition in JobDefinition.json. Wenn ein bestehendes Projekt mit den in der Datei angegebenen Namen bereits definiert ist, wird das Cmdlet ob ersetzen lassen.

**Beispiel 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

PowerShell-Befehl ersetzt die Auftragsdefinition für StreamingJob.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Neue AzureStreamAnalyticsOutput | Neue AzureRMStreamAnalyticsOutput
Neue Ausgabe in einen Stream Analytics-Auftrag erstellt oder eine vorhandene Ausgabe aktualisiert.  

Der Name der Ausgabe kann in der JSON-Datei oder in der Befehlszeile angegeben werden. Wenn beide angegeben sind, muss der Name in der Befehlszeile in die Datei identisch sein.

Wenn Sie eine bereits Ausgabe nicht angeben – Parameter Force das Cmdlet Fragen ob vorhandene Ausgabe ersetzen.

Wenn Sie angeben – erzwingen Parameter, und geben Sie einen vorhandenen Namen ausgeben, die Ausgabe ohne Bestätigung ersetzt.

Ausführliche Informationen zu JSON Struktur und Inhalt finden Sie [Erstellen Ausgabe (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].

**Beispiel 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

PowerShell-Befehl erstellt eine neue Ausgabe "Ausgabe" im Auftrag von StreamingJob aufgerufen. Wenn bereits vorhandene Ausgabe mit diesem Namen wird das Cmdlet ob ersetzen lassen.

**Beispiel 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

PowerShell-Befehl ersetzt die Definition für "Ausgabe" der Auftrag StreamingJob.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Neue AzureStreamAnalyticsTransformation | Neue AzureRMStreamAnalyticsTransformation
Eine neue Transformation in einen Stream Analytics-Auftrag erstellt oder aktualisiert die vorhandene Transformation.
  
Name der Transformation kann in der JSON-Datei oder in der Befehlszeile angegeben werden. Wenn beide angegeben sind, muss der Name in der Befehlszeile in die Datei identisch sein.

Wenn Sie eine Transformation, die bereits geben die Parameter Force das Cmdlet fragt, ob er ersetzt die vorhandene Transformation.

Wenn Sie angeben – erzwingen Parameter, und geben Sie eine vorhandene Transformation an Transformation ohne Bestätigung ersetzt werden.

Finden Sie detaillierte Informationen zu JSON Struktur und Inhalt der [Transformation erstellen (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].

**Beispiel 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

PowerShell-Befehl erstellt eine neue Transformation namens StreamingJobTransform in das Projekt StreamingJob. Wenn eine vorhandene Transformation mit diesem Namen bereits definiert ist, wird das Cmdlet ob ersetzen lassen.

**Beispiel 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 PowerShell-Befehl ersetzt die Definition des StreamingJobTransform im Auftrag StreamingJob.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Entfernen AzureStreamAnalyticsInput | AzureRMStreamAnalyticsInput entfernen
Asynchron löscht bestimmte Eingaben aus einem Stream Analytics-Auftrag in Microsoft Azure.  
Wenn Sie angeben – Parameter Force ohne Bestätigung die Eingabe gelöscht werden.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

PowerShell-Befehl entfernt input EventStream Auftrag StreamingJob  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Entfernen AzureStreamAnalyticsJob | AzureRMStreamAnalyticsJob entfernen
Asynchron Löscht einen bestimmten Stream Analytics-Auftrag in Microsoft Azure.  
Bei Angabe der – Force-Parameter der Auftrag ohne Bestätigung gelöscht.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

PowerShell-Befehl entfernt den Auftrag StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Entfernen AzureStreamAnalyticsOutput | AzureRMStreamAnalyticsOutput entfernen
Asynchron Löscht eine bestimmte Ausgabe aus einem Stream Analytics-Auftrag in Microsoft Azure.  
Wenn Sie angeben – Parameter Force Ausgabe ohne Bestätigung gelöscht.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Dieser Befehl PowerShell entfernt die Ausgabe im Auftrag StreamingJob ausgegeben.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Start AzureStreamAnalyticsJob | AzureRMStreamAnalyticsJob starten
Asynchron bereitgestellt und Microsoft Azure Stream Analytics-Auftrag beginnt.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

PowerShell-Befehl startet den Auftrag StreamingJob mit einer benutzerdefinierten Ausgabe Startzeit, 12. Dezember 2012 12:12:12 festgelegt UTC.


### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Stop AzureStreamAnalyticsJob | AzureRMStreamAnalyticsJob beenden
Asynchron beendet einen Stream Analytics-Auftrag in Microsoft Azure ausgeführt und Zuweisung aufgehoben wurden, verwendet Ressourcen. Die Auftragsdefinition und Metadaten bleibt in Ihr Abonnement durch den Azure-Portal und Management-APIs der Auftrag bearbeitet und neu gestartet werden. Sie werden nicht für einen Auftrag beendet belastet.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

PowerShell-Befehl beendet den Auftrag StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Test-AzureStreamAnalyticsInput | Test-AzureRMStreamAnalyticsInput
Testen, ob der Stream Analytics Herstellen einer Eingabe.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

PowerShell-Befehl testet den Verbindungsstatus der Eingabe EntryStream StreamingJob.  

###<a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Test-AzureStreamAnalyticsOutput | Test-AzureRMStreamAnalyticsOutput
Testen, ob der Stream Analytics angegebene Ausgabe herstellen.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Dieser Befehl PowerShell Tests der Verbindungsstatus der Ausgabe StreamingJob ausgegeben.  

## <a name="get-support"></a>Support erhalten
Versuchen Sie [Azure Stream Analytics Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)für weitere Unterstützung. 


## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 



[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
