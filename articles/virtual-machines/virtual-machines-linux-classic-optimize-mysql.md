<properties
    pageTitle="Optimieren der Leistung unter Linux VMs MySQL | Microsoft Azure"
    description="Erfahren Sie mehr über das MySQL auf unter Linux ein Azure Virtual Machine (VM) zu optimieren."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="optimizing-mysql-performance-on-azure-linux-vms"></a>Optimieren der Leistung MySQL Azure Linux VMs

Es gibt viele Faktoren, die in Azure MySQL Leistungseinbußen in virtuelle Hardwarekonfiguration Auswahl und Software. Dieser Artikel befasst sich Optimieren der Leistung über Speicher, System und Datenbankkonfigurationen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


##<a name="utilizing-raid-on-an-azure-virtual-machine"></a>Mit RAID auf Azure VM
Speicher ist der Schlüsselfaktor, die Datenbank-Performance in einer Cloud-Umgebung auswirkt.  Im Vergleich zu einer Festplatte, bietet RAID schnelleren Zugriff über Parallelität.  Weitere Informationen finden Sie unter [Standard RAID-Level](http://en.wikipedia.org/wiki/Standard_RAID_levels) .   

E/a-Durchsatz und i/o-Reaktionszeit in Azure können durch RAID erheblich verbessert werden. Unsere Labortests Durchsatz verdoppelt und i/o-Reaktionszeit reduziert werden um die Hälfte durchschnittlich verdoppelt die Anzahl der RAID-Festplatten Datenträger anzeigen (von 2 bis 4, 4, 8 usw.). Details finden Sie in [Anhang A](#AppendixA) .  

Neben der Festplatte verbessert die MySQL-Performance bei RAID-Level erhöhen.  Details finden Sie in [Anhang B](#AppendixB) .  

Sie möchten auch die Segmentgröße berücksichtigen. Im Allgemeinen bei größeren Chunk-Größe erhalten niedriger, insbesondere für große schreibt Sie. Jedoch wenn Chunk-Größe zu groß ist, Hinzufügen zusätzlichen Overhead und RAID nutzen kann nicht. Aktuelle Standardgröße beträgt 512KB für allgemeine produktionsumgebung optimal ist. Details finden Sie in [Anhang C](#AppendixC) .   

Beachten Sie, dass gibt wie viele Laufwerke bei anderen virtuellen Computer hinzufügen können. Diese Grenzwerte sind in [virtuellen und Azure Cloud Service Größen](http://msdn.microsoft.com/library/azure/dn197896.aspx)aufgeführt. Zwar kann mit weniger Festplatten RAID einrichten möchten, benötigen Sie 4 angeschlossenen Datenträger im RAID-Beispiel in diesem Artikel folgen.  

Vorausgesetzt Sie haben bereits eine virtuellen Linux-Maschine und MYSQL installiert und konfiguriert haben. Weitere Informationen zu den ersten Schritten finden Sie MySQL Azure installieren.  

###<a name="setting-up-raid-on-azure"></a>Einrichten von RAID auf Azure
Die folgenden Schritte zeigen, wie RAID auf Azure erstellen mit klassischen Azure-Portal. Sie können auch RAID einrichten, mithilfe von Windows PowerShell-Skripts.
In diesem Beispiel wird mit 4 Festplatten RAID 0 konfiguriert.  

####<a name="step-1-add-a-data-disk-to-your-virtual-machine"></a>Schritt 1: Ihr virtueller Computer einen Datenträger hinzufügen  

Klicken Sie auf virtuellen Maschinen von klassischen Azure-Portal auf den virtuellen Computer, den Sie einen Datenträger hinzufügen möchten. In diesem Beispiel ist der virtuelle Computer mysqlnode1.  

![][1]

Klicken Sie auf der Seite für den virtuellen Computer auf **Dashboard**.  

![][2]


Klicken Sie in der Taskleiste auf **Anfügen**.

![][3]

Und anschließend auf **leeren Datenträger anfügen**.  

![][4]

Für Datenträger sollten **Host Cache-Einstellung** auf **None**festgelegt werden.  

Dadurch wird eine leere Diskette in virtuellen Computers hinzugefügt. Wiederholen Sie diesen Schritt drei mehrmals, sodass 4 Datenträger RAID.  

Die hinzugefügten Laufwerke auf dem virtuellen Computer sehen Kernel-Message-Protokoll ansehen. Hierzu auf Ubuntu, z. B. den folgenden Befehl:  

    sudo grep SCSI /var/log/dmesg

####<a name="step-2-create-raid-with-the-additional-disks"></a>Schritt 2: Erstellen Sie RAID mit zusätzlichen Festplatten
Folgendermaßen Sie ausführliche RAID-Setup:  

[Konfigurieren von Software-RAID auf Linux](virtual-machines-linux-configure-raid.md)

>[AZURE.NOTE] Bei Verwendung des Dateisystems XFS gehen Sie nach der Erstellung RAID.

XFS Debian, Ubuntu oder Linux Mint installieren, verwenden Sie den folgenden Befehl ein:  

    apt-get -y install xfsprogs  

XFS auf Fedora CentOS oder RHEL installieren, verwenden Sie den folgenden Befehl ein:  

    yum -y install xfsprogs  xfsdump


####<a name="step-3-set-up-a-new-storage-path"></a>Schritt 3: Einrichten einer neuen Speicherpfad
Verwenden Sie den folgenden Befehl ein:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

####<a name="step-4-copy-the-original-data-to-the-new-storage-path"></a>Schritt 4: Kopieren der ursprünglichen Daten für den neuen Speicherpfad
Verwenden Sie den folgenden Befehl ein:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

####<a name="step-5-modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Schritt 5: Ändern von Berechtigungen, MySQL zugreifen kann (Lesen und Schreiben) der Datenträger
Verwenden Sie den folgenden Befehl ein:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


##<a name="adjust-the-disk-io-scheduling-algorithm"></a>Datenträger-e/a-Planungsalgorithmus anpassen
Linux implementiert vier Typen von e/a Planungsalgorithmen:  

-   NOOP-Algorithmus (No Operation)
-   Stichtag Algorithmus (Endtermin)
-   Vollständig fair Queuing-Algorithmus (CFQ)
-   Periode Budget-Algorithmus (Vorausschauhorizont)  

Wählen Sie verschiedene e/a-Planer in verschiedenen Szenarien zu optimieren. In einer Umgebung vollständig RAM ist ein großer Unterschied zwischen CFQ und Stichtag Algorithmen für die Leistung nicht. Frist für die Stabilität die MySQL-Datenbank-Umgebung gesetzt wird empfohlen. Viele sequenzieller Zugriff kann, CFQ Datenträger e/a-Leistung verringern.   

SSD und andere Geräte erreichen mit NOOP oder Frist eine bessere Leistung als der Standardplaner.   

Vom Kernel 2.5 ist der Standard e/a-scheduling Stichtag. Vom Kernel 2.6.18 wurde CFQ Standard e/a-scheduling-Algorithmus.  Sie können diese Einstellung zur Startzeit Kernel angeben oder dynamisch diese Einstellung ändern, wenn das System ausgeführt wird.  

Im folgenden Beispiel wird veranschaulicht, wie überprüfen und den Standardplaner NOOP-Algorithmus.  

Für Debian-Distribution-Familie:

###<a name="step-1view-the-current-io-scheduler"></a>Schritt 1. Ansicht der aktuellen e/a-Planer
Verwenden Sie den folgenden Befehl ein:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Sie sehen nach Ausgabe, die aktuellen Planer angibt.  

    noop [deadline] cfq


###<a name="step-2-change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Schritt 2. Das aktuelle Gerät (/ Dev/Sda) i/o-Algorithmus planen
Verwenden Sie die folgenden Befehle:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

>[AZURE.NOTE] Einstellung für sda allein ist nicht sinnvoll. Es muss auf alle Datenträger festgelegt werden, in dem sich die Datenbank befindet.  

Die folgende Ausgabe zeigt an, dass diese grub.cfg erfolgreich erstellt wurde und der Standardplaner NOOP aktualisiert wurde, sollte angezeigt werden.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Familie Verteilung Redhat benötigen Sie nur den folgenden Befehl:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

##<a name="configure-system-file-operations-settings"></a>Datei Operationen Systemeinstellungen konfigurieren
Eine empfiehlt Atime Protokollierungsfunktion im Dateisystem deaktivieren. Atime ist die letzte Datei zugreifen. Wenn eine Datei zugegriffen wird, protokolliert Datei der Timestamp im Protokoll. Diese Information wird jedoch selten verwendet. Sie können es deaktivieren, wenn Sie es benötigen, insgesamt Festplattenzugriffe wird.  

Um Atime Protokollierung deaktivieren möchten, müssen Sie Datei System Configuration Datei sich Fstab ändern und die Option **Noatime** hinzufügen.  

Beispielsweise bearbeiten Sie die Vim/etc/fstab-Datei Noatime wie folgt hinzufügen.  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Dann montieren Sie das Dateisystem mit dem folgenden Befehl:  

    mount -o remount /RAID0

Die geänderte Testergebnis. Beachten Sie, dass beim Ändern der Testdatei die Zugriffszeit nicht aktualisiert.  

Beispiel: vor     

![][5]

Beispiel:

![][6]

##<a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Die maximale Anzahl von Systemhandles hoher Parallelität zu erhöhen
MySQL ist hoher Parallelität Datenbank. Die Standardanzahl von gleichzeitigen Handles ist 1024 für Linux nicht immer ausreichend. **Gehen Sie folgendermaßen vor, um maximale gleichzeitige Handles des Systems zur Unterstützung hoher Parallelität von MySQL zu erhöhen**.

###<a name="step-1-modify-the-limitsconf-file"></a>Schritt 1: Ändern Sie die Datei limits.conf
Fügen Sie die folgenden vier Zeilen in der Datei /etc/security/limits.conf maximal zulässige parallele Handles zu. Beachten Sie, dass 65536 maximale Anzahl, die das System unterstützen kann.   

    * Weiche Nofile 65536
    * Festplatte Nofile 65536
    * Weiche Nproc 65536
    * Festplatte Nproc 65536

###<a name="step-2-update-the-system-for-the-new-limits"></a>Schritt 2: Aktualisieren Sie das System für die neuen Grenzwerte
Führen Sie die folgenden Befehle:  

    ulimit -SHn 65536
    ulimit -SHu 65536

###<a name="step-3-ensure-that-the-limits-are-updated-at-boot-time"></a>Schritt 3: Sicherstellen Sie, dass die Grenzwerte beim Start aktualisiert werden
Damit die folgenden Startbefehle in der Datei /etc/rc.local sie bei jedem Neustart wirksam wird.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

##<a name="mysql-database-optimization"></a>Optimierung der MySQL-Datenbank
Performance tuning Strategie können Sie MySQL auf Azure als auf einem lokalen Computer konfigurieren.  

Die e/a-Optimierungsregeln sind:   

-   Erhöhen Sie die Cachegröße.
-   I/o-Reaktionszeit verringert.  

Optimieren Sie MySQL Server konfigurieren können Sie die Datei my.cnf aktualisieren die Standardkonfigurationsdatei für Server- und Clientcomputern.  

Die folgenden Konfigurationselemente sind die wichtigsten Faktoren, die MySQL-Leistung beeinflussen:  

-   **Innodb_buffer_pool_size**: Pufferpool gepufferten Daten und den Index enthält. Dies ist normalerweise auf 70 % des physischen Speichers festgelegt.
-   **Innodb_log_file_size**: Dies ist die Größe wiederherstellen. Redo-Protokolle verwenden Sie sicherstellen, dass Schreibvorgänge nach einem Systemabsturz schnell, zuverlässig und wiederhergestellt werden. Dies soll auf 512 MB an ausreichend Speicherplatz für die Protokollierung schreiben.
-   **Max_connections**: manchmal Applikationen lassen Verbindungen ordnungsgemäß. Ein größerer Wert geben dem Server länger beanspruchte Verbindungen wiederverwenden. Die maximale Anzahl Verbindungen 10000, jedoch die empfohlene maximale 5000.
-   **Innodb_file_per_table**: Diese Einstellung aktivieren oder Deaktivieren der InnoDB Tabellen in separaten Dateien gespeichert. Aktivieren der Option wird sichergestellt, dass erweiterte Verwaltungsvorgänge effizient eingesetzt werden können. Aus Sicht des Performance kann Tabelle Raum Übertragung beschleunigen und Optimierung der Performance Management Fremdkörper. Dies ist die empfohlene Einstellung für dieses auf.</br>
    Die Standardeinstellung ist von MySQL 5.6 auf. Daher ist keine Aktion erforderlich. Für andere Versionen ist 5.6 vor ist standardmäßig deaktiviert. Diese ist erforderlich. Und sollte vor dem Daten geladen werden, da nur neu erstellte Tabellen betroffen sind.
-   **Innodb_flush_log_at_trx_commit**: Standardwert ist 1 mit dem Gültigkeitsbereich 0 ~ 2. Der Standardwert ist die passende Option für eigenständige MySQL DB. Die Einstellung 2 kann die meisten Datenintegrität und eignet sich für Master in MySQL Cluster. Die Einstellung 0 kann beeinflussen Zuverlässigkeit in einigen Fällen eine bessere Leistung und MySQL Clusters Slave für Datenverlust.
-   **Innodb_log_buffer_size**: der Protokollpuffer ohne das Protokoll auf Datenträger zu leeren, bevor Transaktionen Commit ermöglicht. Große binäre Objekte oder Textfeld vorhanden ist, jedoch der Cache sehr schnell verbraucht und häufige Datenträger ausgelöst. Es ist besser die Puffergröße erhöhen, Statusvariable Innodb_log_waits ist nicht 0.
-   **Query_cache_size**: die beste Option ist von Anfang an zu deaktivieren. Query_cache_size auf 0 (Dies ist nun die Standardeinstellung in MySQL 5.6) und verwenden Sie andere Methoden, um Abfragen zu beschleunigen.  

Siehe [Anhang D](#AppendixD) für den Vergleich nach der Optimierung.


##<a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Schalten Sie langsam Abfrageprotokoll MySQL für die Analyse von Performance-Engpässen
Langsame Abfrageprotokoll MySQL helfen Ihnen Identifizieren langsamer Abfragen für MySQL. Nach der Aktivierung langsam Abfrageprotokoll MySQL, können MySQL-Tools wie **Mysqldumpslow** um Leistungsengpässe zu identifizieren.  

Beachten Sie, dass standardmäßig diese Option nicht aktiviert ist. Langsame Abfrageprotokoll einschalten kann einige CPU-Ressourcen verbrauchen. Daher wird empfohlen, dass Sie dies vorübergehend aktivieren, für die Behandlung von Performance-Engpässen.

###<a name="step-1-modify-mycnf-file-by-adding-the-following-lines-to-the-end"></a>Schritt 1: Ändern Sie my.cnf-Datei, indem Sie die folgenden Zeilen am Ende hinzufügen   

    long_query_time = 2
    slow_query_log = 1
    slow_query_log_file = /RAID0/mysql/mysql-slow.log

###<a name="step-2-restart-mysql-server"></a>Schritt 2: Starten Sie Mysql-server
    service  mysql  restart

###<a name="step-3-check-whether-the-setting-is-taking-effect-using-the-show-command"></a>Schritt 3: Überprüfen Sie, ob die Einstellung mit dem Befehl "Show" stattfindet

![][7]   

![][8]

In diesem Beispiel sehen langsam Abfragefunktion aktiviert wurde. Dann können Sie das Tool **Mysqldumpslow** Performance-Engpässe ermitteln und optimieren, wie Indizes.





##<a name="appendix"></a>Anhang

Die folgenden Tests Leistungsdaten abtasten gezielte Lab-Umgebung erstellt werden, sie Hintergrund auf dem Trend der Performance-Daten mit anderen Methoden für die Optimierung jedoch variieren die Ergebnisse unter Umgebung oder Produkt Varianten.

<a name="AppendixA"></a>Anhang A:  
**Datenträgerleistung (IOPS) mit verschiedenen RAID-Ebenen**


![][9]

**Testbefehle:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

>AZURE. Hinweis: Die Arbeitslast dieses Tests verwendet 64 Threads versuchen, RAID erreicht.

<a name="AppendixB"></a>Anhang B:  
**MySQL (Durchsatz) Vergleich mit verschiedenen RAID-Ebenen**   
(XFS Dateisystem)


![][10]  
![][11]

**Testbefehle:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**MySQL (OLTP) Vergleich mit verschiedenen RAID-Ebenen**  
![][12]

**Testbefehle:**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

<a name="AppendixC"></a>Anhang C:   
**Datenträger (IOPS) Leistungsvergleich für die verschiedenen Chunk-Größe**  
(XFS Dateisystem)


![][13]

**Testbefehle:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Hinweis die für diese Tests verwendete Größe 30 und 1 GB ist, mit RAID 0(4 disks) XFS Speicherabbilddatei System.


<a name="AppendixD"></a>Anhang D:  
**MySQL (Durchsatz) Vergleich vor und nach der Optimierung**  
(XFS File System)


![][14]

**Testbefehle:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**Die Einstellung für Standardwert und Optimierung lautet wie folgt:**

|Parameter |Standard    |Optimierung
|-----------|-----------|-----------
|**innodb_buffer_pool_size**    |Keine   |7G
|**innodb_log_file_size**   |5M |512 MB
|**max_connections**    |100    |5000
|**innodb_file_per_table**  |0  |1
|**innodb_flush_log_at_trx_commit** |1  |2
|**innodb_log_buffer_size** |8M |128 MB
|**query_cache_size**   |26.    |0


Mehr detaillierte Optimierung Konfigurationsparameter finden Sie in der offiziellen Dokumentation Mysql.  

[http://dev.MySQL.com/doc/refman/5.6/en/InnoDB-Configuration.HTML](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html)  

[http://dev.MySQL.com/doc/refman/5.6/en/InnoDB-Parameters.HTML#sysvar_innodb_flush_method](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method)

**Umgebung**  

|Hardware   |Details
|-----------|-------
|CPU    |AMD Opteron(tm) Prozessor 4171 HE / 4 Kernen
|Speicher |14G
|Datenträger   |10G-Datenträger
|Betriebssystem |Ubuntu 14.04.1 LTS



[1]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png
