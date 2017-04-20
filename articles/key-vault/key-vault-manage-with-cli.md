<properties
    pageTitle="Schlüsseltresor mit CLI verwalten | Microsoft Azure"
    description="Verwenden Sie dieses Lernprogramm automatisieren Aufgaben im Schlüsseltresor mit CLI"
    services="key-vault"
    documentationCenter=""
    authors="BrucePerlerMS"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="bruceper"/>

# <a name="manage-key-vault-using-cli"></a>Verwalten Sie Schlüsseltresor mit CLI #
Azure Key Vault ist in den meisten Regionen verfügbar. Weitere Informationen finden Sie unter [Key Vault Preisseite](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Einführung  
Mithilfe dieses Lernprogramm Einstieg mit Azure Schlüssel verstärkten Container (Vault) in Azure zum Speichern und Verwalten von kryptografischen Schlüssel und geheime Informationen in Azure erstellen. Es führt Sie durch die Verwendung von Azure plattformübergreifende Befehlszeilenschnittstelle ein Depot erstellen, enthält einen Schlüssel oder ein Kennwort, das Sie mit einer Azure-Anwendung verwenden können. Es zeigt dann Sie wie eine Anwendung dieser Schlüssel bzw. das Kennwort dann verwenden kann.

**Geschätzte Zeit:** 20 Minuten

>[AZURE.NOTE]  Dieses Lernprogramm enthält keine Informationen zur Azure-Anwendung schreiben, einer der Schritte die veranschaulicht enthält, wie eine Anwendung einen Schlüssel oder ein Schlüssel im Schlüssel Tresor autorisieren.
>
>Sie können nicht derzeit Azure Key Vault in Azure-Portal konfigurieren. Verwenden Sie diese Anleitung plattformübergreifende Befehlszeilenschnittstelle. Oder Hinweise Azure PowerShell [Lernprogramms entspricht](key-vault-get-started.md).

Übersicht über Azure Key Vault finden Sie unter [Neuigkeiten Azure Key Vault?](key-vault-whatis.md)

## <a name="prerequisites"></a>Erforderliche Komponenten
Um dieses Lernprogramm benötigen Sie Folgendes:

- Ein Abonnement für Microsoft Azure. Wenn Sie eine nicht verfügen, können Sie für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial)anmelden.
- Befehlszeilenschnittstelle Version 0.9.1 oder höher. Installieren Sie die neueste Version und zum Azure Abonnement, finden Sie unter [Installieren und Konfigurieren der Azure plattformübergreifende Befehlszeilenschnittstelle](../xplat-cli-install.md).
- Eine Anwendung verwenden konfiguriert oder das Kennwort, das in diesem Lernprogramm erstellen. Eine beispielanwendung ist im [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343)verfügbar. Eine Anleitung finden Sie die Readme-Datei.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Hilfe mit Azure plattformübergreifende Befehlszeilen-Schnittstelle

In diesem Lernprogramm wird davon ausgegangen, dass Sie mit der Befehlszeilenschnittstelle (Bash Terminal Befehlszeile)

-Hilfe oder h - Parameter wird die Hilfe für Befehle anzeigen. Auch die Azure Help [Befehl] [Optionen] Format auch verwendet werden kann, um die gleichen Informationen zurückgeben. Die folgenden Befehle geben z. B. dieselbe Informationen zurück:

    azure account set --help

    azure account set -h

    azure help account set

Im Zweifel vom Befehl benötigten Parametern finden Sie Hilfe mit Hilfe -h und Azure Help [Befehl].

Lesen Sie die folgenden Lernprogramme zu kennen Azure Ressourcenmanager in Azure plattformübergreifende Befehlszeilen-Schnittstelle:

- [Zum Installieren und Konfigurieren von Azure plattformübergreifende Befehlszeilenschnittstelle](../xplat-cli-install.md)
- [Mithilfe von Azure plattformübergreifende Befehlszeilenschnittstelle mit Azure Resource Manager](../xplat-cli-azure-resource-manager.md)


## <a name="connect-to-your-subscriptions"></a>Verbindung zu Ihren Abonnements

Um eine Organisationseinheit Konto anzumelden, verwenden Sie den folgenden Befehl ein:

    azure login -u username -p password

oder wenn Sie durch Eingabe interaktiv anmelden

    azure login

>[AZURE.NOTE]  Login-Methode funktioniert nur mit Organisationseinheiten Konto. Eine Organisation Firma ist ein Benutzer, der von Ihrer Organisation verwaltet und in Ihrer Organisation Azure Active Directory Mandanten definiert.


Wenn Sie aktuell keine Organisationseinheit Konto und Microsoft-Konto verwenden an Ihr Azure-Abonnement anmelden, können Sie problemlos mit den folgenden Schritten erstellen.

