<properties
    pageTitle="Hyper-V-Kapazitätsplanertools für Site Recovery ausführen | Microsoft Azure"
    description="Dieser Artikel enthält Informationen zur Verwendung des Hyper-V-Kapazitätsplanertools für Azure Site Recovery"
    services="site-recovery"
    documentationCenter="na"
    authors="rayne-wiselman"
    manager="jwhit"
    editor="" />
<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/12/2016"
    ms.author="raynew" />

# <a name="run-the-hyper-v-capacity-planner-tool-for-site-recovery"></a>Hyper-V-Kapazitätsplanertools für Site Recovery ausführen

Als Teil der Azure Site Recovery-Bereitstellung müssen Sie herausfinden, die Replikation und Bandbreite. Hyper-V-Kapazitätsplanertools für Site Recovery können Sie herausfinden, die Replikation und Bandbreite an Hyper-V-VM-Replikation.


Dieser Artikel beschreibt das Hyper-V-Kapazitätsplanertools ausführen. Dieses Tool sollte zusammen mit anderen Planungstools Kapazität und Angaben in [Planung für Site Recovery](site-recovery-capacity-planner.md)verwendet werden.


## <a name="before-you-start"></a>Bevor Sie beginnen

Sie führen auf einem Hyper-V Server oder Cluster-Knoten im primären Standort. Führen Sie das Tool Hyper-V-Host-Server muss:

- Betriebssystem: Windows Server® 2012 oder Windows Server® 2012 R2
- Speicher: 20 MB (Minimum)
- CPU: 5 Prozent Aufwand (Minimum)
- Festplattenspeicher: 5 MB (Minimum)

Vor dem Ausführen des Tools müssen Sie den primären Standort vorbereiten. Wenn Sie Replikation zwischen zwei lokalen Sites und Bandbreite überprüfen möchten, müssen Sie auch einen Replikatserver vorbereiten.


## <a name="step-1-prepare-the-primary-site"></a>Schritt 1: Vorbereiten des primären Standorts
1. Der primäre Standort stellen Sie eine Liste aller Hyper-V virtuelle Computer zu replizieren und Hyper-V-Hosts/Cluster auf denen sie sich befinden. Das Tool kann jeweils für mehrere eigenständige Hosts oder einem Cluster jedoch nicht beide zusammen ausgeführt. Es muss separat für jedes Betriebssystem ausgeführt, so sollten Sie erfassen und beachten Sie die Hyper-V-Server wie folgt:

  - Windows Server® 2012 Standalone--Server
  - Windows Server® 2012-Clustern
  - Windows Server® 2012 R2 Standalone--Server
  - Windows Server® 2012 R2-Cluster

3. Remotezugriff zu WMI auf dem Hyper-V-Hosts und Cluster. Führen Sie diesen Befehl auf jeden Servercluster zu Sie Firewall-Regeln und Berechtigungen festlegen:

        netsh firewall set service RemoteAdmin enable

5. Aktivieren Sie die Leistungsindikatoren auf Server und Cluster, wie folgt:

  - Die Windows-Firewall mit **Erweiterter Sicherheit** -Snap-in öffnen und aktivieren Sie dann die folgenden eingehenden Regeln: **COM+-Netzwerkzugriff (DCOM-IN)** und alle Regeln in der **Gruppe Remote Event Log-Management**.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Schritt 2: Bereiten Sie einen Replikatserver (lokal für lokale Replikation vor)

Sie müssen hierzu in Azure Replikation.

Wir empfehlen, dass Sie einen einzelnen Hyper-V-Host als Wiederherstellungsserver so einrichten, dass dummy VM auf Bandbreite überprüfen repliziert werden kann.  Überspringen Sie diese, aber Sie wird Bandbreite messen, wenn Sie dies tun.

