<properties
    pageTitle="Ressourcengruppen Richtlinien | Microsoft Azure"
    description="Erfahren Sie mehr über wichtigen Entwurf und Implementierung von Richtlinien zur Bereitstellung von Ressourcengruppen in Azure Infrastrukturdienste."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="azure-resource-group-guidelines"></a>Azure Ressource Richtlinien

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Dieser Artikel befasst sich Kenntnisse logisch Ihrer Umgebung und gruppieren alle Komponenten in Ressourcengruppen.


## <a name="implementation-guidelines-for-resource-groups"></a>Richtlinien für die Implementierung von Ressourcengruppen

Entscheidung:

- Werden die Hauptkomponenten der Infrastruktur oder durch Bereitstellung einer vollständigen Anwendung, Ressourcengruppen erstellen?
- Müssen Sie Zugriff auf Ressourcengruppen über rollenbasierte Zugriffskontrolle?

Aufgaben:

- Welche zentralen Infrastrukturkomponenten und Ressourcengruppen Sie müssen spezielle definieren
- Überprüfen Sie zum Ressourcen-Manager Vorlagen für konsistente, reproduzierbare Bereitstellung implementieren.
- Was Sie benötigen für die Steuerung des Zugriffs auf Ressourcen Zugriff Benutzerrollen definieren.
- Erstellen Sie Ressourcengruppen mit der Namenskonvention. Azure-CLI oder Portal können.


## <a name="resource-groups"></a>Ressourcengruppen

In Azure gruppieren Sie logisch Verwandte Ressourcen wie Speicherkonten, virtuelle Netzwerke und virtuelle Maschinen (VMs) bereitstellen, verwalten und als eine Einheit verwalten. Dieser Ansatz erleichtert Applikationen bereitstellen und die zugehörigen Ressourcen zusammen aus Sicht oder andere Zugriff auf diese Gruppe von Ressourcen. Lesen Sie für ein umfassendes Verständnis der Ressourcengruppen [Azure-Ressourcen-Manager (Übersicht)](../azure-resource-manager/resource-group-overview.md).

Eine Schlüsselfunktion Ressourcengruppen ist die Möglichkeit, Ihre Umgebung mithilfe einer JSON-Datei, die den Speichernetzwerken deklariert und compute-Ressourcen. Sie können auch verknüpfte benutzerdefinierte Skripts oder Konfigurationen angewendet. Mithilfe dieser JSON-Vorlagen erstellen Sie konsistente, reproduzierbare Bereitstellung für die Anwendung. Dadurch können Sie eine Umgebung Entwicklung erstellen und dann mithilfe dieser Vorlage ein Produktionsbetrieb erstellen oder umgekehrt. Finden Sie ein besseres Verständnis über die Verwendung von Vorlagen, [die Vorlage Exemplarische Vorgehensweise](../resource-manager-template-walkthrough.md) , die durch die einzelnen Schritte beim Aufbau einer JSON-Vorlage führt.

Es gibt zwei verschiedene Ansätze können Sie beim Entwerfen der Umgebung Ressourcengruppen:

- Ressourcengruppen für jede anwendungsbereitstellung, die Kombination Speicherkonten, virtuelle Netzwerke und Subnets VMs Balancers usw. geladen werden.
- Zentrale Ressourcengruppen, die Ihre Core virtuellen Netzwerke und Subnets oder Storage-Konten enthalten. Die Anträge sind eigene Ressourcengruppen, die nur VMs Lastenausgleich Netzwerkschnittstellen usw. enthalten.

Wie Sie skalieren, zentrale Ressourcengruppen für virtuelle Netzwerk und Subnetze erstellen Erstellen erleichtert Netzwerkanschlüsse standortübergreifende für Hybrid-Konnektivitätsoptionen. Alternativer Ansatz ist für jede Anwendung virtuelle Netzwerk, das Konfiguration und Wartung erforderlich sind. [Rollenbasierte Zugriffskontrolle](../active-directory/role-based-access-control-what-is.md) präzise die Möglichkeit, Zugriff auf Ressourcen zu steuern. Produktions-Anwendung können Sie steuern, die Benutzer, die auf Ressourcen zugreifen können oder für die Core-Infrastruktur-Ressourcen Sie nur Infrastructure-Ingenieure arbeiten können. Der Anwendungsbesitzer haben nur Zugriff auf die Anwendungskomponenten in ihrer Gruppe und nicht der Azure-Infrastruktur der Umgebung. Entwerfen Ihrer Umgebung sollten Sie die Benutzer, die Zugriff auf die Ressourcen und die Ressourcengruppen entsprechend zu gestalten. 


## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 