<properties
   pageTitle="Debuggen von apps in einem lokalen Andockfenster Container | Microsoft Azure"
   description="Erfahren Sie, wie eine Anwendung ändern, die in einem lokalen Andockfenster Container ausgeführt wird, aktualisieren Sie den Container bearbeiten und aktualisieren und Festlegen von Debughaltepunkten"
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
   ms.date="07/22/2016"
   ms.author="mlearned" />

# <a name="debugging-apps-in-a-local-docker-container"></a>Debuggen von apps in einem lokalen Andockfenster container

## <a name="overview"></a>Übersicht
Visual Studio-Tools für Andockfenster bietet ein konsistentes Verfahren zum Entwickeln ein und überprüfen Sie die Anwendung lokal in einem Container Linux Andockfenster.
Sie müssen Container machen einen Code ändern jedes Mal neu gestartet.
Dieser Artikel wird veranschaulichen die Funktion "Bearbeiten und aktualisieren" Starten einer ASP.NET Core Web app in einem lokalen Andockfenster Container, ändern und aktualisieren Sie den Browser, um diese Änderungen.
Es werden auch Haltepunkte zum Debuggen festgelegt angezeigt.

> [AZURE.NOTE] Windows-Container-Unterstützung wird in zukünftigen Versionen kommen

## <a name="prerequisites"></a>Erforderliche Komponenten
Die folgenden Tools installiert werden müssen.

- [Visual Studio 2015 Update 2](https://go.microsoft.com/fwlink/?LinkId=691978)
- Installieren Sie [Visual Studio 2015 Update 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)

Andockfenster Container ausführen, benötigen einen lokalen Andockfenster Client Sie.
Mithilfe der freigegebenen [Andockfenster Toolbox](https://www.docker.com/products/overview#/docker_toolbox) die Hyper-V deaktiviert werden muss, oder [Windows Beta Andockfenster](https://beta.docker.com) Hyper-V verwendet und erfordert Windows 10 verwenden.

Verwenden der Toolbox Andockfenster müssen [Andockfenster Client](./vs-azure-tools-docker-setup.md) konfigurieren Sie

## <a name="1-create-a-web-app"></a>1. erstellen Sie 1. eine Webanwendung

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Andockfenster Unterstützung hinzufügen

[AZURE.INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]


## <a name="3-edit-your-code-and-refresh"></a>3. bearbeiten Sie Code und aktualisieren

Um Änderungen zu berücksichtigen, können Anwendung innerhalb eines Containers und weiterhin ändern, und wie IIS Express anzeigen.

1. Legen Sie die Projektmappenkonfiguration auf `Debug` und drücken Sie ** &lt;STRG + F5 >** die Andockfenster erstellen und lokal ausführen.

    Container-Bild erstellt wurde und in einem Container Andockfenster ausgeführt wird, startet Visual Studio Web app im Standardbrowser.
    Wenn Sie den Browser Microsoft Edge oder andernfalls siehe [Problembehandlung bei](vs-azure-tools-docker-troubleshooting-docker-errors.md) Fehlern.

1. Die über Seite, wo wir unsere geändert werden.

1. Zurück zu Visual Studio, und öffnen Sie `Views\Home\About.cshtml`.

1. Fügen Sie den folgenden HTML-Inhalt an das Ende der Datei und speichern Sie der.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```

1.  Im Ausgabefenster anzeigen die .NET erstellt und diese Zeilen sehen, wechseln Sie zu Ihrem Browser und aktualisieren Sie die Seite über

    ```
    Now listening on: http://*:80
    Application started. Press Ctrl+C to shut down
    ```

1.  Die Änderungen wurden angewendet.

## <a name="4-debug-with-breakpoints"></a>4. Debuggen Sie mit Haltepunkten

Oft benötigen ändert weitere Überprüfung der Debugfeatures von Visual Studio nutzen.

1.  Zurück zu Visual Studio und öffnen`Controllers\HomeController.cs`

1.  Ersetzen Sie den Inhalt der About()-Methode mit der folgenden:

    ```
    string message = "Your application description page from wthin a Container";
    ViewData["Message"] = message;
    ````

1.  Legen Sie einen Haltepunkt neben der `string message`... Zeile.

1.  Drücken Sie ** &lt;F5 >** Debuggen.

1.  Navigieren Sie zu der Informationsseite der Haltepunkt.

1.  Wechseln Sie zu Visual Studio den Haltepunkt an, und überprüfen Sie den Wert der Nachricht.

    ![][2]

##<a name="summary"></a>Zusammenfassung

Mit [Visual Studio 2015 Tools für Andockfenster](https://aka.ms/DockerToolsForVS)erhalten Sie die Produktivität der Produktion Realismus innerhalb eines Containers Andockfenster Entwicklung lokal arbeiten.

## <a name="troubleshooting"></a>Problembehandlung

[Problembehandlung bei der Entwicklung von Visual Studio Andockfenster](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Weitere Informationen zu Visual Studio und Windows Azure Andockfenster

- [Andockfenster Tools für Visual Studio](http://aka.ms/dockertoolsforvs) - Entwicklung von .NET Core Code in einem container
- [Andockfenster Tools für Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - Erstellung und Bereitstellung Andockfenster Container
- [Andockfenster Tools für Visual Studio-Code](http://aka.ms/dockertoolsforvscode) - Language Services zur Bearbeitung Andockfenster Dateien kommen weitere e2e-Szenarien
- [Windows-Containerinformationen](http://aka.ms/containers)- Nano Informationen und Windows Server
- [Azure Containerservice](https://azure.microsoft.com/services/container-service/) - [des Inhalts von Azure Container](http://aka.ms/AzureContainerService)
-    Weitere Beispiele für die Arbeit mit Andockfenster finden Sie unter [Arbeiten mit Andockfenster](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 verbinden [Demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Weitere Schnellstarts HealthClinic.biz Demo finden Sie unter [Azure Developer Tools Schnellstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Andockfenster Werkzeugen

[Einige große Andockfenster Tools (Steve Lasker Blog)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Gute Artikel

[Einführung Microservices NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Präsentationen

- [Steve Lasker: VS Live Las Vegas 2016 - Andockfenster e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
- [Einführung in ASP.NET Core @ 2016 - wo Sie auf Demo erstellen](https://channel9.msdn.com/Events/Build/2016/B810)
- [Entwicklung von .NET apps in Containern, Channel 9](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
