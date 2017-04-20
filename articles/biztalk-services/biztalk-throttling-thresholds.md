<properties 
    pageTitle="Erfahren Sie mehr über BizTalk-Dienste Drosselung | Microsoft Azure" 
    description="Informationen Sie zu drosseln Schwellenwerte und was Laufzeitverhalten für BizTalk-Dienste. Drosselung Speicherverwendung und Anzahl der Nachrichten basiert. MAK, WABS" 
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





# <a name="biztalk-services-throttling"></a>BizTalk-Dienste: Drosselung

Azure BizTalk-Dienste implementiert Service Drosselung basierend auf zwei mögliche Ursachen: die Speicherverwendung und die Anzahl der Nachrichten gleichzeitig verarbeiten. Dieses Thema listet die Drosselung Schwellenwerte und das Laufzeitverhalten Drosselung Fall beschrieben.

## <a name="throttling-thresholds"></a>Drosselung Schwellenwerte

Tabelle der Drosselung Quelle und Schwellenwerte:

||Beschreibung|Niedriger Schwellenwert|Hoher Schwellenwert|
|---|---|---|---|
|Speicher|total System Memory verfügbar/PageFileBytes. <p><p>Total verfügbare PageFileBytes ist ca. 2 Mal RAM des Systems.|60 %|70 %|
|Die Verarbeitung von Nachrichten|Anzahl der Nachrichten, die gleichzeitig|40 * Anzahl der Kerne|100 * Anzahl der Kerne|

Wenn ein hoher Schwellenwert erreicht wird, startet Azure BizTalk-Dienste einschränken. Drosselung beendet, wenn der niedrige Schwellenwert erreicht wird. Ihr Dienst verwendet beispielsweise 65 % Systemspeicher. In diesem Fall wird der Dienst nicht einschränken. Der Dienst startet mit 70 % Systemspeicher. In diesem Fall wird der Dienst Steuerung und weiterhin Drosseln, bis der Dienst 60 % (niedrige Schwellenwert) Systemspeicher verwendet.

Azure BizTalk-Dienste verfolgt die Drosselung Status (Normalzustand und reduzierten Zustand) und Drosselung Dauer.


## <a name="runtime-behavior"></a>Laufzeitverhalten

Wenn Azure BizTalk Services Drosselung Zustand wechselt, geschieht Folgendes:

- Beschränkung ist pro Instanz der Rolle. Zum Beispiel:<br/>
RoleInstanceA Einschränkung. RoleInstanceB ist keine Einschränkung. In diesem Fall werden Nachrichten in RoleInstanceB wie erwartet. Nachrichten in RoleInstanceA werden verworfen, und die folgenden Fehler:<br/><br/>
**Server ist ausgelastet. Bitte versuchen Sie es erneut.**<br/><br/>
- Alle Quellen Pull abrufen oder eine Nachricht nicht. Zum Beispiel:<br/>
Eine Pipeline abruft Nachrichten von externen FTP-Quelle. Ausführen der Pull Rolleninstanz wird in einen Drosselung. In diesem Fall beendet die Pipeline weitere Nachrichten downloaden, bis die Rolleninstanz Drosselung beendet.
- Eine Antwort wird an den Client gesendet, damit der Client die Nachricht erneut übermitteln kann.
- Sie müssen warten, bis die Drosselung. Warten Sie, bis der niedrige Schwellenwert erreicht wird.

## <a name="important-notes"></a>Wichtige Hinweise
- Einschränkung kann nicht deaktiviert werden.
- Drosselung Schwellenwerte kann geändert werden.
- Drosselung ist systemweit implementiert.
- Azure SQL-Datenbankserver hat auch integrierte Drosselung.

## <a name="additional-azure-biztalk-services-topics"></a>Weitere Themen Azure BizTalk-Dienste

-  [Azure BizTalk-Dienste SDK installieren](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Lernprogramme: Azure BizTalk-Dienste](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Wie kann ich starten mithilfe von Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk-Dienste](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Siehe auch
- [BizTalk-Dienste: Entwickler, Basic, Standard und Premium Editions Diagramm](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk-Dienste: Bereitstellung mithilfe von Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk-Dienste: Bereitstellung Statusdiagramm](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [Der BizTalk-Dienste: Registerkarten Dashboard, Monitor und skalieren](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [Der BizTalk-Dienste: Sicherung und Wiederherstellung](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk-Dienste: Ausstellername und Aussteller Schlüssel](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
 
