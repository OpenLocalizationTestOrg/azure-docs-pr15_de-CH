<properties
    pageTitle="Planen der Kapazität zum Schutz von virtuellen Computern und Servern in Azure Site Recovery | Microsoft Azure"
    description="Azure Site Recovery koordiniert die Replikation, Failover und Recovery von virtuellen Computern und Servern am lokalen Azure oder lokalen sekundären Standort." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="07/12/2016" 
    ms.author="raynew"/>

# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Planen der Kapazität zum Schutz von virtuellen Computern und Servern in Azure Site Recovery

Azure Site Recovery Capacity Planner-Tool können Sie herausfinden, der Kapazitätsbedarf für den Schutz von Hyper-V virtuelle Computer, virtuelle VMware-Computer und Windows-Linux-Servern mit Azure Site Recovery.


## <a name="overview"></a>Übersicht

Verwenden der Site Recovery Kapazitätsplaner um den Quell-Umgebung und Arbeitslast analysieren und herauszufinden, Bandbreite benötigt, müssen Sie an Ihrem Quellspeicherort Serverressourcen und Ressourcen (virtuellen Maschinen und Speicher usw.), die Sie an Ihr Ziel. 

Das Tool kann in einigen Modi ausgeführt werden:

- **Schnelle Planung**: das Tool in diesem Modus zu Netzwerk- und Projektionen auf einer durchschnittlichen Anzahl von VMs, Festplatten, Speicher und Änderungsrate basierend ausführen.
- **Detaillierte Planung**: in diesem Modus ausführen und Einzelheiten jede Arbeitslast auf VM-Ebene. Analysieren von VM-Kompatibilität und Netzwerk- und Projektionen zu erhalten.

## <a name="before-you-start"></a>Bevor Sie beginnen

Bevor Sie das Tool ausführen:

