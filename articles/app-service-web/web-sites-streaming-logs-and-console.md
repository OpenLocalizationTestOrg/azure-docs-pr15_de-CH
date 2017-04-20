<properties 
    pageTitle="Streaming-Protokolle und Konsole" 
    description="Streaming-Protokolle und (Übersicht)" 
    authors="btardif" 
    manager="wpickett" 
    editor="" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="byvinyal"/>

# <a name="streaming-logs-and-the-console"></a>Streaming-Protokolle und die Konsole

## <a name="streaming-logs"></a>Streaming-Protokolle

**Azure-Portal** bietet eine integrierte streaming Protokollanzeige, die Ablaufverfolgungsereignisse Ihren **App Service** -Apps in Echtzeit angezeigt.  

Diese Funktion einrichten erfordert einige einfache Schritte:

- Spuren im Code schreiben
- Aktivieren Sie Anwendung **Diagnoseprotokolle** für Ihre Anwendung
- Den Stream von der integrierten Benutzeroberfläche **Streaming-Protokolle** in **Azure-Portal**anzeigen

### <a name="how-to-write-traces-in-your-code"></a>Spuren im Code schreiben ###

Schreiben von Spuren im Code ist einfach.  In C# ist es einfach folgenden Code schreiben:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

Die Trace-Klasse befindet sich im System.Diagnostics-Namespace.

Node.js-App können Sie diesen Code, um das gleiche Ergebnis schreiben:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>Aktivieren und Anzeigen der streaming-Protokolle
![][BrowseSitesScreenshot]Diagnosen werden pro Anwendung aktiviert. Zunächst zu der Website, der Sie dieses Feature aktivieren möchten.  
  
![][DiagnosticsLogs]Menü unten im Abschnitt **Überwachung** und klicken Sie auf **(1) Diagnoseprotokollen**. Anschließend ermöglicht **Anwendung Protokollierung (Dateisystem)** **(2) aktivieren** oder **Anwendung protokollieren (Blob)** die Option **Ebene** den Schweregrad zu erfassen. Wenn Sie nur mit der Funktion vertraut, Einstellen der **ausführlich** , um sicherzustellen, dass alle Anweisung Trace gesammelt.

Klicken Sie am oberen Rand der Blade **Speichern** und Protokolle anzeigen.

>[AZURE.NOTE] Je höher der **Schweregrad** Protokoll und weitere Spuren mehr Ressourcen verbraucht werden entstehen. Sicherstellen Sie, dass der **Schweregrad** der richtigen Ausführlichkeit für eine Produktion oder hohem Datenverkehr Website konfiguriert ist. 

![][StreamingLogsScreenshot]Klicken Sie die **streaming-Protokolle** von Azure Portal **(1)-Datenstrom** auch im Abschnitt **Überwachung** Einstellungsmenü. Wenn Ihre Anwendung aktiv Trace-Anweisung schreiben, sollten Sie sie in der **Benutzeroberfläche protokolliert (2) streaming** in Echtzeit sehen.

## <a name="console"></a>Konsole
**Azure-Portal** bietet Zugriff auf die Konsole für Ihre Anwendung. Erkunden der app Dateisystem und Powershell-Cmd-Skripts ausführen. Sie werden denselben Berechtigungssatz als ausgeführte app Code Ausführung Befehle gebunden. Zugriff auf geschützte Verzeichnisse oder Skripts, die höhere Berechtigungen erfordern blockiert.  

![][ConsoleScreenshot]Menü Einstellungen Scrollen **Entwicklungstools** und klicken Sie auf Konsole **(1)** und **(2) Konsole** UI rechts geöffnet.

Versuchen Sie, mit der **Konsole**vertraut grundlegende Befehle wie:

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
