<properties
   pageTitle="Azure Verschlüsselung Windows und Linux IaaS VMs | Microsoft Azure"
   description="Das Papier Überblick Azure Datenträger Verschlüsselung für Windows und Linux IaaS VMs."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="krkhan"/>


#<a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a>Azure Verschlüsselung Windows und Linux IaaS VMs

Microsoft Azure ist stark verpflichtet Ihre Daten, datenhoheit können Sie Ihre Azure Daten durch eine Reihe von gehostete Steuerelement Technologien verschlüsseln, steuern und Verschlüsselungsschlüssel verwalten und überwachen Zugriff auf Daten. Dadurch Azure-Kunden die Flexibilität, die Lösung zu wählen, die ihren geschäftlichen Bedürfnissen am besten entspricht. In diesem Whitepaper werden wir eine neue technologielösung "Azure Verschlüsselung Windows und Linux IaaS VM" vorgestellt zu schützen und Ihre Daten entsprechend Ihren Sicherheitsrichtlinien und Compliance Zusagen. Das Papier bietet eine ausführliche Anleitung zur Verwendung die Azure Datenträger Verschlüsselungsfunktionen unterstützten Szenarien sowie der Benutzer auftreten.

**Hinweis**: bestimmte Empfehlungen möglicherweise erhöhte, Netzwerk oder Compute Ressourcenverwendung zusätzliche Lizenz oder Abonnement Kosten.

## <a name="overview"></a>Übersicht

