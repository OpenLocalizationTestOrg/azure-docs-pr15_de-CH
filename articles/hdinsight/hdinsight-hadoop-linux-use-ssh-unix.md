<properties
   pageTitle="Verwenden Sie SSH-Schlüssel mit Linux-basierten Hadoop von Linux, Unix oder OS X | Microsoft Azure"
   description=" Linux-basierte HDInsight mit Secure Shell (SSH) möglich. Dieses Dokument enthält Informationen zum Verwenden von SSH auf Linux, Unix oder OS X Clients mit HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/13/2016"
   ms.author="larryfr"/>

#<a name="use-ssh-with-linux-based-hadoop-on-hdinsight-from-linux-unix-or-os-x"></a>Verwenden Sie SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix und Mac OS

> [AZURE.SELECTOR]
- [Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
- [Linux, Unix, Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) können Sie Remote auf Linux-basierten HDInsight Cluster über eine Befehlszeilenschnittstelle ausführen. Dieses Dokument enthält Informationen zum Verwenden von SSH auf Linux, Unix oder OS X Clients mit HDInsight.

> [AZURE.NOTE] Die Schritte in diesem Artikel angenommen, Linux, Unix oder OS X-Clients verwenden. Folgendermaßen können auf einem Windows-basierten Client ausgeführt werden, wenn Sie ein Paket installiert haben, das bietet `ssh` und `ssh-keygen`, z. B. [Bash auf Ubuntu unter Windows](https://msdn.microsoft.com/commandline/wsl/about).
>
> Wenn Sie nicht SSH auf Ihrem Windows-basierten Client installiert haben, gehen Sie in [Verwenden SSH mit Linux-basierte HDInsight (Hadoop) von Windows](hdinsight-hadoop-linux-use-ssh-windows.md) Informationen zur Installation und Verwendung kitten.

##<a name="prerequisites"></a>Erforderliche Komponenten

* **SSH-Keygen** und **ssh** für Linux, Unix und Mac OS-Clients. Diese Dienstprogramme werden normalerweise mit dem Betriebssystem oder über das Verwaltungssystem bereitgestellt.

* Einen modernen Webbrowser, der HTML5 unterstützt.

ODER

* [Azure CLI](../xplat-cli-install.md).

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##<a name="what-is-ssh"></a>Was ist SSH?

SSH ist ein Dienstprogramm zum Anmelden und Remote auf einem Remoteserver Befehle ausführen. Linux-basierte HDInsight SSH richtet eine verschlüsselte Verbindung mit Cluster Hauptknoten und stellt einen Befehl, mit dem Sie Befehle eingeben. Befehle werden dann direkt auf dem Server ausgeführt.

###<a name="ssh-user-name"></a>SSH-Benutzername

Ein SSH-Benutzername ist HDInsight Cluster Authentifizierung verwendete Name. Bei Angabe ein Benutzernamens SSH während der Clustererstellung ist dieser Benutzer auf allen Knoten im Cluster erstellt. Nach Erstellung des Clusters können dieser Benutzername Sie HDInsight Cluster Headnodes herstellen. Von der Headnodes können Sie dann einzelne Arbeitskraft Knoten verbinden.

###<a name="ssh-password-or-public-key"></a>SSH-Kennwort oder öffentlichen Schlüssel

Ein SSH-Benutzer können entweder ein Kennwort oder öffentlichen Schlüssel für die Authentifizierung. Ein Kennwort ist nur eine Textzeichenfolge soll, während ein öffentlicher Schlüssel Teil ein kryptografisches Schlüsselpaar generiert, um eindeutig zu identifizieren.

Ein Schlüssel ist sicherer als ein Kennwort erfordert zusätzliche Schritte zum Generieren des Schlüssels, und Sie müssen die Dateien mit dem Schlüssel an einem sicheren Ort. Jeder Benutzer erhält Zugriff auf wichtigen Dateien erhalten sie Zugriff auf Ihr Konto. Oder wenn Sie wichtigen Dateien verlieren, Sie werden nicht auf Ihr Konto anmelden.

Ein Schlüsselpaar besteht aus einem öffentlichen Schlüssel (der HDInsight Server an) und einem privaten Schlüssel (der auf dem Clientcomputer gespeichert wird.) Beim Herstellen einer Verbindung mit dem HDInsight-Server über SSH verwendet SSH-Client den privaten Schlüssel auf Ihrem Computer den Server authentifiziert.

##<a name="create-an-ssh-key"></a>Erstellen Sie einen SSH-Schlüssel

Verwenden Sie Folgendes, wenn Sie mit Ihrem SSH-Schlüssel verwenden möchten. Wenn Sie ein Kennwort verwenden möchten, können Sie diesen Abschnitt überspringen.

1. Öffnen Sie ein terminal, und verwenden Sie den folgenden Befehl, um festzustellen, ob alle vorhandenen SSH-Schlüssel:

        ls -al ~/.ssh

    Suchen Sie die folgenden Dateien im Verzeichnis. Dies sind allgemeine Namen für Öffentliche SSH-Schlüssel.

    * ID\_dsa.pub
    * ID\_ecdsa.pub
    * ID\_ed25519.pub
    * ID\_rsa.pub

2. Wenn Ihnen keine vorhandenen SSH-Schlüssel keine vorhandene Datei verwenden möchten, verwenden Sie folgende eine neue Datei generieren:

        ssh-keygen -t rsa

    Sie werden aufgefordert, die folgenden Informationen:

    * Die Speicherort - der Speicherort standardmäßig ~/.ssh/id\_Rsa.
    * Eine Passphrase - werden Sie aufgefordert, diese erneut eingeben.

        > [AZURE.NOTE] Es wird dringend empfohlen, dass Sie eine sichere Passphrase für den Schlüssel verwenden. Wenn Sie das Kennwort vergessen, gibt jedoch keine Möglichkeit, die Datenbank wiederherzustellen.

    Nachdem der Befehl abgeschlossen ist, müssen Sie zwei neue Dateien, den privaten Schlüssel (z. B. **Id\_Rsa**) und den öffentlichen Schlüssel (z. B. **Id\_rsa.pub**).

##<a name="create-a-linux-based-hdinsight-cluster"></a>Erstellen Sie einen Linux-basierten HDInsight-cluster

Beim Erstellen eines Linux-basierte HDInsight-Clusters müssen Sie den zuvor erstellten öffentlichen Schlüssel angeben. Linux, Unix oder OS X-Clients werden zwei Methoden zum Erstellen eines Clusters HDInsight:

* **Azure-Portal** - verwendet Web-basierte Portal Erstellen des Clusters.

* **CLI für Mac, Linux und Windows Azure** - verwendet Befehlen Erstellen des Clusters.

Jede dieser Methoden erfordert ein Kennwort oder einen öffentlichen Schlüssel. Vollständige Informationen zum Erstellen eines Linux-basierte HDInsight-Clusters finden Sie unter [Bereitstellung von Linux-basierten HDInsight-Cluster](hdinsight-hadoop-provision-linux-clusters.md).

###<a name="azure-portal"></a>Azure-Portal

Verwendung der [Azure-Portal] [ preview-portal] zum Erstellen eines Clusters Linux-basierten HDInsight müssen Sie geben einen **SSH-Benutzernamen**und ein **Kennwort** oder eine **Öffentliche SSH-Schlüssel**eingeben.

Bei Auswahl der **Öffentliche SSH-Schlüssel**können Sie __SSH PublicKey__ -Feld einfügen den öffentlichen Schlüssel (in der Datei mit der Erweiterung **pub** ) oder klicken Sie auf Durchsuchen und Auswählen der Datei für den öffentlichen Schlüssel __eine Datei auswählen__ .

![Bild des Formular für öffentliche Schlüssel](./media/hdinsight-hadoop-linux-use-ssh-unix/ssh-key.png)

> [AZURE.NOTE] Die Schlüsseldatei ist lediglich eine Textdatei. Der Inhalt sollte etwa wie folgt aussehen:
> ```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCelfkjrpYHYiks4TM+r1LVsTYQ4jAXXGeOAF9Vv/KGz90pgMk3VRJk4PEUSELfXKxP3NtsVwLVPN1l09utI/tKHQ6WL3qy89WVVVLiwzL7tfJ2B08Gmcw8mC/YoieT/YG+4I4oAgPEmim+6/F9S0lU2I2CuFBX9JzauX8n1Y9kWzTARST+ERx2hysyA5ObLv97Xe4C2CQvGE01LGAXkw2ffP9vI+emUM+VeYrf0q3w/b1o/COKbFVZ2IpEcJ8G2SLlNsHWXofWhOKQRi64TMxT7LLoohD61q2aWNKdaE4oQdiuo8TGnt4zWLEPjzjIYIEIZGk00HiQD+KCB5pxoVtp user@system
> ```

Dies erstellt einen Benutzernamen für den angegebenen Benutzer mit dem Kennwort oder öffentliche Schlüssel, die Sie bereitstellen.

###<a name="azure-command-line-interface-for-mac-linux-and-windows"></a>Befehlszeilenschnittstelle für Mac, Linux und Windows Azure

Können Sie einen neuen Cluster erstellen [CLI für Mac, Linux und Windows Azure](../xplat-cli-install.md) die `azure hdinsight cluster create` Befehl.

Weitere Informationen zum Verwenden dieses Befehls finden Sie unter [Bereitstellung Hadoop Linux-Clustern in HDInsight mit benutzerdefinierten Optionen](hdinsight-hadoop-provision-linux-clusters.md).

##<a name="connect-to-a-linux-based-hdinsight-cluster"></a>Linux-basierte HDInsight Cluster verbinden

Von einer terminal Sitzung Befehl SSH auf den Hauptknoten Cluster zugreifen, indem Sie die Adresse und den Namen:

* **SSH-Adresse** – zwei Adressen, die Verbindung zu einem Cluster über SSH verwendet werden können:

    * **Mit dem Hauptknoten**: Clustername gefolgt von **--SSH.azurehdinsight.NET**. Z. B. **MeinCluster-SSH.azurehdinsight.NET**.
    
    * **Mit der edgeknoten**: Wenn Cluster Server auf HDInsight R ist Cluster enthält außerdem einen Kantenknoten mit **RServer.CLUSTERNAME.ssh.azurehdinsight.net**, wobei __CLUSTERNAME__ der Name des Clusters ist zugegriffen werden kann.

* **Benutzername** - der SSH Benutzer eingegebenen Namen bei der Erstellung des Clusters.

Im folgende Beispiel wird die primäre Hauptknoten **MeinCluster** als Benutzer **mir**herstellen:

    ssh me@mycluster-ssh.azurehdinsight.net

Wenn Sie ein Kennwort für das Benutzerkonto verwendet, werden Sie aufgefordert, das Kennwort einzugeben.

Wenn SSH-Schlüssel, der gesichert ist mit einer Passphrase verwendet, werden Sie aufgefordert, das Kennwort einzugeben. Andernfalls versucht SSH automatisch anhand eines lokalen privaten Schlüssel auf dem Client zu authentifizieren.

> [AZURE.NOTE] Wenn SSH nicht automatisch mit dem richtigen privaten Schlüssel authentifiziert, verwenden Sie den Parameter **-i** und geben Sie den Pfad zum privaten Schlüssel. Das folgende Beispiel lädt den privaten Schlüssel aus `~/.ssh/id_rsa`:
>
> `ssh -i ~/.ssh/id_rsa me@mycluster-ssh.azurehdinsight.net`

Wenn Sie Verbinden mit der Adresse für den Hauptknoten und kein Port angegeben, wird SSH Port 22 standardmäßig die primäre Hauptknoten auf dem HDInsight-Cluster verbunden wird. Verwenden Sie Port 23 verbindet Sie mit der sekundären. Weitere Informationen zu den Headnodes finden Sie unter [Verfügbarkeit und Zuverlässigkeit von Hadoop Cluster in HDInsight](hdinsight-high-availability-linux.md).

###<a name="connect-to-worker-nodes"></a>Arbeitskraft Knoten verbinden

Workerknoten sind nicht direkt von außerhalb der Azure-Rechenzentrum von Cluster Hauptknoten über SSH zugegriffen werden

Wenn Sie SSH-Schlüssel verwenden, um Ihr Benutzerkonto authentifizieren, müssen Sie auf dem Client die folgenden Schritte aus:

1. Öffnen Sie einen Texteditor, mit `~/.ssh/config`. Wenn diese Datei vorhanden ist, können sie durch Eingabe von `touch ~/.ssh/config` im Terminal.

2. Fügen Sie Folgendes in die Datei. Der Name des Clusters HDInsight ersetzen Sie *CLUSTERNAME* .

        Host CLUSTERNAME-ssh.azurehdinsight.net
          ForwardAgent yes

    SSH-Agent weiterleiten für Ihren HDInsight wird konfiguriert.

3. Testen Sie SSH-Agent mit dem folgenden Befehl vom Terminal weiterleiten:

        echo "$SSH_AUTH_SOCK"

    Diese sollten Informationen ähnlich der folgenden zurück:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Wenn nothing zurückgegeben wird, bedeutet dies, dass der **ssh-Agent** nicht ausgeführt wird. Dokumentation Ihr Betriebssystem spezifische Schritte zur Installation und Konfiguration von **ssh-Agent**oder unter [Verwendung von ssh-Agent mit ssh](http://mah.everybody.org/docs/ssh).

4. Nachdem Sie sichergestellt haben, dass der **ssh-Agent** ausgeführt wird, verwenden Sie folgende der Agent den SSH-Schlüssel hinzugefügt:

        ssh-add ~/.ssh/id_rsa

    Der private Schlüssel in einer anderen Datei gespeichert ist, ersetzen Sie `~/.ssh/id_rsa` durch den Pfad zu der Datei.

Gehen Sie die Arbeitskraft Knoten im Cluster herstellen.

> [AZURE.IMPORTANT] Verwenden Sie SSH-Schlüssel zu Ihrem Konto authentifizieren, müssen Sie die vorherigen Schritte, um sicherzustellen, dass der Agent Weiterleiten funktioniert.

1. Eine Verbindung zu der HDInsight-Cluster über SSH, wie zuvor beschrieben.

2. Sobald Sie verbunden sind, verwenden Sie Folgendes zum Abrufen einer Liste der Knoten im Cluster. Das Kennwort für Ihr Administratorkonto Cluster ersetzen Sie *ADMINPASSWORD* . Der Name des Clusters ersetzen Sie *CLUSTERNAME* .

        curl --user admin:ADMINPASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/hosts

    Dies gibt Informationen im JSON-Format für die Knoten im Cluster einschließlich `host_name`, den vollqualifizierten Domänennamen (FQDN) für jeden Knoten enthält. Im folgenden ist ein Beispiel für eine `host_name` Eintrag **curl** -Befehl zurückgegeben:

        "host_name" : "workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net"

3. Haben Sie eine Liste der Arbeitskraft Knoten herstellen möchten, verwenden Sie folgenden Befehl SSH-Sitzung auf dem Server eine Verbindung mit einem workerknoten:

        ssh USERNAME@FQDN

    SSH-Benutzername und *FQDN* mit dem vollqualifizierten Domänennamen für die Arbeitskraft Knoten ersetzen Sie *Benutzernamen* . Z. B. `workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net`.

    > [AZURE.NOTE] Verwenden Sie ein Kennwort zur Authentifizierung der SSH-Sitzung, werden Sie aufgefordert, das Kennwort erneut einzugeben. Wenn Sie SSH-Schlüssel verwenden, sollte die Verbindung ohne Aufforderung abgeschlossen werden.

4. Nach Einrichtung der Sitzung ändern terminal Aufforderung von `username@hn#-clustername` , `username@wk#-clustername` an, dass die Arbeitskraft Knoten verbunden sind. Zu diesem Zeitpunkt ausgeführten Befehle werden auf Arbeitskraft Knoten ausgeführt.

4. Nach dem Ausführen von Aktionen für die Arbeitskraft Knoten verwenden die `exit` Befehl zum Schließen der Sitzung an den workerknoten. Erhalten Sie die `username@hn#-clustername` aufgefordert.

## <a name="connect-to-a-domain-joined-hdinsight-cluster"></a>Verbinden Sie mit einem Cluster Domäne HDInsight

[Domäne HDInsight](hdinsight-domain-joined-introduction.md) integriert Hadoop in HDInsight Kerberos. Da der Benutzer SSH kein Benutzer Active Directory-Domäne ist, kann nicht dieses Benutzerkonto Hadoop Befehle über SSH-Shell auf einem Cluster Domäne direkt ausführen. Sie müssen zuerst *Kinit* ausführen. 

**Struktur ausgeführt Abfragen für eine Domäne HDInsight Cluster über SSH**

1. Verbinden Sie mit einer Domäne HDInsight Cluster über SSH.  Instrocutions finden Sie unter [Verbinden einer Linux-basierten HDInsight Cluster](#connect-to-a-linux-based-hdinsight-cluster).
2. Führen Sie Kinit. Es fragt Sie für Domänenbenutzername und Domäne Benutzerkennwort. Weitere Informationen zum Konfigurieren Sie Domänenbenutzer für Domäne HDInsight Cluster, finden Sie unter [Konfigurieren Domäne HDInisight Cluster](hdinsight-domain-joined-configure.md).

    ![HDInsight Hadoop Domäne kinit](./media/hdinsight-hadoop-linux-use-ssh-unix/hdinsight-domain-joined-hadoop-kinit.png)
3. Öffnen Sie die Konsole von Struktur:

        hive

    Anschließend können Sie die Struktur Befehle ausführen.

##<a name="add-more-accounts"></a>Weitere Konten hinzufügen

1. Generieren Sie einen neuen öffentlichen und privaten Schlüssel für das neue Benutzerkonto wie im Abschnitt [Erstellen einer SSH-Schlüssel](#create-an-ssh-key-optional) .

    > [AZURE.NOTE] Der private Schlüssel sollte entweder auf einem Client generiert, die der Benutzer verwendet zum Herstellen oder nach der Erstellung sicher an einen Client übertragen.

1. Fügen Sie aus einer SSH-Sitzung zum Cluster neuen Benutzer mit dem folgenden Befehl hinzu:

        sudo adduser --disabled-password <username>

    Erstellen eines neuen Benutzerkontos dies Kennwortauthentifizierung deaktiviert.

2. Erstellen Sie das Verzeichnis und Dateien zu den Schlüssel mithilfe der folgenden Befehle:

        sudo mkdir -p /home/<username>/.ssh
        sudo touch /home/<username>/.ssh/authorized_keys
        sudo nano /home/<username>/.ssh/authorized_keys

3. Beim Öffnen des Nano-Editors kopieren und Einfügen in den Inhalt des öffentlichen Schlüssels für das neue Benutzerkonto. Schließlich verwenden Sie **STRG + X** , speichern Sie die Datei und beenden Sie den Editor.

    ![Bild der Nano-Editor mit Beispiel](./media/hdinsight-hadoop-linux-use-ssh-unix/nano.png)

4. Verwenden Sie den folgenden Befehl an .ssh Ordner und Inhalt des neuen Benutzerkontos ändern:

        sudo chown -hR <username>:<username> /home/<username>/.ssh

5. Jetzt werden an den Server mit dem neuen Benutzerkonto und privaten Schlüssel authentifiziert.

##<a id="tunnel"></a>SSH-tunneling

SSH kann lokale Anfragen wie Webanfragen HDInsight Cluster Tunnel verwendet werden. Die Anforderung wird dann auf die angeforderte Ressource weitergeleitet auf HDInsight Cluster Hauptknoten stammen hatte.

> [AZURE.IMPORTANT] SSH-Tunnel ist eine Voraussetzung für den Zugriff auf die Webbenutzeroberfläche für einige Dienste Hadoop. Z. B. der Auftrag Geschichte Benutzeroberfläche oder Ressourcenmanager UI nur möglich mit einem SSH-Tunnel.

Weitere Informationen zum Erstellen und Verwenden eines SSH-Tunnels finden Sie unter [Verwenden SSH Tunneling auf Ambari Webbenutzeroberfläche ResourceManager, JobHistory, NameNode, Oozie, und andere Webbenutzeroberfläche](hdinsight-linux-ambari-ssh-tunnel.md).

##<a name="next-steps"></a>Nächste Schritte

Damit Sie verstehen wie die Authentifizierung mithilfe eines SSH-Schlüssels, erfahren Sie MapReduce Hadoop auf HDInsight verwenden.

* [Struktur mit HDInsight verwenden](hdinsight-use-hive.md)

* [Verwenden Sie Schwein mit HDInsight](hdinsight-use-pig.md)

* [Verwenden Sie MapReduce Aufträge mit HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
