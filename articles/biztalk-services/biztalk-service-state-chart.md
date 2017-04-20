<properties 
    pageTitle="Vorgängen in unterschiedlichen Zuständen oder Status BizTalk-Dienste | Microsoft Azure" 
    description="In anderen MAK Status zulässigen Aktionen/Operationen: beenden, starten, starten, anhalten, fortsetzen, löschen, Skalierung, Konfiguration und Unterstützung von aktualisieren" 
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



# <a name="biztalk-services-service-state-chart"></a>BizTalk-Dienste: Service Zustandsdiagramms
Abhängig vom aktuellen Status der BizTalk-Dienst sind Vorgänge, die Sie oder der BizTalk-Dienst nicht möglich.

Beispielsweise wird einen neuen BizTalk-Dienst im klassischen Azure-Portal bereitgestellt. Nach erfolgreichem Abschluss wird der BizTalk-Dienst aktiviert Im aktiven Zustand können Sie den BizTalk-Dienst beenden. Wenn Stop erfolgreich ist, geht der BizTalk-Dienst auf einem angehaltenen Zustand. Schlägt Stopp geht der BizTalk-Dienst in einem StopFailed Zustand. In der StopFailed können Sie den BizTalk-Dienst starten. Wenn Sie eine Operation, der nicht zulässig ist versuchen, tritt der BizTalk-Dienst wie Lebenslauf folgender Fehler:

**Nicht zulässig**

BizTalk Service bereitstellen möchten, gehen Sie zu [BizTalk-Dienste: Bereitstellung mithilfe von Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/p/?LinkID=302280).

Tabellen Vorgänge oder Aktivitäten, die ausgeführt werden, wenn BizTalk Service in einem bestimmten Zustand befindet. Ein ✔ bedeutet, dass der Vorgang in dem Mitgliedstaat zulässig ist. Ein leerer Eintrag bedeutet, dass der Vorgang in diesem Zustand ausgeführt werden kann.

## <a name="start-stop-restart-suspend-resume-and-delete-operations"></a>Starten Sie, stoppen Sie, starten Sie, Anhalten Sie, fortsetzen und löschen
<table border="1">
<tr>
        <th colspan="15">Vorgang oder Aktion</th>
</tr>

<tr>
        <th rowspan="18">BizTalk-Dienststatus</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Starten</th>
        <th>Beenden</th>
        <th>Neu starten</th>
        <th>Anhalten</th>
        <th>Fortsetzen</th>
        <th>Löschen</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktiv</b></td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Deaktiviert</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Angehalten</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Beendet</b></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Service-Aktualisierung fehlgeschlagen</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
</table>
<br/>

## <a name="scale-update-configuration-and-backup-operations"></a>Skalieren, Konfiguration und Backup-Vorgänge
<table border="1">
<tr>
        <th colspan="15">Vorgang oder Aktion</th>
</tr>

<tr>
        <th rowspan="18">BizTalk-Dienststatus</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Skalierung</th>
        <th>Konfiguration aktualisieren</th>
        <th>Sicherung</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktiv</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Deaktiviert</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Angehalten</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Beendet</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Service-Aktualisierung fehlgeschlagen</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
</tr>
</table>

## <a name="see-also"></a>Siehe auch
- [BizTalk-Dienste: Bereitstellung mithilfe von Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [Der BizTalk-Dienste: Registerkarten Dashboard, Monitor und skalieren](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk-Dienste: Entwickler, Basic, Standard und Premium Editions Diagramm](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [Der BizTalk-Dienste: Sicherung und Wiederherstellung](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk-Dienste: Drosselung](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
- [BizTalk-Dienste: Ausstellername und Aussteller Schlüssel](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
- [Wie kann ich starten mithilfe von Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)


 
