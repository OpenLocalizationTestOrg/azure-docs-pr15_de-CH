<properties
   pageTitle="Branding-Richtlinien für Applikationen | Microsoft Azure"
   description="Ein umfassendes Handbuch Entwicklerorientierte Ressourcen für Azure Active Directory"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/23/2016"
   ms.author="mbaldwin"/>


# <a name="branding-guidelines-for-applications"></a>Richtlinien für Applikationen


Dieses Thema erläutert die Richtlinien, bei der Entwicklung mit Azure Active Directory (Azure AD) verwenden soll. Verweisen Sie Ihre Kunden, wenn sie ihr Konto Arbeits- oder Schulcomputer verwaltet in Azure AD verwenden oder Ihren Konto für die Anmeldung und melden Sie sich bei der Anwendung helfen dieser Richtlinien.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Personal und Arbeit oder schulkonten von Microsoft

Microsoft verwaltet zwei Arten von Benutzerkonten:

- **Persönliche Konten** (früher als Windows Live ID). Diese Konten der Beziehung zwischen dem *Benutzer* und Microsoft und auf Geräten und Diensten von Microsoft verwendet. Diese Konten sind für den persönlichen Gebrauch vorgesehen.

- **Geschäfts- oder Konten.** Diese Konten werden von Microsoft für Unternehmen verwaltet, die Azure Active Directory. Diese Konten werden von Microsoft Office 365 und andere kommerzielle Dienstleistungen anmelden verwendet.

Microsoft, Geschäfts-oder von Organisationen (Unternehmen, Schule, Regierungsbehörde) normalerweise Endbenutzer (Mitarbeiter, Studenten, federal Mitarbeiter) zugewiesen sind. Diese Konten direkt in der Cloud in Azure AD-Plattform beherrschen oder Azure AD aus einem lokalen Verzeichnis, wie Windows Server Active Directory synchronisiert. Microsoft ist der *Treuhänder* Arbeits- oder Schulcomputer Konten jedoch Konten gehört und von der Organisation kontrolliert.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Verweisen auf Azure AD-Konten in der Anwendung

Microsoft nicht Endbenutzer Azure oder Active Directory-Markennamen und weder verfügbar machen soll.

- Sobald Benutzer eingeloggt sind, verwenden Sie die Organisation Name und Logo so weit wie möglich. Das ist besser als allgemeine Begriffe wie "Organisation".

- Wenn Benutzer nicht angemeldet sind, lesen Sie ihre Konten als "Arbeit oder Schule" und das Microsoft-Logo um zu vermitteln, dass diese Konten von Microsoft verwaltet werden. Verwenden Sie keine Begriffe wie "Unternehmenskonto", "Geschäftskonto" oder "corporate-Konto" die Benutzer verwirren.

## <a name="user-account-pictogram"></a>Benutzer Konto Piktogramm
In einer früheren Version dieser Richtlinien wird empfohlen, einen Piktogramm "blaue Abzeichen" verwenden. Aufgrund des Feedbacks für Benutzer und Entwickler empfehlen wir jetzt mit dem Microsoft-Logo stattdessen. Dadurch können Benutzer wissen, dass sie das Konto, dass sie mit Office 365 oder anderen Microsoft BusinessServices mit Ihrer Anwendung anmelden wiederverwenden können.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Anmeldung und Azure AD anzumelden

Ihre app möglicherweise separate Pfade zu Anmeldung anmelden vorhanden und die folgenden Abschnitte bieten visual für beide Szenarien.

**, Wenn Ihre Anwendung Benutzer Zeichen unterstützt (z. B. kostenlose Testversion oder Freemium) auf**: **eine, die Benutzer auf Ihre app mit ihrer Arbeit-Konto oder ihrem persönlichen Konto Schaltfläche** anzeigen. Azure wird Zustimmung aufgefordert erstmaligen Anzeige auf Ihre Anwendung zugreifen.

