<properties
    pageTitle="PHP Web- und Workerrollen Rollen | Microsoft Azure"
    description="Ein Handbuch zu PHP Web- und Workerrollen Rollen in einem Azure-Cloud-Dienst erstellen und Konfigurieren von PHP-Laufzeit."
    services=""
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

#<a name="how-to-create-php-web-and-worker-roles"></a>PHP Web- und Workerrollen Rollen erstellen

## <a name="overview"></a>Übersicht

Dieses Handbuch zeigt wie PHP Web oder Worker-Rollen in einer windowsumgebung erstellen, wählen die "integrierten" Versionen einer bestimmten Version von PHP die PHP-Konfiguration ändern, Extensions aktivieren und schließlich in Azure bereitstellen. Es beschreibt auch zu einem Web oder Arbeitskraft Rolle PHP-Laufzeit (mit Konfiguration und Erweiterung) verwenden, die Sie bereitstellen.

## <a name="what-are-php-web-and-worker-roles"></a>Was sind PHP Web- und Workerrollen Rollen?

Azure bietet drei Modelle für die Ausführung der Anwendung berechnen: Azure App Service Azure Virtual Machines und Azure Cloud Services. Alle drei Modelle unterstützen PHP. Cloud-Dienste, einschließlich Web-und Workerrollen Plattform *als Service (PaaS)*. Im Cloud-Dienst bietet eine Webrolle einen dedizierter Webserver (Internetinformationsdienste) Host Front-End-Anwendung. Eine Worker-Rolle kann asynchrone langer oder ständige Aufgaben unabhängig von Benutzer oder Eingabe ausgeführt.

Weitere Informationen zu diesen Optionen finden Sie unter [Hosting-Optionen von Azure Compute](./cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>Für PHP Azure SDK herunterladen

[Azure SDK für PHP] besteht aus mehreren Komponenten. In diesem Artikel verwenden zwei: Azure PowerShell und Azure Emulatoren. Diese beiden Komponenten können über die Microsoft-Webplattform-Installer installiert. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](powershell-install-configure.md).

## <a name="create-a-cloud-services-project"></a>Cloud-Services-Projekt erstellen

Der erste Schritt beim Erstellen einer PHP Web oder Worker-Rolle ist ein Azure Service-Projekt erstellen. ein Azure Service-Projekt fungiert als logischer Container für Web-und Workerrollen und enthält die Dateien des Projekts [Dienstdefinition (.csdef)] und [Konfiguration (.cscfg)] .

Erstellen Sie ein neues Webdienstprojekt Azure Azure PowerShell als Administrator ausführen und den folgenden Befehl ausführen:

    PS C:\>New-AzureServiceProject myProject

Dieser Befehl erstellt ein neues Verzeichnis (`myProject`), können Sie Web-und Workerrollen hinzufügen.

## <a name="add-php-web-or-worker-roles"></a>PHP Web oder Worker-Rollen hinzufügen

Um eine Webrolle PHP zu einem Projekt hinzuzufügen, führen Sie den folgenden Befehl im Stammverzeichnis des Projekts:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Verwenden Sie diesen Befehl für eine Worker-Rolle:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [AZURE.NOTE] Die `roleName` Parameter ist optional. Wenn es weggelassen wird, wird der Rollenname automatisch generiert. Die erste Webrolle erstellt werden `WebRole1`, der zweite `WebRole2`und so weiter. Die erste Arbeitskraft Rolle erstellt werden `WorkerRole1`, der zweite `WorkerRole2`und so weiter.

## <a name="specify-the-built-in-php-version"></a>Geben Sie die integrierte PHP version

Wenn ein Projekt eine PHP Web oder Worker-Rolle hinzufügen, werden Projektdateien Konfiguration geändert, sodass PHP installiert wird für jede Instanz Web oder Arbeitskraft der Anwendung bereitgestellt werden. Version von PHP finden, die standardmäßig installiert werden, führen Sie den folgenden Befehl ein:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

Die Ausgabe des obigen Befehls sieht etwa wie nachfolgend gezeigt. In diesem Beispiel die `IsDefault` gesetzt ist `true` für PHP 5.3.17, dass die standardmäßige PHP Version installiert werden.

    Runtime Version     PackageUri                      IsDefault
    ------- -------     ----------                      ---------
    Node 0.6.17         http://nodertncu.blob.core...   False
    Node 0.6.20         http://nodertncu.blob.core...   True
    Node 0.8.4          http://nodertncu.blob.core...   False
    IISNode 0.1.21      http://nodertncu.blob.core...   True
    Cache 1.8.0         http://nodertncu.blob.core...   True
    PHP 5.3.17          http://nodertncu.blob.core...   True
    PHP 5.4.0           http://nodertncu.blob.core...   False

