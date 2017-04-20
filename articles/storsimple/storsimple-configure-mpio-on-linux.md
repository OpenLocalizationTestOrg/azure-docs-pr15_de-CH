<properties
   pageTitle="MPIO StorSimple Linux-Host konfigurieren | Microsoft Azure"
   description="Konfigurieren Sie MPIO auf StorSimple mit einem Linux-Server mit CentOS 6.6 verbunden"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a>Konfigurieren Sie MPIO auf einem StorSimple Host unter CentOS

Multipathing e/a (MPIO) auf dem Host-Server Centos 6.6 Konfiguration erforderlichen Schritte erläutert. Der Hostserver ist mit Ihrem Gerät Microsoft Azure StorSimple für hohe Verfügbarkeit über iSCSI-Initiatoren verbunden. Es beschreibt im Detail die automatische Erkennung der Multipath Geräte und bestimmte Setup nur für StorSimple-Volumes.

Dieses Verfahren gilt für alle Modelle StorSimple 8000-Serie Geräte.

>[AZURE.NOTE] Dieses Verfahren kann für ein StorSimple virtuelles Gerät verwendet werden. Weitere Informationen finden Sie unter Hostserver für Ihr virtuelles Gerät konfigurieren.

## <a name="about-multipathing"></a>Über multipathing

Multipathing-Funktion können Sie mehrere i/o-Pfade zwischen einem Server und einem Speichergerät zu konfigurieren. Diese e/a-Pfade sind SAN Anschlüsse, die verschiedene Kabel, Switches, Netzwerkschnittstellen und Domänencontroller aufnehmen können. Multipathing aggregiert die e/a-Pfade Konfigurieren aller aggregierten Pfade zugeordnet ist ein neues Gerät.

Die Multipathing dient zwei:

- **Hohe Verfügbarkeit**: Es bietet einen alternativen Pfad, schlägt ein Element der e/a-Pfad (z. B. Kabel, Switches, Schnittstelle oder Controller).

- **Lastenausgleich**: je nach Konfiguration des Speichergeräts verbessern sie die Leistung auf die e/a-Pfade und dynamischen Ausgleich der Lasten.


### <a name="about-multipathing-components"></a>Multipathing-Komponenten

Multipathing in Linux besteht Kernelkomponenten und Benutzer-Komponenten wie tabellarisch.

- **Kernel**: *Device Mapper* , die e/a leitet und unterstützt Failover Pfade und Pfadgruppen ist.

1. **Benutzerbereich**: *Multipath-Tools* , Multipath-Geräte Verwalten von Multipath-Device Mapper-Modul Vorgehensweise angewiesen, sind. Die Tools umfassen:

    - **Multipath**: Listen und multipathed Geräte konfiguriert.

    - **Multipathd**: Daemon, der Multipath ausgeführt und überwacht die Pfade.

    - **Devmap Name**: Devmaps einen aussagekräftigen Gerätenamen zu Udev bereit.

    - **Kpartx**: Device-Partitionen zu Multipath Karten partitionierbare lineare Devmaps zugeordnet.

    - **Multipath.conf**: Konfigurationsdatei für Multipath-Daemon, mit der die integrierte Konfigurationstabelle überschreiben.

### <a name="about-the-multipathconf-configuration-file"></a>Über die Konfigurationsdatei multipath.conf

Die Konfigurationsdatei `/etc/multipath.conf` macht viele Features Multipathing benutzerdefinierbar. Die `multipath` Befehl und Kernel-Daemon `multipathd` Informationen in dieser Datei gefunden. Die Datei wird nur bei der Konfiguration der Multipath-Geräte gehört. Stellen Sie sicher, dass alle Änderungen vor dem Ausführen der `multipath` Befehl. Wenn Sie die Datei später ändern, müssen Sie beenden und Starten von Multipathd für die Änderung wirksam wird.

Der multipath.conf enthält fünf Abschnitte:

