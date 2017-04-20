<properties
    pageTitle="Bereitstellung von Linux-Daten Wissenschaft Virtual Machine | Microsoft Azure"
    description="Konfigurieren und Erstellen einer Daten Wissenschaft virtuellen Linux-Maschine auf Azure Analytics und maschinelles lernen."
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
    ms.date="09/12/2016"
    ms.author="bradsev" />

# <a name="provision-the-linux-data-science-virtual-machine"></a>Bereitstellung von Linux-Daten Wissenschaft Virtual Machine

Daten Wissenschaft virtuellen Linux-Maschine ist ein Azure virtueller Computer, der mit einer Sammlung von bereits installierten Tools. Diese Tools werden häufig verwendet, um Datenanalyse und maschinelles lernen. Die wichtigsten Softwarekomponenten enthalten sind:

- Microsoft R Server Developer Edition
- Anaconda Python-Distribution (Version 2.7 und 3.5) einschließlich bekanntes Analysebibliotheken
- JupyterHub - eine Mehrbenutzer Jupyter Notebook Server unterstützen R, Python, Julia kernels
- Azure-Speicher-Explorer
- Azure Befehlszeilenschnittstelle (CLI) für die Verwaltung von Azure-Ressourcen
- PostgresSQL Datenbank
- Werkzeugmaschinen lernen
    - [Rechnerische Netzwerk Toolkit (CNTK)](https://github.com/Microsoft/CNTK): eine Tiefe lernen Toolkit Software von Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): einen schnellen Computer Lernsystem wie online, hashing, Allreduce, Reduzierung der learning2search, aktiv unterstützen und interaktiven Lernens.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): ein Tool schnell und präzise stärkere Struktur Implementierung.
    - [Klappern](http://rattle.togaware.com/) (die R Analytical Tool zu lernen leicht): ein Tool, das erste Schritte mit Datenanalysen einfach mit GUI-basierte r lernen und Modellierung mit automatischen R Code Generation macht.
- Azure SDK in Java, Python, node.js, Ruby, PHP
- Bibliotheken und Python für Azure maschinelles lernen und andere Azure-Dienste verwenden
- Entwicklungstools und Editoren (Eclipse, Emacs, Gedit, vi)

Daten Wissenschaft umfasst auf eine Abfolge von Aufgaben durchlaufen:

1. Suchen und laden Daten vorverarbeitet
2. Erstellen und Testen von Modellen
3. Bereitstellen von Modellen für den Verbrauch in intelligenten Clientanwendungen

Datenwissenschaftler verwenden verschiedene Tools für diese Aufgaben. Es kann sehr lange dauern die entsprechenden Versionen der Software, und kompilieren Sie dann herunterladen und diese Versionen installieren.

Daten Wissenschaft virtuellen Linux-Maschine kann diese Belastung deutlich vereinfachen. Verwenden sie Analytics Projekt sofort. Sie können Aufgaben in verschiedenen Sprachen, einschließlich R, Python, SQL, Java und C++. Eclipse ermöglicht eine IDE zum Entwickeln und Testen von Code, der einfach zu verwenden ist. Azure SDK enthalten in der VM können Sie Ihrer Anwendung erstellen, indem Sie verschiedene Dienste unter Linux für die Microsoft-Cloud. Darüber hinaus haben Sie Zugriff auf andere Sprachen wie Ruby, Perl, PHP, node.js bereits installiert sind.

Es fallen keine Software für diese Science-VM-Image. Sie bezahlen nur die Azure Hardware Gebühren, die ermittelt anhand der Größe des virtuellen Computers, der mit der VM-Image bereitstellen. Weitere Informationen zu Gebühren berechnen finden Sie auf der [Eintragsseite auf Azure VM ](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).


## <a name="prerequisites"></a>Erforderliche Komponenten

Vor der Erstellung einer Daten Wissenschaft virtuellen Linux-Maschine benötigen Sie Folgendes:

- **Ein Azure-Abonnement**: eine finden Sie unter [Abrufen von Azure-Testversion](https://azure.microsoft.com/free/).
- **Ein Azure-Speicherkonto**: eine finden Sie [Azure Storage-Konto erstellen](storage-create-storage-account.md#create-a-storage-account). Alternativ kann das Speicherkonto als Teil des Prozesses zum Erstellen der VM erstellt, wenn Sie kein vorhandenes Konto verwenden möchten.


## <a name="create-your-linux-data-science-virtual-machine"></a>Linux-Daten Wissenschaft virtuellen Computers erstellen

Hier werden die Schritte zum Erstellen einer Instanz der Daten Wissenschaft virtuellen Linux-Maschine:

1.  Navigieren Sie zum virtuellen Computer auf den [Azure-Portal](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm).
2.   Klicken Sie auf **Erstellen** (unten), um den Assistenten aufzurufen. ![konfigurieren-Daten-Science-Vm](./media/machine-learning-data-science-linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3.   Die folgenden Abschnitte enthalten die Eingaben für die Schritte des Assistenten (auf der rechten Seite der Abbildung oben aufgelistet) Microsoft Daten Wissenschaft virtuellen Computer erstellt. Hier werden die Eingaben zum Konfigurieren der einzelnen Schritte erforderlich:

    ein. **Grundlagen**:

  - **Name**: Name der Wissenschaft Datenserver erstellen.
  - **Benutzername**: erstes Konto anmelden ID
  - **Kennwort**: erste Kennwort (können öffentlichen SSH-Schlüssel statt Kennwort).
  - **Abonnement**: haben Sie mehr als ein Abonnement, wählen Sie eine auf dem Computers wird erstellt und in Rechnung gestellt werden. Sie müssen Ressourcen über Berechtigungen für dieses Abonnement.
  - **Ressourcengruppe**: Erstellen Sie eine neue oder eine vorhandene Gruppe verwenden.
  - **Ort**: Wählen Sie das Rechenzentrum am besten geeignet ist. Normalerweise ist das Rechenzentrum, die meisten Ihrer Daten an Ihrem Standort für den schnellsten Netzwerk am nächsten ist.

    b. **Größe**:

  - Wählen Sie eine Server-Typen, die die funktionale Anforderung und Kosten entspricht. Wählen Sie **Alle anzeigen** weiterer Auswahlmöglichkeiten VM-Größen.

    c. **Einstellung**:

  - **Festplattentyp**: **Premium** bevorzugen eine solid State Drive (SSD) wählen. Andernfalls wählen Sie **Standard**aus.
  - **Konto**: Sie können ein neues Azure-Speicher-Konto in Ihrem Abonnement erstellen oder eine vorhandene an derselben Stelle wurde **Grundlagen** Schritt des Assistenten.
  - **Weitere Parameter**: In den meisten Fällen verwenden Sie nur die Standardwerte. Prüfen Sie nicht standardmäßige Werte Information Link Hilfe auf bestimmte Felder den Mauszeiger.

    d. **Zusammenfassung**:

  - Stellen Sie sicher, dass alle eingegebenen Informationen richtig ist.

    e. **Kaufen**:

  - Um die Bereitstellung zu starten, klicken Sie auf **kaufen**. Die Transaktion wird ein Link bereitgestellt. Die VM Zusatzkosten über die Berechnung für die Servergröße keinen **Größe** Schritt gewählten.

Die Bereitstellung sollte ungefähr 10 bis 20 Minuten. Der Status der Bereitstellung wird der Azure-Portal angezeigt.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Zugreifen auf Daten Wissenschaft virtuellen Linux-Maschine

Nachdem die VM erstellt wurde, können Sie damit anmelden über SSH. Verwenden Sie die Kontoanmeldeinformationen, die Sie im Abschnitt **Grundlagen** von Schritt 3 für die Shell-Textschnittstelle erstellt. Unter Windows können Sie eine SSH-Client-Tools wie [kitten](http://www.putty.org). Wunsch grafischen Desktop (X Windows System) können X11 auf kitten weiterleiten verwenden oder X2Go-Client installieren.

>[AZURE.NOTE] X2Go Client durchgeführt erheblich besser als X11 testen weiterleiten. Wir empfehlen den X2Go Client für Desktop-Benutzeroberfläche.


## <a name="installing-and-configuring-x2go-client"></a>Installieren und Konfigurieren von X2Go-client

Linux VM ist bereits X2Go Server bereitgestellt und Clientverbindungen zu akzeptieren. Verbindung zu Linux VM grafisch Desktop gehen Sie auf dem Client:

1. Downloaden und Installieren des Clients X2Go Ausführen von [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Führen Sie die X2Go, und wählen Sie **Neue Sitzung**. Er öffnet eine Konfiguration mit mehreren Registerkarten. Geben Sie die folgenden Konfigurationsparameter:
    * **Sitzungsregisterkarte**:
        - **Host**: Hostname oder IP-Adresse die Linux Daten Wissenschaft VM.
        - **Benutzername**: Benutzername auf Linux VM.
        - **SSH-Port**: 22, den Standardwert belassen.
        - **Sitzungstyp**: Ändern Sie den Wert XFCE. Linux VM unterstützt derzeit nur XFCE Desktop.
    * **Registerkarte Medien**: Deaktivieren sound Support und drucken, wenn Sie sie verwenden müssen.
    * **Freigegebene Ordner**: Verzeichnisse vom Client-Computer auf Linux VM bereitgestellt werden soll, fügen Sie die Client Computer Verzeichnissen VM auf dieser Registerkarte zur Verfügung.

Nach der für die VM Anmeldung mit SSH-Client oder XFCE grafisch Desktop über den X2Go Client, können Sie beginnen mit den installiert und auf dem virtuellen Computer konfiguriert sind. Auf XFCE sehen Sie Applikationen Tastenkombinationen und desktop-Symbole für viele der Tools.


## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Supporttools auf Daten Wissenschaft virtuellen Linux-Maschine

### <a name="microsoft-r-open"></a>Microsoft R öffnen
R ist eine der beliebtesten Sprachen für die Analyse und maschinelles lernen. Wenn Sie R für die Analyse verwenden möchten, hat die VM Microsoft R öffnen (MRO) mit Math Kernel Library (MKL). Die MKL optimiert Rechenvorgänge häufig für analytische Algorithmen. MRO ist 100 % kompatibel mit CRAN R und eines R Bibliotheken im CRAN veröffentlicht auf der MRO installiert werden kann. Sie können Programme R eines Standardeditoren wie vi, Emacs oder Gedit bearbeiten. Sie können auch herunterladen und anderen IDEs wie [RStudio](http://www.rstudio.com)verwenden. Der Einfachheit halber wird ein einfaches Skript (installRStudio.sh) im Verzeichnis **/dsvm/tools** bereitgestellt, die RStudio installiert. Wenn Sie Emacs-Editor verwenden, wurde Beachten Sie, dass die Emacs ESS (Emacs spricht Statistiken) packen vereinfacht R Dateien im Editor Emacs vorinstalliert.

Starten Sie R Geben Sie einfach **R** in der Shell. Dadurch gelangen Sie zu einer interaktiven Umgebung. R Programm entwickeln, in der Regel verwenden Sie einen Editor wie Emacs vi oder Gedit, und führen Sie die Skripts in R. Installation RStudio haben eine vollständige grafische IDE-Umgebung zu R Programm.

Es gibt auch eine R-Skript für die [oberen 20 R-Pakete](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) installiert. Dieses Skript kann ausgeführt werden, nachdem Sie in der R interaktive Schnittstelle sind die **R** in der Shell eingeben (wie erwähnt) eingegeben werden können.  

### <a name="python"></a>Python
Für die Entwicklung mit Python wurde Anaconda Python-Distribution 2.7 und 3.5 installiert. Diese Verteilung enthält grundlegenden Python mit etwa 300 beliebtesten Mathematik, Engineering und Data Analytics-Pakete. Sie können Standard-Text-Editoren. Darüber hinaus können Sie Spyder, Python-IDE, die mit Anaconda Python-Distributionen. Spyder benötigt eine grafisch Desktop- oder X11 weiterleiten. Grafisch Desktop wird eine Verknüpfung zur Spyder bereitgestellt.

Da wir Python 2.7 und 3.5 haben, müssen Sie speziell die gewünschte Python Version aktivieren in der aktuellen Sitzung arbeiten möchten. Die Aktivierung wird der PATH-Variablen auf die gewünschte Version von Python.

Aktivieren Sie Python 2.7, führen Sie Folgendes aus der Shell:

    source /anaconda/bin/activate root

Python 2.7 wird unter */anaconda/bin*installiert.

Um Python 3.5 zu aktivieren, führen Sie Folgendes aus der Shell:

    source /anaconda/bin/activate py35


Python 3.5 wird unter */anaconda/envs/py35/bin*installiert.

Um eine interaktive Sitzung Python aufzurufen, geben Sie **Python** der Shell. Wenn Sie auf einer Benutzeroberfläche oder X11 festlegen weiterleiten, geben Sie **Spyder** Python IDE starten.

### <a name="jupyter-notebook"></a>Jupyter notebook

Anaconda Verteilung verfügt außerdem über ein Jupyter Notebook Umgebung Code und Analyse. Jupyter Notebook erfolgt über JupyterHub. Sie Anmelden mit Ihrem lokalen Linux-Benutzernamen und Kennwort.

Jupyter Notebook Server wurde bereits mit Python 2, Python 3 und R-Kernels konfiguriert. Es ist ein Desktop-Symbol mit dem Namen "Jupyter-Notebook" Starten Sie den Browser, um die Notebook-Server zugreifen. Sind Sie auf dem virtuellen Computer über SSH oder X2Go Client, besuchen Sie auch [Https://localhost:8000 /](https://localhost:8000/) Jupyter Notebook berechtigt.

>[AZURE.NOTE] Fahren Sie Wenn zertifikatwarnungen erhalten.

Sie können von jedem Host Jupyter Notebook-Server zugreifen. Geben Sie einfach *https://\<VM DNS-Name oder IP-Adresse\>: 8000 /*

>[AZURE.NOTE] Port 8000 ist die Firewall standardmäßig geöffnet, wenn die VM bereitgestellt wird.

Wir haben Beispielnotizbücher – ein in Python und in r verpackt. Sie sehen den Link zu den Beispielen auf der Homepage Notizbuch nach Jupyter Notebook mit Ihren lokalen Linux-Benutzernamen und Kennwort authentifizieren. Wählen **neu**und anschließend auf die entsprechende Sprache Kernel können Sie ein neues Notizbuch erstellen. Wenn die Schaltfläche **neu** nicht angezeigt wird, klicken Sie **Jupyter** oben links, um zur Homepage des Servers Notizbuch zu wechseln.


### <a name="ides-and-editors"></a>IDEs und Editoren

Sie stehen mehrere Code-Editoren. Dazu gehören vi-VIM, Emacs, gEdit und Eclipse. gEdit Eclipse grafisch Editoren und Sie grafisch Desktop anmelden verwenden. Diese Editoren verfügen über Desktop und Anwendung Tastenkombinationen zum Starten.

**VIM** und **Emacs** sind textbasierten Editoren. Auf Emacs haben wir ein Add-On-Paket namens Emacs spricht Statistiken (ESS), die mit R Emacs-Editor erleichtert installiert. Weitere Informationen finden unter [ESS](http://ess.r-project.org/).

**Eclipse** ist eine offene Quelle erweiterbaren IDE, das mehrere Sprachen unterstützt. Java-Entwickler Edition ist die Instanz auf dem virtuellen Computer installiert. Gibt Plugins für verschiedene gängige Sprachen erweitern Eclipse-Umgebung installiert werden können. Wir haben auch ein Plugin in Eclipse namens **Azure Toolkit für Eclipse**installiert. Sie können Sie erstellen, entwickeln, testen und Bereitstellen von Azure Applications mithilfe des Eclipse Development Environment, die Sprachen wie Java unterstützt. Es gibt auch ein **Azure SDK für Java** , die verschiedene Azure-Services innerhalb einer Java-Umgebung zuzugreifen. Weitere Informationen zu Azure Toolkit für Eclipse zur [Azure-Toolkit für Eclipse](../azure-toolkit-for-eclipse.md)finden.

**LaTex** ist über Texlive Paket mit Emacs [Auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) Zusatzpaket, vereinfacht die Erstellung der LaTex Dokumente in Emacs installiert.  

### <a name="databases"></a>Datenbanken

#### <a name="postgres"></a>Postgres
Open Source-Datenbank **Postgres** ist auf dem virtuellen Computer ausgeführten Dienste und Initdb bereits abgeschlossen. Weiterhin müssen Datenbanken und Benutzer erstellen. Weitere Informationen finden Sie in der [Dokumentation zu Postgres](https://www.postgresql.org/docs/).  

####  <a name="graphical-sql-client"></a>Grafisch SQL client
Andere Datenbanken (z. B. Microsoft SQL Server, Postgres und MySQL) und SQL-Abfragen ausführen wurde **Eichhörnchen SQL**grafisch SQL-Client bereitgestellt. Sie können dieser aus einer grafisch desktop-Sitzung (z. B. mit dem Client X2Go) ausführen. Um Eichhörnchen SQL aufzurufen, über das Symbol auf dem Desktop starten oder führen Sie folgenden Befehl in die Shell.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Vor der ersten Verwendung, Treibern und Datenbank-Aliase einrichten. Die JDBC-Treiber befinden sich unter:

*/usr/share/Java/jdbcdrivers*

Weitere Informationen finden Sie unter [Eichhörnchen SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Befehlszeilenprogramme für den Zugriff auf Microsoft SQL Server

ODBC-Treiber-Paket für SQL Server enthält außerdem zwei Befehlszeilentools:

**Bcp**: Bulk-Dienstprogramm Bcp kopiert Daten zwischen einer Instanz von Microsoft SQL Server und einer Datendatei in einem vom Benutzer angegebenen Format. Das Dienstprogramm Bcp kann viele neue Zeilen in SQL Server-Tabellen importieren oder Exportieren von Daten aus Tabellen in Datendateien verwendet werden. Zum Importieren von Daten in eine Tabelle müssen Sie mithilfe einer Formatdatei für diese Tabelle erstellt oder Verstehen der Struktur der Tabelle und die Datentypen, die für Spalten zulässig sind.

Weitere Informationen finden Sie unter [Verbinden mit Bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**SQLCMD**: Sie können Transact-SQL-Anweisung mit dem Dienstprogramm Sqlcmd sowie Systemprozeduren, und Skriptdateien in der Befehlszeile. Dieses Dienstprogramm verwendet ODBC Transact-SQL-Batches ausgeführt.

Weitere Informationen finden Sie unter [Verbinden mit Sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

>[AZURE.NOTE] Es gibt einige Unterschiede in diesem Dienstprogramm Linux- und Windows-Plattformen. Finden Sie weitere Informationen.


#### <a name="database-access-libraries"></a>Datenbank Zugriff auf Bibliotheken

Gibt Bibliotheken und Python in Access-Datenbanken.

- R kann die **RODBC** oder **Dplyr** -Paket Abfragen oder SQL-Anweisungen auf dem Datenbankserver ausführen.
- In Python Zugriff **Pyodbc** Bibliothek Datenbank mit ODBC als die darunter liegende Ebene.  

Auf **Postgres**:

- R: Verwenden Sie das Paket **RPostgreSQL**.
- Python: Verwenden Sie **psycopg2** -Bibliothek.


### <a name="azure-tools"></a>Azure tools
Die folgenden Azure Tools werden auf dem virtuellen Computer installiert:

- **Azure Befehlszeilenschnittstelle**: der Azure-CLI ermöglicht das Erstellen und Verwalten von Azure-Ressourcen durch Shell-Befehle. Geben Sie zum Aufrufen von Azure Tools nur **Azure Help**. Weitere Informationen finden Sie in der [Azure-CLI-Dokumentationsseite](../virtual-machines-command-line-tools.md).
- **Microsoft Azure Storage Explorer**: Microsoft Azure Storage Explorer ist ein Grafiktool, die durch die Objekte navigieren, die in Ihrem Konto Azure-Speicher gespeichert zu uploaden und Downloaden von Daten zu und von Azure-Blobs verwendet wird. Sie können die Desktop-Verknüpfung Speicher-Explorer zugreifen. Sie können es aus einer Shell-Aufforderung Eingabe **StorageExplorer**aufrufen. Sie müssen ein X2Go-Client angemeldet oder Weiterleitung Satz von X11.
- **Azure-Bibliotheken**: im folgenden sind einige der vorinstallierten Bibliotheken.

 - **Python**: der Azure-bezogene Bibliotheken installiert werden Python **Azure**, **Azureml**, **Pydocumentdb**und **Pyodbc**sind. Mit den ersten drei Bibliotheken möglich Azure-Speicherdienste, Azure maschinelles lernen und Azure DocumentDB (eine NoSQL-Datenbank auf Azure). Vierter Bibliothek Pyodbc (zusammen mit der Microsoft ODBC-Treiber für SQL Server), ermöglicht den Zugriff auf SQL Server, Azure SQL-Datenbank und Azure SQL Data Warehouse aus Python mit ODBC-Schnittstelle. Geben Sie **Pip-Liste** aufgeführten Bibliotheken. Achten Sie darauf, dass dieser Befehl Python 2.7 und 3.5 Umgebung.
 - **R**: der Azure-bezogene Bibliotheken r die installierten sind **AzureML** und **RODBC**.
 - **Java**: Liste der Azure-Java-Bibliotheken finden Sie in dem Verzeichnis **/dsvm/sdk/AzureSDKJava** auf dem virtuellen Computer. Die wichtigsten Bibliotheken sind Azure Speicher- und Management-APIs, DocumentDB und JDBC-Treiber für SQL Server.  

Vorinstallierte Firefox-Browser können Sie [Azure-Portal](https://portal.azure.com) zugreifen. Azure-Portal können Sie erstellen, verwalten und Überwachen von Azure Ressourcen.

### <a name="azure-machine-learning"></a>Azure maschinelles lernen

Azure Machine Learning ist eine vollständig verwaltete Cloud-Dienst, mit dem Sie zum Erstellen, bereitstellen und Vorhersageanalytik Lösungen. Erstellen der Experimente und Modelle von Azure Machine Learning Studio. Sie können über einen Webbrowser auf Daten Wissenschaft virtuellen Computer auf [Microsoft Azure Machine Learning](https://studio.azureml.net)zugegriffen werden.

Nach dem Einloggen Azure Machine Learning Studio haben Sie Zugriff auf eine Leinwand experimentieren, können Sie einen logischen Fluss für den Computer lernen Algorithmen erstellen. Sie haben Zugriff auf ein Jupyter Notebook auf Azure Computer gehostet und nahtlos mit den Experimenten Machine Learning Studio. Durchsetzen des Computers lernen Modelle durch Einbinden in eine Webdienstschnittstelle erstellt haben. Dadurch können Clients in beliebigen Sprachen geschriebene Vorhersagen lernen Modelle Computer aufrufen. Weitere Informationen finden Sie in der [Dokumentation für maschinelles lernen](https://azure.microsoft.com/documentation/services/machine-learning/).

Sie können auch Modelle in R oder Python auf dem virtuellen Computer erstellen und in Produktion auf Azure Computer bereitstellen. Wir haben in R (**AzureML**) und Python (**Azureml**) zum Aktivieren dieser Funktionalität Bibliotheken installiert.

Informationen zum Bereitstellen von Modellen und Python in Azure Computer finden Sie unter [zehn Punkte auf Daten virtuellen lassen](machine-learning-data-science-vm-do-ten-things.md) (insbesondere den Abschnitt "Modelle mit R oder Python und durchsetzen sie mit Azure Machine Learning").

>[AZURE.NOTE] Diese Informationen wurden für die Windows-Version der VM wissenschaftliche Daten geschrieben. Aber die Informationen zum Bereitstellen von Modellen mit Azure Machine Learning gibt Linux VM.

### <a name="machine-learning-tools"></a>Werkzeugmaschinen lernen

Die VM enthält einige Computer lernen Tools und Algorithmen, die bereits kompiliert wurden und bereits lokal installiert. Dazu gehören:

* **CNTK** (Computational Netzwerk Toolkit von Microsoft Research): eine Tiefe Toolkit lernen.
* **Vowpal Wabbit**: Online-lernalgorithmus.
* **Xgboost**: ein Tool Struktur optimiert, stärkere Algorithmen.
* **Python**: Anaconda Python Lieferumfang Computer lernalgorithmen wie Scikit lernen. Sie können andere Bibliotheken mit den `pip install` Befehl.
* **R**: r steht eine umfangreiche Bibliothek von Learning Funktionen Die Bibliotheken vorinstalliert sind lm, Glm, RandomForest, Rpart. Andere Bibliotheken können mit installiert:

        install.packages(<lib name>)

Hier ist einige zusätzliche Informationen über die ersten drei computerlernen Tools in der Liste.

#### <a name="cntk"></a>CNTK
Dies ist eine offene Quelle Tiefe Toolkit lernen. Es ist ein Befehlszeilentool (Cntk) und ist bereits im Pfad.

Um eine einfache Beispiel auszuführen, führen Sie die folgenden Befehle in der Shell:

    # Copy samples to your home directory and execute cntk
    cp -r /dsvm/tools/CNTK-2016-02-08-Linux-64bit-CPU-Only/Examples/Other/Simple2d cntkdemo
    cd cntkdemo/Data
    cntk configFile=../Config/Simple.cntk

Modell-Ausgabe erfolgt im *~/cntkdemo/Output/Models*.

Weitere Informationen finden Sie in Abschnitt CNTK [GitHub](https://github.com/Microsoft/CNTK)und [CNTK-Wiki](https://github.com/Microsoft/CNTK/wiki).


#### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit ist ein System mit Techniken wie online, hashing, Allreduce, Reduzierung der learning2search, aktiv, lernen und interaktiven Lernens.

Führen Sie das Tool auf ein sehr einfaches Beispiel wie folgt:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Es gibt andere, größere Demos in diesem Verzeichnis. Weitere Informationen zu VW finden Sie unter [Dieser Abschnitt GitHub](https://github.com/JohnLangford/vowpal_wabbit)und [Vowpal Wabbit-Wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
Dies ist eine Bibliothek, die entwickelt und optimiert für stärkere (Struktur) Algorithmen. Das Ziel dieser Bibliothek ist die Berechnung Maschinen mussten umfangreiche Struktur angehoben, der extremen Grenzen skalierbar, portable und präzise.

Es dient als Befehlszeile ein R-Bibliothek.

Verwendung dieser Bibliothek r können zunächst eine interaktive Sitzung R (nur Eingabe von **R** in der Shell) und der Bibliothek.

Hier ist ein einfaches Beispiel R Aufforderung ausführen:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Zum Ausführen der Befehlszeile Xgboost sind hier die Befehle in der Shell ausführen:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


.Model-Datei wird in das angegebene Verzeichnis geschrieben. Informationen zu diesem Demobeispiel finden [auf GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Weitere Informationen zu Xgboost finden Sie unter [Xgboost-Dokumentationsseite](https://xgboost.readthedocs.org/en/latest/)und [Github Repository](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Klappern
GUI-basierte Exploration und Modellierung verwendet Rasseln ( **R** **eine**analytische **T**Ool **T**o **L**verdienen **E**gendarmenmarktes). Zeigt statistische und visuelle Übersichten Daten Transformationen, die problemlos verwendet werden, unbeaufsichtigt und überwacht Modelle aus den Daten erstellt, stellt grafisch dar, die Leistung der Modelle und Ergebnisse neuer Daten. Er generiert außerdem R Code replizieren die Vorgänge in der Benutzeroberfläche, die auf R ausführen oder zur weiteren Analyse als Ausgangspunkt verwendet werden können.

Führen Sie Klappern müssen Sie in einem grafisch desktop-Sitzung. Geben Sie das Terminal ```R``` R-Umgebung eingeben. R aufgefordert werden Geben Sie die folgenden Befehle:

    library(rattle)
    rattle()

Jetzt öffnet eine Benutzeroberfläche mit Registerkarten. Hier sind die ersten Schritte Klappern ein Beispieldataset Wetter und Modell benötigt. Einige Schritte werden Sie automatisch installieren und Laden Sie einige erforderliche R-Pakete, die nicht bereits auf dem System.

>[AZURE.NOTE] Haben Sie Zugriff auf das Paket im Systemverzeichnis (Standard) zu installieren, sehen Sie eine Meldung im Fenster Konsole R Bibliothek Pakete installieren. Beantworten Sie *y* , wenn diese Aufforderung angezeigt.

1. Klicken Sie auf **Ausführen**.
2. Ein Dialogfeld erscheint, ob das Wetter Beispieldataset verwenden möchten. Klicken Sie auf **Ja,** um das Beispiel zu laden.
3. Klicken Sie auf **die Registerkarte** .
4. Klicken Sie auf **Ausführen** , um eine Entscheidungsstruktur zu erstellen.
5. Klicken Sie auf **Zeichnen** , um die Struktur anzuzeigen.
6. Klicken Sie auf die **Gesamtstruktur** , und klicken Sie auf **Ausführen** , um eine zufällige Gesamtstruktur erstellen.
7. Klicken Sie auf **Auswerten** .
8. Klicken Sie auf das Optionsfeld **Risiko** und auf **Execute** um zwei Risiko (kumuliert) Leistung Plots anzuzeigen.
9. Klicken Sie auf die Registerkarte **Protokoll** , um den Code generieren R vorhergehenden Arbeitsgänge anzuzeigen.
(Aufgrund eines Fehlers in der aktuellen Version von Rasseln eingefügt werden muss ein *#* im Text des Protokolls vor *diesem Protokoll exportieren* Zeichen.)
10. Klicken Sie **Exportieren** R-Skript mit dem Namen *Weather_script speichern. R* auf den Basisordner.

Beenden von Rattle und R. Sie können jetzt ändern Sie das generierte Skript R oder verwenden wie jederzeit ausführen, um alles wiederholen, die innerhalb der Benutzeroberfläche Rattern durchgeführt wurde. Insbesondere für Anfänger r ist dies so schnell analysieren und Computer lernen in einer einfachen Benutzeroberfläche automatisch generierter Code r ändern und/oder erfahren.


## <a name="next-steps"></a>Nächste Schritte
Hier ist wie Ihr lernen und Untersuchung fortgesetzt werden kann:

* [Datenwissenschaft auf Daten Wissenschaft virtuellen Linux-Maschine](machine-learning-data-science-linux-dsvm-walkthrough.md) Exemplarische Vorgehensweise veranschaulicht die mehrere allgemeine Daten Wissenschaft mit Linux Daten Wissenschaft VM bereitgestellt hier auszuführen. 
* Untersuchen Sie die verschiedenen Wissenschaft Tools auf Daten VM versuchen Sie in diesem Artikel beschriebenen Tools. Sie können auch *Dsvm Weitere Informationen* auf dem virtuellen Computer für eine Einführung und Hinweise auf Weitere Informationen zu den auf dem virtuellen Computer installierten Tools ausführen.  
* Informationen Sie zum End-to-End-Lösungen systematisch mit [Team wissenschaftliche Daten](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/)erstellen.
* Besuchen Sie [Cortana Analytics-Katalog](http://gallery.cortanaanalytics.com) für Computer lernen und Data Analytics Beispiele, die cortana Analytics Suite verwenden.
