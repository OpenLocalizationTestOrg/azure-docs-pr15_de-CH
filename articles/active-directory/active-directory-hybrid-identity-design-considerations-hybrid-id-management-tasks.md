<properties
    pageTitle="Azure Active Directory Hybrid-Identität Entwurfsaspekte - Hybrid Identity Management-Aufgaben bestimmen | Microsoft Azure"
    description="Mit bedingten Zugriffskontrolle überprüft Azure Active Directory die besonderen Umständen, wenn Benutzer authentifizieren, bevor Zugriff auf die Anwendung auswählen. Die Suchkriterien erfüllt, wird der Benutzer authentifiziert und Zugriff auf die Anwendung."
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

# <a name="plan-for-hybrid-identity-lifecycle"></a>Hybride Identity Lifecycle planen 

Identität ist eines der Fundamente der Mobilität und Anwendung Zugriff Unternehmensstrategie. Ob auf Ihrem mobilen Gerät oder SaaS-app Anmelden ist Ihre Identität die Taste, um den Zugriff auf alles. Auf der höchsten Ebene umfasst eine Lösung zur Identität Vereinheitlichung und synchronisieren Ihre Identität Repositories automatisieren und den Prozess der Bereitstellung von Ressourcen zentralisiert. Identitätslösung sollte eine zentrale Identität über lokalen und Cloud und auch gewisse Identitätsverbund verwalten zentralisierten Authentifizierung und sichere Freigabe und die Zusammenarbeit mit externen Benutzern und Unternehmen. Ressourcen reichen von Betriebssystemen und Anwendungsprogrammen Personen in oder mit einer Organisation zugeordnet. Die Struktur kann die Bereitstellung Richtlinien und Verfahren geändert werden.

Es ist auch eine Identität Lösung ausgerichtet, um Benutzer mit Self-service-Erlebnis zu produktiv zu wichtig. Ihre identitätslösung ist robuster sie einmaliges Anmelden für Benutzer für alle Ressourcen ermöglicht Administratoren Zugang benötigten Ebenen standardisierte Verfahren verwenden können für die Verwaltung von Anmeldeinformationen. Einige Verwaltungsebenen reduziert oder beseitigt, je nach Umfang der Bereitstellung Management-Lösung. Darüber hinaus können Sie sichere Verwaltungsfunktionen, manuell oder automatisch zwischen verschiedenen Organisationen verteilen. Ein Domänenadministrator kann z. B. nur die Personen und Ressourcen in dieser Domäne dienen. Dieser Benutzer Verwaltung und Bereitstellung Aufgaben ausführen, jedoch nicht für Konfiguration, wie Erstellen von Workflows Aufgaben autorisiert.


## <a name="determine-hybrid-identity-management-tasks"></a>Bestimmen der Hybrid Identity Management-Aufgaben
Verteilen von Verwaltungsaufgaben in der Organisation verbessert die Genauigkeit und Effizienz der Verwaltung und den Saldo der Arbeitslast einer Organisation verbessert. Im folgenden werden bestimmt, die eine stabile Identität Managementsystem definieren.

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)


Hybride Identität Aufgaben definieren, müssen Sie einige wichtige Merkmale der Organisation verstehen, die Hybrid-Identität annehmen. Es ist wichtig zu verstehen, die aktuelle Repositories Identität Quellen verwendet. Die Kernelemente kennen, müssen die grundlegenden Vorschriften und anhand dieser detailliertere Fragen, die Sie für Ihre identitätslösung eine bessere Entscheidung zu müssen.  

Definieren Sie diese Vorschriften, sicher, dass mindestens die folgenden Fragen

- Optionen-Bereitstellung: 
 - Unterstützt die hybrididentitätslösung eine robuste Zugriff Kontenverwaltung und Bereitstellungssystem?
 - Wie werden Benutzer, Gruppen und Kennwörter verwaltet werden?
 - Reagiert das Identity Lifecycle Management? 
      - Wie lange dauert Kennwort Updates Sperrung?
      
- Verwaltung: 
 - Ist hybride Identität Lösung behandelt Lizenzmanagement?
     - Wenn Ja, welche Funktionen verfügbar sind?
- Behandelt die Lösung gruppenbasierten Verwaltung? 
      -Wenn Ja, ist es möglich, eine Sicherheitsgruppe zuweisen? 
       -Wenn Ja, wird das cloudverzeichnis alle Mitglieder der Gruppe automatisch Lizenzen zuweisen? 
        -Was passiert, wenn ein Benutzer später hinzugefügt oder aus der Gruppe entfernt, wird eine Lizenz automatisch zugewiesen oder gegebenenfalls entfernt? 

- Integration mit Drittanbieter-Identität:
- Integrierbar diese Hybrid-Lösung mit Drittanbietern Identitätsprovider einmaliges implementieren?
- Ist es möglich, alle anderen Identitätsanbieter in einem zusammenhängenden Identitätssystem vereinheitlichen?
- Wenn Ja, wie sie und Funktionen sind?

## <a name="synchronization-management"></a>Synchronisierung Management
Eines der Ziele von Identitätsmanager können alle Identitätsprovider und halten sie synchronisiert. Die Daten synchronisiert einen autorisierenden master Identitätsanbieter Grundlage. In einem Szenario mit Identität hybrider mit synchronisierten Verwaltungsmodell alle Benutzer- und Identitäten auf einem lokalen Server verwalten und Synchronisieren der Konten und optional Kennwörter in die Cloud. Benutzer die gleiche Kennwort lokalen er oder sie ist in der Cloud und zur Anmeldung das Kennwort der Identität Lösung bestätigt. Dieses Modell verwendet eine Directory-Synchronisierungstools.
 
![](./media/hybrid-id-design-considerations/Directory_synchronization.png)Ordnungsgemäße Entwurf die Synchronisierung Ihrer Identität Hybrid-Lösung sicherzustellen, dass die folgenden Fragen beantwortet werden: • die Synchronisierung Lösungen für die hybrididentitätslösung verfügbar?
• Was Einmalanmeldung Funktionen verfügbar sind?
• Was sind die Optionen für Identitätsverbund zwischen B2B und B2C?

## <a name="next-steps"></a>Nächste Schritte
[Hybride Identity Management Annahme Strategie](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)


## <a name="see-also"></a>Siehe auch
[Design Considerations (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)

