<properties
    pageTitle="Konfigurieren der Oracle DataGuard in VMs | Microsoft Azure"
    description="Schrittweise Anleitung zur Einrichtung und Implementierung von Oracle Data Guard in Azure virtuelle Computer für hohe Verfügbarkeit und Disaster Recovery."
    services="virtual-machines-windows"
    authors="rickstercdn"
    manager="timlt"
    documentationCenter=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/06/2016"
    ms.author="rclaus" />

#<a name="configuring-oracle-data-guard-for-azure"></a>Konfigurieren der Oracle DataGuard für Azure


Dieses Lernprogramm veranschaulicht und Oracle Data Guard in Azure Virtual Machines Umgebung für hohe Verfügbarkeit und Disaster Recovery implementieren. Das Lernprogramm Schwerpunkt unidirektionalen Replikation für RAC Oracle-Datenbanken.

Oracle Data Guard unterstützt Datenschutz und Disaster Recovery für Oracle-Datenbanken. Ist eine einfache, leistungsstarke Technik Lösung für Disaster Recovery, Datenschutz und hohe Verfügbarkeit für die gesamte Oracle-Datenbank.

Diese praktische Einführung geht schon theoretischen und praktischen Kenntnisse Oracle Datenbank mit hoher Verfügbarkeit und Disaster Recovery. Informationen finden Sie in der [Oracle-Website](http://www.oracle.com/technetwork/database/features/availability/index.html) und [Oracle Data Guard Konzepte und Administration Guide](https://docs.oracle.com/cd/E11882_01/server.112/e41134/toc.htm).

Das Lernprogramm vorausgesetzt darüber hinaus die folgenden erforderlichen Komponenten bereits implementiert haben:

- Sie haben bereits hohe Verfügbarkeit und Disaster Recovery Aspekte Abschnitt im Thema [Oracle virtuellen Computerimages - verschiedene Aspekte](virtual-machines-windows-classic-oracle-considerations.md) überprüft. Azure unterstützt eigenständige Oracle-Datenbankinstanzen zurzeit keine Oracle Real Application Clusters (Oracle RAC).


- Sie haben zwei virtuellen Maschinen (VMs) in Azure Verwendung derselben Plattform Oracle Enterprise Edition Bild erstellt. Stellen Sie sicher, dass die virtuellen Computer im [selben Cloud-Dienst](virtual-machines-windows-load-balance.md) und im gleichen virtuellen Netzwerk um sicherzustellen, dass sie einander über den permanenten privaten IP-Adresse zugegriffen werden können. Außerdem wird empfohlen, setzen die VMs in derselben [Verfügbarkeit](virtual-machines-windows-manage-availability.md) zu Azure in separaten Fehlerdomänen und Aktualisieren von Domänen. Oracle Data Guard ist nur mit Oracle Database Enterprise Edition verfügbar. Jeder Computer muss über mindestens 2 GB Arbeitsspeicher und 5 GB Speicherplatz. Aktuelle Informationen über die Plattform VM Größen finden Sie [Größen für Azure Virtual Machine](virtual-machines-windows-sizes.md). Benötigen Sie zusätzliche Datenträger für Ihre VMs können Sie zusätzliche Festplatten anhängen. Informationen finden Sie unter [wie ein Datenträger mit einem virtuellen Computer](virtual-machines-windows-classic-attach-disk.md).



- Die virtuellen Namen haben als "Machine1" primäre VM und "Machine2" für die Standby-VM zur klassischen Azure-Portal festlegen.

- Die **ORACLE_HOME** -Umgebungsvariable wie auf der gleichen Oracle Installation Stammpfad primäre und Standby virtuelle Computer festgelegt haben `C:\OracleDatabase\product\11.2.0\dbhome_1\database`.

- Anmeldung auf dem WindowsServer als Mitglied **der Administratorengruppe** oder ein Mitglied der Gruppe **ORA_DBA** .

In diesem Lernprogramm werden Sie:

Implementieren der physischen Standbydatenbank Umgebung

1. Erstellen einer primären Datenbank

2. Die primäre Datenbank Vorbereitung Standbydatenbank erstellen

    1. Erzwungene Protokollierung aktivieren

    2. Erstellen Sie eine Datei

    3. Konfigurieren einer standby wiederherstellen

    4. Aktivieren der Archivierung

    5. Primäre Datenbank Initialisierung Parameter

Erstellen Sie eine physische Standby-Datenbank

1. Parameter Initialisierungsdatei für Standbydatenbank vorbereiten

2. Konfigurieren Sie die Listener und Tnsnames die Datenbank auf primäre und Standby-Unterstützung

    1. Konfigurieren Sie listener.ora auf beiden Servern Einträge für beide Datenbanken

    2. Enthält Einträge für primäre und Standby-Datenbanken konfigurieren Sie tnsnames.ora primäre und standby-VMs. 

    3. Starten Sie den Listener und überprüfen Sie Tnsping auf beiden virtuellen Computern an beide Dienste.

3. Starten Sie die Standby-Instanz in Nomount Zustand

4. Verwenden Sie RMAN der Datenbank und erstellen eine Standby-Datenbank

5. Starten Sie physische Standbydatenbank im verwalteten Wiederherstellungsmodus

6. Überprüfen Sie die physische Standbydatenbank

> [AZURE.IMPORTANT] In diesem Lernprogramm wurde einrichten und mit der folgenden Konfiguration von Hardware und Software getestet:
>
>|                      | **Primäre Datenbank**                      | **Standby-Datenbanken**                      |
>|----------------------|-------------------------------------------|-------------------------------------------|
>| **Oracle-Version**   | Oracle11g Enterprise-Version (11.2.0.4.0) | Oracle11g Enterprise-Version (11.2.0.4.0) |
>| **Name des Computers**     | Machine1                                  | Machine2                                  |
>| **Betriebssystem** | Windows 2008 R2                           | Windows 2008 R2                           |
>| **Oracle SID**       | TEST                                      | Prüfung\_STBY                                |
>| **Speicher**           | Min. 2 GB                                  | Min. 2 GB                                  |
>| **Speicherplatz**       | Min 5 GB                                  | Min 5 GB                                  |

Für nachfolgende Versionen von Oracle Database und Oracle Data Guard möglicherweise einige weiteren Änderungen, die Sie implementieren müssen. Aktuelle versionsspezifische Informationen Dokumentation [Data Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) und [Oracle-Datenbank](http://www.oracle.com/us/corporate/features/database-12c/index.html) auf Oracle-Website.

##<a name="implement-the-physical-standby-database-environment"></a>Implementieren der physischen Standbydatenbank Umgebung
### <a name="1-create-a-primary-database"></a>1. erstellen Sie 1. eine primäre Datenbank

- Erstellen einer primären Datenbank "TEST" im primären virtuellen Computer. Informationen finden Sie unter Erstellen und Konfigurieren einer Oracle-Datenbank.
- Um den Namen der Datenbank finden Sie unter Verbinden mit Ihrer Datenbank als SYS-Benutzer mit SYSDBA-Rolle in der SQL * Plus Befehlszeile und führen Sie die folgende Anweisung:

        SQL> select name from v$database;

        The result will display like the following:

        NAME
        ---------
        TEST
- Anschließend Fragen Sie die Namen der Datenbankdateien aus der Dba_data_files-Systemansicht:

        SQL> select file_name from dba_data_files;
        FILE_NAME
        -------------------------------------------------------------------------------
        C:\ <YourLocalFolder>\TEST\USERS01.DBF
        C:\ <YourLocalFolder>\TEST\UNDOTBS01.DBF
        C:\ <YourLocalFolder>\TEST\SYSAUX01.DBF
        C:\<YourLocalFolder>\TEST\SYSTEM01.DBF
        C:\<YourLocalFolder>\TEST\EXAMPLE01.DBF

### <a name="2-prepare-the-primary-database-for-standby-database-creation"></a>2. Vorbereitung der Primärdatenbank Standbydatenbank erstellen

Bevor eine Standbydatenbank erstellen, empfiehlt es sich sicherzustellen, dass die primäre Datenbank ordnungsgemäß konfiguriert ist. Folgendes ist eine Liste der Schritte, die Sie durchführen müssen:

1. Erzwungene Protokollierung aktivieren
2. Erstellen Sie eine Datei
3. Konfigurieren einer standby wiederherstellen
4. Aktivieren der Archivierung
5. Primäre Datenbank Initialisierung Parameter

#### <a name="enable-forced-logging"></a>Erzwungene Protokollierung aktivieren

Zum Implementieren einer Standby-Datenbank müssen "Erzwungene Protokollierung" in der primären Datenbank aktivieren. Diese Option gewährleistet, dass auch erfolgt eine Operation 'Nologging' Force Protokollierung Vorrang alle Vorgänge die Redo-Protokolle angemeldet sind. Daher stellen wir sicher, dass alles in der primären Datenbank protokolliert und der Standby-Replikation alle Vorgänge in der primären Datenbank umfasst. Führen Sie zum Aktivieren der Protokollierung Kraft der Alter Database-Anweisung:

    SQL> ALTER DATABASE FORCE LOGGING;

    Database altered.

#### <a name="create-a-password-file"></a>Erstellen Sie eine Datei

Um und archivierte Protokolle vom primären Server auf den Standbyserver anwenden, muss das Kennwort Sys auf primäre und standby-Server identisch sein. Deshalb erstellen Sie eine Datei in der primären Datenbank und auf den Standbyserver kopieren.

>[AZURE.IMPORTANT] Bei Verwendung von Oracle Database 12 c ist ein neuer Benutzer **SYSDG**, mit Oracle Data Guard verwalten. Weitere Informationen finden Sie in der [Oracle-Datenbank Version 12 c](http://docs.oracle.com/database/121/UNXAR/release_changes.htm#UNXAR404).

Stellen Sie außerdem sicher, dass die ORACLE\_HOME-Umgebung bereits in Machine1 definiert ist. Wenn nicht, als einer Umgebungsvariablen im Dialogfeld Umgebungsvariablen definieren. Um auf dieses Dialogfeld zuzugreifen, starten Sie **das Programm** durch Doppelklicken auf das Symbol im **Bedienfeld**; dann klicken Sie auf die Registerkarte **Erweitert** , und wählen Sie **Umgebungsvariablen**. Um die Umgebungsvariablen klicken Sie auf die Schaltfläche **neu** unter **Systemvariablen**. Nach dem Einrichten der Umgebungsvariablen, schließen Sie die Windows-Befehlszeile und öffnen Sie eine neue.

Führen Sie folgende Anweisung Umstieg auf Oracle\_Home-Verzeichnis, z. B. C:\\OracleDatabase\\Produkt\\11.2.0\\Dbhome\_1\\Datenbank.

    cd %ORACLE_HOME%\database

Erstellen Sie eine Datei mit dem Kennwort Datei Erstellungsdienstprogramm [ORAPWD](http://docs.oracle.com/cd/B28359_01/server.111/b28310/dba007.htm). Führen Sie in der gleichen Windows-Befehlszeile in Machine1 den folgenden Befehl den Kennwortwert **SYS**-Kennwort festlegen:

    ORAPWD FILE=PWDTEST.ora PASSWORD=password FORCE=y

Dieser Befehl erstellt eine Datei, als PWDTEST.ora in der ORACLE\_Start\\Datenbankverzeichnis. Kopieren Sie diese Datei % ORACLE\_HOME %\\Datenbank-Verzeichnis Machine2 manuell.

#### <a name="configure-a-standby-redo-log"></a>Konfigurieren einer standby wiederherstellen

Dann müssen Sie ein Standby Redo-Protokoll so konfigurieren, dass primären Standby wird ordnungsgemäß die wiederholen erhalten. Erstellen sie vorab hier kann auch standby Redo-Protokolle auf dem Standbyserver automatisch erstellt werden. Es ist wichtig, Standby Wiederherstellen Protokolle (SRL) mit der gleichen Größe konfigurieren, wie der online-Protokolle wiederherstellen. Die Größe des aktuellen standby Redo-Protokolldateien muss genau die Größe der aktuellen primären Datenbank online-Redo-Protokolldateien übereinstimmen.

Führen Sie folgende Anweisung in den SQL\*PLUS Befehlszeile Machine1. V$ Logfile wird eine Systemansicht, die Informationen über Redo-Protokolldateien enthält.

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE    MEMBER                                                       IS_
    ---------- ------- ------- ------------------------------------------------------------ ---
    3         ONLINE  C:\<YourLocalFolder>\TEST\REDO03.LOG               NO
    2         ONLINE  C:\<YourLocalFolder>\TEST\REDO02.LOG               NO
    1         ONLINE  C:\<YourLocalFolder>\TEST\REDO01.LOG               NO
    Next, query the v$log system view, displays log file information from the control file.
    SQL> select bytes from v$log;
    BYTES
    ----------
    52428800
    52428800
    52428800


Beachten Sie, dass 52428800 gesetzt 50 MB.

Dann, in den SQL\*Plus Fenster führen Sie die folgenden Anweisungen um eine Standbydatenbank neue standby Wiederherstellen-Protokolldateigruppe hinzufügen, und geben Sie eine Nummer zur Identifizierung der Gruppe die GROUP-Klausel. Mithilfe von Gruppen können auf standby Redo Log-Dateigruppen einfacher verwalten:

    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 'C:\<YourLocalFolder>\TEST\REDO04.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 'C:\<YourLocalFolder>\TEST\REDO05.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 'C:\<YourLocalFolder>\TEST\REDO06.LOG' SIZE 50M;
    Database altered.

Führen Sie dann die folgenden Systemansicht, Informationen zu wiederholen Protokolldateien. Dieser Vorgang wird geprüft, ob die standby Redo Log Dateigruppen erstellt wurden:

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE MEMBER IS_
    ---------- ------- ------- --------------------------------------------- ---
    3         ONLINE C:\<YourLocalFolder>\TEST\REDO03.LOG NO
    2         ONLINE C:\<YourLocalFolder>\TEST\REDO02.LOG NO
    1         ONLINE C:\<YourLocalFolder>\TEST\REDO01.LOG NO
    4         STANDBY C:\<YourLocalFolder>\TEST\REDO04.LOG
    5         STANDBY C:\<YourLocalFolder>\TEST\REDO05.LOG NO
    6         STANDBY C:\<YourLocalFolder>\TEST\REDO06.LOG NO
    6 rows selected.

#### <a name="enable-archiving"></a>Aktivieren der Archivierung

Aktivieren Sie dann die Archivierung mit folgenden Aussagen die primäre Datenbank im ARCHIVELOG Modus und automatische Archivierung aktivieren. Archiv-Modus können die Datenbank und dann den Befehl Archivelog.

Zunächst als Sysdba anmelden. In der Windows-Befehlszeile ausführen:

    sqlplus /nolog

    connect / as sysdba

Dann Herunterfahren der Datenbank in die SQL\*Plus Befehlszeile:

    SQL> shutdown immediate;
    Database closed.
    Database dismounted.
    ORACLE instance shut down.

Führen Sie den Befehl Start bereitstellen, die Datenbank bereitzustellen. Dies gewährleistet, dass Oracle die Instanz mit der angegebenen Datenbank verknüpft.

    SQL> startup mount;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.

Führen Sie dann:

    SQL> alter database archivelog;
    Database altered.

Führen der Alter Database-Anweisung mit der öffnen-Klausel die Datenbank normal verfügbar:

    SQL> alter database open;

    Database altered.

#### <a name="set-primary-database-initialization-parameters"></a>Primäre Datenbank Initialisierung Parameter

Konfigurieren Sie den Data Guard müssen Parameter erstellen und Konfigurieren der standby auf regulären Pfile (Initialisierung Parameter Textdatei) erste. Bei der Pfile werden kann, müssen Sie in eine Server-Parameter (SPFILEVERKNÜPFT) konvertieren.

Sie können mithilfe der Parameter im INIT Data Guard-Umgebung steuern. ORA-Datei. Bei diesem Lernprogramm müssen Sie die primäre Datenbank INIT aktualisieren. ORA daher so beide Rollen speichern: primären oder Standby.

    SQL> create pfile from spfile;
    File created.

Als Nächstes müssen Sie Pfile fügen Sie die Standby-Parameter bearbeiten. Öffnen Sie dazu die INITTEST. ORA Datei an % ORACLE\_HOME %\\Datenbank. INITTEST.ora-Datei dann Folgendes hinzufügen. Die Namenskonvention für die Initialisierung. ORA-Datei ist INIT\<YourDatabaseName\>. ORA.

    db_name='TEST'
    db_unique_name='TEST'
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1= 'LOCATION=C:\OracleDatabase\archive   VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=TEST'
    LOG_ARCHIVE_DEST_2= 'SERVICE=TEST_STBY LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=TEST_STBY'
    LOG_ARCHIVE_DEST_STATE_1=ENABLE
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE
    LOG_ARCHIVE_FORMAT=%t_%s_%r.arc
    LOG_ARCHIVE_MAX_PROCESSES=30
    # Standby role parameters --------------------------------------------------------------------
    fal_server=TEST_STBY
    fal_client=TEST
    standby_file_management=auto
    db_file_name_convert='TEST_STBY','TEST'
    log_file_name_convert='TEST_STBY','TEST'
    # ---------------------------------------------------------------------------------------------


Der vorherige Anweisungsblock gehören drei wichtig:
-   **... LOG_ARCHIVE_CONFIG:** Mit dieser Anweisung eindeutige Datenbank-Ids definieren.
-   **... LOG_ARCHIVE_DEST_1:** Mit dieser Anweisung lokalen Archiv-Ordner definieren. Es wird empfohlen, erstellen Sie ein neues Verzeichnis für Ihre Datenbank Archivierungsfunktionen und geben den lokalen Archiv-Speicherort mithilfe dieser Anweisung explizit anstelle von Oracle Standard Ordner % ORACLE_HOME%\database\archive.
-   **LOG_ARCHIVE_DEST_2... LGWR ASYNCHRON...:** einen asynchronem Writer-Prozess (LGWR) sammeln Daten wiederherstellen und es Standby-Ziele definieren. Hier gibt der DB_UNIQUE_NAME einen eindeutigen Namen für die Datenbank auf dem Zielserver standby.

Wenn neue Parameterdatei bereit ist, müssen Sie die spfileverknüpft erstellen.

Zunächst Herunterfahren der Datenbank:

    SQL> shutdown immediate;

    Database closed.

    Database dismounted.

    ORACLE instance shut down.

Führen Sie Nomount Befehl wie folgt:

    SQL> startup nomount pfile='c:\OracleDatabase\product\11.2.0\dbhome_1\database\initTEST.ora';
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes

Erstellen Sie jetzt eine spfileverknüpft:

    SQL>create spfile frompfile='c:\OracleDatabase\product\11.2.0\dbhome\_1\database\initTEST.ora';

    File created.

Dann Herunterfahren der Datenbank:

    SQL> shutdown immediate;

    ORA-01507: database not mounted

Anschließend verwenden Sie den Startbefehl starten eine Instanz:

    SQL> startup;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.
    Database opened.

##<a name="create-a-physical-standby-database"></a>Erstellen Sie eine physische Standby-Datenbank
Dieser Abschnitt behandelt die Schritte durchführen in Machine2 physische Standbydatenbank vorbereiten müssen.

Zunächst müssen Sie Remotedesktop Machine2 über klassischen Azure-Portal.

Erstellen Sie auf dem Standbyserver (Machine2) alle erforderlichen Ordner Standbydatenbank wie C:\\\<YourLocalFolder\>\\testen. Sicherstellen Sie beim Durchführen dieser praktischen Einführung, dass die Ordnerstruktur die Ordnerstruktur auf Machine1 alle notwendigen Dateien wie Controlfile, Datendateien, Redologfiles, Udump, Bdump und Cdump zu entsprechen. Darüber hinaus definieren den ORACLE\_HOME und ORACLE\_Basis Umgebungsvariablen in Machine2. Wenn nicht, als einer Umgebungsvariablen im Dialogfeld Umgebungsvariablen definieren. Um auf dieses Dialogfeld zuzugreifen, starten Sie **das Programm** durch Doppelklicken auf das Symbol im **Bedienfeld**; dann klicken Sie auf die Registerkarte **Erweitert** , und wählen Sie **Umgebungsvariablen**. Um die Umgebungsvariablen klicken Sie auf die Schaltfläche **neu** unter **Systemvariablen**. Nach dem Einrichten der Umgebungsvariablen müssen Sie der Windows-Befehlszeile zu schließen und eine neue Änderungen an.

Gehen Sie anschließend folgendermaßen vor:

1. Parameter Initialisierungsdatei für Standbydatenbank vorbereiten

2. Konfigurieren Sie die Listener und Tnsnames die Datenbank auf primäre und Standby-Unterstützung

    1. Konfigurieren Sie listener.ora auf beiden Servern Einträge für beide Datenbanken

    2. Konfigurieren von tnsnames.ora auf den primären und den Standbycomputer virtuellen Computer Einträge für primäre und standby-Datenbanken

    3. Starten Sie den Listener und überprüfen Sie Tnsping auf beiden virtuellen Computern an beide Dienste.

3. Starten Sie die Standby-Instanz in Nomount Zustand

4. Verwenden Sie RMAN der Datenbank und erstellen eine Standby-Datenbank

5. Starten Sie physische Standbydatenbank im verwalteten Wiederherstellungsmodus

6. Überprüfen Sie die physische Standbydatenbank

### <a name="1-prepare-an-initialization-parameter-file-for-standby-database"></a>1. Parameter Initialisierungsdatei für Standbydatenbank vorbereiten

Dieser Abschnitt veranschaulicht die Parameter Initialisierungsdatei für die Standbydatenbank vorbereiten. Dazu kopieren Sie zuerst die INITTEST. ORA Datei von Computer 1 an Machine2. Sie sollte die INITTEST angezeigt. In der ORACLE ORA-Datei\_HOME %\\Ordner auf beiden Computern. Ändern Sie die Datei INITTEST.ora in Machine2 festgelegt für die Standby-Rolle wie unten angegeben:

    db_name='TEST'
    db_unique_name='TEST_STBY'
    db_create_file_dest='c:\OracleDatabase\oradata\test_stby’
    db_file_name_convert=’TEST’,’TEST_STBY’
    log_file_name_convert='TEST','TEST_STBY'


    job_queue_processes=10
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1='LOCATION=c:\OracleDatabase\TEST_STBY\archives VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=’TEST'
    LOG_ARCHIVE_DEST_2='SERVICE=TEST LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
    LOG_ARCHIVE_DEST_STATE_1='ENABLE'
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
    LOG_ARCHIVE_FORMAT='%t_%s_%r.arc'
    LOG_ARCHIVE_MAX_PROCESSES=30


Der vorherige Anweisungsblock gehören zwei wichtig:

-   **\*. LOG_ARCHIVE_DEST_1:** müssen Sie den Ordner c:\OracleDatabase\TEST_STBY\archives in Machine2 manuell erstellen.
-   **\*. LOG_ARCHIVE_DEST_2:** Dies ist optional. Diese Einstellung wird bei Bedarf möglicherweise, wenn der primäre Computer Wartung und Standby-Computer eine primäre Datenbank wird.

Dann müssen Sie die standby-Instanz starten. Geben Sie den folgenden Befehl in einer Windows-Befehlszeile Oracle-Instanz erstellen, erstellen Sie einen Windows-Dienst auf Standby-Datenbankserver:

    oradim -NEW -SID TEST\_STBY -STARTMODE MANUAL

**Oradim** -Befehl erstellt eine Oracle-Instanz jedoch nicht gestartet. Sie finden sie in C:\\OracleDatabase\\Produkt\\11.2.0\\Dbhome\_1\\BIN-Verzeichnis.

##<a name="configure-the-listener-and-tnsnames-to-support-the-database-on-primary-and-standby-machines"></a>Konfigurieren Sie die Listener und Tnsnames die Datenbank auf primäre und Standby-Unterstützung
Bevor Sie eine Standbydatenbank erstellen, müssen Sie sicherstellen, dass die primären und standby-Datenbanken in Ihrer Konfiguration miteinander kommunizieren können. Hierzu müssen Sie den Listener und die TNSNames manuell oder mithilfe von Netzwerk-Konfigurationsprogramm NETCA konfigurieren. Dies ist eine obligatorische Aufgabe bei Verwendung des Dienstprogramms Recovery Manager (RMAN).

### <a name="configure-listenerora-on-both-servers-to-hold-entries-for-both-databases"></a>Konfigurieren Sie listener.ora auf beiden Servern Einträge für beide Datenbanken

Remotedesktop Machine1 und bearbeiten die Datei listener.ora als unten angegeben. Wenn die Datei Liste-Achten Sie öffnende und schließende Klammer in derselben Spalte ausgerichtet. Finden Sie die Datei listener.ora im folgenden Ordner: c:\\OracleDatabase\\Produkt\\11.2.0\\Dbhome\_1\\Netzwerk\\ADMIN\\.

    # listener.ora Network Configuration File: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )

Nächste, remote Desktop Machine2 und die listener.ora-Datei folgendermaßen bearbeiten: # Liste-Network Configuration File: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test_stby)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )


### <a name="configure-tnsnamesora-on-the-primary-and-standby-virtual-machines-to-hold-entries-for-both-primary-and-standby-databases"></a>Konfigurieren von tnsnames.ora auf den primären und den Standbycomputer virtuellen Computer Einträge für primäre und standby-Datenbanken

Remotedesktop Machine1 und bearbeiten die Datei tnsnames.ora wie unten angegeben. Finden Sie die Datei tnsnames.ora im folgenden Ordner: c:\\OracleDatabase\\Produkt\\11.2.0\\Dbhome\_1\\Netzwerk\\ADMIN\\.

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )

Remotedesktop Machine2 und Bearbeiten der tnsnames.ora wie folgt:

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )


### <a name="start-the-listener-and-check-tnsping-on-both-virtual-machines-to-both-services"></a>Starten Sie den Listener und überprüfen Sie Tnsping auf beiden virtuellen Computern an beide Dienste.

Öffnen Sie eine neue Windows-Befehlszeile in primäre und standby virtuellen Maschinen und führen Sie die folgende Anweisung aus:

    C:\Users\DBAdmin>tnsping test

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:08
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test)))
    OK (0 msec)


    C:\Users\DBAdmin>tnsping test_stby

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:16
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test_stby)))
    OK (260 msec)


