<properties
    pageTitle="FAQs - Azure Active Directory-Domänendienste | Microsoft Azure"
    description="Häufig gestellte Fragen zur Azure Active Directory-Domänendienste"
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
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory Domain Services: Häufig gestellte Fragen (FAQs)

Diese Seite beantwortet häufig gestellte Fragen zur Azure Active Directory Domain Services. Halten Sie nachsehen, ob Updates.

### <a name="troubleshooting-guide"></a>Fehlerbehebung
Finden Sie Antworten auf häufig auftretende Probleme beim Konfigurieren oder Verwalten von Azure Active Directory-Domänendienste unsere [Fehlersuche](active-directory-ds-troubleshooting.md) .


### <a name="configuration"></a>Konfiguration

#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>Können mehrere Domänen für ein einzelnes erstellen Azure AD-Verzeichnis?
Nein. Sie können nur eine einzelne Domäne vom Azure Active Directory-Domänendienste für ein einzelnes erstellen Azure AD-Verzeichnis.  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Kann Azure AD-Domänendienste in einem virtuellen Netzwerk Azure-Ressourcen-Manager aktivieren?
Nein. Azure Active Directory-Domänendienste können nur in einem klassischen Azure virtuelles Netzwerk aktiviert. Sie können ein Ressourcenmanager virtuelles Netzwerk virtuelles Netzwerk peering Verwendung die verwaltete Domäne in einem virtuellen Netzwerk Ressourcen-Manager das klassische virtuelle Netzwerk verbinden.

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Kann ich Azure Active Directory-Domänendienste in mehrere virtuelle Netzwerke in mein Abonnement zur Verfügung?
Der Dienst selbst unterstützt dieses Szenario nicht direkt. Azure Active Directory-Domänendienste ist jeweils nur in einem virtuellen Netzwerk verfügbar. Allerdings können Sie Konnektivität zwischen Azure AD-Domänendienste auf andere virtuelle Netzwerke verfügbar machen mehrere virtuelle Netzwerke konfigurieren. Dieser Artikel beschreibt, wie [virtuelle Netzwerke in Azure herstellen](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)können.

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>Kann Azure AD-Domänendienste mit PowerShell aktivieren?
PowerShell/automatisierte Bereitstellung von Azure Active Directory-Domänendienste ist derzeit nicht verfügbar.

