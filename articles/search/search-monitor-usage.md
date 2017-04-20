<properties 
   pageTitle="Statistiken zu Nutzung und in Azure-Suchdienst überwachen | Microsoft Azure | Gehostete Cloud-Suchdienst" 
   description="Spurgröße Ressource Verbrauch und Index Azure, für eine gehostete Cloud-Suchdienst auf Microsoft Azure." 
   services="search" 
   documentationCenter="" 
   authors="HeidiSteen" 
   manager="jhubbard" 
   editor=""
   tags="azure-portal"/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required" 
   ms.date="05/17/2016"
   ms.author="heidist"/>

# <a name="monitor-usage-and-statistics-in-an-azure-search-service"></a>Überwachung der Nutzung und Statistiken in Azure-Suchdienst

Das Wachstum von Indizes und Dokumentgröße verfolgen können Sie proaktiv anpassen Kapazität vor der Obergrenze für den Dienst eingerichtet haben. 

Zum Überwachen der Ressourcenverwendung zählt und Statistiken für Ihren Dienst in [Azure-Portal](https://portal.azure.com)leicht sind, aber Sie erhalten auch Informationen programmgesteuert erstellen ein Verwaltungstool Kundenservice. Dieser Artikel beschreibt die Schritte für beide Verfahren.

Analyse die neuen Suchfunktion-Datenverkehr verwenden Sie Einblicke in bei der Aktivität. Besuchen Sie [Suche Datenverkehr Analytics Azure Suche](search-traffic-analytics.md) zu beginnen.

##<a name="view-counts-and-metrics-in-the-portal"></a>Anzahl und Metriken im Portal anzeigen 

1. [Azure-Portal](https://portal.azure.com)anmelden. 

2. Öffnen Sie das Dashboard Service von Azure Suchdienst. Kacheln für den Dienst finden Sie auf der Homepage können, oder Sie für den Dienst aus durchsuchen auf der Indexleiste. Eine schrittweise Anleitung finden Sie unter [Erstellen eines Dienstes](search-create-service-portal.md) .

Im Abschnitt Verwendung enthält eine Anzeige, das angibt, welcher Teil der verfügbaren Ressourcen sind zurzeit in Verwendung.

  ![][1]

Daran erinnern, daß der gemeinsame Dienst maximal ein Replikat und jede partition. Darüber hinaus unterstützt es nur 10.000 Dokumente insgesamt oder 50 MB, ausstellt.

##<a name="get-index-statistics-using-the-rest-api"></a>Statistiken Sie Index über die REST-API

Die REST-API von Azure Suche und .NET SDK ermöglichen den programmgesteuerten Zugriff auf Dienstmetrik.  Verwenden Sie [Indexer](https://msdn.microsoft.com/library/azure/dn946891.aspx) einen Index von Azure SQL-Datenbank oder DocumentDB, eine zusätzliche API laden steht zu Zahlen, die Sie benötigen. 

  + [Index-Statistiken](https://msdn.microsoft.com/library/azure/dn798942.aspx)
  + [Anzahl Dokumente](https://msdn.microsoft.com/library/azure/dn798924.aspx)
  + [Indexer-Status abrufen](https://msdn.microsoft.com/library/azure/dn946884.aspx)

## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie [Grenzen und Kapazität](search-limits-quotas-capacity.md) aus Partitionen und Replikate, die Sie benötigen, wenn vorhandener Kapazität nicht ausreicht. 

Weitere Informationen zum Verwalten von Diensten finden Sie auf [der Suchdienst auf Microsoft Azure verwalten](search-manage.md) .

<!--Image references-->
[1]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG




 
