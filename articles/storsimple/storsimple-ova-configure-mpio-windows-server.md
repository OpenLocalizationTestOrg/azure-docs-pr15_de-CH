<properties 
   pageTitle="MPIO auf dem StorSimple virtual Array Host konfigurieren | Microsoft Azure"
   description="Multipfad-e/a (MPIO) für die StorSimple Konfiguration Verbindung virtuelles Array mit einem Host unter Windows Server 2012 R2 beschrieben."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>Konfigurieren Sie Multipfad-e/a auf Windows Server-Host für StorSimple Virtual Array

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt das Feature Multipfad-e/a (MPIO) auf Ihre Windows-Server installieren, gelten bestimmte Konfigurationen für nur StorSimple-Volumes und überprüfen MPIO StorSimple-Volumes. Es wird vorausgesetzt, dass Ihr StorSimple 1200 Virtual Array mit zwei Netzwerkschnittstellen auf einem Windows-Server mit zwei Netzwerkschnittstellen verbunden ist. Die Informationen in diesem Artikel gilt nur für virtuelle Array. Informationen zur StorSimple-8000-Serie Geräte finden Sie [MPIO StorSimple Host konfigurieren](storsimple-configure-mpio-windows-server.md). 

Die MPIO-Funktion in Windows Server hilft hochverfügbare, fehlertolerante Speicherung Buildkonfigurationen. MPIO verwendet physikalischen Pfad redundante Komponenten-Adapter, Kabel und Switches – logische Pfade zwischen dem Server und dem Speichergerät erstellt. Ist einem Komponentenausfall verursacht einen logischen Pfad fehlschlägt, verwendet Multipathing Logik einen alternativen Pfad für e/a, sodass Programme noch auf ihre Daten zugreifen können. Außerdem kann je nach Konfiguration Ihres MPIO auch Leistungssteigerung erneut die Last auf alle Pfade. Weitere Informationen finden Sie in der [MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO-Überblick und Funktionen").  

Konfigurieren Sie für hohe Verfügbarkeit der Lösung StorSimple MPIO auf den Windows Server-Hosts StorSimple 1200 Virtual Array (auch bekannt als lokalen virtuellen Gerät) angeschlossen. Die Hostserver können dann eine Verknüpfung, Netzwerk oder Schnittstellenfehler tolerieren. 

Sie müssen diese Schritte MPIO konfigurieren: 

- Erforderliche Konfiguration

- Schritt 1: Installation MPIO auf Windows Server

- Schritt 2: Konfigurieren von MPIO für StorSimple-volumes

- Schritt 3: Mount StorSimple-Volumes auf dem host

Die Schritte werden in den folgenden Abschnitten erläutert.


## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Abschnitt werden die Konfiguration erforderlichen Komponenten für die Windows Server-Host und virtual Array.

### <a name="on-windows-server-host"></a>Auf Windows-Server

-  Stellen Sie sicher, dass Ihre Windows Server-Host 2 Netzwerkschnittstellen aktiviert.


### <a name="on-storsimple-virtual-array"></a>Virtuelle StorSimple Array

- Virtuelle Array sollte als iSCSI-Server konfiguriert werden. Informationen finden Sie unter [virtual Array als iSCSI-Server einrichten](storsimple-ova-deploy3-iscsi-setup.md). Das Array sollte ein oder mehrere Netzwerkschnittstellen aktiviert.   

- Netzwerkschnittstellen virtual Array sollte aus der Windows Server-Host erreichbar.

- Ein oder mehrere Volumes sollte auf Ihrem virtuellen StorSimple-Array erstellt werden. Weitere finden Sie unter [Hinzufügen eines Volumes](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume) auf Ihrem virtuellen StorSimple 1200-Array. In diesem Verfahren erstellt 3 Volumes (2 lokal fixierte und 1 tiered Volume siehe unten) auf das virtual Array.
    
    ![mpio0](./media/storsimple-ova-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Hardwarekonfiguration für virtuelle StorSimple array

Die nachfolgende Abbildung zeigt die Hardwarekonfiguration für hohe Verfügbarkeit und Lastenausgleich Multipathing für Windows Server-Host und virtual Array StorSimple in dieser Prozedur verwendet.  

![MPIO-Hardwarekonfiguration](./media/storsimple-ova-configure-mpio-windows-server/1200hardwareconfig.png)

Wie in der vorherigen Abbildung dargestellt:

- Virtuelle StorSimple Array auf Hyper-V bereitgestellt ist ein Knoten aktiv Gerät als iSCSI-Server konfiguriert.

- Arrays sind zwei virtuellen Netzwerkschnittstellen aktiviert. Überprüfen Sie in der lokalen Web-Benutzeroberfläche des Arrays virtuellen 1200 zwei Netzwerkschnittstellen Navigieren zu **Network Settings** wie folgt aktiviert werden:

    ![Netzwerkschnittstellen auf 1200 aktiviert](./media/storsimple-ova-configure-mpio-windows-server/mpio9.png)
    
    Beachten Sie die IPv4-Adressen von Netzwerk-Schnittstellen (Ethernet 2 Ethernet) und zur späteren Verwendung auf dem Server speichern.

- Auf Ihrem Windows Server werden zwei Netzwerkschnittstellen aktiviert. Wenn verbundenen Schnittstellen für Host und im gleichen Subnetz sind, dann werden 4 Pfade verfügbar. Dies war bei diesem Verfahren. Jedoch werden jeder Netzwerkschnittstelle auf Array- und Host-Schnittstelle in einem anderen IP-Subnetz (und nicht routingfähige) hingegen nur 2 Pfade verfügbar sein.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Schritt 1: Installation MPIO auf Windows Server

MPIO ist eine optionale Funktion in Windows Server und wird standardmäßig nicht installiert. Er sollte als ein Feature über Server-Manager installiert werden. Gehen Sie folgendermaßen vor, um diese Funktion auf Ihrem Windows Server installieren.

[AZURE.INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]


## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Schritt 2: Konfigurieren von MPIO für StorSimple-volumes

MPIO muss StorSimple Volumes ermitteln konfiguriert werden. Konfigurieren Sie MPIO StorSimple-Volumes erkennen Schritte.

[AZURE.INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Schritt 3: Mount StorSimple-Volumes auf dem host

Nach der Konfiguration MPIO auf Windows Server Volumes auf dem StorSimple-Array erstellt montiert werden und können dann nutzen MPIO Redundanz. Führen Sie die folgenden Schritte, um ein Volume bereitzustellen.

#### <a name="to-mount-volumes-on-the-host"></a>Bereitstellen von Volumes auf dem host

1. Öffnen Sie das Fenster **iSCSI Initiator Properties** auf Windows-Server. Klicken Sie auf **Server Manager > Dashboard > Extras > iSCSI Initiator**.
2. Klicken Sie im Dialogfeld **iSCSI Initiator Properties** klicken Sie auf die Registerkarte Discovery und klicken Sie dann auf **Zielportal erkennen**.
3. Klicken Sie im Dialogfeld **Zielportal erkennen** folgendermaßen Sie vor:
    
    - Geben Sie die IP-Adresse der ersten Netzwerk-Schnittstelle auf Ihrem virtuellen StorSimple-Array. Standardmäßig wäre **Ethernet**. 
    - Klicken Sie auf **OK** , um das Dialogfeld **iSCSI Initiator Properties** zurückzukehren.

    >[AZURE.IMPORTANT] **Wenn Sie ein privates Netzwerk für iSCSI-Verbindung verwenden, geben Sie die IP-Adresse des Daten-Ports, die mit dem privaten Netzwerk verbunden ist.**

4. Wiederholen Sie die Schritte 2-3 für eine zweite Netzwerkschnittstelle (z. B. Ethernet-2) für das Array. 

5. Wählen Sie auf die Registerkarte **Targets** im Dialogfeld **iSCSI Initiator Properties** . Für das virtual Array sollte jede Fläche Volume als Ziel unter **Erkannte Ziele**angezeigt werden. In diesem Fall würde drei Ziele (entspricht drei Volumes) erkannt.

    ![MPIO1](./media/storsimple-ova-configure-mpio-windows-server/mpio1.png)

6. Klicken Sie auf **Verbinden** , um eine iSCSI-Sitzung mit Ihrem StorSimple. Das Dialogfeld **Verbindung mit Ziel** wird angezeigt. Aktivieren Sie das Kontrollkästchen **Multipath aktivieren** . Klicken Sie auf **Erweitert**.

    ![mpio2](./media/storsimple-ova-configure-mpio-windows-server/mpio2.png)

8. Klicken Sie im Dialogfeld **Erweiterte Einstellungen** folgendermaßen Sie vor:                                       
    -    **Lokale Adapter** Dropdown-Liste Wählen Sie **Microsoft iSCSI-Initiator**.
    -    **Initiator-IP-** Dropdown-Liste Wählen Sie die IP-Adresse des Hosts aus.
    -    **Zielportal** IP-Dropdown-Liste Wählen Sie die IP-Adresse des Array-Schnittstelle.
    -    Klicken Sie auf **OK** , um das Dialogfeld **iSCSI Initiator Properties** zurückzukehren.

    ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

9. Klicken Sie auf **Eigenschaften**. 

    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)
10. Klicken Sie im Dialogfeld **Eigenschaften** auf **Sitzung hinzufügen**.

    ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)

10. Klicken Sie im Dialogfeld **Verbindung auf** das Kontrollkästchen Sie **Multipath aktivieren** . Klicken Sie auf **Erweitert**.
11. Im Dialogfeld " **Erweiterte Einstellungen** ":                                        
    -  **Lokale Adapter** Dropdown-Liste Wählen Sie Microsoft iSCSI-Initiator.
    -  Wählen Sie in der Dropdownliste **Initiator IP** die IP-Adresse für den Host aus In diesem Fall verbinden Sie zwei Netzwerkschnittstellen auf dem Array zu einer einzelnen Netzwerkschnittstelle auf dem Host. Daher ist diese Schnittstelle für die erste Sitzung identisch.
    -  **Target Portal IP** Dropdown-Liste Wählen Sie die IP-Adresse für die zweite Schnittstelle im Array aktiviert aus.
    -  Klicken Sie auf **OK** , um das Dialogfeld iSCSI Initiator Properties zurückzukehren. Sie haben das Ziel eine zweite Sitzung hinzugefügt.

        ![mpio11](./media/storsimple-ova-configure-mpio-windows-server/mpio11.png)

    - Nach dem Hinzufügen der gewünschten Sitzungen (Pfade) im Dialogfeld **iSCSI Initiator Properties** , wählen Sie das Ziel, und klicken Sie auf **Eigenschaften**. Beachten Sie in dem Register Sessions im Dialogfeld **Eigenschaften** vier Sitzung Bezeichner Permutationen Pfad entsprechen. Eine Sitzung abbrechen, das Kontrollkästchen neben einem Sitzungsbezeichner und klicken Sie dann auf **Trennen**.
 
    - Um Geräte innerhalb Sessions anzuzeigen, wählen Sie die Registerkarte **Geräte** . Klicken Sie auf **MPIO**konfigurieren Sie MPIO-Richtlinie für eine ausgewählte Gerät. Die **
    -  **Details wird angezeigt. Auf der **MPIO** Registerkarte können Sie die entsprechende **Lastenausgleichsrichtlinie** Settings. Außerdem können die **aktiv** oder **Standby ** Pfad.

10. Wiederholen Sie die Schritte 8 bis 11 Ziel zusätzliche Sessions (Pfade) hinzu. Fügen Sie zwei Schnittstellen auf dem Host, auf dem virtual Array insgesamt vier Sitzungen für jedes Ziel. 

    ![mpio14](./media/storsimple-ova-configure-mpio-windows-server/mpio14.png)

11. Sie müssen diese Schritte für jedes Volume (Flächen als Ziel) wiederholen.

    ![mpio15](./media/storsimple-ova-configure-mpio-windows-server/mpio15.png)

12. Öffnen Sie **Computer Management** durch Navigieren zur **Server Manager > Dashboard > Computer Management**. Klicken Sie im linken Bereich auf **Storage > Disk Management**. Die für diesen Host Volumes auf virtuelle StorSimple Array erstellt wird unter **Datenträger** als neuer Datenträger angezeigt.

13. Initialisieren Sie den Datenträger, und erstellen Sie ein neues Volume. Wählen Sie während der Formatierungsvorgang Größe der Zuordnungseinheiten (AUS) von 64 KB. Wiederholen Sie den Vorgang für alle Volumes.

    ![Datenträger](./media/storsimple-ova-configure-mpio-windows-server/mpio20.png)

14. **Datenträger** **Festplatte** Maustaste und wählen **Eigenschaften**.

15. Klicken Sie im Dialogfeld **Eigenschaften Multipath** **MPIO** -Registerkarte.

    ![Datenträgereigenschaften](./media/storsimple-ova-configure-mpio-windows-server/mpio21.png)

16. Im Abschnitt **DSM Name** auf **Details** , und überprüfen Sie, ob die Standard-Parameter festgelegt wurden. Die sind:

    - Pfad überprüfen Periode = 30
    - Anzahl der Wiederholungsversuche = 3
    - Entfernen von PDOS Periode = 20
    - Wiederholungsintervall = 1
    - Pfad überprüfen aktiviert = deaktiviert.

    >[AZURE.NOTE] **Ändern Sie die Standardparameter.**


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur [Verwendung des StorSimple Manager-Dienstes zum Verwalten der virtuellen StorSimple-Array](storsimple-ova-manager-service-administration.md).
 
