<properties
    pageTitle="Richten Sie einen virtuellen Computer SQL Server als Server IPython Notebook | Microsoft Azure"
    description="Festlegen von einem Daten Wissenschaft virtuellen Computer IPython Server mit SQL Server."
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
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Einrichten eines Azure SQL Server virtueller Computers als Server IPython Notebook für erweiterte Analyse

Dieses Thema wird zum Bereitstellen und konfigurieren einen virtuellen Computer SQL Server als Teil einer Umgebung Wissenschaft cloudbasierten verwendet werden. Windows Virtual Machine ist mit Tools wie IPython Notebook, Azure Storage Explorer und AzCopy sowie andere Dienstprogramme für Forschungsprojekte Daten konfiguriert. Azure Storage Explorer und AzCopy, ermöglichen z. B. einfache Daten Azure Blob-Speicher vom lokalen Computer hochladen oder von BLOB-Speicher auf Ihrem lokalen Computer herunterladen.

Azure virtuellen Katalog enthält mehrere Bilder, die Microsoft SQL Server enthalten. Wählen Sie ein SQL Server-VM-Bild, das für Ihre Daten geeignet ist. Empfohlene Bilder sind:

- SQL Server 2012 SP2 Enterprise für kleine und mittlere Daten
- SQL Server 2012 SP2 Enterprise optimiert für DataWarehousing Arbeitslasten für große bis sehr große Datenmengen

 > [AZURE.NOTE] SQL Server 2012 SP2 Enterprise Bild **enthält keinen Datenträger**. Sie müssen hinzufügen, oder fügen ein oder mehrere virtuelle Festplatten zum Speichern von Daten. Beim Erstellen einer Azure virtuellen Computers verfügt über eine Festplatte für das Betriebssystem auf Laufwerk C zugeordnet und ein temporärer Speicherplatz auf Laufwerk D zugeordnet. Verwenden Sie Laufwerk D nicht zum Speichern von Daten. Wie der Name schon sagt, wird die temporäre Speicherung. Sie bietet Redundanz keine Sicherung, da in Azure-Speicher befinden wird.


##<a name="Provision"></a>Mit der Azure-Verwaltungsportal und Bereitstellen einen virtuellen SQL Server-Computer

