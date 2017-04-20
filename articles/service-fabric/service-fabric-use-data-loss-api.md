<properties
   pageTitle="Wie Sie Datenverlust bei Fabric-Dienst aufrufen | Microsoft Azure"
   description="Beschreibt, wie den Datenverlust api"
   services="service-fabric"
   documentationCenter=".net"
   authors="LMWF"
   manager="rsinha"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="lemai"/>
   
# <a name="how-to-invoke-data-loss-on-services"></a>Wie Sie Datenverlust bei Services aufrufen

>[AZURE.WARNING] Dieses Dokument beschrieben in Ihre Dienste zu Datenverlusten führen und sollte mit Vorsicht verwendet werden.

## <a name="introduction"></a>Einführung
Sie können Datenverlust auf einer Partition der Fabric-Dienst durch Aufrufen von StartPartitionDataLossAsync() aufrufen.  Diese api verwendet Fault Injection und Analysis Services auf um gen Daten zu arbeiten.

## <a name="using-the-fault-injection-and-analysis-service"></a>Mithilfe der Fault Injection und Analysis Services

Fault Injection und Analysis Services unterstützt derzeit die folgenden APIs in der Tabelle.  Das Diagramm rechts zeigt entsprechende PowerShell-Cmdlet.  Finden Sie in der Msdn-Dokumentation für jede API Weitere Informationen zu jeder.

|           C#-API                    |         PowerShell-Cmdlets                      |
|-------------------------------------|-----------------------------------------------:|
|[StartPartitionDataLossAsync] [dl]   |[ServiceFabricPartitionDataLoss starten] [psdl]   |
|[StartPartitionQuorumLossAsync] [ql] |[ServiceFabricPartitionQuorumLoss starten] [psql] |
|[StartPartitionRestartAsync] [rp]    |[ServiceFabricPartitionRestart starten] [psrp]    |

## <a name="conceptual-overview-of-running-a-command"></a>Übersicht über einen Befehl ausführen

Fault Injection und Analysis Services verwendet ein asynchrones Modell, Sie starten mit einem API, API "Start" in diesem Dokument genannten und überprüft den Status dieses Befehls mit einem "" GetProgress "" API bis einen Endzustand erreicht ist oder bis Sie sie stornieren.
Starten Sie einen Befehl rufen Sie API "Start auf" für die entsprechende API.  Wenn diese API gibt Fault Injection und Analysis Services wurde die Anforderung akzeptiert.  Aber es zeigt nicht wie weit ein Befehl ausgeführt wurde oder noch gestartet wurde.  Status eines Befehls zu überprüfen, rufen Sie die "" GetProgress ""-API, die früher "Start"-API entspricht.  Die "" GetProgress "" API wird Objekteigenschaften, die den aktuellen Status des Befehls in der State-Eigenschaft zurück.  Ein Befehl läuft bis:

1.  Es wird erfolgreich abgeschlossen.  ""GetProgress "" auf in diesem Fall beim Aufruf wird Fortschritt Zustand abgeschlossen.
2.  Einen schwerwiegenden Fehler auftritt.  ""GetProgress "" auf in diesem Fall beim Aufruf wird Fortschritt Zustand fehlerhaft sein
3.  Abbrechen durch [CancelTestCommandAsync]  [ cancel] API oder [Stop-ServiceFabricTestCommand]  [ cancelps] PowerShell-Cmdlet.  Rufen Sie ""GetProgress "" auf in diesem Fall, werden Fortschritt Zustand abgebrochen oder ForceCancelled, je nach Argument für diese API.  Dokumentation für [CancelTestCommandAsync]  [ cancel] Weitere Informationen.


## <a name="details-of-running-a-command"></a>Details zum Ausführen eines Befehls

Rufen Sie auf, um eine Befehls-API beginnen mit der erwarteten Argumente.  Alle-APIs müssen eine Guid-Argument namens OperationId.  Sie sollten das Argument OperationId mitverfolgen da es verfolgen dieses Befehls verwendet wird.  Dies muss in die "" GetProgress"" API übergeben werden, um Verfolgen des Befehls.  Die OperationId muss eindeutig sein.

Nach dem Start API erfolgreich aufrufen, sollte API "GetProgress" bis zum Abschluss der zurückgegebenen Status des Status-Objekts in einer Schleife aufgerufen werden.  Alle [FabricTransientException des]  [ fte] und OperationCanceledExceptions sollte wiederholt werden.
Befehls (abgeschlossen, Faulted oder abgebrochen) einen Endzustand erreicht, müssen die zurückgegebenen Status-Objekts Ergebnis Weitere Informationen.  Wenn der Status abgeschlossen ist, enthält Result.SelectedPartition.PartitionId die Partitions-Id ausgewählt wurde.  Result.Exception ist null.  Wenn der Status fehlerhaft ist, Result.Exception die Ursache Fault Injection und Analysis Services den Befehl fehlerhaft.  Result.SelectedPartition.PartitionId haben die Partitions-Id, die ausgewählt wurde.  In einigen Fällen der Befehl möglicherweise nicht weit genug vorgegangen, eine Partition auszuwählen.  In diesem Fall werden die IDs 0.  Wenn der Status abgebrochen wird, wird Result.Exception null sein.  Wie Faulted Result.SelectedPartition.PartitionId haben die Partitions-Id, die ausgewählt wurde, sondern der Befehl dazu nicht weit genug fortgeschritten ist, werden 0.  Siehe auch das Beispiel unten.

Der nachfolgende Beispielcode veranschaulicht starten und Überprüfen des Fortschritts eines Befehls auf einer bestimmten Partition zu Datenverlusten führen.

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // The id of the target partition inside the target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have the chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

Im folgenden Beispiel veranschaulicht, wie mithilfe der PartitionSelector eine zufällige Partition des angegebenen Dienstes auszuwählen:

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have the Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## <a name="history-and-truncation"></a>Geschichte und Abschneiden

Nach ein Befehl einen Endzustand erreicht hat, bleiben seine Metadaten Fault Injection und Analyse für eine bestimmte Zeit bevor es entfernt, um Platz zu sparen.  Wenn "" GetProgress "" OperationId einen Befehl verwenden, nachdem es entfernt wurde, wird ein FabricException mit einem Fehlercode KeyNotFound zurückgegeben.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
