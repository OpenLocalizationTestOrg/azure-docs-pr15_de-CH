<properties
    pageTitle="Erstellen eines Abbildes Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie mehr über die verfügbaren Optionen zum Erstellen von Bildern für Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="create-an-azure-remoteapp-image"></a>Erstellen eines Abbildes Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Azure RemoteApp verwendet Bilder für apps, die für die Benutzer freigeben. (Wir Ihr Bild und erstellen VMs - das den Benutzerzugriff in Azure RemoteApp Anmeldung). Erstellen eine Azure RemoteApp-Auflistung mit Clientanwendungen Cloud oder Hybrid, zunächst erstellen Sie ein Bild mit dieser Programme. Erstellen einer Auflistung, die das Abbild verwendet, Zuweisen von Benutzern zu der Auflistung und apps Benutzer veröffentlichen.

Sie haben mehrere Optionen zum Erstellen oder Bilder. [Anforderung](remoteapp-imagereqs.md) für ein Bild ist, dass sie Windows Server 2012 R2 ausführen und die Remote Desktop Session Host (RDSH) installiert. Wie erhalten, dass werden es interessant.

Bei Bildern stehen Ihnen die folgenden Optionen:

- Sie können importieren und einem [Bild basierend auf Azure virtuellen Computer](remoteapp-image-on-azurevm.md)verwenden. Das ist gut für Branchen apps, die benutzerdefinierte Einstellung. Sie können das Bild der app arbeiten.
- Sie können [Erstellen und ein benutzerdefiniertes Bild hochladen](remoteapp-create-custom-image.md). Dies ist nützlich, wenn Sie bereits ein Bild haben, für die lokale Remote Desktop Services-Bereitstellung verwenden.
- Sie können eine [Vorlage Bilder](remoteapp-images.md) in Ihrem Abonnement RemoteApp verwenden. Diese Bilder werden erstellt und RemoteApp-Team verwaltet und enthalten einige standard Programme (wie die Office-Suite), die Sie den Benutzern zur Verfügung stellen können. Beachten Sie, dass nur das Office 365 Pro Plus Bild in einer Produktion verwendet werden kann.

Unabhängig erhalten Sie das Bild wie erstellen sollten Sie sicherstellen, dass die [app-Anforderungen](remoteapp-appreqs.md) , sicherzustellen, dass Ihre app in RemoteApp verstehen. Schließlich besteht der nächste Schritt zum Erstellen einer [Cloud](remoteapp-create-cloud-deployment.md) oder [Hybrid](remoteapp-create-hybrid-deployment.md) -Auflistung.
