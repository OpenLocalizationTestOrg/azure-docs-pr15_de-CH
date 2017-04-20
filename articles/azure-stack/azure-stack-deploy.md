<properties
    pageTitle="Vor der Bereitstellung von Azure Stapel POC | Microsoft Azure"
    description="Umgebung und Hardware an Azure Stapel POC (Dienstadministratoren) anzeigen"
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="erikje"/>

# <a name="azure-stack-deployment-prerequisites"></a>Azure Stapel Bereitstellung erforderlicher Komponenten

Vor der Bereitstellung von Azure Stapel POC ([Proof of Concept](azure-stack-poc.md)) stellen Sie sicher, dass Ihr Computer die erfüllt.
Die Bereitstellung Technical Preview 2 Vorschriften für die Prüfung des Konzepts sind für Technical Preview 1 identisch. Daher können Sie die gleiche Hardware, die für die vorherige Single-Box verwendet.

## <a name="hardware"></a>Hardware

| Komponente | Mindestens  | Empfohlen |
|---|---|---|
| Laufwerke: Betriebssystem | 1 Betriebssystem-Datenträger mit mindestens 200 GB für die Systempartition (SSD oder Festplatte) | 1 Betriebssystem-Datenträger mit mindestens 200 GB für die Systempartition (SSD oder Festplatte) |
| Laufwerke: Allgemeine Azure Stapeldaten POC | 4 Festplatten. Jedes Laufwerk stellt mindestens 140 GB Kapazität (SSD oder Festplatte). Alle verfügbare Festplatten verwendet werden. | 4 Festplatten. Jedes Laufwerk stellt mindestens 250 GB Kapazität (SSD oder Festplatte). Alle verfügbare Festplatten verwendet werden.|
| Berechnen: CPU | Dual-Socket: 12 physischen Kernen (gesamt)  | Dual-Socket: 16 physischen Kernen (gesamt) |
| Berechnen: Speicher | 96 GB RAM  | 128 GB RAM |
| Berechnen: BIOS | Hyper-V aktiviert (mit LEISTE)  | Hyper-V aktiviert (mit LEISTE) |
| Netzwerk: NIC | Windows Server 2012 R2-Zertifizierung für NIC erforderlich; keine spezielle Features erforderlich | Windows Server 2012 R2-Zertifizierung für NIC erforderlich; keine spezielle Features erforderlich |
| HW-Logo-Zertifizierung | [Zertifiziert für Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Zertifiziert für Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

**Daten Festplattenkonfiguration:** Alle Datenlaufwerke muss denselben Typ (SAS oder SATA-) und Kapazität. SAS-Festplatten verwendet, die Laufwerke über einen einzelnen Pfad beizufügen (keine MPIO Multi Path-Unterstützung bereitgestellt).

**HBA-Konfigurationsoptionen**
 
- (Bevorzugt) Einfache HBA
- RAID HBA-Adapter muss im Modus "passieren" konfiguriert werden
- HBA RAID-Festplatten konfiguriert werden als einzelne, RAID 0

**Geben Sie Kombinationen unterstützten Bus und Medien**

-   SATA-FESTPLATTE

-   SAS-FESTPLATTE

-   RAID-FESTPLATTE

-   RAID-SSD (wenn der Medientyp nicht angegeben/unbekannt\*)

-   SATA SSD + SATA-FESTPLATTEN

-   SAS-SSD + -SAS-HDD

\*RAID-Controller ohne Pass-Through-Funktion können den Medientyp nicht erkennen. Dieser Controller markiert HDD und SSD als unbekannt. In diesem Fall wird die SSD als permanenten Speicher statt Zwischenspeichern Geräte verwendet werden. Daher können Sie Microsoft Azure Stapel POC auf die SSDs bereitstellen.

**Beispiel-HBAs**: LSI 9207 8i, LSI 9300 8i oder LSI-9265-8i in Pass-Through-Modus

Es stehen Beispielkonfigurationen OEM.

## <a name="operating-system"></a>Betriebssystem

| | **Vorschriften**  |
|---|---|
| **Version des Betriebssystems** | Windows Server 2012 R2 oder höher. Version des Betriebssystems nicht wichtig, vor die Bereitstellung als Host-Computer in der virtuellen Festplatte gestartet werden, die in Azure Stapel Installations-Zip enthalten. Das Betriebssystem und alle erforderlichen Patches sind bereits in das Abbild integriert. Verwenden Sie Schlüssel nicht aktivieren alle Windows Server-Instanzen in der POC verwendet.|

## <a name="deployment-requirements-check-tool"></a>Deployment überprüfen tool

Nach der Installation des Betriebssystems können Sie [Bereitstellung Checker für Azure Stapel Technical Preview 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) um zu bestätigen, dass die Hardware die Mindestanforderungen erfüllt.



## <a name="microsoft-azure-active-directory-accounts"></a>Microsoft Azure Active Directory-Konten

Microsoft Azure Stapel POC-Bereitstellung muss in Azure verbunden sein. Daher müssen Sie Microsoft Azure Active Directory-Konto vor die Bereitstellung PowerShell-Skript ausführen. Dieses Konto wird der globale Administrator für den Mandanten Azure Active Directory. Es wird zum Bereitstellen und Applikationen und Service-Prinzipale für alle Azure Stack-Dienste, die mit Active Directory Azure und Grafik-API delegieren. Es wird auch als Eigentümer der Anbieter Standardabonnement verwendet werden (die Sie später ändern können). Mit diesem Konto, können Sie sich Ihre Azure Stack System Admin Portal anmelden.

1. Erstellen Sie mnmthCreatinganAccount Azure AD Verzeichnisadministrator mindestens Azure Active Directory. Wenn Sie bereits haben, können Sie die. Andernfalls können kostenlos in [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (in China, besuchen Sie <http://go.microsoft.com/fwlink/?LinkID=717821> stattdessen.)

    Speichern Sie diese Anmeldeinformationen zur Verwendung in Schritt 6 [das Bereitstellungsskript PowerShell ausführen](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script). *Diese Dienstadministratorkonten* kann konfiguriert und verwaltet Ressource Wolken, Benutzerkonten Mieter Pläne, Quoten und Preise. Im Portal können sie Website Wolken, private Clouds virtuelle Computer erstellen, Pläne und Benutzerabonnements verwalten.

2. [Erstellen](azure-stack-add-new-user-aad.md) mindestens berücksichtigen, damit Sie Azure Stapel POC als Mieter anmelden können.

  	| **Azure Active Directory-Konto**  | **Unterstützt?** |
  	|---|---| 
  	| Arbeits- oder Schulcomputer Konto mit gültigen öffentlichen Azure-Abonnement  | Ja |
  	| Microsoft Account mit gültigen öffentlichen Azure-Abonnement  | Nein |
  	| Arbeits- oder Schulcomputer Konto gültige China Azure-Abonnement  | Ja |
  	| Arbeits- oder Schulcomputer Konto mit gültigen US-Regierung Azure-Abonnement  | Ja |


## <a name="network"></a>Netzwerk

### <a name="switch"></a>Schalter

Einen verfügbaren Port auf einen Schalter für die POC-Maschine.  

Azure Stapel POC-Computer unterstützt einen Switchport Zugriff oder Trunk-Port verbinden. Keine spezielle Features sind am Switch erforderlich. Wenn Sie einen Trunk-Port oder eine VLAN-ID konfigurieren möchten, Sie haben die VLAN-ID als Bereitstellungsparameter bereitstellen. Sie können in der [Liste der Bereitstellungsparameter](azure-stack-run-powershell-script.md)Beispiele.

### <a name="subnet"></a>Subnetz

Schließen Sie den POC-Computer die folgenden Subnetze zu:
- 192.168.200.0/24
- 192.168.100.0/27
- 192.168.101.0/26
- 192.168.102.0/24
- 192.168.103.0/25
- 192.168.104.0/25

Diese Subnets sind der internen Netzwerke in Microsoft Azure Stapel POC-Umgebung vorbehalten.

### <a name="ipv4ipv6"></a>IPv4-IPv6

Nur IPv4 unterstützt. IPv6-Netzwerke kann nicht erstellt werden.

### <a name="dhcp"></a>DHCP

Stellen Sie sicher, dass gibt ein DHCP-Server im Netzwerk, die mit die Netzwerkkarte verbunden. Wenn DHCP nicht verfügbar ist, müssen Sie ein weitere statisches IPv4-Netzwerk neben einem Host vorbereiten. Geben Sie die IP-Adresse und Gateway als Bereitstellungsparameter. Sie können in der [Liste der Bereitstellungsparameter](azure-stack-run-powershell-script.md)Beispiele.

### <a name="internet-access"></a>Internetzugriff

Azure Stapel benötigt Zugriff auf das Internet entweder direkt oder über einen transparenten Proxy. Azure Stapel unterstützt nicht die Konfiguration der Webproxy Internetzugang aktivieren. Host-IP und die neue IP-MAS-BGPNAT01 (über DHCP oder statische IP-Adresse) zugewiesen muss Internet zugreifen können. Die Ports 80 und 443 wird unter den Bereichen graph.windows.net und login.windows.net.

### <a name="telemetry"></a>Telemetrie

Zur Unterstützung der Telemetrie Datenfluss muss Port 443 (HTTPS) im Netzwerk geöffnet sein. Clientendpunkt ist https://vortex-win.data.microsoft.com.


## <a name="next-steps"></a>Nächste Schritte

[Azure Stapel POC-Bereitstellungspaket downloaden](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Bereitstellen von Azure Stapel POC](azure-stack-run-powershell-script.md)
