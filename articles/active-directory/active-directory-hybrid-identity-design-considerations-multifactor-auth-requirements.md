<properties
    pageTitle="Azure bestimmen Active Directory hybride Identität Design - mehrstufige Authentifizierung"
    description="Mit bedingten Zugriffskontrolle überprüft Azure Active Directory die besonderen Umständen, wenn Benutzer authentifizieren, bevor Zugriff auf die Anwendung auswählen. Die Suchkriterien erfüllt, wird der Benutzer authentifiziert und Zugriff auf die Anwendung."
    documentationCenter=""
    services="active-directory"
    authors="femila"
    manager="billmath"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Anforderungen Sie mehrstufige Authentifizierung für die hybrididentitätslösung

In dieser Welt der Mobilität mit Benutzern den Zugriff auf Daten und in der Cloud und von jedem Gerät diese Informationen größter geworden.  Jeden Tag gibt es eine neue Überschrift zu einem.  Zwar gibt es keine Garantie gegen diese Verstöße, bietet mehrstufige Authentifizierung eine zusätzliche Sicherheitsebene für diese Verstöße zu verhindern.
Bewertung erforderlichen Organisationen für die kombinierte Authentifizierung starten. Also, was Organisation versucht sichern.  Dabei ist wichtig, die technischen Vorschriften für einrichten und aktivieren die Benutzer Organisationen für mehrstufige Authentifizierung.

>[AZURE.NOTE]
Wenn Sie nicht vertraut mit MFA und was es tut, wird dringend empfohlen, Sie den Artikel lesen [Neuigkeiten Azure mehrstufige Authentifizierung?](../multi-factor-authentication/multi-factor-authentication.md) vor dem Lesen dieses Abschnitts.

Stellen Sie sicher die folgenden beantworten:

- Versucht Ihr Unternehmen Microsoft apps sichern? 
- Wie werden diese apps veröffentlicht?
- Bietet Ihr Unternehmen RAS Mitarbeiter zu lokalen apps zugreifen können?

Wenn Ja, welche Art von RAS? Außerdem müssen bewerten, Benutzern Zugriff auf diese Anwendung gespeichert werden. Diese Bewertung ist ein weiterer wichtiger Schritt die richtigen mehrstufige Authentifizierungsstrategie definieren. Stellen Sie sich folgende Fragen:

- Wo werden die Benutzer befinden?
- Können sie überall befinden?
- Möchte Ihr Unternehmen Einschränkungen nach Standort des Benutzers herstellen?

Sobald diese Anforderungen unbedingt auch Vorschriften für mehrstufige Authentifizierung des Benutzers ausgewertet. Dabei ist wichtig, da an mehrstufige Authentifizierung Rollout definiert werden. Stellen Sie sich folgende Fragen:

- Sind die Benutzer mit mehrstufige Authentifizierung?
- Erforderlich einige Verwendungsmöglichkeiten für zusätzliche Authentifizierung?  
 - Wenn Ja, immer wenn aus externen Netzwerken oder den Zugriff auf bestimmte Programme oder unter anderen Umständen?
- Benötigen die Benutzer Schulung zum Einrichten und mehrstufige Authentifizierung?
- Was sind wichtige Szenarios, die Ihr Unternehmen mehrstufige Authentifizierung für Benutzer aktivieren möchte?

Nach der vorherigen Fragen werden Sie verstehen, gibt es mehrstufige Authentifizierung bereits lokal. Dabei ist wichtig, die technischen Vorschriften für einrichten und aktivieren die Benutzer Organisationen für mehrstufige Authentifizierung. Stellen Sie sich folgende Fragen:

- Muss Ihr Unternehmen privilegierte Konten mit MFA schützen?
- Muss Ihr Unternehmen für bestimmte Anwendung aus Compliance-Gründen MFA aktivieren?
- Muss Ihr Unternehmen MFA für alle berechtigten Benutzer dieser Anwendung oder nur Administratoren aktivieren?
- Haben Sie müssen MFA immer aktiviert oder nur, wenn der Benutzer außerhalb des Firmennetzwerks angemeldet sind?


## <a name="next-steps"></a>Nächste Schritte
[Definieren Sie eine hybride Identität Annahme Strategie](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)


## <a name="see-also"></a>Siehe auch
[Design Considerations (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)
