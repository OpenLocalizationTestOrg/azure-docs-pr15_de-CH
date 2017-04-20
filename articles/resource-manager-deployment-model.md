<properties
   pageTitle="Ressourcenmanager und Classic Bereitstellung | Microsoft Azure"
   description="Beschreibt die Unterschiede zwischen den Ressourcen-Manager-Bereitstellungsmodell und klassischen (oder Dienst) Bereitstellungsmodell."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-the-state-of-your-resources"></a>Azure Ressourcenmanager und klassischen Bereitstellung: Bereitstellungsmodelle und den Zustand Ihrer Ressourcen verstehen

In diesem Thema lernen Sie Azure Ressourcenmanager und klassischen Bereitstellungsmodelle, den Zustand Ihrer Ressourcen und Ressourcen mit einer bereitgestellt wurden. Ressourcenmanager und klassischen Bereitstellungsmodelle stellen zwei Arten der Bereitstellung und Verwaltung der Azure-Lösungen. Arbeiten mit ihnen über zwei verschiedene API und bereitgestellten Ressourcen können wichtige Unterschiede enthalten. Die beiden Modelle sind nicht vollständig kompatibel. Diese Unterschiede beschrieben.

Zur Vereinfachung der Bereitstellung und Verwaltung von Ressourcen empfiehlt Microsoft die Verwendung von Ressourcen-Manager für alle neuen Ressourcen. Microsoft empfiehlt ggf. vorhandene Ressourcen über Ressourcen-Manager erneut.

Wenn Sie zum Ressourcen-Manager sind, empfiehlt es sich, prüfen Sie zunächst die Terminologie in der [Azure-Ressourcen-Manager (Übersicht)](azure-resource-manager/resource-group-overview.md)definiert.

## <a name="history-of-the-deployment-models"></a>Verlauf der Bereitstellungsmodelle

Azure bereitgestellt ursprünglich nur das klassische Bereitstellungsmodell. In diesem Modell war jede Ressource einzeln; gab es keine Möglichkeit, verwandte Ressourcen gruppieren. Stattdessen mussten Sie manuell die Ressourcen Lösung oder Anwendung verfolgen und denken Sie daran, eine koordinierte Verwaltung. Um eine Projektmappe bereitzustellen, musste jede Ressource einzeln über das Verwaltungsportal erstellen oder erstellen Sie ein Skript, die alle Ressourcen in der richtigen Reihenfolge bereitgestellt. Zum Löschen einer Lösung musste jede Ressource einzeln löschen. Sie nicht leicht anzuwenden und Zugriffsrichtlinien für verwandte Ressourcen aktualisieren. Schließlich konnte nicht wenden Sie Tags zu Ressourcen, die sie mit bezeichnen, mit denen Sie Ihre Ressourcen überwachen und Verwalten von Abrechnung.

2014 eingeführt Azure Ressourcen-Manager, die das Konzept einer Ressourcengruppe hinzugefügt. Eine Ressourcengruppe ist ein Container für Ressourcen, die einen gemeinsamen Lebenszyklus aufweisen. Das Ressourcen-Manager-Bereitstellungsmodell bietet mehrere Vorteile:

- Sie können bereitstellen, verwalten und überwachen alle Dienste für die Lösung als Gruppe anstatt behandeln diese einzeln services.
- Sie können wiederholt während des gesamten Lebenszyklus Ihrer Lösung bereitstellen und Vertrauen in einem konsistenten Zustand Ihrer Ressourcen bereitgestellt werden.
- Gelten Zugriffskontrolle für alle Ressourcen in der Ressourcengruppe und die Richtlinien werden automatisch angewendet, wenn neue Ressourcen der Ressourcengruppe hinzugefügt werden.
- Sie können Ressourcen alle Ressourcen in Ihrem Abonnement logisch zu Tags anwenden.
- JavaScript Object Notation (JSON) können Sie die Infrastruktur für die Projektmappe definieren. JSON-Datei wird als Ressourcen-Manager-Vorlage bezeichnet.
- Sie können die Abhängigkeit zwischen Ressourcen, damit sie in der richtigen Reihenfolge bereitgestellt werden.

