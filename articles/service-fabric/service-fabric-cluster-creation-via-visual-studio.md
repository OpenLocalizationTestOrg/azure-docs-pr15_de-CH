<properties
   pageTitle="Einrichten eines Service Fabric-Clusters mit Visual Studio | Microsoft Azure"
   description="Beschreibt das Service Fabric-Cluster mit Azure Ressourcenmanager Vorlage erstellt eine Ressourcengruppe Azure-Projekt in Visual Studio"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>Mit Visual Studio Service Fabric-Cluster einrichten
Dieser Artikel beschreibt das Einrichten eines Azure Service Fabric-Clusters mithilfe von Visual Studio und Azure-Ressourcen-Manager-Vorlage. Wir verwenden Projekt Visual Studio Azure Ressourcen zum Erstellen der Vorlage. Nachdem die Vorlage erstellt wurde, kann es in Visual Studio direkt in Azure bereitgestellt werden. Sie können auch aus einem Skript oder als Teil der Anlage fortlaufende Integration (CI) verwendet werden.

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>Mithilfe einer Projektgruppe Azure Ressource Vorlage Cluster Service Fabric
Zunächst öffnen Sie Visual Studio und erstellen ein Azure Ressource Gruppenprojekt (es steht im Ordner " **Cloud** "):

![Dialogfeld "Neues Projekt" Azure-Ressourcengruppe Projekt][1]

Sie können eine neue Visual Studio-Projektmappe für dieses Projekt erstellen oder eine vorhandene Projektmappe hinzufügen.

