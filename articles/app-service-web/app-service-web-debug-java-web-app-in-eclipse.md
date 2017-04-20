<properties 
    pageTitle="Debuggen eine Java Web App auf Azure in Eclipse | Microsoft Azure" 
    description="Dieses Lernprogramm veranschaulicht Azure Toolkit für Eclipse zum Debuggen einer Java Web App auf Azure verwendet." 
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
    ms.date="09/20/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="debug-a-java-web-app-on-azure-in-eclipse"></a>Debuggen einer Java Web App auf Azure in Eclipse

Dieses Lernprogramm zeigt ein Java Web App auf Azure mithilfe der [Azure-Toolkit für Eclipse]Debuggen. Der Einfachheit halber verwenden Sie ein einfaches Beispiel für Java Server Page (JSP) für dieses Lernprogramm jedoch Schritte wäre für Java Servlet Debuggen auf Azure.

Abschluss dieses Lernprogramm sieht die Anwendung wie in der folgenden Abbildung Wenn Sie Eclipse Debuggen:

![][01]
 
## <a name="prerequisites"></a>Erforderliche Komponenten

* Eine Java Developer Kit (JDK), V 1,8 oder höher.
* IDE für Java EE Entwickler Eclipse Indigo oder höher. Dies kann von <http://www.eclipse.org/downloads/>heruntergeladen werden.
* Eine Verteilung einer Java-basierten Webserver oder Anwendungsserver Steg oder Apache Tomcat.
* Azure-Abonnement, <https://azure.microsoft.com/en-us/free/> oder <http://azure.microsoft.com/pricing/purchase-options/>erworben.
* Der Azure-Toolkit für Eclipse. Weitere Informationen finden Sie in der [Azure-Toolkit für Eclipse installieren].
* Ein dynamisches Webprojekt erstellt und Azure App Service bereitgestellt. Beispielsweise finden Sie unter [Erstellen einer Hello World Web App für Azure in Eclipse].

## <a name="to-debug-a-java-web-app-on-azure"></a>Eine Java Web App auf Azure Debuggen

Diese Schritte in diesem Abschnitt können Sie einem vorhandenen dynamischen Webprojekt bereits eingesetzt als Java Web App in Azure, [Dynamic Web Beispielprojekt] herunterladen und Schritte [erstellen eine Hello World Web App für Azure in Eclipse] auf Azure bereitgestellt. 

1. Öffnen Sie Eclipse.

1. Konfigurieren Sie Timeouts für Remotedebugging:

    1. Klicken Sie im Menü **Fenster** in Eclipse und klicken Sie auf **Voreinstellungen**.
    1. Erweitern Sie den Knoten **Java** und dann **Debuggen**.
    1. **Debugger-Zeitlimit (ms)** und **Zeitlimit (ms) starten** zu konfigurieren `120000`.

        ![][02]

    1. Klicken Sie auf **OK** , um das Dialogfeld **"Voreinstellungen"** zu schließen.

1. Wählen Sie in Eclipse Projekt-Explorer-Ansicht die dynamischen Web die in Azure bereitgestellt haben. Wenn das Kontextmenü angezeigt wird, wählen Sie **Debuggen**und dann auf **Azure Web App**.

    ![][03]

1. Ist dies beim ersten dynamischen Webprojekt Debuggen, wird das Dialogfenster **Debug Configurations** geöffnet; Sie können die Standardwerte übernehmen, die vom Toolkit auf der Registerkarte **Verbindung** angegeben werden. Klicken Sie auf der Registerkarte **Quelle** auf **Hinzufügen**und dann auf **Java-Projekt**wählen Sie **Dynamische Webprojekt**und klicken Sie dann auf **OK**. Nachdem Sie diese Schritte abgeschlossen haben, klicken Sie auf **Debuggen**.

    ![][04]

1. Aufforderung zum **Remotedebugging aktivieren in remote Web App jetzt?**, klicken Sie auf **OK**.

1. **Ihrer Anwendung kann jetzt für das Remotedebugging**, aufgefordert werden, klicken Sie auf **OK**.

    ![][05]

1. Wenn **Debug Configurations** Dialogfeld erneut angezeigt wird, klicken Sie auf **Debuggen**.

1. Ein Windows-Befehlszeile oder Unix-Shell wird öffnen und notwendigen zum Debuggen vorbereitet. Sie müssen warten, bis die Verbindung zu Ihrer remote Java Anwendung erfolgreich ist, bevor Sie fortfahren. Wenn Sie Windows verwenden, sieht es wie in der folgenden Abbildung.

    ![][06]

1. Legen Sie einen Haltepunkt in der JSP-Seite, und öffnen Sie die URL Ihrer Anwendung in einem Browser Java:

    1. Öffnen Sie Eclipse **Azure Explorer** .
    1. Navigieren Sie zur **Web Apps** und Java Web App zu debuggen.
    1. Klicken Sie mit der rechten Maustaste auf das Web App und auf **im Browser öffnen**.
    1. Eclipse tritt jetzt in den Debugmodus.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center].

Weitere Informationen zum Erstellen von Azure Web Apps finden Sie unter [Web Apps Overview].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure-Toolkit für Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure Toolkit installieren für Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Erstellen Sie Hello World Web App für Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Dynamische Web-Beispielprojekt]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web Apps-Überblick]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-eclipse/01-debug-java-web-app-in-eclipse.png
[02]: ./media/app-service-web-debug-java-web-app-in-eclipse/02-configure-eclipse-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-eclipse/03-debug-as.png
[04]: ./media/app-service-web-debug-java-web-app-in-eclipse/04-debug-configurations.png
[05]: ./media/app-service-web-debug-java-web-app-in-eclipse/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-eclipse/06-windows-command-prompt-connection-successful-to-remote.png
