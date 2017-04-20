<properties 
    pageTitle="Erstellen Sie eine Hello World Web App für Azure in IntelliJ | Microsoft Azure" 
    description="In diesem Lernprogramm wird veranschaulicht, wie mit dem Azure-Toolkit für IntelliJ eine Hello World Web App für Azure." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="selvasingh" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-intellij"></a>Erstellen Sie eine Hello World Web App für Azure in IntelliJ

In diesem Lernprogramm wird das Erstellen und Bereitstellen einer einfachen Hello World Anwendung in Azure Web App mit dem [Azure-Toolkit für IntelliJ]. Ein einfaches Beispiel für JSP Einfachheit angezeigt, aber sehr ähnliche Schritte wäre für Java-Servlet angeht Azure-Bereitstellung ist.

Abschluss dieses Lernprogramm sieht die Anwendung wie in der folgenden Abbildung bei der Anzeige in einem Webbrowser:

![][01]
 
## <a name="prerequisites"></a>Erforderliche Komponenten

* Eine Java Developer Kit (JDK), V 1,8 oder höher.
* IntelliJ IDEE Ultimate Edition. Dies kann von <https://www.jetbrains.com/idea/download/index.html>heruntergeladen werden.
* Eine Verteilung einer Java-basierten Webserver oder Anwendungsserver Steg oder Apache Tomcat.
* Azure-Abonnement, <https://azure.microsoft.com/free/> oder <http://azure.microsoft.com/pricing/purchase-options/>erworben.
* Der Azure-Toolkit für IntelliJ. Weitere Informationen finden Sie unter [Installieren der Azure-Toolkit für IntelliJ].

## <a name="to-create-a-hello-world-application"></a>Eine Hello World-Anwendung erstellen

Zunächst beginnen wir mit einem Java-Projekt erstellen.

1. Starten Sie IntelliJ, und klicken Sie im Menü auf **Datei**und dann auf **neu**und dann auf **Projekt**.

   ![][02]

1. Klicken Sie im Dialogfeld Neues Projekt wählen Sie **Java**dann **Anwendung**, und klicken Sie auf **Weiter**.

   ![][03a]

   Wenn weiterhin keine SDK zugewiesen, klicken Sie auf **Ja**.

   ![][03b]

1. Projektnamen Sie für dieses Lernprogramm im **Java-Web-App-auf-Azure**, und klicken Sie dann auf **Fertig stellen**.

   ![][04]

1. Innerhalb IntelliJs Projekt-Explorer-Ansicht erweitern Sie **Java-Web-App-auf-Azure**und **Web**und doppelklicken Sie auf **index.jsp**.

   ![][05]

