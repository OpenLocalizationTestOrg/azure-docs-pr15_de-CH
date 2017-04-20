<properties
pageTitle="HDInsight Zugriff auf Daten mit Shared Access Signatures"
description="Erfahren Sie mit Shared Access Signatures HDInsight Zugriff auf Daten in Azure-Speicher-Blobs."
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
ms.date="10/11/2016"
ms.author="larryfr"/>

#<a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-with-hdinsight"></a>Mithilfe von Azure Storage Shared Access Signaturen Zugriff auf Daten mit HDInsight

HDInsight verwendet Blobs Azure-Speicher zum Speichern von Daten. HDInsight muss Vollzugriff auf das Blob als Standardspeicher für Cluster verwendet haben, können Sie Berechtigungen auf Daten in anderen Blobs vom Cluster verwendet einschränken. Sie möchten z. B. Daten schreibgeschützt. Sie können dazu Shared Access Signatures.

Freigegebene-Signaturen (SAS) sind ein Feature von Azure-Speicherkonten, Zugriff auf Daten einschränken können. Zum Beispiel bietet schreibgeschützten Zugriff auf Daten. In diesem Dokument erfahren Sie, wie mithilfe SAS lesen und Liste nur einem BLOB-Container von HDInsight aktiviert.

##<a name="requirements"></a>Vorschriften

* Ein Azure-Abonnement

* C# oder Python. C#-Beispielcode dient als Visual Studio-Lösung.

    * Visual Studio muss Version 2013 2015
    
    * Python muss Version 2.7 oder höher sein.

* Linux-basierte HDInsight cluster oder [Azure PowerShell] [ powershell] -Wenn Sie einen vorhandenen Linux-basierten Cluster verfügen, können Sie Ambari einen SAS zum Cluster hinzufügen. Wenn dies nicht der Fall ist, können Sie einen neuen Cluster erstellen und Hinzufügen einer SAS bei der Clustererstellung Azure PowerShell.