- **Standardeinstellungen auf System** *(Standardwert)*: auf Systemstandards überschreiben.

1. **Blacklisted Geräte** *(schwarze Liste)*: Sie können die Liste der Geräte Device Mapper nicht gesteuert werden soll.

1. **Schwarze Liste Ausnahmen** *(Blacklist_exceptions)*: Erkennen bestimmter Geräte als Multipath Geräte behandelt werden, auch wenn in der schwarzen Liste aufgeführt.

1. **Speicher-Controller-Einstellungen** *(Geräte)*: Konfigurationen, die Geräte angewendet werden, die Hersteller- und Produkt-Informationen festlegen.

1. **Geräte-Einstellungen** *(Multipaths)*: Verwenden Sie diesen Abschnitt, um Konfigurationen für die einzelnen LUNs zu optimieren.

## <a name="configure-multipathing-on-storsimple-connected-to-linux-host"></a>Linux-Host verbunden StorSimple konfigurieren Sie multipathing

StorSimple Geräte von einem Linux-Server kann für hohe Verfügbarkeit und Lastenausgleich konfiguriert werden. Beispielsweise wenn Linux-Server verfügt über zwei Schnittstellen mit dem SAN verbunden und das Gerät verfügt über zwei Schnittstellen mit dem SAN verbunden werden, sodass diese Schnittstellen im selben Subnetz sind, werden dann 4 Pfade verfügbar. Jedoch werden jede Schnittstelle für Geräte und Host-Schnittstelle in einem anderen IP-Subnetz (und nicht routingfähige) hingegen nur 2 Pfade verfügbar sein. Sie können Multipathing, um automatisch alle verfügbaren Pfade entdecken, Lastenausgleichsalgorithmus für diese Pfade auswählen, gelten bestimmte Konfigurationen für nur StorSimple-Volumes aktivieren und überprüfen Multipathing konfigurieren.

Das folgende Verfahren beschreibt das Multipathing konfigurieren, wenn ein StorSimple mit zwei Netzwerkschnittstellen auf einem Server mit zwei Netzwerkschnittstellen verbunden ist.

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Abschnitt werden die erforderliche Konfiguration für CentOS-Server und dem Gerät StorSimple.

### <a name="on-centos-host"></a>Auf CentOS host



1. Stellen Sie sicher, dass die CentOS Host 2 Netzwerkschnittstellen aktiviert ist. Typ:

    `ifconfig`

    Das folgende Beispiel zeigt die Ausgabe, wenn zwei-Schnittstellen Netzwerk (`eth0` und `eth1`) auf dem Host vorhanden sind.

        [root@centosSS ~]# ifconfig
        eth0  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:41  
        inet addr:10.126.162.65  Bcast:10.126.163.255  Mask:255.255.252.0
        inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3341/64 Scope:Global
        inet6 addr: fe80::215:5dff:fea2:3341/64 Scope:Link
        UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
        RX packets:36536 errors:0 dropped:0 overruns:0 frame:0
        TX packets:6312 errors:0 dropped:0 overruns:0 carrier:0
        collisions:0 txqueuelen:1000
        RX bytes:13994127 (13.3 MiB)  TX bytes:645654 (630.5 KiB)

        eth1  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:42  
        inet addr:10.126.162.66  Bcast:10.126.163.255  Mask:255.255.252.0
        inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3342/64 Scope:Global
        inet6 addr: fe80::215:5dff:fea2:3342/64 Scope:Link
        UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
        RX packets:25962 errors:0 dropped:0 overruns:0 frame:0
        TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
        collisions:0 txqueuelen:1000
        RX bytes:2597350 (2.4 MiB)  TX bytes:754 (754.0 b)

        loLink encap:Local Loopback  
        inet addr:127.0.0.1  Mask:255.0.0.0
        inet6 addr: ::1/128 Scope:Host
        UP LOOPBACK RUNNING  MTU:65536  Metric:1
        RX packets:12 errors:0 dropped:0 overruns:0 frame:0
        TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
        collisions:0 txqueuelen:0
        RX bytes:720 (720.0 b)  TX bytes:720 (720.0 b)


