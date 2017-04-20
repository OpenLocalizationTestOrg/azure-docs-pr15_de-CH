<properties
   pageTitle="Ein Azure-Cluster und Ressourcen löschen | Microsoft Azure"
   description="Erfahren Sie, wie vollständig löschen Service Fabric cluster-Ressourcengruppe, die den Cluster löschen oder selektiv Löschen von Ressourcen."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>

# <a name="delete-a-service-fabric-cluster-on-azure-and-the-resources-it-uses"></a>Löschen Sie einen Cluster Service Fabric Azure und die verwendeten Ressourcen

Service Fabric-Cluster besteht aus vielen anderen Azure Ressourcen die Ressource selbst. Um einen Service Fabric-Cluster löschen, den müssen Sie auch alle Ressourcen löschen aus.
Haben Sie zwei Optionen: Löschen Sie die Ressourcengruppe, die der Cluster wird (die löscht die Clusterressource und anderen Ressourcen in der Ressourcengruppe) speziell die Ressource löschen und zugehörige Ressourcen (aber keine anderen Ressourcen in der Ressourcengruppe).

>[AZURE.NOTE] Der Cluster Ressource **nicht** löschen alle anderen Ressourcen, die der Service Fabric-Cluster aus.

## <a name="delete-the-entire-resource-group-rg-that-the-service-fabric-cluster-is-in"></a>Löschen Sie die gesamte Ressourcengruppe (RG) Service Fabric-cluster

Dies ist die einfachste Möglichkeit, um sicherzustellen, dass alle Ressourcen des Clusters einschließlich der Ressourcengruppe löschen. Sie können die Ressourcengruppe mit PowerShell oder Azure-Portal. Wenn der Ressourcengruppe Ressourcen, die nicht auf Service Fabric-Cluster beziehen, können Sie bestimmte Ressourcen löschen.

### <a name="delete-the-resource-group-using-azure-powershell"></a>Mithilfe von Azure PowerShell Ressourcengruppe löschen

Mit den folgenden Azure PowerShell-Cmdlets können Sie auch die Ressourcengruppe löschen. Stellen Sie sicher, dass Azure PowerShell 1.0 oder höher auf Ihrem Computer installiert ist. Wenn Sie dies noch nicht geschehen, führen Sie die Schritte [Installieren und Konfigurieren von Azure PowerShell.](../powershell-install-configure.md)

Öffnen Sie ein PowerShell-Fenster und führen Sie die folgenden Cmdlets PS:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

Sie erhalten eine Aufforderung, den Löschvorgang zu bestätigen, wenn Sie nicht die *-Force* Option. Bestätigung der RG und alle darin enthaltenen Ressourcen gelöscht.

### <a name="delete-a-resource-group-in-the-azure-portal"></a>Löschen einer Ressourcengruppe im Azure-portal  

1. Anmeldung bei [Azure-Portal](https://portal.azure.com).
2. Navigieren Sie zu der Service Fabric-Cluster zu löschen.
3. Klicken Sie auf die Ressource auf der Seite Essentials.
4. Dadurch wird die **Ressource Gruppe Essentials** -Seite.
5. Klicken Sie auf **Löschen**.
6. Gehen Sie die Seite löschen der Ressourcengruppe ausführen.

![Gruppe löschen][ResourceGroupDelete]


## <a name="delete-the-cluster-resource-and-the-resources-it-uses-but-not-other-resources-in-the-resource-group"></a>Löschen Sie die Ressource und die verwendeten Ressourcen jedoch nicht andere Ressourcen in der Ressourcengruppe

Wenn der Ressourcengruppe nur Ressourcen, die zum Cluster Service Fabric beziehen, die Sie löschen möchten, ist es einfacher, die gesamte Ressourcengruppe löschen. Wenn Sie eins von Ressourcen in der Ressourcengruppe selektiv löschen möchten, dann gehen Sie folgendermaßen vor.

Wenn Sie Cluster das Portal oder Service Fabric Resource Manager Vorlagen aus den Vorlagen bereitgestellt, sind alle Ressourcen, die der Cluster mit den folgenden zwei Tags gekennzeichnet. Sie können die Ressourcen festlegen möchten.

***1 tag:*** Key = ClusterName, Wert = "Name des Clusters"

***Tag 2:*** Key = ResourceName, Wert = ServiceFabric

### <a name="delete-specific-resources-in-the-azure-portal"></a>Löschen Sie bestimmte Ressourcen in Azure-portal

1. Anmeldung bei [Azure-Portal](https://portal.azure.com).
2. Navigieren Sie zu der Service Fabric-Cluster zu löschen.
3. Werden Sie **Alle** auf dem Essentials-Blade.
4. Klicken Sie auf **Tags** unter **Resource Management** Blade Settings.
5. Klicken Sie auf die **Tags** Tags Blatt eine Liste aller Ressourcen mit Tags.

    ![Resource-Tags][ResourceTags]

6. Haben Sie die Liste der markierten Ressourcen auf alle Ressourcen und löschen.

    ![Markierte Ressourcen][TaggedResources]

### <a name="delete-the-resources-using-azure-powershell"></a>Mithilfe von Azure PowerShell Ressourcen löschen

Sie können die Ressourcen einzeln durch Ausführen der folgenden Azure PowerShell-Cmdlets. Stellen Sie sicher, dass Azure PowerShell 1.0 oder höher auf Ihrem Computer installiert ist. Wenn Sie dies noch nicht geschehen, führen Sie die Schritte [Installieren und Konfigurieren von Azure PowerShell.](../powershell-install-configure.md)

Öffnen Sie ein PowerShell-Fenster und führen Sie die folgenden Cmdlets PS:

```powershell
Login-AzureRmAccount
```
Für jede Ressource möchten löschen, führen Sie Folgendes aus:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of the resource group>" -Force
```

Um die Ressource zu löschen, führen Sie folgenden Befehl:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of the resource group>" -Force
```

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den folgenden auch Informationen zum Aktualisieren eines Clusters und Partitionierung Services:

- [Erfahren Sie mehr über Cluster-upgrades](service-fabric-cluster-upgrade.md)
- [Erfahren Sie mehr über stateful Services für höchste Partitionierung](service-fabric-concepts-partitioning.md)


<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
