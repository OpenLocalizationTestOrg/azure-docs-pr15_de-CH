Azure stellt fest, dass Ihre Python **Wenn beides zutrifft Anwendung**:

- Requirements.txt-Datei im Stammverzeichnis
- eine .py-Datei im Stammordner oder runtime.txt, die Python angibt

Wenn dies der Fall ist, verwendet es eine Python Bereitstellungsskript die standardmäßige Synchronisierung von Dateien sowie zusätzliche Python-Vorgänge wie ausführt:

- Automatische Verwaltung der virtuellen Umgebung
- Installation von Paketen mit Pip requirements.txt aufgelistet
- Erstellung der entsprechenden Datei web.config basierend auf der ausgewählten Python-Version.
- Dateien Sie statische für Django Applikationen

Sie können bestimmte Bereitstellungsschritte standardmäßig ohne das Skript anpassen steuern.

Wenn Sie alle Python bestimmte Bereitstellungsschritte überspringen möchten, können Sie diese leere Datei erstellen:

    \.skipPythonDeployment

Wenn Sie mehrere statische Dateien für die Anwendung Django überspringen möchten:

    \.skipDjango 

Für mehr Kontrolle über die Bereitstellung können Sie das Bereitstellungsskript Standard überschreiben, erstellen Sie die folgenden Dateien:

    \.deployment
    \deploy.cmd

[Azure Befehlszeilenschnittstelle][] können Sie um die Dateien zu erstellen.  Verwenden Sie diesen Befehl aus dem Projektordner:

    azure site deploymentscript --python

Wenn diese Dateien existieren, Azure temporäre Bereitstellungsskript erstellt und ausgeführt.  Sie ist identisch mit dem obigen Befehl erstellen.

[Azure Befehlszeilenschnittstelle]: http://azure.microsoft.com/downloads/
