<properties
pageTitle="Verwenden Sie SSH-Tunneling auf Ambari Webbenutzeroberfläche ResourceManager, JobHistory, NameNode, Oozie und andere Web UI"
description="Erfahren Sie, SSH-Tunnel auf sichere Webressourcen auf Linux-basierten HDInsight Knoten gehostet."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/17/2016"
ms.author="larryfr"/>

# <a name="use-ssh-tunneling-to-access-ambari-web-ui-jobhistory-namenode-oozie-and-other-web-uis"></a>Verwenden Sie SSH-Tunneling auf Ambari Web-Benutzeroberfläche, JobHistory, NameNode, Oozie und andere Web UI

Linux-basierte HDInsight Cluster bieten Zugriff auf Ambari Web-Benutzeroberfläche über das Internet jedoch einige Funktionen der Benutzeroberfläche nicht. Beispielsweise die Webbenutzeroberfläche für andere Dienste, die durch Ambari dargestellt werden. Für vollständige Funktionalität der Ambari Webbenutzeroberfläche verwenden Sie SSH-Tunnel zum Cluster Head.

## <a name="what-requires-an-ssh-tunnel"></a>Was ist einen SSH-Tunnel?

Einige Menüs in Ambari füllt nicht vollständig ohne einen Tunnel SSH wie sie Websites und Dienste von anderen Hadoop-Dienste auf dem Cluster verfügbar gemacht werden. Diese Websites sind oft nicht gesichert, daher es nicht sicher ist, direkt im Internet verfügbar machen. Manchmal wird der Dienst die Website auf einem anderen Knoten wie ein Zookeeper Knoten ausgeführt.

Folgende sind Dienste, die Webbenutzeroberfläche Ambari verwendet, und ohne einen SSH-Tunnel zugegriffen werden können:

* JobHistory,
* NameNode,
* Thread-Stacks
* Oozie Web-Benutzeroberfläche
* HBase Master und Protokolle Benutzeroberfläche

Verwenden Sie Skriptaktionen Cluster anpassen, benötigen alle Dienste und Dienstprogramme, die Sie installieren, die eine Webbenutzeroberfläche bereitstellen einen SSH-Tunnel. Z. B. Installation Farbton mit einer Aktion Skript verwenden SSH-Tunnel Sie Farbton Webbenutzeroberfläche auf.

## <a name="what-is-an-ssh-tunnel"></a>Was ist ein Tunnel SSH?

[Secure Shell (SSH) tunneling](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) leitet Datenverkehr an einen Port auf der lokalen Arbeitsstation über eine SSH-Verbindung zu Ihrem HDInsight Cluster Head-Knoten, in denen die Anforderung auf dem Head-Knoten stammen dann behoben ist. Die Antwort wird dann durch den Tunnel an Ihrer Arbeitsstation weitergeleitet.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bei Verwendung ein SSH-Tunnels für Webdatenverkehr benötigen Sie Folgendes:

* SSH-Client. Verteilung von Linux und Unix oder Macintosh OS X die `ssh` Befehl wird mit dem Betriebssystem bereitgestellt. Unter Windows sollten [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

    > [AZURE.NOTE] SSH-Client verwenden soll `ssh` oder kitten bitte die Dokumentation für Ihren Client auf einen SSH-Tunnel einrichten.

* Ein Webbrowser konfiguriert einen SOCKS-Proxyserver

## <a name="usessh"></a>Erstellen Sie mit dem Befehl SSH tunnel

Verwenden Sie der folgende Befehl zum Erstellen einer SSH tunnel mit der `ssh` Befehl. Ersetzen Sie __Benutzernamen__ mit einem SSH-Benutzer für den HDInsight-Cluster, und __CLUSTERNAME__ durch den Namen des Clusters HDInsight

    ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

Dies erstellt eine Verbindung, die Datenverkehr am lokalen Port 9876 Cluster über SSH weiterleitet. Die Optionen sind:

* **D 9876** - der lokale Port, die durch den Tunnel Datenverkehr weiterleiten.

* **C** - alle Daten komprimieren, da Webdatenverkehr hauptsächlich Text ist.

* **2** - Force SSH-Protokoll, Version 2 nur versuchen.

* **Q** - Stiller Modus.

* **T** - deaktivieren Pseudo-tty-Zuweisung, da wir nur einen Port weitergeleitet.

* **n** - Lesen von STDIN, verhindern, da wir nur einen Port weitergeleitet.

* **N** - remote-Befehl nicht ausführen, da wir nur einen Port weitergeleitet.

* **f** - im Hintergrund ausgeführt.

Wenn Sie mit einem SSH-Schlüssel des Clusters konfiguriert, möglicherweise mit den `-i` Parameter und geben Sie den Pfad zum privaten SSH-Schlüssel.

Nachdem der Befehl abgeschlossen ist, Datenverkehr auf Port 9876 auf dem lokalen Computer über weitergeleitet werden, Secure Sockets Layer (SSL) zum Cluster head-Knoten und offenbar vorhanden.

## <a name="useputty"></a>Erstellen eines Tunnels mit kitten

Gehen Sie folgendermaßen vor, um ein kitten mit SSH-Tunnel zu erstellen.

1. Öffnen Sie kitten, und geben Sie die Verbindungsinformationen. Wenn Sie nicht mit kitten vertraut sind, finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) Informationen mit HDInsight verwendet.

2. Im Abschnitt **Kategorien** links im Dialogfeld erweitern Sie **Verbindung** **SSH**, und wählen Sie dann **Tunnel**.

