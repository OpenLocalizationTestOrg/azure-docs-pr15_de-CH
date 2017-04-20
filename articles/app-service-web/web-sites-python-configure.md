<properties 
    pageTitle="Konfigurieren von Python mit Azure App Service webapps" 
    description="In diesem Lernprogramm werden Optionen zum Erstellen und Konfigurieren eines grundlegenden Webservers Gateway Interface (WSGI) kompatible Python Anwendung in Azure App Service Web Apps beschrieben." 
    services="app-service" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/26/2016" 
    ms.author="huvalo"/>




# <a name="configuring-python-with-azure-app-service-web-apps"></a>Konfigurieren von Python mit Azure App Service webapps

In diesem Lernprogramm werden die Optionen zum Erstellen und Konfigurieren einer einfachen Web Server Gateway Interface (WSGI) kompatiblen Python Anwendung in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)beschrieben.

Weitere Features der Git Bereitstellung Paketinstallation mit requirements.txt wie virtuelle Umgebung beschrieben.


## <a name="bottle-django-or-flask"></a>Flaschen, Django oder Kolben?

Azure Marketplace enthält Vorlagen für Frameworks Flaschen, Django und Kolben. Entwickeln Ihrer ersten Anwendung in Azure App Service oder Sie Git nicht kennen, sollten Sie diese Lernprogramme folgen umfasst eine schrittweise Anleitung zum Erstellen einer Anwendung arbeiten, aus der Galerie Git Bereitstellung von Windows oder Mac:

- [Web Apps mit Flasche](web-sites-python-create-deploy-bottle-app.md)
- [Web Apps mit Django](web-sites-python-create-deploy-django-app.md)
- [Web Apps mit Kolben](web-sites-python-create-deploy-flask-app.md)


## <a name="web-app-creation-on-azure-portal"></a>Web app erstellen Azure-Portal

Diese praktische Einführung geht ein Azure-Abonnement und Zugriff auf das Azure-Portal.

Wenn Sie eine vorhandenes Web app nicht haben, können Sie aus dem [Azure-Portal](https://portal.azure.com)erstellen.  Die neue-Schaltfläche in der oberen linken Ecke klicken Sie auf **Web + Mobile** > **WebApp**.

## <a name="git-publishing"></a>Git veröffentlichen

Konfigurieren Sie Git Veröffentlichung Ihrer neu erstellten Anwendung wie bei [Lokalen Git Bereitstellung Azure App Service](app-service-deploy-local-git.md). Diese praktische Einführung verwendet Git erstellen, verwalten und veröffentlichen unsere Python WebApp Azure App Service.

Sobald Git veröffentlichen eingerichtet ist, wird ein Git Repository erstellt und Ihrer Anwendung zugeordnet. Die Repository-URL wird angezeigt und kann nun zum Senden der Daten aus der lokalen Development Environment in die Cloud. Zum Veröffentlichen der Anwendung über Git Git Client ebenfalls installiert ist und Anweisungen müssen Webinhalte app Azure App Service verwenden.


## <a name="application-overview"></a>Anwendungsübersicht

In den nächsten Abschnitten werden die folgenden Dateien erstellt. Sie sollten im Stammverzeichnis der Git Repository befinden.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>WSGI-Handler

WSGI ist eine Python [PEP 3333](http://www.python.org/dev/peps/pep-3333/) definieren Sie eine Schnittstelle zwischen dem Webserver und Python beschrieben. Es bietet eine standardisierte Schnittstelle verschiedene ASP.NET-Webanwendungen und Frameworks mit Python geschrieben. Beliebte Python Web Frameworks verwenden heute WSGI. Azure App Service Web Apps bietet für diese Frameworks Unterstützung; Darüber hinaus können erfahrene Benutzer auch eigene erstellen als benutzerdefinierte Handler WSGI Spezifikation Richtlinien einhalten.

Hier ist ein Beispiel für ein `app.py` , einen benutzerdefinierten Handler definiert:

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

Führen Sie diese Anwendung lokal mit `python app.py`, wechseln Sie zu `http://localhost:5555` in Ihrem Webbrowser.


## <a name="virtual-environment"></a>Virtuelle Umgebung

Auch Beispiel-app über externe Pakete erfordert, ist es wahrscheinlich, dass die Anwendung einige benötigen.

Zum Verwalten der externen paketabhängigkeiten unterstützt Azure Git Bereitstellung die Erstellung virtueller Umgebungen.

Erkennt Azure ein requirements.txt im Stammverzeichnis des Repositorys, erstellt automatisch eine virtuelle Umgebung mit dem Namen `env`. Dies tritt nur bei der ersten Bereitstellung oder während der Bereitstellung nach ausgewählten Python Runtime geändert wurde.

Sie möchten wahrscheinlich Erstellen einer virtuellen Umgebung für Entwicklung, aber nicht in der Git Repository einfügen.


## <a name="package-management"></a>Verwalten von Paketen

In requirements.txt aufgeführten Pakete werden in der virtuellen Umgebung mit Pip installiert. Dies tritt bei jeder Bereitstellung jedoch Pip überspringt Installation, wenn ein Paket bereits installiert ist.

Beispiel `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Python-Version

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Beispiel `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config

Sie müssen zum Erstellen einer web.config-Datei, um anzugeben, wie der Server Anfragen verarbeitet.

Beachten Sie, dass Sie eine web.x.y.config in Ihrem Repository haben, wobei x.y entspricht der ausgewählten Python-Runtime und Azure kopiert automatisch die entsprechende Datei Web.config.

Web.config-Beispiele basieren auf ein Proxyskript virtuelle Umgebung, die im nächsten Abschnitt beschrieben.  Sie arbeiten mit der im Beispiel verwendeten WSGI `app.py` oben.

Beispiel `web.config` für Python 2.7:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Beispiel `web.config` für Python 3.4:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Statische Dateien werden vom Webserver direkt behandelt ohne Python-Code zur Verbesserung der Leistung.

In den Beispielen oben sollte der Speicherort der statischen Dateien auf dem Datenträger den Speicherort in der URL übereinstimmen. Dies bedeutet, dass eine Anforderung für `http://pythonapp.azurewebsites.net/static/site.css` dienen zur Datei `\static\site.css`.

`WSGI_ALT_VIRTUALENV_HANDLER`ist dem WSGI-Handler angeben. In den Beispielen oben hat `app.wsgi_app` ist der Handler eine Funktion namens `wsgi_app` in `app.py` im Stammordner.

`PYTHONPATH`kann angepasst werden, aber wenn Sie die Abhängigkeit in der virtuellen Umgebung durch deren Angabe in requirements.txt installieren, Sie sollten sie ändern müssen.


## <a name="virtual-environment-proxy"></a>Virtuelle Umgebung Proxy

Das folgende Skript wird zum Abrufen des WSGI-Handlers, aktivieren die virtuelle Umgebung und protokollieren Sie Fehler. Es soll generisch und ohne Modifikationen.

Inhalt des `ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n');

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')
    
        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)
    
        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()
    
        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))
    
        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []
    
        site.main()
    
        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>Git-Bereitstellung anpassen

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]


## <a name="troubleshooting---package-installation"></a>Problembehandlung - Paketinstallation

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Fehlerbehebung – virtuelle Umgebung

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Python Developer Center](/develop/python/).

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert
* Eine Anleitung zur Änderung von Websites zu App Service finden Sie unter: [Azure App Service und seine Auswirkung auf vorhandene Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)





 