1. Möchten Sie das Replikat Konfigurieren von Hyper-V Replica Broker einen Clusterknoten verwenden:

    - Öffnen Sie im **Server-Manager** **Failovercluster-Manager**.
    - Cluster verbunden, markieren Sie den Clusternamen, und klicken Sie auf **Aktionen** > **Rolle konfigurieren** , öffnen Sie den Assistenten für hohe Verfügbarkeit.
    - Klicken Sie auf **Hyper-V Replica Broker**Rolle **Auswählen** . Geben Sie im Assistenten einen **NetBIOS-Namen** und **IP-Adresse** als Verbindungspunkt für den Cluster (Clientzugriffspunkt genannt) verwendet werden. **Hyper-V Replica Broker** wird Client Access Point ein, die Sie beachten sollten, was konfiguriert werden.
    - Überprüfen Sie die Rolle für Hyper-V Replica Broker erfolgreich online geschaltet und kann ein Failover zwischen allen Knoten des Clusters. Dazu klicken Sie mit der rechten Maustaste die Rolle **zu**zeigen Sie und klicken Sie auf **Knoten auswählen**. Wählen Sie einen Knoten > **OK**.
    - Wenn Sie zertifikatbasierte Authentifizierung verwenden, stellen Sie sicher, jeder Knoten des Clusters und der Client Zugriffspunkt haben alle das Zertifikat installiert.
2.  Aktivieren Sie einen Replikatserver:

    - Ein Cluster Fehler Cluster-Manager öffnen, mit dem Cluster herstellen und auf **Rollen** > select Rolle > **Einstellung**s > **Aktivieren dieses als Replikatserver**. Beachten Sie, dass bei Verwendung ein Clusters als Replikat Sie Hyper-V Replica Broker Rolle vorhanden im Cluster im primären Standort haben müssen.
    - Ein eigenständiger Server öffnen Sie Hyper-V-Manager. Klicken Sie im **Aktionsbereich** klicken Sie **Hyper-V** Server aktivieren möchten und Konfiguration der **Replikation** auf **diesen Computer aktivieren als einen Replikatserver**.
3. Authentifizierung einrichten:

    - **Authentifizierung** und Ports wählen Sie den primären Server und Anschlüsse Authentifizierung authentifizieren aus. Verwenden Sie Zertifikat klicken Sie auf **Zertifikat auswählen** , um einen auszuwählen. Verwenden Sie Kerberos in derselben Domäne oder vertrauenswürdiger Domänen sind die primären und Recovery Hyper-V-Hosts. Verwenden von Zertifikaten für verschiedene Domänen oder Bereitstellung in einer Arbeitsgruppe.
    - **Autorisierung** und Speicher können Sie **Alle** authentifizierten (Primärserver) dieser Replikatserver Replikationsdaten an. Klicken Sie auf **OK** oder auf **Übernehmen**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

    - Führen Sie zum Prüfen, ob der Listener für den Protokollport läuft angegebene **Netsh http Servicestate anzeigen** :  
4. Richten Sie Firewalls. Während der Installation der Hyper-V werden Firewall-Regeln auf die Anschlüsse (HTTPS auf 443, Kerberos 80) Datenverkehr erstellt. Aktivieren Sie diese Regeln wie folgt:

        - Certificate authentication on cluster (443): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}**
        - Kerberos authentication on cluster (80): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}**
        - Certificate authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"**
        - Kerberos authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"**

## <a name="step-3-run-the-capacity-planner-tool"></a>Schritt 3: Ausführen des Kapazitätsplanertools

Nach dem primären Standort vorbereitet und richten Sie einen Wiederherstellungsserver können Sie das Tool ausführen.

