<properties
   pageTitle="Hintergrund Aufträge Hinweise | Microsoft Azure"
   description="Anleitung zum Hintergrund Aufgaben, unabhängig von der Benutzeroberfläche ausführen."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="masashin"/>

# <a name="background-jobs-guidance"></a>Hintergrund Aufträge Anleitung

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>Übersicht

Viele Anwendungstypen erfordern Hintergrundaufgaben unabhängig von der Benutzeroberfläche (UI) ausgeführt. Beispiele: Stapelverarbeitungen, intensive Verarbeitungsaufgaben und Prozessen wie Workflows. Hintergrundjobs ohne Benutzerinteraktion - Anwendung ausgeführt werden können der Auftrag gestartet und dann interaktive Anfragen vom Benutzer bearbeitet werden. Dadurch kann die Belastung der Anwendungsbenutzeroberfläche minimieren die Verfügbarkeit verbessern und interaktive Reaktionszeiten verringern.

Wenn eine Anwendung zu Benutzern hochgeladen werden Miniaturansichten von Bildern erforderlich ist, können sie z. B. als Hintergrund und speichern Miniaturansicht Speicher Abschluss – ohne zu warten, bis der Vorgang abgeschlossen werden. Auf gleiche Weise kann ein Benutzer eine Bestellung einen Hintergrund Workflow initiieren, der die Reihenfolge die Benutzeroberfläche kann den Benutzer weiterhin Browsen Web app verarbeitet. Nach Abschluss der Hintergrundauftrag können sie gespeicherte Aufträge Daten aktualisieren und senden eine e-Mail an den Benutzer, der die Bestellung bestätigt wird.

Wenn Sie überlegen, ob eine Aufgabe im Hintergrund implementiert das wichtigste Kriterium ist, ob der Task ohne Benutzereingriff und ohne Benutzeroberfläche ausgeführt zu warten, bis der Auftrag abgeschlossen werden. Aufgaben, die der Benutzer oder die Benutzeroberfläche warten abgeschlossen sind, erfordern möglicherweise nicht als Hintergrundjobs.

## <a name="types-of-background-jobs"></a>Typen von Hintergrundjobs

Hintergrundjobs umfassen in der Regel eine oder mehrere der folgenden Projekte:

