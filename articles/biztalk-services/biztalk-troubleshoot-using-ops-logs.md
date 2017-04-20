<properties 
    pageTitle="Problembehandlung bei Protokolle mithilfe der BizTalk-Dienste | Microsoft Azure" 
    description="Problembehandlung bei BizTalk Services Protokolle verwenden. MAK, WABS" 
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


# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk-Dienste: Problembehandlung bei der Verwendung der Protokolle

## <a name="what-are-the-operation-logs"></a>Was sind die Protokolle
Protokolle ist ein Management Services Feature klassischen Azure-Portal, das Verlaufsprotokolle Operationen auf der Azure-Dienste, einschließlich der BizTalk-Dienste angezeigt werden. So können Sie Daten zur Versionsgeschichte zu Verwaltungsvorgänge für Abonnements BizTalk Service bis 180 Tage anzeigen.

> [AZURE.NOTE]Diese Funktion zeichnet nur Protokolle für Verwaltungsvorgänge für BizTalk-Dienste, z. B. wenn der Dienst gestartet wurde, gesichert, und so weiter. Solche Operationen werden überwacht, unabhängig davon, ob sie von klassischen Azure-Portal oder über [BizTalk Service REST APIs](http://msdn.microsoft.com/library/azure/dn232347.aspx)ausgeführt. Eine vollständige Liste der Vorgänge mithilfe von Management Services finden Sie unter [Operations verfolgt mithilfe von Azure Management Services](#bizops).<br/><br/>
Diese erfasst nicht die Protokolle für Aktivitäten im Zusammenhang mit BizTalk Service-Laufzeit (z. B. verarbeitete Meldung durch Brücken usw..). Verwenden Sie zum Anzeigen dieser Protokolle Tracking Ansicht über das Portal BizTalk-Dienste. Weitere Informationen finden Sie unter [Nachrichten nachverfolgen](http://msdn.microsoft.com/library/azure/hh949805.aspx).

## <a name="view-biztalk-services-operation-logs"></a>BizTalk-Dienste Protokolle anzeigen
1. Wählen Sie in der Azure-Verwaltungsportal **Management Services**, und wählen Sie dann die Registerkarte **Protokolle** .
2. Sie können die Protokolle basierend auf den Parametern wie Abonnements, Datumsbereich, Diensttyp (z.B. BizTalk-Dienste), Dienstname oder Status des Vorgangs (erfolgreich, Fehler) filtern.
3. Wählen Sie die Markierung die gefilterte Liste angezeigt. Das folgende Bild zeigt Aktivitäten Testbiztalkservice:  ![Protokolle anzeigen][ViewLogs] 
4. Um mehr über eine bestimmte Operation anzuzeigen, wählen Sie die Zeile, und klicken Sie auf **Details** in der Taskleiste am unteren.


## <a name="bizops"></a>Operationen mit Azure Management Services nachverfolgt
Die folgende Tabelle enthält die Vorgänge, die mithilfe von Azure Management Services:

Name des Vorgangs | Aufgabe
--- | ---
CreateBizTalkService | Vorgang zum Erstellen einer neuen BizTalk Service
DeleteBizTalkService | Vorgang zum Löschen einer BizTalk Service
RestartBizTalkService | Operation BizTalk Service neu starten
StartBizTalkService | Vorgang zu BizTalk Service
StopBizTalkService | Vorgang zu BizTalk Service
DisableBizTalkService | Operation BizTalk Service deaktivieren
EnableBizTalkService | Operation BizTalk Service aktivieren
BackupBizTalkService | Vorgang sichern BizTalk Service
RestoreBizTalkService | BizTalk Service von angegebenen Sicherung Wiederherstellung
SuspendBizTalkService | Operation BizTalk Service anhalten
ResumeBizTalkService | Vorgang fortsetzen BizTalk Service
ScaleBizTalkService | Operation BizTalk Service nach oben oder unten skalieren
ConfigUpdateBizTalkService | Vorgang zum Aktualisieren der Konfigurations von BizTalk Service
ServiceUpdateBizTalkService | Operation upgrade oder Downgrade für BizTalk Service auf eine andere version
PurgeBackupBizTalkService | Operation Backups von BizTalk Service außerhalb der Beibehaltungsdauer löschen


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

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png
 
