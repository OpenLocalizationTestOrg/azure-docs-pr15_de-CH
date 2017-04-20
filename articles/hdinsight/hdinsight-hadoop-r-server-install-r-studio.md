<properties
    pageTitle="Installieren RStudio mit R Server HDInsight (Vorschau) | Microsoft Azure"
    description="RStudio mit HDInsight (Vorschau) R Server installieren"
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/16/2016"
   ms.author="jeffstok"/>


# <a name="installing-rstudio-with-r-server-on-hdinsight-preview"></a>RStudio mit HDInsight (Vorschau) R Server installieren

Gibt es mehrere integrierte Umgebungen (IDE) für R heute, zuletzt von Microsoft kündigte [R-Tools für Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (FORMAL) eine Reihe von Desktop- und Server-Tools von [RStudio](https://www.rstudio.com/products/rstudio-server/)oder Malwares Eclipse-basierten [angegeben](http://www.walware.de/goto/statet). Zu den beliebtesten unter Linux ist die Verwendung von [RStudio Server](https://www.rstudio.com/products/rstudio-server/) , Browser-basierten IDE für remote Clients bereitstellt.  RStudio-Installation auf dem Rand Knoten eines Clusters HDInsight Premium bietet eine vollständige IDE für die Entwicklung und Ausführung von Skripten, R R Server im Cluster und sind wesentlich produktiver als standardmäßige Verwendung von R-Konsole.

In diesem Artikel erfahren Sie, wie der Gemeinschaft (kostenlos) RStudio Server auf dem edgeknoten eines Clusters installieren mithilfe eines benutzerdefinierten Skripts. Auf Wunsch kommerziell lizenzierte proversion von RStudio Server befolgen Sie die Installationshinweise [RStudio](https://www.rstudio.com/products/rstudio/download-server/)Server.

> [AZURE.NOTE] Die Schritte in diesem Dokument erfordern einen Server R HDInsight Cluster und nicht korrekt ausgeführt, wenn Sie einen HDInsight-Cluster verwenden, R mit [R Skriptaktion installieren](hdinsight-hadoop-r-scripts-linux.md)installiert wurde.

## <a name="prerequisites"></a>Erforderliche Komponenten

* Ein Azure HDInsight Cluster mit R-Server installiert. Anleitung finden Sie in [Erste Schritte mit R HDInsight Cluster Server](hdinsight-hadoop-r-server-get-started.md).
* SSH-Client. Verteilung von Linux und Unix oder Macintosh OS X die `ssh` Befehl wird mit dem Betriebssystem bereitgestellt. Unter Windows sollten [Cygwin](http://www.redhat.com/services/custom/cygwin/) [OpenSSH Option](https://www.youtube.com/watch?v=CwYSvvGaiWU)oder [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  


## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>Installieren von RStudio auf dem Cluster mit einem benutzerdefinierten Skript

1. Identifizieren des Edge-Knotens des Clusters. Für einen HDInsight-Cluster mit R Server ist folgende Namenskonvention für Kopf und edgeknoten.

    * Head-Knoten-`CLUSTERNAME-ssh.azurehdinsight.net`
    * Kantenknoten-`R-Server.CLUSTERNAME-ssh.azurehdinsight.net` 

2. SSH in den Rand Knoten des Clusters mit dem obigen Benennungsmuster. 
 
    * Wenn Sie einen Linux-Client herstellen, finden Sie unter [Verbinden HDInsight Linux-basierten Cluster](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).
    * Wenn Sie aus einem Windows-Client verbunden sind, finden Sie unter [mit einem HDInsight Linux-basierten Cluster mit kitten](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster).

3. Sobald Sie verbunden sind, werden Sie ein Root-Benutzer auf dem Cluster. Verwenden Sie den folgenden Befehl in der SSH-Sitzung.

        sudo su -

4. Downloaden Sie das benutzerdefinierte Skript RStudio installieren. Verwenden Sie den folgenden Befehl ein.

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Die Berechtigungen für die benutzerdefinierte Datei, und führen Sie das Skript. Verwenden Sie die folgenden Befehle.

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Wenn Sie beim Erstellen eines Clusters HDInsight R Server SSH-Kennwort verwendet, können Sie diesen Schritt überspringen und mit der nächsten fortfahren. Wenn Sie SSH-Schlüssel stattdessen verwendet Cluster erstellen, müssen Sie ein Kennwort für den Benutzer SSH festlegen. Sie benötigen dieses Kennwort beim Verbinden mit RStudio. Führen Sie die folgenden Befehle. Drücken Sie nach **aktuellen Kerberos-Kennwort**gefragt die **EINGABETASTE**.  Hinweis Sie ersetzen `USERNAME` mit einem SSH-Benutzer für den HDInsight-Cluster.

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:
        
    Wenn das Kennwort erfolgreich festgelegt wurde, sollte eine Meldung angezeigt werden.

        passwd: password updated successfully


    Beenden Sie die SSH-Sitzung.

7. Erstellen einen SSH-Tunnel zum Cluster zuordnen `localhost:8787` im HDInsight-Cluster auf den Clientcomputer. Sie müssen einen SSH-Tunnel erstellen, vor dem Öffnen eines neuen Browsersitzung.

    * Einen Linux-Client oder Windows-Client mit [Cygwin](http://www.redhat.com/services/custom/cygwin/) dann öffnen Sie eine terminal Sitzung und verwenden Sie den folgenden Befehl.

            ssh -L localhost:8787:localhost:8787 USERNAME@R-Server.CLUSTERNAME-ssh.azurehdinsight.net
            
        Ersetzen **Benutzernamen** durch einen Benutzer SSH für den HDInsight-Cluster, und **CLUSTERNAME** mit dem Namen des Clusters HDInsight können Sie auch einen SSH-Schlüssel als ein Kennwort hinzufügen`-i id_rsa_key`     

    * Wenn einen Windows-Client mit kitten verwenden

        1.  Öffnen Sie kitten, und geben Sie die Verbindungsinformationen. Wenn Sie nicht mit kitten vertraut sind, finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) Informationen mit HDInsight verwendet.
        2.  Im Abschnitt **Kategorien** links im Dialogfeld erweitern Sie **Verbindung** **SSH**, und wählen Sie dann **Tunnel**.
        3.  Angaben Sie folgende im Formular **Optionen zum Steuern der SSH Port weiterleiten** :

            * **Ursprungs-Port** - Port auf dem Client, den Sie weiterleiten möchten. Beispielsweise **8787**.
            * **Ziel** - das Ziel des Clientcomputers zugeordnet werden muss. Beispielsweise **Localhost:8787**.

            ![Erstellen eines Tunnels SSH] (./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Erstellen eines Tunnels SSH")

        4. Klicken Sie auf **Hinzufügen** , um die Einstellungen hinzufügen, und klicken Sie auf **Öffnen** , um eine SSH-Verbindung zu öffnen.
        5. Wenn Sie aufgefordert werden, melden Sie sich an den Server an. Eine SSH-Sitzung einrichten wird und aktivieren den Tunnel.

8. Öffnen Sie einen Webbrowser, und geben Sie den folgenden URL basierend auf der Port für den Tunnel eingegeben.

        http://localhost:8787/ 

9. Sie werden aufgefordert SSH Benutzername und Kennwort für die Verbindung mit dem Cluster. Wenn Sie SSH-Schlüssel beim Erstellen des Clusters verwendet, geben Sie das Kennwort, das Sie in Schritt 5 erstellt haben.

    ![R-Studio verbinden] (./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Erstellen eines Tunnels SSH")

10. Testen, ob die RStudio-Installation erfolgreich war, können Sie einen Test ausführen, der R führt skriptbasierten MapReduce und Spark Aufträge im Cluster. SSH-Konsole zurück, und geben Sie die folgenden Befehle das Testskript ausführen in RStudio herunterladen.

    * Wenn Sie R Hadoop Cluster erstellt, verwenden Sie diesen Befehl.
        
            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r

    * Wenn Sie R Spark-Cluster erstellt, verwenden Sie diesen Befehl.

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

11. In RStudio sehen Sie das Testskript, das Sie heruntergeladen haben. Doppelklicken Sie die Datei öffnen, markieren Sie den Inhalt der Datei und klicken Sie auf **Ausführen**. Die Ausgabe im **Konsolenfenster** sollte angezeigt werden.
 
    ![Testen der installation] (./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Testen der installation")

Eine andere Möglichkeit wäre, geben `source(testhdi.r)` oder `source(testhdi_spark.r)` ausführen.

## <a name="see-also"></a>Siehe auch

- [Berechnen Sie Kontextoptionen R Server HDInsight-Cluster](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure Speicheroptionen für R Server HDInsight Premium](hdinsight-hadoop-r-server-storage.md)


 