>[AZURE.NOTE] Azure Ressource Gruppenprojekt Cloud-Knoten nicht angezeigt wird, haben Sie keine Azure SDK installiert. Starten Web Platform Installer ([Jetzt installieren](http://www.microsoft.com/web/downloads/platform.aspx) noch nicht getan haben), und suchen Sie nach "Azure SDK für .NET" die Version, die mit Ihrer Version von Visual Studio.

Nach die Schaltfläche OK drücken fordert Visual Studio Sie Ressourcen-Manager-Vorlage auswählen, die Sie erstellen möchten:

![Wählen Sie Vorlage Azure Service Fabric-Cluster-Vorlage ausgewählt][2]

Wählen Sie die Vorlage Service Fabric-Cluster und klicken Sie OK wieder. Die Projekt- und Ressourcenmanager Vorlage ist jetzt erstellt.

## <a name="prepare-the-template-for-deployment"></a>Vorbereiten der Vorlage für die Bereitstellung
Vor der Bereitstellung der Vorlage zum Erstellen des Clusters müssen Sie Werte für die Vorlagenparameter erforderliche angeben. Diese Parameterwerte aus Lesen der `ServiceFabricCluster.parameters.json` -Datei in die `Templates` Ordner des Projekts Gruppe Ressource. Öffnen Sie die Datei, und geben Sie die folgenden Werte:

|Parametername           |Beschreibung|
|-----------------------  |--------------------------|
|adminUserName            |Der Name des Administratorkontos für Service Fabric-Computern (Knoten).|
|certificateThumbprint    |Der Fingerabdruck des Zertifikats, das Cluster sichert.|
|sourceVaultResourceId    |Die *Ressourcen-ID* des Schlüssels Depots des Zertifikats, die Cluster sichert Speicherort.|
|certificateUrlValue      |Der URL des Cluster-Sicherheitszertifikat.|

Vorlage der Visual Studio Service Fabric Resource Manager erstellt einen sicheren Cluster durch ein Zertifikat geschützt ist. Dieses Zertifikat wird durch die letzten drei Vorlagenparameter identifiziert (`certificateThumbprint`, `sourceVaultValue`, und `certificateUrlValue`), und muss in einer **Azure Key Vault**vorhanden sein. Weitere Informationen zum Cluster-Sicherheitszertifikat [Service Fabric cluster Sicherheitsszenarios](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) Artikel erstellen.

## <a name="optional-change-the-cluster-name"></a>Optional: Ändern Sie den Namen des Clusters
Jeder Cluster Service Fabric hat einen Namen. Beim Erstellen ein Clusters Fabric in Azure bestimmt Clusternamen (zusammen mit der Azure-Region) Namen (DNS = Domain Name System) für den Cluster. Beispielsweise Cluster name `myBigCluster`, und der Speicherort (Azure Region) der Ressource, die den neuen Cluster hosten ostasiatischen US, der DNS-Namen des Clusters `myBigCluster.eastus.cloudapp.azure.com`.

Standardmäßig ist der Clustername automatisch generiert und durch Anfügen einer zufälligen Suffix "Cluster" Präfix eindeutig gemacht. Dies erleichtert die Vorlage als Teil eines Systems **fortlaufende Integration** (CI) verwenden. Wenn Sie einen bestimmten Namen für den Cluster verwenden möchten, einen sinnvollen, legen Sie den Wert von der `clusterName` Variable in der Ressourcen-Manager (`ServiceFabricCluster.json`) ausgewählten Namen. Die erste Variable in der Datei definiert ist.

## <a name="optional-add-public-application-ports"></a>Optional: Fügen Sie öffentliche Anwendungsports
Sie sollten auch öffentliche Anwendungsports für den Cluster vor der Bereitstellung zu ändern. Standardmäßig öffnet die Vorlage nur zwei öffentliche TCP-Port (80 und 8081). Benötigen Sie für Ihre Anwendung, ändern Sie die Definition der Azure-Lastenausgleich in der Vorlage. Die Definition wird in der wichtigsten gespeichert (`ServiceFabricCluster.json`). Öffnen Sie die Datei, und suchen Sie nach `loadBalancedAppPort`. Jeder Port gibt es drei Elemente:

1. Eine Vorlage-Variable, die den Wert der TCP-Port für den Port definiert:

    ```json
    "loadBalancedAppPort1": "80"
    ```

2. Eine *Probe* , die definiert, wie oft und für wie lange Azure Load Balancer versucht, einen bestimmten Service Fabric-Knoten vor dem Failover auf einen anderen verwenden. Prüfpunkte sind Teil der Lastenausgleich Ressource. Hier ist die Probe Definition für den ersten Standardanschluss Anwendung:

    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```

3. Eine *Regel Lastenausgleich* , die verbindet den Port und Sonde Lastenausgleich über mehrere Service Fabric-Clusterknoten aktiviert:

    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
Wenn die Anwendung, die Sie zum Cluster bereitstellen möchten mehr Ports benötigen, können Sie sie Erstellen zusätzlicher Prüfpunkt mit Lastenausgleich Definitionen hinzufügen. Weitere Informationen zum Arbeiten mit Lastenausgleich Azure Ressourcenmanager Vorlagen finden Sie unter [Erste Schritte beim Erstellen einer internen Lastenausgleich mithilfe einer Vorlage](../load-balancer/load-balancer-get-started-ilb-arm-template.md).

## <a name="deploy-the-template-by-using-visual-studio"></a>Bereitstellen der Vorlage mithilfe von Visual Studio
Nach dem Speichern die erforderlichen Parameterwerte in den`ServiceFabricCluster.param.dev.json` Datei, Sie können die Vorlage bereitstellen und Service Fabric-Cluster erstellen. Maustaste Ressource Gruppenprojekt in Visual Studio Solution Explorer und wählen Sie **Bereitstellen | Neue Bereitstellung...**. Ggf. zeigen Visual Studio das Dialogfeld **Bereitstellen Ressourcengruppe** Azure Authentifizierung aufgefordert:

![Ressourcengruppe Dialogfeld bereitstellen][3]

Im Dialogfeld können Sie eine Ressourcengruppe Ressourcen-Manager für den Cluster auswählen und bietet die Möglichkeit, eine neue zu erstellen. Es ist normalerweise sinnvoll Service Fabric-Cluster mit einer eigenen Ressourcengruppe.

Nach dem Drücken der Schaltfläche bereitstellen fordert Visual Studio Sie Parameterwerte Vorlage zu bestätigen. Die Schaltfläche **Speichern** . Ein Parameter keinen dauerhaften Wert: das Administratorkonto-Kennwort für den Cluster. Sie müssen einen Wert bereitstellen, wenn Visual Studio eine einzugeben.

>[AZURE.NOTE] Beginnend mit Azure SDK 2.9 unterstützt Visual Studio lesen Kennwörter von **Azure Key Vault** während der Bereitstellung. Klicken Sie im Dialogfeld Vorlage Parameter Beachten Sie, dass die `adminPassword` wurde das Textfeld Parameter ein "Key" Symbol auf der rechten Seite. Dieses Symbol können Sie einen vorhandenen Schlüssel Vault-Schlüssel als das Administratorkennwort für den Cluster auswählen. Achten Sie erste Azure Resource Manager Zugriff für die vorlagenbereitstellung in die erweiterte Richtlinien des Schlüssels Vault aktivieren. 

Überwachen Sie den Fortschritt des Bereitstellungsprozesses im Visual Studio-Ausgabefenster. Nach Abschluss die vorlagenbereitstellung ist der neue Cluster einsatzbereit!

>[AZURE.NOTE] Wenn PowerShell Azure Computer verwalten, die Sie verwenden noch nie verwendet wurde, müssen Sie etwas bringen.
>1. Aktivieren Sie PowerShell Skripts mit den [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) Befehl. Richtlinie "unrestricted" ist für die Entwicklungscomputer normalerweise ausreichend.
>2. Entscheiden, ob Sie Diagnosedaten Auflistung von Azure PowerShell-Befehle ermöglichen, und führen Sie [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) oder [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) nach Bedarf. Dies verhindert unnötige Aufforderung während der Bereitstellung.

Wenn Fehler vorhanden sind, zum [Azure-Portal](https://portal.azure.com/) und öffnen die Ressourcengruppe, der Sie bereitgestellt. **Alle Einstellungen**klicken **Bereitstellung** auf die Standardeinstellungen. Eine fehlgeschlagene Ressourcengruppen Bereitstellung bleibt ausführlichen Diagnoseinformationen vorhanden.

>[AZURE.NOTE] Service Fabric-Cluster benötigen eine bestimmte Anzahl von Knoten werden Verfügbarkeit und Beibehalten des Status - genannt "Quorum verwalten". Daher ist es nicht sicher alle Computer im Cluster Herunterfahren, wenn Sie zuerst eine [vollständige Sicherung des Systemstatus](service-fabric-reliable-services-backup-restore.md)ausgeführt haben.

## <a name="next-steps"></a>Nächste Schritte
- [Informationen Sie zum Einrichten von Service Fabric-Clusters mithilfe der Azure-portal](service-fabric-cluster-creation-via-portal.md)
- [Informationen Sie zum Verwalten und Bereitstellen von Service Fabric-Anwendung mit Visual Studio](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
