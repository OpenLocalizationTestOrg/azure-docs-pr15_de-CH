<properties
    pageTitle="MariaDB (MySQL) Cluster ausgeführte auf Windows Azure"
    description="Erstellen einer MariaDB + Galera MySQL auf Azure virtuelle cluster-Maschinen"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="sabbour"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="04/15/2015"
    ms.author="v-ahsab"/>

# <a name="mariadb-mysql-cluster---azure-tutorial"></a>MariaDB (MySQL) Cluster - Azure-Lernprogramm

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

> [AZURE.NOTE]  MariaDB Enterprise Cluster steht in Azure Marketplace.  Das neue Angebot wird automatisch einen MariaDB Galera Cluster auf ARM bereitgestellt. Verwenden Sie das neue Angebot von https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/ 

Wir erstellen einen Multi-Master [Galera](http://galeracluster.com/products/) Cluster [MariaDBs](https://mariadb.org/en/about/), stabile, skalierbare und zuverlässige Ersatz für MySQL in einer hochgradig verfügbaren Umgebung auf Azure Virtual Machines arbeiten.

## <a name="architecture-overview"></a>Übersicht über die Architektur

Dieses Thema führt die folgenden Schritte aus:

1. Erstellen Sie einen Cluster mit 3 Knoten
2. Trennen Sie den Datenträger vom Betriebssystem-Datenträger
3. Erstellen Sie den Datenträger in RAID-0/Striping Einstellung IOPS erhöhen
4. Verwenden der Azure-Lastenausgleich Lastenausgleich für 3 Knoten
5. Minimieren Sie Routinearbeiten ein VM-Image mit MariaDB + Galera erstellen und anderen Cluster virtuellen Computer erstellen.

![Architektur](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Setup.png)

> [AZURE.NOTE]  In diesem Thema wird der [Azure-CLI](../xplat-cli-install.md) -Tools, also herunterladen und zum Azure Abonnement Anweisungen verbinden. Benötigen Sie einen Verweis auf die Befehle in der Azure-CLI checken Sie diesen Link für [Referenz Azure CLI](../virtual-machines-command-line-tools.md). Außerdem müssen [SSH-Schlüssel für die Authentifizierung] und notieren **PEM-Speicherort**.


## <a name="creating-the-template"></a>Erstellen der Vorlage

### <a name="infrastructure"></a>Infrastruktur

1. Erstellen einer Gruppe Affinität zum Zusammenhalten der Ressourcen

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"

2. Erstellen eines virtuellen Netzwerks

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet

3. Erstellen Sie ein Speicherkonto unseren Disks hosten. Beachten Sie, dass Sie mehr als 40 stark platzieren sollten nicht verwendet Festplatten im selben Speicher-Konto um zu vermeiden, dass 20.000 IOPS Konto die maximale Größe. In diesem Fall sind wir weit von dieser Zahl wir alles im selben Konto Einfachheit speichern

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster

3. Suchen Sie den Namen des Bildes CentOS 7 Virtual Machine

        azure vm image list | findstr CentOS
Diese Ausgabe etwa `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`. Verwenden Sie den Namen im folgenden Schritt.

4. Ersetzen **/path/to/key.pem** durch den Pfad des Speicherortes den generierten PEM-SSH-Schlüssel VM-Vorlage erstellen

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser

5. Die VM für die Verwendung in der RAID-Konfiguration 4 x 500-GB-Datenträger zuordnen

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd

6. SSH in die Vorlage VM, die Sie am **mariadbhatemplate.cloudapp.net:22** erstellt und mit Ihrem privaten Schlüssel.

### <a name="software"></a>Software

1. Stamm zu erhalten

        sudo su

2. RAID-Unterstützung zu installieren:

     - Mdadm installieren

                yum install mdadm

     - Erstellen Sie RAID 0-Stripe-Konfiguration mit einem Dateisystem EXT4

                mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
                mdadm --detail --scan >> /etc/mdadm.conf
                mkfs -t ext4 /dev/md0

     - Das Mount-Verzeichnis erstellen

                mkdir /mnt/data

     - Den UUID des neu erstellten RAID-Gerät abrufen

                blkid | grep /dev/md0

     - / Etc/fstab bearbeiten

                vi /etc/fstab

     - Fügen Sie zu Auto Mouting Neustart vor dem Befehl **Blkid** abgerufenen Wert ersetzen die UUID aktivieren hinzu

                UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2

     - Laden Sie die neue partition

                mount /mnt/data

3. Installieren Sie MariaDB:

     - Erstellen Sie die Datei MariaDB.repo:

                vi /etc/yum.repos.d/MariaDB.repo

     - Füllen Sie es mit dem folgenden Inhalt

                [mariadb]
                name = MariaDB
                baseurl = http://yum.mariadb.org/10.0/centos7-amd64
                gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
                gpgcheck=1

     - Entfernen Sie vorhandene Postfix und Mariadb-Bibliotheken, um Konflikte zu vermeiden

            yum remove postfix mariadb-libs-*

     - Installieren Sie MariaDB mit Galera

            yum install MariaDB-Galera-server MariaDB-client galera

4. Verschieben Sie MySQL-Datenverzeichnis RAID-Block-Gerät

     - MySQL-Arbeitsverzeichnis in die neue Position kopieren und das alte Verzeichnis löschen

            cp -avr /var/lib/mysql /mnt/data  
            rm -rf /var/lib/mysql

     - Neues Verzeichnis Berechtigungen entsprechend festgelegt

            chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/  

     - Erstellen Sie einen symbolischen Link auf die neue Position auf der RAID-Partition des alten Verzeichnisses

            ln -s /mnt/data/mysql /var/lib/mysql

5. Da [die Clustervorgänge SELinux beeinträchtigen](http://galeracluster.com/documentation-webpages/configuration.html#selinux), für die aktuelle Sitzung sind (bis eine kompatible Version angezeigt wird) deaktivieren. Bearbeiten `/etc/selinux/config` für alle weiteren Starts deaktivieren:

            setenforce 0

       then editing `/etc/selinux/config` to set `SELINUX=permissive`

6. Überprüfen von MySQL wird ausgeführt

    - MySQL starten

            service mysql start

    - Sichern der MySQL-Installations, das Root-Passwort festgelegt, anonyme Benutzer remote Root Login deaktivieren und Entfernen der Datenbank entfernen

            mysql_secure_installation

    - Erstellen Sie einen Benutzer für die Datenbank für Clustervorgänge und optional einer Anwendung

            mysql -u root -p
            GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
            exit

   - MySQL beenden

            service mysql stop

7. Platzhalter für Konfiguration erstellen

    - Bearbeiten Sie die MySQL-Konfiguration, um einen Platzhalter für die Clustereinstellungen erstellen. Tauschen Sie die **`<Vairables>`** oder jetzt entfernen. Das geschieht, nachdem wir einen virtuellen Computer mit dieser Vorlage erstellen.

            vi /etc/my.cnf.d/server.cnf

    - Bearbeiten Sie den Abschnitt **[Galera]** und bereinigen

    - Bearbeiten Sie den Abschnitt **[Mariadb]**

            wsrep_provider=/usr/lib64/galera/libgalera_smm.so
            binlog_format=ROW
            wsrep_sst_method=rsync
            bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
            default_storage_engine=InnoDB
            innodb_autoinc_lock_mode=2

            wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
            #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
            #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
            #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
            #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server

8. Öffnen der erforderlichen Ports in der Firewall (mit FirewallD CentOS 7)

    - MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`
    - GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`
    - GALERA IST:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`
    - RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`
    - Laden Sie die Firewall:`firewall-cmd --reload`

9.  Optimieren der Leistung. Lesen Sie diesen Artikel [Performance tuning Strategie](virtual-machines-linux-classic-optimize-mysql.md) für Weitere Informationen

    - Bearbeiten Sie die Konfigurationsdatei MySQL erneut

            vi /etc/my.cnf.d/server.cnf

    - Bearbeiten Sie den Abschnitt **[Mariadb]** und fügen die folgenden

    > [AZURE.NOTE] Es wird empfohlen, **Innodb\_Puffer\_Pool_size** 70 % Ihrer VM-Speicher. Es wurde hier 2,45 GB für mittlere Azure VM mit 3,5 GB RAM festgelegt.

            innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70% of physical memory.
            innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
            max_connections = 5000 # A larger value will give the server more time to recycle idled connections
            innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
            innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
            innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
            query_cache_size = 0

10. Beenden Sie MySQL, MySQL-Dienst beim Start zu durcheinander Cluster beim Hinzufügen eines neuen Knotens ausgeführt deaktivieren und Entziehen von dem Computer.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision

11. Erfassen Sie die VM über das Portal. (Derzeit [Problem Nr. 1268 in der Azure-CLI] -Tools beschreibt die Tatsache, dass Bilder von Azure CLI-Tools nicht angeschlossenen Datenträger erfassen).

    - Herunterfahren des Computers über das portal
    - Klicken Sie auf Aufnahme Geben Sie Bild als **Mariadb Galera Bild** beschreiben Sie und überprüfen Sie "Ich Waagent laufen".
    ![Erfassen Sie den virtuellen Computer](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture.png)
    ![der virtuellen Maschine aufnehmen](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture2.PNG)

## <a name="creating-the-cluster"></a>Erstellen des Clusters

3 VMs aus der Vorlage nur erstellt und anschließend konfigurieren und starten Sie den Cluster erstellen.

1. Erstellen der ersten CentOS 7 VM **Mariadb Galera Bild** Bild Sie, die virtuellen Netzwerk namens **Mariadbvnet** und Subnetz **Mariadb**Maschine Größe **Mittel**im Cloud-Dienst zu **Mariadbha** name (oder den Namen mariadbha.cloudapp.net zugegriffen werden soll), der Name dieser Maschine **mariadb1** und Benutzername, **Azureuser**und SSH-Zugriff aktivieren und übergeben den SSH-Zertifikat PEM-Datei und Ersetzen **/path/to/key.pem** durch den Pfad, in dem Sie den generierte PEM-SSH-Schlüssel gespeichert.

    > [AZURE.NOTE] Die folgenden Befehle werden über mehrere Zeilen aus Gründen der Übersichtlichkeit aufgeteilt, aber geben Sie jede Zeile.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser

2. 2 weitere virtuelle Computer erstellen, _indem_ Sie die gerade erstellte **Mariadbha** Cloud-Dienst einen eindeutigen Port nicht in Konflikt mit anderen virtuellen Computern im selben Cloud-Dienst den **Namen des virtuellen Computers** als auch **SSH-Port** ändern.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
und MariaDB3

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser

3. Sie benötigen die interne IP-Adresse jedes 3 VMs für den nächsten Schritt:

    ![IP-Adresse](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/IP.png)

4. SSH in 3 VMs und und bearbeiten Sie die Konfigurationsdatei auf

        sudo vi /etc/my.cnf.d/server.cnf

    Sie auskommentieren **`wsrep_cluster_name`** und **`wsrep_cluster_address`** durch Entfernen der **#** am Anfang und Validierung sind tatsächlich erwünscht.
    Darüber hinaus ersetzt **`<ServerIP>`** in **`wsrep_node_address`** und **`<NodeName>`** in **`wsrep_node_name`** mit der VM-IP-Adresse und bzw. und kommentieren Sie diese Zeile.

5. Starten Sie den Cluster auf MariaDB1 und Systemstart laufen lassen

        sudo service mysql bootstrap
        chkconfig mysql on

6. Starten Sie MySQL MariaDB2 und MariaDB3 und beim Systemstart laufen lassen

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balancing-the-cluster"></a>Lastenausgleich des Clusters
Beim Erstellen von gruppierten VMs haben Sie eine Verfügbarkeit Set aufgerufen **Clusteravset** zu gewährleisten setzten auf verschiedenen Domänen und Azure nie Wartung auf allen Computern gleichzeitig hinzugefügt. Diese Konfiguration entspricht durch, Azure Service Level Agreement (SLA) unterstützt werden.

Jetzt verwenden Sie Azure Lastenausgleich zwischen unseren 3 Knoten auszugleichen.

Führen Sie die folgenden Befehle auf dem Computer mit der Azure-CLI.
Die Befehlsstruktur Parameter ist:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

Schließlich da CLI Lastenausgleich Prüfpunkt Intervall von 15 Sekunden festgelegt (die etwas zu lang sein können), ändern sie im Portal unter **Endpunkte** für alle VMs

![Endpunkt bearbeiten](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint.PNG)

dann klicken Sie auf Reconfigure The Load-Balanced festgelegt und Weiter

![Ausgewogene RECONFIGURE laden](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint2.PNG)

Ändern Sie das Intervall Prüfpunkt auf 5 Sekunden und speichern

![Die Sonde Änderungsintervall](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validating-the-cluster"></a>Überprüfen des Clusters

Die Arbeit erfolgt. Cluster sollte jetzt auf `mariadbha.cloudapp.net:3306` die Lastenausgleich erreicht und Routen zwischen 3 VMs, reibungslos und effizient.

Verwenden Sie Ihre bevorzugten MySQL-Client herstellen oder nur eines der VMs zu überprüfen, ob diese Cluster arbeiten.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Eine neue Datenbank erstellen und mit Daten füllen

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

In der folgenden Tabelle führt

    +----+--------+
  	| id | value  |
    +----+--------+
  	|  1 | Value1 |
  	|  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel 3 erstellten Knoten MariaDB + Galera Hochverfügbarkeits-Cluster auf Azure Virtual Machines CentOS 7 ausgeführt. Die VMs erfolgt ein Lastenausgleich mit Azure Lastenausgleich.

Möglicherweise möchten [können MySQL Linux Cluster](virtual-machines-linux-classic-mysql-cluster.md) und Methoden zum [optimieren und Testen MySQL Leistung auf Azure Linux VMs](virtual-machines-linux-classic-optimize-mysql.md)betrachten.

<!--Anchors-->
[Architecture overview]: #architecture-overview
[Creating the template]: #creating-the-template
[Creating the cluster]: #creating-the-cluster
[Load balancing the cluster]: #load-balancing-the-cluster
[Validating the cluster]: #validating-the-cluster
[Next steps]: #next-steps

<!--Image references-->

<!--Link references-->
[Galera]: http://galeracluster.com/products/
[MariaDBs]: https://mariadb.org/en/about/
[Erstellen Sie einen SSH-Schlüssel für die Authentifizierung]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[Problem #1268 in der Azure-CLI]:https://github.com/Azure/azure-xplat-cli/issues/1268