1.  Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com/)und klicken Sie auf Active Directory.
2.  Wenn kein Verzeichnis vorhanden ist, wählen Sie Ihr Verzeichnis erstellen und Informationen Sie angeforderten.
3.  Wählen Sie das Verzeichnis und fügen Sie einen neuen Benutzer hinzu. Dieser neue Benutzer ist eine organisatorische Konto. Während der Erstellung des Benutzers wird sowohl eine e-Mail-Adresse für den Benutzer und ein temporäres Kennwort angegeben werden. Speichern Sie diese Informationen in einem anderen Schritt verwendet wird.
4.  Das Portal bearbeiten Sie und wählen Sie dann die Administratoren. Hinzufügen, und fügen Sie den neuen Benutzer als Co-Administrator. Dadurch Konto Organisation Ihre Azure-Abonnement verwalten.
5.  Schließlich melden Sie Azure-Portal und dann in die neue Organisationseinheit Konto. Ist das erste Mal anmelden mit diesem Konto werden Sie aufgefordert, das Kennwort zu ändern.

Weitere Informationen über eine Organisation Konto mit Microsoft Azure finden Sie unter [für Microsoft Azure Organisation anmelden](../active-directory/sign-up-organization.md).

Wenn Sie mehrere Abonnements vorhanden sind und einer bestimmten Azure Key Vault verwenden möchten, geben Sie Folgendes um Abonnements für Ihr Konto anzuzeigen:

    azure account list

Um das Abonnement mit anzugeben, geben Sie:

    azure account set <subscription name>

Weitere Informationen zur Konfiguration von Azure plattformübergreifende Befehlszeilenschnittstelle finden Sie unter [Installieren und konfigurieren Sie Azure plattformübergreifende Befehlszeilenschnittstelle](../xplat-cli-install.md).


## <a name="switch-to-using-azure-resource-manager"></a>Wechseln Sie zur Azure-Ressourcen-Manager verwenden

Key Vault erfordert Azure-Ressourcen-Manager, so geben Sie Folgendes in Azure-Ressourcen-Manager-Modus wechseln:

    azure config mode arm

## <a name="create-a-new-resource-group"></a>Erstellen Sie eine neue Ressourcengruppe

Wenn Azure-Ressourcen-Manager verwenden, werden alle zugehörige Ressourcen innerhalb einer Ressourcengruppe erstellt. Es erstellt eine neue Ressourcengruppe 'ContosoResourceGroup' für dieses Lernprogramm.

    azure group create 'ContosoResourceGroup' 'East Asia'

Der erste Parameter den Namen ist und der zweite Parameter ist der Speicherort. Position, Befehl `azure location list` wie an einem alternativen Speicherort in diesem Beispiel zu identifizieren. Wenn Sie weitere Informationen benötigen, geben Sie ein:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>Registrieren des Ressourcenanbieters Key Vault
Stellen Sie sicher, dass dieser Schlüssel Depot Ressource in Ihrem Abonnement registrieren:

`azure provider register Microsoft.KeyVault`

Dies muss nur einmal pro Abonnement ausgeführt werden.


## <a name="create-a-key-vault"></a>Ein Schlüssel Depot erstellen

Verwenden der `azure keyvault create` Befehl Schlüssel Depot erstellen. Das Skript enthält drei erforderliche Parameter: eine Gruppe Ressourcenname ein Schlüssel Depot und den geografischen Standort.

Wenn der Vault-Name des ContosoKeyVault, der Ressource ContosoResourceGroup Namen und Speicherort Ostasien verwenden, beispielsweise:

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

Die Ausgabe dieses Befehls zeigt Eigenschaften der soeben erstellten schlüsseltresors. Die zwei wichtigsten Eigenschaften sind:

- **Name**: In dem Beispiel ist ContosoKeyVault. Sie werden diesen Namen für andere Schlüssel Vault-Cmdlets verwenden.
- **VaultUri**: In dem Beispiel ist https://contosokeyvault.vault.azure.net. Ihrem Tresor über die REST-API verwenden, müssen dieser URI verwenden.

Ein Azure-Konto darf jetzt alle Operationen auf diesen Schlüssel. Ist niemand.


## <a name="add-a-key-or-secret-to-the-key-vault"></a>Hinzufügen eines Schlüssels oder geheimen Schlüssel Depot

Azure Key Vault Software geschützt Schlüssel erstellen, verwenden Sie die `azure key create` Befehl, und geben Sie Folgendes ein:

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

Allerdings haben Sie einen vorhandenen Schlüssel in einer Datei PEM-als lokale Datei in eine Datei namens softkey.pem in Azure Schlüssel Depot hochladen möchten, geben Sie Folgendes zum import des Schlüssels aus der. PEM-Datei, die den Schlüssel Software Service Key Vault schützt:

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