1. Informationen Sie zu Ihrer Umgebung VMs Laufwerke pro VM pro Festplatte.
2. Identifizieren Sie die tägliche (Churn) Änderungsrate für replizierte Daten. Dazu:

    - Wenn Sie Hyper-V-VMs sind replizieren [Planungstool für Hyper-V-Kapazität](https://www.microsoft.com/download/details.aspx?id=39057) der Änderungsrate zu downloaden. [Weitere Informationen](site-recovery-capacity-planning-for-hyper-v-replication.md) zu diesem Tool. Wir empfehlen, dieses Tool über eine Woche Durchschnittswerte erfassen ausführen.
    - Replikation virtuelle VMware-Maschinen verwenden Sie [vSphere Kapazität Planung Appliance](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) Abwanderung Rate herauszufinden.
    - Wenn Sie physische Server replizieren können müssen Sie manuell zu schätzen.

## <a name="run-the-quick-planner"></a>Schnellen Planer ausführen
1.  Downloaden und [Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) -Tool öffnen. Sie müssen Makros wählen bearbeiten und Inhalt der Aufforderung ausgeführt. 
2.  Wählen Sie in **Wählen Sie einen Planer** **Schnell Planer** aus dem Listenfeld.

    ![Erste Schritte](./media/site-recovery-capacity-planner/getting-started.png)

3.  Geben Sie im Arbeitsblatt **Capacity Planner** die erforderlichen Informationen ein. Alle im folgenden Screenshot rot eingekreist Felder müssen ausgefüllt werden.

    - Wählen Sie in **Ihrem Szenario auswählen** **Hyper-V in Azure** oder **VMware/physische in Azure**.
    - In **durchschnittlichen täglichen Datenänderungsrate (%)** Geben Sie die Informationen gesammelt, die mit dem [Planungstool für Hyper-V-Kapazität](site-recovery-capacity-planning-for-hyper-v-replication.md) oder [vSphere Kapazität Appliance planen](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance).  
    - **Komprimierung** ist nur für Komprimierung bei VMware-VMs oder physische Server in Azure angeboten. Wir schätzen 30 % oder mehr jedoch die Einstellung nach Bedarf ändern. Zum Replizieren von Hyper-V VMs Azure-Komprimierung können Sie eine Anwendung von Drittanbietern wie Riverbed. 
    -  Geben Sie **Aufbewahrung Eingaben** wie lange Replikate beibehalten werden soll. Sie sind VMware replizieren oder physische Server Geben Sie den Wert in Tagen. Wenn Sie Hyper-V-Replikation die Zeit in Stunden angeben.
    -  **Anzahl der Stunden in die anfängliche Replikation für den Stapel von virtuellen Maschinen sollte abgeschlossen** und **Anzahl virtueller Computer pro Batch Erstreplikation** Geben Sie Einstellungen Erstreplikation Bedarf berechnet.  Beim Bereitstellen von Site Recovery sollte die gesamte Anfangsbestand hochgeladen werden. 

    ![Eingaben](./media/site-recovery-capacity-planner/inputs.png)

2.  Nachdem Sie die Werte für die Quell-Umgebung setzen, umfasst angezeigte Ausgabe:

    - **Bandbreite für Delta-Replikation erforderlich** (MB/s). Netzwerkbandbreite Deltareplikation errechnet die durchschnittlichen täglichen Datenänderungsrate.
    - **Für die erste Replikation erforderlichen Bandbreite** (MB/s). Netzwerkbandbreite für die erste Replikation wird die erste Replikation Werte berechnet in setzen. 
    - **Speicher** ist (in GB) die gesamte Azure-Speicher erforderlich.
    - **Total IOPS standard Speicherkonten** ist auf 8 K IOPS Größe insgesamt Standardspeicher Konten berechnet.  Für die schnell Planer, den die Anzahl berechnet, basierend auf dem Quelldatenträger VMs und täglichen Daten Änderungsrate. Detaillierte Planer die Anzahl berechnet basierend auf VMs, die mit standardmäßigen Azure VMs und Daten ändern Sie auf die VMs. 
    - **Anzahl der standardspeicherkonten** stellt die Gesamtzahl der standardspeicherkonten zu VMs. Beachten Sie, dass ein standard Speicherkonto kann bis zu 20.000 IOPS über alle virtuellen Computer in einer standardmäßigen Speicher und maximal 500 IOPS pro Datenträger unterstützt. 
    - **Anzahl der BLOB-Datenträger** gibt die Anzahl der Festplatten auf den Azure-Speicher erstellt werden.
    - **Anzahl der Premium-Speicherkonten** stellt die Gesamtzahl der Premium-Speicherkonten VMs schützen. Beachten Sie, dass eine Quelle VM mit hohen IOPS (mehr als 20000) ein Speicherkonto Premium. Ein Premium-Speicherkonto kann bis zu 80.000 IOPS enthalten.
    - **Total IOPS Premium-Speicher** ist auf 256 K IOPS Größe auf insgesamt Premium-Speicherkonten berechnet.  Für die schnell Planer, den die Anzahl berechnet, basierend auf dem Quelldatenträger VMs und täglichen Daten Änderungsrate. Für detaillierte Planer die Anzahl richtet auf VMs Premium Azure VM (DS und GS-Serie) zugeordnet sind und die Datenänderungsrate auf die VMs. 
    - **Anzahl der Konfigurationsserver** zeigt wie viele Konfigurationsserver für die Bereitstellung (1)
    - **Anzahl zusätzlicher Server** zeigt an, ob weitere Server neben den Prozess-Server erforderlich sind, die standardmäßig auf dem Konfigurationsserver konfiguriert ist.
    - **zusätzlicher Speicher der Quelle 100 %** zeigt an, ob am Quellspeicherort zusätzlicher Speicher erforderlich ist.
            
    ![Ausgabe](./media/site-recovery-capacity-planner/output.png)
 
## <a name="run-the-detailed-planner"></a>Detaillierten Planer ausführen


1.  Downloaden und [Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) -Tool öffnen. Sie müssen Makros wählen bearbeiten und Inhalt der Aufforderung ausgeführt. 
2.  Wählen Sie im **Planer auswählen** **Detaillierte Planer** aus dem Listenfeld aus.

    ![Erste Schritte](./media/site-recovery-capacity-planner/getting-started-2.png)

3.  Geben Sie die erforderlichen Informationen in das Arbeitsblatt **Arbeitslast** . Sie müssen alle Felder ausfüllen.

    - In **Prozessorkerne** wird Geben Sie die Gesamtzahl der Kerne auf einem Quellserver an.
    - Zuweisung von **Speicher in MB** Geben Sie des Arbeitsspeichers Quellserver an 
    - **Anzahl Netzwerkkarten** geben die Anzahl der Netzwerkadapter auf einem Quellserver. 
    -  In **Gesamtspeicher (in GB)** gibt die Gesamtgröße des VM-Speicher. Zum Beispiel wenn der Quellserver 3 Festplatten mit 500 GB Gesamtspeichergröße aufweist, 1500 GB.
    -  Anzahl **von Datenträgern** Geben Sie die Gesamtzahl der Laufwerke des Quellservers.
    -  Geben Sie **Festplatten-Auslastung** die durchschnittliche Auslastung.
    -  Geben Sie **tägliche Änderungsrate (% an)** die täglichen Daten Quellserver ändern.
    -  **Zuordnen von Azure Größe** geben Azure VM-Größe, die Sie zuordnen möchten. Klicken Sie auf**Berechnen IaaS VMs**, möchten Sie diese jetzt. Beachten Sie, dass wenn manuelle Einstellung eingeben und dann auf berechnen IaaS VMs manuelle Einstellung überschrieben, möglicherweise weil Compute-Prozess automatisch die beste Übereinstimmung Azure virtueller Speicher bezeichnet.

    ![Workload-Qualifikation](./media/site-recovery-capacity-planner/workload-qualification.png)

4.  Klicken Sie wird hier **Berechnen IaaS VMs** was:

    - Überprüft die Pflichtfelder.
    - IOPS berechnet und empfiehlt die beste Azure VM aize Übereinstimmung für die einzelnen VMs für Replikation in Azure. Wenn eine entsprechende Größe des Azure VM kann, ein Fehler ausgegeben erkannt werden. Wenn z. B. die Anzahl der Festplatten in angeschlossenen 65 ein Fehler Probleme seit der höchsten Größe ist Azure VM 64.
    - Schlägt ein Speicherkonto für eine VM Azure verwendet werden kann.
    - Berechnet die Gesamtzahl Standardspeicher und Premium-Speicherkonten für die Arbeitslast erforderlich. Unten Sie rechts Azure Speichertyp und das Speicherkonto für einen Quellserver verwendet werden kann
    - Abgeschlossen ist und den Rest der Tabelle basierend auf erforderliche Speichertyp (standard oder Premium) zugewiesen für einen virtuellen Computer und die Anzahl von Datenträgern sortiert. Zeigt Ja für alle virtuellen Computer, die die Unterstützung Azure Spalte A (ist VM qualifiziert?) erfüllen. Wenn eine VM gesichert werden kann nicht wird Azure ein Fehler angezeigt.

Spalten AA AE ausgegeben und Informationen für jede VM.

![Workload-Qualifikation](./media/site-recovery-capacity-planner/workload-qualification-2.png)


### <a name="example"></a>Beispiel
Das Tool beispielsweise für sechs virtuelle Computer, die Werte in der Tabelle berechnet und weist die beste Übereinstimmung Azure VM und Azure Speicherbedarf.

![Workload-Qualifikation](./media/site-recovery-capacity-planner/workload-qualification-3.png)

- In der Ausgabe für dieses Beispiel ist Folgendes zu beachten:
    
    - Die erste Spalte ist eine Spalte Validierung für VMs, Festplatten und Abwanderung.
    - Zwei standard-Speicherkonten und einer Premium-Speicherkonto sind für fünf VMs erforderlich. 
    -  VM3 nicht Schutz geeignet ist, da eine oder mehrere Festplatten mit mehr als 1 TB sind.
    -  VM1 und VM2 können das erste standardspeicherkonto
    -  VM4 können die zweite Standardspeicher verwenden.
    -  VM5 und VM6 benötigen ein Speicherkonto Premium und können sowohl ein einzelnes Konto.

    >[AZURE.NOTE]  IOPS Standard- und Premium-Speicher werden auf VM-Ebene und nicht auf Festplatte berechnet. Standardmäßigen virtuellen Computer können bis zu 500 IOPS pro Festplatte behandeln. Wenn IOPS Datenträger größer als 500 benötigen Sie Premium-Speicher. Jedoch werden einem Standard Wenn IOPS Datenträger mehr als 500 aber IOPS der gesamten VM-Laufwerke die Unterstützung standard Azure VM (VM Größe, Anzahl der Festplatten, Anzahl der Adapter, CPU, Speicher) dann Planer sind VM nicht den DS oder GS-Serie. Sie müssen die Zuordnung Azure Größe Zelle mit entsprechenden GS oder DS Serie VM manuell aktualisieren.

5. Nachdem alle Details sind, klicken Sie auf **Daten senden, mit dem Planer** **Capacity Planner** öffnen Arbeitslasten sind hervorgehoben, um anzuzeigen, ob sie schutzfähig oder nicht sind.


### <a name="submit-data-in-the-capacity-planner"></a>Daten Sie in der Kapazitätsplaner

1.  Beim Öffnen des Arbeitsblatts **Capacity Planner** wird es auf der Basis angegebene aufgefüllt. Das Wort 'Arbeit' in der Zelle **Infrarotanschluss Eingaben Quelle** zeigen, dass das Eingabe **Arbeitslast** Arbeitsblatt angezeigt. 
2.  Möchten Sie ändern müssen Sie das **Arbeitslast** Arbeitsblatt ändern und erneut auf Daten senden, mit dem Planer.  

    ![Capacity Planner](./media/site-recovery-capacity-planner/capacity-planner.png)


