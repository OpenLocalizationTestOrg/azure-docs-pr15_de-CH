<properties
    pageTitle="Get gestartet Azure MFA in die Cloud | Microsoft Azure"
    description="Dies ist die Seite Microsoft Azure mehrstufige Authentifizierung die Verwendung Einstieg in Azure MFA in der Cloud."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Erste Schritte mit Azure mehrstufige Authentifizierung in der cloud
Dieser Artikel geht wie den Einstieg Azure mehrstufige Authentifizierung in der Cloud.

> [AZURE.NOTE]  Die folgende Dokumentation enthält Informationen zum Benutzer mithilfe der **Azure-Verwaltungsportal**. Informationen zum Azure mehrstufige Authentifizierung für Benutzer von Office 365 einrichten suchen, finden Sie unter [mehrstufige Authentifizierung für Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![MFA in der Cloud](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisites"></a>Erforderliche Komponenten
Die folgenden Komponenten sind erforderlich, bevor Sie Azure mehrstufige Authentifizierung für Ihre Benutzer aktivieren können.


1. [Melden Sie sich für ein Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/) - Wenn Sie noch nicht über ein Azure-Abonnement verfügen, müssen Sie Anmeldung für eine. Wenn Sie nur ausgehend und Azure MFA können Sie ein Testabonnement
2. [Erstellen einer kombinierten Authentifizierungsanbieter](multi-factor-authentication-get-started-auth-provider.md) und weisen sie das Verzeichnis bzw. die [Lizenzen für Benutzer zuweisen](multi-factor-authentication-get-started-assign-licenses.md)

> [AZURE.NOTE]  Lizenzen werden Benutzer Azure MFA, Azure AD Premium oder Enterprise Mobility Suite (EMS).  MFA ist in Azure AD Premium und EMS enthalten. Wenn Sie über genügend Lizenzen verfügen, müssen Sie kein Authentifizierungsanbieter erstellen.


## <a name="turn-on-two-step-verification-for-users"></a>Zweistufige Überprüfung für Benutzer aktivieren
Dazu müssen zwei Start Überprüfung auf für einen Benutzer ändern Sie den Status des Benutzers deaktiviert bzw. aktiviert.  Weitere Informationen zu Benutzer finden Sie unter [User Zustände in Azure mehrstufige Authentifizierung](multi-factor-authentication-get-started-user-states.md)

Gehen Sie folgendermaßen vor, um MFA für Ihre Benutzer aktivieren.

### <a name="to-turn-on-multi-factor-authentication"></a>Mehrstufige Authentifizierung aktivieren

1.  Melden Sie sich als Administrator in [Azure-Verwaltungsportal](https://manage.windowsazure.com) an.
2.  Klicken Sie auf der linken Seite auf **Active Directory**.
3.  Wählen Sie unter Verzeichnis das Verzeichnis für den Benutzer aktivieren möchten.
![Klicken Sie auf Verzeichnis](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Klicken Sie oben auf **Benutzer**.
5.  Klicken Sie am unteren Rand der Seite auf **Verwaltung mehrstufige Authentifizierung**. Eine neue Registerkarte wird geöffnet.
![Klicken Sie auf Verzeichnis](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Suchen Sie den Benutzer, den zweistufigen Authentifizierung aktivieren möchten. Sie müssen die Ansicht oben ändern. Stellen Sie sicher, dass der Status **deaktiviert.** 
 ![Können](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Setzen Sie in das Feld neben dem Namen eine **Überprüfen** .
7.  Klicken Sie auf der rechten Seite auf **Aktivieren**.
![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Aktivieren Sie **mehrstufige Authentifizierung**.
![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Beachten Sie, dass der Benutzerstatus **deaktiviert** **aktiviert**wurde.
![Anwender](./media/multi-factor-authentication-get-started-cloud/user.png)

Nachdem Sie Ihre Benutzer aktiviert haben, sollten Sie sie per e-Mail benachrichtigen. Beim nächsten anmelden wollen werden sie aufgefordert, Firmen-zweistufigen Überprüfung registrieren. Nach der ersten zweistufigen Authentifizierung verwenden, müssen sie eine app-Kennwörter einrichten zu nicht-Browser-apps gesperrt wird.


## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>Mithilfe von PowerShell automatisiert zweistufigen Authentifizierung aktivieren

Ändern Sie den [Status](multi-factor-authentication-whats-next.md) mithilfe von [Azure AD PowerShell](../powershell-install-configure.md)können Sie folgende.  Sie können `$st.State` auf einen der folgenden Status:

- Aktiviert
- Erzwungen
- Deaktiviert  

> [AZURE.IMPORTANT]  Wir raten für Verschieben von Benutzern direkt aus den Aktivierungsstatus Zustand erzwungen. Nicht browserbasierte apps sind nicht mehr da der Benutzer nicht MFA-Registrierung durchlaufen und ein [app-Kennwort](multi-factor-authentication-whats-next.md#app-passwords)erhalten. Wenn Sie Browser basierende apps und app Kennwörter, empfehlen wir aktiviert einen deaktivierten Zustand gelangen. Dadurch können Benutzer registrieren und ihre app-Kennwörter. Danach können Sie auf erzwungen verschieben.

Mithilfe von PowerShell wäre eine Option für das Massenkopieren Anwender. Derzeit gibt es keine Bulk-Feature aktivieren im Azure-Portal und müssen Sie jeden Benutzer einzeln auszuwählen. Dies ist eine Aufgabe, wenn Sie viele Benutzer haben. Durch Erstellen einer PowerShell Skript folgt, durchlaufen Sie eine Liste der Benutzer und ermöglichen.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Hier ist ein Beispiel:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Weitere Informationen finden Sie unter [User Zustände in Azure mehrstufige Authentifizierung](multi-factor-authentication-get-started-user-states.md)

## <a name="next-steps"></a>Nächste Schritte
Da Azure mehrstufige Authentifizierung in der Cloud eingerichtet haben, können Sie konfigurieren und Einrichten der Bereitstellung. Weitere Informationen finden Sie unter [Konfiguration von Azure mehrstufige Authentifizierung](multi-factor-authentication-whats-next.md) .
