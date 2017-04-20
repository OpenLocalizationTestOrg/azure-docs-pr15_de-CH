<properties
    pageTitle="Neuigkeiten in Azure RemoteApp Vorlage Bilder | Microsoft Azure"
    description="Erfahren Sie mehr über die Vorlage Bilder Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Neuigkeiten in Azure RemoteApp Vorlage Bilder

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Ihr Abonnement Azure RemoteApp umfasst drei Vorlage Bilder:


- Windows Server 2012
- Microsoft Office 365 ProPlus (Office 365-Abonnement erforderlich)
- Microsoft Office 2013 Professional Plus (nur Testversion)

> [AZURE.IMPORTANT]Ihr Abonnement Azure RemoteApp gewährt Zugriff auf die Software in Bildern mit Ausnahme von Office 365 ProPlus, erfordert ein separates Abonnement und Office 2013 in der Produktion verwendet werden kann. Dies bedeutet, dass Benutzer Programme oder apps Vorlage Bilder freigeben können. Wenn Sie eine Auflistung, die Windows Server 2012 R2-Images verwendet erstellen, können Sie System Center Endpoint Protection für Zugriff auf über RemoteApp veröffentlichen.
>
> [RemoteApp Lizenzierung Details](remoteapp-licensing.md) für Weitere Informationen überprüfen. Und [Mithilfe von Office Azure RemoteApp](remoteapp-o365.md) für die Office-Lizenzierung Info.

Weiter Informationen zum Inhalt jedes Bild.

## <a name="windows-server-2012-r2--the-vanilla-image"></a>Windows Server 2012 R2 ("die herkömmlichen Image")
Dieses Bild basiert auf Microsoft Windows Server 2012 R2 Datacenter und hat die folgenden installierten Serverrollen und Features Anforderungen für Azure RemoteApp Vorlage Bilder:


- .NET Framework 4.5 3.5.1, 3.5
- Desktop Experience
- Freihand- und Handschriftdienste
- Media Foundation
- Remote Desktop Session Host
- Windows PowerShell 4.0
- Windows PowerShell ISE
- WoW64-Unterstützung

Dieses Bild hat auch die folgenden Programme installiert:

- Adobe Flash Player
- Microsoft Silverlight
- Microsoft System Center 2012 Endpoint Protection
- Microsoft Windows Media Player


## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (Abonnement erforderlich)
Office 365 ist die am häufigsten angeforderten Anwendung so haben wir "Eigene" für Sie arbeiten.

Dieses Bild ist eine Erweiterung des herkömmlichen Bild und besteht aus folgenden Komponenten von Microsoft Office 365 ProPlus-neben Windows Server 2012 R2 Bild beschriebenen Komponenten installiert:


- Zugriff
- Excel
- Lync
- OneNote
- OneDrive für Unternehmen (Beachten Sie der Sync-Agent für die Verwendung mit Azure RemoteApp nicht unterstützt)
- Outlook
- PowerPoint
- Word
- Microsoft Office Proofing Tools

Das Bild auch Visio Pro und Pro Projekt.

Und die folgenden Programme sowie:

- SQL Native client
- ODBC-Treiber
- SQL Server Data Mining-client
- MasterDataServices Kunden
- Microsoft Publisher
- PowerQuery
- PowerMap


Volle Funktionalität von Office 365 ProPlus apps steht nur für Benutzer mit Office 365 ProPlus Plan. Weitere Informationen zu Office 365 finden Sie unter Abonnement [Servicepläne für Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx). Haben Sie noch Fragen? [Office 365 + RemoteApp](remoteapp-o365.md) -Informationen anzeigen Überprüfen Sie außerdem die neuen Artikel [wie Ihr Office 365-Abonnement Azure RemoteApp](remoteapp-officesubscription.md).

Beachten Sie, dass Sie Office 365 ProPlus Visio Pro und Project Pro-Lizenz haben jeweils eigene Lizenz.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (nur Testversion)
Während der Testphase können Sie den Dienst mit der Office 2013 testen.

Dieses Bild ist eine Erweiterung des herkömmlichen Bild und besteht aus folgenden Komponenten von Microsoft Office 2013 Professional Plus zusätzlich in Windows Server 2012 R2-Images beschriebenen Komponenten installiert:


- Zugriff
- Excel
- Lync
- OneNote
- OneDrive für Unternehmen (Beachten Sie der Sync-Agent für die Verwendung mit Azure RemoteApp nicht unterstützt)
- Outlook
- PowerPoint
- Projekt
- Visio
- Word
- Microsoft Office Proofing Tools

> [AZURE.IMPORTANT]**Rechtliche Hinweise:** Dieses Bild enthält kein Microsoft Office-Lizenz und *nicht für die Produktion verwendet werden*. Das Bild Testversion nur soll Office 2013 Professional Plus. Wenn Sie Office apps in Azure RemoteApp für die Produktion verwenden möchten, müssen Sie Office 365 ProPlus benutzen. Weitere Informationen zur Lizenzierung von Office finden Sie unter [Verwendung von Office 365 Azure RemoteApp](remoteapp-o365.md)
