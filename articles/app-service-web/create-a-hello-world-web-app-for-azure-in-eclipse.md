<properties 
    pageTitle="Erstellen Sie Hello World Web App für Azure in Eclipse" 
    description="In diesem Lernprogramm wird veranschaulicht, wie mit dem Azure-Toolkit für Eclipse ein Hello World Web App für Azure." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-eclipse"></a>Erstellen Sie Hello World Web App für Azure in Eclipse

In diesem Lernprogramm wird das Erstellen und Bereitstellen einer einfachen Hello World Anwendung in Azure Web App mit dem [Azure-Toolkit für Eclipse]. Ein einfaches Beispiel für JSP Einfachheit angezeigt, aber sehr ähnliche Schritte wäre für Java-Servlet angeht Azure-Bereitstellung ist.

Abschluss dieses Lernprogramm sieht die Anwendung wie in der folgenden Abbildung bei der Anzeige in einem Webbrowser:

![][01]
 
## <a name="prerequisites"></a>Erforderliche Komponenten

* Eine Java Developer Kit (JDK), V 1.7 oder höher.
* IDE für Java EE Entwickler Eclipse Indigo oder höher. Dies kann von <http://www.eclipse.org/downloads/>heruntergeladen werden.
* Eine Verteilung einer Java-basierten Webserver oder Anwendungsserver Steg oder Apache Tomcat.
* Azure-Abonnement, <https://azure.microsoft.com/en-us/free/> oder <http://azure.microsoft.com/pricing/purchase-options/>erworben.
* Der Azure-Toolkit für Eclipse. Weitere Informationen finden Sie in der [Azure-Toolkit für Eclipse installieren].

## <a name="to-create-a-hello-world-application"></a>Eine Hello World-Anwendung erstellen

Zunächst beginnen wir mit einem Java-Projekt erstellen.

1. Starten Sie Eclipse klicken Sie im Menü auf **Datei**, klicken Sie auf **neu**und klicken Sie dann auf **Dynamische Webprojekt**. ( **Dynamische Webprojekt** als verfügbaren Projekt nach dem Klicken auf **Datei** und **neu**nicht angezeigt wird, dann folgendermaßen: Klicken Sie auf **Datei**, klicken Sie auf **neu**, klicken Sie auf **Projekt** **Web**erweitern, klicken Sie auf **Dynamische Webprojekt**, und klicken Sie auf **Weiter**.)

1. Projektnamen Sie für dieses Lernprogramm im **MyHelloWorld**. Ihr Bildschirm wird ähnlich der folgenden angezeigt:

    ![][02]

1. Klicken Sie auf **Fertig stellen**.

1. Erweitern Sie in Eclipse Projekt-Explorer-Ansicht **MyHelloWorld**. Maustaste **Inhaltsordner**, klicken Sie auf **neu**und klicken Sie dann auf **JSP-Datei**

1. Das Dialogfeld **Neue JSP-Datei** den Namen der Datei **index.jsp**. Behalten Sie den übergeordneten Ordner **MyHelloWorld-Inhaltsordner**.

1. Klicken Sie im Dialogfeld **JSP-Vorlage auswählen** für dieses Lernprogramm wählen Sie **Neue JSP-Datei (html)**, und klicken Sie dann auf **Fertig stellen**.

