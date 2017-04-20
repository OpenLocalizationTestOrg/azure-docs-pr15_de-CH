<properties
   pageTitle="Problembehandlung bei Fehlermeldungen Azure-Bereitstellung | Microsoft Azure"
   description="Beschreibt die allgemeine Fehlerbehebung beim Bereitstellen von Ressourcen in Azure Azure-Ressourcen-Manager verwenden."
   services="azure-resource-manager"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"
   keywords="Fehler bei der Bereitstellung, Azure-Bereitstellung in Azure bereitstellen"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Problembehandlung bei Fehlermeldungen Azure-Bereitstellung mit Azure-Ressourcen-Manager

Beschrieben Lösungsvorschlägen häufigen Azure-Bereitstellungsfehler auftreten. Benötigen Sie weitere Informationen zum Fehler bei der Bereitstellung, [Bereitstellung Ansichtsoperationen](resource-manager-troubleshoot-deployments-portal.md) angezeigt und dann wieder zu diesem Artikel für Hilfe bei der Behebung des Fehlers. In den Abschnitten in diesem Thema aufgeführt Fehlercode angezeigt.

## <a name="invalid-template"></a>Ungültige Vorlage

Dieser Fehler kann verschiedene Arten von Fehlern führen. 

### <a name="syntax-error"></a>Syntaxfehler:

Wenn Sie eine Fehlermeldung, die die Bestätigung der Vorlage fehlgeschlagen angibt erhalten, müssen Sie kein Syntax in der Vorlage. 

    Code=InvalidTemplate 
    Message=Deployment template validation failed

Dieser Fehler ist einfach da Vorlagenausdrücke kompliziert sein können. Beispielsweise enthält die folgenden Namen Zuordnung ein Speicherkonto Klammern, drei Funktionen drei Klammernpaar, einen Satz von Anführungszeichen und eine Eigenschaft:

    "name": "[concat('storage', uniqueString(resourceGroup().id))]",

Wenn Sie nicht die entsprechende Syntax angeben, erstellt die Vorlage einen Wert, der die Absicht unterscheidet.

Wenn Sie diese Art von Fehler erhalten, überprüfen Sie sorgfältig die Ausdruckssyntax. Erwägen Sie eine JSON-Editor wie [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) oder [Visual Studio Code](resource-manager-vs-code.md)Syntaxfehler gewarnt werden können. 

### <a name="incorrect-segment-lengths"></a>Falsche Segmentlängen

Eine andere unzulässige Fehler tritt bei der Ressourcennamen nicht im richtigen Format ist.

    Code=InvalidTemplate
    Message=Deployment template validation failed: 'The template resource {resource-name}' 
    for type {resource-type} has incorrect segment lengths.

Ressource für einen Stamm muss ein kleiner Abschnitt den Namen als in den Ressourcentyp. Jedes Segment wird durch einen Schrägstrich unterschieden. Im folgenden Beispiel hat zwei Segmente und der Name ist ein Segment einen **gültigen Namen**.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "myHostingPlanName",
      ...
    }

Aber das nächste Beispiel ist **kein gültiger Name** , da er dieselbe Anzahl Segmente als Typ besitzt.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "appPlan/myHostingPlanName",
      ...
    }

Für untergeordnete Ressourcen haben Typ und Namen die gleiche Anzahl von Segmenten. Diese Anzahl von Segmenten ist sinnvoll, da den vollständigen Namen und Typ des untergeordneten umfasst übergeordneter Name und Typ. Daher hat der vollständige Name noch weniger ein Segment als vollständigen Typ. 

    "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "contosokeyvault",
            ...
            "resources": [
                {
                    "type": "secrets",
                    "name": "appPassword",
                    ...
                }
            ]
        }
    ]

Richtige Segmente möglich mit Ressourcen-Manager-Typen, die auf Ressourcen angewendet werden. Anwenden einer Ressource Sperren einer Website erfordert beispielsweise ein mit vier Segmenten. Daher heißt der Segmente:

    {
        "type": "Microsoft.Web/sites/providers/locks",
        "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
        ...
    }

### <a name="copy-index-is-not-expected"></a>Kopie-Index wird nicht erwartet.

