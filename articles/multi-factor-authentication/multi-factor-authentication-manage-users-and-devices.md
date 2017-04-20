<properties 
    pageTitle="Azure mehrstufige Authentifizierung Berichte"
    description="Beschreibt, wie Benutzer ändern, wie der Benutzer den Nachweis Prozess wiederholen."
    documentationCenter=""
    services="multi-factor-authentication"
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Verwalten von Einstellungen Azure mehrstufige Authentifizierung in der cloud

Als Administrator können Sie Benutzer und Gerät Folgendes verwalten.  

- [Ausgewählte Benutzer Kontaktmethoden erneut eingeben](#require-selected-users-to-provide-contact-methods-again)
- [Vorhandene app Kennwörter löschen](#delete-users-existing-app-passwords)
- [Wiederherstellen Sie MFA auf alle angehaltenen Geräte für einen Benutzer](#restore-mfa-on-all-suspended-devices-for-a-user)






Dies ist hilfreich, wenn ein Computer oder Gerät verloren geht oder gestohlen oder Sie einen Benutzerzugriff entfernen müssen.


## <a name="require-selected-users-to-provide-contact-methods-again"></a>Ausgewählte Benutzer Kontaktmethoden erneut eingeben

Diese Einstellung wird zwingt Benutzer erneut registrieren, wenn er sich anmeldet. Beachten Sie, dass-Browser-apps funktionieren weiterhin, wenn der Benutzer Kennwörter app für sie.  Sie können app Benutzerkennwörtern löschen, indem Sie auch **alle vorhandenen Anwendung Kennwörter generiert durch die ausgewählten Benutzer löschen**.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Wie Benutzer Kontaktmethoden erneut eingeben




1. Anmelden bei klassischen Azure-Portal.
2. Klicken Sie auf der linken Seite auf Active Directory.
3. Klicken Sie unter auf Verzeichnis auf das Verzeichnis für den Benutzer sollen ihre Kontaktmethode erneut bereitstellen.
4. Klicken Sie oben auf Benutzer.
5. Klicken Sie am unteren Rand der Seite auf mehrstufige Authentifizierung verwalten Dadurch wird die kombinierte Authentifizierungsseite geöffnet.
6. Suchen Sie den Benutzer, den Sie verwalten, und setzen Sie ein Häkchen in das Feld neben dem Namen möchten. Sie müssen die Ansicht oben ändern.
7. Dadurch wird der **Verwalten Benutzer** Hyperlink auf der rechten Seite angezeigt. Klicken Sie auf.
8. Aktivieren Sie **ausgewählte Benutzer Kontaktmethoden erneut bereitstellen**.
![Bereitstellen von Kontaktmöglichkeiten](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
10. Klicken Sie auf Speichern.
11. Klicken Sie auf Schließen

## <a name="delete-users-existing-app-passwords"></a>Vorhandene app Kennwörter löschen

Dies löscht alle app-Kennwörter, die ein Benutzer erstellt hat. Ohne Browser-apps, die diese app Kennwörter zugeordnet wurden nicht mehr funktionieren, bis ein neues app-Kennwort erstellt.

### <a name="how-to-delete-users-existing-app-passwords"></a>App-Kennwörter vorhandene Benutzer löschen

1. Anmelden bei klassischen Azure-Portal.
2. Klicken Sie auf der linken Seite auf Active Directory.
3. Klicken Sie unter auf Verzeichnis im Verzeichnis des Benutzers app Kennwörter löschen möchten.
4. Klicken Sie oben auf Benutzer.
5. Klicken Sie am unteren Rand der Seite auf mehrstufige Authentifizierung verwalten Dadurch wird die kombinierte Authentifizierungsseite geöffnet.
6. Suchen Sie den Benutzer, den Sie verwalten, und setzen Sie ein Häkchen in das Feld neben dem Namen möchten. Sie müssen die Ansicht oben ändern.
7. Dadurch wird der **Verwalten Benutzer** Hyperlink auf der rechten Seite angezeigt. Klicken Sie auf.
8. Aktivieren Sie **alle vorhandenen Anwendung Kennwörter generiert durch die ausgewählten Benutzer löschen**.
![App-Kennwörter löschen](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
10. Klicken Sie auf Speichern.
10. Klicken Sie auf Schließen.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Wiederherstellen Sie MFA auf alle gespeicherten Benutzer

Administratoren können mehrstufige Authentifizierung auf Geräte und Browser wiederherstellen. Dabei wird dadurch speichern MFA aller Geräte des Benutzers und Browser und der Benutzer muss MFA verwenden, bei der nächsten Anmeldung.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>MFA auf alle angehaltenen Geräte für einen Benutzer wiederherstellen

1. Anmelden bei klassischen Azure-Portal.
2. Klicken Sie auf der linken Seite auf Active Directory.
3. Klicken Sie unter auf Verzeichnis im Verzeichnis des Benutzers Mfa auf wiederherstellen möchten.
4. Klicken Sie oben auf Benutzer.
5. Klicken Sie am unteren Rand der Seite auf mehrstufige Authentifizierung verwalten Dadurch wird die kombinierte Authentifizierungsseite geöffnet.
6. Suchen Sie den Benutzer, den Sie verwalten, und setzen Sie ein Häkchen in das Feld neben dem Namen möchten. Sie müssen die Ansicht oben ändern.
7. Dadurch wird der **Verwalten Benutzer** Hyperlink auf der rechten Seite angezeigt. Klicken Sie auf.
8. Aktivieren Sie **Wiederherstellen mehrstufige Authentifizierung auf allen Geräten gespeicherte**
![app Kennwörter löschen](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. Klicken Sie auf Speichern.
10. Klicken Sie auf Schließen.
