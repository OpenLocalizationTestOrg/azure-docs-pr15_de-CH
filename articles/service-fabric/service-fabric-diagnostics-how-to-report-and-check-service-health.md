<properties
   pageTitle="Bericht und überprüfen mit Azure Service | Microsoft Azure"
   description="Informationen Sie Berichte Code Dienst senden und auf den Zustand Ihres Dienstes mit den Health monitoring Azure Fabric Service bietet."
   services="service-fabric"
   documentationCenter=".net"
   authors="toddabel"
   manager="mfussell"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/06/2016"
   ms.author="toddabel"/>

# <a name="report-and-check-service-health"></a>Bericht und Überprüfen des Dienststatus
Wenn Ihre Dienste Probleme hängt die Möglichkeit zu reagieren auf Vorfälle und Ausfälle beheben Probleme schnell erkennen. Wenn Sie Probleme und Fehler Azure Service Fabric Health Manager Service Code melden, können Sie standard Health monitoring Tools Fabric Service bietet, um den Status zu überprüfen.

Es gibt zwei Methoden, um aus dem Dienst zu melden:

- Verwenden Sie [Partition](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.aspx) oder [CodePackageActivationContext](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.aspx) Objekte.  
Sie können die `Partition` und `CodePackageActivationContext` Objekte auf den Zustand der Elemente zu melden, die Teil des aktuellen Kontexts. Beispielsweise kann Code, der Teil eines Replikats meldet Gesundheit nur dieses Replikat, die zugehörige Partition und die Anwendung, der es gehört.

- Mit `FabricClient`.   
Sie können `FabricClient` Bericht Gesundheit aus den Code ist der Cluster nicht [sichere](service-fabric-cluster-security.md) oder wenn der Dienst mit Administratorrechten ausgeführt wird. Nicht werden in den meisten realen Szenarien. Mit `FabricClient`, eine Entität, die Teil des Clusters ist Zustand melden. Im Idealfall sollten jedoch Servicecode nur Berichte senden, die eigene Gesundheit zusammenhängen.

Dieser Artikel führt Sie durch ein Beispiel, das aus den Code meldet. Außerdem wird veranschaulicht wie die Fabric Service bietet Tools zur den Status überprüfen. Dieser Artikel soll eine kurze Einführung in Health Service Fabric Überwachungsfunktionen. Weitere Informationen erhalten Sie ausführliche Artikelserie über Gesundheit, die mit den Link am Ende dieses Artikels.

## <a name="prerequisites"></a>Erforderliche Komponenten
Sie müssen folgende Komponenten installiert:

   * Visual Studio 2015
   * Service Fabric SDK

## <a name="to-create-a-local-secure-dev-cluster"></a>Erstellen eines lokalen sicheren Dev-Clusters
- PowerShell mit Administratorrechten geöffnet, und führen Sie die folgenden Befehle.

