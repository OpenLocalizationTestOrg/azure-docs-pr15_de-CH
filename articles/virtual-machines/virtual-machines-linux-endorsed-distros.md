<properties
    pageTitle="Linux-Distributionen unterstützt | Microsoft Azure"
    description="Lernen Sie Linux auf Azure unterstützt Verteilung sowie für Ubuntu, OpenLogic, Oracle und SUSE."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager"
    />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="szark"/>



#<a name="linux-on-azure-endorsed-distributions"></a>Linux auf Azure unterstützt Distributionen

> [AZURE.NOTE] Haben Sie einen Moment, Hilfe zu Azure Linux VM-Dokumentation mit dieser [Kurzübersicht](https://aka.ms/linuxdocsurvey) Erfahrungen. Jede Antwort kann wir Ihnen helfen, Ihre Arbeit.

Linux Bilder in Azure Gallery oder Marketplace Partnern bereitgestellt, und wir arbeiten mit verschiedenen Linux unterstützt Verteilerliste auch weitere Varianten hinzu. In der Zwischenzeit können Sie für Distributionen nicht aus dem Katalog immer in-Your-eigenen-Linux anhand der Richtlinien auf [dieser Seite](virtual-machines-linux-classic-create-upload-vhd.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="supported-distributions--versions"></a>Unterstützten Distributionen und -Versionen ##

Die folgende Tabelle listet die Linux-Distributionen und -Versionen auf Azure unterstützt. Siehe auch weitere [Unterstützung für Linux-Bilder in Microsoft Azure](https://support.microsoft.com/en-us/kb/2941892) .

Linux Integration Services (LIS) Treiber für Hyper-V und Azure sind Kernel-Module, die Microsoft an den upstream Kernel beiträgt.  LIS-Treiber werden standardmäßig entweder in der Verteilung Kernel erstellt oder für ältere RHEL/CentOS-basierten Distributionen verfügbar als separater Download [hier](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409).  Finden Sie [in diesem Artikel](virtual-machines-linux-create-upload-generic.md#linux-kernel-requirements) Weitere Informationen über die LIS-Treiber.

Azure Linux-Agent bereits vorinstalliert auf Azure Galeriebilder und stehen in der Regel die Verteilung Paket-Repository.  Quellcode finden Sie auf [GitHub](https://github.com/azure/walinuxagent).

Verteilung|Version|Treiber|Agent
---|---|---|---
CentOS von OpenLogic | CentOS 6.3 + 7.0 + | CentOS 6.3: [LIS herunterladen](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: Im Kernel | Paket: In [OpenLogic Repo](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) unter "WALinuxAgent" <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent)
[CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) | 494.4.0+ | Im Kernel | Quellcode: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent)
Debian | Debian 7.9 +, 8.2 + | Im Kernel | Paket: In Repo unter "Waagent" <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent)
Oracle Linux | 6.4 +, 7.0 + | Im Kernel | Paket: In Repo unter "WALinuxAgent" <br/>Quellcode: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
Red Hat Enterprise Linux | RHEL 6.7 + 7.1 + | Im Kernel|Paket: In Repo unter "WALinuxAgent" <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent)
SUSE Linux Enterprise | SLES 11 SP4, SLES 12 SP1 + und <p> SLES für SAP 11 SP3 + | Im Kernel | Paket: Im [Cloud-Tools](https://build.opensuse.org/project/show/Cloud:Tools) Repo unter "Python-Azure-Agent" <br/>Quellcode: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
openSUSE | OpenSUSE 13.2 + | Im Kernel | Paket: Im [Cloud-Tools](https://build.opensuse.org/project/show/Cloud:Tools) Repo unter "Python-Azure-Agent" <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent)
Ubuntu|Ubuntu 12.04, 14.04, 16.04, 16.10 | Im Kernel | Paket: In Repo unter "Walinuxagent" <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent)


## <a name="partners"></a>Partner

### <a name="openlogic"></a>OpenLogic
[http://www.openlogic.com/Azure](http://www.openlogic.com/azure)

OpenLogic ist ein führender Anbieter von Enterprise open Source-Lösungen für die Cloud und das Rechenzentrum. OpenLogic kann Hunderte von führenden Unternehmen über eine Vielzahl von Branchen sicher erwerben, unterstützen und open Source-Software steuern. OpenLogic bietet handelsüblichen technischen Support sowie Schadenersatz für 600 open-Source-Pakete OpenLogic Experten-Community, einschließlich Unterstützung für CentOS Enterprise sowie die Vertriebspartner für CentOS-basierte Abbilder auf Azure unterstützt.

### <a name="coreos"></a>CoreOS
[https://coreos.com/docs/Running-coreos/Cloud-Providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

Von der CoreOS-Website:

*CoreOS dient zur Sicherheit, Konsistenz und Zuverlässigkeit. Statt Pakete über Yum oder apt, verwendet CoreOS Linux Container zum Verwalten Ihrer Dienste auf einer höheren Ebene der Abstraktion. Code für einen einzelnen Dienst und alle abhängigen werden in einem Container gepackt, die auf einem oder mehreren CoreOS ausgeführt werden können.*


### <a name="credativ"></a>Credativ
[http://www.credativ.co.uk/credativ-Blog/Debian-Images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ ist eine unabhängige Beratung und Services spezialisiert, Entwicklung und Implementierung von professionelle Lösungen mit freier Software. Als Open Source Spezialisten hat Credative internationalen Anerkennung mit vielen IT-Abteilung ihre Unterstützung. In Verbindung mit Microsoft bereitet Credativ entsprechende Debian Images für Debian 8 (Jessie) und Debian vor 7 (Wheezy) speziell auf Azure ausgeführt und kann einfach über die Plattform verwaltet werden. Credativ unterstützen auch die langfristige Wartung und Aktualisierung von Debian Bilder für Azure durch den Open Source Support-Zentren.

### <a name="oracle"></a>Oracle
[http://www.Oracle.com/technetwork/Topics/Cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle Strategie ist bieten ein umfassendes Portfolio von Lösungen für öffentliche und private Clouds und Kunden Auswahlmöglichkeiten und Flexibilität wie sie Oracle-Software in Oracle Wolken sowie andere Wolken bereitstellen.  Oracle Partnerschaft mit Microsoft kann Kunden Oracle-Software in Microsoft public und private Clouds vertrauensvoll Zertifizierungsstelle bereitstellen und von Oracle unterstützt.  Oracle und Investitionen in Oracle öffentliche und private Cloudlösungen bleibt unverändert.

### <a name="red-hat"></a>Red Hat
[http://www.redhat.com/en/Partners/Strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Weltweit führender Anbieter von open Source-Lösungen unterstützt Red Hat mehr als 90 % der Fortune 500-Unternehmen geschäftliche Probleme zu lösen, richten Sie ihre IT und Business-Strategien und bereit für die Zukunft der Technologie. Red Hat dabei sichere Lösungen öffnen Geschäftsmodell und einer erschwinglichen, vorhersagbare Abonnementmodell.

### <a name="suse"></a>SUSE
[http://www.SuSE.com/SuSE-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server auf Azure ist eine bewährte Plattform, die hohe Zuverlässigkeit und Sicherheit für Cloud computing. SUSE Linux Plattform integriert sich nahtlos in Azure Cloud Services für leicht verwaltbaren Cloud-Umgebung. Und mit mehr als 9200 zertifizierten über 1.800 unabhängige Softwarehersteller für SUSE Linux Enterprise Server, SUSE Arbeitslasten mit unterstützten im Rechenzentrum sicher auf Azure bereitgestellt werden können.

### <a name="canonical"></a>Kanonische
[http://www.Ubuntu.com/Cloud/Azure](http://www.ubuntu.com/cloud/azure)

Kanonische Engineering und offene Gemeinschaft Governance Laufwerk Ubuntu Erfolg Client, Server und Cloud computing, einschließlich personal Cloud-Services für Kunden. Die kanonischen Vision eine einheitliche kostenlose Plattform in Ubuntu von Telefon in die cloud mit einer kohärenten Schnittstellen für Telefon, Tablet, TV und Desktop macht Ubuntu zur erste Wahl für verschiedene Institutionen öffentliche Cloud-Anbieter zu den Herstellern von Unterhaltungselektronik bei einzelnen Techniker.

Entwickler und engineering-Zentren auf der ganzen Welt Canonical ist eindeutig positioniert partner von Hardwarehersteller Inhaltsanbieter oder Softwareentwickler Ubuntu Lösungen auf den Markt, PCs, Servern und handheld-Geräte.

