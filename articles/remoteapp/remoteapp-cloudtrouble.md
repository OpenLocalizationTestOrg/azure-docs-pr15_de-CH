
<properties
    pageTitle="Problembehandlung bei RemoteApp Cloud Sammlungen - Erstellung | Microsoft Azure"
    description="Erfahren Sie, wie RemoteApp Cloud Auflistung erstellen Fehler beheben"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Problembehandlung beim Erstellen von RemoteApp Cloud-Sammlungen

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Wenn Cloud Sammlung Probleme auftreten, lesen Sie die folgenden Informationen.

## <a name="your-image-is-invalid"></a>Das Bild ist ungültig ##
Wenn eine Meldung wie "GoldImageInvalid" beim Warten Azure Bereitstellung Ihrer Sammlung, bedeutet dies, dass Ihre Vorlagenbild [Bild Anforderungen](remoteapp-imagereqs.md)entspricht. Also diese [Vorschriften](remoteapp-imagereqs.md)lesen, Ihr Bild korrigieren und versuchen, Ihre Sammlung zu erstellen.

## <a name="common-errors-seen-in-the-azure-management-portal"></a>Allgemeine Fehler im Azure-Verwaltungsportal

    DNS server could not be reached
    ProvisioningTimeout

Cloud-Sammlungen häufig Fehler während der Erstellung durch Sie benutzerdefinierte Bilder verwenden.  Wenn eine der oben aufgeführten Fehler und verwenden Sie ein benutzerdefiniertes Bild die Auflistung, überprüfen Sie Folgendes:

- Sicherstellen Sie, dass das benutzerdefinierte Bild hochgeladenen Bild erfüllt.
- Häufige Problem ist meist, dass das Bild nicht korrekt Syspreped.  
- Überprüfen Sie das Bild kann in Hyper-V zu starten oder eine VM IAAS direkt in Azure Abonnements mit dem Bild erstellen. Die VM nicht starten und nicht starten, gibt dies normalerweise, dass das benutzerdefinierte Abbild nicht ordnungsgemäß vorbereitet wurde.  Überprüfen des Abbilds erstellt wurde wie folgt zum Erstellen einer benutzerdefinierten Vorlagenbild für RemoteApp

Verwenden Sie eines der Microsoft-Bilder in Ihrem Abonnement enthalten sind, versuchen Sie, die Auflistung zu erstellen. Wenn das Problem weiterhin besteht dann wenden Sie sich an Microsoft Support Services.

    PlatformImageTrialModeOnly

Wenn diese Fehlermeldung bedeutet dies normalerweise, der zu einem gebührenpflichtigen Konto aktualisiert wurde, aber Sie möchten Microsoft bereitgestellten Abbild verwenden, während der Testmodus des Dienstes gültig ist. In diesem Fall versucht der cloudsammlung erneut erstellen, aber das richtige Bild angeben.
