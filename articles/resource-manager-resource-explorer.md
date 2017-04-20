<properties
   pageTitle="Azure-Explorers | Microsoft Azure"
   description="Beschreibt Azure Resource Explorer und wie sie verwendet werden, anzeigen und Aktualisieren von Installationen durch Azure-Ressourcen-Manager"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="stuartleeks"
   manager="ankodu"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/01/2016"
   ms.author="stuartle;tomfitz"/>

# <a name="use-azure-resource-explorer-to-view-and-modify-resources"></a>Verwenden Sie Azure Resource Explorer anzeigen und Bearbeiten von Ressourcen
[Azure-Ressourcen-Explorer](https://resources.azure.com) ist ein hervorragendes Tool für Ressourcen, die Sie bereits in Ihrem Abonnement erstellt betrachten. Mit diesem Tool können Sie verstehen, wie die Ressourcen sind und die Eigenschaften jeder Ressource zugeordnet. Erfahren Sie über die REST-API-Funktionen und für einen Ressourcentyp PowerShell-Cmdlets und Befehle können über die Benutzeroberfläche. Ressourcen-Explorer kann besonders hilfreich sein, wenn Sie Ressourcenmanager Vorlagen erstellen, da die vorhandenen Ressourcen anzeigen können.

Die Quelle für das Ressourcen-Explorer-Tool steht auf [Github](https://github.com/projectkudu/ARMExplorer)bietet eine wertvolle benötigen Sie ein ähnliches Verhalten in der Anwendung implementieren.

## <a name="view-resources"></a>Ressourcen anzeigen
Navigieren Sie zu [https://resources.azure.com](https://resources.azure.com) und melden Sie sich mit denselben Anmeldeinformationen für [Azure-Portal](https://portal.azure.com)verwenden möchten.

Nach dem Laden können in der Strukturansicht auf der linken Seite Drilldown in Ihre Abonnements und Ressourcengruppen:

![TreeView](./media/resource-manager-resource-explorer/are-01-treeview.png)

In einer Ressourcengruppe Bohren erhalten Anbieter Sie für die Ressourcen in dieser Gruppe vorhanden sind:

![Anbieter](./media/resource-manager-resource-explorer/are-02-treeview-providers.png)

Dort können Sie die Ressource Instanzen Bohren beginnen. In der Abbildung unten sehen Sie die `sltest` SQL Server-Instanz in der Strukturansicht. Auf der rechten Seite finden Sie Informationen über die REST API-Anfragen mit dieser Ressource verwenden. Navigieren Sie zum Knoten für eine Ressource, stellt Ressourcen-Explorer automatisch GET-Anforderung zum Abrufen von Informationen über die Ressource. Im großen Bereich unter der URL sehen Sie die Antwort der API. 

Wenn Sie mit Ressourcen-Manager Vorlagen vertraut beginnt der Textinhalt vertraut! Im Abschnitt **Eigenschaften** der Antwort entspricht die Werte im Abschnitt **Eigenschaften** der Vorlage bieten.

![SqlServer](./media/resource-manager-resource-explorer/are-03-sqlserver-with-response.png)

Ressourcen-Explorer können Sie zu untergeordneten Ressourcen bei der SQL-Datenbankserver Drilldown halten, untergeordneten Ressourcen wie Datenbanken und Firewall-Regeln vorhanden sind.

Durchsuchen einer Datenbank zeigt uns die Eigenschaften für die Datenbank. Im Screenshot unten sehen wir, dass die Datenbank `edition` ist `Standard` und `serviceLevelObjective` (oder Datenbank) ist `S1`.

![SQL-Datenbank](./media/resource-manager-resource-explorer/are-04-database-get.png)

## <a name="change-resources"></a>Ressourcen ändern

Sobald Sie einer Ressource gewechselt sind, können Sie die Schaltfläche Inhalt JSON bearbeitbar auswählen. Sie können Ressourcen-Explorer bearbeiten JSON und Anfrage setzen die Ressource ändern. Die Abbildung unten zeigt beispielsweise die Datenbankebene geändert `S0`:

![Datenbank - PUT-Anforderung](./media/resource-manager-resource-explorer/are-05-database-put.png)

**Ausgewählt** wird die Anforderung senden. 

Nachdem der Antrag stellt erneut Ressourcen-Explorer die GET-Anforderung zum Aktualisieren des Status. In diesem Fall sehen wir, dass die `requestedServiceObjectiveId` wurde aktualisiert und unterscheidet sich von der `currentServiceObjectiveId` angibt, die eine Skalierung ausgeführt wird. Klicken Sie auf die Schaltfläche abrufen, um den Status manuell zu aktualisieren.

![Datenbank - request2 abrufen](./media/resource-manager-resource-explorer/are-06-database-get2.png)

## <a name="performing-actions-on-resources"></a>Ausführen von Aktionen für Ressourcen

**Die Registerkarte** können Sie zu zusätzlichen REST Operationen. Bei der Auswahl einer Websiteressource, stellt die Registerkarte Aktionen eine lange Liste der verfügbaren Operationen, die unten angezeigt werden.

![Web - POST-Anforderung](./media/resource-manager-resource-explorer/are-web-post.png)

## <a name="invoking-the-api-via-powershell"></a>Aufrufen der API über PowerShell
PowerShell auf Ressourcen-Explorer zeigt die Cmdlets Interaktion mit der Ressource, die Sie gerade untersuchen. Je nach ausgewählten Ressource reicht von einfachen Cmdlets angezeigte PowerShell-Skript (z. B. `Get-AzureRmResource` und `Set-AzureRmResource`), komplizierter Cmdlets (z. B. auf einer Website austauschen). 

![PowerShell](./media/resource-manager-resource-explorer/are-07-powershell.png)

Informationen auf der Azure-PowerShell-Cmdlets finden Sie unter [Verwendung von Azure PowerShell mit Azure-Ressourcen-Manager](powershell-azure-resource-manager.md)

## <a name="summary"></a>Zusammenfassung
Bei der Arbeit mit Ressourcen-Manager kann der Explorer äußerst nützlich sein. Es ist eine hervorragende Möglichkeit zu PowerShell zum Abfragen und ändern. Wenn Sie die REST-API arbeiten, ist es hervorragend und API-Aufrufe schnell testen, bevor Sie schreiben Code. Und wenn Sie Vorlagen schreiben, sind ein gutes verstehen der Ressourcenhierarchie und wo Konfiguration - Änderung im Portal finden Sie die entsprechenden Einträge im Ressourcen-Explorer!

Sehen Sie für Weitere Informationen [Channel 9 video Scott Hanselman und David Ebbo](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Resource-Manager-Explorer-with-David-Ebbo)