Sie können die Laufzeitversion PHP, PHP-Versionen festzulegen. Beispielsweise legen Sie die PHP-Version (für eine Rolle mit dem Namen `roleName`), 5.4.0, den folgenden Befehl verwenden:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [AZURE.NOTE] PHP Versionen möglicherweise in Zukunft ändern.

## <a name="customize-the-built-in-php-runtime"></a>Anpassen der integrierten PHP-Laufzeit

Sie haben die vollständige Kontrolle über die Konfiguration der PHP-Laufzeit, die installiert wird, wenn Sie die Schritte oben, einschließlich der Änderung der `php.ini` Settings und Extensions aktivieren.

Um integrierte PHP-Laufzeit anzupassen, gehen Sie folgendermaßen vor:

1. Fügen Sie einen neuen Ordner mit dem Namen `php`, die `bin` Verzeichnis Ihrer Web-Rolle. Für eine Worker-Rolle in der Rolle Stammverzeichnis hinzufügen.
2. In der `php` Ordner erstellt einen Ordner namens `ext`. Setzen `.dll` Dateien (z.B. `php_mongo.dll`), die diesem Ordner hinzugefügt werden soll.
3. Hinzufügen einer `php.ini` -Datei in die `php` Ordner. Aktivieren Sie benutzerdefinierten Erweiterungen und PHP Richtlinien in dieser Datei. Wenn Sie wollten beispielsweise `display_errors` auf und die `php_mongo.dll` Erweiterung, den Inhalt der `php.ini` lauten wie folgt:

        display_errors=On
        extension=php_mongo.dll

> [AZURE.NOTE] Eine Einstellung, die Sie nicht in explizit die `php.ini` Datei wird automatisch bereitgestellt auf ihre Standardwerte festgelegt werden. Aber dabei eine vollständige hinzuzufügen `php.ini` Datei.

## <a name="use-your-own-php-runtime"></a>Verwenden Sie Ihre eigenen PHP-Laufzeit
In einigen Fällen statt eine integrierte PHP-Laufzeit auswählen und konfigurieren, wie oben beschrieben, möchten Sie Ihre eigenen PHP-Laufzeit bereitstellen. Beispielsweise können dieselbe PHP-Laufzeit Web oder Worker-Rolle Sie die Verwendung in der Umgebung. Dies erleichtert das sicherstellen, dass die Anwendung nicht Verhalten in Ihrem Unternehmen ändern.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Konfigurieren einer Webrolle Verwendung eigener PHP-Laufzeit

Konfigurieren eine Webrolle Verwendung von PHP-Laufzeit, die Sie bereitstellen, gehen Sie folgendermaßen vor:

1. Ein Azure Service-Projekt erstellen und Hinzufügen einer Webrolle PHP, wie zuvor in diesem Thema beschrieben.
2. Erstellen einer `php` Ordner in die `bin` Ordner, die im Stammverzeichnis der Webrolle und fügen die PHP-Laufzeit (alle Binärdateien, Konfigurationsdateien, Unterordner usw.) die `php` Ordner.
3. (OPTIONAL) Wenn der PHP-Laufzeit die [Microsoft-Treiber für PHP für SQL Server]verwendet[sqlsrv drivers], müssen die Web-Rolle installieren Sie [SQL Server Native Client 2012] konfigurieren[ sql native client] Wenn bereitgestellt. Dazu fügen den [sqlncli.msi X64 Installer] die `bin` Ordner im Stammverzeichnis Ihrer Webserverrolle. Das Startskript im nächsten Schritt beschrieben werden das Installationsprogramm unbeaufsichtigt ausgeführt, wenn die Funktion bereitgestellt wird. Wenn der PHP-Laufzeit nicht Microsoft Drivers für PHP für SQL Server verwendet, können Sie die folgende Zeile in das Skript im nächsten Schritt entfernen:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Definieren Sie eine Startaufgabe, die [Internet Information Services (IIS)] konfiguriert[ iis.net] mit der PHP-Laufzeit Anfragen für `.php` Seiten. Öffnen Sie hierzu die `setup_web.cmd` Datei (in der `bin` Datei des Stammverzeichnisses der Webrolle) in einem Text-Editor und Ersetzen Sie den Inhalt mit dem folgenden Skript:

        @ECHO ON
        cd "%~dp0"

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
        SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"

5. Stammverzeichnis der Webrolle Anwendungsdateien hinzugefügt. Stammverzeichnis des Webservers werden.

