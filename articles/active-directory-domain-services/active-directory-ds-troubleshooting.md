<properties
    pageTitle="Azure Active Directory Domain Services: Fehlerbehebung | Microsoft Azure"
    description="Problembehandlung bei Azure Active Directory-Domänendienste"
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

# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure Active Directory-Domänendienste - Fehlerbehebung
Dieser Artikel enthält Hinweise zur Problembehandlung bei Problemen, die auftreten können, wenn Domänendienste Azure Active Directory (AD) verwalten.


## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Azure Active Directory-Domänendienste für Azure AD-Verzeichnis kann nicht aktiviert werden.
In diesem Abschnitt können Sie die Fehler beheben, wenn Sie Azure AD-Domäne für das Verzeichnis aktivieren und schlägt fehl, oder auf 'Deaktiviert' umgeschaltet wird.

Wählen Sie die Fehlerbehebungsschritte, die entsprechen der Fehlermeldung auftreten.

|**Fehlermeldung**|**Auflösung**|
|---|:---|
|*Contoso100.com Name ist bereits im Netzwerk verwendet. Geben Sie einen Namen, der nicht verwendet wird.*|[Domäne-Namenskonflikt im virtuellen Netzwerk](active-directory-ds-troubleshooting.md#domain-name-conflict)
|*Domänendienste konnte nicht in Azure AD-Mandanten aktiviert werden. Der Dienst muss nicht dazu berechtigt, die Anwendung mit dem Namen "Azure Active Directory Domain Services Sync". Löschen Sie die Anwendung namens "Azure Active Directory Domain Services Sync" und versuchen Sie, für Ihren Mandanten Azure Active Directory-Domänendienste zu aktivieren.*|[Domänendienste keinen Berechtigungen der Anwendung Azure Active Directory Domain Services synchronisieren](active-directory-ds-troubleshooting.md#inadequate-permissions)|
|*Domänendienste konnte nicht in Azure AD-Mandanten aktiviert werden. Domänendienste Anwendung in Azure AD-Mandanten muss nicht die erforderlichen Berechtigungen Domäne aktivieren. Löschen Sie die Anwendung mit d87dcbc6-a371-462e-88e3-28ad15ec4e64 Bezeichner Anwendung, und wiederholen Sie für Ihren Mandanten Azure AD-Domänendienste aktivieren.*|[Die Domänendienste Anwendung ist in Ihrem Mandanten nicht richtig konfiguriert.](active-directory-ds-troubleshooting.md#invalid-configuration)|
|*Domänendienste konnte nicht in Azure AD-Mandanten aktiviert werden. Microsoft Azure AD-Anwendung ist in Azure AD-Mandanten deaktiviert. Die Anwendung mit 00000002-0000-0000-c000-000000000000 Bezeichner Anwendung, und versuchen Sie, für Ihren Mandanten Azure AD-Domänendienste aktivieren.*|[Microsoft Graph-Anwendung ist in Azure AD-Mandanten deaktiviert.](active-directory-ds-troubleshooting.md#microsoft-graph-disabled)|


### <a name="domain-name-conflict"></a>Domäne-Namenskonflikt
**Fehlermeldung:**

*Contoso100.com Name ist bereits im Netzwerk verwendet. Geben Sie einen Namen, der nicht verwendet wird.*

**Behebung:**

Stellen Sie sicher, dass Sie keine vorhandene Domäne mit dem gleichen Domänennamen in diesem virtuellen Netzwerk verfügbar. Angenommen Sie beispielsweise, einer Domäne "contoso.com" bereits für das ausgewählte virtuelle Netzwerk aufgerufen. Später versuchen Sie, eine verwaltete Azure Active Directory-Domänendienste-Domäne mit dem gleichen Domänennamen (d. h. "contoso.com") in diesem virtuellen Netzwerk aktivieren. Fehler auftreten beim Azure AD-Domäne aktivieren.

Dieser Fehler wird aufgrund von Namenskonflikten für den Domänennamen in diesem virtuellen Netzwerk. In diesem Fall verwenden Sie einen anderen Namen Ihrer verwalteten Azure Active Directory-Domänendienste-Domäne einrichten. Sie können auch Bereitstellung aufheben die vorhandene Domäne und dann Azure AD-Domäne aktivieren.


### <a name="inadequate-permissions"></a>Unzureichende Berechtigungen
**Fehlermeldung:**

*Domänendienste konnte nicht in Azure AD-Mandanten aktiviert werden. Der Dienst muss nicht dazu berechtigt, die Anwendung mit dem Namen "Azure Active Directory Domain Services Sync". Löschen Sie die Anwendung namens "Azure Active Directory Domain Services Sync" und versuchen Sie, für Ihren Mandanten Azure Active Directory-Domänendienste zu aktivieren.*

**Behebung:**

Überprüfen Sie, ob eine mit dem Namen "Azure Active Directory Domain Services Sync" in Azure AD-Verzeichnis Anwendung. Wenn diese Anwendung vorhanden ist, löschen Sie und reaktivieren Sie Azure Active Directory-Domänendienste.

Folgendermaßen Sie überprüft das Vorhandensein der Anwendung zu löschen, wenn die Anwendung vorhanden ist:

  1. Navigieren Sie zu der **Azure-Verwaltungsportal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
  2. **Active Directory** -Knoten im linken Bereich auswählen.
  3. Wählen Sie den Azure AD-Mandanten (Verzeichnis) für den Azure Active Directory-Domänendiensten aktivieren möchten.
  4. Navigieren Sie zu der Registerkarte **Applications** .
  5. Die Option **Applikationen Mein Unternehmen besitzt** in der Dropdownliste aus.
  6. Überprüfen Sie für eine Anwendung namens **Azure Active Directory Domain Services synchronisieren**. Wenn die Anwendung vorhanden ist, fahren Sie löschen.
  7. Gelöschte Anwendung versuchen Sie, Azure Active Directory-Domänendienste wieder zu aktivieren.


### <a name="invalid-configuration"></a>Ungültige Konfiguration
**Fehlermeldung:**

*Domänendienste konnte nicht in Azure AD-Mandanten aktiviert werden. Domänendienste Anwendung in Azure AD-Mandanten muss nicht die erforderlichen Berechtigungen Domäne aktivieren. Löschen Sie die Anwendung mit d87dcbc6-a371-462e-88e3-28ad15ec4e64 Bezeichner Anwendung, und wiederholen Sie für Ihren Mandanten Azure AD-Domänendienste aktivieren.*

**Behebung:**

Überprüfen Sie, ob eine Anwendung mit dem Namen 'AzureActiveDirectoryDomainControllerServices' (mit Anwendung der d87dcbc6-a371-462e-88e3-28ad15ec4e64) in Azure AD-Verzeichnis verfügen. Existiert diese Anwendung müssen Sie löschen und Azure AD-Domäne Dienste wieder aktivieren.

Verwenden Sie das folgende PowerShell-Skript die Anwendung finden und löschen.

> [AZURE.NOTE] Dieses Skript verwendet **Azure AD PowerShell Version 2** -Cmdlets. Finden Sie eine vollständige Liste aller verfügbaren Cmdlets und zum Downloaden des Moduls [AzureAD PowerShell Dokumentation](https://msdn.microsoft.com/library/azure/mt757189.aspx).

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Microsoft Graph deaktiviert
**Fehlermeldung:**

Domänendienste konnte nicht in Azure AD-Mandanten aktiviert werden. Microsoft Azure AD-Anwendung ist in Azure AD-Mandanten deaktiviert. Die Anwendung mit 00000002-0000-0000-c000-000000000000 Bezeichner Anwendung, und versuchen Sie, für Ihren Mandanten Azure AD-Domänendienste aktivieren.

**Behebung:**

Überprüfen Sie, ob Sie eine Anwendung mit der ID 00000002-0000-0000-c000-000000000000 deaktiviert haben. Diese Anwendung wird die Anwendung Microsoft Azure AD und Azure AD-Mandanten Graph API-Zugriff. Azure Active Directory-Domänendiensten benötigt diese Anwendung zu Azure AD-Mandanten der verwalteten Domäne synchronisieren.

Beheben Sie diesen Fehler, diese Anwendung und versuchen, für Ihren Mandanten Azure Active Directory-Domänendienste zu aktivieren.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Benutzer können die verwalteten Azure Active Directory-Domänendienste-Domäne anmelden
Sind ein oder mehrere Benutzer in Azure AD-Mandanten neu erstellten verwalteten Domäne anmelden, führen Sie die folgenden Problembehandlungsschritte aus:

- **Mit UPN-Format:** Anmelden mit dem UPN-Format (z. B. 'joeuser@contoso.com') statt SAMAccountName-Format ('CONTOSO\joeuser'). SAMAccountName kann Benutzern, deren UPN-Präfix ist zu lang oder entspricht einem anderen Benutzer in der verwalteten Domäne, automatisch generiert. Das UPN-Format garantiert eindeutig innerhalb von Azure AD-Mandanten.

> [AZURE.NOTE] Wir empfehlen das UPN-Format verwalteten Azure Active Directory-Domänendienste-Domäne anmelden.

- Achten Sie nach die Schritte im Handbuch Erste Schritte [Kennwortsynchronisation aktiviert](active-directory-ds-getting-started-password-sync.md) .

- **Externe Konten:** Sicherstellen Sie, dass das betreffende Benutzerkonto kein externes Konto in Azure AD-Mandanten. Externe Konten zählen Microsoft-Konten (z. B. 'joe@live.com') oder Benutzerkonten von einer externen Azure AD-Verzeichnis. Da Azure Active Directory-Domänendienste verfügt nicht über Anmeldeinformationen für diese Benutzerkonten, können diese Benutzer der verwalteten Domäne anmelden.

- **Konten synchronisiert:** Wenn die betreffenden Benutzerkonten aus einem lokalen Verzeichnis synchronisiert werden, überprüfen Sie Folgendes:
    - Bereitgestellt oder die [neuesten empfohlene Version von Azure AD Connect](active-directory-ds-getting-started-password-sync.md#install-or-update-azure-ad-connect)aktualisiert haben.

    - Azure AD mit [Führen Sie eine vollständige Synchronisierung](active-directory-ds-getting-started-password-sync.md)konfiguriert haben.

    - Je nach Größe des Verzeichnisses kann dauern für Benutzerkonten und Anmeldeinformationen Hashes in Azure AD-Domänendienste verfügbar sein. Sicherstellen Sie, dass Sie Wiederholungsversuchen lange Authentifizierung (je nach Größe des Verzeichnisses - Stunden einen Tag oder zwei großen Verzeichnissen).

    - Wenn das Problem weiterhin besteht, nachdem die vorhergehenden Schritte überprüft, starten Sie Microsoft Azure AD-Synchronisierungsdienst. Starten Sie von Ihrem Computer synchronisieren eine Befehlszeile und führen Sie die folgenden Befehle:
      1. Net Stop "Microsoft Azure AD Sync"
      2. Net Start "Microsoft Azure AD Sync"

- **Nur-Cloud - Konten**: das betreffende Benutzerkonto nur Cloud Benutzerkonto sicher, dass der Benutzer sein Kennwort geändert hat, nachdem Azure Active Directory-Domänendienste aktiviert. Dadurch wird die Anmeldeinformationen Hashes generiert für Azure AD-Domäne erforderlich.


## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Benutzer von Azure AD-Mandanten entfernt werden nicht aus der verwalteten Domäne entfernt.
Azure AD verhindert versehentliches Löschen von Benutzerobjekten. Beim Löschen eines Benutzerkontos von Azure AD-Mandanten wird das entsprechende Benutzerobjekt in den Papierkorb verschoben. Löschvorgang der verwalteten Domäne synchronisiert wird, wird das entsprechende Benutzerkonto deaktiviert gekennzeichnet werden. Dieses Feature können Sie wiederherstellen oder das Benutzerkonto später wiederherstellen.

Um das Benutzerkonto aus der verwalteten Domäne vollständig zu entfernen, Löschen des Benutzers von Azure AD-Mandanten. Verwenden Sie das Remove-MsolUser PowerShell-Cmdlet mit dem '-RemoveFromRecycleBin' option, wie in diesem [MSDN-Artikel](https://msdn.microsoft.com/library/azure/dn194132.aspx)beschrieben.


## <a name="contact-us"></a>Kontaktieren Sie uns
Wenden Sie sich an das Produktteam Azure Active Directory Domain Services, [Meinung oder Support] (Active-Directory-ds-Kontakt-us.md).
