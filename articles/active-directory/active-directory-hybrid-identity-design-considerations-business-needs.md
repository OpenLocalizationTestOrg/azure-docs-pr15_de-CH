<properties
    pageTitle="Azure Active Directory hybride Identität Design - bestimmen Identität Anforderungen | Microsoft Azure"
    description="Identifizieren des Unternehmens Bedarf, die für die Identität Verstauvarianten Anforderungen führen."
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

# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Identität für Ihre Lösung hybride Identität ermitteln
Der erste Schritt beim Entwerfen einer Identität Hybrid-Lösung ist für die Organisation bestimmen, die diese Lösung nutzen.  Hybrid-Identität beginnt als Rolle (unterstützt alle Cloudlösungen von Authentifizierung) und auf neue und interessante Funktionen bieten, die neue Arbeitslasten für Benutzer zu entsperren.  Diese Arbeitslasten oder Dienste, die Sie für Ihre Benutzer übernehmen wollen bestimmen die Vorschriften für die Identität Verstauvarianten.  Diese Dienste und Workloads müssen Hybrid-Identität sowohl lokal nutzen und in der Cloud.  

Müssen Sie diese wichtige Aspekte des Unternehmens zu verstehen, was eine Anforderung jetzt und was für die Zukunft geplant. Haben Sie die Sichtbarkeit der langfristigen Strategie für Identität Verstauvarianten, sind Chancen, Ihrer Lösung Unternehmen muss und nicht skalierbar sein.   T er Diagramm zeigt ein Beispiel für eine hybride identitätsarchitektur und die Arbeitslasten für Benutzer freigegeben werden. Dies ist nur ein Beispiel für die neuen Möglichkeiten, die entsperrt und solide hybride Identität Strategie übermittelt werden. 
 
