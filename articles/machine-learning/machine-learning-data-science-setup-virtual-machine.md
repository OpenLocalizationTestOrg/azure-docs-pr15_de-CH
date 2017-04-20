<properties
    pageTitle="Richten Sie einen virtuellen Computer als Server IPython Notebook | Microsoft Azure"
    description="Einrichten einer Azure Virtual Machine für die Verwendung in einer Umgebung Wissenschaft IPython Server für erweiterte Analyse festgelegt."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Einrichten von Azure virtuellen Computer als Server IPython Notebook für erweiterte Analyse

In diesem Thema wird zum Bereitstellen und Konfigurieren einer Azure Virtual Machine für erweiterte Analyse als Teil einer Wissenschaft-Umgebung verwendet werden können. Mit Tools wie z. B. IPython Notebook, Azure Storage Explorer, AzCopy sowie andere Dienstprogramme für erweiterte Analyse Projekte ist Windows Virtual Machine konfiguriert. Azure Storage Explorer und AzCopy, ermöglichen z. B. einfache Daten Azure Blob-Speicher vom lokalen Computer hochladen oder von BLOB-Speicher auf Ihrem lokalen Computer herunterladen.

## <a name="create-vm"></a>Schritt 1: Erstellen Sie allgemeine Azure Virtual machine

