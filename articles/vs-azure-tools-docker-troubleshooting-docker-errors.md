<properties
   pageTitle="Andockfenster Client Fehlerbehebung mit Visual Studio unter Windows | Microsoft Azure"
   description="Behandeln von Problemen, die bei der Verwendung von Visual Studio erstellen und Bereitstellen von Web-apps zu Andockfenster unter Windows mit Visual Studio auftreten."
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="allclark" />

# <a name="troubleshooting-visual-studio-docker-development"></a>Problembehandlung bei der Entwicklung von Visual Studio Andockfenster

Bei der Arbeit mit Visual Studio-Tools für Andockfenster können Probleme aufgrund der Vorschau auftreten.
Es folgen einige häufige Probleme und Lösungsmöglichkeiten.


## <a name="unable-to-validate-volume-mapping"></a>Nicht Volume Zuordnung überprüfen
Volume-Zuordnung muss Ordner app im Container Quellcode und Binärdateien der Anwendung zur Verfügung.  Volumen-Mappings sind in die Andockfenster compose.dev.debug.yml und Andockfenster compose.dev.release.yml Dateien enthalten. Dateien auf dem Hostcomputer geändert, Änderungen der Container diese in eine ähnliche Ordnerstruktur.

Aktivieren Sie Volume Zuordnung öffnen Sie **Einstellungen** über das Taskleistensymbol Andockfenster für "Moby", und wählen Sie die Registerkarte **Freigegebene Laufwerke** .  Sicherstellen Sie, dass der Laufwerkbuchstabe dem Projekt gehostet sowie % USERPROFILE% Speicherort Laufwerkbuchstaben von gemeinsam verwendet überprüfen und anschließend auf **Übernehmen**.

Testen Volume Zuordnung funktioniert nach Laufwerke freigegeben wurden, fordert Wiederherstellung und F5 in Visual Studio oder versuchen Sie, in einen Befehl:

*In einer Windows-Befehlszeile*

*[Hinweis: Dies übernimmt der Benutzerordner befindet sich auf Laufwerk "C" und freigegeben wurde.  Aktualisieren Sie nach Bedarf, wenn Sie ein anderes Laufwerk freigegeben haben]*
```
docker run -it -v /c/Users/Public:/wormhole busybox
```

*In der Linux-container*

```
/ # ls
```

Sie sollten eine Verzeichnisliste vom Benutzer/Öffentliche Ordner sehen.
Wenn keine Dateien angezeigt werden und Ihre /c/Users/Public Ordner ist nicht leer, ist Volume Zuordnung nicht richtig konfiguriert. 

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Ändern Sie in den Inhalt des Verzeichnisses Wurmloch der `/c/Users/Public` Verzeichnis:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

**Hinweis:** *Bei der Arbeit mit Linux VMs das Container wird Groß-/Kleinschreibung.*

##<a name="build--prepareforbuild-task-failed-unexpectedly"></a>Build: "PrepareForBuild"-Aufgabe mit unerwartetem Fehler.

Microsoft.DotNet.Docker.CommandLine.ClientException: Fehler bei dem Versuch, eine Verbindung herzustellen:

Überprüfen Sie, ob der Andockfenster Standardhost ausgeführt wird. Öffnen Sie ein Eingabeaufforderungsfenster und führen:

```
docker info
```

Wenn die zurückgegebene Fehler versuchen **Andockfenster für** desktop app gestartet.  Läuft die desktop app dann das **Moby** -Symbol in der Taskleiste sollte sichtbar sein. Klicken Sie mit der rechten Maustaste auf das Taskleistensymbol und **Öffnen**.  Klicken Sie auf das **Zurücksetzen** und dann **Neustart Andockfenster...**.

##<a name="manually-upgrading-from-version-031-to-040"></a>Manuelles Aktualisieren von Version 0,31 0,40


1. Projekt sichern
1. Löschen Sie die folgenden Dateien im Projekt:

    ```
      Dockerfile
      Dockerfile.debug
      DockerTask.ps1
      docker-compose-yml
      docker-compose.debug.yml
      .dockerignore
      Properties\Docker.props
      Properties\Docker.targets
    ```

1. Schließen Sie die Projektmappe, und entfernen Sie die folgenden Zeilen aus der Datei .xproj:

    ```
      <DockerToolsMinVersion>0.xx</DockerToolsMinVersion>
      <Import Project="Properties\Docker.props" />
      <Import Project="Properties\Docker.targets" />
    ```

1. Öffnen Sie die Projektmappe
1. Entfernen Sie die folgenden Zeilen aus der Datei Properties\launchSettings.json:

    ```
      "Docker": {
        "executablePath": "%WINDIR%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "commandLineArgs": "-ExecutionPolicy RemoteSigned .\\DockerTask.ps1 -Run -Environment $(Configuration) -Machine '$(DockerMachineName)'"
      }
    ```

1. Entfernen Sie die folgenden Dateien für Andockfenster von project.json in der PublishOptions:

    ```
    "publishOptions": {
      "include": [
        ...
        "docker-compose.yml",
        "docker-compose.debug.yml",
        "Dockerfile.debug",
        "Dockerfile",
        ".dockerignore"
      ]
    },
    ```

1. Deinstallieren Sie die frühere Version und installieren Sie Andockfenster Tools 0,40 und **Andockfenster Support-> Add** wieder aus dem Kontextmenü für Ihre ASP.NET-Webseiten Core oder Konsolenanwendungsprojekt. Dadurch wird die neue erforderliche Andockfenster Artefakte zu Ihrem Projekt hinzugefügt. 

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Eine Fehlermeldung tritt beim Versuch auf **Hinzufügen -> Andockfenster Unterstützung** oder (F5) Debuggen einer Anwendung ASP.NET Core in einem container

Wir haben gelegentlich nach deinstallieren und Extensions Installation Visual Studio MEF (Managed Extensibility Framework) Cache kann beschädigt sein. In diesem Fall, dass es verschiedene Fehler kann Dialogfelder beim Support Andockfenster hinzufügen bzw. ausführen oder Debuggen (F5) Ihrer zentralen Anwendung ASP.NET versucht. Führen Sie als vorübergehende Lösung folgendermaßen löschen und MEF Cache-Erneuerung.

1. Schließen Sie alle Instanzen von Visual Studio
1. %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\ öffnen
1. Die folgenden Ordner löschen
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Visual Studio öffnen
1. Wiederholen Sie das Szenario 
