<properties
    pageTitle="Debuggen von Azure in Eclipse"
    description="Lernen Sie mit dem Azure-Toolkit für Eclipse Debugging Azure Applications."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690949.aspx -->

# <a name="debugging-azure-applications-in-eclipse"></a>Debuggen von Azure in Eclipse #

Azure-Toolkit für Eclipse verwenden, können Sie die Anwendung debuggen, ob sie in Azure oder Serveremulator ausgeführt werden, wenn Sie Windows als Betriebssystem verwenden. Die folgende Abbildung zeigt die Eigenschaftenseite **Debuggen** der Dialogfenster zum Remotedebuggen verwendet:

![][ic719504]

In diesem Lernprogramm wird davon ausgegangen, dass Sie bereits eine Anwendung, die erfolgreich erstellt wurde und mit der Serveremulator und Bereitstellen von Azure.

Wir verwenden die Anwendung [mithilfe der Azure-Laufzeitbibliothek JSP][] -Lernprogramm als Ausgangspunkt für dieses Thema. Erstellen Sie bevor Sie fortfahren diese Anwendung Wenn noch nicht geschehen.

## <a name="to-debug-your-application-while-running-in-azure"></a>Zum Debuggen Ihrer Anwendung während der Ausführung in Azure ##

>[AZURE.WARNING] Aktuelle Unterstützung für Java-Debuggen das Toolkit soll in erster Linie für die Bereitstellung in Azure-Serveremulator ausgeführt. Da debugging Verbindung nicht sicher ist, sollten Sie Aktivieren des Remotedebuggens im Praxiseinsatz. Benötigen Sie zum Debuggen einer Anwendung in Azure Cloud, verwenden Sie staging-Bereitstellung, jedoch feststellen Sie, dass nicht autorisierter Zugriff auf die Debugsitzung möglich, wenn jemand die IP-Adresse Ihrer Bereitstellung Cloud ist ist es staging-Bereitstellung.

1. Erstellen Sie das Projekt zum Testen im Emulator: des In Eclipse Projekt-Explorer mit der rechten Maustaste **MyAzureProject**klicken Sie auf **Eigenschaften**, **Azure**und auf Bereitstellung **cloud**festlegen **für erstellen** .
1. Erstellen Sie das Projekt neu: Eclipse-Menü **Projekt**klicken Sie auf **Alle erstellen**.
1. Bereitstellen der Anwendung in Azure *Bereitstellen* .
    >[AZURE.IMPORTANT] Wie oben erwähnt, wird dringend empfohlen, dass Sie Debuggen im Serveremulator meist nur im Bedarfsfall zusätzliche Debuggen wird in die Stagingumgebung Debuggen. Es wird empfohlen, nicht in der Produktion zu debuggen.
1. Nach die Bereitstellung in Azure bereit ist, erhalten Sie den DNS-Namen für die Bereitstellung von [Azure-Verwaltungsportal][]. Staging-Bereitstellung ist ein DNS-Name im Format http://*&lt;Guid&gt;*., cloudapp.net, * &lt;Guid&gt; * ist ein GUID-Wert von Azure zugewiesen.
1. Klicken Sie auf **Debuggen**Sie Eclipse Projekt-Explorer mit der rechten Maustaste **WorkerRole1**und **Azure**auf.
1. Im Dialogfeld **Eigenschaften für WorkerRole1 Debuggen** :
    1. Überprüfen Sie **für diese Rolle Remotedebugging aktivieren.**
    1. Verwenden Sie für **Input-Endpunkt verwendet** **Debuggen (8090: Public, Private: 8090)**.
    1. Sicherstellen Sie, dass **Starten JVM im Standbymodus Debugger Sekunden** deaktiviert ist.
        >[AZURE.IMPORTANT] Die Option **Start JVM im Standbymodus Debugger Sekunden** soll für Debugszenarios im Compute-Emulator (nicht für Cloud-Bereitstellung). Wird die Option **Start JVM im Standbymodus Debugger Sekunden** wird Serverprozess Starten angehalten, bis der JVM Eclipse Debugger verbunden ist. Diese Option für eine Debugsitzung mit Serveremulator verwenden konnte, verwenden Sie es für eine Debugsitzung in einer Cloud-Umgebung. Initialisierung des Servers erfolgt in Azure Startaufgabe und Azure Cloud macht öffentliche Endpunkte verfügbar bis starttask abgeschlossen ist. Daher wird ein Startvorgang nicht erfolgreich abgeschlossen bei aktivierter Option in einer Cloudbereitstellung, da es auf einer Verbindung von einem externen Client Eclipse nicht.
