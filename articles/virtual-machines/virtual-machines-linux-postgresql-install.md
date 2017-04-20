<properties
    pageTitle="Einrichten von PostgreSQL auf Linux VM | Microsoft Azure"
    description="Informationen Sie zum Installieren und Konfigurieren von PostgreSQL auf einer virtuellen Linux-Maschine in Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


# <a name="install-and-configure-postgresql-on-azure"></a>Installieren und Konfigurieren von PostgreSQL auf Azure

PostgreSQL ist eine erweiterte Open Source-Datenbank wie Oracle und DB2. Er enthält Funktionen für Unternehmen vollständige ACID Compliance und zuverlässige transaktionale Verarbeitung versionsübergreifende Parallelität. Darüber hinaus unterstützt Standards wie ANSI SQL und SQL/MED (einschließlich fremden Daten Wrapper für Oracle, MySQL MongoDB und viele andere). Es ist äußerst erweiterbare mit Unterstützung von über 12 Verfahrenssprachen, GIN und den Kern Geodaten-Unterstützung und mehrere NoSQL-ähnliche Funktionen für JSON oder Schlüssel-Wert-basierte.

In diesem Artikel erfahren Sie, wie installieren und Konfigurieren von PostgreSQL auf einer Azure virtuellen Maschine unter Linux.


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="install-postgresql"></a>PostgreSQL installieren

> [AZURE.NOTE] Sie müssen einen virtuellen Azure-Computer mit Linux um dieses Lernprogramm abgeschlossen. Erstellen und Einrichten einer Linux VM vor finden Sie im [Lernprogramm Azure Linux VM](virtual-machines-linux-quick-create-cli.md).

In diesem Fall verwenden Sie Port 1999 als PostgreSQL-Port.  

Verbinden Sie mit Linux VM über kitten erstellt. Ist dies das erste Mal eine Azure Linux VM verwenden, finden Sie unter [wie verwenden SSH mit Linux auf Azure](virtual-machines-linux-mac-create-ssh-keys.md) kitten Verbindung mit Linux VM erlernen.

1. Führen Sie den folgenden Befehl in das Stammverzeichnis (Admin) wechseln:

        # sudo su -

2. Einige Distributionen sind abhängig, die Sie vor der Installation von PostgreSQL installieren. Überprüfen Sie Ihre Distribution in dieser Liste und führen Sie den entsprechenden Befehl aus:

    - Base Red Hat Linux:

            # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

    - Debian Linux Basis:

            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  

    - SUSE Linux:

            # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

