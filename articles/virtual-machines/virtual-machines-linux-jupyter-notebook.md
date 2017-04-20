<properties
    pageTitle="Erstellen ein Notizbuchs Jupyter-IPython | Microsoft Azure"
    description="Weitere Informationen zum Bereitstellen von Jupyter-IPython-Notizbuch auf einem Linux-Computer mit der Ressourcen-Manager-Bereitstellungsmodell in Azure erstellt."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="crwilcox"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="python"
    ms.topic="article"
    ms.date="11/10/2015"
    ms.author="crwilcox"/>

# <a name="jupyter-notebook-on-azure"></a>Jupyter Notebook auf Azure

Das [Jupyter Projekt](http://jupyter.org), früher [IPython Projekt](http://ipython.org)stellt eine Sammlung von Tools für wissenschaftliche Datenverarbeitung mit leistungsstarken interaktive Shell, die Ausführung von Code bei der Erstellung eines live rechnerische kombinieren. Diese Notebook-Dateien können beliebige, mathematische Formeln, eingegebenen Code, Ergebnisse, Grafiken, Videos und andere Arten von Medien ist ein moderne Webbrowser anzeigen enthalten. Absolut neu in Python und Einführung in eine interessante, interaktive Umgebung oder möchten einige schwerwiegende technischen Parallel computing ist Jupyter Notebook eine hervorragende Wahl.

![Abbildung](./media/virtual-machines-linux-jupyter-notebook/ipy-notebook-spectral.png) Pakete analysieren die Struktur einer Aufnahme mithilfe von SciPy und Matplotlib.


## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter zwei Methoden: Azure Notebooks oder benutzerdefinierte Bereitstellung
Azure bietet, die Sie zum [schnellen Einstieg Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx)verwenden können.  Mithilfe der Azure-Notebook-Service können Sie problemlos skalierbar Datenverarbeitungsressourcen mit der Leistung von Python Webordner Schnittstelle des Jupyter und viele Bibliotheken zugreifen.  Da die Installation vom Dienst behandelt wird, können Benutzer diese Ressourcen ohne Verwaltung und Konfiguration der Benutzer zugreifen.

Wenn der Notebook-Service für Ihr Szenario nicht funktioniert bitte weiterhin die Jupyter Notebook auf Microsoft Azure bereitstellen zeigen Ihnen wird mit Linux virtuellen Maschinen (VMs) in diesem Artikel.

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Erstellen und Konfigurieren einer VM auf Azure

Zunächst ist die Erstellung eine virtuellen Maschine (VM) in Azure ausgeführt.
Diese VM ist ein vollständiges Betriebssystem in der Cloud und Jupyter Notebook ausführen verwendet werden. Azure kann Linux und Windows virtuelle Computer ausgeführt und die Einrichtung von Jupyter für beide Typen von virtuellen Maschinen werden behandelt.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Linux VM erstellen und Öffnen von Ports für Jupyter

Gehen Sie [hier] angegebenen[ portal-vm-linux] zum Erstellen eines virtuellen Computers *Ubuntu* Verteilung. Diese praktische Einführung verwendet Ubuntu Server 14.04 LTS. Wir nehmen Benutzer namens *Azureuser*.

Nachdem der virtuelle Computer bereitgestellt müssen wir eine Sicherheitsregel Sicherheitsgruppe Netzwerk öffnen.  Azure-Portal zur **Netzwerk-Sicherheitsgruppen** und öffnen Sie die Registerkarte für die Sicherheitsgruppe für die VM. Sie müssen eine eingehende Regel folgendermaßen hinzufügen: das Protokoll **TCP** **\*** Quellport (öffentlich) und **9999** für den Zielport (privat).

![Screenshot](./media/virtual-machines-linux-jupyter-notebook/azure-add-endpoint.png)

In Ihrem Netzwerk-Sicherheitsgruppe auf **Netzwerkschnittstellen** , und beachten Sie bei Bedarf für die Verbindung der VM im nächsten Schritt wird die **Öffentliche IP-Adresse** .

## <a name="install-required-software-on-the-vm"></a>Erforderliche Software auf dem virtuellen Computer installieren

Führen Sie Jupyter Notebook auf unsere VM installieren wir zuerst Jupyter und Abhängigkeit. Mit den Linux Vm mit ssh und Benutzername/Kennwort Koppeln Sie gewählt haben, wenn die Vm erstellt. In diesem Lernprogramm werden wir kitten verwenden und Windows verbinden.

### <a name="installing-jupyter-on-ubuntu"></a>Installieren von Jupyter auf Ubuntu
Anaconda, ein bekanntes Wissenschaft Python Verteilung mithilfe von [Kontinuum Analytics](https://www.continuum.io/downloads)bereitgestellt Links zu installieren.  Zum Zeitpunkt der Erstellung dieses Dokuments die folgenden Hyperlinks sind die älteren Versionen.

#### <a name="anaconda-installs-for-linux"></a>Anaconda Installation für Linux
<table>
  <th>Python 3.4</th>
  <th>Python 2.7</th>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64-bit</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64-bit</href>
    </td>
  </tr>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32-bit</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32-bit</href>
    </td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a>Anaconda3 Installieren von 64-Bit-on Ubuntu 2.3.0
Beispielsweise ist dies wie Sie Ubuntu Anaconda installieren können

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter to the latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Screenshot](./media/virtual-machines-linux-jupyter-notebook/anaconda-install.png)


### <a name="configuring-jupyter-and-using-ssl"></a>Jupyter konfigurieren und Verwenden von SSL
Nach der Installation müssen wir kurz die Konfigurationsdateien für Jupyter einrichten. Wenn Probleme treten bei der Konfiguration der Jupyter [Jupyter Notebook läuft zu](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html)betrachten hilfreich sein können.

Weiter wir `cd` zum Profilverzeichnis SSL-Zertifikat erstellen und Bearbeiten der Konfigurationsdatei Profile.

Unter Linux Befehl.

    cd ~/.jupyter

Verwenden Sie den folgenden Befehl, um das SSL-Zertifikat (Linux und Windows) erstellen.

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Beachten Sie, dass, da wir ein selbstsigniertes Zertifikat erstellen und mit dem Notebook Browser einen Sicherheitshinweis wird Ihnen.  Für die langfristige Produktion sollten Sie ein ordnungsgemäß signiertes Zertifikat Ihrer Organisation zugeordnet.  Zertifikatsmanagement im Rahmen dieser Demo ist es ein selbstsigniertes Zertifikat jetzt bleiben.

Zusätzlich ein Zertifikat verwenden, müssen Sie auch ein Kennwort für Ihr Notebook vor unbefugtem Zugriff geschützt angeben.  Aus Sicherheitsgründen verwendet Jupyter verschlüsselte Kennwörter in seiner Konfigurationsdatei müssen Sie zuerst das Kennwort verschlüsselt.  IPython bietet ein Dienstprogramm dazu. Führen Sie den folgenden Befehl in der Befehlszeile.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

Diese aufgefordert, ein Kennwort und die Bestätigung und druckt anschließend das Kennwort. Beachten Sie dies für den folgenden Schritt aus.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

Als Nächstes bearbeiten wir das Profil-Konfigurationsdatei, die die `jupyter_notebook_config.py` Datei im wird Verzeichnis.  Beachten Sie, dass diese Datei möglicherweise nicht vorhanden ist – nur erstellen, wenn dies der Fall ist.  Diese Datei hat eine Reihe von Feldern und standardmäßig alle sind auskommentiert.  Öffnen Sie diese Datei mit einem beliebigen Texteditor Ihren Wünschen entspricht, und Sie sollten mindestens den folgenden Inhalt hat. **C.NotebookApp.password in der Konfigurationsdatei mit sha1 aus dem vorherigen Schritt ersetzen**.

    c = get_config()

    # You must give the path to the certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-the-jupyter-notebook"></a>Führen Sie Jupyter Notebook

An diesem Punkt sind wir Jupyter Notebook starten. Hierzu wechseln Sie zum Verzeichnis, Notebooks und starten Sie den Server Jupyter Notebook mit dem folgenden Befehl werden soll.

    /anaconda3/bin/jupyter-notebook

Sie sollte jetzt Ihre Jupyter Notizbuchs an der Adresse `https://[PUBLIC-IP-ADDRESS]:9999`.

Beim Aufrufen von Ihrem Notizbuch fordert die Anmeldeseite Ihr Kennwort. Und sobald Sie sich anmelden, Sie sehen "Jupyter Notebook Dashboard" ist der uerelement für alle Notebooks.  Auf dieser Seite können neue Notizbücher zu erstellen und vorhandene zu öffnen.

![Screenshot](./media/virtual-machines-linux-jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a>Mithilfe der Jupyter Notebook

Wenn Sie die Schaltfläche **neu** klicken, sehen Sie die folgende Seite öffnen.

![Screenshot](./media/virtual-machines-linux-jupyter-notebook/jupyter-untitled-notebook.png)

Markiert der Bereich mit einer `In []:` Aufforderung ist der Bereich, und geben Sie hier alle gültigen Python Code und führt Sie bei `Shift-Enter` oder klicken Sie auf "Wiedergabe" (das Dreieck nach rechts in der Symbolleiste).

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>Ein leistungsfähiges Paradigma: live rechnerische Dokumente mit rich Media

Die Notebooks selbst sollte sehr natürlich, wer Python und ein Textverarbeitungsprogramm verwendet hat fühlen, ist in gewisser Hinsicht eine Mischung beider: Blöcke von Python-Code ausführen, aber Sie können weiterhin Notizen und anderen Text durch Ändern einer Zelle "Code", "Abzug" über das Dropdown-Menü in der Symbolleiste.

Jupyter ist mehr als ein Textverarbeitungsprogramm wie ermöglicht das Kombinieren von Berechnung und Rich Media (Text, Grafiken, Video und praktisch alles, was ein modernen Webbrowser angezeigt werden kann). Sie können kombinieren, Text, Code, Videos und mehr!

![Screenshot](./media/virtual-machines-linux-jupyter-notebook/jupyter-editing-experience.png)

Und mit der Python viele hervorragenden Bibliotheken für wissenschaftliche und technische Datenverarbeitung im folgenden Screenshot eine einfache Berechnung problemlos einen komplexen Netzwerk-Analyse, in einer Umgebung ausgeführt werden kann.

Dieses Paradigma macht das moderne Internet mit live Berechnung mischen bietet viele und eignet sich für die Cloud; das Notizbuch kann verwendet werden:

* Rechnerische Scratchpad explorativen aufzeichnen Problem arbeiten.

* Ergebnisse mit einer "live" rechnerische oder Hardcopy-Formaten (HTML, PDF) freigeben.

* Materialien, die Berechnung betreffen, so dass Studenten sofort mit dem richtigen Code experimentieren können verteilen und live Unterricht ändern und interaktiv ausführen.

* Geben Sie "ausführbare Papers", die die Ergebnisse, die sofort wiedergegeben präsentieren, bestätigt und von anderen Benutzern erweitert.

* Als Plattform für die Zusammenarbeit computing: mehrere Benutzer können denselben Notebook Server live rechnerische Sitzung freigeben anmelden.


Gehen IPython Quellcode- [Repository][]finden Sie ein ganzes Verzeichnis Notebook Beispiele downloaden und auf Ihre eigenen Azure Jupyter VM experimentieren.  Laden Sie einfach die `.ipynb` Dateien in der Website und Hochladen auf dem Dashboard des Notizbuchs Azure VM (oder direkt in der VM herunterladen).

## <a name="conclusion"></a>Abschluss

Jupyter Notebook bietet eine leistungsstarke Schnittstelle für den Zugriff auf die Stromversorgung des Ökosystems Python Azure interaktiv.  Es umfasst eine Vielzahl von Szenarios einschließlich einfache Erforschung und Python, Analyse und Visualisierung, Simulation und parallel computing. Die resultierende Notebook Dokumente enthalten eine vollständige Aufzeichnung der Berechnungen, die gemeinsam mit anderen Benutzern von Jupyter durchgeführt werden.  Jupyter Notebook kann als eine lokale Anwendung verwendet werden, aber es ist ideal für die Bereitstellung auf Azure cloud

Hauptfunktionen von Jupyter sind auch verfügbar in Visual Studio über die [Python-Tools für Visual Studio][] (PTVS). PTVS ist ein kostenloses und Open Source-Plug-in von Microsoft, die eine erweiterte Python Entwicklungsumgebung mit einem erweiterten Editor mit IntelliSense, Debuggen, Visual Studio verwandelt, profiling und parallel computing Integration.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Python Developer Center](/develop/python/).

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[Repository]: https://github.com/ipython/ipython
[Python-Tools für Visual Studio]: http://aka.ms/ptvs
