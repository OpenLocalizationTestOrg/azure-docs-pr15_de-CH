<properties
    pageTitle="Erstellen Sie Hello World Clouddienst für Azure in Eclipse"
    description="Informationen Sie zum Erstellen einer einfachen Hello World-Anwendung mit dem Azure-Toolkit für Eclipse."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->

# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>Erstellen Sie Hello World Clouddienst für Azure in Eclipse #

Die folgenden Schritte zeigen Sie zum Erstellen und Bereitstellen einer einfachen JSP-Anwendung in Azure mit dem Azure-Toolkit für Eclipse. Ein JSP Beispiel Einfachheit, aber sehr ähnliche Schritte wäre für Java-Servlet angeht Azure-Bereitstellung ist.

Die Anwendung sieht etwa wie folgt:

![][ic600360]

## <a name="prerequisites"></a>Erforderliche Komponenten ##

* Eine Java Developer Kit (JDK), V 1.7 oder höher.
* IDE für Java EE Entwickler Eclipse Indigo oder höher. Dies kann von <http://www.eclipse.org/downloads/>heruntergeladen werden.
* Eine Verteilung einer Java-basierten Webserver oder Anwendungsserver wie Apache Tomcat, GlassFish, JBoss-Anwendungsserver, Steg oder IBM® WebSphere® Application Server Liberty Core.
* Azure-Abonnement, von <http://azure.microsoft.com/pricing/purchase-options/>erworben.
* Der Azure-Toolkit für Eclipse. Weitere Informationen finden Sie in der [Azure-Toolkit für Eclipse installieren][].

## <a name="to-create-a-hello-world-application"></a>Hello World-Anwendung erstellen ##

Zunächst beginnen wir mit einem Java-Projekt erstellen.

*  Starten Sie Eclipse klicken Sie im Menü auf **Datei**, klicken Sie auf **neu**und klicken Sie dann auf **Dynamische Webprojekt**. ( **Dynamische Webprojekt** als verfügbaren Projekt nach dem Klicken auf **Datei**, **neu**, sehen dann folgendermaßen: Klicken Sie auf **Datei**, klicken Sie auf **neu**, klicken Sie auf **Projekt** **Web**erweitern, klicken Sie auf **Dynamische Webprojekt**, und klicken Sie auf **Weiter**.)
*  Projektnamen Sie für dieses Lernprogramm im **MyHelloWorld**. (Sorgen Sie diesen Namen, Schritte in diesem Lernprogramm erwarten die WAR-Datei MyHelloWorld genannt werden). Ihr Bildschirm wird ähnlich der folgenden angezeigt: ![][ic589576]
* Klicken Sie auf **Fertig stellen**.
* Erweitern Sie in Eclipse Projekt-Explorer-Ansicht **MyHelloWorld**. Maustaste **Inhaltsordner**, klicken Sie auf **neu**und klicken Sie dann auf **JSP-Datei**
* Das Dialogfeld **Neue JSP-Datei** den Namen der Datei **index.jsp**. Halten Sie den übergeordneten Ordner als **MyHelloWorld-Inhaltsordner**wie im folgenden dargestellt:  ![][ic659262]
* **JSP-Vorlage auswählen** -Dialogfeld für dieses Lernprogramm **Neue JSP-Datei (html)** und klicken Sie auf **Fertig stellen**.
* Beim Öffnen der Datei index.jsp in Eclipse fügen Text dynamisch anzeigen **Hello World!** innerhalb der `<body>` Element. Die aktualisierte `<body>` Inhalt sollte wie folgt angezeigt:
```
    <body>
    <b><% out.println("Hello World!"); %></b>
    </body>
```
* Speichern Sie index.jsp

## <a name="to-deploy-your-application-to-azure-the-quick-and-simple-way"></a>Zum Bereitstellen der Anwendung in Azure, schnell und einfach ##

Als haben eine Java Anwendung testen können folgende Verknüpfung Sie direkt auf der Azure-Cloud ausprobieren.

