<properties 
    pageTitle="Schützen von SQL Server mit SQL Server-Wiederherstellung und Azure Site Recovery | Microsoft Azure" 
    description="Dieser Artikel beschreibt die Disaster-Funktionen von Azure Site Recovery von SQL Server mit SQL Server replizieren." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.workload="backup-recovery" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2016" 
    ms.author="raynew"/>


# <a name="protect-sql-server-with-sql-server-disaster-recovery-and-azure-site-recovery"></a>Schützen von SQL Server mit SQL Server-Wiederherstellung und Azure Site Recovery 


Azure Site Recovery-Dienst trägt Ihre Business Continuity und Disaster Recovery (BCDR)-Strategie durch Replikation, Failover und Recovery von virtuellen Computern und Servern orchestriert. Computer können Azure oder einem lokalen sekundären Rechenzentrum repliziert werden. Finden Sie einen schnellen Überblick über [Neuigkeiten Azure Site Recovery?](site-recovery-overview.md).

 Dieser Artikel beschreibt, wie SQL Server-Back-End einer Anwendung mit einer Kombination aus SQL Server BCDR Technologies und Azure Site Recovery geschützt. Gute Kenntnisse der SQL Server-Disaster Recovery-Funktionen müssen (Failover clustering AlwaysOn Availability Gruppen Datenbank Spiegelung Protokollversand) und Azure Site Recovery, bevor Sie die in diesem Artikel beschriebenen Szenarien bereitstellen.



## <a name="overview"></a>Übersicht

Viele verschiedene Arbeitslasten verwenden SQL Server als Grundlage. Programme wie SharePoint, Dynamics und SAP verwenden SQL Server Data Services implementiert.  Applikationen bereitstellen SQL Server in einer Reihe von Arten:

- **Eigenständige SQL Server**: SQL Server und alle Datenbanken auf einem einzigen Computer (physisch oder virtuell) gehostet werden. Wenn virtualisiert, Hostclustering lokale hohe Verfügbarkeit dient. Keine hoher Verfügbarkeit auf Gast-Ebene implementiert.
- **SQL Server Failover Clustering-Instanzen (immer auf FCI)**: zwei oder mehr Knoten SQL Server-Instanzen mit freigegebenen Datenträger in einem Windows-Failovercluster konfiguriert. Wenn Cluster Instanzen ist, kann SQL Server zu einer anderen Instanz Cluster Failover. Dieses Setup wird normalerweise für HA auf einem primären Standort verwendet. Es schützt nicht gegen Fehler oder Ausfall der freigegebenen Speicherschicht. Freigegebene Datenträger kann mit ISCSI, Fibre Channel oder freigegebene VHDx implementiert werden.
- **SQL immer auf Verfügbarkeitsgruppen**: In diesem zwei Knoten werden in einer gemeinsam genutzten nichts cluster mit SQL Server-Datenbanken in einer Availability Group mit synchroner Replikation und automatisches Failover konfiguriert.

In Enterprise Edition bietet SQL Server auch systemeigene Disaster Recovery-Funktionen für die Wiederherstellung von Datenbanken in einem remote-Standort. In diesem Artikel wir nutzen und integriert diese systemeigenen SQL Disaster Recovery-Funktionen: 

- SQL immer auf Verfügbarkeitsgruppen Disaster Recovery für SQL Server 2012 oder 2014 Enterprise Edition.
- SQL datenbankspiegelung im hohen Sicherheitsmodus für SQL Server Standard Edition (beliebige Version) oder SQL Server 2008 R2.


Site Recovery kann SQL Server schützen, wie in der Tabelle zusammengefasst.

 |**Lokal zu lokalen** | **Lokal in Azure** 
---|---|---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja 
**Physische server** | Ja | Ja


## <a name="support-and-integration"></a>Unterstützung und integration

Diese Versionen von SQL Server unterstützt die Szenarien in diesem Artikel:


- SQL Server 2014 Enterprise und Standard
- SQL Server 2012 Enterprise und Standard
- SQL Server 2008 R2 Enterprise und Standard


Site Recovery integrierbar mit systemeigenen SQL Server BCDR in der folgenden Tabelle zusammengefasst, um eine Disaster Recovery-Lösung zu bieten.