1. Klicken Sie auf **Debugkonfigurationen**.
1. Im Dialogfeld **Konfiguration von Azure Debuggen** :
    1. Wählen Sie für **Java-Projekt Debuggen**das **MyHelloWorld** Projekt.
    1. Überprüfen Sie zum **Debuggen konfigurieren** **Azure Cloud (Bereitstellung)**.
    1. Sicherstellen Sie, dass **Azure compute Emulator** deaktiviert ist.
    1. Geben Sie für **Host**-DNS-Namen der stufenweise Bereitstellung, ohne die vorherigen **http:// ein** Beispiel (verwenden Sie die GUID anstelle der hier gezeigten GUID): **65-6973-526f62657274.cloudapp.net 4e616d65-6f6e-6 d**
1. Klicken Sie auf **OK** , um das Dialogfeld **Azure Debug Configuration** zu schließen.
1. Klicken Sie auf **OK** , um das Dialogfeld **Eigenschaften für WorkerRole1 Debuggen** zu schließen.
1. Wenn Sie einen Haltepunkt im index.jsp bereits festgelegt haben, legen Sie es:
    1. Eclipse Projekt-Explorer erweitern Sie **MyHelloWorld** **Inhaltsordner**, und doppelklicken Sie auf **index.jsp**.
    1. Index.jsp mit der rechten Maustaste in die blaue Leiste links neben den Java-Code und **Ein/aus Haltepunkte**auf, wie im folgenden dargestellt: ![][ic551537]
1. In Eclipse Menü auf **Ausführen** , und klicken Sie auf **Debuggen Konfigurationen**.
1. Im Dialogfeld **Debug Configurations** erweitern Sie **Remoteanwendung Java** im linken Bereich, wählen Sie **Azure Cloud (WorkerRole1)**und klicken Sie auf **Debuggen**.
1. Führen Sie in Ihrem Browser die bereitgestellte Anwendung **http://***&lt;Guid&gt;***.cloudapp.net/MyHelloWorld**, ersetzen die GUID aus dem DNS-Namen für * &lt;Guid&gt;*. Aufforderung durch ein Dialogfeld **Bestätigen Perspektive wechseln** , klicken Sie auf **Ja**. Die Debugsitzung jetzt in der Codezeile ausgeführt wird, in dem der Haltepunkt festgelegt wurde.

>[AZURE.NOTE] Wenn Sie versuchen, zu einem wird Verbindung mit einer Bereitstellung mit mehrere Instanzen ausführen, können nicht Sie die Instanz Steuerelement derzeit Debuggen Debugger zunächst, verbunden als Azure Lastenausgleich eine Instanz zufällig auswählen. Nachdem Sie mit dieser Instanz verbunden sind, aber weiterhin Sie dieselbe Instanz Debuggen. Beachten Sie auch, ist ein Zeitraum der Inaktivität von mehr als 4 Minuten (z. B. Wenn Sie anhalten an einem Haltepunkt zu lang), Azure kann die Verbindung schließen.

## <a name="debugging-a-specific-role-instance-in-a-multi-instance-deployment"></a>Debuggen einer bestimmten Rolleninstanz in einer Bereitstellung mit mehreren Instanzen ##

Wenn die Bereitstellung in der Cloud ausgeführt wird, läuft Sie wahrscheinlich es in mehreren Compute, oder Rolle Instanzen. Dadurch zu Azure 99,95 % garantierte Verfügbarkeit und Skalierung Ihrer Anwendung.