1. Beim Öffnen der Datei index.jsp in IntelliJ Text dynamisch anzeigen hinzufügen **Hello World!** innerhalb der `<body>` Element. Die aktualisierte `<body>` Inhalt sollte dem folgenden Beispiel ähneln:

   `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Speichern Sie index.jsp

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Zum Bereitstellen der Anwendung zu einem Container Azure Web App

Es gibt verschiedene Arten, eine Java Web-Anwendung in Azure bereitstellen können. Diese praktische Einführung beschreibt eine der einfachsten: Bereitstellung der Anwendung ein Azure Web App Container - besondere Projekttyp keine zusätzliche Tools erforderlich. JDK und der Container Software erhalten Sie von Azure, so keine Notwendigkeit zum Hochladen eigener besteht; Alles wird die Java Web App. Daher wird Veröffentlichungsprozess für Ihre Anwendung, nicht Minuten dauern.

1. IntelliJs Projekt-Explorer mit der rechten Maustaste des **Java-Web-App-auf-Azure** -Projekts. Wenn im Kontextmenü angezeigt wird, wählen Sie **Azure**und dann auf **Veröffentlichen Azure Web App...**

   ![][06]

1. Wenn Sie nicht in Azure aus IntelliJ angemeldet haben, werden Sie aufgefordert, Ihre Azure-Konto anmelden:

   ![][07]

   Hinweis: Haben mehrere Azure Konten möglicherweise einige dieser Aufforderung während der Anmeldung mehrmals angezeigt auch wenn sie dasselbe. In diesem Fall führen Sie die folgenden anmelden Anleitung.

1. Nachdem Sie in Azure-Konto angemeldet haben, zeigt das Dialogfeld **Abonnements verwalten** eine Liste von Abonnements Ihren Anmeldeinformationen zugeordnet sind. Wenn mehrere Abonnements aufgelistet und mit nur einer Teilmenge davon arbeiten möchten, können Sie optional die deaktivieren, die Sie verwenden möchten. Wenn Sie Ihre Abonnements ausgewählt haben, klicken Sie auf **Schließen**.

   ![][08]

1. Erscheint das Dialogfeld **Azure Web App Container bereitstellen** , wird Web App Container angezeigt, die Sie zuvor erstellt haben; Wenn keine Container erstellt haben, wird die Liste leer sein.   

   ![][09]

1. Gehen Sie ein Azure Web App Container vor nicht erstellt haben oder wenn Sie Ihre Anwendung in einem neuen Container veröffentlichen möchten, folgendermaßen vor. Andernfalls wählen Sie eine vorhandene App-Container, und fahren Sie mit Schritt 6 fort.

  1. Klicken Sie auf**+**

        ![][10]

  1. Im Dialogfeld **Neue Web App Container** wird die für die folgenden Schritte dienen angezeigt.

        ![][11]

  1. Geben Sie eine **DNS-Bezeichnung** für Ihr Web App Container; Diese bilden die Bezeichnung Blatt DNS-Host-URL für Ihre Webanwendung in Azure. Hinweis: Der Name muss verfügbar und Azure Web App Namenskonventionen entspricht.

  1. Wählen Sie im Dropdown- **Web Container** die entsprechende Software für die Anwendung.

        Derzeit können Sie Tomcat 8, Tomcat 7 oder 9 Steg. Aktuelle Verteilung der ausgewählten Software von Azure bereitgestellt und führt eine aktuelle Verteilung des JDK 8 von Oracle und Azure bereitgestellt.

  1. Wählen Sie im Dropdown- **Abonnement** das Abonnement für die Bereitstellung verwenden möchten.

  1. Wählen Sie im Dropdown- **Ressourcengruppe** Ressourcengruppe, die Sie Ihrer Anwendung zuordnen möchten.

        Hinweis: Azure Ressourcengruppen können Sie verwandte Ressourcen gruppieren, z. B. sie zusammen gelöscht werden können.

        Sie können eine vorhandene Ressourcengruppe (falls vorhanden) und überspringen g unten oder verwenden Sie die folgende neue Ressourcengruppe erstellen folgendermaßen:

      * Klicken Sie auf **neu...**

      * Das Dialogfeld **Neue Ressourcengruppe** wird angezeigt:

            ![][12]

      * In das Textfeld **Name** einen Namen für die neue Ressourcengruppe.

      * In der **Region** Dropdown-Menü select geeignete Azure Rechenzentren Standort für die Ressourcengruppe.

      * Klicken Sie auf **OK**.

  1. Im Dropdownmenü **App Service-Plan** enthält die app Service-Pläne, die der Ressourcengruppe zugeordnet sind, die Sie ausgewählt.

        Hinweis: Eine App Service-Plan enthält Informationen wie den Speicherort Ihrer Anwendung den Tarif und Größe der Compute. Eine einzelne App Service-Plan kann für mehrere Web Apps verwendet werden, weshalb sie von einer bestimmten Web App getrennt verwaltet wird.

        Sie können eine vorhandene App Service-Plan (falls vorhanden) und überspringen h unten oder im folgenden diese Schritte zum Erstellen einer neuen App Service-Plan verwenden:

      * Klicken Sie auf **neu...**

      * Das Dialogfeld **Neue App Service-Plan** wird angezeigt:

            ![][13]

      * In das Textfeld **Name** einen Namen für die neue App Service-Plan.

      * In der **Position** Dropdown-Menü select geeignete Azure Rechenzentren Speicherort für den Plan.

      * In der **Ebene Preise** Dropdown-Menü Wählen Sie die entsprechenden Preise für den Plan. Zu Testzwecken können Sie **frei**wählen.

      * In der **Instanz** Dropdown-Menü Wählen Sie die entsprechende Instanz Größe für den Plan. Zu Testzwecken können Sie **kleine**.

  1. Sobald Sie alle oben genannten Schritte abgeschlossen haben, sollte das Dialogfeld neuen Web App Container die folgende Abbildung ähneln:

        ![][14]

  1. Klicken Sie auf **OK** , um die Erstellung des neuen Containers Web App abzuschließen.

        Warten für die Liste der Container Web App aktualisiert werden und der Container neu erstellte Web app sollte jetzt in der Liste ausgewählt.

1. Sie können nun die erste Bereitstellung Ihrer Anwendung in Azure abgeschlossen; Klicken Sie auf **OK** , um die Java-Anwendung in ausgewählten Web App-Container bereitstellen.

    ![][15]

    Hinweis: Standardmäßig wird die Anwendung als Unterverzeichnis des Application Servers bereitgestellt. Aktivieren Sie als Anwendung bereitgestellt werden soll, das Kontrollkästchen **Deploy to Root** bevor Sie auf **OK**.

1. Anschließend sollten Sie **Azure-Aktivitätsprotokoll** anzeigen sehen die den Bereitstellungsstatus von Ihrer Anwendung angeben.

    ![][16]

    Der Prozess der Bereitstellung Ihrer Anwendung in Azure dauert nur wenige Sekunden. Wenn Ihre Anwendung bereit, eine Verknüpfung mit dem Namen in der Spalte **Status** **veröffentlicht** sehen Sie. Wenn Sie auf den Link klicken, gelangen Sie zur Startseite der bereitgestellten Web App oder können Sie die Schritte im folgenden Abschnitt um zu Ihrer Anwendung zu wechseln.

## <a name="browsing-to-your-web-app-on-azure"></a>Durchsuchen Ihrer Anwendung in Azure

Verwenden Sie zum brauen zu Ihrer Anwendung in Azure **Azure Explorer** -Ansicht.

Wenn die **Azure Explorer** nicht bereits geöffnet ist, können Sie öffnen es dann **Ansicht** im Menü in IntelliJ, klicken Sie auf **Fenster**und **Service-Explorer**klicken. Wenn Sie nicht bereits angemeldet haben, werden sie dazu aufgefordert.

Wenn die **Azure-Explorer** -Ansicht angezeigt wird, verwenden folgendermaßen zu Ihrer Anwendung 

1. Erweitern Sie **Azure** .

1. Erweitern Sie den Knoten **Web Apps** . 

1. Klicken Sie auf das gewünschte Web App.

1. Wenn das Kontextmenü angezeigt wird, klicken Sie auf **im Browser öffnen**.

    ![][17]

## <a name="updating-your-web-app"></a>Aktualisieren Ihrer Anwendung

Aktualisieren einer vorhandenen Azure Web App ausgeführt ist ein schneller und einfacher Vorgang und stehen zwei Optionen für die Aktualisierung:

* Sie können die Bereitstellung einer vorhandenen Java Web App.
* Sie können eine weitere Java-Anwendung zu demselben Web App Container veröffentlichen.

In beiden Fällen wird der Prozess ist identisch und dauert nur wenige Sekunden:

1. Klicken Sie im Projekt-Explorer IntelliJ Java-Anwendung zu aktualisieren oder zu einem vorhandenen Web App Container hinzufügen.

1. Wenn das Kontextmenü angezeigt wird, wählen Sie **Azure** und **Veröffentlichen Azure Web App...**

1. Da Sie bereits angemeldet haben, sehen Sie eine Liste Ihrer vorhandenen Web App-Container. Wählen Sie die zu veröffentlichen oder die Java-Anwendung erneut veröffentlichen und auf **OK**.

Einige Sekunden später **Azure-Aktivitätsprotokoll** anzeigen Zeigt die aktualisierte Bereitstellung **veröffentlicht** und werden die aktualisierte Anwendung in einem Webbrowser überprüfen.

## <a name="starting-or-stopping-an-existing-web-app"></a>Starten oder Beenden einer vorhandenen Web App

Starten oder Beenden eines vorhandenen Azure Web App-Containers (einschließlich bereitgestellten Java-Anwendung darin), können Sie die **Azure Explorer** -Ansicht.

Wenn die **Azure Explorer** nicht bereits geöffnet ist, können Sie öffnen es dann **Ansicht** im Menü in IntelliJ, klicken Sie auf **Fenster**und **Service-Explorer**klicken. Wenn Sie nicht bereits angemeldet haben, werden sie dazu aufgefordert.

Wenn die **Azure-Explorer** -Ansicht angezeigt wird, verwenden folgendermaßen starten oder Beenden Ihrer Anwendung 

1. Erweitern Sie **Azure** .

1. Erweitern Sie den Knoten **Web Apps** . 

1. Klicken Sie auf das gewünschte Web App.

1. Wenn das Kontextmenü angezeigt wird, klicken Sie auf **Starten** oder **Beenden**. Beachten Sie, dass die Menüoptionen Kontext unterstützt nur ausgeführte Web app beenden oder starten eine Web app, die derzeit nicht ausgeführt wird.

    ![][18]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure-Toolkits für Java IDEs finden Sie unter den folgenden Links:

- [Azure-Toolkit für Eclipse]
  - [Azure Toolkit installieren für Eclipse]
  - [Erstellen Sie Hello World Web App für Azure in Eclipse]
  - [Neuigkeiten in Azure Toolkit für Eclipse]
- [Azure-Toolkit für IntelliJ]
  - [Installieren von Azure Toolkit für IntelliJ]
  - *Erstellen Sie eine Hello World Web App für Azure in IntelliJ (dieser Artikel)*
  - [Neuigkeiten in Azure Toolkit für IntelliJ]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center].

Weitere Informationen zum Erstellen von Azure Web Apps finden Sie unter [Web Apps Overview].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure-Toolkit für Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure-Toolkit für IntelliJ]: ../azure-toolkit-for-intellij.md
[Erstellen Sie Hello World Web App für Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Azure Toolkit installieren für Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Installieren von Azure Toolkit für IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Neuigkeiten in Azure Toolkit für Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Neuigkeiten in Azure Toolkit für IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web Apps-Überblick]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-No-SDK-Specified.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
