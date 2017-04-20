<properties
    pageTitle="Azure App Service WebApp erweiterte Konfiguration und Erweiterung"
    description="Verwenden Sie XML-Dokument Transformation(XDT) Deklarationen zum Transformieren der Datei ApplicationHost.config in Ihrer Anwendung Azure App Service und private Extensions zum Aktivieren von benutzerdefinierten Aktionen."
    authors="cephalin"
    writer="cephalin"
    editor="mollybos"
    manager="wpickett"
    services="app-service"
    documentationCenter=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Azure App Service WebApp erweiterte Konfiguration und Erweiterung

Mithilfe von [Transformation von XML-Dokumenten](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) Deklarationen können Sie die Datei [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) in Ihrer Anwendung in Azure App Service transformieren. XDT-Deklarationen können Sie private Erweiterungen um benutzerdefinierte Web app Verwaltung ermöglichen. Dieser Artikel enthält eine beispielerweiterung PHP-Manager Web app, die Management von PHP-Einstellung über eine Webschnittstelle ermöglicht.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

##<a id="transform"></a>Erweiterte Konfiguration durch ApplicationHost.config
Die App Service-Plattform bietet Flexibilität und Kontrolle für die Webkonfiguration für die Anwendung. Obwohl die standardmäßige IIS ApplicationHost.config Konfigurationsdatei nicht für die direkte Bearbeitung in App Service verfügbar ist, unterstützt die Plattform ein deklaratives ApplicationHost.config Transformation Modell basiert auf XML-Dokument Transformation (XDT).

