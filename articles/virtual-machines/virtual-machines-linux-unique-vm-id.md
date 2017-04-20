<properties
   pageTitle="Zugriff auf VM-ID"
   description="Beschreibt den Zugriff auf und Azure VM eindeutige ID verwenden"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="accessing-and-using-azure-vm-unique-id"></a>Zugreifen auf und Verwenden von Azure VM eindeutige ID

Azure VM eindeutige ID ist 128-Bezeichner codiert und in alle Azure IaaS VM SMBIOS gespeichert und kann derzeit Plattform BIOS Befehle gelesen werden.

Azure VM eindeutige ID ist eine schreibgeschützte Eigenschaft. Azure VM Einmalige ändert sich nicht, beim Neustart Herunterfahren (entweder geplant für ungeplante), Start/Stop freigeben, service, Reparatur oder Wiederherstellen an. Wenn die VM eine Momentaufnahme ist und zum Erstellen einer neuen Instanz kopiert, jedoch neue Azure VM-ID konfiguriert.

> [AZURE.NOTE] Wenn Sie ältere VMs erstellt und da dieses neue Feature (18. September 2014) bitte Neustart eingeführt haben die VM zu automatisch ein Azure eindeutige ID


Zugriff von Azure VM Einmalige aus innerhalb der virtuellen Maschine


## <a name="create-a-vm"></a>Erstellen Sie einen virtuellen Computer
 

Weitere Informationen finden Sie unter [Erstellen eines virtuellen Computers](virtual-machines-linux-creation-choices.md)


## <a name="connect-to-the-vm"></a>Verbinden der VM
 

Weitere Informationen finden Sie unter [SSH unter Linux](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="query-vm-unique-id"></a>Abfrage VM eindeutige ID

Befehl (Beispiel **Ubuntu**):

    sudo dmidecode | grep UUID
    
Beispiel erwartete Ergebnisse:

    UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
    
Durch Big Endian bit Sortieren werden die tatsächlichen VM-ID in diesem Fall:

    DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
    
    
Azure VM eindeutige ID kann in verschiedenen Szenarien verwendet werden, ob die VM in Azure ausgeführt wird oder lokal und kann Lizenzierung, reporting oder allgemeine Überwachung Bedarf haben Sie Ihre Azure IaaS-Bereitstellung. Viele unabhängige Softwareanbieter erstellen und Azure Zertifizierung erfordern eine Azure VM während Ihres gesamten Lebenszyklus zu und feststellen, ob die VM in Azure ausgeführt wird lokal oder auf andere Cloudanbieter. Diese Plattformbezeichner können beispielsweise feststellen, ob die Software ordnungsgemäß lizenziert oder VM Daten an der Quelle wie korrelieren auf Festlegen der richtigen Metriken für die richtige Plattform zu unterstützen und zu korrelieren dieser Metriken unter anderem Hilfe.
