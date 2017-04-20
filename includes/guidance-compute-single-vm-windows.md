Dieser Artikel beschreibt eine Reihe von bewährten Methoden für das Ausführen einer Windows Virtual Machine (VM) in Azure auf Skalierbarkeit, Verfügbarkeit, Verwaltung und Sicherheit. 

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: [Azure Resource Manager] [ resource-manager-overview] und classic. In diesem Artikel verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt.

Wir empfehlen nicht mit einer einzigen VM für Produktions-Workloads nämlich keine Zeit Vereinbarung zum Servicelevel (SLA) für die einzelnen VMs auf Azure. Zu den SLA müssen mehrere VMs in eine [Verfügbarkeit]bereitstellen[availability-set]. Weitere Informationen finden Sie unter [mehrere Windows-VMs auf Azure ausführen][multi-vm]. 

## <a name="architecture-diagram"></a>Architekturdiagramm

VM in Azure-Bereitstellung umfasst mehr bewegliche Teile als selbst VM. Gibt es Elemente berechnen, Netzwerke und Speicher.

> Ein Visio-Dokument, das diese Architekturdiagramm enthält steht auf der [Microsoft download Center]zum Download[visio-download]. Dieses Diagramm ist "Compute - VM Seite.

![[0]][0]


- **Ressourcengruppe.** Eine [_Ressourcengruppe_] [ resource-manager-overview] ist ein Container, der zugehörige Ressourcen enthält. Erstellen Sie eine Ressourcengruppe, die Ressourcen für diese VM enthält.

- **VM**. Sie können einen virtuellen Computer aus einer Liste von veröffentlichten Bildern oder eine virtuelle Festplatte (VHD), die in Azure BLOB-Speicher hochladen bereitstellen.

- **Betriebssystem-Datenträger.** Betriebssystem-Datenträger ist eine virtuelle Festplatte in [Azure-Speicher]gespeicherte[azure-storage]. Das bedeutet, dass es weiterhin auftritt, wenn der Hostcomputer ausfällt.

- **Temporärer Speicherplatz.** Die VM wird mit einem temporären Datenträger erstellt (die `D:` Laufwerk unter Windows). Datenträger wird auf einem physikalischen Laufwerk auf dem Host gespeichert. Es ist _nicht_ in Azure-Speicher gespeichert und kann während des Neustarts und andere Lebenszyklusereignisse VM verschwinden. Verwenden Sie diese Diskette nur für temporäre Daten wie Seite oder austauschen.

- **Datenträger.** [Datenträger] [ data-disk] einer permanenten VHD für Daten verwendet wird. Datenträger werden in Azure-Speicher, wie das Betriebssystem gespeichert.

- **Virtuelles Netzwerk (VNet) und Subnetz.** Jede VM in Azure wird in einem VNet bereitgestellt, weiter in Teilnetze unterteilt.

- **Öffentliche IP-Adresse.** Eine öffentliche IP-Adresse zur Kommunikation mit der VM benötigt&mdash;beispielsweise über Remotedesktop (RDP).

- **Netzwerkschnittstelle (NIC)**. Die NIC ermöglicht die VM mit dem virtuellen Netzwerk kommunizieren.

- **Netzwerk-Sicherheitsgruppe (NSG)**. [NSG] [ nsg] dient zum Zulassen/Verweigern von Netzwerkverkehr an das Subnetz. Sie können eine NSG mit einer einzelnen NIC oder ein Subnetz zuordnen. Wenn Sie mit einem Subnetz zuordnen, gelten die NSG Regeln für alle virtuellen Computer in diesem Subnetz.
 
- **Diagnose.** Diagnoseprotokolle ist entscheidend für die Verwaltung und Problembehandlung der VM.

## <a name="recommendations"></a>Empfehlung

Azure bietet viele verschiedene Ressourcen und Ressourcentypen, sodass diese Referenzarchitektur kann viele verschiedene Arten bereitgestellt. Wir haben eine Vorlage Azure-Ressourcen-Manager um die Referenzarchitektur installieren, die diese Empfehlung folgt bereitgestellt. Wenn Sie eigene Referenzarchitektur erstellen diese Empfehlung Sie befolgen nur eine bestimmte Anforderung, die Empfehlung nicht passen.

### <a name="vm-recommendations"></a>VM-Empfehlung

Wir empfehlen DS und GS-Serie, da diese Computer [Premium-Speicher]unterstützen[premium-storage]. Wählen Sie eine dieser Größen Computer nur spezielle Aufgaben wie High Performance computing. Details finden Sie in der [Größe der virtuellen][virtual-machine-sizes]. Beim Verschieben einer vorhandenen Arbeitslast in Azure beginnen Sie mit VM-Größe, die der lokale Server am nächsten kommt. Dann die Leistung Ihrer eigentlichen Arbeit in Bezug auf CPU, Arbeitsspeicher und Festplatte e/a-Operationen pro Sekunde (IOPS) und bei Bedarf die Größe anpassen. Benötigen Sie mehrere Netzwerkkarten, auch über NIC Grenze für jede Größe.  