Azure Disk Encryption ist eine neue Funktion, mit der Sie die virtuellen Datenträger Windows und Linux IaaS verschlüsseln kann. Azure Datenträgerverschlüsselung nutzt der Branche [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Standardfunktion von Windows und die Funktion [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux Verschlüsselung für das Betriebssystem und die Datenträger bereitstellen. Die Lösung ist integriert mit [Azure Schlüssel](https://azure.microsoft.com/documentation/services/key-vault/) zum Steuern und Verwalten der Datenträger Verschlüsselungsschlüssel und Geheimnisse in Ihrem Abonnement Key Vault dabei sicherzustellen, dass alle virtuellen Laufwerke ruhende in Azure-Speicher verschlüsselt werden.

Azure Verschlüsselung Windows und Linux IaaS VMs ist im **Allgemeinen Verfügbarkeit** Azure öffentliche überall Standard VMs und VMs mit Premium.

### <a name="encryption-scenarios"></a>Szenarios für die Verschlüsselung

Azure Disk Encryption-Lösung unterstützt Kunden Folgendes:

- Aktivieren der Verschlüsselung für neue IaaS VMs vor der Verschlüsselung VHD und Verschlüsselungsschlüssel erstellt
- Aktivieren der Verschlüsselung für neue IaaS VMs von Azure Galeriebilder erstellt
- Aktivieren der Verschlüsselung für vorhandene IaaS VMs in Azure ausgeführt
- Deaktivieren Sie die Verschlüsselung auf Windows IaaS VMs
- Deaktivieren Sie die Verschlüsselung für Datenlaufwerke für Linux IaaS VMs

Die Lösung unterstützt die folgenden für IaaS VMs in Microsoft Azure aktiviert:

- Integration in Azure-Tresor
- Standard-Tier VMs - [A, D, DS, G, GS usw. Serie IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)
- Aktivieren der Verschlüsselung für Windows und Linux IaaS VMs
- Deaktivieren Sie die Verschlüsselung auf Betriebssystem und Daten für Windows IaaS VMs
- Deaktivieren Sie die Verschlüsselung für Datenlaufwerke für Linux IaaS VMs
- Aktivieren der Verschlüsselung für IaaS VMs mit Windows Client-Betriebssystem
- Aktivieren Sie die Verschlüsselung auf Volumes mit Bereitstellungspfaden
- Aktivieren der Verschlüsselung für Linux VMs mit softwarebasierten RAID-System konfiguriert 
- Aktivieren der Verschlüsselung für Windows-VMs mit Storage Spaces konfiguriert
- Alle Azure öffentliche Bereiche werden unterstützt

Die Lösung unterstützt nicht die folgenden Szenarien, Features und Technologie eingeführt:

- Grundlegende Ebene IaaS VMs
- Deaktivieren Sie die Verschlüsselung auf OS für Linux IaaS VMs
- IaaS VMs mit klassischen VM Methode erstellt
- Integration mit Ihrem lokalen Key Management Service
- Windows Server 2016 Technical Preview wird in dieser Version nicht unterstützt.
- Azure Dateien (Azure Dateifreigabe), NFS (NFS) dynamische Volumes mit softwarebasierten RAID-Systeme konfiguriert Windows-VMs


### <a name="encryption-features"></a>Funktionen

Beim Aktivieren und Bereitstellen von Azure Verschlüsselung Azure IaaS VMs werden je nach der Konfiguration die folgenden Funktionen aktiviert:

- Verschlüsselung von Betriebssystemvolume Startvolume Ruhende Kunden eingelagerten schützen
- Verschlüsselung von Daten-Volumes/s Datenvolumen Ruhende Kunden eingelagert zu
- Deaktivieren Sie die Verschlüsselung auf Betriebssystem und Daten für Windows IaaS VMs
- Deaktivieren Sie die Verschlüsselung für Datenlaufwerke für Linux IaaS VMs
- Schlüssel und geheime Schlüssel bei Azure-Tresor Abonnement schützen
- Statusberichte Verschlüsselung verschlüsselte IaaS-VM
- Entfernen von Disk Encryption-Konfigurationen IaaS virtuellen Computer

Die Azure Verschlüsselung IaaS VMS für Windows- und Linux-Lösung enthält Datenträger Verschlüsselung Erweiterung für Windows, Disk Encryption-Erweiterung für Linux, Disk Encryption-PowerShell-Cmdlets, Disk Encryption CLI Cmdlets und Disk Encryption Azure Ressourcenmanager Vorlagen. Azure Datenträger Verschlüsselungssystem wird IaaS VMs unter Windows oder Linux-Betriebssystem unterstützt. Weitere Informationen zu den unterstützten Betriebssystemen anzeigen Sie erforderlichen Komponenten Abschnitt

**Hinweis**: ist kostenlos VM Festplatten mit Azure Datenträgerverschlüsselung verschlüsseln.

### <a name="value-proposition"></a>Value Proposition

Azure Disk Encryption Management ermöglicht die folgenden Geschäftsbedürfnisse in der Cloud:

-   IaaS VMs werden ruhende Industry standard-Verschlüsselung Technologie organisatorische Sicherheit und Vorschriften geschützt.
-   IaaS VM Boot unter Kunde Schlüssel und Richtlinien, und können sie deren Verwendung im Schlüsseltresor überwachen.


### <a name="encryption-workflow"></a>Verschlüsselung-Workflow

Die Schritte zum Aktivieren der Verschlüsselung Windows und der Linux VM erforderlich sind:

1. Der Kunde wählt eine Verschlüsselungsszenario obigen Szenarios Verschlüsselung
2. Kunde wählt in datenträgerverschlüsselung über Azure Datenträger Verschlüsselung Ressourcenmanager Vorlage oder PS-Cmdlets oder CLI-Befehl aktivieren und gibt die Verschlüsselungskonfiguration

    - Für verschlüsselte VHD Kundenszenarien Kunden Uploads verschlüsselte VHD auf ihre Storage Konto und Verschlüsselung Schlüssel die Schlüssel Depot und Verschlüsselungskonfiguration zum Aktivieren der Verschlüsselung auf einer neuen IaaS-VM
    - Erstellt der neuen VMs aus der Galerie Azure bereits vorhandenen VM in Azure bieten Kunden Verschlüsselungskonfiguration IaaS VM Verschlüsselung aktivieren

3. Debitor gewährt Zugriff auf Azure-Plattform ihre wichtigsten Depot zum Aktivieren der Verschlüsselung auf die IaaS-VM die Verschlüsselungsschlüsseln (BitLocker-Verschlüsselung Schlüssel für Windows-Systeme und Passphrase für Linux) gelesen
4. Erarbeitung Azure AD Anwendungsidentität verwendeten Verschlüsselung ihrer wichtigsten Tresor zum Aktivieren der Verschlüsselung auf die IaaS VM in #2 oben genannten Szenarien schreiben
5.  Azure VM-Servicemodell mit Verschlüsselung und Schlüssel Depotkonfiguration aktualisiert und Vorschriften verschlüsselt VM für den Kunden

![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a>Entschlüsselung Workflow

Die Schritte deaktivieren der Verschlüsselung des IaaS VM erforderlich sind:

1. Kunde wählt Verschlüsselung (entschlüsseln) auf einer laufenden IaaS VM in Azure Azure Datenträger Verschlüsselung Ressourcenmanager Vorlage oder PS-Cmdlets und gibt die Entschlüsselung Konfiguration.
2. Schritt Verschlüsselung deaktivieren deaktiviert die Verschlüsselung OS oder Datenträgers oder auf laufenden Neuerung von Windows. Jedoch deaktivieren OS Verschlüsselung für Linux Datenträger werden nicht wie in der oben genannten Dokumentation erwähnt. Schritt deaktivieren darf nur für Datenlaufwerke unter Linux VMs. 
4. Azure VM Servicemodell aktualisiert und VM IaaS entschlüsselten markiert. Der Inhalt der VM nicht statisch mehr verschlüsselt.
5. Der Verschlüsselungsvorgang deaktivieren Kunden wichtige Depot und den Verschlüsselungsschlüsseln nicht löschen-BitLocker-Verschlüsselung Schlüssel für Windows oder die Passphrase für Linux.

## <a name="prerequisites"></a>Erforderliche Komponenten

Folgende sind für Azure Datenträgerverschlüsselung in Azure IaaS VMs für die unterstützten Szenarios in der Übersicht aufgerufen aktivieren

- Benutzer müssen gültiges active Azure-Abonnement Ressourcen in Azure in unterstützten Regionen erstellen
- Azure Datenträgerverschlüsselung wird auf dem folgenden WindowsServer-SKUs - Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2 unterstützt. Windows Server 2016 Technical Preview wird in dieser Version nicht unterstützt.
- Azure Datenträgerverschlüsselung wird auf dem folgenden Windows Client-SKUs - Client Windows 8 und Windows 10 Clients unterstützt.

**Hinweis**: für Windows Server 2008 R2, .net Framework 4.5 installiert vor dem Aktivieren der Verschlüsselung in Azure. Sie können es von Windows Update installieren Installation optionaler Updates "Microsoft.NET Framework 4.5.2 für Windows Server 2008 R2 X 64-Systeme ([KB2901983](https://support.microsoft.com/kb/2901983))"

- Azure Datenträgerverschlüsselung wird auf dem folgenden LinuxServer - SKUs Ubuntu CentOS, SUSE und SUSE Linux Enterprise Server (SLES) und Red Hat Enterprise Linux unterstützt.

**Hinweis**: Linux-Betriebssystem datenträgerverschlüsselung wird derzeit in folgenden Linux-Distributionen - RHEL 7.2 CentOS 7.2 Ubuntu 16.04 unterstützt

- Alle Ressourcen (Ex: Key Vault, Storage Konto VM usw.) müssen die gleichen Azure und Abonnement gehören.

**Hinweis**: Azure datenträgerverschlüsselung erfordert eine Taste Depot und VMs in derselben Azure-Region befinden. In separaten Bereich konfigurieren verursacht Fehler in Azure Datenträger Verschlüsselung aktivieren.

- Einrichten und Konfigurieren von Azure Key Vault für Azure Verschlüsselung Datenträgerverwendung Abschnitt **festlegen und Konfigurieren von Azure Schlüssel für die Verschlüsselung Azure Datenträgerverwendung** im Abschnitt *erforderliche Komponenten* in diesem Artikel.
- Einrichten und Konfigurieren von Azure AD-Anwendung in Azure Active Directory für Azure Datenträgerverwendung Verschlüsselung finden Sie unter Abschnitt im Abschnitt *erforderliche* **Azure AD-Anwendung in Azure Active Directory einrichten** .
- Zum Einrichten und konfigurieren Key Vault-Richtlinien für Azure AD-Anwendung finden Abschnitt **Einstellung Key Vault-Richtlinien für Azure AD-Anwendung** im Abschnitt *erforderliche Komponenten* .
- Vorbereitung vor der Verschlüsselung Windows VHD Abschnitt **Vorbereiten einer virtuellen Festplatte vor der Verschlüsselung Windows** im Anhang dieses Artikels.
- Vorbereitung vor der Verschlüsselung Linux VHD Abschnitt **Vorbereiten einer virtuellen Festplatte vor der Verschlüsselung Linux** im Anhang dieses Artikels.
- Azure-Plattform benötigt Zugriff auf Schlüssel oder Kennwörter bei Azure Key Vault, um die virtuellen Computer starten und virtuellen OS Volume entschlüsseln verfügbar machen. Zur Azure-Plattform auf Kunden Key Vault zu gewähren, muss **EnabledForDiskEncryption** Eigenschaft auf den Schlüssel für diese Anforderung festgelegt werden. Siehe Abschnitt **festlegen und Konfigurieren von Azure Schlüssel für die Verschlüsselung Azure Datenträgerverwendung** im Anhang dieses Artikels Weitere Informationen.
- Geheime Schlüssel Depot und Key Encryption Key (KEK) URLs müssen mit Versionsangabe. Azure erzwingt diese Einschränkung der Versionskontrolle. Hier einige Beispiele für gültige Schlüssel und KEK-URL:
    - Beispiel für gültige geheime URL:   *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
    - Beispiel für gültige KRK KEK:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
- Verschlüsselung Azure Festplatten unterstützt als Vault Schlüssel geheim und KEK URLs Portnummern nicht. Hier einige Beispiele für unterstützte Schlüssel Vault-URL:
    - Abgelehnten Schlüssel Depot URL   *Https://contosovault.vault.azure.net:443/Geheimnisse/Contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
    - Akzeptierte-Depot URL:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
- Zum Aktivieren der Verschlüsselung von Azure Datenträger muss Feature IaaS VMs Netzwerk Endpunkt-Konfiguration Folgendes erfüllen: 
    - IaaS VM muss Active Directory Azure Endpunkt herstellen \[Login.windows.net\] Token Verbindung zum Azure-Tresor abrufen
    - IaaS VM muss Azure Key Vault-Endpunkt der Verschlüsselungsschlüssel Kunden wichtige Tresor schreiben herstellen können
    - IaaS VM muss Azure-Speicher Endpunkt herstellen, hostet Azure Erweiterung Repository und Azure Storage-Konto die VHD-Dateien enthält

**Hinweis:** Wenn Ihre Sicherheitsrichtlinie Zugriff von Azure VMs auf Internet beschränkt ist, können Sie oben URI beheben, Konnektivität benötigen, und konfigurieren Sie eine bestimmte Regel ausgehende Verbindung zu IP-Adressen zulassen.

- Verwenden Sie die neueste Version von Azure PowerShell SDK Version Azure Datenträgerverschlüsselung konfigurieren. Herunterladen der neuesten Version von [Azure PowerShell freigeben](https://github.com/Azure/azure-powershell/releases)

**Hinweis:** Azure Datenträgerverschlüsselung wird auf [Azure PowerShell SDK Version 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016)nicht unterstützt. Wenn Sie einen Fehler mit Azure PowerShell 1.1.0 erhalten, finden Sie unter [Azure Datenträger Verschlüsselung Fehler mit Azure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx)Artikel.

- Azure-CLI-Befehle ausführen und Azure-Abonnement zugeordnet, installieren Sie zuerst Azure CLI-Version:
    - Azure-CLI installieren und Ihr Azure-Abonnement zuordnen, finden Sie unter [Installieren und Konfigurieren von Azure-CLI](../xplat-cli-install.md)
    - Mithilfe der Azure-CLI für Mac, Linux und Windows Azure Ressourcenmanager finden Sie [hier](azure-cli-arm-commands.md)
- Azure Datenträger Verschlüsselungssystem verwenden externe BitLocker-Schlüsselschutzvorrichtung für Windows IaaS VMs. Wenn Ihre VMs Domäne beigetreten sind, drücken Sie Gruppenrichtlinien, die TPM-Schutzvorrichtungen erzwingen. "BitLocker ohne kompatibles TPM zulassen" finden Sie in [diesem Artikel](https://technet.microsoft.com/library/ee706521) auf die Gruppenrichtlinie.
- Azure Datenträger Verschlüsselung erforderliche PowerShell-Skript zu Azure AD-Anwendung Erstellen neuer Schlüssel Depot oder vorhandenen Schlüssel Depot einrichten und Aktivieren der Verschlüsselung ist [hier](https://github.com/Azure/azure-powershell/blob/dev/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).

#### <a name="setup-the-azure-ad-application-in-azure-active-directory"></a>Azure AD-Anwendung in Azure Active Directory einrichten

Wenn Verschlüsselung einen ausgeführten virtuellen Computer in Azure aktiviert sein muss, Azure datenträgerverschlüsselung generiert und schreibt die Verschlüsselungsschlüssel in Ihrem Tresor Schlüssel. Verwalten von Verschlüsselungsschlüsseln in Schlüssel Depot benötigt Azure AD-Authentifizierung.

Zu diesem Zweck sollte eine Azure AD-Anwendung erstellt werden. Eine ausführliche Anleitung zum Registrieren einer Anwendung, in der "Abrufen einer Identität für die Anwendung" Abschnitt in diesem [Blogbeitrag](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx)finden Sie hier.  Dieser Beitrag enthält auch nützliche Beispiele zur Bereitstellung und Konfiguration von Vault Schlüssel. Zur Authentifizierung geheimen basierte Authentifizierung oder Azure AD zertifikatbasierte Clientauthentifizierung verwendet werden kann.

##### <a name="client-secret-based-authentication-for-azure-ad"></a>Geheimen basierte Authentifizierung für Azure AD

Den folgenden Abschnitten sind die erforderlichen Schritte zum Konfigurieren einer geheimen Basis Clientauthentifizierung für Azure AD.

##### <a name="create-a-new-azure-ad-app-using-azure-powershell"></a>Erstellen Sie ein neues Azure AD app mit Azure PowerShell

Verwenden Sie das folgende PowerShell-Cmdlet zum Erstellen eines neuen Azure AD app:

    $aadClientSecret = “yourSecret”
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

**Hinweis:** $azureAdApplication.ApplicationId Azure AD ClientID und $aadClientSecret Client-Schlüssel, die Sie später sollten ADE aktivieren. Schützen Sie den Azure AD Clientschlüssel entsprechend.


##### <a name="provisioning-the-azure-ad-client-id-and-secret-from-the-azure-classic-deployment-model-portal"></a>Bereitstellung von Azure AD Client-ID und Schlüssel von Azure klassischen Bereitstellungsmodell Portal

Azure AD Client-ID und Schlüssel können auch https://manage.windowsazure.com mit der Azure-Bereitstellung Klassisch Portal bereitgestellt werden, gehen Sie zum Ausführen dieser Aufgabe:

1. Klicken Sie auf die Registerkarte Active Directory, wie in Abbildung dargestellt:

![Verschlüsselung Azure Festplatten](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. Klicken Sie auf Anwendung hinzufügen und geben Sie die Anwendung wie folgt:

![Verschlüsselung Azure Festplatten](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3. Klicken Sie auf den Pfeil, und konfigurieren Sie die Anwendung Eigenschaften wie folgt:

![Verschlüsselung Azure Festplatten](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. Klicken Sie auf das Häkchen in der unteren linken Ecke auf Fertig stellen. Die app-Konfigurationsseite angezeigt wird. Beachten Sie, dass Azure AD Client-ID befindet sich unten auf der Seite wie in der Abbildung dargestellt.

![Verschlüsselung Azure Festplatten](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5. speichern Sie klicken Sie auf die Schaltfläche Azure AD Clientschlüssel. Klicken Sie in der Schaltfläche und beachten Sie das Geheimnis aus dem Textfeld, Azure AD Client-Schlüssel. Schützen Sie den Azure AD Clientschlüssel entsprechend.

![Verschlüsselung Azure Festplatten](./media/azure-security-disk-encryption/disk-encryption-fig7.png)


**Hinweis:** diesem Datenstrom oben in das Portal nicht unterstützt.

##### <a name="use-an-existing-app"></a>Verwenden einer vorhandenen Anwendung

Führen Sie die folgenden Befehle benötigen Sie Modul Azure AD PowerShell [hier](https://technet.microsoft.com/library/jj151815.aspx)gewonnen werden kann.

**Hinweis:** müssen die folgenden Befehle ein PowerShell-Fenster ausgeführt werden. Verwenden Sie diese Befehle ausführen nicht Azure PowerShell oder Azure-Ressourcen-Manager-Fenster. Der Grund für diese Empfehlung ist, da diese Cmdlets MSOnline Modul oder Azure AD PowerShell sind.

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your AAD app>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a>Basis-Zertifikatauthentifizierung Azure AD

> [AZURE.NOTE] AAD Zertifikatauthentifizierung wird auf Linux VMs derzeit nicht unterstützt.

Den folgenden Abschnitten sind die erforderlichen Schritte zum Konfigurieren einer Authentifizierung Zertifikat für Azure AD.

##### <a name="create-a-new-azure-ad-app"></a>Erstellen Sie ein neues Azure AD app

Um eine neue PowerShell-Cmdlets ausführen Azure AD app:

**Hinweis:** Ersetzen Sie `yourpassword` Zeichenfolge unten mit Ihrem Passwort und das Kennwort zu schützen.

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

Nachdem Sie diesen Schritt abgeschlossen haben, Hochladen Sie eine PFX-Datei auf Schlüssel Depot und aktivieren Sie die Zugriffsrichtlinie bereitzustellende Zertifikats einer VM benötigt.

##### <a name="use-an-existing-azure-ad-app"></a>Eine vorhandene Azure AD App
Verwenden Sie Konfigurieren der Zertifikat-Authentifizierung für eine vorhandene Anwendung folgenden PowerShell-Cmdlets. Achten Sie darauf, dass diese neue PowerShell-Fenster ausführen.

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your AAD app>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

Nachdem Sie diesen Schritt abgeschlossen haben, Hochladen Sie eine PFX-Datei auf Schlüssel Depot und aktivieren Sie die Zugriffsrichtlinie bereitzustellende Zertifikats einer VM benötigt.

##### <a name="upload-a-pfx-file-to-key-vault"></a>Eine PFX-Datei auf Schlüssel Depot hochladen
Lesen Sie diesen [Blogbeitrag](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx) detaillierte Erläuterung zur Funktionsweise dieses Prozesses. Die folgenden PowerShell-Cmdlets sind jedoch für diese Aufgabe benötigen. Stellen Sie sicher, sie von Azure PowerShell-Konsole auszuführen:

**Hinweis:** Ersetzen Sie `yourpassword` Zeichenfolge unten mit Ihrem Passwort und das Kennwort zu schützen.

    $certLocalPath = 'C:\certs\myaadapp.pfx'
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’

    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@

    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### <a name="deploy-a-certificate-in-key-vault-to-an-existing-vm"></a>Bereitstellen eines Zertifikats im Schlüsseltresor vorhandenen VM
Nach dem Hochladen der PFX beenden, gehen Sie unten ein Zertifikat im Schlüssel Depot auf eine vorhandene VM bereitstellen:

    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName


#### <a name="setting-key-vault-access-policy-for-the-azure-ad-application"></a>Key Vault-Richtlinien festlegen für Azure AD-Anwendung

Die Azure AD-Anwendung benötigt Zugriffsrechte für Schlüssel oder Kennwörter im Tresor. Verwenden Sie das Cmdlet " [Set-AzureKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/dn903607.aspx) " zu der Anwendung, die Client-Id (die bei die Anwendung registriert wurde generiert wurde) als Parameterwert – ServicePrincipalName gewähren. Lesen Sie [in diesem Blogbeitrag](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx) Beispiele dazu. Hier haben Sie ein Beispiel für diesen Vorgang über PowerShell:

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

**Hinweis**: Azure datenträgerverschlüsselung erfordert die folgenden Richtlinien zur Clientanwendung AAD - Konfiguration 'WrapKey' und 'Set' Berechtigungen

## <a name="terminology"></a>Terminologie

Verwenden Sie Terminologie-Tabelle als Referenz zu dieser Technologie verwendeten Begriffe verstehen:

| Terminologie           | Definition                                                                                                                                                                                                                                   |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Azure AD                   | Azure AD ist [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Azure Konto ist Voraussetzung für Authentifizierung, speichern und Abrufen von Schlüssel aus dem Schlüsseltresor.                                                                                                        |
| Azure Key Vault [AKV] | Azure Key Vault ist kryptografischen Schlüsselverwaltungsdienst basierend auf FIPS-überprüften Hardwaresicherheitsmodule kryptografische Schlüssel und vertrauliche Kennwörter sicher schützen., [Key Vault](https://azure.microsoft.com/services/key-vault/) Dokumentation weitere Informationen.          |
| ARM                   | Azure Ressourcenmanager                                                                                                                                                                                                                       |
| BitLocker             | [BitLocker](https://technet.microsoft.com/library/hh831713.aspx) ist ein anerkannter Windows Volume Verschlüsselungstechnik IaaS Systemressourcen datenträgerverschlüsselung aktivieren                                                                                                                  |
| BEK                   | BitLocker-Verschlüsselungsschlüssel zum Startvolume OS und Datenträger verschlüsseln. Die BitLocker-Schlüssel sind Schutzmaßnahmen in Azure Key Vault Kunden als Schlüssel.                                                                              |
| CLI                   | [Azure Befehlszeilenschnittstelle](../xplat-cli-install.md)                                                                                                                                                                                                                 |
| DM-Crypt              | [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) ist die Linux-basierten transparente Verschlüsselung Datenträgersubsystem zur Verschlüsselung von Festplatten unter Linux IaaS VMs aktivieren                                                                                                                           |
| KEK                   | Schlüssel für die Verschlüsselung ist der asymmetrische Schlüssel (RSA 2048) schützen oder den Schlüssel bei Bedarf umbrochen. Sie können eine HSM geschützt oder Software geschützt Taste bereitstellen. Weitere Informationen finden Sie in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Dokumentation |
| PS-cmdlets            | [Azure PowerShell-cmdlets](powershell-install-configure.md)                                                                                                                                                                                                                                           |

### <a name="setting-and-configuring-azure-key-vault-for-azure-disk-encryption-usage"></a>Einstellung und Konfiguration von Azure Key Vault für Azure die Datenträgerverwendung Verschlüsselung

Azure datenträgerverschlüsselung schützt Datenträger Schlüssel und geheime Informationen in Azure Key Vault. Gehen Sie auf jedem der folgenden Abschnitte Schlüssel Depot für auf Azure Verschlüsselung Datenträgerverwendung.

#### <a name="create-a-new-key-vault"></a>Erstellen Sie ein neues Depot Schlüssels
Erstellen Sie ein neues Depot Schlüssel mit einer der unten aufgeführten Optionen:

- Verwenden die "101-erstellen-Schlüsseltresor" Ressourcenmanager Vorlage [hier](https://github.com/Azure/azure-quickstart-templates/blob/master/101-create-key-vault/azuredeploy.json)
- Verwenden Sie Azure PowerShell- [Cmdlets Schlüssel Depot](https://msdn.microsoft.com/library/dn868052.aspx).
- Verwenden Sie Portal Manager Azure Ressource.

**Hinweis:** Haben Sie bereits eine Schlüssel Vault-Setup für Ihr Abonnement, fahren Sie mit Abschnitt.

![Azure-Tresor](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="provisioning-a-key-encryption-key-optional"></a>Bereitstellung eines Verschlüsselungsschlüssels Schlüssel (optional)

Möchten Sie eine zusätzliche Sicherheitsebene Key Encryption Key (KEK) verwenden, um die BitLocker-Verschlüsselungsschlüssel umschließen, sollten Sie Ihrem Tresor Schlüssel für die Verwendung bei der Bereitstellung einer KEK hinzufügen.  Verwenden Sie das Cmdlet [Hinzufügen AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868048.aspx) einen neuen Schlüssel Schlüssel im Schlüssel Depot erstellen. Sie können auch KEK aus Ihrem lokalen Schlüsselmanagement HSM. Weitere Informationen finden Sie im [Schlüssel Vault-Dokumentation](https://azure.microsoft.com/documentation/services/key-vault/).

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

Die KEK kann Ressourcenmanager Azure Portal auch mit Azure Key Vault UX hinzugefügt werden

![Azure-Tresor](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions-to-allow-the-azure-platform-access-to-the-keys-and-secrets"></a>Azure-Plattform Zugriff auf Schlüssel und geheime Schlüssel Depot Berechtigungen

Die Azure-Plattform benötigt Zugriff auf Schlüssel oder Kennwörter in Azure Key Vault um VM starten und entschlüsselt die Datenträger zur Verfügung stellen. Zur Azure-Plattform zu gewähren, damit das Depot Schlüssel zugreifen können, muss die *EnabledForDiskEncryption* -Eigenschaft auf dem Schlüssel festgelegt werden. Die Eigenschaft EnabledForDiskEncryption lassen sich auf die wichtigsten Schlüssel Tresor PS-Cmdlet verwenden:

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

Die *EnabledForDiskEncryption* -Eigenschaft lassen https://resources.azure.com besuchen. Die *EnabledForDiskEncryption* -Eigenschaft muss auf die Schlüssel festlegen wie oben erwähnt. Andernfalls schlägt die Bereitstellung fehl.

Sie können Richtlinien für Anwendung AAD UX Depot Schlüssel einrichten:

![Azure-Tresor](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure-Tresor](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

Stellen Sie sicher, dass Schlüssel Depot für Datenträgerverschlüsselung in "Erweiterte Richtlinien" aktiviert ist:

![Azure-Tresor](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a>Disk Encryption Bereitstellungsszenarios und Benutzer

Es gibt viele Szenarios, datenträgerverschlüsselung aktivieren und die Schritte nach Szenario je können. Abschnitten werden diese Szenarios ausführlicher behandelt.

### <a name="enable-encryption-on-new-iaas-vms-created-from-the-azure-gallery"></a>Aktivieren der Verschlüsselung für neue IaaS VM aus dem Azure-Katalog erstellt

Datenträgerverschlüsselung ermöglicht auf neuen Windows Neuerung Azure Gallery in Azure mithilfe der Ressourcen-Manager-Vorlage veröffentlichten [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image). Klicken Sie auf "Nach Azure bereitstellen" Azure Schnellstart-Vorlage input Verschlüsselungskonfiguration in Blade-Parameter, und klicken Sie auf OK. Wählen Sie Abonnements, Ressourcengruppe, Gruppe Ort, rechtlich und Vereinbarung und klicken Sie erstellen, um die Aktivierung der Verschlüsselung für eine neue IaaS VM.

**Hinweis:** Diese Vorlage erstellt eine neue verschlüsselte Windows VM mit Windows Server 2012 Galeriebild.

Datenträgerverschlüsselung kann auf eine neue RedHat Linux 7.2 Neuerung mit 200 GB RAID 0 mit [dieser](https://aka.ms/fde-rhel) Ressourcen-Manager-Vorlage aktiviert. Nach die Vorlage bereitgestellt wird, überprüfen Sie die VM Verschlüsselung mit den `Get-AzureRmVmDiskEncryptionStatus` Cmdlets wie im Abschnitt "[OS verschlüsselnde Laufwerk ausgeführten Linux VM](#encrypting-os-drive-on-a-running-linux-vm)" beschrieben. Der Computer gibt beim Status `VMRestartPending`, starten Sie den virtuellen Computer neu.

Sie können die Vorlagendetails Parameter Ressourcenmanager neue VM Azure Gallery Szenario Azure AD Client-ID in der folgenden Tabelle finden Sie unter:

| Parameter                        | Beschreibung|
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| adminUserName                 | Admin-Benutzernamen für den virtuellen Computer                                                                                                                           |
| adminPassword                 | Admin-Benutzerkennwort für den virtuellen Computer                                                                                                                       |
| newStorageAccountName         | Name des Speicherkontos Betriebssystem und Daten VHDs speichern                                                                                                             |
| vmSize                        | Größe des virtuellen Computers. Derzeit werden nur Standard A, D und G-Serie unterstützt                                                                                          |
| virtualNetworkName            | Name des VNet, VM NIC gehören soll.                                                                                                            |
| subnetName                    | Name der im Subnetz vNet, VM NIC gehören soll                                                                                               |
| AADClientID                   | Client-ID für die Azure AD app mit Schreibberechtigung Geheimnisse für Key Vault                                                                                       |
| AADClientSecret               | Geheimen Azure AD-Anwendung, die über Schreibberechtigungen Geheimnisse für Key Vault                                                                                   |
| keyVaultURL                   | URL des Schlüssels Depots auf die BitLocker Schlüssel in übertragen werden sollen. Erhalten Sie mit dem Cmdlet: (Get-AzureRmKeyVault - VaultName-ResourceGroupName). VaultURI |
| keyEncryptionKeyURL           | URL der Key Encryption generierten BitLocker-Schlüssel verschlüsselt. Dies ist optional.                                                               |
| keyVaultResourceGroup         | Ressourcengruppe Key vault                                                               |
| vmName                        | Name der VM die Verschlüsselung Vorgang durchgeführt wird


**Hinweis:** KeyEncryptionKeyURL ist ein optionaler Parameter. Sie bringen eigene KEK weiteren Schutz Daten Verschlüsselungsschlüssel (Kennwort geheim) im Schlüsseltresor.

### <a name="enable-encryption-on-new-iaas-vms-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Aktivieren der Verschlüsselung für neue IaaS VM von Kunden verschlüsselt VHD und Verschlüsselungsschlüssel erstellt

In diesem Szenario können Sie verschlüsseln mit Ressourcen-Manager-Vorlage, PowerShell-Cmdlets oder CLI-Befehle. Die folgenden Abschnitte erläutern ausführlicher Ressourcenmanager Vorlage und CLI-Befehle.

Führen Sie die Schritte aus einem der folgenden Abschnitten vor der Verschlüsselung Bilder, die in Azure verwendet werden können. Nachdem das Bild erstellt wurde, können die Schritte im nächsten Abschnitt zum Erstellen einer verschlüsselten Azure VM verwendet werden.

- [Vorbereiten einer virtuellen Festplatte vor der Verschlüsselung Windows](#preparing-a-pre-encrypted-windows-vhd)
- [Vor der Verschlüsselung Linux VHD vorbereiten](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-resource-manager-template"></a>Ressourcenmanager Vorlage

Datenträgerverschlüsselung ermöglicht auf Kunden verschlüsselt VHD mit der Ressourcen-Manager-Vorlage veröffentlichten [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm). Klicken Sie auf "Nach Azure bereitstellen" Azure Schnellstart-Vorlage input Verschlüsselungskonfiguration in Blade-Parameter, und klicken Sie auf OK. Wählen Sie Abonnements, Ressourcengruppe, Gruppe Ort, rechtlich und Vereinbarung und klicken Sie erstellen, um die Aktivierung der Verschlüsselung für neue IaaS VM.

Ressourcenmanager Vorlage Parameter Details für Kundenszenario verschlüsselt VHD werden in der folgenden Tabelle beschrieben:

| Parameter                        | Beschreibung|
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| newStorageAccountName | Name des Speicherkontos verschlüsselte OS VHD-Datei gespeichert. Dieses Speicherkonto sollte bereits in derselben Ressourcengruppe und demselben Speicherort wie die VM erstellt wurden                                                     |
| osVhdUri              | URI des OS Vhd von Konto                                                                                                                                                                                      |
| osType                | Produkt-Betriebssystem (Windows/Linux)                                                                                                                                                                                         |
| virtualNetworkName    | Name des VNet, VM NIC gehören soll. Dies sollte bereits in derselben Ressourcengruppe und demselben Speicherort wie die VM erstellt wurden                                                                     |
| subnetName            | Name der im Subnetz vNet, VM NIC gehören soll                                                                                                                                                     |
| vmSize                | Größe des virtuellen Computers. Derzeit werden nur Standard A, D und G-Serie unterstützt                                                                                                                                                |
| keyVaultResourceID    | ResourceID Key Vault Ressource in ARM identifizieren. Erhalten Sie mit dem PowerShell-Cmdlet: (Get-AzureRmKeyVault - VaultName &lt;YourKeyVaultName&gt; - ResourceGroupName &lt;YourResourceGroupName&gt;). ResourceId |
| keyVaultSecretUrl     | URL des Verschlüsselungsschlüssels Datenträger im Schlüssel Tresor bereitgestellt                                                                                                                                                                |
| keyVaultKekUrl        | URL der Schlüssel, der Verschlüsselungsschlüssel generierten Laufwerk verschlüsseln Verschlüsselung                                                                                                                                       |
| vmName               | Name der Neuerung   |


#### <a name="using-powershell-cmdlets"></a>Mithilfe von PowerShell-cmdlets

Datenträgerverschlüsselung kann aktiviert Kunden verschlüsselt mit PS-Cmdlets VHD veröffentlichten [hier](https://msdn.microsoft.com/library/azure/mt603746.aspx).  

#### <a name="using-cli-commands"></a>CLI-Befehlen

Gehen Sie dabei CLI-Befehlen Verschlüsselung aktivieren:

1. Legen Sie Richtlinien auf Schlüssel:
    - Kennzeichen Sie "EnabledForDiskEncryption":`azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
    - Festlegen von Berechtigungen zum Azure AD app Geheimnisse Schlüsseltresor schreiben:`azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]`
2. Geben Sie zum Aktivieren der Verschlüsselung auf eine vorhandene/Ausführung VM:  *Azure Vm-Datenträger-Verschlüsselung aktivieren – Ressourcengruppen <resourceGroupName> -Namen <vmName> – Aad-Client-Id <aadClientId> – Aad Clientschlüssel <aadClientSecret> -Datenträger-Verschlüsselung-Schlüssel-Vault-Url <keyVaultURL> -Datenträger-Verschlüsselung-Schlüssel-Depot-Id <keyVaultResourceId> *
3. Verschlüsselung des Status: *"Azure Vm anzeigen-Datenträger-Verschlüsselung-Status-Ressourcengruppe <resourceGroupName> -Namen <vmName> Json-"*
4. Aktivieren der Verschlüsselung auf einer neuen VM Kunden verschlüsselt VHD, verwenden Sie die folgenden Parameter mit dem Befehl "Azure Vm erstellen":
    - Datenträger-Verschlüsselung-Schlüssel-Depot-Id < Datenträger-Verschlüsselung-Schlüssel-Depot-Id >
    - Datenträger-Verschlüsselung-Schlüssel-Url < Datenträger-Verschlüsselung-Schlüssel-Url >
    - Key-Verschlüsselung-Schlüssel-Depot-Id < Key-Verschlüsselung-Schlüssel-Depot-Id >
    - Key-Verschlüsselung-Schlüssel-Url < Key-Verschlüsselung-Schlüssel-Url >


### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a>Aktivieren der Verschlüsselung für vorhandene oder Windows Neuerung in Azure

In diesem Szenario können Sie verschlüsseln mit Ressourcen-Manager-Vorlage, PowerShell-Cmdlets oder CLI-Befehle. In den folgenden Abschnitten werden weitere Einzelheiten Ressourcenmanager Vorlage mit CLI-Befehlen aktivieren erläutert.

#### <a name="using-resource-manager-template"></a>Ressourcenmanager Vorlage

Datenträgerverschlüsselung kann aktiviert werden, auf vorhandene/Running Windows Neuerung in Azure mithilfe der Ressourcen-Manager-Vorlage veröffentlichten [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm). Klicken Sie auf "Nach Azure bereitstellen" Azure Schnellstart-Vorlage input Verschlüsselungskonfiguration in Blade-Parameter, und klicken Sie auf OK. Wählen Sie Abonnements, Ressourcengruppe, Gruppe Ort, rechtlich und Vereinbarung und klicken Sie erstellen, um die Aktivierung der Verschlüsselung für vorhandene/Ausführung IaaS VM.

Ressourcenmanager Vorlage Parameter Details für vorhandene/Ausführung VM Szenario mit Azure AD Client-ID finden Sie in der folgenden Tabelle:

| Parameter                 | Beschreibung|
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AADClientID            | Client-ID für die Azure AD app mit Schreibberechtigung Geheimnisse für Key Vault                                                                                                                                              |
| AADClientSecret         | Geheimen Azure AD-Anwendung, die über Schreibberechtigungen Geheimnisse für Key Vault                                                                                                                                          |
| keyVaultName | Name des Depots Schlüssel auf die BitLocker Schlüssel in übertragen werden sollen. Erhalten Sie mit dem Cmdlet: (Get-AzureRmKeyVault - ResourceGroupName <yourResourceGroupName>). Vaultname   |
|  keyEncryptionKeyURL   | URL der Key Encryption generierten BitLocker-Schlüssel verschlüsselt. Optional bei Auswahl `nokek` in der Dropdownliste UseExistingKek. Bei Auswahl von `kek` in der Dropdownliste UseExistingKek müssen Sie den KeyEncryptionKeyURL-Wert eingeben                                                                                                                        |
| volumeType             | Typ des Datenträgers für die Verschlüsselung Operation ausgeführt wird. Gültige Werte sind "OS", "Daten", "Alle"                                                                                                                     |
| sequenceVersion         | Sequenz-Version des BitLocker-Vorgangs. Diese Versionsnummer erhöht, jedes Mal ein Festplattenvorgang Verschlüsselung auf dem gleichen virtuellen Computer ausgeführt wird                                                                             |
| vmName                 | Name der VM die Verschlüsselung Vorgang durchgeführt wird


**Hinweis:** KeyEncryptionKeyURL ist ein optionaler Parameter. Sie bringen eigene KEK weiteren Schutz Daten Verschlüsselungsschlüssel (BitLocker-Verschlüsselung Geheimnis) im Schlüsseltresor.

#### <a name="using-powershell-cmdlets"></a>Mithilfe von PowerShell-cmdlets

Finden Sie in **Azure untersuchen Disk Encryption mit Azure PowerShell** Blog Post [Teil 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) und [Teil 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) Weitere Informationen zum Aktivieren der Verschlüsselung Verschlüsselung Azure Datenträger mit PS-Cmdlets.

#### <a name="using-cli-commands"></a>CLI-Befehlen

Gehen Sie zum Aktivieren der Verschlüsselung auf vorhandene/Ausführung von Windows Neuerung in Azure CLI-Befehlen:

1. Legen Sie Richtlinien auf Schlüssel:
    - Kennzeichen "EnabledForDiskEncryption": "Azure schlüsseltresor Set-Policy-Vault-Name <keyVaultName> – aktiviert-für-Datenträger-Verschlüsselung True"
    - Azure AD app Geheimnisse Schlüsseltresor schreiben Berechtigungen festlegen: "Azure schlüsseltresor Set-Policy-Vault-Name <keyVaultName> Spn - <aadClientID> -Dauerwelle Schlüssel [\"alle\"]--Dauerwelle geheime Schlüssel [\"alle\"]"
2. Geben Sie zum Aktivieren der Verschlüsselung auf eine vorhandene/Ausführung VM:  *Azure Vm-Datenträger-Verschlüsselung aktivieren – Ressourcengruppen <resourceGroupName> -Namen <vmName> – Aad-Client-Id <aadClientId> – Aad Clientschlüssel <aadClientSecret> -Datenträger-Verschlüsselung-Schlüssel-Vault-Url <keyVaultURL> -Datenträger-Verschlüsselung-Schlüssel-Depot-Id <keyVaultResourceId> *
3. Verschlüsselung des Status: *"Azure Vm anzeigen-Datenträger-Verschlüsselung-Status-Ressourcengruppe <resourceGroupName> -Namen <vmName> Json-"*
4. Aktivieren der Verschlüsselung auf einer neuen VM Kunden verschlüsselt VHD, verwenden Sie die folgenden Parameter mit dem Befehl "Azure Vm erstellen":
    - Datenträger-Verschlüsselung-Schlüssel-Depot-Id < Datenträger-Verschlüsselung-Schlüssel-Depot-Id >
    - Datenträger-Verschlüsselung-Schlüssel-Url < Datenträger-Verschlüsselung-Schlüssel-Url >
    - Key-Verschlüsselung-Schlüssel-Depot-Id < Key-Verschlüsselung-Schlüssel-Depot-Id >
    - Key-Verschlüsselung-Schlüssel-Url < Key-Verschlüsselung-Schlüssel-Url >


### <a name="enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure"></a>Aktivieren der Verschlüsselung für vorhandene oder Linux Neuerung in Azure

Festplattenverschlüsselung kann auf vorhandenen ausführen/Linux Neuerung in Azure mithilfe der Ressourcen-Manager-Vorlage aktiviert veröffentlichten [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm). Klicken Sie auf "Nach Azure bereitstellen" Azure Schnellstart-Vorlage input Verschlüsselungskonfiguration in Blade-Parameter, und klicken Sie auf OK. Wählen Sie Abonnements, Ressourcengruppe, Gruppe Ort, rechtlich und Vereinbarung und klicken Sie erstellen, um die Aktivierung der Verschlüsselung für vorhandene/Ausführung IaaS VM.

Ressourcenmanager Vorlage Parameter Details für vorhandene/Ausführung VM Szenario Azure AD Client-ID werden in der folgenden Tabelle beschrieben:

| Parameter                 | Beschreibung|
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AADClientID            | Client-ID für die Azure AD app mit Schreibberechtigung Geheimnisse für Key Vault                                                                                                                                              |
| AADClientSecret         | Geheimen Azure AD-Anwendung, die über Schreibberechtigungen Geheimnisse für Key Vault                                                                                                                                          |
| keyVaultName | Name des Depots Schlüssel auf die BitLocker Schlüssel in übertragen werden sollen. Erhalten Sie mit dem Cmdlet: (Get-AzureRmKeyVault - ResourceGroupName <yourResourceGroupName>). Vaultname   |
|  keyEncryptionKeyURL   | URL der Key Encryption generierten BitLocker-Schlüssel verschlüsselt. Dies ist optional, wenn Sie "Nokek" in der Dropdownliste UseExistingKek auswählen. Bei Auswahl von "Kek" in der Dropdownliste UseExistingKek müssen Sie den KeyEncryptionKeyURL-Wert eingeben.                                                                                                                        |
| volumeType             | Typ des Datenträgers für die Verschlüsselung Operation ausgeführt wird. Gültige Werte werden unterstützt "OS" "Alle" (für RHEL 7.2 CentOS 7.2 und Ubuntu 16.04) und "" für alle Distributionen.                                                                                                                      |
| sequenceVersion         | Sequenz-Version des BitLocker-Vorgangs. Diese Versionsnummer erhöht, jedes Mal ein Festplattenvorgang Verschlüsselung auf dem gleichen virtuellen Computer ausgeführt wird                                                                             |
| vmName                 | Name der VM die Verschlüsselung Vorgang durchgeführt wird
| passPhrase              | Geben Sie eine sichere Passphrase Verschlüsselungsschlüssel Daten                                                                                                                                                                       |                                                                                                                                                                                                                                                      

**Hinweis:** KeyEncryptionKeyURL ist ein optionaler Parameter. Sie bringen eigene KEK weiteren Schutz Daten Verschlüsselungsschlüssel (Kennwort geheim) im Schlüsseltresor.

#### <a name="cli-commands"></a>CLI-Befehle

Datenträgerverschlüsselung ermöglicht auf Kunden verschlüsselte virtuelle Festplatte mit dem CLI-Befehl [hier](../xplat-cli-install.md)installiert. Gehen Sie zum Aktivieren der Verschlüsselung auf vorhandene/Ausführung von Linux Neuerung in Azure CLI-Befehlen:

1. Legen Sie Richtlinien auf Schlüssel:
    - Kennzeichen "EnabledForDiskEncryption": "Azure schlüsseltresor Set-Policy-Vault-Name <keyVaultName> – aktiviert-für-Datenträger-Verschlüsselung True"
    - Azure AD app Geheimnisse Schlüsseltresor schreiben Berechtigungen festlegen: "Azure schlüsseltresor Set-Policy-Vault-Name <keyVaultName> Spn - <aadClientID> -Dauerwelle Schlüssel [\"alle\"]--Dauerwelle geheime Schlüssel [\"alle\"]"
2. Geben Sie zum Aktivieren der Verschlüsselung auf eine vorhandene/Ausführung VM:  *Azure Vm-Datenträger-Verschlüsselung aktivieren – Ressourcengruppen <resourceGroupName> -Namen <vmName> – Aad-Client-Id <aadClientId> – Aad Clientschlüssel <aadClientSecret> -Datenträger-Verschlüsselung-Schlüssel-Vault-Url <keyVaultURL> -Datenträger-Verschlüsselung-Schlüssel-Depot-Id <keyVaultResourceId> *
3. Verschlüsselungsstatus zu erhalten: "Azure Vm anzeigen-Datenträger-Verschlüsselung-Status-Ressourcengruppe <resourceGroupName> -Namen <vmName> - Json"
4. Aktivieren der Verschlüsselung auf einer neuen VM Kunden verschlüsselt VHD, verwenden Sie die folgenden Parameter mit dem Befehl "Azure Vm erstellen".
    - *Datenträger-Verschlüsselung-Schlüssel-Depot-Id < Datenträger-Verschlüsselung-Schlüssel-Depot-Id >*
    - *Datenträger-Verschlüsselung-Schlüssel-Url < Datenträger-Verschlüsselung-Schlüssel-Url >*
    - *Key-Verschlüsselung-Schlüssel-Depot-Id < Key-Verschlüsselung-Schlüssel-Depot-Id >*
    - *Key-Verschlüsselung-Schlüssel-Url < Key-Verschlüsselung-Schlüssel-Url >*

### <a name="get-encryption-status-of-an-encrypted-iaas-vm"></a>Verschlüsselung des Status einer verschlüsselten IaaS-VM

Verschlüsselung mit Ressourcenmanager Azure-Portal, [PowerShell-Cmdlets](https://msdn.microsoft.com/library/azure/mt622700.aspx) oder CLI-Befehle erhalten. In den folgenden Abschnitten erläutern von Azure-Portal und CLI-Befehle verwenden, um die Verschlüsselung erhalten.

#### <a name="get-encryption-status-of-an-encrypted-windows-vm-using-azure-resource-manager-portal"></a>Verschlüsselung des Status einer verschlüsselten Windows VM mit Ressourcenmanager Azure-portal

Sie erhalten den Verschlüsselungsstatus der VM IaaS Ressourcenmanager Azure-Portal. Azure-Portal unter https://portal.azure.com/ anmelden Sie, klicken Sie auf virtuellen Maschinen Link im linken Menü auf den virtuellen Computern in Ihrem Abonnement Zusammenfassung anzeigen. Der Abonnementname aus Abonnement auswählen, um virtuelle Computer anzeigen zu filtern. Klicken Sie auf Spalten oben im Seitenmenü virtuelle Computer. Wählen Sie Spalte Blade wählen Sie Disk Encryption Spalte aus und auf aktualisieren. Die Datenträger Verschlüsselung Spalte zeigt den Verschlüsselungsstatus "Aktiviert" oder "Nicht aktiviert" für jeden virtuellen Computer wie in der folgenden Abbildung dargestellt sollte angezeigt werden.

![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-encryption-status-of-an-encrypted-windowslinux-iaas-vm-using-disk-encryption-ps-cmdlet"></a>Verschlüsselungsstatus einer verschlüsselten (Windows/Linux) IaaS VM Datenträger Verschlüsselung PS-Cmdlet zu erhalten
Verschlüsselungsstatus IaaS-VM erhalten von Disk Encryption PS Cmdlet "Get-AzureRmVMDiskEncryptionStatus". Um die Verschlüsselung für die VM zu erhalten, geben Sie in Azure PowerShell-Sitzung:

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName
    
    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

Die Ausgabe von Get-AzureRmVMDiskEncryptionStatus für die Verschlüsselung überprüft werden wichtige URLs.
    
    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMNam
    e $VMName -ExtensionName $ExtensionName
    C:\> $status.OsVolumeEncryptionSettings

    DiskEncryptionKey                                                 KeyEncryptionKey                                               Enabled
    -----------------                                                 ----------------                                               -------
    Microsoft.Azure.Management.Compute.Models.KeyVaultSecretReference Microsoft.Azure.Management.Compute.Models.KeyVaultKeyReference    True


    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey.SecretUrl
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a
    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey
    
    SecretUrl                                                                                                               SourceVault
    ---------                                                                                                               -----------
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a Microsoft.Azure.Management....

Wert Einstellung OSVolumeEncrypted und DataVolumesEncrypted sollen "Verschlüsselt", dass die Volumes mit Azure datenträgerverschlüsselung verschlüsselt werden. Finden Sie in **Azure untersuchen Disk Encryption mit Azure PowerShell** Blog Post [Teil 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) und [Teil 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) Weitere Informationen zum Aktivieren der Verschlüsselung Verschlüsselung Azure Datenträger mit PS-Cmdlets.

**Hinweis**: auf Linux VMs, die `Get-AzureRmVMDiskEncryptionStatus` Cmdlet Minuten 3-4 Verschlüsselung Statusbericht.

#### <a name="get-encryption-status-of-the-iaas-vm-from-disk-encryption-cli-command"></a>Verschlüsselung von Festplatten CLI-Befehl erhalten Sie Verschlüsselungsstatus IaaS-VM

Verschlüsselungsstatus IaaS-VM erhalten von Disk Encryption CLI-Befehl *Azure Vm anzeigen-Datenträger-Verschlüsselung-Status*. Um die Verschlüsselung für die VM zu erhalten, geben Sie in der Azure-CLI-Sitzung:

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a>Deaktivieren Sie die Verschlüsselung auf Windows Neuerung

Sie können deaktivieren Sie die Verschlüsselung auf einem ausgeführten Windows oder Linux Neuerung Azure Datenträger Verschlüsselung Ressourcenmanager Vorlage oder PS-Cmdlets und gibt die Entschlüsselung Konfiguration.


##### <a name="windows-vm"></a>Windows VM

Schritt Verschlüsselung deaktivieren deaktiviert die Verschlüsselung OS oder Datenträgers oder auf laufenden Neuerung von Windows. Sie können nicht das Betriebssystemvolume deaktivieren und das Datenvolume verschlüsselt. Bei Schritt Verschlüsselung deaktivieren Azure klassischen Bereitstellungsmodell aktualisiert Servicemodell VM und Windows Neuerung entschlüsselten markiert. Der Inhalt der VM nicht statisch mehr verschlüsselt. Kunden wichtige Depot und den Verschlüsselungsschlüsseln, die BitLocker-Verschlüsselung Schlüssel für Windows und Linux Passphrase Verschlüsselung deaktivieren nicht gelöscht. 

##### <a name="linux-vm"></a>Linux VM

Schritt Verschlüsselung deaktivieren deaktiviert die Verschlüsselung des Datenvolumens ausgeführten Linux Neuerung

**Hinweis**: Deaktivieren der Verschlüsselung auf Betriebssystemdatenträger für Linux VMs nicht zulässig.

##### <a name="disable-encryption-on-existingrunning-iaas-vm-in-azure-using-resource-manager-template"></a>Deaktivieren Sie die Verschlüsselung auf vorhandenen ausführen/IaaS VM in Azure mit Ressourcen-Manager-Vorlage

Datenträgerverschlüsselung kann auf Windows Neuerung mit der Ressourcen-Manager-Vorlage deaktiviert veröffentlichten [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm). Klicken Sie auf "Nach Azure bereitstellen" Azure Schnellstart-Vorlage input Entschlüsselung Konfiguration Blade-Parameter, und klicken Sie auf OK. Wählen Sie Abonnements, Ressourcengruppe, Gruppe Ort, rechtlich und Vereinbarung und klicken Sie erstellen, um die Aktivierung der Verschlüsselung für eine neue IaaS VM.

[Diese](https://aka.ms/decrypt-linuxvm) Vorlage kann für Linux VM Verschlüsselung deaktivieren verwendet werden.

Ressourcenmanager Vorlage Parameter Einzelheiten Deaktivieren der Verschlüsselung auf IaaS VM:

| vmName         | Name der VM die Verschlüsselung Vorgang durchgeführt wird                                                                                                                                                                       |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| volumeType     | Typ des Datenträgers für die Entschlüsselung Operation ausgeführt wird. Gültige Werte sind "OS", "Daten", "Alle". **Hinweis:** Verschlüsselung unter Windows Neuerung Betriebssystemstart Volume ohne Deaktivieren der Verschlüsselung auf "Daten" kann nicht deaktiviert werden. **Hinweis**: Deaktivieren der Verschlüsselung auf Betriebssystemdatenträger für Linux VMs nicht zulässig. |
| sequenceVersion | Sequenz-Version des BitLocker-Vorgangs. Diese Versionsnummer erhöht, jedem Datenträger entschlüsselt werden auf dem gleichen virtuellen Computer ausgeführt wird                                                                                          |

##### <a name="disable-encryption-on-existingrunning-iaas-vm-in-azure-using-ps-cmdlet"></a>Deaktivieren Sie die Verschlüsselung auf vorhandenen ausführen/IaaS VM in Azure mit PS-cmdlet

Deaktivieren mit PS-Cmdlet deaktiviert [Deaktivieren-AzureRmVMDiskEncryption](https://msdn.microsoft.com/library/azure/mt715776.aspx) -Cmdlet Verschlüsselung eine Infrastruktur als Service (IaaS) virtueller Computer. Dieses Cmdlet unterstützt sowohl Windows als auch Linux VMs. Dieses Cmdlet installiert eine Erweiterung auf dem virtuellen Computer zu deaktivieren. Wenn der Name-Parameter nicht angegeben ist, wird eine Erweiterung mit dem Namen "AzureDiskEncryption für Windows VMs" erstellt. 

Unter Linux VMs ist die Erweiterung "AzureDiskEncryptionForLinux" verwendet.

**Hinweis**: dieses Cmdlet den virtuellen Computer neu gestartet. 

## <a name="appendix"></a>Anhang

### <a name="connect-to-your-subscription"></a>Verbinden mit Ihrem Abonnement

Überprüfen Sie im Abschnitt *erforderliche Komponenten* in diesem Dokument. Stellen Sie sicher, dass alle erforderlichen Komponenten erfüllt, gehen Sie die Verbindung zu Ihrem Abonnement:

1. Starten Sie Azure PowerShell-Sitzung und melden Sie sich bei Ihrem Azure-Konto mit dem folgenden Befehl:

    Login-AzureRmAccount

2. Wenn Sie mehrere Abonnements und eine bestimmte verwenden möchten, geben Sie Folgendes um Abonnements für Ihr Konto anzuzeigen:

    Get-AzureRmSubscription

3. Geben Sie 3. das Abonnement zu verwenden:

    Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>

4. um überprüfen Abonnement so konfiguriert sind, geben Sie Folgendes ein:

    Get-AzureRmSubscription

5. bestätigen Cmdlets Verschlüsselung Azure Festplatten installiert sind, geben Sie Folgendes ein:

    Get-command *diskencryption*

6. müsste der folgenden Ausgabe Azure Datenträger Verschlüsselung PowerShell Installation bestätigen:

    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     

### <a name="preparing-a-pre-encrypted-windows-vhd"></a>Vorbereiten einer virtuellen Festplatte vor der Verschlüsselung Windows
Die folgenden Abschnitte sind vor der Verschlüsselung Windows VHD Vorbereiten der Bereitstellung als verschlüsselte VHD in Azure IaaS. Die Schritte zum Vorbereiten und Starten von frischen Windows Hyper-V oder Azure VM (Vhd).

#### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>Aktualisieren der Gruppenrichtlinie ohne TPM OS Schutz ermöglichen
Sie müssen die BitLocker-Gruppenrichtlinie Einstellung BitLocker Drive Encryption, finden Sie unter Richtlinien für Lokaler Computer \Computer Configuration\Administrative Vorlagen\Windows-Komponenten konfigurieren. Diese Einstellung: *Betriebssystem-Laufwerken - erfordert zusätzliche Authentifizierung beim Start - BitLocker ohne kompatibles TPM zulassen* , wie in der folgenden Abbildung dargestellt:

![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a>BitLocker Komponenten installieren
Für Windows Server 2012 und höher verwenden den folgenden Befehl:

    dism /online /Enable-Feature /all /FeatureName:Bitlocker /quiet /norestart

Für Windows Server 2008 R2 mit dem folgenden Befehl:

    ServerManagerCmd -install BitLockers

#### <a name="prepare-os-volume-for-bitlocker-using-bdehdcfg"></a>Verwenden von BitLocker bereiten Sie Betriebssystemvolume vor`bdehdcfg`

Führen Sie den Befehl unten, um die Betriebssystempartition komprimieren und den Computer für BitLocker vorzubereiten.

    bdehdcfg -target c: shrink -quiet

#### <a name="using-bitlocker-to-protect-the-os-volume"></a>Verwenden von BitLocker auf das Betriebssystemvolume schützen
Verwenden der [`manage-bde`](https://technet.microsoft.com/library/ff829849.aspx) Befehl zum Aktivieren der Verschlüsselung auf dem Startvolume mithilfe einer externen Schlüsselschutzvorrichtung und externen Schlüssel (.bek Datei) auf die Festplatte oder das Volume. Verschlüsselung wird auf dem System/Boot Volume nach dem nächsten Neustart aktiviert.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

**Hinweis:** Die VM muss mit einer separaten Datenressource Vhd für die externe Schlüssel mit BitLocker vorbereitet werden.

#### <a name="encrypting-os-drive-on-a-running-linux-vm"></a>OS ausgeführten Linux VM Laufwerk verschlüsseln

Verschlüsselung von OS Laufwerk ausgeführten Linux VM wird für die folgenden Distributionen unterstützt:

- RHEL 7.2
- CentOS 7.2
- Ubuntu 16.04

Erforderliche Komponenten für OS Datenträger Verschlüsselung:

- VM muss von Azure Galeriebild in Azure Ressourcenmanager Portal erstellt werden.
- Azure VM mit mindestens 4 GB RAM (empfohlen: Größe 7 GB).
- (Für RHEL und CentOS) SELinux muss auf dem virtuellen Computer [deaktiviert](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) . VM muss mindestens einmal neu gestartet werden, nachdem SELinux deaktiviert.


##### <a name="steps"></a>Schritte

1. erstellen Sie 1. einen virtuellen Computer mit einer der oben angegebenen Distributionen.

Für CentOS 7.2 wird OS festplattenverschlüsselung über spezielle Bild unterstützt. Um dieses Bild zu verwenden, geben Sie "7.2n" als Sku Erstellung die VM:

    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"

2. konfigurieren Sie 2. die VM nach Ihren Bedürfnissen. Wenn Sie beabsichtigen, alle verschlüsseln (OS + Daten) die Daten auf den Laufwerken, die Laufwerke angegebenen und Montage von/etc/fstab müssen.

> [AZURE.NOTE] Verwenden Sie UUID =... an Daten in/Etc/Fstab anstatt den Block Gerätenamen, z. B./dev/sdb1 Laufwerke. Bei der Verschlüsselung wird die Reihenfolge der Laufwerke auf dem virtuellen Computer ändern. Wenn die VM auf Blockgeräte Reihenfolge nach der Verschlüsselung Montage fehlschlagen wird.

3. Abmeldung SSH-Sitzung.

4. um das Betriebssystem zu verschlüsseln, geben Sie VolumeType als "alle" oder "OS" beim [Aktivieren der Verschlüsselung](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).

> [AZURE.NOTE] Nicht als Benutzerbereich Prozesse `systemd` Dienste werden getötet mit einer `SIGKILL`. Die VM muss neu gestartet werden. Bitte planen Sie Ausfallzeiten von VM beim Aktivieren OS-Verschlüsselung für einen ausgeführten virtuellen Computer Festplatte.

5. regelmäßig überwachen Sie die Verschlüsselung mithilfe der Anleitung im [nächsten Abschnitt](#monitoring-os-encryption-progress).

6. wenn Get-AzureRmVmDiskEncryptionStatus "VMRestartPending" zeigt, starten entweder Protokollierung an oder über PowerShell/Portal/CLI VM neu.

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName
    
    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, please reboot the VM

Es wird empfohlen, [Boot-Diagnose](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) der VM *vor dem* Neustart speichern.

#### <a name="monitoring-os-encryption-progress"></a>Überwachung von OS-Verschlüsselung

Es gibt drei Möglichkeiten, OS Verschlüsselung überwachen.

1. verwenden Sie 1. das Cmdlet "Get-AzureRmVmDiskEncryptionStatus", und überprüfen Sie das ProgressMessage-Feld: 
 
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started

Erreicht die VM "OS Datenträger Verschlüsselung gestartet" dauert etwa 40 bis 50 Minuten auf Premium-Speicher gesichert VM.

[Problem Nr. 388](https://github.com/Azure/WALinuxAgent/issues/388) in WALinuxAgent `OsVolumeEncrypted` und `DataVolumesEncrypted` werden als `Unknown` in einigen Distributionen. WALinuxAgent Version 2.1.5 und dies wird automatisch behoben. Sehen Sie `Unknown` in der Ausgabe können Sie Datenträger-Verschlüsselung mithilfe von Azure Ressource Viewer überprüfen.

Gehen Sie zur [Azure Ressource Viewer](https://resources.azure.com/)und erweitern Sie diese Hierarchie im Auswahlfenster Links:

~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

In der InstanceView einen Bildlauf zu den Verschlüsselungsstatus der Laufwerke.

![VM-Instanz anzeigen](./media/azure-security-disk-encryption/vm-instanceview.png)

2. Überprüfen Sie 2. [Boot-Diagnose](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/). Nachrichten von ADE-Erweiterung müssen mit dem Präfix `[AzureDiskEncryption]`.

3. Anmeldung an der VM über SSH und Erweiterung Protokoll abrufen

    /var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

Es wird nicht empfohlen, die VM anmelden, während BS-Verschlüsselung ausgeführt wird. Daher sollten die Protokolle kopiert werden nur bei beiden anderen Methoden fehlgeschlagen sind.

#### <a name="preparing-a-pre-encrypted-linux-vhd"></a>Vor der Verschlüsselung Linux VHD vorbereiten

##### <a name="ubuntu-16"></a>Ubuntu 16

###### <a name="configure-encryption-during-distro-install"></a>Konfigurieren Sie die Verschlüsselung während der Distribution installieren

1. Wählen Sie "Configure verschlüsselten Volumes" beim Partitionieren von Festplatten.

![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Erstellen einer separaten Boot-Laufwerks muss nicht verschlüsselt werden. Verschlüsseln Sie Ihr Stammlaufwerk.

![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Geben Sie 3. ein Passwort. Dies ist das Kennwort, das Sie in Schlüsseltresor hochladen.

![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Beenden Sie 4. partitionieren.

![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. wenn die VM starten, werden Sie aufgefordert, ein Kennwort anzugeben. Verwenden Sie die in Schritt 3 eingegebene Passphrase.

![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. Vorbereitung der Upload in Azure mit [dieser](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/)VM. Den letzten Schritt (Aufheben der VM) nicht ausgeführt noch.

###### <a name="configure-encryption-to-work-with-azure"></a>Konfigurieren Sie die Verschlüsselung mit Azure arbeiten

1. erstellen Sie 1. eine Datei unter /usr/local/sbin/azure_crypt_key.sh, mit dem Inhalt im folgenden Skript. Beachten der KeyFileName ist der Passphrase Dateiname einfügen von Azure.

    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do
        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED to find suitable passphrase file ..." >&2
        echo -n "Try to enter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi


2. ändern Sie Crypt Config in */etc/crypttab*. Es sollte wie folgt aussehen:

    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh

3. Wenn Sie *azure_crypt_key.sh* in Windows bearbeiten und Linux kopiert, unbedingt *dos2unix /usr/local/sbin/azure_crypt_key.sh*ausführen.

4. das Skript ausführbare Berechtigungen hinzufügen:

    chmod +x /usr/local/sbin/azure_crypt_key.sh

4. bearbeiten Sie */etc/initramfs-tools/modules* Zeilen anhängen:

    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1

5. Führen Sie `update-initramfs -u -k all` Initramfs zu aktualisieren die `keyscript` wirksam.
6. jetzt können Sie die VM entziehen.

![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

7. weiterhin in Azure weiter und [die virtuelle Festplatte hochladen](#upload-encrypted-vhd-to-an-azure-storage-account) .

##### <a name="opensuse-132"></a>OpenSUSE 13.2

###### <a name="configure-encryption-during-distro-install"></a>Konfigurieren Sie die Verschlüsselung während der Distribution installieren

1.Wählen Sie "Verschlüsseln Volume Group" beim Partitionieren von Festplatten. Geben Sie ein Passwort. Dies ist das Kennwort, das Sie in Schlüsseltresor hochladen.

![OpenSUSE 13.2 einrichten](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Starten Sie 2. Ihre Passphrase mit VM.

![OpenSUSE 13.2 einrichten](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. Vorbereitung der Upload in Azure mit [dieser](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131)VM. Den letzten Schritt (Aufheben der VM) nicht ausgeführt noch.

###### <a name="configure-encryption-to-work-with-azure"></a>Konfigurieren Sie die Verschlüsselung mit Azure arbeiten

1. bearbeiten Sie 1. das /etc/dracut.conf, und fügen Sie die folgende Zeile hinzu:

    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"

2. kommentieren Sie diese Zeilen am Ende der Datei "/ usr/lib/dracut/modules.d/90crypt/module-setup.sh":

    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator



3. Fügen Sie die folgende Zeile am Anfang der Datei "/ usr/lib/dracut/modules.d/90crypt/parse-crypt.sh"

    DRACUT_SYSTEMD=0

und Ersetzen Sie alle Vorkommen von

    if [ -z "$DRACUT_SYSTEMD" ]; then

An

    if [ 1 ]; then

4. bearbeiten Sie 4. /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh und fügen Sie diese nach "# Öffnen LUKS Gerät"

    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done


5. Führen Sie die "/ Usr/Sbin/Dracut - f - V" Initrd aktualisieren.

6. jetzt können Sie die VM und [die virtuelle Festplatte hochladen](#upload-encrypted-vhd-to-an-azure-storage-account) in Azure entziehen.

##### <a name="centos-7"></a>CentOS 7

###### <a name="configure-encryption-during-distro-install"></a>Konfigurieren Sie die Verschlüsselung während der Distribution installieren

1. Wählen Sie "Daten verschlüsseln" beim Partitionieren von Festplatten.

![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. stellen sicher, dass "Verschlüsseln" für Root-Partition ausgewählt ist.

![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. Geben Sie 3. ein Passwort. Dies ist das Kennwort, das Sie in Schlüsseltresor hochladen.

![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. wenn die VM starten, werden Sie aufgefordert, ein Kennwort anzugeben. Verwenden Sie die in Schritt 3 eingegebene Passphrase.

![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. Vorbereitung der Upload in Azure mit [dieser](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70)VM. Den letzten Schritt (Aufheben der VM) nicht ausgeführt noch.

6. jetzt können Sie die VM und [die virtuelle Festplatte hochladen](#upload-encrypted-vhd-to-an-azure-storage-account) in Azure entziehen.

###### <a name="configure-encryption-to-work-with-azure"></a>Konfigurieren Sie die Verschlüsselung mit Azure arbeiten

1. bearbeiten Sie 1. das /etc/dracut.conf, und fügen Sie die folgende Zeile hinzu:

    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"

2. kommentieren Sie diese Zeilen am Ende der Datei "/ usr/lib/dracut/modules.d/90crypt/module-setup.sh":

    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator



3. Fügen Sie die folgende Zeile am Anfang der Datei "/ usr/lib/dracut/modules.d/90crypt/parse-crypt.sh"

    DRACUT_SYSTEMD=0

und Ersetzen Sie alle Vorkommen von

    if [ -z "$DRACUT_SYSTEMD" ]; then

An

    if [ 1 ]; then

4. bearbeiten Sie 4. /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh und fügen Sie diese nach "# Öffnen LUKS Gerät"

    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done


5. Führen Sie die "/ Usr/Sbin/Dracut - f - V" Initrd aktualisieren.

![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a>Verschlüsselte VHD Azure Storage-Konto hochladen
Nach der Aktivierung von BitLocker-Verschlüsselung Pr DM-Crypt Verschlüsselung muss lokale verschlüsselte VHD das Speicherkonto hochgeladen werden.

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-disk-encryption-secret-for-the-pre-encrypted-vm-to-key-vault"></a>Hochladen Sie Disk Encryption Schlüssel für vorab verschlüsselte VM Schlüssel Tresor
Der Datenträger Verschlüsselung Schlüssel abgerufen zuvor als Geheimnis im Schlüsseltresor geladen werden muss. Key Vault muss Berechtigungen für Ihren Client AAD aktiviert sowie die datenträgerverschlüsselung.


    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $KeyVault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>Disk Encryption Schlüssel nicht mit einem KEK verschlüsselt
Verwenden Sie [Set AzureKeyVaultSecret](https://msdn.microsoft.com/library/dn868050.aspx) Bereitstellung der geheime Schlüssel Depot. Bei einem Windows-Computer Bek Datei als String base64-codiert und dann Schlüssel Tresor mit dem Set-AzureKeyVaultSecret-Cmdlet hochgeladen. Unter Linux die Passphrase als String base64-codiert und dann Schlüssel Tresor hochgeladen. Darüber hinaus stellen Sie sicher, dass die folgenden Tags beim Erstellen des Schlüssels im Schlüssel Tresor festgelegt sind.

    # This is the passphrase that was provided for encryption during distro install
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

Die `$secretUrl` wird im nächsten Schritt für [den Betriebssystem-Datenträger ohne KEK](#without-using-a-kek)verwendet werden.

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>Disk Encryption Schlüssel verschlüsselt mit einem KEK

Der geheime Schlüssel kann optional mit einem Schlüssel vor dem Hochladen auf Vault Schlüssel verschlüsselt werden. Verwenden Sie die Wrap [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) zuerst den Schlüssel der Verschlüsselungsschlüssel verschlüsseln. Die Ausgabe dieser Operation Wrap ist String URL codiert base64 die dann mithilfe des [Set-AzureKeyVaultSecret](https://msdn.microsoft.com/library/dn868050.aspx) -Cmdlet geheim hochgeladen.

    # This is the passphrase that was provided for encryption during distro install
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id

Die `$KeyEncryptionKey` und `$secretUrl` wird im nächsten Schritt für [Betriebssystem-Datenträger mit KEK](#using-a-kek)verwendet werden.

### <a name="specify-secret-url-when-attaching-os-disk"></a>Geben Sie geheime URL beim Betriebssystem-Datenträger

#### <a name="without-using-a-kek"></a>Ohne ein KEK

Beim Anfügen der Betriebssystemdatenträger `$secretUrl` übergeben werden muss. Der URL wurde im Abschnitt ["Disk Encryption Schlüssel verschlüsselt nicht mit einem KEK"](#disk-encryption-secret-not-encrypted-with-a-kek)generiert.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a>Verwenden einer KEK

Beim Anfügen der Betriebssystemdatenträger `$KeyEncryptionKey` und `$secretUrl` übergeben werden müssen. Der URL wurde im Abschnitt ["Disk Encryption Schlüssel verschlüsselt mit einem KEK"](#disk-encryption-secret-encrypted-with-a-kek)generiert.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id

## <a name="download-this-guide"></a>Dieses Handbuch herunterladen
Sie können dieses Handbuch aus der [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).


## <a name="for-more-information"></a>Weitere Informationen
[Verschlüsselung mit Azure PowerShell Azure Festplatten durchsuchen](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)

[Untersuchen von Azure Disk Encryption Azure PowerShell - Teil 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