3. Herunterladen Sie PostgreSQL in das Stammverzeichnis und Entpacken Sie das Paket:

        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/

        # tar jxvf  postgresql-9.3.5.tar.bz2

    Ein Beispiel ist. Weitere Download-Adresse finden Sie im [Index der pub/Quelle](https://ftp.postgresql.org/pub/source/).

4. Führen Sie diese Befehle aus, um den Build zu starten:

        # cd postgresql-9.3.5

        # ./configure --prefix=/opt/postgresql-9.3.5

5. Führen Sie alles erstellen, die erstellt werden können, einschließlich der Dokumentation (HTML- und Man-Seiten) und zusätzliche Module (für die Altersvorsorge), stattdessen den folgenden Befehl:

        # gmake install-world

    Sie erhalten die folgende Meldung angezeigt:

        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>PostgreSQL konfigurieren

1. (Optional) Erstellen einer symbolischen Verknüpfung kürzen Sie PostgreSQL-Verweis, um die Versionsnummer nicht enthalten:

        # ln -s /opt/pgsql9.3.5 /opt/pgsql

2. Erstellen Sie ein Verzeichnis für die Datenbank:

        # mkdir -p /opt/pgsql_data

3. Einen nicht-Root-Benutzer erstellen und Profil des Benutzers ändern. Schalten Sie diese neue Benutzer (in unserem Beispiel *Postgres* genannt):

        # useradd postgres

        # chown -R postgres.postgres /opt/pgsql_data

        # su - postgres

   > [AZURE.NOTE] Aus Sicherheitsgründen verwendet PostgreSQL-Root-Benutzer zu initialisieren, starten oder Herunterfahren der Datenbank.


4. Bearbeiten Sie die Datei *Bash_profile* die folgenden Befehle eingeben. Diese Zeilen werden am Ende der Datei *Bash_profile* hinzugefügt:

        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF

5. Führen Sie die Datei *Bash_profile* :

        $ source .bash_profile

6. Überprüfen Sie die Installation mit dem folgenden Befehl:

        $ which psql

    Wenn die Installation erfolgreich ist, wird Folgendes angezeigt:

        /opt/pgsql/bin/psql

7. Sie können auch die PostgreSQL-Version überprüfen:

        $ psql -V

8. Initialisieren Sie die Datenbank:

        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W

    Sie erhalten die folgende Ausgabe:

![Bild](./media/virtual-machines-linux-postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>PostgreSQL einrichten

<!--    [postgres@ test ~]$ exit -->

Führen Sie die folgenden Befehle:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Ändern Sie zwei Variablen in der Datei /etc/init.d/postgresql. Das Präfix des Installationspfads PostgreSQL festgelegt ist: **/opt/pgsql**. PGDATA auf Daten Speicherpfad PostgreSQL festgelegt ist: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Bild](./media/virtual-machines-linux-postgresql-install/no2.png)

Ändern Sie zu ausführbaren Datei.

    # chmod +x /etc/init.d/postgresql

PostgreSQL zu starten:

    # /etc/init.d/postgresql start

Überprüfen Sie, ob der Endpunkt PostgreSQL auf:

    # netstat -tunlp|grep 1999

Die folgende Ausgabe sollte angezeigt werden:

![Bild](./media/virtual-machines-linux-postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Verbinden Sie mit der Datenbank Postgres

Wechseln Sie wieder zu Postgres Benutzer:

    # su - postgres

Erstellen einer Datenbank Postgres:

    $ createdb events

Verbinden Sie mit der Datenbank Ereignisse, die Sie gerade erstellt haben:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Erstellen oder Löschen einer Tabelle Postgres

Nun, die Verbindung zur Datenbank hergestellt haben, können Sie es Tabellen erstellen.

Erstellen Sie eine neue Beispiel Postgres Tabelle z. B. mit dem folgenden Befehl:

    CREATE TABLE potluck (name VARCHAR(20), food VARCHAR(30),   confirmed CHAR(1), signup_date DATE);

Sie haben jetzt vierspaltige Tabelle mit der folgenden Spalte Beschränkungen eingerichtet:

1. Die Spalte "Name" wurde vom Befehl VARCHAR 20 Zeichen beschränkt.
2. Die Spalte "Lebensmittel" gibt Lebensmittel, die jeder bringt. VARCHAR wird dieser Text unter 30 Zeichen beschränkt.
3. In der Spalte "bestätigte" aufgezeichnet, ob die Person, die Buffet um Antwort gebeten hat. Die zulässigen Werte lauten "Y" und "N".
4. Der "Date" Spalte wird angezeigt, wenn sie für das Ereignis registriert. Postgres erfordert, dass Daten als Yyyy-mm-dd geschrieben werden

Folgende sollten angezeigt, wenn die Tabelle erfolgreich erstellt wurde:

![Bild](./media/virtual-machines-linux-postgresql-install/no4.png)

Mit dem folgenden Befehl können Sie auch die Tabellenstruktur überprüfen:

![Bild](./media/virtual-machines-linux-postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Hinzufügen von Daten zu einer Tabelle

Zunächst Informationen in einer Zeile einfügen:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Diese Ausgabe sollte angezeigt werden:

![Bild](./media/virtual-machines-linux-postgresql-install/no6.png)

Sie können der Tabelle sowie einigen weitere Personen hinzufügen. Hier sind einige Optionen oder eigene erstellen:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Tabellen anzeigen

Verwenden Sie den folgenden Befehl, um eine Tabelle anzuzeigen:

    select * from potluck;

Die Ausgabe lautet:

![Bild](./media/virtual-machines-linux-postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Löschen von Daten in einer Tabelle

Verwenden Sie den folgenden Befehl zum Löschen von Daten in einer Tabelle:

    delete from potluck where name=’John’;

Dies löscht alle Informationen in der Zeile "John". Die Ausgabe lautet:

![Bild](./media/virtual-machines-linux-postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Aktualisieren von Daten in einer Tabelle

Verwenden Sie den folgenden Befehl zum Aktualisieren von Daten in einer Tabelle. Für diese bestätigt Sandy, dass sie besucht, damit wir ihre Antwort "N", "Y" ändern:

    UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


##<a name="get-more-information-about-postgresql"></a>Informationen Sie weitere zu PostgreSQL
Abschluss die Installation von PostgreSQL in einer Azure Linux VM genießen Sie in Azure verwenden. Über PostgreSQL finden Sie auf der [Website PostgreSQL](http://www.postgresql.org/).
