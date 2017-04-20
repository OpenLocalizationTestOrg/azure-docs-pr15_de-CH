<properties
    pageTitle="MySQL mit Lastenausgleich clusterize | Microsoft Azure"
    description="Einen Lastenausgleich hohen Verfügbarkeit Linux MySQL Cluster mit dem klassischen Bereitstellungsmodell Azure erstellt Setup"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="bureado"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/14/2015"
    ms.author="jparrel"/>

# <a name="using-load-balanced-sets-to-clusterize-mysql-on-linux"></a>Verwenden mit Lastenausgleich, MySQL Linux clusterize

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Dieser Artikel ist zu erläutern die verschiedenen Methoden zum Bereitstellen hoher Verfügbarkeit Linux-basierten Services auf Microsoft Azure erforschen MySQL Server hohe Verfügbarkeit als Einstieg in. Eine Video zur Veranschaulichung dieses Ansatzes steht auf [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL).

Beschreiben wir eine solch zwei Knoten Einzelmaster-MySQL Lösung mit hoher Verfügbarkeit basierend auf DRBD, Corosync und Schrittmacher. Nur ein Knoten MySQL zu einem Zeitpunkt ausgeführt wird. Lesen und Schreiben aus der DRBD Ressource ist auch jeweils nur ein Knoten auf.

Gibt es keine Notwendigkeit einer VIP-Lösung wie LVS seit wir Microsoft Azure Lastsätze Balanced Roundrobin-Funktionalität und der endpunkterkennung, Entfernung und ordnungsgemäßes Beenden der VIP-Adresse. Die VIP ist ein global routingfähig IPv4-Adresse von Microsoft Azure beim ersten Cloud-Dienst erstellen.

