<properties
    pageTitle="Azure Active Directory Domain Services: Bereitstellungsszenarien | Microsoft Azure"
    description="Bereitstellungsszenarien für Azure Active Directory-Domänendienste"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>


# <a name="deployment-scenarios-and-use-cases"></a>Bereitstellungsszenarien und Fallstudien
In diesem Abschnitt betrachten wir einige Szenarios und Anwendungsfälle, die von Azure Active Directory (AD)-Domänendienste profitieren.

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Sichere und einfache Verwaltung von Azure virtuelle Computer
Azure Active Directory Domain Services können Sie Ihre Azure virtuellen Computer optimierte Verwaltung. Der verwalteten Domäne, um Ihre corporate AD-Anmeldeinformationen für die Anmeldung verwenden können Azure virtuelle Computer hinzugefügt. Dadurch vermeiden Sie Anmeldeinformationen Management Probleme wie lokale Administratorkonten auf jedem Ihrer Azure virtuellen Maschinen.

Server virtuelle Computer, die Mitglied der verwalteten Domäne können auch verwaltet und gesichert mit Gruppenrichtlinien. Können Sie Ihre Azure virtuellen Computer erforderlichen Sicherheitsbaselines zuweisen und gemäß den Sicherheitsrichtlinien des Unternehmens sperren. Group Policy Management-Funktionen können Sie beschränken die Anwendungstypen, die auf diese virtuellen Computer gestartet werden kann.

