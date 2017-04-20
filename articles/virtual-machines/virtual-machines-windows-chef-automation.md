<properties
   pageTitle="Azure VM-Bereitstellung mit | Microsoft Azure"
   description="Lernen Sie Chef automatisierte VM-Bereitstellung und Konfiguration auf Microsoft Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="diegoviso"
   manager="timlt"
   tags="azure-service-management,azure-resource-manager"
   editor=""/>

<tags ms.service="virtual-machines-windows" ms.workload="infrastructure-services"
ms.tgt_pltfrm="vm-multiple"
ms.devlang="na"
ms.topic="article"
ms.date="05/19/2015"
ms.author="diviso"/>

# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Automatisierung der Bereitstellung mit Azure Virtual machine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Chef ist ein hervorragendes Tool für die Bereitstellung von Automation und des gewünschten Status Konfigurationen.

Unsere Cloud-API-Version bietet Chef nahtlos in Azure, die Möglichkeit für die Bereitstellung und Konfiguration Status über einen einzelnen Befehl bereitstellen.

In diesem Artikel werde ich zeigen, wie Ihr Chef Umgebung Azure bereitstellen und Sie durch Erstellen einer Richtlinie oder "Kochbuch" und Bereitstellen dieses Kochbuch Azure virtuellen Computer eingerichtet.

Beginnen!

## <a name="chef-basics"></a>Grundlagen der Chef

Bevor Sie beginnen, empfehle ich die grundlegenden Konzepte der Chef überprüfen. Ist Material <a href="http://www.chef.io/chef" target="_blank">hier</a> und empfehlen eine schnelle Lesen vor dieser exemplarischen Vorgehensweise. Ich werden jedoch die Grundlagen rekapitulieren, bevor wir beginnen.

Das folgende Diagramm zeigt die vielschichtige Architektur Chef.

![][2]

Chef hat drei wichtigste Komponenten: Chef Server Chef-Client (Knoten) und Chef-Arbeitsstation.

Der Chef Server unserer Verwaltungspunkt und es gibt zwei Optionen für den Chef Server: eine gehostete oder lokale Lösung. Wir verwenden eine gehostete Lösung.

Der Chef Client (Knoten) ist der Agent auf den Servern befindet, die Sie verwalten.

Chef-Arbeitsstation ist unsere Verwaltungsarbeitsstation wo wir unsere Richtlinien erstellen und unsere Befehle ausführen. Wir führen den Befehl **Messer** der Chef Workstation unsere Infrastruktur.

Außerdem ist das Konzept der "Kochen" und "Rezepte". Diese sind effektiv die wir definieren und anwenden auf unseren Servern.

## <a name="preparing-the-workstation"></a>Vorbereiten der Arbeitsstation

Erstens können Sie die Arbeitsstation vorbereiten. Ich verwende Windows-Arbeitsstationen. Wir müssen unsere Konfigurationsdateien und Kochen Verzeichnis erstellen.

Zunächst erstellen Sie ein Verzeichnis namens C:\chef.

Erstellen Sie eine zweite Verzeichnis c:\chef\cookbooks.

Nun müssen wir unsere Azure Einstellungsdatei herunterladen, damit unsere Azure-Abonnement Chef kommunizieren kann.