In solchen Szenarios müssen Sie die Java-Anwendung in eine spezifische Rolleninstanz Remote Debuggen. Jedoch aktivieren nur regulären input Endpunkt zum Debuggen machen Azure Lastenausgleich es praktisch unmöglich, spezifische Rolleninstanz den Debugger Verbindung. Stattdessen wird es eine Rolleninstanz herstellen, die nach dem Zufallsprinzip ausgewählt.

Dies ist das Szenario, nutzen Instanz input Endpunkte für eine bestimmte Rolleninstanz Debuggen erleichtert.

Angenommen, Sie bis zu 5 Instanzen der Bereitstellung ausführen möchten. **Endpunkte** Eigenschaftenseite im Dialogfeld Eigenschaften Rolle, erstellen Sie eine Instanz input und zahlreiche öffentliche Ports, anstatt eine einzelne Portnummer zugewiesen. In das Eingabefeld **Öffentliche** beispielsweise **81-85**.

Nach dem Bereitstellen der Anwendung mit dieser Instanz weist Azure einer eindeutige Portnummer aus diesem Bereich zu jeder der Instanzen. Klicken, um herauszufinden, welche Instanz der Anschlussnummer zugewiesen haben, können Sie die Umgebungsvariable *InstanceEndpointName***_PUBLICPORT** (wobei *InstanceEndpointName* der Name beim Erstellen von Instanzendpunkt zugeordnet ist) automatisch konfigurierte Toolkit in der Bereitstellung (z. B. durch seinen Wert in der Fußzeile einer Webseite zurückgibt, damit Sie gelesen werden können, wenn Sie suchen).

Wenn Sie wissen, welche öffentliche Portnummer Instanz zugewiesen wurde, durch Anbringung an den Hostnamen des Dienstes in der Debugkonfiguration in Eclipse verweisen. Dadurch Eclipse Debugger diese bestimmte Instanz und keine anderen Instanzen herstellen.

## <a name="windows-only-to-debug-your-application-while-running-in-the-compute-emulator"></a>Nur Windows: die Anwendung während der Ausführung im Serveremulator Debuggen ##

>[AZURE.NOTE] Azure-Emulator ist nur unter Windows verfügbar. Überspringen Sie diesen Abschnitt, wenn Sie ein Betriebssystem als Windows verwenden. 

1. Erstellen Sie das Projekt zum Testen im Emulator: des In Eclipse Projekt-Explorer mit der rechten Maustaste **MyAzureProject**klicken Sie auf **Eigenschaften**, **Azure**und auf **Testen im Emulator**soll **für erstellen** .
1. Erstellen Sie das Projekt neu: Eclipse-Menü **Projekt**klicken Sie auf **Alle erstellen**.
1. Klicken Sie auf **Debuggen**Sie Eclipse Projekt-Explorer mit der rechten Maustaste **WorkerRole1**und **Azure**auf.
1. Im Dialogfeld **Eigenschaften für WorkerRole1 Debuggen** :
    1. Überprüfen Sie **für diese Rolle Remotedebugging aktivieren.**
    1. **Eingabe-Endpunkt verwendet**verwenden der Standardendpunkt Toolkit als **Debuggen (8090: Public, Private: 8090)**generiert.
    1. Sicherstellen Sie, dass die Option **Start JVM im Standbymodus Debugger Sekunden** deaktiviert ist.
        >[AZURE.IMPORTANT] Die Option **Start JVM im Standbymodus Debugger Sekunden** soll für Debugszenarios im Compute-Emulator (nicht für Cloud-Bereitstellung). Wird die Option **Start JVM im Standbymodus Debugger Sekunden** wird Serverprozess Starten angehalten, bis der JVM Eclipse Debugger verbunden ist. Diese Option für eine Debugsitzung mit Serveremulator verwenden konnte, verwenden Sie es für eine Debugsitzung in einer Cloud-Umgebung. Initialisierung des Servers erfolgt in Azure Startaufgabe und Azure Cloud macht öffentliche Endpunkte verfügbar bis starttask abgeschlossen ist. Daher wird ein Startvorgang nicht erfolgreich abgeschlossen bei aktivierter Option in einer Cloudbereitstellung, da es auf einer Verbindung von einem externen Client Eclipse nicht.