##<a name="start-up-the-standby-instance-in-nomount-state"></a>Starten Sie die Standby-Instanz in Nomount Zustand
Zum Einrichten der Umgebung die Standbydatenbank auf Standby-VM (MACHINE2) unterstützen.

Zunächst kopieren Sie die Passwort-Datei auf dem primären Computer (Machine1) Standby-Computer (Machine2) manuell. Dies ist erforderlich, da das **Sys** -Kennwort auf beiden Computern identisch sein muss.

Dann öffnen Sie die Windows-Befehlszeile in Machine2 und setup Umgebungsvariablen Standby-Datenbank wie folgt auf:

    SET ORACLE_HOME=C:\OracleDatabase\product\11.2.0\dbhome_1
    SET ORACLE_SID=TEST_STBY

Anschließend starten Sie die Standby-Datenbank in Nomount Zustand und generieren Sie einer spfileverknüpft.

Starten Sie die Datenbank:

    SQL>shutdown immediate;

    SQL>startup nomount
    ORACLE instance started.

    Total System Global Area  747417600 bytes
    Fixed Size                  2179496 bytes
    Variable Size             473960024 bytes
    Database Buffers          264241152 bytes
    Redo Buffers                7036928 bytes


##<a name="use-rman-to-clone-the-database-and-to-create-a-standby-database"></a>Verwenden Sie RMAN der Datenbank und erstellen eine Standby-Datenbank
Das Dienstprogramm Recovery Manager (RMAN) können Sicherungskopie der primären Datenbank zu physischen Standbydatenbank zu.

