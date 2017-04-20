<properties
    pageTitle="Verwendung von Azure Slave-Plug-in mit Hudson fortlaufende Integration | Microsoft Azure"
    description="Beschreibt das plug-in Azure Slave mit kontinuierlicher Integration Hudson."
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

# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>Verwendung von Azure Slave-Plug-in mit Hudson fortlaufende Integration

Plug-in für Hudson Azure Slave können Sie untergeordnete Knoten in Azure bereitstellen, beim Ausführen von verteilten erstellt.

## <a name="install-the-azure-slave-plug-in"></a>Installieren der Azure-Slave-Plug-in

1. Klicken Sie im Schaltpult Hudson **Hudson verwalten**.

1. Klicken Sie auf **Hudson verwalten** auf **Plug-Ins verwalten**.

1. Klicken Sie auf die Registerkarte **verfügbar** .

1. Klicken Sie auf **Suchen** , und geben Sie **Azure** , um die Liste zu relevanten Plug-ins.

    Wenn Sie einen Bildlauf durch die Liste der verfügbaren Plug-Ins entscheiden, finden Azure Slave-Plug-in Sie unter Abschnitt **Clusterverwaltung und verteilte erstellen** auf der Registerkarte **andere** .

1. Aktivieren Sie das Kontrollkästchen für **Azure Slave Plug-in**.

1. Klicken Sie auf **Installieren**.

1. Starten Sie Hudson.

Nun, dass das plug-in installiert ist, wäre die nächsten Schritte mit der Azure-Abonnement plug-in konfigurieren und Erstellen einer Vorlage, die verwendet werden in die VM für untergeordnete Knoten erstellen.

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>Konfigurieren der Azure-Slave-Plug-in mit Ihrem Abonnement

Ein Abonnement auch Einstellungen zu veröffentlichen ist eine XML-Datei mit sicheren Anmeldeinformationen und Weitere Informationen zu Azure in Umgebung arbeiten müssen. Konfigurieren Sie die Azure Slave-Plug-in benötigen Sie:

* Die Abonnement-id
* Zeugnisse für Ihr Abonnement

Diese können in Ihrem [Abonnementprofil]gefunden. Im folgenden ist ein Beispiel eines Abonnements.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Haben Sie Ihr Abonnementprofil folgendermaßen Azure Slave-Plug-in konfigurieren.

1. Klicken Sie im Schaltpult Hudson **Hudson verwalten**.

1. Klicken Sie auf **System konfigurieren**.

1. Unten Sie auf der Seite im Abschnitt **Cloud** .

1. Klicken Sie auf **Hinzufügen neuen Cloud > Microsoft Azure**.

    ![neuen Cloud hinzufügen][add new cloud]

    Diese zeigen die Felder sollten Ihre Abonnementdetails eingeben.

    ![Profil konfigurieren][configure profile]

1. Das Abonnement-Id und Management Zertifikat aus Ihrem Abonnementprofil kopieren und in die entsprechenden Felder einfügen.

    Beim Kopieren des Abonnement-Id und Management Zertifikats enthalten **nicht** angeboten, die die Werte schließen.

1. Klicken Sie auf **Überprüfen**.

1. Wenn die Konfiguration erfolgreich überprüft wird, klicken Sie auf **Speichern**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Einrichten einer Vorlage für virtuelle Computer für die Azure-Plug-in

Eine Vorlage für virtuelle Computer definiert Parameter verwendeten Plug-Ins Azure untergeordnete Knoten erstellen. In den folgenden Schritten werden wir Vorlage für Ubuntu-VM erstellen.

1. Klicken Sie im Schaltpult Hudson **Hudson verwalten**.

1. Klicken Sie auf **System konfigurieren**.

1. Unten Sie auf der Seite im Abschnitt **Cloud** .

1. **Im Bereich **Cloud** hinzufügen Azure Virtual Machine aus,** und klicken Sie auf die Schaltfläche **Hinzufügen** .

    ![Vm-Vorlage hinzufügen][add vm template]

1. Cloud-Dienstnamen im Feld **Name** angeben. Wenn einem vorhandenen Clouddienst der Namen verweist, wird die VM in diesem Dienst bereitgestellt werden. Azure erstellen, eine neue.

1. Geben Sie im Feld **Beschreibung** ein, die die Vorlage beschreibt, die Sie erstellen. Diese Informationen werden nur für Dokumentationszwecke und nicht in einer VM-Bereitstellung verwendet.

1. Geben Sie im Feld **Beschriftung** **Linux**. Diese Bezeichnung wird verwendet, um die Vorlage zu identifizieren, die Sie erstellen und anschließend auf die Vorlage beim Erstellen eines Auftrags Hudson verwendet.