1. Beim Öffnen der Datei index.jsp in Eclipse Text dynamisch anzeigen hinzufügen **Hello World!** innerhalb der `<body>` Element. Die aktualisierte `<body>` Inhalt sollte dem folgenden Beispiel ähneln:

    `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Speichern Sie index.jsp

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Zum Bereitstellen der Anwendung zu einem Container Azure Web App

Es gibt verschiedene Arten, eine Java Web-Anwendung in Azure bereitstellen können. Diese praktische Einführung beschreibt eine der einfachsten: Bereitstellung der Anwendung ein Azure Web App Container - besondere Projekttyp keine zusätzliche Tools erforderlich. JDK und der Container Software erhalten Sie von Azure, so keine Notwendigkeit zum Hochladen eigener besteht; Alles wird die Java Web App. Daher wird Veröffentlichungsprozess für Ihre Anwendung, nicht Minuten dauern.

1. Eclipse Projekt-Explorer mit der rechten Maustaste **MyHelloWorld**.

1. Klicken Sie im Kontextmenü **Azure**, klicken Sie auf **Veröffentlichen Azure Web App...**

    ![][03]
   
    Während das Webanwendungsprojekt im Projekt-Explorer ausgewählt ist, können Sie alternativ **Veröffentlichen** Dropdown-Schaltfläche auf der Symbolleiste auf und wählen **als Azure Web App veröffentlichen** aus:
   
    ![][14]
   
1. Wenn Sie nicht in Azure aus Eclipse angemeldet haben, werden Sie aufgefordert, Ihre Azure-Konto anmelden:

    ![][04]
   
    Hinweis: Haben mehrere Azure Konten möglicherweise einige dieser Aufforderung während der Anmeldung mehrmals angezeigt auch wenn sie dasselbe. In diesem Fall führen Sie die folgenden anmelden Anleitung.
1. Nachdem Sie in Azure-Konto angemeldet haben, zeigt das Dialogfeld **Abonnements verwalten** eine Liste von Abonnements Ihren Anmeldeinformationen zugeordnet sind. Wenn mehrere Abonnements aufgelistet und mit nur einer Teilmenge davon arbeiten möchten, können Sie optional die deaktivieren, die Sie verwenden möchten. Wenn Sie Ihre Abonnements ausgewählt haben, klicken Sie auf **Schließen**.

    ![][05]
   
1. Erscheint das Dialogfeld **Azure Web App Container bereitstellen** , wird Web App Container angezeigt, die Sie zuvor erstellt haben; Wenn keine Container erstellt haben, wird die Liste leer sein.

    ![][06]
   
1. Gehen Sie ein Azure Web App Container vor nicht erstellt haben oder wenn Sie Ihre Anwendung in einem neuen Container veröffentlichen möchten, folgendermaßen vor. Andernfalls wählen Sie eine vorhandene App-Container, und fahren Sie mit Schritt 7 fort.

    1. Klicken Sie auf **neu...**

    1. Klicken Sie im Dialogfeld **Neue Web App Container** wird angezeigt:

        ![][07]

    1. Geben Sie eine **DNS-Bezeichnung** für Ihr Web App Container; Diese bilden die Bezeichnung Blatt DNS-Host-URL für Ihre Webanwendung in Azure. Hinweis: Der Name muss verfügbar und Azure Web App Namenskonventionen entspricht.

    1. Wählen Sie im Dropdown- **Web Container** die entsprechende Software für die Anwendung.

        Derzeit können Sie Tomcat 8, Tomcat 7 oder 9 Steg. Aktuelle Verteilung der ausgewählten Software von Azure bereitgestellt und führt eine aktuelle Verteilung des JDK 8 von Oracle und Azure bereitgestellt.

    1. Wählen Sie im Dropdown- **Abonnement** das Abonnement für die Bereitstellung verwenden möchten.

    1. Wählen Sie im Dropdown- **Ressourcengruppe** Ressourcengruppe, die Sie Ihrer Anwendung zuordnen möchten.

        Hinweis: Azure Ressourcengruppen können Sie verwandte Ressourcen gruppieren, z. B. sie zusammen gelöscht werden können.

        Sie können eine vorhandene Ressourcengruppe (falls vorhanden) und überspringen g unten oder verwenden Sie die folgende neue Ressourcengruppe erstellen folgendermaßen:

        * Klicken Sie auf **neu...**

        * Das Dialogfeld **Neue Ressourcengruppe** wird angezeigt:

            ![][08]

        * In das Textfeld **Name** einen Namen für die neue Ressourcengruppe.

        * In der **Region** Dropdown-Menü select geeignete Azure Rechenzentren Standort für die Ressourcengruppe.

        * Klicken Sie auf **OK**.

    1. Im Dropdownmenü **App Service-Plan** enthält die app Service-Pläne, die der Ressourcengruppe zugeordnet sind, die Sie ausgewählt.

        Hinweis: Eine App Service-Plan enthält Informationen wie den Speicherort Ihrer Anwendung den Tarif und Größe der Compute. Eine einzelne App Service-Plan kann für mehrere Web Apps verwendet werden, weshalb sie von einer bestimmten Web App getrennt verwaltet wird.

        Sie können eine vorhandene App Service-Plan (falls vorhanden) und überspringen h unten oder im folgenden diese Schritte zum Erstellen einer neuen App Service-Plan verwenden:

        * Klicken Sie auf **neu...**

        * Das Dialogfeld **Neue App Service-Plan** wird angezeigt:

            ![][09]

        * In das Textfeld **Name** einen Namen für die neue App Service-Plan.

        * In der **Position** Dropdown-Menü select geeignete Azure Rechenzentren Speicherort für den Plan.

        * In der **Ebene Preise** Dropdown-Menü Wählen Sie die entsprechenden Preise für den Plan. Zu Testzwecken können Sie **frei**wählen.

        * In der **Instanz** Dropdown-Menü Wählen Sie die entsprechende Instanz Größe für den Plan. Zu Testzwecken können Sie **kleine**.

    1. Sobald Sie alle oben genannten Schritte abgeschlossen haben, sollte das Dialogfeld neuen Web App Container die folgende Abbildung ähneln:

        ![][10]

    1. Klicken Sie auf **OK** , um die Erstellung des neuen Containers Web App abzuschließen.

        Warten für die Liste der Container Web App aktualisiert werden und der Container neu erstellte Web app sollte jetzt in der Liste ausgewählt.

1. Sie können nun die erste Bereitstellung Ihrer Anwendung in Azure ausführen:

    ![][11]

    Klicken Sie auf **OK** , um die Java-Anwendung in ausgewählten Web App-Container bereitstellen.

    Hinweis: Standardmäßig wird die Anwendung als Unterverzeichnis des Application Servers bereitgestellt. Aktivieren Sie als Anwendung bereitgestellt werden soll, das Kontrollkästchen **Deploy to Root** bevor Sie auf **OK**.

1. Anschließend sollten Sie **Azure-Aktivitätsprotokoll** anzeigen sehen die den Bereitstellungsstatus von Ihrer Anwendung angeben.

    ![][12]

    Der Prozess der Bereitstellung Ihrer Anwendung in Azure dauert nur wenige Sekunden. Wenn Ihre Anwendung bereit, eine Verknüpfung mit dem Namen in der Spalte **Status** **veröffentlicht** sehen Sie. Wenn Sie auf den Link klicken, dauert Sie zur Startseite der bereitgestellten Web App.

## <a name="updating-your-web-app"></a>Aktualisieren Ihrer Anwendung

Aktualisieren einer vorhandenen Azure Web App ausgeführt ist ein schneller und einfacher Vorgang und stehen zwei Optionen für die Aktualisierung:

* Sie können die Bereitstellung einer vorhandenen Java Web App.
* Sie können eine weitere Java-Anwendung zu demselben Web App Container veröffentlichen.

In beiden Fällen wird der Prozess ist identisch und dauert nur wenige Sekunden:

1. Klicken Sie im Projektexplorer Eclipse Java-Anwendung zu aktualisieren oder zu einem vorhandenen Web App Container hinzufügen.

2. Wenn das Kontextmenü angezeigt wird, wählen Sie **Azure** und **Veröffentlichen Azure Web App...**

3. Da Sie bereits angemeldet haben, sehen Sie eine Liste Ihrer vorhandenen Web App-Container. Wählen Sie die zu veröffentlichen oder die Java-Anwendung erneut veröffentlichen und auf **OK**.

Einige Sekunden später **Azure-Aktivitätsprotokoll** anzeigen Zeigt die aktualisierte Bereitstellung **veröffentlicht** und werden die aktualisierte Anwendung in einem Webbrowser überprüfen.

## <a name="stopping-an-existing-web-app"></a>Beendet eine bestehende Web App

Zu einem vorhandenen Azure Web App-Container (einschließlich bereitgestellten Java-Anwendung darin), **Azure Explorer** -Ansicht verwenden können.

Wenn **Azure Explorer** -Ansicht nicht bereits geöffnet ist, können Sie Menü dann **Fenster** in Eclipse öffnen und klicken Sie auf **Ansicht anzeigen** **...**und dann **Azure**und dann auf **Azure Explorer**. Wenn Sie nicht bereits angemeldet haben, werden sie dazu aufgefordert.

Wenn die **Azure-Explorer** -Ansicht angezeigt wird, verwenden folgendermaßen zu Ihrer Anwendung 

1. Erweitern Sie **Azure** .

1. Erweitern Sie den Knoten **Web Apps** . 

1. Klicken Sie auf das gewünschte Web App.

1. Wenn das Kontextmenü angezeigt wird, klicken Sie auf **Beenden**.

    ![][13]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den folgenden Links:

* [Java Developer Center](/develop/java/).
* [Web Apps-Überblick](app-service-web-overview.md)

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure-Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Toolkit installieren für Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[01]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/01-Web-Page.png
[02]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/02-Dynamic-Web-Project.png
[03]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/03-Context-Menu.png
[04]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/04-Log-In-Dialog.png
[05]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/05-Manage-Subscriptions-Dialog.png
[06]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/06-Deploy-To-Azure-Web-Container.png
[07]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/07-New-Web-App-Container-Dialog.png
[08]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/08-New-Resource-Group-Dialog.png
[09]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/09-New-Service-Plan-Dialog.png
[10]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/11-Completed-Deploy-Dialog.png
[12]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/12-Activity-Log-View.png
[13]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/13-Azure-Explorer-Web-App.png
[14]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/publishDropdownButton.png