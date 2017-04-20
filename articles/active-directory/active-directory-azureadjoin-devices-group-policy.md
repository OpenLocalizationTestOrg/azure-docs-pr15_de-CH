<properties
    pageTitle="Geräte Domäne Azure AD für Windows 10 Erfahrungen | Microsoft Azure"
    description="Erklärt, wie Administratoren Gruppenrichtlinien um Geräte mit dem Unternehmensnetzwerk Domäne aktivieren konfigurieren können."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Geräte Domäne Azure AD für Windows 10 Erlebnisse

Domänenbeitritt ist traditionell Organisationen für die Arbeit der letzten 15 Jahre angeschlossen haben. Es wurde aktiviert Benutzer anmelden sich Windows Server Active Directory (AD) Arbeit oder Schule Konten und IT-Geräte vollständig verwalten. Unternehmen in der Regel auf imaging Methoden stellen Sie Geräte, Benutzer und verwenden im allgemeinen System Center Configuration Manager (SCCM) oder Gruppenrichtlinien verwalten.

Beitreten zu einer Domäne in Windows 10 bietet die folgenden Vorteile nach in Azure Active Directory (Azure AD Geräten):

- Einmaliges Anmelden (SSO) Azure AD Ressourcen überall
- Zugriff auf Enterprise Windows Store mit Arbeits- oder Schulcomputer Konten (kein Microsoft-Konto erforderlich)
- Enterprise-kompatible roaming User Settings auf Geräten mit Arbeits- oder Schulcomputer Konten (kein Microsoft-Konto erforderlich)
- Starke Authentifizierung und einfache Anmeldung für Arbeit oder Schule mit Microsoft Passport Windows Hello
- Fähigkeit, Zugriff nur für Geräte, die organisatorische geräteeinstellungen entsprechen

## <a name="prerequisites"></a>Erforderliche Komponenten

Beitreten zu einer Domäne weiterhin nützlich. Allerdings profitieren Sie von SSO Azure AD, roaming Einstellungen mit Arbeit oder Schule und den Zugriff auf Windows Store mit Geschäfts-oder benötigen Sie Folgendes:

- Azure AD-Abonnement
- Azure AD mit lokalen Verzeichnis Azure AD erweitern
- Richtlinie wurde zum Azure AD Geräte Domäne herstellen
- Windows 10 Build (Build 10551 oder höher) für Geräte

Um Microsoft Passport und Windows Hello zu aktivieren, benötigen Sie auch Folgendes:

