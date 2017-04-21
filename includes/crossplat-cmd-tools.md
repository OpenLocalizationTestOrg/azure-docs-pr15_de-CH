#<a name="how-to-use-the-azure-command-line-tools-for-mac-and-linux"></a>Verwendung der Azure-Befehlszeilentools für Mac und Linux

Dieses Handbuch beschreibt das Erstellen und Verwalten von Services in Azure Azure Befehlszeilentools für Mac und Linux verwenden. Die Szenarios enthalten **die Tools installieren**, **Importieren Einstellungen veröffentlichen**, **Erstellen und Verwalten von Azure Websites**und **Erstellen und Verwalten von Azure Virtual Machines**. Umfassende Referenzinformationen finden Sie unter [Azure Befehlszeilenprogramm für Mac und Linux-Dokumentation][reference-docs]. 

##<a name="table-of-contents"></a>Inhaltsverzeichnis
* [Was sind Azure-Befehlszeilentools für Mac und Linux](#Overview)
* [Azure-Befehlszeilentools für Mac und Linux installieren](#Download)
* [Ein Azure-Konto erstellen](#CreateAccount)
* [Zum Herunterladen und importieren Settings](#Account)
* [Zum Erstellen und Verwalten einer Azure-Website](#WebSites)
* [Zum Erstellen und Verwalten von Azure Virtual Machine](#VMs)


<h2><a id="Overview"></a>Was sind Azure-Befehlszeilentools für Mac und Linux</h2>

Azure-Befehlszeilentools für Mac und Linux sind eine Reihe von Befehlszeilenprogrammen zum Bereitstellen und Verwalten von Azure Services.
 
Unterstützten Aufgaben umfassen Folgendes:

* Importieren Sie publishing Einstellungen.
* Erstellen und Verwalten von Azure Websites.
* Erstellen und Verwalten von Azure Virtual Machines.

Geben Sie eine vollständige Liste der unterstützten Befehle `azure -help` in der Befehlszeile nach der Installation der Tools oder finden Sie in der [Dokumentation zur][reference-docs].

<h2><a id="Download">Azure-Befehlszeilentools für Mac und Linux installieren</a></h2>

Die folgende Liste enthält Informationen zum Installieren von Befehlszeilentools je nach Betriebssystem:

* **Mac**: Download des Windows [Azure SDK Installer][mac-installer]. Heruntergeladene .pkg-Datei öffnen und die Installation Schritte, wenn Sie aufgefordert werden.

* **Linux**: Installieren der neueste Version von [Node.js] [ nodejs-org] (siehe [Installation Node.js über Paket-Manager][install-node-linux]), führen Sie folgenden Befehl:

        npm install azure-cli -g

    **Hinweis**: Sie müssen diesen Befehl mit erhöhten Rechten ausführen:

        sudo npm install azure-cli -g

* **Windows**: Führen Sie Winows Installer (MSI) hier: [Azure-Befehlszeilentools][windows-installer].


Geben Sie zum Testen der Installation `azure` in der Befehlszeile. Wenn die Installation erfolgreich war, sehen Sie eine Liste mit allen verfügbaren `azure` Befehle.

<h2><a id="CreateAccount"></a>Ein Azure-Konto erstellen</h2>

Um Azure-Befehlszeilentools für Mac und Linux zu verwenden, benötigen Sie ein Azure-Konto.

Öffnen Sie einen Webbrowser, und navigieren Sie zu [http://www.windowsazure.com] [ windowsazuredotcom] und **kostenlose Testversion** in der oberen rechten Ecke klicken.

![Azure-Website][Azure Web Site]

Führen Sie die Schritte zum Erstellen eines Kontos.

<h2><a id="Account"></a>Zum Herunterladen und importieren Settings</h2>

Zunächst müssen Sie zuerst herunterladen und Importieren der Einstellungen zu veröffentlichen. So können Sie die Tools zum Erstellen und Verwalten von Azure Services verwenden. Herunterladen der Einstellungen zu veröffentlichen, verwenden Sie die `account download` Befehl:

    azure account download

Dies wird als Standardbrowser öffnen und fordert Sie Verwaltungsportal anmelden. Nach der Anmeldung, die `.publishsettings` Datei wird heruntergeladen. Notieren Sie, wo diese Datei gespeichert ist.

Anschließend importieren der `.publishsettings` Datei durch Ausführen des folgenden Befehls ersetzen `{path to .publishsettings file}` durch den Pfad zu der `.publishsettings` Datei:

    azure account import {path to .publishsettings file}

Entfernen Sie alle Informationen von der <code>import</code> Befehl mithilfe der <code>account clear</code> Befehl:

    azure account clear

Um eine Liste der Optionen finden Sie unter `account` Befehle, die `-help` Option:

    azure account -help

Nach dem Importieren der Einstellungen veröffentlichen, löschen Sie die `.publishsettings` Datei aus Sicherheitsgründen.

> [AZURE.NOTE] Beim Importieren von Einstellungen veröffentlichen, Anmeldeinformationen für den Zugriff auf Ihre Azure-Abonnement befinden sich innerhalb der `user` Ordner. Die `user` Ordner vom Betriebssystem geschützt. Jedoch wird empfohlen, dass Sie zusätzliche Verfahren zum Verschlüsseln der `user` Ordner. So führen Sie die folgenden Methoden:    
> 
> - Unter Windows die Ordnereigenschaften ändern oder BitLocker verwenden.
> - Mac für den Ordner FileVault aktivieren.
> - Ubuntu verwenden Sie verschlüsselte Home Directory Feature. Andere Linux-Distributionen bieten entspricht.

Sie können jetzt erstellen und Verwalten von Azure Websites und Azure Virtual Machines.  

<h2><a id="WebSites"></a>Zum Erstellen und Verwalten einer Azure-Website</h2>

###<a name="create-a-website"></a>Erstellen einer Website

Erstellen Sie eine Azure-Website zunächst ein leeres Verzeichnis `MySite` und in dieses Verzeichnis.

Führen Sie den folgenden Befehl:

    azure site create MySite --git

Die Ausgabe dieses Befehls enthält die Standard-URL für die neu erstellte Website. Die `--git` können Sie mit Git Git Repositories erstellen, in dem lokalen Anwendungsverzeichnis und Rechenzentrum Ihrer Website auf Ihrer Website veröffentlichen. Dabei ist der lokale Ordner bereits ein Git Repository der Befehl neue Remote auf das Repository im Rechenzentrum Ihrer Website vorhandene Repository hinzugefügt wird.

Notiz, die Sie ausführen können, die `azure site create` Befehl mit einer der folgenden Optionen:

* `--location [location name]`. Mit dieser Option können Sie den Speicherort des Rechenzentrums angeben, in der Ihre Website (z. B. "West US") erstellt wird. Wenn Sie diese Option auslassen, werden Sie Passworte wählen.
* `--hostname [custom host name]`. Mit dieser Option können Sie einen benutzerdefinierten Hostnamen für die Website angeben.

Sie können das Verzeichnis Website Inhalte hinzugefügt werden. Verwenden Sie das reguläre Git (`git add`, `git commit`) dazu Ihre Inhalte. Befehl der folgenden Git Inhalt in Azure drücken: 

    git push azure master

###<a name="set-up-publishing-from-github"></a>Veröffentlichung von GitHub einrichten

Verwenden, um fortlaufende Veröffentlichung von GitHub Repository einzurichten, die `--GitHub` option beim Erstellen einer Website:

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

Wenn Sie einen lokalen Klon eines GitHub Repository oder haben ein Repository mit einzelnen entfernten GitHub Repository dieser Befehl Code im Repository GitHub zu Ihrer Website veröffentlichen. Danach Änderungen GitHub Repository abgelegt automatisch zu Ihrer Website erscheint.

Beim Einrichten der Veröffentlichung von GitHub ist der Standardzweig verwendet master Zweig. Zum Angeben einer anderen Verzweigung von Ihrem lokalen Repository führen Sie den folgenden Befehl aus:

    azure site repository <branch name>

###<a name="configure-app-settings"></a>Anwendung konfigurieren

App sind Schlüssel-Wert-Paaren, die die Anwendung zur Laufzeit zur Verfügung stehen. Wenn für eine Azure-Website festgelegt überschreiben app Werte Einstellungen mit demselben Schlüssel in der Datei Web.config der Site definiert werden. Node.js und PHP sind app-Einstellungen als Umgebungsvariablen verfügbar. Das folgende Beispiel zeigt, wie ein Schlüssel-Wert-Paar:

    azure site config add <key>=<value> 

Um eine Liste aller Schlüssel-Wert-Paaren, die folgenden:

    azure site config list 

Oder wissen den Schlüssel und den Wert anzeigen können:

    azure site config get <key> 

Möchten Sie den Wert eines vorhandenen Schlüssels ändern müssen Sie zunächst den vorhandenen Schlüssel deaktivieren und erneut hinzufügen. Der Befehl Löschen ist:

    azure site config clear <key> 

###<a name="list-and-show-sites"></a>Auflisten und Anzeigen von Websites

Um Ihre Websites aufzulisten, verwenden Sie den folgenden Befehl ein:

    azure site list

Detaillierte Informationen zu einer Website mithilfe der `site show` Befehl. Das folgende Beispiel zeigt Details für `MySite`:

    azure site show MySite

###<a name="stop-start-or-restart-a-site"></a>Beenden, starten oder Neustarten einer Website

Beenden, starten oder neu starten eine Site mit den `site stop`, `site start`, oder `site restart` Befehle:

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

###<a name="delete-a-site"></a>Löschen einer Website

Sie können außerdem eine Site mit dem `site delete` Befehl:

    azure site delete MySite

Beachten Sie, dass Sie über Befehle im Ordner ausgeführt werden, haben `site create`, Sie müssen nicht den Websitenamen angeben `MySite` als letzten Parameter.

Um eine vollständige Liste finden Sie unter `site` Befehle, die `-help` Option:

    azure site -help 

<h2><a id="VMs"></a>Zum Erstellen und Verwalten von Azure Virtual Machine</h2>

Azure Virtual Machine wird, die Sie bereitstellen oder verfügbar in der Bildergalerie, Abbild eines virtuellen Computers (VHD-Datei) erstellt. Um die verfügbaren Bilder anzuzeigen, verwenden Sie die `vm image list` Befehl:

    azure vm image list

Bereitstellen und Starten eines virtuellen Computers eines der verfügbaren Bilder mit den `vm create` Befehl. Im folgenden Codebeispiel zum Erstellen einer virtuellen Linux-Maschine (genannt `myVM`) aus einem Bild in der Bildergalerie (CentOS 6.2). Root-Benutzernamen und Kennwort für den virtuellen Computer sind `myusername` und `Mypassw0rd` bzw.. (Beachten Sie, dass die `--location` Parameter gibt das Data Center in der virtuellen Computer erstellt. Wenn Sie auslassen der `--location` Parameter, werden Sie aufgefordert, einen Speicherort auswählen.)

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

Sollten Sie übergeben der `--ssh` Flag (Linux) oder `--rdp` -Flag (Windows) `vm create` , Remoteverbindungen mit dem neu erstellten virtuellen Computer.

Wenn stattdessen eine virtuelle Maschine von einem benutzerdefinierten Bild bereitstellen möchten, können ein Bild aus einer VHD-Datei mit den `vm image create` Befehl und anschließend die `vm create` Befehl Bereitstellung virtueller Computer. Im folgenden Beispiel wird veranschaulicht, wie ein Linux-Image erstellen (genannt `myImage`) von lokalen VHD-Datei. (Die `--location` Parameter gibt die Daten in dem das Bild gespeichert ist.)

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

Anstatt ein Bild aus einer lokalen VHD können Sie ein Bild aus einer VHD in Azure BLOB-Speicher gespeicherte erstellen. Sie erreichen dies mit der `blob-url` Parameter:

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

Sie können einen virtuellen Computer aus dem Bild nach dem Erstellen eines Abbildes bereitstellen, mit `vm create`. Der folgende Befehl erstellt eine virtuelle Maschine namens `myVM` aus dem Bild oben erstellte (`myImage`).

    azure vm create myVM myImage myusername --location "West US"

Nachdem Sie einen virtuellen Computer bereitgestellt haben, sollten Sie für Ihre virtuellen Computer (beispielsweise) Remotezugriff Endpunkte erstellen. Im folgenden Beispiel wird die `vm create endpoint` zum Öffnen von externen Port 22 und lokalen Port 22 auf `myVM`:

    azure vm endpoint create myVM 22 22

Sie erhalten detaillierte Informationen zu einem virtuellen Computer (einschließlich IP-Adresse, DNS-Namen und Endpunktinformationen) mit den `vm show` Befehl:

    azure vm show myVM

Beenden Starten Sie, oder starten Sie den virtuellen Computer, verwenden Sie einen der folgenden Befehle:

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

Um den virtuellen Computer zu löschen, verwenden Sie die `vm delete` Befehl:

    azure vm delete myVM

Verwenden Sie eine vollständige Liste von Befehlen zum Erstellen und Verwalten von virtuellen Maschinen, die `-h` Option:

    azure vm -h

<!-- IMAGES -->
[Azure Web Site]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com

