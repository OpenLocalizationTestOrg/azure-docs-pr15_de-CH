<properties
   pageTitle="Sicherheit von Azure Management und Überwachung Übersicht | Microsoft Azure"
   description=" Azure bietet Sicherheitsmechanismen für die Verwaltung und Überwachung von Azure Cloud Services und virtuellen Maschinen.  Dieser Artikel bietet eine Übersicht über wichtige Sicherheitsfeatures und Services. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-security-management-and-monitoring-overview"></a>Sicherheit von Azure Management und Überwachung (Übersicht)

Azure bietet Sicherheitsmechanismen für die Verwaltung und Überwachung von Azure Cloud Services und virtuellen Maschinen. Dieser Artikel bietet eine Übersicht über wichtige Sicherheitsfeatures und Services. Artikel werden Links bereitgestellt, die Details der einzelnen geben, damit Sie mehr.

Die Sicherheit Ihrer Microsoft Cloud-Dienste ist eine Partnerschaft und gemeinsamen Verantwortlichkeit zwischen Ihnen und Microsoft. Gemeinsame Verantwortung bedeutet Microsoft ist verantwortlich für die Microsoft Azure und physische Sicherheit von Daten (durch Sicherheitsmaßnahmen wie gesperrte Badge Eintrag Türen, Zäune und schützt). Azure bietet außerdem starke Cloud-Sicherheit auf der Softwareebene, die Sicherheit, Datenschutz und Compliance anspruchsvollsten Kunden gerecht.

Sie eigene Daten und Identitäten, die Verantwortung für schützen sie die Sicherheit Ihrer lokalen Ressourcen und Sicherheit Cloud-Komponenten über die Kontrolle haben. Microsoft bietet Sicherheit und Funktionen, um Ihre Daten und Programme zu schützen. Die Verantwortung für die Sicherheit basiert auf dem Cloud-Dienst.

Das folgende Diagramm fasst Saldo gegenüber Microsoft und dem Kunden.

![Gemeinsame Verantwortung][1]

Ein ausführlicher Sicherheits-Management finden Sie unter [Security Management in Azure](azure-security-management.md).

Hier sind die Kernfunktionen in diesem Artikel behandelt:

- Rollenbasierte Zugriffskontrolle
- Antimalware
- Mehrstufige Authentifizierung
- ExpressRoute
- Virtuelle Netzwerk-gateways
- Privilegierte Identitätsmanagement
- Schutz
- Sicherheitscenter

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffskontrolle

Role-Based Access Control (RBAC) bietet detaillierte Verwaltung für Azure-Ressourcen. RBAC verwenden, können Sie Personen nur die Anzahl der Zugriffe gewähren, die sie zum Erfüllen ihrer Aufgaben benötigen.  RBAC auch können Sie sicherstellen, dass Benutzer die Organisation verlassen sie auf Ressourcen in der Cloud zugreifen.

Weitere Informationen:

