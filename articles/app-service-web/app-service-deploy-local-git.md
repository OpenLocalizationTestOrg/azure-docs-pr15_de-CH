<properties
    pageTitle="Lokale Git Bereitstellung Azure App Service"
    description="Informationen Sie zum lokalen Git Azure App Service aktivieren."
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
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="local-git-deployment-to-azure-app-service"></a>Lokale Git Bereitstellung Azure App Service

Dieses Lernprogramm zeigt wie Ihre app [Azure App Service] von einem Git-Repository auf dem lokalen Computer bereitgestellt. App Service unterstützt diesen Ansatz mit **Lokalen Git** Bereitstellung in [Azure-Portal].  
Viele der in diesem Artikel beschriebenen Git Befehle werden automatisch ausgeführt, beim Erstellen einer App Service-app unter der [Azure-Befehlszeilenschnittstelle] beschrieben [hier](app-service-web-get-started.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm müssen wie folgt vor:

- Git. Sie können die Installation binäre [hier](http://www.git-scm.com/downloads)herunterladen.  
- Grundlegende Kenntnisse der Git.
- Ein Microsoft Azure-Konto. Haben Sie ein Konto, können Sie [sich für eine kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial) oder [die Visual Studio-Abonnementvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter-app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.  

## <a name="Step1"></a>Schritt 1: Erstellen Sie ein lokales repository

Führen Sie die folgenden Aufgaben zum Erstellen eines neuen Git Repository.

1. Starten Sie ein Befehlszeilenprogramm **GitBash** (Windows) oder **Bash** (Unix Shell). Auf OS X Systeme können Sie die Befehlszeile über **Terminal** -Anwendung zugreifen.

2. Navigieren Sie zu dem Verzeichnis, in dem der Inhalt bereitstellen befinden würde.

3. Verwenden Sie folgenden Befehl ein neues Git Repository initialisieren:

        git init

## <a name="Step2"></a>Schritt 2: Übernehmen der Inhalts

App-Dienst unterstützt die Anwendung in verschiedenen Programmiersprachen erstellt. 

1. Enthält Ihr Repository bereits Inhalt überspringen diese zeigen und Nummer 2 unten verschieben. Enthält Ihr Repository bereits Inhalt einfach keinen füllen Sie mit statischen HTML-Datei wie folgt: 

    - Mit einem Texteditor eine neue Datei namens **"Index.HTML"** im Stammverzeichnis der Git Repository erstellen
    - Fügen Sie den folgenden Text, und der Inhalt der Datei index.html Datei speichern: *Hallo Git!*
        
2. Überprüfen Sie in der Befehlszeile unter dem Stamm der Git-Repository sind. Dann verwenden Sie den folgenden Befehl, um Dateien zum Repository hinzufügen:

        git add -A 

4. Als Nächstes Änderungen Sie im Repository mit dem folgenden Befehl:

        git commit -m "Hello Azure App Service"

## <a name="Step3"></a>Schritt 3: Aktivieren Sie App Service app repository

Die folgenden Schritte ein Git Repository für Ihre App-Dienst aktivieren.

1. [Azure-Portal]anmelden.

2. Ihre App Service-app Blatt klicken Sie auf **Settings > bereitstellungsquelle**. Klicken Sie auf **Quelle auswählen**, klicken Sie auf **Lokale Git Repository**, und klicken Sie dann auf **OK**.  

    ![Git-Repository](./media/app-service-deploy-local-git/local_git_selection.png)

3. Ist dies der erste Systemzeit Repository in Azure, müssen Sie Anmeldeinformationen erstellen. Sie verwenden sie Azure Repository und Push ändert sich von Ihrem lokalen Git Repository anmelden. Wählen Sie Ihre app-Blade **Settings > Bereitstellung Anmeldeinformationen**, konfigurieren Sie Bereitstellung Benutzername und Kennwort. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Schritt 4: Projekt bereitstellen

Gehen Sie mit lokalen Git App Service Ihre app veröffentlichen.

1. Klicken Sie auf Blatt Ihrer Anwendung in Azure-Portal **Settings > Eigenschaften** **Git URL**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    **Git URL** ist Remotebezug von Ihrem lokalen Repository bereitstellen. Sie verwenden diese URL in den folgenden Schritten.

2. Verwenden der Befehlszeile, überprüfen Sie Sie im Stammverzeichnis des lokalen Git Repository.

3. Mit `git remote` remote Verweis hinzufügen aufgeführten **Git** URL aus Schritt 1. Der Befehl sieht etwa wie folgt:

        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
    > [AZURE.NOTE] Der **remote** -Befehl fügt einen benannten Verweis auf ein remote-Repository. In diesem Beispiel wird ein Verweis mit dem Namen "Azure" für Ihre Web-app Repository erstellt.

4. Drücken Sie Ihre Inhalte App-Dienst mithilfe der neuen **Azure** remote gerade erstellt.

        git push azure master

    Sie werden für das Kennwort aufgefordert zuvor erstellten beim Zurücksetzen der Bereitstellung Anmeldeinformationen im Azure-Portal. Geben Sie das Kennwort (Beachten Sie, dass Gitbash nicht Sternchen in der Konsole ausgeben, wie Ihr Kennwort). 
       
5. Für Ihre Anwendung in Azure-Portal zurück. Ein Protokolleintrag der neuesten Push sollte **Bereitstellung** Blatt angezeigt werden. 

    ![](./media/app-service-deploy-local-git/deployment_history.png)

6. Klicken Sie **Durchsuchen** oben Blatt der app überprüfen, ob der Inhalt bereitgestellt wurde. 
    
## <a name="Step5"></a>Problembehandlung

Im folgenden werden Fehler oder Probleme häufig Verwendung Git App Service-app in Azure veröffentlichen:

****

**Symptom**: kein Zugriff auf "SiteURL": Fehler beim Verbinden mit [ScmAddress]

**Ursache**: Dieser Fehler kann auftreten, wenn die Anwendung nicht ausgeführt wird.

**Lösung**: Starten Sie die Anwendung in Azure-Portal. Git Bereitstellung funktioniert nicht, wenn die Anwendung ausgeführt wird. 


****

**Symptom**: Host 'Hostname' konnte nicht aufgelöst werden

**Ursache**: Dieser Fehler kann auftreten, wenn die Erstellung der Azure remote eingegebene Informationen falsch war.

**Auflösung**: Verwenden der `git remote -v` Befehl Listen Sie alle Fernbedienungen mit zugeordneten URL. Überprüfen Sie die URL für den "Azure" remote. Falls erforderlich, entfernen und neu Fernbedienung die richtige URL verwenden.

****

**Symptom**: keine Verweise und keine angegeben; nichts zu tun. Geben Sie vielleicht eine Verzweigung wie 'Master'.

**Ursache**: Dieser Fehler kann auftreten, wenn Sie eine Verzweigung nicht angeben, wenn einen Git push-Vorgang und nicht push.default Wert verwendet Git festgelegt.

**Auflösung**: Vorgang der Push, master Verzweigung angeben. Zum Beispiel:

    git push azure master

****

**Symptom**: Src Refspec [Branchname] stimmt nicht überein.

**Ursache**: Dieser Fehler kann auftreten, wenn Sie versuchen, auf eine Verzweigung als Master auf den Azure remote drücken.

**Auflösung**: Vorgang der Push, master Verzweigung angeben. Zum Beispiel:

    git push azure master

****

**Symptom**: Fehler - Änderungen in remote-Repository aber Ihrer Anwendung nicht aktualisiert.

**Ursache**: Dieser Fehler kann auftreten, wenn eine Node.js-Anwendung mit einer package.json-Datei bereitstellen, erforderliche Zusatzmodule angibt.

**Auflösung**: zusätzliche Nachrichten mit "Npm ERR!" sollte protokollierten Fehler vor und bieten zusätzlichen Kontext für den Fehler. Folgende Ursachen für diesen Fehler und die entsprechenden "Npm ERR!" kennen Nachricht:

* **Ungültige package.json-Datei**: Npm ERR! Abhängigkeit konnte nicht gelesen werden.

* **Systemeigene Module, die eine binäre Verteilung für Windows nicht aufweisen**:

    * Npm Fehler! \`Cmd "/ C" "Gips Knoten neu erstellen"\` 1 fehlgeschlagen

        ODER

    * Npm Fehler! [modulename@version]Vorinstallieren: \`stellen || gmake\`


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Git Dokumentation](http://git-scm.com/documentation)
* [Kudu Dokumentation](https://github.com/projectkudu/kudu/wiki)
* [Kontinuierliche Bereitstellung Azure App Service](app-service-continuous-deployment.md)
* [Verwendung von PowerShell für Azure](../powershell-install-configure.md)
* [Verwendung der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md)

[Azure App Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure-Portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure Befehlszeilenschnittstelle]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
