<properties services="virtual-machines" title="Setting up PowerShell" authors="JoeDavies-MSFT" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm=""
   ms.workload="infrastructure"
   ms.date="05/12/2015"
   ms.author="rasquill" />

## <a name="setting-up-powershell"></a>Einrichten von PowerShell

Gehen Sie folgendermaßen vor, bevor Sie Azure PowerShell verwenden können.

### <a name="verify-powershell-versions"></a>PowerShell Versionen überprüfen

Bevor Sie Windows PowerShell verwenden können, müssen Sie Windows PowerShell, Version 3.0 oder 4.0. Zum Ermitteln der Version von Windows PowerShell Geben Sie diesen Befehl in einer Windows PowerShell-Befehlszeile.

    $PSVersionTable

Folgendes sollte angezeigt werden.

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2

Stellen Sie sicher, dass der Wert von **PSVersion** 3.0 oder 4.0. Eine kompatible Version finden Sie unter [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Sie müssen auch Azure PowerShell Version 0.8.0 oder höher. Die Version von Azure PowerShell finden, die Sie mit diesem Befehl an der Befehlszeile Azure PowerShell installiert haben.

    Get-Module azure | format-table version

Folgendes sollte angezeigt werden.

    Version
    -------
    0.8.16.1

Informationen und einen Link auf die neueste Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](powershell-install-configure.md).


### <a name="set-your-azure-account-and-subscription"></a>Legen Sie Ihre Azure-Konto und Ihr Abonnement

Wenn Sie bereits ein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnementvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder Zeichen, für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)aktivieren.

Öffnen Sie eine Befehlszeile Azure PowerShell und Azure anmelden mit diesem Befehl.

    Add-AzureAccount

Haben Sie mehrere Azure-Abonnements können Sie Ihre Azure-Abonnements mit diesem Befehl auflisten.

    Get-AzureSubscription

Den folgenden Informationstyp erhalten:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName : 
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Sie können Azure Abonnement Ausführen dieser Befehle an der Befehlszeile Azure PowerShell festlegen. Alles innerhalb der Anführungszeichen, einschließlich der Zeichen mit dem richtigen Namen < und >.

    $subscr="<SubscriptionName from the display of Get-AzureSubscription>"
    Select-AzureSubscription -SubscriptionName $subscr -Current 

Weitere Informationen über Konten und Azure-Abonnements finden Sie [wie: Verbinden mit Ihrem Abonnement](powershell-install-configure.md#Connect).