#### <a name="is-azure-ad-domain-services-available-in-the-new-azure-portal"></a>Steht Azure Active Directory-Domänendienste im neuen Azure-Portal?
Nein. Azure kann AD-Domäne nur im [klassischen Azure-Portal](https://manage.windowsazure.com)konfiguriert werden. Wir erwarten in Zukunft Unterstützung für [Azure-Portal](https://portal.azure.com) erweitern.

#### <a name="can-i-add-domain-controllers-to-an-azure-ad-domain-services-managed-domain"></a>Können Domänencontroller einer verwalteten Azure Active Directory-Domänendienste-Domäne werden hinzugefügt?
Nein. Die Domäne von Azure Active Directory-Domänendiensten bereitgestellt ist einer verwalteten Domäne. Nicht bereitstellen, konfigurieren und Domänencontroller für diese Domäne - Verwaltung müssen diese Aktivitäten als Dienst von Microsoft erhalten. Daher kann keine zusätzliche Domänencontroller (Lese-/ Schreibzugriff oder schreibgeschützt) der verwalteten Domäne hinzufügen.

### <a name="administration-and-operations"></a>Verwaltung und Betrieb

#### <a name="can-i-connect-to-the-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Kann ich für meine verwalteten Domäne mithilfe von Remotedesktop auf den Domänencontroller verbinden?
Nein. Sie verfügen nicht Berechtigungen zum Domänencontroller verwalteten Domäne über Remotedesktop herstellen. Mitglieder der Gruppe "AAD DC Administratoren" können die verwaltete Domäne AD-Verwaltungstools wie Active Directory Administration Center (ADAC) oder AD PowerShell verwalten. Diese Tools werden mit der Funktion "Remoteserver-Verwaltungstools" auf einem Windows-Server verwalteten Domäne installiert.

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-to-domain-join-machines-to-this-domain"></a>Ich habe Azure Active Directory-Domänendienste aktiviert. Welche Benutzerkonto werden diese Domäne Domäne beitreten Maschinen werden verwendet?
Benutzer der administrativen Gruppe (z. B. "AAD DC Administrators') hinzugefügten können Domänenbeitritt Computer. Darüber hinaus erhalten Benutzer in dieser Gruppe remote desktop-Zugriff auf Computer, die der Domäne angehören.

#### <a name="can-i-wield-domain-administrator-privileges-for-the-domain-provided-by-azure-ad-domain-services"></a>Kann Domäne über Administratorrechte für die Domäne von Azure Active Directory-Domänendiensten bereitgestellt haben?
Nein. Sie werden nicht über Administratorrechte in der verwalteten Domäne gewährt. "Domänenadministrator" und "Enterprise Administrator-Rechte sind nicht zur Verwendung innerhalb der Domäne. Bestehenden Domänenadministrator oder Enterprise Administratorgruppen innerhalb Ihres Verzeichnisses Azure AD auch keine Administratorrechte für die Domäne Domäne Unternehmen erhalten.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-domains-provided-by-azure-ad-domain-services"></a>Kann ich Gruppenmitgliedschaften mit LDAP oder andere Active Directory-Verwaltungsprogramme in Azure Active Directory-Domänendienste-Domänen ändern?
Nein. Gruppenmitgliedschaften können vom Azure Active Directory-Domänendienste Domänen geändert werden. Das gleiche gilt für Benutzerattribute. Sie können jedoch in Azure AD oder auf Ihrer lokalen Domäne Gruppenmitgliedschaften oder Benutzerattribute ändern. Eine solche Änderung werden in Azure AD-Domänendienste automatisch synchronisiert.

#### <a name="can-i-extend-the-schema-of-the-domain-provided-by-azure-ad-domain-services"></a>Kann das Schema der Dienstleistungen Azure AD-Domäne Domäne?
Nein. Das Schema wird von Microsoft verwalteten Domäne verwaltet. Schema-Erweiterung werden von Azure Active Directory-Domänendienste nicht unterstützt.

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Kann ändern oder hinzufügen DNS-Datensätze in Meine verwalteten Domäne?
Ja. Benutzer der Gruppe "Administratoren AAD DC" erhalten "DNS-Administrator Berechtigungen zum Ändern der DNS-Einträge in der verwalteten Domäne. Diese DNS-Manager-Konsole auf einem Computer mit Windows Server verwalteten Domäne können Benutzer DNS verwalten. Um die DNS-Manager-Konsole verwenden, installieren Sie "DNS Server Tools" ist Teil des "Remoteserver-Verwaltungstools" optionale Features auf dem Server. Weitere Informationen über [Dienstprogramme für die Verwaltung, Überwachung und Problembehandlung von DNS](https://technet.microsoft.com/library/cc753579.aspx) ist über TechNet verfügbar.


### <a name="billing-and-availability"></a>Abrechnung und Verfügbarkeit

#### <a name="is-azure-ad-domain-services-a-paid-service"></a>Ist Azure AD-Domäne kostenpflichtig?
Ja. Weitere Informationen finden Sie unter [Preisseite](https://azure.microsoft.com/pricing/details/active-directory-ds/).

#### <a name="is-there-a-free-trial-for-the-service"></a>Gibt es eine kostenlose Testversion für den Dienst?
Dieser Dienst ist in der kostenlosen Testversion für Azure enthalten. Sie können einen [einmonatigen Testversion von Azure](https://azure.microsoft.com/pricing/free-trial/)anmelden.

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems"></a>Erhalte ich Azure Active Directory-Domänendienste als Teil des Enterprise Mobility Suite (EMS)?
#### <a name="do-i-need-azure-ad-premium-to-use-azure-ad-domain-services"></a>Muss Azure AD Premium Azure Active Directory-Domänendienste verwenden?
Nein. Azure Active Directory-Domänendienste ist nutzungsbasierte Azure Service und ist nicht Teil des EMS. Azure Active Directory-Domänendienste stehen für alle Editionen von Azure AD (kostenlose, einfache und Premium) und stündlich, je nach Nutzung abgerechnet.

#### <a name="what-azure-regions-is-the-service-available-in"></a>Ist Azure Regionen der Dienst in?
Siehe Seite [Azure Services nach Region](https://azure.microsoft.com/regions/#services/) , eine Liste der Azure-Regionen Azure Active Directory-Domänendienste ist verfügbar.