Ressourcen-Manager hinzugefügt wurde, wurden alle Ressourcen Standardressourcengruppen rückwirkend hinzugefügt. Beim Erstellen einer Ressource über die klassische Bereitstellung jetzt wird die Ressource automatisch innerhalb einer Ressourcengruppe Standard für diesen Dienst erstellt, obwohl dieser Ressourcengruppe bei der Bereitstellung nicht angegeben haben. Allerdings bedeutet nur innerhalb einer Ressourcengruppe vorhandenen nicht, dass die Ressource in der Ressourcen-Manager-Modell konvertiert wurde. Wir werden wie jeder Dienst zwei Modelle im nächsten Abschnitt behandelt. 

## <a name="understand-support-for-the-models"></a>Unterstützung für die Modelle verstehen 

Bei welcher Bereitstellungsmodell für Ihre Ressourcen sind drei Szenarien zu:

1. Der Dienst unterstützt Ressourcenmanager und stellt nur einen einzigen Typ.
2. Der Dienst unterstützt Ressourcenmanager sondern zwei Arten - Ressourcen-Manager und Classic. Dieses Szenario gilt nur für virtuelle Computer, Speicherkonten und virtuelle Netzwerke.
3. Der Dienst unterstützt keine Ressourcen-Manager.

Um zu ermitteln, ob ein Dienst Ressourcen-Manager unterstützt, finden Sie unter [Resource Manager Anbieter](resource-manager-supported-services.md).

Wenn der Dienst verwenden möchten Ressourcen-Manager nicht unterstützt, müssen Sie weiterhin klassische Bereitstellung.

Wenn der Dienst Ressourcenmanager und **ist** ein virtueller Computer, Speicherkonto oder virtuelles Netzwerk unterstützt, können Sie ohne Komplikationen Ressourcenmanager.

Für virtuelle Computer, Speicherkonten und virtuelle Netzwerke Wenn die Ressource über die klassische Bereitstellung erstellt wurde müssen Sie weiterhin über klassische Vorgänge ausgeführt werden. Wenn die virtuellen Computer Speicherkonto oder virtuelles Netzwerk Ressourcenmanager Bereitstellung erstellt wurde, müssen Sie weiterhin mit Ressourcen-Manager. Diese Unterscheidung kann verwirrend, wenn Ihr Abonnement eine Mischung von Ressourcen über Ressourcen-Manager und klassische Bereitstellung erstellt wurden enthält. Diese Kombination von Ressourcen kann unerwartete Ergebnisse erstellen, da die Ressourcen nicht dieselben Operationen unterstützen.

In einigen Fällen ein Ressourcen-Manager-Befehl kann Informationen über die klassische Bereitstellung erstellt abrufen oder führen eine administrative Aufgabe wie eine klassische Ressource in eine andere Ressourcengruppe verschieben. Aber diesen sollte nicht den Eindruck, dass der Ressourcen-Manager-Vorgänge unterstützt. Beispielsweise haben Sie eine Ressourcengruppe mit einem virtuellen Computer, der mit klassischen Bereitstellung erstellt wurde. Wenn Sie den folgenden Ressourcen-Manager-PowerShell-Befehl ausführen:

    Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines

Der virtuelle Computer wird:
    
    Name              : ExampleClassicVM
    ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
    ResourceName      : ExampleClassicVM
    ResourceType      : Microsoft.ClassicCompute/virtualMachines
    ResourceGroupName : ExampleGroup
    Location          : westus
    SubscriptionId    : {guid}

