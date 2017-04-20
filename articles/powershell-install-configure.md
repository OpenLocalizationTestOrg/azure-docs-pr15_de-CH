<properties
    pageTitle="Zum Installieren und Konfigurieren von Azure PowerShell"
    description="Informationen Sie zum Installieren und Konfigurieren von Azure PowerShell."
    editor="tysonn"
    manager="dongill"
    documentationCenter=""
    services=""
    authors="coreyp-at-msft"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="powershell"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="coreyp"/>

# <a name="how-to-install-and-configure-azure-powershell"></a>Zum Installieren und Konfigurieren von Azure PowerShell

<div class="dev-center-tutorial-selector sublanding"><a href="/manage/install-and-configure-windows-powershell/" title="PowerShell" class="current">PowerShell</a><a href="/manage/install-and-configure-cli/" title="Azure CLI">Azure CLI</a></div>

##<a name="what-is-azure-powershell"></a>Was ist Azure PowerShell?
Azure PowerShell ist eine Reihe von Modulen, die zum Verwalten von Azure mithilfe von Windows PowerShell-Cmdlets ermöglichen. Sie können die-Cmdlets verwenden, erstellen, testen, bereitstellen und verwalten Solutions und Services der Azure-Plattform bereitgestellt. In den meisten Fällen können die Cmdlets für dieselben Aufgaben wie das Azure-Portal erstellen und Konfigurieren von Clouddiensten, virtuelle Computer, virtuelle Netzwerke und Web-apps verwendet werden.

## <a name="how-versioning-works"></a>Funktionsweise der Versionskontrolle

Azure PowerShell verwendet semantische Versioning, was bedeutet, dass bei Version A > Version B und dann Version A hat die aktuelle APIs. Außerdem bedeutet dies, dass eine oder mehrere Cmdlets in der Hauptversion Bruch ändert.  So ist z. B. Version 1.7.0 Hotfix eine unterbrechende Änderung in Version 1.x von Azure PowerShell an.

Weitere Informationen zu Methoden in Azure PowerShell semantische Versionskontrolle finden Sie die semantische Versioning Spezifikation unter: http://semver.org
 
