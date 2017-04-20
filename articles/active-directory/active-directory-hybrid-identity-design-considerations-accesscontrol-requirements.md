
<properties
    pageTitle="Azure Active Directory hybride Identität Design - bestimmen zugriffsanforderungen | Microsoft Azure"
    description="Behandelt die Säulen und Identifizieren von Ressourcen für Benutzer in einer hybridumgebung erforderlich."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Anforderungen Sie Access Control für Ihre Identität Hybrid-Lösung
Beim Entwerfen ihrer hybrididentitätslösung einer Organisation können sie auch diese Gelegenheit nutzen, erforderlich für Ressourcen zu überprüfen, die sie für Benutzer verfügbar machen möchten. Datenzugriff Schneidet alle vier Säulen der Identität, die:

- Verwaltung
- Authentifizierung
- Autorisierung
- Überwachung

Authentifizierung und Autorisierung ausführlicher in den folgenden Abschnitten behandelt, Verwaltung und Überwachung der Hybrid Identity Lifecycle. Finden Sie weitere Informationen zu diesen Funktionen [bestimmen Hybrid Identity Management-Aufgaben](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) .

>[AZURE.NOTE]
Weitere Informationen über jeweils die Säulen finden Sie [Die vier Säulen der Identität - Identity Management in ALTER hybride IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) .

## <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung
Verschiedene Szenarien für Authentifizierung und Autorisierung, diese Szenarien müssen bestimmte Vorschriften die hybrididentitätslösung erfüllen müssen, die das Unternehmen erlassen wird. Szenarien Business to Business (B2B) Kommunikation können eine zusätzliche Herausforderung für IT-Administratoren hinzugefügt werden, da sie müssen sicherstellen, dass die Authentifizierung und Autorisierung Methode der Organisation mit Geschäftspartnern kommunizieren kann. Sicherzustellen Sie bei der Gestaltung für Authentifizierung und Autorisierung, dass die folgenden Fragen beantwortet werden:

- Ihrer Organisation authentifiziert und autorisiert nur Benutzer ihre Identität Management System?
 - Gibt es Pläne für B2B-Szenarien?
 - Wenn Ja, wissen Sie bereits, welche Protokolle (SAML, OAuth, Kerberos, Token oder Zertifikate) Verbindung beider Unternehmen verwendet werden?
- Unterstützt hybrididentitätslösung, das Unterstützung erlassen werden diese Protokolle?

Ein weiterer wichtiger Punkt ist, Authentifizierung Repository, das von Benutzern und Partnern verwendet gespeichert werden und das Verwaltungsmodell verwendet werden. Berücksichtigen Sie die folgenden beiden Optionen:
- Zentrale: in diesem Modell des Benutzers Anmeldeinformationen, Richtlinien und Verwaltung kann zentrale lokal oder in der Cloud.
- Hybrid: in diesem Modell des Benutzers Anmeldeinformationen, Richtlinien und Verwaltung lokal zentrale und einer replizierten in der Cloud werden.

Welches Modell der Organisation übernehmen nach seinen Geschäftserfordernissen variiert, beantworten Sie folgende Fragen, Wohnsitz Identity Managementsystem und den Administratormodus verwendet werden soll:

- Ihre Organisation derzeit besitzt eine Identitätsmanagement vor Ort?
 - Wenn Ja, möchten sie es beibehalten?
 - Gibt es eine Verordnung oder Compliance, dass Ihre Organisation sich Identity Managementsystem befinden sollte, bestimmt folgen?
- Verwendet Unternehmens-auf lokale apps befindet oder in der Cloud?
 - Wenn Ja, wirkt die Annahme einer Identität Hybridmodell dabei?

## <a name="access-control"></a>Zugriffskontrolle
Authentifizierung und Autorisierung Kernelemente ermöglichen Zugriff auf Unternehmensdaten durch Überprüfung des Benutzers sind, unbedingt auch die Zugriffsstufe steuern, die Benutzer und die Ebene der Administratoren über die Ressourcen, die sie verwalten müssen. Die hybrididentitätslösung muss präzise Zugriff auf Ressourcen und Delegierung Basiszugriff steuern können. Sicherstellen Sie, dass die folgende Frage beantwortet werden zur Zugriffskontrolle:

- Hat Ihr Unternehmen mehrere Benutzer mit erhöhten Rechten zur Verwaltung Ihres Systems Identität?
 - Wenn Ja, benötigt jeder Benutzer die gleiche Zugriffsebene?
- Muss Ihr Unternehmen Zugriff auf bestimmte Ressourcen verwaltet übertragen?
 - Wenn Ja, wie häufig dies geschieht?
- Würde Ihr Unternehmen müssen Access Control-Funktionen zwischen lokalen und integrieren Ressourcen?
- Ihr Unternehmen Zugriff auf Ressourcen entsprechend den Umständen beschränken muss?
- Hätte Ihr Unternehmen jeder Anwendung, die benutzerdefinierte Steuerelement Zugriff auf einige Ressourcen?
 - Falls Ja, wo diese apps befinden (lokal oder in der Cloud)?
 - Falls Ja, wo sind die Ziel Ressourcen (lokal oder in der Cloud)?

>[AZURE.NOTE]
Stellen Sie sicher zu Notizen jeder Antwort die Gründe für die Antwort. [Datenschutzstrategie definieren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) gehen über die verfügbaren Optionen und die vor-und Nachteile jeder Option.  Diese Fragen müssen Sie auswählen, welche Option am besten den Bedürfnissen Ihres Unternehmens passt.

## <a name="next-steps"></a>Nächste Schritte

[Reaktion auf Sicherheitsvorfälle ermitteln](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Siehe auch
[Design Considerations (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)
