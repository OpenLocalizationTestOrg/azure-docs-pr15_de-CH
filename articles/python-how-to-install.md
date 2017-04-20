<properties
    pageTitle="Installieren Sie Python und SDK - Azure"
    description="Informationen Sie zum Installieren von Python und das SDK mit Azure."
    services=""
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="installing-python-and-the-sdk"></a>Installation von Python und SDK

Python lässt sich unter Windows und Mac, Linux und [Windows Bash](https://msdn.microsoft.com/commandline/wsl/about)wird vorinstalliert geliefert. Dieses Handbuch führt Sie durch die Installation und erste Computers direkt in Azure.

## <a name="whats-in-the-python-azure-sdk"></a>Neuigkeiten in Python Azure SDK?

Azure SDK Python enthält Komponenten, die Sie entwickeln, bereitstellen und Verwalten von Python Anträge Azure ermöglichen. Azure SDK Python umfasst Folgendes:

* **Management-Bibliotheken**. Diese Klassenbibliotheken bieten eine Schnittstelle Verwalten von Azure Ressourcen wie Speicher virtueller Maschinen.

* **Laufzeitbibliotheken**. Diese Klassenbibliotheken bieten eine Schnittstelle für den Zugriff auf Azure-Funktionen wie Storage und Service Bus.

## <a name="which-python-and-which-version-to-use"></a>Die Python und welche Version verwendet

Gibt es verschiedene Arten von Python-Interpreter - Beispiele:

* CPython - standard und am häufigsten verwendeten Python-interpreter
* PyPy - schnelle und kompatible alternative Implementierung, CPython
* IronPython - Python-Interpreter auf .net-CLR ausgeführt wird
* Jython - Python Befehlsinterpreter, die bzw. der Java Virtual Machine

**CPython** Version 2.7 werden oder v3. 3 + PyPy 5.4.0 getestet und unterstützt Python Azure SDK.

## <a name="where-to-get-python"></a>Wo werden Python zu?

Es gibt mehrere Arten CPython:

* Direkt von [www.python.org][]
* Von einem seriösen Distribution wie [www.continuum.io][], [www.enthought.com][] oder [www.activestate.com][]
* Erstellen von Quelle!

Wenn bestimmte benötigen sollten die ersten beiden Optionen.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>SDK-Installation auf Windows, Linux und Mac OS (nur Clientbibliotheken)

Wenn Python installiert noch können Pip Sie bündeln alle Clientbibliotheken im vorhandenen Python 2.7 oder Python 3.3 + Umgebung installieren. Die Pakete werden von [Python Package Index][] (PyPI) herunterladen.

Sie benötigen Administratorrechte:

- Linux und MacOS, verwenden die `sudo` Befehl: `sudo pip install azure-mgmt-compute`.
- Windows: Öffnen Sie die PowerShell-Befehl Prompt als administrator

Sie können einzeln jede Bibliothek für jede Azure Service:

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Vorschau-Pakete mit installiert die `--pre` Flag:

```console
   $ pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

Sie können auch eine Gruppe von Azure Bibliotheken in einer Zeile mit dem `azure` Meta-Paket. Da jedoch nicht alle Pakete in diesem Meta-Paket als stabil veröffentlicht werden die `azure` Meta-Paket ist weiterhin in der Vorschau. Jedoch können die Core-Pakete, Code Quality-Vollständigkeit Perspektiven "stabil" zu diesem Zeitpunkt gelten
- Es ist offiziell als solche synchron mit anderen Sprachen so bald wie möglich gekennzeichnet. Wir planen keine weitere Änderungen erst dann.

Ist eine Vorabversion müssen mit den `--pre` Flag:

```console
   $ pip install --pre azure
```
   
direkt oder

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Immer mehr Pakete

[Python Package Index][] (PyPI) hat eine umfangreiche Auswahl von Python-Bibliotheken.  Wenn Sie eine Distribution installieren, haben Sie bereits die meisten interessanten Bits für Szenarien Webentwicklung Technical Computing.


## <a name="python-tools-for-visual-studio"></a>Python-Tools für Visual Studio

[Python-Tools für Visual Studio][] (PTVS) ist ein Plugin/OSS von Microsoft VS in eine vollwertige Python IDE wird:

![How-to-Install-Python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Mit PTVS ist optional, aber empfohlen, wie es Ihnen Unterstützung für Python und Web Projektmappe debuggen, profiling, interaktiven Fenster, Bearbeitung und Intellisense.

PTVS vereinfacht auch die Microsoft Azure mit Unterstützung für die Bereitstellung [Cloud-Dienste][] und [Websites][]bereitstellen.

PTVS funktioniert mit Ihrem vorhandenen Visual Studio 2013 oder 2015 Installation.  Dokumentation, Downloads und Diskussionen finden Sie unter [Python-Tools für Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Azure Szenarien für Linux und Mac OS

Für Linux oder MacOS Azure Hauptszenarien, die unterstützt werden:

1. Nutzung der Azure Dienste mit den Clientbibliotheken für Python

2. Ihre app im Linux VM ausgeführt

3. Entwickeln und Veröffentlichen von Azure Websites mit Git

Im erste Szenario können Sie umfangreiche webapps erstellen, die Funktionen PaaS Azure [BLOB-Speicher][], [Warteschlangenspeicher][] [Tabellenspeicher][] usw. über Pythonische Wrapper für Azure REST-APIs nutzen. Diese funktioniert genauso wie in Windows, Mac und Linux.  Sie können auch diese Clientbibliotheken von Ihrem lokalen Entwicklungscomputer oder einem Linux VM auf Windows Azure ausgeführte.

VM-Szenario einfach ein Linux VM Ihrer Wahl (Ubuntu, CentOS Suse) starten und ausführen und managen Sie wie.  Beispielsweise können [IPython][] REPL/Notebook auf Ihrem Mac/Windows/Linux-Computer ausführen und eine Linux oder Windows Multiprozessor-VM IPython Engine auf Windows Azure ausgeführte Browser darauf. Finden Sie im Lernprogramm [IPython Notebook auf Azure][] Weitere Informationen.

Informationen zum Einrichten einer Linux-VM finden Sie im Lernprogramm [erstellen einen virtuellen Linux ausgeführt][] .

Git-Bereitstellung können Sie entwickeln eine Anwendung Python und von einem Betriebssystem auf ein Azure-Website veröffentlichen.  Wenn Sie das Repository in Azure drücken, erstellt automatisch eine virtuelle Umgebung und Pip erforderlichen Pakete installieren.

Weitere Informationen zum Entwickeln und Veröffentlichen von Azure-Websites finden Sie unter den Lernprogrammen [Erstellen Websites mit Django][] [Flasche Websites erstellen][]und [Erstellen von Websites mit Kolben][]. Weitere allgemeine Informationen über WSGI-kompatiblen Framework finden Sie unter [Python mit Azure Websites konfigurieren][].


## <a name="additional-software-and-resources"></a>Zusätzliche Software und Ressourcen:

* [Azure SDK für Python-ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure SDK für Python Github](https://github.com/Azure/azure-sdk-for-python)
* [Offizielle Azure Beispiele für Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Kontinuierliche Analyse Python Verteilung][]
* [Enthought Python-Verteilung][]
* [ActiveState Python-Verteilung][]
* [SciPy - Suite wissenschaftlichen Python-Bibliotheken][]
* [NumPy - Numerikbibliothek für Python][]
* [Django Projekt - Reifen Web Framework/CMS][]
* [IPython - eine erweiterte REPL/Notebook für Python][]
* [IPython Notebook auf Azure][]
* [Python-Tools für Visual Studio GitHub][]
* [Python-Entwicklercenter](/develop/python/)

[Kontinuierliche Analyse Python Verteilung]: http://continuum.io
[Enthought Python-Verteilung]: http://www.enthought.com
[ActiveState Python-Verteilung]: http://www.activestate.com
[www.Python.org]: http://www.python.org
[www.Continuum.IO]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.ActiveState.com]: http://www.activestate.com
[SciPy - Suite wissenschaftlichen Python-Bibliotheken]: http://www.scipy.org
[NumPy - Numerikbibliothek für Python]: http://www.numpy.org
[Django Projekt - Reifen Web Framework/CMS]: http://www.djangoproject.com
[IPython - eine erweiterte REPL/Notebook für Python]: http://ipython.org
[IPython]: http://ipython.org
[IPython Notebook auf Azure]: virtual-machines-linux-jupyter-notebook.md
[Cloud-Dienste]: cloud-services-python-ptvs.md
[Websites]: web-sites-python-ptvs-django-mysql.md
[Python-Tools für Visual Studio]: http://aka.ms/ptvs
[Python-Tools für Visual Studio GitHub]: https://github.com/microsoft/ptvs
[Python Package Index]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[Setting up a Linux VM via the Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How to use the Azure Command-Line Interface]: crossplat-cmd-tools.md
[Erstellen Sie einen virtuellen Computer mit Linux]: virtual-machines-linux-quick-create-cli.md
[Erstellen von Websites mit Django]: web-sites-python-create-deploy-django-app.md
[Erstellen von Websites mit Flasche]: web-sites-python-create-deploy-bottle-app.md
[Erstellen von Websites mit Kolben]: web-sites-python-create-deploy-flask-app.md
[Konfigurieren von Python mit Azure Websites]: web-sites-python-configure.md
[Tabelle speichern]: storage-python-how-to-use-table-storage.md
[Warteschlangenspeicher]: storage-python-how-to-use-queue-storage.md
[BLOB-Speicher]: storage-python-how-to-use-blob-storage.md