1. Klicken Sie im Projektexplorer Eclipse **MyHelloWorld**.
1. Eclipse-Symbolleiste auf **Veröffentlichen** Dropdown-Schaltfläche und dann auf **Veröffentlichen als Azure Cloud Service**
    ![][publishDropdownButton]
1. Wenn Sie diese Anwendung zum ersten Mal in Azure veröffentlichen und Azure Bereitstellungsprojekt für diese Anwendung vor nicht erstellt haben, ein Azure Bereitstellungsprojekt erstellt automatisch. Sie sollten sehen folgende Aufforderung die JDK-Paket auch Listen und Anwendungsserver, die zum Ausführen der Anwendung automatisch bereitgestellt werden.
    ![][ic789598]

    Verknüpfung ermöglicht schnelle und einfache Möglichkeit zum Testen Ihrer Anwendung in Azure ohne Konfigurieren eines bestimmten Servers oder JDK von den Standardwerten abweicht. Wenn Sie die Standardeinstellungen zufrieden sind, können Sie **OK** , um mit den folgenden Schritten fortfahren klicken.
    Allerdings möchten Sie JDK ändern oder Anwendungsserver für Ihre Anwendung verwenden kann, die später durch Bearbeiten des Projekts Azure-Bereitstellung automatisch für Sie erstellt wurde und jetzt **Abbrechen** und **zum Azure-Bereitstellung Projekte Abschnitt** dieser praktischen Einführung lesen.
1. Im Dialogfeld **Azure veröffentlichen** :
    1. Sind keine Abonnements wählen in der Liste **Abonnement** noch gehen Ihre Abonnementinformationen importieren:
        1. Klicken Sie auf **Veröffentlichen EINSTELLUNGEN importieren**.
        1. Klicken Sie im Dialogfeld **Import Abonnementinformationen** auf **Veröffentlichen-Einstellungsdatei herunterladen**. Wenn Sie Ihre Azure-Konto noch nicht angemeldet sind, werden Sie aufgefordert, anmelden. Anschließend werden Sie aufgefordert zum Speichern einer Azure Datei veröffentlichen. Auf Ihrem Computer speichern.
        1. Klicken Sie im Dialogfeld **Import Abonnementinformationen** klicken Sie auf die Schaltfläche **Durchsuchen** , wählen Sie die Datei veröffentlichen, die Sie im vorherigen Schritt lokal gespeichert und klicken Sie auf **Öffnen**. Ihr Bildschirm sollte wie folgt aussehen: ![][ic644267]
        1. Klicken Sie auf **OK**.
    1. **Abonnement**wählen Sie das Abonnement, die für die Bereitstellung verwenden möchten.
    1. **Konto**wählen Sie Speicherkonto, das Sie verwenden möchten, oder klicken Sie auf **neu** , um ein neues Speicherkonto erstellen.
    1. **Dienstname**wählen Sie Cloud-Dienst, den Sie verwenden möchten, oder klicken Sie auf **neu** , um einen neuen Clouddienst erstellen.
    1. **Ziel-Betriebssystem**wählen Sie die Version des Betriebssystems für die Bereitstellung verwenden möchten.
    1. Wählen Sie **Umgebung**für dieses Lernprogramm **Staging**. (Wenn Sie an Ihrem Standort bereitgestellt sind, diese **Produktion**ändern Sie.)
    1. Optional: Sicherstellen Sie, dass **Überschreiben der vorherigen Bereitstellung** aktiviert ist, soll die neue Bereitstellung die vorherige Bereitstellung automatisch überschrieben. Wenn Sie diese Option aktivieren, wird keine Erfahrung "409 Conflict" Probleme beim Veröffentlichen in derselben Position.
        Beachten Sie, dass das Dialogfeld **in Azure veröffentlichen** einen Abschnitt für den **Remotezugriff**. Standardmäßig ist nicht aktiviert, und wir nicht für dieses Beispiel aktiviert wird. Um zu aktivieren, geben Sie einen Benutzernamen und ein Kennwort für Remote anmelden. Weitere Informationen zum Remotezugriff finden Sie unter [RAS für Azure in Eclipse aktivieren][].
        Dialog **in Azure veröffentlichen** wird ähnlich der folgenden angezeigt: ![][ic719488]
