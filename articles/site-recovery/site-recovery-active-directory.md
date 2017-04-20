<properties
    pageTitle="Schützen von Active Directory und DNS mit Azure Site Recovery | Microsoft Azure"
    description="Dieser Artikel beschreibt, wie eine Disaster Recovery-Lösung für Active Directory mit Azure Site Recovery implementiert."
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="08/31/2016"
    ms.author="pratshar"/>

# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Schützen von Active Directory und DNS mit Azure Site Recovery

Anwendung wie SharePoint, Dynamics AX und SAP hängt davon ab, Active Directory und DNS-Infrastruktur ordnungsgemäß funktioniert. Zur Erstellung eine Disaster Recovery-Lösung für Applikationen unbedingt müssen zum Schützen und Wiederherstellen von Active Directory und DNS vor anderen Anwendungskomponenten, daß Dinge funktionieren Notfall.

Site Recovery ist eine Azure Service, der Wiederherstellung durch Replikation, Failover und Recovery von virtuellen Maschinen orchestriert bereitstellt. Site Recovery unterstützt eine Reihe von Replikationsszenarien zu konsistent, nahtlos Failover virtuellen Maschinen und Applikationen Private, Public oder Hoster Wolken.

Site Recovery können Sie einen vollständig automatisierte Disaster Recovery-Plan für Active Directory erstellen. Wenn Störungen, können Sie überall Sekunden ein Failover initiiert und Active Directory läuft in wenigen Minuten. Wenn Active Directory für mehrere SharePoint und SAP, in dem primären Standort bereitgestellt haben und vollständige Website Failover werden soll, können Sie nicht über Active Directory zunächst mit Site Recovery und Failover für die genutzte anwendungsspezifische Recovery-Pläne.

Erläutert, wie eine Disaster Recovery-Lösung für Active Directory, wie geplante und ungeplante ausgeführt und Test-Failover mit Mausklick Wiederherstellungsplan, unterstützte Konfigurationen und erforderliche Komponenten erstellen.  Sie sollten Active Directory mit Azure Site Recovery vertraut sein, bevor Sie beginnen.

Es gibt zwei empfohlene Optionen abhängig von der Komplexität Ihrer Umgebung.

### <a name="option-1"></a>Option 1

Wenn eine kleine Anzahl von Programmen und einem einzelnen Domänencontroller haben und Failover für die gesamte Site möchten, empfehlen wir mit Site Recovery repliziert den Domänencontroller an den sekundären Standort (ob Sie Azure oder an einem sekundären Standort Failover sind). Demselben replizierten virtuellen Computer für die testfailover-zu dienen.

### <a name="option-2"></a>Option 2

Wenn eine große Anzahl von Applikationen und mehrere Domänencontroller in der Umgebung vorhanden ist, oder möchten Sie einige Applikationen gleichzeitig Failover, sollten neben dem virtuellen Domänencontrollercomputer mit Standort replizieren richten Sie auch einen zusätzlichen Domänencontroller in der Zielsite (Azure oder einem lokalen sekundären Datencenter).

