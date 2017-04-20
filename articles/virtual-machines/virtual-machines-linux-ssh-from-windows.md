<properties 
    pageTitle="Verwenden Sie SSH-Schlüssel mit Windows für Linux VMs | Microsoft Azure" 
    description="Informationen Sie zum Generieren und mit SSH unter Windows um auf einer virtuellen Linux-Maschine auf Azure zu verbinden." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="squillace" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="rasquill"/>

# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Wie verwendet SSH-Schlüssel mit Windows Azure

> [AZURE.SELECTOR]
- [Windows](virtual-machines-linux-ssh-from-windows.md)
- [Linux-Mac](virtual-machines-linux-mac-create-ssh-keys.md)

Wenn Sie Linux virtuellen Maschinen (VMs) in Azure verbinden, verwenden Sie [Kryptographie](https://wikipedia.org/wiki/Public-key_cryptography) , sicherer Linux VM anmelden zu ermöglichen. Dieser Prozess beinhaltet öffentlichen und privaten Schlüsselaustausch Befehl secure Shell (SSH) selbst als Benutzername und Kennwort authentifizieren. Kennwörter sind anfällig für Brute-Force-Angriffen besonders auf Internetzugriff VMs wie Webserver. Dieser Artikel enthält eine Übersicht über SSH-Schlüssel und wie die entsprechenden Schlüssel auf einem windowscomputer.


## <a name="overview-of-ssh-and-keys"></a>Übersicht über SSH und Schlüssel

Sie können sicher auf Linux VM Anmelden mit öffentlichen und privaten Schlüsseln:

- Der **öffentliche Schlüssel** liegt auf Linux VM oder einen anderen Dienst mit öffentlichen Schlüsseln verwendet werden soll.
- Der **private Schlüssel** ist was Sie Linux VM präsentieren beim Anmelden zur Überprüfung Ihrer Identität. Dieser private Schlüssel zu schützen. Nicht freigeben.

Diese öffentliche und private Schlüssel können auf mehrere VMs und Dienste verwendet werden. Schlüssel ist nicht erforderlich, für jeden virtuellen Computer oder Dienst zugreifen möchten. Ausführlichere Informationen finden Sie unter [Kryptographie](https://wikipedia.org/wiki/Public-key_cryptography).

SSH ist ein verschlüsselt, die sichere Benutzernamen über eine unsichere Verbindung zulässt. Es ist das Standardprotokoll für Linux VMs in Azure gehostet. Zwar selbst SSH verschlüsselt, missbräuchliche SSH-Verbindungen Kennwörter verwenden weiterhin die VM Brute-Force-Angriffen oder erraten von Kennwörtern. Eine sichere und bevorzugte Methode einen virtuellen Computer über SSH wird mithilfe dieser öffentlichen und privaten Schlüsseln, auch SSH-Schlüssel.

Wenn Sie keine SSH-Schlüssel verwenden möchten, können Sie weiterhin auf Ihre Linux-VMs mit einem Kennwort anmelden. Wenn die VM nicht mit dem Internet verbunden ist, kann Kennwörter ausreichen. Allerdings müssen Sie immer noch zu verwalten Ihre Kennwörter für jede Linux VM fehlerfrei Kennwortrichtlinien und Praktiken wie minimale Kennwortlänge und regelmäßig aktualisieren. SSH-Schlüssel reduziert die Komplexität der Verwaltung von eigenen Anmeldeinformationen über mehrere VMs.


## <a name="windows-packages-and-ssh-clients"></a>Windows-Pakete und SSH-clients

Sie Verbinden mit und Linux VMs in Azure mit einem **ssh** -Client verwalten. Windows-Computer müssen nicht in der Regel einen **ssh** -Client installiert. Allgemeine Windows SSH-Clients zu installierende gehören die folgenden Pakete:

- [Git für Windows](https://git-for-windows.github.io/)
- [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
- [MobaXterm](http://mobaxterm.mobatek.net/)
- [Cygwin](https://cygwin.com/)

> [AZURE.NOTE] Das neueste Windows 10 Jahrestag beinhaltet Bash für Windows. Dieses Feature können Sie vom Windows-Subsystem für Linux und Access wie SSH-Client ausführen. Bash für Windows noch in der Entwicklung ist und Betaversion als wird. Weitere Informationen zu Windows Bash finden Sie unter [Bash auf Ubuntu unter Windows](https://msdn.microsoft.com/commandline/wsl/about).


## <a name="which-key-files-do-you-need-to-create"></a>Welche Dateien möchten Sie erstellen?

Azure erfordert mindestens 2048-Bit **ssh Rsa** formatieren öffentliche und private Schlüsseln. Verwalten von Azure Ressourcen mit dem klassischen Bereitstellungsmodell müssen auch zu einem PEM (`.pem` Datei).

Hier sind die Bereitstellungsszenarien und in jedem verwendeten Dateitypen:

1. **SSH-Rsa** -Schlüssel werden für jede Bereitstellung mithilfe der [Azure-Portal](https://portal.azure.com)und Ressourcenmanager Installationen mit der [Azure-CLI](../xplat-cli-install.md).
    - Diese Schlüssel sind in der Regel alle Benutzer.
2. `.pem`Datei muss VMs mit [klassischen Portal](https://manage.windowsazure.com)erstellen. Diese Schlüssel sind auch im klassischen Bereitstellung unterstützt, [Azure CLI](../xplat-cli-install.md)verwenden.
    - Sie müssen nur diese zusätzliche Schlüssel und Zertifikate erstellen, wenn Sie mit dem klassischen Bereitstellungsmodell erstellte Ressourcen verwalten.


## <a name="install-git-for-windows"></a>Git für Windows installieren

Im vorherigen Abschnitt aufgeführten mehrere Pakete, die die `openssl` -Tools für Windows. Dieses Tool muss öffentliche und private Schlüssel erstellen. In den folgenden Beispielen erläutert installieren und Verwenden der **Git für Windows**, wenn Sie Installationsprogramm können Sie bevorzugen. **Git für Windows** erhalten Sie Zugriff auf einige weitere Open Source-Software ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) Tools und Dienstprogramme, die beim Arbeiten mit Linux VMs nützlich sein können.

1. Downloaden und installieren Sie **Git für Windows** von folgender Adresse: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).

2. Während der Installation übernehmen Sie die Standardoptionen, wenn Sie nicht ausdrücklich ändern.

3. **Git Bash** vom **Startmenü**ausführen > **Git** > **Git Bash**. Die Konsole ähnelt dem folgenden Beispiel:

    ![Für Windows Git Bash shell](./media/virtual-machines-linux-ssh-from-windows/git-bash-window.png)


## <a name="create-a-private-key"></a>Erstellen eines privaten Schlüssels

1. Verwenden Sie das Fenster **Git Bash** `openssl.exe` einen privaten Schlüssel erstellen. Im folgenden Beispiel wird einen Schlüssel mit dem Namen `myPrivateKey` und mit dem Namen `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    Die Ausgabe ähnelt dem folgenden Beispiel:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

2. Beantworten Sie die Aufforderung Ländernamen, Position, Name der Organisation.

3. Ihre neuen privaten Schlüssel und das Zertifikat werden im aktuellen Arbeitsverzeichnis erstellt. Security best Practices sollten Sie die Berechtigungen für den privaten Schlüssel festlegen, damit nur Sie darauf zugreifen können:

    ```bash
    chmod 0600 myPrivateKey
    ```

4. Wenn Sie Classic Ressourcen verwalten möchten, konvertieren die `myCert.pem` , `myCert.cer` (DER codierte X509 Zertifikat). Durchführen Sie mit diesem optionalen Schritt nur soll insbesondere älteren Classic Ressourcen 

    Konvertieren Sie das Zertifikat mit dem folgenden Befehl:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>Erstellen Sie einen privaten Schlüssel für kitten

Kitten ist eine gemeinsame SSH-Client für Windows. SSH-Client verwenden können. Um kitten verwenden, müssen Sie eine zusätzliche Schlüssel - ein kitten Private Schlüssel (PPK) zu erstellen. Wenn Sie keine kitten verwenden möchten, überspringen Sie diesen Abschnitt.

Im folgenden Beispiel wird dieser zusätzliche private Schlüssel für kitten verwenden:

1. Mit **Git Bash** konvertieren Ihres privaten Schlüssels in einen privaten RSA-Schlüssel, die PuTTYgen verstehen kann. Im folgenden Beispiel wird einen Schlüssel mit dem Namen `myPrivateKey_rsa` aus dem vorhandenen Schlüssel mit dem Namen `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Security best Practices sollten Sie die Berechtigungen für den privaten Schlüssel festlegen, damit nur Sie darauf zugreifen können:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```

2. Herunterladen und Ausführen von PuTTYgen aus dem folgenden Speicherort: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

3. Menü: **Datei** > **Laden eines privaten Schlüssels**

4. Suchen Sie Ihren privaten Schlüssel (`myPrivateKey_rsa` im vorherigen Beispiel). Das Standardverzeichnis zu Beginn **Git Bash** `C:\Users\%username%`. Ändern den Dateifilter an **Alle Dateien (\*.\*)**:

    ![Den vorhandenen privaten Schlüssel in PuTTYgen laden](./media/virtual-machines-linux-ssh-from-windows/load-private-key.png)

5. Klicken Sie auf **Öffnen**. Eine Meldung gibt an, dass der Schlüssel erfolgreich importiert wurde:

    ![Erfolgreich importierten Schlüssel PuTTYgen](./media/virtual-machines-linux-ssh-from-windows/successfully-imported-key.png)

6. Klicken Sie auf **OK** , um die Meldung zu schließen.

7. Der öffentliche Schlüssel wird am oberen Fensterrand **PuTTYgen** angezeigt. Sie kopieren und Einfügen dieser öffentliche Schlüssel in Azure-Portal oder Azure Ressourcenmanager Vorlage beim Erstellen einer Linux VM. Sie können auch eine Kopie auf Ihrem Computer speichern **Speichern öffentlichen Schlüssel** klicken:

    ![PuTTY öffentliche Schlüsseldatei speichern](./media/virtual-machines-linux-ssh-from-windows/save-public-key.png)

    Das folgende Beispiel zeigt, wie Sie kopieren und Azure-Portal diesen öffentlichen Schlüssel einfügen, wenn Sie Linux VM erstellen. Der öffentliche Schlüssel werden dann normalerweise in `~/.ssh/authorized_keys` auf der neuen VM.

    ![Verwenden Sie öffentlichen Schlüssel, wenn Sie einen virtuellen Computer in Azure-Portal erstellen](./media/virtual-machines-linux-ssh-from-windows/use-public-key-azure-portal.png)

7. Klicken Sie im **PuTTYgen**auf **privaten Schlüssel speichern**:

    ![Kitten Privatschlüssel speichern](./media/virtual-machines-linux-ssh-from-windows/save-ppk-file.png)

    > [AZURE.WARNING] Frage möchten Sie fortfahren, ohne ein Kennwort für den Schlüssel. Eine Passphrase ist wie ein Kennwort auf Ihrem privaten Schlüssel zugeordnet. Selbst wenn jemand Ihren privaten Schlüssel abrufen, wäre sie noch keine Taste nur authentifizieren. Sie müssten ebenfalls die Passphrase. Ohne ein Kennwort erhält jemand Ihren privaten Schlüssel können sie anmelden VM oder Dienst, der den Schlüssel verwendet. Wir empfehlen eine Passphrase erstellen. Wenn Sie das Kennwort vergessen, gibt jedoch keine Möglichkeit, die Datenbank wiederherzustellen.

    Möchten Sie eine Passphrase eingeben, klicken Sie auf **Nein**, geben Sie ein Kennwort im Hauptfenster PuTTYgen und klicken Sie erneut auf **privaten Schlüssel speichern** . Klicken Sie anderenfalls auf **Ja** weiterhin ohne das optionale Kennwort.

8. Geben Sie einen Namen und Speicherort für die Datei PPK.


## <a name="use-putty-to-ssh-to-a-linux-machine"></a>Kitten SSH auf einem Linux-Computer verwenden

Kitten ist eine gemeinsame SSH-Client für Windows. SSH-Client verwenden können. Die folgenden Schritte beschreiben Verwendung Ihres privaten Schlüssels über SSH Azure-VM authentifiziert. Die beschriebenen Schritte in anderen SSH-Schlüssel Clients hinsichtlich Ihres privaten Schlüssels geladen werden müssen, SSH-Verbindung zu authentifizieren.

1. Herunterladen und Ausführen kitten aus dem folgenden Speicherort: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

2. Geben Sie den Hostnamen oder die IP-Adresse der VM von Azure-Portal:

    ![Neue PuTTY Verbindung öffnen](./media/virtual-machines-linux-ssh-from-windows/putty-new-connection.png)

3. Vor der Auswahl **Öffnen**, klicken Sie auf **Verbindung** > **SSH** > **Auth** -Registerkarte. Suchen Sie und wählen Sie Ihren privaten Schlüssel:

    ![Wählen Sie Ihren kitten Privatschlüssel für die Authentifizierung](./media/virtual-machines-linux-ssh-from-windows/putty-auth-dialog.png)

4. Klicken Sie auf **Öffnen** mit dem virtuellen Computer verbinden
 

## <a name="next-steps"></a>Nächste Schritte
Sie können auch den öffentlichen und privaten Schlüsseln [OS X und Linux](virtual-machines-linux-mac-create-ssh-keys.md)generieren.

Weitere Informationen zu Windows Bash und die Vorteile von OSS-Tools verfügbar auf Ihrem Windows-Computer finden Sie unter [Bash auf Ubuntu unter Windows](https://msdn.microsoft.com/commandline/wsl/about).

Haben Sie Probleme bei der Verwendung von SSH Verbindung zu Ihrem Linux-VMs, finden Sie unter [Problembehandlung SSH Verbindung mit einer Azure Linux VM](virtual-machines-linux-troubleshoot-ssh-connection.md).