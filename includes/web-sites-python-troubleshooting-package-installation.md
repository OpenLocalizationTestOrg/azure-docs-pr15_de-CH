Einige Pakete können nicht installiert mit Pip auf Azure ausgeführt.  Es möglicherweise einfach, dass das Paket nicht Python Package Index verfügbar ist.  Es könnte sein, dass ein Compiler erforderlich (ein Compiler ist nicht auf dem Computer Web app in Azure App Service).

In diesem Abschnitt betrachten wir Umgang mit diesem Problem.

### <a name="request-wheels"></a>Anforderung Räder

Die Paketinstallation einen Compiler benötigt, sollten Sie Kontakt Besitzer des Pakets, um anzufordern, dass die Räder für das Paket verfügbar gemacht werden.

Mit der aktuellen Verfügbarkeit der [Microsoft Visual C++-Compiler für Python 2.7][]ist es jetzt einfacher Pakete erstellen, die systemeigenen Code für Python 2.7.

### <a name="build-wheels-requires-windows"></a>Erstellen der Räder (erfordert Windows)

Hinweis: Bei Verwendung dieser Option müssen Sie das Paket eine Python-Umgebung, die Plattform/Architektur/Version übereinstimmt, die Web-Anwendung in Azure App Service kompilieren (Windows/32-bit/2.7 oder 3.4).

Wenn das Paket installiert ist, da es einen Compiler erfordert können vom Compiler auf Ihrem lokalen Computer installieren und erstellen ein Rad für das Paket, das Sie dann in Ihrem Repository enthält.

Mac-Linux-Benutzer: Haben Sie Zugriff auf einen Windows-Computer, finden Sie unter [Erstellen einer virtuellen Maschine unter Windows][] für eine VM auf Azure zu erstellen.  Sie können die Räder erstellen, zum Repository hinzufügen und die VM ggf. verwerfen. 

Python 2.7 können Sie [Microsoft Visual C++-Compiler für Python 2.7][]installieren.

3.4 Python können Sie [Microsoft Visual C++ 2010 Express][]installieren.

Um Rollen zu erstellen, benötigen Sie das Rad-Paket:

    env\scripts\pip install wheel

Verwenden Sie `pip wheel` , eine Abhängigkeit zu kompilieren:

    env\scripts\pip wheel azure==0.8.4

Dies erstellt eine Datei speichern, im Ordner \wheelhouse.  \Wheelhouse Ordner und Rad-Dateien zum Repository hinzufügen.

Bearbeiten der requirements.txt hinzufügen der `--find-links` oben. Dies weist Pip sich nach einer exakten Übereinstimmung im lokalen Ordner vor dem Index Paket Python.

    --find-links wheelhouse
    azure==0.8.4

Wunsch alle Abhängigkeiten im Ordner \wheelhouse und Python Package Index nicht zu verwenden, Sie können erzwingen, Pip ignoriert paketindex hinzufügen `--no-index` am Anfang der requirements.txt.

    --no-index

### <a name="customize-installation"></a>Installation anpassen

Das Bereitstellungsskript zum Installieren eines Pakets in der virtuellen Umgebung mit einem anderen Installer wie leicht anpassen\_installieren.  Siehe deploy.cmd Beispiel auskommentiert wird.  Stellen Sie sicher, dass solche Pakete in requirements.txt Pip Installation verhindern aufgeführt sind.

Das Bereitstellungsskript Folgendes hinzu:

    env\scripts\easy_install somepackage

Sie möglicherweise auch einfach\_installieren, um eine Exe-Installer (zip kompatibel, einfach einige\_Installation unterstützt).  Das Repository das Installationsprogramm hinzu, und rufen Sie einfach\_installieren und den Pfad der ausführbaren Datei.

Das Bereitstellungsskript Folgendes hinzu:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-the-virtual-environment-in-the-repository-requires-windows"></a>Enthalten Sie die virtuelle Umgebung im Repository (erfordert Windows)

Hinweis: Bei Verwendung dieser Option müssen Sie eine virtuelle Umgebung verwenden, die Plattform/Architektur/Version übereinstimmt, die Web-Anwendung in Azure App Service (Windows/32-bit/2.7 oder 3.4).

Wenn Sie die virtuelle Umgebung in das Repository einfügen, können Sie verhindern, dass das Bereitstellungsskript Management der virtuellen Umgebung auf Azure, indem eine leere Datei erstellen:

    .skipPythonDeployment

Wir empfehlen löschen vorhandene virtuelle Umgebung auf die Anwendung, dass übrig gebliebene Dateien beim virtuelle Umgebung automatisch verwaltet wurde.


[Erstellen einer virtuellen Maschine unter Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++-Compiler für Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