>[AZURE.NOTE] Auch wenn Sie dafür ein testfailover mit Site Recovery Domänencontroller repliziert müssen Option 2 implementieren. Lesen Sie weitere [Aspekte der Failover testen](#considerations-for-test-failover) .


Die folgenden Abschnitte erläutern Schutz für einen Domänencontroller in der Site Recovery und zum Domänencontroller in Azure einrichten.


## <a name="prerequisites"></a>Erforderliche Komponenten

- Eine lokale Bereitstellung von Active Directory und DNS-Server.
- Ein Azure Site Recovery Services Depot in ein Microsoft Azure-Abonnement.
- Sie Azure ausführen Azure Virtual Machine Readiness Assessment Tool auf VMs sicherstellen Replikation ist kompatibel mit Azure VMs und Azure Site Recovery Services.


## <a name="enable-protection-using-site-recovery"></a>Aktivieren Sie Site Recovery-Schutz


### <a name="protect-the-virtual-machine"></a>Der virtuelle Computer schützen

Aktivieren des Schutzes des Domain Controller-DNS virtuellen Computers Site Recovery. Einstellungen Sie Site Recovery-basierend auf dem virtuellen Computer (Hyper-V oder VMware). Wir empfehlen eine konsistente Replikationsrate Absturz von 15 Minuten.

###<a name="configure-virtual-machine-network-settings"></a>VM-Netzwerk konfigurieren

Konfigurieren Sie für Domänencontroller-DNS virtuellen Computer Einstellungen Site Recovery VM nach einem Failover an das richtige Netzwerk angeschlossen werden soll. Beispielsweise wenn Sie Hyper-V VMs in Azure Replikation können Sie die VM in der VMM-Cloud oder in der Schutzgruppe die Einstellungen wie folgt konfigurieren auswählen

![VM Network Settings](./media/site-recovery-active-directory/VM-Network-Settings.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Schützen von Active Directory mit Active Directory-Replikation

### <a name="site-to-site-protection"></a>Standort-zu-Standort-Schutz

Erstellen Sie Domänencontroller am sekundären Standort, und geben Sie den Namen der gleichen Domäne, die beim Heraufstufen des Servers zu einer Domänencontrollerfunktion am primären Standort verwendet wird. Das Snap-in **Active Directory-Standorte und-Dienste** können Sie um auf das Objekt zu konfigurieren, die Websites hinzugefügt werden. Durch die Standardeinstellungen für eine standortverknüpfung konfigurieren, können Sie steuern, wenn Replikation zwischen mindestens zwei Standorten und wie oft. Weitere Informationen finden Sie unter [Planen der Replikation zwischen Standorten](https://technet.microsoft.com/library/cc731862.aspx) .

###<a name="site-to-azure-protection"></a>Site Azure Schutz

Führen Sie auf [einen Domänencontroller in Azure virtuelles Netzwerk erstellen](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Beim Heraufstufen des Servers zu einer Domänencontrollerfunktion Geben Sie den Domänennamen des primären Standorts verwendet wird.

Dann [Konfigurieren Sie den DNS-Server für das virtuelle Netzwerk](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), mit dem DNS-Server in Azure.

![Azure-Netzwerk](./media/site-recovery-active-directory/azure-network.png)

## <a name="test-failover-considerations"></a>Testen der Failover-Hinweise

Testfailover tritt in einem Netzwerk, das vom Produktionsnetzwerk isoliert ist, so dass keine Auswirkung auf Produktions-Workloads.

Die meisten verlangen auch ein Domänencontroller und ein DNS-Server funktioniert, damit vor dem Failover der Anwendung ein Domänencontroller im isolierten Netzwerk für testfailover zu verwendende erstellt werden soll. Die einfachste Möglichkeit hierzu ist auf Domäne-Controller-DNS virtuelle Maschine mit Site Recovery und führen Sie ein testfailover des virtuellen Computers, bevor ein testfailover der Wiederherstellungsplan für die Anwendung ausführen. Hier ist die Vorgehensweise:

1. Aktivieren Sie Schutz in Site Recovery für Domain Controller-DNS virtuellen Computer.
2. Ein isoliertes Netzwerk zu erstellen. Ein virtuelles Netzwerk in Azure standardmäßig erstellt wird isoliert von anderen Netzwerken. Wir empfehlen der IP-Adressbereich für dieses Netzwerk wie das Produktionsnetzwerk. Aktivieren Sie Standort-zu-Standort-Verbindung in diesem Netzwerk nicht.
3. Geben Sie eine DNS IP Adresse im Netzwerk erstellt, die IP-Adresse, die der DNS-VM zu erwarten. Wenn Sie in Azure replizieren, geben Sie die IP-Adresse für den virtuellen Computer auf Failover in **Ziel-IP-** Einstellung VM-Eigenschaften verwendet wird. Wenn Replikation an einen anderen lokalen Site und Sie DHCP folgen auf [DNS und DHCP für testfailover setup](site-recovery-failover.md#prepare-dhcp)

>[AZURE.NOTE] Die IP-Adresse bei einem testfailover zu einer virtuellen Maschine zugeordnet ist wie die IP-Adresse erhalten sie bei einem geplanten oder ungeplanten Failover wird auf die IP-Adresse im Testnetzwerk Failover verfügbar. Andernfalls erhält der virtuelle Computer eine andere IP-Adresse, die im Test-Failover-Netzwerk verfügbar ist.

4. Führen Sie auf dem virtuellen Domänencontrollercomputer ein testfailover der im isolierten Netzwerk. Verwenden Sie aktuellen Anwendung konsistent Wiederherstellungspunkt virtuellen Domänencontrollercomputer testfailover. 
5. Führen Sie einen testfailover für Anwendung Wiederherstellungsplan.
6. Nach Abschluss der Tests markieren Sie Failover Testauftrags virtuellen Domänencontrollercomputer und den Wiederherstellungsplan 'Vollständig' auf der Registerkarte **Aufträge** im Portal Site Recovery.

### <a name="dns-and-domain-controller-on-different-machines"></a>DNS und Domänencontroller auf unterschiedlichen Computern

Wenn DNS auf dem gleichen virtuellen Computer als Domänencontroller nicht DNS VM für testfailover erstellen müssen. Wenn sie auf dem gleichen virtuellen Computer sind, können Sie diesen Abschnitt überspringen.

Sie können neuen DNS-Server verwenden und alle erforderlichen Zonen. Wenn Active Directory-Domäne "contoso.com" ist, können Sie eine DNS-Zone mit dem Namen "contoso.com" erstellen. Die Einträge zu Active Directory müssen in DNS wie folgt aktualisiert:

1. Sicherzustellen Sie, dass diese Einstellungen vor anderen virtuellen Computern in der Wiederherstellungsplan sind:

    - Die Zone muss nach der Gesamtstruktur-Stammname benannt.
    - Die Zone muss Datei gesichert.
    - Die Zone muss auf sichere und nicht sichere Updates aktiviert sein.
    - Die Auflösung des virtuellen Domänencontrollercomputer sollte der IP-Adresse des DNS-virtuellen Computers verweisen.

2. Führen Sie den folgenden Befehl auf einem virtuellen Domänencontrollercomputer:

    `nltest /dsregdns`

3. Zone auf dem DNS-Server hinzufügen und dafür ein Eintrag DNS nicht sichere Updates zulassen:

        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1


## <a name="next-steps"></a>Nächste Schritte

Lesen [welche Arbeitslasten können schützen?](../site-recovery/site-recovery-workload.md) Weitere Informationen zu Enterprise-Arbeitslasten mit Azure Site Recovery geschützt.
