<properties 
   pageTitle="Adapter für SharePoint StorSimple | Microsoft Azure"
   description="Beschreibt das Installieren und konfigurieren oder entfernen Sie den Adapter StorSimple für SharePoint in eine SharePoint-Serverfarm."
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
   ms.workload="TBD"
   ms.date="07/11/2016"
   ms.author="v-sharos" />

# <a name="install-and-configure-the-storsimple-adapter-for-sharepoint"></a>Installieren Sie und konfigurieren Sie den Adapter StorSimple für SharePoint

## <a name="overview"></a>Übersicht

StorSimple Adapter für SharePoint ist eine Komponente, die Microsoft Azure StorSimple flexible Lagerung und Datenschutz SharePoint-Serverfarmen ermöglicht. Den Adapter können Binary Large Object (BLOB) Inhalte von SQL Server Inhaltsdatenbanken mit Microsoft Azure StorSimple Hybriden Cloud-Speichergerät verschieben.

StorSimple Adapter für SharePoint dient als Remote-Blob-Speicher (RSP) Anbieter und verwendet SQL Server Remote-Blob-Speicher-Funktion, um unstrukturierte SharePoint-Inhalte (in Form von BLOBs) speichern auf einem Dateiserver, der von einem StorSimple-Gerät unterstützt wird.

>[AZURE.NOTE] StorSimple Adapter für SharePoint unterstützt SharePoint Server 2010 Remote-Blob-Speicher (RSP). SharePoint Server 2010 externen BLOB-Speicher (EBS) wird nicht unterstützt.

- Download der StorSimple Adapter für SharePoint wechseln [StorSimple] Adapter für SharePoint[ 1] im Microsoft Download Center.

- Informationen zum Planen für RSP und RSP Grenzen Gehe zu [Entscheidung RSP in SharePoint 2013 mit] [ 2] oder [Planen für RSP (SharePoint Server 2010)][3].

