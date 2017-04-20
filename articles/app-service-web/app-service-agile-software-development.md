<properties
    pageTitle="Agile-Softwareentwicklung mit Azure App Service"
    description="Erfahren Sie, wie stark komplexe Programme mit Azure App erstellen, die agile-Softwareentwicklung unterstützt."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>


# <a name="agile-software-development-with-azure-app-service"></a>Agile-Softwareentwicklung mit Azure App Service #

In diesem Lernprogramm erfahren Sie, wie stark komplexe Programme mit [Azure App](/services/app-service/) erstellen, die [agile-Softwareentwicklung](https://en.wikipedia.org/wiki/Agile_software_development)unterstützt. Setzt voraus, dass Sie wissen, wie [komplexe Anwendung vorhersehbar in Azure bereitgestellt](app-service-deploy-complex-application-predictably.md).

Nachteile in technischen Prozessen können erfolgreiche agilen Methodiken oft entgegenstehen. Azure App Service mit [kontinuierlichen Veröffentlichung](app-service-continuous-deployment.md) [Stagingumgebungen](web-sites-staged-publishing.md) (Steckplätze) und [Überwachung](web-sites-monitor.md), wenn Weise mit der Orchestrierung Bereitstellung in [Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md)kann eine gute Lösung für Entwickler gehören, die sich agile Softwareentwicklung.

In der folgenden Tabelle ist eine kurze Liste mit agiler Entwicklung und wie Azure ermöglicht jeder von ihnen services.

| Anforderung | Wie kann Azure |
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Jedes Commit wird erstellt<br>-Erstellen Sie automatisch und schnell | Kontinuierliche Bereitstellung konfiguriert, kann Azure App Service als live ausgeführten Builds auf Grundlage einer Verzweigung Dev fungieren. Jedem Code in der Verzweigung gedrückt wird, wird das automatisch erstellte und Ausführen in Azure.|
| -Stellen Sie Builds Selbsttest | Laden von Tests von Webtests usw., mit der Azure-Ressourcen-Manager bereitgestellt werden.|
| -Führen Sie Tests in einem Klon der Produktion | Azure Ressourcenmanager Vorlagen dienen zur Erstellung von Clones von Azure Produktionsumfeld (einschließlich app-Einstellungen, Connection String Vorlagen, Skalierung, etc.) für schnell und vorhersehbar.|
| -Neueste Ergebnis problemlos anzeigen | Kontinuierliche Bereitstellung in Azure aus einem Repository bedeutet, dass Sie neuen Code in einer live-Anwendung testen können, sobald die Änderungen. |
| -Commit in die main-Verzweigung täglich<br>-Bereitstellung automatisieren | Fortlaufende Integration einer Produktion Anwendung mit einer main Zweig bereit automatisch jedes Commit/Merge, die main-Verzweigung zur Produktion |

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Vorgehensweise ##

Über Dev-Test-Phase-Produktion Workflow normalerweise gehen Änderungen an die [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) Probe Anwendung veröffentlichen besteht aus zwei [webapps](/services/app-service/web/)ein Front-End (FE) und die andere Backend-Web-API (BE) und eine [SQL-Datenbank](/services/sql-database/). Sie arbeiten mit Bereitstellungsarchitektur unten:

![](./media/app-service-agile-software-development/what-1-architecture.png)

Das Bild in Worte fassen:

-   Die Bereitstellungsarchitektur in drei unterschiedlichen Umgebungen oder [Ressourcengruppen](../azure-resource-manager/resource-group-overview.md) in Azure, jeweils mit eigenen [App Serviceplan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)Einstellungen [Skalierung](web-sites-scale.md) und SQL-Datenbank getrennt. 
-   Jede Umgebung kann separat verwaltet werden. Sie können auch in anderen Abonnements vorhanden.
-   Staging und Produktion werden als zwei Steckplätze der gleichen App Service-Anwendung implementiert. Die master-Verzweigung ist für fortlaufende Integration mit der Stagingdatenbank.
-   Wenn ein Commit master Branch stagingslot (mit Produktionsdaten) überprüft, in der Produktion Steckplatz [ohne Ausfallzeiten](web-sites-staged-publishing.md)überprüfte staging app ausgelagert.

Die Produktion und Staging-Umgebung zur Vorlage festgelegten [ * &lt;Repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json).

Entwicklungs- und Umgebung festgelegten zur Vorlage [ * &lt;Repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json).

Auch benötigen der normale Verzweigungsstrategie mit Verschieben von der Verzweigung Dev bis den Zweig Test dann master Zweig (Verschieben von Qualität, sozusagen).

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-will-need"></a>Sie benötigen ##

-   Ein Azure-Konto
-   [GitHub](https://github.com/) -Konto
-   Git Shell (mit [GitHub für Windows](https://windows.github.com/)installiert) - dadurch der Git und PowerShell Befehle in derselben Sitzung ausführen 
-   Neueste [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/0.9.4-June2015/azure-powershell.0.9.4.msi) -bits
-   Grundlegendes Verständnis der folgenden:
    -   Bereitstellung von [Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md) Vorlage (Siehe auch [eine komplexe Anwendung vorhersehbar in Azure bereitstellen](app-service-deploy-complex-application-predictably.md))
    -   [Git](http://git-scm.com/documentation)
    -   [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Ein Azure-Konto zum Bearbeiten dieses Lernprogramms benötigen Sie:
> + Sie können [ein Azure-Konto frei](/pricing/free-trial/) - Sie erhalten ein Guthaben bezahlt Azure Services ausprobieren können und sogar nachdem sie verbraucht das Konto beibehalten mit kostenlosen Azure Services wie Web Apps.
> + Kann [Visual Studio Abonnementvorteile aktivieren](/pricing/member-offers/msdn-benefits-details/) - der Visual Studio-Abonnement bietet Ihnen Credits monatlich für bezahlte Azure Services verwendet werden können.
>
> Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="set-up-your-production-environment"></a>Richten Sie Ihre produktionsumgebung ##

>[AZURE.NOTE] In diesem Lernprogramm verwendeten Skripts konfiguriert automatisch fortlaufende Veröffentlichung von GitHub-Repository. Dies erfordert, dass Anmeldedaten GitHub sind bereits in Azure, andernfalls skriptbasierte Bereitstellung fehl beim Source Control für Web-apps konfigurieren. 
>
>Erstellen Sie zum Speichern Ihrer Anmeldeinformationen GitHub in Azure Web app in [Azure-Portal](https://portal.azure.com/) und [GitHub Bereitstellung konfigurieren](app-service-continuous-deployment.md). Sie müssen nur einmal erforderlich. 

In einer Situation DevOps eine Anwendung, die aktiv in Azure ausgeführt wird und Sie durch kontinuierliche Veröffentlichung ändern möchten. In diesem Szenario müssen Sie eine Vorlage, die entwickelt, getestet und zur Bereitstellung dieser Umgebung. Sie können sie in diesem Abschnitt einrichten.

1.  Erstellen Sie eigener Verzweigung des Repositorys [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) . Informationen zum Erstellen Ihrer Gabel anzeigen Sie [Gabel Repo](https://help.github.com/articles/fork-a-repo/) Nach Erstellung der Verzweigung es im Browser angezeigt.
 
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Git Shell-Sitzung zu öffnen. Wenn Sie Git Shell noch nicht, installieren Sie jetzt [GitHub für Windows](https://windows.github.com/) .

3.  Klonen Sie lokalen Gabel durch den folgenden Befehl ausführen:

        git clone https://github.com/<your_fork>/ToDoApp.git 

4.  Haben Sie Ihre lokalen Klon erstellen, navigieren Sie zu * &lt;Repository_root >*\ARMTemplates und die deploy.ps1 Skript wie folgt ausführen:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git

4.  Wenn Sie aufgefordert werden, geben Sie den gewünschten Benutzernamen und Kennwort für den Zugriff auf.

    Bereitstellung Fortschritt der verschiedenen Azure Ressourcen sollte angezeigt werden. Nach Abschluss der Bereitstellung wird das Skript starten Sie die Anwendung im Browser und Ihnen angezeigten Signalton.

    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
 
    >[AZURE.TIP] Schauen Sie sich * &lt;Repository_root >*\ARMTemplates\Deploy.ps1 zu sehen, wie Ressourcen mit eindeutigen IDs generiert. Den gleichen Ansatz können zur Erstellung von Clones von derselben Bereitstellung ohne Konflikt Ressourcennamen.
 
6.  Wieder in der Git Shell-Sitzung ausgeführt:

        .\swap –Name ToDoApp<unique_string>master

    ![](./media/app-service-agile-software-development/production-4-swap.png)

7.  Nach Abschluss des Skripts auf die Front-End-Adresse zurück (http://ToDoApp*&lt;Unique_string >*master.azurewebsites.net/) an die Anwendung in der Produktion.
 
5.  [Azure-Portal](https://portal.azure.com/) melden Sie an und einen Blick auf erstellten.

    Sie sollen finden zwei Web-apps in derselben Ressourcengruppe, mit der `Api` Suffix im Namen. Betrachten Sie die Ressourcenansicht Gruppe, sehen Sie auch die SQL-Datenbank und Server App Service-Plan und staging-Steckplätze für Web-apps. Die verschiedenen Ressourcen durchsuchen und vergleichen Sie sie mit * &lt;Repository_root >*\ARMTemplates\ProdAndStage.json finden Sie unter Konfiguration in der Vorlage.

    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

Sie haben jetzt die produktionsumgebung eingerichtet. Anschließend werden Sie ein neues Update der Anwendung beginnen.

## <a name="create-dev-and-test-branches"></a>Erstellen Sie Entwickler und Testen Sie Zweige ##

Jetzt haben Sie eine komplexe Anwendung, die als Produktionssysteme in Azure machen Sie ein Update der Anwendung nach agile Methodik. In diesem Abschnitt die Entwickler erstellen und Testen Sie die erforderlichen Updates müssen Zweige.

1.  Erstellen Sie zuerst die Umgebung. Führen Sie die folgenden Befehle zum Erstellen der Umgebung für einen neuen Zweig bezeichnet **NewUpdate**in der Git Shell-Sitzung. 

        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate

1.  Wenn Sie aufgefordert werden, geben Sie den gewünschten Benutzernamen und Kennwort für den Zugriff auf. 

    Nach Abschluss der Bereitstellung wird das Skript starten Sie die Anwendung im Browser und Ihnen angezeigten Signalton. Und schon jetzt einen neuen Zweig mit eigenen Umgebung. Betrachten Sie wir ein paar Dinge über diese Umgebung überprüfen:

    -   Sie können das in Azure-Abonnement erstellen. Das bedeutet, dass Produktionsumfeld separat von der Umgebung verwaltet werden kann.
    -   Die Umgebung wird aktiv in Azure ausgeführt.
    -   Ihre Umgebung entspricht Produktionsumfeld außer staging Steckplätze und die Skalierung. Sie können dies wissen, da sind die einzigen Unterschiede zwischen ProdandStage.json und Dev.json.
    -   Sie können eigene App Service-Plan mit einer anderen (z. B. **frei**) Ihre Umgebung verwalten.
    -   Löschen diese Umgebung werden so einfach wie die Ressourcengruppe löschen. Sie werden diese [später](#delete)erfahren.

2.  Fahren Sie mit eine Verzweigung Dev erstellen, indem Sie die folgenden Befehle ausführen:

        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev

3.  Wenn Sie aufgefordert werden, geben Sie den gewünschten Benutzernamen und Kennwort für den Zugriff auf. 

    Betrachten Sie wir ein paar Dinge über diese Dev Umgebung überprüfen: 

    -   Dev-Umgebung hat eine Konfiguration der Umgebung mit, da es mit der gleichen Vorlage bereitgestellt wird.
    -   Azure Entwickler Abonnement aus der Umgebung separat verwaltet werden kann jeder Entwicklungsumgebung erstellt werden.
    -   Dev-Umgebung wird aktiv in Azure ausgeführt.
    -   Dev-Umgebung löschen ist so einfach wie die Ressourcengruppe löschen. Sie werden diese [später](#delete)erfahren.

>[AZURE.NOTE] Bei arbeiten mehrere Entwickler an das neue Update kann jeweils eine Verzweigung und dedizierte Dev Umgebung einfach folgendermaßen erstellen:
>
>1. Erstellen Sie eigene Verzweigung des Repositorys in GitHub (siehe [Verzweigung Repo](https://help.github.com/articles/fork-a-repo/)).
>2. Die Verzweigung auf ihren lokalen Computer kopieren
>3. Führen Sie die gleichen Befehle zum Erstellen von eigenen Verzweigung Dev und Umgebung.

Wenn Sie fertig sind, müssen die GitHub-Verzweigung drei:

![](./media/app-service-agile-software-development/test-1-github-view.png)

Und haben Sie sechs webapps (drei Sätze von zwei) in drei separaten Ressourcengruppen:

![](./media/app-service-agile-software-development/test-2-all-webapps.png)
 
>[AZURE.NOTE] Beachten Sie, dass ProdandStage.json die produktionsumgebung gibt um **Standard** Tarif für Skalierbarkeit produktionsanwendung verwenden.

## <a name="build-and-test-every-commit"></a>Erstellen Sie und Testen Sie jede commit ##

Vorlagendateien, ProdAndStage.json und Dev.json geben bereits Steuerungsparameter Quelle standardmäßig richtet kontinuierlichen für Web app veröffentlichen. Daher löst jedes Commit GitHub Verzweigung eine automatische Bereitstellung in Azure aus einem Zweig. Mal sehen, wie Ihr Setup jetzt funktioniert.

1.  Stellen Sie sicher, dass in der Verzweigung Dev lokalen Repository befinden. Hierzu führen Sie den folgenden Befehl in Git Shell:

        git checkout Dev

2.  Ändern Sie einfacher UI-Ebene der Anwendung durch den Code [Bootstrap](http://getbootstrap.com/components/) Listen ändern. Open * &lt;Repository_root >*\src\MultiChannelToDo.Web\index.cshtml und unten hervorgehobenen Änderung vornehmen:

    ![](./media/app-service-agile-software-development/commit-1-changes.png)

    >[AZURE.NOTE] Wenn Sie das Bild oben lesen können: 
    >
    >- Ändern Sie in Zeile 18, `check-list` , `list-group`.
    >- Ändern Sie in Zeile 19 `class="check-list-item"` , `class="list-group-item"`.

3.  Speichern Sie die Änderung. Wieder in der Git Schale, führen Sie die folgenden Befehle ein:

        cd <repository_root>
        git add .
        git commit -m "changed to bootstrap style"
        git push origin Dev
 
    Diese Befehle Git ähneln "Überprüfung im Code" in einem anderen Quellcodeverwaltungssystem wie TFS. Beim Ausführen von `git push`, neue Commit löst einen automatische Push, Azure, dann die Anwendung erstellt entsprechend die Änderung in der Entwicklung.

4.  Um sicherzustellen, dass dieser Code Push für Ihre Umgebung Dev aufgetreten ist, gehen Sie zu Ihrer Umgebung Dev Web app Blade und die **Bereitstellung** Teil betrachten. Sie sollten Ihre neueste Commit Nachricht anzeigen können.

    ![](./media/app-service-agile-software-development/commit-2-deployed.png)

5.  Klicken Sie auf **Durchsuchen** , um die neue Änderung in der aktiven Anwendung in Azure finden dort.

    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)

    Dies ist eine relativ geringfügige Änderung zur Anwendung. Oft Änderungen, eine komplexe Anwendung hat unbeabsichtigte und unerwünschte Nebeneffekte. Können einfach jedes Commit live Builds testen kann diese Probleme erkennen, bevor Ihre Kunden sehen.

Jetzt sollten Sie mit wohl, als Entwickler am Projekt **NewUpdate** , Sie einfach schaffen, Entwickler werden und jedes Commit erstellen und Testen jeder Erstellung.

## <a name="merge-code-into-test-environment"></a>Umgebung Code zusammenführen ##

Nun müssen Code aus bis zu NewUpdate Verzweigung Dev wird standard Git Prozess:

1.  Zusammenführen Sie alle neuen verpflichtet NewUpdate in der Verzweigung Dev in GitHub, wie von anderen Entwicklern erstellte Commits. Alle neuen Commit auf GitHub Tastendruck Code ausgelöst und in Dev-Umgebung erstellen. Können stellen sicher, dass der Code in der Verzweigung Dev mit den neuesten Bits aus NewUpdate funktioniert.

2.  NewUpdate Verzweigung auf GitHub alle neuen Commits von Verzweigung Dev zusammengeführt. Diese Aktion löst ein Code Push und Build in der Umgebung. 

Beachten Sie, dass da fortlaufende Bereitstellung bereits mit diesen Zweigstellen Git ist, müssen Sie nicht aktiv wie mit Integration erstellt. Einfach durchzuführenden standard Source Control Vorgehensweisen Git und Azure führt alle Buildprozesse für Sie.

Jetzt drücken wir Code **NewUpdate** Zweig. Führen Sie in der Git Schale die folgenden Befehle:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

Das wars! 

Web app Blade für Ihre Umgebung an Ihre neuen Commit (NewUpdate Verzweigung zusammengeführt) jetzt in der Umgebung verschoben werden. Klicken Sie auf **Durchsuchen** , um, dass die Formatänderung jetzt live in Azure ausgeführt wird.

## <a name="deploy-update-to-production"></a>Update für die Produktion bereitstellen ##

Drücken Code für die Staging und Produktion sollten können dem, was Sie bereits getan, wenn Code in der Umgebung verschoben. Es ist wirklich so einfach. 

Führen Sie in der Git Schale die folgenden Befehle:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Basierend auf die Umgebung Staging und Produktion Einrichtung in ProdandStage.json ist daran, neue Code **Staging** -Steckplatz abgelegt und dort ausgeführt wird. So navigieren staging-Steckplatz URL den neuen Code es ausgeführt angezeigt werden. Führen Sie dazu die `Show-AzureWebsite` Cmdlet in der Git Schale.

    Show-AzureWebsite -Name ToDoApp<unique_string>master -Slot Staging
 
Und jetzt, nachdem Sie das Update im staging-Steckplatz überprüft haben, nur noch dazu in Betrieb ausgetauscht. Führen Sie in der Git Schale einfach die folgenden Befehle:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Herzlichen Glückwunsch! Sie haben erfolgreich ein neues Update der Produktion Web-Anwendung veröffentlicht. Was ist nur haben Entwickler einfach erstellen und Testen Umgebung erstellen und Testen jede Commit. Diese sind entscheidend für die agile Softwareentwicklung.

<a name="delete"></a>
## <a name="delete-dev-and-test-enviroments"></a>Dev löschen und Umgebungen testen ##

Da Ihre Umgebung Entwicklungs- und eigenständige Ressourcengruppen werden absichtlich entworfen haben, ist es sehr einfach löschen. Zum Löschen erstellt Sie in diesem Lernprogramm die GitHub Zweige und Azure Artefakte, führen Sie einfach die folgenden Befehle in der Git Schale:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Zusammenfassung ##

Agile Software Development ist ein muss für viele Unternehmen, die Azure als Anwendungsplattform erlassen. In diesem Lernprogramm haben Sie gelernt, wie erstellen und exakte Replikate oder nahe Replikate der produktionsumgebung problemlos selbst für komplexe Applikationen. Sie haben auch gelernt, nutzen Sie die Möglichkeit, einen zu erstellen, erstellen und Testen jeder einzelnen Commit in Azure. Dieses Lernprogramm zeigt hoffentlich, beste Verwendung Azure App Service und Azure-Ressourcen-Manager zusammen eine DevOps Lösung erstellen, die agilen Methodiken bietet. Anschließend erstellen Sie in diesem Szenario durch erweiterte DevOps Techniken wie [Produktion Tests](app-service-web-test-in-production-get-start.md)ausführen. Ein häufiges Szenario testen-Produktion finden Sie unter [Flighting-Bereitstellung (beta-Tests) in Azure App Service](app-service-web-test-in-production-controlled-test-flight.md).

## <a name="more-resources"></a>Weitere Ressourcen ##

-   [Bereitstellen einer komplexen Anwendung vorhersehbar in Azure](app-service-deploy-complex-application-predictably.md)
-   [Agile Entwicklung in der Praxis: Tipps und Tricks für modernisierte Entwicklungszyklus](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
-   [Erweiterte Bereitstellungsstrategien für Azure Web Apps mit Ressourcen-Manager-Vorlagen](http://channel9.msdn.com/Events/Build/2015/2-620)
-   [Azure Resource Manager Vorlagen erstellen](../resource-group-authoring-templates.md)
-   [JSONLint - die JSON-Bestätigung](http://jsonlint.com/)
-   [ARMClient – GitHub Veröffentlichung Website einrichten](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
-   [Git Verzweigen – grundlegende Verzweigen und Zusammenführen](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [David Ebbo Blog](http://blog.davidebbo.com/)
-   [Azure PowerShell](../powershell-install-configure.md)
-   [Azure plattformübergreifende Befehlszeilentools](../xplat-cli-install.md)
-   [Benutzer erstellen oder Bearbeiten in Azure AD](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
-   [Projekt Kudu Wiki](https://github.com/projectkudu/kudu/wiki)