1. Installieren Sie *iSCSI-Initiator-Utils* auf dem CentOS-Server. Führen Sie die folgenden Schritte zum Installieren von *iSCSI-Initiator-Utils*.

    1. Melden Sie sich als `root` in die CentOS-Host.

    1. Installieren der *iSCSI-Initiator-Utils*. Typ:

        `yum install iscsi-initiator-utils`


    1. Nach der erfolgreichen der *iSCSI-Initiator-Utils Installation* starten Sie iSCSI-Dienst Typ:

        `service iscsid start`

        Mehrfach `iscsid` tatsächlich startet nicht und `--force` Option erforderlich

    1. Um sicherzustellen, dass der iSCSI-Initiator beim Booten aktiviert ist, verwenden die `chkconfig` Befehl, um den Dienst zu aktivieren.

        `chkconfig iscsi on`


    1. Um zu überprüfen, wurde ordnungsgemäß eingerichtet, führen Sie den Befehl:

        `chkconfig --list | grep iscsi`

        Nachfolgend finden Sie eine Beispielausgabe.

            iscsi   0:off   1:off   2:on3:on4:on5:on6:off
            iscsid  0:off   1:off   2:on3:on4:on5:on6:off

        Aus dem obigen Beispiel sehen Sie, dass der iSCSI-Umgebung auf run-Level 2, 3, 4 und 5 auf Start ausgeführt wird.


1. Installieren Sie *Device Mapper Multipath*. Typ:

    `yum install device-mapper-multipath`

    Die Installation wird gestartet. Geben Sie **Y** weiterhin Aufforderung zur Bestätigung.



### <a name="on-storsimple-device"></a>StorSimple-Gerät

Das StorSimple-Gerät müssen:

- Mindestens zwei Schnittstellen für iSCSI aktiviert. Um sicherzustellen, dass zwei Schnittstellen iSCSI-fähigen Geräts StorSimple, folgendermaßen Sie im klassischen Azure-Portal für Ihr StorSimple-Gerät:

    1. Verwaltungsportal für Ihr StorSimple-Gerät anmelden.

    1. Wählen Sie Ihr StorSimple Manager-Dienst, klicken Sie auf **Geräte** und das Gerät StorSimple. Klicken Sie auf **Konfigurieren** und überprüfen Sie Netzwerk-Schnittstelle. Nachfolgend finden Sie ein Screenshot mit zwei iSCSI-Netzwerk. Hier Daten 2 und Daten 3 beide 10 GbE für iSCSI-Schnittstellen aktiviert sind.

        ![MPIO StorsSimple Daten 2-Konfiguration](./media/storsimple-configure-mpio-on-linux/IC761347.png)

        ![MPIO StorSimple Daten 3 Config](./media/storsimple-configure-mpio-on-linux/IC761348.png)

        Auf der Seite **Konfigurieren**

        1. Sicherstellen Sie, dass beide Netzwerkschnittstellen iSCSI aktiviert. Feld **iSCSI aktiviert** auf **Ja**fest.
        2. Sicherstellen Sie, dass die Netzwerk-Schnittstellen die gleiche Geschwindigkeit aufweisen, beide 1 GbE oder 10 GbE.
        3. Beachten Sie die IPv4-Adressen des iSCSI-aktivierten Schnittstellen und zur späteren Verwendung auf dem Server speichern.


- ISCSI-Schnittstellen auf dem Gerät StorSimple sollte die CentOS-Server erreichbar.

    Hierzu müssen Sie die IP-Adressen der Schnittstellen StorSimple iSCSI-Netzwerk auf dem Hostserver bereitstellen. Die Befehle und der entsprechenden Ausgabe mit DATA2 (10.126.162.25) und DATA3 (10.126.162.26) sind im folgenden dargestellt:

        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target


