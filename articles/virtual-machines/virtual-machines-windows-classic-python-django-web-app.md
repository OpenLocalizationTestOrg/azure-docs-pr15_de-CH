<properties
    pageTitle="Python web app mit Django | Microsoft Azure"
    description="Dieses Lernprogramm zeigt Ihnen wie Django-basierte Website auf Azure mit einer klassischen Bereitstellungsmodell mit Windows Server 2012 R2 Datacenter virtuellen Computer."
    services="virtual-machines-windows"
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>


<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="huvalo"/>


# <a name="django-hello-world-web-application-on-a-windows-server-vm"></a>Django Hello World-Anwendung auf einem Windows Server-VM

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac-Linux](virtual-machines-linux-python-django-web-app.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

In diesem Lernprogramm beschreibt eine Django-basierten Website auf Microsoft Azure mit einem virtuellen Windows Server-Computer. Es wird vorausgesetzt, dass Sie keine Erfahrung mit Azure haben. Am Ende dieses Lernprogramms haben Sie eine Django-Anwendung einrichten und in der Cloud ausgeführt.

Erfahren Sie, wie Sie:

* Einrichten von Azure virtuelle Maschine Host Django. Während dieser praktischen Einführung dazu unter Windows Server erläutert, kann dasselbe auch mit Linux VM in Azure gehostet durchgeführt.
* Erstellen Sie eine neue Django-Anwendung von Windows.

Anhand dieses Lernprogramms erstellen Sie eine einfache Hello World Web-Anwendung. Die Anwendung wird in Azure virtuellen Computer gehostet werden.

Screenshot der abgeschlossenen Anwendung wird weiter angezeigt.

![Ein Browserfenster anzuzeigen Hello World in Azure][1]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Erstellen und Konfigurieren von Azure virtuelle Maschine Host Django

1. Folgen Sie der Anleitung angegebenen [hier](virtual-machines-windows-classic-tutorial.md) einen Azure virtuellen Computer der Windows Server 2012 R2 Datacenter Verteilung erstellt.

1. Weisen Sie Azure Port 80-Verkehr aus dem Internet an Port 80 auf dem virtuellen Computer direkt:
 - Navigieren Sie zu der neu erstellten virtuellen Computer im klassischen Azure-Portal und auf **aus** .
 - Klicken Sie am unteren Bildschirmrand auf die Schaltfläche **Hinzufügen** .
    ![Endpunkt hinzufügen](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-addendpoint.png)

 - Öffnen Sie das **TCP-** Protokoll ist **öffentlichen PORT 80** als **PRIVATE PORT 80**.
![][port80]
1. Klicken Sie auf der Registerkarte **DASHBOARD** **Verbinden** **Remotedesktopfeatures neu erstellten Azure Virtual Machine Remote anmelden** .  

**Hinweis:** Alle Anweisungen wird davon ausgegangen, dass Sie den virtuellen Computer ordnungsgemäß angemeldet und dort Befehle anstelle von Ihrem lokalen Computer ausgeben werden.

## <a id="setup"> </a>Installation von Python, Django, WFastCGI

**Hinweis:** Mithilfe von Internet Explorer möglicherweise für IE konfigurieren herunterladen (Start/Administrative Tools-Server Manager/Local Server **Verstärkte Sicherheitskonfiguration für IE**klicken, deaktiviert).

1. Installieren der neuesten Python 2.7 oder 3.4 von [python.org][].
1. Installieren Sie die Pakete Wfastcgi und Django mit Pip.

    Für Python 2.7 Befehl.

        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django

    Für Python 3.4 Befehl.

        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="installing-iis-with-fastcgi"></a>Installieren von IIS mit FastCGI

1. Installieren Sie IIS mit FastCGI-Unterstützung.  Dies dauert mehrere Minuten.

        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="creating-a-new-django-application"></a>Erstellen einer neuen Anwendung Django

1.  *C:\inetpub\wwwroot*Geben Sie den folgenden Befehl zum Erstellen eines neuen Projekts Django:

    Für Python 2.7 Befehl.

        C:\Python27\Scripts\django-admin.exe startproject helloworld

    Für Python 3.4 Befehl.

        C:\Python34\Scripts\django-admin.exe startproject helloworld

    ![Das Ergebnis des Befehls neu AzureService](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-cmd-new-azure-service.png)

1.  **Django-Admin** -Befehl generiert eine Grundstruktur für Django-basierte Websites:

  -   **helloworld\manage.py** können Sie hosten starten und Beenden Ihrer Django-basierte Website-hosting
  -   **helloworld\helloworld\settings.py** enthält Django für die Anwendung.
  -   **helloworld\helloworld\urls.py** enthält die Zuordnung zwischen einzelnen und seine.

1.  Erstellen Sie eine neue Datei namens **views.py** im Verzeichnis *C:\inetpub\wwwroot\helloworld\helloworld* . Diese enthält die Ansicht, die "Hello World" Seite rendert. Starten Sie Editor, und geben Sie Folgendes ein:

        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Ersetzen Sie den Inhalt der Datei urls.py folgende.

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )

## <a name="configuring-iis"></a>Konfigurieren von IIS

1. Die Sperrung der Abschnitte Handler in der globalen applicationhost.config.  Dies ermöglicht die Verwendung von Python-Handler in web.config.

        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers

1. Aktivieren Sie WFastCGI.  Dadurch wird eine Anwendung der globalen applicationhost.config hinzugefügt, die die ausführbare Python-Interpreter und das Skript wfastcgi.py auf.

    Python 2.7:

        c:\python27\scripts\wfastcgi-enable

    Python 3.4:

        c:\python34\scripts\wfastcgi-enable

1. Erstellen Sie eine web.config-Datei in *C:\inetpub\wwwroot\helloworld*.  Der Wert der `scriptProcessor` Attribut sollte die Ausgabe des vorherigen Schrittes übereinstimmen.  Pypi für weitere auf Wfastcgi finden Sie auf der Seite für [Wfastcgi][] .

    Python 2.7:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

    Python 3.4:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

1. Aktualisieren Sie den Speicherort der IIS Default Web Site Projektordner Django auf.

        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"

1. Schließlich Laden der Webseite in Ihrem Browser.

![Ein Browserfenster anzuzeigen Hello World in Azure][1]


## <a name="shutting-down-your-azure-virtual-machine"></a>Herunterfahren des virtuellen Computers Azure

Heruntergefahren Sie in diesem Lernprogramm danach bzw. entfernen Sie neu erstellten Azure virtuellen Computers um Ressourcen für andere Lernprogramme und Azure Benutzungsgebühren zu vermeiden.

[1]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