![Optimierte Verwaltung Azure virtueller Maschinen](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Server und andere Infrastruktur erreicht End of Life, ist Contoso vielen derzeit lokal in die Cloud verschieben. Ihre aktuelle IT-Standard verlangt, dass Server corporate Applikationen Domäne und mithilfe von Gruppenrichtlinien verwaltet werden müssen. Contoso IT-Administrator möchte Domäne beitreten virtuellen Maschinen in Azure bereitgestellt Verwaltung erleichtern. Dadurch können Administratoren und Benutzer mit ihrer corporate Anmeldeinformationen anmelden. Computer können zur gleichen Zeit zur Einhaltung der erforderlichen Sicherheitsbaselines mithilfe von Gruppenrichtlinien konfiguriert werden. Contoso lieber nicht bereitstellen, überwachen und Verwalten von Domänencontrollern in Azure Azure virtuelle Computer sichern. Azure Active Directory-Domänendienste deshalb hervorragend für diesen Anwendungsfall.

**Deployment-Hinweise**

Berücksichtigen Sie die folgenden wichtigen Punkte für dieses Bereitstellungsszenario aus:

- Verwaltete Domänen Azure AD-Domäne Dienstleistungen bieten eine einzelne flache Struktur für Organisationseinheiten (Organisationseinheit) standardmäßig. Alle Computer der Domäne befinden in einer einzelnen flachen Organisationseinheit. Sie können jedoch benutzerdefinierte Organisationseinheiten erstellen.

- Azure Active Directory-Domänendienste unterstützt einfache Gruppenrichtlinie in Form eines integrierten Gruppenrichtlinienobjekts jedes für Benutzer und Computer Container. Sie können GP von OU-Abteilung abzielen, WMI-Filter oder benutzerdefinierte Gruppenrichtlinienobjekte erstellen.

- Azure Active Directory-Domänendienste unterstützt AD Computer-Objekt Basisschema. Sie können das Computerobjekt Schema nicht erweitern.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>Aufzug-und-Schicht eine lokale Anwendung, die LDAP-Bind-zu Azure Infrastruktur-Services Authentifizierung

![LDAP-Bindung](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso verfügt über eine lokale Anwendung, die ein ISV Jahren gekauft wurde. Die Anwendung derzeit im Wartungsmodus vom ISV und Anfordern von Änderungen an der Anwendung ist teuer für Contoso. Diese Anwendung besitzt einen webbasierten Front-End, das ein Webformular mit Benutzeranmeldeinformationen erfasst und dann authentifiziert Benutzer durch eine LDAP-Bindung zu Active Directory des Unternehmens ausführen. Contoso möchte diese Anwendung in Azure Infrastructure Services migrieren. Es ist wünschenswert, dass die Anwendung sich ohne jede Änderung. Außerdem sollte Benutzern mit ihren vorhandenen Unternehmen Anmeldeinformationen authentifizieren und ohne Benutzer anders umschulen. Anders gesagt sollten Benutzer nicht, in dem die Anwendung ausgeführt wird und die Migration sollten Sie transparent sein.

**Deployment-Hinweise**

Berücksichtigen Sie die folgenden wichtigen Punkte für dieses Bereitstellungsszenario aus:

- Stellen Sie sicher, dass die Anwendung nicht ändern notwendigerweise/Schreibzugriff auf das Verzeichnis. LDAP-Schreibzugriff auf verwalteten Domänen von Azure Active Directory-Domänendiensten bereitgestellt wird nicht unterstützt.

- Sie können nicht direkt anhand der verwalteten Domäne ändern. Benutzer können ihre Kennwörter entweder Azure AD Self-service-Kennwort ändern Mechanismus oder lokalen Verzeichnis. Diese werden automatisch synchronisiert und verfügbaren verwalteten Domäne.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>Heben Sie UMSCHALT lesen eine lokale Anwendung, LDAP Directory Azure Infrastructure Services zugreifen
Contoso hat eine lokale Line-of-Business (LOB)-Anwendung, die nahezu ein Jahrzehnt entwickelt wurde. Diese Anwendung ist bekannt und wurde mit Windows Server Active Directory. Die Anwendung verwendet LDAP (Lightweight Directory Access Protocol) Informationsattribute Benutzer aus Active Directory gelesen. Die Anwendung Attribute geändert oder anderweitig in das Verzeichnis schreiben. Contoso möchte diese Anwendung Infrastrukturdienste Azure und Alterung lokale Hardware derzeit Hostinganwendung zurückziehen. Die Anwendung kann nicht auf modernen Verzeichnisses APIs wie die REST-basierten Azure AD Graph-API angepasst werden. Daher anheben und UMSCHALT-Option gewünscht, wobei die Anwendung migriert werden kann, ohne Code ändern oder Umschreiben der Anwendung in der Cloud ausführen.

**Deployment-Hinweise**

Berücksichtigen Sie die folgenden wichtigen Punkte für dieses Bereitstellungsszenario aus:

- Stellen Sie sicher, dass die Anwendung nicht ändern notwendigerweise/Schreibzugriff auf das Verzeichnis. LDAP-Schreibzugriff auf verwalteten Domänen von Azure Active Directory-Domänendiensten bereitgestellt wird nicht unterstützt.

- Stellen Sie sicher, dass die Anwendung keine benutzerdefinierten erweitert Active Directory-Schema. Schema-Erweiterung werden in Azure Active Directory-Domänendiensten nicht unterstützt.


## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>Migrieren einer lokalen Dienst oder Dämon Anwendung zu Azure Infrastruktur-Services
Einige Programme bestehen mehrere Tiers benötigt einen Ebenen Authentifizierte Aufrufe auf Back-End wie eine Datenbankebene auszuführen. Active Directory-Dienstkonten werden häufig für diese Anwendungsfälle verwendet. Heben Sie UMSCHALT können Anträge Azure Infrastructure Services und Azure Active Directory-Domänendienste Bedarf Identität dieser Anwendung verwenden. Sie können dasselbe Dienstkonto verwenden, das aus dem lokalen Verzeichnis Azure AD synchronisiert wird. Alternativ können Sie zunächst eine benutzerdefinierte Organisationseinheit und erstellen ein separates Dienstkonto in dieser Organisationseinheit Anträge bereitstellen.

![Dienstkonto mit WIA-Unterstützung](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso hat eine maßgeschneiderte Software Vault-Anwendung, die ein Web-front-End, SQL Server und einem Back-End-FTP-Server enthält. Integrierte Windows-Authentifizierung von Dienstkonten zur Web-front-End für den FTP-Server Authentifizierung. Web-front-End soll als Dienstkonto ausführen. Der Back-End-Server konfiguriert Autorisieren des Zugriffs vom Dienstkonto für Web-front-End. Contoso möchte keinen virtuellen Domänencontrollercomputer in der Cloud, diese Anwendung zu Azure Infrastruktur-Services bereitstellen. Contoso kann IT-Administratoren Server, Web-front-End, SQL Server und dem FTP-Server Azure virtuellen Maschinen bereitstellen. Diese Computer sind dann zu einer verwalteten Domäne Azure Active Directory-Domänendiensten verknüpft. Dann können sie das gleiche Dienstkonto in ihrem lokalen Verzeichnis für die app-Authentifizierung. Dieses Dienstkonto Azure Active Directory-Domänendienste verwalteten Domäne synchronisiert und steht zur Verfügung.

**Deployment-Hinweise**

Berücksichtigen Sie die folgenden wichtigen Punkte für dieses Bereitstellungsszenario aus:

- Sicherstellen Sie, dass die Anwendung Benutzernamen und Kennwort für die Authentifizierung verwendet. Zertifikat/Smartcard-basierten Authentifizierung wird von Azure Active Directory-Domänendienste nicht unterstützt.

- Sie können nicht direkt anhand der verwalteten Domäne ändern. Benutzer können ihre Kennwörter entweder Azure AD Self-service-Kennwort ändern Mechanismus oder lokalen Verzeichnis. Diese werden automatisch synchronisiert und verfügbaren verwalteten Domäne.


## <a name="azure-remoteapp"></a>Azure RemoteApp
Azure RemoteApp kann der Administrator von Contoso eine Auflistung Domäne erstellen. Dieses Leistungsmerkmal Remoteanwendungen von Azure RemoteApp bedient auf Domänencomputern und andere Ressourcen integrierte Windows-Authentifizierung. Contoso können Azure Active Directory-Domänendienste zu einer verwalteten Domäne Domäne Azure RemoteApp-Sammlungen verwendet.

![Azure RemoteApp](./media/active-directory-domain-services-scenarios/azure-remoteapp.png)

Weitere Informationen zu diesem Bereitstellungsszenario finden Sie im Artikel Remote Desktop Services Blog [heben Sie UMSCHALTTASTE die Arbeitslasten mit Azure RemoteApp und Azure Active Directory Domain Services](http://blogs.msdn.com/b/rds/archive/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services.aspx).
