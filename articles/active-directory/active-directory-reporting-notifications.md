<properties
    pageTitle="Azure Active Directory Reporting Benachrichtigung"
    description="Wie Sie Azure Active Directory reporting von Notifications verdächtige Zeichen ins."
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/07/2016"
    ms.author="dhanyahk"/>

# <a name="azure-active-directory-reporting-notifications"></a>Azure Active Directory Reporting Benachrichtigung

## <a name="what-reports-generate-email-notifications"></a>Welche Berichte per e-Mail benachrichtigt

Zu diesem Zeitpunkt e-Mail nur unregelmäßig anmelden Bericht Aktivitätsauslösern Benachrichtigung.

## <a name="what-is-an-irregular-sign-in"></a>Was ist ein "unregelmäßigen anmelden"?

Unregelmäßige Anmeldungen sind die von unserem Computer lernen Algorithmen auf der Grundlage einer "unmöglich Travel" Bedingung kombiniert mit abweichenden Speicherort Gerät identifiziert. Möglicherweise versucht ein Hacker mit diesem Konto anmelden.

## <a name="who-receives-the-email-notifications"></a>Wer erhält die e-Mail-Benachrichtigungen?

E-Mail wird an alle globalen Administratoren Active Directory Premium-Lizenz zugewiesen wurde gesendet. Um sicherzustellen, dass sie bereitgestellt wird, senden wir die Admins alternative e-Mail-Adresse sowie. Administratoren sollen aad-alerts-noreply@mail.windowsazure.com in ihrer Liste sicherer Absender sie e-Mail verpassen.

## <a name="how-often-are-these-emails-sent"></a>Wie häufig werden diese e-Mails gesendet?

Die e-Mail wird 10 neue unregelmäßige anmelden Aktivitäten in den letzten 30 Tagen, ob seit die letzte e-Mail gesendet wurde, ist.

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>Zugriffs des Berichts in der e-Mail

Wenn Sie auf den Link klicken, werden Sie auf die Berichtsseite im klassischen Azure-Portal weitergeleitet. Um den Bericht zuzugreifen, müssen Sie sein:

- Ein Administrator oder co-Administrator Ihres Azure-Abonnements

- Ein globaler Administrator im Verzeichnis und Active Directory Premium-Lizenz. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

## <a name="can-i-turn-off-these-emails"></a>Können diese e-Mails werden deaktiviert?

Ja, um Benachrichtigungen zu abweichenden Anmeldungen in Azure-Verwaltungsportal zu deaktivieren, auf **Konfigurieren**, und wählen Sie dann im Abschnitt **Benachrichtigung** **deaktiviert** .

## <a name="whats-next"></a>Was kommt als nächstes
- Sicherheit, interessieren überwachen und Berichte zur Verfügung? [Sicherheit von Azure AD, Überwachung](active-directory-view-access-usage-reports.md) und Berichte überprüfen
- [Erste Schritte mit Azure Active Directory Premium](active-directory-get-started-premium.md)
- [Branding auf anmelden und Abdeckung Seiten Firma](active-directory-add-company-branding.md)