Wenn Sie den virtuellen Computer und andere Ressourcen bereitstellen, müssen Sie einen Speicherort angeben. Im Allgemeinen wählen Sie, Ihre interne Benutzer oder Kunden am nächsten. Allerdings können nicht alle VM, an allen Standorten verfügbar. Details finden Sie unter [Services nach Region][services-by-region]. Führen Sie zum Auflisten der VM Größen an einem bestimmten Ort Azure Befehlszeilenschnittstelle (CLI) Folgendes:

```
    azure vm sizes --location <location>
```

Informationen zum Auswählen einer veröffentlichten VM-Image finden Sie unter [Navigieren und wählen Sie virtuellen Computerimages Azure][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Festplatten- und Vorschläge

Festplatte aus Leistungsgründen empfohlen [Storage Premium][premium-storage], die Daten auf solid-State-Laufwerken (SSDs) speichert. Kosten basiert auf der Größe des bereitgestellten Datenträgers. IOPS wie Durchsatz hängen Festplattenspeicher so bereitstellen eine Festplatte berücksichtigen alle drei Faktoren (Kapazität IOPS und Durchsatz). 

Ein Speicherkonto kann 1 bis 20 VMs unterstützen.

Fügen Sie ein oder mehrere Datenträger. Wenn Sie eine neue virtuelle Festplatte erstellen, ist unformatiert. Melden Sie sich bei VM zu formatieren.

Haben Sie eine große Anzahl von Datenträgern werden Sie die gesamte e/a-Grenzen des Speicherkontos. Weitere Informationen finden Sie in der [Virtual Machine Grenzen][vm-disk-limits].

Für eine optimale Leistung Erstellen einer getrennten Konto Diagnoseprotokolle enthalten. Diagnoseprotokolle genügt ein Standardbenutzerkonto lokal redundanten Speicher (LRS).

Wenn möglich, installieren Sie Applikationen auf Datenträger nicht die Betriebssystem-Datenträger. Jedoch müssen einige Legacyanwendungen Komponenten auf Laufwerk C: installieren. In diesem Fall können Sie [die Betriebssystemdatenträger Größe] [ resize-os-disk] mithilfe von PowerShell.

### <a name="network-recommendations"></a>Netzwerk-Empfehlung

Die öffentliche IP-Adresse kann dynamisch oder statisch sein. Der Standardwert ist dynamisch.

- Reservieren Sie eine [statische IP-Adresse] [ static-ip] benötigen Sie eine feste IP-Adresse, die nicht ändern &mdash; beispielsweise einen A-Eintrag in DNS erstellt werden sollen oder muss die IP-Adresse auf der weißen Liste zu.

- Sie können auch einen vollqualifizierten Domänennamen (FQDN) die IP-Adresse erstellen. Sie können dann einen [CNAME-Eintrag] registrieren[ cname-record] in DNS auf den vollqualifizierten Domänennamen. Weitere Informationen finden Sie unter [Erstellen einer vollqualifizierten Domänennamen im Azure-Portal][fqdn].

Alle NSGs enthalten eine Reihe von [Standardregeln][nsg-default-rules], einschließlich einer Regel, die alle eingehenden Internet-Datenverkehr blockiert. Die Regeln können nicht gelöscht werden, jedoch andere Regeln überschreiben können. Aktivieren Sie Internetverkehr Regeln erstellen, die eingehenden für bestimmte Ports Datenverkehr &mdash; z. B. port 80 für HTTP.  

Um RDP zu aktivieren, fügen Sie eine NSG Regel eingehenden Datenverkehr an TCP-Port 3389.

## <a name="scalability-considerations"></a>Aspekte der Skalierung

Sie können eine VM oben oder unten skalieren, durch [Ändern der Textgröße VM][vm-resize]. 

Zum horizontalen Skalieren, hinter einem Lastenausgleich Verfügbarkeit setzen zwei oder mehrere VMs. Details finden Sie unter [mehrere Windows-VMs auf Azure ausführen][multi-vm].

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

Wie bereits erwähnt, gibt es kein SLA für eine VM. Um die SLA zu erhalten, müssen Sie mehrere VMs an Verfügbarkeit bereitstellen.

VM kann durch [geplante Wartung] beeinflusst[ planned-maintenance] oder [nicht geplanten Wartung][manage-vm-availability]. [VM starten Protokolle] können[ reboot-logs] bestimmen, ob ein Neustart VM Wartungsarbeiten zurückzuführen.

VHDs von [Azure-Speicher]gesichert[azure-storage], die für Langlebigkeit und Verfügbarkeit repliziert wird.

Schutz gegen Datenverlust während des normalen Betriebs (z. B. wegen Benutzerfehler), sollten Sie auch Point-in-Time-Backups mit [BLOB-Snapshots] implementieren[ blob-snapshot] oder einem anderen Tool.

## <a name="manageability-considerations"></a>Aspekte der Verwaltung

**Ressourcengruppen.** Eng verknüpft Ressourcen, die denselben Lebenszyklus in derselben [Ressourcengruppe]verwenden[resource-manager-overview]. Ressourcengruppen können bereitstellen und Überwachen von Ressourcen als Gruppe und Abrechnung Kosten von Ressourcengruppe Rollup. Sie können auch Ressourcen als Gruppe ist sehr hilfreich bei der Bereitstellung testen. Geben Sie Ressourcen aussagekräftige Namen. Das erleichtert das Suchen einer bestimmten Ressource und seine Rolle. [Empfohlene Namenskonventionen für Azure-Ressourcen]finden Sie unter[naming conventions].

**VM-Diagnose.** Überwachung aktivieren und Diagnose sowie grundlegende Zustandsmetriken, Infrastruktur Diagnoseprotokolle [Boot Diagnose][boot-diagnostics]. Boot-Diagnose können Sie Ihre virtuellen Computer in einer nicht startfähigen Zustand wird ein Startfehler analysieren. Weitere Informationen finden Sie unter [Überwachung und Diagnose][enable-monitoring]. Verwenden der [Azure-Protokollsammlung] [ log-collector] Erweiterung Azure-Plattform Protokolle sammeln und Azure-Speicher hochgeladen.   

Der folgende CLI-Befehl ermöglicht Diagnose:

```
    azure vm enable-diag <resource-group> <vm-name>
```

**Beenden einen virtuellen Computer.** Azure unterscheidet zwischen "Beendet" und "Deallocated". Sie Zahlen der VM-Status "beendet". Sie werden nicht berechnet, wenn die VM freigegeben.

Verwenden Sie folgenden CLI-Befehl eine VM freigeben:

```
    azure vm deallocate <resource-group> <vm-name>
```

Die Schaltfläche **Beenden** in Azure-Portal gibt auch die VM. Jedoch wenn Sie über das Betriebssystem herunterfahren angemeldet, wird die VM beendet aber _nicht_ freigegeben, so dass Sie weiterhin berechnet.

**Löschen einen virtuellen Computer.** Wenn Sie einen virtuellen Computer löschen, werden die VHDs nicht gelöscht. Das bedeutet, dass die VM sicher ohne Datenverlust gelöscht werden können. Aber Sie weiterhin für Speicher berechnet. Um die virtuelle Festplatte zu löschen, löschen Sie die Datei aus dem [BLOB-Speicher][blob-storage].

Um versehentliches Löschen zu verhindern, verwenden Sie eine [Sperre für Ressource] [ resource-lock] die gesamte Ressource Gruppe oder Sperren einzelnen Ressourcen wie die VM gesperrt. 

## <a name="security-considerations"></a>Sicherheitsaspekte

Verwenden von [Azure Security Center] [ security-center] eine zentrale Ansicht des Sicherheitsstatus Ihrer Azure Ressourcen abrufen. Sicherheitscenter überwacht Probleme wie, Antimalware aktualisiert, und einen umfassenden Überblick über den Sicherheitsstatus Ihrer Bereitstellung bietet. 

- Das Sicherheitscenter ist pro Azure-Abonnement konfiguriert. Zulassen Sie Datensammlung Sicherheit unter [Verwendung Security Center]

- Bei aktiviertem Datensammlung scannt Sicherheitscenter automatisch VMs unter dieses Abonnement erstellt.

**Patch-Management.** Wenn aktiviert, prüft das Sicherheitscenter Sicherheitsupdates und Updates fehlen. Verwenden Sie [Gruppenrichtlinien] [ group-policy] auf dem virtuellen Computer automatische System-Updates aktivieren.

**Antimalware.** Wenn aktiviert, prüft Sicherheitscenter Antimalware-Software installiert ist. Security Center können Sie Antischadsoftware in Azure-Portal installieren.

[Rollenbasierte Zugriffskontrolle] verwenden[ rbac] (RBAC) Zugriff auf den Azure-Ressourcen steuern, die Sie bereitstellen. RBAC können Sie Teammitgliedern DevOps Autorisierungsrollen zuweisen. Z. B. kann Rolle Azure Ressourcen jedoch nicht erstellen, verwalten und löschen. Einige Rollen sind spezifisch für bestimmte Typen von Azure. Z. B. kann Virtual Machine Teilnehmerrolle neu starten oder Freigeben eine VM, Zurücksetzen des Administratorkennworts, einen neuen virtuellen Computer erstellen, usw. Andere [integrierte RBAC-Rollen] [ rbac-roles] , die möglicherweise nützlich für diese Referenzarchitektur enthalten [DevTest Labs Benutzer] [ rbac-devtest] und [Netzwerk-Teilnehmer][rbac-network]. Benutzer kann mehreren Rollen zugewiesen werden, und erstellen Sie benutzerdefinierte Rollen für noch mehr abgestimmte Berechtigungen.

> [AZURE.NOTE] RBAC schränkt nicht die Aktionen, die Benutzer in einer VM ausführen können. Die Kontenart für das Gastbetriebssystem diese Berechtigungen bestimmt.   

Um das lokale Administratorkennwort zurückzusetzen, führen Sie die `vm reset-access` Azure CLI-Befehl.

```
    azure vm reset-access -u <user> -p <new-password> <resource-group> <vm-name>
```

Verwenden Sie [Überwachungsprotokolle] [ audit-logs] Bereitstellung von Aktionen und andere VM-Ereignisse anzeigen.

Betrachten Sie [Azure Disk Encryption] [ disk-encryption] benötigen Sie Festplatten Betriebssystem und Daten verschlüsseln. 

## <a name="solution-deployment"></a>Bereitstellung der Lösung

[Bereitstellung] [ github-folder] für eine Architektur, die diese best Practices veranschaulicht ist verfügbar. Diese Referenzarchitektur enthält ein virtuelles Netzwerk (VNet), Netzwerk-Sicherheitsgruppe (NSG) und einer einzelnen virtuellen Maschine (VM).

Es gibt verschiedene Arten dieser Referenzarchitektur bereitstellen. Die einfachste Möglichkeit ist, die folgenden Schritte: 

1. Rechts klicken und entweder "Link öffnen in neuem Tab" oder "Link in neuem Fenster öffnen".  
[![In Azure bereitstellen](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)

2. Sobald die Verknüpfung im Azure-Portal geöffnet hat, geben Sie Werte für einige der Einstellungen: 
    - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert, wählen Sie **Neu erstellen** und geben Sie `ra-single-vm-rg` im Textfeld.
    - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
    - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
    - Das Dropdown-Feld **Windows** **Betriebssystem** auswählen.
    - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
    - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Warten Sie, bis die Bereitstellung abgeschlossen.

4. Parameterdateien enthalten einen hartcodierten Administrator-Benutzernamen und ein Kennwort, und es wird dringend empfohlen, dass Sie beide sofort ändern. Klicken Sie auf den virtuellen Computer mit dem Namen `ra-single-vm0 `im Azure-Portal. Klicken Sie auf **Kennwort zurücksetzen** **Support + Problembehandlung** Blatt. Wählen Sie **Kennwort zurücksetzen** im Feld Dropdown- **Modus** und anschließend einen neuen **Benutzernamen** und **ein Kennwort**. Klicken Sie auf die Schaltfläche **Aktualisieren** , um den neuen Benutzernamen und das Kennwort beibehalten.

Informationen über weitere Methoden zum Bereitstellen dieser Referenzarchitektur, lesen Sie die Infodatei in der [Anleitung einzelnen Vm][github-folder]] Github Ordner. 

## <a name="customize-the-deployment"></a>Anpassen der Bereitstellung

Möchten Sie die Bereitstellung Ihren Bedürfnissen entsprechend ändern, gehen Sie in der [Infodatei][github-folder]. 

## <a name="next-steps"></a>Nächste Schritte

Reihenfolge für die [SLA für virtuelle Maschinen] [ vm-sla] anzuwenden, müssen Sie zwei oder mehr Instanzen in einem Verfügbarkeit bereitstellen. Weitere Informationen finden Sie unter [mehrere VMs auf Azure ausführen][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-windows-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-windows-portal-create-fqdn.md
[github-folder]: http://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm
[group-policy]: https://technet.microsoft.com/en-us/library/dn595129.aspx
[log-collector]: https://azure.microsoft.com/en-us/blog/simplifying-virtual-machine-troubleshooting-using-azure-log-collector/
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-windows-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-windows-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[resize-os-disk]: ../articles/virtual-machines/virtual-machines-windows-expand-os-disk.md
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[security-center]: https://azure.microsoft.com/en-us/services/security-center/
[select-vm-image]: ../articles/virtual-machines/virtual-machines-windows-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[Verwenden Sie das Sicherheitscenter]: ../articles/security-center/security-center-get-started.md#use-security-center
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-windows-sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[0]: ./media/guidance-blueprints/compute-single-vm.png "Einzelne Windows VM-Architektur in Azure"
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[blocks]: https://github.com/mspnp/template-building-blocks