1. Wählen Sie einen Bereich, in dem die VM erstellt werden.

1. Wählen Sie die entsprechende VM.

1. Geben Sie ein Speicherkonto, der virtuellen Computer erstellt werden. Stellen Sie sicher, dass im Bereich Cloud-Dienst ist, den Sie verwenden. Wenn Sie neuen Speicher erstellt werden soll, können Sie dieses Feld leer lassen.

1. Aufbewahrungszeit gibt die Anzahl der Minuten Hudson Leerlauf Slave löscht. Beibehalten der Standardwert 60.

1. **Verwendung**wählen Sie die entsprechende Bedingung dieser untergeordnete Knoten verwendet werden aus Wählen Sie jetzt **Nutzen dieser Knoten so weit wie möglich**.

    An dieser Stelle würde das Formular etwa so aussehen:

    ![Vorlagenkonfiguration][template config]

1. **Bild-Produktfamilie** oder Id müssen Sie angeben, welche Systemabbild auf Ihrem virtuellen Computer installiert werden. Wählen Sie aus einer Liste von Bild Familien oder ein benutzerdefiniertes Bild anzugeben.

    Wenn Sie aus einer Liste von Familien Bild auswählen möchten, geben Sie das erste Zeichen (Groß-/Kleinschreibung) den Namen des Abbilds. Beispielsweise wird **U** eingeben eine Liste der Ubuntu Server Familien bringen. Sobald Sie aus der Liste auswählen, verwendet Jenkins die neueste Version des Bildes System aus, wenn die VM-Bereitstellung.

    ![Liste der BS-Familie][OS family list]

    Haben Sie ein benutzerdefiniertes Bild, das Sie verwenden möchten, geben Sie den Namen des benutzerdefinierten Bildes. Benutzerdefinierte Namen werden nicht in einer Liste angezeigt, um sicherzustellen, dass der Name richtig eingegeben haben.    

    Geben Sie für dieses Lernprogramm **U** um Ubuntu Abbilder und wählen **Ubuntu Server 14.04 LTS**.

1. Wählen Sie **SSH** **Methode starten**.

1. Das folgende Skript kopieren und Einfügen im Feld **Init-Skript** .

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

    **Init-Skript** wird ausgeführt, nachdem die VM erstellt. In diesem Beispiel werden das Skript Java, Git und Ant installiert.

1. Geben Sie Ihre bevorzugten Werte in den Feldern **Benutzername** und **Kennwort** für das Administratorkonto auf Ihrem virtuellen Computer erstellt werden.

1. Klicken Sie auf **Vorlage überprüfen** überprüft, ob der angegebene Parameter gültig sind.

1. Klicken Sie auf **Speichern**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Erstellen eines Auftrags Hudson, das untergeordnete Knoten in Azure ausgeführt wird

In diesem Abschnitt werden Sie Hudson Aufgabe erstellen, die untergeordnete Knoten in Azure ausgeführt wird.

1. Klicken Sie im Schaltpult Hudson auf **Neuer Auftrag**.

1. Geben Sie einen Namen für den Auftrag, den Sie erstellen.

1. Der Einzelvorgangstyp wählen Sie **erstellen einen Auftrag frei Software**.

1. Klicken Sie auf **OK**.

1. Wählen Sie auf der Konfigurationsseite Auftrag **beschränken, in dem das Projekt ausgeführt werden kann**.

1. Wählen Sie **Knoten und das Etikett** und **Linux** (angegeben diese Bezeichnung beim Erstellen der Vorlage für virtuelle Computer im vorherigen Abschnitt).

1. Im Abschnitt **Erstellen** auf **Buildschritt hinzufügen** , und wählen Sie **Execute-Shell**.

1. Bearbeiten Sie das folgende Skript **{Github Kontonamen}** **{Projektname}**und **{Projektverzeichnis}** durch entsprechende Werte ersetzen, und fügen Sie das bearbeitete Skript im Textbereich.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e {your project directory} ]; then

            cd {your project directory}

            git pull origin master

        else

            git clone https://github.com/{your github account name}/{your project name}.git

        fi

        # change directory to project

        cd $currentDir/{your project directory}

        #Execute build task

        ant

1. Klicken Sie auf **Speichern**.

1. Im Dashboard Hudson den gesuchten Sie Auftrag nur erstellt und klicken Sie auf **Zeitplan eines Builds** .

Hudson erstellen Sie einen untergeordnete Knoten im vorherigen Abschnitt erstellte Vorlage und führen Sie das Skript in den Buildschritt für diesen Task angegebenen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Abonnementprofil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

