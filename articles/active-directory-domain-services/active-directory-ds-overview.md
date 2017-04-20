<properties
    pageTitle="Übersicht über Azure Active Directory-Domänendienste | Microsoft Azure"
    description="Übersicht über Azure Active Directory-Domänendienste"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure Active Directory-Domänendienste

## <a name="overview"></a>Übersicht
Azure Infrastructure Services können Sie eine Vielzahl von computing-Lösungen auf flexible Weise bereitstellen. Azure Virtual Machines können Sie fast sofort bereitstellen und Sie bezahlen nur die Minute. Unterstützung für Windows, Linux, SQL Server, Oracle, IBM, SAP und BizTalk verwenden, können Sie alle Arbeitslasten, jede Sprache nahezu jedem Betriebssystem bereitstellen. Diese Vorteile können lokal Legacyanwendungen bereitgestellt in Azure Betriebskosten zu migrieren.

Ein wichtiger Aspekt der Migration von lokalen Anwendung in Azure ist Identität muss der Anwendung behandeln. Verzeichnisabhängige Anwendung basieren auf LDAP für Lese- oder Schreibzugriff auf das Unternehmensverzeichnis oder integrierte Windows-Authentifizierung (Kerberos oder NTLM-Authentifizierung) authentifiziert Benutzer abhängig. Line-of-Business (LOB) Applikationen unter Windows Server werden normalerweise auf Domäne beitreten bereitgestellt, damit sie mit Gruppenrichtlinien verwaltet werden können. 'Anheben und Umschalttaste' lokal Applikationen in die Cloud müssen diese Abhängigkeit der corporate Identitätsinfrastruktur aufgelöst werden.

Administratoren aktivieren häufig zu Folgendes Identität ihrer Anwendung in Azure bereitgestellt erfüllen:

- Bereitstellen Sie eine Standort-zu-Standort-VPN-Verbindung zwischen Arbeitslasten in Azure Infrastructure Services und das lokale Unternehmensverzeichnis.
- Erweitern der AD-Domäne/Gesamtstruktur Unternehmensinfrastruktur Replikatdomänencontroller Azure virtuelle Maschinen einrichten.
- Bereitstellen einer eigenständigen Domäne in Azure-Domänencontroller als Azure virtuelle Computer bereitgestellt.

Diese Ansätze Leiden hohe Kosten und Verwaltungsaufwand. Administratoren müssen mithilfe von virtuellen Computern in Azure bereitstellen. Darüber hinaus müssen sie verwalten, sichere patch, überwachen, Sichern und diese virtuellen Computer zu beheben. Die Abhängigkeit von VPN-Verbindungen in das lokale Verzeichnis wird Arbeitslasten in Azure kurzzeitige Probleme oder Ausfälle anfällig bereitgestellt. Diese Netzwerk-Ausfallzeiten führen wiederum niedrigere Verfügbarkeit und geringere Zuverlässigkeit dafür.

Wir entwickelt Azure AD-Domänendienste eine einfachere Alternative.


## <a name="introducing-azure-ad-domain-services"></a>Einführung in Azure Active Directory-Domänendienste
Azure Active Directory Domain Services bietet verwalteten Domäne wie Domänenbeitritt Group Policy, LDAP, Kerberos-NTLM-Authentifizierung, die vollständig kompatibel mit Windows Server Active Directory. Sie können diese Domänendienste ohne bereitstellen, verwalten und Patchen von Domänencontrollern in der Cloud nutzen. Azure Active Directory-Domänendienste integriert die vorhandenen Azure AD-Mandanten, wodurch Benutzer melden sich mit ihren Anmeldeinformationen der Firmen. Darüber hinaus können Sie sicheren Zugriff auf Ressourcen, damit ein glatter "Aufzug-und-Shift" lokale Ressourcen Azure Infrastructure Services vorhandenen Gruppen und Benutzerkonten.