Auftreten dieses **InvalidTemplate** **Copy** -Element zu einem Teil der Vorlage angewendet haben, das dieses Element unterstützt. Sie können nur einen Ressourcentyp Copy-Element zuweisen. Sie können keine Kopie p einen Ressourcentyp anwenden. Z. B. Kopie einer virtuellen Maschine angewendet, aber Sie können Betriebssystem-Datenträger für einen virtuellen Computer zuweisen. In einigen Fällen können Sie eine übergeordnete Ressource zum Erstellen einer kopierschleife untergeordnete Ressource konvertieren. Weitere Informationen zum Verwenden der Kopie finden Sie unter [mehrere Instanzen von Ressourcen in Azure-Ressourcen-Manager erstellen](resource-group-create-multiple.md).

### <a name="parameter-is-not-valid"></a>Der Parameter ist ungültig

Wenn Vorlage gibt die zulässigen Werte für einen Parameter einen Wert, der nicht diese Werte angeben, erhalten Sie eine Meldung ähnlich der folgenden Fehler:

    Code=InvalidTemplate; 
    Message=Deployment template validation failed: 'The provided value {parameter vaule} 
    for the template parameter {parameter name} is not valid. The parameter value is not 
    part of the allowed value(s)

Überprüfen Sie die zulässigen Werte in der Vorlage, und während der Bereitstellung bieten.

## <a name="not-found-or-resource-not-found"></a>Nicht gefunden (oder Ressource nicht gefunden)

Wenn die Vorlage den Namen der Ressource, die aufgelöst werden kann enthält, wird eine ähnliche Fehlermeldung:

    Code=NotFound;
    Message=Cannot find ServerFarm with name exampleplan.

Wenn Sie die fehlende Ressource in der Vorlage bereitstellen möchten, überprüfen Sie, ob Sie eine Abhängigkeit hinzufügen möchten. Ressourcenmanager optimiert Bereitstellung von Ressourcen möglichst gleichzeitig erstellen. Wenn eine Ressource nach einer anderen Ressource bereitgestellt werden muss, müssen Sie **DependsOn** -Element in der Vorlage verwenden, um eine Abhängigkeit für die Ressource zu erstellen. Wenn eine Webanwendung bereitstellen, muss z. B. App Service-Plan vorhanden. Wenn Sie nicht angegeben haben App Serviceplan Web app abhängt, erstellt Ressourcenmanager beide Ressourcen gleichzeitig. Sie erhalten eine Fehlermeldung besagt, dass die App Service Plan-Ressource nicht gefunden werden kann, da es noch nicht gibt beim Festlegen einer Eigenschaft für die Web-app. Sie können diesen Fehler verhindern, durch Festlegen der Abhängigkeit im Web app.

    {
      "apiVersion": "2015-08-01",
      "type": "Microsoft.Web/sites",
      ...
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      ...
    }
 
