<properties
   pageTitle="Wie mehrstufige Authentifizierung | Microsoft Azure"
   description="Erfahren Sie, wie mehrstufige Authentifizierung (MFA) für privilegierte Identitäten Azure Active Directory privilegierte Identitätsmanagement-Erweiterung."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="how-to-require-mfa-in-azure-ad-privileged-identity-management"></a>Zum Anfordern von MFA in Azure AD privilegierten Identitätsmanagement

Wir empfehlen, mehrstufige Authentifizierung (MFA) für alle Administratoren erforderlich. Dies verringert das Risiko eines Angriffs durch ein Kennwort offengelegt.

Sie können festlegen, dass Benutzer ein MFA Zusatzübungseinheit, wenn sie sich anmelden. Blogbeitrag [MFA für Office 365 und MFA für Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) vergleicht, was mit Microsoft Azure mehrstufige Authentifizierung Angebot enthaltenen Funktionen in Office und Azure-Abonnements enthalten ist.

Sie können auch festlegen, dass Benutzer eine Herausforderung MFA abgeschlossen, wenn sie eine Rolle in Azure AD PIM aktivieren. So, wenn der Benutzer eine Herausforderung MFA nicht Wenn sie eingeloggt, werden sie aufgefordert dazu von PIM.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Die MFA in Azure AD Berechtigungen Identitätsmanagement

Bei der Verwaltung von Identitäten in PIM als bevorzugte Rollenadministrator sehen Sie Alarme, die MFA für privilegierten Konten zu empfehlen. Sicherheitshinweis PIM-Dashboard auf und öffnet ein neues Blatt mit einer Liste von Administratorkonten, die MFA erforderlich ist.  Erfordern MFA mehrere Rollen auswählen und dann auf die Schaltfläche **Update** und klicken Sie auf die Auslassungspunkte neben Rollen und klicken Sie dann auf die Schaltfläche **Update** .

> [AZURE.IMPORTANT] Rechts, Azure MFA funktioniert nur Arbeit oder schulkonten nicht Microsoft-Konten (in der Regel ein persönliches Konto, mit dem Anmeldung bei Microsoft Services wie Skype, Xbox, Outlook.com.). Daher kann jeder Benutzer mit einem microsoftkonto Administrator berechtigt sein, da sie MFA verwenden können, um ihre Rollen aktivieren. Wenn diese Benutzer weiterhin Arbeitslasten mit einem microsoftkonto verwalten, erhöhen sie permanente Administratoren jetzt.

Außerdem können Sie im Abschnitt Rollen PIM-Dashboard auf die MFA-Anforderung für eine bestimmte Rolle ändern. Klicken Sie **auf** Blade-Rolle und dann unter mehrstufige Authentifizierung auswählen **können** .

## <a name="how-azure-ad-pim-validates-mfa"></a>Wie Azure AD PIM MFA überprüft

Es gibt zwei Optionen für MFA überprüfen, wenn ein Benutzer eine Rolle aktiviert.

Die einfachste Option ist zu Azure für Benutzer verlassen, eine privilegierte Rolle aktivieren. Zu diesem Zweck zunächst sicher, dass Benutzer lizenziert sind, bei Bedarf und für Azure MFA erfasst. Weitere Informationen hierzu ist in [Erste Schritte mit Azure mehrstufige Authentifizierung in der Cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). Es wird empfohlen, aber nicht erforderlich, konfigurieren Sie Azure Anzeige MFA für diese Benutzer erzwingen, wenn sie sich anmelden. Ist die MFA-Kontrollen von Azure AD PIM selbst erfolgt.

Wenn Benutzer lokale Authentifizierung haben Sie Ihre Identitätsanbieter MFA verantwortlich. Konfigurieren von AD umfasst Verbunddienste Smartcard-Authentifizierung vor dem Zugriff auf Azure AD [Sicherung Cloudressourcen Azure mehrstufige Authentifizierung mit AD FS](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) beispielsweise Informationen zum Konfigurieren von AD FS für Ansprüche Azure AD senden. Wenn ein Benutzer versucht, eine Rolle aktivieren, akzeptiert Azure AD PIM MFA für den Benutzer entsprechende Ansprüche erhält bereits überprüft wurde.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
