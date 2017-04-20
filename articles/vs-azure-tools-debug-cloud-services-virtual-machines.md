<properties 
    pageTitle="Ein Azure-Cloud-Dienst oder einen virtuellen Computer in Visual Studio | Microsoft Azure"
    description="Ein Cloud-Dienst oder einen virtuellen Computer in Visual Studio"
    services="visual-studio-online"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />
<tags 
    ms.service="visual-studio-online"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/15/2016"
    ms.author="tarcher" />

# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Ein Azure-Cloud-Dienst oder einen virtuellen Computer in Visual Studio

Visual Studio bietet verschiedene Optionen zum Debuggen von Azure Cloud Services und virtuellen Maschinen.



## <a name="debug-your-cloud-service-on-your-local-computer"></a>Den Cloud-Dienst auf dem lokalen Computer Debuggen

Sparen Sie Zeit und Geld mit dem Azure compute Emulator Cloud-Dienst auf einem lokalen Computer debuggen. Debuggen eines Dienstes lokal vor der Bereitstellung, steigern Zuverlässigkeit und Leistung Sie ohne Rechenzeit. Allerdings einige mögliche Fehler nur bei einen Clouddienst in Azure ausführen selbst. Sie können diese Fehler Debuggen aktivieren Sie Remotedebuggen, wenn Ihr Dienst veröffentlichen und fügen den Debugger an eine Rolleninstanz.

