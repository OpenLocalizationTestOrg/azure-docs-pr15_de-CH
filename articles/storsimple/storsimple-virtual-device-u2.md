<properties 
   pageTitle="Virtuelles Gerät StorSimple Update 2 | Microsoft Azure"
   description="Informationen Sie zum Erstellen, bereitstellen und Verwalten eines StorSimple virtuellen Geräts in einem virtuellen Microsoft Azure-Netzwerk. (Gilt für StorSimple Update 2)."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/23/2016"
   ms.author="alkohli" />

# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>Bereitstellen und Verwalten eines StorSimple virtuellen Geräts in Azure


##<a name="overview"></a>Übersicht
StorSimple-8000-Serie virtuelle Gerät ist eine zusätzliche Funktion, die die Microsoft Azure StorSimple Lösung. StorSimple virtuelle Gerät auf einem virtuellen Computer in einem virtuellen Netzwerk Microsoft Azure ausgeführt wird, und können sie und Ihre Hosts Daten kopieren. Dieses Lernprogramm beschreibt das Bereitstellen und Verwalten eines virtuellen Geräts in Azure und sind für alle virtuellen Geräte mit Softwareversion Update 2.


#### <a name="virtual-device-model-comparison"></a>Virtuelles Gerät Modellvergleich

StorSimple virtuelle Gerät steht in zwei Modelle, ein standard 8010 (ehemals 1100) und eine Premium 8020 (Update 2 eingeführt). Ein Vergleich der beiden Modelle ist tabellarisch.