Verwenden Sie zum Abrufen der aktuellen APIs Version 2.x. Aber wenn Sie Skripte Version 1.x und Sie möchten wichtige Änderung in Version aufnehmen 2.x 2.x [Release Notes](https://github.com/Azure/azure-powershell/blob/dev/documentation/release-notes/migration-guide.2.0.0.md)beschrieben 1.7.0 installieren sollten.

Ein Versionskonflikt kann auftreten, wenn die neueste Version des Profilmoduls installiert und eine frühere Version eines Moduls hängt anschließend geladen. Die einfachste Möglichkeit zur Lösung des Problems ist von der neuesten MSI installiert. Die MSI-Datei bereinigt automatisch ältere Versionen der Module.
 
###<a name="installing-module-versions-side-by-side"></a>Modul-Versionen Side-by-Side installieren

Version 2.1.0 Version 1.2.6 AzureStack sind die ersten Modulversionen installiert werden und Side-by-Side verwendet. Da Azure PowerShell binäre Module verwendet, öffnen Sie ein PowerShell-Fenster und **Import-Module** verwenden, um eine bestimmte Version der AzureRM-Cmdlets importieren:

    Import-Module AzureRM -RequiredVersion 2.1.0**

Versionen vor 2.1.0 (außer 1.2.6) nicht funktionieren gut nebeneinander mit anderen Azure PowerShell-Modul-Versionen. Beim Laden einer früheren Version von Azure PowerShell-Module mit einem Befehl wie oben nicht kompatible Versionen des Moduls **AzureRM.Profile** werden geladen, wodurch Cmdlets werden Sie aufgefordert, melden Sie sich bei jedem Ausführen ein Cmdlet, selbst nachdem Sie sich angemeldet haben.

Am einfachsten ist das Installieren der neuesten Azure PowerShell aus der WebPI beheben feed oder MSI-entfernt die frühere Versionen der Module aus der Gallery installiert. 

Beachten Sie, dass Azure und AzureRM Module Abhängigkeiten, wenn eine Aktualisierung beide Module verwenden, sollten Sie sowohl aktualisieren. Frühere Versionen des Moduls Azure haben das gleiche Problem mit Side-by-Side-Modul laden, frühere Versionen des Moduls AzureRM haben.

<a id="Install"></a>
## <a name="step-1-install"></a>Schritt 1: Installieren

Es folgen zwei Methoden mit denen Azure PowerShell installieren können. Sie können es aus der Galerie PowerShell oder die WebPI.

###<a name="installing-azure-powershell-from-the-powershell-gallery"></a>Installieren von Azure PowerShell von PowerShell-Galerie

Die bevorzugte Methode ist PowerShell Galerie. Sie benötigen PowerShellGet Modul mit der PowerShell-Galerie. Dies ist hier verfügbar: [PowerShellGallery.com](https://www.powershellgallery.com/)

Installieren Sie Azure PowerShell 1.3.0 oder höher von PowerShell Galerie mit einem erweiterten Windows PowerShell oder PowerShell Integrated Scripting Environment (ISE) Prompt mit den folgenden Befehlen:

    # Install the Azure Resource Manager modules from the PowerShell Gallery
    Install-Module AzureRM

    # Install the Azure Service Management module from the PowerShell Gallery
    Install-Module Azure

####<a name="more-about-these-commands"></a>Weitere Informationen zu diesen Befehlen

- **Installation-Modul AzureRM** installiert ein Rollup-Modul für Ressourcenmanager Azure-Cmdlets. Das Modul AzureRM hängt 
- einen bestimmten Versionsbereich für jede Azure-Ressourcen-Manager-Modul. Bereich enthaltene Version wird sichergestellt, dass keine aktuelle Modul Änderungen enthalten sein, wenn AzureRM Module mit derselben Hauptversion installieren. Bei der Installation des Moduls AzureRM wird jede Azure-Ressourcen-Manager-Modul, die zuvor nicht installiert heruntergeladen und aus der Galerie PowerShell installiert. Weitere Informationen zu semantischen Versionskontrolle von Azure PowerShell-Module verwendet finden Sie unter [semver.org](http://semver.org). 
- **Installation-Modul Azure** installiert Azure-Modul. Dieses Modul ist das Service-Modul Azure PowerShell 0.9.x. Dies sollte keinen großen geändert und für die vorherige Version des Moduls Azure austauschbar.

###<a name="installing-azure-powershell-from-webpi"></a>Installieren von Azure PowerShell WebPI

Installieren von Azure PowerShell 1.0 und höher von WebPI ist dasselbe wie für 0.9.x. [Azure PowerShell](http://aka.ms/webpi-azps) herunterladen und die Installation zu starten. Haben Sie Azure PowerShell 0.9.x installiert, Version 0.9.x als Teil der Aktualisierung deinstalliert werden. Wenn Azure PowerShell-Module von PowerShell Gallery installiert entfernt das Installationsprogramm automatisch Module vor der Installation eine konsistente Azure PowerShell-Umgebung sicherzustellen.

> [AZURE.NOTE] Wenn Sie Azure Module aus der Galerie PowerShell installiert, wird das Installationsprogramm automatisch entfernen. Dies verhindert Verwirrung über welche Versionen installiert haben und wo sich diese befinden. PowerShell Galerie Module werden normalerweise im **%ProgramFiles%\WindowsPowerShell\Modules**installiert. Im Gegensatz dazu WebPI wird installiert Azure Module in * *% (x86) ProgramFiles %\Microsoft SDKs\Azure\PowerShell\**. Während der Installation ein Fehler auftritt, manuell entfernen Sie die Azure* Ordner im Ordner **%ProgramFiles%\WindowsPowerShell\Modules** , und versuchen Sie die Installation erneut.

Nach Abschluss der Installation die ```$env:PSModulePath``` Einstellung sollen die Verzeichnisse mit den Azure-PowerShell-Cmdlets.

> [AZURE.NOTE] Es ist ein bekanntes Problem mit PowerShell **$env: PSModulePath** , die bei der Installation von WebPI auftreten können. Wenn Ihr Computer einen Neustart Systemupdates oder anderen Installationen benötigt, kann er Updates **$env: PSModulePath** , nicht den Pfad dem Azure PowerShell installiert ist. In diesem Fall sehen Sie beim Verwenden von Azure PowerShell-Cmdlets nach der Installation oder Aktualisierung "Cmdlet nicht erkannt" angezeigt. In diesem Fall sollte dem Neustart das Problem beheben.

Wenn beim Laden oder Ausführen des Cmdlets, eine Fehlermeldung wie die folgende erhalten:

```
    PS C:\> Get-AzureRmResource
    Get-AzureRmResource : The term 'Get-AzureRmResource' is not recognized as the name of a cmdlet, function,
    script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is
    correct and try again.
    At line:1 char:1
    + Get-AzureRmResource
    + ~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (get-azurermresourcefork:String) [], CommandNotFoundException
        + FullyQualifiedErrorId : CommandNotFoundException
```

Dies kann nach dem Neustart oder importieren die Cmdlets aus C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\ folgendermaßen korrigiert (, wobei XXXX die Version von PowerShell installiert ist:

```
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\azure.psd1"
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\expressroute\expressroute.psd1"
```

## <a name="step-2-start"></a>Schritt 2: Starten
Sie können die Cmdlets von standard Windows PowerShell-Konsole oder von PowerShell Integrated Scripting Environment (ISE) ausführen.
Methode zum Öffnen einer Konsole verwenden hängt von der ausgeführten Windows-Version:

- Auf einem Computer mit mindestens Windows 8 oder Windows Server 2012 können Sie die integrierte Suche. Geben Sie aus **dem Startbildschirm** macht. Dies gibt eine bewertete Liste Apps mit Windows PowerShell. Klicken Sie zum Öffnen der Konsole auf eine app. (Zu app auf dem Bildschirm **zu starten** , klicken Sie auf das Symbol.)

- Verwenden Sie auf einem Computer eine Version vor Windows 8 oder Windows Server 2012 das **Startmenü**. Im **Startmenü** auf **Alle Programme**, **Zubehör**, klicken Sie auf den Ordner **Windows PowerShell** und klicken Sie dann auf **Windows PowerShell**.

Sie können auch **Windows PowerShell ISE** Menübefehle und Tastenkombinationen verwenden, um viele der Aufgaben ausführen wie in der Windows PowerShell-Konsole würde ausführen. Um die ISE in Windows PowerShell-Konsole Cmd.exe oder im **Ausführen** , geben Sie, **powershell_ise.exe**.

###<a name="commands-to-help-you-get-started"></a>Befehle zu beginnen

    # To make sure the Azure PowerShell module is available after you install
    Get-Module –ListAvailable 
    
    # To log in to Azure Resource Manager
    Login-AzureRmAccount

    # You can also use a specific Tenant if you would like a faster log in experience
    # Login-AzureRmAccount -TenantId xxxx

    # To view all subscriptions for your account
    Get-AzureRmSubscription

    # To select a default subscription for your current session
    Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

    # View your current Azure PowerShell session context
    # This session state is only applicable to the current session and will not affect other sessions
    Get-AzureRmContext

    # To select the default storage context for your current session
    Set-AzureRmCurrentStorageAccount –ResourceGroupName “your resource group” –StorageAccountName “your storage account name”

    # View your current Azure PowerShell session context
    # Note: the CurrentStorageAccount is now set in your session context
    Get-AzureRmContext

    # To list all of the blobs in all of your containers in all of your accounts
    Get-AzureRmStorageAccount | Get-AzureStorageContainer | Get-AzureStorageBlob


## <a name="step-3-connect"></a>Schritt 3: Verbinden
Die Cmdlets benötigen Ihr Abonnement sie Ihre Dienste verwalten können. Sie können ein Azure-Abonnement erwerben, haben Sie bereits eine. Eine Anleitung finden Sie unter [Azure erwerben](http://go.microsoft.com/fwlink/p/?LinkId=320795).

1. Typ **Login-AzureRmAccount**

2. Geben Sie die e-Mail-Adresse und das Kennwort für Ihr Konto. Azure authentifiziert und die Anmeldeinformationen speichert und schließt das Fenster.

– ODER –

Melden Sie sich bei Ihrer Arbeit oder Schule Konto:

    $cred = Get-Credential
    Login-AzureRmAccount -Credential $cred
> [AZURE.NOTE] Haben Sie mehr als einen Mandanten zugeordnete Konto Ihrer Organisation geben Sie den Parameter TenantId:

    $loadersubscription = Get-AzureRmSubscription -SubscriptionName $YourSubscriptionName -TenantId $YourAssociatedSubscriptionTenantId


> [AZURE.NOTE] Diese interaktiv anmelden Methode funktioniert nur mit einer Arbeit oder Schule. Ein Geschäfts- oder Konto ist ein Benutzer, der Ihre Arbeit oder Schule werden verwaltet und im Active Directory Azure-Instanz für Ihre Arbeit oder Schule definiert. Wenn Sie derzeit keinen Arbeits- oder Schulcomputer Konto und Microsoft-Konto verwenden an Ihr Azure-Abonnement anmelden, können Sie problemlos mit den folgenden Schritten erstellen.

> 1. Melden Sie das [klassische Azure-Portal an](https://manage.windowsazure.com)und auf **Active Directory**.

> 2. Wenn kein Verzeichnis vorhanden ist, wählen Sie **das Verzeichnis erstellen** und Informationen Sie angeforderten.

> 3. Wählen Sie das Verzeichnis und fügen Sie einen neuen Benutzer hinzu. Dieser neue Benutzer kann mit einem Arbeits- oder Schulcomputer Konto anmelden. Während der Erstellung des Benutzers wird sowohl eine e-Mail-Adresse für den Benutzer und ein temporäres Kennwort angegeben werden. Speichern Sie diese Informationen in Schritt 5 unten verwendet wird.

> 4. Der Azure-Verwaltungsportal **Einstellungen Sie** und wählen Sie dann die **Administratoren**. Wählen Sie **Hinzufügen**und fügen Sie den neuen Benutzer als Co-Administrator hinzu. Dies kann das Konto Arbeits- oder Schulcomputer Azure-Abonnement verwalten.

> 5. Schließlich aus klassischen Azure-Portal anmelden und melden Sie sich mit der Arbeit oder Schule Konto. Ist das erste Mal anmelden mit diesem Konto werden Sie aufgefordert, das Kennwort zu ändern.

> Weitere Informationen zur Anmeldung bei Microsoft Azure mit einer Arbeit oder Schule sehen Sie [für Microsoft Azure Organisation anmelden](./active-directory/sign-up-organization.md).

> Weitere Informationen über Authentifizierung und Abonnement in Azure finden Sie unter [Konten verwalten, Abonnements und Verwaltungsfunktionen](http://go.microsoft.com/fwlink/?LinkId=324796).

### <a name="view-account-and-subscription-details"></a>Einzelheiten zum Konto und Ihr Abonnement

Sie können mehrere Konten und Abonnements für Azure PowerShell verfügbar. Sie können mehrere Konten **Hinzufügen AzureRmAccount** mehr als einmal ausführen hinzufügen.

Um die verfügbaren Azure Konten anzuzeigen, geben Sie **Get-AzureAccount**.

Um Ihre Azure-Abonnements anzuzeigen, geben Sie **Get-AzureRmSubscription**.

##<a id="Help"></a>Hilfe##

Diese Ressourcen helfen bestimmte Cmdlets:


-   In der Konsole können Sie das integrierte Hilfesystem verwenden. Das Cmdlet " **Get-Help** " kann auf diesem System. 

- Hilfe versuchen Sie diese beliebten Foren:

 - [Azure-Forum auf MSDN]( http://go.microsoft.com/fwlink/p/?LinkId=320212)
 - [Stackoverflow](http://go.microsoft.com/fwlink/?LinkId=320213)

##<a name="learn-more"></a>Weitere Informationen


Die folgenden Ressourcen Weitere Informationen zur Verwendung der Cmdlets zu sehen:

Grundlegende Informationen zur Verwendung von Windows PowerShell finden Sie unter [Verwendung von Windows PowerShell](http://go.microsoft.com/fwlink/p/?LinkId=321939).

Referenzinformationen zu den-Cmdlets finden Sie unter [Azure Cmdlet Verweis](https://msdn.microsoft.com/library/windowsazure/jj554330.aspx).

Skripts und Anleitung zu Azure verwalten über Skripts finden Sie im [Script Center](http://go.microsoft.com/fwlink/p/?LinkId=321940).