Remotedesktop auf Standby-VM (MACHINE2) und RMAN-Dienstprogramm ausführen, indem Sie eine vollständige Verbindungszeichenfolge angeben Zeichenfolge für beide das Ziel (primäre Datenbank Machine1) und ZUSÄTZLICHES (Standbydatenbank, Machine2) Instanzen.

>[AZURE.IMPORTANT] Verwenden Sie Betriebssystem-Authentifizierung nicht, wie es keine Datenbank im standby-Server-Computer noch.

    C:\> RMAN TARGET sys/password@test AUXILIARY sys/password@test_STBY

    RMAN>DUPLICATE TARGET DATABASE
      FOR STANDBY
      FROM ACTIVE DATABASE
      DORECOVER
        NOFILENAMECHECK;

##<a name="start-the-physical-standby-database-in-managed-recovery-mode"></a>Starten Sie physische Standbydatenbank im verwalteten Wiederherstellungsmodus
In diesem Lernprogramm veranschaulicht erstellen Sie eine physische Standbydatenbank. Informationen zum Erstellen einer logischen Standbydatenbank finden Sie in der Oracle-Dokumentation.

Öffnen Sie SQL\*Befehlszeile und Data Guard auf standby virtuellen Computer oder Server (MACHINE2) wie folgt aktivieren:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

