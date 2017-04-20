<properties 
    pageTitle="Hinzufügen einer Java-Anwendung zu Azure App Service Web Apps" 
    description="Dieses Lernprogramm veranschaulicht das Hinzufügen einer Seite oder Anwendung Ihrer Instanz von Azure App Service Web-Apps, die bereits für die Verwendung von Java konfiguriert ist." 
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

# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Hinzufügen einer Java-Anwendung zu Azure App Service Web Apps

Nachdem Sie wie unter [Erstellen einer Java Web app in Azure App Service](web-sites-java-get-started.md)Ihrer Anwendung in [Azure App Service][] Java initialisiert haben, können Sie Ihre Anwendung mit Ihrem WAR im Ordner **Webapps** hochladen.

Der Pfad zum Ordner **Webapps** hängt wie Web Apps-Instanz einrichten.

- Wenn Sie Ihrer Anwendung mithilfe von Azure Marketplace eingerichtet, wird der Pfad zu dem Ordner **Webapps** in Form **d:\home\site\wwwroot\bin\application\_Server\webapps**, wobei **Anwendung\_Server** ist der Name der Application Server für Ihr Web Apps-Instanz. 
- Wenn Sie Ihrer Anwendung mithilfe von Azure Konfigurationsbenutzeroberfläche eingerichtet, wird der Pfad zu dem Ordner **Webapps** Formular **d:\home\site\wwwroot\webapps** 

Beachten Sie, dass Sie Datenquellen-Steuerelement die Anwendung oder Webseite [fortlaufende Integrationsszenarien](app-service-continuous-deployment.md)hochladen. FTP ist auch für Ihre Anwendung oder Webseiten hochladen.

Hinweis für Tomcat webapps: Nachdem Sie die WAR-Datei **Webapps** Ordner hochgeladen haben, erkennt Tomcat Application Server hinzugefügt haben, und wird automatisch geladen. Beachten Sie, dass beim Kopieren von Dateien (als WAR-Dateien) in das Stammverzeichnis der Application Server muss neu gestartet werden, bevor die Dateien verwendet werden. Die Autoload-Funktionalität für den Tomcat Java Web apps auf Windows Azure ausgeführte basiert auf neuen WAR-Datei hinzugefügt wird, oder neue Dateien oder Verzeichnisse **Webapps** -Ordner hinzugefügt. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Java Developer Center](/develop/java/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
