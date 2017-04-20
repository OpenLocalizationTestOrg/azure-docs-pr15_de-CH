 <properties
   pageTitle="Azure und Linux | Microsoft Azure"
   description="Azure Compute, Speicher und Netzwerke beschreibt mit virtuelle Linux-Computer."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines-linux"
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/14/2016"
   ms.author="v-livech"/>

# <a name="azure-and-linux"></a>Azure und Linux

Microsoft Azure ist eine wachsende Sammlung von integrierten öffentlichen cloud-Dienste einschließlich Analytics VMs, Datenbanken, mobil, Netzwerk, Speicher und web – ideal für Projektmappen.  Microsoft Azure bietet eine skalierbare computing-Plattform, mit der Sie nur bezahlen, was Sie verwenden, wenn Sie wollen - ohne zu lokalen Hardware.  Azure ist bereit, Ihre Projektmappen skalieren und, was Maßstab Sie benötigen, um die Bedürfnisse Ihrer Kunden.

Wenn Sie die verschiedenen Funktionen von Amazon kennen AWS überprüfen Sie Azure Vs AWS [Dokuments zuordnen](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).


## <a name="regions"></a>Regionen
Microsoft Azure Ressourcen sind mehrere Regionen auf der ganzen Welt verteilt.  "Region" stellt mehrere Rechenzentren in einem geografischen Bereich dar.  Ab dem 1. Januar 2016 dabei: 8 in Amerika, Europa, Asien, 2 in der Volksrepublik China und Indien 3 6 2.  Eine vollständige Liste aller Azure Bereiche gegebenenfalls pflegen wir eine Liste der vorhandenen und Regionen angekündigt.