1. Klicken Sie auf **Debugkonfigurationen**.
1. Im Dialogfeld **Konfiguration von Azure Debuggen** :
    1. Wählen Sie für **Java-Projekt Debuggen**das **MyHelloWorld** Projekt.
    1. Überprüfen Sie zum **Debuggen konfigurieren** **Azure compute Emulator**.
1. Klicken Sie auf **OK** , um das Dialogfeld **Azure Debug Configuration** zu schließen.
1. Klicken Sie auf **OK** , um das Dialogfeld **Eigenschaften für WorkerRole1 Debuggen** zu schließen.
1. Legen Sie einen Haltepunkt im index.jsp:
    1. Eclipse Projekt-Explorer erweitern Sie **MyHelloWorld** **Inhaltsordner**, und doppelklicken Sie auf **index.jsp**.
    1. Index.jsp mit der rechten Maustaste in die blaue Leiste links neben den Java-Code und **Ein/aus Haltepunkte**auf, wie im folgenden dargestellt: ![][ic551537]

       Ein Haltepunkt festgelegt ist, ein Haltepunktsymbol innerhalb der blaue Balken neben den Java-Code angezeigt.
1. Starten Sie die Anwendung im Serveremulator Schaltfläche **Ausführen im Emulator Azure** Azure Symbolleiste.
1. In Eclipse Menü auf **Ausführen** , und klicken Sie auf **Debuggen Konfigurationen**.
1. Im Dialogfeld **Debug Configurations** **Remoteanwendung Java** im linken Fensterbereich erweitern, **Azure-Emulator (WorkerRole1)**auswählen und klicken Sie auf **Debuggen**.
1. Nachdem der Serveremulator gibt an, dass die Anwendung in Ihrem Browser führen Sie **Http://localhost: 8080/MyHelloWorld**ausgeführt wird.
    Aufforderung durch ein Dialogfeld **Bestätigen Perspektive wechseln** , klicken Sie auf **Ja**.
    Die Debugsitzung jetzt in der Codezeile ausgeführt wird, in dem der Haltepunkt festgelegt wurde.

Dieser Vergleich ergab Serveremulator Debuggen; im nächsten Abschnitt wird das Debuggen einer Anwendung in Azure bereitgestellt.

## <a name="debugging-notes"></a>Debuggen von Notizen ##

* Nach dem Debuggen, wechseln Sie die Perspektive aus **Debuggen** **Java** über Menü des Eclipse, indem **Fenster** **Öffnen Perspektive**und die Perspektive, die Sie verwenden möchten.
* Zum Aktivieren des Remotedebuggens im GlassFish verwenden Sie nicht remote debugging Konfigurationsfunktion Azure-Toolkit für Eclipse. Stattdessen konfigurieren Sie GlassFish manuell. Aufgrund der GlassFish vordefinierte Umgebungsvariablen Java-Optionen behandelt, funktioniert remote debugging Konfigurationsfunktion das Toolkit mit GlassFish nicht. Wenn das Toolkit remote debugging Configuration-Funktion aktiviert ist, kann es GlassFish ab.

## <a name="see-also"></a>Siehe auch ##

[Azure-Toolkit für Eclipse][]

[Erstellen eine Hello World-Anwendung für Azure in Eclipse][]

[Azure Toolkit installieren für Eclipse][] 

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Verwaltungsportal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure-Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Erstellen eine Hello World-Anwendung für Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure Toolkit installieren für Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Verwendung der Laufzeitbibliothek Azure Service JSP]: http://go.microsoft.com/fwlink/?LinkID=699551

<!-- IMG List -->

[ic719504]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic719504.png
[ic551537]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic551537.png
