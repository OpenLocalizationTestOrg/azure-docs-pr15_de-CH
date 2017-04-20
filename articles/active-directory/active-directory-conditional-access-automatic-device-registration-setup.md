<properties
    pageTitle="Richten Sie automatische Registrierung des Windows-Domäne Geräte mit Active Directory Azure | Microsoft Azure"
    description="Richten Sie Ihre Domäne Windows automatisch und im Hintergrund mit Azure Active Directory registriert."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="markvi"/>



# <a name="set-up-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>Automatische Registrierung von Windows Azure Active Directory-Domäne Geräte einrichten

Verwendung von [Azure Active Directory Gerät bedingten Zugriff](active-directory-conditional-access.md)müssen Windows Domäne beigetretenen Computer Azure Active Directory (Azure AD) registriert werden. In diesem Artikel erfahren Sie, was Sie tun müssen, um Registrierung Windows Domäne Geräte mit Azure in Ihrer Organisation.

Mit Zugangskontrolle in Azure AD bietet Ihnen folgende Vorteile:

-   Erweiterte einmaliges Anmelden (SSO) bei Azure AD apps durch Geschäfts-oder
-   Enterprise roaming Settings auf Geräten
-   Zugriff auf Windows Store für Unternehmen
-   Stärkere Authentifizierung und einfache Anmeldung mit Windows Hello

> [AZURE.NOTE] Windows 10 November Update bietet verbesserte Benutzerfunktionalität in Azure AD, aber Windows 10 Jahre Update unterstützt Geräte bedingten Zugriff. Weitere Informationen über bedingte finden Sie unter [Active Directory Azure Gerät bedingten Zugriff](active-directory-conditional-access.md). Weitere Informationen über Windows 10 Geräte am Arbeitsplatz und wie Benutzer ein 10 Windows Azure AD registriert finden Sie [Windows 10 für Unternehmen: Geräte für Arbeit verwenden](active-directory-azureadjoin-windows10-devices-overview.md).

Sie können einigen früheren Versionen von Windows, einschließlich Versionen registrieren:

-   Windows 8.1
-   Windows 7

Wenn Sie Windows Server-Computers als Desktop verwenden, können Sie diese Plattformen registrieren:

- WindowsServer 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2


## <a name="prerequisites"></a>Erforderliche Komponenten

Die wichtigste Voraussetzung für die automatische Registrierung von Geräten mithilfe von Azure AD-Domäne ist eine aktuelle Version von Azure Active Directory verbinden (Azure AD Connect).

Je nach Bereitstellung von Azure AD Connect und ob eine Schnellinstallation oder benutzerdefinierte Installation oder eine direkte Aktualisierung verwendet die folgenden Komponenten möglicherweise automatisch konfiguriert wurden:

-   **Service Connection Point im lokalen Active Directory**. Suche nach Azure Mieter Anzeigeninformationen Computer registrieren, Azure AD.

-   **Active Directory-Verbunddienste (AD FS) Ausstellung transformieren Regeln**. Für die Authentifizierung bei der Registrierung (für föderierte Konfigurationen).

Wenn einige Geräte in Ihrem Unternehmen nicht Windows 10 Domäne Geräte sind, sollten Sie die folgenden Aufgaben ausführen:

- Eine Richtlinie in Azure AD festgelegt, damit Benutzer Geräte registrieren kann
- Festlegen Sie integrierte Windows-Authentifizierung (IWA) als Alternative zu mehrstufige Authentifizierung in AD FS



## <a name="set-a-service-connection-point-for-discovery-of-the-azure-ad-tenant"></a>Legen Sie einen Dienstverbindungspunkt für die Ermittlung von Azure AD-Mandanten

Ein Service-Objekt muss vorhanden sein, in die Konfigurationsnamenpartition Kontext der Gesamtstruktur der Domäne, Computer hinzugefügt werden. Der Dienstverbindungspunkt enthält Informationen über Azure AD-Mandanten, auf dem Computer registrieren. In einer Active Directory-Konfiguration mit mehreren Gesamtstrukturen muss der Dienstverbindungspunkt in allen Gesamtstrukturen vorhanden sind, Domänencomputern verwenden.

Legen Sie den Dienstverbindungspunkt an diesen Standorten in Active Directory:

    CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Configuration Naming Context]

> [AZURE.NOTE] Für eine Gesamtstruktur mit dem Active Directory-Domäne *example.com*, Konfigurationsnamenskontext CN ist = Configuration, DC = Beispiel, DC = com.

