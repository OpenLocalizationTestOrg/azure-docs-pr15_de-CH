<properties
   pageTitle="Verwenden Windows-Client-Images für Test-/Szenarien | Microsoft Azure"
   description="Wie Visual Studio Abonnementvorteile zum Bereitstellen von Windows 7/8/10 in Azure für Test-/Szenarien"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="iainfou"/>

# <a name="using-windows-client-in-azure-for-devtest-scenarios"></a>Verwenden von Windows-Client in Azure für Test-/Szenarien

Können Sie Windows 7, Windows 8 oder Windows Azure für Test-/Szenarien 10, sofern ein entsprechenden Visual Studio (ehemals MSDN) Abonnement verfügen. Dieser Artikel beschreibt die Teilnahmebedingungen für Azure und Azure Galeriebilder ausgeführte Windows-Client.


## <a name="subscription-eligibility"></a>Abonnement-Berechtigung
Aktive Visual Studio-Abonnenten (Personen, die eine Visual Studio-Abonnementlizenz erworben haben) können Windows-Client für Entwicklungs- und Testzwecke. Hardware und Azure virtuellen Maschinen in einem Azure-Abonnement kann Windows-Client verwendet werden. Windows-Client möglicherweise nicht werden bereitgestellt für normale Azure verwendet oder von Personen, die nicht aktive Visual Studio-Abonnenten verwendet.

Der Einfachheit halber haben wir Windows 10 Bilder verfügbar Azure Gallery in [Anspruch Test-/bietet](#eligible-offers). Visual Studio-Abonnenten in einem Typ Angebot können auch [angemessen vorbereiten und erstellen](virtual-machines-windows-prepare-for-upload-vhd-image.md) einer 64-Bit-Windows 7, Windows 8 oder Windows 10 Bild und [Azure hochladen](virtual-machines-windows-upload-image.md). Die Verwendung ist auf Test-/aktive Visual Studio-Abonnenten.


## <a name="eligible-offers"></a>Berechtigte Angebote
Die folgende Tabelle enthält Angebot IDs 10 Windows Azure-Katalog bereitstellen können. Windows 10 Bilder sind nur für Angebote. Visual Studio-Abonnenten Windows-Client in ein anderes Angebot müssen müssen Sie [angemessen vorbereiten und erstellen](virtual-machines-windows-prepare-for-upload-vhd-image.md) einer 64-Bit-Windows 7, Windows 8 oder Windows 10 Bild und [Azure hochladen](virtual-machines-windows-upload-image.md).

| Name des Angebotes | Angebotsnummer | Verfügbare Clientabbilder |
|:-----------|:------------:|:-----------------------:|
| [Nutzungsbasierte Test-](https://azure.microsoft.com/offers/ms-azr-0023p/)                          | 0023P | Windows 10 |
| [Visual Studio Enterprise (MPN)-Abonnenten](https://azure.microsoft.com/offers/ms-azr-0029p/)      | 0029P | Windows 10 |
| [Visual Studio Professional-Abonnenten](https://azure.microsoft.com/offers/ms-azr-0059p/)          | 0059P | Windows 10 |
| [Visual Studio Test Professional-Abonnenten](https://azure.microsoft.com/offers/ms-azr-0060p/)     | 0060P | Windows 10 |
| [Visual Studio Premium mit MSDN (Gewinn)](https://azure.microsoft.com/offers/ms-azr-0061p/)       | 0061P | Windows 10 |
| [Visual Studio Enterprise-Abonnenten](https://azure.microsoft.com/offers/ms-azr-0063p/)            | 0063P | Windows 10 |
| [Visual Studio Enterprise (BizSpark)-Abonnenten](https://azure.microsoft.com/offers/ms-azr-0064p/) | 0064P | Windows 10 |
| [Enterprise Test-](https://azure.microsoft.com/ofers/ms-azr-0148p/)                              | 0148P | Windows 10 |


## <a name="check-your-azure-subscription"></a>Überprüfen Sie Ihre Azure-Abonnement
Wenn Sie Ihr Angebot-ID nicht kennen, können Sie es Azure-Portal oder das Portal Konto abrufen.

Die Abonnement-Angebot-ID wird auf "Abonnements" Blade in Azure-Portal verzeichnet:

![Einzelheiten von Azure-Portal-ID](./media/virtual-machines-windows-client-images/offer_id_azure_portal.png) 

Sie können auch die Kennung auf der [Registerkarte "Abonnements"](http://account.windowsazure.com/Subscriptions) des Portals Azure-Konto anzeigen:

![Einzelheiten über das Portal Azure-Konto-ID](./media/virtual-machines-windows-client-images/offer_id_azure_account_portal.png) 


## <a name="next-steps"></a>Nächste Schritte
Sie können jetzt Ihre VMs mit [PowerShell](virtual-machines-windows-ps-create.md), [Ressourcenmanager Vorlagen](virtual-machines-windows-ps-template.md)oder [Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)bereitstellen.
