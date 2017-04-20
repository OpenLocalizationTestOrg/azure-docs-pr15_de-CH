<properties
    pageTitle="Festlegen von Richtlinien für den Kennwortablauf in Azure Active Directory | Microsoft Azure"
    description="Informationen Sie zu Richtlinien für den Kennwortablauf Kennwortablauf Benutzer einzeln oder gebündelt Azure Active Directory-Kennwörter ändern"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="set-password-expiration-policies-in-azure-active-directory"></a>Kennwort in Active Directory Azure Ablaufrichtlinien festlegen

> [AZURE.IMPORTANT] **Sind Sie hier beim Anmelden Probleme auftreten?** In diesem Fall [hier wie ändern und Ihr eigenes Kennwort](active-directory-passwords-update-your-own-password.md).

Als globaler Administrator für einen Microsoft Cloud-Dienst können der Microsoft Azure Active Directory-Modul für Windows PowerShell Kennwörter einrichten nicht ablaufen. Sie können auch Windows PowerShell-Cmdlets Entfernen der nie-abläuft Konfiguration oder die Benutzer Kennwörter werden eingerichtet, läuft ab. Dieser Artikel bietet Unterstützung für Cloud-Dienste wie Windows Intune und Office 365, die Identität und Verzeichnisdienste von Microsoft Azure Active Directory abhängig.

  > [AZURE.NOTE] Nur Kennwörter für Benutzerkonten, die nicht durch Synchronisierung synchronisiert werden können konfiguriert werden nicht ablaufen. Weitere Informationen über Synchronisierung finden Sie unter Liste der Hilfethemen in [Directory Synchronization](https://msdn.microsoft.com/library/azure/hh967642.aspx).

Um Windows PowerShell-Cmdlets verwenden, müssen Sie zuerst installieren.

## <a name="what-do-you-want-to-do"></a>Was möchten Sie tun?

- [Ablaufrichtlinie Kennwort überprüfen](#how-to-check-expiration-policy-for-a-password)

- [Legen Sie ein Kennwort abläuft](#set-a-password-to-expire)

- [Festlegen Sie ein Kennwort, so dass es nicht abläuft](#set-a-password-to-never-expire)

## <a name="how-to-check-expiration-policy-for-a-password"></a>Ablaufrichtlinie Kennwort überprüfen

1.  Verbinden Sie mit Windows PowerShell mithilfe Ihrer Anmeldeinformationen Unternehmen.

2.  Führen Sie eine der folgenden:

    - Um festzustellen, ob einzelne Benutzer Kennwort nie ablaufen soll, führen Sie das folgende Cmdlet mit der Benutzerprinzipalname (UPN) (z. B. aprilr@contoso.onmicrosoft.com) oder die Benutzer-ID des Benutzers zu überprüfen:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`

    - Um die Einstellung "Kennwort läuft nie ab" für alle Benutzer anzuzeigen, führen Sie das folgende Cmdlet:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

## <a name="set-a-password-to-expire"></a>Legen Sie ein Kennwort abläuft

1.  Verbinden Sie mit Windows PowerShell mithilfe Ihrer Anmeldeinformationen Unternehmen.

2.  Führen Sie eine der folgenden:

    - Um das Kennwort eines Benutzers festgelegt, dass das Kennwort abläuft, führen Sie das folgende Cmdlet mit der Benutzerprinzipalname (UPN) oder die Benutzer-ID des Benutzers:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`

    - Um Kennwörter für alle Benutzer in der Organisation festgelegt, dass sie abläuft, verwenden Sie das folgende Cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

## <a name="set-a-password-to-never-expire"></a>Legen Sie ein Kennwort läuft nie ab

1. Verbinden Sie mit Windows PowerShell mithilfe Ihrer Anmeldeinformationen Unternehmen.

2.  Führen Sie eine der folgenden:

    - Legen Sie das Kennwort eines Benutzers niemals ablaufen führen Sie das folgende Cmdlet mit der Benutzerprinzipalname (UPN) oder die Benutzer-ID des Benutzers:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`

    - Um Kennwörter für alle Benutzer in einer Organisation kein Ablaufdatum festzulegen, führen Sie das folgende Cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Nächste Schritte

* **Sind Sie hier beim Anmelden Probleme auftreten?** In diesem Fall [hier wie ändern und Ihr eigenes Kennwort](active-directory-passwords-update-your-own-password.md).
