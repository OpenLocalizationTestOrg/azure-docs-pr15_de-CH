<properties 
    pageTitle="Debuggen eine Java Web App auf Azure in IntelliJ | Microsoft Azure" 
    description="In diesem Lernprogramm wird veranschaulicht, wie Azure Toolkit für IntelliJ zum Debuggen einer Java Web App auf Azure verwendet." 
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

# <a name="debug-a-java-web-app-on-azure-in-intellij"></a>Debuggen einer Java Web App auf Azure in IntelliJ

Dieses Lernprogramm zeigt, wie eine Java Web App auf Azure mithilfe der [Azure-Toolkit für IntelliJ]Debuggen. Der Einfachheit halber verwenden Sie ein einfaches Beispiel für Java Server Page (JSP) für dieses Lernprogramm jedoch Schritte wäre für Java Servlet Debuggen auf Azure.

Abschluss dieses Lernprogramm sieht die Anwendung wie in der folgenden Abbildung Wenn Sie in IntelliJ Debuggen:

![][01]
 
## <a name="prerequisites"></a>Erforderliche Komponenten

* Eine Java Developer Kit (JDK), V 1,8 oder höher.
* IntelliJ IDEE Ultimate Edition. Dies kann von <https://www.jetbrains.com/idea/download/index.html>heruntergeladen werden.
* Eine Verteilung einer Java-basierten Webserver oder Anwendungsserver Steg oder Apache Tomcat.
* Azure-Abonnement, <https://azure.microsoft.com/en-us/free/> oder <http://azure.microsoft.com/pricing/purchase-options/>erworben.
* Der Azure-Toolkit für IntelliJ. Weitere Informationen finden Sie unter [Installieren der Azure-Toolkit für IntelliJ].
* Ein dynamisches Webprojekt erstellt und Azure App Service bereitgestellt. Beispielsweise finden Sie unter [Erstellen einer Hello World Web App für Azure in IntelliJ].

## <a name="to-debug-a-java-web-app-on-azure"></a>Eine Java Web App auf Azure Debuggen

Diese Schritte in diesem Abschnitt können Sie einem vorhandenen dynamischen Webprojekt bereits eingesetzt als Java Web App in Azure, [Dynamic Web Beispielprojekt] herunterladen und Schritte [erstellen eine Hello World Web App für Azure in IntelliJ] auf Azure bereitgestellt. 

1. Öffnen Sie das Java Web App-Projekt die in Azure in IntelliJ bereitgestellt.

1. Klicken Sie im Menü **Ausführen** und dann auf **Konfigurationen bearbeiten**.

    ![][02]

1. Wenn das Dialogfenster **Ausführen und Debuggen Konfigurationen** : 

    1. Wählen Sie **Azure WebApp**.
    1. Klicken Sie auf **+** , eine neue Konfiguration hinzuzufügen.
    1. Geben Sie einen **Namen** für die Konfiguration.
    1. **Übernehmen Sie der verbleibenden Standardwerte Azure Toolkit vorgeschlagen**

        ![][03]

1. Wählen Sie die Debugkonfiguration Azure Web App die soeben Menü rechts oben erstellte und **Debuggen** auf

    ![][04]

1. Aufforderung zum **Remotedebugging aktivieren in remote Web App jetzt?**, klicken Sie auf **OK**.

1. **Ihrer Anwendung kann jetzt für das Remotedebugging**, aufgefordert werden, klicken Sie auf **OK**.

    ![][05]

1. Wählen Sie die Debugkonfiguration Azure Web App die soeben rechts oben im Menü erstellte und klicken Sie dann auf **Debuggen**.

1. Ein Windows-Befehlszeile oder Unix-Shell wird öffnen und notwendigen zum Debuggen vorbereitet. Sie müssen warten, bis die Verbindung zu Ihrer remote Java Anwendung erfolgreich ist, bevor Sie fortfahren. Wenn Sie Windows verwenden, wird es wie in der folgenden Abbildung aussehen:

    ![][06]

1. Legen Sie einen Haltepunkt in der JSP-Seite, und öffnen Sie die URL Ihrer Anwendung in einem Browser Java:

    1. IntelliJ **Azure Explorer** öffnen.
    1. Navigieren Sie zur **Web Apps** und Java Web App zu debuggen.
    1. Klicken Sie mit der rechten Maustaste auf das Web App und auf **im Browser öffnen**.
    1. IntelliJ treten nun Debugmodus.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center].

Weitere Informationen zum Erstellen von Azure Web Apps finden Sie unter [Web Apps Overview].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure-Toolkit für IntelliJ]: ../azure-toolkit-for-intellij.md
[Installieren von Azure Toolkit für IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Erstellen Sie eine Hello World Web App für Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Dynamische Web-Beispielprojekt]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web Apps-Überblick]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-intellij/01-debug-java-web-app-in-intellij.png
[02]: ./media/app-service-web-debug-java-web-app-in-intellij/02-configure-intellij-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-intellij/03-debug-configuration.png
[04]: ./media/app-service-web-debug-java-web-app-in-intellij/04-select-debug.png
[05]: ./media/app-service-web-debug-java-web-app-in-intellij/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-intellij/06-windows-command-prompt-connection-successful-to-remote.png