1.  Melden Sie sich mit Ihrem Konto [Azure-Verwaltungsportal](http://manage.windowsazure.com/) .
    Wenn Sie nicht über ein Azure-Konto verfügen, besuchen Sie [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).

2.  Klicken Sie auf der Azure-Verwaltungsportal unten links der Webseite auf **+ neu**, klicken Sie auf **berechnen**klicken Sie auf **virtuelle Computer**und klicken Sie dann auf **Aus Galerie**.

3.  Aktivieren Sie auf der Seite **Erstellen eines virtuellen Computers** Abbild eines virtuellen Computers mit SQL Server basierend auf Ihren Bedürfnissen und klicken Sie dann auf den Pfeil rechts unten auf der Seite. Die Informationen auf die unterstützten SQL Server Bilder Azure finden Sie unter [Erste Schritte mit SQL Server in Azure Virtual Machines](http://go.microsoft.com/fwlink/p/?LinkId=294720) Thema in der Dokumentation zu [SQL Server in Azure Virtual Machines](http://go.microsoft.com/fwlink/p/?LinkId=294719) .

    ![SQL Server-VM auswählen][1]

4.  Angaben Sie auf der ersten Seite **Konfiguration des virtuellen Computers** folgende:

    -   Geben Sie einen **NAME der virtuellen MASCHINE**.
    -   Geben Sie im Feld **Neuer Benutzername** Benutzernamen für das lokale Administratorkonto VM.
    -   Geben Sie im Feld **Neues Kennwort** ein Kennwort ein. Weitere Informationen finden Sie unter [Sichere Kennwörter](http://msdn.microsoft.com/library/ms161962.aspx).
    -   Im Feld **Kennwort bestätigen** das Kennwort erneut ein.
    -   Wählen Sie die entsprechende **Größe** aus der Dropdown-Liste.

     > [AZURE.NOTE] Die Größe des virtuellen Computers während der Bereitstellung angegeben: A2 ist die kleinste Größe für Produktionsarbeitslasten empfohlen. Die empfohlene Mindestgröße für eine virtuelle Maschine ist A3, wenn SQL Server Enterprise Edition verwenden. Wählen Sie A3 oder höher, wenn Sie SQL Server Enterprise Edition verwenden. Wählen Sie bei Verwendung von SQL Server 2012 oder 2014 Unternehmen optimiert für transaktionale Workloads Bilder A4.
Wählen Sie bei Verwendung von SQL Server 2012 oder 2014 Unternehmen optimiert für Data Warehousing Arbeitslasten Bilder A7. Die ausgewählte Größe begrenzt die Anzahl der Datenträger, die Sie konfigurieren können. Aktuelle Informationen zu verfügbaren virtuellen Größe und der Anzahl der Datenträger einer virtuellen Maschine Anfügen finden Sie [Größen für Azure Virtual Machine](http://msdn.microsoft.com/library/azure/dn197896.aspx). Informationen zu Preisen, finden Sie unter [Virtuelle Cricket Preise](https://azure.microsoft.com/pricing/details/virtual-machines/).

    Klicken Sie auf den Pfeil rechts unten fort.

    ![VM-Konfiguration][2]

5.  Konfigurieren Sie auf der zweiten Seite **Konfiguration des virtuellen Computers** für Netzwerke, Speicher und Verfügbarkeit:

    -   Wählen Sie im **Cloud-Dienst** **eine neue Cloud-Dienst erstellen**.
    -   Geben Sie im **Cloud Service DNS-Namen** den ersten Teil einen DNS-Namen Ihrer Wahl, um einen Namen im Format **TESTNAME.cloudapp.net** Abschluss
    -   Wählen Sie im **Bereich-AFFINITÄT Gruppe und virtuellen Netzwerk** einen Bereich dieser virtuellen Bildes Hosting.
    -   **Konto**wählen ein Storage-Konto oder eine automatisch generierte.
    -   Wählen Sie im Feld **Verfügbarkeit** **(keine)**.
    -   Lesen Sie und akzeptieren Sie die Preisinformationen.

6.  Klicken Sie in der leeren Dropdownliste unter **NAME**im Abschnitt **ENDPUNKTE** wählen Sie **MSSQL** und geben Sie die Portnummer der Instanz des Datenbankmoduls (**1433** für die Standardinstanz).

7.  Der SQL Server-VM kann auch als IPython Notebook-Server dienen, das in einem späteren Schritt konfiguriert werden.
    Hinzufügen eines neuen Endpunkts um den Port für den Server IPython Notebook verwenden. Geben Sie einen Namen in der Spalte **NAME** , wählen Sie eine Portnummer für die öffentliche Schnittstelle, und 9999 für den privaten Anschluss aus.

    Klicken Sie auf den Pfeil rechts unten fort.

    ![Wählen Sie Ports MSSQL und IPython][3]

8.  Übernehmen Sie die Standardoption **VM installieren Agent** aktiviert und auf das Häkchen in der unteren rechten Ecke des Assistenten die VM Bereitstellung abgeschlossen.

    `![Letzte VM-Optionen][4]

9.  Warten Sie, während Azure den virtuellen Computer vorbereitet. Erwarten Sie virtuellen Status durchlaufen:

    -   Starten (Bereitstellung)
    -   Beendet
    -   Starten (Bereitstellung)
    -   Ausgeführt (Bereitstellung)
    -   Ausführen


##<a name="RemoteDesktop"></a>Öffnen Sie den virtuellen Computer mithilfe von Remotedesktop und Setup abzuschließen

1.  Bereitstellung abgeschlossen ist, klicken Sie auf den Namen des virtuellen Computers die Seite DASHBOARD zu. **Klicken Sie auf am unteren Rand der Seite.**

2.  Mithilfe des Programms Windows Remote Desktop Rpd-Datei öffnen (`%windir%\system32\mstsc.exe`).

3.  Geben Sie das Kennwort für das lokale Administratorkonto, das Sie in einem früheren Schritt angegeben, klicken Sie im Dialogfeld **Windows-Sicherheit** .
    (Sie u. u. die Anmeldeinformationen des virtuellen Computers überprüfen.)

4.  Beim ersten an diesem virtuellen Computer anmelden müssen mehrere Prozesse abgeschlossen einschließlich Setup Desktop, Windows-Updates und Windows Erstkonfiguration Aufgaben (Sysprep). Nach Abschluss der Windows Sysprep schließt SQL Server Setup Konfigurationsaufgaben. Diese Aufgaben möglicherweise eine Verzögerung von wenigen Minuten, während sie abgeschlossen. `SELECT @@SERVERNAME`kann den richtigen Namen erst beendet SQL Server Setup abgeschlossen ist und SQL Server Management Studio möglicherweise nicht auf der Startseite sichtbar.

Nachdem Sie den virtuellen Computer mit Windows-Remotedesktop verbunden sind, funktioniert der virtuelle Computer wie andere Computer. Verbinden Sie mit der Standardinstanz von SQL Server mit SQL Server Management Studio (auf dem virtuellen Computer ausgeführt) auf die übliche Weise.


##<a name="InstallIPython"></a>IPython Notebook und andere unterstützende Tools installieren

Konfigurieren Ihrer neuen SQL Server-VM als ein IPython Notebook Server und zusätzliche unterstützende Tools installieren, solche AzCopy, Azure Storage Explorer und nützliche Daten Wissenschaft Python-Pakete ist ein spezieller Benutzerskript bereitgestellt. So installieren Sie:

- Mit der rechten Maustaste des Symbols **Windows starten** und auf der **Befehlszeile (Admin)**
- Die folgenden Befehle Kopieren und Einfügen in die Befehlszeile eingeben.

        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

- Bei Aufforderung geben Sie ein Kennwort Ihrer Wahl für den Server IPython Notebook.
- Anpassung Skript automatisiert mehrere nach Abschluss der Installation Verfahren gehören:
    + Installation und Setup IPython Notebook-server
    + Öffnen TCP-Ports in der Windows-Firewall für die zuvor erstellte Endpunkte:
    + SQL Server-Remotekonnektivität
    + IPython Notebook Server-Remotekonnektivität
    + Abrufen von IPython Beispielnotizbücher und SQL-Skripts
    + Herunterladen und Installieren von nützliche Daten Wissenschaft Python-Pakete
    + Herunterladen und Installieren von Azure Tools wie AzCopy und Azure-Speicher-Explorer  
<br>
- Möglicherweise Zugriff auf und führen IPython Notebook über einen Webbrowser lokal oder remote mit einem URL der Form `https://<virtual_machine_DNS_name>:<port>`, wobei Anschluss IPython öffentlichen Port beim virtuellen Computer bereitstellen aktiviert.
- IPython Notebook-Server wird als Hintergrunddienst ausgeführt und automatisch gestartet wird, beenden Sie den virtuellen Computer.

##<a name="Optional"></a>Legen Sie Datenträger nach Bedarf

Enthält die VM-Image keinen Datenträger, d. h. Festplatten als Laufwerk C (OS Festplatte) und Laufwerk D (temporärer), müssen Sie mindestens einen Datenträger zum Speichern von Daten hinzufügen. VM-Image für SQL Server 2012 SP2 Enterprise optimiert für DataWarehousing Arbeitslasten enthält vorkonfigurierte zusätzliche Datenträger für SQL Server-Daten und Protokolldateien.

 > [AZURE.NOTE] Verwenden Sie Laufwerk D nicht zum Speichern von Daten. Wie der Name schon sagt, wird die temporäre Speicherung. Sie bietet Redundanz keine Sicherung, da in Azure-Speicher befinden wird.

Um zusätzliche Datenträger anfügen, befolgen Sie die Schritte unter [How to Attach einen Datenträger auf einem Windows-Computer](virtual-machines-windows-classic-attach-disk.md), der führt Sie durch:

1. Der virtuelle Computer bereitgestellt in früheren Schritten zuweisen leere Datenträger
2. Initialisierung der neuen Laufwerke auf dem virtuellen Computer


##<a name="SSMS"></a>Mit SQL Server Management Studio und die aktivieren Authentifizierung im gemischten Modus

Die SQL Server-Datenbank-Engine kann nicht ohne Domäne Windows-Authentifizierung verwenden. Die Verbindung mit der Datenbank-Engine von einem anderen Computer konfigurieren Sie SQL Server für die Authentifizierung im gemischten Modus. Authentifizierung im gemischten Modus kann sowohl Windows-Authentifizierung als auch SQL Server-Authentifizierung. SQL-Authentifizierungsmodus ist erforderlich, um Daten direkt aus der SQL Server-VM Datenbanken im [Azure Machine Learning Studio](https://studio.azureml.net) Moduls Import aufnehmen.

1.  Beim virtuellen Computer über Remotedesktop verbunden, im Windows- **Suche** und geben Sie **SQL Server Management Studio** (SMSS). Klicken Sie zum Starten von SQL Server Management Studio (SSMS). Sie möchten eine Verknüpfung SSMS auf Ihrem Desktop für die zukünftige Verwendung hinzufügen.

    ![SSMS starten][5]

    Beim ersten Öffnen von Management Studio müssen sie Benutzer Management Studio-Umgebung erstellen. Dies kann einen Moment dauern.

2.  Beim Öffnen wird Management Studio im Dialogfeld **Verbindung mit Server herstellen** . Geben Sie im Feld **Servername** den Namen des virtuellen Computers für das Datenbankmodul mit Objekt-Explorer verbinden.
    (Nicht den virtuellen Namen auch **(lokal)** oder ein Punkt als **Servernamen**können Sie. Wählen Sie **Windows-Authentifizierung**, und lassen Sie ** *der\_VM\_Name*\\der\_lokale\_Administrator** in das Feld **Benutzername** . Klicken Sie auf **Verbinden**.

    ![Verbindung zu Server herstellen][6]

    <br>

     > [AZURE.TIP] Den SQL Server-Authentifizierungsmodus eine wichtige Änderung der Windows Registrierung oder SQL Server Management Studio kann geändert werden. Um mit der Registrierung Schlüssel ändern, starten Sie eine **Neue Abfrage** zu, und führen Sie Folgendes Skript:

        USE master
        go

        EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
        go


    So ändern Sie den Authentifizierungsmodus mit SQL Server Management Studio

3.  Namen Sie im **Objekt-Explorer von SQL Server Management Studio**den der Instanz von SQL Server (virtuelle Rechnername) und klicken Sie dann auf **Eigenschaften**.

    ![Server-Eigenschaften][7]

4.  Wählen Sie auf der Seite **Sicherheit** unter **Authentifizierung** **SQL Server und Windows-Authentifizierung**, und klicken Sie auf **OK**.

    ![Authentifizierungsmodus auswählen][8]

5.  Klicken Sie im Dialogfeld **SQL Server Management Studio** auf **OK** , um die Anforderung an SQL Server neu starten zu bestätigen.

6.  Im **Objekt-Explorer**mit der rechten Maustaste des Servers und dann auf **neu starten**. (SQL Server-Agent ausgeführt wird, muss er ebenfalls neu gestartet werden.)

    ![Neu starten][9]

7.  Klicken Sie auf **Ja** , um zu bestätigen, dass Sie SQL Server neu starten möchten, klicken Sie im Dialogfeld **SQL Server Management Studio** .

##<a name="Logins"></a>SQL Server-Authentifizierung Benutzernamen erstellen

Die Verbindung mit der Datenbank-Engine von einem anderen Computer müssen Sie mindestens einen SQL Server-Authentifizierung Benutzernamen erstellen.  

Sie können neue SQL Server-Benutzernamen programmgesteuert oder mithilfe von SQL Server Management Studio. Starten Sie eine **Neue Abfrage** zu und führen Sie Folgendes Skript, um einen neuen Sysadmin-Benutzer mit SQL-Authentifizierung zu erstellen. Ersetzen Sie < neuen Benutzernamen\> und < Neues Kennwort\> mit Ihrem *Benutzernamen* und *Kennwort*. 


    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


Passen Sie die Kennwortrichtlinie Bedarf (das Beispiel Code deaktiviert Richtlinie geprüft und Kennwort Ablaufdatum an). Weitere Informationen zu SQL Server-Benutzernamen finden Sie unter [Erstellen einer Anmeldung](http://msdn.microsoft.com/library/aa337562.aspx).  

So erstellen Sie neue SQL Server-Benutzernamen mithilfe von SQL Server Management Studio

1.  Erweitern Sie im **Objekt-Explorer von SQL Server Management Studio**-Serverinstanz, in dem Sie den neuen Benutzernamen erstellen möchten.

2.  Klicken Sie den Ordner **Sicherheit** , zeigen Sie auf **neu**und wählen Sie **Anmeldung...**

    ![Neuer Benutzername][10]

3.  Geben Sie im Dialogfeld **Anmeldung - neu** auf der Registerkarte **Allgemein** den Namen des neuen Benutzers im Feld **Benutzername** aus.

4.  Wählen Sie **SQL Server-Authentifizierung**.

5.  Geben Sie im Feld **Kennwort** ein Kennwort für den neuen Benutzer. Geben Sie das Kennwort erneut in das Feld **Kennwort bestätigen** .

6.  Wählen Sie zum Erzwingen der Richtlinie Kennwortoptionen Komplexität und Durchsetzung **Kennwortrichtlinie erzwingen** (empfohlen). Dies ist Standardoption, wenn SQL Server-Authentifizierung ausgewählt ist.

7.  Wählen Sie enforce Richtlinienoptionen Kennwort abgelaufen **Kennwortablauf erzwingen** (empfohlen). Erzwingen Kennwortrichtlinie muss aktiviert sein, um dieses Kontrollkästchen zu aktivieren. Dies ist Standardoption, wenn SQL Server-Authentifizierung ausgewählt ist.

8.  Erzwingen den Benutzer ein neues Kennwort nach dem ersten Erstellen der Benutzername verwendet wird, wählen Sie **Benutzer muss Kennwort bei der nächsten Anmeldung ändern** (empfohlen, wenn dieser Anmeldung für jemand anders verwendet wird. Ist die Anmeldung zur eigenen Verwendung nicht dieser Option.) Erzwingen Kennwortablauf muss aktiviert sein, um dieses Kontrollkästchen zu aktivieren. Dies ist Standardoption, wenn SQL Server-Authentifizierung ausgewählt ist.

9.  Wählen Sie aus **Standarddatenbank** Standarddatenbank für den Benutzernamen. **Master** ist der Standardwert für diese Option. Wenn Sie eine Datenbank noch nicht erstellt haben, Standardeinstellung **master**.

10. **In der Liste **Standardsprache** Standardwert als Wert.**

    ![Anmelde-Eigenschaften][11]

11. Ist dies die erste Anmeldung, die Sie erstellen, möchten Sie diesen Benutzernamen als SQL Server-Administrator festlegen. Dann überprüfen Sie auf der Seite **Serverrollen** **Sysadmin**.

    > [AZURE.IMPORTANT] Mitglieder der festen Serverrolle Sysadmin haben vollständige Kontrolle über die Datenbank-Engine. Aus Sicherheitsgründen sollten Sie sorgfältig die Mitgliedschaft in dieser Rolle einschränken.

    ![Systemadministrator][12]

12. Klicken Sie auf OK.

##<a name="DNS"></a>Bestimmen der DNS-Name des virtuellen Computers

Verbindung zu SQL Server-Datenbank-Engine von einem anderen Computer benötigen Sie Namen (DNS = Domain Name System) des virtuellen Computers.

(Dies ist der Name, der zur Identifizierung des virtuellen Computers auf das Internet zugreift. Die IP-Adresse verwenden, aber die IP-Adresse ändert wechselt Azure Ressourcen für Redundanz oder Wartung. Der DNS-Name ist stabil, da eine neue IP-Adresse umgeleitet werden kann.)

1.  Wählen Sie **virtuelle Computer**in Azure-Verwaltungsportal (oder aus dem vorherigen Schritt).

2.  Suchen Sie auf der Seite **VIRTUAL MACHINE-Instanzen** in der **DNS-NAME** -Spalte und kopieren Sie den DNS-Namen für den virtuellen Computer die **http://**vorangestellt. (Die Benutzeroberfläche möglicherweise nicht den gesamten Name angezeigt, aber Sie können mit der rechten Maustaste darauf und wählen Sie kopieren.)

##<a name="cde"></a>Verbinden Sie mit der Datenbank-Engine von einem anderen computer

1.  Öffnen Sie SQL Server Management Studio auf einem Computer mit dem Internet verbunden.

2.  Klicken Sie im Dialogfeld **Verbindung mit Server herstellen** oder **Verbindung mit Datenbankmodul herstellen** im Feld **Servername** Geben Sie den DNS-Namen der virtuellen Computer (wie in der vorherigen Aufgabe) und öffentlichen Endpunkt Anschlussnummer im Format *DNSName Portnummer* wie **tutorialtestVM.cloudapp.net,57500**.

3.  Wählen Sie im **Feld** **SQL Server-Authentifizierung**.

4.  Geben Sie im Feld **Benutzername** ein Benutzername, der in einem früheren Vorgang erstellt.

5.  Geben Sie im Feld **Kennwort** das Kennwort für die Anmeldung, die in einem früheren Vorgang erstellen.

6.  Klicken Sie auf **Verbinden**.

##<a name="amlconnect"></a>Verbindung mit der Datenbank-Engine von Azure maschinelles lernen

In späteren Phasen des Data Science verwenden Sie [Azure Machine Learning Studio](https://studio.azureml.net) erstellen und Bereitstellen von Learning Modelle. Um Daten aus der SQL Server-VM-Datenbanken direkt in Azure Machine Learning bzw. Bewertung Aufnahme mithilfe des **Importierten Daten** in einem neuen [Azure Machine Learning Studio](https://studio.azureml.net) . Dieses Thema wird ausführlicher über Links Guide Team wissenschaftliche Daten behandelt. Eine Einführung finden Sie unter [Neuigkeiten Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md).

2.  Wählen Sie im **Eigenschaftenbereich des [Importdaten Modul](https://msdn.microsoft.com/library/azure/dn905997.aspx)** **Azure SQL-Datenbank** aus der Dropdownliste **Datenquelle** .

3.  Geben Sie im Textfeld **Servername**`tcp:<DNS name of your virtual machine>,1433`

4.  Geben Sie den SQL-Benutzernamen im Textfeld **Server Benutzerkontonamen** .

5.  Geben Sie im Textfeld **Benutzerkennwort Server** Sql-Benutzerkennwort.

    ![Azure ML Importdaten][13]

##<a name="shutdown"></a>Herunterfahren und virtuellen Computer nicht freigeben

Azure Virtual Machines werden als **nur bezahlen, was Sie verwenden**Preis. Um sicherzustellen, dass Sie nicht beim virtuellen Computer nicht verwenden abgerechnet werden, muss es im Zustand **beendet (Deallocated)** .

> [AZURE.NOTE] Der virtuelle Computer heruntergefahren von innen (mit Windows-Energieoptionen) die VM beendet jedoch bleibt zugeordnet. Um sicherzustellen, dass Sie nicht in Rechnung gestellt werden, immer halten Sie virtueller Maschinen von [Azure-Verwaltungsportal an](http://manage.windowsazure.com/). Die VM über Powershell hält durch Aufrufen von ShutdownRoleOperation mit "PostShutdownAction" "StoppedDeallocated" entspricht.

Herunterfahren und den virtuellen Computer freigeben:

1. Melden Sie sich mit Ihrem Konto [Azure-Verwaltungsportal](http://manage.windowsazure.com/) .  

2. Wählen Sie die **virtuellen Computer** in der linken Navigationsleiste.

3. In der Liste der virtuellen Computer auf den Namen des virtuellen Computers Klicken zur **DASHBOARD** -Seite.

4. Klicken Sie am unteren Rand der Seite auf **Herunterfahren**.

![VM Herunterfahren][15]

Der virtuelle Computer wird freigegeben, aber nicht gelöscht. Jederzeit von Azure-Verwaltungsportal kann den virtuellen Computer neu starten.

## <a name="your-azure-sql-server-vm-is-ready-to-use-whats-next"></a>Azure SQL Server-VM ist einsatzbereit: Was?

Ihr virtueller Computer kann jetzt Daten Wissenschaft Übungen verwenden. Der virtuelle Computer ist auch einsatzbereit als IPython Notebook-Server für die Untersuchung und Verarbeitung von Daten und andere Aufgaben in Verbindung mit Azure Maschine und Team Daten Wissenschaft Prozess (TDSP).

Die nächsten Schritte im Prozess Wissenschaft im [Team wissenschaftliche Daten](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) zugeordnet und Verschieben von Daten in HDInsight, verarbeiten und es zur Vorbereitung von Daten mit Azure Computer lernen probieren Schritte einschließen.


[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png
 
