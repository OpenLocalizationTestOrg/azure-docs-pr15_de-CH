<properties
    pageTitle="Überwachung und Problembehandlung für virtuelle Computer und physischen Servern | Microsoft Auzre" 
    description="Azure Site Recovery koordiniert Replikation, Failover und Recovery von virtuellen Maschinen auf lokalen Servern Azure zu einem sekundären Datencenter. Anhand dieses Artikels überwachen und Behandeln von VMM bzw. Hyper-V-Website." 
    services="site-recovery" 
    documentationCenter="" 
    authors="anbacker" 
    manager="mkjain" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/13/2016"    
    ms.author="rajanaki"/>
    
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Überwachung und Problembehandlung für virtuelle Computer und physischen Servern

Diese Überwachung und Problembehandlung können Sie den Replikationsstatus verfolgen und Problembehandlungsverfahren für Azure Site Recovery.

## <a name="understanding-the-components"></a>Grundlegendes zu den Komponenten

### <a name="vmwarephysical-site-deployment-for-replication-between-on-premises-and-azure"></a>VMware-physikalische Site Deployment für die Replikation zwischen lokalen und Azure.
DR zwischen lokalen VMware-physische Computer einrichten; Konfigurationsserver Master Ziel und Prozess-Server muss konfiguriert werden. Gleichzeitig Schutz für den Quellserver installiert Azure Site Recovery Mobility-Dienst. POST lokalen Ausfall Quellserver Failover in Azure Kundenbedürfnisse Prozessserver in Azure einrichten und einen Master-Zielserver lokalen Schutz den Quellserver auf neu erstellten lokalen. 

