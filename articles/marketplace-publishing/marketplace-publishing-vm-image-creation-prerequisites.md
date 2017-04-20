<properties
   pageTitle="Technische Voraussetzung für die Erstellung von Abbild eines virtuellen Computers für den Azure | Microsoft Azure"
   description="Verstehen der Anforderungen zum Erstellen und Bereitstellen von Abbild eines virtuellen Computers Azure Marketplace für andere erwerben."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="04/29/2016"
  ms.author="hascipio; v-divte"/>

# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Technische Voraussetzung für den Azure Abbild eines virtuellen Computers erstellen
Lesen Sie den Prozess gründlich vor und verstehen Sie, wo und warum jeder Schritt ausgeführt wird. So weit wie möglich Sie sollten Ihre Unternehmensdaten und andere Daten vorbereiten, erforderlichen Tools herunterladen und technische Komponenten vor dem Angebot erstellen erstellen. Diese Elemente sollten aus diesem Artikel sein.  

## <a name="download-needed-tools--applications"></a>Werkzeuge & Anwendung herunterladen
Sie müssen Folgendes vor dem bereit:

- Je nachdem, welches Betriebssystem Sie verwenden möchten, installieren Sie [Azure PowerShell-Cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) oder [Linux Befehlszeilenschnittstelle Tool](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) von [Azure](https://azure.microsoft.com/downloads/) -Downloadseite.
- Installieren Sie Azure-Speicher-Explorer von CodePlex.
- Downloaden Sie und installieren Sie Zertifizierung Testtool für Azure zertifiziert:
  - [http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913). Sie benötigen einen Windows-Computer das Zertifizierungsstellen-Tool ausführen. Wenn Sie keinen Windows-basierten Computer können Sie das Werkzeug mit einer Windows-basierten VM in Azure ausführen.

## <a name="platforms-supported"></a>Unterstützte Plattformen
Azure-basierten VMs unter Windows oder Linux entwickeln. Einige Elemente der Veröffentlichungsvorgang – wie eine Azure-kompatible virtuelle Festplatte (VHD)--mit verschiedenen Tools und je nachdem, welches Betriebssystem Schritte erstellen:  

- Wenn Linux finden Sie im Abschnitt "Erstellen einer Azure-kompatiblen VHD (Linux-basierten)" [Anleitung zum virtuellen Abbild veröffentlichen](marketplace-publishing-vm-image-creation.md).
- Wenn Sie Windows verwenden, finden Sie im Abschnitt "Erstellen einer Azure-kompatiblen VHD (Windows-)" [Anleitung zum virtuellen Abbild veröffentlichen](marketplace-publishing-vm-image-creation.md).

> [AZURE.NOTE] Sie benötigen Zugriff auf eine Windows-basierte Computer:
- Validierung von Zertifizierungsstellen ausführen.
- Erstellen Sie VHD shared Access Signatur URL für Übermittlung Zertifizierungsstellen VHD.

## <a name="develop-your-vhd"></a>Entwickeln Sie Ihre VHD
Azure VHDs in der Cloud oder lokal entwickeln:

- Cloud-Entwicklung bedeutet, dass alle Entwicklungsschritte Remote auf einer virtuellen Festplatte auf Azure ausgeführt werden.
- Lokale Entwicklung erfordert eine VHD herunterladen und lokale Infrastruktur entwickeln. Obwohl dies möglich ist, empfohlen nicht es. Beachten Sie, dass für Windows oder SQL lokalen benötigen Sie die entsprechenden lokalen Lizenzschlüssel. Sie können nicht oder SQL Server nach dem Erstellen einer VM installieren. Sie müssen auch Ihr Angebot auf genehmigten SQL Azure-Portal eines Bildes basieren. Wenn Sie lokale entwickeln möchten, müssen Sie einige Schritte ausführen anders, als wenn Sie in der Cloud entwickelten. Informationen finden Sie unter [Erstellen einer lokalen VM-Image](marketplace-publishing-vm-image-creation-on-premise.md).

## <a name="next-steps"></a>Nächste Schritte
Überprüft die erforderlichen Komponenten und der erforderlichen Aufgaben können Sie mit dem Erstellen des virtuellen Computer Bild Angebots gemäß der [Anleitung zum virtuellen Abbild veröffentlichen](marketplace-publishing-vm-image-creation.md)vorwärts verschieben.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte: Angebot Azure Marketplace veröffentlichen](marketplace-publishing-getting-started.md)
- [Erstellen Sie einen virtuellen Computer mit Windows Azure Vorschau-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)


[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
