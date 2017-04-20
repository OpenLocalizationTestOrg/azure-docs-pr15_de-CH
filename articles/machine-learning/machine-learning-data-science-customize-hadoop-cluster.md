<properties 
    pageTitle="Hadoop Cluster für Team wissenschaftliche Daten anpassen | Microsoft Azure" 
    description="Beliebte Python-Module in benutzerdefinierten Azure HDInsight Hadoop-Cluster zur Verfügung gestellt."
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="hangzh;bradsev" />

# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a>Anpassen von Azure HDInsight Hadoop-Cluster für Team wissenschaftliche Daten 

Dieser Artikel beschreibt einen HDInsight Hadoop-Cluster mit 64-Bit-Anaconda (Python 2.7) anpassen auf jedem Knoten als Cluster als HDInsight-Dienst bereitgestellt wird. Es veranschaulicht auch auf den Hauptknoten um benutzerdefinierte Aufträge zum Cluster senden. Diese Anpassung kann viele beliebte Python-Module, die anaconda verfügbar zur Verwendung in benutzerdefinierten Funktionen (UDFs) enthalten sind, die Struktur Datensätze im Cluster verarbeitet werden soll. Informationen zu den Verfahren in diesem Szenario verwendeten finden Sie unter [Struktur Abfragen absenden](machine-learning-data-science-move-hive-tables.md#submit).

Klicken Sie im Menü unten Links zu Themen zum Einrichten der verschiedenen Science-Umgebung von [Team Daten Wissenschaft Prozess (TDSP)](data-science-process-overview.md)verwendet werden.

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]


## <a name="customize"></a>Anpassen von Azure HDInsight Hadoop-Cluster

Zum Erstellen eines benutzerdefinierten HDInsight Hadoop-Clusters müssen Benutzer [**Klassischen Azure-Portal**](https://manage.windowsazure.com/)anmelden, klicken Sie in der linken unteren Ecke auf **neu** , und dann wählen Sie DATENDIENSTE -> HDINSIGHT -> **Benutzerdefinierte** **Cluster** Detailfenster bringen. 

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Geben Sie den Namen des Clusters auf Seite 1 der Konfiguration erstellt werden, und übernehmen Sie die Standardwerte für die Felder. Klicken Sie auf den Pfeil auf die folgende Konfiguration zu gelangen. 

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Auf Seite 2 der Anzahl der **DATENKNOTEN**, wählen Sie das **REGION-VIRTUAL NETWORK**und die Größen der **HEAD-Knoten** und den **Knoten**. Klicken Sie auf den Pfeil auf die folgende Konfiguration zu gelangen.

>[AZURE.NOTE] **REGION-virtuellen Netzwerk** hat als Bereich für das Speicherkonto übereinstimmen, die für den HDInsight Hadoop-Cluster verwendet werden. Andernfalls wird im vierten Seite Speicherkonto, das Benutzer verwenden möchten nicht in der Dropdown-Liste der **Kontoname**angezeigt.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

Geben Sie auf Seite 3 einen Benutzernamen und ein Kennwort für HDInsight Hadoop-Cluster. **Nicht** die _EINGABETASTE Hive-Oozie-Metastore_auswählen Klicken Sie den Pfeil auf die folgende Konfiguration zu gelangen. 

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

Geben Sie auf Seite 4 Speicher-Kontonamen Standardcontainer HDInsight Hadoop-Cluster. Wenn Benutzer _Erstellen Standardcontainer_ **STANDARDCONTAINER** Dropdown-Liste auswählen, wird ein Container mit demselben Namen wie der Cluster erstellt. Klicken Sie auf den Pfeil, um zur letzten Konfigurationsseite wechseln.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

Endgültige **Skriptaktionen** -Konfigurationsseite auf **Skriptaktion** hinzufügen und füllen Sie die Textfelder mit den folgenden Werten.
 
* **NAME** – eine beliebige Zeichenfolge als Name für diese Skriptaktion. 
* **Knotentyp** auswählen **alle Knoten**. 
* **Skript-URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
    * *Publicscripts* ist eine öffentliche Container im Speicherkonto 
    * *Getgoing* PowerShell-Skript Dateien für Benutzer Arbeit in Azure verwenden. 
* **Parameter** - (leer lassen)

Klicken Sie auf das Häkchen die Erstellung von benutzerdefinierten HDInsight Hadoop Cluster gestartet. 

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Zugriff auf Head-Knoten des Clusters Hadoop

Benutzer müssen remote Zugriff auf den Cluster Hadoop in Azure aktivieren, bevor sie den Head-Knoten des Clusters Hadoop über RDP zugreifen können. 

1. [**Klassische Azure-Portal**](https://manage.windowsazure.com/)melden Sie an, wählen Sie **HDInsight** links wählen Sie Hadoop Cluster Cluster aus, klicken Sie auf die Registerkarte **Konfiguration** und **Ermöglichen REMOTE** -Symbol am unteren Rand der Seite klicken.
    
    ![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)

2. Geben Sie im Fenster **Remotedesktop konfigurieren** die Felder Benutzername und Kennwort ein, und wählen Sie das Ablaufdatum für den Remotezugriff. Klicken Sie auf das Häkchen, um den Remotezugriff auf dem Head-Knoten des Clusters Hadoop.

    ![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)
    
>[AZURE.NOTE] Benutzername und Kennwort für den Remotezugriff sind nicht den Benutzernamen und das Kennwort für die Erstellung des Clusters Hadoop. Einen separaten Satz von Anmeldeinformationen sind. Außerdem muss das Ablaufdatum der remote-Zugriff innerhalb von 7 Tagen ab dem aktuellen Datum.

Remote-Zugriff aktiviert ist, klicken Sie auf **CONNECT** am unteren Rand der Seite remote in den Head-Knoten. Anmeldung auf dem Head-Knoten des Clusters Hadoop durch Eingabe der Anmeldeinformationen für den RAS-Benutzer, den Sie zuvor angegeben.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

Die nächsten Schritte im Prozess erweiterte Analyse im [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) zugeordnet und Verschieben von Daten in HDInsight, verarbeiten und es zur Vorbereitung von Daten mit Azure Computer lernen probieren Schritte einschließen.

Finden Sie unter [Struktur Abfragen absenden](machine-learning-data-science-move-hive-tables.md#submit) Hinweise auf anaconda aus dem Head-Knoten des Clusters in benutzerdefinierte Funktionen (UDFs) enthalten sind, mit der Struktur Datensätze im Cluster verarbeitet werden, Python-Module.

 