* Die Beispieldateien aus [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Dieses Repository enthält Folgendes:

    * Ein Visual Studio-Projekt, das ein Behälter, gespeicherte Richtlinie und SAS mit HDInsight erstellen
    
    * Ein Python-Skript, die einen Behälter, gespeicherte Richtlinie und SAS mit HDInsight erstellen
    
    * Ein Powershellskript, die einen neuen HDInsight-Cluster erstellen und so konfigurieren, dass die SAS verwenden kann.

##<a name="shared-access-signatures"></a>SAS

Es gibt zwei Formen von Shared Access Signaturen:

* Ad-hoc: Startzeit, Ablaufzeit und Berechtigungen für die SAS werden alle angegebenen URI SAS (ausdrücklich oder konkludent, Startzeit fehlt, bei).

* Gespeicherte Zugriffsrichtlinie: gespeicherte Zugriffsrichtlinie auf - BLOB-Container, Tabelle, Warteschlange oder einer Dateifreigabe - Ressourcencontainer definiert und können so verwalten Sie Integritätsregeln für eine oder mehrere SAS verwendet werden. Wenn Sie eine gespeicherte Zugriffsrichtlinie SAS zuordnen, erbt die SAS Constraints - Startzeit, Ablaufzeit und -für die gespeicherte Richtlinie definierten Berechtigungen.

Der Unterschied zwischen den beiden Formen ist wichtig für ein Hauptszenario: Sperrung. SAS ist ein URL, wer die SAS erhält, unabhängig davon, wer zu angefordert verwenden kann. Wenn eine SAS veröffentlicht wird, kann von jedem in der ganzen Welt verwendet werden. SAS verteilt ist gültig, bis eine der folgenden vier Dinge:

1. Die Ablaufzeit für die SAS angegeben erreicht ist.

2. Die Ablaufzeit auf die gespeicherte Zugriffsrichtlinie auf die SAS angegeben erreicht ist (Wenn eine gespeicherte Zugriffsrichtlinie verwiesen wird und eine Ablaufzeit angegeben). Dies kann entweder auftreten, da das Intervall abgelaufen ist oder Sie die gespeicherte Zugriffsrichtlinie zu einem Ablaufdatum in der Vergangenheit ist eine Möglichkeit, die SAS widerrufen geändert haben.

3. Die gespeicherte Richtlinie verweist die SAS wird gelöscht, können die SAS widerrufen ist. Beachten Sie, dass wenn Sie die gespeicherte Zugriffsrichtlinie mit demselben Namen neu erstellen, vorhandene SAS-Token erneut entsprechend den zugeordneten, gespeicherten Richtlinien (sofern nicht die Ablaufzeit der SAS vergangen ist) gültig. Wenn Sie die SAS sperren, müssen Sie einen anderen Namen verwenden, wenn Sie die Richtlinie mit einem Ablaufdatum in der Zukunft neu erstellen.

4. Kontoschlüssel, die mit dem SAS erstellt wurde neu erstellt. Beachten Sie, dass dadurch werden alle Komponenten mit diesem Konto Key nicht authentifizieren, bis sie aktualisiert werden, um die anderen gültigen kontoschlüssel oder neu generierten kontoschlüssel verwenden.

> [AZURE.IMPORTANT] SAS URI ist mit dem Erstellen die Signatur verwendeten kontoschlüssel verknüpft und zugeordneten gespeicherten eine Zugriffsrichtlinie, (gegebenenfalls). Keine gespeicherten Richtlinien angegeben ist, ist die einzige Möglichkeit, eine SAS widerrufen Key Account ändern. 

Es wird empfohlen, stets gespeicherte Richtlinien, sodass Signaturen sperren oder das Ablaufdatum nach Bedarf zu verlängern. Die Schritte in diesem Dokument gespeicherte Richtlinien zum Generieren der SAS.

Weitere Informationen zu freigegebenen Zugriff Signaturen finden Sie unter [Understanding SAS-Modell](../storage/storage-dotnet-shared-access-signature-part-1.md).

##<a name="create-a-stored-policy-and-generate-a-sas"></a>Erstellen Sie eine gespeicherte Richtlinie und generieren SAS

Derzeit müssen Sie gespeicherte Richtlinie programmgesteuert erstellen. Sowohl C# als auch Python Beispiel erstellen eine gespeicherte und SAS zur [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature)finden.

###<a name="create-a-stored-policy-and-sas-using-c"></a>Erstellen Sie eine gespeicherte Richtlinie und SAS mit C\#

1. Öffnen Sie die Projektmappe in Visual Studio.

2. Im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt __SASToken__ , und wählen Sie __Eigenschaften__.

3. __Bearbeiten__ und Hinzufügen von Werten für die folgenden Einträge:

    * StorageConnectionString: Die Verbindungszeichenfolge für das Speicherkonto eine gespeicherte und SAS für erstellen möchten. Sollte `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` , `myaccount` ist der Name Ihres Speicherkontos und `mykey` der Schlüssel für das Speicherkonto.
    
    * ContainerName: Der Container im Speicherkonto Zugriff beschränken möchten.
    
    * SASPolicyName: Der Name für gespeicherte Richtlinie verwenden, die erstellt wird.
    
    * FileToUpload: Der Pfad zu einer Datei, die dem Container hochgeladen werden.
    
4. Führen Sie das Projekt. Ein Konsolenfenster angezeigt und nach dem Generieren der SAS Informationen ähnlich der folgenden angezeigt:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
        
    Speichern Sie diese benötigen das Speicherkonto HDInsight Cluster zuordnen SAS-Richtlinie Token. Sie benötigen auch den speicherkontonamen und den Containernamen.
    
###<a name="create-a-stored-policy-and-sas-using-python"></a>Erstellen Sie eine gespeicherte Richtlinie und Python mithilfe SAS

1. Öffnen Sie die Datei SASToken.py, und ändern Sie die folgenden Werte:

    * Richtlinie\_Name: der Name für gespeicherte Richtlinie, die erstellt wird.
    
    * Storage\_Konto\_Name: den Namen Ihres Speicherkontos.
    
    * Storage\_Konto\_Schlüssel: der Schlüssel für das Speicherkonto.
    
    * Storage\_Container\_Name: Container im Speicherkonto Zugriff beschränken möchten.
    
    * Beispiel\_Datei\_Pfad: der Pfad zu einer Datei, die dem Container hochgeladen werden

2. Führen Sie das Skript. Das SAS-Token ähnlich der folgenden wird nach Abschluss des Skripts angezeigt:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
    
    Speichern Sie diese benötigen das Speicherkonto HDInsight Cluster zuordnen SAS-Richtlinie Token. Sie benötigen auch den speicherkontonamen und den Containernamen.
    
##<a name="use-the-sas-with-hdinsight"></a>Verwenden Sie die SAS mit HDInsight

Erstellung ein Clusters HDInsight Geben Sie ein Konto Primärspeicher, und Sie können optional weitere Speicherkonten. Beide Methoden zum Hinzufügen von Speicher benötigen Vollzugriff auf Speicherkonten und Container verwendet werden.

Um einen SAS Zugriff auf Container einzuschränken, müssen Sie die __Core -__ Standortkonfiguration für den Cluster einen benutzerdefinierten Eintrag hinzufügen.

* Für __Windows-__ oder __Linux-basierte__ HDInsight-Cluster können Sie während der Erstellung des Clusters mit PowerShell dazu.

* Für __Linux-basierte__ HDInsight-Cluster ändern Sie nach Erstellung des Clusters mit Ambari.

###<a name="create-a-new-cluster-that-uses-the-sas"></a>Erstellen Sie einen neuen Cluster mit SAS

Erstellen Sie einen HDInsight Cluster mit SAS befindet sich auf der `CreateCluster` Verzeichnis des Repository. Gehen Sie folgendermaßen vor, um es zu verwenden:

1. Öffnen der `CreateCluster\HDInsightSAS.ps1` -Datei in einem Texteditor und ändern Sie die folgenden Werte am Anfang des Dokuments.

        # Replace 'mycluster' with the name of the cluster to be created
        $clusterName = 'mycluster'
        # Valid values are 'Linux' and 'Windows'
        $osType = 'Linux'
        # Replace 'myresourcegroup' with the name of the group to be created
        $resourceGroupName = 'myresourcegroup'
        # Replace with the Azure data center you want to the cluster to live in
        $location = 'North Europe'
        # Replace with the name of the default storage account to be created
        $defaultStorageAccountName = 'mystorageaccount'
        # Replace with the name of the SAS container created earlier
        $SASContainerName = 'sascontainer'
        # Replace with the name of the SAS storage account created earlier
        $SASStorageAccountName = 'sasaccount'
        # Replace with the SAS token generated earlier
        $SASToken = 'sastoken'
        # Set the number of worker nodes in the cluster
        $clusterSizeInNodes = 2
        
    Ändern Sie z. B. `'mycluster'` auf den Namen des Clusters, die Sie erstellen möchten. SAS-Werte übereinstimmen die Werte aus den vorherigen Schritten beim Erstellen ein Speicherkonto und SAS-Token.
    
    Nachdem Sie die Werte geändert haben, speichern Sie die Datei.

1. Öffnen Sie eine neue Azure PowerShell Prompt. Wenn Sie mit Azure PowerShell vertraut sind, oder nicht installiert haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][powershell].

