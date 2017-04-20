<properties
    pageTitle="Kontinuierliche Bereitstellung mit Visual Studio Team Services in Azure | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren von Visual Studio Team Services Teamprojekte automatisch erstellen und Web App Feature in Azure App Service oder Cloud-Dienste bereitstellen."
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

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>Kontinuierliche Bereitstellung in Azure mithilfe von Visual Studio Team Services

Sie können Visual Studio Team Services Teamprojekte automatisch erstellen und Bereitstellen von Azure Web Apps oder cloud-Dienste konfigurieren.  (Informationen zum Einrichten eines fortlaufenden Builds und Bereitstellen System mit einem *lokalen* Team Foundation Server finden Sie auf der [Kontinuierlichen Bereitstellung Cloud-Dienste in Azure](cloud-services-dotnet-continuous-delivery.md).)

Es wird vorausgesetzt, dass Sie Visual Studio 2013 und Azure SDK installiert. Haben Sie bereits Visual Studio 2013, wählen Sie den Link **kostenlos gestartet** [www.visualstudio.com](http://www.visualstudio.com)herunterladen. Installieren Sie das Azure SDK [hier](http://go.microsoft.com/fwlink/?LinkId=239540).

> [AZURE.NOTE]Benötigen Sie ein Konto Visual Studio Team Services zum Bearbeiten dieses Lernprogramms: Sie können [Konto Visual Studio Team Services kostenlos](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Gehen Sie folgendermaßen vor, um einen Cloud-Dienst automatisch erstellen und Azure mithilfe von Visual Studio Team Services bereitstellen.

## <a name="1-create-a-team-project"></a>1: Erstellen eines Teamprojekts

Gehen Sie [hier](http://go.microsoft.com/fwlink/?LinkId=512980) Teamprojekt erstellen und verknüpfen Sie es mit Visual Studio. In dieser exemplarischen Vorgehensweise wird vorausgesetzt, dass Sie Team Foundation Version Control (TFVC) als Quelle Steuerelement Lösung verwenden. Git für die Versionskontrolle verwenden, finden Sie unter [Git Version dieser exemplarischen Vorgehensweise](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-to-source-control"></a>2: Einchecken eines Projekts zur Versionskontrolle

1. Öffnen Sie in Visual Studio die Projektmappe bereitstellen möchten oder erstellen Sie eine neue.
Die Schritte in dieser exemplarischen Vorgehensweise können Sie eine Webanwendung oder einem Clouddienst (Azure-Anwendung) bereitstellen.
Möchten Sie eine neue Projektmappe erstellen, erstellen Sie ein neues Azure Cloud Service-Projekt oder ein neues ASP.NET MVC-Projekt. Stellen Sie sicher, dass das Projekt auf.NET Framework 4 oder 4.5 abzielt, und wenn ein Cloud-Dienstprojekt erstellen, fügen Sie ein ASP.NET MVC Webrolle und eine Worker-Rolle und Internet-Anwendung für die Web-Rolle. Schreiben Sie **Internet-Anwendung**.
Wenn Sie eine Web app erstellen möchten, wählen Sie die Projektvorlage ASP.NET Web-Anwendung, und dann MVC. Siehe [Erstellen einer ASP.NET Web-Anwendung in Azure App Service](../app-service-web/web-sites-dotnet-get-started.md).

    > [AZURE.NOTE] Visual Studio Team Services unterstützen zurzeit nur CI Bereitstellung von Visual Studio Web Applications. Websiteprojekte gehören.

1. Das Kontextmenü für die Projektmappe, und wählen Sie **Lösung zur Versionskontrolle hinzufügen**.

    ![][5]

1. Akzeptieren oder ändern Sie die Standardeinstellungen und die Schaltfläche **OK** . Nach Abschluss des Vorgangs Quellcodeverwaltungssymbole im **Projektmappen-Explorer**angezeigt.

    ![][6]

1. Öffnen Sie das Kontextmenü für die Projektmappe, und wählen Sie **Einchecken**.

    ![][7]

1. Geben Sie einen Kommentar für die Bereich **Pending Changes** **Team Explorer**und wählen Sie die Schaltfläche **Einchecken** .

    ![][8]

    Beachten Sie die Optionen zum ein- oder Ausschließen von Änderungen beim Einchecken. Wenn Änderungen ausgeschlossen werden, **Enthalten alle** Link auswählen

    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: Verbinden Sie das Projekt in Azure

1. Nachdem ein Teamprojekt VS Team Services mit Quellcode enthalten, können Sie Azure Teamprojekt herstellen.  Wählen Sie Ihre Cloud-Dienst oder Web app im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)oder eine neue erstellen durch Auswählen der **+** Symbol in der unteren linken und **Cloud-Dienst** oder **Web App** und dann **Schnell erstellen**. Wählen Sie den Link **Veröffentlichen mit Visual Studio Team Services einrichten** .

    ![][10]

1. Geben Sie im Assistenten den Namen Ihres Kontos Visual Studio Team Services in das Textfeld ein und klicken Sie auf den Link **Jetzt autorisieren** . Möglicherweise müssen Sie anmelden.

    ![][11]

1. Wählen Sie im Popup-Dialogfeld **Verbindung anfordern** **annehmen** ermächtigen Azure Teamprojekt VS Team Services konfigurieren.

    ![][12]

1. Bei erfolgreicher Autorisierung wird eine Dropdownliste mit Visual Studio Team Services Teamprojekte angezeigt. Wählen Sie den Namen des Teamprojekts, die in den vorherigen Schritten erstellt, und dann den Assistenten Kontrollkästchen.

    ![][13]

1. Nachdem das Projekt verknüpft ist, sehen Sie einige Hinweise zum Einchecken des Projekts Visual Studio Team Services Team.  Bei Ihrem nächsten Einchecken wird Visual Studio Team Services erstellen und Projekt in Azure bereitstellen.  Versuchen Sie dies nun durch Klicken auf den Link **Aus Visual Studio** und dann Link **Visual Studio starten** (oder entsprechende **Visual Studio** -Schaltfläche am unteren Bildschirmrand Portal).

    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: Rebuild auslösen und das Projekt erneut bereitstellen

1. Wählen Sie in Visual Studio **Team Explorer**den **Quellcodeverwaltungs-Explorer** Link.

    ![][15]

1. Die Projektmappendatei navigieren und öffnen.

    ![][16]

1. Im **Projektmappen-Explorer**eine Datei öffnen und ändern. Beispielsweise ändern die Datei `_Layout.cshtml` unter Ansichten\\freigegebene Ordner in einer Webrolle MVC.

    ![][17]

1. Bearbeiten Sie das Logo der Website, und wählen Sie **STRG + S** zum Speichern der Datei.

    ![][18]

1. Wählen Sie in **Team Explorer**Link **Pending Changes** .

    ![][19]

1. Geben Sie einen Kommentar ein, und wählen Sie dann die Schaltfläche **Einchecken** .

    ![][20]

1. Wählen Sie **Home** **Team Explorer** -Startseite zurückzukehren.

    ![][21]

1. Wählen Sie den Link **erstellt** , an die Builds ausgeführt.

    ![][22]

    **Team Explorer** zeigt, dass für Ihren Build ausgelöst wurde.

    ![][23]

1. Doppelklicken Sie auf den Namen des Builds zu detailliert als Buildstatuswerte angezeigt.

    ![][24]

1. Der Build in Bearbeitung ist, betrachten Sie die Builddefinition TFS in Azure verknüpft, mithilfe des Assistenten erstellt wurde.  Öffnen Sie das Kontextmenü für die Builddefinition, und wählen Sie **Builddefinition bearbeiten**.

    ![][25]

    Auf der Registerkarte **Trigger** sehen die Builddefinition standardmäßig ist auf jedem Einchecken.

    ![][26]

    Auf der Registerkarte **Prozess** finden Sie unter Deployment-Umgebung den Namen der Cloud-Dienst oder Web app festgelegt ist. Wenn Sie Web-apps arbeiten, werden die angezeigten Eigenschaften anders hier.

    ![][27]

1. Geben Sie Werte für die Eigenschaften ggf. andere Werte als die Standardwerte. Die Eigenschaften für Azure veröffentlichen sind im Abschnitt **Bereitstellung** .

    Die folgende Tabelle zeigt die verfügbaren Eigenschaften im Abschnitt **Bereitstellung** :

  	|Eigenschaft|Standardwert|
  	|---|---|
  	|Nicht vertrauenswürdige Zertifikate zulassen|Wenn false, müssen eine Stammzertifizierungsstelle Zertifikate unterzeichnet.|
  	|Aktualisierung zulassen|Ermöglicht die Bereitstellung eine vorhandene Bereitstellung anstatt eine neue zu aktualisieren. Die IP-Adresse erhält.|
  	|Nicht löschen|True überschreiben eine vorhandene unabhängige Bereitstellung nicht (Aktualisierung zulässig).|
  	|Pfad zum Deployment Settings|Der Pfad zur Datei .pubxml für eine Webanwendung relativ zum Stammordner des Repo. Cloud-Dienste ignoriert.|
  	|SharePoint Deployment-Umgebung|Der Dienstnamen identisch.|
  	|Azure Deployment-Umgebung|Namen des Webdienstes app oder Cloud.|

1. Bei Verwendung mehrerer Dienstkonfigurationen (.cscfg-Dateien) können Sie die gewünschte Konfiguration der Einstellung **erstellen, erweitert MSBuild-Argumente** angeben. Beispielsweise um ServiceConfiguration.Test.cscfg verwenden zu können, legen Sie MSBuild-Argumente Befehlszeilenoption `/p:TargetProfile=Test`.

    ![][38]

    Zu diesem Zeitpunkt sollte der Build erfolgreich abgeschlossen.

    ![][28]

1. Wenn Sie Build-Namen doppelklicken, zeigt Visual Studio eine **Buildzusammenfassung**mit Testergebnissen aus zugehörigen Projekte.

    ![][29]

1. Im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)sehen Sie zugeordnete Bereitstellung auf der Registerkarte **Bereitstellung** beim staging-Umgebung ausgewählt ist.

    ![][30]

1.  Wechseln Sie zu der Website-URL. Klicken Sie für eine Webanwendung auf die Schaltfläche **Durchsuchen** auf der Befehlsleiste. Wählen Sie die URL für einen Clouddienst im Abschnitt **Kurzer Blick** der **Dashboard** -Seite, die die Staging-Umgebung für einen Clouddienst zeigt. Bereitstellung von fortlaufende Integration für Cloud-Dienste werden standardmäßig Staging-Umgebung veröffentlicht. Sie können diese Einstellung **Alternative Cloud Service-Umgebung** zur **Produktion**ändern. Dieses Bildschirmabbild zeigt ist die URL für den Cloud-Dienst Dashboardseite.

    ![][31]

    Eine neue Registerkarte wird geöffnet, um Ihre laufenden Site anzuzeigen.

    ![][32]

    Cloud-Dienste Wenn Sie andere zum Projekt Änderungen Sie Trigger mehr erstellt und Sammeln Sie mehrere Bereitstellungen. Aktuell als aktiv gekennzeichnet.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: erneut einen früheren Build

Dieser Schritt gilt für Cloud-Services und ist optional. Wählen Sie im klassischen Azure-Portal eine Bereitstellung und wählen Sie dann **erneut** auf Ihrer Site eine frühere Einchecken zurückspulen.  Einem neuen Build in TFS und einen neuen Eintrag in Ihrem Verlauf der Bereitstellung erstellt beachten.

![][34]

## <a name="6-change-the-production-deployment"></a>6: ändern die produktionsbereitstellung

Dieser Schritt gilt nur für Cloud-Services nicht webapps. Wenn Sie bereit sind, können Sie die Staging-Umgebung zur Produktionsumfeld heraufstufen, wählen Sie die Schaltfläche **Swap** im klassischen Azure-Portal. Neu bereitgestellte Staging-Umgebung zur Produktion heraufgestuft und vorherigen Produktionsumfeld, wird eine Staging-Umgebung. Möglicherweise aktive Bereitstellung für die Produktion und Staging-Umgebungen, aber die Bereitstellung letzten Builds ist gleich Umgebung.

![][35]

## <a name="7-run-unit-tests"></a>7: Ausführen von Komponententests

Dieser Schritt gilt nur für web-apps, cloud-Dienste nicht. Um eine Qualitätsstufe auf die Bereitstellung Komponententests ausführen und wenn sie fehlschlagen können, halten Sie die Bereitstellung.

1.  Fügen Sie in Visual Studio ein Komponententestprojekt.

    ![][39]

1.  Fügen Sie Projektverweise hinzu, die Sie testen möchten.

    ![][40]

1.  Einigen Komponententests hinzufügen. Versuchen Sie zunächst einen dummy Test immer übergeben werden.

        ```
        using System;
        using Microsoft.VisualStudio.TestTools.UnitTesting;

        namespace UnitTestProject1
        {
            [TestClass]
            public class UnitTest1
            {
                [TestMethod]
                [ExpectedException(typeof(NotImplementedException))]
                public void TestMethod1()
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1.  Bearbeiten Sie die Builddefinition, wählen Sie die Registerkarte **Prozess** und erweitern Sie **den Knoten** .

1.  **Buildfehler bei Testfehler** auf True festgelegt. Dies bedeutet, dass die Bereitstellung durchgeführt wird, wenn die Tests bestanden.

    ![][41]

1.  Neuen Build in die Warteschlange.

    ![][42]

    ![][43]

1. Während des Builds verläuft, überprüfen Sie den Fortschritt.

    ![][44]

    ![][45]

1. Nach Abschluss des Builds Überprüfen der Testergebnisse.

    ![][46]

    ![][47]

1.  Probieren Sie einen Test fehl. Fügen Sie einen neuen Test zuerst kopieren, Umbenennen Sie und kommentieren Sie die Codezeile, die besagt, dass NotImplementedException eine erwartete Ausnahme ist.

        ```
        [TestMethod]
        //[ExpectedException(typeof(NotImplementedException))]
        public void TestMethod2()
        {
            throw new NotImplementedException();
        }
        ```

1. Überprüfen Sie im Feld ändern in einen neuen Build in die Warteschlange.

    ![][48]

1. Zeigen Sie die Testergebnisse für Informationen über den Fehler an.

    ![][49]

    ![][50]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über Komponententests in Visual Studio Team Services finden Sie unter [Ausführen von Komponententests im Build](http://go.microsoft.com/fwlink/p/?LinkId=510474). Wenn Sie Git verwenden, finden Sie unter [Freigabe Code Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) und [kontinuierliche Bereitstellung Azure App Service](../app-service-web/app-service-continuous-deployment.md).  Weitere Informationen zu Visual Studio Team Services finden Sie unter [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
