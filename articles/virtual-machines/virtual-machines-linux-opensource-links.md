<properties
    pageTitle="Linux- und Open Source-Computing in Azure | Microsoft Azure"
    description="Listet Linux- und Open Source-Computing Artikel Azure einschließlich grundlegende Linux Verwendung einige grundlegende Konzepte ausgeführt oder Hochladen von Bildern auf Azure und andere Inhalte Optimierungen zu speziellen Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="06/27/2016"
    ms.author="rasquill"/>



# <a name="linux-and-open-source-computing-on-azure"></a>Linux und Open-Source-computing in Azure

Finden Sie die Dokumentation zum Erstellen und Verwalten von Linux-basierten virtuellen Maschinen im klassischen Bereitstellungsmodell müssen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="get-started"></a>Erste Schritte
- [Einführung für Linux auf Azure](virtual-machines-linux-intro-on-azure.md)
- [Häufig gestellte Fragen zur Azure erstellte virtuelle Maschinen mit dem klassischen Bereitstellungsmodell](virtual-machines-linux-classic-faq.md)
- [Bilder für virtuelle Maschinen](virtual-machines-linux-classic-about-images.md)
- [Eigene Distribution hochladen](virtual-machines-linux-classic-create-upload-vhd.md) (und auch Anleitung eine [Azure-Endorsed Verteilung](virtual-machines-linux-endorsed-distros.md))
- [Melden Sie sich mit einem Linux VM klassische Azure-portal](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="set-up"></a>Einrichten

- [Installieren von Azure Befehlszeilenschnittstelle (CLI Azure)](../xplat-cli-install.md)


## <a name="tutorials"></a>Lernprogramme

- [Installieren Sie LAMP-Stapel auf einer virtuellen Linux-Maschine in Azure](virtual-machines-linux-create-lamp-stack.md)
- [Ruby auf Schienen-Anwendung auf eine Azure-VM](linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
- [Gewusst wie: Installieren Apache Qpid Proton-C AMQP und Servicebus](../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Datenbanken
- [Optimieren der Leistung von MySQL in Azure](virtual-machines-linux-classic-optimize-mysql.md)
- [MySQL-Clustern](virtual-machines-linux-classic-mysql-cluster.md)
- [Cassandra mit Linux auf Azure und Node.js zugreifen](virtual-machines-linux-classic-cassandra-nodejs.md)
- [Erstellen Sie einen Multi-Master Cluster MariaDbs](virtual-machines-linux-classic-mariadb-mysql-cluster.md)

### <a name="hpc"></a>HPC
- [Erste Schritte mit Linux Compute-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md)
- [NAMD Microsoft HPC Pack auf Linux Computeknoten in Azure ausgeführt](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
- [Einrichten eines Clusters Linux RDMA MPI-Anwendung ausführen](virtual-machines-linux-classic-rdma-cluster.md)

### <a name="docker"></a>Andockfenster
- [Mit der Erweiterung Andockfenster VM aus der Befehlszeile Azure Schnittstelle (Azure CLI)](virtual-machines-linux-classic-cli-use-docker.md)
- [Andockfenster VM-Erweiterung von Azure-Portal verwenden](virtual-machines-linux-classic-portal-use-docker.md)
- [Wie Sie Andockfenster Computer auf Azure](virtual-machines-linux-docker-machine.md)

### <a name="ubuntu"></a>Ubuntu
- [Gewusst wie: MySQL Cluster](virtual-machines-linux-classic-mysql-cluster.md)
- [Gewusst wie: Node.js und Cassandra](virtual-machines-linux-classic-cassandra-nodejs.md)

### <a name="opensuse"></a>OpenSUSE
- [Gewusst wie: Installieren und Ausführen von MySQL](virtual-machines-linux-classic-mysql-on-opensuse.md)

### <a name="coreos"></a>CoreOS
- [Gewusst wie: CoreOS in Azure verwenden](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="planning"></a>Planung
- [Richtlinien für die Implementierung von Azure-Infrastruktur-services](virtual-machines-linux-infrastructure-subscription-accounts-guidelines.md)
- [Linux-Benutzernamen auswählen](virtual-machines-linux-usernames.md)
- [Festlegen für virtuelle Maschinen im klassischen Bereitstellungsmodell Verfügbarkeit konfigurieren](virtual-machines-linux-classic-configure-availability.md)
- [Geplante Wartungsarbeiten am Azure VMs planen](virtual-machines-linux-planned-maintenance-schedule.md)
- [Die Verfügbarkeit der virtuellen Computer verwalten](virtual-machines-linux-manage-availability.md)
- [Geplante Wartung für virtuelle Linux-Computer in Azure](virtual-machines-linux-planned-maintenance.md)


## <a name="deployment"></a>Bereitstellung
- [Erstellen Sie einen benutzerdefinierten virtuellen Computer mit Linux](virtual-machines-linux-classic-createportal.md)
- [Die Grundlagen: Erfassen von Linux VM zu einer Vorlage](virtual-machines-linux-classic-capture-image.md)
- [Informationen für die Verteilung nicht unterstützt](virtual-machines-linux-create-upload-generic.md)


## <a name="management"></a>Management

- [SSH](virtual-machines-linux-mac-create-ssh-keys.md)
- [Zurücksetzen eines Kennworts oder SSH Eigenschaften für Linux](virtual-machines-linux-classic-reset-access.md)
- [Stamm verwenden](virtual-machines-linux-use-root-privileges.md)


## <a name="azure-resources"></a>Azure-Ressourcen

- [Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md)
- [Azure VM-Erweiterungen und Funktionen](virtual-machines-windows-extensions-features.md)
- [Benutzerdefinierte Daten in einer VM mit Cloud-Init-Injektion](virtual-machines-windows-classic-inject-custom-data.md)


## <a name="storage"></a>Speicher

- [Linux VM zuweisen Datenträger](virtual-machines-linux-classic-attach-disk.md)
- [Datenträger von Linux VM trennen](virtual-machines-linux-classic-detach-disk.md)
- [RAID](virtual-machines-linux-configure-raid.md)


## <a name="networking"></a>Netzwerk
- [Endpunkte auf einem klassischen virtuellen Computer in Azure einrichten](virtual-machines-linux-classic-setup-endpoints.md)


## <a name="troubleshooting"></a>Problembehandlung
- [Problembehandlung bei Secure Shell (SSH) Verbindung zu einer Linux-basierten Azure virtuellen Maschine](virtual-machines-linux-troubleshoot-ssh-connection.md)
- [Problembehandlung für klassische Bereitstellung erstellen Sie eine neue virtuellen Linux-Maschine in Azure](virtual-machines-linux-classic-troubleshoot-deployment-new-vm.md)  
- [Problembehandlung für Neustart oder das Ändern einer vorhandenen virtuellen Linux-Maschine in Azure klassische Bereitstellung](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md) 


## <a name="reference"></a>Referenz

- [Azure CLI-Befehle in Azure Service Management (Asm) Modus](../virtual-machines-command-line-tools.md)
- [Azure Servicemanagement REST-API](https://msdn.microsoft.com/library/azure/ee460799.aspx)




## <a name="general-links"></a>Allgemeine Links
Die folgenden Links sind für Microsoft Blogs, Technet-Seiten und Websites als Azure.com Dokumentation wie oben beschrieben. Azure und Open-Source-Computerwelt sind schnell Ziele ist es fast sicher sind die folgenden Links veraltet, *trotz* der Tatsache, dass wir versuchen werden ständig neue Themen und veraltete entfernen. Wenn wir eine verpasst haben, Bitte teilen Sie uns in den Kommentaren, oder Antrag auf unserer [GitHub Repo](https://github.com/Azure/azure-content/)ziehen.

- [ASP.NET 5 auf Linux Andockfenster Container ausgeführt](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
- [Zum Bereitstellen einer CentOS VM-Image von OpenLogic](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
- [SUSE-Infrastruktur](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
- [SUSE Linux Enterprise Server für SAP Cloud Appliance Library](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
- [Hochverfügbare Linux erstellen auf Azure in 12 Schritte](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
- [Automatisieren der Bereitstellung von Linux auf Azure Azure CLI, node.js (jhawk)](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
- [Ende auf Azure](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
- [FreeBSD in Azure ausgeführt](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
- [FreeBSD bereitzustellen](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
- [Bereitstellen eines benutzerdefinierten Abbilds FreeBSD](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
- [Kaspersky Antivirus für Linux-Dateiserver](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL

- [8 Open Source NoSql-Datenbanken für Azure](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
- [Slideshare (MSOpenTech): Erfahrungen mit CouchDb Azure](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
- [CouchDB-as-a-Service mit node.js, CORS und schwierige ausgeführt](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)

- [Redis unter Windows in Azure Redis Cache-Dienst](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
- [Ankündigung von ASP.NET Session State Anbieter für Vorabversion Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)

- [Blog: RavenHQ jetzt in Azure Marketplace verfügbar](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>Big Data
- [Hadoop auf VMs Azure Linux installieren](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
- [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Relationale Datenbank
- [MySQL hoher Verfügbarkeit Architektur in Microsoft Azure](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
- [Installieren von Postgres mit Corosync Pg_bouncer mit ILB](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>Linux high Performance computing (HPC)

- [Vorlage Quickstart: Einrichten eines Clusters SLURM](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (und [Blogbeitrag](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
- [Schnellstart-Vorlage: Erstellen Sie einen HPC-Cluster mit Linux Compute-Knoten](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>DevOps, Verwaltung und Optimierung

Wie Devops Management und Optimierung ist sehr umfangreich und sehr schnell ändern, sollten Sie die Liste als Ausgangspunkt.

- [Video: Azure Virtual Machines: Bäckermeister verwenden, Marionette und Andockfenster für Linux VMs verwalten](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)

- [Umfassender Leitfaden zur automatisierten Bereitstellung Kubernetes Gewebe mit CoreOS](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
- [Kubernetes-Schnellansicht](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)

- [Jenkins Slave Plug-in für Azure](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
- [GitHub Repo: Jenkins Storage Plug-in für Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)

- [Dritten: Hudson Slave Plug-in für Azure](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
- [Drittanbietern: Speicher Hudson Plug-in für Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)

- [Video: Was ist Chef, und wie funktioniert es?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)

- [Video: Verwendung von Azure Automatisierung mit Linux VMs](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)

- [Blog: Wie Powershell DSC für Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
- [GitHub: Andockfenster Client DSC](https://github.com/anweiss/DockerClientDSC)

- [Packer-Plugin für Azure](https://github.com/msopentech/packer-azure)
