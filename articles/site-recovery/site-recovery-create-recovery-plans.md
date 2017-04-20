<properties
    pageTitle="Recovery-Pläne erstellen | Microsoft Azure" 
    description="Erstellen Sie Recovery-Pläne mit Azure Site Recovery, Failover und Gruppen von virtuellen Computern und Servern." 
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
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="create-recovery-plans"></a>Erstellen von Recovery-Plänen

Azure Site Recovery-Dienst trägt Ihre Business Continuity und Disaster Recovery (BCDR)-Strategie durch Replikation, Failover und Recovery von virtuellen Computern und Servern orchestriert. Computer können Azure oder einem lokalen sekundären Rechenzentrum repliziert werden. Finden Sie einen schnellen Überblick über [Neuigkeiten Azure Site Recovery?](site-recovery-overview.md).


## <a name="overview"></a>Übersicht

Dieser Artikel enthält Informationen zum Erstellen und Anpassen von Recovery-Plänen. 

Recovery-Pläne bestehen geordnete Gruppen, die virtuellen Computern oder Servern, die gemeinsam ein Failover enthalten. Recovery-Pläne wie folgt:

- Definieren von Gruppen von Computern, die ein Failover und starten Sie dann gemeinsam.
- Modell Abhängigkeiten Maschinen in einer Recovery Plan Gruppe gruppieren. Beispiel wenn Failover und einer bestimmten Anwendung bringen Sie die virtuellen Computer für die Anwendung der Recovery Plan Gruppe Gruppe würde.
- Automatisieren und Erweitern von Failover. Sie können einen Test geplanten oder ungeplanten Failover auf einen Wiederherstellungsplan ausführen. Sie können Recovery-Pläne mit Skripts, Azure Automatisierung und manuelle Aktionen anpassen.

Recovery-Pläne werden auf **Recovery-Pläne** Site Recovery-Portal angezeigt.


