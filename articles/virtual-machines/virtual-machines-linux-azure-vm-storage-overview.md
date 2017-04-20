<properties
  pageTitle="Azure und Linux VM-Speicher | Microsoft Azure"
  description="Beschreibt virtuelle Linux-Computer Azure Standard- und Premium-Speicher."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines-linux"
  authors="vlivech"
  manager="timlt"
  editor=""/>

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="NA"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure"
  ms.date="10/04/2016"
  ms.author="v-livech"/>

# <a name="azure-and-linux-vm-storage"></a>Azure und Linux VM-Speicher

Azure-Speicher ist der Cloud-Storage-Lösung für moderne, die auf Dauerhaftigkeit, Verfügbarkeit und Skalierbarkeit für die Bedürfnisse ihrer Kunden.  Zusätzlich ermöglichen Entwicklern umfangreiche Anwendung um neue Szenarios zu unterstützen, bietet Azure Storage Storage Foundation auch Azure Virtual Machines.

## <a name="azure-storage-standard-and-premium"></a>Azure-Speicher: Standard- und Premium

Azure VM kann standard-Datenträger oder Premium-Datenträger erstellt werden.  Wenn Sie das Portal VM auswählen muss eine Dropdown-Liste auf dem Bildschirm Grundlagen Standard- und Premium-Datenträger anzeigen umschalten  Der folgende Screenshot zeigt, Menü.  Wenn umgeschaltet SSD nur Premium Speicher aktiviert VMs angezeigt werden, alle von SSD Laufwerke.  Umschalten auf Festplatte aktiviert Standardspeicher VMs unterstützt rotierende Festplatten mit Premium-Speicher angezeigt wird, durch SSD VMs unterstützt.

  ![bildschirm1](../virtual-machines/media/virtual-machines-linux-azure-vm-storage-overview/screen1.png)

Beim Erstellen einer VM aus der `azure-cli` können zwischen Standard- und Premium bei virtueller Speicher über die `-z` oder `--vm-size` Cli-Flag.

### <a name="create-a-vm-with-standard-storage-vm-on-the-cli"></a>Erstellen Sie einen virtuellen Computer mit Standardspeicher VM auf die cli

Cli-Flag `-z` Standard_A1 wählt a1 ein Standardspeicher basiert Linux VM.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_A1
```

### <a name="create-a-vm-with-premium-storage-on-the-cli"></a>Erstellen Sie einen virtuellen Computer mit Premium-Speicher auf die cli

Cli-Flag `-z` Standard_DS1 wählt mit DS1 Premium-Speicher basiert Linux VM.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_DS1
```

## <a name="standard-storage"></a>Standardspeicher

Azure Standard-Speicher ist der Speicher.  Standardspeicher ist gleichzeitig leistungsfähige kostengünstig.  

## <a name="premium-storage"></a>Premium-Speicher

Azure Storage Premium bietet Unterstützung für virtuelle Maschinen, I/O-Intensive Arbeitslasten hohe Leistung, niedriger Latenz Datenträger. Virtual Machine (VM) Datenträger mit Premium-Speicher speichern Daten auf solid-State-Laufwerken (SSDs). Sie können die Anwendung VM Festplatten Azure Premium Speicher für die Geschwindigkeit und Leistung dieser Festplatten migrieren.

Speicher-Zusatzfunktionen:

- Premium-Datenträger: Azure Premium-Speicher unterstützt VM Festplatten, die DS, DSv2 oder GS-Serie Azure VMs angeschlossen werden können.

- Premium Seite Blob: Premium-Speicher unterstützt Azure Seitenblobs permanente Datenträger für Azure virtuelle Maschinen (VMs) gespeichert.

- Premium lokal redundanter Speicher: Premium Speicherkonto nur lokal redundanter Speicher (LRS) als Replikationsoption unterstützt und hält drei Kopien der Daten in einem einzigen Bereich.

