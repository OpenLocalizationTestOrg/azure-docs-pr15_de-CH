<properties
    pageTitle="Azure Active Directory-Domänendienste: Kennwortsynchronisation aktivieren | Microsoft Azure"
    description="Erste Schritte mit Azure Active Directory-Domänendienste"
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
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Synchronisierung von Kennwörtern in Azure AD-Domänendienste aktivieren
In vorhergehenden Aufgaben aktiviert Azure Active Directory-Domänendienste für Azure AD-Mandanten. Die nächste Aufgabe ist die Synchronisierung von Kennwörtern Azure Active Directory-Domänendienste. Wenn Anmeldeinformationen Synchronisierung eingerichtet ist, können Benutzer über ihre Unternehmensanmeldeinformationen der verwalteten Domäne anmelden.

Die Schritte unterscheiden sich abhängig, ob Ihr Unternehmen nur Cloud Azure AD verfügt Mieter oder mit Ihrem lokalen Verzeichnis mit Azure AD verbinden synchronisieren soll.

<br>

> [AZURE.SELECTOR]
- [Nur Cloud Azure AD Mieter](active-directory-ds-getting-started-password-sync.md)
- [Azure AD-Mandanten synchronisiert](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-synced-azure-ad-tenant"></a>Aufgabe 5: Aktivieren Kennwortsynchronisation AAD Domain Services für synchronisierte Azure AD-Mandanten
Eine synchronisierte Azure AD-Mandanten mit Azure AD Verbinden mit lokalen Verzeichnis der Organisation festgelegt ist. Azure AD Connect synchronisiert nicht NTLM und Kerberos-Anmeldeinformationen Hashes Azure AD standardmäßig. Um Azure Active Directory-Domänendienste verwenden zu können, müssen Sie Azure AD Connect synchronisieren Anmeldeinformationen Hashes für NTLM und Kerberos-Authentifizierung erforderlich. So aktivieren Sie Synchronisierung erforderlichen Anmeldeinformationen Hashes, Azure AD-Mandanten.


### <a name="install-or-update-azure-ad-connect"></a>Installieren oder Aktualisieren von Azure AD verbinden
Installieren Sie die neueste empfohlene Version von Azure AD Connect auf einem verknüpften Computer. Haben Sie eine vorhandene Instanz von Azure AD-Verbindung einrichten, müssen Sie aktualisieren, um die neueste Version von Azure AD Connect verwenden. Um bekannte Probleme-Fehler zu vermeiden, die möglicherweise bereits behoben wurden, stellen Sie sicher, dass Sie immer die neueste Version von Azure AD Connect verwenden.

**[Download Azure AD verbinden](http://www.microsoft.com/download/details.aspx?id=47594)**

Empfohlene Version: **1.1.281.0** - am 7. September 2016.

  > [AZURE.WARNING] Installieren Sie die neueste empfohlene Version der Azure AD mit die älteren Anmeldeinformationen (erforderlich für NTLM und Kerberos-Authentifizierung) aktivieren, Azure AD-Mandanten zu synchronisieren. Diese Funktion ist nicht verfügbar in früheren Versionen von Azure AD Connect oder mit älteren Dirsync-Tool.

Installationshinweise für Azure AD Connect sind folgende Artikel - [Erste Schritte mit Azure AD verbinden](../active-directory/active-directory-aadconnect.md)


### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>Aktivieren Sie NTLM und Kerberos-Anmeldeinformationen Hashes Azure AD-Synchronisierung
Führen Sie das folgende PowerShell-Skript auf pro AD-Gesamtstruktur vollständige Synchronisierung erzwingen, und aktivieren Sie alle lokalen Benutzer Anmeldeinformationen Hashes Azure AD-Mandanten synchronisieren. Dieses Skript ermöglicht die Anmeldeinformationen Hashes erforderlich für NTLM/Kerberos-Authentifizierung mit Azure AD-Mandanten synchronisiert werden.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

Je nach Größe des Verzeichnisses (Anzahl der Benutzer, Gruppen usw.), Synchronisierung von Anmeldeinformationen Hashes Azure AD dauert. Die Kennwörter werden verwendet Azure Active Directory-Domänendienste verwalteten Domäne kurz nach Anmeldeinformationen Hashes Azure AD synchronisiert wurden.


<br>

## <a name="related-content"></a>Verwandte Inhalte

- [Aktivieren Kennwortsynchronisation AAD Domain Services für Cloud nur Azure AD-Verzeichnis](active-directory-ds-getting-started-password-sync.md)

- [Eine verwaltete Azure Active Directory-Domänendienste-Domäne verwalten](active-directory-ds-admin-guide-administer-domain.md)

- [Einen Windows-Computer zu einer verwalteten Azure Active Directory-Domänendienste-Domäne hinzufügen](active-directory-ds-admin-guide-join-windows-vm.md)

- [Eine Red Hat Enterprise Linux VM zu einer verwalteten Azure Active Directory-Domänendienste-Domäne hinzufügen](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