Der Ressourcen-Manager-Cmdlet **Get-AzureRmVM** gibt jedoch nur über Ressourcen-Manager eingesetzt. Der folgende Befehl kehrt der virtuellen Maschine über die klassische Bereitstellung erstellt.

    Get-AzureRmVM -ResourceGroupName ExampleGroup

Nur Ressourcen über Ressourcen-Manager unterstützt Tags erstellt. Sie können keine klassischen Ressourcen Tags zuweisen.

## <a name="resource-manager-characteristics"></a>Ressourcen-Manager-Eigenschaften

Um die beiden Modelle verstehen, betrachten wir die Eigenschaften der Ressourcen-Manager:

- [Azure-Portal](https://portal.azure.com/)erstellt.

     ![Azure-portal](./media/resource-manager-deployment-model/portal.png)

     Für COMPUTE-, Speicher- und Netzwerkressourcen können Sie mithilfe der Ressourcen-Manager oder Classic Bereitstellung. Wählen Sie **Ressource-Manager**.

     ![Ressourcen-Manager-Bereitstellung](./media/resource-manager-deployment-model/select-resource-manager.png)

- Mit Ressourcen-Manager Version von Azure PowerShell-Cmdlets erstellt. Diese Befehle haben das Format *AzureRmNoun Verb*.

        New-AzureRmResourceGroupDeployment

- Über die übrigen Operationen [Azure Ressourcenmanager REST-API](https://msdn.microsoft.com/library/azure/dn790568.aspx) erstellt.

- Erstellt durch Azure-CLI-Befehle in den **Arm** -Modus ausgeführt.

        azure config mode arm
        azure group deployment create 

- Der Ressourcentyp umfasst **(classic)** nicht in den Namen. Die folgende Abbildung zeigt den Typ als **Speicherkonto**.

    ![WebApp](./media/resource-manager-deployment-model/resource-manager-type.png)

## <a name="classic-deployment-characteristics"></a>Klassische Bereitstellungsmerkmale

Service Management-Modell können Sie auch das klassische Bereitstellungsmodell wissen.

Ressourcen im klassischen Bereitstellungsmodell gemeinsam die folgenden Merkmale:

- Über das [Verwaltungsportal](https://manage.windowsazure.com) erstellt

     ![Verwaltungsportal](./media/resource-manager-deployment-model/classic-portal.png)

     Oder der Azure-Portal und **klassischen** Bereitstellung (Compute, Speicher und Netzwerke) angeben.

     ![Klassische Bereitstellung](./media/resource-manager-deployment-model/select-classic.png)

- Über die Service Management-Version von Azure PowerShell-Cmdlets erstellt. Diese Namen haben das Format *AzureNoun Verb*.

        New-AzureVM 

- Über die [Service Management REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) für die übrigen Vorgänge erstellt.
- Erstellt durch Azure-CLI-Befehle im **Asm** -Modus ausführen.

        azure config mode asm
        azure vm create 

- Der Ressourcentyp enthält den Namen **(classic)** . Die folgende Abbildung zeigt den Typ als **Speicherkonto (classic)**.

    ![Klassische](./media/resource-manager-deployment-model/classic-type.png)

Azure-Portal können Sie Ressourcen verwalten, die über die klassische Bereitstellung erstellt wurden.

## <a name="changes-for-compute-network-and-storage"></a>Ändert für Compute, Netzwerk und Speicher

Das folgende Diagramm zeigt Computing-, Netzwerk- und Speicher-Ressourcen über Ressourcen-Manager bereitgestellt.

![Ressourcen-Manager-Architektur](./media/virtual-machines-azure-resource-manager-architecture/arm_arch3.png)

Beachten Sie die folgende Beziehung zwischen Ressourcen:

- Alle Ressourcen vorhanden in einer Ressourcengruppe.
- Der virtuelle Computer hängt ein bestimmtes Konto definiert den Speicher Ressource zum Speichern der Festplatten im BLOB-Speicher (erforderlich).
- Der virtuelle Computer verweist auf eine bestimmte Netzwerkkarte in der Ressource Netzwerkanbieter (erforderlich) und eine Verfügbarkeit festgelegt in der Ressourcenanbieter berechnen (optional) definiert.
- Die NIC verweist auf dem virtuellen Computer zugewiesene IP-Adresse (erforderlich) Subnetz des virtuellen Netzwerks für den virtuellen Computer (erforderlich) und eine Netzwerk-Sicherheitsgruppe (optional).
- Das Subnetz in einem virtuellen Netzwerk verweist auf eine Netzwerk-Sicherheitsgruppe (optional).
- Load Balancer Instanz verweist auf den Back-End-Pool von IP-Adressen, die die Netzwerkkarte eines virtuellen Computers (optional) und verweist auf eine Load Balancer öffentliche oder private IP-Adresse (optional).

Hier werden die Komponenten und deren Beziehungen für klassische Bereitstellung:

![Klassische Architektur](./media/virtual-machines-azure-resource-manager-architecture/arm_arch1.png)

Klassische Lösung für einen virtuellen Computer hosten umfasst:

- Eine erforderliche Clouddienst als Container fungiert für virtuelle Computer (Berechnung) gehostet. Virtuelle Computer werden automatisch mit Netzwerkschnittstellenkarte (NIC) und IP-Adresse von Azure. Cloud-Dienst enthält außerdem eine externe Load Balancer Instanz, eine öffentliche IP-Adresse und Standardendpunkte remote Remotedesktop und PowerShell für Windows-basierten virtuellen Maschinen und Secure Shell (SSH) Datenverkehr für Linux-basierten virtuellen Maschinen zu.
- Eine erforderliche Speicherkonto, die VHDs für einen virtuellen Computer, einschließlich des Betriebssystems temporäre und zusätzliche Datenträger (Speicher) gespeichert.
- Ein optionales virtuelles Netzwerk, das einen zusätzlichen Container fungiert, in dem Sie ein Subnetz Struktur und Festlegen des Subnetzes auf dem virtuellen Computer befindet (Netzwerk).

Die folgende Tabelle beschreibt die Änderungen Interaktion Ressourcenprovider Computing-, Netzwerk- und Speicher:

 Artikel | Classic | Ressourcen-Manager
 ---|---|---
| Cloud-Dienst für virtuelle Maschinen |  Clouddienst konnte einen Container zum Speichern von virtuellen Maschinen, die Verfügbarkeit von Plattform und Lastenausgleich erforderlich. | Cloud-Dienst ist nicht mehr ein Objekt zum Erstellen einer virtuellen Maschine mit dem neuen Modell erforderlich. |
| Virtuelle Netzwerke | Ein virtuelles Netzwerk ist optional für den virtuellen Computer. Wenn, kann das virtuelle Netzwerk mit Ressourcen-Manager bereitgestellt werden. | Virtuelle Computer benötigt ein virtuelles Netzwerk mit Ressourcen-Manager bereitgestellt wurde. |
| Speicherkonten | Der virtuelle Computer benötigt ein Speicherkonto, die Festplatten für das Betriebssystem temporäre und zusätzliche Datenträger speichert. | Der virtuelle Computer benötigt ein Speicherkonto seine Laufwerke im BLOB-Speicher gespeichert. |
| Verfügbarkeit-Sets | Verfügbarkeit der Plattform wurde durch die gleichen "AvailabilitySetName" auf den virtuellen Computern konfigurieren angezeigt. Die maximale Anzahl von Fehlerdomänen ist 2. | Verfügbarkeit ist eine Ressource von Microsoft.Compute-Anbieter verfügbar gemacht werden. Virtuelle Computer, die hohen Verfügbarkeit erfordern Verfügbarkeit festlegen beizufügen. Die maximale Anzahl von Fehlerdomänen ist jetzt 3. |
| Gruppen | Gruppen wurden für die Erstellung von virtuellen Netzwerken erforderlich. Jedoch war mit der Einführung von regionalen virtuelle Netzwerke, die nicht erforderlichen mehr. |Zur Vereinfachung ist das Konzept von Gruppen in über Azure-Ressourcen-Manager verfügbaren APIs vorhanden. |
| Lastenausgleich    | Erstellen eines Cloud-Diensts enthält eine implizite Lastenausgleich für die virtuellen Computer bereitgestellt. | Der Lastenausgleich ist eine Ressource von der Microsoft.Network-Anbieter verfügbar gemacht. Die primäre Netzwerkschnittstelle virtueller Maschinen, die Lastenausgleich sollte Lastenausgleich verweisen. Systeme zum Lastenausgleich können intern oder extern sein. Eine Load Balancer Instanz verweist auf den Back-End-Pool von IP-Adressen, die die Netzwerkkarte eines virtuellen Computers (optional) und verweist auf eine Load Balancer öffentliche oder private IP-Adresse (optional). [Erfahren Sie mehr.](../articles/resource-groups-networking.md) |
|Virtuelle IP-Adresse | Cloud-Dienste erhalten standardmäßig VIP (Virtual IP-Adresse), wenn ein virtueller Computer einen Cloud-Dienst hinzugefügt wird. Die virtuelle IP-Adresse ist die Adresse implizite Lastenausgleich.    | Öffentliche IP-Adresse ist eine Ressource von der Microsoft.Network-Anbieter verfügbar gemacht werden. Öffentliche IP-Adresse kann (reserviert) statisch oder dynamisch sein. Ein Lastenausgleich kann dynamische öffentliche IP-Adressen zugewiesen. Öffentliche IP-Adressen kann Sicherheitsgruppen gesichert werden. |
|Reservierte IP-Adresse|   Sie können eine IP-Adresse in Azure reservieren und Cloud-Dienst um sicherzustellen, dass die IP-Adresse sticky zuordnen.   | Öffentliche IP-Adresse "statisch" erstellt werden, und bietet dieselbe Funktion wie ein "reservierte IP-Adresse". Statische öffentliche IP-Adressen kann nur ein Lastenausgleich jetzt zugewiesen werden. |
|Öffentliche IP-Adresse (PIP) pro VM | Öffentliche IP-Adressen können auch direkt einen VM zugeordnet werden. | Öffentliche IP-Adresse ist eine Ressource von der Microsoft.Network-Anbieter verfügbar gemacht werden. Öffentliche IP-Adresse kann (reserviert) statisch oder dynamisch sein. Jedoch können nur dynamische öffentliche IP-Adressen eine Netzwerkschnittstelle zugewiesen werden eine öffentliche IP-Adresse pro VM jetzt abrufen. |
|Endpunkte| Input Endpunkte erforderlich, auf einem virtuellen Computer Konnektivität für bestimmte Ports öffnen, konfiguriert werden. Einer der gängigen Modi mit virtuellen Computern möglich, indem Sie input Endpunkte zu verbinden. | Eingehende NAT-Regeln können auf Lastenausgleich zu dieselbe Funktion aktivieren Endpunkte auf bestimmten Ports für VMs konfiguriert werden. |
|DNS-Name| Cloud-Dienst erhalten eine implizite global eindeutigen DNS-Namen. Beispiel: `mycoffeeshop.cloudapp.net`. | DNS-Namen sind optionale Parameter, die von einer öffentlichen IP-Adresse angegeben werden. Der FQDN ist im folgenden Format - `<domainlabel>.<region>.cloudapp.azure.com`. |
|Netzwerk-Schnittstellen | Primäre und sekundäre Netzwerkschnittstelle und deren Eigenschaften als Netzwerkkonfiguration des virtuellen Computers definiert wurden. | Netzwerkschnittstelle ist eine Ressource von Microsoft.Network-Anbieter verfügbar gemacht werden. Der Lebenszyklus der Netzwerkschnittstelle ist nicht auf einem virtuellen Computer gebunden. Sie verweist auf dem virtuellen Computer zugewiesene IP-Adresse (erforderlich) Subnetz des virtuellen Netzwerks für den virtuellen Computer (erforderlich) und eine Netzwerk-Sicherheitsgruppe (optional). |

Zum Verbinden von virtueller Netzwerken aus verschiedenen Modellen finden Sie unter [virtuelle Netzwerke aus verschiedenen Modellen im Portal verbinden](./vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-to-resource-manager"></a>Migrieren von klassischen Resource Manager

Wenn Sie Ressourcen von klassischen Bereitstellung Ressourcenmanager Bereitstellung migrieren möchten, finden Sie unter:

1. [Technische detaillierte technische Informationen zur Plattform unterstützt Migration von Classic an Azure-Ressourcen-Manager](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
2. [Plattform-Migration IaaS-Ressourcen von Classic an Azure-Ressourcen-Manager](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
3. [Migrieren von IaaS-Ressourcen von Classic an Azure-Ressourcen-Manager mithilfe von Azure PowerShell](./virtual-machines/virtual-machines-windows-ps-migration-classic-resource-manager.md)
4. [Migrieren Sie IaaS-Ressourcen von Classic an Azure-Ressourcen-Manager mit Azure-CLI](./virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Kann mit Azure-Ressourcen-Manager für die Bereitstellung in einem virtuellen Netzwerk oder Speicherkonto klassische Bereitstellung erstellt einen virtuellen Computer erstellen?**

Dies wird nicht unterstützt. Azure-Ressourcen-Manager können einen virtuellen Computer in einem virtuellen Netzwerk bereitstellen, die mit klassischen Bereitstellung erstellt wurde.

**Erstelle ich einen virtuellen Computer mit der Azure-Ressourcen-Manager über ein Bild, das mithilfe der Azure Service Management-APIs erstellt wurde?**

Dies wird nicht unterstützt. Sie können jedoch kopieren VHD-Dateien aus ein Speicherkonto mithilfe der Service Management-APIs erstellt wurde und ein neues Konto erstellt durch Azure-Ressourcen-Manager hinzufügen.

**Was ist die Auswirkung auf die Quote für mein Abonnement?**

Kontingente für virtuelle Computer, virtuelle Netzwerke und Speicherkonten über Azure-Manager erstellten unterscheiden sich von anderen Kontingente. Jedes Abonnement wird Kontingente Ressourcen unter Verwendung der neuen APIs erstellen. Erfahren Sie mehr über zusätzliche Kontingente [hier](../articles/azure-subscription-service-limits.md).

**Kann ich weiterhin automatisierte Skripts für die Bereitstellung von virtuellen Maschinen, virtuelle Netzwerke und Speicherkonten durch den Ressourcen-Manager-APIs verwenden?**

Die Automatisierung und Skripts, die Sie erstellt haben weiterhin für vorhandene virtuelle Computer, virtuelle Netzwerke unter Azure Service-Modus erstellt. Skripts müssen jedoch aktualisiert werden, um das neue Schema für die Erstellung von Ressourcen über Ressourcen-Manager-Modus verwenden.

**Wo finde ich Beispiele für Ressourcenmanager Azure Vorlagen?**

Umfassende startvorlagen [Azure Resource Manager QuickStart Vorlagen](https://azure.microsoft.com/documentation/templates/)entnehmen.

## <a name="next-steps"></a>Nächste Schritte

- Um die Erstellung der Vorlage durchlaufen, die einen virtuellen Computer definiert, siehe Speicherkonto und virtuelles Netzwerk [Ressourcenmanager Vorlage Exemplarische Vorgehensweise](resource-manager-template-walkthrough.md).
- Die Befehle zum Bereitstellen einer Vorlage finden Sie unter [Bereitstellen einer Anwendung mit Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).