![Befehle, die zum Erstellen eines sicheren Dev Clusters anzeigen](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Bereitstellen einer Anwendung und deren Status überprüfen

1. Öffnen Sie Visual Studio als Administrator.

2. Erstellen Sie ein Projekt mithilfe der Vorlage **Statusbehaftete Dienst** .

    ![Erstellen einer Anwendung Service Fabric mit statusbehaftete Dienst](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)

3. Drücken Sie **F5** , um die Anwendung im Debugmodus ausgeführt. Die Anwendung wird auf dem lokalen Cluster bereitgestellt werden.

4. Wenn die Anwendung ausgeführt wird, das lokale Cluster Manager-Symbol im Infobereich Maustaste, und wählen **Lokalen Cluster verwalten** im Kontextmenü Service Fabric-Explorer öffnen.

    ![Infobereich Service Fabric-Explorer öffnen](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)

5. Die Anwendungsintegrität sollen in diesem Bild angezeigt werden. Zu diesem Zeitpunkt sollte die Anwendung fehlerfrei ohne Fehler.

    ![Gesunde Anwendung Service Fabric-Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)

6. Sie können auch den Zustand mithilfe von PowerShell überprüfen. Sie können ```Get-ServiceFabricApplicationHealth``` des Anwendungszustands, und Sie können ```Get-ServiceFabricServiceHealth``` Überprüfen des Dienststatus. Integritätsbericht für dieselbe Anwendung in PowerShell ist in diesem Bild.

    ![Gesunde Anwendung in PowerShell](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Ihr Service-Code benutzerdefinierte Systemereignisse hinzufügen
Service Fabric-Projektvorlagen in Visual Studio enthalten Beispielcode. Die folgenden Schritte zeigen Berichte benutzerdefinierten Systemüberwachungsereignissen Service Code. Berichte werden automatisch in die standard-Tools für die Überwachung bereit Service Fabric Service Fabric-Explorer Azure Portal gesundheitlichen und PowerShell angezeigt.

1. Öffnen Sie die Anwendung, die Sie in Visual Studio erstellt, oder erstellen Sie eine neue Anwendung mit der **Statusbehaftete Dienst** Visual Studio-Vorlage.

2. Öffnen Sie die Datei Stateful1.cs, und suchen die `myDictionary.TryGetValueAsync` in Aufrufen der `RunAsync` Methode. Sie sehen können, diese Methode gibt einen `result` , den aktuellen Wert des Leistungsindikators enthält, ist die Schlüssel Logik in dieser Anwendung Zählung ausgeführt. Bei einer echten Anwendung und mangelnde Ergebnis ein Fehler dargestellt, möchten Sie dieses Ereignis kennzeichnen.

3. Melden Sie ein Ereignis, wenn der Mangel Ergebnis Fehler darstellt, hinzugefügt.

    ein. Hinzufügen der `System.Fabric.Health` Namespace in die Datei Stateful1.cs.

    ```csharp
    using System.Fabric.Health;
    ```

    b. Fügen Sie folgenden Code nach dem `myDictionary.TryGetValueAsync` aufrufen

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    Wir melden Replikat Gesundheit da statusbehaftete Service gemeldet wird. Die `HealthInformation` -Parameter speichert Informationen die Gesundheit, die gemeldet wird.

    Wenn Sie statusfreien Dienst erstellt haben, verwenden Sie folgenden code

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```

4. Wenn Ihr Dienst mit Administratorrechten ausgeführt wird oder wenn der Cluster nicht [sicher](service-fabric-cluster-security.md)ist, Sie auch können `FabricClient` Bericht Gesundheit wie nachfolgend gezeigt.  

    ein. Erstellen der `FabricClient` -Instanz nach der `var myDictionary` Deklaration.

    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```

    b. Fügen Sie folgenden Code nach dem `myDictionary.TryGetValueAsync` aufrufen.

    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.ServiceInitializationParameters.PartitionId,
            this.ServiceInitializationParameters.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```

5. Wir diesen Fehler simulieren und es Health monitoring Tools angezeigt. Um den Fehler zu simulieren, kommentieren Sie die erste Zeile der Gesundheitsberichterstattung zuvor hinzugefügten Code. Nachdem Sie die erste Zeile auskommentieren, sieht der Code wie im folgenden Beispiel.

    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
 Dieser Code jetzt löst diese Integritätsbericht jeweils `RunAsync` ausgeführt wird. Nachdem Sie diese Änderung vorgenommen haben, drücken Sie **F5** , um die Anwendung auszuführen.

6. Wenn die Anwendung ausgeführt wird, öffnen Sie Service Fabric-Explorer, um den Zustand der Anwendung überprüfen. Dieses Mal zeigt Service Fabric-Explorer, dass die Anwendung fehlerhaft ist. Dies ist der Fehler, die vom Code gemeldet wurde bereits hinzugefügt.

    ![Fehlerhafte Anwendung Service Fabric-Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)

7. Wenn Sie in der Strukturansicht des Service Fabric-Explorer primäre Replikat auswählen, sehen Sie **Zustand** zu zeigt einen Fehler an. Service Fabric-Explorer zeigt auch die Gesundheit Berichtsdetails, die hinzugefügt wurden, die `HealthInformation` Parameter im Code. Sie können dieselben Health Berichte in PowerShell und Azure-Portal anzeigen.

    ![Replikat Gesundheit Service Fabric-Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Dieser Bericht bleibt im Status-Manager bis ersetzt wird von einem anderen Bericht oder dieses Replikat gelöscht wird. Da wir nicht festgelegt haben `TimeToLive` für diesen Bericht Zustand in der `HealthInformation` -Objekt Bericht nie abläuft.

Wir empfehlen, Gesundheit auf die präzise gemeldet werden sollen, in diesem Fall das Replikat. Sie können auch melden Gesundheit `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Bericht Gesundheit auf `Application`, `DeployedApplication`, und `DeployedServicePackage`, verwenden `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Nächste Schritte
[Detaillierte technische Informationen zum Service Fabric-Zustand](service-fabric-health-introduction.md)
