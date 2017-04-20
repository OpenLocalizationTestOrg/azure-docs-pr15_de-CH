
<properties
    pageTitle="Azure Active Directory hybride Identität Design - bestimmen Vorfall rResponse Vorschriften | Microsoft Azure Vorschriften "
    description="Bestimmen Sie Überwachung und reporting-Funktionen für hybrididentitätslösung, die auf dem Computer können IT-Aktionen zu minimieren mögliche Risiken"
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

# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Anforderungen Sie Reaktion auf Sicherheitsvorfälle für die hybrididentitätslösung

Große oder mittlere Unternehmen wahrscheinlich haben eine [Sicherheitsstrategie](https://technet.microsoft.com/library/cc700825.aspx) hilft IT Aktionen entsprechend der Ebene des Vorfalls. Identity Managementsystem ist eine wichtige Komponente bei der Reaktion auf Sicherheitsvorfälle, da zu identifizieren, die eine bestimmte Aktion gegen das Ziel durchgeführt verwendet werden kann. Hybride identitätslösung muss monitoring und reporting-Funktionen bieten, die auf dem Computer können IT-Aktionen zu eine potentielle Bedrohung zu verringern. In einer normalen Sicherheitsstrategien müssen Sie die folgenden Phasen als Teil des Plans:

1.  Anfängliche Bewertung.
2.  Kommunikation von Vorfällen.
3.  Schadensbegrenzung und Risikominderung.
4.  Kennung des war Kompromiss Schweregrad.
5.  Beibehaltung der Beweise.
6.  Benachrichtigung an beteiligten Parteien.
7.  Wiederherstellung des Systems.
8.  Dokumentation.
9.  Bewertung von Schäden und Kosten.
10. Prozess und Plan Revision.

Kennung was gefährden und Schweregrad Phase war, werden die Systeme, die gefährdet, Dateien zu identifizieren, die zugegriffen wurde und Bestimmen der Vertraulichkeit dieser Dateien. Der Hybrid-Identitätssystem sollen diese erfüllen Sie die Identifizierung des Benutzers, der die Änderungen unterstützen. 

## <a name="monitoring-and-reporting"></a>Überwachung und Berichterstattung
Oft Identitätssystem können auch im ersten Bewertungsphase, vor allem, wenn das System erstellt hat, Überwachung und reporting-Funktionen. Während der anfänglichen Bewertung müssen IT Admin verdächtige Aktivitäten erkannt, oder System sollen Trigger automatisch basierend auf vorkonfigurierten Aufgabe. Viele Aktivitäten deutet auf einen möglichen Angriff aber in anderen Fällen ein fehlerhaft konfiguriertes System Anzahl fälschlich Intrusion Detection System führen kann. 

Das Identity Managementsystem sollte IT Admins zu identifizieren und die verdächtigen Aktivitäten unterstützen. Normalerweise können diese Bedürfnisse erfüllt werden alle monitoring und reporting-Funktionen, die Gefahren hervorgehoben werden kann. Mit der folgenden Fragen können Sie hybride Identität Lösung Berücksichtigung berücksichtigen Vorfällen Vorschriften:

- Hat Ihr Unternehmen eine standardisierte Sicherheitsstrategie vorhanden?
 - Wenn Ja, wird das aktuelle Identity Managementsystem als Teil des Prozesses verwendet?
- Müssen Ihr Unternehmen verdächtige anmelden Versuche von Benutzern auf verschiedenen Geräten?
- Muss Ihr Unternehmen potenziellen beeinträchtigte Anmeldeinformationen erkennen?
- Muss Ihr Unternehmen zugreifen und die Aktion des Benutzers überwachen?
- Muss Ihr Unternehmen wissen, wenn ein Benutzer sein Kennwort zurücksetzen?

## <a name="policy-enforcement"></a>Durchsetzen von Richtlinien

Schaden und Risiko Reduction-Phase unbedingt tatsächlichen und potenziellen Folgen eines Angriffs wird reduziert. Diese Aktion, die Sie machen den Unterschied zwischen einem Minderjährigen und einem großen an dieser Stelle. Die genaue Reaktion hängt von Ihrer Organisation und der Art des Angriffs bestehen. Wenn die anfängliche Bewertung ein Konto kompromittiert wurde zufolge, müssen Sie Richtlinie erzwingen, um dieses Konto zu sperren. Das ist nur ein Beispiel, in dem das Identity Managementsystem genutzt werden. Verwenden Sie die folgenden Fragen beim Entwerfen Ihrer hybrididentitätslösung berücksichtigen wie Richtlinien durchgesetzt werden, um einen laufenden Vorfall reagieren:

- Muss Ihr Unternehmen Richtlinien an Benutzer sperren Zugriff im Netzwerk ggf.?
 - Wenn Ja, integrieren die aktuelle Projektmappe Hybrid Identity Management System, die Sie erlassen werden?
- Muss Ihr Unternehmen bedingten Zugriff für Benutzer, die in Quarantäne erzwingen? 
 
>[AZURE.NOTE]
Stellen Sie sicher zu Notizen jeder Antwort die Gründe für die Antwort. [Definieren Datenschutzstrategie](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) gehen über die verfügbaren Optionen und die vor-und Nachteile jeder Option.  Durch die Beantwortung dieser Fragen wählen Sie welche Option am besten Ihr Unternehmen passt benötigt.

## <a name="next-steps"></a>Nächste Schritte
[Definieren der Strategie für den Datenschutz](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Siehe auch
[Design Considerations (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)