Mit dem folgenden Windows PowerShell-Skript können Sie für Azure AD Mieter Objekt und Erkennung überprüfen. (Ersetzen Sie Konfigurationsnamenskontext im Beispiel mit der Konfigurationsnamenskontext.)

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=example,DC=com";

    $scp.Keywords;

Die `$scp.Keywords` Ausgabe zeigt Azure AD Tenant-Informationen:

    azureADName:microsoft.com

    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Wenn der Dienstverbindungspunkt nicht existiert, Erstellen von PowerShell-Skript auf dem Azure AD Connect Server ausgeführt:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


> [AZURE.NOTE]Beim Ausführen von `$aadAdminCred = Get-Credential`, verwenden Sie das Format *user@example.com* für den Benutzernamen im Dialogfeld **Get-Credential** .  
> Beim Ausführen des Initialize-ADSyncDomainJoinedComputerSync-Cmdlets ersetzen Sie [*Connector Kontoname*] mit dem Domänenkonto in Active Directory Connector-Konto verwendet wird.  
> Das Cmdlet verwendet Active Directory PowerShell-Modul, die Active Directory-Webdienste in einer Domäne benötigt. Active Directory-Webdienste ist auf Domänencontrollern unter Windows Server 2008 R2 und höher unterstützt. Domänencontroller in Windows Server 2008 oder früher verwenden Sie System.DirectoryServices API über PowerShell, um den Dienstverbindungspunkt erstellen und dann weisen Sie die Schlüsselwörter Werte zu.

## <a name="create-ad-fs-rules-for-instant-device-registration-in-federated-organizations"></a>Erstellen Sie AD FS Regeln für instant geräteregistrierung in föderierten Organisationen

In einer verbundenen Azure AD-Konfiguration Geräte basieren auf AD FS (oder auf dem lokalen Verbundserver) Azure AD authentifizieren. Registrieren sie dann gegen Azure Active Directory Gerät Registrierung-Dienst Azure AD Gerät Registrierung.

> [AZURE.NOTE] In einer Konfiguration ohne Verband Benutzerkennworthashes Azure AD synchronisiert und Windows 10 und Windows Server 2016 Domänencomputern Authentifizierung Registrierungsdienst Azure AD Gerät. Benutzerauthentifizierung mit Anmeldeinformationen der Benutzer in ihre lokale Computerkonten schreibt und die Azure AD über Azure AD Connect weitergeleitet. Für Windows 10 und Windows Server 2016 Computer in einer Konfiguration ohne Verband stehen Ihnen Optionen zum Gerät basierende Zertifizierungsstelle in Ihrer Organisation festlegen. Weitere Informationen finden Sie im Abschnitt Download Windows Installer-Pakete für Windows 10 Computer in diesem Artikel.

Für Windows 10 und Windows Server 2016 ordnet Azure AD verbinden das Geräteobjekt in Azure AD lokalen Computerkontoobjekt. Folgenden Angaben müssen vorhanden sein, während der Authentifizierung für Azure AD Gerät Registrierungsdienst, Registrierung und das Objekt zu erstellen:

- http://Schemas.Microsoft.com/WS/2012/01/AccountType enthält den DJ-Wert den principal Authentifikator als Domäne beigetretenen Computer identifiziert.
- http://Schemas.Microsoft.com/Identity/Claims/onpremobjectguid enthält den Wert des Attributs **ObjectGUID** des lokalen Computerkontos.
- http://Schemas.Microsoft.com/WS/2008/06/Identity/Claims/primarysid enthält den Computer primäre Sicherheits-ID (SID) **ObjectSid** -Attributwert des lokalen Computerkontos entspricht.
- http://Schemas.Microsoft.com/WS/2008/06/Identity/Claims/issuerid enthält den Wert, die Azure AD verwendet das Token ausgestellt von AD FS oder aus der lokalen Sicherheit Sicherheitstokendienst (STS) vertrauen. Dies muss in einer Active Directory-Konfiguration mit mehreren Gesamtstrukturen. In dieser Konfiguration können Computer einer anderen Gesamtstruktur angehören, die Azure AD in AD FS herstellt oder lokalen STS. Verwenden Sie den Wert für AD FS http://<*Domänenname*>/Adfs/services/Trust/<*Domänenname*> ist die überprüfte Domänennamen in Azure AD.

Zum Erstellen dieser Regeln manuell in AD FS verwenden Sie das folgende PowerShell-Skript in einer Sitzung mit dem Server verbunden ist. Ersetzen Sie die erste Zeile mit Ihrem Domänennamen überprüften Unternehmen in Azure AD.