| Modell          | 8010<sup>1</sup>                                                                     | 8020                                                                                                                               |
|-----------------------|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Maximale Kapazität**      | 30 TB                                                                     | 64 TB                                                                                                                                |
| **Azure VM**              | Standard_A3 (4 Kernen, 7 GB Speicher)                                                                      | Standard_DS3 (4 Kernen, 14 GB Speicher)                                                                                                                          |
| **Versionskompatibilität** | Aktualisieren mit Versionen vor 2 oder höher                                             | Versionen mit Update 2 oder höher                                                                                                  |
| **Region-Verfügbarkeit**   | Alle Azure-Regionen                                                         | Azure-Regionen, die Premium-Speicher unterstützen<br></br>Eine Liste der Regionen Siehe [Regionen 8020 unterstützt](#supported-regions-for-8020) |
| **Speichertyp**          | Verwendet standardmäßige Azure-Speicher für lokale Datenträger<br></br> Informationen zum [Erstellen einer Standard Speicherkonto]() | Verwendet Azure Premium-Speicher für lokale Datenträger<sup>2</sup> <br></br>Informationen zum [erstellen ein Speicherkonto Premium](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)                                                               |
| **Arbeitslast-Hinweise**     | Artikel auf Abruf von Dateien aus Sicherungskopien                                              | Cloud-Entwicklung und test Szenarien Latenz höhere Leistung Arbeitslasten <br></br>Sekundäres Gerät für Disaster recovery                                                                                            |
 
<sup>1</sup> *früher 1100*.

<sup>2</sup> *die 8010 und 8020 verwenden Azure Standardspeicher für Cloud-Ebene. Der Unterschied besteht nur in der lokalen Ebene im Gerät*.

#### <a name="supported-regions-for-8020"></a>Unterstützte Regionen 8020

Tabellarisch Premium Storage-Bereiche, die derzeit für 8020 unterstützt werden. Diese Liste wird Premium Speicher mehr verfügbar wird fortlaufend aktualisiert. 

| S. Nein.                                                  | Derzeit unterstützt Bereiche |
|---------------------------------------------------------|--------------------------------|
| 1                                                       | USA                     |
| 2                                                       |  Osten der USA                       |
| 3                                                       |  USA 2 OST                     |
| 4                                                       | Westen der USA                        |
| 5                                                       | Nordeuropa                   |
| 6                                                       | Westeuropa                    |
| 7                                                       | Südostasien                 |
| 8                                                       | Japan OST                     |
| 9                                                       | Japan West                     |
| 10                                                      | Australien OST                 |
| 11                                                      | Australien Südost *           |
| 12                                                      | Ostasien *                     |
| 13                                                      | Südlichen zentralen USA *              |

* Premium-Speicher wurde unlängst in diese Geos.

Dieser Artikel beschreibt die einzelnen Schritte beim Bereitstellen eines virtuellen Geräts StorSimple in Azure. Nach dem Lesen dieses Artikels werden Sie folgende Aufgaben ausführen:

- Verstehen Sie, wie das virtuelle Gerät das physische Gerät unterscheidet.

- Erstellen und konfigurieren Sie das virtuelle Gerät sein.

- Verbinden Sie mit virtuellen Gerät.

- Informationen Sie zum Arbeiten mit virtuellen Gerät.

In diesem Lernprogramm gilt für alle StorSimple virtuelle Geräte Update 2 und höher ausgeführt. 

## <a name="how-the-virtual-device-differs-from-the-physical-device"></a>Das physische Gerät wie das virtuelle Gerät unterscheidet

StorSimple virtuelle Gerät ist eine reine Software-Version auf einem einzelnen Knoten in einer Microsoft Azure Virtual Machine StorSimple. Virtuelle Gerät unterstützt Disaster Recovery-Szenarien mit das physische Gerät ist nicht verfügbar und eignet sich für die Verwendung auf Abrufen von Backups, lokalen Disaster Recovery und Cloud Entwicklungs- und Szenarien.

#### <a name="differences-from-the-physical-device"></a>Unterschiede zu physischen Gerät

In der folgenden Tabelle werden einige wichtige Unterschiede zwischen dem virtuellen Gerät StorSimple und StorSimple-Gerät

|                             | Gerät                                          | Virtuelles Gerät                                                                            |
|-----------------------------|----------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **Speicherort**                    | Befindet sich im Datencenter.                               | In Azure ausgeführt wird.                                                                            |
| **Netzwerk-Schnittstellen**          | Hat sechs Netzwerkschnittstellen: Daten 0 bis 5 Daten.                  | Verfügt über nur eine Schnittstelle: Daten 0.                                        |
| **Registrierung**                | Registriert bei der Konfiguration.                | Registrierung ist eine separate Aufgabe.                                                          |
| **Verschlüsselungsschlüssel für Dienstdaten** | Auf das physische Gerät Regenerieren Sie, und aktualisieren Sie das virtuelle Gerät mit dem neuen Schlüssel.           | Virtuelles Gerät kann nicht regeneriert werden. |


## <a name="prerequisites-for-the-virtual-device"></a>Voraussetzung für das virtuelle Gerät

Die folgenden Abschnitte erläutern die Konfiguration erforderlichen Komponenten für das virtuelle Gerät StorSimple. Überprüfen Sie vor der Bereitstellung eines virtuellen Geräts [Sicherheitsaspekte bei Verwendung eines virtuellen Geräts](storsimple-security.md#storsimple-virtual-device-security).

#### <a name="azure-requirements"></a>Azure Vorschriften

Bevor Sie virtuelle Gerät bereitstellen, müssen Sie folgenden vorbereitenden Azure-Umgebung:

- Für das virtuelle Gerät [ein virtuelles Netzwerk in Azure konfigurieren](../virtual-network/virtual-networks-create-vnet-classic-portal.md). Wenn Premium-Speicher, müssen Sie ein virtuelles Netzwerk in Azure-Region erstellen, die Premium-Speicher unterstützt. Weitere Informationen über [Bereiche, die derzeit für 8020 unterstützt werden](#supported-regions-for-8020).
- Es ist ratsam Standard DNS-Servers von Azure anstatt eigene DNS-Server-Namen. Wenn Ihr DNS-Server-Name ungültig ist oder ist der DNS-Server nicht korrekt Auflösen von IP-Adressen, die Erstellung des virtuellen Geräts schlägt fehl.
- Punkt-zu-Standort- und zwischen Standorten sind optional, aber nicht erforderlich. Wenn Sie möchten, können diese Optionen für erweiterte Szenarien konfigurieren. 
- [Azure Virtual Machines](../virtual-machines/virtual-machines-linux-about.md) (Hostserver) können im virtuellen Netzwerk, mit denen Volumes von virtuellen Gerät verfügbar gemacht werden können. Diese Server müssen folgenden Vorschriften erfüllen:                            
    - Windows oder Linux VMs mit iSCSI-Initiator-Software installiert sein.
    - Im gleichen virtuellen Netzwerk als virtuelles Gerät ausgeführt werden.
    - Werden Sie die iSCSI-Ziel des virtuellen Geräts durch die interne IP-Adresse des virtuellen Geräts herstellen.

- Stellen Sie sicher, dass Sie Unterstützung für iSCSI und Cloud im gleichen virtuellen Netzwerk konfiguriert haben.


#### <a name="storsimple-requirements"></a>StorSimple Vorschriften

Stellen Sie die folgenden Updates zu Ihrem Dienst Azure StorSimple vor der Erstellung eines virtuellen Geräts:


- Fügen Sie [Access Control-Datensätze](storsimple-manage-acrs.md) für die virtuellen Computer werden Hostserver für Ihr virtuelles Gerät hinzu

- Verwenden Sie ein [Speicherkonto](storsimple-manage-storage-accounts.md#add-a-storage-account) in derselben Region als virtuelles Gerät. Speicherkonten in unterschiedlichen Regionen können zu Leistungseinbußen führen. Ein Standard oder Premium Storage-Konto können Sie mit dem virtuellen Gerät. Weitere Informationen zum [standardspeicherkonto]() oder ein [Premium-Konto](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk) erstellen

- Verwenden Sie ein Konto verschiedene Speicher für die geräteerstellung von virtuellen aus für Ihre Daten. Gleiche Speicherkonto kann zu Leistungseinbußen führen.

Stellen Sie sicher, dass Folgendes vor:

- Azure klassischen Portal Kontos mit Anmeldeinformationen.

- Eine Kopie der Daten Verschlüsselungsschlüssel auf dem physischen Gerät.


## <a name="create-and-configure-the-virtual-device"></a>Erstellen Sie und konfigurieren Sie das virtuelle Gerät

Sicherstellen Sie bevor Sie diese Verfahren ausführen, dass die [erforderlichen Komponenten für das virtuelle Gerät](#prerequisites-for-the-virtual-device)erfüllt haben. 

Nachdem ein virtuelles Netzwerk erstellt, konfiguriert StorSimple Manager-Dienst und das physische Gerät StorSimple Dienst registriert haben, können Sie die folgenden Schritte zum Erstellen und Konfigurieren eines virtuellen Geräts StorSimple. 

### <a name="step-1-create-a-virtual-device"></a>Schritt 1: Erstellen eines virtuellen Geräts

Die folgenden Schritte StorSimple virtuelle Gerät erstellen.

[AZURE.INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Wenn die Erstellung des virtuellen Geräts in diesem Schritt fehlschlägt, müssen Sie nicht mit dem Internet. Weitere Informationen finden Sie unter [Internet-Verbindungsfehler beheben](#troubleshoot-internet-connectivity-errors) Wenn Vernetzung eines virtuellen Geräts.


### <a name="step-2-configure-and-register-the-virtual-device"></a>Schritt 2: Konfigurieren und virtuelle Gerät registrieren

Stellen Sie bevor Sie beginnen sicher, dass eine Kopie der Daten Verschlüsselungsschlüssel. Der Verschlüsselungsschlüssel Daten wurde das erste StorSimple-Gerät konfiguriert und erstellt wurden beauftragt, an einem sicheren Ort speichern. Wenn Sie eine Kopie der Daten Verschlüsselungsschlüssel nicht haben, müssen Sie Microsoft Support Unterstützung wenden.

Die folgenden Schritte konfigurieren und Registrieren des StorSimple virtuellen Geräts.
[AZURE.INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-the-device-configuration-settings"></a>Schritt 3: (Optional) ändern Device-Konfigurationen

Der folgende Abschnitt beschreibt Device-Konfigurationen benötigt für die StorSimple virtuelles Gerät möchten Sie verwenden CHAP, StorSimple Snapshot-Manager oder das Gerät Administratorkennwort ändern.

#### <a name="configure-the-chap-initiator"></a>Konfigurieren des CHAP-Initiators

Dieser Parameter enthält die Anmeldeinformationen, die Ihre virtuelle Gerät (Ziel) von Initiatoren (Server) erwartet, die auf den Datenträger zugreifen. Die Initiatoren bieten einen CHAP-Benutzernamen und CHAP-Kennwort zur Identifikation auf Ihrem Gerät während der Authentifizierung. Ausführliche Schritte finden Sie unter [CHAP für Ihr Gerät konfigurieren](storsimple-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-the-chap-target"></a>Konfigurieren des CHAP-Ziels

Dieser Parameter enthält die Anmeldeinformationen, die bei ein Initiator CHAP aktiviert gegenseitige oder bidirektionale Authentifizierung fordert Ihr virtuelle Gerät verwendet. Virtuelle Gerät verwendet eine Reverse CHAP-Benutzername Reverse CHAP-Kennwort und sich dabei Authentifizierung an den Initiator identifizieren. Beachten Sie, dass CHAP Target Globale Standardeinstellungen sind. Diese Anwendung werden alle Volumes mit virtuellen Speichergerät verbunden CHAP-Authentifizierung verwenden. Ausführliche Schritte finden Sie unter [CHAP für Ihr Gerät konfigurieren](storsimple-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-the-storsimple-snapshot-manager-password"></a>Das Kennwort StorSimple Snapshot-Manager konfigurieren

StorSimple Snapshot-Manager-Software befindet sich auf dem Windows-Server und Administratoren zum Verwalten des Geräts StorSimple in Form eines lokalen Backups und Snapshots.

>[AZURE.NOTE] Windows-Host ist für das virtuelle Gerät Azure VM.

Wenn Sie ein Gerät StorSimple Snapshot-Manager konfigurieren, werden Sie aufgefordert zu StorSimple Gerät IP-Adresse und ein Kennwort zur Authentifizierung des Speichergeräts. Ausführliche Schritte finden Sie unter [Kennwort StorSimple Snapshot-Manager konfigurieren](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

#### <a name="change-the-device-administrator-password"></a>Ändern des Administratorkennworts Gerät 

Wenn Sie die Windows PowerShell-Schnittstelle auf das virtuelle Gerät verwenden, müssen Sie ein Administratorkennwort Gerät eingeben. Für die Sicherheit Ihrer Daten müssen Sie dieses Kennwort ändern, bevor das virtuelle Gerät verwendet werden kann. Ausführliche Schritte finden Sie unter [Konfigurieren Gerät Administratorkennwort](storsimple-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-to-the-virtual-device"></a>Eine Remoteverbindung mit dem virtuellen Gerät
Remote-Zugriff auf das virtuelle Gerät über die Windows PowerShell-Schnittstelle ist nicht standardmäßig aktiviert. Sie möchten ermöglichen auf das virtuelle Gerät zuerst und dann auf dem Client verwendet wird, um virtuelle Gerät aktivieren.

Unten finden Sie die Schritten die Remoteverbindung.

### <a name="step-1-configure-remote-management"></a>Schritt 1: Konfigurieren der Remoteverwaltung

Die folgenden Schritte remote-Management für virtuelle StorSimple-Gerät konfigurieren.

[AZURE.INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-the-virtual-device"></a>Schritt 2: Remotezugriff auf das virtuelle Gerät

Nach der Aktivierung des remote-Management auf der Konfigurationsseite StorSimple Gerät können Windows PowerShell-Remoting Sie das virtuelle Gerät von einem anderen virtuellen Computer im gleichen virtuellen Netzwerk herstellen; Beispielsweise können Sie von der VM-Host verbinden, die Sie verbinden iSCSI und konfiguriert. In den meisten Installationen wird bereits öffnen einen öffentlichen Endpunkt um Ihren Host VM zugreifen, für den Zugriff auf das virtuelle Gerät.

>[AZURE.WARNING] **Aus Sicherheitsgründen wird dringend empfohlen, HTTPS verwenden, wenn die Endpunkte verbinden und löschen Sie die Endpunkte nach Abschluss der PowerShell-Remotesitzung.**

Führen Sie die Verfahren im [Herstellen einer Verbindung mit dem Gerät StorSimple Remote](storsimple-remote-connect.md) Remoting für Ihr virtuelles Gerät einrichten.

## <a name="connect-directly-to-the-virtual-device"></a>Virtuelles Gerät direkt Verbindung

Sie können auch direkt auf das virtuelle Gerät verbinden. Wenn Sie von einem anderen Computer außerhalb des virtuellen Netzwerks oder außerhalb der Microsoft Azure-Umgebung direkt auf das virtuelle Gerät verbinden möchten, müssen Sie zusätzliche Endpunkte erstellen, wie im folgenden beschrieben. 

Die folgenden Schritte öffentlichen Endpunkt auf das virtuelle Gerät erstellen.

[AZURE.INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

Wir empfehlen, da dies die Anzahl der öffentlichen Endpunkte im virtuellen Netzwerk minimiert von einem anderen virtuellen Computer im gleichen virtuellen Netzwerk verbunden. Wenn Sie diese Methode verwenden, schließen den virtuellen Computer über eine Remotedesktop-Sitzung und konfigurieren Sie diesen virtuellen Computer verwenden wie andere Windows-Client in einem lokalen Netzwerk. Sie müssen nicht die öffentliche Portnummer angefügt werden, weil der Port bereits bekannt ist.

## <a name="work-with-the-storsimple-virtual-device"></a>Arbeiten Sie mit virtuellen StorSimple-Gerät

Damit Sie erstellt und StorSimple virtuelle Gerät konfiguriert haben, können Sie damit arbeiten kann. Sie können mit volumecontainer, Volumes und backup-Policies auf einem virtuellen Gerät arbeiten wie auf einem physischen Gerät StorSimple. der einzige Unterschied ist, müssen Sie sicherstellen, dass das virtuelle Gerät aus der Liste auswählen. Finden Sie schrittweise Verfahren der verschiedenen Verwaltungsaufgaben für das virtuelle Gerät [StorSimple Manager-Dienst zum Verwalten eines virtuellen Geräts verwenden](storsimple-manager-service-administration.md) .

Den folgenden Abschnitten werden einige der Unterschiede beim Arbeiten mit virtuellen Gerät begegnen.

### <a name="maintain-a-storsimple-virtual-device"></a>Ein virtuelles Gerät StorSimple verwalten

Er ist eine reine Software-Gerät ist Wartung des virtuellen Geräts minimale Wartung für das physische Gerät gegenüber. Sie haben die folgenden Optionen:

- **Softwareupdates** – das Datum der letzten Aktualisierung die Software, zusammen mit allen Nachrichten aktualisieren. Können Sie die Schaltfläche **Scannen Updates** am unteren Rand der Seite einen manuellen Scan durchführen, wenn Sie nach neuen Updates suchen möchten. Wenn Updates verfügbar sind, klicken Sie auf Installation von **Updates installieren** . Da nur eine einzige Schnittstelle auf das virtuelle Gerät, dadurch werden geringfügige Unterbrechung Wenn Updates angewendet werden. Virtuelle Gerät Herunterfahren und neu starten (falls erforderlich) zum Anwenden von Updates, die veröffentlicht wurden. Eine schrittweise Anleitung finden Sie [Ihr Gerät aktualisieren](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
- **Paket** erstellen und Support-Paket um zu Problemen mit dem virtuellen Gerät Microsoft Support hochladen. Finden Sie eine schrittweise Anleitung zum [Erstellen und Verwalten einer Support-Paket](storsimple-create-manage-support-package.md).

### <a name="storage-accounts-for-a-virtual-device"></a>Speicherkonten für ein virtuelles Gerät

Speicherkonten werden für die Verwendung der StorSimple Manager-Dienst, das virtuelle Gerät und das physische Gerät erstellt. Erstellung Speicherkonten empfehlen wir die Verwendung eines Regionsbezeichner im Namen um sicherzustellen, dass der Bereich aller Systemkomponenten einheitlich ist. Ein virtuelles Gerät ist es wichtig, dass alle Komponenten in derselben Region um Leistungsprobleme zu vermeiden.

Eine schrittweise Anleitung finden Sie [ein Speicherkonto hinzufügen](storsimple-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-virtual-device"></a>Ein virtuelles Gerät StorSimple deaktivieren

Deaktivieren eines virtuellen Geräts löscht die VM und Ressourcen erstellt, wenn sie bereitgestellt wurde. Nachdem das virtuelle Gerät deaktiviert, nicht um den vorherigen Zustand wiederhergestellt. Bevor Sie das virtuelle Gerät deaktivieren, müssen Sie beenden oder Löschen von Clients und Servern, die davon abhängen.

Deaktivieren eines virtuellen Geräts führt die folgenden Aktionen:

- Virtuelle Gerät wird entfernt.

- Betriebssystem-Datenträger und Datenträger für das virtuelle Gerät erstellt werden entfernt.

- Gehosteter Dienst und während der Bereitstellung erstellte virtuelle Netzwerk bleiben. Wenn Sie sie nicht verwenden, sollten Sie sie manuell löschen.

- Cloud-Snapshots für das virtuelle Gerät erstellt werden beibehalten.

Eine schrittweise Anleitung finden Sie unter [deaktivieren und löschen Sie das Gerät StorSimple](storsimple-deactivate-and-delete-device.md).

Als virtuelle Gerät auf der Seite StorSimple Manager deaktiviert angezeigt wird, können Sie das virtuelle Gerät aus der Liste auf der Seite **Geräte** löschen.


### <a name="start-stop-and-restart-a-virtual-device"></a>Starten, Anhalten und Starten eines virtuellen Geräts
Im Gegensatz zu StorSimple-Gerät kommt kein Strom auf oder Schaltfläche auf ein StorSimple virtuelles Gerät ausschalten. Es kann jedoch Situationen Sie sollten beenden und Neustarten des virtuellen Geräts. Einige Updates erfordern beispielsweise die VM neu gestartet werden, um den Aktualisierungsprozess abzuschließen. Die einfachste Möglichkeit zum Starten, Anhalten und Neustarten eines virtuellen Geräts werden virtuelle Computer Management Console verwendet.

**Beim Betrachten der Verwaltungskonsole läuft der virtuellen Device-Status, da es standardmäßig gestartet wird, nachdem es erstellt wurde.** Sie können starten, beenden und starten einen virtuellen Computer zu einem beliebigen Zeitpunkt.

[AZURE.INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-to-factory-defaults"></a>Auf Standardwerte zurücksetzen

Sollten Sie nur mit dem virtuellen Gerät beginnen möchten, deaktivieren löschen Sie und erstellen Sie eine neue zu. Wie bei das physische Gerät zurückgesetzt wird ist Ihr neue virtuelle Gerät nicht installierten Updates. Stellen Sie daher sicher nach Updates vor.


## <a name="fail-over-to-the-virtual-device"></a>Virtuelles Gerät Failover

Disaster Recovery (DR) ist eine wichtigen Szenarien für StorSimple virtuelle Gerät entwickelt wurde. In diesem Szenario kann Gerät StorSimple oder gesamten Rechenzentrum nicht verfügbar. Glücklicherweise können Sie ein virtuelles Gerät Operationen an einem alternativen Speicherort wiederherstellen. Während DR volumecontainer vom Quellgerät Eigentümerschaft ändern und das virtuelle Gerät übertragen. Die erforderlichen Komponenten für Disaster Recovery sind das virtuelle Gerät wurde erstellt und konfiguriert alle Volumes in der volumecontainer wurden offline und volumecontainer ist ein Cloud-Snapshot.

>[AZURE.NOTE] 
>
> - Verwendung ein virtuelles Geräts als das sekundäre Gerät für Disaster Recovery, denken Sie daran, die 8010 30 TB Standardspeicher und 8020 64 TB Speicher Premium. Höhere Kapazität 8020 virtuelles Gerät möglicherweise eher für DR-Szenario.
> - Failover ist nicht möglich oder Klon von einem Gerät mit Update 2 auf ein Gerät 1 Software vor dem Update. Sie können jedoch nicht über ein Gerät mit Update 2 auf einem Gerät mit Update 1 (1.1 oder 1.2)

Eine schrittweise Anleitung finden Sie unter [Failover zu einem virtuellen Gerät](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device).
 

## <a name="shut-down-or-delete-the-virtual-device"></a>Herunterfahren oder virtuelle Gerät löschen

Wenn Sie zuvor konfiguriert und ein StorSimple virtuelles Gerät jetzt antizipierte berechnen Gebühren für die Verwendung beenden möchten, können Sie das virtuelle Gerät herunterfahren. Virtuelles Gerät Herunterfahren löschen nicht dessen Betriebssystem oder Datenträger im Speicher. Hält Ihres Abonnements anfallenden Gebühren, aber weiterhin Lagerkosten für Datenträger OS und Daten.

Löschen oder virtuelles Gerät heruntergefahren wird es auf Geräten der StorSimple Manager-Dienst als **Offline** angezeigt. Sie können deaktivieren oder löschen Sie das Gerät, wenn auch die Sicherungen virtuelles Gerät löschen möchten. Weitere Informationen finden Sie unter [deaktivieren und Löschen eines StorSimple-Geräts](storsimple-deactivate-and-delete-device.md).

[AZURE.INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[AZURE.INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

   
## <a name="troubleshoot-internet-connectivity-errors"></a>Fehlerbehebung bei der Internet-Konnektivität 

Während der Erstellung eines virtuellen Geräts Wenn keine mit dem Internet Verbindung fehl Schritt beim Erstellen des. Um zu beheben ist der Fehler wegen Internetkonnektivität folgendermaßen Sie im klassischen Azure-Portal:

1. Erstellen Sie Windows Server 2012 virtuellen Computer in Azure. Diese virtuellen Computer verwenden dieselbe Speicherkonto, VNet und Subnetz als virtuelles Gerät verwendet. Haben Sie bereits eine vorhandene Windows Server in Azure Speicherkonto, Vnet und Subnetz verwenden, können Sie auch die Internetkonnektivität zu beheben.
2. Remote anmelden den im vorherigen Schritt erstellten virtuellen Computer. 
3. Öffnen Sie ein Eingabeaufforderungsfenster auf dem virtuellen Computer (Win + R und dann `cmd`).
4. Führen Sie folgende Cmd aufgefordert werden.

    `nslookup windows.net`

5. Wenn `nslookup` fehlschlägt, Verbindungsfehler Internet verhindert, dass das virtuelle Gerät StorSimple Manager-Dienst registriert. 
6. Um sicherzustellen, dass das virtuelle Gerät Azure Websites wie "Windows" ist das virtuelle Netzwerk nehmen Sie erforderlichen Änderungen vor.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [StorSimple Manager-Dienst ein virtuelles Gerät verwalten](storsimple-manager-service-administration.md).
 
- Verstehen, wie [ein StorSimple-Volume aus einem Sicherungssatz](storsimple-restore-from-backup-set.md)wiederherstellen. 

