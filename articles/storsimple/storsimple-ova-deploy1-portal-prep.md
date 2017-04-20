<properties
   pageTitle="Bereitstellen von StorSimple Virtual Array 1 - Portal zur Vorbereitung"
   description="Erste Lernprogramm StorSimple Bereitstellung virtuelles Array umfasst das Portal vorbereiten"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/24/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---prepare-the-portal"></a>Bereitstellen von StorSimple Virtual Array - Portal vorbereiten

![](./media/storsimple-ova-deploy1-portal-prep/getstarted4.png)

## <a name="overview"></a>Übersicht

Dieser Artikel bezieht sich auf Microsoft Azure StorSimple Virtual Array (auch bekannt als StorSimple lokalen virtuellen Gerät oder virtuelles Gerät StorSimple) ausgeführten März 2016 allgemein verfügbar (GA). Dies ist der erste Artikel in der Serie Bereitstellung Lernprogramme virtual Array vollständig als Dateiserver oder ein iSCSI-Server verwendet. Dieser Artikel beschreibt die Vorbereitung zum Erstellen und Konfigurieren der StorSimple Manager-Dienst vor der Bereitstellung eines virtuellen Arrays erforderlich. Dieser Artikel verknüpft auch eine Checkliste für die Bereitstellung sowie Konfiguration erforderlichen Komponenten.

Sie benötigen Administratorrechte, um die Einrichtung und Konfiguration abzuschließen. Wir empfehlen Konfigurationsprüfliste Bereitstellung überprüfen, bevor Sie beginnen. Portal Zubereitung dauert weniger als 10 Minuten.

Die Informationen in diesem Artikel gilt für die Bereitstellung von virtuellen StorSimple-Arrays in Azure-Verwaltungsportal als auch Microsoft Azure Government Cloud.

### <a name="get-started"></a>Erste Schritte

Der Bereitstellungsworkflow besteht aus Vorbereitung des Portals, virtuelles Array in einer virtualisierten Umgebung bereitgestellt und die Installation. Zunächst mit der Bereitstellung StorSimple Virtual Array als Dateiserver oder ein iSCSI-Server müssen Sie tabellarisch Folgendes (Artikel und Videos) beziehen.

#### <a name="deployment-articles"></a>Bereitstellung von Artikeln

Finden Sie in den folgenden Artikeln in der vorgeschriebenen Reihenfolge StorSimple Virtual Array bereitstellen.

| **#** | **In diesem Schritt**                          | **Dies erfolgt...**                                                         | **Verwenden Sie diese Dokumente.**|
|------|-------------------------------------------|--------------------------------------------------------------------------------|------------------------|
|1.   | **Klassischen Azure-Portal einrichten**       | Erstellen Sie und konfigurieren Sie der StorSimple Manager-Dienst vor der Bereitstellung eines virtuellen StorSimple-Geräts.  |[Vorbereiten des Portals](storsimple-ova-deploy1-portal-prep.md)|
|2.   | **Bestimmung des Virtual Array**           | Für Hyper-V bereitgestellt, und ein virtuelles StorSimple-Gerät auf einem Hostsystem mit Hyper-V in Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2 an. <br></br> <br></br> Für VMware bereitstellen und Verbinden mit einem StorSimple lokalen virtuellen Gerät auf einem Hostsystem mit VMware ESXi 5.5 und höher.<br></br>| [Virtuelles Array in Hyper-V-Bereitstellung](storsimple-ova-deploy2-provision-hyperv.md) <br></br> <br></br> [Bereitstellen eines virtuellen Arrays in VMware](storsimple-ova-deploy2-provision-vmware.md)|
|3.    | **Einrichten von Virtual Array**              | Der Dateiserver Erstinstallation durchführen Geräte-Einrichtung abgeschlossen und Ihr StorSimple Dateiserver registrieren. Sie können dann SMB-Freigaben bereitstellen. <br></br> <br></br> ISCSI-Server Erstinstallation ausführen die Geräteinstallation abzuschließen und den StorSimple iSCSI-Server registrieren. ISCSI-Volumes können dann bereitstellen.| [Virtuelles Array als Dateiserver einrichten](storsimple-ova-deploy3-fs-setup.md)<br></br> <br></br>[Virtuelles Array als iSCSI-Server einrichten](storsimple-ova-deploy3-iscsi-setup.md)|

