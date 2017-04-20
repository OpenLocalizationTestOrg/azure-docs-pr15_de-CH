<properties
    pageTitle="Azure Government Services | Microsoft Azure"
    description="Informationen zum Verwalten von Abonnements in Azure Government"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/21/2016"
    ms.author="zakramer" />


#  <a name="managing-and-connecting-to-your-subscription-in-azure-government"></a>Verwalten und eine Verbindung zu Ihrem Abonnement in Azure Government

Azure Regierung hat eindeutige URLs und Endpunkte für die Umwelt. Es ist wichtig, die richtigen Anschlüsse zur Verwaltung der Umgebung über das Portal oder PowerShell. Sobald der Regierung Azure-Umgebung verbunden sind, normalen Operationen zum Verwalten eines Dienstes funktioniert, wenn die Komponente bereitgestellt wurde.

## <a name="connecting-via-the-portal"></a>Verbindung über das portal
Das Portal ist das Hauptverfahren, das häufigsten Azure Regierung verbinden.  Anschließen, suchen Sie das Portal unter [https://portal.azure.us](https://portal.azure.us).  Die ältere Version der Azure-Portal erfolgt über [https://manage.windowsazure.us](https://manage.windowsazure.us).

Abonnements können für Ihr Konto durch Verbinden mit [https://account.windowsazure.us](https://account.windowsazure.us)erstellt werden.

## <a name="connecting-via-powershell"></a>Verbindung über PowerShell

Ob Sie Azure PowerShell verwenden, um große Abonnement über Skript oder Access Features verwalten, nicht derzeit in Azure-Portal Azure Regierung statt öffentlichen Azure herstellen müssen.  Wenn Sie PowerShell öffentlichen Azure verwendet haben, ist es meist identisch.  Die Regierung von Azure sind:

+ Verbinden Ihr Konto
+ Bereichsnamen

>[AZURE.NOTE] Wenn PowerShell noch nicht verwendet haben, lesen Sie die [Einführung in Azure PowerShell](../powershell-install-configure.md).

Beim Starten von PowerShell müssen Sie sagen Azure PowerShell Verbindung Azure Regierung einen Umgebungsparameter angeben.  Der Parameter wird sichergestellt, dass PowerShell mit den richtigen Endpunkten verbunden ist.  Die Auflistung von Endpunkten ist bei Ihrem Konto anmelden Verbindung bestimmt.  Verschiedene APIs erfordern unterschiedliche Versionen des Schalters Umgebung:

Verbindungstyp | Befehl
---|----
[Service-Management](https://msdn.microsoft.com/library/dn708504.aspx) -Befehle | `Add-AzureAccount -Environment AzureUSGovernment`
[Ressourcenmanagement](https://msdn.microsoft.com/library/mt125356.aspx) -Befehle | `Add-AzureRmAccount -EnvironmentName AzureUSGovernment`
[Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx) (Befehle) | `Connect-MsolService -AzureEnvironment UsGovernment`
[Azure Active Directory Befehl v2](https://msdn.microsoft.com/library/azure/mt757189.aspx) | `Connect-AzureAD -AzureEnvironmentName AzureUSGovernment`

Sie können auch verwenden den Schalter Umgebung ein Speicherkonto mit neu AzureStorageContext herstellen und AzureUSGovernment geben.

### <a name="determining-region"></a>Bereich bestimmen

Sobald Sie verbunden sind, ist ein weiterer Unterschied-Bereiche verwendet, um einen Dienst.  Jede Azure-Cloud hat unterschiedliche Regionen.  Sie können auf der Seite Verfügbarkeit angezeigt.  Normalerweise verwenden Sie den Bereich in den Position-Parameter für einen Befehl.

Gibt es einen Haken.  Azure Government Regionen müssen anders als allgemeinen Namen formatiert werden:

Allgemeiner name | Befehl
---|----
US Gov Virginia | USGov Virginia
US Gov Iowa | USGov Iowa

>[AZURE.NOTE] Kein Leerzeichen zwischen USA und Gov wird den Position-Parameter verwenden.

Wenn Sie jemals die verfügbaren Bereiche in Azure Government überprüfen möchten, können Sie führen Sie die folgenden Befehle und drucken Sie die aktuelle Liste:

    Get-AzureLocation

Wenn Sie in Azure verfügbaren Umgebungen neugierig sind, können Sie Folgendes ausführen:

    Get-AzureEnvironment

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Informationen suchen Sie Auschecken können:

+ [PowerShell Dokumente auf GitHub](https://github.com/Azure/azure-powershell)
+ [Eine schrittweise Anleitung zum Verbinden mit Ressourcenmanagement](https://blogs.msdn.microsoft.com/azuregov/2015/10/08/configuring-arm-on-azure-gc/)
+ [Azure PowerShell-Dokumentation auf MSDN](https://msdn.microsoft.com/library/mt619274.aspx)

Zusätzliche Informationen und Updates der [Microsoft Azure Regierung Blog abonnieren Sie] (https://blogs.msdn.microsoft.com/azuregov/)
