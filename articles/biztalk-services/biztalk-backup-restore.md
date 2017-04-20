<properties 
    pageTitle="Erstellen und Wiederherstellen eine Sicherung in der BizTalk-Dienste | Microsoft Azure" 
    description="BizTalk-Dienste enthält Sicherung und Wiederherstellung. Informationen Sie zum Erstellen und Wiederherstellen einer Sicherung und festzustellen Sie, was gesichert werden. MAK, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>


# <a name="biztalk-services-backup-and-restore"></a>Der BizTalk-Dienste: Sicherung und Wiederherstellung

Azure BizTalk Services umfasst Funktionen für Backup und Wiederherstellung. Sichern und Wiederherstellen von klassischen Azure-Portal mit der BizTalk-Dienste beschrieben.

Sie können BizTalk-Dienste mithilfe der [REST-API von BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=325584)sichern. 

> [AZURE.NOTE] Hybrid-Anschlüsse sind nicht unabhängig von der Edition gesichert. Sie müssen Ihre Hybrid-Verbindung neu erstellen.

## <a name="before-you-begin"></a>Bevor Sie beginnen

- Sicherung und Wiederherstellung können für alle Editionen nicht verfügbar. Siehe [BizTalk-Dienste: Editionen Diagramm](biztalk-editions-feature-chart.md).

- Klassische Azure-Portal können Sie eine Sicherung auf Anforderung erstellen oder eine geplante Sicherung zu erstellen. 

- Backup-Content kann der gleichen BizTalk Service oder neue BizTalk Service wiederhergestellt werden. Zum Wiederherstellen der BizTalk Service mit demselben Namen der vorhandenen BizTalk Service müssen gelöscht werden, und der Name muss verfügbar sein. Nach dem Löschen einer BizTalk Service dauert es länger als für denselben Namen zu. Wenn Sie den gleichen Namen zu warten können, anschließend eine neue BizTalk-Service.

- BizTalk-Dienste können auf die gleiche Version oder eine höhere Edition wiederhergestellt werden. BizTalk-Dienste auf eine niedrigere Edition wiederherstellen, wird von wann die Sicherung durchgeführt wurde, nicht unterstützt.

    Beispielsweise kann eine Sicherung der Basic Edition, die Premium Edition wiederhergestellt werden. Eine Sicherung der Premium Edition kann nicht auf Standard Edition wiederhergestellt werden.

- EDI-Steuerelement Zahlen werden für die Kontinuität der Kontrolle Zahlen gesichert. Nachrichten nach der letzten Sicherung verarbeitet, kann das Wiederherstellen dieser Sicherung Inhalt doppelten Zahlen verursachen.

- Hat ein Stapel aktive Nachrichten verarbeiten Sie, die Stapel **vor** eine Sicherung ausführen. Beim Erstellen einer Sicherung (als erforderlich oder geplanten) werden Nachrichten in Batches nie gespeichert. 

    **Wenn eine mit aktiven Nachrichten Sicherung dieser Nachrichten werden nicht gesichert und sind daher verloren.**

- Optional: Im BizTalk-Portal, beenden Sie alle Verwaltungsvorgänge.


## <a name="create-a-backup"></a>Erstellen einer Sicherung

Eine Sicherung kann jederzeit durchgeführt werden und Sie vollständig kontrolliert. Dieser Abschnitt listet die Schritte zum Erstellen von Backups mithilfe der Azure-Verwaltungsportal einschließlich:

[Auf Anforderung sichern](#backupnow)

[Planen einer Sicherung](#backupschedule)

#### <a name="backupnow"></a>Auf Anforderung sichern
1. In der Azure-Verwaltungsportal **BizTalk-Dienste**aktivieren und wählen Sie den BizTalk-Service zu sichern.
2. Wählen Sie im **Dashboard** -Registerkarte am unteren Rand der Seite **Sichern** .
3. Geben Sie einen Sicherungsnamen ein. Geben Sie beispielsweise *MyBizTalkService*BU*Datum*.
4. Wählen Sie ein Speicherkonto Blob und das Prüfzeichen in die Sicherung zu starten.

Sobald die Sicherung abgeschlossen ist, wird ein Container mit der Sicherung eingegebene Name im Speicherkonto erstellt. Dieser Container enthält die Sicherungskonfiguration BizTalk Service.

#### <a name="backupschedule"></a>Planen einer Sicherung

1. Aktivieren Sie in der Azure-Verwaltungsportal **BizTalk-Dienste**, wählen Sie BizTalk Service die Sicherung planen möchten und wählen Sie die Registerkarte **Konfigurieren** .
2. Legen Sie den **Sicherungsstatus** **automatisch**. 
3. Wählen Sie das **Konto** die Sicherung speichern, geben die **Häufigkeit** der Backups erstellen und wie lange die Sicherungen (**Beibehaltung in Tagen**):

    ![][AutomaticBU]

    **Notizen**   
    - **Aufbewahrungstage**muss die Aufbewahrungsdauer backup Häufigkeit größer sein.
    - Wählen Sie **immer mindestens eine Sicherung**, selbst wenn die Aufbewahrungszeit ist.
    

4. **Wählen Sie aus.**


Beim Ausführen ein geplanter Sicherungsauftrag wird einen Container (Daten speichern) im Speicherkonto eingegebene erstellt. Der Name des Containers heißt *BizTalk Service Name-Datum-Uhrzeit*. 

Wenn das Dashboard BizTalk Service Status **Failed** zeigt:

![Status der letzten geplanten backup][BackupStatus] 

Der Link Öffnet das Management Services Protokolle um zu beheben. Siehe [BizTalk-Dienste: Fehlerbehebung Protokolle mit](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Wiederherstellen

Sie können Backups von klassischen Azure-Portal oder die [REST-API für BizTalk Service wiederherstellen](http://go.microsoft.com/fwlink/p/?LinkID=325582)wiederherstellen. Dieser Abschnitt enthält die Schritte zum Wiederherstellen mit dem Verwaltungsportal.

#### <a name="before-restoring-a-backup"></a>Vor dem Wiederherstellen einer Sicherung

- Neue Überwachung, Archivierung und Überwachung Speicher können beim Wiederherstellen einer BizTalk Service eingegeben werden.

- Gleichen Laufzeit EDI-Daten werden wiederhergestellt. Die EDI-Runtime Sicherung speichert Steuerelement Zahlen. Die wiederhergestellten Steuerelement Zahlen sind nacheinander ab dem Zeitpunkt der Sicherung. Nachrichten nach der letzten Sicherung verarbeitet, kann das Wiederherstellen dieser Sicherung Inhalt doppelten Zahlen verursachen.

#### <a name="restore-a-backup"></a>Wiederherstellen einer Sicherung

1. Wählen Sie in der Azure-Verwaltungsportal **neu** > **App Services** > **BizTalk-Dienst** > **Wiederherstellen**:

    ![Wiederherstellen einer Sicherung][Restore]

2. Wählen Sie im **Backup-URL**das Ordnersymbol und Azure Speicherkonto, das BizTalk Service Sicherungskopie gespeichert. Erweitern Sie den Container und im rechten die entsprechende Sicherung txt-Datei. 
<br/><br/>
Klicken Sie auf **Öffnen**.

3. Auf der Seite **BizTalk wiederherstellen** einen **BizTalk Service Name** und überprüfen Sie **URL-Domäne**, **Edition**und **Region** für den wiederhergestellten BizTalk Service. **Erstellen einer neuen SQL-Datenbankinstanz** für die Überwachungsdatenbank:

    ![][RestoreBizTalkService]

    Wählen Sie weiter.

4.  Überprüfen Sie den Namen der SQL-Datenbank, geben Sie den physischen Server, die SQL-Datenbank erstellt werden, und ein Benutzername/Kennwort für diesen Server.


    Wenn Sie die SQL Database Edition konfigurieren möchten, wählen Sie Größe und andere Eigenschaften **Konfigurieren erweiterter Datenbank**. 

    Wählen Sie weiter.

5. Ein neues Speicherkonto erstellen oder ein vorhandenes Speicherkonto für BizTalk Service eingeben.

7. Wählen Sie das Häkchen, die Wiederherstellung zu starten.

Sobald die Wiederherstellung erfolgreich abgeschlossen wurde, wird neue BizTalk Service angehalten auf der Seite BizTalk-Dienste im klassischen Azure-Portal aufgeführt.



### <a name="postrestore"></a>Nach dem Wiederherstellen einer Sicherung

BizTalk Service wird immer im Status " **angehalten** " wiederhergestellt. In diesem Zustand können Sie konfigurationsänderungen vor die neue Umgebung funktionsfähig, einschließlich:

- Erstellt BizTalk Service Applications Azure BizTalk-Dienste SDK verwenden müssen, Sie Anmeldeinformationen Access Control (ACS) in diesen Programmen mit der wiederhergestellten Umgebung zu aktualisieren.

- Wiederherstellen einer BizTalk Service um eine vorhandene BizTalk Service-Umgebung zu replizieren. In diesem Fall gibt es Abkommen im ursprünglichen BizTalk-Portal konfiguriert, mit dem FTP-Quellordner müssen Sie in der wiederhergestellten Umgebung eine andere FTP-Ordner aktualisieren. Andernfalls möglicherweise zwei verschiedene Verträge dieselbe Nachricht ziehen.

- Wenn Sie mehrere BizTalk Service Umgebungen wiederhergestellt, unbedingt die richtige Umgebung in Visual Studio Applikationen, PowerShell-Cmdlets, REST-APIs oder Trading Partner Management OM APIs abzielen.

- In diesem Zusammenhang empfiehlt es sich, automatisierte Backups auf die neu wiederhergestellte BizTalk Service-Umgebung konfigurieren.

Um BizTalk Service im klassischen Azure-Portal zu starten, wählen Sie wiederhergestellte BizTalk Service und **Resume** in der Taskleiste. 



## <a name="what-gets-backed-up"></a>Was gesichert

Wenn eine Sicherung erstellt wird, werden die folgenden Elemente gesichert:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Elemente gesichert</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure BizTalk Services-Portal</strong></td>
</tr> 
<tr>
<td>Konfiguration und Laufzeit</td> 
<td>
<ul>
<li>Partner und Profil-details</li>
<li>Partner-Agreements</li>
<li>Benutzerdefinierte Assemblys bereitgestellt</li>
<li>Brücken bereitgestellt</li>
<li>Zertifikate</li>
<li>Transformationen bereitgestellt</li>
<li>Rohrleitungen</li>
<li>Vorlagen erstellen und speichern im BizTalk-Portal</li>
<li>X12 ST01 und GS01-Zuordnung</li>
<li>Steuerelement Zahlen (EDI)</li>
<li>AS2 Nachricht MIC-Werte</li>
</ul>
</td>
</tr> 
 
<tr>
<td colspan="2">
 <strong>Azure BizTalk-Dienst</strong></td>
</tr> 
<tr>
<td>SSL-Zertifikat</td> 
<td>
<ul>
<li>SSL-Daten</li>
<li>Kennwort für SSL</li>
</ul>
</td>
</tr> 
<tr>
<td>Standardeinstellungen für BizTalk-Dienste</td> 
<td>
<ul>
<li>Anzahl Dezimalstellen</li>
<li>Edition</li>
<li>Produktversion</li>
<li>Region-Rechenzentrum</li>
<li>Access Control Service (ACS) Namespace und Schlüssel</li>
<li>Überwachung der Datenbank-Verbindungszeichenfolge</li>
<li>Archivierung Storage Konto Verbindungszeichenfolge</li>
<li>Überwachung von Speicher Konto Verbindungszeichenfolge</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Weitere Elemente</strong></td>
</tr> 
<tr>
<td>Überwachungsdatenbank</td> 
<td>Beim Erstellen der BizTalk Service werden die Überwachungsdatenbank Details einschließlich Azure SQL-Datenbankserver und der Überwachungsdatenbank eingegeben. Die Überwachungsdatenbank ist nicht automatisch gesichert.
<br/><br/>
<strong>Wichtig</strong><br/>
Die Überwachungsdatenbank gelöscht, und die Datenbank muss wiederhergestellt, muss eine Sicherungskopie vorhanden sein. Wenn eine Sicherung nicht vorhanden ist, können die Überwachungsdatenbank und die Daten nicht wiederhergestellt werden. Erstellen Sie in diesem Fall eine neue Überwachungsdatenbank mit dem gleichen Datenbanknamen. Geo-Replikation wird empfohlen.</td>
</tr> 
</table>

## <a name="next"></a>Weiter

Zum Erstellen von Azure BizTalk Services im klassischen Azure-Portal gehen Sie zu [BizTalk-Dienste: Bereitstellung mithilfe von Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/p/?LinkID=302280). Gehen Sie starten erstellen ASP.NET-Webanwendungen [Azure BizTalk-Dienste](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Siehe auch
- [Backup BizTalk-Dienst](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [BizTalk-Dienst von der Sicherung wiederherstellen](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [BizTalk-Dienste: Entwickler, Basic, Standard und Premium Editions Diagramm](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [BizTalk-Dienste: Bereitstellung mithilfe von Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [BizTalk-Dienste: Bereitstellung Statusdiagramm](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [Der BizTalk-Dienste: Registerkarten Dashboard, Monitor und skalieren](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [BizTalk-Dienste: Drosselung](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [BizTalk-Dienste: Ausstellername und Aussteller Schlüssel](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Wie kann ich starten mithilfe von Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png
 