1. Klicken Sie auf **Veröffentlichen** der Staging-Umgebung veröffentlichen.
    Einen vollständigen Build ausführen, klicken Sie auf **Ja**. Dies kann mehrere Minuten für den ersten.
    Ein **Azure-Aktivitätsprotokoll** wird im Abschnitt Ansichten Eclipse im Registerkartenformat angezeigt.
    ![][ic719489]
   Dieses Protokoll sowie **die Konsolenansicht** können Sie den Fortschritt der Bereitstellung an. Eine Alternative ist der [Azure-Verwaltungsportal][]anmelden und Bereich **Cloud-Dienste** zur Überwachung des Status verwenden.
1. Wenn die Bereitstellung erfolgreich bereitgestellt wird, zeigt **Azure-Aktivitätsprotokoll** Status **veröffentlicht**. Klicken Sie auf **veröffentlicht**, wie in der folgenden Abbildung dargestellt und wird eine Instanz der Bereitstellung geöffnet.
    ![][ic719490]

Dies war eine Bereitstellung in einer Stagingumgebung werden DNS-Namen im Format http://&lt;*Guid*&gt;. cloudapp.net und der URL der DNS-Name und ein Suffix für die Anwendung enthalten. Beispielsweise http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. (Abschnitt **MyHelloWorld** beachtet werden.) Sie können auch den Namen in Azure Platform-Verwaltungsportal (im Teil Clouddienste Verwaltungsportal) klicken Sie auf den DNS-Namen finden.

War dieser Anleitung für die Bereitstellung in die Stagingumgebung folgt einer Bereitstellung für die Produktion dieselben Schritte außer im Dialogfeld **Azure veröffentlichen** , **Produktion** statt **Bereitstellung** für die **Umgebung**auswählen. Bereitstellung für die Produktion führt eine URL basierend auf den DNS-Namen Ihrer Wahl anstelle einer GUID für Staging verwendet.

>[AZURE.WARNING] Zu diesem Zeitpunkt haben Sie Ihre Azure-Anwendung in die Cloud bereitgestellt. Allerdings zunächst bemerken Sie, dass bereitgestellte Anwendung, auch wenn er nicht ausgeführt wird, weiterhin antizipieren Abrechenbare Zeit für Ihr Abonnement. Daher ist es äußerst wichtig, dass unerwünschte Installationen aus dem Azure-Abonnement löschen.

## <a name="about-azure-deployment-projects"></a>Azure-Bereitstellung Projekte ##

Mindestens eine Java-Anwendung in Azure bereitstellen, wird ein Bereitstellungsprojekt Azure benötigt. Es spielt "Package", die die Anwendung in umschlossen werden, um auf Azure veröffentlicht werden.

Neben der Anwendung, Azure Bereitstellungsprojekt enthält auch Informationen über andere wichtigen Komponenten der Bereitstellung wichtiger: Anwendung Servercontainer in Ihrer Anwendung ausgeführt und die Java Runtime ausführen. Azure unterstützt eine große Auswahl an Java Laufzeiten und Java-Anwendungsserver aus wählen.

Auch hier verwendete Beispiel für Schulungszwecke vereinfacht wird, kann Azure Bereitstellungsprojekt auch andere wichtige Konfigurationsinformationen enthalten, erstellen beliebig komplexe, skalierbare, hochverfügbare, Multi-Tier-Cloud-Diensten mit der Anwendung ermöglicht. Sie können **Session Affinität ("sticky Sessions")**, **schnelle Zwischenspeichern** **Remotedebuggen**, **SSL-Abladung**, **Firewallports/routing**, **RAS**und zahlreiche andere leistungsstarken Funktionen.

Abschluss der vorherigen Abschnitt dieser praktischen Einführung ("zur Bereitstellung einer Anwendung in Azure, schnell und einfach") werden Sie sehen nun ein neues Azure-Bereitstellungsprojekt im Projekt-Explorer automatisch generiert und mit dem Namen "**MyHelloWorld_onAzure**".

