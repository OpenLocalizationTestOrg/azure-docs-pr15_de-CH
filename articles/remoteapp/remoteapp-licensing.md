<properties
    pageTitle="Azure RemoteApp Lizenzierung | Microsoft Azure"
    description="Lernen Sie die Funktionsweise der Lizenzierung in Azure RemoteApp."
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


# <a name="how-does-licensing-work-in-azure-remoteapp"></a>Funktionsweise Lizenzierung in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Sie haben Ihren Azure RemoteApp-Dienst Ihre Vorlagen einrichten und apps für Ihre Benutzer veröffentlichen. Aber es noch eine letzte Sache gibt herauszufinden: Lizenzierung. Nur funktioniert wie Lizenzierung für RemoteApp und apps über RemoteApp Teilen?

RemoteApp ist kein Windows-Lizenzen oder Remote Desktop Clientzugriffslizenzen erforderlich. Ihr Abonnement übernimmt der RemoteApp-Seite selbst. (Überprüfen Sie die Details der [Preisübersicht](https://azure.microsoft.com/pricing/details/remoteapp).)

Verwenden Sie eines der Bilder, die in Ihrem Abonnement enthalten ist, können Sie auf das Bild ohne eine separate Lizenz installiert Apps freigeben. Wenn Sie Windows Server 2012 R2 Vorlagenbild Auflistung zu verwenden, können Sie System Center Endpoint Protection für Benutzer freigeben. Die einzige Ausnahme von dieser Regel sind Office 365 ProPlus, erfordert ein separates Abonnement und Office 2013 in Produktion Auflistung geteilt werden können.

Ggf. das Vorlagenbild Office 365 verwenden RemoteApp müssen Sie einen *vorhandenen* Plan Office 365 ProPlus. Das gleiche gilt für alle Office 365-Anwendung, die mithilfe einer benutzerdefinierten Vorlage veröffentlichen. Sie müssen apps mit eigenes Abonnement aktivieren. Dies gilt für Test- und bezahlte Abonnements. Wenn Sie während des Versuchs, *und Sie haben bereits ein Abonnement*, das Vorlagenbild Office 365 verwenden gehen Sie zur Seite Office 365 für ein Testabonnement [Anmelden](https://go.microsoft.com/fwlink/p/?LinkID=403802) . Siehe [wie RemoteApp und Office zusammenarbeiten?](remoteapp-o365.md) Weitere Informationen.

Wenn während des Testzeitraums möchten ein Office 365-Testabonnement, verwenden Office 2013 Professional Plus Vorlage Bild kommt mit RemoteApp. Diese Vorlagenbild kann nur für 30 Tage verwendet werden und nicht in eine bezahlte Auflistung konvertiert werden.

Für andere apps müssen Sie sicherstellen, dass die Lizenz die app freigeben können.

Das macht Sinn, richtig? Sie können jede Anwendung veröffentlichen, die Sie berechtigt freigeben. Und um sicherzustellen, dass Sie wirklich berechtigt sind, Ihre Programme freigeben müssen.

Bitte beachten Sie, dass CAL oder Volume License Agreement in einer Cloud-Auflistung verwendet werden kann. Sie *können* verwenden ein Volumenlizenzvertrags Applications in der Hybrid-Auflistung (außer Office). Sie müssen nur auf Ihr Vorlagenbild die Volumenlizenzmedien installieren. Folgen Sie dem vom Hersteller Anwendung in einer Umgebung mit Remotedesktop Lizenzen installieren.
