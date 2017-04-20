
<properties
    pageTitle="Azure Active Directory hybride Identität Design - bestimmen Hybrid Identity Lifecycle Annahme Strategie | Microsoft Azure"
    description="Hybride Identity Management-Aufgaben gemäß Optionen für jede Lebenszyklusphase definieren können."
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


# <a name="determine-hybrid-identity-lifecycle-adoption-strategy"></a>Hybride Identity Lifecycle Annahme Strategie
Dabei definieren Sie die Identity Management-Strategie für Ihre hybrididentitätslösung den Bedürfnissen [bestimmen Hybrid Identity Management](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)Aufgaben definiert.


Hybrid Identity Management-Aufgaben nach End-to-End-Identity Lifecycle präsentiert in diesem Schritt definieren, müssen Sie die Optionen für jede Lebenszyklusphase.

## <a name="access-management-and-provisioning"></a>Verwaltung und Bereitstellung
Mit einem guten Zugriff kann Ihre Organisation genau verfolgen mit Zugriff auf Informationen innerhalb der Organisation.

Zugriffskontrolle ist eine wichtige Funktion ein zentralisiertes, zentralen Bereitstellungssystem. Neben dem Schutz sensibler Informationen setzen Zugriffskontrollen vorhandene Konten nicht genehmigte Berechtigungen oder sind nicht mehr erforderlich. Veraltete Konten die Bereitstellung Links zusammen Systeminformationen autorisierende Informationen Benutzer steuern die Konten besitzen. Autorisierende Benutzeridentitätsinformationen wird in der Regel in Datenbanken und Verzeichnisse der Humanressourcen verwaltet.

Konten in komplexen IT-Unternehmen gehören Hunderte von Parametern, die die Behörden, und diese Details vom provisioning System gesteuert werden. Neue Benutzer können mit den Daten identifiziert werden, die von der autorisierenden Quelle bereitstellen. Anforderung-Genehmigung Zugriffsfunktion initiiert die Prozesse, die Ressourcen für diese Bereitstellung genehmigen (oder ablehnen).


