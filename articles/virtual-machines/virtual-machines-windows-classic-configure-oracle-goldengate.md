<properties
    pageTitle="Konfigurieren von Oracle Golden Gate in VMs | Microsoft Azure"
    description="Schrittweise Anleitung zur Einrichtung und Implementierung von Oracle Golden Gate auf Azure Windows Server virtueller Computer für hohe Verfügbarkeit und Disaster Recovery."
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


#<a name="configuring-oracle-goldengate-for-azure"></a>Konfigurieren von Oracle Golden Gate für Azure


Dieses Lernprogramm demonstriert die Einrichtung von Oracle Golden Gate Umgebung Azure virtuelle Computer für hohe Verfügbarkeit und Disaster Recovery. Das Lernprogramm konzentriert sich auf [bidirektionale Replikation](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_about_gg.htm) für RAC Oracle-Datenbanken und erfordert, dass beide Seiten aktiv sind.

Golden Gate Oracle unterstützt die Verteilung und Datenintegration. Es ermöglicht eine Verteilung und Daten über die Replikationskonfiguration Oracle Oracle Lösung und bietet eine flexible Hochverfügbarkeits-Lösung. Oracle Golden Gate ergänzt Oracle Data Guard mit der Replikationsfunktionen unternehmensweiten Verteilung und unterbrechungsfreie Upgrades und Migrationen. Ausführliche Informationen finden Sie unter [Verwenden des Oracle Golden Gate Oracle Data Guard](http://docs.oracle.com/cd/E11882_01/server.112/e17157/unplanned.htm).

Oracle Golden Gate enthält folgenden Hauptkomponenten: Datapump, Replicat, Wege zu extrahieren oder Prüfpunkte, Manager und Collector Dateien zu extrahieren. Um bidirektionale Replikation zwischen zwei Standorten müssen Sie zum Erstellen und starten alle Komponenten auf beiden Seiten. Detaillierte Informationen zur Oracle Golden Gate-Architektur finden Sie unter [Oracle Golden Gate Guide](http://docs.oracle.com/goldengate/1212/gg-winux/index.html).

Diese praktische Einführung geht schon theoretischen und praktischen Kenntnisse auf Oracle Datenbank mit hoher Verfügbarkeit und Disaster Recovery sowie [Oracle Golden Gate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html). Weitere Informationen finden Sie in der [Oracle-Website](http://www.oracle.com/technetwork/database/features/availability/index.html).

Das Lernprogramm vorausgesetzt darüber hinaus die folgenden erforderlichen Komponenten bereits implementiert haben:

- Sie haben bereits hohe Verfügbarkeit und Disaster Recovery Aspekte Abschnitt im Thema [Oracle virtuellen Computerimages - verschiedene Aspekte](virtual-machines-windows-classic-oracle-considerations.md) überprüft. Beachten Sie, dass derzeit unterstützt eigenständige Oracle-Datenbankinstanzen jedoch nicht Oracle Real Application Clusters (Oracle RAC) in Azure.

- Oracle Golden Gate Software haben [Oracle Downloads](http://www.oracle.com/us/downloads/index.html) -Website heruntergeladen werden. Sie haben das Produkt Pack Oracle Fusion Middleware-Datenintegration ausgewählt. Dann haben Sie Oracle Golden Gate Oracle v11.2.1 Media Pack für Windows (64 Bit) X64 für eine Oracle Database 11 g aktiviert. Als Nächstes herunterladen Sie Oracle Golden Gate V11.2.1.0.3 für Oracle 11g 64-Bit Windows 2008 (64 Bit)

- Sie haben zwei virtuellen Maschinen (VMs) in Azure mit Oracle Enterprise Edition auf Windows Server. Stellen Sie sicher, dass die virtuellen Computer im [selben Cloud-Dienst](virtual-machines-linux-load-balance.md) und im gleichen [Virtuellen Netzwerk](https://azure.microsoft.com/documentation/services/virtual-network/) um sicherzustellen, dass sie einander über den permanenten privaten IP-Adresse zugegriffen werden können.

- Sie haben die virtuellen Namen als "MachineGG1" für Standort A und Standort b "MachineGG2" klassischen Azure-Portal festgesetzt.

- Testdatenbanken erstellte "TestGG1" "TestGG2" auf Standort b zu Standort A

- Anmeldung auf dem WindowsServer als Mitglied der Administratorengruppe oder ein Mitglied der Gruppe **ORA_DBA** .

In diesem Lernprogramm werden Sie:

1. Datenbank für Standort A und Standort B einrichten  

    1. Anfängliche Daten ausführen

2. Standort A und Standort B für die Replikation vorbereiten

3. Erstellen Sie alle erforderlichen Objekte unterstützen DDL-Replikation

4. Konfigurieren Sie Golden Gate-Manager, Standort A und Standort B

5. Extrahieren Sie Gruppe erstellen und Datapump Prozesse auf Standort A und Standort B

    1. Erstellen Sie extrahieren und Datapump Prozesse auf Standort A

    2. Erstellen einer Tabelle Golden Gate Checkpoint an Standort B

    3. Hinzufügen von REPLICAT für Standort B

    4. Erstellen Sie extrahieren und Datapump Prozesse auf Standort B

    5. Erstellen Sie eine Golden Gate Checkpoint-Tabelle für Standort A

    6. Hinzufügen von REPLICAT für Standort A

    7. Hinzufügen von Trandata für Standort A und Standort B

    8. Starten Sie extrahieren und Datapump Prozesse auf Standort A

    9. Starten Sie extrahieren und Datapump Prozesse auf Standort B

    10. Starten des REPLICAT für Standort A

    11. Starten Sie REPLICAT an Standort B

6. Bidirektionale Replikationsprozess überprüfen

>[AZURE.IMPORTANT] In diesem Lernprogramm wurde und mit der folgenden Softwarekonfiguration getestet:
>
>|                        | **Eine Datenbank**              | **Standort B-Datenbank**              |
>|------------------------|----------------------------------|----------------------------------|
>| **Oracle-Version**     | Oracle11g Release 2 (11.2.0.1) | Oracle11g Release 2 (11.2.0.1) |
>| **Name des Computers**       | MachineGG1                       | MachineGG2                       |
>| **Betriebssystem**   | Windows 2008 R2                  | Windows 2008 R2                  |
>| **Oracle SID**         | TESTGG1                          | TESTGG2                          |
>| **Replikation Schema** | SCOTT                            | SCOTT                            |

Für nachfolgende Versionen von Oracle Database und Oracle Golden Gate möglicherweise einige weiteren Änderungen, die Sie implementieren müssen. Die aktuelle Version bestimmte Informationen Dokumentation [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) und [Oracle-Datenbank](http://www.oracle.com/us/corporate/features/database-12c/index.html) auf Oracle-Website. Angenommen, eine Quelldatenbank Version 11.2.0.4 und höher Erfassung von DDL asynchron vom Server Logmining ausgeführt und erfordert keine spezielle Trigger, Tabellen oder andere Datenbankobjekte installiert werden. Oracle Golden Gate Upgrades können durchgeführt werden, ohne Benutzer-Applikationen. Die Verwendung eines DDL-Triggers und Objekte ist beim Extrahieren im integrierten Modus mit einer Quelldatenbank Oracle 11 g ist älter als Version 11.2.0.4 erforderlich. Detaillierte Hinweise finden Sie unter [Installieren und Konfigurieren von Oracle GoldenGate für Oracle-Datenbanken](http://docs.oracle.com/goldengate/1212/gg-winux/GIORA.pdf).

##<a name="1-setup-database-on-site-a-and-site-b"></a>1. setup Datenbank auf Standort A und Standort B
Dieser Abschnitt erklärt das erforderliche Datenbank für Standort A und Standort b ausführen Sie müssen alle Schritte dieses Abschnitts auf beiden Seiten: Standort A und Standort b

Erstens remote Desktop an Standort A und Standort B über klassischen Azure-Portal. Öffnen einer Windows-Befehlszeile und ein Basisverzeichnis für Oracle Golden Gate Setup-Dateien erstellen:

    mkdir C:\OracleGG

Dann extrahieren Sie und installieren Sie die Oracle Golden Gate Software in diesem Ordner. Nach diesem Schritt können Sie Golden Gate Software Command Interpreter (GGSCI) initiieren, indem Sie den folgenden Befehl ausführen:

    C:\OracleGG\.\ggsci

Sie können [GGSCI](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_gettingstarted.htm) , mehrere Befehle auszuführen, die konfigurieren, steuern und überwachen Oracle Golden Gate.

Führen Sie dann den folgenden Befehl alle Unterordner aus dem Installationspaket zu erstellen:

    GGSCI (Hostname) 1> CREATE SUBDIRS

Führen Sie den folgenden Befehl an der Befehlszeile GGSCI beenden:

    GGSCI (Hostname) 1> EXIT

Dann müssen Sie einen Datenbankbenutzer erstellen, die von Oracle Golden Gate Manager, extrahieren und Replikation Prozesse verwendet werden. Beachten Sie, dass einzelne Benutzer für jeden Prozess erstellen oder nur eine allgemeinen Benutzer konfigurieren. In diesem Lernprogramm erstellen wir einen Benutzer als Ggate bezeichnet. Dann wir dieser Benutzer die erforderlichen Berechtigungen gewähren. Beachten Sie, dass die folgenden Operationen für Standort A und Standort b ausführen müssen

Öffnen Sie SQL\*Plus Befehlsfenster mit Datenbankadministratorrechten mit **SYSDBA**auf Standort A und Standort B:

    Enter username: / as sysdba

Und:

    SQL> create tablespace ggs_data   datafile 'c:\OracleDatabase\oradata\<DBNAME>\<DBNAME>ggs_data01.dbf' size 200m;
    SQL> create user ggate identified by ggate default tablespace ggs_data  temporary tablespace temp;
          grant connect, resource to ggate;
          grant select any dictionary, select any table to ggate;
          grant create table to ggate;
          grant flashback any table to ggate;
          grant execute on dbms_flashback to ggate;
          grant execute on utl_file to ggate;
          grant create any table to ggate;
          grant insert any table to ggate;
          grant update any table to ggate;
          grant delete any table to ggate;
          grant drop any table to ggate;

Als Nächstes suchen Sie INIT\<DatabaseSID\>. In der ORACLE ORA-Datei\_HOME %\\Datenbank auf Standort A und Standort B und INITTEST.ora die folgenden Datenbankparameter hinzufügen:

    UNDO\_MANAGEMENT=AUTO
    UNDO\_RETENTION=86400

Eine vollständige Liste aller Oracle Golden Gate GGSCI Befehle finden Sie unter [Referenz für Oracle Golden Gate für Windows](http://docs.oracle.com/goldengate/1212/gg-winux/GWURF/ggsci_commands.htm).

### <a name="perform-initial-data-load"></a>Anfängliche Daten ausführen

Mehrere Methoden können Sie die Anfangsdaten Last in der Datenbank ausführen. Z. B. können [Oracle Golden Gate direkt laden](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_initsync.htm) oder regulären Export und Import-Dienstprogramme Sie Daten von Standort A an Standort b exportieren

Demonstrieren Sie den Replikationsprozess Oracle Golden Gate veranschaulicht dieses Lernprogramm Erstellen einer Tabelle für Standort A und Standort B mit den folgenden Befehlen.

Zunächst öffnen Sie SQL\*Befehlsfenster, und führen Sie folgenden Befehl ein Lager für Standort A und Standort B Datenbanken erstellen:

    create table scott.inventory
    (prod_id number,
    prod_category varchar2(20),
    qty_in_stock number,
    last_dml timestamp default systimestamp);

Fügen Sie eine Einschränkung auf Standort A und Standort B Datenbanken neu erstellte Tabelle:

    alter table scott.inventory add constraint pk_inventory primary key (prod_id) ;

Anschließend erteilen Sie alle Rechte in die neue Lagertabelle Benutzer Ggate für Standort A und Standort b

    grant all on scott.inventory to ggate;

Anschließend erstellen Sie und aktivieren Sie einen Datenbank-Trigger INVENTORY_CDR_TRG, auf die neu erstellte Tabelle um sicherzustellen, dass alle Transaktionen in die neue Tabelle aufgezeichnet werden, wenn der Benutzer nicht Ggate ist. Führen Sie diesen Vorgang für Standort A und Standort b

    CREATE OR REPLACE TRIGGER INVENTORY_CDR_TRG
    BEFORE UPDATE
    ON SCOTT.INVENTORY
    REFERENCING NEW AS New OLD AS Old
    FOR EACH ROW
    BEGIN
    IF SYS_CONTEXT ('USERENV', 'SESSION_USER') != 'GGATE'
    THEN
    :NEW.LAST_DML := SYSTIMESTAMP;
    END IF;
    END;
    /


##<a name="2-prepare-site-a-and-site-b-for-database-replication"></a>2. Vorbereitung Standort A und Standort B Datenbankreplikation
Dieser Abschnitt erläutert die Replikation Standort A und Standort B vorbereiten. Sie müssen alle Schritte dieses Abschnitts auf beiden Seiten: Standort A und Standort b

Erstens remote Desktop an Standort A und Standort B über klassischen Azure-Portal. Ändern Sie die Datenbank Archivelog Modus die SQL * Plus Befehlsfenster:

    sql>shutdown immediate
    sql>startup mount
    sql>alter database archivelog;
    sql>alter database open;


Aktivieren Sie minimale [zusätzliche Protokollierung](http://docs.oracle.com/cd/E11882_01/server.112/e22490/logminer.htm) dann wie folgt:

    SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

Die Datenbank unterstützt DDL (Data Definition Language)-Replikation vorbereitet:

    SQL> alter system set recyclebin=off scope=spfile;

Dann Herunterfahren und Neustart der Datenbank:

    sql>shutdown immediate
    sql>startup


##<a name="3-create-all-necessary-objects-to-support-ddl-replication"></a>3. erstellen Sie alle erforderlichen Objekte unterstützen DDL-Replikation
Dieser Abschnitt listet die Skripts Sie alle erforderlichen Objekte unterstützen DDL-Replikation erstellen müssen. In diesem Abschnitt für Standort A und Standort b angegebenen Skripts ausgeführt werden sollen

Öffnen einer Windows-Befehlszeile, und navigieren Sie zum Ordner Oracle Golden Gate wie C:\\OracleGG. Starten Sie SQL\*Plus mit Datenbankadministratorrechten wie **SYSDBA** auf Standort A und Standort b

Führen Sie die folgenden Skripts:

    SQL> @marker_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @ddl_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @role_setup.sql
    Enter GoldenGate schema name: ggate
    SQL> grant ggs_ggsuser_role to ggate;
     Grant succeeded.
     SQL> @ddl_enable
     Trigger altered.
     SQL> @ddl_pin ggate


Oracle Golden Gate Tool erfordert eine Tabelle auf Anmeldung für DDL (Data Definition Language)-Unterstützung. Deshalb zusätzliche Protokollierung auf den Befehl ADD-TRANDATA aktivieren. Oracle Golden Gate-Interpreter Befehlsfenster Login, Datenbank, öffnen Sie, und führen Sie den Befehl ADD-TRANDATA:

    GGSCI 5> DBLOGIN USERID ggate, PASSWORD ggate

    GGSCI(Hostname) 6> add trandata scott.inventory

##<a name="4-configure-goldengate-manager-on-site-a-and-site-b"></a>4. konfigurieren Sie Golden Gate-Manager für Standort A und Standort B
Oracle Golden Gate-Manager führt eine Reihe von Funktionen wie die anderen Golden Gate Prozesse starten Trail Protokolldatei-Management und reporting.

Sie müssen den Oracle Golden Gate-Manager-Prozess auf Standort A und Standort b Hierzu gehen Sie am Standort A und Standort b

Fenster Befehlsfenster und Oracle Golden Gate-Befehlsinterpreter initiieren:

    cd C:\OracleGG\
    c:\OracleGG>ggsci
    Oracle GoldenGate Command Interpreter for Oracle
    Version 11.2.1.0.3 14400833 OGGCORE_11.2.1.0.3_PLATFORMS_120823.1258
    Windows x64 (optimized), Oracle 11g on Aug 23 2012 16:50:36
    Copyright (C) 1995, 2012, Oracle and/or its affiliates. All rights reserved.


Protokolliert die GGSCI Sitzung in eine Datenbank, damit Sie Befehle ausführen können, die die Datenbank:

    GGSCI (HostName) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Anzeigen des Status und Verzögerung (gegebenenfalls) für alle Manager extrahieren und Replicat Prozesse in einem System:

    GGSCI (HostName) 2> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Öffnen Sie Parameter mit dem Befehl Parameter bearbeiten, und fügen Sie die folgende Informationen:

    GGSCI (HostName) 3> edit params mgr
    PORT 7809
    USERID ggate, PASSWORD ggate
    PURGEOLDEXTRACTS  C:\OracleGG\dirdat\ex, USECHECKPOINTS

Anzeigen des Status und Verzögerung (gegebenenfalls) für alle Manager extrahieren und Replicat Prozesse in einem System:

    GGSCI (HostName) 46> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Protokolliert die GGSCI Sitzung in eine Datenbank, damit Sie Befehle ausführen können, die die Datenbank:

    GGSCI (HostName) 47> dblogin USERID ggate, PASSWORD ggate

    Successfully logged into database.

Start-Manager-Prozess:

    GGSCI (HostName) 48> start manager
    Manager started.

##<a name="5-create-extract-group-and-data-pump-processes-on-site-a-and-site-b"></a>5. erstellen Sie extrahieren und Datapump Prozesse auf Standort A und Standort B

###<a name="create-extract-and-data-pump-processes-on-site-a"></a>Erstellen Sie extrahieren und Datapump Prozesse auf Standort A

Sie müssen Prozesse extrahieren und Datapump für Standort A und Standort b Remotedesktop an Standort A und Standort B über klassischen Azure-Portal erstellen. GGSCI-Interpreter Befehlsfenster öffnen. Führen Sie folgende Befehle auf Website:

    GGSCI (MachineGG1) 14> add extract ext1 tranlog begin now
    EXTRACT added.
    GGSCI (MachineGG1) 4> add exttrail C:\OracleGG\dirdat\lt, extract ext1
    EXTTRAIL added.
    GGSCI (MachineGG1) 16> add extract dpump1 exttrailsource C:\OracleGG\dirdat\aa
    EXTRACT added.
    GGSCI (MachineGG1) 17> add rmttrail C:\OracleGG\dirdat\ab extract dpump1
    RMTTRAIL added.

Öffnen Sie die Parameterdatei mit dem Befehl Parameter bearbeiten und fügen Sie die folgende Informationen: GGSCI (MachineGG1) 18 > Bearbeiten Params ext1 EXTRACT ext1 USERID Ggate Kennwort Ggate EXTTRAIL C:\OracleGG\dirdat\aa TRANLOGOPTIONS EXCLUDEUSER Ggate Tabelle scott.inventory GETBEFORECOLS (ON UPDATE KEYINCLUDING (Prod_category, Qty_in_stock, Last_dml), auf Löschen KEYINCLUDING (Prod_category, Qty_in_stock, Last_dml))

Öffnen Sie Parameter mit dem Befehl Parameter bearbeiten, und fügen Sie die folgende Informationen:

    GGSCI (MachineGG1) 15> edit params dpump1
    EXTRACT dpump1
     USERID ggate, PASSWORD ggate
     RMTHOST ActiveGG2orcldb, MGRPORT 7809, TCPBUFSIZE 100000
     RMTTRAIL C:\OracleGG\dirdat\ab
     PASSTHRU
     TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-b"></a>Erstellen einer Tabelle Golden Gate Checkpoint an Standort B

Als Nächstes müssen Sie eine Checkpoint-Tabelle auf Standort b hinzufügen Dazu müssen Sie ein Golden Gate-Interpreter Befehlsfenster öffnen und ausführen:

    C:\OracleGG\ggsci
    GGSCI (MachineGG2) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Und fügen Sie die Checkpoint-Tabelle zur Datenbank, Ggate der Besitzer ist:

    GGSCI (MachineGG2) 2> ADD CHECKPOINTTABLE ggate.checkpointtable
    Successfully created checkpoint table ggate.checkpointtable.

GLOBALS-Datei auf dem Zielserver Standort B in diesem Schritt wird fügen Sie der Name der Tabelle das Kontrollkästchen hinzu. Bearbeiten Sie die Datei GLOBALS Standort b

    GGSCI (MachineGG2) 1> EDIT PARAMS ./GLOBALS

Vorhandene GLOBALS-Datei dann den CHECKPOINTTABLE-Parameter hinzufügen:

    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Im letzten Schritt speichern Sie und schließen Sie die Parameterdatei GLOBALS.


###<a name="add-replicat-on-site-b"></a>Hinzufügen von REPLICAT für Standort B
Dieser Abschnitt beschreibt das Hinzufügen ein REPLICAT Prozesses "REP2" auf Standort b

Befehl hinzufügen REPLICAT erstellen Replicat Gruppe auf Standort b

    GGSCI (MachineGG2) 37> add replicat rep2 exttrail C:\OracleGG\dirdatab, checkpointtable ggate.checkpointtable

Öffnen Sie Parameter mit dem Befehl Parameter bearbeiten, und fügen Sie die folgende Informationen:

    GGSCI (MachineGG2) 10> edit params rep2
    REPLICAT rep2
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append,megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

###<a name="create-extract-and-data-pump-processes-on-site-b"></a>Erstellen Sie extrahieren und Datapump Prozesse auf Standort B

Dieser Abschnitt beschreibt das Erstellen einer neuen extrahieren "EXT2" und eine neue Daten Pumpe Prozess "DPUMP2" auf Standort b

    GGSCI (MachineGG2) 3> add extract ext2 tranlog begin now
     EXTRACT added.
    GGSCI (MachineGG2) 4> add exttrail C:\OracleGG\dirdat\ac extract ext2
     EXTTRAIL added.
    GGSCI (MachineGG2) 5> add extract dpump2 exttrailsource C:\OracleGG\dirdat\ac
     EXTRACT added.
    GGSCI (MachineGG2) 6> add rmttrail C:\OracleGG\dirdat\ad extract dpump2
     RMTTRAIL added.

Öffnen Sie Parameter mit dem Befehl Parameter bearbeiten, und fügen Sie die folgende Informationen:

    GGSCI (MachineGG2) 31> edit params ext2
    EXTRACT ext2
    USERID ggate, PASSWORD ggate
    EXTTRAIL C:\OracleGG\dirdat\ac
    TRANLOGOPTIONS EXCLUDEUSER ggate
    TABLE scott.inventory,
    GETBEFORECOLS (
    ON UPDATE KEYINCLUDING (prod_category,qty_in_stock, last_dml),
    ON DELETE KEYINCLUDING (prod_category,qty_in_stock, last_dml));

Öffnen Sie Parameter mit dem Befehl Parameter bearbeiten, und fügen Sie die folgende Informationen:

    GGSCI (MachineGG2) 32> edit params dpump2
    EXTRACT dpump2
    USERID ggate, PASSWORD ggate
    RMTHOST MachineGG1, MGRPORT 7809, TCPBUFSIZE 100000
    RMTTRAIL C:\OracleGG\dirdat\ad
    PASSTHRU
    TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-a"></a>Erstellen Sie eine Golden Gate Checkpoint-Tabelle für Standort A

Oracle Golden Gate-Interpreter Befehlsfenster öffnen und Erstellen einer Checkpoint-Tabelle:

    GGSCI (MachineGG1) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

    GGSCI (MachineGG1) 2> ADD CHECKPOINTTABLE ggate.checkpointtable

    Successfully created checkpoint table ggate.checkpointtable.

Sie müssen auch die Globaldatei a den Namen der Tabelle das Kontrollkästchen hinzufügen

Oracle Golden Gate-Interpreter Befehlsfenster öffnen und Bearbeiten der GLOBALS-Datei auf Website:

    GGSCI (MachineGG1) 1> EDIT PARAMS ./GLOBALS
    Add the CHECKPOINTTABLE parameter to the existing GLOBALS file as follows:
    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Speichern Sie und schließen Sie die Parameterdatei GLOBALS.

###<a name="add-replicat-on-site-a"></a>Hinzufügen von REPLICAT für Standort A

Dieser Abschnitt beschreibt das Hinzufügen ein REPLICAT Prozesses "REP1" von Standort a

Der folgende Befehl erstellt eine rep1 Replicat Gruppe mit dem Namen einer Spur und zugeordneten Checkpointtable.

    GGSCI (MachineGG1) 21> add replicat rep1 exttrail C:\OracleGG\dirdat\ad,checkpointtable ggate.checkpointtable
     REPLICAT added.

Öffnen Sie Parameter mit dem Befehl Parameter bearbeiten, und fügen Sie die folgende Informationen:

    GGSCI (MachineGG1) 10> edit params rep1
    REPLICAT rep1
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append, megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

### <a name="add-trandata-on-site-a-and-site-b"></a>Hinzufügen von Trandata für Standort A und Standort B

Protokollierung zusätzliche auf den Befehl hinzufügen TRANDATA. Öffnen Sie Oracle Golden Gate-Interpreter Befehlsfenster Login, Datenbank, und führen Sie den Befehl hinzufügen TRANDATA.

Remote Desktop MachineGG1, Oracle Golden Gate Befehlsinterpreter öffnen und ausführen:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Remote Desktop MachineGG2, Oracle Golden Gate Befehlsinterpreter öffnen und ausführen:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.

Informationen über den Zustand auf zusätzliche Protokollierung:

    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="add-trandata-on-site-a-and-site-b"></a>Hinzufügen von Trandata für Standort A und Standort B

Protokollierung zusätzliche auf den Befehl hinzufügen TRANDATA. Öffnen Sie Oracle Golden Gate-Interpreter Befehlsfenster Login, Datenbank, und führen Sie den Befehl hinzufügen TRANDATA.

Remote Desktop MachineGG1, Oracle Golden Gate Befehlsinterpreter öffnen und ausführen:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Remote Desktop MachineGG2, Oracle Golden Gate Befehlsinterpreter öffnen und ausführen:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.
    Display information about the state of table-level supplemental logging:
    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="start-extract-and-data-pump-processes-on-site-a"></a>Starten Sie extrahieren und Datapump Prozesse auf Standort A

Starten Sie ext1 Prozess extrahieren auf Website:

    GGSCI (MachineGG1) 31> start extract ext1
    Sending START request to MANAGER …
    EXTRACT EXT1 starting

Starten Sie Datapump Prozess dpump1 auf Site:

    GGSCI (MachineGG1) 23> start extract dpump1
    Sending START request to MANAGER …
    EXTRACT DPUMP1 starting
Anzeigen von Informationen zu ext1 Gruppe extrahieren: GGSCI (MachineGG1) 32 > Info 2013-11-25 extrahieren ext1 ist EXT1 EXTRAHIEREN zuletzt gestartet 08:03 Status ausgeführt Checkpoint Lag 00:00:00 (aktualisierte 00:00:02 vor) Protokoll lesen Checkpoint Oracle wiederholen Protokolle 2013-11-25 08:03:18 Sequenz 6 RBA 3230720 SCN 0.1074371 (1074371)

Anzeigen des Status und Verzögerung (gegebenenfalls) für alle Manager extrahieren und Replicat Prozesse in einem System:

    GGSCI (MachineGG1) 16> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP1      00:00:00      00:46:33
    EXTRACT     RUNNING     EXT1        00:00:00      00:00:04

###<a name="start-extract-and-data-pump-processes-on-site-b"></a>Starten Sie extrahieren und Datapump Prozesse auf Standort B

Starten Sie extrahieren Prozess ext2 auf Standort b

    GGSCI (MachineGG2) 22> start extract ext2
    Sending START request to MANAGER …
    EXTRACT EXT2 starting

Starten Sie Datapump Prozess dpump2 auf Standort b

    GGSCI (MachineGG2) 23> start extract dpump2
    Sending START request to MANAGER …
    EXTRACT DPUMP2 starting

Anzeigen des Status und Verzögerung (gegebenenfalls) für alle Manager extrahieren und Replicat Prozesse in einem System:

    GGSCI (ActiveGG2orcldb) 6> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP2      00:00:00      136:13:33
    EXTRACT     RUNNING     EXT2        00:00:00      00:00:04

### <a name="start-replicat-process-on-site-a"></a>Starten des REPLICAT für Standort A

Dieser Abschnitt beschreibt, wie der REPLICAT "REP1" von Standort a beginnen

Auf Website: Replicat starten

    GGSCI (MachineGG1) 38> start replicat rep1
    Sending START request to MANAGER …
    REPLICAT REP1 starting

Der Status der Replicat-Gruppe:

    GGSCI (MachineGG1) 39> status replicat rep1
     REPLICAT REP1: RUNNING

###<a name="start-replicat-process-on-site-b"></a>Starten Sie REPLICAT an Standort B

Dieser Abschnitt beschreibt, wie der REPLICAT "REP2" auf Standort b beginnen

Auf Standort b Replicat starten

    GGSCI (MachineGG2) 26> start replicat rep2
    Sending START request to MANAGER …
    REPLICAT REP2 starting

Der Status der Replicat-Gruppe:

    GGSCI (MachineGG2) 27> status replicat rep2
    REPLICAT REP2: RUNNING

##<a name="6-verify-the-bi-directional-replication-process"></a>6. Überprüfen Sie bidirektionale Replikationsprozess

Überprüfen Sie die Konfiguration Oracle Golden Gate fügen Sie eine Zeile in die Datenbank am Standort a Remote Desktop Website A. Öffnen Sie SQL * Befehlsfenster und ausführen: SQL > select Name V$ Datenbank;

    NAME
    ———
    TESTGG

    SQL> insert into inventory values  (100,’TV’,100,sysdate);

    1 row created.

    SQL> commit;

    Commit complete.

Dann überprüfen Sie, ob diese Zeile auf Standort b repliziert werden Zu diesem Zweck Remotedesktop auf Standort b öffnen Fenster SQL Plus einrichten und ausführen:

    SQL> select name from v$database;

    NAME
    ———
    TESTGG

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13

Einfügen eines neuen Datensatzes am Standort b

    SQL> insert into inventory  values  (101,’DVD’,10,sysdate);
    1 row created.

    SQL> commit;

    Commit complete.

Remotedesktop-Website ein und überprüfen Sie, ob die Replikation stattgefunden hat:

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13
    101 DVD 10 22-MAR-13

##<a name="additional-resources"></a>Zusätzliche Ressourcen
[Oracle virtuellen Computerimages für Azure](virtual-machines-linux-classic-oracle-images.md)
