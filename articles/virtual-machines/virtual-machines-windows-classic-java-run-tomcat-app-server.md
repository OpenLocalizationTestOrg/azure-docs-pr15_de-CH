<properties
    pageTitle="Tomcat auf einem virtuellen Computer | Microsoft Azure"
    description="Dieses Lernprogramm verwendet Ressourcen mit dem klassischen Bereitstellungsmodell erstellt und veranschaulicht, wie eine Windows Virtual Machine erstellen und so konfigurieren, dass Apache Tomcat Application Server ausgeführt."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.workload="web"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a>Ausführen einen Java-Anwendungsserver auf einem virtuellen Computer mit dem klassischen Bereitstellungsmodell erstellt

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Einen virtuellen Computer können Sie mit Azure Server Funktionen. Beispielsweise kann eine virtuelle Maschine auf Windows Azure ausgeführte einen Java-Anwendungsserver wie Apache Tomcat Host konfiguriert. Nach Abschluss dieses Handbuchs müssen Sie wissen, wie eine virtuelle Maschine auf Windows Azure ausgeführte und konfiguriert einen Java-Anwendungsserver ausgeführt.

Lernen Sie Folgendes:

* Wie Sie einen virtuellen Computer erstellen, der ein Java Development Kit (JDK) installiert wurde.
* Wie Sie Remote mit dem virtuellen Computer anmelden.
* Einen Java-Anwendungsserver auf dem virtuellen Computer installieren
* So erstellen Sie einen Endpunkt für den virtuellen Computer.
* Wie Sie einen Port in der Firewall für den Anwendungsserver zu öffnen.

Im Rahmen dieses Lernprogramms installiert ein Apache Tomcat Application Server auf einem virtuellen Computer. Abgeschlossene Installation führt Tomcat-Installation wie folgt.

![Virtuelle Computer mit Apache Tomcat][virtual_machine_tomcat]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Zum Erstellen eines virtuellen Computers

1. Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)an.
2. Klicken Sie auf **neu** **berechnen**klicken Sie, klicken Sie auf **virtuelle Computer**und klicken Sie dann auf **Aus Galerie**.
3. Wählen Sie im Dialogfeld **virtueller Computer auswählen** **JDK 7 Windows Server 2012**.
Beachten Sie, dass **JDK 6 Windows Server 2012** haben Sie Legacyanwendungen, die nicht in JDK 7 ausgeführt werden.
4. Klicken Sie auf **Weiter**.
5. Im Dialogfeld **Konfiguration des virtuellen Computers** :
    1. Geben Sie einen Namen für den virtuellen Computer.
    2. Geben Sie die Größe des virtuellen Computers.
    3. Geben Sie einen Namen für den Administrator im Feld **Benutzername** ein. Beachten Sie diese Namen und das Kennwort, das Sie anschließend eingeben, verwenden Sie diese beim Remote auf dem virtuellen Computer anmelden.
    4. Geben Sie ein Kennwort im Feld **Neues Kennwort** und geben in das Feld **bestätigen** . Dies ist das Kennwort des Administratorkontos.
    5. Klicken Sie auf **Weiter**.
6. Die nächsten virtuellen Computer im Dialogfeld **Konfiguration** :
    1. **Cloud-Dienst**verwenden Sie unter **Erstellen einer neuen Cloud-Dienst**.
    2. Der Wert für **Cloud-Dienst DNS-Namen** muss über cloudapp.net eindeutig sein. Bei Bedarf diesen Wert ändern, damit diese Azure zeigt an, dass er eindeutig ist.
    2. Geben Sie einen Bereich, Gruppe oder virtuelles Netzwerk. Für die Zwecke dieses Lernprogramms angeben einer Region wie **Westen der USA**.
    2. **Konto**wählen Sie **mit einer automatisch generierten Speicherkonto aus**
    3. **Für **Verfügbarkeit**wählen.**
    4. Klicken Sie auf **Weiter**.
7. Im abschließenden Dialogfeld **Konfiguration des virtuellen Computers** :
    1. Akzeptieren Sie die Standardeinträge Endpunkt.
    2. Klicken Sie auf **abgeschlossen**.

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a>Ihr virtueller Computer remote anmelden

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com).
2. Klicken Sie auf **virtuelle Computer**.
3. Klicken Sie auf den Namen des virtuellen Computers, die Sie sich anmelden möchten.
4. Nachdem der virtuelle Computer gestartet wurde, ermöglicht ein Popupmenü am unteren Rand der Seite Verbindungen.
5. Klicken Sie auf **Verbinden**.
6. Antworten Sie auf die Aufforderung nach Bedarf für die Verbindung mit dem virtuellen Computer. Dies sollte speichern oder öffnen die RDP-Datei, die die Details der Verbindung enthält. Möglicherweise Url: Port als des letzten Teils der ersten Zeile der RDP-Datei kopieren und Einfügen in einer remote-in Anwendung.

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a>Einen Java-Anwendungsserver auf dem virtuellen Computer installieren

Sie können einen Java-Anwendungsserver mit dem virtuellen Computer oder einen Java-Anwendungsserver durch ein Installationsprogramm installiert.

Für dieses Lernprogramm wird Tomcat installiert.