Wenn Sie ein Azure Virtual Machine bereits, möchte ein IPython Notebook-Server eingerichtet können überspringen Sie diesen Schritt und fahren Sie mit [Schritt2: einen Endpunkt für IPython Notebooks vorhandenen virtuellen Computer hinzufügen](#add-endpoint).

Vor der Erstellung einer virtuellen Maschine auf Azure, müssen Sie die Größe des Computers bestimmen, die zum Verarbeiten der Daten für das Projekt benötigt werden. Weniger Speicher und weniger CPU-Kerne als größere Maschinen kleiner Rechner haben, sind sie auch kostengünstiger. Eine Liste der Computertypen und Preise finden Sie die <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Preise von virtuellen Maschinen</a>

1. Melden Sie <a href="https://manage.windowsazure.com" target="_blank">Azure-Verwaltungsportal an</a>und **auf die untere linke Ecke** . Ein Fenster wird eingeblendet. Wählen Sie **berechnen** -> **virtuellen** -> **FROM GALLERY**.

    ![Arbeitsbereich erstellen][24]

2. Wählen Sie eine der folgenden Bilder:

    * Windows Server 2012 R2 Datacenter
    * Windows Server Essentials-Umgebung (Windows Server 2012 R2)

    Klicken Sie den Pfeil rechts unten rechts zu der nächsten Konfigurationsseite.

    ![Arbeitsbereich erstellen][25]

3. Geben Sie einen Namen für den virtuellen Computer zu erstellen, wählen Sie die Größe des Computers (Standard: A3) anhand von Daten der Computer den Prozess wird und wie stark Sie möchten Computer (Speichergröße und die Anzahl von Prozessorkernen), geben einen Benutzernamen und ein Kennwort für den Computer. Klicken Sie den Pfeil nach rechts zu der nächsten Konfigurationsseite.

    ![Arbeitsbereich erstellen][26]

4. Wählen Sie das **REGION-AFFINITÄT Gruppe und virtuellen Netzwerk** , das **SPEICHERKONTO** enthält, die Sie für diesen virtuellen Computer verwenden möchten, und wählen Sie diesem Storage-Konto. Ein Endpunkt am Ende im Feld **ENDPUNKTE** durch Eingabe des Endpunkts ("IPython" hier) hinzu. Eine beliebige Zeichenfolge können als **Namen** den Endpunkt und jede ganze Zahl zwischen 0 und 65536, **verfügbar** als **Öffentlicher PORT**. **Privater PORT** muss **9999**. Benutzer sollten **vermeiden Sie** öffentliche Ports bereits zugewiesen für Internetdienste. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Ports für Internetdienste</a> enthält eine Liste der Ports, die zugewiesen wurden und sollte vermieden werden.

    ![Arbeitsbereich erstellen][27]

5. Klicken Sie zum Starten der virtuellen Maschine Bereitstellung.

    ![Arbeitsbereich erstellen][28]


15-25 Minuten Bereitstellung VM dauern. Nach dem Erstellen der virtuellen Computer weisen des Status dieses Computers **ausgeführt**.

![Arbeitsbereich erstellen][29]

## <a name="add-endpoint"></a>Schritt 2: Hinzufügen eines Endpunkts IPython Notebooks vorhandenen virtuellen Computer

Wenn Sie den virtuellen Computer wie in Schritt 1 erstellt haben, der Endpunkt für IPython Notebook wurde bereits hinzugefügt und kann dieser Schritt übersprungen.

Wenn bereits die virtuellen Computer müssen Sie einen Endpunkt für IPython Notebook hinzufügen, die Sie in Schritt 3 unten Abmelden in Azure-Verwaltungsportal installieren werden, den virtuellen Computer und den Endpunkt für IPython Notebook-Server hinzufügen. Die folgende Abbildung enthält ein Bildschirmabbild des Portals nach einer virtuellen Windows-Maschine der Endpunkt für IPython Notebook hinzugefügt wurde.

![Arbeitsbereich erstellen][17]

## <a name="run-commands"></a>Schritt 3: Installieren von IPython Notebook und andere unterstützende tools

Nach dem Erstellen der virtuellen Computer verwenden Sie Remote Desktop Protocol (RDP) Anmelden bei Windows Virtual Machine. Eine Anleitung finden Sie unter [Anmelden bei einem virtuellen Windows Server ausgeführt wird](../virtual-machines/virtual-machines-windows-classic-connect-logon.md). **Befehlszeile** (**nicht im Befehlsfenster Powershell**) als **Administrator** öffnen Sie, und führen Sie den folgenden Befehl.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Nach Abschluss der Installation IPython Notebook-Server startet automatisch die *C:\\Benutzer\\\<Benutzername\>\\Dokumente\\IPython Notebooks* Verzeichnis.

Wenn Sie aufgefordert werden, geben Sie ein Kennwort für IPython Notebook und das Kennwort für den Administrator. IPython Notebook als Dienst auf dem Computer ausführen können.

## <a name="access"></a>Schritt 4: Access IPython Notebooks über einen Webbrowser
Zugriff auf den Server IPython Notebook, öffnen Sie ein Web-Browser und Eingabe *https://&#60;virtual DNS-Computername >: & #60; öffentliche >* im Textfeld URL. Hier der *& #60; öffentliche >* sollte die Portnummer angegeben als IPython Notebook Endpunkt hinzugefügt wurde.

Die *& #60; DNS-Name des virtuellen Computers >* finden Sie unter den klassischen Portal Azure. Nach dem klassischen Portal anmelden klicken Sie auf **virtuelle Computer**den Computer erstellten wählen Sie **DASHBOARD**und der DNS-Namen wie folgt angezeigt:

![Arbeitsbereich erstellen][19]

Treffen Sie eine Warnung angezeigt wird, _besteht ein Problem mit dem Sicherheitszertifikat dieser Website_ (Internet Explorer) oder _die Verbindung ist nicht privat_ (Chrom), wie in der folgenden Abbildung dargestellt. Klicken Sie auf **Weiter, um diese Website (nicht empfohlen)** (Internet Explorer) oder **Erweitert** und * *zur & #60;* DNS-Namen*> (unsicher) ** (Chrom) fort. Geben Sie dann das zuvor angegebene IPython Notebooks Zugriff auf Kennwort.

**InternetExplorer:**
![Arbeitsbereich erstellen][20]

**Chrome:**
![Arbeitsbereich erstellen][21]

Nach der Anmeldung IPython Notebook wird ein Verzeichnis *DataScienceSamples* im Browser angezeigt. Dieses Verzeichnis enthält Beispiel IPython Notebooks, die von Microsoft Benutzer Daten Wissenschaft Aufgaben verwendet werden. Diese Beispielnotizbücher IPython werden aus [**Github Repository**](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) mit den virtuellen Computern während der Einrichtung IPython Notebook Server ausgecheckt. Microsoft verwaltet und aktualisiert das Repository häufig. Benutzer können Github Repository kürzlich aktualisierte Beispiel IPython Notebooks zu besuchen.
![Arbeitsbereich erstellen][18]

## <a name="upload"></a>Schritt 5: Hochladen eines vorhandenen Notizbuchs IPython von einem lokalen Computer auf den Server IPython Notebook

IPython-Notebooks bieten eine einfache Möglichkeit für Benutzer zum Hochladen eines vorhandenen Notizbuchs IPython auf ihren lokalen Computern IPython Notebook-Server auf den virtuellen Computern. Nachdem Benutzer IPython Notebook in einem Webbrowser anmelden, klicken Sie in das **Verzeichnis** , das IPython Notebook hochgeladen wird. Wählen Sie eine IPython Notebook .ipynb Datei hochladen auf dem lokalen Computer im **Datei-Explorer**und ziehen es in das Verzeichnis IPython Notebook im Webbrowser an. Klicken Sie auf die Schaltfläche **Hochladen** Upload die Datei .ipynb auf dem Server IPython Notebook. Andere Benutzer können dann starten im Webbrowser aus.

![Arbeitsbereich erstellen][22]

![Arbeitsbereich erstellen][23]


##<a name="shutdown"></a>Herunterfahren und Freigeben virtueller Computer nicht

Azure Virtual Machines werden als **nur bezahlen, was Sie verwenden**Preis. Um sicherzustellen, dass Sie nicht beim virtuellen Computer nicht verwenden abgerechnet werden, muss es den Zustand **beendet (Deallocated)** nicht.

> [AZURE.NOTE] Wenn der virtuellen Computer innerhalb des virtuellen Computers Herunterfahren (mit Windows-Energieoptionen) VM beendet aber bleibt solange reserviert. Um sicherzustellen, dass Ihre nicht belastet, immer halten Sie virtueller Maschinen von [Azure-Verwaltungsportal an](http://manage.windowsazure.com/). Die VM über Powershell hält durch Aufrufen von **ShutdownRoleOperation** mit "PostShutdownAction" "StoppedDeallocated" entspricht.

Herunterfahren und den virtuellen Computer freigeben:

1. Melden Sie sich mit Ihrem Konto [Azure-Verwaltungsportal](http://manage.windowsazure.com/) .  

2. Wählen Sie die **virtuellen Computer** in der linken Navigationsleiste.

3. In der Liste der virtuellen Computer auf den Namen des virtuellen Computers Klicken zur **DASHBOARD** -Seite.

4. Klicken Sie am unteren Rand der Seite auf **Herunterfahren**.

![VM Herunterfahren][15]

Der virtuelle Computer wird freigegeben, aber nicht gelöscht. Jederzeit von Azure-Verwaltungsportal kann den virtuellen Computer neu starten.

## <a name="your-azure-vm-is-ready-to-use-whats-next"></a>Azure-VM ist einsatzbereit: Was?

Ihr virtueller Computer kann jetzt Daten Wissenschaft Übungen verwenden. Der virtuelle Computer kann auch zur Verwendung als IPython Notebook-Server für die Untersuchung und Verarbeitung von Daten und andere Aufgaben in Verbindung Azure Computer und Daten Wissenschaft Team.

Die nächsten Schritte des wissenschaftlichen Daten Team [Lernen Pfad](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) zugeordnet sind und Daten in HDInsight verschieben, verarbeiten und es zur Vorbereitung von Daten mit Azure Computer lernen probieren Schritte einschließen.


[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png