Gibt andere Architekturen für MySQL sowie NBD-Cluster, Percona und Galera mehrere Middleware-Projektmappen mindestens als VM auf [VM](http://vmdepot.msopentech.com). Solange diese Lösungen Unicast und Multicast oder Übertragung replizieren können und nicht auf freigegebenen Speicher oder mehrere Netzwerkschnittstellen, sollte Szenarien einfach auf Microsoft Azure bereitgestellt.

Natürlich können dieser Cluster Architekturen zu anderen Produkten wie PostgreSQL und OpenLDAP auf ähnliche Weise erweitert werden. Beispielsweise diese Prozedur ein Lastenausgleich mit shared-Nothing Multimaster OpenLDAP erfolgreich getestet wurde und auf Channel 9 Blog ansehen.

## <a name="getting-ready"></a>Vorbereitung

Sie benötigen ein Microsoft Azure-Konto mit einem gültigen Abonnement mindestens zwei (2) VMs erstellen (XS wurde in diesem Beispiel verwendet), Netzwerk und einem Subnetz, eine Gruppe und eine Verfügbarkeit festgelegt sowie die Möglichkeit, neue virtuelle Festplatten im Bereich Cloud-Dienst erstellen und Anhängen an Linux-VMs.

### <a name="tested-environment"></a>Getestete Umgebung

- Ubuntu 13.10.
  - DRBD
  - MySQL-Server
  - Corosync und Schrittmacher

### <a name="affinity-group"></a>Gruppe

Eine Gruppe für die Lösung wird in der Azure klassischen Portal Bildlauf auf Einstellungen und Erstellen einer neuen Gruppe Affinität erstellt. Diese Gruppe werden später zugewiesene Ressourcen zugewiesen.

### <a name="networks"></a>Netzwerke

Erstellt ein neues Netzwerk und einem Subnetz im Netzwerk erstellt. Subnetz nur ein /24 in einem Netzwerk 10.10.10.0/24 ausgewählt.

### <a name="virtual-machines"></a>Virtuelle Computer

Die erste Ubuntu 13.10. VM mit einem gebilligt Ubuntu Galeriebild erstellt und aufgerufen `hadb01`. Neue Cloud-Dienst wird im Prozess namens Hadb erstellt. Wir nennen es so Art gemeinsam genutzten, Lastenausgleich erläutern, die der Dienst haben wir weitere Ressourcen hinzufügen. Die Erstellung von `hadb01` problemlosen und über das Portal abgeschlossen ist. Ein Endpunkt für SSH automatisch erstellt und unsere erstellte Netzwerk ausgewählt. Wir wählen Sie auch eine neue Verfügbarkeit für VMs festgelegt.

Nach der erste virtuellen Computer entsteht (technisch Cloud-Dienst erstellt wird) fahren wir zweite VM erstellen `hadb02`. Für die zweite VM verwenden wir auch Ubuntu 13.10. VM aus dem Katalog über das Portal jedoch wählen wir einen vorhandenen Cloud-Dienst mit `hadb.cloudapp.net`, anstatt eine neue zu erstellen. Netzwerk- und Verfügbarkeit Satz sollte uns automatisch ausgewählt. Zu wird ein SSH-Endpunkt erstellt.

Nachdem beide VMs erstellt haben, wir werden berücksichtigen den SSH-Port für `hadb01` (TCP 22) und `hadb02` (automatisch zugewiesene von Azure)

### <a name="attached-storage"></a>Speichermedien

Wir beide virtuellen Computer eine neue Festplatte zuordnen und dabei neue 5 GB Festplatten erstellen. Datenträger werden im Container VHD verwendet für unsere Betriebssystem-Datenträger befinden. Datenträger erstellt und zugeordnet ist keine Notwendigkeit starten Sie Linux Kernel das neue Gerät angezeigt wird (normalerweise `/dev/sdc`, überprüfen Sie `dmesg` für die Ausgabe)

Jede VM wir weiter zum Erstellen einer neuen Partition mit `cfdisk` (primär-, Linux-Partition) und neue Partitionstabelle zu schreiben. **Erstellen Sie ein Dateisystem auf dieser Partition nicht** .

## <a name="setting-up-the-cluster"></a>Einrichten des Clusters

Beide VMs Ubuntu müssen wir mit APT Corosync Schrittmacher und DRBD installiert. Mit `apt-get`:

    sudo apt-get install corosync pacemaker drbd8-utils.

**MySQL zurzeit nicht installiert** . Debian und Ubuntu Installationsskripts Initialisiert eine MySQL-Datenverzeichnis auf `/var/lib/mysql`, aber da das Verzeichnis in einem Dateisystem DRBD abgelöst werden, müssen wir dies später tun.

An diesem Punkt sollten wir auch überprüfen (mit `/sbin/ifconfig`), dass beiden VMs Adressen im Subnetz 10.10.10.0/24 und dass sie gegenseitig ein Ping ausführen können. Bei Bedarf können Sie auch `ssh-keygen` und `ssh-copy-id` um sicherzustellen, dass beide virtuelle Computer kommunizieren über SSH ohne ein Kennwort.

### <a name="setting-up-drbd"></a>Einrichten von DRBD

Erstellen wir eine DRBD-Ressource, die das zugrunde liegende verwendet `/dev/sdc1` Partition zu einer `/dev/drbd1` Ressource zu ext3 mit formatiert und in primäre und sekundäre Knoten verwendet. Öffnen Sie hierzu `/etc/drbd.d/r0.res` , und kopieren Sie die folgende Definition der Ressource. Dabei beide virtuelle Computer:

    resource r0 {
      on `hadb01` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.4:7789;
        meta-disk internal;
      }
      on `hadb02` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.5:7789;
        meta-disk internal;
      }
    }

Danach Initialisieren der Ressource mit `drbdadm` in beiden VMs:

    sudo drbdadm -c /etc/drbd.conf role r0
    sudo drbdadm up r0

Und schließlich auf dem primären (`hadb01`) (primär) an die DRBD Ressource erzwingen:

    sudo drbdadm primary --force r0

Wenn Sie den Inhalt von/Proc/Drbd untersuchen (`sudo cat /proc/drbd`) auf beiden VMs finden Sie `Primary/Secondary` auf `hadb01` und `Secondary/Primary` auf `hadb02`, die Lösung zu diesem Zeitpunkt entsprechen. 5 GB Festplatte werden über das Netzwerk 10.10.10.0/24 Kunden kostenlos synchronisiert.

