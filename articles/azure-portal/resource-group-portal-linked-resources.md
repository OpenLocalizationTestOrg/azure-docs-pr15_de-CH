<properties 
    pageTitle="In der Galerie Kachel verknüpften und verknüpften Ressourcen" 
    description="Erfahren Sie mehr über verwandte und verknüpften Ressourcen, die in der Kachel Azure vorschauportal angezeigt werden." 
    services="azure-portal" 
    documentationCenter="" 
    authors="adamabdelhamed" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-portal" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/16/2015" 
    ms.author="adamab"/>

# <a name="related-and-linked-resources-in-the-tile-gallery"></a>In der Galerie Kachel verknüpften und verknüpften Ressourcen

Kachel-Gallery können Sie Kacheln für eine bestimmte Ressource und aktuelle Blatt ziehen. Kachel-Gallery können Sie Management-Ansichten erstellen, die Ressourcen umfassen. Für eine angegebene Ressource enthalten die zugehörigen Ressourcen alle Ressourcen, die die gleichen Ressourcengruppe als Ressource freigeben und alle Ressourcen auf oder aus der Ressource.

## <a name="linked-resources-in-azure-resource-manager"></a>Verknüpfte Ressourcen in Azure-Ressourcen-Manager

Eine Verknüpfung ist ein Feature der Azure-Managers.  Sie können die Beziehung zwischen Ressourcen deklarieren, auch wenn sie nicht in derselben Ressourcengruppe befinden. Verknüpfung wirkt sich nicht auf die Laufzeit Ressourcen keine Auswirkung auf die Abrechnung und keine Beeinträchtigung rollenbasierten Zugriff aus.  Es ist einfach ein Mechanismus, mit Beziehung darstellen, sodass Tools wie die Kachel Galerie eine umfassende Verwaltung kann.  Die Tools können über die Links API Links prüfen und benutzerdefinierte Anwendungsbereich sowie Erfahrungen. 

## <a name="how-do-i-link-my-resources"></a>Wie verknüpfe ich meine Ressourcen?

Beim Erstellen von Ressourcen über das Portal oder durch Bereitstellen einer Vorlage Azure PowerShell oder Azure CLI werden Links für einige Ressourcen automatisch erstellt. Sie können Ressourcen auch programmgesteuert mithilfe der [Verknüpften Ressourcen REST API](https://msdn.microsoft.com/library/azure/mt238499.aspx) oder durch Deklarieren der Beziehung in der Vorlage verknüpfen. Eine vollständige Erläuterung der Arbeit mit verknüpften Ressourcen finden Sie unter [Verknüpfen Ressourcen in Azure-Ressourcen-Manager](../resource-group-link-resources.md).

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung in Azure Resource Manager Vorlagen schreiben, finden Sie unter [Erstellung Vorlagen](../resource-group-authoring-templates.md).
- Tauchen näher zum Erstellen von Links zwischen Ressourcen [Verknüpfen Ressourcen in Azure-Ressourcen-Manager](../resource-group-link-resources.md)angezeigt.
- Mehr zum Arbeiten mit Ressourcengruppen über das vorschauportal finden Sie unter [Using Azure Vorschauportal Azure Ressourcen verwalten](resource-group-portal.md).
