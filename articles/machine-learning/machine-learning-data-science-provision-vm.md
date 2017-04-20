<properties 
    pageTitle="Microsoft Data Science virtuellen Computer bereitstellen | Microsoft Azure" 
    description="Konfigurieren und Erstellen einer Science-VM auf Azure Analytics und maschinelles lernen." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="bradsev" />


# <a name="provision-the-microsoft-data-science-virtual-machine"></a>Microsoft Data Science virtuellen Computer bereitstellen

Microsoft Data Science Virtual Machine ist ein Bild Azure Virtual Machine (VM) vorinstalliert und konfiguriert mit verschiedenen beliebten Tools für Datenanalysen und maschinelles lernen häufig verwendet werden. Die Tools enthalten sind:

- Microsoft R Server Developer Edition
- Python-Distribution anaconda
- Jupyter Notebook (R, Python-Kernel)
- Visual Studio Community Edition
- Power BI desktop
- SQL Server 2016 Developer Edition
- Werkzeugmaschinen lernen
    - [Rechnerische Netzwerk Toolkit (CNTK)](https://github.com/Microsoft/CNTK): eine Tiefe lernen Toolkit Software von Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): einen schnellen Computer Lernsystem wie online, hashing, Allreduce, Reduzierung der learning2search, aktiv unterstützen und interaktiven Lernens.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): ein Tool schnell und präzise stärkere Struktur Implementierung.
    - [Klappern](http://rattle.togaware.com/) (die R Analytical Tool zu lernen leicht): ein Tool, das erste Schritte mit Datenanalysen einfach mit GUI-basierte r lernen und Modellierung mit automatischen R Code Generation macht.
    - [Mxnet](https://github.com/dmlc/mxnet): eine Tiefe Learning Framework zur Effizienz und Flexibilität
- Bibliotheken und Python für Azure maschinelles lernen und andere Azure-Dienste verwenden
- Git einschließlich Git Bash Quellcode-Repositorys einschließlich GitHub, Visual Studio Team Services arbeiten
- Windows Ports verschiedene gängige Linux Befehlszeilenprogramme (einschließlich Awk, Sed, Perl, Grep, suchen, Wget, Curl usw.) über die Befehlszeile. 


Daten Wissenschaft ist auf eine Abfolge von Aufgaben durchlaufen: suchen, laden und vorab verarbeitete Daten erstellen und Modelle testen und Bereitstellen der Modelle für den Verbrauch in intelligenten Clientanwendungen. Datenwissenschaftler verwenden eine Vielzahl von Tools zum Ausführen dieser Aufgaben. Es ist zeitaufwendig, finden die entsprechenden Versionen der Software herunter und installieren Sie sie. Microsoft Data Science Virtual Machine kann erleichtern diese durch die Bereitstellung einer fertige-Bilds, die bereitgestellt werden auf Azure alle verschiedene gängige Tools vorinstalliert und konfiguriert. 

Microsoft Data Science Virtual Machine jump-starts Analytics Projekt. Sie können Aufgaben in verschiedenen Sprachen, einschließlich R, Python, SQL und C#. Visual Studio bietet eine IDE zum Entwickeln und Testen von Code, der einfach zu verwenden ist. Azure SDK enthalten die virtuellen Computer können Sie Ihre Anwendung mit verschiedenen Diensten auf Microsoft Cloud-Plattform erstellen. 

Es fallen keine Software für diese Science-VM-Image. Sie Zahlen nur für Azure Nutzungsgebühren die abhängig von der Größe des virtuellen Computers, den Sie bereitstellen. Weitere Gebühren berechnen im Detailbereich Preise auf [Daten Wissenschaft Virtual Machine](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) finden. 


## <a name="prerequisites"></a>Erforderliche Komponenten

Vor dem Erstellen einer Microsoft-Science-VM benötigen Sie Folgendes:

- **Ein Azure-Abonnement**: eine finden Sie unter [Abrufen von Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

*   **Ein Azure-Speicherkonto**: eine finden Sie [Azure Storage-Konto erstellen](storage-create-storage-account.md#create-a-storage-account). Alternativ kann das Speicherkonto erstellt werden als Teil die VM erstellen, wenn Sie kein vorhandenes Konto verwenden möchten.


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Microsoft Data Science virtuellen Computers erstellen

Hier werden die Schritte zum Erstellen einer Instanz von Microsoft Daten Wissenschaft Virtual Machine:

1.  Navigieren Sie zum virtuellen Computer auf [Azure-Portal](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).
2.   Wählen Sie im unteren Bereich zu einem Assistenten **Erstellen** . ![konfigurieren-Daten-Science-Vm](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3.   Der Assistent Microsoft Data Science Virtual Machine erstellt erfordert **Eingaben** für jeden der **fünf Schritte** auf der rechten Seite der Abbildung aufgelistet. Hier werden die Eingaben zum Konfigurieren der einzelnen Schritte erforderlich:
    
    1.   **Grundlagen**
        1.   **Name**: Name der Wissenschaft Datenserver erstellen.
        2.   **Benutzername**: Benutzername für Admin-Konto.
        3.   **Kennwort**: Admin-Passwort.
        4.   **Abonnement**: haben Sie mehr als ein Abonnement, wählen Sie eine auf dem Computers wird erstellt und in Rechnung gestellt werden.
        5.   **Ressourcengruppe**: Erstellen Sie eine neue oder eine vorhandene Gruppe verwenden.
        6.   **Ort**: Wählen Sie das Rechenzentrum am besten geeignet ist. Normalerweise ist das Rechenzentrum, das größten Teil der Daten oder an Ihrem Standort für den schnellsten Netzwerk am nächsten ist.
         
    2.   **Größe**: Wählen Sie eine Server-Typen, die die funktionale Anforderung und Kosten entspricht. Weitere Entscheidungen VM Größen erhalten "Alle anzeigen" auswählen.
    
    3.   **Einstellung**:
        1.   **Festplattentyp**: Premium wählen Sie bevorzugen ein Solid State Drive (SSD) sonst wählen Sie "Standard".
        2.   **Konto**: ein neues Azure-Speicher-Konto in Ihrem Abonnement erstellen oder ein vorhandenes verwenden gewählt gleichen *Speicherort* **Grundlagen** Schritt des Assistenten.
        3.   **Weitere Parameter**: normalerweise nur verwendet die Standardwerte. Sie können über Information Link Hilfe zu den spezifischen Feldern bewegen, möchten die Verwendung von nicht-Standardwerte.

    4.   **Zusammenfassung**: Stellen Sie sicher, dass alle eingegebenen Informationen richtig sind.
    5.   **Kaufen**: Klicken Sie auf **kaufen** , um die Bereitstellung zu starten. Die Transaktion wird ein Link bereitgestellt. Die VM Zusatzkosten über die Berechnung für die Servergröße keinen **Größe** Schritt gewählten. 


>[AZURE.NOTE] Die Bereitstellung sollte ungefähr 10 bis 20 Minuten. Der Status der Bereitstellung wird der Azure-Portal angezeigt.

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Zugreifen auf Microsoft Daten Wissenschaft Virtual Machine

Nachdem die VM erstellt wurde, können Sie Remotedesktop in mit Admin-Anmeldeinformationen, die Sie im vorherigen Abschnitt **Grundlagen** konfiguriert. 

Wenn die VM erstellt und bereitgestellt wird, können Sie verwenden die Tools installiert und konfiguriert. Gibt Start Menü Kacheln und desktop-Symbole für viele der Tools. 

## <a name="how-to-create-a-strong-password-on-the-jupyter-notebook-server"></a>Erstellen Sie ein sicheres Kennwort für Jupyter Notebook-server 

Erstellen Sie Ihr eigenes sichere Kennwort für den Jupyter Notebook-Server auf dem Computer installierten führen Sie folgenden Befehl von einer Befehlszeile aus auf Daten Wissenschaft Virtual Machine.

    c:\anaconda\python.exe -c "import IPython;print IPython.lib.passwd()"

Wählen Sie ein sicheres Kennwort ein.

Sie sehen den Kennworthash im Format "Sha1:xxxxxx" in der Ausgabe. Diese Kennworthash kopieren und ersetzen den bestehenden Hash, die in Ihrem Notizbuch Konfigurationsdatei am: **C:\ProgramData\jupyter\jupyter_notebook_config.py** mit einem Parameter namens ***c.NotebookApp.password***.

Ersetzen Sie nur den Teil (Xxxxxx) vorhandenen Hashwert, der innerhalb der Anführungszeichen. Anführungszeichen und die ***sha1:*** Präfix Parameterwert beide beibehalten werden müssen.

Schließlich müssen Sie zu der Jupyter Serverneustart als Windows geplanter Task namens **Start_IPython_Notebook**auf dem virtuellen Computer ausgeführt wird. Wenn Ihr neue Kennwort nicht akzeptiert wird, nach dem Neustart dazu, versuchen Sie Tötung aller ausgeführten Python-Prozesse im TaskManager, und starten Sie den geplanten Task. Alternativ starten Sie den virtuellen Computer.

## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Tools für Microsoft Data Science Virtual Machine installiert

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R Server Developer Edition
Wenn Sie R für die Analyse verwenden möchten, hat die VM Microsoft R Server Developer Edition installiert. Microsoft R Server ist eine allgemein einsetzbare Unternehmensklasse Analytics Plattform auf R unterstützt wird, skalierbar und sicher. R Server unterstützt unterstützt eine Reihe von großen Statistik und Vorhersagemodellen maschinelles lernen Funktionen, sämtliche Analytics-Untersuchung, Analyse, Visualisierung und Modellierung. Verwenden und Erweitern von open-Source-R, wird Microsoft R Server kompatibel mit R Skripts, Funktionen und CRAN Paketen Datenanalyse auf Unternehmensebene. Es behandelt auch das Speicherlimit der Open Source parallel und aufgeteilte Verarbeitung hinzufügen. Dadurch ausgeführt Analytics Daten viel größer, was in den Arbeitsspeicher passt.  Visual Studio Community Edition enthalten die VM enthält die R-Tools für Visual Studio-Erweiterung, die eine vollständige IDE stellt für das Arbeiten mit. Sie können auch herunterladen und anderen IDEs sowie wie [RStudio](http://www.rstudio.com)verwenden. 

### <a name="python"></a>Python
Für die Entwicklung mit Python wurde Anaconda Python-Distribution 2.7 und 3.5 installiert. Diese Verteilung enthält grundlegenden Python mit etwa 300 beliebtesten Mathematik, Engineering und Data Analytics-Pakete. Python-Tools können für Visual Studio (PTVS), die in Visual Studio 2015 Community Edition oder eines IDEs gebündelt mit Anaconda im Leerlauf oder Spyder installiert ist. Starten Sie eines dieser anhand der Suchleiste (**Win** + **S** -Taste).

>[AZURE.NOTE] Python-Tools für Visual Studio Anaconda Python 2.7 und 3.5 verweisen, müssen Sie benutzerdefinierte Umgebungen für jede Version zu erstellen. Navigieren Sie zum Festlegen dieser Umgebung Pfade in Visual Studio 2015 Community Edition **Tools** -> **Python Tools** -> **Python-Umgebungen** **+ benutzerdefinierte**klicken. 

Anaconda Python 2.7 ist unter C:\Anaconda und Anaconda Python 3.5 unter c:\Anaconda\envs\py35 installiert. [PTVS](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) Dokumentation weitere Informationen. 

### <a name="jupyter-notebook"></a>Jupyter Notebook
Anaconda Verteilung verfügt außerdem über ein Jupyter Notebook Umgebung Code und Analyse. Ein Jupyter Notebook-Server wurde bereits mit Python 2, Python 3 und R-Kernels konfiguriert. Ein desktop-Symbol mit dem Namen "Jupyter Notebook Browser Zugriff auf Notebook-Server starten. Sind Sie auf dem virtuellen Computer über remote Desktop, finden Sie auch [Https://localhost:9999 /](https://localhost:9999/) Jupyter Notebook der VM angemeldet berechtigt.
 
>[AZURE.NOTE] Fahren Sie Wenn zertifikatwarnungen erhalten. 

Wir haben Beispielnotizbücher in Python und r verpackt. Arbeiten mit Microsoft R Server, SQL Server 2016 R Services (Analytics Datenbank), Python und anderen Azure Techniken nach Anmeldung bei Jupyter anzeigen Jupyter notebooks Sie sehen den Link zu den Beispielen auf der Homepage Notizbuch nach der Authentifizierung Jupyter Notebook mit dem Kennwort, das Sie in einem früheren Schritt erstellt haben. 

### <a name="visual-studio-2015-community-edition"></a>Visual Studio 2015 Community edition
Visual Studio Community Edition auf dem virtuellen Computer installiert. Es ist eine kostenlose Version des beliebten IDE von Microsoft zu Evaluierungszwecken und kleine Teams verwenden können. Die Lizenzierungsinformationen Auschecken [hier](https://www.visualstudio.com/support/legal/mt171547).  Öffnen Sie Visual Studio durch Doppelklicken auf das Symbol desktop oder im **Startmenü** . Sie können auch mit **gewinnen**suchen + **S** und in "Visual Studio". Wenn es Sie Projekte in Sprachen wie C#, Python, R, node.js erstellen können. Plug-Ins werden installiert, die mit Azure Azure Data Catalog Azure HDInsight (Hadoop, Funken) und Azure Data Lake zu vereinfachen. 

>[AZURE.NOTE] Sie erhalten eine Meldung, dass der Evaluierungszeitraum abgelaufen ist. Geben Sie Ihre Anmeldeinformationen für Microsoft, oder erstellen Sie ein neues kostenloses Konto Zugriff auf Visual Studio Community Edition. 

### <a name="sql-server-2016-developer-edition"></a>SQL Server 2016 Developer edition
Entwicklerversion SQL Server 2016 mit R Analysen in der Datenbank wird auf dem virtuellen Computer bereitgestellt. R-Services bieten eine Plattform für die Entwicklung und Bereitstellung von intelligenten Clientanwendungen. Können Sie die umfassende und leistungsfähige R Sprache und viele Pakete aus der Gemeinschaft Modelle erstellen und Vorhersagen für die SQL Server-Daten generieren. Sie können Analytics nahe Daten behalten, da R Services (In der Datenbank) Sprache R in SQL Server integrieren. Dadurch Kosten und Sicherheitsrisiken Datentransfer.

>[AZURE.NOTE] SQL Server 2016 Developer Edition kann nur für Entwicklungs- und Testzwecke verwendet werden. Sie benötigen eine Lizenz in der Produktion ausgeführt. 

Starten **SQL Server Management Studio**können Sie den SQL Server zugreifen. Der Name des virtuellen Computers wird als Servername ausgefüllt. Verwenden Sie Windows-Authentifizierung als Administrator unter Windows. Einmal in SQL Server Management Studio können Sie andere Benutzer erstellen, Datenbanken, Daten und SQL-Abfragen ausführen. 

Um Datenbank Analysen mit Microsoft R zu aktivieren, führen Sie den folgenden Befehl ein Mal nach Anmeldung als Serveradministrator Aktion in SQL Server Management Studio. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 
        
        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure 
Mehrere Azure Tools werden auf dem virtuellen Computer installiert:

- Es ist eine desktop-Verknüpfung auf der Azure-SDK-Dokumentation. 
- **AzCopy**: zum Verschieben von Daten in Microsoft Azure Storage-Konto verwendet. Verwendung, geben Sie **Azcopy** in der Befehlszeile an die Verwendung anzeigen. 
- **Microsoft Azure Storage Explorer**: verwendet, um durch die Objekte navigieren, die in der Azure-Speicherkonto und übertragen Daten von Azure-Speicher gespeichert. Sie können **Speicher-Explorer** durchsuchen oder suchen im Windows Startmenü auf dieses Tool zugreifen. 
- **Adlcopy**: Azure Data Lake Daten verschoben. Geben Sie **Adlcopy** in einer Befehlszeile, Verwendung finden. 
- **Dtui**: Daten zu und von Azure DocumentDB NoSQL-Datenbank in der Cloud verschoben. Geben Sie **Dtui** in der Befehlszeile. 
- **Microsoft Data Management Gateway**: Verschieben von Daten zwischen lokalen Datenquellen und ermöglicht. Es wird in Tools wie Azure Data Factory verwendet. 
- **Microsoft Azure Powershell**: ein Tool zur Verwaltung Ihrer Azure-Ressourcen in der Powershell Skriptsprache auch auf die VM installiert ist. 

###<a name="power-bi"></a>Power BI

Erstellen von Dashboards und hervorragende Visualisierung hierzu wurde **Power BI Desktop** installiert. Mit diesem Tool können Daten aus verschiedenen Quellen, Dashboards und Berichte erstellen und in der Cloud veröffentlichen. Informationen finden Sie unter [Power BI](http://powerbi.microsoft.com) -Website. Power BI Desktop finden Sie im Menü Start. 

>[AZURE.NOTE] Benötigen Sie ein Office 365-Konto auf Power BI. 

## <a name="additional-microsoft-development-tools"></a>Zusätzliche Microsoft-Entwicklungstools
[**Microsoft-Webplattform-Installer**](https://www.microsoft.com/web/downloads/platform.aspx) kann verwendet werden, erkennen und andere Entwicklungstools von Microsoft herunterladen. Außerdem ist eine Verknüpfung mit dem Tool auf dem Desktop Microsoft Data Science Virtual Machine.  

## <a name="important-directories-on-the-vm"></a>Wichtige Verzeichnisse des virtuellen Computers

| Artikel                          | Verzeichnis |
| ------------------------------| ---------------- |
|Jupyter Notebook-Serverkonfigurationen | C:\ProgramData\jupyter |
|Basisverzeichnis für Jupyter Notebook-Beispiele| c:\dsvm\notebooks|
|Weitere Beispiele | c:\dsvm\samples|
| Anaconda (Standard: Python 2.7) | c:\Anaconda |
| Anaconda Python 3.5 Umgebung | c:\Anaconda\envs\py35|
|R Server eigenständige Instanzverzeichnis (R Standard Instanz) | C:\Program Files\Microsoft SQL Server\130\R_SERVER |
| R-Datenbank Instanz Serververzeichnis | C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES |
| Sonstige tools | c:\dsvm\tools|

>[AZURE.NOTE] Instanzen von Microsoft Daten Wissenschaft Virtual Machine erstellt vor 1.5.0 (vor dem 3. September 2016) verwendet eine geringfügig Verzeichnisstruktur in der obigen Tabelle angegeben. 

## <a name="next-steps"></a>Nächste Schritte
Hier sind einige Schritte der Learning und Untersuchung. 

* Untersuchen Sie die verschiedenen Wissenschaft Tools auf Daten VM indem im Startmenü klicken und im Menü aufgelisteten Tools.
* Navigieren Sie zu **C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** Beispiele mit der Bibliothek RevoScaleR r, die Datenanalyse auf Unternehmensebene unterstützt.  
* Artikel: [10 Dinge auf Daten virtuellen lassen](http://aka.ms/dsvmtenthings)
* Informationen Sie zum Ende analytischen Lösungen systematisch mit dem [Team wissenschaftliche Daten](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* Besuchen Sie [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com) Computer lernen und Data Analytics Beispiele, mit denen Cortana Intelligence Suite. Wir haben ein Symbol im **Startmenü** und auf dem Desktop des virtuellen Computers zu diesem Katalog bereitgestellt.