6. Veröffentlichen der Anwendung im Abschnitt [Veröffentlichen der Anwendung](#how-to-publish-your-application) beschrieben.

> [AZURE.NOTE] Die `download.ps1` Skript (in der `bin` Ordner des Stammverzeichnisses der Webrolle) können gelöscht werden, nachdem Sie die eigene PHP-Laufzeit mit dem oben beschriebenen Schritte.

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Konfigurieren Sie eine Worker-Rolle Verwendung eigener PHP-Laufzeit

Konfigurieren Sie eine Worker-Rolle um eine PHP-Laufzeit verwenden, Sie gehen folgendermaßen vor:

1. Erstellen Sie ein Azure Service-Projekt und fügen Sie eine PHP-Worker-Rolle hinzu, wie zuvor in diesem Thema beschrieben.
2. Erstellen einer `php` Ordner im Stammverzeichnis der Worker-Rolle und fügen die PHP-Laufzeit (alle Binärdateien, Konfigurationsdateien, Unterordner usw.) die `php` Ordner.
3. (OPTIONAL) PHP-Laufzeit nutzt [Microsoft-Treiber für PHP für SQL Server][sqlsrv drivers], müssen die Worker-Rolle installieren Sie [SQL Server Native Client 2012] konfigurieren[ sql native client] Wenn bereitgestellt. Stammverzeichnis der Worker-Rolle dazu [sqlncli.msi X64 Installer] hinzufügen. Das Startskript im nächsten Schritt beschrieben werden das Installationsprogramm unbeaufsichtigt ausgeführt, wenn die Funktion bereitgestellt wird. Wenn der PHP-Laufzeit nicht Microsoft Drivers für PHP für SQL Server verwendet, können Sie die folgende Zeile in das Skript im nächsten Schritt entfernen:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Definieren eine Startaufgabe fügt die `php.exe` ausführbare Worker-Rolle PATH-Umgebungsvariable, wenn die Funktion bereitgestellt wird. Öffnen Sie dazu die `setup_worker.cmd` Datei (im Stammverzeichnis der Worker-Rolle) in einem Text-Editor und Ersetzen Sie den Inhalt mithilfe des folgenden Skripts:

        @echo on

        cd "%~dp0"

        echo Granting permissions for Network Service to the web root directory...
        icacls ..\ /grant "Network Service":(OI)(CI)W
        if %ERRORLEVEL% neq 0 goto error
        echo OK

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        setx Path "%PATH%;%~dp0php" /M

        if %ERRORLEVEL% neq 0 goto error

        echo SUCCESS
        exit /b 0

        :error

        echo FAILED
        exit /b -1

5. Stammverzeichnis der Worker-Rolle die Anwendungsdateien hinzufügen.

6. Veröffentlichen der Anwendung im Abschnitt [Veröffentlichen der Anwendung](#how-to-publish-your-application) beschrieben.

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Führen Sie die Anwendung in den Computing- und Emulatoren

Azure Emulatoren bieten eine Umgebung, in der Sie Ihre Azure-Anwendung testen können vor der Bereitstellung in der Cloud. Es gibt einige Unterschiede zwischen der Emulatoren und der Azure-Umgebung. Zum besseren Verständnis finden Sie unter [Verwenden der Azure-Speicheremulator für Entwicklung und testing](./storage/storage-use-emulator.md).

Beachten Sie, dass PHP installiert lokal Serveremulator verwenden müssen. Serveremulator verwendet lokale PHP-Installation, um die Anwendung auszuführen.

Führen Sie das Projekt in den Emulatoren Stammverzeichnis des Projekts führen Sie den folgenden Befehl aus:

    PS C:\MyProject> Start-AzureEmulator

Sie sehen die Ausgabe ähnelt:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Sie sehen die Anwendung im Emulator ausgeführt wird, durch einen Webbrowser öffnen und zu der lokalen Adresse in die Ausgabe (`http://127.0.0.1:81` im obigen Beispiel).

Führen Sie diesen Befehl, um die Emulatoren zu beenden:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Veröffentlichen Sie die Anwendung

Um die Anwendung zu veröffentlichen, müssen Sie zuerst importieren die Standardeinstellungen mithilfe des [Import-AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) -Cmdlets veröffentlichen. Anschließend können Sie die Anwendung mithilfe des Cmdlets [Veröffentlichen AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) veröffentlichen. Informationen zum Anmelden finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](powershell-install-configure.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [PHP-Entwicklercenter](/develop/php/).

[Azure SDK für PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[Dienstdefinition (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[Konfiguration (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[SQLNCLI.msi X64 installer]: http://go.microsoft.com/fwlink/?LinkID=239648