- [Azure-Regionen](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Verfügbarkeit
Für die Bereitstellung unserer 99,95 VM Service Level Agreement qualifizieren müssen zwei bereitstellen oder mehr VMs mit der Arbeitslast innerhalb einer festgelegt. Dadurch Ihre VMs verteilt über mehrere Fehlerdomänen in Rechenzentren sowie auf Servern mit unterschiedlichen Wartungsfenster bereitgestellt. Die vollständige [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) erläutert die garantierte Verfügbarkeit von Azure als Ganzes.

## <a name="azure-virtual-machines--instances"></a>Azure Virtual Machines und Instanzen
Microsoft Azure unterstützt eine Anzahl von Populäre Linux-Distributionen bereitgestellt und von Partnern verwaltet.  Distributionen, wie Red Hat Enterprise, CentOS, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD Azure Marketplace finden. Wir arbeiten aktiv mit verschiedenen Linux Liste [Azure Linux-Distributionen unterstützt](virtual-machines-linux-endorsed-distros.md) mehr Typen hinzu.

Ist Ihre bevorzugte Linux-Distribution Wahl nicht derzeit in der Galerie Sie "bringt eigene Linux" VM [Erstellen](virtual-machines-linux-create-upload-generic.md)und Hochladen einer virtuellen Linux in Azure.

Azure virtuelle Computer können Sie eine Vielzahl von computing-Lösungen agile-Methode bereitstellen. Sie können praktisch jede Arbeitslast und jede Sprache auf nahezu jedem Betriebssystem - Windows, Linux oder ein benutzerdefiniertes erstellt einer der wachsende Liste von Partnern. Immer noch sehen nicht Sie suchen?  Keine Sorge – Sie bringen auch eigene Bilder lokal.

## <a name="vm-sizes"></a>VM-Größen
Beim Bereitstellen einer VM in Azure soll auswählen VM in einer Reihe von Größen, die für Ihre Arbeit. Die Größe wirkt sich auch die Verarbeitungskapazität, Speicher, und Speicher des virtuellen Computers. Sie werden basierend auf den Zeitraum die VM ausführen und die zugeordneten Ressourcen berechnet. Eine vollständige Liste von [virtuellen Maschinen](virtual-machines-linux-sizes.md).

Hier sind einige grundlegenden Richtlinien zum Auswählen einer VM aus einer Reihe (A, D, DS, G und GS).

* Eine Reihe VMs sind unsere Einsteiger VMs für helle Arbeitslasten und Test-/Preis. Sie sind in allen Regionen verfügbar verbinden und alle standard-Ressourcen für virtuelle Maschinen verwenden.
* Eine Reihe sind (A8 - A11) spezielle Compute intensive Konfigurationen einsetzbar High Performance computing Cluster.
* D-Serie VMs sollen Programme ausführen, die höhere rechenleistung temporärer Leistung und. D-Serie VMs bieten schnellere Prozessoren, ein höheres Verhältnis Speicher Core und Solid State Drive (SSD) für die temporäre Diskette.
* Dv2-Serie ist die neueste Version der D-Serie, bietet eine leistungsfähigere CPU. Dv2-Serie CPU ist etwa 35 % schneller als die CPU D-Serie. Basiert auf der neuesten Generation 2,4 GHz Intel Xeon® E5 2673 v3 (Haskell) Prozessor und Intel Turbo Boost Technologie 2.0 bis zu 3,2 GHz. Dv2-Serie verfügt über die gleichen Arbeitsspeicher- und Konfigurationen als der D-Serie.
* G-Serie VMs bieten die meisten Speicher und auf Hosts, die Familie Prozessoren Intel Xeon E5 V3.

Hinweis: DS-Serie und GS-Serie VMs auf Storage Premium - unsere SSD unterstützt hohe Leistung, niedriger Latenz Speicher für e/a-intensive Arbeitslasten. Premium-Speicher ist in bestimmten Regionen verfügbar. Weitere Informationen finden Sie unter:

- [Premium-Speicher: Hochleistungsspeicher für Azure Virtual Machine-Arbeitslasten](../storage/storage-premium-storage.md)

## <a name="automation"></a>Automatisierung
Um einer DevOps Kultur zu erreichen, muss die gesamte Infrastruktur Code.  Wenn alle lebt die Infrastruktur im Code kann es leicht (Phoenix Server) neu erstellt.  Azure verwendet alle wichtigen Tools wie Ansible, Koch, SaltStack und Marionetten Automatisierung.  Azure hat auch eigene Tools für die Automatisierung:

- [Azure-Vorlagen](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

- [Azure VMAccess](virtual-machines-linux-using-vmaccess-extension.md)

Azure rollt Unterstützung für [Cloud-Init](http://cloud-init.io/) über die meisten Linux-Distros, die sie unterstützen.  Derzeit werden die Canonical Ubuntu VMs mit Cloud-Init standardmäßig bereitgestellt.  RedHats RHEL CentOS und Fedora Cloud Init unterstützen, jedoch Azure Bilder von RedHat verwaltet nicht Cloud Init installiert.  Um Cloud Init auf einem RedHat OS verwenden, müssen Sie ein benutzerdefiniertes Bild mit Cloud-Init-Installation erstellen.

- [Cloud-Init verwenden Azure Linux VMs](virtual-machines-linux-using-cloud-init.md)

## <a name="quotas"></a>Kontingente
Jede Azure-Abonnement hat Kontingentstandardgrenzen, der die Bereitstellung eine große Anzahl virtueller Computer für das Projekt auswirken. Die aktuelle Begrenzung auf pro beträgt 20 VMs pro Region.  Kontingente können durch Einreichung von Support-Ticket anfordern eine höhere Grenzen ausgelöst werden.  Weitere Informationen zu Kontingentgrenzen:

- [Azure Abonnement beschränkt](../azure-subscription-service-limits.md)


## <a name="partners"></a>Partner

Microsoft arbeitet eng mit unseren Partnern, damit die Bilder aktualisiert und Azure Runtime optimiert.  Weitere Informationen zu unseren Partnern prüfen Sie die nachfolgenden Seiten marketplace

- [Linux auf Azure unterstützt Distributionen](virtual-machines-linux-endorsed-distros.md)

RedHat - [Azure Marketplace - RedHat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)

Kanonische - [Azure Marketplace - Ubuntu Server 16.04 LTS](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)

Debian - [Azure Marketplace - Debian 8 "Jessie"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)

FreeBSD - [Azure Marketplace - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)

CoreOS - [Azure Marketplace - CoreOS (Stable)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)

RancherOS - [Azure Marketplace - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)

Bitnami - [Bitnami-Bibliothek für Azure](https://azure.bitnami.com/)

Mesosphäre - [Azure Marketplace - Mesosphäre DC/OS auf Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)

Andockfenster - [Azure Marketplace - Containerdienst mit Andockfenster Schwarm Azure](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)

Jenkins - [Azure Marketplace - CloudBees Jenkins Plattform](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)


## <a name="getting-setup-on-azure"></a>Erste Einrichtung in Azure
Mit Azure benötigen Sie ein Azure-Konto, der Azure-CLI installiert und SSH öffentlichen und privaten Schlüssel.

## <a name="sign-up-for-an-account"></a>Melden Sie sich für ein Konto
Der erste Schritt bei der Azure Cloud ist für ein Azure-Konto anmelden.  Wechseln Sie zur Seite [Azure Konto einrichten](https://azure.microsoft.com/pricing/free-trial/) zu beginnen.

## <a name="install-the-cli"></a>Die CLI installieren
Mit dem neuen Azure-Konto können Sie loslegen über Azure-Portal ist eine webbasierte Administrationsseite.  Zum Verwalten der Azure-Cloud über die Befehlszeile installieren Sie die `azure-cli`.  [Azure-CLI ](../xplat-cli-install.md)auf der Arbeitsstation Mac oder Linux installieren.

## <a name="create-an-ssh-key-pair"></a>SSH-Schlüsselpaar erstellen
Haben Sie ein Azure-Konto Azure Webportal und Azure-CLI.  Im nächste Schritt wird ein SSH-Schlüsselpaar erstellen, SSH in Linux ohne Verwendung eines Kennworts.  [Erstellen Sie SSH-Schlüssel unter Linux und Mac](virtual-machines-linux-mac-create-ssh-keys.md) Kennwort weniger Benutzernamen und Sicherheit.

## <a name="getting-started-with-linux-on-microsoft-azure"></a>Erste Schritte mit Microsoft Azure Linux
Ein Azure-Konto einrichten, Azure-CLI installiert und SSH-Schlüssel erstellt können Sie nun in der Azure-Cloud-Infrastruktur aufbauen.  Die erste Aufgabe ist ein paar VMs zu erstellen.

## <a name="create-a-vm-using-the-cli"></a>Erstellen Sie einen virtuellen Computer mit der CLI
Erstellen einer Linux VM mit der CLI ist schnell bereitstellen eine VM ohne Terminal Sie arbeiten.  Alles, was Sie im Webportal können steht ein Befehlszeilenprogramm Flag oder Switch.  

- [Erstellen einer Linux VM mit CLI](virtual-machines-linux-quick-create-cli.md)

## <a name="create-a-vm-in-the-portal"></a>Erstellen Sie einen virtuellen Computer im portal
Linux VM in Azure Webportal erstellen ist ganz einfach zeigen und auf die verschiedenen Optionen zu einer Bereitstellung.  Anstatt Befehlszeilenflags oder Switches können Sie verschiedene Optionen und eine schöne Weblayoutansicht.  Alles über die Befehlszeile steht auch im Portal.

- [Erstellen einer Linux VM über das Portal](virtual-machines-linux-quick-create-portal.md)

## <a name="login-using-ssh-without-a-password"></a>Anmeldung mit SSH ohne Kennwort
Die VM wird jetzt in Azure ausgeführt, und Sie können sich anmelden.  Verwenden von Kennwörtern über SSH Anmelden ist unsicher.  Mit SSH-Schlüssel ist der sicherste Weg und Anmeldung am schnellsten.  Wenn Sie Linux VM über das Portal oder die CLI erstellen, müssen Sie zwei Authentifizierungsoptionen.  Wenn Sie ein für SSH Passwort konfiguriert Azure VM um Benutzernamen über Kennwörter ermöglichen.  Wenn Sie einen öffentlichen SSH-Schlüssel verwenden, Azure konfiguriert die VM damit nur Benutzernamen über SSH-Schlüssel und Kennwort Benutzernamen deaktiviert. Um Linux VM sichern nur SSH-Schlüssel-Benutzernamen, die Option SSH öffentliche Schlüssel während der Erstellung VM im Portal oder CLI.

- [Deaktivieren Sie SSH-Kennwörter auf Linux VM SSHD konfigurieren](virtual-machines-linux-mac-disable-ssh-password-usage.md)

## <a name="related-azure-components"></a>Azure-Komponenten

## <a name="storage"></a>Speicher

- [Einführung in Microsoft Azure-Speicher](../storage/storage-introduction.md)

- [Hinzufügen eines Datenträgers einer Linux VM mit der Azure-cli](virtual-machines-linux-add-disk.md)

- [Wie Datenträger einer Linux VM in Azure-portal](virtual-machines-linux-attach-disk-portal.md)

## <a name="networking"></a>Netzwerk

- [Virtuelles Netzwerk (Übersicht)](../virtual-network/virtual-networks-overview.md)

- [IP-Adressen in Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md)

- [Öffnen von Ports für Linux VM in Azure](virtual-machines-linux-nsg-quickstart.md)

- [Erstellen Sie einen vollqualifizierten Domänennamen in Azure-portal](virtual-machines-linux-portal-create-fqdn.md)


## <a name="containers"></a>Container

- [Virtuelle Maschinen und Container in Azure](virtual-machines-linux-containers.md)

- [Einführung in Azure Container Service](../container-service/container-service-intro.md)

- [Bereitstellen eines Clusters Azure Container Service](../container-service/container-service-deployment.md)

## <a name="next-steps"></a>Nächste Schritte

Sie haben nun einen Überblick über Linux auf Azure.  Der nächste Schritt besteht im und einige virtuelle Computer erstellen.

- [Erstellen Sie Linux VM auf Azure über das Portal](virtual-machines-linux-quick-create-portal.md)

- [Mithilfe einer Linux VM in Azure CLI](virtual-machines-linux-quick-create-cli.md)
