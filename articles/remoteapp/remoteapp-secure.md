
<properties
    pageTitle="Sichern von apps und Ressourcen in Azure RemoteApp | Microsoft Azure"
    description="Informationen Sie zum Sperren von apps und Ressourcen in Azure RemoteApp"
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



# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Sichere apps und Ressourcen in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Azure RemoteApp bietet Benutzern Zugriff auf zentral verwaltete Windows-apps können Sie steuern, welche Inhalte der Benutzer nicht.  Ist besonders nützlich, wenn der Benutzer von einem nicht verwalteten Gerät (wie ihre persönliche Macbook) anschließen und den Benutzerzugriff steuern oder

Beispielsweise wenn Sie Active Directory für die Authentifizierung verwenden und verhindern, dass Benutzer Daten aus einer Anwendung kopiert werden soll, können Sie Remote Desktop Gruppenrichtlinien Block Benutzer Daten kopieren.

Ein weiteres Beispiel ist Internet Sperren für eine bestimmte Anwendung in der Auflistung. Sie können eine Windows-Firewall-Regel erstellen, die den Zugriff blockiert, wenn für die Auflistung erstellen.

## <a name="implementation-options"></a>Implementierungsoptionen

  Hier sind die Schlüssel Implementierungsoptionen, die einzeln oder zusammen verwendet werden können, nach Bedarf:

1.  Wenn Ihre Auflistung RemoteApp Domäne beigetreten ist können Sie eine [Gruppenrichtlinie](https://technet.microsoft.com/library/cc725828.aspx) (mit Ausnahme der Leerlauf trennen Timeout Richtlinien beschrieben und [hier](../azure-subscription-service-limits.md)) erzwingen.
2.  Als Alternative zur Gruppenrichtlinie (sofern die Auflistung nicht Domäne beitreten oder Sie die richtigen Berechtigungen in Active Directory haben) können Sie in Ihrem Vorlagenbild [Lokale Richtlinien](https://technet.microsoft.com/library/cc775702.aspx) konfigurieren.  Hinweis: dieser Gruppe Trumpf lokale Richtlinien Richtlinien, wenn ein Konflikt vorliegt.
3.  Einige BS/Anwendung Einstellungen können können nicht über Gruppenrichtlinien konfiguriert, jedoch über Registrierungsschlüssel mit dem [Programm "RegEdit"](./remoteapp-hybridtrouble.md) beim Konfigurieren der Vorlagenbild.
4.  [Windows-Firewall](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) können Sie steuern den Netzwerkzugriff in und aus dem Computer, auf dem die Anwendung ausgeführt wird. Denken Sie aber daran, URLs und hier definierten Ports blockiert nicht.
5.  [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) können Steuerelement Programme und Dateien, die Benutzer ausführen können. Beispielsweise können Benutzer herausfinden, wie nicht veröffentlichte aber gibt die Auflistung verwendeten Bild - AppLocker Blockieren dieser Anwendung führen.

## <a name="detailed-information"></a>Ausführliche Informationen

- Die folgenden Richtlinien für RDS sind besonders hilfreich sein:
    - [Geräte und Ressourcenumleitung](https://technet.microsoft.com/library/ee791794.aspx)
    - [Clientdruckerumleitung](https://technet.microsoft.com/library/ee791784.aspx)
    - [Profile](https://technet.microsoft.com/library/ee791865.aspx).
- Beachten Sie, dass die Umleitung über RemoteApp PowerShell konfigurieren (wie gesehen [hier](./remoteapp-redirection.md)) basieren auf dem Client-Computer die Richtlinie durchgesetzt, wenn Sicherheit Hauptziel ist Sie wollen die Vorlage Bild lokale Richtlinie oder über Gruppenrichtlinien erzwingen.
- [Windows Server 2012 R2 Richtlinien](https://technet.microsoft.com/library/hh831791.aspx).
- [Office 2013-Richtlinien](https://technet.microsoft.com/library/cc178969.aspx) (wie [Anpassen die Office-Symbolleiste](https://technet.microsoft.com/library/cc179143.aspx)).
