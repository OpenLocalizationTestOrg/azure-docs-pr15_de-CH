<properties
   pageTitle="Mehrere virtuelle Computer ausführen | Referenzarchitektur | Microsoft Azure"
   description="Mehrere VM-Instanzen Ausführen von auf Azure Skalierbarkeit, Stabilität, Verwaltung und Sicherheit."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/19/2016"
   ms.author="mwasson"/>

# <a name="running-multiple-vms-on-azure-for-scalability-and-availability"></a>Mehrere VMs ausgeführte für Skalierbarkeits- und auf Windows Azure 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel beschreibt eine Reihe von bewährten Methoden für das Ausführen mehrerer Instanzen von virtuellen Maschinen (VM) zur Verbesserung der Skalierbarkeit, Verfügbarkeit, Verwaltung und Sicherheit.   

In dieser Architektur wird die Arbeitslast auf die VM-Instanzen verteilt. Gibt es eine einzelne öffentliche IP-Adresse und Internet-Datenverkehr auf VMs mit Lastenausgleichsfunktion verteilt. Diese Architektur kann für eine Single-Tier-Anwendung wie eine statusfreie Anwendung oder Speicher Webcluster verwendet werden. Es ist auch ein Baustein für N-Tier Applications. 

Dieser Artikel baut auf [Einer einzigen VM in Azure][single vm]. Die Ratschläge in diesem Artikel gelten auch für diese Architektur.

## <a name="architecture-diagram"></a>Architekturdiagramm

VMs in Azure benötigen, Netzwerk-und Speicherressourcen unterstützt.

> Ein Visio-Dokument, das diese Architekturdiagramm enthält steht auf der [Microsoft download Center]zum Download[visio-download]. Dieses Diagramm ist "Compute - Multi VM Registerkarte." 

![[0]][0]

Die Architektur besteht aus folgenden Komponenten: 

- **Verfügbarkeit festgelegt.** [Verfügbarkeit] [ availability set] enthält die VMs und ist erforderlich für die Unterstützung der [Verfügbarkeits-SLA für Azure VMs][vm-sla].

- **VNet**. Jede VM in Azure wird in einem virtuellen Netzwerk (VNet) bereitgestellt, das wiederum in **Subnetze**unterteilt.

- **Azure Lastenausgleich.** Der [Lastenausgleich] verteilt eingehende Internetanfragen VM-Instanzen in einem Verfügbarkeit. Lastenausgleich enthält verwandten Ressourcen:

  - **Öffentliche IP-Adresse.** Für den Lastenausgleich Internetverkehr empfangen ist eine öffentliche IP-Adresse erforderlich.

  - **Front-End-Konfiguration.** Lastenausgleich die öffentliche IP-Adresse zugeordnet.

  - **Back-End-Adresspool.** Enthält die Netzwerkschnittstellen (NICs) für die virtuellen Computer, die den eingehenden Datenverkehr empfangen.

- **Laden Sie Balancer Regeln.** Verwendet, um den Netzwerkverkehr zwischen den virtuellen Computern in der Back-End-Adresspool verteilen. 

- **NAT-Regeln.** Datenverkehr einer bestimmten VM verwendet. Erstellen Sie beispielsweise aktivieren Sie remote desktop Protocol (RDP) auf die VMs separate Adresse (Netzwerkadressübersetzung) Netzwerkregel für jeden virtuellen Computer. 

- **Netzwerkschnittstellen (NICs)**. Jede VM besitzt eine Netzwerkkarte mit dem Netzwerk herstellen.

- **Speicher.** Speicherkonten enthalten die VM Bilder und andere Datei bezogene Ressourcen wie VM diagnostische Daten Azure.

## <a name="recommendations"></a>Empfehlung

Azure bietet viele verschiedene Ressourcen und Ressourcentypen, sodass diese Referenzarchitektur kann viele verschiedene Arten bereitgestellt. Wir haben eine Vorlage Azure Resource Manager installieren die Referenzarchitektur, die nachstehenden Vorschläge bereitgestellt. Wenn Sie eigene Referenzarchitektur erstellen diese Empfehlung befolgen Sie nur eine bestimmte Anforderung, die Empfehlung nicht unterstützt. 

