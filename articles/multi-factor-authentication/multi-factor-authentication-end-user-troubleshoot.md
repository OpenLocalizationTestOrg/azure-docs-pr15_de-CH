<properties
    pageTitle="Problembehandlung bei zweistufiger Überprüfung | Microsoft Azure"
    description="Dieses Dokument wird Benutzer Informationen auf, wenn sie ein Problem mit Azure mehrstufige Authentifizierung ausführen."
    services="multi-factor-authentication"
    keywords = "die kombinierte Authentifizierung-Client Authentifizierungsproblem Korrelations-ID"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="having-trouble-with-two-step-verification"></a>Probleme mit zwei Überprüfung

Dieser Artikel beschreibt einige Probleme, die bei zweistufigen Authentifizierung auftreten können. Wenn das Problem sind hier nicht enthalten ist, geben Sie detaillierte Feedback in den Kommentaren zu verbessern können.

## <a name="i-lost-my-phone-or-it-was-stolen"></a>Ich mein Telefon verloren oder gestohlen wurde

Es gibt zwei Arten zu Ihrem Konto. Die erste ist anmelden deaktiviert Telefonnummer, wenn Sie eine eingerichtet haben. Der zweite ist der Systemadministrator Ihre deaktivieren.

Ihr Telefon verloren geht oder gestohlen wurde, sollten auch Systemadministrator Ihre app Kennwörter zurücksetzen und löschen Erinnerung alle Geräte. Wenn Ihr Administrator hierzu nicht sicher, zeigen sie auf diesen Artikel: [Verwalten von Benutzern und Geräten](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords).


### <a name="use-an-alternate-phone-number"></a>Verwenden Sie eine andere Telefonnummer

Wenn Sie mehrere Überprüfungsoptionen eine sekundäre Telefonnummer oder eine Authenticator-Anwendung auf einem anderen Gerät eingerichtet haben können eine dieser Sie anmelden.

Mit der alternativen Telefonnummer anmelden, gehen Sie folgendermaßen vor:

1. Melden Sie sich wie gewohnt.
2. Wenn Sie aufgefordert werden, Ihr Konto zu überprüfen, wählen Sie **verwenden eine andere Bestätigungsmethode**

    ![Andere Überprüfung](./media/multi-factor-authentication-end-user-manage/differentverification.png)

3. Wählen Sie die Telefonnummer, der Sie Zugriff haben.

    ![Telefon](./media/multi-factor-authentication-end-user-manage/altphone2.png)

4. Nachdem Sie in Ihr Konto [verwalten Ihre](multi-factor-authentication-end-user-manage-settings.md) authentifizierungstelefonnummer ändern können.

>[AZURE.IMPORTANT]
>Es ist wichtig, eine sekundäre authentifizierungstelefonnummer konfigurieren. Wenn Ihre primäre Telefonnummer und der mobilen Anwendung auf dem gleichen Telefon, müssen eine dritte option, wenn Ihr Telefon verloren geht oder gestohlen wird.

### <a name="clear-your-settings"></a>Deaktivieren Sie die Einstellung

Wenn Sie eine sekundäre authentifizierungstelefonnummer nicht konfiguriert haben, müssen Sie Ihren Administrator um Hilfe bitten. Haben sie Ihre deaktivieren, sodass beim nächsten Anmelden Sie richten Sie [Ihr Konto](multi-factor-authentication-end-user-first-time.md) wieder aufgefordert werden.


## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>Ich erhalte einen Text oder meinen Telefonanruf

Es gibt mehrere Gründe, warum versuchen Sie möglicherweise, anmelden, aber nicht den Text oder Telefonanruf. Wenn Sie erfolgreich Texte oder Anrufe an Ihr Mobiltelefon in der Vergangenheit erhalten haben, ist dies wahrscheinlich ein Problem mit dem Telefonanbieter nicht auf Ihr Konto. Stellen Sie sicher, dass Sie gute Zelle Signal, und wenn Sie erhalten eine Textnachricht sicherstellen, dass Ihr Telefon und SMS-Nachrichten unterstützen.

Wenn Sie einen Text oder einige Minuten gewartet haben, ist der schnellste Weg zu Ihrem Konto versuchen Sie eine andere Option.

1. Wählen Sie **verwenden eine andere Bestätigungsmethode** auf der Seite der Bestätigung wartet.

    ![Andere Überprüfung](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

2. Wählen Sie die Telefon Nummer oder Lieferung verwenden möchten.

    Wenn Sie mehrere Bestätigungscodes erhalten, funktioniert die neuste Version.

Wenn Sie eine andere Methode konfiguriert haben, wenden Sie sich an Ihren Administrator, und bitten Sie sie Einstellungen deaktivieren. Das nächste Mal anmelden, werden Sie [mehrstufige Authentifizierung](multi-factor-authentication-end-user-first-time.md) erneut aufgefordert werden.


Haben Sie häufig Verzögerungen ungültige Zelle Signal, empfehlen wir Sie [Microsoft Authenticator app](multi-factor-authentication-microsoft-authenticator.md) auf dem Smartphone. Die app können zufällige Sicherheitscodes, mit denen Sie sich anmelden und diese Codes keine Zelle Signal oder Internet-Verbindung erforderlich.


## <a name="app-passwords-are-not-working"></a>App-Kennwörter sind nicht funktionsfähig.

Vergewissern Sie sich, dass Sie das app-Kennwort richtig eingegeben haben.  Sollten Sie immer noch nicht funktioniert Anmeldung und [ein neues Kennwort für die Anwendung erstellen](multi-factor-authentication-end-user-app-passwords.md).  Wenn dies nicht funktioniert, wenden Sie sich an Ihren Administrator und Sie können [Ihre vorhandene Anwendung Kennwörter löschen](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords) und dann eine neue erstellen.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Antwort auf Mein Problem nicht gefunden werden.

Haben diese Schritte jedoch laufen noch Probleme wenden Sie Systemadministrator oder die Person, die kombinierte Authentifizierung für Sie einrichten. Sie sollten Ihnen helfen können.

Stellen Sie eine Frage in [Azure AD-Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) oder [Support](https://support.microsoft.com/contactus) auch wir Antworten für Ihr Problem so schnell wie möglich.

Wenn Sie Support enthalten Sie die folgende Informationen:

- **Benutzer-ID** – Was ist die e-Mail-Adresse, die Sie anmelden?
- **Allgemeine Beschreibung des Fehlers** – welche genauen Fehlermeldung wurde angezeigt?  Wurde keine Fehlermeldung, beschreiben Sie unerwartete Verhalten im Detail bemerkt.
- **Seite** -Seite waren Sie sah den Fehler (auch den URL)?
- **ErrorCode** - der Fehlercode: Sie erhalten.
- **SessionId** - bestimmte Sitzung-Id, die Sie erhalten.
- **Korrelations-ID** – was Correlation ID-Code generiert, wenn der Benutzer den Fehler gesehen haben.
- **Zeitstempel** – was genau Datum und Uhrzeit den Fehler sah (einschließlich die Zeitzone)?

Diese Informationen kann auf der Seite gefunden werden. Wählen Sie bei der Überprüfung der anmelden rechtzeitig nicht **Anzeigen**.

![Fehlerdetails anmelden](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

Diese Informationen kann wir Ihr Problem möglichst schnell zu lösen.

## <a name="related-topics"></a>Verwandte Themen
- [Verwalten der zweistufigen Überprüfung](multi-factor-authentication-end-user-manage-settings.md)  
- [Häufig gestellte Fragen zu Microsoft Authenticator Anwendung](multi-factor-authentication-app-faq.md)