Der Emulator simuliert den Dienst Azure Compute und damit testen und den Cloud-Dienst Debuggen vor der Bereitstellung in Ihrer Umgebung ausgeführt. Der Emulator behandelt den Lebenszyklus der Rolleninstanzen und bietet Zugriff auf simulierten Ressourcen wie lokaler Speicher. Beim Debuggen oder den Dienst von ausführen Visual Studio automatisch als hintergrundanwendung startet den Emulator und Ihren Dienst in den Emulator einsetzt. Den Emulator können Sie Ihre Ausführung in der lokalen Umgebung anzeigen. Sie können die Vollversion oder die express-Version des Emulators ausführen. (Beginnend mit Azure 2.3 ist die express-Version des Emulators Standard.) Finden Sie [mit Emulator Express zu Cloud-Dienst lokal zu debuggen](https://msdn.microsoft.com/library/dn339018.aspx).

### <a name="to-debug-your-cloud-service-on-your-local-computer"></a>Den Cloud-Dienst auf dem lokalen Computer Debuggen

1. Wählen Sie auf der Menüleiste **Debuggen**, **Debuggen starten** , Azure-Cloud-Dienstprojekt ausgeführt. Als Alternative können Sie F5 drücken. Sie sehen eine Meldung, die der Serveremulator gestartet wird. Startet der Emulator bestätigt das Taskleistensymbol es.

    ![Azure-Emulator in der Taskleiste](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)

1. Anzuzeigen Sie die Benutzeroberfläche für Serveremulator, öffnen das Kontextmenü für das Azure-Symbol im Infobereich, und wählen Sie **Berechnen Emulator-Benutzeroberfläche angezeigt**.

    Linken Bereich der Benutzeroberfläche werden die Dienste aufgeführt, die derzeit Serveremulator und die Rolleninstanzen jeder Dienst bereitgestellt werden. Sie können den Dienst oder die Rollen Lebenszyklus, Protokollierung und Diagnoseinformationen im rechten Fensterbereich angezeigt. Wenn Sie den Fokus am oberen Rand eines Fensters enthalten erweitert rechten füllen.

1. Durchlaufen Sie die Anwendung mit den Befehlen im Menü **Debuggen** Haltepunkte im Code festlegen. Beim schrittweisen Durchlaufen der Anwendung im Debugger werden die Bereiche mit dem aktuellen Status der Anwendung aktualisiert. Beim Debuggen beenden, wird die anwendungsbereitstellung gelöscht. Wenn Ihre Anwendung eine Webrolle enthält Start Action-Eigenschaft Starten des Webbrowsers festgelegt haben, startet Visual Studio Web-Anwendung im Browser. Ändert die Anzahl der Instanzen einer Rolle in der Dienstkonfiguration müssen Sie den Cloud-Dienst beenden und neu starten Debuggen, diese Instanzen der Rolle Debuggen.

    **Hinweis:** Wenn Sie beenden ausführen oder den Dienst debuggen, sind nicht lokalen Serveremulator und der Speicheremulator beendet. Sie müssen diese explizit aus dem Infobereich beenden.


## <a name="debug-a-cloud-service-in-azure"></a>Einen Azure-Clouddienst Debuggen

Um einen Cloud-Dienst von einem Remotecomputer zu debuggen, müssen Sie diese Funktionalität aktivieren Wenn Sie Cloud-Dienst bereitstellen, damit Dienste (z. B. msvsmon.exe) auf den virtuellen Computern installiert sind, die die Rolleninstanzen ausgeführt. Aktivieren Sie nicht remote Debuggen, wenn der Dienst veröffentlicht, müssen Sie den Dienst veröffentlichen remote Debuggen aktiviert.

Aktivieren Sie das Remotedebuggen für einen Clouddienst nicht es Leistung verringert oder fallen zusätzliche Gebühren. Verwenden Sie nicht remote Debuggen eines Dienstes Produktion da Clients, die den Dienst beeinträchtigt werden könnte.

>[AZURE.NOTE] Beim Veröffentlichen eines Cloud-Diensts von Visual Studio können Sie **IntelliTrace** für alle Rollen in diesem Dienst aktivieren, die auf.NET Framework 4 oder.NET Framework 4.5 ausgerichtet. Mit **IntelliTrace**können Sie Ereignisse in einer Instanz der Rolle in der Vergangenheit untersuchen und Kontext damals reproduzieren. Siehe [veröffentlichte Clouddienst Visual Studio mit IntelliTrace Debuggen](http://go.microsoft.com/fwlink/?LinkID=623016) und [IntelliTrace verwenden](https://msdn.microsoft.com/library/dd264915.aspx).

### <a name="to-enable-remote-debugging-for-a-cloud-service"></a>Aktivieren des Remotedebuggens für einen Clouddienst

1. Öffnen Sie das Kontextmenü für den Azure-Projekt und wählen Sie dann **Veröffentlichen**.

1. Wählen Sie die **Staging** -Umgebung und **Debug** Configuration.

    Dies ist nur eine Richtlinie. Sie können die testumgebungen in einer produktiven Umgebung ausführen. Allerdings können Sie Benutzer beeinträchtigen aktivieren Sie Remotedebuggen Produktionsumfeld. Wählen Sie die Releasekonfiguration, aber die Debugkonfiguration macht einfacher debuggen.

    ![Wählen Sie die Debugkonfiguration](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)

1. Führen Sie die üblichen Schritte jedoch **Remote Debugger für alle Rollen aktivieren** das Kontrollkästchen auf der Registerkarte **Erweiterte Einstellungen** .

    ![Debug-Konfiguration](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="to-attach-the-debugger-to-a-cloud-service-in-azure"></a>Um den Debugger an einen Clouddienst in Azure

1. Erweitern Sie im Server-Explorer den Knoten für den Clouddienst.

1. Öffnen Sie das Kontextmenü für die Rolle oder die Instanz der Rolle, die Sie anfügen möchten, und wählen Sie **Debugger anfügen**.

    Debuggen eine Rolle wird der Visual Studio Debugger für jede Instanz dieser Rolle. Der Debugger unterbricht auf einen Haltepunkt für die erste Instanz der Rolle, die dieser Codezeile ausgeführt und erfüllt dieser Haltepunkt. Wenn Sie Debuggen eine Instanz der Debugger fügt nur diese Instanz und auf einen Haltepunkt nur, wenn diese bestimmte Instanz dieser Codezeile ausgeführt und den Haltepunkt erfüllt.

    ![Debugger anfügen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)

1. Nachdem der Debugger an einer Instanz angefügt wie gewohnt debuggen. Der Debugger wird automatisch an den entsprechenden Hostprozess für Ihre Rolle. Je nach die Rolle wird der Debugger w3wp.exe, WaWorkerHost.exe oder WaIISHost.exe. Überprüfen Sie den Prozess, der Debugger angefügt ist, erweitern Sie den Knoten im Server-Explorer. Weitere Informationen zu Azure Prozessen finden Sie unter [Architektur der Azure-Rolle](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) .

    ![Wählen Sie im Dialogfeld Typ code](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Öffnen Sie im Dialogfeld Prozesse, auf der Menüleiste auswählen Debuggen, Fenster, Prozesse Prozesse um zu ermitteln, der Debugger angefügt ist. (Tastatur: Strg + Alt + Z) Um einen bestimmten Prozess zu trennen, öffnen Sie das Kontextmenü, und wählen Sie **Prozess abtrennen**. Oder suchen Sie den Knoten im Server-Explorer, suchen Sie den Prozess öffnen Sie im Kontextmenü und wählen Sie **Prozess abtrennen**.

    ![Debuggen von Prozessen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

>[AZURE.WARNING] Vermeiden Sie lange hält an Haltepunkten Remote Debuggen. Azure behandelt einen Prozess, der länger als ein paar Minuten nicht reagiert beendet und Senden von Datenverkehr an die Instanz beendet. Wenn Sie zu lange anhalten, trennt msvsmon.exe des Prozesses.

Trennen Sie den Debugger von allen Prozessen der Instanz oder öffnen Sie im Kontextmenü für die Rolle oder das Debuggen Instanz und wählen Sie die **Debugger trennen**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Grenzen für das Remotedebugging in Azure

Beim Remotedebuggen von Azure SDK 2.3 hat die folgenden Nachteile.

- Mit Remotedebuggen aktiviert können nicht Sie Cloud-Dienst veröffentlichen, in dem keine Rolle mehr als 25 Instanzen hat.

- Der Debugger verwendet die Ports 30400, 30424, 31400, 31424 und 32400 32424. Wenn Sie versuchen, diese Ports verwenden, Sie werden den Dienst veröffentlichen und eine der folgenden Fehlermeldungen erscheinen im Aktivitätsprotokoll für Azure: 

    - Fehler beim Überprüfen der Datei .cscfg .csdef Datei. 
    Der reservierte Bereich 'Portbereich' for Endpoint überschneidet sich Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector von ' Rolle ' mit einem bereits definierten Port oder.
    - Die Zuordnung ist fehlgeschlagen. Später erneut versuchen Sie, verringern Sie die VM Größe oder Anzahl der Rolleninstanzen oder versuchen Sie eine andere Region bereitstellen.


## <a name="debugging-azure-virtual-machines"></a>Debuggen von Azure virtuelle Computer

Sie können Programme debuggen, die auf Azure virtuelle Computer mit Server-Explorer in Visual Studio. Beim Aktivieren von Remotedebuggen auf Azure VM installiert Azure remote debugging Erweiterung auf dem virtuellen Computer. Sie können dann Anfügen an Prozesse auf dem virtuellen Computer und wie gewohnt debuggen.

>[AZURE.NOTE] Durch den Stapel Azure Ressource-Manager erstellte virtuelle Maschinen können Remote mit Cloud Explorer in Visual Studio 2015 gedebuggt werden. Weitere Informationen finden Sie unter [Verwalten von Azure Ressourcen mit Cloud Explorer](http://go.microsoft.com/fwlink/?LinkId=623031).

### <a name="to-debug-an-azure-virtual-machine"></a>Ein Azure Virtual Machine Debuggen

1. Im Server-Explorer erweitern Sie den Knoten virtuelle Computer, und wählen Sie den Knoten des virtuellen Computers, den Sie debuggen möchten.

1. Öffnen Sie das Kontextmenü, und wählen Sie **Debuggen aktivieren**. Gefragt, ob Sie sicher auf dem virtuellen Computer Debuggen aktiviert werden soll, wählen Sie **Ja**.

    Azure installiert remote debugging Erweiterung auf dem virtuellen Computer Debuggen zu aktivieren.

    ![Virtual Machine Debug-Befehl aktivieren](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure Aktivitätsprotokoll](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Nach Abschluss die remote debugging Erweiterung installieren Kontextmenü des virtuellen Computers und **Debugger anhängen** wählen

    Azure Ruft eine Liste der Prozesse auf dem virtuellen Computer ab und zeigt sie an Dialogfeld Prozess anhängen.

    ![Anfügen des Debuggers](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. Wählen Sie im Dialogfeld **an den Prozess anhängen** zum Einschränken der Ergebnisliste zeigen nur die Codetypen zu Debuggen **auswählen** . Sie können 32 oder 64-Bit managed Code und systemeigenen Code Debuggen.

    ![Wählen Sie im Dialogfeld Typ code](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Wählen Sie die Prozesse zu Debuggen auf dem virtuellen Computer **Anfügen**. Beispielsweise können Sie den w3wp.exe-Prozess, wenn Sie eine Webanwendung auf dem virtuellen Computer debuggen. Weitere Informationen finden Sie unter [Debuggen einen oder mehrere Prozesse in Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) und [Azure-Rolle-Architektur](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) .

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Erstellen eines Webprojekts und einer virtuellen Maschine zum Debuggen

Bevor Sie Azure Projekt veröffentlichen, finden Sie es nützlich, Debuggen und Testen von Szenarien unterstützt und wo können testen und Überwachen von Programmen, in einer geschlossenen Umgebung testen. Eine Möglichkeit hierzu ist Remote Debuggen Ihrer Anwendung auf einem virtuellen Computer.

Visual Studio ASP.NET Projekte bieten die Möglichkeit, einen praktischen virtuellen Computer erstellen, den Sie zum Testen der Anwendung verwenden können. Der virtuelle Computer umfasst häufig benötigten Endpunkte PowerShell Remotedesktop und WebDeploy.

### <a name="to-create-a-web-project-and-a-virtual-machine-for-debugging"></a>Erstellen eines Webprojekts und einer virtuellen Maschine zum Debuggen

1. Erstellen Sie in Visual Studio ein neues ASP.NET Web-Anwendung.

1. Wählen Sie im Dialogfeld Neues Projekt von ASP.NET im Abschnitt Azure **VM** im Dropdown-Listenfeld aus. Lassen Sie das Kontrollkästchen **Remoteressourcen erstellen** ausgewählt. Wählen Sie **OK** , um fortzufahren.

    **Erstellen virtueller Computer auf Azure** -Dialogfeld wird angezeigt.


    ![Dialogfeld für ASP.NET Web-Projekt erstellen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Hinweis:** Sie werden aufgefordert, Ihre Azure-Konto anmelden, wenn Sie nicht bereits angemeldet sind.

1. Wählen Sie verschiedene Einstellungen für den virtuellen Computer, und klicken Sie auf **OK**. Weitere Informationen finden Sie unter [virtuelle Computer]( http://go.microsoft.com/fwlink/?LinkId=623033) .

    Der eingegebene DNS-Namen werden der Name des virtuellen Computers. 

    ![Erstellen der virtuellen Computer in Azure Dialogfeld](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure erstellt den virtuellen Computer und Vorschriften und Endpunkte wie Remotedesktop und Web Deploy konfiguriert



1. Nachdem der virtuelle Computer vollständig konfiguriert wurde, wählen Sie im Server-Explorer des virtuellen Computers Knoten.

1. Öffnen Sie das Kontextmenü, und wählen Sie **Debuggen aktivieren**. Gefragt, ob Sie sicher auf dem virtuellen Computer Debuggen aktiviert werden soll, wählen Sie **Ja**. 

    Azure installiert remote debugging Erweiterung virtuellen Computer so aktivieren Sie debuggen.

    ![Virtual Machine Debug-Befehl aktivieren](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure Aktivitätsprotokoll](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Veröffentlichen Sie das Projekt gemäß [wie: Bereitstellen einer Web Project verwenden per Mausklick Veröffentlichen in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx). Um Debuggen auf dem virtuellen Computer auf **der Einstellungsseite des **Veröffentlichen** -Assistenten** wählen Sie **Debuggen** der Konfiguration. Dadurch wird sichergestellt, dass beim Debuggen Codesymbole verfügbar sind.

    ![Eigenschaften veröffentlichen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)

1. **Veröffentlichungsoptionen Datei**versehen Sie, **zusätzliche Dateien am Ziel entfernen** Wenn das Projekt bereits zu einem früheren Zeitpunkt bereitgestellt wurde.

1. Nachdem das Projekt auf im Server-Explorer im Kontextmenü des virtuellen Computers veröffentlicht wählen Sie **Debugger anfügen aus**

    Azure Ruft eine Liste der Prozesse auf dem virtuellen Computer ab und zeigt sie an Dialogfeld Prozess anhängen.

    ![Anfügen des Debuggers](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. Wählen Sie im Dialogfeld **an den Prozess anhängen** zum Einschränken der Ergebnisliste zeigen nur die Codetypen zu Debuggen **auswählen** . Sie können 32 oder 64-Bit managed Code und systemeigenen Code Debuggen.

    ![Wählen Sie im Dialogfeld Typ code](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Wählen Sie die Prozesse zu Debuggen auf dem virtuellen Computer **Anfügen**. Beispielsweise können Sie den w3wp.exe-Prozess, wenn Sie eine Webanwendung auf dem virtuellen Computer debuggen. Weitere Informationen finden Sie unter [Debuggen einen oder mehrere Prozesse in Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) .

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie **Intellitrace** , Protokoll und Ereignisse von einem Releaseserver sammeln. Finden Sie unter [Debuggen eines veröffentlichten Clouddiensts Visual Studio mit IntelliTrace](http://go.microsoft.com/fwlink/?LinkID=623016).
- Verwenden Sie **Azure-Diagnose** sich Informationen von Code in Funktionen, ob die Rollen in der oder in Azure. [Sammeln von Protokolldaten mithilfe von Azure-Diagnose](http://go.microsoft.com/fwlink/p/?LinkId=400450)angezeigt.
