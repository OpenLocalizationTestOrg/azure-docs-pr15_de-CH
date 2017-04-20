<properties
    pageTitle="Verwendung die Azure Slave-Plug-in mit kontinuierlicher Integration Jenkins | Microsoft Azure"
    description="Beschreibt das plug-in Azure Slave Jenkins kontinuierliche Integration."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="how-to-use-the-azure-slave-plug-in-with-jenkins-continuous-integration"></a>Verwendung von Azure Slave-Plug-in mit Jenkins fortlaufende Integration

Sie können Azure Slave-Plug-in Jenkins Bereitstellung Slave-Knoten auf Azure beim Ausführen von verteilten erstellt.

## <a name="install-the-azure-slave-plug-in"></a>Azure Slave-Plug-in installieren

1. Klicken Sie im Schaltpult Jenkins **Jenkins verwalten**.

1. Klicken Sie auf der Seite **Verwalten Jenkins** **Plug-Ins verwalten**.

1. Klicken Sie auf die Registerkarte **verfügbar** .

1. Geben Sie im Filterfeld die Liste der verfügbaren Plug-Ins **Azure** Liste mit den entsprechenden Plug-Ins beschränkt.

    Wenn Sie einen Bildlauf durch die Liste der verfügbaren Plug-Ins entscheiden, finden Azure Slave-Plug-in Sie unter Abschnitt **Clusterverwaltung und verteilt erstellen** .

1. Aktivieren Sie das Kontrollkästchen **Azure Slave-Plugin** .

1. Klicken Sie auf **ohne Neustart installieren** oder **herunterladen und installieren Sie nach dem Neustart**.

Nun, dass das plug-in installiert ist, sind die nächsten Schritte mit der Azure-Abonnement plug-in konfigurieren und erstellen eine Vorlage, die verwendet werden, erstellen Sie den virtuellen Computer für untergeordnete Knoten.


## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>Konfigurieren von Azure Slave-Plug-in mit Ihrem Abonnement

Ein Abonnement auch Einstellungen zu veröffentlichen ist eine XML-Datei mit sicheren Anmeldeinformationen und Weitere Informationen zu Azure in Umgebung arbeiten müssen. Konfigurieren Sie die Azure Slave-Plug-in benötigen Sie:

* Die Abonnement-id
* Zeugnisse für Ihr Abonnement

Diese können in Ihrem [Abonnementprofil]gefunden. Im folgenden ist ein Beispiel eines Abonnements.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID value>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Nachdem Sie Ihr Abonnementprofil haben, folgendermaßen Sie konfigurieren Azure Slave-Plug-in

1. Klicken Sie im Schaltpult Jenkins **Jenkins verwalten**.

1. Klicken Sie auf **System konfigurieren**.

1. Unten Sie auf der Seite im Abschnitt **Cloud** .

1. Klicken Sie auf **Hinzufügen neuen Cloud > Microsoft Azure**.

    ![Cloud-Abschnitt][cloud section]

    Diese zeigen die Felder sollten Ihre Abonnementdetails eingeben.

    ![Abonnement-Konfiguration][subscription configuration]

1. Die Abonnement-Id und Management Zertifikatwerte aus Ihrem Abonnementprofil kopieren und in die entsprechenden Felder einfügen.

    Beim Kopieren des Abonnement-Id und Management Zertifikats enthalten Sie nicht die Angebote, die die Werte einschließen.

1. Klicken Sie auf **Konfiguration überprüfen**.

1. Wenn die Konfiguration korrekt überprüft wird, klicken Sie auf **Speichern**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Einrichten einer Vorlage für virtuelle Computer Azure Slave-Plug-in

Eine Vorlage für virtuelle Computer definiert die Parameter, die das plug-in mit der Slave-Knoten auf Azure erstellen. In den folgenden Schritten erstellen wir eine Vorlage für einen virtuellen Computer Ubuntu.

1. Klicken Sie im Schaltpult Jenkins **Jenkins verwalten**.

1. Klicken Sie auf **System konfigurieren**.

1. Unten Sie auf der Seite im Abschnitt **Cloud** .

1. Suchen Sie im Abschnitt **Cloud** **Azure Virtual Machine-Vorlage hinzufügen**, und klicken Sie auf **Hinzufügen**.

    ![Vm-Vorlage hinzufügen][add vm template]

    Dies zeigt die Felder, Details zur Vorlage eingeben, die Sie erstellen.

    ![leere allgemeine Konfiguration][blank general configuration]

1. Geben Sie im Feld **Name** einen Azure-Cloud-Dienstnamen. Bezieht sich der eingegebene Name auf einen vorhandenen Cloud-Dienst, wird der virtuelle Computer in diesem Dienst bereitgestellt werden. Azure erstellen, eine neue.

1. Geben Sie im Feld **Beschreibung** ein, die die Vorlage beschreibt, die Sie erstellen. Dies ist nur für Ihre Unterlagen und bei der Bereitstellung eines virtuellen Computers nicht verwendet.

1. Das **Etiketten** wird verwendet, um die Vorlage zu identifizieren, die Sie erstellen und anschließend auf die Vorlage beim Erstellen eines Auftrags Jenkins verwendet. Geben Sie zu diesem Zweck in das **Linux** .

1. Klicken Sie in **der Liste** auf den Bereich, den virtuellen Computer erstellt werden.

1. Klicken Sie auf die entsprechende Größe in der Liste **Größe des virtuellen Computers** .

