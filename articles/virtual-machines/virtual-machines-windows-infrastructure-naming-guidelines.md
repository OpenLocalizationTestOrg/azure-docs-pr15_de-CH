<properties
    pageTitle="Benennungsrichtlinien Infrastruktur | Microsoft Azure"
    description="Erfahren Sie mehr über wichtigen Entwurf und Implementierung von Richtlinien für die Benennung von Azure-Infrastruktur-Services."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="infrastructure-naming-guidelines"></a>Benennungsrichtlinien Infrastruktur

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Dieser Artikel konzentriert sich auf wie Namenskonventionen für alle verschiedenen Azure Ressourcen logische und leicht verständliche Ressourcen in Ihrer Umgebung erstellen.

## <a name="implementation-guidelines-for-naming-conventions"></a>Von Implementierungsrichtlinien zu Namenskonventionen

Entscheidung:

- Was sind die Namenskonventionen für Azure-Ressourcen?

Aufgaben:

- Definieren Sie Affixtyp über Ihre Ressourcen mit der Konsistenz.
- Speicher-Kontonamen Voraussetzung eindeutig zu definieren.
- Dokumentieren der Namenskonvention und Verteilen an alle Beteiligten Bereitstellung Konsistenz sicherzustellen.

## <a name="naming-conventions"></a>Namenskonventionen

Eine gute Namenskonvention müssen an vor dem Erstellen alles in Azure. Eine Benennungskonvention wird sichergestellt, dass alle Ressourcen einen vorhersehbaren Namen der senkt den Verwaltungsaufwand für das Management dieser Ressourcen.

Sie können bestimmte Benennungskonventionen für die gesamte Organisation oder für einen bestimmten Azure-Abonnement oder Konto folgen. Obwohl Personen innerhalb von Organisationen impliziten Regeln Wenn Azure Ressourcen, wenn ein Team ein Projekt auf Azure muss einfach ist, ist dieses Modell nicht gut skalieren.

Eine Gruppe von Namenskonventionen vorne vereinbaren. Es gibt einige Aspekte hinsichtlich Benennungskonventionen, die über Ausschneiden, die festlegt, Regeln.

## <a name="affixes"></a>Affixtyp

Wie Sie eine Benennungskonvention definieren, ist eine Entscheidung, ob das Affix an:

- Namen (Präfix)
- Am Ende des Namens (Suffix)

Hier sind beispielsweise zwei mögliche Namen für eine Ressourcengruppe mit dem `rg` anzubringen:

- RG-WebApp (Präfix)
- WebApp-Rg (Suffix)

Affixe finden verschiedene Aspekte, die bestimmten Ressourcen beschreiben. Die folgende Tabelle zeigt einige Beispiele normalerweise verwendet.

| Seitenverhältnis                               | Beispiele                                                               | Notizen                                                                                                      |
|:-------------------------------------|:-----------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------|
| Umgebung                          | Dev Stg, Produkte                                                         | Je nach Zweck und Name der jeweiligen Umgebung.                                                     |
| Speicherort                             | Usw (West US) verwenden (USA Osten 2)                                         | Je nach Region Datencenter oder die Region der Organisation.                               |
| Azure Komponenten, Diensten oder Produkten | RG für Ressourcengruppe, VNet für virtuelle Netzwerk                        | Je nach Produkt, für das die Ressource unterstützt.                                          |
| Rolle                                 | SQL Ora sp, iis                                                      | Je nach der Rolle des virtuellen Computers.                                                              |
| Instanz                             | 01, 02, 03 usw..                                                       | Für Ressourcen, die mehrere Instanzen haben. Beispielsweise mit Lastenausgleich Webserver in einem Clouddienst. |


Bei Ihren Namenskonventionen Achten sie deutlich die Affixe für jede Ressource und in welcher Position (Präfix Vs Suffix) verwenden.

## <a name="dates"></a>Datum

Oft ist es wichtig Erstellungsdatum vom Namen einer Ressource bestimmen. Es wird empfohlen, das Datumsformat jjjjMMtt. Dieses Format wird sichergestellt, dass nicht nur das vollständige Datum erfasst, sondern auch zwei Ressourcen, deren Namen erst zu unterscheiden, ist gleichzeitig alphabetisch und chronologisch sortiert.

## <a name="naming-resources"></a>Benennung von Ressourcen

Definieren jeder Ressource in die Benennungskonvention müssen die Regeln, die definieren, wie jede Ressource benennen, die erstellt wird. Diese Vorschriften sollten für alle Ressourcen gelten:

- Abonnements
- Konten
- Speicherkonten
- Virtuelle Netzwerke
- Subnetze
- Verfügbarkeit-sets
- Ressourcengruppen
- Virtuelle Computer
- Endpunkte
- Netzwerk-Sicherheitsgruppen
- Rollen

Sicherstellen, dass der Name enthält genügend Informationen, um festzustellen, welche Ressource verweist, beschreibende Namen verwenden soll.

## <a name="computer-names"></a>Computernamen

Beim Erstellen einer virtuellen Maschine (VM) erfordert Microsoft Azure VM ein von bis zu 15 Zeichen für den Ressourcennamen verwendet. Azure verwendet den gleichen Namen für den virtuellen Computer installierte Betriebssystem. Aber diese Namen immer dasselbe möglicherweise nicht.

Bei eine VM von VHD-Abbild-Datei, der bereits ein Betriebssystem enthält erstellt wird, kann die VM Betriebssystem Computername der Name in Azure abweichen. Dies kann einen Schwierigkeitsgrad VM-Verwaltung hinzufügen daher nicht empfohlen. Zuweisen der Azure-VM-Ressource den gleichen Namen wie der Computername, die das Betriebssystem die VM zugewiesen.

Wir empfehlen Azure VM-Name den Namen des zugrunde liegenden Betriebssystems Computer identisch ist.

## <a name="storage-account-names"></a>Namen von Dienstkonten

Speicherkonten haben spezielle Regeln für ihre Namen. Sie können nur Kleinbuchstaben und Zahlen. Weitere Informationen finden Sie unter [erstellen ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) . Außerdem sollte der speicherkontoname sowie von Core.Windows.NET, befinden weltweit gültigen und eindeutigen DNS-Namen. Beispielsweise wenn das Speicherkonto Mystorageaccount aufgerufen wird, sollte die folgenden resultierenden DNS-Namen eindeutig:

- mystorageaccount.BLOB.Core.Windows.NET
- mystorageaccount.Table.Core.Windows.NET
- mystorageaccount.Queue.Core.Windows.NET


## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 