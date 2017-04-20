<properties
 pageTitle="NAMD mit Microsoft HPC Pack auf Linux VMs | Microsoft Azure"
 description="Einen Cluster Microsoft HPC Pack Azure bereitstellen und Ausführen einer Simulations NAMD mit Charmrun mehrere Linux Compute-Knoten"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="10/13/2016"
 ms.author="danlep"/>

# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>NAMD Microsoft HPC Pack auf Linux Computeknoten in Azure ausgeführt

Dieser Artikel zeigt eine Möglichkeit, eine Linux High Performance computing (HPC)-Arbeitslast auf Azure virtuelle Computer ausgeführt werden. Hier einen Cluster [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) auf Azure mit Linux Compute-Knoten einrichten und Ausführen einer Simulation [NAMD](http://www.ks.uiuc.edu/Research/namd/) berechnen und die Struktur eines großen biomolekularen visualisieren.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (für molekulare Dynamics Nano-Programm) ist eine parallele molekulare Dynamics entwickelt für leistungsfähige Simulation von großen biomolekularen Systemen mit Millionen von Atome. Beispiele für diese Systeme sind Viren, Zelle und große Proteine. NAMD skaliert Hunderttausende Kerne typisch Simulationen und zu 500.000 Kerne größten Simulationen.

* **Microsoft HPC Pack** bietet umfangreiche HPC und parallele Programme in Clustern mit lokalen oder Azure virtuelle Computer ausführen. Ursprünglich als Lösung für Windows HPC-Arbeitslasten HPC Pack berechnen nun unter Linux HPC Applikationen Linux unterstützt virtuelle Computer Knoten in einem Cluster HPC Pack bereitgestellt. Eine Einführung finden Sie unter [Erste Schritte mit Linux Compute-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) .

Andere Optionen Linux HPC Arbeitslasten in Azure ausgeführt finden Sie [Technische Ressourcen für Stapel und computing](../batch/batch-hpc-solutions.md).




## <a name="prerequisites"></a>Erforderliche Komponenten

* **HPC Pack Cluster mit Linux compute-Knoten** - Bereitstellen eines Clusters HPC Pack mit Linux Compute-Knoten auf Azure mit einer [Azure-Ressourcen-Manager-Vorlage](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) oder eine [Azure PowerShell-Skript](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Weitere [Erste Schritte mit Linux Compute-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) Komponenten und Schritte für beide Optionen. Bei Auswahl die Bereitstellungsoption PowerShell-Skript finden Sie unter Beispiel-Konfigurationsdatei in den Beispieldateien am Ende dieses Artikels. Diese Datei wird einen HPC Pack Azure-basierten Cluster mit Windows Server 2012 R2 Headknoten und vier Größe großer CentOS 6.6 Compute-Knoten konfiguriert. Passen Sie diese Datei für Ihre Umgebung an.


* **NAMD Software und Tutorial Dateien** - NAMD Download-Software für Linux von der [NAMD](http://www.ks.uiuc.edu/Research/namd/) -Website (Registrierung erforderlich). Dieser Artikel basiert auf NAMD Version 2.10 und [64-Bit Intel/AMD (Ethernet) Linux-x86_64](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) -Archiv verwendet. Die [Dateien des Lernprogramms NAMD](http://www.ks.uiuc.edu/Training/Tutorials/#namd)auch herunterladen. Die Downloads sind TAR-Dateien benötigen Sie ein Windows-Tool zum Extrahieren der Dateien auf dem Head-Knoten des Clusters Um die Dateien zu extrahieren, führen Sie die Schritte in diesem Artikel. 

* **VMD** (optional) - die Ergebnisse Ihrer Arbeit NAMD downloaden und installieren das molekularen visualisierungsprogramm [VMD](http://www.ks.uiuc.edu/Research/vmd/) auf einem Computer Ihrer Wahl. Die aktuelle Version ist 1.9.2. Produktdokumentationen Einstieg VMD anzeigen  


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Einrichten von Vertrauen zwischen Compute-Knoten
Ausführen eines Auftrags knotenübergreifende auf mehrere Linux-Nodes erfordert die Knoten (nach **Rsh** oder **ssh**) vertrauen. Beim Erstellen des Clusters HPC Pack mit Microsoft HPC Pack IaaS Bereitstellungsskript legt das Skript automatisch einrichten dauerhafte vertrauen für das Administratorkonto, das Sie angeben. Für nicht-Administratoren, die in die Domäne des Clusters erstellen, müssen Sie temporäre vertrauen zwischen den Knoten einrichten, wenn ein Auftrag zugeordnet ist. Beziehung zerstört dann nach Abschluss des Auftrags. Geben Sie dazu für jeden Benutzer ein RSA-Schlüsselpaar für den Cluster die HPC Pack verwendet, um die Vertrauensstellung einrichten. Befolgen.

### <a name="generate-an-rsa-key-pair"></a>Ein RSA-Schlüsselpaar generieren
Es ist einfach, einen öffentlichen Schlüssel und einen privaten Schlüssel enthält, mit dem Befehl **ssh-Keygen** Linux ein RSA-Schlüsselpaar generieren.

1.  Melden Sie sich beim Linux-Computer.

2.  Führen Sie den folgenden Befehl ein:

    ```bash
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Drücken Sie die **EINGABETASTE** , die Standardeinstellungen zu verwenden, bis der Befehl ausgeführt wird. Geben Sie ein Kennwort nicht; Drücken Sie nach einem Kennwort gefragt die **EINGABETASTE**.

    ![Ein RSA-Schlüsselpaar generieren][keygen]

3.  Wechseln Sie in das Verzeichnis ~/.ssh. Der private Schlüssel wird in Id_rsa und der öffentliche Schlüssel in id_rsa.pub gespeichert.

    ![Private und öffentliche Schlüssel][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Das Schlüsselpaar mit dem HPC Pack Cluster hinzufügen
1.  [Über den Remotedesktop](virtual-machines-windows-connect-logon.md) auf dem Head-Knoten VM mit der Domäne Anmeldeinformationen bereitgestellt als Cluster (z. B. Hpc\clusteradmin) bereitgestellt. Sie verwalten den Cluster aus dem Head-Knoten.

2. Verwenden Sie Windows Server Standardverfahren zum Erstellen eines Domänenbenutzerkontos in Active Directory-Domäne des Clusters. Z. B. das Tool Active Directory-Benutzer und-Computer auf dem Head-Knoten. Die Beispiele in diesem Artikel angenommen, einen Domänenbenutzer mit dem Namen Hpcuser im Bereich Hpclab (Hpclab\hpcuser) erstellen.

3. Hinzufügen des Domänenbenutzers HPC Pack Cluster als clusterbenutzer. Informationen finden Sie unter [Hinzufügen oder entfernen, die cluster-Benutzer](https://technet.microsoft.com/library/ff919330.aspx).

2.  Erstellen Sie eine Datei namens C:\cred.xml und kopieren Sie RSA Key Daten hinein. Ein Beispiel finden Sie in den Beispieldateien am Ende dieses Artikels.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

3.  Öffnen Sie ein Eingabeaufforderungsfenster, und geben Sie den folgenden Befehl der Anmeldeinformationen für das Konto Hpclab\hpcuser festgelegt. Mithilfe den **Extendeddata** -Parameter übergeben Sie den Namen der C:\cred.xml-Datei erstellten für wichtigen Daten.

    ```command
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Dieser Befehl wird ohne Ausgabe erfolgreich abgeschlossen. Nach dem Festlegen der Anmeldeinformationen für die Benutzerkonten müssen Sie Aufträge ausgeführt werden, die cred.xml-Datei an einem sicheren Ort speichern oder löschen.

5.  Wird das RSA-Schlüsselpaar auf einem Linux-Knoten erstellt wird, sollten Sie Schlüssel zu löschen, klicken Sie nach der Verwendung. HPC Pack ist vertrauen nicht einrichten, wenn eine vorhandene Id_rsa oder id_rsa.pub ermittelt.

>[AZURE.IMPORTANT] Wir empfehlen nicht ausführen eines Auftrags Linux als Clusteradministrator auf einem freigegebenen Cluster da ein Auftrag übermittelt werden unter dem Root-Konto auf Linux-Knoten ausgeführt wird. Ein nicht-Administrator-Benutzer übermittelten Taskausführung unter einem lokalen Benutzerkonto von Linux mit demselben Namen wie der Auftrag Benutzer. In diesem Fall richtet HPC Pack vertrauen für den Linux-Benutzer auf allen Knoten, die dem Projekt zugeordnet. Linuxbenutzer manuell auf den Linux-Knoten eingerichtet, Preis oder HPC Pack erstellt den Benutzer automatisch, wenn der Auftrag gesendet wird. HPC Pack erstellt den Benutzer löscht HPC Pack nach Abschluss des Auftrags. Um das Sicherheitsrisiko zu verringern, werden die Schlüssel nach Auftragsabschluss auf den Knoten entfernt.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Richten Sie eine Dateifreigabe für Linux-Knoten

Jetzt eine SMB-Dateifreigabe einrichten und Bereitstellen des freigegebenen Ordners auf allen Linux-Knoten zu Linux-Knoten Zugriff auf Dateien mit einem gemeinsamen Pfad NAMD. Folgendes sind Schritte, einen freigegebenen Ordner auf dem Head-Knoten. Eine Freigabe für Distributionen wie CentOS 6.6 empfohlen, die derzeit nicht den Dienst Azure unterstützen. Die Linux-Knoten eine Dateifreigabe Azure unterstützt, finden Sie unter [Azure Dateispeicher Linux verwenden](../storage/storage-how-to-use-files-linux.md). Zusätzliche Dateifreigabeoptionen HPC Pack finden Sie unter [Erste Schritte mit Linux Compute-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Erstellen Sie einen Ordner auf dem Head-Knoten und mit Lese-und Schreibberechtigungen für alle Benutzer freigeben. In diesem Beispiel \\ \\CentOS66HN\Namd ist der Name des Ordners, in dem CentOS66HN der Hostname des Head-Knotens ist.

2. Erstellen Sie einen Unterordner namens namd2 im freigegebenen Ordner. Erstellen Sie in namd2 einen weiteren Unterordner mit dem Namen Namdsample.

3. Extrahieren Sie NAMD-Dateien im Ordner anhand einer Windows-Version **Tar** oder einer anderen Windows-Dienstprogramm arbeitet tar-Archiv. 
    * NAMD-Tar-Archiv zu extrahieren \\ \\CentOS66HN\Namd\namd2.
    
    * Extrahieren Sie die Dateien des Lernprogramms unter \\ \\CentOS66HN\Namd\namd2\namdsample.

4. Öffnen Sie ein Windows PowerShell-Fenster und führen Sie die folgenden Befehle, den freigegebenen Ordner auf Linux-Knoten.

    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2

    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Der erste Befehl erstellt einen Ordner namens /namd2 auf allen Knoten in der LinuxNodes-Gruppe. Der zweite Befehl stellt die freigegebenen Ordner //CentOS66HN/Namd/namd2 auf den Ordner mit Dir_mode und File_mode-Bits auf 777 gesetzt. *Benutzername* und *Kennwort* im Befehl sollte die Anmeldeinformationen eines Benutzers auf dem Head-Knoten.

>[AZURE.NOTE]Die "\`" Symbol im zweiten Befehl ist ein Escapezeichen für PowerShell. "\`," bedeutet "," (Komma) ist ein Teil des Befehls.


## <a name="create-a-bash-script-to-run-a-namd-job"></a>Erstellen Sie ein Bash-Skript zum Ausführen eines Auftrags NAMD

Ihre NAMD Arbeit benötigt eine *Nodelist* für **Charmrun** bestimmt die Anzahl der Knoten zu Beginn NAMD Prozesse. Sie verwenden ein Bash-Skript, das Nodelist-Datei generiert und **Charmrun** mit dieser Knotenlistendatei. Sie können einen Auftrag NAMD HPC Cluster Manager übermitteln, die das Skript aufruft.

Mit einem Text-Editor Ihrer Wahl, erstellen Sie Bash-Skript im Ordner "/namd2" mit den Programmdateien NAMD und benennen Sie sie hpccharmrun.sh. Für eine schnelle Machbarkeitsstudie Beispielskript für die hpccharmrun.sh am Ende dieses Artikels kopieren und Absenden [eines Auftrags NAMD](#submit-a-namd-job)gehen.

>[AZURE.TIP] Speichern Sie das Skript als Textdatei mit Linux Zeilenenden Sie (nur LF, nicht CR-LF). Dies gewährleistet, dass es korrekt auf der Linux-Knoten ausgeführt wird.

Details zur Funktionsweise dieses Bash folgen. 

1.  Definieren Sie einige Variablen.

    ```bash
    #!/bin/bash

    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    # Charmrun command
    CHARMRUN=${SCRIPT_PATH}/charmrun
    # Argument of ++nodelist
    NODELIST_OPT="++nodelist"
    # Argument of ++p
    NUMPROCESS="+p"
    ```

2.  Die Umgebungsvariablen erhalten Sie Informationen. $NODESCORES speichert eine Liste von Wörtern aufteilen von $CCP_NODES_CORES. $COUNT ist die Größe des $NODESCORES.
    ```
    # Get node information from the environment variables
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    ```    
    
    Das Format für die Variable $CCP_NODES_CORES lautet wie folgt:

    ```
    <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
    ```

    Diese Variable enthält die Gesamtanzahl der Knoten Knotennamen und Anzahl der Kerne auf jedem Knoten, die dem Projekt zugeordnet. Sollte der Auftrag 10 Kernen ausgeführt, ist der Wert $CCP_NODES_CORES ähnlich:

    ```
    3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
    ```
        
3.  Starten Sie $CCP_NODES_CORES-Variable nicht festgelegt ist, direkt **Charmrun** . (Dies sollte nur auftreten, wenn dieses Skript direkt auf Linux-Knoten ausgeführt.)

    ```
    if [ ${COUNT} -eq 0 ]
    then
        # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
        #echo ${CHARMRUN} $*
        ${CHARMRUN} $*
    ```

4.  Oder erstellen Sie eine Nodelist für **Charmrun**.

    ```
    else
        # Create the nodelist file
        NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

        # Write the head line
        echo "group main" > ${NODELIST_PATH}

        # Get every node name and number of cores and write into the nodelist file
        I=1
        while [ ${I} -lt ${COUNT} ]
        do
            echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
            let "I=${I}+2"
        done
```
5.  Führen Sie **Charmrun** mit der Knotenlistendatei und Nodelist Datei am Ende der Rückgabestatus erhalten.

    ${CCP_NUMCPUS} wird eine andere Umgebungsvariable HPC Pack Head-Knoten festlegen. Es speichert die gesamte Kerne, die diesem Auftrag zugewiesen. Wir können sie die Anzahl der Prozesse für Charmrun angeben.

    ```
    # Run charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
    fi

    ```
6.  Beenden Sie den Rückgabestatus **Charmrun** .

    ```
    exit ${RTNSTS}
    ```



Es folgt die Informationen in der Nodelist-Datei das Skript generiert:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Zum Beispiel:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>Senden eines Auftrags NAMD

Jetzt können Sie einen Auftrag NAMD HPC Cluster Manager senden.

1.  Verbindung mit der Cluster-Head-Knoten und HPC Cluster Manager starten.

2.  **Ressourcen-Management**sicherstellen Sie, dass Linux Compute-Knoten in den **Online** -Zustand. Nicht wählen sie und klicken Sie auf **Online schalten**.

2.  **Projektmanagement**klicken Sie auf **Neues Projekt**.

3.  Geben Sie z. B. *Hpccharmrun*.

    ![Neue HPC-Projekt][namd_job]

4.  Wählen Sie auf der Seite **Job-Details** unter **Projekt Ressourcen**die Ressource als **Knoten** und das **Minimum** auf 3 festgelegt. , wir die Stapelverarbeitung auf drei Linux-Knoten, und jeder Knoten verfügt über vier Kerne.

    ![Job-Ressourcen][job_resources]

5. Klicken Sie im linken Navigationsbereich auf **Aufgaben bearbeiten** , und klicken Sie dann auf **Hinzufügen** , um dem Projekt eine Aufgabe hinzuzufügen.    


6. Auf der Seite **e/a-Umleitung und Aufgabendetails** Optionswerte:

    * **Befehlszeile** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`

        >[AZURE.TIP] Die vorstehenden Befehlszeile ist ein Befehl ohne Zeilenumbrüche. Es schließt mehrere Zeilen unter **Befehlszeile**angezeigt.

    * **Working Directory** - /namd2

    * **Minimum** : 3

    ![Aufgabendetails][task_details]

    >[AZURE.NOTE] Sie legen das Arbeitsverzeichnis hier da **Charmrun** versucht, dasselbe Arbeitsverzeichnis auf jedem Knoten navigieren. Wenn das Arbeitsverzeichnis nicht festgelegt ist, startet HPC Pack Befehls in einem zufällig benannten Ordner auf einem Linux-Knoten erstellt. Dadurch wird den folgenden Fehler auf den anderen Knoten: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` um dieses Problem zu vermeiden, geben Sie einen Ordnerpfad alle Knoten als Arbeitsverzeichnis zugegriffen werden kann.

5.  Klicken Sie auf **OK** , und klicken Sie dann auf **Senden** , um diesen Auftrag ausführen.

    Standardmäßig sendet HPC Pack den Auftrag als das aktuelle angemeldete Benutzerkonto. Ein Dialogfeld möglicherweise aufgefordert, den Benutzernamen und das Kennwort einzugeben, nach **Absenden**klicken.

    ![Job-Anmeldeinformationen][creds]

    Unter gewissen Umständen speichert HPC Pack Benutzerinformationen vor Eingabe und dieses Dialogfeld nicht anzeigen. Um HPC Pack wieder anzuzeigen, geben Sie folgenden Befehl an der Befehlszeile und senden Sie den Auftrag.

    ```command
    hpccred delcreds
    ```

6.  Das Projekt nimmt einige Minuten in Anspruch.

7.  Finden Sie das Job-Protokoll am \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log und Ausgabedateien in \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.

8.  Starten Sie VMD zum Anzeigen der Auftragsergebnisse gegebenenfalls. Die Schritte zur Umsetzung der NAMD Ausgabe Dateien (in diesem Fall Ubiquitin proteinmolekül im Bereich Wasser) sind nicht Gegenstand dieses Artikels. Details finden Sie im [Lernprogramm NAMD](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) .

    ![Auftragsergebnisse][vmd_view]

## <a name="sample-files"></a>Beispieldateien

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>XML-Beispielkonfigurationsdatei für Bereitstellung von PowerShell-Skript

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>    
```

### <a name="sample-credxml-file"></a>Cred.xml-Beispieldatei

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```

### <a name="sample-hpccharmrunsh-script"></a>Beispielskript für hpccharmrun.sh

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keys.png
[namd_job]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/namd_job.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/job_resources.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/creds.png
[task_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/task_details.png
[vmd_view]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/vmd_view.png