Herunterladen der Einstellungen [hier](https://manage.windowsazure.com/publishsettings/)veröffentlicht.

Speichern Sie Datei veröffentlichen in C:\chef.

##<a name="creating-a-managed-chef-account"></a>Erstellung eines verwalteten Chef

Eine gehostete Chef Konto [hier](https://manage.chef.io/signup).

Während des Anmeldeprozesses werden Sie aufgefordert, eine neue Organisation erstellen.

![][3]

Sobald Ihre Organisation erstellt wurde, herunterladen Sie das Starterkit

![][4]

> [AZURE.NOTE] Wenn Sie aufgefordert werden, dass die Schlüssel zurückgesetzt werden, kann wie wir keine Infrastruktur wie konfiguriert haben.

Dieses Starter Kit Zip-Datei enthält Ihre organisationsdateien und Schlüssel.

##<a name="configuring-the-chef-workstation"></a>Die Chef Workstation konfigurieren

Extrahieren Sie den Inhalt der Chef-starter.zip, C:\chef.

Kopieren Sie alle Dateien unter Chef Starter\chef Repo\.Chef zum Verzeichnis c:\chef.

Das Verzeichnis sollte jetzt folgendermaßen aussehen.

![][5]

Sie haben nun vier Dateien einschließlich der Azure veröffentlichen Datei im Stammverzeichnis des c:\chef.

Das PEM Dateien Ihrer Organisation und Admin private Schlüssel für die Kommunikation während der knife.rb der Messer-Konfigurations enthält. Wir müssen die Datei knife.rb.

Öffnen Sie die Datei in Ihren Editor Ihrer Wahl, und ändern Sie "Cookbook_path" durch Entfernen der /... / Pfad so wie weiter.

    cookbook_path  ["#{current_dir}/cookbooks"]

Außerdem fügen Sie die folgende Zeile reflektieren den Namen Ihrer Azure veröffentlichen Einstellungsdatei.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Die knife.rb-Datei sollte jetzt wie im folgenden Beispiel aussehen.

![][6]

Diese Zeilen werden sichergestellt, dass Messer verweist auf das Verzeichnis Kochbücher unter c:\chef\cookbooks und unsere Azure veröffentlichen Einstellungsdatei bei Azure verwendet.

## <a name="installing-the-chef-development-kit"></a>Der Chef Development Kit installieren

[Downloaden und installieren Sie](http://downloads.getchef.com/chef-dk/windows) ChefDK (Chef Development Kit) der Arbeitsstation Chef eingerichtet.

![][7]

Installation im Standardverzeichnis c:\opscode. Die Installation dauert ca. 10 Minuten.

Bestätigen Sie, dass die Variable PATH enthält Einträge für C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Nicht vorhanden sind, stellen Sie sicher, dass diese Pfade hinzufügen.

*HINWEIS DIE REIHENFOLGE DES PFADS IST WICHTIG!* Die Opscode-Pfade sind nicht in der richtigen Reihenfolge haben Sie Probleme.

Starten Sie die Arbeitsstation neu, bevor Sie fortfahren.

Anschließend wird die Messer Azure-Erweiterung installiert. Auf diese Weise mit dem Plugin"Azure".

Führen Sie den folgenden Befehl ein.

    chef gem install knife-azure ––pre

> [AZURE.NOTE] Pre-Argument stellt sicher, dass Sie neueste Vorabversion Messer Azure Plugin erhalten Zugriff auf die neuesten APIs.

Es ist wahrscheinlich eine Reihe Abhängigkeiten auch gleichzeitig installiert werden.

![][8]


Um sicherzustellen, dass alles richtig konfiguriert ist, führen Sie den folgenden Befehl ein.

    knife azure image list

Wenn alles richtig konfiguriert ist, sehen Sie eine Liste der verfügbaren Azure Bilder blättern.

Herzlichen Glückwunsch! Die Arbeitsstation ist eingerichtet!

##<a name="creating-a-cookbook"></a>Erstellen eine Kochbuch

Eine Kochbuch Chef dient eine Reihe von Befehlen zu definieren, die auf den verwalteten Client ausgeführt werden soll. Erstellt eine Kochbuch ist einfach und wir Befehl **Chef generieren Kochbuch** Kochbuch Vorlagen generieren. Ich werden Kochbuch Webserver aufrufen, wie ich eine Richtlinie, die IIS automatisch bereitgestellt wird.

Führen Sie den folgenden Befehl in das Verzeichnis C:\Chef.

    chef generate cookbook webserver

Dadurch wird eine Reihe von Dateien im Verzeichnis C:\Chef\cookbooks\webserver generiert. Nun müssen wir definieren die Befehle möchten wir unseren Chef Client unsere verwalteten virtuellen Computer ausführen.

Die Befehle werden in der Datei default.rb gespeichert. In dieser Datei werde ich eine Reihe von Befehlen definiert, die IIS installiert, startet IIS eine Vorlagendatei in den Ordner "Wwwroot" kopiert.

Ändern Sie die Datei C:\chef\cookbooks\webserver\recipes\default.rb, und fügen Sie folgende Zeilen.

    powershell_script 'Install IIS' do
        action :run
        code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
        action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
        source 'Default.htm.erb'
        rights :read, 'Everyone'
    end

Speichern Sie die Datei, sobald Sie fertig sind.

## <a name="creating-a-template"></a>Erstellen einer Vorlage

Wie zuvor erwähnt, muss eine Vorlagendatei generieren als unsere default.html Seite verwendet wird.

Führen Sie den folgenden Befehl die Vorlage generiert.

    chef generate template webserver Default.htm

Nun suchen Sie die Datei C:\chef\cookbooks\webserver\templates\default\Default.htm.erb. Bearbeiten Sie die Datei durch Hinzufügen von einfachen "Hello World" HTML-Code, und speichern Sie die Datei.



## <a name="upload-the-cookbook-to-the-chef-server"></a>Kochbuch Chef Server hochladen

In diesem Schritt kopieren Kochbuch, die wir unsere lokalen erstellt haben und wir Chef gehosteten Server hochladen. Nach dem Hochladen wird Kochbuch Registerkarte **Richtlinien** angezeigt.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Bereitstellen einer virtuellen Maschine mit Messer Azure

Wir Azure virtuellen Computer bereitstellen und anwenden das Cookbook "Webserver" IIS Web Service und Standard Webseite installieren.

Dazu verwenden Sie den Befehl **Messer Azure Server erstellen** .

Am Beispiel des Befehls weiter angezeigt.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

Die Parameter sind selbsterklärend. Bestimmten Variablen ersetzen und führen.

> [AZURE.NOTE] Über die Befehlszeile aus ich bin auch automatisieren Endpunkt Netzwerk Filterregeln mit dem Parameter-Tcp-Endpunkte. Ich habe die Ports 80 und 3389 Zugriff auf Meine Webseite und RDP-Sitzung geöffnet.

Nach Ausführung des Befehls zum Azure-Portal und sehen Ihre Computer Bereitstellung beginnen.

![][13]

Die Befehlszeile wird weiter angezeigt.

![][10]

Nachdem die Bereitstellung abgeschlossen ist, sollte man den Web Service über Port 80 herstellen wir den Port geöffnet war, wenn wir den virtuellen Computer mit dem Befehl Messer Azure bereitgestellt. Wie diesem virtuellen Computer nur virtuellen Computer im Cloud-Dienst, werde ich mit der Cloud-Url Verbindung herstellen.

![][11]

Wie Sie sehen können, habe ich mit HTML-Code creative.

Vergessen Sie nicht, dass wir auch über eine RDP-Sitzung von klassischen Azure-Portal über Port 3389 herstellen können.

Ich hoffe, dies hilfreich. Wechseln und Ihre Infrastruktur wie Code mit Azure jetzt!


<!--Image references-->
[2]: ./media/virtual-machines-windows-chef-automation/2.png
[3]: ./media/virtual-machines-windows-chef-automation/3.png
[4]: ./media/virtual-machines-windows-chef-automation/4.png
[5]: ./media/virtual-machines-windows-chef-automation/5.png
[6]: ./media/virtual-machines-windows-chef-automation/6.png
[7]: ./media/virtual-machines-windows-chef-automation/7.png
[8]: ./media/virtual-machines-windows-chef-automation/8.png
[9]: ./media/virtual-machines-windows-chef-automation/9.png
[10]: ./media/virtual-machines-windows-chef-automation/10.png
[11]: ./media/virtual-machines-windows-chef-automation/11.png
[13]: ./media/virtual-machines-windows-chef-automation/13.png


<!--Link references-->