2. Aus dem Azure-Abonnement authentifizieren anhand der folgenden:

        Login-AzureRmAccount
    
    Wenn aufgefordert werden, melden Sie sich mit dem Konto für Ihre Azure-Abonnement.
    
    Wenn die Anmeldung mehrere Azure-Abonnements zugeordnet ist, müssen mit `Select-AzureRmSubscription` Abonnement auswählen möchten.

2. Wechseln Sie aus den in der `CreateCluster` Verzeichnis mit der Datei HDInsightSAS.ps1. Verwenden Sie die folgenden Skripts ausführen
        
        .\HDInsightSAS.ps1
    
    Wie das Skript ausgeführt wird, wird diese Ausgabe PowerShell Prompt Anmelden als Ressource Gruppe und Speicherkonten erstellt. Er fordert dann Eingabe HTTP-Benutzer für den HDInsight-Cluster. Dies ist das HTTP/s Zugang zu dem Cluster verwendete Benutzerkonto.
    
    Wenn Sie einen Linux-basierten Cluster erstellen, werden Sie auch ein SSH Benutzernamen und Kennwort aufgefordert. Dies wird sich Remote für den Cluster verwendet.
    
    > [AZURE.IMPORTANT] Für den HTTP-s oder SSH-Benutzernamen und das Kennwort erforderlich ein Kennwort, die folgenden Kriterien erfüllen:
    >
    > - Mindestens 10 Zeichen lang muss sein
    > - Muss mindestens eine Ziffer enthalten
    > - Muss mindestens ein nicht-alphanumerischen Zeichen enthalten
    > - Muss mindestens einen oberen oder Kleinbuchstaben


