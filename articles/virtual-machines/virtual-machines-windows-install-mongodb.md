<properties
    pageTitle="MongoDB auf Windows VM installieren | Microsoft Azure"
    description="Informationen Sie zum Installieren von MongoDB auf eine Azure-VM unter Windows Server 2012 R2 mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Installieren und Konfigurieren von MongoDB auf einen virtuellen Computer Windows Azure
[MongoDB](http://www.mongodb.org) ist eine beliebte Open Source, hochleistungsfähigen NoSQL-Datenbank. Dieser Artikel leitet Sie durch die Installation und Konfiguration von MongoDB auf einem Windows Server 2012 R2 virtuellen Computer (VM) in Azure. Sie können auch [auf einer Linux VM in Azure MongoDB installieren](virtual-machines-linux-install-mongodb.md).


## <a name="prerequisites"></a>Erforderliche Komponenten

Vor dem Installieren und Konfigurieren von MongoDB müssen Sie einen virtuellen Computer erstellen und im Idealfall Datenträger hinzufügen. Finden Sie in folgenden Artikeln einen virtuellen Computer erstellen und einen Datenträger hinzufügen:

- [Erstellen einer Windows-VM mit Azure-Portal](virtual-machines-windows-hero-tutorial.md) oder [Erstellen Sie eine Windows Server-VM mit Azure PowerShell](virtual-machines-windows-ps-create.md)
- [Attach einen Datenträger auf einem Windows Server-VM mit Azure-Portal](virtual-machines-windows-attach-disk-portal.md) oder [Attach einen Datenträger auf einem Windows Server-VM Azure PowerShell](https://msdn.microsoft.com/library/mt603673.aspx)
    
Installieren und Konfigurieren von MongoDB mit Remotedesktop [Anmelden an Windows Server-VM](virtual-machines-windows-connect-logon.md) zu beginnen.


## <a name="install-mongodb"></a>MongoDB installieren

> [AZURE.IMPORTANT] MongoDB Sicherheitsfunktionen wie Authentifizierung und IP-Adresse binden sind standardmäßig nicht aktiviert. Sicherheitsfunktionen sollte aktiviert werden, bevor MongoDB in einer produktiven Umgebung bereitstellen. Weitere Informationen finden Sie unter [MongoDB Sicherheit und Authentifizierung](http://www.mongodb.org/display/DOCS/Security+and+Authentication).

1. Nachdem Sie die VM Remotedesktop verbunden sind, öffnen Sie Internet Explorer im **Startmenü** des virtuellen Computers.

2. Wählen Sie **empfohlen, Sicherheit, Datenschutz und kompatibilitätseinstellungen** beim Öffnen von Internet Explorer und klicken Sie auf **OK**.

3. Verstärkte Sicherheitskonfiguration für Internet Explorer ist standardmäßig aktiviert. MongoDB Website zur Liste zugelassener Websites hinzufügen:

    - Wählen Sie **Extras** -Symbol in der oberen rechten Ecke.
    - **Internetoptionen**wählen Sie die Registerkarte **Sicherheit** , und wählen Sie das Symbol **Vertrauenswürdige Sites** .
    - Klicken Sie auf die Schaltfläche **Sites** . Hinzufügen _https://\*. mongodb.org_ zur Liste der vertrauenswürdigen Sites und schließen Sie dann das Dialogfeld.

    ![Konfigurieren von Internet Explorer-Sicherheit](./media/virtual-machines-windows-install-mongodb/configure-internet-explorer-security.png)

4. Wechseln Sie zu der Seite [MongoDB - Downloads](http://www.mongodb.org/downloads) (http://www.mongodb.org/downloads).

5. Standardmäßig sollte es **Community Server** -Edition und die aktuelle stabile Version für Windows Server 2008 R2 64-Bit oder höher wählen. Klicken Sie auf **DOWNLOAD (Msi)**, um das Installationsprogramm.

    ![MongoDB Installer downloaden](./media/virtual-machines-windows-install-mongodb/download-mongodb.png)

    Führen Sie das Installationsprogramm aus, nachdem der Download abgeschlossen ist.

6. Lesen Sie und akzeptieren Sie den Lizenzvertrag. Wenn Sie aufgefordert werden, wählen Sie **vollständig** installieren.

7. Klicken Sie auf dem letzten Bildschirm auf **Installieren**.


## <a name="configure-the-vm-and-mongodb"></a>Konfigurieren Sie die VM und MongoDB

1. Die Path-Variablen werden vom Installationsprogramm MongoDB nicht aktualisiert. Ohne die MongoDB `bin` Position in der Path-Variablen müssen Sie den vollständigen Pfad jedes Mal angeben eine MongoDB ausführbare Datei verwenden. Der Speicherort der Path-Variablen hinzu:

    - Klicken Sie im **Startmenü** und **Wählen**.
    - Klicken Sie auf **Erweiterte Systemeinstellungen**und dann auf **Umgebungsvariablen**.
    - Klicken Sie unter **Systemvariablen**wählen Sie **Pfad aus**und dann auf **Bearbeiten**.

    ![Konfigurieren der PATH-Variablen](./media/virtual-machines-windows-install-mongodb/configure-path-variables.png)

    Fügen Sie den Pfad zu Ihrem MongoDB `bin` Ordner. MongoDB befindet sich normalerweise im `C:\Program Files\MongoDB`. Überprüfen Sie den Installationspfad für die VM. Das folgende Beispiel fügt standardmäßig MongoDB Installationsort, die `PATH` Variable:

    ```
    ;C:\Program Files\MongoDB\Server\3.2\bin
    ```

    > [AZURE.NOTE] Unbedingt das vorangestellte Semikolon (`;`) an eine Position zum Hinzufügen der `PATH` Variable.

2. Erstellen Sie MongoDB Daten- und Verzeichnisse auf der Festplatte Daten. Wählen Sie im Menü **Start** **Befehlszeile**. In den folgenden Beispielen erstellen Sie Verzeichnisse auf Laufwerk F:

    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```

3. MongoDB Instanz starten, mit dem folgenden Befehl den Pfad zu den Daten anpassen und Verzeichnisse entsprechend protokolliert:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```

    Es kann mehrere Minuten dauern MongoDB die Protokolldateien und hören für Verbindungen. Alle Lognachrichten richten als Datei *F:\MongoLogs\mongolog.log* `mongod.exe` Server startet und Protokolldateien belegt.

    > [AZURE.NOTE] Das Eingabeaufforderungsfenster bleibt auf diese Aufgabe, während MongoDB-Instanz ausgeführt wird. Lassen Sie das Eingabeaufforderungsfenster geöffnet MongoDB fort. Oder installieren Sie MongoDB als Dienst wie im nächsten Schritt.

4. Eine stabilere MongoDB Erfahrung Installieren der `mongod.exe` als Dienst. Erstellen eines Diensts bedeutet jedem gewünschten MongoDB ausgeführte Befehlszeile lassen müssen. Erstellen Sie den Dienst, den Pfad für die Daten- und Protokolldateien Verzeichnisse entsprechend anpassen:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```

    Dieser Befehl erstellt einen Dienst namens MongoDB, mit einer Beschreibung des "Mongo DB". Außerdem werden die folgenden Parameter angegeben:

    - Die `--dbpath` Option gibt den Speicherort des Datenverzeichnisses.
    - Die `--logpath` Option muss an eine Protokolldatei verwendet werden, da der ausgeführte Dienst kein Befehlsfenster ausgeben.
    - Die `--logappend` Option gibt an, dass ein Neustart des Dienstes Ausgabe an die vorhandene Protokolldatei angefügt wird.

  Führen Sie den folgenden Befehl, um die MongoDB-Dienst zu starten:

    ```
    net start MongoDB
    ```

    Weitere Informationen zum Erstellen des MongoDB-Dienstes finden Sie unter [Configure Windows-Dienst für MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-the-mongodb-instance"></a>Testen Sie die MongoDB-Instanz

Mit MongoDB als einzelne Instanz ausgeführt oder als Dienst installiert können Sie nun erstellen und Verwenden von Datenbanken. Starten Sie die administrative MongoDB-Shell öffnen Sie ein anderes Fenster über **das Startmenü** und geben Sie den folgenden Befehl:

```
mongo  
```

Datenbanken mit Listen der `db` Befehl. Fügen Sie Daten wie folgt:

```
db.foo.insert( { a : 1 } )
```

Suchen Sie nach Daten ein:

```
db.foo.find()
```

Die Ausgabe ist ähnlich dem folgenden Beispiel:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Beenden der `mongo` Konsole wie folgt:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Firewall und Netzwerk-Sicherheitsgruppe Regeln konfigurieren
Da MongoDB installiert und ausgeführt wird, öffnen Sie einen Port in der Windows-Firewall so Remote MongoDB hergestellt werden kann. Erstellen Sie eine neue eingehende Regel zum Zulassen der TCP-Port 27017 öffnen Sie eine administrative PowerShell Prompt, und geben Sie den folgenden Befehl:

```powerShell
New-NetFirewallRule -DisplayName "Allow MongoDB" -Direction Inbound `
    -Protocol TCP -LocalPort 27017 -Action Allow
```

Die Regel können mithilfe des **Windows-Firewall mit erweiterter Sicherheit** grafisch Management-Tools. Erstellen Sie eine neue eingehende Regel zum Zulassen der TCP-Port 27017.

Erstellen Sie bei Bedarf eine Netzwerk-Sicherheitsgruppe Regel MongoDB von außerhalb des Subnetzes für vorhandene Azure virtuelles Netzwerk Zugriff. Sie können Netzwerk-Sicherheitsgruppe Regeln mithilfe der [Azure-Portal](virtual-machines-windows-nsg-quickstart-portal.md) oder [Azure PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md)erstellen. Können Sie mit der Windows-Firewall-Regeln TCP-Port 27017 der virtuellen Schnittstelle des MongoDB VM.

> [AZURE.NOTE] TCP-Port 27017 ist der Standardport MongoDB verwendet. Sie können mit diesen Port ändern die `--port` Parameter beim Starten `mongod.exe` manuell oder von einem Dienst. Wenn Sie den Port ändern, müssen Sie die Windows-Firewall und Netzwerk-Sicherheitsgruppe Regeln in den vorhergehenden Schritten aktualisieren.


## <a name="next-steps"></a>Nächste Schritte
In diesem Lernprogramm haben Sie zum Installieren und Konfigurieren von MongoDB auf Ihrem Windows-VM. Jetzt können Sie MongoDB auf Ihrem Windows-VM auf die erweiterten Themen in der [MongoDB-Dokumentation](https://docs.mongodb.com/manual/)zugreifen.