### <a name="availability-set-recommendations"></a>Verfügbarkeit festlegen recommendations

Mindestens zwei VMs müssen zur Unterstützung der [Verfügbarkeits-SLA für Azure VMs]Verfügbarkeit erstellen[vm-sla]. Beachten Sie, dass Azure Lastenausgleich muss auch dieselben Verfügbarkeit Lastenausgleich VMs angehören.

### <a name="network-recommendations"></a>Netzwerk-Empfehlung

Alle sollte VMs hinter den Lastenausgleich im selben Subnetz platziert. Machen Sie die virtuellen Computer direkt mit dem Internet, aber stattdessen Geben Sie jede VM eine private IP-Adresse. Clients verbinden die öffentliche IP-Adresse des Lastenausgleich.

### <a name="load-balancer-recommendations"></a>Load Balancer recommendations

Hinzufügen alle VMs in der Back-End-Adresspool des Lastenausgleich festgelegt.

Direkte Netzwerkverkehr VMs definieren Sie Load Balancer Regeln. Beispielsweise HTTP-Datenverkehr zu erstellen eine Regel, die Port 80 von Front-End-Konfiguration auf Port 80 auf dem Back-End-Adresspool zuordnet. Beim Lastenausgleich eine Anforderung an Port 80 der öffentlichen IP-Adresse empfängt, leitet er die Anforderung an Port 80 auf die NICs im Back-End-Adresspool.

Regeln Sie NAT-Datenverkehr einer bestimmten VM. Z. B. damit RDP auf die VMs erstellen eine separate NAT-Regel für jeden virtuellen Computer. Jede Regel sollten unterschiedliche Anschlussnummer auf Port 3389, der Standardport für RDP zuordnen. (Z. B. verwenden Sie Port 50001 für "VM1", Port 50002 für "VM2" usw.) Netzwerkkarten auf dem virtuellen Computer die NAT-Regeln zuweisen. 

### <a name="storage-account-recommendations"></a>Storage-Konto empfohlen

Azure getrennten Konten für jede VM zu virtuellen Festplatten (VHDs) um vermeiden Eingabe-/Ausgabeoperationen pro zweite [(IOPS) Grenzen] [ vm-disk-limits] für Speicherkonten. 

Erstellen Sie ein Speicherkonto Diagnoseprotokollen. Dieses Speicherkonto kann von den VMs freigegeben werden.

## <a name="scalability-considerations"></a>Aspekte der Skalierung

Es gibt zwei Optionen für die Skalierung von VMs in Azure: 

- Verwenden Sie ein Lastenausgleich Netzwerkverkehr über eine Gruppe von VMs verteilen. Skalieren, zusätzlichen virtuellen Computer bereitstellen und hinter dem Lastenausgleich. 

- [Skalierung]verwenden virtuellen[vmss]. Eine Skalierung enthält mehrere identische VMs hinter einem Lastenausgleich angegeben. VM-Skala wird unterstützt automatische Skalierung basierend auf Leistungsindikatoren. Mit steigender Last auf VMs werden zusätzliche VMs Lastenausgleich automatisch hinzugefügt. 

In den nächsten Abschnitten vergleichen dieser beiden Optionen

### <a name="load-balancer-without-vm-scale-sets"></a>Lastenausgleich ohne VM Maßstab legt

Ein Lastenausgleich nimmt Netzwerkanfragen und verteilt über Netzwerkkarten im Back-End-Adresspool. Zum horizontalen Skalieren fügen Sie weitere VM Instanzen der Verfügbarkeit hinzu oder freigeben Sie VMs verkleinern. 

Angenommen Sie, dass Sie einen Webserver ausführen. Sie würden eine Load Balancer Regel für Port 80 und Port 443 (SSL) hinzufügen. Wenn ein Client eine HTTP-Anforderung sendet, wählt Lastenausgleich eine Back-End-IP-Adresse über einen [Hashalgorithmus] [ load balancer hashing] enthält die IP-Quelladresse. Auf diese Weise werden die VMs Clientanforderungen verteilt. 

