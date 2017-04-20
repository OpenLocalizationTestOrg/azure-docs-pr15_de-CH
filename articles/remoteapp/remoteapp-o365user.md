
<properties 
    pageTitle="Verwendung von Azure RemoteApp mit Office 365 Benutzerkonten | Microsoft Azure"
    description="Informationen Sie zum Verwenden von Azure RemoteApp mein Office 365 Benutzerkonten"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-azure-remoteapp-with-office-365-user-accounts"></a>Verwendung von Azure RemoteApp mit Office 365 Benutzerkonten

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Wenn Sie ein Office 365 Abonnement Sie Azure Active Directory verfügen, speichert Ihre Benutzernamen und Kennwörter auf Office 365-Diensten verwendet. Z. B. wenn der Benutzer Office 365 ProPlus aktivieren Authentifizierung sie Azure AD-Lizenzen überprüfen. Die meisten Kunden möchten dasselbe Verzeichnis Azure RemoteApp verwendet.

Bei der Bereitstellung von Azure RemoteApp sind wahrscheinlich Azure-Abonnement verwenden Sie eine andere Azure Anzeige zugeordnet. Ihr Office 365-Verzeichnis verwenden möchten, müssen Sie das Azure-Abonnement in das betreffende Verzeichnis verschieben.

Informationen zum Bereitstellen von Office 365-Clientanwendungen finden Sie unter [Verwenden der Office 365-Abonnement Azure RemoteApp](remoteapp-officesubscription.md).
 
## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>Phase 1: Registrieren Sie Ihr kostenlose Abonnement für Office 365 Azure Active Directory
Bei Verwendung das klassische Azure-Portal gehen Sie in [Active Directory Azure-Testversion registrieren](https://technet.microsoft.com/library/dn832618.aspx) administrativen Zugriff auf Ihre Azure AD über Azure-Verwaltungsportal. Als Ergebnis dieses Prozesses werden Azure-Portal anmelden und sehen – das Verzeichnis zu diesem Zeitpunkt wird nicht viel seit Azure RemoteApp verwendeten vollständige Azure-Abonnement in einem anderen Verzeichnis angezeigt.

Name und Kennwort des Administratorkontos in diesem Schritt erstellten Bedenken sie in Phase 2 erfolgen.

Wenn Sie Azure-Portal verwenden, lesen Sie [registrieren und eine kostenlose Azure Active Directory mit Office 365-Portal aktivieren](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).

## <a name="phase-2-change-the-azure-ad-associated-with-your-azure-subscription"></a>Phase 2: Ändern Sie das Azure AD Azure-Abonnement zugeordnet.
Wir werden Ihre Azure-Abonnement aus ihrem aktuellen Verzeichnis in Office 365-Verzeichnis ändern wir in Phase 1 gearbeitet.

Anweisungen Sie Änderung [in Azure RemoteApp Pächter Azure Active Directory](remoteapp-changetenant.md)beschrieben. Insbesondere die folgenden Schritte:

- Schritt #1: Sollten Sie Azure RemoteApp (ARA) in diesem Abonnement bereitgestellt haben, alle Azure AD-Benutzerkonten von ARA Sammlungen zunächst entfernen vor allem. Alternativ können Sie prüfen, löschen alle vorhandenen Sammlungen.
- Schritt #2: Dies ist ein wichtiger Schritt. Sie benötigen ein Microsoft-Konto (z. B. @outlook.com) als Dienstadministrator Abonnements; deshalb, weil wir alle Benutzerkonten aus der vorhandenen Azure zugeordnete Dauerauftragsgruppe – haben kann Wenn Ja, wir werden in verschiedenen Azure AD verschieben.
- Schritt #4: Wenn Sie ein Verzeichnis hinzufügen, werden das System für das Verzeichnis mit dem Administratorkonto anmelden aufgefordert. Stellen Sie sicher das Administratorkonto aus Phase 1.
- Schritt #5: Ändern Sie das übergeordnete Verzeichnis des Abonnements in Ihr Office 365-Verzeichnis. Das Endergebnis sollte, unter Einstellungen -> Abonnements Ihres Abonnements Office 365-Verzeichnis enthält. 
![Ändern Sie das übergeordnete Verzeichnis des Abonnements](./media/remoteapp-o365user/settings.png)
 

Ist Ihr Abonnement von Azure RemoteApp Ihre Office 365 Azure AD zugeordnet; Sie können vorhandenen Office 365 Benutzerkonten Azure RemoteApp!




