<properties 
    pageTitle="Python web app mit Django auf Linux | Microsoft Azure" 
    description="Erfahren Sie, wie ein Django-basierten Anwendung in Azure mit einer virtuellen Linux-Maschine." 
    services="virtual-machines-linux" 
    documentationCenter="python" 
    authors="huguesv" 
    manager="wpickett" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="huvalo"/>
    
# <a name="django-hello-world-web-application-on-a-linux-vm"></a>Django Hello World-Anwendung auf Linux VM

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac-Linux](virtual-machines-linux-python-django-web-app.md)

<br>

In diesem Lernprogramm beschreibt eine Django-basierten Website auf Microsoft Azure mit einer virtuellen Linux-Maschine. Es wird vorausgesetzt, dass Sie keine Erfahrung mit Azure haben. Nach Abschluss dieses Handbuchs, haben Sie eine Django-Anwendung einrichten und in der Cloud ausgeführt.

Erfahren Sie, wie Sie:

* Einrichten einer Azure VM Host Django. Während dieser praktischen Einführung dazu unter **Linux**erläutert, kann dasselbe auch mit einem Windows Server-VM in Azure gehostet durchgeführt. 
* Erstellen Sie eine neue Django Anwendung von Linux.

Anhand dieses Lernprogramms erstellen Sie eine einfache Hello World Web-Anwendung. Die Anwendung wird in Azure virtuellen Computer gehostet werden.

Screenshot der abgeschlossenen Anwendung lautet wie folgt:

![Ein Browserfenster anzuzeigen Hello World in Azure](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Erstellen und Konfigurieren von Azure virtuelle Maschine Host Django

1. Folgen Sie der Anleitung angegebenen [hier](virtual-machines-linux-quick-create-portal.md) ein Azure VM *Ubuntu Server 14.04 LTS* Verteilung erstellen.  Auf Wunsch können Sie Kennwortauthentifizierung statt öffentlichen SSH-Schlüssel.

1. Bearbeiten die Netzwerk-Sicherheitsgruppe zum Zulassen von http-Datenverkehr an Port 80 mit der [hier](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

1. Standardmäßig haben nicht den neuen virtuellen Computer einen vollqualifizierten Domänennamen (FQDN).  Erstellen Sie eine wie [hier](virtual-machines-linux-portal-create-fqdn.md).  Dieser Schritt ist optional, um das Lernprogramm abzuschließen.

## <a id="setup"> </a>Die Entwicklung fest

**Hinweis:** Wenn Sie Python installieren müssen oder die Clientbibliotheken verwenden möchten, finden Sie unter [Python-Installationshandbuch](../python-how-to-install.md).

Ubuntu Linux VM bereits im Lieferumfang von Python 2.7 vorinstalliert, aber keinen Apache oder Django installiert.  Gehen Sie mit der VM und Apache und Django installieren.

1.  Starten Sie ein neues **Terminal** -Fenster.
    
1.  Geben Sie den folgenden Befehl für die Verbindung der Azure-VM.  Wenn FQDN erstellt haben, können Sie die öffentliche IP-Adresse des virtuellen Computers im klassischen Azure-Portal angezeigt.

        $ ssh yourusername@yourVmUrl

1.  Geben Sie die folgenden Befehle zur Installation von Django:

        $ sudo apt-get install python-setuptools python-pip
        $ sudo pip install django

1.  Geben Sie den folgenden Befehl Apache mit mod Wsgi installieren:

        $ sudo apt-get install apache2 libapache2-mod-wsgi


## <a name="creating-a-new-django-application"></a>Erstellen einer neuen Anwendung Django

1.  Öffnen Sie das **Terminal** -Fenster, das Sie im vorherigen Abschnitt ssh in der VM verwendet.
    
1.  Geben Sie die folgenden Befehle zum Erstellen eines neuen Django-Projekts:

        $ cd /var/www
        $ sudo django-admin.py startproject helloworld

    **Django-admin.py** Skript generiert eine Grundstruktur für Django-Webseiten:
    -   **Manage.py** können Sie hosten starten und Beenden Ihrer Django-basierte Website-hosting
    -   **HelloWorld/HelloWorld/Settings.py** enthält Django für die Anwendung.
    -   **HelloWorld/HelloWorld/URLs.py** enthält die Zuordnung zwischen einzelnen und seine.

1.  Erstellen Sie eine neue Datei namens **views.py** im Verzeichnis **/var/www/helloworld/helloworld** . Diese enthält die Ansicht, die "Hello World" Seite rendert. Starten Sie Editor, und geben Sie Folgendes ein:
        
        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Ersetzen Sie den Inhalt der Datei **urls.py** jetzt mit folgenden:

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )


## <a name="setting-up-apache"></a>Einrichten von Apache

1.  Erstellen Sie einen Apache virtuellen Host-Konfiguration-Datei **/etc/apache2/sites-available/helloworld.conf**. Legen Sie den Inhalt der folgenden, und Ersetzen Sie *YourVmName* durch den tatsächlichen Namen des Computers (zum Beispiel *Pyubuntu verwenden*).

        <VirtualHost *:80>
        ServerName yourVmName
        </VirtualHost>
        WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
        WSGIPythonPath /var/www/helloworld

1.  Aktivieren Sie mit dem folgenden Befehl:

        $ sudo a2ensite helloworld

1.  Starten Sie Apache mit folgendem Befehl:

        $ sudo service apache2 reload

1.  Schließlich Laden der Webseite in Ihrem Browser:

    ![Ein Browserfenster anzuzeigen Hello World in Azure](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)


## <a name="shutting-down-your-azure-virtual-machine"></a>Herunterfahren des virtuellen Computers Azure

Wenn Sie mit diesem Lernprogramm, Herunterfahren oder entfernen fertig neu erstellten Azure virtuellen Computers Ressourcen für andere Lernprogramme und Azure Benutzungsgebühren zu vermeiden.