Beim Öffnen der Standbydatenbank im Modus **Bereitstellen** , das Archivierungsprotokoll Versand und verwaltete Wiederherstellungsprozess auf Standby-Datenbank protokollieren. Dies gewährleistet, dass die Standbydatenbank mit der primären Datenbank bleibt. Beachten Sie, dass die Standbydatenbank für Berichtszwecke während dieser Zeit zugegriffen werden kann.

Wenn die Standbydatenbank im **Schreibgeschützt** -Modus öffnen, das Archiv Protokollversand fortgesetzt. Allerdings verwaltete Wiederherstellungsprozess. Dadurch wird die Standbydatenbank zunehmend veraltet bis verwalteten Recovery-Prozess fortgesetzt wird. Sie können die Standbydatenbank um zu Berichtszwecken während dieser Zeit aber Daten entsprechen möglicherweise nicht den letzten Änderung.

Es wird empfohlen, die Standbydatenbank im Modus **Bereitstellen** , um Daten in die Standbydatenbank Stand bei einem Ausfall der primären Datenbank beizubehalten. Allerdings können Sie die Standbydatenbank im **Schreibgeschützt** -Modus um zu Berichtszwecken je nach der Anwendung belassen. Die folgenden Schritte zeigen, wie Data Guard im schreibgeschützten Modus mit SQL aktiviert\*Plus:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE OPEN READ ONLY;


