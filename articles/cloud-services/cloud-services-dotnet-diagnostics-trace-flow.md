<properties
    pageTitle="Verfolgen Sie den Datenfluss in einer Cloud Services-Anwendung mit Azure-Diagnose | Microsoft Azure"
    description="Ein Azure-Anwendung zu debuggen, Messen der Leistung, Überwachung, Analyse und mehr fügen Sie Tracing Nachrichten hinzu."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Verfolgen des Ablaufs einer Cloud-Services-Anwendung mit Azure-Diagnose

Protokollierung ist eine Möglichkeit für die Ausführung der Anwendung während der Ausführung überwachen. [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx)und [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) Klassen können Informationen über Fehler und Ausführung in Protokollen, Textdateien oder anderen Geräten für die spätere Analyse aufzeichnen. Weitere Informationen zu Tracing finden Sie unter [Tracing and Instrumenting Applications](https://msdn.microsoft.com/library/zs6s4h68.aspx).


## <a name="use-trace-statements-and-trace-switches"></a>Trace-Anweisungen und Ablaufverfolgungsschalter verwenden

Verfolgung in der Cloud-Dienste Anwendung der Anwendungskonfiguration [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) hinzufügen und das Aufrufen von System.Diagnostics.Trace oder System.Diagnostics.Debug im Anwendungscode implementieren. Verwenden Sie Konfiguration Datei *app.config* für WorkerThreads und *web.config* für Webrollen. Beim Erstellen eines neuen gehosteten Diensts mithilfe einer Visual Studio-Vorlage Azure-Diagnose wird automatisch dem Projekt hinzugefügt und der DiagnosticMonitorTraceListener wird die entsprechende Konfigurationsdatei für die Rollen, die Sie hinzufügen hinzugefügt.

Weitere Informationen zum Trace-Anweisung platzieren [How to: Add Trace Statements to Application Code](https://msdn.microsoft.com/library/zd83saa2.aspx).

[Trace-Schalter](https://msdn.microsoft.com/library/3at424ac.aspx) im Code platzieren, können Sie steuern, ob Tracing auftritt und wie umfangreich ist. So können Sie den Status der Anwendung in der Produktion überwachen. Dies ist besonders wichtig in einer Business-Anwendung, die mehrere Komponenten auf mehreren Computern verwendet. Weitere Informationen finden Sie unter [wie: Konfigurieren von Trace-Schaltern](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Konfigurieren des Ablaufverfolgungslisteners in Azure-Anwendung

Trace Debug- und TraceSource, benötigen Sie "Listener" sammeln und Aufzeichnen der Nachrichten. Listener sammeln, speichern und Verfolgung Nachrichten. Sie leiten die Ablaufverfolgungsausgabe an ein entsprechendes Ziel wie Protokolldateien, Fenster oder Textdatei. Azure-Diagnose verwendet die [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) -Klasse.

Bevor Sie das folgende Verfahren durchführen, müssen Sie den Azure Diagnose Monitor initialisieren. Hierzu finden Sie unter [Aktivieren der Diagnose in Microsoft Azure](cloud-services-dotnet-diagnostics.md).

Beachten Sie, dass wenn Sie Vorlagen, die von Visual Studio bereitgestellt werden verwenden, die Konfiguration des Listeners automatisch hinzugefügt wird.


### <a name="add-a-trace-listener"></a>Einen Ablaufverfolgungslistener hinzufügen

1. Öffnen Sie die Datei web.config oder app.config für Ihre Rolle.
2. Fügen Sie folgenden Code in die Datei. Ändern Sie das Versionsattribut Verwendung die Versionsnummer der Assembly, auf die Sie verweisen. Die Version der Assembly ändert nicht notwendigerweise mit jedem Azure SDK, sofern keine Updates.

    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                  <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
    >[AZURE.IMPORTANT] Stellen Sie sicher, dass Sie einen Projektverweis auf die Assembly Microsoft.WindowsAzure.Diagnostics. Aktualisieren Sie die Versionsnummer im obigen XML-Version der referenzierten Assembly Microsoft.WindowsAzure.Diagnostics entsprechend.

3. Speichern Sie die Datei Config.

Weitere Informationen zum Listener finden Sie unter [Ablaufverfolgungslistener](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Nachdem Sie die Listener hinzugefügt haben, können Sie Code Trace-Anweisung hinzufügen.


### <a name="to-add-trace-statement-to-your-code"></a>Trace-Anweisung Code hinzufügen

1. Öffnen Sie eine Datei für die Anwendung. Angenommen, die <RoleName>cs-Datei für die Arbeitskraft oder Webrolle.
2. Fügen Sie die folgende Anweisung verwenden, wenn nicht bereits hinzugefügt wurde:
    ```
        using System.Diagnostics;
    ```
3. Nehmen Sie Trace-Anweisung Informationen über den Zustand der Anwendung erfasst werden soll. Eine Vielzahl von Methoden können zum Formatieren der Ausgabe von Trace-Anweisung. Weitere Informationen finden Sie unter [How to: Add Trace Statements to Application Code](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Speichern Sie die Datei.
