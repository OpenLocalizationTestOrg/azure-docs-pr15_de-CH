<properties services="virtual-machines" title="Using Azure CLI with Azure Resource Manager" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli-with-azure-resource-manager-arm"></a>Mithilfe von Azure CLI mit Azure Resource Manager (ARM)

Bevor Sie Azure-CLI mit Ressourcen-Manager-Befehlen und Vorlagen Azure Ressourcen und Ressourcengruppen-Arbeitslasten verwenden können, benötigen Sie ein Konto mit Azure (natürlich). Wenn Sie nicht über ein Konto verfügen, erhalten Sie eine [kostenlose Azure Testversion](https://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Wenn nicht bereits ein Azure-Konto ein Abonnement für MSDN-Abonnement verfügen, erhalten Sie kostenlose Azure Kredite aktivieren oder die [MSDN-Abonnementvorteile hier](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) kostenlose Konto verwenden können. Entweder funktionieren Azure auf.

### <a name="step-1-verify-the-azure-cli-version"></a>Schritt 1: Überprüfen der Azure-CLI-version

Um Azure-CLI für Befehle und ARM-Vorlagen verwenden, müssen Sie mindestens Version 0.8.17. Überprüfen Sie die Version geben `azure --version`. Sie sollten Folgendes sehen:

    $ azure --version
    0.8.17 (node: 0.10.25)

Möchten Sie Ihre Azure-CLI-Version zu aktualisieren, finden Sie unter [Azure-CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-verify-you-are-using-a-work-or-school-identity-with-azure"></a>Schritt 2: Überprüfen Sie eine Arbeit oder Schule mit Azure

Nur können den ARM-Befehlsmodus bei Verwendung einer [Azure Active Directory Mieter](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) oder einen [Service Principal Name](https://msdn.microsoft.com/library/azure/dn132633.aspx). (Diese werden auch *Organisationseinheiten Ids*bezeichnet.)

Dazu haben Sie eine Eingabe anmelden `azure login` und mit Ihrem Arbeit oder Schule Benutzernamen und Kennwort ein. Wenn Sie eine haben, sollte Folgendes angezeigt werden:

    $ azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@contoso.onmicrosoft.com
    Password: *********
  	|info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription Visual Studio Ultimate with MSDN as default
    info:    Added subscription Azure Free Trial
    +
    info:    login command OK

Wenn Sie nicht angezeigt werden, müssen Sie einen neuen Mandanten (oder Service principal) mit Ihrem Microsoft-Konto erstellen. (Dies ist häufig bei persönlichen MSDN-Abonnements oder freien Testabonnements.) Erstellen einer Arbeitsaufgabe oder Schule Id der Azure-Konto mit Microsoft-Id erstellt, finden Sie unter [Zuordnen einer Azure AD-Verzeichnis mit neuen Azure-Abonnement](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). Wenn Sie, dass Sie bereits eine Organisations-Id haben glauben, müssen Sie mit der Person sprechen, die das Konto für Sie erstellt.

### <a name="step-3-choose-your-azure-subscription"></a>Schritt 3: Auswählen der Azure-Abonnement

Wenn Sie nur ein Abonnement in der Azure-Konto haben, ordnet Azure CLI selbst dieses Abonnement standardmäßig. Haben mehrere Abonnements müssen Sie das Abonnement auswählen eingeben möchten `azure account set <subscription id or name> true` _Abonnement-Id oder Name_ ist die Abonnement-Id oder den Namen, die in der aktuellen Sitzung verwenden möchten.

Folgendes sollte angezeigt werden:

    $ azure account set "Azure Free Trial" true
    info:    Executing command account set
    info:    Setting subscription to "Azure Free Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
    info:    Changes saved
    info:    account set command OK

### <a name="step-4-place-your-azure-cli-in-the-arm-mode"></a>Schritt 4: Stellen Sie Ihre Azure-CLI ARM-Modus

Um Azure Resource Management (ARM) Modus mit der Azure-CLI, geben `azure config mode arm`. Folgendes sollte angezeigt werden:

    $ azure config mode arm
    info:    New mode is arm

> [AZURE.NOTE] Sie können zurück zum Azure Service Befehle verwenden `azure config mode asm`.