Sie können jetzt den Schlüssel verweisen, den erstellt oder mithilfe des URI Azure Schlüssel Tresor hochgeladen. **Https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** die aktuelle Version zu verwenden, und **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** zu dieser speziellen Version verwenden.

Um das Depot ein SQLPassword benannt ist und der Wert der Pa$ $w0rd in Azure Schlüssel Depot ein Geheimnis hinzufügen Folgendes:

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

Sie können nun dieses Kennwort verweisen, das mithilfe des URI Azure Schlüssel Tresor hinzugefügt. **Https://ContosoVault.vault.azure.net/secrets/SQLPassword** die aktuelle Version zu verwenden, und **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** zu dieser speziellen Version verwenden.

Sehen Sie sich den Schlüssel oder den Schlüssel ein, den Sie gerade erstellt haben:

- Um den Schlüssel anzuzeigen, geben Sie Folgendes ein:`azure keyvault key list --vault-name 'ContosoKeyVault'`
- Um geheime anzuzeigen, geben Sie Folgendes ein:`azure keyvault secret list --vault-name 'ContosoKeyVault'`


## <a name="register-an-application-with-azure-active-directory"></a>Registrieren einer Anwendung in Azure Active Directory

Dieser Schritt würde in der Regel von einem Entwickler auf einem separaten Computer ausgeführt. Es ist nicht spezifisch für Azure Key Vault jedoch ist hier der Vollständigkeit enthalten.


>[AZURE.IMPORTANT] Das Lernprogramm, Ihr Konto das Depot und die Anwendung, die Sie in diesem Schritt registrieren müssen alle im gleichen Azure Verzeichnis sein.

Ein Schlüssel Depot verwenden, müssen mit einem Token aus Azure Active Directory authentifizieren. Dazu muss der Besitzer der Anwendung die Anwendung in Azure Active Directory registrieren. Am Ende der Registrierung erhält der Anwendungsbesitzer die folgenden Werten:


- **ID der Anwendung** (auch bekannt als Client-ID) als **Authentifizierungsschlüssel** (auch den gemeinsamen geheimen Schlüssel). Die Anwendung muss beide Werte Azure Active Directory ein Token zu präsentieren. Konfiguration die Anwendung hierfür ist anwendungsspezifisch. Der Anwendungsbesitzer legt für Key Vault-Beispiel-Anwendung diese Werte in der App.config.Datei.



So registrieren Sie die Anwendung in Azure Active Directory

1. Melden Sie sich bei Azure-Portal.
2. Auf der linken Seite auf **Active Directory**, und wählen Sie das Verzeichnis, in dem Sie Ihre Anwendung registrieren. <br> <br> Hinweis: Sie müssen das gleiche Verzeichnis auswählen, das Azure-Abonnement enthält mit dem Key Vault erstellt. Wenn Sie nicht das Verzeichnis wissen ist, **Klicken Sie**mit dem Schlüssel Depot erstellt Abonnement identifizieren und notieren Sie den Namen des Verzeichnisses in der letzten Spalte angezeigt.

