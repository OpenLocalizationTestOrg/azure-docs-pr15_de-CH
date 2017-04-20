<properties
   pageTitle="StorSimple Gerät Regierungsportal bereitstellen | Microsoft Azure"
   description="Beschreibt die Schritte und best Practices zur Bereitstellung StorSimple Update 2 Geräte und Dienste in Azure Government-Portal."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/16/2016"
   ms.author="v-sharos" />

# <a name="deploy-your-on-premises-storsimple-device-in-the-government-portal-update-2"></a>Bereitstellen des lokalen StorSimple Geräts in das Portal der Behörde (Update 2)

[AZURE.INCLUDE [storsimple-version-selector-deploy-gov](../../includes/storsimple-version-selector-deploy-gov.md)]

## <a name="overview"></a>Übersicht

Willkommen Sie bei Microsoft Azure StorSimple gerätebereitstellung. Diese Bereitstellungslernprogramme gelten für Azure Regierungsportal Software Update 2 auf StorSimple-8000-Serie. Dieser Serie von Lernprogrammen enthält eine Konfigurationsprüfliste, eine Konfiguration erforderlichen Komponenten und einzelnen Konfigurationsschritte für das StorSimple-Gerät.

In diesen Lernprogrammen wird davon ausgegangen, dass Sie die Sicherheitsvorkehrungen überprüft entpackt, eingerichtet und StorSimple-Gerät verbunden. Möchten Sie weiterhin die Aufgaben, beginnen Sie mit [Sicherheitsvorkehrungen](storsimple-safety.md). Anweisungen Sie die gerätespezifischen entpacken, rack-Befestigung und Ihr Gerät verbinden.

- [Entpacken und rack-Mount die 8100-Kabel](storsimple-8100-hardware-installation.md)
- [Entpacken und rack-Mount die 8600-Kabel](storsimple-8600-hardware-installation.md)

Sie benötigen Administratorrechte, um die Einrichtung und Konfiguration abzuschließen. Wir empfehlen die Konfigurationsprüfliste überprüfen, bevor Sie beginnen. Die Bereitstellung und Konfiguration kann einige Zeit dauern.

