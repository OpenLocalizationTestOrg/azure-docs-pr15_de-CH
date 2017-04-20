<properties
    pageTitle="Das Verschieben von Daten von Azure-Speicher | Microsoft Azure"
    description="Dieser Artikel bietet einen Überblick über die verschiedenen Methoden zum Verschieben von Daten aus dem Azure-Speicher."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="micurd"/>

# <a name="moving-data-to-and-from-azure-storage"></a>Das Verschieben von Daten aus dem Azure-Speicher

Ggf. für lokale Daten in Azure-Speicher (oder umgekehrt) sind eine Vielzahl von Methoden zur Verfügung. Ansatz, der für Sie am besten hängt von Ihrem Szenario. Dieser Artikel bietet einen Überblick Szenarien und die entsprechenden Angebote für jede.

## <a name="building-applications"></a>Building Applications

Wenn Sie eine Anwendung entwickeln auf die REST-API oder eine unserer vielen Clientbibliotheken eine hervorragende Möglichkeit zum Verschieben von Daten zu und von Azure-Speicher ist.

Azure-Speicher bietet umfangreiche Clientbibliotheken für .NET iOS, Java, Android, universelle Windows-Plattform (UWP), Xamarin, C++, Node.JS, PHP, Ruby und Python. Client-Bibliotheken bieten erweiterte Funktionen wie Wiederholungslogik, Protokollierung und parallele Uploads. Sie können auch direkt gegen die REST-API entwickeln, die von einer beliebigen Sprache aufgerufen werden kann, die HTTP/HTTPS-Anforderungen.

Finden Sie unter [Erste Schritte mit Azure BLOB-Speicher](storage-dotnet-how-to-use-blobs.md) mehr.

Darüber hinaus bieten wir [Azure Storage Data Bewegung Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) ist eine Bibliothek für hochleistungsfähige Kopieren von Daten von Azure. Siehe unsere Daten Bewegung Bibliothek [Dokumentation](https://github.com/Azure/azure-storage-net-data-movement) mehr. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Schnelle Interaktion Anzeige/mit Daten

Soll eine einfache Möglichkeit, Ihre Daten Azure aber auch die Möglichkeit zum Uploaden und Downloaden von Daten anzeigen, sollten Sie eine Azure-Speicher-Explorer verwenden.

Sehen Sie sich unsere Liste der [Azure-Speicher-Explorers](storage-explorers.md) Weitere.

## <a name="system-administration"></a>Systemadministration

Falls Sie benötigen oder mit einem Befehlszeilendienstprogramm (z.B. Systemadministratoren), sind hier einige Optionen zur Auswahl:

### <a name="azcopy"></a>AzCopy

AzCopy ist ein Windows-Befehlszeilenprogramm entwickelt für leistungsfähige Kopieren von Daten von Azure-Speicher. Sie können auch Daten in ein Speicherkonto oder zwischen verschiedenen Konten.

Finden Sie mehr [Daten mit AzCopy Befehlszeilenprogramm übertragen](storage-use-azcopy.md) .

### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell ist ein Modul, die Cmdlets zum Verwalten von Diensten auf Azure bereitstellt. Es ist eine aufgabenbasierte Befehlszeilenshell und Skriptsprache, die speziell für die Systemadministration.

Finden Sie unter [Verwenden von Azure PowerShell mit Azure-Speicher](storage-powershell-guide-full.md) mehr.

### <a name="azure-cli"></a>Azure CLI

Azure CLI bietet eine Reihe von open Source plattformübergreifende Befehle für die Arbeit mit Azure Services. Azure CLI steht Windows OSX und Linux.

Siehe weitere [Verwendung der Azure-CLI mit Azure-Speicher](storage-azure-cli.md) .

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Große Datenmengen mit einem langsamen Netzwerk verschieben

Eines der größten Probleme mit großen Datenmengen ist die Umlagerungszeit. Sie wollen Daten Azure-Speicher ohne Netzwerke Kosten müssen Code schreiben, ist Azure Importieren/Exportieren einer Lösung.

Siehe weitere [Azure importieren/exportieren](storage-import-export-service.md) .

## <a name="backing-up-your-data"></a>Sichern von Daten

Sie benötigen zur Sicherung Ihrer Daten in Azure-Speicher, ist Azure Backup der Weg zu gehen. Dies ist eine leistungsfähige Lösung zum Sichern von lokalen Daten und Azure VMs.

[Azure Backup](../backup/backup-introduction-to-azure-backup.md) mehr anzeigen

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Zugriff auf Ihre Daten – lokal und aus der Cloud

Benötigen Sie eine Lösung für den Zugriff auf Ihre Daten – lokal und aus der Cloud, dann sollten Sie mit Azure Hybriden Cloud Storage-Lösung StorSimple. Diese Lösung besteht aus einem physischen Gerät StorSimple, Intelligent speichert häufig Daten auf SSDs verwendet Daten auf Festplatten und inaktive/Backup/Archivierung auf Azure-Speicher verwendet.

Anzeigen Sie weitere [StorSimple](../storsimple/storsimple-overview.md)

## <a name="recovering-your-data"></a>Wiederherstellen Ihrer Daten

Wenn Sie lokale Arbeitslasten und Applikationen, benötigen Sie eine Lösung, die Ihr Unternehmen im Katastrophenfall fortgesetzt. Azure Site Recovery behandelt Replikation, Failover und Recovery von virtuellen Computern und Servern. Replizierte Daten ist in Azure-Speicher, so dass Sie eine sekundäre Datacenter vor Ort überflüssig.

[Azure Site Recovery](../site-recovery/site-recovery-overview.md) mehr anzeigen