1. [Herunterladen](https://www.microsoft.com/download/details.aspx?id=39057) des Tools im Microsoft Download Center.
2. Ausführen eines primären Server (oder einen Knoten aus dem primären Cluster). Maustaste auf die .exe-Datei und wählen Sie **als Administrator ausführen**.
3. Geben Sie **vor der Installation** für wie lange Sie Daten sammeln möchten. Wir empfehlen, führen Sie das Tool während den Produktionsstunden um sicherzustellen, dass Daten repräsentativ ist. Wenn Sie nur Netzwerkkonnektivität überprüfen möchten, können Sie nur eine Minute sammeln.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4. **Primärdatenbank** Details Geben Sie den Servernamen oder FQDN für einen eigenständigen Server oder ein Clusters geben den vollqualifizierten Domänennamen des Clients akzeptieren Punkt, Clusternamen und alle Knoten im Cluster und klicken Sie auf **Weiter**. Das Tool erkennt automatisch den Namen des Servers ausgeführt wird. Das Tool nimmt VMs für die angegebenen Server überwacht werden können.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5. Wählen Sie im **Replikat-Site-Details** Wenn Replikation in Azure oder zu einem sekundären Rechenzentrum repliziert sind und noch keinen Replikatserver einrichten, **Tests mit Replikat-Site überspringen**. Wenn Sie auf einem sekundären Rechenzentrum replizieren und ein Replikat im FQDN der eigenständige Server oder der Clientzugriffspunkt für den Cluster **Servername (**oder) Hyper-V Replica Broker CAP eingerichtet haben.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. **Erweiterte Replikat** Details aktivieren Sie **mit erweiterten Replikat-Site Tests überspringen**. Sie werden von Site Recovery nicht unterstützt.
7. **Wählen Sie** VMs zu replizierenden Tools verbindet den Server oder Cluster VMs und zeigt Laufwerke auf dem primären Server entsprechend der Angabe auf der Seite **primäre Site** . Hinweis Replikation oder bereits aktiviert VMs ausführen wird angezeigt. Wählen Sie die VMs, für die Sie Metriken sammeln möchten. Die VHDs automatisch auswählen sammelt für die virtuellen Computer zu.
9. Wenn Sie ein Replikat-Server oder Cluster konfiguriert haben, im **Netzwerk** Geben Sie die ungefähre WAN-Bandbreite Denken Sie zwischen primären und Replikat verwendet werden und wählen Sie Zertifikate Zertifikatauthentifizierung konfiguriert haben.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

10. **Zusammenfassung** Einstellungen Sie, und klicken Sie auf **Weiter** um erfassen Metriken. Tool Fortschritt und wird auf der Seite **Kapazität berechnen** . Wenn Tool beendet unter **Bericht** über das Ergebnis zu klicken. Standardmäßig werden Berichte und Protokolle im **Planer %systemdrive%\Users\Public\Documents\Capacity**gespeichert.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)


## <a name="step-4-interpret-the-results"></a>Schritt 4: Interpretieren der Ergebnisse
Hier sind wichtigen Kennzahlen. Metriken, die hier nicht aufgeführt sind, können ignoriert werden. Sie sind nicht für Site Recovery.

### <a name="on-premises-to-on-premises-replication"></a>Räume für lokale Replikation
  - Auswirkung der Replikation auf den primären Server Compute, Speicher
  - Auswirkung der Replikation auf dem primären Server Recovery Storage Speicherplatz IOPS
  - Gesamte Bandbreite für Deltareplikation (Mbit/s)
  - Beobachtete Netzwerkbandbreite zwischen den primären Server und dem Recovery Host (Mbit/s)
  - Vorschlag für die ideale Anzahl der aktiven parallelen Transfers zwischen den beiden Hosts /

### <a name="on-premises-to-azure-replication"></a>Lokale Replikation Azure
  - Auswirkung der Replikation auf den primären Server Compute, Speicher
  - Auswirkung der Replikation auf den primären Server Speicher Speicherplatz IOPS
  - Gesamte Bandbreite für Deltareplikation (Mbit/s)

## <a name="more-resources"></a>Weitere Ressourcen

- Ausführliche Informationen finden Sie im Dokument, das begleitende Tools herunterladen.
- Eine exemplarische Vorgehensweise mit dem Keith Mayers [TechNet-Blog](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx)ansehen
- [Die Ergebnisse](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) unserer Leistungstests für lokale Replikation von lokalen Hyper-V



## <a name="next-steps"></a>Nächste Schritte

Nach Planung können Sie die Bereitstellung von Site Recovery starten:

- [Hyper-V virtuelle Computer in VMM-Clouds in Azure replizieren](site-recovery-vmm-to-azure.md)
- [Replikation von Hyper-V VMs (ohne VMM) in Azure](site-recovery-hyper-v-site-to-azure.md)
- [Hyper-V VMs zwischen VMM Standorten replizieren](site-recovery-vmm-to-vmm.md)
- [Replikation von Hyper-V VMs zwischen VMM mit SAN](site-recovery-vmm-san.md)
- [Replizieren Sie hyper-V VMs auf einzelnen VMM-server](site-recovery-single-vmm.md)