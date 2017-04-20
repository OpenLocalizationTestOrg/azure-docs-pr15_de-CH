<properties
    pageTitle="Azure Active Directory hybride Identität Vorüberlegungen - Übersicht | Microsoft Azure"
    description="Übersicht und Content Map Hybrid-Identität Design Considerations Guide"
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

# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Vorüberlegungen zum Azure Active Directory Hybrid-Identität

Consumer-Geräte sind, nimmt die Wirtschaft und cloudbasierten Software als Service (SaaS) Applikationen sind leicht zu erlassen. Daher stellt die Kontrolle der Benutzer Zugriff auf interne Rechenzentren und Cloud-Plattformen.  Microsoft Identity Solutions umfassen lokalen und Cloud-basierte Funktionen nur eine Benutzeridentität für Authentifizierung und Autorisierung für alle Ressourcen, unabhängig vom Standort erstellen. Wir bezeichnen diese Hybrid-Identität. Gibt es anderes Design und Konfigurationsoptionen für hybride Identität mit Microsoft und in manchen Fällen ist es schwierig zu bestimmen, welche Kombination am besten den Bedürfnissen Ihrer Organisation. Diese hybride Identität Design Considerations Guide hilft Ihnen zu verstehen, wie eine hybride Identität Lösung, die besten Unternehmen und Technologie für Ihre Organisation benötigt.  Dabei beschreiben eine Reihe von Schritten und Aufgaben, die Sie folgen können, um Ihnen eine hybride Identität Lösung, die Ihrer Organisation Anforderungen erfüllt zu. Schritte und Aufgaben präsentieren das Handbuch Technologien und Funktion Optionen für Unternehmen funktionale und Service-Qualität (z. B. Verfügbarkeit, Skalierbarkeit, Leistung, Verwaltung und Sicherheit) Anforderungen. Hybride Identität Design Considerations Guide Ziele sind die folgenden Fragen beantworten: 

- Welche Fragen muss ich stellen und beantworten Verstauvarianten Identity-spezifische Technologie oder Problem Domäne, am besten mein erfüllt Laufwerk?
- Welche Abfolge von Aktivitäten sollte ein Hybrid-Identität für die Technologie oder Problem Domäne zu Lösung abgeschlossen? 
- Hybride Identität Technologie und Konfiguration Optionen stehen Hilfe Meine erfüllen? Was sind die Kompromisse zwischen diesen Optionen, damit ich die beste Option für mein Unternehmen auswählen können


## <a name="who-is-this-guide-intended-for"></a>Die richtet diesem Handbuch?
 CIO, CITO, Chief Identität Architekten, Enterprise Architects und IT-Architekten für eine hybride Identität Lösung für mittlere und große Organisationen verantwortlich.

## <a name="how-can-this-guide-help-you"></a>Wie kann Sie dabei unterstützen? 
Dabei können Sie verstehen, wie eine hybride Identität Lösung, die mit der aktuellen lokalen Identität eine cloudbasierte Identity Management-System integrieren. Die folgende Grafik zeigt ein Beispiel für eine hybride identitätslösung IT Admins verwalten ihrer aktuellen Windows Server Active Directory Lösung lokal mit Microsoft Azure Active Directory Benutzer anwendungsübergreifend befindet sich in der Cloud und lokal anmelden (SSO) verwenden können.

![](./media/hybrid-id-design-considerations/hybridID-example.png)


Die Abbildung zeigt eine hybrididentitätslösung, die Nutzung von Cloud-Diensten mit lokalen eine Endbenutzer-Authentifizierungsprozess einzelne bereitzustellen und zu integrieren IT Management dieser Ressourcen. Obwohl dies ein sehr gängiges Szenario ist, ist jedes Unternehmen Identität Verstauvarianten wahrscheinlich als Beispiel in Abbildung 1 auf unterschiedlichen. Dieses Handbuch bietet eine Reihe von Schritten und Aufgaben folgen können eine hybride Identität Lösung, die Ihrer Organisation Anforderungen erfüllt. Folgende Schritte und Aufgaben stellt das Handbuch Technologien und Optionen Funktion funktionsfähig und Service Quality Level-Bedürfnisse für Ihre Organisation zu.

**Annahmen**: Sie haben einige Erfahrung mit Windows Server, Active Directory Domain Services und Azure Active Directory. In diesem Dokument wird davon ausgegangen, wie diese Lösungen Anforderungen Ihres Unternehmens oder in einer integrierten Lösung können suchen.

## <a name="design-considerations-overview"></a>Design Considerations (Übersicht)
Dieses Dokument enthält eine Reihe von Schritten und Aufgaben, die eine hybride Identität Lösung, die Ihren Anforderungen entsprechen können. Die Schritte werden in einer bestimmten Reihenfolge angezeigt. Vorüberlegungen erfahren Sie in späteren Schritten müssen Sie Entscheidungen ändern Sie in früheren Schritten jedoch durch in Konflikt stehende Designoptionen. Jeder versucht, Konflikte im Dokument Design informiert. 

Der Entwurf erreichen Sie, dass am besten Ihren Anforderungen nur nach dem Durchlaufen der Schritte so oft aus, um alle Aspekte in das Dokument integrieren. 

| Hybride Identität Phase                                             | Themenliste                                                                                                                                                                                       |
|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Identität ermitteln                                   | [Ermitteln der Geschäftsbedürfnisse](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [Directory Synchronization Anforderungen](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Mehrstufige Authentifizierung ermitteln](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Definieren Sie eine hybride Identität Annahme Strategie](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)                       |
| Plan zur Verbesserung der Sicherheit durch starke identitätslösung | [Bestimmen des Datenschutzes](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [Content Management ermitteln](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Access Control Anforderungen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Reaktion auf Sicherheitsvorfälle ermitteln](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Definieren der Strategie für den Datenschutz](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)  |
| Plan für Hybrid Identity lifecycle                                | [Bestimmen der Hybrid Identity Management-Aufgaben](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Synchronisierung Management](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Hybride Identity Management Annahme Strategie](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |     


##<a name="download-this-guide"></a>Dieses Handbuch herunterladen
Sie können eine PDF-Version des Handbuchs hybride Identität Vorüberlegungen [Technet Gallery](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288). 

                                                             
