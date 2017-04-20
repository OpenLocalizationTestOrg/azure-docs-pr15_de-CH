<properties
    pageTitle="Best Practices und Fehlerbehebung für Knoten Applikationen auf Azure Web Apps"
    description="Lernen Sie bewährte Methoden und Schritte zur Problembehandlung für Knoten Applikationen auf Azure Web Apps."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="ranjithr"
    manager="wadeh"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="06/06/2016"
    ms.author="ranjithr;wadeh"/>
    
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Best Practices und Fehlerbehebung für Knoten Applikationen auf Azure Web Apps

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

In diesem Artikel lernen Sie die best Practices und Problembehandlungsschritte [Knoten Applikationen](app-service-web-nodejs-get-started.md) auf Azure Webapps (mit [Iisnode](https://github.com/azure/iisnode)).

>[AZURE.WARNING] Verwenden Sie Schritte zur Problembehandlung auf Ihre. Wird empfohlen, Problembehandlung bei Ihrer Anwendung auf einem Produktionseinstellungen beispielsweise staging-Steckplatz und wenn das Problem behoben ist, tauschen Sie den staging-Steckplatz mit der Produktion.

## <a name="iisnode-configuration"></a>IISNODE-Konfiguration

Diese [Datei](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) zeigt alle Einstellungen für Iisnode konfiguriert werden können. Die Einstellungen, die für Ihre Anwendung nützlich sind:

* nodeProcessCountPerApplication

    Diese Einstellung steuert die Anzahl der Knoten Prozesse, die pro IIS-Anwendung gestartet werden. Standardwert ist 1. Starten so viele Node.exe als die Anzahl der Kerne VM durch diese Einstellung auf 0. Empfohlene Wert ist 0 für die meisten Anwendung, damit alle Kerne auf dem Computer nutzen zu können. Node.exe ist single threaded so eine node.exe maximal 1 Core belegt, um maximale Leistung aus Ihrer Anwendung Knoten alle Kerne nutzen möchten.

* nodeProcessCommandLine

    Diese Einstellung steuert den Pfad zu den node.exe. Sie können diesen Wert auf die Version node.exe festlegen.

* maxConcurrentRequestsPerProcess

    Diese Einstellung steuert die maximale Anzahl von gleichzeitigen Anfragen Iisnode an jeder node.exe. Der Standardwert ist auf Azure Webapps unendlich. Sie werden keinen dieser Einstellung kümmern. Der Standardwert ist außerhalb Azure Webapps 1024. Sie möchten dies konfigurieren, wie viele Anfragen, ruft die Anwendung ab und wie schnell die Anwendung jede Anforderung verarbeitet.

* maxNamedPipeConnectionRetry

    Diese Einstellung steuert die Höchstzahl Iisnode die Verbindung der named Pipe an die Anforderung über node.exe wiederholen. Diese Einstellung zusammen mit NamedPipeConnectionRetryDelay bestimmt das gesamte Timeout jeder Anforderung in Iisnode. Standardwert ist 200 Azure Webapps. Timeout in Sekunden insgesamt = (MaxNamedPipeConnectionRetry \* NamedPipeConnectionRetryDelay) / 1000

* namedPipeConnectionRetryDelay

    Diese Einstellung steuert der Betrag der Zeit (in Millisekunden) Iisnode zwischen den einzelnen Neuversuchen node.exe Anforderung an, über die named Pipe wartet. Standardwert ist 250ms.
    Timeout in Sekunden insgesamt = (MaxNamedPipeConnectionRetry \* NamedPipeConnectionRetryDelay) / 1000

    Standardmäßig wird das gesamte Timeout in Iisnode auf Azure Webapps 200 \* 250ms = 50 Sekunden.

* logDirectory

    Diese Einstellung steuert das Verzeichnis bei Iisnode Stdout/Stderr anmelden. Standardwert ist Iisnode ist relativ zum Haupt-Skript-Verzeichnis (Verzeichnis, in dem wichtigsten server.js vorhanden)

* debuggerExtensionDll

    Diese Einstellung steuert, welche Version von Knoten Inspektor Iisnode beim Debuggen Ihrer Anwendung Knoten verwenden. Iisnode-Inspektor-0.7.3.dll und Iisnode inspector.dll sind derzeit nur 2 gültige Werte für diese Einstellung. Standardwert ist Iisnode-Inspektor-0.7.3.dll. Iisnode-Inspektor-0.7.3.dll Version verwendet Knoten Inspektor 0.7.3 und Websockets Websockets auf Ihre Azure Webapp diese Version aktivieren müssen. Finden Sie unter <http://www.ranjithr.com/?p=98> Weitere Informationen zum Konfigurieren von Iisnode zum neue Knoten-Informationen verwenden.

* flushResponse

    Das Standardverhalten von IIS werden Daten puffert, 4 MB vor dem leeren oder bis zum Ende der Antwort ausstellt. Iisnode bietet eine Einstellung dieses Verhalten überschrieben: geleert ein Fragment des Antworttexts Entität als Iisnode von node.exe erhält, müssen Sie die iisnode/@flushResponse -Attribut in der Datei web.config auf "True":
    
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```

    Aktivieren jedes Fragment des Antworttexts Entität geleert Leistungsverlust, die den Durchsatz des Systems um 5 % (v0.1.13) reduziert werden, sollten diese Einstellung nur für Endpunkte Bereich, die streaming-Antwort erfordern (z.B. die <location> Element in der Datei web.config)

    Zusätzlich müssen für streaming-Applikationen Sie auch ResponseBufferLimit Iisnode Handler auf 0 setzen.
    
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```

* watchedFiles

    Dies ist eine durch Semikolons getrennte Liste der Dateien suchen überwacht werden. Eine Änderung an einer Datei wird die Anwendung wiederverwenden. Jeder Eintrag besteht aus eine optionale Verzeichnisnamen sowie erforderliche Dateiname relativ zum Verzeichnis sind, wo sich der Einstiegspunkt der Anwendung befindet. Platzhalterzeichen dürfen nur die Datei Namensteil. Standardwert ist "\*. js;web.config"

* recycleSignalEnabled

    Standardwert ist false. Wenn aktiviert, können die Knoten Anwendung eine named Pipe (Umgebungsvariable IISNODE\_CONTROL\_PIPE) und "Papierkorb" senden. Dadurch wird der w3wp ordnungsgemäß wiederverwendet.

* idlePageOutTimePeriod

    Standardwert ist 0, was bedeutet, dass dieses Feature deaktiviert ist. Wenn festgelegt auf einen Wert größer als 0, Iisnode, alle untergeordneten Prozesse alle 'IdlePageOutTimePeriod' Millisekunden Seite wird. Um zu verstehen, welche Seite bedeutet, finden Sie in dieser [Dokumentation](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). Diese Einstellung wird für Applikationen nützlich, die viel Speicher beanspruchen und ausgehende Seite Speicher auf der Festplatte gelegentlich einige RAM freigeben möchten.

>[AZURE.WARNING] Verwenden Sie die folgenden Konfigurationen Produktion aktivieren. Es wird empfohlen, nicht auf Produktion Applikationen aktivieren.

* debugHeaderEnabled

    Der Standardwert ist false. Auf true Iisnode jeder HTTP-Antwort eine HTTP-Antwort-Header Iisnode-Debug hinzugefügt wird, sendet er Headerwert Iisnode Debuggen ist eine URL. Einzelinformationen Diagnose anhand des URL-Fragments entnehmen können, aber eine viel bessere Darstellung wird erreicht, indem die URL im Browser öffnen.

* loggingEnabled

    Diese Einstellung steuert die Protokollierung von Stdout und Stderr von Iisnode. Iisnode erfassen Stdout/Stderr aus Knoten, die er gestartet und das Verzeichnis 'LogDirectory' angegeben. Wenn dies aktiviert ist, konnte die Anwendung wird schriftlich Protokolle im Dateisystem und je nach der Protokollierung der Anwendung, Leistungseinbußen.

* devErrorsEnabled

    Standardwert ist false. Wenn auf true Iisnode HTTP-Statuscodes und Win32-Fehlercode im Browser angezeigt. Win32-Code werden bestimmte Probleme Debuggen.
    
* DebuggingEnabled (auf live nicht aktivieren)

    Diese Einstellung steuert die debugging-Funktion. Knoten-Inspektor Iisnode integriert. Durch Aktivieren dieser Einstellung können Sie das Debuggen der Anwendung Knoten. Wenn diese Einstellung aktiviert ist, Iisnode wird Layout erforderlichen Knoten-Inspektor-Dateien im Verzeichnis "DebuggerVirtualDir" bei der ersten Anforderung Debuggen Ihrer Anwendung Knoten. Knoten-Inspektor Laden durch Senden einer Anforderung an http://yoursite/server.js/debug. Debug-URL-Segment mit "DebuggerPathSegment" gesteuert. Standard-DebuggerPathSegment = "debug". Diese lassen GUID beispielsweise, ist es schwieriger, von anderen erkannt werden.

    Überprüfen Sie diesen [Link](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) für Weitere Informationen zum Debuggen.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Szenarien und Empfehlung/Fehlerbehebung

### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Knoten Anwendung macht viele ausgehende Anrufe.

Häufig möchten ausgehende Verbindungen des regulären Betriebs machen. Wenn eine Anforderung eingeht, würde Ihre app Knoten beispielsweise REST-API an anderer Stelle Kontakt und Informationen für die Anforderung. Sie möchten einen Keep alive Agent verwenden http oder Https aufrufen. Beispielsweise können Sie das Agentkeepalive-Modul als Keep alive-Agent, wenn diese Gespräche. Dadurch wird sichergestellt, dass die Sockets auf Ihre Azure Webapp VM wiederverwendet werden und den Aufwand für das Erstellen von neuen Sockets für jede ausgehende Anforderung. Außerdem wird dadurch sichergestellt, dass Sie weniger Anzahl Sockets zu viele ausgehende Anfragen und daher nicht beim Überschreiten MaxSockets, die pro VM zugewiesen werden. Empfehlung Azure Webapps wäre AgentKeepAlive MaxSockets Wert insgesamt 160 Sockets pro VM. Daher haben Sie 4 node.exe auf dem virtuellen Computer ausführen möchten MaxSockets AgentKeepAlive auf 40 node.exe Stunden pro VM 160 festgelegt.

AgentKeepALive-Beispielkonfiguration:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

Es wird angenommen, dass 4 node.exe auf die VM ausgeführt haben. Haben Sie verschiedene node.exe auf dem virtuellen Computer ausführen, müssen Sie entsprechend MaxSockets ändern.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Knoten Anwendung belegt zu viel CPU.

Sie erhalten wahrscheinlich eine Empfehlung von Azure Webapps auf Ihr Portal über hohe CPU-Auslastung. Sie können auch Monitore bestimmter [Metriken](web-sites-monitor.md)überwacht einrichten. Bei der CPU-Auslastung auf [Azure Portal Dashboard](../application-insights/app-insights-web-monitor-performance.md)überprüfen Sie MAX Werte CPU, die Spitzenwerte verpassen.
In Fällen, wo man die Anwendung belegt zu viel CPU und kann nicht warum, müssen Sie die Knoten-Anwendung analysiert.

### 

#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>Profil der Anwendung Knoten Azure Webapps V8-Profiler

Zum Beispiel sagen Sie eine Hello World-Anwendung, die Profil wie folgt soll:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Besuchen Sie die Website https://yoursite.scm.azurewebsites.net/DebugConsole scm

Ein Eingabeaufforderungsfenster wird angezeigt, wie unten dargestellt. Wechseln Sie in das Verzeichnis/Wwwroot Website

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Führen Sie den Befehl "Npm installieren v8 Profiler"

Diese v8 Profiler unter Knoten installieren\_Modulverzeichnis und aller abhängigen.
Nun, bearbeiten Sie die server.js um Ihr Profil

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Darüber Funktion WriteConsoleLog Profil und dann das Profil 'profile.cpuprofile' Datei der Site Wwwroot Ausgabe. Sendet eine Anforderung an die Anwendung. Sie sehen eine 'profile.cpuprofile' unter der Website Wwwroot erstellt.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Diese Datei herunterladen und Sie müssen zum Öffnen dieser Datei mit Chrom F12. Drücken Sie F12 Chrome, klicken Sie auf der Registerkarte"Profile". Klicken Sie auf "Laden". Wählen Sie die profile.cpuprofile-Datei, die Sie gerade heruntergeladen haben. Klicken Sie auf das Profil, das gerade geladen.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Sie sehen, dass 95 % der WriteConsoleLog Funktion verbraucht wurde, wie unten dargestellt. Zeigt auch Sie genaue Zeilennummern und Quelldateien, die das Problem verursachen.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Knoten Anwendung verbraucht zu viel Speicher.

Sie erhalten wahrscheinlich eine Empfehlung von Azure Webapps auf das Portal zu hoher Speicherverbrauch. Sie können auch Monitore bestimmter [Metriken](web-sites-monitor.md)überwacht einrichten. Bei der Speicherverwendung [Azure Portal Dashboard](../application-insights/app-insights-web-monitor-performance.md)überprüfen Sie MAX Werte für Speicher, die Spitzenwerte verpassen.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Speicherverluste aktiviert und Heap vergleichen für node.js 

[Memwatch Knoten](https://github.com/lloyd/node-memwatch) können um Speicherverluste zu identifizieren.
Memwatch wie v8 Profiler installieren und Bearbeiten von Code und Diff Heaps Speicher zu Speicherverlusten in Ihrer Anwendung.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Meine Node.exe sind zufällig getötet 

Es gibt einige Gründe, warum dies passieren kann:

1.  Die Anwendung wirft nicht abgefangene Ausnahmen – Bitte Kontrollkästchen d:\\home\\Protokolldateien\\Anwendung\\Protokollierung magellanverzeichnis Datei Einzelheiten auf die ausgelöste Ausnahme. Diese Datei hat Stapelrahmen, die anhand dieser Anwendung beheben können.

2.  Die Anwendung belegt zu viel Arbeitsspeicher dem Einstieg andere Prozesse auswirkt. Ist der VM Gesamtarbeitsspeicher nahezu 100 %, konnte der Node.exe im Prozessmanager andere Prozesse können Sie arbeiten getötet. Dieses Problem beheben, stellen Sie sicher, dass Ihre Anwendung nicht Arbeitsspeicherbereiche oder wenn Sie Anwendung wirklich viel Arbeitsspeicher, bitte skalieren Sie einer größeren VM mit mehr RAM.

### <a name="my-node-application-does-not-start"></a>Knoten Anwendung wird nicht gestartet.

Wenn Ihre Anwendung 500 Fehler beim Systemstart zurückgeben, kann es Gründe:

1.  Node.exe ist nicht an der richtigen Position vorhanden. Überprüfen Sie NodeProcessCommandLine festlegen.

2.  Haupt-Skriptdatei ist nicht an der richtigen Position vorhanden. Überprüfen Sie web.config und sicherstellen Sie, dass die wichtigsten Skriptdatei mit dem Namen der wichtigsten Skriptdatei im Abschnitt Ereignishandler übereinstimmt.

3.  Konfiguration der Datei Web.config ist nicht korrekt-Namen/Einstellungswerte überprüfen.

4.  Kaltstart – die Anwendung dauert zu lange zum Starten. Wenn Ihre Anwendung länger als dauert (MaxNamedPipeConnectionRetry \* NamedPipeConnectionRetryDelay) / 1000 Sekunden Iisnode 500 Fehler zurück. Erhöhen Sie die Werte dieser Einstellungen die Startzeit der Anwendung zu Iisnode Timeout und 500 Fehler zurückgegeben.

### <a name="my-node-application-crashed"></a>Knoten Anwendung abgestürzt

Die Anwendung wirft nicht abgefangene Ausnahmen – Bitte Kontrollkästchen d:\\home\\Protokolldateien\\Anwendung\\Protokollierung magellanverzeichnis Datei Einzelheiten auf die ausgelöste Ausnahme. Diese Datei hat Stapelrahmen, die anhand dieser Anwendung beheben können.

### <a name="my-node-application-takes-too-much-time-to-startup-cold-start"></a>Knoten Anwendung nimmt zu viel Zeit für den Start (Kaltstart)

Häufigste Ursache hierfür ist, dass die Anwendung viele Dateien im Knoten\_Module und die Anwendung versucht, diese Dateien während des Startvorgangs geladen. Standardmäßig da Dateien auf der Netzwerkfreigabe auf Azure Webapps befinden kann so viele Dateien geladen einige Zeit dauern.
Einige dieser schneller sind:

1.  Sicherstellen Sie, dass Sie mit npm3 die Module flache Abhängigkeitsstruktur und keine doppelten Abhängigkeiten haben.

2.  Versuchen, verzögerte Laden den Knoten\_Module und alle Module beim Start nicht geladen. Dies bedeutet, dass der Aufruf von require('module') ausgeführt werden soll, wenn Sie versuchen, das Modul tatsächlich innerhalb der Funktion benötigt werden.

3.  Azure Webapps bietet einen lokalen Cache genannt. Diese Funktion kopiert die Inhalte von der Netzwerkfreigabe auf die lokale Festplatte des virtuellen Computers. Da die Dateien lokal, Ladezeit des Knotens\_ist viel schneller. - [Dokumentation](../app-service/app-service-local-cache.md) erläutert ausführlich lokalen Cache verwenden.

## <a name="iisnode-http-status-and-substatus"></a>HTTP-Status IISNODE und Unterstatusfehlercodes

Diese [Datei](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) Listet alle möglichen Status-Unterstatus Kombination Iisnode bei Fehler zurückgeben kann.

Aktivieren von FREB für Ihre Anwendung den Win32-Fehlercode (Bitte vergewissern können FREB auf nicht-Produktionsstandorte aus Leistungsgründen).

| HTTP-Status | HTTP-Teilstatus | Mögliche Ursache?                                                                                                                                                                                            
|-------------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------
| 500         | 1000           | Gab es ein Problem Weiterleiten der Anforderung zu IISNODE – überprüfen Sie, ob node.exe gestartet wurde. Node.exe konnte beim Start abgestürzt. Überprüfen Sie die Konfiguration der Datei web.config Fehler.                                                                                                                                                                                                                                                                                     |
| 500         | 1001           | -Win32Error 0 x 2 - Anwendung reagiert nicht auf den URL. Überprüfen Sie URL-Rewrite-Regeln oder express app korrekten definierten Routen. -Win32Error 0x6d-named-Pipe ausgelastet – Node.exe nimmt Anfragen, da die Leitung besetzt ist. Überprüfen Sie hohe CPU-Auslastung. -Fehler überprüfen Sie, ob node.exe abgestürzt.
| 500         | 1002           | Node.exe abgestürzt – d: Überprüfen\\home\\Protokolldateien\\Protokollierung magellanverzeichnis für Stapelrahmen.                                                                                                                                                                                                                                                                                                                                                                                        |
| 500         | 1003           | Pipe Konfiguration Problem – sollten Sie nie angezeigt, aber ansonsten der benannten Pipe ist falsch konfiguriert.                                                                                                                                                                                                                                                                                                                                                          |
| 500         | 1004 1018      | Fehler ist beim Senden der Anforderung oder die Antwort nach node.exe verarbeitet. Überprüfen Sie, ob node.exe abgestürzt. d: Überprüfen\\home\\Protokolldateien\\Protokollierung magellanverzeichnis für Stapelrahmen.                                                                                                                                                                                                                                                                                    |
| 503         | 1000           | Nicht genügend Speicherplatz zum Reservieren von mehr named Pipe-Verbindungen. Überprüfen, warum Ihre app so viel Speicher in Anspruch nimmt. Überprüfen Sie MaxConcurrentRequestsPerProcess Wert. Wenn nicht unendlich und Sie viele Anfragen, erhöhen Sie diesen Wert, um diesen Fehler zu vermeiden.                                                                                                                                                                                                                                                                                                                  |
| 503         | 1001           | Anforderung konnte nicht an node.exe verteilt werden, da die Anwendung wiederverwendet. Nachdem die Anwendung erneuert wurde, sollte normalerweise Anfragen bedient werden.                                                                                                                                                                                                                                                                                                               |
| 503         | 1002           | Kontrollkästchen Win32-Fehlercode tatsächliche Ursache-Anforderung konnte nicht zu einer node.exe verteilt werden.                                                                                                                                                                                                                                                                                                                                                                               |
| 503         | 1003           | Named Pipe ausgelastet ist, überprüfen Sie, ob Knoten viel CPU verbraucht                                                                                                                                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
Eine Einstellung in Knoten NODE.exe\_ausstehend\_PIPE\_Instanzen. Dieser Wert ist standardmäßig außerhalb Azure Webapps 4. Dies bedeutet, dass diese node.exe nur 4 Anfragen named Pipe annehmen können. Dieser Wert auf 5000 eingestellt und sollte dieser Wert reicht für die meisten Knoten auf Azure Webapps Azure Webapps. Sie sollten sehen 503.1003 auf Azure Webapps haben wir einen hohen Wert für den Knoten\_ausstehend\_PIPE\_Instanzen.  |

## <a name="more-resources"></a>Weitere Ressourcen

Folgen Sie diesen Links erfahren Sie mehr zu node.js in Azure App Service.

* [Erste Schritte mit Node.js webapps in Azure App Service](app-service-web-nodejs-get-started.md)
* [Das Debuggen einer Node.js Web app in Azure App Service](web-sites-nodejs-debug.md)
* [Node.js-Module verwenden mit Azure](../nodejs-use-node-modules-azure-apps.md)
* [Azure App Service webapps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js-Entwicklercenter](../nodejs-use-node-modules-azure-apps.md)
* [Großes Geheimnis Kudu der Debug-Konsole durchsuchen](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)