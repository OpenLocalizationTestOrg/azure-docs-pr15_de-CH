<properties 
    pageTitle="Neue Ressourcengruppe Ressourcen verschieben | Microsoft Azure" 
    description="Verwenden Sie Azure-Ressourcen-Manager zum Verschieben der Ressourcen eine neue Ressourcengruppe oder Abonnement." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/21/2016" 
    ms.author="tomfitz"/>

# <a name="move-resources-to-new-resource-group-or-subscription"></a>Neue Ressourcengruppe oder Abonnement Ressourcen verschieben

In diesem Thema veranschaulicht die Ressourcen in ein neues Abonnement oder eine neue Ressourcengruppe im selben Abonnement verschieben. Das Portal, PowerShell, Azure-CLI oder die REST-API können Sie Ressource verschieben. Verschiebeoperationen in diesem Thema stehen Ihnen ohne Hilfe von Azure-Support.

Normalerweise wird Ressourcen verschieben, wenn Sie entscheiden:

- Eine Ressource muss für Zahlungszwecke in ein anderes Abonnement Leben.
- Eine Ressource teilt nicht denselben Lebenszyklus wie die Ressourcen, die zuvor mit gruppiert war. Möchten eine neue Ressourcengruppe verschieben, so dass Sie diese Ressource von anderen Ressourcen getrennt verwalten können.

Beim Verschieben von Ressourcen der Gruppe und die Zielgruppe während des Vorgangs gesperrt. Schreiben und löschen auf Gruppen blockiert, bis zum Abschluss der Verschiebung.

Die Ressource kann nicht geändert werden. Eine Ressource verschieben, nur wird eine neue Ressourcengruppe. Die neue Ressourcengruppe möglicherweise einen anderen Speicherort, aber das ändert nicht die Position der Ressource.

> [AZURE.NOTE] Dieser Artikel beschreibt, wie Ressourcen in vorhandenen Azure bietet Konto verschieben. Wenn Sie tatsächlich mit vorhandenen Ressourcen (z. B. Aktualisieren von nutzungsbasierte Vorauszahlung) mit der Azure-Konto ändern möchten, finden Sie unter [Wechseln der Azure-Abonnement zu einem anderen Angebot](billing-how-to-switch-azure-offer.md). 

## <a name="checklist-before-moving-resources"></a>Checkliste vor dem Verschieben von Ressourcen

Es gibt einige wichtige Schritte vor dem Verschieben einer Ressource durchführen. Diese Bedingung überprüfen, können Sie Fehler vermeiden.

