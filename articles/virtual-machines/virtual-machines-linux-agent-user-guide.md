<properties 
    pageTitle="Benutzerhandbuch für Linux-Agent | Microsoft Azure" 
    description="Informationen Sie zum Installieren und Konfigurieren von Linux-Agent (Waagent) zum Verwalten der virtuellen Maschine Interaktion mit Azure Fabric Controller." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



# <a name="azure-linux-agent-user-guide"></a>Azure Linux-Agent-Benutzerhandbuch

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>Einführung

Microsoft Azure Linux-Agent (Waagent) verwaltet Linux und FreeBSD provisioning und VM Interaktion mit Azure Fabric Controller. Es bietet die folgenden Funktionen für Linux und FreeBSD IaaS Bereitstellung:

> [AZURE.NOTE] Sehen Sie Azure Linux-Agent [Readme-Datei](https://github.com/Azure/WALinuxAgent/blob/master/README.md) für Weitere Informationen.

* **Image-Bereitstellung**
  - Erstellung eines Benutzerkontos
  - Konfigurieren von SSH-Authentifizierungstypen
  - Bereitstellung von öffentlichen SSH-Schlüssel und Schlüsselpaare
  - Festlegen der Host-name
  - Der Hostname Veröffentlichen der DNS-Plattform
  - Reporting SSH Host Key Fingerabdruck der Plattform
  - Datenträger-Ressourcenmanagement
  - Formatierung und Montage der Festplatte
  - Konfigurieren von Auslagerungsspeicher

* **Netzwerk**
  - Routen zur Verbesserung der Kompatibilität mit DHCP-Servern Plattformen managt
  - Die Stabilität des Netzwerks Schnittstelle

* **Kernel**
  - Konfiguriert die virtuellen NUMA (Kernel deaktivieren < 2.6.37)
  - Hyper-V Entropie für/dev/Random verwendet
  - Konfiguriert die SCSI-Zeitlimits für das Root-Gerät (der remote handeln)

* **Diagnose**
  - Umleitung der Konsole an den seriellen port

* **Bereitstellung von SCVMM**
    - Erkennt und VMM-Agenten für Linux startet, wenn in einer Umgebung mit System Center Virtual Machine Manager 2012 R2 ausgeführt

* **VM-Erweiterung**
    - Fügen Sie von Microsoft und Partnern in Linux VM (IaaS) Software aktivieren und Konfiguration Automatisierung erstellte Komponente ein
    - Implementierung des VM-Erweiterung Verweis auf [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)


## <a name="communication"></a>Kommunikation
Der Informationsfluss von der Plattform zum Agent erfolgt über zwei Kanäle:

* Ein Start-DVD für IaaS Bereitstellung angefügt. Diese DVD enthält eine OVF-kompatiblen Konfigurationsdatei, die als tatsächliche SSH-Schlüsselpaare aller Bereitstellungsinformationen enthält.
* Ein TCP-Endpunkt verfügbar machen eine REST-API verwendet, Bereitstellung und Konfiguration der Topologie.


## <a name="requirements"></a>Vorschriften
Die folgenden Systeme wurden getestet und funktionieren mit dem Azure Linux-Agent:

> [AZURE.NOTE] Diese Liste unterscheiden die offizielle Liste mit unterstützten Systemen auf Microsoft Azure-Plattform wie hier beschrieben: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)

* CoreOS
* CentOS 6.3 +
* Red Hat Enterprise Linux 6.7 +
* Debian 7.0 +
* Ubuntu 12.04 +
* OpenSUSE 12.3 +
* SLES 11 SP3 +
* Oracle Linux 6.4 +

Andere unterstützte Systeme:

* FreeBSD 10 + (Azure Linux-Agent v2.0.10)

Linux-Agent hängt einige Systempakete ordnungsgemäß funktionieren:

