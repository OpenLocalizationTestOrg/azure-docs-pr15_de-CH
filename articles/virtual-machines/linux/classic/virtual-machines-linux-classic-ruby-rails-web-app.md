<properties
    pageTitle="Ruby auf Schienen Website Linux VM Host | Microsoft Azure"
    description="Einrichten und Ruby auf Schienen-basierte Website mit einer virtuellen Linux-Maschine Azure gehostet."
    services="virtual-machines-linux"
    documentationCenter="ruby"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="web"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Ruby auf Schienen-Anwendung auf eine Azure-VM

Dieses Lernprogramm zeigt wie Ruby on Rails Website Azure mit virtuellen Linux-Maschine.  

In diesem Lernprogramm wurde mit Ubuntu Server 14.04 LTS bestätigt. Verwenden Sie eine andere Linux-Distribution, müssen Sie die Schritte zur Installation der Schienen ändern.

> [AZURE.IMPORTANT] Azure hat zwei verschiedene Bereitstellungsmodelle für erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und Classic](../../../resource-manager-deployment-model.md).  Dieser Artikel behandelt das klassische Bereitstellungsmodell verwenden. Microsoft empfiehlt, die neue Ressourcen-Manager-Modell verwendet.

## <a name="create-an-azure-vm"></a>Erstellen einer Azure VM

Zunächst eine Azure-VM mit einem Linux-Image erstellen.

Erstellen Sie den virtuellen Computer können Sie klassischen Azure-Portal oder der Azure-Befehlszeilenschnittstelle (CLI).

### <a name="azure-management-portal"></a>Azure-Verwaltungsportal

1. Das [klassische Azure-Portal](http://manage.windowsazure.com) anmelden
2. **Klicken Sie auf** > **berechnen** > **virtuellen** > **schnell erstellen**. Linux-Abbild auswählen.
3. Geben Sie ein Kennwort ein.

Nachdem der virtuelle Computer bereitgestellt wird, der Name auf und klicken Sie auf **Dashboard**. Finden Sie den SSH-Endpunkt unter **SSH Details**aufgeführt.

### <a name="azure-cli"></a>Azure CLI

Folgen Sie den Schritten unter [Erstellen einer Virtual Machine ausgeführt Linux][vm-instructions].

Nachdem der virtuelle Computer bereitgestellt wird, erhalten Sie SSH-Endpunkt, durch Ausführen des folgenden Befehls:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Ruby on Rails installieren

1. SSH Verbindung zur VM verwenden.

2. Verwenden Sie die folgenden Befehle Ruby auf dem virtuellen Computer installieren, über SSH-Sitzung:

        sudo apt-get update -y
        sudo apt-get upgrade -y
        sudo apt-get install ruby ruby-dev build-essential libsqlite3-dev zlib1g-dev nodejs -y

    Die Installation kann einige Minuten dauern. Wenn er abgeschlossen ist, verwenden Sie folgenden Befehl, ob Ruby installiert ist:

        ruby -v

    Dies gibt die Versionsnummer des Ruby installiert wurde.

3. Verwenden Sie den folgenden Befehl Schienen zu installieren:

        sudo gem install rails --no-rdoc --no-ri -V

    Verwendung-Nein Rdoc und -ri keine Flags zum Installieren der Dokumentation schneller.
    Dieser Befehl dauert möglicherweise lange ausgeführt, Hinzufügen von V - Informationen zum Fortschritt Installation angezeigt werden.

## <a name="create-and-run-an-app"></a>Erstellen und Ausführen einer Anwendung

Noch über SSH angemeldet, führen Sie die folgenden Befehle:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

Der [neue](http://guides.rubyonrails.org/command_line.html#rails-new) Befehl erstellt eine neue Schienen app. Der Befehl [Server](http://guides.rubyonrails.org/command_line.html#rails-server) startet WEBrick-Webserver, der mit Schienen. (Für die Produktion, möglicherweise möchten Sie einen anderen Server Einhorns oder Fahrgast verwenden.)

Die folgende Ausgabe sollte angezeigt werden.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Einen Endpunkt hinzufügen

1. [Azure-Verwaltungsportal] zu[ management-portal] , und wählen Sie die virtuellen Computer.

    ![Liste der virtuellen Computer][vmlist]

2. Wählen Sie **ENDPUNKTE** am oberen Rand der Seite und dann auf **+ ENDPUNKT hinzufügen** am unteren Rand der Seite.

    ![Seite][endpoints]

3. Das Dialogfeld **ENDPUNKT hinzufügen** "Standalone-Endpunkt hinzufügen" und klicken Sie auf **Weiter** .

    ![neuen Endpunkt-Dialogfeld][new-endpoint1]

3. Geben Sie im nächsten Dialogfeld Seite die folgenden Informationen ein:

    * **NAME**: HTTP

    * **Protokoll**: TCP

    * **Öffentlicher PORT**: 80

    * **Privater PORT**: 3000

    Dies erstellt einen öffentlichen Port 80, die private Port 3000, Datenverkehr wird, wo der Schienen Server abhört.

    ![neuen Endpunkt-Dialogfeld][new-endpoint]

4. Klicken Sie auf das Häkchen, um den Endpunkt zu speichern.

5. Eine Meldung sollte angezeigt **UPDATE IN PROGRESS**. Nachdem diese Meldung ausgeblendet wird, ist der Endpunkt aktiv. Sie können die Anwendung jetzt zu den DNS-Namen des virtuellen Computers testen. Die Website sollte ähnlich der folgenden angezeigt:

    ![Standardseite Schienen][default-rails-cloud]

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie die meisten Schritte manuell. In einer produktiven Umgebung Sie Ihre app auf einer schreiben und Bereitstellen der Azure-VM. Auch host die meisten produktionsumgebungen Schienen Anwendung in Verbindung mit einem anderen Server Apache oder NginX mehrere Instanzen der Anwendung Schienen routing und statische Ressourcen dienen die Handles anfordern. Weitere Informationen finden Sie unter http://rubyonrails.org/deploy/.

Erfahren Sie mehr über Ruby on Rails besuchen [Ruby on Rails Hilfslinien][rails-guides].

Azure Services von Ruby-Anwendung finden Sie unter:

* [Speichern von unstrukturierten Daten mit blobs][blobs]

* [Speicher-Schlüssel-Wert-Paare mit Tabellen][tables]

* [Bedienen Sie hohe Bandbreite Inhalt mit Content Delivery Network][cdn-howto]

<!-- WA.com links -->
[blobs]: ../../../storage/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]: https://azure.microsoft.com/develop/ruby/app-services/
[management-portal]: https://manage.windowsazure.com/
[tables]: ../../../storage/storage-ruby-how-to-use-table-storage.md
[vm-instructions]: ../../virtual-machines-linux-classic-createportal.md

<!-- External Links -->
[rails-guides]: http://guides.rubyonrails.org/
[sqlite3]: http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png