1. Geben Sie im Feld **Kontoname Speicher** ein Speicherkonto, den virtuellen Computer erstellt werden. Stellen Sie sicher, dass im Bereich Cloud-Dienst ist, den Sie verwenden. Wenn Sie neuen Speicher erstellt werden soll, lassen Sie dieses Feld leer.

1. Aufbewahrungszeit gibt die Anzahl der Minuten Jenkins Leerlauf Slave löscht. Beibehalten der Standardwert 60. Sie können auch Slave löschen, wenn er im Leerlauf ist heruntergefahren. Dazu aktivieren Sie das Kontrollkästchen **Nur Herunterfahren (nicht löschen) nach Retentionszeit** .

1. Klicken Sie in der Liste **Verwendung** auf die entsprechende Bedingung dieser untergeordnete Knoten verwendet wird. Jetzt klicken Sie auf **dieses Knotens so weit wie möglich nutzen**.

    Formular sollte jetzt etwa so aussehen:

    ![Allgemeine Vorlagenkonfiguration Checkpoint][checkpoint general template config]

    Im nächste Schritt werden Angaben über das Betriebssystemabbild, die Ihr Slave erstellt werden soll.

1. Im **Bild Familie oder Id** müssen Sie angeben, welche Systemabbild auf dem virtuellen Computer installiert werden. Wählen Sie aus einer Liste von Bild Familien oder ein benutzerdefiniertes Bild anzugeben.

    Wenn Sie aus einer Liste von Familien Bild auswählen möchten, geben Sie das erste Zeichen (Groß-/Kleinschreibung) den Namen des Abbilds. Beispielsweise wird **U** eingeben eine Liste der Ubuntu Server Familien bringen. Nachdem Sie aus der Liste auswählen, verwendet Jenkins die neueste Version dieses Systemabbild aus, beim virtuellen Computer bereitstellen.

    ![Betriebssystemabbild Liste Beispiel][OS Image list sample]

    Haben Sie ein benutzerdefiniertes Bild, das Sie verwenden möchten, geben Sie den Namen des benutzerdefinierten Bildes. Benutzerdefinierte Namen werden nicht in einer Liste angezeigt, um sicherzustellen, dass der Name richtig eingegeben haben.

    Für dieses Lernprogramm Typ **U** Ubuntu Abbilder bringen und dann auf **Ubuntu Server 14.04 LTS**.

1. Klicken Sie in der Liste **Starten** auf **SSH**.

1. Kopieren Sie das folgende Skript, und fügen Sie ihn im Feld **Init** .

        # Install Java

        sudo apt-get -y update

        sudo apt-get install -y openjdk-7-jdk

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y openjdk-7-jdk

        # Install git

        sudo apt-get install -y git

        #Install ant

        sudo apt-get install -y ant

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y ant

    Initskript wird nach dem Erstellen der virtuellen Computer ausgeführt. In diesem Beispiel werden das Skript Java, Git und Ant installiert.

1. Geben Sie Ihre bevorzugten Werte in den Feldern **Benutzername** und **Kennwort** für das Administratorkonto auf dem virtuellen Computer erstellt werden.

1. Klicken Sie auf **Vorlage überprüfen** prüfen, ob der angegebene Parameter gültig sind.

1. Klicken Sie auf **Speichern**.


## <a name="create-a-jenkins-job-that-runs-on-a-slave-node-on-azure"></a>Erstellen eines Auftrags Jenkins, das untergeordnete Knoten in Azure ausgeführt wird

In diesem Abschnitt werden Sie Jenkins Aufgabe erstellen, die untergeordnete Knoten in Azure ausgeführt wird. Sie müssen Ihr eigenes Projekt auf GitHub Folgen haben.

1. Klicken Sie im Schaltpult Jenkins auf **Neues Element**.

1. Geben Sie einen Namen für die Aufgabe, die Sie erstellen.

1. Klicken Sie auf **Freestyle Projekt**für den Projekttyp.

1. Klicken Sie auf **Ok**.

1. Wählen Sie die Seite Task-Konfiguration **beschränkt, in dem das Projekt ausgeführt werden kann**.

1. Geben Sie im Feld **Bezeichnung Ausdruck** **Linux**. Im vorherigen Abschnitt haben wir eine Vorlage Slave, dass wir mit dem Namen **Linux**, was wir hier angeben.

1. Im Abschnitt **Erstellen** auf **Buildschritt hinzufügen** , und wählen Sie **Execute-Shell**.

1. Bearbeiten Sie das folgende Skript ersetzen **(GitHub Kontonamen)** **(Projektname)**und **(Projektverzeichnis)** mit entsprechenden Werten, und fügen Sie das bearbeitete Skript im Textbereich.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e (your project directory) ]; then

            cd (your project directory)

            git pull origin master

        else

            git clone https://github.com/(your GitHub account name)/(your project name).git

        fi

        # change directory to project

        cd $currentDir/(your project directory)

        #Execute build task

        ant

1. Klicken Sie auf **Speichern**.

1. Im Dashboard Jenkins Mauszeiger gerade erstellten Task, und klicken Sie auf den Dropdown-Pfeil um Optionen anzuzeigen.

1. Klicken Sie auf **jetzt erstellen**.

Jenkins werden untergeordnete Knoten mithilfe der im vorherigen Abschnitt erstellten Vorlage erstellen und führen Sie das Skript in den Buildschritt für diesen Task angegebenen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Abonnementprofil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[cloud section]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-cloud-section.png
[subscription configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-account-configuration-fields.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-add-vm-template.png
[blank general configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration-blank.png
[checkpoint general template config]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration.png
[OS Image list sample]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-os-family-list-sample.png