![VMware-physikalische Site Deployment für die Replikation zwischen lokalen und Azure](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises-site"></a>VMM Site Deployment für die Replikation zwischen lokalen Standort.

Als Bestandteil der DR zwischen zwei lokalen; Azure Site Recovery-Anbieter muss heruntergeladen und auf dem VMM-Server installiert. Provider benötigt Internet-Verbindung, um sicherzustellen, dass alle Vorgänge ausgelöst von Azure-Portal für lokale Vorgänge, wie Schutz aktivieren Herunterfahren primären virtuellen Computer als Teil des Failovers usw. übersetzt wird.

![VMM Site Deployment für die Replikation zwischen lokalen Standort](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises--azure"></a>VMM Site Deployment für die Replikation zwischen lokalen und Azure.

Als Teil des DR zwischen lokalen und Azure einrichten; Azure Site Recovery-Anbieter muss heruntergeladen und installiert auf dem VMM-Server und Azure Services Wiederherstellungsagenten auf jedem Hyper-V-Host installiert werden muss. Weitere Informationen finden Sie in der [Website zum Azure verstehen](./site-recovery-understanding-site-to-azure-protection.md) .

![VMM Site Deployment für die Replikation zwischen lokalen und Azure](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises--azure"></a>Hyper-V Site Deployment für die Replikation zwischen lokalen und Azure

Dies entspricht der VMM-Bereitstellung – nur Unterschied Anbieter & Agent auf Hyper-V-Host selbst installiert. Weitere Informationen finden Sie in der [Website zum Azure verstehen](./site-recovery-understanding-site-to-azure-protection.md) .

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Konfiguration, Schutz und Recovery-Vorgänge überwachen

Jede Operation in ASR wird überwacht und Registerkarte "Aufträge" überwacht. Konfiguration, Schutz oder Fehler bei der Wiederherstellung zu der Registerkarte Aufträge und festzustellen Sie, ob Fehler auftreten.

![Konfiguration, Schutz und Recovery-Vorgänge überwachen](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Sobald Fehler unter JOBS anzeigen gefunden wählen Sie, und klicken Sie auf FEHLERINFORMATIONEN für den Auftrag.

![Konfiguration, Schutz und Recovery-Vorgänge überwachen](media/site-recovery-monitoring-and-troubleshooting/image4.png)

Die Fehlerdetails hilft Ihnen möglicherweise und Empfehlung für das Problem zu identifizieren.

![Konfiguration, Schutz und Recovery-Vorgänge überwachen](media/site-recovery-monitoring-and-troubleshooting/image5.png)

Im obigen Beispiel scheint zu einem anderen Vorgang ausgeführt, Schutzkonfiguration fehlschlägt. Sicherstellen Sie, dass Sie beheben das Problem gemäß der Empfehlung danach klicken Sie auf Starten den Vorgang neu starten.

![Konfiguration, Schutz und Recovery-Vorgänge überwachen](media/site-recovery-monitoring-and-troubleshooting/image6.png)

Option zum Neustart ist nicht für alle Vorgänge, die nicht die Option zum Objekt navigieren und wiederholen Sie den Vorgang erneut. Jeder Auftrag kann gleichzeitig mit der Schaltfläche Abbrechen in Bearbeitung jederzeit abgebrochen werden.

![Konfiguration, Schutz und Recovery-Vorgänge überwachen](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machine"></a>Replikationszustand für den virtuellen Computer überwachen

ASR-Anbieter central & Remoteüberwachung über Azure-Portal für jeden geschützten Entitäten. Navigieren Sie zu GESCHÜTZTEN Elemente danach wählen Sie VMM-CLOUDS oder SCHUTZGRUPPEN. VMM-CLOUDS Registerkarte nur für VMM-basierte Installationen und andere Szenarien haben die geschützten Entitäten Registerkarte SCHUTZGRUPPEN.

![Replikationszustand für den virtuellen Computer überwachen](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Danach wählen Sie geschützte Entität unter der jeweiligen Wolke oder der Schutzgruppe. Nach Auswahl die geschützte Entität alle zulässig sind Vorgänge im unteren Bereich angezeigt.

![Replikationszustand für den virtuellen Computer überwachen](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Im Fall der virtuellen Maschine obigen ist Zustand kritisch – können Sie die FEHLERDETAILS auf nach unten, um die Fehlermeldung klicken. Auf "Ursachen" und "Recommendation" genannten beheben.

![Replikationszustand für den virtuellen Computer überwachen](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Replikationszustand für den virtuellen Computer überwachen](media/site-recovery-monitoring-and-troubleshooting/image11.png)

Hinweis: Navigieren Sie aktive Vorgänge in Bearbeitung oder fehlgeschlagen sind dann zu JOBS anzeigen erwähnte Stelle Fehlermeldungen angezeigt.

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Problembehandlung für lokale Hyper-V

Verbinden zum lokalen Hyper-V-Manager-Konsole, den virtuellen Computer und den Replikationsstatus.

![Problembehandlung für lokale Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image12.png)

In diesem Fall wird als kritisch- *Replikationsstatus Ansicht* sehen *Replikationsintegrität* angezeigt.

![Problembehandlung für lokale Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Für Fälle, wo Replikation für den virtuellen Computer angehalten, klicken Sie *Replikation*->*Resume Replication*
![Problembehandlung bei lokalen Hyper-V-Probleme](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Bei virtuellen Computer einen neuen Hyper-V-Host (innerhalb des Clusters oder einem eigenständigen Computer) die durch ASR konfiguriert migriert, würde nicht Replikation für den virtuellen Computer betroffen. Sicherstellen Sie, dass der neue Hyper-V-Host mit ASR konfiguriert erfüllt alle die pro-Komponenten.

### <a name="event-log"></a>Ereignisprotokoll

| Ereignisquellen                | Details                                                                                                                                                                                           |
|-------------------------  |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| **Programme und Protokolle/Microsoft/VirtualMachineManager/Server/Dienstadministratoren** (VMM-Server)   |  Dies bietet nützliche Protokollierung für Problembehandlung bei vielen anderen VMM. |
| **Programme und Protokolle/MicrosoftAzureRecoveryServices/Replikation** (Hyper-V-Host)   | Dies bietet nützliche Protokollierung für Problembehandlung viele Microsoft Azure Services Wiederherstellungsagenten. <br/> ![Quelle für Hyper-V-host](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Applikationen und Dienst Microsoft/Protokolle/Azure Site Recovery/Anbieter /** (Hyper-V-Host)   | Dies bietet nützliche Protokollierung für Problembehandlung viele Microsoft Azure Site Recovery Service. <br/> ![Quelle für Hyper-V-host](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Programme und Protokolle/Microsoft/Windows/Hyper-V-VMMS/Dienstadministratoren** (Hyper-V-Host) | Dies bietet nützliche Protokollierung für Problembehandlung viele Hyper-V virtuelle Computer Management. <br/> ![Quelle für Hyper-V-host](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |


### <a name="hyper-v-replication-logging-options"></a>Hyper-V-Replikation Protokollierungsoptionen

Hyper-V-VMMS sind alle Ereignisse Hyper-V Replica angemeldet\\Admin-Protokolldatei befindet sich unter **Applikationen und Dienstprotokolle\\Microsoft\\Windows**. Darüber hinaus kann eine analytische Protokoll für Hyper-V-VMMS aktiviert werden. Um dieses Protokoll zu aktivieren, stellen Sie zunächst die analytische und Debugprotokolle Protokolle in der Ereignisanzeige angezeigt. Öffnen Sie Ereignisanzeige, dann im **Menü Ansicht**, klicken Sie auf **Anzeigen von analytischen und Debugprotokollen**.

![Problembehandlung für lokale Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Analytische Protokoll wird unter Hyper-V-VMMS angezeigt

![Problembehandlung für lokale Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image15.png)

Klicken Sie im **Aktionsbereich** auf **Protokoll aktivieren**. Wenn aktiviert, sie wird im **Systemmonitor** ein Event Trace Session unterhalb **Sammlungssätze.**

![Problembehandlung für lokale Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Zum Anzeigen der Informationen zuerst beenden Sie Protokollierung das Protokoll deaktivieren und dann speichern Sie das Protokoll und erneut in der Ereignisanzeige öffnen Sie oder verwenden Sie andere Bedarf zu konvertieren.



## <a name="reaching-out-for-microsoft-support"></a>Microsoft Support erreichen

### <a name="log-collection"></a>Protokoll-Auflistung

VMM Site-Schutz finden Sie sammeln die erforderlichen Protokolle [ASR Protokollsammlung Werkzeug Support Diagnostics Plattform (SDP)](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) .

Für Hyper-V-Site-Schutz das [Tool](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) herunterladen und Ausführen auf dem Hyper-V-Host die Protokolle sammeln.

VMware-physische Szenarios finden Sie in [Azure Recovery Protokoll Websitesammlung für VMware und physischen Standort](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) zum Sammeln der erforderlichen Protokolle.

Tool sammelt Protokolle lokal unter einem zufällig benannten Unterordner unter **%LocalAppData%\ElevatedDiagnostics**

![Beispiele für Schritte von Hyper-V-Schutz.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="opening-a-support-ticket"></a>Support-Ticket öffnen

Zum Auslösen von Support-Ticket für ASR erreichen Sie Azure unterstützt <http://aka.ms/getazuresupport> mit URL

## <a name="kb-articles"></a>KB-Artikel

-   [Zur Erhaltung der Laufwerksbuchstabe für geschützte virtuelle Maschinen, die in Azure migriert oder Failover](http://support.microsoft.com/kb/3031135)
-   [Verwalten von lokal auf Azure Schutz Netzwerkbandbreite](https://support.microsoft.com/kb/3056159)
-   [ASR: "die Ressource konnte nicht gefunden werden" Fehler beim Schutz für eine virtuelle Maschine](http://support.microsoft.com/kb/3010979)
-   [Erkennen Sie und Beheben von Hyper-V Replica-Handbuch](http://www.microsoft.com/en-in/download/details.aspx?id=29016) 

## <a name="common-asr-errors-and-their-resolutions"></a>ASR-Fehlermeldungen und deren Lösung

Unten sind häufige Fehler, die möglicherweise erreicht und deren Auflösung. Jeder Fehler wird in einer separaten Wikiseite dokumentiert.

### <a name="general"></a>Allgemein
-   <span style="color:green;">Neu</span> [Aufträge schlägt fehl mit Fehlermeldung "ein Vorgang ausgeführt wird." Fehler 505 514, 532](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
-   <span style="color:green;">Neu</span> [Aufträge schlägt fehl mit Fehlermeldung "Server ist nicht mit dem Internet verbunden". Fehler beim 25018](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Setup
-   [Der VMM-Server kann aufgrund eines internen Fehlers nicht registriert werden. Jobs anzeigen im Site Recovery-Portal für weitere Details über den Fehler finden. Führen Sie Setup erneut aus, um den Server zu registrieren.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
-   [Hyper-V Recovery Manager Depot kann eine Verbindung hergestellt werden. Überprüfen Sie die Proxyeinstellungen, oder versuchen Sie es erneut.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Konfiguration
-   [Keine Schutzgruppe erstellen: Fehler beim Abrufen der Liste der Server.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
-   [Hyper-V-Host-Cluster enthält mindestens eine statische Netzwerkadapter oder keine verbundenen Adapter konfiguriert DHCP verwenden.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
-   [VMM ist nicht berechtigt, eine Aktion ausführen](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
-   [Das Speicherkonto innerhalb des Abonnements beim Konfigurieren des Schutzes können nicht ausgewählt werden.](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Schutz
- <span style="color:green;">Neu</span> [Schutz aktivieren mit "konnte nicht Fehler Schutz für den virtuellen Computer konfiguriert werden" fehl. Fehler 60007, 40003](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
- <span style="color:green;">Neu</span> [Schutz aktivieren fehlerhafte mit "konnte nicht Fehler für den virtuellen Computer geschützt werden." Fehler beim 70094](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
- <span style="color:green;">Neu</span> [Live-Migrationsfehler 23848 - der virtuelle Computer wird mit Live verschoben werden. Dies konnte den Schutzstatus Wiederherstellung des virtuellen Computers unterbrechen.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx) 
- [Schutz ist fehlgeschlagen, da der Agent nicht auf dem Hostcomputer](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
- [Ein geeigneter Host für den virtuellen replikatcomputer kann nicht gefunden werden - aufgrund compute-Ressourcen](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
- [Ein geeigneter Host für den virtuellen replikatcomputer - keine logischen Netzwerk angeschlossen gefunden](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
- [Keine Verbindung zum Hostcomputer Replikat - Verbindung konnte nicht hergestellt werden](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)


### <a name="recovery"></a>Wiederherstellung
- VMM kann nicht den Host-Vorgang abgeschlossen werden-
    -   [Ein Failover auf den ausgewählten Wiederherstellungspunkt für Virtual Machine: allgemeiner Zugriff verweigert Fehler.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
    -   [Hyper-V auf dem ausgewählten Wiederherstellungspunkt für Virtual Machine Fehler: Vorgang abgebrochen neueren Wiederherstellungspunkt versuchen. (0 x 80004004)](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
    -   Eine Verbindung mit dem Server konnte nicht hergestellt werden (0x00002EFD)
        -   [Fehler beim Aktivieren von reverse-Replikation für den virtuellen Computer Hyper-V](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
        -   [Fehler beim Aktivieren der Replikation für VM Virtual Machine Hyper-V](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
    -   [Failover für virtuelle Computer konnte kein Commit ausgeführt.](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
-   [Der Wiederherstellungsplan enthält virtuelle Computer sind nicht zur geplanten failover](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
-   [Der virtuelle Computer nicht geplanten Failover bereit](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   [Virtuelle Maschine wird nicht ausgeführt und nicht ausgeschaltet](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
-   [Der Betrieb passiert auf einem virtuellen Computer und Commit Failover Fehler](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   Testfailover
    -   [Failover konnte nicht gestartet werden, da testfailover ausgeführt wird](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
-   <span style="color:green;">Neu</span>  Failover tritt ein Timeout mit"PreFailoverWorkflow WaitForScriptExecutionTaskTimeout" aufgrund der Konfiguration der Netzwerk-Sicherheitsgruppe zugeordneten virtuellen Computer oder das Subnetz, zu dem der Computer gehört. Einzelheiten finden Sie unter ["Aufgabe WaitForScriptExecutionTaskTimeout PreFailoverWorkflow"](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) .


### <a name="configuration-server-process-server-master-target"></a>Konfigurationsziel Server Process-Server-Master
Konfigurationsserver (CS) Prozessserver (PS), Master-Targer (MT)
-   [ESXi Host als ein virtueller Computer PS/CS gehostet schlägt mit einem violetten Bildschirm.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Remotedesktop Problembehandlung nach einem failover
-   Viele Kunden haben Probleme mit fehlgeschlagenen über VM in Azure verbinden konfrontiert. [Verwenden Sie zur Problembehandlung Dokument RDP in der VM](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Eine öffentliche IP-Adresse hinzufügen auf einem Computer Ressource-manager
Wenn die Schaltfläche **Verbinden** im Portal abgeblendet und Sie nicht mit Azure über einen Express Route oder eine Standort-zu-Standort-VPN-Verbindung verbunden sind, müssen Sie zu VM eine öffentliche IP-Adresse zuweisen, bevor Sie RDP-SSH verwenden können. Gehen Sie folgendermaßen vor, um eine öffentliche IP-Adresse für die Netzwerkschnittstelle des virtuellen Computers hinzugefügt.  

![Hinzufügen eine öffentliche IP-Adresse für die Netzwerkschnittstelle des virtuellen Computers Failover](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)