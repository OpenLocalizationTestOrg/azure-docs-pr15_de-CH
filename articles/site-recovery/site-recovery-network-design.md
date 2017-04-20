<properties
    pageTitle="Entwerfen der Netzwerkinfrastruktur für die Disaster Recovery | Microsoft Azure"
    description="Dieser Artikel beschreibt die Vorüberlegungen zum Netzwerkentwurf für Azure Site Recovery"
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/19/2016"
    ms.author="pratshar"/>

#  <a name="designing-your-network-infrastructure-for-disaster-recovery"></a>Entwerfen der Netzwerkinfrastruktur für Disaster recovery

Dieser Artikel richtet sich an IT-Experten für die Architektur, Implementierung und Unterstützung von Business Continuity und Disaster Recovery (BCDR)-Infrastruktur und Microsoft Azure Site Recovery (ASR), nutzen möchten und ihre BCDR Dienste. Dieses Dokument beschreibt praktische Hinweise für die Bereitstellung von System Center Virtual Machine Manager-Server, die vor- und Nachteile der gestreckten Subnetzen und Subnetz Failover und Disaster Recovery für virtuelle Websites in Microsoft Azure-Struktur.

## <a name="overview"></a>Übersicht

[Azure Site Recovery (ASR)](https://azure.microsoft.com/services/site-recovery/) ist Microsoft Azure, die Schutz und Recovery virtualisierten Anwendung für Business Continuity Disaster Recovery (BCDR) koordiniert. Dieses Dokument soll die Leser über die Netzwerke entwerfen Architektur IP-Adressbereiche und Subnetze Disaster Recovery-Standort bei virtuellen Maschinen (VMs) mit Site Recovery konzentrieren.

Dieser Artikel beschreibt außerdem, wie Site Recovery ermöglicht, Architektur und Implementierung mehrerer Standorte virtual Datacenter Unterstützung BCDR Dienste zu testen und Disaster.

In einer Welt, wo alle 24/7 Konnektivität erwartet, ist es wichtiger als je zu Ihrer Infrastruktur und Applikationen und ausgeführt. Business Continuity und Disaster Recovery (BCDR) soll ausgefallene Komponenten Wiederherstellung die Organisation normalen Betrieb schnell fortgesetzt werden kann. Entwickeln Wiederherstellungsstrategien mit unwahrscheinlich, verheerenden Ereignisse ist sehr schwierig. Ist aufgrund der Schwierigkeit der Vorhersage der Zukunft insbesondere betrifft unwahrscheinlich Ereignisse und die hohen Kosten angemessene Maßnahmen gegen weitreichende Katastrophen 

Entscheidend für BCDR Planung müssen Recovery Time Objective (RTO) und Recovery Point Objective (RPO) als Bestandteil eines Wiederherstellungsplans definiert werden. Wenn Kunden Rechenzentrum mit Azure Site Recovery können Kunden schnell (niedrigen RTO) Katastrophe Onlineschalten Sie replizierte virtuelle Computer befindet sich im sekundären Rechenzentrum oder Microsoft Azure mit minimalen Datenverlust (niedriger RPO). 

Failover von ASR erfolgt zunächst kopiert festgelegte virtuelle Computer im primären Rechenzentrum sekundären Rechenzentrum oder Azure (je nach Szenario) und Replikate in regelmäßigen Abständen aktualisiert. Bei der infrastrukturplanung Netzwerkentwurf als potentieller Engpass berücksichtigen, die Sie treffen Unternehmen RTO und RPO-Ziele verhindern können.  

Wenn Administratoren eine Disaster Recovery-Lösung bereitstellen, ist eine der wichtigsten Fragen aufgenommenen wie der virtuelle Computer erreichbar nach Abschluss des Failovers. ASR kann der Administrator das Netzwerk auswählen, ein virtuellen Computer nach einem Failover auf verbunden wird. Wenn der primäre Standort von VMM-Server verwaltet wird dies erreicht mit Netzwerk zuordnen. Weitere Informationen finden Sie in der [Netzwerkübersicht vorbereiten](site-recovery-network-mapping.md) .

Beim Entwerfen des Netzwerkes für Recovery-Standort, hat der Administrator zwei Optionen:

- Verwenden Sie einen anderen IP-Adressbereich für das Netzwerk am Recovery-Standort. In diesem Szenario erhalten die virtuellen Computer nach einem Failover eine neue IP-Adresse und der Administrator muss ein DNS-Update. Lesen Sie mehr zu DNS update [hier](site-recovery-vmm-to-vmm.md#test-your-deployment) 
- Verwenden Sie dieselbe IP-Adressbereich für das Netzwerk am Recovery-Standort. In bestimmten Szenarien bevorzugen Administratoren weiterhin die IP-Adressen, die sie auf dem primären Standort auch nach dem Failover. In einem normalen Szenario müsste ein Administrator Routen an den neuen Speicherort der IP-Adressen zu aktualisieren. Aber im Szenario, wo eine gestreckte VLAN zwischen primären und Recovery-Standorten bereitgestellt wird, attraktiv wird die IP-Adressen für den virtuellen Computer beibehalten. Halten das gleiche IP Adressen erleichtert die Wiederherstellung jedes Netzwerk nehmen Post-Failover Schritte beziehen.


Wenn Administratoren eine Disaster Recovery-Lösung bereitstellen, ist eine der wichtigsten Fragen im Kopf wie Anträge erreichbar werden nach Abschluss des Failovers. Moderne Applikationen sind fast immer Netzwerke zu einem gewissen Grad so physisch verschieben ein Dienst von einem Standort zu einem anderen Netzwerk Herausforderung abhängig. Es gibt zwei Hauptmethoden, die dieses Problem in Disaster Recovery-Lösung behandelt. Der erste Ansatz ist feste IP-Adressen zu. Verschieben von Services und hosting-Servern in verschiedenen physischen Standorten nehmen Applikationen IP-Adresskonfiguration mit an den neuen Speicherort. Der zweite Ansatz wird vollständig die IP-Adresse während des Übergangs in der wiederhergestellten Site. Jeder Ansatz hat mehrere Varianten der Implementierung die unten zusammengefasst werden.

Beim Entwerfen des Netzwerkes für Recovery-Standort, hat der Administrator zwei Optionen:

## <a name="option-1-retain-ip-addresses"></a>Option 1: IP-Adressen behalten 

Im Hinblick auf Disaster Recovery-Prozess mit festen IP Adressen scheint die einfachste Methode zum Implementieren, jedoch hat eine Reihe von möglichen Problemen, die an den am wenigsten verbreitete Ansatz stellen. Azure Site Recovery bietet die Möglichkeit, die IP-Adressen in allen Szenarios beibehalten. Bevor eine IP beibehalten, sollten entsprechende der Constraints nachgedacht werden, die Failover-Funktionen stellt. Lassen Sie uns die Faktoren, die zu einer IP-Adressen oder nicht beizubehalten weiterhelfen. Dies kann auf zwei Arten mit einem gestreckten Subnetz oder einen vollständigen Subnet Failover erfolgen.

### <a name="stretched-subnet"></a>Gestrecktes Subnetz

Hier wird das Subnetz gleichzeitig in Primär- und DR-Standorten verfügbar. Einfach ausgedrückt bedeutet dies einen Server und seine IP-(Ebene 3) Konfiguration an den zweiten Standort verschieben und das Netzwerk leitet den Datenverkehr an den neuen Speicherort automatisch. Dies ist aus Serversicht trivial, jedoch hat eine Reihe von Problemen:

- Im Hinblick auf Ebene 2 (Data Link Layer) benötigen Netzwerkkomponenten, die eine gestreckte VLAN verwalten können, aber geworden weniger jetzt verfügbar ist. Das zweite und schwierige Problem ist, dass VLAN potenzielle Fehlerdomäne auf beiden Sites erweitert wird im Grunde eine einzelne Fehlerquelle zu Strecken. Höchstwahrscheinlich ist, geschehen, dass broadcast-Storm gestartet, jedoch nicht isoliert werden. Wir gemischte Meinung über dieses letzte Problem gefunden und habe vielen erfolgreichen Implementierung sowie "wir nie diese Technologie implementiert".
- Gestrecktes Subnetz ist nicht möglich, wenn Sie Microsoft Azure als DR-Standort verwenden.


### <a name="subnet-failover"></a>Subnet-failover

Es ist möglich, Subnetz Failover Vorteile der beschriebenen ohne das Subnetz über mehrere Standorte hinweg gestreckt Subnetz-Lösung zu implementieren. Hier wäre angegebene Subnetz an Standort 1 oder 2, jedoch nie an beiden Standorten gleichzeitig. Aufrechterhaltung der IP-Adressbereich bei einem Failover kann programmgesteuert für die Router-Infrastruktur Subnetze von einem Standort verschieben anordnen. Ein Failover-Szenario Subnetze mit dem zugeordneten verschoben würde geschützt VMs. Der größte Nachteil dieses Ansatzes ist im Falle eines Fehlers, das müssen Sie das gesamte Subnetz die OK, Failover Granularität Aspekte beeinträchtigen. 

Im folgenden wird untersucht, wie ein fiktives Unternehmen namens Contoso die virtuellen an Recovery bei fehlerhaften über das gesamte Subnetz replizieren kann. Wir zunächst betrachten Contoso ist wie Replikation von VMs zwischen zwei lokalen Speicherorten und dann werden wir wie Subnetz Failover funktioniert, wenn Subnetze verwalten [Azure als Disaster Recovery-Standort verwendet wird](#failover-to-azure).

#### <a name="failover-to-a-secondary-on-premises-site"></a>Failover auf eine lokale Website

Schauen Sie wir uns einem Szenario soll wir behalten IP VMs und Failover abgeschlossen Subnetz zusammen. Der primäre Standort wurde im Subnetz 192.168.1.0/24 ausgeführt. Beim Failover wird zum Recovery-Standort Failover werden alle virtuellen Computer, die Teil dieses Subnetz und behalten die IP-Adressen. Routen müssen entsprechend geändert werden, um widerzuspiegeln, die alle virtuellen Computer in Subnetz 192.168.1.0/24 jetzt zum Recovery-Standort verschoben haben. 

In der folgenden Abbildung müssen die Routen zwischen primären Standort und Recovery-Standort, dritte primären Standort und dritten Standort und Recovery-Standort entsprechend geändert werden. 

Die folgenden Bilder zeigt die Subnetze vor dem Failover. Subnetz 192.168.0.1/24 auf dem primären Standort vor dem Failover ist aktiv und aktiv von Recovery-Standort nach dem failover 

![Vor dem Failover](./media/site-recovery-network-design/network-design2.png)

Vor dem failover


Die folgende Abbildung zeigt Netzwerke und Subnets nach einem Failover.
    
![Nach einem Failover](./media/site-recovery-network-design/network-design3.png)

Nach einem failover

Der sekundäre Standort wird lokal verwenden Sie VMM-Server verwalten und bei Schutz für einen bestimmten virtuellen Computer aktivieren, reserviert ASR Netzwerkressourcen nach den folgenden Arbeitsablauf:

- ASR weist eine IP-Adresse für jede Netzwerkschnittstelle auf dem virtuellen Computer aus den statischen IP-Adresspool im entsprechenden Netzwerk für jedes System Center VMM-Instanz definiert.
- Wenn der Administrator den gleichen IP-Adresspool für das Netzwerk am Recovery-Standort wie der IP-Adresspool des Netzwerks auf dem primären Standort definiert während VM Replikat ASR die IP-Adresse zuweisen die IP-Adresse des primären virtuellen Maschine zuweisen.  Die IP-Adresse in VMM reserviert, aber nicht als Failover-IP. Failover IP wird vor dem Failover festgelegt.

![IP-Adresse beibehalten](./media/site-recovery-network-design/network-design4.png)
    
Abbildung 5

Abbildung 5 zeigt Failover TCP/IP-Einstellungen für replizierte virtuelle Computer (Hyper-V-Verwaltungskonsole). Diese Einstellung werden nur vor dem Starten der virtuellen Computer nach einem Failover aufgefüllt werden

Wenn das gleiche IP nicht verfügbar ist, würde ASR einige verfügbare IP-Adresse aus der definierten IP-Adresspool zuweisen. 

Nach dem aktivieren die VM Schutz können folgenden Beispielskript Sie die IP-Adresse überprüfen, die dem virtuellen Computer zugewiesen wurde. Das gleiche IP würde werden als Failover-IP und die VM bei Failover:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

>[AZURE.NOTE] In dem Szenario, virtuelle Computer DHCP verwenden, ist die Verwaltung von IP-Adressen vollständig außerhalb des ASR. Administrator muss sicherstellen, dass der DHCP-Server mit der IP-Adressen auf Recovery-Standort wie der primäre Standort aus dem gleichen Bereich dienen kann.

#### <a name="failover-to-azure"></a>Failover auf Azure

Azure Site Recovery (ASR) ermöglicht Microsoft Azure als Disaster Recovery-Standort für die virtuellen Computer verwendet werden. In diesem Fall müssen Sie eine weitere Einschränkung behandeln. 

Betrachten wir ein Szenario ein fiktives Unternehmen mit dem Namen Woodgrove Bank lokale Infrastruktur ihre branchenanwendungen hat und sie Hosten ihrer Dienste in Azure. Konnektivität zwischen Woodgrove Bank VMs in Azure und lokalen Server erfolgt durch eine Standort-zu-Standort (S2S) Virtual Private Network (VPN). S2S VPN ermöglicht virtuelle Netzwerk der Woodgrove Bank in Azure als Erweiterung der Woodgrove Bank lokalen Netzwerk angezeigt werden. Diese Kommunikation wird von S2S VPN zwischen Woodgrove Bank und Azure virtuelles Netzwerk aktiviert. Jetzt möchte Woodgrove ASR die Arbeitslasten auf die Datencenter in Azure replizieren. Diese Option entspricht der Woodgrove möchte eine wirtschaftliche DR-Option und zum Speichern von Daten in öffentlichen Cloud-Umgebung. Woodgrove Applikationen und Konfigurationen die hartcodierte IP-Adressen abhängen hat daher sie müssen IP-Adressen für ihre Anwendung nach einem Failover zu Azure beibehalten.

Woodgrove möchte Zuweisen von IP-Adressen aus IP-Adressbereich (172.16.1.0/24, 172.16.2.0/24) seine Ressourcen in Azure ausgeführt.

Für die Woodgrove Bank zu virtuellen Computer in Azure replizieren, wobei die IP-Adressen soll ein virtuelles Netzwerk Azure erstellt werden. Es sollte eine Erweiterung des lokalen Netzwerks Applikationen nahtlos Failover von der lokalen Website in Azure können. Azure können Sie virtuelle Netzwerke erstellt in Azure-Standorten sowie Punkt-zu-Standort-VPN-Verbindung hinzu. Wenn die Standort-zu-Standort-Verbindung einrichten, können Sie mit Azure-Netzwerk Datenverkehr an lokalen (Azure wird LAN) nur, wenn der IP-Adressbereich von lokalen IP-Adressbereich ist Azure Dehnen Subnetze unterstützt.  Dies bedeutet, dass wenn Sie lokalen Subnetz 192.168.1.0/24, Sie können nicht in Azure Netzwerk LAN 192.168.1.0/24 hinzufügen. Dürfte Azure weiß, gibt es keine aktiven virtuellen Computer im Subnetz und das Subnetz nur für Disaster Recovery-Zwecke erstellt wird. Um richtig Netzwerkverkehr Azure Netzwerk Subnetze im Netzwerk und das lokale Netzwerk weiterleiten darf nicht im Konflikt. 

![Vor dem Subnetz Failover](./media/site-recovery-network-design/network-design7.png)

Vor dem failover

Woodgrove seinen Geschäftserfordernissen erfüllen müssen, wir die folgenden Workflows implementieren:

- Erstellen Sie ein zusätzliche Netzwerk, nennen Sie wir es Recovery Netzwerk, in dem die Failover-virtuellen Computer erstellt werden.
- Festlegen Sie Registerkarte konfigurieren unter VM-Eigenschaften, um sicherzustellen, dass die IP-Adresse für einen virtuellen Computer nach einem Failover, gehen das gleiche IP hat die VM lokal, und klicken Sie auf Speichern. Wenn die VM Failover, weist Azure Site Recovery bereitgestellte IP virtuellen Computer. 

![Netzwerk-Eigenschaften](./media/site-recovery-network-design/network-design8.png)

Das Failover ausgelöst und die virtuellen Computer im Netzwerk Recovery mit der gewünschten IP erstellt, kann Verbindung zu diesem Netzwerk mit [Vnet Vnet Verbindung](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)hergestellt werden. Falls diese Aktion Skript erforderlich.  Wie wir im vorherigen Abschnitt über Subnetz Failover bei Failover zu Azure müssten Routen reflektieren, 192.168.1.0/24 in Azure nun wurde entsprechend geändert werden. 

![Nach einem Failover Subnetz](./media/site-recovery-network-design/network-design9.png)

Nach einem failover

Haben Sie "Azure Netzwerk" wie in der obigen Abbildung dargestellt. Sie können eine Standort-zu-Standort-Vpn-Verbindung zwischen Ihre "Primärer Standort" und "Recovery Netzwerk nach dem Failover erstellen.  


## <a name="option-2-changing-ip-addresses"></a>Option 2: Ändern der IP-Adressen

Dadurch scheint die häufigste basierend auf gesehen haben. Es hat die Form Ändern der IP-adressedes jeder VM Failover beteiligt ist. Ein Nachteil dieses Ansatzes erfordert eingehende Netzwerk 'Informationen' IPx war Anwendung jetzt IPy steht. Auch wenn IPx und IPy logische Namen sind DNS-Einträge müssen in der Regel ändern oder im Netzwerk gespült zwischengespeicherte Einträge in Netzwerktabellen haben, aktualisiert oder gelöscht werden und daher eine Ausfallzeit sehen je nachdem wie die DNS-Infrastruktur eingerichtet ist. Diese Probleme können durch niedrige TTL-Werte bei Intranetanwendungen und [Azure Traffic Manager mit ASR](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) für Internet-basierte Programme verringert werden

### <a name="changing-the-ip-addresses---illustration"></a>Ändern die IP-Adressen - Abbildung

Lassen Sie uns das Szenario, Planen Sie unterschiedliche IP-Adressen der primären und Recovery-Standorten verwenden. Im folgenden Beispiel wir auch haben eine dritte Website aus, die Anträge auf primären oder Wiederherstellung gehostet Website möglich.

![Andere IP - vor dem Failover](./media/site-recovery-network-design/network-design10.png)

Abbildung 11

In Abbildung 11 sind einige Applikationen im Subnetz 192.168.1.0/24 Subnetz auf dem primären Standort und sie um auf Recovery-Standort im Subnetz 172.16.1.0/24 nach einem Failover konfiguriert wurden. VPN-Verbindungen/Netzwerkrouten haben entsprechend so konfiguriert, dass alle drei Standorte aufeinander zugreifen können.
 
Abbildung 12 zeigt nach einem Failover ein oder mehrere Programme werden sie im Subnetz Recovery wiederhergestellt. In diesem Fall werden wir nicht das gesamte Subnetz Failover gleichzeitig eingeschränkt. Keine VPN- oder Netzwerk Routen konfigurieren müssen. Failover und einigen DNS-Updates werden sichergestellt, dass Applikationen zugänglich bleiben. DNS für dynamische Updates konfiguriert ist würde die virtuellen Computer registrieren Sie sich mit der neuen IP-Wenn sie nach einem Failover beginnen. 

![Andere IP - nach einem Failover](./media/site-recovery-network-design/network-design11.png)

Abbildung 12

Nach einen Failover möglicherweise Replikat virtuellen Computer eine IP-Adresse, die IP-Adresse des primären virtuellen Computers identisch. Virtuelle Computer aktualisieren den DNS-Server, die sie mit nach dem start. DNS-Einträge müssen in der Regel geändert oder im gesamten Netzwerk gespült und zwischengespeicherte Einträge in Netzwerktabellen müssen aktualisiert oder gelöscht, daher es nicht ungewöhnlich, dass Ausfallzeiten ist während dieser Zustand Änderungen ausgesetzt. Dieses Problem kann durch verringert werden:

- Niedrige TTL-Werte verwenden für Intranetanwendungen.
- Mit Azure Traffic Manager ASR Internet Applications basiert.
- Mithilfe des folgenden Skripts in Ihren Wiederherstellungsplan aktualisiert den DNS-Server eine rechtzeitige Aktualisierung sicher (das Skript ist nicht erforderlich, wenn die dynamische DNS-Registrierung konfiguriert wird)

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


### <a name="changing-the-ip-addresses--dr-to-azure"></a>Ändern die IP-Adressen – DR in Azure

Blogbeitrag [Networking Infrastructure Setup für Microsoft Azure als Disaster Recovery](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) erläutert erforderliche Azure Netzwerkinfrastruktur setup-IP-Adressen behalten eine Anforderung nicht. Es beginnt mit der Anwendung, und suchen Sie am lokalen Netzwerke einrichten und Azure und dann wie ein testfailover und einem geplanten Failover.



## <a name="next-steps"></a>Nächste Schritte

[Informationen](site-recovery-network-mapping.md) wie Site Recovery Karten Quell- und Netzwerke beim VMM-Server wird verwendet, um die primäre Site verwalten. 