##<a name="verify-the-physical-standby-database"></a>Überprüfen Sie die physische Standbydatenbank
Dieser Abschnitt zeigt die Konfiguration mit hoher Verfügbarkeit als Administrator überprüfen.

Öffnen Sie SQL\*Plus Eingabeaufforderungsfenster und Kontrollkästchen archiviert redo Log unter Standby Virtual Machine (Machine2):

    SQL> show parameters db_unique_name;

    NAME                                TYPE       VALUE
    ------------------------------------ ----------- ------------------------------
    db_unique_name                      string     TEST_STBY

    SQL> SELECT NAME FROM V$DATABASE

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    45                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   NO
    47                    23-FEB-14   23-FEB-14   NO

Öffnen Sie SQL * Plus Befehl Fenster und Switch-Protokolldateien auf dem primären Computer (Machine1):

    SQL> alter system switch logfile;
    System altered.

    SQL> archive log list
    Database log mode              Archive Mode
    Automatic archival             Enabled
    Archive destination            C:\OracleDatabase\archive
    Oldest online log sequence     69
    Next log sequence to archive   71
    Current log sequence           71

Überprüfen Sie archivierte Wiederholungsprotokoll unter Standby Virtual Machine (Machine2):

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   YES
    48                    23-FEB-14   23-FEB-14   YES

    49                    23-FEB-14   23-FEB-14   YES
    50                    23-FEB-14   23-FEB-14   IN-MEMORY

Ermitteln von Lücken in den Standbymodus virtuellen Computer (Machine2):

    SQL> SELECT * FROM V$ARCHIVE_GAP;
    no rows selected.

Eine andere Verifizierungsmethode konnte Failover auf Standby-Datenbank werden und wenn man Failback zum primären Datenbank testen. Aktivieren Sie die Standby-Datenbank als primäre Datenbank verwenden Sie Folgendes:

    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH;
    SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;

Rückblick auf die ursprüngliche primäre Datenbank nicht aktiviert haben, empfiehlt es sich, die ursprüngliche primäre Datenbank und als Standby-Datenbank neu erstellen.

Wir empfehlen Flashback-Datenbank auf dem Primärserver und Standby-Datenbanken ermöglichen. Fall ein Failover die primäre Datenbank zurück in die Zeit vor dem Failover aktualisiert und schnell in eine Standbydatenbank konvertiert.

##<a name="additional-resources"></a>Zusätzliche Ressourcen
[Oracle virtuellen Computerimages für Azure](virtual-machines-windows-classic-oracle-images.md)
