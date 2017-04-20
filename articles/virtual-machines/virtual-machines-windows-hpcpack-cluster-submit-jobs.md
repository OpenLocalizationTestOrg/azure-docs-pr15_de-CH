<properties
 pageTitle="Senden Aufträge HPC Pack cluster in Azure | Microsoft Azure"
 description="Informationen Sie zum Einrichten eines lokalen Computers zu einem Cluster HPC Pack in Azure Aufträge senden"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>HPC Aufträge von einem lokalen Computer zu einem HPC Pack Cluster in Azure bereitgestellt

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Konfigurieren Sie einen lokalen Clientcomputer Aufträge zu einem Cluster [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) in Azure senden. Dieser Artikel veranschaulicht die lokalen Computer mit Clienttools Auftrag über HTTPS Cluster in Azure übermitteln einrichten. Auf diese Weise können mehrere Cluster zu einem cloudbasierten HPC Pack Cluster sondern ohne direkte Verbindung mit dem Head-Knoten VM Zugriff auf Azure-Abonnement senden.

![Senden eines Auftrags zu einem Cluster in Azure][jobsubmit]

## <a name="prerequisites"></a>Erforderliche Komponenten

* **HPC Pack Head-Knoten in einer VM Azure bereitgestellt** – wir empfehlen Sie automatisierte Tools eine [Azure PowerShell-Skript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) oder eine [Vorlage Azure Schnellstart](https://azure.microsoft.com/documentation/templates/) der Head-Knoten und Cluster. Sie benötigen den DNS-Namen des Head-Knotens und die Anmeldeinformationen der Clusterverwaltung die Schritte in diesem Artikel.

* **Client-Computer** - benötigen Sie einen Windows oder Windows Server Client-Computer, die Clientdienstprogramme HPC Pack ausführen können (siehe [Anforderungen](https://technet.microsoft.com/library/dn535781.aspx)). Wenn Sie nur HPC Pack Webportal oder REST-API verwenden, um Aufträge senden möchten, können Sie jedem Clientcomputer Ihrer Wahl.

* **HPC Pack-Installationsmedien** - HPC Pack-Clientdienstprogramme, kostenlose Installationspaket für die neueste Version von HPC Pack (HPC Pack 2012 R2) installiert ist im [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024)verfügbar. Stellen Sie sicher, dass Sie dieselbe Version von HPC Pack herunterladen, die auf dem Head-Knoten VM installiert ist.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Schritt 1: Installieren Sie und konfigurieren Sie die Webkomponenten auf dem Head-Knoten

Zum Aktivieren einer REST-Schnittstelle Aufträge zum Cluster über HTTPS senden, daß HPC Pack-Webkomponenten auf dem Headknoten HPC Pack konfiguriert sind. Wenn sie bereits installiert sind, zunächst Installieren der Webkomponenten Installationsdatei HpcWebComponents.msi ausführen. Konfigurieren Sie die Komponenten HPC PowerShell Skript **HPCWebComponents.ps1 festlegen**.

Ausführliche Informationen finden Sie in der [Microsoft HPC Pack-Webkomponenten installieren](http://technet.microsoft.com/library/hh314627.aspx).

>[AZURE.TIP] Bestimmte Vorlagen Azure Schnellstart für HPC Pack installieren und konfigurieren die Webkomponenten automatisch. Verwenden Sie das [Bereitstellungsskript HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) Cluster erstellen, können Sie optional installieren und konfigurieren die Webkomponenten als Teil der Bereitstellung.

**Webkomponenten installieren**

1. Eine Verbindung zu den Headknoten VM mit Clusteradministrator-Anmeldeinformationen.

2. HPC Pack Setup Ordner HpcWebComponents.msi auf dem Head-Knoten ausgeführt.

3. Folgen Sie den Schritten im Assistenten, um die Webkomponenten zu installieren

**So konfigurieren Sie die Webkomponenten**

1. Starten Sie HPC PowerShell auf dem Head-Knoten als Administrator.

2. Um den Speicherort des Konfigurationsskripts Verzeichnis ändern, geben Sie folgenden Befehl ein:

    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. Die REST-Schnittstelle konfigurieren und Starten der HPC-Webdienst, geben Sie folgenden Befehl ein:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```

4. Aufforderung ein Zertifikat auswählen wählen Sie das Zertifikat, das den öffentlichen DNS-Namen des Head-Knotens entspricht. Z. B. wenn den Head-Knoten mit dem klassischen Bereitstellungsmodell VM bereitstellen, der Zertifikatnamen sieht CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Verwenden Sie das Ressourcen-Manager-Bereitstellungsmodell, sieht der Name CN =&lt;*HeadNodeDnsName*&gt;. &lt; *Region*&gt;. cloudapp.azure.com.

    >[AZURE.NOTE] Auswahl dieses Zertifikat später beim Übermitteln von Aufträgen auf dem Head-Knoten von einem lokalen Computer. Nicht aktivieren oder Konfigurieren eines Zertifikats der Computername des Head-Knotens in der Active Directory-Domäne entspricht (z. B. CN =*MyHPCHeadNode.HpcAzure.local*).

5. Konfigurieren Sie das Webportal-Auftragsübermittlung Geben Sie folgenden Befehl ein:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. Nach Abschluss des Skripts beenden Sie und neu starten Sie der HPC-Auftragsplanungsdienst durch Eingabe der folgenden Befehle:

    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Schritt 2: Installieren Sie die Clientdienstprogramme HPC Pack auf einem lokalen computer

Wenn Sie die Clientdienstprogramme HPC Pack auf Ihrem Computer installieren möchten, herunterladen Sie HPC Pack Setup-Dateien (vollständige Installation) aus dem [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024) Wenn Sie mit die Installation beginnen, wählen Sie die Installationsoption für **HPC Pack-Clientdienstprogramme**.

Um die Clienttools HPC Pack Aufträge an den Headknoten VM senden, müssen Sie auch aus dem Head-Knoten exportieren und auf dem Clientcomputer installieren. Das Zertifikat muss. CER-Format.

**So exportieren Sie das Zertifikat aus dem Head-Knoten**

1. Auf dem Head-Knoten eine Microsoft Management Console für das lokale Computerkonto fügen Sie das Zertifikate-Snap-in hinzu. Hinzufügen des Snap-Ins finden Sie unter [Zertifikate-Snap-in einer MMC hinzufügen](https://technet.microsoft.com/library/cc754431.aspx).

2. Erweitern Sie in der Konsolenstruktur **Zertifikate – lokale Computer** > **Personal**, und klicken Sie dann auf **Zertifikate**.

3. Suchen Sie das Zertifikat für die Webkomponenten HPC Pack in konfiguriert [Schritt 1: Installieren und konfigurieren Sie die Webkomponenten auf dem Head-Knoten](#step-1:-install-and-configure-the-web-components-on-the-head-node) (z. B. CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net).

4. Maustaste auf das Zertifikat, und klicken Sie auf **Alle Aufgaben** > **Exportieren**.

5. Der Zertifikat-Assistenten klicken Sie auf **Weiter**, und **Nein, privaten Schlüssel nicht exportieren** ausgewählt ist.

6. Die verbleibenden Schritte des Assistenten zum Exportieren des Zertifikats in DER-codierte binäre x. 509 (. CER)-Format.


**So importieren Sie das Zertifikat auf dem Clientcomputer**


1. Kopieren Sie das Zertifikat aus dem Head-Knoten in einen Ordner auf dem Clientcomputer ausgeführt.

2. Führen Sie auf dem Clientcomputer certmgr.msc.

3. Certificate Manager erweitern Sie **Zertifikate-Aktueller Benutzer** > **Vertrauenswürdige Stammzertifizierungsstellen**Maustaste auf **Zertifikate**, und klicken Sie auf **Alle Aufgaben** > **Importieren**.

4. Der Zertifikat-Import-Assistenten auf **Weiter** , und folgen Sie den Schritten, um das Zertifikat zu importieren, das aus dem Head-Knoten im Speicher Vertrauenswürdige Stammzertifizierungsstellen exportiert.



>[AZURE.TIP] Vermutlich eine sicherheitswarnung, da der Clientcomputer die Zertifizierungsstelle auf dem Head-Knoten erkannt wird. Zu Testzwecken können Sie diese Warnung ignorieren und Zertifikatimport.

## <a name="step-3-run-test-jobs-on-the-cluster"></a>Schritt 3: Testaufträge im Cluster

Überprüfen Sie Ihre Konfiguration führen Sie Aufträge auf dem Cluster in Azure vom lokalen Computer. Beispielsweise können Sie Aufträge zum Cluster senden HPC Pack GUI-Tools oder Befehlen. Web-basierte Portal können Sie Aufträge senden.


**Übermittlung Befehle auf dem Client ausführen**


1. Starten Sie auf einem Clientcomputer dem HPC Pack-Clientdienstprogramme installiert sind, ein.

2. Geben Sie einen Befehl. Zum Auflisten aller Aufträge im Cluster beispielsweise einen Befehl wie den folgenden je nach den vollständigen DNS-Namen des Head-Knotens:

    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
    
    oder
    
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```

    >[AZURE.TIP] Verwenden Sie den vollständigen DNS-Namen der Head-Knoten nicht die IP-Adresse in der Planer-URL. Wenn Sie die IP-Adresse angeben, wird ein Fehler angezeigt wie "muss das Zertifikat entweder eine gültige Vertrauenskette oder im vertrauenswürdigen Stammspeicher abgelegt werden."

3. Geben Sie bei Aufforderung den Benutzernamen (im Format &lt;Domänenname&gt;\\&lt;Benutzername&gt;) und das Kennwort des HPC-Cluster-Administrator oder einem anderen clusterbenutzer, die Sie konfiguriert. Sie können Anmeldeinformationen für mehrere Einzelvorgänge an lokal speichern.

    Eine Liste der Aufträge angezeigt.


**HPC-Auftrags-Manager auf dem Client-Computer verwenden**

1. Wenn Sie Domänenanmeldeinformationen eines zuvor nicht beim Senden eines Auftrags speichern, können Sie die Anmeldeinformationen im Anmeldeinformations-Manager hinzufügen.

    ein. Starten Sie im Steuerungsbedienfeld auf dem Clientcomputer Anmeldeinformations-Manager.

    b. Klicken Sie auf **Windows-Anmeldeinformationen** > **generische Anmeldeinformationen hinzufügen**.

    c. Geben Sie die Internetadresse (z. B. https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler oder https://&lt;HeadNodeDnsName&gt;.&lt; Region&gt;.cloudapp.azure.com/HpcScheduler), und der Benutzername (&lt;Domänenname&gt;\\&lt;Benutzername&gt;) und das Kennwort für die Clusterverwaltung oder einem anderen clusterbenutzer, die Sie konfiguriert.

2. Starten Sie auf dem Clientcomputer HPC Auftrags-Manager.

3. Geben Sie die URL auf dem Head-Knoten in Azure im Dialogfeld **Hauptknoten wählen** (z. B. https://&lt;HeadNodeDnsName&gt;. cloudapp.net oder https://&lt;HeadNodeDnsName&gt;.&lt; Region&gt;. cloudapp.azure.com).

    HPC-Auftrags-Manager wird geöffnet und zeigt eine Liste der Aufträge auf dem Head-Knoten.

**Das Web-Portal auf dem Head-Knoten verwenden**

1. Starten Sie einen Web-Browser auf dem Clientcomputer, und geben Sie eine der folgenden Adressen je nach den vollständigen DNS-Namen des Head-Knotens:

    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
    
    oder
    
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. Geben Sie im Dialogfeld Sicherheit die Domänenanmeldeinformationen der HPC-Cluster-Administrator. (Sie können auch andere Cluster Benutzer in verschiedenen Rollen hinzufügen. Siehe [Verwalten von Benutzern Cluster](https://technet.microsoft.com/library/ff919335.aspx).)

    Webportal Listenansicht Auftrag wird geöffnet.

3. Klicken Sie ein Beispiel-Projekt, das die Zeichenfolge "Hello World" aus dem Cluster gibt **Neuer Auftrag** in der linken Navigationsleiste.

4. Klicken Sie auf der Seite **Neuer Auftrag** unter **Vorlage Seiten**auf **HelloWorld**. Der Auftrag senden wird angezeigt.

5. Klicken Sie auf **Senden**. Wenn Sie aufgefordert werden, Anmeldeinformationen der Domäne der HPC-Cluster-Administrator. Der Auftrag gesendet und die Auftrags-ID auf der Seite **Meine Projekte** angezeigt.

6. Um die Ergebnisse des Auftrags anzuzeigen, die Sie übermittelt, auf die Auftrags-ID und dann auf **Aufgaben anzeigen** , um die Ausgabe des Befehls (unter **Ausgabe**) anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

* Sie können auch Aufträge Azure Cluster mit [HPC Pack REST-API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx)senden.

* Clusteraufträge von einem Linux-Client senden, finden Sie unter Python-Beispiel [HPC Pack 2012 R2 SDK und Beispielcode](https://www.microsoft.com/download/details.aspx?id=41633).


<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
