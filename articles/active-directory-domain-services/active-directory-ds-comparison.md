<properties
    pageTitle="Azure Active Directory-Domänendienste: Vergleichen Azure AD-Domänendienste zu Hause Domänencontrollern | Microsoft Azure"
    description="Vergleich von Azure Active Directory-Domänendienste zu Hause Domänencontrollern"
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
    ms.date="10/01/2016"
    ms.author="maheshu"/>

# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>Wie Sie entscheiden, ob Azure Active Directory-Domänendienste ist für Ihren Anwendungsfall geeignet
Azure Active Directory-Domänendiensten können Sie Ihre Arbeitslasten in Azure Infrastructure Services bereitstellen, ohne Ihre Identität Infrastruktur kümmern. Managed Service unterscheidet sich von Windows Server Active Directory typischerweise das Bereitstellen und selbst verwalten. Der Dienst wurde für einfache Bereitstellung, automatisierte Überwachung und Sanierung und einfache Identitätsinfrastruktur für die Cloud entwickelt. Wir entwickeln ständig den Dienst Unterstützung für häufige Bereitstellungsszenarien.

Entscheiden Sie, ob Azure Active Directory-Domänendienste oder einrichten und verwalten Ihre eigenen AD-Infrastruktur in Azure (Yourself):

- Liste der [Funktionen von Azure Active Directory-Domänendienste angeboten](active-directory-ds-features.md).

- Überprüfen Sie häufige [Bereitstellungsszenarien für Azure Active Directory-Domänendienste](active-directory-ds-scenarios.md).

- Schließlich [Azure AD-Domänendienste Yourself AD Option vergleichen](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).


## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>Vergleich von Azure Active Directory-Domänendienste zu Hause AD-Domäne in Azure
In der folgenden Tabelle hilft Ihnen bei der Entscheidung zwischen Azure Active Directory-Domänendienste und verwalten Ihre eigenen AD-Infrastruktur in Azure.