Diese Übersicht beschreibt kurz die Rolle des StorSimple für SharePoint und die SharePoint Kapazität und Performance, die Sie beachten sollten, bevor Sie installieren und konfigurieren Sie den Adapter. Nach Überprüfung dieser Informationen gehen Sie [StorSimple Adapter für SharePoint-Installation](#storsimple-adapter-for-sharepoint-installation) Einrichten des Adapters zu beginnen.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>StorSimple Adapter für SharePoint-Vorteile

Inhalt wird in einer SharePoint-Website als unstrukturierte BLOB-Daten in eine oder mehrere Inhaltsdatenbanken gespeichert. Standardmäßig sind diese Datenbanken auf Computern gehostet, die SQL Server ausführen und in der SharePoint-Serverfarm. BLOBs können schnell, verbraucht viel lokalen Speicherplatz vergrößert. Aus diesem Grund möchten Sie ein anderes, kostengünstigere Lösung zu finden. SQL Server bietet eine Technologie namens Remote-Blob-Speicher (RSP-Code), die BLOB-Inhalt im Dateisystem außerhalb der SQL Server-Datenbank speichern. Mit RBS BLOBs in das Dateisystem auf dem Computer mit SQL Server befinden, oder in das Dateisystem auf einem anderen Server gespeichert werden.

RSP-Code müssen Sie RBS-Anbieter wie StorSimple Adapter für SharePoint RSP in SharePoint zu aktivieren. StorSimple Adapter für SharePoint arbeitet mit RBS BLOBs zu einem Server von Microsoft Azure StorSimple System gesichert verschieben lassen. Microsoft Azure StorSimple speichert dann die BLOB-Daten lokal oder in der Cloud, je nach Verwendung. BLOBs, die sehr aktiv sind (in der Regel als Ebene 1 oder hot Daten bezeichnet) lokal gespeichert. Weniger aktive Daten und archivierte Daten befinden sich in der Cloud. Nach einer Inhaltsdatenbank RSP aktivieren, wird jede neue BLOB-Inhalte in SharePoint auf dem StorSimple-Gerät und nicht in der Inhaltsdatenbank gespeichert.

Microsoft Azure StorSimple Implementierung von RSP bietet folgende Vorteile:

- Von BLOB-Inhalte auf einen anderen Server verschieben reduzieren Sie die Abfrage Belastung SQL Server SQL Server Reaktionsfähigkeit verbessern können. 

- Azure StorSimple verwendet Deduplizierung und Komprimierung Datengröße reduzieren.

- Azure StorSimple bietet Datenschutz die lokalen und Cloud-Snapshots. Außerdem setzen Sie die Datenbank auf dem Gerät StorSimple können Sie die Inhaltsdatenbank und BLOBs zusammen Absturz einheitlich sichern. (Verschieben der Datenbank auf dem Gerät ist nur für das Gerät StorSimple 8000-Serie unterstützt. Dieses Feature ist nicht für die Serie 5000 oder 7000 unterstützt.)

- Azure StorSimple bietet Disaster Recovery einschließlich Failover, Datei- und Volume-Recovery (einschließlich Test Recovery) und schnelle Wiederherstellung von Daten.

- Daten-Recovery-Software wie Kroll Ontrack PowerControls können StorSimple Snapshots von BLOB-Daten auf SharePoint-Inhalt wiederherstellen. (Diese Software Recovery ist eine separate Bestellung.)

- StorSimple Adapter für SharePoint wird in SharePoint Central Administration Portal zu Ihren gesamten SharePoint-Lösung von einem zentralen Ort verwalten.

Verschieben von BLOB-Inhalt auf bieten andere Kosten und Vorteile. Beispielsweise mit RBS reduzieren den Bedarf an teuren Tier 1-Speicher und, da es die Inhaltsdatenbank verkleinert, RSP können weniger Datenbanken in SharePoint-Serverfarm erforderlich. Andere Faktoren, Grenzwerte für die Größe und den Inhalt nicht RSP können auch allerdings Speicherbedarf. Weitere Informationen über die Kosten und Vorteile von RSP finden Sie unter [Planen für RSP (SharePoint Foundation 2010)] [ 4] und [Entscheidung RSP in SharePoint 2013 mit][5].

### <a name="capacity-and-performance-limits"></a>Kapazität und Performance-Grenzwerte

Bevor Sie, RSP in der SharePoint-Lösung erwägen, sollten Sie berücksichtigen die getestete und Kapazitätsgrenzen SharePoint Server 2010 und SharePoint Server 2013 und akzeptable Leistung wie diese Grenzwerte beziehen. Weitere Informationen finden Sie unter [Software und Grenzwerte für SharePoint 2013](https://technet.microsoft.com/library/cc262787.aspx).

Prüfen Sie Folgendes, bevor Sie RSP konfigurieren:

- Stellen Sie sicher, dass die Gesamtgröße des Inhalts (die Größe einer Inhaltsdatenbank) plus der Größe der zugeordneten gelegten BLOBs nicht das RSP-Größenlimit von SharePoint unterstützt. 200 GB beträgt. 

    **Measure-Inhaltsdatenbank und BLOB-Größe**

     1. Abfrage in der zentralen Administration WFE. Starten Sie die SharePoint-Verwaltungsshell, und geben Sie den folgenden Windows PowerShell-Befehl zum Abrufen der Größe der Inhaltsdatenbanken:

        `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`

         Dadurch wird die Größe der Inhaltsdatenbank auf dem Datenträger.

     2. Führen Sie einen der folgenden SQL-Abfragen in SQL Management Studio auf dem SQL Server für jede Inhaltsdatenbank und Hinzufügen zu Nummer erhalten in Schritt 1.

        Geben Sie auf SharePoint 2013 Inhaltsdatenbanken:

        `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`

        Geben Sie auf SharePoint 2010 Inhaltsdatenbanken:

        `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`

        Dadurch wird die Größe des BLOBs, die ausgelagert wurden.

- Wir empfehlen alle BLOB und Datenbank Inhalte lokal auf dem Gerät StorSimple speichern. Das StorSimple-Gerät ist ein Cluster mit zwei Knoten für hohe Verfügbarkeit. Platzieren der Inhaltsdatenbanken und BLOBs auf dem StorSimple-Gerät bietet hohe Verfügbarkeit.

    Verwenden Sie herkömmliche SQL Server best Practices bei der Migration zu der Inhaltsdatenbank auf dem StorSimple-Gerät. Verschieben Sie die Datenbank erst alle BLOB-Inhalt aus der Datenbank auf die Dateifreigabe über RSP verschoben wurde. Die Inhaltsdatenbank mit dem StorSimple-Gerät verschieben möchten, sollten den Inhaltsdatenbank Speicher auf dem Gerät als primäre Volume konfigurieren.

- In Microsoft Azure StorSimple, mehrstufige Volumes verwenden besteht keine Möglichkeit, Inhalte lokal auf Gerät nicht zu Microsoft Azure-Cloud-Speicher gestuft werden StorSimple sicherzustellen. Daher empfehlen wir StorSimple lokal fixiert Volumes zusammen mit SharePoint RBS. Dadurch wird sichergestellt, dass alle BLOB-Inhalt lokal auf dem Gerät StorSimple wird und nicht nach Microsoft Azure.

- Verwenden Sie nicht die Inhaltsdatenbanken auf dem StorSimple-Gerät speichern, herkömmlichen SQL Server hohe Verfügbarkeit best Practices, die RSP unterstützen. SQL Server-clustering RSP unterstützt, während SQL Server Spiegelung nicht. 

>[AZURE.WARNING] Wenn RSP nicht aktiviert haben, empfehlen wir nicht, Verschieben der Datenbank auf Gerät StorSimple. Nicht getestete Konfiguration ist.
 
## <a name="storsimple-adapter-for-sharepoint-installation"></a>StorSimple Adapter für SharePoint-installation

Vor der Installation von StorSimple-Adapter für SharePoint müssen Sie StorSimple-Gerät konfigurieren und stellen Sie sicher, dass die SharePoint-Serverfarm und SQL Server Instanziierung alle Komponenten. In diesem Lernprogramm beschreibt konfigurationsanforderungen sowie Verfahren zum Installieren und Aktualisieren der StorSimple Adapter für SharePoint. 

## <a name="configure-prerequisites"></a>Komponenten konfigurieren

Vor der Installation von StorSimple-Adapter für SharePoint unbedingt StorSimple Gerät, SharePoint-Serverfarm und Instanziierung SQL Server die folgenden Vorkenntnisse mitbringen.

### <a name="system-requirements"></a>Das System

StorSimple Adapter für SharePoint arbeitet mit folgenden Hard- und Software:

- Betriebssystem – Windows Server 2008 R2 SP1, Windows Server 2012 und Windows Server 2012 R2 

- Unterstützte SharePoint-Versionen – SharePoint Server 2010 oder SharePoint Server 2013

- Unterstützte SQL Server-Versionen – SQL Server 2008 Enterprise Edition, SQL Server 2008 R2 Enterprise Edition oder Enterprise Edition von SQL Server 2012

- Unterstützte Geräte StorSimple-StorSimple-8000-Serie, StorSimple 7000-Serie oder StorSimple 5000-Serie.

### <a name="storsimple-device-configuration-prerequisites"></a>StorSimple Gerät Konfiguration erforderliche Komponenten

StorSimple-Gerät ist ein Block-Gerät und erfordert einen Dateiserver, auf dem die Daten gehostet werden können. Wir empfehlen, einen separaten Server anstelle eines vorhandenen Servers aus der SharePoint-Farm zu verwenden. Dieser File Server muss auf demselben lokalen Netzwerk (LAN) wie der SQL Server-Computer sein, die Inhaltsdatenbanken hostet. 

>[AZURE.TIP] 
>
>- Wenn Sie die SharePoint-Farm für hohe Verfügbarkeit konfigurieren, sollten Sie auch den Dateiserver für hohe Verfügbarkeit bereitstellen.
>
>- Verwenden Sie nicht die Inhaltsdatenbank auf dem StorSimple-Gerät speichern, traditionell hohe Verfügbarkeit best Practices, die RSP unterstützen. SQL Server-clustering RSP unterstützt, während SQL Server Spiegelung nicht. 

Stellen Sie sicher, dass Ihr StorSimple-Gerät ordnungsgemäß konfiguriert ist und geeignete Mengen Unterstützung die SharePoint-Bereitstellung konfiguriert sind und über den SQL Server-Computer. Bereitstellen [der lokalen StorSimple Gerät](storsimple-deployment-walkthrough.md) wechseln Sie, wenn noch nicht bereitgestellt und konfiguriert das Gerät StorSimple. Beachten Sie die IP-Adresse des Geräts StorSimple. Sie benötigen diese während der StorSimple Adapter für SharePoint-Installation. 

Darüber hinaus sicher, dass das Volume für BLOB-Auslagerung der Anforderungen:

- Das Volume muss mit einem 64 KB Größe der Zuordnungseinheiten formatiert.

- Die Web-front-End (WFE) und Anwendungsserver müssen der Universal Naming Convention (UNC) Pfad zugreifen können. 

- SharePoint-Serverfarm muss auf den Datenträger schreiben konfiguriert werden.

>[AZURE.NOTE] Nach dem Installieren und konfigurieren Sie den Adapter muss alle BLOB Auslagerung durch das StorSimple-Gerät wechseln (das Gerät stellen die Volumes auf SQL Server und Speicher-Tiers verwalten). Sie können keine andere Ziele für BLOB Auslagerung verwenden.
 
Wenn Sie StorSimple Snapshot-Manager verwenden, um Snapshots der Daten-BLOB und Datenbank erstellen möchten, müssen Sie StorSimple Snapshot-Manager auf dem Datenbankserver installieren, damit es den SQL Writer-Dienst verwenden, um Windows Volume Shadow Copy Service (VSS) implementieren. 

>[AZURE.IMPORTANT] StorSimple Snapshot-Manager unterstützt SharePoint VSS Writer und anwendungskonsistente Snapshots von SharePoint-Daten nicht möglich. In einem Szenario mit SharePoint bietet StorSimple Snapshot-Manager konsistente Backups. 
 
## <a name="sharepoint-farm-configuration-prerequisites"></a>SharePoint-Farm Konfiguration erforderliche Komponenten

Stellen Sie sicher, dass SharePoint-Serverfarm richtig, wie folgt konfiguriert ist:

- Überprüfen Sie SharePoint-Serverfarm in einem ordnungsgemäßen Zustand, und überprüfen Sie Folgendes: 

- Alle SharePoint WFE Anwendungsserver in der Farm registriert ausgeführt und können auf dem Server, auf dem Sie den Adapter StorSimple für SharePoint installieren, gepingt.

- SharePoint-Timerdienst (SPTimerV3 oder SPTimerV4) wird auf einzelnen WFE-Server und Application Server ausgeführt.

- SharePoint-Timerdienst und IIS-Anwendungspools unter dem SharePoint-Zentraladministrationswebsite läuft Administratorrechte. 

- Stellen Sie sicher, dass Internet Explorer Enhanced Sicherheitskontext (für IE) deaktiviert ist. Gehen Sie IE deaktivieren

    1. Schließen Sie alle Instanzen von Internet Explorer.

    2. Starten Sie den Server-Manager.

    3. Klicken Sie im linken Bereich auf **Lokalen Server**.

    4. Klicken Sie im rechten Bereich neben der **Verstärkten Sicherheitskonfiguration für Internet Explorer** **auf**.

    5. Klicken Sie unter **Administratoren** **aus**.

    6. Klicken Sie auf **OK**.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Remote-Blob-Speicher (RSP) Komponenten

Stellen Sie sicher, dass Sie eine unterstützte Version von SQL Server verwenden. Nur die folgenden Versionen werden unterstützt und RSP verwenden:

- SQL Server 2008 Enterprise Edition

- SQL Server 2008 R2 Enterprise Edition

- SQL Server 2012 Enterprise Edition

BLOBs können auf diese Volumes externalisiert werden, die SQL Server-StorSimple-Gerät anzeigt. Keine Ziele für BLOB-Auslagerung werden unterstützt.

Abschluss aller erforderlichen Konfigurationsschritte werden Sie [StorSimple Adapter für SharePoint installieren](#install-the-storsimple-adapter-for-sharepoint).

## <a name="install-the-storsimple-adapter-for-sharepoint"></a>Installieren Sie den Adapter StorSimple für SharePoint

Gehen Sie zum Installieren der StorSimple Adapter für SharePoint. Wenn Sie die Software installieren, finden Sie unter [Aktualisieren oder installieren Sie den Adapter StorSimple für SharePoint](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint). Für die Installation erforderliche Zeit hängt die Gesamtzahl der SharePoint-Datenbanken im SharePoint-Serverfarm.

[AZURE.INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]


## <a name="configure-rbs"></a>RSP konfigurieren

Nach der Installation der StorSimple-Adapter für SharePoint konfigurieren Sie RSP wie in der folgenden Prozedur beschrieben.

>[AZURE.TIP] Der StorSimple Adapter für SharePoint-Stecker auf der Seite SharePoint-Zentraladministration ermöglicht RSP aktiviert oder deaktiviert für jede Inhaltsdatenbank in der SharePoint-Farm. Aktivieren oder Deaktivieren von RSP für die Inhaltsdatenbank verursacht ein Zurücksetzen von IIS, die abhängig von Ihrer Farmkonfiguration vorübergehend die Verfügbarkeit von SharePoint Web front-End (WFE) stören können. (Faktoren wie ein Front-End-Lastenausgleich die aktuelle Server-Arbeitslast und usw. können beschränken oder diese Unterbrechung vermeiden). Um eine Unterbrechung der Benutzer schützen empfehlen wir aktivieren oder deaktivieren RSP nur während eines geplanten Wartungszeitfensters.

[AZURE.INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]
 

## <a name="configure-garbage-collection"></a>Garbagecollection konfigurieren

Beim Löschen von Objekten aus einer SharePoint-Website werden sie nicht automatisch von RSP Speicher-Volume gelöscht. Stattdessen eine asynchrone Hintergrund Wartungsprogramm löscht-verwaiste BLOBs aus dem Dateispeicher. Systemadministratoren können regelmäßig Ausführen des Prozesses planen oder starten sie Bedarf.

Dieses Wartungsprogramms (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe) ist automatisch auf allen SharePoint WFE-Server und Application Server installiert, wenn RSP aktivieren. Das Programm wird im folgenden Verzeichnis installiert: *Startlaufwerk*: \Programme\Microsoft SQL Remote-Blob-Speicher 10.50\Maintainer\

Informationen zum Konfigurieren und verwenden das Wartungsprogramm [Verwalten RSP in SharePoint Server 2013][8].

>[AZURE.IMPORTANT] RSP Maintainer Programm ist ressourcenintensiv. Planen Sie die Ausführung nur bei hellen Aktivität auf der SharePoint-Farm.

### <a name="delete-orphaned-blobs-immediately"></a>Verwaiste BLOBs sofort löschen

Benötigen Sie verwaiste BLOBs löschen, können Sie die folgenden Schritte. Beachten Sie, dass diese Schritte sind in SharePoint 2013-Umgebung mit folgender Vorgehensweise können:

- Der Namen der Datenbank ist WSS_Content.
- Der SQL Server-Name ist SHRPT13-SQL12\SHRPT13.
- Name der Web-Anwendung ist SharePoint-80.

[AZURE.INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]


## <a name="upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint"></a>Aktualisieren Sie oder installieren Sie den Adapter StorSimple für SharePoint

Gehen Sie zu SharePoint Server StorSimple Adapter für SharePoint neu zu installieren oder zu einfach aktualisieren oder installieren den Adapter in einer vorhandenen SharePoint-Serverfarm. 

>[AZURE.IMPORTANT] Lesen Sie die folgenden Informationen, bevor Sie Ihre SharePoint-Software bzw. Aktualisierung aktualisieren oder neu installieren StorSimple Adapter für SharePoint:
>
>- Alle zuvor in den externen Speicher über RSP verschobenen Dateien wird nicht die Neuinstallation abgeschlossen bis RSP-Funktion wieder aktiviert. Führen Sie um Beeinträchtigung für die Benutzer zu beschränken, Aktualisierung oder Neuinstallation während eines geplanten Wartungszeitfensters.
>
>- Die erforderliche Zeit für die Aktualisierung/Neuinstallation variieren je nach SharePoint-Datenbanken im SharePoint-Serverfarm insgesamt.
>
>- Nach Abschluss die Aktualisierung/Neuinstallation müssen Sie die Inhaltsdatenbanken RSP aktivieren. Weitere Informationen finden Sie unter [RSP konfigurieren](#configure-rbs) .
>
>- Wenn Sie RSP für eine SharePoint-Farm, die eine sehr große Anzahl von Datenbanken (mehr als 200) hat konfigurieren, kann die Seite **SharePoint-Zentraladministration** Timeout. In diesem Fall wird aktualisieren Sie die Seite. Dies wirkt sich nicht auf den Konfigurationsprozess.

[AZURE.INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]
 
## <a name="storsimple-adapter-for-sharepoint-removal"></a>StorSimple Adapter für SharePoint entfernen

Die folgenden Verfahren beschreiben die BLOBs auf die Inhaltsdatenbanken SQL Server zurück und StorSimple Adapter für SharePoint deinstallieren. 

>[AZURE.IMPORTANT] Sie müssen wechseln die BLOBs auf die Inhaltsdatenbanken bevor der Adapter-Software deinstallieren. 

### <a name="before-you-begin"></a>Bevor Sie beginnen 

Zusammentragen Sie folgende Informationen, die Daten zurück auf die Inhaltsdatenbanken SQL Server und der Adapter entfernen beginnen:

- Die Namen aller Datenbanken für die RSP aktiviert ist
- Der Quellpfad konfigurierten BLOB-Speicher

### <a name="move-the-blobs-back-to-the-content-databases"></a>Die BLOBs zurück auf die Inhaltsdatenbanken

Bevor Sie StorSimple Adapter für SharePoint-Software deinstallieren, müssen Sie alle BLOBs, die ausgelagert wurden auf die Inhaltsdatenbanken SQL Server migrieren. Wenn Sie versuchen, StorSimple-Adapter für SharePoint deinstallieren, bevor Sie alle BLOBs auf die Inhaltsdatenbanken zurück, sehen Sie die folgende Warnung angezeigt.

![Warnung](./media/storsimple-adapter-for-sharepoint/sasp1.png)
 
####<a name="to-move-the-blobs-back-to-the-content-databases"></a>Die BLOBs auf die Inhaltsdatenbanken zurück 

1. Gelegten Objekte herunterladen

2. Öffnen Sie die Seite **SharePoint-Zentraladministration** und **Systemeinstellungen**durchsuchen. 

3. Klicken Sie unter **Azure StorSimple** **StorSimple-Adapter konfigurieren**.

4. Klicken Sie auf der Seite **StorSimple Adapter konfigurieren** **Deaktivieren** unterhalb der Inhaltsdatenbanken aus externen BLOB-Speicher entfernen möchten. 

5. Die Objekte von SharePoint, und uploaden sie Sie dann erneut.

Sie können auch die Microsoft` RBS Migrate()` PowerShell-Cmdlet mit SharePoint enthalten. Weitere Informationen finden Sie unter [Migrieren Content in oder aus RSP](https://technet.microsoft.com/library/ff628255.aspx).

Nachdem die BLOBs zurück zur Inhaltsdatenbank verschieben zum nächsten Schritt: [Deinstallieren des Adapters](#uninstall-the-adapter).

### <a name="uninstall-the-adapter"></a>Deinstallieren Sie den adapter

Nachdem die BLOBs zurück auf die Inhaltsdatenbanken SQL Server eine der folgenden Optionen StorSimple Adapter für SharePoint deinstallieren.

#### <a name="to-use-the-installation-program-to-uninstall-the-adapter"></a>Das Installationsprogramm verwendet den Adapter deinstallieren 

1. Verwenden Sie ein Konto mit Administratorrechten auf dem Front-End (WFE) Webserver anmelden.
2. Doppelklicken Sie auf StorSimple-Adapter für SharePoint-Installer. Der Setup-Assistent wird gestartet.

    ![Der Setup-Assistent](./media/storsimple-adapter-for-sharepoint/sasp2.png)

3. Klicken Sie auf **Weiter**. Die folgende Seite angezeigt.

    ![Entfernen Seite einrichten](./media/storsimple-adapter-for-sharepoint/sasp3.png)

4. Klicken Sie auf **Entfernen** , wählen Sie den Vorgang. Die folgende Seite angezeigt.

    ![Setup-Assistent-Bestätigungsseite](./media/storsimple-adapter-for-sharepoint/sasp4.png)

5. Klicken Sie auf **Entfernen** , um das Entfernen zu bestätigen. Die folgende Seite wird angezeigt.

    ![Setup-Assistent-Statusseite](./media/storsimple-adapter-for-sharepoint/sasp5.png)

6. Wenn die Installation abgeschlossen ist, wird die Seite Fertig stellen. Klicken Sie auf **Fertig stellen** , um den Setup-Assistenten zu schließen.

#### <a name="to-use-the-control-panel-to-uninstall-the-adapter"></a>Das deinstallieren den Adapter verwenden 

1. Die Systemsteuerungsoption, und klicken Sie dann auf **Programme und Funktionen**.

2. Wählen Sie **StorSimple-Adapter für SharePoint**und klicken Sie dann auf **Deinstallieren**. 

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über StorSimple](storsimple-overview.md).

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/en-us/library/ff943565.aspx