Dieser Fehler wird auch angezeigt, wenn vorhanden ist die Ressource in einer anderen Ressourcengruppe als bereitgestellt wird. In diesem Fall [ResourceId Funktion](resource-group-template-functions.md#resourceid) der vollqualifizierte Name der Ressource zu verwenden.

    "properties": {
        "name": "[parameters('siteName')]",
        "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
    } 

Wenn Sie versuchen, die Funktionen [Verweis](resource-group-template-functions.md#referenc) oder [ListKeys](resource-group-template-functions.md#listkeys) mit einer Ressource verwenden, die nicht aufgelöst werden kann, erhalten Sie folgenden Fehler:

    Code=ResourceNotFound;
    Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource 
    group {resource group name} was not found.

Suchen Sie ein Ausdruck, der den **Verweis** enthält. Überprüfen Sie, dass die Werte der Parameter richtig sind.

## <a name="storage-account-already-exists-or-already-taken"></a>Konto bereits vorhanden ist (oder bereits)

Für Speicherkonten erforderlich einen Namen für die Ressource, die Azure eindeutig ist. Wenn Sie keinen eindeutigen Namen angeben, erhalten Sie eine Fehlermeldung wie:

    Code=StorageAccountAlreadyTaken 
    Message=The storage account named mystorage is already taken.

Verkettet die Namenskonvention mit dem Ergebnis der Funktion [UniqueString](resource-group-template-functions.md#uniquestring) , um einen eindeutigen Namen zu erstellen.

    "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]",
    "type": "Microsoft.Storage/storageAccounts",

Wenn Sie Ihr Abonnement ein Speicherkonto mit demselben Namen wie ein vorhandenes Speicherkonto bereitstellen, aber einen anderen Speicherort an, erhalten Sie eine Fehlermeldung, dass das Speicherkonto bereits an einem anderen Speicherort vorhanden ist. Storage-Konto löschen oder am gleichen Speicherort wie Storage-Konto bereitstellen.

## <a name="account-name-invalid"></a>Ungültiger Kontoname

Fehlermeldung **AccountNameInvalid** beim Versuch, ein Speicherkonto benennen, die enthält Zeichen unzulässigen. Speicher muss zwischen 3 und 24 Zeichen und Ziffern und nur Kleinbuchstaben.

## <a name="no-registered-provider-found"></a>Keine registrierten Anbieter gefunden

Wenn Sie Ressourcen bereitstellen, erhalten Sie folgenden Fehlercode und Fehlermeldung:

    Code: NoRegisteredProviderFound
    Message: No registered resource provider found for location {ocation} 
    and API version {api-version} for type {resource-type}.

Dieser Fehler wird aus drei Gründen:

1. Speicherort für den Ressourcentyp nicht unterstützt
2. API-Version für den Ressourcentyp nicht unterstützt
3. Der Ressourcenanbieter wurde nicht für Ihr Abonnement registriert

Die Fehlermeldung erhalten Sie Vorschläge für die unterstützten Speicherorte und API-Versionen. Sie können die Vorlage eines vorgeschlagenen Werte ändern. Die meisten Anbieter werden automatisch von Azure-Portal oder Befehlszeilen-Schnittstelle verwendetem erfasst, aber nicht alle. Wenn einen bestimmte Ressourcenanbieter vor nicht verwendet haben, müssen Sie diesen Anbieter registrieren. Sie können Informationen über PowerShell oder Azure CLI Ressourcenprovider ermitteln.

### <a name="powershell"></a>PowerShell

Zu Ihrem Registrierungsstatus **Get-AzureRmResourceProvider**verwenden.

    Get-AzureRmResourceProvider -ListAvailable

Einen Anbieter registrieren, **Registrieren AzureRmResourceProvider** und geben Sie den Namen des Ressourcenanbieters registrieren möchten.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn

Zum Abrufen der unterstützten Speicherorte für einen bestimmten Ressourcentyp verwenden:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

Verwenden Sie zum Abrufen von unterstützten API-Versionen für einen bestimmten Typ von Ressource

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

### <a name="azure-cli"></a>Azure CLI

Verwenden, um festzustellen, ob der Dienstleistungserbringer die `azure provider list` Befehl.

    azure provider list

Registrieren Sie einen Ressourcenanbieter verwenden die `azure provider register` Befehl, und geben Sie den *Namespace* registrieren.

    azure provider register Microsoft.Cdn

Verwenden der unterstützten Speicherorte und API-Versionen für ein Ressourcenanbieter finden:

    azure provider show -n Microsoft.Compute --json > compute.json

## <a name="operation-not-allowed"></a>Nicht zulässig

Sie haben möglicherweise Probleme, wenn Bereitstellung ein Kontingent überschreitet die Ressourcengruppe, Abonnements, Konten und anderen Bereichen. Beispielsweise kann Ihr Abonnement konfiguriert werden die Anzahl der Kerne für eine Region. Wenn Sie versuchen, einen virtuellen Computer mit mehr als die zulässigen Betrag Kernen bereitstellen, erhalten Sie eine Fehlermeldung, dass das Kontingent überschritten wurde.
Vollständige Kontingentinformationen finden Sie unter [Azure-Abonnement und Service Grenzen, Kontingente und Einschränkungen](azure-subscription-service-limits.md).

Untersuchen Sie Ihr Abonnement Kontingente für Kerne können Sie die `azure vm list-usage` Befehl in der Azure-CLI. Das folgende Beispiel ist der zentrale Vorgabe für ein kostenloses Testabo 4:

    azure vm list-usage

Die zurückgibt:

    info:    Executing command vm list-usage
    Location: westus
    data:    Name   Unit   CurrentValue  Limit
    data:    -----  -----  ------------  -----
    data:    Cores  Count  0             4
    info:    vm list-usage command OK

Wenn Sie eine Vorlage, die mehr als vier Kerne in der Region West USA erstellt bereitstellen, erhalten Sie einen Bereitstellungsfehler aussieht:

    Code=OperationNotAllowed
    Message=Operation results in exceeding quota limits of Core. 
    Maximum allowed: 4, Current in use: 4, Additional requested: 2.

Oder in PowerShell verwenden Sie das Cmdlet " **Get-AzureRmVMUsage** ".

    Get-AzureRmVMUsage

Die zurückgibt:

    ...
    CurrentValue : 0
    Limit        : 4
    Name         : {
                     "value": "cores",
                     "localizedValue": "Total Regional Cores"
                   }
    Unit         : null
    ...

In diesen Fällen sollte zum Portal und Datei eine Problems Ihr Kontingent für den Bereich ausgelöst, in die Sie bereitstellen möchten.

> [AZURE.NOTE] Beachten Sie, dass für Ressourcengruppen, die Quote für jede einzelne Region nicht für das gesamte Abonnement. Benötigen Sie 30 Kerne im Westen der USA bereitstellen, haben Sie 30 Ressourcenmanager Kerne im Westen der USA erfragen. Benötigen Sie 30 Kernen Bereitstellen in denen Bereiche, die Sie Zugriff haben, Fragen Sie für 30 Ressourcenmanager Kerne in allen Regionen.

## <a name="invalid-content-link"></a>Ungültige Content link

Wenn Sie die Fehlermeldung:

    Code=InvalidContentLink
    Message=Unable to download deployment content from ...

Sie haben wahrscheinlich versucht an eine verschachtelte Vorlage verknüpfen, die nicht verfügbar ist. Überprüfen Sie den URI für die verschachtelte Vorlage bereitgestellt. Wenn die Vorlage in ein Speicherkonto vorhanden ist, unbedingt zugänglich ist. Sie müssen ein SAS-Token übergeben. Weitere Informationen finden Sie in der [verknüpften Vorlagen mit Azure-Ressourcen-Manager](resource-group-linked-templates.md).

## <a name="authorization-failed"></a>Autorisierung fehlgeschlagen

Fehler bei der Bereitstellung möglicherweise weil Konto oder Service principal versucht, die Ressourcen für diese Aktionen keinen Zugriff. Azure Active Directory können Sie oder Ihr Administrator steuern, welche Identitäten Zugriff auf welche Ressourcen mit großer Genauigkeit. Beispielsweise wenn Ihr Konto die Rolle Leser zugewiesen ist, können Sie keine Ressourcen erstellt. In diesem Fall sehen Sie eine Fehlermeldung, dass die Autorisierung fehlgeschlagen ist.

Weitere Informationen über rollenbasierte Zugriffskontrolle Siehe [Azure Role-Based zugreifen](./active-directory/role-based-access-control-configure.md).

Neben rollenbasierte Zugriffskontrolle können Ihre Aktionen Bereitstellung von Richtlinien für das Abonnement beschränkt. Über Richtlinien kann Administrator Konventionen für alle Ressourcen im Abonnement bereitgestellt erzwingen. Ein Administrator kann z. B. erfordern, dass ein bestimmtes Tag-Wert für einen Ressourcentyp bereitgestellt wird. Wenn Sie die Anforderungen nicht erfüllen, erhalten Sie Fehler bei der Bereitstellung. Weitere Informationen zu Richtlinien finden Sie unter [Verwenden von Richtlinien zum Verwalten von Ressourcen und Zugriffskontrolle](resource-manager-policy.md).

## <a name="troubleshooting-virtual-machines"></a>Problembehandlung bei virtuellen Computern

| Fehler | Artikel |
| -------- | ----------- |
| Benutzerdefinierte Erweiterung Skriptfehler | [Windows-VM-Erweiterung-Fehler](./virtual-machines/virtual-machines-windows-extensions-troubleshoot.md)<br />oder<br />[Linux VM-Erweiterung-Fehler](./virtual-machines/virtual-machines-linux-extensions-troubleshoot.md) |
| Betriebssystemabbild Bereitstellung Fehler | [Neue Windows-VM-Fehler](./virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md)<br />oder<br />[Neue Linux VM-Fehler](./virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md ) |
| Zuordnungsfehler | [Windows-VM Zuordnungsfehler](./virtual-machines/virtual-machines-windows-allocation-failure.md)<br />oder<br />[Linux VM Zuordnungsfehler](./virtual-machines/virtual-machines-linux-allocation-failure.md) |
| Secure Shell (SSH)-Fehler beim Herstellen einer Verbindung | [Shell Verbindung mit Linux VM](./virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md) |
| Anwendung auf VM mit Fehlern | [Anwendung auf Windows-VM](./virtual-machines/virtual-machines-windows-troubleshoot-app-connection.md)<br />oder<br />[Anwendung auf Linux VM](./virtual-machines/virtual-machines-linux-troubleshoot-app-connection.md) |
| Remotedesktopverbindung Fehler | [Remotedesktopverbindungen mit Windows VM](./virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md) |
| Fehler durch erneutes behoben | [Virtual Machine neue Azure Knoten erneut](./virtual-machines/virtual-machines-windows-redeploy-to-new-node.md) |
| Fehler in Cloud-Dienst | [Cloud-Dienst Bereitstellungsprobleme](./cloud-services/cloud-services-troubleshoot-deployment-problems.md) |

## <a name="troubleshooting-other-services"></a>Problembehandlung bei anderen Diensten

In der folgenden Tabelle ist keine vollständige Liste der Problembehandlung für Azure. Stattdessen konzentriert sich auf Probleme im Zusammenhang mit der Bereitstellung oder Konfiguration von Ressourcen. Benötigen Sie Hilfe zur Problembehandlung für Fragen mit einer Ressource finden Sie die Dokumentation, Azure Service.

| Dienst | Artikel |
| -------- | -------- |
| Automatisierung | [Problembehandlung für häufige Fehler in Azure Automation](./automation/automation-troubleshooting-automation-errors.md) |
| Azure Stapel | [Problembehandlung bei Microsoft Azure Stapel](./azure-stack/azure-stack-troubleshooting.md) |
| Azure Stapel | [Webapps und Azure Stack](./azure-stack/azure-stack-webapps-troubleshoot-known-issues.md ) |
| Data Factory | [Problembehandlung bei Data Factory](./data-factory/data-factory-troubleshoot.md) |
| Service Fabric | [Behandeln Sie häufige Probleme beim Bereitstellen von Diensten auf Azure Service Fabric](./service-fabric/service-fabric-diagnostics-troubleshoot-common-scenarios.md) |
| Site Recovery | [Überwachung und Problembehandlung für virtuelle Computer und physischen Servern](./site-recovery/site-recovery-monitoring-and-troubleshooting.md) |
| Speicher | [Überwachung, diagnose und Problembehandlung von Microsoft Azure-Speicher](./storage/storage-monitoring-diagnosing-troubleshooting.md) |
| StorSimple | [Problembehandlung bei StorSimple Gerät Bereitstellungsprobleme](./storsimple/storsimple-troubleshoot-deployment.md) |
| SQL-Datenbank | [Verbindungsprobleme, Azure SQL-Datenbank](./sql-database/sql-database-troubleshoot-common-connection-issues.md) |
| SQL Datawarehouse | [Problembehandlung bei Azure SQL Datawarehouse](./sql-data-warehouse/sql-data-warehouse-troubleshoot.md) |

## <a name="understand-when-a-deployment-is-ready"></a>Verstehen Sie, wann eine Bereitstellung bereit

Azure Resource Manager meldet Erfolg Bereitstellung, wenn alle Anbieter von Bereitstellung erfolgreich zurückgegeben. Jedoch wird diese Meldung nicht unbedingt der Ressourcengruppe "aktiv und bereit für die Benutzer" ist Eine Bereitstellung müssen möglicherweise Updates herunterladen, auf nicht-Ressourcen warten oder komplexe Schriftzeichen installieren. Ressourcen-Manager weiß nicht, wenn diese Aufgaben nicht Aktivitäten sind, die ein Anbieter nachverfolgt. In diesen Fällen kann es einige Zeit bevor Ihre Ressourcen realen einsatzbereit sind sein. Daher erwarten, dass der Bereitstellungsstatus erfolgreich einige Zeit vor die Bereitstellung verwendet werden kann.

Sie können verhindern, dass Azure Erfolgsmeldung Bereitstellung, jedoch ein benutzerdefiniertes Skript für Ihre benutzerdefinierte Vorlage erstellen. Das Skript überwacht die gesamte Bereitstellung systemweiten Bereitschaft weiß und gibt erfolgreich nur, wenn Benutzer die gesamte Bereitstellung interagieren können. Möchten Sie sicherstellen, dass ist die Erweiterung zuletzt verwenden der **DependsOn** -Eigenschaft in der Vorlage.

## <a name="next-steps"></a>Nächste Schritte

- Zur Überwachung von Aktionen finden Sie unter [Audit-Operationen mit Ressourcen-Manager](resource-group-audit.md).
- Aktionen bestimmen die Fehler während der Bereitstellung finden Sie unter [Bereitstellungsvorgänge anzeigen](resource-manager-troubleshoot-deployments-portal.md).