Auch kann in diesem Lernprogramm starten indem zunächst ein leeres Azure Bereitstellungsprojekt selbst erstellen und dann Ihre Anwendung(en) zu. Ist ein längerer Prozess jedoch mehr Kontrolle über die Erstkonfiguration ab.

Um ein neues Bereitstellungsprojekt Azure neu zu erstellen, klicken Sie auf **Neues Bereitstellungsprojekt Azure** ![][ic710876].

Unabhängig davon, ob Sie mit einem bereits vorhandenen Azure-Bereitstellungsprojekt arbeiten, oder neu erstellen, Sie können die Konfiguration und Komponenten wie den JDK oder Anwendungsserver auch jederzeit problemlos.

Ändern der JDK Application Server oder die Anwendungsliste in einem vorhandenen Projekt Azure-Bereitstellung:

1. Erweitern Sie im Projekt-Explorer auf den Projektknoten (z.B. **MyHelloWorld_onAzure**)
2. Mit der rechten Maustaste **WorkerRole1**
3. Erweitern Sie im Untermenü " **Azure** " im Kontextmenü
4. Klicken Sie auf **Server-Konfiguration**

Unabhängig davon, ob starten diese Konfigurationsschritte durch Bearbeiten eines Azure Bereitstellungsprojekts obigen oder erstellen eine völlig neue, sehen Sie die gleiche Art von Dialogfeldern ermöglicht das Konfigurieren der JDK, Server- und Anwendungskomponenten. Weitere Informationen zum Ändern in Dialogfelder JDK Application Server ändern und hinzufügen oder Entfernen von Clientanwendungen in einer Bereitstellung finden Sie auf [Server-Konfigurationseigenschaften][] .

## <a name="windows-only-to-deploy-your-application-to-the-compute-emulator"></a>Nur Windows: zum Bereitstellen der Anwendung Serveremulator ##

>[AZURE.NOTE] Azure-Emulator ist nur unter Windows verfügbar. Überspringen Sie diesen Abschnitt, wenn Sie ein Betriebssystem als Windows verwenden.

Wenn Sie ein neues Azure Bereitstellungsprojekt die beschriebenen Schritte oben, d. h. implizit erstellt haben wurden durch Veröffentlichen der Anwendung Azure, JDK und Anwendung Server für die Cloud, nicht jedoch für lokale Emulation konfiguriert. Um das Projekt im lokalen Emulator testen vorzubereiten, gehen Sie folgendermaßen vor:

1. Klicken Sie im Projektexplorer Eclipse **MyHelloWorld_onAzure**.
1. Klicken Sie auf **WorkerRole1**.
1. Erweitern Sie im Untermenü **Azure** im Kontextmenü.
1. Klicken Sie auf **Konfiguration**.
1. Überprüfen Sie auf der Registerkarte **JDK** Wenn das Toolkit Standard vorkonfiguriert wurde lokalen JDK für Sie. Wenn nicht oder angenommenen Standardeinstellungen ändern möchten, dass das Kontrollkästchen **verwenden JDK aus diesem Pfad für lokal testen** aktiviert und angegebene JDK-Installationspfad, den Sie verwenden möchten. Wenn Sie ändern möchten, klicken Sie auf die Schaltfläche **Durchsuchen** , und wählen Sie den Speicherort des JDK verwenden mit dem Steuerelement.
1. Klicken Sie auf die Registerkarte **Server** .
1. Geben Sie im Textfeld **Lokaler Pfad** unten im Dialogfeld den Pfad eines Servers lokal installiert, die den Typ und Hauptversionsnummer des Servers oben im Dialogfeld das Kontrollkästchen **Bereitstellen ein Server dieses Typs** ausgewählt. Wenn Sie eine andere Art oder Hauptversion des Application Servers verwenden möchten, ändern Sie die Auswahl unter diesem Kontrollkästchen zuerst.
1. Klicken Sie auf **OK**.
1. Eclipse-Symbolleiste klicken Sie **im Azure-Emulator ausführen** ![][ic710879]. Wenn die Schaltfläche **im Azure-Emulator ausführen** nicht aktiviert ist, **MyHelloWorld_onAzure** Eclipse Projekt-Explorer ausgewählt ist, und gewährleistet die Eclipse Projekt-Explorer wie im aktuellen Fenster den Fokus besitzt. Zunächst wird einen vollständigen Build des Projekts starten und starten Sie die Java Web-Anwendung im Serveremulator. (Beachten Sie, dass je nach Leistungsmerkmale des Computers der erste Build kann zwischen wenigen Sekunden bis Minuten, aber nachfolgenden builds wird schneller.) Nach Abschluss der erste Buildschritt werden Sie von Windows User Account Control (UAC) aufgefordert zu diesem Befehl des Computers ändern. Klicken Sie auf **Ja**.

