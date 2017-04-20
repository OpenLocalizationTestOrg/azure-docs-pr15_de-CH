<properties
   pageTitle="Einrichten von PowerShell erstellen einen virtuellen Computer für den Markt | Microsoft Azure"
   description="Anleitung zum Einrichten von Azure PowerShell und als ein optionaler Prozess übertragen VM Bilder bereitstellen, und bei Azure Marketplace verkaufen"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/04/2016"
   ms.author="hascipio"/>

# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Erstellen Sie ein Angebot für den Azure Azure PowerShell eingerichtet
Ausführliche Informationen zum Einrichten von PowerShell in Azure finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Ein einfacher Ansatz ist die Zertifikat-Methode verwenden, die heruntergeladen und importiert ein Zertifikat für die Authentifizierung. Um das erforderliche Zertifikat zu erhalten, verwenden Sie das Cmdlet " **Get-AzurePublishSettingsFile** ". Speichern Sie die Datei, wenn Sie dazu aufgefordert werden. Verwenden Sie das **Import-AzurePublishSettingsFile** -Cmdlet zum Importieren des Zertifikats in PowerShell-Sitzung.

Konfigurieren und allgemeinen Microsoft Azure-abonnementeinstellungen der PowerShell-Sitzung speichern verwenden Sie die **Set AzureSubscription** und **Wählen Sie AzureSubscription** -Cmdlets:

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

Der erste Befehl ordnet ein standardspeicherkonto Abonnement (erforderlich für einige VM-Bereitstellung-Vorgänge).  Das zweite ist dem Abonnement der aktuellen (von anderen Cmdlets erkannt).

## <a name="see-also"></a>Siehe auch
- [Erste Schritte: Angebot Azure Marketplace veröffentlichen](marketplace-publishing-getting-started.md)
- [Abbild eines virtuellen Computers erstellen für den Markt](marketplace-publishing-vm-image-creation.md)
