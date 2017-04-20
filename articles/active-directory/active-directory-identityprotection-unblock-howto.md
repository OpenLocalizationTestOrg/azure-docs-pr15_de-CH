<properties
    pageTitle="Azure Active Directory Schutz - Benutzer entsperren | Microsoft Azure"
    description="Erfahren Sie, wie durch eine Azure Active Directory Schutz blockiert wurden Benutzer zulassen."
    services="active-directory"
    keywords="Identitätsschutz Azure active Directory Sperre aufheben"
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
    ms.date="09/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory Schutz - Benutzer entsperren

Mit Azure Active Directory Schutz konfigurieren Sie Richtlinien für Benutzer blockieren, wenn die konfigurierten Kriterien erfuellt sind. In der Regel einen blockierten Benutzer Kontakte Helpdesk aufgehoben werden. Themen erläutert die Schritte, die Sie ausführen können, um blockierte Benutzer zulassen.


## <a name="determine-the-reason-for-blocking"></a>Bestimmen Sie den Grund für die Sperrung

Zunächst einen Benutzer aufheben müssen Sie die Richtlinie bestimmen, die den Benutzer blockiert hat, weil Ihre nächsten Schritte abhängig sind. Mit Azure Active Directory Schutz kann ein Benutzer entweder anmelden Risiko-Policy oder eine Benutzerrichtlinie Risiko blockiert. 

Den Typ der Richtlinie erhalten, die einen Benutzer aus der Überschrift im Dialogfeld blockiert, die dem Benutzer bei einem Anmeldeversuch angezeigt wurde:

|Richtlinie | Dialogfeld "Benutzer"|
|--- | --- |
|Anmelden Risiko | ![Blockierte anmelden](./media/active-directory-identityprotection-unblock-howto/02.png) |
|Risiko | ![Blockierte Konto](./media/active-directory-identityprotection-unblock-howto/104.png) |


Ein Benutzer, der blockiert wird:

- -In Risiko-Policy ist auch bekannt als verdächtige anmelden
- Eine Benutzerrichtlinie Risiko ist auch ein Konto gefährdet

 
## <a name="unblocking-suspicious-sign-ins"></a>Blockierung verdächtiger anmelden

Um verdächtige anmelden zu entsperren, müssen Sie die folgenden Optionen:

1. **Anmelden von einem bekannten Speicherort oder Gerät** - ein häufiger Grund blockiert verdächtige anmelden werden Anmeldeversuche von unbekannten Standorten oder Geräten. Benutzer können schnell bestimmen, ob diese Blockierung Grund Anmelden von einem bekannten Speicherort oder Gerät versucht.


3. **Richtlinie ausschließen** – Wenn Sie glauben, dass die aktuelle Konfiguration der Richtlinien für bestimmte Benutzer Probleme verursacht, können Sie die Benutzer davon ausschließen. [-In Risiko-Policy](active-directory-identityprotection.md#sign-in-risk-policy) für weitere Details anzeigen
 
4. **Richtlinie deaktivieren** - Wenn Sie glauben, dass die Konfiguration für alle Benutzer Probleme verursacht, können Sie die Richtlinie deaktivieren. [-In Risiko-Policy](active-directory-identityprotection.md#sign-in-risk-policy) für weitere Details anzeigen


## <a name="unblocking-accounts-at-risk"></a>Blockierung Konten gefährdet

Um ein Konto Risiko zu entsperren, müssen Sie die folgenden Optionen:

1. **Passwort** - Kennwort des Benutzers zurückgesetzt. Finden Sie weitere Einzelheiten [Manuelles Passwort zurücksetzen](active-directory-identityprotection.md#manual-secure-password-reset) .

2. Benutzer **schließen alle Risikoereignisse** - Benutzerrichtlinie Risiko blockiert wird, wenn der konfigurierte Benutzer Risiko für Sperrung erreicht. Einen Benutzer verringern die Risikostufe manuell schließen Risikoereignisse gemeldet. Weitere Informationen finden Sie unter [Risikoereignisse manuell schließen](active-directory-identityprotection.md#closing-risk-events-manually).

3. **Richtlinie ausschließen** – Wenn Sie glauben, dass die aktuelle Konfiguration der Richtlinien für bestimmte Benutzer Probleme verursacht, können Sie die Benutzer davon ausschließen. [Benutzerrichtlinie Risiko](active-directory-identityprotection.md#user-risk-policy) für weitere Details anzeigen
 
4. **Richtlinie deaktivieren** - Wenn Sie glauben, dass die Konfiguration für alle Benutzer Probleme verursacht, können Sie die Richtlinie deaktivieren. [Benutzerrichtlinie Risiko](active-directory-identityprotection.md#user-risk-policy) für weitere Details anzeigen




## <a name="next-steps"></a>Nächste Schritte

 Möchten Sie mehr über Azure AD-Schutz? [Azure Active Directory Schutz](active-directory-identityprotection.md)anzeigen
 