- [Active Directory-Teamblog zu RBAC](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
- [Azure rollenbasierte Zugriffskontrolle](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Antimalware

Mit Azure können Antischadsoftware von wichtigen Sicherheitsanbietern wie Microsoft, Symantec, Trend Micro, McAfee und Kaspersky um die virtuellen Computer vor bösartigen Dateien, Adware und andere Gefahren zu schützen.

Microsoft Antimalware bietet die Möglichkeit, einen Agenten Antimalware PaaS-Rollen und virtuellen Computern installieren. Basiert auf System Center Endpoint Protection, bringt diese Funktion bewährte lokalen Security-Technologie in die Cloud.

Wir bieten umfassende Integration Trend [Deep Security](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)™ und [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™ Produkte in der Azure-Plattform. DeepSecurity ist eine Antiviren-Lösung und SecureCloud ist eine Verschlüsselung. DeepSecurity werden in VMs Erweiterung Modell bereitgestellt. Portal Benutzeroberfläche und PowerShell verwenden, können Sie mit DeepSecurity in neue VMs erstellt wird werden oder vorhandene virtuelle Computer, die bereits bereitgestellt wurden.

Symantec Endpunkt Schutz (SEP) wird ebenfalls in Azure unterstützt. Portal Integration können Kunden angeben, dass sie in einer VM SEP verwenden möchten. SEP kann auf einen völlig neuen virtuellen Azure-Portal installiert oder auf eine vorhandene VM mit PowerShell installiert werden kann.

Weitere Informationen:

- [Antimalware Lösungen auf Azure Virtual Machines](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Microsoft Antimalware Azure Cloud Services und virtuellen Maschinen](../security/azure-security-antimalware.md)
- [Zum Installieren und Konfigurieren von Trend Micro Deep Security als Dienst auf einem Windows-VM](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Zum Installieren und Konfigurieren von Symantec Endpoint Protection auf einer Windows-VM](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Neue Antimalware-Optionen zum Schutz von Azure Virtual Machines – McAfee Endpoint Protection](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Mehrstufige Authentifizierung

Azure mehrstufige Authentifizierung (MFA) ist eine Methode der Authentifizierung, die mehrere Verifizierungsmethode muss Benutzer anmelden und Transaktionen eine wichtige zweite Sicherheitsebene hinzugefügt. MFA hilft Schutz Zugriff auf Daten und Applikationen Benutzer Nachfrage nach einem einfachen Vorgang. Bietet starke Authentifizierung über eine Reihe von Überprüfungsoptionen, Telefonanruf, SMS oder mobile app Benachrichtigung oder Überprüfung Code und 3. Partei EID-Token.

Weitere Informationen:

- [Mehrstufige Authentifizierung](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Was ist Azure mehrstufige Authentifizierung?](../multi-factor-authentication/multi-factor-authentication.md)
- [Funktionsweise von Azure mehrstufige Authentifizierung](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute

Microsoft Azure ExpressRoute ermöglicht die lokalen Netzwerken in der Cloud von Microsoft über eine dedizierte private Verbindung von einem konnektivitätsanbieter erleichtert. Mit ExpressRoute können Sie die Verbindung mit Microsoft-Cloud-Diensten wie Microsoft Azure, Office 365 und CRM Online herstellen. Verbindung kann eine n: n-Netzwerk (IP VPN), einem Punkt Ethernet-Netzwerk oder virtuellen Cross-Verbindung von einem Konnektivität an einem gemeinsam. ExpressRoute-Verbindung gehen nicht über das öffentliche Internet. Dadurch können ExpressRoute Verbindungen mehr Zuverlässigkeit, höhere Geschwindigkeit niedriger Latenz und höhere Sicherheit als normalen Verbindungen über das Internet anbieten.

Weitere Informationen:

- [ExpressRoute – technische Übersicht](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Virtuelle Netzwerk-gateways

VPN-Gateways auch als Azure Virtual Network Gateways zum Netzwerkverkehr zwischen virtuellen Netzwerken und lokalen senden. Sie werden auch zum Senden von Datenverkehr zwischen mehreren virtuellen Netzwerken in Azure (VNet VNet) verwendet.  VPN-Gateways bieten sichere standortübergreifende Konnektivität zwischen Azure und Ihre Infrastruktur.

Weitere Informationen:

- [Über VPN-gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md)
- [Übersicht über die Sicherheit von Azure-Netzwerk](security-network-overview.md)

## <a name="privileged-identity-management"></a>Privilegierte Identitätsmanagement

Manchmal müssen Benutzer Berechtigungen in Azure Ressourcen oder andere SaaS-Anwendung durchzuführen. Oft bedeutet Organisationen haben Ihnen permanenten Systemzugriff in Azure Active Directory (Azure AD). Deswegen wachsenden SRM für Cloud-gehosteten Ressourcen Organisationen ausreichend überwachen können nicht die Benutzer ihre privilegierten Zugriff.
Außerdem konnte beeinträchtigt ein Benutzerkonto mit Berechtigungen, eine Verletzung der gesamten Cloud-Sicherheit auswirken. Azure AD Berechtigungen Identitätsmanagement hilft Lösung dieses Risikos durch Senkung der Belichtungszeit Berechtigungen und Einblick in Verwendung.  

Privilegierte Identity Management Konzept einer temporären Admin für eine Rolle oder "just in Time" Administratorzugriff, die ein Benutzer eine Aktivierung für diese Rolle muss. Der Aktivierungsprozess ändert die Zuordnung des Benutzers eine Rolle in Azure AD aktiv für eine bestimmte Zeit inaktiv Periode wie 8 Stunden.

Weitere Informationen:

- [Azure AD Berechtigungen Identitätsmanagement](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Erste Schritte mit Azure AD privilegierten Identitätsmanagement](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Schutz

Schutz Azure Active Directory (AD) bietet eine konsolidierte Ansicht verdächtige Aktivitäten und Schwachstellen Ihres Unternehmens schützen. Schutz erkennt verdächtige Aktivitäten für Benutzer und Berechtigungen (Admin) Identitäten wie Brute-Force-Angriffe anhand Speicherblock Anmeldeinformationen und Anmeldungen von unbekannten Standorten und infizierte Geräte.

Indem Benachrichtigungen und empfohlene Wiederherstellung hilft Ihnen Schutz, Risiken in Echtzeit. Berechnet Benutzer Risiko Schweregrad und risikobasierte Richtlinien automatisch Schutzmaßnahmen gegen künftige Virengefahren zugreifen helfen können.

Weitere Informationen:

- [Azure Active Directory-Schutz](../active-directory/active-directory-identityprotection.md)
- [Channel 9: Azure AD und Identität: Identität Schutz Vorschau](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Sicherheitscenter

Azure Security Center können Sie verhindern, erkennen und reagieren auf Gefahren und erhöhte Transparenz, und Kontrolle über die Sicherheit von Azure Ressourcen bietet. Bietet integrierte Überwachung und Policy-Management über Abonnements Azure, Gefahren, die andernfalls vielleicht übersehen würden und arbeitet mit einem ausgedehnten System Security Solutions hilft bei der Ermittlung.

Security Center können Sie optimieren und die Sicherheit von Azure Ressourcen überwachen:

- Aktivieren Sie zum Definieren von Richtlinien für Ihre Azure-Abonnementressourcen Ihres Unternehmens Sicherheitsbedarfs und der Anwendung oder die Daten in jedem Abonnement.
- Überwachen des Status der Azure virtuelle Computer, Netzwerk und Anwendung.
- Die Liste der priorisierten Sicherheitshinweise, einschließlich Warnungen integrated Partner Solutions, Informationen schnell zu untersuchen und zu beheben ein Angriffs.

Weitere Informationen:

- [Einführung in Azure Security Center](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
