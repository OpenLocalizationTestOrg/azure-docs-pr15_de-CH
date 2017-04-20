<properties
    pageTitle="Abonnement und Konten Richtlinien | Microsoft Azure"
    description="Erfahren Sie mehr über wichtigen Entwurf und Implementierung von Richtlinien für Konten auf Azure und Abonnements."
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

# <a name="subscription-and-accounts-guidelines"></a>Richtlinien-Abonnement und Konten

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Dieser Artikel befasst sich wie ein Abonnement und Account Management Ihrer Umgebung verstehen und und Benutzer.


## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Implementierungsrichtlinien für Konten und Abonnements

Entscheidung:

- Welche Gruppe Abonnements und Konten Sie Ihre IT-Arbeitslast oder Infrastruktur müssen?
- Zum Zerlegen der Hierarchie Ihrer Organisation anpassen?

Aufgaben:

- Definieren Sie die logische Organisationshierarchie von einem Abonnement verwalten möchten.
- Entsprechend diesem logischen Hierarchie erforderlichen Konten und Abonnements unter jedem Konto definiert.
- Konten mit Ausklinkungen und Abonnements erstellen.


## <a name="subscriptions-and-accounts"></a>Abonnements und Konten

Arbeiten mit Azure benötigen Sie mindestens eine Azure-Abonnements. Wie virtuelle Maschinen (VMs) oder virtuelle Netzwerke in diese Abonnements vorhanden.

- Unternehmen haben normalerweise eine Enterprise-Registrierung, die oberste Ressource in der Hierarchie, und einem oder mehreren Konten zugeordnet.
- Für Privatkunden und ohne ein Enterprise Agreement-Kunden ist die oberste Ressource das Konto.
- Abonnements sind Konten zugeordnet und kann ein oder mehrere Abonnements pro Konto. Azure Datensätze Abrechnungsinformationen auf Abonnementebene.

Aufgrund der zwei Hierarchieebenen Konto-Abonnement Beziehung unbedingt an die Namenskonvention von Konten und Abrechnung zu Abonnements. Beispielsweise wenn ein globales Unternehmen Azure verwendet, sie können ein Konto pro Region besitzen und Abonnements auf verwaltet die Regionsebene:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Beispielsweise können Sie die folgende Struktur:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Entscheidet sich mehr als ein Abonnement für eine bestimmte Gruppe ein Bereich sollte die Namenskonvention können zusätzlichen Daten des Kontos oder der Abonnementname codieren integrieren. Diese Organisation ermöglicht Geschäftsdatensätze Rechnungsdaten neuen Ebenen der Hierarchie während der Abrechnung Berichte generiert:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

Die Organisation könnte folgendermaßen aussehen:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Wir bieten detaillierte Abrechnung über eine herunterladbare Datei für ein einzelnes Konto oder für alle Konten in einem Enterprise Agreement.


## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 