<properties
    pageTitle="Azure DevTest Labs FAQ | Microsoft Azure"
    description="Enthält Antworten Sie auf häufig gestellte Fragen Azure DevTest Labs"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="tarcher"/>

# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs FAQ

Dieser Artikel beantwortet einige häufig gestellte Fragen zur Azure DevTest Labs.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="general"></a>Allgemein
- [Was geschieht, wenn meine Frage hier nicht beantwortet?](#what-if-my-question-isnt-answered-here)
- [Warum sollte Azure DevTest Labs verwenden?](#why-should-i-use-azure-devtest-labs) 
- [Was bedeutet "-sorglos, Self-service"?](#what-does-quotworry-free-self-servicequot-mean)
- [Wie kann Azure DevTest Labs verwenden?](#how-can-i-use-azure-devtest-labs) 
- [Wie arbeite Azure DevTest Labs in Rechnung?](#how-am-i-billed-for-azure-devtest-labs) 
 
## <a name="security"></a>Sicherheit 
- [Was sind Sicherheitsstufen in Azure DevTest Labs?](#what-are-the-different-security-levels-in-azure-devtest-labs) 
- [Erstellen eine Rolle Benutzer einen bestimmten Vorgang ausführen können](#how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task) 
 
## <a name="cicd-integration--automation"></a>CI-CD Integration und Automatisierung 
 
- [Werden Azure DevTest Labs ist mit meinem CI-CD Toolchain integriert?](#does-azure-devtest-labs-integrate-with-my-cicd-toolchain) 
 
## <a name="virtual-machines"></a>Virtuelle Computer 
 
- [Warum angezeigt nicht bestimmte VMs Azure Virtual Machines Blatt, die in Azure DevTest Labs angezeigt?](#why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs) 
- [Was ist der Unterschied zwischen benutzerdefinierten und Formeln?](#what-is-the-difference-between-custom-images-and-formulas) 
- [Erstellen mehrere VMs aus derselben Vorlage gleichzeitig?](#how-do-i-create-multiple-vms-from-the-same-template-at-once) 
- [Wie wird verschieben meine vorhandene Azure VMs in meinem Azure Labs DevTest Lab?](#how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab) 
- [Können mehrere Festplatten an meine VMs werden angefügt?](#can-i-attach-multiple-disks-to-my-vms) 
- [Wie automatisieren Sie den Prozess der VHD-Dateien erstellen benutzerdefinierte Bilder hochladen](#how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images) 
- [Wie können die VMs in meinem Lab gelöscht automatisieren?](#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab)
 
## <a name="artifacts"></a>Artefakte 
 
- [Was sind Artefakte?](#what-are-artifacts) 
 
## <a name="lab-configuration"></a>Konfiguration des Testlabors 
 
- [Wie erstelle ich eine Übungseinheit aus einer Vorlage Azure Resource Manager?](#how-do-i-create-a-lab-from-an-azure-resource-manager-template) 
- [Warum werden meine VMs in verschiedenen Ressourcengruppen mit beliebigen Namen erstellt? Kann ich oder ändern diese Ressourcengruppen?](#why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups) 
- [Wie viele Labs können dieselbe Abonnements werden erstellt?](#how-many-labs-can-i-create-under-the-same-subscription)
- [Wie viele VMs können pro Lab werden erstellt?](#how-many-vms-can-i-create-per-lab)
- [Wie Teile ich einen Link an mein Labor?](#how-do-i-share-a-direct-link-to-my-lab)
- [Was ist ein Microsoft-Konto?](#what-is-a-microsoft-account)
 
## <a name="troubleshooting"></a>Problembehandlung 
 
- [Meine Artefakt Fehler beim virtuellen Computer erstellen. Wie behebe ich sie?](#my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it) 
- [Warum Netzwerk ist nicht vorhandenes virtuelles ordnungsgemäß speichern?](#why-isnt-my-existing-virtual-network-saving-properly)  

### <a name="what-if-my-question-isnt-answered-here"></a>Was geschieht, wenn meine Frage hier nicht beantwortet?
Wenn Ihre Frage hier nicht aufgeführt ist, lassen Sie uns wissen, und wir helfen Ihnen eine Antwort.

- Frage im [Disqus-Thread](#comments) am Ende dieser FAQ und mit Azure Cache-Team und anderen Mitgliedern zu diesem Artikel.
- Um ein breiteres Publikum erreichen Frage [Azure DevTest Labs MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs)und mit Azure DevTest Labs-Team und anderen Mitgliedern der Community.
- So ein Feature anfordern, Ihre Anfragen und Ideen [Azure DevTest Labs Benutzer Stimme](https://feedback.azure.com/forums/320373-azure-devtest-labs).

### <a name="why-should-i-use-azure-devtest-labs"></a>Warum sollte Azure DevTest Labs verwenden? 
Azure DevTest Labs kann Ihr Team Zeit und Geld sparen. Entwickler können eigene Umgebungen mit mehreren anderen Basen und Artefakte mit schnell bereitstellen und Konfigurieren von Anwendung. Mit benutzerdefinierten Formeln, virtueller Maschinen als Vorlagen gespeichert und problemlos reproduziert. Labs bieten außerdem mehrere konfigurierbare Policies, die Administratoren Lab und eine Team-Umgebung verwalten. Diese Richtlinien schließen Auto-Shutdown, Kosten Schwellenwert Maximale VMs pro Benutzer und maximale VM-Größen. Eine Erläuterung der Azure DevTest Labs lesen Sie die [Übersicht](devtest-lab-overview.md) oder [Einführungsvideo](/documentation/videos/videos/what-is-azure-devtest-labs)Auschecken. 

### <a name="what-does-worry-free-self-service-mean"></a>Was bedeutet "-sorglos, Self-service"?
"Sorglos, Self-Service" bedeutet, dass Entwickler und Tester ihrer eigenen Umgebung erstellen nach Bedarf und Administratoren die Sicherheit, dass Azure DevTest Labs minimiert Abfälle und kontrollieren Sie Kosten. Administratoren können die VM-Größen zulässig sind, die maximale Anzahl der VMs und wenn VMs gestartet und beendet. Azure DevTest Labs erleichtert auch die Kosten zu überwachen und Alarme wissen wie Ressourcen in der Übungseinheit verwendet werden. 

### <a name="how-can-i-use-azure-devtest-labs"></a>Wie kann Azure DevTest Labs verwenden? 
Azure DevTest Labs empfiehlt Sie Entwickler oder Umgebung testen und Sie möchten schnell reproduzieren oder mit Kostenersparnis Richtlinien verwalten. 

Hier sind einige Szenarien, mit denen unsere Kunden Azure DevTest Labs für: 

- Entwicklungs- und Umgebung zentral verwalten, erstellt das Team mit Richtlinien, um Kosten und benutzerdefinierte Bilder freigeben.
- Entwicklung einer Anwendung mit benutzerdefinierten Datenträger in den Entwicklungsphasen speichert.
- Die Kosten im Zusammenhang mit Fortschritt nachverfolgen. 
- Testumgebung für Qualitätssicherungstests Masse erstellen.
- Verwenden Artefakte und Formeln einfach konfigurieren und auf verschiedenen Umgebung reproduzieren. 
- Verteilen von VMs für Hackathons (Dev oder Test Zusammenarbeit) und dann problemlos entziehen sie das Ereignis endet. 

### <a name="how-am-i-billed-for-azure-devtest-labs"></a>Wie arbeite Azure DevTest Labs in Rechnung? 
Azure DevTest Labs ist kostenlos, Labs erstellen und Konfigurieren von Richtlinien, Vorlagen und Artefakten ist. Sie Zahlen nur für Azure Ressourcen innerhalb der Übungseinheiten virtuelle Computer Speicherkonten und virtuelle Netzwerke verwendet. Weitere Informationen über die Kosten von Lab-Ressourcen finden Sie [Azure DevTest Labs Preise](https://azure.microsoft.com/pricing/details/devtest-lab/). 

### <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Was sind Sicherheitsstufen in Azure DevTest Labs?  
[(Azure Role-Based Access Control, RBAC)](../active-directory/role-based-access-built-in-roles.md)Zugriff bestimmt. Zum Verständnis der Funktionsweise von Access hilft es, die Unterschiede zwischen eine Berechtigung einer Rolle und einen Bereich festgelegten RBAC kennen.

- **Berechtigung** - Berechtigung ist definierten Zugriff auf eine bestimmte Aktion. Beispielsweise könnte eine Berechtigung Lesezugriff auf alle virtuellen Maschinen. 
- **Rolle** – eine Rolle ist eine Gruppe von Berechtigungen, die gruppiert und einem Benutzer zugewiesen werden können. Beispielsweise ist "Abonnementbesitzer" Zugriff auf alle Ressourcen in einem Abonnement. 
- **Bereich** : ein Bereich ist in der Hierarchie von Azure-Ressource. Ein Bereich kann z. B. eine Ressourcengruppe oder ein einzelnes Labor oder das gesamte Abonnement sein. 
 
Im Rahmen von Azure DevTest Labs sind zwei Arten von Rollen Berechtigungen definieren: Lab Besitzer und Lab-Benutzer.

- **Lab-Besitzer** - Lab Besitzer hat Zugriff auf Ressourcen in der Übungseinheit. Daher können sie Richtlinien ändern, lesen und Schreiben von VMs, ändern das virtuelle Netzwerk, usw. 
- **Lab-Benutzer** - Lab Benutzer sehen alle Lab-Ressourcen wie VMs, Richtlinien und virtuelle Netzwerke jedoch Richtlinien oder von anderen Benutzern erstellte VMs nicht ändern. Kann auch zum Erstellen von benutzerdefinierter Rollen in Azure DevTest Labs, und erfahren Sie, wie im Artikel [Benutzerberechtigungen zu bestimmten labrichtlinien gewähren](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). 
 
Da Bereiche ein Benutzer in einem bestimmten Bereich ist hierarchisch aufgebaut sind, erhalten sie automatisch diese Berechtigungen an alle untergeordneten Bereich umfasst. Beispielsweise haben ein Benutzer zu Ihrem Besitzer zugewiesen wird, dann sie Zugriff auf alle Ressourcen in einem Abonnement. Diese Ressourcen umfassen alle virtuellen Computer, alle virtuellen Netzwerke und alle Übungseinheiten. Daher erbt eine Abonnementbesitzer automatisch die Rolle des Lab-Besitzer. Allerdings gilt das Gegenteil nicht. Lab-Besitzer verfügt über einen unteren als Abonnement-Level Bereich Labor. Daher kann Lab Besitzer nicht virtuellen Maschinen oder virtuelle Netzwerke oder alle Ressourcen außerhalb des Labors. 

### <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Erstellen eine Rolle Benutzer einen bestimmten Vorgang ausführen können
Ein umfassender Artikel über benutzerdefinierte Rollen erstellen und dieser Rolle Berechtigungen zuweisen finden Sie hier. Hier ist ein Beispiel für ein Skript, das die Rolle "DevTest Labs erweiterte Benutzer" die Berechtigung zum Starten und beenden Sie alle virtuellen Computer in der Übungseinheit erstellt:
 
    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User" 
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*') 
    $policyRoleDef.Id = $null 
    $policyRoleDef.Name = "DevTest Labs Advance User" 
    $policyRoleDef.IsCustom = $true 
    $policyRoleDef.AssignableScopes.Clear() 
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action") 
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  
 
### <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Werden Azure DevTest Labs ist mit meinem CI-CD Toolchain integriert? 
Bei Verwendung von VSTS ist eine [Erweiterung von Azure DevTest Labs Aufgaben](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) , die Ihre Version Pipeline in Azure DevTest Labs automatisieren können. Mithilfe dieser Erweiterung gehören:

- Erstellen und Bereitstellen eines virtuellen Computers automatisch mit dem aktuellen Build mit Azure kopieren oder PowerShell VSTS Aufgaben konfigurieren. 
- Automatische Erfassung den Zustand eines virtuellen Computers nach dem Testen zum Reproduzieren eines Fehlers auf dem gleichen virtuellen Computer näher untersucht. 
- VM am Ende der Pipeline Version wird gelöscht, wenn nicht mehr benötigt. 

Die folgenden Blogeinträge bieten Anleitung und Informationen über VSTS-Erweiterung:
 
- [Azure DevTest Labs VSTS-Erweiterung](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/) 
- [Bereitstellen einer neuen VM in eine vorhandene AzureDevTestLab von VSTS](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS) 
- [Mit VSTS Releasemanagement für kontinuierlichen Bereitstellung für AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs) 
 
Für andere Toolchains CI-CD werden die zuvor beschriebenen Szenarien durch VSTS Aufgaben Erweiterung möglich entsprechend durch Bereitstellen von [Azure-Ressourcen-Manager Vorlagen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) [Azure PowerShell-Cmdlets](../resource-group-template-deploy.md) mit [.NET SDKs](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/)erreicht. [REST-APIs für DevTest Labs](http://aka.ms/dtlrestapis) können Sie Ihre Toolchain integrieren.  

### <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Warum angezeigt nicht bestimmte VMs Azure Virtual Machines Blatt, die in Azure DevTest Labs angezeigt?
Beim Erstellen eine VM in Azure DevTest Labs, erhält die Berechtigung, VM zugreifen. Sie können sowohl in Übungseinheiten Blade- und Blade- **VMs** . Benutzer in der Rolle DevTest Labs sehen alle virtuellen Computer in der Übungseinheit durch die Übungseinheit **alle virtuellen Computer** Blade erstellt. Jedoch sind Benutzer in der Rolle DevTest Labs nicht automatisch Lesezugriff auf VM-Ressourcen gewährt, die andere erstellt haben. Daher werden die VMs Blatt **virtuelle Computer** nicht angezeigt. 

### <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Was ist der Unterschied zwischen benutzerdefinierten und Formeln? 
Ein benutzerdefiniertes Bild ist eine virtuelle Festplatte (virtual Hard Disk) eine Formel ein Bild, das Sie Berechtigungen konfigurieren können, die Sie speichern und reproduzieren können. Ein benutzerdefiniertes Bild möglicherweise vorzuziehen, wenn Sie schnell mehrere Umgebungen mit dasselbe grundlegende, unveränderliche Bild erstellen möchten. Eine Formel empfiehlt sich möglicherweise möchten Sie die Konfiguration der VM mit neuesten Bits, einem virtuellen Netzwerksubnet oder einer bestimmten Größe zu reproduzieren. Eine ausführlichere Erklärung finden Sie im Artikel [Vergleichen benutzerdefinierte Bilder und Formeln in DevTest Labs](devtest-lab-comparing-vm-base-image-types.md). 
 
### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Erstellen mehrere VMs aus derselben Vorlage gleichzeitig? 
[VSTS Aufgaben Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) oder [ein Ressourcenmanager Azure Vorlage](devtest-lab-add-vm-with-artifacts.md#save-arm-template) können Sie beim Erstellen einer VM und [Bereitstellen die Vorlage Azure-Ressourcen-Manager von Windows PowerShell](../resource-group-template-deploy.md). 
 
### <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Wie wird verschieben meine vorhandene Azure VMs in meinem Azure Labs DevTest Lab? 
Entwerfen wir eine Lösung direkt VMs in Azure DevTest Labs verschieben, aber derzeit vorhandenen VMs in kopieren Azure Labs DevTest wie folgt: 

1. Kopieren Sie die VHD-Datei Ihrer vorhandenen VM dieses [Windows PowerShell-Skript](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1) 
1. [Erstellen des Abbilds](devtest-lab-create-template.md) in Azure Labs DevTest Lab. 
1. Erstellen Sie einen virtuellen Computer im Labor aus dem benutzerdefinierten Abbild 
 
### <a name="can-i-attach-multiple-disks-to-my-vms"></a>Können mehrere Festplatten an meine VMs werden angefügt? 
Hinzufügen mehrerer Festplatten zu VMs wird unterstützt.  
 
### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Wie automatisieren Sie den Prozess der VHD-Dateien erstellen benutzerdefinierte Bilder hochladen 
Es gibt zwei Optionen:

- [Azure AzCopy](../storage/storage-use-azcopy.md#blob-upload) kann verwendet werden, zu kopieren oder in der Übungseinheit zugeordnete Speicherkonto VHD-Dateien hochladen.
- [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) ist eine eigenständige Anwendung, die auf Windows OSX und Linux läuft.   
 
Lab zugeordnete Speicherkonto finden, gehen Sie folgendermaßen vor:

1. Mit der [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)anmelden. 
1. Der linke Bereich **Ressourcengruppen** auswählen. 
1. Suchen Sie und wählen Sie die Ressourcengruppe Lab zugeordnet. 
1. **Übersicht über** Blade wählen Sie eine Storage-Konten. 
1. **Blobs**auswählen
1. Uploads in der Liste suchen. Falls nicht vorhanden, zurück zu Schritt 4, und versuchen Sie eine andere Speicherkonto.
1. Verwenden Sie die **URL** als Ziel im Befehl AzCopy.


### <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Wie können die VMs in meinem Lab gelöscht automatisieren?

Neben VMs aus der Übungseinheit in Azure-Portal löschen, können Sie alle virtuellen Computer im Lab mithilfe eines PowerShell-Skripts löschen. Im folgenden Beispiel ändern Sie einfach die Parameterwerte unter Kommentar **Werte ändern** . Rufen Sie die `subscriptionId`, `labResourceGroup`, und `labName` Werte aus dem Lab-Blade in Azure-Portal. 


    # Delete all the VMs in a lab
    
    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount
    
    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    
    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object { 
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}
    
    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }




### <a name="what-are-artifacts"></a>Was sind Artefakte? 
Artefakte sind benutzerdefinierbare Elemente der neuesten Bits oder die Entwicklertools auf einem virtuellen Computer bereitstellen. Sie werden Ihre VM während der Erstellung mit ein paar einfachen Klicks, und nach der Bereitstellung der VM Artefakte bereitstellen und Konfigurieren der VM. In unserer [öffentlichen Github Repository](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts)vorhandenen Artefakte gibt, aber Sie können auch [Eigene Elemente erstellen](devtest-lab-artifact-author.md). 

### <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Wie erstelle ich eine Übungseinheit aus einer Vorlage Azure Resource Manager? 
Wir haben ein [Github Repository für Lab-Vorlagen, Azure-Ressourcen-Manager](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) , das Bereitstellen als angegeben- oder zum Erstellen von benutzerdefinierter Vorlagen für die Übungseinheiten. Jeder dieser Vorlagen wurde eine Verknüpfung können zum Bereitstellen des Labors-eigene Azure-Abonnement wird oder der Vorlage und [mithilfe von PowerShell oder Azure CLI](../resource-group-template-deploy.md).
 
### <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Warum werden meine VMs in verschiedenen Ressourcengruppen mit beliebigen Namen erstellt? Kann ich oder ändern diese Ressourcengruppen? 
Ressourcengruppen sind damit Azure DevTest Labs zum Verwalten von Berechtigungen und Zugriff auf virtuelle Maschinen auf diese Weise erstellt. Während Sie die VM mit dem gewünschten Namen in eine andere Ressourcengruppe verschieben können, ist dies daher nicht empfohlen. Wir arbeiten auf diese um mehr Flexibilität zu verbessern.   
 
### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Wie viele Labs können dieselbe Abonnements werden erstellt? 
Gibt es keine bestimmte Beschränkung für Anzahl Labs pro Abonnement erstellt werden kann. Allerdings werden die pro Abonnement. Sie können [auferlegten Grenzen und Kontingente Azure-Abonnements](../azure-subscription-service-limits.md) und [wie diese Grenzwerte zu](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests)lesen. 
 
### <a name="how-many-vms-can-i-create-per-lab"></a>Wie viele VMs können pro Lab werden erstellt? 
Gibt es keine bestimmte Beschränkung für Anzahl VMs pro Lab erstellt werden können. Das Labor unterstützt derzeit nur 40 VMs gleichzeitig im Standardspeicher ausführen und 25 VMs in Premium-Speicher ausgeführt. Wir arbeiten derzeit an diese Grenzwerte erhöhen. 

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Wie Teile ich einen Link an mein Labor?

Um einen Link für die Laborbenutzer freigeben können Sie das folgende Verfahren ausführen.

1. Wechseln Sie zu der Übungseinheit in Azure-Portal.
2. Lab-URL aus Ihrem Browser kopieren und die Laborbenutzer freigeben. 

>[AZURE.NOTE] Wenn Laborbenutzer externe Benutzer mit einem [Konto MSA](#what-is-a-microsoft-account) und in Active Directory des Unternehmens gehören, erhalten sie Fehler beim Navigieren zu der angegebenen Verknüpfung. Wenn sie eine Fehlermeldung erhalten, weisen sie ihren Namen in der oberen rechten Ecke der Azure-Portal auf und wählen Sie das Verzeichnis die Übungseinheit im Abschnitt **Directory** im Menü besteht.

### <a name="what-is-a-microsoft-account"></a>Was ist ein Microsoft-Konto?

Ein Microsoft-Konto wird für fast alle mit Microsoft-Geräte und Dienste. Es ist eine e-Mail-Adresse und Kennwort Skype, Outlook.com, OneDrive, Windows Phone und Xbox LIVE-Anmeldung bei und also Dateien, Fotos, Kontakten und Einstellungen auf alle Geräte folgen können. 

>[AZURE.NOTE] Microsoft-Konto zum "Windows Live ID" bezeichnet werden.
 
### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Meine Artefakt Fehler beim virtuellen Computer erstellen. Wie behebe ich sie? 
Anhand Blogbeitrag [Problembehandlung Fehler Artefakte im AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) - eines unseren MVPs geschrieben - um Protokolle zur fehlgeschlagenen Artefakt zu lernen. 
 
### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Warum Netzwerk ist nicht vorhandenes virtuelles ordnungsgemäß speichern?  
Eine Möglichkeit ist, dass Ihre virtuellen Netzwerknamen Punkte enthält. In diesem Fall sollten die Punkte entfernen oder durch Bindestriche ersetzt und versuchen Sie erneut, das virtuelle Netzwerk zu speichern.
