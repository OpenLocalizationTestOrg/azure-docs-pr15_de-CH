<properties
   pageTitle="Konfigurieren Sie die Aktualisierung einer Anwendung Service Fabric | Microsoft Azure"
   description="Erfahren Sie, wie die Einstellungen für die Aktualisierung von Service Fabric-Anwendung mithilfe von Microsoft Visual Studio."
   services="service-fabric"
   documentationCenter="na"
   authors="cawaMS"
   manager="paulyuk"
   editor="tglee" />
<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/29/2016"
   ms.author="cawa" />

# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>Konfigurieren Sie die Aktualisierung einer Anwendung Fabric Service in Visual Studio

Visual Studio-Tools für Azure Service unterstützen Upgrade, lokale oder remote-Cluster veröffentlicht. Es gibt zwei Vorteile Aktualisierung der Anwendung auf eine neuere Version statt ersetzen die Anwendung testen und Debuggen:

- Daten werden nicht während der Aktualisierung verloren.
- Verfügbarkeit hoch, Unterbrechung während der Aktualisierung nicht genügend Dienstinstanzen Upgrade Domänen verteilt sind.

Tests können für eine Anwendung ausgeführt werden, während er aktualisiert wird.

## <a name="parameters-needed-to-upgrade"></a>Parameter aktualisieren

Sie können zwei Arten der Bereitstellung: reguläre oder aktualisieren. Eine regelmäßige Bereitstellung löscht alle vorherigen Informationen zur Bereitstellung und Daten auf dem Cluster beim Upgrade Deployment beibehalten. Beim Aktualisieren einer Anwendung Fabric Service in Visual Studio müssen Sie bieten Upgrade Anwendungsparameter und Gesundheit Richtlinien überprüfen. Upgrade Anwendungsparameter kontrollieren die Aktualisierung Health Check Richtlinien legen fest, ob die Aktualisierung erfolgreich war. Siehe [Fabric Service Application Upgrade: Parameter aktualisieren](service-fabric-application-upgrade-parameters.md) Weitere Informationen.

Es gibt drei Upgrade: *überwacht*, *UnmonitoredAuto*und *UnmonitoredManual*.

  - Überwachte Aktualisierung automatisiert die Aktualisierung und Anwendung Health Check.

  - UnmonitoredAuto Aktualisierung automatisiert die Aktualisierung aber überspringt die Überprüfung auf Fehlerfreiheit.

  - Dabei UnmonitoredManual Aktualisierung müssen Sie jede Aktualisierungsdomäne manuell aktualisieren.

Jede Aktualisierungsmodus benötigt unterschiedliche Parameter. Finden Sie weitere Informationen zu den verfügbaren Aktualisierungsoptionen [Anwendung Parameter aktualisieren](service-fabric-application-upgrade-parameters.md) .

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Aktualisieren einer Service Fabric-Anwendung in Visual Studio

Wenn Sie Visual Studio Service Fabric-Tools verwenden Service Fabric-Anwendung aktualisieren, können Sie einen Veröffentlichungsvorgang zu Aktualisierung statt einer normalen Bereitstellung durch das **Aktualisieren der Anwendungdes** Kontrollkästchen angeben.

### <a name="to-configure-the-upgrade-parameters"></a>So konfigurieren Sie die Upgrade-Parameter

1. Klicken Sie auf **die Schaltfläche neben dem Kontrollkästchen** . Das Dialogfeld **Aktualisieren Parameter bearbeiten** wird angezeigt. Das Dialogfeld **Aktualisieren Parameter bearbeiten** Modi der überwachte, UnmonitoredAuto und UnmonitoredManual aktualisieren.

2. Wählen Sie die aktualisieren, die Sie verwenden möchten und füllen Sie das Parameterraster.

    Jeder Parameter hat Standardwerte. Der optionale Parameter *DefaultServiceTypeHealthPolicy* hat eine Hash-tabelleneingabe. Hier ist ein Beispiel für das Hash Tabellenformat für *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* ist optional, die eine Hash-tabelleneingabe im folgenden Format verwendet:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Hier ist ein Beispiel:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```

3. Wählt UnmonitoredManual Aktualisierungsmodus müssen Sie manuell eine PowerShell-Konsole, und beenden den Aktualisierungsvorgang starten. Siehe [Fabric Service Application Upgrade: Erweiterte Themen](service-fabric-application-upgrade-advanced.md) lernen wie manuelle Aktualisierung funktioniert.

## <a name="upgrade-an-application-by-using-powershell"></a>Aktualisieren einer Anwendung mithilfe von PowerShell

PowerShell-Cmdlets können Service Fabric-Anwendung aktualisieren. Ausführliche Informationen finden Sie unter [Service Fabric-Anwendung Lernprogramm aktualisieren](service-fabric-application-upgrade-tutorial.md) und [Start-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) .

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Geben Sie eine Integritätsrichtlinie Kontrollkästchen in der Anwendungsmanifestdatei

Jeder Dienst in einer Anwendung Service Fabric können eigene Gesundheitsparameter, die die Standardwerte überschreiben. Sie können diese Parameterwerte in der Anwendungsmanifestdatei.

Im folgenden Beispiel wird veranschaulicht, wie eine eindeutige Health Check Politik für jeden Dienst im Anwendungsmanifest.

```
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Bereitstellen einer Anwendung finden Sie unter [Bereitstellen einer vorhandenen Anwendung in Azure Service Fabric](service-fabric-deploy-existing-app.md).