Azure Active Directory-Domänendienste-Funktionalität nahtlos egal ob Azure AD-Mandanten nur Cloud oder lokale Active Directory synchronisiert.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Azure Active Directory-Domänendienste Cloud nur Organisationen
Eine Wolke nur Azure AD-Mandanten (oft als "verwaltete Mieter") verfügt nicht über alle lokalen Identität Fußabdruck. In anderen Worten sind Benutzerkonten, ihre Kennwörter und Gruppenmitgliedschaft alle Cloud - d. h. erstellt und verwaltet in Azure AD. Überlegen Sie sich Contoso eine Cloud nur Azure AD-Mandanten. Wie in der folgenden Abbildung dargestellt, hat der Administrator von Contoso ein virtuelles Netzwerk in Azure Infrastructure Services konfiguriert. Serverarbeitslasten und Applikationen werden in diesem virtuellen Netzwerk in Azure virtuelle Computer bereitgestellt. Da Contoso Cloud nur Mieter ist, werden alle Benutzeridentitäten, Anmeldeinformationen und Gruppenmitgliedschaften erstellt und verwaltet in Azure AD.

![Azure AD-Domänendienste (Übersicht)](./media/active-directory-domain-services-overview/aadds-overview.png)

Contoso IT-Administrator kann Azure AD-Domäne ihrer Azure AD-Mandanten aktiviert und Domänendienste in diesem virtuellen Netzwerk verfügbar zu machen. Danach Azure Active Directory-Domänendienste Vorschriften eine verwaltete Domäne und im virtuellen Netzwerk verfügbar gemacht. Alle Benutzerkonten, Gruppenmitgliedschaften und Benutzeranmeldeinformationen für Contoso Azure AD-Mandanten stehen auch in dieser neu erstellte Domäne. Diese Funktion ermöglicht Benutzern in der Organisation Anmelden an der Domäne mit ihren Unternehmensanmeldeinformationen - z. B. bei einer Domäne beigetretenen Computer über Remote Desktop Remoteverbindung. Administratoren können Zugriff auf Ressourcen in der Domäne vorhandenen Gruppenmitgliedschaften mit bereitstellen. Anwendung auf virtuellen Computern in der virtuellen Netzwerk bereitgestellt können Funktionen wie Domänenbeitritt, LDAP lesen LDAP Bind, NTLM und Kerberos-Authentifizierung und Gruppenrichtlinien.

Einige wichtige Aspekte der verwalteten Domäne von Azure Active Directory-Domänendiensten bereitgestellt werden wie folgt:

- Contoso IT-Administrator muss nicht verwalten, patch oder Überwachen dieser Domäne oder Domänencontroller für diese Domäne verwaltet.
- Es ist nicht erforderlich für die AD-Replikation für diese Domäne. Benutzerkonten, Gruppenmitgliedschaften und Anmeldeinformationen von Contoso Azure AD-Mandanten sind automatisch in diesen verwalteten Domäne verfügbar.
- Da Azure Active Directory-Domänendiensten Contoso Domäne verwaltet IT-Administrator keinen Domänenadministrator oder Organisationsadministrator-Berechtigungen in dieser Domäne.


### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>Azure Active Directory-Domänendienste Hybrid-Unternehmen
Organisationen mit einer Hybrid-IT-Infrastruktur nutzen aus Cloud und lokalen Ressourcen. Solche Organisationen Synchronisieren von Identitätsinformationen aus dem lokalen Verzeichnis ihrer Azure AD-Mandanten. Wie hybride Organisationen migriert ihrer lokalen Programme mit der Cloud, insbesondere verzeichnisabhängige Legacyanwendungen, Azure Active Directory-Domänendienste kann nützlich.

Litware Corporation hat [Azure AD Connect](../active-directory/active-directory-aadconnect.md)zum Synchronisieren von Identitätsinformationen aus dem lokalen Verzeichnis ihrer Azure AD-Mandanten bereitgestellt. Synchronisiert Identitätsinformationen enthält Benutzerkonten, deren Anmeldeinformationen Hashes für die Authentifizierung (Kennwort Sync) und Gruppenmitgliedschaften.

> [AZURE.NOTE] **Synchronisierung von Kennwörtern ist für hybride Organisationen Azure Active Directory-Domänendienste verwenden**. Diese Anforderung ist Benutzeranmeldeinformationen im verwalteten Bereich Dienstleistungen Azure AD-Domäne sind diese Benutzer über NTLM oder Kerberos Authentifizierungsmethoden Authentifizierung erforderlich.