- [Premium-Speicher](../storage/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Premium-Speicher unterstützt VMs

Premium-Speicher unterstützt DS-Serie, DSv2-, GS-Serie und Fs-Serie Azure virtuelle Maschinen (VMs). Sie können Standard- und Premium-Datenträger mit Premium VMs unterstützt. Jedoch können keine Premium-Datenträgern mit VM-Serie nicht Premium kompatibel sind.

Im folgenden werden mit Premium wir überprüft Linux-Distributionen.

| Verteilung | Version                 | Unterstützte Kernel    |
|--------------|-------------------------|---------------------|
| Ubuntu       | 12.04                   | 3.2.0-75.110+       |
| Ubuntu       | 14.04                   | 3.13.0-44.73+       |
| Debian       | 7.x, 8.x                | 3.16.7-ckt4-1+      |
| SLES         | SLES 12                 | 3.12.36-38.1+       |
| SLES         | SLES 11 SP4             | 3.0.101-0.63.1+     |
| CoreOS       | 584.0.0+                | 3.18.4+             |
| CentOS       | 6.5, 6.6, 6.7, 7.0, 7.1 | 3.10.0-229.1.2.el7+ |
| RHEL         | 6.8 +, 7.2 +              |                     |


## <a name="file-storage"></a>Dateispeicher

Azure Dateispeicher bietet Dateifreigaben in der Cloud mit SMB-Standardprotokoll. Azure-Dateien können Sie Anwendung migrieren, die auf Dateiservern in Azure. In Azure ausgeführt können problemlos Dateifreigaben von Azure virtuellen Computern Linux bereitstellen. Und mit der neuesten Version der Datei speichern, Sie können auch eine Dateifreigabe von einer lokalen Anwendung, die SMB 3.0 unterstützt.  Da Dateifreigaben SMB-Freigaben sind, können Sie sie über standard-Dateisystem-APIs zugreifen.

Dateispeicher basiert auf derselben Technologie wie Blob, Tabelle und Warteschlange speichern Speicherung bietet Verfügbarkeit, Haltbarkeit, skalierbar und Geo-Redundanz, die in Azure Storage-Plattform integriert. Einzelheiten der Datei Storage Performance-Ziele und Grenzwerte finden Sie unter Azure Storage Skalierbarkeits- und Performance-Ziele.

- [Verwendung von Azure Dateispeicher mit Linux](../storage/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Aktiven Speicher

Azure hot Speicherebene ist optimiert, zum Speichern von Daten, auf die häufig zugegriffen werden.  Hot Speicher ist der Speicher für Blob-Speicher.

## <a name="cool-storage"></a>Coole Speicher

Azure cool Speicherebene wird zum Speichern von Daten, die nur selten zugegriffen und langlebigen optimiert. Anwendungsbeispiele für coole Speicher enthalten Backups, Medieninhalte wissenschaftlichen Daten, Compliance und archivierte Daten. Alle Daten, die selten ist im Allgemeinen ein idealer Kandidat für coole Speicher.

|                             | Hot Speicherebene      | Coole Speicherebene     |
|:----------------------------|:---------------------:|:---------------------:|
| Verfügbarkeit                | 99,9 %                 | 99 %                   |
| Verfügbarkeit (RA-GRS gelesen) | 99,99 %                | 99,9 %                 |
| Nutzungsgebühren               | Höhere Speicherkosten  | Geringere Speicherkosten   |
|                             | Niedrigere Zugriff          | Höhere Zugriff         |
|                             | und Kosten | und Kosten |


## <a name="redundancy"></a>Redundanz

Die Daten in Ihrem Microsoft Azure Storage-Konto ist Haltbarkeit und hohe Verfügbarkeit Azure Storage SLA trotz transiente Hardware-Ausfälle treffen immer repliziert.

Wenn Sie ein Speicherkonto erstellen, müssen Sie eine der folgenden Optionen für die Replikation auswählen:

- Lokal redundanten Speicher (LRS)
- Zone-redundanten Speicher (ZRS)
- Geo-redundanten Speicher (GRS)
- Lesezugriff Geo redundanten Speicher (RA-GRS)

### <a name="locally-redundant-storage"></a>Lokal redundanter Speicher

Lokal redundanter Speicher (LRS) repliziert die Daten innerhalb der Region, in der das Speicherkonto erstellt. Zur Maximierung der Lebensdauer wird jeder Anforderung für Daten in Ihrem Speicherkonto dreimal repliziert. Diese drei Kopien jedes befinden sich in separaten Fehlertoleranz und Upgrade-Domänen.  Erfolgreich beendet eine Anforderung nur einmal alle drei Replikate geschrieben wurde.

### <a name="zone-redundant-storage"></a>Zone-redundanten Speicher

Zone-redundanten Speicher (ZRS) repliziert Daten über zwei Räume innerhalb eines einzelnen Bereichs oder zwei Regionen höhere Haltbarkeit als LRS bereitstellen. Verfügt das Speicherkonto ZRS aktiviert, sind Ihre Daten auch bei an eines der dauerhaften.

### <a name="geo-redundant-storage"></a>Geo-redundanten Speicher

Geo-redundanten Speicher (GRS) repliziert Daten in eine sekundäre Region Hunderte von Meilen von der primären Region. Wenn das Speicherkonto GRS aktiviert ist, sind Ihre Daten dauerhaften auch bei vollständigen regionalen Ausfall oder eine in der primäre Region nicht wiederherstellbar ist.

### <a name="read-access-geo-redundant-storage"></a>Lesezugriff Geo redundante Speicherung

Lesezugriff Geo redundanten Speicher (RA-GRS) maximiert die Verfügbarkeit für das Speicherkonto schreibgeschützten Zugriff auf Daten in sekundären Standort neben Replikation über zwei Bereiche von g bereitgestellt. Die Daten in der primären Region nicht verfügbar ist, kann die Anwendung Daten aus dem sekundären lesen.

Für einen tiefen Einblick in Azure-Speicher Redundanz finden Sie unter:

- [Azure Speicherreplikation](../storage/storage-redundancy.md)

## <a name="scalability"></a>Skalierung

Azure-Speicher ist hochgradig skalierbar, können gespeichert und Hunderte Terabyte Daten die großen Szenarien erforderlich wissenschaftliche, finanzielle Analyse und Mediaprogramme. Oder Sie kleinen Mengen von Daten für small Business-Website. Wo Ihre Bedürfnisse liegen bezahlen Sie nur die Daten, die Sie speichern. Azure-Speicher derzeit Zehntausende Billionen Kundenobjekte speichert und Millionen von Anfragen pro Sekunde durchschnittlich behandelt.

Standardspeicher Konten: standard Storage-Konto hat eine maximale Gesamtbetrag anfordern 20.000 IOPS. Insgesamt IOPS über alle virtuellen Datenträger im standardspeicherkonto darf diese Grenze nicht überschreiten.

Für Premium-Speicherkonten: ein Speicherkonto Premium hat eine maximale Gesamtdurchsatz 50 Gbit/s. Der Gesamtdurchsatz aller Festplatten VM darf diese Grenze nicht überschreiten.

## <a name="availability"></a>Verfügbarkeit

Wir garantieren, dass mindestens 99,99 % (99,9 % Cool Zugriffsschicht) Zeit wir erfolgreich Anfragen Lesezugriff Geo redundanten Speicher (RA-GRS) Konten Daten gelesen wird, fehlgeschlagene Versuche zum Lesen von Daten aus dem primären auf den sekundären Bereich wiederholt werden.

Wir garantieren, dass mindestens 99,9 % (99 % Cool Zugriffsschicht) Zeit, wir erfolgreich Anfragen lokal redundanter Speicher (LRS), Zone redundanten Speicher (ZRS) und Geo redundanten Speicher (GRS) Konten Daten gelesen wird.

Wir garantieren, dass mindestens 99,9 % (99 % Cool Zugriffsschicht) Zeit, wir erfolgreich Anfragen zum Schreiben von Daten lokal redundanter Speicher (LRS), Zone redundanten Speicher (ZRS), Geo redundanten Speicher (GRS) Konten und Konten Lesezugriff Geo redundanten Speicher (RA-GRS) verarbeitet.

- [Azure SLA für Speicher](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)


## <a name="regions"></a>Regionen

Azure ist erhältlich in 30 Regionen der Welt und kündigt Pläne für 4 zusätzliche Regionen. Geografische Aufgliederung ist eine Priorität für Azure ermöglicht unseren Kunden, höhere Leistung und ihre Bedürfnisse und Präferenzen hinsichtlich der Speicherort unterstützt.  Aktuelle Region starten Azure ist in Deutschland.

Microsoft Cloud-Deutschland Möglichkeit eine differenzierte bereits Microsoft Cloud-Dienste in Europa mehr Chancen für Innovation und Wachstum stark reglementierte Partner und Kunden in Deutschland, der Europäischen Union (EU) und der Europäischen Freihandelsassoziation (EFTA) erstellen.

Daten in dieser neuen Rechenzentren in Magdeburg und Frankfurt wird unter der Kontrolle der Treuhänder Daten, T-Systems International, deutsche unabhängig und Tochtergesellschaft der Deutsche Telekom. Microsoft commercial Clouddienste in diesen Rechenzentren deutschen Datenverarbeitung Vorschriften einhalten und bieten Kunden zusätzliche Optionen wie und wo Daten verarbeitet.


- [Azure-Regionen zuordnen](https://azure.microsoft.com/regions/)

## <a name="security"></a>Sicherheit

Azure-Speicher bietet umfassende Sicherheitsfunktionen zusammen ermöglicht Entwicklern, sichere Anwendung zu erstellen. Das Speicherkonto selbst kann rollenbasierte Zugriffskontrolle mit Azure Active Directory gesichert werden. Daten können zwischen einer Anwendung und Azure mithilfe von clientseitigen Encryption, HTTPS oder SMB 3.0 gesichert werden. Daten können mit festgelegt werden automatisch verschlüsselt werden, beim Schreiben in Azure Storage Storage Service Verschlüsselung (SSE). Betriebssystem und Daten von virtuellen Computern verwendete Datenträger lassen Azure Datenträger Verschlüsselung verschlüsselt werden. Delegierter Zugriff auf die Datenobjekte in Azure Storage kann mit Shared Access Signatures erteilt werden.

### <a name="management-plane-security"></a>Management-Ebene Sicherheit

Die Verwaltungsebene umfasst Ressourcen zur Verwaltung Ihres Speicherkontos. In diesem Abschnitt sprechen wir über Azure-Ressourcen-Manager-Bereitstellungsmodell und wie Sie rollenbasierte Zugriffskontrolle (RBAC) Speicherkonten Zugriffskontrolle. Wir werden auch zum Verwalten von Ihrem Konto Speicherschlüssel und wie sie Regenerieren sprechen.

### <a name="data-plane-security"></a>Datenschutz-Ebene

In diesem Abschnitt betrachten wir Zugriff auf die tatsächlichen Datenobjekte in Ihrem Speicherkonto wie Blobs, Dateien, Warteschlangen und Tabellen mit Shared Access Signatures und Zugriffsrichtlinien gespeichert. Service Level SAS und SAS Ebene werden behandelt. Wir sehen auch das Protokoll HTTPS beschränkt Zugriff auf eine bestimmte IP-Adresse (oder der IP-Adressen) zu beschränken und zum einen SAS widerrufen, ohne es abläuft.

## <a name="encryption-in-transit"></a>Verschlüsselung in Transit

Dieser Abschnitt erläutert die Daten zu schützen, wenn Sie in oder aus dem Azure-Speicher übertragen. Wir sprechen über die empfohlene Verwendung von HTTPS und Verschlüsselung von SMB 3.0 für Dateifreigaben Azure verwendet. Wir nehmen auch einen Blick auf die clientseitige Verschlüsselung können Sie die Daten verschlüsseln, bevor sie in einer Clientanwendung in den Speicher übertragen werden und zum Entschlüsseln der Daten nach der Übertragung aus.

## <a name="encryption-at-rest"></a>Verschlüsselung ruhender

Wir sprechen über Storage Service Verschlüsselung (SSE) und wie Sie aktivieren ein Speicherkonto, wodurch die Block-Blobs Seitenblobs und Anhängen Blobs automatisch verschlüsselt werden, wenn in den Azure-Speicher geschrieben. Auch werden wir Azure Datenträger Verschlüsselung und untersuchen die grundlegenden Unterschiede und Anfragen Datenträger Verschlüsselung und SSE und clientseitige Verschlüsselung. FIPS-Konformität für US-Regierung Computer werden kurz behandelt.

- [Azure Storage Security guide](../storage/storage-security-guide.md)

## <a name="cost-savings"></a>Kostenersparnis

- [Speicherkosten](https://azure.microsoft.com/pricing/details/storage/)

- [Speicherkosten Rechner](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Speichergrenzwerte

- [Speichergrenzwerte Service](../azure-subscription-service-limits.md#storage-limits)
