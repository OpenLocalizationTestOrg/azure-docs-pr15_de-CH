<properties
    pageTitle="Erstellen mehrerer virtueller Maschinen | Microsoft Azure"
    description="Optionen für das Erstellen mehrerer virtueller Computer unter Windows"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="guybo"/>

# <a name="create-multiple-azure-virtual-machines"></a>Mehrere Azure virtuelle Maschinen

Es gibt viele Szenarios sollten ähnliche virtuelle Maschinen (VMs) erstellen. Beispiele sind High Performance computing (HPC), umfangreiche Datenanalyse, skalierbar und häufig Staatenlosen mittlerer Ebene oder Back-End-Server (wie Webserver) und verteilten Datenbanken.

Dieser Artikel beschreibt die verfügbaren Optionen, um mehrere VMs in Azure erstellen. Diese Optionen gehen über den einfachen Fällen, wo Sie eine Reihe von VMs manuell erstellen. Erstellen vieler virtueller Computer skalieren nicht die Prozesse, die in der Regel auch benötigen Sie mehr als eine Handvoll VMs.

Können viele ähnliche VMs erstellen ist das Azure Ressourcenmanager Konstrukt der _Ressource Schleifen_.

## <a name="resource-loops"></a>Ressource-Schleifen

Ressource-Schleifen sind syntaktische Kurzform in Azure Ressourcenmanager Vorlagen. Ressource Loops können ähnlich konfigurierten Ressourcen in einer Schleife erstellen. Ressource Loops können Sie mehrere Speicherkonten, Netzwerkschnittstellen oder virtuelle Computer erstellen. Weitere Informationen zu Ressourcen Schleifen finden Sie in [Erstellen VMs Verfügbarkeit wird mit Ressource Schleifen](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>Probleme der Skalierung

Obwohl Ressource Schleifen leichter eine Cloud-Infrastruktur Ebene und präzise Vorlagen erstellen, bleiben bestimmte Probleme. Verwenden Sie eine Schleife Ressource 100 virtuelle Computer erstellen, müssen Sie beispielsweise Network Interface Controller (NICs) mit entsprechenden VMs und Speicherkonten zu korrelieren. Ist die Anzahl der VMs wahrscheinlich von der Anzahl der Storage-Konten müssen Sie anderen Ressource Schleife Größen zu behandeln. Lösbaren Probleme sind jedoch die Komplexität mit erhöht.

Tritt eine weitere Herausforderung benötigen Sie eine Infrastruktur, die elastisch skaliert. Beispielsweise empfiehlt eine Infrastruktur Skalierungsgröße, die automatisch erhöht oder verringert die Anzahl der VMs auf Arbeitslast. VMs geben einen integrierten Mechanismus (skalieren und Dezimalstellen) variieren. Wenn Sie Entfernen von VMs in skalieren, ist es schwierig, garantieren eine hohe Verfügbarkeit sicher, dass virtuelle Computer aktualisieren und Fehlertoleranz Domänen verteilt sind.

Schließlich bei Verwendung eine Ressource Schleife werden mehrere Aufrufe von Ressourcen erstellen zugrunde liegenden Fabric. Wenn mehrere Aufrufe ähnliche Ressourcen erstellen, kann Azure implizite dadurch verbessern und Bereitstellung höherer Zuverlässigkeit und Leistung zu optimieren. Dies sind _virtuelle Computer Maßstab legt fest_ .

## <a name="virtual-machine-scale-sets"></a>Virtual Machine Maßstab legt

Virtual Machine sind Skala eine Azure Compute-Ressource bereitstellen und Verwalten einer Gruppe von VMs identisch. Alle virtuellen Computer konfiguriert, VM Waage, einfach sind zu skalieren und Dezentrales Skalieren. Sie ändern lediglich die Anzahl der VMs in der Gruppe. Sie können auch VM Maßstab legt Skalierungsgröße Anforderungen der Arbeitslast.

Für Applikationen, die Serverressourcen und skalieren, werden Skala Operationen implizit Problem- und Update Domänen verteilt.

Statt mehrere Ressourcen NICs wie VMs korrelieren, hat eine VM Skalierung Netzwerk, Speicher, virtuellen und Erweiterungseigenschaften zentral konfigurieren können.

Einführung zu VM Skalierung finden Sie unter [virtuellen Maßstab legt Produktseite](https://azure.microsoft.com/services/virtual-machine-scale-sets/). Weitere Informationen finden Sie unter [virtuelle Computer Maßstab legt Dokumentation](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).
