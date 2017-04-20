<properties
    pageTitle="Azure Active Directory hybride Identität Design - bestimmen Content Management-Anforderungen | Microsoft Azure"
    description="Einblick in das Content-Management des Unternehmens bestimmen. Normalerweise haben Wenn Benutzer eigene Gerät hat er auch mehrere Anmeldeinformationen, die die Anwendung wechseln, die er verwendet. Es ist wichtig zu unterscheiden, welche Inhalte mit Authentifizierung gegenüber der erstellten Anmeldeinformationen der Firma erstellt wurde. Sollte Ihre identitätslösung mit Cloud Dienste problemlos für den Endbenutzer beim seine Privatsphäre und der Schutz gegen Datenverlust."
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

# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Ermitteln Sie Content-Management für Ihre Lösung hybride Identität

Die Content-Management Anforderungen für Ihr Unternehmen beeinträchtigen direkt bei der Entscheidung über die hybrididentitätslösung verwenden. Die Verbreitung von Geräten und die Möglichkeit der Benutzer ihre eigenen Geräte ([BYOD](http://aka.ms/byodcg)) muss muss das Unternehmen seine eigenen Daten schützen aber es auch Datenschutz intakt. Normalerweise haben Wenn Benutzer eigene Gerät hat er auch mehrere Anmeldeinformationen, die die Anwendung wechseln, die er verwendet. Es ist wichtig zu unterscheiden, welche Inhalte mit Authentifizierung gegenüber der erstellten Anmeldeinformationen der Firma erstellt wurde. Sollte Ihre identitätslösung mit Cloud Dienste problemlos für den Endbenutzer beim seine Privatsphäre und der Schutz gegen Datenverlust. 

Ihre identitätslösung wird durch andere technische Kontrollen genutzt werden, um Content Management bieten wie in der folgenden Abbildung dargestellt:
 
![](./media/hybrid-id-design-considerations/securitycontrols.png)

**Sicherheitsmaßnahmen, die die Identity-Management-System nutzen**

Im Allgemeinen nutzen Content Management-Anforderungen, Ihr Identity Management-System in folgenden Bereichen:

- Datenschutz: den Benutzer identifizieren, die eine Ressource und die entsprechenden Kontrollen um Integrität besitzt.
- Klassifizierung: identifiziert die Benutzer oder Gruppe und Zugriff auf ein Objekt nach seiner Klassifizierung. 
- Datenlecks Datenschutz: Sicherheitsmaßnahmen für den Schutz von Daten zur Vermeidung von Datenlecks müssen mit der Identität überprüft die Identität des Benutzers interagieren. Dies gilt auch für Prüfung Weg.

>[AZURE.NOTE]
[Klassifizierung für Cloud Readiness](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) für Weitere Informationen zu bewährten Methoden und Richtlinien für die Klassifizierung von Daten zu lesen.

Beim Planen Ihrer hybrididentitätslösung sicherstellen, dass nach den Erfordernissen Ihrer Organisation die folgenden Fragen beantwortet werden:

- Verfügt Ihr Unternehmen Sicherheitskontrollen zu Datenschutz erzwingen?
 - Wenn Ja, werden diese Sicherheitskontrollen Hybrid Identity-Lösung integrieren, die Sie erlassen werden können?
- Verwendet Ihr Unternehmen Daten?
 - Wenn Ja, kann die aktuelle Projektmappe Hybrid Identity-Lösung integrieren, die Sie erlassen werden?
- Verfügt Ihr Unternehmen derzeit keine Lösung für Daten? 
 - Wenn Ja, kann die aktuelle Projektmappe Hybrid Identity-Lösung integrieren, die Sie erlassen werden?
- Muss Ihr Unternehmen den Zugriff auf Ressourcen überwachen?
 - Wenn Ja, welche Art von Ressourcen?
 - Wenn Ja, welche Informationen erforderlich ist?
 - Falls Ja, wo muss das Überwachungsprotokoll befinden? Lokal oder in der Cloud?
- Muss Ihr Unternehmen e-Mails zu verschlüsseln, die vertrauliche Daten (Sozialversicherungsnummern, Kreditkartennummern usw.)?
- Muss Ihr Unternehmen alle Dokumente/gemeinsam mit externen Geschäftspartnern verschlüsseln?
- Muss Ihr Unternehmen Unternehmensrichtlinien auf bestimmte e-Mails erzwingen (keine allen Antworten führen, nicht weiterleiten)?
 
>[AZURE.NOTE]
Stellen Sie sicher zu Notizen jeder Antwort die Gründe für die Antwort. [Datenschutzstrategie definieren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) gehen über die verfügbaren Optionen und die vor-und Nachteile jeder Option.  Durch die Beantwortung dieser Fragen wählen Sie welche Option am besten Ihr Unternehmen passt benötigt.


## <a name="next-steps"></a>Nächste Schritte
[Access Control Anforderungen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Siehe auch
[Design Considerations (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)