Um diese Transformation Funktionalität zu nutzen, erstellen Sie eine ApplicationHost.xdt-Datei mit XDT Inhalt und Stammverzeichnis der Website (d:\home\site) in der [Kudu-Konsole](https://github.com/projectkudu/kudu/wiki/Kudu-console). Sie müssen Web App Änderungen wirksam neu starten.

Im folgenden Beispiel applicationHost.xdt veranschaulicht, wie eine neue benutzerdefinierte Umgebungsvariable Web app hinzufügen, PHP 5.4 verwendet.

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
                <fastCgi>
                    <application>
                        <environmentVariables>
                                <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
                        </environmentVariables>
                    </application>
                </fastCgi>
        </system.webServer>
    </configuration>


Eine Protokolldatei mit Transformationsstatus und ist im FTP-Stammverzeichnis unter LogFiles\Transform verfügbar.

Weitere Beispiele finden Sie unter [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).

**Hinweis**<br />
Elemente aus der Liste der Module unter `system.webServer` kann nicht entfernt oder neu geordnet, aber die Liste sind möglich.


##<a id="extend"></a>Erweitern Sie Ihrer Anwendung

###<a id="overview"></a>Überblick über private Web app extensions

App Service unterstützt Web app Extensions als ein Erweiterungspunkt für Verwaltungsvorgänge. Tatsächlich werden einige Features der App Service-Plattform als vorinstallierte Erweiterungen implementiert. Während Extensions vorinstallierte Plattform geändert werden können, können Sie erstellen und Konfigurieren von privaten Extensions für Ihre Webanwendung. Dies hängt auch XDT-Deklarationen. Die wichtigsten Schritte zum Erstellen einer privaten Web app-Erweiterung sind die folgenden:

1. Web-Erweiterung **Inhalt**: Erstellen Sie eine App Service unterstützt
2. Web app Erweiterung **Erklärung**: Erstellen Sie eine ApplicationHost.xdt-Datei
3. Web-Erweiterung **Bereitstellung**: Platzieren von Inhalt im Ordner SiteExtensions unter`root`

Interne Hyperlinks für Web app sollte auf einen Pfad in der Datei ApplicationHost.xdt angegebenen Pfad der Anwendung verweisen. Jede Änderung an der Datei ApplicationHost.xdt erfordert eine Web app Wiederverwendung.

**Hinweis**: Weitere Informationen für diese Hauptelemente steht unter [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

Ein ausführliches Beispiel ist veranschaulichen die Schritte zum Erstellen und Aktivieren einer privaten Web app-Erweiterung enthalten. Der Quellcode für das folgende Beispiel PHP-Manager kann von [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager)heruntergeladen werden.

###<a id="SiteSample"></a>Web app Erweiterung Beispiel: PHP-Manager

PHP-Manager ist eine Web app Erweiterung, die Web app Administratoren einfach anzeigen und konfigurieren Sie ihre PHP über eine Weboberfläche anstatt PHP INI-Dateien direkt ändern kann. Allgemeine Konfigurationsdateien für PHP enthalten die php.ini-Datei finden Sie unter Programmdateien und. Notes.txt Datei im Stammverzeichnis Ihrer Anwendung. Da die php.ini-Datei nicht direkt bearbeitbare App Service-Plattform, die PHP-Manager-Erweiterung verwendet die. Notes.txt Datei Einstellung ändern.

####<a id="PHPwebapp"></a>PHP-Manager Web-Anwendung

Folgendes ist die Homepage der PHP-Manager-Bereitstellung:

![TransformSitePHPUI][TransformSitePHPUI]

Wie Sie sehen können, ist Web app-Erweiterung eine regelmäßige Anwendung, vergleichbar mit einer zusätzlichen ApplicationHost.xdt-Datei im Stammordner der Web app (Weitere Details zur Datei ApplicationHost.xdt stehen im nächsten Abschnitt dieses Artikels) platziert.

PHP-Manager-Erweiterung wurde mit Visual Studio ASP.NET MVC 4 Web Application-Vorlage erstellt. Die Ansicht im Projektmappen-Explorer zeigt die Struktur der PHP-Manager-Erweiterung.

![TransformSiteSolEx][TransformSiteSolEx]

Die einzige besondere Logik für die Dateieingabe ist anzugeben, in dem Verzeichnis "Wwwroot" Web App befindet. Wie das folgende Codebeispiel zeigt, die Umgebungsvariablen "HOME" gibt die Web app Root-Pfad und Wwwroot Pfad durch Anfügen von "Site\wwwroot" erstellt werden:

    /// <summary>
    /// Gives the location of the .user.ini file, even if one doesn't exist yet
    /// </summary>
    private static string GetUserSettingsFilePath()
    {
            var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
            if (rootPath == null)
            {
                rootPath = System.IO.Path.GetTempPath(); // For testing purposes
            };
            var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
            return userSettingsFile;
    }


Nachdem der Pfad haben, können Sie reguläre Datei e/a-Vorgänge zum Lesen und Schreiben von Dateien.

Ein Punkt Vorsicht mit Web app betrifft die Behandlung von internen Hyperlinks.  Haben Sie alle Links in HTML-Dateien, die absolute Pfade interne Hyperlinks in Ihrer Anwendung bereitstellen, müssen Sie sicherstellen, dass der Name der Erweiterung als Stammverzeichnis der Links vorangestellt werden. Dies ist erforderlich, da der Stamm für die Erweiterung nun ist "/`[your-extension-name]`/" anstatt nur "/", sodass interne Hyperlinks müssen entsprechend aktualisiert. Angenommen Sie, eine Verknüpfung zu folgenden haben:

`"<a href="/Home/Settings">PHP Settings</a>"`

Wenn die Verknüpfung Web app-Erweiterung gehört, muss die Verknüpfung in der folgenden Form:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

Diese Anforderung umgehen, indem entweder nur relative Pfade in der Webanwendung oder bei ASP.NET Applications mithilfe der `@Html.ActionLink` -Methode die entsprechenden Links für Sie erstellt.

####<a id="XDT"></a>Die applicationHost.xdt-Datei

Der Code für die Web-app-Erweiterung wird unter %HOME%\SiteExtensions\[der Erweiterungsname]. Wir nennen dies den Stamm Erweiterung.  

Die Web app Erweiterung der Datei applicationHost.config registrieren müssen Sie eine Datei namens ApplicationHost.xdt im Stammverzeichnis Erweiterung. Der Inhalt der Datei ApplicationHost.xdt sollte folgendermaßen aussehen:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.applicationHost>
                <sites>
                    <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
                        <!-- NOTE: Add your extension name in the application paths below -->
                        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
                        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
                            <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
                        </application>
                    </site>
                </sites>
        </system.applicationHost>
    </configuration>

Der Name der Erweiterung Namen wählen sollten als Stammordner Erweiterung gleichnamige.

Dies hat die Auswirkung, hinzufügen einen neuen Anwendung Weg der `system.applicationHost` Liste unterhalb der SCM-Website. Die SCM-Website ist eine Site Administration Endpunkt mit spezifischen Anmeldeinformationen. Hat die URL `https://[your-site-name].scm.azurewebsites.net`.  

    <system.applicationHost>
        ...
        <site name="~1[your-website]" id="1716402716">
                <bindings>
                    <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
                    <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
                </bindings>
                <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
                <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
                <logFile logSiteId="false" />
                <application path="/" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
                </application>
                <!-- Note the custom changes that go here -->
                <application path="/[your-extension-name]" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
                </application>
            </site>
    </sites>
      ...
    </system.applicationHost>

###<a id="deploy"></a>Web-Erweiterung Bereitstellung

Um Ihre app-Erweiterung installieren, können Sie FTP zum Kopieren aller Dateien der Webanwendung der `\SiteExtensions\[your-extension-name]` Ordner Web App auf dem Sie die Erweiterung installieren möchten.  Achten Sie darauf, dass die ApplicationHost.xdt-Datei an dieser Stelle. Starten Sie Ihrer Anwendung um die Erweiterung zu aktivieren.

Sie sollten Ihre Web app Durchwahl sehen können:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Beachten Sie, dass die URL wie die URL Ihrer Anwendung gesucht wird HTTPS und "haben.SCM" enthält.

Es ist möglich, alle private (nicht vorinstallierte) Erweiterungen für Ihrer Anwendung während der Entwicklung und Ermittlungen deaktivieren, durch Hinzufügen einer app-Einstellung mit dem Schlüssel `WEBSITE_PRIVATE_EXTENSIONS` und einem Wert von `0`.

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert
* Eine Anleitung zur Änderung von Websites zu App Service finden Sie unter: [Azure App Service und seine Auswirkung auf vorhandene Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png
 