### <a name="hardware-configuration"></a>Hardware-Konfiguration

Verbinden von zwei iSCSI-Netzwerkschnittstellen getrennte Pfade für Redundanz empfohlen. Die folgende Abbildung zeigt die empfohlene Hardwarekonfiguration für hohe Verfügbarkeit und Lastenausgleich Multipathing für CentOS Server und StorSimple-Gerät.

![MPIO Hardware-Konfiguration für StorSimple auf Linux-Server](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

Wie in der vorherigen Abbildung dargestellt:

- Das StorSimple-Gerät ist in einer Aktiv / Passiv-Konfiguration mit zwei Controllern.

- Gerätecontroller sind zwei SAN-Switches verbunden.

- Zwei iSCSI-Initiatoren auf dem StorSimple-Gerät aktiviert sind.

- Zwei Netzwerkschnittstellen auf dem Host CentOS aktiviert sind.

Die oben genannten Konfiguration führen 4 separate Pfade zwischen dem Gerät und dem Host, wenn Host und Datenschnittstellen geroutet werden.

>[AZURE.IMPORTANT]
>
>- Wir empfehlen Sie 1 GbE und 10 GbE Netzwerkschnittstellen Multipathing nicht mischen. Bei zwei Netzwerkschnittstellen sollte die Schnittstellen verwendeten identisch.
>- Auf dem Gerät StorSimple DATA0, DATA1 DATA4 und DATA5 sind 1 GbE-Schnittstellen DATA2 und DATA3 10 GbE-Netzwerkschnittstellen sind. |

## <a name="configuration-steps"></a>Konfigurationsschritte

Die Konfigurationsschritte für Multipathing erforderlich verfügbaren Pfade für automatische Erkennung angeben den Lastenausgleich Algorithmus, Multipathing aktivieren und schließlich die Konfiguration überprüfen. Jeder dieser Schritte wird in den folgenden Abschnitten ausführlich erläutert.

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a>Schritt 1: Konfigurieren von Multipathing für automatische Erkennung

Multipath-unterstützte Geräte können automatisch erkannt und konfiguriert.

1. Initialisieren `/etc/multipath.conf` Datei. Typ:

     `Copy mpathconf --enable`

    Der obige Befehl erstellt eine `sample/etc/multipath.conf` Datei.


1. Starten Sie Multipath-Service. Typ:

    ``Copy service multipathd start``

    Die folgende Ausgabe wird angezeigt:

    `Starting multipathd daemon:`

1. Ermöglichen Sie automatische Erkennung von Multipaths. Typ:

    `mpathconf --find_multipaths y`

    Dies wird im Abschnitt Standard ändern Ihre `multipath.conf` wie folgt:

        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a>Schritt 2: Konfigurieren von Multipathing für StorSimple-volumes

Standardmäßig werden alle Geräte sind Schwarz in der Datei multipath.conf und übergangen. Sie müssen schwarze Ausnahmen um Multipathing für Volumes von StorSimple-Geräten zu ermöglichen.

1. Bearbeiten der `/etc/mulitpath.conf` Datei. Typ:

    `vi /etc/multipath.conf`

1. Suchen Sie den Abschnitt Blacklist_exceptions in der Datei multipath.conf. Das StorSimple-Gerät muss als schwarze Ausnahme in diesem Abschnitt aufgeführt. Kommentieren Sie relevante Zeilen in dieser Datei ändern wie unten (verwenden Sie das entsprechende Modell des Geräts, das Sie verwenden):

        blacklist_exceptions {
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8100*"
            }
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8600*"
            }
           }

### <a name="step-3-configure-round-robin-multipathing"></a>Schritt 3: Konfigurieren von Round Robin multipathing

Dieser Algorithmus Lastenausgleich verwendet alle verfügbaren Multipaths an den aktiven Controller ausgewogene und Round-Robin.

1. Bearbeiten der `/etc/multipath.conf` Datei. Typ:

    `vi /etc/multipath.conf`

