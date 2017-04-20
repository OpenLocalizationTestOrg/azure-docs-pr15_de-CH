<properties
    pageTitle="Einrichten von MySQL auf Linux VM | Microsoft Azure "
    description="Informationen Sie zum Installieren von MySQL-Stapel auf einer virtuellen Linux-Maschine (Ubuntu oder RedHat-Produktfamilie OS) in Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


#<a name="how-to-install-mysql-on-azure"></a>Installieren von MySQL auf Azure


In diesem Artikel erfahren Sie, wie installieren und Konfigurieren von MySQL auf einer Azure virtuellen Maschine unter Linux.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


##<a name="install-mysql-on-your-virtual-machine"></a>MySQL auf dem virtuellen Computer installieren

> [AZURE.NOTE] Sie müssen einen Microsoft Azure virtuellen Computer mit Linux um dieses Lernprogramm abgeschlossen. Siehe [Azure Linux VM Lernprogramm](virtual-machines-linux-quick-create-cli.md) zum Erstellen und Einrichten von Linux VM mit `mysqlnode` als der Name und `azureuser` Benutzer, bevor Sie fortfahren.

In diesem Fall verwenden Sie Port 3306 MySQL-Port.  

Verbinden Sie mit Linux VM über kitten erstellt. Ist dies das erste Mal Azure Linux VM verwenden, finden Sie unter kitten verwenden Linux VM Verbindung [hier](virtual-machines-linux-mac-create-ssh-keys.md).

Wir verwenden den Repository-Paket MySQL5.6 als Beispiel in diesem Artikel installieren. Tatsächlich hat MySQL5.6 weitere Verbesserung der Leistung als MySQL5.5.  Weitere Informationen [hier](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).


###<a name="how-to-install-mysql56-on-ubuntu"></a>Installieren von MySQL5.6 auf Ubuntu
Wir verwenden hier Linux VM mit Ubuntu von Azure.

- Schritt 1: Installation MySQL Server 5.6 Switch auf `root` Benutzer:

            #[azureuser@mysqlnode:~]sudo su -

    Installieren Sie Mysql-Server 5.6:

            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6

    Während der Installation sehen Sie eine Poping im Dialogfeld Fenster bis die Fragen unten MySQL Root-Kennwort festgelegt, und Sie müssen das Kennwort hier festgelegt.

    ![Bild](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p1.png)


    Geben Sie das Kennwort zur Bestätigung erneut ein.

    ![Bild](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p2.png)

- Schritt 2: Anmeldung MySQL Server

    Abschließend MySQL-Server-Installation wird MySQL-Dienst automatisch gestartet. MySQL Server anmelden `root` Benutzer.
    Mit dem folgenden Befehl Login und Eingabe Kennwort.

             #[root@mysqlnode ~]# mysql -uroot -p

- Schritt 3: Verwalten des ausgeführten Diensts MySQL

    (a) Status der MySQL-Dienst abrufen

             #[root@mysqlnode ~]# service mysql status

    (b) MySQL-Dienst starten

             #[root@mysqlnode ~]# service mysql start

    (c) MySQL-Dienst beenden

             #[root@mysqlnode ~]# service mysql stop

    (d) den MySQL-Dienst starten

             #[root@mysqlnode ~]# service mysql restart


###<a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Installieren von MySQL auf Red Hat-Betriebssystemfamilie wie CentOS Oracle Linux
Wir verwenden hier Linux VM CentOS oder Oracle Linux.

- Schritt 1: Hinzufügen von MySQL Yum Repository Switch auf `root` Benutzer:

            #[azureuser@mysqlnode:~]sudo su -

    Downloaden Sie und installieren Sie das Paket MySQL-Version:

            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm

- Schritt 2: Bearbeiten Sie unter Datei MySQL Repository für Download der MySQL5.6 aktivieren.

            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo

    Aktualisieren Sie jeden Wert dieser Datei unten:

        \# *Enable to use MySQL 5.6*

        [mysql56-community]
        name=MySQL 5.6 Community Server

        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/

        enabled=1

        gpgcheck=1

        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

- Schritt 3: Installieren MySQL von MySQL Repository MySQL installieren:

           #[root@mysqlnode ~]#yum install mysql-community-server

    MySQL RPM-Paket und alle zugehörigen Pakete installiert werden.

- Schritt 4: Verwalten des ausgeführten Diensts MySQL

    (a) überprüfen Sie Dienst MySQL-Server:

           #[root@mysqlnode ~]#service mysqld status

    (b) überprüfen Sie, ob standardmäßig Port MySQL Server ausgeführt wird:

           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306


    (c) starten Sie den MySQL-Server:

           #[root@mysqlnode ~]#service mysqld start

    (d) stoppen Sie den MySQL-Server:

           #[root@mysqlnode ~]#service mysqld stop

    (e) Set MySQL beim Starten des Bootvorgangs:

           #[root@mysqlnode ~]#chkconfig mysqld on


###<a name="how-to-install-mysql-on-suse-linux"></a>Installieren von MySQL auf SUSE Linux
Wir verwenden hier Linux VM mit OpenSUSE.

- Schritt 1: Downloaden und Installieren von MySQL-Server

    Wechseln Sie zu `root` Benutzer über folgenden Befehl:  

           #sudo su -

    Downloaden und Installieren von MySQL-Paket:

           #[root@mysqlnode ~]# zypper update

           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql

- Schritt 2: Laufenden MySQL-Dienst verwalten

    (a) Überprüfen der MySQL-Server:

           #[root@mysqlnode ~]# rcmysql status

    (b) überprüfen, ob der Standardport der MySQL-Server:

           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306


    (c) starten Sie den MySQL-Server:

           #[root@mysqlnode ~]# rcmysql start

    (d) stoppen Sie den MySQL-Server:

           #[root@mysqlnode ~]# rcmysql stop

    (e) Set MySQL beim Starten des Bootvorgangs:

           #[root@mysqlnode ~]# insserv mysql

###<a name="next-step"></a>Nächstes
Finden Sie weitere Verwendung und Informationen MySQL [hier](https://www.mysql.com/).
