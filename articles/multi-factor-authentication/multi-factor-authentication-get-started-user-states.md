<properties 
    pageTitle="Microsoft Azure mehrstufige Authentifizierung Benutzerstatus"
    description="Enthält Informationen Sie zum Benutzerstatus in Azure MFA."
    services="multi-factor-authentication"
    documentationCenter=""
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

# <a name="user-states-in-azure-multi-factor-authentication"></a>Benutzerstatus in Azure mehrstufige Authentifizierung

Benutzerkonten in Azure mehrstufige Authentifizierung haben die folgenden drei unterschiedlichen Zuständen:

Zustand | Beschreibung |Nicht-Browser-apps betroffen| Notizen
:-------------: | :-------------: |:-------------: |:-------------: |
Deaktiviert | Der Standardzustand für einen neuen Benutzer für mehrstufige Authentifizierung nicht registriert.|Nein|Der Benutzer wird nicht mehrstufige Authentifizierung verwendet.
Aktiviert |Mehrstufige Authentifizierung wurde der Benutzer angemeldet.|Nein.  Sie weiterhin bis zum Abschluss der Registrierung.|Der Benutzer ist jedoch die Registrierung wurde nicht abgeschlossen. Sie werden aufgefordert, bei der nächsten Anmeldung abzuschließen.
Erzwungen|Der Benutzer wurde registriert und Registrierung für mehrstufige Authentifizierung abgeschlossen.|Ja.  Apps app-Kennwörter. | Der Benutzer kann oder möglicherweise keine Registrierung abgeschlossen. Wenn sie den Registrierungsvorgang abgeschlossen haben, sind sie mehrstufige Authentifizierung verwenden. Andernfalls wird der Benutzer aufgefordert werden bei der nächsten Anmeldung abzuschließen.

## <a name="changing-a-user-state"></a>Ändern des Benutzerstatus
Ein Benutzer ändert sich, je nachdem, ob es für MFA wurde und ob der Benutzer abgeschlossen.  Wenn Sie MFA für einen Benutzer aktivieren, deaktiviert die Zustandsänderung vom Benutzer aktiviert.  Sobald der Benutzer zum geänderten Zustand wurde aktiviert, meldet und schließt den Prozess ab, ändert Zustand, erzwungene.  

### <a name="to-view-a-users-state"></a>Status des Benutzers anzeigen
--------------------------------------------------------------------------------
1.  Melden Sie sich als Administrator in **Azure-Verwaltungsportal** an.
2.  Klicken Sie auf der linken Seite auf **Active Directory**.
3.  Klicken Sie unter auf **Verzeichnis** auf das Verzeichnis für den Benutzer aktivieren möchten.
![Klicken Sie auf Verzeichnis](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Klicken Sie oben auf **Benutzer**.
5.  Klicken Sie am unteren Rand der Seite auf **Verwaltung mehrstufige Authentifizierung**.
![Klicken Sie auf Verzeichnis](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Dadurch wird eine neue Registerkarte geöffnet.  Sie können den Status der Benutzer anzeigen.
![Klicken Sie auf Verzeichnis](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

###<a name="to-change-the-state-from-disabled-to-enabled"></a>Ändern Sie den Zustand von deaktiviert auf aktiviert
1.  Melden Sie sich als Administrator in **Azure-Verwaltungsportal** an.
2.  Klicken Sie auf der linken Seite auf **Active Directory**.
3.  Klicken Sie unter auf **Verzeichnis** auf das Verzeichnis für den Benutzer aktivieren möchten.
![Klicken Sie auf Verzeichnis](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Klicken Sie oben auf **Benutzer**.
5.  Klicken Sie am unteren Rand der Seite auf **Verwaltung mehrstufige Authentifizierung**.
![Klicken Sie auf Verzeichnis](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Dadurch wird eine neue Registerkarte geöffnet.  Suchen Sie den Benutzer, den Sie für mehrstufige Authentifizierung aktivieren möchten. Sie müssen die Ansicht oben ändern. Stellen Sie sicher, dass der Status **deaktiviert.** 
 ![Können](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Setzen Sie in das Feld neben dem Namen eine **Überprüfen** .
7.  Klicken Sie auf der rechten Seite auf **Aktivieren**.
![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Aktivieren Sie **mehrstufige Authentifizierung**.
![Benutzer aktivieren](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Beachten Sie den Status des Benutzers **deaktiviert** **aktiviert**geändert hat.
![Anwender](./media/multi-factor-authentication-get-started-cloud/user.png)
10.  Nach Benutzer aktiviert haben, wird empfohlen, dass Sie per e-Mail benachrichtigt.  Es sollte auch informieren sie ihre apps-Browser Verwendung zu gesperrt wurde.

### <a name="to-change-the-state-from-enabledenforced-to-disabled"></a>Ändern Sie den Status von aktiviert/erzwungen deaktiviert
1.  Melden Sie sich als Administrator in **Azure-Verwaltungsportal** an.
2.  Klicken Sie auf der linken Seite auf **Active Directory**.
3.  Klicken Sie unter auf **Verzeichnis** auf das Verzeichnis für den Benutzer aktivieren möchten.
![Klicken Sie auf Verzeichnis](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Klicken Sie oben auf **Benutzer**.
5.  Klicken Sie am unteren Rand der Seite auf **Verwaltung mehrstufige Authentifizierung**.
![Klicken Sie auf Verzeichnis](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Dadurch wird eine neue Registerkarte geöffnet.  Suchen Sie den Benutzer, den Sie deaktivieren möchten. Sie müssen die Ansicht oben ändern. Stellen Sie sicher, dass der Status entweder **aktiviert** oder **erzwungen.**
7.  Setzen Sie in das Feld neben dem Namen eine **Überprüfen** .
7.  Klicken Sie auf der rechten Seite auf **Deaktivieren**.
![Benutzer deaktivieren](./media/multi-factor-authentication-get-started-user-states/userstate2.png)
8.  Sie werden aufgefordert, dies zu bestätigen.  Klicken Sie auf **Ja**.
![Benutzer deaktivieren](./media/multi-factor-authentication-get-started-user-states/userstate3.png)
9.  Sie sollten dann sehen, erfolgreich war.  Klicken Sie auf **Schließen.** 
 ![Benutzer deaktivieren](./media/multi-factor-authentication-get-started-user-states/userstate4.png)
