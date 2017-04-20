<properties
    pageTitle="Erste Schritte mit Azure Schlüssel | Microsoft Azure"
    description="Mithilfe dieses Lernprogramm Einstieg mit Azure Schlüssel verstärkten Container in Azure zum Speichern und Verwalten von kryptografischen Schlüssel und geheime Informationen in Azure erstellen."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>

# <a name="get-started-with-azure-key-vault"></a>Erste Schritte mit Azure Schlüssel #
Azure Key Vault ist in den meisten Regionen verfügbar. Weitere Informationen finden Sie unter [Key Vault Preisseite](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Einführung  
Mithilfe dieses Lernprogramm Einstieg mit Azure Schlüssel verstärkten Container (Vault) in Azure zum Speichern und Verwalten von kryptografischen Schlüssel und geheime Informationen in Azure erstellen. Er führt Sie durch den Prozess der Verwendung von Azure PowerShell ein Depot erstellen, enthält einen Schlüssel oder ein Kennwort, das Sie mit einer Azure-Anwendung verwenden können. Es zeigt dann Sie wie eine Anwendung, Schlüssel oder Kennwort verwenden kann.

**Geschätzte Zeit:** 20 Minuten

>[AZURE.NOTE]  Dieses Lernprogramm enthält keine Informationen zur Azure-Anwendung schreiben, einer der Schritte enthält, wie eine Anwendung einen Schlüssel oder ein Schlüssel im Schlüssel Tresor autorisieren.
>
>In diesem Lernprogramm verwendet Azure PowerShell. Plattformübergreifende Befehlszeilenschnittstelle Anleitung finden Sie in [diesem Lernprogramm entsprechende](key-vault-manage-with-cli.md).

Übersicht über Azure Key Vault finden Sie unter [Neuigkeiten Azure Key Vault?](key-vault-whatis.md)

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm benötigen Sie Folgendes:

- Ein Abonnement für Microsoft Azure. Wenn Sie eine nicht verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)anmelden.
- Azure PowerShell **mindestens Version 1.1.0**. Azure PowerShell installieren und Ihr Azure-Abonnement zuordnen, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Wenn Sie Azure PowerShell bereits installiert und die Version von Azure PowerShell-Konsole nicht kennen, geben `(Get-Module azure -ListAvailable).Version`. Wenn Sie Azure PowerShell Version 0.9.1 durch 0.9.8 installiert haben, können Sie noch in diesem Lernprogramm mit kleinen Änderungen. Sie müssen z. B. die `Switch-AzureMode AzureResourceManager` Befehl und Azure Key Vault-Befehle wurden geändert. Finden Sie eine Liste der Cmdlets Key Vault Versionen 0.9.1 bis 0.9.8 [Azure Key Vault Cmdlets](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.98\).aspx). 
- Eine Anwendung verwenden konfiguriert oder das Kennwort, das in diesem Lernprogramm erstellen. Eine beispielanwendung ist im [Microsoft Download Center](http://www.microsoft.com/en-us/download/details.aspx?id=45343)verfügbar. Eine Anleitung finden Sie die Readme-Datei.


Dieses Lernprogramm ist für Anfänger Azure PowerShell, aber setzt voraus, dass die grundlegenden Konzepte wie Module, Cmdlets und Sessions verstehen. Weitere Informationen finden Sie unter [Erste Schritte mit Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx).

Zu Hilfe für alle Cmdlets, die in diesem Lernprogramm sehen, verwenden Sie das Cmdlet " **Get-Help** ".

    Get-Help <cmdlet-name> -Detailed

Hilfe für das **Login-AzureRmAccount** -Cmdlet beispielsweise:

    Get-Help Login-AzureRmAccount -Detailed

Lesen Sie die folgenden Lernprogramme Azure Ressourcenmanager in Azure PowerShell vertraut:

- [Zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)
- [Mithilfe von Azure PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md)


## <a id="connect"></a>Verbindung zu Ihren Abonnements ##

Starten Sie eine Azure PowerShell-Sitzung und melden Sie an, um Ihre Azure-Konto mit dem folgenden Befehl:  

    Login-AzureRmAccount 

Beachten Sie, dass Sie einer bestimmten Instanz von Azure, z. B. Azure Government verwenden-Umgebungsparameter mit diesem Befehl verwenden. Zum Beispiel:`Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

Geben Sie im Popupmenü Browserfenster Ihre Azure Benutzernamen und Kennwort. Azure PowerShell Ruft alle Abonnements, die mit diesem Konto und standardmäßig verwendet die erste.

Wenn Sie mehrere Abonnements vorhanden sind und einer bestimmten Azure Key Vault verwenden möchten, geben Sie Folgendes um Abonnements für Ihr Konto anzuzeigen:

    Get-AzureRmSubscription

Um das Abonnement mit anzugeben, geben Sie:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Weitere Informationen zur Konfiguration von Azure PowerShell Informationen Sie [zur Installation und Konfiguration von Azure PowerShell](../powershell-install-configure.md).


## <a id="resource"></a>Erstellen Sie eine neue Ressourcengruppe ##

Wenn Sie Azure-Ressourcen-Manager verwenden, werden alle zugehörige Ressourcen innerhalb einer Ressourcengruppe erstellt. Es wird eine neue Ressourcengruppe mit dem Namen **ContosoResourceGroup** in diesem Lernprogramm erstellen:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Ein Schlüssel Depot erstellen ##

Verwenden Sie das Cmdlet [Neu AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt603736\(v=azure.300\).aspx) Schlüssel Depot erstellen. Dieses Cmdlet hat drei erforderliche Parameter: einen **Namen**, einen **Schlüssel Depotnamen**und **Standort**.

Wenn der Vault-Name des **ContosoKeyVault**, der Ressource **ContosoResourceGroup**Namen und Speicherort **Ostasien**verwenden, beispielsweise:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Die Ausgabe dieses Cmdlet zeigt Eigenschaften der soeben erstellten schlüsseltresors. Die zwei wichtigsten Eigenschaften sind:

- **Depotnamen**: In dem Beispiel ist **ContosoKeyVault**. Sie werden diesen Namen für andere Schlüssel Vault-Cmdlets verwenden.
- **Vault-URI**: In dem Beispiel ist https://contosokeyvault.vault.azure.net/. Ihrem Tresor über die REST-API verwenden, müssen dieser URI verwenden.

Ein Azure-Konto darf jetzt alle Operationen auf diesen Schlüssel. Ist niemand.

>[AZURE.NOTE]  Wenn Fehlermeldung **das Abonnement nicht registriert wird, um Namespaces "Microsoft.KeyVault"** Wenn Sie versuchen, erstellen Sie den neuen Schlüssel Tresor ausführen `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` , und wiederholen Sie den Befehl neu AzureRmKeyVault. Weitere Informationen finden Sie unter [AzureRmResourceProvider registrieren](https://msdn.microsoft.com/library/azure/mt759831\(v=azure.300\).aspx).
>

## <a id="add"></a>Hinzufügen eines Schlüssels oder geheimen Schlüssel Depot ##

Azure Key Vault Software geschützt Schlüssel erstellen möchten, verwenden Sie das Cmdlet [Hinzufügen AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) und geben Sie Folgendes ein:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Allerdings haben Sie einen vorhandenen Schlüssel Software geschützt ein. PFX-Datei gespeichert auf Laufwerk C:\ in einer Datei namens softkey.pfx in Azure Schlüssel Depot hochladen möchten Folgendes legen Sie die Variable **Securepfxpwd** ein Kennwort **123** für die. PFX-Datei:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Geben Sie Folgendes ein, um den Schlüssel importiert die. PFX-Datei, schützt den Schlüssel Software im Schlüssel Vault-Dienst:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


Sie können nun diesen Schlüssel verweisen, die erstellt oder mithilfe des URI Azure Schlüssel Tresor hochgeladen. **Https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** die aktuelle Version zu verwenden, und **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** zu dieser speziellen Version verwenden.  

Um den URI für den Schlüssel anzuzeigen, geben Sie Folgendes ein:

    $Key.key.kid

Das Depot ein Geheimnis hinzufügen ein SQLPassword benannt und hat den Wert Pa$ $w0rd Azure Schlüssel Tresor zuerst umwandeln Wert der Pa$ $w0rd eine sichere Zeichenfolge indem Sie Folgendes eingeben:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Geben Sie dann Folgendes ein:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Sie können nun dieses Kennwort verweisen, das mithilfe des URI Azure Schlüssel Tresor hinzugefügt. **Https://ContosoVault.vault.azure.net/secrets/SQLPassword** die aktuelle Version zu verwenden, und **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** zu dieser speziellen Version verwenden.

Um den URI für diesen Schlüssel anzuzeigen, geben Sie Folgendes ein:

    $secret.Id

Sehen Sie sich den Schlüssel oder den Schlüssel ein, den Sie gerade erstellt haben:

- Um den Schlüssel anzuzeigen, geben Sie Folgendes ein:`Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
- Um geheime anzuzeigen, geben Sie Folgendes ein:`Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Jetzt kann der Schlüssel Depot und Schlüssel oder geheimen Applikationen verwendet. Autorisieren Sie Applikationen verwenden.  

## <a id="register"></a>Registrieren einer Anwendung in Azure Active Directory ##

Dieser Schritt würde in der Regel von einem Entwickler auf einem separaten Computer ausgeführt. Es ist nicht spezifisch für Azure Key Vault jedoch ist hier der Vollständigkeit enthalten.


>[AZURE.IMPORTANT] Das Lernprogramm, Ihr Konto das Depot und die Anwendung, die Sie in diesem Schritt registrieren müssen alle im gleichen Azure Verzeichnis sein.

Ein Schlüssel Depot verwenden, müssen mit einem Token aus Azure Active Directory authentifizieren. Dazu muss der Besitzer der Anwendung die Anwendung in Azure Active Directory registrieren. Am Ende der Registrierung erhält der Anwendungsbesitzer die folgenden Werten:


- **ID der Anwendung** (auch bekannt als Client-ID) als **Authentifizierungsschlüssel** (auch den gemeinsamen geheimen Schlüssel). Die Anwendung muss diese Werte Azure Active Directory, ein Token abzurufen vorlegen. Konfiguration die Anwendung hierfür ist anwendungsspezifisch. Der Anwendungsbesitzer legt für Key Vault-Beispiel-Anwendung diese Werte in der App.config.Datei.

So registrieren Sie die Anwendung in Azure Active Directory

1. Mit klassischen Azure-Portal anmelden.
2. Auf der linken Seite auf **Active Directory**, und wählen Sie das Verzeichnis, in dem Sie Ihre Anwendung registrieren. <br> <br> **Hinweis:** Sie müssen dasselbe Verzeichnis auswählen, das Azure-Abonnement enthält mit dem Key Vault erstellt. Wenn Sie nicht das Verzeichnis wissen ist, **Klicken Sie**mit dem Schlüssel Depot erstellt Abonnement identifizieren und notieren Sie den Namen des Verzeichnisses in der letzten Spalte angezeigt.

3. Klicken Sie auf **Anwendung**. Wenn keine apps zu Ihrem Verzeichnis hinzugefügt wurden, werden auf dieser Seite nur die Verknüpfung **einer Anwendung hinzufügen** . Klicken Sie auf den Link, oder alternativ auf **Hinzufügen** auf der Befehlsleiste.
4.  **Anwendung hinzufügen** -Assistenten auf den **Was möchten Sie tun?** auf **Mein Unternehmen entwickelten Anwendung hinzufügen**.
5.  Geben Sie auf der Seite **Angaben über Ihre Anwendung** einen Namen für die Anwendung und wählen Sie **WEB APPLICATION oder WEB-API** (Standard). Klicken Sie auf **Weiter** .
6.  Geben Sie auf der Seite **Eigenschaften von App** **Zeichen auf URL** und **APP-ID-URI** für Ihre Webanwendung. Wenn die Anwendung keinen dieser Werte, Sie können sie für diesen Schritt (Sie können beispielsweise http://test1.contoso.com für beide Felder). Es spielt keine Rolle, wenn diese Sites vorhanden sind. Wichtig ist, dass die app-ID URI für jede Anwendung für jede Anwendung im Verzeichnis. Das Verzeichnis verwendet dieser Zeichenfolge zu Ihrer app.
7.  Klicken Sie auf das Speichern im Assistenten **abgeschlossen** .
8.  Klicken Sie auf der Seite **Quick Start** **Konfigurieren**.
9.  Gehen Sie zum Abschnitt **Schlüssel** wählen Sie die Dauer und klicken Sie auf **Speichern**. Die Seite wird aktualisiert und zeigt nun einen Wert. Konfigurieren Sie die Anwendung mit dieser Schlüsselwert und die **CLIENT-ID** -Wert. (Anleitung für diese Konfiguration sind anwendungsspezifische.)
10. Kopieren Sie den Client-ID-Wert auf dieser Seite im nächsten Schritt mit Berechtigungen für den Tresor festlegen.

## <a id="authorize"></a>Autorisieren Sie die Anwendung des Schlüssels oder geheimen ##

Verwenden Sie zur Autorisierung der Anwendung Zugriff auf die Schlüssel oder Schlüssel im Tresor das Cmdlet "  [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625\(v=azure.300\).aspx) ".

Wenn der Vault-Name ist **ContosoKeyVault** und die Anwendung zu autorisieren, hat eine Client-ID des 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed und autorisieren die Anwendung mit Schlüsseln in Ihrem Tresor signieren und entschlüsseln möchten, führen Sie beispielsweise folgende:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Wenn Sie dieselbe Anwendung vertrauliche Informationen in Ihrem Tresor lesen autorisieren möchten, führen Sie folgenden Befehl:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Wenn ein Hardwaresicherheitsmodul (HSM) verwendet werden soll ##

Für zusätzliche Sicherheit können importieren oder Schlüssel in Hardwaresicherheitsmodule (HSMs), die Grenze HSM niemals verlassen. Die HSMs werden FIPS 140-2 Level 2 überprüft Wenn diese Anforderung auf Sie nicht zutrifft, überspringen Sie diesen Abschnitt und [Schlüssel Tresor löschen und Schlüssel und geheime Schlüssel](#delete)zur.

Um diese HSM-geschützten Schlüssel erstellen, verwenden Sie [Azure Key Vault Premium-Service-Tier HSM-geschützten Schlüssel unterstützen](https://azure.microsoft.com/pricing/free-trial/). Beachten Sie zusätzlich, dass diese Funktionalität nicht für Azure China.


Beim Erstellen der wichtigsten Tresors fügen Sie hinzu **-SKU** -Parameter:


    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



Sie können Tasten (siehe oben) Software geschützt und HSM-geschützte dieses wichtigsten Tresor hinzufügen. Einen HSM-geschützter Schlüssel erstellen, der **-Ziel** Parameter "HSM":

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

Können Sie den folgenden Befehl zum Importieren eines Schlüssels aus einer. PFX-Datei auf Ihrem Computer. Dieser Befehl importiert die Taste in HSMs Key Vault-Service:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


Der nächste Befehl importiert eine "schalten Sie Ihren eigenen Schlüssel" (BYOK)-Paket. Dabei können Sie Ihren Schlüssel in lokalen HSM und erläutert im Schlüssel Vault-Dienst ohne Verlassen der HSM-Grenze Schlüssel übertragen:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Weitere Informationen zu diesem Paket BYOK finden Sie unter [generieren und Übertragung HSM-geschützten Schlüssel Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Löschen Sie Key Vault und zugeordneten Schlüssel und Kennwörter ##

Benötigen Sie mehr Schlüssel Depot und den Schlüssel oder geheimen Schlüssel enthält, können Sie die wichtige Tresor mithilfe des Cmdlets [Entfernen AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt619485\(v=azure.300\).aspx) löschen:

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Oder Sie können eine gesamte Azure Ressourcengruppe Schlüssel Depot und andere Ressourcen, die in dieser Gruppe enthalten:

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Andere Azure PowerShell-Cmdlets ##

Andere Befehle, die nützlich für die Verwaltung von Azure Key Vault auftreten:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Dieser Befehl ruft eine tabellarische Anzeigen aller Tasten und ausgewählten Eigenschaften.
- `$Keys[0]`: Dieser Befehl zeigt eine vollständige Liste der Eigenschaften für den angegebenen Schlüssel
- `Get-AzureKeyVaultSecret`: Dieser Befehl zeigt eine tabellarische Anzeigen aller geheimen Namen und ausgewählten Eigenschaften.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Beispiel einen bestimmten Schlüssel entfernen.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Beispiel einen bestimmten Schlüssel entfernen.


## <a id="next"></a>Nächste Schritte ##

Ein Anschluss, die eine Anwendung Azure Key Vault verwendet, finden Sie unter [Verwenden Azure Key Vault von einer Web-Anwendung](key-vault-use-from-web-application.md).

Verwendung der wichtigsten Depot finden Sie unter [Azure Key Vault-Protokollierung](key-vault-logging.md).

Finden Sie eine Liste der neuesten Azure PowerShell-Cmdlets für Azure Key Vault [Azure Key Vault Cmdlets](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.300\).aspx). 
 

Programmierung Verweise finden Sie unter [Azure Key Vault-Entwicklerhandbuch](key-vault-developers-guide.md).
