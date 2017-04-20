<properties
    pageTitle="Azure MFA Anmeldung Erfahrung mit Azure mehrstufige Authentifizierung"
    description="Auf dieser Seite wird Sie Anleitung auf, wo die verschiedenen Anmeldung Methoden mit Azure MFA bereitstellen."
    keywords="Benutzerauthentifizierung, anmelden, anmelden Mobiltelefon mit Telefon"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>Anmelden mit Azure mehrstufige Authentifizierung
> [AZURE.NOTE]  Die folgende Dokumentation auf dieser Seite zeigt normalerweise anmelden.  Hilfe zur Anmeldung finden Sie unter [Probleme mit Azure mehrstufige Authentifizierung](multi-factor-authentication-end-user-manage-settings.md)



## <a name="what-will-your-sign-in-experience-be"></a>Was ist der Anmeldevorgang.
Wie anzumelden und mehrstufige Authentifizierung verwenden werden Ihre Erfahrung hängt.  In diesem Abschnitt bieten wir Informationen zu erwarten, wenn Sie sich anmelden.  Wählen Sie das am besten beschreibt, was Sie tun:


Was machst du?|Beschreibung
:------------- | :------------- |
[Mobil-oder Office anzumelden](#signing-in-with-mobile-or-office-phone) | Dies erwartet Sie mit Ihrem Handy oder Office anmelden.
[Die Microsoft Authenticator app Benachrichtigung anmelden](#signing-in-with-the-microsoft-authenticator-app-using-notification) | Sie erwarten können Benachrichtigungen mit Microsoft Authenticator app.
[Die Microsoft Authenticator app Verifizierungscode anzumelden](#signing-in-with-the-microsoft-authenticator-app-using-verification-code)|Sie erwarten können mit Microsoft Authenticator-Thapp mit einem Bestätigungscode.
[Eine alternative Methode anzumelden](#signing-in-with-an-alternate-method)|Dies wird angezeigt, was Sie erwartet, wenn Sie eine alternative Methode verwenden möchten.

## <a name="signing-in-with-mobile-or-office-phone"></a>Mobil-oder Office anzumelden

Die folgende Informationen wird die Benutzung von mehrstufigen Authentifizierung mit Ihrem Telefon mobil oder Office beschrieben.

### <a name="to-sign-in-with-a-call-to-your-office-or-mobile-phone"></a>Mit einem Aufruf von Ihrem Büro oder Mobiltelefon anmelden

- Melden Sie sich bei einer Anwendung oder einem Dienst wie Office 365 mit Ihrem Benutzernamen und Kennwort an.
- Microsoft wird aufgerufen.

![Microsoft-Aufrufe](./media/multi-factor-authentication-end-user-signin-phone/call.png)

- Telefonanruf, und drücken Sie die #-Taste.

![Antwort](./media/multi-factor-authentication-end-user-signin-phone/phone.png)

- Sie sollten jetzt angemeldet werden.</li>

## <a name="signing-in-with-the-microsoft-authenticator-app-using-notification"></a>Die Microsoft Authenticator app Benachrichtigung anmelden

Die folgende Informationen beschreiben die Benutzung von mehrstufigen Authentifizierung mit Microsoft Authenticator app beim Senden einer Benachrichtigung.

### <a name="to-sign-in-with-a-notification-sent-the-microsoft-authenticator-app"></a>Um eine Benachrichtigung anmelden Microsoft Authenticator app gesendet

- Melden Sie sich bei einer Anwendung oder einem Dienst wie Office 365 mit Ihrem Benutzernamen und Kennwort an.
- Microsoft wird eine Benachrichtigung gesendet.

![Microsoft sendet eine Benachrichtigung](./media/multi-factor-authentication-end-user-signin-app-notify/notify.png)


- Telefonanruf, und drücken Sie die Taste überprüfen.  Wenn Ihr Unternehmen eine PIN werden Sie hier dafür aufgefordert.

![Stellen Sie sicher](./media/multi-factor-authentication-end-user-signin-app-notify/phone.png)

![Setup](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

- Sie sollten jetzt angemeldet werden.


## <a name="signing-in-with-the-microsoft-authenticator-app-using-verification-code"></a>Die Microsoft Authenticator app Verifizierungscode anzumelden

Die folgende Informationen beschreiben die Erfahrung Microsoft Authenticator app mehrstufige Authentifizierung mit, wenn Sie mit einer Überprüfung verwenden.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Die Anwendung Microsoft Authenticator Verifizierungscode mit anmelden

- Melden Sie sich bei einer Anwendung oder einem Dienst wie Office 365 mit Ihrem Benutzernamen und Kennwort an.
- Microsoft fordert Sie einen Bestätigungscode.

![Geben Sie den Verifizierungscode](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)

- Öffnen Sie Microsoft Authenticator-Anwendung auf Ihrem Telefon und geben Sie den Code in das Feld, in dem Sie anmelden.

![Erhalten](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)



- Sie sollten jetzt angemeldet werden.


## <a name="signing-in-with-an-alternate-method"></a>Eine alternative Methode anzumelden


Im folgenden Abschnitt wird zeigen, wie eine alternative Methode anmelden, wenn primäre Methode nicht verfügbar sein.

### <a name="to-sign-in-with-an-alternate-method"></a>Eine alternative Methode anmelden

- Melden Sie sich bei einer Anwendung oder einem Dienst wie Office 365 mit Ihrem Benutzernamen und Kennwort an.
- Eine andere Bestätigungsmethode auswählen.  Sie werden mit verschiedenen Optionen. Die anzeigen werden auf der Grundlage wie viele können

![Alternative Methode verwenden](./media/multi-factor-authentication-end-user-signin-alt/alt.png)

- Wählen Sie eine andere Methode und anmelden.
