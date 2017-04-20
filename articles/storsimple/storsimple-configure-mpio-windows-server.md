<properties 
   pageTitle="MPIO für Ihr StorSimple-Gerät konfigurieren | Microsoft Azure"
   description="Beschreibt das Konfigurieren von Multipfad-e/a (MPIO) für das StorSimple-Gerät mit einem Host unter Windows Server 2012 R2."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-for-your-storsimple-device"></a>Konfigurieren von Multipath i/o für Ihr StorSimple-Gerät

Microsoft Unterstützung für das Feature Multipfad-e/a (MPIO) in Windows Server Hilfe hochverfügbare, fehlertolerante SAN Buildkonfigurationen erstellt. MPIO verwendet physikalischen Pfad redundante Komponenten-Adapter, Kabel und Switches – logische Pfade zwischen dem Server und dem Speichergerät erstellt. Ist einem Komponentenausfall verursacht einen logischen Pfad fehlschlägt, verwendet Multipathing Logik einen alternativen Pfad für e/a, sodass Programme noch auf ihre Daten zugreifen können. Außerdem kann je nach Konfiguration Ihres MPIO auch Leistungssteigerung erneut die Last auf alle Pfade. Weitere Informationen finden Sie in der [MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO-Überblick und Funktionen").  

Hohe Verfügbarkeit der Lösung StorSimple sollte MPIO auf dem StorSimple-Gerät konfiguriert werden. MPIO auf die Hostserver mit Windows Server 2012 R2 installiert ist, können die Server dann ein Link, Netzwerk oder Versagen der Benutzeroberfläche tolerieren. 

MPIO ist eine optionale Funktion in Windows Server und wird standardmäßig nicht installiert. Er sollte als ein Feature über Server-Manager installiert werden. Dieses Thema beschreibt die Schritte zum Installieren und Verwenden der MPIO-Funktion auf einem Server unter Windows Server 2012 R2 befolgen und ein physisches Gerät StorSimple an.

>[AZURE.NOTE] **Dieses Verfahren gilt für StorSimple-8000-Serie. MPIO ist derzeit nicht auf einem virtuellen Gerät StorSimple unterstützt.**

Sie müssen konfigurieren MPIO auf Ihrem Gerät StorSimple folgendermaßen:

- Schritt 1: Installation MPIO auf Windows Server

- Schritt 2: Konfigurieren von MPIO für StorSimple-volumes

- Schritt 3: Mount StorSimple-Volumes auf dem host

- Schritt 4: Konfigurieren Sie MPIO für hohe Verfügbarkeit und Lastenausgleich

Die Schritte werden in den folgenden Abschnitten erläutert.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Schritt 1: Installation MPIO auf Windows Server

Gehen Sie folgendermaßen vor, um diese Funktion auf Ihrem Windows Server installieren.

#### <a name="to-install-mpio-on-the-host"></a>MPIO auf dem Host installiert

1. Öffnen Sie Server-Manager auf Ihrem Windows Server. Server-Manager startet standardmäßig ein Mitglied der Administratoren Protokolle auf einem Computer mit Windows Server 2012 R2 oder Windows Server 2012. Wenn der Server-Manager nicht geöffnet ist, klicken Sie auf **Start > Server-Manager**.
![Server-Manager](./media/storsimple-configure-mpio-windows-server/IC740997.png)
2. Klicken Sie auf **Server Manager > Dashboard > Rollen und Features hinzufügen**. Dies startet den Assistenten zum **Hinzufügen von Rollen und Features** .
![Hinzufügen von Rollen und Features Assistent 1](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. Folgendermaßen Sie im Assistenten zum **Hinzufügen von Rollen und Features** vor:

    - Klicken Sie auf " **Weiter**" auf der Seite **vor** .
    - Akzeptieren Sie die Standardeinstellung der **Rolle oder Funktion-basierten** Installation, auf der Seite **Installationstyp auswählen** . Klicken Sie auf **Weiter**. ![Rollen und Features Assistent 2 hinzufügen](./media/storsimple-configure-mpio-windows-server/IC740999.png)
    - Wählen Sie auf der Seite **Wählen Sie Zielserver** **Wählen Sie einen Server aus dem Server**. Der Hostserver sollte automatisch erkannt werden. Klicken Sie auf **Weiter**.
    - Klicken Sie auf der Seite **Serverrollen auswählen** auf **Weiter**.
    - Wählen Sie auf der Seite **Funktionen wählen** **Multipfad-e/a**, und klicken Sie auf **Weiter**. ![Rollen und Features Assistent 5 hinzufügen](./media/storsimple-configure-mpio-windows-server/IC741000.png)
    - Bestätigen Sie auf der Seite **Installationsauswahl bestätigen** , und wählen Sie dann **Starten Sie den Zielserver ggf. automatisch**wie folgt. Klicken Sie auf **Installieren**. ![Rollen und Features Assistent 8 hinzufügen](./media/storsimple-configure-mpio-windows-server/IC741001.png)
    - Sie werden benachrichtigt, wenn die Installation abgeschlossen ist. Klicken Sie auf **Schließen** , um den Assistenten zu schließen. ![Rollen und Features Assistent 9 hinzufügen](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Schritt 2: Konfigurieren von MPIO für StorSimple-volumes

MPIO muss StorSimple Volumes ermitteln konfiguriert werden. Konfigurieren Sie MPIO StorSimple-Volumes erkennen Schritte.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>So konfigurieren Sie MPIO für StorSimple-volumes

1. Die **MPIO-Konfiguration**zu öffnen. Klicken Sie auf **Server Manager > Dashboard > Extras > MPIO**.

2. Wählen Sie im Dialogfeld **Eigenschaften von MPIO** Registerkarte **Multipfade** .

3. **Unterstützung für iSCSI-Geräte**wählen Sie aus und dann auf **Hinzufügen**.  
![Eigenschaften von MPIO erkennen mehrere Pfade](./media/storsimple-configure-mpio-windows-server/IC741003.png)

4. Starten Sie den Server, wenn Sie aufgefordert werden.
5. Klicken Sie im Dialogfeld **Eigenschaften von MPIO** klicken Sie auf die Registerkarte **MPIO-Geräte** . Klicken Sie auf **Hinzufügen**.
    </br>![MPIO Eigenschaften MPIO Geräte](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. Geben Sie im Dialogfeld **MPIO-Unterstützung hinzufügen** unter **Gerätehardware-ID**Geräte-Seriennummer ein Seriennummer des Geräts können mit Zugriff auf Ihren Dienst StorSimple Manager navigieren zu erhalten **Geräte > Dashboard**. Seriennummer des Geräts wird im rechten Bereich des Dashboards Gerät **Kurzer Blick** angezeigt.
    </br>![Die MPIO-Unterstützung hinzufügen](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Starten Sie den Server, wenn Sie aufgefordert werden.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Schritt 3: Mount StorSimple-Volumes auf dem host

Nach der Konfiguration MPIO auf Windows Server Volumes auf dem StorSimple-Gerät erstellt montiert werden und können dann nutzen MPIO Redundanz. Führen Sie die folgenden Schritte, um ein Volume bereitzustellen.

#### <a name="to-mount-volumes-on-the-host"></a>Bereitstellen von Volumes auf dem host

1. Öffnen Sie das Fenster **iSCSI Initiator Properties** auf Windows-Server. Klicken Sie auf **Server Manager > Dashboard > Extras > iSCSI Initiator**.
2. Klicken Sie im Dialogfeld **iSCSI Initiator Properties** klicken Sie auf die Registerkarte Discovery und klicken Sie dann auf **Zielportal erkennen**.
3. Klicken Sie im Dialogfeld **Zielportal erkennen** folgendermaßen Sie vor:
    
    - Die IP-Adresse des DATA-Port des Geräts StorSimple (z. B. Dateneingabe 0).
    - Klicken Sie auf **OK** , um das Dialogfeld **iSCSI Initiator Properties** zurückzukehren.

    >[AZURE.IMPORTANT] **Wenn Sie ein privates Netzwerk für iSCSI-Verbindung verwenden, geben Sie die IP-Adresse des Daten-Ports, die mit dem privaten Netzwerk verbunden ist.**

4. Wiederholen Sie die Schritte 2 und 3 für eine zweite Netzwerkschnittstelle (z. B. Daten 1) auf dem Gerät. Denken Sie daran, dass diese Schnittstellen für iSCSI aktiviert werden soll. Weitere Informationen hierzu finden Sie unter [Netzwerkschnittstellen ändern](storsimple-modify-device-config.md#modify-network-interfaces).
5. Wählen Sie auf die Registerkarte **Targets** im Dialogfeld **iSCSI Initiator Properties** . StorSimple-Zielgerät IQN unter **Erkannte Ziele**sollte angezeigt werden.
 ![iSCSI-Initiator Eigenschaftenregisterkarte Ziele](./media/storsimple-configure-mpio-windows-server/IC741007.png)
6. Klicken Sie auf **Verbinden** , um eine iSCSI-Sitzung mit dem StorSimple-Gerät herstellen. Das Dialogfeld **Verbindung mit Ziel** wird angezeigt.

7. Klicken Sie im Dialogfeld **Verbindung auf** das Kontrollkästchen Sie **Multipath aktivieren** . Klicken Sie auf **Erweitert**.

8. Klicken Sie im Dialogfeld **Erweiterte Einstellungen** folgendermaßen Sie vor:                                       
    -    **Lokale Adapter** Dropdown-Liste Wählen Sie **Microsoft iSCSI-Initiator**.
    -    **Initiator-IP-** Dropdown-Liste Wählen Sie die IP-Adresse des Hosts aus.
    -    **Zielportal** IP-Dropdown-Liste Wählen Sie die IP-Schnittstelle.
    -    Klicken Sie auf **OK** , um das Dialogfeld **iSCSI Initiator Properties** zurückzukehren.

9. Klicken Sie auf **Eigenschaften**. Klicken Sie im Dialogfeld **Eigenschaften** auf **Sitzung hinzufügen**.
10. Klicken Sie im Dialogfeld **Verbindung auf** das Kontrollkästchen Sie **Multipath aktivieren** . Klicken Sie auf **Erweitert**.
11. Im Dialogfeld " **Erweiterte Einstellungen** ":                                        
    -  **Lokale Adapter** Dropdown-Liste Wählen Sie Microsoft iSCSI-Initiator.
    -  Wählen Sie in der Dropdownliste **Initiator IP** die IP-Adresse für den Host aus In diesem Fall verbinden Sie zwei Netzwerkschnittstellen auf dem Gerät zu einer einzelnen Netzwerkschnittstelle auf dem Host. Daher ist diese Schnittstelle für die erste Sitzung identisch.
    -  **Target Portal IP** Dropdown-Liste Wählen Sie die IP-Adresse für die zweite Schnittstelle auf dem Gerät aktiviert aus.
    -  Klicken Sie auf **OK** , um das Dialogfeld iSCSI Initiator Properties zurückzukehren. Sie haben das Ziel eine zweite Sitzung hinzugefügt.

12. Öffnen Sie **Computer Management** durch Navigieren zur **Server Manager > Dashboard > Computer Management**. Klicken Sie im linken Bereich auf **Storage > Disk Management**. Die Volumes auf dem StorSimple-Gerät erstellt, die für diesen Host wird als neue Festplatten **Datenträger** Verwaltung angezeigt.

13. Initialisieren Sie den Datenträger, und erstellen Sie ein neues Volume. Wählen Sie bei Format eine Blockgröße von 64 KB.
![Datenträger](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. **Datenträger** **Festplatte** Maustaste und wählen **Eigenschaften**.
15. Im Modell StorSimple ### **Multipath Eigenschaften** -Dialogfeld auf die Registerkarte **MPIO** .
![StorSimple 8100 Multipfad-Datenträger DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)

16. Im Abschnitt **DSM Name** auf **Details** , und überprüfen Sie, ob die Standard-Parameter festgelegt wurden. Die sind:

    - Pfad überprüfen Periode = 30
    - Anzahl der Wiederholungsversuche = 3
    - Entfernen von PDOS Periode = 20
    - Wiederholungsintervall = 1
    - Pfad überprüfen aktiviert = deaktiviert.


>[AZURE.NOTE] **Ändern Sie die Standardparameter.**

## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>Schritt 4: Konfigurieren Sie MPIO für hohe Verfügbarkeit und Lastenausgleich

Für Multi-Path hohe Verfügbarkeit und Lastenausgleich, müssen mehrere Sitzungen manuell hinzugefügt werden, die verschiedenen Pfade deklarieren. Z. B. verfügt der Host zwei Schnittstellen mit SAN verbunden und das Gerät verfügt über zwei Schnittstellen mit SAN verbunden, müssen Sie vier Sitzungen mit richtigen Pfad Permutationen (nur zwei ist erforderlich, wenn jede Schnittstelle und Host-Schnittstelle ein anderes IP-Subnetz und nicht routingfähig ist) konfiguriert.

>[AZURE.IMPORTANT] **Wir empfehlen Sie 1 GbE und 10 GbE-Netzwerkschnittstellen nicht mischen. Verwenden Sie zwei Netzwerkschnittstellen, sollten beide Schnittstellen identisch Typ sein.**

Im folgenden beschrieben Sessions hinzufügen, wenn ein StorSimple mit zwei Netzwerkschnittstellen auf einem Server mit zwei Netzwerkschnittstellen verbunden ist.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>MPIO für hohe Verfügbarkeit konfigurieren und Lastenausgleich

1. Ermittlung des Ziels ausführen: Klicken Sie im Dialogfeld **iSCSI Initiator Properties** auf der Registerkarte **Erkennung** auf **Portal erkennen**.
2. Klicken Sie im Dialogfeld **Verbindung auf** die IP-Adresse eines Geräts Netzwerkschnittstellen.
3. Klicken Sie auf **OK** , um das Dialogfeld **iSCSI Initiator Properties** zurückzukehren.

4. Klicken Sie im Dialogfeld **iSCSI Initiator Properties** wählen Sie auf die Registerkarte **Targets** markieren Sie erkannte Ziel und klicken Sie auf **Verbinden**. Das Dialogfeld **Verbinden auf** .

5. Klicken Sie im Dialogfeld **Verbindung auf** :
    
    - Lassen Sie ausgewählte Standardziel für **diese Verbindung hinzufügen** zur Liste der bevorzugten Zielen festlegen. Damit wird das Gerät automatisch versuchen, die Verbindung bei jedem dieses Computers Neustart zu starten.
    - Aktivieren Sie das Kontrollkästchen **Multipath aktivieren** .
    - Klicken Sie auf **Erweitert**.

6. Im Dialogfeld " **Erweiterte Einstellungen** ":
    - **Lokale Adapter** Dropdown-Liste Wählen Sie **Microsoft iSCSI-Initiator**.
    - **Initiator-IP-** Dropdown-Liste Wählen Sie die IP-Adresse des Hosts aus.
    - **Target Portal IP** Dropdown-Liste Wählen Sie die IP-Adresse der Schnittstelle auf dem Gerät aktiviert aus.
    - Klicken Sie auf **OK** , um das Dialogfeld iSCSI Initiator Properties zurückzukehren.

7. Klicken Sie auf **Eigenschaften**, und klicken Sie im Dialogfeld **Eigenschaften** auf **Sitzung hinzufügen**.

8. Klicken Sie im Dialogfeld **Verbindung auf** **Multipath aktivieren** das Kontrollkästchen und klicken Sie dann auf **Erweitert**.

9. Im Dialogfeld " **Erweiterte Einstellungen** ":
    1. **Lokale Adapter** Dropdown-Liste Wählen Sie **Microsoft iSCSI-Initiator**.
    2. Wählen Sie in der Dropdownliste **Initiator IP** die IP-Adresse für die zweite Schnittstelle auf dem Host aus
    3. **Target Portal IP** Dropdown-Liste Wählen Sie die IP-Adresse für die zweite Schnittstelle auf dem Gerät aktiviert aus.
    4. Klicken Sie auf **OK** , um das Dialogfeld **iSCSI Initiator Properties** zurückzukehren. Sie haben nun eine zweite Sitzung zum Ziel hinzugefügt.

10. Wiederholen Sie Schritte 8 bis 10 für weitere Sessions (Pfade) an das Ziel. Mit zwei Schnittstellen auf dem Host und zwei auf dem Gerät können Sie insgesamt vier Sitzungen hinzufügen.

11. Nach dem Hinzufügen der gewünschten Sitzungen (Pfade) im Dialogfeld **iSCSI Initiator Properties** , wählen Sie das Ziel, und klicken Sie auf **Eigenschaften**. Beachten Sie in dem Register Sessions im Dialogfeld **Eigenschaften** vier Sitzung Bezeichner Permutationen Pfad entsprechen. Eine Sitzung abbrechen, das Kontrollkästchen neben einem Sitzungsbezeichner und klicken Sie dann auf **Trennen**.

12. Um Geräte innerhalb Sessions anzuzeigen, wählen Sie die Registerkarte **Geräte** . Klicken Sie auf **MPIO**konfigurieren Sie MPIO-Richtlinie für eine ausgewählte Gerät. Das Dialogfeld **Gerät** wird angezeigt. Wählen Sie auf der Registerkarte **MPIO** **Lastenausgleichsrichtlinie** Einstellungen. Sie können auch Pfadtext **aktiv** oder **Standby** anzeigen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur [Verwendung des StorSimple Manager-Dienstes die Gerätekonfiguration StorSimple ändern](storsimple-modify-device-config.md).
 