Es dauert eine Weile dieses normalerweise ca. 15 Minuten abgeschlossen. Wenn das Skript ohne Fehler abgeschlossen ist, wurde der Cluster erstellt.

###<a name="update-an-existing-cluster-to-use-the-sas"></a>Aktualisieren einer vorhandenen Cluster mithilfe SAS

Wenn Sie einen vorhandenen Linux-basierten Cluster verfügen, können Sie __Core -__ Standortkonfiguration SAS hinzufügen, mithilfe der folgenden Schritte:

1. Öffnen Sie Ambari Webbenutzeroberfläche für den Cluster. Die Adresse für diese Seite ist https://YOURCLUSTERNAME.azurehdinsight.net. Wenn Sie aufgefordert werden, mit der Admin-Name (Admin) und das Kennwort beim Erstellen des Clusters verwendeten authentifizieren.

2. Links der Ambari Web-Benutzeroberfläche wählen Sie __bietet aus__ und wählen Sie dann die Registerkarte __Konfigurationen__ in der Mitte der Seite.

3. Wählen Sie die Registerkarte __Erweitert__ und dann suchen Sie im Abschnitt __benutzerdefinierte Core-Website__ .

4. Erweitern Sie den Abschnitt __benutzerdefinierte Core-Site__ am Ende, und klicken Sie auf __Eigenschaft hinzufügen...__ . Verwenden Sie die folgenden Werte für die __Schlüssel__ und __Wert__ :

    * __Schlüssel__: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
    
    * __Wert__: die SAS zurückgegebene zuvor ausgeführte C# oder Python-Anwendung
    
    Der Containername mit C# oder SAS-Anwendung verwendeten ersetzen Sie __CONTAINERNAME__ . Ersetzen Sie __STORAGEACCOUNTNAME__ mit dem verwendeten Speicher-Kontonamen.

5. Klicken Sie auf __Hinzufügen__ , um diese Schlüssel und Wert zu speichern, und klicken Sie auf __Speichern__ , um die Konfiguration zu speichern. Wenn Sie aufgefordert werden, Beschreibung der Änderung ("SAS-Speicherzugriff hinzufügen" beispielsweise) und dann auf __Speichern__.

    Klicken Sie auf __OK__ , wenn die Änderungen abgeschlossen wurden.

    > [AZURE.IMPORTANT] Dies speichert die Konfiguration, aber mehrere Dienste Neustart bevor die Änderung wirksam wird.

6. Ambari Web-Benutzeroberfläche wählen Sie __bietet__ links aus und wählen Sie dann aus der __Serviceaktionen__ Dropdown-Liste auf der rechten Seite __Starten__ . Bei Aufforderung wählen __Wartungsmodus schalten__ und dann wählen Sie Neustart alle __Conform".

    Wiederholen Sie diesen Vorgang für die MapReduce2 und GARN aus der Liste auf der linken Seite.

7. Diese neu gestartet haben, markieren und deaktivieren Wartungsmodus __Serviceaktionen__ fallen.

##<a name="test-restricted-access"></a>Eingeschränkten Zugriff testen

