<properties
    pageTitle="Kontinuierliche Bereitstellung Azure App Service | Microsoft Azure"
    description="Informationen Sie zum kontinuierlichen Bereitstellung Azure App Service aktivieren."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="continuous-deployment-to-azure-app-service"></a>Kontinuierliche Bereitstellung Azure App Service

Dieses Lernprogramm zeigt eine fortlaufende Bereitstellungsworkflow für Ihre [Azure App Service] konfigurieren. App Dienstintegration BitBucket GitHub und Visual Studio Team Services (VSTS) ermöglicht einen kontinuierliche Bereitstellungsworkflow Azure, wo die neuesten Updates von einer dieser Dienste veröffentlichte Projekt zieht. Kontinuierliche Bereitstellung ist eine hervorragende Option für Projekte, in denen mehrere und regelmäßige Beiträge sind.

## <a name="overview"></a>Kontinuierliche Bereitstellung aktivieren

Kontinuierliche Bereitstellung aktivieren 

1. Veröffentlichen der Anwendung im Repository für die kontinuierliche Bereitstellung verwendet werden.  
    Weitere Informationen zum Veröffentlichen von Projekt auf diese Dienste finden Sie unter [Erstellen eines Repositorys (GitHub)] [Repo (BitBucket) erstellen]und [beginnen mit der VSTS].

2. Klicken Sie in Ihrer Anwendung Menü Blade in [Azure-Portal]auf **Bereitstellung > Bereitstellungsoptionen**. Klicken Sie auf **Auswählen**und wählen Sie bereitstellungsquelle aus.  

    ![](./media/app-service-continuous-deployment/cd_options.png)
    
    > [AZURE.NOTE] Konfigurieren einer VSTS Konto für App Service-Bereitstellung finden Sie in diesem [Lernprogramm](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
    
3. Autorisierung Workflow abgeschlossen. 

4. Wählen Sie Blatt **bereitstellungsquelle** Projekt und Zweigstellen zur Bereitstellung von. Wenn Sie fertig sind, klicken Sie auf **OK**.
  
    ![](./media/app-service-continuous-deployment/github_option.png)

    > [AZURE.NOTE] Beim Aktivieren der kontinuierlichen Bereitstellung mit GitHub oder BitBucket werden öffentliche und private Projekte angezeigt.

    App Service erstellt eine Zuordnung mit dem ausgewählten Repository, Dateien aus dem angegebenen zieht und einen Klon des Repositorys für Ihre App-Dienst verwaltet. Beim Konfigurieren von VSTS kontinuierliche Bereitstellung von Azure-Portal Integration verwendet die App Service [Kudu Bereitstellungsmodul](https://github.com/projectkudu/kudu/wiki)die Build- und Aufgaben mit bereits automatisiert jeden `git push`. Sie müssen keine kontinuierliche Bereitstellung in VSTS separat einrichten. Nach Abschluss dieses Vorgangs zeigt **Bereitstellungsoptionen** app Blade eine aktive Bereitstellung, die Bereitstellung erfolgreich war.

5. Um sicherzustellen, dass die Anwendung erfolgreich bereitgestellt wurde, klicken Sie auf den **URL** oben Blatt der app im Azure-Portal. 

6. Drücken Sie überprüfen kontinuierliche Bereitstellung aus dem Repository Ihrer Wahl stattfindet, eine Änderung im Repository. Ihre Anwendung sollte entsprechend kurz nach Abschluss zu Repository aktualisieren. Sie können überprüfen, ob im Blatt **Bereitstellungsoptionen** Ihrer App-Update abgerufen hat.

## <a name="VSsolution"></a>Kontinuierliche Bereitstellung einer Visual Studio-Projektmappe 

Legt eine Visual Studio-Projektmappe Azure App Service ist so einfach wie auf eine einfache index.html. Das App Service Bereitstellung vereinfacht die Details einschließlich NuGet Abhängigkeiten wiederherstellen und die Binärdateien der Anwendung erstellen. Sie können die Quelle Steuerelement empfohlene Verfahren Code nur in der Git Repository zu verwalten, und übernehmen den Rest App Service-Bereitstellung.

Die Schritte zum Verschieben der Visual Studio-Projektmappe App Service sind identisch wie im [vorherigen Abschnitt](#overview), sofern Sie Projektmappen und Repository wie folgt konfigurieren:

-   Die Option Visual Studio Datenquellen-Steuerelement zu einer `.gitignore` Datei wie das Bild unten oder manuell ein `.gitignore` Datei in Ihrem Repository-Stammverzeichnis mit diesem [.gitignore Beispiel](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore)ähnelt. 

    ![](./media/app-service-continuous-deployment/VS_source_control.png)
 
-   Die gesamte Lösung Verzeichnisstruktur in Ihrem Repository mit der SLN-Datei in das Repository-Stammverzeichnis hinzufügen

Nachdem Sie das Repository wie beschrieben eingerichtet und Ihre app in Azure für fortlaufende Veröffentlichung eines online Git Repositories konfiguriert haben, können Sie Anwendungsentwicklung in Visual Studio ASP.NET und kontinuierlich Code einfach indem Änderungen auf online Git-Repository bereitstellen.

## <a name="disableCD"></a>Kontinuierliche Bereitstellung deaktivieren

Kontinuierliche Bereitstellung deaktivieren 

1. Klicken Sie in Ihrer Anwendung Menü Blade in [Azure-Portal]auf **Bereitstellung > Bereitstellungsoptionen**. **Klicken Sie in den **Bereitstellungsoptionen** Blade.**

    ![](./media/app-service-continuous-deployment/cd_disconnect.png)    

2. Nachdem die Bestätigungsnachricht beantworten **Ja** , Sie können Ihre app Blade und auf **Bereitstellung > Bereitstellungsoptionen** Veröffentlichung aus einer anderen Quelle einrichten möchten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Wie Sie Probleme bei der kontinuierlichen Bereitstellung überprüfen](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Verwendung von PowerShell für Azure]
* [Verwendung der Azure-Befehlszeilentools für Mac und Linux]
* [Git Dokumentation]
* [Projekt-Kudu](https://github.com/projectkudu/kudu/wiki)

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

[Azure App Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/ 
[Azure-portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Verwendung von PowerShell für Azure]: ../articles/powershell-install-configure.md
[Verwendung der Azure-Befehlszeilentools für Mac und Linux]: ../articles/xplat-cli-install.md
[Git Dokumentation]: http://git-scm.com/documentation

[Erstellen eines Repositorys (GitHub)]: https://help.github.com/articles/create-a-repo
[Erstellen eines Repositorys (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Erste Schritte mit VSTS]: https://www.visualstudio.com/get-started/overview-of-get-started-tasks-vs
[Continuous delivery to Azure using Visual Studio Team Services]: ../articles/cloud-services/cloud-services-continuous-delivery-use-vso.md
