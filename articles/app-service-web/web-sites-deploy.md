<properties
    pageTitle="Bereitstellen Sie Ihrer Anwendung auf Azure App Service"
    description="Erfahren Sie, wie Ihre Anwendung auf Azure App Service bereitstellen."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cephalin;dariac"/>
    
# <a name="deploy-your-app-to-azure-app-service"></a>Bereitstellen Sie Ihrer Anwendung auf Azure App Service

In diesem Artikel können Sie bestimmen, die beste Möglichkeit, die Dateien für WebApp, mobile-app Back-End- oder API-app [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)bereitzustellen und führt Sie zu Ressourcen mit Informationen für Ihre bevorzugte Option.

## <a name="overview"></a>Übersicht über Azure App Service-Bereitstellung

Azure App Service verwaltet das Anwendungsframework für Sie (ASP.NET, PHP, Node.js usw.). Einige Frameworks sind standardmäßig aktiviert, während andere, wie Java und Python, benötigen eine einfache Häkchen Konfiguration zu aktivieren. Darüber hinaus können Sie die PHP-Version oder der Bitanzahl der Laufzeit Ihres Anwendungsframeworks anpassen. Weitere Informationen finden Sie unter [Konfigurieren Ihrer Anwendung in Azure App Service](web-sites-configure.md).