> [AZURE.TIP] Beim Hinzufügen einer neuen VM zu einer festzulegen, unbedingt eine Netzwerkkarte für den virtuellen Computer erstellen und die NIC Backend-Adresspool auf dem Lastenausgleich hinzufügen. Andernfalls wird nicht zur neuen VM Internetverkehr weitergeleitet.

Jede Azure-Abonnement hat Standardgrenzwerte, darunter eine Höchstanzahl von VMs pro Region. Unterstützung Antrag, um den Grenzwert zu erhöhen. Weitere Informationen finden Sie unter [Azure-Abonnement und Service Grenzen, Kontingente und Einschränkungen][subscription-limits].  

### <a name="vm-scale-sets"></a>Die Skalierung legt VM 

VM Maßstab legt Hilfe bereitstellen und Verwalten einer identischen VMs. Alle virtuellen Computer identisch konfiguriert, VM Maßstab legt true skalieren ohne Vorabbereitstellung VMs erleichtert umfangreiche Services für große Compute, big Data und Container Arbeitslasten zu unterstützen. 

Für Weitere Informationen über VM Skalierung Übersicht [Virtual Machine Maßstab legt][vmss].

Mit VM Skalierung Aspekte:

- Betrachten Sie Maßstab legt ggf. schnell VMs skalieren oder skalieren müssen. 

- Maßstab legt unterstützt Datenträger derzeit nicht. Die Optionen zum Speichern von Daten sind Azure Dateispeicher, OS-Laufwerk, das Temp-Laufwerk oder einem externen Speicher wie Azure-Speicher. 

- VM-Instanzen innerhalb einer automatisch dieselben von Verfügbarkeit mit 5 Fehler und 5 updatedomänen gehören.

- Standardmäßig verwenden Maßstab legt "overprovisioning", also die Skalierungsgruppe zunächst Vorschriften mehr VMs für bitten, der dann zusätzliche VMs. Dies verbessert die Erfolgsquote beim virtuellen Computer bereitstellen. 

- Wir empfehlen nicht mehr dann als 20 VMs pro Speicher mit Bereitstellung überproportional vieler aktiviert oder nicht mehr als 40 VMs mit Bereitstellung überproportional vieler Konto deaktiviert.  

- Ressourcen-Manager Vorlagen finden bei Bereitstellung Skalierung in [Azure Schnellstart Vorlagen][vmss-quickstart].

- Es gibt zwei grundlegende Verfahren zum Konfigurieren von VMs in einer Skala bereitgestellt: Erstellen eines benutzerdefinierten Bildes oder Extensions verwenden, um die VM konfigurieren, nachdem sie bereitgestellt wurde.

    - Skalierung müssen auf ein benutzerdefiniertes Abbild erstellt alle OS Festplatte VHDs in ein Speicherkonto erstellen. 

    - Benutzerdefinierte Bilder müssen Sie das Bild auf dem Laufenden halten.

    - Mit können sie für eine neu eingerichtete VM Hochfahren länger.

Weitere Hinweise finden Sie unter [Entwerfen VM Maßstab legt für Skalierung][vmss-design].

> [AZURE.TIP]  Bei jeder Lösung automatische Skalierung testen Sie mit Produktion auf Anwendungsebene im voraus. 

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

Ist Set app stabiler bei geplanten und ungeplanten Wartungsereignisse.

- _Geplante Wartung_ tritt auf, wenn Microsoft die zugrunde liegende Plattform aktualisiert verursachen manchmal VMs neu gestartet werden. Azure stellt sicher die VMs in einem Verfügbarkeit nicht alle gleichzeitig gestartet, mindestens werden ausgeführt, während andere neu gestartet.

- _Nicht geplante Wartung_ geschieht, wenn ein Hardwarefehler vorliegt. Azure stellt sicher, dass virtuelle Computer in einem Verfügbarkeit über mehrere Server-Racks bereitgestellt werden. Dadurch Reduzierung der Hardware-Ausfälle, Netzwerkausfälle, Strom unterbrochen und so weiter.