- CPU-intensiven Aufträge Modellanalyse oder mathematische Berechnung.
- I/O-Intensive Aufträge oder Ausführung einer Reihe von Speichertransaktionen Indizierung.
- Stapelverarbeitungen Updates nächtlichen oder geplanten Verarbeitung.
- Lang andauernde Workflows oder Erfüllung, Services und Systeme bereitstellen.
- Vertrauliche Daten verarbeiten, die Aufgabe zu einem sichereren Ort zur Verarbeitung übergeben wird. Z. B. möchten nicht vertrauliche Daten in einer Webanwendung. Stattdessen können Sie ein Muster wie [Gatekeeper](http://msdn.microsoft.com/library/dn589793.aspx) isolierten Hintergrundprozess Daten übertragen, die auf geschützten Speicher zugreifen.

## <a name="triggers"></a>Trigger

Hintergrundjobs können auf verschiedene Arten erfolgen. Sie können in die folgenden Kategorien:

- [**Trigger Event driven**](#event-driven-triggers). Die Aufgabe wird als Antwort auf ein Ereignis in der Regel eine Aktion von einem Benutzer oder einem Schritt in einem Workflow gestartet.
- [**Trigger Zeitplan gesteuert**](#schedule-driven-triggers). Die Aufgabe wird auf einen Zeitplan basierend auf einem Zeitgeber aufgerufen. Möglicherweise einen wiederkehrenden Zeitplan oder einen einmaligen Aufruf für einen späteren Zeitpunkt angegeben ist.

### <a name="event-driven-triggers"></a>Ereignisgesteuerte Trigger

Ereignisgesteuerte verwendet einen Trigger Hintergrundtask zu starten. Beispiele für ereignisgesteuerte Trigger verwenden:

- Die Benutzeroberfläche oder einer anderen Stelle platziert eine Nachricht in einer Warteschlange. Die Nachricht enthält Daten über eine Aktion, wie der Benutzer eine Bestellung ist. Hintergrundaufgabe überwacht diese Warteschlange und erkennt das Eintreffen einer neuen Nachricht. Es liest die Meldung und die Daten darin als Eingabe für den Hintergrundauftrag verwendet.
- Die Benutzeroberfläche oder einer anderen Stelle gespeichert oder einen Wert im Speicher aktualisiert. Der Hintergrundtask Speicher überwacht und erfasst. Liest die Daten und als Eingabe für den Hintergrundauftrag verwendet.
- Die Benutzeroberfläche oder einer anderen Stelle stellt eine Anfrage an einen Endpunkt kein URI für HTTPS oder eine API, die als Webdienst verfügbar gemacht wird. Die Daten, die erforderlich ist, um den Hintergrund Aufgabe als Teil der Anforderung übergeben. Der Endpunkt oder Web Service ruft Hintergrundaufgabe, die Daten als Eingabe verwendet.

Beispiele für ereignisgesteuerte Aufruf geeignet sind Aufgaben umfassen Verarbeitung Workflows Senden von Informationen an remote Services, Senden von e-Mail-Nachrichten und Bereitstellung neuer Benutzer mandantenfähigen Applikationen.

### <a name="schedule-driven-triggers"></a>Trigger Zeitplan gesteuert

Zeitplan-gesteuerte verwendet einen Zeitgeber Hintergrundtask zu starten. Beispiele für die Verwendung von Triggern Zeitplan gesteuert:

- Ein Zeitgeber, der in der Anwendung oder im Betriebssystem die Anwendung lokal ausgeführt wird Ruft eine Hintergrundaufgabe regelmäßig.
- Ein Zeitgeber, der in eine andere Anwendung oder ein Timerdienst wie Azure Scheduler läuft sendet eine Anforderung an eine API oder Web Service regelmäßig. API oder Web Service ruft Hintergrundtask.
- Ein separater Prozess oder eine Anwendung startet einen Zeitgeber, bei dem der Hintergrundtask nach der angegebenen Verzögerung oder zu einem bestimmten Zeitpunkt aufgerufen werden.

Normale Zeitplan basierenden Aufruf geeignet sind Aufgaben gehören Stapelverarbeitung Routinen (z. B. aktualisieren-Produkten Listen für Benutzer basierend auf ihrer aktuellen Verhalten), Verarbeitung der routinemäßigen Aufgaben (oder Aktualisieren von Indizes kumulierte Ergebnisse), Analyse täglich Berichte und Data Retention Bereinigung Daten Konsistenz überprüft.

Verwenden Sie eine Aufgabe Zeitplan gesteuert, die als einzelne Instanz ausführen muss, werden Sie sollten Sie Folgendes beachten:

- Wird skaliert die Serverinstanz, die den Planer (wie virtuelle Computer mithilfe von Windows-Taskplaner) ausgeführt wird, müssen Sie mehrere Instanzen des Planers ausführen. Diese können mehrere Instanzen der Aufgabe starten.
- Wenn Aufgaben länger als der Zeitraum zwischen Planer ausführen, kann der Planer eine andere Instanz der Aufgabe starten, während vorherigen läuft.

## <a name="returning-results"></a>Zurückgeben von Ergebnissen

Hintergrundjobs asynchron in einem getrennten Prozess oder auch an einem anderen Speicherort aus der Benutzeroberfläche oder der Prozess, der Hintergrundaufgabe aufgerufen. Im Idealfall Hintergrundaufgaben sind "fire and forget" Vorgänge und deren Ausführungsstatus wirkt sich nicht auf der Benutzeroberfläche oder der aufrufende Prozess. Dies bedeutet, dass der aufrufende Prozess nicht auf Aufgaben wartet. Daher kann automatisch erkennen, am Ende des Vorgangs.

Benötigen Sie eine Hintergrundaufgabe aufrufende Aufgabe an Fortschritt oder Abschlussereignisse kommunizieren, müssen Sie einen Mechanismus für diese implementieren. Beispiele sind:

- Schreiben Sie einen Statuswert Indikator in Speicher für die Aufgabe UI oder Aufrufer überwachen oder Überprüfen Sie diesen Wert bei Bedarf. Andere Hintergrundaufgabe an den Aufrufer zurückgeben muss Daten können in denselben Speicher platziert werden.
- Richten Sie eine Antwortwarteschlange, der die Benutzeroberfläche oder die Aufrufer überwacht. Hintergrundtask kann Nachrichten in der Warteschlange, der Status und Abschluss anzugeben. Hintergrundaufgabe an den Aufrufer zurückgeben muss Daten können in Nachrichten platziert werden. Bei Verwendung von Azure Service Bus können Sie Eigenschaften **ReplyTo** und **CorrelationId** , um diese Funktion zu implementieren. Weitere Informationen finden Sie unter [Korrelation Service Bus Messaging vermittelt](http://www.cloudcasts.net/devguide/Default.aspx?id=13029).
- Setzen Sie eine API oder einen Endpunkt aus der Hintergrundtask UI oder Aufrufer zugreifen kann, um Statusinformationen zu erhalten. Hintergrundaufgabe an den Aufrufer zurückgeben muss Daten können in der Antwort enthalten sein.
- Sollen Sie Hintergrund Status an vordefinierten Punkten oder Abschluss an die Benutzeroberfläche oder die Aufrufer über eine API aufrufen. Dies möglicherweise durch Ereignisse lokal oder über einen Mechanismus zum Veröffentlichen und abonnieren. Hintergrundaufgabe an den Aufrufer zurückgeben muss Daten können in der Anforderung oder Ereignis-Nutzlast enthalten.

## <a name="hosting-environment"></a>Hosting-Umgebung

Hintergrundaufgaben können mit verschiedenen Azure Platform Services gehostet werden:

- [**Azure webapps und Webaufträge**](#azure-web-apps-and-webjobs). Webaufträge können Sie benutzerdefinierte Aufträge basierend auf einer Reihe von verschiedenen Arten von Skripts oder ausführbare Programme im Rahmen einer Webanwendung ausführen.
- [**Web- und Workerrollen Azure Cloud Services-Rollen**](#azure-cloud-services-web-and-worker-roles). Sie können Code in eine Funktion schreiben, die als Hintergrundtask ausgeführt wird.
- [**Azure Virtual Machines**](#azure-virtual-machines). Wenn Sie einen Windows-Dienst oder Windows Task Scheduler verwenden möchten, gilt die Hintergrundaufgaben in einem dedizierten virtuellen Computer hosten.
- [**Azure Batch**](./batch/batch-technical-overview.md). Ist ein Plattformdienst, der rechenintensiver Arbeit auf einer verwalteten Auflistung von virtuellen Maschinen plant und kann automatisch skalieren Datenverarbeitungsressourcen auf die Bedürfnisse von Aufträgen.

In den folgenden Abschnitten werden diese Optionen ausführlicher beschrieben und Hinweise zur Auswahl die entsprechende Option enthalten.

## <a name="azure-web-apps-and-webjobs"></a>Azure webapps und Webaufträge

Azure Webaufträge können Sie benutzerdefinierte Aufträge als eine Azure Web App Hintergrundaufgaben ausführen. Webaufträge werden im Kontext Ihrer Anwendung als einen fortlaufenden Prozess ausgeführt. Webaufträge werden außerdem auf ein auslösendes Ereignis Azure Scheduler oder externe Faktoren wie Storage Blobs und Nachrichtenwarteschlangen ausgeführt. Aufträge können gestartet und bei Bedarf angehalten und ordnungsgemäß heruntergefahren. Schlägt eine ständig laufende Webauftrag wird er automatisch neu gestartet. Aktionen wiederholen und Fehler werden konfiguriert.

Wenn Sie ein Webauftrag konfigurieren:

- Wenn Sie ein ereignisgesteuertes Trigger beantworten sollen, sollten Sie es als **fortlaufend ausführen**konfigurieren. Skript oder Programm wird in der Site/Wwwroot/App_data/Aufträge/continuous Ordner gespeichert.
- Wenn Sie Trigger Zeitplan gesteuert beantworten sollen, sollten Sie es **nach einem Zeitplan**ausgeführt konfigurieren. Skript oder Programm befindet sich im Ordner Website/Wwwroot/App_data/Aufträge/ausgelöst.
- Wenn Sie beim Konfigurieren eines Auftrags die Option **bei Bedarf ausführen** auswählen, werden beim Starten den gleichen Code wie die Option **planmäßig ausführen** ausgeführt.

Azure Webaufträge werden in der Sandbox Web app. Dies bedeutet, dass sie Zugriff auf Umgebungsvariablen und Informationen wie Verbindungszeichenfolgen für Web app freigeben. Der Auftrag hat Zugriff auf den eindeutigen Bezeichner des Computers, der der Auftrag ausgeführt wird. Die Verbindungszeichenfolge mit dem Namen **AzureWebJobsStorage** bietet Zugriff auf Warteschlangen Azure-Speicher, Blobs und Tabellen für Anwendungsdaten und Zugriff auf Service Bus für messaging und Kommunikation. Die Verbindungszeichenfolge mit dem Namen **AzureWebJobsDashboard** ermöglicht den Zugriff auf die Protokolldateien Auftragsaktion.

Azure Webaufträge haben folgende Eigenschaften:

- **Sicherheit**: Webaufträge werden durch die Anmeldeinformationen für die Bereitstellung der Web-app geschützt.
- **Unterstützte Dateitypen**: Webaufträge mithilfe Befehlsskripts (cmd), Batch-Dateien (. bat), PowerShell-Skripts (. ps1) definieren, bash Shell-Skripts (.sh), PHP-Skripts (PHP) Python-Skripts (.py), JavaScript-Code (JS) und ausführbare Programme (.exe JAR und mehr).
- **Bereitstellung**: Bereitstellen von Skripts und ausführbare Dateien mithilfe des Azure-Portals den Add-in- [WebJobsVs](https://visualstudiogallery.msdn.microsoft.com/f4824551-2660-4afa-aba1-1fcc1673c3d0) für Visual Studio oder [Visual Studio 2013 Update 4](http://www.visualstudio.com/news/vs2013-update4-rc-vs) (erstellen und diese Option bereitstellen), mit [Azure Webaufträge SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)oder direkt an den folgenden Speicherorten kopieren:
  - Ausführung ausgelöst: Site/Wwwroot/App_data/Aufträge/ausgelöst / {job Name}
  - Für die fortlaufende Ausführung: Site/Wwwroot/App_data/Aufträge/continuous / {job Name}
- **Protokollierung**: Console.Out werden Informationen (markiert). Console.Error wird als Fehler behandelt. Überwachung und Diagnose-Informationen möglich mithilfe des Azure-Portals. Sie können Dateien direkt von der Website herunterladen. Sie werden an den folgenden Speicherorten gespeichert:
  - Ausführung ausgelöst: Vfs/Daten/Aufträge/ausgelöst/JobName
  - Für die fortlaufende Ausführung: Vfs/Daten/Aufträge/continuous/JobName
- **Konfiguration**: Webaufträge können mithilfe des Portals, REST-API und PowerShell konfigurieren. Eine Konfigurationsdatei mit dem Namen settings.job im gleichen Root-Verzeichnis als Job-Skript können Sie Konfigurationsinformationen für ein Projekt bereitstellen. Zum Beispiel:
  - {"Stopping_wait_time": 60}
  - {"Is_singleton": true}

### <a name="considerations"></a>Hinweise

- Standardmäßig skalieren Webaufträge Web App. Jedoch können Sie Aufträge Instanz ausgeführt **Is_singleton** Konfiguration-Eigenschaft auf **true**festlegen. Instanz Webaufträge eignen sich für Tasks, nicht sollen oder als gleichzeitig mehrere, Instanzen wie dies, Analyse und ähnliches.
- Um die Auswirkung auf die Leistung der Web-app zu minimieren, sollten Sie Erstellen einer leeren Azure Web App-Instanz in einem neuen App Service-Plan Host Webaufträge, die langlebig oder Ressource intensiv.

### <a name="more-information"></a>Weitere Informationen

- [Azure Webaufträge empfohlene Ressourcen](./app-service-web/websites-webjobs-resources.md) sind viele nützliche Ressourcen, Downloads und Beispiele für Webaufträge aufgeführt.

## <a name="azure-cloud-services-web-and-worker-roles"></a>Azure Cloud Services Web- und Workerrollen-Rollen

Sie können Hintergrundaufgaben in einer Webrolle oder in eine separate Worker-Rolle ausführen. Wenn Sie, ob eine Worker-Rolle verwenden entscheiden, sollten skalierbar und Elastizität Anforderungen, Aufgabe Lebensdauer, freigeben Sie Trittfrequenz, Sicherheit, Fehlertoleranz, Konflikte, Komplexität und der logischen Architektur. Weitere Informationen finden Sie unter [Berechnen Ressource Konsolidierung Muster](http://msdn.microsoft.com/library/dn589778.aspx).

Es gibt verschiedene Wege zur Implementierung des Hintergrundaufgaben in einer Cloud-Services-Rolle:

- Erstellen Sie eine Implementierung der **RoleEntryPoint** -Klasse in der Rolle und Methoden Sie seine Hintergrundaufgaben ausführen. Die Aufgaben im Kontext des WaIISHost.exe ausgeführt. **GetSetting** -Methode der **CloudConfigurationManager** -Klasse können sie Konfigurationsdateien geladen. Weitere Informationen finden Sie unter [Lebenszyklus (Cloud Services)](#lifecycle-cloud-services).
- Verwenden Sie Startaufgaben Hintergrundaufgaben ausführen beim Start der Anwendung. Erzwingen Sie die Vorgänge im Hintergrund weiterhin ausgeführt **TaskType** -Eigenschaft auf **Hintergrund** festgelegt (Wenn Sie dies nicht tun, des Startvorgangs Anwendung angehalten und der Aufgabe zu warten). Weitere Informationen finden Sie unter [Ausführen von Aufgaben in Azure](./cloud-services/cloud-services-startup-tasks.md).
- Mithilfe des WebJobs-SDK Hintergrundaufgaben wie Webaufträge implementieren, die als Start-Vorgang initiiert werden. Weitere Informationen finden Sie unter [Erste Schritte mit Azure Webaufträge SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md).
- Verwenden einer Startaufgabe an um einen Windows-Dienst zu installieren, der mindestens Hintergrundaufgaben ausführt. Die **TaskType** -Eigenschaft müssen **Hintergrund** festlegen, damit der Dienst im Hintergrund ausgeführt wird. Weitere Informationen finden Sie unter [Ausführen von Aufgaben in Azure](./cloud-services/cloud-services-startup-tasks.md).

### <a name="running-background-tasks-in-the-web-role"></a>Hintergrund-Tasks in der Webrolle ausgeführt

Der Hauptvorteil von Hintergrundaufgaben in der Webrolle ausgeführt wird im Hosting-Kosten, da keine Notwendigkeit zum Bereitstellen zusätzlicher Funktionen besteht speichern.

### <a name="running-background-tasks-in-a-worker-role"></a>Ausführen von Hintergrundaufgaben in eine Worker-Rolle

Hintergrundaufgaben in eine Worker-Rolle ausgeführt hat mehrere Vorteile:

- Sie können Sie für jede Rolle einzeln skalieren zu verwalten. Beispielsweise müssen Sie möglicherweise mehrere Instanzen der Webrolle für die aktuelle Last jedoch weniger Instanzen der Rolle der Arbeitskraft, die Hintergrundaufgaben ausführt. Durch Skalierung Hintergrund Aufgabe Serverinstanzen getrennt von UI-Funktionen können Sie akzeptable Leistung Hosting-Kosten reduzieren.
- Der Verarbeitungsaufwand für Hintergrundaufgaben in der Webrolle verlagert. Die Web-Rolle, die die Benutzeroberfläche reaktionsfähig bleiben kann, und kann bedeuten, dass weniger Instanzen einer bestimmten Anfragen von Benutzern unterstützen müssen.
- Es ermöglicht die Trennung von Bereichen implementiert. Jede Rolle kann einen bestimmten Satz von klar definierten und Verwandte Aufgaben implementieren. Dadurch entwerfen und pflegen des Codes vorhanden ist weniger Abhängigkeit von Code und zwischen jeder Rolle.
- Sie können um vertrauliche Prozesse und Daten zu isolieren. Web-Rollen, die die Benutzeroberfläche implementieren müssen beispielsweise nicht auf Daten zugreifen, die verwaltet und gesteuert durch eine Worker-Rolle. Dies ist insbesondere für die Stärkung der Sicherheit, insbesondere wenn Sie ein Muster wie [Gatekeeper Muster](http://msdn.microsoft.com/library/dn589793.aspx)verwenden.  

### <a name="considerations"></a>Hinweise

Die folgenden Punkte berücksichtigen, wie und wo beim Verwenden von Clouddiensten Web- und Workerrollen Hintergrundaufgaben bereitstellen:

- Hosten von Hintergrundaufgaben in einer vorhandenen Webrolle kann die Kosten für eine separate Worker-Rolle für diese Vorgänge speichern. Es ist jedoch die Leistung und Verfügbarkeit der Anwendung beeinträchtigen, wenn und andere Ressourcen Konflikte. Mit einer separaten Arbeiter Rolle schützt die Webrolle vor langer oder ressourcenintensiven Hintergrundaufgaben.
- Wenn Sie mithilfe der **RoleEntryPoint** -Klasse Hintergrundaufgaben hosten, können Sie dies problemlos zu einer anderen Rolle verschieben. Wenn die Klasse in einer Webrolle erstellen und später entscheiden, dass Sie zum Ausführen der Aufgaben in eine Worker-Rolle können Sie die Implementierung der **RoleEntryPoint** -Klasse in Worker-Rolle verschieben.
- Aufgaben sollen ein Programm oder ein Skript ausführen. Ein Hintergrundauftrag als ausführbares Programm bereitstellen ist möglicherweise schwieriger, besonders wenn es auch abhängige Assemblys bereitgestellt erfordert. Möglicherweise ist es einfacher Bereitstellung und ein Skript Hintergrund definieren, wenn Sie Aufgaben verwenden.
- Ausnahmen, die eine Hintergrundaufgabe fehlschlagen verursachen wirken unterschiedliche, je nachdem wie sie gehostet werden:
  - Verwenden Sie den **RoleEntryPoint** Ansatz, führen eine fehlgeschlagene Aufgabe Rolle neu starten, damit die Aufgabe automatisch neu gestartet. Dies kann die Verfügbarkeit der Anwendung auswirken. Um dies zu verhindern, daß stabile Ausnahmebehandlung innerhalb der **RoleEntryPoint** -Klasse und alle Hintergrundaufgaben enthalten. Code verwenden, um Aufgaben neu starten, die nicht das entsprechende ist und der Ausnahme, um die Rolle neu starten, wenn Sie ordnungsgemäß im Code den Fehler beheben können.
  - Verwenden Sie Aufgaben, sind Sie verantwortlich für die Verwaltung der Taskausführung und überprüft, ob es nicht.
- Verwalten und Überwachen von Startaufgaben ist schwieriger als die **RoleEntryPoint** -Klasse Ansatz. Azure Webaufträge SDK enthält jedoch ein Dashboard zu über Aufgaben starten Webaufträge erleichtern.

### <a name="more-information"></a>Weitere Informationen

- [Konsolidierung Muster Ressource berechnen](http://msdn.microsoft.com/library/dn589778.aspx)
- [Erste Schritte mit Azure Webaufträge SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)

## <a name="azure-virtual-machines"></a>Azure Virtual Machines

Hintergrundaufgaben implementiert werden könnte, auf eine Weise, die verhindert, dass sie in Azure Web Apps oder Cloud-Dienste bereitgestellt, oder diese Optionen möglicherweise nicht praktisch. Beispiele sind Windows-Dienste und Hilfsprogramme und ausführbare Programme. Ein weiteres Beispiel möglicherweise Programme für eine ausführungsumgebung, die mit der Anwendung unterscheidet. Z. B. einem UNIX- oder Linux-Programm möglicherweise, die von einer Anwendung für Windows oder .NET ausgeführt werden soll. Sie können eine Auswahl der Betriebssysteme für Azure Virtual Machine und Ihre oder ausführbare Datei auf dem virtuellen Computer führen.

Verwendung von virtuellen Maschinen Auswahl finden Sie [Azure App, Cloud und virtuellen Maschinen Vergleich](./app-service-web/choose-web-site-cloud-service-vm.md). Informationen zu den Optionen für virtuelle Maschinen finden Sie unter [Größe für Azure Virtual Machine und Cloud-Dienst](http://msdn.microsoft.com/library/azure/dn197896.aspx). Weitere Informationen zu Betriebssystemen und vordefinierte Bilder für virtuelle Computer finden Sie unter [Azure VMs Marketplace](https://azure.microsoft.com/gallery/virtual-machines/).

Um Hintergrundtask in einen virtuellen Computer zu starten, müssen Sie eine Reihe von Optionen:

- Sie führen Aufgabe bei Bedarf direkt aus Ihrer Anwendung durch Senden einer Anforderung an einen Endpunkt, den die Aufgabe verfügt. Dies wird den Vorgang Daten. Dieser Endpunkt Ruft die Aufgabe.
- Sie können die Aufgabe mithilfe eines Planers oder Zeitgeber, die in das ausgewählte Betriebssystem nach einem Zeitplan ausgeführt. Z. B. unter Windows können Windows-Taskplaner Sie Skripts und Tasks ausführen. Oder wenn Sie SQL Server auf dem virtuellen Computer installiert haben, können Sie der SQL Server-Agent zum Ausführen von Skripts und Tasks.
- Azure Scheduler können Sie initiieren die Aufgabe durch Hinzufügen einer Nachricht an eine Warteschlange, der die Aufgabe überwacht oder durch Senden einer Anforderung an eine API, die die Aufgabe verfügbar macht.

Siehe den obigen Abschnitt [Trigger](#triggers) Weitere Informationen, wie Sie Hintergrundaufgaben einleiten können.  

### <a name="considerations"></a>Hinweise

Beachten Sie Folgendes bei der Entscheidung, ob Hintergrundaufgaben in Azure VM bereitgestellt:

- Hosting Hintergrundaufgaben in einem Azure virtuellen Computer ermöglicht Flexibilität und Kontrolle über die Einleitung, Durchführung Planung und Zuweisung. Allerdings wird sie Laufzeitkosten erhöhen, wenn ein virtuellen Computer bereitgestellt werden müssen, um Hintergrundaufgaben ausführen.
- Es gibt keine Möglichkeit, den grundlegenden Status des virtuellen Computers überwachen und Verwalten mithilfe von [Azure Service Management Cmdlets](http://msdn.microsoft.com/library/azure/dn495240.aspx)in Azure-Portal und keine automatischen Neustart-Funktion für fehlgeschlagene Aufgaben – Aufgaben überwachen. Es gibt jedoch keine Funktionen zur Steuerung von Prozessen und Threads im Compute-Knoten. In der Regel erfordert mit einem virtuellen Computer zusätzlichen Aufwand einen Mechanismus implementieren, der Daten aus der Instrumentation in der Aufgabe und vom Betriebssystem des virtuellen Computers erfasst. Eine Lösung sollte ist das [System Center Management Pack für Azure](http://technet.microsoft.com/library/gg276383.aspx)verwenden.
- Erstellen von Überwachung Prüfpunkte über HTTP-Endpunkte verfügbar gemacht werden sollten. Der Code für diese Prüfpunkte konnte Gesundheit überprüfen sammeln Informationen und Statistiken – oder Informationen sammeln und an eine Anwendung zurückgeben. Weitere Informationen finden Sie unter [Health Endpunkt Überwachung Muster](http://msdn.microsoft.com/library/dn589789.aspx).

### <a name="more-information"></a>Weitere Informationen

- [Virtuelle Computer](https://azure.microsoft.com/services/virtual-machines/) in Azure
- [Häufig gestellte Fragen zur Azure Virtual Machines](virtual-machines/virtual-machines-linux-classic-faq.md)

## <a name="design-considerations"></a>Vorüberlegungen

Es gibt mehrere grundlegende Faktoren beim Entwerfen von Hintergrundaufgaben. Abschnitten Partitionierung, Konflikte und koordinieren.

## <a name="partitioning"></a>Partitionierung

Möchten Sie Hintergrundaufgaben in einer vorhandenen Serverinstanz (z. B. ein WebApp, Webrolle, vorhandene Worker-Rolle oder virtuellen) enthalten, müssen Sie berücksichtigen, wie Qualitätsattribute die Serverinstanz und Hintergrundtask sich dadurch. Diese Faktoren helfen Ihnen zu entscheiden, ob die Vorgänge mit vorhandenen Serverinstanz zu installieren oder in einer separaten Serverinstanz trennen:

- **Verfügbarkeit**: Hintergrundaufgaben nicht müsste die gleiche Verfügbarkeit wie andere Teile der Anwendung, insbesondere die Benutzeroberfläche und andere Teile, die Benutzerinteraktion beteiligt sind. Hintergrund-Tasks möglicherweise toleranter Wartezeit wiederholt Verbindungsfehler und andere Faktoren die Verfügbarkeit beeinflussen, da die in der Warteschlange gespeichert werden können. Es muss jedoch ausreichend Kapazität für die Sicherung der Anfragen zu verhindern, die Warteschlangen blockiert und die Anwendung als Ganzes auswirken.
- **Skalierbar**: Hintergrundaufgaben sind wahrscheinlich unterschiedliche Skalierbarkeit erforderlich als die Benutzeroberfläche und die interaktiven Teile der Anwendung. Skalieren der Benutzeroberfläche möglicherweise zu Spitzen, während Zeiten mit weniger Datenverkehr durch weniger ausstehenden Hintergrundaufgaben abgeschlossen werden können der Serverinstanzen.
- **Flexibilität**: Ausfall einer Serverinstanz, die nur Hintergrundaufgaben hostet nicht tödlich wirkt die Anwendung als Ganzes Anfragen für diese Aufgaben können in der Warteschlange oder zurückgestellt, bis die Aufgabe wieder verfügbar ist. Falls Serverinstanz oder Aufgaben in einem angemessenen Zeitraum gestartet werden, können Benutzer der Anwendung nicht betroffen.
- **Sicherheit**: Hintergrundaufgaben möglicherweise unterschiedliche oder Einschränkungen der Benutzeroberfläche oder andere Teile der Anwendung. Geben Sie mithilfe einer separaten Serverinstanz eine anderen Umgebung für Aufgaben. Muster wie Gatekeeper können Sie um Serverinstanzen Hintergrund von der Benutzeroberfläche zu isolieren, um maximale Sicherheit und Trennung.
- **Leistung**: welcher Serverinstanz können Hintergrund zu speziell Leistungsbedarf Aufgaben Aufgaben. Dies bedeutet möglicherweise kostengünstiger Berechnungsoption Aufgaben nicht benötigen der Benutzeroberfläche oder eine größere Instanz derselben Verarbeitungsfunktionen benötigen sie zusätzliche Kapazität und Ressourcen verwenden.
- **Verwaltung**: Hintergrundaufgaben möglicherweise einen anderen Rhythmus Entwicklung und Bereitstellung der Anwendungscode oder der Benutzeroberfläche. Bereitstellen in einer separaten Serverinstanz kann Updates und Versionskontrolle vereinfachen.
- **Kosten**: Instanzen auszuführende Hintergrund Aufgaben erhöht Hostingkosten berechnen hinzufügen. Sie sollten sorgfältig abgewogen zusätzliche Kapazität und zusätzlichen Kosten.

Weitere Informationen finden Sie unter [Füllzeichen Wahl](http://msdn.microsoft.com/library/dn568104.aspx) und [Konkurrierenden Verbraucher Muster](http://msdn.microsoft.com/library/dn568101.aspx).

## <a name="conflicts"></a>Konflikte

Haben Sie mehrere Instanzen von Hintergrund kann für den Zugriff auf Ressourcen und Dienste wie Datenbanken und Speicher konkurrieren. Gleichzeitigen Zugriff kann Konflikte bei der Verfügbarkeit der Dienste und der Integrität der Daten im Speicher möglicherweise Ressourcenkonflikte, führen. Ressourcenkonflikte lösen mit pessimistische Sperren Ansatz. Dadurch konkurrierende Instanzen einer Aufgabe gleichzeitig auf einen Dienst zugreifen oder Daten zu beschädigen.

Ein anderer Ansatz zum Lösen von Konflikten ist Hintergrundaufgaben als Singleton, definieren, sodass immer nur eine Instanz ausgeführt. Aber dadurch Zuverlässigkeit und Performance-Vorteilen, die mehrere Instanzkonfiguration bereitstellen kann. Dies ist besonders bei die Benutzeroberfläche ausreichend Arbeit beschäftigt mehr als eine Hintergrundaufgabe angeben kann.

Es ist wichtig sicherzustellen, dass die Hintergrundaufgabe automatisch neu starten kann und hat ausreichend Kapazität für Spitzen bewältigen. Dazu können Sie eine Serverinstanz über ausreichende Ressourcen zuordnen, durch die Implementierung einer Warteschlangenmechanismus, das Speichern von Anfragen für die spätere Ausführung bei Bedarf verringert oder eine Kombination dieser Techniken.

## <a name="coordination"></a>Koordinierung

Hintergrund-Tasks möglicherweise komplexe und möglicherweise mehrere einzelne Aufgaben ein Ergebnis oder alle erfüllen. Es ist üblich, in diesen Fällen Teilen Sie die Aufgabe in kleinere einzelne Schritte oder Teilvorgänge von mehreren Consumern ausgeführt werden können. Aufträge mit mehreren Schritten kann effizienter und flexibler, einzelne Schritte möglicherweise in mehreren Projekten wiederverwendet werden. Es ist auch einfach hinzufügen, entfernen oder die Reihenfolge der Schritte ändern.

Koordination mehrerer Aufgaben und Schritte kann schwierig sein, aber es gibt drei gängige Muster, mit dem Sie die Umsetzung einer Lösung:

- **Eine Aufgabe in mehrere verwendbare Schritte zerlegen**. Eine Anwendung erforderlich Ausführen verschiedenster Aufgaben der unterschiedlichen Komplexität der Informationen verarbeitet. Einfacher, aber unflexibel Ansatz zum Implementieren dieser Anwendung möglicherweise diese Verarbeitung monolithischen Modul. Dieser Ansatz ist jedoch zu Verkaufschancen für den Code Umgestalten, optimieren oder Wiederverwendung derselben Verarbeitung an anderer Stelle in der Anwendung erforderlich sind. Weitere Informationen finden Sie unter [Pipes und Filter](http://msdn.microsoft.com/library/dn568100.aspx).
- **Die Schritte für einen Vorgang verwalten**. Eine Anwendung kann Aufgaben ausführen, die mehrere Schritte umfassen (die Remotedienste aufrufen oder Remoteressourcen zugreifen). Die einzelnen Schritte möglicherweise unabhängig voneinander, aber sie orchestriert von der Anwendungslogik, die die Aufgabe implementiert. Weitere Informationen finden Sie im [Planer Agent Supervisor Muster](http://msdn.microsoft.com/library/dn589780.aspx).
- **Verwaltung Wiederherstellung Aufgabe Schritte, die fehlschlagen**. Eine Anwendung müssen die rückgängig zu machen, die eine Reihe von Schritten ausgeführt wird (die schließlich konsistenten Betrieb zusammen definieren) Wenn eine oder mehrere der Schritte fehlschlagen. Weitere Informationen finden Sie unter [Kompensierende Transaktion Muster](http://msdn.microsoft.com/library/dn589804.aspx).

## <a name="lifecycle-cloud-services"></a>Lifecycle (Cloud Services)

 Wenn Sie Hintergrundjobs für Cloud-Services-Anwendung implementieren, die Web-und Workerrollen mithilfe der **RoleEntryPoint** -Klasse, unbedingt den Lebenszyklus dieser Klasse verstehen, um ordnungsgemäß verwendet werden.

Web-und Workerrollen durchlaufen unterschiedliche Phasen starten, ausführen und beenden. Die **RoleEntryPoint** -Klasse stellt eine Reihe von Ereignissen, die beim Auftreten dieser Phasen an. Sie verwenden diese Initialisierung ausführen und Ihre benutzerdefinierten Hintergrund-Tasks beendet. Abgeschlossen ist:

- Azure lädt die Assembly Rolle und eine von **RoleEntryPoint**abgeleitete Klasse gesucht.
- Wenn diese Klasse gefunden wird, wird **RoleEntryPoint.OnStart()**aufgerufen. Sie überschreiben diese Methode zum Initialisieren der Hintergrundaufgaben.
- Nach Abschluss die **OnStart** -Methode stellen Azure Aufrufe **Application_Start()** in der Anwendung Globaldatei Wenn ist (z. B. Global.asax in einer Webrolle ausgeführt ASP.NET).
- Azure ruft **RoleEntryPoint.Run()** für neue Vordergrundthread, der mit **OnStart()**ausgeführt wird. Sie überschreiben diese Methode zum Starten der Hintergrundaufgaben.
- Beendigung die Run-Methode ruft Azure **Application_End()** zuerst in globalen Anwendungsdatei, wenn dieser vorhanden ist, und dann **RoleEntryPoint.OnStop() Ruft**. Sie überschreiben die **OnStop** -Methode stop die Hintergrundaufgaben Ressourcen bereinigen, Objekte entfernen und Schließen von Verbindungen Aufgaben verwendet haben.
- Der Azure-Rolle Host Arbeitsprozess wurde beendet. Zu diesem Zeitpunkt die Rolle wiederverwendet werden und wird neu gestartet.

Weitere Informationen und ein Beispiel für Methoden der **RoleEntryPoint** -Klasse finden Sie unter [Berechnen Ressource Konsolidierung Muster](http://msdn.microsoft.com/library/dn589778.aspx).

## <a name="considerations"></a>Hinweise

Beachten Sie Folgendes, wenn Sie planen, wie Hintergrundaufgaben in einer Web- oder Worker-Rolle ausgeführt wird:

- Standardmäßige **Ausführen** methodenimplementierung der **RoleEntryPoint** -Klasse enthält einen Aufruf an **Thread.Sleep(Timeout.Infinite)** , die die Rolle auf unbestimmte Zeit aktiv bleibt. Wenn Sie **Run** -Methode überschreiben (normalerweise Hintergrundaufgaben ausführen erforderlich ist), müssen Sie nicht Code die Methode zu beenden, wenn Sie die Instanz der Rolle wiederverwenden möchten zulassen.
- Normale Implementierung der **Run** -Methode enthält Code zum Starten der Hintergrundaufgaben und eine Schleife, die den Status aller Hintergrundaufgaben in regelmäßigen Abständen überprüft. Sie können eine neu starten, die fehlschlagen oder überwachen Abbruchtoken, die angeben, dass Aufträge abgeschlossen haben.
- Wenn eine Hintergrundaufgabe eine nicht behandelte Ausnahme auslöst, sollte diese Aufgabe wiederverwendet gleichzeitig anderen Hintergrundaktivitäten Rolle weiter. Jedoch wenn die Ausnahme durch eine beschädigte Objekte außerhalb der Aufgabe freigegebenen Speicher verursacht wird die Ausnahme behandelt werden soll, durch die **RoleEntryPoint** -Klasse alle Aufgaben abgebrochen werden soll und die **Run** -Methode darf am Ende. Azure wird die Rolle starten.
- Verwenden Sie die **OnStop** -Methode zu unterbrechen oder Hintergrundaufgaben Ressourcen bereinigen. Dies kann lange andauernden oder mehrstufige Aufgaben beenden. Es ist wichtig zu beachten, wie dies erreicht werden kann, um Dateninkonsistenzen zu vermeiden. Hält eine Rolleninstanz aus irgendeinem Grund als Benutzer initiiertes Herunterfahren, in die **OnStop** -Methode ausgeführten Code Vordrucks innerhalb von fünf Minuten, bevor er abgebrochen wird. Sicherstellen Sie, dass Code in dieser Zeitspanne abgeschlossen werden toleriert nicht ausgeführt.  
- Azure Lastenausgleich startet den Verkehr auf die Instanz der Rolle bei die **RoleEntryPoint.OnStart** -Methode gibt den Wert **true**zurück. Deshalb Ihren Initialisierungscode in der **OnStart** -Methode einfügen, damit nicht erfolgreich initialisieren Rolleninstanzen Datenverkehr nicht erhalten.
- Sie können Aufgaben neben der **RoleEntryPoint** -Klasse. Verwenden Sie Startaufgaben Einstellungen initialisieren, die Sie in Azure Lastenausgleich nicht ändern, da diese Aufgaben ausführt, bevor die Rolle Anfragen erhält. Weitere Informationen finden Sie unter [Ausführen von Aufgaben in Azure](./cloud-services/cloud-services-startup-tasks.md).
- Liegt ein Fehler in einer Startaufgabe, möglicherweise die Rolle ständig neu starten erzwingen. Dies kann verhindern Sie eine virtuelle IP-Adresse (VIP) Adresse Swap auf eine zuvor bereitgestellte Version da Swap exklusiven Zugriff auf die Rolle erfordert. Dies konnte nicht abgefragt werden, während die Rolle neu gestartet wurde. Um dieses Problem zu beheben:
    -  Fügen Sie den folgenden Code am Anfang der Methoden **OnStart** und **Ausführen** Ihrer Rolle:

    ```C#
    var freeze = CloudConfigurationManager.GetSetting("Freeze");
    if (freeze != null)
    {
        if (Boolean.Parse(freeze))
        {
            Thread.Sleep(System.Threading.Timeout.Infinite);
        }
    }
    ```

   - Fügen Sie die Definition der Einstellung **fixieren** als boolescher Wert ServiceDefinition.csdef und ServiceConfiguration. *.cscfg Dateien für die Rolle und* *false* *. Wenn die Rolle in wiederholten Neustartmodus wechselt, können Sie die Einstellung ändern * *true** Rolle Ausführung fixieren und mit einer früheren Version ausgetauscht werden.

## <a name="resiliency-considerations"></a>Aspekte der Stabilität

Hintergrund-Tasks muss stabil um zuverlässige Dienste der Anwendung bereitzustellen. Beachten Sie beim Planen und Entwerfen von Hintergrundaufgaben Folgendes:

- Hintergrundaufgaben muss Rolle oder einen Dienst startet ohne Daten oder Inkonsistenzen in die Anwendung ordnungsgemäß verarbeiten. Langer oder mehrstufigen Aufgaben verwenden Sie _Überprüfen zeigen_ den Status von Aufträgen im permanenten Speicher und Nachrichten in einer Warteschlange speichern, wenn dies ist. Sie können z. B. speichern Informationen in einer Nachricht in einer Warteschlange und Zustandsinformationen mit den Vorgangsfortschritt inkrementell aktualisieren, damit die Aufgabe vom letzten bekannten guten Prüfpunkt - statt völlig neu verarbeitet werden kann. Bei Azure Service Bus-Warteschlangen können Nachrichten Sessions Sie dasselbe Szenario aktivieren. Sessions können Sie speichern und Abrufen des Verarbeitungsstatus Anwendung Methoden [SetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.setstate.aspx) und [GetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.getstate.aspx) . Weitere Informationen zum Entwerfen von zuverlässigen mehrstufige Prozesse und Workflows finden Sie unter [Planer Agent Supervisor Muster](http://msdn.microsoft.com/library/dn589780.aspx).
- Wenn Sie Web-oder Workerrollen Hosten mehrerer Hintergrundaufgaben verwenden, Entwerfen Sie Überschreiben der **Run** -Methode für fehlgeschlagene oder angehaltene Aufgaben überwachen und Neustarten. Dies ist nicht praktisch und dabei eine Worker-Rolle erzwingen Sie Worker-Rolle zu starten, indem Sie die **Run** -Methode beenden
- Warteschlangen Verwendung zur Kommunikation mit Hintergrundaufgaben, die Warteschlangen als Puffer zum Speichern von Anfragen an die Aufgaben beim Ausführen der Anwendung werden unter üblichen Auslastung höher ist. Dadurch werden die Aufgaben der Benutzeroberfläche weniger Stoßzeiten aufzuholen. Es bedeutet auch, dass Wiederverwendung der Rolle die Benutzeroberfläche nicht blockiert. Weitere Informationen finden Sie in der [Queue-basierten Auslastungsmuster Kapazitätsabgleich](http://msdn.microsoft.com/library/dn589783.aspx). Sollten Sie einige Aufgaben wichtiger als andere sind, implementieren die [Priorität Warteschlange Muster](http://msdn.microsoft.com/library/dn589794.aspx) um sicherzustellen, dass diese Aufgaben vor weniger wichtigen ausgeführt.
- Hintergrundaufgaben oder Prozess Nachrichten eingeleitet müssen konzipiert Inkonsistenzen Nachrichten angeordnet, Nachrichten, die wiederholt Fehler (oft als _nicht verarbeitbare Nachrichten_) führen, wie Nachrichten mehr als einmal übermittelt. Beachten Sie Folgendes:
  - Nachrichten, die möglicherweise nicht in der ursprünglichen Reihenfolge, in der sie gesendet wurden, Eintreffen verarbeitet werden müssen, in einer bestimmten Reihenfolge, wie die Daten basierend auf den vorhandenen Datenwert (z. B. Hinzufügen eines Werts zu einem vorhandenen Wert) ändern. Auch kann von anderen Instanzen in einer anderen Reihenfolge unterschiedliche Lasten für jede Instanz eines Hintergrundtasks behandelt werden. Nachrichten, die in einer bestimmten Reihenfolge verarbeitet werden sollen, eine Sequenznummer, Schlüssel oder einige andere Indikator Hintergrundaufgaben verwenden können, um sicherzustellen, dass sie in der richtigen Reihenfolge verarbeitet werden. Bei Verwendung von Azure Service Bus können Nachricht Sessions Sie um die Reihenfolge der Bereitstellung zu gewährleisten. Es ist jedoch effizienter, wenn möglich, den Prozess so entwerfen, dass die Reihenfolge der Nachrichten nicht wichtig ist.
  - Normalerweise wird eine Hintergrundaufgabe Nachrichten in der Warteschlange einsehen der vorübergehend von anderen Verbrauchern Nachricht ausgeblendet. Anschließend löscht Nachrichten, nachdem sie erfolgreich verarbeitet wurde. Ausfall ein Hintergrundtasks beim Verarbeiten einer Nachricht wird die Nachricht in der Warteschlange wieder angezeigt, nach Ablauf des Timeouts einsehen. Es wird von einer anderen Instanz der Aufgabe oder bei der nächsten Verarbeitung dieser Instanz verarbeitet. Wenn die Meldung Fehler konsistent im Consumer verursacht, blockiert es die Aufgabe der Warteschlange und schließlich die Anwendung selbst, wenn die Warteschlange voll ist. Daher ist es wichtig zu erkennen und nicht verarbeitbare Nachrichten aus der Warteschlange entfernt. Bei Verwendung von Azure Service Bus können Nachrichten, die Fehler verursachen automatisch oder manuell an eine zugeordnete Dead Letter-Warteschlange verschoben werden.
  - Warteschlangen werden _einmal_ Mechanismen gewährleistet, jedoch können sie dieselbe Nachricht mehrmals liefern. Darüber hinaus schlägt eine Hintergrundaufgabe nach der Verarbeitung einer Nachricht, aber bevor Sie aus der Warteschlange löschen, wird Nachricht zur Verarbeitung wieder verfügbar. Hintergrundaufgaben sollte Idempotent, was bedeutet, dass die gleiche Nachricht mehrmals verarbeitet keinen Fehler oder eine Inkonsistenz in den Anwendungsdaten. Einige Vorgänge sind natürlich Idempotent wie einen gespeicherten Wert mit einem bestimmten neuen Wert. Allerdings werden wie ein bestehender gespeicherter Wert einen Wert hinzufügen, ohne zu überprüfen, dass der gespeicherte Wert immer noch dasselbe wie wenn die Nachricht ursprünglich gesendet wurde zu Inkonsistenzen führen. Azure Service Bus-Warteschlangen können konfiguriert werden, um doppelte Nachrichten automatisch zu entfernen.
  - Einige Messagingsysteme wie Azure-Speicher und Azure Service Bus-Warteschlangen unterstützen De-queue Count-Eigenschaft, der die Anzahl angibt, eine Nachricht aus der Warteschlange gelesen wurde. Dies kann mit wiederholten und nicht verarbeitbare Nachrichten hilfreich sein. Weitere Informationen finden Sie unter [Asynchrones Messaging Primer](http://msdn.microsoft.com/library/dn589781.aspx) und [Idempotenz Muster](http://blog.jonathanoliver.com/2010/04/idempotency-patterns/).

## <a name="scaling-and-performance-considerations"></a>Skalierung und Leistungsfähigkeit

Hintergrundaufgaben müssen ausreichend Leistung, um sicherzustellen, dass sie nicht die Anwendung zu blockieren, oder Inkonsistenzen aufgrund der verzögerten Vorgang, wenn das System unter Last anbieten. In der Regel ist skalieren Serverinstanzen, die Hintergrundaufgaben verbessert. Beim Planen und Entwerfen von Hintergrundaufgaben Folgendes Skalierbarkeits-und Performance:

- Azure unterstützt automatische Skalierung (scaling-out und Skalierung in) auf Nachfrage - basierte oder nach einem vordefinierten Zeitplan für Web-Apps Clouddienste Web- und Worker-Rollen und virtuelle Computer gehostete Bereitstellung. Verwenden Sie diese Funktion, um sicherzustellen, dass die Anwendung als Ganzes ausreichende Leistungsfähigkeit während der Laufzeit Kosten.
- Bei Hintergrundaufgaben unterschiedliche Leistungsfähigkeit von anderen Teilen einer Anwendung Cloud-Dienste (z. B. UI oder Komponenten wie die Datenzugriffsschicht) ermöglicht hosting Hintergrundaufgaben in eine separate Worker-Rolle der Benutzeroberfläche und der Hintergrund Aufgabe unabhängig voneinander zu skalieren der Auslastung zu. Sollten Sie mehrere Hintergrundaufgaben Leistungsfähigkeit erheblich voneinander haben, in separaten Workerthreads unterteilen und jeder Rollentyp unabhängig voneinander skalieren. Beachten Sie jedoch, dass diese Laufzeit Kosten kombinieren aller Aufgaben weniger Rollen zunehmen.
- Einfach skalieren Rollen Leistungsverlust Belastung verhindern möglicherweise nicht. Möglicherweise müssen auch speicherwarteschlangen und andere Ressourcen zu einen einzelnen Punkt des gesamten Prozesskette nicht zum Engpass. Bedenken Sie auch, wie der maximale Durchsatz Speicher und andere Dienste, die die Anwendung und die Hintergrund-Tasks auf andere Einschränkungen.
- Hintergrundaufgaben müssen für die Skalierung konzipiert. Beispielsweise werden sie die Anzahl der speicherwarteschlangen dynamisch mit Abhören oder Senden von Nachrichten an die entsprechende Warteschlange erkennen.
- Standardmäßig skalieren Webaufträge mit ihren zugeordneten Azure Web Apps. Jedoch wenn Webauftrags nur eine Instanz ausgeführt werden soll, können Sie erstellen eine Settings.job-Datei, die JSON-Daten enthält **{"Is_singleton": true}**. Dies erzwingt Azure nur eine Instanz der Webauftrag ausführen, auch wenn mehrere der zugeordneten Anwendung Instanzen. Dies ist eine nützliche Technik für geplante Aufträge, die als eine einzelne Instanz ausführen müssen.

## <a name="related-patterns"></a>Verwandte Muster

- [Asynchrones Messaging-Einführung](http://msdn.microsoft.com/library/dn589781.aspx)
- [Automatische Skalierung Anleitung](http://msdn.microsoft.com/library/dn589774.aspx)
- [Kompensierende Transaktion Muster](http://msdn.microsoft.com/library/dn589804.aspx)
- [Konkurrierende Consumer-Muster](http://msdn.microsoft.com/library/dn568101.aspx)
- [Berechnen Sie Partitionierung Anleitung](http://msdn.microsoft.com/library/dn589773.aspx)
- [Konsolidierung Muster Ressource berechnen](http://msdn.microsoft.com/library/dn589778.aspx)
- [Gatekeeper-Muster](http://msdn.microsoft.com/library/dn589793.aspx)
- [Hinweislinie Wahl Muster](http://msdn.microsoft.com/library/dn568104.aspx)
- [Pipes und Filter-Muster](http://msdn.microsoft.com/library/dn568100.aspx)
- [Priorität Warteschlange Muster](http://msdn.microsoft.com/library/dn589794.aspx)
- [Queue-basierten laden Abgleich Muster](http://msdn.microsoft.com/library/dn589783.aspx)
- [Planer-Agent Supervisor Muster](http://msdn.microsoft.com/library/dn589780.aspx)

## <a name="more-information"></a>Weitere Informationen

- [Skalierung von Azure Applications with Worker-Rollen](http://msdn.microsoft.com/library/hh534484.aspx#sec8)
- [Hintergrundaufgaben ausführen](http://msdn.microsoft.com/library/ff803365.aspx)
- [Start-Lebenszyklus Azure-Rolle](http://blog.syntaxc4.net/post/2011/04/13/windows-azure-role-startup-life-cycle.aspx) (Blogbeitrag)
- [Azure Cloud Services Rolle Lebenszyklus](http://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Windows-Azure-Cloud-Services-Role-Lifecycle) (Video)
- [Erste Schritte mit Azure Webaufträge SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- [Azure und Service Bus Warteschlangen - verglichen und gegenübergestellt](./service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Aktivieren der Diagnose in der Cloud-Dienst](./cloud-services/cloud-services-dotnet-diagnostics.md)
