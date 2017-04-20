<properties
    pageTitle="Arbeiten mit Node.js-Modulen"
    description="Informationen Sie zum Node.js Module arbeiten bei Azure App Service oder Cloud-Dienste verwenden."
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>


# <a name="using-nodejs-modules-with-azure-applications"></a>Node.js-Module verwenden mit Azure

Dieses Dokument bietet einen Leitfaden für Applikationen auf Azure Node.js Module mit. Es enthält Richtlinien zum sicherstellen, dass die Anwendung einer bestimmten Version des Moduls sowie systemeigene Module mit Azure.

Wenn Sie bereits mit vertraut sind wird mit Node.js Module Dateien **package.json** und **Npm-shrinkwrap.json** , die folgenden eine kurze Zusammenfassung der in diesem Artikel beschriebene:

* Azure App Service versteht **package.json** und **Npm-shrinkwrap.json** -Dateien und Module anhand der Einträge in diesen Dateien können.
* Azure Cloud Services erwarten alle Module auf der Development Environment und **Knoten\_Module** Verzeichnis das Bereitstellungspaket enthalten sein. Es ist möglich, Unterstützung für Module **package.json** oder **Npm shrinkwrap.json** Dateien auf Cloud-Dienste jedoch diese Anpassung Standardskripten Clouddienst Projekte erfordert. Ein Beispiel hierzu finden Sie unter [Azure Startaufgabe Npm Installation zum Vermeiden Sie Knoten Module ausführen](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [AZURE.NOTE] Azure Virtual Machines werden nicht in diesem Artikel behandelt die bereitstellungserfahrung in einer VM hängt von dem Betriebssystem vom virtuellen Computer gehostet werden.

##<a name="nodejs-modules"></a>Node.js-Module

Module sind loadable JavaScript-Pakete, die spezifische für Ihre Anwendung Funktionalität. Ein Modul ist normalerweise installiert mit dem Befehlszeilentool **Npm** (wie das HTTP-Modul) als Teil der Core Node.js bereitgestellt werden.

Module installiert sind, sie befinden sich der **Knoten\_Module** Verzeichnis am Stamm der Verzeichnisstruktur der Anwendung. Jedes Modul in der **Knoten\_Module** Directory eigenes **Knoten\_Module** Verzeichnis, das alle Module enthält, hängt dieser Vorgang wiederholt sich erneut für jedes Modul unten der Abhängigkeitskette. Dadurch wird jedes Modul installiert eigene Version an die Module, dies hängt jedoch ganz große Verzeichnisstruktur Dadurch können.

Beim Bereitstellen der **Knoten\_Module** Verzeichnis als Teil Ihrer Anwendung erhöht die Größe der Bereitstellung im Vergleich zu einer **package.json** oder **Npm-shrinkwrap.json** -Datei; Es garantiert, dass die Version des in der Produktion verwendeten Module identisch wie Entwicklung.

###<a name="native-modules"></a>Systemeigene Module

Die meisten Module sind einfach Klartext JavaScript-Dateien einige Module plattformspezifische binäre Abbilder. Diese Module werden normalerweise bei der Installation kompiliert, mithilfe von Python und Gips Knoten. Da Azure Cloud Services auf der **Knoten\_Module** Ordner als Teil der Anwendung, alle systemeigenen Moduls Bestandteil von den installierten Modulen bereitgestellt sollte arbeiten in einem Clouddienst installiert und auf einem Windows-Entwicklungssystem kompiliert wurde.

Azure App Service unterstützt nicht alle systemeigenen Module und zur Kompilierung mit sehr spezifischen Komponenten möglicherweise fehl. Während einige gängige Module wie MongoDB optionale native Abhängigkeiten und verwenden Sie erfolgreich zwei Workarounds mit fast allen systemeigenen heute:

* Führen Sie **Npm installieren** auf einem Windows-Computer, der alle systemeigenen Moduls Software installiert ist. Dann Bereitstellen des erstellten **Knoten\_Module** Ordner als Teil der Anwendung Azure App Service.
* Azure App Service kann konfiguriert werden zum Ausführen von benutzerdefinierten Bash oder Shell-Skripts während der Bereitstellung geben Ihnen die Möglichkeit, benutzerdefinierte Befehle und genau konfigurieren die **Npm installieren** ausgeführt wird. Ein Video zum hierzu finden Sie unter [Benutzerdefinierte Website Bereitstellungsskripts mit Kudu].

###<a name="using-a-packagejson-file"></a>Mithilfe einer Datei package.json

Die Datei **package.json** ist die obersten Ebene Abhängigkeiten angeben muss Ihre Anwendung Hosting-Plattform Abhängigkeiten, anstatt Sie enthalten installieren kann die **Knoten\_Pakete** Ordner als Teil der Bereitstellung. Nach der Bereitstellung der Anwendungdes wird der Befehl **Npm installieren** zu analysieren der Datei **package.json** alle aufgeführten Abhängigkeiten verwendet.

Während der Entwicklung können Sie die **-Speichern**, **-Dev speichern**, oder **-Speichern optionale** Parameter beim Installieren Module, um die Datei **package.json** einen Eintrag für das Modul automatisch hinzufügen. Weitere Informationen finden Sie unter [Npm installieren](https://docs.npmjs.com/cli/install).

Ein mögliches Problem mit der Datei **package.json** werden nur oberste Abhängigkeitsscan Version gibt. Jedes Modul installiert möglicherweise oder möglicherweise kein Version hängt von Modulen und so kann als die Entwicklung mit anderen Abhängigkeit überwindet.

> [AZURE.NOTE]
> Wenn Azure App Service bereitstellen, wenn die Datei <b>package.json</b> ein systemeigenen Modul verweist sehen Sie beim Veröffentlichen der Anwendung Git Fehler, die der folgenden ähnelt:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


###<a name="using-a-npm-shrinkwrapjson-file"></a>Mithilfe einer Npm-shrinkwrap.json-Datei

**Npm-shrinkwrap.json** -Datei ist ein Versuch, das Modul Versioning Beschränkungen der Datei **package.json** . Während die Datei **package.json** nur Versionen der obersten Ebene Module enthält, enthält die Datei **Npm-shrinkwrap.json** Version an der Abhängigkeitskette ganzes Modul.

Wenn die Anwendung für die Produktion bereit ist, Sie können Sperren-Version an und erstellen Sie eine **Npm-shrinkwrap.json** -Datei mithilfe des Befehls **Npm Verpackung** . Dies wird derzeit Version verwenden die **Knoten\_Module** Ordner und diese Datei **Npm-shrinkwrap.json** . Nach der Bereitstellung der Anwendungdes an die Hostingumgebung wird der Befehl **Npm installieren** **Npm shrinkwrap.json** Datei analysieren und installieren Sie alle aufgeführten Abhängigkeiten verwendet. Weitere Informationen finden Sie unter [Npm-Verpackung](https://docs.npmjs.com/cli/shrinkwrap).

> [AZURE.NOTE]
>Wenn Azure App Service bereitstellen, wenn die Datei <b>Npm-shrinkwrap.json</b> ein systemeigenen Modul verweist sehen Sie beim Veröffentlichen der Anwendung Git Fehler, die der folgenden ähnelt:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


##<a name="next-steps"></a>Nächste Schritte

Da Azure Node.js Module mit verstehen lernen wie [Node.js-Version angeben], [Erstellen und Bereitstellen eine Node.js Web app]und [Verwendung der Azure-Befehlszeilenschnittstelle für Mac und Linux].

Weitere Informationen finden Sie unter [Node.js Developer Center](/develop/nodejs/).

[Node.js-Version angeben]: nodejs-specify-node-version-azure-apps.md
[Verwendung der Azure-Befehlszeilenschnittstelle für Mac und Linux]: xplat-cli-install.md
[Erstellen und Bereitstellen einer Node.js Web app]: web-sites-nodejs-develop-deploy-mac.md
[Node.js Web Application with Storage on MongoDB (MongoLab)]: store-mongolab-web-sites-nodejs-store-data-mongodb.md
[Build and deploy a Node.js application to an Azure Cloud Service]: cloud-services-nodejs-develop-deploy-app.md
[Benutzerdefinierte Website Bereitstellungsskripts mit Kudu]: /documentation/videos/custom-web-site-deployment-scripts-with-kudu/
