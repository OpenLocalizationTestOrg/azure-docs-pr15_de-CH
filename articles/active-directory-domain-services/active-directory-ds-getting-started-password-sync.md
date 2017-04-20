<properties
    pageTitle="Azure Active Directory-Domänendienste: Kennwortsynchronisation aktivieren | Microsoft Azure"
    description="Erste Schritte mit Azure Active Directory-Domänendienste"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Synchronisierung von Kennwörtern in Azure AD-Domänendienste aktivieren
In vorhergehenden Aufgaben aktiviert Azure Active Directory-Domänendienste für Azure AD-Mandanten. Die nächste Aufgabe soll Anmeldeinformationen Hashes für NTLM und Kerberos-Authentifizierung mit Active Directory-Domänendienste Azure erforderlich. Wenn Anmeldeinformationen Synchronisierung eingerichtet ist, können Benutzer über ihre Unternehmensanmeldeinformationen der verwalteten Domäne anmelden.

Die Schritte unterscheiden sich abhängig, ob Ihr Unternehmen nur Cloud Azure AD verfügt Mieter oder mit Ihrem lokalen Verzeichnis mit Azure AD verbinden synchronisieren soll.

<br>

> [AZURE.SELECTOR]
- [Nur Cloud Azure AD Mieter](active-directory-ds-getting-started-password-sync.md)
- [Azure AD-Mandanten synchronisiert](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Aufgabe 5: Aktivieren Kennwortsynchronisation AAD Domain Services für Cloud nur Azure AD-Mandanten
Azure Active Directory-Domänendienste müssen Hashes für NTLM und Kerberos-Authentifizierung zum Authentifizieren von Benutzern in der verwalteten Domäne geeigneten Anmeldeinformationen. Aktivieren AAD Domänendienste für Ihren Mandanten Azure AD nicht erstellen oder speichern Sie Anmeldeinformationen Hashwerte in das Format für NTLM oder Kerberos-Authentifizierung erforderlich. Aus offensichtlichen Sicherheitsgründen werden Azure AD auch keine Anmeldeinformationen im Klartext-Formular gespeichert. Daher muss Azure AD keine Möglichkeit, diese NTLM oder Kerberos-Anmeldeinformationen Hashes basierend auf vorhandenen Anmeldeinformationen von Benutzern generieren.

> [AZURE.NOTE] Ihre Organisation hat eine Cloud nur Benutzern mit Azure Active Directory-Domänendienste Azure AD-Mandanten muss ihre Kennwörter ändern.

Diese Änderung des Kennworts wird die Anmeldeinformationen Hashes erforderliche Azure AD-Domänendienste für Kerberos und NTLM-Authentifizierung in Azure AD generiert werden. Sie können entweder ablaufen Kennwörter für alle Benutzer in der Mandanten, die Azure Active Directory-Domänendienste verwenden oder diese Benutzer anzuweisen, ihre Kennwörter ändern.


### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Aktivieren Sie NTLM und Kerberos Hash-Berechtigungsnachweisen für Cloud nur Azure AD-Mandanten
Hier sind Anweisungen Endbenutzer bereitstellen, damit Benutzer ihre Kennwörter ändern können:

1. Navigieren Sie zur Seite Azure AD-Abdeckung für Ihre Organisation am [http://myapps.microsoft.com](http://myapps.microsoft.com).

2. Wählen Sie die Registerkarte **Profil** auf dieser Seite.

3. Klicken Sie auf **Kennwort ändern** nebeneinander auf dieser Seite.

    ![Erstellen Sie ein virtuelles Netzwerk für Azure Active Directory-Domänendienste.](./media/active-directory-domain-services-getting-started/user-change-password.png)

    > [AZURE.NOTE] Sicherstellen Sie die Option **Kennwort ändern** auf Abdeckung nicht angezeigt wird, dass Ihre Organisation [Passwort-Management in Azure AD](../active-directory/active-directory-passwords-getting-started.md)konfiguriert.

4. Geben Sie auf der Seite **Kennwort ändern** das vorhandene (alte) Kennwort und geben Sie ein neues Kennwort ein und bestätigen. Klicken Sie auf **Senden**.

    ![Erstellen Sie ein virtuelles Netzwerk für Azure Active Directory-Domänendienste.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Nachdem Sie Ihr Kennwort geändert haben, wird das neue Kennwort in Kürze in Azure Active Directory-Domänendiensten verwendet werden. Nach einigen Minuten (in der Regel ungefähr 20 Minuten,) auf Computern der verwalteten Domäne mit dem neu geänderten Kennwort anmelden können.

<br>

## <a name="related-content"></a>Verwandte Inhalte

- [Wie Sie Ihr eigenes Kennwort aktualisieren](../active-directory/active-directory-passwords-update-your-own-password.md)

- [Erste Schritte mit Passwort-Management in Azure AD](../active-directory/active-directory-passwords-getting-started.md).

- [Aktivieren Kennwortsynchronisation AAD Domain Services für synchronisierte Azure AD-Mandanten](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [Eine verwaltete Azure Active Directory-Domänendienste-Domäne verwalten](active-directory-ds-admin-guide-administer-domain.md)

- [Einen Windows-Computer zu einer verwalteten Azure Active Directory-Domänendienste-Domäne hinzufügen](active-directory-ds-admin-guide-join-windows-vm.md)

- [Eine Red Hat Enterprise Linux VM zu einer verwalteten Azure Active Directory-Domänendienste-Domäne hinzufügen](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