**Funktion** |**Details** | **SQL Server-version** 
---|---|---
**AlwaysOn Availability group** | Führen Sie mehrere eigenständige Instanzen von SQL Server in einem Failovercluster, der mehrere Knoten hat.<br/><br/>Datenbanken können in Gruppen Failover gruppiert werden, die (gespiegelt) für SQL Server-Instanzen kopieren, damit kein gemeinsamer Speicher erforderlich ist.<br/><br/>Bietet Wiederherstellung zwischen einem primären und sekundären Standorte. Zwei Knoten können in einer gemeinsam genutzten nichts eingerichtet werden mit SQL Server-Datenbanken in einer Availability Group mit synchroner Replikation und automatisches Failover konfiguriert. | SQL Server 2014 & 2012 Enterprise edition
**Failover-Clusterunterstützung (AlwaysOn FCI)** | SQL Server nutzt Windows-Failoverclustering für hohe Verfügbarkeit der lokalen SQL Server-Arbeitslasten.<br/><br/>Knoten Instanzen von SQL Server mit freigegebenen Datenträger werden in einem Failovercluster konfiguriert. Wenn eine Instanz unten Failover Cluster zu anderen.<br/><br/>Cluster schützen nicht Fehler oder Ausfälle bei gemeinsam genutztem Speicher. Der freigegebene Datenträger kann mit iSCSI, Fibre Channel oder VHDXs freigegeben. | SQL Server Enterprise Edition<br/><br/>SQL Server Standard Edition (nur zwei Knoten beschränkt)
**Datenbankspiegelungsmodus (hohe Sicherheit)** | Schützt eine einzelne Datenbank eine sekundäre Kopie. Für hohe Sicherheit (synchron) und hohe Leistung (asynchron) Replikation. Einen Failovercluster ist nicht erforderlich. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise alle Editionen
**Eigenständige SQL Server** | Die SQL Server und die Datenbank befinden sich auf einem Server (physisch oder virtuell). Hostclustering wird für hohe Verfügbarkeit verwendet, wenn der Server virtuell ist. Keine hohe Verfügbarkeit auf Gast-Ebene. | Enterprise oder Standard edition

## <a name="deployment-recommendations"></a>Empfohlene Bereitstellung


In dieser Tabelle sind unsere Empfehlung für die Integration von SQL Server BCDR Technologies mit Site Recovery zusammengefasst.

**Version** |**Edition** | **Bereitstellung** | **Prem, auf prem** | **Prem in Azure** 
---|---|---|---|---
SQL Server 2014 oder 2012 | Unternehmen | Failover Cluster-Instanz | AlwaysOn Availability Gruppen | AlwaysOn Availability Gruppen
 | Unternehmen | AlwaysOn Availability Gruppen für hohe Verfügbarkeit | AlwaysOn Availability Gruppen | AlwaysOn Availability Gruppen
 | Standard | Failoverclusterinstanz (FCI) | Site Recovery Replikation mit lokalen | Site Recovery Replikation mit lokalen
 | Enterprise oder Standard | Eigenständige | Site Recovery Replikation | Site Recovery Replikation
SQL Server 2008 R2 | Enterprise oder Standard | Failoverclusterinstanz (FCI) | Site Recovery Replikation mit lokalen | Site Recovery Replikation mit lokalen
 | Enterprise oder Standard | Eigenständige | Site Recovery Replikation | Site Recovery Replikation
SQL Server (beliebige Version) | Enterprise oder Standard | Failoverclusterinstanz - DTC-Anwendung | Site Recovery Replikation | Nicht unterstützt


## <a name="deployment-prerequisites"></a>Bereitstellung erforderlicher Komponenten

Ist was Sie vor dem start:

