<properties
   pageTitle="Benutzerdefinierte Szenarien | Microsoft Azure"
   description="Wie Sichern Sie Ihre Dienste für sicheres und fehlerhaftes Fehler."
   services="service-fabric"
   documentationCenter=".net"
   authors="anmolah"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="anmola"/>

# <a name="simulate-failures-during-service-workloads"></a>Fehler beim Dienst Arbeitslasten simulieren

Die Prüfbarkeit Szenarien in Azure Service Fabric können Entwickler sich nicht mit einzelnen Fehler. Es gibt Szenarien, wo eine explizite Überlappung der Client Arbeitslast und Fehlern notwendig sein können. Interleaving Client Arbeitslast und Fehler wird sichergestellt, dass der Dienst geschieht Fehler tatsächlich eine Aktion ausführt. Angesichts der Steuerelement, Prüfbarkeit bereitstellt, könnte diese an genau der Arbeitslast Ausführung. Diese Induktion Fehler innerhalb der verschiedenen Zustände in der Anwendung kann Fehler und Qualität.

## <a name="sample-custom-scenario"></a>Benutzerdefinierte Beispielszenario
Dieser Test zeigt ein Szenario, das Unternehmen Arbeitslast mit [ordnungsgemäß und fehlerhaftes](service-fabric-testability-actions.md#graceful-vs-ungraceful-fault-actions)kombiniert. Der Fehler sollte in der Mitte der Dienstvorgänge oder Compute besten ausgelöst.

Betrachten wir ein Beispiel für einen Dienst, der vier Arbeitslasten macht: A, B, C und jede entspricht einer Workflows und Compute, Speicher oder einer Kombination könnte. Der Einfachheit halber werden wir in unserem Beispiel der Workloads abstrahieren. Andere Fehler in diesem Beispiel ausgeführt werden:
  + RestartNode: Fehlerhaftes Fehler einen Neustart des Computers zu simulieren.
  + RestartDeployedCodePackage: Fehlerhaftes Fehler Diensthostprozess simulieren stürzt ab.
  + RemoveReplica: Ordnungsgemäß Fehler Replikat entfernen zu simulieren.
  + MovePrimary: Ordnungsgemäß Fehler Replikat simuliert wird ausgelöst durch Lastenausgleich Service Fabric verschoben.

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll.

using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        // Replace these strings with the actual version for your cluster and application.
        string clusterConnection = "localhost:19000";
        Uri applicationName = new Uri("fabric:/samples/PersistentToDoListApp");
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Workload Test...");
        try
        {
            RunTestAsync(clusterConnection, applicationName, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Workload Test failed: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Workload Test completed successfully.");
        return 0;
    }

    public enum ServiceWorkloads
    {
        A,
        B,
        C,
        D
    }

    public enum ServiceFabricFaults
    {
        RestartNode,
        RestartCodePackage,
        RemoveReplica,
        MovePrimary,
    }

    public static async Task RunTestAsync(string clusterConnection, Uri applicationName, Uri serviceName)
    {
        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);
        // Maximum time to wait for a service to stabilize.
        TimeSpan maxServiceStabilizationTime = TimeSpan.FromSeconds(120);

        // How many loops of faults you want to execute.
        uint testLoopCount = 20;
        Random random = new Random();

        for (var i = 0; i < testLoopCount; ++i)
        {
            var workload = SelectRandomValue<ServiceWorkloads>(random);
            // Start the workload.
            var workloadTask = RunWorkloadAsync(workload);

            // While the task is running, induce faults into the service. They can be ungraceful faults like
            // RestartNode and RestartDeployedCodePackage or graceful faults like RemoveReplica or MovePrimary.
            var fault = SelectRandomValue<ServiceFabricFaults>(random);

            // Create a replica selector, which will select a primary replica from the given service to test.
            var replicaSelector = ReplicaSelector.PrimaryOf(PartitionSelector.RandomOf(serviceName));
            // Run the selected random fault.
            await RunFaultAsync(applicationName, fault, replicaSelector, fabricClient);
            // Validate the health and stability of the service.
            await fabricClient.ServiceManager.ValidateServiceAsync(serviceName, maxServiceStabilizationTime);

            // Wait for the workload to finish successfully.
            await workloadTask;
        }
    }

    private static async Task RunFaultAsync(Uri applicationName, ServiceFabricFaults fault, ReplicaSelector selector, FabricClient client)
    {
        switch (fault)
        {
            case ServiceFabricFaults.RestartNode:
                await client.ClusterManager.RestartNodeAsync(selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RestartCodePackage:
                await client.ApplicationManager.RestartDeployedCodePackageAsync(applicationName, selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RemoveReplica:
                await client.ServiceManager.RemoveReplicaAsync(selector, CompletionMode.Verify, false);
                break;
            case ServiceFabricFaults.MovePrimary:
                await client.ServiceManager.MovePrimaryAsync(selector.PartitionSelector);
                break;
        }
    }

    private static Task RunWorkloadAsync(ServiceWorkloads workload)
    {
        throw new NotImplementedException();
        // This is where you trigger and complete your service workload.
        // Note that the faults induced while your service workload is running will
        // fault the primary service. Hence, you will need to reconnect to complete or check
        // the status of the workload.
    }

    private static T SelectRandomValue<T>(Random random)
    {
        Array values = Enum.GetValues(typeof(T));
        T workload = (T)values.GetValue(random.Next(values.Length));
        return workload;
    }
}
```
