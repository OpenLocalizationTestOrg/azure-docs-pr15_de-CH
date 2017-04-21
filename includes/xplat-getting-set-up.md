<properties services="virtual-machines" title="Setting up Azure CLI for service management" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli"></a>Mithilfe von Azure CLI

Die folgenden Schritte unterstützen Sie Azure CLI einfach die neueste Version und das richtige Abonnement verwenden. Zu Azure CLI zuerst zu Ihrem Konto herstellen, finden Sie unter [Azure-Befehlszeilenschnittstelle (CLI Azure)](xplat-cli-install.md).

### <a name="step-1-update-azure-cli-version"></a>Schritt 1: Update Azure CLI-version

Um Azure-CLI für Befehle mit Service-Modus verwenden, sollten Sie ggf. eine neuere Version verfügen. Überprüfen Sie die Version geben `azure --version`. Sie sollten Folgendes sehen:

    $ azure --version
    0.8.17 (node: 0.10.25)

Wenn Sie Ihre Azure-CLI-Version aktualisieren möchten, finden Sie unter [Azure-CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-set-the-azure-account-and-subscription"></a>Schritt 2: Festlegen der Azure-Konto und Ihr Abonnement

Nachdem Sie Ihre Azure-CLI mit dem Konto, die Sie verwenden möchten verbunden haben, müssen Sie mehrere Abonnements. Fall Sie sollten den verfügbaren Abonnements für Ihr Konto eingeben `azure account list`, und wählen Sie den Dauerauftrag eingeben möchten `azure account set <subscription id or name> true` _Abonnement-Id oder Name_ ist die Abonnement-Id oder den Namen, die in der aktuellen Sitzung verwenden möchten. Folgendes sollte angezeigt werden:

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [AZURE.NOTE] Wenn nicht bereits ein Azure-Konto ein Abonnement für MSDN-Abonnement verfügen, erhalten Sie kostenlose Azure Kredite aktivieren oder die [MSDN-Abonnementvorteile hier](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) kostenlose Konto verwenden können. Entweder funktionieren Azure auf.
