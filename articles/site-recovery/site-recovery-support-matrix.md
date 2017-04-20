<properties
    pageTitle="Azure Site Recovery-Unterstützungsmatrix | Microsoft Azure"
    description="Zeigt eine Zusammenfassung der unterstützten Betriebssysteme und Komponenten für Azure Site Recovery"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="azure-site-recovery-support-matrix"></a>Azure Site Recovery-Unterstützungsmatrix

Dieser Artikel beschreibt die unterstützten Betriebssysteme und Komponenten für die Bereitstellung von Azure Site Recovery. Eine Liste der unterstützten Komponenten und Komponenten steht für jedes Bereitstellungsszenario in jedem entsprechenden Bereitstellung Artikel und dieses Dokument fasst sie zusammen.

## <a name="supported-operating-systems-for-virtualization-servers"></a>Unterstützte Betriebssysteme für Server virtualization


**Quelle** | **Ziel** | **Host-Betriebssystem**
---|---|---|--- 
**Hyper-V-Hosts (ohne VMM)** | Azure | Windows Server 2012 R2 mit neuesten updates
**Hyper-V-Hosts (mit VMM)** | Azure | Windows Server 2012 R2 mit neuesten updates
**Hyper-V-Hosts (mit VMM)** | Sekundärstandort VMM | AT mindestens Windows Server 2012 mit neuesten Updates
**VMware-Hosts/vCenter** | Azure | vCenter 5.5 oder 6.0 (Unterstützung für 5.5 Funktionen) <br/><br/> vSphere 6.0, 5.5 oder 5.1 mit neuesten updates
**VMware-Hosts/vCenter** | Sekundärstandort VMware | vCenter 5.5 oder 6.0 (Unterstützung für 5.5 Funktionen) <br/><br/> vSphere 6.0, 5.5 oder 5.1 mit neuesten updates

## <a name="supported-requirements-for-replicated-machines"></a>Unterstützte an replizierten Maschinen

**Quelle** | **Was wird repliziert** | **Ziel** | **Host-Betriebssystem**
---|---|---|--- 
**Hyper-V-VMs** | Alle Arbeitslasten | Azure | Jeder Gast OS [von Azure unterstützt](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs müssen [Azure Vorschriften](site-recovery-best-practices.md#azure-virtual-machine-requirements) erfüllen.
**Hyper-V VMs (mit VMM)** | Alle Arbeitslasten | Azure | Jeder Gast OS [von Azure unterstützt](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs müssen [Azure Vorschriften](site-recovery-best-practices.md#azure-virtual-machine-requirements) erfüllen.
**Hyper-V VMs (mit VMM)** | Alle Arbeitslasten | Sekundärstandort VMM | Jeder Gast Betriebssystem [Hyper - v unterstützt](https://technet.microsoft.com/library/mt126277.aspx)
**Virtuelle VMware-Computer** | Alle Arbeitslasten auf Windows VM | Azure | 64-Bit-Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2 mit am wenigsten SP1<br/><br/> VMs müssen [Azure Vorschriften](site-recovery-best-practices.md#azure-virtual-machine-requirements) erfüllen.
**Virtuelle VMware-Computer** | Alle Arbeitslasten auf Linux VM | Azure | Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> CentOS 6.5, 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 unter Red Hat kompatibel Kernel oder unverwüstliche Enterprise Kernel-Version 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Erforderlicher Speicher: Dateisystem (EXT3 ETX4 ReiserFS, XFS); Multipath-Software Gerät Mapper (Multipfad)); Volume-Manager: (LVM2). Physische Server mit HP CCISS Controller werden nicht unterstützt. ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.<br/><br/> VMs müssen [Azure Vorschriften](site-recovery-best-practices.md#azure-virtual-machine-requirements) erfüllen.
**Virtuelle VMware-Computer** | Alle Arbeitslasten auf Windows VM | Sekundärstandort VMware | 64-Bit-Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2 mit am wenigsten SP1
**Virtuelle VMware-Computer** | Alle Arbeitslasten auf Linux VM | Sekundärstandort VMware | Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> CentOS 6.5, 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 unter Red Hat kompatibel Kernel oder unverwüstliche Enterprise Kernel-Version 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Erforderlicher Speicher: Dateisystem (EXT3 ETX4 ReiserFS, XFS); Multipath-Software Gerät Mapper (Multipfad)); Volume-Manager: (LVM2). Physische Server mit HP CCISS Controller werden nicht unterstützt. ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.
**Physische Server** | Alle Aufgaben unter Windows | Azure | 64-Bit-Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 mit am wenigsten SP1
**Physische Server** | Alle Arbeitslasten auf Linux | Azure | Red Hat Enterprise Linux-6.7,7.1,7.2 <br/><br/> CentOS 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 unter Red Hat kompatibel Kernel oder unverwüstliche Enterprise Kernel-Version 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Erforderlicher Speicher: Dateisystem (EXT3 ETX4 ReiserFS, XFS); Multipath-Software Gerät Mapper (Multipfad)); Volume-Manager: (LVM2). Physische Server mit HP CCISS Controller werden nicht unterstützt. ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.
**Physische Server** | Alle Aufgaben unter Windows | Sekundärstandort | 64-Bit-Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 mit am wenigsten SP1
**Physische Server** | Alle Arbeitslasten auf Linux | Sekundärstandort | Red Hat Enterprise Linux-6.7,7.1,7.2 <br/><br/> CentOS 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5 unter Red Hat kompatibel Kernel oder unverwüstliche Enterprise Kernel-Version 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Erforderlicher Speicher: Dateisystem (EXT3 ETX4 ReiserFS, XFS); Multipath-Software Gerät Mapper (Multipfad)); Volume-Manager: (LVM2). Physische Server mit HP CCISS Controller werden nicht unterstützt. ReiserFS-Dateisystem wird nur unter SUSE Linux Enterprise Server 11 SP3 unterstützt.


## <a name="provider-versions"></a>Anbieter-Versionen

**Name** | **Beschreibung** | **Neueste version** | **Unterstützung** | **Details**
---|---|---|---| ---
**Azure Site Recovery-Anbieter** | Kommunikation zwischen lokalen Servern und Azure-sekundäre Standort-Koordinaten <br/><br/> Wenn kein VMM Server installiert auf lokalen VMM-Server und Hyper-V Server | 5.1.1700 (Portal verfügbar) | [Aktuellen Features und fixes](https://support.microsoft.com/kb/3155002)
**Azure Site Recovery Unified Setup (VMware in Azure)** | Kommunikation zwischen lokalen VMware Server und Azure-Koordinaten <br/><br/> Auf lokalen VMware Server installiert | 9.3.4246.1 (Portal verfügbar) | [Aktuellen Features und fixes](https://support.microsoft.com/kb/3155002)
**Mobilitätsservice** | Koordiniert die Replikation zwischen lokalen VMware Server-physischen Server und Azure-sekundärer Standort | NA (Portal verfügbar) | Installiert auf jedem VMware VM oder physischen Server, den Sie replizieren möchten. **Microsoft Azure Recovery Services (MARS) agent** | Koordiniert die Replikation zwischen Hyper-V VMs und Azure<br/><br/> Auf lokalen Hyper-V Server (mit oder ohne VMM-Server) installiert | 2.0.8689.0 (Portal verfügbar) | Dieser Agent wird von Azure Site Recovery und Azure Backup verwendet). [Mehr] (https://support.microsoft.com/en-us/kb/2997692)

## <a name="next-steps"></a>Nächste Schritte

[Bereiten auf die Bereitstellung vor](site-recovery-best-practices.md)

