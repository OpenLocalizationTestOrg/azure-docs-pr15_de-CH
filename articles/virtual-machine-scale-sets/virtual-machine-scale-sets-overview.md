<properties
    pageTitle="Virtual Machine Maßstab legt Übersicht | Microsoft Azure"
    description="Erfahren Sie mehr über Virtual Machine skalieren"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

# <a name="virtual-machine-scale-sets-overview"></a>Virtual Machine Maßstab legt (Übersicht)

Virtual Machine sind Skala eine Azure Compute-Ressource zum Bereitstellen und Verwalten einer identischen VMs verwendeten. VMs konfiguriert gleich VM Maßstab legt true skalieren – ohne Vorabbereitstellung VMs unterstützen sollen erforderlich – und so erleichtert umfangreiche Services für große Compute, big Data und Container Arbeitslasten erstellen.

Für Clientanwendungen müssen Datenverarbeitungsressourcen und in Skala Operationen implizit verteilt werden Fehlertoleranz und Update domänenübergreifend skalieren. Einführung in VM Skalierung finden legt der [Azure-Blog zur Ankündigung](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Sehen Sie sich diese Videos für über VM Maßstab legt an:

 - [Mark Russinovich spricht Azure Maßstab legt](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [Virtual Machine Maßstab legt mit Guy Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Erstellen und Verwalten von VM-Skalierung festgelegt

VM Skalierung festlegen können in [Azure-Portal](https://portal.azure.com) durch _neue_ auswählen und "Skalieren" in der Suchleiste eingeben. "Virtual Machine Maßstab" wird in den Ergebnissen angezeigt werden. Dort können Sie füllen Sie die erforderlichen Felder anpassen und Bereitstellen der Skalierung festlegen. 

VM-Skalierung kann auch definiert und mit JSON Vorlagen und [REST-APIs](https://msdn.microsoft.com/library/mt589023.aspx) nur bereitgestellt wie einzelne Azure Ressourcenmanager VMs. Daher können alle standardmäßigen Bereitstellungsmethoden Azure-Ressourcen-Manager verwendet. Weitere Informationen zu Vorlagen finden Sie unter [Erstellen von Azure Resource Manager Vorlagen](../resource-group-authoring-templates.md).

Eine Reihe von Beispielvorlagen für VM Maßstab legt finden Sie in Azure Schnellstart Vorlagen GitHub Repository [hier.](https://github.com/Azure/azure-quickstart-templates) (Suchen Sie nach Vorlagen mit _Vmss_ im Titel)

In den Detailseiten für diese Vorlagen sehen Sie eine Schaltfläche, die der Bereitstellung Funktion verknüpft. VM-Skalierungsgruppe bereitzustellen, klicken auf die Schaltfläche, und geben Sie alle Parameter, die im Portal erforderlich sind. Wenn Sie nicht sicher, ob eine Ressource unterstützt Ober- oder gemischten Fall Parameterwerte Kleinbuchstaben mit ist. Es gibt auch eine praktische video Zerlegung eine VM Skalierung Vorlage hier:

[VM Vorlage Zerlegung festlegen](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Skalierung VM skalieren einstellen und

Erhöhen oder verringern die Anzahl der virtuellen Computer in einer VM skalieren, die _Capacity_ -Eigenschaft ändern und die Vorlage erneut. Diese Einfachheit erleichtert die eigene benutzerdefinierte Skalierung Ebene Schreiben benutzerdefinierter Maßstab Ereignisse definieren, die von Azure skalieren nicht unterstützt werden.

Wenn bereitstellen eine Vorlage für die Kapazität definieren Sie eine viel kleinere Vorlage umfasst nur die SKU und die aktualisierte Kapazität. Ein Beispiel dafür gezeigt [hier.](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing)

Um die Schritte durchgehen, die Skalierung erstellen, die automatisch skaliert wird, finden Sie unter [Automatisch skalieren Computer in eine Virtual Machine skalieren](virtual-machine-scale-sets-windows-autoscale.md)

## <a name="monitoring-your-vm-scale-set"></a>Überwachung der VM-Skalierung

Der [Azure-Portal](https://portal.azure.com) Listen Maßstab legt zeigt grundlegende Eigenschaften, sowie Angebot VMs in der Gruppe. [Azure-Explorers](https://resources.azure.com) können Sie detaillierte VM Maßstab anzuzeigen. VM sind Skala eine Ressource unter Microsoft.Compute, von dieser Site sie sehen erweitern die folgenden Links:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>VM festlegen Szenarios

Dieser Abschnitt enthält einige typischen VM Skalierung festlegen Szenarien. Einige höhere Ebene Azure Services (wie Batch Service Fabric, Azure Container Service) verwendet diesen Szenarien.

 - **RDP / SSH VM Maßstab gesetzt Instanzen** - ein VM-Skalierungsgruppe innerhalb einer VNET erstellt und einzelnen VMs in den Maßstab nicht öffentliche IP-Adressen zugeordnet werden. Deswegen gut Sie wollen im allgemeinen Kosten und Management-Overhead der separate öffentliche IP-Adressen zuweist, die statusfreien Ressourcen im Raster Compute und problemlos herstellen die VMs von anderen Ressourcen in Ihrem VNET, darunter die öffentliche IP-Adressen wie Lastenausgleich oder eigenständige virtuelle Computer.

 - **Mit VMs mit NAT-Regeln** - erstellen Sie eine öffentliche IP-Adresse, ein Lastenausgleich zuweisen und Regeln eingehende NAT Port auf die IP-Adresse, die einen Port auf einem virtuellen Computer in der VM-Skalierungsgruppe zugeordnet. Zum Beispiel:
 
    Quelle | Ursprungs-Port | Ziel | Ziel-Port
    --- | --- | --- | ---
    Öffentliche IP-Adresse | Port 50000 | Vmss\_0 | Anschluss 22
    Öffentliche IP-Adresse | Port 50001 | Vmss\_1 | Anschluss 22
    Öffentliche IP-Adresse | Port 50002 | Vmss\_2 | Anschluss 22

    Hier ist ein Beispiel für das Erstellen einer VM Skalierung der NAT-Regeln verwendet, um SSH-Verbindung mit jeder VM in einem Maßstab über eine einzelne öffentliche IP-Adresse aktivieren: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Hier ist ein Beispiel mit RDP und Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - **Mit VMs mit "Jumpbox"** - erstellen eine VM-Skala festgelegt und eine eigenständige VM in demselben VNET, eigenständige VM und die VM Skalierung VMs mittels ihre interne IP-Adresse herstellen können Adressen gemäß VNET/Subnetz. Wenn eine öffentliche IP-Adresse erstellen und eigenständige VM zuweisen können Sie RDP oder SSH für die eigenständige VM und mit VM Skalierung festlegen Instanzen von diesem Computer verbinden. Sie können sehen, dass eine einfache VM Skalierung sichererer als einfache eigenständiger VM mit einer öffentlichen IP-Adresse in der Standardkonfiguration ist.

    [Diese Vorlage erstellt ein Beispiel dieses Ansatzes einen einfachen Mesos Cluster mit einer eigenständigen Master VM einen VM Skalierungsgruppe basierten Cluster VMs verwaltet.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **Lastenausgleich VM Maßstab Instanzen festgelegt** - Arbeit Computecluster VMs "Roundrobin" Ansatz liefern möchten, können Sie Azure Lastenausgleich mit Lastenausgleich Regeln entsprechend konfigurieren. Können Prüfpunkte zum Überprüfen von Ping Ausführen der Anwendung mit einem angegebenen Pfad Protokoll, Intervall und Anforderung. Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/) unterstützt auch skalieren, sowie komplexere Szenarien für den Lastenausgleich.

    [Hier ist ein Beispiel der VM Skalierung mehrere VMs mit IIS Webserver erstellt, und ein Lastenausgleich den Lastenausgleich jede VM erhält verwendet. Außerdem wird das HTTP-Protokoll zu eine bestimmte URL jedes virtuellen Computers ping verwendet.](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) (Überprüfen Sie den Ressourcentyp Microsoft.Network/loadBalancers, NetworkProfile und ExtensionProfile in der VirtualMachineScaleSet)

 - **Bereitstellen von einen VM als Computecluster in einer PaaS-Clusterverwaltung festlegen** - VM Maßstab legt manchmal als nächsten Generation workerrolle beschrieben werden. Es ist eine gültige Beschreibung es auch die Gefahr verwirrend Skalierung Satz Funktionen mit PaaS v1 Worker-Rolle. Insofern VM Skala ermöglichen eine echte "Worker-Rolle" oder Arbeitskraft, eine verallgemeinerte computeressource bereit Plattform/Runtime unabhängig ist anpassbar und Azure Ressourcenmanager IaaS integriert.

    Eine PaaS v1 Worker-Rolle, Plattform-Runtime-Unterstützung (nur Windows Plattform Bilder) eingeschränkt auch Services wie VIP-Swap Upgrade konfigurierbarer Runtime-app Deployment Einstellungen sind entweder nicht _noch_ für VM legt skalieren oder durch andere höhere Ebene PaaS-Dienste wie Service erfolgt. Dabei können Sie VM Maßstab legt als Infrastruktur betrachten, die PaaS unterstützt. D. h. PaaS-Lösungen wie Service Fabric oder Clustermanager wie Mesos auf VM kann skalieren legt eine skalierbare Compute-Ebene.

    [Diese Vorlage erstellt ein Beispiel dieses Ansatzes einen einfachen Mesos Cluster mit einer eigenständigen Master VM einen VM Skalierungsgruppe basierten Cluster VMs verwaltet.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json) Zukünftige Versionen von [Azure Container Service](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) werden weitere komplexe/abgesichert Versionen dieses Szenarios auf VM Maßstab bereitstellen.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>VM legen Sie Performance und Skalierung Anleitung

- Erstellen Sie jeweils nicht mehr als 500 VMs in mehreren VM Maßstab legt.
- Planen Sie nicht mehr als 20 VMs pro Konto ( _viel_ -Eigenschaft auf "False" festlegen, wobei Sie gehen können bis zu 40).
- Die ersten Buchstaben des Storage Namen möglichst verteilt.  Beispiel VMSS Vorlagen in [Azure Schnellstart Vorlagen](https://github.com/Azure/azure-quickstart-templates/) Beispiele dazu.
- Wenn benutzerdefinierte VMs mit nicht mehr als 40 VMs Pro Skalierungsgruppe VM in ein einzelnes Speicherkonto planen.  Sie benötigen das Bild vor VM Satz Bereitstellung bereits im Speicherkonto kopiert. Finden Sie in den häufig gestellten Fragen Weitere Informationen.
- Planen Sie nicht mehr als 4096 VMs pro VNET.
- Die Anzahl der VMs erstellen Sie bereitstellen vom Kern Kontingent im Bereich beschränkt. Möglicherweise müssen Kundensupport erhöhen die Compute Kontingentgrenze erhöht, auch wenn Sie eine Obergrenze der Kerne für Cloud-Services oder IaaS v1 heute. Ihr Kontingent Abfragen können folgenden Azure CLI-Befehl ausführen: `azure vm list-usage`, und dem folgenden PowerShell-Befehl: `Get-AzureRmVMUsage` (Wenn eine Version von PowerShell 1.0 folgenden `Get-AzureVMUsage`).

## <a name="vm-scale-set-frequently-asked-questions"></a>VM Maßstab häufig gestellte Fragen

**Q.** Wie viele VMs können Sie in einer VM skalieren?

**A.** 100, wenn Sie Plattform-Images verwenden, die mehrere Storage-Konten verteilt werden können. Verwenden Sie benutzerdefinierte Bilder bis zu 40 (Wenn _viel_ -Eigenschaft standardmäßig auf "false" 20 festgelegt ist), seit sind benutzerdefinierte Bilder derzeit auf ein einzelnes Speicherkonto.

**F:** welche anderen Ressourcengrenzen für VM Maßstab legt vorhanden?

**A.** Sie sind auf 500 VMs in mehrere Skalierung Sätze pro Zeitraum 10 Minuten erstellen. Die vorhandenen [Azure Abonnementdienst Grenzwerte /](../azure-subscription-service-limits.md) anwenden.

**Q.** Sind Daten Datenträger unterstützt innerhalb VM Dezimalstellen Mengen

**A.** In der ersten Version. Optionen zum Speichern von Daten sind:

- Azure Dateien (SMB freigegebene Laufwerke)

- OS-Laufwerk

- TEMP-Laufwerk (lokal, nicht unterstütztes Azure-Speicher)

- Azure Datendienst (Azure-Tabellen, Azure-Blobs)

- Externe Datendienst (z. B. remote DB)

**Q.** Der Azure Bereiche unterstützen VM skalieren?

**A.** Jede Azure-Ressourcen-Manager unterstützt Region unterstützt VM skalieren.

**Q.** Wie erstellen Sie einen VM Maßstab Verwendung eines benutzerdefinierten Bildes?

**A.** Lassen Sie die VhdContainers-Eigenschaft leer, beispielsweise:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": "https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd"
            },
            "osType": "Windows"
        }
    },


**Q.** Wenn ich reduzieren Skalierung Meine VM Kapazität von 20 auf 15 VMs entfernt werden?

**A.** Virtuelle Computer werden aus der Maßstab gleichmäßig über Upgrades und Fehlertoleranz Domänen Verfügbarkeit maximieren entfernt. VMs mit der höchsten IDs werden zuerst entfernt.

**Q.** Wie wäre es, wenn ich die Kapazität von 15 bis 18 erhöhen?

**A.** 18 Kapazitätserweiterung werden 3 neue VMs erstellt. Jedes Mal wird die VM-Id aus der bisher höchste Wert (20, 21, 22) erhöht. VMs werden FDs und UDs verteilt.

**Q.** Wenn mehrere Erweiterungen in einer VM Skalierung verwenden, kann ich eine Reihenfolge erzwingen?

**A.** Nicht direkt, sondern für die Erweiterung CustomScript Skript könnte eine andere Erweiterung ([z. B. durch Überwachung das Erweiterung Log](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)) abgeschlossen warten. Zusätzliche Hinweise auf Erweiterung Sequenzierung finden Sie in diesem Blogbeitrag: [Erweiterung Sequenzierung in Azure VM Maßstab legt](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**Q.** Funktionieren VM Maßstab legt mit Azure Verfügbarkeit?

**A.** Ja. Ein VM Skalierung ist eine implizite Verfügbarkeit mit 5 FDs und 5 UDs festgelegt. Sie müssen nicht alles unter VirtualMachineProfile konfigurieren. In zukünftigen Versionen VM Skala sind wahrscheinlich mehrere Mandanten umfassen, aber jetzt eine Skala ist eine einzelne verfügbarkeitsgruppe.
