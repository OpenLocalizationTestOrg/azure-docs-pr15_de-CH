<properties
    pageTitle="Kontinuierliche Bereitstellung Git mit Visual Studio Team Services in Azure | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren von Visual Studio Team Services Teamprojekte um Git automatisch erstellen und mit der Funktion Web App in Azure App Service oder Cloud-Dienste bereitstellen."
    services="cloud-services"
    documentationCenter=".net"
    authors="mlearned"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="mlearned"/>

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>Kontinuierliche Bereitstellung in Visual Studio Team Services mit Git Azure

Visual Studio Team Services Teamprojekte können host Git Repository für Quellcode automatisch erstellen und Azure webapps bereitstellen und cloud-Services bei jedem einen Commit in das Repository drücken.

Sie benötigen Visual Studio 2013 und Azure SDK installiert. Haben Sie bereits Visual Studio 2013, wählen Sie den Link **kostenlos gestartet** [www.visualstudio.com](http://www.visualstudio.com)herunterladen. Installieren Sie das Azure SDK [hier](http://go.microsoft.com/fwlink/?LinkId=239540).


> [AZURE.NOTE]Benötigen Sie ein Konto Visual Studio Team Services zum Bearbeiten dieses Lernprogramms: Sie können [Konto Visual Studio Team Services kostenlos](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Gehen Sie folgendermaßen vor, um einen Cloud-Dienst automatisch erstellen und Azure mithilfe von Visual Studio Team Services bereitstellen.

## <a name="1-create-a-git-repository"></a>1: Erstellen eines Repositorys Git

1. Wenn Sie bereits mit Visual Studio Team Services-Konto haben, erhalten Sie eine [hier](http://go.microsoft.com/fwlink/?LinkId=397665). Wenn Sie das Teamprojekt erstellen, wählen Sie als Quellcodeverwaltungssystem Git. Folgen Sie zum Visual Studio mit dem Teamprojekt herstellen.

2. Wählen Sie in **Team Explorer** **Klonen dieses Repository** verknüpfen.

    ![][3]

3. Geben Sie den Speicherort der lokalen Kopie aus, und klicken Sie dann auf **Clone** .

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2: Erstellen eines Projekts und zum Repository commit

1. Wählen Sie in **Team Explorer**im Abschnitt **Solutions** **neu** Link zum Erstellen eines neuen Projekts im lokalen Repository aus

    ![][4]

2. Die Schritte in dieser exemplarischen Vorgehensweise können Sie eine Webanwendung oder einem Clouddienst (Azure-Anwendung) bereitstellen. Erstellen Sie ein neues Azure Cloud Service-Projekt oder ein neues ASP.NET MVC-Projekt. Stellen Sie sicher, dass das Projekt auf.NET Framework 4 abzielt oder höher. Wenn Sie ein Cloud-Dienstprojekt erstellen, Hinzufügen einer Webrolle ASP.NET MVC und eine Worker-Rolle.
Wenn Sie eine Web app erstellen möchten, wählen Sie die Projektvorlage **ASP.NET Web-Anwendung** , und dann **MVC**. Weitere Informationen finden Sie unter [Erstellen einer ASP.NET Web-Anwendung in Azure App Service](../app-service-web/web-sites-dotnet-get-started.md) .

3. Öffnen Sie das Kontextmenü für die Projektmappe, und wählen Sie **Commit**.

    ![][7]

4. Ist dies das erste Mal Git in Visual Studio Team Services verwendet haben, müssen Sie einige Informationen zur Identifikation Git. Geben Sie im Bereich **Pending Changes** **Team Explorer**Ihren Benutzernamen und e-Mail-Adresse. Geben Sie einen Kommentar für das Festschreiben, und klicken Sie dann auf **Commit** .

    ![][8]

5. Beachten Sie die Optionen zum ein- oder Ausschließen von Änderungen beim Einchecken. Wählen Sie die Änderungen ausgeschlossen, **Alle enthalten**.

6. Sie haben jetzt die Änderungen in der lokalen Kopie des Repository verpflichtet. Synchronisieren Sie die Änderungen mit dem Server anschließend durch den **Sync** -Link auswählen.

## <a name="3-connect-the-project-to-azure"></a>3: Verbinden Sie das Projekt in Azure

1. Jetzt haben Sie ein Git Repository in Visual Studio Team Services mit Quellcode darin können Sie Azure Git-Repository herstellen.  Wählen Sie Ihre Cloud-Dienst oder Web app im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)oder eine neue erstellen durch Auswählen der + Symbol in der unteren linken und **Cloud-Dienst** oder **Web App** und dann **Schnell erstellen**.

    ![][9]

3. Wählen Sie für Cloud-Services den Link **Veröffentlichen mit Visual Studio Team Services einrichten** . Webapps wählen Sie den Link **Einrichten der Bereitstellung vom Datenquellen-Steuerelement** .

    ![][10]

2. Geben Sie im Assistenten den Namen Ihres Kontos Visual Studio Team Services in das Textfeld und wählen Sie den Link **Jetzt autorisieren** . Möglicherweise müssen Sie anmelden.

    ![][11]

3. Wählen Sie im Popup-Dialogfeld **Verbindung anfordern** , **akzeptieren** ermächtigen Azure Teamprojekt in Visual Studio Team Services konfigurieren.

    ![][12]

4. Nach erfolgreicher Autorisierung eine Dropdown-Liste mit Visual Studio Team Services Teamprojekte angezeigt.  Wählen Sie den Namen des Teamprojekts, das Sie im vorherigen Schritt erstellt haben, und Kontrollkästchen des Assistenten.

    ![][13]

    Nächsten Commit an das Repository übertragen wird Visual Studio Team Services erstellen und Bereitstellen von Projekt in Azure.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: Rebuild auslösen und das Projekt erneut bereitstellen

1. In Visual Studio eine Datei öffnen und ändern. Beispielsweise ändern die Datei `_Layout.cshtml` unter Ansichten\\freigegebene Ordner in einer Webrolle MVC.

    ![][17]

2. Bearbeiten Sie Text in die Fußzeile für die Website und speichern Sie die Datei.

    ![][18]

3. Öffnen Sie im **Projektmappen-Explorer**im Kontextmenü für den Projektmappenknoten Projektknoten oder geänderten Datei, und wählen Sie **Commit**.

4. Geben Sie einen Kommentar, und wählen Sie **Übernehmen**.

    ![][20]

5. Wählen Sie den Link **Synchronisieren** .

    ![][38]

6. Wählen Sie den Link **Push** den Commit auf Visual Studio Team Services übertragen. (Sie können die Commits in das Repository kopiert auch die **Sync** -Taste. Der Unterschied ist, dass die **Synchronisierung** der letzten Änderung auch aus dem Repository abruft.)

    ![][39]

7. Wählen Sie **Home** **Team Explorer** -Startseite zurückzukehren.

    ![][21]

8. Wählen Sie bei Builds anzeigen **erstellt** .

    ![][22]

    **Team Explorer** zeigt, dass für Ihren Build ausgelöst wurde.

    ![][23]

9. Um eine ausführliche Protokolldatei während des Buildvorgangs anzuzeigen, doppelklicken Sie auf den Namen des Builds ausgeführt.

10. Der Build in Bearbeitung ist, betrachten Sie die Builddefinition erstellt wurde, wenn der Assistent zum Verknüpfen mit Azure verwendet.  Öffnen Sie das Kontextmenü für die Builddefinition, und wählen Sie **Builddefinition bearbeiten**.

    ![][25]

11. Auf der Registerkarte **Trigger** sehen die Builddefinition auf jedem Einchecken standardmäßig festgelegt ist. (Für einen Clouddienst Visual Studio Team Services erstellt und gibt den master Zweig in die Stagingumgebung automatisch. Sie haben noch ein manueller Schritt auf der Website bereitstellen. Für eine Webanwendung, die Stagingumgebung bereitgestellt master Zweig direkt auf der Website.

    ![][26]

1. Auf der Registerkarte **Prozess** finden Sie unter Deployment-Umgebung den Namen der Cloud-Dienst oder Web app festgelegt ist.

    ![][27]

1. Geben Sie Werte für die Eigenschaften ggf. andere Werte als die Standardwerte. Im Abschnitt **Bereitstellung** werden die Eigenschaften für Azure veröffentlichen und Sie müssen auch MSBuild-Parameter. Beispielsweise in der Cloud Dienstprojekt an einer Konfiguration als "Cloud" parametrisieren MSbuild `/p:TargetProfile=[YourProfile]` *[YourProfile]* entspricht eine Service-Konfigurationsdatei mit dem Namen ServiceConfiguration. *YourProfile*.cscfg.

    Die folgende Tabelle zeigt die verfügbaren Eigenschaften im Abschnitt **Bereitstellung** :

  	|Eigenschaft|Standardwert|
  	|---|---|
  	|Nicht vertrauenswürdige Zertifikate zulassen|Wenn false, müssen eine Stammzertifizierungsstelle Zertifikate unterzeichnet.|
  	|Aktualisierung zulassen|Ermöglicht die Bereitstellung eine vorhandene Bereitstellung anstatt eine neue zu aktualisieren. Die IP-Adresse erhält.|
  	|Nicht löschen|True überschreiben eine vorhandene unabhängige Bereitstellung nicht (Aktualisierung zulässig).|
  	|Pfad zum Deployment Settings|Der Pfad zur Datei .pubxml für eine Webanwendung relativ zum Stammordner des Repo. Cloud-Dienste ignoriert.|
  	|SharePoint Deployment-Umgebung|Der Dienstnamen identisch.|
  	|Azure Deployment-Umgebung|Namen des Webdienstes app oder Cloud.|

1. Zu diesem Zeitpunkt sollte der Build erfolgreich abgeschlossen.

    ![][28]

1. Wenn Sie Build-Namen doppelklicken, zeigt Visual Studio eine **Buildzusammenfassung**mit Testergebnissen aus zugehörigen Projekte.

    ![][29]

1. Im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)sehen Sie zugeordnete Bereitstellung auf der Registerkarte **Bereitstellung** beim staging-Umgebung ausgewählt ist.

    ![][30]

1.  Wechseln Sie zu der Website-URL. Wählen Sie für eine Webanwendung einfach die Schaltfläche **Durchsuchen** im Portal. Wählen Sie die URL für einen Clouddienst im Abschnitt **Kurzer Blick** der **Dashboard** -Seite, die die Staging-Umgebung veranschaulicht.

    Bereitstellung von fortlaufende Integration für Cloud-Dienste werden standardmäßig Staging-Umgebung veröffentlicht. Sie können diese Einstellung **Alternative Cloud Service-Umgebung** zur **Produktion**ändern. Hier ist die URL für den Cloud-Dienst Dashboardseite.

    ![][31]

    Eine neue Registerkarte wird geöffnet, um Ihre laufenden Site anzuzeigen.

    ![][32]

1.  Wenn Sie andere zum Projekt Änderungen Sie Trigger mehr erstellt und Sammeln Sie mehrere Bereitstellungen. Aktuell wird als aktiv markiert.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: erneut einen früheren Build

Dieser Schritt ist optional. Im klassischen Azure-Portal wählen Sie eine Bereitstellung und zurückspulen, die Site auf einen früheren Check-in **erneut** . Einem neuen Build in TFS und einen neuen Eintrag in Ihrem Verlauf der Bereitstellung erstellt beachten.

![][34]

## <a name="6-change-the-production-deployment"></a>6: ändern die produktionsbereitstellung

Wenn Sie bereit sind, können Sie durch Auswählen von **Swap** im klassischen Azure-Portal Staging Umgebung Produktionsumfeld heraufstufen. Neu bereitgestellte Staging-Umgebung zur Produktion heraufgestuft und vorherigen Produktionsumfeld, wird eine Staging-Umgebung. Möglicherweise aktive Bereitstellung für die Produktion und Staging-Umgebungen, aber die Bereitstellung letzten Builds ist gleich Umgebung.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: Bereitstellen von einer arbeitsverzweigung.

Bei Verwendung von Git Sie normalerweise Änderungen in einer arbeitsverzweigung und master Bereich erreicht die Entwicklung ein Fertigerzeugnis integrieren. Während der Entwicklungsphase eines Projekts sollten Sie erstellen und Bereitstellen der arbeitsverzweigung in Azure.

1. Wählen Sie in **Team Explorer**die Schaltfläche **Start** , und klicken Sie dann auf **Zweigniederlassungen** .

    ![][40]

2. Wählen Sie den Link **Niederlassung** .

    ![][41]

3. Geben Sie den Namen der Branche wie "Arbeit", und wählen Sie **Verzweigung erstellen**. Dies erstellt eine neue Filiale.

    ![][42]

4. Veröffentlichen Sie die Verzweigung. Wählen Sie die Zweigstelle in **unveröffentlichten Zweigen**und **Veröffentlichen**.

    ![][44]

6. Standardmäßig lösen nur Änderungen master Verzweigung einen fortlaufenden Build. Um fortlaufende Builds für eine arbeitsverzweigung einzurichten, wählen Sie die Seite **erstellt** in **Team Explorer**und **Builddefinition bearbeiten**.

7. Öffnen Sie die Registerkarte **Einstellungen** . Wählen Sie unter **überwachte verzweigt fortlaufende Integration zu erstellen** **Klicken Sie hier, um eine neue Zeile hinzuzufügen**.

    ![][47]

8. Geben Sie den Zweig, beispielsweise Refs/Köpfe/erstellt.

    ![][48]

9. Im Code ändern Sie, öffnen Sie das Kontextmenü für die geänderte Datei, und dann **Übernehmen**.

    ![][43]

10. Wählen Sie **Synchronisierte Commits** Link und die **Sync** -Taste oder **drücken** Link Änderungen auf die Kopie der arbeitsverzweigung in Visual Studio Team Services kopieren.

    ![][45]

11. Navigieren Sie zu der Ansicht **erstellt** und arbeitsverzweigung finden Sie erstellen, die nur ausgelöst haben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Tipps zur Verwendung von Git mit Visual Studio Team Services finden Sie unter [entwickeln und Code in Visual Studio mit Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) und Informationen über Git Repository, die nicht von Visual Studio Team Services Azure veröffentlichen verwaltet [Kontinuierliche Bereitstellung Azure App Service](../app-service-web/app-service-continuous-deployment.md). Weitere Informationen zu Visual Studio Team Services finden Sie unter [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