|**Funktion**|**Azure Active Directory-Domänendienste**|**"Do It Yourself" AD in Azure VMs**|
|---|:---:|:---:|
|[**Managed Services**](active-directory-ds-comparison.md#managed-service)|**& #x 2713;**|**& #x 2715;**|
|[**Sichere Bereitstellung**](active-directory-ds-comparison.md#secure-deployments)|**& #x 2713;**|Der Administrator muss zum Sichern der Bereitstellung.|
|[**DNS-server**](active-directory-ds-comparison.md#dns-server)|**& #x 2713;** (managed Service)|**& #x 2713;**|
|[**Domänen- oder Organisationsadministrator Administratorrechte**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges)|**& #x 2715;**|**& #x 2713;**|
|[**Beitreten zu einer Domäne**](active-directory-ds-comparison.md#domain-join)|**& #x 2713;**|**& #x 2713;**|
|[**Mit NTLM und Kerberos Domänenauthentifizierung**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos)|**& #x 2713;**|**& #x 2713;**|
|[**Benutzerdefinierte OU-Struktur**](active-directory-ds-comparison.md#custom-ou-structure)|**& #x 2713;**|**& #x 2713;**|
|[**Schema-Erweiterung**](active-directory-ds-comparison.md#schema-extensions)|**& #x 2715;**|**& #x 2713;**|
|[**AD-Domäne/Gesamtstruktur-Vertrauensstellungen**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts)|**& #x 2715;**|**& #x 2713;**|
|[**LDAP-Lesevorgang**](active-directory-ds-comparison.md#ldap-read)|**& #x 2713;**|**& #x 2713;**|
|[**Sicheres LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap)|**& #x 2713;**|**& #x 2713;**|
|[**LDAP-Schreibzugriff**](active-directory-ds-comparison.md#ldap-write)|**& #x 2715;**|**& #x 2713;**|
|[**Gruppenrichtlinien**](active-directory-ds-comparison.md#group-policy)|Einfache|Vollständige|
|[**Geografisch verteilte Bereitstellung**](active-directory-ds-comparison.md#geo-dispersed-deployments)|**& #x 2715;**|**& #x 2713;**|


#### <a name="managed-service"></a>Managed Services
Microsoft Azure Active Directory-Domänendienste-Domänen verwaltet. Sie müssen keinen Patches, Updates, Überwachung, Backups und Verfügbarkeit Ihrer Domäne. Diese Verwaltungsaufgaben als Dienst von Microsoft Azure für verwalteten Domänen angeboten.

#### <a name="secure-deployments"></a>Sichere Bereitstellung
Gemäß den von Microsoft empfohlenen Vorgehensweisen für die Bereitstellung von Active Directory ist das verwaltete Domäne sicher gesperrt. Diese bewährten Methoden sind AD-Produktteam jahrzehntelange engineering und AD Bereitstellung unterstützen. Für Yourself Installationen müssen Sie bestimmte Bereitstellungsschritte Sperren nach unten/sichern die Bereitstellung.

#### <a name="dns-server"></a>DNS-server
Eine verwaltete Azure Active Directory-Domänendienste-Domäne enthält verwalteten DNS-Dienste. Mitglieder der Gruppe 'AAD DC Administrators' können DNS auf verwalteten Domäne verwalten. Mitglieder dieser Gruppe erhalten vollständige DNS-Administratorrechte für die verwalteten Domäne. DNS-Management erfolgt mithilfe der "DNS-Verwaltungskonsole" im Paket (Remote Server Administration Tools RSAT) enthalten.

#### <a name="domain-or-enterprise-administrator-privileges"></a>Domänen- oder Unternehmensadministrator Berechtigungen
Diese erweiterten Berechtigungen werden nicht in einer verwalteten Domäne AAD DS angeboten. Programme, die diese erhöhten installiert ausführen, können gegen verwalteten Domänen ausgeführt werden. Eine kleinere Teilmenge von Administratorrechten ist Mitglied der delegierten Administratorgruppe Namen 'AAD DC Administrators' verfügbar. Diese Berechtigungen sind Berechtigungen konfigurieren, Konfigurieren von Gruppenrichtlinien, Administratorrechte auf Domäne usw. zu erhalten.

#### <a name="domain-join"></a>Beitreten zu einer Domäne
Join virtuelle Computer der verwalteten Domäne ähnlich wie Computer einer Active Directory-Domäne beitreten.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Mit NTLM und Kerberos Domänenauthentifizierung
Azure Active Directory Domain Services können Sie die Unternehmensanmeldeinformationen verwalteten Domäne authentifizieren. Anmeldeinformationen werden mit Azure AD-Mandanten. Synchronisierte Mieter sorgt Azure AD Connect Änderungen vorgenommen Anmeldeinformationen lokal in Azure Active Directory synchronisiert werden. Bei einer Hause Domäne müssen Sie eine Vertrauensstellung mit einer lokalen Konto für Benutzer mit ihren Unternehmensanmeldeinformationen authentifizieren einrichten. Alternativ müssen Sie Active Directory-Replikation einrichten, um sicherzustellen, dass Benutzerkennwörter Ihren Azure Domain Controller virtuellen Computern synchronisieren.

#### <a name="custom-ou-structure"></a>Benutzerdefinierte OU-Struktur
Mitglieder der Gruppe 'AAD DC Administrators' können benutzerdefinierte Organisationseinheiten innerhalb der verwalteten Domäne erstellen.

#### <a name="schema-extensions"></a>Schema-Erweiterung
Das Basisschema einer verwalteten Azure Active Directory-Domänendienste-Domäne kann nicht erweitert werden. Anträge auf Erweiterung AD-Schema (z. B. neue Attribute unter User Object) können daher aufgehoben und AAD-Domänen verschoben.

#### <a name="ad-domain-or-forest-trusts"></a>AD-Domäne oder Gesamtstruktur-Vertrauensstellungen
Verwaltete Domänen können nicht konfiguriert (eingehend/ausgehend)-Vertrauensstellungen mit anderen Domänen einrichten. Daher können nicht Szenarien Fälle, wo Sie lieber nicht Synchronisieren von Kennwörtern in Azure AD, oder Ressourcen-Gesamtstruktur Bereitstellung Azure Active Directory-Domänendienste verwenden.

#### <a name="ldap-read"></a>LDAP-lesen
Die verwaltete Domäne unterstützt LDAP gelesener Workloads. Daher können Sie Programme bereitstellen, die LDAP-Lesevorgänge verwalteten Domäne ausführen.

#### <a name="secure-ldap"></a>Sicheres LDAP
Sie können Azure AD-Domänendienste sicheren LDAP-Zugriff auf Ihre verwalteten Domäne auch über das Internet konfigurieren.

#### <a name="ldap-write"></a>LDAP-Schreibzugriff
Die verwaltete Domäne ist schreibgeschützt für Benutzerobjekte. Daher funktionieren Programme, die LDAP-Schreibzugriff Attributen des Benutzerobjekts Operationen nicht in einer verwalteten Domäne. Benutzerkennwörter können außerdem innerhalb der verwalteten Domäne geändert werden. Ein weiteres Beispiel wäre Änderung von Gruppenmitgliedschaften oder Attribute innerhalb der verwalteten Domäne nicht zulässig ist. Änderungen, Benutzerattribute oder Kennwörter in Azure AD (über PowerShell-Azure-Portal) oder lokalen AD DS AAD verwalteten Domäne synchronisiert.

#### <a name="group-policy"></a>Gruppenrichtlinien
Anspruchsvolle Group Policy Konstrukte werden verwaltete AAD DS-Domäne nicht unterstützt. Sie können nicht z. B. erstellen und verschiedene Gruppenrichtlinienobjekte für jede benutzerdefinierte Organisationseinheit in der Domäne bereitstellen oder verwenden WMI-Filterung für GP abzielen. Ist eine integrierte Gruppenrichtlinienobjekt jeder Container "AADDC Computer und"AADDC Users die Gruppenrichtlinie angepasst werden können.

#### <a name="geo-dispersed-deployments"></a>Geo-verteilte Bereitstellung
Azure Active Directory-Domänendienste verwaltete Domänen stehen in einem virtuellen Netzwerk in Azure. Für Szenarien, die Domänencontroller in mehreren Azure Regionen weltweit verfügbar sein müssen, wäre Domänencontroller in Azure IaaS VMs einrichten die bessere Alternative.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>"Do It Yourself" (DIY) AD-Bereitstellungsoptionen
Bereitstellung Anwendungsfälle möglicherweise, wenn Sie einige der Funktionen von Windows Server Active Directory-Installation benötigen. Sollten Sie in diesen Fällen einen Yourself (DIY) folgendermaßen:

- **Standalone Cloud Domäne:** Sie können eine eigenständige "Cloud Domäne Azure virtuelle Maschinen, die als Domänencontroller konfiguriert einrichten. Diese Infrastruktur integrieren nicht auf Ihrem lokalen AD-Umgebung. Diese Option erfordert einen anderen Satz von "Cloud Anmeldeinformationen anmelden/VMs in der Cloud verwalten.

- **Ressourcen-Gesamtstruktur Bereitstellung:** Einrichten einer Domäne eine Topologie, lassen sich mit Azure Virtual Machines als Domänencontroller konfiguriert. Anschließend können Sie eine Active Directory-Vertrauensstellung mit der lokalen konfigurieren AD-Umgebung. Sie können diese Ressourcengesamtstruktur in der Cloud Domänenbeitritt Computern (Azure VMs). Benutzerauthentifizierung erfolgt entweder über eine VPN-ExpressRoute-Verbindung zu Ihrem lokalen Verzeichnis.

- **Die lokalen Domäne in Azure erweitern:** Sie können Azure virtuelles Netzwerk mit dem lokalen Netzwerk eine Verbindung VPN-ExpressRoute, damit Ihre lokalen Azure VMs angehören können AD. Eine Alternative ist die Förderung replizierten Domänencontroller der lokalen Domäne in Azure als eine VM. Sie können es dann einrichten, über eine VPN-ExpressRoute-Verbindung zum lokalen Verzeichnis repliziert. Dieser Bereitstellungsmodus erweitert effektiv Ihre lokalen in Azure.

> [AZURE.NOTE] Sie können feststellen, dass Hause Option Anwendungsfälle die Bereitstellung besser geeignet ist. [Erteilen von Feedback](active-directory-ds-contact-us.md) zu verstehen, welche Funktionen dazu Azure Active Directory-Domänendienste in der Zukunft haben. Dieses Feedback hilft uns Entwickeln von Service um Ihre Bedürfnisse und Anwendungsfälle anzupassen.

Wir haben [Richtlinien für die Bereitstellung von Windows Server Active Directory in Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx) Hause Installationen vereinfachen veröffentlicht.


## <a name="related-content"></a>Verwandte Inhalte
- [Features - Dienstleistungen Azure AD-Domäne](active-directory-ds-features.md)

- [Bereitstellungsszenarien - Azure Active Directory-Domänendienste](active-directory-ds-scenarios.md)

- [Richtlinien für die Bereitstellung von Windows Server Active Directory in Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)