3. Klicken Sie auf **Anwendung**. Das Verzeichnis keine apps hinzugefügt haben, zeigt diese Seite nur den Link **Hinzufügen einer Anwendung** . Klicken Sie auf den Link, oder alternativ auf **Hinzufügen** auf der Befehlsleiste.
4.  **Anwendung hinzufügen** -Assistenten auf den **Was möchten Sie tun?** auf **Mein Unternehmen entwickelten Anwendung hinzufügen**.
5.  Geben Sie auf der Seite **Angaben über Ihre Anwendung** einen Namen für Ihre Anwendung, und wählen Sie **WEB APPLICATION oder WEB-API** (Standard). Klicken Sie auf Weiter.
6.  Geben Sie auf der Seite **Eigenschaften von App** **Zeichen auf URL** und **APP-ID-URI** für Ihre Webanwendung. Wenn die Anwendung keinen dieser Werte, Sie können sie für diesen Schritt (Sie können beispielsweise http://test1.contoso.com für beide Felder). Es spielt keine Rolle, wenn diese Sites vorhanden sind. wichtig ist, dass die app-ID URI für jede Anwendung für jede Anwendung im Verzeichnis. Das Verzeichnis verwendet dieser Zeichenfolge zu Ihrer app.
7.  Klicken Sie zum Speichern der Änderungen im Assistenten auf abgeschlossen.
8.  Klicken Sie auf der Seite Quick Start **Konfigurieren**.
9.  Gehen Sie zum Abschnitt **Schlüssel** wählen Sie die Dauer und klicken Sie auf **Speichern**. Die Seite wird aktualisiert und zeigt nun einen Wert. Konfigurieren Sie die Anwendung mit dieser Schlüsselwert und die **CLIENT-ID** -Wert. (Anleitung für diese Konfiguration werden anwendungsspezifische.)
10. Kopieren Sie den Client-ID-Wert auf dieser Seite im nächsten Schritt mit Berechtigungen für den Tresor festlegen.




## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Autorisieren Sie die Anwendung des Schlüssels oder geheimen

Verwenden Sie zur Autorisierung der Anwendung Zugriff auf die Schlüssel oder Schlüssel im Tresor der `azure keyvault set-policy` Befehl.

Beispielsweise wenn Vault ContosoKeyVault heißt Anwendung autorisieren möchten eine Client-ID des 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed hat und Sie die Anwendung zum Entschlüsseln und Signieren mit Schlüsseln in Ihrem Tresor autorisieren möchten, führen Sie Folgendes:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '["decrypt","sign"]'

>[AZURE.NOTE] Wenn Sie auf Windows-Befehlszeile ausführen, sollte ersetzen einfache Anführungszeichen in doppelte Anführungszeichen und Escapezeichen auch die internen Anführungszeichen. Beispiel: "[\"entschlüsseln\",\"Zeichen\"]".

Wenn Sie dieselbe Anwendung vertrauliche Informationen in Ihrem Tresor lesen autorisieren möchten, führen Sie folgenden Befehl:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '["get"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Wenn ein Hardwaresicherheitsmodul (HSM) verwendet werden soll ##

Für zusätzliche Sicherheit können importieren oder Schlüssel in Hardwaresicherheitsmodule (HSMs), die Grenze HSM niemals verlassen. Die HSMs werden FIPS 140-2 Level 2 überprüft Wenn diese Anforderung auf Sie nicht zutrifft, überspringen Sie diesen Abschnitt und [Schlüssel Tresor löschen und Schlüssel und geheime Schlüssel](#delete-the-key-vault-and-associated-keys-and-secrets)zur.

Um diese HSM-geschützten Schlüssel erstellen, müssen Sie ein Vault-Abonnement verfügen, HSM-geschützten Schlüssel unterstützt.

Beim Erstellen der schlüsseltresor fügen Sie hinzu 'Sku'-Parameter:

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

Sie können diesen Tresor Tasten (siehe oben) Software geschützt und HSM-geschützte hinzufügen. Erstellen Sie einen HSM-geschützter Schlüssel festlegen Sie Zielparameter, HSM":

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

Folgendes können Sie einen Schlüssel aus einer PEM-Datei auf Ihrem Computer importieren. Dieser Befehl importiert die Taste in HSMs Key Vault-Service:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

Der nächste Befehl importiert eine "schalten Sie Ihren eigenen Schlüssel" (BYOK)-Paket. So können Sie Ihren Schlüssel in lokalen HSM und erläutert im Schlüssel Vault-Dienst ohne Verlassen der HSM-Grenze Schlüssel übertragen:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

Weitere Informationen zu diesem Paket BYOK finden Sie unter [HSM-Protected mit Azure Schlüssel verwenden](key-vault-hsm-protected-keys.md).


## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Löschen Sie Key Vault und zugeordneten Schlüssel und Kennwörter

Benötigen Sie mehr Schlüssel Depot und den Schlüssel oder geheimen Schlüssel enthält, können Sie mithilfe des Befehls Azure schlüsseltresor löschen Key Vault löschen:

    azure keyvault delete --vault-name 'ContosoKeyVault'

Oder Sie können eine gesamte Azure Ressourcengruppe Schlüssel Depot und andere Ressourcen, die in dieser Gruppe enthalten:

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Andere Befehle Azure plattformübergreifende Befehlszeilenschnittstelle

Andere Befehle, die möglicherweise nützlich für die Verwaltung von Azure Key Vault.

Dieser Befehl zeigt eine tabellarische Anzeigen aller Tasten und ausgewählten Eigenschaften:

    azure keyvault key list --vault-name 'ContosoKeyVault'

Dieser Befehl zeigt eine vollständige Liste der Eigenschaften für den angegebenen Schlüssel:

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Dieser Befehl zeigt eine tabellarische Anzeigen aller geheimen Namen und ausgewählten Eigenschaften:

    azure keyvault secret list --vault-name 'ContosoKeyVault'

Hier ist ein Beispiel für einen bestimmten Schlüssel entfernen:

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Hier ist ein Beispiel für einen bestimmten Schlüssel entfernen:

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a>Nächste Schritte

Programmierung Verweise finden Sie unter [Azure Key Vault-Entwicklerhandbuch](key-vault-developers-guide.md).