1. Unter den `defaults` Abschnitt, legen Sie die `path_grouping_policy` , `multibus`. Die `path_grouping_policy` gibt den Standardpfad gruppieren Richtlinie nicht angegebener Multipaths zuweisen. Standard-Abschnitt sieht wie unten dargestellt.

        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }



> [AZURE.NOTE]
> Die gängigsten Werte des `path_grouping_policy` gehören:

> - Failover = 1 Pfad pro Priorität
> - Multibus = alle gültigen Pfade in 1 prioritätsgruppe

### <a name="step-4-enable-multipathing"></a>Schritt 4: Aktivieren multipathing

1. Starten der `multipathd` Daemon. Typ:

    `service multipathd restart`

1. Die Ausgabe ist wie folgt:

        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]




### <a name="step-5-verify-multipathing"></a>Schritt 5: Überprüfen Sie multipathing

1. Vergewissern Sie sich, dass iSCSI-Verbindung mit dem Gerät StorSimple hergestellt wird:


    1. Ermitteln Sie Ihr StorSimple Gerät. Typ:

        `iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on the device>:<iSCSI port on StorSimple device>`

        Ausgabe bei der IP-Adresse für DATA0 10.126.162.25 und Port 3260 wird auf dem Gerät StorSimple für ausgehende iSCSI-Verkehr geöffnet ist wie folgt:

            10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
            10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target


        Kopieren der IQN des Geräts StorSimple `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, aus der obigen Ausgabe.



    1. Verbindung zum Gerät mit dem Ziel IQN. StorSimple ist das iSCSI-Ziel. Typ:

        `iscsiadm -m node --login -T <IQN of iSCSI target>`

        Das folgende Beispiel zeigt die Ausgabe mit einem Ziel IQN des `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`. Die Ausgabe gibt an, dass zwei iSCSI-aktivierte Netzwerkschnittstellen auf dem mobilen Gerät hergestellt werden.

            Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
            Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
            Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
            Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
            Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
            Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
            Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
                Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.


        Wenn nur ein Host-Schnittstelle und zwei Pfade hier angezeigt wird, müssen Sie beide Schnittstellen für iSCSI Host aktivieren. Folgen Sie die [detaillierte Anleitung im Linux-Dokumentation](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).


    1. Ein Volume CentOS Server vom Gerät StorSimple ausgesetzt. Weitere Informationen finden Sie unter [Schritt 6: Erstellen Sie ein Volume](storsimple-deployment-walkthrough.md#step-6-create-a-volume) über klassischen Azure-Portal auf Ihrem Gerät StorSimple.

    1. Überprüfen Sie die verfügbaren Pfade. Typ:

        `multipath –l`

        Das folgende Beispiel zeigt die Ausgabe für zwei Netzwerkschnittstellen auf StorSimple Geräte von einer einzigen Host Netzwerkschnittstelle mit zwei verfügbaren Pfade.

            mpathb (36486fd20cc081f8dcd3fccb992d45a68) dm-3 MSFT,STORSIMPLE 8100
            size=100G features='0' hwhandler='0' wp=rw
            `-+- policy='round-robin 0' prio=0 status=active
              |- 7:0:0:1 sdc 8:32 active undef running
              `- 6:0:0:1 sdd 8:48 active undef running

        Das folgende Beispiel zeigt die Ausgabe für zwei Netzwerkschnittstellen auf StorSimple Geräte zwei Host-Netzwerkschnittstellen mit vier verfügbar.

            mpathb (36486fd27a23feba1b096226f11420f6b) dm-2 MSFT,STORSIMPLE 8100
            size=100G features='0' hwhandler='0' wp=rw
            `-+- policy='round-robin 0' prio=0 status=active
              |- 17:0:0:0 sdb 8:16 active undef running
              |- 15:0:0:0 sdd 8:48 active undef running
              |- 14:0:0:0 sdc 8:32 active undef running
              `- 16:0:0:0 sde 8:64 active undef running

        Nachdem die Pfade konfiguriert sind, finden Sie in der Anleitung auf dem Hostbetriebssystem (Centos 6.6) und dieses Volume formatieren.


## <a name="troubleshoot-multipathing"></a>Problembehandlung bei multipathing

Dieser Abschnitt enthält einige nützliche Tipps bei Problemen während der Multipathing-Konfiguration.

Q. Die Änderung nicht angezeigt `multipath.conf` Datei auswirkt.

A. Wenn vorgenommenen Änderungen an der `multipath.conf` -Datei muss den Multipathing-Service neu starten. Geben Sie folgenden Befehl ein:

    service multipathd restart

Q. Ich habe zwei Netzwerkschnittstellen auf dem Gerät StorSimple und zwei Netzwerkschnittstellen auf dem Host aktiviert. Beim Auflisten der verfügbaren Pfade nur zwei Pfade angezeigt. Ich voraussichtlich verfügbaren Pfade vier.

A. Stellen Sie sicher, dass die beiden Pfade in demselben Subnetz und routingfähig. Wenn Netzwerkschnittstellen auf verschiedenen vLANs und nicht routingfähige Sie zwei Pfade sehen. Eine Möglichkeit hierzu ist sicherzustellen, dass Sie sowohl die Host-Schnittstellen aus einer Netzwerkschnittstelle auf dem Gerät StorSimple erreichen können. Sie werden als diese Überprüfung [Support von Microsoft](storsimple-contact-microsoft-support.md) über eine Sitzung erfolgt benötigen.

Q. Beim Auflisten verfügbaren Pfade sehe ich keine Ausgabe.

A. In der Regel ein Problem mit der Multipathing-Daemon schlägt Multipath Pfade nicht angezeigt und ist wahrscheinlich ein Problem liegt in der `multipath.conf` Datei.

Es wäre auch lohnt, dass Sie einige Laufwerke sehen können nach dem Anschließen an das Ziel als keine Antwort vom Multipath Angebote haben Datenträger nicht auch bedeuten.

- Verwenden Sie folgenden Befehl auf den SCSI-Bus scannen:

    `$ rescan-scsi-bus.sh `(Teil des sg3_utils-Pakets)

- Geben Sie die folgenden Befehle:

    `$ dmesg | grep sd*`

- Oder

    `$ fdisk –l`

    Diese werden Details der kürzlich hinzugefügten Datenträger zurück.

- Um festzustellen, ob ein StorSimple Datenträger ist, verwenden Sie die folgenden Befehle:

    `cat /sys/block/<DISK>/device/model`

    Dies gibt eine Zeichenfolge zurück, ob StorSimple Datenträger ist.

Weniger wahrscheinlich, aber mögliche Ursache könnte veraltete Iscsid pid. Verwenden Sie den folgenden Befehl von iSCSI-Sitzung abmelden:

    iscsiadm -m node --logout -p <Target_IP>

Wiederholen Sie diesen Befehl für alle verbundenen Netzwerkschnittstellen auf dem iSCSI-Ziel StorSimple Geräts. Nachdem Sie alle iSCSI-Sessions abgemeldet haben, mit der das iSCSI-Ziel IQN iSCSI-Sitzung wiederherzustellen. Geben Sie folgenden Befehl ein:

    iscsiadm -m node --login -T <TARGET_IQN>


Q. Ich bin nicht sicher, ob das Gerät auf der weißen Liste.

A. Überprüfen, ob das Gerät auf der weißen Liste befindet, verwenden Sie den folgenden Problembehandlung interaktiven Befehl:

    multipathd –k
    multipathd> show devices
    available block devices:
    ram0 devnode blacklisted, unmonitored
    ram1 devnode blacklisted, unmonitored
    ram2 devnode blacklisted, unmonitored
    ram3 devnode blacklisted, unmonitored
    ram4 devnode blacklisted, unmonitored
    ram5 devnode blacklisted, unmonitored
    ram6 devnode blacklisted, unmonitored
    ram7 devnode blacklisted, unmonitored
    ram8 devnode blacklisted, unmonitored
    ram9 devnode blacklisted, unmonitored
    ram10 devnode blacklisted, unmonitored
    ram11 devnode blacklisted, unmonitored
    ram12 devnode blacklisted, unmonitored
    ram13 devnode blacklisted, unmonitored
    ram14 devnode blacklisted, unmonitored
    ram15 devnode blacklisted, unmonitored
    loop0 devnode blacklisted, unmonitored
    loop1 devnode blacklisted, unmonitored
    loop2 devnode blacklisted, unmonitored
    loop3 devnode blacklisted, unmonitored
    loop4 devnode blacklisted, unmonitored
    loop5 devnode blacklisted, unmonitored
    loop6 devnode blacklisted, unmonitored
    loop7 devnode blacklisted, unmonitored
    sr0 devnode blacklisted, unmonitored
    sda devnode whitelisted, monitored
    dm-0 devnode blacklisted, unmonitored
    dm-1 devnode blacklisted, unmonitored
    dm-2 devnode blacklisted, unmonitored
    sdb devnode whitelisted, monitored
    sdc devnode whitelisted, monitored
    dm-3 devnode blacklisted, unmonitored


Weitere Informationen zum Wechseln Sie [zur Problembehandlung interaktiven Befehl für das multipathing](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html)

## <a name="list-of-useful-commands"></a>Liste der hilfreichen Befehlen

|Typ|Befehl|Beschreibung|
|---|---|---|
|**iSCSI**|`service iscsid start`|ISCSI-Dienst starten|
|&nbsp;|`service iscsid stop`|ISCSI-Dienst beenden|
|&nbsp;|`service iscsid restart`|ISCSI-Dienst|
|&nbsp;|`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>`|Verfügbare Ziele an der angegebenen Adresse erkennen|
|&nbsp;|`iscsiadm -m node --login -T <TARGET_IQN>`|Das iSCSI-Ziel anmelden|
|&nbsp;|`iscsiadm -m node --logout -p <Target_IP>`|Melden Sie vom iSCSI-Ziel ab|
|&nbsp;|`cat /etc/iscsi/initiatorname.iscsi`|ISCSI-Initiatornamen drucken|
|&nbsp;|`iscsiadm –m session –s <sessionid> -P 3`|Überprüfen Sie den Status der iSCSI-Sitzung und Volume auf dem Host gefunden|
|&nbsp;|`iscsi –m session`|Zeigt alle iSCSI-Sitzung zwischen dem Host und dem Gerät StorSimple|
| | | |
|**Multipathing**|`service multipathd start`|Starten Sie Multipath-daemon|
|&nbsp;|`service multipathd stop`|Multipath-Daemon beenden|
|&nbsp;|`service multipathd restart`|Multipath-Daemon starten|
|&nbsp;|`chkconfig multipathd on` </br> ODER </br> `mpathconf –with_chkconfig y`|Aktivieren Sie Multipath-Daemon beim Booten starten|
|&nbsp;|`multipathd –k`|Starten Sie die interaktive Konsole zur Problembehandlung|
|&nbsp;|`multipath –l`|Liste Multipath-Anschlüsse und Geräte|
|&nbsp;|`mpathconf --enable`|Erstellen Sie eine Beispieldatei mulitpath.conf in`/etc/mulitpath.conf`|
||||

## <a name="next-steps"></a>Nächste Schritte

Wie Sie MPIO auf Linux-Server konfigurieren, müssen Sie folgende CentoS 6.6 Dokumente verweisen:

- [MPIO auf CentOS einrichten](http://www.centos.org/docs/5/html/5.1/DM_Multipath/setup_procedure.html)
- [Linux Leitfaden](http://linux-training.be/files/books/LinuxAdm.pdf)