Da Sie die Web-Server oder die Anwendung Framework sorgen besitzen, Bereitstellen Ihrer app zu App Service ist Bereitstellen von Code, Binärdateien, Dateien und ihre entsprechenden Verzeichnisstruktur im [ **/site/wwwroot** Directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) Azure (oder **/Site/Wwwroot/App_Data/Aufträge/** für WebJobs). App-Dienst unterstützt die folgenden Bereitstellungsoptionen: 

- [FTP und FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): Ihre bevorzugte FTP oder FTPS aktiviert Tool Dateien in Azure, [FileZilla](https://filezilla-project.org) , umfassende IDEs wie [NetBeans](https://netbeans.org)verschieben. Dies ist ausschließlich eine Upload-Vorgang. Zusätzliche Dienste werden vom App-Dienst wie Versionskontrolle, Datei Struktur usw. bereitgestellt. 

- [Kudu (Git-Mercurial oder OneDrive-Ablage)](https://github.com/projectkudu/kudu/wiki/Deployment): das [Bereitstellungsmodul](https://github.com/projectkudu/kudu/wiki) in App Service verwenden. Drücken Sie Code zu Kudu direkt aus jedem Repository. Kudu bietet auch Dienstleistungen, wenn Code, einschließlich Versionskontrolle, paketwiederherstellung MSBuild und [Web-Hooks](https://github.com/projectkudu/kudu/wiki/Web-hooks) für kontinuierliche Bereitstellung und andere Automatisierungsaufgaben abgelegt ist. Das Bereitstellungsmodul Kudu unterstützt 3 verschiedene Bereitstellung Quellen:   
    * Inhalt Synchronisieren von OneDrive und Ablage   
    * Repository-basierte kontinuierliche Bereitstellung mit automatische Synchronisierung von GitHub Bitbucket und Visual Studio Team Services  
    * Repository-basierte Bereitstellung mit manuellen Synchronisierung von lokalen Git  

- [Web Deploy](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): Code direkt von Ihrem bevorzugten Microsoft App Service bereitstellen tools wie Visual Studio mit dem gleichen Werkzeuge, die Weitergabe an IIS-Server automatisiert. Dieses Tool unterstützt nur Diff-Bereitstellung, Erstellung, Transformationen Verbindungszeichenfolgen usw.. Web Deploy unterscheidet sich vom Kudu Binärdateien der Anwendung erstellt werden, bevor sie in Azure bereitgestellt werden. Ähnlich wie FTP, werden zusätzliche Dienste von App Service bereitgestellt.

Gängige Webentwicklungstools unterstützen eine oder mehrere dieser Prozesse. Während gewähltem Werkzeug Bereitstellungsverfahren, die Sie nutzen können bestimmt, hängt aus den Bereitstellungsprozess und die spezifischen Tools wählen Sie die tatsächliche DevOps Funktionalität zur Verfügung. Beispielsweise wenn Web Deploy aus [Visual Studio SDK Azure](#vspros)ausführen, obwohl Sie Automatisierung von Kudu erhalten erhalten Sie Paket wiederherstellen und MSBuild Automatisierung in Visual Studio. 

>[AZURE.NOTE] Diese Prozesse nicht tatsächlich [Azure Ressourcen bereitstellen](../resource-group-template-deploy-portal.md) , die Ihre Anwendung möglicherweise. Jedoch die meisten verknüpften Anleitungsartikeln zeigen, wie Sie die Anwendung bereitstellen und Code, End-to-End. Finden Sie zusätzliche Optionen für die Bereitstellung von Azure Ressourcen im Abschnitt [Bereitstellung Befehlszeilenprogramme automatisieren](#automate) .
     
## <a name="ftp"></a>Manuelles Kopieren von Dateien in Azure bereitstellen Sie über FTP
Wenn Sie manuell kopieren den Inhalt der Webseite auf einem Webserver werden verwendet, können einem [FTP-](http://en.wikipedia.org/wiki/File_Transfer_Protocol) Dienstprogramm Sie Dateien [FileZilla](https://filezilla-project.org/)oder Windows Explorer.

Vorteile von Dateien manuell sind:

- Vertrautheit und minimaler Komplexität für FTP-Werkzeuge. 
- Wissen, wo genau Ihre Dateien gehen.
- Zusätzliche Sicherheit mit FTP.

Die Nachteile von Dateien manuell sind:

- Müssen wissen, wie Sie Dateien in den richtigen Verzeichnissen in App Service bereitstellen. 
- Keine Versionskontrolle für Rollback, wenn Fehler auftreten.
- Keine integrierte Bereitstellung Verlauf für die Behandlung von Problemen bei der Bereitstellung.
- Mögliche lange Bereitstellung jederzeit da viele FTP-Tools nicht nur Diff kopieren und einfach kopieren.  

### <a name="howtoftp"></a>Zum Bereitstellen von Azure Manuelles Kopieren
Kopieren von Dateien in Azure umfasst einige einfache Schritte:

1. Sofern bereits Bereitstellung Anmeldeinformationen Informationen der FTP-Verbindung unter **Einstellungen** > **Eigenschaften**und kopieren die Werte für **FTP-Deployment User** **FTP-Hostname**und **FTPS Hostnamen**. Kopieren Sie die **FTP-Bereitstellung** Benutzer Nutzwert wie Azure-Portal, einschließlich der Anwendungsname Kontext für den FTP-Server bereitzustellen.
2. Verwenden Sie FTP-Client die Verbindung mit Ihrer Anwendung gesammelten Informationen.
3. Kopieren Sie Ihre Dateien und ihre entsprechenden Verzeichnisstruktur in [ **/site/wwwroot** Directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (oder **/Site/Wwwroot/App_Data/Aufträge/** für WebJobs).
4. Wechseln Sie zu Ihrer app-URL zu überprüfen, ob die Anwendung ordnungsgemäß ausgeführt wird. 

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [PHP-MySQL Web app erstellen und Bereitstellen von mithilfe von FTP](web-sites-php-mysql-deploy-use-ftp.md).

## <a name="dropbox"></a>Synchronisierung mit einem Cloud-Ordner bereitstellen
Eine gute Alternative zum [Kopieren von Dateien manuell](#ftp) wird Dateien und Ordner App Service von Cloud-Speicherservice Dropbox wie OneDrive synchronisiert. Synchronisierung mit einem Cloud-Ordner Kudu Prozess für die Bereitstellung verwendet (siehe [Überblick über Prozesse](#overview)).

Vorteile von Cloud-Ordner synchronisiert werden:

- Einfache Bereitstellung. Dienste wie OneDrive und Ablage bieten desktop synchronisieren, so ist Ihr lokales Arbeitsverzeichnis auch Ihre Bereitstellungsverzeichnis.
- Ein-Klick-Bereitstellung.
- Alle in das Bereitstellungsmodul Kudu steht (Paket wiederherstellen, Automatisierung).

Die Nachteile der Synchronisierung mit einem Cloud-Ordner sind:

- Keine Versionskontrolle für Rollback, wenn Fehler auftreten.
- Keine automatische Bereitstellung ist die manuelle Synchronisierung erforderlich.

### <a name="howtodropbox"></a>Zum Bereitstellen von Cloud-Ordner synchronisieren
In [Azure-Portal](https://portal.azure.com)können Sie bestimmen Sie einen Ordner für die Synchronisierung von Inhalten in Ihrem OneDrive oder Dropbox Cloud-Speicher, Ihre app-Code und Inhalt des Ordners und App-Dienst durch Klicken auf eine Schaltfläche synchronisieren.

* [Sync Inhalte aus einem Ordner Cloud Azure App Service](app-service-deploy-content-sync.md). 

## <a name="continuousdeployment"></a>Ständig von einem cloudbasierten Source Control Dienst bereitstellen
Wenn Ihr Entwicklungsteam eine cloudbasierte Source Code Management (SCM) wie [Visual Studio Team Services](http://www.visualstudio.com/), [GitHub](https://www.github.com)oder [BitBucket](https://bitbucket.org/)wird, können Sie App Service zu Ihrem Repository integriert kontinuierlich bereitstellen. 

Vor der Bereitstellung von Cloud-basierte Quelle-Steuerungsdienst sind:

- Versionskontrolle Rollback aktivieren.
- Konfigurieren der kontinuierlichen Bereitstellung Git (und gegebenenfalls rast) Repositories. 
- Branchenspezifische Bereitstellung können Branchen in verschiedene [Steckplätze](web-sites-staged-publishing.md)bereitstellen.
- Alle in das Bereitstellungsmodul Kudu steht (z. B. Bereitstellung Versioning Rollback Paket wiederherstellen, Automatisierung).

Bereitstellung von Cloud-basierte Quelle-Steuerungsdienst ist:

- Kenntnisse der jeweiligen SCM Dienst erforderlich.

###<a name="vsts"></a>Ständig von einem cloudbasierten Source Control Dienst bereitstellen
In [Azure-Portal](https://portal.azure.com)können Sie kontinuierliche Bereitstellung von GitHub Bitbucket und Visual Studio Team Services konfigurieren.

* [Kontinuierliche Bereitstellung Azure App Service](app-service-continuous-deployment.md). 

## <a name="localgitdeployment"></a>Bereitstellen von lokalen Git
Wenn Ihr Entwicklungsteam einen lokale lokale Quelle Code Management (SCM) Dienst Git Grundlage verwendet, können Sie dies als Ausgangspunkt Bereitstellung App-Dienst konfigurieren. 

Vor der Bereitstellung von lokalen Git sind:

- Versionskontrolle Rollback aktivieren.
- Branchenspezifische Bereitstellung können Branchen in verschiedene [Steckplätze](web-sites-staged-publishing.md)bereitstellen.
- Alle in das Bereitstellungsmodul Kudu steht (z. B. Bereitstellung Versioning Rollback Paket wiederherstellen, Automatisierung).

Bereitstellen von lokalen Git ist:

- Kenntnisse der jeweiligen SCM-System erforderlich.
- Keine Lösungen für kontinuierliche Bereitstellung. 

###<a name="vsts"></a>Zum Bereitstellen von lokalen Git
In [Azure-Portal](https://portal.azure.com)können Sie lokale Git Bereitstellung konfigurieren.

* [Lokale Git Bereitstellung Azure App Service](app-service-deploy-local-git.md). 
* [Web Apps aus jeder Git-hg Repo veröffentlichen](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Mithilfe einer IDE
Wenn Sie [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) mit einer [Azure SDK](https://azure.microsoft.com/downloads/)oder andere IDE-Suites [Xcode](https://developer.apple.com/xcode/) [Eclipse](https://www.eclipse.org)und [IntelliJ sollten](https://www.jetbrains.com/idea/)bereits verwenden, können Sie in der IDE direkt von Azure bereitstellen. Diese Option ist ideal für einzelne Entwickler.

Visual Studio unterstützt alle drei Prozesse (FTP, Git und Web Deploy), je nach Wunsch, während andere IDEs App Service bereitstellen können, haben sie FTP oder Git Integration (siehe [Überblick über Prozesse](#overview)).

Vor der Bereitstellung mithilfe einer IDE sind:

- Minimieren Sie die Werkzeuge für die End-to-End-Lebenszyklus potenziell. Entwickeln Sie, Debuggen verfolgen Sie und Bereitstellen Sie Ihrer Anwendung zu Azure ohne außerhalb der IDE. 

Die Nachteile der Bereitstellung mithilfe einer IDE sind:

- Komplexität Werkzeuge.
- Weiterhin muss ein Quellcodeverwaltungssystem für ein Teamprojekt.

<a name="vspros"></a>Zusätzliche Vorteile der Bereitstellung von Azure SDK mit Visual Studio sind:

- Azure SDK stellt Azure Ressourcen vorrangigen in Visual Studio. Erstellen, löschen, bearbeiten, starten und Beenden von apps, Back-End-SQL-Datenbank Abfragen, live-Debug Azure-Anwendung und vieles mehr. 
- Live von Azure-Dateien bearbeiten.
- Live Debuggen Apps in Azure.
- Integrierte Azure Explorer.
- Nur Diff-Bereitstellung. 

###<a name="vs"></a>Direkt aus Visual Studio bereitstellen

* [Erste Schritte mit Azure und ASP.NET](web-sites-dotnet-get-started.md). Zum Erstellen und Bereitstellen einer einfachen ASP.NET MVC-Webprojekt mit Visual Studio und Web Deploy.
* [Wie bereitstellen Azure Webaufträge mit Visual Studio](websites-dotnet-deploy-webjobs.md). Das Konsolenanwendungsprojekt Projekte so konfigurieren, dass sie als Webaufträge bereitstellen.  
* [Bereitstellen eine sicheren ASP.NET MVC 5 Anwendung mit Mitgliedschaft, OAuth, SQL Datenbank webapps](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Das Erstellen und Bereitstellen einer ASP.NET MVC-Webprojekt mit einer SQL-Datenbank mithilfe von Visual Studio Web Deploy und Entity Framework Code First-Migrationen.
* [Bereitstellung von ASP.NET mithilfe von Visual Studio](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). Eine 12-Tutorial Serie, die eine vollständige Bereitstellungsaufgaben als andere in dieser Liste umfasst. Einige Features Azure-Bereitstellung wurden hinzugefügt, da das Lernprogramm wurde geschrieben, aber Notizen später erläutern, was fehlt.
* [Eine ASP.NET Website in Azure in Visual Studio 2012 ein Git Repository direkt bereitstellen](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Erläutert, wie ein ASP.NET Webprojekt in Visual Studio mit plug-in Git dazu Code Git zu verbindenden Azure Git Repository bereitstellen. Ab Visual Studio 2013 Git Unterstützung ist integriert und erfordert keine Installation eines Plug-Ins.

###<a name="aztk"></a>Bereitstellen mithilfe der Azure-Toolkits für Eclipse und IntelliJ IDEA

Microsoft ermöglicht Web Apps in Azure Eclipse und IntelliJ über [Azure Toolkit für Eclipse](../azure-toolkit-for-eclipse.md) und [Azure-Toolkit für IntelliJ](../azure-toolkit-for-intellij.md)bereitstellen. Die folgenden Lernprogramme veranschaulichen die Schritte, einfach ein "" Hallo Web App in Azure mit einer IDE bereitstellen:

*  [Hello World Web App für Azure in Eclipse erstellen](./app-service-web-eclipse-create-hello-world-web-app.md). Dieses Lernprogramm veranschaulicht die Azure-Toolkit für Eclipse erstellen und Bereitstellen einer Hello World Web App für Azure verwenden.
*  [Hello World Web App für Azure in IntelliJ erstellen](./app-service-web-intellij-create-hello-world-web-app.md). Dieses Lernprogramm veranschaulicht die Azure-Toolkit für IntelliJ erstellen und Bereitstellen einer Hello World Web App für Azure verwenden.


## <a name="automate"></a>Automatisieren der Bereitstellung mithilfe von Befehlszeilenprogrammen

* [Automatisieren der Bereitstellung mit MSBuild](#msbuild)
* [Kopieren von Dateien mit FTP-Tools und Skripts](#ftp)
* [Automatisieren der Bereitstellung mit Windows PowerShell](#powershell)
* [Automatisieren der Bereitstellung mit .NET API](#api)
* [Bereitstellen von Azure Befehlszeilenschnittstelle (CLI Azure)](#cli)
* [Bereitstellen von Web Deploy-Befehlszeile](#webdeploy)
* [Verwenden von Batchskripts FTP](http://support.microsoft.com/kb/96269).
 
Optional Bereitstellung ist mit einem cloudbasierten Diensts wie [Krake bereitstellen](http://en.wikipedia.org/wiki/Octopus_Deploy). Weitere Informationen finden Sie unter [Bereitstellen von ASP.NET Applications auf Azure-Websites](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

###<a name="msbuild"></a>Automatisieren der Bereitstellung mit MSBuild

Verwenden Sie die [Visual Studio-IDE](#vs) für die Entwicklung können [MSBuild](http://msbuildbook.com/) Sie um alles haben Sie in der IDE zu automatisieren. Sie können MSBuild zum [Web Deploy](#webdeploy) oder [FTP-FTP](#ftp) verwenden, um Dateien zu kopieren. Web Deploy kann auch viele andere Deployment-Aufgaben, z. B. Datenbanken automatisieren.

Weitere Informationen über die Befehlszeile mit MSBuild Bereitstellung finden Sie in folgenden Ressourcen:

* [ASP.NET Web Deployment mit Visual Studio: Befehlszeile Bereitstellung](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Zehnte in eine Reihe von Lernprogrammen zur Bereitstellung in Azure mithilfe von Visual Studio. Veranschaulicht, wie mithilfe der Befehlszeile einrichten Veröffentlichungsprofile in Visual Studio bereitstellen.
* [In die Microsoft Build Engine: MSBuild mit Team Foundation Build](http://msbuildbook.com/). Gedruckte Buch, das Kapitel zur Verwendung von MSBuild für die Bereitstellung enthält.

###<a name="powershell"></a>Automatisieren der Bereitstellung mit Windows PowerShell

Sie können MSBuild oder FTP-Bereitstellung von [Windows PowerShell-](http://msdn.microsoft.com/library/dd835506.aspx)Funktionen. Wenn Sie dies tun, können Sie auch eine Auflistung von Windows PowerShell-Cmdlets, die übrigen Azure Management-API aufrufen, erleichtern.

Weitere Informationen finden Sie in folgenden Ressourcen:

* [Bereitstellen einer Web app GitHub Repository verknüpft](app-service-web-arm-from-github-provision.md)
* [Bereitstellen einer Webanwendung mit einer SQL-Datenbank](app-service-web-arm-with-sql-database-provision.md)
* [Bereitstellung und Microservices vorhersehbar in Azure bereitstellen](app-service-deploy-complex-application-predictably.md)
* [Building reale Cloud Apps mit Azure - alles automatisieren](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). E-Book-Kapitel, die wird, wie die e-Book-beispielanwendung Windows PowerShell-Skripts erläutert zu schaffen Azure testen, bereitstellen. Siehe Abschnitt [Ressourcen](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) Links zu zusätzliche Azure PowerShell-Dokumentation.
* [Mithilfe von Windows PowerShell Skripts Entwicklung und Testumgebung veröffentlichen](../vs-azure-tools-publishing-using-powershell-scripts.md). Wie mit Windows PowerShell-Bereitstellung, die Skripts generiert Visual Studio.

###<a name="api"></a>Automatisieren der Bereitstellung mit .NET API

Sie können C# Code MSBuild oder FTP-Bereitstellung Funktionen schreiben. Wenn Sie dies tun, können Sie die Azure Management REST API Site Management-Funktionen zugreifen.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Automatisieren mit Azure Management Bibliotheken und .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Einführung in die .NET Management-API und Links zu mehr.

###<a name="cli"></a>Bereitstellen von Azure Befehlszeilenschnittstelle (CLI Azure)

Die Befehlszeile können in Windows, Mac und Linux-Maschinen mithilfe von FTP bereitstellen. Wenn Sie dies tun, können Sie auch Azure REST Management-API mit der Azure-Befehlszeilenschnittstelle zugreifen.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Azure Command Line Tools](/downloads/#cmd-line-tools). Portalseite in Azure.com für Befehlszeileninformationen Tool.

###<a name="webdeploy"></a>Bereitstellen von Web Deploy-Befehlszeile

[Web Deploy](http://www.iis.net/downloads/microsoft/web-deploy) ist Microsoft-Software für die Weitergabe an IIS, die nicht nur intelligenter Synchronisierungsfunktionen enthält jedoch auch ausführen oder viele andere Deployment-Aufgaben, die automatisiert werden können koordinieren, wenn Sie FTP verwenden. Z. B. können Web Bereitstellen einer neuen Datenbank oder Datenbankupdates zusammen mit Ihrer Anwendung bereitgestellt werden. Web Deploy minimiert auch die Zeit für eine vorhandene Website aktualisieren, da Intelligent nur geänderte Dateien kopieren können. Microsoft Visual Studio und Team Foundation Server bieten Unterstützung für Web Deploy integrierte, aber Sie können auch Web Deploy direkt über die Befehlszeile automatisierte Bereitstellung. Web Deploy-Befehle sind sehr leistungsfähig, aber die Lernkurve kann steilen.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Einfache webapps: Bereitstellung](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blog von David Ebbo zu einem Tool schrieb er verwenden Web Deploy erleichtern.
* [Webbereitstellungstool](http://technet.microsoft.com/library/dd568996). Offizielle Dokumentation auf der Microsoft TechNet-Website. Vom jedoch weiterhin ein guter Anfang.
* [Mit Web bereitstellen](http://www.iis.net/learn/publish/using-web-deploy). Offizielle Dokumentation auf der Website Microsoft IIS.NET. Darüber hinaus datiert, aber ein guter Anfang.
* [ASP.NET Web Deployment mit Visual Studio: Befehlszeile Bereitstellung](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild ist das Buildmodul von Visual Studio verwendet und es kann auch von der Befehlszeile aus verwendet werden, ASP.NET-Webanwendungen Web Apps bereitstellen. Dieses Lernprogramm ist Teil einer Serie, die hauptsächlich zur Bereitstellung von Visual Studio.

##<a name="nextsteps"></a>Nächste Schritte

In einigen Szenarios möchten Sie problemlos zwischen einem Staging- und eine Produktionsversion der Anwendung wechseln können. Weitere Informationen finden Sie unter [Gestaffelte Bereitstellung auf Web Apps](web-sites-staged-publishing.md).

Einen Plan für Sicherung und Wiederherstellung ist vorhanden ein wichtiger Bestandteil jeder Bereitstellungsworkflow. Informationen zu App-Service backup und Wiederherstellung von Feature, [Web Apps Backups](web-sites-backup.md).  

Finden Sie Informationen zur Verwendung von Azure rollenbasierte Zugriffskontrolle auf App Bereitstellung verwalten [RBAC und Web App veröffentlichen](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).


 