Buchen Sie Kommentare oder Fragen am Ende dieses Artikels oder [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Bevor Sie beginnen

Beachten Sie Folgendes:

- Ein Wiederherstellungsplan sollte nicht mit einem oder mehreren Netzwerkadaptern VMs mischen. Ist die VMs mischen in Azure Cloud-Dienst nicht zulässig ist.
- Sie können Recovery-Pläne mit Skripts und manuelle Aktionen erweitern. Beachten Sie Folgendes:
    - Mit Windows PowerShell-Skripts schreiben.
    - Sicherstellen Sie, dass Skripts Try / Catch-Blöcke, dass Ausnahmen ordnungsgemäß behandelt werden. Wenn eine im Skript Ausnahme beendet wird und der Aufgabe wird als fehlgeschlagen.  Wenn ein Fehler auftritt, keine verbleibenden Teil des Skripts ausgeführt. In diesem Fall beim Ausführen eines ungeplanten Failovers weiterhin der Wiederherstellungsplan. In diesem Fall beim Ausführen eines geplanten Failovers beendet der Wiederherstellungsplan. In diesem Fall korrigieren Sie das Skript sicherstellen Sie, dass er wie erwartet ausgeführt und führen Sie den Wiederherstellungsplan erneut aus.
    - Write-Host-Befehl funktioniert nicht in einem Skript Recovery Plan, und das Skript schlägt fehl. Ggf. Ausgabe erstellen Erstellen einer Proxy-Skript, die wiederum Ihre wichtigste Skript ausführt und sicherzustellen, dass alle Ausgaben mit geleitet wird die >> Befehl.
    - Das Skript wird Timeout, wenn nicht innerhalb von 600 Sekunden zurück.
    - Wenn alles in STDERR geschrieben wird, wird das Skript fehlgeschlagen eingestuft. Diese Informationen werden im Skript Ausführungsdetails angezeigt.
    - Beachten Sie, dass, wenn Sie VMM in Ihrer Bereitstellung verwenden:

        - In einem Wiederherstellungsplan Skripte im Kontext des VMM-Dienstkontos. Sicherstellen, dass dieses Konto Leseberechtigungen auf der Remotefreigabe auf dem das Skript gespeichert ist und Testskripts VMM Service Account-Berechtigungsebene ausgeführt.
        - VMM-Cmdlets werden in einem Windows PowerShell-Modul. VMM Windows PowerShell-Modul wird zusammen mit die VMM-Konsole installiert. VMM-Modul in Ihr Skript mit dem folgenden Befehl im Skript geladen werden: Import-Module-Name Virtualmachinemanager. [Weitere Informationen zu erhalten](hhttps://technet.microsoft.com/library/hh875013.aspx).
        - Stellen Sie mindestens Bibliotheksserver in der VMM-Bereitstellung haben. Standardmäßig befindet Bibliothekspfad für VMM-Server lokal auf dem VMM-Server den Namen des Ordners MSCVMMLibrary.
        - Wenn Ihre Bibliothek Freigabepfad ist (oder lokalen aber nicht freigegeben MSCVMMLibrary, konfigurieren Sie die Freigabe wie folgt (mit \\libserver2.contoso.com\share\ als Beispiel):
            - Öffnen Sie den Registrierungseditor, und navigieren Sie zu HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Website Recovery\Registration.
            -  Bearbeiten Sie den Wert ScriptLibraryPath und setzen Sie den Wert als \\libserver2.contoso.com\share\. vollständige vollqualifizierten Domänennamen an. Bereitstellen Sie Berechtigungen für den freigegebenen Speicherort.
            -  Stellen Sie sicher, dass das Skript mit einem Benutzerkonto zu, die die gleichen Berechtigungen wie VMM-Dienstkonto Testen auf eigenständigen getesteten Skripts genauso ausgeführt, dass sie im Recovery-Pläne werden. Legen Sie auf dem VMM-Server die Ausführungsrichtlinie folgendermaßen umgehen:
                -  Öffnen Sie die 64-Bit-Windows PowerShell-Konsole mit erhöhten Berechtigungen.
                -  Typ: **Set-Executionpolicy umgehen**. [Weitere Informationen zu erhalten](https://technet.microsoft.com/library/ee176961.aspx).

## <a name="create-a-recovery-plan"></a>Erstellen eines Wiederherstellungsplans

Das Erstellen eines Recovery-Plans hängt der Site Recovery-Bereitstellung.

- **Virtuelle VMware-Computer replizieren und physische Server**, erstellen Sie einen Plan und fügen Replikationsgruppen, die virtuellen Computern und Servern auf einen Wiederherstellungsplan enthalten.
- **Replizieren von Hyper-V VMs (in VMM-Clouds)**– erstellen Sie einen Plan und einen Wiederherstellungsplan geschützten Hyper-V virtuelle Computer aus einer VMM-Cloud hinzufügen.
- **Replizieren von Hyper-V VMs (nicht in VMM-Clouds)**– erstellen Sie einen Plan und einen Wiederherstellungsplan geschützten Hyper-V virtuelle Computer aus einer Schutzgruppe hinzufügen.
- **SAN-Replikation**– erstellen Sie einen Plan und fügen eine Replikationsgruppe, virtuelle Maschinen der Wiederherstellungsplan enthält. Da alle virtuellen Computer in einer Replikationsgruppe zusammen Failover muss eine Replikationsgruppe anstatt bestimmte virtuelle Computer auswählen (Failover auf der Speicherebene zuerst eintritt).


Erstellen Sie einen Wiederherstellungsplan wie folgt:

1. Klicken Sie auf Registerkarte **Wiederherstellungspläne** > **Wiederherstellungsplan erstellen**.
Geben Sie einen Namen für den Wiederherstellungsplan, Quelle und Ziel. Der Quellserver müssen virtuelle Computer, die für Failover und Recovery aktiviert.

    - Wenn Sie von VMM VMM replizieren **Quellentyp**auswählen > **VMM**und die Quell- und VMM-Server. Klicken Sie auf **Hyper-V** darauf konfiguriert sind Hyper-V Replica Wolken. 
    - Wenn Sie von VMM VMM SAN select **Herkunftsart**replizieren > **VMM**und die Quell- und VMM-Server. Klicken Sie auf **SAN** Wolken sehen, die für die SAN-Replikation konfiguriert sind.
    - Wenn Sie von VMM in Azure replizieren **Quellentyp**auswählen > **VMM**.  VMM Quellserver und **Azure** als Ziel auswählen
    - Wenn Sie von Hyper-V-Website Replikation **Quellentyp**auswählen > **Hyper-V-Website**. Wählen Sie die Site als Quelle und **Azure **als Ziel.
    - Wenn Sie von VMware oder einem lokalen physischen Server in Azure replizieren, wählen Sie einen Konfigurationsserver als Quelle und als Ziel **Azure**

2. Wählen Sie im **virtuellen Computer auswählen** das virtuelle Computer (Replikationsgruppe), die der Standardgruppe (Gruppe 1) in den Wiederherstellungsplan hinzufügen möchten.

## <a name="customize-recovery-plans"></a>Anpassen von Recovery-Plänen

Nach dem hinzugefügten virtuellen Computer oder der Standardgruppe Recovery Plan Replikationsgruppen geschützt und erstellt den Plan anpassen können:

- **Neue Gruppen hinzufügen**, können Sie zusätzliche Recovery Plangruppen hinzufügen. Gruppen, die Sie hinzufügen werden in der Reihenfolge nummeriert, in denen Sie sie hinzufügen. Sie können bis zu sieben Gruppen hinzufügen. Sie können diese neuen Gruppen mehr Maschinen oder Replikationsgruppen hinzufügen. Beachten Sie, dass nur eines virtuellen Computers oder der Replikationsgruppe in einer Speichergruppe Plan aufgenommen werden kann.
- **Hinzufügen eines Skripts **, Skripts, die vor oder nach einer Gruppe planen können. Wenn Sie ein Skript hinzufügen Fügt einen neuen Satz von Aktionen für die Gruppe. Zum Beispiel werden mehrere Vorbereitungsschritte für Gruppe 1 mit dem Namen erstellt: Gruppe 1: Vorbereitungsschritte. In diesem werden alle Vorbereitungsschritte aufgeführt. Beachten Sie, dass Sie nur ein Skript auf dem primären Standort können VMM-Server bereitgestellt haben.
- **Eine manuelle Aktion hinzufügen**– manuelle Aktionen vor nach einer Wiederherstellung Plan Gruppe oder hinzufügen. Beim Ausführen der Wiederherstellungsplan stoppt an dem Punkt, an dem manuelle Aktion eingefügt, und ein Dialogfeld aufgefordert, anzugeben, dass die manuelle Aktion abgeschlossen wurde.

## <a name="extend-recovery-plans-with-scripts"></a>Erweitern von Recovery-Plänen mit Skripts

Sie können Ihren Wiederherstellungsplan ein Skript hinzufügen:

- Haben Sie eine Quellsite VMM, erstellen Sie ein Skript auf dem VMM-Server und in Ihren Wiederherstellungsplan enthalten.
- Wenn Sie in Azure Replikation integrieren Azure Automatisierung Runbooks Sie in Ihren Wiederherstellungsplan

### <a name="create-a-vmm-script"></a>VMM-Skript erstellen


Erstellen Sie das Skript wie folgt:

1. Erstellen Sie einen neuen Ordner in der Bibliotheksfreigabe, z. B. \<VMMServerName > \MSSCVMMLibrary\RPScripts. Auf der Quelle und Ziel VMM-Server.
2. Erstellen Sie das Skript (zum Beispiel RPScript) und überprüfen sie erwartungsgemäß funktioniert.
3. Legen Sie das Skript an der \<VMMServerName > \MSSCVMMLibrary auf die Quell- und VMM-Server.

### <a name="create-an-azure-automation-runbook"></a>Erstellen Sie ein Runbook Azure Automatisierung

Ein Runbook Azure Automatisierung als Teil des Plans ausgeführt, um Ihren Wiederherstellungsplan zu erweitern. [Mehr](site-recovery-runbook-automation.md).


### <a name="add-custom-settings-to-a-recovery-plan"></a>Einen Wiederherstellungsplan benutzerdefinierte Einstellungen hinzufügen

1. Öffnen Sie den Wiederherstellungsplan anpassen möchten.
2. Auf einem virtuellen Computer hinzufügen oder eine neue Gruppe.
3. Ein Skript oder manuell hinzufügen Aktion klicken auf ein Element in der Liste **Schritt** und klicken Sie dann auf **Skript** oder **Manuell**. Geben Sie an, ob das Skript oder die Aktion vor oder nach dem ausgewählten Element hinzufügen möchten. Verwenden Sie die Schaltflächen **Nach oben** und **Nach unten** , um das Skript nach oben oder unten verschieben.
4. Wenn Sie VMM-Skript hinzufügen, geben Sie in **Pfad** den relativen Pfad der freizugebenden wählen Sie **Failover VMM Skript** Für unser Beispiel die Freigabe am \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, geben Sie den Pfad: \RPScripts\RPScript.PS1.
5. Wenn Sie eine Azure Automatisierung führen Buch, geben Sie in dem Runbook befindet, **Azure Automation-Konto** hinzufügen und den entsprechenden **Azure Runbook Skript wählen**.
5. Führen Sie ein Failover für den Wiederherstellungsplan sicherstellen, dass das Skript wie erwartet funktioniert.


## <a name="next-steps"></a>Nächste Schritte

Sie können verschiedene Failover auf Wiederherstellungsplan ein testfailover Überprüfen Ihrer Umgebung und geplanten oder ungeplanten Failover ausführen. [Erfahren Sie mehr](site-recovery-failover.md).


 