Überprüfen Sie über eingeschränkten Zugriff verfügen, verwenden Sie die folgenden Methoden:

* Verwenden Sie für __Windows-basierte__ HDInsight Cluster Remotedesktop Verbindung zum Cluster. Weitere Informationen finden Sie unter [Verbindungsprogramm zu HDInsight über RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp) .

    Sobald verbunden, verwenden Sie __Hadoop Befehlszeile__ -Symbol auf dem Desktop eine Befehlszeile öffnen.

* Für __Linux-basierte__ HDInsight Cluster SSH Verbindung zum Cluster verwenden. Finden Sie Informationen auf Linux-basierten Clustern SSH mit:

    * [SSH mit Linux-basierten Hadoop auf HDInsight von Linux, OS X und Unix verwenden](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Verwenden Sie SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md)
    
Sobald dem Cluster verbunden, gehen Sie folgendermaßen vor um sicherzustellen, dass nur lesen und Liste Elemente auf das Speicherkonto SAS können:

1. Geben Sie von dazu aufgefordert werden Folgendes ein: Ersetzen Sie __SASCONTAINER__ durch den Namen des Containers für das Speicherkonto SAS erstellt. Ersetzen Sie __SASACCOUNTNAME__ durch den Namen der das Speicherkonto für die SAS verwendet:

        hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    
    Diese Liste wird der Inhalt des Containers, der die Datei enthalten sollte, die hochgeladen wurden als Container und SAS erstellt wurde.
    
2. Verwenden Sie die folgenden überprüfen, ob der Inhalt der Datei zu lesen. Ersetzen Sie __SASCONTAINER__ und __SASACCOUNTNAME__ wie im vorherigen Schritt. Ersetzen Sie __DATEINAMEN__ mit dem Namen der Datei des vorherigen Befehls angezeigt:

        hdfs dfs -text wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
        
    Der Inhalt der Datei werden aufgelistet.
    
3. Verwenden Sie folgende Datei im lokalen Dateisystem:

        hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    
    Dadurch wird die Datei in eine lokale Datei __TestFile.txt__herunterladen

4. Verwenden Sie die folgenden lokale Datei in eine neue Datei mit dem Namen __testupload.txt__ im SAS-Speicher hochladen:

        hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    
    Sie erhalten eine Meldung ähnlich der folgenden:
    
        put: java.io.IOException
        
    Dieser Fehler tritt auf, weil der Speicherort lesen +-Liste ist. Verwenden Sie folgende Daten in der Standardspeicher für den Cluster verschoben wird:
    
        hdfs dfs -put testfile.txt wasbs:///testupload.txt
        
    Dieses Mal sollte der Vorgang erfolgreich abgeschlossen.
    
##<a name="troubleshooting"></a>Problembehandlung

###<a name="a-task-was-canceled"></a>Ein Vorgang wurde abgebrochen.

__Symptome__: beim Erstellen eines Clusters mithilfe der PowerShell-Skript möglicherweise die folgende Fehlermeldung angezeigt:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

__Ursache__: Dieser Fehler kann auftreten, wenn Sie ein Kennwort für den Benutzer Admin-HTTP für den Cluster oder (für Linux-basierten Clustern) verwenden die SSH-Benutzer.

__Auflösung__: Verwenden Sie ein Kennwort, die folgenden Kriterien erfüllen:

- Mindestens 10 Zeichen lang muss sein
- Muss mindestens eine Ziffer enthalten
- Muss mindestens ein nicht-alphanumerischen Zeichen enthalten
- Muss mindestens einen oberen oder Kleinbuchstaben

##<a name="next-steps"></a>Nächste Schritte

Nachdem erfahren Sie HDInsight Cluster beschränkt Zugriff hinzufügen gelernt haben, andere Methoden zum Arbeiten mit Daten in einem Cluster:

* [Struktur mit HDInsight verwenden](hdinsight-use-hive.md)

* [Verwenden Sie Schwein mit HDInsight](hdinsight-use-pig.md)

* [Verwenden Sie MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

[powershell]: ../powershell-install-configure.md