>[AZURE.IMPORTANT] Wenn die UAC-Aufforderung nicht angezeigt wird, Überprüfen der Windows-Taskleiste für die UAC-Symbol, und klicken sie zuerst. Manchmal die UAC Aufforderung nicht als oberstes Fenster angezeigt, aber nur als ein Taskleistensymbol angezeigt wird.

1. Die Ausgabe der Serveremulator Benutzeroberfläche um festzustellen, ob Probleme mit dem Projekt. Je nach Inhalt der Bereitstellung dauert es einige Minuten für Ihre Anwendung im Serveremulator vollständig gestartet werden.
1. Starten Sie Ihren Browser und die URL `http://localhost:8080/MyHelloWorld` als Adresse (die `MyHelloWorld` Teil des URLs wird beachtet). Die MyHelloWorld-Anwendung (die Ausgabe des index.jsp) sollte angezeigt werden wie in der folgenden Abbildung: ![][ic589579]

Wenn Sie Ihre Anwendung im Serveremulator auf Eclipse ausführen möchten klicken Sie **Azure-Emulator zurückzusetzen** , ![][ic710880].

## <a name="to-delete-your-deployment"></a>Löschen die Bereitstellung ##

Zum Löschen der Bereitstellung in Azure Toolkit für Eclipse **MyHelloWorld_onAzure** Eclipse Projekt-Explorer ausgewählt ist, gewährleisten Eclipse Projekt-Explorer das aktuelle Fenster konzentrieren, und klicken Sie dann auf die Schaltfläche **zum Aufheben der Veröffentlichung** ![][ic710883], auf der Ellipse. (Konnte den gleichen Vorgang Zweck mit der rechten Maustaste im Projektexplorer Eclipse **MyHelloWorld_onAzure** , **Azure** klicken und **Bereitstellung zurücknehmen von Azure Cloud**.) Im Dialogfeld **Unpublish Azure-Projekt** wird angezeigt.

![][ic719491]

Wählen Sie Abonnements und Cloud-Dienst, der die Bereitstellung enthält wählen Sie, die Sie löschen möchten und klicken Sie auf **Veröffentlichung**.

(Alternative zum Löschen der Bereitstellung mithilfe des Toolkits ist mit **Cloud-Services** -Abschnitt der Azure-Verwaltungsportal: Navigieren Sie zur Bereitstellung auswählen, und klicken Sie auf die Schaltfläche **Löschen** . Dies wird beendet und die Bereitstellung löschen. Wenn Sie nur die Bereitstellung beenden möchten, nicht löschen klicken Sie **Beenden** anstatt die Schaltfläche **Löschen** , aber wie oben erwähnt, wenn Sie die Bereitstellung nicht löschen, berechenbare Gebühren weiterhin für die Bereitstellung anfallen, auch wenn er beendet wird).

## <a name="see-also"></a>Siehe auch ##

[Azure-Toolkit für Eclipse][]

[Azure Toolkit installieren für Eclipse][] 

[Neuigkeiten in Azure Toolkit für Eclipse][]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Verwaltungsportal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure-Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Aktivieren des Remotezugriffs für Azure Installationen in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699538
[Azure Toolkit installieren für Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Eigenschaften von Server-Konfiguration]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Neuigkeiten in Azure Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png
[publishDropdownButton]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/publishDropdownButton.png