1. Wenn Sie Ihre virtuellen Computer angemeldet sind, öffnen Sie eine Browsersitzung auf [Apache Tomcat](http://tomcat.apache.org/download-70.cgi).
2. Doppelklicken Sie auf den Link für **32-Bit/64-Bit Windows Service Installer**. Mithilfe dieser Technik werden Tomcat als Windows-Dienst installiert.
3. Schreiben Sie das Installationsprogramm ausführen.
4. Innerhalb des **Apache Tomcat** -Assistenten folgen der Tomcat-Installation. Für die Zwecke dieses Lernprogramms ist die Standardeinstellungen akzeptieren Ordnung. Erreicht das Dialogfeld **Fertigstellen des Apache Tomcat-Setup-Assistenten** können Sie wahlweise **Ausgeführt Apache Tomcat** Tomcat jetzt zu überprüfen. Klicken Sie auf **Fertig stellen** , um den Tomcat-Installationsvorgang abzuschließen.

## <a name="to-start-tomcat"></a>Starten Sie Tomcat
Wenn Sie nicht im Dialogfeld **Assistenten Apache Tomcat Setup** Tomcat ausgeführt haben, starten Sie ihn mit öffnen ein Eingabeaufforderungsfenster auf dem virtuellen Computer ausgeführt **Tomcat7 starten**.

Sie sollten jetzt sehen Tomcat ausgeführt, wenn Sie die virtuelle Maschine Browser und öffnen Sie <http://localhost: 8080>.

Tomcat von externen Computern finden müssen Sie eine erstellen und einen Port öffnen.

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a>Einen Endpunkt für den virtuellen Computer erstellen
1. Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)an.
2. Klicken Sie auf **virtuelle Computer**.
3. Klicken Sie auf den Namen des virtuellen Computers, der den Java-Anwendungsserver installiert ist.
4. Klicken Sie auf **Endpunkte**.
5. Klicken Sie auf **Hinzufügen**.
6. Klicken Sie im Dialogfeld **Add Endpunkt** sicher **Hinzufügen eigenständiger Endpunkt** ausgewählt ist, und klicken Sie dann auf **Weiter**.
7. Im Dialogfeld **neue Endpunktdaten** :
    1. Geben Sie einen Namen für den Endpunkt. beispielsweise **HttpIn**.
    2. Geben Sie für das Protokoll **TCP** .
    3. **80** für den öffentlichen Port angeben.
    4. Geben Sie für den privaten Port **8080** .
    5. Klicken Sie auf **abschließen** , um das Dialogfeld zu schließen. Der Endpunkt wird nun erstellt.

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a>Port in der Firewall für den virtuellen Computer öffnen
1. Mit dem virtuellen Computer anmelden.
2. Klicken Sie auf **Windows**.
3. Klicken Sie auf **Systemsteuerung**.
4. Klicken Sie auf **System und Sicherheit**, klicken Sie auf **Die Windows-Firewall**und klicken Sie dann auf **Erweiterte Einstellungen**.
5. Klicken Sie auf **Eingehende Regeln**, und klicken Sie dann auf **Neue Regel**.
 ![Neue eingehende Regel][NewIBRule]
6. Wählen Sie für den **Regeltyp** **Port aus**und dann auf **Weiter**.
 ![Neue eingehende Regel port][NewRulePort]
7. Wählen Sie auf dem Bildschirm **Protokolle und Ports** **TCP**, geben Sie die **spezifischen lokalen Anschluss** **8080** und klicken Sie auf **Weiter**.
 ![Neue eingehende Regel][NewRuleProtocol]
8. Wählen Sie auf dem Bildschirm **Aktion** **Verbindung zulassen aus**und dann auf **Weiter**.
 ![Neue eingehende Regelaktion][NewRuleAction]
9. Sicherzustellen Sie auf dem Bildschirm **Profil** , **Domäne**, **Private**und **Öffentliche** ausgewählt sind, klicken Sie auf **Weiter**.
 ![Neue eingehende Regel Profil][NewRuleProfile]
10. Geben Sie auf dem Bildschirm **Name** einen Namen für die Regel wie **HttpIn** (Regelname wird an den Endpunkt an, jedoch nicht erforderlich), und klicken Sie dann auf **Fertig stellen**.  
 ![Name für neue eingehende Regel][NewRuleName]

An diesem Punkt die Tomcat-Website sollte von einem externen Browser eine URL der Form * *http://*Ihre\_DNS\_Namen*. cloudapp.net**wobei ** *der\_DNS\_Namen*** der DNS-Name angegeben werden beim Erstellen der virtuellen Computer.

## <a name="application-lifecycle-considerations"></a>Application Lifecycle Aspekte
* Sie erstellen eine eigene Anwendung Webarchiv (KRIEG) und **Webapps** Ordner hinzufügen. Z. B. erstellen Sie einer grundlegenden Java Service Page (JSP) dynamische Webprojekt als WAR-Datei exportieren Sie und kopieren Sie die WAR in den Apache Tomcat **Webapps** Ordner auf dem virtuellen Computer in einem Browser ausführen.
* Wenn der Tomcat-Service installiert ist, wird es standardmäßig manuell starten. Sie können mit dem Snap-In Dienste automatisch gestartet. Zunächst das Dienste-Snap-in auf **Windows starten**, **Verwaltung**und **Services**. Doppelklicken Sie auf **Apache Tomcat** -Service und **Starttyp** auf **automatisch**eingestellt.

    ![Dienst automatisch starten festlegen][service_automatic_startup]

    Werden die Vorteile Tomcat Starten automatisch startet, wenn der virtuelle Computer neu gestartet wird (z. B. nach dem Neustart des Softwareupdates installiert sind).

## <a name="next-steps"></a>Nächste Schritte
Lernen Sie andere Dienste (wie Azure Storage Servicebus und SQL-Datenbank) möglicherweise mit einer Java-Anwendung sollen in [Java Developer Center](https://azure.microsoft.com/develop/java/)verfügbaren Informationen anzeigen.

[virtual_machine_tomcat]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProfile.png
