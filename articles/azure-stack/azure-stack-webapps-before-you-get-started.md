<properties
    pageTitle="Azure Stapel App Service Technical Preview 1 zunächst | Microsoft Azure"
    description="Schritte vor der Bereitstellung von Web Apps Azure Stapel"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="before-you-get-started-with-azure-stack-web-apps"></a>Vor dem Einstieg in Azure Stapel Web Apps

> [AZURE.NOTE] Folgendes gilt nur für Azure Stapel TP1 Bereitstellungen.

Sie benötigen einige Elemente Azure Stapel Web Apps installieren:

- Eine abgeschlossene Bereitstellung von [Azure Stapel Technical Preview 1](azure-stack-run-powershell-script.md)
- Genügend Speicherplatz in Ihrem System Azure Stapel für eine kleine Bereitstellung von Azure Stapel Web Apps.  Der erforderliche Speicherplatz ist ungefähr 20GB Speicherplatz.
- [Ein Server, auf dem SQL Server ausgeführt wird](#SQL-Server).

>[AZURE.NOTE] Die folgenden Schritte alle auf Client-VM.

## <a name="before-you-deploy-azure-stack-web-apps"></a>Vor der Bereitstellung von Azure Stapel Web Apps

Um ein Ressourcenanbieter bereitzustellen, müssen Sie die PowerShell Integrated Scripting Environment(ISE) als Administrator ausführen. Aus diesem Grund müssen Sie Cookies und JavaScript in Internet Explorer Profil ermöglichen, mit dem Active Directory Azure anmelden.

## <a name="turn-off-internet-explorer-enhanced-security"></a>Deaktivieren Sie die Internet Explorer enhanced security

1.  Azure Stack Proof of Concept (POC) Computer als **AzureStack-Administrator**melden Sie an, und öffnen Sie **Server-Manager**.
2.  **Verstärkte Sicherheitskonfiguration für Internet Explorer** für Administratoren und Benutzer deaktivieren.
3.  ClientVM.AzureStack.local virtuellen Computer als Administrator melden Sie an und öffnen Sie **Server-Manager**.
4.  **Verstärkte Sicherheitskonfiguration für Internet Explorer** für Administratoren und Benutzer deaktivieren.

## <a name="enable-cookies"></a>Cookies aktivieren

1.  Wählen Sie **Start**> **Alle apps**> **Windows-Zubehör**. Maustaste **Internet Explorer** > **Weitere** > **als Administrator ausführen**.
2.  Wenn Sie aufgefordert werden, wählen Sie **Security verwenden**und klicken Sie auf **OK**.
3.  Wählen Sie in Internet Explorer **Extras** (Zahnradsymbol) > **Internetoptionen** > **Privacy** > **Erweitert**.
4.  Klicken Sie auf **Erweitert**. Stellen Sie sicher, dass beide Kontrollkästchen **akzeptieren** ausgewählt sind, und wählen **OK** und **OK** wieder.
5.  Schließen Sie Internet Explorer und starten Sie PowerShell ISE als Administrator.

## <a name="install-the-latest-version-of-azure-powershell"></a>Installieren Sie die neueste Version von Azure PowerShell

1.  Melden Sie sich bei Azure Stapel POC-Computer als **AzureStack-Administrator**an.
2.  Verwenden Sie die Remotedesktopverbindung ClientVM.AzureStack.local virtuellen Computer als Administrator anmelden.
3.  **Bedienfeld** und wählen Sie **Programm deinstallieren**. 
4.  Mit der rechten Maustaste auf **Microsoft Azure PowerShell - November 2015** und wählen Sie **Deinstallieren**.
5.  Nachdem die Deinstallation abgeschlossen ist, starten Sie den virtuellen Computer ClientVM.AzureStack.local
6.  Downloaden Sie und installieren Sie der neuesten [Azure PowerShell](http://aka.ms/azstackpsh).


## <a name="sql-server"></a>SQL Server

Das Installationsprogramm Azure Stapel Web Apps Wert für SQL Server, mit dem Ressourcenanbieter Azure Stack SQL installiert ist. Bei der Installation der SQL Server-Ressourcenanbieter (SQL RP) sicher, dass Sie den Datenbank-Benutzernamen und Kennwort beachten. Sie benötigen beide, wenn Azure Stapel Web Apps installieren.
Hinweis: Sie können auch können bereitstellen und einen anderen Server zum Ausführen von SQL Server verwenden. Sie können die SQL Server-Instanz, Abschluss Optionen in Azure Stapel Web Apps Installer.