1. Der Dienst muss Ressourcen verschieben können. Liste für Informationen über die [Dienste aktivieren verschieben Ressourcen](#services-that-enable-move)anzeigen
1. Die Quell- und Abonnements müssen innerhalb derselben [Active Directory Mieter](./active-directory/active-directory-howto-tenant.md)vorhanden. Um einen neuen Mandanten zu verschieben, an den Support.
2. Der Ressourcenanbieter Ressource verschoben werden Zielabonnement registriert. Wenn dies nicht der Fall, wird eine Fehlermeldung, dass das **Abonnement für einen Ressourcentyp nicht registriert ist**. Dieses Problem auftreten, wenn eine Ressource auf ein neues Abonnement verschieben, aber nie Abonnement mit diesem Ressourcentyp verwendet wurde. Zum Überprüfen des Status und registrieren Ressourcenprovider finden Sie [Ressourcen und Typen](../resource-manager-supported-services.md#resource-providers-and-types).
4. Verschieben von App Service-app haben [App Service Einschränkungen](#app-service-limitations)überprüft.
4. Verschieben von Ressourcen mit Recovery Services haben Sie [Recovery Services Einschränkungen](#recovery-services-limitations) überprüft.
5. Verschieben von Ressourcen über Klassisch bereitgestellt haben [Classic Bereitstellung Einschränkungen](#classic-deployment-limitations)überprüft.

## <a name="when-to-call-support"></a>Beim Kundendienst

Sie können die meisten Ressourcen durch Self-service-Vorgänge, die in diesem Thema gezeigten verschieben. Self-service-Operationen zu verwenden:

- Verschieben Sie Ressourcenmanager Ressourcen.
- Verschieben Sie klassische Ressourcen nach [klassischen Bereitstellung Grenzen](#classic-deployment-limitations). 

Wenden Sie bei Bedarf:

- Ressourcen, neue Azure-Konto (Active Directory Mieter) verschieben.
- Klassische Ressourcen aber Probleme mit der.

## <a name="services-that-enable-move"></a>Dienste, mit denen verschieben

Jetzt sind die Dienste, mit denen eine neue Ressourcengruppe und Abonnement verschieben:

- API-Management
- App Service apps (Web apps) - Siehe [App Service Einschränkungen](#app-service-limitations)
- Automatisierung
- Stapelverarbeitung
- CDN
- Cloud-Services - Siehe [Classic Bereitstellung Grenzen](#classic-deployment-limitations)
- Data Factory
- DNS
- DocumentDB
- HDInsight-Cluster
- IoT Hubs
- Key Vault 
- Media Services
- Mobile Engagement
- Benachrichtigungshubs
- Betriebliche Informationen
- Redis Cache
- Planer
- Suche
- Servicebus
- Speicher
- (Klassisch) - Siehe [Classic Bereitstellung Grenzen](#classic-deployment-limitations)
- Datenbank der SQL Server - Datenbank und den Server muss in derselben Ressourcengruppe befinden. Beim Verschieben von SQL Server werden auch alle zugehörigen Datenbanken verschoben.
- Virtueller Computer – jedoch wird nicht in ein neues Abonnement verschieben, wenn die Zertifikate in einem Schlüssel gespeichert sind
- Virtuelle Computer (klassisch) - Siehe [Classic Bereitstellung Grenzen](#classic-deployment-limitations)
- Virtuelle Netzwerke

## <a name="services-that-do-not-enable-move"></a>Dienste, die nicht zu, verschieben aktivieren

Die Dienste, die derzeit nicht zu aktivieren, Verschieben einer Ressource sind:

- Application Gateway
- Anwendung Einblicke
- Schnellstraße
- Recovery Services vault - auch nicht verschieben die Ressourcen Computing-, Netzwerk- und Speicher mit der Recovery-Services, siehe [Recovery Services Einschränkungen](#recovery-services-limitations).
- Virtuelle Computer mit Schlüssel Depot gespeichert
- Virtuelle Computer Maßstab legt
- Virtuelle Netzwerke (klassisch) - Siehe [Classic Bereitstellung Grenzen](#classic-deployment-limitations)
- VPN-Gateway

## <a name="app-service-limitations"></a>App Service Grenzen

Beim Arbeiten mit App Service apps verschieben nicht nur einen App Service-Plan. Um App Service apps zu verschieben, sind die Optionen:

- Bewegen der App Service-Plan und alle anderen App Ressourcen Gruppe eine neue Ressourcengruppe, die Ressourcen App noch nicht Das bedeutet, dass auch die Ressourcen App Service verschieben müssen, die nicht die App Service-Plan zugeordnet sind. 
- Verschieben Sie apps in eine andere Ressourcengruppe, aber halten Sie alle App Service-Pläne in der ursprünglichen Ressourcengruppe.

Der ursprüngliche Ressourcengruppe umfasst auch eine Anwendung Insights-Ressource verschieben nicht da Anwendung Einblicke nicht gerade verschoben können Ressource. Wenn Sie beim Verschieben von App Service apps Application Insights-Ressource einschließen, fehl der Umzug. Anwendung Einblicke und App Service-Plan müssen allerdings nicht in derselben Ressourcengruppe wie die Anwendung für die Anwendung ordnungsgemäß befinden.

Wenn beispielsweise der Ressourcengruppe enthält:

- **Web eine** der zugeordneten **Plan ein** und **app Einblicke ein**
- **b Web** **Plan b** und **Einblicke Anw** zugeordnet ist

Die Optionen sind:

- Verschieben **von a**, **Plan a**, **Web-b**und **b Plan**
- Verschieben **von a** und **b Web**
- Verschieben **von a**
- Verschieben **von b**

Alle anderen Kombinationen umfassen einen Ressourcentyp, der (Anwendung Erkenntnisse) verschieben kann nicht verschoben oder hinter einen Ressourcentyp hinter bleiben kann, wenn einen App Service-Plan (jede Art von Anwendung Dienstressource).

Wenn Ihrer Anwendung befindet sich in einer anderen Ressourcengruppe als App Service-Plan jedoch auf eine neue Ressourcengruppe verschieben möchten, müssen Sie das Verschieben in zwei Schritten ausführen. Zum Beispiel:

- **von einer** **Web -** Gruppe befinden
- **Plan ein** **Plan** Gruppe befindet
- Soll **Web ein** und **Planen einer** **kombinierten Gruppe** befinden

Um dies zu erreichen, führen Sie zwei separate Verschiebeoperationen in der folgenden Reihenfolge:

1. Verschieben der **Web eine** **Plan -** Gruppe
2. Verschieben **von a** und **Plan einer** **kombinierten**Gruppe.

Sie können eine neue Ressourcengruppe oder ohne Probleme ein Dienstzertifikat App verschieben. Enthält Ihrer Anwendung ein SSL-Zertifikat, das Sie erworben und an die Anwendung übertragen, müssen Sie das Zertifikat löschen, bevor Web app verschieben. Beispielsweise können Sie die folgenden Schritte ausführen:

1. Hochgeladene Zertifikat aus dem Web app löschen
2. Web app verschieben
3. Das Zertifikat Web App hochladen

## <a name="recovery-services-limitations"></a>Recovery Services Grenzen

Bewegen für Speicher, Netzwerk, nicht aktiviert ist oder Datenverarbeitungsressourcen Disaster Recovery mit Azure Site Recovery eingerichtet. 

Angenommen Sie, die Replikation von Ihrem lokalen Computer ein Speicherkonto (Storage1) eingerichtet haben und geschützten Computer nach einem Failover in Azure als eine virtuelle Maschine (VM1) zu einem virtuellen Netzwerk (verwendet1) kommen. Diese Azure Ressourcen - Storage1 VM1 und verwendet1 - kann nicht über Ressourcengruppen innerhalb des gleichen Abonnements oder Abonnements verschieben.

## <a name="classic-deployment-limitations"></a>Klassische Bereitstellung Grenzen

Optionen zum Verschieben von Ressourcen über das klassische Modell davon abhängig, ob Sie Ressourcen in einem Abonnement oder ein neues Abonnement verschieben. 

### <a name="same-subscription"></a>Dieselbe Abonnement

Beim Verschieben von Ressourcen aus einer Ressourcengruppe in eine andere Ressourcengruppe innerhalb des gleichen Abonnements gelten die folgenden Regeln:

- Virtuelle Netzwerke (classic) können nicht verschoben werden.
- Mit Cloud-Dienst müssen virtuelle Computer (klassisch) verschoben werden. 
- Cloud-Dienst kann nur verschoben werden, wenn das Verschieben der virtuellen Computer enthält.
- Nur ein Cloud-Dienst kann gleichzeitig verschoben werden.
- Nur ein Speicherkonto (classic) kann gleichzeitig verschoben werden.
- Konto (classic) kann im selben Arbeitsgang mit einem virtuellen Computer oder einem Clouddienst verschoben werden.

Verwenden Sie allgemeine Verschiebeoperationen [Portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure-CLI](#use-azure-cli)oder [REST-API](#use-rest-api)um classic Ressourcen innerhalb des gleichen Abonnements eine neue Ressourcengruppe verschieben. Sie verwenden die gleichen Vorgänge wie für Ressourcenmanager Ressourcen verschieben.

### <a name="new-subscription"></a>Neues Abonnement

Beim Verschieben von Ressourcen in ein neues Abonnement gelten die folgenden Regeln:

- Alle classic Ressourcen im Abonnement müssen im selben Arbeitsgang verschoben werden.
- Das gewünschte Abonnement darf keine classic Ressourcen enthalten.
- Die Verschiebung kann nur über eine separate REST API für klassische verschiebt angefordert werden. Ressourcenmanager verschieben Standardbefehle funktionieren nicht, wenn classic Ressourcen in ein neues Abonnement.

Um ein neues Abonnement classic Ressourcen verschieben, müssen Sie REST verwenden die classic Ressourcen sind. Führen Sie die folgenden Schritte aus, um ein neues Abonnement classic Ressourcen verschieben.

1. Überprüfen Sie, ob Quelle Abonnement in einem Cross-Abonnements teilnehmen kann. Verwenden Sie den folgenden Vorgang:

         POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
    
     Im Hauptteil Anforderung enthalten:

         {
           "role": "source"
         }

     Die Antwort für den Überprüfungsvorgang wird im folgenden Format:

         {
           "status": "{status}",
           "reasons": [
             "reason1",
             "reason2"
           ]
         }

2. Überprüfen Sie, ob Cross-Abonnement Verschieben des Zielabonnements teilnehmen kann. Verwenden Sie den folgenden Vorgang:

         POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01

     Im Hauptteil Anforderung enthalten:

         {
           "role": "target"
         }

     Die Antwort liegt im gleichen Format wie die Validierung der Datenquelle Abonnement.

3. Wenn beide Daueraufträge Validierung verschieben Sie alle classic Ressourcen von einem Abonnement einen Dauerauftrag mit dem folgenden Vorgang:

         POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01

    Im Hauptteil Anforderung enthalten:

         {
           "target": "/subscriptions/{target-subscription-id}"
         }

Der Vorgang kann mehrere Minuten ausgeführt. 

## <a name="use-portal"></a>Portal verwenden

Um eine neue Ressourcengruppe **dieselbe Abonnement**Ressourcen verschieben, wählen Sie die Ressourcengruppe, Ressourcen und wählen Sie dann die Schaltfläche **Verschieben** .

![Ressourcen verschieben](./media/resource-group-move-resources/edit-rg-icon.png)

Oder um Ressourcen in ein **Neues Abonnement**zu verschieben, wählen Sie die Ressourcengruppe, Ressourcen und wählen Sie dann das Abonnement Bearbeitungssymbol.

![Ressourcen verschieben](./media/resource-group-move-resources/change-subscription.png)

Wählen Sie Ressourcen verschieben und Ziel Ressourcengruppe aus. Bestätigen Sie, dass Sie Skripts für diese Ressourcen aktualisieren und auf **OK klicken**müssen. Wenn Sie Symbol Abonnement bearbeiten im vorherigen Schritt ausgewählt haben, müssen Sie das Zielabonnement auswählen.

![Ziel auswählen](./media/resource-group-move-resources/select-destination.png)

**Benachrichtigung**anzeigen, dass der Verschiebevorgang ausgeführt wird

![Verschieben-Status anzeigen](./media/resource-group-move-resources/show-status.png)

Wenn er abgeschlossen ist, werden Sie das Ergebnis benachrichtigt.

![Verschieben Ergebnis anzeigen](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>Mithilfe von PowerShell

Um vorhandene Ressourcen Ressourcengruppe oder Abonnement zu wechseln, den Befehl **AzureRmResource verschieben** .

Im ersten Beispiel wird veranschaulicht, wie eine Ressource eine neue Ressourcengruppe verschieben.

    $resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId

Im zweiten Beispiel wird veranschaulicht, wie mehrere Ressourcen in eine neue Ressourcengruppe verschieben.

    $webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
    $plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId

Um ein neues Abonnement zu verschieben, enthalten Sie einen Wert für den Parameter **DestinationSubscriptionId** .

Sie müssen bestätigen, dass die angegebenen Ressourcen verschieben möchten.

    Confirm
    Are you sure you want to move these resources to the resource group
    '/subscriptions/{guid}/resourceGroups/newRG' the resources:

    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

## <a name="use-azure-cli"></a>Verwenden von Azure CLI

Um vorhandene Ressourcen in einer anderen Ressourcengruppe oder Abonnement verschieben, Befehl **Azure Ressource verschieben** . Sie müssen Ressourcen-Ids der Ressourcen zu ermöglichen. Sie können Ressourcen-Ids mit dem folgenden Befehl abrufen:

    azure resource list -g sourceGroup --json

Die folgende Format zurückgegeben:

    [
      {
        "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
        "name": "storagedemo",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "southcentralus",
        "tags": {},
        "kind": "Storage",
        "sku": {
          "name": "Standard_RAGRS",
          "tier": "Standard"
        }
      }
    ]

Im folgenden Beispiel wird veranschaulicht, wie ein Speicherkonto in eine neue Ressourcengruppe verschieben. Der Parameter **-i** bieten Sie eine durch Kommas getrennte Liste der Ressourcen-Id zu.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
    
Sie werden aufgefordert zu bestätigen, dass die angegebene Ressource verschieben möchten.

## <a name="use-rest-api"></a>REST-API verwenden

Um vorhandene Ressourcen Ressourcengruppe oder Abonnement zu wechseln, führen Sie Folgendes aus:

    POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version} 

Im Hauptteil Anforderung Geben Sie die Ziel-Ressourcengruppe und Ressourcen verschieben. Weitere Informationen zu den restlichen Vorgang finden Sie unter [Ressourcen](https://msdn.microsoft.com/library/azure/mt218710.aspx).


## <a name="next-steps"></a>Nächste Schritte
- Informationen zu PowerShell-Cmdlets für die Verwaltung Ihres Abonnements finden Sie unter [Verwendung von Azure PowerShell mit Ressourcen-Manager](powershell-azure-resource-manager.md).
- Informationen zu Azure-CLI-Befehle für die Verwaltung Ihres Abonnements finden Sie unter [Verwendung der Azure-CLI mit Ressourcen-Manager](xplat-cli-azure-resource-manager.md).
- Informationen zu den Funktionen für die Verwaltung Ihres Abonnements finden Sie unter [Verwenden des Azure-Portals zur Verwaltung der Ressourcen](./azure-portal/resource-group-portal.md).
- Zum Anwenden einer logischen Organisation auf Ressourcen finden Sie unter [Tags zu Ressourcen verwenden](resource-group-using-tags.md).
