<properties
    pageTitle="Anmelden Erfahrungen mit Azure AD-Schutz | Microsoft Azure"
    description="Übersicht über die Benutzeroberfläche beim Schutz verringert oder beseitigt einen Benutzer oder eine mehrstufige Authentifizierung erforderlich ist."
    services="active-directory"
    keywords="Azure active Directory Schutz, Cloud app Discovery, Verwalten von Applikationen, Sicherheit, Risiken, Risiko, Schwachstelle, Sicherheitsrichtlinien"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Anmelden Erfahrungen mit Azure AD-Schutz

Mit Azure Active Directory Schutz können Sie:

- Registrierung für mehrstufige Authentifizierung erforderlich

- risikoreiche anmelden und betroffenen Benutzer verarbeiten

Die Reaktion des Systems auf diese Probleme hat einen Einfluss auf einen Benutzer anmelden da direkt-Anmelden mit einen Benutzernamen und ein Kennwort nicht mehr möglich sein. Zusätzliche Schritte erforderlich, um einen Benutzer sicher in Unternehmen.

Dieses Thema enthält eine Übersicht des Benutzers anmelden für alle Fälle, die auftreten können.

**Mehrstufige Authentifizierung**

- Mehrstufige Authentifizierung Registrierung



**Risiko anmelden**

- Risikoreiche recovery

- Risikoreiche anmelden blockiert

- Mehrstufige Authentifizierung Registrierung während riskant anmelden
 

**Benutzer gefährdet**

- Konto wiederherstellen

- Konto gesperrt




## <a name="multi-factor-authentication-registration"></a>Mehrstufige Authentifizierung Registrierung

Besonders benutzerfreundlich sowohl das Konto Recovery und die risikoreiche anmelden, wird der Benutzer selbst wiederherstellen kann. Wenn Benutzer für mehrstufige Authentifizierung registriert sind, haben sie bereits eine Telefonnummer ihrer Konto Sicherheitsprobleme übergeben. Help Desk oder Administrator vorteilhafte muss Kompromittierung Wiederherstellung. Daher wurde empfohlen, die Benutzer für die kombinierte Authentifizierung registriert. 

Administratoren können:

- eine Richtlinie festgelegt, die Benutzer ihren Konten für zusätzliche Überprüfung einrichten. 
- Lassen Sie überspringen mehrstufige Authentifizierung Registrierung bis zu 30 Tage bei sie Benutzern eine Frist vor der Registrierung möchten zu

**Die kombinierte Authentifizierung Registrierung umfasst drei Schritte:**

1. Im ersten Schritt erhält der Benutzer eine Benachrichtigung bezüglich des Kontos für mehrstufige Authentifizierung. 

    ![Behebung] (./media/active-directory-identityprotection-flows/140.png "Behebung")


2. Um Multifaktor-Authentifizierung einzurichten, müssen Sie das System kennen, wie Sie kontaktiert werden möchten.

    ![Behebung] (./media/active-directory-identityprotection-flows/141.png "Behebung")
 
3. Das System sendet eine Herausforderung, und Sie reagieren müssen.

    ![Behebung] (./media/active-directory-identityprotection-flows/142.png "Behebung")

 



## <a name="risky-sign-in-recovery"></a>Risikoreiche recovery

Wenn ein Administrator eine Richtlinie für Risiken anmelden konfiguriert hat, werden die betroffenen Benutzer beim Anmelden benachrichtigt. 

**Risikoreiche anmelden Fluss besteht aus zwei Schritten:** 

1. Der Benutzer wird informiert, dass etwas Ungewöhnliches über ihre anmelden, wie von einem neuen Speicherort, Gerät oder app Anmeldung erkannt wurde. 

    ![Behebung] (./media/active-directory-identityprotection-flows/120.png "Behebung")

2. Der Benutzer muss ausweisen lösen eine Herausforderung. Für mehrstufige Authentifizierung der Benutzer registriert ist benötigen sie einen Roundtrip Sicherheitscode auf ihre Telefonnummer. Da dies nur riskanten anmelden und keinem kompromittierten Konto Benutzer müssen das Kennwort in diesem zu ändern. 

    ![Behebung] (./media/active-directory-identityprotection-flows/121.png "Behebung")



 
## <a name="risky-sign-in-blocked"></a>Risikoreiche anmelden blockiert
Administratoren können auch sperren Benutzer bei der Anmeldung je nach Risiko-In Risiko-Policy fest. Um aufgehoben, müssen Benutzer ein Administrator oder Helpdesk wenden, oder sie können versuchen, von einem bekannten Speicherort oder Gerät anmelden. Lösen Sie mehrstufige Authentifizierung selbst wiederherstellen ist keine Option in diesem Fall.

![Behebung] (./media/active-directory-identityprotection-flows/200.png "Behebung")




## <a name="compromised-account-recovery"></a>Konto wiederherstellen

Benutzersicherheitsrichtlinie Risiko konfiguriert, Risiko Benutzer, die der Benutzer in der Richtlinie angegebenen (und deshalb gefährdet) müssen durch Benutzer Kompromiss Recovery Fluss durchlaufen, bevor sie anmelden können. 

**Benutzer Kompromiss Recovery Flows umfasst drei Schritte:**

1. Der Benutzer wird informiert, dass ihre Firma Sicherheit gefährdet durch auffällige oder Anmeldeinformationen wegen.

    ![Behebung] (./media/active-directory-identityprotection-flows/101.png "Behebung")

2.  Der Benutzer muss ausweisen lösen eine Herausforderung. Wenn Benutzer für mehrstufige Authentifizierung registriert können sie selbst wiederherstellen wird. Sie benötigen einen Roundtrip Sicherheitscode ihre Telefonnummer. 

    ![Behebung] (./media/active-directory-identityprotection-flows/110.png "Behebung")


3.  Schließlich wird der Benutzer gezwungen, ihr Kennwort ändern, da jemand Zugang zu ihrem Konto haben kann. Screenshots von dieser Erfahrung sind unten aufgeführt.
 
    ![Behebung] (./media/active-directory-identityprotection-flows/111.png "Behebung")



## <a name="compromised-account-blocked"></a>Konto gesperrt 

Um einen Benutzer abzurufen, der vom Risiko Benutzersicherheitsrichtlinie Blockierung blockiert wurde, muss der Benutzer sich an den Administrator oder Helpdesk. Lösen Sie mehrstufige Authentifizierung selbst wiederherstellen ist keine Option in diesem Fall.


![Behebung] (./media/active-directory-identityprotection-flows/104.png "Behebung")



 
## <a name="reset-password"></a>Kennwort zurücksetzen

Gefährdete Benutzer blockiert anmelden, kann Administrator für sie ein temporäres Kennwort generieren. Die Benutzer müssen ihr Kennwort beim nächsten Anmelden zu ändern.

![Behebung] (./media/active-directory-identityprotection-flows/160.png "Behebung")


 




 

## <a name="see-also"></a>Siehe auch

- [Azure Active Directory-Schutz](active-directory-identityprotection.md) 