* Python 2.6 +
* OpenSSL 1.0 +
* OpenSSH 5.3 +
* Dateisystem-Dienstprogramme: Sfdisk Fdisk Mkfs, getrennt
* Passwort-Tools: Chpasswd Sudo
* Textverarbeitungstools: Sed Grep
* Netzwerk-Tools: IP-Route
* Kernel Unterstützung für UDF-Dateisysteme einhängen.

## <a name="installation"></a>Installation
Installation mit RPM oder DEB-Paket von der Verteilung Paket-Repository ist die bevorzugte Methode der Installation und Aktualisierung von Azure Linux-Agent. Alle die [Verteilung Anbieter unterstützt](virtual-machines-linux-endorsed-distros.md) integrieren Azure Linux-Agent-Paket in ihre Bilder und Repositories.

Lesen Sie die Dokumentation in [Azure Linux-Agent Repo auf Github](https://github.com/Azure/WALinuxAgent) für erweiterte Installationsoptionen aus Quelle oder benutzerdefinierte Speicherorte oder Präfixe installiert.


## <a name="command-line-options"></a>Befehlszeilenoptionen

### <a name="flags"></a>Flags

- verbose: Ausführlichkeit der Befehl
- erzwingen: interaktive Bestätigung für einige Befehle überspringen

### <a name="commands"></a>Befehle

- Hilfe: Listet die unterstützten Befehle und Flags.

- Identitätsintegrationsprodukts: Versuch, die Anlage und für eine erneute Bereitstellung. Dieser Vorgang gelöscht Folgendes:
 * Alle SSH Hostschlüssel (bei Provisioning.RegenerateSshHostKeyPair 'y' in der Konfigurationsdatei)
 * Nameserver Konfiguration /etc/resolv.conf
 * Root-Kennwort von Shadow (bei Provisioning.DeleteRootPassword 'y' in der Konfigurationsdatei)
 * Zwischengespeicherten DHCP-Clientleases
 * Setzt Hostnamen localhost.localdomain ein


> [AZURE.WARNING] Entfernung garantiert nicht, dass das Bild aller vertraulichen Daten gelöscht, und für die Verteilung.


- Identitätsintegrationsprodukts + User: Alles - entziehen (oben) führt löscht das letzte bereitgestellte Benutzerkonto (erhältlich von /var/lib/waagent) und zugeordnete Daten. Dieser Parameter wird ein Bild entziehen, die zuvor auf Azure bereitgestellt wurde erfasst und verwendet werden kann.

- Version: Zeigt die Version des Waagent

- Serialconsole: konfiguriert die GRUB ttyS0 markieren (den ersten seriellen Anschluss) als Boot-Konsole. Dadurch Kernel Booten Protokolle werden an den seriellen Anschluss gesendet und für das Debuggen.

- Daemon: Waagent als Daemon zum Verwalten der Interaktion mit der Plattform ausführen. Dieses Argument wird für Waagent im Waagent Init-Skript angegeben.

- Starten: Waagent im Hintergrund ausführen


## <a name="configuration"></a>Konfiguration

Eine Konfigurationsdatei (/ etc/waagent.conf) steuert die Aktionen des Waagent. Nachfolgend sehen Sie eine Beispiel-Konfigurationsdatei:

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

Die Optionen werden im folgenden ausführlich beschrieben. Konfigurationsoptionen sind drei Arten. Boolescher Wert, Zeichenfolge oder Ganzzahl. Die booleschen Konfigurationsoptionen können als "y" oder "n" angegeben werden. Das spezielle Schlüsselwort "None" für einige Zeichenfolge Typ Konfigurationseinträge wie unten verwendet werden.

**Provisioning.Enabled:**  
Typ: Boolean  
Standard: y

Dies ermöglicht dem Benutzer aktivieren oder Deaktivieren der Bereitstellung Funktionen im Agent. Gültige Werte sind "y" oder "n". Wenn die Bereitstellung deaktiviert ist, SSH Host- und Schlüssel im Bild bleiben und Konfiguration in Azure API Bereitstellung ignoriert.

> [AZURE.NOTE] Die `Provisioning.Enabled` Parameter standardmäßig auf "n" Ubuntu Cloud Bilder, die für die Bereitstellung von Cloud-Init verwenden.

  
**Provisioning.DeleteRootPassword:**  
Typ: Boolean  
Standard: n

Wenn Set Root-Kennwort in der Datei/Etc/Shadow Bereitstellungsprozess gelöscht.


**Provisioning.RegenerateSshHostKeyPair:**  
Typ: Boolean  
Standard: y

Stellt alle SSH Hostschlüsselpaare (Ecdsa, Dsa und Rsa) während des Bereitstellungsprozesses von/etc/ssh/gelöscht werden. Und ein einzelnes frischen Schlüsselpaar generiert.

Verschlüsselungstyp für das neue Schlüsselpaar lässt sich mit dem Provisioning.SshHostKeyPairType. Beachten Sie, dass einige Distributionen neu erstellt Schlüsselpaare für fehlende Verschlüsselungstypen SSH SSH-Daemon (z. B. bei einem Neustart) neu gestartet wird.


**Provisioning.SshHostKeyPairType:**  
Typ: Zeichenfolge  
Standard: Rsa

Dies kann auf einen Verschlüsselungstyp Algorithmus festgelegt werden, die von den SSH-Daemon auf dem virtuellen Computer unterstützt. In der Regel unterstützte Werte sind "Rsa", "Dsa" und "Ecdsa". Beachten Sie, dass "putty.exe" Windows "Ecdsa" nicht unterstützt. So soll putty.exe Verbindung mit Linux-Bereitstellung unter Windows verwenden, verwenden Sie "Rsa" oder "Dsa".


**Provisioning.MonitorHostName:**  
Typ: Boolean  
Standard: y

Wenn festgelegt, Waagent virtuellen Linux-Maschine Hostname suchen überwacht (wie durch den Befehl "Hostname" zurückgegeben) und die Netzwerkkonfiguration in das Bild entsprechend die Änderung aktualisiert. Um die Änderung der DNS-Server push wird Netzwerke auf dem virtuellen Computer gestartet. Dies führt in Kürze Verlust der Internetkonnektivität.


**Provisioning.DecodeCustomData**  
Typ: Boolean  
Standard: n

Wenn festgelegt, Waagent Eintrag von Base64 decodiert wird.


**Provisioning.ExecuteCustomData**  
Typ: Boolean  
Standard: n

Wenn festgelegt, Waagent Eintrag nach der Bereitstellung ausgeführt werden.


**Provisioning.PasswordCryptId**  
Typ: Zeichenfolge  
Standard: 6

Algorithmus von Crypt beim Kennworthash generieren.  
 1 - MD5  
 2a - Blowfish  
 5 - SHA-256  
 6 - SHA-512  


**Provisioning.PasswordCryptSaltLength**  
Typ: Zeichenfolge  
Standard: 10

Länge der beliebigen Salt verwendet beim Kennworthash generieren.


**ResourceDisk.Format:**  
Typ: Boolean  
Standard: y

Wenn festgelegt, der von der Plattform bereitgestellten Datenträger formatiert und Waagent Wenn Dateisystemtyp angefordert vom Benutzer im "ResourceDisk.Filesystem" als "Ntfs" wird bereitgestellt. Eine Partition des Typs Linux (83) wird auf der Festplatte zur Verfügung gestellt. Beachten Sie, dass diese Partition nicht formatiert wird, wenn sie erfolgreich bereitgestellt werden kann.


**ResourceDisk.Filesystem:**  
Typ: Zeichenfolge  
Standard: ext4

Gibt den Dateisystemtyp für den Datenträger. Unterstützte Werte variieren je nach Linux-Distribution. Wenn die Zeichenfolge X, dann Mkfs. X sollte im Linux-Bild. SLES 11 Bilder sollten in der Regel "ext3" verwenden. 'Ufs2' sollte hier FreeBSD Bilder verwendet werden.


**ResourceDisk.MountPoint:**  
Typ: Zeichenfolge  
Standard: / Mnt/Ressourcen 

Gibt den Pfad auf dem Festplatte der bereitgestellt wird. Beachten Sie, dass die Festplatte ein *temporärer* Speicherplatz ist und gelöscht werden, kann Wenn die VM hat.


**ResourceDisk.MountOptions**  
Typ: Zeichenfolge  
Standard: keine

Gibt Einhängeoptionen Datenträger Mount -o-Befehl übergeben werden. Dies ist eine durch Kommas getrennte Liste von Werten, ex. 'Nodev, Nosuid'. Mount(8) Details anzeigen


**ResourceDisk.EnableSwap:**  
Typ: Boolean  
Standard: n

Wenn festgelegt, eine Auslagerungsdatei (/ verwendet) auf der Festplatte erstellt und der Swap-Speichers hinzugefügt.


**ResourceDisk.SwapSizeMB:**  
Typ: Ganzzahl  
Standardwert: 0

Die Größe der Auslagerungsdatei in MB.


**Logs.Verbose:**  
Typ: Boolean  
Standard: n

Wenn Set Ausführlichkeit des Protokolls erhöht wird. Waagent /var/log/waagent.log meldet und nutzt die Systemfunktionen Logrotate Protokolle rotieren.


**OS. EnableRDMA**  
Typ: Boolean  
Standard: n

Wenn festgelegt, der Agent versucht, installieren und Laden Sie einen RDMA Kernel-Treiber, Version der Firmware auf der zugrunde liegenden Hardware entspricht.


**OS. RootDeviceScsiTimeout:**  
Typ: Ganzzahl  
Standard: 300

Dadurch werden SCSI-Timeout in Sekunden auf die Datenträger- und OS konfiguriert. Wenn nicht, das System Standardwerte verwendet werden.


**OS. OpensslPath:**  
Typ: Zeichenfolge  
Standard: keine

Dies kann an einen alternativen Pfad für binäre für kryptografische Vorgänge Openssl verwendet werden.


**HttpProxy.Host HttpProxy.Port**  
Typ: Zeichenfolge  
Standard: keine

Wenn festgelegt, der Agent diesen Proxy-Server verwendet, um auf das Internet zugreifen. 


## <a name="ubuntu-cloud-images"></a>Ubuntu Cloud Bilder

Beachten Sie, dass Ubuntu Cloud Bilder nutzen [Cloud Init](https://launchpad.net/ubuntu/+source/cloud-init) viele Aufgaben ausführen, die andernfalls von Azure Linux-Agent verwaltet werden.  Beachten Sie die folgenden Unterschiede:


- **Provisioning.Enabled** wird standardmäßig auf "n" Ubuntu Cloud Bilder, die Cloud-Init Bereitstellung Aufgaben verwenden.

- Die folgenden Konfigurationsparameter haben keine Auswirkung auf Ubuntu Cloud Bilder, die mit Cloud-Initialisierung die Festplatte verwalten und swap-Platz:

 - **ResourceDisk.Format**
 - **ResourceDisk.Filesystem**
 - **ResourceDisk.MountPoint**
 - **ResourceDisk.EnableSwap**
 - **ResourceDisk.SwapSizeMB**

- Finden Sie unter konfigurieren den Bereitstellungspunkt Datenträger und auf Ubuntu Cloud Bilder während der Bereitstellung die folgenden Ressourcen:

 - [Ubuntu-Wiki: Swap-Partitionen konfigurieren](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
 - [Benutzerdefinierte Daten einfügen in Azure Virtual Machine](virtual-machines-windows-classic-inject-custom-data.md)

