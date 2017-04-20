<properties 
    pageTitle="Erstellen eine Web-App mit App Service unter Linux | Microsoft Azure" 
    description="Web app erstellen Workflow App für Linux." 
    keywords="Azure app Service, WebApp, Linux Betriebssysteme"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="create-a-web-app-with-app-service-on-linux"></a>Erstellen einer Web-App App Service auf Linux

## <a name="using-the-management-portal-to-create-your-web-app"></a>Mithilfe von Management-Portal Ihrer Anwendung erstellen
Sie können Erstellen Ihrer Anwendung auf Linux [-Verwaltungsportal](https://portal.azure.com) wie unten dargestellt.

![][1]

Wenn Sie die Option wählen, das Blade erstellen sehen Sie wie unten dargestellt. 

![][2]

-   Benennen Sie Ihre Webanwendung.
-   Wählen Sie eine vorhandene Ressourcengruppe oder erstellen Sie eine neue. Siehe Regionen im [Bereich eingeschränkt](./app-service-linux-intro.md).
-   Eine vorhandene Anwendung Serviceplan auswählen oder Erstellen einer (Siehe app Service Plan Notizen im [Abschnitt Einschränkungen](./app-service-linux-intro.md)). 
-   Wählen Sie Büroaufgaben, die Sie verwenden möchten. Sie können zwischen verschiedenen Versionen von Node.js und PHP auswählen. 

Sobald Sie die Anwendung erstellt haben, können Sie Anwendungsstapel aus der Anwendung ändern, wie in der folgenden Abbildung dargestellt.

![][3]

## <a name="deploying-your-web-app"></a>Bereitstellen Ihrer Anwendung

Verwaltungsportal "Bereitstellungsoptionen" auswählen, können Sie die Option zum lokalen Repository Git oder ein GitHub Repository zum Bereitstellen der Anwendung. Die Anleitung danach sind ebenso zu einer nicht-Linux Web app und führen Sie die Schritte in unserer [Git-Bereitstellung](./app-service-deploy-local-git.md) oder [kontinuierliche Bereitstellung](./app-service-continuous-deployment.md) Artikel für GitHub.

FTP können Sie Ihre Anwendung auf die Website hochladen. Sie erhalten FTP-Endpunkt für Ihr Web app aus Abschnitt Protokolle Diagnose wie in der folgenden Abbildung dargestellt.

![][4]


## <a name="next-steps"></a>Nächste Schritte ##

* [Was ist App auf Linux?](./app-service-linux-intro.md)
* [Verwenden der PM2-Konfiguration für Node.js in Web-Apps unter Linux](./app-service-linux-using-nodejs-pm2.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