#### <a name="deployment-videos"></a>Bereitstellung von videos

| **Dieser Schritt...** |  **Dieses Video.**|
|----------------|-------------|
| Eine schrittweise Anleitung zum Einstieg in die virtuelle StorSimple Array. | [Erste Schritte mit der virtuellen StorSimple](https://azure.microsoft.com/documentation/videos/get-started-with-the-storsimple-virtual-array/)|
| Schritt StorSimple virtuelles Array in Hyper-V-Bereitstellung|[Erstellen eines virtuellen StorSimple-Arrays](https://azure.microsoft.com/documentation/videos/create-a-storsimple-virtual-array/) |
|Anleitung zu Konfiguration und Registrierung von StorSimple virtuelles Array|[StorSimple virtuelles Array konfigurieren](https://azure.microsoft.com/documentation/videos/configure-a-storsimple-virtual-array/)|
|Eine schrittweise Anleitung zum Erstellen von Freigaben, Freigaben sichern und Wiederherstellen Daten StorSimple virtuelles Array als Dateiserver konfiguriert|[Verwenden des Virtual Arrays StorSimple](https://azure.microsoft.com/documentation/videos/use-the-storsimple-virtual-array/)|
|Anleitung für Failover und Disaster Recovery eines virtuellen StorSimple-Arrays|[StorSimple virtuelles Array Disaster Recovery](https://azure.microsoft.com/documentation/videos/storsimple-virtual-array-disaster-recovery/)

Sie können jetzt klassischen Azure-Portal einzurichten.

## <a name="configuration-checklist"></a>Checkliste für

Die Konfigurationsprüfliste beschreibt die Informationen sammeln, bevor Sie die Software auf Ihrem Gerät StorSimple konfigurieren. Diese Informationen rechtzeitig vorbereiten können vereinfachen das StorSimple-Gerät in Ihrer Umgebung bereitstellen. Je nachdem, ob das virtuelle Gerät StorSimple als Dateiserver oder ein iSCSI-Server bereitgestellt wird, benötigen Sie einen der folgenden Prüflisten.

-   [StorSimple virtuelles Array Configuration Prüfliste: Dateiserver](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf)herunterladen

-   [StorSimple Virtual Array iSCSI Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf)herunterladen

## <a name="prerequisites"></a>Erforderliche Komponenten

Hier finden Sie Konfiguration Voraussetzung für Ihren Dienst StorSimple Manager, virtual StorSimple-Gerät und Datacenter-Netzwerk.

### <a name="for-the-storsimple-manager-service"></a>Für den StorSimple-Manager-Dienst

Bevor Sie beginnen, stellen Sie sicher, dass:

-   Sie haben Ihr Microsoft-Konto mit Anmeldeinformationen.

-   Sie haben die Microsoft Azure Storage mit Anmeldeinformationen.

-   Ihr Abonnement für Microsoft Azure sollte für StorSimple Manager-Dienst aktiviert.

### <a name="for-the-storsimple-virtual-device"></a>Für das virtuelle Gerät StorSimple

Bevor Sie ein virtuelles Gerät bereitstellen, stellen Sie Folgendes sicher:

-   Sie haben Zugriff auf ein Hostsystem mit Hyper-V auf Windows Server 2008 R2 oder höher oder VMware (ESXi 5.5 oder höher) ist mit der Rückstellung ein.

-   Das Hostsystem ist Folgendes virtuelle Gerät bereitstellen widmen:

    -   Mindestens 4 Kernen.

    -   Mindestens 8 GB RAM.

    -   Eine Netzwerkschnittstelle.

    -   Ein 500 GB virtuellen Datenträger für Daten.

### <a name="for-the-datacenter-network"></a>Für Datacenter-Netzwerk

Bevor Sie beginnen, stellen Sie sicher, dass:

-   Das Netzwerk im Rechenzentrum ist nach Netzwerken an das StorSimple-Gerät konfiguriert. Weitere Informationen finden Sie unter [StorSimple Virtual Array System Requirements](storsimple-ova-system-requirements.md).

-   Virtuelle StorSimple-Gerät verfügt über eine dedizierte 5 Mbit/s Internetbandbreite (oder mehr) jederzeit zur Verfügung stehen. Diese Bandbreite sollte nicht mit einer anderen Anwendung freigegeben werden.

## <a name="step-by-step-preparation"></a>Schrittweise Vorbereitung

Verwenden Sie die folgenden Schritte StorSimple Manager Service Portal vorbereiten.

## <a name="step-1-create-a-new-service"></a>Schritt 1: Erstellen eines neuen Dienstes

Eine Instanz der StorSimple Manager-Dienst kann mehrere StorSimple 1200 Geräte verwalten. Führen Sie die folgenden Schritte aus, um eine neue Instanz der StorSimple-Manager-Dienst zu erstellen. Haben Sie eine vorhandene StorSimple-Verwaltungsdienst 1200-Geräte verwalten, überspringen Sie diesen Schritt und gehen Sie zu [Schritt 2: Abrufen der Dienstregistrierungsschlüssel](#step-2-get-the-service-registration-key).

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

> [AZURE.IMPORTANT]
>
> Wenn die automatische Erstellung von ein Speicherkonto-Dienst nicht aktiviert haben, müssen Sie mindestens ein Speicherkonto erstellen, nachdem Sie einen Dienst erstellt haben.
>

> - Ein Speicherkonto nicht automatisch erstellt haben, finden Sie in [Konfigurieren einer neuen Storage-Konto für den Dienst](#optional-step-configure-a-new-storage-account-for-the-service) Informationen.
>

> - Wenn Sie die automatische Erstellung von ein Speicherkonto aktiviert, gehen Sie zu [Schritt2: Abrufen der Dienstregistrierungsschlüssel](#step-2-get-the-service-registration-key).


## <a name="step-2-get-the-service-registration-key"></a>Schritt 2: Abrufen der Dienstregistrierungsschlüssel


Nach der StorSimple Manager-Dienst ausgeführt wird, müssen Sie den Dienst-Registrierungsschlüssel abrufen. Dieser Schlüssel zum Registrieren und Ihr StorSimple-Gerät mit dem Dienst herstellen.

Die folgenden Schritte im [klassischen Azure-Portal](https://manage.windowsazure.com/).


[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

> [AZURE.NOTE]
>
> Dienst-Registrierungsschlüssel wird die StorSimple Manager Geräte registrieren, die sich StorSimple Manager-Dienst verwendet.

## <a name="step-3-download-the-virtual-device-image"></a>Schritt 3: Virtuellen Geräts herunterladen

Nach Service-Registrierungsschlüssel verfügen, müssen Sie die virtuellen Geräts zum download ein virtuelles Gerät auf dem Hostsystem bereitstellen. Virtuelles Geräteabbilder sind bestimmte Betriebssystem und auf Schnellstart im klassischen Azure-Portal heruntergeladen werden.

> [AZURE.IMPORTANT] Die Software StorSimple Virtual Array kann nur in Verbindung mit der Storsimple Manager-Dienst verwendet werden.


Die folgenden Schritte im [klassischen Azure-Portal](https://manage.windowsazure.com/).

#### <a name="to-get-the-virtual-device-image"></a>Um das virtuelle Geräteabbild

1.  Klicken Sie auf **StorSimple-Manager-Dienst** auf den Dienst, den Sie erstellt haben. Dadurch gelangen Sie zur Seite **Quick Start** . (Das Schnellstart-Symbol klicken ![](./media/storsimple-ova-deploy1-portal-prep/image8.png) auf der Seite **Schnellstart** jederzeit.)

1.  Klicken Sie auf den Link zu dem Bild, das Sie aus dem Microsoft Download Center downloaden möchten. Die Bilddateien sind rund 4,8 GB.

    -   VHDX für Hyper-V in Windows Server 2012 und höher

    -   VHD für Hyper-V auf Windows Server 2008 R2 oder höher

    -   VMDK VMWare ESXi 5.5 und höher

2.  Downloaden Sie und extrahieren Sie die Datei auf ein lokales Laufwerk, eine Notiz, in dem die entpackte Datei befindet.

![Symbol: Video](./media/storsimple-ova-deploy1-portal-prep/video_icon.png) **Video verfügbar**

Das Video schrittweise Anleitung zum Einstieg in die virtuelle StorSimple Array.

> [AZURE.VIDEO get-started-with-the-storsimple-virtual-array]



## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>Optionaler Schritt: konfigurieren ein neuen Storage-Konto für den Dienst

Dies ist ein optionaler Schritt werden nur ausgeführt, wenn Dienst nicht die automatische Erstellung von Speicher-Konto aktiviert haben.

Benötigen Sie ein Konto Azure-Speicher in einen anderen Bereich erstellen, finden Sie unter [erstellen ein Speicherkonto](storage-create-storage-account.md#create-a-storage-account) schrittweise Anleitung.

Die folgenden Schritte im [klassischen Azure-Portal](https://manage.windowsazure.com/) auf der Seite StorSimple Manager ein vorhandenes Microsoft Azure Storage-Konto hinzufügen.

#### <a name="to-add-a-storage-account"></a>Ein Speicherkonto hinzufügen

1.  Wählen Sie auf der Zielseite StorSimple Manager-Dienst den Dienst, und doppelklicken Sie darauf. Dadurch gelangen Sie zur Seite **Quick Start** . Wählen Sie auf der Seite **Konfigurieren** .

2.  Klicken Sie auf **Konto hinzufügen oder bearbeiten**. Klicken Sie im Dialogfeld **Konto hinzufügen/bearbeiten** folgendermaßen Sie vor:

    1.  Klicken Sie auf **Hinzufügen**.

    1.  Geben Sie einen Namen für das Speicherkonto.

    1.  Bereitstellen Sie der primäre **Schlüssel** für das Speicherkonto Microsoft Azure.

    1.  Wählen Sie **SSL aktivieren Modus** einen sicheren Kanal für die Kommunikation zwischen dem Gerät und der Cloud erstellen. Deaktivieren Sie das Kontrollkästchen **SSL aktivieren** nur, wenn Sie in einer privaten Cloud arbeiten.

    1.  Klicken Sie auf Check ![](./media/storsimple-ova-deploy1-portal-prep/image7.png). Sie werden benachrichtigt, nachdem das Speicherkonto erfolgreich erstellt wurde.

        ![](./media/storsimple-ova-deploy1-portal-prep/image11.png)

1.  Das neu erstellte Speicherkonto wird auf der Seite **Konfigurieren** unter **Speicherkonten**angezeigt. Klicken Sie auf **Speichern** , um das neu erstellte Speicherkonto speichern. Klicken Sie auf **OK** , wenn Sie zur Bestätigung aufgefordert.


## <a name="next-step"></a>Nächstes

Im nächste Schritt wird eine virtuelle Maschine für das virtuelle Gerät StorSimple bereitstellen. Je nach dem Hostbetriebssystem finden Sie detaillierte in:

-   [StorSimple virtuelles Array in Hyper-V-Bereitstellung](storsimple-ova-deploy2-provision-hyperv.md)

-   [Bereitstellen eines virtuellen StorSimple-Arrays in VMware](storsimple-ova-deploy2-provision-vmware.md)