| Phase des Lebenszyklus-management          | Auf dem Gelände                                                                                                                                                                                                                                                       | Cloud                                                                                                                                                                                                                                                                                                                     | Hybrid                                                                                   |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Account-Management und Bereitstellung | Mithilfe der Active Directory® (AD DS)-Serverrolle kann eine skalierbare, sichere und verwaltbare Infrastruktur für Benutzer und Ressourcen-Management und bieten Unterstützung für AD-aktivierten Anwendung, wie Microsoft® Exchange Server. <br><br> [Sie können Gruppen in AD DS durch Identitätsmanager bereitstellen.](https://technet.microsoft.com/library/ff686261.aspx) <br>[Sie können Benutzer in AD DS bereitgestellt.](https://technet.microsoft.com/library/ff686263.aspx) <br><br> Zugriffskontrolle können Administratoren verwalten des Benutzerzugriffs auf freigegebene Ressourcen aus Sicherheitsgründen. Zugriffskontrolle erfolgt in Active Directory auf der Objektebene durch Festlegen von unterschiedlichen Zugriff und Berechtigungen für Objekte, z. B. Vollzugriff, schreiben, lesen oder kein Zugriff. Zugriffskontrolle in Active Directory definiert verschiedene Benutzer können Active Directory-Objekte. Standardmäßig werden Berechtigungen für Objekte in Active Directory auf die sicherste Einstellung festgelegt. | Sie müssen ein Konto für jeden Benutzer erstellen, die einen Microsoft Cloud-Dienst zugreifen. Sie können auch Benutzerkonten ändern oder löschen, wenn sie nicht mehr benötigt werden. Standardmäßig Benutzer keinen Administratorberechtigungen jedoch optional können sie. Weitere Informationen finden Sie unter [Verwalten von Benutzern in Azure AD](active-directory-create-users.md). <br><br> In Azure Active Directory ist eines der wichtigsten Features Zugriff auf Ressourcen zu verwalten. Diese Ressourcen können im Verzeichnis wie Berechtigungen zum Verwalten von Objekten über Rollen im Verzeichnis oder externen Directory SaaS-Applikationen Azure Services und SharePoint-Websites oder auf lokale Ressourcen Ressourcen sein. <br><br> Auf die Mitte der Azure Active Directory ist die Lösung der Sicherheitsgruppe. Der Besitzer der Ressource (oder der Administrator des Verzeichnisses) können eine Gruppe eine bestimmte direkt auf die Ressourcen zugreifen, die sie besitzen. Mitglieder der Gruppe erfolgt des Zugriffs und die Ressourcenbesitzer kann das Recht zur Verwaltung der Mitgliederliste der Gruppe an jemanden – ein Abteilungsleiter oder ein Helpdesk-Administrator delegieren<br> <br> Verwalten von Gruppen in Azure AD-Thema enthält weitere Informationen zum Zugriff über Gruppen verwalten.| Erweitern Sie Active Directory Identitäten in der Cloud durch Synchronisierung und Föderation |

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffskontrolle
Rollenbasierter Zugriff steuern (RBAC) verwendet Rollen und Bereitstellung von Richtlinien zu evaluieren, testen und Ihre Geschäftsprozesse und die Regeln für den Zugriff auf Benutzer zu erzwingen. Hauptadministrator Bereitstellung Richtlinien erstellen und Zuweisen von Benutzern zu Rollen und Sätze von Berechtigungen zu Ressourcen für diese Rollen definieren. RBAC erweitert Identity Management-Lösung zur softwarebasierten Prozessen und manuelle Benutzerinteraktion bei der Bereitstellung.
Azure AD RBAC ermöglicht Unternehmen, die eine Person durchführen kann, hat er Zugriff auf Azure-Verwaltungsportal Vorgänge beschränken. Mit RBAC zum Steuern des Zugriffs auf das Portal, IT-Administratoren ca Stellvertretungszugriff mit Access managementansätze:

- **Zuweisung der Sicherheitsrolle Gruppenbasierte**: können Sie Access Azure AD-Gruppen synchronisiert werden aus dem lokalen Active Directory zuweisen. Dadurch können Sie vorhandene Investitionen nutzen, die Ihre Organisation in Werkzeuge und Prozesse zum Verwalten von Gruppen. Sie können auch die delegierte Verwaltungsfunktion Azure AD Premium.
- **Nutzen Sie integrierte Rollen in Azure**: Sie verwenden drei Rollen-Besitzer Mitwirkende und Leser um sicherzustellen, dass Benutzer und Gruppen berechtigt sind, die Aufgaben müssen sie ihre Arbeit.
- **Granulat Zugriff auf Ressourcen**: Zuweisen Rollen für Benutzer und Gruppen für ein bestimmtes Abonnement, Ressourcengruppe oder eine einzelne Azure Ressource wie einer Website oder einer Datenbank. Auf diese Weise können Sie sicherstellen, dass Benutzer Zugriff auf die benötigten Ressourcen und keinen Zugriff auf Ressourcen, die sie nicht verwalten müssen.

## <a name="provisioning-and-other-customization-options"></a>Bereitstellung und andere Anpassungsoptionen
Ihr Team können Geschäftspläne und Vorschriften Sie entscheiden, wie viel die identitätslösung anpassen. Z. B. möglicherweise ein großes Unternehmen einen schrittweisen Rollout-Plan für Workflows und benutzerdefinierte Adapter auf einer Zeitachse für inkrementelle Bereitstellung Applications in Regionen verbreitet sind. Plan zur Anpassung der anderen bietet möglicherweise für zwei oder mehr in einer gesamten Organisation nach erfolgreichem Test bereitgestellt werden. Anwendung Aktivität kann angepasst und Verfahren zur Bereitstellung von Ressourcen können automatisierte Bereitstellung angepasst geändert werden.

Sie können einen Dienst oder eine Komponente entfernen entziehen. Entfernung eines Kontos bedeutet beispielsweise, dass das Konto aus einer Ressource gelöscht wird.

Das Hybridmodell der Bereitstellung von Ressourcen kombiniert Anforderung und rollenbasierte Ansätze, die beide von Azure AD unterstützt. Für eine Teilmenge von Mitarbeitern oder verwalteten Systemen möchten Unternehmen mit rollenbasierte Aufgaben automatisieren. Ein Unternehmen könnte auch alle anderen Anfragen oder Ausnahmen durch Anforderung basierendes Modell verarbeiten. Einige Unternehmen möglicherweise mit manuellen Starten und Hybrid-Modell mit der Absicht einer vollständig rollenbasierte Bereitstellung zu einem späteren Zeitpunkt entwickeln.

Andere Unternehmen können es für geschäftliche Gründe zu vollständige rollenbasierte Bereitstellung und Ziel einen gemischter Ansatz zum gewünschten Ziel. Noch andere Unternehmen möglicherweise nur Anforderung-basierte Bereitstellung zufrieden, und möchten keine zusätzlichen Aufwand definieren und rollenbasierte, automatisierte Bereitstellung Richtlinien verwalten.

## <a name="license-management"></a>Lizenzmanagement
Gruppenbasierte Lizenzmanagement in Azure AD kann Administratoren Benutzer zu einer Sicherheitsgruppe zuweisen und Azure AD weist Lizenzen automatisch an alle Mitglieder der Gruppe. Wenn ein Benutzer später hinzugefügt oder aus der Gruppe entfernt, wird eine Lizenz automatisch zugewiesen oder gegebenenfalls entfernt.

Von synchronisieren Gruppen können lokale AD oder in Azure AD verwalten. Pairing dies mit Azure AD Self-Service-Verwaltung können Sie problemlos lizenzzuweisung an die entsprechenden Entscheidungsträger delegieren. Sie können sicher sein, dass Probleme wie Konflikte Lizenz und fehlende Positionsdaten automatisch sortiert werden.

## <a name="self-regulating-user-administration"></a>Selbst regulieren Benutzeradministration
Beginnt der Organisation Ressourcen für alle internen Organisationen bereitstellen, implementieren Sie die Berechtigung selbst regulieren Verwaltung. Sie können die Vorteile und die Vorteile der Bereitstellung Benutzer über Organisationsgrenzen hinweg erkennen. In dieser Umgebung ist eine Änderung im Status eines Benutzers automatisch Zugriffsrechte über Grenzen Organisation und Regionen wider. Verringert Bereitstellungskosten und rationalisieren Zugriff und Genehmigung. Die Implementierung erkennt das Potenzial rollenbasierte Zugriffskontrolle für End-to-End-Verwaltung in Ihrer Organisation zu implementieren. Sie reduzieren Kosten durch automatisierte Verfahren für die Verwaltung der Benutzer bereitstellen. Sicherheit verbessern Durchsetzung von Sicherheitsrichtlinien, automatisieren und optimieren, und Benutzer Lifecycle Management und Bereitstellung für große Benutzerzahlen zu zentralisieren.

>[AZURE.NOTE]
Weitere Informationen finden Sie unter Einrichten von Azure AD für Self-Service Access Anwendungsmanagement

Azure AD (Anspruch basiert) Lizenz-basierte Dienste durch Aktivieren eines Abonnements in Ihrem Mandanten Azure AD Directory-Dienst. Nachdem das Abonnement aktiv ist können lizenzierte Benutzer Servicefunktionen von Administratoren Directory-Dienst verwaltet und verwendet werden. Weitere Informationen finden Sie unter Azure AD Arbeit Lizenzierung wie funktioniert?
Integration mit anderen 3rd party

Azure Active Directory bietet Single-Sign-in und erweiterte anwendungssicherheit von SaaS-Applikationen und lokalen Web Applications. Eine detaillierte Liste der Azure Active Directory Anwendungskatalog unterstützten SaaS-Anwendung finden Sie in Azure Active Directory Federation Kompatibilitätsliste: Drittanbietern Identitätsanbieter, mit der Anmeldung zu implementieren,

## <a name="define-synchronization-management"></a>Definieren der Synchronisierung management
Integrieren von Azure AD die lokalen Verzeichnisse Ihre Anwender macht produktiver durch Bereitstellung einer gemeinsamen Identität auf Cloud und lokalen Ressourcen. Mit dieser Integration können Benutzer und Organisationen der folgenden nutzen:

- Organisationen können Benutzer mit einer gemeinsamen Hybrid-Identität über lokalen oder Cloud-basierte Services nutzt Windows Server Active Directory herstellen und dann Azure Active Directory bereitstellen.
- Administratoren können bedingte basierend auf Anwendungsressource, Gerät und Benutzeridentität Netzwerkadresse und mehrstufige Authentifizierung zugreifen.
- Benutzer können ihre gemeinsamen Identität über Konten in Azure AD zu Office 365 Intune SaaS-apps und Drittanbietern nutzen.
- Können Entwickler Applikationen, die allgemeine Identitätsmodell nutzen Applikationen in Active Directory lokal oder Azure für Cloud-basierte Integration

Die folgende Abbildung enthält ein Beispiel für eine Gesamtübersicht der Synchronisierungsprozess Identität.

![](./media/hybrid-id-design-considerations/identitysync.png)

Identity-Synchronisierung

Überprüfen Sie in der folgenden Tabelle um Synchronisationsoptionen zu vergleichen:

| Management (Synchronisierungsoption)          | Vorteile                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Nachteile                                                                                                                                                                                                                                                                                  |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Sync-basierte (durch Dirsync- oder AADConnect) | Benutzer und Gruppen aus lokalen und Cloud synchronisiert <br>  **Policy-Steuerung**: Kontorichtlinien können festgelegt werden, über Active Directory Administrator verwalten Kennwortrichtlinien Arbeitsstation beschränkt, Sperre Steuerelemente ermöglicht und mehr ohne zusätzliche Aufgaben in der Cloud.  <br>  **Zugriffskontrolle**: kann Zugriff auf Cloud-Dienst, corporate Umgebung über online Server oder beide Dienste zugegriffen werden können. <br>  Verringert Supportanrufe: Wenn Benutzer weniger Kennwörter haben, sind sie weniger wahrscheinlich vergessen. <br>  Sicherheit: Benutzeridentitäten sind geschützt, da alle Server und Dienste, die einmaliges Anmelden verwendet gemeistert und Informationen lokal gesteuert. <br>  Unterstützung für starke Authentifizierung: Verwenden Sie strenge Authentifizierung (auch zwei-Faktor-Authentifizierung genannt) mit dem Clouddienst. Verwenden der sicheren Authentifizierung müssen Sie einmaliges Anmelden verwenden. |                                                                                                                                                                                                                                                                                                |
| Verbund basierende (durch AD FS)           | Aktivierte Security Token Service (STS). Beim Konfigurieren eines STS einzelne Anmeldung mit einem Microsoft Cloud-Dienst zugreifen erstellen Sie eine Vertrauensstellung zwischen lokalen STS und die Föderationsdomäne in Azure AD-Mandanten angegebenen. <br> Ermöglicht es Endbenutzern, dieselben Anmeldeinformationen verwenden, den Zugriff auf mehrere Ressourcen <br>Benutzer müssen nicht mehrere Sätze von Anmeldeinformationen verwalten. Jedoch müssen Benutzer ihre Anmeldeinformationen an jedem der beteiligten Ressourcen, B2B und B2C-Szenarien unterstützt.                                                                                                                                                                                                                                                                                                                                                                                                             | Fachpersonal für Bereitstellung und Wartung von dedizierten auf Prem erfordert AD FS-Server. Gibt es Beschränkungen sichere Authentifizierung AD FS für STS verwendet werden soll. Weitere Informationen finden Sie unter [Konfigurieren erweiterter Optionen für AD FS 2.0](http://go.microsoft.com/fwlink/?linkid=235649). |

>[AZURE.NOTE]
Weitere Informationen finden Sie unter [Ihrem lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).


## <a name="see-also"></a>Siehe auch
[Design Considerations (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)