> [AZURE.NOTE] Bereitstellung des StorSimple Informationen auf der Website Microsoft Azure gilt für StorSimple-8000-Serie Geräte. Ausführliche Informationen zu Geräten 7000-Serie finden Sie unter: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). 7000-Serie Informationen zur Bereitstellung finden Sie im [StorSimple System Quick Start Guide](http://onlinehelp.storsimple.com/111_Appliance/).

## <a name="deployment-steps"></a>Bereitstellungsschritte

So benötigt Ihr StorSimple Gerät konfigurieren und Ihre StorSimple Manager-Dienst an. Neben den erforderlichen Schritte werden optionale Schritte und Verfahren, die Sie während der Bereitstellung abgeschlossen Schrittweise bereitstellungsanweisungen anzugeben, wenn diese optionale Schritte durchführt.


| Schritt                                                                                   | Beschreibung                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ERFORDERLICHE KOMPONENTEN**                                                                      | Vorbereitung für die bevorstehende Bereitstellung abgeschlossen werden müssen.                                                                                        |
|[Checkliste für die Bereitstellung](#deployment-configuration-checklist)                                                     | Verwenden Sie diese Prüfliste zu sammeln Informationen vor und während der Bereitstellung.                                                                       |
| [Bereitstellung erforderlicher Komponenten](#deployment-prerequsites)                                                               | Diese überprüfen, ob die Umgebung für die Bereitstellung bereit.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **SCHRITTWEISE BEREITSTELLUNG**                                                                   | Diese Schritte sind erforderlich, das StorSimple-Gerät in der Produktion bereitstellen.                                                                                      |
| [Schritt 1: Erstellen eines neuen Dienstes](#step-1-create-a-new-service)                                                         | Cloud-Management und Speicher für Ihr StorSimple-Gerät einrichten. *Überspringen Sie diesen Schritt haben Sie einen vorhandenen Dienst für andere Geräte StorSimple*.              |
| [Schritt 2: Abrufen der Dienstregistrierungsschlüssel](#step-2-get-the-service-registration-key)                                               | Taste registrieren und Ihr Gerät StorSimple-Verwaltungsdienst.                                                                         |
| [Schritt 3: Konfigurieren Sie und registrieren Sie das Gerät über Windows PowerShell für StorSimple](#step 3-configure-and-register-the-device-through-windows-powershell-for-storsimple) | Verbinden Sie das Gerät mit dem Netzwerk und Azure für die Installation mit dem Verwaltungsdienst registrieren.                                            |
| [Schritt 4: Abschließen der minimale Geräteinstallation](#step-4-complete-the-minimum-device-setup) </br>Optional: Aktualisieren Sie Ihr StorSimple Gerät. | Verwenden Sie den Verwaltungsdienst der Geräteinstallation und Speicher zu aktivieren.                                                                      |
| [Schritt 5: Erstellen eines Volume-Containers](#step-5-create-a-volume-container)                                                      | Erstellen Sie einen Container auf Bereitstellung. Ein volumecontainer besitzt Speicherkonto, Bandbreite und Verschlüsselung für alle darin enthaltenen Volumes.    |
| [Schritt 6: Erstellen eines Volumes](#step-6-create-a-volume)                                                               | Bereitstellen Sie Speicher-Volumes auf dem Gerät StorSimple für Server.                                                                                        |
| [Schritt 7: Bereitstellen, initialisieren und Formatieren eines Datenträgers](#step-7-mount-initialize-and-format-a-volume) </br>Optional: Konfigurieren Sie MPIO.            | Schließen Sie die Server, die iSCSI-Speicher des Geräts. Konfigurieren Sie optional MPIO um sicherzustellen, dass Ihre Server Link, Netzwerk und Schnittstellenfehler tolerieren können.                                                                                                                                                              |
| [Schritt 8: Erstellen Sie eine Sicherungskopie](#step-8-take-a-backup)                                                                  | Richten Sie Ihre backup-Richtlinie zum Schutz Ihrer Daten                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **ANDERE VERFAHREN**                                                                   | Sie müssen Verfahren finden Sie die Lösung bereitstellen.                                                                                    |
| [Konfigurieren eines neuen Storage-Kontos für den Dienst](#configure-a-new-storage-account-for-the-service)                                      |                                                                                                                                                               |
| [Verbindung mit Gerät serielle Konsole verwenden Sie kitten](#use-putty-to-connect-to-the-device-serial-console)                                    |                                                                                                                                                               |
| [Suchen und installieren](#scan-for-and-apply-updates)                                                   |                                                                                                                                                               |
| [Abrufen der IQN des Windows Server-Hosts](#get-the-iqn-of-a-windows-server-host)                                                   |                                                                                                                                                               |
| [Erstellen einer manuellen Sicherung](#create-a-manual-backup)                                                                 |
| [MPIO konfigurieren](#configure-mpio)                                                                          |

## <a name="deployment-configuration-checklist"></a>Checkliste für die Bereitstellung

Bevor Sie Ihr StorSimple Gerät bereitstellen, müssen Sie Informationen zum Konfigurieren der Software auf Ihrem Gerät. Einige dieser Informationen rechtzeitig vorbereiten hilft bei den Prozess der Bereitstellung StorSimple Gerät in Ihrer Umgebung optimieren. Downloaden Sie und verwenden Sie diese Prüfliste, ausführliche Konfigurationsinformationen wie das Gerät bereitstellen.  

[Checkliste für die Bereitstellung StorSimple herunterladen](http://www.microsoft.com/download/details.aspx?id=49159)  

## <a name="deployment-prerequisites"></a>Bereitstellung erforderlicher Komponenten

Die folgenden Abschnitte erläutern die Konfiguration erforderlichen Komponenten für Ihr StorSimple-Manager-Dienst und dem Gerät StorSimple.

### <a name="for-the-storsimple-manager-service"></a>Für den StorSimple-Manager-Dienst

Bevor Sie beginnen, stellen Sie sicher, dass:

- Sie haben Ihr Microsoft-Konto mit Anmeldeinformationen.

- Sie haben die Microsoft Azure Storage mit Anmeldeinformationen.

- Microsoft Azure-Abonnement ist für StorSimple Manager-Dienst aktiviert. Durch die [Konzernvertrag](https://azure.microsoft.com/pricing/enterprise-agreement/)sollten Ihr Abonnement erworben werden.

- Sie haben Zugriff auf terminal-Emulationssoftware wie kitten.

### <a name="for-the-device-in-the-datacenter"></a>Für das Gerät im Rechenzentrum

Sicherstellen Sie vor der Konfiguration des Geräts, dass:

- Das Gerät ist vollständig entpackt auf einem Gestell montiert und vollständig verkabelte für Stromversorgung, Netzwerkzugriff und seriellen Zugriff unter:

    -  [Entpacken und rack-Mount Kabel Geräts 8100](storsimple-8100-hardware-installation.md)
    -  [Entpacken und rack-Mount Geräts 8600-Kabel](storsimple-8600-hardware-installation.md)


### <a name="for-the-network-in-the-datacenter"></a>Für das Netzwerk im Rechenzentrum

Bevor Sie beginnen, stellen Sie sicher, dass:

- Die Ports in der Firewall Datacenter sind für iSCSI und cloud-Datenverkehr unter [Netzwerk an Ihr Gerät StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)geöffnet.

## <a name="step-by-step-deployment"></a>Schrittweise Bereitstellung

Verwenden Sie die folgenden Schritte Ihr StorSimple Gerät im Rechenzentrum bereitstellen.

## <a name="step-1-create-a-new-service"></a>Schritt 1: Erstellen eines neuen Dienstes

StorSimple Manager-Dienst kann mehrere StorSimple Geräte verwalten. Führen Sie die folgenden Schritte aus, um eine neue Instanz der StorSimple-Manager-Dienst zu erstellen.

[AZURE.INCLUDE [storsimple-create-new-service-gov](../../includes/storsimple-create-new-service-gov.md)]

> [AZURE.IMPORTANT] Wenn die automatische Erstellung von ein Speicherkonto-Dienst nicht aktiviert haben, müssen Sie mindestens ein Speicherkonto erstellen, nachdem Sie einen Dienst erstellt haben. Dieses Speicherkonto verwendet bei der Erstellung von volumecontainer.
>
> * Ein Speicherkonto nicht automatisch erstellt haben, finden Sie in [Konfigurieren einer neuen Storage-Konto für den Dienst](#configure-a-new-storage-account-for-the-service) Informationen.
> * Wenn Sie die automatische Erstellung von ein Speicherkonto aktiviert, gehen Sie zu [Schritt2: Abrufen der Dienstregistrierungsschlüssel](#step-2-get-the-service-registration-key).

## <a name="step-2-get-the-service-registration-key"></a>Schritt 2: Abrufen der Dienstregistrierungsschlüssel

Nach der StorSimple Manager-Dienst ausgeführt wird, müssen Sie den Dienst-Registrierungsschlüssel abrufen. Dieser Schlüssel zum Registrieren und verbinden Sie das Gerät StorSimple an den Dienst.

Die folgenden Schritte in das Portal der Behörde.

[AZURE.INCLUDE [storsimple-get-service-registration-key-gov](../../includes/storsimple-get-service-registration-key-gov.md)]


## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Schritt 3: Konfigurieren Sie und registrieren Sie das Gerät über Windows PowerShell für StorSimple

Verwenden Sie Windows PowerShell für StorSimple die erste Einrichtung des Geräts StorSimple abgeschlossen, wie im folgenden erläutert. Sie müssen dieses Schrittes mit terminal-Emulationssoftware. Weitere Informationen finden Sie unter [Kitten Verbindung zu Gerät serielle Konsole verwenden](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device-gov](../../includes/storsimple-configure-and-register-device-gov-u2.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Schritt 4: Schließen Sie minimale Geräteinstallation

Für die minimale Gerätekonfiguration des Geräts StorSimple müssen Sie:

- Sekundäre DNS-Server einrichten.
- ISCSI auf mindestens eine Netzwerkschnittstelle zu aktivieren.
- Beide Controller feste IP-Adressen zuweisen.

Die folgenden Schritte in das Portal der Behörde mindestens Geräte-Einrichtung abgeschlossen.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Schritt 5: Erstellen eines Volume-Containers

Ein volumecontainer besitzt Speicherkonto, Bandbreite und Verschlüsselung für alle darin enthaltenen Volumes. Sie müssen einen volumecontainer erstellen, bevor Sie Volumes auf dem StorSimple-Gerät bereitstellen können.

Die folgenden Schritte in das Portal der Behörde einen volumecontainer erstellen.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Schritt 6: Erstellen eines Volumes

Nach dem Erstellen eines Volume-Containers können Sie ein Speichervolume auf Gerät StorSimple für Server bereitstellen. Die folgenden Schritte in das Portal der Behörde ein Volume erstellen.

> [AZURE.IMPORTANT] Azure StorSimple kann nur Thin Provisioning Volumes erstellen.  Erstellen kann nicht vollständig bereitgestellt oder teilweise bereitgestellte Volumes auf einem System Azure StorSimple.

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Schritt 7: Bereitstellen, initialisieren und Formatieren eines Datenträgers

Führen Sie diese Schritte auf Ihrem Windows-Server.

> [AZURE.IMPORTANT]

> - Für eine hohe Verfügbarkeit der Lösung StorSimple empfiehlt Microsoft MPIO auf den Hostservern (optional) vor dem Konfigurieren von iSCSI-konfigurieren. MPIO-Konfiguration auf Host-Server wird sichergestellt, dass der Server einen Link, Netzwerk oder Schnittstellenfehler tolerieren können.

> - MPIO und iSCSI Installations- und Anleitung auf Windows Server wechseln Sie [MPIO für Ihr StorSimple-Gerät konfigurieren](storsimple-configure-mpio-windows-server.md). Dazu gehören auch die Schritte um, initialisieren und formatieren StorSimple-Volumes.

> - MPIO und iSCSI-Installation und Konfiguration von Informationen auf einem Linux-Server gehen Sie [MPIO für Ihren StorSimple Linux-Host konfigurieren](storsimple-configure-mpio-on-linux.md)

Wenn Sie nicht MPIO konfigurieren, folgendermaßen Sie bereitstellen, initialisieren und Formatieren der StorSimple-Volumes auf einem Windows-Server.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Schritt 8: Erstellen Sie eine Sicherungskopie

Backups Point-in-Time-Schutz von Volumes und Wiederherstellung bei gleichzeitiger Minimierung der Wiederherstellungszeiten verbessern. Übernehmen zwei Typen der Sicherung auf Ihrem Gerät StorSimple: lokale Snapshots und Cloud-Snapshots. Jeder dieser Sicherungsarten kann **geplant** oder **manuell**.

Die folgenden Schritte in das Portal der Behörde eine geplante Sicherung erstellen.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Sie können einen manuellen jederzeit Sicherung vornehmen. Prozeduren finden Sie unter [Erstellen einer manuellen Sicherung](#create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-the-service"></a>Konfigurieren eines neuen Storage-Kontos für den Dienst

Dies ist ein optionaler Schritt müssen Sie nur durchführen, wenn Dienst nicht die automatische Erstellung von Speicher-Konto aktiviert haben. Ein Speicherkonto Microsoft Azure muss einen StorSimple Volume-Container erstellen.

Benötigen Sie ein Konto Azure-Speicher in einen anderen Bereich erstellen, finden Sie unter [Über Azure-Speicherkonten](../storage/storage-create-storage-account.md) Anleitung.

Die folgenden Schritte im Portal Regierung auf **StorSimple-Manager-Dienst** .

[AZURE.INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]


## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Verbindung mit Gerät serielle Konsole verwenden Sie kitten

Um Windows PowerShell StorSimple herstellen, müssen Sie terminal-Emulationssoftware wie kitten verwenden. Kitten können Sie das Gerät direkt über die serielle Konsole oder Öffnen einer Telnet-Sitzung von einem Remotecomputer zugreifen.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Suchen und installieren

Aktualisieren Ihr Gerät kann mehrere Stunden dauern. Die folgenden Schritte für Scannen und Anwenden von Updates auf Ihrem Gerät.

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Ihr Gerät aktualisieren

1.  Klicken Sie auf der Seite Geräte für **Quick Start** auf **Geräte**. Wählen Sie das physische Gerät, klicken Sie auf **Wartung** und klicken Sie auf **Updates überprüfen**.  
2.  Auftrag, Suchen nach verfügbaren Updates wird erstellt. Wenn Updates verfügbar sind, ändert **Updates überprüfen** auf **Updates installieren**. Klicken Sie auf **Installieren**.
3.  Ein Auftrag wird erstellt. Überwachen Sie den Status der Aktualisierung von **Projekten**navigieren.

    > [AZURE.NOTE] Wenn die Aktualisierung gestartet wird, wird sofort den Status 50 Prozent. Der Status wechselt zu 100 Prozent, wenn die Aktualisierung abgeschlossen ist. Es ist kein Echtzeitstatus für die Aktualisierung.

4.  Nachdem das Gerät erfolgreich aktualisiert wurde, aktivieren Sie Daten 2 und Daten 3 Netzwerkschnittstellen, wenn diese deaktiviert wurden.

## <a name="get-the-iqn-of-a-windows-server-host"></a>Abrufen der IQN des Windows Server-Hosts

Führen Sie die folgenden Schritte iSCSI qualifizierten Namen (IQN) von einem Windows-Host, auf dem Windows Server® 2012 ausgeführt wird.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Erstellen einer manuellen Sicherung

Die folgenden Schritte in das Portal der Behörde auf dem StorSimple-Gerät eine manuelle Sicherung bei Bedarf für einzelne Volumes erstellen.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup-gov.md)]

## <a name="configure-mpio"></a>MPIO konfigurieren

Multipfad-e/a (MPIO) ist ein optionales Feature und ist nicht standardmäßig unter Windows Server. Er sollte als ein Feature über Server-Manager installiert werden. MPIO Installationshinweise finden Sie [MPIO für Ihr StorSimple-Gerät konfigurieren](storsimple-configure-mpio-windows-server.md).

MPIO Installationshinweise für StorSimple Geräte von einem Linux-Server gehen Sie [MPIO für Linux-Server konfigurieren](storsimple-configure-mpio-on-linux.md).

> [AZURE.NOTE] MPIO ist nicht auf einem virtuellen Gerät StorSimple in Azure unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren Sie ein [Virtuelles Gerät](storsimple-virtual-device-u2.md).

- Verwenden Sie den [StorSimple Manager-Dienst](https://msdn.microsoft.com/library/azure/dn772396.aspx) für das StorSimple Management.