3. Angaben Sie folgende im Formular **Optionen zum Steuern der SSH Port weiterleiten** :

    * **Ursprungs-Port** - Port auf dem Client, den Sie weiterleiten möchten. Z. B. **9876**.

    * **Ziel** - die SSH-Adresse für den Cluster Linux-basierten HDInsight. Z. B. **MeinCluster-SSH.azurehdinsight.NET**.

    * **Dynamische** - ermöglicht dynamische SOCKS-Proxy weiterleiten.

    ![Bild von Tunneling-Optionen](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. Klicken Sie auf **Hinzufügen** , um die Einstellungen hinzufügen, und klicken Sie auf **Öffnen** , um eine SSH-Verbindung zu öffnen.

5. Wenn Sie aufgefordert werden, melden Sie sich an den Server an. Eine SSH-Sitzung einrichten wird und aktivieren den Tunnel.

## <a name="use-the-tunnel-from-your-browser"></a>Der Tunnel vom browser

> [AZURE.NOTE] Die Schritte in diesem Abschnitt verwenden den FireFox-Browser für Linux, Unix und Macintosh OS X Windows Systeme frei verfügbar ist. Andere Browser Unterstützung über einen SOCKS-Proxyserver arbeiten.

1. Konfigurieren Sie den Browser, um **Localhost:9876** als **v5 SOCKS-** Proxy verwenden. Hier wird die Firefox-Einstellungen aussehen. Wenn Sie einen anderen Anschluss als 9876 verwendet, ändern Sie den Port zu verwendeten:

    ![Bild der Firefox-Einstellungen](./media/hdinsight-linux-ambari-ssh-tunnel/socks.png)

    > [AZURE.NOTE] Auswählen von **Remote-DNS-** auflösen Anfragen (DNS = Domain Name System) HDInsight Cluster. Wenn dies deaktiviert ist, wird DNS lokal aufgelöst werden.

2. Stellen Sie sicher, dass Datenverkehr durch den Tunnel weitergeleitet wird durch eine Website wie [http://www.whatismyip.com/](http://www.whatismyip.com/) mit Proxyeinstellungen aktiviert und deaktiviert in Firefox Überwachungsgerät. Während die Einstellung aktiviert ist, werden die IP-Adresse für einen Computer im Microsoft Azure-Rechenzentrum.

##<a name="verify-with-ambari-web-ui"></a>Überprüfen Sie mit Ambari Web-Benutzeroberfläche

Nach der Cluster eingerichtet wurde, gehen Sie folgendermaßen vor, um sicherzustellen, dass Sie Ambari Web Service Web UIs zugreifen können:

1. Gehen Sie in Ihrem Browser Http://headnodehost:8080. Die `headnodehost` Adresse über den Tunnel an den Cluster gesendeten und der Ambari läuft Hauptknoten beheben. Bei Aufforderung geben Sie den Benutzernamen Administrator (Admin) und das Kennwort für den Cluster. Der Ambari Web-Benutzeroberfläche können Sie die erneut aufgefordert. In diesem Fall geben Sie die Informationen.
    
    > [AZURE.NOTE] Wenn die Adresse Http://headnodehost:8080 herstellen, werden Sie verbinden direkt über den Tunnel auf dem Head-Knoten, die Ambari läuft über HTTP und die Kommunikation über SSH-Tunnel gesichert. Bei der Verbindung über das Internet ohne einen Tunnel ist Kommunikation über HTTPS gesichert. Verbindung über das Internet mit HTTPS verwenden Sie https://CLUSTERNAME.azurehdinsight.net, wobei __CLUSTERNAME__ den Namen des Clusters.

2. Ambari Web-Benutzeroberfläche wählen Sie bietet in der Liste auf der linken Seite.

    ![Bild mit bietet ausgewählt](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)

3. Wenn die Dienstinformationen bietet angezeigt wird, wählen __Quick Links__. Der Kopf Clusterknoten aufgeführt. Wählen Sie eine der Head-Knoten, und wählen Sie __NameNode-Benutzeroberfläche__.

    ![Bild mit erweiterten Menü QuickLinks](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)

    > [AZURE.NOTE] Wenn Sie eine langsame Verbindung verfügen oder der Head-Knoten ausgelastet ist, erhalten Sie einen Indikator warten statt eines Menüs Wenn Sie __Quick Links__auswählen. In diesem Fall warten ein oder zwei Minuten die Daten vom Server empfangen und die Liste wiederholen.
    >
    > Wenn Sie eine niedrigere Auflösung, oder Ihr Browser-Fenster nicht maximiert, können einige Einträge im Menü __Quick Links__ von der rechten Seite des Bildschirms abgeschnitten. Dann erweitern Sie das Menü mit der Maus, und verwenden Sie die Pfeiltaste um einen Bildlauf nach rechts, um den Rest des Menüs anzeigen.

4. Sollte eine Seite ähnlich der folgenden angezeigt werden:

    ![Bild der Benutzeroberfläche NameNode](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)

    > [AZURE.NOTE] Beachten Sie die URL für diese Seite. __Http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088-Cluster__ähnlich sollte. Diese internen vollqualifizierten Domänennamen (FQDN) des Knotens verwendet und kann nicht ohne einen SSH-Tunnel.

## <a name="next-steps"></a>Nächste Schritte

Damit haben Sie gelernt haben, erstellen und Verwenden eines SSH-Tunnels, finden Sie Informationen auf Überwachen und Verwalten des Clusters mit Ambari:

* [Verwalten Sie HDInsight-Cluster mithilfe von Ambari](hdinsight-hadoop-manage-ambari.md)

Weitere Informationen über SSH mit HDInsight finden Sie unter:

* [Verwenden Sie SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix und Mac OS](hdinsight-hadoop-linux-use-ssh-unix.md)

* [Verwenden Sie SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)