![Azure Active Directory-Domänendienste für Litware Corporation](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

Die Abbildung zeigt, wie Unternehmen eine hybride IT-Infrastruktur wie Litware Corporation Azure Active Directory-Domänendienste. Litware Applikationen und Serverarbeitslasten, die Domänendienste erfordern werden in einem virtuellen Netzwerk in Azure Infrastructure Services bereitgestellt. Litware der IT-Administrator aktiviert Azure AD-Mandanten Azure AD-Domäne und eine verwaltete Domäne in diesem virtuellen Netzwerk verfügbar machen kann. Da Litware eine Organisation eine hybride IT-Infrastruktur ist, werden Benutzerkonten, Gruppen und Anmeldeinformationen mit ihrer Azure AD-Mandanten aus dem lokalen Verzeichnis synchronisiert. Diese Funktion ermöglicht Benutzern die Unternehmensanmeldeinformationen - z. B. bei Remote mit Computern in der Domäne über Remotedesktop mit an der Domäne anmelden. Administratoren können Zugriff auf Ressourcen in der Domäne vorhandenen Gruppenmitgliedschaften mit bereitstellen. Anwendung auf virtuellen Computern in der virtuellen Netzwerk bereitgestellt können Funktionen wie Domänenbeitritt, LDAP lesen LDAP Bind, NTLM und Kerberos-Authentifizierung und Gruppenrichtlinien.

Einige wichtige Aspekte der verwalteten Domäne von Azure Active Directory-Domänendiensten bereitgestellt werden wie folgt:

- Die verwaltete Domäne ist eine eigenständige Domäne. Es ist nicht Teil Litwares lokalen Domäne.
- Litware der IT-Administrator muss nicht verwalten, Patch oder Domänencontroller für diese Domäne verwaltet.
- Besteht keine Active Directory-Replikation zu dieser Domäne verwalten. Benutzerkonten, Gruppenmitgliedschaften und Anmeldeinformationen von Litwares lokalen Verzeichnisses werden in Azure AD über Azure AD Connect synchronisiert. Diese Benutzerkonten, Gruppenmitgliedschaften und Anmeldeinformationen sind automatisch in der verwalteten Domäne verfügbar.
- Da Azure Active Directory-Domänendiensten Litware Domäne verwaltet IT-Administrator keinen Domänenadministrator oder Organisationsadministrator-Berechtigungen in dieser Domäne.


## <a name="benefits"></a>Vorteile
Mit Azure Active Directory-Domänendienste genießen Sie folgende Vorteile:

-   **Einfach** – Sie können zufrieden Identität Azure Infrastrukturdienste mit ein paar einfachen Klicks eingesetzt. Sie müssen nicht bereitstellen und Verwalten von Identitätsinfrastruktur in Azure oder Setup Verbindung mit Ihrem lokalen Identitätsinfrastruktur.

-   **Integrierte** – Azure Active Directory-Domänendienste ist tief in Azure AD-Mandanten integriert. Jetzt können Azure AD als integrierte Cloud-basierte Enterprise-Verzeichnis, die den Bedürfnissen der modernen Applikationen sowohl herkömmliche verzeichnisabhängige Anwendung bietet.

-   **Kompatibel** – Azure Active Directory-Domänendienste bewährte Enterprise Grade Infrastruktur von Windows Server Active Directory erstellt. Daher können eine bessere Kompatibilität mit Windows Server Active Directory-Funktionen Ihrer Anwendung abhängig. Nicht alle Funktionen in Windows Server Active Directory sind derzeit in Azure Active Directory-Domänendienste. Funktionen sind jedoch kompatibel mit den entsprechenden Windows Server Active Directory-Funktionen, die Sie in Ihrer lokalen Infrastruktur auf. Verknüpfungsfunktionen LDAP, Kerberos, NTLM, Gruppenrichtlinien und Domäne bilden ausgereifte anbieten, die getestet und verschiedene Versionen von Windows Server verbessert wurde.

-   **Kostengünstige** – mit Azure Active Directory-Domänendienste, vermeiden Sie die Infrastruktur- und Belastung mit Identitätsinfrastruktur für herkömmliche verzeichnisabhängige Anwendung verwalten. Sie können diese Anwendung Infrastrukturdienste Azure verschieben und profitieren von geringeren Betriebskosten.