> [AZURE.NOTE] Wenn Sie Ihre lokalen Verbundservers AD FS nicht verwenden, Anleitung Ihres Herstellers Regeln erstellen, die diese Ansprüche ausstellen.

    $validatedDomain = "example.com"      # Replace example.com with your organization's validated domain name in Azure AD

    $rule1 = '@RuleName = "Issue object GUID"

        c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

        c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

        => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain-joined computers"

      c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

> [AZURE.NOTE] Verwenden Sie nur die ersten drei Regeln, wenn die lokale Umgebung ist eine Gesamtstruktur. Sind die Computer in einer anderen Gesamtstruktur als die Synchronisierung mit Azure AD oder der alternative Namen in die Synchronisierungskonfiguration verwenden, müssen Sie die übrigen Regeln angeben.

    $rule3 = '@RuleName = "Pass through primary SID"

      c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(claim = c2);'

    $rule4 = '@RuleName = "Issue AccountType with the value User when it’s not a computer account"

      NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"])

      => add(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "User");'

    $rule5 = '@RuleName = "Capture UPN when AccountType is User and issue the IssuerID"

      c1:[Type == "http://schemas.xmlsoap.org/claims/UPN"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "User"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c1.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));'

    $rule6 = '@RuleName = "Update issuer for DJ computer auth"

      c1:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = "http://'+$validatedDomain+'/adfs/services/trust/");'

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5 + $rule6

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

> [AZURE.NOTE] Windows 10 und Windows Server 2016 Domänencomputern Authentifizierung über IWA zu einem aktiven WS-Trust-Endpunkt, der von AD FS gehostet. Stellen Sie sicher, dass der Endpunkt aktiv ist. Bei Verwendung der Webproxy Anwendung unbedingt diesen Endpunkt über den Proxy veröffentlicht wird. Der Endpunkt ist Adfs/Services/Trust/13/Windowstransport. Überprüfen, ob er aktiv zum AD FS-Verwaltungskonsole **Service** > **Endpunkte**. Wenn Sie Ihre lokalen Verbundservers AD FS nicht verwenden, Anweisungen des Kreditors zu der entsprechende Endpunkt aktiv ist.



## <a name="set-up-ad-fs-for-authentication-of-device-registration"></a>Einrichten von ADFS zur Authentifizierung des geräteregistrierung

Stellen Sie sicher, dass als Alternative zu mehrstufige Authentifizierung für die Registrierung des Geräts in AD FS IWA festgelegt ist. Hierzu müssen Sie Ausgabe Regel umwandeln, das die Methode durchläuft.

1. Wechseln Sie in die AD FS-Verwaltungskonsole zu **AD FS** > **Vertrauensstellungen** > **Vertrauen verlassen**.

2. Microsoft Office 365 Identitätsplattform relying Party Vertrauensobjekt Maustaste, und wählen Sie **Anspruchsregeln bearbeiten**.

3.  Wählen Sie auf der Registerkarte **Ausgabe transformieren Regeln** **Regel hinzufügen**.

4.  Wählen Sie in der Vorlagenliste **Anspruchsregel** **Ansprüche mithilfe einer benutzerdefinierten Regel senden**.

5.  Wählen Sie **Weiter**.

6.  Geben Sie im Feld **Name der Anspruchsregel** **Authentifizierungsregel Methode Anspruch**.