- Infrastruktur öffentlicher Schlüssel (PKI) für Benutzer Zertifikate ausstellen.
- System Center Configuration Manager Version 1509 Technical Preview. Weitere Informationen finden Sie in [Technical Preview von Microsoft System Center Configuration Manager](https://technet.microsoft.com/library/dn965439.aspx#BKMK_TP3Update) und [System Center Configuration Manager-Teamblog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx). Dies ist erforderlich, Zertifikate basierend auf Microsoft Passport Schlüssel bereitstellen.

Als Alternative zur PKI-Bereitstellung erforderlich können Sie Folgendes ein:

- Haben Sie einige Domänencontroller mit Windows Server 2016 Active Directory Domain Services.

Bedingter Zugriff können Sie Gruppenrichtlinien erstellen, die Zugriff auf Domäne Geräte keine zusätzlichen Installationen. Um Zugriffskontrolle basierend auf dem Gerät verwalten zu können, benötigen Sie Folgendes:

- System Center Configuration Manager Version 1509 Technical Preview für Passport-Szenarien

## <a name="deployment-instructions"></a>Anweisungen



### <a name="step-1-deploy-azure-active-directory-connect"></a>Schritt 1: Bereitstellung von Azure Active Directory herstellen

Azure AD Connect können Sie Computer lokal als Geräteobjekte in der Cloud bereitstellen. Zum Bereitstellen von Azure AD Connect finden Sie unter "Installation Azure AD Connect" im Artikel [Ihrer lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md#install-azure-ad-connect).

 - Wenn Sie eine [benutzerdefinierte Installation für Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md) (nicht die Express-Installation) gefolgt, dann gehen Sie **im lokalen Active Directory ein Dienstverbindungspunkt erstellen**in diesem Schritt.
 - Haben Sie verbundene Konfiguration Azure AD vor der Installation von Azure AD verbinden (z. B. Active Directory Federation Services (AD FS) vor bereitgestellt haben), dann gehen Sie **Configure AD FS Anspruchsregeln** , in diesem Schritt.

#### <a name="create-a-service-connection-point-in-on-premises-active-directory"></a>Erstellen Sie im lokalen Active Directory einen Dienstverbindungspunkt

Domäne Geräte verwenden den Dienstverbindungspunkt Azure Mieterinformationen zum Zeitpunkt der automatischen Registrierung mit den Registrierungsdienst Azure Gerät ermitteln.

Azure AD Connect-Server führen Sie die folgenden PowerShell-Befehle:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


Wenn das Cmdlet $aadAdminCred = Get-Credential, verwenden Sie das Format *user@example.com* für den Benutzernamen der Anmeldeinformationen eingegeben haben, erscheint das Get-Credential-Popup.

Beim Ausführen des Cmdlets Initialize ADSyncDomainJoinedComputerSync... Ersetzen Sie [*Connector Kontoname*] mit dem Domänenkonto als Active Directory Connector-Konto verwendet wird.

#### <a name="configure-ad-fs-claim-rules"></a>AD FS Anspruchsregeln konfigurieren
Konfigurieren von Anspruchsregeln AD FS können direkte Registrierung eines Computers mit Azure Gerät Registrierungsdienst, da die Computer die Authentifizierung mithilfe von Kerberos/NTLM über AD FS. Ohne diesen Schritt erhalten Computer Azure AD auf verzögerte (unter Azure AD Sync Verbindungszeiten).

>[AZURE.NOTE]
Wenn AD FS als Föderation Server lokalen haben, gehen Sie des Herstellers der forderungsregeln erstellen.

Führen Sie folgende PowerShell Befehle, auf dem AD FS-Server (oder in einer Sitzung mit dem AD FS-Server verbunden):

      <#----------------------------------------------------------------------
     |   Modify the Azure AD Relying Party to include the claims needed
     |   for DomainJoin++. The rules include:
     |   -ObjectGuid
     |   -AccountType
     |   -ObjectSid
     +---------------------------------------------------------------------#>

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $rule1 = '@RuleName = "Issue object GUID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain joined computers"
          c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

    $rule3 = '@RuleName = "Pass through primary SID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(claim = c2);'

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

>[AZURE.NOTE]
Windows 10 Computern authentifiziert durch integrierte Windows-Authentifizierung mit einem aktiven WS-Trust-Endpunkt von AD FS gehostet. Sicherstellen Sie, dass diesen Endpunkt aktiviert ist. Verwendung der Web-Proxy-Authentifizierung auch sicher, dass dieser Endpunkt über den Proxy veröffentlicht wird. Sie können hierzu die Adfs/Services/Trust/13/Windowstransport überprüfen. Weisen als AD FS-Verwaltungskonsole unter **Service**aktiviert > **Endpunkte**.


### <a name="step-2-configure-automatic-device-registration-via-group-policy-in-active-directory"></a>Schritt 2: Konfigurieren Sie automatische Registrierung über Gruppenrichtlinien in Active Directory

Gruppenrichtlinien können in Active Directory Sie die Windows 10 Domäne Geräte sich automatisch mit Azure konfigurieren.

> [AZURE.NOTE]
> Aktuelle Informationen zum automatische Registrierung Siehe [Automatische Registrierung des Windows-Domäne einrichten mit Azure Active Directory verbunden](active-directory-conditional-access-automatic-device-registration-setup.md).
>
> Diese Vorlage wurde umbenannt in Windows 10. Wenn Sie die Gruppenrichtlinie auf einem Computer Windows 10 ausführen, erscheint die Richtlinie als: <br>
> **Domäne verbundene Computer als Geräte registrieren**<br>
> Die Richtlinie ist am folgenden Speicherort:<br>
> ***Computer Konfiguration/Richtlinien/Administrative Vorlagen/Windows-Komponenten-Gerät Registrierung***


## <a name="additional-information"></a>Weitere Informationen
* [Windows 10 für Unternehmen: Verwendungsmöglichkeiten für Geräte für die Arbeit](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern von Cloud-Funktionen für Windows 10 Geräte Azure Active Directory beitreten](active-directory-azureadjoin-user-upgrade.md)
* [Informationen Sie zu Verwendungsszenarios für Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Geräte Domäne Azure AD für Windows 10 Erlebnisse](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD Join](active-directory-azureadjoin-setup.md)