Weitere Informationen finden Sie unter [Verwalten der Verfügbarkeit virtueller Maschinen][availability set]. Das folgende Video hat auch einen guten Überblick über die Verfügbarkeit legt: [Gewusst wie konfiguriere eine Verfügbarkeit soll Skalierung VMs][availability set ch9]. 

> [AZURE.WARNING]  Stellen Sie sicher konfigurieren die Verfügbarkeit festgelegt, wenn Sie den virtuellen Computer bereitstellen. Derzeit besteht keine Möglichkeit, eine Verfügbarkeit festgelegt, nachdem die VM bereitgestellt Ressourcenmanager VM hinzufügen.

Lastenausgleich überwacht [Zustand überprüft] die Verfügbarkeit von VM-Instanzen. Wenn ein Prüfpunkt eine Instanz innerhalb einer bestimmten Zeit erreicht werden kann, stoppt Lastenausgleich Senden von Datenverkehr an diesen virtuellen Computer. Allerdings Lastenausgleich weiterhin Prüfpunkt und wenn die VM wieder verfügbar ist, Lastenausgleich setzt Senden von Datenverkehr an diesen virtuellen Computer.

Hier sind einige Vorschläge für Load Balancer Health Prüfpunkte:

- Prüfpunkte können HTTP oder TCP testen. Wenn VMs ausgeführt einem HTTP-Server (IIS, Nginx, Node.js app usw.) eine HTTP-Prüfpunkt erstellen. Andernfalls erstellen Sie einen TCP-Prüfpunkt.

- Geben Sie den Pfad an den HTTP-Endpunkt für einen Prüfpunkt HTTP. Der Prüfpunkt überprüft eine HTTP 200 Antwort aus diesem Pfad. Kann die Root-Pfad ("/") oder einen Endpunkt Systemüberwachungsereignissen, der benutzerdefinierte Logik zum Überprüfen der Integrität der Anwendung implementiert. Der Endpunkt muss anonyme HTTP-Anfragen zulassen.

- Der Prüfpunkt geht von einer [bekannten] [ health-probe-ip] IP-Adresse 168.63.129.16. Stellen Sie sicher, dass Datenverkehr von dieser IP-Firewall-Richtlinien oder Netzwerksicherheitsregeln Group (NSG) nicht blockiert.

- [Gesundheit Prüfpunkt] Protokolle[ health probe log] den Status der Prüfpunkte Zustand anzeigen. Aktivieren der Protokollierung in Azure-Portal für jedes System zum Lastenausgleich. Protokolle werden in Azure BLOB-Speicher geschrieben. Protokolle anzeigen wie viele VMs auf dem Back-End des Netzwerkverkehrs durch fehlerhaften Prüfpunkt Antworten nicht empfangen.

## <a name="manageability-considerations"></a>Aspekte der Verwaltung

Mit mehreren VMs wichtig sind zuverlässige und wiederholbare Prozesse automatisieren. [Azure Automation] können[ azure-automation] Bereitstellung, BS-Patches und andere Aufgaben automatisieren. Azure-Automatisierung ist ein Automatisierung basiert auf Windows PowerShell, die in Azure ausgeführt wird. Automatisierung Beispielskripts sind auf TechNet [Gallery Runbook] verfügbar.

## <a name="security-considerations"></a>Sicherheitsaspekte

Virtuelle Netzwerke sind eine Isolationsgrenze Datenverkehr in Azure. VMs in einem VNet direkt auf VMs in verschiedenen VNet kommunizieren kann. VMs innerhalb desselben VNet kommunizieren, sofern Sie [Netzwerk-Sicherheitsgruppen] erstellen[ nsg] (NSGs) Datenverkehr einschränken. Weitere Informationen finden Sie im [Microsoft-Cloud-Dienste und Sicherheit][network-security].

Definieren für eingehende Internetverkehr Load Balancer Regeln welche Back-End erreichen. Unterstützt jedoch Load Balancer Regeln nicht IP-Whitelists Wenn eine NSG Whitelist bestimmte öffentliche IP-Adressen, dem Subnetz hinzufügen möchten.

## <a name="solution-deployment"></a>Bereitstellung der Lösung