Einige Komponenten Bestandteil der Hybrid-Identität sind![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Ermitteln der Geschäftsbedürfnisse
Jedes Unternehmen haben Anforderungen, auch wenn diese Unternehmen Teil derselben Branche echten geschäftlichen Vorschriften abweichen. Best Practices der Branche können weiterhin nutzen, aber letztendlich ist Bedarf des Unternehmens, die für die Identität Verstauvarianten Anforderungen führen. 

Stellen Sie sicher die folgenden Fragen, um Ihren Bedarf zu ermitteln:

- Sucht Ihr Unternehmen IT-Betriebskosten Ausschneiden?
- Sucht Ihr Unternehmen Cloud (SaaS-apps, Infrastruktur) schützen?
- Sucht Ihr Unternehmen Ihre IT modernisieren?
  - Sind Ihre Benutzer mobile und anspruchsvolle IT-Ausnahmen in Ihrer DMZ Arten von Datenverkehr auf verschiedene Ressourcen zu erstellen?
  - Verfügt Ihr Unternehmen Legacyanwendungen, die notwendig auf diese moderne Benutzer veröffentlicht werden aber nicht einfach neu schreiben?
  - Muss Ihr Unternehmen diese Aufgaben ausführen und gleichzeitig unter Kontrolle bringen?
- Sucht Ihr Unternehmen sichern Benutzeridentität und geringere Risiken mit neuen Tools, die Nutzung die Erfahrung von Microsoft Azure Security Expertenwissen vor Ort?
- Ist Ihr Unternehmen versucht der gefürchteten "externen" Konten lokal und in der Cloud nicht inaktiv Bedrohung in Ihrer lokalen Umgebung können verschieben?

## <a name="analyze-on-premises-identity-infrastructure"></a>Lokale Identitätsinfrastruktur analysieren
Jetzt haben Sie eine Idee zu Unternehmen Bedarf müssen Sie Ihre lokalen Identitätsinfrastruktur auswerten. Diese Bewertung ist wichtig für die technischen Vorschriften der aktuellen Identität Lösung Cloud Identity Management System definieren. Stellen Sie sich folgende Fragen:

- Welche Authentifizierung und Autorisierung Ihrer Firma wird lokal verwenden? 
- Verfügt Ihr Unternehmen derzeit lokalen Synchronisierungsdienste?
- Verwendet Ihr Unternehmen Identität Drittanbietern (IdP)?

Sie müssen auch die Cloud-Services, die Ihr Unternehmen möglicherweise. Durchführen einer Bewertung die aktuelle Integration SaaS IaaS und PaaS-Modelle in Ihrer Umgebung zu verstehen, ist sehr wichtig. Vergewissern Sie sich im Rahmen dieser Bewertung die folgenden Fragen beantworten:
- Hat Ihr Unternehmen eine Integration mit Cloud-Dienstanbieter?
- Wenn Ja, werden die Dienste verwendet?
- Diese Integration in der Produktion befindet, oder ist ein Pilot?


>[AZURE.NOTE]
Genaue Zuordnung Ihrer Apps und cloud-Services, können Sie das Cloud App Discovery-Tool. Dieses Tool kann Ihre IT-Abteilung Sichtbarkeit Ihres Unternehmens und Consumer Cloud apps bereitstellen. Das macht es einfacher als je zuvor zu entdecken Schatten IT in der Organisation, einschließlich Details zu Verwendungsmuster und Benutzer Zugriff auf Ihre Cloudanwendungen. Zugriff auf dieses Tool Gehe zu [https://appdiscovery.azure.com](https://appdiscovery.azure.com/)

## <a name="evaluate-identity-integration-requirements"></a>Identity integrationsanforderungen bewerten
Anschließend müssen Sie Identity integrationsanforderungen bewerten. Dabei ist wichtig, die technischen Vorschriften für wie Benutzerauthentifizierung, Darstellung der Organisation Präsenz in der Cloud, wie die Organisation Autorisierung kann und was die Benutzerfunktionalität wird. Stellen Sie sich folgende Fragen:

- Wird Ihre Organisation Föderation und/oder Standardauthentifizierung verwenden?
- Ist Föderation erforderlich?  Durch die folgende:
 - Die Kerberos-SSO
 - Ihr Unternehmen hat eine lokale entwickelte (entweder intern oder 3rd Party), das SAML oder ähnliche Funktionen Föderation verwendet.
 - MFA über Smartcards. RSA SecurID usw.
 - Client-Zugriffsregeln, die folgenden Fragen:
     1. Können alle externen Zugriff auf Office 365 basierend auf der IP-Adresse des Clients blockieren?
     1. Können alle externen Zugriff auf Office 365 außer Exchange ActiveSync blockieren?
     1. Alle externen Zugriff auf Office 365 außer browserbasierte apps (OWA, SPO) können blockieren
     1. Ich kann alle externen Zugriff auf Office 365 für bestimmte Active Directory-Gruppen Mitglieder blockieren
- Probleme Sicherheit Prüfung
- Bereits vorhandene Investitionen in Verbundauthentifizierung
- Verwenden Name wird unserer Organisation für unsere Domäne in der Cloud?
- Verfügt die Organisation eine benutzerdefinierte Domäne?
    1. Ist dieser Domäne öffentliche und problemlos überprüfbar über DNS?
    1. Wenn nicht müssen öffentlichen Domäne Sie, mit der eine alternative UPN im Active Directory registrieren?
- Sind die Benutzer-IDs für Cloud-Darstellung 
- Verfügt die Organisation apps, die Integration mit Cloud-Diensten benötigen?
- Verfügt die Organisation über mehrere Domänen und verwenden alle Standard- oder verbundenen Authentifizierung?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Bewerten Sie Programme, die in Ihrer Umgebung
Wissen über Ihre lokalen und cloud-Infrastruktur müssen Sie die Anwendung auswerten, die in dieser Umgebung ausgeführt. Dabei ist wichtig, die technischen Vorschriften Anwendung Cloud Identity Management System zu integrieren. Stellen Sie sich folgende Fragen:

- Wo wohnen unserer?
- Werden Benutzer lokale Anwendung zugreifen?  In der Cloud? Oder beides?
- Gibt es Pläne zu den vorhandenen Arbeitslasten in die Cloud verschieben?
- Sind neue Anwendungsmöglichkeiten, die entweder lokal gespeichert oder in der Cloud, mit dem cloud-Authentifizierung?

## <a name="evaluate-user-requirements"></a>Anforderungen bewerten
Sie haben auch die erforderlichen Benutzer ausgewertet. Dabei ist wichtig, die Schritte definieren, die Integration und Unterstützung von Benutzern Übergang in die Cloud. Stellen Sie sich folgende Fragen:

- Werden Benutzer Programme lokal zugreifen?
- Werden Benutzer in die Cloud-Anwendung zugreifen?
- Wie Benutzer in der Regel werden bei ihrer lokalen Umgebung?
- Wie werden Benutzer in die Cloud anmelden?

>[AZURE.NOTE]
Stellen Sie sicher zu Notizen jeder Antwort die Gründe für die Antwort. [Bestimmen Vorfällen Vorschriften](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) gehen über die verfügbaren Optionen und die vor-und Nachteile gegenüber jeder Option.  Durch die Beantwortung dieser Fragen wählen Sie welche Option am besten Ihr Unternehmen passt benötigt.

## <a name="next-steps"></a>Nächste Schritte
[Directory Synchronization Anforderungen](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Siehe auch
[Design Considerations (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)