**Wenn Ihre Anwendung Berechtigungen benötigt, die nur Administratoren zustimmen können oder Ihre app Organisationseinheiten müssen**: Trennen Sie Admin Übernahme von Benutzer anmelden. Die **Schaltfläche "get app"** umleiten Administratoren anmelden und bitten um Zustimmung für Benutzer in ihrer Organisation. Dies hat den Vorteil Benutzer Zustimmung aufgefordert, Ihre app unterdrücken.

## <a name="visual-guidance-for-app-acquisition"></a>Visuelle Hinweise zur app Anschaffung

Die Verknüpfung "get app" Umleiten des Benutzers auf Zugriff gewähren Azure AD (Autorisierung) Seite ermöglichen ein Organisationsadministrator ermächtigen Ihre app auf ihre Unternehmensdaten zugreifen, die von Microsoft gehostet wird. Die [Integration von Applikationen Azure Active Directory](active-directory-integrating-applications.md) Artikel behandelt Details zum Zugriff anfordern.

Nachdem Administratoren für Ihre Anwendung bekommen, können sie die Benutzer Office 365 app-Start Erfahrung (nur die waffel und [https://portal.office.com/myapps](https://portal.office.com/myapps)) hinzufügen. Wenn Sie diese Funktion ankündigen möchten, können Sie Begriffe wie "App für Ihr Unternehmen hinzufügen" und eine Schaltfläche wie folgt anzeigen:

![Anwendungstypen und Szenarien](./media/active-directory-branding-guidelines/add-to-my-org.png)

Wir empfehlen jedoch erläuternden Text anstatt auf Schaltflächen zu schreiben. Zum Beispiel:
> *Wenn Sie bereits Office 365 oder einem anderen Business Service von Microsoft verwenden, können Sie einfach < Your_app_name > Zugriff auf Daten Ihrer Organisation gewähren. Dadurch können Benutzer auf < Your_app_name > mit ihrer vorhandenen Arbeitsaufgaben zugreifen.*


## <a name="visual-guidance-for-sign-in"></a>Visuelle Hinweise zur Anmeldung
Ihre app sollte ein Zeichen Taste anzeigen, die Benutzer an den Endpunkt-Anmeldung umgeleitet, die das Protokoll entspricht, mit der Integration von Azure AD Der folgende Abschnitt enthält Details wie die Schaltfläche aussehen sollte.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Piktogramm und "Sich mit Microsoft"
Ist die Zuordnung von dem Microsoft-Logo und "Zeichen mit Microsoft"-Begriffe, die eindeutig Azure AD unter anderen Identitätsanbietern stellt Ihre Anwendung unterstützen. Haben Sie genügend Speicherplatz für "Zeichen mit Microsoft", kann "Anmeldung" verkürzen.

![Anwendungstypen und Szenarien](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Anwendungstypen und Szenarien](./media/active-directory-branding-guidelines/sign-in-light.png)

Sie können auch ein dunkle Farbe für die Schaltflächen.

![Anwendungstypen und Szenarien](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Anwendungstypen und Szenarien](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>Branding- Verhaltensregeln

**Verwenden Sie "Arbeits- oder Schulcomputer Konto" zusammen mit der Schaltfläche "Zeichen mit Microsoft" zusätzliche Erklärung dazu Endbenutzer erkennen, ob sie es verwenden können.** Verwenden Sie **keine** anderen Begriffe wie "Unternehmenskonto", "Geschäftskonto" oder "corporate-Konto".

**Verwenden Sie "Office 365" oder "Azure-ID".** Office 365 ist auch der Name eines von Microsoft Azure AD für die Authentifizierung verwendet.

**Ändern Sie das Microsoft-Logo.**

Setzen Endbenutzern Marken Azure oder Active Directory **nicht** . Es ist jedoch ok Entwickler, IT-Spezialisten und Administratoren diese Begriffe mit.

## <a name="navigation-dos-and-donts"></a>Navigation Regeln

**Bieten Benutzer abmelden und zu einem anderen Benutzerkonto wechseln.** Während die meisten Menschen ein einzelnes persönliches Konto von Microsoft Facebook/Google/Twitter, sind häufig mit mehreren Organisationen. Unterstützung mehrerer angemeldeten Benutzer kommt bald.