Sobald der Datenträger synchronisiert können das Dateisystem auf `hadb01`. Zu Testzwecken ext2 verwendet, aber die folgende Anweisung erstellt eine ext3-Dateisystem:

    mkfs.ext3 /dev/drbd1

### <a name="mounting-the-drbd-resource"></a>Die DRBD-Ressource bereitstellen

Auf `hadb01` nun die DRBD Ressourcen bereitstellen können. Debian und Derivate mit `/var/lib/mysql` als MySQL Datenverzeichnisses. Da wir MySQL installiert haben, wir erstellen das Verzeichnis und die DRBD Ressource bereitstellen. On `hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## <a name="setting-up-mysql"></a>Einrichten von MySQL

Nun Sie MySQL auf Installieren `hadb01`:

    sudo apt-get install mysql-server

Für `hadb02`, haben Sie zwei Optionen. Sie können nun Mysql-Server Datenverzeichnis erstellen und mit einer neuen Datenverzeichnis füllen und anschließend die Inhalte zu entfernen. On `hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

Die zweite Option ist Failover auf `hadb02` und Mysql-Server installieren (Installationsskripts sehen die vorhandene Installation und nicht berühren)

On `hadb01`:

    sudo drbdadm secondary –force r0

On `hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

Wenn Sie Failover DRBD jetzt nicht, ist die erste Option einfacher wohl weniger elegant. Nachdem Sie dies einrichten, können Sie beginnen, der MySQL-Datenbank arbeiten. Auf `hadb02` (oder welche Server aktiv nach DRBD):

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

**Warnung**: Diese letzte Anweisung deaktiviert die Authentifizierung für den Root-Benutzer in dieser Tabelle. Dies sollte Ihre Produktions-Klasse ersetzt-Anweisung gewähren und dient nur zur Veranschaulichung.

Sie müssen aktivieren Netzwerk für MySQL möchten Sie Abfragen von außerhalb den VMs der Zweck dieses Handbuchs. Öffnen Sie beide VMs `/etc/mysql/my.cnf` auf `bind-address`, von 127.0.0.1 auf 0.0.0.0 ändern. Nach dem Speichern der Datei Ausstellen einer `sudo service mysql restart` der aktuellen primären.

### <a name="creating-the-mysql-load-balanced-set"></a>Erstellen der MySQL-Last ausgeglichen festlegen

Gehen wir zurück zum Portal und wechseln Sie zu den `hadb01` VM und Endpunkte. Wir erstellen neue, Dropdown und Teilstriche auf das *Erstellen neuer Lastenausgleich Satz* MySQL (TCP 3306) auswählen. Wir nennen unsere Lastenausgleich Endpunkt `lb-mysql`. Die meisten Optionen lassen außer Zeit allein wir die wir 5 (minimale Sekunden) verringern

Nach dem Erstellen der Endpunkt wir gehen zu `hadb02`, Endpunkte und erstellen Sie einen neuen Endpunkt jedoch wählen wir `lb-mysql`, wählen Sie MySQL aus dem Dropdown-Menü. Sie können auch die Azure-CLI für diesen Schritt.

Im Moment haben wir alles für einen manuellen Vorgang des Clusters.

### <a name="testing-the-load-balanced-set"></a>Testen die Last ausgeglichen festlegen

Prüfungen können von einem externen Computer In diesem Fall mit einem MySQL-Client sowie Applications (z. B. eine Azure-Website unter PhpMyAdmin) MySQLs-Befehlszeilentools auf einen anderen Linux verwendet:

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### <a name="manually-failing-over"></a>Manuelles Failover

Failover können nun MySQL Herunterfahren DRBDs primären wechseln und MySQL erneut starten simulieren.

Auf hadb01:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

Auf hadb02:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

Wenn Sie Failover manuell wiederholt sollten remote Query und perfekt funktioniert.

## <a name="setting-up-corosync"></a>Corosync einrichten

Corosync ist die zugrunde liegende Clusterinfrastruktur für Schrittmacher arbeiten erforderlich. Heartbeat v1 und v2 Benutzer (und andere Methoden wie Ultramonkey) Corosync ist eine Unterbrechung der CRM-Funktionen Schrittmacher Funktionen ähnlich Traffic bleibt.

Die wichtigste Einschränkung für Corosync auf Azure ist Corosync zieht Multicast über Broadcasts über Unicast-Kommunikation jedoch Microsoft Azure Vernetzung unterstützt nur Unicast.

Glücklicherweise Corosync hat ein Unicastmodus arbeiten und die einzige Einschränkung ist, da alle Knoten nicht untereinander *automatisch*kommunizieren, Sie die Knoten in den Konfigurationsdateien, einschließlich IP-Adressen definieren müssen. Können wir die Corosync Beispieldateien für Unicast und nur ändern Adresse und Knotenlisten Protokollierungsverzeichnis binden (Ubuntu verwendet `/var/log/corosync` beim Beispiel mit Dateien `/var/log/cluster`) und Quorum-Tools.

**Beachten Sie die `transport: udpu` Richtlinie unten und manuell definierten IP-Adressen für die Knoten**.

Auf `/etc/corosync/corosync.conf` für beide Knoten:

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }

    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }

    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }

      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }

    quorum {
      provider: corosync_votequorum
    }

Wir kopieren Sie diese Datei in beiden VMs und Corosync in beiden Knoten starten:

    sudo service start corosync

Kurz sollte nach dem Starten des Dienstes Cluster im aktuellen Ring und beschlussfähig sollte durchgeführt werden. Wir können dies durch Überprüfen der Protokolle überprüfen oder:

    sudo corosync-quorumtool –l

Eine Ausgabe ähnlich der Abbildung unten befolgen:

![Corosync-Quorumtool - l Ausgabe](media/virtual-machines-linux-classic-mysql-cluster/image001.png)

## <a name="setting-up-pacemaker"></a>Schrittmacher einrichten

Schrittmacher verwendet Cluster Ressourcen überwachen, wenn Primärfarben gehen definieren und Ressourcen zum sekundäre wechseln. Ressourcen können aus einer Reihe von verfügbaren Skripts oder LSB (Init wie) Skripts unter anderem definiert werden.

Wir möchten Schrittmacher "DRBD-Ressource, die Mountpoint und MySQL-Dienst besitzt". Wenn Schrittmacher auf und DRBD deaktivieren kann, mounten / unmount und Starten/Anhalten MySQL in der richtigen Reihenfolge etwas Schlimmes passiert mit dem primären Einrichtung ist abgeschlossen.

Bei der Installation von Schrittmacher sollte einfach etwas Ihre Konfiguration:

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

Überprüfen sie mit `sudo crm configure show`. Erstellen Sie nun eine Datei (z. B. `/tmp/cluster.conf`) in den folgenden Ressourcen:

    primitive drbd_mysql ocf:linbit:drbd \
          params drbd_resource="r0" \
          op monitor interval="29s" role="Master" \
          op monitor interval="31s" role="Slave"

    ms ms_drbd_mysql drbd_mysql \
          meta master-max="1" master-node-max="1" \
            clone-max="2" clone-node-max="1" \
            notify="true"

    primitive fs_mysql ocf:heartbeat:Filesystem \
          params device="/dev/drbd/by-res/r0" \
          directory="/var/lib/mysql" fstype="ext3"

    primitive mysqld lsb:mysql

    group mysql fs_mysql mysqld

    colocation mysql_on_drbd \
           inf: mysql ms_drbd_mysql:Master

    order mysql_after_drbd \
           inf: ms_drbd_mysql:promote mysql:start

    property stonith-enabled=false

    property no-quorum-policy=ignore

Und jetzt in der Konfiguration (Sie müssen nur im Knoten):

    sudo crm configure
      load update /tmp/cluster.conf
      commit
      exit

Außerdem Achten Sie Schrittmacher Systemstart in beiden Knoten beginnt:

    sudo update-rc.d pacemaker defaults

Nach wenigen Sekunden und `sudo crm_mon –L`, Überprüfen eines Knoten für den Cluster wurde werden alle Ressourcen ausgeführt wird. Sie können Mount und Ps Ressourcen ausgeführt werden.

Der folgende Screenshot zeigt `crm_mon` mit einem Knoten beendet (Beenden mit STRG-C)

![Crm_mon-Knoten angehalten](media/virtual-machines-linux-classic-mysql-cluster/image002.png)

Und dieses Bildschirmabbild zeigt beide Knoten mit einem Master und einem Slave:

![Crm_mon betriebliche Master-slave](media/virtual-machines-linux-classic-mysql-cluster/image003.png)

## <a name="testing"></a>Testen

Wir sind bereit für ein automatisches Failover-Simulation. Es gibt zwei Methoden dazu: Weich- und. Sanfte Weise mithilfe der Cluster Herunterfahrfunktion: ``crm_standby -U `uname -n` -v on``. Mit diesem auf dem Master, übernimmt Slave. Vergessen Sie nicht, wieder auf off gesetzt (Crm_mon informiert Sie Knoten ist andernfalls)

Die ist Art heruntergefahren primäre VM (hadb01) über das Portal oder Runlevel auf dem virtuellen Computer (anhalten, beenden) ändern, dann Corosync und Schrittmacher wir unterstützen durch Signalisieren des Masters gehen. Wir können dies testen (nützlich für Wartungsfenster), aber wir können das Szenario auch erzwingen, indem nur die VM fixieren.

## <a name="stonith"></a>STONITH

Es sollte möglich VM Herunterfahren über CLI Azure anstelle eines STONITH-Skripts, die ein Gerät steuert. Sie können `/usr/lib/stonith/plugins/external/ssh` als Basis aktivieren STONITH in der Clusterkonfiguration. Azure CLI Global installiert werden soll und das Veröffentlichen Einstellungsprofil für Benutzer des Clusters geladen werden sollen.

Beispielcode für die Ressource auf [GitHub](https://github.com/bureado/aztonith). Müssen Sie den Cluster-Konfiguration ändern, indem Sie folgenden `sudo crm configure`:

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

**Hinweis:** das Skript nicht ausführen, oben/unten überprüft. Die ursprüngliche SSH Ressource hat 15 Ping überprüft kann Wiederherstellungszeit für eine Azure-VM jedoch variabler.

## <a name="limitations"></a>Grenzen

Wenden die folgenden Nachteile:

- Das Ressourcenskript Certified DRBD, die als Ressource in Schrittmacher verwendet DRBD verwaltet `drbdadm down` Wenn ein Knoten heruntergefahren, auch wenn der Knoten nur standby. Dies ist nicht ideal, da die Slave nicht DRBD Ressource synchronisieren wird Master schreibt ab. Wenn der Master nicht freundlicherweise fehlschlägt, kann eine ältere Dateisystem Zustand Slave übernehmen. Es gibt zwei Möglichkeiten von dieser Lösung:
  - Erzwingen einer `drbdadm up r0` in allen Clusterknoten über lokale (nicht clusterized) Watchdog oder
  - Sicherstellen, dass Skript bearbeiten Certified DRBD `down` nicht aufgerufen wird, in `/usr/lib/ocf/resource.d/linbit/drbd`.
- Lastenausgleich benötigt mindestens 5 Sekunden reagieren so Anwendung clusterfähig und toleranter Timeout. andere Architekturen auch helfen, z. B. in app Warteschlangen Abfrage Middlewares usw..
- MySQL optimieren muss schreiben erfolgt strömten Tempo und Caches so häufig wie möglich zu Gedächtnisverlust Datenträger geleert werden
- Schreiben wird von virtuellen Computer im virtuellen Switch verbinden, ist der Mechanismus zur DRBD Gerät replizieren