Bereitstellung für eine Referenzarchitektur, die diese Empfehlung implementiert ist auf GitHub verfügbar. Diese Referenzarchitektur enthält ein virtuelles Netzwerk (VNet), Netzwerk-Sicherheitsgruppe (NSG), Lastenausgleich und zwei virtuellen Maschinen (VMs).

Die Referenzarchitektur kann mit Windows oder Linux VMs anhand der Anweisungen bereitgestellt werden: 

1. Rechten Maustaste unten und wählen Sie entweder "Link öffnen in neuem Tab" oder "Link in neuem Fenster öffnen":  
[![In Azure bereitstellen](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-multi-vm%2Fazuredeploy.json)

2. Sobald die Verknüpfung im Azure-Portal geöffnet hat, geben Sie Werte für einige der Einstellungen: 
    - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert, wählen Sie **Vorhandene verwenden** und geben `ra-multi-vm-rg` im Textfeld.
    - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
    - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
    - Wählen Sie aus dem Dropdown-Feld, **Windows** oder **Linux** **Betriebssystem** . 
    - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
    - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Warten Sie, bis die Bereitstellung abgeschlossen.

4. Parameterdateien enthalten einen hartcodierten Administrator-Benutzernamen und ein Kennwort, und es wird dringend empfohlen, dass Sie beide sofort ändern. Klicken Sie auf den virtuellen Computer mit dem Namen `ra-multi-vm1` im Azure-Portal. Klicken Sie auf **Kennwort zurücksetzen** **Support + Problembehandlung** Blatt. Wählen Sie **Kennwort zurücksetzen** im Feld Dropdown- **Modus** und anschließend einen neuen **Benutzernamen** und **ein Kennwort**. Klicken Sie auf die Schaltfläche **Aktualisieren** , um den neuen Benutzernamen und das Kennwort speichern. Wiederholen Sie dies für die VM mit dem Namen `ra-multi-vm2`.

Informationen über weitere Methoden zum Bereitstellen dieser Referenzarchitektur, lesen Sie die Infodatei in der [Anleitung einzelnen Vm] [ github-folder] GitHub Ordner. 

## <a name="next-steps"></a>Nächste Schritte

Mehrere VMs hinter einem Lastenausgleich ist ein Grundbaustein für Architekturen mit mehreren Ebenen erstellen. Weitere Informationen finden Sie unter [Windows-VMs für N-Tier-Architektur in Azure ausgeführt] [ n-tier-windows] und [Linux VMs für N-Tier-Architektur in Azure ausgeführt][n-tier-linux]

<!-- Links -->
[availability set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[availability set ch9]: https://channel9.msdn.com/Series/Microsoft-Azure-Fundamentals-Virtual-Machines/08
[azure-automation]: https://azure.microsoft.com/en-us/documentation/services/automation/
[azure-cli]: ../virtual-machines-command-line-tools.md
[bastion host]: https://en.wikipedia.org/wiki/Bastion_host
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-multi-vm
[health probe log]: ../load-balancer/load-balancer-monitor-log.md
[Gesundheit Prüfpunkte]: ../load-balancer/load-balancer-overview.md#service-monitoring
[health-probe-ip]: ../virtual-network/virtual-networks-nsg.md#special-rules
[Lastenausgleich]: ../load-balancer/load-balancer-get-started-internet-arm-cli.md
[load balancer hashing]: ../load-balancer/load-balancer-overview.md#hash-based-distribution
[n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[n-tier-windows]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[network-security]: ../best-practices-network-security.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md 
[Runbook Galerie]: ../automation/automation-runbook-gallery.md#runbooks-in-runbook-gallery
[single vm]: guidance-compute-single-vm.md
[subscription-limits]: ../azure-subscription-service-limits.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_2/
[vmss]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md
[vmss-design]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md
[vmss-quickstart]: https://azure.microsoft.com/documentation/templates/?term=scale+set
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[0]: ./media/blueprints/compute-multi-vm.png "Architektur einer Lösung Multi-VM Azure mit Verfügbarkeit ein Lastenausgleich mit zwei VMs festlegen"
