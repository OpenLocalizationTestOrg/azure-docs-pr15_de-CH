<properties
    pageTitle="Entwerfen Virtual Machine Maßstab für die Skalierung legt | Microsoft Azure"
    description="Informationen Sie über das Entwerfen der virtuellen Maßstab legt für die Skalierung"
    keywords="virtuellen Linux-Maschine legt virtuellen skalieren" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="gatneil"/>

# <a name="designing-vm-scale-sets-for-scale"></a>Entwerfen von VM-Skala wird für Skalierung

Dieses Thema behandelt Vorüberlegungen für Virtual Machine skalieren. Informationen wie Virtual Machine Skala sind finden Sie unter [Virtual Machine Maßstab legt Overview](virtual-machine-scale-sets-overview.md).


## <a name="storage"></a>Speicher

Eine Skalierung verwenden Speicherkonten Betriebssystemlaufwerke VMs in den speichern. Ein Verhältnis von 20 VMs Speicherkonto oder weniger empfohlen. Es wird empfohlen, dass Sie die ersten Zeichen des Speicher-Kontonamen Alphabet verteilt. Dabei können verschiedene interne Systeme Last verteilt. Beispielsweise folgende Vorlage wir verwenden UniqueString Ressourcenmanager Vorlagenfunktion zu Storage Namen vorangestellt werden Hashes Präfix: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat).


## <a name="overprovisioning"></a>Bereitstellung überproportional vieler

Beginnend mit "2016-03-30" API-Version ist VM Maßstab legt "Bereitstellung überproportional vieler" VMs. Mit Bereitstellung überproportional vieler eingeschaltet, die Skalierung tatsächlich Spins von mehr VMs als für die gestellten festgelegt, der dann zusätzliche VMs zuletzt erstellt. Bereitstellung überproportional vieler verbessert die Bereitstellung Erfolgsraten. Sie nicht diese zusätzlichen VMs berechnet und nicht auf die Kontingentgrenzen zählen.

Bereitstellung überproportional vieler Bereitstellung Erfolgsraten verbessert, kann es verwirrend Verhalten einer Anwendung, die nicht verschwinden unangekündigt VMs behandeln. Aktivieren Sie Bereitstellung überproportional vieler sicherzustellen, dass die folgende Zeichenfolge in der Vorlage: "viel": "false". Weitere Informationen finden in der [VM Skalierung festlegen REST API-Dokumentation](https://msdn.microsoft.com/library/azure/mt589035.aspx).

Wenn Sie die Bereitstellung überproportional vieler deaktivieren, erhalten Sie mit größeren VMs pro Konto jedoch nicht empfohlen, gehen über 40.


## <a name="limits"></a>Grenzen
Maßstab müssen basiert auf ein benutzerdefiniertes Bild (einer von Ihnen erstellten) alle OS Festplatte VHDs in ein Speicherkonto erstellen. Daher ist die maximale empfohlene Anzahl virtueller Computer in einer Skalierung auf ein benutzerdefiniertes Abbild erstellt 20. Wenn Sie die Bereitstellung überproportional vieler deaktivieren, können Sie bis zu 40 wechseln.

Ein Skalierungsgruppe basierend auf einem Plattform-Image ist derzeit auf 100 VMs (Wir empfehlen 5 Speicherkonten für diese Skala).

Für mehrere VMs können Begrenzungen müssen Sie mehrere Maßstab legt bereitstellen, wie in [dieser Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale)dargestellt.