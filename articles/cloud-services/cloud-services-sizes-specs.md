<properties
 pageTitle="Größen für Cloud-Services | Microsoft Azure"
 description="Zeigt die Größe der anderen virtuellen Computer (und IDs) für Azure Cloud Service Web- und Workerrollen Rollen."
 services="cloud-services"
 documentationCenter=""
 authors="Thraka"
 manager="timlt"
 editor=""/>
<tags
 ms.service="cloud-services"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="10/27/2016"
 ms.author="adegeo"/>

# <a name="sizes-for-cloud-services"></a>Größen für Cloud-Services

Dieses Thema beschreibt die Größen und Optionen für Cloud-Dienst Instanzen (Webrollen und Worker-Rollen). Darüber hinaus Bereitstellungsaspekte Planung dieser Ressourcen zu. Jede hat eine ID, die Sie in die [Datei](cloud-services-model-and-package.md#csdef)einfügen.

Cloud-Dienste ist einer von mehreren Typen von Azure computeressourcen. Klicken Sie [hier](cloud-services-choose-me.md) für Weitere Informationen zum Cloud-Dienste.

> [AZURE.NOTE]Verwandte Azure Grenzen finden Sie unter [Azure-Abonnement und Service Grenzen, Kontingente und Integritätsregeln](../azure-subscription-service-limits.md)

## <a name="sizes-for-web-and-worker-role-instances"></a>Größen für Web- und Workerrollen-Instanzen

Es gibt mehrere Standardgrößen auf Azure. Einige Größen Aspekte umfassen:

* D-Serie VMs sollen Programme ausführen, die höhere rechenleistung temporärer Leistung und. D-Serie VMs bieten schnellere Prozessoren, ein höheres Verhältnis Speicher Core und Solid State Drive (SSD) für die temporäre Diskette. Details finden Sie in der Ankündigung Azure Blog [Neuen D-Serie Virtual Machine Größen](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

* Dv2-Serie, ein Nachfolger der ursprünglichen D-Serie bietet eine leistungsfähigere CPU. Dv2-Serie CPU ist etwa 35 % schneller als die CPU D-Serie. Basiert auf der neuesten Generation 2,4 GHz Intel Xeon® E5 2673 v3 (Haswell) Prozessor und Intel Turbo Boost Technologie 2.0 3.1 GHz. Dv2-Serie verfügt über die gleichen Arbeitsspeicher- und Konfigurationen als der D-Serie.

*   G-Serie VMs bieten die meisten Speicher und auf Hosts, die Familie Prozessoren Intel Xeon E5 V3.

*   Eine Reihe virtueller Computer können auf einer Vielzahl von Hardware- und bereitgestellt werden. Die Größe beschränkt basierend auf Hardware bieten konsistente prozessorleistung für ausgeführte Instanz unabhängig von der Hardware auf bereitgestellt wird. Ermitteln die physische Hardware auf der Größe bereitgestellt wird, Fragen Sie ab, die virtuelle Hardware des virtuellen Computers.

*   A0 ist auf der physischen Hardware überzeichnet. Für dieses bestimmte Größe können anderen kundenbereitstellungen von die Leistung der ausgeführten Arbeit auswirken. Die relative Leistung ist als erwartet Baseline unter eine ungefähre Variabilität von 15 Prozent beschrieben.


Die Größe des virtuellen Computers wirkt sich auf die Preise. Die Größe wirkt sich auch auf die Verarbeitung, Speicher und Speicherkapazität des virtuellen Computers. Speicherkosten werden separat auf das Speicherkonto verwendete Seiten berechnet. Details finden Sie unter [Preisdetails virtuellen Maschinen](https://azure.microsoft.com/pricing/details/virtual-machines/) und [Azure Storage Preise](https://azure.microsoft.com/pricing/details/storage/). 


Folgendes können Sie entscheiden, eine Größe:


* Größe A8 A11 und H-Serie sind auch *rechenintensive Instanzen*. Die Hardware, die diesen Größen wird entworfen und optimiert für rechenintensive und Netzwerk-Intensive Anwendungsbereiche, einschließlich High-Performance computing (HPC) cluster Applikationen, Modellierung und Simulationen. Die A8 A11 verwendet Intel Xeon E5 2670 @ 2,6 GHZ und H-Serie verwendet Intel Xeon E5 2667 v3 @ 3,2 GHz. Ausführliche Informationen und Hinweise zur Verwendung Siehe dieser Größe [VMs eine Reihe H-Serie und rechenintensiver](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md). 

* Dv2-Serie D G-Serie sind ideal für die schnellere CPUs verlangen, bessere lokale Festplatten-Performance oder höheren Speicherbedarf haben.  Sie bieten eine leistungsstarke Kombination für viele Unternehmen Applikationen.

*   Einige der physischen Hosts in Azure Rechenzentren unterstützen nicht virtuellen größere wie A5 – A11. Daher sehen die Fehlermeldung **Fehler beim virtuellen Computer {Machine} konfigurieren** oder **Fehler virtuellen Computer {Machine} erstellen** Sie beim Ändern der Größe von vorhandenen virtuellen Computers eine neue Größe; Erstellen einen neuen virtuellen Computer in einem virtuellen Netzwerk vor dem 16. April 2013 erstellt; oder einen vorhandenen Cloud-Dienst einen neuen virtuellen Computer hinzufügen. Siehe [Fehler: "Fehler beim virtuellen Computer konfigurieren"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) im Support-Forum Abhilfen für jedes Bereitstellungsszenario.  

* Ihr Abonnement kann auch die Anzahl der Kerne beschränken bestimmte Größe Familien bereitgestellten. Um das Kontingent zu erhöhen, Support von Azure.


## <a name="performance-considerations"></a>Leistung

Wir haben das Konzept von der Azure Compute Einheit (ACU) zu berechnen (CPU) Leistung in Azure SKUs verglichen. Dadurch leicht identifizieren die SKU am ehesten Ihren Bedürfnissen Leistung.  ACU derzeit auf einem kleinen (Standard_A1) standardisiert wird dann 100 und alle SKUs VM darstellen etwa wie viel schneller, SKU standard Benchmark ausführen kann. 

>[AZURE.IMPORTANT] Das ACU ist nur eine Richtlinie.  Die Ergebnisse für Ihre Arbeitslast variieren. 

<br>

|SKU-Familie |ACU-Kern |
|---|---|
|[Standard_A0](#a-series)   |50 |
|[Standard_A1 4](#a-series) |100 |
|[Standard_A5 7](#a-series) |100 |
|[A8 A11](#a-series)    |225 *|
|[D1-14](#d-series) |160 |
|[D1 15v2](#dv2-series) |210 - 250 *|
|[G1 5](#g-series)  |180 - 240 *|
|[H](#h-series) |290 - 300 *|

ACUs markiert mit einem * Intel® Turbo-Technologie mit der CPU-Frequenz und eine Leistungssteigerung.  Der Umfang der Verstärkung der abhängig VM-Größe, Arbeitslast und andere auf demselben Server ausgeführten Arbeitslasten.

## <a name="size-tables"></a>Größentabellen

Die folgenden Tabellen zeigen die und die Kapazität, die sie bereitstellen.

* Speicherkapazität in GiB oder 1024 gezeigt ^ 3 Bytes. Beim Vergleichen von Disketten gemessen in GB (1000 ^ 3 Bytes), gemessen in GiB Festplatten (1024 ^ 3) daran Kapazität Zahlen in GiB kleiner erscheinen. Zum Beispiel GiB 1023 = 1098.4 GB

* Datenträgerdurchsatz in e/a-Operationen pro Sekunde (IOPS) gemessen und Mbit/s, Mbit/s = 10 ^ 6 Bytes pro Sekunde.

* Datenträger können im Cache oder nicht-Cache-Modus betreiben. Host-Cache-Modus wird für zwischengespeicherte Daten Datenträgervorgang **ReadOnly** oder **ReadWrite**festgelegt.  Host-Cache-Modus wird für nicht zwischengespeicherte Daten Datenträgervorgang auf **None**festgelegt.

* Maximale Netzwerkbandbreite ist die maximale aggregierte Bandbreite zugeteilt und pro VM zugewiesen. Die maximale Bandbreite bietet Hinweise zur Auswahl des richtigen VM darauf ausreichend Kapazität verfügbar ist. Beim Wechsel zwischen niedrig, Mittel, hoch und sehr hoch wird entsprechend den Durchsatz erhöhen. Tatsächliche Leistung hängt von vielen Faktoren, Netzwerk und Anwendung geladen und Netzwerkeinstellungen.


## <a name="a-series"></a>Eine Reihe

| Größe        | CPU-Kerne | Speicher: GiB | Lokale Festplatte: GiB | Max-Datenträger | Maximale Datendurchsatz Datenträger: IOPS | Max. NICs / Netzwerkbandbreite |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A0 | 1         | 0.768        | 20                    | 1              | 1 x 500              | 1 / niedrig                   |
| Standard_A1 | 1         | 1,75         | 70                    | 2              | 2 x 500              | 1 / Mittel              |
| Standard_A2 | 2         | 3,5 GB       | 135                   | 4              | 4 x 500              | 1 / Mittel              |
| Standard_A3 | 4         | 7            | 285                   | 8              | 8 x 500              | 2 / hoch                  |
| Standard_A4 | 8         | 14           | 605                   | 16             | 16 x 500             | 4 / hoch                  |
| Standard_A5 | 2         | 14           | 135                   | 4              | 4 X 500              | 1 / Mittel              |
| Standard_A6 | 4         | 28           | 285                   | 8              | 8 x 500              | 2 / hoch                  |
| Standard_A7 | 8         | 56           | 605                   | 16             | 16 x 500             | 4 / hoch                  |

## <a name="a-series---compute-intensive-instances"></a>A-Serie rechenintensive Instanzen

Informationen und Hinweise zur Verwendung Siehe dieser Größe [VMs eine Reihe H-Serie und rechenintensiver](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).


| Größe         | CPU-Kerne | Speicher: GiB | Lokale Festplatte: GiB | Max-Datenträger | Maximale Datendurchsatz Datenträger: IOPS | Max. NICs / Netzwerkbandbreite |
|--------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A8 * | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / hoch                  |
| Standard_A9 * | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / sehr hoch             |
| Standard_A10 | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / hoch                  |
| Standard_A11 | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / sehr hoch             |

* RDMA kann

## <a name="d-series"></a>D-Serie


| Größe         | CPU-Kerne | Speicher: GiB | Lokale SSD: GiB | Max-Datenträger | Maximale Datendurchsatz Datenträger: IOPS | Max. NICs / Netzwerkbandbreite |
|--------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / Mittel              |
| Standard_D2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / hoch                  |
| Standard_D3  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / hoch                  |
| Standard_D4  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / hoch                  |
| Standard_D11 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / hoch                  |
| Standard_D12 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / hoch                  |
| Standard_D13 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / hoch                  |
| Standard_D14 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / sehr hoch             |

## <a name="dv2-series"></a>Dv2-Serie

| Größe            | CPU-Kerne | Speicher: GiB | Lokale SSD: GiB | Max-Datenträger | Maximale Datendurchsatz Datenträger: IOPS | Max. NICs / Netzwerkbandbreite |
|-----------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1_v2  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / Mittel              |
| Standard_D2_v2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / hoch                  |
| Standard_D3_v2  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / hoch                  |
| Standard_D4_v2  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / hoch                  |
| Standard_D5_v2  | 16        | 56           | 800                  | 32             | 32 x 500             | 8 / hohe        |
| Standard_D11_v2 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / hoch                  |
| Standard_D12_v2 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / hoch                  |
| Standard_D13_v2 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / hoch                  |
| Standard_D14_v2 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / hohe        |
| Standard_D15_v2 | 20        | 140          | 1.000                | 40             | 40 x 500             | 8 / hohe        |

## <a name="g-series"></a>G-Serie

| Größe        | CPU-Kerne | Speicher: GiB  | Lokale SSD: GiB  | Max-Datenträger | Max. Datenträgerdurchsatz: IOPS | Max. NICs / Netzwerkbandbreite |
|-------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_G1 | 2         | 28           | 384                  | 4              | 4 x 500            | 1 / hoch                  |
| Standard_G2 | 4         | 56           | 768                  | 8              | 8 x 500            | 2 / hoch                  |
| Standard_G3 | 8         | 112          | 1.536                | 16             | 16 x 500           | 4 / sehr hoch             |
| Standard_G4 | 16        | 224          | 3.072                | 32             | 32 x 500           | 8 / hohe        |
| Standard_G5 | 32        | 448          | 6,144                | 64             | 64 x 500           | 8 / hohe        |


## <a name="h-series"></a>H-Serie

Azure H-Serie virtuellen Computer sind die hohe Leistung der nächsten Generation computing rechnerische muss high-End molekulare Modellierung und computational Fluid Dynamics VMs angestrebt. Diese 8 und 16 Core VMs basieren auf Intel Haswell E5 2667 V3 Serviceprozessor-Technologie mit DDR4 und lokale SSD basiert. 

Neben wesentlichen CPU-Leistung bietet H-Serie unterschiedliche Optionen für niedrige Latenz RDMA Netzwerke mit FDR InfiniBand mehrere Speicherkonfigurationen intensive rechnerische Arbeitsspeicher unterstützt.


| Größe           | CPU-Kerne | Speicher: GiB | Lokale SSD: GiB | Max-Datenträger | Max. Datenträgerdurchsatz: IOPS | Max. NICs / Netzwerkbandbreite |
|----------------|-----------|-------------|--------------------------|----------------|---------------------------|------------------------------|
| Standard_H8    | 8         | 56          | 1000                     | 16             | 16 x 500                    | 8 / hoch                      |
| Standard_H16   | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / sehr hoch                  |
| Standard_H8m   | 8         | 112         | 1000                     | 16             | 16 x 500                    | 8 / hoch                      |
| Standard_H16m  | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / sehr hoch                 |
| Standard_H16r * | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / sehr hoch                  |
| Standard_H16mr * | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / sehr hoch                  |


* RDMA kann

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Hinweis: Standard A0 - A4 CLI mit PowerShell 

Im klassischen Bereitstellungsmodell sind einige Namen VM Größe geringfügig CLI und PowerShell:

* Standard_A0 ist ExtraSmall 
* Standard_A1 ist
* Standard_A2 ist
* Standard_A3 ist groß
* Standard_A4 ist ExtraLarge

## <a name="configure-sizes-for-cloud-services"></a>Größen für Cloud-Dienste konfigurieren

Sie können die Größe des virtuellen Computers eine Rolleninstanz als Teil der [Datei](cloud-services-model-and-package.md#csdef)beschriebenen Service-Modell. Die Größe der Rolle bestimmt die Anzahl der CPU-Kerne, Speicherkapazität und Größe der lokalen Datei, die einer ausgeführten Instanz zugeordnet ist. Wählen Sie die Rolle basierend auf der Anwendung Ressourcen erforderlich.

Hier ist ein Beispiel für die Rolle Größe eine Webrolle Instanz zu [Standard_D2](#general-purpose-d) :

```xml
<WorkerRole name="Worker1" vmsize="<mark>Standard_D2</mark>">
...
</WorkerRole>
```

## <a name="next-steps"></a>Nächste Schritte

- Informationen Sie zu [Azure-Abonnement und Service Grenzen, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).
- Erfahren Sie mehr [über VMs eine Reihe H-Serie und rechenintensiver](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) für Arbeitslasten wie High Performance Computing (HPC).