- Eine lokale SQL Server-Bereitstellung eine unterstützte Version von SQL Server ausgeführt. In der Regel benötigen Sie auch Active Directory für den SQL Server.
- Die erforderlichen Komponenten für das Szenario bereitgestellt werden soll. Komponenten können in jeder Bereitstellung Artikel gefunden. Links zu diesen werden in der [Übersicht Recovery](site-recovery-overview.md)bereitgestellt.
- Wenn Sie Recovery in Azure einrichten möchten, müssen Sie auf den virtuellen Computern SQL Server, um sicherzustellen, dass sie kompatibel mit Azure und Site Recovery sind [Azure Virtual Machine Readiness Assessment](http://www.microsoft.com/download/details.aspx?id=40898) ausführen.


## <a name="set-up-active-directory"></a>Einrichten von Active Directory

Benötigen Sie Active Directory auf sekundären Recovery-Standort für SQL Server richtig ausgeführt. Es gibt einige Optionen:

- **Kleine Unternehmen**– Wenn eine kleine Anzahl von Programmen und einem einzelnen Domänencontroller am lokalen Standort haben und Failover für die gesamte Site möchten, wird empfohlen, Site Recovery Repication verwenden, um den Domänencontroller im sekundären Datencenter oder Azure replizieren.

- **Mittelständische und große Unternehmen**, wenn eine große der Anwendung Anzahl Active Directory-Gesamtstruktur ausgeführt sind und Anwendung oder Arbeitslast fehlschlagen soll, richten Sie einen zusätzlichen Domänencontroller im sekundären Datencenter oder in Azure wird empfohlen. Hinweis Verwenden Sie AlwaysOn Availability Gruppen auf einem remote-Standort wird empfohlen, dass Sie einen anderen zusätzliche Domänencontroller auf den sekundären Standort oder Azure für die wiederhergestellte SQL Server-Instanz einrichten.

Die Informationen in diesem Dokument davon aus, dass ein Domänencontroller am sekundären Standort verfügbar ist. [Weitere](site-recovery-active-directory.md) Informationen zum Schützen von Active Directory mit Site Recovery

## <a name="integrate-protection-with-sql-server-always-on-on-premises-to-azure"></a>SQL Server ununterbrochenes (lokal in Azure) integrieren Sie Schutz


Site Recovery unterstützt SQL AlwaysOn. Wenn Sie mit einer Azure virtuellen Maschine können Sie Site Recovery Failover der Verfügbarkeit zu verwenden als "Sekundär" Einrichten einer SQL Availability Group erstellt haben. 

>[AZURE.NOTE] Diese Funktion ist derzeit in Vorschau und Hyper-V-Hostservern im primären Rechenzentrum in VMM-Clouds verwaltet werden und VMware Setup [Konfigurationsserver](site-recovery-vmware-to-azure.md#configuration-server-prerequisites)verwaltet. Jetzt diese Funktion nicht im neuen Azure-Portal zur Verfügung.

#### <a name="prerequisites"></a>Erforderliche Komponenten

Hier ist Site Recovery SQL AlwaysOn integriert werden sollen:

- Eine lokale SQL Server (eigenständige Server oder einem Failovercluster).
- Ein oder mehrere Azure virtuelle Computer mit SQL Server installiert
- Eine SQL Verfügbarkeit Gruppe zwischen einer lokalen SQL Server und SQL Server in Azure ausgeführt
- PowerShell-Remoting sollte auf dem lokalen SQL Server-Computer aktiviert werden. VMM-Server oder des sollte remote PowerShell SQL Server aufrufen können.
- Ein Benutzerkonto muss auf SQL Server lokal in dieser SQL-Benutzergruppen diese Berechtigungen hinzugefügt:
    - ALTER Verfügbarkeit: Gruppenberechtigungen [hier](https://msdn.microsoft.com/library/hh231018.aspx)und [hier](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - ALTER DATABASE - Berechtigungen[hier](https://msdn.microsoft.com/library/ff877956.aspx#Security)
- RunAs-Konto auf dem VMM-Server erstellt werden soll oder ein Konto auf dem Konfigurationsserver mit der CSPSConfigtool.exe für den im vorherigen Schritt erwähnt Benutzer erstellt werden 
- SQL-PS-Modul sollte SQL Server lokal und Azure virtuelle Computer installiert:
- VM-Agent sollte installierten virtuellen Computern Azure
- NTAUTHORITY\System müssen folgende Berechtigungen für SQL Server auf virtuellen Computern in Azure ausgeführt:
    - ALTER AVAILABILITY GROUP - Berechtigungen [hier](https://msdn.microsoft.com/library/hh231018.aspx)und [hier](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - ALTER DATABASE - Berechtigungen [hier](https://msdn.microsoft.com/library/ff877956.aspx#Security)

####  <a name="step-1-add-a-sql-server"></a>Schritt 1: Hinzufügen einer SQL Server


1. Klicken Sie auf **SQL hinzufügen** eine neue SQL Server hinzufügen. 

    ![SQL hinzufügen](./media/site-recovery-sql/add-sql.png)

2. Im **Konfigurieren SQL** > **Namen** angeben einen Anzeigenamen auf SQL Server.
3. **In SQL Server (FQDN)** Geben Sie den FQDN der Quelle SQL Server, die Sie hinzufügen möchten. Falls SQL Server auf einem Failovercluster installiert ist, bieten Sie FQDN des Clusters und nicht der Clusterknoten.  
4. Wählen Sie in **SQL Server-Instanz** die Standardinstanz, oder geben Sie den Namen der benutzerdefinierten Instanz.
5. Wählen unter **Verwaltungsserver** VMM-Server oder Server Configuration im Tresor Site Recovery registriert. Site Recovery verwendet diese Management-Server mit SQL Server kommunizieren.
6. Geben Sie den Namen eines Kontos RunAs, die auf dem angegebenen VMM-Server erstellt wurde oder auf dem Configuraaaon-Server erstellte Konto ausführen **als Konto** . Dieses Konto wird verwendet, um den SQL Server zugreifen und muss Lese-und Failover auf Verfügbarkeit auf dem SQL Server-Computer.

    ![SQL-Dialogfeld hinzufügen](./media/site-recovery-sql/add-sql-dialog.png)

Nach dem Hinzufügen von SQL Server wird sie in der Registerkarte **SQL Server** angezeigt. 

![SQL Server-Liste](./media/site-recovery-sql/sql-server-list.png)


#### <a name="step-2-add-a-sql-availability-group"></a>Schritt 2: Hinzufügen einer SQL Availability Group

1. Nach SQL Server wird Computer hinzugefügt, ist der nächste Schritt Site Recovery Verfügbarkeitsgruppen hinzufügen. Hierzu Drilldown in der SQL Server zuvor hinzugefügt und auf SQL Verfügbarkeit Gruppe hinzufügen. 

    ![Hinzufügen von SQL AG](./media/site-recovery-sql/add-sqlag.png)

2. SQL Availability Group kann eine oder mehrere virtuelle Computer in Azure replizieren. Beim Hinzufügen der Sql verfügbarkeitsgruppe müssen Sie den Namen und Abonnement der Azure Virtual Machine, in dem Sie die Verfügbarkeit Failover von Site Recovery durchgeführt werden soll.

    ![SQL AG Dialogfeld hinzufügen](./media/site-recovery-sql/add-sqlag-dialog.png)

3. Im obigen Beispiel Availability Group DB1 AG werden primäre auf virtuellen Computer SQLAGVM2 im Abonnement DevTesting2 auf ein Failover ausgeführt. 

>[AZURE.NOTE] Nur Verfügbarkeitsgruppen primär auf SQL Server in Schritt hinzugefügt werden Site Recovery hinzugefügt werden. Wenn Sie SQL Server eine Verfügbarkeit Gruppe primäre gemacht haben oder wenn Sie Verfügbarkeitsgruppen auf SQL Server hinzugefügt haben, nachdem er hinzugefügt wurde, mit der verfügbaren Option aktualisieren auf SQL Server aktualisieren.

#### <a name="step-3-create-a-recovery-plan"></a>Schritt 3: Erstellen eines Wiederherstellungsplans

Im nächste Schritt werden erstellen Sie einen Wiederherstellungsplan mit virtuellen Computern und Verfügbarkeitsgruppen. Wählen Sie den VMM-Server oder Verwendung Konfigurationsserver in Schritt 1 als Quelle und als Ziel Microsoft Azure.

![Wiederherstellungsplan erstellen](./media/site-recovery-sql/create-rp1.png)

![Wiederherstellungsplan erstellen](./media/site-recovery-sql/create-rp2.png)

Im Beispiel besteht aus 3 virtuellen Maschinen die eine SQL-Availability Group wie die Back-End-Sharepoint-Anwendung. Im Wiederherstellungsplan konnte wählen wir sowohl die verfügbarkeitsgruppe darstellen der virtuellen Computer die Anwendung. 

Den Wiederherstellungsplan können weiter durch Verschieben von virtuellen Maschinen andere Failover Gruppen die Reihenfolge der Failover-Sequenz. Verfügbarkeitsgruppe wird immer zuerst fehlgeschlagen, da diese als Backend jeder Anwendung verwendet werden soll. 

![Wiederherstellungsplan anpassen](./media/site-recovery-sql/customize-rp.png)

### <a name="step-4--fail-over"></a>Schritt 4: Failover

Verschiedene Failoveroptionen sind verfügbar einen Wiederherstellungsplan Availability Group hinzugefügt wurde.

Failover | Details
--- | ---
**Geplantes failover** | Geplantes Failover impliziert ein ohne Datenverlust-Failover. Zu SQL Availability Group Verfügbarkeitsmodus zunächst auf synchrone und dann ein Failover ausgelöst wird, damit die verfügbarkeitsgruppe primäre virtuelle Computer während Site Recovery verfügbarkeitsgruppe hinzufügen. Nach Abschluss des Failovers wird Verfügbarkeit auf denselben Wert eingestellt wie vor geplante Failover ausgelöst wurde.
**Ungeplantes failover** | Ungeplantes Failover kann zu Datenverlust führen. Beim Auslösen der ungeplanten Failovers die Verfügbarkeit der Availability Group ist nicht geändert und die It besteht primären virtuellen Computer während Site Recovery verfügbarkeitsgruppe hinzufügen. Ungeplantes Failover abgeschlossen ist und die lokale Server mit SQL Server zur Verfügung steht, hat Reverse Replikation Availability Group ausgelöst werden. Beachten Sie, dass diese Aktion nicht auf den Wiederherstellungsplan kann man auf SQL Availability Group Registerkarte SQL Server
**Testfailover** | Testfailover für SQL Availability Group wird nicht unterstützt. Auslösen von Test-Failover für einen Wiederherstellungsplan mit SQL Availability Group würde Failover für Availability Group übersprungen.


Betrachten Sie diese Failoveroptionen.

Option | Details
--- | ---
**Option 1** | 1. Führen Sie ein testfailover und Front-End-Ebenen.<br/><br/>2. aktualisieren Sie Replikat Kopie im schreibgeschützten Modus Zugriff auf Anwendungsebene und schreibgeschützt testen Sie der Anwendung.
**Option 2** | 1. erstellen Sie 1. eine Kopie der SQL Server-VM Replikatinstanz (mit VMM Klon-Standorten oder Azure Backup) und in einem Testnetzwerk bringen<br/><br/> 2. durchführen Sie mit den Wiederherstellungsplan testfailover.

Schritt 5: Failback

Möchten Sie Availability Group wieder auf den lokalen SQL Server markieren dann möchten indem auslösen geplanten Failover auf den Plan für die Wiederherstellung und die Richtung von Microsoft Azure lokalen VMM-Server.

>[AZURE.NOTE] Umgekehrte Replikation muss nach einem ungeplanten Failover auf die Verfügbarkeit der Replikation wieder ausgelöst werden. Die Replikation bleibt angehalten, bis dies geschehen ist.



### <a name="protect-machines-without-a-vmm-server-or-a-configuration-server"></a>Schützen von Computern ohne VMM-Server oder einem Server Configuration

Für die VMM-Server oder einem Server-Konfiguration nicht verwalteten Umgebungen Azure Automatisierung Runbooks dienen skriptgesteuerte Failover der SQL Verfügbarkeit konfigurieren. Unten sind die konfigurieren:

1.  Erstellen einer lokalen Datei für Ihr Skript über eine verfügbarkeitsgruppe. Dieses Beispielskript gibt den Pfad zu der verfügbarkeitsgruppe Azure Replikat und über die Replikatinstanz nicht. Dieses Skript wird auf dem SQL Server ausgeführt werden Endung benutzerdefiniertes Skript ist Replikat VM übergeben.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2.  Laden Sie das Skript in ein Blob in Azure Storage-Konto. Verwenden Sie dieses Beispiel:

        $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key"
        Set-AzureStorageBlobContent -Blob "AGFailover.ps1" -Container "script-container" -File "ScriptLocalFilePath" -context $context

3.  Erstellen Sie ein Runbook Azure Automatisierung die Skripts auf dem SQL Server-Replikat virtuellen Computer in Azure aufrufen. Verwenden dieses Beispielskript. [Erfahren Sie mehr](site-recovery-runbook-automation.md) über die Verwendung von Automatisierung Runbooks im Recovery-Pläne 

        workflow SQLAvailabilityGroupFailover
        {
            param (
                [Object]$RecoveryPlanContext
            )

            $Cred = Get-AutomationPSCredential -name 'AzureCredential'
    
            #Connect to Azure
            $AzureAccount = Add-AzureAccount -Credential $Cred
            $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
            Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    
            InLineScript
            {
            #Update the script with name of your storage account, key and blob name
            $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key";
            $sasuri = New-AzureStorageBlobSASToken -Container "script-container"- Blob "AGFailover.ps1" -Permission r -FullUri -Context $context;
     
            Write-output "failovertype " + $Using:RecoveryPlanContext.FailoverType;
               
            if ($Using:RecoveryPlanContext.FailoverType -eq "Test")
                {
                #Skipping TFO in this version.
                #We will update the script in a follow-up post with TFO support
                Write-output "tfo: Skipping SQL Failover";
                }
            else
                {
                Write-output "pfo/ufo";
                #Get the SQL Azure Replica VM.
                #Update the script to use the name of your VM and Cloud Service
                $VM = Get-AzureVM -Name "SQLAzureVM" -ServiceName "SQLAzureReplica";     
       
                Write-Output "Installing custom script extension"
                #Install the Custom Script Extension on teh SQL Replica VM
                Set-AzureVMExtension -ExtensionName CustomScriptExtension -VM $VM -Publisher Microsoft.Compute -Version 1.3| Update-AzureVM; 
                    
                Write-output "Starting AG Failover";
                #Execute the SQL Failover script
                #Pass the SQL AG path as the argument.
       
                $AGArgs="-SQLAvailabilityGroupPath sqlserver:\sql\sqlazureVM\default\availabilitygroups\testag";
       
                Set-AzureVMCustomScriptExtension -VM $VM -FileUri $sasuri -Run "AGFailover.ps1" -Argument $AGArgs | Update-AzureVM;
       
                Write-output "Completed AG Failover";

                }
        
            }
        }

4.  Beim Erstellen ein Wiederherstellungsplan für die Anwendung einen "vorab Group 1 Boot" Skript Schritt, der Automatisierung Runbook Verfügbarkeitsgruppen Failover ruft hinzufügen.

## <a name="integrate-protection-with-sql-alwayson-on-premises-to-on-premises"></a>SQL AlwaysOn (lokal lokal) integrieren Sie Schutz

Wenn der SQL Server-Verfügbarkeitsgruppen für hohe Verfügbarkeit oder ein Failover Cluster-Instanz verwenden, empfehlen wir Verfügbarkeitsgruppen am Recovery-Standort. Beachten Sie, dass diese Anleitung für Applikationen, die verteilte Transaktionen nicht verwenden.

1. [Konfigurieren Datenbanken](https://msdn.microsoft.com/library/hh213078.aspx) Verfügbarkeit gruppiert.
2. Erstellen Sie ein neues virtuelles Netzwerk auf sekundären Standort.
3. Ein Standort-zu-Standort-VPN zwischen der primären und neues virtuelles Netzwerk einrichten.
4. Erstellen Sie einen virtuellen Computer am Recovery-Standort und SQL Server installieren.
5. Erweitern Sie vorhandenen AlwaysOn Availability Gruppen zu den neuen virtuellen SQL Server-Computer. Ein asynchrones Replikat Kopie konfigurieren Sie dieser SQL Server-Instanz.
6. Eine verfügbarkeitsgruppenlistener erstellen oder Aktualisieren des vorhandenen Listeners auf asynchrone Replikat virtuellen Computer.
7. Stellen Sie sicher, dass die Anwendungsfarm Setup den Listener. Ist Setup mit den Datenbankservernamen aktualisieren Sie, um den Listener zu verwenden, müssen Sie nicht nach dem Failover konfigurieren.

Anwendung, die verteilte Transaktionen verwenden wir Empfehlung Sie [Site Recovery mit SAN-Replikation](site-recovery-vmm-san.md) oder [VMWare-physische Server Standort-zu-Standort-Replikation](site-recovery-vmware-to-vmware.md)verwenden.

### <a name="recovery-plan-considerations"></a>Aspekte der Recovery-Plans

1. Der VMM-Bibliothek auf die primäre und sekundäre Standorte dieses Beispielskript hinzufügen.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2. Beim Erstellen ein Wiederherstellungsplan für die Anwendung hinzufügen einen "vorab Group 1 Boot" Skript Schritt, der Ihr Skript Verfügbarkeit Gruppen aufruft.



## <a name="protect-a-standalone-sql-server"></a>Eine eigenständige SQL Server schützen

In dieser Konfiguration wird empfohlen, Site Recovery Replikation den SQL Server-Computer schützen können. Die genaue Vorgehensweise hängt von SQL Server als virtuellen oder physischen Server einrichten und ob Sie in Azure replizieren möchten oder eine lokale Website. Erhalten Sie Informationen für alle Bereitstellungsszenarien im [Recovery-Übersicht](site-recovery-overview.md).


## <a name="protect-a-sql-server-cluster-standard-or-2008-r2"></a>Schützen eines SQL Server-Clusters (Standard oder 2008 R2)

Für einen Cluster mit SQL Server Standard Edition oder SQL Server 2008 R2 empfehlen wir Sie Site Recovery Replikation zu SQL Server.

### <a name="on-premises-to-on-premises"></a>Lokal zu lokalen

- Wenn die Anwendung verteilte Transaktionen verwendet empfehlen wir [Site Recovery mit SAN-Replikation](site-recovery-vmm-san.md) für Hyper-V-Umgebung und [VMware-physische Server VMware](site-recovery-vmware-to-vmware.md) VMware Umgebung bereitstellen.

- Nutzen Sie für nicht-DTC Applikationen Wiederherstellen des Clusters als eigenständigen Server mithilfe von lokalen hohen DB Spiegel dieses Vorgehen.

### <a name="on-premises-to-azure"></a>Lokal in Azure

Site Recovery unterstützt keine Clusterunterstützung Gast bei Azure. SQL Server wird auch eine kostengünstige Disaster Recovery-Lösung für Standard Edition bereitstellen. Empfohlen für lokale SQL Server-Cluster eine eigenständige SQL Server schützen und Wiederherstellen in Azure.


1. Konfigurieren Sie eine zusätzliche eigenständige SQL Server-Instanz auf der lokalen Website.
2. Konfigurieren Sie diese Instanz dazu als Spiegel für die Datenbanken, die Schutz benötigen. Konfigurieren Sie die Spiegelung im Modus für hohe Sicherheit.
3.  Konfigurieren von Site Recovery auf der lokalen Website Umgebung ([Hyper-V](site-recovery-hyper-v-site-to-azure.md) oder [VMware-physische Server](site-recovery-vmware-to-azure-classic.md).
4.  Verwenden Sie Site Recovery Replikation die neue SQL Server-Instanz in Azure replizieren. Ist eine hohe Sicherheit Spiegelungskopie und so mit dem primären Cluster synchronisiert werden können, aber werden in Azure Site Recovery Replikation repliziert werden.

Die folgende Grafik zeigt Setup.

![Standard-cluster](./media/site-recovery-sql/BCDRStandaloneClusterLocal.png)


### <a name="failback-considerations"></a>Failback Aspekte

Standard SQL-Cluster wird Failback nach einem ungeplanten Failover erfordert eine SQL-Sicherung und Wiederherstellung von der Spiegelinstanz auf ursprünglichen Cluster und wieder die Spiegelung.

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen](site-recovery-best-practices.md) zu Site Recovery bereitstellen.










 