7.  Geben Sie im Feld **Anspruchsregel** diesen Befehl:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
    => issue(claim = c);`.

8. Geben Sie auf der Verbundserver PowerShell-Befehl:

    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

<*RPObjectName*> ist der relying Party Objektname Azure AD relying Party vertrauen Objekt. Dieses Objekt wird normalerweise *Microsoft Office 365 Identitätsplattform*benannt.



## <a name="deployment-and-rollout"></a>Bereitstellung und rollout

Wenn Domänencomputern Voraussetzung erfüllen, sind mit Azure registrieren.

Die Windows 10 Jahre Update und Windows Server 2016 Domänencomputern registrieren automatisch beim nächsten Neustart des Geräts oder wenn ein Benutzer meldet sich bei Windows Azure AD. Neue Computer, die Mitglied der Domäne registrieren Azure AD beim Neustart des Geräts nach der Domäne Join-Operation.

> [AZURE.NOTE] Windows 10 Domänencomputern registrieren automatisch mit Azure, nur, wenn das Rollout Gruppenrichtlinienobjekt festgelegt.

Ein Gruppenrichtlinienobjekt können die Einführung einer automatischen Registrierung von Windows 10 und Windows Server 2016 Domänencomputern steuern. Automatische Registrierung Windows 10 Domänencomputern ausgeführt, können Sie ein Windows Installer-Paket auf Computern bereitstellen, die Sie auswählen.

> [AZURE.NOTE] Die Gruppenrichtlinie für Rollout-Steuerelement löst auch die Registrierung von Windows 8.1 Domänencomputern. Die Richtlinien können für Windows 8.1 Domänencomputern registrieren. Oder haben Sie aus Windows-Versionen, einschließlich Windows 7 oder Windows Server-Versionen Anmeldung alle Windows 10 und Windows Server 2016 Computer mithilfe von Windows Installer-Paket.

### <a name="create-a-group-policy-object-to-control-the-rollout-of-automatic-registration"></a>Erstellen Sie ein Gruppenrichtlinienobjekt zum Steuern der Bereitstellung der automatischen Registrierung

Steuern die Einführung einer automatischen Registrierung von Domänencomputern Azure AD können Sie Gruppenrichtlinien **Domänencomputern Geräte registrieren** auf den Computern bereitstellen zu registrieren. Beispielsweise können Sie die Richtlinie zu einer Organisationseinheit oder einer Sicherheitsgruppe bereitstellen.

Führen Sie diese Schritte, um die Richtlinie festgelegt:

1. Öffnen Sie Server-Manager und gehen Sie zu **Extras** > **Verwaltung**.

2. Fahren Sie mit dem Domänenknoten, der die der Domäne entspricht Autom. 10 für Windows oder Windows Server 2016 Computer aktiviert werden soll.

3. Maustaste auf **Gruppenrichtlinienobjekte**, und wählen Sie **neu**.

4. Geben Sie einen Namen für die Gruppenrichtlinienobjekt. Beispielsweise *Die automatische Registrierung in Azure AD*. Wählen Sie **OK**.

4. Ihr neue Gruppenrichtlinienobjekt Maustaste, und wählen Sie **Bearbeiten**.

5. **Computerkonfiguration**zur > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Geräteregistrierung**. **Register Domäne Computer Geräte**Maustaste, und wählen Sie **Bearbeiten**.

    > [AZURE.NOTE] Diese Vorlage wurde aus früheren Versionen von Gruppenrichtlinien-Verwaltungskonsole umbenannt. Wenn Sie eine frühere Version der Konsole verwenden, gehen Sie zu **Computerkonfiguration** > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Arbeitsplatz Join** > **automatisch Arbeitsplatz Join Clientcomputer**.

6. Wählen Sie **aktiviert**und dann **Übernehmen**.

7. Wählen Sie **OK**.

8. Verknüpfen Sie das Gruppenrichtlinienobjekt an einem Speicherort Ihrer Wahl. Beispielsweise können Sie es auf eine bestimmte Organisationseinheit verknüpfen. Auch kann mit einer bestimmten Sicherheitsgruppe Computer verknüpft, die automatisch mit Azure registrieren. Um diese Richtlinie für alle Windows 10 und Windows Server 2016 Domänencomputern in Ihrer Organisation festgelegt, verknüpfen Sie das Gruppenrichtlinienobjekt mit der Domäne.

### <a name="download-windows-installer-packages-for-non-windows-10-computers"></a>Windows Installer-Pakete für Windows 10 Computer herunterladen  

Registrieren Sie Domäne Computern Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2 können Sie herunterladen und Installieren dieser Windows Installer-Paketdateien (MSI):

- [x64](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x64.msi)
- [x86](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x86.msi)

Bereitstellen Sie das Paket mithilfe von Softwareverteilungssystem wie System Center Configuration Manager. Das Paket unterstützt die Optionen standard Hintergrundinstallation mit dem *stillen* Parameter. System Center Configuration Manager 2016 bietet zusätzliche Vorteile aus früheren Versionen, wie abgeschlossene Registrierungen nachverfolgen. Weitere Informationen finden Sie in [System Center 2016](https://www.microsoft.com/en-us/cloud-platform/system-center).

Das Installationsprogramm erstellt einen geplanten Task auf dem System im Kontext des Benutzers ausgeführt wird. Der Task wird ausgelöst, wenn der Benutzer bei Windows anmeldet. Aufgabe registriert das Gerät automatisch mit Azure die Benutzeranmeldeinformationen nach der Authentifizierung durch IWA. Den geplanten Task finden unter **Microsoft** > **Arbeitsplatz zu verknüpfen**, und dann der Taskplaner-Bibliothek.



## <a name="next-steps"></a>Nächste Schritte

- [Azure Active Directory Zugangskontrolle](active-directory-conditional-access.md)
